Unsere token Cache sollte funktionieren in einem einfachen Fall aber, was passiert, wenn das Token abläuft oder gesperrt wird? Das Token konnte abläuft, wenn die app nicht ausgeführt wird. Dies bedeutet, dass der token Cache ungültig ist. Das Token konnte auch abläuft, während die app tatsächlich ausgeführt wird. Das Ergebnis ist ein HTTP-Status Fehlercode 401 "nicht autorisiert". 

Müssen wir in der Lage, eine abgelaufene Token erkennen und zu aktualisieren. Dazu verwenden wir eine [ServiceFilter](http://dl.windowsazure.com/androiddocs/com/microsoft/windowsazure/mobileservices/ServiceFilter.html) aus der [Clientbibliothek Android](http://dl.windowsazure.com/androiddocs/).

In diesem Abschnitt definieren Sie eine ServiceFilter, die eine HTTP-Status Code 401-Antwort erkennen und eine Aktualisierung von dem Token und token Cache auslösen. Darüber hinaus werden diese ServiceFilter andere ausgehenden Anfragen während der Authentifizierung sperren, sodass diese Anfragen das aktualisierte Token verwenden können.

1. Öffnen Sie die Datei ToDoActivity.java, und fügen Sie die folgenden Aussagen importieren:
 
        import java.util.concurrent.atomic.AtomicBoolean;
        import java.util.concurrent.ExecutionException;

        import com.microsoft.windowsazure.mobileservices.MobileServiceException;
 
2. Fügen Sie die folgenden Mitgliedern der `ToDoActivity` Class. 

        public boolean bAuthenticating = false;
        public final Object mAuthenticationLock = new Object();

    Diese werden verwendet werden, um Hilfe bei der Authentifizierung des Benutzers zu synchronisieren. Es sollen nur einmal authentifizieren. Alle Anrufe bei einer Authentifizierung sollte warten, und verwenden Sie das neue Token aus der Authentifizierung wird ausgeführt.

3. Fügen Sie in der Datei ToDoActivity.java die folgende Methode zur Klasse ToDoActivity, die zu blockierenden ausgehende Anrufe in anderen Threads, während der Authentifizierung verwendet werden.

        /**
         * Detects if authentication is in progress and waits for it to complete. 
         * Returns true if authentication was detected as in progress. False otherwise.
         */
        public boolean detectAndWaitForAuthentication()
        {
            boolean detected = false;
            synchronized(mAuthenticationLock)
            {
                do
                {
                    if (bAuthenticating == true)
                        detected = true;
                    try
                    {
                        mAuthenticationLock.wait(1000);
                    }
                    catch(InterruptedException e)
                    {}
                }
                while(bAuthenticating == true);
            }
            if (bAuthenticating == true)
                return true;
            
            return detected;
        }
        

4. Klicken Sie in der Datei ToDoActivity.java fügen Sie die folgende Methode zur Klasse ToDoActivity aus. Diese Methode löst warten und aktualisieren Sie das Token auf ausgehende Anfragen nach Abschluss der Authentifizierung. 

        
        /**
         * Waits for authentication to complete then adds or updates the token 
         * in the X-ZUMO-AUTH request header.
         * 
         * @param request
         *            The request that receives the updated token.
         */
        private void waitAndUpdateRequestToken(ServiceFilterRequest request)
        {
            MobileServiceUser user = null;
            if (detectAndWaitForAuthentication())
            {
                user = mClient.getCurrentUser();
                if (user != null)
                {
                    request.removeHeader("X-ZUMO-AUTH");
                    request.addHeader("X-ZUMO-AUTH", user.getAuthenticationToken());
                }
            }
        }


5. Klicken Sie in der Datei ToDoActivity.java Aktualisieren der `authenticate` Methode zum der ToDoActivity Klasse, sodass es einen booleschen Parameter zulässig erzwingen, dass die Aktualisierung des Caches token und token akzeptiert. Wir müssen alle blockierten Threads benachrichtigt werden, wenn Authentifizierung abgeschlossen ist, sodass sie von der neuen Token auswählen können.

        /**
         * Authenticates with the desired login provider. Also caches the token. 
         * 
         * If a local token cache is detected, the token cache is used instead of an actual 
         * login unless bRefresh is set to true forcing a refresh.
         * 
         * @param bRefreshCache
         *            Indicates whether to force a token refresh. 
         */
        private void authenticate(boolean bRefreshCache) {
            
            bAuthenticating = true;
            
            if (bRefreshCache || !loadUserTokenCache(mClient))
            {
                // New login using the provider and update the token cache.
                mClient.login(MobileServiceAuthenticationProvider.MicrosoftAccount,
                        new UserAuthenticationCallback() {
                            @Override
                            public void onCompleted(MobileServiceUser user,
                                    Exception exception, ServiceFilterResponse response) {
    
                                synchronized(mAuthenticationLock)
                                {
                                    if (exception == null) {
                                        cacheUserToken(mClient.getCurrentUser());
                                        createTable();
                                    } else {
                                        createAndShowDialog(exception.getMessage(), "Login Error");
                                    }
                                    bAuthenticating = false;
                                    mAuthenticationLock.notifyAll();
                                }
                            }
                        });
            }
            else
            {
                // Other threads may be blocked waiting to be notified when 
                // authentication is complete.
                synchronized(mAuthenticationLock)
                {
                    bAuthenticating = false;
                    mAuthenticationLock.notifyAll();
                }
                createTable();
            }
        }   



6. Fügen Sie diesen Code in der Datei ToDoActivity.java zur Angabe eines neuen `RefreshTokenCacheFilter` Klasse in der Klasse ToDoActivity:

        /**
        * The RefreshTokenCacheFilter class filters responses for HTTP status code 401. 
         * When 401 is encountered, the filter calls the authenticate method on the 
         * UI thread. Out going requests and retries are blocked during authentication. 
         * Once authentication is complete, the token cache is updated and 
         * any blocked request will receive the X-ZUMO-AUTH header added or updated to 
         * that request.   
         */
        private class RefreshTokenCacheFilter implements ServiceFilter {
         
            AtomicBoolean mAtomicAuthenticatingFlag = new AtomicBoolean();                     
            
            @Override
            public ListenableFuture<ServiceFilterResponse> handleRequest(
                    final ServiceFilterRequest request, 
                    final NextServiceFilterCallback nextServiceFilterCallback
                    )
            {
                // In this example, if authentication is already in progress we block the request
                // until authentication is complete to avoid unnecessary authentications as 
                // a result of HTTP status code 401. 
                // If authentication was detected, add the token to the request.
                waitAndUpdateRequestToken(request);
     
                // Send the request down the filter chain
                // retrying up to 5 times on 401 response codes.
                ListenableFuture<ServiceFilterResponse> future = null;
                ServiceFilterResponse response = null;
                int responseCode = 401;
                for (int i = 0; (i < 5 ) && (responseCode == 401); i++)
                {
                    future = nextServiceFilterCallback.onNext(request);
                    try {
                        response = future.get();
                        responseCode = response.getStatus().getStatusCode();
                    } catch (InterruptedException e) {
                       e.printStackTrace();
                    } catch (ExecutionException e) {
                        if (e.getCause().getClass() == MobileServiceException.class)
                        {
                            MobileServiceException mEx = (MobileServiceException) e.getCause();
                            responseCode = mEx.getResponse().getStatus().getStatusCode();
                            if (responseCode == 401)
                            {
                                // Two simultaneous requests from independent threads could get HTTP status 401. 
                                // Protecting against that right here so multiple authentication requests are
                                // not setup to run on the UI thread.
                                // We only want to authenticate once. Requests should just wait and retry 
                                // with the new token.
                                if (mAtomicAuthenticatingFlag.compareAndSet(false, true))                                                                                                      
                                {
                                    // Authenticate on UI thread
                                    runOnUiThread(new Runnable() {
                                        @Override
                                        public void run() {
                                            // Force a token refresh during authentication.
                                            authenticate(true);
                                        }
                                    });
                                }
    
                                // Wait for authentication to complete then update the token in the request. 
                                waitAndUpdateRequestToken(request);
                                mAtomicAuthenticatingFlag.set(false);                                                  
                            }
                        }
                    }
                }
                return future;
            }
        }


    Dieser Dienstfilter prüft jede Antwort aus, für den Status der HTTP-code 401 "nicht autorisiert". Wenn eine 401 erreicht wird, wird eine neue Anforderung der Anmeldung erhalten Sie ein neues Token Setup im Thread Benutzeroberfläche sein. Andere Anrufe werden blockiert, bis die Anmeldung abgeschlossen ist, oder 5 ebenfalls fehlgeschlagen sind. Wenn das neue Token abgerufen werden, die Anforderung, die die 401 ausgelöst wird mit dem neuen Token wiederholt und eine gesperrte Anrufe mit dem neuen Token wiederholt werden. 

7. Fügen Sie diesen Code in der Datei ToDoActivity.java zur Angabe eines neuen `ProgressFilter` Klasse in der Klasse ToDoActivity:
        
        /**
        * The ProgressFilter class renders a progress bar on the screen during the time the App is waiting for the response of a previous request.
        * the filter shows the progress bar on the beginning of the request, and hides it when the response arrived.
        */
        private class ProgressFilter implements ServiceFilter {
            @Override
            public ListenableFuture<ServiceFilterResponse> handleRequest(ServiceFilterRequest request, NextServiceFilterCallback nextServiceFilterCallback) {

                final SettableFuture<ServiceFilterResponse> resultFuture = SettableFuture.create();

                runOnUiThread(new Runnable() {

                    @Override
                    public void run() {
                        if (mProgressBar != null) mProgressBar.setVisibility(ProgressBar.VISIBLE);
                    }
                });

                ListenableFuture<ServiceFilterResponse> future = nextServiceFilterCallback.onNext(request);

                Futures.addCallback(future, new FutureCallback<ServiceFilterResponse>() {
                    @Override
                    public void onFailure(Throwable e) {
                        resultFuture.setException(e);
                    }

                    @Override
                    public void onSuccess(ServiceFilterResponse response) {
                        runOnUiThread(new Runnable() {

                            @Override
                            public void run() {
                                if (mProgressBar != null) mProgressBar.setVisibility(ProgressBar.GONE);
                            }
                        });

                        resultFuture.set(response);
                    }
                });

                return resultFuture;
            }
        }
        
    Dieser Filter wird die Statusanzeige angezeigt, klicken Sie auf den Anfang der Anfrage und werden sie ausgeblendet, wenn die Antwort eingegangen.

8. Klicken Sie in der Datei ToDoActivity.java Aktualisieren der `onCreate` Methode wie folgt:

        @Override
        public void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            
            setContentView(R.layout.activity_to_do);
            mProgressBar = (ProgressBar) findViewById(R.id.loadingProgressBar);
        
            // Initialize the progress bar
            mProgressBar.setVisibility(ProgressBar.GONE);
        
            try {
                // Create the Mobile Service Client instance, using the provided
                // Mobile Service URL and key
                mClient = new MobileServiceClient(
                        "https://<YOUR MOBILE SERVICE>.azure-mobile.net/",
                        "<YOUR MOBILE SERVICE KEY>", this)
                           .withFilter(new ProgressFilter())
                           .withFilter(new RefreshTokenCacheFilter());
            
                // Authenticate passing false to load the current token cache if available.
                authenticate(false);
                    
            } catch (MalformedURLException e) {
                createAndShowDialog(new Exception("Error creating the Mobile Service. " +
                    "Verify the URL"), "Error");
            }
        }


       In this code, `RefreshTokenCacheFilter` is used in addition to `ProgressFilter`. Also during `onCreate` we want to load the token cache. So `false` is passed in to the `authenticate` method.



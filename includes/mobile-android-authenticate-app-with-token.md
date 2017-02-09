
Im vorherige Beispiel gezeigt einen Standard anmelden, die erfordert, dass des Clients sowohl der Identitätsanbieter und Back-End-Azure Service jedes Mal wenden Sie sich an, die die app gestartet wird. Diese Methode ist nicht nur nicht effizient, Probleme mit der Verwendung auftreten können viele Kunden versuchen, um app zur gleichen Zeit zu starten. Ein besserer Ansatz ist zum Zwischenspeichern von des Autorisierung Tokens zurückgegebene Azure Service, und versuchen Sie, diese ersten verwenden, bevor eine Anmeldung Anbieter-basierten. 

>[AZURE.NOTE]Sie können das Token ausgestellt von Back-End-Azure Service unabhängig davon, ob Sie Client verwaltet oder Dienst verwaltet Authentifizierung arbeiten Zwischenspeichern. In diesem Lernprogramm verwendet Authentifizierung Dienst verwaltet.


1. Öffnen Sie die Datei ToDoActivity.java, und fügen Sie die folgenden Aussagen importieren:

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.content.SharedPreferences.Editor;

2. Hinzufügen der den folgenden Mitgliedern der `ToDoActivity` Class.

        public static final String SHAREDPREFFILE = "temp"; 
        public static final String USERIDPREF = "uid";  
        public static final String TOKENPREF = "tkn";   


3. Fügen Sie in der Datei ToDoActivity.java die folgende Definition für die `cacheUserToken` Methode.
 
        private void cacheUserToken(MobileServiceUser user)
        {
            SharedPreferences prefs = getSharedPreferences(SHAREDPREFFILE, Context.MODE_PRIVATE);
            Editor editor = prefs.edit();
            editor.putString(USERIDPREF, user.getUserId());
            editor.putString(TOKENPREF, user.getAuthenticationToken());
            editor.commit();
        }   
  
    Diese Methode speichert die Benutzer-Id und das Token in einer Voreinstellungsdatei, die als privat markiert ist. Dies sollte Zugriff auf den Cache schützen, sodass andere apps auf dem Gerät nicht Zugriff auf das Token verfügen, da die Einstellung für die app Sandkasten ist. Jedoch, wenn jemand Zugriff auf das Gerät erhält, ist es möglich, dass sie zum token Cache auf andere Weise zugreifen können. 

    >[AZURE.NOTE]Sie können das Token durch Verschlüsselung weiter schützen, wenn token Zugriff auf Ihre Daten hochgradig vertraulich sind und eine andere Person möglicherweise auf dem Gerät zugreifen. Ist jedoch eine vollständig sichere Lösung Gegenstand dieses Lernprogramms und hängt von der Sicherheit an ein.


4. Fügen Sie in der Datei ToDoActivity.java die folgende Definition für die `loadUserTokenCache` Methode.

        private boolean loadUserTokenCache(MobileServiceClient client)
        {
            SharedPreferences prefs = getSharedPreferences(SHAREDPREFFILE, Context.MODE_PRIVATE);
            String userId = prefs.getString(USERIDPREF, null); 
            if (userId == null)
                return false;
            String token = prefs.getString(TOKENPREF, null); 
            if (token == null)
                return false;
                
            MobileServiceUser user = new MobileServiceUser(userId);
            user.setAuthenticationToken(token);
            client.setCurrentUser(user);
                
            return true;
        }



5. Ersetzen Sie in der Datei *ToDoActivity.java* die `authenticate` Methode mit der folgenden Methode, die einen token Cache verwendet. Ändern Sie den Login-Anbieter, wenn Sie ein anderes Konto als Google verwenden möchten.

        private void authenticate() {
            // We first try to load a token cache if one exists.
            if (loadUserTokenCache(mClient))
            {
                createTable();
            }
            // If we failed to load a token cache, login and create a token cache
            else
            {
                // Login using the Google provider.    
                ListenableFuture<MobileServiceUser> mLogin = mClient.login(MobileServiceAuthenticationProvider.Google);
        
                Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
                    @Override
                    public void onFailure(Throwable exc) {
                        createAndShowDialog("You must log in. Login Required", "Error");
                    }           
                    @Override
                    public void onSuccess(MobileServiceUser user) {
                        createAndShowDialog(String.format(
                                "You are now logged in - %1$2s",
                                user.getUserId()), "Success");
                        cacheUserToken(mClient.getCurrentUser());
                        createTable();  
                    }
                });
            }
        }

6. Erstellen Sie die app und Test-Authentifizierung mit einem gültigen Konto an. Mindestens zwei Mal ausführen. Bei der ersten Ausführung erhalten Sie eine Aufforderung zum Anmelden und zum Erstellen von token Caches. Anschließend jede ausführen versucht laden token Cache für die Authentifizierung, und es sollte nicht für die Anmeldung erforderlich sein.




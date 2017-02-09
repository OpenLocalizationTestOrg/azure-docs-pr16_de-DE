
1. Klicken Sie im **Projekt-Explorer** Android Studio öffnen Sie die Datei ToDoActivity.java, und fügen Sie folgenden Aussagen importieren.

        import java.util.concurrent.ExecutionException;
        import java.util.concurrent.atomic.AtomicBoolean;

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.content.SharedPreferences.Editor;

        import com.microsoft.windowsazure.mobileservices.authentication.MobileServiceAuthenticationProvider;
        import com.microsoft.windowsazure.mobileservices.authentication.MobileServiceUser;

2. Fügen Sie die folgende Methode zur Klasse **ToDoActivity** aus: 
    
        private void authenticate() {
            // Login using the Google provider.
            
            ListenableFuture<MobileServiceUser> mLogin = mClient.login(MobileServiceAuthenticationProvider.Google);
    
            Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
                @Override
                public void onFailure(Throwable exc) {
                    createAndShowDialog((Exception) exc, "Error");
                }           
                @Override
                public void onSuccess(MobileServiceUser user) {
                    createAndShowDialog(String.format(
                            "You are now logged in - %1$2s",
                            user.getUserId()), "Success");
                    createTable();  
                }
            });     
        }


    Dies erstellt eine neue Methode, um den Authentifizierungsprozess zu behandeln. Der Benutzer authentifiziert über eine Google Login. Ein Dialogfeld wird angezeigt, der die ID des authentifizierten Benutzers angezeigt. Ohne eine positive Authentifizierung kann nicht fortgesetzt werden.

    > [AZURE.NOTE] Wenn Sie mit einen Identitätsanbieter als Google arbeiten, ändern Sie den Wert, der an die Methode **Login** über auf einen der folgenden übergeben: _MicrosoftAccount_, _Facebook_, _Twitter_oder _Windowsazureactivedirectory_.

3. In der Methode **OnCreate** , fügen Sie die folgende Zeile des Codes nach dem Code, der instanziiert die `MobileServiceClient` Objekt.

        authenticate();

    Dieser Anruf startet die Authentifizierung.

4. Verschieben Sie den verbleibenden Code nach `authenticate();` in einer neuen **CreateTable** Methode **OnCreate** -Methode, die sieht wie folgt aus:

        private void createTable() {
    
            // Get the table instance to use.
            mToDoTable = mClient.getTable(ToDoItem.class);
    
            mTextNewToDo = (EditText) findViewById(R.id.textNewToDo);
    
            // Create an adapter to bind the items with the view.
            mAdapter = new ToDoItemAdapter(this, R.layout.row_list_to_do);
            ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
            listViewToDo.setAdapter(mAdapter);
    
            // Load the items from Azure.
            refreshItemsFromTable();
        }

9. Wählen Sie im Menü **Ausführen** klicken Sie dann auf **app ausführen** , um die app starten, und melden Sie sich mit Ihrer ausgewählten Identitätsanbieter. 

    Wenn Sie erfolgreich angemeldeten sind, sollte die app ohne Fehler ausgeführt, und Sie sollten den Back-End-Dienst Abfragen und Updates Daten möglicherweise.
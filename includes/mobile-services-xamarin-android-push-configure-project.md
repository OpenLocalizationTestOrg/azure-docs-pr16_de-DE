
1. Klicken Sie in der Lösung-Ansicht (oder **Explorer Lösung** in Visual Studio) mit der rechten Maustaste in des Ordners **Komponenten** , klicken Sie auf **Erste weitere Komponenten...**, für die **Google Cloud Messaging-Client** -Komponente suchen ein, und fügen Sie es des Projekts.

2. Die ToDoActivity.cs Projektdatei öffnen, und fügen Sie die folgende Anweisung in der Klasse verwenden:

        using Gcm.Client;

3. Fügen Sie in der Klasse **ToDoActivity** den folgenden Code ein: 

        // Create a new instance field for this activity.
        static ToDoActivity instance = new ToDoActivity();

        // Return the current activity instance.
        public static ToDoActivity CurrentActivity
        {
            get
            {
                return instance;
            }
        }
        // Return the Mobile Services client.
        public MobileServiceClient CurrentClient
        {
            get
            {
                return client;
            }
        }

    So können Sie die Mobilclient Instanz des Prozesses für Pushbenachrichtigungen Ereignishandler zugreifen.

4.  Fügen Sie den folgenden Code der **OnCreate** -Methode, nachdem die **MobileServiceClient** erstellt wurde:

        // Set the current instance of TodoActivity.
        instance = this;

        // Make sure the GCM client is set up correctly.
        GcmClient.CheckDevice(this);
        GcmClient.CheckManifest(this);

        // Register the app for push notifications.
        GcmClient.Register(this, ToDoBroadcastReceiver.senderIDs);

Ihre **ToDoActivity** ist jetzt vorbereitet für Pushbenachrichtigungen hinzufügen.
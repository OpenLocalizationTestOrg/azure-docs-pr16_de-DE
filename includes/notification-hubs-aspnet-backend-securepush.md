## <a name="webapi-project"></a>WebAPI Projekt

1. Öffnen Sie in Visual Studio **AppBackend** Projekts, die Sie in der **Benutzer benachrichtigen** Lernprogramm erstellt haben.
2. Ersetzen Sie in Notifications.cs die gesamte **Benachrichtigungen** Klasse mit den folgenden Code ein. Achten Sie darauf, dass die Platzhalter mit der Verbindungszeichenfolge (mit Vollzugriff) für Ihre Benachrichtigung Hub und den Hubnamen zu ersetzen. Sie können diese Werte aus dem [Klassischen Azure-Portal](http://manage.windowsazure.com)zu erhalten. Dieses Modul stellt jetzt die anderen sicheren Benachrichtigungen, die gesendet wird. In einer vollständigen Implementierung werden die Benachrichtigungen in einer Datenbank gespeichert werden; zur Vereinfachung speichern in diesem Fall wir ihnen im Speicher.

        public class Notification
        {
            public int Id { get; set; }
            public string Payload { get; set; }
            public bool Read { get; set; }
        }
    
    
        public class Notifications
        {
            public static Notifications Instance = new Notifications();
            
            private List<Notification> notifications = new List<Notification>();
    
            public NotificationHubClient Hub { get; set; }
    
            private Notifications() {
                Hub = NotificationHubClient.CreateClientFromConnectionString("{conn string with full access}",  "{hub name}");
            }

            public Notification CreateNotification(string payload)
            {
                var notification = new Notification() {
                Id = notifications.Count,
                Payload = payload,
                Read = false
                };

                notifications.Add(notification);

                return notification;
            }

            public Notification ReadNotification(int id)
            {
                return notifications.ElementAt(id);
            }
        }

20. Ersetzen Sie in NotificationsController.cs den Code innerhalb der Definition der Klasse **NotificationsController** mit den folgenden Code ein. Diese Komponente implementiert eine Möglichkeit für das Gerät die Benachrichtigung sicher abrufen und bietet ebenfalls eine Möglichkeit zum Auslösen eines sicheren Pushbenachrichtigungen auf Geräte (für die Zwecke dieses Lernprogramms). Beachten Sie, dass beim Senden der Benachrichtigung an den Hub Benachrichtigung, wir nur unformatierte einer Benachrichtigung die ID, der die Benachrichtigung (und keine eigentliche Nachricht senden):

        public NotificationsController()
        {
            Notifications.Instance.CreateNotification("This is a secure notification!");
        }

        // GET api/notifications/id
        public Notification Get(int id)
        {
            return Notifications.Instance.ReadNotification(id);
        }

        public async Task<HttpResponseMessage> Post()
        {
            var secureNotificationInTheBackend = Notifications.Instance.CreateNotification("Secure confirmation.");
            var usernameTag = "username:" + HttpContext.Current.User.Identity.Name;

            // windows
            var rawNotificationToBeSent = new Microsoft.Azure.NotificationHubs.WindowsNotification(secureNotificationInTheBackend.Id.ToString(),
                            new Dictionary<string, string> {
                                {"X-WNS-Type", "wns/raw"}
                            });
            await Notifications.Instance.Hub.SendNotificationAsync(rawNotificationToBeSent, usernameTag);

            // apns
            await Notifications.Instance.Hub.SendAppleNativeNotificationAsync("{\"aps\": {\"content-available\": 1}, \"secureId\": \"" + secureNotificationInTheBackend.Id.ToString() + "\"}", usernameTag);

            // gcm
            await Notifications.Instance.Hub.SendGcmNativeNotificationAsync("{\"data\": {\"secureId\": \"" + secureNotificationInTheBackend.Id.ToString() + "\"}}", usernameTag);


            return Request.CreateResponse(HttpStatusCode.OK);
        }


Beachten Sie, dass die `Post` Methode sendet jetzt keine Benachrichtigung Spruch. Es sendet eine unformatierte Benachrichtigung, die nur die Benachrichtigung-ID, und keine vertrauliche Inhalte enthält. Stellen Sie außerdem sicher, um den Sendevorgang für die Plattformen kommentieren, für die Sie nicht so konfiguriert, dass Ihre Benachrichtigung-Hub Anmeldeinformationen verfügen, wie sie zu Fehlern führen.

21. Wir werden nun erneut diese app für eine Website Azure bereitzustellen, um ein von sämtlichen Geräten zugänglich machen. Mit der rechten Maustaste auf das Projekt **AppBackend** , und wählen Sie **Veröffentlichen**aus.

24. Wählen Sie Azure Website als Ziel veröffentlichen aus. Melden Sie sich mit Ihrem Azure-Konto und wählen Sie aus einer vorhandenen oder neuen Website, und notieren Sie die **Ziel-URL** -Eigenschaft auf der Registerkarte **Verbindung** . Wir werden auf diese URL wie Ihre *Back-End-Endpunkt* später in diesem Lernprogramm verweisen. Klicken Sie auf **Veröffentlichen**.
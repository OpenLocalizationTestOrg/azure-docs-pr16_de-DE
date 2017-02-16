



Wenn Sie Vorlage Benachrichtigungen senden, die Sie nur eine Reihe von Eigenschaften bereit müssen, in diesem Fall schicken die Gruppe von Eigenschaften, die mit der lokalisierten Version der aktuelle Neuigkeiten, z. B. wir:

    {
        "News_English": "World News in English!",
        "News_French": "World News in French!",
        "News_Mandarin": "World News in Mandarin!"
    }


In diesem Abschnitt werden Informationen zum Senden von Benachrichtigungen über eine Console-app

Der darin enthaltenen Code sendet an sowohl Windows Store-iOS-Geräte, da die Back-End-eine der unterstützten Geräte übertragen werden kann.


### <a name="to-send-notifications-using-a-c-console-app"></a>Senden von Benachrichtigungen über eine C#-Console-app 

Ändern der `SendTemplateNotificationAsync` Methode in der Console-app, die Sie zuvor erstellt, mit den folgenden Code haben. Beachten Sie, wie in diesem Fall besteht keine Notwendigkeit, mehrere Benachrichtigungen für verschiedene Gebietsschemas und Plattformen zu senden.

        private static async void SendTemplateNotificationAsync()
        {
            // Define the notification hub.
            NotificationHubClient hub = 
                NotificationHubClient.CreateClientFromConnectionString(
                    "<connection string with full access>", "<hub name>");

            // Sending the notification as a template notification. All template registrations that contain 
            // "messageParam" or "News_<local selected>" and the proper tags will receive the notifications. 
            // This includes APNS, GCM, WNS, and MPNS template registrations.
            Dictionary<string, string> templateParams = new Dictionary<string, string>();

            // Create an array of breaking news categories.
            var categories = new string[] { "World", "Politics", "Business", "Technology", "Science", "Sports"};
            var locales = new string[] { "English", "French", "Mandarin" };

            foreach (var category in categories)
            {
                templateParams["messageParam"] = "Breaking " + category + " News!";

                // Sending localized News for each tag too...
                foreach( var locale in locales)
                {
                    string key = "News_" + locale;

                    // Your real localized news content would go here.
                    templateParams[key] = "Breaking " + category + " News in " + locale + "!";
                }

                await hub.SendTemplateNotificationAsync(templateParams, category);
            }
        }


Beachten Sie, dass diese einfache Anruf beim lokalisierten Teil der Informationen an **Alle** Ihren Geräten, unabhängig von der Plattform vorführen, wie Ihre Benachrichtigung Hub erstellt und die richtige systemeigene Nutzlast an alle Geräte, die zu einem bestimmten Tag abonniert stellt.

### <a name="sending-the-notification-with-mobile-services"></a>Senden die Benachrichtigung mit Mobile-Dienste

In der Scheduler Mobile Service können Sie das folgende Skript verwenden:

    var azure = require('azure');
    var notificationHubService = azure.createNotificationHubService('<hub name>', '<connection string with full access>');
    var notification = {
            "News_English": "World News in English!",
            "News_French": "World News in French!",
            "News_Mandarin", "World News in Mandarin!"
    }
    notificationHubService.send('World', notification, function(error) {
        if (!error) {
            console.warn("Notification successful");
        }
    });
    



Dieser Abschnitt listet wie auf dem neusten Stand aus einer .NET Console-app als markierte Vorlage Benachrichtigungen senden an.

Bei Verwendung von Mobile-Apps zum [Hinzufügen von Pushbenachrichtigungen für Mobile-Apps] Lernprogramm verweisen, und wählen Sie Ihre Plattform oben.

Wenn Sie Java verwenden möchten oder PHP finden Sie unter [How Benachrichtigung Hubs von Java/PHP verwendet]. Sie können alle Back-End-über die [Benutzeroberfläche Benachrichtigung Hubs REST]Benachrichtigungen senden.

Wenn Sie die Console-app für Benachrichtigungen senden, wenn Sie [Erste Schritte mit Benachrichtigung Hubs]abgeschlossen erstellt haben, fahren Sie Schritte 1 bis 3 fort.

1. Erstellen Sie eine neue Visual c# Console-Anwendung in Visual Studio:

    ![][13]

2. Klicken Sie in Visual Studio-Hauptmenü auf **Extras**, **Bibliothek Paket-Manager**und **Paket-Manager-Konsole**, klicken Sie dann in den Typ des Fensters Console vor, und drücken Sie die **EINGABETASTE**:

        Install-Package Microsoft.Azure.NotificationHubs

    Dadurch wird einen Verweis auf die mit dem [Microsoft.Azure.Notification Hubs NuGet-Paket]Azure Benachrichtigung Hubs SDK hinzugefügt.

3. Öffnen Sie die Datei Program.cs, und fügen Sie den folgenden `using` Anweisung:

        using Microsoft.Azure.NotificationHubs;

4. In der `Program` Klasse, die folgende Methode hinzuzufügen oder zu ersetzen, wenn sie bereits vorhanden ist:

        private static async void SendTemplateNotificationAsync()
        {
            // Define the notification hub.
            NotificationHubClient hub =
                NotificationHubClient.CreateClientFromConnectionString(
                    "<connection string with full access>", "<hub name>");

            // Create an array of breaking news categories.
            var categories = new string[] { "World", "Politics", "Business",
                                            "Technology", "Science", "Sports"};

            // Sending the notification as a template notification. All template registrations that contain
            // "messageParam" and the proper tags will receive the notifications.
            // This includes APNS, GCM, WNS, and MPNS template registrations.

            Dictionary<string, string> templateParams = new Dictionary<string, string>();

            foreach (var category in categories)
            {
                templateParams["messageParam"] = "Breaking " + category + " News!";
                await hub.SendTemplateNotificationAsync(templateParams, category);
            }
         }

    Dieser Code sendet eine Vorlage Benachrichtigung für jede der sechs Kategorien in der Zeichenfolge an. Die Verwendung von Tags stellt sicher, dass Geräte nur für die registrierten Kategorien Benachrichtigungen erhalten.

6. Ersetzen Sie im obigen Code, der `<hub name>` und `<connection string with full access>` Platzhalter mit Ihren Benachrichtigung Hubnamen und die Verbindungszeichenfolge aus dem Dashboard von Ihrer Benachrichtigung Hub für *DefaultFullSharedAccessSignature* .

7. Fügen Sie die folgenden Zeilen in der **Main** -Methode hinzu:

         SendTemplateNotificationAsync();
         Console.ReadLine();

8. Erstellen der Console-app.

<!-- Images. -->
[13]: ./media/notification-hubs-back-end/notification-hub-create-console-app.png

<!-- URLs. -->
[Erste Schritte mit Benachrichtigung Hubs]: ../articles/notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[Benachrichtigung Hubs REST-Schnittstelle]: http://msdn.microsoft.com/library/windowsazure/dn223264.aspx
[Hinzufügen von Pushbenachrichtigungen für Mobile-Apps]: ../articles/app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md
[So verwenden Sie die Benachrichtigung Hubs von Java/PHP]: ../articles/notification-hubs/notification-hubs-java-push-notification-tutorial.md
[Microsoft.Azure.Notification Hubs NuGet-Paket]: http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/

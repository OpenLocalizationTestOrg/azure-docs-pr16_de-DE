<properties
    pageTitle="Senden von Benachrichtigungen Plattformen für Benutzer mit Benachrichtigung Hubs (ASP.NET)"
    description="Erfahren Sie, wie Benachrichtigung Hubs Vorlagen verwenden, in einer Anforderung, eine Benachrichtigung Plattform-unabhängige senden, die alle Plattformen."
    services="notification-hubs"
    documentationCenter=""
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/03/2016" 
    ms.author="yuaxu"/>

# <a name="send-cross-platform-notifications-to-users-with-notification-hubs"></a>Senden von Benachrichtigungen Plattformen für Benutzer mit Benachrichtigung Hubs


In der vorherigen Lernprogramm [benachrichtigen Benutzer mit Benachrichtigung Hubs]gelernt Sie Benachrichtigungen für alle Geräte, die von einem bestimmten authentifizierten Benutzer registriert Pushbenachrichtigungen. In diesem Lernprogramm wurden mehrere Anforderungen zum Senden einer Benachrichtigung an jede unterstützte Clientplattform erforderlich. Benachrichtigung Hubs unterstützt Vorlagen, mit die Sie können angeben, wie ein bestimmtes Gerät Benachrichtigungen erhalten möchte. Dies vereinfacht Plattformen Benachrichtigungen senden. In diesem Thema veranschaulicht, wie Vorlagen, in einer Anforderung, eine Benachrichtigung Plattform-unabhängige senden, die alle Plattformen nutzen. Ausführlichere Informationen zu Vorlagen finden Sie unter [Azure Benachrichtigung Hubs Übersicht][Templates].

> [AZURE.NOTE] Benachrichtigung Hubs kann ein Gerät mehrere Vorlagen mit demselben Tag registrieren. In diesem Fall führt eine eingehende Nachricht verwendet, denen die Kategorie mehrere Benachrichtigungen auf das Gerät, eine für jede Vorlage übermittelt. So können Sie dieselbe Nachricht in mehreren visuelle Benachrichtigungen, beispielsweise beide als einen Badge und als eine Benachrichtigung Spruch in einer Windows Store-app anzeigen.

Führen Sie die folgenden Schritte aus, um zwischen Plattformen Benachrichtigungen mithilfe von Vorlagen zu senden:

1. Explorer-Lösung in Visual Studio erweitern Sie den Ordner **Controller** , und klicken Sie dann öffnen Sie die Datei RegisterController.cs.

2. Suchen Sie den Codeblock in den **Beitrag** Methode zum Erstellen einer neuen Registrierung ersetzen die `switch` Inhalt mit den folgenden Code:

        switch (deviceUpdate.Platform)
        {
            case "mpns":
                var toastTemplate = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                    "<wp:Notification xmlns:wp=\"WPNotification\">" +
                       "<wp:Toast>" +
                            "<wp:Text1>$(message)</wp:Text1>" +
                       "</wp:Toast> " +
                    "</wp:Notification>";
                registration = new MpnsTemplateRegistrationDescription(deviceUpdate.Handle, toastTemplate);
                break;
            case "wns":
                toastTemplate = @"<toast><visual><binding template=""ToastText01""><text id=""1"">$(message)</text></binding></visual></toast>";
                registration = new WindowsTemplateRegistrationDescription(deviceUpdate.Handle, toastTemplate);
                break;
            case "apns":
                var alertTemplate = "{\"aps\":{\"alert\":\"$(message)\"}}";
                registration = new AppleTemplateRegistrationDescription(deviceUpdate.Handle, alertTemplate);
                break;
            case "gcm":
                var messageTemplate = "{\"data\":{\"message\":\"$(message)\"}}";
                registration = new GcmTemplateRegistrationDescription(deviceUpdate.Handle, messageTemplate);
                break;
            default:
                throw new HttpResponseException(HttpStatusCode.BadRequest);
        }

    Durch diesen Code wird die Plattform-spezifische Methode zum Erstellen einer Vorlage Registrierung anstelle einer systemeigenen Registrierung. Vorhandene Registrierungen müssen nicht geändert werden, da Registrierungen Vorlage aus einer systemeigenen Registrierungen abgeleitet werden.

3. Ersetzen Sie in den Controller **Benachrichtigungen** **SendNotification** -Methode mit den folgenden Code ein:

        public async Task<HttpResponseMessage> Post()
        {
            var user = HttpContext.Current.User.Identity.Name;
            var userTag = "username:" + user;

            var notification = new Dictionary<string, string> { { "message", "Hello, " + user } };
            await Notifications.Instance.Hub.SendTemplateNotificationAsync(notification, userTag);

            return Request.CreateResponse(HttpStatusCode.OK);
        }

    Dieser Code sendet eine Benachrichtigung an alle Plattformen, zur gleichen Zeit und ohne systemeigene Nutzlast anzugeben. Benachrichtigung Hubs erstellt und stellt die richtige Nutzlast an jedem Gerät mit dem Wert bereitgestellten _Kategorie_ , gemäß Angabe in den registrierten Vorlagen.

4. Erneut veröffentlichen Sie WebApi Back-End-Projekt aus.

5. Führen Sie die app Client erneut aus, und stellen Sie sicher, dass die Registrierung erfolgreich ist.

6. (Optional) Bereitstellen der Client-app mit einem zweiten Gerät, und führen Sie die app.

    Beachten Sie, dass eine Benachrichtigung, auf jedem Gerät angezeigt wird.

## <a name="next-steps"></a>Nächste Schritte

Jetzt, da Sie in diesem Lernprogramm abgeschlossen haben, finden Sie weitere Informationen zu Benachrichtigung Hubs und Vorlagen in folgenden Themen:

+ **[Verwenden Sie die Benachrichtigung Hubs auf dem neusten Stand zu senden]** <br/>Veranschaulicht, ein weiteres Szenario für die Verwendung von Vorlagen

+  **[Azure Benachrichtigung Hubs (Übersicht)][Templates]**<br/>Übersichtsthema enthält weitere Informationen zu Vorlagen detaillierte.


<!-- Anchors. -->

<!-- Images. -->




<!-- URLs. -->
[Push to users ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Push to users Mobile Services]: /manage/services/notification-hubs/notify-users/
[Visual Studio 2012 Express for Windows 8]: http://go.microsoft.com/fwlink/?LinkId=257546

[Verwenden Sie die Benachrichtigung Hubs auf dem neusten Stand zu senden]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
[Azure Notification Hubs]: http://go.microsoft.com/fwlink/p/?LinkId=314257
[Benutzer mit Benachrichtigung Hubs benachrichtigen]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Templates]: http://go.microsoft.com/fwlink/p/?LinkId=317339
[Notification Hub How to for Windows Store]: http://msdn.microsoft.com/library/windowsazure/jj927172.aspx

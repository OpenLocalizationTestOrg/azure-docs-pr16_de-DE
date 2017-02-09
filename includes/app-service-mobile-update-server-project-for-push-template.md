In diesem Abschnitt Aktualisieren Sie Code in Ihrem vorhandenen Mobile-Apps Back-End-Projekt eine Benachrichtigung Pushbenachrichtigungen zu senden, jedes Mal, wenn ein neues Element hinzugefügt wird. Dies wird durch Aktivieren Plattformen schiebt Benachrichtigung-Hubs [Vorlage](../articles/notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) Features betrieben. Werden die verschiedenen Clients für Pushbenachrichtigungen mithilfe von Vorlagen registriert und geklickt eines einzelnen universeller Pushbenachrichtigungen können Sie alle Client-Plattformen.

Wählen Sie das Verfahren, die Ihre Back-End-Projekttyp entspricht&mdash;entweder [.NET Back-End-](#dotnet) oder [Node.js Back-End](#nodejs).

### <a name="a-namedotnetanet-backend-project"></a><a name="dotnet"></a>.NET Back-End-Projekt
1. In Visual Studio mit der rechten Maustaste in die Project Server, und klicken Sie auf **NuGet-Pakete verwalten**, suchen Sie nach `Microsoft.Azure.NotificationHubs`, klicken Sie dann auf **Installieren**. So installieren Sie die Benachrichtigung Hubs Bibliothek für das Senden von Benachrichtigungen über Ihre Back-End.

3. Öffnen Sie in der Project Server **Controller** > **TodoItemController.cs**, und fügen Sie den folgenden Anweisungen verwenden:

        using System.Collections.Generic;
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.Mobile.Server.Config;
    

2. Fügen Sie in der Methode **PostTodoItem** nach den Anruf an die **InsertAsync**den folgenden Code hinzu:  

        // Get the settings for the server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings = 
            this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();
        
        // Get the Notification Hubs credentials for the Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create a new Notification Hub client.
        NotificationHubClient hub = NotificationHubClient
        .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

        // Sending the message so that all template registrations that contain "messageParam"
        // will receive the notifications. This includes APNS, GCM, WNS, and MPNS template registrations.
        Dictionary<string,string> templateParams = new Dictionary<string,string>();
        templateParams["messageParam"] = item.Text + " was added to the list.";

        try
        {
            // Send the push notification and log the results.
            var result = await hub.SendTemplateNotificationAsync(templateParams);

            // Write the success result to the logs.
            config.Services.GetTraceWriter().Info(result.State.ToString());
        }
        catch (System.Exception ex)
        {
            // Write the failure result to the logs.
            config.Services.GetTraceWriter()
                .Error(ex.Message, null, "Push.SendAsync Error");
        }

    Dies sendet eine Vorlage Benachrichtigung, die das Element enthalten ist. Text, wenn Sie ein neues Element eingefügt wird.

4. Veröffentlichen Sie das Serverprojekt erneut. 

### <a name="a-namenodejsanodejs-backend-project"></a><a name="nodejs"></a>Node.js Back-End-Projekt

1. Wenn Sie noch nicht getan haben, [Laden Sie das Schnellstart Back-End-Projekt](app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart) oder sonst [online-Editors in der Azure-Portal](app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor)verwenden.

2. Ersetzen Sie den Code in todoitem.js durch den folgenden Code ein:

        var azureMobileApps = require('azure-mobile-apps'),
        promises = require('azure-mobile-apps/src/utilities/promises'),
        logger = require('azure-mobile-apps/src/logger');
    
        var table = azureMobileApps.table();
        
        table.insert(function (context) {
        // For more information about the Notification Hubs JavaScript SDK, 
        // see http://aka.ms/nodejshubs
        logger.info('Running TodoItem.insert');
        
        // Define the template payload.
        var payload = '{"messageParam": "' + context.item.text + '" }';  
        
        // Execute the insert.  The insert returns the results as a Promise,
        // Do the push as a post-execute action within the promise flow.
        return context.execute()
            .then(function (results) {
                // Only do the push if configured
                if (context.push) {
                    // Send a template notification.
                    context.push.send(null, payload, function (error) {
                        if (error) {
                            logger.error('Error while sending push notification: ', error);
                        } else {
                            logger.info('Push notification sent successfully!');
                        }
                    });
                }
                // Don't forget to return the results from the context.execute()
                return results;
            })
            .catch(function (error) {
                logger.error('Error while running context.execute: ', error);
            });
        });

        module.exports = table;  

    Dies sendet eine Vorlage Benachrichtigung, die die item.text enthält, wenn ein neues Element eingefügt wird.

2. Wenn Sie die Datei auf Ihrem lokalen Computer bearbeiten, erneut veröffentlichen der Project Server aus.

Gehen Sie vor, die Ihre Back-End-Projekttyp entspricht&mdash;entweder [.NET Back-End-](#dotnet) oder [Node.js Back-End](#nodejs).

### <a name="a-namedotnetanet-backend-project"></a><a name="dotnet"></a>.NET Back-End-Projekt

1. In Visual Studio mit der rechten Maustaste in die Project Server, und klicken Sie auf **NuGet-Pakete verwalten**, suchen Sie nach `Microsoft.Azure.NotificationHubs`, klicken Sie dann auf **Installieren**. So installieren Sie die Benachrichtigung Hubs Client-Bibliothek.

2. Klicken Sie in den Ordner Controller TodoItemController.cs öffnen, und fügen Sie den folgenden `using` Anweisungen:

        using Microsoft.Azure.Mobile.Server.Config;
        using Microsoft.Azure.NotificationHubs;

3. Ersetzen der `PostTodoItem` Methode mit den folgenden Code:  

      
        public async Task<IHttpActionResult> PostTodoItem(TodoItem item)
        {
            TodoItem current = await InsertAsync(item);
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

            // Android payload
            var androidNotificationPayload = "{ \"data\" : {\"message\":\"" + item.Text + "\"}}";

            try
            {
                // Send the push notification and log the results.
                var result = await hub.SendGcmNativeNotificationAsync(androidNotificationPayload);

                // Write the success result to the logs.
                config.Services.GetTraceWriter().Info(result.State.ToString());
            }
            catch (System.Exception ex)
            {
                // Write the failure result to the logs.
                config.Services.GetTraceWriter()
                    .Error(ex.Message, null, "Push.SendAsync Error");
            }
            return CreatedAtRoute("Tables", new { id = current.Id }, current);
        }

4. Veröffentlichen Sie das Serverprojekt erneut.

### <a name="a-namenodejsanodejs-backend-project"></a><a name="nodejs"></a>Node.js Back-End-Projekt

1. Wenn Sie noch nicht getan haben, [Laden Sie das Projekt Schnellstart](app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart) sonst [online-Editors in der Azure-Portal](app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor)verwenden.
 
1. Ersetzen Sie den Code in der Datei todoitem.js durch den folgenden Code ein:

        var azureMobileApps = require('azure-mobile-apps'),
        promises = require('azure-mobile-apps/src/utilities/promises'),
        logger = require('azure-mobile-apps/src/logger');
        
        var table = azureMobileApps.table();
        
        table.insert(function (context) {
        // For more information about the Notification Hubs JavaScript SDK, 
        // see http://aka.ms/nodejshubs
        logger.info('Running TodoItem.insert');
        
        // Define the GCM payload.
        var payload = {
            "data": {
                "message": context.item.text
            }
        };   
        
        // Execute the insert.  The insert returns the results as a Promise,
        // Do the push as a post-execute action within the promise flow.
        return context.execute()
            .then(function (results) {
                // Only do the push if configured
                if (context.push) {
                    // Send a GCM native notification.
                    context.push.gcm.send(null, payload, function (error) {
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

    Dies sendet eine GCM-Benachrichtigung, die die item.text enthält, wenn ein neues erledigen Element eingefügt wird. 

2. Wenn Sie die Datei in Ihrem lokalen Computer bearbeiten, erneut veröffentlichen der Project Server aus. 

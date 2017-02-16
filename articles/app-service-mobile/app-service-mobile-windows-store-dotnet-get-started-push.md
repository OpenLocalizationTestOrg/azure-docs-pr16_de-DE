<properties
    pageTitle="Hinzufügen von Pushbenachrichtigungen zu Ihrer Anwendung Universal Windows-Plattform (UWP) | Azure Mobile-Apps"
    description="Informationen Sie zum Azure App Dienst Mobile-Apps und Azure Benachrichtigung Hubs verwenden, um Pushbenachrichtigungen zu Ihrer Anwendung Universal Windows-Plattform (UWP) zu senden."
    services="app-service\mobile,notification-hubs"
    documentationCenter="windows"
    authors="ysxu"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-windows-app"></a>Hinzufügen von Pushbenachrichtigungen zu Ihrem Windows-app

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

##<a name="overview"></a>(Übersicht)

In diesem Lernprogramm fügen Sie Pushbenachrichtigungen des Projekts [Windows schnell zu starten](app-service-mobile-windows-store-dotnet-get-started.md) , damit eine Pushbenachrichtigung an das Gerät gesendet werden, jedes Mal, wenn ein Datensatz eingefügt wird.

Wenn Sie die heruntergeladene Schnellstart Server Project nicht verwenden, benötigen Sie das Pushbenachrichtigungen Benachrichtigung Erweiterungspaket. Weitere Informationen finden Sie unter [Arbeiten mit der Back-End-Server SDK für Mobile-Apps Azure](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) .

##<a name="a-nameconfigure-hubaconfigure-a-notification-hub"></a><a name="configure-hub"></a>Konfigurieren eines Benachrichtigung Hubs

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

##<a name="register-your-app-for-push-notifications"></a>Registrieren Sie Ihre app für Pushbenachrichtigungen

Sie müssen übermitteln Sie Ihre app im Windows Store, und klicken Sie dann Konfigurieren des Projekts Server mit Windows Benachrichtigung Services (WNS) zum Senden von Pushbenachrichtigungen integriert werden soll.

1. In Visual Studio-Lösung Explorer Maustaste auf das UWP-app-Projekt, und klicken Sie auf **Speichern** > **App mit dem Store... zugeordnet werden soll**. 

    ![Verknüpfen mit Windows Store-app](./media/app-service-mobile-windows-store-dotnet-get-started-push/notification-hub-associate-uwp-app.png)
    
2. Klicken Sie im Assistenten klicken Sie auf **Weitersuchen**, melden Sie sich mit Ihrem Microsoft-Konto, geben Sie einen Namen für Ihre app in **Reservieren einen neuen Namen für die app**, klicken Sie auf **Reservieren**.

3. Nach die app-Registrierung erfolgreich erstellt wurde, wählen Sie den neuen Namen für die app aus, klicken Sie auf **Weiter**, und klicken Sie dann auf **Verbinden**. Dadurch wird die erforderliche Windows Store-Registrierungsinformationen Anwendungsmanifest hinzugefügt.  

7. Navigieren Sie zum [Windows Developer Center](https://dev.windows.com/en-us/overview), mit Ihrem Microsoft-Konto anmelden, klicken Sie auf die neue app-Registrierung in **Meine apps**, und klicken Sie dann erweitern Sie **Dienste** > **Pushbenachrichtigungen**. 

8. Klicken Sie auf der Seite **Pushbenachrichtigungen** unter **Microsoft Azure Mobile-Dienste**auf **Live Services-Website** .

9. Notieren Sie in der Registrierungsseite den Wert unter **Anwendung Kennwörter** und **Paket SID**, die Sie als Nächstes zum Konfigurieren Ihrer mobilen app Back-End-verwenden. 

    ![Verknüpfen mit Windows Store-app](./media/app-service-mobile-windows-store-dotnet-get-started-push/app-service-mobile-uwp-app-push-auth.png)

    > [AZURE.IMPORTANT] Der Client geheim und Paket SID sind wichtige Sicherheitsanmeldeinformationen. Teilen Sie diese Werte mit jedem oder verteilen Sie diese App nicht. Die **Anwendung-Id** wird mit dem geheimen Schlüssel zum Konfigurieren der Microsoft Account Authentifizierung verwendet.

##<a name="configure-the-backend-to-send-push-notifications"></a>Konfigurieren Sie die Back-End-um Pushbenachrichtigungen zu senden.

[AZURE.INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

##<a name="a-idupdate-serviceaupdate-the-server-to-send-push-notifications"></a><a id="update-service"></a>Aktualisieren Sie den Server, um Pushbenachrichtigungen zu senden.

Verwenden Sie das Verfahren, die Ihre Back-End-Projekttyp entspricht&mdash;entweder [.NET Back-End-](#dotnet) oder [Node.js Back-End](#nodejs).

### <a name="a-namedotnetanet-backend-project"></a><a name="dotnet"></a>.NET Back-End-Projekt

1. In Visual Studio mit der rechten Maustaste in die Project Server klicken Sie auf **NuGet-Pakete verwalten**, suchen Sie nach Microsoft.Azure.NotificationHubs, und klicken Sie auf **Installieren**. So installieren Sie die Benachrichtigung Hubs Client-Bibliothek.

2. **Blenden Sie**, TodoItemController.cs zu öffnen, und fügen Sie den folgenden Anweisungen verwenden:

        using System.Collections.Generic;
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.Mobile.Server.Config;

3. Fügen Sie in der Methode **PostTodoItem** nach den Anruf an die **InsertAsync**den folgenden Code hinzu:

        // Get the settings for the server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings =
            this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();

        // Get the Notification Hubs credentials for the Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create the notification hub client.
        NotificationHubClient hub = NotificationHubClient
            .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

        // Define a WNS payload
        var windowsToastPayload = @"<toast><visual><binding template=""ToastText01""><text id=""1"">"
                                + item.Text + @"</text></binding></visual></toast>";
        try
        {
            // Send the push notification.
            var result = await hub.SendWindowsNativeNotificationAsync(windowsToastPayload);

            // Write the success result to the logs.
            config.Services.GetTraceWriter().Info(result.State.ToString());
        }
        catch (System.Exception ex)
        {
            // Write the failure result to the logs.
            config.Services.GetTraceWriter()
                .Error(ex.Message, null, "Push.SendAsync Error");
        }

    Dieser Code weist den Benachrichtigung Hub eine Benachrichtigung Pushbenachrichtigungen zu senden, nach der Einfügemarke um ein neues Element.

4. Veröffentlichen Sie das Serverprojekt erneut.

### <a name="a-namenodejsanodejs-backend-project"></a><a name="nodejs"></a>Node.js Back-End-Projekt

1. Wenn Sie noch nicht getan haben, [Laden Sie das Projekt Schnellstart](app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart) sonst [online-Editors in der Azure-Portal](app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor)verwenden.

2. Ersetzen Sie den Code in der Datei todoitem.js durch den folgenden Code ein:

        var azureMobileApps = require('azure-mobile-apps'),
        promises = require('azure-mobile-apps/src/utilities/promises'),
        logger = require('azure-mobile-apps/src/logger');

        var table = azureMobileApps.table();

        table.insert(function (context) {
        // For more information about the Notification Hubs JavaScript SDK,
        // see http://aka.ms/nodejshubs
        logger.info('Running TodoItem.insert');

        // Define the WNS payload that contains the new item Text.
        var payload = "<toast><visual><binding template=\ToastText01\><text id=\"1\">"
                                    + context.item.text + "</text></binding></visual></toast>";

        // Execute the insert.  The insert returns the results as a Promise,
        // Do the push as a post-execute action within the promise flow.
        return context.execute()
            .then(function (results) {
                // Only do the push if configured
                if (context.push) {
                    // Send a WNS native toast notification.
                    context.push.wns.sendToast(null, payload, function (error) {
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

    Dies sendet eine WNS Spruch Benachrichtigung, die die item.text enthält, wenn ein neues erledigen Element eingefügt wird.

2. Wenn Sie die Datei auf Ihrem lokalen Computer bearbeiten, erneut veröffentlichen der Project Server aus.

##<a name="a-idupdate-appaadd-push-notifications-to-your-app"></a><a id="update-app"></a>Hinzufügen von Pushbenachrichtigungen zu Ihrer Anwendung

Als Nächstes muss Ihre app für Pushbenachrichtigungen beim Start registrieren. Wenn Sie bereits Authentifizierung aktiviert haben, stellen Sie sicher, dass der Benutzer Vorzeichen Add-in vor dem Versuch für Pushbenachrichtigungen zu registrieren.

1. Öffnen Sie die Projektdatei **App.xaml.cs** , und fügen Sie den folgenden `using` Anweisungen:

        using System.Threading.Tasks;
        using Windows.Networking.PushNotifications;

2. Fügen Sie in derselben Datei die Definition der folgende **InitNotificationsAsync** -Methode der **App** -Klasse hinzu:

        private async Task InitNotificationsAsync()
        {
            // Get a channel URI from WNS.
            var channel = await PushNotificationChannelManager
                .CreatePushNotificationChannelForApplicationAsync();

            // Register the channel URI with Notification Hubs.
            await App.MobileService.GetPush().RegisterAsync(channel.Uri);
        }

    Dieser Code Ruft die ChannelURI für die app aus WNS ab, und registriert die ChannelURI App Dienst Mobile-App.

3. Am oberen Rand der **OnLaunched** Ereignishandler in **App.xaml.cs**den **asynchrone** Modifizierer Definition der Methode und Hinzufügen des folgenden Anrufs an die neue **InitNotificationsAsync** -Methode, wie im folgenden Beispiel:

        protected async override void OnLaunched(LaunchActivatedEventArgs e)
        {
            await InitNotificationsAsync();

            // ...
        }

    Dadurch wird sichergestellt, dass die kurzlebigen ChannelURI jedes Mal registriert ist, die die Anwendung gestartet wird.

4. Erstellen Sie Ihr UWP app-Projekt neu. Ihre app kann nun Spruch Benachrichtigungen erhalten.

##<a name="a-idtestatest-push-notifications-in-your-app"></a><a id="test"></a>Testen von Pushbenachrichtigungen in Ihrer app

[AZURE.INCLUDE [app-service-mobile-windows-universal-test-push](../../includes/app-service-mobile-windows-universal-test-push.md)]


##<a name="a-idmoreanext-steps"></a><a id="more"></a>Nächste Schritte

Weitere Informationen zu Pushbenachrichtigungen:

* [Verwendung von verwalteten Client für Azure Mobile-Apps](app-service-mobile-dotnet-how-to-use-client-library.md#how-to-register-push-templates-to-send-cross-platform-notifications)  
Vorlagen bieten Ihnen Flexibilität Plattformen schiebt und lokalisierten Push-Vorgänge senden. Informationen Sie zum Registrieren von Vorlagen.

* [Diagnostizieren von Pushbenachrichtigungen Benachrichtigung Probleme bei](../notification-hubs/notification-hubs-push-notification-fixer.md)  
Es gibt verschiedene Gründe, warum Benachrichtigungen möglicherweise abgelegt erhalten, oder gehen Sie wie folgt auf Geräten nicht beenden. In diesem Thema wird gezeigt, wie analysieren und Ermitteln der Ursachen von Pushbenachrichtigungen Benachrichtigung Fehlern. 

Berücksichtigen Sie mit einer der folgenden Lernprogramme fortfahren:

+ [Authentifizierung für Ihre app hinzufügen](app-service-mobile-windows-store-dotnet-get-started-users.md)  
  Erfahren Sie, wie Benutzer der app mit einem Identitätsanbieter authentifizieren.

+ [Offlinesynchronisierung für Ihre app zu aktivieren](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  Erfahren Sie, wie offlinesupport Ihre app ein Mobile-App Back-End hinzufügen. Offlinesynchronisierung ermöglicht es den Endbenutzern Interaktion mit einem mobilen app&mdash;anzeigen, hinzufügen oder Ändern von Daten&mdash;auch, wenn keine Verbindung zum Netzwerk vorhanden ist.

<!-- Anchors. -->

<!-- URLs. -->
[Azure Portal]: https://portal.azure.com/

<!-- Images. -->


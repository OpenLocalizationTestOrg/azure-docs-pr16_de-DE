<properties
    pageTitle="Senden von Pushbenachrichtigungen in Chrome-apps mit Azure Benachrichtigung Hubs | Microsoft Azure"
    description="Informationen Sie zum Azure Benachrichtigung Hubs verwenden, um Pushbenachrichtigungen zu einer App Chrome zu senden."
    services="notification-hubs"
    keywords="Mobile-Pushbenachrichtigungen, Pushbenachrichtigungen, Pushbenachrichtigungen Benachrichtigung chrome Pushbenachrichtigungen"
    documentationCenter=""
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-chrome"
    ms.devlang="JavaScript"
    ms.topic="hero-article"
    ms.date="10/03/2016"
    ms.author="yuaxu"/>

# <a name="send-push-notifications-to-chrome-apps-with-azure-notification-hubs"></a>Senden von Pushbenachrichtigungen in Chrome-apps mit Azure Benachrichtigung Hubs

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

In diesem Thema wird gezeigt, wie mit Azure Benachrichtigung Hubs Pushbenachrichtigungen zu einer App Chrome senden, das innerhalb des Kontexts von Google Chrome-Browsers angezeigt wird. In diesem Lernprogramm erstellen wir eine app Chrome, die Pushbenachrichtigungen empfängt mithilfe von [Google Cloud Messaging (GCM)](https://developers.google.com/cloud-messaging/). 

>[AZURE.NOTE] Um dieses Lernprogramms abgeschlossen haben, müssen Sie ein aktives Azure-Konto verfügen. Wenn Sie kein Konto haben, können Sie ein kostenloses Testversion Konto nur wenigen Minuten erstellen. Weitere Informationen finden Sie unter [Azure kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%notification-hubs-chrome-get-started%2F).

Das Lernprogramm führt Sie durch die folgenden grundlegenden Schritte aus, um Pushbenachrichtigungen zu aktivieren:

* [Aktivieren von Google-Cloud Messaging](#register)
* [Konfigurieren Sie den Benachrichtigung hub](#configure-hub)
* [Herstellen einer Verbindung im Infobereich Hub mit Ihrer Chrome-App](#connect-app)
* [Senden Sie eine Benachrichtigung Pushbenachrichtigungen zu Ihrer Anwendung Chrome](#send)
* [Zusätzliche Funktionen und Funktionen](#next-steps)

>[AZURE.NOTE] Chrome app Pushbenachrichtigungen sind nicht generische im Browser Benachrichtigungen – sie sind speziell für die Erweiterbarkeit Browser modellieren (Details finden Sie unter [Übersicht über Chrome-Apps] ). Führen Sie zusätzlich zu den Desktopbrowser Chrome-apps auf Mobile (Android und iOS) bis Apache Cordova ein. Finden Sie unter [Chrome Apps Mobile] Weitere.

Konfigurieren von GCM und Azure Benachrichtigung Hubs entspricht konfigurieren für Android, da [Google Cloud Messaging für Chrome] veraltet ist und die gleiche GCM jetzt sowohl Android-Geräten und Chrome Instanzen unterstützt.

##<a name="a-idregisteraenable-google-cloud-messaging"></a><a id="register"></a>Aktivieren von Google-Cloud Messaging

1. Navigieren Sie zu der Website [Google-Cloud-Verwaltungskonsole] , melden Sie sich mit Ihrer Gmail-Konto-Anmeldeinformationen, und klicken Sie dann auf die Schaltfläche **Projekt erstellen** . Geben Sie einen entsprechenden **Projektnamen ein**, und klicken Sie dann auf die Schaltfläche **Erstellen** .

    ![Google Cloud Console - Projekt erstellen][1]

2. Notieren Sie die **Projektnummer** auf der Seite **Projekte** für das Projekt, das Sie gerade erstellt haben. Sie werden diese als die **GCM Absender-ID** in der App Chrome verwenden, um mit GCM registrieren.

    ![Google Cloud Console - Projektnummer][2]

3. Im linken Bereich klicken Sie auf **APIs und Authentifizierung**, und klicken Sie dann einen Bildlauf nach unten, und klicken Sie auf die Umschaltfläche, um das Aktivieren von **Google Cloud Messaging für Android**. Sie müssen nicht **Google Cloud für Chrome Messaging**aktivieren.

    ![Google Cloud Console - Server-Taste][3]

4. Klicken Sie im linken Bereich auf **Anmeldeinformationen** > **Erstellen neuer Schlüssel** > **Serverschlüssel** > **Erstellen**.

    ![Google Cloud Console - Anmeldeinformationen][4]

5. Notieren Sie die Server **-API-Taste**. Dies konfigurieren in der Benachrichtigung Hub als Nächstes Sie um Pushbenachrichtigungen zu GCM senden ermöglichen.

    ![Google Cloud Console - API-Schlüssel][5]

##<a name="a-idconfigure-hubaconfigure-your-notification-hub"></a><a id="configure-hub"></a>Konfigurieren Sie den Benachrichtigung hub

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]


&emsp;&emsp;6. Wählen Sie in den **Einstellungen** Blade **Benachrichtigungsdienst** und **Google (GCM)**aus. Geben Sie den Key API und speichern.

&emsp;&emsp;![Azure Benachrichtigung Hubs - Google (GCM)](./media/notification-hubs-android-get-started/notification-hubs-gcm-api.png)

##<a name="a-idconnect-appaconnect-your-chrome-app-to-the-notification-hub"></a><a id="connect-app"></a>Herstellen einer Verbindung im Infobereich Hub mit Ihrer Chrome-App

Ihre Benachrichtigung Hub ist jetzt so konfiguriert, dass die Arbeit mit GCM, und Sie haben die Verbindungszeichenfolgen Ihre app zum Empfangen und Senden von Pushbenachrichtigungen registrieren. LK

###<a name="create-a-new-chrome-app"></a>Erstellen einer neuen Chrome-App

Im folgenden Beispiel wird basierend auf den [Chrome App GCM Stichproben] und die empfohlene Vorgehensweise zum Erstellen einer App Chrome verwendet. Wir markieren Sie die Schritten speziell im Zusammenhang mit Azure Benachrichtigung Hubs. 

>[AZURE.NOTE] Es empfiehlt sich, dass Sie die Quelle für diese App Chrome [Chrome App Benachrichtigung Hub (Beispiel)]herunterladen.

Wird die App Chrome über JavaScript erstellt, und Sie können Ihre bevorzugten Word Editoren für Sie erstellt haben. Es folgt wie diese App Chrome aussieht.

![Google Chrome-App][15]

1. Erstellen Sie einen Ordner aus, und nennen Sie es `ChromePushApp`. Der Name ist natürlich beliebig – Wenn Sie etwas anderes nennen, stellen Sie sicher, dass Sie den Pfad in den erforderlichen Codesegmenten ersetzen.

2. Laden Sie die [kryptomobilität Js Bibliothek] in den Ordner, den Sie im zweiten Schritt erstellt haben. Diese Bibliotheksordner enthält zwei Unterordner: `components` und `rollups`.

3. Erstellen einer `manifest.json` Datei. Alle Chrome-Apps als Unterstützung über eine Manifestdatei, die app-Metadaten und die meisten enthält, wichtiger ist, dass alle Berechtigungen, die die app gewährt werden, wenn der Benutzer es installiert.

        {
          "name": "NH-GCM Notifications",
          "description": "Chrome platform app.",
          "manifest_version": 2,
          "version": "0.1",
          "app": {
            "background": {
              "scripts": ["background.js"]
            }
          },
          "permissions": ["gcm", "storage", "notifications", "https://*.servicebus.windows.net/*"],
          "icons": { "128": "gcm_128.png" }
        }

    Hinweis Die `permissions` Element, das angibt, dass diese App Chrome Pushbenachrichtigungen von GCM erhalten werden. Sie müssen außerdem Azure Benachrichtigung Hubs URI angeben, in dem die App Chrome REST registrieren aufrufen wird.
    Unsere Beispiel-app verwendet auch eine Symboldatei `gcm_128.png`, die finden Sie an der Quelle, die aus der ursprünglichen GCM Stichprobe wiederverwendet wird. Sie können es in jedem Bild ersetzen, die den [Kriterien Symbol](https://developer.chrome.com/apps/manifest/icons)entspricht.

4. Erstellen Sie eine Datei namens `background.js` mit den folgenden Code:

        // Returns a new notification ID used in the notification.
        function getNotificationId() {
          var id = Math.floor(Math.random() * 9007199254740992) + 1;
          return id.toString();
        }

        function messageReceived(message) {
          // A message is an object with a data property that
          // consists of key-value pairs.

          // Concatenate all key-value pairs to form a display string.
          var messageString = "";
          for (var key in message.data) {
            if (messageString != "")
              messageString += ", "
            messageString += key + ":" + message.data[key];
          }
          console.log("Message received: " + messageString);

          // Pop up a notification to show the GCM message.
          chrome.notifications.create(getNotificationId(), {
            title: 'GCM Message',
            iconUrl: 'gcm_128.png',
            type: 'basic',
            message: messageString
          }, function() {});
        }

        var registerWindowCreated = false;

        function firstTimeRegistration() {
          chrome.storage.local.get("registered", function(result) {

            registerWindowCreated = true;
            chrome.app.window.create(
              "register.html",
              {  width: 520,
                 height: 500,
                 frame: 'chrome'
              },
              function(appWin) {}
            );
          });
        }

        // Set up a listener for GCM message event.
        chrome.gcm.onMessage.addListener(messageReceived);

        // Set up listeners to trigger the first-time registration.
        chrome.runtime.onInstalled.addListener(firstTimeRegistration);
        chrome.runtime.onStartup.addListener(firstTimeRegistration);

    Dies ist die Datei, die im HTML-Code (**register.html**) des Chrome-App-Fenster informiert und definiert außerdem die Ereignishandler **MessageReceived** , um der Pushbenachrichtigung von eingehenden zu behandeln.

5. Erstellen Sie eine Datei namens `register.html` -Hiermit wird die Benutzeroberfläche von der App Chrome definiert. 

   >[AZURE.NOTE] In diesem Beispiel wird die **CryptoJS v3.1.2**verwendet. Wenn Sie eine andere Version der Bibliothek heruntergeladen haben, vergewissern Sie sich ordnungsgemäß ersetzen Sie die Version in der `src` Pfad.

        <html>

        <head>
        <title>GCM Registration</title>
        <script src="register.js"></script>
        <script src="CryptoJS v3.1.2/rollups/hmac-sha256.js"></script>
        <script src="CryptoJS v3.1.2/components/enc-base64-min.js"></script>
        </head>

        <body>

        Sender ID:<br/><input id="senderId" type="TEXT" size="20"><br/>
        <button id="registerWithGCM">Register with GCM</button>
        <br/>
        <br/>
        <br/>

        Notification Hub Name:<br/><input id="hubName" type="TEXT" style="width:400px"><br/><br/>
        Connection String:<br/><textarea id="connectionString" type="TEXT" style="width:400px;height:60px"></textarea>

        <br/>

        <button id="registerWithNH" disabled="true">Register with Azure Notification Hubs</button>

        <br/>
        <br/>

        <textarea id="console" type="TEXT" readonly style="width:500px;height:200px;background-color:#e5e5e5;padding:5px"></textarea>

        </body>

        </html>

6. Erstellen Sie eine Datei namens `register.js` durch den folgenden Code. Diese Datei gibt an, das Skript hinter `register.html`. Chrome Apps erlauben nicht Inline ausführen, daher Sie in einem separaten dahinter liegende Skript für die Benutzeroberfläche zu erstellen müssen.

        var registrationId = "";
        var hubName        = "", connectionString = "";
        var originalUri    = "", targetUri = "", endpoint = "", sasKeyName = "", sasKeyValue = "", sasToken = "";

        window.onload = function() {
           document.getElementById("registerWithGCM").onclick = registerWithGCM;  
           document.getElementById("registerWithNH").onclick = registerWithNH;
           updateLog("You have not registered yet. Please provider sender ID and register with GCM and then with  Notification Hubs.");
        }

        function updateLog(status) {
          currentStatus = document.getElementById("console").innerHTML;
          if (currentStatus != "") {
            currentStatus = currentStatus + "\n\n";
          }

          document.getElementById("console").innerHTML = currentStatus  + status;
        }

        function registerWithGCM() {
          var senderId = document.getElementById("senderId").value.trim();
          chrome.gcm.register([senderId], registerCallback);

          // Prevent register button from being clicked again before the registration finishes.
          document.getElementById("registerWithGCM").disabled = true;
        }

        function registerCallback(regId) {
          registrationId = regId;
          document.getElementById("registerWithGCM").disabled = false;

          if (chrome.runtime.lastError) {
            // When the registration fails, handle the error and retry the
            // registration later.
            updateLog("Registration failed: " + chrome.runtime.lastError.message);
            return;
          }

          updateLog("Registration with GCM succeeded.");
          document.getElementById("registerWithNH").disabled = false;

          // Mark that the first-time registration is done.
          chrome.storage.local.set({registered: true});
        }

        function registerWithNH() {
          hubName = document.getElementById("hubName").value.trim();
          connectionString = document.getElementById("connectionString").value.trim();
          splitConnectionString();
          generateSaSToken();
          sendNHRegistrationRequest();
        }

        // From http://msdn.microsoft.com/library/dn495627.aspx
        function splitConnectionString()
        {
          var parts = connectionString.split(';');
          if (parts.length != 3)
          throw "Error parsing connection string";

          parts.forEach(function(part) {
            if (part.indexOf('Endpoint') == 0) {
            endpoint = 'https' + part.substring(11);
            } else if (part.indexOf('SharedAccessKeyName') == 0) {
            sasKeyName = part.substring(20);
            } else if (part.indexOf('SharedAccessKey') == 0) {
            sasKeyValue = part.substring(16);
            }
          });

          originalUri = endpoint + hubName;
        }

        function generateSaSToken()
        {
          targetUri = encodeURIComponent(originalUri.toLowerCase()).toLowerCase();
          var expiresInMins = 10; // 10 minute expiration

          // Set expiration in seconds.
          var expireOnDate = new Date();
          expireOnDate.setMinutes(expireOnDate.getMinutes() + expiresInMins);
          var expires = Date.UTC(expireOnDate.getUTCFullYear(), expireOnDate
            .getUTCMonth(), expireOnDate.getUTCDate(), expireOnDate
            .getUTCHours(), expireOnDate.getUTCMinutes(), expireOnDate
            .getUTCSeconds()) / 1000;
          var tosign = targetUri + '\n' + expires;

          // Using CryptoJS.
          var signature = CryptoJS.HmacSHA256(tosign, sasKeyValue);
          var base64signature = signature.toString(CryptoJS.enc.Base64);
          var base64UriEncoded = encodeURIComponent(base64signature);

          // Construct authorization string.
          sasToken = "SharedAccessSignature sr=" + targetUri + "&sig="
                          + base64UriEncoded + "&se=" + expires + "&skn=" + sasKeyName;
        }

        function sendNHRegistrationRequest()
        {
          var registrationPayload =
          "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
          "<entry xmlns=\"http://www.w3.org/2005/Atom\">" +
              "<content type=\"application/xml\">" +
                  "<GcmRegistrationDescription xmlns:i=\"http://www.w3.org/2001/XMLSchema-instance\" xmlns=\"http://schemas.microsoft.com/netservices/2010/10/servicebus/connect\">" +
                      "<GcmRegistrationId>{GCMRegistrationId}</GcmRegistrationId>" +
                  "</GcmRegistrationDescription>" +
              "</content>" +
          "</entry>";

          // Update the payload with the registration ID obtained earlier.
          registrationPayload = registrationPayload.replace("{GCMRegistrationId}", registrationId);

          var url = originalUri + "/registrations/?api-version=2014-09";
          var client = new XMLHttpRequest();

          client.onload = function () {
            if (client.readyState == 4) {
              if (client.status == 200) {
                updateLog("Notification Hub Registration succesful!");
                updateLog(client.responseText);
              } else {
                updateLog("Notification Hub Registration did not succeed!");
                updateLog("HTTP Status: " + client.status + " : " + client.statusText);
                updateLog("HTTP Response: " + "\n" + client.responseText);
              }
            }
          };

          client.onerror = function () {
                updateLog("ERROR - Notification Hub Registration did not succeed!");
          }

          client.open("POST", url, true);
          client.setRequestHeader("Content-Type", "application/atom+xml;type=entry;charset=utf-8");
          client.setRequestHeader("Authorization", sasToken);
          client.setRequestHeader("x-ms-version", "2014-09");

          try {
              client.send(registrationPayload);
          }
          catch(err) {
              updateLog(err.message);
          }
        }

    Das obige Skript weist die folgenden wichtigen Parameter:
    - **Window.OnLoad** definiert die Schaltfläche Klick-Ereignisse von zwei Schaltflächen auf der Benutzeroberfläche an. Eine mit GCM registriert und die andere verwendet die Registrierung-ID, die nach der Registrierung mit GCM mit Azure Benachrichtigung Hubs registrieren zurückgegeben wird.
    - **UpdateLog** ist die Funktion, die Funktionen einfache Protokollierung verarbeitet werden kann.
    - **RegisterWithGCM** ist der erste Schaltfläche Klick Ereignishandler, wodurch die `chrome.gcm.register` einwählen GCM die aktuelle Instanz des Chrome-App zu registrieren.
    - **RegisterCallback** ist der Rückruffunktion, die aufgerufen wird, wenn der Anruf GCM Registrierung gibt.
    - **RegisterWithNH** ist im zweiten Schaltfläche Klick Ereignishandler die mit Benachrichtigung Hubs registriert. Es wird `hubName` und `connectionString` (die der Benutzer angegeben hat) und den Anruf Benachrichtigung Hubs Registrierung REST-API bietet.
    - **SplitConnectionString** und **GenerateSaSToken** , sind Hilfen, die die JavaScript-Implementierung von token Erstellungsprozess eines SaS darstellen, die in allen REST-API Aufrufen verwendet werden muss. Weitere Informationen finden Sie unter [Allgemeine Konzepte](http://msdn.microsoft.com/library/dn495627.aspx).
    - **SendNHRegistrationRequest** ist die Funktion, die eine HTTP (REST) anrufen Azure Benachrichtigung Hubs ist.
    - **RegistrationPayload** definiert die Registrierung XML-Nutzlast. Weitere Informationen finden Sie unter [Registrierung NH REST-API erstellen]. Wir aktualisieren die Registrierung ID darin mit was wir von GCM erhalten.
    - **Client** ist eine Instanz von **XMLHttpRequest** , die wir verwenden, um die HTTP POST-Anforderung vorzunehmen. Notiz, die wir aktualisieren die `Authorization` Kopfzeile mit `sasToken`. Erfolgreichen Abschluss dieses Anruf wird diese Instanz des Chrome-App mit Azure Benachrichtigung Hubs registrieren.


Die gesamte Ordnerstruktur für dieses Projekt sollte wie folgt aussehen:     ![Google Chrome App - Ordnerstruktur][21]

###<a name="set-up-and-test-your-chrome-app"></a>Richten Sie ein und Testen Sie Ihre App Chrome

1. Öffnen Sie Ihren Chrome-Browser. Öffnen Sie **Chrome-Erweiterungen** zu, und aktivieren Sie **Entwicklermodus**.

    ![Google Chrome - Entwicklermodus aktivieren][16]

2. Klicken Sie auf **entpackt Erweiterung laden** , und navigieren Sie zu dem Ordner, in dem Sie die Dateien erstellt. Sie können auch optional **Chrome-Apps und Erweiterungen Developer Tools**verwenden. Dieses Tool ist eine App Chrome in selbst (aus dem Chrome-Web-Store installiert) und stellt erweiterte Debuggen Funktionen für Ihre Chrome App-Entwicklung.

    ![Google Chrome - entpackt Erweiterung laden][17]

3. Wenn die App Chrome ohne Fehler erstellt wurde, wird Ihre Chrome-App angezeigt angezeigt.

    ![Google Chrome - Chrome-App anzeigen][18]

4. Geben Sie die **Projektnummer** , die Sie zuvor von der **Google-Cloud-Konsole** als Absender-ID erhalten haben, und klicken Sie auf **mit GCM registrieren**. Sie müssen die Nachricht finden Sie unter **Registrierung mit GCM erfolgreich war.**

    ![Google Chrome - Chrome App Anpassung][19]

5. Geben Sie Ihren **Benachrichtigung Hubnamen** und die **DefaultListenSharedAccessSignature** , die Sie zuvor auf dem Portal erhalten haben, und klicken Sie auf **mit Azure Benachrichtigung Hub zu registrieren**. Sie müssen die Nachricht finden Sie unter **Benachrichtigung Hub Registrierung erfolgreich!** und die Details der Antwort auf die Registrierung, die die Registrierung Azure Benachrichtigung Hubs enthält-ID.

    ![Geben Sie Google Chrome - Benachrichtigung Hub Details][20]  

##<a name="a-namesendasend-a-notification-to-your-chrome-app"></a><a name="send"></a>Senden einer Benachrichtigung zu Ihrer Anwendung Chrome

Zu Testzwecken schicken wir, dass Pushbenachrichtigungen mithilfe einer .NET Chrome Anwendung console. 

>[AZURE.NOTE] Sie können alle Back-End-über unsere öffentlichen <a href="http://msdn.microsoft.com/library/windowsazure/dn223264.aspx">REST-Schnittstelle</a>Pushbenachrichtigungen mit Benachrichtigung Hubs senden. Schauen Sie sich unsere [Dokumentation Portal](https://azure.microsoft.com/documentation/services/notification-hubs/) Weitere Beispiele für die Plattformen.

1. Wählen Sie in Visual Studio im Menü **Datei** , **neu** und dann auf **Projekt**aus. Klicken Sie unter **Visual c#**klicken Sie auf **Windows** und **Console-Anwendung**, und klicken Sie dann auf **OK**.  Dadurch wird ein neues Projekt der Console-Anwendung erstellt.

2. Klicken Sie im Menü **Extras** auf **Bibliothek Paket-Manager** , und klicken Sie dann **Paket-Manager-Konsole**. Dadurch werden die Paket-Manager-Konsole.

3. Führen Sie im Fenster Konsole den folgenden Befehl aus:

        Install-Package Microsoft.Azure.NotificationHubs

    Dadurch wird einen Verweis auf die Azure Service Bus SDK mit dem <a href="http://nuget.org/packages/  WindowsAzure.ServiceBus/">WindowsAzure.ServiceBus NuGet-Paket</a>hinzugefügt.

4. Open `Program.cs` , und fügen Sie den folgenden `using` Anweisung:

        using Microsoft.Azure.NotificationHubs;

5. In der `Program` Klasse, fügen Sie die folgende Methode:

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            String message = "{\"data\":{\"message\":\"Hello Chrome from Azure Notification Hubs\"}}";
            await hub.SendGcmNativeNotificationAsync(message);
        }

    Vergewissern Sie sich, ersetzen die `<hub name>` Platzhalter mit dem Namen der Benachrichtigung-Hub, die im [Portal](https://portal.azure.com) in der Benachrichtigung Hub Blade angezeigt wird. Ersetzen Sie die Verbindung Zeichenfolge Platzhalter auch mit der Verbindungszeichenfolge aufgerufen `DefaultFullSharedAccessSignature` , die Sie im Abschnitt Konfiguration Hub Benachrichtigung erhalten haben.

    >[AZURE.NOTE] Stellen Sie sicher, dass Sie die Verbindungszeichenfolge mit **Vollzugriff, nicht **Abhören** Access** verwenden. Die Verbindungszeichenfolge Access **Abhören** gewährt keine Berechtigungen, um Pushbenachrichtigungen zu senden.

5. Fügen Sie die folgenden Aufrufe in der `Main` Methode:

         SendNotificationAsync();
         Console.ReadLine();
         
6. Stellen Sie sicher, dass Chrome ausgeführt wird, und führen Sie die Anwendung Console.

7. Die folgende Benachrichtigung sollte popupwarnung auf dem Desktop angezeigt werden.

    ![Google Chrome - Benachrichtigung][13]

8. Sie können auch alle Ihre Benachrichtigungen mithilfe der Fenster Chrome Benachrichtigungen auf der Taskleiste (in Windows) anzeigen wenn Chrome ausgeführt wird.

    ![Google Chrome - Benachrichtigungsliste][14]

>[AZURE.NOTE] Sie benötigen keine der App Chrome ausgeführt haben, oder öffnen im Browser (obwohl Chrome-Browsers selbst ausgeführt werden muss). Sie erhalten auch eine konsolidierte Übersicht über alle Ihre Benachrichtigungen im Fenster Chrome Benachrichtigungen.

## <a name="next-steps"> </a>Nächste Schritte

Weitere Informationen zu Benachrichtigung Hubs in [Benachrichtigung Hubs Übersicht].

Wenn Sie bestimmte Zielpublikum, finden Sie unter des Lernprogramms [Azure Benachrichtigung Hubs benachrichtigen Benutzer] . 

Wenn Sie Ihre Benutzer Zinsen gruppenweise segmentieren möchten, können Sie das Lernprogramm [Azure Benachrichtigung Hubs Neuigkeiten] folgen.

<!-- Images. -->
[1]: ./media/notification-hubs-chrome-get-started/GoogleConsoleCreateProject.PNG
[2]: ./media/notification-hubs-chrome-get-started/GoogleProjectNumber.png
[3]: ./media/notification-hubs-chrome-get-started/EnableGCM.png
[4]: ./media/notification-hubs-chrome-get-started/CreateServerKey.png
[5]: ./media/notification-hubs-chrome-get-started/ServerKey.png
[6]: ./media/notification-hubs-chrome-get-started/CreateNH.png
[7]: ./media/notification-hubs-chrome-get-started/NHNamespace.png
[8]: ./media/notification-hubs-chrome-get-started/NamespaceConfigure.png
[9]: ./media/notification-hubs-chrome-get-started/NHConfigure.png
[10]: ./media/notification-hubs-chrome-get-started/NHConfigureGCM.png
[11]: ./media/notification-hubs-chrome-get-started/NHDashboard.png
[12]: ./media/notification-hubs-chrome-get-started/NHConnString.png
[13]: ./media/notification-hubs-chrome-get-started/ChromeNotification.png
[14]: ./media/notification-hubs-chrome-get-started/ChromeNotificationWindow.png
[15]: ./media/notification-hubs-chrome-get-started/ChromeApp.png
[16]: ./media/notification-hubs-chrome-get-started/ChromeExtensions.png
[17]: ./media/notification-hubs-chrome-get-started/ChromeLoadExtension.png
[18]: ./media/notification-hubs-chrome-get-started/ChromeAppLoaded.png
[19]: ./media/notification-hubs-chrome-get-started/ChromeAppGCM.png
[20]: ./media/notification-hubs-chrome-get-started/ChromeAppNH.png
[21]: ./media/notification-hubs-chrome-get-started/FinalFolderView.png

<!-- URLs. -->
[Chrome App Benachrichtigung Hub (Beispiel)]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToChromeApps
[Google-Cloud-Konsole]: http://cloud.google.com/console
[Azure Classic Portal]: https://manage.windowsazure.com/
[Benachrichtigung Hubs (Übersicht)]: notification-hubs-push-notification-overview.md
[Chrome-Apps (Übersicht)]: https://developer.chrome.com/apps/about_apps
[Chrome App GCM Stichprobe]: https://github.com/GoogleChrome/chrome-app-samples/tree/master/samples/gcm-notifications
[Installable Web Apps]: https://developers.google.com/chrome/apps/docs/
[Chrome-Apps auf Mobile]: https://developer.chrome.com/apps/chrome_apps_on_mobile
[Erstellen der Registrierung NH REST-API]: http://msdn.microsoft.com/library/azure/dn223265.aspx
[kryptomobilität Js Bibliothek]: http://code.google.com/p/crypto-js/
[GCM with Chrome Apps]: https://developer.chrome.com/apps/cloudMessaging
[Google-Cloud-Messaging für Chrome]: https://developer.chrome.com/apps/cloudMessagingV1
[Benutzer benachrichtigen, Azure Benachrichtigung Hubs]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Benachrichtigung Hubs dem neusten Stand Azure]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md

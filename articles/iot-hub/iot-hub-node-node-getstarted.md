<properties
    pageTitle="Azure IoT Hub für erste Schritte Node.js | Microsoft Azure"
    description="Azure IoT Hub mit Node.js erste Schritte Lernprogramm. Verwenden Sie Azure IoT Hub und Node.js mit Microsoft Azure IoT SDKs, um eine Lösung Internet der Dinge implementieren."
    services="iot-hub"
    documentationCenter="nodejs"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="javascript"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/12/2016"
     ms.author="dobett"/>

# <a name="get-started-with-azure-iot-hub-for-nodejs"></a>Erste Schritte mit Azure IoT Hub für Node.js

[AZURE.INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Am Ende dieses Lernprogramms stehen Ihnen drei Node.js Console Applications aus:

* **CreateDeviceIdentity.js**, die einer Identität Gerät und zugehörigen Sicherheitsupdates-Taste, um die Verbindung Ihres Geräts simulierten erstellt.
* **ReadDeviceToCloudMessages.js**, die die von Ihrem Gerät simulierten gesendet werden angezeigt.
* **SimulatedDevice.js**, das eine Verbindung mit Ihrem IoT Hub mit der Identität des Geräts zuvor erstellten und sendet einer Nachricht werden jeder zweiten mithilfe der AMQP-Protokoll.

> [AZURE.NOTE] Im Artikel [IoT Hub SDKs] [ lnk-hub-sdks] enthält Informationen zu den verschiedenen SDKs, die Sie verwenden können, um beide Applikationen für Geräte und Ihre Lösung Back-End ausgeführt zu erstellen.

Damit dieses Lernprogramm abgeschlossen, benötigen Sie Folgendes:

+ Node.js Version 0.10.x oder höher.

+ Ein aktives Azure-Konto. (Wenn Sie kein Konto haben, können Sie ein [kostenloses Konto] erstellen[ lnk-free-trial] in nur ein paar Minuten.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Sie haben Ihre IoT Hub jetzt erstellt. Stehen Ihnen die IoT Hub Hostname und der IoT Hub Verbindungszeichenfolge zurück, die Sie im weiteren Verlauf dieses Lernprogramms ausführen müssen.

## <a name="create-a-device-identity"></a>Erstellen Sie eine Gerät Identität

In diesem Abschnitt erstellen Sie eine Node.js Console-app, die eine Identität Gerät in der Registrierung Identität Ihrer IoT Hub erstellt. Ein Gerät kann keine Verbindung IoT Hub hergestellt, es sei denn, sie einen Eintrag in der Registrierung des Geräts Identität gibt. Finden Sie im Abschnitt **Gerät Identität Registrierung** des [IoT Hub Developer Guide] [ lnk-devguide-identity] Weitere Informationen. Wenn Sie diese Console-Anwendung ausführen, wird eine eindeutige Geräte-ID und Ihr Gerät verwenden können, um sich zu identifizieren, wenn er Gerät-Cloud-Nachrichten an IoT Hub sendet-Taste generiert.

1. Erstellen eines neuen leeren Ordners mit dem Namen **Createdeviceidentity**. Klicken Sie im Ordner **Createdeviceidentity** Erstellen einer package.json-Datei mit dem folgenden Befehl in der Befehlszeile. Übernehmen Sie alle Standardeinstellungen:

    ```
    npm init
    ```

2. Führen Sie in der Befehlszeile in den Ordner **Createdeviceidentity** den folgenden Befehl zum Installieren des **Azure-Iothub** Service SDK-Pakets:

    ```
    npm install azure-iothub --save
    ```

3. Erstellen Sie eine **CreateDeviceIdentity.js** -Datei mit einem Text-Editor, in den Ordner **Createdeviceidentity** aus.

4. Fügen Sie den folgenden `require` -Anweisung am Anfang der Datei **CreateDeviceIdentity.js** :

    ```
    'use strict';
    
    var iothub = require('azure-iothub');
    ```

5. Fügen Sie den folgenden Code zu der Datei **CreateDeviceIdentity.js** , und Ersetzen Sie den Platzhalterwert mit der Verbindungszeichenfolge für den IoT-Hub an, die, den Sie im vorherigen Abschnitt erstellt haben: 

    ```
    var connectionString = '{iothub connection string}';
    
    var registry = iothub.Registry.fromConnectionString(connectionString);
    ```

6. Fügen Sie den folgenden Code ein, um ein Gerätedefinition in der Registrierung Gerät Identität Ihrer IoT-Hub zu erstellen. Dieser Code erstellt ein Gerät aus, wenn die Geräte-Id in der Registrierung nicht vorhanden ist, andernfalls gibt es den Schlüssel dem vorhandenen Gerät:

    ```
    var device = new iothub.Device(null);
    device.deviceId = 'myFirstNodeDevice';
    registry.create(device, function(err, deviceInfo, res) {
      if (err) {
        registry.get(device.deviceId, printDeviceInfo);
      }
      if (deviceInfo) {
        printDeviceInfo(err, deviceInfo, res)
      }
    });

    function printDeviceInfo(err, deviceInfo, res) {
      if (deviceInfo) {
        console.log('Device id: ' + deviceInfo.deviceId);
        console.log('Device key: ' + deviceInfo.authentication.SymmetricKey.primaryKey);
      }
    }
    ```

7. Speichern Sie und schließen Sie die Datei **CreateDeviceIdentity.js** .

8. Führen Sie zum Ausführen der Anwendungs **Createdeviceidentity** an der Eingabeaufforderung den folgenden Befehl in den Ordner Createdeviceidentity ein:

    ```
    node CreateDeviceIdentity.js 
    ```

9. Notieren Sie die **Geräte-Id** und das **Gerät-Taste**. Sie benötigen diese Werte später beim Erstellen einer Anwendungs, die eine Verbindung IoT Hub als Gerät herstellt.

> [AZURE.NOTE] Die IoT Hub Identität Registrierung speichert nur Identitäten Gerät, um den sicheren Zugriff auf den Hub ermöglichen. Es speichert Geräte-IDs und Schlüssel verwenden als Sicherheitsanmeldeinformationen und eine Kennzeichnung aktiviert/deaktiviert, die Sie verwenden können, um Zugriff für ein einzelnes Gerät zu deaktivieren. Wenn die Anwendung benötigt, um andere Geräte-spezifischen Metadaten zu speichern, sollte eine anwendungsspezifische Store verwendet werden. Verweisen auf [IoT Hub Developer Guide] [ lnk-devguide-identity] Weitere Informationen.

## <a name="receive-device-to-cloud-messages"></a>Gerät-Cloud-Nachrichten erhalten

In diesem Abschnitt erstellen Sie eine Node.js Console-app, die Cloud-Gerät Nachrichten aus IoT Hub liest. Ein Hub IoT macht ein [Ereignis Hubs][lnk-event-hubs-overview]-kompatiblen Endpunkt, sodass Sie Gerät-Cloud-Nachrichten lesen können. Um die Dinge einfach zu halten, erstellt in diesem Lernprogramm einen grundlegenden Reader, der für eine Bereitstellung von hohen Durchsatz nicht geeignet ist. Der [Prozess Gerät-Cloud - Nachrichten] [ lnk-process-d2c-tutorial] Lernprogramm erfahren Sie, wie Gerät-Cloud-Nachrichten bei Verarbeitungszeit. Der [Erste Schritte mit Ereignis Hubs] [ lnk-eventhubs-tutorial] Lernprogramm enthält weitere Informationen über das zum Verarbeiten von Nachrichten aus dem Ereignis Hubs und gilt für die Endpunkte IoT Hub Ereignis Hub kompatiblen.

> [AZURE.NOTE] Der Ereignis-Hub kompatiblen Endpunkt zum Lesen von Gerät Cloud-Nachrichten immer verwendet das AMQP-Protokoll.

1. Erstellen eines neuen leeren Ordners mit dem Namen **Readdevicetocloudmessages**. Klicken Sie im Ordner **Readdevicetocloudmessages** Erstellen einer package.json-Datei mit dem folgenden Befehl in der Befehlszeile. Übernehmen Sie alle Standardeinstellungen:

    ```
    npm init
    ```

2. Führen Sie in der Befehlszeile in den Ordner **Readdevicetocloudmessages** den folgenden Befehl zum Installieren des Pakets **Azure-Ereignis-Hubs** aus:

    ```
    npm install azure-event-hubs --save
    ```

3. Erstellen Sie eine **ReadDeviceToCloudMessages.js** -Datei mit einem Text-Editor, in den Ordner **Readdevicetocloudmessages** aus.

4. Fügen Sie den folgenden `require` Anweisungen am Anfang der Datei **ReadDeviceToCloudMessages.js** :

    ```
    'use strict';

    var EventHubClient = require('azure-event-hubs').Client;
    ```

5. Fügen Sie die folgende Variable Deklaration hinzu, und Ersetzen Sie den Platzhalterwert mit der Verbindungszeichenfolge für Ihre IoT Hub:

    ```
    var connectionString = '{iothub connection string}';
    ```

6. Fügen Sie die folgenden beiden Funktionen, die Ausgabe an die Konsole ausdrucken hinzu:

    ```
    var printError = function (err) {
      console.log(err.message);
    };

    var printMessage = function (message) {
      console.log('Message received: ');
      console.log(JSON.stringify(message.body));
      console.log('');
    };
    ```

7. Fügen Sie den folgenden Code ein, um die **EventHubClient**zu erstellen, öffnen Sie die Verbindung zu Ihrem IoT Hub und einen Empfänger für jede Partition erstellen. Diese Anwendung verwendet einen Filter aus, wenn sie einen Empfänger erstellt, sodass der Empfänger nur Nachrichten an IoT Hub gesendet werden liest, nachdem der Empfänger wird automatisch ausgeführt. Dieser Filter eignet sich in einer testumgebung, sodass Sie nur den aktuellen Satz von Nachrichten angezeigt. In einer Umgebung Herstellung Code stellen Sie sicher, dass sie alle Nachrichten - verarbeitet finden Sie unter der [wie IoT Hub Gerät-Cloud - Nachrichten verarbeitet] [ lnk-process-d2c-tutorial] zusammengehörenden Weitere Informationen:

    ```
    var client = EventHubClient.fromConnectionString(connectionString);
    client.open()
        .then(client.getPartitionIds.bind(client))
        .then(function (partitionIds) {
            return partitionIds.map(function (partitionId) {
                return client.createReceiver('$Default', partitionId, { 'startAfterTime' : Date.now()}).then(function(receiver) {
                    console.log('Created partition receiver: ' + partitionId)
                    receiver.on('errorReceived', printError);
                    receiver.on('message', printMessage);
                });
            });
        })
        .catch(printError);
    ```

8. Speichern Sie und schließen Sie die Datei **ReadDeviceToCloudMessages.js** .

## <a name="create-a-simulated-device-app"></a>Erstellen Sie eine app simulierten Gerät

In diesem Abschnitt erstellen Sie eine Node.js Console-app, die ein Gerät simuliert, das Gerät-Cloud-Nachrichten an einen Hub IoT sendet.

1. Erstellen eines neuen leeren Ordners mit dem Namen **Simulateddevice**. Klicken Sie im Ordner **Simulateddevice** Erstellen einer package.json-Datei mit dem folgenden Befehl in der Befehlszeile. Übernehmen Sie alle Standardeinstellungen:

    ```
    npm init
    ```

2. Führen Sie an der Eingabeaufforderung in den Ordner **Simulateddevice** den folgenden Befehl zum Installieren der **Azure-Iot-Gerät** Gerät SDK-Paket und **Azure-Iot-Gerät-Amqp** -Paket aus:

    ```
    npm install azure-iot-device azure-iot-device-amqp --save
    ```

3. Erstellen Sie eine neue **SimulatedDevice.js** -Datei mit einem Text-Editor, in den Ordner **Simulateddevice** .

4. Fügen Sie den folgenden `require` Anweisungen am Anfang der Datei **SimulatedDevice.js** :

    ```
    'use strict';

    var clientFromConnectionString = require('azure-iot-device-amqp').clientFromConnectionString;
    var Message = require('azure-iot-device').Message;
    ```

5. Fügen Sie eine Variable **ConnectionString** und verwenden Sie, um ein Gerät Client erstellen. **{Youriothostname}** ersetzen durch den Namen des IoT-Hub Sie im Abschnitt *Erstellen einer IoT Hub* erstellt haben. Ersetzen Sie **{Yourdevicekey}** , mit dem Gerät Schlüsselwert, den Sie im Abschnitt *Erstellen einer Identität Gerät* generiert:

    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myFirstNodeDevice;SharedAccessKey={yourdevicekey}';
    
    var client = clientFromConnectionString(connectionString);
    ```

6. Fügen Sie die folgende Funktion zum Anzeigen der Ausgabe der Anwendung:

    ```
    function printResultFor(op) {
      return function printResult(err, res) {
        if (err) console.log(op + ' error: ' + err.toString());
        if (res) console.log(op + ' status: ' + res.constructor.name);
      };
    }
    ```

7. Erstellen Sie einen Rückruf, und verwenden Sie die **SetInterval** -Funktion, um eine neue Nachricht an Ihre IoT Verteiler pro Sekunde senden:

    ```
    var connectCallback = function (err) {
      if (err) {
        console.log('Could not connect: ' + err);
      } else {
        console.log('Client connected');

        // Create a message and send it to the IoT Hub every second
        setInterval(function(){
            var windSpeed = 10 + (Math.random() * 4);
            var data = JSON.stringify({ deviceId: 'myFirstNodeDevice', windSpeed: windSpeed });
            var message = new Message(data);
            console.log("Sending message: " + message.getData());
            client.sendEvent(message, printResultFor('send'));
        }, 1000);
      }
    };
    ```

8. Öffnen Sie die Verbindung zu Ihrem IoT Hub und starten Sie senden von Nachrichten zu:

    ```
    client.open(connectCallback);
    ```

9. Speichern Sie und schließen Sie die Datei **SimulatedDevice.js** .

> [AZURE.NOTE] Zur Vereinfachung, implementiert keine "Wiederholen" Richtlinie in diesem Lernprogramm. Herstellung Code, sollten Sie "Wiederholen" Richtlinien (beispielsweise einer exponentiellen Backoff), wie im MSDN-Artikel [Behandeln von vorübergehenden Fehlerstrukturanalyse]vorgeschlagen implementieren[lnk-transient-faults].


## <a name="run-the-applications"></a>Führen Sie die Anwendung

Sie können nun zur Ausführung der Anwendung.

1. In einer Befehlszeile im Ordner **Readdevicetocloudmessages** zum Überwachen Ihrer IoT Hub beginnen den folgenden Befehl ausführen:

    ```
    node ReadDeviceToCloudMessages.js 
    ```

    ![Node.js IoT Hub-Clientanwendung zu überwachen Gerät-Cloud-Nachrichten][7]

2. Führen Sie in einer Befehlszeile in den Ordner **Simulateddevice** den folgenden Befehl zu beginnen, werden Daten an Ihre IoT Verteiler senden:

    ```
    node SimulatedDevice.js
    ```

    ![Node.js IoT Hub Gerät Clientanwendung Gerät-Cloud-Nachrichten senden][8]

3. Die Kachel **Verwendung** im [Portal Azure] [ lnk-portal] zeigt die Anzahl der Nachrichten, die an den Hub gesendet:

    ![Azure Portals Verwendung Kachel mit Anzahl der gesendeten Nachrichten an IoT Verteiler][43]

## <a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm so konfiguriert, dass einen neuen IoT Hub Azure-Portal, und klicken Sie dann in der Hub Identität Registrierung erstellt eine Gerät Identität. Sie verwendet diese Identität Gerät, um die app simulierten Gerät Gerät-Cloud-Nachrichten an den Hub senden aktivieren. Sie auch eine app erstellt haben, die die vom Hub empfangenen Nachrichten angezeigt werden. 

Erste Schritte mit IoT Hub fortsetzen und anderen Szenarien IoT untersuchen, finden Sie unter:

- [Verbinden von Ihrem Gerät][lnk-connect-device]
- [Erste Schritte mit Gerät-Verwaltung][lnk-device-management]
- [Erste Schritte mit dem IoT Gateway SDK][lnk-gateway-SDK]

Erfahren Sie, wie Ihre Lösung IoT erweitern und Gerät-Cloud-Nachrichten bei verarbeiten, finden Sie unter den [Prozess Gerät-Cloud - Nachrichten] [ lnk-process-d2c-tutorial] Lernprogramm.

<!-- Images. -->
[7]: ./media/iot-hub-node-node-getstarted/runapp1.png
[8]: ./media/iot-hub-node-node-getstarted/runapp2.png
[43]: ./media/iot-hub-csharp-csharp-getstarted/usage.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/

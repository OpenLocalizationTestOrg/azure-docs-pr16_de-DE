<properties
    pageTitle="Senden von Nachrichten von Cloud-zu-Gerät mit IoT Hub | Microsoft Azure"
    description="Führen Sie dieses Lernprogramm erfahren Sie, wie Sie mit Azure IoT Hub mit Java Cloud-zu-Gerät-Nachrichten senden."
    services="iot-hub"
    documentationCenter="nodejs"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="javascript"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/23/2016"
     ms.author="dobett"/>

# <a name="tutorial-how-to-send-cloud-to-device-messages-with-iot-hub-and-nodejs"></a>Lernprogramm: Zum Senden von Nachrichten von Cloud-zu-Gerät mit IoT Hub und Node.js

[AZURE.INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a>Einführung

Azure IoT-Hub ist ein vollständig verwalteter Dienst, der hilft aktivieren zuverlässige und sichere bidirektionale Kommunikation zwischen mehreren Millionen IoT Geräte und eine Anwendung wieder zu beenden. Das [Erste Schritte mit IoT Hub] -Lernprogramm wird gezeigt, wie einen IoT Hub erstellen, eine Gerät Identität darin bereitstellen und Fehlercode ein simuliertes Gerät, das Gerät-Cloud-Nachrichten sendet.

In diesem Lernprogramm basiert auf [Erste Schritte mit IoT Hub]. Es wird gezeigt, wie an:

- Ihrer Anwendung Cloud Back-End senden Sie-Cloud-zu-Gerät-Nachrichten zu einem einzelnen Gerät über IoT Hub.
- Empfangen Sie Cloud-zu-Gerät auf einem Gerät.
- Aus der Cloud Anwendung back-End, Übermittlung Bestätigung (*Feedback*) für von IoT-Hub auf einem Gerät gesendeten Nachrichten anfordern.

Finden Sie weitere Informationen in der Cloud-zu-Gerät-Nachrichten, die [IoT Hub Developer Guide][IoT Hub Developer Guide - C2D].

Am Ende dieses Lernprogramms führen Sie zwei Node.js Console Applications aus:

* **SimulatedDevice**, eine geänderte Version der app in [den ersten Schritten mit IoT Hub], die eine Verbindung mit Ihrem IoT Hub und empfängt Nachrichten, die Cloud-zu-Gerät erstellt.
* **SendCloudToDeviceMessage**, der eine Nachricht Cloud-zu-Gerät mit dem simulierten Gerät über IoT Hub sendet, und klicken Sie dann erhält deren Übermittlung Bestätigung.

> [AZURE.NOTE] IoT Hub enthält SDK-Unterstützung für zahlreiche Plattformen und Sprachen (einschließlich C, Java und Javascript) bis Azure IoT Geräte-SDKs. Eine schrittweise Anleitung zum Herstellen einer Verbindung Ihr Gerät in diesem Lernprogramm den Code und im Allgemeinen Azure IoT Hub finden Sie unter der [Azure IoT Developer Center].

Damit dieses Lernprogramm abgeschlossen, benötigen Sie Folgendes:

+ Node.js Version 0.10.x oder höher.

+ Ein aktives Azure-Konto. (Wenn Sie kein Konto haben, können Sie ein [kostenloses Konto] erstellen[ lnk-free-trial] in nur ein paar Minuten.)

## <a name="receive-messages-on-the-simulated-device"></a>Empfangen von Nachrichten auf dem simulierten Gerät

In diesem Abschnitt, ändern Sie die simulierten Gerät Anwendung Sie in der Cloud-zu-Gerät Nachrichten vom Hub IoT erhalten [Erste Schritte mit IoT Hub] erstellt haben.

1. Mit einem Text-Editor, öffnen Sie die Datei SimulatedDevice.js.

2. Ändern Sie die Funktion **ConnectCallback** zum Behandeln von IoT Hub gesendeten Nachrichten. In diesem Beispiel ruft das Gerät immer die **vollständige** Funktion um IoT Hub zu benachrichtigen, dass die Nachricht verarbeitet wurde. Die neue Version der Funktion **ConnectCallback** sieht wie folgt aus:

    ```
    var connectCallback = function (err) {
      if (err) {
        console.log('Could not connect: ' + err);
      } else {
        console.log('Client connected');
        client.on('message', function (msg) {
          console.log('Id: ' + msg.messageId + ' Body: ' + msg.data);
          client.complete(msg, printResultFor('completed'));
        });
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

    > [AZURE.NOTE] Wenn Sie HTTP statt MQTT oder AMQP als den Transport verwenden, überprüft die **DeviceClient** -Instanz für Nachrichten selten IoT-Hub (weniger als 25 Minuten). Weitere Informationen über die Unterschiede zwischen MQTT, AMQP und HTTP-Support und IoT Hub begrenzungsebene finden Sie unter der [IoT Hub Developer Guide][IoT Hub Developer Guide - C2D].

## <a name="send-a-cloud-to-device-message"></a>Senden einer Nachricht Cloud-zu-Gerät

In diesem Abschnitt erstellen Sie eine Node.js Console-app, die Cloud-zu-Gerät Nachrichten bei der app simulierten Gerät sendet. Sie benötigen das Gerät-Id des Geräts, die Sie in die [Erste Schritte mit IoT Hub] Lernprogramm hinzugefügt. Sie benötigen ferner die Verbindungszeichenfolge für Ihre IoT-Hub an, den Sie in der [Azure-Portal]finden können.

1. Erstellen Sie einen leeren Ordner namens **Sendcloudtodevicemessage**. Klicken Sie im Ordner **Sendcloudtodevicemessage** Erstellen einer package.json-Datei mit dem folgenden Befehl in der Befehlszeile. Übernehmen Sie alle Standardeinstellungen:

    ```
    npm init
    ```

2. Führen Sie in der Befehlszeile in den Ordner **Sendcloudtodevicemessage** den folgenden Befehl zum Installieren des Pakets **Azure-Iothub** aus:

    ```
    npm install azure-iothub --save
    ```

3. Erstellen Sie eine neue **SendCloudToDeviceMessage.js** -Datei mit einem Text-Editor, in den Ordner **Sendcloudtodevicemessage** .

4. Fügen Sie den folgenden `require` Anweisungen am Anfang der Datei **SendCloudToDeviceMessage.js** :

    ```
    'use strict';
    
    var Client = require('azure-iothub').Client;
    var Message = require('azure-iot-common').Message;
    ```

5. Fügen Sie den folgenden Code in **SendCloudToDeviceMessage.js** -Datei ein. Ersetzen Sie den Wert der Verbindungszeichenfolge Platzhalter mit der Verbindungszeichenfolge für den IoT-Hub an, die, den Sie in das [Erste Schritte mit IoT Hub] Lernprogramm erstellt haben. Ersetzen Sie den Platzhalter der Ziel-Gerät mit der Geräte-Id des Geräts, die Sie in die [Erste Schritte mit IoT Hub] Lernprogramm hinzugefügt:

    ```
    var connectionString = '{iot hub connection string}';
    var targetDevice = '{device id}';

    var serviceClient = Client.fromConnectionString(connectionString);
    ```

6. Fügen Sie die folgende Funktion zum Drucken der Ergebnisse in der Konsole an:

    ```
    function printResultFor(op) {
      return function printResult(err, res) {
        if (err) console.log(op + ' error: ' + err.toString());
        if (res) console.log(op + ' status: ' + res.constructor.name);
      };
    }
    ```

7. Fügen Sie die folgende Funktion zum Drucken der Übermittlung Feedback Nachrichten an die Konsole hinzu:

    ```
    function receiveFeedback(err, receiver){
      receiver.on('message', function (msg) {
        console.log('Feedback message:')
        console.log(msg.getData().toString('utf-8'));
      });
    }
    ```

8. Fügen Sie den folgenden Code zum Senden einer Nachricht an Ihr Gerät und die Feedback-Meldung zu behandeln, wenn das Gerät erkennt die Cloud-zu-Gerät-Nachricht an:

    ```
    serviceClient.open(function (err) {
      if (err) {
        console.error('Could not connect: ' + err.message);
      } else {
        console.log('Service client connected');
        serviceClient.getFeedbackReceiver(receiveFeedback);
        var message = new Message('Cloud to device message.');
        message.ack = 'full';
        message.messageId = "My Message ID";
        console.log('Sending message: ' + message.getData());
        serviceClient.send(targetDevice, message, printResultFor('send'));
      }
    });
    ```

7. Speichern Sie und schließen Sie die Datei **SendCloudToDeviceMessage.js** .

## <a name="run-the-applications"></a>Führen Sie die Anwendung

Sie können nun zur Ausführung der Anwendung.

1. Führen Sie in der Befehlszeile in den Ordner **Simulateddevice** den folgenden Befehl werden an IoT Verteiler senden und Cloud-zu-Gerät Nachrichten empfangen ein:

    ```
    node SimulatedDevice.js 
    ```

    ![Führen Sie die app simulierten Gerät][img-simulated-device]

2. Führen Sie in einer Befehlszeile in den Ordner **Sendcloudtodevicemessage** zum Senden einer Nachricht Cloud-zu-Gerät und warten, bis die Bestätigung Feedback den folgenden Befehl aus:

    ```
    node SendCloudToDeviceMessage.js 
    ```

    ![Führen Sie die app, um den Befehl c2d zu senden.][img-send-command]

    > [AZURE.NOTE] Aus Gründen der Vereinfachung implementiert dieses Lernprogramms keine Richtlinie "Wiederholen". Herstellung Code sollten Sie "Wiederholen" Richtlinien zur Verfügung (z. B. exponentiellen Backoff), wie im MSDN-Artikel [Behandeln von vorübergehenden Fehlerstrukturanalyse]vorgeschlagen implementieren.

## <a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm haben Sie wie Cloud-zu-Gerät-Nachrichten senden und empfangen. 

Beispiele für umfassende End-to-End-Lösung, die IoT Hub verwenden, finden Sie unter [Azure IoT Suite].

Wenn Sie weitere Informationen zur Entwicklung von Lösungen mit IoT Hub finden Sie im [IoT Hub Developer Guide].

<!-- Images -->
[img-simulated-device]: media/iot-hub-node-node-c2d/receivec2d.png
[img-send-command]:  media/iot-hub-node-node-c2d/sendc2d.png

<!-- Links -->

[Erste Schritte mit IoT-Hub]: iot-hub-node-node-getstarted.md
[IoT Hub Developer Guide - C2D]: iot-hub-devguide-messaging.md
[IoT Hub Developer Guide]: iot-hub-devguide.md
[Azure IoT-Entwicklercenter]: http://www.azure.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[Vorübergehende Fehlerbehandlung]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure-portal]: https://portal.azure.com
[Azure IoT Suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
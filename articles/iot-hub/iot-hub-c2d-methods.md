<properties
 pageTitle="Verwenden der direkte Methoden | Microsoft Azure"
 description="In diesem Lernprogramm wird gezeigt, wie direkte Methoden verwenden"
 services="iot-hub"
 documentationCenter=""
 authors="nberdy"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/05/2016"
 ms.author="nberdy"/>

# <a name="tutorial-use-direct-methods"></a>Lernprogramm: Verwenden Sie direkte Methoden

## <a name="introduction"></a>Einführung

Azure IoT-Hub ist ein vollständig verwalteter Dienst, der zuverlässigen ermöglicht, und sichere bidirektionale Kommunikation zwischen mehreren Millionen IoT Geräte und eine Anwendung back-End. Vorherigen Lernprogramme ([Erste Schritte mit IoT Hub] und [Senden der Cloud-zu-Gerät Nachrichten mit IoT Hub]) veranschaulichen Gerät Cloud und Cloud-zu-Gerät Textnachrichten Grundfunktionen des IoT-Hub an. IoT Hub gibt Ihnen außerdem die Möglichkeit, nicht beständigen Methoden für Geräte aus der Cloud aufzurufen. Methoden darstellen eine Besprechungsanfrage Antworten Interaktion mit einem Gerät zu einem Anruf HTTP ähnliche darin, dass er erfolgreich ist oder nicht sofort (nach einem Benutzer angegebenen Timeout) informieren den Benutzer den Status des Anrufs kennen. [Um eine direkte Methode auf einem Gerät aufzurufen] [ lnk-devguide-methods] Methoden in ausführlicher beschrieben und enthält einige Tipps zur Verwendung von Methoden im Vergleich zu Nachrichten Cloud-zu-Gerät.

In diesem Lernprogramm erfahren Sie, wie in:

- Verwenden des Azure-Portals erstellen einen IoT Hub, und erstellen eine Gerät Identität Ihrer IoT-Hub an.
- Erstellen Sie ein simuliertes Gerät, das eine direkte Methode hat, die von der Cloud aufgerufen werden können.
- Erstellen Sie eine Console-Anwendung, die auf dem simulierten Gerät über Ihre IoT-Hub eine direkte Methode ruft ein.

> [AZURE.NOTE] Zu diesem Zeitpunkt für direkte Methoden nur von Geräten, die Verbindung IoT Hub mit dem MQTT-Protokoll zugegriffen werden. Lizenzinformationen finden Sie die [MQTT unterstützen] [ lnk-devguide-mqtt] Artikel Anweisungen zum Konvertieren von vorhandenen Gerät app MQTT verwenden.

Am Ende dieses Lernprogramms haben Sie zwei Node.js Console Applications aus:

* **CallMethodOnDevice.js**, ruft eine Methode auf dem simulierten Gerät und die Antwort angezeigt.
* **SimulatedDevice.js**, die eine Verbindung mit Ihrem IoT Hub mit der Identität des Geräts zuvor erstellten und reagiert auf die Methode, die von der Cloud aufgerufen.

> [AZURE.NOTE] Im Artikel [IoT Hub SDKs] [ lnk-hub-sdks] enthält Informationen zu den verschiedenen SDKs, die Sie verwenden können, um beide Applikationen für Geräte und Ihre Lösung Back-End ausgeführt zu erstellen.

Damit dieses Lernprogramm abgeschlossen, benötigen Sie Folgendes:

+ Node.js Version 0.10.x oder höher.

+ Ein aktives Azure-Konto. (Wenn Sie kein Konto haben, können Sie ein [kostenloses Konto] erstellen[ lnk-free-trial] in nur ein paar Minuten.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-a-simulated-device-app"></a>Erstellen Sie eine app simulierten Gerät

In diesem Abschnitt erstellen Sie eine Node.js Console-app, die an eine Methode namens durch die Cloud reagiert.

1. Erstellen eines neuen leeren Ordners mit dem Namen **Simulateddevice**. Klicken Sie im Ordner **Simulateddevice** Erstellen einer package.json-Datei mit dem folgenden Befehl in der Befehlszeile. Übernehmen Sie alle Standardeinstellungen:

    ```
    npm init
    ```

2. Führen Sie an der Eingabeaufforderung in den Ordner **Simulateddevice** den folgenden Befehl zum Installieren der **Azure-Iot-Gerät** Gerät SDK-Paket und **Azure-Iot-Gerät-Mqtt** -Paket aus:

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. Erstellen Sie eine neue **SimulatedDevice.js** -Datei mit einem Text-Editor, in den Ordner **Simulateddevice** .

4. Fügen Sie den folgenden `require` Anweisungen am Anfang der Datei **SimulatedDevice.js** :

    ```
    'use strict';

    var Mqtt = require('azure-iot-device-mqtt').Mqtt;
    var DeviceClient = require('azure-iot-device').Client;
    ```

5. Fügen Sie eine Variable **ConnectionString** und verwenden Sie, um ein Gerät Client erstellen. Ersetzen Sie **{Gerät Verbindungszeichenfolge}** durch die Verbindungszeichenfolge, die Sie im Abschnitt *Erstellen einer Identität Gerät* generiert:

    ```
    var connectionString = '{device connection string}';
    var client = DeviceClient.fromConnectionString(connectionString, Mqtt);
    ```

6. Fügen Sie die folgende Funktion zum Implementieren der Methode auf dem Gerät:

    ```
    function onWriteLine(request, response) {
        console.log(request.payload);

        response.send(200, 'Input was written to log.', function(err) {
            if(err) {
                console.error('An error ocurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.' );
            }
        });
    }
    ```

7. Öffnen die Verbindung IoT Hub und Start Initialisierung der Zuhörer Methode:

    ```
    client.open(function(err) {
        if (err) {
            console.error('could not open IotHub client');
        }  else {
            console.log('client opened');
            client.onDeviceMethod('writeLine', onWriteLine);
        }
    });
    ```

8. Speichern Sie und schließen Sie die Datei **SimulatedDevice.js** .

> [AZURE.NOTE] Zur Vereinfachung, implementiert keine "Wiederholen" Richtlinie in diesem Lernprogramm. Herstellung Code, sollten Sie "Wiederholen" Richtlinien (z. B. Verbindungsversuch), wie im MSDN-Artikel [Behandeln von vorübergehenden Fehlerstrukturanalyse]vorgeschlagen implementieren[lnk-transient-faults].

## <a name="call-a-method-on-a-device"></a>Rufen Sie eine Methode auf einem Gerät

In diesem Abschnitt erstellen Sie eine Node.js Console-app, die Ruft eine Methode auf dem simulierten Gerät, und klicken Sie dann die Antwort angezeigt.

1. Erstellen eines neuen leeren Ordners mit dem Namen **Callmethodondevice**. Klicken Sie im Ordner **Callmethodondevice** Erstellen einer package.json-Datei mit dem folgenden Befehl in der Befehlszeile. Übernehmen Sie alle Standardeinstellungen:

    ```
    npm init
    ```

2. Führen Sie in der Befehlszeile in den Ordner **Callmethodondevice** den folgenden Befehl zum Installieren des Pakets **Azure-Iothub** aus:

    ```
    npm install azure-iothub@dtpreview --save
    ```

3. Erstellen Sie eine **CallMethodOnDevice.js** -Datei mit einem Text-Editor, in den Ordner **Callmethodondevice** aus.

4. Fügen Sie den folgenden `require` Anweisungen am Anfang der Datei **CallMethodOnDevice.js** :

    ```
    'use strict';

    var Client = require('azure-iothub').Client;
    ```

5. Fügen Sie die folgende Variable Deklaration hinzu, und Ersetzen Sie den Platzhalterwert mit der Verbindungszeichenfolge für Ihre IoT Hub:

    ```
    var connectionString = '{iothub connection string}';
    var methodName = 'writeLine';
    var deviceId = 'myDeviceId';
    ```

6. Erstellen Sie den Client, um die Verbindung zu Ihrem IoT-Hub zu öffnen.

    ```
    var client = Client.fromConnectionString(connectionString);
    ```
    
7. Fügen Sie die folgende Funktion zum Aufrufen der Methode Gerät und Drucken die Antwort des Geräts auf der Konsole an:

    ```
    var methodParams = {
        methodName: methodName,
        payload: 'a line to be written',
        timeoutInSeconds: 30
    };

    client.invokeDeviceMethod(deviceId, methodParams, function (err, result) {
        if (err) {
            console.error('Failed to invoke method \'' + methodName + '\': ' + err.message);
        } else {
            console.log(methodName + ' on ' + deviceId + ':');
            console.log(JSON.stringify(result, null, 2));
        }
    });
    ```

7. Speichern Sie und schließen Sie die Datei **CallMethodOnDevice.js** .

## <a name="run-the-applications"></a>Führen Sie die Anwendung

Sie können nun zur Ausführung der Anwendung.

1. Führen Sie in einer Befehlszeile in den Ordner **Simulateddevice** den folgenden Befehl zum Starten der Überwachung Aufrufe aus Ihrem IoT Hub-Methode:

    ```
    node SimulatedDevice.js
    ```

    ![][7]
    
2. In einer Befehlszeile im Ordner **Callmethodondevice** zum Überwachen Ihrer IoT Hub beginnen den folgenden Befehl ausführen:

    ```
    node CallMethodOnDevice.js 
    ```

    ![][8]
    
3. Das Gerät reagiert an die Methode, mit der die Nachricht und die Anwendung, die die Anzeige der Methode der Antwort vom Gerät aufgerufen Drucken werden angezeigt:

    ![][9]
    
## <a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm so konfiguriert, dass einen neuen IoT Hub Azure-Portal, und klicken Sie dann in der Hub Identität Registrierung erstellt eine Gerät Identität. Sie verwendet diese Identität Gerät, um die app simulierten Gerät reagieren auf Methoden aufgerufen, indem der Cloud zu aktivieren. Sie auch eine app erstellt haben, durch die Methoden auf dem Gerät, und zeigt die Antwort vom Gerät. 

Erste Schritte mit IoT Hub fortsetzen und anderen Szenarien IoT untersuchen, finden Sie unter:

- [Erste Schritte mit IoT-Hub]
- [Planen von Projekten auf mehreren Geräten][lnk-devguide-jobs]

Erfahren Sie, wie Sie Ihre IoT erweitern Lösung und Zeitplan Methode auf mehreren Geräten Ruft, finden Sie im [Zeitplan und die übertragenen Aufträge] [ lnk-tutorial-jobs] Lernprogramm.

<!-- Images. -->
[7]: ./media/iot-hub-c2d-methods/run-simulated-device.png
[8]: ./media/iot-hub-c2d-methods/run-callmethodondevice.png
[9]: ./media/iot-hub-c2d-methods/methods-output.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-tutorial-jobs]: iot-hub-schedule-jobs.md
[lnk-devguide-methods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[Senden Sie Cloud-zu-Gerät Nachrichten mit IoT Hub]: iot-hub-csharp-csharp-c2d.md
[Process Device-to-Cloud messages]: iot-hub-csharp-csharp-process-d2c.md
[Erste Schritte mit IoT-Hub]: iot-hub-node-node-getstarted.md

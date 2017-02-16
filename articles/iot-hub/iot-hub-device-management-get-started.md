<properties
 pageTitle="Erste Schritte mit Gerätemanagement | Microsoft Azure"
 description="In diesem Lernprogramm wird gezeigt, wie Sie erste Schritte mit auf Azure IoT Hub Gerät-Verwaltung"
 services="iot-hub"
 documentationCenter=".net"
 authors="juanjperez"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="juanpere"/>

# <a name="tutorial-get-started-with-device-management-preview"></a>Lernprogramm: Erste Schritte mit Gerätemanagement (Preview)

## <a name="introduction"></a>Einführung
IoT Cloud Applikationen können Primitives Azure IoT Hub, nämlich das Gerät zwei und direkten Methoden Remote starten und überwachen Gerät Management-Aktionen auf Geräten.  Dieser Artikel enthält Leitfäden und Code für die Anwendung Cloud IoT ein Gerät Funktionsweise und um initiieren und Überwachen von einem entfernten Gerät Neustart IoT-Hub verwenden.

Verwenden Sie zum Starten und überwachen Gerät Management-Aktionen auf Ihren Geräten aus einer cloudbasierten, Back-End-app, Azure IoT Hub Primitives wie [Gerät und] [ lnk-devtwin] und [Cloud-zu-Gerät (C2D) Methoden][lnk-c2dmethod]. In diesem Lernprogramm erfahren Sie wie einer Back-End-app und einem Gerät zusammenarbeiten können Sie aktivieren initiieren und überwachen remote Gerät einen Neustart von IoT-Hub an.

Sie verwenden eine Methode C2D Gerät Management-Aktionen (z. B. Neustart, Factory zurücksetzen und Firmwareupdate) aus einer Back-End-app in der Cloud einleiten. Das Gerät ist verantwortlich für:

- Behandeln von Methode Anfrage von IoT Hub gesendet.
- Initiieren der entsprechenden Gerät bestimmte Aktion auf dem Gerät.
- Bereitstellen von statusaktualisierungen durch das Gerät zwei gemeldet Eigenschaften an IoT Verteiler.

Eine Back-End-app können in der Cloud Gerät und Abfragen zum Bericht über den Fortschritt Ihrer Gerät Management Aktionen ausführen.

In diesem Lernprogramm erfahren Sie, wie in:

- Verwenden des Azure-Portals Erstellen einer IoT Hub, und erstellen eine Gerät Identität Ihrer IoT-Hub an.
- Erstellen Sie ein simuliertes Gerät, das eine direkte Methode verfügt über dem Neustart können die Cloud aufgerufen werden können.
- Erstellen Sie eine Console-Anwendung, die die direkte Methode Neustart auf dem simulierten Gerät über Ihre IoT Hub Anrufe an.

Am Ende dieses Lernprogramms haben Sie zwei Node.js Console Applications aus:

**dmpatterns_getstarted_device.js**, die an Ihre IoT Verteiler mit der Identität des Geräts zuvor erstellte Verbindung herstellt, empfängt eine direkte Neustart-Methode, simuliert einen physischen Neustart und die Uhrzeit für den letzten Neustart-Berichte.

**dmpatterns_getstarted_service.js**, welche Ruft eine direkte Methode auf dem simulierten Gerät, zeigt die Antwort und zeigt die aktualisierten Gerät und gemeldet Eigenschaften an.

Damit dieses Lernprogramm abgeschlossen, benötigen Sie Folgendes:

Node.js Version 0.12.x oder höher <br/>  [Vorbereiten Ihrer Entwicklungsumgebung] [ lnk-dev-setup] beschrieben, wie Sie in diesem Lernprogramm Node.js unter Windows oder Linux zu installieren.

Ein aktives Azure-Konto. (Wenn Sie kein Konto haben, können Sie ein [kostenloses Konto] erstellen[ lnk-free-trial] in nur ein paar Minuten.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-a-simulated-device-app"></a>Erstellen Sie eine app simulierten Gerät

In diesem Abschnitt erstellen Sie eine Node.js Console-app, die an eine direkte Methode, die von der Cloud, die einen simuliertes Gerät Neustart auslöst aufgerufen, reagiert und verwendet das Gerät zwei gemeldet Eigenschaften Gerät und Abfragen zu identifizieren, Geräte und beim letzten Neustart aktivieren.

1. Erstellen eines neuen leeren Ordners mit dem Namen **Manageddevice**.  Klicken Sie im Ordner **Manageddevice** Erstellen einer package.json-Datei mit dem folgenden Befehl in der Befehlszeile.  Übernehmen Sie alle Standardeinstellungen:

    ```
    npm init
    ```
    
2. Bei der Eingabeaufforderung in den Ordner **Manageddevice** , führen Sie den folgenden Befehl zum Installieren der **azure-iot-device@dtpreview** Gerät SDK-Paket und **azure-iot-device-mqtt@dtpreview** Paket:

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. Erstellen Sie eine neue **dmpatterns_getstarted_device.js** -Datei mit einem Text-Editor, in den Ordner **Manageddevice** .

4. Fügen Sie die folgenden "Anweisungen am Anfang der Datei **dmpatterns_getstarted_device.js** erforderlich" hinzu:

    ```
    'use strict';

    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```

5. Fügen Sie eine Variable **ConnectionString** und verwenden Sie, um ein Gerät Client erstellen.  Ersetzen Sie die Verbindungszeichenfolge mit Ihrem Gerät Verbindungszeichenfolge aus.  

    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myDeviceId;SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

6. Fügen Sie die folgende Funktion zum Implementieren der direkten Methode auf dem Gerät

    ```
    var onReboot = function(request, response) {
        
        // Respond the cloud app for the direct method
        response.send(200, 'Reboot started', function(err) {
            if (!err) {
                console.error('An error occured when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.');
            }
        });
        
        // Report the reboot before the physical restart
        var date = new Date();
        var patch = {
            iothubDM : {
                reboot : {
                    lastReboot : date.toISOString(),
                }
            }
        };

        // Get device Twin
        client.getTwin(function(err, twin) {
            if (err) {
                console.error('could not get twin');
            } else {
                console.log('twin acquired');
                twin.properties.reported.update(patch, function(err) {
                    if (err) throw err;
                    console.log('Device reboot twin state reported')
                });  
            }
        });
        
        // Add your device's reboot API for physical restart.
        console.log('Rebooting!');
    };
    ```

7. Öffnen Sie die Verbindung zu Ihrem IoT Hub, und starten Sie die direkte Methode Zuhörer:

    ```
    client.open(function(err) {
        if (err) {
            console.error('Could not open IotHub client');
        }  else {
            console.log('Client opened.  Waiting for reboot method.');
            client.onDeviceMethod('reboot', onReboot);
        }
    });
    ```
    
8. Speichern Sie und schließen Sie die Datei **dmpatterns_getstarted_device.js** .

 [AZURE.NOTE] Zur Vereinfachung, implementiert keine "Wiederholen" Richtlinie in diesem Lernprogramm. Herstellung Code, sollten Sie "Wiederholen" Richtlinien (beispielsweise einer exponentiellen Backoff), wie im MSDN-Artikel [Behandeln von vorübergehenden Fehlerstrukturanalyse]vorgeschlagen implementieren[lnk-transient-faults].

## <a name="trigger-a-remote-reboot-on-the-device-using-a-direct-method"></a>Lösen Sie einen remote Neustart auf dem Gerät mithilfe einer direkten Methode aus 

In diesem Abschnitt erstellen Sie eine Node.js Console-app, die einen remote-Neustart auf einem Gerät mit einer direkten Methode initiiert und Gerät und Abfragen verwendet, um die Uhrzeit der letzten Neustart für das Gerät zu finden.

1. Erstellen eines neuen leeren Ordners mit dem Namen **Triggerrebootondevice**.  Klicken Sie im Ordner **Triggerrebootondevice** Erstellen einer package.json-Datei mit dem folgenden Befehl in der Befehlszeile.  Übernehmen Sie alle Standardeinstellungen:

    ```
    npm init
    ```
    
2. Bei der Eingabeaufforderung in den Ordner **Triggerrebootondevice** , führen Sie den folgenden Befehl zum Installieren der **azure-iothub@dtpreview** Gerät SDK-Paket und **azure-iot-device-mqtt@dtpreview** Paket:

    ```
    npm install azure-iothub@dtpreview --save
    ```
    
3. Erstellen Sie eine neue **dmpatterns_getstarted_service.js** -Datei mit einem Text-Editor, in den Ordner **Triggerrebootondevice** .

4. Fügen Sie die folgenden "Anweisungen am Anfang der Datei **dmpatterns_getstarted_service.js** erforderlich" hinzu:

    ```
    'use strict';

    var Registry = require('azure-iothub').Registry;
    var Client = require('azure-iothub').Client;
    ```

5. Fügen Sie die folgenden Variablen Deklarationen, und Ersetzen Sie den Platzhalterwerte:

    ```
    var connectionString = '{iothubconnectionstring}';
    var registry = Registry.fromConnectionString(connectionString);
    var client = Client.fromConnectionString(connectionString);
    var deviceToReboot = 'myDeviceId';
    ```
    
6. Fügen Sie die folgende Funktion zum Aufrufen der Gerät-Methode, um das Gerät neu zu starten:

    ```
    var startRebootDevice = function(twin) {

        var methodName = "reboot";
        
        var methodParams = {
            methodName: methodName,
            payload: null,
            timeoutInSeconds: 30
        };
        
        client.invokeDeviceMethod(deviceToReboot, methodParams, function(err, result) {
            if (err) { 
                console.error("Direct method error: "+err.message);
            } else {
                console.log("Successfully invoked the device to reboot.");  
            }
        });
    };
    ```

7. Fügen Sie die folgende Funktion zum Abfrage für das Gerät, und die Uhrzeit der letzte Neustart erhalten:

    ```
    var queryTwinLastReboot = function() {

        registry.getTwin(deviceToReboot, function(err, twin){

            if (twin.properties.reported.iothubDM != null)
            {
                if (err) {
                    console.error('Could not query twins: ' + err.constructor.name + ': ' + err.message);
                } else {
                    var lastRebootTime = twin.properties.reported.iothubDM.reboot.lastReboot;
                    console.log('Last reboot time: ' + JSON.stringify(lastRebootTime, null, 2));
                }
            } else 
                console.log('Waiting for device to report last reboot time.');
        });
    };
    ```
    
8. Fügen Sie den folgenden Code ein, um die Funktionen aufrufen, die die direkte Methode Neustart und eine Abfrage für die Uhrzeit der letzten Neustart auslöst hinzu:

    ```
    startRebootDevice();
    setInterval(queryTwinLastReboot, 2000);
    ```
    
9. Speichern Sie und schließen Sie die Datei **dmpatterns_getstarted_service.js** .

## <a name="run-the-applications"></a>Führen Sie die Anwendung

Sie können nun zur Ausführung der Anwendung.

1. Führen Sie an der Eingabeaufforderung in den Ordner **Manageddevice** den folgenden Befehl aus, für die direkte Methode Neustart überwacht werden soll.

    ```
    node dmpatterns_getstarted_device.js
    ```

2. Führen Sie bei der Eingabeaufforderung in den Ordner **Triggerrebootondevice** den folgenden Befehl zum Auslösen der remote-Neustart und Abfrage für das Gerät und zum Suchen des letzten Mal neu starten.

    ```
    node dmpatterns_getstarted_service.js
    ```

3. Sie werden die reagieren, um das direkte Methode anzeigen, indem Sie die Nachricht ausgeben

## <a name="customize-and-extend-the-device-management-actions"></a>Passen Sie an und erweitern Sie des Geräts Management-Aktionen

Ihre IoT Lösungen können erweitern Sie die definierten Gerät Management Muster oder benutzerdefinierte Mustern mit Hilfe der Gerät Ziel und C2D Methode Primitives aktivieren. Andere Beispiele für Gerät Management-Aktionen sind Factory zurücksetzen, Firmwareupdate, Softwareupdate, Power Management, Verwaltung von Netzwerk- und Verbindungsproblemen und Daten Verschlüsselung.

## <a name="device-maintenance-windows"></a>Gerät Wartung windows

In der Regel konfigurieren Sie die Geräte nacheinander Aktionen, die Interruptions und Ausfallzeiten minimiert.  Gerät Wartung Windows sind ein häufig verwendete Muster zum Definieren des Wenn ein Gerät seine Konfiguration aktualisiert werden soll. Ihre Back-End-Lösungen können die gewünschten Eigenschaften für das Gerät und zu definieren, und aktivieren Sie eine Richtlinie auf Ihrem Gerät, das ein Wartungsfenster ermöglicht. Wenn ein Gerät die Richtlinie für die Wartung Fenster empfängt, können sie das Gerät und die gemeldete Eigenschaft verwenden, um den Status der Richtlinie aufzuzeichnen. Die Back-End-app dann können Gerät und Abfragen, Compliance Geräte und jede Richtlinie abschließen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm verwendet Sie eine direkte Methode zum Auslösen eines remote Neustarts auf einem Gerät verwendet das Gerät und für das Gerät und zum Ermitteln der Uhrzeit der letzten Neustart des Geräts aus der Cloud abgefragt und gemeldet Eigenschaften, um die Uhrzeit der letzten Neustart vom Gerät zu melden.

Um den Vorgang fortzusetzen überfordert fühlen IoT Hub und Gerät Management Mustern wie Remote über das Air Firmwareupdate finden Sie unter:

[Lernprogramm: So führen Sie ein Firmwareupdate][lnk-fwupdate]

Erfahren Sie, wie Sie Ihre IoT erweitern Lösung und Zeitplan Methode auf mehreren Geräten Ruft, finden Sie im [Zeitplan und die übertragenen Aufträge] [ lnk-tutorial-jobs] Lernprogramm.

Um den Vorgang fortzusetzen überfordert fühlen IoT Hub finden Sie unter [Erste Schritte mit dem IoT Gateway SDK][lnk-gateway-SDK].


<!-- images and links -->
[img-output]: media/iot-hub-get-started-with-dm/image6.png
[img-dm-ui]: media/iot-hub-get-started-with-dm/dmui.png

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md

[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-fwupdate]: iot-hub-firmware-update.md
[Azure portal]: https://portal.azure.com/
[Using resource groups to manage your Azure resources]: ../azure-portal/resource-group-portal.md
[lnk-dm-github]: https://github.com/Azure/azure-iot-device-management
[lnk-tutorial-jobs]: iot-hub-schedule-jobs.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
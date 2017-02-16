<properties
 pageTitle="So führen Sie ein Firmwareupdate | Microsoft Azure"
 description="In diesem Lernprogramm wird gezeigt, wie ein Firmwareupdate führen"
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

# <a name="tutorial-how-to-do-a-firmware-update-preview"></a>Lernprogramm: So führen Sie eine Firmware Update (Preview)

## <a name="introduction"></a>Einführung
Der [Erste Schritte mit Gerätemanagement] [ lnk-dm-getstarted] Lernprogramm haben Sie gesehen, wie Sie das [Gerät zwei] [ lnk-devtwin] und [Cloud-zu-Gerät (C2D) Methoden] [ lnk-c2dmethod] Primitives auf einem Gerät Remote-Neustart. In diesem Lernprogramm verwendet die gleichen IoT Hub primitive und enthält Hinweise und zeigt, wie Sie ein simuliertes End-to-End-Firmwareupdate führen.  Dieses Muster wird in der Firmware Update-Implementierung für die Stichprobe Intel Edison Gerät verwendet.

In diesem Lernprogramm erfahren Sie, wie in:

- Erstellen Sie eine Console-Anwendung, die die direkte Methode FirmwareUpdate auf dem simulierten Gerät über Ihre IoT Hub Anrufe an.
- Erstellen Sie ein simuliertes Gerät, das eine direkte FirmwareUpdate-Methode der führt eine mehrstufige Prozess aus, die zum Herunterladen des Bilds Firmware wartet, das Firmware-Image downloads implementiert und schließlich gilt ten Firmware-Image.  In der gesamten aufgeführte jede Phase das Gerät anhand der zwei Eigenschaften aktualisieren gemeldet Fortschreiten.

Am Ende dieses Lernprogramms haben Sie zwei Node.js Console Applications aus:

**dmpatterns_fwupdate_service.js**, wodurch eine direkte Methode auf dem simulierten Gerät aufgerufen, zeigt die Antwort, und regelmäßig (jeder 500 ms) zeigt die aktualisierten Gerät zwei gemeldet Eigenschaften.

**dmpatterns_fwupdate_device.js**, die an Ihre IoT Verteiler mit der Identität des Geräts zuvor erstellte Verbindung herstellt, empfängt eine FirmwareUpdate direkte Methode, mit mehreren Zuständen schrittweise simulieren einer Firmware Update einschließlich ausgeführt wird: Warten auf das Bild herunterladen, das neue Bild herunterladen und schließlich das Bild anwenden.


Damit dieses Lernprogramm abgeschlossen, benötigen Sie Folgendes:

Node.js Version 0.12.x oder höher <br/>  [Vorbereiten Ihrer Entwicklungsumgebung] [ lnk-dev-setup] beschrieben, wie Sie in diesem Lernprogramm Node.js unter Windows oder Linux zu installieren.

Ein aktives Azure-Konto. (Wenn Sie kein Konto haben, können Sie ein [kostenloses Konto] erstellen[ lnk-free-trial] in nur ein paar Minuten.)

Führen Sie im Artikel [Erste Schritte mit der Verwaltung von Gerät](iot-hub-device-management-get-started.md) zum Erstellen Ihrer IoT Hub und Abrufen der Verbindungszeichenfolge aus.

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-a-simulated-device-app"></a>Erstellen Sie eine app simulierten Gerät

In diesem Abschnitt erstellen Sie eine Node.js Console-app, die an eine direkte Methode, die von der Cloud, die ein simuliertes Gerät Firmwareupdate auslöst aufgerufen, reagiert und verwendet das Gerät zwei gemeldet Eigenschaften Gerät und Abfragen zu identifizieren, Geräte und beim letzten Neustart aktivieren.

1. Erstellen eines neuen leeren Ordners mit dem Namen **Manageddevice**.  Klicken Sie im Ordner **Manageddevice** Erstellen einer package.json-Datei mit dem folgenden Befehl in der Befehlszeile.  Übernehmen Sie alle Standardeinstellungen:

    ```
    npm init
    ```
    
2. Bei der Eingabeaufforderung in den Ordner **Manageddevice** , führen Sie den folgenden Befehl zum Installieren der **azure-iot-device@dtpreview** Gerät SDK-Paket und **azure-iot-device-mqtt@dtpreview** Paket:

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. Erstellen Sie eine neue **dmpatterns_fwupdate_device.js** -Datei mit einem Text-Editor, in den Ordner **Manageddevice** .

4. Fügen Sie die folgenden "Anweisungen am Anfang der Datei **dmpatterns_fwupdate_device.js** erforderlich" hinzu:

    ```
    'use strict';

    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```

5. Fügen Sie eine Variable **ConnectionString** und verwenden Sie, um ein Gerät Client erstellen.  

    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myDeviceId;SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

6. Fügen Sie die folgende Funktion die verwendet wird, aktualisieren und gemeldet Eigenschaften

    ```
    var reportFWUpdateThroughTwin = function(twin, firmwareUpdateValue) {
      var patch = {
          iothubDM : {
            firmwareUpdate : firmwareUpdateValue
          }
      };

      twin.properties.reported.update(patch, function(err) {
        if (err) throw err;
        console.log('twin state reported')
      });
    };
    ```
    
7. Fügen Sie die folgenden Funktionen, die den Download simulieren und des Bilds Firmware anwenden.

    ```
    var simulateDownloadImage = function(imageUrl, callback) {
      var error = null;
      var image = "[fake image data]";
      
      console.log("Downloading image from " + imageUrl);
      
      callback(error, image);
    }

    var simulateApplyImage = function(imageData, callback) {
      var error = null;
      
      if (!imageData) {
        error = {message: 'Apply image failed because of missing image data.'};
      }
      
      callback(error);
    }
    ```

8. Fügen Sie die folgenden Eigenschaften, warten auf herunterladen gemeldet wurden Funktion, die den Aktualisierungsstatus Firmware durch das Ziel aktualisiert werden soll.  In der Regel Geräte eines verfügbaren Updates informiert werden und ein Administrator Richtlinie Ursachen definiert das Gerät herunterladen und Anwenden der Aktualisierung zu beginnen.  Dies ist die Stelle, an der die Logik So aktivieren Sie die Richtlinie ausführen möchten.  Zur Vereinfachung wir für 4 Sekunden verzögern und das Firmware-Image herunterladen. 

    ```
    var waitToDownload = function(twin, fwPackageUriVal, callback) {
      var now = new Date();
      
      reportFWUpdateThroughTwin(twin, {
        fwPackageUri: fwPackageUriVal,
        status: 'waiting',
        error : null,
        startedWaitingTime : now.toISOString()
      });
      setTimeout(callback, 4000);
    };
    ```
    
9. Fügen Sie die folgenden Eigenschaften, das Firmware-Image herunterladen gemeldet wurden Funktion, die den Aktualisierungsstatus Firmware durch das Ziel aktualisiert werden soll.  Es folgt nach oben durch einen Firmware-Download simulieren und schließlich aktualisiert den Aktualisierungsstatus Firmware um entweder einer Download Erfolg oder eines Fehlers zu informieren.

    ```
    var downloadImage = function(twin, fwPackageUriVal, callback) {
      var now = new Date();   
      
      reportFWUpdateThroughTwin(twin, {
        status: 'downloading',
      });
      
      setTimeout(function() {
        // Simulate download
        simulateDownloadImage(fwPackageUriVal, function(err, image) {
          
          if (err)
          {
            reportFWUpdateThroughTwin(twin, {
              status: 'downloadfailed',
              error: {
                code: error_code,
                message: error_message,
              }
            });
          }
          else {        
            reportFWUpdateThroughTwin(twin, {
              status: 'downloadComplete',
              downloadCompleteTime: now.toISOString(),
            });
            
            setTimeout(function() { callback(image); }, 4000);   
          }
        });
        
      }, 4000);
    }
    ```
    
10. Fügen Sie die folgenden Eigenschaften, das Firmware-Image anwenden gemeldet wurden Funktion, die den Aktualisierungsstatus Firmware durch das Ziel aktualisiert werden soll.  Es folgt nach oben durch eine Anwenden des Bilds Firmware simulieren und schließlich aktualisiert den Aktualisierungsstatus Firmware um entweder einer übernehmen Erfolg oder eines Fehlers zu informieren.

    ```
    var applyImage = function(twin, imageData, callback) {
      var now = new Date();   
      
      reportFWUpdateThroughTwin(twin, {
        status: 'applying',
        startedApplyingImage : now.toISOString()
      });
      
      setTimeout(function() {
        
        // Simulate apply firmware image
        simulateApplyImage(imageData, function(err) {
          if (err) {
            reportFWUpdateThroughTwin(twin, {
              status: 'applyFailed',
              error: {
                code: err.error_code,
                message: err.error_message,
              }
            });
          } else { 
            reportFWUpdateThroughTwin(twin, {
              status: 'applyComplete',
              lastFirmwareUpdate: now.toISOString()
            });    
            
          }
        });
        
        setTimeout(callback, 4000);
        
      }, 4000);
    }
    ```

11. Fügen Sie die folgenden Functoin, die die Methode FirmwareUpdate verarbeiten und Aktualisierungsvorgang mehrstufige einleiten.

    ```
    var onFirmwareUpdate = function(request, response) {
            
      // Respond the cloud app for the direct method
      response.send(200, 'FirmwareUpdate started', function(err) {
        if (!err) {
          console.error('An error occured when sending a method response:\n' + err.toString());
        } else {
          console.log('Response to method \'' + request.methodName + '\' sent successfully.');
        }
      });

      // Get the parameter from the body of the method request
      var fwPackageUri = JSON.parse(request.payload).fwPackageUri;

      // Obtain the device twin
      client.getTwin(function(err, twin) {
        if (err) {
          console.error('Could not get device twin.');
        } else {
          console.log('Device twin acquired.');
          
          // Start the multi-stage firmware update
          waitToDownload(twin, fwPackageUri, function() {
            downloadImage(twin, fwPackageUri, function(imageData) {
              applyImage(twin, imageData, function() {});    
            });  
          });

        }
      });
    }
    ```
    
12. Fügen Sie schließlich den folgenden Code der an IoT Verteiler als ein Gerät eine Verbindung herstellt, 

    ```
    client.open(function(err) {
      if (err) {
        console.error('Could not connect to IotHub client');
      }  else {
        console.log('Client connected to IoT Hub.  Waiting for firmwareUpdate direct method.');
      }
      
      client.onDeviceMethod('firmwareUpdate', onFirmwareUpdate(request, response));
    });
    ```

> [AZURE.NOTE] Zur Vereinfachung, implementiert keine "Wiederholen" Richtlinie in diesem Lernprogramm. Herstellung Code, sollten Sie "Wiederholen" Richtlinien (beispielsweise einer exponentiellen Backoff), wie im MSDN-Artikel [Behandeln von vorübergehenden Fehlerstrukturanalyse]vorgeschlagen implementieren[lnk-transient-faults].

## <a name="trigger-a-remote-firmware-update-on-the-device-using-a-direct-method"></a>Auslösen einer remote Firmwareupdate auf dem Gerät mithilfe einer direkten Methode 

In diesem Abschnitt erstellen Sie eine Node.js Console-app, die ein remote Firmwareupdate auf einem Gerät mit einer direkten Methode initiiert und Gerät und Abfragen verwendet, um den Status der aktiven Firmwareupdate regelmäßig auf das Gerät zu erhalten.


1. Erstellen eines neuen leeren Ordners mit dem Namen **Triggerfwupdateondevice**.  Klicken Sie im Ordner **Triggerfwupdateondevice** Erstellen einer package.json-Datei mit dem folgenden Befehl in der Befehlszeile.  Übernehmen Sie alle Standardeinstellungen:

    ```
    npm init
    ```
    
2. Führen Sie an der Eingabeaufforderung in den Ordner **Triggerfwupdateondevice** den folgenden Befehl zum Installieren der **Azure-Iothub** Gerät SDK-Paket und **Azure-Iot-Gerät-Mqtt** -Paket aus:

    ```
    npm install azure-iot-hub@dtpreview --save
    ```
    
3. Erstellen Sie eine neue **dmpatterns_getstarted_service.js** -Datei mit einem Text-Editor, in den Ordner **Triggerfwupdateondevice** .

4. Fügen Sie die folgenden "Anweisungen am Anfang der Datei **dmpatterns_getstarted_service.js** erforderlich" hinzu:

    ```
    'use strict';

    var Registry = require('azure-iothub').Registry;
    var Client = require('azure-iothub').Client;
    ```

5. Fügen Sie die folgenden Variablen Deklarationen, und Ersetzen Sie den Platzhalterwerte:

    ```
    var connectionString = '{device_connectionstring}';
    var registry = Registry.fromConnectionString(connectionString);
    var client = Client.fromConnectionString(connectionString);
    var deviceToUpdate = 'myDeviceId';
    ```
    
6. Fügen Sie die folgende Funktion zum Suchen und Anzeigen der Wert von der FirmwareUpdate gemeldet Eigenschaft.

    ```
    var queryTwinFWUpdateReported = function() {
        registry.getTwin(deviceToUpdate, function(err, twin){
            if (err) {
              console.error('Could not query twins: ' + err.constructor.name + ': ' + err.message);
            } else {
              console.log((JSON.stringify(twin.properties.reported.iothubDM.firmwareUpdate)) + "\n");
            }
        });
    };
    ```

7. Fügen Sie die folgende Funktion zum Aufrufen der FirmwareUpdate-Methode, um das Gerät neu zu starten:

    ```
    var startFirmwareUpdateDevice = function() {
      var params = {
          fwPackageUri: 'https://secureurl'
      };
      
      var methodName = "firmwareUpdate";
      var payloadData =  JSON.stringify(params);
      
      var methodParams = {
        methodName: methodName,
        payload: payloadData,
        timeoutInSeconds: 30
      };
      
      client.invokeDeviceMethod(deviceToUpdate, methodParams, function(err, result) {
        if (err) {
          console.error('Could not start the firmware update on the device: ' + err.message)
        } 
      });
    };
    ```

8. Hinzufügen die Funktion folgende Code ein, um die Firmware zu starten Sequenz aktualisiert und starten Sie regelmäßig mit dem Ziel gemeldet schließlich Eigenschaften:

    ```
    startFirmwareUpdateDevice();
    setInterval(queryTwinFWUpdateReported, 500);
    ```
    
9. Speichern Sie und schließen Sie die Datei **dmpatterns_fwupdate_service.js** .

## <a name="run-the-applications"></a>Führen Sie die Anwendung

Sie können nun zur Ausführung der Anwendung.

1. Führen Sie an der Eingabeaufforderung in den Ordner **Manageddevice** den folgenden Befehl aus, für die direkte Methode Neustart überwacht werden soll.

    ```
    node dmpatterns_fwupdate_device.js
    ```

2. Führen Sie bei der Eingabeaufforderung in den Ordner **Triggerfwupdateondevice** den folgenden Befehl zum Auslösen der remote-Neustart und Abfrage für das Gerät und zum Suchen des letzten Mal neu starten.

    ```
    node dmpatterns_fwupdate_service.js
    ```

3. Sie werden die reagieren, um das direkte Methode anzeigen, indem Sie die Nachricht ausgeben

## <a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm verwendet Sie eine direkte Methode zum Auslösen eines remote Firmwareupdate auf einem Gerät und regelmäßig verwendet das Ziel gemeldet Eigenschaften zu verstehen, den Fortschritt der Firmware aktualisieren Process.  

Erfahren Sie, wie Sie Ihre IoT erweitern Lösung und Zeitplan Methode auf mehreren Geräten Ruft, finden Sie im [Zeitplan und die übertragenen Aufträge] [ lnk-tutorial-jobs] Lernprogramm.

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-dm-getstarted]: iot-hub-device-management-get-started.md
[lnk-tutorial-jobs]: iot-hub-schedule-jobs.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

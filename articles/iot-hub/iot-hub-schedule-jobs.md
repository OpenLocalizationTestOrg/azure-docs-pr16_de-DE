<properties
 pageTitle="Zum Planen von Aufträgen | Microsoft Azure"
 description="In diesem Lernprogramm erfahren Sie, wie zum Planen von Aufträgen"
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

# <a name="tutorial-schedule-and-broadcast-jobs-preview"></a>Lernprogramm: Planen Sie und übertragen Sie Aufträge (Preview)

## <a name="introduction"></a>Einführung
Azure IoT-Hub ist ein vollständig verwalteter Dienst, der eine Anwendung Back-End erstellen und Nachverfolgen von Aufträge, die planen und Aktualisieren von Millionen von Geräten, ermöglicht.  Aufträge können für die folgenden Aktionen verwendet werden:

- Eigenschaften des Geräts gewünscht und aktualisieren
- Aktualisieren von Gerät zwei Kategorien
- Cloud-zu-Gerät Methoden aufrufen

Im Prinzip ein Auftrags umgebrochen wird eine der folgenden Aktionen und zeigt den Verlauf der Ausführung für eine Reihe von Geräten, die von einer Abfrage Ziel definiert ist.  Beispielsweise kann mithilfe eines Auftrags für eine Anwendung Back-End eine Methode Neustart auf 10.000 Geräten, durch eine Abfrage zwei angegebenen und zu einem späteren Zeitpunkt geplant aufrufen.  Diese Anwendung kann dann überwachen wie jede dieser Geräte erhalten, und führen Sie die Methode Neustart.

Weitere Informationen zu jedem der folgenden Funktionen in folgenden Artikeln:

- Gerät Ziel und Eigenschaften: [Erste Schritte mit zwei] [ lnk-get-started-twin] und [Lernprogramm: So verwenden Sie zwei Eigenschaften][lnk-twin-props]
- Cloud-zu-Gerät Methoden: [Developer Guide - direkten Methoden] [ lnk-dev-methods] und [Lernprogramm: C2D Methoden][lnk-c2d-methods]

In diesem Lernprogramm erfahren Sie, wie in:

- Erstellen Sie ein simuliertes Gerät, das eine direkte Methode weist, die LockDoor ermöglicht, die von der Anwendung wieder Ende aufgerufen werden können.
- Erstellen Sie eine Console-Anwendung, ruft die direkten LockDoor-Methode auf dem simulierten Gerät mithilfe eines Projekt aktualisiert die gewünschte Ziel Eigenschaften ein Auftrags Gerät verwenden.

Am Ende dieses Lernprogramms haben Sie zwei Node.js Console Applications aus:

**simDevice.js**, die eine Verbindung mit Ihrem IoT Hub mit der Identität des Geräts und LockDoor direkte Methode empfängt.

**scheduleJobService.js**, ruft eine direkte Methode auf dem simulierten Gerät und das Ziel des Nachlauf Eigenschaften mithilfe eines Auftrags aktualisieren.

Damit dieses Lernprogramm abgeschlossen, benötigen Sie Folgendes:

Node.js Version 0.12.x oder höher <br/>  [Vorbereiten Ihrer Entwicklungsumgebung] [ lnk-dev-setup] beschrieben, wie Sie in diesem Lernprogramm Node.js unter Windows oder Linux zu installieren.

Ein aktives Azure-Konto. (Wenn Sie kein Konto haben, können Sie ein [kostenloses Konto] erstellen[ lnk-free-trial] in nur ein paar Minuten.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-a-simulated-device-app"></a>Erstellen Sie eine app simulierten Gerät

In diesem Abschnitt erstellen Sie eine Node.js Console-app, die an eine direkte Methode, die von der Cloud, die einen simuliertes Gerät Neustart auslöst aufgerufen, reagiert und verwendet das Gerät zwei gemeldet Eigenschaften Gerät und Abfragen zu identifizieren, Geräte und beim letzten Neustart aktivieren.

1. Erstellen eines neuen leeren Ordners mit dem Namen **SimDevice**.  Klicken Sie im Ordner **SimDevice** Erstellen einer package.json-Datei mit dem folgenden Befehl in der Befehlszeile.  Übernehmen Sie alle Standardeinstellungen:

    ```
    npm init
    ```
    
2. Führen Sie an der Eingabeaufforderung in den Ordner **SimDevice** den folgenden Befehl zum Installieren der **Azure-Iot-Gerät** Gerät SDK-Paket und **Azure-Iot-Gerät-Mqtt** -Paket aus:

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. Erstellen Sie eine neue **simDevice.js** -Datei mit einem Text-Editor, in den Ordner **SimDevice** .

4. Fügen Sie die folgenden "Anweisungen am Anfang der Datei **simDevice.js** erforderlich" hinzu:

    ```
    'use strict';

    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```

5. Fügen Sie eine Variable **ConnectionString** und verwenden Sie, um ein Gerät Client erstellen.  

    ```
    var connectionString = 'HostName={youriothostname};DeviceId={yourdeviceid};SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

6. Fügen Sie die folgende Funktion aus, um die Methode LockDoor behandeln.

    ```
    var onLockDoor = function(request, response) {
        
        // Respond the cloud app for the direct method
        response.send(200, function(err) {
            if (!err) {
                console.error('An error occured when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.');
            }
        });
        
        console.log('Locking Door!');
    };
    ```

7. Fügen Sie den folgenden Code ein, um den Ereignishandler für die Methode LockDoor zu registrieren.

    ```
    client.open(function(err) {
        if (err) {
            console.error('Could not connect to IotHub client.');
        }  else {
            console.log('Client connected to IoT Hub.  Waiting for reboot direct method.');
            client.onDeviceMethod('lockDoor', onLockDoor);
        }
    });
    ```
    
8. Speichern Sie und schließen Sie die Datei **simDevice.js** .

> [AZURE.NOTE] Zur Vereinfachung, implementiert keine "Wiederholen" Richtlinie in diesem Lernprogramm. Herstellung Code, sollten Sie "Wiederholen" Richtlinien (beispielsweise einer exponentiellen Backoff), wie im MSDN-Artikel [Behandeln von vorübergehenden Fehlerstrukturanalyse]vorgeschlagen implementieren[lnk-transient-faults].

## <a name="schedule-jobs-for-calling-a-direct-method-and-updating-a-twins-properties"></a>Planen von Aufträgen für eine direkte Methode aufrufen und Aktualisieren einer und Eigenschaften

In diesem Abschnitt erstellen Sie eine Node.js Console-app, die eine remote LockDoor auf einem Gerät mit einer direkten Methode initiiert und das Ziel des Eigenschaften aktualisieren.

1. Erstellen eines neuen leeren Ordners mit dem Namen **ScheduleJobService**.  Klicken Sie im Ordner **ScheduleJobService** Erstellen einer package.json-Datei mit dem folgenden Befehl in der Befehlszeile.  Übernehmen Sie alle Standardeinstellungen:

    ```
    npm init
    ```
    
2. Führen Sie an der Eingabeaufforderung in den Ordner **ScheduleJobService** den folgenden Befehl zum Installieren der **Azure-Iothub** Gerät SDK-Paket und **Azure-Iot-Gerät-Mqtt** -Paket aus:

    ```
    npm install azure-iothub@dtpreview uuid --save
    ```
    
3. Erstellen Sie eine neue **scheduleJobService.js** -Datei mit einem Text-Editor, in den Ordner **ScheduleJobService** .

4. Fügen Sie die folgenden "Anweisungen am Anfang der Datei **dmpatterns_gscheduleJobServiceetstarted_service.js** erforderlich" hinzu:

    ```
    'use strict';

    var uuid = require('uuid');
    var JobClient = require('azure-iothub').JobClient;
    ```

5. Fügen Sie die folgenden Variablen Deklarationen, und Ersetzen Sie den Platzhalterwerte:

    ```
    var connectionString = '{iothubconnectionstring}';
    var deviceArray = ['myDeviceId'];
    var startTime = new Date();
    var maxExecutionTimeInSeconds =  3600;
    var jobClient = JobClient.fromConnectionString(connectionString);
    ```
    
6. Fügen Sie die folgende Funktion, die zum Überwachen der Ausführung des Auftrags verwendet wird:

    ```
    function monitorJob (jobId, callback) {
        var jobMonitorInterval = setInterval(function() {
            jobClient.getJob(jobId, function(err, result) {
            if (err) {
                console.error('Could not get job status: ' + err.message);
            } else {
                console.log('Job: ' + jobId + ' - status: ' + result.status);
                if (result.status === 'completed' || result.status === 'failed' || result.status === 'cancelled') {
                clearInterval(jobMonitorInterval);
                callback(null, result);
                }
            }
            });
        }, 5000);
    }
    ```

7. Fügen Sie den folgenden Code ein, um den Auftrag zu planen, der die Gerät Methode ruft hinzu:

    ```
    var methodParams = {
        methodName: 'lockDoor',
        payload: null,
        timeoutInSeconds: 45
    };

    var methodJobId = uuid.v4();
    console.log('scheduling Device Method job with id: ' + methodJobId);
    jobClient.scheduleDeviceMethod(methodJobId,
                                deviceArray,
                                methodParams,
                                startTime,
                                maxExecutionTimeInSeconds,
                                function(err) {
        if (err) {
            console.error('Could not schedule device method job: ' + err.message);
        } else {
            monitorJob(methodJobId, function(err, result) {
                if (err) {
                    console.error('Could not monitor device method job: ' + err.message);
                } else {
                    console.log(JSON.stringify(result, null, 2));
                }
            });
        }
    });
    ```
        
8. Fügen Sie den folgenden Code ein, um den Auftrag das Ziel aktualisieren planen:

    ```
    var twinPatch = {
        etag: '*',
        desired: {
            building: '43',
            floor: 3
        }
    };

    var twinJobId = uuid.v4();

    console.log('scheduling Twin Update job with id: ' + twinJobId);
    jobClient.scheduleTwinUpdate(twinJobId,
                                deviceArray,
                                twinPatch,
                                startTime,
                                maxExecutionTimeInSeconds,
                                function(err) {
        if (err) {
            console.error('Could not schedule twin update job: ' + err.message);
        } else {
            monitorJob(twinJobId, function(err, result) {
                if (err) {
                    console.error('Could not monitor twin update job: ' + err.message);
                } else {
                    console.log(JSON.stringify(result, null, 2));
                }
            });
        }
    });
    ```
    
9. Speichern Sie und schließen Sie die Datei **scheduleJobService.js** .

## <a name="run-the-applications"></a>Führen Sie die Anwendung

Sie können nun zur Ausführung der Anwendung.

1. Führen Sie an der Eingabeaufforderung in den Ordner **SimDevice** den folgenden Befehl aus, für die direkte Methode Neustart überwacht werden soll.

    ```
    node simDevice.js
    ```

2. Führen Sie bei der Eingabeaufforderung in den Ordner **ScheduleJobService** den folgenden Befehl zum Auslösen der remote-Neustart und Abfrage für das Gerät und zum Suchen des letzten Mal neu starten.

    ```
    node scheduleJobService.js
    ```

3. Die Ausgabe von sowohl Gerät und Back-End-Anwendungen werden angezeigt.


## <a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm haben Sie ein Projekt verwendet, um eine direkte Methode zum ein Gerät und der Aktualisierung der Eigenschaften für das Gerät-und planen.

Um den Vorgang fortzusetzen überfordert fühlen IoT Hub und Gerät Management Mustern wie Remote über das Air Firmwareupdate finden Sie unter:

[Lernprogramm: So führen Sie ein Firmwareupdate][lnk-fwupdate]

Um den Vorgang fortzusetzen überfordert fühlen IoT Hub finden Sie unter [Erste Schritte mit dem IoT Gateway SDK][lnk-gateway-SDK].

[lnk-get-started-twin]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-props]: iot-hub-node-node-twin-how-to-configure.md
[lnk-c2d-methods]: iot-hub-c2d-methods.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-fwupdate]: iot-hub-firmware-update.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
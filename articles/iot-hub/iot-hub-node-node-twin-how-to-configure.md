<properties
    pageTitle="Verwenden Sie zwei Eigenschaften | Microsoft Azure"
    description="In diesem Lernprogramm wird gezeigt, wie zwei Eigenschaften verwenden"
    services="iot-hub"
    documentationCenter=".net"
    authors="fsautomata"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="node"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/13/2016"
     ms.author="elioda"/>

# <a name="tutorial-use-desired-properties-to-configure-devices-preview"></a>Lernprogramm: Verwenden der gewünschte Eigenschaften zum Konfigurieren von Geräten (Preview)

[AZURE.INCLUDE [iot-hub-selector-twin-how-to-configure](../../includes/iot-hub-selector-twin-how-to-configure.md)]

Am Ende dieses Lernprogramms haben Sie zwei Node.js Console Applications:

* **SimulateDeviceConfiguration.js**, eines simulierten Gerät-app, die eine gewünschte Konfiguration Update wartet und Berichten des Status eines simulierten Konfiguration Aktualisierungsvorgang.
* **SetDesiredConfigurationAndQuery.js**, eine Node.js app werden die legt der gewünschten Konfigurations auf einem Gerät und Abfragen Aktualisierungsvorgang Konfiguration von Back-End ausgeführt werden sollen.

> [AZURE.NOTE] Im Artikel [IoT Hub SDKs] [ lnk-hub-sdks] finden Sie Informationen zu den verschiedenen SDKs aus, die Sie verwenden können, um Gerät und der Back-End-Anwendung zu erstellen.

Zum Abschluss des Lernprogramms benötigen Sie Folgendes:

+ Node.js Version 0.10.x oder höher.

+ Ein aktives Azure-Konto. (Wenn Sie kein Konto haben, können Sie ein [kostenloses Konto] erstellen[ lnk-free-trial] in nur ein paar Minuten.)

Wenn Sie die [Erste Schritte mit dem Gerät im Vergleich] befolgt[ lnk-twin-tutorial] Lernprogramm bereits ein Gerät aktiviert Management-Hub und ein Gerät Identität aufgerufen **MyDeviceId**; und Sie können fahren Sie mit dem [Erstellen der app simulierten Gerät] [ lnk-how-to-configure-createapp] Abschnitt.

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-the-simulated-device-app"></a>Erstellen Sie die app simulierten Gerät

In diesem Abschnitt erstellen Sie eine Node.js Console-app, die eine Verbindung mit Ihrem Hub als **MyDeviceId**, ein Update für die gewünschte Konfiguration wartet und Berichte klicken Sie dann auf den Aktualisierungsvorgang simulierten Konfiguration Updates.

1. Erstellen eines neuen leeren Ordners mit dem Namen **Simulatedeviceconfiguration**. Klicken Sie im Ordner **Simulatedeviceconfiguration** Erstellen einer neuen package.json-Datei mit dem folgenden Befehl in der Befehlszeile. Übernehmen Sie alle Standardeinstellungen:

    ```
    npm init
    ```

2. Führen Sie in der Befehlszeile in den Ordner **Simulatedeviceconfiguration** den folgenden Befehl zum Installieren der **Azure-Iot-Gerät**und **Azure-Iot-Gerät-Mqtt** -Paket aus:

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. Erstellen Sie eine neue **SimulateDeviceConfiguration.js** -Datei mit einem Text-Editor, in den Ordner **Simulatedeviceconfiguration** .

4. Fügen Sie den folgenden Code zu der Datei **SimulateDeviceConfiguration.js** , und Ersetzen Sie den Platzhalter **{Gerät Verbindungszeichenfolge}** mit der Verbindungszeichenfolge aus, die Sie kopiert haben, wenn Sie die Identität des **MyDeviceId** Geräts erstellt haben:

        'use strict';
        var Client = require('azure-iot-device').Client;
        var Protocol = require('azure-iot-device-mqtt').Mqtt;

        var connectionString = '{device connection string}';
        var client = Client.fromConnectionString(connectionString, Protocol);

        client.open(function(err) {
            if (err) {
                console.error('could not open IotHub client');
            } else {
                client.getTwin(function(err, twin) {
                    if (err) {
                        console.error('could not get twin');
                    } else {
                        console.log('retrieved device twin');
                        twin.properties.reported.telemetryConfig = {
                            configId: "0",
                            sendFrequency: "24h"
                        }
                        twin.on('properties.desired', function(desiredChange) {
                            console.log("received change: "+JSON.stringify(desiredChange));
                            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
                            if (desiredChange.telemetryConfig &&desiredChange.telemetryConfig.configId !== currentTelemetryConfig.configId) {
                                initConfigChange(twin);
                            }
                        });
                    }
                });
            }
        });

    Das **Client** -Objekt macht alle Methoden für die Interaktion mit dem Gerät im Vergleich vom Gerät erforderlich. Vorherige Code nach der Initialisierung des **Client** -Objekts, das Ziel für **MyDeviceId**ruft und fügt einen Ereignishandler für das Update auf die gewünschten Eigenschaften. Der Ereignishandler überprüft, die es ist eine Anforderung einer tatsächliche Konfiguration durch Vergleich der ConfigIds, und klicken Sie dann Ruft eine Methode, die Änderung der Konfiguration wird gestartet.

    Beachten Sie, dass aus Gründen der Vereinfachung des vorherigen Codes für die ursprüngliche Konfiguration hartcodierte Standardwert verwendet. Reale-app würde wahrscheinlich die Konfiguration aus einer lokalen Speicher zu laden.
    
    > [AZURE.IMPORTANT] Gewünschte Eigenschaft Änderungsereignisse sind in Verbindung mit Gerät immer einmal ausgegeben, stellen Sie sicher, dass es eine tatsächliche Änderung in die gewünschten Eigenschaften, bevor eine Aktion ausgeführt wird.

5. Fügen Sie die folgenden Methoden vor der `client.open()` aufrufen:

        var initConfigChange = function(twin) {
            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
            currentTelemetryConfig.pendingConfig = twin.properties.desired.telemetryConfig;
            currentTelemetryConfig.status = "Pending";

            var patch = {
            telemetryConfig: currentTelemetryConfig
            };
            twin.properties.reported.update(patch, function(err) {
                if (err) {
                    console.log('Could not report properties');
                } else {
                    console.log('Reported pending config change: ' + JSON.stringify(patch));
                    setTimeout(function() {completeConfigChange(twin);}, 60000);
                }
            });
        }

        var completeConfigChange =  function(twin) {
            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
            currentTelemetryConfig.configId = currentTelemetryConfig.pendingConfig.configId;
            currentTelemetryConfig.sendFrequency = currentTelemetryConfig.pendingConfig.sendFrequency;
            currentTelemetryConfig.status = "Success";
            delete currentTelemetryConfig.pendingConfig;
            
            var patch = {
                telemetryConfig: currentTelemetryConfig
            };
            patch.telemetryConfig.pendingConfig = null;

            twin.properties.reported.update(patch, function(err) {
                if (err) {
                    console.error('Error reporting properties: ' + err);
                } else {
                    console.log('Reported completed config change: ' + JSON.stringify(patch));
                }
            });
        };

    Die Methode **InitConfigChange** gemeldete Eigenschaften auf das Objekt lokale und mit der Config Update Anforderung aktualisiert setzt den Status auf **steht noch aus**, und so aktualisiert, dass das Gerät und klicken Sie auf den Dienst. Nach dem erfolgreichen Update das Ziel, simuliert es einen langer Prozess, der bei der Ausführung der **CompleteConfigChange**beendet wird. Diese Methode aktualisiert die lokale Ziel gemeldeten Eigenschaften den Status bei **Erfolg** und entfernen das Objekt **PendingConfig** . Klicken Sie dann aktualisiert die Ziel-Dienst.

    Beachten Sie, das Speichern von Bandbreite, gemeldet Eigenschaften werden aktualisiert, indem Sie nur die Eigenschaften, um (benannte **Patch** im obigen Code), statt Ersetzen das gesamte Dokument geändert werden.

    > [AZURE.NOTE] In diesem Lernprogramm keine Verhalten für gleichzeitige Konfiguration Updates simulieren. Einige Konfiguration Update Prozesse möglicherweise Änderungen der Zielkonfiguration aufzunehmen, während die Aktualisierung wird ausgeführt, andere möglicherweise sie die Warteschlange müssen und andere mit einer Bedingung zurück abzulehnen konnte. Vergewissern Sie sich das gewünschte Verhalten für bestimmte Konfigurationsprozesses zu berücksichtigen, und fügen Sie die entsprechende Logik, bevor Sie mit der Änderung der Konfiguration.

6. Führen Sie die app Gerät:

        node SimulateDeviceConfiguration.js

    Sie sollten die Nachricht finden Sie unter `retrieved device twin`. Lassen Sie die app ausgeführt.

## <a name="create-the-service-app"></a>Die Dienst-app erstellen

In diesem Abschnitt erstellen Sie eine Node.js Console-app, die die *gewünschten Eigenschaften* auf das Ziel zugeordnet **MyDeviceId** mit einem neuen werden Konfigurationsobjekt aktualisiert. Klicken Sie dann das Gerät im Vergleich im Hub gespeicherte Abfragen, und zeigt die Differenz zwischen den gewünschten und gemeldeten Konfigurationen des Geräts.

1. Erstellen eines neuen leeren Ordners mit dem Namen **Setdesiredandqueryapp**. Klicken Sie im Ordner **Setdesiredandqueryapp** Erstellen einer neuen package.json-Datei mit dem folgenden Befehl in der Befehlszeile. Übernehmen Sie alle Standardeinstellungen:

    ```
    npm init
    ```

2. Führen Sie in der Befehlszeile in den Ordner **Setdesiredandqueryapp** den folgenden Befehl zum Installieren des Pakets **Azure-Iothub** aus:

    ```
    npm install azure-iothub@dtpreview node-uuid --save
    ```

3. Erstellen Sie eine neue **SetDesiredAndQuery.js** -Datei mit einem Text-Editor, in den Ordner **Addtagsandqueryapp** .

4. Fügen Sie den folgenden Code zu der Datei **SetDesiredAndQuery.js** , und Ersetzen Sie den Platzhalter **{Dienst Verbindungszeichenfolge}** mit der Verbindungszeichenfolge aus, die Sie kopiert haben, wenn Sie Ihre Hub erstellt haben:

        'use strict';
        var iothub = require('azure-iothub');
        var uuid = require('node-uuid');
        var connectionString = '{service connection string}';
        var registry = iothub.Registry.fromConnectionString(connectionString);
         
        registry.getTwin('myDeviceId', function(err, twin){
            if (err) {
                console.error(err.constructor.name + ': ' + err.message);
            } else {
                var newConfigId = uuid.v4();
                var newFrequency = process.argv[2] || "5m";
                var patch = {
                    properties: {
                        desired: {
                            telemetryConfig: {
                                configId: newConfigId,
                                sendFrequency: newFrequency
                            }
                        }
                    }
                }
                twin.update(patch, function(err) {
                    if (err) {
                        console.error('Could not update twin: ' + err.constructor.name + ': ' + err.message);
                    } else {
                        console.log(twin.deviceId + ' twin updated successfully');
                    }
                });
                setInterval(queryTwins, 10000);
            }
        });
            

    Das Objekt **Registrierung** macht alle Methoden für die Interaktion mit dem Gerät im Vergleich vom Dienst erforderlich. Vorherige Code nach der Initialisierung des **Registrierung** -Objekts, das Ziel für **MyDeviceId**Ruft, und die gewünschten Eigenschaften mit einem neuen werden Konfigurationsobjekt aktualisiert. Anschließend wird das **QueryTwins** Funktion Ereignis 10 Sekunden.

    > [AZURE.IMPORTANT] Diese Anwendung fragt IoT Hub für Erläuterung alle 10 Sekunden. Verwenden Sie Abfragen aus, um Benutzer zugänglichen Berichte auf vielen Geräten und nicht um die Änderungen zu erkennen. Wenn Ihre Lösung erfordert in Echtzeit Benachrichtigungen über Geräteereignisse verwenden [Gerät-Cloud - Nachrichten][lnk-d2c].

5. Fügen Sie den folgenden Code direkt vor der `registry.getDeviceTwin()` Aufrufen die Funktion **QueryTwins** implementiert wird:

        var queryTwins = function() {
            var query = registry.createQuery("SELECT * FROM devices WHERE deviceId = 'myDeviceId'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed to fetch the results: ' + err.message);
                } else {
                    console.log();
                    results.forEach(function(twin) {
                        var desiredConfig = twin.properties.desired.telemetryConfig;
                        var reportedConfig = twin.properties.reported.telemetryConfig;
                        console.log("Config report for: " + twin.deviceId);
                        console.log("Desired: ");
                        console.log(JSON.stringify(desiredConfig, null, 2));
                        console.log("Reported: ");
                        console.log(JSON.stringify(reportedConfig, null, 2));
                    });
                }
            });
        };

    Vorherigen Code Abfragen, die im Vergleich im Hub gespeichert und druckt die Konfigurationen gewünschten und gemeldet werden. Schlagen Sie in der [Abfragesprache IoT Hub] [ lnk-query] erfahren, wie über Ihren sämtlichen Geräten Rich-Berichte zu erstellen.


8. Führen Sie die Anwendung mit, mit **SimulateDeviceConfiguration.js** Ausführung:

        node SetDesiredAndQuery.js 5m

    Folgendes sollte angezeigt werden die gemeldeten Konfiguration Änderung dem **Erfolg** **Ausstehend** zum **Erfolg** erneut mit dem neuen aktiven Häufigkeit von fünf Minuten anstelle von 24 Stunden zu senden.

    > [AZURE.IMPORTANT] Es gibt eine Verzögerung von bis zu eine Minute um zwischen den Gerät Bericht Vorgang und das Abfrageergebnis an. Dies ist die Abfrage Infrastruktur für die Arbeit mit sehr hoch Maßen aktivieren. Zum Abrufen verwenden eine einzelne und konsistente Ansichten die **GetDeviceTwin** -Methode in der Klasse **Registry** .

## <a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm legen Sie die gewünschte Konfiguration als die *gewünschten Eigenschaften* aus einer Back-End-Anwendung, und geschrieben haben eine app simulierten Gerät zum Ermitteln, die ändern und einen mit mehreren Schritten Aktualisierungsvorgang als *Eigenschaften gemeldet* seine Statusberichte das Ziel zu reproduzieren.

Verwenden Sie die folgenden Ressourcen um zu erfahren Sie, wie Sie:

- werden von Geräten mit der [Erste Schritte mit IoT Hub] senden[ lnk-iothub-getstarted] Lernprogramm
- Planung oder Operationen für eine Vielzahl von Geräten finden Sie unter [verwenden Aufträge zu planen und zu übertragen Gerätevorgänge] [ lnk-schedule-jobs] Lernprogramm.
- Steuern von Geräten interaktiv (z. B. einschalten ein Lüfter aus einer app benutzergesteuerte), mit der [direkten Methoden verwenden] [ lnk-methods-tutorial] Lernprogramm.


<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-dm-overview]: iot-hub-device-management-overview.md
[lnk-twin-tutorial]: iot-hub-node-node-twin-getstarted.md
[lnk-schedule-jobs]: iot-hub-schedule-jobs.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-methods-tutorial]: iot-hub-c2d-methods.md

[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier

[lnk-how-to-configure-createapp]: iot-hub-node-node-twin-how-to-configure.md#create-the-simulated-device-app

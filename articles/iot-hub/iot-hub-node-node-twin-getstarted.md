<properties
    pageTitle="Erste Schritte mit im Vergleich | Microsoft Azure"
    description="In diesem Lernprogramm erfahren Sie, wie im Vergleich zu verwenden"
    services="iot-hub"
    documentationCenter="node"
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

# <a name="tutorial-get-started-with-device-twins-preview"></a>Lernprogramm: Erste Schritte mit dem Gerät im Vergleich (Preview)

[AZURE.INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

Am Ende dieses Lernprogramms haben Sie zwei Node.js Console Applications:

* **AddTagsAndQuery.js**, eine Node.js app werden die Kategorien addiert und Abfragen Gerät im Vergleich von Back-End ausgeführt werden sollen.
* **TwinSimulatedDevice.js**, eine app von Node.js simuliert ein Gerät, mit der Identität des Geräts zuvor erstellten an Ihre IoT Verteiler verbindet, und seine Bedingung Connectivity-Berichte.

> [AZURE.NOTE] Im Artikel [IoT Hub SDKs] [ lnk-hub-sdks] finden Sie Informationen zu den verschiedenen SDKs aus, die Sie verwenden können, um Gerät und der Back-End-Anwendung zu erstellen.

Zum Abschluss des Lernprogramms benötigen Sie Folgendes:

+ Node.js Version 0.10.x oder höher.

+ Ein aktives Azure-Konto. (Wenn Sie kein Konto haben, können Sie ein [kostenloses Konto] erstellen[ lnk-free-trial] in nur ein paar Minuten.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-the-service-app"></a>Die Dienst-app erstellen

In diesem Abschnitt erstellen Sie eine Node.js Console-app, die das Ziel zugeordnet **MyDeviceId**Speicherort Metadaten hinzufügt. Dann Abfragen, dass der im Vergleich gespeichert im Hub, indem die Geräte in den USA, und klicken Sie dann auf diejenigen befindet sich die Berichterstattung einer mobilen Verbindungs.

1. Erstellen eines neuen leeren Ordners mit dem Namen **Addtagsandqueryapp**. Klicken Sie im Ordner **Addtagsandqueryapp** Erstellen einer neuen package.json-Datei mit dem folgenden Befehl in der Befehlszeile. Übernehmen Sie alle Standardeinstellungen:

    ```
    npm init
    ```

2. Führen Sie in der Befehlszeile in den Ordner **Addtagsandqueryapp** den folgenden Befehl zum Installieren des Pakets **Azure-Iothub** aus:

    ```
    npm install azure-iothub@dtpreview --save
    ```

3. Erstellen Sie eine neue **AddTagsAndQuery.js** -Datei mit einem Text-Editor, in den Ordner **Addtagsandqueryapp** .

4. Fügen Sie den folgenden Code zu der Datei **AddTagsAndQuery.js** , und Ersetzen Sie den Platzhalter **{Dienst Verbindungszeichenfolge}** mit der Verbindungszeichenfolge aus, die Sie kopiert haben, wenn Sie Ihre Hub erstellt haben:

        'use strict';
        var iothub = require('azure-iothub');
        var connectionString = '{service hub connection string}';
        var registry = iothub.Registry.fromConnectionString(connectionString);

        registry.getTwin('myDeviceId', function(err, twin){
            if (err) {
                console.error(err.constructor.name + ': ' + err.message);
            } else {
                var patch = {
                    tags: {
                        location: {
                            region: 'US',
                            plant: 'Redmond43'
                      }
                    }
                };
             
                twin.update(patch, function(err) {
                  if (err) {
                    console.error('Could not update twin: ' + err.constructor.name + ': ' + err.message);
                  } else {
                    console.log(twin.deviceId + ' twin updated successfully');
                    queryTwins();
                  }
                });
            }
        });

    Das Objekt **Registrierung** macht alle Methoden für die Interaktion mit dem Gerät im Vergleich vom Dienst erforderlich. Der vorherige Code initialisiert zuerst das **Registrierungs** -Objekt, und klicken Sie dann Ruft das Ziel für **MyDeviceId**und schließlich seine Tags mit den gewünschten Speicherortinformationen aktualisiert.

    Nach dem Aktualisieren der Kategorien wird die Funktion **QueryTwins** .

7. Fügen Sie den folgenden Code am Ende des **AddTagsAndQuery.js** zum Implementieren der Funktion **QueryTwins** hinzu:

        var queryTwins = function() {
            var query = registry.createQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed to fetch the results: ' + err.message);
                } else {
                    console.log("Devices in Redmond43: " + results.map(function(twin) {return twin.deviceId}).join(','));
                }
            });
            
            query = registry.createQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43' AND properties.reported.connectivity.type = 'cellular'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed to fetch the results: ' + err.message);
                } else {
                    console.log("Devices in Redmond43 using cellular network: " + results.map(function(twin) {return twin.deviceId}).join(','));
                }
            });
        };

    Der vorherige Code werden zwei Abfragen ausgeführt: die erste wählt nur das Gerät im Vergleich von Geräten in **Redmond43** Pflanze ist, und das zweite passt die Abfrage aus, um nur die Geräte auszuwählen, die auch über das mobile Netzwerk verbunden sind.

    Beachten Sie, dass der vorherige Code, wenn sie das Objekt **Abfrage** erstellt eine maximale Anzahl von zurückgegebenen Dokumente angibt. Das **Abfrage** -Objekt enthält eine **HasMoreResults** boolesche Eigenschaft, die Sie verwenden können, um die **NextAsTwin** Methoden mehrmals aufzurufen, um alle Ergebnisse abzurufen. Eine Methode namens **nächsten** steht für die Ergebnisse, die nicht Gerät im Vergleich zum Beispiel Resultate Aggregationsabfragen sind.

8. Führen Sie die Anwendung mit:

        node AddTagsAndQuery.js

    Finden Sie unter einem einzigen Gerät in die Ergebnisse für die Abfrage mit allen Geräten in **Redmond43** und keine für die Abfrage, die die Ergebnisse auf Geräte beschränkt, die einem Mobilfunknetz verwenden.

    ![][1]

Im nächsten Abschnitt erstellen Sie eine Gerät-app, die die Informationen Connectivity-Berichte und ändert sich das Ergebnis der Abfrage im vorherigen Abschnitt.

## <a name="create-the-device-app"></a>Erstellen Sie die app Gerät

In diesem Abschnitt erstellen Sie eine Node.js Console-app, die eine Verbindung mit Ihrem Hub als **MyDeviceId**, und klicken Sie dann aktualisiert dessen Ziel des gemeldet Eigenschaften, die Daten enthalten, dass er mit einem Mobilfunknetz verbunden ist.

> [AZURE.NOTE] Zu diesem Zeitpunkt für Gerät im Vergleich nur von Geräten, die Verbindung IoT Hub mit dem MQTT-Protokoll zugegriffen werden. Lizenzinformationen finden Sie die [MQTT unterstützen] [ lnk-devguide-mqtt] Artikel Anweisungen zum Konvertieren von vorhandenen Gerät app MQTT verwenden.

1. Erstellen eines neuen leeren Ordners mit dem Namen **Reportconnectivity**. Klicken Sie im Ordner **Reportconnectivity** Erstellen einer neuen package.json-Datei mit dem folgenden Befehl in der Befehlszeile. Übernehmen Sie alle Standardeinstellungen:

    ```
    npm init
    ```

2. Führen Sie in der Befehlszeile in den Ordner **Reportconnectivity** den folgenden Befehl zum Installieren der **Azure-Iot-Gerät**und **Azure-Iot-Gerät-Mqtt** -Paket aus:

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. Erstellen Sie eine neue **ReportConnectivity.js** -Datei mit einem Text-Editor, in den Ordner **Reportconnectivity** .

4. Fügen Sie den folgenden Code zu der Datei **ReportConnectivity.js** , und Ersetzen Sie den Platzhalter **{Gerät Verbindungszeichenfolge}** mit der Verbindungszeichenfolge aus, die Sie kopiert haben, wenn Sie die Identität des **MyDeviceId** Geräts erstellt haben:

        'use strict';
        var Client = require('azure-iot-device').Client;
        var Protocol = require('azure-iot-device-mqtt').Mqtt;

        var connectionString = '{device connection string}';
        var client = Client.fromConnectionString(connectionString, Protocol);

        client.open(function(err) {
        if (err) {
            console.error('could not open IotHub client');
        }  else {
            console.log('client opened');

            client.getTwin(function(err, twin) {
            if (err) {
                console.error('could not get twin');
            } else {
                var patch = {
                    connectivity: {
                        type: 'cellular'
                    }
                };

                twin.properties.reported.update(patch, function(err) {
                    if (err) {
                        console.error('could not update twin');
                    } else {
                        console.log('twin state reported');
                        process.exit();
                    }
                });
            }
            });
        }
        });

    Das **Client** -Objekt macht alle Methoden, die Sie für die Interaktion mit dem Gerät im Vergleich vom Gerät benötigen. Vorherige Code wird nach der Initialisierung des **Client** -Objekts, ruft das Ziel für **MyDeviceId** und seine Eigenschaft gemeldete mit der Konnektivitätsinformationen aktualisiert.

5. Führen Sie die app Gerät

        node ReportConnectivity.js

    Sie sollten die Nachricht finden Sie unter `twin state reported`.

6. Jetzt, da das Gerät seine Informationen Connectivity gemeldet, sollten sie in beiden Abfragen angezeigt. Wechseln Sie zurück in den Ordner **Addtagsandqueryapp** , und führen Sie die Abfragen erneut aus:

        node AddTagsAndQuery.js

    Dieses Mal **MyDeviceId** sollte beide Abfrageergebnisse angezeigt werden.

    ![][3]

## <a name="next-steps"></a>Nächste Schritte
In diesem Lernprogramm so konfiguriert, dass einen neuen IoT Hub Azure-Portal, und klicken Sie dann in der Hub Identität Registrierung erstellt eine Gerät Identität. Sie Gerät Metadaten als Tags aus einer Back-End-Anwendung hinzugefügt und geschrieben haben eine app simulierten Gerät zu Bericht Gerät Connectivity Informationen in das Ziel Gerät. Außerdem haben Sie erfahren, wie Sie diese Informationen mithilfe der IoT Hub Abfragesprache SQL-ähnliche Abfragen.

Verwenden Sie die folgenden Ressourcen um zu erfahren Sie, wie Sie:

- werden von Geräten mit der [Erste Schritte mit IoT Hub] senden[ lnk-iothub-getstarted] Lernprogramm
- Konfigurieren von Geräten verwenden und die gewünschten Eigenschaften mit den [gewünschte Eigenschaften zum Konfigurieren von Geräten verwenden] [ lnk-twin-how-to-configure] Lernprogramm
- Steuern von Geräten interaktiv (z. B. einschalten ein Lüfter aus einer app benutzergesteuerte), mit der [direkten Methoden verwenden] [ lnk-methods-tutorial] Lernprogramm.

<!-- images -->
[1]: media/iot-hub-node-node-twin-getstarted/service1.png
[3]: media/iot-hub-node-node-twin-getstarted/service2.png

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-identity]: iot-hub-devguide-identity-registry.md

[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/

[lnk-twin-how-to-configure]: iot-hub-node-node-twin-how-to-configure.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md

[lnk-methods-tutorial]: iot-hub-c2d-methods.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
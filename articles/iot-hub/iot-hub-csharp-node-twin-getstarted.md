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

Am Ende dieses Lernprogramms haben Sie eine .NET und eine Node.js Console-Anwendung:

* **AddTagsAndQuery.sln**, eine .NET app werden die Kategorien addiert und Abfragen Gerät im Vergleich von Back-End ausgeführt werden sollen.
* **TwinSimulatedDevice.js**, eine app von Node.js simuliert ein Gerät, mit der Identität des Geräts zuvor erstellten an Ihre IoT Verteiler verbindet, und seine Bedingung Connectivity-Berichte.

> [AZURE.NOTE] Im Artikel [IoT Hub SDKs] [ lnk-hub-sdks] finden Sie Informationen zu den verschiedenen SDKs aus, die Sie verwenden können, um Gerät und der Back-End-Anwendung zu erstellen.

Zum Abschluss des Lernprogramms benötigen Sie Folgendes:

+ Microsoft Visual Studio 2015.

+ Node.js Version 0.10.x oder höher.

+ Ein aktives Azure-Konto. (Wenn Sie kein Konto haben, können Sie ein [kostenloses Konto] erstellen[ lnk-free-trial] in nur ein paar Minuten.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-the-service-app"></a>Die Dienst-app erstellen

In diesem Abschnitt erstellen Sie eine Node.js Console-app, die das Ziel zugeordnet **MyDeviceId**Speicherort Metadaten hinzufügt. Dann Abfragen, dass der im Vergleich gespeichert im Hub, indem die Geräte in den USA, und klicken Sie dann auf diejenigen befindet sich die Berichterstattung einer mobilen Verbindungs.

1. Fügen Sie in Visual Studio ein Visual C#-herkömmlichen Windows-Desktop-Projekt in der aktuellen Lösung mithilfe der Projektvorlage **Console-Anwendung** . Nennen Sie das Projekt **AddTagsAndQuery**.

    ![Neue Visual c# herkömmlichen Windows-Desktop-Projekt][img-createapp]

2. Im Explorer-Lösung mit der rechten Maustaste in des Projekts **AddTagsAndQuery** , und klicken Sie dann auf **Nuget-Pakete verwalten**.

3. Im Fenster **Nuget-Paket-Manager** , stellen Sie sicher, dass **Vorabversion einschließen** aktiviert ist, **microsoft.azure.devices**suchen, wählen Sie aus, **Installieren** , Installieren der *die Vorabversion von der **Microsoft.Azure.Devices** * Packen und akzeptieren der Vereinbarung. Dieses Verfahren downloads, Installationen und fügt einen Verweis auf die [Microsoft Azure IoT Service SDK] [ lnk-nuget-service-sdk] Nuget-Paket und die zugehörigen Dateien.

    ![Fenster NuGet-Paket-Manager][img-servicenuget]

4. Fügen Sie den folgenden `using` Anweisungen am oberen Rand der Datei **Program.cs** :

        using Microsoft.Azure.Devices;

5. Fügen Sie die folgenden Felder in der Klasse **Programm** aus. Ersetzen Sie den Platzhalterwert mit der Verbindungszeichenfolge für den IoT-Hub an, den Sie im vorherigen Abschnitt erstellt haben.

        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";

6. Fügen Sie die folgende Methode zur Klasse **Programm** aus:

        public static async Task AddTagsAndQuery()
        {
            var twin = await registryManager.GetTwinAsync("myDeviceId");
            var patch =
                @"{
                    tags: {
                        location: {
                            region: 'US',
                            plant: 'Redmond43'
                        }
                    }
                }";
            await registryManager.UpdateTwinAsync(twin.DeviceId, patch, twin.ETag);

            var query = registryManager.CreateQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43'", 100);
            var twinsInRedmond43 = await query.GetNextAsTwinAsync();
            Console.WriteLine("Devices in Redmond43: {0}", string.Join(", ", twinsInRedmond43.Select(t => t.DeviceId)));

            query = registryManager.CreateQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43' AND properties.reported.connectivity.type = 'cellular'", 100);
            var twinsInRedmond43UsingCellular = await query.GetNextAsTwinAsync();
            Console.WriteLine("Devices in Redmond43 using cellular network: {0}", string.Join(", ", twinsInRedmond43UsingCellular.Select(t => t.DeviceId)));
        }

    Die **RegistryManager** -Klasse macht alle Methoden für die Interaktion mit dem Gerät im Vergleich vom Dienst erforderlich. Der vorherige Code initialisiert zuerst das **RegistryManager** -Objekt, und klicken Sie dann Ruft das Ziel für **MyDeviceId**und schließlich seine Tags mit den gewünschten Speicherortinformationen aktualisiert.

    Nach dem Aktualisieren, es werden zwei Abfragen ausgeführt: die erste wählt nur das Gerät im Vergleich von Geräten in **Redmond43** Pflanze ist, und das zweite passt die Abfrage aus, um nur die Geräte auszuwählen, die auch über das mobile Netzwerk verbunden sind.

    Beachten Sie, dass der vorherige Code, wenn sie das Objekt **Abfrage** erstellt eine maximale Anzahl von zurückgegebenen Dokumente angibt. Das **Abfrage** -Objekt enthält eine **HasMoreResults** boolesche Eigenschaft, die Sie verwenden können, um die **GetNextAsTwinAsync** Methoden mehrmals aufzurufen, um alle Ergebnisse abzurufen. Eine Methode namens **GetNextAsJson** ist für die Ergebnisse, die nicht Gerät im Vergleich zum Beispiel Resultate Aggregationsabfragen sind verfügbar.

7. Fügen Sie schließlich die folgenden Zeilen für die **Main** -Methode:

        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        AddTagsAndQuery().Wait();
        Console.WriteLine("Press Enter to exit.");
        Console.ReadLine();

8. Führen Sie diese Anwendung sollte, und es ein Gerät in die Ergebnisse für die Abfrage mit allen Geräten in **Redmond43** und keine für die Abfrage, die die Ergebnisse auf Geräte beschränkt, die einem Mobilfunknetz verwenden.

    ![Abfrageergebnisse im Fenster][img-addtagapp]

Im nächsten Abschnitt erstellen Sie eine Gerät-app, die die Informationen Connectivity-Berichte und ändert sich das Ergebnis der Abfrage im vorherigen Abschnitt.

## <a name="create-the-device-app"></a>Erstellen Sie die app Gerät

In diesem Abschnitt erstellen Sie eine Node.js Console-app, die eine Verbindung mit Ihrem Hub als **MyDeviceId**, und klicken Sie dann aktualisiert dessen Ziel des gemeldet Eigenschaften, die Daten enthalten, dass er mit einem Mobilfunknetz verbunden ist.

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

6. Jetzt, da das Gerät seine Informationen Connectivity gemeldet, sollten sie in beiden Abfragen angezeigt. Führen Sie die app .NET **AddTagsAndQuery** zum erneuten Ausführen die Abfragen. Dieses Mal **MyDeviceId** sollte beide Abfrageergebnisse angezeigt werden.

    ![][img-addtagapp2]

## <a name="next-steps"></a>Nächste Schritte
In diesem Lernprogramm so konfiguriert, dass einen neuen IoT Hub Azure-Portal, und klicken Sie dann in der Hub Identität Registrierung erstellt eine Gerät Identität. Sie Gerät Metadaten als Tags aus einer Back-End-Anwendung hinzugefügt und geschrieben haben eine app simulierten Gerät zu Bericht Gerät Connectivity Informationen in das Ziel Gerät. Außerdem haben Sie erfahren, wie Sie diese Informationen mithilfe der IoT Hub Abfragesprache SQL-ähnliche Abfragen.

Verwenden Sie die folgenden Ressourcen um zu erfahren Sie, wie Sie:

- werden von Geräten mit der [Erste Schritte mit IoT Hub] senden[ lnk-iothub-getstarted] Lernprogramm
- Konfigurieren von Geräten verwenden und die gewünschten Eigenschaften mit den [gewünschte Eigenschaften zum Konfigurieren von Geräten verwenden] [ lnk-twin-how-to-configure] Lernprogramm
- Steuern von Geräten interaktiv (z. B. einschalten ein Lüfter aus einer app benutzergesteuerte) mit den [direkten Methoden verwenden] [ lnk-methods-tutorial] Lernprogramm.

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-node-twin-getstarted/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-twin-getstarted/createnetapp.png
[img-addtagapp]: media/iot-hub-csharp-node-twin-getstarted/addtagapp.png
[img-addtagapp2]: media/iot-hub-csharp-node-twin-getstarted/addtagapp2.png

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/1.1.0-preview-004

[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-identity]: iot-hub-devguide-identity-registry.md

[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-methods-tutorial]: iot-hub-c2d-methods.md
[lnk-twin-how-to-configure]: iot-hub-csharp-node-twin-how-to-configure.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md


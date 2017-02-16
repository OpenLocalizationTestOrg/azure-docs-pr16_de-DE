<properties
    pageTitle="Azure IoT Hub für C#-erste Schritte | Microsoft Azure"
    description="Azure IoT Hub mit C#-erste Schritte Lernprogramm. Verwenden Sie Azure IoT Hub und c# mit Microsoft Azure IoT SDKs, um eine Lösung Internet der Dinge implementieren."
    services="iot-hub"
    documentationCenter=".net"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="dotnet"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/12/2016"
     ms.author="dobett"/>

# <a name="get-started-with-azure-iot-hub-for-net"></a>Erste Schritte mit Azure IoT Hub für .NET

[AZURE.INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Am Ende dieses Lernprogramms stehen Ihnen drei Windows-Konsole Applications aus:

* **CreateDeviceIdentity**, die einer Identität Gerät und zugehörigen Sicherheitsupdates-Taste, um die Verbindung Ihres Geräts simulierten erstellt.
* **ReadDeviceToCloudMessages**, die die von Ihrem Gerät simulierten gesendet werden angezeigt.
* **SimulatedDevice**, die eine Verbindung mit Ihrem IoT Hub mit der Identität des Geräts zuvor erstellten und sendet eine Nachricht werden pro Sekunde über das Protokoll AMQP.

> [AZURE.NOTE] Weitere Informationen zu den verschiedenen SDKs, die Sie verwenden können, um beide Applikationen auf Geräte und Ihre Lösung Back-End ausführen zu erstellen, finden Sie unter [IoT Hub SDKs][lnk-hub-sdks].

Damit dieses Lernprogramm abgeschlossen, benötigen Sie Folgendes:

+ Microsoft Visual Studio 2015.

+ Ein aktives Azure-Konto. (Wenn Sie kein Konto haben, können Sie ein [kostenloses Konto] erstellen[ lnk-free-trial] in nur ein paar Minuten.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Jetzt Ihre IoT Hub erstellt haben, und Sie haben die Hostname und Verbindung Zeichenfolge, die Sie im weiteren Verlauf dieses Lernprogramms ausführen müssen.

## <a name="create-a-device-identity"></a>Erstellen Sie eine Gerät Identität

In diesem Abschnitt erstellen Sie eine Windows-Console-app, die eine Identität Gerät in der Registrierung Identität Ihrer IoT Hub erstellt. Ein Gerät kann keine Verbindung IoT Hub hergestellt, es sei denn, sie einen Eintrag in der Registrierung des Geräts Identität gibt. Weitere Informationen finden Sie im Registrierungsabschnitt "Gerät Identität" des [IoT Hub Developer Guide][lnk-devguide-identity]. Wenn Sie diese app Konsole ausführen, wird eine eindeutige Geräte-ID und Ihr Gerät verwenden können, um sich zu identifizieren, wenn er Gerät-Cloud-Nachrichten an IoT Hub sendet-Taste generiert.

1. Fügen Sie in Visual Studio ein Visual C#-herkömmlichen Windows-Desktop-Projekt in der aktuellen Lösung mithilfe der Projektvorlage **Console-Anwendung** . Stellen Sie sicher, dass die .NET Framework-Version 4.5.1 ist oder höher. Nennen Sie das Projekt **CreateDeviceIdentity**.

    ![Neue Visual c# herkömmlichen Windows-Desktop-Projekt][10]

2. Im Explorer-Lösung mit der rechten Maustaste in des Projekts **CreateDeviceIdentity** , und klicken Sie dann auf **Nuget-Pakete verwalten**.

3. Im Fenster **Nuget-Paket-Manager** wählen Sie **Durchsuchen**, suchen Sie nach **microsoft.azure.devices**, wählen Sie zum Installieren des Pakets **Microsoft.Azure.Devices** **Installieren** und akzeptieren Sie der Vereinbarung. Dieses Verfahren downloads, Installationen und fügt einen Verweis auf die [Microsoft Azure IoT Service SDK] [ lnk-nuget-service-sdk] Nuget-Paket und die zugehörigen Dateien.

    ![Fenster NuGet-Paket-Manager][11]

4. Fügen Sie den folgenden `using` Anweisungen am oberen Rand der Datei **Program.cs** :

        using Microsoft.Azure.Devices;
        using Microsoft.Azure.Devices.Common.Exceptions;

5. Fügen Sie die folgenden Felder in der Klasse **Programm** aus. Ersetzen Sie den Platzhalterwert mit der Verbindungszeichenfolge für den IoT-Hub an, den Sie im vorherigen Abschnitt erstellt haben.

        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";

6. Fügen Sie die folgende Methode zur Klasse **Programm** aus:

        private static async Task AddDeviceAsync()
        {
            string deviceId = "myFirstDevice";
            Device device;
            try
            {
                device = await registryManager.AddDeviceAsync(new Device(deviceId));
            }
            catch (DeviceAlreadyExistsException)
            {
                device = await registryManager.GetDeviceAsync(deviceId);
            }
            Console.WriteLine("Generated device key: {0}", device.Authentication.SymmetricKey.PrimaryKey);
        }

    Diese Methode erstellt eine Gerät Identität mit ID **MyFirstDevice**. (Wenn die Geräte-ID in der Registrierung bereits vorhanden ist, ermittelt den Code einfach die vorhandenen Geräteinformationen.) Anschließend wird die app den Primärschlüssel für die Identität angezeigt. Verbindung zu Ihrem IoT-Hub verwenden Sie diesen Schlüssel in dem simulierten Gerät.

7. Fügen Sie schließlich die folgenden Zeilen für die **Main** -Methode:

        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        AddDeviceAsync().Wait();
        Console.ReadLine();

8. Führen Sie diese Anwendung, und notieren Sie die Taste Gerät.

    ![Von der Anwendung generierte Gerät-Taste][12]

> [AZURE.NOTE] Die IoT Hub Identität Registrierung speichert nur Identitäten Gerät, um den sicheren Zugriff auf den Hub ermöglichen. Es speichert Geräte-IDs und Schlüssel verwenden als Sicherheitsanmeldeinformationen und eine Kennzeichnung aktiviert/deaktiviert, die Sie verwenden können, um Zugriff für ein einzelnes Gerät zu deaktivieren. Wenn die Anwendung benötigt, um andere Geräte-spezifischen Metadaten zu speichern, sollte eine anwendungsspezifische Store verwendet werden. Weitere Informationen finden Sie unter [IoT Hub Developer Guide][lnk-devguide-identity].

## <a name="receive-device-to-cloud-messages"></a>Gerät-Cloud-Nachrichten erhalten

In diesem Abschnitt erstellen Sie eine Windows-Console-app, die liest Gerät-Cloud-Nachrichten über IoT-Hub an. Ein Hub IoT macht eine [Azure Ereignis Hubs][lnk-event-hubs-overview]-kompatiblen Endpunkt, sodass Sie Gerät-Cloud-Nachrichten lesen können. Um die Dinge einfach zu halten, erstellt in diesem Lernprogramm einen grundlegenden Reader, der für eine Bereitstellung von hohen Durchsatz nicht geeignet ist. Weitere Informationen zum Verarbeiten von Gerät-Cloud-Nachrichten bei finden Sie unter den [Prozess Gerät-Cloud - Nachrichten] [ lnk-process-d2c-tutorial] Lernprogramm. Weitere Informationen zum Verarbeiten von Nachrichten aus dem Ereignis Hubs, finden Sie unter der [Erste Schritte mit Ereignis Hubs] [ lnk-eventhubs-tutorial] Lernprogramm. (In diesem Lernprogramm ist die Endpunkte IoT Hub Ereignis Hub kompatiblen anwendbar).

> [AZURE.NOTE] Der Ereignis-Hub kompatiblen Endpunkt zum Lesen von Gerät Cloud-Nachrichten immer verwendet das AMQP-Protokoll.

1. Fügen Sie in Visual Studio ein Visual C#-herkömmlichen Windows-Desktop-Projekt in der aktuellen Lösung mithilfe der Projektvorlage **Console-Anwendung** . Stellen Sie sicher, dass die .NET Framework-Version 4.5.1 ist oder höher. Nennen Sie das Projekt **ReadDeviceToCloudMessages**.

    ![Neue Visual c# herkömmlichen Windows-Desktop-Projekt][10]

2. Im Explorer-Lösung mit der rechten Maustaste in des Projekts **ReadDeviceToCloudMessages** , und klicken Sie dann auf **Nuget-Pakete verwalten**.

3. Im Fenster **Nuget-Paket-Manager** suchen Sie nach **WindowsAzure.ServiceBus**, wählen Sie **Installieren**und akzeptieren Sie der Vereinbarung. Dieses Verfahren downloads, Installationen und fügt einen Verweis auf [Azure-Dienstbus][lnk-servicebus-nuget], mit sämtlichen abhängigen. Dieses Paket ermöglicht die Anwendung Verbindung zu den Ereignis-Hub kompatiblen Endpunkt auf Ihre IoT-Hub an.

4. Fügen Sie den folgenden `using` Anweisungen am oberen Rand der Datei **Program.cs** :

        using Microsoft.ServiceBus.Messaging;
        using System.Threading;

5. Fügen Sie die folgenden Felder in der Klasse **Programm** aus. Ersetzen Sie den Platzhalterwert mit der Verbindungszeichenfolge für den IoT-Hub an, die, den Sie im Abschnitt "Erstellen einer IoT Hub" erstellt haben.

        static string connectionString = "{iothub connection string}";
        static string iotHubD2cEndpoint = "messages/events";
        static EventHubClient eventHubClient;

6. Fügen Sie die folgende Methode zur Klasse **Programm** aus:

        private static async Task ReceiveMessagesFromDeviceAsync(string partition, CancellationToken ct)
        {
            var eventHubReceiver = eventHubClient.GetDefaultConsumerGroup().CreateReceiver(partition, DateTime.UtcNow);
            while (true)
            {
                if (ct.IsCancellationRequested) break;
                EventData eventData = await eventHubReceiver.ReceiveAsync();
                if (eventData == null) continue;

                string data = Encoding.UTF8.GetString(eventData.GetBytes());
                Console.WriteLine("Message received. Partition: {0} Data: '{1}'", partition, data);
            }
        }

    Diese Methode verwendet eine Instanz **EventHubReceiver** erhalten Nachrichten aus allen der IoT Hub Gerät-zu-Cloud erhalten Partitionen. Beachten Sie, wie Sie übergeben ein `DateTime.Now` Parameter beim Erstellen des Objekts **EventHubReceiver** so, dass er nur nach dem Start gesendete Nachrichten empfängt. Dieser Filter eignet sich in einer Umgebung für den aktuellen Satz von Nachrichten zu sehen. In einer Umgebung für die Herstellung sollten Code überprüfen, ob sie alle Nachrichten verarbeitet. Weitere Informationen finden Sie unter [wie IoT Hub Gerät-Cloud - Nachrichten verarbeitet] [ lnk-process-d2c-tutorial] Lernprogramm.

7. Fügen Sie schließlich die folgenden Zeilen für die **Main** -Methode:

        Console.WriteLine("Receive messages. Ctrl-C to exit.\n");
        eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, iotHubD2cEndpoint);

        var d2cPartitions = eventHubClient.GetRuntimeInformation().PartitionIds;

        CancellationTokenSource cts = new CancellationTokenSource();

        System.Console.CancelKeyPress += (s, e) =>
        {
          e.Cancel = true;
          cts.Cancel();
          Console.WriteLine("Exiting...");
        };

        var tasks = new List<Task>();
        foreach (string partition in d2cPartitions)
        {
           tasks.Add(ReceiveMessagesFromDeviceAsync(partition, cts.Token));
        }  
        Task.WaitAll(tasks.ToArray());

## <a name="create-a-simulated-device-app"></a>Erstellen Sie eine app simulierten Gerät

In diesem Abschnitt erstellen Sie eine Windows-Console-app, die ein Gerät simuliert, das Gerät-Cloud-Nachrichten an einen Hub IoT sendet.

1. Fügen Sie in Visual Studio ein Visual C#-herkömmlichen Windows-Desktop-Projekt in der aktuellen Lösung mithilfe der Projektvorlage **Console-Anwendung** . Stellen Sie sicher, dass die .NET Framework-Version 4.5.1 ist oder höher. Nennen Sie das Projekt **SimulatedDevice**.

    ![Neue Visual c# herkömmlichen Windows-Desktop-Projekt][10]

2. Im Explorer-Lösung mit der rechten Maustaste in des Projekts **SimulatedDevice** , und klicken Sie dann auf **Nuget-Pakete verwalten**.

3. Im Fenster **Nuget-Paket-Manager** wählen Sie **Durchsuchen**, suchen Sie nach **Microsoft.Azure.Devices.Client**, wählen Sie zum Installieren des Pakets **Microsoft.Azure.Devices.Client** **Installieren** und akzeptieren Sie der Vereinbarung. Dieses Verfahren downloads, Installationen und fügt einen Verweis auf die [Azure IoT - Gerät SDK Nuget-Paket] [ lnk-device-nuget] und die zugehörigen Dateien.

4. Fügen Sie den folgenden `using` Anweisung am oberen Rand der Datei **Program.cs** :

        using Microsoft.Azure.Devices.Client;
        using Newtonsoft.Json;

5. Fügen Sie die folgenden Felder in der Klasse **Programm** aus. Ersetzen durch die Platzhalterwerte mit der IoT Hub Hostname, die Sie im Abschnitt "Erstellen einen IoT Hub" abgerufen und die Taste Gerät, die im Abschnitt "Erstellen eine Identität Gerät" abgerufen.

        static DeviceClient deviceClient;
        static string iotHubUri = "{iot hub hostname}";
        static string deviceKey = "{device key}";

6. Fügen Sie die folgende Methode zur Klasse **Programm** aus:

        private static async void SendDeviceToCloudMessagesAsync()
        {
            double avgWindSpeed = 10; // m/s
            Random rand = new Random();

            while (true)
            {
                double currentWindSpeed = avgWindSpeed + rand.NextDouble() * 4 - 2;

                var telemetryDataPoint = new
                {
                    deviceId = "myFirstDevice",
                    windSpeed = currentWindSpeed
                };
                var messageString = JsonConvert.SerializeObject(telemetryDataPoint);
                var message = new Message(Encoding.ASCII.GetBytes(messageString));

                await deviceClient.SendEventAsync(message);
                Console.WriteLine("{0} > Sending message: {1}", DateTime.Now, messageString);

                Task.Delay(1000).Wait();
            }
        }

    Diese Methode sendet eine neue Gerät Cloud Nachricht pro Sekunde. Die Nachricht enthält ein JSON-serialisierten Objekt, mit der Geräte-ID und einer erzeugten Zufallszahl, um einen Wind Geschwindigkeit Sensor zu reproduzieren.

7. Fügen Sie schließlich die folgenden Zeilen für die **Main** -Methode:

        Console.WriteLine("Simulated device\n");
        deviceClient = DeviceClient.Create(iotHubUri, new DeviceAuthenticationWithRegistrySymmetricKey("myFirstDevice", deviceKey));

        SendDeviceToCloudMessagesAsync();
        Console.ReadLine();

  Standardmäßig erstellt die **Create** -Methode eine **DeviceClient** -Instanz, die das Protokoll AMQP zur Kommunikation mit IoT-Hub verwendet an. Wenn das HTTP-Protokoll verwenden möchten, verwenden Sie die Außerkraftsetzung der Methode **Erstellen** , die Sie das Protokoll angeben kann. Wenn Sie das HTTP-Protokoll verwenden, sollten Sie auch das **Microsoft.AspNet.WebApi.Client** Nuget-Paket zum Projekt, um den Namespace **System.Net.Http.Formatting** enthalten hinzufügen.

In diesem Lernprogramm führt Sie durch die Schritte zur Erstellung eines IoT Hub Gerät Clients. Sie können auch den [Dienst Azure IoT Hub angeschlossen] [ lnk-connected-service] Visual Studio-Erweiterung zu den erforderlichen Code zur Clientanwendung Gerät hinzufügen.


> [AZURE.NOTE] Zur Vereinfachung, implementiert keine "Wiederholen" Richtlinie in diesem Lernprogramm. Herstellung Code, sollten Sie "Wiederholen" Richtlinien (beispielsweise einer exponentiellen Backoff), wie im MSDN-Artikel [Behandeln von vorübergehenden Fehlerstrukturanalyse]vorgeschlagen implementieren[lnk-transient-faults].

## <a name="run-the-applications"></a>Führen Sie die Anwendung

Sie können nun zur Ausführung der Anwendung.

1.  In Visual Studio im Explorer-Lösung mit der rechten Maustaste Ihre Lösung, und klicken Sie dann auf **Start festlegen Projekte**. Wählen Sie **mehrere Startprojekte**aus, und **Wählen Sie dann als Aktion für die **ReadDeviceToCloudMessages** und die **SimulatedDevice** Projekte** .

    ![Project-Starteigenschaften][41]

2.  Drücken Sie **F5** , um beide ausgeführt apps zu starten. Die Ausgabe Console der app **SimulatedDevice** zeigt die Nachrichten, die Ihrem simulierte Gerät an Ihre IoT Hub sendet. Die Ausgabe Console der app **ReadDeviceToCloudMessages** zeigt die Nachrichten, die Ihre IoT Hub empfängt.

    ![Console Ausgabe von apps][42]

3. Die Kachel **Verwendung** im [Portal Azure] [ lnk-portal] zeigt die Anzahl der Nachrichten, die an den Hub gesendet:

    ![Azure Portals Verwendung-Kachel][43]


## <a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm so konfiguriert, dass einen IoT Hub Azure-Portal, und klicken Sie dann in der Hub Identität Registrierung erstellt eine Gerät Identität. Sie verwendet diese Identität Gerät, um die app simulierten Gerät Gerät-Cloud-Nachrichten an den Hub senden aktivieren. Sie auch eine app erstellt haben, die die vom Hub empfangenen Nachrichten angezeigt werden. 

Erste Schritte mit IoT Hub fortsetzen und anderen Szenarien IoT untersuchen, finden Sie unter:

- [Verbinden von Ihrem Gerät][lnk-connect-device]
- [Erste Schritte mit Gerät-Verwaltung][lnk-device-management]
- [Erste Schritte mit dem IoT Gateway SDK][lnk-gateway-SDK]

Erfahren Sie, wie Ihre Lösung IoT erweitern und Gerät-Cloud-Nachrichten bei verarbeiten, finden Sie unter den [Prozess Gerät-Cloud - Nachrichten] [ lnk-process-d2c-tutorial] Lernprogramm.

<!-- Images. -->
[41]: ./media/iot-hub-csharp-csharp-getstarted/run-apps1.png
[42]: ./media/iot-hub-csharp-csharp-getstarted/run-apps2.png
[43]: ./media/iot-hub-csharp-csharp-getstarted/usage.png
[10]: ./media/iot-hub-csharp-csharp-getstarted/create-identity-csharp1.png
[11]: ./media/iot-hub-csharp-csharp-getstarted/create-identity-csharp2.png
[12]: ./media/iot-hub-csharp-csharp-getstarted/create-identity-csharp3.png

<!-- Links -->
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-servicebus-nuget]: https://www.nuget.org/packages/WindowsAzure.ServiceBus
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-device-nuget]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-connected-service]: https://visualstudiogallery.msdn.microsoft.com/e254a3a5-d72e-488e-9bd3-8fee8e0cd1d6
[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/

<properties
    pageTitle="Senden von Nachrichten von Cloud-zu-Gerät mit IoT Hub | Microsoft Azure"
    description="Führen Sie dieses Lernprogramm erfahren Sie, wie Sie mit Azure IoT Hub mit C#-Cloud-zu-Gerät-Nachrichten senden."
    services="iot-hub"
    documentationCenter=".net"
    authors="fsautomata"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="dotnet"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="06/23/2016"
     ms.author="elioda"/>

# <a name="tutorial-how-to-send-cloud-to-device-messages-with-iot-hub-and-net"></a>Lernprogramm: Zum Senden von Nachrichten von Cloud-zu-Gerät mit IoT Hub und .net

[AZURE.INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a>Einführung

Azure IoT-Hub ist ein vollständig verwalteter Dienst, der hilft aktivieren zuverlässige und sichere bidirektionale Kommunikation zwischen mehreren Millionen IoT Geräte und eine Anwendung wieder zu beenden. Das [Erste Schritte mit IoT Hub] -Lernprogramm wird gezeigt, wie einen IoT Hub erstellen, eine Gerät Identität darin bereitstellen und Fehlercode ein simuliertes Gerät, das Gerät-Cloud-Nachrichten sendet.

In diesem Lernprogramm basiert auf [Erste Schritte mit IoT Hub]. Es wird gezeigt, wie an:

- Ihrer Anwendung Cloud Back-End senden Sie-Cloud-zu-Gerät-Nachrichten zu einem einzelnen Gerät über IoT Hub.
- Empfangen Sie Cloud-zu-Gerät auf einem Gerät.
- Aus der Cloud Anwendung back-End, Übermittlung Bestätigung (*Feedback*) für von IoT-Hub auf einem Gerät gesendeten Nachrichten anfordern.

Finden Sie weitere Informationen in der Cloud-zu-Gerät-Nachrichten, die [IoT Hub Developer Guide][IoT Hub Developer Guide - C2D].

Führen Sie zwei Windows-Konsole Applications am Ende dieses Lernprogramms:

* **SimulatedDevice**, eine geänderte Version der app in [den ersten Schritten mit IoT Hub], die eine Verbindung mit Ihrem IoT Hub und empfängt Nachrichten, die Cloud-zu-Gerät erstellt.
* **SendCloudToDevice**, der eine Nachricht Cloud-zu-Gerät mit dem simulierten Gerät über IoT Hub sendet, und klicken Sie dann erhält deren Übermittlung Bestätigung.

> [AZURE.NOTE] IoT Hub enthält SDK-Unterstützung für zahlreiche Plattformen und Sprachen (einschließlich C, Java und Javascript) bis Azure IoT Geräte-SDKs. Eine schrittweise Anleitung zum Herstellen einer Verbindung Ihr Gerät in diesem Lernprogramm den Code und im Allgemeinen Azure IoT Hub finden Sie unter der [Azure IoT Developer Center].

Damit dieses Lernprogramm abgeschlossen, benötigen Sie Folgendes:

+ Microsoft Visual Studio 2015

+ Ein aktives Azure-Konto. (Wenn Sie kein Konto haben, können Sie ein [kostenloses Konto] erstellen[ lnk-free-trial] in nur ein paar Minuten.)

## <a name="receive-messages-on-the-simulated-device"></a>Empfangen von Nachrichten auf dem simulierten Gerät

In diesem Abschnitt, ändern Sie die simulierten Gerät Anwendung Sie in der Cloud-zu-Gerät Nachrichten vom Hub IoT erhalten [Erste Schritte mit IoT Hub] erstellt haben.

1. Fügen Sie die folgende Methode in Visual Studio im Projekt **SimulatedDevice** der Klasse **Programm** .

        private static async void ReceiveC2dAsync()
        {
            Console.WriteLine("\nReceiving cloud to device messages from service");
            while (true)
            {
                Message receivedMessage = await deviceClient.ReceiveAsync();
                if (receivedMessage == null) continue;

                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received message: {0}", Encoding.ASCII.GetString(receivedMessage.GetBytes()));
                Console.ResetColor();

                await deviceClient.CompleteAsync(receivedMessage);
            }
        }

    Die `ReceiveAsync` Methode gibt asynchrone empfangene Nachricht zu dem Zeitpunkt an, die sie vom Gerät erhält. Es gibt *null* nach Ablauf eines Timeout festzulegen (in diesem Fall die Standardeinstellung von einer Minute wird verwendet). In diesem Fall sollte der Code neue Nachrichten warten. Dies ist der Grund für die `if (receivedMessage == null) continue` Linie.

    Den Anruf an `CompleteAsync()` benachrichtigt Sie IoT Hub, dass die Nachricht erfolgreich verarbeitet wurde. Die Nachricht kann aus der Gerätewarteschlange entfernt werden. Wenn ein Fehler ist, der verhindert, dass die app Gerät die Verarbeitung der Nachricht durchführen aufgetreten, übermittelt IoT Hub erneut. Unbedingt dann Nachricht Verarbeitungslogik in der app Gerät *Idempotent*, sein, damit die gleiche Nachricht mehrmals empfängt das gleiche Ergebnis erzeugt. Eine Anwendung kann auch vorübergehend eine Nachricht aufgeben die Ergebnisse in der Nachricht in der Warteschlange für zukünftige Ernährung Aufbewahrung IoT-Hub. Oder die Anwendung eine Nachricht, die die Nachricht endgültig aus der Warteschlange entfernt ablehnen kann. Weitere Informationen zum Lebenszyklus Cloud-zu-Gerät Nachricht finden Sie unter der [IoT Hub Developer Guide][IoT Hub Developer Guide - C2D].

    > [AZURE.NOTE] Bei Verwendung von HTTP statt MQTT oder AMQP als einen Transport, die `ReceiveAsync` Methode sofort zurückgegeben. Das unterstützte Muster für Cloud-zu-Gerät Nachrichten mit HTTP ist zeitweise verbundenen Geräte, das Kontrollkästchen für Nachrichten selten (weniger als 25 Minuten). Weitere HTTP ausgeben empfängt Ergebnisse IoT Hub begrenzungsebene die Anfragen. Weitere Informationen über die Unterschiede zwischen MQTT, AMQP und HTTP-Support und IoT Hub begrenzungsebene finden Sie unter der [IoT Hub Developer Guide][IoT Hub Developer Guide - C2D].

2. Fügen Sie die folgende Methode in der Methode **Main** unmittelbar vor der `Console.ReadLine()` Zeile:

        ReceiveC2dAsync();

> [AZURE.NOTE] Aus Gründen der Vereinfachung implementiert dieses Lernprogramms keine Richtlinie "Wiederholen". Herstellung Code sollten Sie "Wiederholen" Richtlinien zur Verfügung (z. B. exponentiellen Backoff), wie im MSDN-Artikel [Behandeln von vorübergehenden Fehlerstrukturanalyse]vorgeschlagen implementieren.

## <a name="send-a-cloud-to-device-message"></a>Senden einer Nachricht Cloud-zu-Gerät

In diesem Abschnitt werden Sie eine Windows-Console-app schreiben, in die Cloud-zu-Gerät Nachrichten bei der app simulierten Gerät sendet.

1. Erstellen Sie ein neues Visual C#-Desktop-App-Projekt in der aktuellen Visual Studio-Lösung mithilfe der Projektvorlage **Console-Anwendung** . Nennen Sie das Projekt **SendCloudToDevice**.

    ![Neues Projekt in Visual Studio][20]

2. Im Lösung-Explorer mit der rechten Maustaste in der Lösung, und klicken Sie dann auf **NuGet-Pakete verwalten, für die Lösung...**. 

    Daraufhin wird das Fenster **NuGet-Pakete verwalten** .

3. Suchen Sie nach `Microsoft Azure Devices`, klicken Sie auf **Installieren**und akzeptieren der Vereinbarung. 

    Diese downloads, Installationen und fügt einen Verweis auf die [Azure IoT - Dienst SDK NuGet-Paket].

4. Fügen Sie den folgenden `using` Anweisung am oberen Rand der Datei **Program.cs** :

        using Microsoft.Azure.Devices;

5. Fügen Sie die folgenden Felder in der Klasse **Programm** aus. Ersetzen Sie den Platzhalterwert mit der IoT Hub Verbindungszeichenfolge aus der [Erste Schritte mit IoT-Hub]an:

        static ServiceClient serviceClient;
        static string connectionString = "{iot hub connection string}";

6. Fügen Sie die folgende Methode zur Klasse **Programm** aus:

        private async static Task SendCloudToDeviceMessageAsync()
        {
            var commandMessage = new Message(Encoding.ASCII.GetBytes("Cloud to device message."));
            await serviceClient.SendAsync("myFirstDevice", commandMessage);
        }

    Diese Methode sendet eine neue Cloud-zu-Gerät-Nachricht an das Gerät mit der ID `myFirstDevice`. Ändern Sie diesen Parameter entsprechend, falls Sie es aus in die [Erste Schritte mit IoT Hub]verwendet nicht geändert.

7. Fügen Sie schließlich die folgenden Zeilen für die **Main** -Methode:

        Console.WriteLine("Send Cloud-to-Device message\n");
        serviceClient = ServiceClient.CreateFromConnectionString(connectionString);

        Console.WriteLine("Press any key to send a C2D message.");
        Console.ReadLine();
        SendCloudToDeviceMessageAsync().Wait();
        Console.ReadLine();

8. In Visual Studio mit der rechten Maustaste Ihre Lösung und wählen Sie **Start festlegen Projekte...**aus. Wählen Sie **mehrere Startprojekte**, und wählen Sie die Aktion **beginnen** für **ProcessDeviceToCloudMessages**, **SimulatedDevice**und **SendCloudToDevice**.

9.  Drücken Sie **F5**. Alle drei Programme sollten beginnen. Aktivieren Sie das Fenster **SendCloudToDevice** , und drücken Sie die **EINGABETASTE**. Die von der app simulierten empfangene Nachricht sollte angezeigt werden.

    ![App empfangen Nachricht][21]

## <a name="receive-delivery-feedback"></a>Übermittlung Feedback erhalten
Es ist möglich, Anforderung Übermittlung (oder Ablauf) Empfangsbestätigungen aus IoT Hub für jede Nachricht Cloud-zu-Gerät. Dadurch werden die Cloud Back-End "Wiederholen" oder Vergütungsstufe Logik einfach zu informieren. Weitere Informationen zu Cloud-zu-Gerät Feedback, finden Sie unter der [IoT Hub Developer Guide][IoT Hub Developer Guide - C2D].

In diesem Abschnitt ändern Sie die app **SendCloudToDevice** um Feedback anfordern, und erhalten es aus IoT Hub.

1. Fügen Sie die folgende Methode in Visual Studio im Projekt **SendCloudToDevice** der Klasse **Programm** .
   
        private async static void ReceiveFeedbackAsync()
        {
            var feedbackReceiver = serviceClient.GetFeedbackReceiver();

            Console.WriteLine("\nReceiving c2d feedback from service");
            while (true)
            {
                var feedbackBatch = await feedbackReceiver.ReceiveAsync();
                if (feedbackBatch == null) continue;

                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received feedback: {0}", string.Join(", ", feedbackBatch.Records.Select(f => f.StatusCode)));
                Console.ResetColor();

                await feedbackReceiver.CompleteAsync(feedbackBatch);
            }
        }

    Beachten Sie, dass das Muster empfangen am gleichen zum Cloud-zu-Gerät empfangen von Nachrichten aus der app Gerät geeignet ist.

2. Fügen Sie die folgende Methode in der Methode **Main** direkt nach der `serviceClient = ServiceClient.CreateFromConnectionString(connectionString)` Zeile:

        ReceiveFeedbackAsync();

3. Um Feedback für die Übermittlung der Nachricht Cloud-zu-Gerät anfordern zu können, müssen Sie eine Eigenschaft in der Methode **SendCloudToDeviceMessageAsync** angeben. Fügen Sie die folgende Zeile, direkt nach der `var commandMessage = new Message(...);` Zeile:

        commandMessage.Ack = DeliveryAcknowledgement.Full;

4.  Führen Sie die apps durch Drücken von **F5**. Alle drei Programme starten sollte angezeigt werden. Aktivieren Sie das Fenster **SendCloudToDevice** , und drücken Sie die **EINGABETASTE**. Es sollte die empfangene Nachricht von der app simulierten und nach ein paar Sekunden, die von der Anwendung **SendCloudToDevice** empfangene Feedback Nachricht angezeigt.

    ![App empfangen Nachricht][22]

> [AZURE.NOTE] Aus Gründen der Vereinfachung implementiert dieses Lernprogramms keine Richtlinie "Wiederholen". Herstellung Code sollten Sie "Wiederholen" Richtlinien zur Verfügung (z. B. exponentiellen Backoff), wie im MSDN-Artikel [Behandeln von vorübergehenden Fehlerstrukturanalyse]vorgeschlagen implementieren.

## <a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm haben Sie wie Cloud-zu-Gerät-Nachrichten senden und empfangen. 

Beispiele für umfassende End-to-End-Lösung, die IoT Hub verwenden, finden Sie unter [Azure IoT Suite].

Wenn Sie weitere Informationen zur Entwicklung von Lösungen mit IoT Hub finden Sie im [IoT Hub Developer Guide].

<!-- Images -->
[20]: ./media/iot-hub-csharp-csharp-c2d/create-identity-csharp1.png
[21]: ./media/iot-hub-csharp-csharp-c2d/sendc2d1.png
[22]: ./media/iot-hub-csharp-csharp-c2d/sendc2d2.png

<!-- Links -->

[Azure IoT - Dienst SDK NuGet-Paket]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[Vorübergehende Fehlerbehandlung]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[IoT Hub Developer Guide - C2D]: iot-hub-devguide-messaging.md

[IoT Hub Developer Guide]: iot-hub-devguide.md
[Erste Schritte mit IoT-Hub]: iot-hub-csharp-csharp-getstarted.md
[Azure IoT-Entwicklercenter]: http://www.azure.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[Azure IoT Suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
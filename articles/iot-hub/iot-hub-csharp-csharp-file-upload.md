<properties
    pageTitle="Hochladen von Dateien von Geräten mit IoT Hub | Microsoft Azure"
    description="Führen Sie dieses Lernprogramm erfahren Sie, wie Sie zum Hochladen von Dateien von Geräten mit Azure IoT Hub mit C#."
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
     ms.date="06/21/2016"
     ms.author="elioda"/>

# <a name="tutorial-how-to-upload-files-from-devices-to-the-cloud-with-iot-hub"></a>Lernprogramm: Zum Hochladen von Dateien von Geräten in der Cloud mit IoT-Hub

## <a name="introduction"></a>Einführung

Azure IoT-Hub ist ein vollständig verwalteter Dienst, der zuverlässigen ermöglicht, und sichere bidirektionale Kommunikation zwischen mehreren Millionen IoT Geräte und eine Anwendung back-End. Vorherigen Lernprogramme ([Erste Schritte mit IoT Hub] und [Senden von Nachrichten von Cloud-zu-Gerät mit IoT Hub]) veranschaulichen der grundlegenden Gerät-zu-Cloud und messaging Cloud-zu-Gerät-Funktionen IoT Hub und den [Prozess Gerät-Cloud-Nachrichten] Lernprogramm beschreibt eine Möglichkeit zum Gerät-Cloud-Nachrichten zuverlässig in Azure Blob-Speicher zu speichern. In einigen Szenarios können nicht Sie jedoch ganz einfach die Daten zuordnen, die Ihren Geräten in der relativ kleinen Gerät-Cloud-Nachrichten senden, die IoT Hub akzeptiert. Beispiele für große Dateien, die Bilder, Videos, Vibration Daten mit großer Häufigkeit Stichprobe enthalten oder die einige Form von vorverarbeiteten Daten enthalten. Diese Dateien werden in der Regel Stapel in der Cloud mithilfe von Tools wie [Azure Data Factory] oder den Stapel [Hadoop] verarbeitet. Wenn Dateiuploads auf einem Gerät zum Senden von Ereignissen bevorzugte sind, können Sie weiterhin IoT Hub Sicherheit und Zuverlässigkeit Funktionen verwenden.

In diesem Lernprogramm basiert auf den Code, in der [Cloud-zu-Gerät Nachrichten mit IoT Hub senden] Lernprogramm zu zeigen, wie Sie die Datei hochladen-Funktionen von IoT Hub verwenden. Es wird gezeigt, wie sicher Bereitstellen von einem Gerät mit einer Azure BLOB-URI für das Hochladen einer Datei, und wie Sie die IoT Hub Datei hochladen Benachrichtigungen zum Auslösen Verarbeiten der Datei in Ihrer app Back-End.

Am Ende dieses Lernprogramms führen Sie zwei Windows-Konsole Applications aus:

* **SimulatedDevice**, eine geänderte Version der app in die [Cloud-zu-Gerät Nachrichten mit IoT Hub senden] Lernprogramm, das eine Datei hochgeladen wird ein SAS URI von Ihrem IoT Hub bereitgestellten mit dem Speicher erstellt.
* **ReadFileUploadNotification**, die Datei hochladen Benachrichtigungen über Ihre IoT Hub empfängt.

> [AZURE.NOTE] IoT Hub unterstützt viele Geräteplattformen und Sprachen (einschließlich C, Java und Javascript) bis Azure IoT Gerät SDKs. Schlagen Sie in der [Azure IoT Developer Center] für Schritt-Anleitung zum Verbinden des Codes erzielt werden in diesem Lernprogramm und im Allgemeinen Azure IoT Hub von Ihrem Geräts.

Um dieses Lernprogramms abgeschlossen haben, benötigen Sie Folgendes:

+ Microsoft Visual Studio 2015

+ Ein aktives Azure-Konto. (Wenn Sie kein Konto haben, können Sie ein [kostenloses Konto] erstellen[ lnk-free-trial] in nur ein paar Minuten.)

## <a name="associate-an-azure-storage-account-to-iot-hub"></a>Zuordnen einer Firma Azure-Speicher an IoT Verteiler

Da das simulierte Gerät in ein Blob eine Datei hochgeladen wird, müssen Sie ein [Azure-Speicher] -Konto, IoT Hub zugeordnet ist verfügen. Wenn Sie ein Konto Azure-Speicher mit einem IoT-Hub zuordnen, kann im Hub einen SAS URI generieren, die sichere Hochladen einer Datei zu einem Container Blob ein Gerät verwenden können. Der Dienst IoT Hub und den Geräte-SDKs koordinieren den Prozess, der URI SAS generiert und an ein Gerät verwenden, eine Datei hochzuladen bereitgestellt.

Folgen Sie den Anweisungen in [Konfigurieren von Dateiuploads Azure-Portal mit] [ lnk-configure-upload] , ein Konto Azure-Speicher an Ihre IoT Verteiler zugeordnet werden soll.

## <a name="upload-a-file-from-a-simulated-device"></a>Hochladen einer Datei auf einem simulierten Gerät

In diesem Abschnitt, ändern Sie die Anwendung simulierten Gerät Sie erstellt haben, in die Cloud-zu-Gerät Nachrichten aus dem IoT Hub empfangen [Cloud-zu-Gerät Nachrichten mit IoT Hub zu senden] .

1. In Visual Studio mit der rechten Maustaste in des Projekts **SimulatedDevice** , klicken Sie auf **Hinzufügen**, und klicken Sie dann auf **Vorhandenes Element**. Navigieren Sie zu der Bilddatei, und gehören Sie es in einem Projekt. In diesem Lernprogramm wird davon ausgegangen, ist das Bild mit dem Namen `image.jpg`.

2. Mit der rechten Maustaste auf das Bild, und klicken Sie dann auf **Eigenschaften**. Stellen Sie sicher, dass **immer kopieren** **Kopieren in die Ausgabeverzeichnis** festgelegt ist.

    ![][1]

3. Fügen Sie in der Datei **Program.cs** die folgenden Aussagen am oberen Rand der Datei ein:

        using System.IO;

4. Fügen Sie die folgende Methode zur Klasse **Programm** aus:
         
        private static async void SendToBlobAsync()
        {
            string fileName = "image.jpg";
            Console.WriteLine("Uploading file: {0}", fileName);
            var watch = System.Diagnostics.Stopwatch.StartNew();

            using (var sourceData = new FileStream(@"image.jpg", FileMode.Open))
            {
                await deviceClient.UploadToBlobAsync(fileName, sourceData);
            }

            watch.Stop();
            Console.WriteLine("Time to upload file: {0}ms\n", watch.ElapsedMilliseconds);
        }

    Die `UploadToBlobAsync` Methode in der Datei Stream und die Quelle der Datei hochgeladen werden akzeptiert und Uploads auf Speicher verarbeitet. Die Console-Anwendung zeigt die Zeit, die benötigt wird, um die Datei hochzuladen.

5. Fügen Sie die folgende Methode in der Methode **Main** unmittelbar vor der `Console.ReadLine()` Zeile:

        SendToBlobAsync();

> [AZURE.NOTE] Aus Gründen der Vereinfachung implementiert dieses Lernprogramms keine Richtlinie "Wiederholen". Herstellung Code sollten Sie "Wiederholen" Richtlinien zur Verfügung (z. B. exponentiellen Backoff), wie im MSDN-Artikel [Behandeln von vorübergehenden Fehlerstrukturanalyse]vorgeschlagen implementieren.

## <a name="receive-a-file-upload-notification"></a>Erhalten Sie eine Benachrichtigung der Datei hochladen

In diesem Abschnitt schreiben Sie eine Windows-Console-app, die Datei hochladen Benachrichtigungsnachrichten aus IoT Hub empfängt.

1. Erstellen Sie ein neues Visual C#-Windows-Projekt mithilfe der Projektvorlage **Console-Anwendung** , in der aktuellen Visual Studio-Lösung. Nennen Sie das Projekt **ReadFileUploadNotification**.

    ![Neues Projekt in Visual Studio][2]

2. Im Explorer-Lösung mit der rechten Maustaste in des Projekts **ReadFileUploadNotification** , und klicken Sie dann auf **NuGet-Pakete verwalten**.

    Das Fenster NuGet-Pakete verwalten wird angezeigt.

2. Suchen Sie nach `Microsoft.Azure.Devices`, klicken Sie auf **Installieren**und akzeptieren der Vereinbarung. 

    Diese downloads, Installationen und fügt einen Verweis auf die [Azure IoT - Dienst SDK NuGet-Paket] im Projekt **ReadFileUploadNotification** hinzu.

3. Fügen Sie in der Datei **Program.cs** die folgenden Aussagen am oberen Rand der Datei ein:

        using Microsoft.Azure.Devices;

4. Fügen Sie die folgenden Felder in der Klasse **Programm** aus. Ersetzen Sie den Platzhalterwert mit der IoT Hub Verbindungszeichenfolge aus der [Erste Schritte mit IoT-Hub]an:

        static ServiceClient serviceClient;
        static string connectionString = "{iot hub connection string}";
        
5. Fügen Sie die folgende Methode zur Klasse **Programm** aus:
   
        private async static Task ReceiveFileUploadNotificationAsync()
        {
            var notificationReceiver = serviceClient.GetFileNotificationReceiver();

            Console.WriteLine("\nReceiving file upload notification from service");
            while (true)
            {
                var fileUploadNotification = await notificationReceiver.ReceiveAsync();
                if (fileUploadNotification == null) continue;

                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received file upload noticiation: {0}", string.Join(", ", fileUploadNotification.BlobName));
                Console.ResetColor();

                await notificationReceiver.CompleteAsync(fileUploadNotification);
            }
        }

    Beachten Sie, dass das Muster empfangen am gleichen zum Cloud-zu-Gerät empfangen von Nachrichten aus der app Gerät geeignet ist.

6. Fügen Sie schließlich die folgenden Zeilen für die **Main** -Methode:

        Console.WriteLine("Receive file upload notifications\n");
        serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
        ReceiveFileUploadNotificationAsync().Wait();
        Console.ReadLine();

## <a name="run-the-applications"></a>Führen Sie die Anwendung

Jetzt sind Sie bereit sind, der die Anwendung läuft.

1. Klicken Sie in Visual Studio mit der rechten Maustaste Ihre Lösung, und wählen Sie **Start festlegen Projekte**. Wählen Sie **mehrere Startprojekte**, und wählen Sie die Aktion **beginnen** für **ReadFileUploadNotification** und **SimulatedDevice**.

2. Drücken Sie **F5**. Beide Applikationen sollte beginnen. Es sollte das Hochladen in eine Console-app und die Upload-Nachricht empfangen von der anderen Console-app abgeschlossen angezeigt. Der [Azure-Portal] oder Visual Studio-Server-Explorer können Sie Ihr Konto Azure-Speicher für das Vorhandensein der hochzuladenden Datei einchecken.

  ![][50]


## <a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm haben Sie gelernt, wie Sie die Datei hochladen-Funktionen von IoT Hub zu vereinfachen, Dateiuploads von Geräten nutzen können. Sie können weiterhin IoT Hub Features und Szenarien mit den folgenden Artikeln:

- [Erstellen Sie einen Hub IoT programmgesteuert][lnk-create-hub]
- [Einführung in C SDK][lnk-c-sdk]
- [IoT Hub SDKs][lnk-sdks]

Die Funktionen von IoT Hub weiteren kann, finden Sie unter:

- [Ein Gerät mit dem IoT Gateway SDK simulieren][lnk-gateway]

<!-- Images. -->

[50]: ./media/iot-hub-csharp-csharp-file-upload/run-apps1.png
[1]: ./media/iot-hub-csharp-csharp-file-upload/image-properties.png
[2]: ./media/iot-hub-csharp-csharp-file-upload/create-identity-csharp1.png

<!-- Links -->

[Azure-portal]: https://portal.azure.com/

[Factory Azure-Daten]: https://azure.microsoft.com/documentation/services/data-factory/
[Hadoop]: https://azure.microsoft.com/documentation/services/hdinsight/

[Senden Sie Cloud-zu-Gerät Nachrichten mit IoT Hub]: iot-hub-csharp-csharp-c2d.md
[Prozess Gerät-Cloud-Nachrichten]: iot-hub-csharp-csharp-process-d2c.md
[Erste Schritte mit IoT-Hub]: iot-hub-csharp-csharp-getstarted.md
[Azure IoT-Entwicklercenter]: http://www.azure.com/develop/iot

[Vorübergehende Fehlerbehandlung]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure-Speicher]: ../storage/storage-create-storage-account.md#create-a-storage-account
[lnk-configure-upload]: iot-hub-configure-file-upload.md
[Azure IoT - Dienst SDK NuGet-Paket]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md



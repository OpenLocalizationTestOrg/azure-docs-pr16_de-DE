<properties
    pageTitle="Erste Schritte mit Blob-Speicher und Visual Studio verbunden Services (WebJob Projekte) | Microsoft Azure"
    description="Erste Schritte mit Blob-Speicher in einem Projekt WebJob nach dem Herstellen einer Verbindung mit einer Azure-Speicher mit Visual Studio verbunden Services."
    services="storage"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="web"
    ms.tgt_pltfrm="vs-getting-started"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/18/2016"
    ms.author="tarcher"/>

# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-webjob-projects"></a>Erste Schritte mit Azure Blob-Speicher und Visual Studio verbunden Services (WebJob Projekte)

[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>(Übersicht)

Dieser Artikel enthält C#-Codebeispielen, die zeigen, wie ein Vorgang ausgelöst, wenn eine Azure Blob erstellt oder aktualisiert wird. Die Codebeispielen verwenden Sie die [WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk.md) Version 1.x. Wenn Sie ein Speicherkonto zu einem Projekt WebJob hinzufügen, über das Dialogfeld Visual Studio **Verbunden Dienste hinzufügen** , das entsprechende Azure-Speicher NuGet-Paket installiert ist, werden die entsprechenden .NET Verweise zum Projekt hinzugefügt, und Verbindungszeichenfolgen für das Speicherkonto in der App aktualisiert.



## <a name="how-to-trigger-a-function-when-a-blob-is-created-or-updated"></a>Wie eine Funktion ausgelöst, wenn ein Blob erstellt oder aktualisiert wird

In diesem Abschnitt wird gezeigt, wie das Attribut **BlobTrigger** verwendet wird.

 **Hinweis:** Das WebJobs SDK scannt Protokolldateien neue oder geänderte Blobs zu überwachen. Hierbei handelt es sich langsam; eine Funktion möglicherweise nicht bis einige Minuten oder mehr ausgelöst, nachdem das Blob erstellt wurde.  Wenn die Anwendung Blobs sofort zu verarbeiten muss, ist die empfohlene Methode zum Erstellen einer Warteschlange-Nachricht, wenn Sie das Blob erstellen und verwenden das Attribut [QueueTrigger](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#trigger) anstelle der **BlobTrigger** Attribut auf die Funktion, die zur Verarbeitung des BLOBs.

### <a name="single-placeholder-for-blob-name-with-extension"></a>Einzelne Platzhalter für Blob-Namen mit der Erweiterung  

Im folgenden Beispiel kopiert Text Blobs, die im Container *Eingabewerte* über den Container *Ausgabe* angezeigt werden:

        public static void CopyBlob([BlobTrigger("input/{name}")] TextReader input,
            [Blob("output/{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

Attributkonstruktors hat einen Parameter, der den Containernamen und einen Platzhalter für den Namen Blob angibt. In diesem Beispiel in die *Eingabewerte* Container ein Blob mit dem Namen *Blob1.txt* erstellt wurde, erstellt die Funktion einen Blob mit dem Namen *Blob1.txt* in der *Ausgabe* Container.

Sie können mit dem Namen Blob Platzhalter, ein Namensmuster angeben, wie im folgenden Beispiel gezeigt:

        public static void CopyBlob([BlobTrigger("input/original-{name}")] TextReader input,
            [Blob("output/copy-{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

Dieser Code wird nur Blobs, deren Namen mit "ursprünglichen-" Anfang kopiert. *Original-Blob1.txt* im Container *Eingabewerte* wird beispielsweise zum *Kopieren-Blob1.txt* im Container *Ausgabe* kopiert.

Wenn Sie ein Namensmuster für Blob-Namen angeben, die geschweifte Klammern in den Namen aufweisen müssen, doppelklicken Sie die geschweiften Klammern ein. Wenn Sie Blobs im Container *Bilder* , deren Namen wie folgt suchen möchten beispielsweise:

        {20140101}-soundfile.mp3

Verwenden Sie diese Option für das Muster:

        images/{{20140101}}-{name}

Im Beispiel wäre *der Platzhalterwert* *soundfile.mp3*.

### <a name="separate-blob-name-and-extension-placeholders"></a>Separate Blob-Namen und die Erweiterung Platzhaltern

Im folgenden Beispiel wird die Erweiterung ihn Blobs beim Kopieren, die im Container *Eingabewerte* über den Container *Ausgabe* angezeigt werden. Der Code die Erweiterung des *Eingabewerte* Blob protokolliert und die Erweiterung des Blob *Ausgabe* auf *txt*festgelegt.

        public static void CopyBlobToTxtFile([BlobTrigger("input/{name}.{ext}")] TextReader input,
            [Blob("output/{name}.txt")] out string output,
            string name,
            string ext,
            TextWriter logger)
        {
            logger.WriteLine("Blob name:" + name);
            logger.WriteLine("Blob extension:" + ext);
            output = input.ReadToEnd();
        }

## <a name="types-that-you-can-bind-to-blobs"></a>Dateitypen, die Sie Blobs gebunden werden können

Sie können das Attribut **BlobTrigger** auf die folgenden Arten:

* **Zeichenfolge**
* **TextReader**
* **Stream**
* **ICloudBlob**
* **CloudBlockBlob**
* **CloudPageBlob**
* Andere Arten von [ICloudBlobStreamBinder](#getting-serialized-blob-content-by-using-icloudblobstreambinder) deserialisiert

Wenn Sie direkt mit dem Konto Azure-Speicher arbeiten möchten, können Sie auch **CloudStorageAccount** Parameter der Methode Signatur hinzufügen.

## <a name="getting-text-blob-content-by-binding-to-string"></a>Erste Blob Textinhalten von Bindung Zeichenfolge

Wenn Text Blobs erwartet werden, können **BlobTrigger** auf **einen Parameter** angewendet werden. Im folgenden Beispiel bindet einen Blob Text an **einen Parameter mit dem Namen **LogMessage**** . Die Funktion verwendet die Parameter der Inhalt des Blob zum Dashboard WebJobs SDK geschrieben.

        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name,
            TextWriter logger)
        {
             logger.WriteLine("Blob name: {0}", name);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }

## <a name="getting-serialized-blob-content-by-using-icloudblobstreambinder"></a>Erste BLOB-Inhalt mithilfe von ICloudBlobStreamBinder serialisiert

Im folgenden Beispiel wird eine Klasse, **ICloudBlobStreamBinder** , um das Attribut **BlobTrigger** einen Blob in den Typ **WebImage** binden aktivieren implementiert verwendet.

        public static void WaterMark(
            [BlobTrigger("images3/{name}")] WebImage input,
            [Blob("images3-watermarked/{name}")] out WebImage output)
        {
            output = input.AddTextWatermark("WebJobs SDK",
                horizontalAlign: "Center", verticalAlign: "Middle",
                fontSize: 48, opacity: 50);
        }
        public static void Resize(
            [BlobTrigger("images3-watermarked/{name}")] WebImage input,
            [Blob("images3-resized/{name}")] out WebImage output)
        {
            var width = 180;
            var height = Convert.ToInt32(input.Height * 180 / input.Width);
            output = input.Resize(width, height);
        }

Der **WebImage** Bindung Code wird in einer **WebImageBinder** Klasse bereitgestellt, die von **ICloudBlobStreamBinder**abgeleitet wird.

        public class WebImageBinder : ICloudBlobStreamBinder<WebImage>
        {
            public Task<WebImage> ReadFromStreamAsync(Stream input,
                System.Threading.CancellationToken cancellationToken)
            {
                return Task.FromResult<WebImage>(new WebImage(input));
            }
            public Task WriteToStreamAsync(WebImage value, Stream output,
                System.Threading.CancellationToken cancellationToken)
            {
                var bytes = value.GetBytes();
                return output.WriteAsync(bytes, 0, bytes.Length, cancellationToken);
            }
        }

## <a name="how-to-handle-poison-blobs"></a>Wie beschädigte Blobs behandelt.

Wenn eine Funktion **BlobTrigger** fehlschlägt, wird aufgerufen SDK erneut, falls aufgrund einer vorübergehenden Fehler fehlschlägt wurde. Wenn der Fehler aufgrund des Inhalts der Blob verursacht wird, schlägt die Funktion jedes Mal, wenn sie versucht, das Blob zu verarbeiten. Standardmäßig ruft das SDK eine Funktion bis zu 5 Mal für einen angegebenen Blob. Wenn das fünfte testen fehlschlägt, fügt das SDK eine Nachricht an eine Warteschlange mit dem Namen *Webjobs-Blobtrigger-Poison*an.

Lässt sich die maximale Anzahl der Wiederholungsversuche. Die gleiche [MaxDequeueCount](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#configqueue) -Einstellung wird für beschädigte Blob Handhabung und Nachrichtenbehandlung beschädigte Warteschlange verwendet.

Die Nachricht Warteschlange für nicht verarbeitete Nachrichten Blobs ist ein JSON-Objekt, das die folgenden Eigenschaften enthält:

* FunctionId (im Format *{WebJob Name}*. Funktionen. *{Funktionsname}*, z. B.: WebJob1.Functions.CopyBlob)
* BlobType ("BlockBlob" oder "PageBlob")
* ContainerName
* BlobName
* ETag (Blob Version Bezeichner enthält, beispielsweise: "0x8D1DC6E70A277EF")

Im folgenden Beispiel enthält die Funktion **CopyBlob** Code, der bewirkt, dass es zum Fehlschlagen jedes Mal, wenn sie aufgerufen wird. Nachdem das SDK für die maximale Anzahl der Wiederholungsversuche aufgerufen, wird eine Nachricht in der Warteschlange beschädigte Blob erstellt, und diese Nachricht von der Funktion **LogPoisonBlob** verarbeitet wird.

        public static void CopyBlob([BlobTrigger("input/{name}")] TextReader input,
            [Blob("textblobs/output-{name}")] out string output)
        {
            throw new Exception("Exception for testing poison blob handling");
            output = input.ReadToEnd();
        }

        public static void LogPoisonBlob(
        [QueueTrigger("webjobs-blobtrigger-poison")] PoisonBlobMessage message,
            TextWriter logger)
        {
            logger.WriteLine("FunctionId: {0}", message.FunctionId);
            logger.WriteLine("BlobType: {0}", message.BlobType);
            logger.WriteLine("ContainerName: {0}", message.ContainerName);
            logger.WriteLine("BlobName: {0}", message.BlobName);
            logger.WriteLine("ETag: {0}", message.ETag);
        }

Das SDK deserialisiert automatisch die JSON-Nachricht. Hier ist die **PoisonBlobMessage** -Klasse:

        public class PoisonBlobMessage
        {
            public string FunctionId { get; set; }
            public string BlobType { get; set; }
            public string ContainerName { get; set; }
            public string BlobName { get; set; }
            public string ETag { get; set; }
        }

### <a name="blob-polling-algorithm"></a>BLOB Umfragen Algorithmus

Das WebJobs SDK scannt alle Container von **BlobTrigger** Attributen beim Starten der Anwendung angegeben. In einem großen Speicherkonto kann diese Überprüfung einige Zeit in Anspruch nehmen, sodass es möglicherweise eine Weile sein, bevor neue Blobs gefunden werden und **BlobTrigger** Funktionen ausgeführt werden.

Um neue oder geänderte Blobs nach dem Start der Anwendung zu erkennen, liest das SDK regelmäßig aus den BLOB-Speicher Protokollen. Die Protokolle Blob gepuffert werden und nur erste physisch alle 10 Minuten geschrieben oder Ja, sodass es möglicherweise erheblichen Verzögerung nach ein Blob erstellt oder werden, bevor Sie das entsprechende aktualisiert **BlobTrigger** -Funktion ausgeführt wird.

Es gibt eine Ausnahme für Blobs, die Sie mit dem Attribut **Blob** erstellen. Wenn das WebJobs SDK einen neuen Blob erstellt, übergibt diese das neue Blob sofort an alle übereinstimmenden **BlobTrigger** Funktionen. Daher, wenn Sie eine Kette von Blob Eingaben und Ausgaben verfügen, kann das SDK sie effizient verarbeiten. Wenn niedrigen Wartezeit Ihrer Verarbeitung Funktionen für Blobs, die erstellt oder anderweitig aktualisiert Blob ausgeführt werden soll, wir empfehlen jedoch **QueueTrigger** statt **BlobTrigger**verwenden.

### <a name="blob-receipts"></a>BLOB-übermittlungsbestätigungen

Das WebJobs SDK stellt sicher, dass keine **BlobTrigger** -Funktion für das gleiche neue oder aktualisierte Blob mehrmals aufgerufen wird. Dies geschieht durch *Blob Belege* verwalten, um festzustellen, ob eine Version des angegebenen Blob verarbeitet wurde.

BLOB Belege sind in einem Container mit dem Namen *Azure-Webjobs-Hosts* im angegebenen durch die Verbindungszeichenfolge AzureWebJobsStorage Azure-Speicherkonto gespeichert. Bestätigung Blob weist die folgende Informationen:

* Die Funktion, die für das Blob ("*{WebJob Name}*. aufgerufen wurde Funktionen. *{Funktionsname}*", beispielsweise:"WebJob1.Functions.CopyBlob")
* Der Containername
* Der BLOB-Typ ("BlockBlob" oder "PageBlob")
* Der Blob-name
* Das ETag (Blob Version Bezeichner enthält, beispielsweise: "0x8D1DC6E70A277EF")

Wenn Sie beim erneuten Verarbeiten eines Blob erzwingen möchten, können Sie das Blob-Bestätigung dieser Blob manuell aus dem *Azure-Webjobs-Hosts* Container löschen.

## <a name="related-topics-covered-by-the-queues-article"></a>Verwandte Themen durch Warteschlangen Artikel

Informationen zur Behandlung von Blob-Verarbeitung durch eine Nachricht Warteschlange ausgelöst oder für WebJobs SDK finden Sie unter Szenarien nicht spezifisch BLOB-Verarbeitung, [Azure Warteschlangenspeicher mit dem WebJobs SDK verwenden](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md).

Die folgenden: Verwandte Themen in diesem Artikel behandelt

* Asynchrone Funktionen
* Mehrere Instanzen
* Sicheres war(en)
* Verwenden Sie WebJobs SDK Attribute in den Textkörper einer Funktion
* Legen Sie die SDK Verbindungszeichenfolgen in Code ein.
* Festlegen der Werte für WebJobs SDK Parameter in code
* Konfigurieren von **MaxDequeueCount** für die Behandlung von beschädigte Blob.
* Eine Funktion manuell auslösen
* Schreiben von Protokollen

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel hat Codebeispielen bereitgestellt, die veranschaulichen, allgemeine Szenarien für das Arbeiten mit Azure Blobs zu behandeln. Weitere Informationen zum Verwenden von Azure WebJobs und das WebJobs SDK finden Sie unter [Azure WebJobs Dokumentationsressourcen](http://go.microsoft.com/fwlink/?linkid=390226).

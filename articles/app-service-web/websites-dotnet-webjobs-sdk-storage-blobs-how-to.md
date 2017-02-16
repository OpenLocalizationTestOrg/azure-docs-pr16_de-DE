<properties 
    pageTitle="Verwenden von Azure Blob-Speicher mit dem WebJobs SDK" 
    description="Informationen Sie zur Verwendung von Azure Blob-Speicher mit dem SDK WebJobs. Auslösen eines Prozesses, wenn ein neuer Blob in einem Container angezeigt wird, und 'beschädigte Blobs' behandeln." 
    services="app-service\web, storage" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="06/01/2016" 
    ms.author="tdykstra"/>

# <a name="how-to-use-azure-blob-storage-with-the-webjobs-sdk"></a>Verwenden von Azure Blob-Speicher mit dem WebJobs SDK

## <a name="overview"></a>(Übersicht)

Dieses Handbuch bietet C#-Codebeispielen, die zeigen, wie ein Vorgang ausgelöst, wenn eine Azure Blob erstellt oder aktualisiert wird. Verwenden Sie die Codebeispielen [WebJobs SDK](websites-dotnet-webjobs-sdk.md) -Version 1.x.

Für Codebeispielen, die zum Erstellen von Blobs finden Sie unter [Verwenden von Azure Warteschlangenspeicher mit dem SDK WebJobs](websites-dotnet-webjobs-sdk-storage-queues-how-to.md)anzeigen. 
        
Das Handbuch wird davon ausgegangen, Sie wissen, [wie ein WebJob-Projekt in Visual Studio mit Verbindungszeichenfolgen erstellen, die mit Ihrem Speicherkonto zeigen,](websites-dotnet-webjobs-sdk-get-started.md) oder [mehrere Speicherkonten](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).

## <a name="a-idtriggera-how-to-trigger-a-function-when-a-blob-is-created-or-updated"></a><a id="trigger"></a>Wie eine Funktion ausgelöst, wenn ein Blob erstellt oder aktualisiert wird

In diesem Abschnitt veranschaulicht, wie die `BlobTrigger` Attribut. 

> [AZURE.NOTE] Das WebJobs SDK scannt Protokolldateien neue oder geänderte Blobs zu überwachen. Dieses Verfahren ist nicht in Echtzeit; eine Funktion möglicherweise nicht bis einige Minuten oder mehr ausgelöst, nachdem das Blob erstellt wurde. Darüber hinaus [Speicher, die Protokolle, klicken Sie auf "beste steigern erstellt werden"](https://msdn.microsoft.com/library/azure/hh343262.aspx) Basis; Es gibt keine Garantie, dass alle Ereignisse erfasst werden. Klicken Sie unter bestimmten Umständen möglicherweise Protokolle verloren gehen. Wenn die Geschwindigkeit und Zuverlässigkeit Schwächen Blob Trigger nicht für die Anwendung gültig sind, wird empfohlen, eine Warteschlange-Nachricht zu erstellen, wenn Sie das Blob erstellen und verwenden Sie das Attribut [QueueTrigger](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#trigger) statt der `BlobTrigger` Attribut für die Funktion zur Verarbeitung des BLOBs.

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

## <a name="a-idtypesa-types-that-you-can-bind-to-blobs"></a><a id="types"></a>Dateitypen, die Sie Blobs gebunden werden können

Sie können die `BlobTrigger` Attribut für die folgenden Arten:

* `string`
* `TextReader`
* `Stream`
* `ICloudBlob`
* `CloudBlockBlob`
* `CloudPageBlob`
* `CloudBlobContainer`
* `CloudBlobDirectory`
* `IEnumerable<CloudBlockBlob>`
* `IEnumerable<CloudPageBlob>`
* Andere Arten von [ICloudBlobStreamBinder](#icbsb) deserialisiert 

Wenn Sie direkt mit dem Konto Azure-Speicher arbeiten möchten, können Sie auch Hinzufügen einer `CloudStorageAccount` Parameter der Signatur der Methode.

Beispiele finden Sie unter den [Blob Bindung Code im Repository Azure-Webjobs-Sdk auf GitHub.com](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/BlobBindingEndToEndTests.cs).

## <a name="a-idstringa-getting-text-blob-content-by-binding-to-string"></a><a id="string"></a>Erste Blob Textinhalten von Bindung Zeichenfolge

Wenn Text Blobs erwartet werden, `BlobTrigger` angewendet werden können, um eine `string` Parameter. Im folgenden Beispiel bindet einen Blob Text zu einer `string` Parameter mit dem Namen `logMessage`. Die Funktion verwendet die Parameter der Inhalt des Blob zum Dashboard WebJobs SDK geschrieben. 
 
        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name, 
            TextWriter logger)
        {
             logger.WriteLine("Blob name: {0}", name);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }

## <a name="a-idicbsba-getting-serialized-blob-content-by-using-icloudblobstreambinder"></a><a id="icbsb"></a>Erste BLOB-Inhalt mithilfe von ICloudBlobStreamBinder serialisiert

Im folgenden Beispiel wird eine Klasse, implementiert verwendet `ICloudBlobStreamBinder` Aktivieren der `BlobTrigger` Attribut Blob, Binden der `WebImage` Typ.

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

Die `WebImage` Bindung Code finden Sie unter einem `WebImageBinder` abgeleitete `ICloudBlobStreamBinder`.

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

## <a name="getting-the-blob-path-for-the-triggering-blob"></a>Beim Abrufen des Blob-Pfads für die auslösenden blob

Zum Abrufen der Containername und Blob-Name des Blob, die die Funktion ausgelöst wurde, beziehen Sie eine `blobTrigger` Zeichenfolge Parameter in der Signatur (Funktion).

        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name,
            string blobTrigger,
            TextWriter logger)
        {
             logger.WriteLine("Full blob path: {0}", blobTrigger);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }


## <a name="a-idpoisona-how-to-handle-poison-blobs"></a><a id="poison"></a>Wie beschädigte Blobs behandelt.

Wenn eine `BlobTrigger` Funktion fehlschlägt, das SDK ruft sie erneut, falls aufgrund einer vorübergehenden Fehler fehlschlägt wurde. Wenn der Fehler aufgrund des Inhalts der Blob verursacht wird, schlägt die Funktion jedes Mal, wenn sie versucht, das Blob zu verarbeiten. Standardmäßig ruft das SDK eine Funktion bis zu 5 Mal für einen angegebenen Blob. Wenn die fünfte testen fehlschlägt, fügt das SDK eine Nachricht an eine Warteschlange mit dem Namen *Webjobs-Blobtrigger-Poison*an.

Lässt sich die maximale Anzahl der Wiederholungsversuche. Die gleiche [MaxDequeueCount](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#configqueue) -Einstellung wird für beschädigte Blob Handhabung und Nachrichtenbehandlung beschädigte Warteschlange verwendet. 

Die Nachricht Warteschlange für nicht verarbeitete Nachrichten Blobs ist ein JSON-Objekt, das die folgenden Eigenschaften enthält:

* FunctionId (im Format *{WebJob Name}*. Funktionen. *{Funktionsname}*, z. B.: WebJob1.Functions.CopyBlob)
* BlobType ("BlockBlob" oder "PageBlob")
* ContainerName
* BlobName
* ETag (Blob Version Bezeichner enthält, beispielsweise: "0x8D1DC6E70A277EF")

Im folgenden Beispiel die `CopyBlob` Funktion enthält Code, der bewirkt, dass es zum Fehlschlagen jedes Mal, wenn sie aufgerufen wird. Nachdem das SDK sie für die maximale Anzahl der Wiederholungsversuche Ruft, eine Nachricht in der Warteschlange beschädigte Blob erstellt wird und diese Nachricht verarbeiteten der `LogPoisonBlob` (Funktion). 

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

Das SDK deserialisiert automatisch die JSON-Nachricht. Hier finden Sie die `PoisonBlobMessage` Klasse: 

        public class PoisonBlobMessage
        {
            public string FunctionId { get; set; }
            public string BlobType { get; set; }
            public string ContainerName { get; set; }
            public string BlobName { get; set; }
            public string ETag { get; set; }
        }

### <a name="a-idpollinga-blob-polling-algorithm"></a><a id="polling"></a>BLOB Umfragen Algorithmus

Das WebJobs SDK scannt alle Container, die vom angegebenen `BlobTrigger` Attribute beim Starten der Anwendung. In einem großen Speicherkonto dieser Scan, damit es eine Weile vor dem neuen Blobs gefunden werden möglicherweise einige Zeit dauern kann und `BlobTrigger` Funktionen ausgeführt werden.

Um neue oder geänderte Blobs nach dem Start der Anwendung zu erkennen, liest das SDK regelmäßig aus den BLOB-Speicher Protokollen. Die Protokolle Blob gepuffert werden, und nur erste physisch alle 10 Minuten geschrieben oder Ja, damit es möglicherweise erheblichen Verzögerung nach ein Blob erstellt oder werden, bevor Sie das entsprechende aktualisiert `BlobTrigger` Funktion wird ausgeführt. 

Es wird eine Ausnahme für Blobs, die Sie erstellen, indem Sie mit der `Blob` Attribut. Wenn das WebJobs SDK einen neuen Blob erstellt, übergibt er das neue Blob sofort in einer beliebigen passende `BlobTrigger` Funktionen. Daher, wenn Sie eine Kette von Blob Eingaben und Ausgaben verfügen, kann das SDK sie effizient verarbeiten. Wenn niedrigen Wartezeit Ausführen Ihrer Blob Funktionen zum Verarbeiten für Blobs, die erstellt oder anderweitig aktualisiert werden soll, verwenden wir empfehlen jedoch `QueueTrigger` sondern `BlobTrigger`.

### <a name="a-idreceiptsa-blob-receipts"></a><a id="receipts"></a>BLOB-übermittlungsbestätigungen

Das WebJobs SDK stellt sicher, dass keine `BlobTrigger` Funktion für das gleiche neue oder aktualisierte Blob mehrmals aufgerufen wird. Dies geschieht durch *Blob Belege* verwalten, um festzustellen, ob eine Version des angegebenen Blob verarbeitet wurde.

BLOB Belege sind in einem Container mit dem Namen *Azure-Webjobs-Hosts* im angegebenen durch die Verbindungszeichenfolge AzureWebJobsStorage Azure-Speicherkonto gespeichert. Bestätigung Blob weist die folgende Informationen:

* Die Funktion, die für das Blob ("*{WebJob Name}*. aufgerufen wurde Funktionen. *{Funktionsname}*", beispielsweise:"WebJob1.Functions.CopyBlob")
* Der Containername
* Der BLOB-Typ ("BlockBlob" oder "PageBlob")
* Der Blob-name
* Das ETag (Blob Version Bezeichner enthält, beispielsweise: "0x8D1DC6E70A277EF")

Wenn Sie beim erneuten Verarbeiten eines Blob erzwingen möchten, können Sie das Blob-Bestätigung dieser Blob manuell aus dem *Azure-Webjobs-Hosts* Container löschen.

## <a name="a-idqueuesarelated-topics-covered-by-the-queues-article"></a><a id="queues"></a>Verwandte Themen durch Warteschlangen Artikel

Informationen zur Behandlung von Blob-Verarbeitung durch eine Nachricht Warteschlange ausgelöst oder für WebJobs SDK finden Sie unter Szenarien nicht spezifisch BLOB-Verarbeitung, [Azure Warteschlangenspeicher mit dem WebJobs SDK verwenden](websites-dotnet-webjobs-sdk-storage-queues-how-to.md). 

Die folgenden: Verwandte Themen in diesem Artikel behandelt

* Asynchrone Funktionen
* Mehrere Instanzen
* Sicheres war(en)
* Verwenden Sie WebJobs SDK Attribute in den Textkörper einer Funktion
* Legen Sie die SDK Verbindungszeichenfolgen in Code ein.
* Festlegen der Werte für WebJobs SDK Parameter in code
* Konfigurieren von `MaxDequeueCount` für die Behandlung von beschädigte Blob.
* Eine Funktion manuell auslösen
* Schreiben von Protokollen

## <a name="a-idnextstepsa-next-steps"></a><a id="nextsteps"></a>Nächste Schritte

In diesem Handbuch weist Codebeispielen bereitgestellt, die veranschaulichen, allgemeine Szenarien für das Arbeiten mit Azure Blobs zu behandeln. Weitere Informationen zum Verwenden von Azure WebJobs und das WebJobs SDK finden Sie unter [Azure WebJobs empfohlen Ressourcen](http://go.microsoft.com/fwlink/?linkid=390226).
 

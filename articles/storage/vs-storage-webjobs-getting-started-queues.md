<properties
    pageTitle="Erste Schritte mit Warteschlangenspeicher und Visual Studio verbunden Services (WebJob Projekte) | Microsoft Azure"
    description="Erste Schritte mit Azure Warteschlange-Speicher in einem Projekt WebJob nach dem Herstellen einer Verbindung mit einem Speicherkonto mithilfe von Visual Studio verbunden Services."
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

# <a name="getting-started-with-azure-queue-storage-and-visual-studio-connected-services-webjob-projects"></a>Erste Schritte mit Azure Warteschlange-Speicher und Visual Studio verbunden Services (WebJob Projekte)

[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>(Übersicht)

Dieser Artikel beschreibt, wie die ersten Schritte mit Azure Warteschlange-Speicher in einem Projekt Visual Studio Azure WebJob, nachdem Sie erstellt oder auf die verwiesen wird ein Konto Azure-Speicher mithilfe des Dialogfelds Visual Studio **Verbunden Dienste hinzufügen** . Wenn Sie ein Speicherkonto zu einem Projekt WebJob hinzufügen, über das Dialogfeld Visual Studio **Verbunden Dienste hinzufügen** , die entsprechenden Azure-Speicher NuGet Pakete installiert sind, werden die entsprechenden .NET Verweise zum Projekt hinzugefügt und Verbindungszeichenfolgen für das Speicherkonto in der App aktualisiert werden.  

Dieser Artikel enthält C#-Codebeispielen, die veranschaulichen, wie die Azure WebJobs SDK-Version mit der Warteschlange Azure-Speicherdienst 1.x.

Azure Warteschlange-Speicher ist ein Dienst für das Speichern einer großen Anzahl von Nachrichten, die über eine beliebige Stelle in der Welt über authentifizierten Anrufe mit HTTP oder HTTPS zugegriffen werden können. Eine einzelne Warteschlange Nachricht kann bis zu 64 KB sein, und eine Warteschlange kann Millionen von Nachrichten, auf die gesamte Kapazität Beschränkung eines Kontos Speicher enthalten. Weitere Informationen finden Sie unter [Erste Schritte mit Azure Warteschlangenspeicher mit .NET](storage-dotnet-how-to-use-queues.md) . Weitere Informationen zu ASP.NET finden Sie unter [ASP.NET](http://www.asp.net).



## <a name="how-to-trigger-a-function-when-a-queue-message-is-received"></a>Wie eine Funktion ausgelöst, wenn eine Warteschlange-Nachricht empfangen wird

Um eine Funktion zu schreiben, die das WebJobs SDK Anrufe, wenn eine Warteschlange-Nachricht empfangen wird, verwenden Sie das Attribut **QueueTrigger** aus. Der Attributkonstruktor hat einen Parameter, der angibt, den Namen der Warteschlange abgefragt werden soll. Informationen, wie der Name der Warteschlange dynamisches festlegen möchten, aktivieren, [wie Sie festlegen](#how-to-set-configuration-options).

### <a name="string-queue-messages"></a>Zeichenfolge Warteschlangennachrichten

Im folgenden Beispiel enthält die Warteschlange eine Zeichenfolgennachricht **QueueTrigger** angewendet wird, um einen Parameter mit dem Namen **LogMessage** , das den Inhalt der Nachricht Warteschlange enthält. Die Funktion, [schreibt eine Protokoll Meldung zum Dashboard](#how-to-write-logs)ist.


        public static void ProcessQueueMessage([QueueTrigger("logqueue")] string logMessage, TextWriter logger)
        {
            logger.WriteLine(logMessage);
        }

Als **Zeichenfolge**möglicherweise der Parameter ein Byte-Array, ein **CloudQueueMessage** -Objekt oder eine POCO, die Sie definieren.

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a>POCO [(schlicht alten CLR-Objekt](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) in die Warteschlange Nachrichten

Im folgenden Beispiel enthält die Nachricht Warteschlange JSON für ein **BlobInformation** -Objekt, das eine Eigenschaft **BlobName** enthält. Das SDK deserialisiert automatisch das Objekt aus.

        public static void WriteLogPOCO([QueueTrigger("logqueue")] BlobInformation blobInfo, TextWriter logger)
        {
            logger.WriteLine("Queue message refers to blob: " + blobInfo.BlobName);
        }

Das SDK verwendet das [Newtonsoft.Json NuGet-Paket](http://www.nuget.org/packages/Newtonsoft.Json) serialisiert und deserialisiert Nachrichten an. Wenn Sie Nachrichten in Warteschlange in einem Programm, in denen das WebJobs SDK verwendet wird nicht erstellen, können Sie Schreiben von Code wie im folgenden Beispiel eine POCO Warteschlange Nachricht zu erstellen, die das SDK analysiert werden kann.

        BlobInformation blobInfo = new BlobInformation() { BlobName = "log.txt" };
        var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
        logQueue.AddMessage(queueMessage);

### <a name="async-functions"></a>Asynchrone Funktionen

Die folgenden asynchronen Funktion [schreibt ein Protokoll zum Dashboard](#how-to-write-logs).

        public async static Task ProcessQueueMessageAsync([QueueTrigger("logqueue")] string logMessage, TextWriter logger)
        {
            await logger.WriteLineAsync(logMessage);
        }

Asynchrone Funktionen können ein [Abbruchtoken](http://www.asp.net/mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4#CancelToken), dauern, wie im folgenden Beispiel dargestellt, die einen Blob kopiert. (Eine Erläuterung der **QueueTrigger** Platzhalter, finden Sie im Abschnitt [Blobs](#how-to-read-and-write-blobs-and-tables-while-processing-a-queue-message) .)

        public async static Task ProcessQueueMessageAsyncCancellationToken(
            [QueueTrigger("blobcopyqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}",FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new",FileAccess.Write)] Stream blobOutput,
            CancellationToken token)
        {
            await blobInput.CopyToAsync(blobOutput, 4096, token);
        }

## <a name="types-the-queuetrigger-attribute-works-with"></a>Typen funktioniert das Attribut QueueTrigger mit

Sie können **QueueTrigger** mit den folgenden Arten:

* **Zeichenfolge**
* Eine als JSON serialisiert POCO-Datentyp
* **Byte]**
* **CloudQueueMessage**

## <a name="polling-algorithm"></a>Umfragen Algorithmus

Das SDK implementiert einen zufälligen exponentiellen Back-off Algorithmus zum Verringern des Effekts im Leerlauf Warteschlange auf Kosten für Speicher Transaktion abrufen.  Wenn eine Nachricht nicht gefunden wird, wird das SDK wartet zwei Sekunden und sucht anschließend nach einer anderen Nachricht; Wenn keine Meldung gefunden wird, wartet er ungefähr vier Sekunden vor dem erneuten Versuch. Nach nachfolgende Versuche eine Warteschlange-Nachricht erhalten wird die Wartezeit weiterhin erhöhen, bis die maximale Wartezeit, das in eine Minute um übernommen erreicht. [Die maximale Wartezeit kann konfiguriert werden](#how-to-set-configuration-options).

## <a name="multiple-instances"></a>Mehrere Instanzen

Wenn für mehrere Instanzen Web app ausgeführt wird, ein fortlaufender WebJobs, die auf jedem Computer ausgeführt wird und jedem Computer warten auf Trigger und versucht, die Funktionen ausführen. In einigen Szenarien, die für einige Funktionen, die dieselben Daten zweimal Verarbeitung zu, also Funktionen Idempotent sein (geschrieben, damit mehrfach aufgerufen, mit der gleichen Eingabedaten doppelte Ergebnissen führen nicht).  

## <a name="parallel-execution"></a>Parallele Ausführung

Wenn Sie mehrere Funktionen, die unterschiedliche Warteschlange abhören verfügen, ruft das SDK sie parallel Wenn Nachrichten gleichzeitig empfangen werden.

Dasselbe gilt, wenn mehrere Nachrichten für eine einzelne Warteschlange empfangen werden. Standardmäßig wird das SDK erhält eine Reihe von 16 Warteschlangennachrichten nacheinander und führt die Funktion, die sie parallel verarbeitet. [Lässt sich die Stapelgröße](#how-to-set-configuration-options). Wenn die Zahl verarbeiteten Hälfte der Stapelgröße erreicht ab, wird das SDK Ruft einen anderen Blattnamen und startet die Verarbeitung von Nachrichten. Daher ist die maximale Anzahl der pro Funktion verarbeiteten Nachrichten gleichzeitig eine und eine Hälfte Mal Stapel so groß. Dieser Grenzwert gilt separat für jede Funktion, die ein **QueueTrigger** Attribut enthält. Wenn Sie keine parallele Ausführung für Nachrichten, die auf eine Warteschlange empfangen möchten, legen Sie die Stapelgröße auf 1.

## <a name="get-queue-or-queue-message-metadata"></a>Abrufen von Warteschlange oder Warteschlange Nachrichtenmetadaten

Sie können die folgenden Nachrichteneigenschaften erhalten, durch Hinzufügen von Parametern zu der Signatur der Methode:

* **DateTimeOffset** expirationTime
* **DateTimeOffset** insertionTime
* **DateTimeOffset** nextVisibleTime
* **Zeichenfolge** QueueTrigger (Nachrichtentext enthält)
* **String** -id
* **Zeichenfolge** popReceipt
* **Int** dequeueCount

Wenn Sie direkt mit den Azure-Speicher-API arbeiten möchten, können Sie auch **CloudStorageAccount** Parameter hinzufügen.

Im folgende Beispiel schreibt all dieser Metadaten in einem Informationen Anwendungsprotokoll. Im Beispiel enthalten sowohl LogMessage und QueueTrigger den Inhalt der Nachricht in der Warteschlange einfügen.

        public static void WriteLog([QueueTrigger("logqueue")] string logMessage,
            DateTimeOffset expirationTime,
            DateTimeOffset insertionTime,
            DateTimeOffset nextVisibleTime,
            string id,
            string popReceipt,
            int dequeueCount,
            string queueTrigger,
            CloudStorageAccount cloudStorageAccount,
            TextWriter logger)
        {
            logger.WriteLine(
                "logMessage={0}\n" +
            "expirationTime={1}\ninsertionTime={2}\n" +
                "nextVisibleTime={3}\n" +
                "id={4}\npopReceipt={5}\ndequeueCount={6}\n" +
                "queue endpoint={7} queueTrigger={8}",
                logMessage, expirationTime,
                insertionTime,
                nextVisibleTime, id,
                popReceipt, dequeueCount,
                cloudStorageAccount.QueueEndpoint,
                queueTrigger);
        }

Hier ist ein Beispiel für Protokoll von der Stichprobe Code geschrieben:

        logMessage=Hello world!
        expirationTime=10/14/2014 10:31:04 PM +00:00
        insertionTime=10/7/2014 10:31:04 PM +00:00
        nextVisibleTime=10/7/2014 10:41:23 PM +00:00
        id=262e49cd-26d3-4303-ae88-33baf8796d91
        popReceipt=AgAAAAMAAAAAAAAAfc9H0n/izwE=
        dequeueCount=1
        queue endpoint=https://contosoads.queue.core.windows.net/
        queueTrigger=Hello world!

## <a name="graceful-shutdown"></a>Sicheres war(en)

Eine Funktion, die in einer zusammenhängenden WebJob ausgeführt wird, kann **CancellationToken** Parameter übernehmen ermöglicht das Betriebssystem, um die Funktion zu benachrichtigen, wenn die WebJob gerade beendet werden soll. Sie können diese Benachrichtigung verwenden, um sicherzustellen, dass die Funktion unerwartete Beenden auf eine Weise nicht, die Daten in einem inkonsistenten Zustand belässt.

Im folgenden Beispiel wird gezeigt, wie bevorstehende WebJob Beendigung in einer Funktion überprüfen.

    public static void GracefulShutdownDemo(
                [QueueTrigger("inputqueue")] string inputText,
                TextWriter logger,
                CancellationToken token)
    {
        for (int i = 0; i < 100; i++)
        {
            if (token.IsCancellationRequested)
            {
                logger.WriteLine("Function was cancelled at iteration {0}", i);
                break;
            }
            Thread.Sleep(1000);
            logger.WriteLine("Normal processing for queue message={0}", inputText);
        }
    }

**Hinweis:** Das Dashboard möglicherweise nicht ordnungsgemäß angezeigt werden, den Status und die Ausgabe der Funktionen, die beendet wurden.

Weitere Informationen finden Sie unter [WebJobs ordnungsgemäß war(en)](http://blog.amitapple.com/post/2014/05/webjobs-graceful-shutdown/#.VCt1GXl0wpR).   

## <a name="how-to-create-a-queue-message-while-processing-a-queue-message"></a>So erstellen Sie eine Nachricht Warteschlange beim Verarbeiten einer Nachricht Warteschlange

Um eine Funktion zu schreiben, die eine neue Warteschlange-Nachricht erstellt wird, verwenden Sie das Attribut **Warteschlange** aus. Wie **QueueTrigger**Sie den Namen der Warteschlange als Zeichenfolge übergeben, oder Sie können [den Namen der Warteschlange dynamisch festlegen](#how-to-set-configuration-options).

### <a name="string-queue-messages"></a>Zeichenfolge Warteschlangennachrichten

Im folgenden Beispiel nicht asynchrone erstellt eine neue Warteschlange-Nachricht in der Warteschlange mit dem Namen "Outputqueue" mit denselben Inhalt wie der Warteschlange Nachricht in der Warteschlange mit dem Namen "Inputqueue". (Für asynchrone Funktionen verwenden **IAsyncCollector<T> ** wie weiter unten in diesem Abschnitt dargestellt.)


        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] string queueMessage,
            [Queue("outputqueue")] out string outputQueueMessage )
        {
            outputQueueMessage = queueMessage;
        }

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a>POCO [(schlicht alten CLR-Objekt](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) in die Warteschlange Nachrichten

Zum Erstellen einer Warteschlange-Nachricht, die eine POCO anstelle einer Zeichenfolge enthält, wird an den Attributkonstruktor **Warteschlange** übergeben Sie den POCO-Typ als Output-Parameter.

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] BlobInformation blobInfoInput,
            [Queue("outputqueue")] out BlobInformation blobInfoOutput )
        {
            blobInfoOutput = blobInfoInput;
        }

Das SDK wird automatisch das Objekt, das JSON serialisiert. Eine Warteschlange Nachricht wird immer erstellt, auch wenn das Objekt null ist.

### <a name="create-multiple-messages-or-in-async-functions"></a>Erstellen mehrerer Nachrichten oder asynchronen Funktionen

Um mehrere Nachrichten zu erstellen, nehmen Sie den Parametertyp für die Ausgabewarteschlange **ICollector<T> ** oder **IAsyncCollector<T>**, wie im folgenden Beispiel gezeigt.

        public static void CreateQueueMessages(
            [QueueTrigger("inputqueue")] string queueMessage,
            [Queue("outputqueue")] ICollector<string> outputQueueMessage,
            TextWriter logger)
        {
            logger.WriteLine("Creating 2 messages in outputqueue");
            outputQueueMessage.Add(queueMessage + "1");
            outputQueueMessage.Add(queueMessage + "2");
        }

Jede Nachricht Warteschlange wird erstellt, sofort gestartet, wenn die **Add** -Methode aufgerufen wird.

### <a name="types-that-the-queue-attribute-works-with"></a>Dateitypen, mit denen das Attribut Warteschlange zusammenarbeitet

Sie können das Attribut **Warteschlange** auf die folgenden Arten von Parameter verwenden:

* **Zeichenfolge** (erstellt Warteschlange Nachricht übergebener Wert ungleich Null ist, nach Beendigung der Funktion)
* **Auschecken Byte]** (funktioniert nicht wie **Zeichenfolge**)
* **Auschecken CloudQueueMessage** (funktioniert nicht wie **Zeichenfolge**)
* **Auschecken POCO** (ein serialisierbarer Typ, erstellt eine Nachricht mit einem null-Objekt, wenn der Parameter null ist, nach Beendigung der Funktion)
* **ICollector**
* **IAsyncCollector**
* **CloudQueue** (zum manuellen Erstellen von Nachrichten mithilfe der Azure-Speicher-API direkt)

### <a name="use-webjobs-sdk-attributes-in-the-body-of-a-function"></a>Verwenden Sie WebJobs SDK Attribute in den Textkörper einer Funktion

Wenn Sie einige Arbeit in der Funktion vor der Verwendung einer WebJobs SDK Attribut wie **Warteschlange**, **Blob**oder **Tabelle**erledigen müssen, können Sie die Benutzeroberfläche **IBinder** .

Im folgende Beispiel eine Nachricht Eingabewerte Warteschlange akzeptiert und erstellt eine neue Nachricht mit denselben Inhalt in einer Ausgabewarteschlange. Der Name der Warteschlange wird vom Code in den Textkörper der Funktion festgelegt.

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] string queueMessage,
            IBinder binder)
        {
            string outputQueueName = "outputqueue" + DateTime.Now.Month.ToString();
            QueueAttribute queueAttribute = new QueueAttribute(outputQueueName);
            CloudQueue outputQueue = binder.Bind<CloudQueue>(queueAttribute);
            outputQueue.AddMessage(new CloudQueueMessage(queueMessage));
        }

Die Schnittstelle **IBinder** kann auch mit der **Tabelle** und **Blob** -Attribute verwendet werden.

## <a name="how-to-read-and-write-blobs-and-tables-while-processing-a-queue-message"></a>Zum Lesen und schreiben beim Verarbeiten einer Nachricht Warteschlange Blobs und Tabellen

Die Attribute **Blob** und **Tabelle** aktivieren Sie zum Lesen und Schreiben von Blobs und Tabellen. In den Beispielen in diesem Abschnitt beziehen sich auf Blobs. Zum Codebeispielen, die zeigen, wie Prozesse ausgelöst, wenn Blobs erstellt oder aktualisiert werden, finden Sie unter [Verwenden von Azure Blob-Speicher mit dem SDK WebJobs](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)und Codebeispielen, die lesen und Schreiben von Tabellen, finden Sie unter [So Azure Table Storage mit dem WebJobs SDK verwenden](../app-service-web/websites-dotnet-webjobs-sdk-storage-tables-how-to.md).

### <a name="string-queue-messages-triggering-blob-operations"></a>Zeichenfolge Warteschlangennachrichten auslösen Blob-Vorgänge

Für eine Warteschlange-Nachricht, die eine Zeichenfolge enthält, ist die **QueueTrigger** einen Platzhalter, die, den Sie in das **Blob** -Attribut **BlobPath** Parameter verwenden können, die den Inhalt der Nachricht enthält.

Im folgende Beispiel wird zum Lesen und Schreiben von Blobs **Stream** Objekte verwendet. Die Nachricht Warteschlange wird Name eines Blob befindet sich im Container Textblobs. Eine Kopie der Blob mit "-Neu" angefügt, um der Namen im selben Container erstellt wird.

        public static void ProcessQueueMessage(
            [QueueTrigger("blobcopyqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}",FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new",FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

Der **Blob** -Attributkonstruktor dauert **BlobPath** Parameter, der angibt, die Container und Blob-Name. Weitere Informationen zu dieser Platzhalter finden Sie unter [Azure Blob-Speicher mit dem WebJobs SDK verwenden](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md).

Wenn das Attribut ein Objekts **Stream** wird, gibt eine andere Konstruktorparameter **FileAccess** -Modus als lesen, schreiben oder schreibgeschützt.

Im folgende Beispiel wird mit einer **CloudBlockBlob** Objekt einen Blob gelöscht. Die Nachricht Warteschlange ist der Name des Blob.

        public static void DeleteBlob(
            [QueueTrigger("deleteblobqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}")] CloudBlockBlob blobToDelete)
        {
            blobToDelete.Delete();
        }

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a>POCO [(schlicht alten CLR-Objekt](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) in die Warteschlange Nachrichten

Für eine POCO als JSON in der Warteschlange Nachricht gespeichert ist können Sie Platzhalter die Nameneigenschaften des Objekts in der **Warteschlange** -Attribut **BlobPath** Parameter verwenden. Sie können auch Warteschlange Eigenschaft Metadatennamen als Platzhalter verwenden. Finden Sie unter [Abrufen von Warteschlange oder Warteschlange Nachrichtenmetadaten](#get-queue-or-queue-message-metadata).

Im folgende Beispiel kopiert einen Blob in einer neuen Blob mit einer anderen Erweiterung. Die Nachricht Warteschlange ist ein **BlobInformation** -Objekt, das Eigenschaften **BlobName** und **BlobNameWithoutExtension** enthält. Die Eigenschaftennamen werden als Platzhalter in den Blob-Pfad für die **Blob** -Attribute verwendet.

        public static void CopyBlobPOCO(
            [QueueTrigger("copyblobqueue")] BlobInformation blobInfo,
            [Blob("textblobs/{BlobName}", FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{BlobNameWithoutExtension}.txt", FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

Das SDK verwendet das [Newtonsoft.Json NuGet-Paket](http://www.nuget.org/packages/Newtonsoft.Json) serialisiert und deserialisiert Nachrichten an. Wenn Sie Nachrichten in Warteschlange in einem Programm, in denen das WebJobs SDK verwendet wird nicht erstellen, können Sie Schreiben von Code wie im folgenden Beispiel eine POCO Warteschlange-Nachricht zu erstellen, die das SDK analysiert werden kann.

        BlobInformation blobInfo = new BlobInformation() { BlobName = "boot.log", BlobNameWithoutExtension = "boot" };
        var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
        logQueue.AddMessage(queueMessage);

Wenn Sie einige Arbeit in der Funktion vor dem Binden eines BLOBs zu einem Objekt ausführen müssen, können Sie das Attribut im Textkörper der Funktion, siehe [WebJobs SDK verwenden Attribute in den Textkörper einer Funktion](#use-webjobs-sdk-attributes-in-the-body-of-a-function).

###<a name="types-you-can-use-the-blob-attribute-with"></a>Dateitypen, die, denen Sie mit das Blob-Attribut verwenden können

Das Attribut **Blob** kann mit den folgenden Arten verwendet werden:

* **Stream** (Lesen Sie oder Schreiben Sie mithilfe des Konstruktorparameters FileAccess angegeben)
* **TextReader**
* **TextWriter**
* **Zeichenfolge** (schreibgeschützt)
* **Zeichenfolge** (Schreiben; einen Blob erstellt, nur, wenn der Parameter nicht Null ist, gibt die Funktion)
* POCO (schreibgeschützt)
* Auschecken POCO (Schreiben; immer einen Blob erstellt, als null-Objekt wird erstellt, wenn POCO Parameter null ist, gibt die Funktion)
* **CloudBlobStream** (Schreiben)
* **ICloudBlob** (Lese- oder Schreibzugriff)
* **CloudBlockBlob** (Lese- oder Schreibzugriff)
* **CloudPageBlob** (Lese- oder Schreibzugriff)

##<a name="how-to-handle-poison-messages"></a>Beschädigte Nachrichten behandelt

Nachrichten, deren Inhalt bewirkt, eine Funktion zum Fehlschlagen dass, sind *beschädigte Nachrichten*bezeichnet. Wenn die Funktion fehlschlägt, wird die Warteschlange-Nachricht wird nicht gelöscht und schließlich wird übernommen erneut bewirken, dass des Zyklus wiederholt werden soll. Das SDK kann automatisch den Zyklus nach eine eingeschränkte Anzahl von Iterationen unterbrechen oder manuell vornehmen.

### <a name="automatic-poison-message-handling"></a>Automatische beschädigte Nachrichtenbehandlung

Das SDK Ruft eine Funktion bis zu 5 Mal eine Nachricht Warteschlange Verarbeitungszeit. Wenn die fünfte testen fehlschlägt, wird die Nachricht an eine beschädigte Warteschlange verschoben. Sie können finden Sie unter So konfigurieren Sie die maximale Anzahl der Wiederholungsversuche in [so festlegen](#how-to-set-configuration-options).

Die beschädigte Warteschlange heißt *{Originalqueuename}*-beschädigte. Sie können schreiben, dass eine Funktion zum Verarbeiten von Nachrichten aus der beschädigte Warteschlange durch diese Protokollierung oder Senden einer Benachrichtigung die manuelle Aufmerksamkeit erforderlich ist.

Im folgenden Beispiel die **CopyBlob** tritt (Funktion), wenn eine Warteschlange-Nachricht den Namen eines Blob enthält, die nicht vorhanden ist. In diesem Fall wird die Nachricht aus der Warteschlange Copyblobqueue in der Warteschlange Copyblobqueue-Poison verschoben. Der **ProcessPoisonMessage** meldet dann an die beschädigte Nachricht.

        public static void CopyBlob(
            [QueueTrigger("copyblobqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}", FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new", FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

        public static void ProcessPoisonMessage(
            [QueueTrigger("copyblobqueue-poison")] string blobName, TextWriter logger)
        {
            logger.WriteLine("Failed to copy blob, name=" + blobName);
        }

Die folgende Abbildung zeigt die Ausgabe in der Konsole aus dieser Funktionen, wenn eine beschädigte Nachricht verarbeitet wird.

![Ausgabe in der Konsole für die Behandlung von beschädigte Nachricht](./media/vs-storage-webjobs-getting-started-queues/poison.png)

### <a name="manual-poison-message-handling"></a>Manuelle beschädigte Nachrichtenbehandlung

Sie können die Anzahl der Häufigkeit, mit die eine Nachricht von entnommen wurde für die Verarbeitung durch Hinzufügen eines **Int** -Parameters, die mit dem Namen **DequeueCount** zu Ihrer Funktion erhalten. Dann können Sie die Anzahl der zum Abrufen aus Funktion Code überprüfen und Ausführen Ihrer eigenen beschädigte Nachrichtenbehandlung, wenn die Zahl einen Schwellenwert überschreitet wie im folgenden Beispiel dargestellt.

        public static void CopyBlob(
            [QueueTrigger("copyblobqueue")] string blobName, int dequeueCount,
            [Blob("textblobs/{queueTrigger}", FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new", FileAccess.Write)] Stream blobOutput,
            TextWriter logger)
        {
            if (dequeueCount > 3)
            {
                logger.WriteLine("Failed to copy blob, name=" + blobName);
            }
            else
            {
            blobInput.CopyTo(blobOutput, 4096);
            }
        }

## <a name="how-to-set-configuration-options"></a>Zum Festlegen von Optionen

Den Typ des **JobHostConfiguration** können Sie die folgenden Optionen festlegen:

* Legen Sie die SDK Verbindungszeichenfolgen in Code ein.
* Konfigurieren von **QueueTrigger** Einstellungen wie Maximum Nachrichten zählen.
* Abrufen von Warteschlangennamen aus der Konfiguration.

###<a name="set-sdk-connection-strings-in-code"></a>Festlegen von SDK Verbindungszeichenfolgen in code

Festlegen der SDK Verbindungszeichenfolgen in Code können Sie eigene Zeichenfolge Verbindungsnamen in Konfigurationsdateien oder Umgebungsvariablen verwenden, wie im folgenden Beispiel dargestellt.

        static void Main(string[] args)
        {
            var _storageConn = ConfigurationManager
                .ConnectionStrings["MyStorageConnection"].ConnectionString;

            var _dashboardConn = ConfigurationManager
                .ConnectionStrings["MyDashboardConnection"].ConnectionString;

            var _serviceBusConn = ConfigurationManager
                .ConnectionStrings["MyServiceBusConnection"].ConnectionString;

            JobHostConfiguration config = new JobHostConfiguration();
            config.StorageConnectionString = _storageConn;
            config.DashboardConnectionString = _dashboardConn;
            config.ServiceBusConnectionString = _serviceBusConn;
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }

### <a name="configure-queuetrigger--settings"></a>Konfigurieren von Einstellungen zur QueueTrigger

Sie können die folgenden Einstellungen konfigurieren, die für die Verarbeitung von Nachrichten Warteschlange zutreffen:

- Die maximale Anzahl von Nachrichten in Warteschlange, die gleichzeitig aufgenommen werden, parallel ausgeführt werden soll (der Standardwert liegt 16).
- Die maximale Anzahl der Wiederholungsversuche vor dem Senden einer Nachricht Warteschlange an eine beschädigte Warteschlange (Standard ist 5).
- Die maximale Wartezeit erneut abrufen, wenn eine Warteschlange leer ist (Standardwert ist 1 Minute).

Im folgenden Beispiel wird gezeigt, wie diese Einstellungen konfigurieren:

        static void Main(string[] args)
        {
            JobHostConfiguration config = new JobHostConfiguration();
            config.Queues.BatchSize = 8;
            config.Queues.MaxDequeueCount = 4;
            config.Queues.MaxPollingInterval = TimeSpan.FromSeconds(15);
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }

### <a name="set-values-for-webjobs-sdk-constructor-parameters-in-code"></a>Festlegen der Werte für WebJobs SDK Parameter in code

Manchmal möchten Sie einen Warteschlangennamen, einen Blob-Namen oder Container angeben, oder eine Tabelle in Code statt programmiert benennen sie. Sie möchten beispielsweise, geben Sie den Warteschlangennamen für **QueueTrigger** in eine Datei oder -Umgebung, die mit der Konfiguration wird.

Sie können, die durch die Übergabe eines **NameResolver** -Objekts in dem Typ **JobHostConfiguration** ausführen. Sie Inhalte umgeben von Prozentzeichen (%) WebJobs SDK Parameter in Platzhaltern einschließen und der Code **NameResolver** gibt an, der tatsächlichen Werte anstelle diese Platzhalter verwendet werden.

Nehmen Sie beispielsweise an, dass Sie eine Warteschlange mit dem Namen Logqueuetest in der testumgebung und einer benannten Logqueueprod Herstellung verwenden möchten. Statt einen codierten Warteschlangennamen möchten Sie den Namen eines Eintrags in der Auflistung **AppSettings** angeben, die den Namen der tatsächlichen Warteschlange müssten. Ist der Schlüssel **AppSettings** Logqueue, könnte die Funktion wie im folgenden Beispiel aussehen.

        public static void WriteLog([QueueTrigger("%logqueue%")] string logMessage)
        {
            Console.WriteLine(logMessage);
        }

Ihre **NameResolver** Klasse konnte dann Warteschlange Abrufen des Vornamens aus **AppSettings** wie im folgenden Beispiel gezeigt:

        public class QueueNameResolver : INameResolver
        {
            public string Resolve(string name)
            {
                return ConfigurationManager.AppSettings[name].ToString();
            }
        }

Sie übergeben die **NameResolver** Klasse in auf das Objekt **JobHost** wie im folgenden Beispiel dargestellt.

        static void Main(string[] args)
        {
            JobHostConfiguration config = new JobHostConfiguration();
            config.NameResolver = new QueueNameResolver();
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }

**Hinweis:** Warteschlange, Tabelle und Blob-Namen werden jedes Mal eine Funktion aufgerufen wird, jedoch nur, wenn die Anwendung gestartet wird, werden Blob Containernamen aufgelöst aufgelöst. Sie können Blob Containername nicht ändern, während der Auftrag ausgeführt wird.

## <a name="how-to-trigger-a-function-manually"></a>Wie Sie eine Funktion manuell auslösen

Um eine Funktion manuell ausgelöst wird, verwenden Sie die **Anrufen** oder **CallAsync** Methode auf das Objekt **JobHost** und das **NoAutomaticTrigger** -Attribut für die Funktion, wie im folgenden Beispiel dargestellt.

        public class Program
        {
            static void Main(string[] args)
            {
                JobHost host = new JobHost();
                host.Call(typeof(Program).GetMethod("CreateQueueMessage"), new { value = "Hello world!" });
            }

            [NoAutomaticTrigger]
            public static void CreateQueueMessage(
                TextWriter logger,
                string value,
                [Queue("outputqueue")] out string message)
            {
                message = value;
                logger.WriteLine("Creating queue message: ", message);
            }
        }

## <a name="how-to-write-logs"></a>So schreiben Sie Protokolle

Das Dashboard zeigt Protokolle an zwei Orten aufsuchen: der Seite für die WebJob sowie die Seite für einen bestimmten WebJob aufrufen.

![Protokolle WebJob Seite](./media/vs-storage-webjobs-getting-started-queues/dashboardapplogs.png)

![Funktion aufrufen Seite Protokolle](./media/vs-storage-webjobs-getting-started-queues/dashboardlogs.png)

Ausgabe Console-Methoden, die Sie in einer Funktion oder in **der Main()-Methode** Aufrufen angezeigt wird, in der Dashboard-Seite für die WebJob, nicht auf der Seite für eine bestimmte Methode aufrufen. Ausgabe aus dem TextWriter-Objekt, das Sie von einem Parameter in Ihrer Methodensignatur abrufen wird im Dashboard-Seite für eine Methode aufrufen.

Ausgabe in der Konsole kann nicht auf eine bestimmte Methode aufrufen verknüpft werden, da die Konsole Single threaded ist, während viele Funktionen zur gleichen Zeit ausgeführt werden können. Warum ist das SDK bietet jede Funktion aufrufen mit einem eigenen eindeutigen Log Autor Objekt.

Verwenden Sie zum Schreiben von [Anwendung Tracing Protokolle](web-sites-dotnet-troubleshoot-visual-studio.md#logsoverview) **Console.Out** (Protokolle markiert Informationen erstellt) und **Console.Error** (erstellt Protokolle als Fehler markiert) aus. Eine Alternative ist [Spur oder TraceSource](http://blogs.msdn.com/b/mcsuksoldev/archive/2014/09/04/adding-trace-to-azure-web-sites-and-web-jobs.aspx), verwenden Sie die ausführlich, Warnung und kritisch Ebenen sowie Informationen zu Vorgehensweisen und Fehler bereitstellt. Anwendung Tracing Protokolle angezeigt werden, in den Web app-Protokolldateien, Azure Tabellen oder Azure-blobs je nachdem, wie Sie Ihre Azure Web app konfigurieren. Der gesamte Console Ausgabe true festgelegt ist, werden die letzten 100 Anwendungsprotokolle auch in der Dashboard-Seite für die WebJob, nicht auf der Seite für eine Funktion aufrufen angezeigt.

Ausgabe in der Konsole angezeigt wird, in dem Dashboard nur, wenn das Programm in eine WebJob Azure ausgeführt wird, nicht, wenn das Programm lokal ausgeführt wird oder in einer anderen Umgebung.

Sie können Protokollierung deaktivieren, indem Sie die Verbindungszeichenfolge Dashboard auf null. Weitere Informationen finden Sie unter [so festlegen](#how-to-set-configuration-options).

Das folgende Beispiel zeigt verschiedene Methoden zum Schreiben von Protokollen:

        public static void WriteLog(
            [QueueTrigger("logqueue")] string logMessage,
            TextWriter logger)
        {
            Console.WriteLine("Console.Write - " + logMessage);
            Console.Out.WriteLine("Console.Out - " + logMessage);
            Console.Error.WriteLine("Console.Error - " + logMessage);
            logger.WriteLine("TextWriter - " + logMessage);
        }

Im Dashboard SDK WebJobs beim angezeigt werden der Ausgabe aus dem Objekt **TextWriter** wechseln Sie zu der Seite für eine bestimmte Funktion aufrufen und wählen Sie **Den Schalter Ausgabe**:

![Link aufrufen](./media/vs-storage-webjobs-getting-started-queues/dashboardinvocations.png)

![Funktion aufrufen Seite Protokolle](./media/vs-storage-webjobs-getting-started-queues/dashboardlogs.png)

Die letzten 100 Zeilen der Verwaltungskonsole ausgeben anzeigen im Dashboard SDK WebJobs nach oben, wenn Sie wechseln Sie zur Seite für die WebJob (nicht für die Funktion aufrufen), und wählen Sie **Den Schalter Ausgabe**.

![Umschaltfläche Ausgabe](./media/vs-storage-webjobs-getting-started-queues/dashboardapplogs.png)

In einer zusammenhängenden WebJob werden Anwendungsprotokolle in/Daten/Aufträge/fortlaufender /*{Webjobname}*/job_log.txt im Dateisystem Web app.

        [09/26/2014 21:01:13 > 491e54: INFO] Console.Write - Hello world!
        [09/26/2014 21:01:13 > 491e54: ERR ] Console.Error - Hello world!
        [09/26/2014 21:01:13 > 491e54: INFO] Console.Out - Hello world!

In einer Azure BLOB-Anwendung Protokolle aussehen wie folgt: 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738373502,0,17404,17,Console.Write - Hallo Welt!, 2014-09-26T21:01:13,Error,contosoadsnew,491e54,635473620738373502,0,17404,19,Console.Error - Hallo Welt!, 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738529920,0,17404,17,Console.Out - Hallo Welt!,

Und in einer Tabelle Azure die Protokolle **Console.Out** und **Console.Error** wie folgt aussehen:

![Infolog in einer Tabelle](./media/vs-storage-webjobs-getting-started-queues/tableinfo.png)

![Fehlerprotokoll in einer Tabelle](./media/vs-storage-webjobs-getting-started-queues/tableerror.png)

##<a name="next-steps"></a>Nächste Schritte

In diesem Artikel hat Codebeispielen bereitgestellt, die veranschaulichen, allgemeine Szenarien für das Arbeiten mit Azure Warteschlangen zu behandeln. Weitere Informationen zum Verwenden von Azure WebJobs und das WebJobs SDK finden Sie unter [Azure WebJobs Dokumentationsressourcen](http://go.microsoft.com/fwlink/?linkid=390226).

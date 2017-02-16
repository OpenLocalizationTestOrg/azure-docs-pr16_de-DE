<properties
    pageTitle="Erste Schritte mit Warteschlangenspeicher und Visual Studio verbunden Services (ASP.NET 5) | Microsoft Azure"
    description="Erste Schritte mit Azure Warteschlangenspeicher in einem Projekt ASP.NET 5 in Visual Studio"
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

# <a name="get-started-with-queue-storage-and-visual-studio-connected-services-aspnet-5"></a>Erste Schritte mit Warteschlangenspeicher und Visual Studio verbunden Services (ASP.NET 5)

[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

##<a name="overview"></a>(Übersicht)

In diesem Artikel werden die Schritte in Visual Studio Warteschlange Azure-Speicher verwenden, nachdem Sie erstellt oder auf die verwiesen wird ein Konto Azure-Speicher in einem Projekt ASP.NET 5 über das Dialogfeld Visual Studio **Verbunden Dienste hinzufügen** . Der Vorgang **Verbunden Dienste hinzufügen** Installationen die entsprechenden NuGet Pakete Azure-Speicher im Projekt zugreifen und Projektdateien Konfiguration die Verbindungszeichenfolge für den Speicherkonto hinzugefügt.

Azure Warteschlangenspeicher ist ein Dienst für das Speichern einer großen Anzahl von Nachrichten, die über eine beliebige Stelle in der Welt über authentifizierten Anrufe mit HTTP oder HTTPS zugegriffen werden können. Eine einzelne Warteschlange Nachricht kann Größe von bis zu 64 Kilobyte (KB), und eine Warteschlange Millionen von Nachrichten, auf die gesamte Kapazität Beschränkung eines Kontos Speicher enthalten kann.

Um anzufangen, müssen Sie zuerst eine Azure Warteschlange in Ihr Speicherkonto zu erstellen. Wir zeigen Ihnen zum Erstellen einer Warteschlange im Code. Wir werden Sie auch wie grundlegende Warteschlangenoperationen, wie z. B. hinzufügen, ändern, lesen und Entfernen von Warteschlangennachrichten anzeigen. Die Beispiele in C geschrieben werden\# code ein, und verwenden Sie die Azure-Speicher-Client-Bibliothek für .NET. Weitere Informationen zu ASP.NET finden Sie unter [ASP.NET](http://www.asp.net).

**Hinweis:** Einige der APIs, die Anrufe an Azure-Speicher in ASP.NET 5 ausführen sind asynchrone. Weitere Informationen finden Sie unter [asynchrone Programmierung mit asynchronen und Await](http://msdn.microsoft.com/library/hh191443.aspx) . Im folgenden Code wird davon ausgegangen, dass asynchrone Programmierung Methoden verwendet werden.

- Weitere Informationen zum programmgesteuerten Bearbeiten von Warteschlangen finden Sie unter [Erste Schritte mit Azure Warteschlange-Speicher mit .NET](storage-dotnet-how-to-use-queues.md) .
- Allgemeine Informationen zur Azure-Speicher finden Sie unter [Speicher-Dokumentation](https://azure.microsoft.com/documentation/services/storage/) .
- Allgemeine Informationen zu Azure Cloud Services finden Sie unter [Cloud Services-Dokumentation](https://azure.microsoft.com/documentation/services/cloud-services/) .
- Weitere Informationen zu ASP.NET Applications programming finden Sie unter [ASP.NET](http://www.asp.net) .





##<a name="access-queues-in-code"></a>Access-Warteschlangen Code

Um Warteschlangen in ASP.NET 5 Projekte zugreifen zu können, müssen Sie die folgenden Elemente zu C#-Quelldatei, enthalten, die greift Azure Warteschlangenspeicher auf.

1. Stellen Sie sicher, dass die Namespace-Deklaration am oberen Rand der C#-Datei diese Anweisungen **verwenden** einschließen.

        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Queue;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;

2. Abrufen einer **CloudStorageAccount** -Objekt, das Ihre Kontoinformationen Speicher darstellt. Verwenden Sie den folgenden Code zum Abrufen der Ihre Verbindungszeichenfolge Speicher und die Speicher-Kontoinformationen aus der Konfiguration Azure Service.

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

3. Abrufen eines **CloudQueueClient** -Objekts in der Warteschlangenobjekte in Ihr Speicherkonto zu verweisen.  

        // Create the CloudQueueClient object for the storage account.
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

4. Abrufen eines Objekts **CloudQueue** Verweis auf eine bestimmte Warteschlange an.

        // Get a reference to the CloudQueue named "messageQueue"
        CloudQueue messageQueue = queueClient.GetQueueReference("messageQueue");


**Hinweis:** Verwenden Sie alle obigen Code vor den Code in den folgenden Beispielen aus.

###<a name="create-a-queue-in-code"></a>Erstellen einer Warteschlange Code

Um die Azure Warteschlange Code zu erstellen, fügen Sie nur einen Anruf an **CreateIfNotExistsAsync**.

    // Create the CloudQueue if it does not exist.
    await messageQueue.CreateIfNotExistsAsync();

##<a name="add-a-message-to-a-queue"></a>Fügen Sie eine Nachricht an eine Warteschlange

Erstellen eines neuen **CloudQueueMessage** -Objekts zum Einfügen einer Nachricht in eine vorhandene Warteschlange, und klicken Sie dann rufen Sie die Methode **AddMessageAsync** .

Ein Objekt **CloudQueueMessage** kann aus einer Zeichenfolge (im UTF-8-Format) oder ein Byte-Array erstellt werden.

Hier ist ein Beispiel für den die Nachricht "Hallo, Welt eingefügt.

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    await messageQueue.AddMessageAsync(message);

##<a name="read-a-message-in-a-queue"></a>Lesen von Nachrichten in einer Warteschlange

Sie können die Nachricht an eine Warteschlange der Vorderseite einsehen, ohne aus der Warteschlange entfernt werden, indem Sie die Methode **PeekMessageAsync** aufrufen.

    // Peek the next message in the queue. 
    CloudQueueMessage peekedMessage = await messageQueue.PeekMessageAsync();


##<a name="read-and-remove-a-message-in-a-queue"></a>Lesen Sie und entfernen Sie eine Nachricht in einer Warteschlange zu.

Code entfernen kann (aus der Warteschlange entfernt) eine Nachricht von einer Warteschlange in zwei Schritten.
1. Rufen Sie **GetMessageAsync** , um die nächste Nachricht in einer Warteschlange zu erhalten. Eine Meldung, die von **GetMessageAsync** zurückgegeben wird für alle anderen Code Lesen von Nachrichten aus dieser Warteschlange nicht sichtbar. Standardmäßig bleibt diese Meldung 30 Sekunden nicht sichtbar.
2.  Wenn Sie fertig sind, wobei die Nachricht aus der Warteschlange entfernt, rufen Sie **DeleteMessageAsync**aus.

Diese zwei Verfahren zum Entfernen einer Nachricht wird sichergestellt, dass Code nicht verarbeiten einer Nachricht aufgrund von Fehlern Hardware oder Software, einer anderen Instanz des Codes kann dieselbe Nachricht erhalten und versuchen Sie es erneut. Der folgende Code ruft **DeleteMessageAsync** rechts, nachdem die Nachricht verarbeitet wurde.

    // Get the next message in the queue.
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();

    // Process the message in less than 30 seconds.

    // Then delete the message.
    await messageQueue.DeleteMessageAsync(retrievedMessage);

## <a name="leverage-additional-options-for-dequeuing-messages"></a>Nutzen Sie zusätzliche Optionen für Abarbeiten der Warteschlange Nachrichten

Es gibt zwei Möglichkeiten, die Sie aus einer Warteschlange Nachrichtenübermittlung anpassen können.
Zunächst erhalten Sie eine Reihe von Nachrichten (bis zu 32). Zweites, können Sie einen Timeout verlängern oder verkürzen Invisibility festlegen, gleicht der Code mehr oder weniger jede Nachricht vollständig Verarbeitungszeit. Im folgenden Code wird verwendet die **GetMessages** Methode, um 20 Nachrichten in einem Anruf zu gelangen. Dann wird jede Nachricht mit einer **Foreach** -Schleife verarbeitet. Invisibility Timeout wird auch auf 5 Minuten für jede Nachricht festgelegt. Beachten Sie, dass starten 5 Minuten für alle Nachrichten gleichzeitig, übergeben nach 5 Minuten haben nach den Anruf an **GetMessages**, alle Nachrichten, die nicht gelöscht wurden erneut angezeigt werden.

    // Retrieve 20 messages at a time, keeping those messages invisible for 5 minutes, 
    //   delete each message after processing.

    foreach (CloudQueueMessage message in messageQueue.GetMessages(20, TimeSpan.FromMinutes(5)))
    {
        // Process all messages in less than 5 minutes, deleting each message after processing.
        queue.DeleteMessage(message);
    }

## <a name="get-the-queue-length"></a>Abrufen der Länge der Warteschlange

Sie können eine Schätzung der Anzahl von Nachrichten in einer Warteschlange abrufen. Die Methode **FetchAttributes** gefragt werden, zum Abrufen der Warteschlange-Attributen, die Anzahl der Nachricht einschließlich den Warteschlangendienst. Die Eigenschaft **ApproximateMethodCount** gibt den letzten Wert von der **FetchAttributes** -Methode ohne Aufrufen des Diensts Warteschlange abgerufen.

    // Fetch the queue attributes.
    messageQueue.FetchAttributes();

    // Retrieve the cached approximate message count.
    int? cachedMessageCount = messageQueue.ApproximateMessageCount;

    // Display the number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## <a name="use-the-async-await-pattern-with-common-queue-apis"></a>Verwenden Sie das Muster asynchronen erwartet mit gemeinsame Warteschlange-APIs

Dieses Beispiel zeigt, wie das Muster asynchronen erwartet mit gemeinsame Warteschlange APIs verwendet wird. Im Beispiel ruft die asynchrone Version des jeweiligen der angegebenen Methoden. Dies kann von jeder Methode Fix nach der asynchronen angezeigt werden. Wenn eine asynchrone Methode verwendet wird, hält das Muster asynchronen erwartet lokale Ausführung bis zum Abschluss des Anrufs an. Dieses Verhalten kann es sich um den aktuellen Thread andere Arbeiten fortsetzen Leistungsengpässe vermieden und der gesamten Reaktionszeiten der Anwendung verbessert. Weitere Details zur Verwendung des Musters asynchronen-Await in .NET finden Sie unter [asynchrone und Await (C#- und Visual Basic)] (https://msdn.microsoft.com/library/hh191443.aspx)

    // Create a message to add to the queue.
    CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

    // Async enqueue the message.
    await messageQueue.AddMessageAsync(cloudQueueMessage);
    Console.WriteLine("Message added");

    // Async dequeue the message.
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();
    Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

    // Async delete the message.
    await messageQueue.DeleteMessageAsync(retrievedMessage);
    Console.WriteLine("Deleted message");
## <a name="delete-a-queue"></a>Löschen einer Warteschlange

Rufen Sie zum Löschen einer Warteschlange und alle darin enthaltenen Nachrichten die **Löschen** -Methode für die Warteschlange-Objekt ein.

    // Delete the queue.
    messageQueue.Delete();


##<a name="next-steps"></a>Nächste Schritte

[AZURE.INCLUDE [vs-storage-dotnet-queues-next-steps](../../includes/vs-storage-dotnet-queues-next-steps.md)]

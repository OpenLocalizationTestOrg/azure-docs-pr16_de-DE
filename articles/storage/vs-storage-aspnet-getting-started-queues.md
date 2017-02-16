<properties
    pageTitle="Erste Schritte mit Warteschlangenspeicher und Visual Studio verbunden Services (ASP.NET) | Microsoft Azure"
    description="Erste Schritte mit Azure Warteschlange-Speicher in einem ASP.NET-Projekt in Visual Studio nach dem Herstellen einer Verbindung mit einem Speicherkonto mithilfe von Visual Studio verbunden services"
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
    ms.date="08/15/2016"
    ms.author="tarcher"/>

# <a name="get-started-with-azure-queue-storage-and-visual-studio-connected-services"></a>Erste Schritte mit Azure Warteschlange Speicher und Visual Studio verbunden Services

[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>(Übersicht)

Dieser Artikel beschreibt, wie erste Schritte mit Azure Warteschlange Speicher in Visual Studio, nachdem Sie erstellt oder auf die verwiesen wird ein Konto Azure-Speicher in einem ASP.NET-Projekt mithilfe des Dialogfelds Visual Studio **Verbunden Dienste hinzufügen** .

Es wird gezeigt, wie zu erstellen und zu einer Warteschlange Azure in Ihr Speicherkonto zugreifen. Wir werden Sie auch wie grundlegende Warteschlangenoperationen, wie z. B. hinzufügen, ändern, lesen und Entfernen von Warteschlangennachrichten anzeigen. Die Beispiele in C#-Code geschrieben sind, und verwenden Sie die [Microsoft Azure-Speicher-Client-Bibliothek für .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx). Weitere Informationen zu ASP.NET finden Sie unter [ASP.NET](http://www.asp.net).

Azure Warteschlange-Speicher ist ein Dienst für das Speichern einer großen Anzahl von Nachrichten, die über eine beliebige Stelle in der Welt über authentifizierten Anrufe mit HTTP oder HTTPS zugegriffen werden können. Eine einzelne Warteschlange Nachricht kann bis zu 64 KB sein, und eine Warteschlange kann Millionen von Nachrichten, auf die gesamte Kapazität Beschränkung eines Kontos Speicher enthalten.

## <a name="access-queues-in-code"></a>Access-Warteschlangen Code

Um Warteschlangen in ASP.NET-Projekte zugreifen zu können, müssen Sie die folgenden Elemente zu C#-Quelldatei, enthalten, die Warteschlange Azure-Speicher zugreifen.

1. Stellen Sie sicher, dass die Namespace-Deklaration am oberen Rand der C#-Datei diese Anweisungen **verwenden** einschließen.

        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Queue;

2. Abrufen einer **CloudStorageAccount** -Objekt, das Ihre Kontoinformationen Speicher darstellt. Verwenden Sie den folgenden Code zum Abrufen der Ihre Verbindungszeichenfolge Speicher und die Speicher-Kontoinformationen aus der Konfiguration Azure Service.

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

3. Abrufen eines **CloudQueueClient** -Objekts in der Warteschlangenobjekte in Ihr Speicherkonto zu verweisen.  

        // Create the CloudQueueClient object for this storage account.
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

4. Abrufen eines Objekts **CloudQueue** Verweis auf eine bestimmte Warteschlange an.

        // Get a reference to a queue named "messageQueue"
        CloudQueue messageQueue = queueClient.GetQueueReference("messageQueue");


**Hinweis** Verwenden Sie alle obigen Code vor den Code in den folgenden Beispielen aus.

## <a name="create-a-queue-in-code"></a>Erstellen einer Warteschlange Code

Zum Erstellen einer Azure Warteschlange Code hinzufügen von nur einen Anruf an **CreateIfNotExists** zu den oben angegebenen Code.

    // Create the messageQueue if it does not exist
    messageQueue.CreateIfNotExists();

## <a name="add-a-message-to-a-queue"></a>Fügen Sie eine Nachricht an eine Warteschlange

Klicken Sie zum Einfügen einer Nachricht in eine vorhandene Warteschlange Erstellen eines neuen **CloudQueueMessage** -Objekts und dann die **AddMessage** -Methode aufrufen.

Ein Objekt **CloudQueueMessage** kann aus einer Zeichenfolge (im UTF-8-Format) oder ein Byte-Array erstellt werden.

Hier ist ein Beispiel für den die Nachricht "Hallo, Welt eingefügt.

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    messageQueue.AddMessage(message);

## <a name="read-a-message-in-a-queue"></a>Lesen von Nachrichten in einer Warteschlange

Sie können die Nachricht an eine Warteschlange der Vorderseite einsehen, ohne aus der Warteschlange entfernt werden, indem Sie die Methode PeekMessage() aufrufen.

    // Peek at the next message
    CloudQueueMessage peekedMessage = messageQueue.PeekMessage();

## <a name="read-and-remove-a-message-in-a-queue"></a>Lesen Sie und entfernen Sie eine Nachricht in einer Warteschlange zu.

Code entfernen kann (Warteschlange) eine Nachricht von einer Warteschlange in zwei Schritten.
1. Rufen Sie GetMessage(), um die nächste Nachricht in einer Warteschlange zu erhalten. Eine Meldung, die von GetMessage() zurückgegeben wird für alle anderen Code Lesen von Nachrichten aus dieser Warteschlange nicht sichtbar. Standardmäßig bleibt diese Meldung 30 Sekunden nicht sichtbar.
2.  Wenn Sie fertig sind, wobei die Nachricht aus der Warteschlange entfernt, rufen Sie **DeleteMessage**aus.

Diese zwei Verfahren zum Entfernen einer Nachricht wird sichergestellt, dass Code nicht verarbeiten einer Nachricht aufgrund von Fehlern Hardware oder Software, einer anderen Instanz des Codes kann dieselbe Nachricht erhalten und versuchen Sie es erneut. Der folgende Code ruft **DeleteMessage** rechts, nachdem die Nachricht verarbeitet wurde.

    // Get the next message in the queue.
    CloudQueueMessage retrievedMessage = messageQueue.GetMessage();

    // Process the message in less than 30 seconds

    // Then delete the message.
    await messageQueue.DeleteMessage(retrievedMessage);


## <a name="use-additional-options-for-de-queuing-messages"></a>Verwenden Sie zusätzliche Optionen für Warteschlange Nachrichten

Es gibt zwei Möglichkeiten, die Sie aus einer Warteschlange Nachrichtenübermittlung anpassen können.
Zunächst erhalten Sie eine Reihe von Nachrichten (bis zu 32). Zweites, können Sie einen Timeout verlängern oder verkürzen Invisibility festlegen, gleicht der Code mehr oder weniger jede Nachricht vollständig Verarbeitungszeit. Im folgenden Code wird verwendet die **GetMessages** Methode, um 20 Nachrichten in einem Anruf zu gelangen. Dann wird jede Nachricht mit einer **Foreach** -Schleife verarbeitet. Invisibility Timeout wird auch auf fünf Minuten für jede Nachricht festgelegt. Beachten Sie, dass das beginnt 5 Minuten für alle Nachrichten zur gleichen Zeit, nach 5 Minuten haben übergeben, da des Anrufs an die **GetMessages**, keine Nachrichten, die nicht gelöscht wurden erneut angezeigt werden soll.

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    foreach (CloudQueueMessage message in queue.GetMessages(20, TimeSpan.FromMinutes(5)))
    {
        // Process all messages in less than 5 minutes, deleting each message after processing.
        queue.DeleteMessage(message);
    }

## <a name="get-the-queue-length"></a>Abrufen der Länge der Warteschlange

Sie können eine Schätzung der Anzahl von Nachrichten in einer Warteschlange abrufen. Die Methode **FetchAttributes** gefragt werden, die Queueservice zum Abrufen der Warteschlange-Attributen, die Anzahl der Nachricht einschließlich. Die Eigenschaft **ApproximateMethodCount** gibt den letzten Wert von der Methode **FetchAttributes** abgerufen werden, ohne die Queueservice aufzurufen.

    // Fetch the queue attributes.
    messageQueue.FetchAttributes();

    // Retrieve the cached approximate message count.
    int? cachedMessageCount = messageQueue.ApproximateMessageCount;

    // Display number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## <a name="use-async-await-pattern-with-common-queueapis"></a>Verwenden Sie Muster asynchronen erwartet mit allgemeine queueAPIs

Dieses Beispiel zeigt, wie das Muster asynchronen erwartet mit allgemeinen QueueAPIs verwendet wird. Im Beispiel ruft die asynchrone Version des jeweiligen der angegebenen Methoden, dies kann nach der asynchronen Fix der einzelnen Methoden in angezeigt werden. Eine asynchrone Methode wird verwendet, wenn die Rollen-wartet auf Muster ausgesetzt lokale Ausführung bis zum Abschluss des Anrufs. Dieses Verhalten kann es sich um den aktuellen Thread andere Arbeiten fortsetzen Leistungsengpässe vermieden und der gesamten Reaktionszeiten der Anwendung verbessert. Weitere Informationen zur Verwendung des Musters asynchronen-Await in .NET [asynchrone und Await (C#- und Visual Basic)] finden Sie unter (https://msdn.microsoft.com/library/hh191443.aspx)

    // Create a message to put in the queue
    CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

    // Async enqueue the message
    await messageQueue.AddMessageAsync(cloudQueueMessage);
    Console.WriteLine("Message added");

    // Async dequeue the message
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();
    Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

    // Async delete the message
    await messageQueue.DeleteMessageAsync(retrievedMessage);
    Console.WriteLine("Deleted message");

## <a name="delete-a-queue"></a>Löschen einer Warteschlange

Rufen Sie zum Löschen einer Warteschlange und alle darin enthaltenen Nachrichten die **Löschen** -Methode für die Warteschlange-Objekt ein.

    // Delete the queue.
    messageQueue.Delete();

## <a name="next-steps"></a>Nächste Schritte

[AZURE.INCLUDE [vs-storage-dotnet-queues-next-steps](../../includes/vs-storage-dotnet-queues-next-steps.md)]
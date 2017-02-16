<properties
    pageTitle="Erste Schritte mit Warteschlangenspeicher und Visual Studio verbunden Services (Cloud Services) | Microsoft Azure"
    description="Erste Schritte mit Azure Warteschlange-Speicher in einem Projekt in Visual Studio Cloud-Dienst nach dem Herstellen einer Verbindung mit einem Speicherkonto mithilfe von Visual Studio verbunden services"
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

# <a name="getting-started-with-azure-queue-storage-and-visual-studio-connected-services-cloud-services-projects"></a>Erste Schritte mit Azure Warteschlange-Speicher und Visual Studio verbunden Services (Cloud services Projekte)

[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>(Übersicht)

In diesem Artikel werden die Schritte in Visual Studio Warteschlange Azure-Speicher verwenden, nachdem Sie erstellt oder auf die verwiesen wird ein Konto Azure-Speicher in einen Cloud Services-Projekt mithilfe des Dialogfelds Visual Studio **Verbunden Dienste hinzufügen** .

Wir zeigen Ihnen zum Erstellen einer Warteschlange im Code. Wir werden Sie auch wie grundlegende Warteschlangenoperationen, wie z. B. hinzufügen, ändern, lesen und Entfernen von Warteschlangennachrichten anzeigen. Die Beispiele in C#-Code geschrieben sind, und verwenden Sie die [Microsoft Azure-Speicher-Client-Bibliothek für .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).

Der Vorgang **Verbunden Dienste hinzufügen** Installationen die entsprechenden NuGet Pakete Azure-Speicher im Projekt zugreifen und Projektdateien Konfiguration die Verbindungszeichenfolge für den Speicherkonto hinzugefügt.

 - Weitere Informationen zum Bearbeiten von Warteschlangen Code finden Sie unter [Erste Schritte mit Azure Warteschlange-Speicher mit .NET](storage-dotnet-how-to-use-queues.md) .
 - Allgemeine Informationen zur Azure-Speicher finden Sie unter [Speicher-Dokumentation](https://azure.microsoft.com/documentation/services/storage/) .
 - Allgemeine Informationen zu Azure Cloud Services finden Sie unter [Cloud Services-Dokumentation](https://azure.microsoft.com/documentation/services/cloud-services/) .
 - Weitere Informationen zu ASP.NET Applications programming finden Sie unter [ASP.NET](http://www.asp.net) .


Azure Warteschlange-Speicher ist ein Dienst für das Speichern einer großen Anzahl von Nachrichten, die über eine beliebige Stelle in der Welt über authentifizierten Anrufe mit HTTP oder HTTPS zugegriffen werden können. Eine einzelne Warteschlange Nachricht kann bis zu 64 KB sein, und eine Warteschlange kann Millionen von Nachrichten, auf die gesamte Kapazität Beschränkung eines Kontos Speicher enthalten.


## <a name="access-queues-in-code"></a>Access-Warteschlangen Code

Um Warteschlangen in Visual Studio Cloud Services-Projekte zugreifen zu können, müssen Sie die folgenden Elemente zu C#-Quelldatei, enthalten, die Warteschlange Azure-Speicher zugreifen.

1. Stellen Sie sicher, dass die Namespace-Deklaration am oberen Rand der C#-Datei diese Anweisungen **verwenden** einschließen.

        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Queue;

2. Abrufen einer **CloudStorageAccount** -Objekt, das Ihre Kontoinformationen Speicher darstellt. Verwenden Sie den folgenden Code zum Abrufen der Ihre Verbindungszeichenfolge Speicher und die Speicher-Kontoinformationen aus der Konfiguration Azure Service.

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

3. Abrufen eines **CloudQueueClient** -Objekts in der Warteschlangenobjekte in Ihr Speicherkonto zu verweisen.  

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

4. Abrufen eines Objekts **CloudQueue** Verweis auf eine bestimmte Warteschlange an.

        // Get a reference to a queue named "messageQueue"
        CloudQueue messageQueue = queueClient.GetQueueReference("messageQueue");


**Hinweis:** Verwenden Sie alle obigen Code vor den Code in den folgenden Beispielen aus.

## <a name="create-a-queue-in-code"></a>Erstellen einer Warteschlange Code

Um die Warteschlange Code zu erstellen, fügen Sie nur einen Anruf an **CreateIfNotExists**.

    // Create the CloudQueue if it does not exist
    messageQueue.CreateIfNotExists();

## <a name="add-a-message-to-a-queue"></a>Fügen Sie eine Nachricht an eine Warteschlange

Klicken Sie zum Einfügen einer Nachricht in eine vorhandene Warteschlange Erstellen eines neuen **CloudQueueMessage** -Objekts und dann die **AddMessage** -Methode aufrufen.

Ein Objekt **CloudQueueMessage** kann aus einer Zeichenfolge (im UTF-8-Format) oder ein Byte-Array erstellt werden.

Hier ist ein Beispiel für den die Nachricht "Hallo, Welt eingefügt.

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    messageQueue.AddMessage(message);

## <a name="read-a-message-in-a-queue"></a>Lesen von Nachrichten in einer Warteschlange

Sie können die Nachricht an eine Warteschlange der Vorderseite einsehen, ohne durch Aufrufen der Methode **PeekMessage** aus der Warteschlange entfernt.

    // Peek at the next message
    CloudQueueMessage peekedMessage = messageQueue.PeekMessage();

## <a name="read-and-remove-a-message-in-a-queue"></a>Lesen Sie und entfernen Sie eine Nachricht in einer Warteschlange zu.

Code entfernen kann (Warteschlange) eine Nachricht von einer Warteschlange in zwei Schritten.

1. Rufen Sie **GetMessage** , um die nächste Nachricht in einer Warteschlange zu erhalten. Für alle anderen Code Lesen von Nachrichten aus dieser Warteschlange unsichtbar eine Nachricht von **GetMessage** zurückgegeben. Standardmäßig bleibt diese Meldung 30 Sekunden nicht sichtbar.
2.  Wenn Sie fertig sind, wobei die Nachricht aus der Warteschlange entfernt, rufen Sie **DeleteMessage**aus.

Diese zwei Verfahren zum Entfernen einer Nachricht wird sichergestellt, dass Code nicht verarbeiten einer Nachricht aufgrund von Fehlern Hardware oder Software, einer anderen Instanz des Codes kann dieselbe Nachricht erhalten und versuchen Sie es erneut. Der folgende Code ruft **DeleteMessage** rechts, nachdem die Nachricht verarbeitet wurde.

    // Get the next message in the queue.
    CloudQueueMessage retrievedMessage = messageQueue.GetMessage();

    // Process the message in less than 30 seconds

    // Then delete the message.
    await messageQueue.DeleteMessage(retrievedMessage);


## <a name="use-additional-options-to-process-and-remove-queue-messages"></a>Verwenden Sie zusätzliche Optionen zum Verarbeiten und Nachrichten in Warteschlange entfernen

Es gibt zwei Möglichkeiten, die Sie aus einer Warteschlange Nachrichtenübermittlung anpassen können.

 - Sie können eine Reihe von Nachrichten (bis zu 32) erhalten.
 - Sie können festlegen, einen Timeout verlängern oder verkürzen Invisibility zulassen von Ihrem Code mehr oder weniger jede Nachricht vollständig Verarbeitungszeit. Im folgenden Code wird verwendet die **GetMessages** Methode, um 20 Nachrichten in einem Anruf zu gelangen. Dann wird jede Nachricht mit einer **Foreach** -Schleife verarbeitet. Invisibility Timeout wird auch auf fünf Minuten für jede Nachricht festgelegt. Beachten Sie, dass das beginnt 5 Minuten für alle Nachrichten zur gleichen Zeit, nach 5 Minuten haben übergeben, da des Anrufs an die **GetMessages**, keine Nachrichten, die nicht gelöscht wurden erneut angezeigt werden soll.

Hier ist ein Beispiel:

    foreach (CloudQueueMessage message in messageQueue.GetMessages(20, TimeSpan.FromMinutes(5)))
    {
        // Process all messages in less than 5 minutes, deleting each message after processing.

        // Then delete the message after processing
        messageQueue.DeleteMessage(message);

    }

## <a name="get-the-queue-length"></a>Abrufen der Länge der Warteschlange

Sie können eine Schätzung der Anzahl von Nachrichten in einer Warteschlange abrufen. Die Methode **FetchAttributes** gefragt werden, zum Abrufen der Warteschlange-Attributen, die Anzahl der Nachricht einschließlich den Warteschlange-Dienst. Die Eigenschaft **ApproximateMethodCount** gibt den letzten Wert von der **FetchAttributes** -Methode ohne Aufrufen des Diensts Warteschlange abgerufen.

    // Fetch the queue attributes.
    messageQueue.FetchAttributes();

    // Retrieve the cached approximate message count.
    int? cachedMessageCount = messageQueue.ApproximateMessageCount;

    // Display number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## <a name="use-the-async-await-pattern-with-common-azure-queue-apis"></a>Verwenden Sie das Muster asynchronen erwartet mit allgemeine Azure Warteschlange-APIs

Dieses Beispiel zeigt, wie das Muster asynchronen erwartet mit allgemeine Azure Warteschlange-APIs verwendet wird. Im Beispiel ruft die asynchrone Version des jeweiligen der angegebenen Methoden, dies kann nach der **asynchronen** Fix der einzelnen Methoden in angezeigt werden. Eine asynchrone Methode wird verwendet, wenn die Rollen-wartet auf Muster ausgesetzt lokale Ausführung bis zum Abschluss des Anrufs. Dieses Verhalten kann es sich um den aktuellen Thread andere Arbeiten fortsetzen Leistungsengpässe vermieden und der gesamten Reaktionszeiten der Anwendung verbessert. Weitere Informationen zur Verwendung des Musters asynchronen-Await in .NET [asynchrone und Await (C#- und Visual Basic)] finden Sie unter (https://msdn.microsoft.com/library/hh191443.aspx)

    // Create a message to put in the queue
    CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

    // Add the message asynchronously
    await messageQueue.AddMessageAsync(cloudQueueMessage);
    Console.WriteLine("Message added");

    // Async dequeue the message
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();
    Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

    // Delete the message asynchronously
    await messageQueue.DeleteMessageAsync(retrievedMessage);
    Console.WriteLine("Deleted message");

## <a name="delete-a-queue"></a>Löschen einer Warteschlange

Rufen Sie zum Löschen einer Warteschlange und alle darin enthaltenen Nachrichten die **Löschen** -Methode für die Warteschlange-Objekt ein.

    // Delete the queue.
    messageQueue.Delete();

## <a name="next-steps"></a>Nächste Schritte

[AZURE.INCLUDE [vs-storage-dotnet-queues-next-steps](../../includes/vs-storage-dotnet-queues-next-steps.md)]

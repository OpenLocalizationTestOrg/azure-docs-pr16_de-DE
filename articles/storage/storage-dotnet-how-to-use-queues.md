<properties
    pageTitle="Erste Schritte mit Azure Warteschlange-Speicher mit .NET | Microsoft Azure"
    description="Azure Warteschlangen bieten zuverlässigen, asynchrone messaging zwischen Anwendungskomponenten. Cloud messaging ermöglicht Ihrer Anwendungskomponenten unabhängig voneinander zu skalieren."
    services="storage"
    documentationCenter=".net"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/12/2016"
    ms.author="robinsh"/>

# <a name="get-started-with-azure-queue-storage-using-net"></a>Erste Schritte mit Azure Warteschlange-Speicher mit .NET

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>(Übersicht)

Azure Warteschlange-Speicher bietet Cloud messaging zwischen Anwendungskomponenten. Beim Entwerfen für die Skalierung Applications aus, sind Anwendungskomponenten häufig abgekoppelt, damit diese unabhängig voneinander zu skalieren können. Warteschlangenspeicher bietet asynchrones messaging für die Kommunikation zwischen Komponenten der Anwendung, ob sie in der Cloud, auf dem Desktop, klicken Sie auf einem lokalen Server oder auf einem mobilen Gerät ausgeführt werden. Warteschlangenspeicher unterstützt auch asynchrone Aufgaben verwalten und Prozess Arbeit Zahlungen erstellen.

### <a name="about-this-tutorial"></a>Informationen zu diesem Lernprogramm

In diesem Lernprogramm erfahren, wie .NET Code für einige häufige Szenarien mit Azure Warteschlange Speicher geschrieben. Verdeckt Szenarien enthalten, erstellen und Löschen von Warteschlangen hinzufügen, lesen und Löschen von Nachrichten in Warteschlange.

**Geschätzte Dauer:** 45 Minuten

**Prerequisities:**

- [Microsoft Visual Studio](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx)
- [Client-Bibliothek für .NET Azure-Speicher](https://www.nuget.org/packages/WindowsAzure.Storage/)
- [Azure-Konfigurations-Manager für .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
- Ein [Konto Azure-Speicher](storage-create-storage-account.md#create-a-storage-account)


[AZURE.INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[AZURE.INCLUDE [storage-development-environment-include](../../includes/storage-development-environment-include.md)]

### <a name="add-namespace-declarations"></a>Fügen Sie Namespacedeklarationen

Fügen Sie den folgenden `using` Anweisungen am oberen Rand der `program.cs` Datei:

    using Microsoft.Azure; // Namespace for CloudConfigurationManager
    using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
    using Microsoft.WindowsAzure.Storage.Queue; // Namespace for Queue storage types

### <a name="parse-the-connection-string"></a>Analysieren Sie die Verbindungszeichenfolge

[AZURE.INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-the-queue-service-client"></a>Erstellen des Warteschlange-Service-Clients

Die **CloudQueueClient** -Klasse können Sie zum Abrufen von Warteschlangen in Warteschlange-Speicher gespeichert. Hier ist eine Methode, um den Service-Client zu erstellen:

    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

Jetzt sind Sie bereit sind, Schreiben von Code, die Daten aus liest und schreibt Daten in Warteschlange-Speicher.

## <a name="create-a-queue"></a>Erstellen einer Warteschlange

In diesem Beispiel wird gezeigt, wie eine Warteschlange erstellen, wenn es nicht bereits vorhanden ist:

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a container.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Create the queue if it doesn't already exist
    queue.CreateIfNotExists();

## <a name="insert-a-message-into-a-queue"></a>Fügen Sie eine Nachricht in einer Warteschlange

Zum Einfügen einer Nachricht in eine vorhandene Warteschlange erstellen Sie zuerst eine neue **CloudQueueMessage**. Rufen Sie als Nächstes die **AddMessage** -Methode aus. Eine **CloudQueueMessage** kann aus einer Zeichenfolge (im UTF-8-Format) oder ein **Byte** -Array erstellt werden. Hier Code, der eine Warteschlange erstellt (wenn nicht vorhanden) und fügt die Meldung "Hallo, Welt:

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Create the queue if it doesn't already exist.
    queue.CreateIfNotExists();

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    queue.AddMessage(message);

## <a name="peek-at-the-next-message"></a>Vorschau der nächsten Nachricht

Sie können die Nachricht an eine Warteschlange der Vorderseite einsehen, ohne durch Aufrufen der Methode **PeekMessage** aus der Warteschlange entfernt.

    // Retrieve storage account from connection string
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Peek at the next message
    CloudQueueMessage peekedMessage = queue.PeekMessage();

    // Display message.
    Console.WriteLine(peekedMessage.AsString);

## <a name="change-the-contents-of-a-queued-message"></a>Ändern des Inhalts einer Nachricht in der Warteschlange

Sie können den Inhalt einer Nachricht in-situ in der Warteschlange ändern. Wenn die Nachricht eine Aufgabe darstellt, können Sie dieses Feature, um den Status des Vorgangs Arbeit aktualisieren. Mit dem folgende Code die Warteschlange-Nachricht mit neuen Inhalt aktualisiert, und legt das Timeout Sichtbarkeit, um weitere 60 Sekunden zu erweitern. Dies speichert den Status der Arbeit mit der Nachricht zugeordnet ist, und bietet dem Kunden ein anderes Minute, um die Arbeit der Nachricht fortzusetzen. Dieses Verfahren können das Nachverfolgen von Workflows mit mehreren Schritten in Warteschlangennachrichten, ohne ein Verarbeitungsschritt schlägt aufgrund von Fehlern Hardware oder Software ab dem Anfang beginnen. In der Regel würden Sie auch "Wiederholen" Anzahl behalten, und wenn die Nachricht mehr als *n* Mal wiederholt wird, möchten Sie es löschen. Hierdurch wird Schutz vor einer Nachricht, die einen Anwendungsfehler jedes Mal auslöst, die sie verarbeitet wird.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Get the message from the queue and update the message contents.
    CloudQueueMessage message = queue.GetMessage();
    message.SetMessageContent("Updated contents.");
    queue.UpdateMessage(message,
        TimeSpan.FromSeconds(60.0),  // Make it visible for another 60 seconds.
        MessageUpdateFields.Content | MessageUpdateFields.Visibility);

## <a name="de-queue-the-next-message"></a>Die nächste Nachricht Warteschlange

Code Warteschlange eine Nachricht von einer Warteschlange in zwei Schritten. Wenn Sie **GetMessage**aufrufen, erhalten Sie die nächste Nachricht in einer Warteschlange. Eine Nachricht von **GetMessage** zurückgegeben wird für alle anderen Code Lesen von Nachrichten aus dieser Warteschlange nicht sichtbar. Standardmäßig bleibt diese Meldung 30 Sekunden nicht sichtbar. Wenn Sie fertig sind, wobei die Nachricht aus der Warteschlange entfernt, müssen Sie auch **DeleteMessage**aufrufen. Diese zwei Verfahren zum Entfernen einer Nachricht wird sichergestellt, dass Code nicht verarbeiten einer Nachricht aufgrund von Fehlern Hardware oder Software, einer anderen Instanz des Codes kann dieselbe Nachricht erhalten und versuchen Sie es erneut. Code ruft **DeleteMessage** rechts, nachdem die Nachricht verarbeitet wurde.

    // Retrieve storage account from connection string
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Get the next message
    CloudQueueMessage retrievedMessage = queue.GetMessage();

    //Process the message in less than 30 seconds, and then delete the message
    queue.DeleteMessage(retrievedMessage);

## <a name="use-async-await-pattern-with-common-queue-storage-apis"></a>Verwenden Sie Muster asynchronen erwartet mit allgemeine Warteschlangenspeicher-APIs

In diesem Beispiel wird gezeigt, wie das Muster asynchronen erwartet mit allgemeine Warteschlangenspeicher-APIs verwenden. Im Beispiel ruft die asynchrone Version der beiden angegebenen Methoden, wie durch das Suffix *asynchrone* der einzelnen Methoden angezeigt. Wenn eine asynchrone Methode verwendet wird, die Rollen-wartet auf Muster ausgesetzt lokale Ausführung bis zum Abschluss des Anrufs. Dieses Verhalten kann die aktuellen Thread andere arbeiten, führen Sie die Leistungsengpässe vermieden und der gesamten Reaktionszeiten der Anwendung verbessert. Weitere Informationen zur Verwendung des Musters asynchronen-Await in .NET finden Sie unter [asynchrone und Await (C#- und Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx)

    // Create the queue if it doesn't already exist
    if(await queue.CreateIfNotExistsAsync())
    {
        Console.WriteLine("Queue '{0}' Created", queue.Name);
    }
    else
    {
        Console.WriteLine("Queue '{0}' Exists", queue.Name);
    }

    // Create a message to put in the queue
    CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

    // Async enqueue the message
    await queue.AddMessageAsync(cloudQueueMessage);
    Console.WriteLine("Message added");

    // Async dequeue the message
    CloudQueueMessage retrievedMessage = await queue.GetMessageAsync();
    Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

    // Async delete the message
    await queue.DeleteMessageAsync(retrievedMessage);
    Console.WriteLine("Deleted message");

## <a name="leverage-additional-options-for-de-queuing-messages"></a>Nutzen Sie zusätzliche Optionen für Warteschlange Nachrichten

Es gibt zwei Möglichkeiten, die Sie aus einer Warteschlange Nachrichtenübermittlung anpassen können.
Zunächst erhalten Sie eine Reihe von Nachrichten (bis zu 32). Zweites, können Sie einen Timeout verlängern oder verkürzen Invisibility festlegen, gleicht der Code mehr oder weniger jede Nachricht vollständig Verarbeitungszeit. Im folgenden Code wird verwendet die **GetMessages** Methode, um 20 Nachrichten in einem Anruf zu gelangen. Dann wird jede Nachricht mit einer **Foreach** -Schleife verarbeitet. Invisibility Timeout wird auch auf fünf Minuten für jede Nachricht festgelegt. Beachten Sie, dass das beginnt 5 Minuten für alle Nachrichten zur gleichen Zeit, nach 5 Minuten haben übergeben, da des Anrufs an die **GetMessages**, keine Nachrichten, die nicht gelöscht wurden erneut angezeigt werden soll.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

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

Sie können eine Schätzung der Anzahl von Nachrichten in einer Warteschlange abrufen. Die Methode **FetchAttributes** gefragt werden, zum Abrufen der Warteschlange-Attributen, die Anzahl der Nachricht einschließlich den Warteschlange-Dienst. Die Eigenschaft **ApproximateMessageCount** gibt den letzten Wert von der **FetchAttributes** -Methode ohne Aufrufen des Diensts Warteschlange abgerufen.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Fetch the queue attributes.
    queue.FetchAttributes();

    // Retrieve the cached approximate message count.
    int? cachedMessageCount = queue.ApproximateMessageCount;

    // Display number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## <a name="delete-a-queue"></a>Löschen einer Warteschlange

Rufen Sie zum Löschen einer Warteschlange und alle darin enthaltenen Nachrichten die **Löschen** -Methode für die Warteschlange-Objekt ein.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Delete the queue.
    queue.Delete();

## <a name="next-steps"></a>Nächste Schritte

Die Grundlagen der Warteschlangenspeicher bearbeitet haben, folgen Sie diesen Links komplexere Speicheraufgaben Lernen aus.

- Anzeigen der Warteschlange Bezug servicedokumentation ausführliche Informationen zu den verfügbaren APIs:
    - [Speicher-Client-Bibliothek für .NET Verweis](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
    - [REST-API-Referenz](http://msdn.microsoft.com/library/azure/dd179355)
- Erfahren Sie, wie Sie den Code zu vereinfachen, die, den Sie für die Arbeit mit Azure-Speicher, mit dem [Azure WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk.md)schreiben.
- Zeigen Sie weitere Features Führungslinien zusätzliche Optionen zum Speichern von Daten in Azure lernen an
    - [Erste Schritte mit Azure Tabellenspeicher mit .NET](storage-dotnet-how-to-use-tables.md) , um strukturierte Daten zu speichern.
    - [Erste Schritte mit Azure Blob-Speicher mit .NET](storage-dotnet-how-to-use-blobs.md) , unstrukturierte Daten gespeichert.
    - [Verbinden mit SQL-Datenbank mithilfe von .NET (c#)](../sql-database/sql-database-develop-dotnet-simple.md) zum Speichern von relationaler Daten.

  [Download and install the Azure SDK for .NET]: /develop/net/
  [.NET client library reference]: http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409
  [Creating a Azure Project in Visual Studio]: http://msdn.microsoft.com/library/azure/ee405487.aspx
  [Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
  [OData]: http://nuget.org/packages/Microsoft.Data.OData/5.0.2
  [Edm]: http://nuget.org/packages/Microsoft.Data.Edm/5.0.2
  [Spatial]: http://nuget.org/packages/System.Spatial/5.0.2

<properties
    pageTitle="So verwenden Sie Warteschlange-Speicher von Java | Microsoft Azure"
    description="Informationen zum Verwenden der Warteschlange Azure Service erstellen und Löschen von Warteschlangen, einfügen, abrufen und Löschen von Nachrichten. Die Beispiele in Java geschrieben."
    services="storage"
    documentationCenter="java"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robinsh"/>

# <a name="how-to-use-queue-storage-from-java"></a>So verwenden Sie Warteschlange-Speicher von Java

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>(Übersicht)

In diesem Handbuch wird gezeigt, wie häufige Szenarien, die mit der Warteschlange Azure-Speicherdienst ausführen werden. Die Beispiele in Java geschrieben sind, und verwenden Sie den [Azure-Speicher SDK für Java][]. Die Szenarios dieser gehören **Einfügen**, **Empfang**, **Abrufen**und **Löschen von** Nachrichten in Warteschlange sowie **Erstellen** und **Löschen von** Warteschlangen. Weitere Informationen zum Warteschlangen finden Sie im Abschnitt für die [nächsten Schritte](#Next-Steps) .

Hinweis: Ein SDK steht für Entwickler, die Azure-Speicher auf Android-Geräten verwenden. Weitere Informationen finden Sie unter der [Azure-Speicher SDK für Android][].

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a>Erstellen einer Java-Anwendungs

In diesem Handbuch Verwenden Sie Speicher Features die innerhalb einer Java-Anwendung lokal oder innerhalb einer Webrolle oder Worker-Rolle in Azure ausgeführt Code ausgeführt werden kann.

Dazu müssen Sie Java Development Kit (JDK) installieren und erstellen Sie ein Azure-Speicher-Konto in Ihrem Azure-Abonnement. Nachdem Sie getan haben, müssen Sie überprüfen, ob Ihr Entwicklungssystem erfüllt die Mindestanforderungen und Abhängigkeiten, die im Repository [Azure Speicher SDK für Java][] auf GitHub aufgeführt sind. Wenn das System über diese Anforderungen erfüllt, können Sie die Anweisungen zum Herunterladen und Installieren der Azure-Speicherbibliotheken für Java auf Ihrem System aus diesem Repository folgen. Nachdem Sie diese Aufgaben abgeschlossen haben, werden Sie möglicherweise eine Java-Anwendung zu erstellen, die in den Beispielen in diesem Artikel verwendet.

## <a name="configure-your-application-to-access-queue-storage"></a>Konfigurieren der Anwendungs Zugriff auf Warteschlangenspeicher

Fügen Sie die folgenden Aussagen importieren an den Anfang der Java-Datei, die Sie Azure-Speicher-APIs verwenden, um Warteschlangen zuzugreifen möchten:

    // Include the following imports to use queue APIs.
    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.queue.*;

## <a name="setup-an-azure-storage-connection-string"></a>Einrichten einer Verbindungszeichenfolge Azure-Speicher

Ein Azure-Speicher-Client verwendet eine Verbindungszeichenfolge Speicher zum Speichern von Endpunkten und die Anmeldeinformationen für den Zugriff auf Daten Management Services. Wenn Sie in einer Clientanwendung ausführen zu können, müssen Sie die Verbindungszeichenfolge Speicher in folgendem Format ein, bereitstellen, verwenden den Namen der Ihr Speicherkonto und die primäre Zugriffstaste für das Speicherkonto im [Azure-Portal](https://portal.azure.com) für die Werte *Kontoname* und *AccountKey* aufgeführt. Dieses Beispiel zeigt, wie Sie ein statisches Feld zu halten Sie die Verbindungszeichenfolge manuell deklarieren können:

    // Define the connection-string with your values.
    public static final String storageConnectionString =
        "DefaultEndpointsProtocol=http;" +
        "AccountName=your_storage_account;" +
        "AccountKey=your_storage_account_key";

In einer Anwendung, die innerhalb einer Rolle in Microsoft Azure ausgeführt wird wird diese Zeichenfolge in der Konfiguration Dienstdatei *ServiceConfiguration.cscfg*, gespeichert werden kann und einen Anruf an die Methode **RoleEnvironment.getConfigurationSettings** zugegriffen werden kann. Hier ist ein Beispiel für die Verbindungszeichenfolge aus einer **Einstellung** Element mit dem Namen *StorageConnectionString* in der Konfiguration Dienstdatei erste aus:

    // Retrieve storage account from connection-string.
    String storageConnectionString =
        RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");

In den folgenden Beispielen wird davon ausgegangen, dass Sie eine der folgenden beiden Methoden zum Abrufen der Verbindungszeichenfolge Speicher verwendet haben.

## <a name="how-to-create-a-queue"></a>So: Erstellen einer Warteschlange

Ein Objekt **CloudQueueClient** können Sie die Verweisobjekte für Warteschlangen zu erhalten. Der folgende Code erstellt ein **CloudQueueClient** Objekt. (Notiz: Es werden weitere Methoden zum Erstellen von **CloudStorageAccount** Objekte; Weitere Informationen finden Sie unter **CloudStorageAccount** in den [Azure-Speicher Client SDK-Referenz].)

Verwenden Sie das Objekt **CloudQueueClient** , um einen Verweis auf die Warteschlange zu erhalten, die Sie verwenden möchten. Sie können die Warteschlange erstellen, wenn es nicht vorhanden ist.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

       // Create the queue client.
       CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

       // Retrieve a reference to a queue.
       CloudQueue queue = queueClient.getQueueReference("myqueue");

       // Create the queue if it doesn't already exist.
       queue.createIfNotExists();
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-add-a-message-to-a-queue"></a>So: hinzufügen eine Nachricht an eine Warteschlange

Zum Einfügen einer Nachricht in eine vorhandene Warteschlange erstellen Sie zuerst eine neue **CloudQueueMessage**. Rufen Sie als Nächstes die **AddMessage** -Methode aus. Eine **CloudQueueMessage** kann aus einer Zeichenfolge (im UTF-8-Format) oder ein Byte-Array erstellt werden. Hier Code, der eine Warteschlange erstellt (wenn nicht vorhanden) und fügt die Meldung "Hallo, Welt".

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Create the queue if it doesn't already exist.
        queue.createIfNotExists();

        // Create a message and add it to the queue.
        CloudQueueMessage message = new CloudQueueMessage("Hello, World");
        queue.addMessage(message);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-peek-at-the-next-message"></a>So: Einsehen der nächsten Nachricht

Sie können die Nachricht an eine Warteschlange der Vorderseite ohne aus der Warteschlange entfernt werden, indem Sie **PeekMessage**einsehen.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Peek at the next message.
        CloudQueueMessage peekedMessage = queue.peekMessage();

        // Output the message value.
        if (peekedMessage != null)
        {
          System.out.println(peekedMessage.getMessageContentAsString());
       }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-change-the-contents-of-a-queued-message"></a>So: Ändern des Inhalts einer Nachricht in der Warteschlange

Sie können den Inhalt einer Nachricht in-situ in der Warteschlange ändern. Wenn die Nachricht eine Aufgabe darstellt, können Sie dieses Feature, um den Status des Vorgangs Arbeit aktualisieren. Mit dem folgende Code die Warteschlange-Nachricht mit neuen Inhalt aktualisiert, und legt das Timeout Sichtbarkeit, um weitere 60 Sekunden zu erweitern. Dies speichert den Status der Arbeit mit der Nachricht zugeordnet ist, und bietet dem Kunden ein anderes Minute, um die Arbeit der Nachricht fortzusetzen. Dieses Verfahren können das Nachverfolgen von Workflows mit mehreren Schritten in Warteschlangennachrichten, ohne ein Verarbeitungsschritt schlägt aufgrund von Fehlern Hardware oder Software ab dem Anfang beginnen. In der Regel würden Sie auch "Wiederholen" Anzahl behalten, und wenn die Nachricht mehr als *n* Mal wiederholt wird, möchten Sie es löschen. Hierdurch wird Schutz vor einer Nachricht, die einen Anwendungsfehler jedes Mal auslöst, die sie verarbeitet wird.

Die folgenden Code Stichprobe Suchläufe durch die Warteschlange der Nachrichten, findet die erste Nachricht, die entspricht "Hallo, Welt" für den Inhalt die Nachricht Inhalt ändert und beendet wird.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // The maximum number of messages that can be retrieved is 32.
        final int MAX_NUMBER_OF_MESSAGES_TO_PEEK = 32;

        // Loop through the messages in the queue.
        for (CloudQueueMessage message : queue.retrieveMessages(MAX_NUMBER_OF_MESSAGES_TO_PEEK,1,null,null))
        {
            // Check for a specific string.
            if (message.getMessageContentAsString().equals("Hello, World"))
            {
                // Modify the content of the first matching message.
                message.setMessageContent("Updated contents.");
                // Set it to be visible in 30 seconds.
                EnumSet<MessageUpdateFields> updateFields =
                    EnumSet.of(MessageUpdateFields.CONTENT,
                    MessageUpdateFields.VISIBILITY);
                // Update the message.
                queue.updateMessage(message, 30, updateFields, null, null);
                break;
            }
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

Im folgenden Beispiel wird Alternativ nur die erste sichtbare Nachricht in der Warteschlange aktualisiert.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Retrieve the first visible message in the queue.
        CloudQueueMessage message = queue.retrieveMessage();

        if (message != null)
        {
            // Modify the message content.
            message.setMessageContent("Updated contents.");
            // Set it to be visible in 60 seconds.
            EnumSet<MessageUpdateFields> updateFields =
                EnumSet.of(MessageUpdateFields.CONTENT,
                MessageUpdateFields.VISIBILITY);
            // Update the message.
            queue.updateMessage(message, 60, updateFields, null, null);
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-get-the-queue-length"></a>So: Abrufen die Länge der Warteschlange

Sie können eine Schätzung der Anzahl von Nachrichten in einer Warteschlange abrufen. Die Methode **DownloadAttributes** gefragt werden, den Warteschlange-Dienst für mehrere aktuelle Werte, einschließlich der Anzahl der Anzahl der Nachrichten in der Warteschlange befinden. Die Anzahl die steht nur ungefähre, da Nachrichten hinzugefügt oder entfernt, nachdem der Warteschlange-Dienst Anforderung beantwortet werden können. Die Methode **GetApproximateMessageCount** gibt den letzten Wert, indem Sie den Anruf an **DownloadAttributes**, ohne Aufrufen des Diensts Warteschlange abgerufen.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

       // Download the approximate message count from the server.
        queue.downloadAttributes();

        // Retrieve the newly cached approximate message count.
        long cachedMessageCount = queue.getApproximateMessageCount();

        // Display the queue length.
        System.out.println(String.format("Queue length: %d", cachedMessageCount));
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-dequeue-the-next-message"></a>So: Abrufen die nächste Nachricht

Der Code übergibt eine Nachricht von einer Warteschlange in zwei Schritten. Wenn Sie **RetrieveMessage**aufrufen, erhalten Sie die nächste Nachricht in einer Warteschlange. Eine Meldung, die von **RetrieveMessage** zurückgegeben wird für alle anderen Code Lesen von Nachrichten aus dieser Warteschlange nicht sichtbar. Standardmäßig bleibt diese Meldung 30 Sekunden nicht sichtbar. Wenn Sie fertig sind, wobei die Nachricht aus der Warteschlange entfernt, müssen Sie auch **DeleteMessage**aufrufen. Diese zwei Verfahren zum Entfernen einer Nachricht wird sichergestellt, dass Code nicht verarbeiten einer Nachricht aufgrund von Fehlern Hardware oder Software, einer anderen Instanz des Codes kann dieselbe Nachricht erhalten und versuchen Sie es erneut. Code ruft **DeleteMessage** rechts, nachdem die Nachricht verarbeitet wurde.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Retrieve the first visible message in the queue.
        CloudQueueMessage retrievedMessage = queue.retrieveMessage();

        if (retrievedMessage != null)
        {
            // Process the message in less than 30 seconds, and then delete the message.
            queue.deleteMessage(retrievedMessage);
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }


## <a name="additional-options-for-dequeuing-messages"></a>Zusätzliche Optionen für Abarbeiten der Warteschlange Nachrichten

Es gibt zwei Möglichkeiten, die Sie aus einer Warteschlange Nachrichtenübermittlung anpassen können. Zunächst erhalten Sie eine Reihe von Nachrichten (bis zu 32). Zweites, können Sie einen Timeout verlängern oder verkürzen Invisibility festlegen, gleicht der Code mehr oder weniger jede Nachricht vollständig Verarbeitungszeit.

Im folgenden Code wird verwendet die **RetrieveMessages** Methode, um 20 Nachrichten in einem Anruf zu gelangen. Dann wird jede Nachricht mit einer Schleife **für** verarbeitet. Invisibility Timeout wird auch auf 5 Minuten (300 Sekunden) für jede Nachricht festgelegt. Notiz, die die fünf Minuten beginnt für alle Nachrichten gleichzeitig, also bei fünf Minuten bestanden wurden seit dem Aufrufen von **RetrieveMessages**, keine Nachrichten, die nicht gelöscht wurden sichtbar zu machen erneut.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Retrieve 20 messages from the queue with a visibility timeout of 300 seconds.
        for (CloudQueueMessage message : queue.retrieveMessages(20, 300, null, null)) {
            // Do processing for all messages in less than 5 minutes,
            // deleting each message after processing.
            queue.deleteMessage(message);
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-list-the-queues"></a>So: Liste die Warteschlangen

Um eine Liste mit den aktuellen Warteschlangen zu erhalten, rufen Sie die **CloudQueueClient.listQueues()** -Methode, die eine Auflistung von Objekten **CloudQueue** zurückgibt.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient =
            storageAccount.createCloudQueueClient();

        // Loop through the collection of queues.
        for (CloudQueue queue : queueClient.listQueues())
        {
            // Output each queue name.
            System.out.println(queue.getName());
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-delete-a-queue"></a>So: löschen eine Warteschlange

Rufen Sie die **DeleteIfExists** -Methode auf das Objekt **CloudQueue** , zum Löschen einer Warteschlange und alle darin enthaltenen Nachrichten.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Delete the queue if it exists.
        queue.deleteIfExists();
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="next-steps"></a>Nächste Schritte

Die Grundlagen der Warteschlangenspeicher bearbeitet haben, folgen Sie diesen Links komplexere Speicheraufgaben Lernen aus.

- [Azure SDK für Java-Speicher][]
- [Azure-Speicher Client SDK-Referenz][]
- [Azure-Speicherservices REST-API][]
- [Azure-Speicher-Teamblog][]

[Azure SDK for Java]: http://go.microsoft.com/fwlink/?LinkID=525671
[Azure SDK für Java-Speicher]: https://github.com/azure/azure-storage-java
[Azure-Speicher SDK für Android]: https://github.com/azure/azure-storage-android
[Azure-Speicher Client SDK-Referenz]: http://dl.windowsazure.com/storage/javadoc/
[Azure-Speicherservices REST-API]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Azure-Speicher-Teamblog]: http://blogs.msdn.com/b/windowsazurestorage/

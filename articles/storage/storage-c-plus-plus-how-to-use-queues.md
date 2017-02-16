<properties
    pageTitle="So verwenden Sie Warteschlangenspeicher (C++) | Microsoft Azure"
    description="Informationen Sie zum Verwenden der Warteschlange-Speicherdienst in Azure. Beispiele sind in C++ geschrieben."
    services="storage"
    documentationCenter=".net"
    authors="dineshmurthy"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="dineshm"/>

# <a name="how-to-use-queue-storage-from-c"></a>So Warteschlange-Speicher von C++ verwenden  

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>(Übersicht)
In diesem Handbuch wird gezeigt, wie häufige Szenarien, die mit der Warteschlange Azure-Speicherdienst ausführen werden. Die Beispiele in C++ geschrieben sind und [Azure-Speicher-Client-Bibliothek für C++](http://github.com/Azure/azure-storage-cpp/blob/master/README.md)verwenden. Die Szenarios dieser gehören **Einfügen**, **Empfang**, **Abrufen**und **Löschen von** Nachrichten in Warteschlange sowie **Erstellen und Löschen von Warteschlangen**.

>[AZURE.NOTE] Dieses Handbuch richtet sich an den Azure-Speicher Client-Bibliothek für C++ Version 1.0.0 und über. Die empfohlene Version ist Speicher Clientbibliothek 2.2.0, die über [NuGet](http://www.nuget.org/packages/wastorage) oder [GitHub](http://github.com/Azure/azure-storage-cpp/)verfügbar ist.

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]
[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a>Erstellen einer C++-Anwendungs  
In diesem Handbuch Verwenden Sie Speicher Features der in einer C++-Anwendung ausgeführt werden können.

Dazu müssen Sie die Bibliothek Azure-Speicher Client für C++ installieren und erstellen Sie ein Azure-Speicher-Konto in Ihrem Azure-Abonnement.

Um die Azure-Speicher-Client-Bibliothek für C++ installiert haben, können Sie die folgenden Methoden verwenden:

-   **Linux:** Anweisungen Sie in der [Azure Speicher Client für C++ Infos](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) Bibliotheksseite angegeben.
-   **Windows:** Klicken Sie in Visual Studio auf **Tools > NuGet-Paket-Manager > Paket-Manager-Konsole**. Geben Sie den folgenden Befehl in die [NuGet-Paket-Manager-Konsole](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) ein, und drücken Sie die **EINGABETASTE**.

        Install-Package wastorage

## <a name="configure-your-application-to-access-queue-storage"></a>Konfigurieren der Anwendungs Zugriff auf Warteschlange-Speicher
Fügen Sie die folgenden Anweisungen an den Anfang der Datei C++ einschließen Stelle mit der Azure-Speicher-APIs auf Warteschlangen zugreifen:  

    #include "was/storage_account.h"
    #include "was/queue.h"

## <a name="set-up-an-azure-storage-connection-string"></a>Einrichten einer Verbindungszeichenfolge Azure-Speicher

Ein Azure-Speicher-Client verwendet eine Verbindungszeichenfolge Speicher zum Speichern von Endpunkten und die Anmeldeinformationen für den Zugriff auf Daten Management Services. Wenn Sie in einer Clientanwendung ausführen zu können, müssen Sie angeben, die Verbindungszeichenfolge Speicher in folgendem Format ein, mit den Namen der Ihr Speicherkonto und die Speicher Zugriffstaste für das Speicherkonto im [Azure-Portal](https://portal.azure.com) für die Werte *Kontoname* und *AccountKey* aufgeführt. Informationen zu Speicherkonten und Tastenkombinationen finden Sie unter [Informationen zum Azure Speicherkonten](storage-create-storage-account.md). Dieses Beispiel zeigt, wie Sie ein statisches Feld zu halten Sie die Verbindungszeichenfolge manuell deklarieren können:  

    // Define the connection-string with your values.
    const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));

Klicken Sie zum Testen Ihrer Anwendungs in Ihrem lokalen Computer mit Windows können Sie den Microsoft Azure [Speicheremulator](storage-use-emulator.md) verwenden, die mit dem [Azure SDK](https://azure.microsoft.com/downloads/)installiert ist. Speicheremulator ist ein Programm, das die verfügbaren in Azure auf Ihrem Computer lokale Entwicklung Dienste Blob, Warteschlange und Tabelle simuliert. Das folgende Beispiel zeigt, wie Sie ein statisches Feld zu halten Sie die Verbindungszeichenfolge zu Ihrem lokalen Speicheremulator deklarieren können:  

    // Define the connection-string with Azure Storage Emulator.
    const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  

Um Azure Speicheremulator zu beginnen, wählen Sie die Schaltfläche " **Start** ", oder drücken Sie die **Windows** -Taste. Beginnen mit der Eingabe **Azure Speicheremulator**, und wählen Sie **Microsoft Azure Speicheremulator** aus der Liste der Programme.

In den folgenden Beispielen wird davon ausgegangen, dass Sie eine der folgenden beiden Methoden zum Abrufen der Verbindungszeichenfolge Speicher verwendet haben.

## <a name="retrieve-your-connection-string"></a>Abrufen der Verbindungszeichenfolge
**Cloud_storage_account** -Klasse können Ihre Speicher Kontoinformationen darstellen. Wenn Ihre Kontoinformationen Speicher aus der Verbindungszeichenfolge Speicher abrufen möchten, können Sie die Methode **Analysieren** .

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

## <a name="how-to-create-a-queue"></a>So: Erstellen einer Warteschlange
Ein Objekt **Cloud_queue_client** können Sie die Verweisobjekte für Warteschlangen zu erhalten. Der folgende Code erstellt ein **Cloud_queue_client** Objekt.

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create a queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

Verwenden Sie das Objekt **Cloud_queue_client** , um einen Verweis auf die Warteschlange zu erhalten, die Sie verwenden möchten. Sie können die Warteschlange erstellen, wenn es nicht vorhanden ist.

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Create the queue if it doesn't already exist.
    queue.create_if_not_exists();  

## <a name="how-to-insert-a-message-into-a-queue"></a>So: Einfügen eine Nachricht in einer Warteschlange
Zum Einfügen einer Nachricht in eine vorhandene Warteschlange erstellen Sie zuerst eine neue **Cloud_queue_message**. Rufen Sie als Nächstes die **Add_message** -Methode. Eine **Cloud_queue_message** kann aus einer Zeichenfolge oder ein **Byte** -Array erstellt werden. Hier Code, der eine Warteschlange erstellt (wenn nicht vorhanden) und fügt die Meldung "Hallo, Welt:

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Create the queue if it doesn't already exist.
    queue.create_if_not_exists();

    // Create a message and add it to the queue.
    azure::storage::cloud_queue_message message1(U("Hello, World"));
    queue.add_message(message1);  

## <a name="how-to-peek-at-the-next-message"></a>So: Einsehen der nächsten Nachricht
Sie können die Nachricht an eine Warteschlange der Vorderseite einsehen, ohne aus der Warteschlange entfernt werden, indem Sie die Methode **Peek_message** aufrufen.

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Peek at the next message.
    azure::storage::cloud_queue_message peeked_message = queue.peek_message();

    // Output the message content.
    std::wcout << U("Peeked message content: ") << peeked_message.content_as_string() << std::endl;

## <a name="how-to-change-the-contents-of-a-queued-message"></a>So: Ändern des Inhalts einer Nachricht in der Warteschlange
Sie können den Inhalt einer Nachricht in-situ in der Warteschlange ändern. Wenn die Nachricht eine Aufgabe darstellt, können Sie dieses Feature, um den Status des Vorgangs Arbeit aktualisieren. Mit dem folgende Code die Warteschlange-Nachricht mit neuen Inhalt aktualisiert, und legt das Timeout Sichtbarkeit, um weitere 60 Sekunden zu erweitern. Dies speichert den Status der Arbeit mit der Nachricht zugeordnet ist, und bietet dem Kunden ein anderes Minute, um die Arbeit der Nachricht fortzusetzen. Dieses Verfahren können das Nachverfolgen von Workflows mit mehreren Schritten in Warteschlangennachrichten, ohne ein Verarbeitungsschritt schlägt aufgrund von Fehlern Hardware oder Software ab dem Anfang beginnen. In der Regel würden Sie auch "Wiederholen" Anzahl behalten, und wenn die Nachricht mehr als n Mal wiederholt wird, möchten Sie es löschen. Hierdurch wird Schutz vor einer Nachricht, die einen Anwendungsfehler jedes Mal auslöst, die sie verarbeitet wird.

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_conection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Get the message from the queue and update the message contents.
    // The visibility timeout "0" means make it visible immediately.
    // The visibility timeout "60" means the client can get another minute to continue
    // working on the message.
    azure::storage::cloud_queue_message changed_message = queue.get_message();

    changed_message.set_content(U("Changed message"));
    queue.update_message(changed_message, std::chrono::seconds(60), true);

    // Output the message content.
    std::wcout << U("Changed message content: ") << changed_message.content_as_string() << std::endl;  

## <a name="how-to-de-queue-the-next-message"></a>So: Warteschlange die nächste Nachricht
Code Warteschlange eine Nachricht von einer Warteschlange in zwei Schritten. Wenn Sie **Get_message**aufrufen, erhalten Sie die nächste Nachricht in einer Warteschlange. Eine Meldung, die von **Get_message** zurückgegeben wird für alle anderen Code Lesen von Nachrichten aus dieser Warteschlange nicht sichtbar. Wenn Sie fertig sind, wobei die Nachricht aus der Warteschlange entfernt, müssen Sie auch **Delete_message**aufrufen. Diese zwei Verfahren zum Entfernen einer Nachricht wird sichergestellt, dass Code nicht verarbeiten einer Nachricht aufgrund von Fehlern Hardware oder Software, einer anderen Instanz des Codes kann dieselbe Nachricht erhalten und versuchen Sie es erneut. Code ruft **Delete_message** rechts, nachdem die Nachricht verarbeitet wurde.

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Get the next message.
    azure::storage::cloud_queue_message dequeued_message = queue.get_message();
    std::wcout << U("Dequeued message: ") << dequeued_message.content_as_string() << std::endl;

    // Delete the message.
    queue.delete_message(dequeued_message);

## <a name="how-to-leverage-additional-options-for-de-queuing-messages"></a>So: Nutzen Sie zusätzliche Optionen für Warteschlange Nachrichten
Es gibt zwei Möglichkeiten, die Sie aus einer Warteschlange Nachrichtenübermittlung anpassen können. Zunächst erhalten Sie eine Reihe von Nachrichten (bis zu 32). Zweites, können Sie einen Timeout verlängern oder verkürzen Invisibility festlegen, gleicht der Code mehr oder weniger jede Nachricht vollständig Verarbeitungszeit. Im folgenden Code wird verwendet die **Get_messages** Methode, um 20 Nachrichten in einem Anruf zu gelangen. Dann wird jede Nachricht mit einer Schleife **für** verarbeitet. Invisibility Timeout wird auch auf fünf Minuten für jede Nachricht festgelegt. Beachten Sie, dass das beginnt 5 Minuten für alle Nachrichten zur gleichen Zeit, nach 5 Minuten haben übergeben, da des Anrufs an die **Get_messages**, keine Nachrichten, die nicht gelöscht wurden erneut angezeigt werden soll.

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Dequeue some queue messages (maximum 32 at a time) and set their visibility timeout to
    // 5 minutes (300 seconds).
    azure::storage::queue_request_options options;
    azure::storage::operation_context context;

    // Retrieve 20 messages from the queue with a visibility timeout of 300 seconds.
    std::vector<azure::storage::cloud_queue_message> messages = queue.get_messages(20, std::chrono::seconds(300), options, context);

    for (auto it = messages.cbegin(); it != messages.cend(); ++it)
    {
        // Display the contents of the message.
        std::wcout << U("Get: ") << it->content_as_string() << std::endl;
    }

## <a name="how-to-get-the-queue-length"></a>So: Abrufen die Länge der Warteschlange
Sie können eine Schätzung der Anzahl von Nachrichten in einer Warteschlange abrufen. Die Methode **Download_attributes** gefragt werden, zum Abrufen der Warteschlange-Attributen, die Anzahl der Nachricht einschließlich den Warteschlange-Dienst. Die Methode **Approximate_message_count** Ruft die ungefähre Anzahl von Nachrichten in der Warteschlange an.

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Fetch the queue attributes.
    queue.download_attributes();

    // Retrieve the cached approximate message count.
    int cachedMessageCount = queue.approximate_message_count();

    // Display number of messages.
    std::wcout << U("Number of messages in queue: ") << cachedMessageCount << std::endl;  

## <a name="how-to-delete-a-queue"></a>So: löschen eine Warteschlange
Zum Löschen einer Warteschlange und alle darin enthaltenen Nachrichten rufen Sie die **Delete_queue_if_exists** -Methode der Warteschlange-Objekt aus.

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // If the queue exists and delete it.
    queue.delete_queue_if_exists();  

## <a name="next-steps"></a>Nächste Schritte

Die Grundlagen der Warteschlangenspeicher bearbeitet haben, führen Sie die folgenden Links, um weitere Informationen zur Azure-Speicher.

-   [So verwenden Sie BLOB-Speicher von C++](storage-c-plus-plus-how-to-use-blobs.md)
-   [Zum Verwenden von C++ Tabellenspeicher](storage-c-plus-plus-how-to-use-tables.md)
-   [Ressourcen in C++ Azure-Speicher](storage-c-plus-plus-enumeration.md)
-   [Client-Bibliothek zu Referenzzwecken C++ Speicher](http://azure.github.io/azure-storage-cpp)
-   [Dokumentation Azure-Speicher](https://azure.microsoft.com/documentation/services/storage/)


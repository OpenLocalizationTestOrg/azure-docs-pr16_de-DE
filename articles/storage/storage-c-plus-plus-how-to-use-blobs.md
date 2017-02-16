<properties
    pageTitle="So verwenden Sie BLOB-Speicher (Objektspeicher) von C++ | Microsoft Azure"
    description="Speichern von unstrukturierten Daten in der Cloud mit Azure Blob-Speicher (Objektspeicher)."
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

# <a name="how-to-use-blob-storage-from-c"></a>So verwenden Sie BLOB-Speicher von C++  

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>(Übersicht)

Azure Blob-Speicher ist ein Dienst, der unstrukturierte Daten als Objekte/Blobs in der Cloud gespeichert sind. BLOB-Speicher kann beliebige binäre Daten, beispielsweise ein Dokument, Media-Datei oder das Anwendungsinstallationsprogramm oder zu speichern. BLOB-Speicher wird auch als Objektspeicher bezeichnet.

Dieses Handbuch, wie Sie allgemeine Szenarien mithilfe des Diensts für Azure BLOB-Speicher ausführen.. Die Beispiele in C++ geschrieben sind und [Azure-Speicher-Client-Bibliothek für C++](http://github.com/Azure/azure-storage-cpp/blob/master/README.md)verwenden. Die Szenarios dieser einschließen **Hochladen**, **Auflisten**, **herunterladen**und **Löschen von** Blobs.  

>[AZURE.NOTE] Dieses Handbuch richtet sich an den Azure-Speicher Client-Bibliothek für C++ Version 1.0.0 und über. Die empfohlene Version ist Speicher Clientbibliothek 2.2.0, die über [NuGet](http://www.nuget.org/packages/wastorage) oder [GitHub](https://github.com/Azure/azure-storage-cpp)verfügbar ist.

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]
[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a>Erstellen einer C++-Anwendungs
In diesem Handbuch Verwenden Sie Speicher Features der in einer C++-Anwendung ausgeführt werden können.  

Dazu müssen Sie die Bibliothek Azure-Speicher Client für C++ installieren und erstellen Sie ein Azure-Speicher-Konto in Ihrem Azure-Abonnement.   

Um die Azure-Speicher-Client-Bibliothek für C++ installiert haben, können Sie die folgenden Methoden verwenden:

-   **Linux:** Anweisungen Sie in der [Azure Speicher Client für C++ Infos](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) Bibliotheksseite angegeben.  
-   **Windows:** Klicken Sie in Visual Studio auf **Tools > NuGet-Paket-Manager > Paket-Manager-Konsole**. Geben Sie den folgenden Befehl in die [NuGet-Paket-Manager-Konsole](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) ein, und drücken Sie die **EINGABETASTE**.  

        Install-Package wastorage

## <a name="configure-your-application-to-access-blob-storage"></a>Konfigurieren der Anwendungs Zugriff auf Blob-Speicher  
Fügen Sie, dass die folgenden Anweisungen an den Anfang der C++-Datei einfügen, in der Azure-Speicher-APIs zu verwenden, um Blobs zuzugreifen werden soll:  

    #include "was/storage_account.h"
    #include "was/blob.h"

## <a name="setup-an-azure-storage-connection-string"></a>Einrichten einer Verbindungszeichenfolge Azure-Speicher
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

Als Nächstes rufen Sie einen Verweis auf eine **Cloud_blob_client** Klasse wie es Ihnen ermöglicht, Objekte abzurufen, die Container und Blobs innerhalb der Blob-Speicherdienst gespeicherte darstellen. Der folgende Code erstellt ein **Cloud_blob_client** Objekt mithilfe des Objekts der Speicher-Konto, dem wir über abgerufen:  

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();  

## <a name="how-to-create-a-container"></a>So: erstellen ein Containers

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

Dieses Beispiel zeigt, wie Sie einen Container erstellen, wenn es nicht bereits vorhanden ist:  

    try
    {
        // Retrieve storage account from connection string.
        azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

        // Create the blob client.
        azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

        // Retrieve a reference to a container.
        azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

        // Create the container if it doesn't already exist.
        container.create_if_not_exists();
    }
    catch (const std::exception& e)
    {
        std::wcout << U("Error: ") << e.what() << std::endl;
    }  

Standardmäßig der neue Container ist privat, und geben Sie Ihre Speicher Zugriffstaste zum Herunterladen von Blobs aus diesem Container. Wenn Sie die Dateien (Blobs) innerhalb des Containers für alle Benutzer zur Verfügung stellen möchten, können Sie den Container mit dem folgenden Code öffentlich sein festlegen:  

    // Make the blob container publicly accessible.
    azure::storage::blob_container_permissions permissions;
    permissions.set_public_access(azure::storage::blob_container_public_access_type::blob);
    container.upload_permissions(permissions);  

Jeder im Internet kann in einem öffentlichen Container Blobs sehen, jedoch können Sie ändern oder löschen sie nur, wenn Sie über die entsprechenden Zugriffstaste verfügen.  

## <a name="how-to-upload-a-blob-into-a-container"></a>Wie: Hochladen einen Blob in einen Container
Azure Blob-Speicher unterstützt blockieren Blobs und Seitenblobs. In den meisten Fällen ist blockieren Blob die empfohlenen ein verwenden.  

Klicken Sie zum Hochladen einer Datei auf einen Blockblob Abrufen eines Containers Verweises, und verwenden sie einen Block Blob Verweis abrufen. Nachdem Sie einen Bezug Blob haben, können Sie alle Stream von Daten zu hochladen, durch die Methode **Upload_from_stream** aufrufen. Dieser Vorgang erstellt das Blob aus, wenn er nicht zuvor vorhanden oder überschreiben Sie ihn an, wenn er vorhanden ist. Im folgenden Beispiel wird gezeigt, wie einen Blob in einen Container hochladen und wird davon ausgegangen, dass der Container bereits erstellt wurde.  

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a previously created container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Retrieve reference to a blob named "my-blob-1".
    azure::storage::cloud_block_blob blockBlob = container.get_block_blob_reference(U("my-blob-1"));

    // Create or overwrite the "my-blob-1" blob with contents from a local file.
    concurrency::streams::istream input_stream = concurrency::streams::file_stream<uint8_t>::open_istream(U("DataFile.txt")).get();
    blockBlob.upload_from_stream(input_stream);
    input_stream.close().wait();

    // Create or overwrite the "my-blob-2" and "my-blob-3" blobs with contents from text.
    // Retrieve a reference to a blob named "my-blob-2".
    azure::storage::cloud_block_blob blob2 = container.get_block_blob_reference(U("my-blob-2"));
    blob2.upload_text(U("more text"));

    // Retrieve a reference to a blob named "my-blob-3".
    azure::storage::cloud_block_blob blob3 = container.get_block_blob_reference(U("my-directory/my-sub-directory/my-blob-3"));
    blob3.upload_text(U("other text"));  

Alternativ können Sie die **Upload_from_file** Methode zum Hochladen einer Datei in ein Blob blockieren aus.

## <a name="how-to-list-the-blobs-in-a-container"></a>So: Liste der Blobs in einem Container
Rufen Sie zunächst einen Containerverweis, um die Liste der Blobs in einem Container. Des Containers **List_blobs** Methode können dann die Blobs und/oder Verzeichnisse darin abzurufen. Um die Rich-Satz von Eigenschaften und Methoden für eine zurückgegebene **List_blob_item**zugreifen zu können, müssen Sie die **list_blob_item.as_blob** Methode zum Abrufen eines Objekts **Cloud_blob** oder die **list_blob.as_directory** Methode zum Abrufen eines Objekts Cloud_blob_directory aufrufen. Im folgende Code veranschaulicht, wie abrufen und den URI des jedes Element im Container **Meine Stichprobe Container** ausgeben:

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a previously created container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Output URI of each item.
    azure::storage::list_blob_item_iterator end_of_results;
    for (auto it = container.list_blobs(); it != end_of_results; ++it)
    {
        if (it->is_blob())
        {
            std::wcout << U("Blob: ") << it->as_blob().uri().primary_uri().to_string() << std::endl;
        }
        else
        {
            std::wcout << U("Directory: ") << it->as_directory().uri().primary_uri().to_string() << std::endl;
        }
    }

Weitere Details zum Auflisten von Vorgängen finden Sie in der [Liste Azure Speicher-Ressourcen in C++](storage-c-plus-plus-enumeration.md).

## <a name="how-to-download-blobs"></a>So: Blobs herunterladen
Klicken Sie zum Herunterladen Blobs zuerst rufen Sie einen Blob-Verweis ab, und rufen Sie die Methode **Download_to_stream** . Im folgende Beispiel verwendet die **Download_to_stream** Methode, um den Blob-Inhalt in einem Streamobjekt zu übertragen, die Sie dann in eine lokale Datei beibehalten werden können.  

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a previously created container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Retrieve reference to a blob named "my-blob-1".
    azure::storage::cloud_block_blob blockBlob = container.get_block_blob_reference(U("my-blob-1"));

    // Save blob contents to a file.
    concurrency::streams::container_buffer<std::vector<uint8_t>> buffer;
    concurrency::streams::ostream output_stream(buffer);
    blockBlob.download_to_stream(output_stream);

    std::ofstream outfile("DownloadBlobFile.txt", std::ofstream::binary);
    std::vector<unsigned char>& data = buffer.collection();

    outfile.write((char *)&data[0], buffer.size());
    outfile.close();  

Alternativ können Sie die **Download_to_file** Methode zum Herunterladen des Inhalts einer Blob in eine Datei aus.
Darüber hinaus können Sie auch die **Download_text** Methode zum Herunterladen des Inhalts einer Blob als Zeichenfolge verwenden.  

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a previously created container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Retrieve reference to a blob named "my-blob-2".
    azure::storage::cloud_block_blob text_blob = container.get_block_blob_reference(U("my-blob-2"));

    // Download the contents of a blog as a text string.
    utility::string_t text = text_blob.download_text();

## <a name="how-to-delete-blobs"></a>So: Blobs löschen
Zum Löschen eines BLOBs zuerst einen Verweis Blob und rufen Sie dann die Methode **Delete_blob** daran.  

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a previously created container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Retrieve reference to a blob named "my-blob-1".
    azure::storage::cloud_block_blob blockBlob = container.get_block_blob_reference(U("my-blob-1"));

    // Delete the blob.
    blockBlob.delete_blob();

## <a name="next-steps"></a>Nächste Schritte
Die Grundlagen des Blob-Speicher bearbeitet haben, führen Sie die folgenden Links, um weitere Informationen zur Azure-Speicher.  

-   [So Warteschlange-Speicher von C++ verwenden](storage-c-plus-plus-how-to-use-queues.md)
-   [Zum Verwenden von C++ Tabellenspeicher](storage-c-plus-plus-how-to-use-tables.md)
-   [Ressourcen in C++ Azure-Speicher](storage-c-plus-plus-enumeration.md)
-   [Client-Bibliothek zu Referenzzwecken C++ Speicher](http://azure.github.io/azure-storage-cpp)
-   [Dokumentation Azure-Speicher](https://azure.microsoft.com/documentation/services/storage/)
- [Übertragen von Daten mit dem Befehlszeilendienstprogramm AzCopy](storage-use-azcopy.md)

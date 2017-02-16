<properties
    pageTitle="Zum Verwenden von C++ Dateispeicher | Microsoft Azure"
    description="Speichern Sie Dateidaten in der Cloud mit Speicher Azure-Datei."
    services="storage"
    documentationCenter=".net"
    authors="seguler"
    manager="jahogg"
    editor="tysonn" />

<tags ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="seguler" />

# <a name="how-to-use-file-storage-from-c"></a>Zum Verwenden von C++ Dateispeicher

[AZURE.INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-files](../../includes/storage-try-azure-tools-files.md)]
[AZURE.INCLUDE [storage-file-overview-include](../../includes/storage-file-overview-include.md)]

## <a name="about-this-tutorial"></a>Informationen zu diesem Lernprogramm

In diesem Lernprogramm erfahren Sie, wie Sie grundlegende Vorgänge in der Datei mit Microsoft Azure-Speicherdienst ausführen. Bis-Beispiele in C++ erfahren Sie, wie Sie Freigaben und Verzeichnisse erstellen, hochladen, Liste, und Löschen von Dateien. Wenn Sie mit Microsoft Azure des Datei-Speicherdienst vertraut sind, werden bis die Konzepte in den folgenden Abschnitten vertraut sehr hilfreich, Grundlegendes zu den Beispielen.

[AZURE.INCLUDE [storage-file-concepts-include](../../includes/storage-file-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a>Erstellen einer C++-Anwendungs

Um die Beispiele zu erstellen, müssen Sie die Azure-Speicher Client-Bibliothek 2.4.0 für C++ zu installieren. Sie sollten auch ein Konto Azure-Speicher erstellt haben.

Um den Azure-Speicher-Client 2.4.0 für C++ installiert haben, können Sie eine der folgenden Methoden verwenden:

-   **Linux:** Anweisungen Sie in der [Azure Speicher Client für C++ Infos](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) Bibliotheksseite angegeben.

-   **Windows:** Klicken Sie in Visual Studio auf **Tools &gt; NuGet Package Manager &gt; Paket-Manager-Konsole**. Geben Sie den folgenden Befehl in die [NuGet-Paket-Manager-Konsole](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) ein, und drücken Sie die **EINGABETASTE**.

    Installieren-Paket wastorage

## <a name="set-up-your-application-to-use-file-storage"></a>Richten Sie Ihrer Anwendung Dateispeicher verwenden ein

Fügen Sie die folgenden Anweisungen an den Anfang der Datei C++ enthalten, in der Azure-Speicher-APIs Zugriff auf Dateien verwenden werden soll:

    #include "was/storage_account.h"
    #include "was/file.h"

## <a name="set-up-an-azure-storage-connection-string"></a>Einrichten einer Verbindungszeichenfolge Azure-Speicher

Zum Speichern von Daten verwenden möchten, müssen Sie eine Verbindung mit Ihrem Konto Azure-Speicher. Dieser erste Schritt wäre zum Konfigurieren einer Verbindungszeichenfolge die Verbindung zu Ihrem Speicherkonto verwendet wird. Definieren Sie uns eine statische Variable zum erledigen.

    // Define the connection-string with your values.
    const utility::string_t 
    storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));

## <a name="connecting-to-an-azure-storage-account"></a>Herstellen einer Verbindung mit einer Firma Azure-Speicher

**Cloud_storage_account** -Klasse können Ihre Speicher Kontoinformationen darstellen. Wenn Ihre Kontoinformationen Speicher aus der Verbindungszeichenfolge Speicher abrufen möchten, können Sie die Methode **Analysieren** .

    // Retrieve storage account from connection string. 
    azure::storage::cloud_storage_account storage_account = 
      azure::storage::cloud_storage_account::parse(storage_connection_string);

## <a name="how-to-create-a-share"></a>So: Erstellen Sie eine Freigabe

Alle Dateien und Verzeichnisse in Dateispeicher befinden sich in einem Container bezeichnet eine **Freigeben**. Ihr Speicherkonto kann beliebig viele Freigaben wie Ihr Konto Kapazität ermöglicht haben. Um Zugriff auf eine Freigabe und deren Inhalt zu erhalten, müssen Sie einen Datei Speicher-Client verwendet werden.

    // Create the file storage client.
    azure::storage::cloud_file_client file_client = 
      storage_account.create_cloud_file_client();

Verwenden den Datei-Speicher-Client, können Sie dann einen Verweis auf eine Freigabe abrufen.

    // Get a reference to the file share
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));

Verwenden Sie die **Create_if_not_exists** -Methode des Objekts **Cloud_file_share** , um die Freigabe zu erstellen.

    if (share.create_if_not_exists()) { 
        std::wcout << U("New share created") << std::endl;  
    }

An diesem Punkt enthält das **Freigeben** einen Verweis auf eine Freigabe namens **Meine Stichprobe freigeben**.

## <a name="how-to-upload-a-file"></a>Wie: Hochladen einer Datei

Zumindest enthält eine Azure-Speicher Dateifreigabe ein Stammverzeichnis, wo Dateien gespeichert werden können. In diesem Abschnitt erfahren Sie, wie zum Hochladen einer Datei aus dem lokalen Speicher auf einer Freigabe des Stammverzeichnisses.

Der erste Schritt beim Hochladen einer Datei ist in einen Verweis Verzeichnis abzurufen, in denen es befinden soll. Dazu Aufrufen der **Get_root_directory_reference** -Methode des Objekts freigeben.

    //Get a reference to the root directory for the share.
    azure::storage::cloud_file_directory root_dir = share.get_root_directory_reference();

Jetzt, da Sie einen Bezug auf das Stammverzeichnis der Freigabe verfügen, können Sie eine Datei auf diesem Hochladen. In diesem Beispiel uploads aus einer Datei, Text und aus einem Stream.

    // Upload a file from a stream.
    concurrency::streams::istream input_stream = 
      concurrency::streams::file_stream<uint8_t>::open_istream(_XPLATSTR("DataFile.txt")).get();

    azure::storage::cloud_file file1 = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-1"));
    file1.upload_from_stream(input_stream);

    // Upload some files from text.
    azure::storage::cloud_file file2 = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-2"));
    file2.upload_text(_XPLATSTR("more text"));

    // Upload a file from a file.
    azure::storage::cloud_file file4 = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-3"));
    file4.upload_from_file(_XPLATSTR("DataFile.txt"));  

## <a name="how-to-create-a-directory"></a>So: ein Verzeichnis erstellen

Sie können auch Speicher organisieren, indem Sie Dateien in Unterverzeichnisse anstatt aller Folien im Stammverzeichnis. Der Speicherdienst Azure-Datei können Sie Verzeichnisse erstellen, wie viel wie Ihr Konto werden kann. Im folgenden Code wird ein Verzeichnis **Verzeichnis Meine-Beispiel** unter Stammverzeichnis als auch ein Unterverzeichnis mit dem Namen **Meine Stichprobe Unterverzeichnis**erstellen.
    
    // Retrieve a reference to a directory
    azure::storage::cloud_file_directory directory = share.get_directory_reference(_XPLATSTR("my-sample-directory"));
    
    // Return value is true if the share did not exist and was successfully created.
    directory.create_if_not_exists();
    
    // Create a subdirectory.
    azure::storage::cloud_file_directory subdirectory = 
      directory.get_subdirectory_reference(_XPLATSTR("my-sample-subdirectory"));
    subdirectory.create_if_not_exists();

## <a name="how-to-list-files-and-directories-in-a-share"></a>So: Liste von Dateien und Verzeichnissen in eine Freigabe

Abrufen einer Liste von Dateien und Verzeichnissen innerhalb einer Freigabe erfolgt mühelos durch **List_files_and_directories** für einen Verweis **Cloud_file_directory** aufrufen. Um die Rich-Satz von Eigenschaften und Methoden für eine zurückgegebene **List_file_and_directory_item**zugreifen zu können, müssen Sie die **list_file_and_directory_item.as_file** Methode zum Abrufen eines Objekts **Cloud_file** oder die **list_file_and_directory_item.as_directory** Methode zum Abrufen eines Objekts **Cloud_file_directory** aufrufen.

Im folgende Code veranschaulicht, wie abrufen und ausgeben den URI jedes Elements im Stammverzeichnis der Freigabe.

    //Get a reference to the root directory for the share.
    azure::storage::cloud_file_directory root_dir = 
      share.get_root_directory_reference();
    
    // Output URI of each item.
    azure::storage::list_file_and_diretory_result_iterator end_of_results;
    
    for (auto it = directory.list_files_and_directories(); it != end_of_results; ++it)
    {
        if(it->is_directory())
        {
            ucout << "Directory: " << it->as_directory().uri().primary_uri().to_string() << std::endl;
        }
        else if (it->is_file())
        {
            ucout << "File: " << it->as_file().uri().primary_uri().to_string() << std::endl;
        }       
    }
    

## <a name="how-to-download-a-file"></a>Wie: Herunterladen eine Datei

Laden Sie Dateien herunter, rufen Sie zuerst einen Dateiverweis, und rufen Sie die **Download_to_stream** -Methode, um den Inhalt der Datei zu einem Streamobjekt übertragen, die dann in eine lokale Datei beibehalten werden. Alternativ können Sie die **Download_to_file** Methode zum Herunterladen des Inhalts einer Datei in einer lokalen Datei aus. **Download_text** -Methode können den Inhalt einer Datei als Zeichenfolge herunterladen.

Im folgende Beispiel verwendet die Methoden **Download_to_stream** und **Download_text** um zu veranschaulichen Download der Dateien, die in den vorherigen Abschnitten erstellt wurden.

    // Download as text
    azure::storage::cloud_file text_file = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-2"));
    utility::string_t text = text_file.download_text();
    ucout << "File Text: " << text << std::endl;
    
    // Download as a stream.
    azure::storage::cloud_file stream_file = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-3"));
    
    concurrency::streams::container_buffer<std::vector<uint8_t>> buffer;
    concurrency::streams::ostream output_stream(buffer);
    stream_file.download_to_stream(output_stream);
    std::ofstream outfile("DownloadFile.txt", std::ofstream::binary);
    std::vector<unsigned char>& data = buffer.collection();
    outfile.write((char *)&data[0], buffer.size());
    outfile.close();

## <a name="how-to-delete-a-file"></a>So: Löschen einer Datei

Eine andere allgemeine Datei Speichervorgang ist Datei löschen. Der folgende Code Löscht eine Datei mit dem Namen Meine-Beispiel-Datei-3 unter dem Stammverzeichnis gespeichert.

    // Get a reference to the root directory for the share. 
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));
    
    azure::storage::cloud_file_directory root_dir = 
      share.get_root_directory_reference();
    
    azure::storage::cloud_file file = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-3"));
    
    file.delete_file_if_exists();

## <a name="how-to-delete-a-directory"></a>So: ein Verzeichnis löschen

Löschen eines Verzeichnisses ist ein einfacher Vorgang, auch wenn darauf hinzuweisen, sollten Sie ein Verzeichnis, die Dateien immer noch enthält oder anderen Verzeichnissen können nicht gelöscht.
    
    // Get a reference to the share.
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));
    
    // Get a reference to the directory.
    azure::storage::cloud_file_directory directory = 
      share.get_directory_reference(_XPLATSTR("my-sample-directory"));
    
    // Get a reference to the subdirectory you want to delete.
    azure::storage::cloud_file_directory sub_directory =
      directory.get_subdirectory_reference(_XPLATSTR("my-sample-subdirectory"));
    
    // Delete the subdirectory and the sample directory.
    sub_directory.delete_directory_if_exists();
    
    directory.delete_directory_if_exists();

## <a name="how-to-delete-a-share"></a>So: löschen eine Freigabe

Löschen eine Freigabe erfolgt durch die **Delete_if_exists** -Methode für ein Objekt Cloud_file_share aufrufen. So sieht, die das Beispielcode aus.
    
    // Get a reference to the share.
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));
    
    // delete the share if exists
    share.delete_share_if_exists();

## <a name="set-the-maximum-size-for-a-file-share"></a>Legen Sie die maximale Größe für eine Dateifreigabe

Sie können das Kontingent (oder die maximale Größe) für eine Dateifreigabe, in Gigabyte festlegen. Sie können auch überprüfen, um anzuzeigen, wie viele Daten aktuell für die Freigabe gespeichert ist.

Das Kontingent für eine Freigabe festgelegt ist, können Sie die Gesamtgröße der Dateien, die für die Freigabe gespeichert werden. Überschreitet die Gesamtgröße der Dateien in der Freigabe das Kontingent für die Freigabe festlegen, klicken Sie dann werden Clients nicht erhöhen Sie die Größe der vorhandenen Dateien oder Erstellen neuer Dateien, es sei denn, diese Dateien leer sind.

Das folgende Beispiel zeigt, wie die aktuelle Verwendung für eine Freigabe überprüfen und wie Sie das Kontingent für die Freigabe festlegen.

    // Parse the connection string for the storage account.
    azure::storage::cloud_storage_account storage_account = 
      azure::storage::cloud_storage_account::parse(storage_connection_string);
    
    // Create the file client.
    azure::storage::cloud_file_client file_client = 
      storage_account.create_cloud_file_client();
    
    // Get a reference to the share.
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));
    if (share.exists())
    {
        std::cout << "Current share usage: " << share.download_share_usage() << "/" << share.properties().quota();
    
        //This line sets the quota to 2560GB
        share.resize(2560);
    
        std::cout << "Quota increased: " << share.download_share_usage() << "/" << share.properties().quota();
    
    }

## <a name="generate-a-shared-access-signature-for-a-file-or-file-share"></a>Generieren einer freigegebenen Access-Signatur für eine Datei oder eine Dateifreigabe

Sie können eine freigegebene Access Signatur (SAS) für eine Dateifreigabe oder für eine bestimmte Datei generieren. Sie können auch eine freigegebenen Zugriffsrichtlinie auf einer Dateifreigabe zum Verwalten von freigegebenen Access Signaturen erstellen. Erstellen einer freigegebenen Zugriffsrichtlinie wird empfohlen, wie es bietet eine Möglichkeit, die SAS widerrufen, wenn es beeinträchtigt werden sollte.

Im folgende Beispiel eine freigegebenen Zugriffsrichtlinie auf einer Freigabe erstellt, und anschließend die Richtlinie verwendet, um die Einschränkungen für eine SAS in einer Datei in die Freigabe bereitstellen.

    // Parse the connection string for the storage account.
    azure::storage::cloud_storage_account storage_account = 
      azure::storage::cloud_storage_account::parse(storage_connection_string);
    
    // Create the file client and get a reference to the share
    azure::storage::cloud_file_client file_client = 
      storage_account.create_cloud_file_client();
    
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));
    
    if (share.exists())
    {
        // Create and assign a policy
        utility::string_t policy_name = _XPLATSTR("sampleSharePolicy");

        azure::storage::file_shared_access_policy sharedPolicy = 
          azure::storage::file_shared_access_policy();

        //set permissions to expire in 90 minutes
        sharedPolicy.set_expiry(utility::datetime::utc_now() + 
          utility::datetime::from_minutes(90));

        //give read and write permissions
        sharedPolicy.set_permissions(azure::storage::file_shared_access_policy::permissions::write | azure::storage::file_shared_access_policy::permissions::read);

        //set permissions for the share
        azure::storage::file_share_permissions permissions; 

        //retrieve the current list of shared access policies
        azure::storage::shared_access_policies<azure::storage::file_shared_access_policy> policies;
        
        //add the new shared policy
        policies.insert(std::make_pair(policy_name, sharedPolicy));

        //save the updated policy list
        permissions.set_policies(policies);
        share.upload_permissions(permissions);

        //Retrieve the root directory and file references
        azure::storage::cloud_file_directory root_dir = 
          share.get_root_directory_reference();
        azure::storage::cloud_file file = 
          root_dir.get_file_reference(_XPLATSTR("my-sample-file-1"));

        // Generate a SAS for a file in the share 
        //  and associate this access policy with it.       
        utility::string_t sas_token = file.get_shared_access_signature(sharedPolicy);
        
        // Create a new CloudFile object from the SAS, and write some text to the file.     
        azure::storage::cloud_file file_with_sas(azure::storage::storage_credentials(sas_token).transform_uri(file.uri().primary_uri()));
        utility::string_t text = _XPLATSTR("My sample content");        
        file_with_sas.upload_text(text);        
        
        //Download and print URL with SAS.
        utility::string_t downloaded_text = file_with_sas.download_text();      
        ucout << downloaded_text << std::endl;      
        ucout << azure::storage::storage_credentials(sas_token).transform_uri(file.uri().primary_uri()).to_string() << std::endl;
    
    }

Weitere Informationen zum Erstellen und Verwenden von freigegebenen Access Signaturen finden Sie unter [Verwenden von freigegebenen Access Signaturen (SAS)](storage-dotnet-shared-access-signature-part-1.md).

## <a name="next-steps"></a>Nächste Schritte

Untersuchen Sie diese Ressourcen, um weitere Informationen zur Azure-Speicher:

-   [Speicher-Client-Bibliothek für C++](https://github.com/Azure/azure-storage-cpp)

-   [Azure-Speicher-Explorer](http://go.microsoft.com/fwlink/?LinkID=822673&clcid=0x409)

-   [Dokumentation Azure-Speicher](https://azure.microsoft.com/documentation/services/storage/)

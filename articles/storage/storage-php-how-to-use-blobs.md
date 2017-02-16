<properties
    pageTitle="So verwenden Sie BLOB-Speicher (Objektspeicher) von PHP | Microsoft Azure"
    description="Speichern von unstrukturierten Daten in der Cloud mit Azure Blob-Speicher (Objektspeicher)."
    documentationCenter="php"
    services="storage"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="how-to-use-blob-storage-from-php"></a>So verwenden Sie BLOB-Speicher von PHP

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>(Übersicht)

Azure Blob-Speicher ist ein Dienst, der unstrukturierte Daten als Objekte/Blobs in der Cloud gespeichert sind. BLOB-Speicher kann beliebige binäre Daten, beispielsweise ein Dokument, Media-Datei oder das Anwendungsinstallationsprogramm oder zu speichern. BLOB-Speicher wird auch als Objektspeicher bezeichnet.

In diesem Handbuch wird gezeigt, wie häufige Szenarien mithilfe des Azure Blob-Diensts durchführen. Die Beispiele in PHP geschrieben sind, und Verwenden des [Azure SDK für PHP] [download]. Die Szenarios dieser einschließen **Hochladen**, **Auflisten**, **herunterladen**und **Löschen von** Blobs. Weitere Informationen Blobs finden Sie im Abschnitt für die [nächsten Schritte](#next-steps) .

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a>Erstellen Sie eine Anwendung von PHP

Die einzige Anforderung zum Erstellen einer PHP-Anwendung, die auf den Dienst Azure Blob zugegriffen wird das Erstellen von Verweisen auf Klassen im Azure SDK für von PHP innerhalb des Codes. Alle Development Tools können zum Erstellen einer Anwendung, einschließlich Editor.

In diesem Handbuch Verwenden Sie Service-Features, die innerhalb einer Anwendung von PHP lokal oder innerhalb einer Webrolle Azure, Worker-Rolle oder Website ausgeführten Code aufgerufen werden können.

## <a name="get-the-azure-client-libraries"></a>Abrufen der Azure Clientbibliotheken

[AZURE.INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-access-the-blob-service"></a>Konfigurieren der Anwendungs Zugriff auf die Blob-Dienst

Um die Azure Blob-Service-APIs verwenden zu können, müssen Sie:

1. Verweisen auf die Autoloader-Datei mit der Anweisung [Require_once] und
2. Verweisen auf eine beliebige Klassen, die Sie eventuell verwenden.

Im folgenden Beispiel wird so fügen Sie die Datei Autoloader und die Klasse **ServicesBuilder** verweisen.

> [AZURE.NOTE] In diesem Beispiel (und andere Beispiele in diesem Artikel) wird davon ausgegangen, dass Sie die PHP-Client-Bibliotheken für Azure über Composer installiert haben. Wenn Sie die Bibliotheken manuell installiert haben, müssen Sie Bezug auf die `WindowsAzure.php` Autoloader-Datei.

    require_once 'vendor/autoload.php';
    use WindowsAzure\Common\ServicesBuilder;


In den folgenden Beispielen der `require_once` Anweisung wird immer angezeigt, aber nur die notwendigen Klassen für das Beispiel auszuführende verwiesen werden.

## <a name="set-up-an-azure-storage-connection"></a>Einrichten einer Verbindung Azure-Speicher

Um eine Azure Blob-Service-Client instanziieren, müssen Sie zuerst eine gültige Verbindungszeichenfolge verfügen. Das Format für die Verbindungszeichenfolge der Blob-Dienst lautet:

Für den Zugriff auf einen live-Dienst:

    DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]

Für den Zugriff auf Speicheremulator:

    UseDevelopmentStorage=true


Zum Erstellen einer beliebigen Azure Service-Client, müssen Sie die Klasse **ServicesBuilder** verwenden. Sie können:

* Die Verbindungszeichenfolge direkt an übergeben oder
* Verwenden Sie **CloudConfigurationManager (CCM)** , um mehrere externen Quellen für die Verbindungszeichenfolge aktivieren:
    * Standardmäßig bietet es Unterstützung für eine externe Quelle - Umgebung Variablen.
    * Sie können neue Quellen hinzufügen, indem Sie die **ConnectionStringSource** Klasse erweitern.

Für die hier beschriebenen Beispielen wird die Verbindungszeichenfolge direkt übergeben.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;

    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);

## <a name="create-a-container"></a>Erstellen eines Containers

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

Ein Objekt **BlobRestProxy** ermöglicht die Erstellung ein Containers Blob mit der Methode **CreateContainer** . Wenn Sie einen Container erstellen, können die Optionen für den Container fest, aber dies ist nicht erforderlich. (Im folgenden Beispiel wird veranschaulicht, wie die Container Zugriffssteuerungsliste () und Container Metadaten festgelegt.)

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Blob\Models\CreateContainerOptions;
    use MicrosoftAzure\Storage\Blob\Models\PublicAccessType;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    // OPTIONAL: Set public access policy and metadata.
    // Create container options object.
    $createContainerOptions = new CreateContainerOptions();

    // Set public access policy. Possible values are
    // PublicAccessType::CONTAINER_AND_BLOBS and PublicAccessType::BLOBS_ONLY.
    // CONTAINER_AND_BLOBS:
    // Specifies full public read access for container and blob data.
    // proxys can enumerate blobs within the container via anonymous
    // request, but cannot enumerate containers within the storage account.
    //
    // BLOBS_ONLY:
    // Specifies public read access for blobs. Blob data within this
    // container can be read via anonymous request, but container data is not
    // available. proxys cannot enumerate blobs within the container via
    // anonymous request.
    // If this value is not specified in the request, container data is
    // private to the account owner.
    $createContainerOptions->setPublicAccess(PublicAccessType::CONTAINER_AND_BLOBS);

    // Set container metadata.
    $createContainerOptions->addMetaData("key1", "value1");
    $createContainerOptions->addMetaData("key2", "value2");

    try {
        // Create container.
        $blobRestProxy->createContainer("mycontainer", $createContainerOptions);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

Aufrufen von **SetPublicAccess (PublicAccessType::CONTAINER\_und\_BLOBS)** macht die Container und Blob-Daten über anonyme Anfragen zugänglich. Aufrufen von **setPublicAccess(PublicAccessType::BLOBS_ONLY)** macht nur BLOB-Daten über anonyme Anfragen zugänglich. Weitere Informationen zu den Container ACLs, finden Sie unter [festlegen Container ACL (REST-API)][container-acl].

Weitere Informationen zu Fehlercodes für Blob-Dienst finden Sie unter [Blob-Dienst Fehlercodes][error-codes].

## <a name="upload-a-blob-into-a-container"></a>Hochladen eines BLOBs in einen container

Verwenden Sie zum Hochladen einer Datei als Blob die Methode **BlobRestProxy-CreateBlockBlob >** ein. Dieser Vorgang erstellt das Blob, wenn es nicht vorhanden ist oder wenn bedeutet überschrieben. Codebeispiel unten wird davon ausgegangen, dass der Container bereits erstellt wurde und [Fopen verwendet] [ fopen] zum Öffnen der Datei als Stream.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    $content = fopen("c:\myfile.txt", "r");
    $blob_name = "myblob";

    try {
        //Upload blob
        $blobRestProxy->createBlockBlob("mycontainer", $blob_name, $content);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

Beachten Sie, dass im vorherige Beispiel einen Blob als Stream hochlädt. Jedoch ein Blob kann auch hochgeladen werden als eine Zeichenfolge verwenden, beispielsweise die [Datei\_abrufen\_Inhalt] [ file_get_contents] (Funktion). Ändern Sie hierzu mit dem vorherigen Beispiel `$content = fopen("c:\myfile.txt", "r");` auf `$content = file_get_contents("c:\myfile.txt");`.

## <a name="list-the-blobs-in-a-container"></a>Liste der Blobs in einem container

Verwenden Sie die **BlobRestProxy -> ListBlobs** Methode mit **Foreach** Schleife über das Ergebnis, um die Liste der Blobs in einem Container. Im folgende Code zeigt den Namen der einzelnen Blob in der Ausgabe in einem Container und deren URI im Browser angezeigt.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    try {
        // List blobs.
        $blob_list = $blobRestProxy->listBlobs("mycontainer");
        $blobs = $blob_list->getBlobs();

        foreach($blobs as $blob)
        {
            echo $blob->getName().": ".$blob->getUrl()."<br />";
        }
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }


## <a name="download-a-blob"></a>Herunterladen eines BLOBs

Zum Herunterladen eines BLOBs **BlobRestProxy -> GetBlob** -Methode und anschließend Aufrufen der Methode **GetContentStream** auf das sich daraus ergebende **GetBlobResult** Objekt.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    try {
        // Get blob.
        $blob = $blobRestProxy->getBlob("mycontainer", "myblob");
        fpassthru($blob->getContentStream());
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

Beachten Sie, dass im oben genannten Beispiel einen Blob als Streamressource (das Standardverhalten) erhält. Sie können jedoch die [Stream\_erhalten\_Inhalt] [ stream-get-contents] Funktion, um die zurückgegebene Stream in eine Zeichenfolge konvertieren.

## <a name="delete-a-blob"></a>Löschen eines BLOBs

Zum Löschen eines BLOBs übergeben Sie den Container mit dem Namen und den Namen der Blob an **BlobRestProxy -> DeleteBlob**ein.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    try {
        // Delete blob.
        $blobRestProxy->deleteBlob("mycontainer", "myblob");
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

## <a name="delete-a-blob-container"></a>Löschen eines Containers blob

Übergeben Sie **BlobRestProxy -> DeleteContainer**schließlich zum Löschen eines Containers Blob den Containername.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    try {
        // Delete container.
        $blobRestProxy->deleteContainer("mycontainer");
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

## <a name="next-steps"></a>Nächste Schritte

Die Grundlagen des Diensts Azure Blob bearbeitet haben, folgen Sie diesen Links komplexere Speicheraufgaben Lernen aus.

- Besuchen Sie den [Teamblog Azure-Speicher](http://blogs.msdn.com/b/windowsazurestorage/)
- Finden Sie im [Beispiel für Blob von PHP blockieren](https://github.com/WindowsAzure/azure-sdk-for-php-samples/blob/master/storage/BlockBlobExample.php)aus.
- Siehe [Beispiel von PHP Seite Blob](https://github.com/WindowsAzure/azure-sdk-for-php-samples/blob/master/storage/PageBlobExample.php).
- [Übertragen von Daten mit den AzCopy Befehlszeilenprogramms](storage-use-azcopy.md)
 
Weitere Informationen finden Sie auch im [Developer Center von PHP](/develop/php/).


[download]: http://go.microsoft.com/fwlink/?LinkID=252473
[container-acl]: http://msdn.microsoft.com/library/azure/dd179391.aspx
[error-codes]: http://msdn.microsoft.com/library/azure/dd179439.aspx
[file_get_contents]: http://php.net/file_get_contents
[require_once]: http://php.net/require_once
[fopen]: http://www.php.net/fopen
[stream-get-contents]: http://www.php.net/stream_get_contents

<properties
    pageTitle="So verwenden Sie Azure BLOB-Speicher (Objektspeicher) von Java | Microsoft Azure"
    description="Speichern von unstrukturierten Daten in der Cloud mit Azure Blob-Speicher (Objektspeicher)."
    services="storage"
    documentationCenter="java"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="how-to-use-blob-storage-from-java"></a>So verwenden Sie BLOB-Speicher von Java

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>(Übersicht)

Azure Blob-Speicher ist ein Dienst, der unstrukturierte Daten als Objekte/Blobs in der Cloud gespeichert sind. BLOB-Speicher kann beliebige binäre Daten, beispielsweise ein Dokument, Media-Datei oder das Anwendungsinstallationsprogramm oder zu speichern. BLOB-Speicher wird auch als Objektspeicher bezeichnet.

In diesem Artikel wird gezeigt, wie häufige Szenarien mit Microsoft Azure Blob-Speicher ausführen werden. Die Beispiele in Java geschrieben sind, und verwenden Sie den [Azure-Speicher SDK für Java][]. Die Szenarios dieser einschließen **Hochladen**, **Auflisten**, **herunterladen**und **Löschen von** Blobs. Weitere Informationen Blobs finden Sie im Abschnitt für die [Nächsten Schritte](#Next-Steps) .

> [AZURE.NOTE] Ein SDK steht für Entwickler, die Azure-Speicher auf Android-Geräten verwenden. Weitere Informationen finden Sie unter der [Azure-Speicher SDK für Android][].

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a>Erstellen einer Java-Anwendungs

In diesem Artikel werden Sie Speicher Features verwenden, die innerhalb einer Java-Anwendung lokal oder innerhalb einer Webrolle oder Worker-Rolle in Azure ausgeführt Code ausgeführt werden kann.

Dazu müssen Sie Java Development Kit (JDK) installieren und erstellen Sie ein Azure-Speicher-Konto in Ihrem Azure-Abonnement. Nachdem Sie getan haben, müssen Sie überprüfen, ob Ihr Entwicklungssystem erfüllt die Mindestanforderungen und Abhängigkeiten, die im Repository [Azure Speicher SDK für Java][] auf GitHub aufgeführt sind. Wenn das System über diese Anforderungen erfüllt, können Sie die Anweisungen zum Herunterladen und Installieren der Azure-Speicherbibliotheken für Java auf Ihrem System aus diesem Repository folgen. Nachdem Sie diese Aufgaben abgeschlossen haben, werden Sie möglicherweise eine Java-Anwendung zu erstellen, die in den Beispielen in diesem Artikel verwendet.

## <a name="configure-your-application-to-access-blob-storage"></a>Konfigurieren der Anwendungs Zugriff auf Blob-Speicher

Fügen Sie die folgenden Aussagen importieren an den Anfang der Java-Datei, die Sie die Azure-Speicher-APIs verwenden, um Blobs zugreifen möchten.

    // Include the following imports to use blob APIs.
    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.blob.*;

## <a name="set-up-an-azure-storage-connection-string"></a>Einrichten einer Verbindungszeichenfolge Azure-Speicher

Ein Azure-Speicher-Client verwendet eine Verbindungszeichenfolge Speicher zum Speichern von Endpunkten und die Anmeldeinformationen für den Zugriff auf Daten Management Services. Wenn Sie in einer Clientanwendung ausführen zu können, müssen Sie die Verbindungszeichenfolge Speicher in folgendem Format ein, bereitstellen, verwenden den Namen der Ihr Speicherkonto und die primäre Zugriffstaste für das Speicherkonto im [Azure-Portal](https://portal.azure.com) für die Werte *Kontoname* und *AccountKey* aufgeführt. Im folgende Beispiel wird gezeigt, wie Sie ein statisches Feld zu halten Sie die Verbindungszeichenfolge manuell deklarieren können.

    // Define the connection-string with your values
    public static final String storageConnectionString =
        "DefaultEndpointsProtocol=http;" +
        "AccountName=your_storage_account;" +
        "AccountKey=your_storage_account_key";

In einer Anwendung, die innerhalb einer Rolle in Microsoft Azure ausgeführt wird wird diese Zeichenfolge in der Konfiguration Dienstdatei *ServiceConfiguration.cscfg*, gespeichert werden kann und einen Anruf an die Methode **RoleEnvironment.getConfigurationSettings** zugegriffen werden kann. Im folgenden Beispiel wird Ruft die Verbindungszeichenfolge aus einem benannten *StorageConnectionString* in der Konfiguration Dienstdatei **Einstellung** -Element ab.

    // Retrieve storage account from connection-string.
    String storageConnectionString =
        RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");

In den folgenden Beispielen wird davon ausgegangen, dass Sie eine der folgenden beiden Methoden zum Abrufen der Verbindungszeichenfolge Speicher verwendet haben.

## <a name="create-a-container"></a>Erstellen eines Containers

Ein Objekt **CloudBlobClient** können Sie die Verweisobjekte für Container und Blobs zu erhalten. Der folgende Code erstellt ein **CloudBlobClient** Objekt.

> [AZURE.NOTE] Es gibt weitere Methoden zum Erstellen von **CloudStorageAccount** Objekte; Weitere Informationen finden Sie unter **CloudStorageAccount** in den [Azure-Speicher Client SDK-Referenz].

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

Verwenden Sie das Objekt **CloudBlobClient** , um einen Verweis auf den Container zu erhalten, die Sie verwenden möchten. Sie können den Container erstellen, wenn sie mit der Methode **CreateIfNotExists** vorhanden ist, die andernfalls den vorhandenen Container zurückgibt. Standardmäßig ist die neue Container privat, damit Sie Ihre Speicher Zugriffstaste angeben müssen (wie zuvor) zum Herunterladen von Blobs aus diesem Container.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

        // Create the blob client.
       CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

       // Get a reference to a container.
       // The container name must be lower case
       CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

       // Create the container if it does not exist.
        container.createIfNotExists();
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

### <a name="optional-configure-a-container-for-public-access"></a>Optional: Konfigurieren eines Containers für öffentliche Zugriff

Berechtigungen für einen Container für private Access standardmäßig konfiguriert sind, aber Sie können ganz einfach Konfigurieren eines Containers Berechtigungen zu öffentlichen, schreibgeschützten Zugriff für alle Benutzer im Internet zu ermöglichen:

    // Create a permissions object.
    BlobContainerPermissions containerPermissions = new BlobContainerPermissions();

    // Include public access in the permissions object.
    containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);

    // Set the permissions on the container.
    container.uploadPermissions(containerPermissions);

## <a name="upload-a-blob-into-a-container"></a>Hochladen eines BLOBs in einen container

Klicken Sie zum Hochladen einer Datei in ein Blob Abrufen eines Containers Verweises, und verwenden sie einen Verweis Blob abrufen. Nachdem Sie einen Bezug Blob haben, können Sie alle Stream durch Aufrufen von Upload für den Bezug Blob hochladen. Dieser Vorgang erstellt das Blob aus, wenn es nicht vorhanden ist oder wenn bedeutet zu überschreiben. Im folgenden Beispiel zeigt dies und setzt voraus, dass der Container bereits erstellt wurde.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

        // Create the blob client.
        CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

       // Retrieve reference to a previously created container.
        CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

        // Define the path to a local file.
        final String filePath = "C:\\myimages\\myimage.jpg";

        // Create or overwrite the "myimage.jpg" blob with contents from a local file.
        CloudBlockBlob blob = container.getBlockBlobReference("myimage.jpg");
        File source = new File(filePath);
        blob.upload(new FileInputStream(source), source.length());
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="list-the-blobs-in-a-container"></a>Liste der Blobs in einem container

Rufen Sie zunächst einen Containerverweis, wie Sie zum Hochladen eines BLOBs gemacht haben, um die Liste der Blobs in einem Container. Sie können mit einer Schleife **für** des Containers **ListBlobs** Methode verwenden. Mit dem folgende Code wird den Uri der einzelnen Blob in einem Container auf der Konsole ausgegeben.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

        // Create the blob client.
        CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

        // Retrieve reference to a previously created container.
        CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

        // Loop over blobs within the container and output the URI to each of them.
        for (ListBlobItem blobItem : container.listBlobs()) {
           System.out.println(blobItem.getUri());
       }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

Beachten Sie, dass Sie einen Namen Blobs mit Pfadinformationen in ihren Namen eingeben können. Dies erstellt eine virtuelle Verzeichnis-Struktur, die Sie organisieren und durchlaufen, wie Sie ein traditionelles Dateisystem verwenden können. Beachten Sie, dass die Verzeichnisstruktur nur virtuelle steht – nur Ressourcen in Blob-Speicher verfügbar, Container und Blobs sind. Die Client-Bibliothek bietet jedoch eine **CloudBlobDirectory** -Objekt, schlagen Sie in einem virtuellen Verzeichnis und vereinfachen das Arbeiten mit Blobs, die auf diese Weise angeordnet sind.

Beispielsweise könnten Sie ein Containers mit dem Namen "Fotos", in denen Sie Blobs mit dem Namen "rootphoto1", "2010/Foto1", "2010/Foto2" und "2011/Foto1" Hochladen möglicherweise haben. Dies würde die virtuellen Verzeichnisse "2010" und "2011" innerhalb des Containers "Fotos" erstellen. Wenn Sie auf den Container "Fotos" **ListBlobs** aufrufen, enthält die zurückgegebene Auflistung **CloudBlobDirectory** und **CloudBlob** -Objekte, die Verzeichnisse und Blobs enthalten sind, auf der obersten Ebene darstellt. In diesem Fall werden Verzeichnisse "2010" und "2011" als auch Foto "rootphoto1" zurückgegeben. Der **Instanceof** -Operator können diese Objekte zu unterscheiden.

Optional können Sie Parameter an die Methode **ListBlobs** mit der **UseFlatBlobListing** -Parameter auf True festgelegt übergeben. Dies führt zu Fehlern in jeder Blob zurückgegeben wird, unabhängig vom Verzeichnis. Weitere Informationen finden Sie unter **CloudBlobContainer.listBlobs** in den [Azure-Speicher Client SDK-Referenz].

## <a name="download-a-blob"></a>Herunterladen eines BLOBs

Wenn Blobs herunterladen möchten, führen Sie dieselben Schritte wie beim für das Hochladen eines BLOBs akzeptieren, um einen Verweis Blob erhalten möchten. Im Beispiel hochladen können Sie für das Objekt Blob Upload aufgerufen. Rufen Sie im folgenden Beispiel Download, um den Blob-Inhalt in einem Streamobjekt wie **FileOutputStream** zu übertragen, mit denen Sie das Blob in einer lokalen Datei beibehalten werden.

    try
    {
        // Retrieve storage account from connection-string.
       CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

       // Create the blob client.
       CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

       // Retrieve reference to a previously created container.
       CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

       // Loop through each blob item in the container.
       for (ListBlobItem blobItem : container.listBlobs()) {
           // If the item is a blob, not a virtual directory.
           if (blobItem instanceof CloudBlob) {
               // Download the item and save it to a file with the same name.
                CloudBlob blob = (CloudBlob) blobItem;
                blob.download(new FileOutputStream("C:\\mydownloads\\" + blob.getName()));
            }
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="delete-a-blob"></a>Löschen eines BLOBs

Zum Löschen eines BLOBs, erhalten eine Blob-Referenz, und rufen **DeleteIfExists**.

    try
    {
       // Retrieve storage account from connection-string.
       CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

       // Create the blob client.
       CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

       // Retrieve reference to a previously created container.
       CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

       // Retrieve reference to a blob named "myimage.jpg".
       CloudBlockBlob blob = container.getBlockBlobReference("myimage.jpg");

       // Delete the blob.
       blob.deleteIfExists();
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="delete-a-blob-container"></a>Löschen eines Containers blob

Schließlich zum Löschen eines Containers Blob rufen Sie eines Containers Blob-Referenz ab, und rufen Sie **DeleteIfExists**.

    try
    {
       // Retrieve storage account from connection-string.
       CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

       // Create the blob client.
       CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

       // Retrieve reference to a previously created container.
       CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

       // Delete the blob container.
       container.deleteIfExists();
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="next-steps"></a>Nächste Schritte

Die Grundlagen des Blob-Speicher bearbeitet haben, folgen Sie diesen Links komplexere Speicheraufgaben Lernen aus.

- [Azure SDK für Java-Speicher][]
- [Azure-Speicher Client SDK-Referenz][]
- [Azure-Speicher REST-API][]
- [Azure-Speicher-Teamblog][]

Weitere Informationen finden Sie auch im [Java Developer Center](/develop/java/).

[Azure SDK for Java]: http://go.microsoft.com/fwlink/?LinkID=525671
[Azure SDK für Java-Speicher]: https://github.com/azure/azure-storage-java
[Azure-Speicher SDK für Android]: https://github.com/azure/azure-storage-android
[Azure-Speicher Client SDK-Referenz]: http://dl.windowsazure.com/storage/javadoc/
[Azure-Speicher REST-API]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Azure-Speicher-Teamblog]: http://blogs.msdn.com/b/windowsazurestorage/

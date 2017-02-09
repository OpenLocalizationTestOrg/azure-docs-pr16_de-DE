<properties
    pageTitle="Erste Schritte mit Azure Blob-Speicher (Objektspeicher) mit .NET | Microsoft Azure"
    description="Speichern von unstrukturierten Daten in der Cloud mit Azure Blob-Speicher (Objektspeicher)."
    services="storage"
    documentationCenter=".net"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="get-started-with-azure-blob-storage-using-net"></a>Erste Schritte mit Azure Blob-Speicher mit .NET

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>(Übersicht)

Azure Blob-Speicher ist ein Dienst, der unstrukturierte Daten als Objekte/Blobs in der Cloud gespeichert sind. BLOB-Speicher kann beliebige binäre Daten, beispielsweise ein Dokument, Media-Datei oder das Anwendungsinstallationsprogramm oder zu speichern. BLOB-Speicher wird auch als Objektspeicher bezeichnet.

### <a name="about-this-tutorial"></a>Informationen zu diesem Lernprogramm

In diesem Lernprogramm erfahren, wie .NET Code für einige häufige Szenarien mit Azure Blob-Speicher geschrieben. Verdeckt Szenarien enthalten hochladen, auflisten, herunterladen und Löschen von Blobs.

**Geschätzte Dauer:** 45 Minuten

**Prerequisities:**

- [Microsoft Visual Studio](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx)
- [Client-Bibliothek für .NET Azure-Speicher](https://www.nuget.org/packages/WindowsAzure.Storage/)
- [Azure-Konfigurations-Manager für .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
- Ein [Konto Azure-Speicher](storage-create-storage-account.md#create-a-storage-account)


[AZURE.INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

### <a name="more-samples"></a>Weitere Beispiele

Weitere Beispiele für die Verwendung von Blob-Speicher finden Sie unter [Erste Schritte mit Azure BLOB-Speicher in .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/). Sie können die Stichprobe Anwendung herunterladen und auszuführen, oder Durchsuchen Sie den Code auf GitHub.


[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[AZURE.INCLUDE [storage-development-environment-include](../../includes/storage-development-environment-include.md)]

### <a name="add-namespace-declarations"></a>Fügen Sie Namespacedeklarationen

Fügen Sie den folgenden `using` Anweisungen am oberen Rand der `program.cs` Datei:

    using Microsoft.Azure; // Namespace for CloudConfigurationManager
    using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
    using Microsoft.WindowsAzure.Storage.Blob; // Namespace for Blob storage types

### <a name="parse-the-connection-string"></a>Analysieren Sie die Verbindungszeichenfolge

[AZURE.INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-the-blob-service-client"></a>Erstellen des Blob-Service-Clients

Die **CloudBlobClient** -Klasse können Sie zum Abrufen von Containern und Blobs gehörende Kehrmatrix Blob-Speicher. Hier ist eine Methode, um den Service-Client zu erstellen:

    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

Jetzt sind Sie bereit sind, Schreiben von Code, die Daten aus liest und schreibt Daten in Blob-Speicher.

## <a name="create-a-container"></a>Erstellen eines Containers

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

Dieses Beispiel zeigt, wie Sie einen Container erstellen, wenn es nicht bereits vorhanden ist:

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve a reference to a container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Create the container if it doesn't already exist.
    container.CreateIfNotExists();

Standardmäßig ist der neue Container privat, was bedeutet, dass Sie Ihre Speicher Zugriffstaste zum Herunterladen von Blobs aus diesem Container angeben müssen. Wenn Sie die Dateien innerhalb des Containers für alle Benutzer zur Verfügung stellen möchten, können Sie den Container mit dem folgenden Code öffentlich sein festlegen:

    container.SetPermissions(
        new BlobContainerPermissions { PublicAccess = BlobContainerPublicAccessType.Blob });

Jeder im Internet kann in einem öffentlichen Container Blobs sehen, jedoch können Sie ändern oder löschen ist möglich, wenn die entsprechende Konto Zugriffstaste oder einer freigegebenen Access-Signatur ist.

## <a name="upload-a-blob-into-a-container"></a>Hochladen eines BLOBs in einen container

Azure Blob-Speicher unterstützt blockieren Blobs und Seitenblobs.  In den meisten Fällen ist blockieren Blob die empfohlenen ein verwenden.

Klicken Sie zum Hochladen einer Datei auf einen Blockblob Abrufen eines Containers Verweises, und verwenden sie einen Block Blob Verweis abrufen. Nachdem Sie einen Bezug Blob haben, können Sie alle Stream Daten darauf hochladen, durch die Methode **UploadFromStream** aufrufen. Dieser Vorgang erstellt das Blob aus, wenn er nicht zuvor vorhanden oder überschreiben Sie ihn an, wenn er vorhanden ist.

Im folgenden Beispiel wird gezeigt, wie einen Blob in einen Container hochladen und wird davon ausgegangen, dass der Container bereits erstellt wurde.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite the "myblob" blob with contents from a local file.
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        blockBlob.UploadFromStream(fileStream);
    }

## <a name="list-the-blobs-in-a-container"></a>Liste der Blobs in einem container

Rufen Sie zunächst einen Containerverweis, um die Liste der Blobs in einem Container. Des Containers **ListBlobs** Methode können dann die Blobs und/oder Verzeichnisse darin abzurufen. Um die Rich-Satz von Eigenschaften und Methoden für eine zurückgegebene **IListBlobItem**zugreifen zu können, müssen Sie es zu einem Objekt **CloudBlockBlob**, **CloudPageBlob**oder **CloudBlobDirectory** umwandeln.  Wenn der Typ bekannt ist, können Sie ein Häkchen Typ ermitteln, welche wandeln Sie es zu verwenden.  Im folgende Code wird veranschaulicht, wie abrufen und ausgeben den URI für jedes Element in der `photos` Container:

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("photos");

    // Loop over items within the container and output the length and URI.
    foreach (IListBlobItem item in container.ListBlobs(null, false))
    {
        if (item.GetType() == typeof(CloudBlockBlob))
        {
            CloudBlockBlob blob = (CloudBlockBlob)item;

            Console.WriteLine("Block blob of length {0}: {1}", blob.Properties.Length, blob.Uri);

        }
        else if (item.GetType() == typeof(CloudPageBlob))
        {
            CloudPageBlob pageBlob = (CloudPageBlob)item;

            Console.WriteLine("Page blob of length {0}: {1}", pageBlob.Properties.Length, pageBlob.Uri);

        }
        else if (item.GetType() == typeof(CloudBlobDirectory))
        {
            CloudBlobDirectory directory = (CloudBlobDirectory)item;

            Console.WriteLine("Directory: {0}", directory.Uri);
        }
    }

Wie oben gezeigt wird, können Sie Blobs mit Pfadinformationen in ihren Namen benennen. Dies erstellt eine virtuelle Verzeichnis-Struktur, die Sie organisieren und durchlaufen, wie Sie ein traditionelles Dateisystem verwenden können. Beachten Sie, dass die Verzeichnisstruktur nur virtuelle steht – nur Ressourcen in Blob-Speicher verfügbar, Container und Blobs sind. Die Speicher-Client-Bibliothek bietet jedoch eine **CloudBlobDirectory** -Objekt, schlagen Sie in einem virtuellen Verzeichnis und vereinfachen das Arbeiten mit Blobs, die auf diese Weise angeordnet sind.

Betrachten Sie beispielsweise den folgenden Satz von blockieren Blobs in einem Container mit dem Namen `photos`:

    photo1.jpg
    2010/architecture/description.txt
    2010/architecture/photo3.jpg
    2010/architecture/photo4.jpg
    2011/architecture/photo5.jpg
    2011/architecture/photo6.jpg
    2011/architecture/description.txt
    2011/photo7.jpg

Wenn Sie auf den Container 'Fotos' (wie in der obigen Beispiel) **ListBlobs** anrufen, wird eine hierarchische Liste zurückgegeben. Sie enthält sowohl die **CloudBlobDirectory** **CloudBlockBlob** Objekte, die Verzeichnisse und Blobs im Container darstellt. Die Ausgabe sieht wie folgt aus:

    Directory: https://<accountname>.blob.core.windows.net/photos/2010/
    Directory: https://<accountname>.blob.core.windows.net/photos/2011/
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg


Optional können Sie den Parameter **UseFlatBlobListing** von der Methode **ListBlobs** auf **true**festlegen. In diesem Fall wird jeder Blob im Container als ein Objekt **CloudBlockBlob** zurückgegeben. Der Anruf an die **ListBlobs** eine flache Auflistung zurückzugebenden sieht wie folgt aus:

    // Loop over items within the container and output the length and URI.
    foreach (IListBlobItem item in container.ListBlobs(null, true))
    {
       ...
    }

und die Ergebnisse wie folgt aussehen:

    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2010/architecture/description.txt
    Block blob of length 314618: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo3.jpg
    Block blob of length 522713: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo4.jpg
    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2011/architecture/description.txt
    Block blob of length 419048: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo5.jpg
    Block blob of length 506388: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo6.jpg
    Block blob of length 399751: https://<accountname>.blob.core.windows.net/photos/2011/photo7.jpg
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg


## <a name="download-blobs"></a>Herunterladen von blobs

Klicken Sie zum Herunterladen Blobs zuerst rufen Sie einen Blob-Verweis ab, und rufen Sie die Methode **DownloadToStream** . Im folgende Beispiel verwendet die **DownloadToStream** Methode, um den Blob-Inhalt in einem Streamobjekt zu übertragen, die Sie dann in eine lokale Datei beibehalten werden können.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save blob contents to a file.
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        blockBlob.DownloadToStream(fileStream);
    }

Die **DownloadToStream** -Methode können Sie auch den Inhalt eines Blob als Zeichenfolge herunterladen.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "myblob.txt"
    CloudBlockBlob blockBlob2 = container.GetBlockBlobReference("myblob.txt");

    string text;
    using (var memoryStream = new MemoryStream())
    {
        blockBlob2.DownloadToStream(memoryStream);
        text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
    }

## <a name="delete-blobs"></a>Löschen von blobs

Zum Löschen eines BLOBs zuerst einen Verweis Blob und rufen Sie dann die Methode **Löschen** daran.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete the blob.
    blockBlob.Delete();


## <a name="list-blobs-in-pages-asynchronously"></a>Asynchrone Liste von Blobs in Seiten

Wenn Ihr eine große Anzahl von Blobs Angebot oder die Anzahl der Ergebnisse zu steuern, die Sie in einem Eintrag Vorgang zurückkehren möchten, können Sie Blobs in Seiten Ergebnisse auflisten. In diesem Beispiel wird veranschaulicht, wie asynchrone zurückzugebenden Ergebnisse in Seiten, sodass die Ausführung nicht blockiert, während Sie warten, um eine große Menge von Ergebnissen zurückzukehren.

Dieses Beispiel zeigt einen flachen Blob auflisten, aber Sie können auch eine hierarchische Liste, durchführen, indem Sie festlegen der `useFlatBlobListing` Parameter der **ListBlobsSegmentedAsync** -Methode `false`.

Da die Methode für die Stichprobe eine asynchrone Methode ruft, muss es mit vorangestellten der `async` Schlüsselwort, und er muss ein **Task** -Objekt zurückgeben. Das Await-Schlüsselwort für das **ListBlobsSegmentedAsync** angegebene ausgesetzt Ausführung der Stichprobe Methode bis der Eintrag Vorgang abgeschlossen ist.

    async public static Task ListBlobsSegmentedInFlatListing(CloudBlobContainer container)
    {
        //List blobs to the console window, with paging.
        Console.WriteLine("List blobs in pages:");

        int i = 0;
        BlobContinuationToken continuationToken = null;
        BlobResultSegment resultSegment = null;

        //Call ListBlobsSegmentedAsync and enumerate the result segment returned, while the continuation token is non-null.
        //When the continuation token is null, the last page has been returned and execution can exit the loop.
        do
        {
            //This overload allows control of the page size. You can return all remaining results by passing null for the maxResults parameter,
            //or by calling a different overload.
            resultSegment = await container.ListBlobsSegmentedAsync("", true, BlobListingDetails.All, 10, continuationToken, null, null);
            if (resultSegment.Results.Count<IListBlobItem>() > 0) { Console.WriteLine("Page {0}:", ++i); }
            foreach (var blobItem in resultSegment.Results)
            {
                Console.WriteLine("\t{0}", blobItem.StorageUri.PrimaryUri);
            }
            Console.WriteLine();

            //Get the continuation token.
            continuationToken = resultSegment.ContinuationToken;
        }
        while (continuationToken != null);
    }

## <a name="writing-to-an-append-blob"></a>Schreiben in eine Blob Anfügen

Ein Blob Anfügen ist eine neue Art von Blob, eingeführt, mit der Version 5.x der Clientbibliothek Azure-Speicher für .NET. Ein Blob Anfügen ist für Vorgänge anfügen, wie z. B. protokollieren optimiert. Wie ein Blob blockieren einer anfügen Blob besteht aus Bausteinen, aber wenn Sie einen neuen Textblock ein Blob anfügen hinzufügen, ist es immer angefügt an das Ende der Blob. Aktualisieren oder Löschen einen vorhandenen Block in einer Blob anfügen. Des Zeitraums IDs für eine anfügen Blob sind nicht verfügbar, wie für ein Blob blockieren.

Jeder Block in einer Blob anfügen kann eine andere Größe, bis zu einem Maximum von 4 MB und eine Blob anfügen kann maximal 50.000 Blöcke enthalten. Die maximale Größe eines Blob Anfügen ist deshalb etwas mehr als 195 GB (4 MB X 50.000 Blöcke).

Im folgenden Beispiel erstellt einen neue anfügen Blob und einiger Daten darauf, stellt eine einfache Protokollierung simulieren.

    //Parse the connection string for the storage account.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

    //Create service client for credentialed access to the Blob service.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    //Get a reference to a container.
    CloudBlobContainer container = blobClient.GetContainerReference("my-append-blobs");

    //Create the container if it does not already exist.
    container.CreateIfNotExists();

    //Get a reference to an append blob.
    CloudAppendBlob appendBlob = container.GetAppendBlobReference("append-blob.log");

    //Create the append blob. Note that if the blob already exists, the CreateOrReplace() method will overwrite it.
    //You can check whether the blob exists to avoid overwriting it by using CloudAppendBlob.Exists().
    appendBlob.CreateOrReplace();

    int numBlocks = 10;

    //Generate an array of random bytes.
    Random rnd = new Random();
    byte[] bytes = new byte[numBlocks];
    rnd.NextBytes(bytes);

    //Simulate a logging operation by writing text data and byte data to the end of the append blob.
    for (int i = 0; i < numBlocks; i++)
    {
        appendBlob.AppendText(String.Format("Timestamp: {0:u} \tLog Entry: {1}{2}",
            DateTime.UtcNow, bytes[i], Environment.NewLine));
    }

    //Read the append blob to the console window.
    Console.WriteLine(appendBlob.DownloadText());

Weitere Informationen zu den Unterschieden zwischen den drei Arten von Blobs finden Sie unter [Grundlegendes zu blockieren Blobs, Seitenblobs, und Blobs angefügt werden soll](https://msdn.microsoft.com/library/azure/ee691964.aspx) .

## <a name="managing-security-for-blobs"></a>Verwalten der Sicherheit für blobs

Standardmäßig behält Azure-Speicher von Daten durch Einschränken des Zugriffs auf das, die im Besitz der Zugriffstasten Konto ist Kontobesitzer secure. Wenn Sie BLOB-Daten in Ihr Konto Speicherplatz freigeben müssen, ist es wichtig, ohne dass die Sicherheit von Ihrem Konto Zugriffstasten beeinträchtigt vergeblich. Darüber hinaus können Sie verschlüsseln Blob-Daten, um sicherzustellen, dass sie über das Netzwerk und Azure-Speicher vertraut sicher ist.

[AZURE.INCLUDE [storage-account-key-note-include](../../includes/storage-account-key-note-include.md)]

### <a name="controlling-access-to-blob-data"></a>Steuern des Zugriffs auf BLOB-Daten

BLOB-Daten in Ihr Speicherkonto zugegriffen werden standardmäßig nur für Speicher-Kontobesitzer. Authentifizieren gegen die Blob-Speicher erfordert die Zugriffstaste Konto standardmäßig. Möglicherweise möchten Sie jedoch, bestimmte BLOB-Daten für andere Benutzer verfügbar machen. Sie haben zwei Optionen:

- **Anonymer Zugriff:** Sie können eines Containers oder deren Blobs für anonymer Zugriff öffentlich verfügbar machen. Weitere Informationen finden Sie unter [anonyme Lesezugriff auf Container und Blobs verwalten](storage-manage-access-to-resources.md) .
- **Freigegeben Access Signaturen:** Sie können Clients mit einer freigegebenen Access-Signatur (SAS), bereitstellen, die mit den Berechtigungen, die Sie angeben und über einen bestimmten Zeitraum, den Sie angeben delegierten Zugriff auf eine Ressource in Ihr Speicherkonto bereitstellt. Weitere Informationen finden Sie unter [Verwenden von freigegebenen Access Signaturen (SAS)](storage-dotnet-shared-access-signature-part-1.md) .

### <a name="encrypting-blob-data"></a>Verschlüsseln von Blob-Daten

Azure-Speicher unterstützt Verschlüsselung Blob-Daten auf dem Client sowohl auf dem Server:

- **Clientseitige Verschlüsselung:** Der Speicher-Client-Bibliothek für .NET unterstützt Verschlüsselung von Daten in Clientanwendungen vor dem Hochladen in Azure-Speicher und Entschlüsseln von Daten beim Herunterladen auf den Client. Die Bibliothek unterstützt Integration mit Azure-Taste Tresor auch für Speicher-Konto Key-Verwaltung. Weitere Informationen finden Sie unter [Clientseitige Verschlüsselung mit .NET für Microsoft Azure-Speicher](storage-client-side-encryption.md) . Siehe auch [Lernprogramm: Verschlüsseln und Entschlüsseln Blobs in Azure-Taste Tresor mit Microsoft Azure-Speicher](storage-encrypt-decrypt-blobs-key-vault.md).
- **Serverseitige Verschlüsselung**: Azure-Speicher unterstützt jetzt die serverseitige Verschlüsselung. Finden Sie unter [Azure-Speicher Dienst Verschlüsselung von Daten in der restlichen (Preview)](storage-service-encryption.md).

## <a name="next-steps"></a>Nächste Schritte

Die Grundlagen des Blob-Speicher bearbeitet haben, führen Sie die folgenden Links, um weitere Informationen.

### <a name="microsoft-azure-storage-explorer"></a>Microsoft Azure-Speicher-Explorer
- [Microsoft Azure Speicher Explorer (MASE)](../vs-azure-tools-storage-manage-with-storage-explorer.md) ist kostenlos, eigenständige Anwendung von Microsoft, die Sie auf Windows, OS X und Linux visuell mit Azure-Speicher Daten arbeiten kann.

### <a name="blob-storage-samples"></a>Beispiele für BLOB-Speicher

- [Erste Schritte mit Azure Blob-Speicher in .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/)

### <a name="blob-storage-reference"></a>BLOB-Speicher Bezug

- [Speicher-Client-Bibliothek für .NET Verweis](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
- [REST-API-Referenz](http://msdn.microsoft.com/library/azure/dd179355)

### <a name="conceptual-guides"></a>Konzeptionelle Führungslinien

- [Übertragen von Daten mit dem Befehlszeilendienstprogramm AzCopy](storage-use-azcopy.md)
- [Erste Schritte mit Dateispeicher für .NET](storage-dotnet-how-to-use-files.md)
- [Verwenden von Azure Blob-Speicher mit dem WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)

  [Blob5]: ./media/storage-dotnet-how-to-use-blobs/blob5.png
  [Blob6]: ./media/storage-dotnet-how-to-use-blobs/blob6.png
  [Blob7]: ./media/storage-dotnet-how-to-use-blobs/blob7.png
  [Blob8]: ./media/storage-dotnet-how-to-use-blobs/blob8.png
  [Blob9]: ./media/storage-dotnet-how-to-use-blobs/blob9.png

  [Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
  [Configuring Connection Strings]: http://msdn.microsoft.com/library/azure/ee758697.aspx
  [.NET client library reference]: http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409
  [REST API reference]: http://msdn.microsoft.com/library/azure/dd179355

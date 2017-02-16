<properties
    pageTitle="Erste Schritte mit Blob-Speicher und Visual Studio verbunden Services (ASP.NET 5) | Microsoft Azure"
    description="Erste Schritte mit Azure Blob-Speicher in einem Projekt Visual Studio ASP.NET 5, nach der Erstellung eines Speicher-Kontos, die mit Visual Studio verbunden services"
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

# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-aspnet-5"></a>Erste Schritte mit Azure Blob-Speicher und Visual Studio verbunden Services (ASP.NET 5)

[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

##<a name="overview"></a>(Übersicht)

In diesem Artikel werden die Schritte in Visual Studio Azure Blob-Speicher verwenden, nachdem Sie erstellt oder auf die verwiesen wird ein Konto Azure-Speicher in einem Projekt ASP.NET 5 über das Dialogfeld Visual Studio verbunden Dienste hinzufügen.

Azure Blob-Speicher ist ein Dienst für das Speichern von große Mengen von unstrukturierten Daten, die über eine beliebige Stelle in der Welt über HTTP oder HTTPS zugegriffen werden können. Ein einzelnes Blob kann jede beliebige Größe sein. BLOBs können Elemente wie Bilder, Audio-und Videodateien, unformatierten Daten und Dokumentdateien werden. Dieser Artikel beschreibt, wie mit Blob-Speicher Schritte, nachdem Sie ein Konto Azure-Speicher mithilfe des Dialogfelds Visual Studio **Verbunden Dienste hinzufügen** in einem Projekt ASP.NET 5 erstellt haben.

So, wie Sie Dateien in Ordnern live, live Speicher Blobs in Containern. Nachdem Sie einen Speicher erstellt haben, erstellen Sie eine oder mehrere Container im Speicher. In einem Speicher "Scrapbook" aufgerufen, beispielsweise können Sie Container erstellen, in die Speicherung namens "Bilder", um Bilder zu speichern und ein anderes zum Speichern von Audiodateien "audio" bezeichnet. Nachdem Sie den Container erstellen, können Sie können einzelne BLOB-Dateien hochladen. Weitere Informationen zum Bearbeiten von programmgesteuert Blobs finden Sie unter [Erste Schritte mit Azure Blob-Speicher mit .NET](storage-dotnet-how-to-use-blobs.md) .

##<a name="access-blob-containers-in-code"></a>Access Blob Container Code

Blobs in Projekten ASP.NET 5 programmgesteuert Zugriff auf, müssen Sie die folgenden Elemente hinzufügen, wenn diese nicht bereits vorhanden sind.

1. Fügen Sie die folgenden Code Namespacedeklarationen, an den Anfang einer beliebigen c#-Datei, in der Sie programmgesteuert Azure-Speicher zugreifen möchten.

        using Microsoft.Extensions.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Extensions.Logging.LogLevel;

2. Abrufen einer **CloudStorageAccount** -Objekt, das Ihre Kontoinformationen Speicher darstellt. Verwenden Sie den folgenden Code zum Abrufen der Ihre Verbindungszeichenfolge Speicher und die Speicher-Kontoinformationen aus der Konfiguration Azure Service.

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

    **Hinweis:** Verwenden der oben aufgeführte Code vor den Code in den folgenden Abschnitten.


3. Verwenden Sie ein Objekt **CloudBlobClient** , um einen **CloudBlobContainer** Bezug zu einem vorhandenen Container in Ihr Speicherkonto zu gelangen.

        // Create a blob client.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        // Get a reference to a container named “mycontainer.”
        CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");



##<a name="create-a-container-in-code"></a>Erstellen eines Containers Code

Die **CloudBlobClient** können Sie auch um einen Container in Ihr Speicherkonto zu erstellen. Sie müssen lediglich einen Anruf an **CreateIfNotExistsAsync** wie in den folgenden Code hinzufügen:

    // Create a blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Get a reference to a container named “my-new-container.”
    CloudBlobContainer container = blobClient.GetContainerReference("my-new-container");

    // If “mycontainer” doesn’t exist, create it.
    await container.CreateIfNotExistsAsync();


**Hinweis:** Die APIs, die Anrufe an Azure-Speicher in ASP.NET 5 ausführen, sind asynchrone. Weitere Informationen finden Sie unter [asynchrone Programmierung mit asynchronen und Await](http://msdn.microsoft.com/library/hh191443.aspx) . Im folgenden Code wird davon ausgegangen, dass asynchrone Programmierung Methoden verwendet werden.

Um die Dateien innerhalb des Containers für alle Benutzer verfügbar zu machen, können Sie den Container öffentlich sein, mit dem folgenden Code festlegen.

    await container.SetPermissionsAsync(new BlobContainerPermissions
    {
        PublicAccess = BlobContainerPublicAccessType.Blob
    });

##<a name="upload-a-blob-into-a-container"></a>Hochladen eines BLOBs in einen container

Klicken Sie zum Hochladen einer Blobdatei in einen Container Abrufen eines Containers Verweises, und verwenden sie einen Verweis Blob abrufen. Nachdem Sie einen Bezug Blob haben, können Sie alle Stream Daten darauf hochladen, durch die Methode **UploadFromStreamAsync** aufrufen. Dieser Vorgang erstellt das Blob aus, wenn es nicht bereits vorhanden ist, oder es überschreibt, sofern sie vorhanden ist. Im folgenden Beispiel wird gezeigt, wie einen Blob in einen Container hochladen und wird davon ausgegangen, dass der Container bereits erstellt wurde.

    // Get a reference to a blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite the "myblob" blob with the contents of a local file
    // named “myfile”.
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        await blockBlob.UploadFromStreamAsync(fileStream);
    }

##<a name="list-the-blobs-in-a-container"></a>Liste der Blobs in einem container
Rufen Sie zunächst einen Containerverweis, um die Liste der Blobs in einem Container. Sie können dann des Containers **ListBlobsSegmentedAsync** Methode, um die Blobs und/oder Verzeichnisse darin abzurufen aufrufen. Um die Rich-Satz von Eigenschaften und Methoden für eine zurückgegebene **IListBlobItem**zugreifen zu können, müssen Sie es zu einem Objekt **CloudBlockBlob**, **CloudPageBlob**oder **CloudBlobDirectory** umwandeln. Wenn Sie nicht, dass den BLOB-Typ wissen, können ein Häkchen Typ Sie bestimmen, welche er umgewandelt. Im folgende Code veranschaulicht, wie abrufen und ausgeben den URI für jedes Element in einem Container.

    BlobContinuationToken token = null;
    do
    {
        BlobResultSegment resultSegment = await container.ListBlobsSegmentedAsync(token);
        token = resultSegment.ContinuationToken;

        foreach (IListBlobItem item in resultSegment.Results)
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
    } while (token != null);

Es gibt andere Methoden zum Auflisten des Inhalts eines Containers Blob. Weitere Informationen finden Sie unter [Erste Schritte mit Azure Blob-Speicher mit .NET](storage-dotnet-how-to-use-blobs.md#list-the-blobs-in-a-container) .

##<a name="download-a-blob"></a>Herunterladen eines BLOBs
Zum Herunterladen eines BLOBs, rufen Sie zunächst einen Verweis auf die Blob und dann die Methode **DownloadToStreamAsync** aufrufen. Im folgende Beispiel verwendet die **DownloadToStreamAsync** Methode, um den Blob-Inhalt in einem Streamobjekt zu übertragen, die dann als lokale Datei gespeichert werden kann.

    // Get a reference to a blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save the blob contents to a file named “myfile”.
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        await blockBlob.DownloadToStreamAsync(fileStream);
    }

Es gibt weitere Methoden zum Blobs als Dateien gespeichert werden. Weitere Informationen finden Sie unter [Erste Schritte mit Azure Blob-Speicher mit .NET](storage-dotnet-how-to-use-blobs.md#download-blobs) .

##<a name="delete-a-blob"></a>Löschen eines BLOBs
Zum Löschen eines BLOBs zuerst einen Verweis auf die Blob, und rufen Sie dann die Methode **DeleteAsync** daran.

    // Get a reference to a blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete the blob.
    await blockBlob.DeleteAsync();

## <a name="next-steps"></a>Nächste Schritte

[AZURE.INCLUDE [vs-storage-dotnet-blobs-next-steps](../../includes/vs-storage-dotnet-blobs-next-steps.md)]

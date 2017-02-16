<properties
    pageTitle="Erste Schritte mit Blob-Speicher und Visual Studio verbunden Services (ASP.NET) | Microsoft Azure"
    description="Erste Schritte mit Azure Blob-Speicher in einem ASP.NET-Projekt in Visual Studio nach dem Herstellen einer Verbindung mit einem Speicherkonto mithilfe von Visual Studio verbunden services"
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

# <a name="get-started-with-blob-storage-and-visual-studio-connected-services-aspnet"></a>Erste Schritte mit Blob-Speicher und Visual Studio verbunden Services (ASP.NET)

[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>(Übersicht)

In diesem Artikel werden die Schritte Azure Blob-Speicher verwenden, nachdem Sie erstellt oder auf die verwiesen wird ein Konto Azure-Speicher in einer app ASP.NET mithilfe des Dialogfelds Visual Studio **Verbunden Dienste hinzufügen** . Der Artikel beschreibt, wie Blob Container erstellen und andere allgemeine Aufgaben wie hochladen, wobei, herunterladen und Löschen von Blobs. Die Beispiele in C geschrieben werden\# und [Microsoft Azure-Speicher-Client-Bibliothek für .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx)verwenden.

 - Weitere allgemeine Informationen zur Verwendung von Azure Blob-Speicher finden Sie unter [Erste Schritte mit Azure Blob-Speicher .NET verwenden](storage-dotnet-how-to-use-blobs.md).
 - Weitere Informationen zu ASP.NET-Projekte finden Sie unter [ASP.NET](http://www.asp.net).


Azure Blob-Speicher ist ein Dienst für das Speichern von große Mengen von unstrukturierten Daten, die über eine beliebige Stelle in der Welt über HTTP oder HTTPS zugegriffen werden können. Ein einzelnes Blob kann jede beliebige Größe sein. BLOBs können Elemente wie Bilder, Audio-und Videodateien, unformatierten Daten und Dokumentdateien werden.

So, wie Sie Dateien in Ordnern live, live Speicher Blobs in Containern. Nachdem Sie ein Speicherkonto erstellt haben, erstellen Sie eine oder mehrere Container im Speicher. Angenommen, in einem Speicher "Scrapbook" aufgerufen, Sie können Blob-Container erstellen, in die Speicherung namens "Bilder", um Bilder zu speichern, und ein anderes zum Speichern von Audiodateien "audio" bezeichnet. Nachdem Sie den Container erstellen, können Sie können einzelne BLOB-Dateien hochladen.




## <a name="access-blob-containers-in-code"></a>Access Blob Container Code

Blobs in Projekten ASP.NET programmgesteuert zugreifen zu können, müssen Sie die folgenden Elemente, hinzufügen, wenn diese nicht bereits vorhanden sind.

1. Fügen Sie die folgenden Code Namespacedeklarationen, an den Anfang einer beliebigen c#-Datei, in der Sie programmgesteuert Azure-Speicher zugreifen möchten.

        using Microsoft.Azure;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Auth;
        using Microsoft.WindowsAzure.Storage.Blob;


2. Abrufen einer **CloudStorageAccount** -Objekt, das Ihre Kontoinformationen Speicher darstellt. Verwenden Sie den folgenden Code zum Abrufen der Ihre Verbindungszeichenfolge Speicher und die Speicher-Kontoinformationen aus der Konfiguration Azure Service.

        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

    > [AZURE.NOTE] Verwenden Sie alle vorherigen Code vor den Code in den folgenden Abschnitten.

3. Abrufen eines Objekts **CloudBlobClient** auf einen vorhandenen Container in Ihr Speicherkonto verweisen.

        // Create a blob client.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        // Get a reference to a container named “mycontainer.”
        CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

> [AZURE.NOTE] Einige der APIs, die Aufrufe Azure-Speicher in ASP.NET 5 ausführen sind asynchrone. Weitere Informationen finden Sie unter [Asynchrone Programmierung mit asynchronen und Await](http://msdn.microsoft.com/library/hh191443.aspx) .


## <a name="create-a-blob-container-in-code"></a>Erstellen eines Containers BLOB-Code

Das Objekt **CloudBlobClient** können Sie auch um einen Container in Ihr Speicherkonto zu erstellen. Sie müssen lediglich einen Anruf an **CreateIfNotExistsAsync** an den oben angegebenen Code hinzufügen, wie im folgenden Beispiel dargestellt.

    // If “mycontainer” doesn’t exist, create it.
    await container.CreateIfNotExistsAsync();

## <a name="upload-a-blob-into-a-container"></a>Hochladen eines BLOBs in einen container

Azure BLOB-Speicher unterstützt blockieren Blobs und Seitenblobs.  In den meisten Fällen ist blockieren Blob die empfohlenen ein verwenden.

Klicken Sie zum Hochladen einer Datei auf einen Blockblob Abrufen eines Containers Verweises, und verwenden sie einen Block Blob Verweis abrufen. Nachdem Sie einen Bezug Blob haben, können Sie alle Stream von Daten zu hochladen, durch die Methode **UploadFromStream** aufrufen. Dieser Vorgang erstellt das Blob aus, wenn er nicht zuvor vorhanden oder überschreiben Sie ihn an, wenn er vorhanden ist. Im folgenden Beispiel wird gezeigt, wie einen Blob in einen Container hochladen und wird davon ausgegangen, dass der Container bereits erstellt wurde.

    // Create or overwrite the "myblob" blob with contents from a local file.
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        blockBlob.UploadFromStream(fileStream);
    }

## <a name="list-the-blobs-in-a-container"></a>Liste der Blobs in einem container

Verwenden Sie die **ListBlobs** Methode zum Abrufen der Blobs und/oder Verzeichnisse darin, um die Liste der Blobs in einem Container. Um die Rich-Satz von Eigenschaften und Methoden für eine zurückgegebene **IListBlobItem**zugreifen zu können, müssen Sie es zu einem Objekt **CloudBlockBlob**, **CloudPageBlob**oder **CloudBlobDirectory** umwandeln.  Wenn der Typ bekannt ist, können Sie ein Häkchen Typ ermitteln, welche wandeln Sie es zu verwenden.  Im folgende Code veranschaulicht, wie abgerufen und den URI für jedes Element im Container **Fotos** ausgegeben.

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

Wie im vorherigen Beispiel dargestellt, enthält der Blob-Dienst des Konzepts der Verzeichnisse im Container, als auch aus. Dies ist, dass Sie Ihre Blobs in eine weitere Ordner wie Struktur organisieren können. Betrachten Sie beispielsweise den folgenden Satz von blockieren Blobs in einem Container mit dem Namen **Fotos**aus.

    photo1.jpg
    2010/architecture/description.txt
    2010/architecture/photo3.jpg
    2010/architecture/photo4.jpg
    2011/architecture/photo5.jpg
    2011/architecture/photo6.jpg
    2011/architecture/description.txt
    2011/photo7.jpg

Wenn Sie **ListBlobs** für den Container 'Fotos' (wie im vorherigen Beispiel gezeigt) aufrufen, enthält die zurückgegebene Auflistung **CloudBlobDirectory** und **CloudBlockBlob** -Objekte, die Verzeichnisse und Blobs enthalten sind, auf der obersten Ebene darstellt. Im folgenden Beispiel wird die Ausgabe an.

    Directory: https://<accountname>.blob.core.windows.net/photos/2010/
    Directory: https://<accountname>.blob.core.windows.net/photos/2011/
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg


Optional können Sie den Parameter **UseFlatBlobListing** von der Methode **ListBlobs** auf **true**festlegen. Dies ergibt jeder Blob als eine **CloudBlockBlob**, unabhängig vom Verzeichnis zurückgegeben wird.  Im folgenden Beispiel wird den Anruf an **ListBlobs**.

    // Loop over items within the container and output the length and URI.
    foreach (IListBlobItem item in container.ListBlobs(null, true))
    {
       ...
    }

Und das nächste Beispiel zeigt die Ergebnisse.

    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2010/architecture/description.txt
    Block blob of length 314618: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo3.jpg
    Block blob of length 522713: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo4.jpg
    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2011/architecture/description.txt
    Block blob of length 419048: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo5.jpg
    Block blob of length 506388: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo6.jpg
    Block blob of length 399751: https://<accountname>.blob.core.windows.net/photos/2011/photo7.jpg
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg



## <a name="download-blobs"></a>Herunterladen von blobs

Verwenden Sie die **DownloadToStream** Methode, um Blobs herunterzuladen. Im folgende Beispiel verwendet die **DownloadToStream** Methode, um den Blob-Inhalt in einem Streamobjekt zu übertragen, die Sie dann in eine lokale Datei beibehalten werden können.

    // Retrieve a reference to a blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save blob contents to a file.
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        blockBlob.DownloadToStream(fileStream);
    }

Die **DownloadToStream** -Methode können Sie auch den Inhalt eines Blob als Zeichenfolge herunterladen.

    // Retrieve a reference to a blob named "myblob.txt"
    CloudBlockBlob blockBlob2 = container.GetBlockBlobReference("myblob.txt");

    string text;
    using (var memoryStream = new MemoryStream())
    {
        blockBlob2.DownloadToStream(memoryStream);
        text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
    }

## <a name="delete-blobs"></a>Löschen von blobs

Verwenden Sie zum Löschen eines BLOBs die Methode zum **Löschen** aus.

    // Retrieve reference to a blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete the blob.
    blockBlob.Delete();


## <a name="list-blobs-in-pages-asynchronously"></a>Asynchrone Liste von Blobs in Seiten

Wenn Ihr eine große Anzahl von Blobs Angebot oder die Anzahl der Ergebnisse zu steuern, die Sie in einem Eintrag Vorgang zurückkehren möchten, können Sie Blobs in Seiten Ergebnisse auflisten. Im folgenden Beispiel wird veranschaulicht, wie asynchrone zurückzugebenden Ergebnisse in Seiten, sodass die Ausführung nicht blockiert, während Sie warten, um eine große Menge von Ergebnissen zurückzukehren.

Dieses Beispiel zeigt einen flachen Blob auflisten, aber Sie können auch eine hierarchische Liste, indem Sie den Parameter **UseFlatBlobListing** der Methode **ListBlobsSegmentedAsync** auf **falsch**durchführen.

Da die Methode für die Stichprobe eine asynchrone Methode ruft, muss es mit dem Schlüsselwort **asynchrone** vorangestellt werden, und müssen ein **Task** -Objekt zurück. Das Await-Schlüsselwort für das **ListBlobsSegmentedAsync** angegebene ausgesetzt Ausführung der Stichprobe Methode, bis der Eintrag Vorgang abgeschlossen ist.

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

## <a name="next-steps"></a>Nächste Schritte

[AZURE.INCLUDE [vs-storage-dotnet-blobs-next-steps](../../includes/vs-storage-dotnet-blobs-next-steps.md)]

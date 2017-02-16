<properties 
    pageTitle="Hochladen von Dateien in ein Media-Dienste-Konto mithilfe von .NET | Microsoft Azure" 
    description="Informationen Sie zum Media-Dienste Medieninhalten verschaffen, indem Sie erstellen und Anlagen hochladen." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/19/2016" 
    ms.author="juliako"/>



# <a name="upload-files-into-a-media-services-account-using-net"></a>Hochladen von Dateien in einer mit .NET Media-Dienste-Konto

 > [AZURE.SELECTOR]
 - [.NET](media-services-dotnet-upload-files.md)
 - [REST](media-services-rest-upload-files.md)
 - [Portal](media-services-portal-upload-files.md)

Media-Dienste Sie hochladen (oder Aufnahme) Ihre digitalen Dateien in einer Anlage. Die **Anlage** Entität kann Video, Audio, Bilder, Miniaturansichten Sammlungen, Text Spuren und Untertitel Dateien (und die Metadaten für diese Dateien.) enthalten.  Sobald die Dateien hochgeladen werden, werden Ihre Inhalte in der Cloud für die weitere Verarbeitung und streaming sicher gespeichert.

Die Dateien in der Anlage werden als **Anlage Dateien**bezeichnet. Die Instanz **AssetFile** und die ist-Media-Datei sind zwei verschiedene Objekte. Die Instanz AssetFile enthält Metadaten über die Media-Datei, während die Media-Datei der tatsächlichen Medieninhalten enthält.

>[AZURE.NOTE]Folgendes gilt beim Auswählen eines Namens für die Anlage auf:
>
>- Media-Dienste verwendet den Wert der Eigenschaft IAssetFile.Name beim Erstellen von URLs für das streaming von Inhalten (z. B. http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters). Aus diesem Grund ist die prozentkodierung nicht zulässig. Der Wert der **Name** -Eigenschaft kann nicht die folgenden [Zeichen %-Codierung-reserviert](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters)haben: !*'();:@&=+$,/?%#[]". Darüber hinaus gibt es nur kann eine '.' für die Dateinamenerweiterung.
>
>- Die Länge des Namens sollte nicht größer als 260 Zeichen lang sein.

Wenn Sie Posten erstellen, können Sie die folgenden Verschlüsselungsoptionen angeben. 

- **Keine** - keine Verschlüsselung verwendet wird. Dies ist der Standardwert. Beachten Sie, dass bei Verwendung dieser Option der Inhalt nicht übertragen werden oder bei Rest im Speicher geschützt ist.
Wenn Sie beabsichtigen, eine MP4 vorführen mit schrittweisen herunterladen, verwenden Sie diese Option. 
- **CommonEncryption** - verwenden Sie diese Option, wenn Sie Inhalte hochladen, die bereits verschlüsselt und mit allgemeinen Verschlüsselung oder PlayReady DRM (z. B. interpolierten Streaming geschützt mit PlayReady DRM) geschützt.
- **EnvelopeEncrypted** – verwenden Sie diese Option, wenn Sie mit AES verschlüsselt HLS hochladen. Beachten Sie, dass müssen die Dateien codierte und Transformieren Manager verschlüsselt wurden.
- **StorageEncrypted** - verschlüsselt löschen Inhalten mit lokal statisch AES-256-bit-Verschlüsselung und dann Uploads, die verschlüsselt er Azure-Speicher, in dem sie gespeichert ist. Posten mit Speicher Verschlüsselung geschützt werden automatisch entschlüsselt in eine verschlüsselte Dateisystem vor Codierung platziert und optional vor dem Hochladen wieder als eine neue Ausgabe Anlage erneut verschlüsselt. Die primäre Anwendungsfall-für die Verschlüsselung der Speicher ist, wenn Sie von hoher Qualität von Mediendateien mit Verschlüsselung statisch sind auf dem Datenträger sichern möchten.

    Media-Dienste ermöglicht die Verschlüsselung der auf dem Datenträger Speicherplatz für Ihre Anlagen, die nicht über das Netzwerk wie Digital Rights Management (DRM).

    Falls Ihre Anlage Speicher verschlüsselt ist, müssen Sie die Anlage Übermittlung Richtlinie konfigurieren. Weitere Informationen finden Sie unter [Konfigurieren von Anlage Übermittlung Richtlinie](media-services-dotnet-configure-asset-delivery-policy.md).

Wenn Sie für Ihre Anlage mit einer Option **CommonEncrypted** oder eine Option **EnvelopeEncypted** verschlüsselt werden angeben, müssen Sie Ihre Anlage mit einer **ContentKey**verknüpfen. Weitere Informationen finden Sie unter [So erstellen Sie eine ContentKey](media-services-dotnet-create-contentkey.md). 

Wenn Sie für Ihre Anlage mit der Option **StorageEncrypted** verschlüsselt werden angeben, wird der Media Services SDK für .NET einer **StorateEncrypted** **ContentKey** für Ihre Anlage erstellen.


In diesem Thema wird gezeigt, wie Medien Services .NET SDK ebenso wie Media Services .NET SDK Erweiterungen zum Hochladen von Dateien in einer Anlage Media-Dienste verwenden.

 
## <a name="upload-a-single-file-with-media-services-net-sdk"></a>Hochladen einer einzelnen Datei mit Media Services .NET SDK 

Der folgende Beispielcode verwendet .NET SDK die folgenden Aufgaben ausführen: 

- Erstellt eine leere Anlage.
- Erstellt eine Instanz von AssetFile, die wir die Anlage zuordnen möchten.
- Erstellt eine Instanz von AccessPolicy, die die Berechtigungen und die Dauer der Zugriff auf die Anlage definiert.
- Erstellt eine Instanz von Locator, die Zugriff auf die Anlage enthält.
- Wird eine einzelne Media-Datei in Media Services hochgeladen. 

        
        static public IAsset CreateAssetAndUploadSingleFile(AssetCreationOptions assetCreationOptions, string singleFilePath)
        {
            if (!File.Exists(singleFilePath))
            {
                Console.WriteLine("File does not exist.");
                return null;
            }

            var assetName = Path.GetFileNameWithoutExtension(singleFilePath);
            IAsset inputAsset = _context.Assets.Create(assetName, assetCreationOptions); 

            var assetFile = inputAsset.AssetFiles.Create(Path.GetFileName(singleFilePath));

            Console.WriteLine("Created assetFile {0}", assetFile.Name);

            var policy = _context.AccessPolicies.Create(
                                    assetName,
                                    TimeSpan.FromDays(30),
                                    AccessPermissions.Write | AccessPermissions.List);

            var locator = _context.Locators.CreateLocator(LocatorType.Sas, inputAsset, policy);

            Console.WriteLine("Upload {0}", assetFile.Name);

            assetFile.Upload(singleFilePath);
            Console.WriteLine("Done uploading {0}", assetFile.Name);

            locator.Delete();
            policy.Delete();

            return inputAsset;
        }

##<a name="upload-multiple-files-with-media-services-net-sdk"></a>Mehrere Dateien mit Media Services .NET SDK hochladen 

Im folgende Code veranschaulicht, wie eine Anlage erstellen und mehrere Dateien hochladen.

Der Code führt Folgendes aus:
    
-   Erstellt eine leere Anlage mithilfe der CreateEmptyAsset-Methode, die im vorherigen Schritt definiert.
    
-   Erstellt eine Instanz von **AccessPolicy** , die die Berechtigungen und die Dauer der Zugriff auf die Anlage definiert.
    
-   Erstellt eine Instanz von **Locator** , die Zugriff auf die Anlage enthält.
    
-   Erstellt eine Instanz der **BlobTransferClient** . Dieses Typs stellt einen Client, der für die Azure Blobs ausgeführt wird. In diesem Beispiel verwenden wir den Client zum Überwachen des Fortschritts hochladen aus. 
    
-   Dateien im angegebenen Verzeichnis durchläuft und erstellt eine Instanz der **AssetFile** für jede Datei.
    
-   Wird die Dateien in Media-Dienste mithilfe der Methode **UploadAsync** hochgeladen. 
    
>[AZURE.NOTE] Verwenden Sie die UploadAsync Methode, um sicherzustellen, dass die Anrufe nicht blockiert werden und die Dateien parallel hochgeladen werden.
    
    
        static public IAsset CreateAssetAndUploadMultipleFiles(AssetCreationOptions assetCreationOptions, string folderPath)
        {
            var assetName = "UploadMultipleFiles_" + DateTime.UtcNow.ToString();

            IAsset asset = _context.Assets.Create(assetName, assetCreationOptions);

            var accessPolicy = _context.AccessPolicies.Create(assetName, TimeSpan.FromDays(30),
                                                                AccessPermissions.Write | AccessPermissions.List);

            var locator = _context.Locators.CreateLocator(LocatorType.Sas, asset, accessPolicy);

            var blobTransferClient = new BlobTransferClient();
            blobTransferClient.NumberOfConcurrentTransfers = 20;
            blobTransferClient.ParallelTransferThreadCount = 20;

            blobTransferClient.TransferProgressChanged += blobTransferClient_TransferProgressChanged;

            var filePaths = Directory.EnumerateFiles(folderPath);

            Console.WriteLine("There are {0} files in {1}", filePaths.Count(), folderPath);

            if (!filePaths.Any())
            {
                throw new FileNotFoundException(String.Format("No files in directory, check folderPath: {0}", folderPath));
            }

            var uploadTasks = new List<Task>();
            foreach (var filePath in filePaths)
            {
                var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
                Console.WriteLine("Created assetFile {0}", assetFile.Name);

                // It is recommended to validate AccestFiles before upload. 
                Console.WriteLine("Start uploading of {0}", assetFile.Name);
                uploadTasks.Add(assetFile.UploadAsync(filePath, blobTransferClient, locator, CancellationToken.None));
            }

            Task.WaitAll(uploadTasks.ToArray());
            Console.WriteLine("Done uploading the files");

            blobTransferClient.TransferProgressChanged -= blobTransferClient_TransferProgressChanged;

            locator.Delete();
            accessPolicy.Delete();

            return asset;
        }
    
    static void  blobTransferClient_TransferProgressChanged(object sender, BlobTransferProgressChangedEventArgs e)
    {
        if (e.ProgressPercentage > 4) // Avoid startup jitter, as the upload tasks are added.
        {
            Console.WriteLine("{0}% upload competed for {1}.", e.ProgressPercentage, e.LocalFile);
        }
    }



Wenn eine große Anzahl von Anlagen hochladen, sollten beachten Sie Folgendes.

- Erstellen eines neuen **CloudMediaContext** -Objekts pro Thread an. Die **CloudMediaContext** -Klasse ist nicht Thread-Sicherheit.
 
- NumberOfConcurrentTransfers aus den Standardwert von 2 auf einen höheren Wert wie 5 zu erhöhen. Diese Einstellung wirkt sich auf alle Instanzen von **CloudMediaContext**aus. 
 
- Behalten Sie den Standardwert von 10 ParallelTransferThreadCount ein.
 
##<a name="a-idingestinbulkaingesting-assets-in-bulk-using-media-services-net-sdk"></a><a id="ingest_in_bulk"></a>Posten in Massen mithilfe von Media Services .NET SDK Aufnahme 

Hochladen von Dateien mit großen Anlage kann ein Engpass während der Erstellung von Ressourcen sein. Aufnahme Anlagen in Massen oder Massen "Aufnahme", umfasst die Erstellung von Ressourcen aus dem Uploadprozess Entkopplung. Um eine Aufnahme Ansatz Massen verwenden zu können, erstellen Sie ein Manifest (IngestManifest), die das Objekt und die zugehörigen Dateien beschrieben. Verwenden Sie dann die Upload-Methode Ihrer Wahl, um die zugehörigen Dateien zu des Manifests Blob Container hochladen. Microsoft Azure Media Services überwacht zugeordneten Manifest Blob Container. Nachdem Sie auf den Container Blob eine Datei hochgeladen wird, führt Microsoft Azure Media-Dienste die Erstellung von Ressourcen basierend auf der Konfiguration der Anlage im Manifest (IngestManifestAsset) aus.


Rufen Sie zum Erstellen einer neuen IngestManifest Create-Methode, die von der IngestManifests-Auflistung auf das CloudMediaContext verfügbar gemacht werden. Diese Methode erstellt eine neue IngestManifest mit den Manifestnamen, die Sie bereitstellen.

    IIngestManifest manifest = context.IngestManifests.Create(name);

Erstellen Sie die Anlagen, bei die das gleichzeitige IngestManifest zugeordnet werden soll. Konfigurieren Sie die gewünschte Verschlüsselung-Optionen auf die Anlage für Massen Aufnahme.

    // Create the assets that will be associated with this bulk ingest manifest
    IAsset destAsset1 = _context.Assets.Create(name + "_asset_1", AssetCreationOptions.None);
    IAsset destAsset2 = _context.Assets.Create(name + "_asset_2", AssetCreationOptions.None);

Eine IngestManifestAsset ordnet eine Massen IngestManifest für Massen Aufnahme eine Anlage. Der Befehl verbindet auch die AssetFiles, aus denen jede Anlage besteht. Verwenden Sie zum Erstellen einer IngestManifestAsset die Methode erstellen auf dem Serverkontext aus.

Im folgende Beispiel wird veranschaulicht, Hinzufügen von zwei neue IngestManifestAssets, die die zuvor erstellten an die Massen zwei Anlagen zuordnen Manifest Aufnahme. Jede IngestManifestAsset ordnet auch einen Satz von Dateien, die hochgeladen wird für jedes Objekt während Massen Aufnahme.  

    string filename1 = _singleInputMp4Path;
    string filename2 = _primaryFilePath;
    string filename3 = _singleInputFilePath;
    
    IIngestManifestAsset bulkAsset1 =  manifest.IngestManifestAssets.Create(destAsset1, new[] { filename1 });
    IIngestManifestAsset bulkAsset2 =  manifest.IngestManifestAssets.Create(destAsset2, new[] { filename2, filename3 });
    
Jeder Clientanwendung hochgeschwindigkeit kann die Objektdateien in den BLOB-Speichercontainer zur Verfügung gestellt, indem Sie die Eigenschaft **IIngestManifest.BlobStorageUriForUpload** der IngestManifest URI hochladen können. Eine wichtige hochgeschwindigkeit Upload Dienst ist [Aspera On Demand für Azure-Anwendung](https://datamarket.azure.com/application/2cdbc511-cb12-4715-9871-c7e7fbbb82a6). Sie können auch Code zum Hochladen der Posten-Dateien, wie im folgenden Beispiel dargestellt.
    
    static void UploadBlobFile(string destBlobURI, string filename)
    {
        Task copytask = new Task(() =>
        {
            var storageaccount = new CloudStorageAccount(new StorageCredentials(_storageAccountName, _storageAccountKey), true);
            CloudBlobClient blobClient = storageaccount.CreateCloudBlobClient();
            CloudBlobContainer blobContainer = blobClient.GetContainerReference(destBlobURI);
    
            string[] splitfilename = filename.Split('\\');
            var blob = blobContainer.GetBlockBlobReference(splitfilename[splitfilename.Length - 1]);
    
            using (var stream = System.IO.File.OpenRead(filename))
                blob.UploadFromStream(stream);
    
            lock (consoleWriteLock)
            {
                Console.WriteLine("Upload for {0} completed.", filename);
            }
        });
    
        copytask.Start();
    }

Im folgenden Beispiel wird der Code für das Hochladen der Anlage Dateien das Beispiel in diesem Thema verwendete angezeigt.
    
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename1);
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename2);
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename3);
    

Sie können den Fortschritt der Massen Aufnahme für alle Anlagen einer **IngestManifest** zugeordnet sind, indem Sie die Eigenschaft Statistiken der **IngestManifest**abrufen ermitteln. Um Statusinformationen zu aktualisieren, müssen Sie eine neue **CloudMediaContext** jedes Mal verwenden Sie die Eigenschaft Statistiken Umfrage.

Das folgende Beispiel veranschaulicht eine IngestManifest anhand dessen **Id**abrufen.
    
    static void MonitorBulkManifest(string manifestID)
    {
       bool bContinue = true;
       while (bContinue)
       {
          CloudMediaContext context = GetContext();
          IIngestManifest manifest = context.IngestManifests.Where(m => m.Id == manifestID).FirstOrDefault();
    
          if (manifest != null)
          {
             lock(consoleWriteLock)
             {
                Console.WriteLine("\nWaiting on all file uploads.");
                Console.WriteLine("PendingFilesCount  : {0}", manifest.Statistics.PendingFilesCount);
                Console.WriteLine("FinishedFilesCount : {0}", manifest.Statistics.FinishedFilesCount);
                Console.WriteLine("{0}% complete.\n", (float)manifest.Statistics.FinishedFilesCount / (float)(manifest.Statistics.FinishedFilesCount + manifest.Statistics.PendingFilesCount) * 100);
    
                if (manifest.Statistics.PendingFilesCount == 0)
                {
                   Console.WriteLine("Completed\n");
                   bContinue = false;
                }
             }
    
             if (manifest.Statistics.FinishedFilesCount < manifest.Statistics.PendingFilesCount)
                Thread.Sleep(60000);
          }
          else // Manifest is null
             bContinue = false;
       }
    }
    


##<a name="upload-files-using-net-sdk-extensions"></a>Hochladen von Dateien mit .NET SDK Erweiterungen 

Im folgenden Beispiel wird gezeigt, wie eine einzelne Datei von .NET SDK Erweiterungen hochladen. In diesem Fall die **CreateFromFile** -Methode wird verwendet, aber die asynchrone Version ist ebenfalls verfügbar (**CreateFromFileAsync**). Die **CreateFromFile** -Methode können Sie den Dateinamen, Verschlüsselungsoption und einen Rückruf akzeptieren, um den Fortschritt Hochladen der Datei melden angeben.


    static public IAsset UploadFile(string fileName, AssetCreationOptions options)
    {
        IAsset inputAsset = _context.Assets.CreateFromFile(
            fileName,
            options,
            (af, p) =>
            {
                Console.WriteLine("Uploading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
            });
    
        Console.WriteLine("Asset {0} created.", inputAsset.Id);
    
        return inputAsset;
    }

Im folgende Beispiel UploadFile Funktion und gibt Speicher Verschlüsselung als Anlage Erstellungsoption an.  


    var asset = UploadFile(@"C:\VideoFiles\BigBuckBunny.mp4", AssetCreationOptions.StorageEncrypted);


##<a name="media-services-learning-paths"></a>Media-Dienste Learning Wege

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="next-step"></a>Als Nächstes

Jetzt, da Sie eine Anlage in Media Services hochgeladen haben, fahren Sie mit dem Thema [So erhalten Sie einen Media-Prozessor][] .

[So erhalten Sie einen Media-Prozessor]: media-services-get-media-processor.md
 

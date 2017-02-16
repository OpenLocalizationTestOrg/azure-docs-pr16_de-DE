<properties 
    pageTitle="Herunterladen von Media-Objekten" 
    description="Sie lernen, wie etwa Anlagen auf Ihren Computer herunterzuladen. Codebeispielen in c# geschrieben sind, und verwenden den Media Services SDK für .NET." 
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
    ms.date="09/26/2016"
    ms.author="juliako"/>

#<a name="how-to-deliver-an-asset-by-download"></a>So: vorführen eine Anlage zum Download

In diesem Thema werden die Optionen für die Bereitstellung von Medienobjekten geladen Media-Dienste erläutert. Sie können Medienservices-Inhalten in zahlreichen Anwendungsszenarien vorführen. Sie können Media-Objekten herunterladen oder greifen sie mithilfe eines Locators. Sie können in eine andere Anwendung oder einem anderen Anbieter Medieninhalten senden. Verbesserte Leistung und Skalierbarkeit können Sie auch Inhalt mithilfe eines Content Delivery Network (CDN) vorführen.

Dieses Beispiel zeigt, wie Sie Medienobjekten von Media-Dienste mit Ihrem lokalen Computer herunterladen. Der Code der Einzelvorgänge mit dem Konto Media-Dienste nach Auftrags-ID in Abfragen und greift auf deren **OutputMediaAssets** -Auflistung (d. h. den Satz von mindestens Ausgabe Media-Objekten, der Ausführung eines Auftrags ergibt). In diesem Beispiel wird gezeigt, wie Ausgabe Media-Objekten aus einem Auftrag herunterladen, aber Sie können den gleichen Ansatz zum Herunterladen anderer Vermögenswerte anwenden.

    
    // Download the output asset of the specified job to a local folder.
    static IAsset DownloadAssetToLocal( string jobId, string outputFolder)
    {
        // This method illustrates how to download a single asset. 
        // However, you can iterate through the OutputAssets
        // collection, and download all assets if there are many. 
    
        // Get a reference to the job. 
        IJob job = GetJob(jobId);
    
        // Get a reference to the first output asset. If there were multiple 
        // output media assets you could iterate and handle each one.
        IAsset outputAsset = job.OutputMediaAssets[0];
    
        // Create a SAS locator to download the asset
        IAccessPolicy accessPolicy = _context.AccessPolicies.Create("File Download Policy", TimeSpan.FromDays(30), AccessPermissions.Read);
        ILocator locator = _context.Locators.CreateLocator(LocatorType.Sas, outputAsset, accessPolicy);
    
        BlobTransferClient blobTransfer = new BlobTransferClient
        {
            NumberOfConcurrentTransfers = 20,
            ParallelTransferThreadCount = 20
        };
    
        var downloadTasks = new List<Task>();
        foreach (IAssetFile outputFile in outputAsset.AssetFiles)
        {
            // Use the following event handler to check download progress.
            outputFile.DownloadProgressChanged += DownloadProgress;
    
            string localDownloadPath = Path.Combine(outputFolder, outputFile.Name);
    
            Console.WriteLine("File download path:  " + localDownloadPath);
    
            downloadTasks.Add(outputFile.DownloadAsync(Path.GetFullPath(localDownloadPath), blobTransfer, locator, CancellationToken.None));
    
            outputFile.DownloadProgressChanged -= DownloadProgress;
        }
    
        Task.WaitAll(downloadTasks.ToArray());
    
        return outputAsset;
    }
    
    static void DownloadProgress(object sender, DownloadProgressChangedEventArgs e)
    {
        Console.WriteLine(string.Format("{0} % download progress. ", e.Progress));
    }



##<a name="media-services-learning-paths"></a>Media-Dienste Learning Wege

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

   
##<a name="see-also"></a>Siehe auch 

[Vorführen einer streaming von Inhalten](media-services-deliver-streaming-content.md)


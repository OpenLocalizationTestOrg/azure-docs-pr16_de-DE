<properties 
    pageTitle="Verwalten von Medien Posten über mehrere Speicherkonten hinweg Services | Microsoft Azure" 
    description="Dieser Artikel bieten Ihnen Leitfäden zum Verwalten von Dienstleistungen Medienobjekten über mehrere Speicherkonten hinweg." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
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


#<a name="managing-media-services-assets-across-multiple-storage-accounts"></a>Verwalten von Medien Services Posten über mehrere Speicherkonten hinweg

Beginnend mit Microsoft Azure Media Services 2.2, können Sie mehrere Speicherkonten mit einem einzigen Media-Dienste-Konto anfügen. Möglichkeit zum Anfügen mehrere Speicherkonten ein Konto Media-Dienste bietet folgende Vorteile:

- Lastenausgleich Ihre Bestände jederzeit über mehrere Speicherkonten.
- Anpassungsbereich für Media-Dienste für große Mengen von inhaltsverarbeitung (wie aktuell ein einzelnes Speicherkonto max maximal 500 TB hat). 

In diesem Thema veranschaulicht, wie mehrere Speicherkonten mit einem Media-Dienste-Konto mithilfe von Azure Service Management REST-API anfügen. Es wird gezeigt, wie verschiedene Speicher-Konten Erstellung von Anlagen mit dem Media Services SDK angegeben werden. 

##<a name="considerations"></a>Aspekte

Wenn Sie mehrere Speicherkonten bei Ihrem Konto Media-Dienste anhängen, gelten unter folgenden Aspekten:

- Alle Speicherkonten mit einem Konto Media-Dienste angefügt muss sich in derselben Data Center als das Konto Media-Dienste.
- Aktuell, nachdem Sie ein Speicherkonto das angegebene Media-Dienste-Konto zugeordnet ist, können sie getrennt werden.
- Primäre Speicher-Konto wird während der Erstellungszeit für Media-Dienste-Konto angegeben. Sie können derzeit nicht Speicher Standardkonto ändern. 

Andere Aspekte:

Media-Dienste verwendet den Wert der Eigenschaft **IAssetFile.Name** beim Erstellen von URLs für das streaming von Inhalten (z. B. http://{WAMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters). Aus diesem Grund ist die prozentkodierung nicht zulässig. Der Wert der Name-Eigenschaft kann nicht die folgenden [Zeichen %-Codierung-reserviert](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters)haben: !*'();:@&=+$,/?%#[]". Darüber hinaus gibt es nur kann eine "." für die Dateinamenerweiterung.

##<a name="to-attach-a-storage-account-with-azure-service-management-rest-api"></a>Anfügen von einem Konto Speicher mit Azure Service Management REST-API

Aktuell, besteht die einzige Möglichkeit zum Anfügen mehrere Speicherkonten mithilfe von [Azure Service Management REST-API](http://msdn.microsoft.com/library/azure/dn167014.aspx)ein. Im Beispiel in die [So: verwenden Media Services Management REST-API](https://msdn.microsoft.com/library/azure/dn167656.aspx) Thema definiert die **AttachStorageAccountToMediaServiceAccount** -Methode, die ein Speicherkonto an der angegebenen Medienservices-Kontos anfügt. Der Code in demselben Thema definiert die **ListStorageAccountDetails** -Methode, die alle an der angegebenen Medienservices-Kontos angefügten Speicherkonten aufgelistet sind.


##<a name="to-manage-media-services-assets-across-multiple-storage-accounts"></a>Zum Verwalten von Anlagen Media-Dienste über mehrere Speicher-Konten

Mit dem folgende Code verwendet das aktuelle Media Services SDK die folgenden Aufgaben ausführen:

1. Zeigen Sie alle Speicherkonten, die mit dem angegebenen Media-Dienste-Konto verknüpft ist.
1. Rufen Sie den Namen des Standardkontos Speicher ein.
1. Erstellen Sie eine neue Anlage in das Standardkonto-Speicher.
1. Erstellen eines Wirtschaftsguts Ausgabe des Codierung Auftrags in der angegebenen Speicher-Konto an.
    
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System;
        using System.Collections.Generic;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using System.Text;
        using System.Threading;
        using System.Threading.Tasks;
        
        namespace MultipleStorageAccounts
        {
            class Program
            {
                // Location of the media file that you want to encode. 
                private static readonly string _singleInputFilePath =
                    Path.GetFullPath(@"../..\supportFiles\multifile\interview2.wmv");
        
                private static readonly string MediaServicesAccountName = 
                    ConfigurationManager.AppSettings["MediaServicesAccountName"];
                private static readonly string MediaServicesAccountKey = 
                    ConfigurationManager.AppSettings["MediaServicesAccountKey"];
        
                private static CloudMediaContext _context;
                private static MediaServicesCredentials _cachedCredentials = null;
    
                static void Main(string[] args)
                {
    
                    // Create and cache the Media Services credentials in a static class variable.
                    _cachedCredentials = new MediaServicesCredentials(
                                    MediaServicesAccountName,
                                    MediaServicesAccountKey);
                    // Used the cached credentials to create CloudMediaContext.
                    _context = new CloudMediaContext(_cachedCredentials);
    
        
                    // Display the storage accounts associated with 
                    // the specified Media Services account:
                    foreach (var sa in _context.StorageAccounts)
                        Console.WriteLine(sa.Name);
        
                    // Retrieve the name of the default storage account.
                    var defaultStorageName = _context.StorageAccounts.Where(s => s.IsDefault == true).FirstOrDefault();
                    Console.WriteLine("Name: {0}", defaultStorageName.Name);
                    Console.WriteLine("IsDefault: {0}", defaultStorageName.IsDefault);
        
                    // Retrieve the name of a storage account that is not the default one.
                    var notDefaultStroageName = _context.StorageAccounts.Where(s => s.IsDefault == false).FirstOrDefault();
                    Console.WriteLine("Name: {0}", notDefaultStroageName.Name);
                    Console.WriteLine("IsDefault: {0}", notDefaultStroageName.IsDefault);
                    
                    // Create the original asset in the default storage account.
                    IAsset asset = CreateAssetAndUploadSingleFile(AssetCreationOptions.None, 
                        defaultStorageName.Name, _singleInputFilePath);
                    Console.WriteLine("Created the asset in the {0} storage account", asset.StorageAccountName);
                    
                    // Create an output asset of the encoding job in the other storage account.
                    IAsset outputAsset = CreateEncodingJob(asset, notDefaultStroageName.Name, _singleInputFilePath);
                    if(outputAsset!=null)
                        Console.WriteLine("Created the output asset in the {0} storage account", outputAsset.StorageAccountName);
        
                }
        
                static public IAsset CreateAssetAndUploadSingleFile(AssetCreationOptions assetCreationOptions, string storageName, string singleFilePath)
                {
                    var assetName = "UploadSingleFile_" + DateTime.UtcNow.ToString();
                    
                    // If you are creating an asset in the default storage account, you can omit the StorageName parameter.
                    var asset = _context.Assets.Create(assetName, storageName, assetCreationOptions);
        
                    var fileName = Path.GetFileName(singleFilePath);
        
                    var assetFile = asset.AssetFiles.Create(fileName);
        
                    Console.WriteLine("Created assetFile {0}", assetFile.Name);
        
                    assetFile.Upload(singleFilePath);
                    
                    Console.WriteLine("Done uploading {0}", assetFile.Name);
        
                    return asset;
                }
        
                static IAsset CreateEncodingJob(IAsset asset, string storageName, string inputMediaFilePath)
                {
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My encoding job");
                    // Get a media processor reference, and pass to it the name of the 
                    // processor to use for the specific task.
                    IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
        
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My encoding task",
                        processor,
                        "H264 Multiple Bitrate 720p",
                        Microsoft.WindowsAzure.MediaServices.Client.TaskOptions.ProtectedConfiguration);
        
                    // Specify the input asset to be encoded.
                    task.InputAssets.Add(asset);
                    // Add an output asset to contain the results of the job. 
                    // This output is specified as AssetCreationOptions.None, which 
                    // means the output asset is not encrypted. 
                    task.OutputAssets.AddNew("Output asset", storageName,
                        AssetCreationOptions.None);
        
                    // Use the following event handler to check job progress.  
                    job.StateChanged += new
                            EventHandler<JobStateChangedEventArgs>(StateChanged);
        
                    // Launch the job.
                    job.Submit();
        
                    // Check job execution and wait for job to finish. 
                    Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);
                    progressJobTask.Wait();
        
                    // Get an updated job reference.
                    job = GetJob(job.Id);
        
                    // If job state is Error the event handling 
                    // method for job progress should log errors.  Here we check 
                    // for error state and exit if needed.
                    if (job.State == JobState.Error)
                    {
                        Console.WriteLine("\nExiting method due to job error.");
                        return null;
                    }
        
                    // Get a reference to the output asset from the job.
                    IAsset outputAsset = job.OutputMediaAssets[0];
        
                    return outputAsset;
                }
        
        
                private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
                {
                    var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
                        ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();
        
                    if (processor == null)
                        throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));
        
                    return processor;
                }
        
                private static void StateChanged(object sender, JobStateChangedEventArgs e)
                {
                    Console.WriteLine("Job state changed event:");
                    Console.WriteLine("  Previous state: " + e.PreviousState);
                    Console.WriteLine("  Current state: " + e.CurrentState);
        
                    switch (e.CurrentState)
                    {
                        case JobState.Finished:
                            Console.WriteLine();
                            Console.WriteLine("********************");
                            Console.WriteLine("Job is finished.");
                            Console.WriteLine("Please wait while local tasks or downloads complete...");
                            Console.WriteLine("********************");
                            Console.WriteLine();
                            Console.WriteLine();
                            break;
                        case JobState.Canceling:
                        case JobState.Queued:
                        case JobState.Scheduled:
                        case JobState.Processing:
                            Console.WriteLine("Please wait...\n");
                            break;
                        case JobState.Canceled:
                        case JobState.Error:
                            // Cast sender as a job.
                            IJob job = (IJob)sender;
                            // Display or log error details as needed.
                            Console.WriteLine("An error occurred in {0}", job.Id);
                            break;
                        default:
                            break;
                    }
                }
        
                static IJob GetJob(string jobId)
                {
                    // Use a Linq select query to get an updated 
                    // reference by Id. 
                    var jobInstance =
                        from j in _context.Jobs
                        where j.Id == jobId
                        select j;
                    // Return the job reference as an Ijob. 
                    IJob job = jobInstance.FirstOrDefault();
        
                    return job;
                }
            }
        }
 

##<a name="media-services-learning-paths"></a>Media-Dienste Learning Wege

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

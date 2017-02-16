<properties 
    pageTitle="Erweiterte Codierung mit Media Encoder Premium Workflow | Microsoft Azure" 
    description="Erfahren Sie, wie mit Media Encoder Premium Workflow codieren. Codebeispielen in c# geschrieben sind, und verwenden den Media Services SDK für .NET." 
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

#<a name="advanced-encoding-with-media-encoder-premium-workflow"></a>Erweiterte Codierung mit Media Encoder Premium Workflow

>[AZURE.NOTE] In diesem Thema erläuterten Media Encoder Premium Workflow Media-Prozessor ist nicht verfügbar in China.

Premium Encoder Fragen per e-Mail an Mepd unter Microsoft.com.

##<a name="overview"></a>(Übersicht)

Microsoft Azure Media-Dienste ist die **Media Encoder Premium Workflow** Medienprozessor Einführung in. Dieser Prozessor Angebote Advance-Codierung Funktionen für Ihre Premium bei Bedarf Workflows. 

Gliedern Sie Details im Zusammenhang mit **Media Encoder Premium Workflow**in den folgenden Themen: 

- [Formate unterstützt vom Workflow Media Encoder Premium](media-services-premium-workflow-encoder-formats.md) – Discusses die Dateiformate und Codecs unterstützt von **Media Encoder Premium Workflow**.

- Im Abschnitt [Encoder vergleichen](media-services-encode-asset.md#compare_encoders) vergleicht die codieren Funktionen von **Media Encoder Premium Workflow** und **Media Encoder Standard**.

In diesem Thema veranschaulicht, wie mit **Media Encoder Premium Workflow** mit .NET codieren.

Codierung Aufgaben für die **Media Encoder Premium Workflows** erfordern eine separate Konfigurationsdatei, als Workflow-Datei bezeichnet. Diese Dateien haben die Erweiterung .workflow und mithilfe des Tools für die [Workflow-Designer](media-services-workflow-designer.md) erstellt werden.

##<a name="encode"></a>Codieren

Codierung Aufgaben für die **Media Encoder Premium Workflows** erfordern eine separate Konfigurationsdatei, als Workflow-Datei bezeichnet. Diese Dateien haben die Erweiterung .workflow und mithilfe des Tools für die [Workflow-Designer](media-services-workflow-designer.md) erstellt werden.


Sie erhalten auch die Standardeinstellung Workflowdateien [hier](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows). Der Ordner enthält auch die Beschreibung der diese Dateien.

Die Workflowdateien auf Ihr Konto Media-Dienste als Anlage hochgeladen werden müssen, und diese Anlage die Codierung Vorgang übergeben werden sollte.

Im folgenden Beispiel wird veranschaulicht, wie mit **Media Encoder Premium Workflow**codieren. 

Die folgenden Schritte ausgeführt werden: 
 
1. Erstellen eines Wirtschaftsguts und Hochladen einer Datei Workflow. 
2. Erstellen eines Wirtschaftsguts und eine Quelle Media-Datei hochladen.
3. Abrufen des Medien-Prozessors "Media Encoder Premium Workflow".
4. Erstellen Sie ein Projekt und eine Aufgabe ein. 

    In den meisten Fällen Konfigurationszeichenfolge für den Vorgang ist leer (wie im folgenden Beispiel). Es gibt einige erweiterten Szenarien (die Sie zur Laufzeiteigenschaften dynamisch festlegen erforderlich) in diesem Fall Sie eine XML-Zeichenfolge, die dem Vorgang Codierung bereitstellen möchten. Beispiele für solche Szenarien sind: Erstellen einer Überlagerung, parallele oder sequenzielle Medien zusammenfügen, Untertitelung.
5. Fügen Sie zwei von Anlagen, die dem Vorgang an.
    
    ein. 1st – die Workflow-Anlage.

    b. 2nd – video Anlage.
    
    **Hinweis**: die Workflow-Anlage muss die Aufgabe aus, bevor die Medien Anlage hinzugefügt werden. Die Konfigurationszeichenfolge für diese Aufgabe sollte leer sein. 

6. Senden Sie den Auftrag codieren.

Im folgenden finden ein vollständiges Beispiel. Informationen zum Einrichten von mit Media Services .NET Development finden Sie unter [Media-Dienste Entwicklung mit .NET](media-services-dotnet-how-to-use.md).


    using System; 
    using System.Linq;
    using System.Configuration;
    using System.IO;
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using System.Collections.Generic;
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.MediaServices.Client;
    
    namespace MediaEncoderPremiumWorkflowSample
    {
        class Program
        {
            // Read values from the App.config file.
            private static readonly string _mediaServicesAccountName =
                ConfigurationManager.AppSettings["MediaServicesAccountName"];
            private static readonly string _mediaServicesAccountKey =
                ConfigurationManager.AppSettings["MediaServicesAccountKey"];
    
            // Field for service context.
            private static CloudMediaContext _context = null;
            private static MediaServicesCredentials _cachedCredentials = null;
    
            private static readonly string _supportFiles =
                Path.GetFullPath(@"../..\Media");
    
            private static readonly string _workflowFilePath =
                Path.GetFullPath(_supportFiles + @"\H264 Progressive Download MP4.workflow");
            
            private static readonly string _singleMP4InputFilePath =
                Path.GetFullPath(_supportFiles + @"\BigBuckBunny.mp4");
    
    
            static void Main(string[] args)
            {
                // Create and cache the Media Services credentials in a static class variable.
                _cachedCredentials = new MediaServicesCredentials(
                                _mediaServicesAccountName,
                                _mediaServicesAccountKey);
    
                // Used the cached credentials to create CloudMediaContext.
                _context = new CloudMediaContext(_cachedCredentials);
    
                var workflowAsset = CreateAssetAndUploadSingleFile(_workflowFilePath);
                var videoAsset = CreateAssetAndUploadSingleFile(_singleMP4InputFilePath);
                IAsset outputAsset = CreateEncodingJob(workflowAsset, videoAsset); 
    
            }
    
            static public IAsset CreateAssetAndUploadSingleFile(string singleFilePath)
            {
                var assetName = "UploadSingleFile_" + DateTime.UtcNow.ToString();
                var asset = _context.Assets.Create(assetName, AssetCreationOptions.None);
    
                var fileName = Path.GetFileName(singleFilePath);
    
                var assetFile = asset.AssetFiles.Create(fileName);
    
                Console.WriteLine("Created assetFile {0}", assetFile.Name);
    
                var accessPolicy = _context.AccessPolicies.Create(assetName, TimeSpan.FromDays(30),
                                                                    AccessPermissions.Write | AccessPermissions.List);
    
                var locator = _context.Locators.CreateLocator(LocatorType.Sas, asset, accessPolicy);
    
                Console.WriteLine("Upload {0}", assetFile.Name);
    
                assetFile.Upload(singleFilePath);
                Console.WriteLine("Done uploading {0}", assetFile.Name);
    
                locator.Delete();
                accessPolicy.Delete();
    
                return asset;
            }
    
            static public IAsset CreateEncodingJob(IAsset workflow, IAsset video)
            {
                // Declare a new job.
                IJob job = _context.Jobs.Create("Premium Workflow encoding job");
                // Get a media processor reference, and pass to it the name of the 
                // processor to use for the specific task.
                IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Premium Workflow");
    
                // Create a task with the encoding details, using a string preset.
                ITask task = job.Tasks.AddNew("Premium Workflow encoding task",
                    processor,
                    "",
                    TaskOptions.None);
    
                // Specify the input asset to be encoded.
                task.InputAssets.Add(workflow);
                task.InputAssets.Add(video); // we add one asset
                // Add an output asset to contain the results of the job. 
                // This output is specified as AssetCreationOptions.None, which 
                // means the output asset is not encrypted. 
                task.OutputAssets.AddNew("Output asset",
                    AssetCreationOptions.None);
    
                // Use the following event handler to check job progress.  
                job.StateChanged += new
                        EventHandler<JobStateChangedEventArgs>(StateChanged);
    
                // Launch the job.
                job.Submit();
    
                // Check job execution and wait for job to finish. 
                Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);
                progressJobTask.Wait();
    
                // If job state is Error the event handling 
                // method for job progress should log errors.  Here we check 
                // for error state and exit if needed.
                if (job.State == JobState.Error)
                {
                    throw new Exception("\nExiting method due to job error.");
                }
    
                return job.OutputMediaAssets[0];
            }
    
            static private void StateChanged(object sender, JobStateChangedEventArgs e)
            {
                Console.WriteLine("Job state changed event:");
                Console.WriteLine("  Previous state: " + e.PreviousState);
                Console.WriteLine("  Current state: " + e.CurrentState);
    
                switch (e.CurrentState)
                {
                    case JobState.Finished:
                        Console.WriteLine();
                        Console.WriteLine("Job is finished.");
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
                        LogJobStop(job.Id);
                        break;
                    default:
                        break;
                }
            }
    
            static private void LogJobStop(string jobId)
            {
                StringBuilder builder = new StringBuilder();
                IJob job = _context.Jobs.Where(j => j.Id == jobId).FirstOrDefault();
    
                builder.AppendLine("\nThe job stopped due to cancellation or an error.");
                builder.AppendLine("***************************");
                builder.AppendLine("Job ID: " + job.Id);
                builder.AppendLine("Job Name: " + job.Name);
                builder.AppendLine("Job State: " + job.State.ToString());
                builder.AppendLine("Job started (server UTC time): " + job.StartTime.ToString());
                builder.AppendLine("Media Services account name: " + _mediaServicesAccountName);
                // Log job errors if they exist.  
                if (job.State == JobState.Error)
                {
                    builder.Append("Error Details: \n");
                    foreach (ITask task in job.Tasks)
                    {
                        foreach (ErrorDetail detail in task.ErrorDetails)
                        {
                            builder.AppendLine("  Task Id: " + task.Id);
                            builder.AppendLine("    Error Code: " + detail.Code);
                            builder.AppendLine("    Error Message: " + detail.Message + "\n");
                        }
                    }
                }
                builder.AppendLine("***************************\n");
    
                Console.Write(builder.ToString());
            }
    
    
            static private IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
            {
                var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
                    ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();
    
                if (processor == null)
                    throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));
    
                return processor;
            }
        }
    }


Premium Encoder Fragen per e-Mail an Mepd unter Microsoft.com.

##<a name="media-services-learning-paths"></a>Media-Dienste Learning Wege

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

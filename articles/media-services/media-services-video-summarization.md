<properties
    pageTitle="Video Azure Media-Miniaturansichten verwenden, um ein Video Zusammenfassung erstellen | Microsoft Azure"
    description="Video Zusammenfassung helfen Ihnen die Zusammenfassung der langen Videos erstellen, indem Sie interessante Codeausschnitte automatisch aus dem Quellvideo auswählen. Dies ist nützlich, wenn Sie einen schnellen Überblick über was Sie erwartet in einem langen Video bereitstellen möchten."
    services="media-services"
    documentationCenter=""
    authors="juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/26/2016"   
    ms.author="milanga;juliako;"/>

#<a name="use-azure-media-video-thumbnails-to-create-a-video-summarization"></a>Verwenden Sie Video Azure Media-Miniaturansichten, um ein Video Zusammenfassung erstellen
##<a name="overview"></a>(Übersicht)

Der **Azure Medien Video Miniaturansichten** Medienprozessor (MP) können Sie eine Zusammenfassung eines Videos zu erstellen, die für Kunden eignet, die nur eine Zusammenfassung der ein langes Video Vorschau anzeigen möchten. Beispielsweise sollten Kunden auf einen kurzen "Zusammenfassung video" finden Sie unter Wenn diese zeigen Sie auf eine Miniaturansicht. Die MPS leistungsstarke Screenshot Erkennung und Verkettung Technologie können Sie durch die Anpassung der Parameter **Azure Media Video Miniaturansichten** durch eine Konfiguration Voreinstellung und eine beschreibende Clipkopie über einen Algorithmus generieren.  

Die **Azure Medien Video-Miniaturansicht** MP gibt es zurzeit in der Vorschau.

In diesem Thema bietet Details **Azure Medien Video-Miniaturansicht** und gezeigt, wie sie mit Media Services SDK für .NET

##<a name="video-summary-example"></a>Beispiel für Video Zusammenfassung 

Hier sind einige Beispiele für der Azure Medien Video Miniaturansichten Medienprozessor Möglichkeiten:

###<a name="original-video"></a>Ursprüngliche video

[Ursprüngliche video](http://ampdemo.azureedge.net/azuremediaplayer.html?url=https%3A%2F%2Fnimbuscdn-nimbuspm.streaming.mediaservices.windows.net%2Faed33834-ec2d-4788-88b5-a4505b3d032c%2FMicrosoft%27s%20HoloLens%20Live%20Demonstration.ism%2Fmanifest)

###<a name="video-thumbnail-result"></a>Video-Miniaturansicht Ergebnis

[Video-Miniaturansicht Ergebnis](http://ampdemo.azureedge.net/azuremediaplayer.html?url=http%3A%2F%2Fnimbuscdn-nimbuspm.streaming.mediaservices.windows.net%2Ff5c91052-4232-41d4-b531-062e07b6a9ae%2FHololens%2520Demo_VideoThumbnails_MotionThumbnail.mp4)

##<a name="task-configuration-preset"></a>Task-Konfiguration (Voreinstellung)

Wenn einen video Miniaturansichten Vorgang mit **Azure Medien Video Miniaturansichten**zu erstellen, müssen Sie eine Voreinstellung Konfiguration angeben. Miniaturansicht oben gezeigten Beispiel wurde mit der folgenden grundlegenden JSON-Konfiguration erstellt:

    {"version":"1.0"}

Aktuell, können Sie die folgenden Parameter ändern:

Parameter|Beschreibung
---|---
outputAudio|Gibt an, ob das resultierende Video Audioelemente enthält. <br/>Sind die Werte zulässig: True oder False. Standardmäßig ist wahr.
fadeInFadeOut|Gibt an, Verblassen, unabhängig davon, ob ein Übergang zwischen den Miniaturansichten der separaten Bewegung verwendet werden.  <br/>Sind die Werte zulässig: True oder False.  Standardmäßig ist wahr.
maxMotionThumbnailDurationInSecs|Ganze Zahl, die angibt, wie lange das gesamte resultierende Video betragen.  Standardwert hängt ursprünglichen Videodauer aus.


Die folgende Tabelle beschreibt die standardmäßige Dauer, wenn **MaxMotionThumbnailInSecs** nicht verwendet wird.

||||
---|---|---|---|---
Videodauer|d < 3 min|3 min < d < 15 Min.|15 min < d < 30 Minuten| 30 Minuten < d
Miniaturansicht Dauer|15 sec (Szenen 2 und 3)| 30 sec (Szenen 3 bis 5)|60 sec (Szenen 5 bis 10)|90 Sekunden (Szenen 10 bis 15)


Die folgende JSON legt verfügbaren Parameter.
    
    {
        "version": "1.0",
        "options": {
            "outputAudio": "true",
            "maxMotionThumbnailDurationInSecs": "10",
            "fadeInFadeOut": "true"
        }
    }

## <a name="sample-code"></a>Beispiel-code

Das folgende Programm wird so:

1. Erstellen eines Wirtschaftsguts und Hochladen einer Media-Datei in die Anlage.
1. Erstellt ein Projekt mit einem video Miniaturansichten Vorgang basierend auf einer Konfigurationsdatei, die die folgenden Json-Voreinstellung enthält. 
        
        {               
            "version": "1.0",
            "options": {
                "outputAudio": "true",
                "maxMotionThumbnailDurationInSecs": "30",
                "fadeInFadeOut": "false"
            }
        }

1. Die Ausgabedateien herunter. 

###<a name="net-code"></a>.NET code
     
    using System;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Threading;
    using System.Threading.Tasks;
    
    namespace VideoSummarization
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
    
            static void Main(string[] args)
            {
    
                // Create and cache the Media Services credentials in a static class variable.
                _cachedCredentials = new MediaServicesCredentials(
                                _mediaServicesAccountName,
                                _mediaServicesAccountKey);
                // Used the cached credentials to create CloudMediaContext.
                _context = new CloudMediaContext(_cachedCredentials);
    

                // Run the thumbnail job.
                var asset = RunVideoThumbnailJob(@"C:\supportFiles\VideoThumbnail\BigBuckBunny.mp4",
                                            @"C:\supportFiles\VideoThumbnail\config.json");
    
                // Download the job output asset.
                DownloadAsset(asset, @"C:\supportFiles\VideoThumbnail\Output");
            }
    
            static IAsset RunVideoThumbnailJob(string inputMediaFilePath, string configurationFile)
            {
                // Create an asset and upload the input media file to storage.
                IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                    "My Video Thumbnail Input Asset",
                    AssetCreationOptions.None);
    
                // Declare a new job.
                IJob job = _context.Jobs.Create("My Video Thumbnail Job");
    
                // Get a reference to Azure Media Video Thumbnails.
                string MediaProcessorName = "Azure Media Video Thumbnails";
    
                var processor = GetLatestMediaProcessorByName(MediaProcessorName);
    
                // Read configuration from the specified file.
                string configuration = File.ReadAllText(configurationFile);
    
                // Create a task with the encoding details, using a string preset.
                ITask task = job.Tasks.AddNew("My Video Thumbnail Task",
                    processor,
                    configuration,
                    TaskOptions.None);
    
                // Specify the input asset.
                task.InputAssets.Add(asset);
    
                // Add an output asset to contain the results of the job.
                task.OutputAssets.AddNew("My Video Thumbnail Output Asset", AssetCreationOptions.None);
    
                // Use the following event handler to check job progress.  
                job.StateChanged += new EventHandler<JobStateChangedEventArgs>(StateChanged);
    
                // Launch the job.
                job.Submit();
    
                // Check job execution and wait for job to finish.
                Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);
    
                progressJobTask.Wait();
    
                // If job state is Error, the event handling
                // method for job progress should log errors.  Here we check
                // for error state and exit if needed.
                if (job.State == JobState.Error)
                {
                    ErrorDetail error = job.Tasks.First().ErrorDetails.First();
                    Console.WriteLine(string.Format("Error: {0}. {1}",
                                                    error.Code,
                                                    error.Message));
                    return null;
                }
    
                return job.OutputMediaAssets[0];
            }
    
            static IAsset CreateAssetAndUploadSingleFile(string filePath, string assetName, AssetCreationOptions options)
            {
                IAsset asset = _context.Assets.Create(assetName, options);
    
                var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
                assetFile.Upload(filePath);
    
                return asset;
            }
    
            static void DownloadAsset(IAsset asset, string outputDirectory)
            {
                foreach (IAssetFile file in asset.AssetFiles)
                {
                    file.Download(Path.Combine(outputDirectory, file.Name));
                }
            }
    
            static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
            {
                var processor = _context.MediaProcessors
                    .Where(p => p.Name == mediaProcessorName)
                    .ToList()
                    .OrderBy(p => new Version(p.Version))
                    .LastOrDefault();
    
                if (processor == null)
                    throw new ArgumentException(string.Format("Unknown media processor",
                                                               mediaProcessorName));
    
                return processor;
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
                        // LogJobStop(job.Id);
                        break;
                    default:
                        break;
                }
            }
    
        }
    }

###<a name="video-thumbnail-output"></a>Video-Miniaturansicht Ausgabe

[Video-Miniaturansicht Ausgabe](http://ampdemo.azureedge.net/azuremediaplayer.html?url=http%3A%2F%2Fnimbuscdn-nimbuspm.streaming.mediaservices.windows.net%2Fd06f24dc-bc81-488e-a8d0-348b7dc41b56%2FHololens%2520Demo_VideoThumbnails_MotionThumbnail.mp4)

##<a name="media-services-learning-paths"></a>Media-Dienste Learning Wege

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="related-links"></a>Links zu verwandten Themen

[Azure Media Services Analytics (Übersicht)](media-services-analytics-overview.md)

[Azure Medien Analytics demos](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)
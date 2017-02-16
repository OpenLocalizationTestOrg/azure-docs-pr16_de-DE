<properties
    pageTitle="Bedrucken Schwärzen mit Azure Media Analytics | Microsoft Azure"
    description="In diesem Thema veranschaulicht Flächen mit Azure Media Analytics Schwärzen."
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
    ms.date="09/12/2016"   
    ms.author="juliako;"/>
 
#<a name="face-redaction-with-azure-media-analytics"></a>Smiley Schwärzen mit Azure Media analytics

##<a name="overview"></a>(Übersicht)

**Azure Medien Redactor** ist eine [Azure Medien Analytics](media-services-analytics-overview.md) Medien-Prozessor (MP), der skalierbare Smiley Schwärzen in der Cloud bietet. Smiley Schwärzen ermöglicht es Ihnen, das Video zu ändern, und Flächen des ausgewählten Personen verwischen. Sie möchten den Smiley Schwärzen-Dienst in öffentlichen Sicherheit und News Medien Szenarien verwenden. Können einige Minuten lang, die mehrere Flächen enthält Stunden manuell Schwärzen dauern, aber mit diesem Dienst wird der Smiley Schwärzen Prozess wenige einfache Schritten erforderlich. Weitere Informationen finden Sie in [diesem](https://azure.microsoft.com/blog/azure-media-redactor/) Blog.

Dieses Thema bietet Details **Azure Medien Redactor** und zeigt, wie sie mit Media Services SDK für .NET.

**Azure Medien Redactor** MP gibt es zurzeit in der Vorschau.

## <a name="face-redaction-modes"></a>Smiley Schwärzen Modi

Gesichtsausdruck Schwärzen funktioniert, wenn Sie Flächen in jeder Frame des Videos erkennen und Nachverfolgen von das Smiley Objekt beide vorwärts und rückwärts Zeitpunkt, damit die gleiche Person aus anderen Winkel ebenfalls verzerrt werden kann. Die automatisierte Schwärzen erfolgt sehr komplexe und werden nicht immer Naturprodukt (100 %) der gewünschten Ausgabe, daher Medien Analytics bietet Ihnen verschiedene Möglichkeiten, um die endgültige Ausgabe ändern.

Zusätzlich zu einem vollständig automatischen Modus ist es ein zwei-Pass-Workflow, der der Auswahl/Deserialisieren-selection der gefundenen Flächen über eine Liste von IDs kann. Darüber hinaus verwendet um beliebige pro Frame Anpassungen der VA machen eine Datei mit Metadaten im JSON-Format. Diesen Workflow wird folgendermaßen unterteilt **Analysieren** und **Redact** Modi. Sie können die beiden Modi in einer einzigen Übergabe kombinieren, die beide Aufgaben in einem Projekt ausgeführt wird; In diesem Modus heißt **kombiniert**.

###<a name="combined-mode"></a>Kombinierte Modus

Dies wird automatisch eine geschwärzten mp4 ohne alle Handbücher Eingabemethoden führen.

Phase|Dateiname|Notizen
---|---|---
Einer Anlage|foo.Bar|Video im WMV, MOV oder MP4-format
Eingabemethoden config|Voreingestellte Auftrag-Konfiguration|{'Version':'1.0 ', 'Optionen': {'Modus': 'kombiniert'}}
Die Ausgabe Anlage|foo_redacted.mp4|Video mit Weichzeichnen angewendet

####<a name="input-example"></a>Eingabe Beispiel:

[In diesem Video anzeigen](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fed99001d-72ee-4f91-9fc0-cd530d0adbbc%2FDancing.mp4)

####<a name="output-example"></a>Ausgabebeispiel:

[In diesem Video anzeigen](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc6608001-e5da-429b-9ec8-d69d8f3bfc79%2Fdance_redacted.mp4)

###<a name="analyze-mode"></a>Analysieren Sie Modus

Der **Analysieren** Lauf des Workflows zwei-Pass übernimmt eine video Eingabe und erzeugt eine JSON-Datei der Smiley Speicherorte und Jpg-Bilder für jede Fläche erkannt.

Phase|Dateiname|Notizen
---|---|----
Einer Anlage|foo.Bar|Video im WMV, MPV oder MP4-format
Eingabemethoden config|Voreingestellte Auftrag-Konfiguration|{'Version':'1.0 ', 'Optionen': {'Modus': 'analysieren'}}
Die Ausgabe Anlage|foo_annotations.JSON|Anmerkungsdaten der Smiley Speicherorte im JSON-Format. Dies kann vom Benutzer zum Ändern der Weichzeichnen umgebenden Felder bearbeitet werden. Siehe Beispiel unten an.
Die Ausgabe Anlage|foo_thumb%06d.jpg [foo_thumb000001.jpg, foo_thumb000002.jpg]|Eine zugeschnittene Jpg der einzelnen erkannt Smiley, wo gibt die Anzahl der LabelId der Fläche an

####<a name="output-example"></a>Ausgabebeispiel:

    {
      "version": 1,
      "timescale": 50,
      "offset": 0,
      "framerate": 25.0,
      "width": 1280,
      "height": 720,
      "fragments": [
        {
          "start": 0,
          "duration": 2,
          "interval": 2,
          "events": [
            [  
              {
                "id": 1,
                "x": 0.306415737,
                "y": 0.03199235,
                "width": 0.15357475,
                "height": 0.322126418
              },
              {
                "id": 2,
                "x": 0.5625317,
                "y": 0.0868245438,
                "width": 0.149155334,
                "height": 0.355517566
              }
            ]
          ]
        },

… abgeschnitten


###<a name="redact-mode"></a>Schwärzen Modus

Beim zweiten Durchgang des Workflows hat eine größere Anzahl von Eingaben, die in einer einzelnen Anlage kombiniert werden müssen.

Dies umfasst eine Liste der IDs Weichzeichnen, das ursprüngliche Video und die Anmerkungen JSON. In diesem Modus wird mit der Anmerkungen auf die Eingabe video Weichzeichnen angewendet.

Die Ausgabe der analysieren Durchgang ist das ursprüngliche Video nicht enthalten. Das Video muss in einer Anlage für den Vorgang des Redact Modus hochgeladen werden und wie die primäre Datei ausgewählt werden.

Phase|Dateiname|Notizen
---|---|---
Einer Anlage|foo.Bar|Video im WMV, MPV oder MP4-Format. Dasselbe video wie in Schritt 1.
Einer Anlage|foo_annotations.JSON|Anmerkungen Metadaten-Datei aus, die eine Phase mit optional Änderungen.
Einer Anlage|foo_IDList.txt (Optional)|Optional neue Zeile kommagetrennte Liste der Smiley Schwärzen-IDs. Wenn Sie nichts zeichnet dies alle Flächen aus.
Eingabemethoden config|Voreingestellte Auftrag-Konfiguration|{'Version':'1.0 ', 'Optionen': {'Modus': 'Schwärzen'}}
Die Ausgabe Anlage|foo_redacted.mp4|Video mit Weichzeichnen angewendet ausgehend von Anmerkungen

####<a name="example-output"></a>Beispiel für die Ausgabe

Dies ist die Ausgabe einer Zonen eine ID, die ausgewählt werden.

[In diesem Video anzeigen](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fad6e24a2-4f9c-46ee-9fa7-bf05e20d19ac%2Fdance_redacted1.mp4)

##<a name="attribute-descriptions"></a>Attribut Beschreibungen

Die VA Schwärzen bietet hohe Genauigkeit Smiley Speicherort Erkennung und nachverfolgen, die in einer Videoframe bis zu 64 personenbezogenen Flächen erkennen kann. Frontale Flächen bieten die besten Ergebnisse erzielen Sie, während der seitlichen Flächen und kleine Flächen (kleiner oder gleich 24 x 24 Pixel) eine Herausforderung sind.

Die Flächen erkannten und nachverfolgten werden mit Koordinaten zurück, der angibt, des Speicherorts der Flächen sowie eine Smiley-ID-Zahl, die angibt, den Verlauf dieser Person als Ausführenden zurückgegeben. Smiley-ID-Nummern sind zu unter Umständen zurücksetzen, wenn die Frontale Smiley verloren gegangen oder überlappende in den Rahmen, was einige Personen erste mehrere IDs zugewiesen.

Ausführliche erläuterungen für die Attribute finden Sie unter [Smiley erkennen und Emotionen mit Azure Medien Analytics](media-services-face-and-emotion-detection.md) Thema.

## <a name="sample-code"></a>Beispiel-code

Das folgende Programm wird so:

1. Erstellen eines Wirtschaftsguts und Hochladen einer Media-Datei in die Anlage.
1. Erstellen eines Auftrags mit Smiley Schwärzen Vorgangs an, basierend auf einer Konfigurationsdatei, die die folgenden Json-Voreinstellung enthält. 
                    
        {'version':'1.0', 'options': {'mode':'combined'}}

1. Die Ausgabe JSON-Dateien herunterladen. 
         
        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Threading.Tasks;
        
        namespace FaceRedaction
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
        
                    // Run the FaceRedaction job.
                    var asset = RunFaceRedactionJob(@"C:\supportFiles\FaceRedaction\SomeFootage.mp4",
                                                @"C:\supportFiles\FaceRedaction\config.json");
        
                    // Download the job output asset.
                    DownloadAsset(asset, @"C:\supportFiles\FaceRedaction\Output");
                }
        
                static IAsset RunFaceRedactionJob(string inputMediaFilePath, string configurationFile)
                {
                    // Create an asset and upload the input media file to storage.
                    IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                        "My Face Redaction Input Asset",
                        AssetCreationOptions.None);
        
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My Face Redaction Job");
        
                    // Get a reference to Azure Media Redactor.
                    string MediaProcessorName = "Azure Media Redactor";
        
                    var processor = GetLatestMediaProcessorByName(MediaProcessorName);
        
                    // Read configuration from the specified file.
                    string configuration = File.ReadAllText(configurationFile);
        
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My Face Redaction Task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input asset.
                    task.InputAssets.Add(asset);
        
                    // Add an output asset to contain the results of the job.
                    task.OutputAssets.AddNew("My Face Redaction Output Asset", AssetCreationOptions.None);
        
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


##<a name="next-step"></a>Als Nächstes

Überprüfen Sie die Pfade learning Media-Dienste.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="related-links"></a>Links zu verwandten Themen

[Azure Media Services Analytics (Übersicht)](media-services-analytics-overview.md)

[Azure Medien Analytics demos](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

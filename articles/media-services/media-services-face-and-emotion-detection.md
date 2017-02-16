<properties
    pageTitle="Erkennen Smiley und Emotionen mit Azure Media Analytics | Microsoft Azure"
    description="In diesem Thema veranschaulicht, wie Flächen und Emotionen mit Azure Medien Analytics erkennen."
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
 
#<a name="detect-face-and-emotion-with-azure-media-analytics"></a>Fläche und Emotionen mit Azure Media Analytics erkennen

##<a name="overview"></a>(Übersicht)

Der **Azure Medien Smiley Erkennung** Media-Prozessor (MP) ermöglicht es Ihnen zu zählen, abgestimmt nachverfolgen und sogar die Teilnahme an der Zielgruppe und Reaktion über Gesichtsausdrücke-Zeitachse. Dieser Dienst enthält zwei Funktionen: 

- **Smiley Erkennung**

    Smiley Erkennung findet und verfolgt personenbezogenen Flächen innerhalb eines Videos. Mehrere Flächen erkannt werden können und anschließend nachverfolgt werden, wie sie versehen, mit der Zeit und Ort Metadaten zurückgegeben, die in einer JSON-Datei verschoben. Während der Verlauf wird versucht, eine konsistente ID zur gleichen Fläche erteilen, während die Person, die auf dem Bildschirm bewegen wird auch wenn sie verdeckt werden, oder lassen Sie den Rahmen kurz.

    >[AZURE.NOTE]Diese Services führt keine Gesichtsausdruck Spracherkennung. Eine Person, die bewirkt, dass den Rahmen oder wird für verdeckt zu lang versehen wird eine neue ID sobald wieder.

- **Emotionen Erkennung**
    
    Emotionen Erkennung ist eine optionale Komponente von der Smiley Erkennung Medienprozessor, die Analyse auf mehrere emotionale Attribute aus den Flächen erkannt, einschließlich Glück, Sadness, Angst, Anger und zurückgibt. 

**Azure Medien Smiley Erkennung** MP gibt es zurzeit in der Vorschau.

Dieses Thema bietet Details **Azure Medien Smiley Erkennung** und gezeigt, wie sie mit Media Services SDK für .NET.

##<a name="face-detector-input-files"></a>Bedrucken Erkennung von Dateien

Videodateien. Derzeit mit die folgenden Formaten werden unterstützt: MP4, MOV und WMV.

##<a name="face-detector-output-files"></a>Bedrucken Erkennung Ausgabedateien

Die Smiley Erkennung und Verlauf-API bietet hohe Genauigkeit Smiley Speicherort Erkennung und Verlauf, die in einem Video bis zu 64 personenbezogenen Flächen erkennen kann. Frontale Flächen bieten die besten Ergebnisse erzielen Sie, während der seitlichen Flächen und kleine Flächen (kleiner oder gleich 24 x 24 Pixel) möglicherweise nicht so genau.

Die Flächen erkannten und nachverfolgten werden mit Koordinaten (links, oben, Breite und Höhe) zurückgegeben, die die Position der Flächen im das Bild in Pixel als auch einen Smiley-ID-Zahl, die im Verlauf dieser Person als ausführenden angibt. Smiley-ID-Nummern sind zu unter Umständen zurücksetzen, wenn die Frontale Smiley verloren gegangen oder überlappende in den Rahmen, was einige Personen erste mehrere IDs zugewiesen.

###<a name="a-idoutputelementsaelements-of-the-output-json-file"></a><a id="output_elements"></a>Elemente der Ausgabe JSON-Datei

Für das Smiley Erkennung und Vorgang nachverfolgen enthält das Ergebnis der Ausgabe die Metadaten aus den Flächen innerhalb der angegebenen Datei im JSON-Format.

Das Smiley Erkennung und Nachverfolgen von JSON umfasst die folgenden Attribute:

Element|Beschreibung
---|---
Version|Dies bezieht sich auf die Version der Video-API.
Zeitskala|"Teilstriche" pro Sekunde des Videos.
Bereich.verschieben|Dies ist der Zeitoffset für Zeitstempel. In Video-APIs Version 1.0, wird dies immer 0 sein. Szenarien, die wir unterstützen, kann in Zukunft diesen Wert geändert werden.
Bildrate|Bilder pro Sekunde des Videos.
Satzteile|Die Metadaten wird von in anderen Segmente, so genannte Satzteile aufgeteilter. Jedes Fragment enthält eine starten, Dauer, Anzahl Intervall und Ereignisse an.
Starten|Die Startzeit des ersten Ereignisses in 'Teilstriche'.
Dauer|Die Länge der das Fragment, in "Teilstriche".
Intervall|Das Zeitintervall für jeden Ereigniseintrag im Fragment, in "Teilstriche".
Ereignisse|Jedes Ereignis enthält die Flächen erkannt und in die Zeitdauer erfasst. Es ist ein Array von Array von Ereignissen. Die äußere Matrix stellt ein Zeitintervall dar. Das innere Array besteht aus 0 oder mehr Ereignisse, die zu diesem Zeitpunkt passiert ist. Eine leere Klammer [] bedeutet, dass keine Flächen nicht erkannt wurden.
ID| Die ID der Fläche, die verfolgt wird. Diese Anzahl kann versehentlich geändert werden, wenn eine Fläche nicht erkannt wird. Eine bestimmte Einzelperson sollte die gleiche ID in der gesamten Video haben, aber dies kann nicht mit Sicherheit aufgrund Einschränkungen der Erkennungsalgorithmus (verschiedenen usw.)
X, Y|Der oberen linken Ecke X und Y-Koordinaten der Fläche umgebenden Felds in einem standardisierten Maßstab 0,0 und 1,0. <br/>-X und Y Koordinaten sind relativ zu Querformat immer, wenn Sie eine Hochformat video (oder Kopf, wenn iOS) verfügen, Sie haben die Koordinaten entsprechend vertauschen.
Breite, Höhe|Die Breite und Höhe der Fläche umgebenden Felds in einem standardisierten Maßstab 0,0 und 1,0.
facesDetected|Diese finden Sie am Ende der JSON-Ergebnisse und enthält eine Übersicht über die Anzahl der Flächen, die der Algorithmus während des Videos erkannt. Da die IDs versehentlich zurückgesetzt werden können, wenn eine Fläche nicht erkannt wird (z. B. Smiley nicht mehr leuchtet Bildschirm Looks abwesend), diese Nummer möglicherweise nicht immer true Anzahl der Flächen im Video gleich.

Smiley Erkennung verwendet Techniken Fragmentierung (Stelle, an der die Metadaten kann in Blöcken zeitbasierte aufgeteilt werden und Sie können nur die gewünschten Informationen herunterladen) und Segmentierung (, werden die Ereignisse aufgeteilt für den Fall, dass sie zu groß werden). Einige einfachen Berechnungen können helfen, die Daten zu transformieren. Beispielsweise, wenn ein Ereignis am 6300 (Teilstriche), mit einer Zeitskala von 2997 (Teilstriche/sec) gestartet und Framerate von 29,97 (Rahmen/s), klicken Sie dann:

- Start/Zeitskala = 2.1 Sekunden
- Sekunden x (Framerate/einer Zeitskala) = 63 Frames

Im folgenden finden Sie ein einfaches Beispiel Extrahieren des JSON in einer pro Rahmenformat für Smiley Erkennung und Verlauf:
    
    var faceDetectionResultJsonString = operationResult.ProcessingResult;
    var faceDetecionTracking = 
         JsonConvert.DeserializeObject<FaceDetectionResult>(faceDetectionResultJsonString, settings);


##<a name="face-detection-input-and-output-example"></a>Bedrucken Sie Erkennung Eingabe- und ausgeben Beispiel

###<a name="input-video"></a>Von video

[Eingabemethoden-Video](http://ampdemo.azureedge.net/azuremediaplayer.html?url=https%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc8834d9f-0b49-4b38-bcaf-ece2746f1972%2FMicrosoft%20Convergence%202015%20%20Keynote%20Highlights.ism%2Fmanifest&amp;autoplay=false)

###<a name="task-configuration-preset"></a>Task-Konfiguration (Voreinstellung)

Beim Erstellen einer Aufgabe mit **Azure Medien Smiley Erkennung**müssen Sie eine Voreinstellung Konfiguration angeben. Die folgenden Konfiguration Voreinstellung ist nur für Smiley Erkennung.

    {"version":"1.0"}

###<a name="json-output"></a>JSON-Ausgabe

Im folgenden Beispiel wird der JSON-Ausgabe wurde abgeschnitten.

    {
    "version": 1,
    "timescale": 30000,
    "offset": 0,
    "framerate": 29.97,
    "width": 1280,
    "height": 720,
    "fragments": [
        {
        "start": 0,
        "duration": 60060
        },
        {
        "start": 60060,
        "duration": 60060,
        "interval": 1001,
        "events": [
            [
            {
                "id": 0,
                "x": 0.519531,
                "y": 0.180556,
                "width": 0.0867188,
                "height": 0.154167
            }
            ],
            [
            {
                "id": 0,
                "x": 0.517969,
                "y": 0.181944,
                "width": 0.0867188,
                "height": 0.154167
            }
            ],
            [
            {
                "id": 0,
                "x": 0.517187,
                "y": 0.183333,
                "width": 0.0851562,
                "height": 0.151389
            }
            ],

        . . . 

##<a name="emotion-detection-input-and-output-example"></a>Emotionen Erkennung Eingabe- und Beispiel


###<a name="input-video"></a>Von video

[Eingabemethoden-Video](http://ampdemo.azureedge.net/azuremediaplayer.html?url=https%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc8834d9f-0b49-4b38-bcaf-ece2746f1972%2FMicrosoft%20Convergence%202015%20%20Keynote%20Highlights.ism%2Fmanifest&amp;autoplay=false)

###<a name="task-configuration-preset"></a>Task-Konfiguration (Voreinstellung)

Beim Erstellen einer Aufgabe mit **Azure Medien Smiley Erkennung**müssen Sie eine Voreinstellung Konfiguration angeben. Die folgenden Konfiguration Voreinstellung gibt an, um JSON basierend auf den Emotionen Erkennung zu erstellen.
    
    {
      "version": "1.0",
      "options": {
        "aggregateEmotionWindowMs": "987",
        "mode": "aggregateEmotion",
        "aggregateEmotionIntervalMs": "342"
      }
    }


####<a name="attribute-descriptions"></a>Attribut Beschreibungen

Attributname|Beschreibung
---|---
Modus|Flächen: Konfrontiert nur Erkennung <br/>AggregateEmotion: Rückgabetyp Mittelwert Emotionen Werte für alle Flächen im Rahmen.
AggregateEmotionWindowMs|Verwenden Sie die AggregateEmotion Modus ausgewählt. Gibt die Länge des Videos verwendet jedes Ergebnis Aggregat in Millisekunden an.
AggregateEmotionIntervalMs|Verwenden Sie die AggregateEmotion Modus ausgewählt. Gibt an, mit welcher Häufigkeit zu aggregieren Ergebnissen führen.

####<a name="aggregate-defaults"></a>Aggregieren Standardeinstellungen

Nachfolgend werden Werte für die aggregate-Fenster und Intervall Einstellungen empfohlen. AggregateEmotionWindowMs sollte länger als AggregateEmotionIntervalMs sein.

   |Standardeinstellungen (s)|Max(s)|Min(s)
---|---|---|---
AggregateEmotionWindowMs|0,5|2|0,25
AggregateEmotionIntervalMs|0,5|1|0,25

###<a name="json-output"></a>JSON-Ausgabe

JSON Ausgabe für aggregieren Emotionen (abgeschnitten):
 
    
    {
     "version": 1,
     "timescale": 30000,
     "offset": 0,
     "framerate": 29.97,
     "width": 1280,
     "height": 720,
     "fragments": [
       {
         "start": 0,
         "duration": 60060,
         "interval": 15015,
         "events": [
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ]
         ]
       },
       {
         "start": 60060,
         "duration": 60060,
         "interval": 15015,
         "events": [
           [
             {
               "windowFaceDistribution": {
                 "neutral": 1,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0.688541,
                 "happiness": 0.0586323,
                 "surprise": 0.227184,
                 "sadness": 0.00945675,
                 "anger": 0.00592107,
                 "disgust": 0.00154993,
                 "fear": 0.00450447,
                 "contempt": 0.0042109
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 1,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,


##<a name="limitations"></a>Einschränkungen

- Unterstützte Video-Eingabeformate gehören MP4, MOV und WMV.
- Der Bereich Erkennung Smiley Größe ist 24 x 24 zu 2048 x 2048 Pixel. Die Flächen aus diesem Bereich werden nicht erkannt.
- Für jedes Video ist die maximale Anzahl der Flächen zurückgegeben 64.
- Aufgrund von technischen werden einige Flächen möglicherweise nicht erkannt; z. B. sehr großen Smiley Winkel (Kopf-darstellen), und großen verschiedenen. Frontale und in der Nähe Frontal Flächen haben die besten Ergebnisse erzielen.


## <a name="sample-code"></a>Beispiel-code

Das folgende Programm wird so:

1. Erstellen eines Wirtschaftsguts und Hochladen einer Media-Datei in die Anlage.
1. Erstellt ein Projekt mit Smiley Erkennung Vorgangs an, basierend auf einer Konfigurationsdatei, die die folgenden Json-Voreinstellung enthält. 
                    
        {
            "version": "1.0"
        }

1. Die Ausgabe JSON-Dateien herunter. 
         
        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Threading.Tasks;
        
        namespace FaceDetection
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
        
                    // Run the FaceDetection job.
                    var asset = RunFaceDetectionJob(@"C:\supportFiles\FaceDetection\BigBuckBunny.mp4",
                                                @"C:\supportFiles\FaceDetection\config.json");
        
                    // Download the job output asset.
                    DownloadAsset(asset, @"C:\supportFiles\FaceDetection\Output");
                }
        
                static IAsset RunFaceDetectionJob(string inputMediaFilePath, string configurationFile)
                {
                    // Create an asset and upload the input media file to storage.
                    IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                        "My Face Detection Input Asset",
                        AssetCreationOptions.None);
        
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My Face Detection Job");
        
                    // Get a reference to Azure Media Face Detector.
                    string MediaProcessorName = "Azure Media Face Detector";
        
                    var processor = GetLatestMediaProcessorByName(MediaProcessorName);
        
                    // Read configuration from the specified file.
                    string configuration = File.ReadAllText(configurationFile);
        
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My Face Detection Task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input asset.
                    task.InputAssets.Add(asset);
        
                    // Add an output asset to contain the results of the job.
                    task.OutputAssets.AddNew("My Face Detectoion Output Asset", AssetCreationOptions.None);
        
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


##<a name="media-services-learning-paths"></a>Media-Dienste Learning Wege

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="related-links"></a>Links zu verwandten Themen

[Azure Media Services Analytics (Übersicht)](media-services-analytics-overview.md)

[Azure Medien Analytics demos](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)
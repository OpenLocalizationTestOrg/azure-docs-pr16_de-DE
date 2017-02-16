<properties
    pageTitle="Erkennen von Werkzeugbewegung(en) mit Azure Media Analytics | Microsoft Azure"
    description="Der Azure Medien Bewegungsmelder Medienprozessor (MP) können Sie effizient Abschnitte relevante innerhalb eines Videos andernfalls lange und ergebnislosen ermitteln."
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
    ms.date="10/10/2016"  
    ms.author="milanga;juliako;"/>
 
# <a name="detect-motions-with-azure-media-analytics"></a>Erkennen von Werkzeugbewegung(en) mit Azure Media Analytics

##<a name="overview"></a>(Übersicht)

Der **Azure Medien Bewegungsmelder** Medienprozessor (MP) können Sie effizient Abschnitte relevante innerhalb eines Videos andernfalls lange und ergebnislosen ermitteln. Bewegung Erkennung kann auf statischen Kamera entfernt verwendet werden, um Abschnitte des Videos zu identifizieren, in denen Motion auftritt. Er generiert eine JSON-Datei mit Metadaten, die mit dem Zeitstempel und der Bereich des umgebenden, in dem das Ereignis aufgetreten ist.

Als Ziel in Richtung Sicherheit video-Feeds, kann diese Technologie Bewegung relevante Ereignisse und falsche Interpretation wie Schatten und Beleuchtung Änderungen zugewiesen. So können Sie aus der Kamera Feeds von Sicherheitshinweisen generieren, ohne aufführen mit unbeschränkter irrelevante Ereignisse, während Sie Augenblicke relevante aus sehr lange Überwachung Videos zu extrahieren.

**Azure Medien Bewegungsmelder** MP gibt es zurzeit in der Vorschau.

In diesem Thema bietet Details **Azure Medien Bewegungsmelder** und gezeigt, wie sie mit Media Services SDK für .NET


##<a name="motion-detector-input-files"></a>Bewegung Erkennung von Dateien

Videodateien. Derzeit mit die folgenden Formaten werden unterstützt: MP4, MOV und WMV.

##<a name="task-configuration-preset"></a>Task-Konfiguration (Voreinstellung)

Beim Erstellen einer Aufgabe mit **Azure Medien Bewegungsmelder**müssen Sie eine Voreinstellung Konfiguration angeben. 

###<a name="parameters"></a>Parameter

Sie können die folgenden Parameter verwenden:

Namen|Optionen|Beschreibung|Standard
---|---|---|---
sensitivityLevel|Zeichenfolge: 'Niedrig', 'Mittel', 'high'|Legt die Vertraulichkeit bei der Werkzeugbewegung(en) gemeldet wird. Passen Sie hier, um die Menge der falsche anpassen.|'Mittel'
frameSamplingValue|Positive ganze Zahl|Legt die Häufigkeit bei dem Algorithmus ausgeführt wird. 1 ist gleich jeder Rahmen, 2 bedeutet, dass jeder 2. Rahmen usw..|1
detectLightChange|Boolesche: "true", "false"|Legt fest, ob light Änderungen in den Ergebnissen gemeldet werden|"False"
mergeTimeThreshold|X-Time: Hh: mm:<br/>Beispiel: 00:00:03|Gibt das Zeitfenster zwischen Bewegung Ereignisse, 2 Ereignisse kombiniert und als 1 gemeldet werden.|00:00:00
detectionZones|Ein Array von Erkennung Zonen:<br/>-Erkennung Zone ist ein Array von 3 oder mehr Punkten<br/>-Punkt ist eine x und y Koordinieren von 0 1.|Beschreibt die Liste der Polygon Erkennung Zonen verwendet werden.<br/>Ergebnisse werden als eine ID, wobei das erste Bild 'Id' mit Zonen gemeldet: 0|Einzelne Zone, der den gesamten Rahmen enthält.

###<a name="json-example"></a>JSON-Beispiel

    
    {
      'version': '1.0',
      'options': {
        'sensitivityLevel': 'medium',
        'frameSamplingValue': 1,
        'detectLightChange': 'False',
        "mergeTimeThreshold":
        '00:00:02',
        'detectionZones': [
          [
            {'x': 0, 'y': 0},
            {'x': 0.5, 'y': 0},
            {'x': 0, 'y': 1}
           ],
          [
            {'x': 0.3, 'y': 0.3},
            {'x': 0.55, 'y': 0.3},
            {'x': 0.8, 'y': 0.3},
            {'x': 0.8, 'y': 0.55},
            {'x': 0.8, 'y': 0.8},
            {'x': 0.55, 'y': 0.8},
            {'x': 0.3, 'y': 0.8},
            {'x': 0.3, 'y': 0.55}
          ]
        ]
      }
    }


##<a name="motion-detector-output-files"></a>Bewegung Erkennung Ausgabedateien

Ein Bewegung Erkennung Auftrag zurückgegeben werden eine JSON-Datei in der Ausgabe Anlage kann das beschreibt die Bewegung Benachrichtigungen, und ihre Kategorien, innerhalb des Videos. Die Datei enthält Informationen über die Zeit und die Dauer der Bewegung im Video erkannt.

Der Bewegung Erkennung-API bietet Indikatoren, sobald eine feste Hintergrund video (z. B. eine Überwachung video) in Bewegung Objekte vorhanden sind. Der Bewegungsmelder ist gelernt, falscher Alarme, wie etwa Beleuchtung und Schatten Änderungen zu verringern. Aktuelle Einschränkungen der Algorithmen gehören Nacht Sehschwäche Videos, halb transparentes Objekte und kleine Objekte.

###<a name="a-idoutputelementsaelements-of-the-output-json-file"></a><a id="output_elements"></a>Elemente der Ausgabe JSON-Datei

>[AZURE.NOTE]In der neuesten Version das Ausgabe JSON-Format hat sich geändert und möglicherweise eine Änderung abgeschnitten werden für einige Kunden darstellen.

Die folgende Tabelle beschreibt die Elemente der Ausgabe JSON-Datei.

Element|Beschreibung
---|---
Version|Dies bezieht sich auf die Version der Video-API. Die aktuelle Version ist 2.
Zeitskala|"Teilstriche" pro Sekunde des Videos.
Bereich.verschieben|Der Zeitoffset für Zeitstempel in "Teilstriche". In Video-APIs Version 1.0, wird dies immer 0 sein. Szenarien, die wir unterstützen, kann in Zukunft diesen Wert geändert werden.
Bildrate|Bilder pro Sekunde des Videos.
Breite, Höhe|Verweist auf die Breite und Höhe des Videos in Pixel ein.
Starten|Der Start-Zeitstempel in "Teilstriche".
Dauer|Die Länge des Ereignisses, in "Teilstriche".
Intervall|Das Intervall für jeden Eintrag in der Veranstaltung in "Teilstriche".
Ereignisse|Jedes Ereignis Fragment enthält die Bewegung innerhalb dieser Zeitdauer erkannt.
Typ|Dies ist in der aktuellen Version immer "2" für generische Bewegung. Dieses Etikett bietet Video-APIs die Flexibilität zum Kategorisieren motion in zukünftigen Versionen.
RegionID|Wie bereits dargelegt, wird dies immer 0 in dieser Version sein. Diese Beschriftung kann Video-API flexibel Bewegung in verschiedenen Regionen in zukünftigen Versionen zu suchen.
Regionen|Bezieht sich auf den Bereich im Video, in dem Sie Bewegung interessieren. <br/><br/>-"Id" stellt den Region-Bereich – in dieser Version ist nur eine ID 0. <br/>-"Typ" darstellt, die Form des Bereichs für Bewegung interessieren. Derzeit gibt "Rechteck" und "Polygon" unterstützt.<br/> Wenn Sie "Rechteck" angegeben haben, hat die Region Dimensionen in X, Y, Breite und Höhe. X- und Y-Koordinaten darstellen die linken oberen XY-Koordinaten des Bereichs in einem standardisierten Maßstab 0,0 und 1,0. Die Breite und Höhe wird die Größe des Bereichs in einem standardisierten Maßstab 0,0 und 1,0 dar. In der aktuellen Version sind X, Y, Breite und Höhe immer feste bei 0, 0 und 1, 1. <br/>Wenn Sie "Polygon" angegeben haben, hat die Region Dimensionen in Punkt an. <br/>
Satzteile|Die Metadaten wird von in anderen Segmente, so genannte Satzteile aufgeteilter. Jedes Fragment enthält eine starten, Dauer, Anzahl Intervall und Ereignisse an. Ein Fragment mit keine Ereignisse bedeutet, dass keine Bewegung nicht, während Sie die Startzeit und die Dauer erkannt wurde an.
Klammern]|Jede Klammer stellt ein Intervall in das Ereignis dar. Die leeren Sie Klammern für dieses Intervall bedeutet, dass keine Bewegung nicht erkannt wurde.
Speicherorte|Diese neue Eintrag unter Ereignisse listet die Position, an die Bewegung aufgetreten ist. Dies ist detaillierter als Erkennung Zonen.

So sieht ein Beispiel der JSON-Ausgabe

    {
      "version": 2,
      "timescale": 23976,
      "offset": 0,
      "framerate": 24,
      "width": 1280,
      "height": 720,
      "regions": [
        {
          "id": 0,
          "type": "polygon",
          "points": [{'x': 0, 'y': 0},
            {'x': 0.5, 'y': 0},
            {'x': 0, 'y': 1}]
        }
      ],
      "fragments": [
        {
          "start": 0,
          "duration": 226765
        },
        {
          "start": 226765,
          "duration": 47952,
          "interval": 999,
          "events": [
            [
              {
                "type": 2,
                "typeName": "motion",
                "locations": [
                  {
                    "x": 0.004184,
                    "y": 0.007463,
                    "width": 0.991667,
                    "height": 0.985185
                  }
                ],
                "regionId": 0
              }
            ],
    
    …
##<a name="limitations"></a>Einschränkungen

- Unterstützte Video-Eingabeformate gehören MP4, MOV und WMV.
- Bewegung Erkennung ist für Stillstand Hintergrund Videos optimiert. Der Algorithmus befasst sich mit falscher Alarme, wie etwa Beleuchtung Änderungen und Schatten zu verringern.
- Einige Bewegung aufgrund Herausforderung werden möglicherweise nicht erkannt; z. B. Nacht Sehschwäche Videos, halb transparentes Objekte und kleine Objekte.


## <a name="sample-code"></a>Beispiel-code

Das folgende Programm wird so:

1. Erstellen eines Wirtschaftsguts und Hochladen einer Media-Datei in die Anlage.
1. Erstellt ein Projekt mit video Bewegung Erkennung Vorgangs an, basierend auf einer Konfigurationsdatei, die die folgenden Json-Voreinstellung enthält. 
                    
        {
          'Version': '1.0',
          'Options': {
            'SensitivityLevel': 'medium',
            'FrameSamplingValue': 1,
            'DetectLightChange': 'False',
            "MergeTimeThreshold":
            '00:00:02',
            'DetectionZones': [
              [
                {'x': 0, 'y': 0},
                {'x': 0.5, 'y': 0},
                {'x': 0, 'y': 1}
               ],
              [
                {'x': 0.3, 'y': 0.3},
                {'x': 0.55, 'y': 0.3},
                {'x': 0.8, 'y': 0.3},
                {'x': 0.8, 'y': 0.55},
                {'x': 0.8, 'y': 0.8},
                {'x': 0.55, 'y': 0.8},
                {'x': 0.3, 'y': 0.8},
                {'x': 0.3, 'y': 0.55}
              ]
            ]
          }
        }

1. Die Ausgabe JSON-Dateien herunter. 
         
        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Threading.Tasks;
        
        namespace VideoMotionDetection
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
        
                    // Run the VideoMotionDetection job.
                    var asset = RunVideoMotionDetectionJob(@"C:\supportFiles\VideoMotionDetection\BigBuckBunny.mp4",
                                                @"C:\supportFiles\VideoMotionDetection\config.json");
        
                    // Download the job output asset.
                    DownloadAsset(asset, @"C:\supportFiles\VideoMotionDetection\Output");
                }
        
                static IAsset RunVideoMotionDetectionJob(string inputMediaFilePath, string configurationFile)
                {
                    // Create an asset and upload the input media file to storage.
                    IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                        "My Video Motion Detection Input Asset",
                        AssetCreationOptions.None);
        
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My Video Motion Detection Job");
        
                    // Get a reference to Azure Media Motion Detector.
                    string MediaProcessorName = "Azure Media Motion Detector";
        
                    var processor = GetLatestMediaProcessorByName(MediaProcessorName);
        
                    // Read configuration from the specified file.
                    string configuration = File.ReadAllText(configurationFile);
        
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My Video Motion Detection Task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input asset.
                    task.InputAssets.Add(asset);
        
                    // Add an output asset to contain the results of the job.
                    task.OutputAssets.AddNew("My Video Motion Detectoion Output Asset", AssetCreationOptions.None);
        
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
[Azure Media Services Bewegungsmelder-blog](https://azure.microsoft.com/blog/motion-detector-update/)

[Azure Media Services Analytics (Übersicht)](media-services-analytics-overview.md)

[Azure Medien Analytics demos](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

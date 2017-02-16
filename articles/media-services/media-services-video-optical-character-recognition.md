<properties
    pageTitle="Verwenden von Azure Medien Analytics Textinhalt in Videodateien in digitalen Text konvertiert | Microsoft Azure"
    description="Azure Medien Analytics OCR (optische Zeichen Spracherkennung) ermöglicht es Ihnen, Textinhalt in Videodateien in bearbeitbar, durchsucht digitale Text zu konvertieren.  So können Sie die Extraktion von Metadaten aussagekräftigen aus das Videosignal Ihrer Medien zu automatisieren."
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
    ms.author="juliako"/>
 
#<a name="use-azure-media-analytics-to-convert-text-content-in-video-files-into-digital-text"></a>Verwenden von Azure Medien Analytics Textinhalt in Videodateien in digitalen Text konvertiert 

##<a name="overview"></a>(Übersicht)

Wenn Sie die Videodateien Textinhalte extrahiert und Generieren eines digitalen Texts bearbeitet werden, die durchsucht werden müssen, sollten Sie Azure Medien Analytics OCR (optische Zeichen Spracherkennung) verwenden. Dieser Azure Media-Prozessor erkennt Textinhalt in Sie Videodateien und Textdateien für Ihre Verwendung generiert. OCR ermöglicht Ihnen, die Extraktion von Metadaten aussagekräftigen aus das Videosignal Ihrer Medien zu automatisieren.

Bei Verwendung in Verbindung mit einer Suchmaschine können Sie einfach Ihre Medien nach Text indizieren und verbessern die Auffindbarkeit Teil Ihres Inhalts. Dies ist sehr hilfreich, hochgradig Textform Video, wie etwa ein videoaufzeichnung oder eine Bildschirmaufnahme einer Diashow-Präsentation. Der Azure OCR Medienprozessor ist für digitale Text optimiert.

Der **Azure Medien OCR** Medienprozessor gibt es zurzeit in der Vorschau.

Dieses Thema bietet Details **Azure Medien OCR** und gezeigt, wie sie mit Media Services SDK für .NET. Weitere Informationen und Beispiele finden Sie unter [diesen Blog](https://azure.microsoft.com/blog/announcing-video-ocr-public-preview-new-config/).

##<a name="ocr-input-files"></a>OCR von Dateien

Videodateien. Derzeit mit die folgenden Formaten werden unterstützt: MP4, MOV und WMV.

##<a name="task-configuration"></a>Task-Konfiguration 

Task-Konfiguration (Voreinstellung). Wenn Sie eine Aufgabe mit **Azure Medien OCR**erstellen, müssen Sie eine voreingestellte mit JSON oder XML-Konfiguration angeben. 

###<a name="attribute-descriptions"></a>Attribut Beschreibungen

Attributname|Beschreibung
---|---
Sprache|(optional) werden die Sprache des Texts für die gesucht wird. Eine der folgenden Optionen: automatische Erkennung (Standard), Arabisch, ChineseSimplified, ChineseTraditional, Tschechisch Dänisch, Niederländisch, Englisch, Finnisch, Französisch, Deutsch, Griechisch, Ungarisch, Italienisch, Japanisch, Koreanisch, Norwegisch, Polnisch, Portugiesisch, Rumänisch, Russisch, SerbianCyrillic, SerbianLatin, Slowakisch, Spanisch, Schwedisch, Türkisch.
TextOrientation|(optional) werden die Ausrichtung von Text für die gesucht.  "Links" bedeutet, dass oben auf alle Buchstaben werden in Richtung der linken Seite Arbeitsordner.  Standardtext (wie, die in einem Buch gefunden werden kann) kann "Nach oben" ausgerichteten aufgerufen werden.  Eine der folgenden Optionen: automatische Erkennung (Standard), nach oben, rechts, unten, links.
TimeInterval|(optional) werden die Rate werden.  Standardmäßig wird jede Sekunde 1/2.<br/>JSON-Format – hh: mm:. SSS (Standard 00:00:00.500)<br/>XML-Format – W3C XSD-Dauer Primitiv (Standard PT0.5)
DetectRegions|(optional) Ein Array von DetectRegion Objekte geben die Bereiche in den Videoframe in der Text zu erkennen.<br/>Ein Objekt DetectRegion besteht aus den folgenden vier ganzzahligen Werten:<br/>Links – Pixel aus dem linken Randes<br/>Oben – Pixel vom oberen Seitenrand<br/>Breite – Breite des Bereichs in Pixel<br/>Höhe – Höhe des Bereichs in Pixel

####<a name="json-preset-example"></a>Vordefinierte JSON-Beispiel
        
    {
        'Version':'1.0', 
        'Options': 
        {
        'Language':'English', 
            'TimeInterval':'00:00:01.5',
            'DetectRegions': 
             [
                {'Left':'1','Top':'1','Width':'1','Height':'1'},
                {'Left':'2','Top':'2','Width':'2','Height':'2'}
             ],
            'TextOrientation':'Up'
        }
    }

####<a name="xml-preset-example"></a>Vordefinierte XML-Beispiel

    <?xml version=""1.0"" encoding=""utf-16""?>
    <VideoOcrPreset xmlns:xsi=""http://www.w3.org/2001/XMLSchema-instance"" xmlns:xsd=""http://www.w3.org/2001/XMLSchema"" Version=""1.0"" xmlns=""http://www.windowsazure.com/media/encoding/Preset/2014/03"">
      <Options>
         <Language>English</Language>
         <TimeInterval>PT1.5S</TimeInterval>
         <DetectRegions>
             <DetectRegion>
                   <Left>1</Left>
                   <Top>1</Top>
                   <Width>1</Width>
                   <Height>1</Height>
            </DetectRegion>
       </DetectRegions>
       <TextOrientation>Up</TextOrientation>
      </Options>
    </VideoOcrPreset>

##<a name="ocr-output-files"></a>OCR Ausgabedateien

Die Ausgabe der OCR Medienprozessor ist eine JSON-Datei.

###<a name="elements-of-the-output-json-file"></a>Elemente der Ausgabe JSON-Datei

Die Ausgabe Video OCR bietet Zeit unterteilt Daten an die Zeichen im Video gefunden.  Attributen, wie etwa Sprache oder Ausrichtung können Sie Büro auf genau die Wörter Add-in, die Sie analysieren möchten. 

Die Ausgabe enthält die folgenden Attribute:

Element|Beschreibung
---|---
Zeitskala|"Teilstriche" pro Sekunde des Videos
Bereich.verschieben|Zeit offset für Zeitstempel. In Video-APIs Version 1.0, wird dies immer 0 sein.
Bildrate|Bilder pro Sekunde des Videos
Breite|Breite des Videos in Pixel
Höhe|Höhe des Videos in Pixel
Satzteile|Array von Segmenten zeitbasierte, Videos, in denen die Metadaten aufgeteilter ist
Starten|Startzeit des ein Fragment in "Teilstriche"
Dauer|Länge der ein Fragment in "Teilstriche"
Intervall|Intervall der einzelnen Ereignisse im angegebenen fragment
Ereignisse|Matrix, Bereiche enthält
Region|Wörter oder Ausdrücke erkannt Objekt darstellt.
Sprache|Sprache des Texts in einem Bereich erkannt
Ausrichtung|Ausrichtung des Texts in einem Bereich erkannt
Linien|Array von Textzeilen in einem Bereich erkannt
Text|der eigentliche text

###<a name="json-output-example"></a>Beispiel für JSON-Ausgabe

Im folgenden Ausgabebeispiel enthält das video allgemeine Informationen und mehrere video Satzteile. In jeder video-Fragment enthaltenen jeder Region, die mit der Sprache von OCR MP erkannt wird und die Ausrichtung von Text. Der Bereich enthält auch jeder Zeile Word in diesem Bereich mit den Text der Zeile, Position der Linie und jeder Word Informationen (Word-Inhalt, Position und KONFIDENZ) in dieser Zeile. Im folgenden Beispiel wird, und ich einige Kommentare Inline setzen.

    {
        "version": 1, 
        "timescale": 90000, 
        "offset": 0, 
        "framerate": 30, 
        "width": 640, 
        "height": 480,  // general video information
        "fragments": [
            {
                "start": 0, 
                "duration": 180000, 
                "interval": 90000,  // the time information about this fragment
                "events": [
                    [
                       { 
                            "region": { // the detected region array in this fragment 
                                "language": "English",  // region language
                                "orientation": "Up",  // text orientation
                                "lines": [  // line information array in this region, including the text and the position
                                    {
                                        "text": "One Two", 
                                        "left": 10, 
                                        "top": 10, 
                                        "right": 210, 
                                        "bottom": 110, 
                                        "word": [  // word information array in this line
                                            {
                                                "text": "One", 
                                                "left": 10, 
                                                "top": 10, 
                                                "right": 110, 
                                                "bottom": 110, 
                                                "confidence": 900
                                            }, 
                                            {
                                                "text": "Two", 
                                                "left": 110, 
                                                "top": 10, 
                                                "right": 210, 
                                                "bottom": 110, 
                                                "confidence": 910
                                            }
                                        ]
                                    }
                                ]
                            }
                        }
                    ]
                ]
            }
        ]
    }

## <a name="sample-code"></a>Beispiel-code

Das folgende Programm wird so:

1. Erstellen eines Wirtschaftsguts und Hochladen einer Media-Datei in die Anlage.
1. Erstellt ein Projekt mit einer OCR Konfiguration/Voreinstellung Datei an.
1. Die Ausgabe JSON-Dateien herunter. 
         
        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Threading.Tasks;
        
        namespace OCR
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
        
                    // Run the OCR job.
                    var asset = RunOCRJob(@"C:\supportFiles\OCR\presentation.mp4",
                                                @"C:\supportFiles\OCR\config.json");
        
                    // Download the job output asset.
                    DownloadAsset(asset, @"C:\supportFiles\OCR\Output");
                }
        
                static IAsset RunOCRJob(string inputMediaFilePath, string configurationFile)
                {
                    // Create an asset and upload the input media file to storage.
                    IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                        "My OCR Input Asset",
                        AssetCreationOptions.None);
        
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My OCR Job");
        
                    // Get a reference to Azure Media OCR.
                    string MediaProcessorName = "Azure Media OCR";
        
                    var processor = GetLatestMediaProcessorByName(MediaProcessorName);
        
                    // Read configuration from the specified file.
                    string configuration = File.ReadAllText(configurationFile);
        
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My OCR Task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input asset.
                    task.InputAssets.Add(asset);
        
                    // Add an output asset to contain the results of the job.
                    task.OutputAssets.AddNew("My OCR Output Asset", AssetCreationOptions.None);
        
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

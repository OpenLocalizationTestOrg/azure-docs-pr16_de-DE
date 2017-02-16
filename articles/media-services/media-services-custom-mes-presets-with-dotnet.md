<properties 
    pageTitle="Erweiterte Codierung mit Media Encoder Standard | Microsoft Azure" 
    description="In diesem Thema wird gezeigt, wie erweiterte Codierung durch Media Encoder Standard Vorgang Voreinstellungen anpassen ausführen. Das Thema wird gezeigt, wie Medien Services .NET SDK verwenden, um eine Codierung Vorgangs- und Position zu erstellen. Es wird gezeigt, wie benutzerdefinierte Voreinstellungen, um die Codierung Position angeben." 
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


#<a name="advanced-encoding-with-media-encoder-standard"></a>Erweiterte Codierung mit Media Encoder Standard

##<a name="overview"></a>(Übersicht)

In diesem Thema wird gezeigt, wie mit Media Encoder Standard erweiterte Codierung Aufgaben ausführen. Das Thema veranschaulicht, [wie .NET zum Erstellen einer Codierung Vorgangs- und ein Projekt, das diesen Vorgang ausführt](media-services-custom-mes-presets-with-dotnet.md#encoding_with_dotnet). Es wird gezeigt, wie benutzerdefinierte Voreinstellungen, die dem Vorgang Codierung angeben. Beschreibung der Elemente, die von den Voreinstellungen verwendet werden, finden Sie unter [Dieses Dokument](https://msdn.microsoft.com/library/mt269962.aspx). 

Die benutzerdefinierten Voreinstellungen, die codieren folgenden Aufgaben ausführen, werden veranschaulicht:

- [Generieren von Miniaturansichten](media-services-custom-mes-presets-with-dotnet.md#thumbnails)
- [Kürzen eines Videos (eines Bildschirmausschnitts)](media-services-custom-mes-presets-with-dotnet.md#trim_video)
- [Erstellen einer Überlagerung](media-services-custom-mes-presets-with-dotnet.md#overlay)
- [Einfügen einer automatischen audio nachverfolgen, wenn Eingabe kein Audiosignal enthält](media-services-custom-mes-presets-with-dotnet.md#silent_audio)
- [Deaktivieren der automatischen Deinterlacing](media-services-custom-mes-presets-with-dotnet.md#deinterlacing)
- [Nur Audio-Voreinstellungen](media-services-custom-mes-presets-with-dotnet.md#audio_only)

##<a name="a-idencodingwithdotnetaencoding-with-media-services-net-sdk"></a><a id="encoding_with_dotnet"></a>Mit den Diensten von Medien .NET SDK Codierung

Im folgenden Code wird verwendet Media Services .NET SDK die folgenden Aufgaben ausführen:

- Erstellen einer Codierung an.
- Ein Verweis auf die Media Encoder Standard Encoder abgerufen.
- Laden Sie die benutzerdefinierte XML- oder JSON-Voreinstellung an. Sie können die XML-Datei speichern oder JSON (z. B. [XML-](media-services-custom-mes-presets-with-dotnet.md#xml) oder [JSON](media-services-custom-mes-presets-with-dotnet.md#json) in einer Datei, und verwenden Sie den folgenden Code, die Datei zu laden.

            // Load the XML (or JSON) from the local file.
            string configuration = File.ReadAllText(fileName);  
- Hinzufügen eines codieren Vorgangs dem Projekt. 
- Geben Sie die Eingabewerte Anlage codiert werden.
- Erstellen einer Ausgabe Anlage, die die codierte Anlage enthalten sollen.
- Fügen Sie einen Ereignishandler zum Überprüfen des Fortschritts Position ein.
- Senden Sie den Auftrag.
    
        using System;
        using System.Collections.Generic;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using System.Net;
        using System.Security.Cryptography;
        using System.Text;
        using System.Threading.Tasks;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using Newtonsoft.Json.Linq;
        using System.Threading;
        using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
        using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
        using System.Web;
        using System.Globalization;
        
        namespace CustomizeMESPresests
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
        
                private static readonly string _mediaFiles =
                    Path.GetFullPath(@"../..\Media");
        
                private static readonly string _singleMP4File =
                    Path.Combine(_mediaFiles, @"BigBuckBunny.mp4");
        
                static void Main(string[] args)
                {
                    // Create and cache the Media Services credentials in a static class variable.
                    _cachedCredentials = new MediaServicesCredentials(
                                    _mediaServicesAccountName,
                                    _mediaServicesAccountKey);
                    // Used the chached credentials to create CloudMediaContext.
                    _context = new CloudMediaContext(_cachedCredentials);
        
                    // Get an uploaded asset.
                    var asset = _context.Assets.FirstOrDefault();
        
                    // Encode and generate the output using custom presets.
                    EncodeToAdaptiveBitrateMP4Set(asset);
        
                    Console.ReadLine();
                }
        
                static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset asset)
                {
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("Media Encoder Standard Job");
                    // Get a media processor reference, and pass to it the name of the 
                    // processor to use for the specific task.
                    IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
                
        
                    // Load the XML (or JSON) from the local file.
                    string configuration = File.ReadAllText("CustomPreset_JSON.json");
                
                    // Create a task
                    ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
                        processor,
                        configuration,
                        TaskOptions.None);
                
                    // Specify the input asset to be encoded.
                    task.InputAssets.Add(asset);
                    // Add an output asset to contain the results of the job. 
                    // This output is specified as AssetCreationOptions.None, which 
                    // means the output asset is not encrypted. 
                    task.OutputAssets.AddNew("Output asset",
                        AssetCreationOptions.None);
                
                    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
                    job.Submit();
                    job.GetExecutionProgressTask(CancellationToken.None).Wait();
                
                    return job.OutputMediaAssets[0];
                }
        
                static public IAsset UploadMediaFilesFromFolder(string folderPath)
                {
                    IAsset asset = _context.Assets.CreateFromFolder(folderPath, AssetCreationOptions.None);
        
                    foreach (var af in asset.AssetFiles)
                    {
                        // The following code assumes 
                        // you have an input folder with one MP4 and one overlay image file.
                        if (af.Name.Contains(".mp4"))
                            af.IsPrimary = true;
                        else
                            af.IsPrimary = false;
        
                        af.Update();
                    }
        
                    return asset;
                }
        
        
                static public IAsset EncodeWithOverlay(IAsset assetSource, string customPresetFileName)
                {
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("Media Encoder Standard Job");
                    // Get a media processor reference, and pass to it the name of the 
                    // processor to use for the specific task.
                    IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
        
                    // Load the XML (or JSON) from the local file.
                    string configuration = File.ReadAllText(customPresetFileName);
        
                    // Create a task
                    ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input assets to be encoded.
                    // This asset contains a source file and an overlay file.
                    task.InputAssets.Add(assetSource);
        
                    // Add an output asset to contain the results of the job. 
                    task.OutputAssets.AddNew("Output asset",
                        AssetCreationOptions.None);
        
                    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
                    job.Submit();
                    job.GetExecutionProgressTask(CancellationToken.None).Wait();
        
                    return job.OutputMediaAssets[0];
                }
        

                private static void JobStateChanged(object sender, JobStateChangedEventArgs e)
                {
                    Console.WriteLine("Job state changed event:");
                    Console.WriteLine("  Previous state: " + e.PreviousState);
                    Console.WriteLine("  Current state: " + e.CurrentState);
                    switch (e.CurrentState)
                    {
                        case JobState.Finished:
                            Console.WriteLine();
                            Console.WriteLine("Job is finished. Please wait while local tasks or downloads complete...");
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
                            break;
                        default:
                            break;
                    }
                }
        
        
                private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
                {
                    var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
                    ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();
        
                    if (processor == null)
                        throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));
        
                    return processor;
                }
        
            }
        }


##<a name="a-idthumbnailsagenerate-thumbnails"></a><a id="thumbnails"></a>Generieren von Miniaturansichten

In diesem Abschnitt wird gezeigt, wie eine Voreinstellung anpassen, die Miniaturansichten generiert wird. Die nachstehend definierte Voreinstellung enthält Informationen darüber, wie Sie Ihre Datei sowie die erforderlichen Informationen zum Generieren von Miniaturansichten codieren möchten. Sie können keines der MWS Voreinstellungen dokumentierten [hier](https://msdn.microsoft.com/library/mt269960.aspx) und hinzufügen, Miniaturansichten generiert.  

>[AZURE.NOTE]Die Einstellung **SceneChangeDetection** in den folgenden voreingestellten können nur werden auf true, wenn Sie zu einem einzelnen Bitrate video codieren festgelegt. Wenn Sie zu einem Video Multi-Bitrate Codierung werden und **SceneChangeDetection** true festlegen, wird der Encoder einen Fehler zurück.  


Informationen zum Schema finden Sie unter [in diesem](https://msdn.microsoft.com/library/mt269962.aspx) Thema.

Vergewissern Sie sich im Abschnitt [Aspekte](media-services-custom-mes-presets-with-dotnet.md#considerations) zu überprüfen.

###<a name="a-idjsonajson-preset"></a><a id="json"></a>JSON-Voreinstellung


    {
      "Version": 1.0,
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "SceneChangeDetection": "true",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 4500,
              "MaxBitrate": 4500,
              "BufferWindow": "00:00:05",
              "Width": 1280,
              "Height": 720,
              "ReferenceFrames": 3,
              "EntropyMode": "Cabac",
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
       
            }
          ],
          "Type": "H264Video"
        },
        {
          "JpgLayers": [
            {
              "Quality": 90,
              "Type": "JpgLayer",
              "Width": 640,
              "Height": 360
            }
          ],
          "Start": "{Best}",
          "Type": "JpgImage"
        },
        {
          "PngLayers": [
            {
              "Type": "PngLayer",
              "Width": 640,
              "Height": 360,
            }
          ],
          "Start": "00:00:01",
          "Step": "00:00:10",
          "Range": "00:00:58",
          "Type": "PngImage"
        },
        {
          "BmpLayers": [
            {
              "Type": "BmpLayer",
              "Width": 640,
              "Height": 360
            }
          ],
          "Start": "10%",
          "Step": "10%",
          "Range": "90%",
          "Type": "BmpImage"
        },
        {
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "JpgFormat"
          }
        },
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "PngFormat"
          }
        },
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "BmpFormat"
          }
        },
        {
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }


###<a name="a-idxmlaxml-preset"></a><a id="xml"></a>XML-Voreinstellung


    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Encoding>
        <H264Video>
          <KeyFrameInterval>00:00:02</KeyFrameInterval>
          <SceneChangeDetection>true</SceneChangeDetection>
          <H264Layers>
            <H264Layer>
              <Bitrate>4500</Bitrate>
              <Width>1280</Width>
              <Height>720</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>4500</MaxBitrate>
            </H264Layer>
          </H264Layers>
        </H264Video>
        <AACAudio>
          <Profile>AACLC</Profile>
          <Channels>2</Channels>
          <SamplingRate>48000</SamplingRate>
          <Bitrate>128</Bitrate>
        </AACAudio>
        <JpgImage Start="{Best}">
          <JpgLayers>
            <JpgLayer>
              <Width>640</Width>
              <Height>360</Height>
              <Quality>90</Quality>
            </JpgLayer>
          </JpgLayers>
        </JpgImage>
        <BmpImage Start="10%" Step="10%" Range="90%">
          <BmpLayers>
            <BmpLayer>
              <Width>640</Width>
              <Height>360</Height>
            </BmpLayer>
          </BmpLayers>
        </BmpImage>
        <PngImage Start="00:00:01" Step="00:00:10" Range="00:00:58">
          <PngLayers>
            <PngLayer>
              <Width>640</Width>
              <Height>360</Height>
            </PngLayer>
          </PngLayers>
        </PngImage>
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}_{Width}x{Height}_{VideoBitrate}.mp4">
          <MP4Format />
        </Output>
        <Output FileName="{Basename}_{Index}{Extension}">
          <JpgFormat />
        </Output>
        <Output FileName="{Basename}_{Index}{Extension}">
          <BmpFormat />
        </Output>
        <Output FileName="{Basename}_{Index}{Extension}">
          <PngFormat />
        </Output>
      </Outputs>
    </Preset>

###<a name="considerations"></a>Aspekte

Folgendes gilt:

- Die Verwendung von expliziten Zeitstempel für den Startbereich/Schritt/wird davon ausgegangen, dass die Quelle von mindestens 1 Minute ist.
- JPG/Png/BmpImage Elemente besitzen Start-, Schritt und Bereich Attribute Zeichenfolge – diese als interpretiert werden können:

    - Frame-Nummer, wenn sie nicht Negative ganze Zahlen, z. b werden. "Start": "120"
    - Relativ zur Dauer der Quelldaten Wenn als % Suffix ausgedrückt, z. b. "Start": "15 %"; oder
    - Zeitstempel, wenn als hh: mm: ausgedrückt... Format. Z. b. "Start": "00: 01:00"

    Sie können mischen und Notationen als Sie bitte entsprechen.
    
    Darüber hinaus Start auch eine spezielle Makros unterstützt: {optimale}, die versucht, das erste "interessante" Bild der Inhalt Fuß-oder ENDNOTE zu ermitteln: (Schritt und Bereich werden ignoriert beim Start auf {optimal} festgelegt ist)
    
    - Standardeinstellungen: Starten Sie: {optimale}
- Ausgabeformat explizit für jedes Bildformat bereitgestellt werden muss: Jpg/Png/BmpFormat. Wenn vorhanden ist, entspricht MWS usw. JpgVideo zu JpgFormat. Ausgabeformat wird ein neues Bild-Codec Makro bestimmte eingeführt: {Index}, welche muss werden präsentieren (einmal und nur einmal) für Ausgabeformat Bild.

##<a name="a-idtrimvideoatrim-a-video-clipping"></a><a id="trim_video"></a>Kürzen eines Videos (eines Bildschirmausschnitts)

Informationen zum Ändern der Voreinstellungen Encoder um ClipArt oder das Stelle, an der die Eingabe einer so genannte Mezzanine oder bei Bedarf Datei ist Eingabewerte Video kürzen erörtert. Der Encoder kann auch verwendet werden, um ClipArt oder eine Anlage erfasst oder archiviert aus einem live Stream wird kürzen – die Details für diese stehen in [diesem Blog](https://azure.microsoft.com/blog/sub-clipping-and-live-archive-extraction-with-media-encoder-standard/).

Wenn Sie Ihre Videos zuzuschneiden, können Sie keines der MWS Voreinstellungen dokumentierten [hier](https://msdn.microsoft.com/library/mt269960.aspx) übernehmen und ändern das Element **Quellen** (siehe unten). Der Wert der Startzeit muss die absoluten Zeitstempel des Videos Eingabewerte entsprechen. Beispielsweise, wenn Sie der erste Frame des Videos von einem Zeitstempel der 12:00:10.000 enthält, dann Startzeit sollten mindestens 12:00:10.000 und höher. Im folgenden Beispiel wird davon ausgegangen, dass das Eingabewerte Video einen Anfangs Zeitstempel 0 (null) ist. Beachten Sie, dass **Quellen** am oberen Rand des Schemas platziert werden soll. 
 
###<a name="a-idjsonajson-preset"></a><a id="json"></a>JSON-Voreinstellung
    
    {
      "Version": 1.0,
      "Sources": [
        {
          "StartTime": "00:00:04",
          "Duration": "00:00:16"
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "StretchMode": "AutoSize",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 3400,
              "MaxBitrate": 3400,
              "BufferWindow": "00:00:05",
              "Width": 1280,
              "Height": 720,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 2250,
              "MaxBitrate": 2250,
              "BufferWindow": "00:00:05",
              "Width": 960,
              "Height": 540,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1500,
              "MaxBitrate": 1500,
              "BufferWindow": "00:00:05",
              "Width": 960,
              "Height": 540,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1000,
              "MaxBitrate": 1000,
              "BufferWindow": "00:00:05",
              "Width": 640,
              "Height": 360,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 650,
              "MaxBitrate": 650,
              "BufferWindow": "00:00:05",
              "Width": 640,
              "Height": 360,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 400,
              "MaxBitrate": 400,
              "BufferWindow": "00:00:05",
              "Width": 320,
              "Height": 180,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            }
          ],
          "Type": "H264Video"
        },
        {
          "Profile": "AACLC",
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    } 

###<a name="xml-preset"></a>XML-Voreinstellung
    
Wenn Sie Ihre Videos zuzuschneiden, können Sie keines der MWS Voreinstellungen dokumentierten [hier](https://msdn.microsoft.com/library/mt269960.aspx) übernehmen und ändern das Element **Quellen** (siehe unten).

    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Sources>
        <Source StartTime="PT4S" Duration="PT14S"/>
      </Sources>
      <Encoding>
        <H264Video>
          <KeyFrameInterval>00:00:02</KeyFrameInterval>
          <H264Layers>
            <H264Layer>
              <Bitrate>3400</Bitrate>
              <Width>1280</Width>
              <Height>720</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>3400</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>2250</Bitrate>
              <Width>960</Width>
              <Height>540</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>2250</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>1500</Bitrate>
              <Width>960</Width>
              <Height>540</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>1500</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>1000</Bitrate>
              <Width>640</Width>
              <Height>360</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>1000</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>650</Bitrate>
              <Width>640</Width>
              <Height>360</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>650</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>400</Bitrate>
              <Width>320</Width>
              <Height>180</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>400</MaxBitrate>
            </H264Layer>
          </H264Layers>
        </H264Video>
        <AACAudio>
          <Profile>AACLC</Profile>
          <Channels>2</Channels>
          <SamplingRate>48000</SamplingRate>
          <Bitrate>128</Bitrate>
        </AACAudio>
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}_{Width}x{Height}_{VideoBitrate}.mp4">
          <MP4Format />
        </Output>
      </Outputs>
    </Preset>

##<a name="a-idoverlayacreate-an-overlay"></a><a id="overlay"></a>Erstellen einer Überlagerung

Der Standard Media Encoder können Sie ein Bild auf einer vorhandenen Videos überlagern. Derzeit mit die folgenden Formaten werden unterstützt: Png, Jpg, Gif, Bmp, und. Die nachstehend definierte Voreinstellung ist ein einfaches Beispiel einer Überlagerung video an.

Neben der Definition einer voreingestellten Datei, müssen Sie auch zulassen, dass Media Services wissen, welche Datei in der Anlage das Bild Überlagerung wird und welche Datei die Quelle video auf dem Sie das Bild überlagern möchten. Die Videodatei muss die **primäre** Datei sein. 

Im oben genannten Beispiel für .NET definiert zwei Funktionen: **UploadMediaFilesFromFolder** und **EncodeWithOverlay**. Die Funktion UploadMediaFilesFromFolder uploads von Dateien aus einem Ordner (z. B. BigBuckBunny.mp4 und Image001.png) und legt die mp4-Datei in der primären Datei in der Anlage zu sein. Die **EncodeWithOverlay** -Funktion verwendet, die benutzerdefinierte voreingestellte Datei, die darauf (beispielsweise die Vorgabe, die folgt) übergeben wurde die Codierung Aufgabe zu erstellen. 

>[AZURE.NOTE]Aktuelle Einschränkungen:
>
>Die Einstellung für die Überlagerung Deckkraft wird nicht unterstützt.
>
>Die Quelldatei video einfügen und die Überlagerungsdatei müssen in der gleichen Anlage sein.

###<a name="json-preset"></a>JSON-Voreinstellung
    
    {
      "Version": 1.0,
      "Sources": [
        {
          "Streams": [],
          "Filters": {
            "VideoOverlay": {
              "Position": {
                "X": 100,
                "Y": 100,
                "Width": 100,
                "Height": 50
              },
              "AudioGainLevel": 0.0,
              "MediaParams": [
                {
                  "OverlayLoopCount": 1
                },
                {
                  "IsOverlay": true,
                  "OverlayLoopCount": 1,
                  "InputLoop": true
                }
              ],
              "Source": "Image001.png",
              "Clip": {
                "Duration": "00:00:05"
              },
              "FadeInDuration": {
                "Duration": "00:00:01"
              },
              "FadeOutDuration": {
                "StartTime": "00:00:03",
                "Duration": "00:00:04"
              }
            }
          },
          "Pad": true
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "H264Layers": [
            {
              "Profile": "Baseline",
              "Level": "auto",
              "Bitrate": 1045,
              "MaxBitrate": 1045,
              "BufferWindow": "00:00:05",
              "ReferenceFrames": 3,
              "EntropyMode": "Cavlc",
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "Width": "400",
              "Height": "400",
              "FrameRate": "0/1"
            }
          ],
          "Type": "H264Video"
        },
        {
          "Type": "CopyAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}{Extension}",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }

###<a name="xml-preset"></a>XML-Voreinstellung
    
    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Sources>
        <Source>
          <Streams />
          <Filters>
            <VideoOverlay>
              <Source>Image001.png</Source>
              <Clip Duration="PT5S" />
              <FadeInDuration Duration="PT1S" />
              <FadeOutDuration StartTime="PT3S" Duration="PT4S" />
              <Position X="100" Y="100" Width="100" Height="50" />
              <Opacity>0</Opacity>
              <AudioGainLevel>0</AudioGainLevel>
              <MediaParams>
                <MediaParam>
                  <IsOverlay>false</IsOverlay>
                  <OverlayLoopCount>1</OverlayLoopCount>
                  <InputLoop>false</InputLoop>
                </MediaParam>
                <MediaParam>
                  <IsOverlay>true</IsOverlay>
                  <OverlayLoopCount>1</OverlayLoopCount>
                  <InputLoop>true</InputLoop>
                </MediaParam>
              </MediaParams>
            </VideoOverlay>
          </Filters>
          <Pad>true</Pad>
        </Source>
      </Sources>
      <Encoding>
        <H264Video>
          <KeyFrameInterval>00:00:02</KeyFrameInterval>
          <H264Layers>
            <H264Layer>
              <Bitrate>1045</Bitrate>
              <Width>400</Width>
              <Height>400</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Baseline</Profile>
              <Level>auto</Level>
              <BFrames>0</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cavlc</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>1045</MaxBitrate>
            </H264Layer>
          </H264Layers>
        </H264Video>
        <CopyAudio />
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}{Extension}">
          <MP4Format />
        </Output>
      </Outputs>
    </Preset>

##<a name="a-idsilentaudioainsert-a-silent-audio-track-when-input-has-no-audio"></a><a id="silent_audio"></a>Einfügen einer automatischen audio nachverfolgen, wenn Eingabe kein Audiosignal enthält

Standardmäßig, wenn Sie eine Eingabe an den Encoder senden, die nur Video und kein Audiosignal, enthält wird die Anlage Ausgabe Dateien enthalten, die nur video Daten enthalten. Einige Player möglicherweise nicht verarbeiten diesen Ausgabe Streams. Sie können diese Einstellung verwenden, so erzwingen Sie den Encoder eine automatische audio Nachverfolgen der Ausgabe in diesem Szenario hinzufügen.

Geben Sie den Wert "InsertSilenceIfNoAudio" zum Erzwingen des Encoders, um eine Anlage zu erzeugen, die eine automatische audio nachverfolgen enthält, wenn Eingabe kein Audiosignal enthält.

Sie können keines der MWS Voreinstellungen dokumentierten [hier](https://msdn.microsoft.com/library/mt269960.aspx)übernehmen, und nehmen Sie die folgende Änderung:

###<a name="json-preset"></a>JSON-Voreinstellung

    {
      "Channels": 2,
      "SamplingRate": 44100,
      "Bitrate": 96,
      "Type": "AACAudio",
      "Condition": "InsertSilenceIfNoAudio"
    }

###<a name="xml-preset"></a>XML-Voreinstellung

    <AACAudio Condition="InsertSilenceIfNoAudio">
      <Channels>2</Channels>
      <SamplingRate>44100</SamplingRate>
      <Bitrate>96</Bitrate>
    </AACAudio>

##<a name="a-iddeinterlacingadisable-auto-de-interlacing"></a><a id="deinterlacing"></a>Deaktivieren der automatischen Deinterlacing

Kunden erforderlich tun, wenn sie den Inhalt Interlace automatisch heben verschachtelten werden wie nicht. Bei der automatischen Deinterlacing an (Standard), die MWS unterstützt die automatische Erkennung von verschachtelten Rahmen und interlaces nur Rahmen gekennzeichnet mit Zeilensprung heben.

Sie können Deaktivieren der automatischen Deinterlacing. Diese Option wird nicht empfohlen.

###<a name="json-preset"></a>JSON-Voreinstellung
    
    "Sources": [
    {
     "Filters": {
        "Deinterlace": {
          "Mode": "Off"
        }
      },
    }
    ]

###<a name="xml-preset"></a>XML-Voreinstellung
    
    <Sources>
    <Source>
      <Filters>
        <Deinterlace>
          <Mode>Off</Mode>
        </Deinterlace>
      </Filters>
    </Source>
    </Sources>


##<a name="a-idaudioonlyaaudio-only-presets"></a><a id="audio_only"></a>Nur Audio-Voreinstellungen

In diesem Abschnitt veranschaulicht zwei nur Audio MWS Voreinstellungen: AAC Audio- und AAC gute Qualität Audio.

###<a name="aac-audio"></a>AAC-Audio 

    {
      "Version": 1.0,
      "Codecs": [
        {
          "Profile": "AACLC",
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_AAC_{AudioBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }

###<a name="aac-good-quality-audio"></a>Gute Audioqualität AAC.

    {
      "Version": 1.0,
      "Codecs": [
        {
          "Profile": "AACLC",
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 192,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_AAC_{AudioBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }

##<a name="media-services-learning-paths"></a>Media-Dienste Learning Wege

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Siehe auch 

[Media-Dienste Codierung (Übersicht)](media-services-encode-asset.md)

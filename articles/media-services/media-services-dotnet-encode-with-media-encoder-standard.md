<properties 
    pageTitle="Eine Anlage mit Media Encoder Standard mit .NET codieren | Microsoft Azure" 
    description="In diesem Thema wird gezeigt, wie .NET zu verwenden, um eine Anlage unter Verwendung Media Encoder Strandard codieren." 
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
    ms.author="juliako;anilmur"/>


# <a name="encode-an-asset-with-media-encoder-standard-using-net"></a>Codieren einer Anlageguts mit Media Encoder Standard mit .NET

Codierung Stellen sind eine der am häufigsten verwendeten Verarbeitungsvorgänge in Media-Dienste. Sie erstellen die Codierung Aufträge zum Konvertieren von Mediendateien aus einer Codierung in eine andere. Wenn Sie codieren, können Sie die integrierten Media Encoder von Media-Dienste. Sie können auch einen Encoder bereitgestellt, die von einem Partner Media-Dienste verwenden; Drittanbieter-Encoder sind verfügbar über die Azure Marketplace. 

In diesem Thema wird gezeigt, wie .NET zu verwenden, um Ihre Bestände jederzeit mit Media Encoder Standard (MWS) codieren. Media Encoder Standard ist so konfiguriert, dass mithilfe einer der der Encoder Voreinstellungen beschrieben [hier](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).

Es wird empfohlen, immer Codieren von Mezzanine-Dateien in einer adaptive Bitrate MP4 festlegen, und klicken Sie dann in das gewünschte Format mithilfe der [Dynamischen Verpackung](media-services-dynamic-packaging-overview.md)konvertieren festlegen. Um dynamische Verpackung nutzen zu können, müssen Sie zunächst mindestens eine bei Bedarf streaming Einheit für das streaming Endpunkt abrufen, aus denen Sie bis zur Bereitstellung des Inhalts planen. Weitere Informationen finden Sie unter [So skalieren Media-Dienste](media-services-portal-manage-streaming-endpoints.md).

Ist der Ausgabe Anlage Speicher verschlüsselt, müssen Sie die Anlage Übermittlung Richtlinie konfigurieren. Weitere Informationen finden Sie unter [Konfigurieren von Anlage Übermittlung Richtlinie](media-services-dotnet-configure-asset-delivery-policy.md).

>[AZURE.NOTE]MWS erzeugt eine Ausgabedatei mit einem Namen, der die ersten 32 Zeichen des Namens einer Datei enthält. Der Name basiert auf was in der voreingestellten Datei angegeben ist. Beispielsweise "Dateiname": "{Basename} _ {Index} {Erweiterung}". {Basename} wird durch die ersten 32 Zeichen des Dateinamens Eingabe ersetzt.

###<a name="mes-formats"></a>MWS-Formate

[Formate und codecs](media-services-media-encoder-standard-formats.md)

###<a name="mes-presets"></a>MWS Voreinstellungen

Media Encoder Standard ist so konfiguriert, dass mithilfe einer der der Encoder Voreinstellungen beschrieben [hier](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).

###<a name="input-and-output-metadata"></a>Eingabe- und Metadaten

Wenn Sie eine Eingabe Anlage (oder Posten) mit MWS codieren, erhalten Sie eine Anlage Ausgabe bei der erfolgreichen Abschluss der Aufgabe zu codieren. Die Ausgabe Anlage enthält, Video, Audio, Miniaturansichten, Manifest usw. basierend auf die Codierung Voreinstellung, die Sie verwenden.

Die Ausgabe Anlage enthält auch eine Datei mit Metadaten zur Eingabe Anlage. Der Name der XML-Metadaten-Datei weist das folgende Format: < Asset_id > _metadata.xml (beispielsweise 41114ad3-eb5e - 4c 57-8d 92-5354e2b7d4a4_metadata.xml), wobei < Asset_id > der Posten-ID-Wert der Eingabewerte Ressource ist. Das Schema dieser von XML-Metadaten beschrieben wird [hier](http://msdn.microsoft.com/library/azure/dn783120.aspx).

Die Ausgabe Anlage enthält auch eine Datei mit Metadaten zur Ausgabe Anlage. Der Name der XML-Metadaten-Datei weist das folgende Format: < Source_file_name > _manifest.xml (beispielsweise BigBuckBunny_manifest.xml). Das Schema dieser Ausgabe Metadaten XML beschrieben wird [hier](http://msdn.microsoft.com/library/azure/dn783217.aspx).

Wenn Sie eine der beiden Metadatendateien untersuchen möchten, können Sie einen SAS Locator erstellen und Laden Sie die Datei auf Ihrem lokalen Computer. Sie können ein Beispiel zum Erstellen eines SAS Locators und Herunterladen einer Datei mit Media Services .NET SDK Extensions suchen.

##<a name="download-sample"></a>Beispiel für herunterladen

Abrufen und Ausführen einer Stichprobe aus [hier](https://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/).

##<a name="example"></a>Beispiel

Im folgenden Code wird verwendet Media Services .NET SDK die folgenden Aufgaben ausführen:

- Erstellen einer Codierung an.
- Ein Verweis auf die Media Encoder Standard Encoder abgerufen.
- Gibt an, dass Sie verwenden die "H264 mehrere Bitrate 720p" voreingestellten. Sie können sehen, die bei die Voreinstellungen [hier](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409). Sie können auch das Schema aus, zu dem diese Voreinstellungen entsprechen müssen, überprüfen [hier](https://msdn.microsoft.com/library/mt269962.aspx) Thema.
- Hinzufügen eines einzelnen codieren Vorgangs dem Projekt. 
- Geben Sie die Eingabewerte Anlage codiert werden.
- Erstellen einer Ausgabe Anlage, die die codierte Anlage enthalten sollen.
- Fügen Sie einen Ereignishandler zum Überprüfen des Fortschritts Position ein.
- Senden Sie den Auftrag.
        
        static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset asset)
        {
            // Declare a new job.
            IJob job = _context.Jobs.Create("Media Encoder Standard Job");
            // Get a media processor reference, and pass to it the name of the 
            // processor to use for the specific task.
            IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
        

            // Create a task with the encoding details, using a string preset.
            // In this case "H264 Multiple Bitrate 720p" preset is used.
            ITask task = job.Tasks.AddNew("My encoding task",
                processor,
                "H264 Multiple Bitrate 720p",
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


##<a name="media-services-learning-paths"></a>Media-Dienste Learning Wege

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Siehe auch 

[Wie Miniaturansicht mit Media Encoder Standard und .NET generiert](media-services-dotnet-generate-thumbnail-with-mes.md)
[Medien Services Codierung (Übersicht)](media-services-encode-asset.md)

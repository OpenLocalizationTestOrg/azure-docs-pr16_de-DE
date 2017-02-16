<properties
    pageTitle="Mediendateien mit Azure Media Hyperlapse Hyperlapse | Microsoft Azure"
    description="Azure Medien Hyperlapse erstellt interpolierten Zeit abgelaufen Videos aus dem ersten-Person oder Aktion-Kamera Inhalt. In diesem Thema wird gezeigt, wie Medien Indexer verwendet werden."
    services="media-services"
    documentationCenter=""
    authors="asolanki"
    manager="johndeu"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/19/2016"  
    ms.author="adsolank"/>


# <a name="hyperlapse-media-files-with-azure-media-hyperlapse"></a>Mediendateien mit Azure Media Hyperlapse Hyperlapse

Azure Medien Hyperlapse ist eine Medien Prozessor (MP), die interpolierten Zeit abgelaufen Videos aus dem ersten-Person oder Aktion-Kamera Inhalt erstellt wird.  [Microsoft Research des desktop Hyperlapse Pro](http://aka.ms/hyperlapse)und Hyperlapse Mobile Phone-basierten nebengeordneten cloudbasierten Microsoft Hyperlapse für Azure Media Services werden die Unterschiede den großen Maßstab der Azure Media Services Medien Verarbeitung Plattform zum horizontalen Skalieren und parallelisieren gruppenweise Hyperlapse verarbeiten.

>[AZURE.IMPORTANT]Microsoft Hyperlapse soll am besten auf die erste Person Inhalt mit einer gleitenden Kamera arbeiten.  Obwohl noch-Kamera entfernt noch arbeiten kann, können die Leistung und die Qualität des der Azure Medien Hyperlapse Media-Prozessor für andere Arten von Inhalten garantiert werden.  Schauen Sie sich die [Einführung Blogbeitrag](http://aka.ms/azurehyperlapseblog) aus der public Preview-Version, um weitere Informationen zu Microsoft-Hyperlapse für Azure Media Services und einige Beispiel Videos angezeigt wird.

Ein Azure Medien Hyperlapse Auftrag als nimmt Eingabe eine Bilddatei MP4, MOV oder WMV zusammen mit einer Konfigurationsdatei, die angibt, welche Bilder, Videos Zeit abgelaufen werden sollen und welche Geschwindigkeit (z. B. ersten 10.000 frames am 2 X).  Die Ausgabe ist eine stabilisierte und Zeit abgelaufen Formatvariante der Eingabe video an.

Die neuesten Updates für Azure Medien Hyperlapse finden Sie unter [Blogs Media-Dienste](https://azure.microsoft.com/blog/topics/media-services/).

## <a name="hyperlapse-an-asset"></a>Hyperlapse einer Anlage

Zuerst müssen Sie die gewünschte Standardeingabesprache Datei in Azure Media Services hochladen.  Erfahren Sie mehr über Konzepte hochladen und Verwalten von Inhalten, lesen Sie im [Artikel Content Management](media-services-portal-vod-get-started.md).

###  <a name="a-idconfigurationaconfiguration-preset-for-hyperlapse"></a><a id="configuration"></a>Konfiguration Voreinstellung für Hyperlapse

Sobald die Inhalte in Ihrem Konto Media-Dienste ist, müssen Sie die Konfiguration Voreinstellung zu erstellen.  In der folgenden Tabelle wird erläutert, die benutzerdefinierte Felder:

 Feld | Beschreibung
-------|-------------
StartFrame|Der Rahmen, auf dem die Microsoft Hyperlapse Verarbeitung beginnen soll.
Mit der NumFrames|Die Anzahl der Rahmen Verarbeitungszeit
Geschwindigkeit|Der Faktor, mit denen zum Beschleunigen des Eingabewerte Videos.

Im folgenden finden ein Beispiel für eine Konfigurationsdatei Konformität in XML und JSON:

**XML-Voreinstellung:**

    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
        <Sources>
            <Source StartFrame="0" NumFrames="10000" />
        </Sources>
        <Options>
            <Speed>12</Speed>
        </Options>
    </Preset>

**JSON-Voreinstellung:**

    {
        "Version":1.0,
        "Sources": [
            {
                "StartFrame":0,
                "NumFrames":2147483647
            }
        ],
        "Options": {
            "Speed":1,
            "Stabilize":false
        }
    }

###  <a name="a-idsamplecodea-microsoft-hyperlapse-with-the-ams-net-sdk"></a><a id="sample_code"></a>Microsoft-Hyperlapse mit AMS .NET SDK

Die folgende Methode eine Media-Datei als Anlage hochgeladen und ein Projekt mit dem Azure Medien Hyperlapse Media-Prozessor erstellt.

> [AZURE.NOTE] Sie sollten eine CloudMediaContext bereits im Bereich mit dem Namen "Kontext" für diesen Code entwickelt haben.  Lesen Sie weitere Informationen hierzu finden Sie im [Artikel Content Management](media-services-dotnet-get-started.md)ein.

> [AZURE.NOTE] Das Zeichenfolgenargument "HyperConfig" soll eine Konformität Konfiguration voreingestellte entweder JSON oder XML-wie zuvor beschrieben werden.

statische Bool RunHyperlapseJob (Zeichenfolge, Zeichenfolgenausgabe, Zeichenfolge HyperConfig) {/ / Anlage erstellen, mit der Eingabe Datei IAsset Anlage = Kontext. Posten. CreateAssetAndUploadSingleFile (Eingabe "Meine Hyperlapse Eingabe", AssetCreationOptions.None).

Werfen Sie Instanzen von Azure Medien Hyperlapse MP IMediaProcessor mp = Kontext. MediaProcessors. GetLatestMediaProcessorByName ("Azure Media Hyperlapse");

Auftrag mit Hyperlapse Vorgang IJob Auftrag erstellen = Kontext. Aufträge. Erstellen Sie (String.Format (Eingabe "Hyperlapse {0}"));

Wenn (String.IsNullOrEmpty(hyperConfig)) {/ / Config kann nicht leeren Rückgabetyp falsch;}

HyperConfig = File.ReadAllText(hyperConfig);

ITask HyperlapseTask = Position. Tasks.AddNew ("Hyperlapse Task", mp, HyperConfig, TaskOptions.None) hyperlapseTask.InputAssets.Add(asset); hyperlapseTask.OutputAssets.AddNew ("Hyperlapse Ausgabe", AssetCreationOptions.None);


Position. Ein;

Erstellen des Vorgangsfortschritts Druck- und Abfragen Aufgaben Aufgabe ProgressPrintTask = neue Task(() = > {}

IJob JobQuery = Null; Gehen Sie wie folgt {Var ProgressContext = Kontext; JobQuery = progressContext.Jobs. Wo (j = > j.Id == Position. ID). First(); Console.WriteLine (Zeichenfolge. Format ("\t {1} \t {0} {2}", DateTime.Now, jobQuery.State, jobQuery.Tasks[0]. Bearbeitung)); Thread.Sleep(10000); } während (jobQuery.State! = JobState.Finished & & jobQuery.State! = JobState.Error & & jobQuery.State! = JobState.Canceled); }); progressPrintTask.Start();

            Task progressJobTask = job.GetExecutionProgressTask(
                                                 CancellationToken.None);
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
                return false;                  
            }

        DownloadAsset(job.OutputMediaAssets.First(), output);
        return true;
    }

    static void DownloadAsset(IAsset asset, string outputDirectory)
    {
        foreach (IAssetFile file in asset.AssetFiles)
        {
            file.Download(Path.Combine(outputDirectory, file.Name));
        }
    }


    static IAsset CreateAssetAndUploadSingleFile(string filePath, string assetName, AssetCreationOptions options)
    {
        IAsset asset = context.Assets.Create(assetName, options);

        var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
        assetFile.Upload(filePath);

        return asset;
    }

    static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = context.MediaProcessors
        .Where(p => p.Name == mediaProcessorName)
        .ToList()
        .OrderBy(p => new Version(p.Version))
        .LastOrDefault();

        if (processor == null)
            throw new ArgumentException(string.Format("Unknown media processor",
                                                       mediaProcessorName));

        return processor;
    }

### <a name="a-idfiletypesasupported-file-types"></a><a id="file_types"></a>Unterstützte Dateitypen

- MP4
- MOV
- WMV



##<a name="media-services-learning-paths"></a>Media-Dienste Learning Wege

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="related-links"></a>Links zu verwandten Themen

[Azure Media Services Analytics (Übersicht)](media-services-analytics-overview.md)

[Azure Medien Analytics demos](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

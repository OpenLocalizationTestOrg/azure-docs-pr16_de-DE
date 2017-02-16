<properties
    pageTitle="Mediendateien mit Azure Media Indexer Indizierung"
    description="Azure Medien Indexer können Sie die nach Inhalt von Mediendateien gesucht werden und ein Transkript Volltextindex für geschlossene Untertitel und Stichwörter generieren. In diesem Thema wird gezeigt, wie Medien Indexer verwendet werden."
    services="media-services"
    documentationCenter=""
    authors="Asolanki"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/12/2016"   
    ms.author="adsolank;juliako;johndeu"/>


# <a name="indexing-media-files-with-azure-media-indexer"></a>Mediendateien mit Azure Media Indexer Indizierung


Azure Medien Indexer können Sie die nach Inhalt von Mediendateien gesucht werden und ein Transkript Volltextindex für geschlossene Untertitel und Stichwörter generieren. Sie können eine Media-Datei oder mehrere Mediendateien in einem Stapel verarbeiten.  

>[AZURE.IMPORTANT] Beim Inhalt indizieren, stellen Sie sicher, um Mediendateien mit sehr klar Sprache (ohne Hintergrundmusik, Rauschen, Effekte oder Mikrofon Rauschen) verwendet werden. Einige Beispiele für entsprechende Inhalte sind: Besprechungen, vorlesungen oder Präsentationen aufgezeichnet. Der folgende Inhalte möglicherweise nicht dazu geeignet: Filme, Fernsehsendungen, einen anderen Wert als mit gemischtem Audio und Soundeffekte, aufgezeichnet schlecht Inhalte mit Hintergrundgeräusche (Rauschen).


Die folgenden Ausgaben kann ein Indizierung Auftrag generiert werden:

- Geschlossen Beschriftungsdateien in den folgenden Formaten: **SAMISCH**, **TTML**und **WebVTT**.

    Untertitel Dateien enthalten eine Kategorie besseren Erkennbarkeit empfiehlt sich, welche Faktoren ausgehend von ein Indizierung Auftrag wie erkennbar ist die Sprache in der Quellvideo wird aufgerufen.  Sie können den Wert der besseren Erkennbarkeit empfiehlt sich zum Prüfen von Dateien für Nutzbarkeit Ausgabe verwenden. Eine niedrige Bewertung bedeutet beeinträchtigt Indizierung Ergebnissen führen, weil die Audioqualität.
- Schlüsselwort-Datei (XML).
- Audio Indizierung BLOB-Datei (AIB) für die Verwendung mit SqlServer.

    Weitere Informationen finden Sie unter [Verwenden von AIB-Dateien mit Microsoft Azure Medien Indexer und SQL Server](https://azure.microsoft.com/blog/2014/11/03/using-aib-files-with-azure-media-indexer-and-sql-server/).


In diesem Thema wird gezeigt, wie Indizierung Aufträge **Index eines Wirtschaftsguts** und **Indizieren mehrerer Dateien**zu erstellen.

Die neuesten Updates für Azure Medien Indexer finden Sie unter [Blogs Media-Dienste](#preset).

## <a name="using-configuration-and-manifest-files-for-indexing-tasks"></a>Verwenden von Konfiguration und Manifest-Dateien für die Indizierung Aufgaben

Sie können weitere Details für Ihre Vorgänge Indizierung mithilfe des Task-Konfiguration angeben. Beispielsweise können Sie die Metadaten für die Media-Datei mit angeben. Diese Metadaten von Language-Engine verwendet, um Vokabular erweitern und erheblich verbessert die Genauigkeit der Spracherkennung.  Sie können auch die gewünschte Ausgabedateien angeben.

Sie können mehrere Mediendateien auch gleichzeitig mithilfe einer Manifestdatei verarbeiten.

Weitere Informationen finden Sie unter [Vorgang voreingestellte für Azure Medien Indexer](https://msdn.microsoft.com/library/dn783454.aspx).

## <a name="index-an-asset"></a>Indizieren einer Anlageguts

Die folgende Methode eine Media-Datei als Anlage hochgeladen und erstellt ein Projekt, um die Anlage zu indizieren.

Beachten Sie, dass keine Konfigurationsdatei festgelegt ist, die Media-Datei mit allen Standardeinstellungen indiziert werden.

    static bool RunIndexingJob(string inputMediaFilePath, string outputFolder, string configurationFile = "")
    {
        // Create an asset and upload the input media file to storage.
        IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
            "My Indexing Input Asset",
            AssetCreationOptions.None);

        // Declare a new job.
        IJob job = _context.Jobs.Create("My Indexing Job");

        // Get a reference to the Azure Media Indexer.
        string MediaProcessorName = "Azure Media Indexer";
        IMediaProcessor processor = GetLatestMediaProcessorByName(MediaProcessorName);

        // Read configuration from file if specified.
        string configuration = string.IsNullOrEmpty(configurationFile) ? "" : File.ReadAllText(configurationFile);

        // Create a task with the encoding details, using a string preset.
        ITask task = job.Tasks.AddNew("My Indexing Task",
            processor,
            configuration,
            TaskOptions.None);

        // Specify the input asset to be indexed.
        task.InputAssets.Add(asset);

        // Add an output asset to contain the results of the job.
        task.OutputAssets.AddNew("My Indexing Output Asset", AssetCreationOptions.None);

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
            Console.WriteLine("Exiting method due to job error.");
            return false;
        }

        // Download the job outputs.
        DownloadAsset(task.OutputAssets.First(), outputFolder);

        return true;
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
<!-- __ -->
### <a name="a-idoutputfilesaoutput-files"></a><a id="output_files"></a>Die Ausgabedateien

Standardmäßig generiert eine Indizierung Position die folgenden Ausgabedateien. Die Dateien werden in der ersten Ausgabe Anlage gespeichert werden.

Wenn mehrere Eingabewerte Media-Datei vorhanden ist, wird Indexer eine Manifestdatei für die Position Ausgaben, mit dem Namen 'JobResult.txt' generieren. Für jede für die Eingabe Media-Datei, die resultierende AIB, SAMISCH, TTML, WebVTT und Dateien Schlüsselwort, sequenziell nummeriert und mit dem Namen "Alias" verwenden.

Dateiname | Beschreibung
----------|------------
__InputFileName.aib__ | Indizierung Blob Audiodatei. <br /><br /> Audiodatei Indizierung Blob (AIB) ist eine Binärdatei, die in Microsoft SQL Server mithilfe der Suchfunktion vollständigen Text durchsucht werden kann.  Die Datei AIB ist zweierlei die Beschriftungsdateien einfache, da Alternativen für jedes Wort enthaltenen, gleicht viel reichhaltigere Suche zu verbessern. <br/> <br/>Es ist die Installation des Add-Ons Indexer SQL auf einem Computer ausgeführten Microsoft SQL Server 2008 oder höher erforderlich. Suchen die AIB mithilfe von Microsoft SQL bietet Server vollständigen Text suchen genauere Suchergebnisse als bei der Suche der Untertitel Dateien von WAMI generiert. Dies ist, da die AIB Alternativen für ein Wort enthält ein ähnliche Sound, während die Untertitel-Dateien für jedes Segment des Audiosignals der höchste KONFIDENZ Wort enthalten. Wenn Sie suchen nach gesprochenem Text upmost wichtig ist, empfiehlt es die AIB In Verbindung mit Microsoft SQL Server verwenden.<br/><br/> Wenn das Add-on herunterladen möchten, klicken Sie auf <a href="http://aka.ms/indexersql">Azure Medien Indexer SQL-Add-On</a>. <br/><br/>Es ist auch möglich, andere Suchmaschinen wie Apache Lucene/Solr so einfach indizieren Sie das Video basierend auf den Untertitel und Schlüsselwort XML-Dateien zu nutzen, aber dies führt zu Fehlern in den Suchergebnissen weniger genau.
__InputFileName.smi__<br />__InputFileName.ttml__<br />__InputFileName.vtt__ |Geschlossene Beschriftung (CC) Dateien in den Formaten SAMISCH, TTML und WebVTT.<br/><br/>Sie können verwendet werden, um Audio-und Videodateien für Menschen mit Behinderung hören zugänglich machen.<br/><br/>Geschlossene Beschriftungsdateien enthalten eine Kategorie <b>besseren Erkennbarkeit empfiehlt sich</b> , welche Faktoren wie erkennbar ist die Sprache, in der Quelle video ein Indizierung Auftrag anhand wird, aufgerufen.  Sie können den Wert der <b>besseren Erkennbarkeit empfiehlt sich</b> zum Prüfen von Dateien für Nutzbarkeit Ausgabe verwenden. Eine niedrige Bewertung bedeutet beeinträchtigt Indizierung Ergebnissen führen, weil die Audioqualität.
__InputFileName.kw.xml<br />InputFileName.info__ |Schlüsselwort und Info-Dateien. <br/><br/>Schlüsselwort-Datei ist eine XML-Datei, die Schlüsselwörter extrahiert aus dem Sprachinhalt mit Häufigkeit und Offset-Daten enthält. <br/><br/>Informationsdatei ist eine nur-Text-Datei, die detaillierten Informationen zu jeder erkannt Ausdruck enthält. Die erste Zeile ist Inhalte und der besseren Erkennbarkeit empfiehlt sich Punktzahl enthält. Jede zweite Zeile ist eine Registerkarte getrennte Liste mit den folgenden Daten: Zeit, Endzeit, Word/Ausdruck, gibt KONFIDENZ starten. Die Zeiten in Sekunden angegeben werden, und die KONFIDENZ wird als Zahl von 0 bis 1 angegeben. <br/><br/>Beispiel-Zeile: "1,20 1,45 Word 0,67" <br/><br/>Diese Dateien oder verwendet werden können für eine Reihe von Zwecken einsetzen, z. B. Sprache Analytics, ausführen verfügbar gemacht, um die Suchmaschinen wie Bing, Google oder Microsoft SharePoint die Mediendateien leichter auffindbar oder sogar, dass weitere relevante Werbung vorführen verwendet.
__JobResult.txt__ |Ausgabemanifests, nur, wenn Sie mehrere Dateien, die mit den folgenden Informationen Indizierung präsentieren:<br/><br/><table border="1"><tr><th>Eingabedatei</th><th>Alias</th><th>MediaLength</th><th>Fehler</th></tr><tr><td>a.mp4</td><td>Media_1</td><td>300</td><td>0</td></tr><tr><td>b.mp4</td><td>Media_2</td><td>0</td><td>3000</td></tr><tr><td>c.mp4</td><td>Media_3</td><td>600</td><td>0</td></tr></table><br/>



Wenn dies nicht alle Eingabewerte Mediendateien erfolgreich indiziert sind, der Auftrag Indizierung tritt ein Fehler mit dem Fehlercode 4000 ist. Weitere Informationen finden Sie unter [Fehlercodes](#error_codes).

## <a name="index-multiple-files"></a>Indizieren Sie mehrerer Dateien

Die folgende Methode mehrere Mediendateien als Anlage hochgeladen, und erstellt ein Projekt, um alle diese Dateien in einem Stapel indizieren.

Eine Manifestdatei mit der Erweiterung .lst wird erstellt und in der Anlage hochladen. Die Manifestdatei enthält die Liste aller Dateien der Anlage. Weitere Informationen finden Sie unter [Vorgang voreingestellte für Azure Medien Indexer](https://msdn.microsoft.com/library/dn783454.aspx).

    static bool RunBatchIndexingJob(string[] inputMediaFiles, string outputFolder)
    {
        // Create an asset and upload to storage.
        IAsset asset = CreateAssetAndUploadMultipleFiles(inputMediaFiles,
            "My Indexing Input Asset - Batch Mode",
            AssetCreationOptions.None);

        // Create a manifest file that contains all the asset file names and upload to storage.
        string manifestFile = "input.lst";            
        File.WriteAllLines(manifestFile, asset.AssetFiles.Select(f => f.Name).ToArray());
        var assetFile = asset.AssetFiles.Create(Path.GetFileName(manifestFile));
        assetFile.Upload(manifestFile);

        // Declare a new job.
        IJob job = _context.Jobs.Create("My Indexing Job - Batch Mode");

        // Get a reference to the Azure Media Indexer.
        string MediaProcessorName = "Azure Media Indexer";
        IMediaProcessor processor = GetLatestMediaProcessorByName(MediaProcessorName);

        // Read configuration.
        string configuration = File.ReadAllText("batch.config");

        // Create a task with the encoding details, using a string preset.
        ITask task = job.Tasks.AddNew("My Indexing Task - Batch Mode",
            processor,
            configuration,
            TaskOptions.None);

        // Specify the input asset to be indexed.
        task.InputAssets.Add(asset);

        // Add an output asset to contain the results of the job.
        task.OutputAssets.AddNew("My Indexing Output Asset - Batch Mode", AssetCreationOptions.None);

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
            Console.WriteLine("Exiting method due to job error.");
            return false;
        }

        // Download the job outputs.
        DownloadAsset(task.OutputAssets.First(), outputFolder);

        return true;
    }

    private static IAsset CreateAssetAndUploadMultipleFiles(string[] filePaths, string assetName, AssetCreationOptions options)
    {
        IAsset asset = _context.Assets.Create(assetName, options);

        foreach (string filePath in filePaths)
        {
            var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
            assetFile.Upload(filePath);
        }

        return asset;
    }

### <a name="partially-succeeded-job"></a>Teilweise erfolgreich Position

Wenn dies nicht alle Eingabewerte Mediendateien erfolgreich indiziert sind, der Auftrag Indizierung tritt ein Fehler mit dem Fehlercode 4000 ist. Weitere Informationen finden Sie unter [Fehlercodes](#error_codes).


Sind die gleichen Ausgaben (als erfolgreich Aufträge) generiert. Sie können auf die Ausgabemanifestdatei, um herauszufinden, welche Eingabewerte Dateien, entsprechend der Fehlerwerte Spalte fehlgeschlagen sind verweisen. Für die Eingabewerte Dateien, die Fehler bei, die sich ergebende AIB, SAMISCH, TTML, WebVTT und das Schlüsselwort werden Dateien nicht generiert werden.

### <a name="a-idpreseta-task-preset-for-azure-media-indexer"></a><a id="preset"></a>Aufgabe Voreinstellung für Indexer Azure Medien

Die Verarbeitung von Azure Medien Indexer kann angepasst werden, indem Sie eine optionale Aufgabe voreingestellte entlang der Aufgabe.  Nachfolgend wird das Format des dieser Konfiguration Xml beschrieben.

Namen | Erforderlich | Beschreibung
----|----|---
__Eingabe__ | falsch | Anlage Datei(en) aus, die Sie indizieren möchten.</p><p>Azure Medien Indexer unterstützt die folgenden Dateiformate von Medien: MP4, WMV, MP3-, M4A, WMA, AAC, WAV-Dateien.</p><p>Sie können den Dateinamen (s) im **Namen** oder **Listen** -Attribut des Elements **Eingabewerte** angeben (siehe unten). Wenn Sie die Bilddatei auf Index nicht angeben, wird die primäre Datei entnommen. Wenn keine primäre Bilddatei festgelegt ist, wird die erste Datei in die Eingabewerte Anlage indiziert.</p><p>Wenn Sie den Dateinamen der Anlage explizit angeben möchten, gehen Sie wie folgt:<br />`<input name="TestFile.wmv">`<br /><br />Sie können auch mehrere Anlage Dateien auf einmal (bis zu 10) indizieren. Zweck<br /><br /><ol class="ordered"><li><p>Erstellen Sie eine Textdatei (Manifestdatei), und probieren Sie es eine Erweiterung .lst. </p></li><li><p>Hinzufügen einer Liste aller Dateinamen der Anlage zu dieser Manifestdatei Ihrer Eingabe Ressource. </p></li><li><p>Hinzufügen von Thanifest-Datei (hochladen) auf die Anlage.  </p></li><li><p>Geben Sie den Namen der Manifestdatei in Attribut für die Eingabe in der Liste aus.<br />`<input list="input.lst">`</li></ol><br /><br />Hinweis: Wenn Sie mehr als 10 Dateien zur Manifestdatei hinzufügen, wird die Indizierung mit dem Fehlercode 2006 nicht erfolgreich.
__Metadaten__ | falsch | Metadaten für die angegebene Anlage Datei(en) aus, die für die Anpassung Vokabular verwendet.  Vorbereiten der Indexer nicht standardmäßige Vokabular Wörter wie z. B. Eigennamen erkennen hilfreich.<br />`<metadata key="..." value="..."/>` <br /><br />Sie können die __Werte__ für vordefinierte __Schlüssel__angeben. Die folgenden Schlüssel sind aktuell nicht unterstützt:<br /><br />"Titel" und "Beschreibung" - für Vokabular Anpassung verwendet, um die Sprache gibt für den Job modellieren und Genauigkeit der Spracherkennung verbessern.  Die Werte Startwert für Suchvorgänge im Internet zum Suchen von je nach Kontext relevante Textdokumente unter Verwendung des Inhalts das interne Wörterbuch für die Dauer der Aufgabe Indizierung zu erweitern.<br />`<metadata key="title" value="[Title of the media file]" />`<br />`<metadata key="description" value="[Description of the media file] />"`
__Features__ <br /><br /> In Version 1.2 hinzugefügt. Derzeit ist das einzige unterstützte Feature Spracherkennung ("ASR").| falsch | Die Spracherkennung weist die folgenden Einstellungen Schlüssel:<table><tr><th><p>Schlüssel</p></th>     <th><p>Beschreibung</p></th><th><p>Beispiel für einen Wert</p></th></tr><tr><td><p>Sprache</p></td><td><p>Die natürlicher Sprache in der Multimediadatei erkannt werden.</p></td><td><p>Englisch, Spanisch</p></td></tr><tr><td><p>CaptionFormats</p></td><td><p>eine durch Semikolons getrennte Liste der gewünschten Beschriftung Ausgabeformat (falls vorhanden)</p></td><td><p>Ttml; Samisch; webvtt</p></td></tr><tr><td><p>GenerateAIB</p></td><td><p>Eine boolesche Kennzeichnung angegeben werden, ob eine Datei AIB (zur Verwendung mit SQL Server und den Kunden Indexer IFilter) erforderlich ist.  Weitere Informationen finden Sie unter <a href="http://azure.microsoft.com/blog/2014/11/03/using-aib-files-with-azure-media-indexer-and-sql-server/">Verwenden von AIB-Dateien mit Microsoft Azure Medien Indexer und SQL Server</a>.</p></td><td><p>WAHR; Falsch</p></td></tr><tr><td><p>GenerateKeywords</p></td><td><p>Eine boolesche Kennzeichnung angegeben werden, ob eine Schlüsselwort XML-Datei erforderlich ist.</p></td><td><p>WAHR; Falsch. </p></td></tr><tr><td><p>ForceFullCaption</p></td><td><p>Eine boolesche Kennzeichnung, die angibt, ob ein so erzwingen Sie vollständige Untertitel (unabhängig davon Confidence Level).  </p><p>Standard ist "false", "in diesem Fall Wörter und Ausdrücke, denen eine kleiner als 50 % Konfidenzintervall in die endgültige Beschriftung Ausgaben ausgelassen und werden durch Auslassungszeichen ("...") ersetzt.  Die drei Punkte sind nützlich für Beschriftung qualitätssicherung und Überwachung.</p></td><td><p>WAHR; Falsch. </p></td></tr></table>

### <a name="a-iderrorcodesaerror-codes"></a><a id="error_codes"></a>Fehlercodes

Bei einem Fehler Azure Medien Indexer Bericht sollte eine der folgenden Fehlercodes zurück:

Code | Namen | Mögliche Gründe
-----|------|------------------
2000 | Ungültige Konfiguration | Ungültige Konfiguration
2001 | Ungültige von Anlagen | Fehlende Eingabe Vermögenswerte oder leere Anlage.
2002 | Ungültige Manifestdatei | Manifest leer ist oder Manifest enthält ungültige Elemente.
2003 | Fehler beim Herunterladen des Media-Datei | Ungültige URL in Manifestdatei.
2004 | Nicht unterstütztes Protokoll | Protokoll der Media-URL wird nicht unterstützt.
2005 | Nicht unterstützte Dateityp | Eingabe Media-Dateityp wird nicht unterstützt.
2006 | Zu viele von Dateien | Es gibt mehr als 10 Dateien in das Eingabemanifest.
3000 | Fehler beim Entschlüsseln Media-Datei | Nicht unterstützte Media-Codecs <br/>oder<br/> Beschädigte Media-Datei <br/>oder<br/> Keine Audiodatenstrom in Eingabewerte Medien.
4000 | Teilweise erfolgreich Stapel Indizierung | Einige der Eingabewerte Mediendateien werden konnten nicht indiziert werden. Weitere Informationen finden Sie unter <a href="#output_files">Ausgabedateien</a>.
andere | Interner Fehler | Wenden Sie sich an Supportteam. indexer@microsoft.com


## <a name="a-idsupportedlanguagesasupported-languages"></a><a id="supported_languages"></a>Unterstützte Sprachen

Derzeit werden die Sprachen Englisch und Spanisch unterstützt. Weitere Informationen finden Sie unter [der Version 1.2 Release Blogbeitrag veröffentlichen](https://azure.microsoft.com/blog/2015/04/13/azure-media-indexer-spanish-v1-2/).


##<a name="media-services-learning-paths"></a>Media-Dienste Learning Wege

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


## <a name="related-links"></a>Links zu verwandten Themen

[Azure Media Services Analytics (Übersicht)](media-services-analytics-overview.md)

[Verwenden von AIB-Dateien mit Azure Media Indexer und SQLServer](https://azure.microsoft.com/blog/2014/11/03/using-aib-files-with-azure-media-indexer-and-sql-server/)

[Mediendateien mit Azure Media Indexer 2 Vorschau Indizierung](media-services-process-content-with-indexer2.md)

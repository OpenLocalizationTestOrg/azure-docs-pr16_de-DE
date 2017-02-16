<properties
    pageTitle="Erste Schritte mit der Bereitstellung von Inhalten bei Bedarf mit .NET | Azure"
    description="Dieses Lernprogramm führt Sie durch die Schritte zum Implementieren der Anwendung Bereitstellung von Inhalten auf Demand mit Azure Media Services mit .NET."
    services="media-services"
    documentationCenter=""
    authors="Juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/17/2016"
    ms.author="juliako"/>


# <a name="get-started-with-delivering-content-on-demand-using-net-sdk"></a>Erste Schritte mit der Bereitstellung von Inhalten bei Bedarf mit .NET SDK

[AZURE.INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]

>[AZURE.NOTE]
> Damit dieses Lernprogramm abgeschlossen, benötigen Sie ein Azure-Konto an. Weitere Informationen finden Sie unter [Azure kostenlose Testversion](/pricing/free-trial/?WT.mc_id=A261C142F). 
 
##<a name="overview"></a>(Übersicht) 

Dieses Lernprogramm führt Sie durch die Schritte zum Implementieren einer Demand (VoD) Bereitstellung von Inhalten Anwendungs mit Azure Media Services (AMS) SDK für .NET.


Das Lernprogramm führt grundlegende Media-Dienste Workflows und die am häufigsten verwendeten programming Objekte und Aufgaben für die Entwicklung von Media-Dienste erforderlich. Am Ende des Lernprogramms werden Sie möglicherweise zu übertragen oder schrittweise herunterladen eine Stichprobe Media-Datei, die Sie hochgeladen, codierte und heruntergeladen.

## <a name="what-youll-learn"></a>Sie erfahren

Das Lernprogramm wird gezeigt, wie die folgenden Aufgaben ausführen:

1.  Erstellen einer Media-Dienste-Konto (mit dem Azure-Portal).
2.  Konfigurieren Sie streaming Endpunkt (mit dem Azure-Portal).
3.  Erstellen Sie und konfigurieren Sie einer Visual Studio-Projekt.
5.  Verbinden mit dem Konto Media-Dienste.
6.  Erstellen Sie eine neue Anlage und Hochladen Sie eine Videodatei.
7.  Codieren Sie die Quelldatei in eine Reihe von adaptive Bitrate MP4-Dateien ein.
8.  Veröffentlichen Sie die Anlage, und erhalten Sie URLs für streaming und beständigen herunterladen.
9.  Testen Sie, indem Sie die Wiedergabe von Inhalten.

## <a name="prerequisites"></a>Erforderliche Komponenten

Die folgenden sind für dieses Lernprogramm erforderlich erforderlich.

- Damit dieses Lernprogramm abgeschlossen, benötigen Sie ein Azure-Konto an. 
    
    Wenn Sie kein Konto haben, können Sie ein kostenloses Testversion Konto nur wenigen Minuten erstellen. Weitere Informationen finden Sie unter [Azure kostenlose Testversion](/pricing/free-trial/?WT.mc_id=A261C142F). Sie erhalten Gutschriften, die mit dem Testen kostenpflichtiges Azure Services verwendet werden können. Auch nachdem die Gutschriften von verwendet werden, können Sie das Konto beibehalten und kostenlosen Azure Dienste und Features, wie das Feature Web Apps in Azure-App-Dienst verwenden.
- Betriebssysteme: Windows 8 oder höher, Windows 2008 R2, Windows 7.
- .NET Framework 4.0 oder höher
- Visual Studio 2010 SP1 (Professional, Premium, Ultimate oder Express) oder höhere Versionen.


##<a name="download-sample"></a>Beispiel für herunterladen

Abrufen und Ausführen einer Stichprobe aus [hier](https://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/).

## <a name="create-an-azure-media-services-account-using-the-azure-portal"></a>Erstellen Sie ein Azure Media Services-Konto über das Azure-portal

Die Schritte in diesem Abschnitt zeigen, wie ein Konto AMS zu erstellen.

1. Melden Sie sich bei der [Azure-Portal](https://portal.azure.com/)an.
2. Klicken Sie auf **+ neue** > **Medien + CDN** > **Media-Dienste**.

    ![Media-Dienste zu erstellen.](./media/media-services-portal-vod-get-started/media-services-new1.png)

3. Geben Sie die gewünschten Werte, in **MEDIA SERVICES-Kontos zu erstellen** .

    ![Media-Dienste zu erstellen.](./media/media-services-portal-vod-get-started/media-services-new3.png)
    
    1. Geben Sie in das Feld **Kontoname**den Namen des neuen AMS-Kontos ein. Ein Kontonamen Media-Dienste ist alle Kleinbuchstaben Ziffern oder Buchstaben ohne Leerzeichen und 3 bis 24 Zeichen lang ist.
    2. Wählen Sie im Abonnement zwischen den verschiedenen Azure-Abonnements, denen Sie Zugriff haben.
    
    2. Wählen Sie in der **Ressourcengruppe**der neuen oder vorhandenen Ressource ein.  Eine Ressourcengruppe ist eine Sammlung von Ressourcen, die Lebenszyklus, Berechtigungen und Richtlinien gemeinsam nutzen. Weitere finden Sie [hier](azure-resource-manager/resource-group-overview.md#resource-groups).
    3. **Speicherort**werden auswählen die geografische Region verwendet, um die Einträge Medien und Metadaten für Ihr Konto Media-Dienste zu speichern. Diese Region wird zum Verarbeiten und Streamen von Medien verwendet. Die verfügbaren Media-Dienste Regionen werden im Dropdown-Listenfeld angezeigt. 
    
    3. Wählen Sie in **Speicher-Konto**ein Speicherkonto, um Blob-Speicher des Inhalts Medien über Ihr Konto Media-Dienste bereitzustellen. Sie können ein vorhandenes Speicherkonto in der gleichen geografische Region als Ihr Konto Media-Dienste auswählen, oder Sie können ein Speicherkonto erstellen. Ein neues Speicherkonto wird in der gleichen Region erstellt. Die Regeln für Speicher Kontonamen sind die gleichen wie bei Medien Dienstkonten.

        Weitere Informationen zu Speicher [hier](storage-introduction.md).

    4. Wählen Sie die **Pin zum Dashboard** , um den Fortschritt der Bereitstellung Konto finden Sie unter.
    
7. Klicken Sie auf **Erstellen** am unteren Rand des Formulars.

    Nachdem das Konto erfolgreich erstellt wurde, ändert sich der Status für die **Ausführung von**. 

    ![Media-Dienste-Einstellungen](./media/media-services-portal-vod-get-started/media-services-settings.png)

    Verwalten Sie Ihr Konto AMS (z. B. Hochladen von Videos, Codieren von Anlagen, Überwachung des Projektstatus) verwenden Sie das Fenster **Einstellungen** .

## <a name="configure-streaming-endpoints-using-the-azure-portal"></a>Konfigurieren von streaming Endpunkte mithilfe von Azure-portal

Wenn Sie mit Azure Media-Dienste zu arbeiten, die eine der häufigsten Szenarios über adaptive Bitrate streaming Video zum Kunden übermittelt. Media-Dienste unterstützt die folgende adaptive Bitrate Technologien streaming: HTTP Live Streaming (HLS), interpolierten Streaming, MPEG Gedankenstrich und HDS (für nur Adobe vorzeigbare/Access Lizenznehmern).

Media Services bietet dynamische Verpacken, dem Sie Ihre adaptive Bitrate MP4-codierte Inhalte von Media-Dienste (MPEG Gedankenstrich HLS, interpolierten Streaming, HDS) nur-Time, ohne dass Sie vorkonfigurierte Versionen von jedem der folgenden streaming-Formaten speichern unterstützten Formate streaming vorführen kann.

Um dynamische Verpackung nutzen zu können, müssen Sie die folgenden Aktionen ausführen:

- Codieren der Datei Mezzanine (Quelle) in eine Reihe von adaptive Bitrate MP4-Dateien (die Codierung Schritte sind weiter unten in diesem Lernprogramm gezeigt).  
- Erstellen Sie mindestens eine streaming Einheit für das *streaming Endpunkt* aus der Sie die Übermittlung von Inhalten erstellen möchten. Schritte anzeigen zum Ändern der Anzahl der streaming Einheiten

Dynamische Verpackung, müssen Sie nur zu speichern und die Dateien in den einzelnen Speicherformat bezahlen und Media-Dienste erstellt und die entsprechende Antwort basierend auf einem Client-Anfragen fungiert.

Zum Erstellen und ändern die Anzahl der Einheiten reservierte streaming, führen Sie folgende Schritte aus:


1. Klicken Sie im Fenster **Einstellungen** auf **Streaming Endpunkte**. 

2. Klicken Sie auf die Standardeinstellungen für streaming-Endpunkt. 

    Das Fenster **STANDARDMÄßIG STREAMING ENDPUNKTDETAILS** wird angezeigt.

3. Um die Anzahl der Einheiten streaming anzugeben, schieben Sie den Schieberegler **Streaming Einheiten** .

    ![Streaming Einheiten](./media/media-services-portal-vod-get-started/media-services-streaming-units.png)

4. Klicken Sie auf die Schaltfläche **Speichern** , um die Änderungen zu speichern.

    >[AZURE.NOTE]Die Zuordnung von allen neuen Einheiten kann bis zu 20 Minuten dauern.

##<a name="create-and-configure-a-visual-studio-project"></a>Erstellen Sie und konfigurieren Sie einer Visual Studio-Projekt

1. Erstellen Sie eine neue c# in Visual Studio 2013, Visual Studio 2012 oder Visual Studio 2010 SP1. Geben Sie den **Namen**, **Speicherort**und **Namen der Lösung**, und klicken Sie dann auf **OK**.

2. Verwenden Sie das [windowsazure.mediaservices.extensions](https://www.nuget.org/packages/windowsazure.mediaservices.extensions) NuGet-Paket zum **Azure Media Services .NET SDK Erweiterungen**zu installieren.  Die Medien Services .NET SDK Erweiterungen ist eine Reihe von Erweiterungsmethoden und Helper-Funktionen, die von Code zu vereinfachen und zur Entwicklung mit Media-Dienste zu erleichtern. Installation dieses Pakets, **Media Services.NET SDK** installiert und alle anderen erforderlichen Abhängigkeiten addiert.

3. Fügen Sie einen Verweis auf System.Configuration Assembly hinzu. Diese Assembly enthält die **System.Configuration.ConfigurationManager** Klasse, mit der Konfigurationsdateien, z. B. App.config zugreifen.

4. Öffnen Sie die App (die Datei wird zu einem Projekt hinzufügen, wenn sie nicht standardmäßig hinzugefügt wurde), und fügen Sie einen Abschnitt *AppSettings* zu der Datei. Legen Sie die Werte für Ihren Azure Media Services Servername und die Kontoinformationen kontoschlüssel, wie im folgenden Beispiel dargestellt. Um den Kontonamen und wichtige Informationen zu erhalten, wechseln Sie zum [Azure-Portal](https://portal.azure.com/) , und wählen Sie Ihr Konto AMS. Wählen Sie dann auf **Einstellungen** > **Tasten**. Windows-Taste verwalten zeigt den Namen des Kontos und die primären und sekundären Schlüssel wird angezeigt.

        <configuration>
        ...
          <appSettings>
            <add key="MediaServicesAccountName" value="Media-Services-Account-Name" />
            <add key="MediaServicesAccountKey" value="Media-Services-Account-Key" />
          </appSettings>
          
        </configuration>

5. Überschreiben Sie die vorhandenen **mithilfe von** Anweisungen am Anfang der Datei Program.cs mit den folgenden Code ein.

        using System;
        using System.Collections.Generic;
        using System.Linq;
        using System.Text;
        using System.Threading.Tasks;
        using System.Configuration;
        using System.Threading;
        using System.IO;
        using Microsoft.WindowsAzure.MediaServices.Client;
        

6. Erstellen eines neuen Ordners unter dem Verzeichnis Projects, und kopieren Sie eine MP4 oder WMV-Datei, die Sie codieren und übertragen oder schrittweise herunterladen möchten. In diesem Beispiel wird der Pfad "C:\VideoFiles" verwendet.

##<a name="connect-to-the-media-services-account"></a>Verbinden mit dem Konto Media-Dienste

Beim Media-Dienste mit .NET verwenden zu können, muss die Klasse **CloudMediaContext** für die meisten Aufgaben programming Media-Dienste verwendet: Herstellen einer Verbindung mit Media Services-Kontos; Erstellen, aktualisieren, den Zugriff auf und löschen die folgenden Objekte: Anlagen, Anlage Dateien, Aufträge, Richtlinien, Locator usw..

Überschreiben Sie die standardmäßige Programmklasse mit den folgenden Code ein. Der Code wird veranschaulicht, lesen Sie die Verbindung Werte aus der App und das **CloudMediaContext** -Objekts zu erstellen, um die Verbindung mit Media-Dienste. Weitere Informationen zum Verbinden mit Media-Dienste finden Sie unter [Herstellen einer Verbindung mit Media-Dienste mit dem Media Services SDK für .NET](http://msdn.microsoft.com/library/azure/jj129571.aspx).

Die Funktion **Main** ruft Methoden, die definiert werden, werden in diesem Abschnitt eingehender.

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
            try
            {
                // Create and cache the Media Services credentials in a static class variable.
                _cachedCredentials = new MediaServicesCredentials(
                                _mediaServicesAccountName,
                                _mediaServicesAccountKey);
                // Used the chached credentials to create CloudMediaContext.
                _context = new CloudMediaContext(_cachedCredentials);

                // Add calls to methods defined in this section.

                IAsset inputAsset =
                    UploadFile(@"C:\VideoFiles\BigBuckBunny.mp4", AssetCreationOptions.None);

                IAsset encodedAsset =
                    EncodeToAdaptiveBitrateMP4s(inputAsset, AssetCreationOptions.None);

                PublishAssetGetURLs(encodedAsset);
            }
            catch (Exception exception)
            {
                // Parse the XML error message in the Media Services response and create a new
                // exception with its content.
                exception = MediaServicesExceptionParser.Parse(exception);

                Console.Error.WriteLine(exception.Message);
            }
            finally
            {
                Console.ReadLine();
            }
        }

##<a name="create-a-new-asset-and-upload-a-video-file"></a>Erstellen Sie eine neue Anlage und Hochladen eine Videodatei

Media-Dienste Sie hochladen (oder Aufnahme) Ihre digitalen Dateien in einer Anlage. Die **Anlage** Entität kann Video, Audio, Bilder, Miniaturansichten Sammlungen, Text Spuren, und Untertitel Dateien (und die Metadaten für diese Dateien.) enthalten.  Sobald die Dateien hochgeladen werden, werden Ihre Inhalte in der Cloud für die weitere Verarbeitung und streaming sicher gespeichert. Die Dateien in der Anlage werden als **Anlage Dateien**bezeichnet.

Die **UploadFile** -Methode unter Anrufe **CreateFromFile** (in .NET SDK Erweiterungen definiert) definiert. **CreateFromFile** erstellt eine neue Anlage, in der die angegebene Quelldatei hochgeladen wird.

Die Methode **CreateFromFile** nimmt **AssetCreationOptions** , dem Sie eine der folgenden Optionen Anlage Erstellung angeben können:

- **Keine** - keine Verschlüsselung verwendet wird. Dies ist der Standardwert. Beachten Sie, wenn Sie diese Option verwenden zu können, der Inhalt nicht übertragen werden oder bei Rest im Speicher geschützt ist.
Wenn Sie beabsichtigen, eine MP4 vorführen mit schrittweisen herunterladen, verwenden Sie diese Option.
- **StorageEncrypted** - verwenden Sie diese Option, um verschlüsseln löschen Inhalte lokal mit erweiterte Verschlüsselung AES (Standard)-256-Bit-Verschlüsselung, der dann es zum Azure-Speicher hochgeladen wird, in dem sie gespeichert ist, bei Rest verschlüsselt. Posten mit Speicher Verschlüsselung geschützt werden automatisch entschlüsselt in eine verschlüsselte Dateisystem vor Codierung platziert und optional vor dem Hochladen wieder als eine neue Ausgabe Anlage erneut verschlüsselt. Die primäre Anwendungsfall-für die Verschlüsselung der Speicher ist, wenn Sie Ihre Dateien von Medien in hoher Qualität mit Verschlüsselung statisch sind auf dem Datenträger schützen möchten.
- **CommonEncryptionProtected** - verwenden Sie diese Option, wenn Sie Inhalte hochladen, die bereits verschlüsselt und mit allgemeinen Verschlüsselung oder PlayReady DRM (z. B. interpolierten Streaming geschützt mit PlayReady DRM) geschützt.
- **EnvelopeEncryptionProtected** – verwenden Sie diese Option, wenn Sie mit AES verschlüsselt HLS hochladen. Beachten Sie, dass müssen die Dateien codierte und Transformieren Manager verschlüsselt wurden.

Die **CreateFromFile** -Methode können Sie einen Rückruf angeben, um den Status für das Hochladen der Datei zu melden.

Im folgenden Beispiel geben wir **keine** der Optionen für die Anlage ein.

Fügen Sie die folgende Methode zur Klasse Programm aus.

    static public IAsset UploadFile(string fileName, AssetCreationOptions options)
    {
        IAsset inputAsset = _context.Assets.CreateFromFile(
            fileName,
            options,
            (af, p) =>
            {
                Console.WriteLine("Uploading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
            });

        Console.WriteLine("Asset {0} created.", inputAsset.Id);

        return inputAsset;
    }


##<a name="encode-the-source-file-into-a-set-of-adaptive-bitrate-mp4-files"></a>Codieren von der Quelldatei in eine Reihe von adaptive Bitrate MP4-Dateien

Nach Aufnahme Anlagen in Media-Dienste, Medien werden können codiert, Transmuxed, mit einem Wasserzeichen versehen, usw., wenn es an die Clients übermittelt wird. Diese Aktivitäten sind geplant, und führen Sie für mehrere Instanzen von Hintergrund Rolle um hohe Leistung und Verfügbarkeit sicherzustellen. Diese Aktivitäten werden Aufträge bezeichnet, und jedes Projekt besteht atomare Aufgaben, die die ist-Arbeit auf die Bilddatei ausführen.

Wie bereits erwähnt wurde bei der Arbeit mit Azure Media Services, ist eines der am häufigsten verwendeten Szenarios adaptive Bitrate streaming an Ihre Kunden vorführen. Media-Dienste können dynamisch eine Reihe von adaptive Bitrate MP4-Dateien in einem der folgenden Formate Verpacken: HTTP Live Streaming (HLS), interpolierten Streaming, MPEG Gedankenstrich und HDS (für nur Adobe vorzeigbare/Access Lizenznehmern).

Um dynamische Verpackung nutzen zu können, müssen Sie die folgenden Aktionen ausführen:

- Codieren oder Codierung Ihrer Mezzanine (Quelle) in eine Reihe von adaptive Bitrate MP4-Dateien oder adaptive Bitrate interpolierten Streaming-Dateien ablegen.  
- Holen Sie mindestens eine streaming Einheit für den streaming Endpunkt, aus dem Sie bis zur Bereitstellung des Inhalts planen.

Mit dem folgende Code wird gezeigt, wie übermitteln eine Codierung werden. Der Auftrag enthält eine Aufgabe, die zu Codierung die Mezzanine-Datei in eine Reihe von adaptive Bitrate MP4s mit **Media Encoder Standard**angibt. Der Code sendet den Auftrag und wartet, bis er abgeschlossen ist.

Nachdem Sie der Auftrag abgeschlossen ist, möchten Sie möglicherweise übertragen Ihrer Ressource oder den download schrittweise MP4-Dateien, die als Ergebnis Umcodierung erstellt wurden.
Hinweis die benötigten nicht mehr als 0 streaming Einheiten um schrittweise MP4-Dateien heruntergeladen haben.

Fügen Sie die folgende Methode zur Klasse Programm aus.

    static public IAsset EncodeToAdaptiveBitrateMP4s(IAsset asset, AssetCreationOptions options)
    {
    
        // Prepare a job with a single task to transcode the specified asset
        // into a multi-bitrate asset.
    
        IJob job = _context.Jobs.CreateWithSingleTask(
            "Media Encoder Standard",
            "H264 Multiple Bitrate 720p",
            asset,
            "Adaptive Bitrate MP4",
            options);
    
        Console.WriteLine("Submitting transcoding job...");
    
    
        // Submit the job and wait until it is completed.
        job.Submit();
    
        job = job.StartExecutionProgressTask(
            j =>
            {
                Console.WriteLine("Job state: {0}", j.State);
                Console.WriteLine("Job progress: {0:0.##}%", j.GetOverallProgress());
            },
            CancellationToken.None).Result;
    
        Console.WriteLine("Transcoding job finished.");
    
        IAsset outputAsset = job.OutputMediaAssets[0];
    
        return outputAsset;
    }

##<a name="publish-the-asset-and-get-urls-for-streaming-and-progressive-download"></a>Veröffentlichen Sie die Anlage, und erhalten Sie URLs für streaming und beständigen herunterladen

Um das Streaming oder Herunterladen einer Anlageguts, müssen Sie zuerst "ihn um seine Erlaubnis" durch ein Locator erstellen. Locator ermöglichen den Zugriff auf Dateien in der Anlage. Media Services unterstützt zwei Arten von Locator: OnDemandOrigin Locator, verwendet, um Medien (z. B. MPEG Gedankenstrich, HLS oder interpolierten Streaming) und Access-Signatur (SAS) Locator, zum Herunterladen von Mediendateien verwendet.

Nachdem Sie die Locator erstellt haben, können Sie die URLs erstellen, mit denen das Streaming oder Herunterladen von Dateien.


Eine streaming-URL für die reibungslose Streaming weist das folgende Format:

     {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

Eine streaming URL für HLS weist das folgende Format:

     {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

Eine streaming URL für MPEG Gedankenstrich weist das folgende Format:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

Eine SAS-URL zum Herunterladen von Dateien verwendet weist das folgende Format:

    {blob container name}/{asset name}/{file name}/{SAS signature}

Media Services .NET SDK Erweiterungen geeignete Helper Methoden bereitstellen, die formatierten URLs für die Anlage veröffentlichten zurückzukehren.

Im folgenden Code wird .NET SDK Erweiterungen Locator erstellen und streaming erhalten und URLs schrittweisen Download. Der Code wird gezeigt, wie Dateien in einer lokalen Ordner herunterladen.

Fügen Sie die folgende Methode zur Klasse Programm aus.

    static public void PublishAssetGetURLs(IAsset asset)
    {
        // Publish the output asset by creating an Origin locator for adaptive streaming,
        // and a SAS locator for progressive download.

        _context.Locators.Create(
            LocatorType.OnDemandOrigin,
            asset,
            AccessPermissions.Read,
            TimeSpan.FromDays(30));

        _context.Locators.Create(
            LocatorType.Sas,
            asset,
            AccessPermissions.Read,
            TimeSpan.FromDays(30));


        IEnumerable<IAssetFile> mp4AssetFiles = asset
                .AssetFiles
                .ToList()
                .Where(af => af.Name.EndsWith(".mp4", StringComparison.OrdinalIgnoreCase));

        // Get the Smooth Streaming, HLS and MPEG-DASH URLs for adaptive streaming,
        // and the Progressive Download URL.
        Uri smoothStreamingUri = asset.GetSmoothStreamingUri();
        Uri hlsUri = asset.GetHlsUri();
        Uri mpegDashUri = asset.GetMpegDashUri();

        // Get the URls for progressive download for each MP4 file that was generated as a result
        // of encoding.
        List<Uri> mp4ProgressiveDownloadUris = mp4AssetFiles.Select(af => af.GetSasUri()).ToList();


        // Display  the streaming URLs.
        Console.WriteLine("Use the following URLs for adaptive streaming: ");
        Console.WriteLine(smoothStreamingUri);
        Console.WriteLine(hlsUri);
        Console.WriteLine(mpegDashUri);
        Console.WriteLine();

        // Display the URLs for progressive download.
        Console.WriteLine("Use the following URLs for progressive download.");
        mp4ProgressiveDownloadUris.ForEach(uri => Console.WriteLine(uri + "\n"));
        Console.WriteLine();

        // Download the output asset to a local folder.
        string outputFolder = "job-output";
        if (!Directory.Exists(outputFolder))
        {
            Directory.CreateDirectory(outputFolder);
        }

        Console.WriteLine();
        Console.WriteLine("Downloading output asset files to a local folder...");
        asset.DownloadToFolder(
            outputFolder,
            (af, p) =>
            {
                Console.WriteLine("Downloading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
            });

        Console.WriteLine("Output asset files available at '{0}'.", Path.GetFullPath(outputFolder));
    }

##<a name="test-by-playing-your-content"></a>Testen Sie, indem Sie die Wiedergabe von Inhalten  

Nachdem Sie das Programm, das im vorherigen Abschnitt definiert ausführen, werden die URLs ähnlich wie der folgende Console-Fenster angezeigt.

Adaptive streaming URLs:

Interpolierten Streaming

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest

HLS

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=m3u8-aapl)

MPEG-BINDESTRICH

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=mpd-time-csf)

Schrittweisen Download URLs (Audio und Video).

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1500kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1000kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_56kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z


Verwenden Sie [Azure Services Medienwiedergabe](http://amsplayer.azurewebsites.net/azuremediaplayer.html), um das Video zu streamen.

Klicken Sie zum schrittweisen Download zu testen, fügen Sie eine URL in einem Browser (z. B. Internet Explorer, Chrome oder Safari) aus.


##<a name="next-steps-media-services-learning-paths"></a>Nächste Schritte: Media-Dienste learning Wege

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


### <a name="looking-for-something-else"></a>Suchen Sie nach etwas anderem?

Wenn Sie dieses Thema enthielt, was Sie erwartet haben einen Beitrag nicht angezeigt wird, oder in andere Weise nicht Ihren Anforderungen, benötigen wir Ihr Feedback mit dem folgenden Disqus Thread auf.


<!-- Anchors. -->


<!-- URLs. -->
  [Web Platform Installer]: http://go.microsoft.com/fwlink/?linkid=255386
  [Portal]: http://manage.windowsazure.com/

<properties
    pageTitle="Bereitstellung von Inhalten für Kunden | Microsoft Azure"
    description="Dieses Thema bietet einen Überblick darüber, was im Zusammenhang mit dem Inhalt mit Azure Media Services vorführen."
    services="media-services"
    documentationCenter=""
    authors="Juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="juliako"/>


# <a name="deliver-content-to-customers"></a>Bereitstellung von Content für Kunden
Wenn Sie den Inhalt streaming oder Demand für Kunden vorführen sind, ist das Ziel verschiedenen Geräten unter anderen Netzwerkprobleme hoch auflösendes Video vorführen.

Um dies zu erreichen, können Sie folgende Aktionen ausführen:

- Codieren der Stream zu einem Videodatenstrom Multi-Bitrate (adaptive Bitrate). Dies wird Qualität und Netzwerk Bedingungen erledigen.
- Verwenden Sie Microsoft Azure Media-Dienste [dynamische Verpackung](media-services-dynamic-packaging-overview.md) Ihrer Stream in unterschiedlichen Protokollen dynamisch erneut verpacken. Dies wird übernehmen auf verschiedenen Geräten streaming. Media-Dienste unterstützt die folgenden adaptive Bitrate streaming Technologien Übermittlung: HTTP Live Streaming (HLS), interpolierten Streaming, MPEG-Strich und HDS (für nur Adobe vorzeigbare/Access Lizenznehmern).

Dieser Artikel bietet einen Überblick über die Bereitstellung von Inhalten wichtige Konzepte.

Um bekannte Probleme zu überprüfen, finden Sie unter [bekannte Probleme](media-services-deliver-content-overview.md#known-issues).

## <a name="dynamic-packaging"></a>Dynamische Verpackung

Mit der dynamischen Verpackung, die Media-Dienste bereitstellt, können Sie Ihre adaptive Bitrate MP4 oder interpolierten Streaming codierte Inhalt von Media-Dienste (MPEG-Strich, HLS, interpolierten Streaming, HDS) unterstützten Formate ohne erneut Verpacken in den folgenden Formaten streaming streaming vorführen. Es empfiehlt sich, Inhalt mit dynamischen Verpackung vorführen.

Um dynamische Verpackung nutzen zu können, müssen Sie die folgenden Aktionen ausführen:

- Codieren Sie Ihre Datei Mezzanine (Quelle) in eine Reihe von adaptive Bitrate MP4-Dateien oder adaptive Bitrate interpolierten Streaming-Dateien an.
- Holen Sie mindestens eine bei Bedarf streaming Einheit für den streaming Endpunkt von Inhalten vorführen möchten. Weitere Informationen finden Sie unter [So bei Bedarf streaming reservierte Einheiten skalieren](media-services-portal-manage-streaming-endpoints.md).

Mit dynamischen Verpackung, speichern und die Dateien in den einzelnen Speicherformat bezahlen. Media-Dienste erstellen und die entsprechende Antwort entsprechend Ihrer Anfragen dienen.

Zusätzlich zum Bereitstellen des Zugriffs auf dynamische Verpackung Funktionen, geben Sie bei Bedarf streaming reservierte Einheiten mit dedizierten Ausgang Kapazität, die in Schritten 200 MB / erworben werden kann. Standardmäßig wird bei Bedarf streaming in einem Modell freigegebene Instanz konfiguriert für welche Server Ressourcen (beispielsweise berechnen oder Ausgang Kapazität) für alle anderen Benutzer freigegeben wurden. Sie können eine bei Bedarf streaming Durchsatz verbessern, indem Sie bei Bedarf streaming reservierte Einheiten erwerben.

Weitere Informationen finden Sie unter [dynamische Verpackung](media-services-dynamic-packaging-overview.md).

## <a name="filters-and-dynamic-manifests"></a>Filter und dynamische Manifeste

Sie können Filter für Ihre Anlagen mit den Diensten von Medien definieren. Diese Filter sind serverseitige Regeln, mit die Hilfe Ihre Kunden kann beispielsweise einen bestimmten Teil eines Videos wiedergeben, oder geben Sie eine Teilmenge der Audio- und Formatvarianten, die Ihre Kunden Gerät kann (und nicht alle Formatvarianten, die der Anlage zugeordnet sind) behandelt. Diese Filterung erfolgt durch *dynamische Manifeste* , erstellt werden, wenn Ihr Kunde anfordert, ausgehend von einem oder mehreren angegebenen Filtern Video streamen.

Weitere Informationen finden Sie unter [Filter und dynamische Manifeste](media-services-dynamic-manifest-overview.md).

## <a name="locators"></a>Locator

Wenn Sie Ihre Benutzer mit einer URL angeben, das zum übertragen oder Herunterladen von Inhalten verwendet werden können, müssen Sie zunächst Ihre Anlage durch Erstellen eines Locators veröffentlichen. Ein Locator bietet einen Einstiegspunkt in einer Anlage enthaltenen Dateien zugreifen. Media Services unterstützt zwei Arten von Locator:

- OnDemandOrigin Locator. Diese werden verwendet, um Medien (z. B. MPEG-Strich, HLS oder interpolierten Streaming) oder schrittweise Dateien herunterladen.
-  Freigegebene Access Signatur (SAS) URL Locator. Diese werden verwendet, um Mediendateien auf Ihrem lokalen Computer herunterzuladen.

Eine *Zugriffsrichtlinie* wird verwendet, um die Berechtigungen (z. B. lesen, schreiben und Liste) definieren und für die weist ein Client Access für eine bestimmte Dauer. Beachten Sie, dass die Berechtigung (AccessPermissions.List) in einer OrDemandOrigin Locator erstellen nicht verwendet werden soll.

Locator haben Ablaufdatum. Azure-Portal legt ein Ablaufdatum in der Zukunft 100 Jahre für Locator.

>[AZURE.NOTE] Wenn Sie das Azure-Portal um zu erstellen, bevor Sie März 2015 Locator verwendet, wurden diese Locator nach zwei Jahren Gültigkeitsdauer festgelegt.

Verwenden Sie zum Aktualisieren einer Locator ein Ablaufdatum [REST](http://msdn.microsoft.com/library/azure/hh974308.aspx#update_a_locator ) oder [.NET](http://go.microsoft.com/fwlink/?LinkID=533259) APIs aus. Beachten Sie, dass die URL, wenn Sie das Ablaufdatum der SAS Locator aktualisieren, ändert sich.

Locator dienen nicht pro-Steuerung des Benutzerzugriffs verwalten. Sie können verschiedene Zugriffsrechte für einzelne Benutzer mithilfe der Verwaltung digitaler Rechte (Digital Rights Management, DRM) Lösungen verleihen. Weitere Informationen finden Sie unter [Medien sichern](http://msdn.microsoft.com/library/azure/dn282272.aspx).

Wenn Sie einen Locator erstellt haben, können eine 30 Sekunden Verzögerung aufgrund erforderlichen Speicher und die Weitergabe von Prozessen in Azure-Speicher vorhanden sein.


## <a name="adaptive-streaming"></a>Adaptives streaming

Adaptive Bitrate Technologien zulassen Videoplayer Applikationen Netzwerkprobleme ermitteln, und wählen Sie aus mehreren Bitrate angewendet. Beim Netzwerkkommunikation nimmt ab, kann der Client eine niedrigere Bitrate auswählen, damit die Wiedergabe mit der Videoqualität unteren fortgesetzt werden kann. Wie Netzwerkprobleme verbessern kann der Kunden in eine höhere Bitrate mit verbesserte Videoqualität wechseln. Azure Media Services unterstützt die folgenden adaptive Bitrate Technologien: HTTP Live Streaming (HLS), interpolierten Streaming, MPEG-Strich und HDS.

Um Benutzern mit streaming URLs bereitstellen zu können, müssen Sie zuerst einen OnDemandOrigin Locator erstellen. Erstellen des Locators bietet Ihnen den grundlegenden Pfad auf die Anlage, die den Inhalt enthält, die, den Sie übertragen möchten. Um dieses Inhaltstyps übertragen können, müssen Sie jedoch weiteren dieser Pfad ändern. Um eine vollständige URL der streaming Manifestdatei erstellen zu können, müssen Sie Verketten des Locator Pfadwert und das Manifest (filename.ism) Dateinamen ein. Klicken Sie dann fügen Sie **an/Manifest** und eine entsprechende Format (falls erforderlich) auf den Pfad Locator.

>[AZURE.NOTE]Sie können auch den Inhalt über eine SSL-Verbindung übertragen. Dazu sicherzustellen Sie, dass Ihre streaming URLs mit HTTPS beginnen.

Sie können nur über SSL übertragen, wenn der streaming Endpunkt aus dem Sie den Inhalt vorführen nach 10. September 2014 erstellt wurde. Wenn Ihre streaming URLs auf das streaming Endpunkte erstellt nach dem 10. September 2014 basieren, enthält die URL "streaming.mediaservices.windows.net". SSL unterstützt Streaming URLs mit "origin.mediaservices.windows.net" (das alte Format) nicht. Wenn Ihre URL im alten Format ist und kann über SSL übertragen werden soll, erstellen Sie einen neuen streaming Endpunkt. Verwenden Sie URLs basierend auf der neuen streaming Endpunkt Streamen von Inhalten über SSL aus.


## <a name="streaming-url-formats"></a>Streaming URL-Formate

### <a name="mpeg-dash-format"></a>MPEG-Strich-format

{streaming Endpunkt Name-Media-Dienste Konto name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/Manifest(Format=MPD-Time-CSF)



### <a name="apple-http-live-streaming-hls-v4-format"></a>Apple HTTP Live Streaming (HLS) V4-format

{streaming Endpunkt Name-Media-Dienste Konto name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/Manifest(Format=m3u8-AAPL)

### <a name="apple-http-live-streaming-hls-v3-format"></a>Apple HTTP Live Streaming (HLS) V3-format

{streaming Endpunkt Name-Media-Dienste Konto name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl-v3)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/Manifest(Format=m3u8-AAPL-V3)

### <a name="apple-http-live-streaming-hls-format-with-audio-only-filter"></a>Apple HTTP Live Streaming (HLS) Format mit nur-Audio-filter

Standardmäßig sind nur Audio Spuren enthalten, in der HLS Manifest. Dies ist für Apple Store-Zertifizierung für Mobile Netzwerke erforderlich. In diesem Fall ist ein Client verfügt nicht über ausreichend Bandbreite oder über eine Verbindung 2G verbunden ist, wechselt Wiedergabe nur Audio. Auf diese Weise können Inhalte streaming ohne Pufferung beibehalten, aber kein Bild angezeigt wird. In einigen Szenarien kann über nur-Audio-Player Pufferung bevorzugte befinden. Wenn Sie den nur-Audio-Titel entfernen möchten, fügen Sie **nur Audio = falsch** zur URL.

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/Manifest(Format=m3u8-AAPL-v3,Audio-only=false)

Weitere Informationen finden Sie unter [Unterstützung für dynamische Manifest Systeme und HLS Zusatzfunktionen ausgegeben](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/).


### <a name="smooth-streaming-format"></a>Kontinuierliche Streaming format

{streaming Endpunkt Name-Media-Dienste Konto name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

Beispiel:

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/Manifest

### <a name="a-idfmp4v20asmooth-streaming-20-manifest-legacy-manifest"></a><a id="fmp4_v20"></a>Interpolierten Streaming 2.0 Manifest (legacy Manifest)

Standardmäßig enthält Manifestformat interpolierten Streaming die wiederholen Kategorie (R-Tag). Einige Spieler unterstützt des R-Tags jedoch nicht. Mit diesen Spieler Clients können ein Format verwenden, die das R-Tag deaktiviert:

{streaming Endpunkt Name-Media-Dienste Konto name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=fmp4-v20)

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=fmp4-v20)

### <a name="hds-for-adobe-primetimeaccess-licensees-only"></a>HDS (für nur Adobe vorzeigbare/Access Lizenznehmern)

{streaming Endpunkt Name-Media-Dienste Konto name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=f4m-f4f)

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=f4m-f4f)

## <a name="progressive-download"></a>Schrittweisen herunterladen

Mit den schrittweisen herunterladen können Sie beginnen, Medien wiedergegeben werden, bevor die gesamte Datei heruntergeladen wurde. Sie können keine schrittweise .ism * (Ismv, Isma, Ismt oder Ismc) Dateien herunterladen.

Wenn schrittweise Inhalt herunterladen möchten, verwenden Sie die OnDemandOrigin Locator. Im folgenden Beispiel wird die URL, die auf dem OnDemandOrigin von Locator basiert:

    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4

Sie müssen alle Anlagen Speicher verschlüsselt entschlüsseln, die Sie aus dem Dienst Origin zum schrittweisen Download übertragen möchten.

## <a name="download"></a>Herunterladen

Um den Inhalt zu einem Clientgerät herunterladen möchten, müssen Sie ein SAS Locator erstellen. Der SAS Locator erhalten Sie zentralen Zugriff auf den Container Azure-Speicher, in dem die Datei befindet. Um den Download-URL zu erstellen, müssen Sie den Dateinamen zwischen Host und Signatur SAS-einbetten.

Im folgenden Beispiel wird die URL, die im Locator SAS basiert:

    https://test001.blob.core.windows.net/asset-ca7a4c3f-9eb5-4fd8-a898-459cb17761bd/BigBuckBunny.mp4?sv=2012-02-12&se=2014-05-03T01%3A23%3A50Z&sr=c&si=7c093e7c-7dab-45b4-beb4-2bfdff764bb5&sig=msEHP90c6JHXEOtTyIWqD7xio91GtVg0UIzjdpFscHk%3D

Folgendes gilt:

- Sie müssen alle Anlagen Speicher verschlüsselt entschlüsseln, die Sie aus dem Dienst Origin zum schrittweisen Download übertragen möchten.
- Download, der nicht innerhalb von 12 Stunden abgeschlossen ist, schlägt fehl.

## <a name="streaming-endpoints"></a>Streaming Endpunkte

Ein Endpunkt streaming stellt einen streaming-Dienst, mit der Inhalt direkt auf eine Player-Clientanwendung oder im Netzwerk Bereitstellung von Inhalten (CDN) für die weitere Verteilung vorführen kann. Ausgehende Streams aus einem streaming Endpunkt Dienst kann live-Streams oder einer Demand-Objekt in Ihr Konto Media-Dienste. Sie können auch die Kapazität des Diensts streaming Endpunkt wachsende Bandbreite Anforderungen anpassen streaming reservierte Einheiten zu behandeln steuern. Sie sollten Sie mindestens eine reservierte Einheit für Applikationen in einer Umgebung für die Herstellung zuweisen. Weitere Informationen finden Sie unter [So skalieren Media-Dienst](media-services-portal-manage-streaming-endpoints.md).

## <a name="known-issues"></a>Bekannte Probleme

### <a name="changes-to-smooth-streaming-manifest-version"></a>Änderungen an interpolierten Streaming manifest version

Vor der Freigabe des Juli 2016 Service – wenn Posten von Media Encoder Standard erzeugt Media Encoder Premium Workflow oder dem früheren Azure Media Encoder wurden gestreamt mithilfe von dynamischen Bauweise – das Streaming interpolierten Manifest zurückgegeben würde Version 2.0 entsprechen. In Version 2.0 verwenden Sie die Dauer Fragment nicht die Tags so genannte wiederholen (R). Beispiel:

<?xml version="1.0" encoding="UTF-8"?>
    <SmoothStreamingMedia MajorVersion="2" MinorVersion="0" Duration="8000" TimeScale="1000">
        <StreamIndex Chunks="4" Type="video" Url="QualityLevels({bitrate})/Fragments(video={start time})" QualityLevels="3" Subtype="" Name="video" TimeScale="1000">
            <QualityLevel Index="0" Bitrate="1000000" FourCC="AVC1" MaxWidth="640" MaxHeight="360" CodecPrivateData="00000001674D4029965201405FF2E02A100000030010000003032E0A000F42400040167F18E3050007A12000200B3F8C70ED0B16890000000168EB7352" />
            <c t="0" d="2000" n="0" />
            <c d="2000" />
            <c d="2000" />
            <c d="2000" />
        </StreamIndex>
    </SmoothStreamingMedia>

In der Version Juli 2016 Dienst entspricht generierten interpolierten Streaming Manifests Version 2.2, Fragment Dauer mithilfe von Kategorien wiederholen. Beispiel:

    <?xml version="1.0" encoding="UTF-8"?>
    <SmoothStreamingMedia MajorVersion="2" MinorVersion="2" Duration="8000" TimeScale="1000">
        <StreamIndex Chunks="4" Type="video" Url="QualityLevels({bitrate})/Fragments(video={start time})" QualityLevels="3" Subtype="" Name="video" TimeScale="1000">
            <QualityLevel Index="0" Bitrate="1000000" FourCC="AVC1" MaxWidth="640" MaxHeight="360" CodecPrivateData="00000001674D4029965201405FF2E02A100000030010000003032E0A000F42400040167F18E3050007A12000200B3F8C70ED0B16890000000168EB7352" />
            <c t="0" d="2000" r="4" />
        </StreamIndex>
    </SmoothStreamingMedia>

Einige der Clients älterer Versionen interpolierten Streaming unterstützt möglicherweise nicht die wiederholen Tags und nicht das Manifest geladen. Um dieses Problem zu verringern, können Sie den Parameter legacy Manifestformat **(Format = fmp4-v20)** oder aktualisieren Sie Ihren Kunden auf die neueste Version, die wiederholen Tags unterstützt. Weitere Informationen finden Sie unter [interpolierten Streaming 2.0](media-services-deliver-content-overview.md#fmp4_v20).

## <a name="media-services-learning-paths"></a>Media-Dienste Learning Wege

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-topics"></a>Verwandte Themen

[Aktualisieren von Media-Dienste Locator nach parallelen Speicher Tasten](media-services-roll-storage-access-keys.md)

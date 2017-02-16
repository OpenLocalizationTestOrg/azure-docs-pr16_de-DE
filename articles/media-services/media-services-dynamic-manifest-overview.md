<properties 
    pageTitle="Filter und dynamische Manifeste | Microsoft Azure" 
    description="In diesem Thema beschreibt das Erstellen von Filtern, sodass sie Ihren Kunden auf bestimmte Bereiche eines Streams Stream verwenden kann. Media-Dienste erstellt dynamische Manifeste zum selektiven streaming archivieren." 
    services="media-services" 
    documentationCenter="" 
    authors="cenkdin" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ne" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="cenkdin;juliako"/>

# <a name="filters-and-dynamic-manifests"></a>Filter und dynamische Manifeste

Ab Version 2.11 können Media-Dienste Sie zum Definieren von Filtern für Ihre Anlagen. Diese Filter sind serverseitige Regeln, die Ihre Kunden auswählen, wie Dinge zu ermöglichen: Wiedergabe nur einen Teil eines Videos (statt wiedergeben das gesamte Video), oder geben Sie nur eine Teilmenge der Audio- und Formatvarianten, die Ihre Kunden Gerät kann (und nicht alle Formatvarianten, die der Anlage zugeordnet sind) behandelt. Diese Filterung der Datenbestände wird archiviert durch **Dynamische Manifest**s, die auf Ihre Kunden verlangen ein Videos auf der Grundlage der angegebenen Filter streamen erstellt werden.

Diese Themen behandelt allgemeine Szenarien, in denen mit Filter wäre sehr hilfreich, um Ihre Kunden und Links zu Themen, die veranschaulichen, wie Filter programmgesteuert erstellt (aktuell, Sie können Filter erstellen mit REST-APIs nur).

##<a name="overview"></a>(Übersicht)

Beim Übermitteln von Inhalten für Kunden (streaming live Ereignisse oder Demand) ist das Ziel ein Videos hoher Qualität verschiedenen Geräten unter anderen Netzwerkprobleme vorführen. Erzielen Sie diese Zielsetzung gehen Sie folgendermaßen vor:

- Codieren von Ihrer Stream zu Multi-Bitrate ([adaptive Bitrate](http://en.wikipedia.org/wiki/Adaptive_bitrate_streaming)) Videodatenstrom (dies übernimmt der Qualität und Netzwerk Konditionen) und 
- Verwenden Sie Media-Dienste [Dynamische Verpackung](media-services-dynamic-packaging-overview.md) dynamisch Ihre Stream unterschiedliche Protokolle erneut Verpacken (dies übernimmt streaming auf verschiedene Geräte). Media-Dienste unterstützt die folgenden adaptive Bitrate streaming Technologien Übermittlung: HTTP Live Streaming (HLS), interpolierten Streaming, MPEG Gedankenstrich und HDS (für nur Adobe vorzeigbare/Access Lizenznehmern). 

###<a name="manifest-files"></a>Manifestdateien 

Eine Datei **manifest** (Wiedergabeliste) wird erstellt, wenn Sie eine Anlage für das streaming adaptive Bitrate codieren, (die Datei ist textbasierten oder XML-basierten). **Die Manifestdatei** enthält Metadaten wie streaming: Typ (Audio, Video oder Text) nachverfolgen, Name, Start-und Endzeit, Bitrate (Merkmale) nachverfolgen Sprachen, Nachverfolgen Präsentationsfenster (verschiebbaren Fenster feste Dauer), video-Codec (FourCC). Es weist auch im Player auf das nächste Fragment abrufen, indem Sie Informationen über die nächsten wiedergegeben werden können video Fragmente verfügbar und deren Speicherort bereitgestellt. Satzteile (oder Segmente) werden die tatsächlichen "Blöcken" eine Videoinhalt.


Hier ist ein Beispiel für eine Manifestdatei ein: 

    
    <?xml version="1.0" encoding="UTF-8"?>  
    <SmoothStreamingMedia MajorVersion="2" MinorVersion="0" Duration="330187755" TimeScale="10000000">
    
    <StreamIndex Chunks="17" Type="video" Url="QualityLevels({bitrate})/Fragments(video={start time})" QualityLevels="8">
    <QualityLevel Index="0" Bitrate="5860941" FourCC="H264" MaxWidth="1920" MaxHeight="1080" CodecPrivateData="0000000167640028AC2CA501E0089F97015202020280000003008000001931300016E360000E4E1FF8C7076850A4580000000168E9093525" />
    <QualityLevel Index="1" Bitrate="4602724" FourCC="H264" MaxWidth="1920" MaxHeight="1080" CodecPrivateData="0000000167640028AC2CA501E0089F97015202020280000003008000001931100011EDC00002CD29FF8C7076850A45800000000168E9093525" />
    <QualityLevel Index="2" Bitrate="3319311" FourCC="H264" MaxWidth="1280" MaxHeight="720" CodecPrivateData="000000016764001FAC2CA5014016EC054808080A00000300020000030064C0800067C28000103667F8C7076850A4580000000168E9093525" />
    <QualityLevel Index="3" Bitrate="2195119" FourCC="H264" MaxWidth="960" MaxHeight="540" CodecPrivateData="000000016764001FAC2CA503C045FBC054808080A000000300200000064C1000044AA0000ABA9FE31C1DA14291600000000168E9093525" />
    <QualityLevel Index="4" Bitrate="1469881" FourCC="H264" MaxWidth="960" MaxHeight="540" CodecPrivateData="000000016764001FAC2CA503C045FBC054808080A000000300200000064C04000B71A0000E4E1FF8C7076850A4580000000168E9093525" />
    <QualityLevel Index="5" Bitrate="978815" FourCC="H264" MaxWidth="640" MaxHeight="360" CodecPrivateData="000000016764001EAC2CA50280BFE5C0548303032000000300200000064C08001E8480004C4B7F8C7076850A45800000000168E9093525" />
    <QualityLevel Index="6" Bitrate="638374" FourCC="H264" MaxWidth="640" MaxHeight="360" CodecPrivateData="000000016764001EAC2CA50280BFE5C0548303032000000300200000064C080013D60000C65DFE31C1DA1429160000000168E9093525" />
    <QualityLevel Index="7" Bitrate="388851" FourCC="H264" MaxWidth="320" MaxHeight="180" CodecPrivateData="000000016764000DAC2CA505067E7C054830303200000300020000030064C040030D40003D093F8C7076850A45800000000168E9093525" />
    
    <c t="0" d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="9600000"/>
    </StreamIndex>
    
    
    <StreamIndex Chunks="17" Type="audio" Url="QualityLevels({bitrate})/Fragments(AAC_und_ch2_128kbps={start time})" QualityLevels="1" Name="AAC_und_ch2_128kbps">
    <QualityLevel AudioTag="255" Index="0" BitsPerSample="16" Bitrate="125658" FourCC="AACL" CodecPrivateData="1210" Channels="2" PacketSize="4" SamplingRate="44100" />
    
    <c t="0" d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="6965987" /></StreamIndex>
    
    
    <StreamIndex Chunks="17" Type="audio" Url="QualityLevels({bitrate})/Fragments(AAC_und_ch2_56kbps={start time})" QualityLevels="1" Name="AAC_und_ch2_56kbps">
    <QualityLevel AudioTag="255" Index="0" BitsPerSample="16" Bitrate="53655" FourCC="AACL" CodecPrivateData="1210" Channels="2" PacketSize="4" SamplingRate="44100" />
    
    <c t="0" d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="6965987" /></StreamIndex>
    
    </SmoothStreamingMedia>
    
###<a name="dynamic-manifests"></a>Dynamische Manifeste

Es gibt [Szenarien](media-services-dynamic-manifest-overview.md#scenarios) bei Ihren Kunden benötigt mehr Flexibilität als was der Standard-Anlage Manifestdatei beschrieben ist. Beispiel:

- Bestimmte Geräte: vorführen nur die angegebenen Formatvarianten Sperren Sprachspuren angegeben, die vom Gerät unterstützt werden, die mit der Wiedergabe des Inhalts ("Formatvariante filtern"). 
- Verringern Sie das Manifest, um einen unter-Clip eines Ereignisses live ("unter-Clip filtern") anzuzeigen.
- Kürzen Sie Starten eines Videos ("Video kürzen").
- Passen Sie Präsentation Fenster (DVR), um bereitstellen eine begrenzte Länge des Fensters DVR im Player ("Anpassen Präsentationsfenster").
 
Um diese Flexibilität zu erreichen, bietet Media-Dienste **Dynamische-Manifeste** basierend auf vordefinierte [Filter](media-services-dynamic-manifest-overview.md#filters).  Nachdem Sie die Filter definieren, konnte Kunden sie verwenden, um eine bestimmte Formatvariante oder unter-Clips des Videos übertragen. Sie möchten Filter im streaming URL angeben. Filter konnte auf adaptive streaming von [Dynamischen Verpackung](media-services-dynamic-packaging-overview.md)unterstützten Protokolle Bitrate angewendet werden: HLS, MPEG-Strich, interpolierten Streaming und HDS. Beispiel:

MPEG Gedankenstrich URL mit filter

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf,filter=MyLocalFilter)

Interpolierten Streaming URL mit filter

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=MyLocalFilter)


Weitere Informationen zum Übermitteln von Inhalten und Erstellen von URLs streaming finden Sie unter [Übersicht über die Übermittlung von Inhalten](media-services-deliver-content-overview.md).


>[AZURE.NOTE]Beachten Sie, dass dynamische-Manifeste nicht die Anlage und das Standardmanifest für die Anlage zu ändern. Anfordern eines Streams mit oder ohne Filter kann Ihren Kunden auswählen. 


###<a name="a-idfiltersafilters"></a><a id="filters"></a>Filter 

Es gibt zwei Arten von Objekt filtern: 

- Allgemeine Filter (kann auf einem beliebigen Objekt in das Konto Azure Media Services angewendet werden, haben Sie die Gültigkeitsdauer des Kontos) und 
- Lokale Filter (kann nur angewendet werden, um eine Anlage mit dem der Filter bei der Erstellung zugeordnet wurde, eine Nutzungsdauer eines Wirtschaftsguts haben). 

Globale und lokale Filtertypen haben dieselben Eigenschaften. Der wichtigste Unterschied zwischen den beiden ist für welche Szenarios welche Art von einem File-Server besser geeignet ist. Allgemeine Filter eignen sich in der Regel für Geräteprofile (Formatvariante Filtern), in dem konnte lokale Filter verwendet werden, um eine spezielle Ressource kürzen.


##<a name="a-idscenariosacommon-scenarios"></a><a id="scenarios"></a>Häufige Szenarien 

Wie bereits erwähnt wurde beim Übermitteln von Inhalten für Kunden (streaming live Ereignisse oder Demand) Ihr Ziel ist ein Video hoher Qualität verschiedenen Geräten unter anderen Netzwerkprobleme vorführen. Darüber hinaus werden Ihre möglicherweise haben andere Anforderungen, die Ihre Bestände jederzeit filtern und Verwenden von **Dynamischen Manifest**s betreffen. Geben Sie einen kurzen Überblick über verschiedene Filterung Szenarien in den folgenden Abschnitten.

- Geben Sie nur eine Teilmenge der Audio- und Formatvarianten, die bestimmte Geräte können (und nicht alle Formatvarianten, die der Anlage zugeordnet sind) behandelt. 
- Wiedergeben von nur einen Teil eines Videos (anstelle der Wiedergabe des gesamten Videos).
- Passen Sie DVR Präsentationsfenster.

##<a name="rendition-filtering"></a>Filtern einer Formatvariante eines Bilds 

Sie können Ihre Anlage mehrere Codierung Profile (h. 264 Basisplan, h. 264 hoch, AACL, AACH, Dolby Digital Plus-) und mehrere Qualität eine Bitrate codieren. Nicht alle Client-Geräte unterstützt jedoch alle Ihre Anlage Profile und eine Bitrate. Ältere Android-Geräten unterstützt beispielsweise nur h. 264 geplante + AACL. Senden eine höhere Bitrate an ein Gerät die Vorteile erreicht werden kann, verschwendet Bandbreite und Gerät Berechnung. Solche Gerät muss alle angegebene Informationen nur zu verkleinern, für die Anzeige entschlüsseln.

Sie können mit dynamischen Manifest Geräteprofile erstellen wie Mobile console, HD/SD usw. und enthalten die Spuren und Eigenschaften, die einen Teil der einzelnen Profile werden soll.

 
![Formatvariante Filterung Beispiel][renditions2]

Im folgenden Beispiel wurde ein Encoder verwendet, um eine Anlage Mezzanine in sieben ISO MP4s video Formatvarianten (von 180 p, um 1080p) codieren. Codierte Anlage dynamisch in eines der folgenden streaming Protokolle verpackt werden kann: HLS, Weiche, MPEG Gedankenstrich und HDS.  Am oberen Rand des Diagramms, das HLS Manifest für die Anlage ohne Filter dargestellt ist (er enthält alle sieben Formatvarianten).  In der unteren linken werden HLS Manifest, das ein Filter mit dem Namen "Ott" angewendet wurde. Der Filter "Ott" gibt an, um alle eine Bitrate unter 1 Mbit/s, zu entfernen, sodass er entfernt, in der Antwort unten zwei Qualität Ebenen geführt haben.  In der unteren rechten werden HLS Manifest, das ein Filter mit der Bezeichnung "mobile" angewendet wurde. Der Filter "mobile" gibt an, um Formatvarianten entfernen, wo finde ich die Auflösung größer als 720p geführt den beiden hat, 1080p Formatvarianten entfernt wird.

![Filtern einer Formatvariante eines Bilds][renditions1]

##<a name="removing-language-tracks"></a>Entfernen von Sprachspuren

Ihre Bestände jederzeit enthalten möglicherweise mehrere audio Sprachen wie Englisch, Spanisch, Französisch. In der Regel Player SDK Vorgesetzten Standard audio nachverfolgen Auswahl und verfügbare Audio verfolgt pro Benutzerauswahl. Es ist eine Herausforderung solche Player SDKs entwickeln möchte, es andere Implementierungen über Geräte-spezifischen Player-Framework erfordert. Darüber hinaus auf manchen Plattformen Player APIs maximal und audio Auswahlfeature, in dem Benutzer nicht auswählen oder ändern die Standard-audio-Spur, nicht beifügen. Mit Anlage filtern können Sie das Verhalten durch Erstellen von Filtern, die nur die gewünschte audio Sprachen enthalten steuern.

![Sprachspuren filtern][language_filter]


##<a name="trimming-start-of-an-asset"></a>Starten einer Anlage Zuschneiden 

Führen Sie in den meisten live streaming Ereignisse einige Tests vor dem Ereignis ist-Operatoren. Angenommen, sie einem Slate wie folgt vor dem Start des Ereignisses umfassen möglicherweise: "Programm wird kurzzeitig gestartet". Wenn das Programm Archivierung ist, Test- und Blattes Daten auch archiviert werden und werden in der Präsentation enthalten sein. Diese Informationen sollte jedoch nicht auf die Clients angezeigt werden. Mit dynamischen Manifest können Erstellen einer Zeitfilter Start- und die unerwünschten Daten aus dem Manifest entfernen.

![Zuschneiden starten][trim_filter]

##<a name="creating-sub-clips-views-from-a-live-archive"></a>Erstellen von unter-Clips (Ansichten) aus einem Archiv live

Viele live Ereignisse sind lang ausgeführt und live Archiv möglicherweise mehrere Ereignisse sind. Nach dem live Ereignis endet Sendeanstalten möchten möglicherweise unterbrechen live Archiv in logischen Programm starten und beenden folgen. Klicken Sie dann veröffentlichen Sie diese virtuelle Programme separat ohne Beitrag Verarbeitung live-Archiv und nicht erstellen eigenständige Vermögenswerte (die nicht von der vorhandenen Cache Fragmenten in der CDNs nutzen können). Beispiele für solche virtuelle Programme (unter-Clips) werden die Quartale eines Football oder Basketball Spiel, die Innings in Baseball oder einzelne Ereignisse von einem Tag der Olympischen Spiele-Programm.

Mit dynamischen Manifest können mithilfe von Anfangs-/Ende-Zeiten Filter erstellen und im oberen Bereich der live-Archiv virtuelle Ansichten erstellen. 

![Clipkopie filter][subclip_filter]

Gefilterte Objekt:

![Skifahren][skiing]

##<a name="adjusting-presentation-window-dvr"></a>Anpassen der Präsentationsfenster (DVR)

Azure Media Services bietet derzeit kreisförmiger archivieren, die Dauer zwischen 5 Minuten – 25 Stunden konfiguriert werden kann. Manifest filtern kann, ein paralleles DVR-Fenster über den oberen Rand der Archiv zu erstellen, ohne das Löschen von Medien verwendet werden. Es gibt viele Szenarios Sendeanstalten möchten eine begrenzte DVR-Fenster angeben, welche verschiebt am Rand live und zur gleichen Zeit ein vergrößern Archivierung Fenster beibehalten. Ein Sender die Daten zu verwenden, die aus dem Fenster DVR zum Hervorheben von Clips heraus ist möchten oder He\she anderen DVR-Fenster für verschiedene Geräte bieten möchten. Beispielsweise nicht für die meisten mobilen Geräte große DVR Windows behandeln (Sie können ein 2 Minute DVR-Fenster für mobile Geräte und 1 Stunde für desktop-Clients haben).

![DVR-Fenster][dvr_filter]

##<a name="adjusting-livebackoff-live-position"></a>Anpassen der LiveBackoff (live Position)

Manifest Filterung kann einige Sekunden vom live Rand live Programm entfernen verwendet werden. Dadurch wird die Sendeanstalten schauen Sie sich die Präsentation, zeigen Sie im Vorschau-Publikation und Ankündigung Einfügemarke Punkte erstellen, bevor die Viewer Streams (normalerweise gesicherten-Deaktivieren von 30 Sekunden) erhalten. Sendeanstalten können dann drücken Sie diese Werbung an ihre Kunden Framework Zeitpunkt für diese empfangenen und verarbeiten die Informationen, bevor die Verkaufschance Ankündigung.

Neben dem Support Ankündigung kann LiveBackoff für Client live Download Position anpassen, damit beim Clients wandert, und drücken Sie die live Kante weiterhin Satzteile vom Server statt erste 404 oder 412 HTTP-Fehler ausgegeben können verwendet werden.

![livebackoff_filter][livebackoff_filter]


##<a name="combining-multiple-rules-in-a-single-filter"></a>Kombinieren mehrerer Regeln in einem einzelnen filter

Sie können mehrere Filterung Regeln in einem einzelnen Filter kombinieren. Beispielsweise können Sie eine Regel Bereich Slate aus einem Archiv live entfernen und Filtern Sie auch eine verfügbare Bitrate definieren. Für mehrere Filterung Regeln ist das Ergebnis die Bestandteile (nur Schnittpunkt) dieser Regeln.

![Vielfache-Regeln][multiple-rules]

##<a name="create-filters-programmatically"></a>Programmgesteuertes Erstellen von Filtern

Im folgende Thema wird erläutert, Media-Dienste von Personen, die mit Filtern verknüpft sind. Das Thema wird gezeigt, wie Filter zu erstellen.  

[Erstellen von Filtern mit REST-APIs](media-services-rest-dynamic-manifest.md).

## <a name="combining-multiple-filters-filter-composition"></a>Kombinieren mehrerer Filter (Filter Bearbeitung)

Sie können auch mehrere Filter in einer einzelnen URL kombinieren. 

Das folgende Szenario veranschaulicht, warum Sie möglicherweise Filter kombinieren möchten:

1. Sie müssen Ihre video Merkmale für mobile Geräte wie Android oder iPAD Filtern (um video Merkmale zu beschränken). Wenn die unerwünschten Merkmale entfernen möchten, erstellen Sie einen globalen Filter, der für Geräteprofile geeignet ist. Wie zuvor erwähnt, können allgemeine Filter für alle Anlagen unter demselben Media Services Konto ohne weiteren Association verwendet werden. 
2. Sie möchten die Start- und Anzeigedauer eines Wirtschaftsguts kürzen. Um dies zu erreichen, möchten Sie einen lokalen Filter erstellen, und legen Sie die Anfangs-/Ende-Zeit. 
3. Kombinieren Sie die beiden folgenden Filter (ohne Kombination müssen Sie würde hinzufügen Qualität Filtern zum Verkürzen Filter die Verwendung der Filters erschweren) werden soll.

Um Filter zu kombinieren, müssen Sie die Filternamen hinzu Manifest/festlegen URL mit durch Semikolons getrennt. Angenommen, Sie haben einen Filter mit dem Namen *MyMobileDevice* Filter Merkmale und Sie haben eine andere benannte *MyStartTime* auf eine bestimmte Startzeit festlegen. Sie können diese kombinieren, wie folgt:

    http://teststreaming.streaming.mediaservices.windows.net/3d56a4d-b71d-489b-854f-1d67c0596966/64ff1f89-b430-43f8-87dd-56c87b7bd9e2.ism/Manifest(filter=MyMobileDevice;MyStartTime)

Sie können bis zu 3 Filter kombinieren. 

Weitere Informationen finden Sie in [diesem](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/) Blog.


##<a name="know-issues-and-limitations"></a>Wissen, Probleme und Einschränkungen

- Dynamische Manifest arbeitet in GOP Begrenzung (Taste Frames) daher kürzen weist GOP Genauigkeit. 
- Sie können die gleichen Filtername für lokale und globale Filter verwenden. Beachten Sie den lokalen Filter haben Vorrang und überschreibt globale Filter.
- Wenn Sie einen Filter aktualisieren, kann für die Regeln aktualisieren streaming Endpunkt bis zu 2 Minuten dauern. Wenn der Inhalt wurde served Verwendung bestimmter Filter (und Cache in Proxys und CDN Caches), kann diese Filter aktualisieren Player Fehlern bei führen. Es wird empfohlen, den Cache zu leeren, nach dem Aktualisieren des Filters. Wenn diese Option nicht möglich ist, sollten erwägen Sie, einen anderen Filter zu verwenden.


##<a name="media-services-learning-paths"></a>Media-Dienste Learning Wege

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



##<a name="see-also"></a>Siehe auch

[Bereitstellung von Inhalten für Kunden (Übersicht)](media-services-deliver-content-overview.md)

[renditions1]: ./media/media-services-dynamic-manifest-overview/media-services-rendition-filter.png
[renditions2]: ./media/media-services-dynamic-manifest-overview/media-services-rendition-filter2.png

[rendered_subclip]: ./media/media-services-dynamic-manifests/media-services-rendered-subclip.png
[timeline_trim_event]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-event.png
[timeline_trim_subclip]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-subclip.png

[multiple-rules]:./media/media-services-dynamic-manifest-overview/media-services-multiple-rules-filters.png

[subclip_filter]: ./media/media-services-dynamic-manifest-overview/media-services-subclips-filter.png
[trim_event]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-event.png
[trim_subclip]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-subclip.png
[trim_filter]: ./media/media-services-dynamic-manifest-overview/media-services-trim-filter.png
[redered_subclip]: ./media/media-services-dynamic-manifests/media-services-rendered-subclip.png
[livebackoff_filter]: ./media/media-services-dynamic-manifest-overview/media-services-livebackoff-filter.png
[language_filter]: ./media/media-services-dynamic-manifest-overview/media-services-language-filter.png
[dvr_filter]: ./media/media-services-dynamic-manifest-overview/media-services-dvr-filter.png
[skiing]: ./media/media-services-dynamic-manifest-overview/media-services-skiing.png
 
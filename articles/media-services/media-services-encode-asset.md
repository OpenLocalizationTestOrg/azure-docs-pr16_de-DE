<properties 
    pageTitle="Übersicht und Vergleich von Azure auf Demand Media Encoder | Microsoft Azure" 
    description="Dieses Thema bietet einen Überblick und Vergleich von Azure auf Demand Media Encoder." 
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
    ms.author="juliako"/>

#<a name="overview-and-comparison-of-azure-on-demand-media-encoders"></a>Übersicht und Vergleich von Azure auf Demand Media Encoder

##<a name="encoding-overview"></a>Codierung (Übersicht)

Azure Media Services bietet mehrere Optionen für die Codierung von Medien in der Cloud.

Beim Starten von mit den Diensten von Medien ab, ist es wichtig, den Unterschied zwischen den Formaten Codecs und Datei zu verstehen.
Codecs sind die Software, die die Komprimierung/ursprünglich Algorithmen implementiert während Dateiformate Container sind, die das komprimierte Video zu halten.

Media Services bietet dynamische Verpacken, dem Sie Ihre adaptive Bitrate MP4 oder interpolierten Streaming codierte Inhalt von Media-Dienste (MPEG Gedankenstrich HLS, interpolierten Streaming, HDS) unterstützten Formate streaming vorführen kann ohne dass Sie in den folgenden Formaten streaming erneut verpacken.

Um [dynamische Verpackung](media-services-dynamic-packaging-overview.md)nutzen zu können, müssen Sie die folgenden Aktionen ausführen:

- Codieren der Datei Mezzanine (Quelle) in eine Reihe von adaptive Bitrate MP4-Dateien oder adaptive Bitrate interpolierten Streaming-Dateien (die Codierung Schritte sind weiter unten in diesem Lernprogramm gezeigt).
- Holen Sie mindestens eine bei Bedarf streaming Einheit für den streaming Endpunkt, aus dem Sie bis zur Bereitstellung des Inhalts planen. Weitere Informationen finden Sie unter [So skalieren bei Bedarf Streaming reservierte Einheiten](media-services-portal-manage-streaming-endpoints.md).

Media-Dienste unterstützt die folgenden auf Demand Encoder, die in diesem Artikel beschrieben sind:

- [Media Encoder Standard](media-services-encode-asset.md#media-encoder-standard)
- [Media Encoder Premium Workflow](media-services-encode-asset.md#media-encoder-premium-workflow)

Dieser Artikel bietet einen kurzen Überblick bei Bedarf Media Encoder und enthält Links zu Artikeln, die ausführlichen Informationen zu verleihen. Das Thema enthält auch Vergleich der der Encoder.

Beachten Sie, dass standardmäßig jedes Konto Media-Dienste einen aktive Codierung Aufgabe gleichzeitige Kontoverbindungen verwenden kann. Sie können die Codierung Einheiten reservieren, mit denen Sie mehrere Codierung Aufgaben, die gleichzeitig ausgeführt, eine für jede Codierung reservierte Einheit, die Sie erworben haben. Informationen hierzu finden Sie unter [Skalierung Codierung Einheiten](media-services-scale-media-processing-overview.md).

##<a name="media-encoder-standard"></a>Media Encoder Standard

###<a name="how-to-use"></a>So verwenden Sie

[So codieren mit Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md)

###<a name="formats"></a>Formate

[Formate und codecs](media-services-media-encoder-standard-formats.md)

###<a name="presets"></a>Voreinstellungen

Media Encoder Standard ist so konfiguriert, dass mithilfe einer der der Encoder Voreinstellungen beschrieben [hier](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).

###<a name="input-and-output-metadata"></a>Eingabe- und Metadaten

Die Eingabewerte Encoder Metadaten beschrieben wird [hier](http://msdn.microsoft.com/library/azure/dn783120.aspx).

Die Encoder Ausgabe Metadaten beschrieben wird [hier](http://msdn.microsoft.com/library/azure/dn783217.aspx).

###<a name="generate-thumbnails"></a>Generieren von Miniaturansichten

Informationen finden Sie unter [wie Miniaturansichten mit Media Encoder Standard generiert](media-services-custom-mes-presets-with-dotnet.md#thumbnails).

###<a name="trim-videos-clipping"></a>Kürzen Sie Videos (eines Bildschirmausschnitts)

Informationen finden Sie unter [zum Kürzen von Videos Media Encoder Standard verwenden](media-services-custom-mes-presets-with-dotnet.md#trim_video).

###<a name="create-overlays"></a>Erstellen von überlagert

Informationen finden Sie unter [So erstellen Sie mithilfe von Media Encoder Standard überlagert](media-services-custom-mes-presets-with-dotnet.md#overlay).

###<a name="see-also"></a>Siehe auch

[Die Media-Dienste-blog](https://azure.microsoft.com/blog/2015/07/16/announcing-the-general-availability-of-media-encoder-standard/)
 
##<a name="media-encoder-premium-workflow"></a>Media Encoder Premium Workflow

###<a name="overview"></a>(Übersicht)

[Einführung in Premium Codierung in Azure Media Services](https://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services/)

###<a name="how-to-use"></a>So verwenden Sie

Media Encoder Premium Workflow wird mithilfe von komplexe Workflows konfiguriert. Workflowdateien erstellt werden konnten und mithilfe des Tools für die [Workflow-Designer](media-services-workflow-designer.md) aktualisiert.

[So Premium Codierung in Azure Media Services verwenden](https://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services/)

###<a name="known-issues"></a>Bekannte Probleme

Wenn Ihre Eingabe Video nicht enthält enthält Untertitel, die Ausgabe Anlage dann immer noch eine leere TTML-Datei. 


##<a name="a-idcompareencodersacompare-encoders"></a><a id="compare_encoders"></a>Vergleichen von Encoder

###<a name="a-idbillingabilling-meter-used-by-each-encoder"></a><a id="billing"></a>Jede Encoder untersuchten Meter Abrechnung

Name der Medien-Prozessor|Geltendes Preise|Notizen
---|---|---
**Media Encoder Standard** |ENCODER|Codieren von Aufgaben an die Größe der Ausgabe belastet Anlage in GB, angegebenen Rate [hier][1], unter der Spalte ENCODER.
**Media Encoder Premium Workflow** |PREMIUM ENCODER|Codieren von Aufgaben an die Größe der Ausgabe belastet Anlage in GB, angegebenen Rate [hier][1], unter der Spalte PREMIUM ENCODER.


In diesem Abschnitt vergleicht die codieren Funktionen von **Media Encoder Standard-** und **Media Encoder Premium Workflow**an.


###<a name="input-containerfile-formats"></a>Eingabemethoden Sie-Formate Container-Datei

Eingabemethoden Sie-Formate Container-Datei|Media Encoder Standard|Media Encoder Premium Workflow
---|---|---
Adobe® Flash® F4V           |Ja|Ja
MXF/SMPTE 377M              |Ja|Ja
GXF                         |Ja|Ja
MPEG-2-Streams    |Ja|Ja
MPEG-2 Programm Streams      |Ja|Ja
MPEG-4/MP4                  |Ja|Ja
Windows Media-ASF           |Ja|Ja
AVI (nicht komprimiert 8 Bit/10 Bit)|Ja|Ja
3GPP/3GPP2                  |Ja|Nein
Interpolierten Streaming-Dateiformat (PIFF 1.3)|Ja|Nein
[Microsoft Digital Video Recording(DVR-MS)](https://msdn.microsoft.com/library/windows/desktop/dd692984)|Ja|Nein
Matroska/WebM               |Ja|Nein
QuickTime (MOV) |Ja|Nein

###<a name="input-video-codecs"></a>Eingabemethoden-Video-Codecs

Eingabemethoden-Video-Codecs|Media Encoder Standard|Media Encoder Premium Workflow
---|---|---
AVC 8-Bit-10-Bit, bis zu 4:2:2, einschließlich AVCIntra   |8-bit-4:2:0 und 4:2:2|Ja
Avid DNxHD (in MXF)                                 |Ja|Ja
DVCPro/DVCProHD (in MXF)                            |Ja|Ja
JPEG 2000                                            |Ja|Ja
MPEG-2 (nach Zeitphasen bis zum 422 Profil- und hoher Ebene, einschließlich Varianten wie XDCAM, XDCAM HD, XDCAM IMX, CableLabs® und D10)|Bis zu 422 Profil|Ja
MPEG-1                                              |Ja|Ja
Windows Media Video/VC-1                            |Ja|Ja
Canopus HQ/HQX                                      |Nein|Nein
MPEG-4 Teil 2                                       |Ja|Nein
[Theora](https://en.wikipedia.org/wiki/Theora)      |Ja|Nein
Apple ProRes 422    |Ja|Nein
Apple ProRes 422 LT |Ja|Nein
Apple ProRes 422 HQ |Ja|Nein
Apple ProRes Proxy|Ja|Nein
Apple ProRes 4444 |Ja|Nein
Apple ProRes 4444 XQ |Ja|Nein

###<a name="input-audio-codecs"></a>Eingabemethoden-Audio-Codecs

Eingabemethoden-Audio-Codecs|Media Encoder Standard|Media Encoder Premium Workflow
---|---|---
AES (SMPTE 331 M und 302 M, AES3-2003)        |Nein|Ja
Dolby® E                                    |Nein|Ja
Dolby® digitalen (AC3)                        |Nein|Ja
Dolby® Digital Plus (E-AC3)                 |Nein|Ja
AAC (AAC-LC, HE-AAC und AAC-HEv2; bis zu 5.1)|Ja|Ja
MPEG Layer 2|Ja|Ja
MP3 (MPEG-1 Audio Layer 3)|Ja|Ja
Windows Media-Audiodatei|Ja|Ja
WAV/PCM|Ja|Ja
[FLAC](https://en.wikipedia.org/wiki/FLAC)</a>|Ja|Nein
[Opus](https://en.wikipedia.org/wiki/Opus_(audio_format)) |Ja|Nein
[Vorbis](https://en.wikipedia.org/wiki/Vorbis)</a>|Ja|Nein


###<a name="output-containerfile-formats"></a>Formate für die Ausgabe Container-Datei

Formate für die Ausgabe Container-Datei|Media Encoder Standard|Media Encoder Premium Workflow
---|---|---
Adobe® Flash® F4V|Nein|Ja
MXF (OP1a, XDCAM und AS02)|Nein|Ja
DPP (einschließlich AS11)|Nein|Ja
GXF|Nein|Ja
MPEG-4/MP4|Ja|Ja
MPEG-TERMINALDIENSTE|Ja|Ja
Windows Media-ASF|Nein|Ja
AVI (nicht komprimiert 8 Bit/10 Bit)|Nein|Ja
Interpolierten Streaming-Dateiformat (PIFF 1.3)|Nein|Ja

###<a name="output-video-codecs"></a>Die Ausgabe Video-Codecs

Die Ausgabe Video-Codecs|Media Encoder Standard|Media Encoder Premium Workflow
---|---|---
AVC (h. 264; 8-Bit-; nach Zeitphasen bis zum Profil hoher Ebene 5.2; 4 K extrem HD; AVC innerhalb)|Nur 8-bit-4:2:0|Ja
Avid DNxHD (in MXF)|Nein|Ja
MPEG-2 (nach Zeitphasen bis zum 422 Profil- und hoher Ebene, einschließlich Varianten wie XDCAM, XDCAM HD, XDCAM IMX, CableLabs® und D10)|Nein|Ja
MPEG-1|Nein|Ja
Windows Media Video/VC-1|Nein|Ja
Erstellen von Miniaturansichten JPEG|Nein|Ja

###<a name="output-audio-codecs"></a>Die Ausgabe-Audio-Codecs

Die Ausgabe-Audio-Codecs|Media Encoder Standard|Media Encoder Premium Workflow
---|---|---
AES (SMPTE 331 M und 302 M, AES3-2003)|Nein|Ja
Dolby® digitalen (AC3)|Nein|Ja
Dolby® Digital Plus-(E-AC3) bis zu 7.1|Nein|Ja
AAC (AAC-LC, HE-AAC und AAC-HEv2; bis zu 5.1)|Ja|Ja
MPEG Layer 2|Nein|Ja
MP3 (MPEG-1 Audio Layer 3)|Nein|Ja
Windows Media-Audiodatei|Nein|Ja


##<a name="error-codes"></a>Fehlercodes  

Die folgende Tabelle enthält die Fehlercodes, die zurückgegeben werden können, für den Fall, dass während der Ausführung der Codierung Vorgang ein Fehler aufgetreten ist.  Verwenden Sie Fehlerdetails in .NET Code, um die [ErrorDetails](http://msdn.microsoft.com/library/microsoft.windowsazure.mediaservices.client.errordetail.aspx) Class. Verwenden Sie Fehlerdetails im restlichen Code, um die [ErrorDetail](https://msdn.microsoft.com/library/jj853026.aspx) REST-API.

ErrorDetail.Code|Mögliche Ursachen für Fehler
-----|-----------------------
Unbekannt| Fehler beim Ausführen der Aufgabe
ErrorDownloadingInputAssetMalformedContent|Kategorie von Fehlern, die Fehler in Herunterladen einer Anlage, z. B. ungültige Dateinamen, 0 (null) Länge-Dateien, die falsche behandelt usw. formatiert.
ErrorDownloadingInputAssetServiceFailure|Kategorie von Fehlern, die auf der Seite ein Service - Beispiel Netzwerk- oder Pfad Fehler beim Herunterladen von Problemen behandelt.
ErrorParsingConfiguration|Kategorie von Fehlern, in dem Vorgang <see cref="MediaTask.PrivateData"/> (Konfiguration) ist ungültig, beispielsweise die Konfiguration ist nicht gültige System voreingestellte oder ungültige XML-Daten enthält.
ErrorExecutingTaskMalformedContent|Kategorie von Fehlern bei der Ausführung des Vorgangs, bei dem Probleme innerhalb der Eingabewerte Mediendateien Fehler verursacht.
ErrorExecutingTaskUnsupportedFormat|Kategorie von Fehlern, in dem der Medienprozessor kann nicht verarbeitet werden die Dateien zur Verfügung gestellt – Medien Formatieren nicht, unterstützt, oder die Konfiguration stimmen nicht überein. Beispielsweise versuchen, eine nur-Audio-Ausgabe von einer Anlage zu erzeugen, die nur Video enthält
ErrorProcessingTask|Kategorie von anderen Fehlern, die der Medienprozessor stößt während der Verarbeitung des Vorgangs, die nicht im Zusammenhang mit Inhalt sind.
ErrorUploadingOutputAsset|Der Fehler beim Hochladen der Anlage Ausgabe Kategorie
ErrorCancelingTask|Kategorie von Fehlern zu Fehlern Deckblatt, wenn Sie versuchen, den Vorgang abzubrechen.
TransientError|Kategorie von Fehlern vorübergehende Probleme (z. b. temporäre Netzwerke Probleme mit Azure-Speicher)


Um Hilfe aus dem Team **Media-Dienste** zu erhalten, öffnen Sie ein [Ticket unterstützen](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).



##<a name="media-services-learning-paths"></a>Media-Dienste Learning Wege

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="related-articles"></a>Verwandte Artikel

- [Führen Sie erweiterte Codierung Aufgaben aus, indem Sie Media Encoder Standard Voreinstellungen anpassen](media-services-custom-mes-presets-with-dotnet.md)
- [Kontingente und Einschränkungen](media-services-quotas-and-limitations.md)

 
<!--Reference links in article-->
[1]: http://azure.microsoft.com/pricing/details/media-services/

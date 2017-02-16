<properties 
    pageTitle="Media Encoder Standardformate und codecs" 
    description="Dieses Thema bietet einen Überblick über Media Encoder Standardformate und Codecs." 
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
    ms.date="10/10/2016"
    ms.author="juliako;anilmur"/>

#<a name="media-encoder-standard-formats-and-codecs"></a>Media Encoder Standardformate und Codecs


Dieses Dokument enthält eine Liste der am häufigsten verwendeten Import / Export-Dateiformate, die mit Media Encoder Standard verwendet werden können.


##<a name="input-containerfile-formats"></a>Eingabemethoden Sie-Formate Container-Datei

Dateiformate (Dateitypen)|Unterstützt
---|---|---|---
FLV (mit h. 264 und AAC Codecs) (.flv)          |Ja 
MXF (.mxf)                  |Ja 
GXF (.gxf)                  |Ja 
MPEG2-PS, MPEG2-Terminaldienste, 3GP (.ts, PS, 3GP, .3gpp, MPG)   |Ja 
Windows Media Video (WMV) / ASF (.wmv, .asf) |Ja 
AVI (nicht komprimiert 8 Bit/10 Bit) (AVI)|Ja 
MP4 (MP4, M4A, M4V) / ISMV (.isma, .ismv)|Ja 
[Microsoft Digital Video Recording(DVR-MS)](https://msdn.microsoft.com/library/windows/desktop/dd692984) (.dvr-ms) |Ja 
Matroska/WebM (.mkv)        |Ja 
WELLE/WAV (WAV) |Ja 
QuickTime (MOV) |Ja

>[AZURE.NOTE] Höher ist eine Liste der am häufigsten auftretenden Dateierweiterungen aus. Media Encoder Standard unterstützt viele andere (zum Beispiel: .m2ts, .mpeg2video, .qt). Wenn Sie versuchen, eine Datei codieren, und Sie erhalten eine Fehlermeldung über das Format nicht unterstützt wird, geben Sie einen Feedback [hier](https://feedback.azure.com/forums/169396-media-services/category/144411-encoding-and-processing/).

###<a name="audio-formats-in-input-containers"></a>Audio-Formate in von Containern 

Die folgenden audio-Formate in Eingabewerte Container Durchführung Media Encoder Standard unterstützt:

- MXF, GXF und QuickTime-Dateien die audio Spuren mit überlappende Stereo oder 5.1 Beispiele haben.

oder

- MXF, GXF und QuickTime-Dateien, in dem die Audiodatei als separate PCM Spuren erfolgt ist jedoch die Zuordnung Kanal (in Stereo oder 5.1) aus den Dateimetadaten abgeleitet werden können

Beachten Sie, dass die Unterstützung für explizite/eingegebenen Kanal Zuordnung in Kürze bereitgestellt wird.


##<a name="input-video-codecs"></a>Eingabemethoden-Video-Codecs

Eingabemethoden-Video-Codecs|Unterstützt
---|---|---|---
AVC 8-Bit-10-Bit, bis zu 4:2:2, einschließlich AVCIntra   |8-bit-4:2:0 und 4:2:2 
Avid DNxHD (in MXF)                                 |Ja 
DVCPro/DVCProHD (in MXF)                            |Ja 
Digitales Video (DV) (in AVI-Dateien)                   |Ja
JPEG 2000                                           |Ja 
MPEG-2 (nach Zeitphasen bis zum 422 Profil- und hoher Ebene, einschließlich Varianten wie XDCAM, XDCAM HD, XDCAM IMX, CableLabs® und D10)|Bis zu 422 Profil 
MPEG-1                                              |Ja 
VC-1/WMV9                                           |Ja 
Canopus HQ/HQX                                      |Nein 
MPEG-4 Teil 2                                       |Ja 
[Theora](https://en.wikipedia.org/wiki/Theora)      |Ja 
YUV420 nicht komprimiert oder mezzanine                   |Ja
Apple ProRes 422                                    |Ja
Apple ProRes 422 LT |Ja
Apple ProRes 422 HQ |Ja
Apple ProRes Proxy|Ja
Apple ProRes 4444 |Ja
Apple ProRes 4444 XQ |Ja



##<a name="input-audio-codecs"></a>Eingabemethoden-Audio-Codecs

Eingabemethoden-Audio-Codecs|Unterstützt
---|---|---|---
AAC (AAC-LC, HE-AAC und AAC-HEv2; bis zu 5.1)|Ja 
MPEG Layer 2|Ja 
MP3 (MPEG-1 Audio Layer 3)|Ja 
Windows Media-Audiodatei|Ja 
WAV/PCM|Ja 
[FLAC](https://en.wikipedia.org/wiki/FLAC)</a>|Ja 
[Opus](http://go.microsoft.com/fwlink/?LinkId=822667) |Ja 
[Vorbis](https://en.wikipedia.org/wiki/Vorbis)</a>|Ja 
AMR (adaptive mit mehreren Rate)|Ja
AES (SMPTE 331 M und 302 M, AES3-2003)        |Nein 
Dolby® E                                    |Nein 
Dolby® digitalen (AC3)                        |Nein 
Dolby® Digital Plus (E-AC3)                 |Nein 


##<a name="output-formats-and-codecs"></a>Ausgabeformat und codecs

Die folgende Tabelle enthält die Codecs und Dateiformate, die für den Export unterstützt werden.


Dateiformat|Video-Codec|Audio-Codecs
---|---|---
MP4 <br/><br/>(einschließlich Multi-Bitrate MP4 Container) |H. 264 (hoch, Primär und geplante Profile)|AAC-LC, HE-AAC Version 1, HE-AAC 2 
MPEG2-TERMINALDIENSTE |H. 264 (hoch, Primär und geplante Profile)|AAC-LC, HE-AAC Version 1, HE-AAC 2 



##<a name="media-services-learning-paths"></a>Media-Dienste Learning Wege

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Siehe auch

[Codierung On Demand-Inhalten mit Azure Media-Dienste](media-services-encode-asset.md)

[So codieren mit Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md)

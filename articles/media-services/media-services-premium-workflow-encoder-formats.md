<properties 
    pageTitle="Media Encoder Premium Workflow Formate und Codecs | Microsoft Azure" 
    description="Dieses Thema bietet einen Überblick über die Formate Media Encoder Premium-Workflow-Formate und codecs" 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erik43" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"    
    ms.author="juliako;anilmur"/>

#<a name="media-encoder-premium-workflow-formats-and-codecs"></a>Media Encoder Premium Workflow Formate und codecs


>[AZURE.NOTE]Premium Encoder Fragen per e-Mail an Mepd unter Microsoft.com.
>
>In diesem Thema erläuterten Media Encoder Premium Workflow Media-Prozessor ist nicht verfügbar in China. 

Dieses Dokument enthält eine Liste der Eingabe- und Ausgabe-Dateiformate und Codecs, die von der öffentlichen Preview-Version von **Media Encoder Premium Workflow** Encoder unterstützt werden.

[Media Encoder Premium Worflow Eingabesprache Formate und Codecs](#input_formats)

[Media Encoder Premium Worflow Ausgabe Formate und Codecs](#output_formats)

**Media Encoder Premium Workflow** unterstützt Untertitel in [diesem](#closed_captioning) Abschnitt beschrieben. 


##<a name="a-idinputformatsamedia-encoder-premium-workflow-input-formats-and-codecs"></a><a id="input_formats"></a>Media Encoder Premium Workflow Eingabemethoden Formate und Codecs

Im folgende Abschnitt listet die Codecs und Dateiformate, die dieser Medienprozessor als Eingabe unterstützt.

###<a name="input-containerfile-formats"></a>Eingabemethoden Sie-Formate Container-Datei

- Adobe® Flash® F4V
- MXF/SMPTE 377M
- GXF
- MPEG-2-Streams
- MPEG-2 Programm Streams
- MPEG-4/MP4
- Windows Media-ASF
- AVI (nicht komprimiert 8 Bit/10 Bit)

###<a name="input-video-codecs"></a>Eingabemethoden-Video-Codecs

- AVC 8-Bit-10-Bit, bis zu 4:2:2, einschließlich AVCIntra
- Avid DNxHD (in MXF)
- DVCPro/DVCProHD (in MXF)
- JPEG 2000
- MPEG-2 (nach Zeitphasen bis zum 422 Profil- und hoher Ebene, einschließlich Varianten wie XDCAM, XDCAM HD, XDCAM IMX, CableLabs® und D10)
- MPEG-1
- Windows Media Video/VC-1

###<a name="input-audio-codecs"></a>Eingabemethoden-Audio-Codecs

- AES (SMPTE 331 M und 302 M, AES3-2003)
- Dolby® E
- Dolby® digitalen (AC3)
- AAC (AAC-LC, HE-AAC und AAC-HEv2; bis zu 5.1)
- MPEG Layer 2
- MP3 (MPEG-1 Audio Layer 3)
- Windows Media-Audiodatei
- WAV/PCM
 
##<a name="a-idoutputformatamedia-encoder-premium-workflow-output-formats-and-codecs"></a><a id="output_format"></a>Media Encoder Premium Workflow Ausgabeformate und Codecs

Im folgende Abschnitt listet die Codecs und Dateiformate, die in der Ausgabe aus dieser Medienprozessor unterstützt werden.

###<a name="output-containerfile-formats"></a>Formate für die Ausgabe Container-Datei

- Adobe® Flash® F4V
- MXF (OP1a, XDCAM und AS02)
- DPP (einschließlich AS11)
- GXF
- MPEG-4/MP4
- Windows Media-ASF
- AVI (nicht komprimiert 8 Bit/10 Bit)
- Interpolierten Streaming-Dateiformat (PIFF 1.3)
- MPEG-TERMINALDIENSTE 


###<a name="output-video-codecs"></a>Die Ausgabe Video-Codecs

- AVC (h. 264; 8-Bit-; nach Zeitphasen bis zum Profil hoher Ebene 5.2; 4 K extrem HD; AVC innerhalb)
- Avid DNxHD (in MXF)
- DVCPro/DVCProHD (in MXF)
- MPEG-2 (nach Zeitphasen bis zum 422 Profil- und hoher Ebene, einschließlich Varianten wie XDCAM, XDCAM HD, XDCAM IMX, CableLabs® und D10)
- MPEG-1
- Windows Media Video/VC-1
- Erstellen von Miniaturansichten JPEG

###<a name="output-audio-codecs"></a>Die Ausgabe-Audio-Codecs

- AES (SMPTE 331 M und 302 M, AES3-2003)
- Dolby® digitalen (AC3)
- Dolby® Digital Plus-(E-AC3) bis zu 7.1
- AAC (AAC-LC, HE-AAC und AAC-HEv2; bis zu 5.1)
- MPEG Layer 2
- MP3 (MPEG-1 Audio Layer 3)
- Windows Media-Audiodatei

##<a name="a-idclosedcaptioningasupport-for-closed-captioning"></a><a id="closed_captioning"></a>Unterstützung für Untertitel

Aufnahme aktivieren, **Media Encoder Premium Workflow** unterstützt:

1. SCC-Dateien
1. SMPTE-TT-Dateien
1. CEA-608/CEA-708 – als Benutzerdaten (Auswertung Nachrichten h. 264 Mitarbeit Streams, ATSC/53, SCTE20) oder durchgeführt als zusätzliche Daten in MXF/GXF-Dateien
1. STL Untertitel-Dateien

Bei der Ausgabe stehen die folgenden Optionen:

1. CEA-608 zu CEA-708 Übersetzung
1. CEA-608/CEA-708 pass-through (in h. 264 Mitarbeit Streams Auswertung Nachrichten eingebettet oder übertragen als zusätzliche Daten in MXF-Dateien)
1. SCC
1. SMPTE zeitlich Text (von Quelle CEA-608 pro SMPTE RP2052; einschließlich DFXP Datei erstellen)
1. SRT Untertitel-Datei
1. DVB Untertitel streams

Hinweis: nicht alle oben Ausgabeformat werden für die Übermittlung per streaming in Azure Media Services unterstützt.

##<a name="known-issues"></a>Bekannte Probleme

Wenn Ihre Eingabe Video nicht enthält enthält Untertitel, die Ausgabe Anlage dann immer noch eine leere TTML-Datei. 


##<a name="media-services-learning-paths"></a>Media-Dienste Learning Wege

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

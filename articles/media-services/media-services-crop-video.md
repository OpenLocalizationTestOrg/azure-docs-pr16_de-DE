<properties
    pageTitle="Video Zuschneiden | Microsoft Azure"
    description="In diesem Artikel wird gezeigt, wie Videos mit Media Encoder Standard zuschneiden."
    services="media-services"
    documentationCenter=""
    authors="anilmur"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/26/2016"  
    ms.author="anilmur;juliako;"/>

# <a name="crop-videos-with-media-encoder-standard"></a>Zuschneiden von Videos mit Media Encoder Standard

Media Encoder Standard (MWS) können Sie Ihre Eingaben video zuschneiden. Zuschneiden versteht der Auswahl eines rechteckigen Fensters in den Videoframe und Codierung nur die Pixel in das Fenster. Das folgende Diagramm hilft Erläutern Sie den Vorgang.

![Zuschneiden eines Videos](./media/media-services-crop-video/media-services-crop-video01.png)

Angenommen Sie, Sie als Eingabe ein Videos, das eine Auflösung von 1920 x 1080 Pixel (Seitenverhältnis 16:9) hat, jedoch schwarze Balken (Säule Listenfelder) am links und rechts, damit nur ein Fenster 4:3 oder 1440 x 1080 Pixel aktiven Video enthält. Sie können MWS Zuschneiden oder Bearbeiten von schwarzen Balken verwenden und den Bereich des 1440 x 1080 codieren.

Zuschneiden in MWS ist eine Phase vor der Verarbeitung, damit die Zuschneidepunkt Parameter in die Codierung Voreinstellung auf das ursprüngliche Eingabewerte Video anwenden. Codierung ist eine weitere Stufe, und die Breite/Höhe Einstellungen anwenden, die *vorab verarbeitet* Video- und nicht mit dem ursprünglichen Video. Beim Entwerfen der Voreinstellung müssen Sie Folgendes ausführen: (a) Wählen Sie die Zuschneiden Parameter basierend auf der ursprünglichen von Video und (b) Ihre Einstellungen basierend auf das zugeschnittene Video codieren. Wenn Sie keine Übereinstimmung gibt Ihre Einstellungen für das zugeschnittene Video codieren, die Ausgabe werden nicht wie erwartet.

Im [folgenden](media-services-advanced-encoding-with-mes.md#encoding_with_dotnet) Thema wird zum Erstellen einer Codierung mit MWS und wie Sie eine benutzerdefinierte Vorgabe für den Vorgang Codierung angeben. 

## <a name="creating-a-custom-preset"></a>Erstellen einer benutzerdefinierten Voreinstellung

Im Beispiel in der Abbildung dargestellt:

1. Ursprüngliche Eingabe ist 1920 x 1080
1. Es muss der Ausgabe des 1440 x 1080, die im Rahmen von zentriert ist abgeschnitten
1. Dies bedeutet, dass einen X-Offset (1920 – 1440) / 2 = 240 und eine Y-offset 0 (null)
1. Die Breite und Höhe des zuschneiderechtecks sind 1440 und 1080, Hilfethemas
1. In der Phase Codieren der Fragen werden drei Ebenen ist, sind die Lösungen 1440 x 1080, 960 x 720 und 480 x 360, jeweils

###<a name="json-preset"></a>JSON-Voreinstellung


    {
      "Version": 1.0,
      "Sources": [
        {
          "Streams": [],
          "Filters": {
            "Crop": {
                "X": 240,
                "Y": 0,
                "Width": 1440,
                "Height": 1080
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
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 3400,
              "MaxBitrate": 3400,
              "BufferWindow": "00:00:05",
              "Width": 1440,
              "Height": 1080,
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
              "Bitrate": 1250,
              "MaxBitrate": 1250,
              "BufferWindow": "00:00:05",
              "Width": 480,
              "Height": 360,
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


##<a name="restrictions-on-cropping"></a>Einschränkungen auf Zuschneiden

Das Feature "Zuschneiden" sollte manuelle darstellen. Sie müssten von Videos in einer Tool zum Bearbeiten der geeignete laden, mit dem Sie wählen Sie Bilder von Interesse, positionieren Sie den Cursor Ermittlung Versatz für das Zuschneidepunkt Rechteck, um die Codierung Voreinstellung zu bestimmen, die für diesen bestimmten Videos usw. optimiert ist. Dieses Feature ist nicht zum Aktivieren von Aspekten wie vorgesehen: automatische Erkennung und Entfernung von Schwarz Letterbox/Pillarbox Rahmen in Ihrer Eingabe video.

Folgende Einschränkungen gelten für das Feature "Zuschneiden". Wenn diese nicht erfüllt ist, kann die Codierung Vorgang fehl, oder ein unerwartetes Ergebnis erzeugen.

1. Die Koordinaten und die Größe des zuschneiderechtecks müssen innerhalb von Videos anpassen
1. Wie zuvor erwähnt, müssen die Breite und Höhe in den Einstellungen codieren zugeschnittene Videos entsprechen
1. Zuschneiden von Videos im Querformat erfasst gilt (d. h. Videos mit einem Smartphone aufgezeichnet wird nicht anwendbar frei, vertikal oder in Hochformat)
1. Funktioniert am besten mit schrittweisen Video mit quadratischen Pixeln erfasst

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="next-step"></a>Als Nächstes
 
Finden Sie unter Azure Media Services learning Pfade können Sie sich hervorragend von AMS bereitgestellten Features vertraut zu machen.  

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

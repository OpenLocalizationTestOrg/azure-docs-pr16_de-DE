<properties 
    pageTitle="Wiedergabe von Inhalten | Microsoft Azure" 
    description="In diesem Thema werden die vorhandenen Player, dass Sie zur Wiedergabe von Inhalten verwenden können." 
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
    ms.date="10/12/2016" 
    ms.author="juliako"/>


#<a name="playing-your-content-with-existing-players"></a>Wiedergabe von Inhalten mit vorhandenen Spieler

Azure Media Services unterstützt viele beliebter streaming-Formate, wie z. B. interpolierten Streaming, Live HTTP-Streaming und MPEG-Strich. In diesem Thema verweist auf vorhandene Player, die Sie verwenden können, testen Sie Ihre Streams.

>[AZURE.NOTE]Dynamisch verpackt wiedergeben oder dynamisch verschlüsselter Inhalt, müssen Sie mindestens eine streaming Einheit für den Endpunkt streaming erhalten von Inhalten vorführen möchten. Informationen zum Skalieren streaming Einheiten finden Sie unter: [das streaming Einheiten skalieren](media-services-portal-manage-streaming-endpoints.md).

### <a name="the-azure-portal-media-services-content-player"></a>Der Azure Portals Media-Dienste Inhalt player

Das **Azure** -Portal bietet einen Inhalten Player, den Sie verwenden können, um das Video zu testen.

Klicken Sie auf das gewünschte Video (Stellen Sie sicher, dass es [veröffentlicht](media-services-portal-publish.md)wurde), und klicken Sie auf **die Wiedergabeschaltfläche am unteren Rand des Portals** .

Einige Überlegungen Ursachen zurückzuführen:

- Der **Inhalt SERVICES MEDIENWIEDERGABE** wiedergegeben wird, aus der Standardeinstellungen für streaming-Endpunkt. Wenn Sie von einer nicht standardmäßigen streaming Endpunkt wiedergeben möchten, verwenden Sie einen anderen Spieler. Beispielsweise [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).


![AMSPlayer][AMSPlayer]

###<a name="azure-media-player"></a>Azure MediaPlayer

Verwenden von [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html) auf Wiedergabe (löschen oder geschützten) den Inhalt in einem der folgenden Formate:

- Interpolierten Streaming
- MPEG-BINDESTRICH
- HLS
- Schrittweisen MP4


###<a name="flash-player"></a>Flash Player

####<a name="aes-encrypted-with-token"></a>AES-Verschlüsselung mit Token

[http://aestoken.azurewebsites.NET](http://aestoken.azurewebsites.net)

###<a name="silverlight-players"></a>Silverlight-Player

####<a name="monitoring"></a>Für die Überwachung

[http://SMF.cloudapp.NET/HealthMonitor](http://smf.cloudapp.net/healthmonitor)

####<a name="playready-with-token"></a>PlayReady mit Token

[http://sltoken.azurewebsites.NET](http://sltoken.azurewebsites.net)

### <a name="dash-players"></a>Gedankenstrich Spieler

[http://dashplayer.azurewebsites.NET](http://dashplayer.azurewebsites.net)

[http://dashif.org](http://dashif.org)

###<a name="other"></a>Andere

So testen Sie HLS URLs auch verwendet werden kann:

- **Safari** auf einem iOS-Gerät oder
- **3ivx HLS Player** unter Windows.

##<a name="developing-video-players"></a>Entwickeln Videoplayer

Informationen dazu, wie Sie Ihre eigenen Spieler entwickeln finden Sie unter [entwickeln Videoplayer](media-services-develop-video-players.md)




##<a name="media-services-learning-paths"></a>Media-Dienste Learning Wege

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


[AMSPlayer]: ./media/media-services-playback-content-with-existing-players/media-services-portal-player.png

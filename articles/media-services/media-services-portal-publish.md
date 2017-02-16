<properties
    pageTitle="  Veröffentlichen von Inhalten mit dem Portal Azure | Microsoft Azure"
    description="In diesem Lernprogramm führt Sie durch die Schritte zum Veröffentlichen von Inhalten mit dem Azure-Portal an."
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
    ms.date="10/24/2016"
    ms.author="juliako"/>

# <a name="publish-content-with-the-azure-portal"></a>Veröffentlichen von Inhalten mit dem Azure-portal

> [AZURE.SELECTOR]
- [Portal](media-services-portal-publish.md)
- [.NET](media-services-deliver-streaming-content.md)
- [REST](media-services-rest-deliver-streaming-content.md)

## <a name="overview"></a>(Übersicht)

> [AZURE.NOTE] Damit dieses Lernprogramm abgeschlossen, benötigen Sie ein Azure-Konto an. Weitere Informationen finden Sie unter [Azure kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/). 

Um Ihre Benutzer mit einer URL angeben, das zum übertragen oder Herunterladen von Inhalten verwendet werden können, müssen Sie zuerst der Anlage "Veröffentlichen", indem Sie einen Locator erstellen. Locator ermöglichen den Zugriff auf Dateien in der Anlage. Media Services unterstützt zwei Arten von Locator: 

- Streaming (OnDemandOrigin) Locator, für adaptives streaming (z. B., Stream MPEG Gedankenstrich HLS oder interpolierten Streaming) verwendet. Um einen streaming Locator Ihrer Ressource erstellen müssen eine Datei .ism enthalten. 
- Schrittweisen (SAS) Locator, für die Übermittlung Videos über schrittweisen Download verwendet.


Streaming URL weist das folgende Format, und Sie können Anlagen interpolierten Streaming zum Ausprobieren verwenden.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

Zum Erstellen einer URL streaming HLS anfügen (Format = m3u8-Aapl) zur URL.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

Zum Erstellen einer URL streaming MPEG-Gedankenstrich anfügen (Format = Mpd-Uhrzeit-csf) zur URL.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

Eine SAS-URL weist das folgende Format.

    {blob container name}/{asset name}/{file name}/{SAS signature}

Weitere Informationen finden Sie unter [Übersicht über die Übermittlung von Inhalten](media-services-deliver-content-overview.md).

>[AZURE.NOTE] Wenn Sie im Portal Locator vor März 2015 erstellt, wurden Locator mit einem Ablaufdatum zwei Jahr erstellt.  

Verwenden Sie zum Aktualisieren einer Locator ein Ablaufdatum [REST](http://msdn.microsoft.com/library/azure/hh974308.aspx#update_a_locator ) oder [.NET](http://go.microsoft.com/fwlink/?LinkID=533259) APIs aus. Beachten Sie, dass die URL, wenn Sie das Ablaufdatum der SAS Locator aktualisieren, ändert sich.

### <a name="to-use-the-portal-to-publish-an-asset"></a>Verwenden Sie im Portal veröffentlichen eine Anlage

Wenn Sie im Portal verwenden, um eine Anlage zu veröffentlichen, führen Sie folgende Schritte aus:

1. Wählen Sie im [Portal Azure](https://portal.azure.com/)Ihrer Azure Media Services-Konto ein.
1. Wählen Sie **Einstellungen**aus > **Posten**.
1. Wählen Sie die Anlage, die Sie veröffentlichen möchten.
1. Klicken Sie auf die Schaltfläche **Veröffentlichen** .
1. Wählen Sie die URL-Typ aus.
2. Drücken Sie auf **Hinzufügen**.

    ![Veröffentlichen](./media/media-services-portal-vod-get-started/media-services-publish1.png)

Die URL wird die Liste der **URLs veröffentlicht**hinzugefügt werden.

## <a name="play-content-from-the-portal"></a>Wiedergeben von Inhalten aus dem portal

Azure-Portal bietet einen Inhalten Player, den Sie verwenden können, um das Video zu testen.

Klicken Sie auf das gewünschte Video, und klicken Sie dann auf **die Wiedergabeschaltfläche** .

![Veröffentlichen](./media/media-services-portal-vod-get-started/media-services-play.png)

Einige Überlegungen Ursachen zurückzuführen:

- Stellen Sie sicher, dass das Video veröffentlicht wurde.
- Diese **MediaPlayer** wiedergegeben wird, aus der Standardeinstellungen für streaming-Endpunkt. Wenn Sie von einer nicht standardmäßigen streaming Endpunkt wiedergeben möchten, klicken Sie auf zum Kopieren der URL und einen anderen Spieler verwenden. Beispielsweise [Azure Services Medienwiedergabe](http://amsplayer.azurewebsites.net/azuremediaplayer.html).
- Der streaming Endpunkt aus dem Sie streaming werden muss ausgeführt werden.  
- Um von einem streaming Endpunkt streamen, sollten Sie mindestens eine streaming Einheit hinzufügen. Weitere Informationen finden Sie [in diesem](media-services-portal-scale-streaming-endpoints.md) Thema.   

##<a name="next-steps"></a>Nächste Schritte

Überprüfen Sie die Pfade learning Media-Dienste.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



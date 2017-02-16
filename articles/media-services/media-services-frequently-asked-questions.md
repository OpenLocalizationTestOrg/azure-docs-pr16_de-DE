<properties 
    pageTitle="Häufig gestellte Fragen | Microsoft Azure" 
    description="Häufig gestellte Fragen (FAQs)" 
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


#<a name="frequently-asked-questions"></a>Häufig gestellte Fragen

##<a name="general-ams-faqs"></a>Allgemeine AMS häufig gestellte Fragen

F: wie skalieren Sie die Indizierung?

A: die Einheiten reservierten sind gleich für Codierung und Indizierung Vorgänge. Anweisungen Sie zum [Skalieren Codierung reservierte Einheiten](media-services-scale-media-processing-overview.md). **Beachten Sie** , dass die Leistung Indexer reservierte Einheitentyp keine Auswirkung.

F: Ich hochgeladen, codierte und ein Video veröffentlicht. Was ist der Grund für das Video nicht wiedergegeben, wenn ich versuche, es übertragen?

A: eine der häufigsten Ursachen ist, dass Sie nicht mindestens eine reservierte streaming Einheit auf den streaming Endpunkt, aus dem Sie Wiedergabe kennwortgeschütztes, reserviert verfügen.  Anweisungen Sie zum [Skalieren Streaming reservierte Einheiten](media-services-portal-scale-streaming-endpoints.md).

F: tun kann ich, live-Stream zusammensetzen?

A: zusammensetzen auf live Streams wird derzeit nicht Azure Media Services, angeboten, sodass Sie müssen auf Ihrem Computer vorab erstellen möchten.

F: verwenden kann ich Azure CDN mit Live Streaming?

A: Media Services unterstützt die Integration mit Azure CDN (Weitere Informationen finden Sie unter [So verwalten Streaming Endpunkten in einem Media Services-Konto](media-services-portal-manage-streaming-endpoints.md)).  Sie können mit CDN streaming Live verwenden. Azure Media Services bietet interpolierten Streaming, HLS und MPEG-Strich Ausgaben. Alle Formate verwenden HTTP zum Übertragen von Daten und Vorteile von HTTP Zwischenspeichern erhalten. In live streaming ist-Video oder Audio-Daten, um Fragmente aufgeteilt und diese einzelne Fragmente in CDN Cache abrufen. Nur Daten muss aktualisiert werden, ist die Manifestdaten. CDN wird regelmäßig Manifestdaten aktualisiert.

F: bedeutet Azure Media Services unterstützt das Speichern von Bildern?

A: Wenn Sie nur zum Speichern von JPEG oder PNG-Bildern gefunden werden, sollten Sie die in Azure BLOB-Speicher behalten. Es gibt keine Vorteile, diese in Ihr Konto Media-Dienste, wenn sie Ihr Video oder Audio Posten zugeordnet bleiben soll. Oder wenn Sie möglicherweise eine müssen die Bilder als überlagert im video Encoder verwendet haben. Media Encoder Standard unterstützt Überlagern von Bildern auf Videos und das ist, was es JPEG und PNG als unterstützt Listen Eingabemethoden-Formate. Weitere Informationen finden Sie unter [Erstellen von überlagert](media-services-custom-mes-presets-with-dotnet.md#overlay).

F: wie kann ich Anlagen aus einem Media-Dienste-Konto in ein anderes kopieren.

A: um Anlagen aus einem Media-Dienste-Konto in ein anderes kopieren mit .NET [IAsset.Copy](https://github.com/Azure/azure-sdk-for-media-services-extensions/blob/dev/MediaServices.Client.Extensions/IAssetExtensions.cs#L354) Erweiterungsmethode verfügbar im Repository [Azure Media Services .NET SDK Erweiterungen](https://github.com/Azure/azure-sdk-for-media-services-extensions/) verwenden. Weitere Informationen finden Sie unter [diesem](https://social.msdn.microsoft.com/Forums/azure/28912d5d-6733-41c1-b27d-5d5dff2695ca/migrate-media-services-across-subscription?forum=MediaServices) Forumthread.

F: Was sind die unterstützten Zeichen für die Benennung von Dateien bei der Arbeit mit AMS?

A: Media-Dienste verwendet den Wert der Eigenschaft IAssetFile.Name beim Erstellen von URLs für das streaming von Inhalten (z. B. http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters). Aus diesem Grund ist die prozentkodierung nicht zulässig. Der Wert der **Name** -Eigenschaft kann nicht die folgenden [Zeichen %-Codierung-reserviert](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters)haben: !*'();:@&=+$,/?%#[]". Darüber hinaus gibt es nur kann eine "." für die Dateinamenerweiterung.


F: wie eine Verbindung mit der REST wird?

A: nach erfolgreich eine Verbindung zu https://media.windows.net herstellen, erhalten Sie eine 301 Umleitung eine andere Medien Services-URI angegeben. Sie müssen nachfolgenden anrufen zu den neuen URI wie in [Verbindung mit dem REST-API mit Media-Dienste](media-services-rest-connect-programmatically.md)beschrieben. 


F: wie kann ich ein Video während der Codierung Prozess drehen.

A: [Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md) unterstützt Drehung durch die Winkel 90/180/270. Das Standardverhalten ist "Auto", bei denen es versucht zu erkennen die Drehung Metadaten in der Datei für eingehende MP4/MOV und dafür anzugleichen. Gehören Sie das folgende **Quellen** Element auf einen der das Json Voreinstellungen definierten [hier](http://msdn.microsoft.com/library/azure/mt269960.aspx):
    
    "Version": 1.0,
    "Sources": [
    {
      "Streams": [],
      "Filters": {
        "Rotation": "90"
      }
    }
    ],
    "Codecs": [
    
    ...




##<a name="media-services-learning-paths"></a>Media-Dienste Learning Wege

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

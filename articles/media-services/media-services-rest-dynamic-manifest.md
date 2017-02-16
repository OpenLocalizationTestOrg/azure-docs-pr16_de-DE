<properties 
    pageTitle="Erstellen von Filtern mit Azure Media Services REST-API | Microsoft Azure" 
    description="In diesem Thema beschreibt das Erstellen von Filtern, sodass sie Ihren Kunden auf bestimmte Bereiche eines Streams Stream verwenden kann. Media-Dienste erstellt dynamische Manifeste zum selektiven streaming zu erzielen."
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ne" 
    ms.topic="article" 
    ms.date="09/26/2016"  
    ms.author="juliako;cenkdin"/>

#<a name="creating-filters-with-azure-media-services-rest-api"></a>Erstellen von Filter mit Azure Media Services REST-API

> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-dynamic-manifest.md)
- [REST](media-services-rest-dynamic-manifest.md)


Ab Version 2.11 können Media-Dienste Sie zum Definieren von Filtern für Ihre Anlagen. Diese Filter sind serverseitige Regeln, die Ihre Kunden auswählen, wie Dinge zu ermöglichen: Wiedergabe nur einen Teil eines Videos (statt wiedergeben das gesamte Video), oder geben Sie nur eine Teilmenge der Audio- und Formatvarianten, die Ihre Kunden Gerät kann (und nicht alle Formatvarianten, die der Anlage zugeordnet sind) behandelt. Diese Filterung der Datenbestände wird archiviert durch **Dynamische Manifest**s, die auf Ihre Kunden verlangen ein Videos auf der Grundlage der angegebenen Filter streamen erstellt werden.

Ausführlichere Informationen im Zusammenhang mit Filter und dynamische Manifest finden Sie unter [Übersicht über die dynamische Manifeste](media-services-dynamic-manifest-overview.md).

In diesem Thema wird gezeigt, wie REST-APIs verwenden, um das Erstellen, aktualisieren und Löschen von Filtern. 

##<a name="types-used-to-create-filters"></a>Typen, die zum Erstellen von Filtern verwendet werden.

Die folgenden Arten werden verwendet, wenn Sie Filter zu erstellen:  

- [Filter](http://msdn.microsoft.com/library/azure/mt149056.aspx)
- [AssetFilter](http://msdn.microsoft.com/library/azure/mt149053.aspx)
- [PresentationTimeRange](http://msdn.microsoft.com/library/azure/mt149052.aspx)
- [FilterTrackSelect und FilterTrackPropertyCondition](http://msdn.microsoft.com/library/azure/mt149055.aspx)



>[AZURE.NOTE] Bei der Arbeit mit der Media Services REST-API gelten die folgenden Punkte:
>
>Wenn Sie Elemente in Media-Dienste zugreifen zu können, müssen Sie bestimmte Felder und Werte in der HTTP-Anfragen festlegen. Weitere Informationen finden Sie unter [Einrichten für die Entwicklung von Medien Services REST API](media-services-rest-how-to-use.md).

>Nachdem erfolgreich eine Verbindung zu https://media.windows.net, erhalten Sie eine 301 Umleitung zurück, der eine andere Medien Services URI angibt. Nehmen Sie die nachfolgende anrufen, um den neuen URI wie in [Verbindung mit dem REST-API mit Media-Dienste](media-services-rest-connect-programmatically.md)beschrieben. 


##<a name="create-filters"></a>Erstellen von Filtern

###<a name="create-global-filters"></a>Allgemeine Filter erstellen

Verwenden Sie zum Erstellen eines globalen Filters die folgenden HTTP-Anfragen ein:  

####<a name="http-request"></a>HTTP-Anforderung

Anfordern von Kopfzeilen

    POST https://media.windows.net/API/Filters HTTP/1.1 
    DataServiceVersion:3.0 
    MaxDataServiceVersion: 3.0 
    Content-Type: application/json 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000 
    Host:media.windows.net 

Anforderungstexts 

    {  
       "Name":"GlobalFilter",
       "PresentationTimeRange":{  
          "StartTimestamp":"0",
          "EndTimestamp":"9223372036854775807",
          "PresentationWindowDuration":"12000000000",
          "LiveBackoffDuration":"0",
          "Timescale":"10000000"
       },
       "Tracks":[  
          {  
             "PropertyConditions":
                  [  
                {  
                   "Property":"Type",
                   "Value":"audio",
                   "Operator":"Equal"
                },
                {  
                   "Property":"Bitrate",
                   "Value":"0-2147483647",
                   "Operator":"Equal"
                }
             ]
          }
       ]
    }




####<a name="http-response"></a>HTTP-Antwort
    
    HTTP/1.1 201 Created 

###<a name="create-local-assetfilters"></a>Lokale AssetFilters erstellen

Verwenden Sie zum Erstellen einer lokalen AssetFilter die folgenden HTTP-Anfragen ein:  

####<a name="http-request"></a>HTTP-Anforderung

Anfordern von Kopfzeilen

    POST https://media.windows.net/API/AssetFilters HTTP/1.1 
    DataServiceVersion: 3.0 
    MaxDataServiceVersion: 3.0 
    Content-Type: application/json 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000 
    Host: media.windows.net  

Anforderungstexts 

    {   
       "Name":"AssetFilter", 
       "ParentAssetId":"nb:cid:UUID:536e555d-1500-80c3-92dc-f1e4fdc6c592", 
       "PresentationTimeRange":{   
          "StartTimestamp":"0", 
          "EndTimestamp":"9223372036854775807", 
          "PresentationWindowDuration":"12000000000", 
          "LiveBackoffDuration":"0", 
          "Timescale":"10000000" 
       }, 
       "Tracks":[   
          {   
             "PropertyConditions": 
                  [   
                {   
                   "Property":"Type", 
                   "Value":"audio", 
                   "Operator":"Equal" 
                }, 
                {   
                   "Property":"Bitrate", 
                   "Value":"0-2147483647", 
                   "Operator":"Equal" 
                } 
             ] 
          } 
       ] 
    } 

####<a name="http-response"></a>HTTP-Antwort 

    HTTP/1.1 201 Created 
    . . . 

##<a name="list-filters"></a>Listenfilter

###<a name="get-all-global-filters-in-the-ams-account"></a>Abrufen von alle globalen **Filter**s in das AMS-Konto

Um Filter auflisten, verwenden Sie die folgenden HTTP-Anfragen: 

####<a name="http-request"></a>HTTP-Anforderung
     
    GET https://media.windows.net/API/Filters HTTP/1.1 
    DataServiceVersion:3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    Host: media.windows.net 
    
### <a name="get-assetfilters-associated-with-an-asset"></a>Abrufen von **AssetFilter**s Zusammenhang mit einer Anlage

####<a name="http-request"></a>HTTP-Anforderung

    GET https://media.windows.net/API/Assets('nb%3Acid%3AUUID%3A536e555d-1500-80c3-92dc-f1e4fdc6c592')/AssetFilters HTTP/1.1 
    DataServiceVersion: 3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000 
    Host: media.windows.net 

###<a name="get-an-assetfilter-based-on-its-id"></a>Abrufen einer **AssetFilter** auf Grundlage der Id

####<a name="http-request"></a>HTTP-Anforderung

    GET https://media.windows.net/API/AssetFilters('nb%3Acid%3AUUID%3A536e555d-1500-80c3-92dc-f1e4fdc6c592__%23%23%23__TestFilter') HTTP/1.1 
    DataServiceVersion: 3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    x-ms-client-request-id: 00000000


##<a name="update-filters"></a>Aktualisieren von Filtern
 
Verwenden Sie PATCH sich oder Verbinden ein Filters mit neuen Eigenschaftswerte aktualisieren.  Weitere Informationen zu diesen Operationen finden Sie unter [PATCH, setzen, ZUSAMMENFÜHREN](http://msdn.microsoft.com/library/dd541276.aspx).
 
Wenn Sie einen Filter aktualisieren, kann für die Regeln aktualisieren streaming Endpunkt bis zu 2 Minuten dauern. Wenn der Inhalt wurde served mit diesem Filter (und Cache in Proxys und CDN Caches), kann diesen Filter aktualisieren Player Fehlern bei führen. Es wird empfohlen, den Cache zu leeren, nach der Aktualisierung des Filters. Wenn diese Option nicht möglich ist, sollten erwägen Sie, einen anderen Filter zu verwenden.  
 
###<a name="update-global-filters"></a>Allgemeine Filter aktualisieren

Um eine globale Filter zu aktualisieren, verwenden Sie die folgenden HTTP-Anfragen: 

####<a name="http-request"></a>HTTP-Anforderung
 
Anfrage-Header: 

    MERGE https://media.windows.net/API/Filters('filterName') HTTP/1.1 
    DataServiceVersion:3.0 
    MaxDataServiceVersion: 3.0 
    Content-Type: application/json 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000 
    Host: media.windows.net 
    Content-Length: 384
    
Anforderungstexts: 
    
    { 
       "Tracks":[   
          {   
             "PropertyConditions": 
             [   
                {   
                   "Property":"Type", 
                   "Value":"audio", 
                   "Operator":"Equal" 
                }, 
                {   
                   "Property":"Bitrate", 
                   "Value":"0-2147483647", 
                   "Operator":"Equal" 
                } 
             ] 
          } 
       ] 
    } 

###<a name="update-local-assetfilters"></a>Aktualisieren der lokalen AssetFilters

Verwenden Sie die folgenden HTTP-Anfragen, um einen lokalen Filter zu aktualisieren: 

####<a name="http-request"></a>HTTP-Anforderung

Fordern Sie Kopfzeilen an: 

    MERGE https://media.windows.net/API/AssetFilters('nb%3Acid%3AUUID%3A536e555d-1500-80c3-92dc-f1e4fdc6c592__%23%23%23__TestFilter')  HTTP/1.1 
    DataServiceVersion: 3.0 
    MaxDataServiceVersion: 3.0 
    Content-Type: application/json 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000 
    Host: media.windows.net 
    
Anforderungstexts: 
    
    { 
       "Tracks":[   
          {   
             "PropertyConditions": 
             [   
                {   
                   "Property":"Type", 
                   "Value":"audio", 
                   "Operator":"Equal" 
                }, 
                {   
                   "Property":"Bitrate", 
                   "Value":"0-2147483647", 
                   "Operator":"Equal" 
                } 
             ] 
          } 
       ] 
    } 


##<a name="delete-filters"></a>Löschen von Filtern


###<a name="delete-global-filters"></a>Allgemeine Filter löschen

Um eine globale Filter zu löschen, verwenden Sie die folgenden HTTP-Anfragen:
    
####<a name="http-request"></a>HTTP-Anforderung

    DELETE https://media.windows.net/api/Filters('GlobalFilter') HTTP/1.1 
    DataServiceVersion:3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    Host: media.windows.net 


###<a name="delete-local-assetfilters"></a>Lokale AssetFilters löschen

Um eine lokale AssetFilter löschen möchten, verwenden Sie die folgenden HTTP-Anfragen:

####<a name="http-request"></a>HTTP-Anforderung

    DELETE https://media.windows.net/API/AssetFilters('nb%3Acid%3AUUID%3A536e555d-1500-80c3-92dc-f1e4fdc6c592__%23%23%23__LocalFilter') HTTP/1.1 
    DataServiceVersion: 3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    Host: media.windows.net 

##<a name="build-streaming-urls-that-use-filters"></a>Erstellen von streaming URLs, die Filter verwenden

Informationen zum Veröffentlichen und übermitteln Ihre Bestände jederzeit finden Sie unter [Vorführen Inhalt Kunden (Übersicht)](media-services-deliver-content-overview.md).


Den folgenden Beispielen wird die wie Ihre streaming URLs Filter hinzugefügt.

**MPEG-BINDESTRICH** 

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf, filter=MyFilter)

**Apple HTTP Live Streaming (HLS) V4**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl, filter=MyFilter)

**Apple HTTP Live Streaming (HLS) V3**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl-v3, filter=MyFilter)

**Interpolierten Streaming**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=MyFilter)


**HDS**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=f4m-f4f, filter=MyFilter)


##<a name="media-services-learning-paths"></a>Media-Dienste Learning Wege

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="see-also"></a>Siehe auch 

[Dynamische Manifeste (Übersicht)](media-services-dynamic-manifest-overview.md)
 

 

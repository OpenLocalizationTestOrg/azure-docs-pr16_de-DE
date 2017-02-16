<properties 
    pageTitle="Erstellen von Filtern mit Azure Media Services .NET SDK" 
    description="In diesem Thema beschreibt das Erstellen von Filtern, sodass sie Ihren Kunden auf bestimmte Bereiche eines Streams Stream verwenden kann. Media-Dienste erstellt dynamische Manifeste zum selektiven streaming zu erzielen." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ne" 
    ms.topic="article" 
    ms.date="07/18/2016"
    ms.author="juliako;cenkdin"/>


#<a name="creating-filters-with-azure-media-services-net-sdk"></a>Erstellen von Filtern mit Azure Media Services .NET SDK

> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-dynamic-manifest.md)
- [REST](media-services-rest-dynamic-manifest.md)

Ab Version 2.11 können Media-Dienste Sie zum Definieren von Filtern für Ihre Anlagen. Diese Filter sind serverseitige Regeln, die Ihre Kunden auswählen, wie Dinge zu ermöglichen: Wiedergabe nur einen Teil eines Videos (statt wiedergeben das gesamte Video), oder geben Sie nur eine Teilmenge der Audio- und Formatvarianten, die Ihre Kunden Gerät kann (und nicht alle Formatvarianten, die der Anlage zugeordnet sind) behandelt. Diese Filterung der Datenbestände wird durch **Dynamische Manifest**s, die auf Ihre Kunden verlangen ein Videos auf der Grundlage der angegebenen Filter streamen erstellt wurden erreicht.

Ausführlichere Informationen im Zusammenhang mit Filter und dynamische Manifest finden Sie unter [Übersicht über die dynamische Manifeste](media-services-dynamic-manifest-overview.md).

In diesem Thema wird gezeigt, wie Medien Services .NET SDK zu erstellen, aktualisieren und Löschen von Filtern verwenden. 


Beachten Sie, wenn Sie einen Filter aktualisieren, kann es für das streaming Endpunkt, um die Regeln Aktualisieren von bis zu 2 Minuten dauern. Wenn der Inhalt wurde served mit diesem Filter (und Cache in Proxys und CDN Caches), kann das Aktualisieren dieser Filter Player Fehlern führen. Es wird empfohlen, den Cache zu leeren, nach dem Aktualisieren des Filters. Wenn diese Option nicht möglich ist, sollten erwägen Sie, einen anderen Filter zu verwenden. 

##<a name="types-used-to-create-filters"></a>Typen, die zum Erstellen von Filtern verwendet werden.

Die folgenden Arten werden verwendet, wenn Sie Filter zu erstellen: 

- **IStreamingFilter**.  Dieses Typs basiert auf den folgenden REST-API [Filter](http://msdn.microsoft.com/library/azure/mt149056.aspx)
- **IStreamingAssetFilter**. Dieses Typs basiert auf den folgenden REST-API [AssetFilter](http://msdn.microsoft.com/library/azure/mt149053.aspx)
- **PresentationTimeRange**. Dieses Typs basiert auf den folgenden REST-API [PresentationTimeRange](http://msdn.microsoft.com/library/azure/mt149052.aspx)
- **FilterTrackSelectStatement** und **IFilterTrackPropertyCondition**. Diese Typen basieren auf den folgenden REST-APIs [FilterTrackSelect und FilterTrackPropertyCondition](http://msdn.microsoft.com/library/azure/mt149055.aspx)


##<a name="createupdatereaddelete-global-filters"></a>Allgemeine Filter erstellen/aktualisieren/gelesen/löschen

Im folgende Code veranschaulicht, wie .NET erstellen, aktualisieren, lesen Sie verwenden und Anlage Filter löschen.
    
    string filterName = "GlobalFilter_" + Guid.NewGuid().ToString();
                
    List<FilterTrackSelectStatement> filterTrackSelectStatements = new List<FilterTrackSelectStatement>();
    
    FilterTrackSelectStatement filterTrackSelectStatement = new FilterTrackSelectStatement();
    filterTrackSelectStatement.PropertyConditions = new List<IFilterTrackPropertyCondition>();
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackNameCondition("Track Name", FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackBitrateRangeCondition(new FilterTrackBitrateRange(0, 1), FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackTypeCondition(FilterTrackType.Audio, FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatements.Add(filterTrackSelectStatement);
    
    // Create
    IStreamingFilter filter = _context.Filters.Create(filterName, new PresentationTimeRange(), filterTrackSelectStatements);
    
    // Update
    filter.PresentationTimeRange = new PresentationTimeRange(timescale: 500);
    filter.Update();
    
    // Read
    var filterUpdated = _context.Filters.FirstOrDefault();
    Console.WriteLine(filterUpdated.Name);

    // Delete
    filter.Delete();


##<a name="createupdatereaddelete-asset-filters"></a>Erstellen, aktualisieren und gelesen/löschen Anlage Filter

Im folgende Code veranschaulicht, wie .NET erstellen, aktualisieren, lesen Sie verwenden und Anlage Filter löschen.

    
    string assetName = "AssetFilter_" + Guid.NewGuid().ToString();
    var asset = _context.Assets.Create(assetName, AssetCreationOptions.None);
    
    string filterName = "AssetFilter_" + Guid.NewGuid().ToString();
    
        
    // Create
    IStreamingAssetFilter filter = asset.AssetFilters.Create(filterName,
                                        new PresentationTimeRange(), 
                                        new List<FilterTrackSelectStatement>());
    
    // Update
    filter.PresentationTimeRange = 
            new PresentationTimeRange(start: 6000000000, end: 72000000000);
    
    filter.Update();
    
    // Read
    asset = _context.Assets.Where(c => c.Id == asset.Id).FirstOrDefault();
    var filterUpdated = asset.AssetFilters.FirstOrDefault();
    Console.WriteLine(filterUpdated.Name);
    
    // Delete
    filterUpdated.Delete();
    



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
 


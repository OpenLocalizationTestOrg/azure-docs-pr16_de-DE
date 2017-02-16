<properties 
    pageTitle="Veröffentlichen von Azure Media Services-Inhalten mit .NET" 
    description="Erfahren Sie, wie einen Locator zu erstellen, der zum Erstellen eines streaming URLs verwendet wird. Codebeispielen in c# geschrieben sind, und verwenden den Media Services SDK für .NET." 
    authors="juliako" 
    manager="erikre" 
    editor="" 
    services="media-services" 
    documentationCenter=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/30/2016"
    ms.author="juliako"/>


# <a name="publish-azure-media-services-content-using-net"></a>Veröffentlichen von Azure Media Services-Inhalten mit .NET
 
> [AZURE.SELECTOR]
- [REST](media-services-rest-deliver-streaming-content.md)
- [.NET](media-services-deliver-streaming-content.md)
- [Portal](media-services-portal-publish.md)

##<a name="overview"></a>(Übersicht)

Sie können eine adaptive Bitrate streamen, MP4 durch Erstellen eines auf-Anforderung streaming Locators und Erstellen eines streaming-URL festgelegt. Das Thema [Codierung einer Anlageguts](media-services-encode-asset.md) wird gezeigt, wie in einer adaptive Bitrate codieren festgelegten MP4. 

>[AZURE.NOTE]Wenn Ihre Inhalte verschlüsselt ist, konfigurieren Sie Anlage Übermittlung Richtlinie vor dem Erstellen eines Locators (wie in [diesem](media-services-dotnet-configure-asset-delivery-policy.md) Artikel beschrieben). 

Sie können auch eine streaming Locator auf-Anforderung verwenden, URLs erstellen, die auf MP4-Dateien verweisen, das schrittweise heruntergeladen werden kann.  

In diesem Thema wird gezeigt, wie ein Locator, um Ihre Anlage veröffentlichen und erstellen eine weiche, MPEG Gedankenstrich und URLs streaming HLS streaming auf-Anforderung erstellen. Es werden auch wichtiges schrittweisen Download URLs erstellen. 
     
##<a name="create-an-ondemand-streaming-locator"></a>Erstellen einer auf streaming Locator-Anforderung

Zum Erstellen der auf-Anforderung streaming Locator und URLs erhalten müssen Sie die folgenden Aktionen ausführen:

   1. Wenn der Inhalt verschlüsselt ist, definieren Sie eine Zugriffsrichtlinie.
   2. Erstellen einer auf streaming Locator-Anforderung an.
   3. Wenn Sie beabsichtigen, übertragen, erhalten Sie streaming Manifestdatei (.ism) in die Anlage ein. 
        
    Wenn Sie beabsichtigen, schrittweise herunterladen, erhalten Sie die Namen der MP4-Dateien in die Anlage ein.  
   4. Erstellen von URLs auf die Manifestdatei MP4-Dateien. 
   

###<a name="use-media-services-net-sdk"></a>Verwenden von .NET SDK Media-Dienste 

Erstellen von Streaming URLs 

    private static void BuildStreamingURLs(IAsset asset)
    {
    
        // Create a 30-day readonly access policy. 
        // You cannot create a streaming locator using an AccessPolicy that includes write or delete permissions.
        IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
            TimeSpan.FromDays(30),
            AccessPermissions.Read);
    
        // Create a locator to the streaming content on an origin. 
        ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
            policy,
            DateTime.UtcNow.AddMinutes(-5));
    
        // Display some useful values based on the locator.
        Console.WriteLine("Streaming asset base path on origin: ");
        Console.WriteLine(originLocator.Path);
        Console.WriteLine();
    
        // Get a reference to the streaming manifest file from the  
        // collection of files in the asset. 
        var manifestFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                                    EndsWith(".ism")).
                                    FirstOrDefault();
        
        // Create a full URL to the manifest file. Use this for playback
        // in streaming media clients. 
        string urlForClientStreaming = originLocator.Path + manifestFile.Name + "/manifest";
        Console.WriteLine("URL to manifest for client streaming using Smooth Streaming protocol: ");
        Console.WriteLine(urlForClientStreaming);
        Console.WriteLine("URL to manifest for client streaming using HLS protocol: ");
        Console.WriteLine(urlForClientStreaming + "(format=m3u8-aapl)");
        Console.WriteLine("URL to manifest for client streaming using MPEG DASH protocol: ");
        Console.WriteLine(urlForClientStreaming + "(format=mpd-time-csf)"); 
        Console.WriteLine();
    }

Gibt der Code:
    
    URL to manifest for client streaming using Smooth Streaming protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest
    URL to manifest for client streaming using HLS protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest(format=m3u8-aapl)
    URL to manifest for client streaming using MPEG DASH protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest(format=mpd-time-csf)
    

>[AZURE.NOTE]Sie können auch den Inhalt über eine SSL-Verbindung übertragen. Dazu sicherzustellen Sie, dass Ihre streaming URLs mit HTTPS beginnen. 

Erstellen von schrittweisen Download-URLs 

    private static void BuildProgressiveDownloadURLs(IAsset asset)
    {
        // Create a 30-day readonly access policy. 
        IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
            TimeSpan.FromDays(30),
            AccessPermissions.Read);
    
        // Create an OnDemandOrigin locator to the asset. 
        ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
            policy,
            DateTime.UtcNow.AddMinutes(-5));
    
        // Display some useful values based on the locator.
        Console.WriteLine("Streaming asset base path on origin: ");
        Console.WriteLine(originLocator.Path);
        Console.WriteLine();
    
        // Get MP4 files.
        IEnumerable<IAssetFile> mp4AssetFiles = asset
            .AssetFiles
            .ToList()
            .Where(af => af.Name.EndsWith(".mp4", StringComparison.OrdinalIgnoreCase));
                
        // Create a full URL to the MP4 files. Use this to progressively download files.
        foreach (var pd in mp4AssetFiles)
            Console.WriteLine(originLocator.Path + pd.Name);
    }

Gibt der Code:
    
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4
    
    . . . 

###<a name="use-media-services-net-sdk-extensions"></a>Verwenden von Medien Services .NET SDK Erweiterungen

Der folgende Code ruft .NET SDK Erweiterungsmethoden, mit die einen Locator erstellen und die interpolierten Streaming, HLS und MPEG-Strich URLs für adaptives streaming generieren.

    // Create a loctor.
    _context.Locators.Create(
        LocatorType.OnDemandOrigin,
        inputAsset,
        AccessPermissions.Read,
        TimeSpan.FromDays(30));
    
    // Get the streaming URLs.
    Uri smoothStreamingUri = inputAsset.GetSmoothStreamingUri();
    Uri hlsUri = inputAsset.GetHlsUri();
    Uri mpegDashUri = inputAsset.GetMpegDashUri();
    
    Console.WriteLine(smoothStreamingUri);
    Console.WriteLine(hlsUri);
    Console.WriteLine(mpegDashUri);


##<a name="media-services-learning-paths"></a>Media-Dienste Learning Wege

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Siehe auch

[Herunterladen von Anlagen](media-services-deliver-asset-download.md)
[Konfigurieren Anlage Übermittlung Richtlinie](media-services-dotnet-configure-asset-delivery-policy.md)

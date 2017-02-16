<properties 
    pageTitle="Entwickeln von Applications Videoplayer" 
    description="Das Thema enthält Links zu Player Framework und -Plug-Ins, mit denen Sie Ihre eigenen Clientanwendungen entwickeln, die Stream-Medien von Media-Dienste nutzen können." 
    authors="Juliako" 
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
    ms.date="09/26/2016"
    ms.author="juliako"/>


#<a name="develop-video-player-applications"></a>Entwickeln von Applications Videoplayer

##<a name="overview"></a>(Übersicht)

Azure Media Services bietet die Tools zum Erstellen von leistungsfähigen, dynamische Player Clientanwendungen für die meisten Plattformen einschließlich erforderlichen: iOS-Geräte, Android, Windows, Windows Phone, Xbox und Set-Top Felder. Dieses Thema enthält auch Links zu SDKs und Player Framework, mit denen Sie Ihre eigenen Clientanwendungen entwickeln, die Stream-Medien von Azure Media-Dienste nutzen können.

##<a name="azure-media-player"></a>Azure MediaPlayer

[Azure Media Player](http://aka.ms/ampinfo) ist ein Web-Videoplayer erstellt, dass Sie Medieninhalten aus Microsoft Azure Media-Dienste auf einer Vielzahl von Browsern und Geräten wiedergeben. Azure Media Player nutzt Branchenstandards, wie HTML5, Medien Quelle Erweiterungen (MSE) und verschlüsselt Medien Erweiterungen (EME) um eine erweiterte adaptive streaming Erfahrung bereitzustellen. Wenn diese Standards nicht auf einem Gerät oder in einem Browser verfügbar sind, wird in Azure Media Player Flash- und Silverlight als Ersatz Technologie verwendet. Unabhängig von der verwendeten Wiedergabe-Technologie haben Entwickler eine einheitliche JavaScript-Benutzeroberfläche für die APIs zugreifen. Dies ermöglicht Inhalte von Azure Media Services served über eine Vielzahl von Geräten und Browsern ohne alle zusätzlichen Aufwand wiedergegeben werden soll.

Media-Dienste von Microsoft Azure ermöglicht Inhalte mit Bindestrich, interpolierten Streaming und streaming-Formaten HLS Zustellung zum Wiedergeben von Inhalten. Azure Media Player berücksichtigt verschiedene Formate und wiedergegeben wird automatisch den optimale Link basierend auf den Platform-Browser-Funktionen. Microsoft Azure Media-Dienste auch ermöglicht eine dynamische Verschlüsselung eines Anlagegutes mit PlayReady-Verschlüsselung oder AES-128-bit-Verschlüsselung Umschlag. Azure Media Player ermöglicht die Verschlüsselung von PlayReady und AES-128-bit-verschlüsselte Inhalte beim ordnungsgemäß konfiguriert. 

Weitere Informationen:

- [Azure MediaPlayer](http://aka.ms/ampinfo)
- [Azure Media Player Dokumentation](http://aka.ms/ampdocs) 
- [Azure MediaPlayer erste Schritte-Blog](https://azure.microsoft.com/blog/2015/04/15/announcing-azure-media-player/)
- [Registrieren Sie sich ein, um mit den neuesten von Azure Media Player auf dem neuesten Stand zu bleiben.](http://aka.ms/ampsignup)
- [Hinzufügen von neuen Features Besprechungsanfragen, Ideen, feedback](http://aka.ms/ampuservoice ) 


##<a name="other-tools-for-creating-player-applications"></a>Andere Tools zum Erstellen von Applications Player

Sie können auch eine der folgenden SDKs verwenden:

- [Kontinuierliche Streaming Client SDK](http://www.iis.net/downloads/microsoft/smooth-streaming) 
- [Interpolierten Streaming Windows Store-App](media-services-build-smooth-streaming-apps.md)
- [Microsoft Media-Plattform: Player Framework](http://playerframework.codeplex.com/) 
- [HTML5 Player Framework-Dokumentation](http://playerframework.codeplex.com/wikipage?title=HTML5%20Player&referringTitle=Documentation) 
- [Microsoft-optimierten Streaming-Plug-In für OSMF](https://www.microsoft.com/download/details.aspx?id=36057) 
- [Zur Lizenzierung Microsoft® interpolierten Streaming-Client Portieren Kit](http://aka.ms/sspk) 
- [Video XBOX Entwicklung](http://xbox.create.msdn.com/) 
 

##<a name="advertising"></a>Werbung

Azure Media-Dienste bieten Unterstützung für Werbung über die Windows Media-Plattform: Player Framework. Player Framework mit Active Directory-stehen für Windows 8, Silverlight, Windows Phone 8 und iOS-Geräte. Jedes Framework Player enthält Beispielcode, der Sie wird gezeigt, wie eine Player-Anwendung zu implementieren. Es gibt drei verschiedene Arten von anzeigen, die Sie in Ihre Medien einfügen können:

Linear – vollständigen Frame anzeigen, die das Hauptfenster Video angehalten werden

Nichtlinear – Überlagerung anzeigen, die angezeigt werden, wie das Hauptfenster Video wiedergegeben wird, in der Regel ein Logo oder ein anderes statischen Bild platziert innerhalb des Players

Companion – anzeigen, die außerhalb des Players angezeigt werden

Anzeigen können an jeder Stelle in das Hauptfenster Video Zeitachse platziert werden. Sie müssen dem Player mitteilen, wann die Anzeige wiedergeben und welche Werbung zur Wiedergabe. Dies wird mithilfe einer Reihe von standard XML-basierten Dateien: Video Ad Service-Vorlage (VAST), digitale Video mehrere Ad Wiedergabeliste (VMAP), Medien abstrakte Abfolge Vorlage (MOTTE) und digitale Video Player Ad-Oberfläche Definition (VPAID). GROßE Dateien angeben, welche Anzeige dargestellt wird. VMAP Dateien festlegen, wann zu verschiedenen Werbung wiedergeben und große XML enthalten. MOTTE Dateien sind eine weitere Möglichkeit zum Sequenz anzeigen, die auch große XML enthalten können. VPAID Dateien definieren Sie eine Schnittstelle zwischen dem video-Player und den Ad oder Ad-Server. Weitere Informationen finden Sie unter [Einfügen von anzeigen](https://msdn.microsoft.com/library/dn387398.aspx).

Informationen zur Unterstützung von geschlossenen Untertitel und Werbung in Live streaming Videos finden Sie unter [unterstützte Untertitel und Ad Einfügemarke Standards](https://msdn.microsoft.com/library/c49e0b4d-357e-4cca-95e5-2288924d1ff3#caption_ad).


##<a name="media-services-learning-paths"></a>Media-Dienste Learning Wege

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Siehe auch

[Einbetten eines MPEG-Strich Adaptive Streaming-Videos in einer Anwendung HTML5 mit DASH.js](media-services-embed-mpeg-dash-in-html5.md)

[GitHub dash.js repository](https://github.com/Dash-Industry-Forum/dash.js)
 

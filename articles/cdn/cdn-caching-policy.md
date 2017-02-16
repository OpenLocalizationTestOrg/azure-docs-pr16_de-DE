<properties
    pageTitle="CDN Zwischenspeichern Richtlinie in Erweiterung Medien"
    description="In diesem Thema bietet einen Überblick über einem CDN Richtlinie in Medien Erweiterung Zwischenspeichern."
    services="media-services,cdn"
    documentationCenter=".NET"
    authors="juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="juliako"/>
 
#<a name="cdn-caching-policy-in-media-services-extension"></a>CDN Zwischenspeichern Richtlinie in Erweiterung Medien

Azure Media Services bietet HTTP Adaptives Streaming und schrittweisen Download basiert. HTTP-basierte streaming ist hochgradig skalierbar mit Vorteile der Zwischenspeichern in Proxy und CDN Layer als auch das clientseitige Zwischenspeichern. Streaming Endpunkte bietet allgemeine streaming-Funktionen und auch Konfiguration für HTTP-Cache-Header. Streaming Endpunkte legt HTTP-Cache-Control: Max-Alter und Kopfzeilen läuft ab. Sie erhalten weitere Informationen zu HTTP-Cache-Header [w3.org](http://www.w3.org/Protocols/rfc2616/rfc2616-sec13.html).

##<a name="default-caching-headers"></a>Standardmäßige Zwischenspeichern Kopfzeilen

Standardmäßig gelten streaming Endpunkte 3-Tages-Cache-Header für bei Bedarf streaming Daten (ist-Medien Satzteile/Blöcken) und manifest(playlist). Live-streaming streaming Endpunkte anwenden 3-Tages-Cache-Header für Daten (ist-Medien Satzteile/Blöcken) und 2 Sekunden zwischenspeichern Kopfzeile für manifest(playlist) Anfragen. Wenn live Programm, um bei Bedarf (live Archiv deaktiviert) wenden Sie bei Bedarf streaming-Cache-Header an.

##<a name="azure-cdn-integration"></a>Azure CDN-integration

Azure Media Services bietet [integrierte CDN](https://azure.microsoft.com/updates/azure-media-services-now-fully-integrated-with-azure-cdn/) für streaming Endpunkte. Cache-Control-Header gilt auf die gleiche Weise wie streaming von Endpunkten in CDN streaming Endpunkte aktiviert. Azure CDN verwendet streaming Endpunkt konfiguriert Cache Werte zum Definieren des Nutzungsdauer intern zwischengespeicherten Objekte und auch dieser Wert verwendet, um die Übermittlung Cache Kopfzeilen festzulegen. Wenn mit CDN streaming Endpunkte aktiviert empfiehlt es nicht kleine Cache Werte. Kleine Werte wird die Leistung beeinträchtigen und reduzieren die Vorteile von CDN. Es ist nicht zulässig, Cache Kopfzeilen kleiner als 600 Sekunden für CDN streaming Endpunkte aktiviert festlegen.

>[AZURE.IMPORTANT] Azure Media Services-Integration in Azure CDN wird auf **Azure CDN von Verizon**implementiert.  Wenn Sie **Azure CDN von Akamai** für Azure Media-Dienste verwenden möchten, müssen Sie [den Endpunkt manuell konfigurieren](cdn-create-new-endpoint.md).  Weitere Informationen zu den Features von Azure CDN finden Sie unter der [CDN Übersicht](cdn-overview.md).

##<a name="configuring-cache-headers-with-azure-media-services"></a>Konfigurieren von Cache Überschriften mit Azure Media Services

Azure-Verwaltungsportal oder Azure Media Services-APIs können Sie Werte für die Kopfzeile Cache konfiguriert werden.

1. Zum Konfigurieren der Cache Überschriften mit Management-Portal Näheres [zum Verwalten von Streaming Endpunkte](../media-services/media-services-portal-manage-streaming-endpoints.md) Abschnitt Konfiguration des Endpunkts Streaming.
2. Azure Media Services REST-API, [StreamingEndpoint](https://msdn.microsoft.com/library/azure/dn783468.aspx#StreamingEndpointCacheControl).
3. Azure Media Services .NET SDK, [StreamingEndpointCacheControl Eigenschaften](http://go.microsoft.com/fwlink/?LinkId=615302).

##<a name="cache-configuration-precedence-order"></a>Cache Konfiguration Rangfolge

1. Azure Media-Dienste konfiguriert Cachewert überschreibt Standardwert.
2. Ist keine manuelle Konfiguration, Standardwerte angewendet wird.
3. Durch Standard 2 Sekunden Cache gilt für Überschriften Live-streaming manifest(playlist) unabhängig von Azure Medien oder Azure-Speicher Konfiguration, und Überschreiben dieses Werts steht nicht zur Verfügung.

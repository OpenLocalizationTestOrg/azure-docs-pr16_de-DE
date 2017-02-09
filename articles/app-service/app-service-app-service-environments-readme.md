<properties 
    pageTitle="App-Service-Umgebung | Microsoft Azure" 
    description="Was ist eine Azure-App-Service-Umgebung? Eine Einführung in die App-Service-Umgebung." 
    keywords="app Azure Service-Umgebung, virtuelles Netzwerk secure Netzwerke"
    services="app-service" 
    documentationCenter="" 
    authors="stefsch" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="stefsch"/>

# <a name="app-service-environment-documentation"></a>App-Umgebung Servicedokumentation

Eine App-Service-Umgebung ist eine [Premium] [ PremiumTier] service Plan Möglichkeit, Azure-App-Dienst, der eine vollständig isolierte und dedizierte Umgebung für die Ausführung von Azure-App-Verwaltungsdienst apps sicher bei hoch, einschließlich der [Web Apps]ermöglicht[WebApps], [Mobile-Apps][MobileApps], und [API Apps][APIApps].  

App-Service-Umgebungen eignen sich optimal für Auslastung mit Anforderung:

- Sehr hoch-Farben-Skala
- Grad der Isolation und sicheren Netzwerkzugriff

Kunden können mehrere App Service-Umgebungen in einem einzigen Azure Bereich und über mehrere Azure Regionen erstellen.  Dadurch wird die App-Service-Umgebungen für die horizontale Skalierung Ebenen der Zustand zugängliche Anwendung zur Unterstützung hoher Auslastung von RPS eignet.

App-Service-Umgebungen isoliert nur einen einzelnen Kunden Programme ausgeführt werden, und immer in einem virtuellen Netzwerk bereitgestellt werden.  Kunden haben abgestimmte Kontrolle über beide eingehenden und ausgehenden Anwendung Netzwerkverkehr mithilfe von [Netzwerk-Sicherheitsgruppen][NetworkSecurityGroups].  Applikationen können auch schnelle sichere Verbindungen über virtuelle Netzwerke zu lokalen Ihres Unternehmens Ressourcen einrichten.

Apps müssen häufig auf Ihres Unternehmens Ressourcen wie interne Datenbanken zugreifen und web Services.  Apps für App-Service-Umgebungen ausgeführt Zugriff auf Ressourcen, die über die [Standort-zu-Standort] erreichbar[ SiteToSite] VPN und [Azure ExpressRoute] [ ExpressRoute] Verbindungen.

* [Was ist eine App-Service-Umgebung?](../app-service-web/app-service-app-service-environment-intro.md)
* [Erstellen einer App-Service-Umgebung](../app-service-web/app-service-web-how-to-create-an-app-service-environment.md)
* [Erstellen von Apps in einer App-Service-Umgebung](../app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md)
* [Erstellen und verwenden eine interne Lastenausgleich mit App-Service-Umgebungen](../app-service-web/app-service-environment-with-internal-load-balancer.md)
* [Konfigurieren einer App-Service-Umgebung](../app-service-web/app-service-web-configure-an-app-service-environment.md) 
* [Anpassungsbereich für Apps in einer App-Service-Umgebung](../app-service-web/app-service-web-scale-a-web-app-in-an-app-service-environment.md)
* [Netzwerk-Sicherheit und Architektur](../app-service-web/app-service-app-service-environment-network-architecture-overview.md)

## <a name="how-tos"></a>So des

[AZURE.INCLUDE [app-service-blueprint-app-service-environment](../../includes/app-service-blueprint-app-service-environment.md)]


## <a name="videos"></a>Videos
[AZURE.VIDEO azurecon-2015-deploying-highly-scalable-and-secure-web-and-mobile-apps]

[AZURE.VIDEO microsoft-ignite-2015-running-enterprise-web-and-mobile-apps-on-azure-app-service]


<!-- LINKS -->
[PremiumTier]: http://azure.microsoft.com/pricing/details/app-service/
[WebApps]: http://azure.microsoft.com/documentation/articles/app-service-web-overview/
[MobileApps]: http://azure.microsoft.com/documentation/articles/app-service-mobile-value-prop-preview/
[APIApps]: http://azure.microsoft.com/documentation/articles/app-service-api-apps-why-best-platform/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[SiteToSite]: https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/

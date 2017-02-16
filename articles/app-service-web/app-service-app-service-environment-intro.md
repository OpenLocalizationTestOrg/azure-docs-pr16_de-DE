<properties 
    pageTitle="Einführung in die App-Service-Umgebung" 
    description="Lernen Sie die App-Service-Umgebung Feature, das sicherer VNet verknüpft, dedizierten Maßeinheiten für die Ausführung aller Ihrer apps bereitstellt." 
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

# <a name="introduction-to-app-service-environment"></a>Einführung in die App-Service-Umgebung

## <a name="overview"></a>(Übersicht) ##
Eine App-Service-Umgebung ist eine [Premium] [ PremiumTier] service Plan Möglichkeit, Azure-App-Dienst, der eine vollständig isolierte und dedizierte Umgebung für die Ausführung von App-Verwaltungsdienst Azure apps sicher bei hoch, einschließlich der [Web Apps]ermöglicht[WebApps], [Mobile-Apps][MobileApps], und [API Apps][APIApps].  

App-Service-Umgebungen eignen sich optimal für Auslastung mit Anforderung:

- Sehr hoch-Farben-Skala
- Grad der Isolation und sicheren Netzwerkzugriff

Kunden können mehrere App Service-Umgebungen in einem einzigen Azure Bereich und über mehrere Azure Regionen erstellen.  Dadurch wird die App-Service-Umgebungen für die horizontale Skalierung Ebenen der Zustand zugängliche Anwendung zur Unterstützung hoher Auslastung von RPS eignet.

App-Service-Umgebungen isoliert nur einen einzelnen Kunden Programme ausgeführt werden, und immer in einem virtuellen Netzwerk bereitgestellt werden.  Kunden haben abgestimmte Kontrolle über beide eingehenden und ausgehenden Anwendung Netzwerkverkehr und Applikationen können schnelle sichere Verbindungen über virtuelle Netzwerke zu lokalen Ihres Unternehmens Ressourcen herstellen.

Alle Artikel und wie-über die App-Service-Umgebungen stehen in der [Infodatei für Service-Umgebungen Anwendung](../app-service/app-service-app-service-environments-readme.md).

Einen Überblick darüber, wie App-Service-Umgebungen Aktivieren von hohem skalieren und abzusichern Netzwerkzugriff, finden Sie unter der [AzureCon aneignen] [ AzureConDeepDive] auf App-Service-Umgebungen!

Eine tief greifende auf horizontale Skalierung mit mehreren App-Service-Umgebungen finden Sie im Artikel zum Einrichten einer [app Geo verteilt Platzbedarf][GeodistributedAppFootprint].

Um anzuzeigen, wie die Sicherheitsarchitektur dargestellt, in der AzureCon aneignen konfiguriert wurde, finden Sie im Artikel zum Implementieren eines [Wertpapiers Architektur überlappende](app-service-app-service-environment-layered-security.md) mit App-Service-Umgebungen.

Apps auf App-Service-Umgebungen ausführen können deren gated vom übergeordneten Geräten wie Web Anwendung Firewalls (WAF) zugreifen können.  Zum [Konfigurieren einer WAF für App-Service-Umgebungen](app-service-app-service-environment-web-application-firewall.md) werden dieses Szenario behandelt. 

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="dedicated-compute-resources"></a>Berechnen von dedizierten Ressourcen ##
Alle Ressourcen in einer App-Service-Umgebung berechnen engagieren uns ausschließlich für ein einzelnes Abonnement und einer App-Service-Umgebung kann mit bis zu 50 (50) berechnen Ressourcen für den exklusiven Zugriff durch eine einzelne Anwendung konfiguriert werden.

Eine App-Service-Umgebung besteht aus einer Front-End-berechnen Ressourcenpool als auch ein bis drei Worker berechnen Ressourcenpools. 

Der Front-End-Pool enthält berechnen Ressourcen für SSL-Beendigung als auch automatischen Lastenausgleich der app-Anfragen innerhalb einer App-Service-Umgebung verantwortlich ist. 

Jeder Worker Pool enthält berechnen zugeordneten Ressourcen reserviert, [App Service-Pläne][AppServicePlan], die wiederum eine oder mehrere Azure-App-Verwaltungsdienst-apps enthalten.  Da in einer App-Service-Umgebung bis zu drei verschiedene Worker Pools vorhanden sein kann, müssen Sie die Flexibilität können Sie andere berechnen Ressourcen für jeden Worker Pool auswählen.  

Beispielsweise können Sie eine Worker Ressourcenpool mit weniger leistungsfähige berechnen Ressourcen für die App für die Entwicklung oder Test apps vorgesehen Service-Pläne erstellen.  Eine zweite (oder sogar dritte) Worker Ressourcenpool könnten leistungsfähigere berechnen Ressourcen für die App Service-Pläne ausgeführt Herstellung apps vorgesehen verwenden.

Weitere Details auf die Menge der berechnen Ressourcen der Front-End- und Worker Pools zur Verfügung, finden Sie unter [So konfigurieren Sie eine App-Service-Umgebung][HowToConfigureanAppServiceEnvironment].  

Weitere Informationen über die verfügbaren berechnen Ressource Größen in einer App-Service-Umgebung unterstützt, wenden Sie sich an die [App Preisen] [ AppServicePricing] Seite, und überprüfen Sie die verfügbaren Optionen für App-Dienst Umgebungen in die Preise Ebene Premium.

## <a name="virtual-network-support"></a>Virtuelle Netzwerk-Support ##
Eine App-Service-Umgebung erstellt werden können in **entweder** ein Ressourcenmanager Azure-virtuellen Netzwerk, Modellieren **oder** eine klassische Bereitstellung virtuelles Netzwerk ([Weitere Informationen auf virtuelle Netzwerke][MoreInfoOnVirtualNetworks]).  Da eine App Service-Umgebung immer in einem virtuellen Netzwerk und genauer innerhalb einer Subnetz eines virtuellen Netzwerks vorhanden ist, können Sie die Sicherheitsfeatures von virtuellen Netzwerken so steuern Sie beide eingehenden und ausgehenden Netzwerkkommunikation nutzen.  

Eine App-Service-Umgebung kann entweder Internet gegenüberliegende mit einer öffentlichen IP-Adresse oder internen gegenüberliegende nur eine Adresse zu Azure internen laden Lastenausgleich (ILB) sein.

[Netzwerk-Sicherheitsgruppen] können[ NetworkSecurityGroups] zum Einschränken des eingehenden Netzwerkkommunikation mit dem Subnetz, in einer App-Service-Umgebung gespeichert ist.  So können Sie apps hinter übergeordneten Geräte und Dienste wie Web Anwendung Firewalls und Netzwerk SaaS Anbieter ausführen.

Apps müssen auch häufig auf Ihres Unternehmens Ressourcen wie interne Datenbanken zugreifen und web Services.  Ein gemeinsames Konzept ist diese Endpunkte nur für interne Netzwerkverkehr parallelen in einem Azure-virtuellen Netzwerk zur Verfügung stellen.  Nachdem eine App-Service-Umgebung im gleichen virtuellen Netzwerk als den internen Diensten hinzugefügt wird, der Umgebung apps können darauf zugreifen, einschließlich Endpunkten über [Zwischen Standorten] erreichbar[ SiteToSite] und [Azure ExpressRoute] [ ExpressRoute] Verbindungen.

Für weitere Details zur Funktionsweise von App-Service-Umgebungen mit virtuelle Netzwerke und lokale Netzwerke den folgenden Artikeln auf [Netzwerkarchitektur]wenden Sie sich an[NetworkArchitectureOverview], [Eingehender Datenverkehr steuern][ControllingInboundTraffic], und eine [Sichere Verbindung]zu Downloadzeit[SecurelyConnectingToBackends]. 

## <a name="getting-started"></a>Erste Schritte

Um mit der App-Service-Umgebungen anzufangen, finden Sie unter [Wie zum Erstellen einer App Service-Umgebung][HowToCreateAnAppServiceEnvironment]

Alle Artikel und wie-des für App-Service-Umgebungen in der [Infodatei für Service-Umgebungen Anwendung](../app-service/app-service-app-service-environments-readme.md)verfügbar sind.

Weitere Informationen über die App-Verwaltungsdienst Azure-Plattform finden Sie unter [Azure-App-Verwaltungsdienst][AzureAppService].

Finden Sie unter Übersicht über die Netzwerkarchitektur App-Service-Umgebung, die [Network-Architektur (Übersicht)] [ NetworkArchitectureOverview] Artikel.

Detaillierte Informationen zur Verwendung einer App-Service-Umgebung mit ExpressRoute, finden Sie im folgenden Artikel auf [Express-Routing und App-Service-Umgebungen][NetworkConfigDetailsForExpressRoute].

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[PremiumTier]: http://azure.microsoft.com/pricing/details/app-service/
[MoreInfoOnVirtualNetworks]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePlan]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[HowToCreateAnAppServiceEnvironment]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[WebApps]: http://azure.microsoft.com/documentation/articles/app-service-web-overview/
[MobileApps]: http://azure.microsoft.com/documentation/articles/app-service-mobile-value-prop-preview/
[APIApps]: http://azure.microsoft.com/documentation/articles/app-service-api-apps-why-best-platform/
[LogicApps]: http://azure.microsoft.com/documentation/articles/app-service-logic-what-are-logic-apps/
[AzureConDeepDive]:  https://azure.microsoft.com/documentation/videos/azurecon-2015-deploying-highly-scalable-and-secure-web-and-mobile-apps/
[GeodistributedAppFootprint]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-geo-distributed-scale/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[SiteToSite]: https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[HowToConfigureanAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
[ControllingInboundTraffic]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[SecurelyConnectingToBackends]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-securely-connecting-to-backend-resources/
[NetworkArchitectureOverview]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-architecture-overview/
[NetworkConfigDetailsForExpressRoute]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 

<!-- IMAGES -->

 

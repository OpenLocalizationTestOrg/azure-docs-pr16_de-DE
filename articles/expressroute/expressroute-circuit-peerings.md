<properties 
   pageTitle="ExpressRoute Schaltkreise und Domänen routing | Microsoft Azure"
   description="Diese Seite enthält eine Übersicht über ExpressRoute Schaltkreise und den Domänen routing."
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carmonm"
   editor=""/>
<tags 
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="10/10/2016"
   ms.author="cherylmc"/>

# <a name="expressroute-circuits-and-routing-domains"></a>ExpressRoute Schaltkreise und Domänen routing

 Sie müssen ein *ExpressRoute Verbindung* Verbindung Ihrer lokalen Infrastruktur an Microsoft über einen Anbieter Connectivity bestellen. Die folgende Abbildung enthält eine logische Darstellung der Verbindung zwischen Ihrem WAN und Microsoft.

![](./media/expressroute-circuit-peerings/expressroute-basic.png)

## <a name="expressroute-circuits"></a>Schaltkreise ExpressRoute

Ein *ExpressRoute Verbindung* stellt eine logische Verbindung zwischen Ihrem lokalen Infrastruktur und Microsoft-Cloud-Diensten über einen Connectivity-Anbieter. Sie können mehrere ExpressRoute Schaltkreise bestellen. Jede Verbindung in der gleichen oder in verschiedenen Regionen werden kann, und mit ihren lokale durch andere Connectivity-Anbieter verbunden werden kann. 

ExpressRoute Schaltkreise keine physischen Einheiten zugeordnet werden. Eine Verbindung wird durch einen Standard GUID als Schlüssel Service (s-Tasten) aufgerufen, eindeutig identifiziert. Der Schlüssel ist der einzige Information zwischen Microsoft und den Anbieter Connectivity ausgetauscht. Die s-Taste ist keinen Schlüssel für die Sicherheit. Es gibt eine 1:1-Zuordnung zwischen einer ExpressRoute Verbindung und die s-Taste.

Eine Verbindung ExpressRoute kann bis zu drei unabhängigen Peerings haben: Azure öffentlichen, Azure privat und Microsoft. Jede peering besteht aus zwei unabhängigen BGP Sitzungen jede hiervon redundant für hohe Verfügbarkeit konfiguriert. Es gibt eine 1: n (1 < = N < = 3) zwischen einer Verbindung ExpressRoute Zuordnung und Domänen routing. Eine Verbindung ExpressRoute kann eine, zwei oder alle drei Peerings pro ExpressRoute Verbindung aktiviert haben.
 
Jede Verbindung verfügt über eine feste Bandbreite (50 MB /, 100 MB/s, 200 MB, 500 MB/s, 1 Gbps; 10 Gbps) und einen Anbieter Konnektivität und einen Speicherort Peeringliste zugeordnet ist. Ist die Bandbreite, Sie wählen, über alle Peerings für die Verbindung freigegeben werden. 

### <a name="quotas-limits-and-limitations"></a>Kontingente, Grenzwerte und Einschränkungen

Standardkontingente und Grenzwerte wenden Sie für jede ExpressRoute Verbindung. Schlagen Sie in der Seite [Azure-Abonnement und Beschränkungen Service, Kontingente, und Einschränkungen](../azure-subscription-service-limits.md) für die aktuelle Informationen zu Kontingenten.

## <a name="expressroute-routing-domains"></a>ExpressRoute routing-Domänen

Eine Verbindung ExpressRoute weist mehrere routing Domänen zugeordnet: Azure öffentlichen, Azure privat und Microsoft. Jeder dieser Domänen routing auf einer Routerpaar identisch konfiguriert ist (aktive oder laden Freigabe Konfiguration) für eine hohe Verfügbarkeit. Azure Services werden als *Azure öffentlichen* und *privaten Azure* zum Darstellen des IP adressieren Phishingversuchen kategorisiert.


![](./media/expressroute-circuit-peerings/expressroute-peerings.png)


### <a name="private-peering"></a>Private peering

Azure berechnen Dienste, nämlich virtuellen Computern (IaaS), und Cloud Services (PaaS), die in einem virtuellen Netzwerk bereitgestellt werden durch die private Peeringliste Domäne verbunden werden können. Die private Peeringliste Domäne gilt als vertrauenswürdig Erweiterung Ihres Netzwerks Core in Microsoft Azure. Sie können die bidirektionale Konnektivität zwischen Ihrem Netzwerk Core und Azure-virtuellen Netzwerken (VNets) einrichten. Diese peering können Sie eine Verbindung mit virtuellen Computern und cloud Services direkt auf ihre privaten IP-Adressen.  

Sie können mehrere virtuelle Netzwerke mit der privaten Peeringliste Domäne verbinden. Überprüfen der [FAQ-Webseite](expressroute-faqs.md) Informationen Grenzwerte und Einschränkungen. Sie können die Seite [Azure-Abonnement und Beschränkungen Service, Kontingente, und Einschränkungen](../azure-subscription-service-limits.md) für die aktuelle Informationen auf Grenzwerte besuchen.  Schlagen Sie in der Seite [Routing](expressroute-routing.md) detaillierte Informationen zur Konfiguration für routing.

### <a name="public-peering"></a>Öffentliche peering

Klicken Sie auf öffentliche IP-Adressen werden Diensten wie Azure-Speicher, SQL-Datenbanken und Websites angeboten. Sie können auf Dienste auf öffentliche IP-Adressen, einschließlich VIPs Ihrer Cloud-Dienste, bis der öffentlichen Peeringliste routing-Domäne gehostet privat verbinden. Sie können Herstellen einer Verbindung Ihrer DMZ mit der öffentlichen Peeringliste Domäne und Herstellen einer Verbindung mit allen Azure Dienste auf ihre öffentlichen IP-Adressen aus Ihrem WAN ohne Verbindung über das Internet. 

Konnektivität wird immer aus Ihrem WAN mit Microsoft Azure Services initiiert. Microsoft Azure Services möglich einleiten Verbindungen in Ihrem Netzwerk durch routing-Domäne nicht. Nachdem öffentlichen peering aktiviert ist, werden Sie zu allen Azure Services eine Verbindung herstellen können. Wir sind nicht ausreichend Selektives Services auswählen, für denen wir leitet zu ankündigen. Sie können die Liste der Präfixe überprüfen, die wir Ihnen ankündigen durch diese peering auf der Seite [Microsoft Azure Datacenter IP-Bereiche](http://www.microsoft.com/download/details.aspx?id=41653) . Die Seite wird wöchentlich aktualisiert.

Sie können benutzerdefinierte Routing Filter definieren, in Ihrem Netzwerk nur die Arbeitspläne nutzen, die Sie benötigen. Schlagen Sie in der Seite [Routing](expressroute-routing.md) detaillierte Informationen zur Konfiguration für routing. Sie können benutzerdefinierte Routing Filter definieren, in Ihrem Netzwerk nur die Arbeitspläne nutzen, die Sie benötigen. 

Services unterstützt, bis der öffentlichen Peeringliste routing-Domäne finden Sie auf der [FAQ-Webseite](expressroute-faqs.md) für Weitere Informationen. 
 
### <a name="microsoft-peering"></a>Microsoft peering

[AZURE.INCLUDE [expressroute-office365-include](../../includes/expressroute-office365-include.md)]

Verbindung zu allen anderen Microsoft-Onlinedienste (beispielsweise Office 365-Diensten) werden durch die Microsoft peering. Wir ermöglichen bidirektionale Konnektivität zwischen Ihrer WAN und Microsoft Cloud-Diensten über die Microsoft Peeringliste routing-Domäne. Sie müssen die Verbindung zu Microsoft-Cloud-Diensten nur über einen öffentlichen IP-Adressen, die von Ihnen oder Ihrem Anbieter Connectivity besitzt, und Sie müssen alle definierten Regeln entsprechen. Die Seite [ExpressRoute Voraussetzungen für](expressroute-prerequisites.md) Weitere Informationen finden Sie unter.

Finden Sie weitere Informationen zu den Services unterstützt, Kosten und Details zur Konfiguration der [FAQ-Webseite](expressroute-faqs.md) . Informationen finden Sie die [ExpressRoute Orte](expressroute-locations.md) Seite in der Liste der Connectivity-Anbieter Geschenk Peeringliste Microsoft-Support.

## <a name="routing-domain-comparison"></a>Vergleich der Weiterleitung Domäne

In der folgenden Tabelle werden drei Domänen routing verglichen.

||**Private Peering**|**Öffentliche Peering**|**Microsoft Peering**|
|---|---|---|---|
|**Max. # Präfixe pro peering unterstützt**|standardmäßig 10.000 mit ExpressRoute Premium 4000|200|200|
|**Unterstützte IP-Adressbereiche**|Eine gültige IPv4-Adresse in Ihrem WAN.|Öffentliche IPv4-Adressen im Besitz Sie oder Ihr Connectivity-Anbieter.|Öffentliche IPv4-Adressen im Besitz Sie oder Ihr Connectivity-Anbieter.|
|**ALS Zahl Anforderungen**|Private und öffentliche als Zahlen. Sie müssen der Besitzer der Öffentlichkeit als Zahl, wenn Sie eine verwenden. | Private und öffentliche als Zahlen. Sie müssen jedoch Besitzrechte für die öffentliche IP-Adressen nachweisen.| Private und öffentliche als Zahlen. Sie müssen jedoch Besitzrechte für die öffentliche IP-Adressen nachweisen.|
|**Weiterleitung Interface IP-Adressen**|RFC1918 und öffentliche IP-Adressen|Öffentliche IP-Adressen, die Sie in der Weiterleitung Register registriert.| Öffentliche IP-Adressen, die Sie in der Weiterleitung Register registriert.|
|**MD5-Hash-support**| Ja|Ja|Ja|

Sie können auch eine oder mehrere Domänen routing als Teil ihrer ExpressRoute Verbindung zu aktivieren. Sie können auswählen, dass alle Weiterleitung Domänen, die auf dem gleichen VPN setzen, wenn sie zu einer einzigen Weiterleitung Domäne kombiniert werden sollen. Sie können auch auf andere routing-Domänen, ähnlich wie das Diagramm einfügen. Empfohlene Konfiguration ist, dass private peering direkt mit dem Core-Netzwerk verbunden ist, und die öffentlichen und Microsoft Peeringverbindungen mit Ihrer DMZ verbunden sind.
 
Wenn Sie alle drei Peeringliste Sitzungen lassen, müssen Sie drei Paare von BGP Sitzungen (ein Paar für jeden Peeringliste) haben. Die BGP Sitzung-Paare bereitstellen ein hoch verfügbaren Links. Wenn Sie bis Ebene 2 Connectivity-Anbieter verbunden sind, werden Sie für das Konfigurieren und Verwalten von routing verantwortlich sein. Sie können die [Workflows](expressroute-workflows.md) für die Einrichtung von ExpressRoute überprüfen, um mehr erfahren.

## <a name="next-steps"></a>Nächste Schritte

- Suchen nach einem Dienstanbieter. Finden Sie unter [ExpressRoute-Dienstanbieter und Speicherorte](expressroute-locations.md).
- Stellen Sie sicher, dass alle erforderlichen Komponenten vorhanden sind. Finden Sie unter [Voraussetzungen für die ExpressRoute](expressroute-prerequisites.md).
- Konfigurieren Sie die Verbindung ExpressRoute.
    - [Erstellen einer Verbindung ExpressRoute](expressroute-howto-circuit-classic.md)
    - [Konfigurieren der Weiterleitung (Verbindung Peerings)](expressroute-howto-routing-classic.md)
    - [Verknüpfen eines VNet zu einer ExpressRoute Verbindung](expressroute-howto-linkvnet-classic.md)

<properties
   pageTitle="Verbinden von Ihrem lokalen Netzwerk mit Azure | Microsoft Azure"
   description="Erläutert und vergleicht die unterschiedlichen Methoden zum Herstellen einer Verbindung mit Microsoft-Cloud-Diensten wie Azure, Office 365 und Dynamics CRM Online."
   services=""
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags=""/>
<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/25/2016"
   ms.author="jdial"/>
   
# <a name="connecting-your-on-premises-network-to-azure"></a>Verbinden von Ihrem lokalen Netzwerk mit Azure

Microsoft bietet mehrere Typen von Cloud-Dienste. Während Sie auf alle Dienste über das öffentliche Internet herstellen können, können Sie auch auf einige der Dienste verwenden einen Tunnel virtuelles privates Netzwerk (VPN) über das Internet oder einer direkten Verbindung zu Microsoft verbinden. In diesem Artikel können Sie bestimmen, welche Konnektivitätsoption am besten wird Ihre Bedürfnisse basierend auf den Typen von Microsoft-Cloud-Diensten, die Sie nutzen. Die meisten Organisationen nutzen mehrere Verbindungstypen nachfolgend beschriebenen.

## <a name="connecting-over-the-public-internet"></a>Herstellen einer Verbindung über das Internet öffentlichen

Dieser Verbindungstyp ermöglicht den Zugriff auf Microsoft-Cloud-Diensten direkt über das Internet, wie unten dargestellt.

![Internet] (./media/guidance-connecting-your-on-premises-network-to-azure/internet.png "Internet")

Diese Verbindung ist normalerweise der erste Typ für das Herstellen einer Verbindung mit Microsoft-Cloud-Diensten verwendet. Die folgende Tabelle enthält die vor- und Nachteile dieses Typs Verbindung.



| **Vorteile**| **Aspekte**|
|---------|---------|
|Keine Änderung mit Ihrem lokalen Netzwerk erfordert, solange alle Client-Geräte haben Unbeschränkter Zugriff auf alle IP-Adressen und Ports im Internet.|Obwohl Datenverkehr häufig mit HTTPS verschlüsselt ist, kann bei der Übertragung abgefangen werden, da sie das öffentliche Internet durchläuft.|
|Alle Microsoft-Cloud-Dienste, die im öffentlichen Internet offen gelegt können verbinden.|Unvorhergesehene Wartezeit, da die Verbindung mit das Internet durchläuft.|
|Verwendet den vorhandenen Internetzugang an.||
|Verwaltung von einem beliebigen Connectivity-Geräten nicht erforderlich ist.||

Diese Verbindung hat keine Konnektivität oder Kosten für Bandbreite, da Sie den vorhandenen Internetzugang verwenden. 

## <a name="connecting-with-a-point-to-site-connection"></a>Herstellen einer Verbindung mit einer Punkt-zu-Standort-Verbindung

Dieser Verbindungstyp ermöglicht den Zugriff auf einige Microsoft Cloud-Dienste über einen Tunnel Secure Sockets Tunnel Protokoll (SSTP) über das Internet, wie unten dargestellt.

![P2S] (./media/guidance-connecting-your-on-premises-network-to-azure/p2s.png "Punkt-zu-Standort-Verbindung")

Die Verbindung über Ihre bestehende Internet-Verbindung besteht, jedoch benötigen Sie eine Azure VPN-Gateway. Die folgende Tabelle enthält die vor- und Nachteile dieses Typs Verbindung.

| **Vorteile**| **Aspekte**|
|---------|---------|
|Keine Änderung mit Ihrem lokalen Netzwerk erfordert, solange alle Client-Geräte haben Unbeschränkter Zugriff auf alle IP-Adressen und Ports im Internet.|Obwohl Datenverkehr mit IPSec verschlüsselt ist, kann bei der Übertragung abgefangen werden, da sie das öffentliche Internet durchläuft.|
|Verwendet den vorhandenen Internetzugang an.|Unvorhergesehene Wartezeit, da die Verbindung mit das Internet durchläuft.|
|Datendurchsatz von bis zu 200 Mb/s pro Gateway.|Erfordert erstellen und Verwalten von separaten Verbindungen zwischen jedem Gerät in Ihrem lokalen Netzwerk und jedes Gateway, jedem Gerät zu verbinden muss.|
|Kann verwendet werden zu den Azure Services herstellen, die mit einer Azure-virtuellen Netzwerken (VNet) verbunden sein können wie Azure-virtuellen Computern und Azure-Cloud-Dienste.|Erfordert minimale laufenden Verwaltung einer Azure VPN-Gateway.|
||Kann keine Verbindung zu Microsoft Office 365 oder Dynamics CRM Online verwendet werden.
||Kann nicht verwendet werden, um mit Azure Webdiensten herstellen, die mit einer VNet verbunden werden kann.|

Weitere Informationen zu den [VPN-Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md) -Dienst, deren [Preise](https://azure.microsoft.com/pricing/details/vpn-gateway)und ausgehende Daten durchstellen [Preise](https://azure.microsoft.com/pricing/details/data-transfers).

## <a name="connecting-with-a-site-to-site-connection"></a>Herstellen einer Verbindung mit einer Verbindung zwischen Standorten

Dieser Verbindungstyp ermöglicht den Zugriff auf einige Microsoft-Cloud-Diensten über einen IPSec-Tunnel über das Internet, wie unten dargestellt.

![S2S] (./media/guidance-connecting-your-on-premises-network-to-azure/s2s.png "Website-zu-Standort-Verbindung")

Die Verbindung über Ihre bestehende Internet-Verbindung besteht, jedoch benötigen Sie eine Azure VPN-Gateway mit dem zugehörigen Preise und ausgehende Datenübertragung Preise. Die folgende Tabelle enthält die vor- und Nachteile dieses Typs Verbindung.

| **Vorteile**| **Aspekte**|
|---------|---------|
|Alle Geräte in Ihrem lokalen Netzwerk können mit Azure Services mit einer VNet verbunden ist, damit es nicht erforderlich ist, so konfigurieren Sie einzelne Verbindungen für jedes Gerät kommunizieren.|Obwohl Datenverkehr mit IPSec verschlüsselt ist, kann bei der Übertragung abgefangen werden, da sie das öffentliche Internet durchläuft.|
|Verwendet den vorhandenen Internetzugang an.|Unvorhergesehene Wartezeit, da die Verbindung mit das Internet durchläuft.|
|Kann Verbindung zum Azure Services, die mit einem VNet wie virtuellen Computern verbunden werden können und Cloud-Diensten verwendet werden.|Konfigurieren und Verwalten einer überprüften VPN-Gerät * müssen lokale.|
|Datendurchsatz von bis zu 200 Mb/s pro Gateway.|Erfordert minimale laufenden Verwaltung einer Azure VPN-Gateway.|
|Ausgehenden Datenverkehr initiiert von Cloud-virtuellen Computern über das lokale Netzwerk für Prüfung und Protokollierung mithilfe benutzerdefinierter weitergeleitet oder Rahmen Gateway Protocol (BGP) können erzwingen **.|Kann keine Verbindung zu Microsoft Office 365 oder Dynamics CRM Online verwendet werden.|
||Kann nicht verwendet werden, um mit Azure Webdiensten herstellen, die mit einer VNet verbunden werden kann.|
||Wenn Sie die Dienste, die Verbindungen wieder auf lokale Geräte initiieren verwenden und Ihre Sicherheitsrichtlinien als erforderlich, benötigen Sie möglicherweise eine Firewall zwischen dem lokalen Netzwerk und Azure.|

- * Anzeigen einer Liste von [VPN-Geräte überprüft](../vpn-gateway/vpn-gateway-about-vpn-devices.md#validated-vpn-devices).
- ** Weitere Informationen zum Verwenden von [benutzerdefinierten weitergeleitet](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md) oder [BGP](../vpn-gateway/vpn-gateway-bgp-overview.md) So erzwingen Sie routing von Azure VNets zu einem lokalen Gerät.

## <a name="connecting-with-a-dedicated-private-connection"></a>Herstellen einer Verbindung mit einer dedizierten privaten Verbindung

Dieser Verbindungstyp ermöglicht den Zugriff auf alle Microsoft-Cloud-Dienste über eine dedizierte private Verbindung an Microsoft, das durchlaufen nicht im Internet, wie unten dargestellt.

![ER] (./media/guidance-connecting-your-on-premises-network-to-azure/er.png "ExpressRoute Verbindung")

Die Verbindung erfordert die Verwendung von der ExpressRoute-Dienst und eine Verbindung mit einem Connectivity-Anbieter. Die folgende Tabelle enthält die vor- und Nachteile dieses Typs Verbindung.

| **Vorteile**| **Aspekte**|
|---------|---------|
|Datenverkehr kann nicht bei der Übertragung über das Internet öffentlichen abgefangen werden, da eine dedizierte Verbindung über einen Dienstanbieter verwendet wird.|Erfordert lokalen Router Management.|
|Bandbreite bis zu 10 Gb/s pro ExpressRoute Verbindung und Datendurchsatz von bis zu 2 Gb/s, um jedes Gateway.|Ist eine dedizierte Verbindung mit einem Anbieter Connectivity erforderlich.|
|Vorhersehbar Wartezeit da es sich um eine dedizierte Verbindung zu Microsoft handelt, die nicht im Internet durchlaufen.|Erfordern minimalen laufenden Verwaltung ein oder mehrere Azure VPN-Gateways (wenn die Verbindung zu VNets Herstellen einer Verbindung).|
|Ist keine verschlüsselten Kommunikation erforderlich, obwohl Sie gegebenenfalls den Datenverkehr verschlüsseln können.| Wenn Sie Cloud-Dienste, die Verbindungen wieder auf lokale Geräte initiieren verwenden, benötigen Sie möglicherweise eine Firewall zwischen dem lokalen Netzwerk und Azure.|
|Alle Microsoft-Cloud-Dienste, mit wenigen Ausnahmen * können direkt verbinden.|Netzwerk-Adresse (Netzwerkadressübersetzung) erfordert, lokale IP-Adressen eingeben der Microsoft Data Center für Dienste, die mit VNet.* * verbunden werden kann|
|Erzwingen, ausgehenden Datenverkehr initiiert von Cloud-virtuellen Computern über das lokale Netzwerk für Prüfung und Protokollierung BGP verwenden können.|

- * Anzeigen einer [Liste der Dienste](../expressroute/expressroute-faqs.md#supported-services) , die mit ExpressRoute verwendet werden kann. Ihr Abonnement Azure muss genehmigt werden, um die Verbindung zu Office 365.  Finden Sie im Artikel [Azure ExpressRoute für Office 365](https://support.office.com/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd?ui=en-US&rs=en-US&ad=US&fromAR=1) Details aus.
- ** Weitere Informationen zum ExpressRoute [NAT](../expressroute/expressroute-nat.md) Anforderungen.

Weitere Informationen zu [ExpressRoute](../expressroute/expressroute-introduction.md)seine zugeordneten [Preise](https://azure.microsoft.com/pricing/details/expressroute)und [Connectivity-Anbieter](../expressroute/expressroute-locations.md#connectivity-provider-locations).

## <a name="additional-considerations"></a>Weitere Aspekte

- Einige der oben genannten Optionen müssen verschiedene Höchstgrenzen bei Ihnen für VNet Verbindungen, Gateway-Verbindungen und anderen Kriterien unterstützten. Es wird empfohlen, dass Sie die Azure [Netzwerke beschränkt überprüfen](../azure-subscription-service-limits.md#networking-limits) um verstehen, wenn Sie jede hiervon die Typen Connectivity auswirken, die Sie verwenden möchten. 
- Wenn Sie die gleichen VNet als ExpressRoute Gateway einen Gateway eine Standort-zu-Standort VPN-Verbindung herstellen möchten, sollten Sie sich mit Einschränkungen wichtige zuerst vertraut machen. Finden Sie im Artikel [ExpressRoute konfigurieren und gleichzeitig vorhandener Verbindungen zwischen Standorten](../expressroute/expressroute-howto-coexist-resource-manager.md#limits-and-limitations) Weitere Details aus.

## <a name="next-steps"></a>Nächste Schritte

Die folgenden Ressourcen erläutert, wie die Verbindungstypen in diesem Artikel behandelt implementieren.

-   [Implementieren einer Punkt-zu-Standort-Verbindung](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)
-   [Eine Verbindung zwischen Standorten implementieren](guidance-hybrid-network-vpn.md)
-   [Implementieren eine dedizierte private Verbindung](guidance-hybrid-network-expressroute.md)
-   [Implementieren eine dedizierte private Verbindung mit einer Verbindung zwischen Standorten für hohe Verfügbarkeit](guidance-hybrid-network-expressroute-vpn-failover.md)

<properties 
   pageTitle="VPN-Gateway Planung und Design | Microsoft Azure"
   description="Erfahren Sie mehr über VPN-Gateway Planung und Design für Cross lokale, Hybrid und VNet-VNet-Verbindungen"
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management,azure-resource-manager"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="cherylmc"/>

# <a name="planning-and-design-for-vpn-gateway"></a>Planung und Entwurf für VPN-Gateway

Planen und Entwerfen Ihrer Cross lokale und VNet-VNet-Konfigurationen kann einfache oder komplizierte, je nach Ihren Anforderungen Netzwerken. In diesem Artikel führt Sie durch einfache Planung und Design Aspekte.

## <a name="planning"></a>Planung


### <a name="a-namecompareacross-premises-connectivity-options"></a><a name="compare"></a>Cross lokale Verbindungsoptionen

Wenn Sie Ihren lokalen Websites sichere Verbindung zu einem virtuellen Netzwerk herstellen möchten, stehen Ihnen drei verschiedene Arten vergeblich: Standort-zu-Standort Punkt-zu-Standort und ExpressRoute. Vergleichen Sie die verschiedenen Cross lokale Verbindungen, die verfügbar sind. Die Option, die Sie auswählen kann verschiedene Aspekte, wie abhängig sind:


- Welche Arten von Durchsatz erfordert Ihre Lösung?
- Möchten Sie über das öffentliche Internet über secure VPN oder über eine private Verbindung kommunizieren?
- Haben Sie eine öffentliche IP-Adresse für die Verwendung verfügbar?
- Planen Sie, ein VPN-Gerät verwenden? In diesem Fall ist es kompatibel?
- Sind Sie mit nur wenige Computern herstellen, oder möchten Sie eine dauerhafte Verbindung für Ihre Website?
- Welche Art von VPN-Gateway für die Lösung erforderlich ist, die Sie erstellen möchten?
- Welches Gateway SKU sollten Sie verwenden?


In der folgenden Tabelle können Sie entscheiden, die beste Konnektivitätsoption für Ihre Lösung helfen.


[AZURE.INCLUDE [vpn-gateway-cross-premises](../../includes/vpn-gateway-cross-premises-include.md)]



### <a name="a-namegwrequireagateway-requirements-by-vpn-type-and-sku"></a><a name="gwrequire"></a>Gateway-Anforderungen von VPN-Typ und SKU

[AZURE.INCLUDE [vpn-gateway-gwsku](../../includes/vpn-gateway-gwsku-include.md)]

Weitere Informationen zu Gateway SKUs finden Sie unter [VPN-Gateway Settings](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

#### <a name="aggregate-throughput-by-sku-and-vpn-type"></a>Aggregierte Durchsatz nach SKU und VPN-Typ

In der folgenden Tabelle zeigt die Typen Gateway und die geschätzte aggregierte Durchsatz. Der geschätzte aggregierte Durchsatz möglicherweise Faktor für den Entwurf.
Preise Gateway SKUs unterschiedlich. Informationen zur Preisgestaltung finden Sie unter [VPN Gateway Preise](https://azure.microsoft.com/pricing/details/vpn-gateway/). Diese Tabelle gilt für die Ressourcenmanager und klassischen Bereitstellungsmodelle.

[AZURE.INCLUDE [vpn-gateway-table-gwtype-aggtput](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)] 

#### <a name="supported-configurations-by-sku-and-vpn-type"></a>Unterstützte Konfigurationen nach SKU und VPN-Typ

[AZURE.INCLUDE [vpn-gateway-table-requirements](../../includes/vpn-gateway-table-requirements-include.md)] 

### <a name="a-namewfaworkflow"></a><a name="wf"></a>Workflow

Die folgende Liste führt den allgemeinen Workflow für die Cloud-Konnektivität:

1.  Entwerfen und Planen der Suchtopologie Konnektivität und Liste die Adresse Leerzeichen für alle Netzwerke, die Sie verbinden möchten.
2.  Erstellen Sie ein Azure-virtuelles Netzwerk ein. 
3.  Erstellen Sie ein VPN-Gateway für das virtuelle Netzwerk an.
4.  Erstellen und Konfigurieren von Verbindungen mit lokalen Netzwerken oder in anderen Netzwerken virtuellen (nach Bedarf).
5.  Erstellen und Konfigurieren einer Punkt-zu-Standort-Verbindungs für Ihre Azure VPN Gateway (nach Bedarf).
 

## <a name="design"></a>Entwurf

### <a name="a-nametopologiesaconnection-topologies"></a><a name="topologies"></a>Verbindung Topologien

Zunächst, suchen Sie im Artikel [Über VPN-Gateway](vpn-gateway-about-vpngateways.md) -Diagramme. Der Artikel enthält grundlegende Diagramme, die Bereitstellungmodelle für jede Suchtopologie (Ressourcenmanager oder Classic), und welche Bereitstellungstools können Sie Ihre Konfiguration bereitstellen.   

### <a name="a-namedesignbasicsadesign-basics"></a><a name="designbasics"></a>Grundlagen des Datenbankentwurfs

Den folgenden Abschnitten werden die Grundlagen der VPN-Gateway. Erwägen Sie auch, [Netzwerke Services Einschränkungen](../articles/azure-subscription-service-limits.md#networking-limits)aus.


#### <a name="a-namesubnetsaabout-subnets"></a><a name="subnets"></a>Informationen zu Subnetzen

Wenn Sie Verbindungen erstellen, müssen Sie Ihrem Subnetbereiche berücksichtigen. Sie können keine überlappenden Subnetz Adressbereiche. Eine überlappende Subnetz ist, wenn eine virtuelle Netzwerk oder lokalen Speicherort desselben Adressbereichs enthält, die der anderen Ort enthält. Dies bedeutet, dass Sie Ihr Netzwerk Ingenieure für Ihre lokale lokale Netzwerke wird ein Bereich für Ihre Verwendung für Ihre IP Azure festzulegen benötigen Speicherplatz/Subnetze adressieren. Sie benötigen Adressbereichs, der nicht im Netzwerk lokale lokal verwendet wird. 

Vermeiden von sich überlappenden Subnetze ist es wichtig, wenn Sie beim Arbeiten mit VNet-VNet-Verbindungen. Wenn Ihre Subnetze überlappen und eine IP-Adresse senden und Ziel VNets jeglichen, fehl VNet-VNet-Verbindungen. Azure kann die Daten nicht an der anderen VNet weiterleiten, da die Zieladresse Teil der sendenden VNet ist. 

VPN-Gateways erfordern spezielles Subnetz ein Gateway Subnetz bezeichnet. Alle Gateway Subnetze müssen den Namen GatewaySubnet ordnungsgemäß funktioniert. Achten Sie darauf, nicht zu Ihrem Subnetz gehören, Gateway benennen Sie einen anderen Namen ein, und Bereitstellen Sie nicht mit dem Gateway Subnetz virtuellen Computern oder Sonstiges. Finden Sie unter [Gateway Subnetze](vpn-gateway-about-vpn-gateway-settings.md#gwsub).

#### <a name="a-namelocalaabout-local-network-gateways"></a><a name="local"></a>Informationen zu lokalen Netzwerkgateways

Das Gateway lokales Netzwerk bezieht sich normalerweise auf Ihrem lokalen Speicherort. Im Bereitstellungsmodell klassischen wird das Gateway lokales Netzwerk als eine lokale Netzwerk-Website bezeichnet. Beim Konfigurieren eines Gateways für lokales Netzwerk Sie geben sie einen Namen, geben die öffentliche IP-Adresse des Geräts lokalen VPN, und geben Sie die Adresspräfixe, die am lokalen Standort befinden. Azure befasst sich mit der Ziel Adresspräfixe für Netzwerkdatenverkehr, berät die Konfiguration, die Sie angegeben haben, für das Gateway lokales Netzwerk und leitet Pakete entsprechend weiter. Sie können diese Adresspräfixe bei Bedarf ändern. Weitere Informationen finden Sie unter [Lokales Netzwerkgateways](vpn-gateway-about-vpn-gateway-settings.md#lng).


#### <a name="a-namegwtypeaabout-gateway-types"></a><a name="gwtype"></a>Zu Gateway-Datentypen

Auswählen des richtigen Gateway für Ihre Suchtopologie ist entscheidend. Wenn Sie den falschen Typ auswählen, funktionieren Ihr Gateway ordnungsgemäß nicht. Der Gateway-Typ gibt an, wie das Gateway selbst eine Verbindung herstellt, und eine erforderliche Konfiguration Einstellung für das Modell zur Bereitstellung von Ressourcen-Manager.

Die Gateway-Typen sind:

- VPN
- ExpressRoute

#### <a name="a-nameconnectiontypeaabout-connection-types"></a><a name="connectiontype"></a>Informationen zu Datentypen Verbindung

Jede Konfiguration erfordert einen bestimmten Verbindungstyp. Die Verbindungstypen sind:

- IPsec
- Vnet2Vnet
- ExpressRoute
- VPNClient


#### <a name="a-namevpntypeaabout-vpn-types"></a><a name="vpntype"></a>Informationen zu VPN-Typen

Jede Konfiguration erfordert einen bestimmten Typ von VPN. Wenn Sie zwei Konfigurationen, z. B. das Erstellen einer Verbindung zwischen Standorten und einer Punkt-zu-Standort-Verbindung mit der gleichen VNet kombinieren, müssen Sie einen Typ VPN verwenden, der sowohl Verbindung Anforderungen erfüllt.

[AZURE.INCLUDE [vpn-gateway-vpntype](../../includes/vpn-gateway-vpntype-include.md)] 

Die folgende Tabelle enthält den VPN-Typ wie jede Verbindungskonfiguration zugeordnet ist. Stellen Sie sicher, dass der Option VPN-Typ für Ihr Gateway die Konfiguration entspricht, die Sie erstellen möchten. 


[AZURE.INCLUDE [vpn-gateway-table-vpntype](../../includes/vpn-gateway-table-vpntype-include.md)] 

### <a name="a-namedevicesavpn-devices-for-site-to-site-connections"></a><a name="devices"></a>VPN-Geräten für Verbindungen zwischen Standorten

Zum Konfigurieren einer Verbindung zwischen Standorten, unabhängig vom Modell zur Bereitstellung, benötigen Sie die folgenden Elemente:

- Ein VPN-Gerät, das mit Azure VPN-Gateways kompatibel ist
- Eine öffentlich zugängliche IPv4-IP-Adresse, die nicht hinter einem NAT ist

Sie müssen Erfahrung im Konfigurieren von Ihrem Gerät VPN haben oder eine andere Person, die das Gerät für Sie konfigurieren können. Weitere Informationen zu VPN-Geräten finden Sie unter [Informationen zu VPN-Geräten](vpn-gateway-about-vpn-devices.md). VPN-Geräte Artikel enthält Informationen zur überprüften Geräte, Anforderungen für Geräte, die nicht überprüft wurden und Links zu Gerät Konfiguration von Dokumenten, falls vorhanden.

### <a name="a-nameforcedtunnelaconsider-forced-tunnel-routing"></a><a name="forcedtunnel"></a>Erwägen Sie die erzwungene Tunnel routing

Für die meisten Konfigurationen können Sie den erzwungenen Tunnel konfigurieren. Erzwungene Tunnel können Sie umleiten oder "Force" alle Internet gerichtete Datenverkehr zurück zu Ihrem lokalen Standort über eine Website-zu-Standort VPN-Tunnel für Prüfung und Überwachung. Dies ist eine Vorbedingung kritische Sicherheit für die meisten Enterprise IT Richtlinien. 

Ohne Erzwungene Tunnel wird Internet gerichtete Datenverkehr von Ihrem virtuellen Computern in Azure immer Durchlaufen von Azure Netzwerkinfrastruktur direkt, mit dem Internet, ohne die Möglichkeit, können Sie prüfen oder den Verkehr überwachen. Unbefugten Zugriff auf das Internet kann potenziell zu Informationsfreigabe oder andere Arten von Sicherheitsverstößen führen.

**Wird Tunneling-Diagramm**

![Erzwungene Tunnel Verbindung] (./media/vpn-gateway-plan-design/forced-tunnel.png "Erzwungene Tunnel")

Eine erzwungene Tunneling-Verbindung kann in beiden Bereitstellungsmodellen und mithilfe von anderen Tools konfiguriert werden. Finden Sie weitere Informationen in der folgenden Tabelle aus. Wir aktualisieren in dieser Tabelle, sobald neue Beiträge, neue Bereitstellungsmodelle und zusätzliche Tools für diese Konfiguration verfügbar sind. Wenn ein Artikel verfügbar ist, verknüpfen wir direkt mit, aus der Tabelle.

[AZURE.INCLUDE [vpn-gateway-table-forcedtunnel](../../includes/vpn-gateway-table-forcedtunnel-include.md)] 



## <a name="next-steps"></a>Nächste Schritte

Siehe folgende Artikel [VPN Gateway häufig gestellte Fragen](vpn-gateway-vpn-faq.md) und [Informationen zu VPN-Gateway](vpn-gateway-about-vpngateways.md) für Weitere Informationen zur Unterstützung bei Ihrem Design.

Weitere Informationen zu bestimmten gatewayeinstellungen finden Sie unter [Informationen zum VPN Gateway Settings](vpn-gateway-about-vpn-gateway-settings.md).





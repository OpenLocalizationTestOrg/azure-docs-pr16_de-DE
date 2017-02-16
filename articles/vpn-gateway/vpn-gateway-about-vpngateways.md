<properties 
   pageTitle="Informationen zu VPN-Gateway | Microsoft Azure"
   description="Informationen Sie zu VPN-Gateway-Verbindungen für virtuelle Netzwerke Azure."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager,azure-service-management"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="cherylmc" />

# <a name="about-vpn-gateway"></a>Informationen zu VPN-Gateway


Ein Gateway virtuelles Netzwerk dient zum Senden von Netzwerkverkehr zwischen den Azure virtuelle Netzwerke und lokalen Speicherorten und auch virtuelle Netzwerke in Azure (VNet VNet). Wenn Sie über ein VPN-Gateway konfigurieren, müssen Sie erstellen und Konfigurieren eines Gateways virtuelles Netzwerk und eine-Gateway-Verbindung.

Beim Erstellen einer Ressource virtuelles Netzwerk Gateway, geben Sie im Bereitstellungsmodell Ressourcenmanager mehrere Einstellungen. Ist eine der erforderlichen Einstellungen '-GatewayType'. Es gibt zwei Arten von virtuellen Netzwerk-Gateway: Vpn und ExpressRoute. 

Wenn Netzwerkverkehr auf eine dedizierte private Verbindung gesendet wird, verwenden Sie den Gateway-Typ 'ExpressRoute'. Dies wird auch als ExpressRoute Gateway bezeichnet. Wenn Netzwerkverkehr über eine Verbindung mit öffentlichen verschlüsselte gesendet wird, verwenden Sie den Gateway-Typ 'Vpn' aus. Dies ist ein VPN-Gateway genannt. Website-zu-Standort verwenden Punkt-zu-Standort und VNet-VNet-Verbindungen alle einen VPN-Gateway.

Jedes virtuelles Netzwerk kann nur eine virtuelle Netzwerk Gateways pro Gateway Typ verfügen. Beispielsweise können Sie haben eine virtuelle Netzwerk-Gateways, das - GatewayType ExpressRoute verwendet, und eine, die - GatewayType Vpn verwendet. Dieser Artikel befasst sich hauptsächlich auf VPN Gateway. Weitere Informationen zu ExpressRoute finden Sie unter der [ExpressRoute – technische Übersicht](../expressroute/expressroute-introduction.md).

## <a name="pricing"></a>Preise

[AZURE.INCLUDE [vpn-gateway-about-pricing-include](../../includes/vpn-gateway-about-pricing-include.md)] 


## <a name="gateway-skus"></a>Gateway SKUs

[AZURE.INCLUDE [vpn-gateway-gwsku-include](../../includes/vpn-gateway-gwsku-include.md)]

Weitere Informationen zu Gateway SKUs finden Sie unter [Gateway SKUs](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

### <a name="estimated-aggregate-throughput-by-sku"></a>Geschätzte aggregierte Durchsatz nach SKU

In der folgenden Tabelle zeigt die Typen Gateway und die geschätzte aggregierte Durchsatz. Diese Tabelle gilt für die Ressourcenmanager und klassischen Bereitstellungsmodelle.

[AZURE.INCLUDE [vpn-gateway-table-gwtype-aggthroughput](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)] 

## <a name="configuring-a-vpn-gateway"></a>Konfigurieren eines VPN-Gateways

Wenn Sie über ein VPN-Gateway konfigurieren, hängen die Anweisungen, die Sie verwenden die Bereitstellungsmodell, das Sie zum Erstellen von virtuellen Netzwerks verwendet. Beispielsweise, wenn Sie Ihre VNet mithilfe des Modells klassischen Bereitstellung erstellt haben, verwenden Sie die Richtlinien und Anweisungen für das Bereitstellungsmodell klassischen erstellen und konfigurieren Sie Ihre VPN-Gateway. Weitere Informationen zu Datenmodellen Bereitstellung finden Sie unter [Grundlegendes zu Ressourcenmanager und klassischen Bereitstellungsmodelle](../resource-manager-deployment-model.md).

Ein Gateway VPN beruht auf mehrere Ressourcen, die mit bestimmten Einstellungen konfiguriert werden. Die meisten Ressourcen können getrennt konfiguriert werden, auch wenn sie in einer bestimmten Reihenfolge in einigen Fällen konfiguriert werden müssen. Sie können beginnen, erstellen und Konfigurieren von Ressourcen mithilfe einer Konfigurationstools, wie etwa die Azure-Portal. Sie können dann später zum Umschalten auf ein weiteres Tool, wie z. B. PowerShell, so konfigurieren Sie zusätzliche Ressourcen oder vorhandene Ressourcen bei Bedarf zu ändern. Sie können keine derzeit jeder Ressourcen- und Ressourcen Einstellung Azure-Portal konfigurieren. Die Anweisungen in den Artikeln für jede Verbindung Suchtopologie angeben, wenn eine bestimmte Konfigurationstools erforderlich ist. Informationen zu einzelnen Ressourcen und Einstellungen für VPN-Gateway finden Sie unter [Informationen zum VPN-Gateway Settings](vpn-gateway-about-vpn-gateway-settings.md).

Die folgenden Abschnitte enthalten Tabellen mit einer Liste:

- Verfügbare Bereitstellungsmodell
- Verfügbare Konfigurationstools
- Links, die Sie direkt zu einem Artikel, falls vorhanden, ausführen

Verwenden Sie die Diagramme und Beschreibungen helfen, wählen Sie die Suchtopologie Verbindung zu Ihren Anforderungen entsprechen. Die Diagramme anzeigen die Hauptfenster geplante Topologien, aber es ist möglich, eine komplexere Konfiguration mithilfe der Diagramme als Richtlinie zu erstellen.

## <a name="site-to-site-and-multi-site"></a>Website-zu-Standort und Multi-Website

### <a name="site-to-site"></a>Website-zu-Standort

Eine Standort-zu-Standort (S2S) VPN-Gateway-Verbindung ist eine Verbindung über IPSec-/IKE (IKEv1 oder IKEv2) VPN-Tunnel. Diese Art von Verbindung erfordert ein VPN-Gerät ansässig lokalen, die eine öffentliche IP-Adresse zugewiesen hat und befindet sich nicht hinter einem NAT S2S-Verbindungen für Cross lokale und Hybrid verwendet werden können Konfigurationen.   

![S2S Verbindung] (./media/vpn-gateway-about-vpngateways/demos2s.png "Website-zu-Standort")


### <a name="multi-site"></a>Multi-Website

Sie können erstellen und konfigurieren eine VPN-Gateway-Verbindung zwischen Ihrem VNet und mehrere lokale Netzwerke. Bei der Arbeit mit mehreren Verbindungen müssen Sie einen Typ RouteBased VPN (dynamic Gateway für klassische VNets) verwenden. Da eine VNet nur über ein VPN-Gateway verfügen kann, geben Sie alle Verbindungen über das Gateway die verfügbare Bandbreite frei. Dies ist häufig eine Verbindung "Multi-Website" bezeichnet.
 

![Verbindung mit mehreren Standorten] (./media/vpn-gateway-about-vpngateways/demomulti.png "Multi-Website")

### <a name="deployment-models-and-methods-for-site-to-site-and-multi-site"></a>Bereitstellungsmodelle und Methoden für die Website-zu-Standort und Multi-Website

[AZURE.INCLUDE [vpn-gateway-table-site-to-site](../../includes/vpn-gateway-table-site-to-site-include.md)] 

## <a name="vnet-to-vnet"></a>VNet-zu-VNet

Herstellen einer Verbindung ein virtuelles Netzwerk zu einem anderen virtuellen Netzwerk ähnelt (VNet VNet) eine VNet in einer lokalen Website verbinden. Beide Typen Connectivity verwenden einen VPN-Gateway, um einen sicheren Tunnel mit IPSec-/IKE bereitzustellen. Sie können auch VNet-VNet-Kommunikation mit Verbindung mit mehreren Standorten Konfigurationen kombinieren. Auf diese Weise können Sie Netzwerken herstellen, die Cross lokale Konnektivität mit zwischen virtuelle Netzwerkkonnektivität kombinieren.

Die VNets beim Herstellen einer Verbindung sind möglich:

- in der gleichen oder in verschiedenen Regionen
- in der gleichen oder einem anderen Abonnements 
- in der gleichen von anderen Bereitstellungsmodellen


![VNet auf VNet-Verbindung] (./media/vpn-gateway-about-vpngateways/demov2v.png "Vnet-zu-vnet")

#### <a name="connections-between-deployment-models"></a>Verbindungen zwischen Bereitstellung Datenmodellen

Azure aktuell hat zwei Bereitstellungsmodelle: klassischen und Ressourcenmanager. Wenn Sie Azure für einige Zeit verwendet haben, haben Sie wahrscheinlich Azure-virtuellen Computern und Instanz Rollen in einem klassischen VNet ausgeführt. Ihre neuere virtuellen Computern und Rolleninstanzen möglicherweise in einer VNet erstellt in Ressourcenmanager ausgeführt werden. Sie können eine Verbindung zwischen der VNets die Ressourcen in eine VNet direkt mit Ressourcen in einer anderen kommunizieren dürfen erstellen.

#### <a name="vnet-peering"></a>VNet peering

Sie können VNet peering Ihre Verbindung erstellen, solange virtuellen Netzwerks bestimmte Anforderungen erfüllt verwenden können. VNet peering wird ein Netzwerkgateway virtuelle nicht verwendet werden. Weitere Informationen finden Sie unter [VNet peering](../virtual-network/virtual-network-peering-overview.md).


### <a name="deployment-models-and-methods-for-vnet-to-vnet"></a>Bereitstellungsmodelle und Methoden für VNet-zu-VNet

[AZURE.INCLUDE [vpn-gateway-table-vnet-to-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)] 


## <a name="point-to-site"></a>Punkt-zu-Standort

Eine Punkt-zu-Standort (P2S) VPN-Gateway-Verbindung können Sie eine sichere Verbindung mit Ihrem Netzwerk virtuelle aus einem einzelnen Clientcomputer zu erstellen. P2S ist eine VPN-Verbindung über SSTP (Secure Sockets Tunnel Protocol). Ein VPN-Gerät oder eine öffentlich zugängliche IP-Adresse entwickelt erforderlich P2S Verbindungen keine. Sie richten die VPN-Verbindung, indem Sie ihn aus dem Clientcomputer zu starten. Diese Lösung ist sinnvoll, wenn Sie zu Ihrer VNet von einem entfernten Standort, wie Start oder einer Konferenz, eine Verbindung herstellen möchten, oder wenn Sie nur wenige Clients haben, die Verbindung zu einem VNet benötigen. P2S-Verbindungen können in Verbindung mit S2S Verbindungen über das gleiche VPN Gateway verwendet werden, vorausgesetzt, dass alle erforderlichen Konfiguration für die beiden Verbindungen kompatibel sind.


![Punkt-zu-Standort-Verbindung] (./media/vpn-gateway-about-vpngateways/demop2s.png "Punkt-zu-Standort")

### <a name="deployment-models-and-methods-for-point-to-site"></a>Bereitstellungsmodelle und Methoden für Punkt-zu-Standort

[AZURE.INCLUDE [vpn-gateway-table-point-to-site](../../includes/vpn-gateway-table-point-to-site-include.md)] 


## <a name="expressroute"></a>ExpressRoute

[AZURE.INCLUDE [expressroute-intro](../../includes/expressroute-intro-include.md)]

Ein Gateway virtuelles Netzwerk ist in einer ExpressRoute-Verbindung mit dem Gateway Typ 'ExpressRoute' statt 'Vpn' konfiguriert. Weitere Informationen zu ExpressRoute finden Sie unter der [ExpressRoute – technische Übersicht](../expressroute/expressroute-introduction.md).


## <a name="site-to-site-and-expressroute-coexisting-connections"></a>Zwischen Standorten und ExpressRoute gleichzeitig vorhandener Verbindungen

ExpressRoute ist eine direkte, dedizierte Verbindung aus Ihrem WAN (nicht über das öffentliche Internet) mit Microsoft Azure einschließlich Services. Website-zu-Standort VPN Datenverkehr Reisen über das öffentliche Internet verschlüsselt. Zum Konfigurieren von Website-zu-Standort VPN und ExpressRoute Verbindungen für das gleiche virtuelle Netzwerk bietet mehrere Vorteile.

Sie können ein Standort-zu-Standort VPN als sichere Failover Pfad für ExpressRoute konfigurieren, oder verwenden Website-zu-Standort VPN, für die Verbindung zu Websites, die nicht Teil Ihres Netzwerks sind, aber, die durch ExpressRoute verbunden sind. Beachten Sie, dass dies zwei virtuelle Netzwerkgateways für das gleiche virtuelle Netzwerk erfordert eine verwendet - GatewayType Vpn, und das andere - GatewayType ExpressRoute.


![Coexist Verbindung] (./media/vpn-gateway-about-vpngateways/demoer.png "Expressroute-site2site")


### <a name="deployment-models-and-methods-for-s2s-and-expressroute"></a>Bereitstellungsmodelle und Methoden für S2S und ExpressRoute

[AZURE.INCLUDE [vpn-gateway-table-coexist](../../includes/vpn-gateway-table-coexist-include.md)] 


## <a name="next-steps"></a>Nächste Schritte

Planen Sie Ihre VPN-Gateway-Konfiguration. Finden Sie unter [VPN Gateway Planung und Design](vpn-gateway-plan-design.md) und [Verbinden von Ihrem lokalen Netzwerk Azure](../guidance/guidance-connecting-your-on-premises-network-to-azure.md).








 

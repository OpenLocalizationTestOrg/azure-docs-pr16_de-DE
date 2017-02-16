<properties 
   pageTitle="Konfigurieren eines erzwungenen Tunnel für Standort-zu-Standort-Verbindungen mit dem Modell zur Bereitstellung von Ressourcenmanager | Microsoft Azure"
   description="So leiten Sie oder 'force' alle Internet gerichtete Datenverkehr an Ihrem lokalen Standort."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/10/2016"
   ms.author="cherylmc" />

# <a name="configure-forced-tunneling-using-the-azure-resource-manager-deployment-model"></a>Konfigurieren eines erzwungenen Tunnel, verwenden das Modell zur Bereitstellung von Azure Ressourcenmanager

> [AZURE.SELECTOR]
- [PowerShell - klassisch](vpn-gateway-about-forced-tunneling.md)
- [PowerShell - Ressourcenmanager](vpn-gateway-forced-tunneling-rm.md)

Erzwungene Tunnel können Sie umleiten oder "Force" alle Internet gerichtete Datenverkehr zurück zu Ihrem lokalen Standort über eine Website-zu-Standort VPN-Tunnel für Prüfung und Überwachung. Dies ist eine Vorbedingung kritische Sicherheit für die meisten Enterprise IT Richtlinien.

Ohne Erzwungene Tunnel wird Internet gerichtete Datenverkehr von Ihrem virtuellen Computern in Azure immer Durchlaufen von Azure Netzwerkinfrastruktur direkt, mit dem Internet, ohne die Möglichkeit, können Sie prüfen oder den Verkehr überwachen. Unbefugten Zugriff auf das Internet kann potenziell zu Informationsfreigabe oder andere Arten von Sicherheitsverstößen führen.

In diesem Artikel Schritte zum Konfigurieren Erzwungene Tunnel für virtuelle Netzwerke mit dem Modell zur Bereitstellung von Ressourcenmanager erstellt.

**Informationen zu Datenmodellen Azure-Bereitstellung**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

**Bereitstellungsmodelle und Tools für die erzwungene Tunnel**

Eine erzwungene Tunneling-Verbindung kann für sowohl die klassischen Bereitstellung auch das Bereitstellungsmodell Ressourcenmanager konfiguriert werden. Finden Sie weitere Informationen in der folgenden Tabelle aus. Wir aktualisieren in dieser Tabelle, sobald neue Beiträge, neue Bereitstellungsmodelle und zusätzliche Tools für diese Konfiguration verfügbar sind. Wenn ein Artikel verfügbar ist, verknüpfen wir direkt mit, aus der Tabelle.

[AZURE.INCLUDE [vpn-gateway-table-forced-tunneling](../../includes/vpn-gateway-table-forcedtunnel-include.md)] 


## <a name="about-forced-tunneling"></a>Info Erzwungene Tunnel


Das folgende Diagramm veranschaulicht, wie die erzwungene Tunneling-funktioniert. 

![Erzwungene Tunnel](./media/vpn-gateway-forced-tunneling-rm/forced-tunnel.png)

Im Beispiel oben geschickt der Front-End Subnetz nicht erzwungen wird. Die Auslastung in der Front-End-Subnetz können weiterhin akzeptieren und Beantworten von Besprechungsanfragen für Kunden aus dem Internet direkt. Die Ebene Mid und Back-End-Subnetze werden erzwungen getunnelten. Alle ausgehenden Verbindungen aus diesen zwei Subnetzen mit dem Internet werden wird oder zurück zu einer lokalen Website über eine der die S2S VPN-Tunnel umgeleitet.

So können Sie einschränken und prüfen Zugriff auf das Internet aus Ihrer virtuellen Computern oder cloud Services in Azure, verschiebe Ihrer mit mehreren Ebenen Dienstarchitektur erforderlich aktivieren. Sie können auch die erzwungene Tunnel auf die gesamte virtuelle Netzwerke befinden sich in der Liste virtuelle Netzwerke keine Internet zugänglichen Auslastung anwenden.

## <a name="requirements-and-considerations"></a>Anforderungen und Aspekte

Erzwungene Tunnel in Azure ist über virtuelle Netzwerk benutzerdefinierte leitet konfiguriert. Umleiten Datenverkehr zu einem lokalen Standort wird als eine Standard-Routing Azure VPN-Gateway ausgedrückt. Weitere Informationen zu benutzerdefinierten routing und virtuelle Netzwerke finden Sie unter [benutzerdefinierte leitet und IP-Weiterleitung](../virtual-network/virtual-networks-udr-overview.md).

- Jedes Subnetz virtuelles Netzwerk verfügt über eine integrierte System-routing-Tabelle. Die System-routing-Tabelle weist die folgenden drei Gruppen von leitet:

    - **Lokale VNet leitet:** Direkt an das Ziel virtuellen Computern im gleichen virtuellen Netzwerk
    
    - **Lokale leitet:** Mit dem Gateway Azure VPN
    
    - **Standard-Routing:** Direkt mit dem Internet. Pakete an den privaten IP-Adressen, die nicht von der vorherigen zwei leitet abgedeckt werden gelöscht.

-  Dieses Verfahren verwendet benutzerdefinierte leitet (UDR) erstellen Sie eine routing-Tabelle zum Hinzufügen eines Standard-Routing, und ordnen Sie die routing-Tabelle, um Ihre VNet Subnetze erzwungenen Tunnel in diesen Subnetzen aktivieren.

- Erzwungene Tunnel muss ein VNet zugeordnet sein, die ein Routing-basierten VPN-Gateway verfügt. Sie müssen eine "Standardwebsite" zwischen der lokalen Cross lokale Websites mit dem virtuellen Netzwerk verbunden festlegen.

- ExpressRoute Erzwungene Tunnel über diese Methode nicht konfiguriert ist, aber durch eine Standard-Routing über die ExpressRoute BGP Peeringliste Sitzungen Werbung stattdessen aktiviert ist. Finden Sie in der [Dokumentation ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/) Weitere Informationen.

## <a name="configuration-overview"></a>Übersicht über die Konfiguration

Das folgende Verfahren können Sie eine Ressourcengruppe und eine VNet erstellen. Sie erhalten dann erstellen einen VPN-Gateway und Erzwungene Tunnel konfigurieren. In diesem Verfahren unter weist das virtuelle Netzwerk "Mehrstufigen-VNet" 3 Subnetze: *Front-End*, *Midtier*und *Back-End-*mit 4 Cross lokale Verbindungen: *DefaultSiteHQ*und 3 *Verzweigungen*.

Mit den Schritten Verfahren der *DefaultSiteHQ* als Standard-Website-Verbindung für Erzwungene Tunnel einrichten und Konfigurieren der Midtier und Back-End-Subnetze verwendet werden, Tunnel.

    
## <a name="before-you-begin"></a>Vorbemerkung

Stellen Sie sicher, dass Sie über Folgendes verfügen, bevor Sie mit die Konfiguration beginnen.

- Ein Azure-Abonnement. Wenn Sie bereits über ein Azure-Abonnement besitzen, können Sie Ihre [MSDN-Vorteile für Abonnenten](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder melden Sie sich für ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/)von aktivieren.

- Sie müssen die neueste Version der Azure Ressourcenmanager PowerShell-Cmdlets (1.0 oder höher) installieren. Weitere Informationen zum Installieren der PowerShell-Cmdlets finden Sie unter [Informationen zum Installieren und konfigurieren Azure PowerShell](../powershell-install-configure.md) .


## <a name="configure-forced-tunneling"></a>Konfigurieren eines erzwungenen Tunnel

1. Melden Sie sich in der PowerShell-Konsole Ihrem Azure-Konto an. Dieses Cmdlet fordert Sie für die Anmeldeinformationen für Ihr Konto Azure. Nach der Anmeldung lädt diese Einstellungen für Ihr Konto, damit sie für Azure PowerShell verfügbar sind.

        Login-AzureRmAccount 

2. Abrufen einer Liste von Azure-Abonnements.

        Get-AzureRmSubscription

2. Geben Sie das Abonnement, das Sie verwenden möchten. 

        Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
        
3. Erstellen Sie eine Ressourcengruppe aus.

        New-AzureRmResourceGroup -Name "ForcedTunneling" -Location "North Europe"

4. Erstellen Sie ein virtuelles Netzwerk und geben Sie Subnetze an. 

        $s1 = New-AzureRmVirtualNetworkSubnetConfig -Name "Frontend" -AddressPrefix "10.1.0.0/24"
        $s2 = New-AzureRmVirtualNetworkSubnetConfig -Name "Midtier" -AddressPrefix "10.1.1.0/24"
        $s3 = New-AzureRmVirtualNetworkSubnetConfig -Name "Backend" -AddressPrefix "10.1.2.0/24"
        $s4 = New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix "10.1.200.0/28"
        $vnet = New-AzureRmVirtualNetwork -Name "MultiTier-VNet" -Location "North Europe" -ResourceGroupName "ForcedTunneling" -AddressPrefix "10.1.0.0/16" -Subnet $s1,$s2,$s3,$s4

5. Erstellen der lokalen Netzwerkgateways.

        $lng1 = New-AzureRmLocalNetworkGateway -Name "DefaultSiteHQ" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.111" -AddressPrefix "192.168.1.0/24"
        $lng2 = New-AzureRmLocalNetworkGateway -Name "Branch1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.112" -AddressPrefix "192.168.2.0/24"
        $lng3 = New-AzureRmLocalNetworkGateway -Name "Branch2" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.113" -AddressPrefix "192.168.3.0/24"
        $lng4 = New-AzureRmLocalNetworkGateway -Name "Branch3" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.114" -AddressPrefix "192.168.4.0/24"
        
6. Erstellen Sie die Routingtabelle und Routing Regel.

        New-AzureRmRouteTable –Name "MyRouteTable" -ResourceGroupName "ForcedTunneling" –Location "North Europe"
        $rt = Get-AzureRmRouteTable –Name "MyRouteTable" -ResourceGroupName "ForcedTunneling" 
        Add-AzureRmRouteConfig -Name "DefaultRoute" -AddressPrefix "0.0.0.0/0" -NextHopType VirtualNetworkGateway -RouteTable $rt
        Set-AzureRmRouteTable -RouteTable $rt


7. Zuordnen der Routingtabelle, um die Midtier und Back-End-Subnetze an.

        $vnet = Get-AzureRmVirtualNetwork -Name "MultiTier-Vnet" -ResourceGroupName "ForcedTunneling"
        Set-AzureRmVirtualNetworkSubnetConfig -Name "MidTier" -VirtualNetwork $vnet -AddressPrefix "10.1.1.0/24" -RouteTable $rt
        Set-AzureRmVirtualNetworkSubnetConfig -Name "Backend" -VirtualNetwork $vnet -AddressPrefix "10.1.2.0/24" -RouteTable $rt
        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

8. Das Gateway mit einer Standardwebsite zu erstellen. Dieser Schritt dauert einige Zeit, manchmal 45 Minuten oder mehr abgeschlossen werden, da Sie sind erstellen und Konfigurieren des Gateways.<br> Die `-GatewayDefaultSite` ist der Cmdlet-Parameter, die die erzwungene routing-Konfiguration zu arbeiten, daher müssen Sie sicherstellen so konfigurieren Sie diese Einstellung ordnungsgemäß ermöglicht. Für diesen Parameter ist in PowerShell 1.0 oder höher verfügbar.

        $pip = New-AzureRmPublicIpAddress -Name "GatewayIP" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -AllocationMethod Dynamic
        $gwsubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
        $ipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "gwIpConfig" -SubnetId $gwsubnet.Id -PublicIpAddressId $pip.Id
        New-AzureRmVirtualNetworkGateway -Name "Gateway1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -IpConfigurations $ipconfig -GatewayType Vpn -VpnType RouteBased -GatewayDefaultSite $lng1 -EnableBgp $false

9. Richten Sie die Standort-zu-Standort VPN-Verbindungen.

        $gateway = Get-AzureRmVirtualNetworkGateway -Name "Gateway1" -ResourceGroupName "ForcedTunneling"
        $lng1 = Get-AzureRmLocalNetworkGateway -Name "DefaultSiteHQ" -ResourceGroupName "ForcedTunneling" 
        $lng2 = Get-AzureRmLocalNetworkGateway -Name "Branch1" -ResourceGroupName "ForcedTunneling" 
        $lng3 = Get-AzureRmLocalNetworkGateway -Name "Branch2" -ResourceGroupName "ForcedTunneling" 
        $lng4 = Get-AzureRmLocalNetworkGateway -Name "Branch3" -ResourceGroupName "ForcedTunneling" 

        New-AzureRmVirtualNetworkGatewayConnection -Name "Connection1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng1 -ConnectionType IPsec -SharedKey "preSharedKey"
        New-AzureRmVirtualNetworkGatewayConnection -Name "Connection2" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng2 -ConnectionType IPsec -SharedKey "preSharedKey"
        New-AzureRmVirtualNetworkGatewayConnection -Name "Connection3" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng3 -ConnectionType IPsec -SharedKey "preSharedKey"
        New-AzureRmVirtualNetworkGatewayConnection -Name "Connection4" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng4 -ConnectionType IPsec -SharedKey "preSharedKey"

        Get-AzureRmVirtualNetworkGatewayConnection -Name "Connection1" -ResourceGroupName "ForcedTunneling"
        




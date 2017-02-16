<properties
   pageTitle="Erstellen eines virtuelles Netzwerks mit einer Standort-zu-Standort VPN-Verbindung mithilfe von Azure Ressourcenmanager und PowerShell | Microsoft Azure"
   description="In diesem Artikel führt Sie durch Erstellen einer VNet mithilfe des Modells der Ressourcenmanager Bereitstellung und die Verbindung mit Ihrem lokalen lokalen Netzwerk über eine S2S VPN-Gateway-Verbindung."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/14/2016"
   ms.author="cherylmc"/>

# <a name="create-a-vnet-with-a-site-to-site-connection-using-powershell"></a>Erstellen einer VNet mit einer Website-zu-Standort-Verbindung mithilfe der PowerShell

> [AZURE.SELECTOR]
- [Ressourcenmanager - Azure-Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Ressourcenmanager - PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
- [Klassische - klassischen-Portal](vpn-gateway-site-to-site-create.md)

In diesem Artikel führt Sie durch die Erstellung eines virtuellen Netzwerks und eine Website-zu-Standort VPN Gateway-Verbindung mit Ihrem lokalen Netzwerk mithilfe des Modells der Ressourcenmanager Azure-Bereitstellung. Website-zu-Standort-Verbindungen für Cross lokale und Hybrid verwendet werden können Konfigurationen.

![Website-zu-Standort-Diagramm] (./media/vpn-gateway-create-site-to-site-rm-powershell/s2srmps.png "Website-zu-Standort") 


### <a name="deployment-models-and-methods-for-site-to-site-connections"></a>Bereitstellungsmodelle und Methoden für die Verbindungen zwischen Standorten

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

In der folgenden Tabelle zeigt die aktuell verfügbare Bereitstellung-Modelle und Methoden für die Website-zu-Standort-Konfigurationen. Wenn ein Artikel mit Konfigurationsschritte verfügbar ist, verknüpfen wir aus dieser Tabelle direkt an. 

[AZURE.INCLUDE [site-to-site table](../../includes/vpn-gateway-table-site-to-site-include.md)]

#### <a name="additional-configurations"></a>Zusätzliche Konfigurationen

Wenn Sie VNets miteinander zu verbinden möchten, aber keine Verbindung mit einem lokalen Speicherort erstellen, finden Sie unter [Konfigurieren einer VNet-VNet - Verbindung](vpn-gateway-vnet-vnet-rm-ps.md). Wenn Sie eine Verbindung zwischen Standorten eine VNet hinzufügen, die bereits eine Verbindung möchten, finden Sie unter [Hinzufügen einer S2S-Verbindung zu einem VNet mit einer vorhandenen VPN-Gateway-Verbindung](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md).

## <a name="before-you-begin"></a>Vorbemerkung

Stellen Sie sicher, dass Sie über Folgendes verfügen, bevor Sie beginnen Konfiguration.

- Ein kompatibles VPN-Gerät und eine Person, die sie konfigurieren kann. Finden Sie unter [VPN-Geräte](vpn-gateway-about-vpn-devices.md). Wenn Sie nicht mit Ihrem Gerät VPN konfigurieren vertraut sind, oder klicken Sie mit der IP-Adresse, die Bereiche in ansässig vertraut sind, Ihre lokal Netzwerkkonfiguration, müssen Sie mit einer anderen Person zu koordinieren, die für Sie diese Details abgerufen werden können.

- Eine extern ausgerichteten öffentlichen IP-Adresse für Ihr VPN-Gerät. Diese IP-Adresse kann sich nicht hinter einem NAT befinden
    
- Ein Azure-Abonnement. Wenn Sie bereits über ein Azure-Abonnement besitzen, können Sie Ihre [MSDN-Vorteile für Abonnenten](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder melden Sie sich für ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/)von aktivieren.
    
- Die neueste Version der Azure Ressourcenmanager PowerShell-Cmdlets. Weitere Informationen zum Installieren der PowerShell-Cmdlets finden Sie unter [Informationen zum Installieren und konfigurieren Azure PowerShell](../powershell-install-configure.md) .


## <a name="a-namelogina1-connect-to-your-subscription"></a><a name="Login"></a>1. Herstellen einer Verbindung mit Ihrem Abonnement 

Stellen Sie sicher, dass Sie in PowerShell-Modus, um die Ressourcenmanager Cmdlets verwenden, wechseln. Weitere Informationen finden Sie unter [Verwenden von Windows PowerShell mit Ressourcenmanager](../powershell-azure-resource-manager.md).

Öffnen Sie in der PowerShell-Konsole und eine Verbindung mit Ihrem Konto herstellen. Verwenden Sie im folgende Beispiel, damit Sie eine Verbindung herstellen können:

    Login-AzureRmAccount

Überprüfen Sie die Abonnements für das Konto ein.

    Get-AzureRmSubscription 

Geben Sie das Abonnement, das Sie verwenden möchten.

    Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

## <a name="a-namevneta2-create-a-virtual-network-and-a-gateway-subnet"></a><a name="VNet"></a>2. erstellen Sie ein virtuelles Netzwerk und ein Gateway Subnetz

Die Beispiele verwenden ein Gateway-Subnetz des /28. Es ist, zwar möglich, ein Gateway Subnetz /29 möglichst klein erstellen wird empfohlen, dass Sie eine größere Subnetz, die mehrere Adressen enthält erstellen, indem Sie mindestens /28 oder /27 auswählen. Damit wird genügend Adressen mögliche zusätzliche Konfigurationen gerecht werden kann, die Sie möglicherweise in der Zukunft möchten.

Wenn Sie bereits ein virtuelles Netzwerk mit einem Subnetz der Gateways, die/29 oder größere, Sie können springen [Schlüsselaufgaben lokales Netzwerk](#localnet)hinzufügen.


[AZURE.INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]  

### <a name="to-create-a-virtual-network-and-a-gateway-subnet"></a>So erstellen ein virtuelles Netzwerk und ein Gateway Subnetz

Verwenden Sie im folgende Beispiel, um ein virtuelles Netzwerk und ein Gateway Subnetz erstellen. Ersetzen Sie die Werte für Ihren eigenen. 

Erstellen Sie zuerst eine Ressourcengruppe aus:
    
    New-AzureRmResourceGroup -Name testrg -Location 'West US'

Erstellen Sie anschließend virtuelle Netzwerks ein. Stellen Sie sicher, dass die Adresse Leerzeichen, die Sie angeben sich die Adressräume überschneiden nicht, die Sie in Ihrem lokalen Netzwerk besitzen.

Im folgende Beispiel wird ein virtuelles Netzwerk mit dem Namen *Testvnet* und zwei Subnetzen, eine sogenannte *GatewaySubnet* erstellt und weitere mit der Bezeichnung *Subnet1*. Es ist wichtig, ein Subnetz mit dem Namen speziell *GatewaySubnet*erstellen. Wenn Sie einen anderen Namen, tritt die Verbindungskonfiguration. 

Setzen Sie die Variablen.

    $subnet1 = New-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.0.0/28
    $subnet2 = New-AzureRmVirtualNetworkSubnetConfig -Name 'Subnet1' -AddressPrefix '10.0.1.0/28'

Erstellen der VNet.

    New-AzureRmVirtualNetwork -Name testvnet -ResourceGroupName testrg `
    -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $subnet1, $subnet2

### <a name="a-namegatewaysubnetato-add-a-gateway-subnet-to-a-virtual-network-you-have-already-created"></a><a name="gatewaysubnet"></a>Hinzufügen eines Gateways Subnetz zu einem virtuellen Netzwerk, die bereits erstellt haben

Dieser Schritt ist erforderlich, nur, wenn Sie müssen ein Gateway Subnetz eine VNet hinzufügen, die Sie zuvor erstellt haben.

Sie können Ihr Gateway Subnetz erstellen, indem Sie mit dem folgenden Beispiel. Achten Sie darauf, um das Gateway Subnetz 'GatewaySubnet' nennen. Wenn Sie einen anderen Namen, erstellen Sie ein Subnetz, aber Azure wird nicht als Subnetz Gateway behandelt.

Setzen Sie die Variablen.

    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName testrg -Name testvnet

Das Gateway Subnetz zu erstellen.

    Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/28 -VirtualNetwork $vnet

Festlegen der Konfiguration. 

    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

## <a name="3-a-namelocalnetaadd-your-local-network-gateway"></a>3. <a name="localnet"> </a>Ihr lokales Netzwerkgateway hinzufügen

In einem virtuellen Netzwerk bezieht sich normalerweise auf Ihrem lokalen Speicherort des Gateways lokales Netzwerk. Sie benennen Sie der Website mit dem Azure darauf verweisen kann, und geben Sie auch das Adresse Leerzeichen Präfix für das Gateway lokales Netzwerk. 

Azure verwendet das Präfix der IP-Adresse, die, das Sie angeben, welche Datenverkehr an Ihren lokalen Standort senden. Dies bedeutet, dass Sie jedes Adresspräfix angeben, die mit Ihrem lokalen Netzwerkgateway verknüpft werden soll. Sie können diese Präfixe einfach aktualisieren, wenn Ihrem lokalen Netzwerk ändert. 

Wenn Sie die Beispiele PowerShell verwenden zu können, beachten Sie Folgendes:
    
- Die *GatewayIPAddress* ist die IP-Adresse von Ihrem lokalen VPN-Gerät. Ihr Gerät VPN kann sich nicht hinter einem NAT befinden 
- Die *AddressPrefix* ist Ihr Speicherplatz für lokale Adresse.

So fügen Sie ein lokales Netzwerkgateway mit einer einzelnen Adresspräfix hinzu:

    New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
    -Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.5.51.0/24'

So fügen Sie ein lokales Netzwerkgateway mit mehreren Adresspräfixe hinzu:

    New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
    -Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix @('10.0.0.0/24','20.0.0.0/24')

### <a name="to-modify-ip-address-prefixes-for-your-local-network-gateway"></a>So ändern Sie die IP-Adresspräfixe für Ihr lokales Netzwerk-gateway

Manchmal ändert sich Ihr lokales Netzwerk Gateway Präfixe. Die Schritte, die Sie zum Ändern der IP-Adresspräfixe ausführen, hängt davon ab, ob Sie eine VPN-Gateway-Verbindung erstellt haben. Finden Sie im Abschnitt [Ändern IP-Adresspräfixe für ein Gateway lokales Netzwerk](#modify) in diesem Artikel aus.


## <a name="a-namepublicipa4-request-a-public-ip-address-for-the-vpn-gateway"></a><a name="PublicIP"></a>4 Anforderung einer öffentlichen IP-Adresse für das Gateway VPN

Als Nächstes Anfordern einer öffentlichen IP-Adresse für das Azure VNet VPN-Gateway bereitzustellenden aus. Dies ist kein die IP-Adresse, die mit Ihrem Gerät VPN zugeordnet ist. Es wird lieber Azure VPN-Gateway selbst zugewiesen. Sie können nicht die IP-Adresse angeben, die Sie verwenden möchten. Es ist den Schlüsselaufgaben dynamisch zugewiesen. Sie verwenden diese IP-Adresse bei Ihrem lokalen VPN-Gerät der Konfiguration für die Verbindung mit dem Gateway.

Das Gateway Azure VPN für den Ressourcenmanager Modell zur Bereitstellung von aktuell unterstützt nur öffentliche IP-Adressen mithilfe der Methode der dynamischen Zuteilung. Dies bedeutet jedoch nicht, dass die IP-Adresse geändert wird. Nur dann die Änderungen Azure VPN Gateway IP-Adresse ist, wenn das Gateway gelöscht und erneut erstellt wird. Die öffentliche IP-Adresse des Gateways ändert sich nicht über das Ändern der Größe, zurücksetzen oder anderen internen Wartung/Upgrades Ihrer Azure VPN-Gateways.

Verwenden Sie die folgende PowerShell-Beispiel:

    $gwpip= New-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName testrg -Location 'West US' -AllocationMethod Dynamic

## <a name="a-namegatewayipconfiga5-create-the-gateway-ip-addressing-configuration"></a><a name="GatewayIPConfig"></a>5. erstellen Sie die Gateway IP-Adressen-Konfiguration

Die Gateway-Konfiguration definiert das Subnetz und die öffentliche IP-Adresse zu verwenden. Verwenden Sie im folgende Beispiel, um Ihre Gateway-Konfiguration zu erstellen.

    $vnet = Get-AzureRmVirtualNetwork -Name testvnet -ResourceGroupName testrg
    $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
    $gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name gwipconfig1 -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id 

## <a name="a-namecreategatewaya6-create-the-virtual-network-gateway"></a><a name="CreateGateway"></a>6. virtuelle Netzwerk-Gateway erstellen

In diesem Schritt erstellen Sie das Netzwerkgateway virtuelle aus. Erstellen eines Gateways, kann einen langen Zeit in Anspruch nehmen. Häufig 45 Minuten oder mehr. 

Verwenden Sie die folgenden Werte ein:

- Die *-GatewayType* für eine Website-zu-Standort-Konfiguration ist *Vpn*. Der Gateway-Typ ist immer bestimmte an der Konfiguration, die Sie implementieren. Weitere Gateway-Konfigurationen erfordern möglicherweise - GatewayType ExpressRoute. 

- Die *-VpnType* kann *RouteBased* (als dynamische Gateway in einigen Dokumentation bezeichnet), oder *Richtlinien* (als statische Gateway in einigen Dokumentation bezeichnet). Weitere Informationen zu VPN-Gateway Typen finden Sie unter [Informationen zu VPN-Gateways](vpn-gateway-about-vpngateways.md#vpntype).
- Die *-GatewaySku* kann *einfache*, *Standard*oder *HighPerformance*sein.   

        New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
        -Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn `
        -VpnType RouteBased -GatewaySku Standard

## <a name="a-nameconfigurevpndevicea7-configure-your-vpn-device"></a><a name="ConfigureVPNDevice"></a>7 Konfigurieren von Ihrem Gerät VPN

An diesem Punkt, benötigen Sie die öffentliche IP-Adresse des Gateways virtuelles Netzwerk für das Konfigurieren von Ihrem lokalen VPN-Gerät. Arbeiten Sie mit Ihrem Gerätehersteller detaillierte Informationen zur Konfiguration. Sie können auf die [Option VPN-Geräten](vpn-gateway-about-vpn-devices.md) für Weitere Informationen verweisen.

Verwenden Sie zum Aufrufen die öffentliche IP-Adresse Ihres Gateways virtuelles Netzwerk finden Sie im folgende Beispiel:

    Get-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName testrg

## <a name="a-namecreateconnectiona8-create-the-vpn-connection"></a><a name="CreateConnection"></a>8. die VPN-Verbindung zu erstellen

Erstellen Sie anschließend die Standort-zu-Standort VPN Verbindung zwischen Schlüsselaufgaben virtuelles Netzwerk und Ihre VPN-Gerät. Achten Sie darauf, dass Sie die Werte durch ein eigenes ersetzen. Der gemeinsame Schlüssel muss der Wert entsprechen, die, den Sie für Ihre VPN-Gerätekonfiguration verwendet. Beachten Sie, dass die `-ConnectionType` für Standort-zu-Standort *IPsec*ist. 

Setzen Sie die Variablen.

    $gateway1 = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
    $local = Get-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg

Die Verbindung zu erstellen.

    New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg `
    -Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
    -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

Nach kurzer Zeit, die Verbindung wird hergestellt werden. 

## <a name="a-nametoverifyato-verify-a-vpn-connection"></a><a name="toverify"></a>Überprüfen Sie die VPN-Verbindung

Es gibt verschiedene Möglichkeiten zur Überprüfung Ihrer VPN-Verbindungs aus.

[AZURE.INCLUDE [vpn-gateway-verify-connection-rm](../../includes/vpn-gateway-verify-connection-rm-include.md)]

## <a name="a-namemodifyato-modify-ip-address-prefixes-for-a-local-network-gateway"></a><a name="modify"></a>So ändern Sie die IP-Adresspräfixe für ein Gateway lokales Netzwerk

Wenn Sie die Präfixe für Ihr lokales Netzwerkgateway ändern müssen, verwenden Sie die folgenden Anweisungen. Zwei Sätze von Anweisungen stehen zur Verfügung. Die Anweisungen, die Sie auswählen, hängt davon ab, ob Sie die Verbindung Gateway bereits erstellt haben. 

[AZURE.INCLUDE [vpn-gateway-modify-ip-prefix-rm](../../includes/vpn-gateway-modify-ip-prefix-rm-include.md)]

## <a name="a-namemodifygwipaddressato-modify-the-gateway-ip-address-for-a-local-network-gateway"></a><a name="modifygwipaddress"></a>So ändern Sie die Gateway IP-Adresse für ein Gateway lokales Netzwerk

[AZURE.INCLUDE [vpn-gateway-modify-lng-gateway-ip-rm](../../includes/vpn-gateway-modify-lng-gateway-ip-rm-include.md)]

## <a name="next-steps"></a>Nächste Schritte

- Sie können Ihre virtuelle Netzwerke virtuellen Computern hinzufügen. Schritte finden Sie unter [Erstellen eines virtuellen Computers](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .

- Informationen zu BGP finden Sie unter der [BGP Übersicht](vpn-gateway-bgp-overview.md) und [BGP konfigurieren](vpn-gateway-bgp-resource-manager-ps.md).


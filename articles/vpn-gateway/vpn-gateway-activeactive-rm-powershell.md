<properties
   pageTitle="So konfigurieren Sie die aktive S2S VPN-Verbindungen mit Azure VPN-Gateways mit Azure Ressourcenmanager und PowerShell | Microsoft Azure"
   description="In diesem Artikel führt Sie durch die aktive Verbindungen mit Azure VPN-Gateways mit Azure Ressourcenmanager und PowerShell konfigurieren."
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/26/2016"
   ms.author="yushwang"/>

# <a name="configure-active-active-s2s-vpn-connections-with-azure-vpn-gateways-using-azure-resource-manager-and-powershell"></a>Konfigurieren von aktive S2S VPN-Verbindungen mit Azure VPN-Gateways mit Azure Ressourcenmanager und PowerShell

In diesem Artikel führt Sie durch die Schritte zum Erstellen von aktive Cross lokale und VNet-VNet-Verbindungen mit dem Modell zur Bereitstellung von Ressourcenmanager und PowerShell.


**Informationen zu Datenmodellen Azure-Bereitstellung**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="about-highly-available-cross-premises-connections"></a>Informationen zu hoch verfügbaren Cross lokale Verbindungen

Um hohe Verfügbarkeit für Cross lokale und VNet-VNet-Konnektivität zu erreichen, sollten Sie mehrere VPN-Gateways bereitstellen und mehrere parallele Verbindungen zwischen Ihren Netzwerken und Azure herstellen. Finden Sie in [hochgradig verfügbar Cross lokale und VNet-VNet - Konnektivität](./vpn-gateway-highlyavailable.md) einen Überblick über die Optionen für die Netzwerkkonnektivität und Suchtopologie.

Dieser Artikel enthält die Anweisungen, um eine aktive Cross lokale VPN-Verbindung, und klicken Sie auf aktive Verbindung zwischen zwei virtuelle Netzwerke einzurichten:

- [Teil 1: Erstellen und konfigurieren Sie Ihre Azure VPN Gateway in aktive Modus](#aagateway)

- [Teil 2: aktive Cross lokale Verbindungen herstellen](#aacrossprem)

- [Teil 3: aktive VNet-VNet-Verbindungen herstellen](#aav2v)

- [Teil 4 - Update vorhandenen Gateway zwischen aktive und aktiv-standby](#aaupdate)

Sie können diese zusammen, um eine komplexere, hoch verfügbare Netzwerk Suchtopologie zu erstellen, die Ihren Anforderungen entspricht kombinieren.

>[AZURE.IMPORTANT] Bitte beachten Sie, dass der aktive Modus nur in HighPerformance SKU funktioniert


## <a name="a-name-aagatewayapart-1---create-and-configure-active-active-vpn-gateways"></a><a name ="aagateway"></a>Teil 1: Erstellen und Konfigurieren von aktive VPN-Gateways

Die folgenden Schritte werden Ihr Azure VPN Gateway aktive Modi konfigurieren. Die wichtigsten Unterschiede zwischen den Gateways aktive und aktiv-Standby:

- Sie müssen zwei Gateway-IP-Konfigurationen mit zwei öffentlichen IP-Adressen zu erstellen.
- Sie müssen die EnableActiveActiveFeature Kennzeichnung festlegen.
- Das Gateway SKU muss HighPerformance sein.

Die anderen Eigenschaften sind aktiv-aktiv Gateways entspricht. 

### <a name="before-you-begin"></a>Vorbemerkung

- Stellen Sie sicher, dass Sie ein Azure-Abonnement verfügen. Wenn Sie bereits über ein Azure-Abonnement besitzen, können Sie Ihre [MSDN-Vorteile für Abonnenten](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder melden Sie sich für ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/)von aktivieren.
    
- Sie müssen die Azure Ressourcenmanager PowerShell-Cmdlets installieren. Weitere Informationen zum Installieren der PowerShell-Cmdlets finden Sie unter [Informationen zum Installieren und konfigurieren Azure PowerShell](../powershell-install-configure.md) .

### <a name="step-1---create-and-configure-vnet1"></a>Schritt 1: Erstellen und Konfigurieren von VNet1

#### <a name="1-declare-your-variables"></a>1. Deklarieren der Variablen

In dieser Übung zunächst wir unsere Variablen zu deklarieren. Im folgenden Beispiel wird die mit den Werten für diese Übung Variablen deklariert. Achten Sie darauf, dass Sie die Werte durch ein eigenes ersetzen, beim für Herstellung konfigurieren. Sie können diese Variablen verwenden, wenn Sie die Schritte zum mit dieser Art von Konfiguration vertraut ausgeführt werden. Ändern der Variablen und klicken Sie dann auf Kopieren und Einfügen in der PowerShell-Konsole.

    $Sub1          = "Ross"
    $RG1           = "TestAARG1"
    $Location1     = "West US"
    $VNetName1     = "TestVNet1"
    $FESubName1    = "FrontEnd"
    $BESubName1    = "Backend"
    $GWSubName1    = "GatewaySubnet"
    $VNetPrefix11  = "10.11.0.0/16"
    $VNetPrefix12  = "10.12.0.0/16"
    $FESubPrefix1  = "10.11.0.0/24"
    $BESubPrefix1  = "10.12.0.0/24"
    $GWSubPrefix1  = "10.12.255.0/27"
    $VNet1ASN      = 65010
    $DNS1          = "8.8.8.8"
    $GWName1       = "VNet1GW"
    $GW1IPName1    = "VNet1GWIP1"
    $GW1IPName2    = "VNet1GWIP2"
    $GW1IPconf1    = "gw1ipconf1"
    $GW1IPconf2    = "gw1ipconf2"
    $Connection12  = "VNet1toVNet2"
    $Connection151 = "VNet1toSite5_1"
    $Connection152 = "VNet1toSite5_2"

#### <a name="2-connect-to-your-subscription-and-create-a-new-resource-group"></a>2. Herstellen einer Verbindung mit Ihrem Abonnement und Erstellen einer neuen Ressourcengruppe

Stellen Sie sicher, dass Sie in PowerShell-Modus, um die Ressourcenmanager Cmdlets verwenden, wechseln. Weitere Informationen finden Sie unter [Verwenden von Windows PowerShell mit Ressourcenmanager](../powershell-azure-resource-manager.md).

Öffnen Sie in der PowerShell-Konsole und eine Verbindung mit Ihrem Konto herstellen. Verwenden Sie im folgende Beispiel, damit Sie eine Verbindung herstellen können:

    Login-AzureRmAccount
    Select-AzureRmSubscription -SubscriptionName $Sub1
    New-AzureRmResourceGroup -Name $RG1 -Location $Location1

#### <a name="3-create-testvnet1"></a>3. TestVNet1 erstellen

Im folgenden Beispiel wird ein virtuelles Netzwerk mit dem Namen TestVNet1 und drei Subnetze, eine sogenannte GatewaySubnet, eine sogenannte Front-End und eine sogenannte Back-End-erstellt. Werte ersetzen, es ist wichtig, dass Sie Ihrem Subnetz gehören, Gateway immer benennen speziell GatewaySubnet. Wenn Sie einen anderen Namen, tritt der Erstellung des Gateways. 

    $fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
    $besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
    $gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

    New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1

### <a name="step-2---create-the-vpn-gateway-for-testvnet1-with-active-active-mode"></a>Schritt 2 – Erstellen von VPN-Gateway für TestVNet1 mit aktive Modus

#### <a name="1-create-the-public-ip-addresses-and-gateway-ip-configurations"></a>1. erstellen Sie die öffentliche IP-Adressen und dem Gateway IP-Konfigurationen

Fordern Sie zwei öffentliche IP-Adressen für das Gateway bereitzustellenden, den Sie für Ihre VNet erstellen. Sie können auch das Subnetz und die IP-Konfigurationen erforderlich definieren. 

    $gw1pip1    = New-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
    $gw1pip2    = New-AzureRmPublicIpAddress -Name $GW1IPName2 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic

    $vnet1      = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
    $subnet1    = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
    $gw1ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf1 -Subnet $subnet1 -PublicIpAddress $gw1pip1
    $gw1ipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf2 -Subnet $subnet1 -PublicIpAddress $gw1pip2

#### <a name="2-create-the-vpn-gateway-with-active-active-configuration"></a>2. Erstellen des Gateways VPN mit aktive Konfiguration

Erstellen von virtuellen Netzwerkgateways für TestVNet1. Beachten Sie, dass zwei GatewayIpConfig Einträge enthalten sind, und die EnableActiveActiveFeature Kennzeichnung festgelegt ist. Aktive Modus erfordert ein Gateway Routing-basierten VPN von HighPerformance SKU. Erstellen eines Gateways kann eine Weile dauern (30 Minuten oder mehr durchführen).

    New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gw1ipconf1,$gw1ipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance -Asn $VNet1ASN -EnableActiveActiveFeature -Debug

#### <a name="3-obtain-the-gateway-public-ip-addresses-and-the-bgp-peer-ip-address"></a>3 erhalten Sie 3 die Gateway öffentlichen IP-Adressen und die BGP Peer-IP-Adresse

Nachdem das Gateway erstellt wurde, müssen Sie die BGP Peer-IP-Adresse auf dem Azure VPN Gateway zu erhalten. Diese Adresse ist so konfigurieren Sie die Azure VPN-Gateway als BGP Peer für Ihren lokalen VPN-Geräten erforderlich.

    $gw1pip1 = Get-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1
    $gw1pip2 = Get-AzureRmPublicIpAddress -Name $GW1IPName2 -ResourceGroupName $RG1
    $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1

Verwenden Sie die folgenden Cmdlets zum Anzeigen von zwei öffentlichen IP-Adressen für das VPN-Gateway und der zugehörigen BGP Peer-IP-Adressen für jede Gatewayinstanz zugewiesen an:

    PS D:\> $gw1pip1.IpAddress
    40.112.190.5

    PS D:\> $gw1pip2.IpAddress
    138.91.156.129

    PS D:\> $vnet1gw.BgpSettingsText
    {
      "Asn": 65010,
      "BgpPeeringAddress": "10.12.255.4,10.12.255.5",
      "PeerWeight": 0
    }

Die Reihenfolge der öffentlichen IP-Adressen für das Gateway Instanzen und die entsprechenden BGP Peering Adressen sind gleich. In diesem Beispiel das Gateway virtueller Computer mit öffentliche IP-Adresse des 40.112.190.5 wird 10.12.255.4 als seine BGP Peering Adresse verwenden, und das Gateway mit 138.91.156.129 10.12.255.5 verwendet wird. Diese Informationen werden benötigt, wenn Sie Ihren auf lokale VPN-Geräten Herstellen einer Verbindung mit dem Gateway aktive einrichten. Das Gateway ist im Diagramm mit Adressen aller unten dargestellt:

![aktive gateway](./media/vpn-gateway-activeactive-rm-powershell/active-active-gw.png)

Nachdem das Gateway erstellt wurde, können Sie dieses Gateway, aktive Cross lokale oder VNet-VNet-Verbindung herstellen. In den folgenden Abschnitten werden die Schritte zum Abschließen der Übung durchzuführen.


## <a name="a-name-aacrosspremapart-2---establish-an-active-active-cross-premises-connection"></a><a name ="aacrossprem"></a>Teil 2: Herstellen einer Verbindung aktive Cross lokale

Um eine Verbindung mit der Cross lokale, müssen Sie ein lokales Netzwerk-Gateway zur Darstellung von Ihrem lokalen VPN-Geräts und eine Verbindung mit dem Gateway lokales Netzwerk das Gateway Azure VPN Verbindung erstellen. In diesem Beispiel befindet sich das Gateway Azure VPN im aktive Modus. Daher, obwohl nur eine VPN-Gerät (lokales Netzwerkgateway) und eine Verbindungsressource lokal ist, werden beide Azure VPN Gateway Instanzen S2S VPN-Tunnel mit dem lokalen Gerät herzustellen.

Stellen Sie bevor Sie fortfahren sicher, dass Sie [Teil 1](#aagateway) dieser Übung abgeschlossen haben.

### <a name="step-1---create-and-configure-the-local-network-gateway"></a>Schritt 1: Erstellen und Konfigurieren des Gateways lokales Netzwerk

#### <a name="1-declare-your-variables"></a>1. Deklarieren der Variablen

In dieser Übung weiterhin im Diagramm dargestellten Konfiguration erstellen. Achten Sie darauf, dass Sie die Werte durch die ersetzen, die Sie für Ihre Konfiguration verwenden möchten.

    $RG5           = "TestAARG5"
    $Location5     = "West US"
    $LNGName51     = "Site5_1"
    $LNGPrefix51   = "10.52.255.253/32"
    $LNGIP51       = "131.107.72.22"
    $LNGASN5       = 65050
    $BGPPeerIP51   = "10.52.255.253"

Ein paar Punkte, die Sie hinsichtlich der lokalen Netzwerk Gateway-Parameter zu beachten:

- Das Gateway lokales Netzwerk kann in der gleichen oder einem anderen Speicherort und Ressourcengruppe als VPN-Gateway sein. Dieses Beispiel zeigt ihnen in anderen Ressourcengruppen jedoch an derselben Stelle Azure.

- Wenn nur eine lokale VPN-Gerät vorhanden ist, wie oben gezeigt, kann die aktive Verbindung mit oder ohne BGP-Protokoll arbeiten. In diesem Beispiel wird die BGP für die Cross lokale Verbindung.

- Wenn BGP aktiviert ist, ist das Präfix, die, das Sie für das Gateway lokales Netzwerk deklarieren müssen, die Hostadresse Ihrer BGP Peer-IP-Adresse auf Ihrem Gerät VPN aus. In diesem Fall ist es eine /32 Präfix "10.52.255.253/32".

- Als Erinnerung müssen Sie verschiedene BGP ASNs zwischen Ihrem lokalen Netzwerken und Azure VNet verwenden. Wenn sie gleich sind, müssen Sie Ihre VNet ASN ändern, wenn es sich bei Ihrem lokalen VPN-Gerät das ASN bereits wird verwendet, um mit anderen BGP Nachbarn peer.

#### <a name="2-create-the-local-network-gateway-for-site5"></a>2. Erstellen des Gateways lokales Netzwerk für Site5
    
Bevor Sie fortfahren, stellen Sie sicher, dass Sie weiterhin Abonnement 1 verbunden sind. Erstellen der Ressourcengruppe an, wenn sie noch nicht erstellt wird.

    New-AzureRmResourceGroup       -Name $RG5 -Location $Location5
    New-AzureRmLocalNetworkGateway -Name $LNGName51 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP51 -AddressPrefix $LNGPrefix51 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP51

### <a name="step-2---connect-the-vnet-gateway-and-local-network-gateway"></a>Schritt 2 – schließen Sie die VNet Gateway und lokale Netzwerk-gateway

#### <a name="1-get-the-two-gateways"></a>1 holen Sie 1 sich die zwei gateways

    $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
    $lng5gw1 = Get-AzureRmLocalNetworkGateway   -Name $LNGName51 -ResourceGroupName $RG5

#### <a name="2-create-the-testvnet1-to-site5-connection"></a>2. erstellen Sie die TestVNet1 auf Site5-Verbindung

In diesem Schritt erstellen Sie die Verbindung aus TestVNet1 zu Site5_1 mit "EnableBGP" auf $True festlegen.

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection151 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw1 -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP True

#### <a name="3-vpn-and-bgp-parameters-for-your-on-premises-vpn-device"></a>3. VPN und BGP Parameter für das lokale VPN-Gerät

Im folgenden Beispiel werden die Parameter, die Sie in Abschnitt Konfiguration BGP auf Ihrem lokalen VPN-Gerät für diese Übung eingeben werden aufgeführt:

    - Site5 ASN: 65050
    - Site5 BGP IP-: 10.52.255.253
    - Präfixe, vorstellen: (zum Beispiel) 10.51.0.0/16 und 10.52.0.0/16
    - Azure VNet ASN: 65010
    - Azure VNet BGP IP 1: 10.12.255.4 für Tunnel zu 40.112.190.5
    - Azure VNet BGP IP 2: 10.12.255.5 für Tunnel zu 138.91.156.129
    - Statische leitet: Ziel 10.12.255.4/32, Nexthop der VPN-Tunnel Schnittstelle mit 40.112.190.5 Ziel 10.12.255.5/32, der VPN-Tunnel zu 138.91.156.129 Schnittstelle, Nexthop
    - eBGP Multihop: Vergewissern Sie sich die Option "Multihop" für eBGP auf Ihrem Gerät aktiviert ist, falls erforderlich

Die Verbindung hergestellt werden soll, nach ein paar Minuten, und die Peeringliste BGP-Sitzung wird gestartet, sobald die IPSec-Verbindung eingerichtet wird. In diesem Beispiel wurde bisher nur eine lokale VPN-Gerät im unten gezeigten Diagramm resultierender konfiguriert:

![aktiv-aktiv-crossprem](./media/vpn-gateway-activeactive-rm-powershell/active-active.png)

### <a name="step-3---connect-two-on-premises-vpn-devices-to-the-active-active-vpn-gateway"></a>Schritt 3: Herstellen einer Verbindung aktive VPN-Gateway mit zwei lokalen VPN-Geräte

Wenn Sie zwei VPN-Geräte in demselben lokalen Netzwerk verfügen, können Sie zwei Redundanz durch Herstellen einer Verbindung des Gateways Azure VPN auf das zweite VPN-Gerät erzielen.

#### <a name="1-create-the-second-local-network-gateway-for-site5"></a>1. erstellen Sie 1. das zweite lokales Netzwerkgateway für Site5

Beachten Sie, dass die Gateway IP-Adresse, Adresspräfix und BGP Peeringliste Adresse für das zweite lokales Netzwerkgateway nicht mit dem vorherigen lokales Netzwerkgateway für das lokale Netzwerk überlappen müssen. 

    $LNGName52     = "Site5_2"
    $LNGPrefix52   = "10.52.255.254/32"
    $LNGIP52       = "131.107.72.23"
    $BGPPeerIP52   = "10.52.255.254"

    New-AzureRmLocalNetworkGateway -Name $LNGName52 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP52 -AddressPrefix $LNGPrefix52 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP52
 
#### <a name="2-connect-the-vnet-gateway-and-the-second-local-network-gateway"></a>2. Schließen Sie das Gateway VNet und das zweite lokales Netzwerkgateway

Erstellen Sie die Verbindung von TestVNet1 zu Site5_2 mit "EnableBGP" auf $True festlegen

    $lng5gw2 = Get-AzureRmLocalNetworkGateway   -Name $LNGName52 -ResourceGroupName $RG5

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection152 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw2 -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP True

#### <a name="3-vpn-and-bgp-parameters-for-your-second-on-premises-vpn-device"></a>3. VPN und BGP Parameter für das zweite lokalen VPN-Gerät

Auf ähnliche Weise, klicken Sie unter Listen die Parameter geben Sie in das zweite VPN-Gerät ein:

    - Site5 ASN: 65050
    - Site5 BGP IP-: 10.52.255.254
    - Präfixe, vorstellen: (zum Beispiel) 10.51.0.0/16 und 10.52.0.0/16
    - Azure VNet ASN: 65010
    - Azure VNet BGP IP 1: 10.12.255.4 für Tunnel zu 40.112.190.5
    - Azure VNet BGP IP 2: 10.12.255.5 für Tunnel zu 138.91.156.129
    - Statische leitet: Ziel 10.12.255.4/32, Nexthop der VPN-Tunnel Schnittstelle mit 40.112.190.5 Ziel 10.12.255.5/32, der VPN-Tunnel zu 138.91.156.129 Schnittstelle, Nexthop
    - eBGP Multihop: Vergewissern Sie sich die Option "Multihop" für eBGP auf Ihrem Gerät aktiviert ist, falls erforderlich

Sobald die Verbindung (Tunnel) eingerichtet werden, haben Sie zwei redundante VPN-Geräte und Verbinden von Ihrem lokalen Netzwerk und Azure Tunnel:

![zwei-Redundanz-crossprem](./media/vpn-gateway-activeactive-rm-powershell/dual-redundancy.png)


## <a name="a-name-aav2vapart-3---establish-an-active-active-vnet-to-vnet-connection"></a><a name ="aav2v"></a>Teil 3: Stellen Sie eine aktive VNet-VNet-Verbindung

In diesem Abschnitt erstellt eine aktive VNet-VNet-Verbindung mit BGP. 

Die folgenden Anweisungen fortsetzen aus den vorherigen Schritten aufgeführten. [Teil 1](#aagateway) zum Erstellen und Konfigurieren von TestVNet1 und VPN-Gateway mit BGP muss abgeschlossen sein. 

### <a name="step-1---create-testvnet2-and-the-vpn-gateway"></a>Schritt 1 – Erstellen von TestVNet2 und VPN-gateway

Es ist wichtig, um sicherzustellen, dass die IP-Adresse das neue virtuelle Netzwerk TestVNet2, Speicherplatz für alle Ihre VNet Bereiche nicht überlappt.

In diesem Beispiel wird die virtuellen Netzwerke zu einziges Abonnement gehören. Sie können VNet-VNet-Verbindungen zwischen verschiedenen Abonnements einrichten; Näheres [Konfigurieren einer VNet-VNet - Verbindung](./vpn-gateway-vnet-vnet-rm-ps.md) , um weitere Details zu erfahren. Stellen Sie sicher, dass Sie Hinzufügen der "-EnableBgp $True" beim Erstellen der Verbindungen zum BGP aktivieren.

#### <a name="1-declare-your-variables"></a>1. Deklarieren der Variablen

Achten Sie darauf, dass Sie die Werte durch die ersetzen, die Sie für Ihre Konfiguration verwenden möchten.

    $RG2           = "TestAARG2"
    $Location2     = "East US"
    $VNetName2     = "TestVNet2"
    $FESubName2    = "FrontEnd"
    $BESubName2    = "Backend"
    $GWSubName2    = "GatewaySubnet"
    $VNetPrefix21  = "10.21.0.0/16"
    $VNetPrefix22  = "10.22.0.0/16"
    $FESubPrefix2  = "10.21.0.0/24"
    $BESubPrefix2  = "10.22.0.0/24"
    $GWSubPrefix2  = "10.22.255.0/27"
    $VNet2ASN      = 65020
    $DNS2          = "8.8.8.8"
    $GWName2       = "VNet2GW"
    $GW2IPName1    = "VNet2GWIP1"
    $GW2IPconf1    = "gw2ipconf1"
    $GW2IPName2    = "VNet2GWIP2"
    $GW2IPconf2    = "gw2ipconf2"
    $Connection21  = "VNet2toVNet1"
    $Connection12  = "VNet1toVNet2"

#### <a name="2-create-testvnet2-in-the-new-resource-group"></a>2. erstellen Sie TestVNet2 in die neue Ressourcengruppe

    New-AzureRmResourceGroup -Name $RG2 -Location $Location2

    $fesub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName2 -AddressPrefix $FESubPrefix2
    $besub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName2 -AddressPrefix $BESubPrefix2
    $gwsub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName2 -AddressPrefix $GWSubPrefix2

    New-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2 -Location $Location2 -AddressPrefix $VNetPrefix21,$VNetPrefix22 -Subnet $fesub2,$besub2,$gwsub2

#### <a name="3-create-the-active-active-vpn-gateway-for-testvnet2"></a>3. erstellen Sie das aktive VPN Gateway für TestVNet2

Fordern Sie zwei öffentliche IP-Adressen für das Gateway bereitzustellenden, den Sie für Ihre VNet erstellen. Sie können auch das Subnetz und die IP-Konfigurationen erforderlich definieren. 

    $gw2pip1    = New-AzureRmPublicIpAddress -Name $GW2IPName1 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic
    $gw2pip2    = New-AzureRmPublicIpAddress -Name $GW2IPName2 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic

    $vnet2      = Get-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
    $subnet2    = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
    $gw2ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW2IPconf1 -Subnet $subnet2 -PublicIpAddress $gw2pip1
    $gw2ipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW2IPconf2 -Subnet $subnet2 -PublicIpAddress $gw2pip2

Erstellen von VPN-Gateway mit den AS-Nummer und die Kennzeichnung "EnableActiveActiveFeature". Beachten Sie, dass die Standardeinstellung ASN auf Ihren Azure VPN Gateways überschrieben werden müssen. Die ASNs für die verbundenen VNets müssen BGP und Übertragung routing aktivieren unterschiedlich sein.

    New-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gw2ipconf1,$gw2ipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance -Asn $VNet2ASN -EnableActiveActiveFeature

### <a name="step-2---connect-the-testvnet1-and-testvnet2-gateways"></a>Schritt 2: Verbinden der Gateways TestVNet1 und TestVNet2

In diesem Beispiel werden beide Gateways im selben Abonnement aus. Sie können diesen Schritt abgeschlossen haben, in der gleichen PowerShell-Sitzung.

#### <a name="1-get-both-gateways"></a>1. erste beide gateways

Vergewissern Sie sich, melden Sie sich und beim Herstellen einer Verbindung Abonnement 1 mit.

    $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
    $vnet2gw = Get-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2
    
#### <a name="2-create-both-connections"></a>2. erstellen Sie beide Verbindungen

In diesem Schritt können Sie die Verbindung zwischen TestVNet1 und TestVNet2 und die Verbindung TestVNet2 zu TestVNet1 erstellen.

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

>[AZURE.IMPORTANT] Achten Sie darauf, dass BGP für beide Verbindungen zu aktivieren.

Nach Abschluss der Herstellen der Verbindung in wenigen Minuten, und die BGP Schritten, werden Peeringliste Sitzung einrichten, nachdem die VNet-VNet-Verbindung mit zwei Redundanz abgeschlossen ist:

![aktiv-aktiv-v2v](./media/vpn-gateway-activeactive-rm-powershell/vnet-to-vnet.png)

## <a name="a-name-aaupdateapart-4---update-existing-gateway-between-active-active-and-active-standby"></a><a name ="aaupdate"></a>Teil 4 - Update vorhandenen Gateway zwischen aktive und aktiv-standby

Im letzte Abschnitt wird beschrieben, wie Sie einer vorhandenen Azure VPN Gateway aus aktiv-Standby aktive Modus oder umgekehrt konfigurieren können.

>[AZURE.IMPORTANT] Bitte beachten Sie, dass der aktive Modus nur in HighPerformance SKU funktioniert

### <a name="configure-an-active-standby-gateway-to-active-active-gateway"></a>Konfigurieren eines aktiv-Standby-Gateways für die aktive gateway

#### <a name="1-gateway-parameters"></a>1. Parameter gateway

Im folgende Beispiel konvertiert einen aktiv-Standby-Gateway in eine aktive Gateway. Sie müssen eine andere öffentliche IP-Adresse zu erstellen, und klicken Sie dann Hinzufügen einer zweiten Gateway-IP-Konfigurations. Folgende enthält die Parameter verwendet:

    $GWName     = "TestVNetAA1GW"
    $VNetName   = "TestVNetAA1"
    $RG         = "TestVPNActiveActive01"
    $GWIPName2  = "gwpip2"
    $GWIPconf2  = "gw1ipconf2"

    $vnet       = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
    $subnet     = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
    $gw         = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
    $location   = $gw.Location

#### <a name="2-create-the-public-ip-address-then-add-the-second-gateway-ip-configuration"></a>2. erstellen Sie die öffentliche IP-Adresse, und klicken Sie dann fügen die zweite Gateway IP-Konfiguration hinzu

    $gwpip2     = New-AzureRmPublicIpAddress -Name $GWIPName2 -ResourceGroupName $RG -Location $location -AllocationMethod Dynamic
    Add-AzureRmVirtualNetworkGatewayIpConfig -VirtualNetworkGateway $gw -Name $GWIPconf2 -Subnet $subnet -PublicIpAddress $gwpip2 

#### <a name="3-enable-active-active-mode-and-update-the-gateway"></a>3 aktiv-aktiv-Modus aktivieren und ein update des Gateways

Sie müssen des Gateway-Objekts in die eigentliche Aktualisierung ausgelöst PowerShell festlegen. Die SKU des Gatewayobjekts muss auch in HighPerformance geändert werden, da es als Standard zuvor erstellt wurde.

    Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -EnableActiveActiveFeature -GatewaySku HighPerformance

Diese Aktualisierung kann 30 bis 45 Minuten dauern.

### <a name="configure-an-active-active-gateway-to-active-standby-gateway"></a>Konfigurieren eines Gateways aktive mit aktiv-Standby-gateway

#### <a name="1-gateway-parameters"></a>1. Parameter gateway

Verwenden Sie die gleichen Parameter wie oben, und erhalten Sie den Namen der IP-Konfiguration, die Sie entfernen möchten.

    $GWName     = "TestVNetAA1GW"
    $RG         = "TestVPNActiveActive01"

    $gw         = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
    $ipconfname = $gw.IpConfigurations[1].Name

#### <a name="2-remove-the-gateway-ip-configuration-and-disable-the-active-active-mode"></a>2. entfernen Sie die Gateway-IP-Konfiguration, und deaktivieren Sie den aktive Modus

Auf ähnliche Weise müssen Sie den Gateway-Objekts in die eigentliche Aktualisierung ausgelöst PowerShell festlegen.

    Remove-AzureRmVirtualNetworkGatewayIpConfig -Name $ipconfname -VirtualNetworkGateway $gw
    Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -DisableActiveActiveFeature

Dieses Update kann bis zu 30 und 45 Minuten dauern.


## <a name="next-steps"></a>Nächste Schritte

Nachdem die Verbindung abgeschlossen ist, können Sie Ihre virtuelle Netzwerke virtuellen Computern hinzufügen. Schritte finden Sie unter [Erstellen eines virtuellen Computers](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .


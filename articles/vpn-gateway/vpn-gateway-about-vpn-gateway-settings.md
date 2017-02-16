<properties 
   pageTitle="Informationen zu VPN-Gateway-Einstellungen für virtuelles Netzwerkgateways | Microsoft Azure"
   description="Informationen Sie zu VPN-Gateway-Einstellungen für Azure-virtuellen Netzwerk."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager,azure-service-management"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="cherylmc" />

# <a name="about-vpn-gateway-settings"></a>Informationen zu VPN-Gateway-Einstellungen

Eine VPN-Gateway Verbindung Lösung abhängig von der Konfiguration von mehreren Ressourcen um Netzwerkverkehr zwischen virtuellen Netzwerken senden und lokale Speicherorte. Jede Ressource enthält konfigurierbare Einstellungen. Die Kombination von Ressourcen und Einstellungen bestimmt das Ergebnis Verbindung.

In den Abschnitten in diesem Artikel werden die Ressourcen und Einstellungen, die auf einem VPN-Gateway im Bereitstellungsmodell **Ressourcenmanager** beziehen. Möglicherweise finden Sie es hilfreich sein, die verfügbaren Varianten mithilfe der Verbindung Suchtopologie Diagrammen anzeigen. Sie können die Beschreibungen und Suchtopologie Diagramme für jede Verbindung Lösung im Artikel [Über VPN-Gateway](vpn-gateway-about-vpngateways.md) suchen. 

## <a name="a-namegwtypeagateway-types"></a><a name="gwtype"></a>Gateway-Typen

Jedes virtuelles Netzwerk kann nur eine virtuelle Netzwerk-Gateway jedes Typs verfügen. Wenn Sie ein Gateway virtuelles Netzwerk erstellen, müssen Sie sicherstellen, der Typ des Gateways für Ihre Konfiguration korrekt sind.

Die verfügbaren Werte für - GatewayType sind: 

- VPN
- ExpressRoute

Ein VPN-Gateway erfordert die `-GatewayType` *Vpn*.  

Beispiel:

    New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
    -Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn `
    -VpnType RouteBased
 

## <a name="a-namegwskuagateway-skus"></a><a name="gwsku"></a>Gateway SKUs


[AZURE.INCLUDE [vpn-gateway-gwsku-include](../../includes/vpn-gateway-gwsku-include.md)]

### <a name="configuring-the-gateway-sku"></a>Konfigurieren des Gateways SKU

**Angeben des Gateways SKU Azure-Portal**

Wenn Sie Azure-Portal verwenden, um ein Ressourcenmanager virtuelle Netzwerk-Gateway zu erstellen, können Sie das Gateway SKU mithilfe der Dropdownliste auswählen. Die Optionen, die, denen Sie mit präsentiert werden, entsprechen den Gateway-Typ und VPN-Typ aus, die Sie auswählen.

Beispielsweise, wenn Sie den Gateway-Typ 'VPN' und der VPN-Typ 'Policy-basierte' auswählen, wird nur die 'Einfache' SKU da dies der einzige SKU für Richtlinien VPN verfügbar ist. Wenn Sie 'Routing-basierten' auswählen, können Sie Basic, Standard und HighPerformance SKUs auswählen. 


**Angeben des Gateways SKU mithilfe der PowerShell**


Im folgende Beispiel der PowerShell gibt an, die `-GatewaySku` als *Standard*.

    New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
    -Location 'West US' -IpConfigurations $gwipconfig -GatewaySku Standard `
    -GatewayType Vpn -VpnType RouteBased

**Ändern eines Gateways SKU**

Wenn Sie ein upgrade Ihrer Gateways SKU mit einer leistungsfähigeren SKU (von Basic/Standard zu HighPerformance möchten) können Sie die `Resize-AzureRmVirtualNetworkGateway` PowerShell-Cmdlet. Sie können auch das Gateway SKU Größe, die mit diesem Cmdlet herabstufen.

Im folgende Beispiel der PowerShell zeigt einen Gateway SKU wird geändert werden HighPerformance.

    $gw = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
    Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance

### <a name="estimated-aggregate-throughput-by-gateway-sku-and-type"></a>Geschätzte aggregierte Durchsatz von einem Gateway SKU und Typ

In der folgenden Tabelle zeigt die Typen Gateway und die geschätzte aggregierte Durchsatz. Diese Tabelle gilt für die Ressourcenmanager und klassischen Bereitstellungsmodelle.

[AZURE.INCLUDE [vpn-gateway-table-gwtype-aggthroughput](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)] 


## <a name="a-nameconnectiontypeaconnection-types"></a><a name="connectiontype"></a>Verbindungstypen

Im Bereitstellungsmodell Ressourcenmanager erfordert jede Konfiguration einen bestimmten virtuelles Netzwerk Gateway Verbindungstyp. Die verfügbaren Ressourcenmanager PowerShell Werte für `-ConnectionType` sind:

- IPsec
- Vnet2Vnet
- ExpressRoute
- VPNClient

Im folgenden Beispiel PowerShell erstellen wir eine S2S Verbindung, die den Verbindungstyp *IPsec*erforderlich ist.

    New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg `
    -Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
    -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'


## <a name="a-namevpntypeavpn-types"></a><a name="vpntype"></a>VPN-Typen

Wenn Sie das Gateway virtuelles Netzwerk für eine VPN-Gateway-Konfiguration erstellen, müssen Sie einen Typ VPN angeben. VPN, die Sie auswählen, richtet sich nach der topologieaktivierung Verbindung, die Sie erstellen möchten. Beispielsweise erfordert eine Verbindung P2S einen Typ RouteBased VPN. Ein anderes VPN kann auch auf der Hardware abhängig sind, die Sie verwenden möchten. S2S Konfigurationen erfordern ein VPN-Gerät. Einige VPN-Geräten unterstützt nur einen bestimmten Typ von VPN auf.

Der VPN-Typ aus, die, den Sie auswählen, erfüllen muss alle die Verbindung Anforderungen für die Lösung, dass Sie erstellen möchten. Beispielsweise, wenn Sie ein Gateway S2S VPN und eine P2S VPN-Gateway-Verbindung für das gleiche virtuelle Netzwerk erstellen möchten, verwenden VPN-Typ *RouteBased* Sie da P2S einen Typ RouteBased VPN erforderlich ist. Sie müssen auch überprüfen, dass Ihr Gerät VPN eine Verbindung RouteBased VPN unterstützt. 

Nachdem ein Gateway virtuelles Netzwerk erstellt wurde, können Sie den Typ des VPN nicht ändern. Sie müssen Löschen des Gateways virtuelles Netzwerk und Erstellen eines neuen Kontos. Es gibt zwei Arten von VPN:

[AZURE.INCLUDE [vpn-gateway-vpntype](../../includes/vpn-gateway-vpntype-include.md)]


Im folgende Beispiel der PowerShell gibt an, die `-VpnType` als *RouteBased*. Beim Erstellen eines Gateways, müssen Sie sicherstellen, dass die - VpnType für Ihre Konfiguration korrekt ist. 

    New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
    -Location 'West US' -IpConfigurations $gwipconfig `
    -GatewayType Vpn -VpnType RouteBased

##  <a name="a-namerequirementsagateway-requirements"></a><a name="requirements"></a>Gateway-Anforderungen

[AZURE.INCLUDE [vpn-gateway-table-requirements](../../includes/vpn-gateway-table-requirements-include.md)] 


## <a name="a-namegwsubagateway-subnet"></a><a name="gwsub"></a>Gateway Subnetz

Um ein Gateway virtuelles Netzwerk konfigurieren zu können, müssen Sie zuerst ein Gateway Subnetz für Ihre VNet zu erstellen. Das Gateway Subnetz muss den Namen *GatewaySubnet* ordnungsgemäß funktioniert. Dieser Name kann Azure wissen, dass dieses Subnetz für das Gateway verwendet werden soll.

Die minimale Größe des Gateways Subnetz hängt vollständig die Konfiguration, die Sie erstellen möchten. Obwohl es möglich, erstellen Sie ein Gateway-Subnetz /29 möglichst klein ist, es empfiehlt sich, dass Sie ein Gateway Subnetz /28 oder größere erstellen (/ 28, /27, /26, usw..). 

Erstellen einer größeren Gateway verhindert, dass von gegen Gateway Größe Einschränkungen ausführt. Angenommen, Sie möglicherweise ein Gateway virtuelles Netzwerk mit einer Gateway Subnetz Größe /29 für eine Verbindung S2S erstellt haben. Sie jetzt möchten Konfigurieren einer S2S/ExpressRoute gleichzeitig Konfiguration verwendet werden. Die Konfiguration ist Gateway Subnetz Mindestgröße /28 erforderlich. Um Ihre Konfiguration zu erstellen, müssen Sie das Gateway Subnetz angepasst den Mindestanforderungen für die Verbindung, also /28 ändern.

Im folgende Beispiel der Ressourcenmanager PowerShell zeigt ein Gateway Subnetz mit dem Namen GatewaySubnet. Sie können sehen, dass die CIDR-Notation gibt an, einen /27, welche genügend IP-Adressen für die meisten Konfigurationen ermöglicht, die aktuell vorhanden sind.

    Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/27

[AZURE.INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)] 


## <a name="a-namelngalocal-network-gateways"></a><a name="lng"></a>Lokales Netzwerkgateways

Beim Erstellen einer VPN-Gateway-Konfigurations stellt das Gateway lokales Netzwerk häufig Ihren lokalen Standort aus. Das Gateway lokales Netzwerk wurde im Bereitstellungsmodell klassischen als eine lokale Website bezeichnet. 

Sie benennen lokales Netzwerkgateways, die öffentliche IP-Adresse des Geräts lokalen VPN, und geben Sie die Adresspräfixe, die sich auf den lokalen Speicherort befinden. Azure befasst sich mit der Ziel Adresspräfixe für Netzwerkdatenverkehr, berät die Konfiguration, die Sie für Ihr lokales Netzwerkgateway angegeben haben, und leitet Pakete entsprechend weiter. Darüber hinaus geben lokales Netzwerkgateways für VNet-VNet-Konfigurationen, die eine VPN-Gateway-Verbindung zu verwenden.

Im folgende Beispiel der PowerShell erstellt ein neues lokales Netzwerk-Gateway:

    New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
    -Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.5.51.0/24'

Manchmal müssen Sie die Einstellungen für lokales Netzwerk Gateway zu ändern. Beispielsweise beim Hinzufügen oder Ändern von Adressbereichs, oder wenn sich die IP-Adresse des Geräts VPN ändert. Für eine klassische VNet können Sie diese Einstellungen in der klassischen Portal auf der Seite lokale Netzwerke ändern. Für Ressourcenmanager, finden Sie unter [mithilfe der PowerShell gatewayeinstellungen für lokales Netzwerk ändern](vpn-gateway-modify-local-network-gateway.md).

## <a name="a-nameresourcesarest-apis-and-powershell-cmdlets"></a><a name="resources"></a>REST-APIs und PowerShell-cmdlets

Zusätzliche technische Ressourcen und bestimmte syntaxanforderungen bei Verwendung von REST-APIs und PowerShell-Cmdlets für VPN-Gateway-Konfigurationen finden Sie in den folgenden Seiten angezeigt:

|**Klassische** | **Ressourcenmanager**|
|-----|----|
|[PowerShell](https://msdn.microsoft.com/library/mt270335.aspx)|[PowerShell](https://msdn.microsoft.com/library/mt163510.aspx)|
|[REST-API](https://msdn.microsoft.com/library/jj154113.aspx)|[REST-API](https://msdn.microsoft.com/library/mt163859.aspx)|


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu verfügbare Verbindung Konfigurationen finden Sie unter [Informationen zum VPN-Gateway](vpn-gateway-about-vpngateways.md) . 







 

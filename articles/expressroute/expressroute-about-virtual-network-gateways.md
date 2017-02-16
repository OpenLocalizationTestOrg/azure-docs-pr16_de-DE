<properties 
   pageTitle="Informationen zu ExpressRoute virtuelle Netzwerkgateways | Microsoft Azure"
   description="Erfahren Sie virtuelle Netzwerkgateways für ExpressRoute aus."
   services="expressroute"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager, azure-service-management"/>
<tags 
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/03/2016"
   ms.author="cherylmc" />

# <a name="about-virtual-network-gateways-for-expressroute"></a>Informationen zu virtuellen Netzwerkgateways für ExpressRoute


Ein Gateway virtuelles Netzwerk dient zum Senden von Netzwerkverkehr zwischen Azure virtuelle Netzwerke und lokale Speicherorte. Wenn Sie eine Verbindung ExpressRoute konfigurieren, müssen Sie erstellen und Konfigurieren eines Gateways virtuelles Netzwerk und eine-Gateway-Verbindung.

Wenn Sie ein Gateway virtuelles Netzwerk erstellen, geben Sie mehrere Einstellungen aus. Eine erforderliche Einstellung gibt an, ob das Gateway für ExpressRoute oder Website-zu-Standort VPN-Verkehr verwendet werden. Im Bereitstellungsmodell Ressourcenmanager ist die Einstellung '-GatewayType'.

Wenn Netzwerkverkehr auf eine dedizierte private Verbindung gesendet wird, verwenden Sie den Gateway-Typ 'ExpressRoute'. Dies wird auch als ExpressRoute Gateway bezeichnet. Wenn Netzwerkverkehr verschlüsselte über das Internet gesendet wird, verwenden Sie den Gateway-Typ 'Vpn' aus. Dies ist ein VPN-Gateway genannt. Website-zu-Standort verwenden Punkt-zu-Standort und VNet-VNet-Verbindungen alle einen VPN-Gateway. 

Jedes virtuelles Netzwerk kann nur eine virtuelle Netzwerk Gateways pro Gateway Typ verfügen. Beispielsweise können Sie haben eine virtuelle Netzwerk-Gateways, das - GatewayType Vpn verwendet, und eine, die - GatewayType ExpressRoute verwendet. In diesem Artikel liegt der Schwerpunkt auf dem ExpressRoute virtuelles Netzwerkgateway.

## <a name="a-namegwskuagateway-skus"></a><a name="gwsku"></a>Gateway SKUs

[AZURE.INCLUDE [expressroute-gwsku-include](../../includes/expressroute-gwsku-include.md)]

Wenn Sie ein upgrade Ihrer Gateways mit einer leistungsfähigeren Gateway SKU möchten, können Sie in den meisten Fällen des 'Größenänderungs--AzureRmVirtualNetworkGateway' PowerShell-Cmdlets verwenden. Dieses Verfahren funktioniert für Upgrades auf Standard- und HighPerformance SKUs. Jedoch um auf die SKU UltraPerformance zu aktualisieren, müssen Sie das Gateway neu zu erstellen.

###  <a name="a-nameaggthroughputaestimated-aggregate-throughput-by-gateway-sku"></a><a name="aggthroughput"></a>Geschätzte aggregierte Durchsatz von einem Gateway SKU


In der folgenden Tabelle zeigt die Typen Gateway und die geschätzte aggregierte Durchsatz. Diese Tabelle gilt für die Ressourcenmanager und klassischen Bereitstellungsmodelle.

[AZURE.INCLUDE [expressroute-table-aggthroughput](../../includes/expressroute-table-aggtput-include.md)] 


## <a name="a-nameresourcesarest-apis-and-powershell-cmdlets"></a><a name="resources"></a>REST-APIs und PowerShell-cmdlets

Zusätzliche technische Ressourcen und bestimmte syntaxanforderungen bei Verwendung von REST-APIs und PowerShell-Cmdlets für virtuelle Netzwerk-Gateway-Konfigurationen finden Sie in den folgenden Seiten angezeigt:

|**Klassische** | **Ressourcenmanager**|
|-----|----|
|[PowerShell](https://msdn.microsoft.com/library/mt270335.aspx)|[PowerShell](https://msdn.microsoft.com/library/mt163510.aspx)|
|[REST-API](https://msdn.microsoft.com/library/jj154113.aspx)|[REST-API](https://msdn.microsoft.com/library/mt163859.aspx)|


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu verfügbare Verbindung Konfigurationen finden Sie unter [Übersicht über die ExpressRoute](expressroute-introduction.md) . 







 

<properties
   pageTitle="Konfigurieren ein Gateways VNet für ExpressRoute mithilfe der PowerShell | Microsoft Azure"
   description="Konfigurieren ein Gateways VNet für eine klassische Bereitstellung VNet mithilfe der PowerShell für eine ExpressRoute Konfiguration modellieren."
   documentationCenter="na"
   services="expressroute"
   authors="charwen"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/03/2016"
   ms.author="charwen"/>

# <a name="configure-a-virtual-network-gateway-for-expressroute-using-the-classic-deployment-model-and-powershell"></a>Konfigurieren eines Gateways virtuelles Netzwerk für ExpressRoute mit dem Modell zur klassischen Bereitstellung und PowerShell

> [AZURE.SELECTOR]
- [PowerShell - Ressourcenmanager](expressroute-howto-add-gateway-resource-manager.md)
- [PowerShell - klassisch](expressroute-howto-add-gateway-classic.md)

In diesem Artikel führt Sie durch die Schritte zum Hinzufügen, Ändern der Größe und Entfernen eines Gateways virtuelles Netzwerk (VNet) für eine bereits vorhandene VNet. Die Schritte werden für diese Konfiguration speziell für VNets, die erstellt wurden das **Modell zur klassischen Bereitstellung** verwenden und, die werden verwendet in einer ExpressRoute Konfiguration. 

**Informationen zu Datenmodellen Azure-Bereitstellung**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="before-beginning"></a>Bevor Sie beginnen

Stellen Sie sicher, dass Sie die Azure PowerShell-Cmdlets für diese Konfiguration erforderlich installiert haben (1.0.2 oder höher). Wenn Sie die Cmdlets installiert haben, müssen Sie dies tun, bevor Sie beginnen die Konfigurationsschritte. Weitere Informationen zum Installieren von Azure PowerShell finden Sie unter [Informationen zum Installieren und konfigurieren Azure PowerShell](../powershell-install-configure.md).


[AZURE.INCLUDE [expressroute-gateway-classic-ps](../../includes/expressroute-gateway-classic-ps-include.md)]

    
## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie das Gateway VNet erstellt haben, können Sie Ihre VNet mit einer Verbindung ExpressRoute verknüpfen. [Link eine virtuelle Netzwerk eine Verbindung ExpressRoute](expressroute-howto-linkvnet-classic.md)angezeigt.

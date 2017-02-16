<properties
   pageTitle="Ändern der lokalen Netzwerk Gateway IP-Adresspräfixe und Gateway-IP | Microsoft Azure"
   description="Dieser Artikel führt Sie durch die IP-Adresspräfixe für Ihr lokales Netzwerkgateway ändern"
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
   ms.date="08/08/2016"
   ms.author="cherylmc"/>

# <a name="modify-local-network-gateway-settings-using-powershell"></a>Ändern von Einstellungen für lokales Netzwerk Gateway mithilfe der PowerShell

Manchmal ändert sich die Einstellungen für Ihr lokales Netzwerkgateway AddressPrefix oder GatewayIPAddress. Die folgenden Anweisungen hilft Ihnen die Ihrem lokalen Netzwerk Gateway-Einstellungen ändern. Sie können auch diese Einstellungen im Azure-Portal ändern.

## <a name="before-you-begin"></a>Vorbemerkung
    
Sie müssen die neueste Version der Azure Ressourcenmanager PowerShell-Cmdlets installieren. Weitere Informationen zum Installieren der PowerShell-Cmdlets finden Sie unter [Informationen zum Installieren und konfigurieren Azure PowerShell](../powershell-install-configure.md) .

## <a name="to-modify-ip-address-prefixes"></a>So ändern Sie die IP-Adresspräfixe

[AZURE.INCLUDE [vpn-gateway-modify-ip-prefix-rm](../../includes/vpn-gateway-modify-ip-prefix-rm-include.md)]

## <a name="to-modify-the-gateway-ip-address"></a>So ändern Sie die IP-Adresse des Gateways

[AZURE.INCLUDE [vpn-gateway-modify-lng-gateway-ip-rm](../../includes/vpn-gateway-modify-lng-gateway-ip-rm-include.md)]

## <a name="next-steps"></a>Nächste Schritte

Sie können die Verbindung Gateway überprüfen. Finden Sie unter [Überprüfen einer Gateway-Verbindung](vpn-gateway-verify-connection-resource-manager.md).


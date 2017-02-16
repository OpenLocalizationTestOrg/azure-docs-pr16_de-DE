<properties
   pageTitle="Überprüfen Sie die Verbindung eines Gateways | Microsoft Azure"
   description="In diesem Artikel wird gezeigt, wie eine Verbindung Gateway im Bereitstellungsmodell Ressourcenmanager überprüfen"
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
   ms.date="10/14/2016"
   ms.author="cherylmc"/>

# <a name="verify-a-gateway-connection"></a>Überprüfen Sie die Verbindung eines Gateways

Sie können die Verbindung des Gateways auf verschiedene Weise überprüfen. In diesem Artikel wird gezeigt, wie zum Überprüfen des Status einer Ressourcenmanager Gateway-Verbindung mithilfe von Azure-Portal und mithilfe der PowerShell werden.


## <a name="verify-using-powershell"></a>Vergewissern Sie sich mithilfe der PowerShell

Sie müssen die neueste Version der Azure Ressourcenmanager PowerShell-Cmdlets installieren. Informationen zum Installieren von PowerShell-Cmdlets finden Sie unter [Informationen zum Installieren und konfigurieren Azure PowerShell](../powershell-install-configure.md). Weitere Informationen zur Verwendung von Ressourcenmanager Cmdlets finden Sie unter [Verwenden von Windows PowerShell mit Ressourcenmanager](../powershell-azure-resource-manager.md).

### <a name="step-1-log-in-to-your-azure-account"></a>Schritt 1: Melden Sie sich bei Ihrem Konto Azure

1. Öffnen Sie in der PowerShell-Konsole mit erhöhten und eine Verbindung mit Ihrem Konto herstellen.

        Login-AzureRmAccount

2. Überprüfen Sie die Abonnements für das Konto ein.

        Get-AzureRmSubscription 

3. Geben Sie das Abonnement, das Sie verwenden möchten.

        Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

### <a name="step-2-verify-your-connection"></a>Schritt 2: Überprüfen Sie die Verbindung


[AZURE.INCLUDE [verify connection powershell](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)] 


## <a name="verify-using-the-azure-portal"></a>Vergewissern Sie sich über das Azure-portal

[AZURE.INCLUDE [verify connection portal](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)] 


## <a name="next-steps"></a>Nächste Schritte

- Sie können Ihre virtuelle Netzwerke virtuellen Computern hinzufügen. Schritte finden Sie unter [Erstellen eines virtuellen Computers](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .


<properties 
    pageTitle="Windows-virtuellen Computern erneut | Microsoft Azure" 
    description="Beschreibt, wie Windows-virtuellen Computern so RDP Verbindungsprobleme erneut bereitstellen." 
    services="virtual-machines-windows" 
    documentationCenter="virtual-machines" 
    authors="iainfoulds" 
    manager="timlt"
    tags="azure-resource-manager,top-support-issue" 
/>
    

<tags 
    ms.service="virtual-machines-windows" 
    ms.devlang="na" 
    ms.topic="support-article" 
    ms.tgt_pltfrm="vm-windows"
    ms.workload="infrastructure" 
    ms.date="09/19/2016" 
    ms.author="iainfou" 
/>


# <a name="redeploy-virtual-machine-to-new-azure-node"></a>Stellen Sie erneut bereit virtuellen Computers zu neuen Azure-Knoten

Wenn Sie eine Ansprechpartner gegenüberliegende wurden möglicherweise die Problembehandlung RDP (Remote Desktop) Verbindung oder einer Anwendung Zugriff auf Windows basierende Azure-virtuellen Computern (virtueller Computer), den virtuellen Computer erneutes helfen. Wenn Sie einen virtuellen Computer erneut bereitstellen, bewegt sich den virtuellen Computer auf einen anderen Knoten innerhalb der Azure-Infrastruktur und schaltet es dann wieder auf, Aufbewahrung Ihrer Konfigurationsoptionen und die zugeordneten Ressourcen. In diesem Artikel wird gezeigt, wie einen virtueller Computer mit Azure PowerShell oder der Azure-Portal erneut bereitgestellt werden können.

> [AZURE.NOTE] Nachdem Sie einen virtuellen Computer erneut bereitstellen, der temporäre Datenträger geht verloren, und dynamische IP-Adressen Schnittstelle virtuelles Netzwerk zugeordnet werden aktualisiert. 

## <a name="using-azure-powershell"></a>Mithilfe der PowerShell Azure

Stellen Sie sicher, dass die neuesten Azure PowerShell 1.x auf Ihrem Computer installiert sein. Weitere Informationen finden Sie unter [Informationen zum Installieren und konfigurieren Azure PowerShell](../powershell-install-configure.md).

Verwenden Sie diesen Azure PowerShell-Befehl des virtuellen Computers erneut bereitzustellen:

    Set-AzureRmVM -Redeploy -ResourceGroupName $rgname -Name $vmname 


[AZURE.INCLUDE [virtual-machines-common-redeploy-to-new-node](../../includes/virtual-machines-common-redeploy-to-new-node.md)]


## <a name="next-steps"></a>Nächste Schritte
Wenn beim Herstellen einer Verbindung mit Ihrem virtuellen Computer Probleme auftreten, können Sie bestimmte Hilfe zur [Problembehandlung RDP Verbindungen](virtual-machines-windows-troubleshoot-rdp-connection.md) oder [detaillierte RDP Schritte zur Problembehandlung](virtual-machines-windows-detailed-troubleshoot-rdp.md)finden. Wenn Sie die Anwendung ausführen Ihrer virtuellen Computers zugreifen können, können Sie auch die [Anwendung Behandlung von Problemen](virtual-machines-windows-troubleshoot-app-connection.md)lesen.
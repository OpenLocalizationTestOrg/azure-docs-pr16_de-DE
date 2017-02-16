<properties 
    pageTitle="Erneut Linux virtuellen Computern bereitstellen | Microsoft Azure" 
    description="Beschreibt, wie Linux virtuellen Computern so SSH Verbindungsprobleme erneut bereitstellen." 
    services="virtual-machines-linux" 
    documentationCenter="virtual-machines" 
    authors="iainfoulds" 
    manager="timlt"
    tags="azure-resource-manager,top-support-issue" 
/>
    

<tags 
    ms.service="virtual-machines-linux" 
    ms.devlang="na" 
    ms.topic="support-article" 
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure" 
    ms.date="09/19/2016" 
    ms.author="iainfou" 
/>

# <a name="redeploy-virtual-machine-to-new-azure-node"></a>Stellen Sie erneut bereit virtuellen Computers zu neuen Azure-Knoten

Wenn Sie haben wurde gegenüberliegende Ansprechpartner, die bei der Problembehandlung SSH oder Anwendungszugriff auf eine Azure-virtuellen Computern (virtueller Computer), den virtuellen Computer erneutes möglicherweise helfen können. Wenn Sie einen virtuellen Computer erneut bereitstellen, bewegt sich den virtuellen Computer auf einen anderen Knoten innerhalb der Azure-Infrastruktur und schaltet es dann wieder auf, Aufbewahrung Ihrer Konfigurationsoptionen und die zugeordneten Ressourcen. In diesem Artikel wird gezeigt, wie einen virtueller Computer mit Azure CLI oder Azure-Portal erneut bereitgestellt werden können.

> [AZURE.NOTE] Nachdem Sie einen virtuellen Computer erneut bereitstellen, der temporäre Datenträger geht verloren, und dynamische IP-Adressen Schnittstelle virtuelles Netzwerk zugeordnet werden aktualisiert. 


## <a name="using-azure-cli"></a>Verwenden von Azure CLI

Stellen Sie sicher, dass Sie die [Neuesten Azure CLI installiert](../xplat-cli-install.md) haben, auf Ihrem Computer, und Sie werden im Modus Ressourcenmanager (`azure config mode arm`).

Verwenden Sie den folgenden Befehl aus Azure CLI des virtuellen Computers erneut bereitzustellen:

```bash
azure vm redeploy --resourcegroup <resourcegroup> --vm-name <vmname> 
```

Sie können den Status der Änderung virtueller Computer anzeigen, während sie die Schritte zum Bereitstellen, geht. Die `PowerState` von den virtuellen Computer wechselt von 'Running' auf 'Aktualisieren', dann'starten', und klicken Sie abschließend 'ausführen' wie es durch den Vorgang erneutes Bereitstellen auf einen neuen Host wechselt. Überprüfen des Status der virtuellen Computern innerhalb einer Ressourcengruppe mit:

```bash
azure vm list -g <resourcegroup>
```


[AZURE.INCLUDE [virtual-machines-common-redeploy-to-new-node](../../includes/virtual-machines-common-redeploy-to-new-node.md)]


## <a name="next-steps"></a>Nächste Schritte
Wenn beim Herstellen einer Verbindung mit Ihrem virtuellen Computer Probleme auftreten, können Sie bestimmte Hilfe zur [Problembehandlung SSH Verbindungen](virtual-machines-linux-troubleshoot-ssh-connection.md) oder [detaillierte SSH Schritte zur Problembehandlung](virtual-machines-linux-detailed-troubleshoot-ssh-connection.md)finden. Wenn Sie die Anwendung ausführen Ihrer virtuellen Computers zugreifen können, können Sie auch die [Anwendung Behandlung von Problemen](virtual-machines-linux-troubleshoot-app-connection.md)lesen.
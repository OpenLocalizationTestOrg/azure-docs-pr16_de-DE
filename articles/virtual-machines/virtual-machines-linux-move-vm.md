<properties
    pageTitle="Verschieben eines Linux virtuellen Computers | Microsoft Azure"
    description="Verschieben eines Linux VM in eine andere Azure-Abonnements oder Ressource Gruppe im Bereitstellungsmodell Ressourcenmanager."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="cynthn"/>

    


# <a name="move-a-linux-vm-to-another-subscription-or-resource-group"></a>Verschieben eines Linux VM in eine andere Gruppe von Abonnements oder von Ressourcen

In diesem Artikel führt Sie durch Verschieben von einem Linux VM zwischen Ressourcengruppen oder Abonnements. Verschieben eines virtuellen Computers zwischen Abonnements kann nützlich sein, wenn Sie einen virtuellen Computer in einem personal-Abonnements erstellt haben und nun Ihres Unternehmens Abonnement verschoben werden soll.

> [AZURE.NOTE] Neue Ressourcen-IDs werden als Teil der Verschiebung erstellt. Nachdem Sie der virtuellen Computer verschoben wurde, müssen Sie Ihre Tools und Skripts verwenden Sie die neue Ressourcen-IDs zu aktualisieren. 


## <a name="use-the-azure-cli-to-move-a-vm"></a>Verwenden Sie die Azure CLI zum Verschieben eines virtuellen Computers 

Um einen virtuellen erfolgreich verschieben möchten, müssen Sie den virtuellen Computer und alle zugehörigen unterstützenden Ressourcen zu verschieben. Liste aller Ressourcen in einer Ressourcengruppe und ihrer IDs mithilfe des Befehls **Azure Gruppe anzuzeigen** . Es hilfreich, leiten Sie die Ausgabe dieses Befehls in eine Datei, kopieren und fügen Sie die IDs in höher Befehle.

    azure group show <resourceGroupName>

Verwenden Sie zum Verschieben eines virtuellen Computers und seine Ressourcen zu einem anderen Ressourcengruppe Befehl CLI **Azure Ressource verschieben** . Im folgenden Beispiel wird gezeigt, wie zum Verschieben eines virtuellen Computers und den am häufigsten verwendeten Ressourcen, die erforderlich ist. Wir verwenden Sie den Parameter **-i** und übergeben eine durch Trennzeichen getrennte Liste (ohne Leerzeichen) IDs für die Ressourcen zu verschieben.

    
    vm=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Compute/virtualMachines/<vmName>
    nic=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/networkInterfaces/<nicName>
    nsg=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/networkSecurityGroups/<nsgName>
    pip=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/publicIPAddresses/<publicIPName>
    vnet=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/virtualNetworks/<vnetName>
    diag=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Storage/storageAccounts/<diagnosticStorageAccountName>
    storage=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Storage/storageAccounts/<storageAcountName>      
    
    azure resource move --ids $vm,$nic,$nsg,$pip,$vnet,$storage,$diag -d "<destinationResourceGroup>"
    
Wenn Sie den virtuellen Computer und seine Ressourcen auf ein anderes Abonnement verschieben möchten, fügen Sie der **– Ziel-SubscriptionId & #60; DestinationSubscriptionID & #62;** Parameter, um das Zielabonnement anzugeben.

Wenn Sie über die Befehlszeile auf einem Windows-Computer arbeiten, müssen Sie zum Hinzufügen eines **$** vor der Variablennamen, wenn Sie diese deklarieren. Dies ist nicht erforderlich, Linux.

Sie werden aufgefordert, zu bestätigen, dass die angegebene Ressource verschoben werden soll. Geben Sie **Y** , um zu bestätigen, dass Sie die Ressourcen verschieben möchten.
    

[AZURE.INCLUDE [virtual-machines-common-move-vm](../../includes/virtual-machines-common-move-vm.md)]

## <a name="next-steps"></a>Nächste Schritte

Sie können viele verschiedene Typen von Ressourcen zwischen Ressourcengruppen und Abonnements verschieben. Weitere Informationen finden Sie unter [Verschieben von Ressourcen zu neuen Ressourcengruppe oder das Abonnement](../resource-group-move-resources.md).    
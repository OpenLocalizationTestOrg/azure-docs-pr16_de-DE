<properties
    pageTitle="Verschieben eines Windows virtueller Computer | Microsoft Azure"
    description="Verschieben eines Windows virtuellen Computers in eine andere Azure-Abonnements oder Ressource Gruppe im Bereitstellungsmodell Ressourcenmanager."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="cynthn"/>

    


# <a name="move-a-windows-vm-to-another-azure-subscription-or-resource-group"></a>Verschieben eines Windows virtuellen Computers in eine andere Azure-Abonnements oder Ressource Gruppe 

In diesem Artikel führt Sie durch einen Windows-virtuellen Verschieben von zwischen Ressourcengruppen oder Abonnements. Wechseln zwischen Abonnements kann nützlich sein, wenn Sie einen virtuellen ursprünglich in einer personal-Abonnements erstellt haben und jetzt Ihres Unternehmens Abonnement weiterhin Ihre Arbeit verschoben werden soll.

> [AZURE.NOTE] Neue Ressourcen-IDs werden als Teil der Verschiebung erstellt. Nachdem Sie der virtuellen Computer verschoben wurde, müssen Sie Ihre Tools und Skripts verwenden Sie die neue Ressourcen-IDs zu aktualisieren. 


[AZURE.INCLUDE [virtual-machines-common-move-vm](../../includes/virtual-machines-common-move-vm.md)]


## <a name="use-powershell-to-move-a-vm"></a>Verwenden Sie zum Verschieben eines virtuellen Computers Powershell

Zum Verschieben eines virtuellen Computers zu einem anderen Ressourcengruppe müssen Sie sicherstellen, dass Sie auch alle abhängigen Ressourcen verschieben. Wenn Sie das Cmdlet verschieben-AzureRMResource verwenden zu können, benötigen Sie den Namen der Ressource und den Typ der Ressource ein. Sie können beide des Cmdlets suchen-AzureRMResource erhalten.

    Find-AzureRMResource -ResourceGroupNameContains "<sourceResourceGroupName>"
    

Zum Verschieben eines virtuellen Computers müssen mehrere Ressourcen zu verschieben. Wir können nur erstellen Sie separate Variablen für jede Ressource, und klicken Sie dann auflisten. In diesem Beispiel enthält die meisten grundlegenden Ressourcen für einen virtuellen Computer, aber Sie können mehrere Bedarf hinzufügen aus.

    $sourceRG = "<sourceResourceGroupName>"
    $destinationRG = "<destinationResourceGroupName>"
    
    $vm = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Compute/virtualMachines" -ResourceName "<vmName>"
    $storageAccount = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Storage/storageAccounts" -ResourceName "<storageAccountName>"
    $diagStorageAccount = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Storage/storageAccounts" -ResourceName "<diagnosticStorageAccountName>"
    $vNet = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/virtualNetworks" -ResourceName "<vNetName>"
    $nic = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/networkInterfaces" -ResourceName "<nicName>"
    $ip = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/publicIPAddresses" -ResourceName "<ipName>"
    $nsg = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/networkSecurityGroups" -ResourceName "<nsgName>"
    
    Move-AzureRmResource -DestinationResourceGroupName $destinationRG -ResourceId $vm.ResourceId, $storageAccount.ResourceId, $diagStorageAccount.ResourceId, $vNet.ResourceId, $nic.ResourceId, $ip.ResourceId, $nsg.ResourceId

Um die Ressourcen zu verschiedenen Abonnement verschieben möchten, schließen Sie den **- DestinationSubscriptionId** -Parameter ein. 

    Move-AzureRmResource -DestinationSubscriptionId "<destinationSubscriptionID>" -DestinationResourceGroupName $destinationRG -ResourceId $vm.ResourceId, $storageAccount.ResourceId, $diagStorageAccount.ResourceId, $vNet.ResourceId, $nic.ResourceId, $ip.ResourceId, $nsg.ResourceId



Sie werden aufgefordert werden, um zu bestätigen, dass Sie die angegebenen Ressourcen verschieben möchten. Geben Sie **Y** , um zu bestätigen, dass Sie die Ressourcen verschieben möchten.

  
## <a name="next-steps"></a>Nächste Schritte

Sie können viele verschiedene Typen von Ressourcen zwischen Ressourcengruppen und Abonnements verschieben. Weitere Informationen finden Sie unter [Verschieben von Ressourcen zu neuen Ressourcengruppe oder das Abonnement](../resource-group-move-resources.md).    
<properties
    pageTitle="Ändern der Größe eines Windows virtueller Computer | Microsoft Azure"
    description="Ändern der Größe eines Windows-Computers im Bereitstellungsmodell Ressourcenmanager, mithilfe der Powershell Azure erstellt."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="Drewm3"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="drewm"/>

    
# <a name="resize-a-windows-vm"></a>Ändern der Größe eines Windows virtueller Computer

In diesem Artikel wird gezeigt, wie zum Ändern der Größe eines Windows virtuellen Computers im Bereitstellungsmodell Ressourcenmanager mithilfe der Powershell Azure erstellt werden können.

Nach dem Erstellen eines virtuellen Computers (virtueller Computer) können Sie den virtuellen Computer nach oben oder unten skalieren, durch Ändern der Größe des virtuellen Computer. In einigen Fällen müssen Sie zuerst den virtuellen Computer freigeben. Dies kann geschehen, wenn die neue Größe nicht auf dem Cluster Hardware verfügbar ist, die derzeit den virtuellen Computer befindet.

## <a name="resize-a-windows-vm-not-in-an-availability-set"></a>Ändern der Größe eines Windows virtuellen Computers nicht in einer Menge Verfügbarkeit

1. Liste der Größen virtueller Computer, die auf dem Cluster Hardware verfügbar sind, der virtuellen Computer gehostet wird. 

    ```powershell
    Get-AzureRmVMSize -ResourceGroupName <resourceGroupName> -VMName <vmName> 
    ```

2. Wenn die gewünschte Größe nicht aufgeführt ist, führen Sie die folgenden Befehle zum Ändern der Größe des virtuellen Computer an. Wenn die gewünschte Größe nicht aufgeführt wird, fahren Sie mit Schritt 3.

    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroupName> -VMName <vmName>
    $vm.HardwareProfile.VmSize = "<newVMsize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName <resourceGroupName>
    ```

3. Wenn die gewünschte Größe nicht aufgeführt ist führen Sie die folgenden Befehle freigeben den virtuellen Computer, ihre Größe zu ändern, und starten Sie den virtuellen Computer neu.

    ```powershell
    $rgname = "<resourceGroupName>"
    $vmname = "<vmName>"
    Stop-AzureRmVM -ResourceGroupName $rgname -VMName $vmname -Force
    $vm = Get-AzureRmVM -ResourceGroupName $rgname -VMName $vmname
    $vm.HardwareProfile.VmSize = "<newVMSize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName $rgname
    Start-AzureRmVM -ResourceGroupName $rgname -Name $vmname
    ```

> [AZURE.WARNING] Freigeben von den virtuellen Computer frei eine dynamischen IP-Adressen, die den virtuellen Computer zugewiesen ist. Der Datenträger OS und Daten sind hiervon nicht betroffen. 

## <a name="resize-a-windows-vm-in-an-availability-set"></a>Ändern der Größe eines Windows virtuellen Computers in einem Satz Verfügbarkeit

Wenn die neue Größe für einen virtuellen Computer in einem Satz Verfügbarkeit nicht auf dem Hardware Cluster Hostinganbieter aktuell auf den virtuellen Computer verfügbar ist, müssen alle virtuellen Computern festlegen Verfügbarkeit zum Ändern der Größe des virtuellen Computer freigegeben werden. Möglicherweise müssen auch die Größe der anderen virtuellen Computern im Feld Verfügbarkeit festgelegt, nachdem ein virtueller Computer Größe geändert wurde aktualisiert. Führen Sie zum Ändern der Größe eines virtuellen Computers in einem Satz Verfügbarkeit der folgenden Schritte aus.

1. Liste der Größen virtueller Computer, die auf dem Cluster Hardware verfügbar sind, der virtuellen Computer gehostet wird.

    ```powershell
    Get-AzureRmVMSize -ResourceGroupName <resourceGroupName> -VMName <vmName>
    ```

2. Wenn die gewünschte Größe nicht aufgeführt ist, führen Sie die folgenden Befehle zum Ändern der Größe des virtuellen Computer an. Wenn es nicht aufgeführt ist, wechseln Sie zu Schritt 3.

    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroupName> -VMName <vmName>
    $vm.HardwareProfile.VmSize = "<newVmSize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName <resourceGroupName>
    ```

3. Wenn die gewünschte Größe nicht aufgeführt ist, fahren Sie mit den folgenden Schritten zum Freigeben aller virtuellen Computern Verfügbarkeit festlegen, Ändern der Größe virtuellen Computern und starten sie.

4.  Beenden Sie alle virtuellen Computern Verfügbarkeit festlegen.

    ```powershell
    $rg = "<resourceGroupName>"
    $as = Get-AzureRmAvailabilitySet -ResourceGroupName $rg
    $vmIds = $as.VirtualMachinesReferences
    foreach ($vmId in $vmIDs){
        $string = $vmID.Id.Split("/")
        $vmName = $string[8]
        Stop-AzureRmVM -ResourceGroupName $rg -Name $vmName -Force
    } 
    ```
              
5.  Ändern der Größe aus, und starten Sie die virtuellen Computern Verfügbarkeit festlegen.

    ```powershell
    $rg = "<resourceGroupName>"
    $newSize = "<newVmSize>"
    $as = Get-AzureRmAvailabilitySet -ResourceGroupName $rg
    $vmIds = $as.VirtualMachinesReferences
    foreach ($vmId in $vmIDs){
        $string = $vmID.Id.Split("/")
        $vmName = $string[8]
        $vm = Get-AzureRmVM -ResourceGroupName $rg -Name $vmName
        $vm.HardwareProfile.VmSize = $newSize
        Update-AzureRmVM -ResourceGroupName $rg -VM $vm
        Start-AzureRmVM -ResourceGroupName $rg -Name $vmName
    }
    ```

## <a name="next-steps"></a>Nächste Schritte

- Zusätzliche Skalierbarkeit mehrere Instanzen von virtuellen Computer ausführen und skalieren. Weitere Informationen finden Sie unter [Skalieren automatisch auf Windows-Computer in virtuellen Computern Skalierung festlegen](../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md).




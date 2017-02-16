<properties
    pageTitle="Ändern eine virtuellen Computern Verfügbarkeit Menge | Microsoft Azure"
    description="Informationen Sie zum Ändern der Verfügbarkeit für Ihre virtuellen Computer mithilfe von Azure PowerShell und das Modell zur Bereitstellung von Ressourcenmanager festlegen."
    keywords=""
    services="virtual-machines-windows"
    documentationCenter=""
    authors="Drewm3"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>
<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/15/2016"
    ms.author="drewm"/>



# <a name="change-the-availability-set-for-a-windows-vm"></a>Ändern der Verfügbarkeit für einen Windows-virtuellen festlegen

Die folgenden Schritte beschreiben, wie die Verfügbarkeit eines virtuellen Computers mit Azure PowerShell ändern. Ein virtueller Computer kann nur in einer Verfügbarkeit legen Sie bei der Erstellung hinzugefügt werden. Legen Sie akzeptieren, um die Verfügbarkeit zu ändern, müssen Sie löschen und neu erstellen des virtuellen Computers. 

## <a name="change-the-availability-set-using-powershell"></a>Ändern der Verfügbarkeit mithilfe der PowerShell festlegen

1. Erfassen Sie die folgenden wichtigen Details aus dem virtuellen Computer korrigiert werden muss.

    Namen der virtuellen Computer
    
    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <Name-of-resource-group> -Name <name-of-VM>
    $vm.Name
    ```
 
    Virtueller Speicher
    
    ```powershell
    $vm.HardwareProfile.VmSize
    ```

    Primäre Netzwerkschnittstelle und optional Netzwerk-Schnittstellen, wenn sie auf dem virtuellen Computer vorhanden sind.
    
    ```powershell
    $vm.NetworkProfile.NetworkInterfaces[0].Id
    ```

    OS Datenträger Profil

    ```powershell
    $vm.StorageProfile.OsDisk.OsType
    $vm.StorageProfile.OsDisk.Name
    $vm.StorageProfile.OsDisk.Vhd.Uri
    ```

    Datenträger Profile für jeden Datenträger Daten 
    
    ```powershell
    $vm.StorageProfile.DataDisks[<index>].Lun
    $vm.StorageProfile.DataDisks[<index>].Vhd.Uri
    ```

    Virtueller Computer Extensions installiert 
    
    ```powershell
    $vm.Extensions
    ```

2. Löschen Sie den virtuellen Computer, ohne die Datenträger oder im Netzwerkschnittstellen löschen.

    ```powershell
    Remove-AzureRmVM -ResourceGroupName <resourceGroupName> -Name <vmName> 
    ```

3. Erstellen Sie die Verfügbarkeit festgelegt, wenn er nicht bereits vorhanden ist

    ```powershell
    New-AzureRmAvailabilitySet -ResourceGroupName <resourceGroupName> -Name <availabilitySetName> -Location "<location>" 
    ```

4. Erstellen Sie den virtuellen Computer mithilfe der neuen Verfügbarkeit Satzes neu

    ```powershell
    $vm2 = New-AzureRmVMConfig -VMName <VM-name> -VMSize <vm-size> -AvailabilitySetId <availability-set-id>

    Set-AzureRmVMOSDisk -CreateOption "Attach" -VM <vmConfig> -VhdUri <osDiskURI> -Name <osDiskName> [-Windows | -Linux]

    Add-AzureRmVMNetworkInterface -VM <vmConfig> -Id  <nicId> 

    New-AzureRmVM -ResourceGroupName <resourceGroupName> -Location <location> -VM <vmConfig>
    ``` 

5. Hinzufügen von Daten Datenträger und Erweiterungen. Weitere Informationen finden Sie unter [Anfügen Datenträger zum virtuellen Computer](virtual-machines-windows-attach-disk-portal.md) und [Erweiterung Konfiguration Beispiele](virtual-machines-windows-extensions-configuration-samples.md). Daten- und Erweiterungen können den virtuellen Computer mithilfe der PowerShell oder Azure CLI hinzugefügt werden.

## <a name="example-script"></a>Beispielskript

Das folgende Skript enthält ein Beispiel für das Sammeln der erforderlichen Informationen, Löschen der ursprünglichen virtuellen und es dann in einer neuen Verfügbarkeit neu zu erstellen.

```powershell
    #set variables
    $rg = "demo-resource-group"
    $vmName = "demo-vm"
    $newAvailSetName = "demo-as"
    $outFile = "C:\temp\outfile.txt"

    #Get VM Details
    $OriginalVM = get-azurermvm -ResourceGroupName $rg -Name $vmName

    #Output VM details to file
    "VM Name: " | Out-File -FilePath $outFile 
    $OriginalVM.Name | Out-File -FilePath $outFile -Append

    "Extensions: " | Out-File -FilePath $outFile -Append
    $OriginalVM.Extensions | Out-File -FilePath $outFile -Append

    "VMSize: " | Out-File -FilePath $outFile -Append
    $OriginalVM.HardwareProfile.VmSize | Out-File -FilePath $outFile -Append

    "NIC: " | Out-File -FilePath $outFile -Append
    $OriginalVM.NetworkProfile.NetworkInterfaces[0].Id | Out-File -FilePath $outFile -Append

    "OSType: " | Out-File -FilePath $outFile -Append
    $OriginalVM.StorageProfile.OsDisk.OsType | Out-File -FilePath $outFile -Append

    "OS Disk: " | Out-File -FilePath $outFile -Append
    $OriginalVM.StorageProfile.OsDisk.Vhd.Uri | Out-File -FilePath $outFile -Append

    if ($OriginalVM.StorageProfile.DataDisks) {
    "Data Disk(s): " | Out-File -FilePath $outFile -Append
    $OriginalVM.StorageProfile.DataDisks | Out-File -FilePath $outFile -Append
    }

    #Remove the original VM
    Remove-AzureRmVM -ResourceGroupName $rg -Name $vmName

    #Create new availability set if it does not exist
    $availSet = Get-AzureRmAvailabilitySet -ResourceGroupName $rg -Name $newAvailSetName -ErrorAction Ignore
    if (-Not $availSet) {
    $availset = New-AzureRmAvailabilitySet -ResourceGroupName $rg -Name $newAvailSetName -Location $OriginalVM.Location
    }

    #Create the basic configuration for the replacement VM
    $newVM = New-AzureRmVMConfig -VMName $OriginalVM.Name -VMSize $OriginalVM.HardwareProfile.VmSize -AvailabilitySetId $availSet.Id
    Set-AzureRmVMOSDisk -VM $NewVM -VhdUri $OriginalVM.StorageProfile.OsDisk.Vhd.Uri  -Name $OriginalVM.Name -CreateOption Attach -Windows

    #Add Data Disks
    foreach ($disk in $OriginalVM.StorageProfile.DataDisks ) { 
    Add-AzureRmVMDataDisk -VM $newVM -Name $disk.Name -VhdUri $disk.Vhd.Uri -Caching $disk.Caching -Lun $disk.Lun -CreateOption Attach -DiskSizeInGB $disk.DiskSizeGB
    }

    #Add NIC(s)
    foreach ($nic in $OriginalVM.NetworkInterfaceIDs) {
        Add-AzureRmVMNetworkInterface -VM $NewVM -Id $nic
    }

    #Create the VM
    New-AzureRmVM -ResourceGroupName $rg -Location $OriginalVM.Location -VM $NewVM -DisableBginfoExtension
```

## <a name="next-steps"></a>Nächste Schritte

Hinzufügen von weiteren Speicherplatz zu Ihrer virtuellen Computer durch Hinzufügen einer weiteren [Daten Datenträger](virtual-machines-windows-attach-disk-portal.md).


<properties
    pageTitle="Verwalten von virtuellen Computern mit Ressourcenmanager und PowerShell | Microsoft Azure"
    description="Verwalten von virtuellen Computern Azure Ressourcenmanager und PowerShell verwenden."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="davidmu"/>

# <a name="manage-azure-virtual-machines-using-resource-manager-and-powershell"></a>Verwalten von Azure-Computer mithilfe von Ressourcenmanager und PowerShell

## <a name="install-azure-powershell"></a>Installieren von Azure PowerShell
 
Informationen zum Installieren der neuesten Version von Azure PowerShell, Ihr Abonnement auswählen, und melden Sie sich bei Ihrem Konto finden Sie unter [Informationen zum Installieren und konfigurieren Azure PowerShell](../powershell-install-configure.md) .

## <a name="set-variables"></a>Festlegen von Variablen

Alle Befehle im Artikel erfordern den Namen der Ressourcengruppe, in der virtuelle Computer befindet, und den Namen des virtuellen Computers zu verwalten. Ersetzen Sie den Wert von **$rgName** mit dem Namen der Ressourcengruppe, die die virtuellen Computern enthält. Ersetzen Sie den Wert von **$vmName** mit dem Namen der den virtuellen Computer an. Erstellen Sie die Variablen.

    $rgName = "resource-group-name"
    $vmName = "VM-name"

## <a name="display-information-about-a-virtual-machine"></a>Anzeigen von Informationen zu einem virtuellen Computern

Erhalten Sie die virtuellen Computerinformationen.
  
    Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName

Es gibt ungefähr wie im folgenden Beispiel:

    ResourceGroupName        : rg1
    Id                       : /subscriptions/{subscription-id}/resourceGroups/
                               rg1/providers/Microsoft.Compute/virtualMachines/vm1
    Name                     : vm1
    Type                     : Microsoft.Compute/virtualMachines
    Location                 : centralus
    Tags                     : {}
    AvailabilitySetReference : {
                                  "id": "/subscriptions/{subscription-id}/resourceGroups/
                                  rg1/providers/Microsoft.Compute/availabilitySets/av1"
                               }
    Extensions               : []
    HardwareProfile          : {
                                  "vmSize": "Standard_A0"
                               }
    InstanceView             : null
    NetworkProfile           : {
                                  "networkInterfaces": [
                                    {
                                      "properties.primary": null,
                                      "id": "/subscriptions/{subscription-id}/resourceGroups/
                                      rg1/providers/Microsoft.Network/networkInterfaces/nc1"
                                    }
                                  ]
                               }
    OSProfile                : {
                                  "computerName": "vm1",
                                  "adminUsername": "myaccount1",
                                  "adminPassword": null,
                                  "customData": null,
                                  "windowsConfiguration": {
                                    "provisionVMAgent": true,
                                    "enableAutomaticUpdates": true,
                                    "timeZone": null,
                                    "additionalUnattendContents": [],
                                    "winRM": null
                                  },
                                  "linuxConfiguration": null,
                                  "secrets": []
                               }
    Plan                     : null
    ProvisioningState        : Succeeded
    StorageProfile           : {
                                  "imageReference": {
                                    "publisher": "MicrosoftWindowsServer",
                                    "offer": "WindowsServer",
                                    "sku": "2012-R2-Datacenter",
                                    "version": "latest"
                                  },
                                  "osDisk": {
                                    "osType": "Windows",
                                    "encryptionSettings": null,
                                    "name": "osdisk",
                                    "vhd": {
                                      "Uri": "http://sa1.blob.core.windows.net/vhds/osdisk1.vhd"
                                    }
                                    "image": null,
                                    "caching": "ReadWrite",
                                    "createOption": "FromImage"
                                  }
                                  "dataDisks": [],
                               }
    DataDiskNames            : {}
    NetworkInterfaceIDs      : {/subscriptions/{subscription-id}/resourceGroups/
                                rg1/providers/Microsoft.Network/networkInterfaces/nc1}

## <a name="stop-a-virtual-machine"></a>Beenden eines virtuellen Computers

Beenden der laufenden virtuellen Computern an.

    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName

Sie sind zur Bestätigung aufgefordert werden:

    Virtual machine stopping operation
    This cmdlet will stop the specified virtual machine. Do you want to continue?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
        
Geben Sie **Y** zum Beenden des virtuellen Computers.

Nach ein paar Minuten gibt es ungefähr wie im folgenden Beispiel:

    StatusCode : Succeeded
    StartTime  : 9/13/2016 12:11:57 PM
    EndTime    : 9/13/2016 12:14:40 PM

## <a name="start-a-virtual-machine"></a>Starten eines virtuellen Computers

Starten Sie den virtuellen Computer aus, wenn sie abgebrochen wird.

    Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName

Nach ein paar Minuten gibt es ungefähr wie im folgenden Beispiel:

    StatusCode : Succeeded
    StartTime  : 9/13/2016 12:32:55 PM
    EndTime    : 9/13/2016 12:35:09 PM

Wenn Sie möchten Neustarten eines virtuellen Computers, das bereits ausgeführt wird, beschrieben verwenden **Restart-AzureRmVM** weiter aus.

## <a name="restart-a-virtual-machine"></a>Starten eines virtuellen Computers

Starten des laufenden virtuellen Computers erneut.

    Restart-AzureRmVM -ResourceGroupName $rgName -Name $vmName

Es gibt ungefähr wie im folgenden Beispiel:

    StatusCode : Succeeded
    StartTime  : 9/13/2016 12:54:40 PM
    EndTime    : 9/13/2016 12:55:54 PM

## <a name="delete-a-virtual-machine"></a>Löschen eines virtuellen Computers

Löschen des virtuellen Computers.  

    Remove-AzureRmVM -ResourceGroupName $rgName –Name $vmName

> [AZURE.NOTE] Können die **-Force** -Parameter der bestätigungsaufforderung überspringen.

Wenn Sie verwenden den Parameter erzwingen, dass Sie zur Bestätigung aufgefordert sind:

    Virtual machine removal operation
    This cmdlet will remove the specified virtual machine. Do you want to continue?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):

Es gibt ungefähr wie im folgenden Beispiel:

    RequestId  IsSuccessStatusCode  StatusCode  ReasonPhrase
    ---------  -------------------  ----------  ------------
                              True          OK  OK

## <a name="update-a-virtual-machine"></a>Aktualisieren eines virtuellen Computers

In diesem Beispiel wird gezeigt, wie die Größe des virtuellen Computers zu aktualisieren.
        
    $vmSize = "Standard_A1"
    $vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
    $vm.HardwareProfile.vmSize = $vmSize
    Update-AzureRmVM -ResourceGroupName $rgName -VM $vm
    
Es gibt ungefähr wie im folgenden Beispiel:

    RequestId  IsSuccessStatusCode  StatusCode  ReasonPhrase
    ---------  -------------------  ----------  ------------
                              True          OK  OK
                              
Eine Liste der verfügbaren Größen für einen virtuellen Computer finden Sie unter [Größe für virtuelle Computer in Azure](virtual-machines-windows-sizes.md) .

## <a name="add-a-data-disk-to-a-virtual-machine"></a>Fügen Sie einen Datenträger mit einem virtuellen Computer

Dieses Beispiel zeigt, wie Sie einen Datenträger vorhandenen virtuellen Computer hinzufügen.

    $vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
    Add-AzureRmVMDataDisk -VM $vm -Name "disk-name" -VhdUri "https://mystore1.blob.core.windows.net/vhds/datadisk1.vhd" -LUN 0 -Caching ReadWrite -DiskSizeinGB 1 -CreateOption Empty
    Update-AzureRmVM -ResourceGroupName $rgName -VM $vm

Der Datenträger, den Sie hinzufügen, wird keine Initialisierung. Zur Initialisierung des Datenträgers können darauf anmelden und Datenträger Verwaltung verwenden. Wenn Sie WinRM und ein Zertifikat dort installiert, wenn Sie es erstellt haben, können Sie remote PowerShell Initialisierung den Datenträger verwenden. Sie können auch eine benutzerdefiniertes Skript-Erweiterung verwenden: 

    $location = "location-name"
    $scriptName = "script-name"
    $fileName = "script-file-name"
    Set-AzureRmVMCustomScriptExtension -ResourceGroupName $rgName -Location $locName -VMName $vmName -Name $scriptName -TypeHandlerVersion "1.4" -StorageAccountName "mystore1" -StorageAccountKey "primary-key" -FileName $fileName -ContainerName "scripts"

Die Skriptdatei kann ungefähr wie dieser Code der Datenträger Initialisierung enthalten:

    $disks = Get-Disk |   Where partitionstyle -eq 'raw' | sort number

    $letters = 70..89 | ForEach-Object { ([char]$_) }
    $count = 0
    $labels = @("data1","data2")

    foreach($d in $disks) {
        $driveLetter = $letters[$count].ToString()
        $d | 
        Initialize-Disk -PartitionStyle MBR -PassThru |
        New-Partition -UseMaximumSize -DriveLetter $driveLetter |
        Format-Volume -FileSystem NTFS -NewFileSystemLabel $labels[$count] `
            -Confirm:$false -Force 
        $count++
    }

## <a name="next-steps"></a>Nächste Schritte

Wäre es Probleme bei einer-Bereitstellung, können Sie bei der [Problembehandlung Ressource Gruppe Bereitstellungen mit Azure-Portal](../resource-manager-troubleshoot-deployments-portal.md) aussehen.

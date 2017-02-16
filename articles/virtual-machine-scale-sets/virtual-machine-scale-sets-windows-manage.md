<properties
    pageTitle="Verwalten von virtuellen Computern in einer Gruppe von virtuellen Computern skalieren | Microsoft Azure"
    description="Verwalten von virtuellen Computern in einer virtuellen Computern Skala mit Azure PowerShell festgelegt."
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="davidmu"/>

# <a name="manage-virtual-machines-in-a-virtual-machine-scale-set"></a>Verwalten von virtuellen Computern in einer Gruppe von virtuellen Computern skalieren

Verwenden Sie die Aufgaben in diesem Artikel zum Verwalten von virtuellen Computern in Ihrer virtuellen Computern Skalieren festlegen.

Die meisten Aufgaben, die Verwaltung einer virtuellen Computern in einer Gruppe von Farben-Skala betreffen erfordern, dass Sie die Instanz-ID des Computers kennen, die Sie verwalten möchten. [Azure-Explorers](https://resources.azure.com) können Sie um die Instanz-ID eines virtuellen Computers in einer Gruppe von Farben-Skala zu ermitteln. Sie verwenden Sie Resource Explorer zum Überprüfen des Status der Aufgaben, die Sie fertig sind.

Informationen zum Installieren der neuesten Version von Azure PowerShell, Ihr Abonnement auswählen, und melden Sie sich bei Ihrem Konto finden Sie unter [Informationen zum Installieren und konfigurieren Azure PowerShell](../powershell-install-configure.md) .

## <a name="display-information-about-a-scale-set"></a>Anzeigen von Informationen über eine Gruppe von Farben-Skala

Sie können allgemeine Informationen über eine Reihe Maßstab abzurufen, die auch als die Instanzansicht bezeichnet wird. Oder gelangen Sie spezifischere Informationen, wie z. B. Informationen zu den Ressourcen Maßstab festlegen.

Ersetzen Sie die Werte in Anführungszeichen mit dem Namen oder der Ressourcengruppe und Skalierung festlegen, und führen Sie den Befehl aus:

    Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"

Es gibt ungefähr wie folgt aus:

    Id                                          : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/virtualMachineScaleSets/myvmss1
    Name                                        : myvmss1
    Type                                        : Microsoft.Compute/virtualMachineScaleSets
    Location                                    : centralus
    Sku                                         :
      Name                                      : Standard_A0
      Tier                                      : Standard
      Capacity                                  : 3
    UpgradePolicy                               :
      Mode                                      : Manual
    VirtualMachineProfile                       :
      OsProfile                                 :
        ComputerNamePrefix                      : vmss1
        AdminUsername                           : admin1
        WindowsConfiguration                    :
          ProvisionVMAgent                      : True
          EnableAutomaticUpdates                : True
    StorageProfile                              :
      ImageReference                            :
        Publisher                               : MicrosoftWindowsServer
        Offer                                   : WindowsServer
        Sku                                     : 2012-R2-Datacenter
        Version                                 : latest
      OsDisk                                    :
        Name                                    : vmssosdisk
        Caching                                 : ReadOnly
        CreateOption                            : FromImage
        VhdContainers[0]                        : https://astore.blob.core.windows.net/vmss
        VhdContainers[1]                        : https://gstore.blob.core.windows.net/vmss
        VhdContainers[2]                        : https://mstore.blob.core.windows.net/vmss
        VhdContainers[3]                        : https://sstore.blob.core.windows.net/vmss
        VhdContainers[4]                        : https://ystore.blob.core.windows.net/vmss
    NetworkProfile                              :
      NetworkInterfaceConfigurations[0]         :
        Name                                    : mync1
        Primary                                 : True
        IpConfigurations[0]                     :
          Name                                  : ip1
          Subnet                                :
            Id                                  : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Network/virtualNetworks/myvn1/subnets/mysn1
          LoadBalancerBackendAddressPools[0]    :
            Id                                  : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Network/loadBalancers/mylb1/backendAddressPools/bepool1
        LoadBalancerInboundNatPools[0]          :
            Id                                  : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Network/loadBalancers/mylb1/inboundNatPools/natpool1
    ExtensionProfile                            :
      Extensions[0]                             :
        Name                                    : Microsoft.Insights.VMDiagnosticsSettings
        Publisher                               : Microsoft.Azure.Diagnostics
        Type                                    : IaaSDiagnostics
        TypeHandlerVersion                      : 1.5
        AutoUpgradeMinorVersion                 : True
        Settings                                : {"xmlCfg":"...","storageAccount":"astore"}
    ProvisioningState                           : Succeeded
    
Ersetzen Sie die Werte in Anführungszeichen mit dem Namen der Ressource gruppieren und skalieren zurück. Ersetzen Sie *#* mit der Instanz-ID des virtuellen Computers die gewünschten Informationen zu erhalten, und führen Sie diese:

    Get-AzureRmVmssVM -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #
        
Es gibt ungefähr wie im folgenden Beispiel:

    Id                            : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/
                                    virtualMachineScaleSets/myvmss1/virtualMachines/0
    Name                          : myvmss1_0
    Type                          : Microsoft.Compute/virtualMachineScaleSets/virtualMachines
    Location                      : centralus
    InstanceId                    : 0
    Sku                           :
      Name                        : Standard_A0
      Tier                        : Standard
    LatestModelApplied            : True
    StorageProfile                :
      ImageReference              :
        Publisher                 : MicrosoftWindowsServer
        Offer                     : WindowsServer
        Sku                       : 2012-R2-Datacenter
        Version                   : 4.0.20160617
      OsDisk                      :
        OsType                    : Windows
        Name                      : vmssosdisk-os-0-e11cad52959b4b76a8d9f26c5190c4f8
        Vhd                       :
          Uri                     : https://astore.blob.core.windows.net/vmss/vmssosdisk-os-0-e11cad52959b4b76a8d9f26c5190c4f8.vhd
        Caching                   : ReadOnly
        CreateOption              : FromImage
    OsProfile                     :
      ComputerName                : myvmss1-0
      AdminUsername               : admin1
      WindowsConfiguration        :
        ProvisionVMAgent          : True
        EnableAutomaticUpdates    : True
    NetworkProfile                :
      NetworkInterfaces[0]        :
        Id                        : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/virtualMachineScaleSets/
                                    myvmss1/virtualMachines/0/networkInterfaces/mync1
    ProvisioningState             : Succeeded
    Resources[0]                  :
      Id                          : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/virtualMachines/
                                    myvmss1_0/extensions/Microsoft.Insights.VMDiagnosticsSettings
      Name                        : Microsoft.Insights.VMDiagnosticsSettings
      Type                        : Microsoft.Compute/virtualMachines/extensions
      Location                    : centralus
      Publisher                   : Microsoft.Azure.Diagnostics
      VirtualMachineExtensionType : IaaSDiagnostics
      TypeHandlerVersion          : 1.5
      AutoUpgradeMinorVersion     : True
      Settings                    : {"xmlCfg":"...","storageAccount":"astore"}
      ProvisioningState           : Succeeded
        
## <a name="start-a-virtual-machine-in-a-scale-set"></a>Starten eines virtuellen Computers in einer Gruppe von Farben-Skala

Ersetzen Sie die Werte in Anführungszeichen mit den Namen der Ressource gruppieren und Skalierung festlegen. Ersetzen Sie *#* mit dem Bezeichner des virtuellen Computers, die Sie starten, und führen Sie es möchten:

    Start-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

Ressourcen-Explorer können wir angezeigt werden, dass der Status der Instanz **ausgeführt**wird:

    "statuses": [
      {
        "code": "ProvisioningState/succeeded",
        "level": "Info",
        "displayStatus": "Provisioning succeeded",
        "time": "2016-03-15T02:10:08.0730839+00:00"
      },
      {
        "code": "PowerState/running",
        "level": "Info",
        "displayStatus": "VM running"
      }
    ]

Sie können den virtuellen Computern in den Maßstab festlegen, indem Sie nicht den Parameter - InstanceId beginnen.
    
## <a name="stop-a-virtual-machine-in-a-scale-set"></a>Beenden eines virtuellen Computers in einer Gruppe von Farben-Skala

Ersetzen Sie die Werte in Anführungszeichen mit dem Namen der Ressource gruppieren und skalieren zurück. Ersetzen Sie *#* mit dem Bezeichner des virtuellen Computers, die Sie beenden, und führen Sie es möchten:

    Stop-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

Ressourcen-Explorer können wir angezeigt werden, dass der Status der Instanz **freigegeben**ist:

    "statuses": [
      {
        "code": "ProvisioningState/succeeded",
        "level": "Info",
        "displayStatus": "Provisioning succeeded",
        "time": "2016-03-15T01:25:17.8792929+00:00"
      },
      {
        "code": "PowerState/deallocated",
        "level": "Info",
        "displayStatus": "VM deallocated"
      }
    ]
    
Verwenden Sie zum Beenden eines virtuellen Computers, und es nicht frei, den StayProvisioned - Parameter ein. Sie können den virtuellen Computern in der Gruppe beenden, nicht mit dem InstanceId - Parameter.
    
## <a name="restart-a-virtual-machine-in-a-scale-set"></a>Starten eines virtuellen Computers in einer Gruppe von Farben-Skala

Ersetzen Sie die Werte in Anführungszeichen mit dem Namen der Ressourcengruppe und den Maßstab festlegen. Ersetzen Sie *#* mit dem Bezeichner des virtuellen Computers, die Sie neu starten, und führen Sie es möchten:

    Restart-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #
    
Sie können alle virtuellen Computer in der Gruppe neu starten, nicht mit dem InstanceId - Parameter.

## <a name="remove-a-virtual-machine-from-a-scale-set"></a>Entfernen eines virtuellen Computers in einem Satz skalieren

Ersetzen Sie die Werte in Anführungszeichen mit dem Namen der Ressourcengruppe und den Maßstab festlegen. Ersetzen Sie *#* mit dem Bezeichner des virtuellen Computers, die Sie entfernen, und führen Sie es möchten:  

    Remove-AzureRmVmss -ResourceGroupName "resource group name" –VMScaleSetName "scale set name" -InstanceId #

Sie können die virtuellen Computern Skalieren festlegen auf einmal nicht mit dem InstanceId - Parameter entfernen.

## <a name="change-the-capacity-of-a-scale-set"></a>Ändern Sie die Kapazität einer Skala zurück

Sie können hinzufügen oder Entfernen von virtuellen Computern durch Ändern der Speicherkapazität des festlegen. Erhalten Sie den Maßstab festlegen, den Sie ändern, legen Sie die Kapazität, was es sein soll, und aktualisieren Sie die Skalierung für die neue Kapazität festlegen möchten. Ersetzen Sie die Werte in Anführungszeichen in diesen Befehlen mit den Namen der Ressourcengruppe und die Skalierung festlegen.

  $vmss = Get-AzureRmVmss - ResourceGroupName "Gruppe Ressourcenname" - VMScaleSetName "Skalierung setName" $vmss.sku.capacity = 5 Update-AzureRmVmss - ResourceGroupName "Gruppe Ressourcenname"-Name "SetName skalieren" - VirtualMachineScaleSet $vmss 

Wenn Sie virtuellen Computern aus dem Maßstab Satz entfernen, werden zuerst die virtuellen Computer mit den höchsten Ids entfernt.

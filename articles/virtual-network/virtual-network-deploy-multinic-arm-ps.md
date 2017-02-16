<properties
   pageTitle="Bereitstellen von Multi NIC virtuellen Computern mithilfe der PowerShell in Ressourcenmanager | Microsoft Azure"
   description="Weitere Informationen zum Bereitstellen von Multi NIC virtuellen Computern mithilfe der PowerShell in Ressourcenmanager"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/02/2016"
   ms.author="jdial" />

#<a name="deploy-multi-nic-vms-using-powershell"></a>Bereitstellen von Multi NIC virtuellen Computern mithilfe der PowerShell

[AZURE.INCLUDE [virtual-network-deploy-multinic-arm-selectors-include.md](../../includes/virtual-network-deploy-multinic-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-deploy-multinic-intro-include.md](../../includes/virtual-network-deploy-multinic-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][klassische Bereitstellungsmodell](virtual-network-deploy-multinic-classic-ps.md).

[AZURE.INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

Derzeit haben nicht virtueller Computer mit einen einzelnen Netzwerkadapter und virtueller Computer mit mehreren NICs in demselben Satz Verfügbarkeit. Daher müssen Sie die Back-End-Server in einer anderen Ressourcengruppe als andere unsichere Komponenten implementieren. Verwenden Sie die folgenden Schritte aus eine Ressourcengruppe mit dem Namen *IaaSStory* für das Hauptfenster Ressourcengruppe und *IaaSStory-Back-End* für die Back-End-Server aus.

## <a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie die Back-End-Server bereitstellen können, müssen Sie die Hauptfenster Ressourcengruppe mit den notwendigen Ressourcen für dieses Szenario bereitstellen. Um diese Ressourcen bereitstellen zu können, führen Sie die folgenden Schritte aus.

1. Navigieren Sie zu [der Vorlagenseite](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).
2. Klicken Sie in der Vorlagenseite rechts vom **übergeordneten Ressourcengruppe**auf **Bereitstellen in Azure**.
3. Bei Bedarf ändern Sie die gewünschten Parameterwerte, und führen Sie die Schritte im Portal Azure Vorschau die Ressourcengruppe bereitstellen.

> [AZURE.IMPORTANT] Stellen Sie sicher, dass Ihre Speicher Kontonamen eindeutig sind. Sie können keine doppelten Speicher Kontonamen Azure haben.

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="deploy-the-back-end-vms"></a>Bereitstellen der Back-End-virtuellen Computern

Die Back-End-virtuellen Computern abhängig von der Erstellung der unten aufgeführten Ressourcen.

- **Speicher-Konto für Datenfestplatten**. Für eine bessere Leistung werden Festplatten mit den Daten auf dem Datenbankserver einfarbige State Drive (SSD)-Technologie, verwenden, die eine Premium Speicher-Konto erforderlich. Vergewissern Sie sich den Azure Speicherort, die Sie zur Unterstützung von Premium Speicher bereitstellen.
- **NICs**. Jeder virtueller Computer haben die beiden Karten eine Datenbank zugreifen, und eine für die Verwaltung.
- **Verfügbarkeit festzulegen**. Alle Datenbankserver werden Verfügbarkeit einer einzelnen festlegen, um sicherzustellen, dass mindestens eines der virtuellen Computern aktiv ist und ausgeführt werden, während die Wartung hinzugefügt werden.  

### <a name="step-1---start-your-script"></a>Schritt 1 – starten Sie das Skript

Sie können das vollständige PowerShell-Skript zum Herunterladen [können](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/arm/virtual-network-deploy-multinic-arm-ps.ps1). Folgen Sie den Schritten unter So ändern Sie das Skript in Ihrer Umgebung zu arbeiten.

[AZURE.INCLUDE [powershell-preview-include.md](../../includes/powershell-preview-include.md)]

1. Ändern Sie die Werte der Variablen unten basierend auf Ihrer vorhandenen Ressourcengruppe in [Vorkenntnisse](#Prerequisites)über bereitgestellt.

        $existingRGName        = "IaaSStory"
        $location              = "West US"
        $vnetName              = "WTestVNet"
        $backendSubnetName     = "BackEnd"
        $remoteAccessNSGName   = "NSG-RemoteAccess"
        $stdStorageAccountName = "wtestvnetstoragestd"

2. Ändern Sie die Werte der Variablen unten auf der Grundlage der Werte, die Sie für die Back-End-Bereitstellung verwenden möchten.

        $backendRGName         = "IaaSStory-Backend"
        $prmStorageAccountName = "wtestvnetstorageprm"
        $avSetName             = "ASDB"
        $vmSize                = "Standard_DS3"
        $publisher             = "MicrosoftSQLServer"
        $offer                 = "SQL2014SP1-WS2012R2"
        $sku                   = "Standard"
        $version               = "latest"
        $vmNamePrefix          = "DB"
        $osDiskPrefix          = "osdiskdb"
        $dataDiskPrefix        = "datadisk"
        $diskSize              = "120"  
        $nicNamePrefix         = "NICDB"
        $ipAddressPrefix       = "192.168.2."
        $numberOfVMs           = 2

3. Abrufen der vorhandenen Ressourcen für die Bereitstellung erforderlich.

        $vnet                  = Get-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $existingRGName
        $backendSubnet         = $vnet.Subnets|?{$_.Name -eq $backendSubnetName}
        $remoteAccessNSG       = Get-AzureRmNetworkSecurityGroup -Name $remoteAccessNSGName -ResourceGroupName $existingRGName
        $stdStorageAccount     = Get-AzureRmStorageAccount -Name $stdStorageAccountName -ResourceGroupName $existingRGName

### <a name="step-2---create-necessary-resources-for-your-vms"></a>Schritt 2: Erstellen der erforderlichen Ressourcen für Ihre virtuellen Computer

Sie müssen zum Erstellen einer neuen Ressourcengruppe, ein Speicherkonto für Festplatten mit den Daten und eine Verfügbarkeit für alle virtuellen Computern festlegen. Benötigen Sie ebenfalls die Anmeldeinformationen des Kontos Lokaler Administrator, für jeden virtuellen Computer. Führen Sie die folgenden Schritte aus, um diese Ressourcen zu erstellen.

1. Erstellen einer neuen Ressourcengruppe an.

        New-AzureRmResourceGroup -Name $backendRGName -Location $location

2. Erstellen Sie ein neues Premium Speicher-Konto in der oben erstellten Ressourcengruppe aus.

        $prmStorageAccount = New-AzureRmStorageAccount -Name $prmStorageAccountName `
            -ResourceGroupName $backendRGName -Type Premium_LRS -Location $location

3. Erstellen einer neuen Verfügbarkeit festlegen.

        $avSet = New-AzureRmAvailabilitySet -Name $avSetName -ResourceGroupName $backendRGName -Location $location

4. Abrufen des lokalen Administrators Anmeldeinformationen für jeden virtuellen Computer verwendet werden soll.

        $cred = Get-Credential -Message "Type the name and password for the local administrator account."

### <a name="step-3---create-the-nics-and-backend-vms"></a>Schritt 3 – Erstellen Sie die NICs und Back-End-virtuellen Computern

Sie müssen eine Schleife verwenden, um so viele virtuellen Computern, wie Sie möchten, und erstellen die erforderlichen NICs und virtuellen Computern innerhalb der Schleife zu erstellen. Führen Sie die folgenden Schritte aus, um die NICs und virtuellen Computern zu erstellen.

1. Starten einer `for` Schleife, um die Befehle zum Erstellen eines virtuellen Computers und zwei NICs so oft wie erforderlich, wiederholen basierend auf dem Wert, der die `$numberOfVMs` Variable.

        for ($suffixNumber = 1; $suffixNumber -le $numberOfVMs; $suffixNumber++){

2. Erstellen Sie die NIC für den Datenbankzugriff verwendet.

            $nic1Name = $nicNamePrefix + $suffixNumber + "-DA"
            $ipAddress1 = $ipAddressPrefix + ($suffixNumber + 3)
            $nic1 = New-AzureRmNetworkInterface -Name $nic1Name -ResourceGroupName $backendRGName `
                -Location $location -SubnetId $backendSubnet.Id -PrivateIpAddress $ipAddress1

3. Erstellen Sie die NIC für den Remotezugriff verwendet. Beachten Sie, wie diesen Netzwerkadapter eine NSG zugeordnet wurde.

            $nic2Name = $nicNamePrefix + $suffixNumber + "-RA"
            $ipAddress2 = $ipAddressPrefix + (53 + $suffixNumber)
            $nic2 = New-AzureRmNetworkInterface -Name $nic2Name -ResourceGroupName $backendRGName `
                -Location $location -SubnetId $backendSubnet.Id -PrivateIpAddress $ipAddress2 `
                -NetworkSecurityGroupId $remoteAccessNSG.Id

4. Erstellen von `vmConfig` Objekt.

            $vmName = $vmNamePrefix + $suffixNumber
            $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize -AvailabilitySetId $avSet.Id

5. Erstellen Sie zwei Daten Datenträger pro virtueller Computer an. Beachten Sie, dass die Daten in der zuvor erstellten Premium Speicher-Konto sind.

            $dataDisk1Name = $vmName + "-" + $osDiskPrefix + "-1"    
            $data1VhdUri = $prmStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $dataDisk1Name + ".vhd"
            Add-AzureRmVMDataDisk -VM $vmConfig -Name $dataDisk1Name -DiskSizeInGB $diskSize `
                -VhdUri $data1VhdUri -CreateOption empty -Lun 0

            $dataDisk2Name = $vmName + "-" + $dataDiskPrefix + "-2"    
            $data2VhdUri = $prmStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $dataDisk2Name + ".vhd"
            Add-AzureRmVMDataDisk -VM $vmConfig -Name $dataDisk2Name -DiskSizeInGB $diskSize `
                -VhdUri $data2VhdUri -CreateOption empty -Lun 1

6. Konfigurieren des Betriebssystems und Bild, um für den virtuellen Computer verwendet werden.

            $vmConfig = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $vmName -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
            $vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig -PublisherName $publisher -Offer $offer -Skus $sku -Version $version

7. Fügen Sie die beiden Karten über erstellt wurde, in der `vmConfig` Objekt.

            $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic1.Id -Primary
            $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic2.Id

8. Erstellen Sie den Datenträger OS, und erstellen Sie den virtuellen Computer. Hinweis Die `}` endet die `for` Schleife.

            $osDiskName = $vmName + "-" + $osDiskSuffix
            $osVhdUri = $stdStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $osDiskName + ".vhd"
            $vmConfig = Set-AzureRmVMOSDisk -VM $vmConfig -Name $osDiskName -VhdUri $osVhdUri -CreateOption fromImage
            New-AzureRmVM -VM $vmConfig -ResourceGroupName $backendRGName -Location $location
        }

### <a name="step-4---run-the-script"></a>Schritt 4 – Führen Sie das Skript

Jetzt, da Sie heruntergeladen und das Skript entsprechend Ihren Anforderungen geändert, Skripts Runt er zum Back-End Datenbank virtuellen Computern mit mehreren NICs erstellen.

1. Speichern Sie das Skript, und führen Sie sie aus der **PowerShell** -Eingabeaufforderung oder **PowerShell ISE**. Die Ausgabe die anfängliche, sehen, wie unten dargestellt.

        ResourceGroupName : IaaSStory-Backend
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                            Actions  NotActions
                            =======  ==========
                            *                  

        ResourceId        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/IaaSStory-Backend

2. Füllen Sie nach ein paar Minuten die Anmeldeinformationen auffordern, und klicken Sie auf **OK**. Die nachstehende Ausgabe darstellt ein einzelnes virtuellen Computers. Beachten Sie den gesamten Prozess benötigte 8 Minuten.

        ResourceGroupName            :
        Id                           :
        Name                         : DB2
        Type                         :
        Location                     :
        Tags                         :
        TagsText                     : null
        AvailabilitySetReference     : Microsoft.Azure.Management.Compute.Models.AvailabilitySetReference
        AvailabilitySetReferenceText : {
                                         "ReferenceUri": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend/providers/
                                       Microsoft.Compute/availabilitySets/ASDB"
                                       }
        Extensions                   :
        ExtensionsText               : null
        HardwareProfile              : Microsoft.Azure.Management.Compute.Models.HardwareProfile
        HardwareProfileText          : {
                                         "VirtualMachineSize": "Standard_DS3"
                                       }
        InstanceView                 :
        InstanceViewText             : null
        NetworkProfile               :
        NetworkProfileText           : null
        OSProfile                    :
        OSProfileText                : null
        Plan                         :
        PlanText                     : null
        ProvisioningState            :
        StorageProfile               : Microsoft.Azure.Management.Compute.Models.StorageProfile
        StorageProfileText           : {
                                         "DataDisks": [
                                           {
                                             "Lun": 0,
                                             "Caching": null,
                                             "CreateOption": "empty",
                                             "DiskSizeGB": 127,
                                             "Name": "DB2-datadisk-1",
                                             "SourceImage": null,
                                             "VirtualHardDisk": {
                                               "Uri": "https://wtestvnetstorageprm.blob.core.windows.net/vhds/DB2-datadisk-1.vhd"
                                             }
                                           }
                                         ],
                                         "ImageReference": null,
                                         "OSDisk": null
                                       }
        DataDiskNames                : {DB2-datadisk-1}
        NetworkInterfaceIDs          :
        RequestId                    :
        StatusCode                   : 0


        ResourceGroupName            :
        Id                           :
        Name                         : DB2
        Type                         :
        Location                     :
        Tags                         :
        TagsText                     : null
        AvailabilitySetReference     : Microsoft.Azure.Management.Compute.Models.AvailabilitySetReference
        AvailabilitySetReferenceText : {
                                         "ReferenceUri": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend/providers/
                                       Microsoft.Compute/availabilitySets/ASDB"
                                       }
        Extensions                   :
        ExtensionsText               : null
        HardwareProfile              : Microsoft.Azure.Management.Compute.Models.HardwareProfile
        HardwareProfileText          : {
                                         "VirtualMachineSize": "Standard_DS3"
                                       }
        InstanceView                 :
        InstanceViewText             : null
        NetworkProfile               :
        NetworkProfileText           : null
        OSProfile                    :
        OSProfileText                : null
        Plan                         :
        PlanText                     : null
        ProvisioningState            :
        StorageProfile               : Microsoft.Azure.Management.Compute.Models.StorageProfile
        StorageProfileText           : {
                                         "DataDisks": [
                                           {
                                             "Lun": 0,
                                             "Caching": null,
                                             "CreateOption": "empty",
                                             "DiskSizeGB": 127,
                                             "Name": "DB2-datadisk-1",
                                             "SourceImage": null,
                                             "VirtualHardDisk": {
                                               "Uri": "https://wtestvnetstorageprm.blob.core.windows.net/vhds/DB2-datadisk-1.vhd"
                                             }
                                           },
                                           {
                                             "Lun": 1,
                                             "Caching": null,
                                             "CreateOption": "empty",
                                             "DiskSizeGB": 127,
                                             "Name": "DB2-datadisk-2",
                                             "SourceImage": null,
                                             "VirtualHardDisk": {
                                               "Uri": "https://wtestvnetstorageprm.blob.core.windows.net/vhds/DB2-datadisk-2.vhd"
                                             }
                                           }
                                         ],
                                         "ImageReference": null,
                                         "OSDisk": null
                                       }
        DataDiskNames                : {DB2-datadisk-1, DB2-datadisk-2}
        NetworkInterfaceIDs          :
        RequestId                    :
        StatusCode                   : 0


        EndTime             : 10/30/2015 9:30:03 AM -08:00
        Error               :
        Output              :
        StartTime           : 10/30/2015 9:22:54 AM -08:00
        Status              : Succeeded
        TrackingOperationId : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
        RequestId           : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
        StatusCode          : OK

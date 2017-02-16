<properties
    pageTitle="Erstellen einer virtuellen Computern Skalierung festlegen mithilfe der PowerShell | Microsoft Azure"
    description="Erstellen einer virtuellen Computern Skalierung festlegen mithilfe der PowerShell"
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
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="davidmu"/>

# <a name="create-a-windows-virtual-machine-scale-set-using-azure-powershell"></a>Erstellen einer Windows virtuellen Computern Skala mit Azure PowerShell festgelegt

Diese Schritte eines ausfüllen-in-the-Leerzeichen Ansatzes zum Erstellen einer Azure-virtuellen Computern Skalieren festlegen. Finden Sie unter [Übersicht des virtuellen Computern Umfang legt](virtual-machine-scale-sets-overview.md) Weitere Informationen zu skalieren Sätze.

Es sollte ungefähr 30 Minuten dauern, die Schritte in diesem Artikel ausführen.

## <a name="step-1-install-azure-powershell"></a>Schritt 1: Installieren Sie Azure PowerShell

Informationen zum Installieren der neuesten Version von Azure PowerShell, Ihr Abonnement auswählen, und melden Sie sich bei Ihrem Konto finden Sie unter [Informationen zum Installieren und konfigurieren Azure PowerShell](../powershell-install-configure.md) .

## <a name="step-2-create-resources"></a>Schritt 2: Erstellen von Ressourcen

Erstellen Sie die Ressourcen, die für Ihre neue Maßstab erforderlich sind.

### <a name="resource-group"></a>Ressourcengruppe

Ein virtuellen Computern skalieren Satz muss in einer Ressourcengruppe enthalten sein.

1. Erhalten Sie eine Liste der verfügbaren Speicherorte aus, in denen Sie Ressourcen platzieren können:

        Get-AzureLocation | Sort Name | Select Name

2. Wählen Sie einen Speicherort aus, der für Sie am besten geeignet ist, ersetzen Sie den Wert von **$locName** mit dem Namen des Orts, und klicken Sie dann erstellen Sie die Variable:

        $locName = "location name from the list, such as Central US"

3. Ersetzen Sie den Wert von **$rgName** mit dem Namen, den Sie für die neue Ressourcengruppe verwenden, und klicken Sie dann die Variable erstellen möchten: 

        $rgName = "resource group name"
        
4. Erstellen der Ressourcengruppe an:
    
        New-AzureRmResourceGroup -Name $rgName -Location $locName

    Sie sollten ungefähr wie im folgenden Beispiel sehen:

        ResourceGroupName : myrg1
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/########-####-####-####-############/resourceGroups/myrg1

### <a name="storage-account"></a>Speicher-Konto

Ein Speicherkonto eines virtuellen Computers dient zum Speichern der Datenträger Betriebssystem und Diagnose für Skalierung verwendeten Daten. Wenn möglich, empfiehlt es sich um bewährte Methode ein Speicherkonto für jeden virtuellen Computer in einer Gruppe von Farben-Skala erstellt haben. Wenn dies nicht möglich, Planen der Verwendung von nicht mehr als 20 virtuellen Computern pro Storage-Konto. Das Beispiel in diesem Artikel zeigt drei Speicherkonten für drei virtuellen Computern erstellt wird.

1. Ersetzen Sie den Wert von **$saName** mit einen Namen für den Speicherkonto an. Testen Sie den Namen für die Eindeutigkeit. 

        $saName = "storage account name"
        Get-AzureRmStorageAccountNameAvailability $saName

    Wenn die Antwort **Wahr**ist, ist der vorgeschlagenen Namen eindeutig.

3. Ersetzen Sie den Wert von **$saType** mit dem Typ des Speicherkontos, und klicken Sie dann erstellen Sie die Variable:  

        $saType = "storage account type"
        
    Mögliche Werte sind: Standard_LRS, Standard_GRS, Standard_RAGRS oder Premium_LRS.
        
4. Erstellen Sie das Konto ein:
    
        New-AzureRmStorageAccount -Name $saName -ResourceGroupName $rgName –Type $saType -Location $locName

    Sie sollten ungefähr wie im folgenden Beispiel sehen:

        ResourceGroupName   : myrg1
        StorageAccountName  : myst1
        Id                  : /subscriptions/########-####-####-####-############/resourceGroups/myrg1/providers/Microsoft
                              .Storage/storageAccounts/myst1
        Location            : centralus
        AccountType         : StandardLRS
        CreationTime        : 3/15/2016 4:51:52 PM
        CustomDomain        :
        LastGeoFailoverTime :
        PrimaryEndpoints    : Microsoft.Azure.Management.Storage.Models.Endpoints
        PrimaryLocation     : centralus
        ProvisioningState   : Succeeded
        SecondaryEndpoints  :
        SecondaryLocation   :
        StatusOfPrimary     : Available
        StatusOfSecondary   :
        Tags                : {}
        Context             : Microsoft.WindowsAzure.Commands.Common.Storage.AzureStorageContext

5. Wiederholen Sie die Schritte 1 bis 4, um die drei Speicherkonten, zum Beispiel myst1, myst2 und myst3 zu erstellen.

### <a name="virtual-network"></a>Virtuelles Netzwerk

Ein virtuelles Netzwerk ist erforderlich für die virtuellen Computer Maßstab festlegen.

1. Ersetzen Sie den Wert von **$subnetName** mit dem Namen, den Sie für das Subnetz in das virtuelle Netzwerk verwenden, und klicken Sie dann die Variable erstellen möchten: 

        $subnetName = "subnet name"
        
2. Die Subnetzkonfiguration zu erstellen:
    
        $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
        
    Das Adresspräfix kann in Ihrem Netzwerk virtuelle abweichen.

3. Ersetzen Sie den Wert von **$netName** mit dem Namen, den Sie für das virtuelle Netzwerk verwenden, und klicken Sie dann die Variable erstellen möchten: 

        $netName = "virtual network name"
        
4. Das virtuelle Netzwerk zu erstellen:
    
        $vnet = New-AzureRmVirtualNetwork -Name $netName -ResourceGroupName $rgName -Location $locName -AddressPrefix 10.0.0.0/16 -Subnet $subnet

### <a name="configuration-of-the-scale-set"></a>Konfiguration von der Skalierung festlegen

Sie müssen alle Ressourcen, die Sie benötigen für die Skalierung der Konfiguration festlegen, also zu erstellen.  

1. Ersetzen Sie den Wert von **$ipName** mit dem Namen, den Sie für die IP-Konfiguration verwenden, und klicken Sie dann die Variable erstellen möchten: 

        $ipName = "IP configuration name"
        
2. Erstellen Sie die IP-Konfiguration:

        $ipConfig = New-AzureRmVmssIpConfig -Name $ipName -LoadBalancerBackendAddressPoolsId $null -SubnetId $vnet.Subnets[0].Id

2. Ersetzen Sie den Wert von **$vmssConfig** mit dem Namen, den Sie verwenden möchten, verwenden Sie für die Konfiguration der Skalierung festlegen und die Variable erstellen:   

        $vmssConfig = "Scale set configuration name"
        
3. Erstellen Sie die Konfiguration für die Skalierung festlegen:

        $vmss = New-AzureRmVmssConfig -Location $locName -SkuCapacity 3 -SkuName "Standard_A0" -UpgradePolicyMode "manual"
        
    Dieses Beispiel zeigt, dass ein Maßstab festgelegt mit drei virtuellen Computern erstellt wird. Finden Sie unter [Virtuellen Computern Maßstab legt Überblick](virtual-machine-scale-sets-overview.md) mehr über die Kapazität von Sätzen skalieren. Dieses Schritts beinhaltet auch das Festlegen der Größe (SkuName genannt) den virtuellen Computern festlegen. Um eine Größe zu finden, die Ihren Anforderungen entspricht, prüfen Sie [Größen für virtuellen Computern](../virtual-machines/virtual-machines-windows-sizes.md)aus.
    
4. Hinzufügen von Benutzeroberflächen-Netzwerkkonfiguration an der Konfiguration der Skalierung festlegen:
        
        Add-AzureRmVmssNetworkInterfaceConfiguration -VirtualMachineScaleSet $vmss -Name $vmssConfig -Primary $true -IPConfiguration $ipConfig
        
    Sie sollten ungefähr wie im folgenden Beispiel sehen:

        Sku                   : Microsoft.Azure.Management.Compute.Models.Sku
        UpgradePolicy         : Microsoft.Azure.Management.Compute.Models.UpgradePolicy
        VirtualMachineProfile : Microsoft.Azure.Management.Compute.Models.VirtualMachineScaleSetVMProfile
        ProvisioningState     :
        OverProvision         :
        Id                    :
        Name                  :
        Type                  :
        Location              : Central US
        Tags                  :

#### <a name="operating-system-profile"></a>Betriebssystemprofil

1. Ersetzen Sie den Wert von **$computerName** mit dem Präfix der Computer-Namen, das Sie verwenden, und klicken Sie dann die Variable erstellen möchten: 

        $computerName = "computer name prefix"
        
2. Ersetzen Sie den Wert der **$adminName** der Name des Administratorkontos auf den virtuellen Computern, und klicken Sie dann erstellen Sie die Variable:

        $adminName = "administrator account name"
        
3. Ersetzen Sie den Wert von **$adminPassword** mit dem Kennwort für das Konto, und erstellen Sie dann die Variable:

        $adminPassword = "password for administrator accounts"
        
4. Erstellen Sie das Betriebssystemprofil an:

        Set-AzureRmVmssOsProfile -VirtualMachineScaleSet $vmss -ComputerNamePrefix $computerName -AdminUsername $adminName -AdminPassword $adminPassword

#### <a name="storage-profile"></a>Speicher-Profil

1. Ersetzen Sie den Wert von **$storageProfile** mit dem Namen, den Sie für das Profil Speicher verwenden, und klicken Sie dann die Variable erstellen möchten:  

        $storageProfile = "storage profile name"
        
2. Erstellen Sie die Variablen, die das zu verwendende Bild definieren:  
      
        $imagePublisher = "MicrosoftWindowsServer"
        $imageOffer = "WindowsServer"
        $imageSku = "2012-R2-Datacenter"
        
    Um finden die Informationen zu anderen Bildern zu verwenden, prüfen Sie [Navigieren und select Azure-virtuellen Computern Bilder mit Windows PowerShell und Azure CLI](../virtual-machines/virtual-machines-windows-cli-ps-findimage.md).
        
3. Ersetzen Sie den Wert von **$vhdContainers** mit einer Liste, die die Pfade enthält, in dem die virtuellen Festplatten gespeichert sind, wie etwa "https://mystorage.blob.core.windows.net/vhds", und klicken Sie dann erstellen Sie die Variable:
       
        $vhdContainers = @("https://myst1.blob.core.windows.net/vhds","https://myst2.blob.core.windows.net/vhds","https://myst3.blob.core.windows.net/vhds")
        
4. Erstellen Sie das Profil Speicher:

        Set-AzureRmVmssStorageProfile -VirtualMachineScaleSet $vmss -ImageReferencePublisher $imagePublisher -ImageReferenceOffer $imageOffer -ImageReferenceSku $imageSku -ImageReferenceVersion "latest" -Name $storageProfile -VhdContainer $vhdContainers -OsDiskCreateOption "FromImage" -OsDiskCaching "None"  

### <a name="virtual-machine-scale-set"></a>Festlegen von virtuellen Computern skalieren

Schließlich können Sie den Maßstab Satz erstellen.

1. Ersetzen Sie den Wert von **$vmssName** mit dem Namen des virtuellen Computers Maßstab festlegen, und erstellen Sie dann die Variable:

        $vmssName = "scale set name"
        
2. Erstellen Sie den Maßstab festlegen:

        New-AzureRmVmss -ResourceGroupName $rgName -Name $vmssName -VirtualMachineScaleSet $vmss

    Sie sollte ungefähr wie in diesem Beispiel wird angezeigt, die eine erfolgreiche Bereitstellung anzeigt:

        Sku                   : Microsoft.Azure.Management.Compute.Models.Sku
        UpgradePolicy         : Microsoft.Azure.Management.Compute.Models.UpgradePolicy
        VirtualMachineProfile : Microsoft.Azure.Management.Compute.Models.VirtualMachineScaleSetVMProfile
        ProvisioningState     : Updating
        OverProvision         :
        Id                    : /subscriptions/########-####-####-####-############/resourceGroups/myrg1/providers/Microso
                                ft.Compute/virtualMachineScaleSets/myvmss1
        Name                  : myvmss1
        Type                  : Microsoft.Compute/virtualMachineScaleSets
        Location              : centralus
        Tags                  :

## <a name="step-3-explore-resources"></a>Schritt 3: Untersuchen von Ressourcen

Verwenden Sie diese Ressourcen, um die Menge des virtuellen Computers Maßstab untersuchen, die Sie erstellt haben:

- Azure-Portal - steht eine begrenzte Menge von Informationen über das Portal.
- [Azure Ressource Explorer](https://resources.azure.com/) - dieses Tool ist das beste für untersuchen den aktuellen Status der Skalierung zurück.
- Azure PowerShell - verwenden Sie diesen Befehl, um Informationen zu erhalten:

        Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"
        
        Or 
        
        Get-AzureRmVmssVM -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"
        

## <a name="next-steps"></a>Nächste Schritte

- Verwalten von Maßstab festlegen, dass die soeben erstellte mit den Informationen im [Verwalten von virtuellen Computern in einer virtuellen Computern Skalierung festlegen.](virtual-machine-scale-sets-windows-manage.md)
- Erwägen Sie das Einrichten der automatischen Skalierung der Ihren Maßstab festlegen, indem Sie Informationen in die [Automatische Skalierung und virtuellen Computern Maßstab legt fest](virtual-machine-scale-sets-autoscale-overview.md)
- Erfahren Sie mehr über die vertikale Skalierung durch [vertikale Skalieren mit Skalierung des virtuellen Computers](virtual-machine-scale-sets-vertical-scale-reprovision.md) überprüfen

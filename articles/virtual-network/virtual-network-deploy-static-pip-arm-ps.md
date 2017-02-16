<properties 
   pageTitle="Bereitstellen ein virtuellen Computers mit einer statischen öffentliche IP-Adresse mithilfe der PowerShell in Ressourcenmanager | Microsoft Azure"
   description="Weitere Informationen zum Bereitstellen virtueller Computer mit einer statischen öffentliche IP-Adresse mithilfe der PowerShell in Ressourcenmanager"
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
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="deploy-a-vm-with-a-static-public-ip-using-powershell"></a>Bereitstellen eines virtuellen Computers mit einer statischen öffentliche IP-Adresse mithilfe der PowerShell

[AZURE.INCLUDE [virtual-network-deploy-static-pip-arm-selectors-include.md](../../includes/virtual-network-deploy-static-pip-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)]Klassische Bereitstellungsmodell.

[AZURE.INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="step-1---start-your-script"></a>Schritt 1 – starten Sie das Skript

Sie können das vollständige PowerShell-Skript zum Herunterladen [können](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/virtual-network-deploy-static-pip-arm-ps.ps1). Folgen Sie den Schritten unter So ändern Sie das Skript in Ihrer Umgebung zu arbeiten.

1. Ändern Sie die Werte der Variablen unten auf der Grundlage der Werte, die Sie für die Bereitstellung verwenden möchten. Die Werte unter Karte mit dem Szenario in diesem Dokument verwendet werden soll.

        # Set variables resource group
        $rgName                = "IaaSStory"
        $location              = "West US"
        
        # Set variables for VNet
        $vnetName              = "WTestVNet"
        $vnetPrefix            = "192.168.0.0/16"
        $subnetName            = "FrontEnd"
        $subnetPrefix          = "192.168.1.0/24"
        
        # Set variables for storage
        $stdStorageAccountName = "iaasstorystorage"
        
        # Set variables for VM
        $vmSize                = "Standard_A1"
        $diskSize              = 127
        $publisher             = "MicrosoftWindowsServer"
        $offer                 = "WindowsServer"
        $sku                   = "2012-R2-Datacenter"
        $version               = "latest"
        $vmName                = "WEB1"
        $osDiskName            = "osdisk"
        $nicName               = "NICWEB1"
        $privateIPAddress      = "192.168.1.101"
        $pipName               = "PIPWEB1"
        $dnsName               = "iaasstoryws1"

## <a name="step-2---create-the-necessary-resources-for-your-vm"></a>Schritt 2: Erstellen der erforderlichen Ressourcen für Ihre virtuellen Computer

Vor dem Erstellen eines virtuellen Computers, benötigen Sie eine Ressourcengruppe, VNet, öffentliche IP-Adresse und NIC, von dem virtuellen Computer verwendet werden soll.

1. Erstellen einer neuen Ressourcengruppe an.

        New-AzureRmResourceGroup -Name $rgName -Location $location
        
2. Erstellen Sie die VNet und Subnetz.

        $vnet = New-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName `
            -AddressPrefix $vnetPrefix -Location $location   
        
        Add-AzureRmVirtualNetworkSubnetConfig -Name $subnetName `
            -VirtualNetwork $vnet -AddressPrefix $subnetPrefix
        
        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet 

3. Erstellen der öffentlichen IP-Ressource an. 

        $pip = New-AzureRmPublicIpAddress -Name $pipName -ResourceGroupName $rgName `
            -AllocationMethod Static -DomainNameLabel $dnsName -Location $location

4. Erstellen der Schnittstelle (NIC) für den virtuellen Computer in der obigen erstellt, mit der öffentlichen IP-Subnetz an. Beachten Sie das erste Cmdlet Abrufen der VNet aus Azure, dies ist erforderlich, da ein **Set-AzureRmVirtualNetwork** ausgeführt wurde, um die vorhandenen VNet zu ändern.

        $vnet = Get-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName
        $subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name $subnetName
        $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName `
            -Subnet $subnet -Location $location -PrivateIpAddress $privateIPAddress `
            -PublicIpAddress $pip

5. Erstellen Sie ein Speicherkonto, um das Laufwerk virtueller Computer OS hosten.

        $stdStorageAccount = New-AzureRmStorageAccount -Name $stdStorageAccountName `
            -ResourceGroupName $rgName -Type Standard_LRS -Location $location

## <a name="step-3---create-the-vm"></a>Schritt 3 – Erstellen Sie den virtuellen Computer 

Jetzt, dass alle notwendigen Ressourcen vorhanden sind, können Sie einen neuen virtuellen Computer erstellen.

1. Erstellen Sie das Konfigurationsobjekt für den virtuellen Computer an.

        $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize 

1. Abrufen von Anmeldeinformationen für das lokale Administratorkonto virtueller Computer.

        $cred = Get-Credential -Message "Type the name and password for the local administrator account."

2. Erstellen Sie ein virtueller Computer Konfigurationsobjekt ein.

        $vmConfig = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $vmName `
            -Credential $cred -ProvisionVMAgent -EnableAutoUpdate

3. Legen Sie das Bild: das Betriebssystem für den virtuellen Computer an.

        $vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig -PublisherName $publisher `
            -Offer $offer -Skus $sku -Version $version

4. Konfigurieren des Datenträgers OS.

        $osVhdUri = $stdStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $osDiskName + ".vhd"
        $vmConfig = Set-AzureRmVMOSDisk -VM $vmConfig -Name $osDiskName -VhdUri $osVhdUri -CreateOption fromImage

5. Fügen Sie die NIC auf dem virtuellen Computer an.

        $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id -Primary

6. Erstellen Sie den virtuellen Computer an.

        New-AzureRmVM -VM $vmConfig -ResourceGroupName $rgName -Location $location

7. Speichern Sie die Skriptdatei ein.

## <a name="step-4---run-the-script"></a>Schritt 4 – Führen Sie das Skript

Nach dem notwendigen Änderungen vorgenommen werden, und das Skript Grundlegendes über anzuzeigen Sie, führen Sie das Skript. 

1. Führen Sie aus einer PowerShell-Konsole oder PowerShell ISE das Skript oben aus.
2. Die nachstehende Ausgabe sollte nach ein paar Minuten angezeigt werden.

        ResourceGroupName : IaaSStory
        Location          : westus
        ProvisioningState : Succeeded
        Tags              : 
        ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory
                
        AddressSpace      : Microsoft.Azure.Commands.Network.Models.PSAddressSpace
        DhcpOptions       : Microsoft.Azure.Commands.Network.Models.PSDhcpOptions
        Subnets           : {FrontEnd}
        ProvisioningState : Succeeded
        AddressSpaceText  : {
                              "AddressPrefixes": [
                                "192.168.0.0/16"
                              ]
                            }
        DhcpOptionsText   : {}
        SubnetsText       : [
                              {
                                "Name": "FrontEnd",
                                "AddressPrefix": "192.168.1.0/24"
                              }
                            ]
        ResourceGroupName : IaaSStory
        Location          : westus
        ResourceGuid      : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
        Tag               : {}
        TagsTable         : 
        Name              : WTestVNet
        Etag              : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        Id                : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory/providers/Microsoft.Network/virtualNetworks/WTestVNet
        
        AddressSpace      : Microsoft.Azure.Commands.Network.Models.PSAddressSpace
        DhcpOptions       : Microsoft.Azure.Commands.Network.Models.PSDhcpOptions
        Subnets           : {FrontEnd}
        ProvisioningState : Succeeded
        AddressSpaceText  : {
                              "AddressPrefixes": [
                                "192.168.0.0/16"
                              ]
                            }
        DhcpOptionsText   : {
                              "DnsServers": []
                            }
        SubnetsText       : [
                              {
                                "Name": "FrontEnd",
                                "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory/providers/Microsoft.Network/virtualNetworks/WTestVNet/subnets/FrontEnd",
                                "AddressPrefix": "192.168.1.0/24",
                                "IpConfigurations": [],
                                "ProvisioningState": "Succeeded"
                              }
                            ]
        ResourceGroupName : IaaSStory
        Location          : westus
        ResourceGuid      : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
        Tag               : {}
        TagsTable         : 
        Name              : WTestVNet
        Etag              : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        Id                : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory/providers/Microsoft.Network/virtualNetworks/WTestVNet
                
        TrackingOperationId : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
        RequestId           : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
        Status              : Succeeded
        StatusCode          : OK
        Output              : 
        StartTime           : 1/12/2016 12:57:56 PM -08:00
        EndTime             : 1/12/2016 12:59:13 PM -08:00
        Error               : 
        ErrorText           : 

   
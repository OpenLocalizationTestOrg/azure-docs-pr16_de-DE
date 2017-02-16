<properties
    pageTitle="Erstellen von virtuellen Computer aus einer GRG virtuellen | Microsoft Azure"
    description="Informationen Sie zum Erstellen von eines Windows-Computers aus einem GRG virtuelle Festplattenabbild mithilfe der PowerShell Azure im Bereitstellungsmodell Ressourcenmanager."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="cynthn"/>

# <a name="create-a-vm-from-a-generalized-vhd-image"></a>Erstellen eines virtuellen Computers aus einem GRG virtuelle Festplattenabbild

Ein GRG virtuelle Festplattenabbild wurden alle Ihre persönlichen Kontoinformationen mit [Sysprep](virtual-machines-windows-generalize-vhd.md)entfernt. Sie können eine GRG virtuelle Festplatte durch Ausführen von Sysprep einer lokalen virtuellen Computers, klicken Sie dann [die virtuelle Festplatte in Azure hochladen](virtual-machines-windows-upload-image.md), oder durch Ausführen von Sysprep auf einer vorhandenen Azure-virtuellen Computer und dann auf [kopieren die virtuelle Festplatte](virtual-machines-windows-vhd-copy.md)erstellen.

Wenn Sie einen virtuellen Computer aus eine spezialisierte virtuelle Festplatte erstellen möchten, finden Sie unter [Erstellen eines virtuellen Computers aus einer bestimmten virtuellen](virtual-machines-windows-create-vm-specialized.md).

Die schnellste Möglichkeit zum Erstellen eines virtuellen Computers aus einer GRG virtuellen besteht darin, eine [Schnellstart Vorlage] verwenden (https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image). 


## <a name="prerequisites"></a>Erforderliche Komponenten

Wenn Sie beabsichtigen, verwenden Sie eine virtuelle Festplatte hochgeladen aus einer lokalen virtueller Computer, wie eine erstellt Hyper-V, stellen Sie sicher, folgen Sie die Anweisungen unter [Vorbereiten einer virtuellen Windows Azure hochladen](virtual-machines-windows-prepare-for-upload-vhd-image.md). 

Sowohl hochgeladene virtuelle Festplatten und vorhandenen virtuellen Festplatten Azure-virtuellen Computer müssen vor der Erstellung eines virtuellen Computers GRG werden bei dieser Methode können. Weitere Informationen finden Sie unter [verallgemeinern einem Windows-Computer mithilfe von Sysprep](virtual-machines-windows-generalize-vhd.md). 


## <a name="set-the-uri-of-the-vhd"></a>Festlegen Sie den URI der virtuellen Festplatte

Der URI für die virtuelle Festplatte verwendet wird im Format: https://**Mystorageaccount**.blob.core.windows.net/**Mycontainer**/**MyVhdName**VHD. In diesem Beispiel die virtuelle Festplatte mit dem Namen **MyVHD** ist im Speicher Konto **Mystorageaccount** in den Container **Mycontainer**.

```powershell
$imageURI = "https://mystorageaccount.blob.core.windows.net/mycontainer/myVhd.vhd"
```


## <a name="create-a-virtual-network"></a>Erstellen Sie ein virtuelles Netzwerk

Erstellen Sie die vNet und Subnetz des [virtuellen Netzwerk](../virtual-network/virtual-networks-overview.md).


1. Erstellen Sie das Subnetz ein. Im folgende Beispiel wird ein Subnetz mit dem Namen **MySubnet** in der Ressource Gruppe **MyResourceGroup** mit dem Adresspräfix **10.0.0.0/24**erstellt.  

    ```powershell
    $rgName = "myResourceGroup"
    $subnetName = "mySubnet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
      
2. Erstellen Sie das virtuelle Netzwerk an. Im folgende Beispiel wird ein virtuelles Netzwerk mit dem Namen **MyVnet** **Westen US** Speicherort mit dem Adresspräfix der **10.0.0.0/16**erstellt.  

    ```powershell
    $location = "West US"
    $vnetName = "myVnet"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    
            
## <a name="create-a-public-ip-address-and-network-interface"></a>Erstellen Sie eine öffentliche IP-Adresse und Netzwerk-Schnittstelle

Klicken Sie zum Aktivieren der Kommunikation mit den virtuellen Computern in das virtuelle Netzwerk benötigen Sie eine [öffentliche IP-Adresse](../virtual-network/virtual-network-ip-addresses-overview-arm.md) und eine Netzwerk-Benutzeroberfläche an.

1. Erstellen Sie eine öffentliche IP-Adresse ein. In diesem Beispiel wird eine öffentliche IP-Adresse mit dem Namen **MyPip**erstellt. 

    ```powershell
    $ipName = "myPip"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location -AllocationMethod Dynamic
    ```       

2. Erstellen der NIC. In diesem Beispiel erstellt einen Netzwerkadapter mit dem Namen **myNic**. 

    ```powershell
    $nicName = "myNic"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

## <a name="create-the-network-security-group-and-an-rdp-rule"></a>Erstellen der Netzwerksicherheitsgruppe und eine Regel RDP

Um sich bei Ihrem virtuellen Computer mit RDP anmelden können, müssen Sie eine Sicherheitsregel für die haben, die RDP Zugriff auf Port 3389 zulässt. 

In diesem Beispiel erstellt eine NSG mit dem Namen **MyNsg** , die enthält eine Regel mit der Bezeichnung **MyRdpRule** , die RDP Verkehr über Port 3389 zulässt. Weitere Informationen zu NSGs finden Sie unter [Öffnen von Ports eines virtuellen Computers in Azure mithilfe der PowerShell](virtual-machines-windows-nsg-quickstart-powershell.md).

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```


## <a name="create-a-variable-for-the-virtual-network"></a>Erstellen Sie eine Variable für das virtuelle Netzwerk

Erstellen Sie eine Variable für das abgeschlossene virtuelle Netzwerk ein. 

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName
```

## <a name="create-the-vm"></a>Erstellen Sie den virtuellen Computer

Das folgende PowerShell-Skript gezeigt, wie die virtuellen Computerkonfigurationen einrichten und verwenden das hochgeladene virtueller Computer Bild als Quelle für die neue Installation.

</br>


```powershell
    # Enter a new user name and password to use as the local administrator account 
    # for remotely accessing the VM.
    $cred = Get-Credential
    
    # Name of the storage account where the VHD is located. This example sets the 
    # storage account name as "myStorageAccount"
    $storageAccName = "myStorageAccount"
    
    # Name of the virtual machine. This example sets the VM name as "myVM".
    $vmName = "myVM"
    
    # Size of the virtual machine. This example creates "Standard_D2_v2" sized VM. 
    # See the VM sizes documentation for more information: 
    # https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/
    $vmSize = "Standard_D2_v2"
    
    # Computer name for the VM. This examples sets the computer name as "myComputer".
    $computerName = "myComputer"
    
    # Name of the disk that holds the OS. This example sets the 
    # OS disk name as "myOsDisk"
    $osDiskName = "myOsDisk"
    
    # Assign a SKU name. This example sets the SKU name as "Standard_LRS"
    # Valid values for -SkuName are: Standard_LRS - locally redundant storage, Standard_ZRS - zone redundant storage, Standard_GRS - geo redundant storage, Standard_RAGRS - read access geo redundant storage, Premium_LRS - premium locally redundant storage. 
    $skuName = "Standard_LRS"
    
    # Get the storage account where the uploaded image is stored
    $storageAcc = Get-AzureRmStorageAccount -ResourceGroupName $rgName -AccountName $storageAccName
    
    # Set the VM name and size
    $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize
    
    #Set the Windows operating system configuration and add the NIC
    $vm = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $computerName `
        -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
    
    # Create the OS disk URI
    $osDiskUri = '{0}vhds/{1}-{2}.vhd' `
        -f $storageAcc.PrimaryEndpoints.Blob.ToString(), $vmName.ToLower(), $osDiskName
    
    # Configure the OS disk to be created from the existing VHD image (-CreateOption fromImage).
    $vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri `
        -CreateOption fromImage -SourceImageUri $imageURI -Windows
    
    # Create the new VM
    New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vm
```

## <a name="verify-that-the-vm-was-created"></a>Stellen Sie sicher, dass der virtuellen Computer erstellt wurde 

Wenn Sie fertig sind, sollten Sie finden Sie unter den neu erstellten virtuellen Computer im [Azure-Portal](https://portal.azure.com) unter **Durchsuchen** > **virtuellen Computern**, oder indem Sie die folgenden PowerShell Befehle verwenden:

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a>Nächste Schritte

Zum Verwalten Ihrer neuen virtuellen Computern mit Azure PowerShell finden Sie unter [Verwalten von virtuellen Computern Azure Ressourcenmanager und PowerShell verwenden](virtual-machines-windows-ps-manage.md).



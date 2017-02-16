<properties
    pageTitle="Erstellen Sie eine Kopie Ihrer Windows virtueller Computer | Microsoft Azure"
    description="Informationen Sie zum Erstellen einer Kopie Ihrer spezialisierte Azure virtueller Computer unter Windows, in dem Modell zur Bereitstellung von Ressourcenmanager."
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
    ms.date="09/21/2016"
    ms.author="cynthn"/>

# <a name="create-a-vm-from-a-specialized-vhd"></a>Erstellen eines virtuellen Computers aus einer bestimmten virtuellen

Erstellen eines neuen virtuellen Computers durch eine spezielle virtuelle Festplatte wie des OS Datenträgers mithilfe der Powershell Anfügen an. Eine spezielle virtuelle Festplatte verwaltet die Benutzerkonten, Anwendungen und anderen Zustandsdaten aus Ihrem ursprünglichen virtuellen. 

Wenn Sie einen virtuellen Computer aus einer GRG virtuellen erstellen möchten, finden Sie unter [Erstellen eines virtuellen Computers aus einem GRG virtuelle Festplattenabbild](virtual-machines-windows-create-vm-generalized.md).

## <a name="create-the-subnet-and-vnet"></a>Erstellen Sie die Subnetz und vNet

Erstellen der vNet und Subnetz des [virtuellen Netzwerks](../virtual-network/virtual-networks-overview.md).

1. Erstellen Sie das Subnetz ein. In diesem Beispiel erstellt ein Subnetz mit dem Namen **MySubNet**in der Ressource Gruppe **MyResourceGroup**, und das Präfix Subnetz Adresse auf **10.0.0.0/24**.

    ```powershell
    $rgName = "myResourceGroup"
    $subnetName = "mySubNet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```

2. Erstellen der vNet. In diesem Beispiel wird den virtuellen Netzwerknamen **MyVnetName**, den Speicherort für **Westen US**und das Adresspräfix für das virtuelle Netzwerk zu **10.0.0.0/16**sein. 

    ```powershell
    $location = "West US"
    $vnetName = "myVnetName"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    
            
## <a name="create-a-public-ip-address-and-nic"></a>Erstellen Sie eine öffentliche IP-Adresse und NIC

Klicken Sie zum Aktivieren der Kommunikation mit den virtuellen Computern in das virtuelle Netzwerk benötigen Sie eine [öffentliche IP-Adresse](../virtual-network/virtual-network-ip-addresses-overview-arm.md) und eine Netzwerk-Benutzeroberfläche an.

1. Erstellen Sie die öffentliche IP-Adresse ein. In diesem Beispiel wird der Name der öffentliche IP-Adresse auf **MyIP**festgelegt.

    ```powershell
    $ipName = "myIP"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location -AllocationMethod Dynamic
    ```       

2. Erstellen der NIC. In diesem Beispiel wird der Namen auf **MyNicName**festgelegt.

    ```powershell
    $nicName = "myNicName"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

## <a name="create-the-network-security-group-and-an-rdp-rule"></a>Erstellen der Netzwerksicherheitsgruppe und eine Regel RDP

Um sich bei Ihrem virtuellen Computer mit RDP anmelden können, müssen Sie eine Sicherheitsregel haben, die RDP Zugriff auf Port 3389 zulässt. Da die virtuelle Festplatte für den neuen virtuellen Computer aus einer vorhandenen erstellt wurde können spezielle virtueller Computer, nachdem Sie der virtuellen Computer erstellt wurde ein vorhandenes Kontos aus der Quelle virtuellen Computern, die über die Berechtigung zum Anmelden mit RDP aufwiesen.

In diesem Beispiel wird der Name NSG **MyNsg** und der Regelname RDP zu **MyRdpRule**.

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```

Weitere Informationen über Endpunkte und NSG Regeln finden Sie unter [Öffnen von Ports eines virtuellen Computers in Azure mithilfe der PowerShell](virtual-machines-windows-nsg-quickstart-powershell.md).

## <a name="create-the-vm-configuration"></a>Erstellen der Konfigurations virtueller Computer

Richten Sie die Konfiguration virtueller Computer aus, zu die kopierte virtuellen Festplatte als die OS virtuelle Festplatte anfügen.


```powershell
    # Set the URI for the VHD that you want to use. In this example, the VHD file named "myOsDisk.vhd" is kept in a storage account named "myStorageAccount" in a container named "myContainer".
    $osDiskUri = "https://myStorageAccount.blob.core.windows.net/myContainer/myOsDisk.vhd"
    
    #Set the VM name and size. This example sets the VM name to "myVM" and the VM size to "Standard_A2".
    $vmName = "myVM"
    $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A2"

    #Add the NIC
    $vm = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id

    #Add the OS disk by using the URL of the copied OS VHD. In this example, when the OS disk is created, the term "osDisk" is appened to the VM name to create the OS disk name. This example also specifies that this Windows-based VHD should be attached to the VM as the OS disk.
    $osDiskName = $vmName + "osDisk"
    $vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri -CreateOption attach -Windows
```


Wenn Sie Daten Datenträger verfügen, die müssen den virtuellen Computer zugeordnet werden soll, sollten Sie auch die folgenden hinzufügen: 

```powershell
    # Optional: Add data disks by using the URLs of the copied data VHDs at the appropriate Logical Unit Number (Lun).
    $dataDiskName = $vmName + "dataDisk"
    $vm = Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -VhdUri $dataDiskUri -Lun 0 -CreateOption attach
```

Die Daten und das Betriebssystem von Datenträger-URLs ungefähr wie folgt aussehen: `https://StorageAccountName.blob.core.windows.net/BlobContainerName/DiskName.vhd`. Finden Sie diese auf das Portal indem Sie mit dem Ziel Speichercontainer durchsuchen, auf das Betriebssystem oder die Daten virtuelle Festplatte, die Sie kopiert wurde und kopieren dann den Inhalt der URL.


## <a name="create-the-vm"></a>Erstellen Sie den virtuellen Computer

Erstellen Sie den virtuellen Computer mithilfe der Konfigurationen, die wir gerade erstellt haben.

```powershell
#Create the new VM
New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vm
```

Wenn dieser Befehl erfolgreich war, wird die Ausgabe wie folgt angezeigt:

```
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK   
 
```
 
## <a name="verify-that-the-vm-was-created"></a>Stellen Sie sicher, dass der virtuellen Computer erstellt wurde 
 
Sie sollten finden Sie unter dem neu erstellten virtuellen Computer entweder in der [Azure-Portal](https://portal.azure.com)unter **Durchsuchen** > **virtuellen Computern**, oder mithilfe der folgenden Befehle PowerShell:

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a>Nächste Schritte

Zum Anmelden bei Ihrem neuen virtuellen Computern, navigieren Sie zu dem virtuellen Computer im [Portal](https://portal.azure.com), klicken Sie auf **Verbinden**, und öffnen Sie die Datei RDP Remote Desktop. Verwenden Sie die Anmeldeinformationen des ursprünglichen virtuellen Computers Anmelden bei Ihrem neuen virtuellen Computern an. Weitere Informationen finden Sie unter [So verbinden, und melden Sie sich bei einer Azure-virtuellen Computern unter Windows](virtual-machines-windows-connect-logon.md).








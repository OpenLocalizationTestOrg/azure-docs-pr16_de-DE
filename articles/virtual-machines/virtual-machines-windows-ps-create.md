<properties
    pageTitle="Erstellen einer Azure virtueller Computer mithilfe der PowerShell | Microsoft Azure"
    description="Verwenden von Azure PowerShell und Azure Ressourcenmanager auf einfache Weise einen neuen virtuellen Computer unter Windows Server erstellen."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/21/2016"
    ms.author="davidmu"/>

# <a name="create-a-windows-vm-using-resource-manager-and-powershell"></a>Erstellen eines Windows virtuellen Computers mit Ressourcenmanager und PowerShell

In diesem Artikel wird gezeigt, wie Schnelles Erstellen einer Azure-virtuellen Computern mit Windows Server und die erforderlichen [Ressourcenmanager](../azure-resource-manager/resource-group-overview.md) und PowerShell mit Ressourcen werden kann. 

Alle Schritte in diesem Artikel zum Erstellen eines virtuellen Computers erforderlich sind, und die Schritte ausführen, sollte ungefähr 30 Minuten dauern. Ersetzen Sie Beispiel Parameterwerte Befehle mit Namen, die für Ihre Umgebung am sinnvollsten.

## <a name="step-1-install-azure-powershell"></a>Schritt 1: Installieren Sie Azure PowerShell

Informationen zum Installieren der neuesten Version von Azure PowerShell, Ihr Abonnement auswählen, und melden Sie sich bei Ihrem Konto finden Sie unter [Informationen zum Installieren und konfigurieren Azure PowerShell](../powershell-install-configure.md) .
        
## <a name="step-2-create-a-resource-group"></a>Schritt 2: Erstellen einer Ressourcengruppe

Alle Ressourcen in einer Ressourcengruppe enthalten sein müssen, lassen Sie uns zuerst das erstellen.  

1. Erhalten Sie eine Liste der verfügbaren Speicherorte aus, in dem Ressourcen erstellt werden können.

    ```powershell
    Get-AzureRmLocation | sort Location | Select Location
    ```

2. Legen Sie den Speicherort für die Ressourcen. Dieser Befehl legt den Speicherort auf **Centralus**.

    ```powershell
    $location = "centralus"
    ```
    
3. Erstellen Sie eine Ressourcengruppe aus. Dieser Befehl erstellt die Ressourcengruppe mit dem Namen **MyResourceGroup** an der Position, die Sie festlegen.

    ```powershell
    $myResourceGroup = "myResourceGroup"
    New-AzureRmResourceGroup -Name $myResourceGroup -Location $location
    ```
    
## <a name="step-3-create-a-storage-account"></a>Schritt 3: Erstellen eines Speicher-Kontos

Ein [Speicher-Konto](../storage/storage-introduction.md) ist erforderlich, um die virtuelle Festplatte zu speichern, die vom virtuellen Computer verwendet wird, die Sie erstellen. Speicher Kontonamen müssen zwischen 3 und 24 Zeichen lang sein und darf Zahlen und nur Kleinbuchstaben enthalten.

1. Der Name des Speicher für die Eindeutigkeit zu testen. Dieser Befehl überprüft den Namen **MyStorageAccount**.

    ```powershell
    $myStorageAccountName = "mystorageaccount"
    Get-AzureRmStorageAccountNameAvailability $myStorageAccountName
    ```
    
    Wenn **dieser Befehl zurückgegeben wird,**ist der vorgeschlagenen Namen im Azure eindeutig. 
    
2. Erstellen Sie nun das Speicherkonto an.
    
    ```powershell    
    $myStorageAccount = New-AzureRmStorageAccount -ResourceGroupName $myResourceGroup -Name $myStorageAccountName -SkuName "Standard_LRS" -Kind "Storage" -Location $location
    ```
    
## <a name="step-4-create-a-virtual-network"></a>Schritt 4: Erstellen eines virtuellen Netzwerks

Alle virtuellen Computern sind Teil eines [virtuelles Netzwerk](../virtual-network/virtual-networks-overview.md).

1. Erstellen Sie ein Subnetz für das virtuelle Netzwerk an. Dieser Befehl erstellt ein Subnetz mit dem Namen **MySubnet** mit einer Adresspräfix 10.0.0.0/24.
        
    ```powershell
    $mySubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnet" -AddressPrefix 10.0.0.0/24
    ```
    
2. Erstellen Sie nun das virtuelle Netzwerk aus. Dieser Befehl erstellt ein virtuelles Netzwerk mit dem Namen **MyVnet** mit dem Subnetz, das Sie erstellt haben und ein Adresspräfix der **10.0.0.0/16**an.

    ```powershell
    $myVnet = New-AzureRmVirtualNetwork -Name "myVnet" -ResourceGroupName $myResourceGroup -Location $location -AddressPrefix 10.0.0.0/16 -Subnet $mySubnet
    ```
        
## <a name="step-5-create-a-public-ip-address-and-network-interface"></a>Schritt 5: Erstellen einer öffentlichen IP-Adresse und Netzwerk-Benutzeroberfläche

Klicken Sie zum Aktivieren der Kommunikation mit den virtuellen Computern in das virtuelle Netzwerk benötigen Sie eine [öffentliche IP-Adresse](../virtual-network/virtual-network-ip-addresses-overview-arm.md) und eine Netzwerk-Benutzeroberfläche an.

1. Erstellen Sie die öffentliche IP-Adresse ein. Dieser Befehl erstellt eine öffentliche IP-Adresse mit dem Namen **MyPublicIp** mit einer Zuteilung-Methode der **dynamischen**.
 
    ```powershell
    $myPublicIp = New-AzureRmPublicIpAddress -Name "myPublicIp" -ResourceGroupName $myResourceGroup -Location $location -AllocationMethod Dynamic
    ```
        
2. Erstellen der Schnittstelle an. Dieser Befehl erstellt eine Netzwerk-Benutzeroberfläche mit dem Namen **myNIC**.

    ```powershell
    $myNIC = New-AzureRmNetworkInterface -Name "myNIC" -ResourceGroupName $myResourceGroup -Location $location -SubnetId $myVnet.Subnets[0].Id -PublicIpAddressId $myPublicIp.Id
    ```
       
## <a name="step-6-create-a-virtual-machine"></a>Schritt 6: Erstellen eines virtuellen Computers

Nachdem Sie alle Teile angeordnet haben, ist es Zeit zum Erstellen des virtuellen Computers.

1. Führen Sie diesen Befehl, um den Administratorkontonamen und das Kennwort des virtuellen Computers festlegen.

        $cred = Get-Credential -Message "Type the name and password of the local administrator account."
        
    Das Kennwort muss bei 12-123 Zeichen lang sein und mindestens einen Kleinbuchstaben, einen Großbuchstaben, eine Zahl und ein Sonderzeichen haben. 
        
2. Erstellen Sie das Konfigurationsobjekt des virtuellen Computers. Dieser Befehl erstellt ein Konfigurationsobjekt mit dem Namen **MyVmConfig** , die den Namen der virtuellen Computer und die Größe der virtuellen Computer definiert.

    ```powershell
    $myVm = New-AzureRmVMConfig -VMName "myVM" -VMSize "Standard_DS1_v2"
    ```
     
    Eine Liste der verfügbaren Größen für einen virtuellen Computer finden Sie unter [Größe für virtuelle Computer in Azure](virtual-machines-windows-sizes.md) .
    
3. Konfigurieren Sie das Betriebssystem von Einstellungen für den virtuellen Computer an. Dieser Befehl legt den Computernamen, Betriebssystemtyp und Anmeldeinformationen für den virtuellen Computer an.

    ```powershell
    $myVM = Set-AzureRmVMOperatingSystem -VM $myVM -Windows -ComputerName "myVM" -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    ```
    
4. Definieren Sie das Bild zu verwenden, um die virtuellen Computer bereitstellen. Dieser Befehl definiert das Windows Server-Bild für den virtuellen Computer verwendet werden soll. 

    ```powershell
    $myVM = Set-AzureRmVMSourceImage -VM $myVM -PublisherName "MicrosoftWindowsServer" -Offer "WindowsServer" -Skus "2012-R2-Datacenter" -Version "latest"
    ```
        
    Weitere Informationen zum Auswählen von Bildern zu verwenden finden Sie unter [Navigieren und Auswählen von Windows virtuellen Computern Bilder in Azure mit PowerShell oder der CLI](virtual-machines-windows-cli-ps-findimage.md).
        
5. Fügen Sie an der Konfiguration der Schnittstelle, die Sie erstellt haben.

    ```powershell
    $myVM = Add-AzureRmVMNetworkInterface -VM $myVM -Id $myNIC.Id
    ```
        
6. Definieren Sie den Namen und den Speicherort der Festplatte virtueller Computer an. Die Datei virtuelle Festplatte wird in einem Container gespeichert. Dieser Befehl erstellt den Datenträger in einem Container mit dem Namen **vhds/WindowsVMosDisk.vhd** in das Speicherkonto, das Sie erstellt haben.

    ```powershell
    $blobPath = "vhds/myOsDisk1.vhd"
    $osDiskUri = $myStorageAccount.PrimaryEndpoints.Blob.ToString() + $blobPath
    ```
        
7. Fügen Sie das Betriebssystem von Datenträgerinformationen an der Konfiguration virtueller Computer an. Ersetzen Sie den Wert von **$diskName** mit einen Namen für das Betriebssystem-Laufwerk ein. Erstellen Sie die Variable, und fügen Sie die Datenträgerinformationen zur Konfiguration.
    
    ```powershell
    $vm = Set-AzureRmVMOSDisk -VM $myVM -Name "myOsDisk1" -VhdUri $osDiskUri -CreateOption fromImage
    ```
        
8. Erstellen Sie schließlich des virtuellen Computers.

    ```
    New-AzureRmVM -ResourceGroupName $myResourceGroup -Location $location -VM $myVM
    ```
                                  
## <a name="next-steps"></a>Nächste Schritte

- Falls Probleme mit der Bereitstellung aufgeführt sind, wäre im nächsten Schritt die [Problembehandlung Ressource Gruppe Bereitstellungen mit Azure-Portal](../resource-manager-troubleshoot-deployments-portal.md) eigenständig
- Erfahren Sie, wie des virtuellen Computers zu verwalten, die Sie erstellt haben, indem Sie [Verwalten virtuellen Computern Azure Ressourcenmanager und PowerShell verwenden](virtual-machines-windows-ps-manage.md).
- Nutzen Sie die Vorteile der Verwendung einer Vorlage zum Erstellen eines virtuellen Computers unter Verwendung der Informationen in [einem Windows-Computer mit einer Vorlage Ressourcenmanager-erstellen](virtual-machines-windows-ps-template.md)

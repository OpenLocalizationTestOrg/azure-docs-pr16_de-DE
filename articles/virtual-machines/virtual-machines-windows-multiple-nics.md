<properties
   pageTitle="Erstellen Sie einen virtuellen Windows mit mehreren NICs | Microsoft Azure"
   description="Informationen Sie zum Erstellen eines Windows virtuellen Computers mit mehreren NICs beigefügt Azure PowerShell oder Ressourcenmanager Vorlagen verwenden."
   services="virtual-machines-windows"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure"
   ms.date="10/27/2016"
   ms.author="iainfou"/>

# <a name="creating-a-windows-vm-with-multiple-nics"></a>Erstellen einen virtuellen Windows mit mehreren NICs
Sie können einen virtuellen Computer (virtueller Computer) in Azure erstellen, die mehrere virtuelle Netzwerkschnittstellen (NICs) verknüpft ist. Ein gängiges Szenario wäre für Front-End- und Back-End-Konnektivität oder ein Netzwerk einer Überwachung oder zusätzliche Lösung für verschiedene Subnetze verfügen. Dieser Artikel bietet schnelle Befehle zum Erstellen eines virtuellen Computers mit mehreren NICs beigefügt. Ausführliche Informationen zum Erstellen von mehreren NICs innerhalb Ihrer eigenen PowerShell-Skripts, einschließlich Weitere Informationen zum [Bereitstellen von Multi-NIC virtuellen Computern](../virtual-network/virtual-network-deploy-multinic-arm-ps.md). Verschiedene [Größen virtueller Computer](virtual-machines-windows-sizes.md) eine unterschiedliche Anzahl von NICs unterstützen, also entsprechend Größe Ihrer virtuellen Computer.

>[AZURE.WARNING] Mehrere Netzwerkkarten muss angefügt werden, wenn Sie einen virtuellen Computer erstellen – NICs zu einem vorhandenen virtuellen Computer hinzuzufügen. Sie können [einen virtuellen basierend auf den ursprünglichen virtuellen Laufwerken zu erstellen](virtual-machines-windows-vhd-copy.md) , und erstellen Sie mehrere Netzwerkkarten aus, wie Sie den virtuellen Computer bereitstellen.

## <a name="create-core-resources"></a>Erstellen von Core Ressourcen
Stellen Sie sicher, dass Sie die [neuesten Azure PowerShell installiert und konfiguriert](../powershell-install-configure.md)haben. Melden Sie sich bei Ihrem Konto Azure aus:

```powershell
Login-AzureRmAccount
```

Ersetzen Sie in den folgenden Beispielen Beispiel Parameternamen durch Ihre eigenen Werte ein. Beispiel für Parameternamen enthalten `myResourceGroup`, `mystorageaccount`, und `myVM`.

Erstellen Sie zuerst eine Ressourcengruppe ein. Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen `myResourceGroup` in der `WestUs` Speicherort:

```powershell
New-AzureRmResourceGroup -Name "myResourceGroup" -Location "WestUS"
```

Erstellen Sie ein Speicherkonto zum Speichern Ihrer virtuellen Computer an. Im folgenden Beispiel wird ein Speicherkonto mit dem Namen `mystorageaccount`:

```powershell
$storageAcc = New-AzureRmStorageAccount -ResourceGroupName "myResourceGroup" `
    -Location "WestUS" -Name "mystorageaccount" `
    -Kind "Storage" -SkuName "Premium_LRS" 
```

## <a name="create-virtual-network-and-subnets"></a>Erstellen von virtuellen Netzwerk und Subnetze
Definieren von zwei virtuelle Netzwerksubnets - eine für Front-End-Datenverkehr und eine für die Back-End-Datenverkehr. Im folgende Beispiel definiert zwei Subnetzen, mit dem Namen `mySubnetFrontEnd` und `mySubnetBackEnd`:

```powershell
$mySubnetFrontEnd = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnetFrontEnd" `
    -AddressPrefix "192.168.1.0/24"
$mySubnetBackEnd = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnetBackEnd" `
    -AddressPrefix "192.168.2.0/24"
```

Erstellen Sie virtuelle Netzwerk und Subnetze. Im folgenden Beispiel wird ein virtuelles Netzwerk mit dem Namen `myVnet`:

```powershell
$myVnet = New-AzureRmVirtualNetwork -ResourceGroupName "myResourceGroup" `
    -Location "WestUS" -Name "myVnet" -AddressPrefix "192.168.0.0/16" `
    -Subnet $mySubnetFrontEnd,$mySubnetBackEnd
```


## <a name="create-multiple-nics"></a>Erstellen mehrerer NICs
Erstellen Sie die beiden Karten anfügen eine Netzwerkkarte mit dem Front-End-Subnetz und eine Netzwerkkarte mit dem Back-End-Subnetz. Im folgenden Beispiel wird die beiden Karten mit dem Namen `myNic1` und `myNic2`:

```powershell
$frontEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetFrontEnd'}
$myNic1 = New-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
    -Location "WestUS" -Name "myNic1" -SubnetId $frontEnd.Id

$backEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetBackEnd'}
$myNic2 = New-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
    -Location "WestUS" -Name "myNic2" -SubnetId $backEnd.Id
```

In der Regel erstellen Sie auch eine [Netzwerksicherheitsgruppe](../virtual-network/virtual-networks-nsg.md) oder [Lastenausgleich](../load-balancer/load-balancer-overview.md) besser verwalten und den Datenverkehr auf Ihre virtuellen Computern verteilen. [Ausführlichere Multi-NIC virtueller Computer](../virtual-network/virtual-network-deploy-multinic-arm-ps.md) Artikel leitet Sie durch Erstellen einer Sicherheitsgruppe Netzwerk und Zuweisen von NICs.


## <a name="create-the-virtual-machine"></a>Erstellen des virtuellen Computers
Erstellen die Konfiguration virtueller Computer jetzt beginnen. Jeder virtueller Speicher sind maximal für die Gesamtzahl der NICs, die Sie einen virtuellen Computer hinzufügen können. Weitere Informationen zu [Windows virtueller Computer Größen](virtual-machines-windows-sizes.md). 

Legen Sie zunächst Ihre Anmeldeinformationen ein virtueller Computer auf die `$cred` Variable wie folgt:

```powershell
$cred = Get-Credential
```

Im folgenden Beispiel wird einen virtueller Computer mit dem Namen `myVM` und verwendet eine virtueller Speicher, die unterstützt bis zu zwei NICs (`Standard_DS2_v2`):

```powershell
$vmConfig = New-AzureRmVMConfig -VMName "myVM" -VMSize "Standard_DS2_v2"
```

Erstellen Sie den Rest der Datei Config virtueller Computer an. Im folgenden Beispiel wird ein Windows Server 2012 R2 virtueller Computer:

```powershell
$vmConfig = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName Te"MyVM" `
    -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
$vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig -PublisherName "MicrosoftWindowsServer" `
    -Offer "WindowsServer" -Skus "2012-R2-Datacenter" -Version "latest"
```

Fügen Sie die beiden Karten, die Sie zuvor erstellt haben:

```powershell
$vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $myNic1.Id -Primary
$vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $myNic2.Id
```

Konfigurieren Sie die Speicherung und virtuelle Laufwerk für Ihre neue virtuellen Computer an:

```powershell
$blobPath = "vhds/WindowsVMosDisk.vhd"
$osDiskUri = $storageAcc.PrimaryEndpoints.Blob.ToString() + $blobPath
$diskName = "windowsvmosdisk"
$vmConfig = Set-AzureRmVMOSDisk -VM $vmConfig -Name $diskName -VhdUri $osDiskUri `
    -CreateOption "fromImage"
```

Schließlich ein virtuellen Computers zu erstellen:

```powershell
New-AzureRmVM -VM $vmConfig -ResourceGroupName "myResourceGroup" -Location "WestUS"
```

## <a name="creating-multiple-nics-using-resource-manager-templates"></a>Erstellen von mehreren NICs Ressourcenmanager Vorlagen verwenden
Azure Ressourcenmanager Vorlagen verwenden declarative JSON-Dateien, um Ihre Umgebung definieren. Sie können eine [Übersicht der Azure Ressourcenmanager](../azure-resource-manager/resource-group-overview.md)lesen. Ressourcenmanager Vorlagen bieten eine Möglichkeit, mehrere Instanzen einer Ressource während der Bereitstellung, z. B. das Erstellen von mehreren NICs erstellen. Verwenden Sie *Kopieren* , um die Anzahl der Instanzen erstellen anzugeben:

```bash
"copy": {
    "name": "multiplenics"
    "count": "[parameters('count')]"
}
```

Weitere Informationen zum [Erstellen von mehreren Instanzen über *Kopieren*](../resource-group-create-multiple.md). 

Sie können auch eine `copyIndex()` klicken Sie dann eine Zahl an einen Ressourcennamen angefügt, die Sie erstellen können `myNic1`, `MyNic2`usw.. Die nachstehende Abbildung zeigt ein Beispiel für Anhängen des Werts vom Index:

```bash
"name": "[concat('myNic', copyIndex())]", 
```

Sie können ein vollständiges Beispiel für [mehrere NICs mit Ressourcenmanager Vorlagen erstellen](../virtual-network/virtual-network-deploy-multinic-arm-template.md), lesen.

## <a name="next-steps"></a>Nächste Schritte
Vergewissern Sie sich [Größen virtuellen Windows-Computer](virtual-machines-windows-sizes.md) zu überprüfen, bei dem Versuch, einen virtuellen Computer mit mehreren NICs erstellen. Achten Sie auf die maximale Anzahl von NICs jedes virtueller Speicher unterstützt. 

Denken Sie daran, dass Sie keine weiteren NICs einer vorhandenen virtuellen Computer hinzufügen, müssen Sie alle Netzwerkkarten erstellen, wenn Sie den virtuellen Computer bereitstellen. Achten Sie beim Planen der Bereitstellung Ihrer, um sicherzustellen, dass Sie die erforderlichen Netzwerkkonnektivität von Anfang an haben.
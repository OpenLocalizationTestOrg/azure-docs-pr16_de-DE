<properties
   pageTitle="Azure Hybrid verwenden Vorteile für Fenster Server | Microsoft Azure"
   description="Erfahren Sie, wie Sie Ihre Windows Server Software Assurance-Vorteile zur lokalen Lizenzen an Azure bringen maximieren"
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
   ms.workload="infrastructure-services"
   ms.date="07/13/2016"
   ms.author="georgem"/>

# <a name="azure-hybrid-use-benefit-for-windows-server"></a>Azure Hybrid verwenden Vorteile für WindowsServer

Für Kunden mit Windows Server mit Software Assurance können Sie wieder abrufen Ihrer lokalen Windows Server-Lizenzen in Azure und Ausführen von Windows Server virtuellen Computern in Azure bei einer reduzierten Kosten. Die Vorteile von Azure Hybrid verwenden, können Sie Windows Server virtuellen Computern in Azure ausgeführt und nur erste in Rechnung gestellt für die Rate Basis berechnen. Weitere Informationen finden Sie in der [Azure Hybrid verwenden Vorteile Lizenzierung Seite](https://azure.microsoft.com/pricing/hybrid-use-benefit/). In diesem Artikel wird erläutert, wie Windows Server virtuellen Computern in Azure mit diesen zur Lizenzierung Vorteil bereitstellen.

> [AZURE.NOTE] Sie können Bilder Azure Marketplace Bereitstellen von Windows Server-virtuellen Computern Nutzung der Azure Hybrid verwenden Vorteil nutzen. Sie müssen Ihre virtuellen Computer verwenden entweder PowerShell oder Ressourcenmanager Vorlagen Ihre virtuellen Computern korrekt als Basis berechnen Zins Rabatt berechtigt registriert bereitstellen.

## <a name="pre-requisites"></a>Erforderliche Komponenten
Es gibt ein paar erforderlichen Komponenten und Azure Hybrid verwenden Vorteile für Windows Server virtuellen Computern in Azure verwendet wurden:

- Haben Sie das Azure PowerShell-Modul installiert
- Haben Sie Ihre Windows Server virtuelle Festplatte geladen Azure-Speicher

### <a name="install-azure-powershell"></a>Installieren von Azure PowerShell
Stellen Sie sicher, dass Sie [installiert und die neuesten Azure PowerShell konfiguriert](../powershell-install-configure.md)haben. Auch wenn Sie beabsichtigen, Ihre virtuellen Computer mithilfe von Vorlagen Ressourcenmanager bereitstellen, immer noch benötigten Azure PowerShell installiert haben, um Ihre Windows Server virtuelle Festplatte Hochladen (Siehe den folgenden Schritt).

### <a name="upload-a-windows-server-vhd"></a>Hochladen eines Windows Servers virtuelle Festplatte

Um einen Windows Server virtuellen Computer Azure bereitstellen zu können, müssen Sie zuerst eine virtuelle Festplatte zu erstellen, die Ihre Basis Windows Server-Build enthält. Diese virtuelle Festplatte muss über Sysprep ordnungsgemäß vorbereitet werden, bevor Sie es in Azure hochladen. Sie können [Weitere Informationen zu den Anforderungen virtuelle Festplatte und Sysprep-Prozess](./virtual-machines-windows-upload-image.md) und [Sysprep-Unterstützung für Serverrollen](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles). Sichern Sie den virtuellen Computer aus, bevor Sie Sysprep starten. Nachdem Sie Ihre virtuelle Festplatte vorbereitet haben, die virtuelle Festplatte hochladen, über Ihr Konto Azure-Speicher von der `Add-AzureRmVhd` Cmdlet wie folgt:

```
Add-AzureRmVhd -ResourceGroupName MyResourceGroup -Destination "https://mystorageaccount.blob.core.windows.net/vhds/myvhd.vhd" -LocalFilePath 'C:\Path\To\myvhd.vhd'
```

> [AZURE.NOTE] Microsoft SQL Server, SharePoint-Server und Dynamics kann auch Ihre Software Assurance-Lizenzierung nutzen. Sie müssen immer noch das Windows Server-Image vorbereiten nach der Installation von Ihrer Anwendungskomponenten und Lizenz Tasten entsprechend bereitstellen sowie das Datenträgerabbild in Azure hochladen. Überprüfen Sie die Dokumentation zur mit der Anwendung, beispielsweise [Überlegungen für die Installation von SQL Server mithilfe von Sysprep](https://msdn.microsoft.com/library/ee210754.aspx) oder [Erstellen Sie ein SharePoint Server 2016 Bezug Bild (Sysprep)](http://social.technet.microsoft.com/wiki/contents/articles/33789.build-a-sharepoint-server-2016-reference-image-sysprep.aspx).

Sie können auch weitere Informationen zum [Hochladen der virtuellen Festplatte zu Azure Prozess](./virtual-machines-windows-upload-image.md#upload-the-vm-image-to-your-storage-account).

> [AZURE.TIP] Dieser Artikel befasst sich Windows Server virtuellen Computern bereitstellen. Sie können auch Windows-Client-virtuellen Computern auf dieselbe Weise bereitstellen. Ersetzen Sie in den folgenden Beispielen, `Server` mit `Client` ordnungsgemäß.

## <a name="deploy-a-vm-via-powershell-quick-start"></a>Bereitstellen eines virtuellen Computers über PowerShell Schnellstart
Wenn Sie Ihre Windows Server virtueller Computer über PowerShell bereitstellen, haben Sie einen weiteren Parameter für `-LicenseType`. Nachdem Sie Ihre virtuelle Festplatte in Azure hochgeladen haben, erstellen Sie ein neues virtueller Computer mit `New-AzureRmVM` , und geben Sie den Typ zur Lizenzierung wie folgt:

```
New-AzureRmVM -ResourceGroupName MyResourceGroup -Location "West US" -VM $vm -LicenseType Windows_Server
```

Sie können, [Lesen eine ausführliche exemplarische Vorgehensweise auf Bereitstellen eines virtuellen Computers in Azure über PowerShell](./virtual-machines-windows-hybrid-use-benefit-licensing.md#deploy-windows-server-vm-via-powershell-detailed-walkthrough) unten, oder Lesen einen aussagekräftigeren Leitfaden auf die verschiedenen Schritte zum [Erstellen eines Windows virtuellen Computers Ressourcenmanager und PowerShell verwenden](./virtual-machines-windows-ps-create.md).

## <a name="deploy-a-vm-via-resource-manager"></a>Bereitstellen eines virtuellen Computers über Ressourcenmanager
In den Ressourcenmanager Vorlagen, einen zusätzlichen Parameter für `licenseType` angegeben werden können. Sie können weitere Informationen zur [Erstellung von Azure Ressourcenmanager Vorlagen](../resource-group-authoring-templates.md). Nachdem Sie Ihre virtuelle Festplatte in Azure hochgeladen haben, bearbeiten Sie Ressourcenmanager-Vorlage zum Einschließen des Lizenz Typs als Teil der Anbieter berechnen und Bereitstellen Ihrer Vorlage wie gewohnt aus:

```
"properties": {  
   "licenseType": "Windows_Server",
   "hardwareProfile": {
        "vmSize": "[variables('vmSize')]"
   },
```
 
## <a name="verify-your-vm-is-utilizing-the-licensing-benefit"></a>Stellen Sie sicher, dass Ihre virtuellen Computer nutzt die Vorteile zur Lizenzierung
Nachdem Sie Ihre virtuellen Computer über der PowerShell oder Ressourcenmanager-Bereitstellung-Methode bereitgestellt haben, überprüfen Sie den Lizenztyp mit `Get-AzureRmVM` wie folgt:
 
```
Get-AzureRmVM -ResourceGroup MyResourceGroup -Name MyVM
```

Die Ausgabe ist ähnlich wie die folgende:

```
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              : Windows_Server
```

Dies steht im Gegensatz zu des folgenden virtuellen Computer bereitgestellt werden, ohne die Vorteile von Azure Hybrid verwenden Lizenzierung, wie etwa einen virtuellen direkt aus dem Katalog Azure bereitgestellt:

```
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              : 
```
 
## <a name="detailed-powershell-walkthrough"></a>Ausführliche exemplarische der PowerShell

Die folgenden detaillierten Schritte für PowerShell anzeigen eine vollständige Bereitstellung eines virtuellen Computers Lesen Sie mehr Kontext hinsichtlich der ist-Cmdlets und anderen Komponenten in [einen Windows virtuellen Computer mithilfe von Ressourcenmanager und PowerShell erstellen](./virtual-machines-windows-ps-create.md)erstellt wird. Sie Schritt durch die Erstellung der Ressourcengruppe, Speicher-Konto und virtuelle Netzwerke, und klicken Sie dann Ihre virtuellen Computer definieren und schließlich Erstellen Ihrer virtuellen Computer.
 
Zunächst sichere erhalten Sie Anmeldeinformationen zu, legen Sie einen Speicherort und einen Ressourcengruppennamen:

```
$cred = Get-Credential
$location = "West US"
$resourceGroupName = "TestLicensing"
```

Erstellen Sie eine öffentliche IP-Adresse:

```
$publicIPName = "testlicensingpublicip"
$publicIP = New-AzureRmPublicIpAddress -Name $publicIPName -ResourceGroupName $resourceGroupName -Location $location -AllocationMethod Dynamic
```

Definieren Sie Ihrem Subnetz, NIC und VNET:

```
$subnetName = "testlicensingsubnet"
$nicName = "testlicensingnic"
$vnetName = "testlicensingvnet"
$subnetconfig = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/8
$vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroupName -Location $location -AddressPrefix 10.0.0.0/8 -Subnet $subnetconfig
$nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $resourceGroupName -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $publicIP.Id
```

Benennen Sie Ihre virtuellen Computer, und erstellen Sie eine Config virtueller Computer:

```
$vmName = "testlicensing"
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A1"
```

Definieren Sie Ihre OS:

```
$computerName = "testlicensing"
$vm = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $computerName -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
```

Fügen Sie den virtuellen Computer Ihrer NIC hinzu:

```
$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

Definieren des Speicher-Kontos zu verwenden:

```
$storageAcc = Get-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -AccountName testlicensing
```

Hochladen der virtuellen Festplatte, angemessen vorbereitet, und Anfügen an Ihre virtuellen Computer für die Verwendung:

```
$osDiskName = "licensing.vhd"
$osDiskUri = '{0}vhds/{1}{2}.vhd' -f $storageAcc.PrimaryEndpoints.Blob.ToString(), $vmName.ToLower(), $osDiskName
$urlOfUploadedImageVhd = "https://testlicensing.blob.core.windows.net/vhd/licensing.vhd"
$vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri -CreateOption FromImage -SourceImageUri $urlOfUploadedImageVhd -Windows
```

Schließlich erstellen Sie Ihrer virtuellen Computer und definieren Sie den Typ zur Lizenzierung, um die Vorteile von Azure Hybrid verwenden zu nutzen:

```
New-AzureRmVM -ResourceGroupName $resourceGroupName -Location $location -VM $vm -LicenseType Windows_Server
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur [Lizenzierung Vorteile von Azure Hybrid verwenden](https://azure.microsoft.com/pricing/hybrid-use-benefit/).

Weitere Informationen zum [Verwenden von Ressourcenmanager Vorlagen](../azure-resource-manager/resource-group-overview.md).

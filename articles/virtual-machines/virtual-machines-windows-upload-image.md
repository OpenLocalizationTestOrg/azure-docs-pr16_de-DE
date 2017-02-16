<properties
    pageTitle="Hochladen einer Windows virtuelle Festplatte für Ressourcenmanager | Microsoft Azure"
    description="Sie lernen, wie einen Windows-Computer virtuelle Festplatte aus lokalen in Azure, verwenden das Modell zur Bereitstellung von Ressourcenmanager hochladen. Sie können eine virtuelle Festplatte aus entweder eine GRG oder einen speziellen virtuellen hochladen."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="cynthn"/>

# <a name="upload-a-windows-vhd-from-an-on-premises-vm-to-azure"></a>Hochladen einer Windows virtuelle Festplatte aus einer lokalen virtuellen Computer zu Azure 


In diesem Artikel wird veranschaulicht, wie erstellen und Hochladen einer Windows virtuellen Festplatte (virtuelle Festplatte), um beim Erstellen einer Azure-virtuellen Computer verwendet werden. Sie können eine virtuelle Festplatte aus GRG eines virtuellen Computers oder einen speziellen virtuellen hochladen. 

Weitere Informationen hierzu zu Festplatten und virtuelle Festplatten in Azure finden Sie unter [Datenträger und virtueller Festplatten für virtuelle Computer](virtual-machines-linux-about-disks-vhds.md).


## <a name="prepare-the-vm"></a>Bereiten Sie den virtuellen Computer vor 

Sie können GRG und spezielle virtuelle Festplatten in Azure hochladen. Jeder Typ erfordert, dass Sie den virtuellen Computer, bevor Sie beginnen vorbereiten.

- **GRG-virtuelle Festplatte** - GRG eine virtuelle Festplatte wurden alle Ihre persönlichen Kontoinformationen mit Sysprep entfernt. Wenn Sie beabsichtigen, die virtuelle Festplatte als Bild verwenden, um neue virtuelle Computer aus zu erstellen, sollten Sie folgende Schritte ausführen:
    - [Vorbereiten einer virtuellen Windows Azure hochladen](virtual-machines-windows-prepare-for-upload-vhd-image.md). 
    - [Verallgemeinern des virtuellen Computers Sysprep verwendet](virtual-machines-windows-generalize-vhd.md)wird. 

- **Spezielle virtuelle Festplatte** - eine spezialisierte virtuelle Festplatte verwaltet die Benutzerkonten, Anwendungen und anderen Zustandsdaten aus Ihrem ursprünglichen virtuellen. Wenn Sie beabsichtigen, verwenden Sie die virtuelle Festplatte als-besteht darin, das Erstellen eines neuen virtuellen Computers, stellen Sie sicher, die folgenden Schritte ausgeführt werden. 
    - [Vorbereiten einer virtuellen Windows Azure hochladen](virtual-machines-windows-prepare-for-upload-vhd-image.md). **Keine** generalize den virtuellen Computer mithilfe von Sysprep.
    - Entfernen Sie alle Gast Virtualisierung Tools und Agents, die des virtuellen Computers (d. h. VMware Tools) installiert sind.
    - Stellen Sie sicher, dass der virtuellen Computer ziehen Sie deren IP-Adresse und DNS-Einstellungen über DHCP konfiguriert ist. Dadurch wird sichergestellt, dass der Server eine IP-Adresse innerhalb der VNet beim Starten erhält. 

## <a name="log-in-to-azure"></a>Melden Sie sich bei Azure

Wenn Sie noch keine PowerShell Version 1.4 oder höher installiert ist, finden Sie hier [Informationen zum Installieren und konfigurieren Azure PowerShell](../powershell-install-configure.md)haben.

1. Öffnen Sie Azure PowerShell zu, und melden Sie sich bei Ihrem Konto Azure. Ein Popupfenster wird geöffnet, dienen soll, geben Sie Ihre Kontoanmeldeinformationen ein Azure.

    ```powershell
    Login-AzureRmAccount
    ```


2. Holen Sie sich das Abonnement IDs für Ihre Abonnements zur Verfügung.

    ```powershell
    Get-AzureRmSubscription
    ```

3. Legen Sie das richtige Abonnement mit der Abonnement-ID. Ersetzen Sie `<subscriptionID>` die ID des richtigen Abonnements.

    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"
    ```

## <a name="get-the-storage-account"></a>Erhalten der Speicherkontos

Benötigen Sie ein Speicherkonto in Azure das hochgeladene virtueller Computer Bild gespeichert. Sie können ein vorhandenes Speicherkonto verwenden oder einen neuen erstellen. 

Um den verfügbaren Speicherplatz Konten anzeigen möchten, geben Sie Folgendes ein:

```powershell
Get-AzureRmStorageAccount
```

Wenn Sie ein vorhandenes Speicherkonto verwenden möchten, gehen Sie zum Abschnitt [das virtueller Computer Bild hochladen](#upload-the-vm-vhd-to-your-storage-account) .

Wenn Sie ein Speicherkonto erstellen müssen, gehen Sie folgendermaßen vor:

1. Sie benötigen den Namen der Ressourcengruppe Stelle, an der das Speicherkonto erstellt werden soll. Alle Ressourcengruppen finden, die im Rahmen Ihres Abonnements sind, geben Sie Folgendes ein:

    ```powershell
    Get-AzureRmResourceGroup
    ```

Um eine Ressourcengruppe mit dem Namen **MyResourceGroup** in der Region **Westen US** erstellen möchten, geben Sie Folgendes ein:

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location "West US"
    ```

2. Erstellen einer benannten **Mystorageaccount** in dieser Ressourcengruppe mithilfe des Cmdlets [New-AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) Speicher-Konto an:

    ```powershell
    New-AzureRmStorageAccount -ResourceGroupName myResourceGroup -Name mystorageaccount -Location "West US" -SkuName "Standard_LRS" -Kind "Storage"
    ```
            
    Gültige Werte für - SkuName sind:

    - **Standard_LRS** - lokal redundante Speicherung. 
    - **Standard_ZRS** - Zone redundante Speicherung.
    - **Standard_GRS** - Geo redundante Speicherung. 
    - **Standard_RAGRS** - Lesezugriff Geo redundante Speicherung. 
    - **Premium_LRS** - Premium lokal redundante Speicherung. 



## <a name="upload-the-vhd-to-your-storage-account"></a>Hochladen der virtuellen Festplatte bei Ihrem Speicherkonto

Verwenden Sie das Cmdlet [AzureRmVhd hinzufügen](https://msdn.microsoft.com/library/mt603554.aspx) , um das Bild zu einem Container in Ihr Speicherkonto hochzuladen. In diesem Beispiel uploads der Datei **myVHD.vhd** von `"C:\Users\Public\Documents\Virtual hard disks\"` in einem Speicher Konto mit dem Namen **Mystorageaccount** in der Ressourcengruppe **MyResourceGroup** . Die Datei wird in den Container mit dem Namen **Mycontainer** platziert werden und der neuen Dateinamen werden **myUploadedVHD.vhd**.

```powershell
$rgName = "myResourceGroup"
$urlOfUploadedImageVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzureRmVhd -ResourceGroupName $rgName -Destination $urlOfUploadedImageVhd -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
```


Wenn der Vorgang erfolgreich ist, erhalten Sie eine Antwort, die sieht ungefähr wie folgt aus:

```
  C:\> Add-AzureRmVhd -ResourceGroupName myResourceGroup -Destination https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
  MD5 hash is being calculated for the file C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd.
  MD5 hash calculation is completed.
  Elapsed time for the operation: 00:03:35
  Creating new page blob of size 53687091712...
  Elapsed time for upload: 01:12:49

  LocalFilePath           DestinationUri
  -------------           --------------
  C:\Users\Public\Doc...  https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd
```

Abhängig von Ihrer Netzwerk-Verbindung und die Größe Ihrer Datei virtuelle Festplatte kann dieser Befehl eine Weile dauern.


## <a name="next-steps"></a>Nächste Schritte

- [Erstellen eines virtuellen Computers in Azure aus einer GRG virtuellen](virtual-machines-windows-create-vm-generalized.md)
- [Erstellen eines virtuellen Computers in Azure aus einer bestimmten virtuellen](virtual-machines-windows-create-vm-specialized.md) , indem Sie sie beim Erstellen eines neuen virtuellen Computers als einen Datenträger OS anfügen.



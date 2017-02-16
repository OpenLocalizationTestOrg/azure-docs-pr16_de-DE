<properties 
   pageTitle="Allgemeine PowerShell-Befehle für virtuelle Computer | Microsoft Azure"
   description="Allgemeine PowerShell-Befehle, die Ihnen beim Einstieg erstellen und Verwalten von Ihrem virtuellen Computern in auf Windows Azure"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="davidmu1" 
   manager="timlt" 
   editor="tysonn" 
   tags="azure-resource-manager"/>
   
<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="09/27/2016"
   ms.author="davidmu" />

# <a name="common-powershell-commands-for-creating-and-managing-vms"></a>Allgemeine PowerShell-Befehle zum Erstellen und Verwalten von virtuellen Computern

Dieser Artikel behandelt einige der Azure PowerShell Befehle, die Sie zum Erstellen und Verwalten von virtuellen Computern in Ihrem Abonnement Azure verwenden können.  Weitere ausführliche Hilfe zu mit bestimmten Befehlszeilenoptionen und Optionen können Sie **Hilfe** *Befehl*verwenden.

Informationen zum Installieren der neuesten Version von Azure PowerShell, Ihr Abonnement auswählen, und melden Sie sich bei Ihrem Konto finden Sie unter [Informationen zum Installieren und konfigurieren Azure PowerShell](../powershell-install-configure.md) .

## <a name="create-a-vm"></a>Erstellen eines virtuellen Computers

Aufgabe | Befehl
-------------- | -------------------------
Erstellen Sie eine Konfiguration virtueller Computer | $vm = Vm_size" [New-AzureRmVMConfig](https://msdn.microsoft.com/library/mt603727.aspx) - VMName"Vm_name"- VMSize"<BR></BR><BR></BR>Die Konfiguration virtueller Computer dient zum Definieren oder Aktualisieren der Einstellungen für den virtuellen Computer. Die Konfiguration ist mit dem Namen der virtuellen Computer und dessen [Größe](virtual-machines-windows-sizes.md)Initialisierung.
Fügen Sie die | $vm = [Set-AzureRmVMOperatingSystem](https://msdn.microsoft.com/library/mt603843.aspx) - virtuellen Computer $vm-Windows - ComputerName "Computername"-Anmeldeinformationen $cred - ProvisionVMAgent - EnableAutoUpdate<BR></BR><BR></BR>Betriebssystem-Einstellungen, einschließlich der [Anmeldeinformationen](https://technet.microsoft.com/library/hh849815.aspx) werden das Konfigurationsobjekt hinzugefügt, die Sie zuvor erstellt haben, neu-AzureRmVMConfig verwenden.
Hinzufügen einer Netzwerk-Benutzeroberfläche | $vm = [Hinzufügen-AzureRmVMNetworkInterface](https://msdn.microsoft.com/library/mt619351.aspx) - virtuellen Computer $vm-Id $ NIC. ID<BR></BR><BR></BR>Ein virtueller Computer müssen eine [Schnittstelle](virtual-machines-windows-ps-create.md) in einem virtuellen Netzwerk kommunizieren. [Get-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619434.aspx) können Sie auch ein vorhandenes Netzwerk Benutzeroberflächen-Objekt abzurufen.
Geben Sie ein Bild Plattform | $vm = Herausgebername" [Set-AzureRmVMSourceImage](https://msdn.microsoft.com/library/mt619344.aspx) virtueller Computer - $vm - PublisherName"-anbieten "Publisher_offer" - Skus "Product_sku"-Version "neueste"<BR></BR><BR></BR>[Abbildung Informationen](virtual-machines-windows-cli-ps-findimage.md) wird das Konfigurationsobjekt hinzugefügt, die Sie zuvor erstellt haben, neu-AzureRmVMConfig verwenden. Das Objekt aus dieser Befehl zurückgegeben wird nur verwendet, wenn Sie den Datenträger OS zum Verwenden eines Bilds Plattform festlegen.
Festlegen von OS-Datenträger zum Verwenden eines Bilds Plattform | $vm = [Set-AzureRmVMOSDisk](https://msdn.microsoft.com/library/mt603746.aspx) - virtuellen Computer $vm-http://mystore1.blob.core.windows.net/vhds/disk_name.vhd"Name"Disk_name"- VhdUri" - CreateOption FromImage<BR></BR><BR></BR>Auf das Konfigurationsobjekt, die Sie zuvor erstellt haben, wird der Name des Laufwerks Betriebssystem und seiner Position im [Speicher](../storage/storage-powershell-guide-full.md) hinzugefügt.
Festlegen von OS Datenträger ein GRG Bild verwenden | $vm = Set-AzureRmVMOSDisk - virtuellen Computer $vm-https://mystore1.blob.core.windows.net/system/Microsoft.Compute/Images/myimages/myprefix-osDisk"Name"Disk_name"- SourceImageUri. {Guid} VHD"- VhdUri"https://mystore1.blob.core.windows.net/vhds/disk_name.vhd"- CreateOption FromImage-Windows<BR></BR><BR></BR>Der Name der Datenträger: das Betriebssystem, die Position des Quellbilds und [Speicherort des Datenträgers](../storage/storage-powershell-guide-full.md) wird das Konfigurationsobjekt hinzugefügt.
Festlegen von OS-Datenträger, um eine spezielle Bild verwenden | $vm = Set-AzureRmVMOSDisk - virtuellen Computer $vm-http://mystore1.blob.core.windows.net/vhds/"Name"Name_of_disk"- VhdUri" - CreateOption anfügen - Windows
Erstellen eines virtuellen Computers | [Neu-AzureRmVM]() - ResourceGroupName "Resource_group_name"-Speicherort "Location_name" virtuellen Computer - $vm<BR></BR><BR></BR>Alle Ressourcen werden in einer [Ressourcengruppe](../powershell-azure-resource-manager.md)erstellt. Der virtuellen Computer muss am selben [Speicherort](https://msdn.microsoft.com/library/azure/dn495177.aspx) wie die Ressourcengruppe erstellt werden. Bevor Sie diesen Befehl ausführen, führen Sie neu-AzureRmVMConfig, Set-AzureRmVMOperatingSystem, Set-AzureRmVMSourceImage, hinzufügen-AzureRmVMNetworkInterface und Set-AzureRmVMOSDisk.

## <a name="get-information-about-vms"></a>Erhalten von Informationen zu virtuellen Computern

Aufgabe | Befehl
-------------- | -------------------------
Liste virtuellen Computern in ein Abonnement| [Get-AzureRmVM](https://msdn.microsoft.com/library/mt603718.aspx)
Liste virtuellen Computern in einer Ressourcengruppe | Get-AzureRmVM - ResourceGroupName "Resource_group_name"<BR></BR><BR></BR>Um eine Liste der Ressourcengruppen im Rahmen Ihres Abonnements erhalten möchten, verwenden Sie [Get-AzureRmResourceGroup](https://msdn.microsoft.com/library/mt679016.aspx)aus.
Erhalten von Informationen zu eines virtuellen Computers | "Resource_group_name" Get-AzureRmVM - ResourceGroupName-Namen "Vm_name"

## <a name="manage-vms"></a>Verwalten von virtuellen Computern

Aufgabe | Befehl
-------------- | -------------------------
Starten eines virtuellen Computers | [Start-AzureRmVM](https://msdn.microsoft.com/library/mt603453.aspx) - ResourceGroupName "Resource_group_name"-Namen "Vm_name"
Beenden eines virtuellen Computers | "Resource_group_name" [Stopp-AzureRmVM](https://msdn.microsoft.com/library/mt603483.aspx) - ResourceGroupName-Namen "Vm_name"
Starten eines laufenden virtuellen Computers | "Resource_group_name" [Restart-AzureRmVM](https://msdn.microsoft.com/library/mt603775.aspx) - ResourceGroupName-Namen "Vm_name"
Löschen eines virtuellen Computers | "Resource_group_name" [Entfernen-AzureRmVM](https://msdn.microsoft.com/library/mt603641.aspx) - ResourceGroupName-Namen "Vm_name"
Generalize eines virtuellen Computers | [Set-AzureRmVm](https://msdn.microsoft.com/library/mt603688.aspx) - ResourceGroupName YourResourceGroup-Namen "Vm_name"-GRG<BR></BR><BR></BR>Führen Sie diesen Befehl, bevor Sie speichern-AzureRmVMImage ausführen.
Erfassen eines virtuellen Computers | [Speichern-AzureRmVMImage](https://msdn.microsoft.com/library/mt619423.aspx) - ResourceGroupName "Resource_group_name" - VMName "Vm_name" - DestinationContainerName "Image_container" - VHDNamePrefix "Image_name_prefix"-Pfad "C:\filepath\filename.json"<BR></BR><BR></BR>Eine virtuellen Computern muss [fahren und GRG](virtual-machines-windows-generalize-vhd.md) verwendet werden, um ein Bild zu erstellen. Bevor Sie diesen Befehl ausführen, führen Sie-AzureRmVm festlegen.
Aktualisieren eines virtuellen Computers | [Update-AzureRmVM](https://msdn.microsoft.com/library/mt603662.aspx) - ResourceGroupName "Resource_group_name" virtuellen Computer - $vm<BR></BR><BR></BR>Abrufen der aktuellen virtuellen Computer-Konfigurations, die mit Get-AzureRmVM, Konfiguration Einstellungen auf das Objekt virtueller Computer ändern, und führen Sie diesen Befehl.
Fügen Sie einen Datenträger zum eines virtuellen Computers | [Hinzufügen-AzureRmVMDataDisk](https://msdn.microsoft.com/library/mt603673.aspx) - virtuellen Computer $vm-https://mystore1.blob.core.windows.net/vhds/disk_name.vhd"Name"Disk_name"- VhdUri" - LUN #-Zwischenspeichern Lesen/Schreiben - DiskSizeinGB # - CreateOption leeren<BR></BR><BR></BR>Verwenden Sie Get-AzureRmVM zum Abrufen des Objekts virtueller Computer an. Geben Sie die Anzahl der LUN und die Größe des Datenträgers. Führen Sie die Update-AzureRmVM, um die Konfiguration für den virtuellen Computer zu verwenden. Der Datenträger, den Sie hinzufügen, wird keine Initialisierung. Informationen der Initialisierung der Datenträger hinzugefügt werden finden Sie unter [Verwalten von Azure virtuellen Computern Ressourcenmanager und PowerShell zu verwenden](virtual-machines-windows-ps-manage.md).
Entfernen Sie einen Datenträger eines virtuellen Computers | [Entfernen-AzureRmVMDataDisk](https://msdn.microsoft.com/library/mt603614.aspx) - virtuellen Computer $vm-Namen "Disk_name"<BR></BR><BR></BR>Verwenden Sie Get-AzureRmVM zum Abrufen des Objekts virtueller Computer an. Führen Sie die Update-AzureRmVM, um die Konfiguration für den virtuellen Computer zu verwenden.
Hinzufügen einer Erweiterung zu eines virtuellen Computers | "Resource_group_name" [Set-AzureRmVMExtension](https://msdn.microsoft.com/library/mt603745.aspx) - ResourceGroupName-Vm_name"Speicherort"Azure_location"- VMName"-Namen "Extension_name"-Publisher "Herausgebername"-Typ "Extension_type" - TypeHandlerVersion "#. #"-Einstellungen $Settings - ProtectedSettings $ProtectedSettings<BR></BR><BR></BR>Führen Sie diesen Befehl mit den entsprechenden [Informationen zur Konfiguration](virtual-machines-windows-extensions-configuration-samples.md) für die Erweiterung, die Sie installieren möchten.
Entfernen einer Erweiterung virtueller Computer | [Entfernen-AzureRmVMExtension](https://msdn.microsoft.com/library/mt603782.aspx) - ResourceGroupName "Resource_group_name"-Vm_name"Name"Extension_name"- VMName"

## <a name="next-steps"></a>Nächste Schritte

- Finden Sie die grundlegenden Schritte zum Erstellen eines virtuellen Computers in [einen Windows virtuellen Computer mithilfe von Ressourcenmanager und PowerShell erstellen](virtual-machines-windows-ps-create.md)aus.


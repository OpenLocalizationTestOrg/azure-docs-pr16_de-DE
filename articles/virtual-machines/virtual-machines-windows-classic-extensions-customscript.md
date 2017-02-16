<properties
   pageTitle="Benutzerdefinierte Skripts Erweiterung auf einen Windows-virtuellen | Microsoft Azure"
   description="Automatisieren von Aufgaben zur Azure-virtuellen Computer mithilfe der Erweiterung benutzerdefiniertes Skript einen remote virtuellen von Windows PowerShell Skripts auszuführen"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="kundanap"
   manager="timlt"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="08/06/2015"
   ms.author="kundanap"/>

# <a name="custom-script-extension-for-windows-virtual-machines"></a>Benutzerdefinierte Skripts-Erweiterung für Windows-virtuellen Computern

Dieser Artikel bietet einen Überblick über die Verwendung die benutzerdefinierte Skripts Erweiterung auf Windows-virtuellen Computern mithilfe von Azure PowerShell-Cmdlets mit Azure Service Management-APIs.

Virtuellen Computern (virtueller Computer) Extensions von Microsoft erstellt und vertrauenswürdige Herausgeber von Drittanbietern, um die Funktionalität der den virtuellen Computer zu erweitern. Übersicht über virtueller Computer Erweiterungen finden Sie unter [Azure-virtuellen Computer Extensions und Features](virtual-machines-windows-extensions-features.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Erfahren Sie, wie [Führen Sie diese Schritte unter Verwendung des Datenmodells Ressourcenmanager](virtual-machines-windows-extensions-customscript.md).

## <a name="custom-script-extension-overview"></a>Benutzerdefinierte Skripts Erweiterung (Übersicht)

Mit der benutzerdefinierte Skript-Erweiterung für Windows können Sie ohne Anmeldung darauf PowerShell-Skripts auf einen remote virtuellen Computer ausführen. Sie können die Skripts nach der Bereitstellung des virtuellen Computer oder zu einem beliebigen Zeitpunkt während des Lebenszyklus von den virtuellen Computer ausführen, ohne das Öffnen von einem beliebigen zusätzlichen Ports. Die am häufigsten verwendeten Fällen wird Ausführen benutzerdefinierter Skripts Erweiterung einschließen ausgeführt wird, installieren und konfigurieren zusätzlichen Software des virtuellen Computers, nachdem sie bereitgestellt wurde.

### <a name="prerequisites-for-running-the-custom-script-extension"></a>Erforderliche Komponenten für die Erweiterung benutzerdefinierte Skripts ausgeführt

1. Installieren von <a href="http://azure.microsoft.com/downloads" target="_blank">Azure PowerShell-Cmdlets</a> Version 0.8.0 oder höher.
2. Wenn Sie die Skripts auf einer vorhandenen virtuellen Computer ausführen möchten, stellen Sie sicher, dass die virtuellen Computer-Agent des virtuellen Computers aktiviert ist. Wenn es nicht installiert ist, führen Sie folgende [Schritte](virtual-machines-windows-classic-agents-and-extensions.md)aus. Wenn Sie der virtuellen Computer vom Azure-Portal erstellt wurde, wird virtueller Computer Agent standardmäßig installiert.
3. Hochladen der Skripts, die in des virtuellen Computers zu Azure-Speicher ausgeführt werden sollen. Die Skripts können aus einem einzelnen Container oder mehrere Speichercontainer stammen.
4. Das Skript sollte erstellt werden, damit das Eintrag Skript, die von der Erweiterung gestartet wird, andere Skripts beginnt.

## <a name="custom-script-extension-scenarios"></a>Benutzerdefinierte Skripts Erweiterung Szenarien

### <a name="upload-files-to-the-default-container"></a>Hochladen von Dateien auf den Standardcontainer

Das folgende Beispiel zeigt, wie Sie Ihre Skripts des virtuellen Computers ausführen können, wenn Sie sich im Speichercontainer des Standardkontos Ihres Abonnements befinden. Laden Sie Ihre Skripts ContainerName hoch. Sie können mithilfe des Standardkontos für den Speicher Überprüfen der **Get-AzureSubscription – Standard** Befehl.

Im folgende Beispiel wird einen virtueller Computer erstellt, aber Sie können auch das gleiche Szenario eines vorhandenen virtuellen Computers ausführen.

    # Create a new VM in Azure.
    $vm = New-AzureVMConfig -Name $name -InstanceSize Small -ImageName $imagename
    $vm = Add-AzureProvisioningConfig -VM $vm -Windows -AdminUsername $username -Password $password
    // Add Custom Script extension to the VM. The container name refers to the storage container that contains the file.
    $vm = Set-AzureVMCustomScriptExtension -VM $vm -ContainerName $container -FileName 'start.ps1'
    New-AzureVM -ServiceName $servicename -Location $location -VMs $vm
    #  After the VM is created, the extension downloads the script from the storage location and executes it on the VM.

    # Viewing the  script execution output.
    $vm = Get-AzureVM -ServiceName $servicename -Name $name
    # Use the position of the extension in the output as index.
    $vm.ResourceExtensionStatusList[i].ExtensionSettingStatus.SubStatusList

### <a name="upload-files-to-a-non-default-storage-container"></a>Hochladen von Dateien auf einer nicht standardmäßigen Speichercontainer

Dieses Szenario veranschaulicht, wie einen nicht standardmäßige Speichercontainer innerhalb des gleichen Abonnements oder in ein anderes Abonnement für Skripts und Dateien hochladen. Dieses Beispiel zeigt eine vorhandene virtueller Computer, aber die gleichen Operationen können während der Erstellung eines virtuellen Computers vorgenommen werden.

        Get-AzureVM -Name $name -ServiceName $servicename | Set-AzureVMCustomScriptExtension -StorageAccountName $storageaccount -StorageAccountKey $storagekey -ContainerName $container -FileName 'file1.ps1','file2.ps1' -Run 'file.ps1' | Update-AzureVM

### <a name="upload-scripts-to-multiple-containers-across-different-storage-accounts"></a>Hochladen von Skripts auf mehrere Container über verschiedene Speicher-Konten

  Wenn die Skriptdateien über mehrere Container gespeichert sind, müssen Sie den freigegebenen Vollzugriff Signaturen (SAS)-URL für die Dateien zum Ausführen der Skripts zu erstellen.

      Get-AzureVM -Name $name -ServiceName $servicename | Set-AzureVMCustomScriptExtension -StorageAccountName $storageaccount -StorageAccountKey $storagekey -ContainerName $container -FileUri $fileUrl1, $fileUrl2 -Run 'file.ps1' | Update-AzureVM


### <a name="add-the-custom-script-extension-from-the-azure-portal"></a>Fügen Sie die benutzerdefinierte Skripts Erweiterung vom Azure-portal

Wechseln Sie zu dem virtuellen Computer im <a href="https://portal.azure.com/ " target="_blank">Azure-Portal</a> , und fügen Sie die Erweiterung durch Ausführen der Skriptdatei angeben.

  ![Skriptdatei angeben][5]


### <a name="uninstall-the-custom-script-extension"></a>Deinstallieren Sie die Erweiterung benutzerdefinierte Skripts

Sie können die Erweiterung benutzerdefiniertes Skript mit dem folgenden Befehl aus dem virtuellen Computer deinstallieren.

      get-azureVM -ServiceName KPTRDemo -Name KPTRDemo | Set-AzureVMCustomScriptExtension -Uninstall | Update-AzureVM

### <a name="use-the-custom-script-extension-with-templates"></a>Verwenden Sie die benutzerdefinierte Skripts Erweiterung mit Vorlagen

Wenn Sie wissen möchten, wie Sie die Erweiterung benutzerdefinierte Skripts mit Azure Ressourcenmanager Vorlagen verwenden, finden Sie unter [benutzerdefinierte Skript-Erweiterung für Windows virtueller Computer mit Azure Ressourcenmanager Vorlagen verwenden](virtual-machines-windows-extensions-customscript.md).

<!--Image references-->
[5]: ./media/virtual-machines-windows-classic-extensions-customscript/addcse.png

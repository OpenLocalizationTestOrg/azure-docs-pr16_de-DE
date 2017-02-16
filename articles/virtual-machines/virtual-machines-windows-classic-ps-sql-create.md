<properties
    pageTitle="Erstellen einer SQL Server-virtuellen Computern in Azure PowerShell (klassisch) | Microsoft Azure"
    description="Enthält Schritte und PowerShell-Skripts zum Erstellen einer Azure-virtuellen Computer mit SQL Server-virtuellen Computern Gallery-Bilder. In diesem Thema wird den Bereitstellung klassischen Modus verwendet."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/07/2016"
    ms.author="jroth" />

# <a name="provision-a-sql-server-virtual-machine-using-azure-powershell-classic"></a>Bereitstellen einer SQL Server-virtuellen Computern mit Azure PowerShell (klassisch)

## <a name="overview"></a>(Übersicht)

In diesem Artikel enthält eine schrittweise Anleitung zum Erstellen einer SQL Server-virtuellen Computern in Azure, mithilfe der PowerShell-Cmdlets.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Der Ressourcenmanager Version dieses Themas finden Sie unter [Bereitstellen einer SQL Server-virtuellen Computern Azure PowerShell Ressourcenmanager verwenden](virtual-machines-windows-ps-sql-create.md).

### <a name="install-and-configure-powershell"></a>Installieren und Konfigurieren von PowerShell:

1. Wenn Sie nicht über ein Azure-Konto verfügen, besuchen Sie [Azure-Testversion frei](https://azure.microsoft.com/pricing/free-trial/).

2. [Herunterladen und installieren die neuesten Azure PowerShell-Befehlen](../powershell-install-configure.md).

3. Starten Sie Windows PowerShell, und verbinden Sie es mit Ihr Abonnement Azure, mit dem Befehl **AzureAccount hinzufügen** .

        Add-AzureAccount

## <a name="determine-your-target-azure-region"></a>Ermitteln des Ziels Azure region

Der SQL Server-virtuellen Computern wird in einen Cloud-Dienst gehostet werden, der einen bestimmten Bereich einer Azure gespeichert ist. Die folgenden Schritte helfen Ihnen anhand Ihrer Region, Speicher-Konto, und cloud-Dienst, der für die restlichen des Lernprogramms verwendet werden soll.

1. Ermitteln Sie das Data Center, das Sie verwenden, um Ihre SQL Server virtueller Computer hosten möchten. Die folgenden PowerShell-Befehle werden die verfügbaren Regionen ausführlich mit einer Zusammenfassung Liste am Ende angezeigt.

        Get-AzureLocation
        (Get-AzureLocation).Name

2.  Wenn Sie die gewünschte Position ermittelt haben, legen Sie eine Variable namens **$dcLocation** für diese Region.

        $dcLocation = "<region name>"

## <a name="set-your-subscription-and-storage-account"></a>Legen Sie Ihr Abonnement und Speicher-Konto

1. Ermitteln des Azure-Abonnements, das für den neuen virtuellen Computer soll verwendet werden.

        (Get-AzureSubscription).SubscriptionName

1. Ihr Ziel Azure-Abonnement der Variablen **$subscr** zuzuweisen. Sie legen Sie dann unter Ihrem aktuellen Azure-Abonnement.

        $subscr="<subscription name>"
        Select-AzureSubscription -SubscriptionName $subscr –Current

1. Aktivieren Sie dann für vorhandene Speicherkonten. Das folgende Skript zeigt alle Speicherkonten, die in der ausgewählten Region vorhanden sind:

        (Get-AzureStorageAccount | where { $_.GeoPrimaryLocation -eq $dcLocation }).StorageAccountName

    >[AZURE.NOTE] Wenn Sie ein neues Speicherkonto benötigen, erstellen Sie zuerst einen Kontonamen alle Kleinbuchstaben Speicher mit dem Befehl neu-AzureStorageAccount wie im folgenden Beispiel gezeigt: **neu-AzureStorageAccount - StorageAccountName "<storage account name>"-Speicherort $dcLocation**

1. Zuweisen der **$staccount**den Namen des Kontos Ziel Speicher hinzu. Klicken Sie dann Formular **Set-AzureSubscription** mit dem Abonnement und dem aktuellen Speicherkonto einrichten.

        $staccount="<storage account name>"
        Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

## <a name="select-a-sql-server-virtual-machine-image"></a>Wählen Sie ein Bild der SQL Server-virtuellen Computern

1. Erfahren Sie die Liste der verfügbaren SQL Server-virtuellen Computern Bilder aus dem Katalog aus. Diese alle Bilder verfügen über eine **ImageFamily** -Eigenschaft, die mit "SQL" beginnt. Die folgende Abfrage enthält die Familie Bild verfügbar für Sie, die SQL Server vorinstalliert aufweisen.

        Get-AzureVMImage | where { $_.ImageFamily -like "SQL*" } | select ImageFamily -Unique | Sort-Object -Property ImageFamily

1. Wenn Sie die Familie virtuellen Computern Bild gefunden haben, könnte es mehrere veröffentlichte Bilder in dieser Familie. Verwenden Sie das folgende Skript, um die neueste veröffentlichten virtuellen Computern Bildnamen für Ihre Familie ausgewähltes Bild (wie etwa **SQL Server 2016 RTM Enterprise unter Windows Server 2012 R2**) zu ermitteln:

        $family="<ImageFamily value>"
        $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

        echo "Selected SQL Server image name:"
        echo "   $image"

## <a name="create-the-virtual-machine"></a>Erstellen des virtuellen Computers

Erstellen Sie schließlich des virtuellen Computers mit PowerShell:

1. Erstellen Sie einen Clouddienst, um den neuen virtuellen Computer zu hosten. Beachten Sie, dass auch möglich ist, können Sie stattdessen einen vorhandenen Clouddienst verwenden. Erstellen Sie eine neue Variable **$svcname** mit den kurzen Namen des Cloud-Dienst an.

        $svcname = "<cloud service name>"
        New-AzureService -ServiceName $svcname -Label $svcname -Location $dcLocation

2. Geben Sie den Namen des virtuellen Computers und eine Größe. Weitere Informationen zu virtuellen Computern Größen finden Sie unter [Virtuellen Computern Größen für Azure](virtual-machines-windows-sizes.md).

        $vmname="<machine name>"
        $vmsize="<Specify one: Large, ExtraLarge, A5, A6, A7, A8, A9, or see the link to the other VM sizes>"
        $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

3. Geben Sie die lokale Administratorkonto und das Kennwort ein.

        $cred=Get-Credential -Message "Type the name and password of the local administrator account."
        $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password

4. Führen Sie das folgende Skript zum Erstellen des virtuellen Computers.

        New-AzureVM –ServiceName $svcname -VMs $vm1

>[AZURE.NOTE] Zusätzliche Erläuterung und Konfigurationsoptionen finden Sie im Abschnitt **Erstellen Ihrer Befehlssatz** in [Azure-PowerShell zu erstellen und Windows-basierten virtuellen Maschinen vorkonfiguriert verwenden](virtual-machines-windows-classic-create-powershell.md).

## <a name="example-powershell-script"></a>Beispiel für PowerShell-Skript

Das folgende Skript bietet und Beispiel für ein vollständiges Skript erstellt, die eine **SQL Server 2016 RTM Enterprise unter Windows Server 2012 R2** virtuellen Computern. Wenn Sie dieses Skript verwenden, müssen Sie die ursprünglichen Variablen basierend auf den vorherigen Schritten in diesem Thema anpassen.

    # Customize these variables based on your settings and requirements:
    $dcLocation = "East US"
    $subscr="mysubscription"
    $staccount="mystorageaccount"
    $family="SQL Server 2016 RTM Enterprise on Windows Server 2012 R2"
    $svcname = "mycloudservice"
    $vmname="myvirtualmachine"
    $vmsize="A5"

    # Set the current subscription and storage account
    # Comment out the New-AzureStorageAccount line if the account already exists
    Select-AzureSubscription -SubscriptionName $subscr –Current
    New-AzureStorageAccount -StorageAccountName $staccount -Location $dcLocation
    Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

    # Select the most recent VM image in this image family:
    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

    # Create the new cloud service; comment out this line if cloud service exists already:
    New-AzureService -ServiceName $svcname -Label $svcname -Location $dcLocation

    # Create the VM config:
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    # Set administrator credentials:
    $cred=Get-Credential -Message "Type the name and password of the local administrator account."
    $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password

    # Create the SQL Server VM:
    New-AzureVM –ServiceName $svcname -VMs $vm1


## <a name="connect-with-remote-desktop"></a>Verbinden mit Remotedesktop

1. Erstellen der. RDP-Dateien in den aktuellen Benutzer Dokumentordner zu diesen virtuellen Computern zu Setup abschließen zu starten:

        $documentspath = [environment]::getfolderpath("mydocuments")
        Get-AzureRemoteDesktopFile -ServiceName $svcname -Name $vmname -LocalPath "$documentspath\vm1.rdp"

1. Starten Sie im Dokumentverzeichnis die RDP-Datei ein. Verbinden mit den Administrator-Benutzernamen und das Kennwort vorher bereitgestellten (z. B. Falls Ihr Benutzername VMAdmin gehandelt hat, geben Sie "\VMAdmin" als der Benutzer an und geben Sie das Kennwort).

        cd $documentspath
        .\vm1.rdp

## <a name="complete-the-configuration-of-the-sql-server-machine-for-remote-access"></a>Schließen Sie die Konfiguration des Computers SQL Server für den Remotezugriff

Konfigurieren Sie nach der Anmeldung an den Computer mit Remotedesktop, SQL Server basierend auf den Anweisungen in den [Schritten zum Konfigurieren von SQL Server-Konnektivität auf eine Azure-virtuellen Computer](virtual-machines-windows-classic-sql-connect.md#steps-for-configuring-sql-server-connectivity-in-an-azure-vm)ein.

## <a name="next-steps"></a>Nächste Schritte

Sie können weitere Anweisungen für die Bereitstellung von virtuellen Computern mit PowerShell in der [Dokumentation virtuellen Computern](virtual-machines-windows-classic-create-powershell.md)suchen. Zusätzliche Skripts im Zusammenhang mit SQL Server und Premium-Speicher finden Sie unter [Verwenden Azure Premium Speicher mit SQL Server auf virtuellen Computern](virtual-machines-windows-classic-sql-server-premium-storage.md).

In vielen Fällen ist im nächste Schritt Migrieren einer Datenbank mit diesem neuen SQL Server virtuellen Computer aus. Hinweise zur Migration der Datenbank finden Sie unter [Migrieren einer Datenbank mit SQL Server ein Azure-virtuellen Computers](virtual-machines-windows-migrate-sql.md).

Wenn Sie auch über das Azure-Portal zum Erstellen von SQL-virtuellen Computern sind, finden Sie in [einer SQL Server virtuellen Computern auf Azure bereitgestellt](virtual-machines-windows-portal-sql-server-provision.md). Beachten Sie, dass das Lernprogramm, das Sie über das Portal geführt virtuellen Computern mit empfohlenen Ressourcenmanager Modell, anstatt die in diesem Thema PowerShell verwendete Klassisch erstellt.

Neben diesen Ressourcen empfehlen wir, dass Sie [Weitere Themen im Zusammenhang mit dem SQL Server in Azure virtuellen Computern ausgeführt](virtual-machines-windows-sql-server-iaas-overview.md)überprüfen.

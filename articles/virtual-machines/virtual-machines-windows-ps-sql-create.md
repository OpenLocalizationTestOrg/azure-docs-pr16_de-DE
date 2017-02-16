<properties
    pageTitle="Erstellen einer SQL Server-virtuellen Computern in Azure PowerShell (Ressourcenmanager) | Microsoft Azure"
    description="Enthält Schritte und PowerShell-Skripts zum Erstellen einer Azure-virtuellen Computer mit SQL Server-virtuellen Computern Gallery-Bilder."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-resource-manager" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/25/2016"
    ms.author="jroth"/>

# <a name="provision-a-sql-server-virtual-machine-using-azure-powershell-resource-manager"></a>Bereitstellen einer SQL Server-virtuellen Computern mit Azure PowerShell (Ressourcen-Manager)

> [AZURE.SELECTOR]
- [Portal](virtual-machines-windows-portal-sql-server-provision.md)
- [PowerShell](virtual-machines-windows-ps-sql-create.md)

## <a name="overview"></a>(Übersicht)

In diesem Lernprogramm erfahren Sie, wie eine einzelne Azure-virtuellen Computern mithilfe des **Azure Ressourcenmanager** Bereitstellung-Modells mithilfe von Azure PowerShell-Cmdlets erstellt. In diesem Lernprogramm erstellen wir eine einzelne virtuellen Computern, die mit einem einzelnen Laufwerk aus einem Bild im Katalog SQL. Wir erstellen neue Anbieter für den Speicher, Netzwerk und Berechnen von Ressourcen, die vom virtuellen Computer verwendet werden. Wenn Sie vorhandene Anbieter nach jedem der folgenden Ressourcen verfügen, können Sie stattdessen die Anbieter verwenden.

Wenn Sie die klassische Version dieses Themas benötigen, finden Sie unter [Bereitstellen einer SQL Server virtuellen Computern Azure PowerShell klassischen verwenden](virtual-machines-windows-classic-ps-sql-create.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Für dieses Lernprogramms müssen Sie:

- Ein Azure-Konto und Abonnements, bevor Sie beginnen. Wenn Sie eine, melden Sie sich für eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/)besitzen.
- [Azure PowerShell)](../powershell-install-configure.md), Mindestversion von 1.4.0 oder höher (dieses Lernprogramms geschrieben Version 1.5.0 verwenden).
    - Geben Sie zum Abrufen von Ihrer Version **Get-Modul Azure-ListAvailable**aus.

## <a name="configure-your-subscription"></a>Konfigurieren Sie Ihr Abonnement

Öffnen Sie Windows PowerShell und richten Sie Zugriff auf Ihr Azure-Konto ein, indem Sie das folgende Cmdlet ausgeführt. Es wird mit einer Anmeldebildschirm Ihre Anmeldeinformationen eingeben angezeigt. Verwenden Sie den gleichen e-Mail- und das Kennwort, mit denen Sie Azure-Portal anmelden.

    Add-AzureRmAccount

Nach der erfolgreichen Anmeldung finden Sie einige Informationen auf dem Bildschirm, die die SubscriptionId enthält, mit denen Sie sich angemeldet. Dies ist das Abonnement, in dem die Ressourcen in diesem Lernprogramm erstellt werden, es sei denn, Sie in ein anderes Abonnement ändern. Wenn Sie mehrere SubscriptionIds haben, führen Sie das folgende Cmdlet aus, um eine Liste aller Ihrer SubscriptionIds zurückgegeben:

    Get-AzureRmSubscription

Um zu einem anderen SubscriptionID ändern möchten, führen Sie das folgende Cmdlet mit der gewünschten SubscriptionId aus.

    Select-AzureRmSubscription -SubscriptionId xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

## <a name="define-image-variables"></a>Definieren von Variablen Bild

Zur Vereinfachung der Nutzbarkeit und das Verständnis der fertigen Skript aus diesem Lernprogramm beginnen wir mit dem eine Anzahl von Variablen definieren. Ändern Sie die gewünschten Parameterwerte, wie Sie finden Sie unter anpassen, allerdings nicht für die Benennung von Einschränkungen im Zusammenhang mit dem Namen Länge und Sonderzeichen beim Ändern der Werte liegen.

### <a name="location-and-resource-group"></a>Speicherort und Ressourcengruppe
Verwenden Sie zwei Variablen zum Definieren des Datenbereichs und der Ressourcengruppe, in der Sie die anderen Ressourcen für den virtuellen Computer erstellen möchten.

Wie gewünscht ändern Sie, und führen Sie die folgenden Cmdlets zur Initialisierung dieser Variablen.

    $Location = "SouthCentralUS"
    $ResourceGroupName = "sqlvm1"

### <a name="storage-properties"></a>Eigenschaften von Speicher

Verwenden Sie die folgenden Variablen zum Definieren von Speicher-Konto und den Typ der Speicher von virtuellen Computers verwendet werden soll.

Wie gewünscht ändern Sie, und führen Sie das folgende Cmdlet aus, um diese Variablen Initialisierung. Beachten Sie, dass in diesem Beispiel wir [Premium Speicher](../storage/storage-premium-storage.md)verwenden, die für Produktionsarbeitslasten empfohlen wird. Informationen zu diesem Leitfaden und anderen Empfehlungen, finden Sie unter [Leistung bewährte Methoden für SQL Server in Azure virtuellen Computern](virtual-machines-windows-sql-performance.md).

    $StorageName = $ResourceGroupName + "storage"
    $StorageSku = "Premium_LRS"

### <a name="network-properties"></a>Netzwerk-Eigenschaften

Verwenden Sie die folgenden Variablen zum Definieren von der Schnittstelle, die Methode der Verteilung TCP/IP, den Netzwerknamen des virtuellen, den Subnetnamen des virtuellen, den Bereich der IP-Adressen für das virtuelle Netzwerk, die von IP-Adressen für das Subnetz und die Bezeichnung für Öffentliche Domäne von im Netzwerk des virtuellen Computers verwendet werden soll.

Wie gewünscht ändern Sie, und führen Sie das folgende Cmdlet aus, um diese Variablen Initialisierung.

    $InterfaceName = $ResourceGroupName + "ServerInterface"
    $TCPIPAllocationMethod = "Dynamic"
    $VNetName = $ResourceGroupName + "VNet"
    $SubnetName = "Default"
    $VNetAddressPrefix = "10.0.0.0/16"
    $VNetSubnetAddressPrefix = "10.0.0.0/24"
    $DomainName = "sqlvm1"   

### <a name="virtual-machine-properties"></a>Eigenschaften von virtuellen Computern

Verwenden Sie die folgenden Variablen zum Definieren von Name des virtuellen Computers, den Namen des Computers, die Größe des virtuellen Computers und den Namen des Betriebssystems Festplatte des virtuellen Computers.

Wie gewünscht ändern Sie, und führen Sie das folgende Cmdlet aus, um diese Variablen Initialisierung.

    $VMName = $ResourceGroupName + "VM"
    $ComputerName = $ResourceGroupName + "Server"
    $VMSize = "Standard_DS13"
    $OSDiskName = $VMName + "OSDisk"

### <a name="image-properties"></a>Bildeigenschaften

Verwenden Sie die folgenden Variablen, um das Bild des virtuellen Computers verwenden definieren. In diesem Beispiel wird das Bild SQL Server 2016 Enterprise verwendet.

Wie gewünscht ändern Sie, und führen Sie das folgende Cmdlet aus, um diese Variablen Initialisierung.

    $PublisherName = "MicrosoftSQLServer"
    $OfferName = "SQL2016-WS2016"
    $Sku = "Enterprise"
    $Version = "latest"

Beachten Sie, dass Sie eine vollständige Liste der SQL Server-Bild Angebote mit dem Befehl Get-AzureRmVMImageOffer zugreifen können:

    Get-AzureRmVMImageOffer -Location 'East US' -Publisher 'MicrosoftSQLServer'

Und Sie den Artikel, der für ein Angebot mit dem Befehl Get-AzureRmVMImageSku verfügbar angezeigt werden. Mit dem folgende Befehl zeigt, dass alle Artikel, die für die **SQL2014SP1-WS2012R2** verfügbar anbieten.

    Get-AzureRmVMImageSku -Location 'East US' -Publisher 'MicrosoftSQLServer' -Offer 'SQL2014SP1-WS2012R2' | Select Skus

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Mit dem Bereitstellungsmodell Ressourcenmanager ist das erste Objekt, das Sie erstellen die Ressourcengruppe an. Erstellen einer Azure Ressourcengruppe und seine Ressourcen mit Ressourcen Gruppenname und Speicherort, die durch die Variablen, die Sie zuvor Initialisierung definiert, verwenden Sie das Cmdlet " [New-AzureRmResourceGroup](https://msdn.microsoft.com/library/mt759837.aspx) ".

Führen Sie das folgende Cmdlet aus, um die neue Ressourcengruppe zu erstellen.

    New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location

## <a name="create-a-storage-account"></a>Erstellen Sie ein Speicherkonto

Des virtuellen Computers ist Speicherressourcen für den Datenträger Betriebssystem und für die Daten und Protokolldateien SQL Server erforderlich. Zur Vereinfachung erstellen wir eine Festplatte für beide. Sie können weitere Datenträger später mithilfe des [Hinzufügen Azure Datenträger](https://msdn.microsoft.com/library/azure/dn495252.aspx) -Cmdlets und platzieren Sie die SQL Server-Daten und Protokolldateien auf dem Datenträger anfügen. Erstellen eines standard-Speicher-Kontos aus, in die neue Ressourcengruppe und mit Speicher Kontonamen, Speicher Sku Name und Speicherort die Variablen verwenden, die Sie zuvor Initialisierung definiert, verwenden Sie das Cmdlet " [New-AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) ".

Führen Sie das folgende Cmdlet aus, um Ihr neues Speicherkonto zu erstellen.  

    $StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageName -SkuName $StorageSku -Kind "Storage" -Location $Location

## <a name="create-network-resources"></a>Erstellen von Netzwerk-Ressourcen

Des virtuellen Computers erfordert eine Reihe von Netzwerk-Ressourcen für Netzwerkkonnektivität an.

- Jeder virtuelle Computer erfordert ein virtuelles Netzwerk.
- Ein virtuelles Netzwerk müssen mindestens ein Subnetz definiert.
- Netzwerk-Schnittstellen muss mithilfe einer öffentlichen oder einer privaten IP-Adresse definiert werden.

### <a name="create-a-virtual-network-subnet-configuration"></a>Erstellen einer virtuellen Netzwerkkonfiguration Subnetz

Zunächst wird durch Erstellen einer Subnetz Konfigurations für unsere virtuelle Netzwerk. Für unsere Lernprogramm erstellen wir eine Standard-Subnetz mithilfe des Cmdlets [New-AzureRmVirtualNetworkSubnetConfig](https://msdn.microsoft.com/library/mt619412.aspx) . Wir werden unsere virtuelle Netzwerkkonfiguration Subnetz erstellen, mit dem Subnetz Namens- und Präfix die Variablen verwenden, die Sie zuvor Initialisierung definiert.

>[AZURE.NOTE] Sie können zusätzliche Eigenschaften mit diesem Cmdlet virtuelle Subnetz Netzwerkkonfiguration definieren, aber, die im Rahmen dieses Lernprogramms ist.

Führen Sie das folgende Cmdlet aus, um Ihre virtuelle Subnetzkonfiguration zu erstellen.

    $SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix

### <a name="create-a-virtual-network"></a>Erstellen Sie ein virtuelles Netzwerk

Als Nächstes erstellen wir unsere virtuelle Netzwerk mithilfe des Cmdlets [New-AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt603657.aspx) . Erstellen wir unsere virtuelle Netzwerk in Ihrer neuen Ressourcengruppe, mit dem Namen, den Speicherort und die Adresse Präfix definiert die Variablen, die Sie zuvor Initialisierung und Verwenden der Subnetz-Konfigurations, die Sie im vorherigen Schritt definiert.

Führen Sie das folgende Cmdlet zum Erstellen von virtuellen Netzwerks ein.

    $VNet = New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName -Location $Location -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig

### <a name="create-the-public-ip-address"></a>Erstellen der öffentlichen IP-Adresse

Jetzt, da wir unsere virtuelle Netzwerk definiert haben, müssen wir eine IP-Adresse für eine Verbindung zu des virtuellen Computers zu konfigurieren. In diesem Lernprogramm erstellen wir eine öffentliche IP-Adresse mit dynamischen IP-Adressen Internet Connectivity unterstützt. Erstellen die öffentliche IP-Adresse aus, in der Ressourcengruppe Prevously erstellt und mit den Namen, Speicherort, Verteilungsmethode und DNS-Namen des Domänennamens mit der Variablen, die Sie zuvor Initialisierung definiert, verwenden Sie das Cmdlet " [New-AzureRmPublicIpAddress](https://msdn.microsoft.com/library/mt603620.aspx) ".

>[AZURE.NOTE] Sie können zusätzliche Eigenschaften der öffentlichen IP-Adresse, die mit diesem Cmdlet definieren, aber ist, die nicht Gegenstand dieses Lernprogramms initial. Sie können auch eine private Adresse oder eine Adresse mit einer statischen Adresse erstellen, aber das ist auch im Rahmen dieses Lernprogramms.

Führen Sie das folgende Cmdlet aus, um Ihre öffentliche IP-Adresse zu erstellen.

    $PublicIp = New-AzureRmPublicIpAddress -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName

### <a name="create-the-network-interface"></a>Erstellen der Schnittstelle

Wir können nun die Netzwerk-Benutzeroberfläche zu erstellen, die unsere virtuellen Computern verwendet wird. Um unsere Netzwerk-Benutzeroberfläche in der früheren und mit Name, Position, Subnetz und öffentliche IP-Adresse, die zuvor definierte erstellt Ressourcengruppe zu erstellen, verwenden Sie das Cmdlet " [New-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619370.aspx) ".

Führen Sie das folgende Cmdlet aus, um Ihre Netzwerk-Oberfläche zu erstellen.

    $Interface = New-AzureRmNetworkInterface -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id

## <a name="configure-a-vm-object"></a>Konfigurieren eines virtuellen Computer-Objekts

Jetzt, da wir Speicher und Netzwerk-Ressourcen definiert haben, können wir berechnen Ressourcen des virtuellen Computers zu definieren. Für unsere Lernprogramm wird wir die Größe des virtuellen Computers und verschiedene Betriebssystemeigenschaften festlegen, die Schnittstelle, die wir zuvor erstellt haben, definieren Blob-Speicher, und geben Sie dann auf den Datenträger Betriebssystem.

### <a name="create-the-vm-object"></a>Erstellen Sie das Objekt virtueller Computer

Zunächst wird die Größe des virtuellen Computers angeben. In diesem Lernprogramm werden wir eine DS13 angeben. Erstellen ein Objekt konfigurierbare virtuellen Computern mit dem Namen und die Größe die Variablen verwenden, die Sie zuvor Initialisierung definiert, verwenden Sie das Cmdlet " [New-AzureRmVMConfig](https://msdn.microsoft.com/library/mt603727.aspx) ".

Führen Sie das folgende Cmdlet aus, um das Objekt des virtuellen Computers zu erstellen.

    $VirtualMachine = New-AzureRmVMConfig -VMName $VMName -VMSize $VMSize

### <a name="create-a-credential-object-to-hold-the-name-and-password-for-the-local-administrator-credentials"></a>Erstellen Sie ein Anmeldeinformationsobjekt, um den Namen und das Kennwort für die Anmeldeinformationen für lokale Administratoren halten

Bevor wir die Betriebssystemeigenschaften des virtuellen Computers festlegen können, müssen wir die Anmeldeinformationen für das lokale Administratorkonto als eine sichere Zeichenfolge angeben. Um dies zu erreichen, verwenden wir das Cmdlet " [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) " aus.

Führen Sie das folgende Cmdlet und im Windows PowerShell Anmeldeinformationen anfordern, geben Sie den Namen und das Kennwort für das lokale Administratorkonto in die Windows-Computer verwendet werden soll.

    $Credential = Get-Credential -Message "Type the name and password of the local administrator account."

### <a name="set-the-operating-system-properties-for-the-virtual-machine"></a>Festlegen der Eigenschaften Betriebssystem des virtuellen Computers

Nun können wir des virtuellen Computers Betriebssystemeigenschaften festlegen. Wir verwenden das Cmdlet " [Set-AzureRmVMOperatingSystem](https://msdn.microsoft.com/library/mt603843.aspx) ", um den Typ des Betriebssystems als Windows festgelegt ist, der [virtuellen Computern Agents](virtual-machines-windows-classic-agents-and-extensions.md) installiert werden, angeben, dass das Cmdlet AutoUpdate ermöglicht und Festlegen der Name des virtuellen Computers, den Namen des Computers und die Verwendung der Variablen, die Sie zuvor Initialisierung Anmeldeinformationen erforderlich.

Führen Sie das folgende Cmdlet aus, um die: das Betriebssystem von Eigenschaften für den virtuellen Computer festzulegen.

    $VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate

### <a name="add-the-network-interface-to-the-virtual-machine"></a>Hinzufügen der Schnittstelle virtuellen Computer

Als Nächstes werden wir um hinzuzufügen die Netzwerkschnittstelle, die wir erstellt haben zuvor des virtuellen Computers. Hinzufügen die Schnittstelle mithilfe der Netzwerk-Benutzeroberflächen-Variable, die Sie zuvor definiert, verwenden Sie das Cmdlet [AzureRmVMNetworkInterface hinzufügen](https://msdn.microsoft.com/library/mt619351.aspx) .

Führen Sie das folgende Cmdlet aus, um die Netzwerk-Benutzeroberfläche für Ihre virtuellen Computern festzulegen.

    $VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id

### <a name="set-the-blob-storage-location-for-the-disk-to-be-used-by-the-virtual-machine"></a>Legen Sie den BLOB-Speicher für den Datenträger von des virtuellen Computers verwendet werden soll

Als Nächstes werden wir den Blob-Speicherort für den Datenträger verwendet werden, indem Sie die Verwendung der Variablen, die Sie zuvor definiert virtuellen Computern festlegen.

Führen Sie das folgende Cmdlet aus, um den Speicherort der Blob-Speicher festzulegen.

    $OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"

### <a name="set-the-operating-system-disk-properties-for-the-virtual-machine"></a>Festlegen des Betriebssystems Datenträgereigenschaften des virtuellen Computers

Als Nächstes werden wir das Betriebssystem Datenträgereigenschaften des virtuellen Computers festlegen. Angeben, dass das Betriebssystem des virtuellen Computers aus einem Bild zum Zwischenspeichern kommen um schreibgeschützt, (weil SQL Server auf derselben Festplatte installiert wird) festlegen und definieren Name des virtuellen Computers und dem Betriebssystem Datenträger mithilfe der Variablen, die wir zuvor definiert definiert werden, verwenden Sie das Cmdlet " [Set-AzureRmVMOSDisk](https://msdn.microsoft.com/library/mt603746.aspx) ".

Führen Sie das folgende Cmdlet aus, um das Betriebssystem Datenträgereigenschaften für Ihre virtuellen Computern festzulegen.

    $VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage

### <a name="specify-the-platform-image-for-the-virtual-machine"></a>Geben Sie das Bild Plattform des virtuellen Computers

Unsere letzte Konfigurationsschritt ist das Bild Plattform für unsere virtuellen Computern an. Unsere Lernprogramm verwenden das neueste SQL Server 2016 CTP Bild wir. Diese Abbildung verwenden, wie Sie durch die Variablen definiert sind, die Sie zuvor definiert, verwenden Sie das Cmdlet " [Set-AzureRmVMSourceImage](https://msdn.microsoft.com/library/mt619344.aspx) ".

Führen Sie das folgende Cmdlet aus, um das Bild Plattform für den virtuellen Computer anzugeben.

    $VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName $PublisherName -Offer $OfferName -Skus $Sku -Version $Version

## <a name="create-the-sql-vm"></a>Erstellen Sie den SQL-virtuellen Computer

Jetzt, da Sie die Konfigurationsschritte abgeschlossen haben, sind Sie bereit zum Erstellen des virtuellen Computers. Erstellen des virtuellen Computers mit den Variablen, die wir definiert haben, verwenden Sie das Cmdlet " [New-AzureRmVM](https://msdn.microsoft.com/library/mt603754.aspx) ".

Führen Sie das folgende Cmdlet aus, um Ihre virtuellen Computers zu erstellen.

    New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine

Des virtuellen Computers wird erstellt. Beachten Sie, dass ein standard Speicherkonto für Boot Diagnose erstellt wird, da die angegebene Größe für die Festplatte des virtuellen Computers zu berücksichtigen ist ein Premium Speicher-Konto an.

Sie können nun diesem Computer im Azure-Portal zu [dessen öffentliche IP-Adresse und den vollqualifizierten Domänennamen](virtual-machines-windows-portal-sql-server-provision.md#Connect)finden Sie unter anzeigen.  

## <a name="example-script"></a>Beispielskript

Das folgende Skript enthält das vollständige PowerShell-Skript für dieses Lernprogramm. Es wird vorausgesetzt, dass Sie bereits einrichten das Azure-Abonnement verwenden Sie die Befehle **Hinzufügen-AzureRmAccount** und **AzureRmSubscription auswählen** .


    # Variables
    ## Global
    $Location = "SouthCentralUS"
    $ResourceGroupName = "sqlvm1"
    ## Storage
    $StorageName = $ResourceGroupName + "storage"
    $StorageSku = "Premium_LRS"

    ## Network
    $InterfaceName = $ResourceGroupName + "ServerInterface"
    $VNetName = $ResourceGroupName + "VNet"
    $SubnetName = "Default"
    $VNetAddressPrefix = "10.0.0.0/16"
    $VNetSubnetAddressPrefix = "10.0.0.0/24"
    $TCPIPAllocationMethod = "Dynamic"
    $DomainName = "sqlvm1"

    ##Compute
    $VMName = $ResourceGroupName + "VM"
    $ComputerName = $ResourceGroupName + "Server"
    $VMSize = "Standard_DS13"
    $OSDiskName = $VMName + "OSDisk"

    ##Image
    $PublisherName = "MicrosoftSQLServer"
    $OfferName = "SQL2016-WS2016"
    $Sku = "Enterprise"
    $Version = "latest"

    # Resource Group
    New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location

    # Storage
    $StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageName -SkuName $StorageSku -Kind "Storage" -Location $Location

    # Network
    $SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix
    $VNet = New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName -Location $Location -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig
    $PublicIp = New-AzureRmPublicIpAddress -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName
    $Interface = New-AzureRmNetworkInterface -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id

    # Compute
    $VirtualMachine = New-AzureRmVMConfig -VMName $VMName -VMSize $VMSize
    $Credential = Get-Credential -Message "Type the name and password of the local administrator account."
    $VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate #-TimeZone = $TimeZone
    $VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id
    $OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"
    $VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage

    # Image
    $VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName $PublisherName -Offer $OfferName -Skus $Sku -Version $Version

    ## Create the VM in Azure
    New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine

## <a name="next-steps"></a>Nächste Schritte
Nach der Erstellung des virtuellen Computers sind Sie bereit sind, die Verbindung des virtuellen Computers RDP und Setup-Konnektivität verwenden. Weitere Informationen finden Sie unter [Verbinden zu einer SQL Server virtuellen Computern auf Azure (Ressourcen-Manager)](virtual-machines-windows-sql-connect.md).

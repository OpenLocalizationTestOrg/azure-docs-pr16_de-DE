<properties
    pageTitle="Verwenden von Azure Premium Speicher mit SQLServer | Microsoft Azure"
    description="In diesem Artikel erstellte, mit dem Bereitstellungsmodell klassischen Ressourcen verwendet und enthält eine Anleitung zum Verwenden von Azure Premium Speicher mit SQL Server auf Azure virtuellen Computern ausgeführt."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="danielsollondon"
    manager="jhubbard"
    editor="monicar"    
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="08/19/2016"
    ms.author="jroth"/>

# <a name="use-azure-premium-storage-with-sql-server-on-virtual-machines"></a>Verwenden von Azure Premium Speicher mit SQLServer auf virtuellen Computern


## <a name="overview"></a>(Übersicht)

[Azure Premium Speicher](../storage/storage-premium-storage.md) ist die nächste Generation von Storage, Niedrig Wartezeit und hohen Durchsatz EA bereitstellt. Es funktioniert am besten für Key EA stark Auslastung, wie etwa SQL Server auf IaaS [virtuellen Computern](https://azure.microsoft.com/services/virtual-machines/).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


In diesem Artikel bietet Planung und bei der Migration eines virtuellen Computers mit SQL Server, Premium Speicher zu verwenden. Dies umfasst Azure Infrastruktur (Netzwerke, Speicher) und Gast virtuellen Windows-Computer vor. Das Beispiel in der [Anlage](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage) zeigt eine vollständige umfassende durchgehende Migration zum Verschieben größere virtuelle Computer verbesserte lokale SSD Speicherung mit PowerShell nutzen.

Es ist wichtig, den End-to-End-Prozess die Nutzung Azure Premium-Speicher mit SQL Server auf virtuellen Computern die IAAS zu verstehen. Dies umfasst:

- Kennung Komponenten, Premium Speicher zu verwenden.
- Beispiele für Bereitstellung von SQL Server auf IaaS zu Premium-Speicher für neue Bereitstellungen.
- Beispiele für Migrieren von vorhandenen Bereitstellungen sowohl eigenständigen Servern und Bereitstellungen Entwurfsphase SQL immer auf Verfügbarkeit.
- Mögliche Migration Ansätze.
- Vollständige End-to-End-Beispiel mit Azure, Windows und SQL Server Schritte für die Migration einer vorhandenen Implementierung immer auf.

Weitere Hintergrundinformationen auf SQL Server in Azure virtuellen Computern finden Sie unter [SQL Server in Azure virtuellen Computern](virtual-machines-windows-sql-server-iaas-overview.md).

**Autor:** Daniel Sol **technische Prüfer:** Luis Carlos Vargas Hering Sanjay Mishra, Pravin Mital, Juergen Thomas, Gonzalo Ruiz.

## <a name="prerequisites-for-premium-storage"></a>Erforderliche Komponenten für Premium-Speicher

Es gibt mehrere Voraussetzung für die Verwendung von Premium-Speicher.

### <a name="machine-size"></a>Computer-Größe

Für die Verwendung von Premium-Speicher müssen Sie DS Serie virtueller virtuellen Computern (Computer) zu verwenden. Wenn Sie nicht in der Cloud-Dienst vor dem DS Serie Autos verwendet haben, müssen Sie den vorhandenen virtuellen Computer löschen, halten Sie die angefügten Datenträger und erstellen Sie dann einen neuen Clouddienst vor den virtuellen Computer als DS * Rolle Größe, neu zu erstellen. Weitere Informationen zu virtuellen Computern Größen finden Sie unter [virtuellen Computern und Cloud-Dienst Größen für Azure](virtual-machines-linux-sizes.md).

### <a name="cloud-services"></a>Cloud-Dienste

Sie können nur DS * virtuellen Computern mit Premium-Speicher verwenden, wenn sie in einem neuen Clouddienst erstellt werden. Wenn Sie SQL Server immer auf in Azure verwenden, bezieht die immer auf Zuhörer Azure internen oder externen laden Lastenausgleich IP-Adresse sich auf, die einen Clouddienst zugeordnet ist. Dieser Artikel befasst sich wie bei weitgehender Verfügbarkeit in diesem Szenario migrieren.

> [AZURE.NOTE] Eine Reihe von DS * muss sich auf den ersten virtuellen Computer aus, die in der neuen Cloud-Dienst bereitgestellt wird.

### <a name="regional-vnets"></a>Landes-/ VNETS

Für DS * virtuelle Computer müssen Sie die Hostinganbieter Ihrer virtuellen Computern benutzerspezifisch Landes-/ virtuelle Netzwerk (VNET) konfigurieren. Dies "Erweitert" die VNET besteht darin, das größere virtuelle Computer in andere Cluster bereitgestellt werden und zulassen der Kommunikation zwischen ihnen ermöglichen. In der folgenden Abbildung zeigt, wo hervorgehobenen Landes-/ VNETs, während das erste Ergebnis eines "schmale" VNET anzeigt.

![RegionalVNET][1]

Sie können eine Microsoft-Support-Ticket zum Migrieren zu einer regionalen VNET heraufstufen, Microsoft wird eine Änderung, und klicken Sie dann zum Abschließen der Migrations zu regionalen VNETs, ändern Sie die Eigenschaft AffinityGroup in der Netzwerkkonfiguration. Zuerst exportieren der PowerShell-Netzwerkkonfiguration, und Ersetzen Sie die Eigenschaft **AffinityGroup** im **VirtualNetworkSite** -Element mit einer Eigenschaft **Position** . Geben Sie `Location = XXXX` , in dem `XXXX` ist ein Azure Region. Klicken Sie dann importieren Sie die neue Konfiguration ein.

Erwägen beispielsweise die folgende VNET Konfiguration:

    <VirtualNetworkSite name="danAzureSQLnet" AffinityGroup="AzureSQLNetwork">
    <AddressSpace>
      <AddressPrefix>10.0.0.0/8</AddressPrefix>
      <AddressPrefix>172.16.0.0/12</AddressPrefix>
    </AddressSpace>
    <Subnets>
    ...
    </VirtualNetworkSite>

Um dies zu einer regionalen VNET in Europa Westen bis zu verschieben, ändern Sie die Konfiguration der folgende aus:

    <VirtualNetworkSite name="danAzureSQLnet" Location="West Europe">
    <AddressSpace>
      <AddressPrefix>10.0.0.0/8</AddressPrefix>
      <AddressPrefix>172.16.0.0/12</AddressPrefix>
    </AddressSpace>
    <Subnets>
    ...
    </VirtualNetworkSite>

### <a name="storage-accounts"></a>Speicherkonten

Sie müssen ein neues Speicherkonto zu erstellen, das für die Speicherung Premium konfiguriert ist. Beachten Sie, dass die Verwendung von Premium Speicher am Speicher-Konto, nicht auf einzelne virtuelle Festplatten, festgelegt ist, jedoch beim DS * Reihe virtueller des virtuellen Festplatte aus Premium und Standard-Speicher Konten anfügen können. Sie sollten dies, wenn Sie nicht die OS virtuelle Festplatte dem Konto Premium Speicher platzieren möchten.

Der folgende Befehl aus **Neu-AzureStorageAccountPowerShell** mit dem **Typ** des "Premium_LRS" erstellt ein Premium Speicher-Konto an:

    $newstorageaccountname = "danpremstor"
    New-AzureStorageAccount -StorageAccountName $newstorageaccountname -Location "West Europe" -Type "Premium_LRS"   

### <a name="vhds-cache-settings"></a>Einstellungen des Caches für virtuelle Festplatten

Der wichtigste Unterschied zwischen dem Erstellen von Datenträger, die Teil eines Kontos Premium Speicher sind, ist der Datenträger-Cache-Einstellung. Für SQL Server-Daten Lautstärke Datenträger empfiehlt es sich, dass Sie '**Zwischenspeichern gelesen**' verwenden. Für Transaktion Log Datenmengen sollte die Datenträger-Cache-Einstellung '**keine**' festgelegt werden. Dies unterscheidet sich von der Empfehlungen für Standard-Speicher-Konten.

Sobald die virtuelle Festplatten angefügt wurden, kann die Einstellung Cache geändert werden. Sie müssten zu trennen, und fügen Sie die virtuelle Festplatte mit einer Einstellung aktualisierten Cache wieder.

### <a name="windows-storage-spaces"></a>Windows-Speicher Leerzeichen

[Windows-Speicher Leerzeichen](https://technet.microsoft.com/library/hh831739.aspx) können wie beim vorherigen Standard-Speicher, können Sie einen virtuellen Computer migrieren, die bereits Speicher Leerzeichen genutzt wird. Im Beispiel in [Anhang](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage) (Schritt 9 und Weiterleiten) veranschaulicht den Powershell-Code zum Extrahieren und Importieren eines virtuellen Computers mit mehreren angefügten virtuellen Festplatten.

Speicherpools wurden mit Standard Azure-Speicher-Konto verwendet, um verbessern Durchsatz und Wartezeit zu verringern. Unter Umständen Wert Speicherpools mit Premium-Speicher für neue Bereitstellungen testen, aber können diese zusätzliche Komplexität Zeichnungsblatts Speicher hinzufügen.

#### <a name="how-to-find-which-azure-virtual-disks-map-to-storage-pools"></a>So suchen Sie welche Azure-virtuellen Laufwerke zugeordnet Speicherpools

Wie es verschiedene Cache Einstellung Empfehlungen für angefügten virtuellen Festplatten gibt, empfiehlt es sich, die virtuellen Festplatten mit einem Konto Premium Speicher zu kopieren. Wenn Sie für die neue DS Serie virtueller Computer erneut hinzuzufügen, müssen Sie die Einstellungen des Caches ändern. Es ist einfacher, die Einstellungen des Caches empfohlen, wenn Sie eine separate virtuelle Festplatten für die SQL-Datendateien und Protokolldateien (anstelle einer einzelnen virtuellen, die beide enthält) haben Premium Speicher anwenden.

> [AZURE.NOTE] Wenn Sie die SQL Server-Daten und der Protokolldateien Dateien auf demselben Volume verfügen, hängt die EA Access Muster bei Ihrer Datenbanken die Option zum Zwischenspeichern ausgewählt haben. Nur testen kann vorführen die Option zum Zwischenspeichern für dieses Szenario am besten geeignet ist.

Wenn Sie Windows-Speicher Leerzeichen die von mehreren virtuellen Festplatten bestehen verwenden Sie eigenständig müssen Ihrer ursprünglichen Skripts zu identifizieren, die virtuellen Festplatten angefügt sind jedoch in welche bestimmten Pool, also Sie dann die Einstellungen des Caches festlegen können entsprechend für jeden Datenträger.

Wenn Sie keinen ursprünglichen Skripts zur Verfügung, um Ihnen zu zeigen virtuellen Festplatten, die dem Speicherpool zugeordnet sind, können Sie die folgenden Schritte aus, um die Zuordnung Festplattenspeicher/Ressourcenpool zu bestimmen.

Gehen Sie für jedes Laufwerk folgendermaßen vor:

1. Erhalten Sie Liste der Datenträger an virtuellen Computer angeschlossen ist, mit dem Befehl **Get-AzureVM** :

    Get-AzureVM - ServiceName <servicename> -Namen <vmname> | Get-AzureDataDisk

1. Beachten Sie die Diskname und LUN.

    ![DisknameAndLUN][2]

1. Remotedesktop in den virtuellen Computer. Wechseln Sie dann zu **Computer Management** | **Geräte-Manager** | **Laufwerke**. Schauen Sie sich die Eigenschaften der einzelnen 'Microsoft virtuelle Datenträger'

    ![VirtualDiskProperties][3]

1. Die Anzahl von LUN hier ist ein Verweis auf die LUN-Nummer, die Sie angeben, wenn die virtuelle Festplatte auf dem virtuellen Computer anfügen.
1. Für das 'Microsoft virtuelle Laufwerk' auf der Registerkarte **Details** wechseln, klicken Sie dann in der Liste **Eigenschaft** , wechseln Sie zu **Treiber-Taste**. Beachten Sie der **Wert** **versetzt**, welche 0002 in den folgenden Screenshot ist. Die 0002 kennzeichnet die PhysicalDisk2, die der Speicherpool verweist.

    ![VirtualDiskPropertyDetails][4]

2. Für jede Speicherpool zu sichern, die zugehörigen Datenträger:

    Get-StoragePool - FriendlyName AMS1pooldata | Get-PhysicalDisk

    ![GetStoragePool][5]

Mittlerweile angefügt diese Informationen für die zuordnen virtueller Festplatten auf physische Datenträger in Speicherpools.

Nachdem Sie virtuelle Festplatten auf physische Datenträger in Speicherpools zugeordnet haben, die können Sie dann trennen und kopieren Sie diese mit einem Konto Premium Speicher, fügen Sie diese über die richtige Cache-Einstellung. Wenden Sie das Beispiel in der [Anlage](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage), Schritte 8 bis 12. Schritte anzeigen zum Extrahieren einer Konfigurations virtueller Computer angeschlossen virtuelle Festplatte Datenträger in eine CSV-Datei, kopieren die virtuellen Festplatten, ändern die Einstellungen des Caches für Datenträger Konfiguration und klicken Sie abschließend erneut den virtuellen Computer als DS Serie virtueller Computer mit alle angefügten Datenträger bereitstellen

### <a name="vm-storage-bandwidth-and-vhd-storage-throughput"></a>Virtueller Computer Speicher Bandbreite und Speicherdurchsatz virtuelle Festplatte

Die Menge an Speicherplatz Leistung hängt davon ab, die angegebene Größe der DS * VM und die virtuelle Festplatte Größen. Der virtuelle Computer haben die maximale Bandbreite (MB/s) unterstützten und andere softwarebegrenzungen für die Anzahl der virtuelle Festplatten, die angefügt werden kann. Die Zahlen bestimmte Bandbreite finden Sie unter [virtuellen Computern und Cloud-Dienst Größen für Azure](virtual-machines-linux-sizes.md).

Höhere IOPS, die mit dem Datenträger größere erzielt werden. Dies ist sollten, wenn Sie den Migrationspfad für die anzustellen. Weitere Informationen [finden Sie in der Tabelle IOPS und Datenträgertypen](../storage-premium-storage.md#scalability-and-performance-targets-when-using-premium-storage).

Sollten Sie abschließend, dass virtuelle Computer anderen maximale Datenträger Bandbreiten haben, Ihnen für alle verbundenen Datenträger unterstützten aus. Hoher Auslastung können Sie die maximale Datenträgerbandbreite für die Größe der virtuellen Computer Rolle Sättigung erhöhen. Beispielsweise wird eine Standard_DS14 bis zu 512 MB/s unterstützen. könnten Sie daher mit drei P30 Datenträger die Datenträgerbandbreite, der den virtuellen Computer Sättigung erhöhen. Aber in diesem Beispiel die Durchsatzgrenze abhängig von der Kombination aus Lese- und Schreibberechtigungen IOs überschritten werden konnte.

## <a name="new-deployments"></a>Neue Bereitstellungen

In den beiden nächsten Abschnitten veranschaulichen, wie Sie SQL Server-virtuellen Computern Premium Speicher bereitstellen können. Wie bereits erwähnt, müssen Sie nicht unbedingt platzieren Sie den Datenträger OS auf Premium Storage. Sie könnten Sie entscheiden vorgehen, wenn Sie die Absicht haben, um alle stark EA Auslastung auf die OS virtuelle Festplatte zu platzieren.

Im ersten wird veranschaulicht die Nutzung von vorhandenen Azure Gallery-Bilder. Das zweite Beispiel zeigt, wie Sie ein benutzerdefiniertes Bild für die virtuellen Computer verwenden, das Sie in einem vorhandenen Standard Speicherkonto besitzen.

> [AZURE.NOTE] In diesen Beispielen wird davon ausgegangen, dass Sie bereits eine Landes-/ VNET erstellt haben.

### <a name="create-a-new-vm-with-premium-storage-with-gallery-image"></a>Erstellen eines neuen virtuellen Computers mit Premium-Speicher mit Bild im Katalog

Im folgenden Beispiel wird gezeigt, wie die OS virtuelle Festplatte auf Premium Storage platzieren und Anfügen Premium Speicher virtueller Festplatten. Sie können jedoch, platzieren Sie den Datenträger OS auch in einem standardmäßigen Speicher-Konto und fügen Sie virtuelle Festplatten, die in einem Konto Premium Storage gespeichert sind. Sowohl Szenarien es werden veranschaulicht.

    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Set up subscription
    Set-AzureSubscription -SubscriptionName $mysubscription
    Select-AzureSubscription -SubscriptionName $mysubscription -Current  

#### <a name="step-1-create-a-premium-storage-account"></a>Schritt 1: Erstellen eines Premium-Speicher-Kontos


    #Create Premium Storage account, note Type
    $newxiostorageaccountname = "danspremsams"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  


#### <a name="step-2-create-a-new-cloud-service"></a>Schritt 2: Erstellen eines neuen Cloud-Diensts

    $destcloudsvc = "danNewSvcAms"
    New-AzureService $destcloudsvc -Location $location


#### <a name="step-3-reserve-a-cloud-service-vip-optional"></a>Schritt 3: Reservieren einer Cloud-Dienst VIP (Optional)
    #check exisitng reserved VIP
    Get-AzureReservedIP

    $reservedVIPName = “sqlcloudVIP”
    New-AzureReservedIP –ReservedIPName $reservedVIPName –Label $reservedVIPName –Location $location

#### <a name="step-4-create-a-vm-container"></a>Schritt 4: Erstellen eines Containers virtueller Computer
    #Generate storage keys for later
    $xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

    ##Generate storage acc contexts
    $xioContext = New-AzureStorageContext –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary   

    #Create container
    $containerName = 'vhds'
    New-AzureStorageContainer -Name $containerName -Context $xioContext

#### <a name="step-5-placing-os-vhd-on-standard-or-premium-storage"></a>Schritt 5: Platzieren OS virtuelle Festplatte auf Standard oder Premium-Speicher
    #NOTE: Set up subscription and default storage account which will be used to place the OS VHD in

    #If you want to place the OS VHD Premium Storage Account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount  $newxiostorageaccountname  

    #If you wanted to place the OS VHD Standard Storage Account but attach Premium Storage VHDs then you would run this instead:
    $standardstorageaccountname = "danstdams"

    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount  $standardstorageaccountname

#### <a name="step-6-create-vm"></a>Schritt 6: Erstellen von virtuellen Computer
    #Get list of available SQL Server Images from the Azure Image Gallery.
    $galleryImage = Get-AzureVMImage | where-object {$_.ImageName -like "*SQL*2014*Enterprise*"}
    $image = $galleryImage.ImageName

    #Set up Machine Specific Information
    $vmName = "dsDan1"
    $vnet = "dansvnetwesteur"
    $subnet = "SQL"
    $ipaddr = "192.168.0.8"

    #Remember to change to DS series VM
    $newInstanceSize = "Standard_DS1"

    #create new Avaiability Set
    $availabilitySet = "cloudmigAVAMS"

    #Machine User Credentials
    $userName = "myadmin"
    $pass = "mycomplexpwd4*"

    #Create VM Config
    $vmConfigsl = New-AzureVMConfig -Name $vmName -InstanceSize $newInstanceSize -ImageName $image  -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` -AdminUserName $userName -Password $pass | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    #Add Data and Log Disks to VM Config
    #Note the size specified ‘-DiskSizeInGB 1023’, this will attach 2 x P30 Premium Storage Disk Type
    #Utilising the Premium Storage enabled Storage account

    $vmConfigsl | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 0 -HostCaching "ReadOnly"  -DiskLabel "DataDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-data1.vhd"
    $vmConfigsl | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 1 -HostCaching "None"  -DiskLabel "logDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-log1.vhd"

    #Create VM
    $vmConfigsl  | New-AzureVM –ServiceName $destcloudsvc -VNetName $vnet ## Optional (-ReservedIPName $reservedVIPName)  

    #Add RDP Endpoint
    $EndpointNameRDPInt = "3389"
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmName | Add-AzureEndpoint -Name "EndpointNameRDP" -Protocol "TCP" -PublicPort "53385" -LocalPort $EndpointNameRDPInt  | Update-AzureVM

    #Check VHD storage account, these should be in $newxiostorageaccountname
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmName | Get-AzureDataDisk
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmName |Get-AzureOSDisk


### <a name="create-a-new-vm-to-use-premium-storage-with-a-custom-image"></a>Erstellen eines neuen virtuellen Computers um Premium Speicher durch ein benutzerdefiniertes Bild verwenden

Dieses Szenario veranschaulicht, in dem vorhandenen angepasste haben Sie Bilder, die in einem standardmäßigen Speicher Konto befinden. Wie schon erwähnt, wenn die OS virtuelle Festplatte Premium Speichermenge eingefügt werden sollen müssen Sie das Bild, das vorhanden ist in der Standard-Speicher Konto kopieren und auf einer Premium-Speicher übertragen, bevor er verwendet werden kann. Wenn Sie ein Bild lokal verfügen, können Sie diese Methode auch verwenden, um, die direkt in das Konto Premium Speicher zu kopieren.

#### <a name="step-1-create-storage-account"></a>Schritt 1: Erstellen von Speicher-Konto
    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Create Premium Storage account
    $newxiostorageaccountname = "danspremsams"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  

    #Standard Storage account
    $origstorageaccountname = "danstdams"

#### <a name="step-2-create-cloud-service"></a>Schritt 2 erstellen Cloud-Dienst
    $destcloudsvc = "danNewSvcAms"
    New-AzureService $destcloudsvc -Location $location


#### <a name="step-3-use-existing-image"></a>Schritt 3: Verwenden Sie vorhandenes Bild
Sie können ein vorhandenes Bild verwenden. Alternativ können Sie [ein Bild von vorhandenen Computer ausführen](virtual-machines-windows-classic-capture-image.md). Beachten Sie den Computer Sie Bild hat keinen DS* werden Computer. Nachdem Sie das Bild haben, die folgenden Schritte zeigen, wie es mit dem Konto Premium Speicher mit Kopieren der * *Start-AzureStorageBlobCopy** PowerShell-Cmdlet.

    #Get storage account keys:
    #Standard Storage account
    $originalstorage =  Get-AzureStorageKey -StorageAccountName $origstorageaccountname
    #Premium Storage account
    $xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

    #Set up contexts for the storage accounts:
    $origContext = New-AzureStorageContext  –StorageAccountName $origstorageaccountname -StorageAccountKey $originalstorage.Primary
    $destContext = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary  

#### <a name="step-4-copy-blob-between-storage-accounts"></a>Schritt 4: Kopieren Blob zwischen Speicherkonten
    #Get Image VHD
    $myImageVHD = "dansoldonorsql2k14-os-2015-04-15.vhd"
    $containerName = 'vhds'

    #Copy Blob between accounts
    $blob = Start-AzureStorageBlobCopy -SrcBlob $myImageVHD -SrcContainer $containerName `
    -DestContainer vhds -Destblob "prem-$myImageVHD" `
    -Context $origContext -DestContext $destContext  

#### <a name="step-5-regularly-check-copy-status"></a>Schritt 5: Überprüfen Sie regelmäßig Status kopieren:
    $blob | Get-AzureStorageBlobCopyState

#### <a name="step-6-add-image-disk-to-azure-disk-repository-in-subscription"></a>Schritt 6: Hinzufügen von Bild Datenträger zu Azure Datenträger Repository im Abonnement
    $imageMediaLocation = $destContext.BlobEndPoint+"/"+$myImageVHD
    $newimageName = "prem"+"dansoldonorsql2k14"

    Add-AzureVMImage -ImageName $newimageName -MediaLocation $imageMediaLocation

> [AZURE.NOTE] Vorkommen, dass, obwohl die Statusberichte als Erfolg noch einen Fehler verleasen werden konnte. In diesem Fall warten Sie etwa 10 Minuten.

#### <a name="step-7--build-the-vm"></a>Schritt 7: Erstellen Sie den virtuellen Computer
Hier sind Sie den virtuellen Computer auf das Bild und Anfügen von zwei Premium Speicher virtuellen Festplatten erstellen:

    $newimageName = "prem"+"dansoldonorsql2k14"
    #Set up Machine Specific Information
    $vmName = "dansolchild"
    $vnet = "westeur"
    $subnet = "Clients"
    $ipaddr = "192.168.0.41"

    #This will need to be a new cloud service
    $destcloudsvc = "danregsvcamsxio2"

    #Use to DS Series VM
    $newInstanceSize = "Standard_DS1"

    #create new Avaiability Set
    $availabilitySet = "cloudmigAVAMS3"

    #Machine User Credentials
    $userName = "myadmin"
    $pass = "theM)stC0mplexP@ssw0rd!”


    #Create VM Config
    $vmConfigsl2 = New-AzureVMConfig -Name $vmName -InstanceSize $newInstanceSize -ImageName $newimageName  -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` -AdminUserName $userName -Password $pass | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    $vmConfigsl2 | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 0 -HostCaching "ReadOnly"  -DiskLabel "DataDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-Datadisk-1.vhd"
    $vmConfigsl2 | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 1 -HostCaching "None"  -DiskLabel "LogDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-logdisk-1.vhd"



    $vmConfigsl2 | New-AzureVM –ServiceName $destcloudsvc -VNetName $vnet

## <a name="existing-deployments-that-do-not-use-always-on-availability-groups"></a>Vorhandene Bereitstellungen, die nicht immer auf Verfügbarkeit Gruppen verwenden

> [AZURE.NOTE] Vorhandene Bereitstellungen zuerst finden Sie im Abschnitt " [erforderliche Komponenten](#prerequisites-for-premium-storage) " dieses Themas.

Es gibt verschiedene Aspekte für SQL Server-Bereitstellungen, die immer auf Verfügbarkeit und Gruppen, die nicht verwenden. Wenn Sie nicht immer auf verwenden und einer vorhandenen eigenständigen SQL Server, können Sie mithilfe eines neuen Cloud-Dienst und Speicher-Kontos zu Premium Speicher aktualisieren. Beachten Sie die folgenden Optionen aus:

- **Erstellen eines neuen SQL Server virtuellen Computers**. Sie können ein neues SQL Server virtueller Computer, die ein Konto Premium Speicher verwendet erstellen, wie in der neuen Bereitstellungen erläutert. Klicken Sie dann sichern und Wiederherstellen von SQL Server-Konfiguration und Benutzer Datenbanken. Die Anwendung muss aktualisiert werden, um den neuen SQL Server verweisen, wenn sie intern oder extern zugegriffen wird. Sie müssen alle 'außerhalb des Db' Objekte kopieren, als wäre Sie eine nebeneinander (SxS) SQL Server-Migration getan haben möchten. Dies umfasst Objekte wie Benutzernamen, Zertifikate und verknüpften Servers.
- **Migrieren einer vorhandenen SQL Server virtueller Computer**. Dieser Vorgang erfordert Offlineschalten der SQL Server virtueller Computer und dann Übertragung an einen neuen Clouddienst, der enthält alle zugehörigen angefügten virtuellen Festplatten mit dem Konto Premium Speicher kopiert werden. Wenn Sie der virtuellen Computer online ist, wird die Anwendung Serverhostname als vor verwiesen werden. Achten Sie darauf, dass sich die Größe des vorhandenen Datenträgers die Leistungsmerkmale auswirken. Beispielsweise erhält eine 400 GB Festplatte einer P20 aufgerundet. Wenn Sie wissen, dass Sie die Leistungsfähigkeit der Festplatte nicht benötigen, könnten Sie neu erstellen den virtuellen Computer als einen DS Serie virtuellen und Anfügen von virtuellen Festplatten Premium-Speicher der Spezifikation Größe/Leistung, die Sie benötigen. Trennen, und schließen Sie die SQL-DB-Dateien wieder.

> [AZURE.NOTE] Beim Kopieren des virtuellen Festplatte Datenträgers der Größe, abhängig von der Größe beachtet werden sollten in welcher Premium Speicher Datenträger bedeuten diese fallen, dies Datenträger Leistung Spezifikation bestimmt. Azure wird runden auf die nächste Datenträger Größe, wenn Sie einen Datenträger 400 GB verfügen, dies auf einer P20 aufgerundet wird. Je nach Ihren vorhandenen EA-Anforderungen der virtuellen Festplatte OS müssen Sie nicht diese mit einem Premium Speicher-Konto migrieren.

Wenn auf dem SQL Server extern zugegriffen wird, ändert sich die Cloud-Dienst VIP dann. Sie können auch zum Aktualisieren Ihrer Endpunkte ACLs und DNS-Einstellungen sind.

## <a name="existing-deployments-that-use-always-on-availability-groups"></a>Vorhandene Bereitstellungen, die immer auf Verfügbarkeit Gruppen verwenden

> [AZURE.NOTE] Vorhandene Bereitstellungen zuerst finden Sie im Abschnitt " [erforderliche Komponenten](#prerequisites-for-premium-storage) " dieses Themas.

Zunächst in diesem Abschnitt betrachten wir immer auf und Azure Networking Interaktion. Wir werden dann Teile aufzuteilen Migration in zu folgenden beiden Szenarien: Migration, wo langweilig vorgenommen werden kann, und Migration, müssen Sie Ausfälle erzielen.

Lokalen SQL Server immer auf Verfügbarkeit Gruppen verwenden einer Zuhörer lokalen, die einen virtuellen DNS-Namen sowie eine IP-Adresse registriert hat, die zwischen einem oder mehreren SQL-Servern freigegeben werden. Wenn Clients eine Verbindung herstellen, die sie durch die Zuhörer IP-Adresse auf dem primären SQL Server weitergeleitet werden. Dies ist der Server, der die immer auf IP-Ressource zu diesem Zeitpunkt besitzt.

![Klicken Sie auf DeploymentsUseAlways][6]

In Microsoft Azure können Sie nur eine IP-Adresse, einen Netzwerkadapter des virtuellen Computers zugewiesen haben, verwendet also das erreichen abstrakte als lokal die gleiche Ebene Azure die IP-Adresse, die die interne/externe Lastenausgleich (ILB/ELB) zugeordnet ist. IP-Ressource, die zwischen den Servern freigegeben ist, wird auf die gleiche IP-Adresse als die ILB/ELB festgelegt. Dies ist im DNS veröffentlicht und Clientdatenverkehr wird durch die ILB/ELB an die primäre SQL Server geleitet. Die ILB/ELB kennt die SQL Server primären ist, da es PrüfpunkteDer Prüfpunkt immer auf IP-Ressource. Im vorherigen Beispiel überprüft jedem Knoten, der einen Endpunkt optimiert die ELB/ILB, je nachdem, was die Absichten des primären SQL Server ist.

> [AZURE.NOTE] Die ILB und ELB werden in einem bestimmten Azure-Cloud-Dienst zugewiesen, daher jeder Migration Cloud in Azure wahrscheinlich bedeutet, dass die Auslastung Lastenausgleich IP-Adresse geändert wird.

### <a name="migrating-always-on-deployments-that-can-allow-some-downtime"></a>Migrieren von immer auf Bereitstellungen, mit denen langweilig können können

Es gibt zwei Strategien immer auf Bereitstellungen migrieren, die langweilig zulässig ist:

1. **Hinzufügen von weiteren sekundären Kopien zu einem vorhandenen immer auf Cluster**
1. **Um einen neuen immer auf Cluster migrieren**

#### <a name="1-add-more-secondary-replicas-to-an-existing-always-on-cluster"></a>1 fügen Sie 1 weiteren sekundäre Kopien zu einem vorhandenen immer auf Cluster

Eine Strategie besteht darin, weitere sekundäre der immer auf Verfügbarkeit Gruppe hinzufügen. Sie müssen diese in einem neuen Cloud-Dienst hinzufügen und aktualisieren Sie die Zuhörer mit den neuen Laden Lastenausgleich IP.

##### <a name="points-of-downtime"></a>Points Ausfall:

- Cluster Überprüfung.
- Testen immer auf Failovers für neue sekundäre aus.

Wenn Sie für höhere EA Durchsatz Windows Speicherpools innerhalb der virtuellen Computer verwenden, klicken Sie dann diese offline während einer vollständigen Cluster Überprüfung gelangen. Der Überprüfung ist erforderlich, wenn Sie die Cluster Knoten hinzufügen. Die zum Ausführen des Tests erforderliche Zeit kann variieren, sodass Sie dies in Ihrer testumgebung Vertreter abzurufenden eine ungefähre Zeit wie lange dies dauert testen sollten.

Sie sollten Zeit bereitstellen, wo Sie Manuelles Failover und Chaos Testen für die neu hinzugefügten Knoten, um sicherzustellen, dass immer auf hohen Verfügbarkeit Funktionen wie erwartet ausführen können.

![DeploymentUseAlways On2][7]

> [AZURE.NOTE] Sie sollten beenden Sie alle Instanzen von SQL Server, in dem die Speicherpools verwendeten, vor der Validierung ausgeführt wird.
##### <a name="high-level-steps"></a>Allgemeinen Schritte

1. Erstellen Sie zwei neue SQL Server in der neuen Cloud-Dienst mit angefügten Premium-Speicher.
1. Kopieren Sie über vollständige Sicherungskopien und mit **NORECOVERY**wiederherstellen.
1. Kopieren Sie über 'außerhalb des Benutzers DB' abhängige Objekte, z. B. Benutzernamen usw. aus.
1. Neu erstellen Sie eines neuen internen laden Lastenausgleich (ILB) oder verwenden Sie eine externe laden Lastenausgleich (ELB), und dann auf Laden verteilt Endpunkte richten Sie ein, auf beiden neuen Knoten.
> [AZURE.NOTE] Überprüfen Sie, dass alle Knoten die richtige Endpunktkonfiguration verfügen, bevor Sie fortfahren

1. Beenden der Benutzer oder einer Anwendung Zugriff auf die SQL Server (falls Speicherpools verwenden).
1. Beenden Sie SQL Server-Engine Services auf allen Knoten (falls Speicherpools verwenden).
1. Fügen Sie neue Knoten zum cluster und umfassende Validierung ausführen.
1. Nachdem die Überprüfung erfolgreich ist, beginnen Sie alle SQL Server-Dienste.
1. Transaktionsprotokolle sichern und Wiederherstellen von Benutzerdatenbanken.
1. Hinzufügen von neuen Knoten in der immer auf Verfügbarkeit Gruppe, und setzen Sie die Replikation in **synchron**.
1. Fügen Sie die IP-Adressen-Ressource für die neue Cloud Service ILB/ELB über PowerShell für immer auf basierend auf der Website mit mehreren Beispiel in die [Anlage](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage)ein. Legen Sie in Windows-Cluster die **möglichen Besitzer** der Ressource **IP-Adresse** auf die alte neuen Knoten ein. Finden Sie im Abschnitt "Hinzufügen von IP-Adresse Resource in demselben Subnetz" [Anlage](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage)aus.
1. Failover auf einem neuen Knoten.
1. Stellen Sie die neuen Knoten automatische Failover Partner und Test Failovers.
1. Entfernen von ursprünglichen Knoten aus Verfügbarkeit Gruppe.

##### <a name="advantages"></a>Vorteile

- Neue SQL Server werden können (SQL Server und Anwendung) getestet, bevor sie immer auf hinzugefügt werden.
- Sie können Ändern der Größe des virtuellen Computer und im Speicher zu Ihren genauen Anforderungen anpassen. Jedoch wäre es positiv auf alle Pfade der SQL-Datei beizubehalten.
- Sie können steuern, wann die Übertragung DB Sicherungen auf der Sekundärachse Replikate gestartet werden soll. Dies unterscheidet sich von mit Azure **Start-AzureStorageBlobCopy** -Cmdlet virtuellen Festplatten, kopieren, da dies eine asynchrone Kopie ist.

##### <a name="disadvantages"></a>Nachteile
- Bei der Verwendung von Windows-Speicherpools ist es Cluster Ausfallzeiten während der vollständigen Cluster Validierung für die neuen zusätzlichen Knoten ein.
- Abhängig von der SQL Server-Version und die Anzahl der vorhandenen sekundären Replikate Sie weiteren sekundären Kopien hinzufügen, ohne die vorhandene sekundäre entfernen möglicherweise nicht.
- SQL Daten durchstellen lange beim Einrichten der sekundäre könnte.
- Während der Migration ist zusätzlicher Kosten, während Sie neue Computer parallel ausgeführt haben.

#### <a name="2-migrate-to-a-new-always-on-cluster"></a>2. Migrieren Sie, um einen neuen immer auf Cluster

Eine andere Strategie ist das Erstellen einer ganz neuen immer auf Cluster mit ganz neuen Knoten in der neuen Cloud-Dienst und leiten Sie anschließend die Clients, um ihn zu verwenden.

##### <a name="points-of-downtime"></a>Ausfall Punkte

Es gibt Ausfallzeiten bei der Übertragung Applikationen und Benutzer in der neuen Zuhörer immer auf. Abhängig von der Ausfall:

- Die Zeit abgeschlossen Transaktion Log Sicherungskopien in Datenbanken auf neuen Servern wiederherstellen.
- Die Zeit Clientanwendungen mit neuen Zuhörer immer auf aktualisieren.

##### <a name="advantages"></a>Vorteile

- Sie können die tatsächliche Herstellung-Umgebung, SQL Server, testen und OS erstellen Änderungen.
- Sie haben die Option zum Anpassen der Speichers und potenziell Verringern der Größe virtueller Computer. Verringerung der Kosten möglich.
- Sie können Ihre SQL Server erstellen oder Version während dieses Vorgangs aktualisieren. Sie können auch das Betriebssystem aktualisieren.
- Der vorherigen immer auf Cluster kann als einfarbige zurücksetzen Ziel fungieren.

##### <a name="disadvantages"></a>Nachteile

- Sie müssen den DNS-Namen für die Zuhörer bei Bedarf beide gleichzeitig ausgeführte Cluster immer auf ändern. Dadurch wird die Verwaltung Aufwand während der Migration wie Client Anwendung Zeichenfolgen zu den neuen Namen für die Zuhörer entsprechen müssen hinzugefügt.
- Sie müssen ein Verfahren Synchronisierung zwischen den beiden Umgebung belassen sie so nah wie möglich, die endgültige Synchronisierung Anforderungen vor der Migration zu minimieren implementieren.
- Es ist gegen Gebühr während der Migration, während Sie auf die neue Umgebung ausgeführt haben.

### <a name="migrating-always-on-deployments-for-minimal-downtime"></a>Migrieren von immer auf Bereitstellungen für Ausfälle

Es gibt zwei Strategien für das Migrieren von immer auf Bereitstellungen für Ausfälle aus:

1. **Verwenden einer vorhandenen sekundären: Single-Website**
1. **Nutzen Sie vorhandene sekundäre Replikate: Multi-Website**

#### <a name="1-utilize-an-existing-secondary-single-site"></a>1. Verwenden einer vorhandenen sekundären: Single-Website

Eine Strategie für Ausfälle ist Übernehmen einer vorhandenen Cloud sekundäre und aus der aktuellen Cloud-Dienst zu entfernen. Klicken Sie dann kopieren Sie die virtuellen Festplatten mit dem neuen Premium Speicher Konto, und erstellen Sie den virtuellen Computer in der neuen Cloud-Dienst. Aktualisieren Sie dann die Zuhörer in Cluster und Failover ein.

##### <a name="points-of-downtime"></a>Ausfall Punkte

- Es gibt Ausfallzeiten an, wenn Sie den letzten Knoten mit den Lastenausgleich Endpunkt aktualisieren.
- Der Client erneut eine Verbindung herzustellen, möglicherweise je nach Client/DNS-Konfiguration verzögert werden.
- Es gibt zusätzliche Ausfallzeiten bei Auswahl die Gruppe immer auf Cluster offline ergreifen können, um die IP-Adressen können. Sie können dies vermeiden, indem Sie mit einer Abhängigkeit oder und mögliche Besitzer für die hinzugefügte Ressource für die IP-Adresse. Finden Sie im Abschnitt "Hinzufügen von IP-Adresse Resource in demselben Subnetz" [Anlage](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage)aus.

> [AZURE.NOTE] Wenn Sie den hinzugefügten Knoten als eine immer auf Failoverpartner in partake soll, müssen Sie einen Endpunkt Azure mit einem Verweis auf Laden verteilt festlegen hinzufügen. Wenn Sie den Befehl **Hinzufügen-AzureEndpoint** Aktion ausführen, werden aktuelle Verbindungen zu öffnen, aber neue Verbindungen mit dem Zuhörer bleiben werden nicht eingerichtet werden, bis der Lastenausgleich aktualisiert haben. Testen Sie dies wurde betrachtet zum letzten 90-120seconds, dies getestet werden soll.

##### <a name="advantages"></a>Vorteile

- Ohne zusätzliche Kosten entstehen, während der Migration.
- Eine 1: 1-Migration.
- Geringere Komplexität.
- Höhere IOPS ermöglicht von Premium Speicher SKUs. Wenn der Datenträger aus dem virtuellen Computer getrennt werden und in der neuen Cloud-Dienst kopiert eine 3rd party kann Tool zum virtuelle Festplatte, vergrößern, die höhere Datendurchsätzen bereitstellt verwendet werden. Zunehmender virtuelle Festplatte Größen, finden Sie in diesem [Forumsdiskussion](https://social.msdn.microsoft.com/Forums/azure/4a9bcc9e-e5bf-4125-9994-7c154c9b0d52/resizing-azure-data-disk?forum=WAVirtualMachinesforWindows).

##### <a name="disadvantages"></a>Nachteile

- Es gibt eine temporäre Verlust der HA und DR während der Migration.
- Da es sich um eine 1:1-Migration handelt, müssen Sie eine Mindestgröße virtuellen Computer verwenden, die die Anzahl der virtuellen Festplatten, unterstützen, sodass Sie möglicherweise nicht mehr Ihrer virtuellen Computer hervor.
- Dieses Szenario würde das Cmdlet Azure **Start-AzureStorageBlobCopy** verwenden, welche asynchrone ist. Es gibt keine Vereinbarung zum SERVICELEVEL Abschluss kopieren aus. Die Dauer der Kopien ist, während dies in Warteschlange warten abhängt, die auch auf der Datenmenge übertragen abhängig sind wird. Die Uhrzeit Kopieren erhöht wird, wenn die Übertragung an eine andere Azure Datacenter geht, das in einem anderen Region Premium Speicher unterstützt wird. Wenn Sie nur 2 Knoten verfügen, erwägen Sie eine mögliche Lösung an, für den Fall, dass die Kopie länger als testen dauert. Dies kann die folgenden Punkte umfassen.
    - Fügen Sie eine temporäre 3rd SQL Server-Knoten für HA vor der Migration mit vereinbarten Ausfallzeiten hinzu.
    - Ausführen der Migrations außerhalb Azure geplante Wartung.
    - Stellen Sie sicher, dass Sie Ihre Clusterquorum ordnungsgemäß konfiguriert haben.  

##### <a name="high-level-steps"></a>Allgemeinen Schritte

Dieses Dokument zeigt, wie ein vollständiges Ende zum Beispiel nicht, aber die [Anlage](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage) Details enthält, die zum Ausführen dieses genutzt werden können.

![MinimalDowntime][8]

- Sammeln Sie Datenträgerkonfiguration, und entfernen Sie den Knoten (angefügte virtuellen Festplatten nicht löschen).
- Erstellen Sie ein Konto Premium Speicher, und kopieren Sie die virtuellen Festplatten aus dem standardmäßigen Speicher-Konto
- Erstellen von neuen Cloud-Dienst und erneut den SQL2 virtuellen Computer in die Cloud-Dienst bereitstellen. Erstellen Sie den virtuellen Computer verwenden die kopierte ursprüngliche OS virtuelle Festplatte und Anfügen von den kopierten virtuellen Festplatten.
- Konfigurieren von ILB / ELB und Hinzufügen von Endpunkten.
- Aktualisieren Sie Zuhörer, indem Sie entweder:
    - Offlineschalten der immer auf Gruppe, und aktualisieren die immer auf Zuhörer mit neuen ILB / ELB IP-Adresse.
    - Oder die IP-Adressressource der neuen Cloud Service ILB/ELB über PowerShell in Windows-Cluster hinzufügen. Klicken Sie dann legen Sie die möglichen Besitzer der Ressource IP-Adresse zum migrierte Knoten und SQL2, und diese als Abhängigkeit von oder in den Netzwerknamen. Finden Sie im Abschnitt "Hinzufügen von IP-Adresse Resource in demselben Subnetz" [Anlage](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage)aus.
- Überprüfen Sie die DNS-Konfiguration/Propagierung an Clients.
- Migrieren von SQL1 virtuellen Computer, und wechseln Sie durch die Schritte 2 bis 4.
- Wenn Schritte 5ii verwenden, klicken Sie dann fügen Sie SQL1 als möglicher Besitzer für die hinzugefügte IP-Adressressource hinzu
- Testen Sie Failovers.

#### <a name="2-utilize-existing-secondary-replicas-multi-site"></a>2 nutzen Sie vorhandene sekundäre Replikate: Multi-Website

Wenn Sie mehrere Azure Datacenter (DC) Knoten haben, oder wenn Sie eine hybridumgebung haben, klicken Sie dann können eine Konfiguration immer auf in dieser Umgebung Sie Ausfallzeiten minimieren.

Der Ansatz ist so ändern Sie die Synchronisierung immer auf zu synchron für die lokale oder sekundäre Azure DC, und klicken Sie dann Failover über auf die SQL Server. Klicken Sie dann kopieren Sie die virtuellen Festplatten mit einem Konto Premium Speicher und erneut bereitstellen Sie des Computers in einem neuen Cloud-Dienst. Aktualisieren der Zuhörer, und klicken Sie dann nicht wieder.

##### <a name="points-of-downtime"></a>Ausfall Punkte

Der Ausfall besteht aus der Zeit bis zur Failover zum alternativen DC und zurück. Es hängt auch der Client/DNS-Konfigurations, und der Client erneut eine Verbindung herzustellen verzögert werden kann.
Betrachten Sie das folgende Beispiel einer Hybrid immer auf Konfiguration aus:

![MultiSite1][9]

##### <a name="advantages"></a>Vorteile

- Sie können vorhandene Infrastruktur nutzen.
- Sie haben die Möglichkeit, den Azure-Speicher auf dem DC DR Azure zuerst vor dem upgrade.
- Die Speicherung DR Azure DC kann neu konfiguriert werden.
- Es gibt mindestens zwei Failovers während der Migration Testfailovers ausschließen.
- Sie müssen nicht Verschieben von SQL Server-Daten mit Sicherung und Wiederherstellung.

##### <a name="disadvantages"></a>Nachteile

- Je nach Clientzugriff auf SQL Server Wenn sich möglicherweise höhere Wartezeit zur Anwendung einer alternativen DC SQL Server ausgeführt wird.
- Die Anzeigedauer Kopieren virtueller Festplatten zu Premium Speicher konnte lang sein. Dies möglicherweise Einfluss auf Ihre Entscheidung, ob Sie in der Gruppe Verfügbarkeit der Knoten erhalten bleiben. Berücksichtigen Sie dies für wann stark Arbeit Log lädt, während der Migration ausgeführt werden erforderlich ist, da der primäre Knoten unreplizierten Transaktionen im aufzeichnen Transaktion beibehalten werden muss. Daher konnte dies erheblich zunehmen.
- Dieses Szenario würde das Cmdlet Azure **Start-AzureStorageBlobCopy** verwenden, welche asynchrone ist. Es gibt keine Vereinbarung zum SERVICELEVEL Abschluss. Die Dauer der Kopien hängt ab, während dies warten in der Warteschlange abhängt, es hängt außerdem die Menge der Daten zu übertragen. Daher haben Sie nur einem Knoten in Ihrem 2nd Data Center, Sie sollten Schritte Reduzierung für den Fall, dass die Kopie länger als testen dauert. Dies kann die folgenden Punkte umfassen.
    - Fügen Sie eine temporäre 2nd SQL-Knoten für HA vor der Migration mit vereinbarten Ausfallzeiten hinzu.
    - Ausführen der Migrations außerhalb Azure geplante Wartung.
    - Stellen Sie sicher, dass Sie Ihre Clusterquorum ordnungsgemäß konfiguriert haben.

Dieses Szenario setzt voraus, dass Sie Ihre Installation dokumentierten haben und wissen, wie die Speicherung zugeordnet wird, um die Änderungen für die Einstellungen des Caches für optimale Datenträger vornehmen.

##### <a name="high-level-steps"></a>Allgemeinen Schritte
![Multisite2][10]

- Nehmen Sie die lokale / abwechselnd Azure DC die primäre SQL Server, und machen Sie es mit den anderen automatische Failover Partner (AFP).
- Sammeln von Informationen der Datenträger-Konfiguration in SQL2, und entfernen Sie den Knoten (angefügte virtuellen Festplatten nicht löschen).
- Erstellen Sie ein Konto Premium Speicher, und kopieren Sie virtuelle Festplatten aus dem standardmäßigen Speicher-Konto.
- Erstellen eines neuen Cloud-Diensts und den SQL2 virtuellen Computer mit seiner Beiträgen Datenträger angefügt.
- Konfigurieren von ILB / ELB und Hinzufügen von Endpunkten.
- Aktualisieren der immer auf Zuhörer mit neuen ILB / ELB IP-Adresse und Test Failover.
- Überprüfen Sie die DNS-Konfiguration.
- Ändern Sie der AFP SQL2, migrieren Sie SQL1 und durchlaufen Sie die Schritte 2 bis 5.
- Testen Sie Failovers.
- Wechseln Sie zurück zur SQL1 und SQL2 der AFP

## <a name="appendix-migrating-a-multisite-always-on-cluster-to-premium-storage"></a>Anlage: Migrieren einer mehreren Sites immer auf Cluster zu Premium-Speicher

Im weiteren Verlauf dieses Themas bietet ein detailliertes Beispiel einen Website mit mehreren immer auf Cluster in Premium Speicher zu konvertieren. Konvertiert aber auch die Zuhörer mit einer externen Lastenausgleich (ELB) an eine interne Lastenausgleich (ILB).

### <a name="environment"></a>Umgebung

- Windows 2k 12 / SQL 2k 12
- SP 1 DB Dateien
- 2 x Speicherpools pro Knoten

![Appendix1][11]

### <a name="vm"></a>VIRTUELLER COMPUTER:

In diesem Beispiel werden wir Umstieg von einer ELB auf ILB veranschaulichen. ELB war vor ILB, verfügbar, damit dies wird gezeigt, wie dies während der Migration zu wechseln.

![Appendix2][12]

### <a name="pre-steps-connect-to-subscription"></a>Pre-Schritte: Herstellen einer Verbindung Abonnement mit

    Add-AzureAccount

    #Set up subscription
    Get-AzureSubscription

#### <a name="step-1-create-new-storage-account-and-cloud-service"></a>Schritt 1: Erstellen Sie neue Speicherkonto und Cloud-Dienst
    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Storage accounts
    #current storage account where the vm to migrate resides
    $origstorageaccountname = "danstdams"

    #Create Premium Storage account
    $newxiostorageaccountname = "danspremsams"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  

    #Generate storage keys for later
    $originalstorage =  Get-AzureStorageKey -StorageAccountName $origstorageaccountname
    $xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

    #Generate storage acc contexts
    $origContext = New-AzureStorageContext  –StorageAccountName $origstorageaccountname -StorageAccountKey $originalstorage.Primary
    $xioContext = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary  

    #Set up subscription and default storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $origstorageaccountname
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

    #CREATE NEW CLOUD SVC
    $vnet = "dansvnetwesteur"

    ##Existing cloud service
    $sourceSvc="dansolSrcAms"

    ##Create new cloud service
    $destcloudsvc = "danNewSvcAms"
    New-AzureService $destcloudsvc -Location $location

#### <a name="step-2-increase-the-permitted-failures-on-resources-optional"></a>Schritt 2: Erhöhen der zulässigen Fehlern auf Ressourcen<Optional>
Klicken Sie auf bestimmte Ressourcen, die immer auf Verfügbarkeit Gruppe angehören Leuchten Grenzwerte wie viele Fehler, die in einer Periode, auftreten können, in dem der Cluster-Dienst versucht, die Ressourcengruppe neu zu starten. Es wird empfohlen, dass Sie dies erhöhen, während Sie über diesem Verfahren unter seit durchlaufen werden, wenn Sie nicht manuell Failover und Trigger Failovers durch beendet auf Computern, die Sie in der Nähe dieser Grenzwert zugreifen können.

Es wäre ratsam, die den Fehler Rabatt, hierfür in Failovercluster-Manager, doppelklicken, wechseln Sie zu der Eigenschaften der Ressourcengruppe:

![Appendix3][13]

Ändern Sie die maximalen Fehlern in 6.

#### <a name="step-3-addition-ip-address-resource-for-cluster-group-optional"></a>Schritt 3: Erweiterung IP-Adresse Ressource für Cluster-Gruppe<Optional>

Wenn Sie nur eine IP-Adresse für die Gruppe Cluster und diese mit der Cloud Subnetz ausgerichtet ist, beachten Sie, wenn Sie versehentlich offline schalten alle Clusterknoten in der Cloud in diesem Netzwerk dann Cluster IP-Ressource und der Netzwerkname online ist nicht möglich. Im Falle Dies wird es Updates zu anderen Clusterressourcen verhindern.

#### <a name="step-4-dns-configuration"></a>Schritt 4: DNS-Konfiguration

Zum Implementieren eines optimierten Übergang hängt von wie DNS-Einträge der ist ausgelastet und aktualisiert.
Wenn immer auf installiert ist, wird eine Windows Cluster Ressourcengruppe erstellt, wenn Sie Failovercluster-Manager öffnen, sehen Sie, dass mindestens drei Ressourcen hat, sind die beiden, die das Dokument auf verweist:

- Virtuelle Netzwerknamen (VNN) – Dies ist der DNS-Name dieser Client Herstellen einer Verbindung mit Wenn mit SQL Server über immer auf verbinden möchten.
- IP-Adresse der Ressource – Dies ist die IP-Adresse, die mit der VNN verknüpft ist, können Sie mehrere haben und in einer Konfiguration mit mehreren Standorten haben Sie eine IP-Adresse pro Website/Subnetz.

Beim Herstellen einer Verbindung mit SQL Server, die SQL Server-Client Treiber Abrufen der DNS-Einträge der Zuhörer zugeordnet wird, und versuchen Sie, die Verbindung jedes immer auf IP-Adresse zugeordnete, erläutern wir unter einige Faktoren für diese können.

Die Anzahl der gleichzeitigen DNS-Einträge, die den Namen der Zuhörer zugeordnet sind, hängt nicht nur die Anzahl der IP-Adressen zugeordnet sind, aber die ' RegisterAllIpProviders'setting in Failoverclustering für die Ressource immer auf VNN.

Wenn Sie immer auf in Azure bereitstellen es gibt verschiedene Schritte zum Erstellen der Zuhörer und IP-Adressen, die 'RegisterAllIpProviders' manuell konfigurieren 1 vorhanden sind, Dies unterscheidet sich in einer vor Ort immer auf Bereitstellung, wo ist diese auf 1 festgelegt.

Wenn 'RegisterAllIpProviders' 0 ist, dann sehen Sie nur eine DNS-Eintrags in DNS die Zuhörer zugeordnet:

![Appendix4][14]

Wenn 'RegisterAllIpProviders' 1 ist:

![Appendix5][15]

Im folgenden Code wird die VNN Einstellungen zu sichern und legen Sie sie für Sie, Bitte beachten, die vorgenommene Änderung wirksam wird, müssen Sie Offlineschalten der VNN und schalten Sie ihn wieder online, diese aufzeichnen Zuhörer offline verursacht Client Connectivity Unterbrechung.

    ##Always On Listener Name
    $ListenerName = "Mylistener"
    ##Get AlwaysOn Network Name Settings
    Get-ClusterResource $ListenerName| Get-ClusterParameter
    ##Set RegisterAllProvidersIP
    Get-ClusterResource $ListenerName| Set-ClusterParameter RegisterAllProvidersIP  1

In einer späteren Migration Schritt müssen Sie die Zuhörer immer auf eine aktualisierte IP-Adresse zu aktualisieren, in denen ein Lastenausgleich verwiesen wird dies wird eine IP-Adresse Ressourcen freistellen und Erweiterung umfassen. Nach der Aktualisierung IP-müssen Sie Vergewissern Sie sich die neue IP-Adresse in DNS Zone aktualisiert wurde und die Clients ihre lokalen DNS-Cache aktualisiert werden.

Wenn Kunden befinden sich in einem anderen Netzwerksegment und einen anderen DNS-Server verweisen, müssen Sie berücksichtigen, was geschieht mit DNS Zone übertragen während der Migration wie schließen Sie die Anwendung wieder Zeit werden durch eingeschränkt mindestens die Zone übertragen Anzeigedauer keine neuen IP-Adressen für die Zuhörer. Wenn Sie unter Zeit Einschränkung hier sind, sollten Sie diskutieren und Testen einer Zone inkrementell zu übertragen mit Ihrer Windows-Teams erzwingen, und setzen Sie den DNS-Hosteintrag auch zu einer unteren Zeit Gültigkeitsdauer (TTL), damit die Clients aktualisieren. Weitere Informationen finden Sie unter [Inkrementell Zone übertragen](https://technet.microsoft.com/library/cc958973.aspx) und [Start-DnsServerZoneTransfer](https://technet.microsoft.com/library/jj649917.aspx).

Ist 1200 Sekunden standardmäßig die Gültigkeitsdauer für DNS-Eintrags, der die Zuhörer in immer auf in Azure zugeordnet ist. Möglicherweise möchten Sie dies zu verringern, wenn Sie unter Zeit, während der Migration, um sicherzustellen, dass die Clients Einschränkung aktualisieren ihre DNS-Einträge mit der aktualisierten IP-Adresse für die Zuhörer, sind. Sie können finden Sie unter, und ändern Sie die Konfiguration durch Abbilden von der Konfigurations der VNN:

    $AGName = "myProductionAG"
    $ListenerName = "Mylistener"
    #Look at HostRecordTTL
    Get-ClusterResource $ListenerName| Get-ClusterParameter

    #Set HostRecordTTL Examples
    Get-ClusterResource $ListenerName| Set-ClusterParameter -Name "HostRecordTTL" 120

Bitte beachten Sie, desto geringer die 'HostRecordTTL', ein höherer Betrag der DNS-Verkehr ausgeführt wird.

##### <a name="client-application-settings"></a>Anwendung Clienteinstellungen

Wenn Ihre SQL-Clientanwendung .net 4.5 unterstützt SQLClient, und Sie können ' MULTISUBNETFAILOVER = TRUE' Schlüsselwort, dies wird empfohlen, da sie während der Failover schneller Verbindung mit SQL immer auf Verfügbarkeit Gruppe ermöglicht angewendet werden soll. Es durchläuft alle IP-Adressen, die immer auf Zuhörer parallel zugeordnet, und eine kürzere TCP-Verbindung "Wiederholen" Geschwindigkeit bei einem Failover ausführt.

Weitere Informationen zu den oben aufgeführten Einstellungen finden Sie unter [MultiSubnetFailover Schlüsselwort und Funktionen verknüpft ist](https://msdn.microsoft.com/library/hh213080.aspx#MultiSubnetFailover). Siehe auch [SqlClient Unterstützung für eine hohe Verfügbarkeit Wiederherstellung](https://msdn.microsoft.com/library/hh205662(v=vs.110).aspx)aus.

#### <a name="step-5-cluster-quorum-settings"></a>Schritt 5: Cluster Quorum Einstellungen

Wie Sie nacheinander sich mindestens eine SQL Server ab dem annehmen möchten, sollten Sie die Einstellung Cluster Quorum ändern, wenn 2 Knoten (Datei freigeben Witness, FSW) mit, sollten Sie das Quorum Knoten Mehrzahl zulassen und Nutzen von dynamischen Abstimmung festlegen, und dies ist für einen einzelnen Knoten stehende bleiben dürfen.


    Set-ClusterQuorum -NodeMajority  

Weitere Informationen zum Verwalten und Konfigurieren des Clusterquorums finden Sie unter [Konfigurieren und Verwalten des Quorums in einem Windows Server 2012-Failovercluster](https://technet.microsoft.com/library/jj612870.aspx).

#### <a name="step-6-extract-existing-endpoints-and-acls"></a>Schritt 6: Extrahieren Sie vorhandene Endpunkte und ACLs
    #GET Endpoint info
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmNameToMigrate | Get-AzureEndpoint
    #GET ACL Rules for Each EP, this example is for the Always On Endpoint
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmNameToMigrate | Get-AzureAclConfig -EndpointName "myAOEndPoint-LB"  

Speichern Sie diese in eine Textdatei aus.

#### <a name="step-7-change-failover-partners-and-replication-modes"></a>Schritt 7: Ändern von Failover Partner und Replikationsmodi

Wenn Sie mehr als 2 SQL Server verfügen, sollten Sie das Failover für einen anderen sekundären in einem anderen DC oder lokal in 'Synchron' ändern und machen Sie es mit einer automatischen Failover Partner (AFP), dies ist, sodass Sie HA verwalten, während Sie Änderungen vornehmen, werden. Sie können dies von TSQL durch SSMS ändern:

![Appendix6][16]

#### <a name="step-8-remove-secondary-vm-from-cloud-service"></a>Schritt 8: Entfernen der sekundäre virtueller Computer aus der Cloud-Dienst

Planen Sie sollten auf einen sekundären Cloud-Knoten zuerst migrieren ist dies derzeit primären, sollten Sie ein manuelles Failover initiieren.

    $vmNameToMigrate="dansqlams2"

    #Check machine status
    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

    #Shutdown secondary VM
    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | stop-AzureVM


    #Extract disk configuration

    ##Building Existing Data Disk Configuration
    $file = "C:\Azure Storage Testing\mydiskconfig_$vmNameToMigrate.csv"
    $datadisks = @(Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureDataDisk )
    Add-Content $file “lun, vhdname, hostcaching, disklabel, diskName”
    foreach ($disk in $datadisks)
    {
      $vhdname = $disk.MediaLink.AbsolutePath -creplace  "/vhds/"
      $disk.Lun, , $disk.HostCaching, $vhdname, $disk.DiskLabel,$disks.DiskName
    # Write-Host "copying disk $disk"
    $adddisk = "{0},{1},{2},{3},{4}" -f $disk.Lun,$vhdname, $disk.HostCaching, $disk.DiskLabel, $disk.DiskName
    $adddisk | add-content -path $file
    }

    #Get OS Disk
    $osdisks = Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureOSDisk ## | select -ExpandProperty MediaLink
    $osvhdname = $osdisks.MediaLink.AbsolutePath -creplace  "/vhds/"
    $osdisks.OS, $osdisks.HostCaching, $osvhdname, $osdisks.DiskLabel, $osdisks.DiskName
    $addosdisk = "{0},{1},{2},{3},{4}" -f $osdisks.OS,$osvhdname, $osdisks.HostCaching, $osdisks.Disklabel , $osdisks.DiskName
    $addosdisk | add-content -path $file

    #Import disk config
    $diskobjects  = Import-CSV $file

    #Check disk config, make sure below returns the disks associated with the VM
    $diskobjects

    #Identify OS Disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osdiskforbuild = $osdiskimport.diskName

    #Check machibe is off
    Get-AzureVM -ServiceName $sourceSvc -Name  $vmNameToMigrate

    #Drop machine and rebuild to new cls
    Remove-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

#### <a name="step-9-change-disk-caching-settings-in-csv-file-and-save"></a>Schritt 9: Datenträger Zwischenspeichern Einstellungen in der CSV-Datei ändern und speichern

Für Datenmengen sollten diese SCHREIBGESCHÜTZTEN festgelegt werden.

Für Nachverfolgungsprotokoll Datenmengen sollte diese keine festgelegt werden.

![Appendix7][17]

#### <a name="step-10-copy-vhds"></a>Schritt 10: Kopieren virtueller Festplatten
    #Ensure you have created the container for these:
    $containerName = 'vhds'

    #Create container
    New-AzureStorageContainer -Name $containerName -Context $xioContext

    ####DISK COPYING####
    #Get disks from csv, get settings for each VHDs and copy to Premium Storage accoun
    ForEach ($disk in $diskobjects)
       {
       $lun = $disk.Lun
       $vhdname = $disk.vhdname
       $cacheoption = $disk.HostCaching
       $disklabel = $disk.DiskLabel
       $diskName = $disk.DiskName
       Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname has cache setting : $cacheoption"

       #Start async copy
       Start-AzureStorageBlobCopy -srcUri "https://$origstorageaccountname.blob.core.windows.net/vhds/$vhdname" `
    -SrcContext $origContext `
    -DestContainer $containerName `
    -DestBlob $vhdname `
    -DestContext $xioContext
       }



Überprüfen Sie den Status der virtuellen Festplatten mit dem Konto Premium Speicher kopieren:

    ForEach ($disk in $diskobjects)
       {
       $lun = $disk.Lun
       $vhdname = $disk.vhdname
       $cacheoption = $disk.HostCaching
       $disklabel = $disk.DiskLabel
       $diskName = $disk.DiskName

       $copystate = Get-AzureStorageBlobCopyState -Blob $vhdname -Container $containerName -Context $xioContext
    Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname, STATUS = " $copystate.Status
       }

![Appendix8][18]

Warten Sie, bis alle diese als Erfolg gespeichert sind.

Informationen für einzelne Blobs:

    Get-AzureStorageBlobCopyState -Blob "blobname.vhd" -Container $containerName -Context $xioContext

#### <a name="step-11-register-os-disk"></a>Schritt 11: Register OS Datenträger

    #Change storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountname
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

    #Register OS disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osvhd = $osdiskimport.vhdname
    $osdiskforbuild = $osdiskimport.diskName

    #Registering OS disk, but as XIO disk
    $xioDiskName = $osdiskforbuild + "xio"
    Add-AzureDisk -DiskName $xioDiskName -MediaLocation  "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$osvhd"  -Label "BootDisk" -OS "Windows"

#### <a name="step-12-import-secondary-into-new-cloud-service"></a>Schritt 12: Importieren Sie sekundäre, in der neuen Cloud-Dienst

Im folgenden Code verwendet auch die Option hinzugefügte Hier können Sie den Computer importieren und verwenden Sie die retainable VIP.

    #Build VM Config
    $ipaddr = "192.168.0.5"
    #Remember to change to XIO
    $newInstanceSize = "Standard_DS13"
    $subnet = "SQL"

    #Create new Avaiability Set
    $availabilitySet = "cloudmigAVAMS"

    #build machine config into object
    $vmConfig = New-AzureVMConfig -Name $vmNameToMigrate -InstanceSize $newInstanceSize -DiskName $xioDiskName -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    #Reload disk config
    $diskobjects  = Import-CSV $file
    $datadiskimport = $diskobjects | where {$_.lun -ne "Windows"}

    ForEach ( $attachdatadisk in $datadiskimport)
       {
    $label = $attachdatadisk.disklabel
    $lunNo = $attachdatadisk.lun
    $hostcach = $attachdatadisk.hostcaching
    $datadiskforbuild = $attachdatadisk.diskName
    $vhdname = $attachdatadisk.vhdname

    ###Attaching disks to a VM during a deploy to a new cloud service and new storage account is different from just attaching VHDs to just a redeploy in a new cloud service
    $vmConfig | Add-AzureDataDisk -ImportFrom -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vhdname" -LUN $lunNo -HostCaching $hostcach -DiskLabel $label

    }

    #Create VM
    $vmConfig  | New-AzureVM –ServiceName $destcloudsvc –Location $location -VNetName $vnet ## Optional (-ReservedIPName $reservedVIPName)

#### <a name="step-13-create-ilb-on-new-cloud-svc-add-load-balanced-endpoints-and-acls"></a>Schritt 13: Erstellen ILB auf neue Cloud Svc, fügen Sie laden verteilt Endpunkte und ACLs
    #Check for existing ILB
    GET-AzureInternalLoadBalancer -ServiceName $destcloudsvc

    $ilb="sqlIntIlbDest"
    $subnet = "SQL"
    $IP="192.168.0.25"
    Add-AzureInternalLoadBalancer -ServiceName $destcloudsvc -InternalLoadBalancerName $ilb –SubnetName $subnet –StaticVNetIPAddress $IP

    #Endpoints
    $epname="sqlIntEP"
    $prot="tcp"
    $locport=1433
    $pubport=1433
    Get-AzureVM –ServiceName $destcloudsvc –Name $vmNameToMigrate  | Add-AzureEndpoint -Name $epname -Protocol $prot -LocalPort $locport -PublicPort $pubport -ProbePort 59999 -ProbeIntervalInSeconds 5 -ProbeTimeoutInSeconds 11  -ProbeProtocol "TCP" -InternalLoadBalancerName $ilb -LBSetName $ilb -DirectServerReturn $true | Update-AzureVM

    #SET Azure ACLs or Network Security Groups & Windows FWs

    #http://msdn.microsoft.com/library/azure/dn495192.aspx

    ####WAIT FOR FULL AlwaysOn RESYNCRONISATION!!!!!!!!!#####

####<a name="step-14-update-always-on"></a>Schritt 14: Immer auf aktualisieren.
    #Code to be executed on a Cluster Node
    $ClusterNetworkNameAmsterdam = "Cluster Network 2" # the azure cluster subnet network name
    $newCloudServiceIPAmsterdam = "192.168.0.25" # IP address of your cloud service

    $AGName = "myProductionAG"
    $ListenerName = "Mylistener"


    Add-ClusterResource "IP Address $newCloudServiceIPAmsterdam" -ResourceType "IP Address" -Group $AGName -ErrorAction Stop |  Set-ClusterParameter -Multiple @{"Address"="$newCloudServiceIPAmsterdam";"ProbePort"="59999";SubnetMask="255.255.255.255";"Network"=$ClusterNetworkNameAmsterdam;"OverrideAddressMatch"=1;"EnableDhcp"=0} -ErrorAction Stop

    #set dependancy and NETBIOS, then remove old IP address

    #set NETBIOS, then remove old IP address
    Get-ClusterGroup $AGName | Get-ClusterResource -Name "IP Address $newCloudServiceIPAmsterdam" | Set-ClusterParameter -Name EnableNetBIOS -Value 0

    #set dependency to Listener (OR Dependency) and delete previous IP Address resource that references:

    #Make sure no static records in DNS

![Appendix9][19]

Entfernen Sie jetzt alten Cloud-Dienst IP-Adresse ein.

![Appendix10][20]

#### <a name="step-15-dns-update-check"></a>Schritt 15: DNS-Aktualisierung aktivieren

Sie sollten jetzt Überprüfen der DNS-Server auf Ihre SQL Server-Client-Netzwerke und stellen Sie sicher, dass Cluster den Eintrag zusätzliche Host für die hinzugefügte IP-Adresse hinzugefügt hat. Wenn Sie diesen DNS-Server nicht aktualisiert haben, sollten Sie erwägen erzwingen eine DNS Zone zu übertragen, und stellen Sie sicher, dass die Clients in es Subnetz sind in beiden immer auf IP-Adressen auflösen, dies ist, sodass Sie nicht für die automatische DNS-Replikation warten müssen.

#### <a name="step-16-reconfigure-always-on"></a>Schritt 16: Konfigurieren immer auf neu.

An diesem Punkt warten Sie des sekundären, die migriert wurde, um erneut mit dem Knoten lokal vollständig synchronisieren und wechseln Sie zum Knoten synchroner Replikation und machen Sie es mit der AFP Knotens.  

#### <a name="step-17-migrate-second-node"></a>Schritt 17: Migrieren Sie zweiten Knoten
    $vmNameToMigrate="dansqlams1"

    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

    #Get endpoint information
    $endpoint = Get-AzureVM -ServiceName $sourceSvc  -Name $vmNameToMigrate | Get-AzureEndpoint

    #Shutdown VM
    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | stop-AzureVM

    #Get disk config

    #Building Existing Data Disk Configuration
    $file = "C:\Azure Storage Testing\mydiskconfig_$vmNameToMigrate.csv"
    $datadisks = @(Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureDataDisk )
    Add-Content $file “lun, vhdname, hostcaching, disklabel, diskName”
    foreach ($disk in $datadisks)
    {
      $vhdname = $disk.MediaLink.AbsolutePath -creplace  "/vhds/"
      $disk.Lun, , $disk.HostCaching, $vhdname, $disk.DiskLabel,$disks.DiskName
    # Write-Host "copying disk $disk"
    $adddisk = "{0},{1},{2},{3},{4}" -f $disk.Lun,$vhdname, $disk.HostCaching, $disk.DiskLabel, $disk.DiskName
    $adddisk | add-content -path $file
    }

    #Get OS Disk
    $osdisks = Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureOSDisk ## | select -ExpandProperty MediaLink
    $osvhdname = $osdisks.MediaLink.AbsolutePath -creplace  "/vhds/"
    $osdisks.OS, $osdisks.HostCaching, $osvhdname, $osdisks.DiskLabel, $osdisks.DiskName
    $addosdisk = "{0},{1},{2},{3},{4}" -f $osdisks.OS,$osvhdname, $osdisks.HostCaching, $osdisks.Disklabel , $osdisks.DiskName
    $addosdisk | add-content -path $file

    #Import disk config
    $diskobjects  = Import-CSV $file

    #Check disk configuration
    $diskobjects

    #Identify OS Disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osdiskforbuild = $osdiskimport.diskName

    #Check machine is off
    Get-AzureVM -ServiceName $sourceSvc -Name  $vmNameToMigrate

    #Drop machine and rebuild to new cls
    Remove-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

#### <a name="step-18-change-disk-caching-settings-in-csv-file-and-save"></a>Schritt 18: Datenträger Zwischenspeichern Einstellungen in der CSV-Datei ändern und speichern

Für Datenmengen sollten diese SCHREIBGESCHÜTZTEN festgelegt werden.

Für Nachverfolgungsprotokoll Datenmengen sollte diese keine festgelegt werden.

![Appendix11][21]

#### <a name="step-19-create-new-independent-storage-account-for-secondary-node"></a>Schritt 19: Erstellen Sie neuer unabhängige Speicher-Kontos zum sekundären Knoten
    $newxiostorageaccountnamenode2 = "danspremsams2"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountnamenode2 -Location $location -Type "Premium_LRS"  

    #Reset the storage account src if node 1 in a different storage account
    $origstorageaccountname2nd = "danstdams2"

    #Generate storage keys for later
    $xiostoragenode2 = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountnamenode2

    #Generate storage acc contexts
    $xioContextnode2 = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountnamenode2 -StorageAccountKey $xiostoragenode2.Primary  

    #Set up subscription and default storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountnamenode2
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

#### <a name="step-20-copy-vhds"></a>Schritt 20: Kopieren virtueller Festplatten
    #Ensure you have created the container for these:
    $containerName = 'vhds'

    #Create container
    New-AzureStorageContainer -Name $containerName -Context $xioContextnode2  

    ####DISK COPYING####
    ##get disks from csv, get settings for each VHDs and copy to Premium Storage accoun
    ForEach ($disk in $diskobjects)
       {
       $lun = $disk.Lun
       $vhdname = $disk.vhdname
       $cacheoption = $disk.HostCaching
       $disklabel = $disk.DiskLabel
       $diskName = $disk.DiskName
       Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname has cache setting : $cacheoption"

       #Start async copy
       Start-AzureStorageBlobCopy -srcUri "https://$origstorageaccountname2nd.blob.core.windows.net/vhds/$vhdname" `
        -SrcContext $origContext `
        -DestContainer $containerName `
        -DestBlob $vhdname `
        -DestContext $xioContextnode2
       }

    #Check for copy progress

    #check induvidual blob status
    Get-AzureStorageBlobCopyState -Blob "danRegSvcAms-dansqlams1-2014-07-03.vhd" -Container $containerName -Context $xioContext


Sie können den virtuelle Festplatte kopieren Status für alle virtuellen Festplatten überprüfen: ForEach ($disk in $diskobjects) {$lun = $disk. LUN $vhdname = $disk.vhdname $cacheoption = $disk. HostCaching $disklabel = $disk. Diskettenetikett $diskName = $disk. DiskName

       $copystate = Get-AzureStorageBlobCopyState -Blob $vhdname -Container $containerName -Context $xioContextnode2
    Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname, STATUS = " $copystate.Status
       }

![Appendix12][22]

Warten Sie, bis alle diese als Erfolg gespeichert sind.

Informationen für einzelne Blobs: #Check Induvidual Blob Status Get-AzureStorageBlobCopyState-BLOB-"DanRegSvcAms-dansqlams1-2014-07-03.vhd"-Container $containerName-Kontext $xioContextnode2

#### <a name="step-21-register-os-disk"></a>Schritt 21: Register OS Datenträger
    #change storage account to the new XIO storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountnamenode2
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

    #Register OS disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osvhd = $osdiskimport.vhdname
    $osdiskforbuild = $osdiskimport.diskName

    #Registering OS disk, but as XIO disk
    $xioDiskName = $osdiskforbuild + "xio"
    Add-AzureDisk -DiskName $xioDiskName -MediaLocation  "https://$newxiostorageaccountnamenode2.blob.core.windows.net/vhds/$osvhd"  -Label "BootDisk" -OS "Windows"

    #Build VM Config
    $ipaddr = "192.168.0.4"
    $newInstanceSize = "Standard_DS13"

    #Join to existing Avaiability Set

    #Build machine config into object
    $vmConfig = New-AzureVMConfig -Name $vmNameToMigrate -InstanceSize $newInstanceSize -DiskName $xioDiskName -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    #Reload disk config
    $diskobjects  = Import-CSV $file
    $datadiskimport = $diskobjects | where {$_.lun -ne "Windows"}

    ForEach ( $attachdatadisk in $datadiskimport)
       {
    $label = $attachdatadisk.disklabel
    $lunNo = $attachdatadisk.lun
    $hostcach = $attachdatadisk.hostcaching
    $datadiskforbuild = $attachdatadisk.diskName
    $vhdname = $attachdatadisk.vhdname

    ###This is different to just a straight cloud service change
    #note if you do not have a disk label the command below will fail, populate as required.
    $vmConfig | Add-AzureDataDisk -ImportFrom -MediaLocation "https://$newxiostorageaccountnamenode2.blob.core.windows.net/vhds/$vhdname" -LUN $lunNo -HostCaching $hostcach -DiskLabel $label

    }

    #Create VM
    $vmConfig  | New-AzureVM –ServiceName $destcloudsvc –Location $location -VNetName $vnet -Verbose

#### <a name="step-22-add-load-balanced-endpoints-and-acls"></a>Schritt 22: Hinzufügen von laden verteilt Endpunkte und ACLs
    #Endpoints
    $epname="sqlIntEP"
    $prot="tcp"
    $locport=1433
    $pubport=1433
    Get-AzureVM –ServiceName $destcloudsvc –Name $vmNameToMigrate  | Add-AzureEndpoint -Name $epname -Protocol $prot -LocalPort $locport -PublicPort $pubport -ProbePort 59999 -ProbeIntervalInSeconds 5 -ProbeTimeoutInSeconds 11  -ProbeProtocol "TCP" -InternalLoadBalancerName $ilb -LBSetName $ilb -DirectServerReturn $true | Update-AzureVM


    #STOP!!! CHECK in the Azure classic portal or Machine Endpoints through powershell that these Endpoints are created!

    #SET ACLs or Azure Network Security Groups & Windows FWs

    #http://msdn.microsoft.com/library/azure/dn495192.aspx

#### <a name="step-23-test-failover"></a>Schritt 23: Testen der failover

Wir Sie nun den migrierten Knoten mit dem lokal immer auf Knoten synchronisieren, platzieren Sie ihn in synchroner Replikationsmodus und warten, bis es synchronisiert wird. Klicken Sie dann migriert Failover aus auf Prem auf den ersten Knoten, welche die AFP ist. Sobald die gearbeitet hat, ändern Sie den letzten migrierten Knoten in der AFP aus.

Sie sollten Failovers zwischen allen Knoten testen und ausführen, obwohl Chaos überprüft, um sicherzustellen, dass Failovers Arbeit als erwartet und in einem schnell Weise zu nutzen.

#### <a name="step-24-put-back-cluster-quorum-settings--dns-ttl--failover-pntrs--sync-settings"></a>Schritt 24: Setzen Sie wieder Cluster Quorum Einstellungen / DNS TTL / Failover Pntrs / Synchronisierungseinstellungen
##### <a name="adding-ip-address-resource-on-same-subnet"></a>Hinzufügen von IP-Adressen-Ressource in demselben Subnetz

Wenn Sie nur 2 SQL Server haben und diese auf einem neuen Cloud-Dienst migrieren möchten, aber sie im selben Subnetz beibehalten möchten, können Sie vermeiden, die Zuhörer offline Löschen der ursprünglichen immer auf IP-Adresse ein, und fügen Sie die neue IP-Adresse aufzeichnen. Wenn Sie die virtuelle Computer in ein anderes Subnetz migrieren, müssen Sie nicht dazu, wie eine zusätzliche Cluster-Netzwerk, in denen das Subnetz verwiesen werden kann.

Sobald Sie von der migrierte sekundären eingebracht und in die neue IP-Adresse Ressource für den neuen Clouddienst vor dem Failover vorhandenen primären hinzugefügt haben, sollten Sie diese Schritte innerhalb der Cluster Failover-Manager durchführen:

Um die IP-Adresse hinzufügen möchten, finden Sie im [Anhang](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage), Schritt 14.

1. Ändern Sie für die aktuelle Ressource IP-Adresse den möglichen Besitzer auf 'Vorhandene primäre SQL Server', im folgenden Beispiel, 'dansqlams4':

    ![Appendix13][23]

1. Ändern Sie den möglichen Besitzer aus 'Migriert sekundäre SQL Server', im folgenden Beispiel, 'dansqlams5' für die neue Ressource für die IP-Adresse:

    ![Appendix14][24]

1. Nachdem Sie dies festgelegt haben, können Sie Failover, und bei der Migration des letzten Knotens die möglichen Besitzer sollte bearbeitet werden, damit der Knoten als möglicher Besitzer hinzugefügt wird:

    ![Appendix15][25]

## <a name="additional-resources"></a>Zusätzliche Ressourcen
- [Premium Azure-Speicher](../storage/storage-premium-storage.md)
- [Virtuellen Computern](https://azure.microsoft.com/services/virtual-machines/)
- [SQLServer auf Azure-virtuellen Computern](virtual-machines-windows-sql-server-iaas-overview.md)

<!-- IMAGES -->
[1]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/1_VNET_Portal.png
[2]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/2_Diskname_Lun.png
[3]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/3_Virtual_Disk_Properties.png
[4]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/4_Virtual_Disk_Properties_Details.png
[5]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/5_Get_Storage_Pool.png
[6]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/6_Deployments_Always_On.png
[7]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/7_Add_More_Secondaries.png
[8]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/8_Use_Existing_Secondary_Single_Site.png
[9]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/9_Use_Existing_Secondary_Multi_Site.png
[10]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/9_Use_Existing_Secondary_Multi_Site_b.png
[11]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_01.png
[12]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_02.png
[13]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_03.png
[14]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_04.png
[15]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_05.png
[16]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_06.png
[17]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_07.png
[18]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_08.png
[19]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_09.png
[20]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_10.png
[21]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_11.png
[22]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_12.png
[23]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_13.png
[24]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_14.png
[25]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_15.png

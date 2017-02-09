<properties
    pageTitle="Erstellen Sie ein Windows-virtueller Computer mit PowerShell | Microsoft Azure"
    description="Windows-virtuellen Computern mit Azure PowerShell und das Bereitstellungsmodell klassischen zu erstellen."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="cynthn"/>

# <a name="create-a-windows-virtual-machine-with-powershell-and-the-classic-deployment-model"></a>Erstellen von einem Windows-Computer mit PowerShell und das Bereitstellungsmodell klassischen 

> [AZURE.SELECTOR]
- [Azure klassischen Portal – Windows](virtual-machines-windows-classic-tutorial.md)
- [PowerShell - Windows](virtual-machines-windows-classic-create-powershell.md)

<br>


[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Erfahren Sie, wie Sie [mithilfe des Modells Ressourcenmanager Schritte ausführen](virtual-machines-windows-ps-create.md).


Diese Schritte zeigen, wie eine Reihe von Azure PowerShell-Befehlen anpassen, die erstellen und einem Windows-basierten Azure-virtuellen Computern mithilfe eines Bausteins Ansatzes vorkonfiguriert. Sie können dieses Verfahren verwenden, um schnell einen Befehlssatz für eine neue Windows-basiertem virtuellen Computern erstellen und Erweitern einer vorhandenen bereitstellungs oder mehrere Befehlssätze erstellen, die eine benutzerdefinierte Test-/oder die IT pro-Umgebung schnell zu erstellen.

Diese Schritte eines ausfüllen-in-the-Leerzeichen Ansatzes zum Erstellen von Azure PowerShell Befehlssätze. Dieser Ansatz kann hilfreich sein, wenn Sie noch neu bei PowerShell sind oder nur wissen, welche Werte für die erfolgreiche Konfiguration angeben möchten. Fortgeschrittene PowerShell-Benutzer können die Befehle und Ersetzen durch eigene Werte für die Variablen (die Zeilen, die mit "$" beginnen).

Wenn dies nicht erfolgt noch, führen Sie die Anweisungen in [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) Azure PowerShell auf Ihrem lokalen Computer installieren. Öffnen Sie dann eine Windows PowerShell-Eingabeaufforderung aus.

## <a name="step-1-add-your-account"></a>Schritt 1: Fügen Sie Ihres Kontos hinzu

1. PowerShell dazu aufgefordert werden Geben Sie **-AzureAccount hinzufügen** , und klicken Sie auf die **EINGABETASTE**. 
2. Geben Sie die e-Mail-Adresse, die mit Ihrem Azure-Abonnement verknüpft ist, und klicken Sie auf **Weiter**. 
3. Geben Sie das Kennwort für Ihr Konto ein. 
4. Klicken Sie auf **Anmelden**.

## <a name="step-2-set-your-subscription-and-storage-account"></a>Schritt 2: Legen Sie Ihr Abonnement und Speicher-Konto

Legen Sie Ihre Azure-Abonnement und Speicher-Konto durch Ausführen der folgenden Befehle in der Windows PowerShell-Befehlszeile. Ersetzen Sie alles innerhalb der Anführungszeichen, einschließlich der Zeichen, die mit den richtigen Namen < und >.

    $subscr="<subscription name>"
    $staccount="<storage account name>"
    Select-AzureSubscription -SubscriptionName $subscr –Current
    Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

Sie können die richtige Abonnementname aus der Eigenschaft SubscriptionName der Ausgabe des Befehls **Get-AzureSubscription** erhalten. Sie können den richtigen Speicher Kontonamen aus der Eigenschaft Beschriftung der Ausgabe des Befehls **Get-AzureStorageAccount** erhalten, nachdem Sie den **Wählen-AzureSubscription** Befehl ausführen.

## <a name="step-3-determine-the-imagefamily"></a>Schritt 3: Ermitteln der ImageFamily

Als Nächstes müssen Sie bestimmen den Wert ImageFamily oder Beschriftung für das bestimmte Bild entspricht der Azure-virtuellen Computern, die Sie erstellen möchten. Sie können die Liste der verfügbaren ImageFamily Werte mit dem folgenden Befehl abrufen.

    Get-AzureVMImage | select ImageFamily -Unique

Hier sind einige Beispiele für ImageFamily Werte für Windows-Computer aus:

- Windows Server 2012 R2 Datacenter
- Windows Server 2008 R2 SP1
- Windows Server 2016 Technical Preview 4
- SQL Server 2012 SP1 Enterprise unter WindowsServer 2012

Wenn Sie das Bild zu, die, das Ihnen gesuchten suchen, öffnen Sie eine neue Instanz von Text-Editor Ihrer Wahl oder PowerShell Integrated Scripting Umgebung (ISE). Fügen Sie den folgenden neuen Textdatei oder der PowerShell ISE, ersetzen den Wert ImageFamily.

    $family="<ImageFamily value>"
    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

In einigen Fällen ist der Namen des Bilds in der Eigenschaft Beschriftung anstelle des Werts ImageFamily aus. Wenn Sie das Bild, das Sie gefunden haben für die Verwendung der ImageFamily Eigenschaft suchen, werden Listen Sie die Bilder auf, indem Sie die Eigenschaft Beschriftung mit dem folgenden Befehl.

    Get-AzureVMImage | select Label -Unique

Wenn Sie das richtige Bild mit dem folgenden Befehl gefunden haben, öffnen Sie eine neue Instanz von einem Text-Editor Ihrer Wahl oder der PowerShell ISE. Kopieren Sie die folgende in die neue Textdatei oder der PowerShell ISE, ersetzen den Wert für die Beschriftung ein.

    $label="<Label value>"
    $image = Get-AzureVMImage | where { $_.Label -eq $label } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

## <a name="step-4-build-your-command-set"></a>Schritt 4: Erstellen Sie Ihrer Befehlssatz

Erstellen Sie den Rest des Befehls festlegen, indem Sie den entsprechenden Satz von Bausteinen unten in der neuen Textdatei oder die ISE kopieren und den Variablenwerten ausfüllen und Entfernen von der < und > Zeichen. Finden Sie in den beiden [Beispiele](#examples) am Ende dieses Artikels für eine Vorstellung des Gesamtergebnisses aus.

Starten Sie den Befehl wählen Sie eine der folgenden zwei Befehl Blöcke (erforderlich).

Option 1: Geben Sie einen Namen für die virtuellen Computern und eine Größe.

    $vmname="<machine name>"
    $vmsize="<Specify one: Small, Medium, Large, ExtraLarge, A5, A6, A7, A8, A9>"
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

Option 2: Geben Sie einen Namen, die Größe und die Verfügbarkeit SetName.

    $vmname="<machine name>"
    $vmsize="<Specify one: Small, Medium, Large, ExtraLarge, A5, A6, A7, A8, A9>"
    $availset="<set name>"
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image -AvailabilitySetName $availset

Die InstanceSize Werte für D, DS oder G-Serie virtuellen Computern finden Sie unter [virtuellen Computern und Cloud-Dienst Größen für Azure](https://msdn.microsoft.com/library/azure/dn197896.aspx).

>[AZURE.NOTE] Wenn Sie ein Enterprise Agreement mit Software Assurance haben und die Windows Server- [Hybrid verwenden Vorteil](https://azure.microsoft.com/pricing/hybrid-use-benefit/)nutzen möchten, mit dem **New-AzureVMConfig** -Cmdlet übergeben des Werts **Windows_Server** für den Fall typische Nutzung fügen Sie den **LicenseType -** Parameter hinzu.  Achten Sie darauf, dass Sie ein Bild verwenden, die Sie hochgeladen haben. Sie können kein Standardbild mit dem Hybrid verwenden Vorteil aus dem Katalog verwenden.

Geben Sie optional für einen eigenständigen Windows-Computer die lokale Administratorkonto und das Kennwort.

    $cred=Get-Credential -Message "Type the name and password of the local administrator account."
    $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.Username -Password $cred.GetNetworkCredential().Password

Wählen Sie ein sicheres Kennwort ein. Wenn um seine Stärke zu überprüfen, besuchen Sie [Kennwort Rechtschreibprüfung: sicherer Kennwörter verwenden](https://www.microsoft.com/security/pc-security/password-checker.aspx).

Geben Sie optional den Windows-Computer zu einer vorhandenen Active Directory-Domäne hinzufügen, das lokale Administratorkonto und Ihr Kennwort ein, die Domäne, Name und Kennwort eines Kontos Domäne.

    $cred1=Get-Credential –Message "Type the name and password of the local administrator account."
    $cred2=Get-Credential –Message "Now type the name (not including the domain) and password of an account that has permission to add the machine to the domain."
    $domaindns="<FQDN of the domain that the machine is joining>"
    $domacctdomain="<domain of the account that has permission to add the machine to the domain>"
    $vm1 | Add-AzureProvisioningConfig -AdminUsername $cred1.Username -Password $cred1.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $cred2.Username -DomainPassword $cred2.GetNetworkCredential().Password -JoinDomain $domaindns

Zusätzliche Optionen vor Konfiguration für Windows-basierten virtuellen Maschinen finden Sie unter legt die Syntax für **Windows** und **WindowsDomain** -Parameter in [AzureProvisioningConfig hinzufügen](https://msdn.microsoft.com/library/azure/dn495299.aspx).

Ordnen Sie des virtuellen Computers optional eine bestimmte IP-Adresse, die als eine statische DIP bezeichnet.

    $vm1 | Set-AzureStaticVNetIP -IPAddress <IP address>

Sie können überprüfen, ob eine bestimmte IP-Adresse mit verfügbar ist:

    Test-AzureStaticVNetIP –VNetName <VNet name> –IPAddress <IP address>

Ordnen Sie optional des virtuellen Computers zu einem bestimmten Subnetz in einem Azure-virtuellen Netzwerk ein.

    $vm1 | Set-AzureSubnet -SubnetNames "<name of the subnet>"

Optional können Sie einen einzelnen Datenträger des virtuellen Computers hinzufügen.

    $disksize=<size of the disk in GB>
    $disklabel="<the label on the disk>"
    $lun=<Logical Unit Number (LUN) of the disk>
    $hcaching="<Specify one: ReadOnly, ReadWrite, None>"
    $vm1 | Add-AzureDataDisk -CreateNew -DiskSizeInGB $disksize -DiskLabel $disklabel -LUN $lun -HostCaching $hcaching

Legen Sie für einen Active Directory-Domäne-Controller $hcaching "Keine" aus.

Optional Hinzufügen des virtuellen Computers zu einem bestehenden Satz der Lastenausgleich für den externen Datenverkehr ein.

    $protocol="<Specify one: tcp, udp>"
    $localport=<port number of the internal port>
    $pubport=<port number of the external port>
    $endpointname="<name of the endpoint>"
    $lbsetname="<name of the existing load-balanced set>"
    $probeprotocol="<Specify one: tcp, http>"
    $probeport=<TCP or HTTP port number of probe traffic>
    $probepath="<URL path for probe traffic>"
    $vm1 | Add-AzureEndpoint -Name $endpointname -Protocol $protocol -LocalPort $localport -PublicPort $pubport -LBSetName $lbsetname -ProbeProtocol $probeprotocol -ProbePort $probeport -ProbePath $probepath

Wählen Sie schließlich eine dieser Blöcke erforderlichen Befehl zum Erstellen des virtuellen Computers.

Option 1: Erstellen des virtuellen Computers in einem vorhandenen Clouddienst an.

    New-AzureVM –ServiceName "<short name of the cloud service>" -VMs $vm1

Der Kurzname der Cloud-Dienst ist der Name, der in der Liste der Cloud Services im klassischen Azure-Portal oder in der Liste der Ressourcengruppen im Azure-Portal angezeigt wird.

Option 2: Erstellen des virtuellen Computers in einer vorhandenen Cloud-Dienst und virtuelle Netzwerk an.

    $svcname="<short name of the cloud service>"
    $vnetname="<name of the virtual network>"
    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname

## <a name="step-5-run-your-command-set"></a>Schritt 5: Ausführen der Befehlssatz

Überprüfen Sie den Azure PowerShell-Befehlssatz, die, den Sie in Ihrem Text-Editor oder der PowerShell ISE mehrere Blöcke von Befehlen aus Schritt 4 aus erstellt. Stellen Sie sicher, dass Sie alle erforderlichen Variablen angegeben haben und über die richtigen Werte verfügen. Vergewissern Sie sich, dass Sie alle entfernt haben außerdem die < und > Zeichen.

Wenn Sie einen Text-Editor verwenden, den Befehlssatz in die Zwischenablage kopieren und anschließend mit der Maustaste der geöffneten Windows PowerShell-Eingabeaufforderung. Dies wird den Befehlssatz als eine Reihe von PowerShell-Befehlen ausgeben und erstellen Sie Ihre Azure-virtuellen Computern. Alternativ führen Sie den Befehl in der PowerShell ISE festlegen.

Wenn Sie dieses virtuellen Computers erneut oder eine ähnliche erstellen, können Sie:

- Speichern Sie diesen Befehl als Skriptdatei PowerShell (ps1) festlegen.
- Speichern Sie diesen Befehl, legen Sie im Abschnitt **Automatisierung** des Portals Azure klassischen als eine Azure Automatisierung Runbooks an.

## <a name="a-idexamplesaexamples"></a><a id="examples"></a>Beispiele

Hier sind zwei Beispiele für die obigen Schritte verwenden, um Azure PowerShell Befehl Datensätze zu erstellen, die Windows-basiertem Azure-virtuellen Computern erstellen aus.

### <a name="example-1"></a>Beispiel 1

Benötige ich eine PowerShell Befehl legen Sie auf den ersten virtuellen Computer für einen Active Directory-Domäne-Controller zu erstellen, die:

- Windows Server 2012 R2 Datacenter Bild verwendet.
- Hat den Namen AZDC1.
- Ist ein eigenständiges Computer.
- Enthält eine zusätzliche Festplatte von 20 GB an.
- Enthält die statische IP-Adresse 192.168.244.4 an.
- Befindet sich die Back-End-Subnetz des virtuellen Netzwerks AZDatacenter.
- In der Cloud-Dienst Azure-TailspinToys ist.

Hier wird der entsprechende Azure PowerShell-Befehlssatz dieses virtuellen Computers, mit leere Zeilen zwischen jeden Zellenblock, um die Lesbarkeit zu erstellen.

    $family="Windows Server 2012 R2 Datacenter"
    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1
    $vmname="AZDC1"
    $vmsize="Medium"
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    $cred=Get-Credential -Message "Type the name and password of the local administrator account."
    $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.Username -Password $cred.GetNetworkCredential().Password

    $vm1 | Set-AzureSubnet -SubnetNames "BackEnd"

    $vm1 | Set-AzureStaticVNetIP -IPAddress 192.168.244.4

    $disksize=20
    $disklabel="DCData"
    $lun=0
    $hcaching="None"
    $vm1 | Add-AzureDataDisk -CreateNew -DiskSizeInGB $disksize -DiskLabel $disklabel -LUN $lun -HostCaching $hcaching

    $svcname="Azure-TailspinToys"
    $vnetname="AZDatacenter"
    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname

### <a name="example-2"></a>Beispiel 2

Benötige ich eine PowerShell Befehl zum Einrichten ein virtuellen Computers für eine Line-of-Business-Server erstellen, die:

- Windows Server 2012 R2 Datacenter Bild verwendet.
- Hat den Namen LOB1.
- Ist ein Mitglied der Domäne corp.contoso.com an.
- Verfügt über eine zusätzliche Datenträger von 200 GB aus.
- Befindet sich das Front-End-Subnetz des virtuellen Netzwerks AZDatacenter.
- In der Cloud-Dienst Azure-TailspinToys ist.

Hier sind der entsprechende Azure-PowerShell-Befehl festlegen, um dieses virtuellen Computers zu erstellen.

    $family="Windows Server 2012 R2 Datacenter"
    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1
    $vmname="LOB1"
    $vmsize="Large"
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    $cred1=Get-Credential –Message "Type the name and password of the local administrator account."
    $cred2=Get-Credential –Message "Now type the name (not including the domain) and password of an account that has permission to add the machine to the domain."
    $domaindns="corp.contoso.com"
    $domacctdomain="CORP"
    $vm1 | Add-AzureProvisioningConfig -AdminUsername $cred1.Username -Password $cred1.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $cred2.Username -DomainPassword $cred2.GetNetworkCredential().Password -JoinDomain $domaindns

    $vm1 | Set-AzureSubnet -SubnetNames "FrontEnd"

    $disksize=200
    $disklabel="LOBData"
    $lun=0
    $hcaching="ReadWrite"
    $vm1 | Add-AzureDataDisk -CreateNew -DiskSizeInGB $disksize -DiskLabel $disklabel -LUN $lun -HostCaching $hcaching

    $svcname="Azure-TailspinToys"
    $vnetname="AZDatacenter"
    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname


## <a name="next-steps"></a>Nächste Schritte

Wenn Sie einen Datenträger OS, der größer als 127 GB ist benötigen, können Sie [das Laufwerk OS zu erweitern](virtual-machines-windows-expand-os-disk.md).





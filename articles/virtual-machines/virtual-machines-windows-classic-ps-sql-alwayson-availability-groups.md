<properties
    pageTitle="Konfigurieren Sie immer auf Verfügbarkeit Gruppe Azure-virtuellen Computer mit PowerShell"
    description="In diesem Lernprogramm verwendet Ressourcen, die mit dem klassischen Bereitstellungsmodell erstellt, und PowerShell zum immer auf Verfügbarkeit Erstellen einer Gruppe in Azure."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/22/2016"
    ms.author="mikeray" />

# <a name="configure-always-on-availability-group-in-azure-vm-with-powershell"></a>Konfigurieren Sie immer auf Verfügbarkeit Gruppe Azure-virtuellen Computer mit PowerShell

> [AZURE.SELECTOR]
- [Ressourcenmanager: Vorlage](virtual-machines-windows-portal-sql-alwayson-availability-groups.md)
- [Ressourcenmanager: Manueller](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)
- [Klassische: Benutzeroberfläche](virtual-machines-windows-classic-portal-sql-alwayson-availability-groups.md)
- [Klassische: PowerShell](virtual-machines-windows-classic-ps-sql-alwayson-availability-groups.md)

<br/>

> [AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

Azure-virtuellen Computern (virtuellen Computern) hilft Datenbankadministratoren niedriger die Kosten für eine hohe Verfügbarkeit von SQL Server-System implementiert wird. In diesem Lernprogramm erfahren Sie, wie eine Verfügbarkeit Gruppe mit SQL Server immer auf End-to-End innerhalb einer Azure-Umgebung implementiert wird. Am Ende des Lernprogramms wird Ihre Lösung SQL Server immer auf in Azure die folgenden Elemente umfassen:

- Ein virtuelles Netzwerk, mehrere Subnetze, einschließlich einer Front-End- und Back-End-Subnetz enthält

- Eine Domäne-Controller mit einer Domäne Active Directory (AD)

- Zwei SQL Server virtueller Computer mit dem Back-End-Subnetz bereitgestellt, und der AD-Domäne

- Eine 3-Knoten WSFC Cluster mit den Knoten Mehrzahl Quorummodell

- Eine Gruppe Verfügbarkeit mit zwei synchroner Commit Replikaten einer Datenbank Verfügbarkeit

Dieses Szenario wird nicht für die Kosten Effektivität oder andere Faktoren auf Azure seine Vereinfachung ausgewählt. Beispielsweise können Sie die Anzahl der virtuellen Computern für eine Gruppe von zwei-Replikat Verfügbarkeit minimieren, um mithilfe des Domänencontrollers als Quorum Dateifreigabenzeuge in einer 2-Knoten WSFC Cluster auf berechnen Stunden in Azure zu speichern. Diese Methode verringert die Anzahl der virtuellen Computer mit einem aus der oben genannten Konfiguration.

In diesem Lernprogramm soll Sie die Schritte erforderlich, um die beschriebenen Lösung über einzurichten, ohne Ausarbeitung auf die Details der einzelnen Schritte anzeigen. Daher verwendet er statt mit der Benutzeroberfläche Konfigurationsschritte, PowerShell-Skripting, um Sie schnell durch die einzelnen Schritte ausführen. Es wird Folgendes vorausgesetzt:

- Sie verfügen bereits über ein Azure-Konto mit dem Abonnement virtuellen Computern.

- Sie haben die [Azure PowerShell-Cmdlets](../powershell-install-configure.md)installiert.

- Sie verfügen bereits über ein grundlegendes Verständnis der immer auf Verfügbarkeit Gruppen für lokal Lösungen. Weitere Informationen finden Sie unter [Immer auf Verfügbarkeit Gruppen (SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx).

## <a name="connect-to-your-azure-subscription-and-create-the-virtual-network"></a>Eine Verbindung mit Ihrem Abonnement Azure und das virtuelle Netzwerk erstellen

1. Importieren Sie in einem PowerShell-Fenster auf Ihrem lokalen Computer Azure-Modul, herunterladen Sie eine Veröffentlichungswebsite-Einstellungsdatei auf den Computer und verbinden Sie Ihre PowerShell-Sitzung zu Ihrem Abonnement Azure durch Importieren heruntergeladenen Einstellungen für die Veröffentlichung.

        Import-Module "C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\Azure\Azure.psd1"
        Get-AzurePublishSettingsFile
        Import-AzurePublishSettingsFile <publishsettingsfilepath>

    Der Befehl **Get-AzurePublishgSettingsFile** generiert automatisch eine Management Zertifikat mit Azure lädt diese auf Ihrem Computer. Ein Browser wird automatisch geöffnet werden und Sie werden aufgefordert, geben Sie die Anmeldeinformationen des Microsoft-Kontos für Ihr Abonnement Azure. Die heruntergeladene **publishsettings** -Datei enthält alle Informationen, die Sie benötigen, um Ihre Azure-Abonnement zu verwalten. Importieren Sie nach dem speichern diese Datei in einem lokalen Verzeichnis ein, mit dem Befehl **Importieren-AzurePublishSettingsFile** aus.

    >[AZURE.NOTE] Die Datei Publishsettings enthält Ihre Anmeldeinformationen (unverschlüsselter), die zur Verwaltung von Azure-Abonnements und Dienstleistungen verwendet werden. Die Erhöhung der Sicherheit für diese Datei ist vorübergehend außerhalb der Quellverzeichnisse (beispielsweise im Ordner Libraries\Documents) speichern und löschen Sie ihn nach Abschluss des Importvorgangs hat. Ein bösartiger Benutzer den Zugriff auf die Datei Publishsettings kann bearbeiten, erstellen und löschen Ihre Azure-Dienste.

1. Definieren Sie eine Reihe von Variablen, mit denen Sie Ihre Cloud IT-Infrastruktur zu erstellen.

        $location = "West US"
        $affinityGroupName = "ContosoAG"
        $affinityGroupDescription = "Contoso SQL HADR Affinity Group"
        $affinityGroupLabel = "IaaS BI Affinity Group"
        $networkConfigPath = "C:\scripts\Network.netcfg"
        $virtualNetworkName = "ContosoNET"
        $storageAccountName = "<uniquestorageaccountname>"
        $storageAccountLabel = "Contoso SQL HADR Storage Account"
        $storageAccountContainer = "https://" + $storageAccountName + ".blob.core.windows.net/vhds/"
        $winImageName = (Get-AzureVMImage | where {$_.Label -like "Windows Server 2008 R2 SP1*"} | sort PublishedDate -Descending)[0].ImageName
        $sqlImageName = (Get-AzureVMImage | where {$_.Label -like "SQL Server 2012 SP1 Enterprise*"} | sort PublishedDate -Descending)[0].ImageName
        $dcServerName = "ContosoDC"
        $dcServiceName = "<uniqueservicename>"
        $availabilitySetName = "SQLHADR"
        $vmAdminUser = "AzureAdmin"
        $vmAdminPassword = "Contoso!000"
        $workingDir = "c:\scripts\"

    Achten Sie auf den folgenden um sicherzustellen, dass die Befehle später erfolgreich ist:

    - Variablen **$storageAccountName** und **$dcServiceName** müssen eindeutig sein, da sie verwendet werden, um Ihre Cloud-Speicher-Konto und Cloud Server, Hilfethemas im Internet zu identifizieren.

    - Namen für die Variablen **$affinityGroupName** und **$virtualNetworkName** angegeben werden im Konfigurationsdokument virtuelles Netzwerk konfiguriert, die später verwendet werden.

    - **$sqlImageName** gibt den aktualisierten Namen des Bilds virtueller Computer, die SQL Server 2012 Service Pack 1 Enterprise Edition enthält.

    - Zur Vereinfachung **Contoso! 000** dasselbe Kennwort in das gesamte Lernprogramm verwendet wird.

1. Erstellen Sie eine Gruppe für die Zugehörigkeit.

        New-AzureAffinityGroup `
            -Name $affinityGroupName `
            -Location $location `
            -Description $affinityGroupDescription `
            -Label $affinityGroupLabel

1. Erstellen Sie ein virtuelles Netzwerk durch Importieren einer Konfigurationsdatei an.

        Set-AzureVNetConfig `
            -ConfigurationPath $networkConfigPath

    Die Konfigurationsdatei enthält das folgende XML-Dokument. Kurz gesagt, es gibt ein virtuelles Netzwerk **ContosoNET** in die Zugehörigkeit Gruppe mit der Bezeichnung **ContosoAG**aufgerufen, und die Adresse Leerzeichen **10.10.0.0/16** hat und verfügt über zwei Subnetze, **10.10.1.0/24** und **10.10.2.0/24**, die im Subnetz Vorder- und Subnetz, Hilfethemas zurück. Das front Subnetz ist, wo Sie können Clientanwendungen wie Microsoft SharePoint platzieren und das Zurücksenden Subnetz ist, in denen Sie die SQL Server-virtuellen Computern platzieren werden. Wenn Sie die Variablen **$affinityGroupName** und **$virtualNetworkName** zuvor ändern, müssen Sie auch die entsprechenden Namen unten ändern.

        <NetworkConfiguration xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
          <VirtualNetworkConfiguration>
            <Dns />
            <VirtualNetworkSites>
              <VirtualNetworkSite name="ContosoNET" AffinityGroup="ContosoAG">
                <AddressSpace>
                  <AddressPrefix>10.10.0.0/16</AddressPrefix>
                </AddressSpace>
                <Subnets>
                  <Subnet name="Front">
                    <AddressPrefix>10.10.1.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="Back">
                    <AddressPrefix>10.10.2.0/24</AddressPrefix>
                  </Subnet>
                </Subnets>
              </VirtualNetworkSite>
            </VirtualNetworkSites>
          </VirtualNetworkConfiguration>
        </NetworkConfiguration>

1. Erstellen Sie ein Speicherkonto, die Sie erstellt haben, und legen Sie es als dem aktuellen Speicherkonto in Ihrem Abonnement die Zugehörigkeit Gruppe zugeordnet ist.

        New-AzureStorageAccount `
            -StorageAccountName $storageAccountName `
            -Label $storageAccountLabel `
            -AffinityGroup $affinityGroupName
        Set-AzureSubscription `
            -SubscriptionName (Get-AzureSubscription).SubscriptionName `
            -CurrentStorageAccount $storageAccountName

1. Erstellen Sie den DC-Server in der neuen Cloud-Dienst und Verfügbarkeit festlegen.

        New-AzureVMConfig `
            -Name $dcServerName `
            -InstanceSize Medium `
            -ImageName $winImageName `
            -MediaLocation "$storageAccountContainer$dcServerName.vhd" `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -Windows `
                -DisableAutomaticUpdates `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword |
                New-AzureVM `
                    -ServiceName $dcServiceName `
                    –AffinityGroup $affinityGroupName `
                    -VNetName $virtualNetworkName

    Diese Reihe von piped Befehle gehen Sie folgendermaßen vor:

    - **Neu-AzureVMConfig** erstellt eine virtueller Computer-Konfiguration.

    - **Hinzufügen-AzureProvisioningConfig** bietet die Konfigurationsparameter einem eigenständigen WindowsServer.

    - **Hinzufügen-AzureDataDisk** fügt den Daten Datenträger, den Sie zum Speichern von Active Directory-Daten, mit der Option keine festgelegt zum Zwischenspeichern verwendet wird.

    - **Neu-AzureVM** erstellt einen neue Cloud-Dienst sowie den neuen Azure-virtuellen Computer in der neuen Cloud-Dienst.

1. Warten Sie auf dem neuen virtuellen Computer vollständig bereitgestellt werden, und Laden Sie die remote desktop-Datei in Ihrem Arbeitsverzeichnis. Da den neuen Azure-virtuellen Computer bereitstellen lange dauert, die Weile Endloswiedergabe weiterhin den neuen virtuellen Computer Umfrage, bis es zur Verfügung steht.

        $VMStatus = Get-AzureVM -ServiceName $dcServiceName -Name $dcServerName

        While ($VMStatus.InstanceStatus -ne "ReadyRole")
        {
            write-host "Waiting for " $VMStatus.Name "... Current Status = " $VMStatus.InstanceStatus
            Start-Sleep -Seconds 15
            $VMStatus = Get-AzureVM -ServiceName $dcServiceName -Name $dcServerName
        }

        Get-AzureRemoteDesktopFile `
            -ServiceName $dcServiceName `
            -Name $dcServerName `
            -LocalPath "$workingDir$dcServerName.rdp"

Der DC-Server ist jetzt erfolgreich bereitgestellt. Konfigurieren Sie als Nächstes Active Directory-Domäne auf diesem Server DC. Lassen Sie das PowerShell-Fenster auf Ihrem lokalen Computer geöffnet. Sie werden sie später erneut verwenden, um die zwei SQL Server virtuellen Computern erstellen.

## <a name="configure-the-domain-controller"></a>Konfigurieren des Domänencontrollers

1. Verbinden Sie mit dem Server DC durch Starten der remote desktop-Datei ein. Verwenden Sie des Administrators AzureAdmin-Benutzernamen und Ihr Kennwort ein **Contoso! 000**, die Sie beim Erstellen des neuen virtuellen Computer angegeben.

1. Öffnen Sie ein PowerShell-Fenster im Administratormodus aus.

1. Führen Sie Folgendes **DCPROMO. EXE** Befehl zum Einrichten der Domäne **corp.contoso.com** mit den Daten Verzeichnissen auf Laufwerk M.

        dcpromo.exe `
            /unattend `
            /ReplicaOrNewDomain:Domain `
            /NewDomain:Forest `
            /NewDomainDNSName:corp.contoso.com `
            /ForestLevel:4 `
            /DomainNetbiosName:CORP `
            /DomainLevel:4 `
            /InstallDNS:Yes `
            /ConfirmGc:Yes `
            /CreateDNSDelegation:No `
            /DatabasePath:"C:\Windows\NTDS" `
            /LogPath:"C:\Windows\NTDS" `
            /SYSVOLPath:"C:\Windows\SYSVOL" `
            /SafeModeAdminPassword:"Contoso!000"

    Sobald der Befehl abgeschlossen ist, wird der virtuellen Computer automatisch neu gestartet.

1. Herstellen einer Verbindung mit dem Server DC erneut durch Starten der remote desktop-Datei. Melden Sie diesmal, sich als **CORP\Administrator**.

1. Öffnen Sie ein PowerShell-Fenster im Administratormodus und importieren Sie das Active Directory-PowerShell-Modul mit dem folgenden Befehl:

        Import-Module ActiveDirectory

1. Führen Sie die folgenden Befehle drei Benutzer zu der Domäne hinzufügen.

        $pwd = ConvertTo-SecureString "Contoso!000" -AsPlainText -Force
        New-ADUser `
            -Name 'Install' `
            -AccountPassword  $pwd `
            -PasswordNeverExpires $true `
            -ChangePasswordAtLogon $false `
            -Enabled $true
        New-ADUser `
            -Name 'SQLSvc1' `
            -AccountPassword  $pwd `
            -PasswordNeverExpires $true `
            -ChangePasswordAtLogon $false `
            -Enabled $true
        New-ADUser `
            -Name 'SQLSvc2' `
            -AccountPassword  $pwd `
            -PasswordNeverExpires $true `
            -ChangePasswordAtLogon $false `
            -Enabled $true

    **CORP\Install** wird verwendet, um die alle Elemente im Zusammenhang mit Instanzen der SQL Server-Dienst, den Cluster WSFC und der Verfügbarkeit Gruppe konfigurieren. **CORP\SQLSvc1** und **CORP\SQLSvc2** werden als die SQL Server-Dienstkonten für die zwei SQL Server virtuelle Computer verwendet.

1. Führen Sie dann die folgenden Befehle zum **CORP\Install** die Berechtigung zum Erstellen von Computerobjekte in der Domäne erteilen.

        Cd ad:
        $sid = new-object System.Security.Principal.SecurityIdentifier (Get-ADUser "Install").SID
        $guid = new-object Guid bf967a86-0de6-11d0-a285-00aa003049e2
        $ace1 = new-object System.DirectoryServices.ActiveDirectoryAccessRule $sid,"CreateChild","Allow",$guid,"All"
        $corp = Get-ADObject -Identity "DC=corp,DC=contoso,DC=com"
        $acl = Get-Acl $corp
        $acl.AddAccessRule($ace1)
        Set-Acl -Path "DC=corp,DC=contoso,DC=com" -AclObject $acl

    Die oben angegebene GUID ist die GUID für den Typ des Objekts. Das Konto **CORP\Install** benötigt die Berechtigung **Alle Eigenschaften lesen** und **Computer-Objekte erstellen** der aktiven direkte Objekte für den WSFC Cluster erstellen. Die Berechtigungen für **Alle Eigenschaften lesen** ist bereits CORP\Install standardmäßig erteilt, damit Sie nicht benötigen, um explizit zu gewähren. Weitere Informationen zu Berechtigungen, die zum Erstellen des WSFC Clusters erforderlich sind, finden Sie unter [Failover Cluster schrittweise Anleitung: Konfigurieren von Konten in Active Directory](https://technet.microsoft.com/library/cc731002%28v=WS.10%29.aspx).

    Jetzt, da Sie die Konfiguration von Active Directory und der Benutzerobjekte abgeschlossen haben, erstellen Sie zwei SQL Server-virtuellen Computern und nehmen sie in dieser Domäne.

## <a name="create-the-sql-server-vms"></a>Erstellen der SQL Server-virtuellen Computern

1. Fahren Sie mit das PowerShell-Fenster verwenden, das auf Ihrem lokalen Computer geöffnet ist. Definieren Sie die folgenden zusätzlichen Variablen:

        $domainName= "corp"
        $FQDN = "corp.contoso.com"
        $subnetName = "Back"
        $sqlServiceName = "<uniqueservicename>"
        $quorumServerName = "ContosoQuorum"
        $sql1ServerName = "ContosoSQL1"
        $sql2ServerName = "ContosoSQL2"
        $availabilitySetName = "SQLHADR"
        $dataDiskSize = 100
        $dnsSettings = New-AzureDns -Name "ContosoBackDNS" -IPAddress "10.10.0.4"

    Die IP-Adresse **10.10.0.4** wird in der Regel den ersten virtuellen Computer zugewiesen, die Sie im Subnetz **10.10.0.0/16** Azure virtuellen Netzwerks erstellen. Überprüfen Sie, dass dies die Adresse des Servers DC ist, führen Sie **IPCONFIG**.

1. Führen Sie die folgenden piped Befehle auf den ersten virtuellen Computer im Cluster WSFC, mit dem Namen **ContosoQuorum**erstellen:

        New-AzureVMConfig `
            -Name $quorumServerName `
            -InstanceSize Medium `
            -ImageName $winImageName `
            -MediaLocation "$storageAccountContainer$quorumServerName.vhd" `
            -AvailabilitySetName $availabilitySetName `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -WindowsDomain `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword `
                -DisableAutomaticUpdates `
                -Domain $domainName `
                -JoinDomain $FQDN `
                -DomainUserName $vmAdminUser `
                -DomainPassword $vmAdminPassword |
                Set-AzureSubnet `
                    -SubnetNames $subnetName |
                    New-AzureVM `
                        -ServiceName $sqlServiceName `
                        –AffinityGroup $affinityGroupName `
                        -VNetName $virtualNetworkName `
                        -DnsSettings $dnsSettings

    Beachten Sie Folgendes zu den oben angegebenen Befehl ein:

    - **Neu-AzureVMConfig** erstellt eine virtueller Computer-Konfiguration mit dem Namen der gewünschten Verfügbarkeit festlegen. Die nachfolgenden virtuellen Computern werden mit demselben Namen Verfügbarkeit festlegen, damit sie mit demselben Satz Verfügbarkeit verknüpft werden erstellt.

    - **Hinzufügen-AzureProvisioningConfig** verknüpft den virtuellen Computer mit Active Directory-Domäne, die Sie erstellt haben.

    - **Set-AzureSubnet** platziert den virtuellen Computer im Subnetz zurück.

    - **Neu-AzureVM** erstellt einen neue Cloud-Dienst sowie den neuen Azure-virtuellen Computer in der neuen Cloud-Dienst. Der Parameter **DnsSettings** gibt an, dass der DNS-Server für die Server in der neuen Cloud-Dienst die IP-Adresse **10.10.0.4**, also die IP-Adresse des Servers DC. Für diesen Parameter ist erforderlich, aktivieren Sie die neuen virtuellen Computern in der Cloud-Dienst, erfolgreich mit Active Directory-Domäne zu verknüpfen. Ohne diesen Parameter müssen Sie manuell festlegen die IPv4-Einstellungen auf Ihre virtuellen Computer verwenden Sie DC Server als primärer DNS-Server nach der virtuellen Computer bereitgestellt wird und Verknüpfung klicken Sie dann den virtuellen Computer Active Directory-Domäne.

1. Führen Sie die folgenden piped Befehle zum Erstellen von SQL Server virtueller Computer, mit dem Namen **ContosoSQL1** und **ContosoSQL2**.

        # Create ContosoSQL1...
        New-AzureVMConfig `
            -Name $sql1ServerName `
            -InstanceSize Large `
            -ImageName $sqlImageName `
            -MediaLocation "$storageAccountContainer$sql1ServerName.vhd" `
            -AvailabilitySetName $availabilitySetName `
            -HostCaching "ReadOnly" `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -WindowsDomain `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword `
                -DisableAutomaticUpdates `
                -Domain $domainName `
                -JoinDomain $FQDN `
                -DomainUserName $vmAdminUser `
                -DomainPassword $vmAdminPassword |
                Set-AzureSubnet `
                    -SubnetNames $subnetName |
                    Add-AzureEndpoint `
                        -Name "SQL" `
                        -Protocol "tcp" `
                        -PublicPort 1 `
                        -LocalPort 1433 |
                        New-AzureVM `
                            -ServiceName $sqlServiceName

        # Create ContosoSQL2...
        New-AzureVMConfig `
            -Name $sql2ServerName `
            -InstanceSize Large `
            -ImageName $sqlImageName `
            -MediaLocation "$storageAccountContainer$sql2ServerName.vhd" `
            -AvailabilitySetName $availabilitySetName `
            -HostCaching "ReadOnly" `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -WindowsDomain `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword `
                -DisableAutomaticUpdates `
                -Domain $domainName `
                -JoinDomain $FQDN `
                -DomainUserName $vmAdminUser `
                -DomainPassword $vmAdminPassword |
                Set-AzureSubnet `
                    -SubnetNames $subnetName |
                    Add-AzureEndpoint `
                        -Name "SQL" `
                        -Protocol "tcp" `
                        -PublicPort 2 `
                        -LocalPort 1433 |
                        New-AzureVM `
                            -ServiceName $sqlServiceName

    Bezug auf die oben genannten Befehle Folgendes zu beachten:

    - **Neu-AzureVMConfig** verwendet die gleiche Verfügbarkeit SetName wie der DC-Server, und das Bild SQL Server 2012 Service Pack 1 Enterprise Edition im Katalog virtuellen Computern. Setzt auch den Datenträger Betriebssystem auf gelesen-nur Zwischenspeichern (keine schreiben Zwischenspeichern). Es wird empfohlen, dass Sie die Datenbankdateien auf einen Datenträger Daten getrennt migrieren, und konfigurieren Sie ihn mit keine Lese- oder Zwischenspeichern Schreiben an den virtuellen Computer anfügen. Ist jedoch weiter am besten zum Zwischenspeichern von auf dem Datenträger Betriebssystem entfernen, da Sie nicht entfernen können Lese-Cache auf dem Datenträger Betriebssystem an.

    - **Hinzufügen-AzureProvisioningConfig** verknüpft den virtuellen Computer mit Active Directory-Domäne, die Sie erstellt haben.

    - **Set-AzureSubnet** platziert den virtuellen Computer im Subnetz zurück.

    - **Hinzufügen-AzureEndpoint** fügt Access Endpunkte, sodass Clientanwendungen diese Services-Instanzen von SQL Server im Internet zugreifen können. Verschiedene Ports werden ContosoSQL1 und ContosoSQL2 erteilt.

    - **Neu-AzureVM** erstellt die neue SQL Server virtueller Computer in der gleichen Cloud-Dienst als ContosoQuorum an. Sie müssen die virtuellen Computern in der gleichen Cloud-Dienst platzieren, wenn Sie in demselben Satz Verfügbarkeit werden sollen.

1. Warten Sie für jeden virtuellen Computer vollständig bereitgestellt werden und deren remote desktop-Datei in Ihrer Arbeitsverzeichnis herunterladen. Die für Schleife die drei neuen virtuellen Computern durchläuft und führt Sie für jede von ihnen die Befehle in der obersten Ebene geschweiften Klammern aus.

        Foreach ($VM in $VMs = Get-AzureVM -ServiceName $sqlServiceName)
        {
            write-host "Waiting for " $VM.Name "..."

            # Loop until the VM status is "ReadyRole"
            While ($VM.InstanceStatus -ne "ReadyRole")
            {
                write-host "  Current Status = " $VM.InstanceStatus
                Start-Sleep -Seconds 15
                $VM = Get-AzureVM -ServiceName $VM.ServiceName -Name $VM.InstanceName
            }

            write-host "  Current Status = " $VM.InstanceStatus

            # Download remote desktop file
            Get-AzureRemoteDesktopFile -ServiceName $VM.ServiceName -Name $VM.InstanceName -LocalPath "$workingDir$($VM.InstanceName).rdp"
        }

    Die SQL Server-virtuellen Computern sind jetzt nach der Bereitstellung und Ausführung, aber sie werden mit Standardoptionen mit SQL Server installiert.

## <a name="initialize-the-wsfc-cluster-vms"></a>Initialisierung der WSFC Cluster virtuellen Computern

In diesem Abschnitt müssen Sie die drei Server zu ändern, in dem WSFC Cluster und SQL Server-Installation erfolgen soll. Insbesondere:

- (Alle Server) Sie müssen **das Failoverclusterfeature** installieren.

- (Alle Server) Sie müssen **CORP\Install** als der **Administrator**des Computers hinzufügen.

- (ContosoSQL1 und ContosoSQL2 nur) Sie müssen **CORP\Install** als **Sysadmin** Rolle in der Standarddatenbank hinzufügen.

- (ContosoSQL1 und ContosoSQL2 nur) Sie müssen **Systemkonto:** als Benutzernamen mit den folgenden Berechtigungen hinzufügen:

    - Alle Verfügbarkeit Gruppe ändern

    - Verbinden von SQL

    - Status des Servers anzeigen

- (ContosoSQL1 und ContosoSQL2 nur) Das **TCP-** Protokoll ist bereits auf dem SQL Server virtuellen Computer aktiviert. Sie müssen jedoch weiterhin die Firewall für den Remotezugriff von SQL Server zu öffnen.

Jetzt können Sie beginnen. Beginnend mit **ContosoQuorum**, führen Sie die folgenden Schritte aus:

1. Eine Verbindung zu **ContosoQuorum** ebenso remote desktop-Dateien. Verwenden Sie des Administrators **AzureAdmin** -Benutzernamen und Ihr Kennwort ein **Contoso! 000**, die Sie beim Erstellen der virtuellen Computern angegeben.

1. Stellen Sie sicher, dass der Computer erfolgreich **corp.contoso.com**hinzugefügt haben.

1. Warten Sie auf SQL Server-Installation zu Ende ausgeführt wurden die automatisierte Initialisierungsaufgaben, bevor Sie fortfahren.

1. Öffnen Sie ein PowerShell-Fenster im Administratormodus aus.

1. Installieren Sie das Windows-Failoverclusterfeature.

        Import-Module ServerManager
        Add-WindowsFeature Failover-Clustering

1. Fügen Sie **CORP\Install** als lokaler Administrator hinzu.

        net localgroup administrators "CORP\Install" /Add

1. Melden Sie sich bei ContosoQuorum ab. Sie sind jetzt mit diesem Server fertig.

        logoff.exe

Als Nächstes Initialisierung **ContosoSQL1** und **ContosoSQL2**. Führen Sie die folgenden Schritte aus, die für beide der SQL Server-virtuellen Computern identisch sind.

1. Verbinden Sie mit den zwei SQL Server-virtuellen Computern durch Starten der remote desktop-Dateien. Verwenden Sie des Administrators **AzureAdmin** -Benutzernamen und Ihr Kennwort ein **Contoso! 000**, die Sie beim Erstellen der virtuellen Computern angegeben.

1. Stellen Sie sicher, dass der Computer erfolgreich **corp.contoso.com**hinzugefügt haben.

1. Warten Sie auf SQL Server-Installation zu Ende ausgeführt wurden die automatisierte Initialisierungsaufgaben, bevor Sie fortfahren.

1. Öffnen Sie ein PowerShell-Fenster im Administratormodus aus.

1. Installieren Sie das Windows-Failoverclusterfeature an.

        Import-Module ServerManager
        Add-WindowsFeature Failover-Clustering

1. Fügen Sie **CORP\Install** als lokalen Administrator hinzu.

        net localgroup administrators "CORP\Install" /Add

1. Importieren der PowerShell-Anbieter für SQL Server.

        Set-ExecutionPolicy -Execution RemoteSigned -Force
        Import-Module -Name "sqlps" -DisableNameChecking

1. Fügen Sie **CORP\Install** wie die Rolle Sysadmin für die standardmäßige SQL Server-Instanz aus.

        net localgroup administrators "CORP\Install" /Add
        Invoke-SqlCmd -Query "EXEC sp_addsrvrolemember 'CORP\Install', 'sysadmin'" -ServerInstance "."

1. Hinzufügen von **Systemkonto:** als eine Anmeldung mit den drei Berechtigungen oben beschriebenen.

        Invoke-SqlCmd -Query "CREATE LOGIN [NT AUTHORITY\SYSTEM] FROM WINDOWS" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT ALTER ANY AVAILABILITY GROUP TO [NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT CONNECT SQL TO [NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT VIEW SERVER STATE TO [NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."

1. Öffnen Sie die Firewall für den Remotezugriff von SQL Server.

        netsh advfirewall firewall add rule name='SQL Server (TCP-In)' program='C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\Binn\sqlservr.exe' dir=in action=allow protocol=TCP

1. Melden Sie sich von beiden virtuellen Computern.

        logoff.exe

Schließlich sind Sie bereit zum Konfigurieren der Verfügbarkeit Gruppe. Sie verwenden den PowerShell-Anbieter für SQL Server für die Arbeit an **ContosoSQL1**zur Verfügung.

## <a name="configure-the-availability-group"></a>Konfigurieren der Verfügbarkeit Gruppe

1. Herstellen einer Verbindung mit **ContosoSQL1** erneut durch Starten der remote desktop-Dateien. Statt mit dem Computerkonto anmelden, melden Sie sich mit **CORP\Install**.

1. Öffnen Sie ein PowerShell-Fenster im Administratormodus aus.

1. Definieren Sie die folgenden Variablen:

        $server1 = "ContosoSQL1"
        $server2 = "ContosoSQL2"
        $serverQuorum = "ContosoQuorum"
        $acct1 = "CORP\SQLSvc1"
        $acct2 = "CORP\SQLSvc2"
        $password = "Contoso!000"
        $clusterName = "Cluster1"
        $timeout = New-Object System.TimeSpan -ArgumentList 0, 0, 30
        $db = "MyDB1"
        $backupShare = "\\$server1\backup"
        $quorumShare = "\\$server1\quorum"
        $ag = "AG1"

1. Importieren von PowerShell-Anbieter für SQL Server.

        Set-ExecutionPolicy RemoteSigned -Force
        Import-Module "sqlps" -DisableNameChecking

1. Ändern Sie die SQL Server-Dienstkontos für ContosoSQL1 in CORP\SQLSvc1.

        $wmi1 = new-object ("Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer") $server1
        $wmi1.services | where {$_.Type -eq 'SqlServer'} | foreach{$_.SetServiceAccount($acct1,$password)}
        $svc1 = Get-Service -ComputerName $server1 -Name 'MSSQLSERVER'
        $svc1.Stop()
        $svc1.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc1.Start();
        $svc1.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)

1. Ändern Sie die SQL Server-Dienstkontos für ContosoSQL2 in CORP\SQLSvc2.

        $wmi2 = new-object ("Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer") $server2
        $wmi2.services | where {$_.Type -eq 'SqlServer'} | foreach{$_.SetServiceAccount($acct2,$password)}
        $svc2 = Get-Service -ComputerName $server2 -Name 'MSSQLSERVER'
        $svc2.Stop()
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc2.Start();
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)

1. Herunterladen von **CreateAzureFailoverCluster.ps1** aus [Erstellen WSFC Cluster für immer auf Verfügbarkeit Gruppen Azure-virtuellen Computer](http://gallery.technet.microsoft.com/scriptcenter/Create-WSFC-Cluster-for-7c207d3a) in der lokalen Arbeitsverzeichnis. Sie werden dieses Skript verwenden, denen Sie einen funktionsübergreifendes WSFC Cluster erstellen können. Wichtige Informationen darüber, wie WSFC mit dem Netzwerk Azure interagiert finden Sie unter [hohe Verfügbarkeit und Wiederherstellung für SQL Server in Azure virtuellen Computern](virtual-machines-windows-sql-high-availability-dr.md).

1. Wechseln Sie in Ihrem Arbeitsverzeichnis und erstellen Sie WSFC Cluster mit dem heruntergeladenen Skript.

        Set-ExecutionPolicy Unrestricted -Force
        .\CreateAzureFailoverCluster.ps1 -ClusterName "$clusterName" -ClusterNode "$server1","$server2","$serverQuorum"

1. Aktivieren Sie immer auf Verfügbarkeit von Gruppen für die standardmäßige SQL Server-Instanzen **ContosoSQL1** und **ContosoSQL2**ein.

        Enable-SqlAlwaysOn `
            -Path SQLSERVER:\SQL\$server1\Default `
            -Force
        Enable-SqlAlwaysOn `
            -Path SQLSERVER:\SQL\$server2\Default `
            -NoServiceRestart
        $svc2.Stop()
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc2.Start();
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)

1. Erstellen Sie ein Verzeichnis Sicherung und Erteilen von Berechtigungen für die SQL Server-Dienstkonten. Sie werden dieses Verzeichnis verwenden, um die Verfügbarkeit Datenbank im sekundären Replikat vorbereiten.

        $backup = "C:\backup"
        New-Item $backup -ItemType directory
        net share backup=$backup "/grant:$acct1,FULL" "/grant:$acct2,FULL"
        icacls.exe "$backup" /grant:r ("$acct1" + ":(OI)(CI)F") ("$acct2" + ":(OI)(CI)F")

1. Erstellen einer Datenbank auf **ContosoSQL1** **MyDB1**aufgerufen, sowohl eine vollständige Sicherung und eine Sicherung ausführen und wiederherstellen, mit der Option **Mit NORECOVERY** auf **ContosoSQL2** .

        Invoke-SqlCmd -Query "CREATE database $db"
        Backup-SqlDatabase -Database $db -BackupFile "$backupShare\db.bak" -ServerInstance $server1
        Backup-SqlDatabase -Database $db -BackupFile "$backupShare\db.log" -ServerInstance $server1 -BackupAction Log
        Restore-SqlDatabase -Database $db -BackupFile "$backupShare\db.bak" -ServerInstance $server2 -NoRecovery
        Restore-SqlDatabase -Database $db -BackupFile "$backupShare\db.log" -ServerInstance $server2 -RestoreAction Log -NoRecovery

1. Erstellen Sie die Verfügbarkeit Gruppe Endpunkte, auf die SQL Server-virtuellen Computern und legen Sie die entsprechenden Berechtigungen auf die Endpunkte.

        $endpoint =
            New-SqlHadrEndpoint MyMirroringEndpoint `
            -Port 5022 `
            -Path "SQLSERVER:\SQL\$server1\Default"
        Set-SqlHadrEndpoint `
            -InputObject $endpoint `
            -State "Started"
        $endpoint =
            New-SqlHadrEndpoint MyMirroringEndpoint `
            -Port 5022 `
            -Path "SQLSERVER:\SQL\$server2\Default"
        Set-SqlHadrEndpoint `
            -InputObject $endpoint `
            -State "Started"

        Invoke-SqlCmd -Query "CREATE LOGIN [$acct2] FROM WINDOWS" -ServerInstance $server1
        Invoke-SqlCmd -Query "GRANT CONNECT ON ENDPOINT::[MyMirroringEndpoint] TO [$acct2]" -ServerInstance $server1
        Invoke-SqlCmd -Query "CREATE LOGIN [$acct1] FROM WINDOWS" -ServerInstance $server2
        Invoke-SqlCmd -Query "GRANT CONNECT ON ENDPOINT::[MyMirroringEndpoint] TO [$acct1]" -ServerInstance $server2

1. Die Verfügbarkeit Replikate zu erstellen.

        $primaryReplica =
            New-SqlAvailabilityReplica `
            -Name $server1 `
            -EndpointURL "TCP://$server1.corp.contoso.com:5022" `
            -AvailabilityMode "SynchronousCommit" `
            -FailoverMode "Automatic" `
            -Version 11 `
            -AsTemplate
        $secondaryReplica =
            New-SqlAvailabilityReplica `
            -Name $server2 `
            -EndpointURL "TCP://$server2.corp.contoso.com:5022" `
            -AvailabilityMode "SynchronousCommit" `
            -FailoverMode "Automatic" `
            -Version 11 `
            -AsTemplate

1. Schließlich erstellen Sie die Verfügbarkeit Gruppe und teilnehmen an der sekundäre Replikat in der Gruppe Verfügbarkeit.

        New-SqlAvailabilityGroup `
            -Name $ag `
            -Path "SQLSERVER:\SQL\$server1\Default" `
            -AvailabilityReplica @($primaryReplica,$secondaryReplica) `
            -Database $db
        Join-SqlAvailabilityGroup `
            -Path "SQLSERVER:\SQL\$server2\Default" `
            -Name $ag
        Add-SqlAvailabilityDatabase `
            -Path "SQLSERVER:\SQL\$server2\Default\AvailabilityGroups\$ag" `
            -Database $db

## <a name="next-steps"></a>Nächste Schritte
Sie haben nun erfolgreich SQL Server immer auf implementiert durch Erstellen einer Gruppe Verfügbarkeit in Azure. Zum Konfigurieren einer Zuhörer für diese Gruppe Verfügbarkeit finden Sie unter [Konfigurieren einer ILB Zuhörer für immer auf Verfügbarkeit von Gruppen in Azure](virtual-machines-windows-classic-ps-sql-int-listener.md).

Weitere Informationen zur Verwendung von SQL Server in Azure finden Sie unter [SQL Server auf Azure virtuellen Computern](virtual-machines-windows-sql-server-iaas-overview.md).

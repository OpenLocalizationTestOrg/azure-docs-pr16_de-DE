<properties
    pageTitle="Konfigurieren eines externen Zuhörer für immer auf Verfügbarkeit Gruppen | Microsoft Azure"
    description="Dieses Lernprogramm führt Sie durch die Schritte zum Erstellen einer immer auf Verfügbarkeit Gruppe Zuhörer in Azure, die extern zugänglich sind, mit der öffentlichen virtuelle IP-Adresse des zugeordneten Cloud-Dienst."
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
    ms.date="07/12/2016"
    ms.author="MikeRayMSFT" />

# <a name="configure-an-external-listener-for-always-on-availability-groups-in-azure"></a>Konfigurieren eines externen Zuhörer für immer auf Verfügbarkeit von Gruppen in Azure

> [AZURE.SELECTOR]
- [Interner Zuhörer](virtual-machines-windows-classic-ps-sql-int-listener.md)
- [Externe Zuhörer](virtual-machines-windows-classic-ps-sql-ext-listener.md)

In diesem Thema wird gezeigt, wie eine Zuhörer bei immer auf Verfügbarkeit Gruppe konfigurieren, die extern im Internet zugänglich ist. Dies wird ermöglicht durch die Zuhörer Cloud-Dienst **Öffentliche virtuelle IP-Adresse (VIP)** Adresse zuordnen.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Die Verfügbarkeit von Gruppe können Replikate, die lokal nur sind nur Azure enthalten oder umfassen sowohl lokalen und Azure möchten. Azure Replikate können innerhalb der gleichen Region oder über mehrere Bereiche, die über mehrere virtuelle Netzwerke (VNets) befinden. Die nachstehenden Schritten wird vorausgesetzt, Sie verfügen bereits über [eine Gruppe Verfügbarkeit konfiguriert](virtual-machines-windows-classic-portal-sql-alwayson-availability-groups.md) aber eine Zuhörer nicht konfiguriert haben.

## <a name="guidelines-and-limitations-for-external-listeners"></a>Richtlinien und Einschränkungen für externe Listener

Beachten Sie die folgenden Richtlinien über die Verfügbarkeit Gruppe Zuhörer in Azure, wenn Sie die Bereitstellung mit der Cloud-Dienst geil VIP-Adresse:

- Die Verfügbarkeit Gruppe Zuhörer wird unter Windows Server 2008 R2, Windows Server 2012 und Windows Server 2012 R2 unterstützt.

- Die Clientanwendung muss auf einem anderen Cloud-Dienst als den befinden, die die Verfügbarkeit Gruppe virtuellen Computern enthält. Azure unterstützt keine direkte Server Eingabe für Client und Server in der gleichen Cloud-Dienst.

- Standardmäßig an die Schritten in diesem Artikel zum Konfigurieren einer Zuhörer um die Cloud-Dienst virtuelle IP-Adresse (VIP) Adresse verwenden. Es ist jedoch möglich, reservieren und mehrere VIP-Adressen für Ihre Cloud-Dienst erstellen. So können Sie die Schritte in diesem Artikel verwenden, um mehrere Listener zu erstellen, die jeweils eine andere VIP zugeordnet sind. Informationen zum Erstellen von mehreren VIP Adressen finden Sie unter [Mehrere VIPs pro Cloud-Dienst](../load-balancer/load-balancer-multivip.md).

- Wenn Sie eine Zuhörer für eine hybridumgebung erstellen, müssen das lokale Netzwerk Konnektivität mit dem öffentlichen Internet zusätzlich zu der Website-zu-Standort VPN mit dem Azure virtuelle Netzwerk. Wenn Sie in der Azure Subnetz ist die Verfügbarkeit Gruppe Zuhörer erreicht werden nur die öffentlichen IP-Adresse des jeweiligen Cloud-Dienst kann.

- Es wird nicht unterstützt, um eine externe Zuhörer in derselben Cloud-Dienst zu erstellen, in dem Sie auch eine interne Zuhörer mit internen laden Lastenausgleich (ILB) haben.

## <a name="determine-the-accessibility-of-the-listener"></a>Bestimmen Sie den Zugriff auf die Zuhörer

[AZURE.INCLUDE [ag-listener-accessibility](../../includes/virtual-machines-ag-listener-determine-accessibility.md)]

Dieser Artikel befasst sich erstellen einen befinden, der **externe Lastenausgleich**verwendet. Wenn Sie eine Zuhörer möchten, die mit Ihrem Netzwerk virtuelle privat ist, finden Sie unter die Version der in diesem Artikel werden die Schritte zum Einrichten einer [Zuhörer mit ILB](virtual-machines-windows-classic-ps-sql-int-listener.md) enthält

## <a name="create-load-balanced-vm-endpoints-with-direct-server-return"></a>Erstellen von virtuellen Computer Endpunkte Lastenausgleich mit direkter Server zurückgegeben

Externe Lastenausgleich mithilfe der virtuellen die öffentliche virtuelle IP-Adresse des Cloud-Dienst, der Ihre virtuellen Computern hostet. Sie müssen daher nicht erstellen oder den Lastenausgleich in diesem Fall konfigurieren zu können.

[AZURE.INCLUDE [load-balanced-endpoints](../../includes/virtual-machines-ag-listener-load-balanced-endpoints.md)]

1. Kopieren Sie das folgende PowerShell-Skript in einen Text-Editor, und legen Sie die Variablenwerten an Ihre Umgebung an (Standardeinstellungen für einige Parameter gewährt wurde). Beachten Sie, dass wenn die Verfügbarkeit Gruppe Azure Regionen umfasst, müssen Sie ausführen das Skript einmal in jedem Datencenter für die Cloud-Dienst und Knoten, die in dieser Datacenter befinden.

        # Define variables
        $ServiceName = "<MyCloudService>" # the name of the cloud service that contains the availability group nodes
        $AGNodes = "<VM1>","<VM2>","<VM3>" # all availability group nodes containing replicas in the same cloud service, separated by commas

        # Configure a load balanced endpoint for each node in $AGNodes, with direct server return enabled
        ForEach ($node in $AGNodes)
        {
            Get-AzureVM -ServiceName $ServiceName -Name $node | Add-AzureEndpoint -Name "ListenerEndpoint" -Protocol "TCP" -PublicPort 1433 -LocalPort 1433 -LBSetName "ListenerEndpointLB" -ProbePort 59999 -ProbeProtocol "TCP" -DirectServerReturn $true | Update-AzureVM
        }

1. Nachdem Sie die Variablen eingerichtet haben, kopieren Sie das Skript aus der Text-Editor in Ihre Azure PowerShell-Sitzung, um Sie auszuführen. Wenn die Aufforderung weiterhin zeigt >>, geben Sie die EINGABETASTE erneut, um sicherzustellen, dass das Skript gestartet.

## <a name="verify-that-kb2854082-is-installed-if-necessary"></a>Stellen Sie sicher, dass KB2854082 bei Bedarf installiert ist

[AZURE.INCLUDE [kb2854082](../../includes/virtual-machines-ag-listener-kb2854082.md)]

## <a name="open-the-firewall-ports-in-availability-group-nodes"></a>Öffnen Sie die Firewallports in Verfügbarkeit Gruppieren von Knoten

[AZURE.INCLUDE [firewall](../../includes/virtual-machines-ag-listener-open-firewall.md)]

## <a name="create-the-availability-group-listener"></a>Erstellen der Verfügbarkeit Gruppe Zuhörer

[AZURE.INCLUDE [firewall](../../includes/virtual-machines-ag-listener-create-listener.md)]

1. Für einen externen Lastenausgleich benötigen Sie die öffentliche virtuelle IP-Adresse des Cloud-Dienst aus, die Ihre Replikate enthält. Melden Sie sich in der klassischen Azure-Portal. Navigieren Sie zu der Cloud-Dienst, der die Verfügbarkeit Gruppe virtueller Computer enthält. Öffnen Sie die **Dashboard** -Ansicht.

3. Beachten Sie die Adresse unter **öffentliche virtuelle IP-Adresse (VIP) Adresse**angezeigt. Wenn Ihre Lösung VNets umfasst, wiederholen Sie diesen Schritt für jeden Clouddienst, die einen virtuellen enthält, die ein Replikat hostet.

4. Klicken Sie auf eine von der virtuellen Maschinen kopieren Sie das folgende PowerShell-Skript in einen Text-Editor, und legen Sie die Variablen auf die Werte, die Sie zuvor notiert haben.

        # Define variables
        $ClusterNetworkName = "<ClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP Address resource name
        $CloudServiceIP = "<X.X.X.X>" # Public Virtual IP (VIP) address of your cloud service

        Import-Module FailoverClusters

        # If you are using Windows Server 2012 or higher, use the Get-Cluster Resource command. If you are using Windows Server 2008 R2, use the cluster res command. Both commands are commented out. Choose the one applicable to your environment and remove the # at the beginning of the line to convert the comment to an executable line of code.

        # Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$CloudServiceIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"OverrideAddressMatch"=1;"EnableDhcp"=0}
        # cluster res $IPResourceName /priv enabledhcp=0 overrideaddressmatch=1 address=$CloudServiceIP probeport=59999  subnetmask=255.255.255.255


1. Einmal Sie festgelegt haben die Variablen, einer erhöhten Windows PowerShell-Fenster zu öffnen und kopieren Sie das Skript aus dem Texteditor und fügen Ihre Azure PowerShell-Sitzung, um Sie auszuführen. Wenn die Aufforderung weiterhin zeigt >>, geben Sie die EINGABETASTE erneut, um sicherzustellen, dass das Skript gestartet.

1. Wiederholen Sie diesen Schritt jedes virtuellen Computers. Dieses Skript die Ressource IP-Adresse die IP-Adresse des Cloud-Dienst konfiguriert und legt anderen Parameter wie den Port Prüfpunkt. Wenn die IP-Adresse Ressource online ist, kann es aus den Lastenausgleich Endpunkt zuvor in diesem Lernprogramm erstellt haben klicken Sie dann auf eine der Abfrage auf den Port Prüfpunkt reagieren.

## <a name="bring-the-listener-online"></a>Zeigen Sie die Zuhörer online

[AZURE.INCLUDE [Bring-Listener-Online](../../includes/virtual-machines-ag-listener-bring-online.md)]

## <a name="follow-up-items"></a>Nachverfolgen von Elementen

[AZURE.INCLUDE [Follow-up](../../includes/virtual-machines-ag-listener-follow-up.md)]

## <a name="test-the-availability-group-listener-within-the-same-vnet"></a>Testen der Verfügbarkeit Gruppe Zuhörer (innerhalb der gleichen VNet)

[AZURE.INCLUDE [Test-Listener-Within-VNET](../../includes/virtual-machines-ag-listener-test.md)]

## <a name="test-the-availability-group-listener-over-the-internet"></a>Testen der Verfügbarkeit Gruppe Zuhörer (über das Internet)

Um die Zuhörer von außerhalb des virtuellen Netzwerks zugreifen zu können, müssen Sie verwenden externe/öffentlichen Lastenausgleich (in diesem Artikel beschrieben) anstatt ILB, also nur zugegriffen innerhalb der gleichen VNet. In der Verbindungszeichenfolge Geben Sie den Namen der Cloud-Dienst an. Wenn Sie einen Cloud-Dienst mit dem Namen *Mycloudservice*hatten, würde Sqlcmd-Anweisung beispielsweise folgendermaßen aussehen:

    sqlcmd -S "mycloudservice.cloudapp.net,<EndpointPort>" -d "<DatabaseName>" -U "<LoginId>" -P "<Password>"  -Q "select @@servername, db_name()" -l 15

Im Gegensatz zu den vorherigen Beispiel muss SQL-Authentifizierung verwendet werden, da der Anrufer Windows-Authentifizierung über das Internet verwenden kann. Weitere Informationen finden Sie unter [immer auf Verfügbarkeit Gruppe Azure-virtuellen Computer: Client Connectivity Szenarien](http://blogs.msdn.com/b/sqlcat/archive/2014/02/03/alwayson-availability-group-in-windows-azure-vm-client-connectivity-scenarios.aspx). Wenn SQL-Authentifizierung verwenden möchten, stellen Sie sicher, dass Sie das gleiche Login auf beide Replikate erstellen. Weitere Informationen zur Behandlung von Problemen mit der Verfügbarkeit von Gruppen Benutzernamen finden Sie unter [So ordnen Sie die Benutzernamen oder verwenden Sie enthaltenen SQL-Datenbankbenutzers zum Verbinden mit anderen Replikaten und zuordnen zur Verfügbarkeit von Datenbanken](http://blogs.msdn.com/b/alwaysonpro/archive/2014/02/19/how-to-map-logins-or-use-contained-sql-database-user-to-connect-to-other-replicas-and-map-to-availability-databases.aspx).

Wenn die Replikate immer auf in unterschiedlichen Subnetzen sind, müssen Clients angeben **MultisubnetFailover = WAHR** in der Verbindungszeichenfolge. Dies führt parallele Verbindungsversuche auf Replikate in verschiedenen Subnetzen. Beachten Sie, dass dieses Szenario eine Kreuz-Region immer auf Verfügbarkeit Gruppe Bereitstellung enthält.

## <a name="next-steps"></a>Nächste Schritte

[AZURE.INCLUDE [Listener-Next-Steps](../../includes/virtual-machines-ag-listener-next-steps.md)]

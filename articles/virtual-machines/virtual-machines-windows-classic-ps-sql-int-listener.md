<properties
    pageTitle="Konfigurieren eines ILB Zuhörer für immer auf Verfügbarkeit Gruppen | Microsoft Azure"
    description="In diesem Lernprogramm erstellte, mit dem Bereitstellungsmodell klassischen Ressourcen verwendet und erstellt eine immer auf Verfügbarkeit Gruppe Zuhörer in Azure mithilfe eines internen laden Lastenausgleich (ILB)."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="08/19/2016"
    ms.author="MikeRayMSFT" />

# <a name="configure-an-ilb-listener-for-always-on-availability-groups-in-azure"></a>Konfigurieren eines ILB Zuhörer für immer auf Verfügbarkeit von Gruppen in Azure

> [AZURE.SELECTOR]
- [Interner Zuhörer](virtual-machines-windows-classic-ps-sql-int-listener.md)
- [Externe Zuhörer](virtual-machines-windows-classic-ps-sql-ext-listener.md)

## <a name="overview"></a>(Übersicht)

In diesem Thema wird gezeigt, wie eine Zuhörer bei immer auf Verfügbarkeit Gruppe mithilfe einer **Internen laden Lastenausgleich (ILB)**konfigurieren.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Zum Konfigurieren einer ILB Zuhörer für eine Gruppe immer auf Verfügbarkeit Ressourcenmanager Modell finden Sie unter [Konfigurieren eines internen Lastenausgleich für eine Gruppe immer auf Verfügbarkeit in Azure](virtual-machines-windows-portal-sql-alwayson-int-listener.md).


Die Verfügbarkeit von Gruppe können Replikate, die lokal nur sind nur Azure enthalten oder umfassen sowohl lokalen und Azure möchten. Azure Replikate können innerhalb der gleichen Region oder über mehrere Bereiche, die über mehrere virtuelle Netzwerke (VNets) befinden. Die nachstehenden Schritten wird vorausgesetzt, Sie verfügen bereits über [eine Gruppe Verfügbarkeit konfiguriert](virtual-machines-windows-classic-portal-sql-alwayson-availability-groups.md) aber eine Zuhörer nicht konfiguriert haben.

## <a name="guidelines-and-limitations-for-internal-listeners"></a>Richtlinien und Einschränkungen für interne Listener
Beachten Sie die folgenden Richtlinien auf die Verfügbarkeit Gruppe Zuhörer in Azure ILB verwenden:

- Die Verfügbarkeit Gruppe Zuhörer wird unter Windows Server 2008 R2, Windows Server 2012 und Windows Server 2012 R2 unterstützt.

- Pro Cloud-Dienst, wird nur eine interne Verfügbarkeit Gruppe Zuhörer unterstützt, da die Zuhörer als die ILB konfiguriert ist, und es nur eine ILB pro Cloud-Dienst ist. Es ist jedoch möglich, mehrere externe Listener zu erstellen. Weitere Informationen finden Sie unter [Konfigurieren einer externen Zuhörer für immer auf Verfügbarkeit von Gruppen in Azure](virtual-machines-windows-classic-ps-sql-ext-listener.md).

- Es wird nicht unterstützt, um eine interne Zuhörer in derselben Cloud-Dienst zu erstellen, in dem Sie auch eine externe Zuhörer mit der Cloud-Dienst öffentliche VIP haben.

## <a name="determine-the-accessibility-of-the-listener"></a>Bestimmen Sie den Zugriff auf die Zuhörer

[AZURE.INCLUDE [ag-listener-accessibility](../../includes/virtual-machines-ag-listener-determine-accessibility.md)]

Erstellen einen befinden, der eine **Interne laden Lastenausgleich (ILB)**behandelt in diesem Artikel. Wenn Sie eine öffentliche/externe Zuhörer benötigen, finden Sie unter die Version der in diesem Artikel werden die Schritte zum Einrichten einer [externen Zuhörer](virtual-machines-windows-classic-ps-sql-ext-listener.md) enthält

## <a name="create-load-balanced-vm-endpoints-with-direct-server-return"></a>Erstellen von virtuellen Computer Endpunkte Lastenausgleich mit direkter Server zurückgegeben

Für ILB müssen Sie zuerst den internen Lastenausgleich erstellen. Dies geschieht im folgenden Skript an.

[AZURE.INCLUDE [load-balanced-endpoints](../../includes/virtual-machines-ag-listener-load-balanced-endpoints.md)]

1. Für **ILB**sollten Sie eine statische IP-Adresse zuweisen. Untersuchen Sie zuerst die aktuelle VNet Konfiguration, indem Sie den folgenden Befehl ausführen:

        (Get-AzureVNetConfig).XMLConfiguration

1. Notieren Sie sich den **Subnetz** Namen für das Subnetz, das die virtuellen Computern enthält, die die Replikate zu hosten. Dies wird in den Parameter **$SubnetName** im Skript verwendet werden.

1. Notieren Sie den Namen des **VirtualNetworkSite** und das Anfangs **AddressPrefix** für das Subnetz, das die virtuellen Computern enthält, die die Replikate zu hosten. Suchen Sie nach eine verfügbare IP-Adresse, indem Sie beide Werte an den Befehl **Test-AzureStaticVNetIP** übergeben und überprüfen die **AvailableAddresses**. Wenn der VNet mit dem Namen *MyVNet* und hatten wurde eine Subnetzadresse-Bereich, die am *172.16.0.128*gestartet, mit dem folgende Befehl würden verfügbare Adressen Liste:

        (Test-AzureStaticVNetIP -VNetName "MyVNet"-IPAddress 172.16.0.128).AvailableAddresses

1. Wählen Sie eine der verfügbaren Adressen aus, und klicken Sie in den Parameter **$ILBStaticIP** des folgenden Skripts verwenden.

3. Kopieren Sie das folgende PowerShell-Skript in einen Text-Editor, und legen Sie die Variable Werte entsprechend Ihrer Umgebung (Beachten Sie die Standardeinstellungen für einige Parameter erteilt wurde). Beachten Sie, dass keine vorhandene Bereitstellungen, mit denen Gruppen die ILB hinzufügen. Weitere Informationen ILB Anforderungen finden Sie unter [Internen Lastenausgleich](../load-balancer/load-balancer-internal-overview.md). Auch wenn Ihre Verfügbarkeit Gruppe Azure Regionen umfasst, führen Sie das Skript einmal in jedem Datencenter für die Cloud-Dienst und Knoten, die in dieser Datacenter befinden.

        # Define variables
        $ServiceName = "<MyCloudService>" # the name of the cloud service that contains the availability group nodes
        $AGNodes = "<VM1>","<VM2>","<VM3>" # all availability group nodes containing replicas in the same cloud service, separated by commas
        $SubnetName = "<MySubnetName>" # subnet name that the replicas use in the VNet
        $ILBStaticIP = "<MyILBStaticIPAddress>" # static IP address for the ILB in the subnet
        $ILBName = "AGListenerLB" # customize the ILB name or use this default value

        # Create the ILB
        Add-AzureInternalLoadBalancer -InternalLoadBalancerName $ILBName -SubnetName $SubnetName -ServiceName $ServiceName -StaticVNetIPAddress $ILBStaticIP

        # Configure a load balanced endpoint for each node in $AGNodes using ILB
        ForEach ($node in $AGNodes)
        {
            Get-AzureVM -ServiceName $ServiceName -Name $node | Add-AzureEndpoint -Name "ListenerEndpoint" -LBSetName "ListenerEndpointLB" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -InternalLoadBalancerName $ILBName -DirectServerReturn $true | Update-AzureVM
        }

1. Nachdem Sie die Variablen eingerichtet haben, kopieren Sie das Skript aus der Text-Editor in Ihre Azure PowerShell-Sitzung, um Sie auszuführen. Wenn die Aufforderung weiterhin zeigt >>, geben Sie die EINGABETASTE erneut, um sicherzustellen, dass das Skript gestartet. Notiz

>[AZURE.NOTE] Im klassische Azure-Portal unterstützt keine internen Lastenausgleich zu diesem Zeitpunkt entweder die ILB oder die Endpunkte in der klassischen Azure-Portal nicht angezeigt wird. **Get-AzureEndpoint** gibt jedoch eine interne IP-Adresse aus, wenn dort Lastenausgleich ausgeführt wird. Andernfalls wird null zurückgegeben.

## <a name="verify-that-kb2854082-is-installed-if-necessary"></a>Stellen Sie sicher, dass KB2854082 bei Bedarf installiert ist

[AZURE.INCLUDE [kb2854082](../../includes/virtual-machines-ag-listener-kb2854082.md)]

## <a name="open-the-firewall-ports-in-availability-group-nodes"></a>Öffnen Sie die Firewallports in Verfügbarkeit Gruppieren von Knoten

[AZURE.INCLUDE [firewall](../../includes/virtual-machines-ag-listener-open-firewall.md)]

## <a name="create-the-availability-group-listener"></a>Erstellen der Verfügbarkeit Gruppe Zuhörer

[AZURE.INCLUDE [firewall](../../includes/virtual-machines-ag-listener-create-listener.md)]

1. Für ILB müssen Sie die IP-Adresse der zuvor erstellten internen laden Lastenausgleich (ILB) verwenden. Verwenden Sie das folgende Skript, um diese IP-Adresse in PowerShell zu erhalten.

        # Define variables
        $ServiceName="<MyServiceName>" # the name of the cloud service that contains the AG nodes
        (Get-AzureInternalLoadBalancer -ServiceName $ServiceName).IPAddress

1. Klicken Sie auf eine von der virtuellen Maschinen Kopieren von PowerShell-Skript für Ihr Betriebssystem in einem Text-Editor, und legen Sie die Variablen auf die Werte, die Sie zuvor notiert haben.

    Verwenden Sie für Windows Server 2012 oder höher das folgende Skript aus:

        # Define variables
        $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP Address resource name
        $ILBIP = “<X.X.X.X>” # the IP Address of the Internal Load Balancer (ILB)

        Import-Module FailoverClusters

        Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
        
    Verwenden Sie für Windows Server 2008 R2 das folgende Skript aus:

        # Define variables
        $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP Address resource name
        $ILBIP = “<X.X.X.X>” # the IP Address of the Internal Load Balancer (ILB)

        Import-Module FailoverClusters

        cluster res $IPResourceName /priv enabledhcp=0 address=$ILBIP probeport=59999  subnetmask=255.255.255.255
    

1. Einmal Sie festgelegt haben die Variablen, einer erhöhten Windows PowerShell-Fenster zu öffnen und kopieren Sie das Skript aus dem Texteditor und fügen Ihre Azure PowerShell-Sitzung, um Sie auszuführen. Wenn die Aufforderung weiterhin zeigt >>, geben Sie die EINGABETASTE erneut, um sicherzustellen, dass das Skript gestartet.

2. Wiederholen Sie diesen Schritt jedes virtuellen Computers. Dieses Skript die Ressource IP-Adresse die IP-Adresse des Cloud-Dienst konfiguriert und legt anderen Parameter wie den Port Prüfpunkt. Wenn die IP-Adresse Ressource online ist, kann es aus den Lastenausgleich Endpunkt zuvor in diesem Lernprogramm erstellt haben klicken Sie dann auf eine der Abfrage auf den Port Prüfpunkt reagieren.

## <a name="bring-the-listener-online"></a>Zeigen Sie die Zuhörer online

[AZURE.INCLUDE [Bring-Listener-Online](../../includes/virtual-machines-ag-listener-bring-online.md)]

## <a name="follow-up-items"></a>Nachverfolgen von Elementen

[AZURE.INCLUDE [Follow-up](../../includes/virtual-machines-ag-listener-follow-up.md)]

## <a name="test-the-availability-group-listener-within-the-same-vnet"></a>Testen der Verfügbarkeit Gruppe Zuhörer (innerhalb der gleichen VNet)

[AZURE.INCLUDE [Test-Listener-Within-VNET](../../includes/virtual-machines-ag-listener-test.md)]

## <a name="next-steps"></a>Nächste Schritte

[AZURE.INCLUDE [Listener-Next-Steps](../../includes/virtual-machines-ag-listener-next-steps.md)]

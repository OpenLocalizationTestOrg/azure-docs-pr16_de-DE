<properties
    pageTitle="Erweitern von HDInsight mithilfe von virtuellen Netzwerk | Microsoft Azure"  
    description="Erfahren Sie, wie virtuelle Netzwerk Azure HDInsight Verbindung mit anderen Cloudressourcen oder Ressourcen Datencenters"
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/21/2016"
   ms.author="larryfr"/>


#<a name="extend-hdinsight-capabilities-by-using-azure-virtual-network"></a>Erweitern Sie HDInsight-Funktionen, die mithilfe von Azure-virtuellen Netzwerk

Azure virtuelles Netzwerk ermöglicht es, erweitern Ihre Hadoop Lösungen lokale Ressourcen, wie etwa SQL Server einbinden, kombinieren Arten von mehreren HDInsight Cluster oder sichere private Netzwerke zwischen Ressourcen in der Cloud zu erstellen.

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell-and-cli.md)]


##<a name="a-idwhatisawhat-is-azure-virtual-network"></a><a id="whatis"></a>Was ist Azure-virtuellen Netzwerk?

[Virtuelle Netzwerk Azure](https://azure.microsoft.com/documentation/services/virtual-network/) ermöglicht Ihnen, erstellen Sie ein sicheres, beständigen Netzwerk, die mit den Ressourcen, die Sie für Ihre Lösung benötigen. Ein virtuelles Netzwerk können Sie:

* Verbinden Sie die Cloudressourcen zusammen in ein privates Netzwerk (nur Cloud).

    ![Diagramm nur Cloud-Konfiguration](media/hdinsight-extend-hadoop-virtual-network/cloud-only.png)

    Verwenden zum Verknüpfen von Azure-Diensten mit Azure HDInsight virtuellen Netzwerk ermöglicht Folgendes:

    * **Aufrufen HDInsight Services oder Aufträge** aus Azure Websites oder Dienste in Azure-virtuellen Computern ausgeführt.

    * **Direkt übertragen von Daten** zwischen HDInsight und Azure SQL-Datenbank, SQL Server oder eine andere Daten Speicher Lösung, die auf einem virtuellen Computer ausgeführt.

    * **Kombinieren von mehreren HDInsight Servern** zu einer einzigen Lösung. HDInsight Cluster einer Vielzahl von Typen, die entsprechen den Arbeitsbelastung oder Technologie, die für der Cluster optimiert ist nützlich sein. Es gibt keine unterstützte Methode zum einen Cluster zu erstellen, der mehrere Typen, z. B. Storm und HBase auf einem Cluster kombiniert. Verwenden eines virtuellen Netzwerks kann mehrere Cluster direkt miteinander kommunizieren.

* Verbinden Sie mit Ihrem lokalen Datencenter Netzwerk (zwischen Standorten oder Punkt-zu-Standort-) Cloudressourcen über ein virtuelles privates Netzwerk (VPN).

    Standort-zu-Standort-Konfiguration können Sie mithilfe einer Hardware VPN oder den Dienst Routing und RAS mehrere Ressourcen aus Datencenters der Azure-virtuellen Netzwerk verbinden.

    ![Diagramm-Standorten Konfiguration](media/hdinsight-extend-hadoop-virtual-network/site-to-site.png)

    Punkt-zu-Standort-Konfiguration können Sie eine bestimmte Ressource mithilfe der Software VPN mit dem Azure-virtuellen-Netzwerk zu verbinden.

    ![Diagramm-Punkt-zu-Standort-Konfiguration](media/hdinsight-extend-hadoop-virtual-network/point-to-site.png)

    Virtuelle Netzwerk zum Verwenden der Cloud und Datencenters ermöglicht ähnliche Szenarios an der Konfiguration nur Cloud. Aber statt nicht für die Arbeit mit Ressourcen in der Cloud beschränkt, Sie können auch arbeiten mit Ressourcen in Ihrem Datencenter.

    * **Direkt übertragen von Daten** zwischen HDInsight und Datencenters aus. Ein Beispiel ist Sqoop zum Übertragen von Daten in den oder aus SQL Server oder Lesen von Daten von einer Line-of-Business (LOB) Anwendung generierten verwenden.

    * **Aufrufen HDInsight Services oder Aufträge** LOB-Anwendung. Ein Beispiel ist HBase Java-APIs zum Speichern und Abrufen von Daten aus einem Cluster HDInsight HBase verwenden.

Weitere Informationen zu Funktionen, Vorteile und Funktionen virtuelles Netzwerk finden Sie unter der [Azure-virtuellen Network (Übersicht)](../virtual-network/virtual-networks-overview.md).

> [AZURE.NOTE] Sie müssen das virtuelle Azure-Netzwerk vor der Bereitstellung eines HDInsight Clusters erstellen. Weitere Informationen finden Sie unter [Aufgaben zur virtuelles Netzwerk](https://azure.microsoft.com/documentation/services/virtual-network/).

## <a name="virtual-network-requirements"></a>Virtuelle Netzwerk-Anforderungen

> [AZURE.IMPORTANT] Erstellen eines HDInsight Clusters in einem virtuellen Netzwerk erfordert bestimmte virtuelle Netzwerk Konfigurationen, die in diesem Abschnitt beschrieben werden.

###<a name="location-based-virtual-networks"></a>Standortbasierte virtuelle Netzwerke

Azure HDInsight unterstützt nur standortbasierte virtuelle Netzwerke und funktioniert derzeit nicht mit virtuelle Netzwerke basierend auf Zugehörigkeit Gruppe.

###<a name="classic-or-v2-virtual-network"></a>Classic oder Version 2 virtuelles Netzwerk

Windows-basiertem Cluster erfordern ein klassischen virtuelles Netzwerk, während Linux-basierten Cluster ein Azure Ressourcenmanager virtuelles Netzwerk erwarten. Wenn Sie nicht die richtige Art von Netzwerk verfügen, wird es nicht verwendet werden, wenn Sie den Cluster zu erstellen.

Wenn Sie Ressourcen in einem virtuellen Netzwerk, die nicht vom Cluster verwendet werden kann, die Sie verfügen erstellen möchten, können Sie erstellen ein neues virtuelle Netzwerk, die vom Cluster verwendet werden kann, und verbinden Sie es mit dem inkompatiblen virtuelle Netzwerk. Anschließend können Sie den Cluster erstellen, in der Netzwerkversion, die erforderlich ist, und Ressourcen zugreifen, in dem anderen Netzwerk werden, da die beiden beigetreten sind. Weitere Informationen zum Herstellen einer Verbindung klassischen und neue virtuelle Netzwerke finden Sie unter [Herstellen einer Verbindung klassischen VNets auf neue VNets](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

###<a name="custom-dns"></a>Benutzerdefinierte DNS-Einträge

Wenn Sie ein virtuelles Netzwerk erstellen, stellt Azure Auflösung von Namen für Azure Diensten wie HDInsight, die im Netzwerk installiert werden. Möglicherweise müssen Sie jedoch eigene System DNS (Domain Name) für Situationen wie cross Auflösung des Domänennamens Netzwerk verwenden. Bei der Kommunikation zwischen Diensten in ansässig verknüpft zwei virtuelle Netzwerke. HDInsight unterstützt sowohl die standardmäßigen Azure namensauflösung als auch die benutzerdefinierten DNS-Einträge bei Verwendung mit Azure-virtuellen Netzwerk.

Weitere Informationen zum Verwenden von Ihrem eigenen DNS-Servers mit Azure-virtuellen Netzwerk finden Sie unter Abschnitt __mit einer Auflösung von Namen mithilfe Ihrer eigenen DNS-Server__ des Dokuments [Mit einer Auflösung von Namen für virtuellen Computern und Rolleninstanzen](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) .

###<a name="secured-virtual-networks"></a>Geschützte virtuelle Netzwerke

Der Dienst HDInsight ist ein verwalteter Dienst, und ist ein Internetzugang erforderlich während der Bereitstellung und während der Ausführung. Dies ist, damit die Azure die Integrität des Cluster, überwachen kann Failover Cluster Ressourcen initiiert, ändern Sie die Anzahl der Knoten im Cluster erfolgt über Anpassungsbereich für Vorgänge und andere Verwaltungsaufgaben.

Wenn Sie HDInsight in einem gesicherten virtuellen Netzwerk installieren müssen, müssen Sie über den Port 443 für die folgenden IP-Adressen, die Azure HDInsight Cluster verwalten können, die eingehenden Zugriff zulassen.

* 168.61.49.99
* 23.99.5.239
* 168.61.48.131
* 138.91.141.162

Eingehenden Zugriffs von Port 443 für diese Adressen ermöglicht Ihnen, HDInsight in einem gesicherten virtuellen Netzwerk erfolgreich zu installieren.

> [AZURE.IMPORTANT] HDInsight unterstützt keine ausgehenden Datenverkehr, nur eingehenden Datenverkehr einschränken. Wenn Sie Netzwerk-Sicherheitsgruppe Regeln für das Subnetz definieren, die HDInsight enthält, nur verwenden Sie eingehende Regeln.

Die folgenden Beispiele veranschaulichen, wie eine neue Netzwerk-Sicherheitsgruppe erstellen, können die erforderlichen Adressen und gilt für die Sicherheitsgruppe ein Subnetz innerhalb des virtuellen Netzwerks. Diesen Schritten wird vorausgesetzt, dass Sie bereits erstellt haben, ein virtuelles Netzwerk und Subnetz gehören, denen Sie in HDInsight installieren möchten.

__Mithilfe der PowerShell Azure__

    $vnetName = "Replace with your virtual network name"
    $resourceGroupName = "Replace with the resource group the virtual network is in"
    $subnetName = "Replace with the name of the subnet that HDInsight will be installed into"
    # Get the Virtual Network object
    $vnet = Get-AzureRmVirtualNetwork `
        -Name $vnetName `
        -ResourceGroupName $resourceGroupName
    # Get the region the Virtual network is in.
    $location = $vnet.Location
    # Get the subnet object
    $subnet = $vnet.Subnets | Where-Object Name -eq $subnetName
    # Create a new Network Security Group.
    # And add exemptions for the HDInsight health and management services.
    $nsg = New-AzureRmNetworkSecurityGroup `
        -Name "hdisecure" `
        -ResourceGroupName $resourceGroupName `
        -Location $location `
        | Add-AzureRmNetworkSecurityRuleConfig `
            -name "hdirule1" `
            -Description "HDI health and management address 168.61.49.99" `
            -Protocol "*" `
            -SourcePortRange "*" `
            -DestinationPortRange "443" `
            -SourceAddressPrefix "168.61.49.99" `
            -DestinationAddressPrefix "VirtualNetwork" `
            -Access Allow `
            -Priority 300 `
            -Direction Inbound `
        | Add-AzureRmNetworkSecurityRuleConfig `
            -Name "hdirule2" `
            -Description "HDI health and management 23.99.5.239" `
            -Protocol "*" `
            -SourcePortRange "*" `
            -DestinationPortRange "443" `
            -SourceAddressPrefix "23.99.5.239" `
            -DestinationAddressPrefix "VirtualNetwork" `
            -Access Allow `
            -Priority 301 `
            -Direction Inbound `
        | Add-AzureRmNetworkSecurityRuleConfig `
            -Name "hdirule3" `
            -Description "HDI health and management 168.61.48.131" `
            -Protocol "*" `
            -SourcePortRange "*" `
            -DestinationPortRange "443" `
            -SourceAddressPrefix "168.61.48.131" `
            -DestinationAddressPrefix "VirtualNetwork" `
            -Access Allow `
            -Priority 302 `
            -Direction Inbound `
        | Add-AzureRmNetworkSecurityRuleConfig `
            -Name "hdirule4" `
            -Description "HDI health and management 138.91.141.162" `
            -Protocol "*" `
            -SourcePortRange "*" `
            -DestinationPortRange "443" `
            -SourceAddressPrefix "138.91.141.162" `
            -DestinationAddressPrefix "VirtualNetwork" `
            -Access Allow `
            -Priority 303 `
            -Direction Inbound
    # Set the changes to the security group
    Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
    # Apply the NSG to the subnet
    Set-AzureRmVirtualNetworkSubnetConfig `
        -VirtualNetwork $vnet `
        -Name $subnetName `
        -AddressPrefix $subnet.AddressPrefix `
        -NetworkSecurityGroupId $nsg

__Verwendung der Azure CLI__

1. Verwenden Sie den folgenden Befehl zum Erstellen einer neuen Netzwerk-Sicherheitsgruppe mit dem Namen `hdisecure`. Ersetzen Sie die Ressourcengruppe, die enthält das virtuelle Azure-Netzwerk und den Speicherort (Region), dem in die Gruppe erstellt wurde __RESOURCEGROUPNAME__ und __einen Speicherort__ aus.

        azure network nsg create RESOURCEGROUPNAME hdisecure LOCATION
    
    Nachdem die Gruppe erstellt wurde, erhalten Sie Informationen zur neuen Gruppe. Suchen nach einer Zeile ähnlich wie der folgende aus, und speichern Sie die `/subscriptions/GUID/resourceGroups/RESOURCEGROUPNAME/providers/Microsoft.Network/networkSecurityGroups/hdisecure` Informationen. Es wird in einem späteren Schritt verwendet werden.
    
        data:    Id                              : /subscriptions/GUID/resourceGroups/RESOURCEGROUPNAME/providers/Microsoft.Network/networkSecurityGroups/hdisecure

2. Anhand der folgenden Regeln zur neuen Netzwerksicherheitsgruppe, mit denen eingehende Kommunikation auf Port 443 vom Azure HDInsight Gesundheit und Management-Dienst hinzufügen. Ersetzen Sie __RESOURCEGROUPNAME__ durch den Namen der Ressourcengruppe, die das virtuelle Azure-Netzwerk enthält.

        azure network nsg rule create RESOURCEGROUPNAME hdisecure hdirule1 -p "*" -o "*" -u "443" -f "168.61.49.99" -e "VirtualNetwork" -c "Allow" -y 300 -r "Inbound"
        azure network nsg rule create RESOURCEGROUPNAME hdisecure hdirule2 -p "*" -o "*" -u "443" -f "23.99.5.239" -e "VirtualNetwork" -c "Allow" -y 301 -r "Inbound"
        azure network nsg rule create RESOURCEGROUPNAME hdisecure hdirule3 -p "*" -o "*" -u "443" -f "168.61.48.131" -e "VirtualNetwork" -c "Allow" -y 302 -r "Inbound"
        azure network nsg rule create RESOURCEGROUPNAME hdisecure hdirule4 -p "*" -o "*" -u "443" -f "138.91.141.162" -e "VirtualNetwork" -c "Allow" -y 303 -r "Inbound"

3. Nachdem Sie die Regeln erstellt wurden, anhand der folgenden die neuen Netzwerk-Sicherheitsgruppe zu einem Subnetz anwenden. Ersetzen Sie __RESOURCEGROUPNAME__ durch den Namen der Ressourcengruppe, die das virtuelle Azure-Netzwerk enthält. Ersetzen Sie __VNETNAME__ und __SUBNETNAME__ mit dem Namen des virtuellen Netzwerks Azure und das Subnetz, das Sie bei der Installation HDInsight von verwendet wird.

        azure network vnet subnet set RESOURCEGROUPNAME VNETNAME SUBNETNAME -w "/subscriptions/GUID/resourceGroups/RESOURCEGROUPNAME/providers/Microsoft.Network/networkSecurityGroups/hdisecure"
    
    Sobald dieser Befehl abgeschlossen ist, können Sie in das gesicherte virtuelle Netzwerk im Subnetz verwendet, die in den folgenden Schritten erfolgreich HDInsight installieren.

> [AZURE.IMPORTANT] Der Azure Cloud verwenden genannten Schritte nur öffnen Zugriff auf den HDInsight Gesundheit und Management-Dienst. Dies können Sie einen HDInsight Cluster erfolgreich in das Subnetz, zu installieren, jedoch Zugriff auf den Cluster HDInsight von außerhalb des virtuellen Netzwerks standardmäßig blockiert wird. Sie müssen zusätzliche Netzwerk-Sicherheitsgruppe Regeln hinzugefügt werden soll, wenn Sie den Zugriff von außerhalb des virtuellen Netzwerks ermöglichen möchten.
>
> Beispielsweise um SSH-Zugriff aus dem Internet zulassen, müssen Sie eine Regel ähnlich wie der folgende hinzuzufügen: 
>
> * Azure PowerShell-```Add-AzureRmNetworkSecurityRuleConfig -Name "SSSH" -Description "SSH" -Protocol "*" -SourcePortRange "*" -DestinationPortRange "22" -SourceAddressPrefix "*" -DestinationAddressPrefix "VirtualNetwork" -Access Allow -Priority 304 -Direction Inbound```
> * Azure CLI-```azure network nsg rule create RESOURCEGROUPNAME hdisecure hdirule4 -p "*" -o "*" -u "22" -f "*" -e "VirtualNetwork" -c "Allow" -y 304 -r "Inbound"```

Weitere Informationen zum Netzwerk-Sicherheitsgruppen enthalten finden Sie unter [Netzwerk Sicherheitsgruppen (Übersicht)](../virtual-network/virtual-networks-nsg.md). Informationen zum Steuern von routing in einem Azure-virtuellen Netzwerk finden Sie unter [Benutzer definiert ist und die IP-Weiterleitung](../virtual-network/virtual-networks-udr-overview.md).

##<a name="a-idtasksatasks-and-information"></a><a id="tasks"></a>Aufgaben und Informationen

Dieser Abschnitt enthält Informationen zu allgemeinen Aufgaben und Informationen, die Sie möglicherweise bei Verwendung von HDInsight mit einem virtuellen Netzwerk müssen.

###<a name="determine-the-fqdn"></a>Bestimmen Sie den vollqualifizierten Domänennamen

HDInsight Cluster wird einen bestimmte vollqualifizierten Domänennamen (FULLY) für die Benutzeroberfläche virtuelles Netzwerk zugeordnet werden. Dies ist die Adresse, die beim Herstellen einer Verbindung mit dem Cluster von anderen Ressourcen im Netzwerk virtuelle verwendet werden soll. Wenn Sie um den vollqualifizierten Domänennamen zu ermitteln, anhand der folgenden URL den Verwaltungsdienst Ambari Abfrage:

    https://<clustername>.azurehdinsight.net/ambari/api/v1/clusters/<clustername>.azurehdinsight.net/services/<servicename>/components/<componentname>

> [AZURE.NOTE] Weitere Informationen zum Verwenden von Ambari mit HDInsight finden Sie unter [Monitor Hadoop Cluster im HDInsight mithilfe der Ambari-API](hdinsight-monitor-use-ambari-api.md).

Sie müssen den Clusternamen, Service und Komponente ausgeführt wird, auf dem Cluster, wie etwa die Ressourcenmanager aus angeben.

> [AZURE.NOTE] Die zurückgegebenen Daten ist ein Dokument von JavaScript Object Notation (JSON), die viele Informationen über die Komponente enthält. Sie sollten einen JSON-Parser zum Abrufen von verwenden, um nur den vollqualifizierten Domänennamen Extrahieren der `host_components[0].HostRoles.host_name` Wert.

Beispielsweise, um den vollqualifizierten Domänennamen aus einem Cluster HDInsight Hadoop zurückzukehren, können eine der folgenden Methoden Sie zum Abrufen der Daten für die Ressourcenmanager aus:

* [Azure PowerShell](../powershell-install-configure.md)

        $ClusterDnsName = <clustername>
        $Username = <cluster admin username>
        $Password = <cluster admin password>
        $DnsSuffix = ".azurehdinsight.net"
        $ClusterFQDN = $ClusterDnsName + $DnsSuffix

        $webclient = new-object System.Net.WebClient
        $webclient.Credentials = new-object System.Net.NetworkCredential($Username, $Password)

        $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/services/yarn/     components/resourcemanager"
        $Response = $webclient.DownloadString($Url)
        $JsonObject = $Response | ConvertFrom-Json
        $FQDN = $JsonObject.host_components[0].HostRoles.host_name
        Write-host $FQDN

* [Aufrollen](http://curl.haxx.se/) und [jq](http://stedolan.github.io/jq/)

        curl -G -u <username>:<password> https://<clustername>.azurehdinsight.net/ambari/api/v1/clusters/<clustername>.azurehdinsight.net/services/yarn/components/resourcemanager | jq .host_components[0].HostRoles.host_name

###<a name="connecting-to-hbase"></a>Herstellen einer Verbindung mit HBase

HBase Remote zum Verbinden mit mithilfe der Java-API müssen Sie die ZooKeeper Quorum-Adressen für den Cluster HBase bestimmen, und geben dies in Ihrer Anwendung.

Verwenden Sie die ZooKeeper Quorum Adresse, um eine der folgenden Methoden den Ambari-Verwaltungsdienst Abfragen:

* [Azure PowerShell](../powershell-install-configure.md)

        $ClusterDnsName = <clustername>
        $Username = <cluster admin username>
        $Password = <cluster admin password>
        $DnsSuffix = ".azurehdinsight.net"
        $ClusterFQDN = $ClusterDnsName + $DnsSuffix

        $webclient = new-object System.Net.WebClient
        $webclient.Credentials = new-object System.Net.NetworkCredential($Username, $Password)

        $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.zookeeper.quorum"
        $Response = $webclient.DownloadString($Url)
        $JsonObject = $Response | ConvertFrom-Json
        Write-host $JsonObject.items[0].properties.'hbase.zookeeper.quorum'

* [Aufrollen](http://curl.haxx.se/) und [jq](http://stedolan.github.io/jq/)

        curl -G -u <username>:<password> "https://<clustername>.azurehdinsight.net/ambari/api/v1/clusters/<clustername>.azurehdinsight.net/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.zookeeper.quorum" | jq .items[0].properties[]

> [AZURE.NOTE] Weitere Informationen zum Verwenden von Ambari mit HDInsight finden Sie unter [Monitor Hadoop Cluster im HDInsight mithilfe der Ambari-API](hdinsight-monitor-use-ambari-api.md).

Nachdem Sie die Quoruminformationen haben, verwenden Sie es in der Clientanwendung.

Zum Beispiel für eine Java-Anwendung, die die HBase-API verwendet, Sie eine **Hbase site.xml** Datei zum Projekt hinzufügen und geben Sie die Quoruminformationen in der Datei wie folgt:

```
<configuration>
  <property>
    <name>hbase.cluster.distributed</name>
    <value>true</value>
  </property>
  <property>
    <name>hbase.zookeeper.quorum</name>
    <value>zookeeper0.address,zookeeper1.address,zookeeper2.address</value>
  </property>
  <property>
    <name>hbase.zookeeper.property.clientPort</name>
    <value>2181</value>
  </property>
</configuration>
```

###<a name="verify-network-connectivity"></a>Überprüfen Sie die Netzwerkkonnektivität

Einige Dienste, wie etwa SQL Server, können eingehende Netzwerk Verbindungen einschränken. Dadurch wird verhindert, dass HDInsight erfolgreich arbeiten mit dieser Dienste.

Wenn Sie Probleme beim Zugriff auf einen Dienst aus HDInsight auftreten, finden Sie in der Dokumentation für den Dienst, um sicherzustellen, dass Sie Zugriff auf das Netzwerk aktiviert haben. Sie können auch Netzwerkzugriff durch Erstellen einer Azure-virtuellen Computern im gleichen virtuellen Netzwerk überprüfen und verwenden Clienttools zur Überprüfung des virtuellen Computers über das virtuelle Netzwerk eine Verbindung zum Dienst herstellen können.

##<a name="a-idnextstepsanext-steps"></a><a id="nextsteps"></a>Nächste Schritte

Die folgenden Beispiele veranschaulichen, wie HDInsight mit Azure-virtuellen Netzwerk verwendet:

* [Analysieren Sensordaten mit Storm und HBase in HDInsight](hdinsight-storm-sensor-data-analysis.md) - veranschaulicht, wie einen Cluster Storm und HBase in einem virtuellen Netzwerk konfigurieren sowie zum Schreiben von Daten per Remotezugriff auf HBase von Storm.

* [Bereitstellen von Hadoop Cluster in HDInsight](hdinsight-hadoop-provision-linux-clusters.md) - enthält Informationen zur Bereitstellung Hadoop Cluster, einschließlich Informationen zur Verwendung von Azure-virtuellen Netzwerk.

* [Verwenden von Sqoop mit Hadoop in HDInsight](hdinsight-use-sqoop-mac-linux.md) - enthält Informationen zur Verwendung von Sqoop zum Übertragen von Daten mit SQL Server über ein virtuelles Netzwerk.

Wenn Sie weitere Informationen zur Azure virtuelle Netzwerke finden Sie unter der [Azure-virtuellen Network (Übersicht)](../virtual-network/virtual-networks-overview.md).

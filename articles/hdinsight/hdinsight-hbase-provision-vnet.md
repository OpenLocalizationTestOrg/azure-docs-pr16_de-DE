<properties
    pageTitle="Erstellen Sie HBase Cluster in einem virtuellen Netzwerk | Microsoft Azure"
    description="Erste Schritte mit HBase in Azure HDInsight. Erfahren Sie, wie HDInsight HBase Cluster virtuellen Netzwerk Azure erstellen."
    keywords=""
    services="hdinsight,virtual-network"
    documentationCenter=""
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/18/2016"
   ms.author="jgao"/>

# <a name="create-hbase-clusters-in-azure-virtual-network"></a>Erstellen von HBase Cluster in Azure-virtuellen Netzwerk 

Informationen zum Erstellen von Azure HDInsight HBase Cluster in einem [Azure-virtuellen Netzwerk][1].

Virtuelle Netzwerk-Integration können HBase Cluster, damit Applikationen direkt mit HBase kommunizieren können mit dem gleichen virtuellen Netzwerk als Ihrer Anwendung bereitgestellt werden. Die folgende Vorteile:

- Direkte Konnektivität der Web-Anwendung zu den Knoten im Cluster HBase, womit Kommunikation über HBase Java-Remoteprozeduraufruf können anrufen APIs (RPC).
- Verbesserte Leistung durch einen nicht Ihren Verkehr über mehrere Gateways und Lastenausgleich wechseln.
- Die Möglichkeit, vertraulichen Informationen auf Weitere sichere Weise zu verarbeiten, ohne Sie zu einen öffentlichen Endpunkt verfügbar machen.

###<a name="prerequisites"></a>Erforderliche Komponenten
Bevor Sie dieses Lernprogramm beginnen, benötigen Sie Folgendes:

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion Azure abrufen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **Eine Arbeitsstationen mit Azure PowerShell**. Finden Sie unter [Installieren und Verwenden von Azure PowerShell](https://azure.microsoft.com/documentation/videos/install-and-use-azure-powershell/). 

## <a name="create-hbase-cluster-into-virtual-network"></a>Erstellen von HBase Cluster in virtuelles Netzwerk

In diesem Abschnitt erstellen Sie einen HBase Linux-basierten Cluster mit dem abhängige Azure-Speicher-Konto in ein Azure-virtuellen Netzwerk mithilfe einer [Vorlage Azure Ressourcenmanager](../resource-group-template-deploy.md). Andere Methoden zur Erstellung und das Verständnis die Einstellungen, finden Sie unter [Erstellen HDInsight Cluster](hdinsight-hadoop-provision-linux-clusters.md). Weitere Informationen zum Verwenden einer Vorlage zum Erstellen Hadoop Cluster in HDInsight, finden Sie unter [Erstellen Hadoop Cluster in mithilfe von Vorlagen Ressourcenmanager Azure HDInsight](hdinsight-hadoop-create-windows-clusters-arm-templates.md)

> [AZURE.NOTE] Einige Eigenschaften wurden in der Vorlage programmiert. Beispiel:
>
> * __Standort__: ostasiatischen USA 2
> * __Cluster Version: 3.4
> * __Cluster Worker Knoten zählen__: 4
> * __Standardkonto Speicher__: &lt;Cluster Name > Speichern
> * __Name des virtuellen Netzwerks__: &lt;Cluster Name >-Vnet
> * __Virtuellen Netzwerk Adressbereichs__: 10.0.0.0/16
> * __Subnetnamen__: Standard
> * __Subnetzadressbereichs__: 10.0.0.0/24
>
> &lt;Cluster Name > ersetzt werden mit dem Clusternamen, die Sie die Vorlage nutzen können.

1. Klicken Sie auf die folgende Abbildung, um die Vorlage im Azure-Portal zu öffnen. Die Vorlage befindet sich in einem öffentlichen Blob-Container. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hbase-cluster-in-vnet.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Geben Sie aus dem Blade **benutzerdefinierte Bereitstellung** Folgendes ein:

    - **Abonnements**: Wählen Sie ein Azure Abonnement verwendet, um die HDInsight Cluster, abhängige Speicher-Konto und das Azure virtuelle Netzwerk erstellen.
    - **Ressourcengruppe**: Wählen Sie **neu erstellen**, und geben Sie einen neuen Ressource an.
    - **Standort**: Wählen Sie einen Speicherort für die Ressourcengruppe.
    - **ClusterName**: Geben Sie einen Namen für den Hadoop Cluster, die Sie erstellen.
    - **Cluster-Benutzernamen und Ihr Kennwort**: der Standard-Anmeldename ist **Admin**.
    - **SSH-Benutzernamen und Ihr Kennwort**: der Standard-Benutzername ist **Sshuser**.  Sie können ihn umbenennen. 
    - **Kann ich die allgemeinen Geschäftsbedingungen der oben genannten stimmen**: (auswählen)
    

3. Klicken Sie auf **kaufen**. Dauert es ungefähr ungefähr 20 Minuten, einen Cluster erstellen. Nachdem der Cluster erstellt wurde, können Sie vorher Cluster in das Portal, um ihn zu öffnen klicken.

Nachdem Sie das Lernprogramm abgeschlossen haben, sollten Sie den Cluster löschen. Mit HDInsight Ihre Daten in Azure-Speicher gespeichert, sodass Sie problemlos einen Cluster löschen können, wenn es nicht verwendet wird. Sie unterliegen auch nach einem HDInsight Cluster, auch wenn es nicht verwendet wird. Da die Gebühren für den Cluster oft mehr als die Gebühren für Speicher sind, ist es economic sinnvoll Cluster löschen, wenn er nicht verwendet werden. Den Anweisungen von einem Cluster löschen finden Sie unter [Verwalten von Hadoop Cluster in HDInsight mithilfe des Azure-Portals](hdinsight-administer-use-management-portal.md#delete-clusters).

Um die Arbeit mit Ihrem neuen HBase Cluster beginnen, können Sie die Verfahren in [Erste Schritte mit HBase mit Hadoop in HDInsight](hdinsight-hbase-tutorial-get-started.md)gefunden.

## <a name="connect-to-the-hbase-cluster-using-hbase-java-rpc-apis"></a>Herstellen einer Verbindung mit HBase Java RPC-APIs HBase Cluster mit

1.  Erstellen Sie eine Infrastruktur als Service (IaaS) virtueller Computer in der gleichen Azure virtuellen Netzwerk und im gleichen Subnetz. Erstellen eines neuen IaaS virtuellen Computers Anweisungen finden Sie unter [Erstellen eines virtuellen Computern ausführen von Windows Server](../virtual-machines/virtual-machines-windows-hero-tutorial.md). Wenn Sie die Schritte in diesem Dokument folgen, müssen Sie für die Netzwerkkonfiguration Folgendes verwenden:

    - __Virtuellen Netzwerk__: &lt;Clustername >-Vnet
    - __Subnetz__: Standard

    > [AZURE.IMPORTANT] Ersetzen Sie &lt;Clustername > durch den Namen, die Sie beim Erstellen des Clusters HDInsight in den vorherigen Schritten verwendet.

    Mit diesen Werten, konfigurieren Sie des virtuellen Computers, um den gleichen virtuellen Netzwerk und Subnetz als HDInsight Cluster zu verwenden. Dadurch können sie direkt miteinander kommunizieren.

2.  Wenn eine Java-Anwendung mit HBase Remote herstellen, müssen Sie den vollqualifizierten Domänennamen (FULLY) verwenden. Um dies zu ermitteln, müssen Sie des Suffixes Verbindung-spezifische DNS-Cluster HBase erhalten. Wenn Sie dies tun, können Sie eine der folgenden Methoden verwenden:

    * Verwenden Sie einen Webbrowser, um einen Ambari Anruf zu tätigen:
    
        Navigieren Sie zu https://&lt;ClusterName >.azurehdinsight.net/api/v1/clusters/&lt;ClusterName > / Hosts?minimal_response = wahr. Es wird eine JSON-Datei mit der DNS-Suffixe.

    * Verwenden Sie die Ambari-Website:

        1. Navigieren Sie zu https://&lt;ClusterName >. azurehdinsight.net.
        2. Klicken Sie im oberen Menü auf **Hosts** .

    * Verwenden von Curl REST anrufen:

            curl -u <username>:<password> -k https://<clustername>.azurehdinsight.net/ambari/api/v1/clusters/<clustername>.azurehdinsight.net/services/hbase/components/hbrest

        Zurückgegebenen Daten die JavaScript Object Notation (JSON) suchen Sie den Eintrag "Hostname". Dies wird den vollqualifizierten Domänennamen für die Knoten im Cluster enthalten. Beispiel:

            ...
            "host_name": "wordkernode0.<clustername>.b1.cloudapp.net
            ...

        Der Teil der Domäne Name beginnt mit dem Clusternamen ist das DNS-Suffix an. Beispielsweise mycluster.b1.cloudapp.net.

    * Verwenden von Azure PowerShell
    
        Verwenden Sie das folgende Azure PowerShell-Skript, um die Funktion **Get-ClusterDetail** erfassen verwendet werden können, um die DNS-Suffix zurückzukehren:

            function Get-ClusterDetail(
                [String]
                [Parameter( Position=0, Mandatory=$true )]
                $ClusterDnsName,
                [String]
                [Parameter( Position=1, Mandatory=$true )]
                $Username,
                [String]
                [Parameter( Position=2, Mandatory=$true )]
                $Password,
                [String]
                [Parameter( Position=3, Mandatory=$true )]
                $PropertyName
                )
            {
            <#
                .SYNOPSIS
                 Displays information to facilitate an HDInsight cluster-to-cluster scenario within the same virtual network.
                .Description
                 This command shows the following 4 properties of an HDInsight cluster:
                 1. ZookeeperQuorum (supports only HBase type cluster)
                    Shows the value of HBase property "hbase.zookeeper.quorum".
                 2. ZookeeperClientPort (supports only HBase type cluster)
                    Shows the value of HBase property "hbase.zookeeper.property.clientPort".
                 3. HBaseRestServers (supports only HBase type cluster)
                    Shows a list of host FQDNs that run the HBase REST server.
                 4. FQDNSuffix (supports all cluster types)
                    Shows the FQDN suffix of hosts in the cluster.
                .EXAMPLE
                 Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName ZookeeperQuorum
                 This command shows the value of HBase property "hbase.zookeeper.quorum".
                .EXAMPLE
                 Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName ZookeeperClientPort
                 This command shows the value of HBase property "hbase.zookeeper.property.clientPort".
                .EXAMPLE
                 Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName HBaseRestServers
                 This command shows a list of host FQDNs that run the HBase REST server.
                .EXAMPLE
                 Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName FQDNSuffix
                 This command shows the FQDN suffix of hosts in the cluster.
            #>

                $DnsSuffix = ".azurehdinsight.net"

                $ClusterFQDN = $ClusterDnsName + $DnsSuffix
                $webclient = new-object System.Net.WebClient
                $webclient.Credentials = new-object System.Net.NetworkCredential($Username, $Password)

                if($PropertyName -eq "ZookeeperQuorum")
                {
                    $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.zookeeper.quorum"
                    $Response = $webclient.DownloadString($Url)
                    $JsonObject = $Response | ConvertFrom-Json
                    Write-host $JsonObject.items[0].properties.'hbase.zookeeper.quorum'
                }
                if($PropertyName -eq "ZookeeperClientPort")
                {
                    $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.zookeeper.property.clientPort"
                    $Response = $webclient.DownloadString($Url)
                    $JsonObject = $Response | ConvertFrom-Json
                    Write-host $JsonObject.items[0].properties.'hbase.zookeeper.property.clientPort'
                }
                if($PropertyName -eq "HBaseRestServers")
                {
                    $Url1 = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.rest.port"
                    $Response1 = $webclient.DownloadString($Url1)
                    $JsonObject1 = $Response1 | ConvertFrom-Json
                    $PortNumber = $JsonObject1.items[0].properties.'hbase.rest.port'

                    $Url2 = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/services/hbase/components/hbrest"
                    $Response2 = $webclient.DownloadString($Url2)
                    $JsonObject2 = $Response2 | ConvertFrom-Json
                    foreach ($host_component in $JsonObject2.host_components)
                    {
                        $ConnectionString = $host_component.HostRoles.host_name + ":" + $PortNumber
                        Write-host $ConnectionString
                    }
                }
                if($PropertyName -eq "FQDNSuffix")
                {
                    $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/services/YARN/components/RESOURCEMANAGER"
                    $Response = $webclient.DownloadString($Url)
                    $JsonObject = $Response | ConvertFrom-Json
                    $FQDN = $JsonObject.host_components[0].HostRoles.host_name
                    $pos = $FQDN.IndexOf(".")
                    $Suffix = $FQDN.Substring($pos + 1)
                    Write-host $Suffix
                }
            }

        Das Skript Azure PowerShell ausgeführt haben, verwenden Sie den folgenden Befehl, um die DNS-Suffix mithilfe der Funktion **Get-ClusterDetail** zurückzukehren. Geben Sie Ihre HDInsight HBase Clustername, Administratornamen und Administratorkennworts aus, wenn Sie diesen Befehl verwenden.

            Get-ClusterDetail -ClusterDnsName <yourclustername> -PropertyName FQDNSuffix -Username <clusteradmin> -Password <clusteradminpassword>

        Dadurch wird das DNS-Suffix zurückgegeben. Beispielsweise **yourclustername.b4.internal.cloudapp.net**.

    * Verwenden von RDP
    
        Sie können auch Remotedesktop Verbindung mit dem Cluster HBase (Sie werden mit dem der am Knoten), und führen Sie **Ipconfig** über eine Befehlszeile, um die DNS-Suffix zu erhalten. Anweisungen aktivieren (Remotedesktopprotokoll) und zum Herstellen einer Verbindung mit dem Cluster über RDP, finden Sie unter [Verwalten von Hadoop Cluster in Verwenden des Portals Azure HDInsight][hdinsight-admin-portal].
        
        ![hdinsight.hbase.DNS.surffix][img-dns-surffix]


<!--
3.  Change the primary DNS suffix configuration of the virtual machine. This enables the virtual machine to automatically resolve the host name of the HBase cluster without explicit specification of the suffix. For example, the *workernode0* host name will be correctly resolved to workernode0 of the HBase cluster.

    To make the configuration change:

    1. RDP into the virtual machine.
    2. Open **Local Group Policy Editor**. The executable is gpedit.msc.
    3. Expand **Computer Configuration**, expand **Administrative Templates**, expand **Network**, and then click **DNS Client**.
    - Set **Primary DNS Suffix** to the value obtained in step 2:

        ![hdinsight.hbase.primary.dns.suffix][img-primary-dns-suffix]
    4. Click **OK**.
    5. Reboot the virtual machine.
-->

Um den Besitz des virtuellen Computers mit dem HBase Cluster kommunizieren können, verwenden Sie den Befehl `ping headnode0.<dns suffix>` virtuellen Computer aus. Beispielsweise Ping headnode0.mycluster.b1.cloudapp.net.

Um diese Informationen in einer Java-Anwendung zu verwenden, folgen Sie die Schritten in [Maven verwenden, um Java-Anwendungen zu erstellen, die HBase mit HDInsight (Hadoop) verwenden](hdinsight-hbase-build-java-maven.md) , um eine Anwendung zu erstellen. Ändern Sie die Verbindung mit eines Remoteservers HBase Anwendung, damit die **Hbase site.xml** Datei in diesem Beispiel verwenden Sie den vollqualifizierten Domänennamen für Zookeeper aus. Beispiel:

    <property>
        <name>hbase.zookeeper.quorum</name>
        <value>zookeeper0.<dns suffix>,zookeeper1.<dns suffix>,zookeeper2.<dns suffix></value>
    </property>

> [AZURE.NOTE] Weitere Informationen zu mit einer namensauflösung von in Azure finden Sie unter virtuelle Netzwerke, wie Sie Ihre eigenen DNS-Server, einschließlich [Namen Auflösung (DNS)](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

##<a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm haben Sie gelernt, wie ein HBase Cluster zu erstellen. Weitere Informationen finden Sie unter:

- [Erste Schritte mit HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md)
- [Konfigurieren der Replikation HBase HDInsight](hdinsight-hbase-geo-replication.md)
- [Hadoop Cluster in HDInsight zu erstellen.](hdinsight-provision-clusters.md)
- [Erste Schritte mit HBase mit Hadoop in HDInsight](hdinsight-hbase-tutorial-get-started.md)
- [Analysieren Sie Twitter-Grüße mit HBase in HDInsight](hdinsight-hbase-analyze-twitter-sentiment.md)
- [Virtuelle Network (Übersicht)][vnet-overview]


[1]: http://azure.microsoft.com/services/virtual-network/
[2]: http://technet.microsoft.com/library/ee176961.aspx
[3]: http://technet.microsoft.com/library/hh847889.aspx

[hbase-get-started]: hdinsight-hbase-tutorial-get-started.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[vnet-overview]: ../virtual-network/virtual-networks-overview.md
[vm-create]: ../virtual-machines/virtual-machines-windows-hero-tutorial.md

[azure-portal]: https://portal.azure.com
[azure-create-storageaccount]: ../storage-create-storage-account.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md#rdp

[hdinsight-powershell-reference]: https://msdn.microsoft.com/library/dn858087.aspx


[twitter-streaming-api]: https://dev.twitter.com/docs/streaming-apis
[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter


[powershell-install]: powershell-install-configure.md


[hdinsight-customize-cluster]: hdinsight-hadoop-customize-cluster.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage-powershell]: ../hdinsight-hadoop-use-blob-storage.md#powershell
[hdinsight-analyze-flight-delay-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-hive-odbc]: hdinsight-connect-excel-hive-ODBC-driver.md
[hdinsight-hbase-replication-dns]: hdinsight-hbase-geo-replication-configure-DNS.md

[img-dns-surffix]: ./media/hdinsight-hbase-provision-vnet/DNSSuffix.png
[img-primary-dns-suffix]: ./media/hdinsight-hbase-provision-vnet/PrimaryDNSSuffix.png
[img-provision-cluster-page1]: ./media/hdinsight-hbase-provision-vnet/hbasewizard1.png "Bereitstellen von Details zu den neuen HBase cluster"
[img-provision-cluster-page5]: ./media/hdinsight-hbase-provision-vnet/hbasewizard5.png "Verwenden Sie zum Anpassen eines HBase Clusters Aktion Skript"

[azure-preview-portal]: https://portal.azure.com

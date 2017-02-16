<properties
    pageTitle="Verfügbarkeit von Hadoop Cluster in HDInsight | Microsoft Azure"
    description="HDInsight bereitstellt, hochgradig verfügbar und zuverlässig Cluster mit einen weiteren am Knoten."
    services="hdinsight"
    tags="azure-portal"
    editor="cgronlun"
    manager="jhubbard"
    authors="mumian"
    documentationCenter=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/21/2016"
    ms.author="jgao"/>


#<a name="availability-and-reliability-of-windows-based-hadoop-clusters-in-hdinsight"></a>Verfügbarkeit und Zuverlässigkeit der Windows-basiertem Hadoop Cluster in HDInsight


>[AZURE.NOTE] Die Schritte in diesem Dokument verwendete sind Windows-basiertem HDInsight Cluster. Wenn Sie einen Linux-basierten Cluster verwenden, finden Sie unter [Verfügbarkeit und Zuverlässigkeit der Linux-basierten Hadoop Cluster in HDInsight](hdinsight-high-availability-linux.md) Linux-spezifische Informationen.

HDInsight kann Kunden bereitstellen eine Vielzahl von Clustertypen für andere Daten Analytics Auslastung. Clustertypen heute angeboten werden Hadoop Cluster für die Abfrage und Analyse Auslastung, HBase Cluster für NoSQL Auslastung und Storm Cluster für die Verarbeitung von Ereignissen Echtzeit Auslastung. Innerhalb eines bestimmten Cluster Typs gibt es unterschiedliche Rollen für die verschiedenen Knoten. Beispiel:



- Hadoop Cluster für HDInsight sind mit zwei Rollen bereitgestellt:
    - Kopf Knoten (2 Knoten)
    - Datenknoten (mindestens 1 Knoten)

- HBase Zuordnungseinheiten für HDInsight sind mit drei Rollen bereitgestellt:
    - Kopf Servern (2 Knoten)
    - Region Servers (mindestens 1 Knoten)
    - Master-/Zookeeper Knoten (3 Knoten)

- Storm Cluster für HDInsight sind mit drei Rollen bereitgestellt:
    - Nimbus Knoten (2 Knoten)
    - Vorgesetzten Servers (mindestens 1 Knoten)
    - Zookeeper Knoten (3 Knoten)

Standard-Implementierungen der Hadoop Cluster befindet sich normalerweise auf einen einzelnen am Knoten. HDInsight hebt die einzelnen geleisteten des Fehlers durch Hinzufügen einer sekundären am Knoten/Head Server/Nimbus Knoten zum Erhöhen der Verfügbarkeit und Zuverlässigkeit des Diensts zum Verwalten von Auslastung erforderlich sind. Diese am Knoten Seitenkopf keinen Servern/Nimbus Knoten dienen den Ausfall der Worker Knoten reibungslos verwalten, aber alle Ausfall der Gestaltungsvorlage auf dem am Knoten ausgeführten Dienste würde Cluster funktioniert nicht mehr.


[ZooKeeper](http://zookeeper.apache.org/ ) Knoten (ZKs) hinzugefügt wurden und für die Füllzeichen Wahl am Knoten verwendet werden, und um sicherzustellen, dass die Worker Knoten und Gateways (GWs) wissen, wann über treten auf den zweiten am Knoten (Kopf Knoten 1), wenn der aktive am Knoten (Kopf Node0) inaktiv wird.

![Diagramm der hochgradig zuverlässigen am Knoten in der HDInsight Hadoop-Implementierung.](./media/hdinsight-high-availability/hadoop.high.availability.architecture.diagram.png)




## <a name="check-active-head-node-service-status"></a>Überprüfen der aktiven am Knoten-Dienststatus
Bestimmen, welche am Knoten aktiv ist und überprüfen Sie den Status der Dienste, die in diesem Knoten am laufen, müssen Sie mithilfe der (Remotedesktopprotokoll) mit dem Cluster Hadoop verbinden. Den RDP Anweisungen finden Sie unter [Verwalten von Hadoop Cluster in HDInsight mithilfe des Azure-Portals](hdinsight-administer-use-management-portal.md#connect-to-hdinsight-clusters-by-using-rdp). Nachdem Sie mit dem Cluster verbunden haben, doppelklicken Sie auf das **Hadoop Dienst verfügbar** -Symbol auf dem Desktop, um den Status zu welcher am Knoten der Namenode abzurufen, Jobtracker, Templeton, Oozieservice, Metastore und Hiveserver2 Services ausgeführt werden, oder für HDI 3.0, Namenode, Ressourcenmanager, Verlauf Server, Templeton, Oozieservice, Metastore und Hiveserver2 Dienste.

![](./media/hdinsight-high-availability/Hadoop.Service.Availability.Status.png)

Klicken Sie auf den Screenshot ist der aktive am Knoten *headnode0*.

## <a name="access-log-files-on-the-secondary-head-node"></a>Access-Protokolldateien auf dem sekundären am Knoten

Zugriff auf Auftrag von Protokollen auf dem sekundären am Knoten den Fall, dass es im aktiven am Knoten Durchsuchen der JobTracker UI geworden ist immer noch funktioniert wie für den primären aktiven Knoten. JobTracker zugreifen zu können, müssen Sie zum Hadoop Cluster verbinden über RDP wie im vorherigen Abschnitt beschrieben. Nachdem Sie Remote zum Cluster haben, doppelklicken Sie auf das Symbol **Hadoop Name Knoten Status** auf dem Desktop, und klicken Sie dann auf die **Protokolle NameNode** , um das Verzeichnis von Protokollen auf dem sekundären am Knoten zu gelangen.

![](./media/hdinsight-high-availability/Hadoop.Head.Node.Log.Files.png)


## <a name="configure-head-node-size"></a>Konfigurieren der Knotengröße am
Die am Knoten werden standardmäßig als großen virtuellen Computern (virtuellen Computern) zugeordnet. Diese Größe ist ausreichend für die Verwaltung von den meisten Hadoop Aufträge Cluster ausgeführt. Es gibt jedoch einige Szenarien, die besonders große virtuellen Computern für die am Knoten erfordern möglicherweise. Ein Beispiel ist, wenn der Cluster verfügt über eine große Anzahl von kleine Oozie Aufträge verwalten.

Besonders große virtuellen Computern können mithilfe von Azure PowerShell-Cmdlets oder das HDInsight SDK konfiguriert werden.

Die Erstellung und Bereitstellung von einem Cluster mithilfe von Azure PowerShell finden Sie unter [Verwalten von HDInsight mithilfe der PowerShell](hdinsight-administer-use-powershell.md). Die Konfiguration eines besonders große am Knotens erfordert das Hinzufügen der `-HeadNodeVMSize ExtraLarge` -Parameter der `New-AzureRmHDInsightcluster` Cmdlet in diesem Code verwendet.

    # Create a new HDInsight cluster in Azure PowerShell
    # Configured with an ExtraLarge head-node VM
    New-AzureRmHDInsightCluster `
                -ResourceGroupName $resourceGroupName `
                -ClusterName $clusterName ` 
                -Location $location `
                -HeadNodeVMSize ExtraLarge `
                -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" `
                -DefaultStorageAccountKey $storageAccountKey `
                -DefaultStorageContainerName $containerName  `
                -ClusterSizeInNodes $clusterNodes

Für die SDK ähnelt die Geschichte. Die Erstellung und Bereitstellung von einem Cluster mit dem SDK wird in [Mithilfe von HDInsight .NET SDK](hdinsight-provision-clusters.md#sdk)beschrieben. Die Konfiguration eines besonders große am Knotens erfordert das Hinzufügen der `HeadNodeSize = NodeVMSize.ExtraLarge` -Parameter der `ClusterCreateParameters()` in dieser Code verwendete Methode.

    # Create a new HDInsight cluster with the HDInsight SDK
    # Configured with an ExtraLarge head-node VM
    ClusterCreateParameters clusterInfo = new ClusterCreateParameters()
    {
        Name = clustername,
        Location = location,
        HeadNodeSize = NodeVMSize.ExtraLarge,
        DefaultStorageAccountName = storageaccountname,
        DefaultStorageAccountKey = storageaccountkey,
        DefaultStorageContainer = containername,
        UserName = username,
        Password = password,
        ClusterSizeInNodes = clustersize
    };


## <a name="next-steps"></a>Nächste Schritte

- [Apache ZooKeeper](http://zookeeper.apache.org/ )
- [Herstellen einer Verbindung HDInsight Cluster mit RDP mit](hdinsight-administer-use-management-portal.md#rdp)
- [HDInsight .NET SDK verwenden](hdinsight-provision-clusters.md#sdk)

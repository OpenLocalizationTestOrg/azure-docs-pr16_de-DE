<properties
    pageTitle="Hadoop, HBase, Sturm oder Spark Cluster unter Linux in mithilfe der PowerShell Azure HDInsight erstellen | Microsoft Azure"
    description="Erfahren Sie, wie Sie mithilfe der PowerShell Azure Hadoop, HBase, Sturm oder Spark Cluster unter Linux für HDInsight erstellen."
    services="hdinsight"
    documentationCenter=""
    authors="nitinme"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="10/05/2016"
    ms.author="nitinme"/>

# <a name="create-linux-based-clusters-in-hdinsight-by-using-azure-powershell"></a>Erstellen von Linux-basierten Cluster in HDInsight mithilfe von Azure PowerShell

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Azure PowerShell handelt es sich um eine leistungsfähige Skriptingtools-Umgebung, die Sie zum Steuern und Automatisierung der Bereitstellung und Verwaltung von Ihrer Auslastung in Microsoft Azure verwenden können. Dieses Dokument enthält Informationen zur Verwendung von einem Cluster Linux-basierten HDInsight mithilfe der PowerShell Azure erstellen. Darüber hinaus eine Beispielskript.

> [AZURE.NOTE] Azure PowerShell steht nur auf Windows-Clients. Wenn Sie einen Linux, Unix oder Mac OS X-Client verwenden, finden Sie unter [mit CLI Azure HDInsight Linux-basierten Cluster erstellen](hdinsight-hadoop-create-linux-clusters-azure-cli.md) Informationen zur Verwendung von CLI Azure einen Cluster erstellen.

## <a name="prerequisites"></a>Erforderliche Komponenten
Benötigen Sie Folgendes, bevor Sie diese Schritte ausführen:

- Ein Azure-Abonnement. Finden Sie [kostenlose Testversion Azure abrufen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- Azure PowerShell.
    Weitere Informationen zur Verwendung von Azure PowerShell mit HDInsight finden Sie unter [Verwalten von HDInsight mithilfe der PowerShell](hdinsight-administer-use-powershell.md). Die Liste der HDInsight Windows PowerShell-Cmdlets finden Sie unter [HDInsight Cmdlet verweisen](https://msdn.microsoft.com/library/azure/dn858087.aspx).

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

### <a name="access-control-requirements"></a>Anforderungen für Access-Steuerelement

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-clusters"></a>Cluster erstellen

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Zum Erstellen eines Clusters HDInsight mithilfe von Azure PowerShell müssen Sie die folgenden Schritte ausführen:

- Erstellen einer Azure Ressourcengruppe
- Erstellen Sie ein Konto Azure-Speicher
- Erstellen eines Azure Blob-Containers
- Erstellen eines HDInsight Clusters

Die beiden wichtigsten Parameter, die Sie zum Erstellen von Linux Cluster festlegen müssen, sind diejenigen, die den Typ OS und die Benutzerdetails SSH angeben:

- Stellen Sie sicher, dass Sie den Parameter **- OSType** **Linux**angeben.
- Wenn SSH für remote Sitzungen auf die Cluster verwenden möchten, können Sie das Benutzerkennwort SSH oder den öffentlichen SSH-Schlüssel angeben. Wenn Sie sowohl das Benutzerkennwort SSH und den öffentlichen SSH-Schlüssel angeben, wird die Taste ignoriert. Wenn Sie die Taste SSH für remote Sitzungen verwenden möchten, müssen Sie ein leeres SSH Kennwort für eine Aufforderung angeben. Weitere Informationen zur Verwendung von SSH mit HDInsight finden Sie unter den folgenden Artikeln:

    * [Verwenden von SSH mit Linux-basierten Hadoop auf HDInsight von Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md)
    * [Verwenden von SSH mit Linux-basierten Hadoop auf HDInsight von Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

Das folgende Skript veranschaulicht, wie einen neuen Cluster zu erstellen:

    $token ="<SpecifyAnUniqueString>"

    $resourceGroupName = $token + "rg"      # Provide a Resource Group name
    $clusterName = $token
    $defaultStorageAccountName = $token + "store"   # Provide a Storage account name
    $defaultStorageContainerName = $token + "container"
    $location = "East US 2"     # Change the location if needed
    $clusterNodes = 1           # The number of nodes in the HDInsight cluster

    # Sign in to Azure
    Login-AzureRmAccount

    # Select the subscription to use if you have multiple subscriptions
    #$subscriptionID = "<SubscriptionName>"        # Provide your Subscription Name
    #Select-AzureRmSubscription -SubscriptionId $subscriptionID

    # Create an Azure Resource Group
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    # Create an Azure Storage account and container used as the default storage
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -StorageAccountName $defaultStorageAccountName `
        -Location $location `
        -Type Standard_LRS
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -Name $defaultStorageAccountName -ResourceGroupName $resourceGroupName)[0].Key1
    $destContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey
    New-AzureStorageContainer -Name $defaultStorageContainerName -Context $destContext

    # Create an HDInsight cluster
    $credentials = Get-Credential -Message "Enter Cluster user credentials" -UserName "admin"
    $sshCredentials = Get-Credential -Message "Enter SSH user credentials"

    # The location of the HDInsight cluster must be in the same data center as the Storage account.
    $location = Get-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -StorageAccountName $defaultStorageAccountName | %{$_.Location}

    New-AzureRmHDInsightCluster `
        -ClusterName $clusterName `
        -ResourceGroupName $resourceGroupName `
        -HttpCredential $credentials `
        -Location $location `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $defaultStorageContainerName  `
        -ClusterSizeInNodes $clusterNodes `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.4" `
        -SshCredential $sshCredentials

Die Werte, die Sie für **$clusterCredentials** angeben, werden die Hadoop Benutzerkonto für den Cluster verwendet. Verwenden Sie für die Verbindung mit dem Cluster dieses Konto ein.

Die Werte, die Sie für **$sshCredentials** angeben, werden zum Erstellen des Benutzers SSH für den Cluster verwendet. Verwenden Sie dieses Konto zum Starten einer remote SSH-Sitzung auf dem Cluster Aufträge und ausführen.

> [AZURE.IMPORTANT] In diesem Skript müssen Sie die Anzahl der Worker Knoten angeben, die in den Cluster enthalten sein sollen. Wenn Sie mehr als 32 Worker Knoten (entweder beim Erstellen des Cluster oder durch Skalierung Cluster nach der Erstellung) verwenden möchten, müssen Sie auch eine Knotengröße am mit mindestens 8 Kernen und 14 GB RAM angeben.
>
> Weitere Informationen zu Knoten Größen und anfallenden Kosten finden Sie unter [HDInsight Preise](https://azure.microsoft.com/pricing/details/hdinsight/).

Es kann bis zu 20 Minuten zum Erstellen eines Clusters dauern.

Im folgende Beispiel wird veranschaulicht, wie zum Hinzufügen eines zusätzlichen Speicher-Kontos:

    # Create another storage account used as additional storage account
    $additionalStorageAccountName = $token + "store2"
    New-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -StorageAccountName $additionalStorageAccountName -Location $location -Type Standard_LRS
    $additionalStorageAccountKey = (Get-AzureRmStorageAccountKey -Name $additionalStorageAccountName -ResourceGroupName $resourceGroupName)[0].Value

    $config = New-AzureRmHDInsightClusterConfig
    Add-AzureRmHDInsightStorage -Config $config -StorageAccountName "$additionalStorageAccountName.blob.core.windows.net" -StorageAccountKey $additionalStorageAccountKey

    # Create a new HDInsight cluster
    New-AzureRmHDInsightCluster `
        -ClusterName $clusterName `
        -ResourceGroupName $resourceGroupName `
        -HttpCredential $credentials `
        -Location $location `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $defaultStorageContainerName  `
        -ClusterSizeInNodes $clusterNodes `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.4" `
        -SshCredential $sshCredentials `
        -Config $config

## <a name="customize-clusters"></a>Anpassen der Cluster

- Finden Sie unter [Anpassen HDInsight Cluster Bootstrap verwenden](hdinsight-hadoop-customize-cluster-bootstrap.md#use-azure-powershell).
- Finden Sie unter [Anpassen von Windows-basierten HDInsight Cluster mithilfe der Aktion Skript](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).

## <a name="delete-the-cluster"></a>Löschen des Clusters

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Nächste Schritte

Jetzt, da Sie einen HDInsight Cluster erfolgreich erstellt haben, verwenden Sie die folgenden Ressourcen zum Arbeiten mit Ihren Cluster finden.

### <a name="hadoop-clusters"></a>Hadoop Cluster

* [Verwenden Sie die Struktur mit HDInsight](hdinsight-use-hive.md)
* [Schwein mit HDInsight verwenden](hdinsight-use-pig.md)
* [Verwenden von MapReduce mit HDInsight](hdinsight-use-mapreduce.md)

### <a name="hbase-clusters"></a>HBase Cluster

* [Erste Schritte mit HBase auf HDInsight](hdinsight-hbase-tutorial-get-started-linux.md)
* [Java-Anwendungen für HBase auf HDInsight entwickeln](hdinsight-hbase-build-java-maven-linux.md)

### <a name="storm-clusters"></a>Storm Cluster

* [Entwickeln Sie Java Topologien für Storm auf HDInsight](hdinsight-storm-develop-java-topology.md)
* [Verwenden Sie Python Komponenten in Storm auf HDInsight](hdinsight-storm-develop-python-topology.md)
* [Bereitstellen Sie und überwachen Sie Topologien mit Storm auf HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)

### <a name="spark-clusters"></a>Spark Cluster

* [Erstellen Sie eine eigenständige Anwendung Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Führen Sie Aufträge Remote auf einem Spark Cluster Livius verwenden](hdinsight-apache-spark-livy-rest-interface.md)
* [Spark mit BI: Ausführen interaktiven Datenanalyse mithilfe von Spark in HDInsight mit BI-Tools](hdinsight-apache-spark-use-bi-tools.md)
* [Spark mit maschinellen Schulung: verwenden Spark in HDInsight Lebensmittel Prüfungsergebnissen Vorhersagen](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Spark Streaming: Verwenden Sie Spark in HDInsight zum Erstellen von in Echtzeit streaming Clientanwendungen](hdinsight-apache-spark-eventhub-streaming.md)

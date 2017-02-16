<properties
    pageTitle="HDInsight Cluster mit Bootstrap anpassen | Microsoft Azure"
    description="Erfahren Sie, wie HDInsight Cluster mit Bootstrap anpassen."
    services="hdinsight"
    documentationCenter=""
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/02/2016"
    ms.author="jgao"/>

# <a name="customize-hdinsight-clusters-using-bootstrap"></a>Anpassen von HDInsight Cluster Bootstrap verwenden

Manchmal möchten die Konfigurationsdateien konfigurieren die enthalten:

- clusterIdentity.xml
- Core-site.xml
- Gateway.Xml
- Hbase-env.xml
- Hbase-site.xml
- Hdfs-site.xml
- Struktur-env.xml
- Struktur-site.xml
- Mapred-Website
- Oozie-site.xml
- Oozie-env.xml
- Storm-site.xml
- Tez-site.xml
- Webhcat-site.xml
- aus – site.xml

Zuordnungseinheiten können keine Änderungen aufgrund erneute Abbildung beibehalten. Weitere Informationen über das erneute Abbildung finden Sie unter [Rolle Instanz Neustart fällig zu Updates für OS](http://blogs.msdn.com/b/kwill/archive/2012/09/19/role-instance-restarts-due-to-os-upgrades.aspx). Wenn Sie die Änderungen über die Cluster gesamte Dauer beibehalten möchten, können Sie HDInsight Cluster Anpassung beim Erstellen der verwenden. Dies ist die empfohlene Methode zum Ändern der Konfiguration von einem Cluster und über diese Azure neu abbilden Neustart Neustart Ereignisse beibehalten werden. Damit die Dienste needn't neu gestartet werden, werden diese Änderungen Konfiguration vor dem Starten des Diensts angewendet. 

Es gibt 3 Methoden Bootstrap verwenden:

- Verwenden von Azure PowerShell

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]
    
- Verwenden von .NET SDK
- Ressourcenmanager Azure-Vorlage verwenden

Informationen zum Installieren von zusätzlicher Komponenten auf HDInsight Cluster während der Erstellungszeit finden Sie unter:

- [Anpassen von HDInsight Cluster mithilfe der Aktion "Skript" (Linux)](hdinsight-hadoop-customize-cluster-linux.md)
- [Anpassen von HDInsight Cluster mithilfe der Aktion "Skript" (Windows)](hdinsight-hadoop-customize-cluster.md)

## <a name="use-azure-powershell"></a>Verwenden von Azure PowerShell

Der folgende PowerShell-Code passt eine Struktur Konfiguration:

    # hive-site.xml configuration
    $hiveConfigValues = @{ "hive.metastore.client.socket.timeout"="90" }
    
    $config = New-AzureRmHDInsightClusterConfig `
        | Set-AzureRmHDInsightDefaultStorage `
            -StorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
            -StorageAccountKey $defaultStorageAccountKey `
        | Add-AzureRmHDInsightConfigValues `
            -HiveSite $hiveConfigValues 
    
    New-AzureRmHDInsightCluster `
        -ResourceGroupName $existingResourceGroupName `
        -ClusterName $clusterName `
        -Location $location `
        -ClusterSizeInNodes $clusterSizeInNodes `
        -ClusterType Hadoop `
        -OSType Windows `
        -Version "3.2" `
        -HttpCredential $httpCredential `
        -Config $config 

Eine vollständige arbeiten PowerShell-Skript finden Sie im [Anhang-A](#hdinsight-hadoop-customize-cluster-bootstrap.md/appx-a:-powershell-sample).

**Um die Änderung zu überprüfen:**

1. Melden Sie sich auf der [Azure-Portal](https://portal.azure.com)an.
2. Im linken Bereich klicken Sie auf **Durchsuchen**, und klicken Sie dann auf **HDInsight Cluster**.
3. Klicken Sie auf den soeben erstellten mithilfe des PowerShell-Skripts Cluster.
4. Klicken Sie auf **Dashboard** zwischen dem oberen Rand der Blade um die Ambari UI zu öffnen.
5. Klicken Sie im Menü links auf **Struktur** .
6. Klicken Sie auf **HiveServer2** aus **Zusammenfassung**.
7. Klicken Sie auf der Registerkarte **Konfigurationen** .
8. Klicken Sie im Menü links auf **Struktur** .
9. Klicken Sie auf die Registerkarte **Erweitert** .
10. Führen Sie einen Bildlauf nach unten, und blenden Sie dann die **Erweiterte Struktur-Website**.
11. Suchen Sie nach **hive.metastore.client.socket.timeout** im Abschnitt.

Einige weitere Beispiele auf andere Konfigurationsdateien anpassen:

    # hdfs-site.xml configuration
    $HdfsConfigValues = @{ "dfs.blocksize"="64m" } #default is 128MB in HDI 3.0 and 256MB in HDI 2.1

    # core-site.xml configuration
    $CoreConfigValues = @{ "ipc.client.connect.max.retries"="60" } #default 50

    # mapred-site.xml configuration
    $MapRedConfigValues = @{ "mapreduce.task.timeout"="1200000" } #default 600000

    # oozie-site.xml configuration
    $OozieConfigValues = @{ "oozie.service.coord.normal.default.timeout"="150" }  # default 120

Weitere Informationen finden Sie unter mit dem Titel [Anpassen HDInsight Cluster Erstellung](http://blogs.msdn.com/b/bigdatasupport/archive/2014/04/15/customizing-hdinsight-cluster-provisioning-via-powershell-and-net-sdk.aspx)Azim Uddins-Blog.

## <a name="use-net-sdk"></a>Verwenden von .NET SDK

Finden Sie unter [Erstellen von Linux-basierten Cluster in HDInsight mit dem .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md#use-bootstrap).

## <a name="use-resource-manager-template"></a>Verwenden von Ressourcenmanager Vorlage

Sie können Bootstrap Ressourcenmanager Vorlage verwenden:

    "configurations": {
        …
        "hive-site": {
            "hive.metastore.client.connect.retry.delay": "5",
            "hive.execution.engine": "mr",
            "hive.security.authorization.manager": "org.apache.hadoop.hive.ql.security.authorization.DefaultHiveAuthorizationProvider"
        }
    }


![Hdinsight Hadoop anpassen Cluster bootstrap Azure Ressource-Manager-Vorlage](./media/hdinsight-hadoop-customize-cluster-bootstrap/hdinsight-customize-cluster-bootstrap-arm.png)



## <a name="see-also"></a>Siehe auch

- [Erstellen von Hadoop Cluster in HDInsight] [ hdinsight-provision-cluster] beschreibt, wie einen Cluster HDInsight zu erstellen, indem Sie mit anderen benutzerdefinierten Optionen.
- [Entwickeln Sie die Aktion Skript Skripts für HDInsight][hdinsight-write-script]
- [Installieren und Verwenden von Spark auf HDInsight Cluster][hdinsight-install-spark]
- [Installieren und Verwenden von R auf HDInsight Cluster][hdinsight-install-r]
- [Installieren und verwenden Sie Cluster Solr auf HDInsight](hdinsight-hadoop-solr-install.md).
- [Installieren und verwenden Sie Cluster Giraph auf HDInsight](hdinsight-hadoop-giraph-install.md).

[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-write-script]: hdinsight-hadoop-script-actions.md
[hdinsight-provision-cluster]: hdinsight-provision-clusters.md
[powershell-install-configure]: powershell-install-configure.md


[img-hdi-cluster-states]: ./media/hdinsight-hadoop-customize-cluster/HDI-Cluster-state.png "Während der Clustererstellung Phasen"

## <a name="appx-a-powershell-sample"></a>Beispiel A:-AppX PowerShell

Dieses PowerShell-Skript einen HDInsight Cluster erstellt und angepasst hat eine Struktur Einstellung:

    ####################################
    # Set these variables
    ####################################
    #region - used for creating Azure service names
    $nameToken = "<ENTER AN ALIAS>" 
    #endregion

    #region - cluster user accounts
    $httpUserName = "admin"  #HDInsight cluster username
    $httpPassword = "<ENTER A PASSWORD>" #"<Enter a Password>"

    $sshUserName = "sshuser" #HDInsight ssh user name
    $sshPassword = "<ENTER A PASSWORD>" #"<Enter a Password>"
    #endregion

    ####################################
    # Service names and varialbes
    ####################################
    #region - service names
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

    $resourceGroupName = $namePrefix + "rg"
    $hdinsightClusterName = $namePrefix + "hdi"
    $defaultStorageAccountName = $namePrefix + "store"
    $defaultBlobContainerName = $hdinsightClusterName

    $location = "East US 2"
    #endregion

    # Treat all errors as terminating
    $ErrorActionPreference = "Stop"

    ####################################
    # Connect to Azure
    ####################################
    #region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Login-AzureRmAccount}
    #endregion

    #region - Create an HDInsight cluster
    ####################################
    # Create dependent components
    ####################################
    Write-Host "Creating a resource group ..." -ForegroundColor Green
    New-AzureRmResourceGroup `
        -Name  $resourceGroupName `
        -Location $location

    Write-Host "Creating the default storage account and default blob container ..."  -ForegroundColor Green
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Location $location `
        -Type Standard_GRS

    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageContext = New-AzureStorageContext `
                                    -StorageAccountName $defaultStorageAccountName `
                                    -StorageAccountKey $defaultStorageAccountKey
    New-AzureStorageContainer `
        -Name $defaultBlobContainerName `
        -Context $defaultStorageContext #use the cluster name as the container name

    ####################################
    # Create a configuration object
    ####################################
    $hiveConfigValues = @{ "hive.metastore.client.socket.timeout"="90" }
        
    $config = New-AzureRmHDInsightClusterConfig `
        | Set-AzureRmHDInsightDefaultStorage `
            -StorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
            -StorageAccountKey $defaultStorageAccountKey `
        | Add-AzureRmHDInsightConfigValues `
            -HiveSite $hiveConfigValues 

    ####################################
    # Create an HDInsight cluster
    ####################################
    $httpPW = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
    $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$httpPW)

    $sshPW = ConvertTo-SecureString -String $sshPassword -AsPlainText -Force
    $sshCredential = New-Object System.Management.Automation.PSCredential($sshUserName,$sshPW)

    New-AzureRmHDInsightCluster `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -Location $location `
        -ClusterSizeInNodes 1 `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.2" `
        -HttpCredential $httpCredential `
        -SshCredential $sshCredential `
        -Config $config

    ####################################
    # Verify the cluster
    ####################################
    Get-AzureRmHDInsightCluster -ClusterName $hdinsightClusterName

    #endregion

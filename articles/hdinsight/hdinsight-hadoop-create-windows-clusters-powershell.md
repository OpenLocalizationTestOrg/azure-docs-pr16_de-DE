<properties
   pageTitle="Erstellen von Windows-basiertem Hadoop Cluster in mithilfe der PowerShell Azure HDInsight | Microsoft Azure"
    description="Erfahren Sie, wie Sie Cluster für Azure HDInsight mithilfe von Azure PowerShell zu erstellen."
   services="hdinsight"
   documentationCenter=""
   tags="azure-portal"
   authors="mumian"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="08/10/2016"
   ms.author="jgao"/>

# <a name="create-windows-based-hadoop-clusters-in-hdinsight-using-azure-powershell"></a>Erstellen von Windows-basiertem Hadoop Cluster in mithilfe der PowerShell Azure HDInsight

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Erfahren Sie, wie Sie mithilfe der PowerShell Azure HDInsight-Cluster erstellen. Azure PowerShell ist ein Modul, Cmdlets zum Verwalten von Azure mit Windows PowerShell bereitstellt. Für die Clustererstellung von anderen Tools und Features klicken Sie auf der Registerkarte wählen Sie am oberen Rand dieser Seite oder finden Sie unter [Methoden zur Erstellung](hdinsight-provision-clusters.md#cluster-creation-methods).


##<a name="prerequisites"></a>Voraussetzungen für:

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Bevor Sie die Anweisungen in diesem Artikel beginnen, müssen Sie die folgenden verfügen:

- Azure-Abonnement. Finden Sie [kostenlose Testversion Azure abrufen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Azure PowerShell.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

### <a name="access-control-requirements"></a>Anforderungen für Access-Steuerelement

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-clusters"></a>Cluster erstellen
Azure PowerShell handelt es sich um eine leistungsfähige Skriptingtools-Umgebung, die Sie zum Steuern und Automatisierung der Bereitstellung und Verwaltung von Ihrer Auslastung in Azure verwenden können. Dieser Abschnitt enthält Anweisungen zum einen Cluster HDInsight mithilfe von PowerShell Azure erstellen. Informationen zum Konfigurieren einer Arbeitsstationen zum Ausführen von HDInsight Windows PowerShell-Cmdlets finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md). Weitere Informationen zum Verwenden von Azure PowerShell mit HDInsight finden Sie unter [Verwalten von HDInsight mithilfe der PowerShell](hdinsight-administer-use-powershell.md). Die Liste der HDInsight Windows PowerShell-Cmdlets finden Sie unter [HDInsight Cmdlet verweisen](https://msdn.microsoft.com/library/azure/dn858087.aspx).


Die folgenden Verfahren sind erforderlich, einen Cluster HDInsight mithilfe von PowerShell Azure erstellt:

    ####################################
    # Set these variables
    ####################################
    #region - used for creating Azure service names
    $nameToken = "<Enter an Alias>" 
    #endregion

    #region - cluster user accounts
    $httpUserName = "admin"  #HDInsight cluster username
    $httpPassword = "<Enter a Password>"
    #endregion

    ###########################################
    # Service names and varialbes
    ###########################################
    #region - service names
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

    $resourceGroupName = $namePrefix + "rg"
    $hdinsightClusterName = $namePrefix + "hdi"
    $defaultStorageAccountName = $namePrefix + "store"
    $defaultBlobContainerName = $hdinsightClusterName

    $location = "East US 2"
    $clusterSizeInNodes = 1
    #endregion

    # Treat all errors as terminating
    $ErrorActionPreference = "Stop"

    ###########################################
    # Connect to Azure
    ###########################################
    #region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Login-AzureRmAccount}
    #endregion

    ###########################################
    # Create the resource group
    ###########################################
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    ###########################################
    # Preapre default storage account and container
    ###########################################
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Type Standard_GRS `
        -Location $location

    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageContext = New-AzureStorageContext `
                                    -StorageAccountName $defaultStorageAccountName `
                                    -StorageAccountKey $defaultStorageAccountKey
    New-AzureStorageContainer `
        -Name $hdinsightClusterName -Context $defaultStorageContext 

    ###########################################
    # Create the cluster
    ###########################################

    $httpPW = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
    $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$httpPW)

    New-AzureRmHDInsightCluster `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -Location $location `
        -ClusterSizeInNodes $clusterSizeInNodes `
        -ClusterType Hadoop `
        -OSType Windows `
        -Version "3.2" `
        -HttpCredential $httpCredential `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $hdinsightClusterName 

    ####################################
    # Verify the cluster
    ####################################
    Get-AzureRmHDInsightCluster -ClusterName $hdinsightClusterName 

## <a name="create-clusters-using-arm-template"></a>Erstellen Sie Cluster mit Cloud-Vorlage

Azure PowerShell können Sie eine Vorlage Cloud bereitstellen, die einen HDInsight Cluster erstellt.  [Rufen Sie mithilfe der PowerShell Azure Vorlagen](hdinsight-hadoop-create-windows-clusters-arm-templates.md#call-templates-using-powershell)finden Sie unter.

##<a name="customize-clusters"></a>Anpassen der Cluster

- Finden Sie unter [Anpassen HDInsight Cluster Bootstrap verwenden](hdinsight-hadoop-customize-cluster-bootstrap.md#use-azure-powershell).
- Finden Sie unter [Anpassen von Windows-basierten HDInsight Cluster mithilfe der Aktion Skript](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).


##<a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie mehrere Methoden zum Erstellen eines HDInsight Clusters erhalten. Weitere Informationen finden Sie unter den folgenden Artikeln:

* [Erste Schritte mit Azure HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md) – Informationen zum Arbeiten mit Ihren Cluster HDInsight starten
* [Hadoop übermitteln Aufträge programmgesteuert](hdinsight-submit-hadoop-jobs-programmatically.md) - erfahren Sie, wie programmgesteuert Aufträge mit HDInsight senden
* [Verwalten von Hadoop Cluster in HDInsight mithilfe der PowerShell](hdinsight-administer-use-powershell.md) - erfahren Sie, wie mit entwickelt HDInsight mithilfe von Azure PowerShell
* [Azure HDInsight SDK-Dokumentation]  [ hdinsight-sdk-documentation] -HDInsight SDK ermitteln




[hdinsight-sdk-documentation]: http://msdn.microsoft.com/library/dn479185.aspx
[azure-preview-portal]: https://manage.windowsazure.com
[connectionmanager]: http://msdn.microsoft.com/library/mt146773(v=sql.120).aspx
[ssispack]: http://msdn.microsoft.com/library/mt146770(v=sql.120).aspx
[ssisclustercreate]: http://msdn.microsoft.com/library/mt146774(v=sql.120).aspx
[ssisclusterdelete]: http://msdn.microsoft.com/library/mt146778(v=sql.120).aspx

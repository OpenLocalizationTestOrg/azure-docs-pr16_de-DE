<properties
    pageTitle="HDInsight Cluster mit Skript-Aktionen anpassen | Microsoft Azure"
    description="Erfahren Sie, wie HDInsight Cluster mithilfe der Aktion Skript anpassen."
    services="hdinsight"
    documentationCenter=""
    authors="nitinme"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="nitinme"/>

# <a name="customize-windows-based-hdinsight-clusters-using-script-action"></a>Anpassen von Windows-basiertem HDInsight Cluster mithilfe der Aktion Skript

**Aktion Skript** kann verwendet werden, um [benutzerdefinierte Skripts](hdinsight-hadoop-script-actions.md) während der Erstellungsprozess Cluster für die Installation von zusätzliche Software auf einem Cluster aufzurufen.

Die Informationen in diesem Artikel sind speziell für Windows-basiertem HDInsight Cluster. Linux-basierten Cluster finden Sie unter [Anpassen Linux-basierten HDInsight Cluster mithilfe der Aktion Skript](hdinsight-hadoop-customize-cluster-linux.md). 

HDInsight Cluster in einer Vielzahl von auch andere Methoden angepasst werden können, z. B., einschließlich zusätzliche Speicher Azure-Konten, ändern die Hadoop Konfiguration Dateien (Core-site.xml, Struktur-site.xml usw.) oder Hinzufügen von Bibliotheken (z. B. Struktur, Oozie) in gängige Speicherorte im Cluster freigegeben. Diese Anpassungen können über Azure PowerShell, Azure HDInsight .NET SDK oder das Azure-Portal erfolgen. Weitere Informationen finden Sie unter [Erstellen von Hadoop Cluster in HDInsight][hdinsight-provision-cluster].

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell-cli-and-dotnet-sdk.md)]

## <a name="script-action-in-the-cluster-creation-process"></a>Skript für Aktion in den Erstellungsprozess cluster

Skriptaktion ist nur verwendet werden, während eine Cluster ist gerade erstellt wird. Das folgende Diagramm veranschaulicht, wenn beim Erstellen der Skriptaktion ausgeführt wird:

![HDInsight Cluster Anpassung und Phasen während der Clustererstellung][img-hdi-cluster-states]

Wenn das Skript ausgeführt wird, gibt der Cluster für die **ClusterCustomization** Phase aus. In dieser Phase das Skript ausgeführt wird, klicken Sie unter dem Administratorkonto des Systems parallel auf allen angegebenen Knoten im Cluster, sowie die vollständigen Administratorberechtigungen auf den Knoten.

> [AZURE.NOTE] Da Sie admininistratorberechtigungen auf den Cluster-Knoten in welcher Phase **ClusterCustomization** verfügen, können Sie das Skript, zum Ausführen von Vorgängen wie Dienste einschließlich Hadoop-bezogene Dienste starten und beenden. Das Skript, daher müssen Sie sicherstellen, dass die Ambari und andere Dienste Hadoop-bezogene ausgeführt werden, bevor das Skript abgeschlossen ist. Diese Dienste sind erforderlich, um die Gesundheit und den Status der Cluster erfolgreich zu überprüfen, während sie erstellt wird. Wenn Sie eine beliebige Konfiguration auf dem Cluster ändern, die folgenden Dienste wirkt sich auf, müssen Sie die Helper-Funktionen verwenden, die zur Verfügung gestellt werden. Weitere Informationen zu Helper Funktionen finden Sie unter [entwickeln Skriptaktion Skripts für HDInsight][hdinsight-write-script].

Die Ausgabe und die Fehlerprotokolle für das Skript werden im Speicher Standardkonto gespeichert, die Sie für den Cluster angegeben haben. Die Protokolle befinden sich in einer Tabelle mit dem Namen **u < \cluster-name-fragment >< \time-stamp > Setuplog**. Dies sind die aggregate Protokolle aus dem Skript auf allen Knoten (am und Worker Knoten) im Cluster ausführen.

Jeder Cluster kann mehrere Skriptaktionen akzeptieren, die in der Reihenfolge aufgerufen werden, in denen sie angegeben sind. Ein Skript kann für den am Knoten, die Knoten Worker oder beides ausgeführt werden.

HDInsight bietet mehrere Skripts, um die folgenden Komponenten auf HDInsight Cluster zu installieren:

Namen | Skript
----- | -----
**Installieren von Spark** | https://hdiconfigactions.BLOB.Core.Windows.NET/sparkconfigactionv03/Spark-Installer-v03.ps1. [Installieren und verwenden Sie aufzeigen auf HDInsight Cluster]finden Sie unter[hdinsight-install-spark].
**Installieren von R** | https://hdiconfigactions.BLOB.Core.Windows.NET/rconfigactionv02/r-Installer-v02.ps1. Finden Sie unter [Installieren und Verwenden von R auf HDInsight Cluster][hdinsight-install-r].
**Installieren von Solr** | https://hdiconfigactions.BLOB.Core.Windows.NET/solrconfigactionv01/solr-Installer-v01.ps1. [Installieren und verwenden Sie Cluster Solr auf HDInsight](hdinsight-hadoop-solr-install.md)finden Sie unter.
- **Installieren von Giraph** | https://hdiconfigactions.BLOB.Core.Windows.NET/giraphconfigactionv01/giraph-Installer-v01.ps1. [Installieren und verwenden Sie Cluster Giraph auf HDInsight](hdinsight-hadoop-giraph-install.md)finden Sie unter.
| **Laden Sie vordefinierte Struktur Bibliotheken** | https://hdiconfigactions.BLOB.Core.Windows.NET/setupcustomhivelibsv01/Setup-customhivelibs-v01.ps1. Finden Sie unter [Hinzufügen von Struktur Bibliotheken auf HDInsight Cluster](hdinsight-hadoop-add-hive-libraries.md) |


## <a name="call-scripts-using-the-azure-portal"></a>Rufen Sie mithilfe der Azure-Portal Skripts

**Vom Azure-Portal**

1. Starten Sie einen Cluster erstellen, wie bei [Erstellen Hadoop Cluster in HDInsight](hdinsight-provision-clusters.md#portal)beschrieben.
2. Klicken Sie unter optionale Konfiguration für das Blade **Skript-Aktionen** auf **Skriptaktion hinzufügen** , um die Details zu der Skriptaktion angeben wie unten dargestellt:

    ![Verwenden Sie Skript für Aktion in einem Cluster anpassen] (./media/hdinsight-hadoop-customize-cluster/HDI.CreateCluster.8.png "Verwenden Sie Skript für Aktion in einem Cluster anpassen")

    <table border='1'>
        <tr><th>Eigenschaft</th><th>Wert</th></tr>
        <tr><td>Namen</td>
            <td>Geben Sie einen Namen für die Skriptaktion ein.</td></tr>
        <tr><td>URI-Skript</td>
            <td>Geben Sie den URI an das Skript, das zum Anpassen des Clusters aufgerufen wird. s</td></tr>
        <tr><td>Kopf/Worker</td>
            <td>Geben Sie die Knoten (**Kopf** oder **Worker**), auf dem das Skript Anpassung ausgeführt wird. </b>.
        <tr><td>Parameter</td>
            <td>Geben Sie den Parameter aus, wenn das Skript erforderlich.</td></tr>
    </table>

    Drücken Sie EINGABETASTE, um mehrere Skriptaktion zum Installieren von mehreren Komponenten auf dem Cluster hinzuzufügen.

3. Klicken Sie auf, **Wählen Sie** zum Speichern der Skript Aktion-Konfigurations, und fahren Sie mit der Clustererstellung.

## <a name="call-scripts-using-azure-powershell"></a>Rufen Sie mithilfe der PowerShell Azure Skripts

Diese folgende PowerShell-Skript veranschaulicht, wie Spark auf Windows basierend HDInsight Cluster installieren.  

    # Provide values for these variables
    $subscriptionID = "<Azure Suscription ID>" # After "Login-AzureRmAccount", use "Get-AzureRmSubscription" to list IDs.

    $nameToken = "<Enter A Name Token>"  # The token is use to create Azure service names.
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")
    
    $resourceGroupName = $namePrefix + "rg"
    $location = "EAST US 2" # used for creating resource group, storage account, and HDInsight cluster.
    
    $hdinsightClusterName = $namePrefix + "spark"
    $httpUserName = "admin"
    $httpPassword = "<Enter a Password>"
    
    $defaultStorageAccountName = "$namePrefix" + "store"
    $defaultBlobContainerName = $hdinsightClusterName
    
    #############################################################
    # Connect to Azure
    #############################################################
    
    Try{
        Get-AzureRmSubscription
    }
    Catch{
        Login-AzureRmAccount
    }
    Select-AzureRmSubscription -SubscriptionId $subscriptionID
    
    #############################################################
    # Prepare the dependent components
    #############################################################
    
    # Create resource group
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    
    # Create storage account
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Location $location `
        -Type Standard_GRS
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext `
                                    -StorageAccountName $defaultStorageAccountName `
                                    -StorageAccountKey $storageAccountKey  
    New-AzureStorageContainer `
        -Name $defaultBlobContainerName `
        -Context $defaultStorageAccountContext
    
    #############################################################
    # Create cluster with ApacheSpark
    #############################################################
    
    # Specify the configuration options
    $config = New-AzureRmHDInsightClusterConfig `
                -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
                -DefaultStorageAccountKey $defaultStorageAccountKey 
                
    
    # Add a script action to the cluster configuration
    $config = Add-AzureRmHDInsightScriptAction `
                -Config $config `
                -Name "Install Spark" `
                -NodeType HeadNode `
                -Uri https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1 `
    
    # Start creating a cluster with Spark installed
    New-AzureRmHDInsightCluster `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $hdinsightClusterName `
            -Location $location `
            -ClusterSizeInNodes 2 `
            -ClusterType Hadoop `
            -OSType Windows `
            -DefaultStorageContainer $defaultBlobContainerName `
            -Config $config


Um weitere Software installieren zu können, müssen Sie die Skriptdatei in das Skript zu ersetzen:


Wenn Sie dazu aufgefordert werden, geben Sie die Anmeldeinformationen für den Cluster aus. Es kann mehrere Minuten dauern, bevor der Cluster erstellt wird.

## <a name="call-scripts-using-net-sdk"></a>Rufen Sie die Skripts mit .NET SDK 

Im folgende Beispiel wird veranschaulicht, wie Spark auf Windows basierend HDInsight Cluster installiert. Um weitere Software installieren zu können, müssen Sie die Skriptdatei in den Code zu ersetzen.

**So erstellen Sie einen HDInsight Cluster mit Spark** 

1. Erstellen Sie eine c# in Visual Studio.
2. Führen Sie die Nuget-Paket-Manager-Konsole den folgenden Befehl ein.

        Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Pre
        Install-Package Microsoft.Azure.Management.ResourceManager -Pre
        Install-Package Microsoft.Azure.Management.HDInsight

2. Verwenden Sie die folgenden Anweisungen in der Datei Program.cs verwenden:

        using System;
        using System.Security;
        using Microsoft.Azure;
        using Microsoft.Azure.Management.HDInsight;
        using Microsoft.Azure.Management.HDInsight.Models;
        using Microsoft.Azure.Management.ResourceManager;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Rest;
        using Microsoft.Rest.Azure.Authentication;

3. Platzieren Sie den Code in der Klasse durch Folgendes ein:

        private static HDInsightManagementClient _hdiManagementClient;

        // Replace with your AAD tenant ID if necessary
        private const string TenantId = UserTokenProvider.CommonTenantId; 
        private const string SubscriptionId = "<Your Azure Subscription ID>";
        // This is the GUID for the PowerShell client. Used for interactive logins in this example.
        private const string ClientId = "1950a258-227b-4e31-a9cf-717495945fc2";
        private const string ResourceGroupName = "<ExistingAzureResourceGroupName>";
        private const string NewClusterName = "<NewAzureHDInsightClusterName>";
        private const int NewClusterNumWorkerNodes = 2;
        private const string NewClusterLocation = "East US";
        private const string NewClusterVersion = "3.2";
        private const string ExistingStorageName = "<ExistingAzureStorageAccountName>";
        private const string ExistingStorageKey = "<ExistingAzureStorageAccountKey>";
        private const string ExistingContainer = "<ExistingAzureBlobStorageContainer>";
        private const string NewClusterType = "Hadoop";
        private const OSType NewClusterOSType = OSType.Windows;
        private const string NewClusterUsername = "<HttpUserName>";
        private const string NewClusterPassword = "<HttpUserPassword>";

        static void Main(string[] args)
        {
            System.Console.WriteLine("Running");

            // Authenticate and get a token
            var authToken = Authenticate(TenantId, ClientId, SubscriptionId);
            // Flag subscription for HDInsight, if it isn't already.
            EnableHDInsight(authToken);
            // Get an HDInsight management client
            _hdiManagementClient = new HDInsightManagementClient(authToken);

            CreateCluster();
        }

        private static void CreateCluster()
        {
            var parameters = new ClusterCreateParameters
            {
                ClusterSizeInNodes = NewClusterNumWorkerNodes,
                Location = NewClusterLocation,
                ClusterType = NewClusterType,
                OSType = NewClusterOSType,
                Version = NewClusterVersion,

                DefaultStorageAccountName = ExistingStorageName,
                DefaultStorageAccountKey = ExistingStorageKey,
                DefaultStorageContainer = ExistingContainer,

                UserName = NewClusterUsername,
                Password = NewClusterPassword,
            };

            ScriptAction sparkScriptAction = new ScriptAction("Install Spark",
                new Uri("https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1"), "");

            parameters.ScriptActions.Add(ClusterNodeType.HeadNode, new System.Collections.Generic.List<ScriptAction> { sparkScriptAction });
            parameters.ScriptActions.Add(ClusterNodeType.WorkerNode, new System.Collections.Generic.List<ScriptAction> { sparkScriptAction });

            _hdiManagementClient.Clusters.Create(ResourceGroupName, NewClusterName, parameters);
        }

        /// <summary>
        /// Authenticate to an Azure subscription and retrieve an authentication token
        /// </summary>
        /// <param name="TenantId">The AAD tenant ID</param>
        /// <param name="ClientId">The AAD client ID</param>
        /// <param name="SubscriptionId">The Azure subscription ID</param>
        /// <returns></returns>
        static TokenCloudCredentials Authenticate(string TenantId, string ClientId, string SubscriptionId)
        {
            var authContext = new AuthenticationContext("https://login.microsoftonline.com/" + TenantId);
            var tokenAuthResult = authContext.AcquireToken("https://management.core.windows.net/", 
                ClientId, 
                new Uri("urn:ietf:wg:oauth:2.0:oob"), 
                PromptBehavior.Always, 
                UserIdentifier.AnyUser);
            return new TokenCloudCredentials(SubscriptionId, tokenAuthResult.AccessToken);
        }
        /// <summary>
        /// Marks your subscription as one that can use HDInsight, if it has not already been marked as such.
        /// </summary>
        /// <remarks>This is essentially a one-time action; if you have already done something with HDInsight
        /// on your subscription, then this isn't needed at all and will do nothing.</remarks>
        /// <param name="authToken">An authentication token for your Azure subscription</param>
        static void EnableHDInsight(TokenCloudCredentials authToken)
        {
            // Create a client for the Resource manager and set the subscription ID
            var resourceManagementClient = new ResourceManagementClient(new TokenCredentials(authToken.Token));
            resourceManagementClient.SubscriptionId = SubscriptionId;
            // Register the HDInsight provider
            var rpResult = resourceManagementClient.Providers.Register("Microsoft.HDInsight");
        }


4. Drücken Sie **F5** , um die Anwendung ausführen.


## <a name="support-for-open-source-software-used-on-hdinsight-clusters"></a>Unterstützung für Open Source-Software auf HDInsight Cluster verwendet
Der Microsoft Azure HDInsight-Dienst ist eine flexible Plattform, die Sie big Data Applications in der Cloud mithilfe einer Netz von Open Source-Technologien um Hadoop gebildeten erstellen kann. Microsoft Azure bietet eine allgemeine Grad der Unterstützung für Open Source-Technologien wie im Abschnitt **Unterstützen Bereich** der <a href="http://azure.microsoft.com/support/faq/" target="_blank">Azure unterstützen häufig gestellte Fragen zur Website</a>erläutert. Der Dienst HDInsight bietet eine zusätzliche Grad der Unterstützung für einige Komponenten, wie unten beschrieben.

Es gibt zwei Arten von Open Source-Komponenten, die im Dienst HDInsight verfügbar sind:

- **Integrierte Komponenten** – diese Komponenten auf HDInsight Cluster vorinstalliert werden und Kernfunktionalität der Cluster bereitstellen. Beispielsweise werden aus Ressourcen-Manager die Struktur-Abfragesprache (HiveQL) und der Mahout-Bibliothek zu dieser Kategorie gehören. Eine vollständige Liste der Clusterkomponenten steht in [Neuigkeiten in die Hadoop Cluster Versionen von HDInsight bereitgestellten?](hdinsight-component-versioning.md) </a>.
- **Benutzerdefinierte Komponenten** -, die Sie als Benutzer von der Cluster können installieren oder verwenden in Ihrer Arbeitsbelastung jede Komponente zur Verfügung, in der Community oder von Ihnen erstellten.

Integrierte Komponenten werden vollständig unterstützt, und hilft Microsoft Support zum Isolieren und Beheben von Problemen im Zusammenhang mit dieser Komponenten.

> [AZURE.WARNING] Komponenten, die mit dem HDInsight Cluster bereitgestellt werden vollständig unterstützt, und hilft Microsoft Support zum Isolieren und Beheben von Problemen im Zusammenhang mit dieser Komponenten.
>
> Benutzerdefinierte Komponenten empfangen professionell angemessenen Support, um Sie dabei unterstützen, das Problem zu beheben. Dadurch kann das Problem zu beheben, oder werden Sie aufgefordert, die verfügbaren Kanäle für das open-Source-Technologien populärer, wo eingehender Erfahrung für diese Technologie gefunden wird. Angenommen, es gibt viele Communitywebsites, die, wie verwendet werden können: [MSDN-Forum für HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Apache Projekte außerdem Projektwebsites auf [http://apache.org](http://apache.org), zum Beispiel: [Hadoop](http://hadoop.apache.org/), [Spark](http://spark.apache.org/).

Der HDInsight-Dienst bietet verschiedene Methoden zur Verwendung von benutzerdefinierter Komponenten. Unabhängig davon, wie eine Komponente oder wird im Cluster installiert wird Sie die gleiche Ebene der Unterstützung angewendet. Es folgt eine Liste der am häufigsten verwendeten Methoden, benutzerdefinierte Komponenten auf HDInsight Cluster verwendet werden können:

1. Senden des Auftrags - Hadoop oder andere Arten von Aufträgen, die ausgeführt werden, oder verwenden Sie benutzerdefinierte Komponenten kann mit dem Cluster gesendet werden.
2. Cluster Anpassung – während der Clustererstellung können Sie angeben, zusätzliche Einstellungen und alle angepassten Komponenten, die auf den Clusterknoten installiert werden.
3. Beispiele – für häufig verwendete benutzerdefinierte Komponenten, Microsoft und andere vorsehen Beispiele, wie diese Komponenten auf die HDInsight Cluster verwendet werden können. In diesen Beispielen werden ohne Support bereitgestellt.

## <a name="develop-script-action-scripts"></a>Entwickeln Sie die Aktion Skript Skripts

[Skript für Aktion entwickeln Skripts für HDInsight]finden Sie unter[hdinsight-write-script].


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

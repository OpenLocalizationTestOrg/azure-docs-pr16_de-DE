<properties
    pageTitle="Verwenden von Hadoop Sqoop in HDInsight | Microsoft Azure"
    description="Erfahren Sie, wie Azure PowerShell von einem Computer ausführen Sqoop importieren und Exportieren von zwischen einem Hadoop-Cluster und einer SQL Azure-Datenbank."
    editor="cgronlun"
    manager="jhubbard"
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/02/2016"
    ms.author="jgao"/>

#<a name="use-sqoop-with-hadoop-in-hdinsight"></a>Verwenden von Sqoop mit Hadoop in HDInsight

[AZURE.INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Informationen Sie zum Verwenden von Sqoop in HDInsight importieren und Exportieren zwischen HDInsight Cluster und SQL Azure-Datenbank oder SQL Server-Datenbank.

Obwohl Hadoop eine gute Wahl für die Verarbeitung von unstrukturierten und semistructured Daten, z. B. Protokolle und Dateien, ist es auch muss möglicherweise eine strukturierte Daten zu verarbeiten, die in relationalen Datenbanken gespeichert sind.

[Sqoop] [ sqoop-user-guide-1.4.4] Datenübertragung zwischen Hadoop Cluster und relationalen Datenbanken optimal geeignet ist. Sie können zum Importieren von Daten aus einer relationalen Datenbank-System (RDBMS), z. B. SQL Server, MySQL oder Oracle in das Dateisystem Hadoop distributed (HDFS), Transformieren der Daten in Hadoop mit MapReduce oder Struktur, und klicken Sie dann die Daten wieder in einem RDBMS exportieren. In diesem Lernprogramm verwenden Sie eine SQL Server-Datenbank für die relationalen Datenbank.

Sqoop-Versionen, die auf HDInsight Cluster unterstützt werden, finden Sie unter [Neuigkeiten in den Cluster Versionen von HDInsight bereitgestellten?] [hdinsight-versions].

##<a name="understand-the-scenario"></a>Dieses Szenario verstehen

HDInsight Cluster im Lieferumfang von einige Beispieldaten. Verwenden Sie die folgenden zwei Beispiele:

- Eine Protokolldatei log4j, die sich unter */example/data/sample.log*befindet. Die folgenden Protokolle werden aus der Datei extrahiert:

        2012-02-03 18:35:34 SampleClass6 [INFO] everything normal for id 577725851
        2012-02-03 18:35:34 SampleClass4 [FATAL] system problem at id 1991281254
        2012-02-03 18:35:34 SampleClass3 [DEBUG] detail for id 1304807656
        ...

- Eine strukturtabelle mit dem Namen *Hivesampletable*, die in der Datendatei am */hive/warehouse/hivesampletable*verweist auf. Die Tabelle enthält einige Mobilgerät Daten. 

  	| Feld | Datentyp |
  	| ----- | --------- |
  	| ClientID | Zeichenfolge |
  	| querytime | Zeichenfolge |
  	| Markt | Zeichenfolge |
  	| deviceplatform | Zeichenfolge |
  	| devicemake | Zeichenfolge |
  	| devicemodel | Zeichenfolge |
  	| Bundesstaat | Zeichenfolge |
  	| Land | Zeichenfolge |
  	| querydwelltime | Doppelte |
  	| SessionID | bigint |
  	| sessionpagevieworder | bigint |

Sie in der SQL Azure-Datenbank oder SQL Server zuerst *sample.log* und *Hivesampletable* exportieren und importieren Sie die Tabelle mit den mobilen Gerätedaten zurück zur HDInsight mithilfe des folgenden Pfads:

    /tutorials/usesqoop/importeddata

## <a name="create-cluster-and-sql-database"></a>Erstellen von Cluster und SQL-Datenbank

In diesem Abschnitt wird gezeigt, wie einem Cluster und Schema der SQL-Datenbank zum Ausführen des Lernprogramms mithilfe der Azure-Portal und eine Azure Ressourcenmanager Vorlage erstellt.  Wenn Sie Azure PowerShell verwenden möchten lieber, finden Sie in [Anhang A](#appendix-a---a-powershell-sample).

1. Klicken Sie auf die folgende Abbildung, um eine Ressource-Manager Vorlage Azure-Portal zu öffnen.         

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fusesqoop%2Fcreate-linux-based-hadoop-cluster-in-hdinsight-and-sql-database.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>
    
    Die Ressource-Manager Vorlage befindet sich in einem öffentlichen Blob-Container *https://hditutorialdata.blob.core.windows.net/usesqoop/create-linux-based-hadoop-cluster-in-hdinsight-and-sql-database.json*. 
    
    Die Vorlage der Ressource-Manager Ruft ein Bacpac-Paket zum Bereitstellen von der Tabellenschemas mit SQL-Datenbank.  Das Paket Bacpac befindet sich auch in einem öffentlichen Blob-Container https://hditutorialdata.blob.core.windows.net/usesqoop/SqoopTutorial-2016-2-23-11-2.bacpac. Wenn Sie einen privaten Container für die Dateien Bacpac verwenden möchten, verwenden Sie die folgenden Werte in der Vorlage:
    
        "storageKeyType": "Primary",
        "storageKey": "<TheAzureStorageAccountKey>",
    
2. Geben Sie aus dem Parameter Blade Folgendes ein:

    - **ClusterName**: Geben Sie einen Namen für den Hadoop Cluster, die Sie erstellen.
    - **Cluster-Benutzernamen und Ihr Kennwort**: der Standard-Anmeldename ist Administrator.
    - **SSH-Benutzernamen und Ihr Kennwort ein**.
    - **SQL-Datenbank-Server-Benutzernamen und Ihr Kennwort ein**.

    Die folgenden Werte sind im Variablenabschnitt:
    
  	|Name des Standardkontos Speicher|<CluterName>Speichern|
  	|----------------------------|-----------------|
  	|Azure SQL-Datenbank-Servernamens|<ClusterName>DB-Server|
  	|Name der Azure SQL-Datenbank|<ClusterName>DB|
    
    Bitte notieren Sie sich diese Werte an.  Sie benötigen diese später im Lernprogramm.
    
3.Klicken klicken Sie auf **OK** , um die Parameter zu speichern.

4.konfigurieren aus dem **benutzerdefinierte Bereitstellung** -Blade klicken Sie auf die Dropdown-Feld **Ressourcengruppe** , und klicken Sie dann auf **neu** , um eine neue Ressourcengruppe erstellen. Die Ressourcengruppe ist ein Container, der Cluster, das Konto abhängige Speicherplatz und andere verknüpfte Ressource gruppiert.

5 ° Klicken Sie auf die **Vertragsbedingungen**, und klicken Sie dann auf **Erstellen**.

6. Klicken Sie auf **Erstellen**. Sehen Sie eine neue Kachel mit dem Titel Submitting Bereitstellung für die Bereitstellung der Vorlage ein. Es dauert zu ungefähr 20 Minuten der Cluster und SQL-Datenbank zu erstellen.

Wenn Sie vorhandene SQL Azure-Datenbank oder Microsoft SQL Server verwenden

- **SQL Azure-Datenbank**: Sie müssen eine Firewall-Regel für den Azure SQL-Datenbankserver an, damit von Ihrem Computer Zugriff konfigurieren. Für eine Anleitung zum Erstellen einer SQL Azure-Datenbank und Konfigurieren der Firewalls finden Sie unter [Erste Schritte mit SQL Azure-Datenbank][sqldatabase-get-started]. 

    > [AZURE.NOTE] Standardmäßig kann eine SQL Azure-Datenbank Verbindungen aus Azure Dienste, wie z. B. Azure HDInsight. Wenn diese Einstellung Firewall deaktiviert ist, müssen Sie aktiviert vom Azure-Portal. Anweisung zu eine SQL Azure-Datenbank erstellen und Konfigurieren der Firewall-Regeln, finden Sie unter [Erstellen und Konfigurieren der SQL-Datenbank][sqldatabase-create-configue].

- **SQL Server**: ist Ihre HDInsight Cluster im gleichen virtuellen Netzwerk in Azure als SQL Server, können Sie die Schritte in diesem Artikel verwenden, importieren und Exportieren von Daten in einer SQL Server-Datenbank.

    > [AZURE.NOTE] HDInsight unterstützt nur standortbasierte virtuelle Netzwerke, und es funktioniert derzeit nicht mit Zugehörigkeit Gruppe basierenden virtuellen Netzwerken.

    * Zum Erstellen und Konfigurieren eines virtuellen Netzwerks, finden Sie unter [Erstellen eines virtuellen Netzwerks Azure-Portal verwenden](../virtual-network/virtual-networks-create-vnet-arm-pportal.md).

        * Wenn Sie in Ihrem Datencenter SQL Server verwenden, müssen Sie das virtuelle Netzwerk als *zwischen Standorten* oder *Punkt-zu-Standort*konfigurieren.

            > [AZURE.NOTE] Für **Punkt-zu-Standort** virtuelle Netzwerke muss SQL Server den VPN-Client-konfigurationsanwendung aus, ausgeführt werden die aus dem **Dashboard** der Azure virtuelle Netzwerkkonfiguration zur Verfügung steht.

        * Wenn Sie auf eine Azure-virtuellen Computern SQL Server verwenden, kann jeder virtuelle Netzwerkkonfiguration verwendet werden, des virtuellen Computers als Host für SQL Server ist ein Mitglied der gleichen virtuellen Netzwerk als HDInsight.

    * Erstellen einen HDInsight Cluster in einem virtuellen Netzwerk finden Sie unter [Erstellen von Hadoop Cluster in HDInsight mit benutzerdefinierten Optionen](hdinsight-provision-clusters.md)

    > [AZURE.NOTE] SQL Server muss auch-Authentifizierung zulässig. Sie müssen SQL Server-Anmeldung verwenden, die Schritte in diesem Artikel ausführen.


## <a name="run-sqoop-jobs"></a>Führen Sie Sqoop Aufträge

HDInsight kann Sqoop Aufträge mithilfe verschiedener Methoden ausführen. Anhand der folgenden Tabelle entscheiden, welche Methode für Sie richtig ist, und klicken Sie dann auf den Link für eine exemplarische Vorgehensweise.

| **Verwenden Sie diese Option** , wenn Sie möchten...                                   | ... .ar **interaktiven** Shell | ... **Stapelverarbeitung** | ... .with dieses **Cluster-Betriebssystem** | ... .from dieses **Client-Betriebssystem** |
|:--------------------------------------------------------------|:---------------------------:|:-----------------------:|:------------------------------------------|:-----------------------------------------|
| [SSH](hdinsight-use-sqoop-mac-linux.md)                        |              ✔              |            ✔            | Linux                                     | Linux, Unix, Mac OS X oder Windows        |
| [.NET SDK für Hadoop](hdinsight-hadoop-use-sqoop-dotnet-sdk.md) |           &nbsp;            |            ✔            | Linux oder Windows                          | Windows (für jetzt)                        |
| [Azure PowerShell](hdinsight-hadoop-use-sqoop-powershell.md)  |           &nbsp;            |            ✔            | Linux oder Windows                          | Windows                                  |

##<a name="limitations"></a>Einschränkungen

* Massen Exportieren - mit Linux-basierten HDInsight, der Sqoop Verbinder verwendet, um Daten in Microsoft SQL Server oder SQL Azure-Datenbank exportieren unterstützt derzeit nicht Massen eingefügt.

* Batchverarbeitung von-mit Linux-basierten HDInsight bei Verwendung der `-batch` wechseln, wenn die Durchführung fügt, Sqoop führt mehrere fügt anstelle der Batchvorgängen einfügen.

##<a name="next-steps"></a>Nächste Schritte

Jetzt haben Sie gelernt Sqoop verwenden. Weitere Informationen finden Sie unter:

- [Verwenden Sie die Struktur mit HDInsight](hdinsight-use-hive.md)
- [Schwein mit HDInsight verwenden](hdinsight-use-pig.md)
- [Verwenden von Oozie mit HDInsight][hdinsight-use-oozie]: Verwenden Sie Sqoop Aktion in einem Workflow Oozie.
- [Analysieren Flugdaten Verzögerung mit HDInsight][hdinsight-analyze-flight-data]: verzögern Daten analysieren Flight Struktur verwenden, und verwenden Sie Sqoop, Daten in einer SQL Azure-Datenbank exportieren.
- [Hochladen von Daten mit HDInsight][hdinsight-upload-data]: Suchen nach anderen Methoden zum Hochladen von Daten in HDInsight/Azure Blob-Speicher.


## <a name="appendix-a---a-powershell-sample"></a>Anhang A – einer Stichprobe PowerShell

Im Beispiel PowerShell führt die folgenden Schritte aus:

1. Verbinden Sie mit Azure.
2. Erstellen einer Azure Ressourcengruppe. Weitere Informationen finden Sie unter [Verwenden von Azure PowerShell Azure Ressourcenmanager](../powershell-azure-resource-manager.md)
3. Erstellen einer Azure SQL-Datenbankserver, einer SQL Azure-Datenbank und zwei Tabellen. 

    Wenn Sie stattdessen SQL Server verwenden, verwenden Sie die folgenden Aussagen Tabellen zu erstellen:
    
        CREATE TABLE [dbo].[log4jlogs](
         [t1] [nvarchar](50),
         [t2] [nvarchar](50),
         [t3] [nvarchar](50),
         [t4] [nvarchar](50),
         [t5] [nvarchar](50),
         [t6] [nvarchar](50),
         [t7] [nvarchar](50))

        CREATE TABLE [dbo].[mobiledata](
         [clientid] [nvarchar](50),
         [querytime] [nvarchar](50),
         [market] [nvarchar](50),
         [deviceplatform] [nvarchar](50),
         [devicemake] [nvarchar](50),
         [devicemodel] [nvarchar](50),
         [state] [nvarchar](50),
         [country] [nvarchar](50),
         [querydwelltime] [float],
         [sessionid] [bigint],
         [sessionpagevieworder][bigint])

    Die einfachste Möglichkeit zum Untersuchen der Datenbank und Tabellen besteht darin, Visual Studio verwenden. Der Datenbankserver und die Datenbank können das Azure-Portal untersucht werden.

4. Erstellen eines HDInsight Clusters an.

    Zum Untersuchen des Clusters können Sie die Azure-Portal oder Azure PowerShell verwenden.

5. Vorab verarbeiten Sie die Quelldatei für die Daten ein.

    In diesem Lernprogramm werden Sie eine Protokolldatei log4j (einer Datei mit Trennzeichen) und eine strukturtabelle in einer SQL Azure-Datenbank exportieren. Die Datei mit Trennzeichen heißt */example/data/sample.log*. Zuvor im Lernprogramm haben Sie ein paar Beispiele für log4j Protokolle gesehen. In der Protokolldatei gibt es einige leeren Zeilen und einige Linien etwa wie folgt:
    
        java.lang.Exception: 2012-02-03 20:11:35 SampleClass2 [FATAL] unrecoverable system problem at id 609774657
            at com.osa.mocklogger.MockLogger$2.run(MockLogger.java:83)
    
    Dies ist für andere Beispiele, bei die diese Daten verwenden in Ordnung, aber wir müssen diese Ausnahmen entfernen, bevor Sie in der SQL Azure-Datenbank oder SQL Server importiert werden kann. Sqoop exportieren schlägt fehl, wenn es eine leere Zeichenfolge oder eine Linie mit einer kleineren ist Anzahl von Elementen als die Anzahl der Felder in der Tabelle der SQL Azure-Datenbank definiert. Die Tabelle log4jlogs weist 7 Zeichenfolgentyp Feldern.

    Dieses Verfahren erstellt eine neue Datei auf dem Cluster: tutorials/usesqoop/data/sample.log. Zum Untersuchen der geänderte Datendatei können Sie die Azure-Portal, ein Azure Storage Explorer-Tool oder Azure PowerShell verwenden. [Erste Schritte mit HDInsight] [ hdinsight-get-started] weist eine Stichprobe Code Azure PowerShell zum Herunterladen einer Datei, und zeigen Sie den Dateiinhalt verwenden.

6. Exportieren einer Datendatei in SQL Azure-Datenbank.

    Die Quelldatei ist tutorials/usesqoop/data/sample.log. Die Tabelle, in dem die Daten exportiert werden, wird als log4jlogs bezeichnet.
    
    > [AZURE.NOTE] Als Zeichenfolge Verbindungsinformationen sollte die Schritte in diesem Abschnitt für eine SQL Azure-Datenbank oder SQL Server funktionieren. Diese Schritte wurden mithilfe der folgenden Konfigurations getestet:
    >
    > * **Azure virtuelle Punkt-zu-Standort-Netzwerkkonfiguration**: ein virtuelles Netzwerk verbunden HDInsight Cluster zu einem SQL Server in einem privaten Datencenter. Weitere Informationen finden Sie unter [Konfigurieren eines Punkt-zu-Standort VPN im Verwaltungsportal](../vpn-gateway/vpn-gateway-point-to-site-create.md) .
    > * **Azure HDInsight 3.1**: finden Sie Informationen zum Erstellen eines Clusters in einem virtuellen Netzwerk [Erstellen Hadoop Cluster in HDInsight mit benutzerdefinierten Optionen](hdinsight-provision-clusters.md) .
    > * **SQL Server 2014**: konfiguriert Authentifizierung und Ausführen des VPN-Clients Paket für eine sichere Verbindung herstellen mit dem virtuellen Netzwerk zulassen.

7. Exportieren Sie eine strukturtabelle in der SQL Azure-Datenbank.

8. Importieren Sie die Tabelle Mobiledata zum HDInsight Cluster ein.

    Zum Untersuchen der geänderte Datendatei können Sie die Azure-Portal, ein Azure Storage Explorer-Tool oder Azure PowerShell verwenden.  [Erste Schritte mit HDInsight] [ hdinsight-get-started] eine Stichprobe Code zur Verwendung von Azure PowerShell zum Herunterladen einer Datei, und zeigen Sie den Dateiinhalt enthält.


### <a name="the-powershell-sample"></a>Der PowerShell Stichprobe

    # Prepare an Azure SQL database to be used by the Sqoop tutorial
    
    #region - provide the following values
    
    $subscriptionID = "<Enter your Azure Subscription ID>"
    
    $sqlDatabaseLogin = "<Enter a SQL Database Login name>" #SQL Database server login
    $sqlDatabasePassword = "<Enter a Password>"
    
    $httpUserName = "admin"  #HDInsight cluster username
    $httpPassword = "<Enter a Password>"
    
    # used for creating Azure service names
    $nameToken = "<Enter an alias>" 
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")
    #endregion
    
    #region - variables
    
    # Resource group variables
    $resourceGroupName = $namePrefix + "rg"
    $location = "East US 2" # used by all Azure services defined in this tutorial
    
    # SQL database varialbes
    $sqlDatabaseServerName = $namePrefix + "sqldbserver"
    $sqlDatabaseName = $namePrefix + "sqldb"
    $sqlDatabaseConnectionString = "Data Source=$sqlDatabaseServerName.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabasePassword;Encrypt=true;Trusted_Connection=false;"
    $sqlDatabaseMaxSizeGB = 10
    
    # Used for retrieving external IP address and creating firewall rules
    $ipAddressRestService = "http://bot.whatismyipaddress.com"
    $fireWallRuleName = "UseSqoop"
    
    # Used for creating tables and clustered indexes
    $cmdCreateLog4jTable = "CREATE TABLE [dbo].[log4jlogs](
        [t1] [nvarchar](50),
        [t2] [nvarchar](50),
        [t3] [nvarchar](50),
        [t4] [nvarchar](50),
        [t5] [nvarchar](50),
        [t6] [nvarchar](50),
        [t7] [nvarchar](50))"
    
    $cmdCreateLog4jClusteredIndex = "CREATE CLUSTERED INDEX log4jlogs_clustered_index on log4jlogs(t1)"
    
    $cmdCreateMobileTable = " CREATE TABLE [dbo].[mobiledata](
    [clientid] [nvarchar](50),
    [querytime] [nvarchar](50),
    [market] [nvarchar](50),
    [deviceplatform] [nvarchar](50),
    [devicemake] [nvarchar](50),
    [devicemodel] [nvarchar](50),
    [state] [nvarchar](50),
    [country] [nvarchar](50),
    [querydwelltime] [float],
    [sessionid] [bigint],
    [sessionpagevieworder][bigint])"
    
    $cmdCreateMobileDataClusteredIndex = "CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(clientid)"
    
    # HDInsight variables
    $hdinsightClusterName = $namePrefix + "hdi"
    $defaultStorageAccountName = $namePrefix + "store"
    $defaultBlobContainerName = $hdinsightClusterName
    #endregion
    
    # Treat all errors as terminating
    $ErrorActionPreference = "Stop"
    
    #region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Login-AzureRmAccount}
    #endregion
    
    #region - Create Azure resouce group
    Write-Host "`nCreating an Azure resource group ..." -ForegroundColor Green
    try{
        Get-AzureRmResourceGroup -Name $resourceGroupName
    }
    catch{
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    }
    #endregion
    
    #region - Create Azure SQL database server
    Write-Host "`nCreating an Azure SQL Database server ..." -ForegroundColor Green
    try{
        Get-AzureRmSqlServer -ServerName $sqlDatabaseServerName -ResourceGroupName $resourceGroupName}
    catch{
        Write-Host "`nCreating SQL Database server ..."  -ForegroundColor Green
    
        $sqlDatabasePW = ConvertTo-SecureString -String $sqlDatabasePassword -AsPlainText -Force
        $credential = New-Object System.Management.Automation.PSCredential($sqlDatabaseLogin,$sqlDatabasePW)
    
        $sqlDatabaseServerName = (New-AzureRmSqlServer `
                                    -ResourceGroupName $resourceGroupName `
                                    -ServerName $sqlDatabaseServerName `
                                    -SqlAdministratorCredentials $credential `
                                    -Location $location).ServerName
        Write-Host "`tThe new SQL database server name is $sqlDatabaseServerName." -ForegroundColor Cyan
    
        Write-Host "`nCreating firewall rule, $fireWallRuleName ..." -ForegroundColor Green
        $workstationIPAddress = Invoke-RestMethod $ipAddressRestService
        New-AzureRmSqlServerFirewallRule `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -FirewallRuleName "$fireWallRuleName-workstation" `
            -StartIpAddress $workstationIPAddress `
            -EndIpAddress $workstationIPAddress
    
        #To allow other Azure services to access the server add a firewall rule and set both the StartIpAddress and EndIpAddress to 0.0.0.0. 
        #Note that this allows Azure traffic from any Azure subscription to access the server.
        New-AzureRmSqlServerFirewallRule `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -FirewallRuleName "$fireWallRuleName-Azureservices" `
            -StartIpAddress "0.0.0.0" `
            -EndIpAddress "0.0.0.0"
    }
    
    #endregion
    
    #region - Create and validate Azure SQL database
    Write-Host "`nCreating an Azure SQL database ..." -ForegroundColor Green
    
    try {
        Get-AzureRmSqlDatabase `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -DatabaseName $sqlDatabaseName
    }
    catch {
        Write-Host "`nCreating SQL Database, $sqlDatabaseName ..."  -ForegroundColor Green
        New-AzureRMSqlDatabase `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -DatabaseName $sqlDatabaseName `
            -Edition "Standard" `
            -RequestedServiceObjectiveName "S1"
    }
    
    #endregion
    
    #region - Create tables
    Write-Host "Creating the log4jlogs table and the mobiledata table ..." -ForegroundColor Green
    
    $conn = New-Object System.Data.SqlClient.SqlConnection
    $conn.ConnectionString = $sqlDatabaseConnectionString
    $conn.Open()
    
    # Create the log4jlogs table and index
    $cmd = New-Object System.Data.SqlClient.SqlCommand
    $cmd.Connection = $conn
    $cmd.CommandText = $cmdCreateLog4jTable
    $ret = $cmd.ExecuteNonQuery()
    $cmd.CommandText = $cmdCreateLog4jClusteredIndex
    $cmd.ExecuteNonQuery()
    
    # Create the mobiledata table and index
    $cmd.CommandText = $cmdCreateMobileTable
    $cmd.ExecuteNonQuery()
    $cmd.CommandText = $cmdCreateMobileDataClusteredIndex
    $cmd.ExecuteNonQuery()
    
    $conn.close()
    
    #endregion
    
    
    #region - Create HDInsight cluster
    
    Write-Host "Creating the HDInsight cluster and the dependent services ..." -ForegroundColor Green
    
    # Create the default storage account
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Location $location `
        -Type Standard_LRS
    
    # Create the default Blob container
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext `
                                        -StorageAccountName $defaultStorageAccountName `
                                        -StorageAccountKey $defaultStorageAccountKey 
    New-AzureStorageContainer `
        -Name $defaultBlobContainerName `
        -Context $defaultStorageAccountContext 
    
    # Create the HDInsight cluster
    $pw = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
    $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)
    
    New-AzureRmHDInsightCluster `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $HDInsightClusterName `
        -Location $location `
        -ClusterType Hadoop `
        -OSType Windows `
        -ClusterSizeInNodes 2 `
        -HttpCredential $httpCredential `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $defaultBlobContainerName 
    
    # Validate the cluster
    Get-AzureRmHDInsightCluster -ClusterName $hdinsightClusterName
    #endregion
    
    #region - pre-process the source file
    
    Write-Host "Preprocessing the source file ..." -ForegroundColor Green
    
    # This procedure creates a new file with $destBlobName
    $sourceBlobName = "example/data/sample.log"
    $destBlobName = "tutorials/usesqoop/data/sample.log"
    
    # Define the connection string
    $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$defaultStorageAccountName;AccountKey=$defaultStorageAccountKey"
    
    # Create block blob objects referencing the source and destination blob.
    $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
    $storageClient = $storageAccount.CreateCloudBlobClient();
    $storageContainer = $storageClient.GetContainerReference($defaultBlobContainerName)
    $sourceBlob = $storageContainer.GetBlockBlobReference($sourceBlobName)
    $destBlob = $storageContainer.GetBlockBlobReference($destBlobName)
    
    # Define a MemoryStream and a StreamReader for reading from the source file
    $stream = New-Object System.IO.MemoryStream
    $stream = $sourceBlob.OpenRead()
    $sReader = New-Object System.IO.StreamReader($stream)
    
    # Define a MemoryStream and a StreamWriter for writing into the destination file
    $memStream = New-Object System.IO.MemoryStream
    $writeStream = New-Object System.IO.StreamWriter $memStream
    
    # Pre-process the source blob
    $exString = "java.lang.Exception:"
    while(-Not $sReader.EndOfStream){
        $line = $sReader.ReadLine()
        $split = $line.Split(" ")
    
        # remove the "java.lang.Exception" from the first element of the array
        # for example: java.lang.Exception: 2012-02-03 19:11:02 SampleClass8 [WARN] problem finding id 153454612
        if ($split[0] -eq $exString){
            #create a new ArrayList to remove $split[0]
            $newArray = [System.Collections.ArrayList] $split
            $newArray.Remove($exString)
    
            # update $split and $line
            $split = $newArray
            $line = $newArray -join(" ")
        }
    
        # remove the lines that has less than 7 elements
        if ($split.count -ge 7){
            write-host $line
            $writeStream.WriteLine($line)
        }
    }
    
    # Write to the destination blob
    $writeStream.Flush()
    $memStream.Seek(0, "Begin")
    $destBlob.UploadFromStream($memStream)
    
    #endregion
    
    #region - export a log file from the cluster to the SQL database
    
    Write-Host "Preprocessing the source file ..." -ForegroundColor Green
    
    $tableName_log4j = "log4jlogs"
    
    # Connection string for Azure SQL Database.
    # Comment if using SQL Server
    $connectionString = "jdbc:sqlserver://$sqlDatabaseServerName.database.windows.net;user=$sqlDatabaseLogin@$sqlDatabaseServerName;password=$sqlDatabasePassword;database=$sqlDatabaseName"
    # Connection string for SQL Server.
    # Uncomment if using SQL Server.
    #$connectionString = "jdbc:sqlserver://$sqlDatabaseServerName;user=$sqlDatabaseLogin;password=$sqlDatabasePassword;database=$sqlDatabaseName"
    
    $exportDir_log4j = "/tutorials/usesqoop/data"
    
    # Submit a Sqoop job
    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
        -Command "export --connect $connectionString --table $tableName_log4j --export-dir $exportDir_log4j --input-fields-terminated-by \0x20 -m 1"
    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose
    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId
    
    Write-Host "Standard Error" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -HttpCredential $httpCredential -JobId $sqoopJob.JobId -DisplayOutputType StandardError
    Write-Host "Standard Output" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -HttpCredential $httpCredential -JobId $sqoopJob.JobId -DisplayOutputType StandardOutput
    
    #endregion
    
    #region - export a Hive table
    
    $tableName_mobile = "mobiledata"
    $exportDir_mobile = "/hive/warehouse/hivesampletable"
    
    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
        -Command "export --connect $connectionString --table $tableName_mobile --export-dir $exportDir_mobile --fields-terminated-by \t -m 1"
    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose
    
    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId
    
    Write-Host "Standard Error" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardError
    
    Write-Host "Standard Output" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardOutput
    
    #endregion
    
    #region - import a database
    
    $targetDir_mobile = "/tutorials/usesqoop/importeddata/"
    
    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
        -Command "import --connect $connectionString --table $tableName_mobile --target-dir $targetDir_mobile --fields-terminated-by \t --lines-terminated-by \n -m 1"
    
    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose
    
    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId
    
    Write-Host "Standard Error" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardError
    
    Write-Host "Standard Output" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardOutput
    
    #endregion



[azure-management-portal]: https://portal.azure.com/

[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md

[sqldatabase-get-started]: ../sql-database/sql-database-get-started.md
[sqldatabase-create-configue]: ../sql-database/sql-database-get-started.md

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: powershell-install-configure.md
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[sqoop-user-guide-1.4.4]: https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html

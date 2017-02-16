<properties
    pageTitle="Migrieren von Azure Ressourcenmanager Development Tools für Cluster HDInsight | Microsoft Azure"
    description="Zum Migrieren zu Azure Ressourcenmanager Development Tools für HDInsight Cluster"
    services="hdinsight"
    editor="cgronlun"
    manager="jhubbard"
    authors="nitinme"
    documentationCenter=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="nitinme"/>

# <a name="migrating-to-azure-resource-manager-based-development-tools-for-hdinsight-clusters"></a>Ressourcenmanager Azure-basierte Entwicklung von Tools für HDInsight Cluster migrieren

HDInsight ist Azure Service Manager ASM basierende Tools für HDInsight veralteter. Wenn Sie Azure PowerShell, Azure CLI oder HDInsight .NET SDK verwendet haben für die Arbeit mit HDInsight Cluster, werden Sie aufgefordert, die Azure Ressource Manager Cloud-basierte Versionen von PowerShell, CLI und .NET SDK Zukunft verwenden. Dieser Artikel enthält Zeiger zum Migrieren zu der neuen Cloud-basierte Ansatz. Wo anwendbar, Datenpunkte in diesem Artikel auch die Unterschiede zwischen der ASM und Cloud Vorgehensweisen für HDInsight.

>[AZURE.IMPORTANT] Die Unterstützung für ASM Basis PowerShell CLI, und .NET SDK wird auf **1 Januar 2017**abzubrechen.

##<a name="migrating-azure-cli-to-azure-resource-manager"></a>Migrieren von Azure CLI zu Azure Ressourcenmanager

Die CLI Azure standardmäßig jetzt Azure Ressource Manager (Cloud) im Modus, wenn Sie ein von einer früheren Installation Upgrade. In diesem Fall müssen Sie mit der `azure config mode arm` Befehl aus, um in Cloud-Modus zu schalten.

Die grundlegende Befehle, die die CLI Azure bereitgestellt zum Arbeiten mit HDInsight mithilfe von Azure Service Management (ASM) sind gleich, wenn Cloud zu verwenden. jedoch einige Parameter und Optionen möglicherweise neue Namen, und es stehen viele neue Parameter bei Verwendung des Cloud. Angenommen, Sie können jetzt `azure hdinsight cluster create` den Azure virtuelle Netzwerk aus, das ein Cluster erstellt werden soll, oder Struktur und Oozie Metastore Informationen angeben.

Grundlegende Befehle für die Arbeit mit HDInsight bis Azure Ressourcenmanager sind:

* `azure hdinsight cluster create`-erstellt einen neuen HDInsight Cluster
* `azure hdinsight cluster delete`-Löscht einen vorhandenen HDInsight Cluster
* `azure hdinsight cluster show`– Anzeigen von Informationen zu einem vorhandenen Cluster
* `azure hdinsight cluster list`-HDInsight Cluster für Ihr Abonnement Azure-Listen

Verwenden der `-h` wechseln zu untersuchenden die Parameter und Optionen für jeden Befehl zur Verfügung.

###<a name="new-commands"></a>Neue Befehle

Neue Befehle mit Azure Ressourcenmanager verfügbar sind:

* `azure hdinsight cluster resize`-dynamisch ändert die Anzahl der Worker-Knoten im Cluster
* `azure hdinsight cluster enable-http-access`-HTTPs-Zugriff auf den Cluster ermöglicht (klicken Sie auf als Standard)
* `azure hdinsight cluster disable-http-access`-HTTPs-Zugriff auf den Cluster deaktiviert
* `azure hdinsight-enable-rdp-access`-ermöglicht Remote Desktop-Protokoll auf einem Windows-basierten HDInsight Cluster
* `azure hdinsight-disable-rdp-access`-Remote Desktop-Protokoll auf einem Windows-basierten HDInsight Cluster deaktiviert
* `azure hdinsight script-action`-enthält Befehle zum Erstellen/Verwalten von Skript-Aktionen in einem Cluster
* `azure hdinsight config`-Befehle enthält, mit eine Konfigurationsdatei erstellen, die verwendet werden kann die `hdinsight cluster create` Befehl aus, um Informationen zur Netzwerkkonfiguration enthalten.

###<a name="deprecated-commands"></a>Veraltete Befehle

Wenn Sie verwenden die `azure hdinsight job` Befehle Aufträge zum Cluster HDInsight, und übermitteln Sie diese stehen nicht zur Verfügung, über die Cloud-Befehle. Wenn Sie Aufträge programmgesteuert mit HDInsight von Skripts zu senden müssen, sollten Sie stattdessen die von HDInsight bereitgestellten REST-APIs verwenden. Weitere Informationen über REST-APIs Aufträge weiterleiten finden Sie unter den folgenden Dokumenten.

* [HDInsight mithilfe von cURL MapReduce Aufträge mit Hadoop ausgeführt](hdinsight-hadoop-use-mapreduce-curl.md)
* [HDInsight mithilfe von cURL Struktur Abfragen mit Hadoop ausgeführt](hdinsight-hadoop-use-hive-curl.md)
* [Schwein Aufträge mit Hadoop HDInsight mithilfe von cURL ausgeführt](hdinsight-hadoop-use-pig-curl.md)

Informationen zu anderen Methoden zum MapReduce, Struktur und Schwein interaktiv ausgeführt werden finden Sie unter [Verwenden von MapReduce mit Hadoop auf HDInsight](hdinsight-use-mapreduce.md), [Verwendung mit Hadoop auf HDInsight Struktur](hdinsight-use-hive.md)und [Verwenden Schwein mit Hadoop auf HDInsight](hdinsight-use-pig.md).

###<a name="examples"></a>Beispiele

__Erstellen einen cluster__

* Alte Befehl (ASM)`azure hdinsight cluster create myhdicluster --location northeurope --osType linux --storageAccountName mystorage --storageAccountKey <storagekey> --storageContainer mycontainer --userName admin --password mypassword --sshUserName sshuser --sshPassword mypassword`
* Neuer Befehl (Cloud)`azure hdinsight cluster create myhdicluster -g myresourcegroup --location northeurope --osType linux --clusterType hadoop --defaultStorageAccountName mystorage --defaultStorageAccountKey <storagekey> --defaultStorageContainer mycontainer --userName admin -password mypassword --sshUserName sshuser --sshPassword mypassword`

__Löschen eines Clusters__

* Alte Befehl (ASM)`azure hdinsight cluster delete myhdicluster`
* Neuer Befehl (Cloud)`azure hdinsight cluster delete mycluster -g myresourcegroup`

__Liste Cluster__

* Alte Befehl (ASM)`azure hdinsight cluster list`
* Neuer Befehl (Cloud)`azure hdinsight cluster list`

> [AZURE.NOTE] Für den Listenbefehl angeben der Ressource Gruppe mit `-g` werden nur die Cluster in der angegebenen Ressourcengruppe zurückzukehren.

__Clusterinformationen anzeigen__

* Alte Befehl (ASM)`azure hdinsight cluster show myhdicluster`
* Neuer Befehl (Cloud)`azure hdinsight cluster show myhdicluster -g myresourcegroup`


##<a name="migrating-azure-powershell-to-azure-resource-manager"></a>Migrieren von Azure PowerShell zu Azure Ressourcenmanager

Die allgemeine Informationen zu Azure PowerShell im Modus Azure Ressource Manager (Cloud) kann bei [Verwendung von Azure PowerShell Azure Ressourcenmanager](../powershell-azure-resource-manager.md)gefunden werden.

Die Cmdlets Azure PowerShell Cloud kann Computer die Cmdlets ASM installiert sein. Die Cmdlets aus den beiden Modi kann nach ihren Namen unterschieden werden.  Der Cloud-Modus wurde *AzureRmHDInsight* im Vergleich zu *AzureHDInsight* im Modus ASM Cmdlet-Namen.  Beispielsweise *Neu-AzureRmHDInsightCluster* im Vergleich zu *Neu-AzureHDInsightCluster*. Parameter und Optionen möglicherweise News Namen, und es stehen viele neue Parameter bei Verwendung des Cloud.  Mehrere Cmdlets erfordern beispielsweise einen neuen Switch *- ResourceGroupName*bezeichnet. 

Bevor Sie die Cmdlets HDInsight verwenden können, müssen Sie Verbinden mit Ihrer Azure-Konto, und erstellen eine neuen Ressourcengruppe:

- Login-AzureRmAccount oder [AzureRmProfile auswählen](https://msdn.microsoft.com/library/mt619310.aspx). Finden Sie unter [Authentifizierung ein Dienst Tilgungsanteile Azure Ressourcenmanager](../resource-group-authenticate-service-principal.md)
- [Neue AzureRmResourceGroup](https://msdn.microsoft.com/library/mt603739.aspx)

###<a name="renamed-cmdlets"></a>Umbenannte cmdlets

Liste der Cmdlets HDInsight ASM in Windows PowerShell-Konsole:

    help *azurermhdinsight*

Die folgende Tabelle enthält die ASM Cmdlets und deren Namen in der Cloud-Modus:

|ASM-cmdlets|Cloud-cmdlets|
|------|------|
| Hinzufügen von AzureHDInsightConfigValues | [Hinzufügen von AzureRmHDInsightConfigValues](https://msdn.microsoft.com/library/mt603530.aspx)|
| Hinzufügen von AzureHDInsightMetastore |[Hinzufügen von AzureRmHDInsightMetastore](https://msdn.microsoft.com/library/mt603670.aspx)|
| Hinzufügen von AzureHDInsightScriptAction |[Hinzufügen von AzureRmHDInsightScriptAction](https://msdn.microsoft.com/library/mt603527.aspx)|
| Hinzufügen von AzureHDInsightStorage |[Hinzufügen von AzureRmHDInsightStorage](https://msdn.microsoft.com/library/mt619445.aspx)|
| Get-AzureHDInsightCluster |[Get-AzureRmHDInsightCluster](https://msdn.microsoft.com/library/mt619371.aspx)|
| Get-AzureHDInsightJob |[Get-AzureRmHDInsightJob](https://msdn.microsoft.com/library/mt603590.aspx)|
| Get-AzureHDInsightJobOutput |[Get-AzureRmHDInsightJobOutput](https://msdn.microsoft.com/library/mt603793.aspx)|
| Get-AzureHDInsightProperties |[Get-AzureRmHDInsightProperties](https://msdn.microsoft.com/library/mt603546.aspx) |
| Gewähren des-AzureHDInsightHttpServicesAccess | [Gewähren des-AzureRmHDInsightHttpServicesAccess](https://msdn.microsoft.com/library/mt619407.aspx)|
| Gewähren des-AzureHdinsightRdpAccess |[Gewähren des-AzureRmHDInsightRdpServicesAccess](https://msdn.microsoft.com/library/mt603717.aspx)|
| Rufen Sie AzureHDInsightHiveJob |[Rufen Sie AzureRmHDInsightHiveJob](https://msdn.microsoft.com/library/mt603593.aspx)|
| Neue AzureHDInsightCluster | [Neue AzureRmHDInsightCluster](https://msdn.microsoft.com/library/mt619331.aspx)|
| Neue AzureHDInsightClusterConfig |[Neue AzureRmHDInsightClusterConfig](https://msdn.microsoft.com/library/mt603700.aspx)|
| Neue AzureHDInsightHiveJobDefinition |[Neue AzureRmHDInsightHiveJobDefinition](https://msdn.microsoft.com/library/mt619448.aspx)|
| Neue AzureHDInsightMapReduceJobDefinition |[Neue AzureRmHDInsightMapReduceJobDefinition](https://msdn.microsoft.com/library/mt603626.aspx)|
| Neue AzureHDInsightPigJobDefinition |[Neue AzureRmHDInsightPigJobDefinition](https://msdn.microsoft.com/library/mt603671.aspx)|
| Neue AzureHDInsightSqoopJobDefinition |[Neue AzureRmHDInsightSqoopJobDefinition](https://msdn.microsoft.com/library/mt608551.aspx)|
| Neue AzureHDInsightStreamingMapReduceJobDefinition |[Neue AzureRmHDInsightStreamingMapReduceJobDefinition](https://msdn.microsoft.com/library/mt603626.aspx)|
| Entfernen-AzureHDInsightCluster |[Entfernen-AzureRmHDInsightCluster](https://msdn.microsoft.com/library/mt619431.aspx)|
| Widerrufen-AzureHDInsightHttpServicesAccess |[Widerrufen-AzureRmHDInsightHttpServicesAccess](https://msdn.microsoft.com/library/mt619375.aspx)|
| Widerrufen-AzureHdinsightRdpAccess |[Widerrufen-AzureRmHDInsightRdpServicesAccess](https://msdn.microsoft.com/library/mt603523.aspx)|
| Set-AzureHDInsightClusterSize |[Set-AzureRmHDInsightClusterSize](https://msdn.microsoft.com/library/mt603513.aspx)|
| Set-AzureHDInsightDefaultStorage |[Set-AzureRmHDInsightDefaultStorage](https://msdn.microsoft.com/library/mt603486.aspx)|
| Start-AzureHDInsightJob |[Start-AzureRmHDInsightJob](https://msdn.microsoft.com/library/mt603798.aspx)|
| Beenden-AzureHDInsightJob |[Beenden-AzureRmHDInsightJob](https://msdn.microsoft.com/library/mt619424.aspx)|
| Verwenden Sie-AzureHDInsightCluster |[Verwenden Sie-AzureRmHDInsightCluster](https://msdn.microsoft.com/library/mt619442.aspx)|
| Warten-AzureHDInsightJob |[Warten-AzureRmHDInsightJob](https://msdn.microsoft.com/library/mt603834.aspx)|

###<a name="new-cmdlets"></a>Neue cmdlets
Im folgenden werden die neuen Cmdlets, die nur in der Cloud-Modus verfügbar sind. 

**Skript für Aktion Zusammenhang Cmdlets:**
- **Get-AzureRmHDInsightPersistedScriptAction**: Ruft die dauerhaften Skriptaktionen für einen Cluster ab und listet sie chronologisch oder Details für eine angegebene dauerhaften Skriptaktion erhält. 
- **Get-AzureRmHDInsightScriptActionHistory**: Ruft den Skript Aktionsverlauf für einen Cluster und es reverse chronologisch Listen oder ruft Details für eine Aktion zuvor ausgeführten Skripts ab. 
- **Entfernen-AzureRmHDInsightPersistedScriptAction**: eine Skriptaktion dauerhaften aus einem HDInsight Cluster entfernt.
- **Set-AzureRmHDInsightPersistedScriptAction**: Legt einen Vorgang zuvor ausgeführten Skript eine Skriptaktion dauerhaften sein.
- **Absenden-AzureRmHDInsightScriptAction**: sendet eine neue Skriptaktion zu einem Cluster Azure HDInsight. 

Weitere Informationen zur Verwendung finden Sie unter [Anpassen Linux-basierten HDInsight Cluster mithilfe der Aktion Skript](hdinsight-hadoop-customize-cluster-linux.md).

**Clsuter Identität Zusammenhang Cmdlets:**

- **Hinzufügen-AzureRmHDInsightClusterIdentity**: Fügt eine Cluster Identität zu einem Objekt der Cluster-Konfiguration, damit der HDInsight Cluster Azure Lake Datenspeicher zugreifen kann. Finden Sie unter [Erstellen einer HDInsight Cluster mit dem Datenspeicher Azure PowerShell verwenden](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell.md).

### <a name="examples"></a>Beispiele

**Erstellen von cluster**

Alte Befehl (ASM): 

    New-AzureHDInsightCluster `
        -Name $clusterName `
        -Location $location `
        -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $storageAccountKey `
        -DefaultStorageContainerName $containerName `
        -ClusterSizeInNodes 2 `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.2" `
        -Credential $httpCredential `
        -SshCredential $sshCredential

Neue Befehl (Cloud):

    New-AzureRmHDInsightCluster `
        -ClusterName $clusterName `
        -ResourceGroupName $resourceGroupName `
        -Location $location `
        -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $storageAccountKey `
        -DefaultStorageContainer $containerName  `
        -ClusterSizeInNodes 2 `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.2" `
        -HttpCredential $httpcredentials `
        -SshCredential $sshCredentials

 
**Cluster löschen**

Alte Befehl (ASM):

    Remove-AzureHDInsightCluster -name $clusterName 

Neue Befehl (Cloud):

    Remove-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName 
                
**Liste cluster**

Alte Befehl (ASM):

    Get-AzureHDInsightCluster
                
Neue Befehl (Cloud):

    Get-AzureRmHDInsightCluster 

**Cluster anzeigen**

Alte Befehl (ASM):

    Get-AzureHDInsightCluster -Name $clusterName
                
Neue Befehl (Cloud):

    Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -clusterName $clusterName


####<a name="other-samples"></a>Andere Beispiele

- [HDInsight Cluster erstellen](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)
- [Struktur Aufträge senden](hdinsight-hadoop-use-hive-powershell.md)
- [Schwein Aufträge senden](hdinsight-hadoop-use-pig-powershell.md)
- [Übermitteln Sqoop Aufträge](hdinsight-hadoop-use-sqoop-powershell.md)



##<a name="migrating-to-the-arm-based-hdinsight-net-sdk"></a>Migrieren zu Cloud-basierte HDInsight .NET SDK

Das Servicemanagement Azure-basierten [(ASM) HDInsight.NET SDK](https://msdn.microsoft.com/library/azure/mt416619.aspx) ist jetzt veraltet. Sie werden aufgefordert, das Ressourcenverwaltung Azure-basiertes [(Cloud) HDInsight .NET SDK](https://msdn.microsoft.com/library/azure/mt271028.aspx)verwenden. Die folgenden HDInsight ASM-basierte Pakete werden nicht mehr unterstützt.

* `Microsoft.WindowsAzure.Management.HDInsight`
* `Microsoft.Hadoop.Client`
 

Dieser Abschnitt enthält Verweise auf Weitere Informationen zur Durchführung bestimmter Aufgaben mithilfe des Cloud-basierte SDK.

| So... mithilfe des Cloud-basierte HDInsight SDK | Links |
| ------------------- | --------------- |
| Erstellen Sie HDInsight Cluster mit .NET SDK| Finden Sie unter [Erstellen von HDInsight Cluster mit .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)|
| Anpassen eines Aktion Skript mit .NET SDK mit Clusters | Finden Sie unter [Anpassen HDInsight Linux Cluster mithilfe der Aktion Skript](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md#use-script-action) |
| Authentifizieren von Applications interaktiv mit .NET SDK mit Azure Active Directory| Finden Sie unter [Ausführen Struktur Abfragen mit .NET SDK](hdinsight-hadoop-use-hive-dotnet-sdk.md). Der Codeausschnitt in diesem Artikel wird den interaktive Authentifizierung Ansatz verwendet.|
| Authentifizieren von Applications, die nicht interaktiv Azure Active Directory mit .NET SDK verwenden | Finden Sie unter [Erstellen von nicht-interaktiven Applications für HDInsight](hdinsight-create-non-interactive-authentication-dotnet-applications.md) |
| Senden Sie einen Struktur Auftrag mit .NET SDK| Finden Sie unter [Senden Struktur Aufträge](hdinsight-hadoop-use-hive-dotnet-sdk.md) |
| Senden einer mit .NET SDK Schwein-Position | Finden Sie unter [Senden Schwein Aufträge](hdinsight-hadoop-use-pig-dotnet-sdk.md)|
| Senden einer Sqoop Position mit .NET SDK | Finden Sie unter [Senden Sqoop Aufträge](hdinsight-hadoop-use-sqoop-dotnet-sdk.md) |
| Liste mit .NET SDK HDInsight-Cluster | Finden Sie unter [Liste HDInsight Cluster](hdinsight-administer-use-dotnet-sdk.md#list-clusters) |
| Skalieren Sie HDInsight Cluster mit .NET SDK | Finden Sie unter [Skalieren HDInsight Cluster](hdinsight-administer-use-dotnet-sdk.md#scale-clusters) |
| Erteilen/widerrufen Sie den Zugriff auf HDInsight Cluster mit .NET SDK | Finden Sie unter [erteilen/widerrufen Sie den Zugriff auf HDInsight Cluster](hdinsight-administer-use-dotnet-sdk.md#grantrevoke-access) |
| Aktualisieren Sie HTTP-Benutzeranmeldeinformationen für HDInsight Cluster mit .NET SDK | Finden Sie unter [Aktualisieren HTTP Benutzeranmeldeinformationen für HDInsight Cluster](hdinsight-administer-use-dotnet-sdk.md#update-http-user-credentials) |
| Suchen des Standardkontos-Speicher für HDInsight Cluster mit .NET SDK | Finden Sie unter [Suchen im Speicher Standardkonto für HDInsight Cluster](hdinsight-administer-use-dotnet-sdk.md#find-the-default-storage-account) |
| Löschen Sie HDInsight Cluster mit .NET SDK | Finden Sie unter [Löschen HDInsight Cluster](hdinsight-administer-use-dotnet-sdk.md#delete-clusters) |

### <a name="examples"></a>Beispiele

Im folgenden werden einige Beispiele für ein Vorgang wie befinden mithilfe der ASM-basierten SDK und den entsprechenden Codeausschnitt für Cloud-basierte SDK ausgeführt.

**Erstellen eines Cluster CRUD-Clients**

* Alte Befehl (ASM)

        //Certificate auth
        //This logs the application in using a subscription administration certificate, which is not offered in Azure Resource Manager (ARM)
     
        const string subid = "454467d4-60ca-4dfd-a556-216eeeeeeee1";
        var cred = new HDInsightCertificateCredential(new Guid(subid), new X509Certificate2(@"path\to\certificate.cer"));
        var client = HDInsightClient.Connect(cred);

* Neuer Befehl (Cloud) (Service Hauptbenutzer Autorisierung)

        //Service principal auth
        //This will log the application in as itself, rather than on behalf of a specific user.
        //For details, including how to set up the application, see:
        //   https://azure.microsoft.com/en-us/documentation/articles/hdinsight-create-non-interactive-authentication-dotnet-applications/
         
        var authFactory = new AuthenticationFactory();
         
        var account = new AzureAccount { Type = AzureAccount.AccountType.ServicePrincipal, Id = clientId };
         
        var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];
         
        var accessToken = authFactory.Authenticate(account, env, tenantId, secretKey, ShowDialog.Never).AccessToken;
         
        var creds = new TokenCloudCredentials(subId.ToString(), accessToken);
         
        _hdiManagementClient = new HDInsightManagementClient(creds);

* Neuer Befehl (Cloud) (Benutzer Autorisierung)

        //User auth
        //This will log the application in on behalf of the user.
        //The end-user will see a login popup.
         
        var authFactory = new AuthenticationFactory();
         
        var account = new AzureAccount { Type = AzureAccount.AccountType.User, Id = username };
         
        var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];
         
        var accessToken = authFactory.Authenticate(account, env, AuthenticationFactory.CommonAdTenant, password, ShowDialog.Auto).AccessToken;
         
        var creds = new TokenCloudCredentials(subId.ToString(), accessToken);
         
        _hdiManagementClient = new HDInsightManagementClient(creds);


**Erstellen einen cluster**

* Alte Befehl (ASM)

        var clusterInfo = new ClusterCreateParameters
                    {
                        Name = dnsName,
                        DefaultStorageAccountKey = key,
                        DefaultStorageContainer = defaultStorageContainer,
                        DefaultStorageAccountName = storageAccountDnsName,
                        ClusterSizeInNodes = 1,
                        ClusterType = type,
                        Location = "West US",
                        UserName = "admin",
                        Password = "*******",
                        Version = version,
                        HeadNodeSize = NodeVMSize.Large,
                    };
        clusterInfo.CoreConfiguration.Add(new KeyValuePair<string, string>("config1", "value1"));
        client.CreateCluster(clusterInfo);


* Neuer Befehl (Cloud)

        var clusterCreateParameters = new ClusterCreateParameters
            {
                Location = "West US",
                ClusterType = "Hadoop",
                Version = "3.1",
                OSType = OSType.Windows,
                DefaultStorageAccountName = "mystorage.blob.core.windows.net",
                DefaultStorageAccountKey =
                    "O9EQvp3A3AjXq/W27rst1GQfLllhp0gUeiUUn2D8zX2lU3taiXSSfqkZlcPv+nQcYUxYw==",
                UserName = "hadoopuser",
                Password = "*******",
                HeadNodeSize = "ExtraLarge",
                RdpUsername = "hdirp",
                RdpPassword = ""*******",
                RdpAccessExpiry = new DateTime(2025, 3, 1),
                ClusterSizeInNodes = 5
            };
        var coreConfigs = new Dictionary<string, string> {{"config1", "value1"}};
        clusterCreateParameters.Configurations.Add(ConfigurationKey.CoreSite, coreConfigs);


**Aktivieren der HTTP-Zugriff**

* Alte Befehl (ASM)

        client.EnableHttp(dnsName, "West US", "admin", "*******");

* Neuer Befehl (Cloud)

        var httpParams = new HttpSettingsParameters
        {
               HttpUserEnabled = true,
               HttpUsername = "admin",
               HttpPassword = "*******",
        };
        client.Clusters.ConfigureHttpSettings(resourceGroup, dnsname, httpParams);

**Löschen eines Clusters**

* Alte Befehl (ASM)

        client.DeleteCluster(dnsName);

* Neuer Befehl (Cloud)

        client.Clusters.Delete(resourceGroup, dnsname);





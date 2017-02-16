<properties
    pageTitle="Berechnen von verknüpften Diensten | Microsoft Azure"
    description="Erfahren Sie, dass Sie in Azure Data Factory Pipelines verwenden können, um Transformation-Prozess Daten Enviornments berechnen."
    services="data-factory"
    documentationCenter=""
    authors="sharonlo101"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/22/2016"
    ms.author="shlo"/>

# <a name="compute-linked-services"></a>Berechnen von verknüpften Diensten

Dieser Artikel erläutert unterschiedlichen berechnen-Umgebungen, mit denen Sie, auf Vorgang oder eine Transformation Daten. Darüber hinaus, dass Details verschiedene Konfigurationen (bei Bedarf im Vergleich zu bringen, Ihre eigenen) von Daten Factory unterstützt, beim Konfigurieren von verknüpfter Diensten verknüpfen diese Umgebungen zu einer Factory Azure-Daten zu berechnen.

Die folgende Tabelle enthält eine Liste der berechnen Umgebungen unterstützt nach Daten Fabrik und die Aktivitäten, die diesen ausgeführt werden können. 

| Berechnen der Umgebung | Aktivitäten |
| ------------------- | -------- | 
| [Bei Bedarf HDInsight Cluster](#azure-hdinsight-on-demand-linked-service) oder [Eigene HDInsight cluster](#azure-hdinsight-linked-service) | [DotNet](data-factory-use-custom-activities.md), [Struktur](data-factory-hive-activity.md), [Schwein](data-factory-pig-activity.md), [MapReduce](data-factory-map-reduce.md), [Hadoop Streaming](data-factory-hadoop-streaming-activity.md) | 
| [Azure Stapel](#azure-batch-linked-service) | [DotNet](data-factory-use-custom-activities.md) |  
| [Learning Azure-Computern](#azure-machine-learning-linked-service) | [Maschinelle Learning Aktivitäten: Stapel Ausführung und Aktualisieren von Ressourcen](data-factory-azure-ml-batch-execution-activity.md) |
| [Azure-Daten Lake Analytics](#azure-data-lake-analytics-linked-service) | [Daten Lake Analytics U-SQL](data-factory-usql-activity.md)
| [SQL Azure](#azure-sql-linked-service), [SQL Azure Datawarehouse](#azure-sql-data-warehouse-linked-service), [SQLServer](#sql-server-linked-service) | [Gespeicherte Prozedur](data-factory-stored-proc-activity.md)

## <a name="on-demand-compute-environment"></a>Bei Bedarf berechnen-Umgebung

In dieser Art von Konfiguration wird die Umgebung vollständig vom Dienst Azure Data Factory verwaltet. Es wird automatisch vom Dienst Daten Factory erstellt werden, bevor ein Auftrags zum Verarbeiten von Daten vorgeschlagen und entfernt, wenn Sie der Auftrag abgeschlossen ist. Sie können einen verknüpften Dienst für die Umgebung bei Bedarf berechnen erstellen, konfigurieren und präzise Einstellungen für die Ausführung Auftrags, Cluster Management und Neustart von Aktionen zu steuern.

> [AZURE.NOTE] Die Konfiguration bei Bedarf wird derzeit nur für Azure HDInsight Cluster unterstützt.

## <a name="azure-hdinsight-on-demand-linked-service"></a>Azure HDInsight bei Bedarf verknüpft Service
Der Azure Data Factory-Dienst kann automatisch einen Windows, Linux-basierten bei Bedarf HDInsight Cluster zum Verarbeiten von Daten erstellen. Der Cluster wird in derselben Region als Cluster zugeordnete Speicherplatz Konto (LinkedServiceName-Eigenschaft in den JSON) erstellt.

Beachten Sie die folgenden **wichtigen** Punkte zu bei Bedarf verknüpft HDInsight Dienst an:

- Erstellt in Ihrem Abonnement Azure HDInsight bei Bedarf Cluster wird nicht angezeigt. der Azure-Daten Factory-Dienst verwaltet Cluster HDInsight bei Bedarf in Ihrem Auftrag.
- Die Protokolle für Projekte, die auf einer bei Bedarf HDInsight Cluster ausgeführt werden, werden in HDInsight Cluster zugeordnete Speicherplatz Konto kopiert. Sie können diese Protokolle vom in die **Aktivität ausführen Details** Blade Azure-Portal zugreifen. Details finden Sie unter [Überwachen und Verwalten von Pipelines](data-factory-monitor-manage-pipelines.md) diesem Artikel.
- Sie unterliegen nur für die Zeit, zu der HDInsight Cluster nach oben und laufenden Aufträge.

> [AZURE.IMPORTANT] Es dauert in der Regel mehr als **15 Minuten** zum Bereitstellen eines Azure HDInsight Clusters bei Bedarf.

### <a name="example"></a>Beispiel
Die folgende JSON definiert einen Linux-basierten bei Bedarf verknüpft HDInsight Dienst an. Der Daten Factory-Dienst erstellt automatisch einen **Linux-basierten** HDInsight Cluster, wenn ein Segment Daten zu verarbeiten. 


    {
        "name": "HDInsightOnDemandLinkedService",
        "properties": {
            "type": "HDInsightOnDemand",
            "typeProperties": {
                "clusterSize": 4,
                "timeToLive": "00:05:00",
                "osType": "linux",
                "linkedServiceName": "StorageLinkedService"
            }
        }
    }

Um einen Windows-basierten HDInsight Cluster verwenden zu können, legen Sie **OsType** auf **Windows** oder verwenden Sie die Eigenschaft nicht, wie der Standardwert: Windows.  

> [AZURE.IMPORTANT] 
> HDInsight Cluster erstellt einen **standardmäßige Container** in den Blob-Speicher, die, den Sie in das JSON (**LinkedServiceName**) angegeben haben. HDInsight wird dieser Container nicht gelöscht, wenn der Cluster gelöscht wird. Dieses Verhalten ist beabsichtigt. Bei Bedarf verknüpft HDInsight Dienst eine HDInsight Cluster erstellt wird jedes Mal, wenn ein Segment muss verarbeitet werden, es sei denn, es ist ein vorhandenes live Cluster (**TimeToLive**) und wird gelöscht, wenn die Verarbeitung abgeschlossen ist. 
> 
> Als weitere Segmente verarbeitet werden, wird in Ihrem Azure Blob Storage viele Container. Wenn Sie nicht zur Behandlung dieses Problems der Einzelvorgänge benötigen, möchten Sie möglicherweise löschen, um die Speicherkosten für reduzieren. Führen Sie die Namen dieser Container ein Muster: "Adf**Yourdatafactoryname**-**Linkedservicename**- Datetimestamp". Mit Tools wie [Microsoft Speicher-Explorer](http://storageexplorer.com/) zum Container in Ihrer Azure Blob-Speicher löschen.

### <a name="properties"></a>Eigenschaften

Eigenschaft | Beschreibung | Erforderlich
-------- | ----------- | --------
Typ | Die Eigenschaft sollte **HDInsightOnDemand**festgelegt werden. | Ja
clusterSize | Anzahl der Worker/Daten Knoten im Cluster. HDInsight Cluster wird mit 2 am Knoten zusammen mit der Anzahl der Worker Knoten erstellt, die Sie für diese Eigenschaft angeben. Die Knoten sind Größe Standard_D3, die 4 Kernen hat, damit ein Cluster mit 4 Worker Knoten 24 Kernen (4*für Worker Knoten 4 + 2*4 für am Knoten) erfordert. Details der Standard_D3 Ebene finden Sie unter [Erstellen von Linux-basierten Hadoop Cluster in HDInsight](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md) .  | Ja
TimeToLive | Der zulässige Leerlauf für Cluster HDInsight bei Bedarf. Gibt an, wie lange bei Bedarf HDInsight Cluster bleibt für nach Abschluss einer Aktivität ausgeführt werden, wenn es keine anderen aktiven Aufträge im Cluster sind aktiv.<br/><br/>Beispielsweise bleibt eine Aktivität Ausführung dauert 6 Minuten, und Timetolive auf 5 Minuten eingestellt ist, der Cluster aktiv für 5 Minuten nach der die Verarbeitungsaktivität 6 Minuten ausführen. Wenn eine andere Aktivität ausführen mit dem Fenster 6 Minuten ausgeführt wird, wird er vom gleichen Cluster verarbeitet werden.<br/><br/>Erstellen eines bei Bedarf HDInsight Clusters ist ein teurer Vorgang (könnte eine Weile dauern), also verwenden Sie diese Einstellung als für optimale Leistung einer Factory Daten, wenn Sie wiederverwenden eines bei Bedarf HDInsight Clusters erforderlich.<br/><br/>Wenn Sie Timetolive Wert 0 festgelegt, sobald die Aktivität ausführen verarbeiteten Cluster gelöscht. Wenn Sie einen hohen Wert festlegen, kann der Cluster andererseits, im Leerlauf resultierender unnötig hohen Kosten aussetzen. Daher ist es wichtig, dass Sie den entsprechenden Wert entsprechend Ihren Anforderungen festlegen.<br/><br/>Mehrere Pipelines können die gleiche Instanz von bei Bedarf HDInsight Cluster freigeben, wenn der Wert der Eigenschaft Timetolive ordnungsgemäß festgelegt ist | Ja
Version | Version der HDInsight Cluster. Der Standardwert ist für Windows-Cluster 3.1 und 3,2 für Linux Cluster. | Nein
linkedServiceName | Azure verknüpft Speicherdienst vom Cluster bei Bedarf zum Speichern und Verarbeiten von Daten verwendet werden. | Ja
additionalLinkedServiceNames | Gibt an, dass zusätzlicher Speicherkonten für die HDInsight Dienst verknüpft, damit der Daten Factory-Dienst in Ihrem Auftrag registrieren werden kann. | Nein
osType | Typ des Betriebssystems. Sind die Werte zulässig: Windows (Standard) und Linux | Nein
hcatalogLinkedServiceName | Der Namen der SQL Azure verknüpft Dienst, die in der Datenbank HCatalog verweisen. Bei Bedarf HDInsight Cluster wird mithilfe der SQL Azure-Datenbank als die Metastore erstellt. | Nein


#### <a name="additionallinkedservicenames-json-example"></a>AdditionalLinkedServiceNames JSON-Beispiel

    "additionalLinkedServiceNames": [
        "otherLinkedServiceName1",
        "otherLinkedServiceName2"
    ]
 
### <a name="advanced-properties"></a>Erweiterte Eigenschaften

Sie können auch die folgenden Eigenschaften für die detaillierte Konfiguration von bei Bedarf HDInsight Cluster angeben.

Eigenschaft | Beschreibung | Erforderlich
:-------- | :----------- | :--------
coreConfiguration | Gibt die Core Konfigurationsparameter (wie in Core-site.xml) für den HDInsight Cluster erstellt werden. | Nein
hBaseConfiguration | Gibt die HBase Konfigurationsparameter (Hbase-site.xml) für den Cluster HDInsight. | Nein
hdfsConfiguration | Gibt die HDFS Konfigurationsparameter (Hdfs-site.xml) für den Cluster HDInsight. | Nein
hiveConfiguration | Gibt die Struktur Konfigurationsparameter (Struktur-site.xml) für den Cluster HDInsight. | Nein
mapReduceConfiguration | Gibt die MapReduce Konfigurationsparameter (Mapred-site.xml) für den Cluster HDInsight. | Nein
oozieConfiguration | Gibt die Oozie Konfigurationsparameter (Oozie-site.xml) für den Cluster HDInsight. | Nein
stormConfiguration | Gibt die Storm Konfigurationsparameter (Storm-site.xml) für den Cluster HDInsight. | Nein
yarnConfiguration | Gibt die aus Konfigurationsparameter (aus – site.xml) für den Cluster HDInsight. | Nein

#### <a name="example--on-demand-hdinsight-cluster-configuration-with-advanced-properties"></a>Beispiel – bei Bedarf HDInsight Cluster-Konfiguration mit erweiterten Eigenschaften

    {
      "name": " HDInsightOnDemandLinkedService",
      "properties": {
        "type": "HDInsightOnDemand",
        "typeProperties": {
          "clusterSize": 16,
          "timeToLive": "01:30:00",
          "linkedServiceName": "adfods1",
          "coreConfiguration": {
            "templeton.mapper.memory.mb": "5000"
          },
          "hiveConfiguration": {
            "templeton.mapper.memory.mb": "5000"
          },
          "mapReduceConfiguration": {
            "mapreduce.reduce.java.opts": "-Xmx4000m",
            "mapreduce.map.java.opts": "-Xmx4000m",
            "mapreduce.map.memory.mb": "5000",
            "mapreduce.reduce.memory.mb": "5000",
            "mapreduce.job.reduce.slowstart.completedmaps": "0.8"
          },
          "yarnConfiguration": {
            "yarn.app.mapreduce.am.resource.mb": "5000",
            "mapreduce.map.memory.mb": "5000"
          },
          "additionalLinkedServiceNames": [
            "datafeeds",
            "adobedatafeed"
          ]
        }
      }
    }

### <a name="node-sizes"></a>Knoten Größen
Sie können die Größe der Leiter, Daten und Zookeeper Knoten mit den folgenden Eigenschaften festlegen: 

Eigenschaft | Beschreibung | Erforderlich
:-------- | :----------- | :--------
headNodeSize | Gibt die Größe des am Knotens. Ist der Standardwert: Standard_D3. Finden Sie im Abschnitt **Angabe Knoten Größen** unter Details aus. | Nein
dataNodeSize | Gibt die Größe des Knotens Daten. Ist der Standardwert: Standard_D3. | Nein
zookeeperNodeSize | Gibt die Größe des Zoodirektor Knotens. Ist der Standardwert: Standard_D3. | Nein
 
#### <a name="specifying-node-sizes"></a>Angeben von Knoten Größen
Finden Sie im Artikel [Größen von virtuellen Computern](../virtual-machines/virtual-machines-linux-sizes.md#size-tables) Zeichenfolgenwerte, die Sie für die oben genannten Eigenschaften angeben müssen. Die Werte müssen die **CMDLETs und APIS** im Artikel referenzierte entsprechen. Wie Sie im Artikel sehen können, hat der Datenknoten groß (Standard) Größe 7 GB Arbeitsspeicher, die möglicherweise nicht zufrieden für Ihr Szenario aus. 

Wenn Sie am D4 Größe und Worker Knoten erstellen möchten, müssen Sie **Standard_D4** als den Wert für die Eigenschaften HeadNodeSize und DataNodeSize angeben. 

    "headNodeSize": "Standard_D4",  
    "dataNodeSize": "Standard_D4",

Wenn Sie einen falschen Wert für diese Eigenschaften angeben, erhalten Sie möglicherweise die folgenden **Fehler:** Fehler beim Cluster erstellen. Ausnahme: Kann nicht abgeschlossen Cluster Erstellungsvorgang. Fehler bei Vorgang mit Code '400' Links hinter Zustand Cluster: "Fehler". Meldung: 'PreClusterCreationValidationFailure'. Wenn Sie diese Fehlermeldung erhalten, stellen Sie sicher, dass Sie den **CMDLET und APIS** Namen aus der Tabelle in der obigen Artikel verwenden.  



## <a name="bring-your-own-compute-environment"></a>Zeigen Sie Ihre eigenen berechnen-Umgebung

In dieser Art von Konfiguration können Benutzer eine bereits vorhandene computing-Umgebung als verknüpfte Dienst in Daten Factory registrieren. Die computing-Umgebung, die vom Benutzer verwaltet wird, und der Daten Factory-Dienst wird es verwendet, um die Aktivitäten ausführen.

Diese Art der Konfiguration wird für die folgenden berechnen Umgebungen unterstützt:

- Azure HDInsight
- Azure Stapel
- Azure-Computern Schulung.

## <a name="azure-hdinsight-linked-service"></a>Azure HDInsight verknüpft Service

Sie können einen Dienst Azure HDInsight verknüpft um eigene HDInsight Cluster mit Daten Factory registrieren erstellen.

### <a name="example"></a>Beispiel

    {
      "name": "HDInsightLinkedService",
      "properties": {
        "type": "HDInsight",
        "typeProperties": {
          "clusterUri": " https://<hdinsightclustername>.azurehdinsight.net/",
          "userName": "admin",
          "password": "<password>",
          "linkedServiceName": "MyHDInsightStoragelinkedService"
        }
      }
    }

### <a name="properties"></a>Eigenschaften

Eigenschaft | Beschreibung | Erforderlich
-------- | ----------- | --------
Typ | Die Eigenschaft sollte **HDInsight**festgelegt werden. | Ja
clusterUri | Der URI des HDInsight Cluster. | Ja
Benutzername | Geben Sie den Namen des Benutzers, die Verbindung zu einem vorhandenen HDInsight Cluster verwendet werden soll. | Ja
Kennwort | Geben Sie das Kennwort für das Benutzerkonto an. | Ja
linkedServiceName | Name des verknüpften Dienst Blob-Speicher, die von diesem Cluster HDInsight verwendet. | Ja

## <a name="azure-batch-linked-service"></a>Azure Stapel verknüpft Service

Sie können einen Azure Stapel verknüpft-Dienst, um einem Stapel Ressourcenpool von virtuellen Computern (virtuelle Computer) zu registrieren zu einer Factory Daten erstellen. Sie können .NET benutzerdefinierte Aktivitäten mit Azure Stapel oder Azure HDInsight ausführen.

Finden Sie unter folgenden Themen aus, wenn Sie neu bei Stapel Azure Service sind:


- [Grundlagen von Azure Stapel](../batch/batch-technical-overview.md) für einen Überblick über den Stapel Azure-Dienst.
- [Neu-AzureBatchAccount](https://msdn.microsoft.com/library/mt125880.aspx) -Cmdlet eine Stapel Azure-Konto (oder) [Azure-Portal](../batch/batch-account-create-portal.md) zum Erstellen des Kontos Azure Stapel mit Azure-Portal erstellt. Finden Sie unter [Verwenden von PowerShell Azure Stapel-Konto verwaltet](http://blogs.technet.com/b/windowshpc/archive/2014/10/28/using-azure-powershell-to-manage-azure-batch-account.aspx) ausführliche Anweisungen auf mithilfe des Cmdlets.
- [Neu-AzureBatchPool](https://msdn.microsoft.com/library/mt125936.aspx) -Cmdlet eine Azure Stapel Ressourcenpool zu erstellen.

### <a name="example"></a>Beispiel

    {
      "name": "AzureBatchLinkedService",
      "properties": {
        "type": "AzureBatch",
        "typeProperties": {
          "accountName": "<Azure Batch account name>",
          "accessKey": "<Azure Batch account key>",
          "poolName": "<Azure Batch pool name>",
          "linkedServiceName": "<Specify associated storage linked service reference here>"
        }
      }
    }

Anfügen von "**. < Regionsname**" auf den Namen Ihres Kontos Stapel für die Eigenschaft **Kontoname** . Beispiel:

            "accountName": "mybatchaccount.eastus"

Eine andere Möglichkeit ist dann, den BatchUri Endpunkt angeben, wie unten dargestellt.  

            "accountName": "adfteam",
            "batchUri": "https://eastus.batch.azure.com",

### <a name="properties"></a>Eigenschaften

Eigenschaft | Beschreibung | Erforderlich
-------- | ----------- | --------
Typ | Die Eigenschaft sollte **AzureBatch**festgelegt werden. | Ja
Kontoname | Der Name des Kontos Azure Stapel. | Ja
accessKey | Tastenkombination für das Stapel Azure-Konto. | Ja
Pool | Name der Ressourcenpool von virtuellen Computern. | Ja
linkedServiceName | Name der Azure-Speicher verknüpft Dienst diesen Dienst Azure Stapel verknüpft zugeordnet. Dieser verknüpfte Dienst wird für das staging Dateien erforderlich, um die Aktivität und speichern die Protokolle der Ausführung der Aktivität auszuführen verwendet. | Ja


## <a name="azure-machine-learning-linked-service"></a>Learning Azure Computer verknüpft Service

Erstellen Sie einen Azure maschinellen Learning verknüpft Dienst um einen Computer Learning Stapel bewerten Endpunkt zu einer Factory Daten zu registrieren.

### <a name="example"></a>Beispiel

    {
      "name": "AzureMLLinkedService",
      "properties": {
        "type": "AzureML",
        "typeProperties": {
          "mlEndpoint": "https://[batch scoring endpoint]/jobs",
          "apiKey": "<apikey>"
        }
      }
    }

### <a name="properties"></a>Eigenschaften

Eigenschaft | Beschreibung | Erforderlich
-------- | ----------- | --------
Typ | Die Typeigenschaft auf festgelegt sein sollte: **AzureML**. | Ja
mlEndpoint | Den Stapel bewerten URL. | Ja
apiKey | Des Modells veröffentlichten Arbeitsbereich-API. | Ja


## <a name="azure-data-lake-analytics-linked-service"></a>Azure-Daten Lake Analytics verknüpft Service
Erstellen Sie einen Dienst **Azure Daten dem Analytics** verknüpft zum Verknüpfen von einer Azure Daten dem Analytics Computing-Service mit einer Factory Azure-Daten, bevor Sie die [Daten dem Analytics U-SQL-Aktivität](data-factory-usql-activity.md) in einem Verkaufspipeline verwenden.

Im folgende Beispiel stellt JSON-Definition für einen Dienst Azure Daten dem Analytics verknüpft.

    {
        "name": "AzureDataLakeAnalyticsLinkedService",
        "properties": {
            "type": "AzureDataLakeAnalytics",
            "typeProperties": {
                "accountName": "adftestaccount",
                "dataLakeAnalyticsUri": "datalakeanalyticscompute.net",
                "authorization": "<authcode>",
                "sessionId": "<session ID>",
                "subscriptionId": "<subscription id>",
                "resourceGroupName": "<resource group name>"
            }
        }
    }


Die folgende Tabelle enthält eine Beschreibung für die Eigenschaften in der JSON-Definition verwendet werden.

Eigenschaft | Beschreibung | Erforderlich
-------- | ----------- | --------
Typ | Die Typeigenschaft auf festgelegt sein sollte: **AzureDataLakeAnalytics**. | Ja
Kontoname | Azure-Daten Lake Analytics-Kontoname. | Ja
dataLakeAnalyticsUri | Lake Analytics-URI Azure-Daten. |  Nein
Autorisierung | Autorisierungscode wird automatisch abgerufen, nach dem Klicken auf die Schaltfläche **Autorisieren** im Factory-Editor und das OAuth Login durchführen. | Ja
subscriptionId | Azure-Abonnement-id | Nein (wenn nicht angegeben, wird der Daten Factory Abonnement verwendet).
resourceGroupName | Gruppennamen Azure Ressource |  Nein (wenn nicht angegeben, wird der Daten Factory Ressourcengruppe verwendet).
sessionId | Sitzung-Id aus der OAuth Autorisierung Sitzung. Jede Id für eine Sitzung ist eindeutig und darf nur einmal verwendet werden. Dies ist in der Factory-Editor automatisch generiert. | Ja

Der Autorisierungscode, den Sie mithilfe der Schaltfläche **Autorisieren** generiert läuft ab nach einer Weile. In der folgenden Tabelle finden Sie die Ablaufzeiten für verschiedene Typen von Benutzerkonten. Wird den folgenden Fehler angezeigt Meldung an, wenn die Authentifizierung **Sicherheitstoken abläuft**: Vorgang Anmeldefehler: Invalid_grant - AADSTS70002: Fehler beim Überprüfen der Anmeldeinformationen. AADSTS70008: Die bereitgestellten Zugriff gewähren abgelaufen ist oder widerrufen. Spur-ID: d18629e8-af88-43c5-88e3-d8419eb1fca1 Korrelations-ID: fac30a0c-6be6-4e02-8d69-a776d2ffefd7 Zeitstempel: 2015-12-15 21:09:31Z

| Benutzertyp | Läuft ab nach |
| :-------- | :----------- | 
| Benutzerkonten, die nicht von Azure Active Directory verwaltet (@hotmail.com, @live.com, usw..) | 12 Stunden |
| Konten für Benutzer von Azure Active Directory (AAD) verwaltet | 14 Tage nach dem letzten Segment ausführen zu können. <br/><br/>90 Tage, wenn ein Segment basierend auf Verknüpfte Dienst OAuth-basierten alle 14 Tage mindestens einmal ausgeführt wird. |
 
Um zu vermeiden, / dieser Fehler beheben, müssen Sie autorisieren möchten, verwenden die **Autorisieren** Schaltfläche, wenn die **Sicherheitstoken abläuft** und die erneute Bereitstellung verknüpfter Dienst. Sie können auch die Werte für SessionId und Autorisierung Eigenschaften programmgesteuert im folgenden Abschnitt mit Code generieren. 

### <a name="to-programmatically-generate-sessionid-and-authorization-values"></a>Programmgesteuert SessionId und Autorisierung Werte generieren 
Der folgende Code generiert **SessionId** und **Autorisierung** Werte.  

    if (linkedService.Properties.TypeProperties is AzureDataLakeStoreLinkedService ||
        linkedService.Properties.TypeProperties is AzureDataLakeAnalyticsLinkedService)
    {
        AuthorizationSessionGetResponse authorizationSession = this.Client.OAuth.Get(this.ResourceGroupName, this.DataFactoryName, linkedService.Properties.Type);

        WindowsFormsWebAuthenticationDialog authenticationDialog = new WindowsFormsWebAuthenticationDialog(null);
        string authorization = authenticationDialog.AuthenticateAAD(authorizationSession.AuthorizationSession.Endpoint, new Uri("urn:ietf:wg:oauth:2.0:oob"));

        AzureDataLakeStoreLinkedService azureDataLakeStoreProperties = linkedService.Properties.TypeProperties as AzureDataLakeStoreLinkedService;
        if (azureDataLakeStoreProperties != null)
        {
            azureDataLakeStoreProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
            azureDataLakeStoreProperties.Authorization = authorization;
        }

        AzureDataLakeAnalyticsLinkedService azureDataLakeAnalyticsProperties = linkedService.Properties.TypeProperties as AzureDataLakeAnalyticsLinkedService;
        if (azureDataLakeAnalyticsProperties != null)
        {
            azureDataLakeAnalyticsProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
            azureDataLakeAnalyticsProperties.Authorization = authorization;
        }
    }

Finden Sie Details zu den Daten Factory-Klassen im Code verwendeten [AzureDataLakeStoreLinkedService Class](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx), [AzureDataLakeAnalyticsLinkedService Class](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx)und [AuthorizationSessionGetResponse Klasse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx) Themen. Sie müssen einen Verweis hinzufügen: Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll für die WindowsFormsWebAuthenticationDialog-Klasse. 
 

## <a name="azure-sql-linked-service"></a>SQL Azure verknüpft Service
Erstellen einen SQL Azure-Verknüpfte Dienst, und verwenden es mit der [Gespeicherten Prozedur für die Aktivität](data-factory-stored-proc-activity.md) , um eine gespeicherte Prozedur aus einer Data Factory Verkaufspipeline aufzurufen. Finden Sie im Artikel Details zu diesen Dienst verknüpften [Azure SQL-Verbinder](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties) .

## <a name="azure-sql-data-warehouse-linked-service"></a>SQL Azure Data Warehouse-Dienst verknüpfte
Erstellen ein verknüpft Data Warehouse mit SQL Azure-Diensts, und verwenden es mit der [Gespeicherten Prozedur für die Aktivität](data-factory-stored-proc-activity.md) , um eine gespeicherte Prozedur aus einer Data Factory Verkaufspipeline aufzurufen. Finden Sie im Artikel Details zu diesen Dienst verknüpften [Azure SQL Data Warehouse Connector](data-factory-azure-sql-data-warehouse-connector.md#azure-sql-data-warehouse-linked-service-properties) .

## <a name="sql-server-linked-service"></a>SQLServer verknüpft Service
Erstellen einen verknüpfte SQL-Server-Dienst, und verwenden es mit der [Gespeicherten Prozedur für die Aktivität](data-factory-stored-proc-activity.md) , um eine gespeicherte Prozedur aus einer Data Factory Verkaufspipeline aufzurufen. [SQL Server-Connector](data-factory-sqlserver-connector.md#sql-server-linked-service-properties) finden Sie im Artikel Details zu diesen Dienst verknüpften.

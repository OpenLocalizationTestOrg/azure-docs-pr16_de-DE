<properties 
    pageTitle="Daten Factory - API Änderungsprotokoll für .NET | Microsoft Azure" 
    description="Beschreibt grundlegende Änderungen, die zusätzlichen Features beschrieben, die Updates usw.... in einer bestimmten Version von .NET API für die Azure-Daten Factory." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/21/2016" 
    ms.author="spelluru"/>

# <a name="azure-data-factory---net-api-change-log"></a>Azure Daten Factory - API .NET Änderungsprotokoll 
Dieser Artikel enthält Informationen zu Azure Data Factory SDK in einer bestimmten Version. Finden Sie das neueste NuGet-Paket für Azure Daten Factory [hier](https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactories) 

## <a name="version-4110"></a>Version 4.11.0
Zusätzlichen Features beschrieben:

- Die folgenden Arten von verknüpften Dienst wurden hinzugefügt:
    - [OnPremisesMongoDbLinkedService](https://msdn.microsoft.com/library/mt765129.aspx)
    - [AmazonRedshiftLinkedService](https://msdn.microsoft.com/library/mt765121.aspx)
    - [AwsAccessKeyLinkedService](https://msdn.microsoft.com/library/mt765144.aspx)
- Die folgenden Arten von Dataset wurden hinzugefügt: 
    - [MongoDbCollectionDataset](https://msdn.microsoft.com/library/mt765145.aspx)
    - [AmazonS3Dataset](https://msdn.microsoft.com/library/mt765112.aspx)
- Die folgenden Arten von kopieren Quelle wurden hinzugefügt:
    - [MongoDbSource](https://msdn.microsoft.com/en-US/library/mt765123.aspx)

## <a name="version-4100"></a>Version 4.10.0
- Die folgenden optionalen Eigenschaften wurden TextFormat hinzugefügt:
    - [SkipLineCount](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.textformat.skiplinecount.aspx)
    - [FirstRowAsHeader](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.textformat.firstrowasheader.aspx)
    - [TreatEmptyAsNull](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.textformat.treatemptyasnull.aspx)
- Die folgenden Arten von verknüpften Dienst wurden hinzugefügt:
    - [OnPremisesCassandraLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisescassandralinkedservice.aspx)
    - [SalesforceLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.salesforcelinkedservice.aspx)
- Die folgenden Arten von Dataset wurden hinzugefügt:
    - [OnPremisesCassandraTableDataset](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisescassandratabledataset.aspx)
- Die folgenden Arten von kopieren Quelle wurden hinzugefügt:
    - [CassandraSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.cassandrasource.aspx)
- Hinzufügen von [WebServiceInputs](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.azuremlbatchexecutionactivity.webserviceinputs.aspx) Eigenschaft zu AzureMLBatchExecutionActivity 
    - Aktivieren Sie Übergabe mehrere Web Service Eingaben zu einem Versuch Azure maschinellen Schulung


## <a name="version-491"></a>Version 4.9.1

### <a name="bug-fix"></a>Fehlerkorrektur

- Deaktivieren Sie WebApi basierende Authentifizierung für [WebLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.weblinkedservice.authenticationtype.aspx).

## <a name="version-490"></a>Version 4.9.0

### <a name="feature-additions"></a>Leistungsmerkmale

- CopyActivity [EnableStaging](https://msdn.microsoft.com/library/mt767916.aspx) und [StagingSettings](https://msdn.microsoft.com/library/mt767918.aspx) Eigenschaften hinzufügen. Die Funktion Details finden Sie unter [' bereitgestellt ' kopieren](data-factory-copy-activity-performance.md#staged-copy) .


### <a name="bug-fix"></a>Fehlerkorrektur

- Stellen Sie eine Überladung der [ActivityWindowOperationExtensions.List](https://msdn.microsoft.com/library/mt767915.aspx) -Methode, die eine Instanz [ActivityWindowsByActivityListParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.activitywindowsbyactivitylistparameters.aspx) akzeptiert vor.
- Markieren Sie die [WriteBatchSize](https://msdn.microsoft.com/library/dn884293.aspx) und [WriteBatchTimeout](https://msdn.microsoft.com/library/dn884245.aspx) in CopySink als optional.

## <a name="version-480"></a>Version 4.8.0

### <a name="feature-additions"></a>Leistungsmerkmale
- Die folgenden optionalen Eigenschaften wurden kopieren Aktivitätstyp Aktivieren der Leistung optimieren hinzugefügt:
    - [ParallelCopies](https://msdn.microsoft.com/library/mt767910.aspx)
    - [CloudDataMovementUnits](https://msdn.microsoft.com/library/mt767912.aspx)

## <a name="version-470"></a>Version 4.7.0

### <a name="feature-additions"></a>Leistungsmerkmale
* Neuen StorageFormat Typ [OrcFormat](https://msdn.microsoft.com/library/mt723391.aspx) zum Kopieren von Dateien im optimierten Zeile einspaltigen (ORC) Format wird hinzugefügt.
* SqlDWSink [AllowPolyBase](https://msdn.microsoft.com/library/mt723396.aspx) und PolyBaseSettings Eigenschaften hinzufügen.
    * Ermöglicht die Verwendung von PolyBase zum Kopieren von Daten in SQL Data Warehouse.

## <a name="version-461"></a>Version 4.6.1

### <a name="bug-fixes"></a>Updates
* Behebt HTTP-Anforderung für die Aktivität Windows an.
    * Gruppe den Namen der Ressource und der Daten Factory-Name entfernt der Anforderungsnutzlast.

## <a name="version-460"></a>Version 4.6.0

### <a name="feature-additions"></a>Leistungsmerkmale

- Die folgenden Eigenschaften haben [PipelineProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties_properties.aspx)hinzugefügt wurde:
    - [PipelineMode](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties.pipelinemode.aspx)
    - [ExpirationTime](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties.expirationtime.aspx)
    - [Datasets](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties.datasets.aspx)
- Die folgenden Eigenschaften haben [PipelineRuntimeInfo](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.common.models.pipelineruntimeinfo.aspx)hinzugefügt wurde:
    - [PipelineState](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.common.models.pipelineruntimeinfo.pipelinestate.aspx)
- Geben Sie hinzugefügten neuen [StorageFormat](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.storageformat.aspx) [JsonFormat](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.jsonformat.aspx) Typ Datasets definieren, deren Daten im JSON-Format ist, ein. 

## <a name="version-450"></a>Version 4.5.0

### <a name="feature-additions"></a>Leistungsmerkmale
* Hinzugefügten [Liste Vorgänge für Aktivitätsfenster](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.activitywindowoperationsextensions.aspx).
    * Hinzugefügte Methoden zum Abrufen von Aktivität Windows mit Filter auf Grundlage der Entitätstypen (d. h., Daten Factory, Datasets, Pipelines und Aktivitäten).
* Die folgenden Arten von verknüpften Dienst wurden hinzugefügt: 
    * [ODataLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.odatalinkedservice.aspx), [WebLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.weblinkedservice.aspx)
* Die folgenden Arten von Dataset wurden hinzugefügt: 
    * [ODataResourceDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.odataresourcedataset.aspx), [WebTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.webtabledataset.aspx)
* Die folgenden Arten von kopieren Quelle wurden hinzugefügt:  
    * [WebSource](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.websource.aspx)

## <a name="version-440"></a>Version 4.4.0

### <a name="feature-additions"></a>Leistungsmerkmale

- Der folgenden verknüpfte Diensttyp als Datenquellen hinzugefügt wurde, und senken für Aktivitäten kopieren:
    - [AzureStorageSasLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.azurestoragesaslinkedservice.aspx). Grundlegende Informationen und Beispiele finden Sie unter [Azure SAS verknüpfte Speicherdienst](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service) . 

## <a name="version-430"></a>Version 4.3.0

### <a name="feature-additions"></a>Leistungsmerkmale

- Die folgenden Arten verknüpfte Dienst noch wurde als Datenquellen für Kopieren Aktivitäten hinzugefügt:
    - [HdfsLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.hdfslinkedservice.aspx). Finden Sie unter [Verschieben von Daten aus HDFS mit Daten Factory](data-factory-hdfs-connector.md) für grundlegende Informationen und Beispiele. 
    - [OnPremisesOdbcLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisesodbclinkedservice.aspx). Finden Sie unter [Verschieben von Daten aus ODBC-Daten gespeichert sind, mithilfe von Azure Data Factory](data-factory-odbc-connector.md) für grundlegende Informationen und Beispiele. 

## <a name="version-420"></a>Version 4.2.0 müssen

### <a name="feature-additions"></a>Leistungsmerkmale

- Der folgenden neuen Aktivitätstyp wurde hinzugefügt: [AzureMLUpdateResourceActivity](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremlupdateresourceactivity.aspx). Details zur Aktivität finden Sie unter [Aktualisieren Azure ML Modelle mithilfe der Ressource-Aktivität aktualisieren](data-factory-azure-ml-batch-execution-activity.md#updating-azure-ml-models-using-the-update-resource-activity).
- Eine neue optionale Eigenschaft [UpdateResourceEndpoint](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremllinkedservice.updateresourceendpoint.aspx) wurde in der [Klasse AzureMLLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremllinkedservice.aspx)hinzugefügt. 
- Eigenschaften [LongRunningOperationInitialTimeout](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.longrunningoperationinitialtimeout.aspx) und [LongRunningOperationRetryTimeout](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.longrunningoperationretrytimeout.aspx) wurden in der Klasse [DataFactoryManagementClient](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.aspx) hinzugefügt. 
- Konfiguration von der Zeitlimit für Client Anrufe an den Daten Factory-Dienst zulassen. 


## <a name="version-410"></a>Version 4.1.0

### <a name="feature-additions"></a>Leistungsmerkmale
* Die folgenden Arten von verknüpften Dienst wurden hinzugefügt: 
    * [AzureDataLakeStoreLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx)
    * [AzureDataLakeAnalyticsLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx)
* Die folgenden Arten von Aktivitäten wurden hinzugefügt: 
    * [DataLakeAnalyticsUSQLActivity](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datalakeanalyticsusqlactivity.aspx)
* Die folgenden Arten von Dataset wurden hinzugefügt: 
    * [AzureDataLakeStoreDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoredataset.aspx)
* Die folgenden Arten von Quelle und Empfänger zum Kopieren Aktivität wurden hinzugefügt:
    * [AzureDataLakeStoreSource](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoresource.aspx)
    * [AzureDataLakeStoreSink](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoresink.aspx)


## <a name="version-401"></a>Version 4.0.1

### <a name="breaking-changes"></a>Unterbrechen der Änderungen
Die folgenden Klassen wurden umbenannt. Die neuen Namen wurden die ursprünglichen Namen von Klassen, bevor 4.0.0 freigeben. 
 
Benennen Sie 4.0.0 | Benennen Sie 4.0.1
:------------ | :------------ 
AzureSqlDataWarehouseDataset | [AzureSqlDataWarehouseTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuresqldatawarehousetabledataset.aspx)
AzureSqlDataset | [AzureSqlTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuresqltabledataset.aspx)
AzureDataset | [AzureTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuretabledataset.aspx)
OracleDataset | [OracleTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.oracletabledataset.aspx)
RelationalDataset | [RelationalTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.relationaltabledataset.aspx)
SqlServerDataset | [SqlServerTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.sqlservertabledataset.aspx)


## <a name="version-400"></a>Version 4.0.0

### <a name="breaking-changes"></a>Unterbrechen der Änderungen



- Die folgenden Klassen-Schnittstellen wurden umbenannt.

| Alter name | Neuer name |
| :-------- | :-------- |
| ITableOperations | [IDatasetOperations](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.idatasetoperations.aspx) |  
| Tabelle | [DataSet](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.dataset.aspx) | 
| TableProperties | [DatasetProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetproperties.aspx) | 
| TableTypeProprerties | [DatasetTypeProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasettypeproperties.aspx) |
| TableCreateOrUpdateParameters | [DatasetCreateOrUpdateParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdateparameters.aspx) | 
| TableCreateOrUpdateResponse | [DatasetCreateOrUpdateResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdateresponse.aspx) | 
| TableGetResponse | [DatasetGetResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetgetresponse.aspx) | 
| TableListResponse | [DatasetListResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetlistresponse.aspx) |
| CreateOrUpdateWithRawJsonContentParameters | [DatasetCreateOrUpdateWithRawJsonContentParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdatewithrawjsoncontentparameters.aspx) | 
    

- Die **Liste** Methoden geben jetzt seitenweise Ergebnisse zurück. Wenn die Antwort auf eine nicht leere **NextLink** Eigenschaft enthält, muss die Clientanwendung fortsetzen die nächste Seite das Abrufen, bis alle Seiten zurückgegeben werden.  Hier ist ein Beispiel: 

        PipelineListResponse response = client.Pipelines.List("ResourceGroupName", "DataFactoryName");
        var pipelines = new List<Pipeline>(response.Pipelines);
    
        string nextLink = response.NextLink;
        while (string.IsNullOrEmpty(response.NextLink))
        {
            PipelineListResponse nextResponse = client.Pipelines.ListNext(nextLink);
            pipelines.AddRange(nextResponse.Pipelines);
    
            nextLink = nextResponse.NextLink;
        }
    
- **Liste** Verkaufspipeline API gibt nur die Zusammenfassung der eine Verkaufspipeline nicht mit vollständigen Details. Beispielsweise enthalten Aktivitäten in einer Zusammenfassung Verkaufspipeline nur Name und Typ.

### <a name="feature-additions"></a>Leistungsmerkmale
- Die [SqlDWSink](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqldwsink.aspx) -Klasse unterstützt zwei neue Eigenschaften **SliceIdentifierColumnName** und **SqlWriterCleanupScript**, Idempotent kopieren in Azure SQL-Data Warehouse unterstützt. Finden Sie im Artikel [Azure SQL-Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md) , insbesondere die [Verfahren 1](data-factory-azure-sql-data-warehouse-connector.md#mechanism-1) und [2 Verfahren](data-factory-azure-sql-data-warehouse-connector.md#mechanism-2) Abschnitte, Details zu diesen Eigenschaften aus.

- Wir unterstützen nun Ausführen der gespeicherten Prozedur für SQL Azure-Datenbank und Azure SQL-Data Warehouse Datenquellen als Teil der Aktivität kopieren. Die [SQLSource aus](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqlsource.aspx) und [SqlDWSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqldwsource.aspx) Klassen haben folgenden Eigenschaften: **SqlReaderStoredProcedureName** und **StoredProcedureParameters**. Weitere Informationen zu diesen Eigenschaften finden Sie unter den Artikeln [Azure SQL-Datenbank](data-factory-azure-sql-connector.md#sqlsource) und [Azure SQL-Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#sqldwsource) auf Azure.com.  
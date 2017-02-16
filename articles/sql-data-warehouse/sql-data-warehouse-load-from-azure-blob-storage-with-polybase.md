<properties
   pageTitle="Laden Sie Daten aus Azure Blob-Speicher in SQL Data Warehouse (PolyBase) | Microsoft Azure"
   description="Erfahren Sie, wie PolyBase verwenden, um Daten aus Azure Blob-Speicher in SQL Data Warehouse zu laden. Einige Tabellen aus öffentlichen Daten in das Schema "Contoso" Retail Data Warehouse zu laden."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="ckarst"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/25/2016"
   ms.author="cakarst;barbkess;sonyama"/>


# <a name="load-data-from-azure-blob-storage-into-sql-data-warehouse-polybase"></a>Laden Sie Daten aus Azure Blob-Speicher in SQL Data Warehouse (PolyBase)

> [AZURE.SELECTOR]
- [Daten Factory](sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md)
- [PolyBase](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md)

Verwenden Sie PolyBase und T-SQL-Befehle zum Laden von Daten aus Azure Blob-Speicher in Azure SQL-Data Warehouse. 

Wenn Sie um einfach zu halten, lädt dieses Lernprogramms aus öffentlichen Azure Blob-Speicher zwei Tabellen in das Schema "Contoso" Retail Data Warehouse an. Zum Laden sämtlicher Daten führen Sie das Beispiel, [Laden die vollständige Contoso Retail Data Warehouse][] aus dem Microsoft SQL Server-Beispiele Repository aus.

In diesem Lernprogramm werden Sie folgende Aufgaben ausführen:

1. Konfigurieren von Azure Blob-Speicher geladen PolyBase
2. Laden von öffentlichen Daten in Ihrer Datenbank
3. Führen Sie nach Abschluss die laden Optimierungen an.


## <a name="before-you-begin"></a>Vorbemerkung
Zum Ausführen dieses Lernprogramms benötigen Sie ein Azure-Konto, die bereits eine SQL Data Warehouse-Datenbank. Wenn Sie dies bereits besitzen, finden Sie unter [Erstellen einer SQL Data Warehouse][].

## <a name="1-configure-the-data-source"></a>1 die Datenquelle konfigurieren

PolyBase verwendet externe T-SQL-Objekte, um den Speicherort und die Attribute der externen Daten definieren. Die externen Objektdefinitionen werden in SQL Data Warehouse gespeichert. Die Daten selbst ist extern gespeichert.

### <a name="11-create-a-credential"></a>1.1. Erstellen Sie einen Satz von Anmeldeinformationen

Sie **diesen Schritt überspringen** , wenn Sie die Contoso öffentlichen Daten geladen sind. Sie benötigen keine sicheren Zugriff auf die öffentlichen Daten, da es bereits allen zugänglich ist.

**Überspringen Sie diesen Schritt nicht** , wenn Sie in diesem Lernprogramm als Vorlage verwenden für Ihre eigenen Daten geladen. Verwenden Sie das folgende Skript zum Erstellen einer Datenbank ausgelegte Anmeldeinformationen für den Zugriff auf Daten über einen Eintrag, und verwenden sie dann, wenn Sie den Speicherort der Datenquelle definieren.


```sql
-- A: Create a master key.
-- Only necessary if one does not already exist.
-- Required to encrypt the credential secret in the next step.

CREATE MASTER KEY;


-- B: Create a database scoped credential
-- IDENTITY: Provide any string, it is not used for authentication to Azure storage.
-- SECRET: Provide your Azure storage account key.


CREATE DATABASE SCOPED CREDENTIAL AzureStorageCredential
WITH
    IDENTITY = 'user',
    SECRET = '<azure_storage_account_key>'
;


-- C: Create an external data source
-- TYPE: HADOOP - PolyBase uses Hadoop APIs to access data in Azure blob storage.
-- LOCATION: Provide Azure storage account name and blob container name.
-- CREDENTIAL: Provide the credential created in the previous step.

CREATE EXTERNAL DATA SOURCE AzureStorage
WITH (
    TYPE = HADOOP,
    LOCATION = 'wasbs://<blob_container_name>@<azure_storage_account_name>.blob.core.windows.net',
    CREDENTIAL = AzureStorageCredential
);
```

Fahren Sie mit Schritt 2 fort.

### <a name="12-create-the-external-data-source"></a>1.2. Erstellen der externen Datenquelle

Verwenden Sie diesen Befehl [Externe Datenquelle erstellen][] , um den Speicherort der Daten und den Typ der Daten zu speichern. 

```sql
CREATE EXTERNAL DATA SOURCE AzureStorage_west_public
WITH 
(  
    TYPE = Hadoop 
,   LOCATION = 'wasbs://contosoretaildw-tables@contosoretaildw.blob.core.windows.net/'
); 
```

> [AZURE.IMPORTANT] Wenn Sie auswählen, dass Ihre Azure Blob Storage Container öffentlich sichtbar, denken Sie daran, dass als Datenbesitzer Sie Daten Ausgang Gebühren belastet werden, wenn Daten das Data Center verlässt. 

## <a name="2-configure-data-format"></a>2. Datenformat konfigurieren

Die Daten in Textdateien in Azure Blob-Speicher gespeichert ist, und jedes Feld ist durch ein Trennzeichen getrennt. Führen Sie diesen Befehl [Externe DATEIFORMAT][] das Format der Daten in der Textdateien an. Die Contoso-Daten sind nicht komprimiert und senkrechter Strich getrennt.

```sql
CREATE EXTERNAL FILE FORMAT TextFileFormat 
WITH 
(   FORMAT_TYPE = DELIMITEDTEXT
,   FORMAT_OPTIONS  (   FIELD_TERMINATOR = '|'
                    ,   STRING_DELIMITER = ''
                    ,   DATE_FORMAT      = 'yyyy-MM-dd HH:mm:ss.fff'
                    ,   USE_TYPE_DEFAULT = FALSE 
                    )
);
``` 

## <a name="3-create-the-external-tables"></a>3 die externen Tabellen erstellen

Jetzt, da Sie das Datenformat der Quell- und Datei angegeben haben, sind Sie bereit sind, die externen Tabellen erstellen. 

### <a name="31-create-a-schema-for-the-data"></a>3.1. Erstellen Sie ein Schema für die Daten ein. 

Wenn Sie um einen Speicherort zum Speichern der Contoso-Daten in einer Datenbank zu erstellen, erstellen Sie ein Schema aus.

```sql
CREATE SCHEMA [asb]
GO
```

### <a name="32-create-the-external-tables"></a>3,2. Erstellen Sie die externen Tabellen. 

Führen Sie dieses Skript zum Erstellen der DimProduct und FactOnlineSales externen Tabellen. Alle hier tun ist Spaltennamen und Datentypen definieren und Bindung an den Speicherort und das Format der Dateien Azure Blob-Speicher. Die Definition wird in SQL Data Warehouse gespeichert und die Daten weiterhin in Azure Blob-Speicher.

Der Parameter **Speicherort** ist den Ordner unter dem Stammordner im Azure Blob-Speicher. Jede Tabelle ist in einem anderen Ordner.


```sql

--DimProduct
CREATE EXTERNAL TABLE [asb].DimProduct (
    [ProductKey] [int] NOT NULL,
    [ProductLabel] [nvarchar](255) NULL,
    [ProductName] [nvarchar](500) NULL,
    [ProductDescription] [nvarchar](400) NULL,
    [ProductSubcategoryKey] [int] NULL,
    [Manufacturer] [nvarchar](50) NULL,
    [BrandName] [nvarchar](50) NULL,
    [ClassID] [nvarchar](10) NULL,
    [ClassName] [nvarchar](20) NULL,
    [StyleID] [nvarchar](10) NULL,
    [StyleName] [nvarchar](20) NULL,
    [ColorID] [nvarchar](10) NULL,
    [ColorName] [nvarchar](20) NOT NULL,
    [Size] [nvarchar](50) NULL,
    [SizeRange] [nvarchar](50) NULL,
    [SizeUnitMeasureID] [nvarchar](20) NULL,
    [Weight] [float] NULL,
    [WeightUnitMeasureID] [nvarchar](20) NULL,
    [UnitOfMeasureID] [nvarchar](10) NULL,
    [UnitOfMeasureName] [nvarchar](40) NULL,
    [StockTypeID] [nvarchar](10) NULL,
    [StockTypeName] [nvarchar](40) NULL,
    [UnitCost] [money] NULL,
    [UnitPrice] [money] NULL,
    [AvailableForSaleDate] [datetime] NULL,
    [StopSaleDate] [datetime] NULL,
    [Status] [nvarchar](7) NULL,
    [ImageURL] [nvarchar](150) NULL,
    [ProductURL] [nvarchar](150) NULL,
    [ETLLoadID] [int] NULL,
    [LoadDate] [datetime] NULL,
    [UpdateDate] [datetime] NULL
)
WITH
(
    LOCATION='/DimProduct/' 
,   DATA_SOURCE = AzureStorage_west_public
,   FILE_FORMAT = TextFileFormat
,   REJECT_TYPE = VALUE
,   REJECT_VALUE = 0
)
;
 
--FactOnlineSales
CREATE EXTERNAL TABLE [asb].FactOnlineSales 
(
    [OnlineSalesKey] [int]  NOT NULL,
    [DateKey] [datetime] NOT NULL,
    [StoreKey] [int] NOT NULL,
    [ProductKey] [int] NOT NULL,
    [PromotionKey] [int] NOT NULL,
    [CurrencyKey] [int] NOT NULL,
    [CustomerKey] [int] NOT NULL,
    [SalesOrderNumber] [nvarchar](20) NOT NULL,
    [SalesOrderLineNumber] [int] NULL,
    [SalesQuantity] [int] NOT NULL,
    [SalesAmount] [money] NOT NULL,
    [ReturnQuantity] [int] NOT NULL,
    [ReturnAmount] [money] NULL,
    [DiscountQuantity] [int] NULL,
    [DiscountAmount] [money] NULL,
    [TotalCost] [money] NOT NULL,
    [UnitCost] [money] NULL,
    [UnitPrice] [money] NULL,
    [ETLLoadID] [int] NULL,
    [LoadDate] [datetime] NULL,
    [UpdateDate] [datetime] NULL
)
WITH
(
    LOCATION='/FactOnlineSales/' 
,   DATA_SOURCE = AzureStorage_west_public
,   FILE_FORMAT = TextFileFormat
,   REJECT_TYPE = VALUE
,   REJECT_VALUE = 0
)
;
```

## <a name="4-load-the-data"></a>4 Laden Sie 4 die Daten
Es gibt verschiedene Arten Zugriff auf externe Daten.  Sie können Daten direkt aus der externen Tabelle Abfragen, laden Sie die Daten in einer neuen Datenbanktabellen oder Hinzufügen von externen Daten zu vorhandenen Datenbanktabellen.  


### <a name="41-create-a-new-schema"></a>4.1. Erstellen Sie ein neues schema

CTAS erstellt eine neue Tabelle, die Daten enthält.  Erstellen Sie zuerst ein Schema für die Contoso-Daten ein.

```sql
CREATE SCHEMA [cso]
GO
```

### <a name="42-load-the-data-into-new-tables"></a>4.2. Laden Sie die Daten in die neuen Tabellen

Verwenden Sie zum Laden von Daten aus Azure Blob-Speicher, und speichern Sie sie in einer Tabelle innerhalb der Datenbank, die Anweisung [CREATE TABLE AS SELECT (Transact-SQL)][] ein. Laden mit CTAS nutzt die stark eingegebenen externen Tabellen, die Sie soeben erstellt haben. Verwenden Sie zum Laden der Daten in die neuen Tabellen pro Tabelle eine [CTAS][] -Anweisung ein. 

CTAS erstellt eine neue Tabelle und füllt sie mit den Ergebnissen einer select-Anweisung. CTAS definiert die neue Tabelle, damit die gleichen Spalten und Datentypen als die Ergebnisse der select-Anweisung. Wenn Sie alle Spalten aus einer externen Tabelle auswählen, werden die neue Tabelle eine Kopie der Spalten und Datentypen in der externen Tabelle.

In diesem Beispiel erstellen wir sowohl die Dimension und der Faktentabelle als Hashing Tabellen verteilte. 


```sql
SELECT GETDATE();
GO

CREATE TABLE [cso].[DimProduct]            WITH (DISTRIBUTION = HASH([ProductKey]  ) ) AS SELECT * FROM [asb].[DimProduct]             OPTION (LABEL = 'CTAS : Load [cso].[DimProduct]             ');
CREATE TABLE [cso].[FactOnlineSales]       WITH (DISTRIBUTION = HASH([ProductKey]  ) ) AS SELECT * FROM [asb].[FactOnlineSales]        OPTION (LABEL = 'CTAS : Load [cso].[FactOnlineSales]        ');
```

### <a name="43-track-the-load-progress"></a>4.3 überwachen Sie laden

Sie können Ihre Last mithilfe von dynamischen Management-Ansichten (DMVs) überwachen. 

```sql
-- To see all requests
SELECT * FROM sys.dm_pdw_exec_requests;

-- To see a particular request identified by its label
SELECT * FROM sys.dm_pdw_exec_requests as r;
WHERE r.[label] = 'CTAS : Load [cso].[DimProduct]             '
      OR r.[label] = 'CTAS : Load [cso].[FactOnlineSales]        '
;

-- To track bytes and files
SELECT
    r.command,
    s.request_id,
    r.status,
    count(distinct input_name) as nbr_files, 
    sum(s.bytes_processed)/1024/1024 as gb_processed
FROM
    sys.dm_pdw_exec_requests r
    inner join sys.dm_pdw_dms_external_work s
        on r.request_id = s.request_id
WHERE 
    r.[label] = 'CTAS : Load [cso].[DimProduct]             '
    OR r.[label] = 'CTAS : Load [cso].[FactOnlineSales]        '
GROUP BY
    r.command,
    s.request_id,
    r.status
ORDER BY
    nbr_files desc,
    gb_processed desc;
```

## <a name="5-optimize-columnstore-compression"></a>5. Columnstore Komprimierung optimieren

Standardmäßig speichert SQL Data Warehouse die Tabelle als gruppierte Columnstore Index. Nach Abschluss einer laden möglicherweise einige der Datenzeilen nicht in der Columnstore komprimiert werden soll.  Es gibt verschiedene Gründe, warum dies geschieht. Weitere Informationen finden Sie unter [Verwalten von Columnstore Indizes][].

Um Leistung von Abfragen und Columnstore Komprimierung nach einer Auslastung optimieren, neu erstellen Sie die Tabelle, um alle Zeilen komprimieren Columnstore Indexes erzwingen aus. 

```sql
SELECT GETDATE();
GO

ALTER INDEX ALL ON [cso].[DimProduct]               REBUILD;
ALTER INDEX ALL ON [cso].[FactOnlineSales]          REBUILD;
```

Weitere Informationen zum Beibehalten von Columnstore Indizes finden Sie im Artikel [Columnstore Indizes verwalten][] .

## <a name="6-optimize-statistics"></a>6. Statistiken optimieren

Es empfiehlt sich, einspaltige Statistiken unmittelbar nach einer Last zu erstellen. Es gibt einige Optionen für die Statistik. Wenn Sie eine einspaltige Statistiken auf jeder Spalte erstellen möglicherweise es sehr viel Zeit in alle statistischen Quelltabelle dauern. Wenn Sie, dass bestimmte Spalten in Abfrage Prädikate werden nicht vertraut sind wissen, können Sie Spalten erstellen Statistiken überspringen.

Wenn Sie eine einspaltige Statistiken für jede Spalte jeder Tabelle erstellen möchten, können Sie die gespeicherten Prozedur Code Stichprobe `prc_sqldw_create_stats` im Artikel [Statistik][] .

Im folgende Beispiel ist ein guter Ausgangspunkt zum Erstellen von Statistiken. Jede Spalte in der Dimensionstabelle und auf jede zu verknüpfende Spalte in den Faktentabellen erstellt einspaltige Statistiken. Sie können andere Spalten der Faktentabelle immer höher Statistiken für einzelne oder mehrere Spalte hinzufügen.


```sql
CREATE STATISTICS [stat_cso_DimProduct_AvailableForSaleDate] ON [cso].[DimProduct]([AvailableForSaleDate]);
CREATE STATISTICS [stat_cso_DimProduct_BrandName] ON [cso].[DimProduct]([BrandName]);
CREATE STATISTICS [stat_cso_DimProduct_ClassID] ON [cso].[DimProduct]([ClassID]);
CREATE STATISTICS [stat_cso_DimProduct_ClassName] ON [cso].[DimProduct]([ClassName]);
CREATE STATISTICS [stat_cso_DimProduct_ColorID] ON [cso].[DimProduct]([ColorID]);
CREATE STATISTICS [stat_cso_DimProduct_ColorName] ON [cso].[DimProduct]([ColorName]);
CREATE STATISTICS [stat_cso_DimProduct_ETLLoadID] ON [cso].[DimProduct]([ETLLoadID]);
CREATE STATISTICS [stat_cso_DimProduct_ImageURL] ON [cso].[DimProduct]([ImageURL]);
CREATE STATISTICS [stat_cso_DimProduct_LoadDate] ON [cso].[DimProduct]([LoadDate]);
CREATE STATISTICS [stat_cso_DimProduct_Manufacturer] ON [cso].[DimProduct]([Manufacturer]);
CREATE STATISTICS [stat_cso_DimProduct_ProductDescription] ON [cso].[DimProduct]([ProductDescription]);
CREATE STATISTICS [stat_cso_DimProduct_ProductKey] ON [cso].[DimProduct]([ProductKey]);
CREATE STATISTICS [stat_cso_DimProduct_ProductLabel] ON [cso].[DimProduct]([ProductLabel]);
CREATE STATISTICS [stat_cso_DimProduct_ProductName] ON [cso].[DimProduct]([ProductName]);
CREATE STATISTICS [stat_cso_DimProduct_ProductSubcategoryKey] ON [cso].[DimProduct]([ProductSubcategoryKey]);
CREATE STATISTICS [stat_cso_DimProduct_ProductURL] ON [cso].[DimProduct]([ProductURL]);
CREATE STATISTICS [stat_cso_DimProduct_Size] ON [cso].[DimProduct]([Size]);
CREATE STATISTICS [stat_cso_DimProduct_SizeRange] ON [cso].[DimProduct]([SizeRange]);
CREATE STATISTICS [stat_cso_DimProduct_SizeUnitMeasureID] ON [cso].[DimProduct]([SizeUnitMeasureID]);
CREATE STATISTICS [stat_cso_DimProduct_Status] ON [cso].[DimProduct]([Status]);
CREATE STATISTICS [stat_cso_DimProduct_StockTypeID] ON [cso].[DimProduct]([StockTypeID]);
CREATE STATISTICS [stat_cso_DimProduct_StockTypeName] ON [cso].[DimProduct]([StockTypeName]);
CREATE STATISTICS [stat_cso_DimProduct_StopSaleDate] ON [cso].[DimProduct]([StopSaleDate]);
CREATE STATISTICS [stat_cso_DimProduct_StyleID] ON [cso].[DimProduct]([StyleID]);
CREATE STATISTICS [stat_cso_DimProduct_StyleName] ON [cso].[DimProduct]([StyleName]);
CREATE STATISTICS [stat_cso_DimProduct_UnitCost] ON [cso].[DimProduct]([UnitCost]);
CREATE STATISTICS [stat_cso_DimProduct_UnitOfMeasureID] ON [cso].[DimProduct]([UnitOfMeasureID]);
CREATE STATISTICS [stat_cso_DimProduct_UnitOfMeasureName] ON [cso].[DimProduct]([UnitOfMeasureName]);
CREATE STATISTICS [stat_cso_DimProduct_UnitPrice] ON [cso].[DimProduct]([UnitPrice]);
CREATE STATISTICS [stat_cso_DimProduct_UpdateDate] ON [cso].[DimProduct]([UpdateDate]);
CREATE STATISTICS [stat_cso_DimProduct_Weight] ON [cso].[DimProduct]([Weight]);
CREATE STATISTICS [stat_cso_DimProduct_WeightUnitMeasureID] ON [cso].[DimProduct]([WeightUnitMeasureID]);
CREATE STATISTICS [stat_cso_FactOnlineSales_CurrencyKey] ON [cso].[FactOnlineSales]([CurrencyKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_CustomerKey] ON [cso].[FactOnlineSales]([CustomerKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_DateKey] ON [cso].[FactOnlineSales]([DateKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_OnlineSalesKey] ON [cso].[FactOnlineSales]([OnlineSalesKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_ProductKey] ON [cso].[FactOnlineSales]([ProductKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_PromotionKey] ON [cso].[FactOnlineSales]([PromotionKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_StoreKey] ON [cso].[FactOnlineSales]([StoreKey]);
```

## <a name="achievement-unlocked"></a>Hervorragende Leistungen nicht gesperrt!

Sie haben die öffentliche Daten erfolgreich in Azure SQL-Data Warehouse geladen. Großartig

Sie können jetzt beginnen Abfrage der Tabellen, Abfragen, wie die folgende verwenden:

```sql
SELECT  SUM(f.[SalesAmount]) AS [sales_by_brand_amount]
,       p.[BrandName]
FROM    [cso].[FactOnlineSales] AS f
JOIN    [cso].[DimProduct]      AS p ON f.[ProductKey] = p.[ProductKey]
GROUP BY p.[BrandName]
```

## <a name="next-steps"></a>Nächste Schritte
Laden Sie die vollständigen Contoso Retail Data Warehouse Daten, verwenden Sie das Skript in Weitere Hinweise zur Entwicklung finden Sie unter [Übersicht über die Entwicklung von SQL Data Warehouse][].

<!--Image references-->

<!--Article references-->
[Erstellen einer SQL Datawarehouse]: sql-data-warehouse-get-started-provision.md
[Load data into SQL Data Warehouse]: sql-data-warehouse-overview-load.md
[Übersicht über die Entwicklung von SQL Data Warehouse]: sql-data-warehouse-overview-develop.md
[Verwalten von Columnstore Indizes]: sql-data-warehouse-tables-index.md
[Statistik]: sql-data-warehouse-tables-statistics.md
[CTAS]: sql-data-warehouse-develop-ctas.md
[label]: sql-data-warehouse-develop-label.md

<!--MSDN references-->
[ERSTELLEN DER EXTERNEN DATENQUELLE]: https://msdn.microsoft.com/en-us/library/dn935022.aspx
[ERSTELLEN VON EXTERNEN-DATEIFORMAT]: https://msdn.microsoft.com/en-us/library/dn935026.aspx
[Erstellen Sie die Tabelle AS SELECT (Transact-SQL)]: https://msdn.microsoft.com/library/mt204041.aspx
[sys.dm_pdw_exec_requests]: https://msdn.microsoft.com/library/mt203887.aspx
[REBUILD]: https://msdn.microsoft.com/library/ms188388.aspx

<!--Other Web references-->
[Microsoft Download Center]: http://www.microsoft.com/download/details.aspx?id=36433
[Laden Sie die vollständige Contoso Retail Data Warehouse]: https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/contoso-data-warehouse/readme.md

<properties
   pageTitle="Übersicht über die Tabellen in SQL Data Warehouse | Microsoft Azure"
   description="Erste Schritte mit Azure SQL-Data Warehouse-Tabellen."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/04/2016"
   ms.author="sonyama;barbkess;jrj"/>

# <a name="overview-of-tables-in-sql-data-warehouse"></a>Übersicht über die Tabellen in SQL Data Warehouse

> [AZURE.SELECTOR]
- [(Übersicht)][]
- [Datentypen][]
- [Verteilen][]
- [Index][]
- [Partition][]
- [Statistik][]
- [Temporäre][]

Erste Schritte bei der Erstellung von Tabellen in SQL Data Warehouse ist einfach.  Die Syntax der einfache [Tabelle erstellen][] folgt die allgemeine Syntax wahrscheinlich bereits mit von der Arbeit mit anderen Datenbanken vertraut.  Zum Erstellen einer Tabelle, müssen Sie einfach nennen Sie Ihre Tabelle, benennen Sie die Spalten und Datentypen für jede Spalte definieren.  Wenn Sie haben die Tabellen in anderen Datenbanken erstellen, sollte Ihnen sehr vertraut aussehen.

```sql  
CREATE TABLE Customers (FirstName VARCHAR(25), LastName VARCHAR(25))
 ``` 

Im Beispiel oben erstellt eine Tabelle mit dem Namen Kunden mit zwei Spalten, FirstName und LastName.  Jede Spalte ist mit dem Datentyp des VARCHAR(25), die Daten auf 25 Zeichen beschränkt definiert.  Diese grundlegenden Attribute einer Tabelle sowie andere, sind hauptsächlich mit anderen Datenbanken identisch.  Datentypen für jede Spalte definiert sind und die Integrität Ihrer Daten gewährleisten.  Indizes können hinzugefügt werden, um die Leistung zu verbessern, indem Sie i/o.  Partitionierung kann hinzugefügt werden, um die Leistung zu verbessern, wenn Sie benötigen, um Daten zu ändern.

[Umbenennen von] [ RENAME] eine Tabelle SQL Data Warehouse sieht wie folgt aus:

```sql  
RENAME OBJECT Customer TO CustomerOrig; 
 ```

## <a name="distributed-tables"></a>Verteilte Tabellen

Ein neues grundlegende Attribut durch verteilte Systeme wie SQL Data Warehouse die **Spalte Verteilung**ist eingeführt werden.  Die Spalte Verteilung ist sehr viel sagt bereits alles.  Es ist die Spalte, die bestimmt, wie verteilen oder eine Division, die Daten im Hintergrund aus.  Beim Erstellen einer Tabelle ohne Angabe der Spalte Verteilung wird die Tabelle automatisch mithilfe der **Funktion RUNDEN Robert**verteilt.  Round Robert Tabellen können in einigen Fällen ausreichend sein, reduziert Verteilung Spalten definieren erheblich Verschieben von Daten bei Abfragen, sodass Optimieren der Leistung.  Finden Sie unter[verteilen] [Verteilen einer Tabelle], um weitere Informationen zum Auswählen einer Spalteninhalts Verteilung.

## <a name="indexing-and-partitioning-tables"></a>Aufteilung von Tabellen und Indizierung

Wenn Sie mehr sich mit SQL Data Warehouse erweitert werden und die Leistung zu optimieren, möchten Sie weitere Informationen zur Tabellenentwurf.  Weitere Informationen finden Sie unter den Artikeln auf [Tabelle Datentypen][Datentypen], [Verteilen einer Tabelle][verteilen], [Indizieren einer Tabelle][Index] und [einer-Tabelle teilen][Partition].

## <a name="table-statistics"></a>Tabellenstatistiken

Statistiken sind eine äußerst wichtig am besten die optimale Leistung von Ihrer SQL Data Warehouse.  Da SQL Data Warehouse nicht noch automatisch erstellt wird und Update-Statistiken für Sie, wie Sie möglicherweise in Azure SQL-Datenbank heutzutage, lesen unseren Artikel auf [Statistik][] erwarten möglicherweise den wichtigsten Artikeln lesen Sie, um sicherzustellen, dass Sie die optimale Leistung von Ihrer Abfragen zu gelangen.

## <a name="temporary-tables"></a>Temporäre Tabellen

Temporäre Tabellen sind Tabellen, die nur für die Dauer der Anmeldung vorhanden sind, und nicht durch andere Benutzer sichtbar.  Temporäre Tabellen können eine gute Möglichkeit, die verhindern, dass andere sehen können, temporäre Ergebnisse und auch verringern die Notwendigkeit zum Aufräumen sein.  Da temporäre Tabellen auch lokalen Speicher nutzen, können sie für einige Vorgänge bessere Leistung bieten.  Siehe folgende Artikel [Temporäre Tabelle][temporäre] ausführliche Informationen zu temporären Tabellen.

## <a name="external-tables"></a>Externe Tabellen

Externe Tabellen, auch bekannt als Polybase Tabellen sind Tabellen, die mit externen Daten aus SQL Data Warehouse SQL Data Warehouse, aber Punkt abgefragt werden können.  Beispielsweise können Sie auf Dateien auf Azure BLOB-Speicher verweist eine externe Tabelle erstellen.  Weitere Informationen zum Erstellen und Abfragen eine externe Tabelle finden Sie unter [Daten mit Polybase laden][].  

## <a name="unsupported-table-features"></a>Nicht unterstützte Tabellenfeatures

Während der SQL Data Warehouse viele andere Datenbanken angebotenen derselben Tabellenfeatures enthält, gibt es jedoch einige Features, die nicht unterstützt werden.  Es folgt eine Liste mit einigen der Tabellenfeatures, die nicht unterstützt werden.

| Nicht unterstützte features |
| --- |
|[Identitätseigenschaft][] (siehe [Zuweisen von Ersatzzeichen Schlüssel dieses Problem zu umgehen][])|
|Primärschlüssel, Fremdschlüssel, Unique und Kontrollkästchen [Tabelle Einschränkungen][]|
|[Eindeutige Indizes][]|
|[Berechnete Spalten][]|
|[Gering gefüllte Spalten][]|
|[Benutzerdefinierte Typen][]|
|[Sequenz][]|
|[Trigger][]|
|[Indizierte Sichten][]|
|[Synonyme][]|

## <a name="table-size-queries"></a>Tabelle Größe Abfragen

Eine einfache Möglichkeit, Leerzeichen und Zeilen von einer Tabelle in jedem der Verteilung 60 verbraucht identifizieren besteht darin, [DBCC PDW_SHOWSPACEUSED][]verwenden.

```sql
DBCC PDW_SHOWSPACEUSED('dbo.FactInternetSales');
```

Allerdings kann mit DBCC-Befehle ganz eingeschränkt werden.  Dynamische Management-Ansichten (DMVs) können Sie viel mehr Details finden Sie unter als auch bieten Ihnen viel mehr Kontrolle über die Ergebnisse der Abfrage.  Erstes erstellen diese Ansicht von vielen unserer Beispiele in dieser und weitere Artikel zu verwiesen wird.

```sql
CREATE VIEW dbo.vTableSizes
AS
WITH base
AS
(
SELECT 
 GETDATE()                                                             AS  [execution_time]
, DB_NAME()                                                            AS  [database_name]
, s.name                                                               AS  [schema_name]
, t.name                                                               AS  [table_name]
, QUOTENAME(s.name)+'.'+QUOTENAME(t.name)                              AS  [two_part_name]
, nt.[name]                                                            AS  [node_table_name]
, ROW_NUMBER() OVER(PARTITION BY nt.[name] ORDER BY (SELECT NULL))     AS  [node_table_name_seq]
, tp.[distribution_policy_desc]                                        AS  [distribution_policy_name]
, c.[name]                                                             AS  [distribution_column]
, nt.[distribution_id]                                                 AS  [distribution_id]
, i.[type]                                                             AS  [index_type]
, i.[type_desc]                                                        AS  [index_type_desc]
, nt.[pdw_node_id]                                                     AS  [pdw_node_id]
, pn.[type]                                                            AS  [pdw_node_type]
, pn.[name]                                                            AS  [pdw_node_name]
, di.name                                                              AS  [dist_name]
, di.position                                                          AS  [dist_position]
, nps.[partition_number]                                               AS  [partition_nmbr]
, nps.[reserved_page_count]                                            AS  [reserved_space_page_count]
, nps.[reserved_page_count] - nps.[used_page_count]                    AS  [unused_space_page_count]
, nps.[in_row_data_page_count] 
    + nps.[row_overflow_used_page_count] 
    + nps.[lob_used_page_count]                                        AS  [data_space_page_count]
, nps.[reserved_page_count] 
 - (nps.[reserved_page_count] - nps.[used_page_count]) 
 - ([in_row_data_page_count] 
         + [row_overflow_used_page_count]+[lob_used_page_count])       AS  [index_space_page_count]
, nps.[row_count]                                                      AS  [row_count]
from 
    sys.schemas s
INNER JOIN sys.tables t
    ON s.[schema_id] = t.[schema_id]
INNER JOIN sys.indexes i
    ON  t.[object_id] = i.[object_id]
    AND i.[index_id] <= 1
INNER JOIN sys.pdw_table_distribution_properties tp
    ON t.[object_id] = tp.[object_id]
INNER JOIN sys.pdw_table_mappings tm
    ON t.[object_id] = tm.[object_id]
INNER JOIN sys.pdw_nodes_tables nt
    ON tm.[physical_name] = nt.[name]
INNER JOIN sys.dm_pdw_nodes pn
    ON  nt.[pdw_node_id] = pn.[pdw_node_id]
INNER JOIN sys.pdw_distributions di
    ON  nt.[distribution_id] = di.[distribution_id]
INNER JOIN sys.dm_pdw_nodes_db_partition_stats nps
    ON nt.[object_id] = nps.[object_id]
    AND nt.[pdw_node_id] = nps.[pdw_node_id]
    AND nt.[distribution_id] = nps.[distribution_id]
LEFT OUTER JOIN (select * from sys.pdw_column_distribution_properties where distribution_ordinal = 1) cdp
    ON t.[object_id] = cdp.[object_id]
LEFT OUTER JOIN sys.columns c
    ON cdp.[object_id] = c.[object_id]
    AND cdp.[column_id] = c.[column_id]
)
, size
AS
(
SELECT
   [execution_time]
,  [database_name]
,  [schema_name]
,  [table_name]
,  [two_part_name]
,  [node_table_name]
,  [node_table_name_seq]
,  [distribution_policy_name]
,  [distribution_column]
,  [distribution_id]
,  [index_type]
,  [index_type_desc]
,  [pdw_node_id]
,  [pdw_node_type]
,  [pdw_node_name]
,  [dist_name]
,  [dist_position]
,  [partition_nmbr]
,  [reserved_space_page_count]
,  [unused_space_page_count]
,  [data_space_page_count]
,  [index_space_page_count]
,  [row_count]
,  ([reserved_space_page_count] * 8.0)                                 AS [reserved_space_KB]
,  ([reserved_space_page_count] * 8.0)/1000                            AS [reserved_space_MB]
,  ([reserved_space_page_count] * 8.0)/1000000                         AS [reserved_space_GB]
,  ([reserved_space_page_count] * 8.0)/1000000000                      AS [reserved_space_TB]
,  ([unused_space_page_count]   * 8.0)                                 AS [unused_space_KB]
,  ([unused_space_page_count]   * 8.0)/1000                            AS [unused_space_MB]
,  ([unused_space_page_count]   * 8.0)/1000000                         AS [unused_space_GB]
,  ([unused_space_page_count]   * 8.0)/1000000000                      AS [unused_space_TB]
,  ([data_space_page_count]     * 8.0)                                 AS [data_space_KB]
,  ([data_space_page_count]     * 8.0)/1000                            AS [data_space_MB]
,  ([data_space_page_count]     * 8.0)/1000000                         AS [data_space_GB]
,  ([data_space_page_count]     * 8.0)/1000000000                      AS [data_space_TB]
,  ([index_space_page_count]  * 8.0)                                   AS [index_space_KB]
,  ([index_space_page_count]  * 8.0)/1000                              AS [index_space_MB]
,  ([index_space_page_count]  * 8.0)/1000000                           AS [index_space_GB]
,  ([index_space_page_count]  * 8.0)/1000000000                        AS [index_space_TB]
FROM base
)
SELECT * 
FROM size
;
```

### <a name="table-space-summary"></a>Zusammenfassung Tabellenbereich

Diese Abfrage gibt Zeilen und Leerzeichen Tabelle.  Es ist eine gute Abfrage zu sehen, welche Tabellen größten Tabellen und, ob sie die Funktion RUNDEN Robert oder Hash verteilt sind.  Für Hashtabellen verteilt wird auch die Spalte Verteilung.  In den meisten Fällen sollten die größten Tabellen Hash mit einem gruppierten Columnstore Index verteilt.

```sql
SELECT 
     database_name
,    schema_name
,    table_name
,    distribution_policy_name
,     distribution_column
,    index_type_desc
,    COUNT(distinct partition_nmbr) as nbr_partitions
,    SUM(row_count)                 as table_row_count
,    SUM(reserved_space_GB)         as table_reserved_space_GB
,    SUM(data_space_GB)             as table_data_space_GB
,    SUM(index_space_GB)            as table_index_space_GB
,    SUM(unused_space_GB)           as table_unused_space_GB
FROM 
    dbo.vTableSizes
GROUP BY 
     database_name
,    schema_name
,    table_name
,    distribution_policy_name
,     distribution_column
,    index_type_desc
ORDER BY
    table_reserved_space_GB desc
;
```

### <a name="table-space-by-distribution-type"></a>Tabelle Abstand nach Verteilungstyp

```sql
SELECT 
     distribution_policy_name
,    SUM(row_count)                as table_type_row_count
,    SUM(reserved_space_GB)        as table_type_reserved_space_GB
,    SUM(data_space_GB)            as table_type_data_space_GB
,    SUM(index_space_GB)           as table_type_index_space_GB
,    SUM(unused_space_GB)          as table_type_unused_space_GB
FROM dbo.vTableSizes
GROUP BY distribution_policy_name
;
```

### <a name="table-space-by-index-type"></a>Tabelle Abstand nach Indextyp

```sql
SELECT 
     index_type_desc
,    SUM(row_count)                as table_type_row_count
,    SUM(reserved_space_GB)        as table_type_reserved_space_GB
,    SUM(data_space_GB)            as table_type_data_space_GB
,    SUM(index_space_GB)           as table_type_index_space_GB
,    SUM(unused_space_GB)          as table_type_unused_space_GB
FROM dbo.vTableSizes
GROUP BY index_type_desc
;
```

### <a name="distribution-space-summary"></a>Verteilung Leerzeichen Zusammenfassung

```sql
SELECT 
    distribution_id
,    SUM(row_count)                as total_node_distribution_row_count
,    SUM(reserved_space_MB)        as total_node_distribution_reserved_space_MB
,    SUM(data_space_MB)            as total_node_distribution_data_space_MB
,    SUM(index_space_MB)           as total_node_distribution_index_space_MB
,    SUM(unused_space_MB)          as total_node_distribution_unused_space_MB
FROM dbo.vTableSizes
GROUP BY     distribution_id
ORDER BY    distribution_id
;
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter den Artikeln auf [Tabelle Datentypen][Datentypen], [Verteilen einer Tabelle][verteilen], [Indizieren einer Tabelle][Index], [eine-Tabelle teilen][Partition], [Tabellenstatistiken Verwalten von][Statistiken] und [Temporäre Tabellen][temporäre].  Weitere Informationen zu best Practices finden Sie unter [Bewährte Methoden für SQL Data Warehouse][].

<!--Image references-->

<!--Article references-->
[(Übersicht)]: ./sql-data-warehouse-tables-overview.md
[Datentypen]: ./sql-data-warehouse-tables-data-types.md
[Verteilen]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistik]: ./sql-data-warehouse-tables-statistics.md
[Temporäre]: ./sql-data-warehouse-tables-temporary.md
[Bewährte Methoden für SQL Datawarehouse]: ./sql-data-warehouse-best-practices.md
[Laden Sie die Daten mit Polybase]: ./sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md

<!--MSDN references-->
[TABELLE ERSTELLEN]: https://msdn.microsoft.com/library/mt203953.aspx
[RENAME]: https://msdn.microsoft.com/library/mt631611.aspx
[DBCC PDW_SHOWSPACEUSED]: https://msdn.microsoft.com/library/mt204028.aspx
[Identitätseigenschaft]: https://msdn.microsoft.com/library/ms186775.aspx
[Zuweisen von Key Ersatzzeichen dieses Problem zu umgehen]: https://blogs.msdn.microsoft.com/sqlcat/2016/02/18/assigning-surrogate-key-to-dimension-tables-in-sql-dw-and-aps/
[Tabelle Einschränkungen]: https://msdn.microsoft.com/library/ms188066.aspx
[Berechnete Spalten]: https://msdn.microsoft.com/library/ms186241.aspx
[Gering gefüllte Spalten]: https://msdn.microsoft.com/library/cc280604.aspx
[Benutzerdefinierte Typen]: https://msdn.microsoft.com/library/ms131694.aspx
[Sequenz]: https://msdn.microsoft.com/library/ff878091.aspx
[Trigger]: https://msdn.microsoft.com/library/ms189799.aspx
[Indizierte Sichten]: https://msdn.microsoft.com/library/ms191432.aspx
[Synonyme]: https://msdn.microsoft.com/library/ms177544.aspx
[Eindeutige Indizes]: https://msdn.microsoft.com/library/ms188783.aspx

<!--Other Web references-->

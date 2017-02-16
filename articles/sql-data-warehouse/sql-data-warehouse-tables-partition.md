<properties
   pageTitle="Tabellen in SQL Data Warehouse Partitionierung | Microsoft Azure"
   description="Erste Schritte mit Tabelle in Azure SQL-Data Warehouse Verteilung."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="jrowlandjones"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="07/18/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="partitioning-tables-in-sql-data-warehouse"></a>Partitionierungstabellen in SQL Data Warehouse

> [AZURE.SELECTOR]
- [(Übersicht)][]
- [Datentypen][]
- [Verteilen][]
- [Index][]
- [Partition][]
- [Statistik][]
- [Temporäre][]

Partitionierung wird auf alle Typen von SQL Data Warehouse Tabelle unterstützt. einschließlich gruppierte Columnstore, gruppierten Index und Heap an.  Partitionierung wird auf alle Verteilungstypen, einschließlich sowohl Hash oder runden Robert verteilt ebenfalls unterstützt.  Partitionierung ermöglicht ist Ihnen, Ihre Daten in kleineren Gruppen von Daten und in den meisten Fällen Partitionierung aufgeteilt auf einer Datumsspalte abgeschlossen.

## <a name="benefits-of-partitioning"></a>Vorteile Aufteilung

Partitionierung kann Daten Wartung und Abfrage Performance profitieren.  Gibt an, ob sie beide oder nur eine Vorteile hängt von wie die Daten geladen werden, und gibt an, ob die gleiche Spalte für beide Zwecke, verwendet werden kann, da Partitionierung nur eine Spalte durchgeführt werden kann.

### <a name="benefits-to-loads"></a>Vorteile lädt

Der primäre Vorteil Aufteilung in SQL Data Warehouse ist verbessern, die Effizienz und Leistung von laden, die Daten von der Partition Löschvorgang verwenden, wechseln und zusammenführen.  In den meisten Fällen ist der Daten auf einer Datumsspalte aufgeteilt, die eng mit der Sequenz verbunden ist, die die Daten in die Datenbank geladen wird.  Einer der Vorteile der Verwendung von Partitionen Daten größtmögliche es Vermeidung der Protokollierung von Transaktionen.  Einfach einfügen, aktualisieren oder Löschen von Daten kann die einfachste besteht, mit etwas Gedanke Aufwand, sein kann durch Partitionierung, während der Ladevorgang Leistungsfähigkeit wesentlich verbessern.

Partition wechseln kann schnell entfernen oder Ersetzen eines Abschnitts in einer Tabelle verwendet werden.  Beispielsweise kann eine Faktentabelle sales nur Daten für die letzten 36 Monate enthalten.  Am Ende des Monats wird der älteste Monat mit Umsatzdaten, aus der Tabelle gelöscht.  Diese Daten konnte mithilfe einer Delete-Anweisung So löschen Sie die Daten für den ältesten Monat gelöscht werden.  Jedoch kann eine große Datenmenge Daten Zeile für Zeile mit einer Delete-Anweisung löschen sehr lange dauern, als auch erstellen das Risiko von großen Transaktionen, die sehr lange rückgängig gemacht dauern konnte, wenn ein Problem auftritt.  Eine weitere optimale Vorgehensweise besteht darin, das älteste Teil Daten einfach ablegen.  Stelle, an der die einzelnen Zeilen löschen Stunden, dauern konnte kann Löschen einer gesamten Partition Sekunden dauern.

### <a name="benefits-to-queries"></a>Vorteile für Abfragen

Partitionierung kann auch für optimale Leistung der Abfrage verwendet werden.  Wenn eine Abfrage ein Filters auf eine partitionierte Spalte angewendet wird, kann dies die Überprüfung auf nur die qualifizierte einschränken, die möglicherweise eine viel kleinere Teilmenge der Daten, einen vollständige Tabellenscan zu vermeiden.  Mit der Einführung von Indizes gruppierte Columnstore Prädikat Beseitigung Leistungsvorteile weniger hilfreich sind, aber in einigen Fällen werden ein Vorteil für Abfragen.  Beispielsweise die Faktentabelle sales in 36 Monate, die mit dem Verkaufsdatum Feld aufgeteilt, und diese Filter auf dem Verkaufsdatum Abfragen kann überspringen Suchen in Partitionen, die nicht den Filter entsprechen.

## <a name="partition-sizing-guidance"></a>Partition Ziehpunkts Anleitungen

Während der Partitionierung verwendet werden kann für optimale Leistung einiger Szenarien, kann die Leistung unter bestimmten Umständen durch die Erstellen einer Tabelle mit **zu viele** Partitionen beeinträchtigt werden.  Diese Bedenken sind besonders WAHR für gruppierte Columnstore Tabellen.  Für Einheit hilfreich sein, ist es wichtig, wann Partitionierung und die Anzahl der zu erstellenden Partitionen zu verstehen.  Es gibt keine schnelle harte Regel anderem Ort tätig, wie viele Partitionen zu viele sind, abhängig von Ihrer Daten und wie viele teilt gleichzeitig zu geladen sind.  Denken Sie als eine allgemeine Faustregel des Hinzufügens von 10 s zu 100 s Partitionen nicht 1000 s.

Beim Erstellen von Tabellen **gruppierte Columnstore** Partitionierung, ist es wichtig zu berücksichtigen, wie viele Zeilen in jeder Partition landen werden.  Für optimale Komprimierung und Leistung von gruppierten Columnstore Tabellen ist mindestens 1 Millionen Zeilen pro Verteilung und Partition erforderlich.  Bevor Sie Partitionen erstellt werden, teilt SQL Data Warehouse bereits jede Tabelle in 60 verteilten Datenbanken.  Alle Partitionierung zu einer Tabelle hinzugefügt wird, sowie die Verteilung im Hintergrund erstellt.  Verwenden in diesem Beispiel, wenn die sales Faktentabelle 36 monatliche Partitionen enthalten und, da SQL Data Warehouse 60 Verteilung aufweist, klicken Sie dann die Umsatz Faktentabelle sollte enthalten 60 Millionen Zeilen pro Monat oder 2.1 Milliarden Zeilen alle Monate aufgefüllt wurden.  Wenn eine Tabelle deutlich weniger Zeilen als die empfohlene minimale Anzahl von Zeilen pro Partition enthält, erwägen Sie, die weniger Partitionen akzeptieren, um die Anzahl der Zeilen pro Partition erhöhen tätigen.  Auch finden Sie unter [Indizierung][Index] die Abfragen enthält, die auf SQL Data Warehouse, um die Qualität des Cluster Columnstore Indizes bewerten ausgeführt werden kann.

## <a name="syntax-difference-from-sql-server"></a>Syntax der Differenz von SQL Server

SQL Data Warehouse bietet eine vereinfachte Definition für Partitionen also geringfügig von SQL Server.  Vorherigen Funktionen und Schemas werden in SQL Data Warehouse nicht verwendet, wie in SQL Server.  In diesem Fall Sie müssen lediglich partitionierte Spalte und die Begrenzungslinie Punkte zu identifizieren.  Wenn die Syntax der Aufteilung möglicherweise etwas anders aus SQL Server ist, sind die grundlegenden Konzepte gleich aus.  SQL Server und SQL Data Warehouse unterstützt eine Partition Spalte pro Tabelle, die Partition ausgeschöpft werden kann.  Erfahren Sie mehr über das aufteilen, finden Sie unter [Tabellen aufgeteilt und Indizes][].

Die teilt den gezeigten Beispiel einer Anweisung SQL Data Warehouse aufgeteilt [Tabelle erstellen][] , der Tabelle FactInternetSales OrderDateKey Spalte:

```sql
CREATE TABLE [dbo].[FactInternetSales]
(
    [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                    (20000101,20010101,20020101
                    ,20030101,20040101,20050101
                    )
                )
)
;
```

## <a name="migrating-partitioning-from-sql-server"></a>Migrieren von SQL Server-Partitionierung

SQL Server-Partitionsdefinitionen einfach in SQL Data Warehouse migrieren:

- Die SQL Server- [Partitionsschema][]zu unterdrücken.
- Hinzufügen der [Partitionsfunktion][] Definition Ihrer Tabelle erstellen.

Wenn Sie eine partitionierte Tabelle aus einer SQL Server-Instanz Migrieren der unter SQL hilft Ihnen, die Anzahl der Zeilen, die in jeder Partition sind, abgefragt.  Beachten Sie, dass, wenn die gleiche vorherigen Genauigkeit SQL Data Warehouse verwendet wird, wird die Anzahl der Zeilen pro Partition mit dem Faktor von 60 verkleinern beibehalten.  

```sql
-- Partition information for a SQL Server Database
SELECT      s.[name]                        AS      [schema_name]
,           t.[name]                        AS      [table_name]
,           i.[name]                        AS      [index_name]
,           p.[partition_number]            AS      [partition_number]
,           SUM(a.[used_pages]*8.0)         AS      [partition_size_kb]
,           SUM(a.[used_pages]*8.0)/1024    AS      [partition_size_mb]
,           SUM(a.[used_pages]*8.0)/1048576 AS      [partition_size_gb]
,           p.[rows]                        AS      [partition_row_count]
,           rv.[value]                      AS      [partition_boundary_value]
,           p.[data_compression_desc]       AS      [partition_compression_desc]
FROM        sys.schemas s
JOIN        sys.tables t                    ON      t.[schema_id]         = s.[schema_id]
JOIN        sys.partitions p                ON      p.[object_id]         = t.[object_id]
JOIN        sys.allocation_units a          ON      a.[container_id]      = p.[partition_id]
JOIN        sys.indexes i                   ON      i.[object_id]         = p.[object_id]
                                            AND     i.[index_id]          = p.[index_id]
JOIN        sys.data_spaces ds              ON      ds.[data_space_id]    = i.[data_space_id]
LEFT JOIN   sys.partition_schemes ps        ON      ps.[data_space_id]    = ds.[data_space_id]
LEFT JOIN   sys.partition_functions pf      ON      pf.[function_id]      = ps.[function_id]
LEFT JOIN   sys.partition_range_values rv   ON      rv.[function_id]      = pf.[function_id]
                                            AND     rv.[boundary_id]      = p.[partition_number]
WHERE       p.[index_id] <=1
GROUP BY    s.[name]
,           t.[name]
,           i.[name]
,           p.[partition_number]
,           p.[rows]
,           rv.[value]
,           p.[data_compression_desc]
;
```

## <a name="workload-management"></a>Arbeitsbelastung management

[Arbeitsbelastung Management][]ist eine endgültige Stück zu berücksichtigen, der Tabelle Partition Entscheidung zu berücksichtigen.  Arbeitsbelastung Management in SQL Data Warehouse wird hauptsächlich die Verwaltung von Arbeitsspeicher und Parallelität.  In SQL Data Warehouse ist der maximale Arbeitsspeicher jede Verteilung während der abfrageausführung Ressourcenklassen geregelt an.  Idealerweise werden Ihre Partitionen hinsichtlich anderer Faktoren wie die Erstellung von Indizes gruppierte Columnstore Arbeitsspeicher Anforderungen Größe geändert werden.  Gruppierte Columnstore Indizes Vorteile erheblich, wenn sie mehr Arbeitsspeicher zugewiesen werden.  Daher sollten Sie sicherstellen, dass eine Partition Index Wiederherstellung Arbeitsspeicher nicht Sicherstellung ist. Erhöhen die Größe des Arbeitsspeichers Ihrer Abfrage verfügbar kann durch umsteigen aus der Standardrolle Smallrc, auf eine der anderen Rollen wie Largerc erzielt werden.

Informationen über die Verteilung der Arbeitsspeicher pro Verteilung wird die Kontrolle dynamische Management Ressourcenansichten Abfragen verfügbar. Tatsächlich werden Ihre erteilen Arbeitsspeicher kleiner als die folgenden Zahlen. Dies bietet jedoch eine Ebene von Leitfäden, die Sie beim Ändern der Größe Ihrer Partitionen für Management Datenoperationen verwenden können.  Versuchen Sie, zu vermeiden, Ändern der Größe Ihrer Partitionen jenseits der von der Ressourcenklasse extra große bereitgestellte Arbeitsspeicher erteilen. Wenn sich Ihre Partitionen jenseits dieser Abbildung wächst ausführen das Risiko von Speicherdruck, was wiederum zu weniger optimale Komprimierung führt.

```sql
SELECT  rp.[name]                               AS [pool_name]
,       rp.[max_memory_kb]                      AS [max_memory_kb]
,       rp.[max_memory_kb]/1024                 AS [max_memory_mb]
,       rp.[max_memory_kb]/1048576              AS [mex_memory_gb]
,       rp.[max_memory_percent]                 AS [max_memory_percent]
,       wg.[name]                               AS [group_name]
,       wg.[importance]                         AS [group_importance]
,       wg.[request_max_memory_grant_percent]   AS [request_max_memory_grant_percent]
FROM    sys.dm_pdw_nodes_resource_governor_workload_groups  wg
JOIN    sys.dm_pdw_nodes_resource_governor_resource_pools   rp ON wg.[pool_id] = rp.[pool_id]
WHERE   wg.[name] like 'SloDWGroup%'
AND     rp.[name]    = 'SloDWPool'
;
```

## <a name="partition-switching"></a>Partition wechseln

SQL Data Warehouse unterstützt Partition aufteilen, Zusammenführen und wechseln. Jede dieser Funktionen ist Excuted mit der [ALTER TABLE][] -Anweisung.

Zum Umschalten zwischen zwei Tabellen Partitionen müssen Sie sicherstellen, dass die Partitionen an ihre jeweiligen Grenzen ausrichten und die Tabellendefinitionen entsprechen. Wie das Kontrollkästchen Einschränkungen nicht erzwingen des Bereiches der Werte in einer Tabelle zur Verfügung stehen muss die Quelltabelle als Zieltabelle in die gleichen Partitionsgrenzen enthalten. Wenn dies nicht der Fall ist, wird die Partition wechseln fehl, wie die Metadaten Partition nicht synchronisiert werden soll.

### <a name="how-to-split-a-partition-that-contains-data"></a>So teilen Sie eine Partition, die Daten enthält

Die effizienteste Methode, um eine Partition zu teilen, die bereits Daten enthält, ist die Verwendung eines `CTAS` Anweisung. Ist die partitionierte Tabelle eine gruppierte Columnstore muss klicken Sie dann die Tabellenpartition leer sein, bevor sie aufgeteilt werden kann.

Es folgt eine partitionierte Columnstore, mit einer einzigen Zeile in jeder Partition Beispieltabelle:

```sql
CREATE TABLE [dbo].[FactInternetSales]
(
        [ProductKey]            int          NOT NULL
    ,   [OrderDateKey]          int          NOT NULL
    ,   [CustomerKey]           int          NOT NULL
    ,   [PromotionKey]          int          NOT NULL
    ,   [SalesOrderNumber]      nvarchar(20) NOT NULL
    ,   [OrderQuantity]         smallint     NOT NULL
    ,   [UnitPrice]             money        NOT NULL
    ,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                    (20000101
                    )
                )
)
;

INSERT INTO dbo.FactInternetSales
VALUES (1,19990101,1,1,1,1,1,1);
INSERT INTO dbo.FactInternetSales
VALUES (1,20000101,1,1,1,1,1,1);


CREATE STATISTICS Stat_dbo_FactInternetSales_OrderDateKey ON dbo.FactInternetSales(OrderDateKey);
```

> [AZURE.NOTE] Durch das Erstellen des statistischen Objekts, stellen wir sicher, dass diese Tabellenmetadaten genauere ist. Wenn wir das Erstellen von Statistiken nicht angeben, wird SQL Data Warehouse Standardwerte verwendet. Ausführliche Informationen zum Statistiken Aufforderung zur Überarbeitung [Statistik][].

Wir können dann Abfragen, für die Zeile zählen mithilfe der `sys.partitions` Katalog anzeigen:

```sql
SELECT  QUOTENAME(s.[name])+'.'+QUOTENAME(t.[name]) as Table_name
,       i.[name] as Index_name
,       p.partition_number as Partition_nmbr
,       p.[rows] as Row_count
,       p.[data_compression_desc] as Data_Compression_desc
FROM    sys.partitions p
JOIN    sys.tables     t    ON    p.[object_id]   = t.[object_id]
JOIN    sys.schemas    s    ON    t.[schema_id]   = s.[schema_id]
JOIN    sys.indexes    i    ON    p.[object_id]   = i.[object_Id]
                            AND   p.[index_Id]    = i.[index_Id]
WHERE t.[name] = 'FactInternetSales'
;
```

Wenn wir versuchen, diese Tabelle teilen, werden wir ein Fehler angezeigt:

```sql
ALTER TABLE FactInternetSales SPLIT RANGE (20010101);
```

Msg 35346, Ebene 15, Status 1, Zeile 44 Teilen-Klausel PARTITION ALTER-Anweisung ist fehlgeschlagen, da die nicht leer sind.  In können nur leere Partitionen aufgeteilt werden, wenn Sie ein Columnstore Index in der Tabelle vorhanden ist. Sollten Sie den Index Columnstore vor dem Ausgeben der Anweisung ALTER PARTITION, und klicken Sie dann erneutes Erstellen des Indexes Columnstore nach Abschluss der ALTER PARTITION deaktivieren.

Wir können jedoch `CTAS` zum Erstellen einer neuen Tabelle unsere Daten aufnehmen soll.

```sql
CREATE TABLE dbo.FactInternetSales_20000101
    WITH    (   DISTRIBUTION = HASH(ProductKey)
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101
                                )
                            )
            )
AS
SELECT *
FROM    FactInternetSales
WHERE   1=2
;
```

Wie die Partitionsgrenzen ausgerichtet werden darf eine wechseln. Auf diese Weise die Quelltabelle mit einer leeren bleibt, die wir anschließend zu teilen können.

```sql
ALTER TABLE FactInternetSales SWITCH PARTITION 2 TO  FactInternetSales_20000101 PARTITION 2;

ALTER TABLE FactInternetSales SPLIT RANGE (20010101);
```

Zum Ausführen offen steht lediglich Flatterrand unsere Daten zu den neuen Partitionsgrenzen mit `CTAS` und unsere Daten wieder in der Tabelle Hauptfenster wechseln

```sql
CREATE TABLE [dbo].[FactInternetSales_20000101_20010101]
    WITH    (   DISTRIBUTION = HASH([ProductKey])
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101,20010101
                                )
                            )
            )
AS
SELECT  *
FROM    [dbo].[FactInternetSales_20000101]
WHERE   [OrderDateKey] >= 20000101
AND     [OrderDateKey] <  20010101
;

ALTER TABLE dbo.FactInternetSales_20000101_20010101 SWITCH PARTITION 2 TO dbo.FactInternetSales PARTITION 2;
```

Nachdem Sie das Verschieben der Daten abgeschlossen haben, ist es eine gute Idee, aktualisieren die Statistiken für die Zieltabelle aus, um sicherzustellen, dass sie genau die neue Verteilung der Daten in den entsprechenden Partitionen widerspiegeln:

```sql
UPDATE STATISTICS [dbo].[FactInternetSales];
```

### <a name="table-partitioning-source-control"></a>Tabelle vorherigen Datenquellen-Steuerelements

Zur Vermeidung von der Tabellendefinition Ihrer von **negativen** in Ihrem System Steuerelement sollten Sie erwägen Sie den folgenden Vorgehensweise:

1. Erstellen Sie die Tabelle aus, wie eine partitionierte Tabelle, aber ohne Partition Werte

```sql
CREATE TABLE [dbo].[FactInternetSales]
(
    [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                    ()
                )
)
;
```

2. `SPLIT`die Tabelle als Teil der Bereitstellungsprozess:

```sql
-- Create a table containing the partition boundaries

CREATE TABLE #partitions
WITH
(
    LOCATION = USER_DB
,   DISTRIBUTION = HASH(ptn_no)
)
AS
SELECT  ptn_no
,       ROW_NUMBER() OVER (ORDER BY (ptn_no)) as seq_no
FROM    (
        SELECT CAST(20000101 AS INT) ptn_no
        UNION ALL
        SELECT CAST(20010101 AS INT)
        UNION ALL
        SELECT CAST(20020101 AS INT)
        UNION ALL
        SELECT CAST(20030101 AS INT)
        UNION ALL
        SELECT CAST(20040101 AS INT)
        ) a
;

-- Iterate over the partition boundaries and split the table

DECLARE @c INT = (SELECT COUNT(*) FROM #partitions)
,       @i INT = 1                                 --iterator for while loop
,       @q NVARCHAR(4000)                          --query
,       @p NVARCHAR(20)     = N''                  --partition_number
,       @s NVARCHAR(128)    = N'dbo'               --schema
,       @t NVARCHAR(128)    = N'FactInternetSales' --table
;

WHILE @i <= @c
BEGIN
    SET @p = (SELECT ptn_no FROM #partitions WHERE seq_no = @i);
    SET @q = (SELECT N'ALTER TABLE '+@s+N'.'+@t+N' SPLIT RANGE ('+@p+N');');

    -- PRINT @q;
    EXECUTE sp_executesql @q;

    SET @i+=1;
END

-- Code clean-up

DROP TABLE #partitions;
```

Bei diesem Ansatz der Code im Datenquellen-Steuerelements statischen bleibt, und die vorherigen Begrenzungslinie Werte dürfen dynamische sein; mit dem Warehouse Weiterentwicklung von über einen Zeitraum.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter den Artikeln auf [Tabelle Übersicht][Übersicht], [Tabelle Datentypen][Datentypen], [Verteilen einer Tabelle][verteilen], [Indizieren einer Tabelle][Index], [Tabellenstatistiken Verwalten von][Statistiken] und [Temporäre Tabellen][temporäre].  Weitere Informationen zu best Practices finden Sie unter [Bewährte Methoden für SQL Data Warehouse][].

<!--Image references-->

<!--Article references-->
[(Übersicht)]: ./sql-data-warehouse-tables-overview.md
[Datentypen]: ./sql-data-warehouse-tables-data-types.md
[Verteilen]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistik]: ./sql-data-warehouse-tables-statistics.md
[Temporäre]: ./sql-data-warehouse-tables-temporary.md
[Arbeitsbelastung management]: ./sql-data-warehouse-develop-concurrency.md
[Bewährte Methoden für SQL Datawarehouse]: ./sql-data-warehouse-best-practices.md

<!-- MSDN Articles -->
[Partitionierte Tabellen und Indizes]: https://msdn.microsoft.com/library/ms190787.aspx
[TABELLE ÄNDERN]: https://msdn.microsoft.com/en-us/library/ms190273.aspx
[TABELLE ERSTELLEN]: https://msdn.microsoft.com/library/mt203953.aspx
[Partitionsfunktion]: https://msdn.microsoft.com/library/ms187802.aspx
[Partitionsschema]: https://msdn.microsoft.com/library/ms179854.aspx


<!-- Other web references -->

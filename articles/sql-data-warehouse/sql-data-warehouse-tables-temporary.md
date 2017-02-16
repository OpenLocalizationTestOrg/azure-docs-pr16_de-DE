<properties
   pageTitle="Temporäre Tabellen in SQL Data Warehouse | Microsoft Azure"
   description="Erste Schritte mit temporären Tabellen in Azure SQL-Data Warehouse."
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
   ms.date="06/29/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="temporary-tables-in-sql-data-warehouse"></a>Temporäre Tabellen in SQL Data Warehouse

> [AZURE.SELECTOR]
- [(Übersicht)][]
- [Datentypen][]
- [Verteilen][]
- [Index][]
- [Partition][]
- [Statistik][]
- [Temporäre][]

Temporäre Tabellen sind sehr hilfreich, wenn Daten - besonders während der Transformation Verarbeitung, wo sich die Zwischenergebnisse vorübergehende befinden. In SQL Data Warehouse vorhanden sind temporäre Tabellen Ebene der Sitzung ein.  Sie sind nur für die Sitzung, in der sie erstellt wurden, und werden automatisch gelöscht, bei der Abmeldung dieser Sitzung, sichtbar.  Temporäre Tabellen anbieten einen Leistungsvorteil, da ihre Ergebnisse auf lokale und remote-Speicher nicht geschrieben werden.  Temporäre Tabellen sind in Azure SQL-Data Warehouse etwas anders als Azure SQL-Datenbank, wie sie von überall innerhalb der Sitzung, einschließlich innerhalb und außerhalb einer gespeicherten Prozedur zugegriffen werden kann.

In diesem Artikel grundlegende Leitfaden für die Verwendung von temporärer Tabellen enthält und die Prinzipien der Sitzung Ebene temporäre Tabellen hervorgehoben. Verwenden die Informationen in diesem Artikel helfen Ihnen modularisiert Code sowohl Wiederverwendung und Pflege des Codes für erleichterte Bedienung zu verbessern.

## <a name="create-a-temporary-table"></a>Erstellen Sie eine temporäre Tabelle

Temporäre Tabellen werden erstellt, indem Sie einfach mit der Tabellenname voranstellen eines `#`.  Beispiel:

```sql
CREATE TABLE #stats_ddl
(
    [schema_name]       NVARCHAR(128) NOT NULL
,   [table_name]            NVARCHAR(128) NOT NULL
,   [stats_name]            NVARCHAR(128) NOT NULL
,   [stats_is_filtered]     BIT           NOT NULL
,   [seq_nmbr]              BIGINT        NOT NULL
,   [two_part_name]         NVARCHAR(260) NOT NULL
,   [three_part_name]       NVARCHAR(400) NOT NULL
)
WITH
(
    DISTRIBUTION = HASH([seq_nmbr])
,   HEAP
)
```

Temporäre Tabellen können auch erstellt werden, mit einer `CTAS` genau die gleiche Weise:

```sql
CREATE TABLE #stats_ddl
WITH
(
    DISTRIBUTION = HASH([seq_nmbr])
,   HEAP
)
AS
(
SELECT
        sm.[name]                                                               AS [schema_name]
,       tb.[name]                                                               AS [table_name]
,       st.[name]                                                               AS [stats_name]
,       st.[has_filter]                                                         AS [stats_is_filtered]
,       ROW_NUMBER()
        OVER(ORDER BY (SELECT NULL))                                            AS [seq_nmbr]
,                                QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [two_part_name]
,       QUOTENAME(DB_NAME())+'.'+QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [three_part_name]
FROM    sys.objects         AS ob
JOIN    sys.stats           AS st   ON  ob.[object_id]      = st.[object_id]
JOIN    sys.stats_columns   AS sc   ON  st.[stats_id]       = sc.[stats_id]
                                    AND st.[object_id]      = sc.[object_id]
JOIN    sys.columns         AS co   ON  sc.[column_id]      = co.[column_id]
                                    AND sc.[object_id]      = co.[object_id]
JOIN    sys.tables          AS tb   ON  co.[object_id]      = tb.[object_id]
JOIN    sys.schemas         AS sm   ON  tb.[schema_id]      = sm.[schema_id]
WHERE   1=1
AND     st.[user_created]   = 1
GROUP BY
        sm.[name]
,       tb.[name]
,       st.[name]
,       st.[filter_definition]
,       st.[has_filter]
)
SELECT
    CASE @update_type
    WHEN 1
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+');'
    WHEN 2
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH FULLSCAN;'
    WHEN 3
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH SAMPLE '+CAST(@sample_pct AS VARCHAR(20))+' PERCENT;'
    WHEN 4
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH RESAMPLE;'
    END AS [update_stats_ddl]
,   [seq_nmbr]
FROM    t1
;
``` 

>[AZURE.NOTE] `CTAS`ist ein sehr leistungsfähige Befehl und bietet den zusätzlichen Vorteil der effizient hinsichtlich der Verwendung des Speicherplatzes Transaktion. 


## <a name="dropping-temporary-tables"></a>Löschen von temporären Tabellen

Beim Erstellen eine neue Sitzung sollte keine temporären Tabellen vorhanden sein.  Jedoch wenn Sie die gleiche gespeicherte Prozedur der erstellt eine temporäre mit demselben Namen aufrufen, um sicherzustellen, dass Ihre `CREATE TABLE` Anweisungen sind erfolgreich eine einfache vor dem Vorhandensein Überprüfung mit einer `DROP` verwendet werden können, wie in der gezeigten Beispiel:

```sql
IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN
    DROP TABLE #stats_ddl
END
```

Für die Codierung Konsistenz, ist es empfiehlt sich das Verwenden dieses Muster für beide Tabellen und temporäre Tabellen.  Es ist auch eine gute Idee, verwenden Sie `DROP TABLE` temporäre Tabellen zu entfernen, wenn Sie mit ihnen in Ihrem Code abgeschlossen haben.  In der gespeicherten Prozedur Entwicklung, die es kommt zu prüfen, ob die Befehle Ablegen am Ende einer Prozedur, um sicherzustellen, dass diese Objekte zusammenfassen häufig werden bereinigt.

```sql
DROP TABLE #stats_ddl
```

## <a name="modularizing-code"></a>Modularizing code

Da temporäre Tabellen an einer beliebigen Stelle in einer Sitzung für Benutzer angezeigt werden können, können Sie Ihrer Anwendungscode modularisiert können dies genutzt werden.  Beispielsweise vereint die gespeicherte Prozedur unten die empfohlenen Vorgehensweisen von oben DDL generieren, die namentlich Statistik alle Statistiken in der Datenbank aktualisiert wird.

```sql
CREATE PROCEDURE    [dbo].[prc_sqldw_update_stats]
(   @update_type    tinyint -- 1 default 2 fullscan 3 sample 4 resample
    ,@sample_pct     tinyint
)
AS

IF @update_type NOT IN (1,2,3,4)
BEGIN;
    THROW 151000,'Invalid value for @update_type parameter. Valid range 1 (default), 2 (fullscan), 3 (sample) or 4 (resample).',1;
END;

IF @sample_pct IS NULL
BEGIN;
    SET @sample_pct = 20;
END;

IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN
    DROP TABLE #stats_ddl
END

CREATE TABLE #stats_ddl
WITH
(
    DISTRIBUTION = HASH([seq_nmbr])
)
AS
(
SELECT
        sm.[name]                                                               AS [schema_name]
,       tb.[name]                                                               AS [table_name]
,       st.[name]                                                               AS [stats_name]
,       st.[has_filter]                                                         AS [stats_is_filtered]
,       ROW_NUMBER()
        OVER(ORDER BY (SELECT NULL))                                            AS [seq_nmbr]
,                                QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [two_part_name]
,       QUOTENAME(DB_NAME())+'.'+QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [three_part_name]
FROM    sys.objects         AS ob
JOIN    sys.stats           AS st   ON  ob.[object_id]      = st.[object_id]
JOIN    sys.stats_columns   AS sc   ON  st.[stats_id]       = sc.[stats_id]
                                    AND st.[object_id]      = sc.[object_id]
JOIN    sys.columns         AS co   ON  sc.[column_id]      = co.[column_id]
                                    AND sc.[object_id]      = co.[object_id]
JOIN    sys.tables          AS tb   ON  co.[object_id]      = tb.[object_id]
JOIN    sys.schemas         AS sm   ON  tb.[schema_id]      = sm.[schema_id]
WHERE   1=1
AND     st.[user_created]   = 1
GROUP BY
        sm.[name]
,       tb.[name]
,       st.[name]
,       st.[filter_definition]
,       st.[has_filter]
)
SELECT
    CASE @update_type
    WHEN 1
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+');'
    WHEN 2
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH FULLSCAN;'
    WHEN 3
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH SAMPLE '+CAST(@sample_pct AS VARCHAR(20))+' PERCENT;'
    WHEN 4
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH RESAMPLE;'
    END AS [update_stats_ddl]
,   [seq_nmbr]
FROM    t1
;
GO
```

In dieser Phase, die die einzige Aktion, die aufgetreten ist, die Erstellung einer gespeicherten Prozedur wird, wird einfach, die generiert einer temporären Tabelle, #Stats_ddl, mit DDL-Anweisungen aus.  Diese gespeicherte Prozedur wird #Stats_ddl ablegen, wenn sie bereits vorhanden ist, um sicherzustellen, dass er nicht fehlschlägt, wenn mehr als einmal innerhalb einer Sitzung ausführen.  Jedoch, da es ist keine `DROP TABLE` am Ende der gespeicherten Prozedur, klicken Sie nach Abschluss die gespeicherte Prozedur beibehalten die erstellte Tabelle, damit sie außerhalb der gespeicherten Prozedur gelesen werden kann.  In SQL Data Warehouse ist es im Gegensatz zu anderen SQL Server-Datenbanken möglich, die temporäre Tabelle außerhalb der Prozedur verwenden, die sie erstellt haben.  SQL Data Warehouse temporäre Tabellen können verwendeten **an einer beliebigen Stelle** innerhalb der Sitzung sein. Dies kann dazu führen, dass weitere modular und verwaltbaren Code wie in der gezeigten Beispiel:

```sql
EXEC [dbo].[prc_sqldw_update_stats] @update_type = 1, @sample_pct = NULL;

DECLARE @i INT              = 1
,       @t INT              = (SELECT COUNT(*) FROM #stats_ddl)
,       @s NVARCHAR(4000)   = N''

WHILE @i <= @t
BEGIN
    SET @s=(SELECT update_stats_ddl FROM #stats_ddl WHERE seq_nmbr = @i);

    PRINT @s
    EXEC sp_executesql @s
    SET @i+=1;
END

DROP TABLE #stats_ddl;
```

## <a name="temporary-table-limitations"></a>Temporäre Tabelle Einschränkungen

SQL Data Warehouse verschiedene Einschränkungen vorgangseinschränkung, beim temporären Tabellen implementieren.  Derzeit werden nur die Sitzung Suchbegriffs temporäre Tabellen unterstützt.  Globale temporäre Tabellen werden nicht unterstützt.  Darüber hinaus können Ansichten für temporäre Tabellen erstellt werden.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter den Artikeln auf [Tabelle Übersicht][Übersicht], [Tabelle Datentypen][Datentypen], [Verteilen einer Tabelle][verteilen], [Indizieren einer Tabelle][Index], [einer-Tabelle teilen][Partition] und [Verwalten von Tabellenstatistiken][Statistik].  Weitere Informationen zu best Practices finden Sie unter [Bewährte Methoden für SQL Data Warehouse][].

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

<!--MSDN references-->

<!--Other Web references-->

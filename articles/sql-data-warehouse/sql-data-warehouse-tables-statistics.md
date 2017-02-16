<properties
   pageTitle="Verwalten von Tabellen in SQL Data Warehouse Statistiken | Microsoft Azure"
   description="Erste Schritte mit Tabellen in Azure SQL-Data Warehouse Statistik."
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
   ms.date="06/30/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="managing-statistics-on-tables-in-sql-data-warehouse"></a>Verwalten von Statistiken für Tabellen in SQL Data Warehouse

> [AZURE.SELECTOR]
- [(Übersicht)][]
- [Datentypen][]
- [Verteilen][]
- [Index][]
- [Partition][]
- [Statistik][]
- [Temporäre][]

SQL Data Warehouse desto kennt die Daten, die schneller können sie Abfragen für Ihre Daten ausführen.  Die Möglichkeit, dass Sie über Ihre Daten zu SQL Data Warehouse informieren ist durch die Erfassung von Statistiken über Ihre Daten.  Statistik für Ihre Daten vorhanden ist, ist eine der wichtigsten Maßnahmen, die Sie ausführen können, um Ihre Abfragen zu optimieren.  Statistik-Hilfe SQL Data Warehouse den optimalen Plan für Ihre Abfragen erstellen.  Dies ist, da der SQL Data Warehouse Abfrage Optimizer einer Kosten Basis Optimizer ist.  D. h., es vergleicht die Kosten der verschiedenen Abfrage-Pläne und wählt dann den Plan mit den niedrigsten Kosten, der auch den Plan sein sollte, der die schnellste Methode ausgeführt wird.

Statistiken können für eine einzelne Spalte, die mehrere Spalten oder eines Indexes einer Tabelle erstellt werden.  Statistiken werden in ein Histogramm gespeichert, die den Bereichs- und Selektivität von Werten erfasst.  Dies ist von besonderem Interesse, wann muss die Optimizer auswerten Verknüpfungen, GROUP BY, HAVING und WHERE-Klausel in einer Abfrage.  Beispielsweise, wenn der Optimizer schätzt, dass das Datum, das Sie in Ihrer Abfrage filtern gibt 1 Zeile zurück, und es möglicherweise wählen Sie eine andere sehr als If-plan schätzt, dass er Sie Datum ausgewählt haben wird 1 Millionen Zeilen zurückgeben.  Beim Erstellen von Statistiken äußerst wichtig ist, ist es wichtiger wirken sich auf den aktuellen Status der Tabelle dieser Statistik *genau* aus.  Auf dem neuesten Stand Statistik vorhanden ist, wird sichergestellt, dass ein guter Plan von der Optimizer ausgewählt ist.  Der Optimizer erstellte Pläne sind nur so gut wie der Statistik für Ihre Daten.

Die Vorgehensweise zum Erstellen und Aktualisieren von Statistiken gibt es zurzeit eines manuellen Prozesses, aber es ist sehr einfach zu tun ist.  Dies ist im Gegensatz zu SQL Server, die automatisch erstellt und aktualisiert die Statistik für die einzelnen Spalten und Indizes.  Mithilfe der folgenden Informationen können Sie die Verwaltung der Statistiken erheblich für Ihre Daten automatisieren. 

## <a name="getting-started-with-statistics"></a>Erste Schritte mit der Lesbarkeitsstatistik

 Aufgenommene Statistik für jede Spalte erstellen ist eine einfache Möglichkeit, den ersten Schritten mit Statistik.  Da es Statistiken auf dem neuesten Stand zu bleiben gleich wichtig ist, möglicherweise ein konservativen Ansatz Ihrer Statistiken täglich oder nach jedem Laden zu aktualisieren. Es gibt immer Kompromisse zwischen Leistung sowie die Kosten für Statistiken erstellen und aktualisieren.  Wenn Sie, dass es dauert zu lang feststellen, um alle Ihre Statistiken verwalten, möchten Sie möglicherweise versuchen, vorzugehen und über die Spalten Statistiken aufweisen oder welche Spalten häufig aktualisiert werden müssen.  Sie möchten beispielsweise Datumsspalten aktualisieren, als neue Werte hinzugefügt werden können, täglich statt nach jedem Ladevorgang wird. Erneut, werden Sie den größten Nutzen steigern, indem Sie Statistik vorhanden ist, klicken Sie auf die Spalten in Verknüpfungen, GROUP BY, HAVING und WHERE-Klausel.  Wenn Sie eine Tabelle mit vielen Spalten die nur in der SELECT-Klausel verwendet werden haben, Statistiken für diese Spalten möglicherweise nicht unterstützen, und Ausgaben ein wenig mehr Aufwand, um nur die Stelle, an der Lesbarkeitsstatistik hilft, Spalten zu identifizieren können Reduzieren der Uhrzeit der Statistik beibehält.

## <a name="multi-column-statistics"></a>Statistik mit mehreren Spalten

Zusätzlich zum Erstellen von Statistiken auf den einzelnen Spalten ein, kann es passieren, dass Ihre Abfragen von mehrspaltigen Statistik profitieren.  Mit mehreren Spalte Statistiken sind Statistiken für eine Liste der Spalten erstellt.  Sie umfassen einspaltigen Statistiken auf die erste Spalte in der Liste sowie einige Informationen zu Cross-Spalte Korrelationskoeffizienten als Volumenmassen bezeichnet.  Beispielsweise, wenn Sie eine Tabelle verfügen, die in einen anderen zwei Spalten verknüpft ist, kann es passieren SQL Data Warehouse besser den Plan optimieren können, wenn sie die Beziehung zwischen zwei Spalten versteht.   Mit mehreren Spalte Statistiken können Abfrage Leistung für einige Vorgänge wie zusammengesetzte Verknüpfungen und Gruppieren nach.

## <a name="updating-statistics"></a>Aktualisieren von Statistiken

Aktualisieren von Statistiken ist ein wichtiger Bestandteil Ihrer Datenbank Management Routine.  Wenn die Verteilung der Daten in der Datenbank ändern zu können, müssen Statistiken aktualisiert werden.  Veraltete Statistiken führen zu einer keine optimale abfrageleistung.

Eine bewährte Methode besteht darin, statistische Daten zu Datumsspalten jeden Tag aktualisieren, wenn neue Daten hinzugefügt werden.  Jedes Mal neue Zeilen in das Datawarehouse geladen sind, werden die neuen Laden Datumsangaben oder Transaktionsdatumsangaben hinzugefügt werden. Diese Verteilung Daten ändern, und nehmen Sie die Statistik veraltete. Umgekehrt möglicherweise Statistiken einer Land Spalte in einer Kundentabelle nie aktualisiert werden müssen, wie die Verteilung der Werte in der Regel nicht geändert wird. Unter der Voraussetzung, dass die Verteilung zwischen Kunden konstant ist, nicht zur Verfügung Hinzufügen neuer Zeilen in der Tabelle Variation gezeigt, die Verteilung der Daten zu ändern. Jedoch müssen, wenn Ihr Datawarehouse nur eine Land enthält und Daten aus einem neuen Land vorverlegen, Daten aus mehreren Ländern, die gespeichert werden wodurch, Sie auf jeden Fall Statistiken Land Spalte zu aktualisieren.

Eines der ersten Fragen, Fragen Sie bei der Problembehandlung für einer Abfrage ist, "die Statistiken auf dem neuesten Stand sind?"

Diese Frage ist keiner, die durch das Alter der Daten beantwortet werden können. Objekt auf dem neuesten Stand Statistiken werden sehr alten, wenn es keine Material ändern, um die zugrunde liegenden Daten wurde. Wenn Sie die Anzahl der Zeilen erheblich geändert hat, oder es wird eine Änderung der Art Material die Verteilung der Werte für eine bestimmte Spalte *dann* es hat Zeit Statistiken aktualisieren.  

Als Referenz aktualisiert **SQL Server** (nicht SQL Data Warehouse) Statistiken für diese Situationen automatisch:

- Wenn Sie 0 (null) Zeilen in der Tabelle haben, wenn Sie Zeilen hinzufügen, erhalten Sie eine automatische Aktualisierung der Statistiken.
- Wenn Sie mehr als 500 Zeilen zu einer Tabelle, beginnend mit kleiner als 500 Zeilen hinzufügen (z. B. am Anfang Sie 499 und fügen Sie anschließend 500 Zeilen insgesamt 999 Zeilen), erhalten Sie eine automatische Aktualisierung 
- Wenn Sie mehr als 500 Zeilen sind, die müssen Sie 500 zusätzliche Zeilen + 20 % der Größe der Tabelle hinzufügen, bevor Sie eine automatische Aktualisierung auf die Stats sehen

Da es keine DMV ist, um festzustellen, ob die Daten in der Tabelle geändert hat, da die Statistiken zuletzt aktualisiert wurden, kann das Alter Ihrer Statistik wissen Sie mit den Teil des Bilds bereitstellen.  Sie können die folgende Abfrage zum Zeitpunkt der letzten Ihrer Statistiken ermitteln, wobei für jede Tabelle aktualisiert.  

> [AZURE.NOTE] Denken Sie daran, ist eine Änderung der Art Material die Verteilung der Werte für eine bestimmte Spalte, sollten Sie unabhängig von der letzten-Statistiken aktualisieren, die sie aktualisiert wurden.  

```sql
SELECT
    sm.[name] AS [schema_name],
    tb.[name] AS [table_name],
    co.[name] AS [stats_column_name],
    st.[name] AS [stats_name],
    STATS_DATE(st.[object_id],st.[stats_id]) AS [stats_last_updated_date]
FROM
    sys.objects ob
    JOIN sys.stats st
        ON  ob.[object_id] = st.[object_id]
    JOIN sys.stats_columns sc    
        ON  st.[stats_id] = sc.[stats_id]
        AND st.[object_id] = sc.[object_id]
    JOIN sys.columns co    
        ON  sc.[column_id] = co.[column_id]
        AND sc.[object_id] = co.[object_id]
    JOIN sys.types  ty    
        ON  co.[user_type_id] = ty.[user_type_id]
    JOIN sys.tables tb    
        ON  co.[object_id] = tb.[object_id]
    JOIN sys.schemas sm    
        ON  tb.[schema_id] = sm.[schema_id]
WHERE
    st.[user_created] = 1;
```

Datumsspalten in einem Datawarehouse, müssen Sie beispielsweise normalerweise Statistiken Updates häufigere. Jedes Mal neue Zeilen in das Datawarehouse geladen sind, werden die neuen Laden Datumsangaben oder Transaktionsdatumsangaben hinzugefügt werden. Diese Verteilung Daten ändern, und nehmen Sie die Statistik veraltete.  Statistik für eine Spalte Geschlecht auf eine Kundentabelle möglicherweise umgekehrt nie aktualisiert werden müssen. Unter der Voraussetzung, dass die Verteilung zwischen Kunden konstant ist, nicht zur Verfügung Hinzufügen neuer Zeilen in der Tabelle Variation gezeigt, die Verteilung der Daten zu ändern. Jedoch, wenn Ihr Datawarehouse nur ein einziges Geschlecht und eine neue Anforderung Ergebnisse in mehreren Genders enthält müssen Sie auf jeden Fall Statistiken Geschlecht Spalte zu aktualisieren.

Weitere Hinweise finden Sie unter [Statistics][] auf MSDN.

## <a name="implementing-statistics-management"></a>Implementierung von Statistiken management

Es ist oft eine gute Idee, erweitern die Daten geladen Prozess, um sicherzustellen, dass am Ende der Last Statistiken aktualisiert werden. Laden von Daten ist bei Tabellen am häufigsten deren Größe und/oder die Verteilung der Werte zu ändern. Daher ist dies ein logischer Ausgangspunkt für einige Management-Prozesse implementieren.

Einige Grundprinzipien unten wurde für die Statistik beim Laden aktualisieren:

- Stellen Sie sicher, dass jede Tabelle geladen mindestens eine Statistik-Objekt aktualisiert wurde. Dadurch wird die Tabellen Größe (Anzahl der Zeilen und Seitenzahl) Informationen als Teil der Aktualisierung Stats aktualisiert.
- Fokussierung auf Spalten beteiligt teilnehmen, GROUP BY, ORDER BY und DISTINCT-Klausel
- Erwägen Sie das Aktualisieren "aufsteigender Schlüssel" Spalten wie Transaktion Datumsangaben häufiger wie diese Werte in das Histogramm Statistiken nicht berücksichtigt werden.
- Erwägen Sie statische Verteilung Spalten häufig aktualisieren.
- Denken Sie daran, dass jedes Objekt Statistik Reihe aktualisiert wird. Einfach mit der Implementierung `UPDATE STATISTICS <TABLE_NAME>` möglicherweise nicht ideal - besonders für Breite Tabellen mit vielen Statistiken Objekte.

> [AZURE.NOTE] Detaillierte Informationen zur [aufsteigender Schlüssel] Lizenzinformationen finden Sie die SQL Server-2014 Kardinalität Abschätzung Model – Whitepaper.

Weitere Hinweise finden Sie unter MSDN [Kardinalität Abschätzung][] .

## <a name="examples-create-statistics"></a>Beispiele: Statistiken erstellen

In diesen Beispielen zeigen, wie verschiedene Optionen zur Erstellung von Statistiken verwenden. Die Optionen, die Sie für jede Spalte verwenden, abhängig von der Merkmale von Daten und die Verwendung der Spalte in Abfragen.

### <a name="a-create-single-column-statistics-with-default-options"></a>A Erstellen eines einspaltigen Statistiken mit Standardoptionen

Um Statistiken für eine Spalte erstellen möchten, einfach bieten Sie einen Namen für das Objekt Statistiken und den Namen der Spalte.

Diese Syntax verwendet alle Standardoption. Standardmäßig nimmt SQL Data Warehouse 20 % der Tabelle, Statistik verwendet werden.

```sql
CREATE STATISTICS [statistics_name] ON [schema_name].[table_name]([column_name]);
```

Beispiel:

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1);
```

### <a name="b-create-single-column-statistics-by-examining-every-row"></a>B. Erstellen Sie einspaltige Statistiken, indem Sie jede Zeile

Der standardmäßige werden Standardsatz 20 % ist in den meisten Fällen ausreichend. Sie können jedoch den Stichproben anpassen.

Wenn Sie die vollständige Tabelle aufnehmen möchten, verwenden Sie folgende Syntax ein:

```sql
CREATE STATISTICS [statistics_name] ON [schema_name].[table_name]([column_name]) WITH FULLSCAN;
```

Beispiel:

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1) WITH FULLSCAN;
```

### <a name="c-create-single-column-statistics-by-specifying-the-sample-size"></a>C. Erstellen eines einspaltigen Statistiken durch Angabe der Stichprobenumfang

Alternativ können Sie den Umfang der Stichprobe als Prozentwert angeben:

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1) WITH SAMPLE = 50 PERCENT;
```

### <a name="d-create-single-column-statistics-on-only-some-of-the-rows"></a>D EINGETRAGEN. Erstellen eines einspaltigen Statistiken auf nur einige der Zeilen

Eine weitere Möglichkeit, können Sie Statistiken für einen Teil der Zeilen in der Tabelle erstellen. Dies ist eine gefilterte Statistik bezeichnet.

Beispielsweise können Sie gefilterte Statistik, wenn Sie beabsichtigen, einem bestimmten Teil einer großen partitionierten Tabelle Abfragen. Durch das Erstellen von Statistiken auf nur die Arbeitsspeicherpartitionswerte, wird die Genauigkeit der Statistiken verbessern und daher Verbessern der Leistung von Abfragen.

In diesem Beispiel erstellt eine Statistik auf einen Bereich von Werten. Die Werte könnte einfach entsprechend den Bereich von Werten in einer Partition definiert werden.

```sql
CREATE STATISTICS stats_col1 ON table1(col1) WHERE col1 > '2000101' AND col1 < '20001231';
```

> [AZURE.NOTE] Für die Abfrage in Betracht gefilterten Statistik, wenn sie den Plan verteilte Abfrage wählt Optimizer muss die Abfrage in der Definition des Objekts Statistiken passen. Verwenden im vorherige Beispiel, der Abfrage, wo-Klausel SP1 Werte zwischen 2000101 und 20001231 angeben muss.

### <a name="e-create-single-column-statistics-with-all-the-options"></a>E. Erstellen eines einspaltigen Statistiken mit alle Optionen

Sie können, die Optionen natürlich miteinander kombinieren. Im folgenden Beispiel erstellt ein Statistikobjekt gefilterten mit einer benutzerdefinierten Stichprobenumfang:

```sql
CREATE STATISTICS stats_col1 ON table1 (col1) WHERE col1 > '2000101' AND col1 < '20001231' WITH SAMPLE = 50 PERCENT;
```

Den vollständigen Verweis finden Sie unter [CREATE STATISTICS][] auf MSDN.

### <a name="f-create-multi-column-statistics"></a>F. Erstellen von Statistiken mit mehreren Spalten

Zum Erstellen einer mehrspaltigen Statistik einfach in den vorherigen Beispielen verwendet, aber weitere Spalten angeben.

> [AZURE.NOTE] Das Histogramm, die zum Schätzen der Anzahl von Zeilen im Abfrageergebnis verwendet wird, ist nur verfügbar, für die erste Spalte in der Definition der Lesbarkeitsstatistik Objekt aufgeführt.

In diesem Beispiel wird das Histogramm auf *Produkt\_Kategorie*. Cross-Spalte Statistiken werden berechnet auf *Produkt\_Kategorie* und *Produkt\_Sub_c\ategory*:

```sql
CREATE STATISTICS stats_2cols ON table1 (product_category, product_sub_category) WHERE product_category > '2000101' AND product_category < '20001231' WITH SAMPLE = 50 PERCENT;
```

Da es eine Beziehung zwischen ist *Produkt\_Kategorie* und *Produkt\_Sub\_Kategorie*, einer mehrspaltigen Statistik ist nützlich, wenn diese Spalten zur gleichen Zeit zugegriffen werden.

### <a name="g-create-statistics-on-all-the-columns-in-a-table"></a>G. Erstellen von Statistiken für alle Spalten in einer Tabelle

Eine Möglichkeit zum Erstellen von Statistiken ist auf Probleme CREATE STATISTICS Befehle nach dem Erstellen der Tabelle.

```sql
CREATE TABLE dbo.table1
(
   col1 int
,  col2 int
,  col3 int
)
WITH
  (
    CLUSTERED COLUMNSTORE INDEX
  )
;

CREATE STATISTICS stats_col1 on dbo.table1 (col1);
CREATE STATISTICS stats_col2 on dbo.table2 (col2);
CREATE STATISTICS stats_col3 on dbo.table3 (col3);
```

### <a name="h-use-a-stored-procedure-to-create-statistics-on-all-columns-in-a-database"></a>H. Verwenden Sie eine gespeicherte Prozedur zum Erstellen von Statistiken für alle Spalten in einer Datenbank

SQL Data Warehouse verfügt in SQL Server keine gespeicherte Prozedur [Sp_create_stats] [] entspricht. Diese gespeicherte Prozedur erstellt eine einzelne Spalte Statistik-Objekt auf jede Spalte der Datenbank, die bereits Statistiken aufweist.

Dies hilft Ihnen den Einstieg in die Struktur Ihrer Datenbank. Können Sie es an Ihre Bedürfnisse anpassen.

```sql
CREATE PROCEDURE    [dbo].[prc_sqldw_create_stats]
(   @create_type    tinyint -- 1 default 2 Fullscan 3 Sample
,   @sample_pct     tinyint
)
AS

IF @create_type NOT IN (1,2,3)
BEGIN
    THROW 151000,'Invalid value for @stats_type parameter. Valid range 1 (default), 2 (fullscan) or 3 (sample).',1;
END;

IF @sample_pct IS NULL
BEGIN;
    SET @sample_pct = 20;
END;

IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN;
    DROP TABLE #stats_ddl;
END;

CREATE TABLE #stats_ddl
WITH    (   DISTRIBUTION    = HASH([seq_nmbr])
        ,   LOCATION        = USER_DB
        )
AS
WITH T
AS
(
SELECT      t.[name]                        AS [table_name]
,           s.[name]                        AS [table_schema_name]
,           c.[name]                        AS [column_name]
,           c.[column_id]                   AS [column_id]
,           t.[object_id]                   AS [object_id]
,           ROW_NUMBER()
            OVER(ORDER BY (SELECT NULL))    AS [seq_nmbr]
FROM        sys.[tables] t
JOIN        sys.[schemas] s         ON  t.[schema_id]       = s.[schema_id]
JOIN        sys.[columns] c         ON  t.[object_id]       = c.[object_id]
LEFT JOIN   sys.[stats_columns] l   ON  l.[object_id]       = c.[object_id]
                                    AND l.[column_id]       = c.[column_id]
                                    AND l.[stats_column_id] = 1
LEFT JOIN   sys.[external_tables] e ON  e.[object_id]       = t.[object_id]
WHERE       l.[object_id] IS NULL
AND         e.[object_id] IS NULL -- not an external table
)
SELECT  [table_schema_name]
,       [table_name]
,       [column_name]
,       [column_id]
,       [object_id]
,       [seq_nmbr]
,       CASE @create_type
        WHEN 1
        THEN    CAST('CREATE STATISTICS '+QUOTENAME('stat_'+table_schema_name+ '_' + table_name + '_'+column_name)+' ON '+QUOTENAME(table_schema_name)+'.'+QUOTENAME(table_name)+'('+QUOTENAME(column_name)+')' AS VARCHAR(8000))
        WHEN 2
        THEN    CAST('CREATE STATISTICS '+QUOTENAME('stat_'+table_schema_name+ '_' + table_name + '_'+column_name)+' ON '+QUOTENAME(table_schema_name)+'.'+QUOTENAME(table_name)+'('+QUOTENAME(column_name)+') WITH FULLSCAN' AS VARCHAR(8000))
        WHEN 3
        THEN    CAST('CREATE STATISTICS '+QUOTENAME('stat_'+table_schema_name+ '_' + table_name + '_'+column_name)+' ON '+QUOTENAME(table_schema_name)+'.'+QUOTENAME(table_name)+'('+QUOTENAME(column_name)+') WITH SAMPLE '+@sample_pct+'PERCENT' AS VARCHAR(8000))
        END AS create_stat_ddl
FROM T
;

DECLARE @i INT              = 1
,       @t INT              = (SELECT COUNT(*) FROM #stats_ddl)
,       @s NVARCHAR(4000)   = N''
;

WHILE @i <= @t
BEGIN
    SET @s=(SELECT create_stat_ddl FROM #stats_ddl WHERE seq_nmbr = @i);

    PRINT @s
    EXEC sp_executesql @s
    SET @i+=1;
END

DROP TABLE #stats_ddl;
```

Rufen Sie einfach das Verfahren, um Statistiken für alle Spalten in der Tabelle mit diesem Verfahren zu erstellen.

```sql
prc_sqldw_create_stats;
```

## <a name="examples-update-statistics"></a>Beispiele: Update-Statistiken

Um die Statistiken aktualisieren möchten, können Sie folgende Aktionen ausführen:

1. Aktualisieren einer Statistik-Objekt. Geben Sie den Namen des Objekts Statistiken, die Sie aktualisieren möchten.
2. Aktualisieren Sie alle Statistiken Objekte in einer Tabelle ein. Geben Sie den Namen der Tabelle anstelle einer bestimmten Statistik-Objekt.


### <a name="a-update-one-specific-statistics-object"></a>A Aktualisieren einer bestimmten Statistik-Objekt ###
Verwenden Sie die folgende Syntax, um einer bestimmten Statistikobjekt zu aktualisieren:

```sql
UPDATE STATISTICS [schema_name].[table_name]([stat_name]);
```

Beispiel:

```sql
UPDATE STATISTICS [dbo].[table1] ([stats_col1]);
```

Durch Aktualisieren von bestimmter Statistiken Objekten, können Sie Zeit und Ressourcen zum Verwalten von Statistiken erforderlich minimieren. Setzt Gedanken über, durch, wählen Sie die beste Statistiken Objekte zu aktualisieren.


### <a name="b-update-all-statistics-on-a-table"></a>B. Aktualisieren Sie aller Statistiken für eine Tabelle ###
Zeigt eine einfache Methode zum Aktualisieren aller Statistiken Objekte in einer Tabelle.

```sql
UPDATE STATISTICS [schema_name].[table_name];
```

Beispiel:

```sql
UPDATE STATISTICS dbo.table1;
```

Diese Anweisung ist einfach zu verwenden. Denken Sie dies alle Statistiken für die Tabelle aktualisiert und daher möglicherweise mehr Arbeit als nötig ausführen. Wenn die Leistung kein Problem darstellt, ist dies auf jeden Fall die am häufigsten abgeschlossen und einfachste Methode sichergestellt, dass Statistiken auf dem neuesten Stand sind.

> [AZURE.NOTE] Wenn alle Statistiken für eine Tabelle aktualisiert werden soll, unterstützt SQL Data Warehouse eine Überprüfung, zum Beispiel der Tabelle für jede Statistik. Wenn die Tabelle groß ist, enthält viele Spalten und viele Statistiken, es möglicherweise effizienter, einzelne Statistiken je nach Bedarf zu aktualisieren.

Bei einer Implementierung von einer `UPDATE STATISTICS` Verfahren finden Sie unter[temporäre] Artikel [Temporäre Tabellen]. Die Implementierungsmethode ist etwas anders als die `CREATE STATISTICS` vorstehenden Verfahren, aber das Ergebnis ist gleich.

Die vollständige Syntax finden Sie unter [Update Statistics][] auf MSDN.

## <a name="statistics-metadata"></a>Statistik Metadaten
Es gibt verschiedene System anzeigen und Funktionen, die Sie zum Suchen nach Informationen zu Statistiken verwenden können. Beispielsweise können Sie sehen, wenn ein Statistikobjekt möglicherweise veraltete mithilfe der Stats Datumsfunktion angezeigt werden Statistiken letzten erstellt oder aktualisiert wurden.

### <a name="catalog-views-for-statistics"></a>Katalogsansichten für Statistik
Diese Systemansichten finden Sie Informationen zu Statistiken:

| Katalogansicht | Beschreibung |
| :----------- | :---------- |
| [Sys.Columns][]  | Eine Zeile für jede Spalte. |
| [Sys.Objects][]  | Eine Zeile für jedes Objekt in der Datenbank. |  |
| [Sys.Schemas][]  | Eine Zeile für jedes Schema in der Datenbank. |  |
| [Sys.Stats][] | Eine Zeile für jedes Objekt Statistik. |
| [Sys.stats_columns][] | Eine Zeile für jede Spalte in der Statistik-Objekt. Enthält Links zu sys.columns wieder. |
| [' sys.Tables '][] | Eine Zeile für jede Tabelle (externe Tabellen enthält). |
| [Sys.table_types][] | Eine Zeile für jeden Datentyp. |


### <a name="system-functions-for-statistics"></a>Systemfunktionen für Statistik
Diese Systemfunktionen sind nützlich für die Arbeit mit Statistiken:

| System (Funktion) | Beschreibung |
| :-------------- | :---------- |
| [STATS_DATE][]    | Datum, das Objekt Statistik zuletzt aktualisiert wurde. |
| [DBCC SHOW_STATISTICS][] | Zusammenfassung und detaillierter Informationen über die Verteilung der Werte enthält, als vom Objekt Statistik. |

### <a name="combine-statistics-columns-and-functions-into-one-view"></a>Kombinieren von Spalten für Statistiken und Funktionen in einer Ansicht

Diese Ansicht wird von Spalten, die auf Statistik beziehen, und ergibt sich aus der [STATS_DATE()] [] Funktion nicht trennen.

```sql
CREATE VIEW dbo.vstats_columns
AS
SELECT
        sm.[name]                           AS [schema_name]
,       tb.[name]                           AS [table_name]
,       st.[name]                           AS [stats_name]
,       st.[filter_definition]              AS [stats_filter_defiinition]
,       st.[has_filter]                     AS [stats_is_filtered]
,       STATS_DATE(st.[object_id],st.[stats_id])
                                            AS [stats_last_updated_date]
,       co.[name]                           AS [stats_column_name]
,       ty.[name]                           AS [column_type]
,       co.[max_length]                     AS [column_max_length]
,       co.[precision]                      AS [column_precision]
,       co.[scale]                          AS [column_scale]
,       co.[is_nullable]                    AS [column_is_nullable]
,       co.[collation_name]                 AS [column_collation_name]
,       QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])
                                            AS two_part_name
,       QUOTENAME(DB_NAME())+'.'+QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])
                                            AS three_part_name
FROM    sys.objects                         AS ob
JOIN    sys.stats           AS st ON    ob.[object_id]      = st.[object_id]
JOIN    sys.stats_columns   AS sc ON    st.[stats_id]       = sc.[stats_id]
                            AND         st.[object_id]      = sc.[object_id]
JOIN    sys.columns         AS co ON    sc.[column_id]      = co.[column_id]
                            AND         sc.[object_id]      = co.[object_id]
JOIN    sys.types           AS ty ON    co.[user_type_id]   = ty.[user_type_id]
JOIN    sys.tables          AS tb ON  co.[object_id]        = tb.[object_id]
JOIN    sys.schemas         AS sm ON  tb.[schema_id]        = sm.[schema_id]
WHERE   1=1
AND     st.[user_created] = 1
;
```

## <a name="dbcc-showstatistics-examples"></a>DBCC SHOW_STATISTICS() Beispiele

DBCC SHOW_STATISTICS() zeigt Daten in einer Statistik-Objekt. Diese Daten stammen in drei Teilen.

1. Kopfzeile
2. Dichtefunktion Vektor
3. Histogramm

Die Kopfzeile Metadaten zu den Statistiken. Das Histogramm zeigt die Verteilung der Werte in der ersten Schlüsselspalte des Objekts Statistik. Die Dichtefunktion Vektor Measures Cross-Spalte Korrelationskoeffizienten. SQLDW berechnet Kardinalität schätzt mit Daten in das Objekt Statistik.

### <a name="show-header-density-and-histogram"></a>Anzeigen von Kopf- und Dichtefunktion Histogramm

Dieses einfache Beispiel zeigt alle drei Teile eines Objekts Statistik.

```sql
DBCC SHOW_STATISTICS([<schema_name>.<table_name>],<stats_name>)
```

Beispiel:

```sql
DBCC SHOW_STATISTICS (dbo.table1, stats_col1);
```

### <a name="show-one-or-more-parts-of-dbcc-showstatistics"></a>Zeigen Sie eine oder mehrere Teile DBCC SHOW_STATISTICS();

Wenn Sie nur bestimmte Teile anzeigen möchten, verwenden Sie die `WITH` Klausel, und geben Sie welche Teile angezeigt werden sollen:

```sql
DBCC SHOW_STATISTICS([<schema_name>.<table_name>],<stats_name>) WITH stat_header, histogram, density_vector
```

Beispiel:

```sql
DBCC SHOW_STATISTICS (dbo.table1, stats_col1) WITH histogram, density_vector
```

## <a name="dbcc-showstatistics-differences"></a>DBCC SHOW_STATISTICS() Unterschiede
DBCC SHOW_STATISTICS() ist mehr grundsätzlich in SQL Data Warehouse im Vergleich zu SQL Server implementiert werden.

1. Nicht dokumentierten Features werden nicht unterstützt.
- Stats_stream kann nicht verwendet werden.
- Ergebnisse nach einer bestimmten Untermenge der Lesbarkeitsstatistik Daten können nicht z. B. teilnehmen (STAT_HEADER Verknüpfung DENSITY_VECTOR)
2. NO_INFOMSGS kann für Nachricht unterdrücken festgelegt werden
3. Quadrat Klammern Statistiknamen kann nicht verwendet werden
4. Spaltennamen können nicht verwendet werden, um Statistiken Objekte zu identifizieren
5. Benutzerdefinierter Fehler 2767 wird nicht unterstützt.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen hierzu finden Sie unter [DBCC SHOW_STATISTICS][] auf MSDN.  Weitere Informationen finden Sie unter den Artikeln auf [Tabelle Übersicht][Übersicht], [Tabelle Datentypen][Datentypen], [Verteilen einer Tabelle][verteilen], [Indizieren einer Tabelle][Index], [eine-Tabelle teilen][Partition] und [Temporäre Tabellen][temporäre].  Weitere Informationen zu best Practices finden Sie unter [Bewährte Methoden für SQL Data Warehouse][].  

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
[Kardinalität Schätzung]: https://msdn.microsoft.com/library/dn600374.aspx
[ERSTELLEN VON STATISTIKEN]: https://msdn.microsoft.com/library/ms188038.aspx
[DBCC SHOW_STATISTICS]:https://msdn.microsoft.com/library/ms174384.aspx
[Statistik]: https://msdn.microsoft.com/library/ms190397.aspx
[STATS_DATE]: https://msdn.microsoft.com/library/ms190330.aspx
[Sys.Columns]: https://msdn.microsoft.com/library/ms176106.aspx
[Sys.Objects]: https://msdn.microsoft.com/library/ms190324.aspx
[Sys.Schemas]: https://msdn.microsoft.com/library/ms190324.aspx
[Sys.Stats]: https://msdn.microsoft.com/library/ms177623.aspx
[Sys.stats_columns]: https://msdn.microsoft.com/library/ms187340.aspx
[' sys.Tables ']: https://msdn.microsoft.com/library/ms187406.aspx
[Sys.table_types]: https://msdn.microsoft.com/library/bb510623.aspx
[AKTUALISIEREN VON STATISTIKEN]: https://msdn.microsoft.com/library/ms187348.aspx

<!--Other Web references-->  

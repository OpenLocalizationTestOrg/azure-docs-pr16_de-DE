<properties
   pageTitle="Optimieren der Transaktionen für SQL Data Warehouse | Microsoft Azure"
   description="Bewährte Methode Anleitungen effiziente Transaktion Aktualisierungen in Azure SQL-Data Warehouse schreiben"
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
   ms.date="07/31/2016"
   ms.author="jrj;barbkess"/>

# <a name="optimizing-transactions-for-sql-data-warehouse"></a>Transaktionen für SQL Data Warehouse optimieren

In diesem Artikel wird erläutert, wie Sie die Leistung des Codes Transaktionen während Risikos für lange entwurfsbearbeitung optimieren.

## <a name="transactions-and-logging"></a>Transaktionen und Protokollierung

Transaktionen sind ein wichtiger Bestandteil einer relationalen Datenbank-Engine. SQL Data Warehouse verwendet Transaktionen während der Änderung von Daten. Diese Transaktionen können explizit oder implizit sein. Einzelne `INSERT`, `UPDATE` und `DELETE` Anweisungen finden Sie alle Beispiele für implizite Transaktionen. Explizite Transaktionen werden von einem Entwickler mit explizit geschrieben `BEGIN TRAN`, `COMMIT TRAN` oder `ROLLBACK TRAN` und in der Regel verwendet, wenn mehrere Änderung Anweisungen zusammen in einer einzelnen atomare Einheit verknüpft werden müssen. 

Azure SQL-Data Warehouse übergibt Änderungen an der Datenbank Transaktionsprotokolle verwenden. Jede Verteilung verfügt über eine eigene Transaktionslog. Transaktion Log schreibt erfolgen automatisch. Es ist keine Konfiguration erforderlich. Während dieses Verfahren wird das Schreiben sichergestellt es als Aufwand im System vorstellen. Sie können diese Auswirkung minimieren, indem überführen effizienten Code schreiben. Überführen effizienten Code Fällt gestreut in zwei Kategorien.

- Nutzen Sie minimale Protokollierung Konstrukte, soweit möglich
- Verarbeiten von Daten mithilfe des Suchbegriffs Blattnamen im singular zeitintensive Transaktionen zu vermeiden.
- Übernehmen einer Partition Muster für umfangreiche Änderungen an eine bestimmte Partition wechseln

## <a name="minimal-vs-full-logging"></a>Minimale im Vergleich zur vollständigen Protokollierung

Im Gegensatz zu vollständig protokollierte Vorgänge, die das Transaktionsprotokoll verwenden, um jede Zeile ändern verfolgen, Nachverfolgen von minimal protokollierte Operationen sind und nur die Metadaten Änderungen. Daher minimale Protokollierung umfasst nur die benötigten Informationen zum Zurücksetzen der Transaktion bei einem Ausfall oder eine explizite Anforderung Protokollierung (`ROLLBACK TRAN`). Wie viel weniger Informationen im Protokoll nachverfolgt wird, führt eine minimal protokollierte besser als gleicher Größe vollständig protokollierten Vorgangs. Darüber hinaus, da weniger schreibt das Transaktionsprotokoll wechseln, wird eine viel kleinere Datenmenge Log generiert und daher Weitere i/o effiziente.

Die Transaktion Sicherheit Grenzwerte gelten nur für vollständig protokollierte Vorgänge.

>[AZURE.NOTE] Minimal protokollierte Operationen können explizite Transaktionen teilnehmen. Wie alle Änderungen im Zuteilung Strukturen erfasst werden, ist es möglich, die Anwendung minimal protokollierte Operationen zurückzusetzen. Es ist wichtig zu verstehen, dass die Änderung "Minimal" angemeldet ist es ist nicht dauerhaften protokollierten.

## <a name="minimally-logged-operations"></a>Minimal protokollierte Operationen

Die folgenden Vorgänge sind in der Lage der minimal protokolliert wird:

- Erstellen Sie die Tabelle AS SELECT ([CTAS][])
- EINFÜGEN VON... WÄHLEN SIE AUS
- INDEX ERSTELLEN
- ALTER INDEX NEU ERSTELLEN
- INDEX LÖSCHEN
- TABELLE VERKÜRZEN
- TABELLE LÖSCHEN
- ALTER WECHSELN TABELLENPARTITION

<!--
- MERGE
- UPDATE on LOB Types .WRITE
- SELECT..INTO
-->

>[AZURE.NOTE] Interner Bewegung Datenoperationen (z. B. `BROADCAST` und `SHUFFLE`) sind von den Transaktion Sicherheit Grenzwert nicht betroffen.

## <a name="minimal-logging-with-bulk-load"></a>Minimale Protokollierung mit Massenladen

`CTAS`und `INSERT...SELECT` sind beide gruppenweise laden Vorgänge. Beide, jedoch werden durch die Definition des Ziels Tabelle beeinflusst und abhängig von dem Szenario laden. Nachfolgend finden Sie eine Tabelle, die erläutert, wenn die Masse Operation vollständig oder minimal angemeldet sein:  

| Primärer Index               | Szenario laden                                            | Protokollierungsmodus |
| --------------------------- | -------------------------------------------------------- | ------------ |
| Heap                        | Alle                                                      | **Minimale**  |
| Gruppierte Index             | Leere Zieltabelle                                       | **Minimale**  |
| Gruppierte Index             | Geladene Zeilen überlappen sich nicht mit vorhandenen Seiten in der Zielliste | **Minimale**  |
| Gruppierte Index             | Geladene Zeilen mit vorhandenen Seiten in der Zielliste überlappen        | Vollständige         |
| Gruppierte Columnstore Index | Stapelverarbeitung Größe > = 102,400 pro Partition ausgerichtete Verteilung | **Minimale**  |
| Gruppierte Columnstore Index | Stapelverarbeitung Größe < 102,400 pro Partition ausgerichtete Verteilung  | Vollständige         |

Es ist zu beachten, dass alle schreibt Aktualisieren des sekundären oder nicht gruppierten Indizes immer vollständig protokollierte Vorgänge genommen werden.

> [AZURE.IMPORTANT] SQL Data Warehouse weist 60 Verteilung an. Daher Voraussetzung alle Zeilen gleichmäßig verteilt werden und in einer Einzelpartition Start, Ihren Stapel müssen 6,144,000 Zeilen enthalten oder größere minimal angemeldet sein, wenn in einem gruppierten Columnstore Index geschrieben. Wenn die Tabelle aufgeteilt und die eingefügten Zeilen Partitionsgrenzen erstrecken, werden Sie 6,144,000 Zeilen pro Partition Begrenzungslinie unter der Voraussetzung auch Daten Verteilung benötigen. Jede Partition in jede Verteilung muss unabhängig voneinander den 102.400 Zeile-Schwellenwert für das Einfügen aus, um die Verteilung minimal angemeldet sein überschreiten.

Laden von Daten in einer nicht leeren Tabelle mit einem gruppierten Index kann oft eine Mischung vollständig protokollierte und minimal protokollierte Zeilen enthalten. Einem gruppierten Index handelt es sich um eine angeglichene Struktur (b-Struktur) von Seiten. Wenn die Seite geschrieben werden, um Zeilen aus einer anderen Transaktion bereits enthält, werden diese schreibt vollständig protokolliert. Jedoch ist die Seite leer wird dann der Schreibzugriff auf dieser Seite minimal protokolliert.

## <a name="optimizing-deletes"></a>Löscht optimieren

`DELETE`ist vollständig protokollierten Vorgangs an.  Wenn Sie eine große Datenmenge in einer Tabelle oder mit löschen müssen, ist es häufig sinnvoller, `SELECT` die Daten zu halten, die als minimal protokollierte Vorgang ausgeführt werden kann.  Um dies zu erreichen, erstellen Sie eine neue Tabelle mit [CTAS][]aus.  Nach dem Erstellen, verwenden Sie [Umbenennen][] , die alte Tabelle mit der neu erstellten Tabelle auszutauschen.

```sql
-- Delete all sales transactions for Promotions except PromotionKey 2.

--Step 01. Create a new table select only the records we want to kep (PromotionKey 2)
CREATE TABLE [dbo].[FactInternetSales_d]
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES  (   20000101, 20010101, 20020101, 20030101, 20040101, 20050101
                                                ,   20060101, 20070101, 20080101, 20090101, 20100101, 20110101
                                                ,   20120101, 20130101, 20140101, 20150101, 20160101, 20170101
                                                ,   20180101, 20190101, 20200101, 20210101, 20220101, 20230101
                                                ,   20240101, 20250101, 20260101, 20270101, 20280101, 20290101
                                                )
)
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
WHERE   [PromotionKey] = 2
OPTION (LABEL = 'CTAS : Delete')
;

--Step 02. Rename the Tables to replace the 
RENAME OBJECT [dbo].[FactInternetSales]   TO [FactInternetSales_old];
RENAME OBJECT [dbo].[FactInternetSales_d] TO [FactInternetSales];
```

## <a name="optimizing-updates"></a>Optimieren von updates

`UPDATE`ist vollständig protokollierten Vorgangs an.  Wenn Sie eine große Anzahl von Zeilen in einer Tabelle oder mit aktualisieren müssen kann es häufig wesentlich effizienter mit einer minimal protokollierte Operation wie [CTAS][] vergeblich sein.

Im folgenden Beispiel wird anhand einer vollständigen Tabelle wurde in Update konvertiert eine `CTAS` , sodass die minimale Protokollierung möglich ist.

In diesem Fall, wir einen Rabattbetrag retrospectively Umsätze in der Tabelle hinzufügen:

```sql
--Step 01. Create a new table containing the "Update". 
CREATE TABLE [dbo].[FactInternetSales_u]
WITH
(   CLUSTERED INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES  (   20000101, 20010101, 20020101, 20030101, 20040101, 20050101
                                                ,   20060101, 20070101, 20080101, 20090101, 20100101, 20110101
                                                ,   20120101, 20130101, 20140101, 20150101, 20160101, 20170101
                                                ,   20180101, 20190101, 20200101, 20210101, 20220101, 20230101
                                                ,   20240101, 20250101, 20260101, 20270101, 20280101, 20290101
                                                )
                )
)
AS 
SELECT
    [ProductKey]  
,   [OrderDateKey] 
,   [DueDateKey]  
,   [ShipDateKey] 
,   [CustomerKey] 
,   [PromotionKey] 
,   [CurrencyKey] 
,   [SalesTerritoryKey]
,   [SalesOrderNumber]
,   [SalesOrderLineNumber]
,   [RevisionNumber]
,   [OrderQuantity]
,   [UnitPrice]
,   [ExtendedAmount]
,   [UnitPriceDiscountPct]
,   ISNULL(CAST(5 as float),0) AS [DiscountAmount]
,   [ProductStandardCost]
,   [TotalProductCost]
,   ISNULL(CAST(CASE WHEN [SalesAmount] <=5 THEN 0
         ELSE [SalesAmount] - 5
         END AS MONEY),0) AS [SalesAmount]
,   [TaxAmt]
,   [Freight]
,   [CarrierTrackingNumber] 
,   [CustomerPONumber]
FROM    [dbo].[FactInternetSales]
OPTION (LABEL = 'CTAS : Update')
;

--Step 02. Rename the tables
RENAME OBJECT [dbo].[FactInternetSales]   TO [FactInternetSales_old];
RENAME OBJECT [dbo].[FactInternetSales_u] TO [FactInternetSales];

--Step 03. Drop the old table
DROP TABLE [dbo].[FactInternetSales_old]
```

> [AZURE.NOTE] Neuerstellen von großen Tabellen kann nutzbringend verwenden SQL Data Warehouse Arbeitsbelastung Verwaltungsfunktionen. Weitere Informationen hierzu finden Sie im Abschnitt Arbeitsbelastung verwalten im Artikel [Concurrency][] .

## <a name="optimizing-with-partition-switching"></a>Optimieren mit Partition wechseln

Bei großem Umfang Änderungen innerhalb einer [Tabellenpartition][]macht eine Partition umsteigen Muster zahlreiche sinnvoll. Wenn die Änderung von Daten von Bedeutung und mehrere Partitionen umfasst, klicken Sie dann einfach die Partitionen durchlaufen gleiche wird das Ergebnis erzielt.

Die Schritte zum Ausführen einer Option Partition sind wie folgt aus:
1. Erstellen Sie eine leere, partition
2. Führen Sie das 'aktualisieren' als eine CTAS
3. Wechseln Sie zu der Tabelle nicht mehr vorhandene Daten
4. Wechseln Sie in die neuen Daten
5. Löschen Sie die Daten

Jedoch zum Identifizieren die Partitionen wechseln wird müssen Sie zuerst eine Prozedur Helper, wie etwa den folgenden zu erstellen. 

```sql
CREATE PROCEDURE dbo.partition_data_get
    @schema_name           NVARCHAR(128)
,   @table_name            NVARCHAR(128)
,   @boundary_value        INT
AS
IF OBJECT_ID('tempdb..#ptn_data') IS NOT NULL
BEGIN
    DROP TABLE #ptn_data
END
CREATE TABLE #ptn_data
WITH    (   DISTRIBUTION = ROUND_ROBIN
        ,   HEAP
        )
AS
WITH CTE
AS
(
SELECT  s.name                          AS [schema_name]
,       t.name                          AS [table_name]
,       p.partition_number              AS [ptn_nmbr]
,       p.[rows]                        AS [ptn_rows]
,       CAST(r.[value] AS INT)          AS [boundary_value]
FROM        sys.schemas                 AS s
JOIN        sys.tables                  AS t    ON  s.[schema_id]       = t.[schema_id]
JOIN        sys.indexes                 AS i    ON  t.[object_id]       = i.[object_id]
JOIN        sys.partitions              AS p    ON  i.[object_id]       = p.[object_id] 
                                                AND i.[index_id]        = p.[index_id] 
JOIN        sys.partition_schemes       AS h    ON  i.[data_space_id]   = h.[data_space_id]
JOIN        sys.partition_functions     AS f    ON  h.[function_id]     = f.[function_id]
LEFT JOIN   sys.partition_range_values  AS r    ON  f.[function_id]     = r.[function_id] 
                                                AND r.[boundary_id]     = p.[partition_number]
WHERE i.[index_id] <= 1
)
SELECT  *
FROM    CTE
WHERE   [schema_name]       = @schema_name
AND     [table_name]        = @table_name
AND     [boundary_value]    = @boundary_value
OPTION (LABEL = 'dbo.partition_data_get : CTAS : #ptn_data')
;
GO
```

Diese Prozedur maximiert Code wieder verwenden und behält die Partition Beispiel kompaktere wechseln.

Der nachstehende Code veranschaulicht die fünf Schritte weiter oben erwähnten, um eine vollständige Partition Routine wechseln zu erzielen.

```sql
--Create a partitioned aligned empty table to switch out the data 
IF OBJECT_ID('[dbo].[FactInternetSales_out]') IS NOT NULL
BEGIN
    DROP TABLE [dbo].[FactInternetSales_out]
END

CREATE TABLE [dbo].[FactInternetSales_out]
WITH
(   DISTRIBUTION = HASH([ProductKey])
,   CLUSTERED COLUMNSTORE INDEX
,   PARTITION   (   [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES  (   20020101, 20030101
                                                )
                )
)
AS
SELECT *
FROM    [dbo].[FactInternetSales]
WHERE 1=2
OPTION (LABEL = 'CTAS : Partition Switch IN : UPDATE')
;

--Create a partitioned aligned table and update the data in the select portion of the CTAS
IF OBJECT_ID('[dbo].[FactInternetSales_in]') IS NOT NULL
BEGIN
    DROP TABLE [dbo].[FactInternetSales_in]
END

CREATE TABLE [dbo].[FactInternetSales_in]
WITH
(   DISTRIBUTION = HASH([ProductKey])
,   CLUSTERED COLUMNSTORE INDEX
,   PARTITION   (   [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES  (   20020101, 20030101
                                                )
                )
)
AS 
SELECT
    [ProductKey]  
,   [OrderDateKey] 
,   [DueDateKey]  
,   [ShipDateKey] 
,   [CustomerKey] 
,   [PromotionKey] 
,   [CurrencyKey] 
,   [SalesTerritoryKey]
,   [SalesOrderNumber]
,   [SalesOrderLineNumber]
,   [RevisionNumber]
,   [OrderQuantity]
,   [UnitPrice]
,   [ExtendedAmount]
,   [UnitPriceDiscountPct]
,   ISNULL(CAST(5 as float),0) AS [DiscountAmount]
,   [ProductStandardCost]
,   [TotalProductCost]
,   ISNULL(CAST(CASE WHEN [SalesAmount] <=5 THEN 0
         ELSE [SalesAmount] - 5
         END AS MONEY),0) AS [SalesAmount]
,   [TaxAmt]
,   [Freight]
,   [CarrierTrackingNumber] 
,   [CustomerPONumber]
FROM    [dbo].[FactInternetSales]
WHERE   OrderDateKey BETWEEN 20020101 AND 20021231
OPTION (LABEL = 'CTAS : Partition Switch IN : UPDATE')
;

--Use the helper procedure to identify the partitions
--The source table
EXEC dbo.partition_data_get 'dbo','FactInternetSales',20030101
DECLARE @ptn_nmbr_src INT = (SELECT ptn_nmbr FROM #ptn_data)
SELECT @ptn_nmbr_src

--The "in" table
EXEC dbo.partition_data_get 'dbo','FactInternetSales_in',20030101
DECLARE @ptn_nmbr_in INT = (SELECT ptn_nmbr FROM #ptn_data)
SELECT @ptn_nmbr_in

--The "out" table
EXEC dbo.partition_data_get 'dbo','FactInternetSales_out',20030101
DECLARE @ptn_nmbr_out INT = (SELECT ptn_nmbr FROM #ptn_data)
SELECT @ptn_nmbr_out

--Switch the partitions over
DECLARE @SQL NVARCHAR(4000) = '
ALTER TABLE [dbo].[FactInternetSales]   SWITCH PARTITION '+CAST(@ptn_nmbr_src AS VARCHAR(20))   +' TO [dbo].[FactInternetSales_out] PARTITION ' +CAST(@ptn_nmbr_out AS VARCHAR(20))+';
ALTER TABLE [dbo].[FactInternetSales_in] SWITCH PARTITION '+CAST(@ptn_nmbr_in AS VARCHAR(20))   +' TO [dbo].[FactInternetSales] PARTITION '     +CAST(@ptn_nmbr_src AS VARCHAR(20))+';'
EXEC sp_executesql @SQL

--Perform the clean-up
TRUNCATE TABLE dbo.FactInternetSales_out;
TRUNCATE TABLE dbo.FactInternetSales_in;

DROP TABLE dbo.FactInternetSales_out
DROP TABLE dbo.FactInternetSales_in
DROP TABLE #ptn_data
```

## <a name="minimize-logging-with-small-batches"></a>Protokollierung mit kleinen Blattnamen minimieren

Für große Änderung Datenoperationen kann es sinnvoll ist, den Vorgang in Blöcken oder Stapeln zur die Arbeitseinheit Bereich aufgeteilt durchführen.

Nachfolgend finden Sie ein Arbeitsbeispiel zu erhalten. Die Stapelgröße weist eine einfache Zahl festgelegt wurde, markieren Sie das Verfahren. In der Praxis wäre die Stapelgröße erheblich größer. 

```sql
SET NO_COUNT ON;
IF OBJECT_ID('tempdb..#t') IS NOT NULL
BEGIN
    DROP TABLE #t;
    PRINT '#t dropped';
END

CREATE TABLE #t
WITH    (   DISTRIBUTION = ROUND_ROBIN
        ,   HEAP
        )
AS
SELECT  ROW_NUMBER() OVER(ORDER BY (SELECT NULL)) AS seq_nmbr
,       SalesOrderNumber
,       SalesOrderLineNumber
FROM    dbo.FactInternetSales
WHERE   [OrderDateKey] BETWEEN 20010101 and 20011231
;

DECLARE @seq_start      INT = 1
,       @batch_iterator INT = 1
,       @batch_size     INT = 50
,       @max_seq_nmbr   INT = (SELECT MAX(seq_nmbr) FROM dbo.#t)
;

DECLARE @batch_count    INT = (SELECT CEILING((@max_seq_nmbr*1.0)/@batch_size))
,       @seq_end        INT = @batch_size
;

SELECT COUNT(*)
FROM    dbo.FactInternetSales f

PRINT 'MAX_seq_nmbr '+CAST(@max_seq_nmbr AS VARCHAR(20))
PRINT 'MAX_Batch_count '+CAST(@batch_count AS VARCHAR(20))

WHILE   @batch_iterator <= @batch_count
BEGIN
    DELETE
    FROM    dbo.FactInternetSales
    WHERE EXISTS
    (
            SELECT  1
            FROM    #t t
            WHERE   seq_nmbr BETWEEN  @seq_start AND @seq_end
            AND     FactInternetSales.SalesOrderNumber      = t.SalesOrderNumber
            AND     FactInternetSales.SalesOrderLineNumber  = t.SalesOrderLineNumber
    )
    ;

    SET @seq_start = @seq_end
    SET @seq_end = (@seq_start+@batch_size);
    SET @batch_iterator +=1;
END
```

## <a name="pause-and-scaling-guidance"></a>Anhalten und dieselbe Skalierung Anleitungen

Azure SQL-Data Warehouse ermöglicht Ihnen anhalten, fortsetzen und Ihre Datawarehouse bei Bedarf skalieren. Beim Zeigen Sie oder Ihr SQL Data Warehouse skalieren ist es wichtig zu verstehen, dass alle Flugzeug Transaktionen sofort beendet werden. bewirken, dass alle offenen Transaktionen zurückgesetzt werden. Wenn Ihre Arbeitsbelastung eine lange ausgeführt werden und unvollständig Änderung von Daten vor dem Anhalten oder Maßstab ausgestellt haben, müssen diese Arbeit nicht rückgängig gemacht werden. Dies kann die Zeit auswirken, zeigen Sie oder Ihre Datenbank Azure SQL-Data Warehouse skalieren dauert. 

> [AZURE.IMPORTANT] Beide `UPDATE` und `DELETE` sind vollständig protokollierte Vorgänge und, damit diese Rückgängig/Wiederholen Vorgänge können dauern erheblich länger als Entsprechung minimal Vorgänge protokolliert. 

Die für den günstigsten besteht darin, in Flight Datenbearbeitungstransaktionen vollständig sind, bevor anhalten oder Skalierung SQL Data Warehouse Änderung informieren. Jedoch kann dies nicht immer Praxis sein. Um das Risiko eine lange zurücksetzen zu verringern, sollten Sie eine der folgenden Optionen aus:

- Zeitintensive Vorgänge neu schreiben [CTAS][] verwenden
- Teilen Sie den Vorgang ab in Segmente; Klicken Sie auf eine Teilmenge der Zeilen in Betrieb

## <a name="next-steps"></a>Nächste Schritte

Finden Sie unter [Transaktionen in SQL Data Warehouse][] erfahren Sie mehr über die Isolationsebenen und Transaktionen Grenzwerte.  Eine Übersicht über andere bewährte Methoden finden Sie unter [Bewährte Methoden für SQL Data Warehouse][].

<!--Image references-->

<!--Article references-->
[Transaktionen in SQL Datawarehouse]: ./sql-data-warehouse-develop-transactions.md
[Tabellenpartition]: ./sql-data-warehouse-tables-partition.md
[Parallelität]: ./sql-data-warehouse-develop-concurrency.md
[CTAS]: ./sql-data-warehouse-develop-ctas.md
[Bewährte Methoden für SQL Datawarehouse]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->
[alter index]:https://msdn.microsoft.com/library/ms188388.aspx
[UMBENENNEN]: https://msdn.microsoft.com/library/mt631611.aspx

<!-- Other web references -->


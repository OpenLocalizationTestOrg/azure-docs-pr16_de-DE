<properties
   pageTitle="Erstellen Sie die Tabelle als SQL Data Warehouse (CTAS) Wählen Sie | Microsoft Azure"
   description="Tipps für die Entwicklung von Lösungen mit der Tabelle erstellen als select (CTAS)-Anweisung in Azure SQL-Data Warehouse Codierung."
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
   ms.date="06/14/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="create-table-as-select-ctas-in-sql-data-warehouse"></a>Erstellen Sie die Tabelle als SELECT-Anweisung (CTAS) in SQL Datawarehouse
Erstellen der Tabelle als SELECT-Anweisung oder `CTAS` ist eines der wichtigsten Features von T-SQL verfügbar. Es ist eine vollständig parallelisierte Vorgang, der eine neue Tabelle basierend auf die Ausgabe einer SELECT-Anweisung erstellt. `CTAS`ist die einfachste und schnellste Möglichkeit dar, um eine Kopie einer Tabelle zu erstellen. Erwägen Sie dies eine FitNesse Version von `SELECT..INTO` , wenn Sie möchten. Dieses Dokument bietet sowohl Beispiele und bewährte Methoden für die `CTAS`.

## <a name="using-ctas-to-copy-a-table"></a>Verwenden CTAS zum Kopieren einer Tabelle

Vielleicht eine der am häufigsten verwendeten verwendet der `CTAS` wird eine Kopie einer Tabelle erstellen, damit Sie die DDL ändern können. Wenn beispielsweise Ihre Tabelle als ursprünglich erstellt haben `ROUND_ROBIN` und jetzt möchten, ändern Sie ihn in eine Tabelle auf eine Spalte verteilt `CTAS` ist, wie Sie die Spalte Verteilung ändern möchten. `CTAS`kann auch verwendet werden, Partitionierung, Indizierung oder Spalte Arten ändern.

Angenommen, Sie dieser Tabelle mithilfe den Verteilung Standardtyp erstellt `ROUND_ROBIN` verteilt, da keine Verteilung-Spalte, in angegeben wurde der `CREATE TABLE`.

```sql
CREATE TABLE FactInternetSales
(
    ProductKey int NOT NULL,
    OrderDateKey int NOT NULL,
    DueDateKey int NOT NULL,
    ShipDateKey int NOT NULL,
    CustomerKey int NOT NULL,
    PromotionKey int NOT NULL,
    CurrencyKey int NOT NULL,
    SalesTerritoryKey int NOT NULL,
    SalesOrderNumber nvarchar(20) NOT NULL,
    SalesOrderLineNumber tinyint NOT NULL,
    RevisionNumber tinyint NOT NULL,
    OrderQuantity smallint NOT NULL,
    UnitPrice money NOT NULL,
    ExtendedAmount money NOT NULL,
    UnitPriceDiscountPct float NOT NULL,
    DiscountAmount float NOT NULL,
    ProductStandardCost money NOT NULL,
    TotalProductCost money NOT NULL,
    SalesAmount money NOT NULL,
    TaxAmt money NOT NULL,
    Freight money NOT NULL,
    CarrierTrackingNumber nvarchar(25),
    CustomerPONumber nvarchar(25)
);
```

Nun möchten Sie eine neue Instanz von dieser Tabelle mit einem gruppierten Columnstore Index zu erstellen, damit Sie die Leistung von Columnstore gruppierte Tabellen nutzen können. Sie auch in der Tabelle auf ProductKey verteilt werden, da Sie anhand dieser Spalte Verknüpfungen erwartet werden sollen und Verschieben von Daten während der Verknüpfungen auf ProductKey zu vermeiden möchten. Und schließlich möchten auch hinzufügen, Partitionierung auf OrderDateKey, damit Sie schnell alte Daten durch Ablegen alte Partitionen löschen können. Hier ist die CTAS-Anweisung, die die alte Tabelle in eine neue Tabelle kopieren möchten.

```sql
CREATE TABLE FactInternetSales_new
WITH
(
    CLUSTERED COLUMNSTORE INDEX,
    DISTRIBUTION = HASH(ProductKey),
    PARTITION
    (
        OrderDateKey RANGE RIGHT FOR VALUES
        (
        20000101,20010101,20020101,20030101,20040101,20050101,20060101,20070101,20080101,20090101,
        20100101,20110101,20120101,20130101,20140101,20150101,20160101,20170101,20180101,20190101,
        20200101,20210101,20220101,20230101,20240101,20250101,20260101,20270101,20280101,20290101
        )
    )
)
AS SELECT * FROM FactInternetSales;
```

Schließlich können Sie Ihre Tabellen in der neuen Tabelle austauschen, und legen dann die alte Tabelle umbenennen.

```sql
RENAME OBJECT FactInternetSales TO FactInternetSales_old;
RENAME OBJECT FactInternetSales_new TO FactInternetSales;

DROP TABLE FactInternetSales_old;
```

> [AZURE.NOTE] Azure SQL-Data Warehouse unterstützt noch nicht automatisch Support erstellen oder auto-Statistiken aktualisieren.  Um die optimale Leistung bei Ihrer Abfragen zu erhalten, es ist wichtig, Statistiken für alle Spalten aller Tabellen, nach dem ersten Laden erstellt werden oder wesentlichen Änderungen in den Daten auftreten.  Eine ausführliche Erläuterung von Statistiken finden Sie unter der [Statistik][] in der Gruppe Entwicklung von Themen.

## <a name="using-ctas-to-work-around-unsupported-features"></a>Verwenden von CTAS zur Umgehung nicht unterstützte features

`CTAS`kann auch zur Umgehung eine Reihe von der unten aufgeführten nicht unterstützten Funktionen verwendet werden. Dies kann häufig als eine Win/Win-Situation werden nicht nur wird Code kompatibel sein, doch es werden häufig schneller ausgeführt auf SQL Data Warehouse. Dies ist als Ergebnis dessen vollständig parallelisierte Entwurf. Szenarien, die mit CTAS umgangen werden können, gehören:

- WÄHLEN SIE AUS. IN
- ANSI-VERKNÜPFUNGEN Updates
- ANSI-Verknüpfungen auf Löschen
- ZUSAMMENFÜHREN-Anweisung

> [AZURE.NOTE] Denken Sie daran "CTAS ersten". Wenn Sie denken, Sie können lösen eines Problems mithilfe von `CTAS` klicken Sie dann das ist im Allgemeinen am besten für die - Vorgehensweise, auch wenn Sie weitere Daten als Ergebnis schreiben.
>

## <a name="selectinto"></a>WÄHLEN SIE AUS. IN
Bietet `SELECT..INTO` wird in einer Reihe von Speicherorten in Ihre Lösung.

Im folgenden Beispiel wird eine `SELECT..INTO` Anweisung:

```sql
SELECT *
INTO    #tmp_fct
FROM    [dbo].[FactInternetSales]
```

Konvertieren der oben, um `CTAS` ist ganz einfach weiterleiten:

```sql
CREATE TABLE #tmp_fct
WITH
(
    DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
;
```

> [AZURE.NOTE] CTAS erfordert zurzeit an, dass eine Spalte Verteilung angegeben werden muss.  Wenn Sie absichtlich nicht versuchen so ändern Sie die Spalte Verteilung Ihrer `CTAS` wird der schnellsten ausgeführt, wenn Sie eine Spalte Verteilung, die der zugrunde liegenden Tabelle identisch ist auswählen, wie diese Strategie Verschieben von Daten vermieden werden.  Wenn Sie eine kleine Tabelle erstellen, Leistung ist kein Faktor, und Sie können angeben, werden `ROUND_ROBIN` zu verhindern, dass auf eine Spalte Verteilung entscheiden.

## <a name="ansi-join-replacement-for-update-statements"></a>ANSI-Verknüpfung Ersatz für Update-Anweisung

Möglicherweise erachten Sie, dass Sie eine komplexe Update haben, die mehr als zwei Tabellen mit ANSI-Syntax Beitritt zum Ausführen der aktualisieren oder löschen verknüpft.

Stellen Sie sich vor, dass Ihnen dieser Tabelle aktualisieren:

```sql
CREATE TABLE [dbo].[AnnualCategorySales]
(   [EnglishProductCategoryName]    NVARCHAR(50)    NOT NULL
,   [CalendarYear]                  SMALLINT        NOT NULL
,   [TotalSalesAmount]              MONEY           NOT NULL
)
WITH
(
    DISTRIBUTION = ROUND_ROBIN
)
;
```

Die ursprüngliche Abfrage möglicherweise ungefähr wie folgt nun wissen, wie:

```sql
UPDATE  acs
SET     [TotalSalesAmount] = [fis].[TotalSalesAmount]
FROM    [dbo].[AnnualCategorySales]     AS acs
JOIN    (
        SELECT  [EnglishProductCategoryName]
        ,       [CalendarYear]
        ,       SUM([SalesAmount])              AS [TotalSalesAmount]
        FROM    [dbo].[FactInternetSales]       AS s
        JOIN    [dbo].[DimDate]                 AS d    ON s.[OrderDateKey]             = d.[DateKey]
        JOIN    [dbo].[DimProduct]              AS p    ON s.[ProductKey]               = p.[ProductKey]
        JOIN    [dbo].[DimProductSubCategory]   AS u    ON p.[ProductSubcategoryKey]    = u.[ProductSubcategoryKey]
        JOIN    [dbo].[DimProductCategory]      AS c    ON u.[ProductCategoryKey]       = c.[ProductCategoryKey]
        WHERE   [CalendarYear] = 2004
        GROUP BY
                [EnglishProductCategoryName]
        ,       [CalendarYear]
        ) AS fis
ON  [acs].[EnglishProductCategoryName]  = [fis].[EnglishProductCategoryName]
AND [acs].[CalendarYear]                = [fis].[CalendarYear]
;
```

Da SQL Data Warehouse nicht unterstützt ANSI verknüpft, der `FROM` -Klausel ein `UPDATE` -Anweisung Sie können keine kopieren Sie diesen Code über ohne etwas ändern.

Sie können eine Kombination aus einer `CTAS` und eine implizite Verknüpfung diesen Code ersetzen:

```sql
-- Create an interim table
CREATE TABLE CTAS_acs
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT  ISNULL(CAST([EnglishProductCategoryName] AS NVARCHAR(50)),0)    AS [EnglishProductCategoryName]
,       ISNULL(CAST([CalendarYear] AS SMALLINT),0)                      AS [CalendarYear]
,       ISNULL(CAST(SUM([SalesAmount]) AS MONEY),0)                     AS [TotalSalesAmount]
FROM    [dbo].[FactInternetSales]       AS s
JOIN    [dbo].[DimDate]                 AS d    ON s.[OrderDateKey]             = d.[DateKey]
JOIN    [dbo].[DimProduct]              AS p    ON s.[ProductKey]               = p.[ProductKey]
JOIN    [dbo].[DimProductSubCategory]   AS u    ON p.[ProductSubcategoryKey]    = u.[ProductSubcategoryKey]
JOIN    [dbo].[DimProductCategory]      AS c    ON u.[ProductCategoryKey]       = c.[ProductCategoryKey]
WHERE   [CalendarYear] = 2004
GROUP BY
        [EnglishProductCategoryName]
,       [CalendarYear]
;

-- Use an implicit join to perform the update
UPDATE  AnnualCategorySales
SET     AnnualCategorySales.TotalSalesAmount = CTAS_ACS.TotalSalesAmount
FROM    CTAS_acs
WHERE   CTAS_acs.[EnglishProductCategoryName] = AnnualCategorySales.[EnglishProductCategoryName]
AND     CTAS_acs.[CalendarYear]               = AnnualCategorySales.[CalendarYear]
;

--Drop the interim table
DROP TABLE CTAS_acs
;
```

## <a name="ansi-join-replacement-for-delete-statements"></a>ANSI-Verknüpfung Ersatz für delete-Anweisungen
Manchmal ist die beste Methode zum Löschen von Daten ist die Verwendung `CTAS`. Markieren Sie die Daten, die Sie behalten möchten, anstatt die Daten einfach löschen. Diese vor allem, wenn `DELETE` Anweisungen, mit denen Ansi Syntax verknüpfen, da SQL Data Warehouse ANSI nicht unterstützt, in verknüpft der `FROM` -Klausel einer `DELETE` Anweisung.

Ein Beispiel für eine konvertierte DELETE-Anweisung steht unter zur Verfügung:

```sql
CREATE TABLE dbo.DimProduct_upsert
WITH
(   Distribution=HASH(ProductKey)
,   CLUSTERED INDEX (ProductKey)
)
AS -- Select Data you wish to keep
SELECT     p.ProductKey
,          p.EnglishProductName
,          p.Color
FROM       dbo.DimProduct p
RIGHT JOIN dbo.stg_DimProduct s
ON         p.ProductKey = s.ProductKey
;

RENAME OBJECT dbo.DimProduct        TO DimProduct_old;
RENAME OBJECT dbo.DimProduct_upsert TO DimProduct;
```

## <a name="replace-merge-statements"></a>Ersetzen Sie die Anweisungen zusammenführen
Seriendruck-Anweisungen ersetzt werden, mindestens Webpart mithilfe von `CTAS`. Sie können Konsolidieren der `INSERT` und die `UPDATE` in einer einzigen Anweisung. Alle gelöschten Datensätze in einer zweiten Anweisung deaktivieren geschlossen werden müssten.

Ein Beispiel für eine `UPSERT` finden Sie unter:

```sql
CREATE TABLE dbo.[DimProduct_upsert]
WITH
(   DISTRIBUTION = HASH([ProductKey])
,   CLUSTERED INDEX ([ProductKey])
)
AS
-- New rows and new versions of rows
SELECT      s.[ProductKey]
,           s.[EnglishProductName]
,           s.[Color]
FROM      dbo.[stg_DimProduct] AS s
UNION ALL  
-- Keep rows that are not being touched
SELECT      p.[ProductKey]
,           p.[EnglishProductName]
,           p.[Color]
FROM      dbo.[DimProduct] AS p
WHERE NOT EXISTS
(   SELECT  *
    FROM    [dbo].[stg_DimProduct] s
    WHERE   s.[ProductKey] = p.[ProductKey]
)
;

RENAME OBJECT dbo.[DimProduct]          TO [DimProduct_old];
RENAME OBJECT dbo.[DimpProduct_upsert]  TO [DimProduct];

```

## <a name="ctas-recommendation-explicitly-state-data-type-and-nullability-of-output"></a>CTAS Empfehlungen: ausdrücklich Datentyp und ob auf NULL festgelegt Ausgabe

Bei der Migration von Code finden Sie möglicherweise, dass Sie über diese Art von Codierung Muster ausführen:

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455

CREATE TABLE result
(result DECIMAL(7,2) NOT NULL
)
WITH (DISTRIBUTION = ROUND_ROBIN)

INSERT INTO result
SELECT @d*@f
;
```

Instinktiv scheint sollten Sie diesen Code zu einer CTAS migrieren und wäre korrekt. Es ist jedoch eine ausgeblendete Problem hier.

Im folgende Code ergibt sich nicht auf das gleiche Ergebnis aus:

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455
;

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT @d*@f as result
;
```

Beachten Sie, dass die Spalte "Ergebnis" Weiterleiten der Datenwerte Typ und NULL-Zulässigkeit des Ausdrucks führt. Dies kann zu raffinierten Varianzen Werte führen, wenn Sie nicht vorsichtig sind.

Gehen Sie wie folgt als Beispiel für ein:

```sql
SELECT result,result*@d
from result
;

SELECT result,result*@d
from ctas_r
;
```

Der Ergebnis gespeicherte Wert unterscheidet. Während der gespeicherte Wert in der Ergebnisspalte in anderen Ausdrücken verwendet wird wird der Fehler noch mehr signifikante.

![][1]

Dies ist besonders wichtig für die Datenmigration. Obwohl die zweite Abfrage wohl genauere ist ein Problem vorliegt. Die Daten wäre anderen im Vergleich zu Quell- und führt zu Fragen der Integrität bei der Migration. Dies ist ein solcher seltene Fall, in dem die "falsche" Antwort tatsächlich am rechten geeignet ist!

Wir diese Unterschiede zwischen den zwei Ergebnissen finden Sie unter deswegen auf implizite Typumwandlung aus. Im ersten Beispiel wird die Tabelle die Spaltendefinition definiert. Tritt auf, eine implizite Typumwandlung, wenn die Zeile eingefügt wird. Im zweiten Beispiel ist keine implizite Typumwandlung als der Ausdruck der Spalte Datentyp definiert. Beachten Sie auch, dass die Spalte im zweiten Beispiel als eine Spalte NULL-Werte zulässt, während im ersten Beispiel es nicht verfügt definiert wurde. Wenn die Tabelle erstellt wurde, in dem ersten Beispiel Spalte ob auf NULL festgelegt wurde explizit definiert. In der zweiten ergibt NULL Definition Beispiel, die sie einfach auf den Ausdruck standardmäßig diese Links und wurde.  

Zum Beheben dieser Probleme müssen Sie explizit die Typumwandlung und NULL-Zulässigkeit in Festlegen der `SELECT` Teil der `CTAS` Anweisung. Sie können nicht im Webpart Tabelle erstellen diese Eigenschaften festlegen.

Im folgenden Beispiel wird veranschaulicht, wie den Code zu beheben:

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT ISNULL(CAST(@d*@f AS DECIMAL(7,2)),0) as result
```

Beachten Sie Folgendes:
- CAST oder konvertieren konnte verwendet wurden
- ISNULL wird verwendet, um die NULL-Zulässigkeit nicht ZUSAMMENGESETZTEN erzwingen
- ISNULL ist der äußere Sortierfelder
- Des zweiten Teils der ISNULL ist eine Konstante h. 0

> [AZURE.NOTE] Damit ordnungsgemäß festzulegenden ob NULL festgelegt ist entscheidend, mit `ISNULL` und nicht `COALESCE`. `COALESCE`ist keine Funktion deterministische und daher das Ergebnis des Ausdrucks ist immer NULL sein kann. `ISNULL`unterscheidet. Es ist deterministisch. Daher beim des zweiten Teils der `ISNULL` Funktion ist eine Konstante oder ein Literal und dann der Ergebniswert werden nicht NULL.

Dieser Tipp ist nicht nur für die Integrität der Ihrer Berechnungen hilfreich. Es ist auch wichtig für die Tabelle Partition wechseln. Stellen Sie sich vor, dass Sie diese Tabelle als Ihre Fact definiert haben:

```sql
CREATE TABLE [dbo].[Sales]
(
    [date]      INT     NOT NULL
,   [product]   INT     NOT NULL
,   [store]     INT     NOT NULL
,   [quantity]  INT     NOT NULL
,   [price]     MONEY   NOT NULL
,   [amount]    MONEY   NOT NULL
)
WITH
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101,20020101
                    ,20030101,20040101,20050101
                    )
                )
)
;
```

Das Wertfeld ist jedoch ein berechneter Ausdruck es nicht Teil der Quelldaten ist.

So erstellen Sie Ihre partitionierte Dataset Aktion werden soll:

```sql
CREATE TABLE [dbo].[Sales_in]
WITH    
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101
                    )
                )
)
AS
SELECT
    [date]    
,   [product]
,   [store]
,   [quantity]
,   [price]   
,   [quantity]*[price]  AS [amount]
FROM [stg].[source]
OPTION (LABEL = 'CTAS : Partition IN table : Create')
;
```

Die Abfrage würde hervorragenden ausgeführt. Das Problem angezeigt wird, wenn Sie versuchen, den Schalter Partition durchzuführen. Die Tabellendefinitionen stimmen nicht überein. Damit wird die Tabellendefinitionen entsprechen den CTAS soll geändert werden.

```sql
CREATE TABLE [dbo].[Sales_in]
WITH    
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101
                    )
                )
)
AS
SELECT
    [date]    
,   [product]
,   [store]
,   [quantity]
,   [price]   
,   ISNULL(CAST([quantity]*[price] AS MONEY),0) AS [amount]
FROM [stg].[source]
OPTION (LABEL = 'CTAS : Partition IN table : Create');
```

Sie können daher sehen, dass Typ Konsistenz und Verwalten von NULL-Zulässigkeit-Eigenschaften auf einer CTAS ist eine gute engineering bewährte Methode. Trägt dazu bei, die um in Ihrer Berechnung Integrität zu erhalten, und auch ist sichergestellt, dass Partition umsteigen möglich ist.

Weitere Informationen zur Verwendung von [CTAS][]finden Sie unter MSDN. Es ist eine der wichtigsten Aussagen in Azure SQL-Data Warehouse. Stellen Sie sicher, dass Sie sorgfältig kennen.

## <a name="next-steps"></a>Nächste Schritte
Weitere Hinweise zur Entwicklung finden Sie unter [Übersicht über die Entwicklung][].

<!--Image references-->
[1]: media/sql-data-warehouse-develop-ctas/ctas-results.png

<!--Article references-->
[Übersicht über die Entwicklung]: sql-data-warehouse-overview-develop.md
[Statistik]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->
[CTAS]: https://msdn.microsoft.com/library/mt204041.aspx

<!--Other Web references-->

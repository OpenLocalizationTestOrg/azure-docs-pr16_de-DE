<properties
   pageTitle="Gruppe von Optionen in SQL Data Warehouse | Microsoft Azure"
   description="Tipps für die Implementierung von Gruppe von Optionen in Azure SQL-Data Warehouse für die Entwicklung von Lösungen."
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

# <a name="group-by-options-in-sql-data-warehouse"></a>Gruppieren Sie nach Optionen in SQL Data Warehouse

Die [GROUP BY][] -Klausel wird zum Aggregieren von Daten auf einer Zusammenfassung Satz von Zeilen verwendet. Sie hat auch einige Optionen, mit die erweitert es-Funktionen, die umgangen werden, wenn sie nicht direkt von Azure SQL-Data Warehouse unterstützt werden müssen.

Diese Optionen sind
- GROUP BY mit ROLLUP
- GRUPPIEREN VON MENGEN
- GROUP BY mit CUBE

## <a name="rollup-and-grouping-sets-options"></a>Rollup und Gruppierung festgelegt Optionen.
Hier die einfachste Möglichkeit ist dann, verwenden `UNION ALL` stattdessen das Rollup ausführen anstatt die explizite Syntax. Das Ergebnis ist gleich

Nachstehend ist ein Beispiel für eine Gruppe Anweisung mithilfe der `ROLLUP` Option:

```sql
SELECT [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
,      SUM(SalesAmount)             AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t       ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY ROLLUP (
                        [SalesTerritoryCountry]
                ,       [SalesTerritoryRegion]
                )
;
```

Mithilfe von ROLLUP haben wir die folgenden Aggregationen angefordert:
- Land und Region
- Land
- Gesamtergebnis

Ersetzen Sie verwenden müssen `UNION ALL`; Angeben der Aggregationen explizit erforderlich, die gleichen Ergebnisse zurückgegeben:

```sql
SELECT [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY
       [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
UNION ALL
SELECT [SalesTerritoryCountry]
,      NULL
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY
       [SalesTerritoryCountry]
UNION ALL
SELECT NULL
,      NULL
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey;
```

Für Gruppierung alle wir legt müssen übernehmen Sie der Hauptbenutzer aber nur erstellen UNION aller Abschnitte für Aggregationsebenen, die wir finden Sie unter möchten

## <a name="cube-options"></a>Cube-Optionen
Es ist möglich, eine GROUP BY mit CUBE mithilfe der UNION ALL und systematisch zu erstellen. Das Problem ist, dass der Code schnell unübersichtlich und schwerfällig werden kann. Um dies zu verringern, können Sie diese Weitere erweiterte Ansatz verwenden.

Verwenden Sie uns im oben genannten Beispiel.

Der erste Schritt besteht definieren 'Cube', der alle Ebenen der Aggregation definiert werden, die wir erstellen möchten. Es ist wichtig, eine Notiz von CROSS JOIN der beiden abgeleiteten Tabellen. Dadurch werden alle Ebenen für uns generiert. Im weiteren Verlauf des Codes bestehen wirklich zur Formatierung.

```sql
CREATE TABLE #Cube
WITH
(   DISTRIBUTION = ROUND_ROBIN
,   LOCATION = USER_DB
)
AS
WITH GrpCube AS
(SELECT    CAST(ISNULL(Country,'NULL')+','+ISNULL(Region,'NULL') AS NVARCHAR(50)) as 'Cols'
,          CAST(ISNULL(Country+',','')+ISNULL(Region,'') AS NVARCHAR(50))  as 'GroupBy'
,          ROW_NUMBER() OVER (ORDER BY Country) as 'Seq'
FROM       ( SELECT 'SalesTerritoryCountry' as Country
             UNION ALL
             SELECT NULL
           ) c
CROSS JOIN ( SELECT 'SalesTerritoryRegion' as Region
             UNION ALL
             SELECT NULL
           ) r
)
SELECT Cols
,      CASE WHEN SUBSTRING(GroupBy,LEN(GroupBy),1) = ','
            THEN SUBSTRING(GroupBy,1,LEN(GroupBy)-1)
            ELSE GroupBy
       END AS GroupBy  --Remove Trailing Comma
,Seq
FROM GrpCube;
```

Die Ergebnisse der CTAS sehen Sie nachfolgend:

![][1]

Im zweite Schritt wird eine Zieltabelle zum Speichern der Zwischenergebnisse angeben:

```sql
DECLARE
 @SQL NVARCHAR(4000)
,@Columns NVARCHAR(4000)
,@GroupBy NVARCHAR(4000)
,@i INT = 1
,@nbr INT = 0
;
CREATE TABLE #Results
(
 [SalesTerritoryCountry] NVARCHAR(50)
,[SalesTerritoryRegion]  NVARCHAR(50)
,[TotalSalesAmount]      MONEY
)
WITH
(   DISTRIBUTION = ROUND_ROBIN
,   LOCATION = USER_DB
)
;
```

Der dritte Schritt ist eine Schleife über unsere Cube von Spalten, die Durchführung der Aggregation durchlaufen. Die Abfrage wird einmal für jede Zeile in der temporären Tabelle #Cube ausgeführt und die Ergebnisse in der #Results temporären Tabelle speichern

```sql
SET @nbr =(SELECT MAX(Seq) FROM #Cube);

WHILE @i<=@nbr
BEGIN
    SET @Columns = (SELECT Cols    FROM #Cube where seq = @i);
    SET @GroupBy = (SELECT GroupBy FROM #Cube where seq = @i);

    SET @SQL ='INSERT INTO #Results
              SELECT '+@Columns+'
              ,      SUM(SalesAmount) AS TotalSalesAmount
              FROM  dbo.factInternetSales s
              JOIN  dbo.DimSalesTerritory t  
              ON s.SalesTerritoryKey = t.SalesTerritoryKey
              '+CASE WHEN @GroupBy <>''
                     THEN 'GROUP BY '+@GroupBy ELSE '' END

    EXEC sp_executesql @SQL;
    SET @i +=1;
END
```

Wir können und schließlich die Ergebnisse zurückgeben, indem Sie einfach aus der temporären Tabelle #Results lesen

```sql
SELECT *
FROM #Results
ORDER BY 1,2,3
;
```

Indem Sie den Code in Abschnitte aufteilen und Generieren einer Schleifenkonstrukt wird der Code überschaubarer und verwaltet werden.


## <a name="next-steps"></a>Nächste Schritte
Weitere Hinweise zur Entwicklung finden Sie unter [Übersicht über die Entwicklung][].

<!--Image references-->
[1]: media/sql-data-warehouse-develop-group-by-options/sql-data-warehouse-develop-group-by-cube.png

<!--Article references-->
[Übersicht über die Entwicklung]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[GRUPPIEREN NACH]: https://msdn.microsoft.com/library/ms177673.aspx


<!--Other Web references-->

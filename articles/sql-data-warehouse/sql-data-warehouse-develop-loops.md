<properties
   pageTitle="Schleifen in SQL Datawarehouse | Microsoft Azure"
   description="Tipps für Transact-SQL-Schleifen und ersetzt Cursor in Azure SQL-Data Warehouse für die Entwicklung von Lösungen."
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

# <a name="loops-in-sql-data-warehouse"></a>Schleifen in SQL Datawarehouse
SQL Data Warehouse unterstützt die Schleife [während][] für das Ausführen von Bausteinen Anweisung wiederholt. Dies geschieht so lange für solange die angegebenen Bedingungen erfüllt sind oder bis der Code speziell mithilfe der Schleife beendet wird die `BREAK` Schlüsselwort. Schleifen eignen sich besonders zum Ersetzen in SQL-Code definierten Cursor. Glücklicherweise fast alle Cursor, die in SQL-Code geschrieben werden von den Wechsel sind nur Vielzahl zu lesen. [Während] Schleifen daher sind eine großartige Alternative aus, wenn Sie sich selbst eine ersetzen müssen finden.

## <a name="leveraging-loops-and-replacing-cursors-in-sql-data-warehouse"></a>Nutzung von Schleifen und der Cursor im SQL Data Warehouse ersetzen
Jedoch vor dem ersten Kopf Einstieg Sie sollten sich folgende Frage berücksichtigen: "Konnte dieser Cursor neu geschrieben werden Mengen basierten Vorgänge verwenden?". In vielen Fällen wird die Antwort Ja werden und ist oft die beste Vorgehensweise. Basis Vorgänge häufig festlegt erheblich schneller als eine iterative, zeilenweise Ansatz.

Schnelles weiterleiten schreibgeschützte Cursor können leicht mit einem Schleifenkonstrukt ersetzt werden. Es folgt ein einfaches Beispiel. In diesem Codebeispiel aktualisiert die Statistik für jede Tabelle in der Datenbank. Indem Sie die Tabellen in der Schleife durchlaufen können wir zu jedem Befehl nacheinander ausführen.

Erstellen Sie zuerst eine temporäre Tabelle, die eine eindeutige Zeilennummer verwendet, um einzelnen Anweisungen identifizieren enthält:

```
CREATE TABLE #tbl
WITH
( DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT  ROW_NUMBER() OVER(ORDER BY (SELECT NULL)) AS Sequence
,       [name]
,       'UPDATE STATISTICS '+QUOTENAME([name]) AS sql_code
FROM    sys.tables
;
```

Zweites, Initialisierung der Variablen im erforderlich, um die Schleife ausführen:

```
DECLARE @nbr_statements INT = (SELECT COUNT(*) FROM #tbl)
,       @i INT = 1
;
```

Durchlaufen Sie jetzt eine aufgeführte Anweisungen nacheinander:

```
WHILE   @i <= @nbr_statements
BEGIN
    DECLARE @sql_code NVARCHAR(4000) = (SELECT sql_code FROM #tbl WHERE Sequence = @i);
    EXEC    sp_executesql @sql_code;
    SET     @i +=1;
END
```

Legen Sie schließlich die temporäre Tabelle im ersten Schritt erstellte

```
DROP TABLE #tbl;
```


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## <a name="next-steps"></a>Nächste Schritte
Weitere Hinweise zur Entwicklung finden Sie unter [Übersicht über die Entwicklung][].

<!--Image references-->

<!--Article references-->
[Übersicht über die Entwicklung]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[WEILE]: https://msdn.microsoft.com/library/ms178642.aspx


<!--Other Web references-->

<properties
   pageTitle="Verteilen von Tabellen in SQL Data Warehouse | Microsoft Azure"
   description="Erste Schritte mit Tabellen in Azure SQL-Data Warehouse verteilen."
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
   ms.date="08/30/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="distributing-tables-in-sql-data-warehouse"></a>Verteilen von Tabellen in SQL Data Warehouse

> [AZURE.SELECTOR]
- [(Übersicht)][]
- [Datentypen][]
- [Verteilen][]
- [Index][]
- [Partition][]
- [Statistik][]
- [Temporäre][]

SQL Data Warehouse ist, dass eine parallele Verarbeitung (MPP) Datenbanksystem verteilt.  Durch die Division von Daten und Verarbeitung von Videofunktionen über mehrere Knoten, kann SQL Data Warehouse riesigen Skalierbarkeit - weit über eine beliebige einzelne System anbieten.  Entscheiden, wie Sie Ihre Daten in der SQL-Data Warehouse verteilen ist einer der wichtigsten Faktoren, um optimale Leistung zu erreichen.   Der Schlüssel für optimale Leistung ist das Verschieben von Daten minimieren und die EINGABETASTE, um das Verschieben von Daten zu minimieren entspricht dem wiederum die richtigen Verteilung Strategie auswählen.

## <a name="understanding-data-movement"></a>Grundlegendes zum Verschieben von Daten

Ein MPP-System werden die Daten aus jeder Tabelle über mehrere zugrunde liegenden Datenbanken verteilt.  Die besten optimierten Abfragen auf einem MPP-System können einfach durch weitergegeben werden, für die einzelnen verteilten Datenbanken ohne Interaktion zwischen den anderen Datenbanken nicht ausführen.  Beispielsweise angenommen, dass Sie eine Datenbank mit Verkaufsdaten haben die beiden Tabellen Sales und Kunden enthält.  Wenn Sie eine Abfrage, die Ihre Verkaufstabelle Ihrer Tabelle Kunden teilnehmen muss und Sie unterteilen Ihre Umsätze und den Kunden Tabellen nach oben nach Kundennummer, mit der einzelnen Kunden in einer separaten Datenbank, können alle Abfragen die Teilnahme an Sales und Kunden innerhalb jeder Datenbank ohne Kenntnisse der anderen Datenbanken gelöst werden.  Im Gegensatz dazu, wenn Sie Ihre Verkaufsdaten durch Auftragsnummer und Ihrer Kundendaten nach Kundennummer geteilt haben, klicken Sie dann alle angegebene Datenbank hat keinen die entsprechenden Daten für jeden Kunden und somit Wenn Sie Ihre Verkaufsdaten auf Kundendaten teilnehmen wollten, müssten Sie die Daten für jeden Kunden aus anderen Datenbanken zu erhalten.  In diesem Beispiel zweiten müssten Verschieben von Daten auftreten, damit um die Kundendaten in die Umsatzdaten, zu verschieben, damit die beiden Tabellen verknüpft werden können.  

Verschieben von Daten ist nicht immer schlecht, manchmal ist es erforderlich, eine Abfrage zu lösen.  Aber dieser zusätzliche Schritt vermieden werden kann, die Ihre Abfrage wird ausgeführt schneller.  Verschieben von Daten entsteht am häufigsten auf, wenn Tabellen verknüpft sind oder Aggregationen ausgeführt werden.  Häufig Sie beides tun müssen, damit die Taste gedrückt, während Sie Sie für ein Szenario, wie eine Verknüpfung, optimieren könnten Sie weiterhin Verschieben von Daten helfen Ihnen, für das andere Szenario, wie eine Aggregation zu lösen benötigen.  Der Kniff ist herauszufinden, welche weniger Arbeit ist.  In den meisten Fällen ist Verteilen einer häufig verknüpften Spalte großen Faktentabellen der am effektivsten Methode zum Verkleinern der meisten Verschieben von Daten aus.  Verteilen von Daten in Verknüpfungsspalten ist eine sehr viel häufiger Methode zum Verschieben von Daten als Verteilen von Daten in Spalten verbindet eine Aggregation zu verringern.

## <a name="select-distribution-method"></a>Wählen Sie die Verteilungsmethode

Hintergrundinformationen teilt SQL Data Warehouse von Daten in 60 Datenbanken an.  Jeder einzelnen Datenbank wird als **Verteilerliste**bezeichnet.  Beim Laden von Daten in die jeweilige Tabelle enthält SQL Data Warehouse wissen, wie Sie Ihre Daten über diese 60 Verteilung dividieren.  

Die Verteilungsmethode Ebene der Tabelle definiert ist, und derzeit sind zwei Optionen:

1. **Round Robert** die Daten gleichmäßig aber zufällig verteilen.
2. Die Daten auf Grundlage der Werte aus einer einzelnen Spalte hashing verteilt **Hash verteilt.**

Standardmäßig Wenn Sie keine Methode Daten Verteilung definieren wird die Tabelle verteilt mithilfe der **Funktion RUNDEN Robert** Verteilungsmethode.  Wie Sie in der Implementierung komplexer sein, sollten Sie erwägen **Hash verteilt** Tabellen zum Verschieben von Daten zu minimieren, die wiederum abfrageleistung optimiert werden kann.

### <a name="round-robin-tables"></a>Round Robert Tabellen

Verwenden der Funktion RUNDEN Robert Methode zum Verteilen von Daten ist sehr ähnlich wie klingt.  Die Daten geladen werden, wird jede Zeile der nächsten Verteilung einfach gesendet.  Diese Methode zum Verteilen von Daten werden die Daten immer zufällig sehr gleichmäßig auf alle die Verteilung verteilen.  D. h., es ist keine Sortierung erfolgen während der Funktion RUNDEN Robert Prozess stellen Ihre Daten.  Eine Funktion RUNDEN Robert Verteilung wird einen zufälligen Hash aus diesem Grund bezeichnet.  Mit einer Funktion RUNDEN-Robert verteilten Tabelle besteht keine Notwendigkeit zu verstehen, die Daten.  Aus diesem Grund stellen Round-Robert Tabellen häufig Ziele gute laden.

Standardmäßig ist keine Verteilungsmethode nicht ausgewählt ist, wird die Funktion RUNDEN Robert Verteilungsmethode verwendet werden.  Während der Funktion RUNDEN Robert Tabellen einfach zu verwenden, sind, weil Daten zufällig im System verteilt ist, bedeutet dies, dass die Verteilung nicht sichergestellt ist nicht möglich, ist jede Zeile jedoch auf.  Daher muss das System manchmal Aufrufen eines Verwaltungsvorgang Daten, um die Daten besser organisieren, bevor sie eine Abfrage auflösen kann.  Dieser zusätzliche Schritt kann Ihre Abfragen verlangsamen.

Erwägen Sie mithilfe der Funktion RUNDEN Robert Verteilung für die Tabelle in den folgenden Szenarien:

- Wenn Sie als Ausgangspunkt einfache die ersten Schritte
- Es ist kein offensichtlich verknüpfen Schlüssel
- Wenn ein guter Kandidat Spalte für Hash verteilen die Tabelle nicht vorhanden ist
- Wenn die Tabelle einen gemeinsamer Schlüssel für die Verknüpfung nicht mit anderen Tabellen gemeinsam
- Ist die Verknüpfung geringer ist als andere Verknüpfungen in der Abfrage
- Wenn die Tabelle eine temporäre staging Tabelle ist

In beiden Beispielen werden eine Tabelle Round Robert zu erstellen:

```SQL
-- Round Robin created by default
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
;

-- Explicitly Created Round Robin Table
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
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
,   DISTRIBUTION = ROUND_ROBIN
)
;
```

> [AZURE.NOTE] Während der Funktion RUNDEN Robert ist, dass der Tabelle Standardtyp wird in Ihrem DDL explizite bewährte Methode betrachtet wird, dass die Absichten Ihrer Tabellenlayout löschen an andere Personen sind.

### <a name="hash-distributed-tables"></a>Hashing verteilte Tabellen

Mit einem **distributed Hash** -Algorithmus zum Verteilen von Tabellen kann für viele Szenarien Leistung durch Verschieben von Daten zum Zeitpunkt der Abfrage verringern.  Verteilt Hashtabellen sind Tabellen, die geteilt werden zwischen den verteilten Datenbanken mit einem hashing Algorithmus für eine einzelne Spalte, die ausgewählt werden.  Die Spalte Verteilung ist, was bestimmt, wie die Daten über Ihre verteilten Datenbanken verteilt ist.  Die Hashfunktion verwendet die Spalte Verteilung Zeilen Verteilung zuweisen.  Das hashing-Algorithmus und die resultierende Verteilung ist deterministisch.  Dies ist die gleiche Wert mit den gleichen Datentyp hat immer die gleiche Verteilung.    

In diesem Beispiel wird eine Tabelle auf Id verteilt zu erstellen:

```SQL
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
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
,  DISTRIBUTION = HASH([ProductKey])
)
;
```

## <a name="select-distribution-column"></a>Wählen Sie die Spalte Verteilung

Wenn Sie die **Hash verteilen** eine Tabelle verwenden, müssen Sie eine einzelne Verteilung Spalte auswählen.  Wenn Sie eine Spalte Verteilung auswählen, gibt es drei wichtige Faktoren zu berücksichtigen.  

Wählen Sie eine einzelne Spalte, wird ein:

1. Nicht aktualisiert werden
2. Gleichmäßiges Verteilen von Daten, zur Vermeidung von Daten Schiefe
3. Verschieben von Daten zu minimieren

### <a name="select-distribution-column-which-will-not-be-updated"></a>Wählen Sie die Verteilung Spalte, die nicht aktualisiert wird

Verteilung Spalten nicht aktualisiert werden, daher, wählen Sie eine Spalte mit statischen Werten.  Wenn Sie eine Spalte wird aktualisiert werden muss, ist es im Allgemeinen nicht Verteilung guter Kandidat.  Ist ein Fall, in dem Sie eine Spalte Verteilung aktualisieren müssen, kann dies ausgeführt werden, indem Sie zuerst die Zeile löschen und dann Einfügen einer neuen Zeile.

### <a name="select-distribution-column-which-will-distribute-data-evenly"></a>Wählen Sie die Spalte Verteilung die Daten gleichmäßig verteilen wird

Da verteilten Systems nur so schnell wie die langsamste Verteilung ausführt, ist es wichtig, um die Arbeit gleichmäßig über die Verteilung unterteilen und um angeglichene Ausführung gesamten System zu erzielen.  Die Möglichkeit, die die Arbeit in einem verteilten System verteilt ist basiert auf die Stelle, an der die Daten für jede Verteilung verfügbar ist.  Dadurch ist es wichtig, wählen Sie die Spalte rechts Verteilung für die Daten verteilen, damit jede Verteilung gleich Arbeit wurde und dauert gleichzeitig seine Teil der Arbeit abgeschlossen.  Bei der Arbeit auch über das System verteilt ist, ist die Verteilung die Daten verteilt.  Wenn die Daten nicht gleichmäßig verteilt, rufen wir diese **Daten Neigung**.  

Um Daten gleichmäßig dividieren und Daten Schiefe vermeiden, berücksichtigen Sie bei die Verteilung Spalte auswählen:

1. Wählen Sie eine Spalte, die eine signifikante Anzahl eindeutiger Werte enthält.
2. Vermeiden Sie Verteilen von Daten in Spalten mit wenigen eindeutigen Werten. 
3. Vermeiden Sie Daten in den Spalten mit der Einstellung Hoch Nullen verteilen.
4. Vermeiden Sie die Daten auf Datumsspalten verteilen.

Da jeder Wert 1 von 60 Verteilung gehasht wird, um die gleichmäßige Verteilung zu erreichen möchten Sie eine Spalte auswählen, die hochgradig eindeutig ist und mehr als 60 eindeutige Werte enthalten.  Um zu veranschaulichen, erwägen Sie einen Fall, in dem eine Spalte nur 40 eindeutigen Werten aus.  Wenn diese Spalte als die zurück-Taste aktiviert wurde, würde die Daten für diese Tabelle höchstens auf 40 Verteilung landen verlassen 20 Verteilung mit keine Daten und keine Verarbeitung zu tun ist.  Die anderen 40 Verteilung müssten umgekehrt mehr Arbeit erledigen, wenn die Daten mehr als 60 Verteilung gleichmäßig zugewiesen wurde.  Dieses Szenario ist ein Beispiel für Schiefe Daten.

Jeder abfrageschritt wartet MPP-System, alle enthalten, um deren Anteil an dieser Arbeit abgeschlossen.  Wenn Sie eine Verteilung mehr Arbeit als die anderen befasst ist, klicken Sie dann die Ressource von den anderen Verteilung sind im Wesentlichen verschwendet nur der beschäftigt Verteilung warten.  Bei der Arbeit nicht gleichmäßig auf alle Verteilung verteilen ist, rufen wir diesen **Schiefe verarbeiten**.  Verarbeitung Schiefe bewirkt, dass Abfragen ausgeführt wird als kann Wenn die Arbeitsbelastung gleichmäßig auf die Verteilung verteilt werden.  Schiefe Daten führen zu Verarbeitung Schiefe.

Vermeiden Sie für eine Spalte hochgradig verteilen, wie die null-Werte in der gleichen Verteilung landen werden. Auf einer Datumsspalte verteilen kann ebenfalls Schiefe Verarbeitung verursachen, da alle Daten für ein angegebenes Datum der gleichen Verteilung landen werden. Wenn mehrere Benutzer Abfragen ausgeführt werden werden alle Filter auf das gleiche Datum, und klicken Sie dann nur 1 von der Verteilung von 60 gesamte Arbeit seit einem bestimmten Datum ausführen wird nur auf einer Verteilung. In diesem Szenario werden die Abfragen wahrscheinlich ausgeführt 60 Mal langsamer als wurden Wenn die Daten gleichmäßig über alle die Verteilung verteilt. 

Wenn keine guter Kandidat Spalten vorhanden sind, sollten Sie mithilfe der Funktion RUNDEN Robert wie die Verteilungsmethode.

### <a name="select-distribution-column-which-will-minimize-data-movement"></a>Wählen Sie die Spalte Verteilung der Verlagerung von Daten zu minimieren, wird

Verschieben von Daten, indem Sie die Spalte rechts Verteilung auswählen minimiert ist eine der wichtigsten Strategien zum Optimieren der Leistung des SQL Data Warehouse.  Verschieben von Daten entsteht am häufigsten auf, wenn Tabellen verknüpft sind oder Aggregationen ausgeführt werden.  Spalten in verwendete `JOIN`, `GROUP BY`, `DISTINCT`, `OVER` und `HAVING` alle für **gute stellen** Klauseln Hashing Verteilung Kandidaten. 

Andererseits, Spalten in der `WHERE` Klausel führen **nicht** Tabellenerstellungsabfrage für gute Hash Spalte Kandidaten, da sie die Teilnahme an der Abfrage, welche Verteilung schränken Neigung Verarbeitung verursacht.  Eine gute Beispiele für eine Spalte, die möglicherweise auf verteilen verlockend, aber oft kann dazu führen, dass diese Verarbeitung Schiefe ist eine Datumsspalte an.

Im Allgemeinen, wenn Sie zwei großen Faktentabellen häufig verbindet in einer Verknüpfung verfügen, lernen die meisten Leistung Sie durch die Verteilung der beiden Tabellen auf eine der Verknüpfungsspalten.  Wenn Sie eine Tabelle, die mit einem anderen großen Faktentabelle nie verknüpft ist verfügen, suchen Sie dann auf Spalten, die häufig in der `GROUP BY` Klausel.

Es gibt ein paar wichtige Kriterien erfüllt sein müssen, um das Verschieben von Daten während einer Verknüpfung zu vermeiden:

1. Die Tabellen verbindet, in die Verknüpfung muss Hash auf **eine** der Spalten in die Verknüpfung Teilnahme verteilt.
2. Die Datentypen der Verknüpfungsspalten müssen zwischen den beiden Tabellen übereinstimmen.
3. Die Spalten müssen mit einem Wert Gleichheitsoperator verknüpft werden.
4. Der Verknüpfungstyp ist möglicherweise keine `CROSS JOIN`.


## <a name="troubleshooting-data-skew"></a>Schiefe Daten zur Problembehandlung

Wenn Tabellendaten verteilt werden mithilfe der Methode der Hash Verteilung besteht die Möglichkeit, dass einige Verteilung werden damit unproportional mehr Daten als andere schief ist. Eine zu große Datenmenge Schiefe kann abfrageleistung auswirken, da das endgültige Ergebnis einer verteilten Abfrage, für die längsten Verteilung warten muss auf Fertig stellen. Je nach den Grad der die Neigung Daten müssen Sie nur noch adressieren.

### <a name="identifying-skew"></a>Identifizieren Schiefe

Eine einfache Möglichkeit, eine Tabelle zu identifizieren, wie geneigt wird mit `DBCC PDW_SHOWSPACEUSED`.  Dies ist eine sehr schnelle und einfache Möglichkeit, um die Anzahl der Tabellenzeilen anzuzeigen, die in jedem der 60 Verteilung der Datenbank gespeichert sind.  Denken Sie daran, dass die Zeilen in der Tabelle verteilt für die am häufigsten angeglichene Leistung, gleichmäßig über alle die Verteilung verteilt werden soll.

```sql
-- Find data skew for a distributed table
DBCC PDW_SHOWSPACEUSED('dbo.FactInternetSales');
```

Wenn Sie die Ansichten Azure SQL-Data Warehouse dynamische Management (DMV) Abfragen können Sie eine ausführlichere Analyse ausführen.  Erstellen Sie damit beginnen, die mit den SQL-Code aus der [Tabelle Übersicht][Übersicht] Artikel [dbo.vTableSizes][] anzeigen aus.  Nachdem die Ansicht erstellt wurde, führen Sie diese Abfrage, wenn Sie wissen möchten, welche Tabellen mehr als 10 % Daten Schiefe aus.

```sql
select *
from dbo.vTableSizes 
where two_part_name in 
    (
    select two_part_name
    from dbo.vTableSizes
    where row_count > 0
    group by two_part_name
    having min(row_count * 1.000)/max(row_count * 1.000) > .10
    )
order by two_part_name, row_count
;
```

### <a name="resolving-data-skew"></a>Beheben von Daten Schiefe

Nicht reicht alle Schiefe zu eine Korrektur rechtfertigt.  In einigen Fällen kann die Leistung von einer Tabelle in einigen Abfragen den Schaden Datenseite Schiefe übersteigen.  Um zu entscheiden, ob Sie Daten aufgelöst werden müssen sollten Schiefe in einer Tabelle, Sie möglichst viele Datenbestände und Abfragen in Ihrer Arbeitsbelastung verstehen.   Eine Methode, um den Einfluss der Schiefe eigenständig ist mit den Schritten in der [Abfrage für die Überwachung][] Artikel überwachen den Einfluss der Schiefe Leistung von Abfragen und speziell Auswirkung auf das Abfragen wie lange dauern, bis auf die einzelnen Verteilung abgeschlossen.

Verteilen von Daten ist eine Frage finden den richtigen Saldo zwischen Daten Schiefe minimieren und Verschieben von Daten zu minimieren. Diese können entgegengesetzte Ziele, und Sie möchten manchmal Daten Schiefe zu halten, um das Verschieben von Daten zu verringern. Beispielsweise wenn die Spalte Verteilung häufig der freigegebenen Spalte Verknüpfungen und Aggregationen ist, werden Sie Verschieben von Daten minimiert werden. Die Vorteile der minimalen Daten Bewegung möglicherweise den Einfluss der Daten, die Neigung Probleme übersteigen. 

Das übliche Verfahren zum Beheben von Daten Schiefe ist die Tabelle mit einer anderen Verteilerliste Spalte neu zu erstellen. Da es keine zum Ändern der Spalte Verteilung für eine vorhandene Tabelle, die Möglichkeit Möglichkeit, die Verteilung der Tabelle ändern sie es mit einem [[CTAS]] neu erstellen.  Hier sind zwei Beispiele zu Daten Schiefe beheben:

### <a name="example-1-re-create-the-table-with-a-new-distribution-column"></a>Beispiel 1: Neuerstellen der Tabelle mit einer Spalte der neuen Verteilung

In diesem Beispiel wird [CTAS] [], um eine Tabelle mit einer anderen Hash Verteilung Spalte neu zu erstellen. 

```sql
CREATE TABLE [dbo].[FactInternetSales_CustomerKey] 
WITH (  CLUSTERED COLUMNSTORE INDEX
     ,  DISTRIBUTION =  HASH([CustomerKey])
     ,  PARTITION       ( [OrderDateKey] RANGE RIGHT FOR VALUES (   20000101, 20010101, 20020101, 20030101
                                                                ,   20040101, 20050101, 20060101, 20070101
                                                                ,   20080101, 20090101, 20100101, 20110101
                                                                ,   20120101, 20130101, 20140101, 20150101
                                                                ,   20160101, 20170101, 20180101, 20190101
                                                                ,   20200101, 20210101, 20220101, 20230101
                                                                ,   20240101, 20250101, 20260101, 20270101
                                                                ,   20280101, 20290101
                                                                )
                        )
    )
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
OPTION  (LABEL  = 'CTAS : FactInternetSales_CustomerKey')
;

--Create statistics on new table
CREATE STATISTICS [ProductKey] ON [FactInternetSales_CustomerKey] ([ProductKey]);
CREATE STATISTICS [OrderDateKey] ON [FactInternetSales_CustomerKey] ([OrderDateKey]);
CREATE STATISTICS [CustomerKey] ON [FactInternetSales_CustomerKey] ([CustomerKey]);
CREATE STATISTICS [PromotionKey] ON [FactInternetSales_CustomerKey] ([PromotionKey]);
CREATE STATISTICS [SalesOrderNumber] ON [FactInternetSales_CustomerKey] ([SalesOrderNumber]);
CREATE STATISTICS [OrderQuantity] ON [FactInternetSales_CustomerKey] ([OrderQuantity]);
CREATE STATISTICS [UnitPrice] ON [FactInternetSales_CustomerKey] ([UnitPrice]);
CREATE STATISTICS [SalesAmount] ON [FactInternetSales_CustomerKey] ([SalesAmount]);

--Rename the tables
RENAME OBJECT [dbo].[FactInternetSales] TO [FactInternetSales_ProductKey];
RENAME OBJECT [dbo].[FactInternetSales_CustomerKey] TO [FactInternetSales];
```

### <a name="example-2-re-create-the-table-using-round-robin-distribution"></a>Beispiel 2: Neuerstellen der Tabelle mithilfe der Funktion RUNDEN Robert Verteilung

In diesem Beispiel wird [CTAS] [], um eine Tabelle mit der Funktion RUNDEN Robert anstelle einer Verteilung Hash neu zu erstellen. Diese Änderung wird auch Daten Verteilung Kosten höhere Daten Bewegung erzeugen. 

```sql
CREATE TABLE [dbo].[FactInternetSales_ROUND_ROBIN] 
WITH (  CLUSTERED COLUMNSTORE INDEX
     ,  DISTRIBUTION =  ROUND_ROBIN
     ,  PARTITION       ( [OrderDateKey] RANGE RIGHT FOR VALUES (   20000101, 20010101, 20020101, 20030101
                                                                ,   20040101, 20050101, 20060101, 20070101
                                                                ,   20080101, 20090101, 20100101, 20110101
                                                                ,   20120101, 20130101, 20140101, 20150101
                                                                ,   20160101, 20170101, 20180101, 20190101
                                                                ,   20200101, 20210101, 20220101, 20230101
                                                                ,   20240101, 20250101, 20260101, 20270101
                                                                ,   20280101, 20290101
                                                                )
                        )
    )
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
OPTION  (LABEL  = 'CTAS : FactInternetSales_ROUND_ROBIN')
;

--Create statistics on new table
CREATE STATISTICS [ProductKey] ON [FactInternetSales_ROUND_ROBIN] ([ProductKey]);
CREATE STATISTICS [OrderDateKey] ON [FactInternetSales_ROUND_ROBIN] ([OrderDateKey]);
CREATE STATISTICS [CustomerKey] ON [FactInternetSales_ROUND_ROBIN] ([CustomerKey]);
CREATE STATISTICS [PromotionKey] ON [FactInternetSales_ROUND_ROBIN] ([PromotionKey]);
CREATE STATISTICS [SalesOrderNumber] ON [FactInternetSales_ROUND_ROBIN] ([SalesOrderNumber]);
CREATE STATISTICS [OrderQuantity] ON [FactInternetSales_ROUND_ROBIN] ([OrderQuantity]);
CREATE STATISTICS [UnitPrice] ON [FactInternetSales_ROUND_ROBIN] ([UnitPrice]);
CREATE STATISTICS [SalesAmount] ON [FactInternetSales_ROUND_ROBIN] ([SalesAmount]);

--Rename the tables
RENAME OBJECT [dbo].[FactInternetSales] TO [FactInternetSales_HASH];
RENAME OBJECT [dbo].[FactInternetSales_ROUND_ROBIN] TO [FactInternetSales];
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Tabellenentwurf zu erhalten, finden Sie unter [verteilen][], [Index][], [Partitions-][], [Datentypen][], [Statistiken][] und[temporäre] Artikeln [Temporäre Tabellen].

Übersicht über bewährte Methoden finden Sie unter [Bewährte Methoden für SQL Data Warehouse][].


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
[Abfrage für die Überwachung]: ./sql-data-warehouse-manage-monitor.md
[dbo.vTableSizes]: ./sql-data-warehouse-tables-overview.md#querying-table-sizes

<!--MSDN references-->
[DBCC PDW_SHOWSPACEUSED()]: https://msdn.microsoft.com/library/mt204028.aspx

<!--Other Web references-->

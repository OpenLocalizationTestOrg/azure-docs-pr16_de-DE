<properties
    pageTitle="SQL In-Memory, erste Schritte | Microsoft Azure"
    description="SQL-In-Memory-Technologien zu erheblich verbessern die Leistung von Transaktionen und Analytics Auslastung. Erfahren Sie, wie diese Technologien nutzen."
    services="sql-database"
    documentationCenter=""
    authors="jodebrui"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="jodebrui"/>


# <a name="get-started-with-in-memory-preview-in-sql-database"></a>Erste Schritte mit In-Memory (Preview) in SQL-Datenbank

In-Memory-Features zu erheblich verbessern die Leistung von Transaktionen und Analytics Auslastung in den richtigen Situationen.

In diesem Thema Schwerpunkt auf zwei Demos, In-Memory OLTP und im Arbeitsspeicher. Jede Demo wird mit den Schritten und Code werden müssten, führen Sie die Demo geliefert. Sie können:

- Verwenden Sie den Code testen Variationen zum Anzeigen der Unterschiede bei Leistungsergebnisse; oder
- Lesen Sie den Code, zu das Szenario verstehen und Informationen zum Erstellen und Verwenden von In-Memory-Objekte.

> [AZURE.VIDEO azure-sql-database-in-memory-technologies]

- [Schnelle starten 1: In-Memory OLTP Technologien schneller T-SQL-Leistung](http://msdn.microsoft.com/library/mt694156.aspx) -ist ein anderer Artikel, die Ihnen beim Einstieg helfen.

#### <a name="in-memory-oltp"></a>In-Memory-OLTP

Die Features von In-Memory [OLTP](#install_oltp_manuallink) (online Transaktion Verarbeitung) sind:

- Speicher optimiert Tabellen.
- Systembedingt kompiliert gespeicherte Prozeduren aus.


Eine Tabelle Speicher optimiert verfügt über eine Darstellung seiner selbst im aktiven Arbeitsspeicher, neben der standard-Darstellung auf einem Festplattenlaufwerk. Geschäftliche Transaktionen für die Tabelle schneller ausgeführt, da sie direkt mit nur die Darstellung interagieren, die im Speicher aktiv ist.

Mit In-Memory OLTP können Sie von erzielen, um 30 Mal in Transaktionsdurchsatz, abhängig von der Arbeitsbelastung zu erhalten.


Systembedingt kompilierte gespeicherte Prozeduren erfordern weniger Computer-Anweisungen zur Laufzeit als herkömmliche interpretiert gespeicherten Prozeduren. Wir haben systemeigener Kompilierungsergebnis in der Dauer, die 1 sind gesehen/100stel die Dauer interpretiert.


#### <a name="in-memory-analytics"></a>Im Arbeitsspeicher 

Das Feature von In-Memory [Analytics](#install_analytics_manuallink) lautet:

Columnstore Indizes verbessern die Leistung von Analysen und Berichte Abfragen. 


#### <a name="real-time-analytics"></a>In Echtzeit Analytics

Für [Real-Time Analytics](http://msdn.microsoft.com/library/dn817827.aspx) kombinieren Sie In-Memory OLTP und Analytics zu erhalten:

- Einen Einblick in Echtzeit Business auf Betrieb Daten basieren.


#### <a name="availability"></a>Verfügbarkeit


GA, allgemeine Verfügbarkeit:

- [Columnstore Indizes](http://msdn.microsoft.com/library/dn817827.aspx) , die *auf dem Datenträger*sind.


Vorschau:

- In-Memory-OLTP
- In Echtzeit Betrieb Analytics


Aspekte beim sind die In-Memory-Features in der Vorschau werden beschrieben [Weiter unten in diesem Thema](#preview_considerations_for_in_memory).


> [AZURE.NOTE] Diese Vorschau-Features sind nur für [*Premium*](sql-database-service-tiers.md) Datenbanken in flexible Pools nicht verfügbar, und nicht für alle Datenbanken Basic oder Standard.  Unterstützung für Premium Datenbanken in flexible Pools wird in Kürze zur Verfügung. 


<a id="install_oltp_manuallink" name="install_oltp_manuallink"></a>

&nbsp;

## <a name="a-install-the-in-memory-oltp-sample"></a>A Installieren Sie das Beispiel In-Memory-OLTP

Sie können die Beispieldatenbank AdventureWorksLT [V12] nach wenigen Klicks in [Azure-Portal](https://portal.azure.com/)erstellen. Klicken Sie dann die Schritte in diesem Abschnitt erläutert, wie Sie Ihre Datenbank AdventureWorksLT mit bereichern können wird:

- In-Memory-Tabellen.
- Eine systembedingt kompilierte gespeicherte Prozedur.


#### <a name="installation-steps"></a>Installationsschritte

1. Im [Portal Azure](https://portal.azure.com/)erstellen Sie eine Premium-Datenbank auf einem Server V12 aus. Legen Sie die **Quelle** in der Beispieldatenbank AdventureWorksLT [V12] ein.
 - Ausführliche Anweisungen sehen Sie [Erstellen Ihrer ersten Azure SQL-Datenbank](sql-database-get-started.md).

2. Verbinden Sie mit der Datenbank mit SQL Server Management Studio [(SSMS.exe)](http://msdn.microsoft.com/library/mt238290.aspx).

3. Kopieren Sie die [In-Memory OLTP Transact-SQL-Skript](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_oltp_sample.sql) in die Zwischenablage.
 - Das T-SQL-Skript erstellt die notwendigen In-Memory-Objekte in der Beispieldatenbank AdventureWorksLT, die Sie in Schritt 1 erstellt haben.

4. Fügen Sie des T-SQL-Skripts in SSMS, und führen Sie das Skript.
 - Unerlässlich ist die `MEMORY_OPTIMIZED = ON` Klausel Tabelle erstellen Anweisungen, wie in:


```
CREATE TABLE [SalesLT].[SalesOrderHeader_inmem](
    [SalesOrderID] int IDENTITY NOT NULL PRIMARY KEY NONCLUSTERED ...,
    ...
) WITH (MEMORY_OPTIMIZED = ON);
```


#### <a name="error-40536"></a>Fehler 40536


Wenn Sie beim Ausführen des T-SQL-Skripts Fehler 40536 erhalten, führen Sie das folgende T-SQL-Skript, um zu überprüfen, ob die Datenbank In-Memory unterstützt:


```
SELECT DatabasePropertyEx(DB_Name(), 'IsXTPSupported');
```


Ein Ergebnis von **0** bedeutet, dass In-Memory wird nicht unterstützt und 1 bedeutet, dass es unterstützt wird. Das Problem diagnostizieren zu können:

- Stellen Sie sicher, dass die Datenbank erstellt wurde, nachdem die OLTP In-Memory-Funktionen für Vorschau aktiv geworden ist.
- Stellen Sie sicher, dass die Datenbank auf der Premium-Service-Stufe ist.


#### <a name="about-the-created-memory-optimized-items"></a>Informationen zu den erstellten Speicher optimiert Elemente

**Tabellen**: im Beispiel enthält die folgenden Tabellen Speicher optimiert:

- SalesLT.Product_inmem
- SalesLT.SalesOrderHeader_inmem
- SalesLT.SalesOrderDetail_inmem
- Demo.DemoSalesOrderHeaderSeed
- Demo.DemoSalesOrderDetailSeed


Sie können Tabellen mithilfe des **Objekt-Explorer** in SSMS von Speicher optimiert prüfen:

- Mit der rechten Maustaste **Tabellen** > **Filter** > **Filtereinstellungen** > **Arbeitsspeicher optimiert ist** gleich 1.


Oder Sie können die Katalogsansichten wie Abfragen:


```
SELECT is_memory_optimized, name, type_desc, durability_desc
    FROM sys.tables
    WHERE is_memory_optimized = 1;
```


**Systembedingt kompilierte gespeicherte Prozedur**: SalesLT.usp_InsertSalesOrder_inmem durch eine Katalog-Abfrage überprüft werden können:


```
SELECT uses_native_compilation, OBJECT_NAME(object_id), definition
    FROM sys.sql_modules
    WHERE uses_native_compilation = 1;
```


&nbsp;

## <a name="run-the-sample-oltp-workload"></a>Führen Sie die Arbeitsbelastung Stichprobe OLTP

Der einzige Unterschied zwischen den folgenden zwei *gespeicherten Prozeduren* ist, dass die erste Prozedur verwendet Speicher optimiert Versionen der Tabellen, während der zweiten Vorgehensweise regulären Tabellen auf dem Datenträger verwendet:

- SalesLT**** aus. Usp_InsertSalesOrder**_inmem**
- SalesLT**** aus. Usp_InsertSalesOrder**_ondisk**


In diesem Abschnitt erfahren Sie, wie in dem Programm praktischen **ostress.exe** ausführen können, die beiden gespeicherten Prozeduren auf anstrengend Ebenen. Sie können vergleichen, wie lange dauert die zwei Belastung ausgeführt ausführen.


Wenn Sie ostress.exe ausführen, empfehlen wir, dass Sie dazu ausgelegt, beide Parameterwerte übergeben:

- Führen Sie eine große Anzahl der aktiven Verbindungen, durch die Verwendung - n100.
- Haben Sie jede Verbindungsschleife Hunderte an, wie oft von verwenden - r500.


Jedoch möglicherweise mit viel kleineren Werten wie - n10 beginnen soll und - arbeitet r50, um die alles sicherzustellen.


### <a name="script-for-ostressexe"></a>Skript für ostress.exe


In diesem Abschnitt zeigt das T-SQL-Skript, das in unseren ostress.exe Befehlszeile eingebettet ist. Das Skript verwendet die Elemente, die von der T-SQL-Skript erstellt wurden, die Sie zuvor installiert haben.


Das folgende Skript fügt einer Stichprobe Auftrag mit fünf Zeilenelementen in den folgenden Speicher optimiert *Tabellen*ein:

- SalesLT.SalesOrderHeader_inmem
- SalesLT.SalesOrderDetail_inmem


```
DECLARE
    @i int = 0,
    @od SalesLT.SalesOrderDetailType_inmem,
    @SalesOrderID int,
    @DueDate datetime2 = sysdatetime(),
    @CustomerID int = rand() * 8000,
    @BillToAddressID int = rand() * 10000,
    @ShipToAddressID int = rand() * 10000;
    
INSERT INTO @od
    SELECT OrderQty, ProductID
    FROM Demo.DemoSalesOrderDetailSeed
    WHERE OrderID= cast((rand()*60) as int);
    
WHILE (@i < 20)
begin;
    EXECUTE SalesLT.usp_InsertSalesOrder_inmem @SalesOrderID OUTPUT,
        @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @od;
    SET @i = @i + 1;
end
```


Um die _ondisk-Version von den vorherigen T-SQL-Code für ostress.exe zu machen, müssen Sie einfach beide Vorkommen der Teilzeichenfolge *_inmem* durch *_ondisk*ersetzen. Diese ersetzt Einfluss auf die Namen von Tabellen und gespeicherten Prozeduren.


### <a name="install-rml-utilities-and-ostress"></a>Installieren von RML-Dienstprogramme und ostress


Sie möchten idealerweise ostress.exe eine Azure-virtuellen Computers ausführen möchten. Sie möchten ein [Azure-virtuellen Computern](https://azure.microsoft.com/documentation/services/virtual-machines/) in der gleichen Azure geografische Region erstellen, in dem die AdventureWorksLT-Datenbank gespeichert ist. Doch Sie können ostress.exe stattdessen auf einem Laptop ausgeführt.


Wählen Sie auf welchen Sie hosten oder des virtuellen Computers aus, installieren Sie die Dienstprogramme Wiedergabe Markup Language (RML), die ostress.exe enthalten.

- Finden Sie unter [In-Memory-OLTP-Beispieldatenbank](http://msdn.microsoft.com/library/mt465764.aspx)ostress.exe.
 - Oder finden Sie unter [In-Memory-OLTP-Beispieldatenbank](http://msdn.microsoft.com/library/mt465764.aspx).
 - Oder finden Sie im [Blog für die Installation von ostress.exe](http://blogs.msdn.com/b/psssql/archive/2013/10/29/cumulative-update-2-to-the-rml-utilities-for-microsoft-sql-server-released.aspx)



<!--
dn511655.aspx is for SQL 2014,
[Extensions to AdventureWorks to Demonstrate In-Memory OLTP]
(http://msdn.microsoft.com/library/dn511655&#x28;v=sql.120&#x29;.aspx)

whereas for SQL 2016+
[Sample Database for In-Memory OLTP]
(http://msdn.microsoft.com/library/mt465764.aspx)
-->



### <a name="run-the-inmem-stress-workload-first"></a>Führen Sie die Arbeitsbelastung _inmem betonen zuerst


Ein *RML Cmd* Eingabeaufforderungsfenster können unsere ostress.exe Befehlszeile ausführen. Die Befehlszeilenparameter direkte Ostress an:

- 100 Verbindungen gleichzeitig ausgeführt werden (-n100).
- Jede Verbindung das T-SQL-Skript 50 Mal ausgeführt haben (-r50).


```
ostress.exe -n100 -r50 -S<servername>.database.windows.net -U<login> -P<password> -d<database> -q -Q"DECLARE @i int = 0, @od SalesLT.SalesOrderDetailType_inmem, @SalesOrderID int, @DueDate datetime2 = sysdatetime(), @CustomerID int = rand() * 8000, @BillToAddressID int = rand() * 10000, @ShipToAddressID int = rand()* 10000; INSERT INTO @od SELECT OrderQty, ProductID FROM Demo.DemoSalesOrderDetailSeed WHERE OrderID= cast((rand()*60) as int); WHILE (@i < 20) begin; EXECUTE SalesLT.usp_InsertSalesOrder_inmem @SalesOrderID OUTPUT, @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @od; set @i += 1; end"
```


So führen Sie die vorherige ostress.exe Befehlszeile:


1. Zurücksetzen des Daten Datenbankinhalt in SSMS, um alle Daten löschen, die indem Sie alle vorherigen ausgeführt eingefügt wurde, den folgenden Befehl ausführen:
```
EXECUTE Demo.usp_DemoReset;
```

2. Kopieren Sie den Text der vorherigen ostress.exe Befehlszeile in die Zwischenablage.

3. Ersetzen Sie die `<placeholders>` für den Parameter -S - U -P -d mit den richtigen realen Werten.

4. Ausführen der bearbeiteten Befehlszeile in einem RML Cmd-Fenster an.


#### <a name="result-is-a-duration"></a>Ergebnis ist eine Dauer


Beim ostress.exe abgeschlossen ist, schreibt die Dauer des Testlaufs als die letzte Zeile der Ausgabe im Fenster RML Cmd. Beispielsweise dauerte ein Testlaufs verkürzen etwa 1,5 Minuten:

`11/12/15 00:35:00.873 [0x000030A8] OSTRESS exiting normally, elapsed time: 00:01:31.867`


#### <a name="reset-edit-for-ondisk-then-rerun"></a>Zurücksetzen, für _ondisk bearbeiten, und klicken Sie dann erneut ausführen


Nachdem Sie das Ergebnis aus der _inmem ausführen haben, führen Sie die folgenden Schritte für die _ondisk ausführen:


1. Zurücksetzen der Datenbank in SSMS, löschen Sie alle Daten, die von der vorherigen Ausführung eingefügt wurde den folgenden Befehl ausführen:
```
EXECUTE Demo.usp_DemoReset;
```

2. Bearbeiten der ostress.exe Befehlszeile aus, um alle *_inmem* mit *_ondisk*zu ersetzen.

3. Führen Sie ostress.exe für der zweiten Zeitangabe erneut aus, und erfassen Sie das Ergebnis der Dauer.

4. Erneut Zurücksetzen der Datenbank, für die Löschung des was eine große Datenmenge Test werden können.


#### <a name="expected-comparison-results"></a>Der erwarteten Vergleichsergebnisse

Unsere In-Memory-Tests haben eine Verbesserung der Performance **9 Zeiten** für diese vereinfachte Arbeitsbelastung, mit Ostress eine Azure-virtuellen Computers in der gleichen Azure Region als die Datenbank ausgeführt gezeigt.



<a id="install_analytics_manuallink" name="install_analytics_manuallink"></a>

&nbsp;


## <a name="b-install-the-in-memory-analytics-sample"></a>B. Installieren Sie das Beispiel In Arbeitsspeicher


In diesem Abschnitt vergleichen Sie die Ergebnisse EA und Statistiken bei Verwendung ein Columnstore Index im Vergleich zu einem Index herkömmliche b-Struktur.


Für in Echtzeit Analytics auf einer OLTP Arbeitsbelastung empfiehlt es sich häufig verwendet einen Index nicht gruppierte Columnstore. Weitere Informationen finden Sie unter [Columnstore Indizes beschrieben](http://msdn.microsoft.com/library/gg492088.aspx).



### <a name="prepare-the-columnstore-analytics-test"></a>Vorbereiten des Columnstore Analytics-Tests


1. Verwenden Sie zum Erstellen einer frischen AdventureWorksLT-Datenbank aus der Stichprobe Azure-Portal.
 - Verwenden Sie die genauen Namen ein.
 - Wählen Sie alle Premium Service Ebene aus.

2. Kopieren Sie die [Sql_in-Memory_analytics_sample](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_analytics_sample.sql) in die Zwischenablage.
 - Das T-SQL-Skript erstellt die notwendigen In-Memory-Objekte in der Beispieldatenbank AdventureWorksLT, die Sie in Schritt 1 erstellt haben.
 - Das Skript erstellt die Tabelle Dimension und zwei Faktentabellen. 3,5 Millionen Zeilen werden die Faktentabellen eingetragen.
 - Das Skript möglicherweise 15 Minuten dauern.

3. Fügen Sie des T-SQL-Skripts in SSMS, und führen Sie das Skript.
 - Entscheidend ist das Schlüsselwort **COLUMNSTORE** in einer **CREATE INDEX** -Anweisung, wie in:<br/>`CREATE NONCLUSTERED COLUMNSTORE INDEX ...;`

4. Festlegen Sie AdventureWorksLT auf Kompatibilität Ebene 130:<br/>`ALTER DATABASE AdventureworksLT SET compatibility_level = 130;`
 - Ebene 130 ist nicht direkt an den Features von In-Memory verknüpft. Aber Ebene 130 bietet in der Regel eine schnellere Abfrage Verarbeitung als 120 unterstützt.


#### <a name="crucial-tables-and-columnstore-indexes"></a>Entscheidend Tabellen und Columnstore Indizes


- Dbo. FactResellerSalesXL_CCI ist eine Tabelle mit einem gruppierten **Columnstore** Index aus der Komprimierung auf *der Datenebene* erweitert wurde.

- Dbo. FactResellerSalesXL_PageCompressed ist eine Tabelle, die nur auf *Seitenebene* komprimiert ist einen entsprechenden regulären gruppierten Index verfügt.


#### <a name="crucial-queries-to-compare-the-columnstore-index"></a>Entscheidend Abfragen an den Index Columnstore vergleichen


[Es](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/clustered_columnstore_sample_queries.sql) gibt verschiedene Arten des T-SQL-Abfrage ausgeführt werden kann leistungsverbesserungen angezeigt. Schritt 2 im T-SQL-Skript gibt es ein Paar von Abfragen, die direkte interessiert sind. Die beiden Abfragen unterscheiden sich nur in einer einzigen Zeile:


- `FROM FactResellerSalesXL_PageCompressed a`
- `FROM FactResellerSalesXL_CCI a`


Ein gruppierte Columnstore Index befindet sich auf der FactResellerSalesXL**_CCI** -Tabelle.

Im folgende Ausschnitt des T-SQL-Skript gibt Statistiken für EA und die Uhrzeit für die Abfrage für jede Tabelle ein.


```
/*********************************************************************
Step 2 -- Overview
-- Page Compressed BTree table v/s Columnstore table performance differences
-- Enable actual Query Plan in order to see Plan differences when Executing
*/
-- Ensure Database is in 130 compatibility mode
ALTER DATABASE AdventureworksLT SET compatibility_level = 130
GO

-- Execute a typical query that joins the Fact Table with dimension tables
-- Note this query will run on the Page Compressed table, Note down the time
SET STATISTICS IO ON
SET STATISTICS TIME ON
GO

SELECT c.Year
    ,e.ProductCategoryKey
    ,FirstName + ' ' + LastName AS FullName
    ,count(SalesOrderNumber) AS NumSales
    ,sum(SalesAmount) AS TotalSalesAmt
    ,Avg(SalesAmount) AS AvgSalesAmt
    ,count(DISTINCT SalesOrderNumber) AS NumOrders
    ,count(DISTINCT a.CustomerKey) AS CountCustomers
FROM FactResellerSalesXL_PageCompressed a
INNER JOIN DimProduct b ON b.ProductKey = a.ProductKey
INNER JOIN DimCustomer d ON d.CustomerKey = a.CustomerKey
Inner JOIN DimProductSubCategory e on e.ProductSubcategoryKey = b.ProductSubcategoryKey
INNER JOIN DimDate c ON c.DateKey = a.OrderDateKey
WHERE e.ProductCategoryKey =2
    AND c.FullDateAlternateKey BETWEEN '1/1/2014' AND '1/1/2015'
GROUP BY e.ProductCategoryKey,c.Year,d.CustomerKey,d.FirstName,d.LastName
GO
SET STATISTICS IO OFF
SET STATISTICS TIME OFF
GO


-- This is the same Prior query on a table with a Clustered Columnstore index CCI 
-- The comparison numbers are even more dramatic the larger the table is, this is a 11 million row table only.
SET STATISTICS IO ON
SET STATISTICS TIME ON
GO
SELECT c.Year
    ,e.ProductCategoryKey
    ,FirstName + ' ' + LastName AS FullName
    ,count(SalesOrderNumber) AS NumSales
    ,sum(SalesAmount) AS TotalSalesAmt
    ,Avg(SalesAmount) AS AvgSalesAmt
    ,count(DISTINCT SalesOrderNumber) AS NumOrders
    ,count(DISTINCT a.CustomerKey) AS CountCustomers
FROM FactResellerSalesXL_CCI a
INNER JOIN DimProduct b ON b.ProductKey = a.ProductKey
INNER JOIN DimCustomer d ON d.CustomerKey = a.CustomerKey
Inner JOIN DimProductSubCategory e on e.ProductSubcategoryKey = b.ProductSubcategoryKey
INNER JOIN DimDate c ON c.DateKey = a.OrderDateKey
WHERE e.ProductCategoryKey =2
    AND c.FullDateAlternateKey BETWEEN '1/1/2014' AND '1/1/2015'
GROUP BY e.ProductCategoryKey,c.Year,d.CustomerKey,d.FirstName,d.LastName
GO

SET STATISTICS IO OFF
SET STATISTICS TIME OFF
GO
```



<a id="preview_considerations_for_in_memory" name="preview_considerations_for_in_memory"></a>


## <a name="preview-considerations-for-in-memory-oltp"></a>Vorschau Aspekte für In-Memory-OLTP


Die Features In-Memory OLTP in Azure SQL-Datenbank wurde [für die Vorschau auf 28 Oktober 2015 aktiv](https://azure.microsoft.com/updates/public-preview-in-memory-oltp-and-real-time-operational-analytics-for-azure-sql-database/).


Klicken Sie in der aktuellen Vorschau wird In-Memory OLTP nur für unterstützt:

- Datenbanken, die auf einer *Premium* -Service-Stufe sind.

- Datenbanken, die erstellt wurden, nachdem die Features In-Memory OLTP aktiv geworden ist.
 - Eine neue Datenbank kann In-Memory OLTP nicht unterstützen, wenn sie aus einer Datenbank wiederhergestellt wird, die erstellt wurde, bevor Sie die Features In-Memory OLTP aktiv geworden ist.


Im Zweifelsfall können Sie immer Ausführen der folgenden T-SQL-SELECT um festzustellen, ob Ihre Datenbank In-Memory OLTP unterstützt. Ein Ergebnis von **1** bedeutet, dass die Datenbank In-Memory OLTP unterstützt:

```
SELECT DatabasePropertyEx(DB_NAME(), 'IsXTPSupported');
```


Wenn die Abfrage gibt **1**zurück, In-Memory OLTP wird in dieser Datenbank unterstützt, und eine Datenbankkopie und die Datenbank wiederherstellen, erstellt ausgehend von dieser Datenbank.


#### <a name="objects-allowed-only-at-premium"></a>Objekte, die nur bei Premium zulässig.


Wenn die Datenbank eine der folgenden Arten von In-Memory OLTP-Objekte oder Datentypen enthält, wird das Downgrade der Dienstebene der Datenbank von Premium entweder einfache oder Standard nicht unterstützt. Um die Datenbank downgrade zu können, legen Sie zuerst dieser Objekte ab:

- Tabellen Speicher optimiert
- Tabelle Speicher optimiert Typen
- Systembedingt kompilierte Module


#### <a name="other-relationships"></a>Weitere Beziehungen


- Verwenden von In-Memory OLTP Features mit Datenbanken in flexible Pools wird in der Vorschau nicht unterstützt.
 - Gehen Sie folgendermaßen vor, um eine Datenbank zu verschieben, die oder Objekte In-Memory OLTP hatte die eine flexible Ressourcenpool:
  - 1. Legen Sie alle Tabellen Speicher optimiert, Tabellentypen und systembedingt kompilierte T-SQL-Module in der Datenbank
  - 2. Ändern der Dienstebene der Datenbank in standard
  - 3. Verschieben Sie die Datenbank in das flexible Ressourcenpool

- Verwenden von In-Memory OLTP mit SQL Data Warehouse wird nicht unterstützt.
 - Die Index-Funktion Columnstore, der im Arbeitsspeicher wird in SQL Data Warehouse unterstützt.

- Die Abfrage Store erfasst nicht Abfragen, dass innen systembedingt Module kompiliert.

- Einige Transact-SQL-Features werden nicht mit In-Memory OLTP unterstützt. Dies gilt für Microsoft SQL Server und Azure SQL-Datenbank. Weitere Informationen finden Sie unter:
 - [Transact-SQL-Unterstützung für In-Memory-OLTP](http://msdn.microsoft.com/library/dn133180.aspx)
 - [Transact-SQL-Konstrukte von In-Memory OLTP nicht unterstützt](http://msdn.microsoft.com/library/dn246937.aspx)


## <a name="next-steps"></a>Nächste Schritte


- Versuchen Sie es [verwenden In-Memory OLTP in einer vorhandenen Azure SQL-Anwendung.](sql-database-in-memory-oltp-migration.md)


## <a name="additional-resources"></a>Zusätzliche Ressourcen

#### <a name="deeper-information"></a>Tiefer Informationen

- [Erfahren Sie mehr über In-Memory-OLTP, welche gilt für Microsoft SQL Server und Azure SQL-Datenbank](http://msdn.microsoft.com/library/dn133186.aspx)

- [Erfahren Sie mehr über in Echtzeit Betrieb Analytics auf MSDN](http://msdn.microsoft.com/library/dn817827.aspx)

- Whitepaper auf [Allgemeine Arbeitsbelastung Muster und Aspekte der Migration](http://msdn.microsoft.com/library/dn673538.aspx), die Arbeitsbelastung Muster beschreibt, in denen In-Memory OLTP häufig sich die Systemleistung bereitstellt.

#### <a name="application-design"></a>Entwerfen der Anwendung

- [In-Memory-OLTP (In-Memory Optimization)](http://msdn.microsoft.com/library/dn133186.aspx)

- [In-Memory-OLTP in einer vorhandenen Azure SQL-Anwendung verwenden.](sql-database-in-memory-oltp-migration.md)

#### <a name="tools"></a>Tools

- [SQL Server Data Tools Preview (SSDT)](http://msdn.microsoft.com/library/mt204009.aspx), nach der neuesten Version des monatlichen.

- [Beschreibung der Wiedergabe Markup Language (RML) Dienstprogramme für SQLServer](http://support.microsoft.com/en-us/kb/944837)

- [Monitor In-Memory-Speicher](sql-database-in-memory-oltp-monitoring.md) für In-Memory OLTP.


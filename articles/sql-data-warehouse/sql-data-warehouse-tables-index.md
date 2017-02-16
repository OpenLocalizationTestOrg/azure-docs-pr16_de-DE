<properties
   pageTitle="Tabellen in SQL Data Warehouse Indizierung | Microsoft Azure"
   description="Erste Schritte mit der Tabelle in Azure SQL-Data Warehouse Indizierung."
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
   ms.date="07/12/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="indexing-tables-in-sql-data-warehouse"></a>Tabellen in SQL Data Warehouse Indizierung

> [AZURE.SELECTOR]
- [(Übersicht)][]
- [Datentypen][]
- [Verteilen][]
- [Index][]
- [Partition][]
- [Statistik][]
- [Temporäre][]

SQL Data Warehouse bietet mehrere Indizierungsoptionen einschließlich [gruppierte Columnstore Indizes][], [gruppierte Indizes und nicht gruppierte Indizes][].  Darüber hinaus gibt es auch eine keine Indexoption auch bekannt als [Heap][].  Dieser Artikel behandelt die Vorteile der einzelnen Indextyp sowie Tipps, die die meisten Leistung von Ihrer Indizes. Finden Sie unter [Erstellen von Table-Syntax][] für weitere Details zum Erstellen einer Tabelle in SQL Data Warehouse.

## <a name="clustered-columnstore-indexes"></a>Gruppierte Columnstore Indizes

SQL Data Warehouse erstellt standardmäßig einen Index gruppierte Columnstore aus, wenn keine Indexoptionen für eine Tabelle angegeben sind. Gruppierte Columnstore Tabellen bieten sowohl der höchsten Ebene der Daten Komprimierung als auch der für die optimale allgemeine abfrageleistung.  Gruppierte Columnstore Tabellen werden im Allgemeinen gruppierte Index oder Heap Tabellen sind und sind in der Regel die beste Wahl für große Tabellen.  Aus diesen Gründen ist gruppierte Columnstore am besten starten, wenn Sie nicht sicher sind, wie Sie Ihre Tabelle indiziert sind.  

Zum Erstellen einer Tabelle gruppierte Columnstore einfach Geben Sie gruppierten COLUMNSTORE INDEX in der WITH-Klausel an, oder lassen Sie die WITH-Klausel deaktivieren:

```SQL
CREATE TABLE myTable   
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH ( CLUSTERED COLUMNSTORE INDEX );
```

Es gibt ein paar Szenarien, wo gruppierte Columnstore eine gute Option möglicherweise nicht:

- Sekundäre nicht gruppierte Indizes unterstützt Columnstore Tabellen nicht.  Erwägen Sie stattdessen Heap oder gruppierten Index Tabellen.
- "Nvarchar(max)", "varchar(max)", nvarchar(max) und varbinary(max) unterstützt Columnstore Tabellen nicht.  Erwägen Sie stattdessen Heap oder gruppierten Index.
- Columnstore Tabellen möglicherweise weniger effizient für vorübergehende Daten.  Erwägen Sie Heap und vielleicht sogar temporäre Tabellen.
- Kleine Tabellen mit weniger als 100 Millionen Zeilen.  Erwägen Sie Heaptabellen an.

## <a name="heap-tables"></a>Heaptabellen

Wenn Sie Daten auf SQL Data Warehouse vorübergehend Start sind, kann es passieren, dass mithilfe einer Tabelle Heap gesamten Vorgang schneller vornehmen.  Dies liegt daran lädt für Heaps sind schneller als Index Tabellen und in einigen Fällen, die nachfolgende Lesen aus dem Cache vorgenommen werden kann.  Wenn Sie Daten nur, wenn sie vor dem Ausführen von weitere Transformationen Phaseneigenschaften laden, werden Laden der Tabelle Heap Tabelle sehr viel schneller, die Daten zu einer Tabelle gruppierte Columnstore geladen. Darüber hinaus werden Laden von Daten in eine [temporäre Tabelle][temporäre] auch schneller als das Laden einer Tabelle dauerhaft zu laden.  

Für kleine Nachschlagetabellen, die weniger als 100 Millionen Zeilen, sinnvoll oft Heaptabellen.  Optimale Komprimierung erzielen, nachdem es mehr als 100 Millionen Zeilen gibt Cluster Columnstore Tabellen zu beginnen.

Geben Sie zum Erstellen einer Tabelle Heap einfach HEAP in der WITH-Klausel:

```SQL
CREATE TABLE myTable   
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH ( HEAP );
```

## <a name="clustered-and-nonclustered-indexes"></a>Gruppierte und nicht gruppierte Indizes

Gruppierte Indizes möglicherweise gruppierte Columnstore Tabellen sind, wenn Sie eine einzelne Zeile schnell abgerufen werden muss.  Erwägen Sie für Abfragen, in dem ein einzelnes oder nur wenige Zeile Nachschlagen Leistung mit extrem Geschwindigkeit erforderlich ist, eine Cluster Index oder nicht gruppierter sekundäre Index aus.  Die Nachteile bei der Verwendung eines gruppierten Indexes ist, dass nur Abfragen, die verwenden ein Filters hochgradig Selektives auf die gruppierten Indexspalte Dienstleistung an.  Zur Verbesserung der Filters auf andere Spalten kann ein nicht gruppierter Index auf andere Spalten hinzugefügt werden.  Jedoch wird jeder Index die zu einer Tabelle hinzugefügt wird sowohl Speicherplatz und Verarbeitungszeit hinzufügen lädt.

Geben Sie zum Erstellen einer gruppierten Index-Tabelle in der WITH-Klausel einfach gruppierten INDEX:

```SQL
CREATE TABLE myTable   
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH ( CLUSTERED INDEX (id) );
```

Um einen nicht gruppierten Index für eine Tabelle hinzufügen möchten, verwenden Sie einfach die folgende Syntax:

```SQL
CREATE INDEX zipCodeIndex ON t1 (zipCode);
```

> [AZURE.NOTE] Ein nicht gruppierten Index wird standardmäßig erstellt, wenn INDEX erstellen verwendet wird. Darüber hinaus ist ein nicht gruppierten Index nur für eine Zeile Speichertabelle (HEAP oder gruppierten INDEX) zulässig. Nicht gruppierte Indizes auf einem gruppierten COLUMNSTORE INDEX ist zurzeit nicht zulässig.


## <a name="optimizing-clustered-columnstore-indexes"></a>Optimieren von gruppierten Columnstore Indizes

Gruppierte Columnstore Tabellen werden Daten in Segmente angezeigt.  Probleme Segment hoher Qualität ist entscheidend, um optimale abfrageleistung für eine Tabelle Columnstore zu erreichen.  Segment Qualität kann durch die Anzahl der Zeilen in einer Zeilengruppe komprimierte gemessen werden.  Segment Qualität wird am besten geeignete, wobei mindestens Zeilen von 100 KB pro Zeile komprimierte gruppieren und erlangen Leistungsabfall als die Anzahl der Zeilen pro Zeile Gruppe Ansatz 1.048.576 Zeilen, welche ist eine Zeilengruppe enthalten, kann die meisten Zeilen vorhanden sind.

Die unter Ansicht erstellt und auf Ihrem System verwendet, berechnet die durchschnittlichen Zeilen pro Zeile Gruppieren und optimal Cluster Columnstore Indizes identifizieren kann.  Die letzte Spalte in dieser Ansicht werden als SQL-Anweisung generieren, die verwendet werden können, um Ihre Indizes neu zu erstellen.

```sql
CREATE VIEW dbo.vColumnstoreDensity
AS
SELECT
        GETDATE()                                                               AS [execution_date]
,       DB_Name()                                                               AS [database_name]
,       s.name                                                                  AS [schema_name]
,       t.name                                                                  AS [table_name]
,   COUNT(DISTINCT rg.[partition_number])                   AS [table_partition_count]
,       SUM(rg.[total_rows])                                                    AS [row_count_total]
,       SUM(rg.[total_rows])/COUNT(DISTINCT rg.[distribution_id])               AS [row_count_per_distribution_MAX]
,   CEILING ((SUM(rg.[total_rows])*1.0/COUNT(DISTINCT rg.[distribution_id]))/1048576) AS [rowgroup_per_distribution_MAX]
,       SUM(CASE WHEN rg.[State] = 0 THEN 1                   ELSE 0    END)    AS [INVISIBLE_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE 0    END)    AS [INVISIBLE_rowgroup_rows]
,       MIN(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE NULL END)    AS [INVISIBLE_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE NULL END)    AS [INVISIBLE_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE NULL END)    AS [INVISIBLE_rowgroup_rows_AVG]
,       SUM(CASE WHEN rg.[State] = 1 THEN 1                   ELSE 0    END)    AS [OPEN_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE 0    END)    AS [OPEN_rowgroup_rows]
,       MIN(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE NULL END)    AS [OPEN_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE NULL END)    AS [OPEN_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE NULL END)    AS [OPEN_rowgroup_rows_AVG]
,       SUM(CASE WHEN rg.[State] = 2 THEN 1                   ELSE 0    END)    AS [CLOSED_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE 0    END)    AS [CLOSED_rowgroup_rows]
,       MIN(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE NULL END)    AS [CLOSED_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE NULL END)    AS [CLOSED_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE NULL END)    AS [CLOSED_rowgroup_rows_AVG]
,       SUM(CASE WHEN rg.[State] = 3 THEN 1                   ELSE 0    END)    AS [COMPRESSED_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE 0    END)    AS [COMPRESSED_rowgroup_rows]
,       SUM(CASE WHEN rg.[State] = 3 THEN rg.[deleted_rows]   ELSE 0    END)    AS [COMPRESSED_rowgroup_rows_DELETED]
,       MIN(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE NULL END)    AS [COMPRESSED_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE NULL END)    AS [COMPRESSED_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE NULL END)    AS [COMPRESSED_rowgroup_rows_AVG]
,       'ALTER INDEX ALL ON ' + s.name + '.' + t.NAME + ' REBUILD;'             AS [Rebuild_Index_SQL]
FROM    sys.[pdw_nodes_column_store_row_groups] rg
JOIN    sys.[pdw_nodes_tables] nt                   ON  rg.[object_id]          = nt.[object_id]
                                                    AND rg.[pdw_node_id]        = nt.[pdw_node_id]
                                                    AND rg.[distribution_id]    = nt.[distribution_id]
JOIN    sys.[pdw_table_mappings] mp                 ON  nt.[name]               = mp.[physical_name]
JOIN    sys.[tables] t                              ON  mp.[object_id]          = t.[object_id]
JOIN    sys.[schemas] s                             ON t.[schema_id]            = s.[schema_id]
GROUP BY
        s.[name]
,       t.[name]
;
```

Jetzt, da Sie die Ansicht erstellt haben, führen Sie diese Abfrage um Tabellen mit Zeilengruppen mit weniger als 100 KB Zeilen zu identifizieren.  Natürlich, wenn Sie den oberen Schwellenwert von 100 KB vergrößern, wenn Sie weitere optimale Segment Qualität suchen möchten. 

```sql
SELECT  *
FROM    [dbo].[vColumnstoreDensity]
WHERE   COMPRESSED_rowgroup_rows_AVG < 100000
        OR INVISIBLE_rowgroup_rows_AVG < 100000
```

Nachdem Sie die Abfrage ausgeführt haben, die Sie beginnen können, schauen Sie sich die Daten und die Ergebnisse zu analysieren. Diese Tabelle erläutert, wie Sie in der Zeile Gruppe Analyse suchen.


| Spalte                             | So verwenden Sie diese Daten                                                                                                                                                                      |
| ---------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [Table_partition_count]            | Wenn die Tabelle konfiguriert ist, dann erwarten Sie möglicherweise zu prüfen, ob höhere öffnen Zeilengruppe ermittelt. Jede Partition in der Verteilung konnte theoretisch eine geöffneten Zeilengruppe zugeordnet haben. Berücksichtigen Sie dies in Ihrer Analyse aus. Eine kleine Tabelle, die weist unterteilt wurde durch die Partitionierung ganz entfernen optimiert werden konnte wie folgt Komprimierung verbessern möchten.                                                                        |
| [Row_count_total]                  | Ergebniszeile Anzahl für die Tabelle. Beispielsweise können Sie diesen Wert zum Berechnen des Prozentsatzes der Zeilen in der komprimierten Zustand aus.                                                                      |
| [Row_count_per_distribution_MAX]   | Wenn Sie alle Zeilen gleichmäßig verteilt werden wäre dieser Wert die Ziel-Anzahl von Zeilen pro Verteilung zurück. Vergleichen Sie diesen Wert mit dem Compressed_rowgroup_count.                                 |
| [COMPRESSED_rowgroup_rows]         | Die Gesamtzahl der Zeilen im Columnstore Format für die Tabelle.                                                                                                                                 |
| [COMPRESSED_rowgroup_rows_AVG]     | Ist die durchschnittliche Anzahl von Zeilen erheblich kleiner als der maximalen Anzahl von Zeilen für eine Zeilengruppe, sollten Sie CTAS oder ALTER INDEX neu erstellen verwenden, die Daten neu komprimieren.                     |
| [COMPRESSED_rowgroup_count]        | Anzahl der Zeilengruppen im Format Columnstore. Diese Anzahl ist sehr hoch in Bezug auf die Tabelle ein Indikator, dass die Dichtefunktion Columnstore niedrig ist.                                  |
| [COMPRESSED_rowgroup_rows_DELETED] | Zeilen werden im Format Columnstore logisch gelöscht. Ist im Verhältnis zur Tabellengröße hoher belegt, erwägen Sie die Partition, neu zu erstellen oder erneutes Erstellen des Indexes wie folgt physisch entfernt. |
| [COMPRESSED_rowgroup_rows_MIN]     | Verwenden Sie diese zusammen mit dem Mittelwert und MAX Spalten um zu verstehen des Bereiches der Werte für die Zeilengruppen in Ihrer Columnstore. Eine niedrige Zahl über den Schwellenwert laden (102,400 pro Partition ausgerichtete Verteilung) wird vorgeschlagen, dass Optimierungen in das Laden von Daten zur Verfügung stehen.                                                                                                                                                 |
| [COMPRESSED_rowgroup_rows_MAX]     | Siehe oben                                                                                                                                                                                  |
| [OPEN_rowgroup_count]              | Öffnen Zeile gruppiert sind normal. Eine erwarten einigermaßen eine GEÖFFNETEN Zeilengruppe pro Tabelle Verteilung (60). Übermäßige Zahlen vorschlagen partitionsübergreifend Laden von Daten. Überprüfen Sie die Partitionierungsstrategie, um sicherzustellen, dass es wird sound                                                                                                                                                                                                |
| [OPEN_rowgroup_rows]               | Jede Zeilengruppe kann bis zu 1.048.576 Zeilen darin werden. Verwenden Sie diesen Wert, um festzustellen, wie voll sind derzeit geöffneten Zeilengruppen                                                                 |
| [OPEN_rowgroup_rows_MIN]           | Öffnen Gruppen angeben, dass Daten entweder Lager wird in die Tabelle geladen oder, dass die vorherige laden verschüttet über die restlichen Zeilen in dieser Zeilengruppe. Verwenden die MIN, MAX, Mittelwert Spalten, um anzuzeigen, wie viele Daten öffnen musst ist Zeile Gruppen. Für kleine Tabellen könnte es 100 % aller Daten! In diesem ALTER INDEX neu erstellen, um die Daten zu Columnstore erzwingen Fall.                                                                       |
| [OPEN_rowgroup_rows_MAX]           | Siehe oben                                                                                                                                                                                  |
| [OPEN_rowgroup_rows_AVG]           | Siehe oben                                                                                                                                                                                  |
| [CLOSED_rowgroup_rows]             | Prüfen Sie die geschlossenen Zeile Gruppieren von Zeilen als Überprüfung aus.                                                                                                                                       |
| [CLOSED_rowgroup_count]            | Die Anzahl der geschlossenen Zeilengruppen sollten niedrig, wenn eine überhaupt angezeigt werden. Geschlossene Zeilengruppen können in komprimierte Rowg Roups mithilfe des Indexes ALTER konvertiert... REORGANISIERT Befehl an. Dies ist jedoch nicht normal erforderlich. Geschlossene Gruppen werden automatisch vom Prozess "Tupels Mover" Hintergrund Columnstore Zeile Gruppen konvertiert.                                                                                               |
| [CLOSED_rowgroup_rows_MIN]         | Geschlossene Zeilengruppen sollte einen Satz sehr hoch Füllung haben. Wenn die Füllung Abschlag (Disagio) einer Zeilengruppe geschlossenen niedrig ist, ist eine weitere Analyse von der Columnstore erforderlich.                                   |
| [CLOSED_rowgroup_rows_MAX]         | Siehe oben                                                                                                                                                                                  |
| [CLOSED_rowgroup_rows_AVG]         | Siehe oben                                                                                                                                                                                  |
| [Rebuild_Index_SQL]         | SQL, Columnstore Index für eine Tabelle neu zu erstellen.                                                                                                                                                     |

## <a name="causes-of-poor-columnstore-index-quality"></a>Ursachen für schlechte Columnstore Index Qualität

Wenn Sie Tabellen mit Qualität beeinträchtigt Segment identifiziert haben, werden Sie die Ursache ermitteln möchten.  Nachstehend sind einige andere häufigsten Ursachen für schlechte Segment Quaility aus:

1. Arbeitsspeicherdruck, wenn der Index erstellt wurde
2. Große Anzahl von DML-Vorgänge
3. Kleine oder laden Vorgänge durchgelassen werden sollen
4. Zu viele Partitionen

Diese Faktoren können dazu führen, dass einen Index Columnstore erheblich sein, die kleiner als die optimalen 1 Millionen Zeilen pro Zeilengruppe.  Sie können auch Zeilen, wechseln Sie zur Gruppe "Delta Zeile" statt einer Zeilengruppe komprimierte verursachen. 

### <a name="memory-pressure-when-index-was-built"></a>Arbeitsspeicherdruck, wenn der Index erstellt wurde

Die Anzahl der Zeilen pro komprimierte Zeilengruppe beziehen sich direkt auf die Breite der Zeile und der Menge an Arbeitsspeicher verfügbar, um die Zeilengruppe auszuführen.  Wenn Zeilen in Columnstore Tabellen unter Arbeitsspeicherdruck geschrieben werden, beeinträchtigt Columnstore Segment Qualität.  Daher ist die beste Methode die Sitzung erteilen der Schreiben in Ihren Columnstore Index Tabellen Zugriff zu viel Speicher wie möglich ist.  Da es eine Wechselwirkung zwischen Arbeitsspeicher und Parallelität, den Anweisungen auf der Rechte arbeitsspeicherzuteilung besteht hängt davon ab, die Daten in jede Zeile der Tabelle, die Menge des DWU, die Sie Ihrem System zugewiesen haben, und der Menge an Parallelität Steckplätze, den, die Sie die Sitzung gewähren können das Schreiben von Daten in der Tabelle.  Es wird empfohlen, als bewährte Methode beginnend mit Xlargerc, wenn Sie DW300 verwenden oder weniger Largerc, wenn Sie DW400 DW600 und Mediumrc verwenden, wenn Sie DW1000 verwenden und über.

### <a name="high-volume-of-dml-operations"></a>Große Anzahl von DML-Vorgänge

Eine große Anzahl von DML-Vorgänge aktualisieren und Löschen von Zeilen kann in der Columnstore Effizienz vorstellen können. Dies gilt besonders, wenn die Mehrzahl der Zeilen in einer Zeilengruppe geändert werden.

- Löschen einer Zeile aus einer Zeilengruppe komprimierte nur logisch markiert die Zeile als gelöscht. Die Zeile bleibt in der Zeilengruppe komprimierte, bis die Partition oder die Tabelle neu erstellt wird.
- Einfügen einer Zeile wird die Zeile zu einer Gruppe Zeile Delta genannt internen Rowstore-Tabelle hinzugefügt. Die eingefügte Zeile wird nicht in Columnstore konvertiert, bis die Delta Zeilengruppe voll ist und als geschlossen markiert ist. Zeilengruppen geschlossen sind, nachdem sie die maximale Kapazität 1.048.576 Zeilen erreichen. 
- Aktualisieren einer Zeile in Columnstore Format wird als eine logische löschen und anschließend eine Insert verarbeitet werden. Die eingefügte Zeile möglicherweise in der Delta gespeichert werden.

Update zusammengefasst, und fügen Sie Vorgänge, die größer sind als den Schwellenwert Massen 102.400 Zeilen pro Partition ausgerichtet Verteilung direkt in das Format Columnstore geschrieben werden wird. Jedoch müssen eine gleichmäßige Verteilung wird vorausgesetzt, Sie auf das Ändern von mehr als 6.144 Millionen von Zeilen in einem einzigen Vorgang für diese ausgeführt werden. Wenn die Anzahl der Zeilen für eine bestimmte Partition ausgerichtet ist Verteilung kleiner als 102,400 aus, und klicken Sie dann die Zeilen im Speicher Delta geleitet werden und es verbleibt bis ausreichend Zeilen eingefügt oder zum Schließen der Zeilengruppe geändert wurden oder der Index wurde neu erstellt.

### <a name="small-or-trickle-load-operations"></a>Kleine oder laden Vorgänge durchgelassen werden sollen

Kleine lädt die Fluss in SQL Data Warehouse werden manchmal auch bekannt als lädt durchgelassen werden sollen. Sie stellen in der Regel einen Nahen Konstanten Videodatenstrom Daten vom System aufgenommen werden. In der Nähe fortlaufender ungeändert diesen Stream ist die Lautstärke der Zeilen nicht besonders groß. Die Daten ist in den meisten Fällen erheblich unter den Schwellenwert für eine direkte Last Columnstore Format erforderlich.

In diesen Fällen empfiehlt sich häufig die Daten zunächst in Azure Blob-Speicher landen, und lassen sie die leistungsstufenpunkte vor dem Laden. Dieses Verfahren wird häufig als *Micro Batchverarbeitung*bezeichnet.

### <a name="too-many-partitions"></a>Zu viele Partitionen

Die Auswirkung auf gruppierte Columnstore Tabellen Aufteilung ist ein anderes außerdem zu berücksichtigen.  Vor dem Partitionieren teilt SQL Data Warehouse bereits Ihre Daten in 60-Datenbanken auf.  Partitionierung teilt weiteren von Daten aus.  Wenn Sie Ihre Daten aufteilen, sollten Sie erwägen, dass **jede** Partition mindestens 1 Millionen von Zeilen aus einem gruppierten Columnstore Index nutzen zu können muss.  Wenn Sie die Tabelle in 100 Partitionen aufteilen, müssen die Tabelle, dass mindestens 6 Milliarden Zeilen aus einem gruppierten Columnstore Index (60 Verteilung *100 Partitionen* 1 Millionen Zeilen) zu nutzen. Wenn Ihre 100 Partitionstabelle keinen 6 Milliarden Zeilen, verringern Sie die Anzahl der Partitionen oder verwenden Sie eine Tabelle Heap stattdessen.

Nachdem Sie Ihre Tabellen mit einigen Daten geladen wurden, führen Sie die folgende Schritte aus, zu identifizieren und Tabellen mit optimal Cluster Columnstore Indizes neu erstellen.

## <a name="rebuilding-indexes-to-improve-segment-quality"></a>Neuerstellen von Indizes zur Verbesserung der Qualität segment

### <a name="step-1-identify-or-create-user-which-uses-the-right-resource-class"></a>Schritt 1: Identifizieren Sie oder erstellen Sie Benutzer, die die richtigen Klasse verwendet.

Eine schnelle Möglichkeit zum sofort Segment Qualität zu verbessern, ist den Index neu erstellen.  Die SQL-Anweisung durch die oben ausgewählte Ansicht zurückgegeben wird eine Anweisung ALTER INDEX neu erstellen zurückgegeben, der verwendet werden können, um Ihre Indizes neu zu erstellen.  Wenn Sie Ihre Indizes neu zu erstellen, müssen Sie sicher, dass Sie über genügend Arbeitsspeicher, um die Sitzung reservieren der den Index neu erstellt wird.  Vergrößern Sie hierzu die Klasse eines Benutzers die Berechtigungen zur Verwaltung der Index für diese Tabelle als empfohlenes Minimum neu erstellt hat.  Die Klasse des Datenbankbenutzers Besitzer kann nicht geändert werden, wenn Sie einen Benutzer nicht auf dem System erstellt haben, Sie zuerst vergeblich müssen.  Das Minimum, das wir empfehlen, ist Xlargerc bei Verwendung von DW300 oder weniger Largerc ab, wenn Sie DW400 DW600 und Mediumrc verwenden, wenn Sie DW1000 verwenden und über.

Es folgt ein Beispiel für mehr Speicher einem Benutzer zuweisen, indem Sie ihrer Ressourcenklasse.  Weitere Informationen zu Ressourcen Klassen und zum Erstellen eines neuen Benutzers finden Sie in der [Parallelität und Arbeitsbelastung Management] [ Concurrency] Artikel.

```sql
EXEC sp_addrolemember 'xlargerc', 'LoadUser'
```

### <a name="step-2-rebuild-clustered-columnstore-indexes-with-higher-resource-class-user"></a>Schritt 2: Quelltabelle gruppierte Columnstore Indizes mit höheren Ressource Klasse Benutzer
Melden Sie sich als der Benutzer aus Schritt 1 (z. B. LoadUser), welches jetzt eine höhere Ressource-Klasse ist, und führen Sie die Anweisungen ALTER INDEX.  Achten Sie darauf, dass diese Benutzer ALTER-Berechtigung mit den Tabellen hat Stelle, an der der Index neu erstellt wird.  Diesen Beispielen wird gezeigt, wie Sie den gesamte Columnstore Index neu zu erstellen oder wie Sie eine einzelne Partition neu erstellen. Bei großen Tabellen ist es geeignet, neu zu erstellen, eine einzelne Partition nacheinander indiziert.

Alternativ könnten Sie statt erneutes Erstellen des Indexes aus, die Tabelle in eine neue Tabelle mit [CTAS][]kopieren.  Welche Methode am besten geeignet ist? Für große Datenmengen ist [CTAS][] normalerweise schneller als [ALTER INDEX][]. Für kleinere Datenmengen [ALTER INDEX][] ist einfacher zu verwenden und müssen Sie die Tabelle können nicht.  Finden Sie unter **Rebuilding Indizes mit CTAS und Partition umsteigen** unter Weitere Details zum Indizes mit CTAS neu zu erstellen.

```sql
-- Rebuild the entire clustered index
ALTER INDEX ALL ON [dbo].[DimProduct] REBUILD
```

```sql
-- Rebuild a single partition
ALTER INDEX ALL ON [dbo].[FactInternetSales] REBUILD Partition = 5
```

```sql
-- Rebuild a single partition with archival compression
ALTER INDEX ALL ON [dbo].[FactInternetSales] REBUILD Partition = 5 WITH (DATA_COMPRESSION = COLUMNSTORE_ARCHIVE)
```

```sql
-- Rebuild a single partition with columnstore compression
ALTER INDEX ALL ON [dbo].[FactInternetSales] REBUILD Partition = 5 WITH (DATA_COMPRESSION = COLUMNSTORE)
```

Erneutes Erstellen eines Indexes in SQL Data Warehouse ist ein offline-Vorgang.  Weitere Informationen zu Indizes neu erstellt werden finden Sie im Abschnitt ALTER INDEX neu erstellen, in [Columnstore Indizes Defragmentierung][] und im Thema Syntax [ALTER INDEX][].
 
### <a name="step-3-verify-clustered-columnstore-segment-quality-has-improved"></a>Schritt 3: Stellen Sie sicher, dass die gruppierten Columnstore Segment Qualität verbessert wurde
Erneut ausführen der Abfrage mit schlechte welcher identifizierte Tabelle segmentieren Qualität und überprüfen Sie die Qualität Segment wurde verbessert.  Wenn Segment Qualität nicht verbessern lässt, ist möglicherweise, dass die Zeilen in der Tabelle zusätzliche breit sind.  Wenn Sie Ihre Indizes neu aufbauen, sollten Sie eine höhere Ressourcenklasse oder DWU in Betracht ziehen.

 
## <a name="rebuilding-indexes-with-ctas-and-partition-switching"></a>Neuerstellen von Indizes mit CTAS und Partition wechseln

In diesem Beispiel wird die [CTAS][] und Partition wechseln, um eine Tabellenpartition neu zu erstellen. 

```sql
-- Step 1: Select the partition of data and write it out to a new table using CTAS
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
FROM    [dbo].[FactInternetSales]
WHERE   [OrderDateKey] >= 20000101
AND     [OrderDateKey] <  20010101
;

-- Step 2: Create a SWITCH out table
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
FROM    [dbo].[FactInternetSales]
WHERE   1=2 -- Note this table will be empty

-- Step 3: Switch OUT the data 
ALTER TABLE [dbo].[FactInternetSales] SWITCH PARTITION 2 TO  [dbo].[FactInternetSales_20000101] PARTITION 2;

-- Step 4: Switch IN the rebuilt data
ALTER TABLE [dbo].[FactInternetSales_20000101_20010101] SWITCH PARTITION 2 TO  [dbo].[FactInternetSales] PARTITION 2;
```

Weitere Details zum Neuerstellen von Partitionen mit `CTAS`, finden Sie im Artikel [Partition][] .

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter den Artikeln auf [Tabelle Übersicht][Übersicht], [Tabelle Datentypen][Datentypen], [Verteilen einer Tabelle][verteilen], [eine-Tabelle teilen][Partition], [Tabellenstatistiken Verwalten von][Statistiken] und [Temporäre Tabellen][temporäre].  Weitere Informationen zu best Practices finden Sie unter [Bewährte Methoden für SQL Data Warehouse][].

<!--Image references-->

<!--Article references-->
[(Übersicht)]: ./sql-data-warehouse-tables-overview.md
[Datentypen]: ./sql-data-warehouse-tables-data-types.md
[Verteilen]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistik]: ./sql-data-warehouse-tables-statistics.md
[Temporäre]: ./sql-data-warehouse-tables-temporary.md
[Concurrency]: ./sql-data-warehouse-develop-concurrency.md
[CTAS]: ./sql-data-warehouse-develop-ctas.md
[Bewährte Methoden für SQL Datawarehouse]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->
[ALTER INDEX]: https://msdn.microsoft.com/library/ms188388.aspx
[Heap]: https://msdn.microsoft.com/library/hh213609.aspx
[gruppierte Indizes und nicht gruppierte Indizes]: https://msdn.microsoft.com/library/ms190457.aspx
[Die Syntax der Tabelle erstellen]: https://msdn.microsoft.com/library/mt203953.aspx
[Columnstore Indizes Defragmentierung]: https://msdn.microsoft.com/library/dn935013.aspx#Anchor_1
[gruppierte Columnstore Indizes]: https://msdn.microsoft.com/library/gg492088.aspx

<!--Other Web references-->

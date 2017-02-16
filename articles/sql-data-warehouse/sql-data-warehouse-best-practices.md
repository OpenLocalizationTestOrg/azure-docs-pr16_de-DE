<properties
   pageTitle="Bewährte Methoden für Azure SQL-Data Warehouse | Microsoft Azure"
   description="Empfehlungen sowie optimale Methoden, die Sie, wie Sie kennen sollten entwickeln Lösungen für Azure SQL-Data Warehouse. Diese hilft Ihnen erfolgreich durchgeführt werden."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/04/2016"
   ms.author="sonyama;barbkess"/>

# <a name="best-practices-for-azure-sql-data-warehouse"></a>Bewährte Methoden für Azure SQL-Data Warehouse

In diesem Artikel wird eine Zusammenstellung von vielen bewährte Methoden, mit denen Sie aus Ihrer Azure SQL-Data Warehouse optimale Leistung zu erzielen.  Einige der Konzepte in diesem Artikel werden grundlegende und einfach zu erläutern, andere Konzepte sind bessere und erweiterte Kratzer wir nur die Fläche in diesem Artikel.  Der Zweck dieses Artikels ist einige grundlegende Richtlinien zur Verfügung, und zum Auslösen Präsenz der wichtige Bereiche zu konzentrieren, wie Sie Ihr Datawarehouse erstellen.  Jeden Abschnitt führt Sie in ein Konzept, und zeigen Sie Sie dann auf ausführlichere Artikeln die des Konzepts ausführlicher behandelt.

Wenn Sie nur mit Azure SQL-Data Warehouse ersten Schritte sind, können Sie nicht in diesem Artikel Sie überlastet.  Die Reihenfolge der Themen ist hauptsächlich in der Reihenfolge der Wichtigkeit: hoch.  Wenn Sie nicht durch die Konzentration auf die erste einige Konzepte starten, werden Sie in einem guten Zustand sein.  Während Sie weitere vertraut und bequeme mit der Verwendung von SQL Datum Warehouse erhalten möchten, kehren Sie danach, und schauen Sie sich ein paar weitere Konzepte.  Dauert nicht lange für alles, um die Verständlichkeit zu.

## <a name="reduce-cost-with-pause-and-scale"></a>Mit anhalten und Dezimalstellen verringern

Ein der wichtigsten Features von SQL Data Warehouse ist die Möglichkeit zum Anhalten, wenn Sie es nicht verwenden der Abrechnung berechnen Ressourcen zu beenden.  Eine weitere wichtige Funktion ist die Möglichkeit zur Skalierung der Ressourcen.  Unterbrechen und Skalierung kann über das Azure-Portal oder über PowerShell-Befehlen vorgenommen werden.  Vertraut machen diese Features wie diese erheblich der Kosten des Datawarehouse verkürzen, wenn es nicht verwendet wird.  Wenn immer Ihr Datawarehouse zugegriffen werden soll, können Sie Sie sich auf die kleinste Größe, eine DW100 skalieren, sondern anhalten möchten.

Siehe auch [Anhalten Ressourcen zu berechnen][], [Lebenslauf Ressourcen zu berechnen][], [Skala berechnen Ressourcen][]


## <a name="drain-transactions-before-pausing-or-scaling"></a>Abzuleiten Sie Transaktionen vor dem Anhalten fortzusetzen oder Skalierung 

Beim Zeigen Sie oder Ihr SQL Data Warehouse skalieren, werden im Hintergrund Ihrer Abfragen abgebrochen beim Initiieren anhalten oder Anforderung skalieren.  Abbrechen einer einfachen Auswahlabfrage ist eine schnelle Vorgang und hat fast keine Auswirkung auf die Zeit, dass dauert Skalieren Ihrer Instanz oder anhalten.  Jedoch Transaktionen Abfragen, die die Daten oder die Struktur der Daten zu ändern, um schnell beenden möglicherweise nicht.  **Transaktionen Abfragen müssen per Definition, entweder im vollständig oder Zurücksetzen die Änderungen ausführen.**  Zurücksetzen, dass die Arbeit, die von einer Abfrage Transaktionen abgeschlossen ausführen kann, wie lange oder sogar länger als die ursprüngliche ändern Sie die Abfrage wurde anwenden.  Angenommen, wenn Sie eine Abfrage wurde Löschen von Zeilen und weist bereits für eine Stunde ausgeführt wurde Abbrechen, konnte es System pro Stunde auf Hintergrund einfügen die Zeilen dauern, die gelöscht wurden.  Wenn Sie anhalten ausführen oder Skalierung während Transaktionen in Flight, Ihre anhalten sind oder Skalierung scheint zu sehr lange dauern, da anhalten und dieselbe Skalierung auf warten muss für das Zurücksetzen ausführen, bevor sie fortfahren kann.

Siehe auch [Grundlegendes zu Transaktionen][], [Optimieren Transaktionen.][]

## <a name="maintain-statistics"></a>Verwalten von Statistiken

Im Gegensatz zu SQL Server, die automatisch erkannt und erstellt oder aktualisiert die Statistik für Spalten, erfordert SQL Data Warehouse manuelle Wartung der Statistik.  Während wir dies in der Zukunft ändern möchten, sollten Sie jetzt Verwalten Ihrer Statistik, um sicherzustellen, dass die SQL Data Warehouse Pläne optimiert sind.  Der Optimizer erstellte Pläne sind nur so gut wie die Statistiken zur Verfügung.  **Aufgenommene Statistik für jede Spalte erstellen ist eine einfache Möglichkeit, den ersten Schritten mit Statistik.**  Es ist ebenso wichtig, Statistiken wie, wesentlichen Änderungen mit Ihren Daten geschehen zu aktualisieren.  Ein konservativen Ansatz möglicherweise die Statistik täglich oder nach jedem Laden aktualisieren.  Es gibt immer Kompromisse zwischen Leistung sowie die Kosten für Statistiken erstellen und aktualisieren. Wenn Sie, dass es dauert zu lange feststellen, alle Ihre Statistiken verwalten, möchten Sie möglicherweise versuchen, vorzugehen und über die Spalten Statistiken aufweisen oder welche Spalten häufig aktualisiert werden müssen.  Sie möchten beispielsweise Datumsspalten, aktualisieren, in dem neue Werte hinzugefügt, tägliche möglicherweise wird. **Statistik auf die Spalten in Verknüpfungen, in der WHERE-Klausel verwendeten Spalten und Spalten in GROUP BY gefunden haben, erhalten ein den größten Nutzen.**

Siehe auch [Verwalten Tabellenstatistiken][], [Statistiken erstellen][], [UPDATE STATISTICS][]

## <a name="group-insert-statements-into-batches"></a>Gruppieren einfügen Anweisungen in Stapeln

Eine einmalige Last an eine kleine Tabelle mit einer INSERT-Anweisung oder sogar ein periodisch Laden der eine Suche möglicherweise für Ihre Bedürfnisse mit einer Anweisung wie problemlos ausführen `INSERT INTO MyLookup VALUES (1, 'Type 1')`.  Jedoch wenn Sie Tausende oder Millionen von Zeilen im Verlauf des Tages laden müssen, können Sie feststellen, dass einzelne fügt nur aufrechterhalten können nicht.  Entwickeln Sie stattdessen Ihre Prozesse, damit diese in eine Datei schreiben und einem anderen Prozess verwendet regelmäßig entlang stammt, und Laden Sie diese Datei.

Siehe auch [Einfügen][]
 
## <a name="use-polybase-to-load-and-export-data-quickly"></a>Verwenden Sie zum Laden und Exportieren von Daten schnell PolyBase

SQL Data Warehouse unterstützt das Laden und Exportieren von Daten über verschiedene Tools, einschließlich Azure Data Factory, PolyBase und BCP.  Für kleine Datenmengen, an dem Leistung kritische nicht, möglicherweise alle Tool für Ihren Anforderungen ausreichend.  Ist jedoch beim Laden oder exportieren Sie große Datenmengen oder schnelle Leistung erforderlich ist, PolyBase die beste Option dar.  PolyBase dient zur Nutzung der Architektur MPP (Massively parallele Processing) des SQL-Data Warehouse wird daher laden und Daten Werte schneller als ein anderes Programm exportieren.  PolyBase lädt können mithilfe von CTAS oder INSERT INTO ausgeführt werden.  **Mit CTAS wird der Protokollierung von Transaktionen und die schnellste Möglichkeit zum Laden Ihre Daten minimieren.**  Azure Data Factory unterstützt auch PolyBase lädt.  PolyBase unterstützt verschiedene Dateiformate einschließlich Gzip-Dateien.  **Um liegenden bei Verwendung von Gzip Textdateien, teilen Sie auf, Dateien in 60 oder mehrere Dateien zum Maximieren Parallelism von Ihrem laden.**  Erwägen Sie für schneller Gesamtdurchsatz gleichzeitig Laden von Daten aus.

Siehe auch [Daten laden][], [Leitfaden für die Verwendung von PolyBase][], [Azure SQL-Data Warehouse Laden von Mustern und Strategien][], [Daten mit Azure Data Factory laden][], [Verschieben von Daten mit Azure Data Factory][], [Externe DATEIFORMAT erstellen][], [erstellt eine Tabelle auswählen (CTAS)][]

## <a name="load-then-query-external-tables"></a>Laden, und klicken Sie dann externe Tabellen Abfragen

Während Polybase, auch bekannt als externen Tabellen, die schnellste Möglichkeit zum Laden von Daten werden kann, ist es nicht optimal für Abfragen. SQL Data Warehouse Polybase Tabellen unterstützen derzeit nur Azure BLOB-Dateien. Diese Dateien haben keine Ressourcen sichern sie berechnen.  Daher SQL Data Warehouse kann nicht diese arbeiten Auslagern und daher müssen Lesen Sie die gesamte Datei durch tempdb laden, um die Daten zu lesen.  Daher, wenn Sie mehrere Abfragen, die diese Daten Abfragen werden wird verfügen, empfiehlt sich diese Daten einmal laden und Abfragen, die die lokale Tabelle verwendet haben.

Siehe auch [Leitfaden für die Verwendung von PolyBase][]


## <a name="hash-distribute-large-tables"></a>Hash Verteilen von großen Tabellen

Standardmäßig werden die Tabellen Round Robert verteilt.  Dies erleichtert Benutzer Schritte beim Erstellen von Tabellen, ohne zu entscheiden, wie ihre Tabellen verteilt werden.  Round Robert Tabellen möglicherweise für einige Auslastung ausreichend ausführen, aber in den meisten Fällen Auswählen einer Verteilung wird Spalte viel besser ausgeführt.  Wenn eine Tabelle nach einer Spalte verteilt weit eine Funktion RUNDEN Robert Tabelle sind die am häufigsten verwendeten Beispiel für ist, wenn zwei großen Faktentabellen verknüpft werden.  Wenn Sie über die Bestellungstabelle, die durch Order_id verteilt ist, und eine Transaktionstabelle, die auch nach Order_id, verteilt wird, wenn Sie die Tabelle Orders Ihrer Tabelle Transaktionen auf Order_id teilnehmen, wird diese Abfrage beispielsweise einer Pass-Through-Abfrage, was bedeutet, dass wir Bewegung Datenvorgänge zu unterdrücken.  Weniger Schritte bedeutet, dass eine schnellere Abfrage.  Weniger Daten Bewegung wird auch für schnellere Abfragen.  Diese Erläuterung ausführlichen einfach. Beim Laden einer Tabelle verteilten, werden Sie sicher, dass Ihre eingehenden Daten wie folgt Ihrer lädt verlangsamen wird nicht auf die zurück-Taste sortiert werden.  Finden Sie unter den links unten für viele weitere Details wie Auswählen einer Spalteninhalts Verteilung kennen und wie zum Definieren einer verteilten Tabelle in der WITH-Klausel Ihrer Abrechnung Tabellen erstellen Leistung kann.

Siehe auch [Tabelle Übersicht][], [Verteilung der Tabelle][], [die Tabelle Verteilung durch das auswählen][], [Tabelle erstellen][], [CREATE TABLE AS SELECT][]

## <a name="do-not-over-partition"></a>Führen Sie nicht zu viel partitionieren

Während der Datenpartitionierung kann sehr wirksam sein für die Verwaltung, dass Ihre Abfragen von Daten über Partition wechseln oder eine Optimierung scannt nach mit beseitigen, Probleme zu viele Partitionen verlangsamen können.  Oft eine hohe Genauigkeit Partitionierungsstrategie was gut auf SQL Server möglicherweise funktioniert möglicherweise nicht gut auf SQL Data Warehouse.  Zu viele Partitionen Probleme kann auch die Effektivität der gruppierten Columnstore Indizes reduzieren, wenn Sie weniger als 1 Millionen Zeilen besitzt.  Beachten Sie, dass im Hintergrund SQL Data Warehouse Ihre Daten für Sie in 60-Datenbanken teilt den, wenn Sie eine Tabelle mit 100 Partitionen erstellen, dies tatsächlich 6000 Partitionen ergibt.  Am besten ist so experimentieren Sie mit Verteilung um anzuzeigen, was für Ihre Arbeitsbelastung am besten geeignet ist jede Arbeitsbelastung unterschiedlich.  Erwägen Sie die unteren Abstufung, was möglicherweise für Sie in SQL Server gearbeitet haben.  Betrachten Sie beispielsweise die wöchentliche oder monatliche Partitionen statt tägliche Partitionen verwenden.

Siehe auch [Tabellenpartitionierung][]

## <a name="minimize-transaction-sizes"></a>Minimieren Transaktion Größen

Einfügen, aktualisieren und löschen Anweisungen ausführen, und wenn sie sich nicht in einer Transaktion er müssen rückgängig gemacht werden.  Minimieren Sie, um das Risiko, dass eine lange zurücksetzen zu minimieren, Transaktion Größen, wann immer möglich.  Dies kann durch Dividieren einfügen, aktualisieren und Löschen von Anweisungen in Teile erfolgen.  Beispielsweise, wenn Sie eine INSERT, die Stunde in Anspruch nehmen Sie erwarten verfügen, falls möglich Teilen Sie auf, das Einfügen in 4 Teile, die jeweils in 15 Minuten ausgeführt werden.  Nutzen Sie Inhalte minimale Protokollierung Fällen wie CTAS, ABSCHNEIDEN, DROP TABLE oder einfügen aus, um leere Tabellen zum Verringern des Risikos zurücksetzen.  Eine weitere Möglichkeit zum Unterdrücken entwurfsbearbeitung besteht darin, nur Metadaten Operationen wie Partition Wechsel für die datenverwaltung verwenden.  Beispielsweise lieber als Ausführen eine DELETE-Anweisung, um alle Zeilen in einer Tabelle zu löschen, in dem die Order_date im Oktober 2001 wurde, konnte Partitionierung Ihrer Daten monatlich und wechseln Sie dann, die Partition mit Daten für eine leere Partition aus einer anderen Tabelle (Siehe ALTER TABLE Beispiele).  Erwägen Sie zur unpartitionierten Tabellen mithilfe einer CTAS zum Schreiben von Daten in einer Tabelle beibehalten möchten, anstelle löschen.  Wenn eine CTAS die gleiche Zeitspanne dauert, handelt es sich um eine viel sicherer Operation ausführen, wobei die Protokollierung von minimal Transaktionen und können schnell abgebrochen, falls erforderlich.

Siehe auch [Grundlegendes zu Transaktionen][], [Optimieren Transaktionen][], [Tabellenpartitionierung][], [Tabelle ABSCHNEIDEN][], [ALTER TABLE][]und [erstellt eine Tabelle auswählen (CTAS)][]

## <a name="use-the-smallest-possible-column-size"></a>Verwenden Sie die Größe der kleinste möglichen Spalte

Wenn Sie Ihre DDL definieren, werden mit den kleinsten Datentyp mit Unterstützung für Ihre Daten Abfrage Leistung.  Dies ist besonders wichtig für Zeichen und VARCHAR-Spalten.  Wenn der längste Wert in einer Spalte 25 Zeichen ist, können definieren Sie Ihre Spalte wie VARCHAR(25).  Vermeiden Sie alle Zeichenspalten zu einem großen standardmäßige Länge definieren.  Darüber hinaus definieren Sie Spalten als VARCHAR, wenn dies alles ist, die erforderlich ist, anstatt verwenden Sie NVARCHAR.

Siehe auch [Tabelle Übersicht][], [Tabelle Datentypen][]und [Tabelle erstellen][]

## <a name="use-temporary-heap-tables-for-transient-data"></a>Verwenden Sie temporäre Heaptabellen für vorübergehende Daten

Wenn Sie Daten auf SQL Data Warehouse vorübergehend Start sind, kann es passieren, dass mithilfe einer Tabelle Heap gesamten Vorgang schneller vornehmen.  Wenn Sie Daten nur, wenn sie vor dem Ausführen von weitere Transformationen Phaseneigenschaften laden, werden Laden der Tabelle Heap Tabelle sehr viel schneller, die Daten zu einer Tabelle gruppierte Columnstore geladen.  Darüber hinaus werden Laden von Daten in eine temporäre Tabelle auch schneller als das Laden einer Tabelle dauerhaft zu laden.  Temporäre Tabellen mit einer "#" beginnen und nur von der Sitzung die, erstellt, sodass sie nur in nur in bestimmten Szenarien funktionieren möglicherweise, zugegriffen werden.   Heaptabellen werden in der WITH-Klausel einer Tabelle erstellen definiert.  Wenn Sie eine temporäre Tabelle verwenden, denken Sie daran, die Statistiken für die temporäre Tabelle zu erstellen.

Siehe auch [temporäre Tabellen][], [Tabelle erstellen][], [CREATE TABLE AS SELECT][]

## <a name="optimize-clustered-columnstore-tables"></a>Gruppierte Columnstore Tabellen optimieren

Gruppierte Columnstore Indizes sind eine der effizientesten weisen, die Sie Ihre Daten in Azure SQL-Data Warehouse speichern können.  Standardmäßig werden die Tabellen in SQL Data Warehouse als gruppierte ColumnStore erstellt.  Tabellen, Probleme gute Segment Qualität ist wichtig, um die optimale Leistung für Abfragen zu Columnstore zu erhalten.  Wenn Zeilen in Columnstore Tabellen unter Arbeitsspeicherdruck geschrieben werden, beeinträchtigt Columnstore Segment Qualität.  Segment Qualität kann durch die Anzahl der Zeilen in einer Gruppe der komprimierten Zeile gemessen werden.  Finden Sie unter der [beeinträchtigt Columnstore Index Qualität bewirkt, dass][] in der [Tabellenindizes][] Artikel Schritt-für-Schritt-Anleitung zum Erkennen und Verbessern der Segment Qualität für gruppierte Columnstore Tabellen.  Da hoher Qualität Columnstore Segmente ist wichtig, ist es eine gute Idee, Benutzer-Ids verwenden, die in der Ressourcenklasse Medium oder umfangreiche für die Daten geladen sind.  Die weniger DWUs Sie verwenden, größere Ressourcenklasse, die Sie bei Ihrer Benutzer laden zuweisen möchten. 

Da Columnstore Tabellen im Allgemeinen Sie wird nicht Daten in einer komprimierten Columnstore Segment schieben bis maximal 1 Millionen Zeilen pro Tabelle vorhanden sind, und jede Tabelle SQL Data Warehouse in 60 Tabellen als Faustregel aufgeteilt, nutzen nicht Columnstore Tabellen eine Abfrage, es sei denn, die Tabelle mit mehr als 60 Millionen Zeilen enthält.  Für die Tabelle mit weniger als 60 Millionen Zeilen kann es keinen Index Columnstore verfügen sinnvoll.  Es kann auch nicht Schaden.  Darüber hinaus, wenn Sie Ihre Daten aufteilen, dann sollten Sie berücksichtigen, dass jede Partition 1 Millionen von Zeilen aus einem gruppierten Columnstore Index profitieren sein muss.  Wenn eine Tabelle 100 Partitionen verfügt, müssen sie mindestens 6 Milliarden Zeilen aus einem gruppierten Spalten Speicher (60 Verteilung *100 Partitionen* 1 Millionen Zeilen) zu profitieren haben.  Wenn die Tabelle 6 Milliarden Zeilen nicht in diesem Beispiel verfügt, verringern Sie die Anzahl der Partitionen oder verwenden Sie eine Tabelle Heap stattdessen.  Es kann auch im Wert experimentieren, um herauszufinden, ob eine bessere Leistung kann, mit einer Tabelle Heap mit sekundären Indizes statt einer Tabelle Columnstore gesteigert werden sein.  Sekundäre Indizes unterstützt Columnstore Tabellen nicht noch nicht.

Bei der Abfrage einer Tabelle Columnstore werden Abfragen schneller ausgeführt, wenn Sie nur die Spalten auswählen, die Sie benötigen.  

Siehe auch [Tabellenindizes][], [Columnstore Indizes Leitfaden][]und [Rebuilding Columnstore Indizes][]

## <a name="use-larger-resource-class-to-improve-query-performance"></a>Verwenden Sie größere Ressourcenklasse für optimale Leistung der Abfrage

SQL Data Warehouse verwendet Ressourcengruppen als eine Möglichkeit, Speicher Abfragen zuweisen.  Außerhalb des im Feld werden alle Benutzer auf die kleine Klasse zugewiesen, die von 100 MB Speicher pro Verteilung gewährt.  Da es gibt immer 60 Verteilung und jede Verteilung wird mindestens 100 MB, Breite der gesamten arbeitsspeicherzuteilung 6.000 MB oder nur unter 6 GB ist System angegeben.  Bestimmte Abfragen, wie groß Verknüpfungen oder lädt mit gruppierten Columnstore Tabellen, profitieren von größeren Arbeitsspeicher Zuweisungen.  Einige Abfragen, wie reines scannt, werden keine Vorteile angezeigt.  Klicken Sie auf der Seite spiegeln wirkt sich auf größere Ressourcenklassen Nutzung gleichzeitiger Zugriff, damit dies berücksichtigen, bevor Sie alle Benutzer in einer großen Ressourcenklasse verschieben möchten.
 
Siehe auch [Parallelität und Arbeitsbelastung management][]

## <a name="use-smaller-resource-class-to-increase-concurrency"></a>Verwenden Sie kleinere Ressourcenklasse Parallelität erhöhen

Wenn Sie feststellen, dass Benutzerabfragen scheint eine Verzögerung haben, könnte es, dass die Benutzer in größeren Ressourcenklassen ausführen und viele Parallelität Steckplätze bewirken, dass andere Abfragen an eine Warteschlange genutzt werden.  Um herauszufinden ob in der Warteschlange Benutzer Abfragen, ausführen `SELECT * FROM sys.dm_pdw_waits` um festzustellen, ob Zeilen zurückgegeben werden.

Siehe auch [Parallelität und Arbeitsbelastung Management][] [sys.dm_pdw_waits][]

## <a name="use-dmvs-to-monitor-and-optimize-your-queries"></a>Verwenden von DMVs überwachen und Optimieren Ihrer Abfragen

SQL Data Warehouse verfügt über mehrere DMVs zum Überwachen der Ausführung der Abfrage verwendet werden können.  Im folgenden Überwachung Artikel durchläuft eine schrittweise Anleitung erfahren Sie, wie die Details einer laufenden Abfrage betrachten.  Um Abfragen in diesen DMVs schnell zu finden, kann die Option Beschriftung mit Ihrer Abfragen mit helfen.

Siehe auch [Ihrer mit DMVs Arbeitsbelastung überwachen][], [Beschriftung][], [OPTION][], [sys.dm_exec_sessions][], [sys.dm_pdw_exec_requests][], [sys.dm_pdw_request_steps][], [sys.dm_pdw_sql_requests][], [sys.dm_pdw_dms_workers], [DBCC PDW_SHOWEXECUTIONPLAN][], [sys.dm_pdw_waits][]

## <a name="other-resources"></a>Weitere Ressourcen

Auch finden Sie im Artikel " [Problembehandlung][] " für häufige Probleme und Lösungen ein.

Wenn Sie gefunden haben, was Sie in diesem Artikel gesucht haben, versuchen Sie die "Suche für Dokumente" auf der linken Seite von dieser Seite, um alle Dokumente Azure SQL-Data Warehouse zu suchen.  Das [Azure SQL-Datawarehouse MSDN-Forum][] wurde als einen Ort für Ihre Fragen an andere Benutzer und der SQL Data Warehouse Produkt Gruppe erstellen.  Wir aktiv zu diesem Forum, um sicherzustellen, dass Ihre Fragen, entweder von einem anderen Benutzer oder einer von uns beantwortet werden überwachen.  Wenn Sie es vorziehen, Ihre Fragen auf Stapelüberlauf, haben wir auch einer [Azure SQL Data Warehouse Stapel Überlauf Forum][].

Schließlich Bitte verwenden Sie die Seite [Azure SQL Data Warehouse Feedback][] Feature anzufordern.  Hinzufügen Ihrer Anfragen oder andere Anforderungen der oben Abstimmung hilft wirklich uns die Features zu priorisieren.

<!--Image references-->

<!--Article references-->
[Create a support ticket]: ./sql-data-warehouse-get-started-create-support-ticket.md
[Parallelität und Arbeitsbelastung management]: ./sql-data-warehouse-develop-concurrency.md
[Erstellen Sie die Tabelle auswählen (CTAS)]: ./sql-data-warehouse-develop-ctas.md
[Tabelle (Übersicht)]: ./sql-data-warehouse-tables-overview.md
[Tabelle-Datentypen]: ./sql-data-warehouse-tables-data-types.md
[Tabelle Verteilung]: ./sql-data-warehouse-tables-distribute.md
[Tabellenindizes]: ./sql-data-warehouse-tables-index.md
[Ursachen für schlechte Columnstore Index Qualität]: ./sql-data-warehouse-tables-index.md#causes-of-poor-columnstore-index-quality
[Neuerstellen von Indizes columnstore]: ./sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[Tabellenpartitionierung]: ./sql-data-warehouse-tables-partition.md
[Tabellenstatistiken verwalten]: ./sql-data-warehouse-tables-statistics.md
[Temporäre Tabellen]: ./sql-data-warehouse-tables-temporary.md
[Leitfaden für die Verwendung von PolyBase]: ./sql-data-warehouse-load-polybase-guide.md
[Laden von Daten]: ./sql-data-warehouse-overview-load.md
[Verschieben von Daten mit Azure Data Factory]: ../data-factory/data-factory-azure-sql-data-warehouse-connector.md
[Laden Sie die Daten mit Azure Data Factory]: ./sql-data-warehouse-get-started-load-with-azure-data-factory.md
[Load data with bcp]: ./sql-data-warehouse-load-with-bcp.md
[Load data with PolyBase]: ./sql-data-warehouse-get-started-load-with-polybase.md
[Überwachen Sie Ihre Arbeitsbelastung DMVs verwenden]: ./sql-data-warehouse-manage-monitor.md
[Anhalten berechnen Ressourcen]: ./sql-data-warehouse-manage-compute-overview.md#pause-compute-bk
[Lebenslauf berechnen Ressourcen]: ./sql-data-warehouse-manage-compute-overview.md#resume-compute-bk
[Maßstab berechnen Ressourcen]: ./sql-data-warehouse-manage-compute-overview.md#scale-performance-bk
[Grundlegendes zu Transaktionen.]: ./sql-data-warehouse-develop-transactions.md
[Optimieren der Transaktionen.]: ./sql-data-warehouse-develop-best-practices-transactions.md
[Behandlung von Problemen]: ./sql-data-warehouse-troubleshoot.md
[BESCHRIFTUNG]: ./sql-data-warehouse-develop-label.md

<!--MSDN references-->
[TABELLE ÄNDERN]: https://msdn.microsoft.com/library/ms190273.aspx
[ERSTELLEN VON EXTERNEN-DATEIFORMAT]: https://msdn.microsoft.com/library/dn935026.aspx
[ERSTELLEN VON STATISTIKEN]: https://msdn.microsoft.com/library/ms188038.aspx
[TABELLE ERSTELLEN]: https://msdn.microsoft.com/library/mt203953.aspx
[ERSTELLEN, WÄHLEN SIE TABELLE AS]: https://msdn.microsoft.com/library/mt204041.aspx
[DBCC PDW_SHOWEXECUTIONPLAN]: https://msdn.microsoft.com/library/mt204017.aspx
[EINFÜGEN]: https://msdn.microsoft.com/library/ms174335.aspx
[OPTION]: https://msdn.microsoft.com/library/ms190322.aspx
[TABELLE VERKÜRZEN]: https://msdn.microsoft.com/library/ms177570.aspx
[AKTUALISIEREN VON STATISTIKEN]: https://msdn.microsoft.com/library/ms187348.aspx
[Sys.dm_exec_sessions]: https://msdn.microsoft.com/library/ms176013.aspx
[Sys.dm_pdw_exec_requests]: https://msdn.microsoft.com/library/mt203887.aspx
[Sys.dm_pdw_request_steps]: https://msdn.microsoft.com/library/mt203913.aspx
[Sys.dm_pdw_sql_requests]: https://msdn.microsoft.com/library/mt203889.aspx
[Sys.dm_pdw_dms_workers]: https://msdn.microsoft.com/library/mt203878.aspx
[Sys.dm_pdw_waits]: https://msdn.microsoft.com/library/mt203893.aspx
[Leitfaden Columnstore Indizes]: https://msdn.microsoft.com/library/gg492088.aspx

<!--Other Web references-->
[Auswählen von Tabelle Verteilung]: https://blogs.msdn.microsoft.com/sqlcat/2015/08/11/choosing-hash-distributed-table-vs-round-robin-distributed-table-in-azure-sql-dw-service/
[SQL Azure Datawarehouse Feedback]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[SQL Azure Datawarehouse MSDN-Forum]: https://social.msdn.microsoft.com/Forums/sqlserver/home?forum=AzureSQLDataWarehouse
[SQL Azure Datawarehouse Stapel Überlauf-Forum]:  http://stackoverflow.com/questions/tagged/azure-sqldw
[Azure SQL Data Warehouse Laden von Mustern und Strategien]: https://blogs.msdn.microsoft.com/sqlcat/2016/02/06/azure-sql-data-warehouse-loading-patterns-and-strategies

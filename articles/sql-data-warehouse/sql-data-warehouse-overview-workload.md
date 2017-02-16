<properties
   pageTitle="Datawarehouse Arbeitsbelastung"
   description="SQL Data Warehouse Elastizität können Sie vergrößern, verkleinern oder Anhalten berechnen Power mithilfe einer gleitenden Skala Datawarehouse Einheiten (DWUs). In diesem Artikel wird erläutert, die Datawarehouse Metrik und deren Bezug auf DWUs. "
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="07/25/2016"
   ms.author="barbkess;mausher;jrj;sonyama"/>


# <a name="data-warehouse-workload"></a>Datawarehouse Arbeitsbelastung
Ein Datawarehouse Arbeitsbelastung bezieht sich auf alle Vorgänge, die für eine Datawarehouse ablaufen müssen. Die Datawarehouse Arbeitsbelastung Steuerelementgruppe den gesamten Prozess Laden von Daten in das Warehouse, Durchführen von Analysen und Berichte über das Datawarehouse, Verwalten von Daten in das Datawarehouse und Exportieren von Daten aus dem Datawarehouse ein. Tiefe und Breite dieser Komponenten, ist häufig die Fälligkeit Ebene des Datawarehouse entsprechen.


## <a name="new-to-data-warehousing"></a>Neu bei Datawarehousing?
Ein Datawarehouse ist eine Sammlung von Daten, die aus einer oder mehreren Datenquellen geladen werden und werden verwendet, um Business Intelligence-Aufgaben wie reporting und Datenanalyse durchführen.

Daten Lager sind durch Abfragen gekennzeichnet, die größere Anzahl von Zeilen, große Datenbereiche Scannen und möglicherweise relativ großen Ergebnisse zurück, für die Zwecke von Analysen und Berichte. Daten Lager sind auch relativ große Datenmengen im Vergleich zu klein-Transaktion fügt/Updates/löscht erkennbar.

- Ein Datawarehouse führt am besten, wenn die Daten in einer Weise gespeichert werden, die beim Optimieren von Abfragen, die die großen Anzahl von Zeilen oder große Datenbereiche scannen müssen. Diese Art von Scannen funktioniert am besten, wenn die Daten gespeichert und statt von Zeilen nach Spalten, durchsucht werden.

>[AZURE.NOTE] Der in-Memory-Columnstore Index, der Spalte Speicher verwendet, bietet bis zu 10 x Komprimierung Gewinne und 100 x Abfrage Leistungsgewinne über herkömmliche binäre Strukturen für die Berichterstattung und Analytics Abfragen. Wir berücksichtigen Columnstore Indizes als Standard für das Speichern und Scannen große Datenmengen in einem Datawarehouse.

- Ein Datawarehouse weist verschiedene Punkte, die als ein System, die für online-Transaktionen processing (OLTP) optimiert. OLTP-System enthält viele einfügen, aktualisieren und Löschen von Vorgängen. Diese Suchvorgänge auf bestimmte Zeilen in der Tabelle. Sucht Tabelle ausführen am besten, wenn die Daten in einer Zeile für Zeile Weise gespeichert werden. Die Daten sortiert werden können und schnell mit einer Division durchsucht und gewinnen Ansatz eine binäre Struktur oder Btree Suche bezeichnet.


## <a name="data-loading"></a>Das Laden von Daten
Das Laden von Daten ist ein wichtiger Teil der Datawarehouse Arbeitsbelastung an. Unternehmen müssen in der Regel ein beschäftigt OLTP-System, die sich während des Tages Änderungen nachverfolgt beim Kunden geschäftliche Transaktionen generieren. Häufig bei Nacht auf während eines Wartungszeitfensters die Transaktionen regelmäßig, verschoben oder kopiert in das Datawarehouse. Sobald die Daten im Datawarehouse befinden, können Sie Analysten Datenanalysen und treffen von Entscheidungen auf Business auf der Registerkarte Daten.

- In der Vergangenheit heißt die Vorgehensweise zum Laden ETL für extrahieren, Transformieren und laden. Daten müssen in der Regel umgewandelt werden, damit es mit anderen Daten in das Datawarehouse konsistent ist. Vorher verwendet Unternehmen dedizierte ETL-Servern, die Transformationen durchführen. Nun mit dieser schnelles parallele Verarbeitung können Daten zunächst in SQL Data Warehouse laden, und führen Sie dann die Transformationen. Dieses Verfahren heißt extrahieren, laden und Transformieren (ELT), und Sie wird immer ein neuer Standard für die Datawarehouse Arbeitsbelastung.

> [AZURE.NOTE] Mit SQL Server 2016: Sie können jetzt ausführen Analytics in Echtzeit auf einer OLTP-Tabelle. Dies ist kein Ersatz für die Notwendigkeit ein Datawarehouse zu speichern und Analysieren von Daten, aber es bietet eine Möglichkeit zum kompliziertere in Echtzeit.

### <a name="reporting-and-analysis-queries"></a>Berichts- und Abfragen
Berichts- und Abfragen werden häufig in klein, Mittel kategorisiert und großen basierend auf einer Reihe von Kriterien, aber es ist in der Regel basierend auf Zeit. In den meisten Daten Lager gibt es eine gemischte Arbeitsbelastung von Fast-Ausführung im Vergleich zu langer Abfragen. In jedem Fall, es ist wichtig, die Farbkombination ermitteln und feststellen, wie oft seine (stündlich, täglich, Monatsende, Ende Quartals usw.). Es ist wichtig zu verstehen, dass die Arbeitsbelastung gemischte Abfrage in Verbindung mit gleichzeitiger Zugriff, zum richtigen Kapazität, Planung für ein Datawarehouse Lead.

- Planen von Kapazität kann eine komplexe Aufgabe für einen Arbeitsbelastung gemischte Abfrage sein, besonders, wenn Sie nacheinander lange Lead Datawarehouse Kapazität hinzuzufügende benötigen. SQL Data Warehouse entfernt die Dringlichkeit der Planung von Kapazität, da können Sie vergrößern und verkleinern Sie berechnen Kapazität aus, zu einem beliebigen Zeitpunkt und seit Speicher und berechnen Kapazität werden unabhängig voneinander in der Größe angepasst.

### <a name="data-management"></a>Datenverwaltung
Verwalten von Daten ist wichtig, besonders, wenn Sie wissen, dass Sie in Kürze nicht genügend Speicherplatz ausführen können. Daten Lager unterteilen die Daten in der Regel in aussagekräftige Bereiche, die als Partitionen in einer Tabelle gespeichert werden. Alle SQL Server-basierte Produkte ermöglichen Ihnen die Partitionen und verkleinerte Anzeige der Tabelle zu verschieben. Diese Berechtigung ermöglicht wechseln Partition verschieben ältere Daten auf weniger teure Speicher und lassen Sie die neuesten Daten verfügbar auf online-Speicher.

- Columnstore Indizes unterstützen partitionierte Tabellen. Für Columnstore Indizes dienen partitionierte Tabellen für die Verwaltung und Archivierung. Für Tabellen gespeicherte Zeile für Zeile müssen Partitionen eine größere Rolle Leistungsabfall Abfrage an.  

- PolyBase spielt eine wichtige Rolle bei der Verwaltung von Daten. Verwenden PolyBase, müssen Sie die Option zum Archivieren älterer Daten auf Hadoop oder Azure Blob-Speicher.  Auf diese Weise zahlreiche Optionen, da die Daten weiterhin online ist.  Möglicherweise dauert länger zum Abrufen von Daten aus Hadoop, aber die Abwägung Abruf Zeit möglicherweise die Speicherkosten übersteigen.

### <a name="exporting-data"></a>Exportieren von Daten
Eine Methode, um die Daten für Berichte und Analysen zur Verfügung stellen besteht darin Daten aus dem Datawarehouse an Servern zum Ausführen von Berichten und Analyse dedizierten senden. Diese Server werden Datamarts bezeichnet. Beispielsweise könnten Sie vorab verarbeiten Berichtsdaten und dann exportieren aus dem Datawarehouse zu viele Server auf der ganzen Welt für Kunden und Analysten gestreut verfügbar machen.

- Berichte zu erstellen, könnten Sie jede Nacht schreibgeschützt Berichtsserver mit einer Momentaufnahme der täglichen Daten auffüllen. Dies ermöglicht höhere Bandbreite für Kunden und zugleich die Anforderungen berechnen Ressource im Data Warehouse. In ein Aspekt Sicherheit ermöglichen Datamarts Sie zum Verringern der Anzahl der Benutzer, die Zugriff auf das Datawarehouse haben.
- Für Analytics können entweder einen Analysis-Cube auf das Datawarehouse erstellen und führen Sie die Analyse für Datawarehouse, oder Daten vor der Verarbeitung und zum für weitere Analytics Analysis-Server zu exportieren.

## <a name="next-steps"></a>Nächste Schritte
Jetzt, da Sie eine kurze Anmerkung SQL Data Warehouse kennen, erfahren Sie, wie Sie schnell [ein SQL Data Warehouse erstellen][] , und [Laden Sie die Beispieldaten][].

<!--Image references-->

<!--Article references-->
[Laden von Beispieldaten]: ./sql-data-warehouse-load-sample-databases.md
[Erstellen einer SQL Data Warehouse]: ./sql-data-warehouse-get-started-provision.md

<!--MSDN references-->

<!--Other web references-->

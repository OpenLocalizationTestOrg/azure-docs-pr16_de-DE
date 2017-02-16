<properties
   pageTitle="Verteilt Daten und Tabellenoptionen für die Massively parallele Verarbeitung (MPP) Systeme SQL Data Warehouse und Parallel Data Warehouse | Microsoft Azure"
   description="Erfahren Sie, wie Daten für Massively parallele Verarbeitung (MPP) und die Optionen zum Verteilen von Tabellen in Azure SQL-Data Warehouse und Parallel Data Warehouse veröffentlicht wurde."
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
   ms.date="10/10/2016"
   ms.author="barbkess"/>


# <a name="distributed-data-and-distributed-tables-for-massively-parallel-processing-mpp"></a>Verteilte Daten und verteilte Tabellen für Massively parallele Verarbeitung (MPP)

Erfahren Sie, wie Benutzerdaten in Azure SQL-Data Warehouse und Parallel Data Warehouse, die Microsoft Massively parallele Verarbeitung (MPP) Systeme sind verteilt ist. Entwerfen Ihr Datawarehouse verteilte Daten effektiv verwenden, können Sie Vorteile der MPP-Architektur Verarbeitung die Abfrage zu erzielen. Einige Datenbank Designoptionen können sich zur Verbesserung der abfrageleistung zur signifikante auswirken.  

>[AZURE.NOTE] Azure SQL-Data Warehouse und Parallel Data Warehouse verwenden das gleiche Massively parallele Verarbeitung (MPP) Design, aber einige Unterschiede aufgrund der zugrunde liegenden Plattform haben. SQL Data Warehouse ist eine Plattform als Service (PaaS), die für Azure ausgeführt wird. Parallel Data Warehouse ausgeführt wird, auf Analytics Plattform System (Apps) also eine lokale Einheit, die auf Windows Server ausgeführt wird.

## <a name="what-is-distributed-data"></a>Was ist die verteilte Daten?

Bezieht sich in SQL Data Warehouse und Parallel Data Warehouse verteilte Daten auf Benutzerdaten, die gesamten System MPP an mehreren Speicherorten gespeichert ist. Jeder der beiden Speicherorte fungiert als eine unabhängige Speicherung und Verarbeitung Einheiten, die Abfragen für einen bestimmten Anteil der Daten ausgeführt wird. Verteilte Daten ist jedoch grundsätzliche zum Ausführen von Abfragen in Parallel zur hohe abfrageleistung zu erzielen.

Zum Verteilen von Daten, weist das Datawarehouse jede Zeile einer Tabelle Benutzer einer verteilten Position ein.  Sie können Tabellen mit einem Hash-Verteilung Abschreibung oder eines Round-Robert verteilen. Die Verteilungsmethode wird in der CREATE TABLE-Anweisung angegeben. 

## <a name="hash-distributed-tables"></a>Hash verteilt Tabellen
  
Das folgende Diagramm veranschaulicht, wie eine vollständige (Tabelle nicht verteilten) als Tabelle Hash verteilt gespeichert wird. Eine deterministische Funktion weist jede Zeile eine Verteilung gehören soll. In der Definition der Tabelle ist eine der Spalten als der Spalte Verteilung festgelegt. Die Hash-Funktion verwendet den Wert in der Spalte Verteilung jede Zeile einer Verteilung zuweisen.

Es gibt Leistungsaspekte für die Auswahl einer Spalte Verteilung wie Unterscheidbarkeit, Schiefe Daten und die Abfragearten auf dem System ausgeführt.
  
![Tabelle verteilt] (media/sql-data-warehouse-distributed-data/hash-distributed-table.png "Tabelle verteilt")  
  
-   Jede Zeile gehört eine Verteilung.  
  
-   Ein deterministische Hashalgorithmus weist jede Zeile eine Verteilung an.  
  
-   Die Anzahl der Tabellenzeilen pro Verteilung variiert dargestellt durch die verschiedenen Größen der Tabellen.

## <a name="round-robin-distributed-tables"></a>Round-Robert verteilten Tabellen

Round-Robert verteilte Tabelle verteilt die Zeilen durch jede Zeile einer Verteilung sequenziell zuweisen. Kurzübersicht zum Laden von Daten in eine Tabelle Round-Robert hat, aber abfrageleistung sind möglicherweise langsamer.  Teilnehmen an einer Funktion RUNDEN Robert Tabelle in der Regel erfordert eine zufällige Auswahl die Zeilen aus, um die Abfrage in ein richtiges Ergebnis, ergeben, das sodass der Zeit in Anspruch nimmt zu aktivieren.

## <a name="distributed-storage-locations-are-called-distributions"></a>Verteilte Speicherorte werden als Verteilung bezeichnet.

Jede verteilten Speicherort heißt eine Verteilung zurück. Wenn eine Abfrage parallel ausgeführt wird, führt jede Verteilung eine SQL-Abfrage auf den Teil der Daten aus. SQL Data Warehouse verwendet SQL-Datenbank, um die Abfrage auszuführen. Parallel Data Warehouse verwendet SQL Server, um die Abfrage auszuführen. Dieser gemeinsame Nutzung stattfindet Entwurf ist für das erreichen Skalierung parallel computing grundlegende.

### <a name="can-i-view-the-distributions"></a>Kann ich die Verteilung anzeigen?

Jede Verteilung hat eine Verteilung-ID und in Ansichten, die auf SQL Data Warehouse und Parallel Data Warehouse beziehen sichtbar ist. Die Verteilung-ID können die um Leistung von Abfragen und sonstige Probleme zu behandeln. Eine Liste der Systemansichten finden Sie unter der [Ansicht "System" MPP](sql-data-warehouse-reference-tsql-statements.md).

## <a name="difference-between-a-distribution-and-a-compute-node"></a>Unterschied zwischen einer Verteilung zurück und einem berechnen Knoten

Eine Verteilung ist die Grundeinheit für verteilte Daten gespeichert und Verarbeitung parallele Abfragen. Verteilung werden in Datenverarbeitungsknoten gruppiert. Jeder Knoten berechnen verfolgt eine oder mehrere Verteilung an.  

-   Analytics Plattform System verwendet Datenverarbeitungsknoten als eine zentrale Komponente der Hardwarefunktionen-Architektur und zu verkleinern. Es verwendet immer acht Verteilung pro Knoten berechnen, wie im Diagramm für Tabellen Hash verteilt dargestellt. Die Anzahl der Knoten berechnen und daher die Anzahl der Verteilung werden durch die Anzahl der berechnen Knoten bestimmt, die Sie für das Gerät kaufen. Wenn Sie acht berechnen Knoten kaufen, erhalten Sie beispielsweise 64 Verteilung (8 berechnen X 8 Verteilung/Knoten). 

-   SQL Data Warehouse weist einer festen Anzahl von 60 Verteilung und eine flexible Anzahl Knoten berechnen. Die Datenverarbeitungsknoten werden mit Azure computing und Speicher Ressourcen implementiert. Die Anzahl der Knoten berechnen kann ändern, ab der Back-End-Dienst Arbeitsbelastung und die computing Kapazität (DWUs), die Sie für das Datawarehouse angeben. Wenn die Anzahl der berechnen Knoten geändert, ändert sich auch die Anzahl der pro Knoten berechnen. 

### <a name="can-i-view-the-compute-nodes"></a>Kann ich die Datenverarbeitungsknoten anzeigen?

Jeder Knoten berechnen hat eine Knoten-ID und in Ansichten, die auf SQL Data Warehouse und Parallel Data Warehouse beziehen sichtbar ist.  Sie können den Knoten berechnen anzeigen, indem Sie suchen Sie nach der Spalte knoten_id Systemansichten, deren Namen mit sys.pdw_nodes beginnen. Eine Liste der Systemansichten finden Sie unter der [Ansicht "System" MPP](sql-data-warehouse-reference-tsql-statements.md).

## <a name="a-namereplicatedareplicated-tables-for-parallel-data-warehouse"></a><a name="Replicated"></a>Tabellen für Parallel Datawarehouse repliziert 
  
Gilt für: Parallel Datawarehouse

Zusätzlich zur Verwendung von verteilter Tabellen, bietet Parallel Data Warehouse eine Option, um Tabellen repliziert. Eine *Tabelle repliziert* ist eine Tabelle, die auf den einzelnen Knoten berechnen vollständig gespeichert ist. Eine Tabelle repliziert entfernt die seine Tabellenzeilen zwischen Knoten berechnen übertragen, bevor Sie die Tabelle in einer Verknüpfung oder Aggregation verwenden müssen. Aufgrund der zusätzlichen Speicher zum Speichern der Tabelle vollständigen auf den einzelnen Knoten berechnen erforderlich sind nur bei replizierte Tabellen mit kleinen Tabellen soweit möglich.  
  
Das folgende Diagramm veranschaulicht eine replizierte Tabelle, die auf den einzelnen Knoten berechnen gespeichert ist. Die Tabelle replizierte wird auf alle Datenträger zum Berechnungsknoten zugewiesen gespeichert. Diese Strategie Datenträger ist mithilfe von SQL Server-Dateigruppen implementiert.  
  
![Tabelle repliziert] (media/sql-data-warehouse-distributed-data/replicated-table.png "Tabelle repliziert") 
  
## <a name="next-steps"></a>Nächste Schritte
  
Um verteilte Tabellen effizient verwenden zu können, finden Sie unter [Verteilen von Tabellen in SQL Data Warehouse](sql-data-warehouse-tables-distribute.md)  
  



  

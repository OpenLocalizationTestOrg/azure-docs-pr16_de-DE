<properties
   pageTitle="Ansichten im SQL Datawarehouse | Microsoft Azure"
   description="Tipps zur Verwendung von Ansichten Transact-SQL Azure SQL-Data Warehouse für die Entwicklung von Lösungen."
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
   ms.date="07/01/2016"
   ms.author="jrj;barbkess;sonyama"/>


# <a name="views-in-sql-data-warehouse"></a>Ansichten im SQL Datawarehouse

Ansichten sind in SQL Data Warehouse besonders hilfreich. Sie können in eine Reihe von verschiedenen Methoden zur Verbesserung der Qualität Ihrer Lösung verwendet werden.  In diesem Artikel werden einige Beispiele zum bereichern Ihrer Lösung mit Ansichten als auch die Einschränkungen, die zu berücksichtigen hervorgehoben.

> [AZURE.NOTE] Syntax für `CREATE VIEW` wird nicht in diesem Artikel erläutert. Lizenzinformationen finden Sie die [Ansicht erstellen][] MSDN-Artikel für diese Informationen verweisen.

## <a name="architectural-abstraction"></a>Architektur Abstraktion
Ein sehr allgemeine Anwendung Muster besteht darin, Tabellen mithilfe von erstellen Tabelle AS auswählen (CTAS) gefolgt von einem Objekt Muster umbenennen, während die Daten geladen erneut erstellen.

Im folgenden Beispiel wird eine Dimension Date Datum neue Einträge hinzugefügt. Beachten Sie, wie eine neue Tabble, DimDate_New, zuerst erstellt und dann umbenannt werden, um die ursprüngliche Version der Tabelle ersetzen.

```sql
CREATE TABLE dbo.DimDate_New
WITH (DISTRIBUTION = ROUND_ROBIN
, CLUSTERED INDEX (DateKey ASC)
)
AS
SELECT *
FROM   dbo.DimDate  AS prod
UNION ALL
SELECT *
FROM   dbo.DimDate_stg AS stg
;

RENAME OBJECT DimDate TO DimDate_Old;
RENAME OBJECT DimDate_New TO DimDate;

```

Dieser Ansatz kann jedoch in Tabellen angezeigte und verschwinden Benutzeransicht ebenso wie "Tabelle ist nicht vorhanden" Fehlermeldungen führen. Ansichten können verwendet werden, um Benutzern eine konsistente Präsentationsebene bieten, während die zugrunde liegenden Objekte umbenannt werden. Durch die Bereitstellung von Benutzern Zugriff auf die Daten durch eine Ansichten, bedeutet, dass Benutzer müssen nicht Sichtbarkeit der zugrunde liegenden Tabellen. Dies bietet ein konsistentes Erscheinungsbild, während das Datenmodell weiterentwickelt und Leistung maximieren, während das Laden von Daten mithilfe von CTAS kann sicherstellen, dass die Data-Designer Warehouse.    

## <a name="performance-optimization"></a>Zur Optimierung der Leistung
Ansichten können auch zum Erzwingen der Leistung optimiert Verknüpfungen zwischen Tabellen genutzt werden. Eine Ansicht kann beispielsweise einen Schlüssel redundante Verteilung als Bestandteil der Kriterien verknüpfen, um das Verschieben von Daten zu minimieren einbinden.  Ein weiterer Vorteil einer Ansicht könnte so erzwingen Sie eine bestimmte Abfrage oder verknüpfen Hinweistext. Verwenden von Ansichten auf diese Weise wird sichergestellt, dass Verknüpfungen auf optimale Weise Benutzer Beachten Sie die richtige erstellen für ihre Verknüpfungen überflüssig immer durchgeführt werden.

## <a name="limitations"></a>Einschränkungen
Ansichten in SQL Data Warehouse sind nur Metadaten.  Die folgenden Optionen sind daher nicht verfügbar:

-   Es gibt keine Schema Bindungsoption
-   Base Tabellen können nicht über die Ansicht aktualisiert werden
-   Ansichten können nicht über temporäre Tabellen erstellt werden
-   Es gibt keine Unterstützung für die erweitern / NOEXPAND Hinweise
-   Es sind keine indizierten Sichten in SQL Data Warehouse


## <a name="next-steps"></a>Nächste Schritte
Weitere Hinweise zur Entwicklung finden Sie unter [Übersicht über die Entwicklung von SQL Data Warehouse][].
Für `CREATE VIEW` Syntax finden Sie in der [Ansicht erstellen][].

<!--Image references-->

<!--Article references-->
[Übersicht über die Entwicklung von SQL Data Warehouse]: ./sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[ANSICHT ERSTELLEN]: https://msdn.microsoft.com/en-us/library/ms187956.aspx

<!--Other Web references-->

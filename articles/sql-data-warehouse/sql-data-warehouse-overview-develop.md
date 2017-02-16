<properties
   pageTitle="Entwerfen von Entscheidungen und Codierung Techniken für die Entwicklung von SQL Data Warehouse | Microsoft Azure"
   description="Entwicklung Konzepte, Entwurf Entscheidungen, Empfehlungen und Codierung Techniken für SQL Data Warehouse."
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
   ms.date="08/16/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="design-decisions-and-coding-techniques-for-sql-data-warehouse"></a>Entwerfen von Entscheidungen und Codierung Techniken für SQL Data Warehouse

Sehen Sie über die folgenden Artikel Entwicklung Key Entwurf Entscheidungen, Empfehlungen und Codierung Techniken für SQL Data Warehouse besser zu verstehen.

## <a name="key-design-decisions"></a>Key Entwurf Entscheidungen
Markieren Sie die folgenden Artikeln einige grundlegende Konzepte und Entwurf Entscheidungen, die Sie für die Entwicklung des verteilten Datawarehouse mithilfe der SQL Data Warehouse bekannt sein müssen:

- [Verbindungen][]
- [Parallelität][]
- [Transaktionen.][]
- [Benutzerdefinierte schemas][]
- [Tabelle Verteilung][]
- [Tabellenindizes][]
- [Tabellenpartitionen][]
- [CTAS][]
- [Statistik][]

## <a name="development-recommendations-and-coding-techniques"></a>Empfehlungen Entwicklung und Codierung Techniken
Die folgenden Artikel hervorheben bestimmte Codierung Techniken, Tipps und Vorschläge für die Entwicklung Ihrer SQL Data Warehouse:

- [gespeicherten Prozeduren][]
- [Etiketten][]
- [Ansichten][]
- [temporäre Tabellen][]
- [Dynamisches SQL][]
- [Endloswiedergabe][]
- [Gruppe von Optionen][]
- [Variable Zuordnung][]

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie den Artikeln Entwicklung bereits kennen sehen Sie über die Seite [Transact-SQL-Referenz][] für weitere Details zur unterstützten Syntax für SQL Data Warehouse.

<!--Image references-->

<!--Article references-->
[Parallelität]: ./sql-data-warehouse-develop-concurrency.md
[Verbindungen]: ./sql-data-warehouse-connect-overview.md
[CTAS]: ./sql-data-warehouse-develop-ctas.md
[Dynamisches SQL]: ./sql-data-warehouse-develop-dynamic-sql.md
[Gruppe von Optionen]: ./sql-data-warehouse-develop-group-by-options.md
[Etiketten]: ./sql-data-warehouse-develop-label.md
[Endloswiedergabe]: ./sql-data-warehouse-develop-loops.md
[Statistik]: ./sql-data-warehouse-tables-statistics.md
[gespeicherten Prozeduren]: ./sql-data-warehouse-develop-stored-procedures.md
[Tabelle Verteilung]: ./sql-data-warehouse-tables-distribute.md
[Tabellenindizes]: ./sql-data-warehouse-tables-index.md
[Tabellenpartitionen]: ./sql-data-warehouse-tables-partition.md
[temporäre Tabellen]: ./sql-data-warehouse-tables-temporary.md
[Transaktionen.]: ./sql-data-warehouse-develop-transactions.md
[Benutzerdefinierte schemas]: ./sql-data-warehouse-develop-user-defined-schemas.md
[Variable Zuordnung]: ./sql-data-warehouse-develop-variable-assignment.md
[Ansichten]: ./sql-data-warehouse-develop-views.md
[Transact-SQL-Referenz]: ./sql-data-warehouse-overview-reference.md

<!--MSDN references-->
[renaming objects]: https://msdn.microsoft.com/library/mt631611.aspx

<!--Other Web references-->

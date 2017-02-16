<properties
   pageTitle="Migrieren von Ihrem Schema zu SQL Data Warehouse | Microsoft Azure"
   description="Tipps für die Migration von Ihrem Schemas in Azure SQL-Data Warehouse für die Entwicklung von Lösungen."
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
   ms.date="08/25/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="migrate-your-schema-to-sql-data-warehouse"></a>Migrieren von Ihrem Schema zu SQL Data Warehouse#

In der folgenden zusammengefasst können Sie die Unterschiede zwischen SQL Server und SQL Data Warehouse helfen Ihnen, Ihre Datenbank migrieren zu verstehen.

## <a name="table-migration"></a>Die Migration – Tabelle

Bei der Migration von Tabellen, sollten Sie mit der Tabellenfeatures von SQL Data Warehouse Tabellen vertraut zu machen.  Die [Tabelle Übersicht][] eignet sich zu starten.  In diesem Artikel werden die wichtigsten Aspekte beim Erstellen einer Tabelle wie die Tabellenstatistiken, Quadrat-verteilten Zufallsgröße Partitionierung und Indizierung.  Es werden auch einige der [nicht unterstützte Tabellenfeatures][] und problemumgehungen behandelt.

SQL Data Warehouse unterstützt die gängigen Datentypen für Business.  Finden Sie im Artikel [Datentypen][] für eine Liste der unterstützten und [nicht unterstützte Datentypen][].  Im Artikel [Datentypen][] enthält auch eine Abfrage, um [nicht unterstützte Datentypen][]zu identifizieren.  Beim Konvertieren von Datentypen, müssen Sie den [Datentyp bewährte Methoden][]eigenständig.

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie Ihre Datenbankschema in SQL Data Warehouse erfolgreich migriert haben, fahren Sie mit den folgenden Artikeln:

- [Migrieren von Daten][]
- [Migrieren von code][]

Weitere Informationen zu best Practices SQL Data Warehouse finden Sie unter [bewährte Methoden][] Artikel.

<!--Image references-->

<!--Article references-->
[Migrieren von code]: ./sql-data-warehouse-migrate-code.md
[Migrieren von Daten]: ./sql-data-warehouse-migrate-data.md
[Bewährte Methoden]: ./sql-data-warehouse-best-practices.md
[Tabelle (Übersicht)]: ./sql-data-warehouse-tables-overview.md
[nicht unterstützte Tabellenfeatures]: ./sql-data-warehouse-tables-overview.md#unsupported-table-features
[Datentypen]: ./sql-data-warehouse-tables-data-types.md
[nicht unterstützte Datentypen]: ./sql-data-warehouse-tables-data-types.md#unsupported-data-types
[Bewährte Methoden des Datentyps]: ./sql-data-warehouse-tables-data-types.md#data-type-best-practices

<!--MSDN references-->


<!--Other Web references-->

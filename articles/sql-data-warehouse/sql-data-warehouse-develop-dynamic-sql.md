<properties
   pageTitle="Dynamisches SQL im SQL Datawarehouse | Microsoft Azure"
   description="Tipps zur Verwendung von dynamischen SQL Azure SQL-Data Warehouse für die Entwicklung von Lösungen."
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

# <a name="dynamic-sql-in-sql-data-warehouse"></a>Dynamisches SQL im SQL Datawarehouse
Bei der Entwicklung der Anwendungscode für SQL Data Warehouse möglicherweise müssen Sie dynamisches Sql verwenden, um Hilfe flexible, generische und modulare Solutions bieten. SQL Data Warehouse unterstützt Blob-Datentypen zurzeit nicht. Dies kann die Größe der Zeichenfolgen beschränken, als Blob-Typen sowohl "nvarchar(max)", "varchar(max)" und nvarchar(max) Dateitypen gehören. Wenn Sie diese Dateitypen in Ihrer Anwendungscode verwendet haben, wenn sehr große Zeichenfolgen erstellen, müssen Sie den Code in Abschnitte aufteilen, und verwenden Sie stattdessen die Anweisung ausführen.

Ein einfaches Beispiel:

```sql
DECLARE @sql_fragment1 VARCHAR(8000)=' SELECT name '
,       @sql_fragment2 VARCHAR(8000)=' FROM sys.system_views '
,       @sql_fragment3 VARCHAR(8000)=' WHERE name like ''%table%''';

EXEC( @sql_fragment1 + @sql_fragment2 + @sql_fragment3);
```

Wenn die Zeichenfolge kurz ist können Sie wie üblich [Sp_executesql][] .

> [AZURE.NOTE] Anweisungen als dynamisches SQL ausgeführt werden weiterhin unterliegen alle TSQL Gültigkeitsprüfungsregeln.

## <a name="next-steps"></a>Nächste Schritte
Weitere Hinweise zur Entwicklung finden Sie unter [Übersicht über die Entwicklung][].

<!--Image references-->

<!--Article references-->
[Übersicht über die Entwicklung]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[sp_executesql]: https://msdn.microsoft.com/library/ms188001.aspx

<!--Other Web references-->

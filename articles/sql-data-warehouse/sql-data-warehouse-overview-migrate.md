<properties
   pageTitle="Migrieren von Ihrer Lösung zu SQL Data Warehouse | Microsoft Azure"
   description="Hinweise zur Migration für Ihre Lösung zur Azure SQL-Data Warehouse Plattform Onlineschalten."
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
   ms.date="08/30/2016"
   ms.author="barbkess;jrj;sonyama"/>

# <a name="migrate-your-solution-to-sql-data-warehouse"></a>Migrieren von Ihrer Lösung zu SQL Data Warehouse

SQL Data Warehouse ist eine verteilte Datenbank-System, die Ihren Anforderungen elastisch skaliert. Um die Leistung und die Skalierung beizubehalten, werden nicht alle Features in SQL Server in SQL Data Warehouse implementiert. In den folgenden Themen der Migration Tippen Sie auf einige der wichtigsten Faktoren für die Migration von der Lösung in SQL Data Warehouse. Entwerfen Lager Skala mit Daten führt ein anderes Design, dass Muster und daher herkömmlichen Vorgehensweisen immer die optimale nicht. Daher bietet zur Anpassung Ihrer vorhandenen Lösung stellt sicher, dass Sie vollständig die verteilte Plattform von SQL Data Warehouse bereitgestellten nutzen.

Es ist auch Denken Sie daran, dass SQL Data Warehouse Plattformen mit in Microsoft Azure ist. Daher möglicherweise auch Teil der Migration enthalten übertragen Ihrer Daten in der Cloud. Datenübertragung ist ein Thema in einem eigenen rechts und muss sorgfältig überlegt; besonders, wenn Datenmengen erhöhen. Datenübertragung oder das Laden von Daten sind diskrete Themen.

## <a name="migration-guidance"></a>Hinweise zur Migration

Stellen Sie sicher, dass Sie über die folgenden Artikel, um sicherzustellen, dass Sie einige der grundlegenden Konzepte und Product Unterschiede verstehen, ehe die Migration gelesen haben.

- [Migrieren von Ihrem schema][]
- [Migrieren von Daten][]
- [Migrieren von code][]

## <a name="next-steps"></a>Nächste Schritte

Die Katze (Ihren Kunden-Team), weist ebenfalls einige gute SQL Data Warehouse Anleitung die diese bis Blogs veröffentlichen.  Sehen Sie sich deren Artikel [Migrieren von Daten zu Azure SQL-Data Warehouse Methode][] , um weitere Unterstützung für die Migration.

<!--Image references-->

<!--Article references-->
[Migrieren von Ihrem schema]: sql-data-warehouse-migrate-schema.md
[Migrieren von Daten]: sql-data-warehouse-migrate-data.md
[Migrieren von code]: sql-data-warehouse-migrate-code.md


<!--MSDN references-->


<!--Other Web references-->
[Migrieren von Daten in der Praxis Azure SQL-Data Warehouse]: https://blogs.msdn.microsoft.com/sqlcat/2016/08/18/migrating-data-to-azure-sql-data-warehouse-in-practice/

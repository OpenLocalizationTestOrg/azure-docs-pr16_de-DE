<properties
   pageTitle="Migrieren zu SQL-Datenbank mithilfe der Transaktionsreplikation | Microsoft Azure"
   description="Importieren von Microsoft Azure SQL-Datenbank Datenbankmigration Datenbank Transaktionsreplikation"
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="sqldb-migrate"
   ms.date="08/23/2016"
   ms.author="carlrab"/>

# <a name="migrate-sql-server-database-to-azure-sql-database-using-transactional-replication"></a>Migrieren von SQL Server-Datenbank mit Azure SQL-Datenbank mithilfe der Transaktionsreplikation

> [AZURE.SELECTOR]
- [SSMS Migrations-Assistenten](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md)
- [Exportieren in Datendatei BACPAC](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
- [Importieren von BACPAC-Datei](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [Transaktionsreplikation](sql-database-cloud-migrate-compatible-using-transactional-replication.md)

In diesem Artikel erfahren Sie, eine SQL Server-kompatible Datenbank mit Azure SQL-Datenbank mit minimaler Ausfallzeit mithilfe der SQL Server-Transaktionsreplikation migrieren.

## <a name="understanding-the-transactional-replication-architecture"></a>Grundlegendes zu den Transaktionsreplikation Architektur

Wenn Sie leisten können, entfernen Sie die SQL Server-Datenbank aus Herstellung während die Migration ausgeführt wird, können Sie Transaktionen SQL Server-Replikation als Ihre Lösung für die Migration verwenden. Um diese Lösung zu verwenden, konfigurieren Sie Ihre Azure SQL-Datenbank als Abonnent für lokalen SQL Server-Instanz, die Sie migrieren möchten. Die lokale Transaktionsreplikation Distributor Daten aus der lokalen Datenbank synchronisiert (Herausgeber) synchronisiert werden, während neue Transaktionen weiterhin auftreten. 

Sie können auch Transaktionsreplikation verwenden, eine Teilmenge der lokale Datenbank migrieren. Die Publikation, die Replikation an Azure SQL-Datenbank kann auf eine Teilmenge der Tabellen in der Datenbank, die repliziert beschränkt werden. Für jede Tabelle repliziert wird können Sie die Daten zu einer Teilmenge der Zeilen und/oder eine Teilmenge der Spalten einschränken.

Für die Transaktionsreplikation angezeigt alle Änderungen an Ihrer Daten oder das Schema in Ihrer Azure SQL-Datenbank. Nachdem die Synchronisierung abgeschlossen ist, und Sie nun migrieren können, ändern Sie die Verbindungszeichenfolge Anwendungen diese auf Ihrer SQL Azure-Datenbank verweisen. Nachdem Transaktionsreplikation dies nach links auf die lokale Datenbank und sämtliche zeigen Sie Ihre Programme zur Azure-Datenbank vornehmen geschieht, können Sie die Transaktionsreplikation deinstallieren. Der SQL Azure-Datenbank ist jetzt der Herstellung System.

 ![SeedCloudTR Diagramm](./media/sql-database-cloud-migrate/SeedCloudTR.png)

## <a name="transactional-replication-requirements"></a>Transaktionen Replikation Anforderungen

Transaktionen Replikation ist eine integrierte und integrierte mit SQL Server-Technologie seit SQL Server 6.5. Es ist eine Reifen und bewährte Technologie, dass die meisten DBAs wissen, mit denen sie Erfahrung haben. Mit den [SQL Server 2016](https://www.microsoft.com/en-us/cloud-platform/sql-server)ist es möglich, Azure SQL-Datenbank als [Transaktionen Obergrenze](https://msdn.microsoft.com/library/mt589530.aspx) zu einer lokalen Publikation zu konfigurieren. Mit der Sie ihn von Management Studio einrichten abrufen ist dieselbe, als wäre Sie Transaktionsreplikation Abonnent auf einem lokalen Server eingerichtet haben. Unterstützung für dieses Szenario wird unterstützt, wenn Sie mindestens eines der folgenden SQL Server-Versionen Herausgeber und der Verteiler sind:

 - SQLServer 2016 und höher 
 - SQL Server 2014 SP1 CU3 und höher
 - SQL Server 2014 RTM CU10 und höher
 - SQL Server 2012 SP2 CU8 und höher
 - SQL Server 2012 SP3 und höher


> [AZURE.IMPORTANT] Verwenden Sie die neueste Version von SQL Server Management Studio, um mit Microsoft Azure und SQL-Datenbank-Updates synchronisiert werden. Ältere Versionen von SQL Server Management Studio können keine SQL-Datenbank als Abonnent einrichten. [Aktualisieren von SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).


## <a name="next-steps"></a>Nächste Schritte

- [Neueste Version von SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)
- [Neueste Version von SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [SQLServer 2016](https://www.microsoft.com/en-us/cloud-platform/sql-server)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Transaktionsreplikation](https://msdn.microsoft.com/library/mt589530.aspx)
- [SQL-Datenbank V12](sql-database-v12-whats-new.md)
- [Transact-SQL teilweise oder nicht unterstützte Funktionen](sql-database-transact-sql-information.md)
- [Migrieren von SQL Server - Datenbanken mit SQL Server Migrations-Assistenten](http://blogs.msdn.com/b/ssma/)

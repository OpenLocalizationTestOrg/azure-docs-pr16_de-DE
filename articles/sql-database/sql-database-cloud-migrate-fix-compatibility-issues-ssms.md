<properties
   pageTitle="Kompatibilitätsprobleme bei der SQL Server-Datenbank mithilfe von SQL Server Management Studio vor der Migration mit SQL-Datenbank beheben | Microsoft Azure"
   description="Microsoft Azure SQL-Datenbank, Datenbankmigration, Kompatibilität, SQL Azure-Assistent für die Migration"
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
   ms.date="08/24/2016"
   ms.author="carlrab"/>

# <a name="fix-sql-server-database-compatibility-issues-using-sql-server-management-studio-before-migration-to-sql-database"></a>Beheben von SQL Server-Datenbank-Kompatibilitätsprobleme mit SQL Server Management Studio vor der Migration zu SQL-Datenbank

> [AZURE.SELECTOR]
- Verwenden von [SQL Azure Migrations-Assistenten](sql-database-cloud-migrate-fix-compatibility-issues.md)
- Verwenden von [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- Verwenden von [SSMS](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md)

Fortgeschrittene Benutzer können Kompatibilitätsprobleme bei der SQL Server-Datenbank mithilfe von SQL Server Management Studio vor der Migration zu SQL Azure-Datenbank beheben.


> [AZURE.IMPORTANT] Es wird empfohlen, dass Sie immer die neueste Version von Management Studio verwenden, um mit Microsoft Azure und SQL-Datenbank-Updates synchronisiert werden. [Aktualisieren von SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).


## <a name="using-sql-server-management-studio"></a>Verwenden von SQL Server Management Studio

Verwenden Sie SQL Server Management Studio, um Kompatibilitätsprobleme mit verschiedenen Transact-SQL-Befehle wie **ALTER DATABASE**zu beheben. Diese Methode ist hauptsächlich für fortgeschrittene Benutzer, die vertraut sind Transact-SQL auf einer live Datenbank arbeiten. Andernfalls wird empfohlen, dass Sie SSDT verwenden. 



## <a name="next-steps"></a>Nächste Schritte

- [Neueste Version von SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Neueste Version von SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)
- [Migrieren einer SQL Server-kompatiblen Datenbank mit SQL-Datenbank](sql-database-cloud-migrate.md#migrate-a-compatible-sql-server-database-to-sql-database)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [SQL-Datenbank V12](sql-database-v12-whats-new.md)
- [Transact-SQL teilweise oder nicht unterstützte Funktionen](sql-database-transact-sql-information.md)
- [Migrieren von SQL Server - Datenbanken mit SQL Server Migrations-Assistenten](http://blogs.msdn.com/b/ssma/)

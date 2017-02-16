<properties
   pageTitle="Exportieren eine SQL Server-Datenbank in eine BACPAC-Datei, die mit SqlPackage | Microsoft Azure"
   description="Microsoft Azure SQL-Datenbank Datenbankmigration Datenbank exportieren, Sqlpackage BACPAC Datei exportieren"
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

# <a name="export-a-sql-server-database-to-a-bacpac-file-using-sqlpackage"></a>Exportieren einer SQL Server-Datenbank in eine BACPAC-Datei, die mit SqlPackage

> [AZURE.SELECTOR]
- [SSMS](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
- [SqlPackage](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md)

In diesem Artikel wird gezeigt, wie eine SQL Server-Datenbank in eine [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) -Datei, die mit dem Befehlszeilendienstprogramm [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) exportieren. Dieses Programm im Lieferumfang von der neuesten Versionen von [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) und [SQL Server Data Tools für Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx), oder Sie können die neueste Version von [SqlPackage](https://www.microsoft.com/en-us/download/details.aspx?id=53876) direkt vom Microsoft Downloadcenter herunterladen.

1. Öffnen Sie ein Eingabeaufforderungsfenster, und ändern Sie ein Verzeichnis mit dem Befehlszeilendienstprogramm sqlpackage.exe: Dieses Programm ist im Lieferumfang von Visual Studio und SQL Server. Verwenden Sie die Suche auf Ihrem Computer den Pfad in Ihrer Umgebung zu finden.
2. Führen Sie den folgenden Befehl aus sqlpackage.exe mit den folgenden Argumenten für Ihre Umgebung an:

    ' sqlpackage.exe /Action:Export /ssn: < Servername > /sdn: < Datenbankname > /tf: < Target_file >

  	| Argumente  | Beschreibung  |
  	|---|---|
  	| < Servername >  | Quelle-Servernamens  |
  	| < Datenbankname >  | Name der Quelldatenbank  |
  	| < Target_file >  | Dateinamen und einen Speicherort für die Datei BACPAC  |

    ![Exportieren einer Datenebene Anwendungs aus dem Menü Aufgaben](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSQLPackage01b.png)

## <a name="next-steps"></a>Nächste Schritte

- [Neueste Version von SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Neueste Version von SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)
- [Importieren einer BACPAC mit SSMS mit Azure SQL-Datenbank](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [Importieren Sie eine BACPAC in SQL Azure-Datenbank SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)
- [Importieren einer BACPAC Azure SQL-Datenbank Azure-Portal](sql-database-import.md)
- [Importieren Sie eine BACPAC in SQL Azure-Datenbank PowerShell](sql-database-import-powershell.md)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [SQL-Datenbank V12](sql-database-v12-whats-new.md)
- [Transact-SQL teilweise oder nicht unterstützte Funktionen](sql-database-transact-sql-information.md)
- [Migrieren von SQL Server - Datenbanken mit SQL Server Migrations-Assistenten](http://blogs.msdn.com/b/ssma/)

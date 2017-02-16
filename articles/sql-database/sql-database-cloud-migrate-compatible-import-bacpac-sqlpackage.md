<properties
   pageTitle="Importieren Sie in SQL-Datenbank aus einer BACPAC-Datei, die mit SqlPackage"
   description="Microsoft Azure SQL-Datenbank Datenbankmigration Datenbank zu importieren, Sqlpackage BACPAC Datei importieren"
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

# <a name="import-to-sql-database-from-a-bacpac-file-using-sqlpackage"></a>Importieren Sie in SQL-Datenbank aus einer BACPAC-Datei, die mit SqlPackage

> [AZURE.SELECTOR]
- [SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)
- [Azure-portal](sql-database-import.md)
- [PowerShell](sql-database-import-powershell.md)

In diesem Artikel wird gezeigt, wie mit SQL-Datenbank aus einer mit dem Befehlszeilendienstprogramm [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) -Datei importieren. Dieses Programm im Lieferumfang von der neuesten Versionen von [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) und [SQL Server Data Tools für Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx), oder Sie können die neueste Version von [SqlPackage](https://www.microsoft.com/en-us/download/details.aspx?id=53876) direkt vom Microsoft Downloadcenter herunterladen.


> [AZURE.NOTE] Die folgenden Schritten wird vorausgesetzt, dass Sie haben bereits einen SQL-Datenbankserver bereitgestellt, die Verbindungsinformationen zur hand haben und überprüft haben, dass die Quelldatenbank kompatibel ist.

## <a name="import-from-a-bacpac-file-into-azure-sql-database-using-sqlpackage"></a>Importieren Sie aus einer Datei BACPAC in Azure SQL-Datenbank mit SqlPackage

Gehen Sie folgendermaßen vor, um das Befehlszeilendienstprogramm [SqlPackage.exe](https://msdn.microsoft.com/library/hh550080.aspx) zu verwenden, um einen kompatiblen SQL Server-Datenbank (oder SQL Azure-Datenbank) importieren aus einer Datei BACPAC.

> [AZURE.NOTE] Die folgenden Schritte angenommen, Sie bereits einen Azure SQL-Datenbankserver bereitgestellt haben und die Verbindungsinformationen zur hand haben.

1. Öffnen Sie ein Eingabeaufforderungsfenster, und ändern Sie ein Verzeichnis mit dem Befehlszeilendienstprogramm sqlpackage.exe: Dieses Programm ist im Lieferumfang von Visual Studio und SQL Server.
2. Führen Sie den folgenden Befehl aus sqlpackage.exe mit den folgenden Argumenten für Ihre Umgebung an:

    `sqlpackage.exe /Action:Import /tsn:< server_name > /tdn:< database_name > /tu:< user_name > /tp:< password > /sf:< source_file >`

  	| Argumente  | Beschreibung  |
  	|---|---|
  	| < Servername >  | Ziel-Servernamens  |
  	| < Datenbankname >  | der Zieldatenbankname  |
  	| < Benutzername >  | der Benutzername in den Ziel-server |
  	| < Kennwort >  | das Kennwort des Benutzers  |
  	| < Source_file >  | der Dateiname und Speicherort für die zu importierende BACPAC-Datei  |

    ![Exportieren einer Datenebene Anwendungs aus dem Menü Aufgaben](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSQLPackage01c.png)

## <a name="next-steps"></a>Nächste Schritte

- [Neueste Version von SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Neueste Version von SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [SQL-Datenbank V12](sql-database-v12-whats-new.md)
- [Transact-SQL teilweise oder nicht unterstützte Funktionen](sql-database-transact-sql-information.md)
- [Migrieren von SQL Server - Datenbanken mit SQL Server Migrations-Assistenten](http://blogs.msdn.com/b/ssma/)

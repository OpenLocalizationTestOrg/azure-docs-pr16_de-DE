<properties
   pageTitle="Untersuchen der SQL Azure-Datenbank Lernprogramme | Microsoft Azure"
   description="Erfahren Sie mehr über die SQL-Datenbank-Features und Funktionen"
   keywords=""
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
   ms.workload="data-management"
   ms.date="08/24/2016"
   ms.author="carlrab"/>
   
# <a name="explore-azure-sql-database-tutorials"></a>Untersuchen der SQL Azure-Datenbank-Lernprogramme

Die folgenden Links erhalten Sie einen Überblick über die einzelnen aufgeführten Features Fläche und einer einfachen Schritt-vom Anfang Lernprogramm für jeden Bereich. Lösung ausgelegte Symbolleiste gestartet wird, die die Verwendung der SQL-Datenbank in eine vollständige Lösung basierend auf realen Szenarien veranschaulicht wird, finden Sie unter [Azure SQL Datenbank Lösung Symbolleiste wird gestartet](sql-database-solution-quick-starts.md).

## <a name="using-sql-server-management-studio"></a>Verwenden von SQL Server Management Studio

Die folgenden Lernprogramme lernen Sie, wie Sie mithilfe von SQL Server Management Studio verwalten und Abfragen Azure SQL-Datenbank.


> [AZURE.IMPORTANT] Es wird empfohlen, dass Sie immer die neueste Version von Management Studio verwenden, um mit Microsoft Azure und SQL-Datenbank-Updates synchronisiert werden. [Aktualisieren von SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).


| Lernprogramm  | Beschreibung  |
|---|---|---|
| [Herstellen einer Verbindung mit einer Ebene Server Hauptbenutzer Login Azure SQL-Datenbank mit](sql-database-get-started-security.md#connect-to-azure-sql-database-using-a-server-level-principal-login)| In diesem Lernprogramm erfahren Sie, wie mit Azure SQL-Datenbank mithilfe eines Servers Ebene Hauptbenutzer Benutzernamens verbinden.|
| [Verbinden Sie mit Azure SQL-Datenbank als Benutzer](sql-database-get-started-security.md#connect-to-azure-sql-database-as-a-user) | In diesem Lernprogramm erfahren Sie, wie die Verbindung mit einer SQL Azure-Datenbank mit einem Datenbank-Level-Benutzerkonto.|
||||

## <a name="elastic-pools"></a>Flexible pools

Die folgenden Lernprogramme lernen Sie, wie Sie mithilfe von [flexible Pools](sql-database-elastic-pool.md) die Leistungsziele für mehrere Datenbanken verwalten, die Streuung variierenden und unvorhergesehene Verwendungsmustern enthalten.

| Lernprogramm  | Beschreibung  |
|---|---|---|
| [Erstellen Sie eine flexible Ressourcenpool](sql-database-elastic-pool-create-portal.md) | In diesem Lernprogramm erfahren Sie, wie Sie einen skalierbaren Pool von SQL Azure-Datenbanken erstellen. |
| [Überwachen einer flexible-Datenbank](sql-database-elastic-pool-manage-portal.md#elastic-database-monitoring) | In diesem Lernprogramm erfahren Sie, wie Sie eine einzelne flexible Datenbank für potenzielle Probleme zu überwachen. |
| [Hinzufügen einer Benachrichtigung für eine Ressource Ressourcenpool](sql-database-elastic-pool-manage-portal.md#add-an-alert-to-a-pool-resource) | In diesem Lernprogramm erfahren Sie, wie Regeln zur Ressourcen hinzufügen, die Senden von e-Mail an Personen oder benachrichtigen Zeichenfolgen mit URL Endpunkten, wenn die Ressource einen Schwellenwert Auslastung Treffer, den Sie einrichten. |
| [Verschieben einer Datenbank in eine flexible Ressourcenpool](sql-database-elastic-pool-manage-portal.md#move-a-database-into-an-elastic-pool) | In diesem Lernprogramm erfahren Sie, wie Sie eine Datenbank in eine flexible Ressourcenpool zu verschieben. |
| [Verschieben einer Datenbank aus einer flexible Ressourcenpool](sql-database-elastic-pool-manage-portal.md#move-a-database-out-of-an-elastic-pool) | In diesem Lernprogramm erfahren Sie, wie Sie eine Datenbank aus einer flexible Ressourcenpool zu verschieben. |
| [Ändern der Einstellungen der Leistung von einem Ressourcenpool](sql-database-elastic-pool-manage-portal.md#change-performance-settings-of-a-pool) | In diesem Lernprogramm erfahren Sie, wie die Leistung und Speicher Grenzwerte für einen Pool anpassen. |
||||

## <a name="elastic-database-jobs"></a>Flexible Datenbankaufträge

Die folgenden Lernprogramme lernen Sie [flexible Datenbankaufträge](sql-database-elastic-jobs-overview.md)verwenden.

| Lernprogramm  | Beschreibung  |
|---|---|---|
| [Erste Schritte mit flexible Datenbanktools](sql-database-elastic-scale-get-started.md) | In diesem Lernprogramm erfahren Sie, wie die Funktionen von flexible Datenbanktools mithilfe einer einfachen sharded Anwendungs verwenden. |
| [Erste Schritte mit flexible Aufträge Azure SQL-Datenbank](sql-database-elastic-jobs-getting-started.md)  | In diesem Lernprogramm erfahren Sie, wie auf das Erstellen und Verwalten von Einzelvorgänge, eine Gruppe von verwandten Datenbanken verwalten.  | 
||||

## <a name="elastic-queries"></a>Flexible Abfragen

Die folgenden Lernprogramme lernen Sie, wie [Flexible Abfragen](sql-database-elastic-query-overview.md)ausgeführt werden. 

| Lernprogramm  | Beschreibung  |
|---|---|---|
| [Abfragen in einer Datenbank mit horizontal partitionierten (sharded))](sql-database-elastic-query-getting-started.md) | In diesem Lernprogramm erfahren Sie, wie Sie zum Erstellen von Berichten aus allen Datenbanken in einer horizontal partitionierten (sharded) Datenbank [flexible Abfrage](sql-database-elastic-query-overview.md) verwenden |
| [Abfragen über eine vertikal partitionierten Datenbank)](sql-database-elastic-query-getting-started-vertical.md#create-database-objects) | In diesem Lernprogramm erfahren Sie, wie Sie zum Erstellen von Berichten aus allen Datenbanken in einem vertikal Datenbank [flexible Abfrage](sql-database-elastic-query-overview.md) verwenden |
| [Migrieren einer vorhandenen Datenbank auf Skalierung](sql-database-elastic-convert-to-use-elastic-tools.md)| In diesem Lernprogramm erfahren Sie horizontal (Shard) eine SQL Azure-Datenbank skalieren. |
||||

## <a name="performance-optimization"></a>Zur Optimierung der Leistung

Die folgenden Lernprogramme lernen Sie, wie Sie die [Leistung der einzelnen Datenbanken](sql-database-performance-guidance.md)optimieren. Optimieren die Leistung von mehreren Datenbanken, finden Sie unter [flexible Pools](#elastic-pools).

| Lernprogramm  | Beschreibung  |
|---|---|---|
| [Ändern der Dienstalter Ebene und Leistung der Datenbank](sql-database-scale-up.md#change-the-service-tier-and-performance-level-of-your-database) | In diesem Lernprogramm erfahren Sie, wie vergrößern oder verkleinern auf die Leistung einer SQL Azure-Datenbank mit Service Ebenen. |
| [SQL-Datenbank Advisor Abfrage Leistung Einblick](sql-database-performance.md#performance-overview) | In diesem Lernprogramm erfahren Sie, wie zu öffnen und SQL-Datenbank Advisor Abfrage Leistung Einblicke zu verwenden.|
| [SQL-Datenbank Advisor Leistung Empfehlungen](sql-database-advisor.md#viewing-recommendations) | In diesem Lernprogramm erfahren Sie, wie Sie zum Anzeigen und SQL-Datenbank Advisor Leistung Empfehlungen anwenden. |
| [Überprüfen Sie die obersten CPU Verarbeitung von Abfragen](sql-database-query-performance.md#review-top-cpu-consuming-queries)| In diesem Lernprogramm erfahren Sie, wie SQL-Datenbank Advisor Abfrage Leistung Einblicke verwendete Abfragen Verarbeitung CPU überprüfen.|
| [Einzelne Abfragedetails anzeigen](sql-database-query-performance.md#viewing-individual-query-details)| In diesem Lernprogramm erfahren Sie, wie Sie SQL-Datenbank Advisor Abfrage Leistung Einblicke, um einzelne Leistung Abfragedetails anzeigen.|
||||

## <a name="sql-database-migration-and-archive"></a>SQL-Datenbankmigration und Archiv 

Die folgenden Lernprogramme erfahren Sie mehr zum [Migrieren einer vorhandenen SQL Server-Datenbank mit Azure SQL-Datenbank](sql-database-cloud-migrate.md).

| Lernprogramm  | Beschreibung  |
|---|---|---|
| [Erkennen von Kompatibilitätsprobleme mit SQL Server Data Tools für Visual Studio](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md#detecting-compatibility-issues-using-sql-server-data-tools-for-visual-studio) | In diesem Lernprogramm erfahren Sie, wie Sie SQL Server Data Tools für Visual Studio, um festzustellen Kompatibilität Azure SQL-Datenbank. |
| [Beheben von Kompatibilitätsproblemen mit SQL Server Data Tools für Visual Studio](sql-database-cloud-migrate-fix-compatibility-issues-ssdt#fixing-compatibility-issues-using-sql-server-data-tools-for-visual-studio) | In diesem Lernprogramm erfahren Sie, wie Sie SQL Server Data Tools für Visual Studio zum Beheben von Kompatibilitätsproblemen Azure SQL-Datenbank. |
| [Ermitteln Sie die SQL-Datenbank-Kompatibilität mit SqlPackage.exe](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md#using-sqlpackageexe) | In diesem Lernprogramm erfahren Sie, wie Sie mithilfe des Befehlszeilendienstprogramms SQLPackage.exe um Kompatibilität Azure SQL-Datenbank zu bestimmen.|
| [Ermitteln Sie die SQL-Datenbank-Kompatibilität mit SSMS](sql-database-cloud-migrate-determine-compatibility-ssms.md#using-sql-server-management-studio) |In diesem Lernprogramm erfahren Sie, wie Sie SQL Server Management Studio, um festzustellen Kompatibilität Azure SQL-Datenbank.|
| [Migrieren von SQL Server-Datenbank mit SQL-Datenbank, die mit Microsoft Azure-Datenbank-Assistent-Datenbank bereitstellen](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md#use-the-deploy-database-to-microsoft-azure-database-wizard) | In diesem Lernprogramm erfahren Sie, wie Sie eine SQL Server-kompatible Datenbank mit Azure SQL-Datenbank verwenden die Datenbank bereitstellen, Microsoft Azure-Datenbank-Assistenten in SQL Server Management Studio migrieren.
| [Exportieren einer SQL Server-Datenbank in eine BACPAC-Datei, die mit SSMS](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md) | In diesem Lernprogramm erfahren Sie, wie Sie eine SQL Server-kompatible Datenbank in einer BACPAC-Datei, die mit dem Exportieren von Daten in Anwendung-Assistenten in SQL Server Management Studio exportieren.|
| [Exportieren einer SQL Server-Datenbank in eine BACPAC-Datei, die mit SqlPackage](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md) | In diesem Lernprogramm erfahren Sie, wie Sie eine SQL Server-kompatible Datenbank in eine BACPAC-Datei, die mit dem Befehlszeilendienstprogramm SQLPackage.exe exportieren.|
| [Importieren einer BACPAC-Datei in SSMS mit Azure SQL-Datenbank](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md) | In diesem Lernprogramm erfahren Sie, wie Sie eine Datenbank in SQL Azure-Datenbank aus einer mit dem Exportieren von Daten in Anwendung-Assistenten in SQL Server Management Studio BACPAC-Datei importieren. |
| [Importieren einer BACPAC-Datei in Azure SQL-Datenbank mit SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md#import-from-a-bacpac-file-into-azure-sql-database-using-sqlpackage) | In diesem Lernprogramm erfahren Sie, wie Sie eine Datenbank in SQL Azure-Datenbank aus einer mit dem Befehlszeilendienstprogramm SQLPackage BACPAC-Datei importieren. |
| [Importieren einer BACPAC-Datei in Azure SQL-Datenbank mit dem Azure-portal](sql-database-import.md) | In diesem Lernprogramm erfahren Sie, wie Sie eine Datenbank in SQL Azure-Datenbank aus einer BACPAC-Datei importieren, die in einer Azure Blob mit dem Portal Azure gespeichert ist.|
| [Importieren einer BACPAC-Datei in Azure SQL-Datenbank mithilfe der PowerShell](sql-database-import-powershell.md) | In diesem Lernprogramm erfahren Sie, wie Sie eine Datenbank in SQL Azure-Datenbank aus einer mithilfe der PowerShell BACPAC-Datei importieren.|
| [Archivieren einer Azure SQL-Datenbank mit dem Azure-portal](sql-database-export.md#export-your-database) | In diesem Lernprogramm erfahren Sie, wie Sie eine SQL Azure-Datenbank in eine BACPAC-Datei mit der Azure-Portal archivieren. |
| [Archivieren einer SQL Azure-Datenbank mithilfe der PowerShell](sql-database-export-powershell.md) | In diesem Lernprogramm erfahren Sie, wie Sie eine SQL Azure-Datenbank in eine BACPAC-Datei mithilfe der PowerShell archivieren. |
| [Kopieren einer Azure SQL-Datenbank mit dem Azure-portal](sql-database-copy.md#copy-your-sql-database) | In diesem Lernprogramm erfahren Sie, wie Sie eine SQL Azure-Datenbank mithilfe des Azure-Portals kopieren. |
| [Kopieren einer SQL Azure-Datenbank mithilfe der PowerShell](sql-database-copy-powershell#copy-your-sql-database) | In diesem Lernprogramm erfahren Sie, wie Sie eine SQL Azure-Datenbank mithilfe der PowerShell kopieren. |
| [Kopieren einer mit Transact-SQL Azure SQL-Datenbank](sql-database-copy-transact-sql.md#copy-your-sql-database) | In diesem Lernprogramm erfahren Sie, wie eine mit Transact-SQL Azure SQL-Datenbank zu kopieren. |
||||

##<a name="develop"></a>Entwickeln

Die folgenden Lernprogramme erfahren Sie mehr zur [Entwicklung von SQL-Datenbank](sql-database-develop-overview.md) und die Verwendung von [Connectivity-Bibliotheken](sql-database-libraries.md).

| Lernprogramm  | Beschreibung  |
|---|---|---|
| [Verbinden Sie mit SQL-Datenbank mithilfe von .NET (c#)](sql-database-develop-dotnet-simple.md) | In diesem Lernprogramm erfahren Sie, wie die Verbindung mit einer SQL Azure-Datenbank mit c#. |
| [Verbinden Sie mit SQL-Datenbank mithilfe von Java](sql-database-develop-java-simple.md) | In diesem Lernprogramm erfahren Sie, wie die Verbindung mit einer SQL Azure-Datenbank mit Java. |
| [Verbinden Sie mit SQL-Datenbank mithilfe von Node.js](sql-database-develop-nodejs-simple.md) | In diesem Lernprogramm erfahren Sie, wie die Verbindung zu einer SQL Azure-Datenbank, die mithilfe von Node.js. |
| [Verbinden Sie mit SQL-Datenbank mithilfe von PHP](sql-database-develop-php-simple.md) | In diesem Lernprogramm erfahren Sie, wie die Verbindung mit einer SQL Azure-Datenbank mithilfe von PHP. |
| [Verbinden Sie mit SQL-Datenbank mithilfe von Python](sql-database-develop-python-simple.md) | In diesem Lernprogramm erfahren Sie, wie die Verbindung zu einer SQL Azure-Datenbank, die mithilfe von Python. |
| [Verbinden Sie mit SQL-Datenbank mithilfe von Ruby](sql-database-develop-ruby-simple.md) | In diesem Lernprogramm erfahren Sie, wie die Verbindung zu einer SQL Azure-Datenbank, die mithilfe von Ruby. |
||||
 
## <a name="database-access"></a>Zugriff auf die Datenbank

Die folgenden Lernprogramme erfahren Sie mehr über das [Erstellen und Verwalten von Benutzernamen und Benutzern](sql-database-manage-logins.md).

| Lernprogramm  | Beschreibung  |
|---|---|---|
| [Erstellen einer Regel für Server Ebene Firewall Azure SQL-Datenbank mit dem Azure-portal](sql-database-configure-firewall-settings.md)  | In diesem Lernprogramm erfahren Sie, wie eine SQL-Datenbank Server Ebene Firewall über das Azure-Portal konfigurieren.  |
| [Erstellen einer Datenbank Ebene Firewallregel, die mit Transact-SQL](sql-database-configure-firewall-settings-tsql.md#database-level-firewall-rules) | In diesem Lernprogramm erfahren Sie, wie Sie zum Erstellen einer Datenbank Ebene Firewallregel, die mit Transact-SQL.|
| [Mit Transact-SQL Server-Ebene Firewallregeln verwalten](sql-database-configure-firewall-settings-tsql.md#manage-server-level-firewall-rules-through-transact-sql) | In diesem Lernprogramm erfahren Sie, wie eine mit Transact-SQL Azure SQL-Datenbank Server-Level-Firewall zu verwalten.|
| [Mithilfe der PowerShell-Server Ebene Firewall-Regeln verwalten](sql-database-configure-firewall-settings-powershell.md#manage-firewall-rules-using-powershell) | In diesem Lernprogramm erfahren Sie, wie eine Firewall auf Server-Ebene Azure SQL-Datenbank mithilfe der PowerShell verwaltet werden.|
| [Verwenden die REST-API Server Ebene Firewallregeln verwalten](sql-database-configure-firewall-settings-rest.md#manage-firewall-rules-using-the-service-management-rest-api) | In diesem Lernprogramm erfahren Sie zum Verwalten einer Azure SQL-Datenbank Server Ebene Firewalls mithilfe der API zurücksetzen.|
| [Herstellen einer Verbindung mit einer Ebene Server Hauptbenutzer Login Azure SQL-Datenbank mit](sql-database-get-started-security.md#connect-to-azure-sql-database-using-a-server-level-principal-login)| In diesem Lernprogramm erfahren Sie, wie mit Azure SQL-Datenbank mithilfe eines Servers Ebene Hauptbenutzer Benutzernamens verbinden.|
| [Datenbankzugriff auf einen Benutzernamen] (sql-database-manage-logins.md#granting-database-access-to-a-login() | In diesem Lernprogramm erfahren Sie, wie Sie den Zugriff auf die Datenbank auf einen Server Ebene Benutzernamen gewähren.|
| [Erstellen Sie Neuer Datenbankbenutzer SSMS verwenden](sql-database-get-started-security.md#create-new-database-user-using-ssms) | In diesem Lernprogramm erfahren Sie, wie Sie zum Erstellen eines neuen Datenbankbenutzers in einer vorhandenen Datenbank SSMS verwenden.|
| [Neue Datenbank Db_owner Benutzerberechtigungen gewähren](sql-database-get-started-security.md#grant-new-database-user-dbowner-permissions) | In diesem Lernprogramm erfahren Sie, wie Sie eine vorhandene Datenbank Db_owner Benutzerberechtigungen erteilen.|
| [Verbinden Sie mit Azure SQL-Datenbank als Benutzer](sql-database-get-started-security.md#connect-to-azure-sql-database-as-a-user) | In diesem Lernprogramm erfahren Sie, wie die Verbindung mit einer SQL Azure-Datenbank mit einem Datenbank-Level-Benutzerkonto.|
||||


## <a name="data-security"></a>Datenschutz

Die folgenden Lernprogramme erfahren Sie mehr zur [Sicherheit von Azure SQL-Datenbankdaten](sql-database-security.md). 

| Lernprogramm  | Beschreibung  |
|---|---|---|
| [Aktivieren der Erkennung für die Datenbank mit dem Azure-portal](sql-database-threat-detection-get-started.md#set-up-threat-detection-for-your-database) | In diesem Lernprogramm erfahren Sie, wie Erkennung im Azure-Portal für die Datenbank einrichten.|
| [Secure vertrauliche Daten Einzelhandelsabonnements immer verschlüsselt.](sql-database-always-encrypted-azure-key-vault.md) | In diesem Lernprogramm verwenden Sie den Assistenten immer verschlüsselt sensible Daten in einer SQL Azure-Datenbank gesichert.|
| [Sichere Senstive-Daten, die mit transparenten Daten-Verschlüsselung](https://msdn.microsoft.com/library/dn948096.aspx)| In diesem Lernprogramm erfahren Sie, wie mit transparenten Daten Verschlüsselung um Senstive Daten zu schützen.|
| [Verschlüsseln einer Datenspalte](https://msdn.microsoft.com/library/ms179331.aspx)| In diesem Lernprogramm erfahren Sie, wie Sie eine Spalte mit Daten mithilfe von Transact-SQL verschlüsseln.|
| [Dynamische Daten Masking einrichten](sql-database-dynamic-data-masking-get-started.md#set-up-dynamic-data-masking-for-your-database-using-the-azure-portal)  | In diesem Lernprogramm erfahren Sie, wie dynamische Daten Masking für die SQL Azure-Datenbank einrichten. |
||||

## <a name="business-continuity-and-query-scale-out"></a>Geschäftskontinuität und zu verkleinern Abfrage

Die folgenden Lernprogramme lernen Sie, wie Sie mit [Geo-wiederherstellen und aktiven Geo-Replikation](sql-database-business-continuity.md) zu Reccover von Fehlern, für die Geschäftskontinuität und für eine Abfrage Skalierung.

| Lernprogramm  | Beschreibung  |
|---|---|---|
| [Wiederherstellen einer SQL Azure-Datenbank zu einem vorherigen Punkt Zeitpunkt mit dem Azure-Portal](sql-database-point-in-time-restore-portal.md)| In diesem Lernprogramm erfahren Sie, wie Sie Ihre Datenbank zu einem früheren Zeitpunkt über das Azure-Portal wiederherstellen.|
| [Wiederherstellen einer SQL Azure-Datenbank zu einem vorherigen Punkt Zeitpunkt mit PowerShell](sql-database-point-in-time-restore-powershell.md) | In diesem Lernprogramm erfahren Sie, wie Sie Ihre Datenbank zu einem früheren Zeitpunkt mithilfe der PowerShell wiederherstellen|
| [Wiederherstellen einer gelöschten SQL Azure-Datenbank mithilfe der Azure-Portal](sql-database-restore-deleted-database-portal.md) | In diesem Lernprogramm erfahren Sie, wie Sie zum Wiederherstellen einer gelöschten Datenbank über das Azure-Portal.|
| [Wiederherstellen einer gelöschten SQL Azure-Datenbank mithilfe der PowerShell](sql-database-restore-deleted-database-powershell.md) | In diesem Lernprogramm erfahren Sie, wie Sie zum Wiederherstellen einer gelöschten Datenbank mithilfe der PowerShell.|
| [Konfigurieren von Geo-Replikation für Azure SQL-Datenbank mit dem Azure-portal](sql-database-geo-replication-portal.md)| In diesem Lernprogramm erfahren Sie, wie aktiven Geo-Replikation mithilfe des Azure-Portals zu konfigurieren.|
| [Konfigurieren von Geo-Replikation für Azure SQL-Datenbank mithilfe der PowerShell](sql-database-geo-replication-powershell.md)| In diesem Lernprogramm erfahren Sie, wie Sie Konfigurieren der aktiven Geo-Replikation mithilfe der PowerShell.|
| [Konfigurieren von Geo-Replikation für mit Transact-SQL Azure SQL-Datenbank](sql-database-geo-replication-transact-sql.md)| In diesem Lernprogramm erfahren Sie, wie Sie Konfigurieren der aktiven Geo-Replikation mithilfe von Transact-SQL.|
| [Einleiten eines geplanten oder ungeplanten Failovers für Azure SQL-Datenbank mit dem Azure-portal](sql-database-geo-replication-failover-portal.md) | In diesem Lernprogramm erfahren Sie, wie Failover an einem Geo repliziert sekundäre Azure-Portal verwenden.|
| [Einleiten eines geplanten oder ungeplanten Failovers für Azure SQL-Datenbank mithilfe der PowerShell](sql-database-geo-replication-failover-powershell.md) | In diesem Lernprogramm erfahren Sie, wie Failover an einem Geo repliziert sekundäre mithilfe der PowerShell.|
| [Einleiten eines geplanten oder ungeplanten Failovers für mit Transact-SQL Azure SQL-Datenbank](sql-database-geo-replication-failover-transact-sql.md) | In diesem Lernprogramm erfahren Sie, wie Sie Failover an einem Geo repliziert sekundäre mit Transact-SQL.|
||||

## <a name="data-sync"></a>Synchronisieren von Daten

In diesem Lernprogramm erfahren Sie mehr über die [Daten synchronisieren](http://download.microsoft.com/download/4/E/3/4E394315-A4CB-4C59-9696-B25215A19CEF/SQL_Data_Sync_Preview.pdf).

| Lernprogramm  | Beschreibung  |
|---|---|---| 
| [Erste Schritte mit SQL Azure-Daten synchronisieren (Preview)](sql-database-get-started-sql-data-sync.md)  | In diesem Lernprogramm erfahren Sie die Grundlagen der Verwenden des klassischen Azure-Portals Daten synchronisieren mit SQL Azure. |
||||

## <a name="next-steps"></a>Nächste Schritte

[Untersuchen der SQL Azure-Datenbank Lösung schnellen Starts](sql-database-solution-quick-starts.md)

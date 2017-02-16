<properties
    pageTitle="Alle Themen für SQL Data Warehouse Dienst | Microsoft Azure"
    description="Tabelle aller Themen für den Dienst Azure benannte SQL Data Warehouse, die auf http://azure.microsoft.com/documentation/articles/, Titel und Beschreibung vorhanden sind."
    services="sql-data-warehouse"
    documentationCenter=""
    authors="barbkess"
    manager="jhubbard"
    editor="MightyPen"/>

<tags
    ms.service="sql-data-warehouse"
    ms.workload="sql-data-warehouse"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="barbkess"/>


# <a name="all-topics-for-azure-sql-data-warehouse-service"></a>Alle Themen Azure SQL-Data Warehouse-Dienst

Dieses Thema enthält eine Liste jeder direkt in den Dienst **Die Data Warehouse SQL** Azure zutreffende Thema. Sie können diese Webseite nach Schlüsselwörtern suchen, mithilfe von **STRG + F**, um den aktuellen relevante Themen zu finden.




## <a name="new"></a>Neu

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 1 | [SQL Data Warehouse Sicherungskopien](sql-data-warehouse-backups.md) | Lernen Sie SQL Data Warehouse integrierten Datenbanksicherungskopien, mit die Sie eine Azure SQL-Data Warehouse auf einen Wiederherstellungspunkt oder ein anderes geografische Region wiederherstellen können. |


## <a name="updated-articles-sql-data-warehouse"></a>Aktualisierte Artikel, SQL Data Warehouse

Dieser Abschnitt listet Artikeln, die vor kurzem aktualisiert wurden, in dem die Aktualisierung wurde, große oder erheblichen. Für jede aktualisierte Artikel ist ein grober Ausschnitt des Texts hinzugefügten Abzug angezeigt. Artikel wurden in den Datumsbereich **2016-08-22** zu **2016-10-05**aktualisiert.

| &nbsp; | Artikel | Aktualisierte Text, Codeausschnitt | Wann aktualisiert |
| --: | :-- | :-- | :-- |
| 2 | [Laden Sie Daten aus Azure Blob-Speicher in SQL Data Warehouse (PolyBase)](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md) | /-Zum Nachverfolgen von Bytes und Dateien SELECT r.command, s.request_id, r.status, Anzahl (distinct Input_name) als Nbr_files, addieren (s.bytes_processed) / 1024/1024 als Gb_processed aus sys.dm_pdw_exec_requests r innere Verknüpfung sys.dm_pdw_dms_external_work s auf r.request_id = s.request_id WHERE R. Beschriftung = ' CTAS: Cso laden. DimProduct ' oder R. Beschriftung = ' CTAS: Cso laden. GROUP BY r.command 'FactOnlineSales, s.request_id, r.status ORDER BY Nbr_files Desc, Gb_processed Desc;  | 2016-09-07 |
| 3 | [SQL Data Warehouse wiederherstellen](sql-data-warehouse-restore-database-overview.md) | **Kann ich ein angehalten Datawarehouse wiederherstellen?** Um ein Datawarehouse wiederherzustellen, der unterbrochen wurde, müssen Sie zuerst wieder online zu schalten. Nachdem das Datawarehouse wieder online ist, müssen Sie sieben Tage der Wiederherstellungspunkte zur Auswahl. **Wiederherstellen von Geo redundante Region** Wenn Sie den Geo redundante Speicher verwenden, können Sie das Datawarehouse zu Ihrem Datencenter für in einem anderen geografische Region wiederherstellen. Das Datawarehouse wird von der letzten tägliche Sicherung wiederhergestellt. **Wiederherstellen der Zeitachse** Sie können eine Datenbank innerhalb der letzten sieben Tage zu einem beliebigen Wiederherstellungspunkt wiederherstellen. Momentaufnahmen starten Sie alle vier bis acht Stunden und stehen sieben Tage lang. Wenn eine Momentaufnahme älter als sieben Tage ist, es abgelaufen ist und seine Wiederherstellungspunkt ist nicht mehr verfügbar. **Wiederherstellen von Kosten** Der Speicher Gebühr für das wiederhergestellte Datawarehouse ist in der Geschwindigkeit Azure Premium Speicher in Rechnung gestellt. Wenn Sie ein wiederhergestellten Datawarehouse anhalten, unterliegen Sie für die Speicherung Rate Azure Premium Storage. Der Vorteil von anhalten ist, dass Sie nicht Gebühr sind. | 2016-09-29 |





## <a name="get-started"></a>Erste Schritte

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 4 | [Authentifizierung in SQL Azure Datawarehouse](sql-data-warehouse-authentication.md) | Azure Active Directory (AAD) und SQL Server-Authentifizierung in Azure SQL-Data Warehouse. |
| 5 | [Bewährte Methoden für Azure SQL-Data Warehouse](sql-data-warehouse-best-practices.md) | Empfehlungen sowie optimale Methoden, die Sie, wie Sie kennen sollten entwickeln Lösungen für Azure SQL-Data Warehouse. Diese hilft Ihnen erfolgreich durchgeführt werden. |
| 6 | [Treiber für SQL Azure Datawarehouse](sql-data-warehouse-connection-strings.md) | Verbindungszeichenfolgen und Treiber für SQL Data Warehouse |
| 7 | [Verbinden Sie mit SQL Azure Datawarehouse](sql-data-warehouse-connect-overview.md) | So suchen Sie den Server und die Verbindungszeichenfolge Zeichenfolge für Ihre Azure SQL-Data Warehouse |
| 8 | [Analysieren von Daten mit Azure Computer-Schulung](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md) | Formular mit Azure maschinellen Learning einer Vorhersage maschinellen learning Modell basierend auf den in Azure SQL-Data Warehouse gespeicherten Daten erstellen. |
| 9 | [Abfrage Azure SQL Data Warehouse (Sqlcmd)](sql-data-warehouse-get-started-connect-sqlcmd.md) | Verwenden von Abfragen Azure SQL-Data Warehouse mit der Befehlszeilenprogramms Sqlcmd. |
| 10 | [Erstellen Sie eine SQL Data Warehouse-Datenbank mithilfe von Transact-SQL (TSQL)](sql-data-warehouse-get-started-create-database-tsql.md) | Informationen Sie zum Erstellen einer Azure SQL-Data Warehouse mit TSQL |
| 11 | [So erstellen Sie ein Ticket Support für SQL Data Warehouse](sql-data-warehouse-get-started-create-support-ticket.md) | Informationen zum Support-Ticket in Azure SQL-Data Warehouse zu erstellen. |
| 12 | [Laden Sie die Daten mit Factory Azure-Daten](sql-data-warehouse-get-started-load-with-azure-data-factory.md) | Informationen Sie zum Laden von Daten mit Azure Data Factory |
| 13 | [Laden Sie die Daten mit PolyBase in SQL Data Warehouse](sql-data-warehouse-get-started-load-with-polybase.md) | Erfahren Sie, was PolyBase ist und wie Sie es für Data Warehouse Szenarien verwenden. |
| 14 | [Erstellen einer SQL Azure Datawarehouse](sql-data-warehouse-get-started-provision.md) | Informationen Sie zum Erstellen einer Azure SQL-Data Warehouse Azure-Portal |
| 15 | [Erstellen von SQL Data Warehouse mithilfe der PowerShell](sql-data-warehouse-get-started-provision-powershell.md) | Erstellen Sie mithilfe der PowerShell SQL Data Warehouse |
| 16 | [Visualisieren von Daten mit Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md) | Visualisieren von SQL Data Warehouse Daten mit Power BI |
| 17 | [Abfrage SQL Azure Datawarehouse (Visual Studio)](sql-data-warehouse-query-visual-studio.md) | Abfrage SQL Datawarehouse mit Visual Studio. |



## <a name="develop"></a>Entwickeln

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 18 | [Transaktionen für SQL Data Warehouse optimieren](sql-data-warehouse-develop-best-practices-transactions.md) | Bewährte Methode Anleitungen effiziente Transaktion Aktualisierungen in Azure SQL-Data Warehouse schreiben |
| 19 | [Parallelität und Arbeitsbelastung Management in SQL Data Warehouse](sql-data-warehouse-develop-concurrency.md) | Grundlegendes zu Parallelität und Arbeitsbelastung Management in Azure SQL-Data Warehouse für die Entwicklung von Lösungen. |
| 20 | [Erstellen Sie die Tabelle als SELECT-Anweisung (CTAS) in SQL Datawarehouse](sql-data-warehouse-develop-ctas.md) | Tipps für die Entwicklung von Lösungen mit der Tabelle erstellen als select (CTAS)-Anweisung in Azure SQL-Data Warehouse Codierung. |
| 21 | [Dynamisches SQL im SQL Datawarehouse](sql-data-warehouse-develop-dynamic-sql.md) | Tipps zur Verwendung von dynamischen SQL Azure SQL-Data Warehouse für die Entwicklung von Lösungen. |
| 22 | [Gruppieren Sie nach Optionen in SQL Data Warehouse](sql-data-warehouse-develop-group-by-options.md) | Tipps für die Implementierung von Gruppe von Optionen in Azure SQL-Data Warehouse für die Entwicklung von Lösungen. |
| 23 | [Verwenden von Etiketten Urkunde Abfragen in SQL Data Warehouse](sql-data-warehouse-develop-label.md) | Tipps zur Verwendung von Etiketten Urkunde Abfragen in Azure SQL-Data Warehouse für die Entwicklung von Lösungen. |
| 24 | [Schleifen in SQL Datawarehouse](sql-data-warehouse-develop-loops.md) | Tipps für Transact-SQL-Schleifen und ersetzt Cursor in Azure SQL-Data Warehouse für die Entwicklung von Lösungen. |
| 25 | [Gespeicherte Prozeduren in SQL Data Warehouse](sql-data-warehouse-develop-stored-procedures.md) | Tipps für die Implementierung von gespeicherter Prozeduren in Azure SQL-Data Warehouse für die Entwicklung von Lösungen. |
| 26 | [Transaktionen in SQL Datawarehouse](sql-data-warehouse-develop-transactions.md) | Tipps für die Implementierung von Transaktionen in Azure SQL-Data Warehouse für die Entwicklung von Lösungen. |
| 27 | [Benutzerdefinierte Schemas im SQL Data Warehouse](sql-data-warehouse-develop-user-defined-schemas.md) | Tipps für die Verwendung von Schemas Transact-SQL Azure SQL-Data Warehouse für die Entwicklung von Lösungen. |
| 28 | [Zuweisen von Variablen in SQL Data Warehouse](sql-data-warehouse-develop-variable-assignment.md) | Tipps für das Zuweisen von Variablen von Transact-SQL Azure SQL-Data Warehouse für die Entwicklung von Lösungen. |
| 29 | [Ansichten im SQL Datawarehouse](sql-data-warehouse-develop-views.md) | Tipps zur Verwendung von Ansichten Transact-SQL Azure SQL-Data Warehouse für die Entwicklung von Lösungen. |
| 30 | [Entwerfen von Entscheidungen und Codierung Techniken für SQL Data Warehouse](sql-data-warehouse-overview-develop.md) | Entwicklung Konzepte, Entwurf Entscheidungen, Empfehlungen und Codierung Techniken für SQL Data Warehouse. |



## <a name="manage"></a>Verwalten

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 31 | [Verwalten von berechnen Power in Azure SQL-Data Warehouse (Übersicht)](sql-data-warehouse-manage-compute-overview.md) | Leistung Skalierung-Funktionen in Azure SQL-Data Warehouse. Skalieren Sie indem Sie DWUs anpassen oder Anhalten Sie und fortsetzen Sie berechnen Ressourcen zum Speichern von Kosten. |
| 32 | [Verwalten von berechnen Power in Azure SQL-Data Warehouse (Azure Portal)](sql-data-warehouse-manage-compute-portal.md) | Azure Portals Aufgaben zum Verwalten von Power zu berechnen. Maßstab berechnen Ressourcen durch DWUs anpassen. Oder anhalten und fortsetzen Ressourcen zum Speichern von Kosten zu berechnen. |
| 33 | [Verwalten von berechnen Power in Azure SQL-Data Warehouse (PowerShell)](sql-data-warehouse-manage-compute-powershell.md) | PowerShell-Aufgaben zum Verwalten von Power zu berechnen. Maßstab berechnen Ressourcen durch DWUs anpassen. Oder anhalten und fortsetzen Ressourcen zum Speichern von Kosten zu berechnen. |
| 34 | [Verwalten von berechnen Power in Azure SQL Data Warehouse (REST)](sql-data-warehouse-manage-compute-rest-api.md) | PowerShell-Aufgaben zum Verwalten von Power zu berechnen. Maßstab berechnen Ressourcen durch DWUs anpassen. Oder anhalten und fortsetzen Ressourcen zum Speichern von Kosten zu berechnen. |
| 35 | [Verwalten von berechnen Power in Azure SQL-Data Warehouse (T-SQL)](sql-data-warehouse-manage-compute-tsql.md) | Transact-SQL (T-SQL) Aufgaben Skalierung Leistung durch DWUs anpassen. Sparen Sie Kosten, indem Sie dieselbe Skalierung wieder Zeiten nicht Höchstwert. |
| 36 | [Überwachen Sie Ihre Arbeitsbelastung DMVs verwenden](sql-data-warehouse-manage-monitor.md) | Informationen Sie zum Überwachen der Arbeitsbelastung DMVs verwenden. |
| 37 | [Verwalten von Datenbanken in Azure SQL-Data Warehouse](sql-data-warehouse-overview-manage.md) | Übersicht über die Data Warehouse SQL-Datenbanken verwalten. Enthält Tools zum Projektmanagement, DWUs und die Skalierung der Systemleistung abfrageleistung, Einrichten von Richtlinien für gute Sicherheit und Wiederherstellen einer Datenbank von Beschädigung der Daten oder von einem regionalen Ausfall Problembehandlung. |
| 38 | [Überwachen der Benutzerabfragen in Azure SQL-Data Warehouse](sql-data-warehouse-overview-manage-user-queries.md) | Übersicht über die Aspekte, bewährte Methoden, und Aufgaben für die Überwachung Benutzerabfragen in Azure SQL-Data Warehouse |
| 39 | [SQL Data Warehouse wiederherstellen](sql-data-warehouse-restore-database-overview.md) | Übersicht über die Datenbank Wiederherstellungsoptionen zum Wiederherstellen einer Datenbank in Azure SQL-Data Warehouse. |
| 40 | [Wiederherstellen einer SQL Azure Datawarehouse (Portal)](sql-data-warehouse-restore-database-portal.md) | Azure Portals Aufgaben zum Wiederherstellen einer Azure SQL-Data Warehouse. |
| 41 | [Wiederherstellen einer SQL Azure Datawarehouse (PowerShell)](sql-data-warehouse-restore-database-powershell.md) | Zum Wiederherstellen einer Azure SQL-Data Warehouse PowerShell-Aufgaben. |
| 42 | [Wiederherstellen einer SQL Azure Datawarehouse (REST-API)](sql-data-warehouse-restore-database-rest-api.md) | REST-API Aufgaben zum Wiederherstellen einer Azure SQL-Data Warehouse. |



## <a name="tables-and-indexes"></a>Tabellen und Indizes

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 43 | [Datentypen für Tabellen in SQL Data Warehouse](sql-data-warehouse-tables-data-types.md) | Erste Schritte mit Datentypen für Azure SQL-Data Warehouse-Tabellen. |
| 44 | [Verteilen von Tabellen in SQL Data Warehouse](sql-data-warehouse-tables-distribute.md) | Erste Schritte mit Tabellen in Azure SQL-Data Warehouse verteilen. |
| 45 | [Tabellen in SQL Data Warehouse Indizierung](sql-data-warehouse-tables-index.md) | Erste Schritte mit der Tabelle, die in Azure SQL-Data Warehouse Indizierung. |
| 46 | [Übersicht über die Tabellen in SQL Data Warehouse](sql-data-warehouse-tables-overview.md) | Erste Schritte mit Azure SQL-Data Warehouse-Tabellen. |
| 47 | [Partitionierungstabellen in SQL Data Warehouse](sql-data-warehouse-tables-partition.md) | Erste Schritte mit Tabelle in Azure SQL-Data Warehouse Verteilung. |
| 48 | [Verwalten von Statistiken für Tabellen in SQL Data Warehouse](sql-data-warehouse-tables-statistics.md) | Erste Schritte mit Tabellen in Azure SQL-Data Warehouse Statistik. |
| 49 | [Temporäre Tabellen in SQL Data Warehouse](sql-data-warehouse-tables-temporary.md) | Erste Schritte mit temporären Tabellen in Azure SQL-Data Warehouse. |



## <a name="integrate"></a>Integrieren

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 50 | [Verwenden von Factory Azure-Daten mit SQL Datawarehouse](sql-data-warehouse-integrate-azure-data-factory.md) | Tipps zur Verwendung von Azure Daten Factory (ADF) mit Azure SQL-Data Warehouse für die Entwicklung von Lösungen. |
| 51 | [Verwenden von Azure-Computern Learning mit SQL Datawarehouse](sql-data-warehouse-integrate-azure-machine-learning.md) | Lernprogramm für die Verwendung von Azure maschinellen Learning mit Azure SQL-Data Warehouse für die Entwicklung von Lösungen. |
| 52 | [Verwenden von Azure Stream Analytics mit SQL Datawarehouse](sql-data-warehouse-integrate-azure-stream-analytics.md) | Tipps zur Verwendung von Azure Stream Analytics mit Azure SQL-Data Warehouse für die Entwicklung von Lösungen. |
| 53 | [Verwenden von Power BI mit SQL Datawarehouse](sql-data-warehouse-integrate-power-bi.md) | Tipps zur Verwendung von Power BI mit Azure SQL-Data Warehouse für die Entwicklung von Lösungen. |
| 54 | [Nutzen Sie andere Services mit SQL Data Warehouse](sql-data-warehouse-overview-integrate.md) | Tools und Partner mit Lösungen, die mit SQL Data Warehouse integriert werden soll.  |



## <a name="load"></a>Beim Laden

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 55 | [Laden Sie Daten aus Azure Blob-Speicher in Azure SQL-Data Warehouse (Azure Daten vorinstalliert)](sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md) | Informationen Sie zum Laden von Daten mit Azure Data Factory |
| 56 | [Laden Sie Daten aus Azure Blob-Speicher in SQL Data Warehouse (PolyBase)](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md) | Erfahren Sie, wie PolyBase verwenden, um Daten aus Azure Blob-Speicher in SQL Data Warehouse zu laden. Einige Tabellen aus öffentlichen Daten in das Schema "Contoso" Retail Data Warehouse zu laden. |
| 57 | [Laden Sie die Daten aus SQL Server in Azure SQL-Data Warehouse (AZCopy)](sql-data-warehouse-load-from-sql-server-with-azcopy.md) | Verwendet Bcp So exportieren Sie Daten aus SQL Server in flachen Dateien, AZCopy zum Importieren von Daten in Azure Blob-Speicher und PolyBase, um die Daten in Azure SQL-Data Warehouse Aufnahme an. |
| 58 | [Laden Sie die Daten aus SQL Server in Azure SQL-Data Warehouse (flachen Dateien)](sql-data-warehouse-load-from-sql-server-with-bcp.md) | Für eine kleine Datengröße verwendet Bcp Daten aus SQL Server in flachen Dateien exportieren und importieren Sie die Daten direkt in Azure SQL-Data Warehouse ein. |
| 59 | [Laden von Daten aus SQL Server in Azure SQL Data Warehouse (SSIS)](sql-data-warehouse-load-from-sql-server-with-integration-services.md) | Es wird gezeigt, wie ein Paket SQL Server Integration Services (SSIS) zum Verschieben von Daten aus einer Vielzahl von Datenquellen in SQL Data Warehouse zu erstellen. |
| 60 | [Laden Sie die Daten mit PolyBase in SQL Data Warehouse](sql-data-warehouse-load-from-sql-server-with-polybase.md) | Verwendet Bcp So exportieren Sie Daten aus SQL Server zu flachen Dateien AZCopy zum Importieren von Daten in Azure Blob-Speicher und PolyBase, um die Daten in Azure SQL-Data Warehouse Aufnahme an. |
| 61 | [Leitfaden für die Verwendung von PolyBase in SQL Data Warehouse](sql-data-warehouse-load-polybase-guide.md) | Richtlinien und Empfehlungen für PolyBase in SQL Data Warehouse Szenarien verwenden. |
| 62 | [Laden Sie die Beispieldaten in SQL Data Warehouse](sql-data-warehouse-load-sample-databases.md) | Laden Sie die Beispieldaten in SQL Data Warehouse |
| 63 | [Laden Sie die Daten mit bcp](sql-data-warehouse-load-with-bcp.md) | Erfahren Sie, welche Bcp ist und wie Sie es für Data Warehouse Szenarien verwenden. |
| 64 | [Laden von Daten in Azure SQL-Data Warehouse](sql-data-warehouse-overview-load.md) | Erfahren Sie die häufige Szenarien in SQL Data Warehouse Laden von Daten aus. Hierzu gehören mit PolyBase, Azure Blob-Speicher, flachen Dateien und Datenträger Versand. Sie können auch Drittanbieter-Tools verwenden. |



## <a name="migrate"></a>Migrieren von

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 65 | [Migrieren von SQL-Code in SQL Data Warehouse](sql-data-warehouse-migrate-code.md) | Tipps für die Migration von SQL-Code in Azure SQL-Data Warehouse für die Entwicklung von Lösungen. |
| 66 | [Migrieren von Daten](sql-data-warehouse-migrate-data.md) | Tipps für die Migration von Daten in Azure SQL-Data Warehouse für die Entwicklung von Lösungen. |
| 67 | [Datawarehouse Migrationsprogramm (Preview)](sql-data-warehouse-migrate-migration-utility.md) | Migrieren Sie zu SQL Datawarehouse. |
| 68 | [Migrieren von Ihrem Schema zu SQL Data Warehouse](sql-data-warehouse-migrate-schema.md) | Tipps für die Migration von Ihrem Schemas in Azure SQL-Data Warehouse für die Entwicklung von Lösungen. |
| 69 | [Migrieren von Ihrer Lösung zu SQL Data Warehouse](sql-data-warehouse-overview-migrate.md) | Hinweise zur Migration für Ihre Lösung zur Azure SQL-Data Warehouse Plattform Onlineschalten. |



## <a name="partners"></a>Partner

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 70 | [SQL Data Warehouse Business Intelligence Partner](sql-data-warehouse-partner-business-intelligence.md) | Listen mit Drittanbieter-Business Intelligence-Partner mit Lösungen, die SQL Data Warehouse unterstützen. |
| 71 | [SQL Data Warehouse Daten Integrationspartner](sql-data-warehouse-partner-data-integration.md) | Listen mit Drittanbieter-Partner mit Daten Integration Lösungen, die Azure SQL-Data Warehouse unterstützen. |
| 72 | [SQL Data Warehouse Daten Management Partner](sql-data-warehouse-partner-data-management.md) | Listen mit Daten von Dritten Management Partner mit Lösungen, die SQL Data Warehouse unterstützen. |



## <a name="reference"></a>Bezug

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 73 | [Verweis Themen SQL Data Warehouse](sql-data-warehouse-overview-reference.md) | Bezug von Links für SQL Data Warehouse. |
| 74 | [PowerShell-Cmdlets und REST-APIs für SQL Data Warehouse](sql-data-warehouse-reference-powershell-cmdlets.md) | Finden Sie die obersten PowerShell-Cmdlets für Azure SQL-Data Warehouse einschließlich anhalten und Fortsetzen einer Datenbank ein. |
| 75 | [Sprachelemente](sql-data-warehouse-reference-tsql-language-elements.md) | Liste mit Links zu Bezug Inhalt für die Transact-SQL-Sprache-Elemente für SQL Data Warehouse verwendet. |
| 76 | [Transact-SQL-Themen](sql-data-warehouse-reference-tsql-statements.md) | Enthält Links zu Bezug Inhalt für die Transact-SQL-Themen von SQL Data Warehouse verwendet. |
| 77 | [Systemansichten](sql-data-warehouse-reference-tsql-system-views.md) | Links zu System Ansichten Inhalte für SQL Data Warehouse. |



## <a name="security"></a>Sicherheit

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 78 | [Unterstützung für SQL Datawarehouse - Clients älterer Versionen für ü und dynamische Daten Masking](sql-data-warehouse-auditing-downlevel-clients.md) | Erfahren Sie mehr über SQL Data Warehouse zur Unterstützung für kompatible Clients für die Überwachung von Daten |
| 79 | [Überwachung in SQL Azure Datawarehouse](sql-data-warehouse-auditing-overview.md) | Erste Schritte mit Azure SQL-Data Warehouse Überwachung |
| 80 | [Erste Schritte mit transparenten Daten Verschlüsselung (TDE) in SQL Data Warehouse](sql-data-warehouse-encryption-tde.md) | Transparent Data Verschlüsselung (TDE) in SQL Datawarehouse |
| 81 | [Erste Schritte mit transparenten Daten Verschlüsselung (TDE)](sql-data-warehouse-encryption-tde-tsql.md) | Transparent Data Verschlüsselung (TDE) in SQL Datawarehouse (T-SQL) |
| 82 | [Sichern einer Datenbank in SQL Data Warehouse](sql-data-warehouse-overview-manage-security.md) | Tipps zum Sichern einer Datenbank in Azure SQL-Data Warehouse für die Entwicklung von Lösungen. |



## <a name="miscellaneous"></a>Verschiedene

| &nbsp; | Titel | Beschreibung |
| --: | :-- | :-- |
| 83 | [Installieren Sie Visual Studio 2015 und SSDT für SQL Datawarehouse](sql-data-warehouse-install-visual-studio.md) | Installieren Sie Visual Studio und SQL Server-Entwicklung-Tools (SSDT) für SQL Azure Datawarehouse |
| 84 | [Migration Premium Speicher Details](sql-data-warehouse-migrate-to-premium-storage.md) | Anweisungen für die Migration ein vorhandenes SQL Data Warehouse zu Premium-Speicher |
| 85 | [Erste Schritte mit Erkennung](sql-data-warehouse-security-threat-detection.md) | Gewusst wie: Erste Schritte mit Erkennung |
| 86 | [Grenzwerte für SQL Data Warehouse Kapazität](sql-data-warehouse-service-capacity-limits.md) | Höchstwerte für Verbindungen, Datenbanken, Tabellen und Abfragen zur SQL Data Warehouse. |
| 87 | [Problembehandlung bei SQL Azure Datawarehouse](sql-data-warehouse-troubleshoot.md) | Problembehandlung bei SQL Azure Datawarehouse aus. |


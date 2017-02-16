<properties
   pageTitle="Problembehandlung bei SQL Azure Datawarehouse | Microsoft Azure"
   description="Problembehandlung bei SQL Azure Datawarehouse aus."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/30/2016"
   ms.author="sonyama;barbkess"/>

# <a name="troubleshooting-azure-sql-data-warehouse"></a>Problembehandlung bei SQL Azure Datawarehouse

In diesem Thema werden einige der häufigeren Fragen zur Problembehandlung, die wir von unseren Kunden hören aufgelistet.

## <a name="connecting"></a>Herstellen einer Verbindung

| Problem                              | Auflösung                                      |
| :----------------------------------| :---------------------------------------------- |
| Fehler bei der Anmeldung für Benutzer 'NT-AUTORITÄT\ANONYMOUS-Anmeldung'. (Microsoft SQL Server, Fehler: 18456) | Dieser Fehler tritt auf, wenn ein Benutzer AAD versucht, zu der master-Datenbank herstellen, aber hat kein Benutzers in das Master-Shape.  Zur Behebung dieses Problems entweder angeben SQL Data Warehouse zur Verbindungszeit herstellen, oder fügen Sie den Benutzer zur master-Datenbank erstellt werden soll.  [Übersicht über die Sicherheit][] finden Sie im Artikel weitere Details.|
|Der Server ist nicht auf die Datenbank "Master" im aktuellen Kontext für Sicherheit zugreifen Hauptbenutzer "MyUserName". Benutzer Standard-Datenbank kann nicht geöffnet werden. Fehler bei der Anmeldung. Fehler bei der Anmeldung für Benutzer 'MyUserName'. (Microsoft SQL Server, Fehler: 916) | Dieser Fehler tritt auf, wenn ein Benutzer AAD versucht, zu der master-Datenbank herstellen, aber hat kein Benutzers in das Master-Shape.  Zur Behebung dieses Problems entweder angeben SQL Data Warehouse zur Verbindungszeit herstellen, oder fügen Sie den Benutzer zur master-Datenbank erstellt werden soll.  [Übersicht über die Sicherheit][] finden Sie im Artikel weitere Details.|
| CTAIP zurück                        | Dieser Fehler kann auftreten, wenn ein Benutzername auf die master SQL Server-Datenbank, aber nicht in die Data Warehouse SQL-Datenbank erstellt wurde.  Wenn dieser Fehler auftreten, sehen Sie sich im Artikel [Übersicht über die Sicherheit][] .  In diesem Artikel wird erläutert, wie erstellen eine Anmeldung und der Benutzer auf Master-Shape, und klicken Sie dann zum Erstellen eines Benutzers in SQL Data Warehouse-Datenbank erstellen.|
| Von der Firewall blockiert                |Azure SQL-Datenbanken werden durch Server und Datenbank Ebene Firewalls sicherstellen geschützt nur bekannte IP-Adressen auf eine Datenbank zugreifen können. Die Firewalls sind standardmäßig, was bedeutet, dass Sie explizit aktiviert werden müssen und IP-Adresse, oder Bereich von Adressen sicher, bevor Sie eine Verbindung herstellen können.  Zum Konfigurieren der Firewalls für den Zugriff, folgen Sie den Schritten [Konfigurieren Server Firewall Access für Ihre Client-IP][] in der [Provisioning Anweisungen][].|
| Mit dem Tool oder Treiber kann keine Verbindung hergestellt werden | SQL Data Warehouse empfiehlt [SSMS][], [SSDT für Visual Studio 2015][]oder [Sqlcmd][] zum Abfragen von Daten verwenden. Weitere Informationen zu Treibern und Herstellen einer Verbindung mit SQL Data Warehouse finden Sie unter [Treiber für Azure SQL-Data Warehouse][] und [Verbindung herstellen mit Azure SQL-Data Warehouse][] Artikel.|


## <a name="tools"></a>Tools

| Problem                              | Auflösung                                      |
| :----------------------------------| :---------------------------------------------- |
| Visual Studio-Objekt-Explorer fehlt AAD Benutzer | Dies ist ein bekanntes Problem.  Anzeigen von Benutzer in [Kataloganzeige][]Problem zu umgehen.  Finden Sie unter [Authentifizierung in Azure SQL-Data Warehouse][] , um weitere Informationen zur Verwendung von Azure Active Directory mit SQL Data Warehouse.|

## <a name="performance"></a>Leistung

|  Problem                             | Auflösung                                      |
| :----------------------------------| :---------------------------------------------- |
| Behandlung von Leistungsproblemen  | Wenn Sie versuchen, mit eine bestimmte Abfrage behandeln, beginnen Sie mit [lernen, wie Sie Ihre Abfragen zu überwachen][].|
| Pläne und abfrageleistung beeinträchtigt ist häufig mit dem Ergebnis fehlende Statistiken   | Die am häufigsten verwendeten beeinträchtigt Leistung verursacht fehlen Statistiken auf Tabellen.  Finden Sie unter [Verwalten Tabelle Statistics] [ Statistics] Details zum Erstellen von Statistiken und warum diese für Ihre Veranstaltung wichtig sind.|
| Niedrig Parallelität / Abfragen in der Warteschlange   | Grundlegendes zu [Arbeitsbelastung Management][] ist wichtig, damit Sie verstehen, wie arbeitsspeicherzuteilung und Parallelität abwägen.|
| Wie bewährte Methoden implementiert wird.    | Platzieren Sie die optimale um zu starten, wenn Sie wissen, dass Methoden zur Verbesserung der Systemleistung Abfrage [SQL Data Warehouse bewährte Methoden][] Artikel ist.|
| Steigern der Leistung mit Skalierung  | Manchmal ist die Lösung zur Verbesserung der Leistung, einfach hinzuzufügen, dass weitere Power Abfragen durch [Skalieren Ihrer SQL Data Warehouse][]zu berechnen.|
| Als Ergebnis beeinträchtigt Index Qualität beeinträchtigt abfrageleistung | In einigen Fällen Abfragen können Verzögerung aufgrund [einer schlechten Columnstore Index Qualität][].  Weitere Informationen in diesem Artikel finden und wie Sie [zur Verbesserung der Qualität Segment Index neu zu erstellen][].|

## <a name="system-management"></a>Systemmanagement

|  Problem                             | Auflösung                                      |
| :----------------------------------| :---------------------------------------------- |
| Msg 40847: Konnte den Vorgang nicht ausführen, da der Server zulässige Datenbank Transaktionseinheit 45000 das Kontingent überschritten würde. | Verringern Sie die [DWU][] der Datenbank, die Sie erstellen möchten oder [Anfordern eines Kontingent erhöhen][].|
| Untersuchung läuft Leerzeichen Auslastung    | Finden Sie unter [Tabellengrößen][] zu verstehen, die Nutzung Platz in Ihrem System.|
| Hilfe bei der Verwaltung von Tabellen          | Finden Sie unter der [Tabelle Übersicht] [ Overview] Artikel Hilfe bei der Verwaltung von Tabellen.  Dieser Artikel enthält Links auch in ausführlichere Themen wie [Datentypen Tabelle][Data types], [Verteilen einer Tabelle][Distribute], [Indizieren einer Tabelle][Index], [eine-Tabelle teilen][Partition], [Verwalten Tabellenstatistiken] [ Statistics] und [temporäre Tabellen][Temporary].|

## <a name="polybase"></a>Polybase

|  Problem                             | Auflösung                                      |
| :----------------------------------| :---------------------------------------------- |
| UTF-8-Fehler                        |  PolyBase unterstützt zurzeit nur das Laden von Datendateien, die UTF-8 codiert wurden.  Hilfe zur Umgehung dieses Problems finden Sie unter [arbeiten, um die Anforderung PolyBase UTF-8][] .|
| Laden schlägt aufgrund von großen Zeilen   | Unterstützung für große Zeile ist derzeit nicht für Polybase verfügbar.  Dies bedeutet, dass die Tabelle 'varchar(max)', 'nvarchar(max)' oder VARBINARY(MAX) enthält, externe Tabellen verwendet werden können, Ihre Daten zu laden.  Lädt für große Zeilen gibt es zurzeit nur über Azure Data Factory (mit BCP), Azure Stream Analytics, SSIS, BCP oder die .NET SQLBulkCopy-Klasse unterstützt. PolyBase Unterstützung für große Zeilen wird in zukünftigen Versionen hinzugefügt werden.|
| Bcp Laden der Tabelle mit MAX-Datentyp schlägt fehl | Es ist ein bekanntes Problem die erfordert, dass am Ende der Tabelle in einigen Szenarien 'varchar(max)', 'nvarchar(max)' oder VARBINARY(MAX) platziert werden.  Versuchen Sie, Ihre maximale Anzahl von Spalten an das Ende der Tabelle zu verschieben.|

## <a name="differences-from-sql-database"></a>Unterschiede aus SQL-Datenbank

|  Problem                             | Auflösung                                      |
| :----------------------------------| :---------------------------------------------- |
| Nicht unterstützte Features der SQL-Datenbank  | Finden Sie unter [nicht unterstützte Tabellenfeatures][].|
| Nicht unterstützte Datentypen in SQL-Datenbank  | [Nicht unterstützte Datentypen][]finden Sie unter.|
| Löschen und aktualisieren Einschränkungen      | Finden Sie unter [Aktualisieren problemumgehungen][], [Löschen von problemumgehungen][] und [CTAS verwenden, um nicht unterstützte aktualisieren und löschen Syntax umgehen][].  |
| SERIENDRUCK-Anweisung wird nicht unterstützt.   | Finden Sie unter [Verbinden von problemumgehungen][].|
| Gespeicherte Prozedur Einschränkungen       | Finden Sie unter [gespeicherte Prozedur Einschränkungen][] , einige der Schwächen gespeicherten Prozeduren zu verstehen.|
| UDFs unterstützt SELECT-Anweisungen nicht | Dies ist eine aktuelle Beschränkung des unsere UDFs.  Die Syntax, die wir Unterstützung finden Sie unter [Erstellen (Funktion)][] .   |

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie sind wurden eine Lösung für Ihr Problem oben finden können nicht genutzt werden, hier einige weitere Ressourcen, die Sie versuchen können.

- [Blogs]
- [Anfragen zu Features]
- [Videos]
- [Katze Teamblogs]
- [Support-Ticket erstellen]
- [MSDN-forum]
- [Stapel Überlauf-forum]
- [Twitter]

<!--Image references-->

<!--Article references-->
[Übersicht über die Sicherheit]: ./sql-data-warehouse-overview-manage-security.md
[SSMS]: https://msdn.microsoft.com/library/mt238290.aspx
[SSDT für Visual Studio 2015]: ./sql-data-warehouse-install-visual-studio.md
[Treiber für SQL Azure Datawarehouse]: ./sql-data-warehouse-connection-strings.md
[Verbinden Sie mit SQL Azure Datawarehouse]: ./sql-data-warehouse-connect-overview.md
[Support-Ticket erstellen]: ./sql-data-warehouse-get-started-create-support-ticket.md
[Ihr SQL Datawarehouse Skalierung]: ./sql-data-warehouse-manage-compute-overview.md
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[Anfordern einer Kontingent erhöhen]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change 
[Lernen, wie Sie Ihre Abfragen zu überwachen.]: ./sql-data-warehouse-manage-monitor.md
[Bereitstellen von Anweisungen]: ./sql-data-warehouse-get-started-provision.md
[Konfigurieren des Zugriffs für Server-Firewall für Ihre Client-IP]: ./sql-data-warehouse-get-started-provision.md#create-a-new-azure-sql-server-level-firewall
[Bewährte Methoden für SQL Data Warehouse]: ./sql-data-warehouse-best-practices.md
[Tabellengrößen]: ./sql-data-warehouse-tables-overview.md#table-size-queries
[Nicht unterstützte Tabellenfeatures]: ./sql-data-warehouse-tables-overview.md#unsupported-table-features
[Nicht unterstützte Datentypen]: ./sql-data-warehouse-tables-data-types.md#unsupported-data-types
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[Beeinträchtigt Columnstore Index Qualität]: ./sql-data-warehouse-tables-index.md#causes-of-poor-columnstore-index-quality
[Index zur Verbesserung der Qualität Segment neu zu erstellen]: ./sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[Arbeitsbelastung management]: ./sql-data-warehouse-develop-concurrency.md
[Verwenden von CTAS zum umgehen, nicht unterstützte Syntax für aktualisieren und löschen]: ./sql-data-warehouse-develop-ctas.md#using-ctas-to-work-around-unsupported-features
[Problemumgehung aktualisieren]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-update-statements
[Problemumgehung löschen]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-delete-statements
[ZUSAMMENFÜHREN von problemumgehungen]: ./sql-data-warehouse-develop-ctas.md#replace-merge-statements
[Gespeicherte Prozedur Einschränkungen]: ./sql-data-warehouse-develop-stored-procedures.md#limitations
[Authentifizierung in SQL Azure Datawarehouse]: ./sql-data-warehouse-authentication.md
[Um die Anforderung PolyBase UTF-8 arbeiten]: ./sql-data-warehouse-load-polybase-guide.md#working-around-the-polybase-utf-8-requirement

<!--MSDN references-->
[Kataloganzeige]: https://msdn.microsoft.com/library/ms187328.aspx
[ERSTELLEN (FUNKTION)]: https://msdn.microsoft.com/library/mt203952.aspx
[Sqlcmd]: https://azure.microsoft.com/en-us/documentation/articles/sql-data-warehouse-get-started-connect-sqlcmd/

<!--Other Web references-->
[Blogs]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Katze Teamblogs]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Anfragen zu Features]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[MSDN-forum]: https://social.msdn.microsoft.com/Forums/home?forum=AzureSQLDataWarehouse
[Stapel Überlauf-forum]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Videos]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse


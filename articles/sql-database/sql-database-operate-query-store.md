<properties
   pageTitle="Betriebssystem Abfrage Store in SQL Azure-Datenbank"
   description="Erfahren Sie, wie die Abfrage Store in Azure SQL-Datenbank ausgeführt werden."
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
   ms.tgt_pltfrm="sqldb-performance"
   ms.workload="data-management"
   ms.date="08/16/2016"
   ms.author="carlrab"/>

# <a name="operating-the-query-store-in-azure-sql-database"></a>Betrieb im Store Abfrage in SQL Azure-Datenbank 

Abfrage-Store in Azure ist eine vollständig verwaltete Datenbank-Funktion, die kontinuierlich sammelt und bietet detaillierte Verlaufsinformationen zu alle Abfragen. Sie können über Abfrage-Speicher wie ein Flugzeug des Flight Daten Aufzeichnung vorstellen, die erheblich vereinfacht die Problembehandlung beide für Cloud Leistung von Abfragen und lokale Kunden. In diesem Artikel wird erläutert, bestimmte Aspekte des Abfrage-Store in Azure Betrieb. Mit diesen Abfragedaten vorab zusammengestellten können schnell diagnostizieren und Beheben von Leistungsproblemen und daher mehr Zeit auf ihr Geschäft konzentrieren. 

Abfrage-Store wurde seit November 2015 [Global verfügbar](https://azure.microsoft.com/updates/general-availability-azure-sql-database-query-store/) in Azure SQL-Datenbank. Speichern der Abfrage ist die Grundlage für die Leistungsanalyse und Optimieren von Features wie [SQL-Datenbank Advisor und Leistungsdashboard](https://azure.microsoft.com/updates/sqldatabaseadvisorga/). Zum Zeitpunkt der Veröffentlichung in diesem Artikel läuft Abfrage Store mehr als 200.000 Benutzerdatenbanken in Azure, im Abfrage-bezogene Informationen für mehrere Monate, ohne Unterbrechung sammeln.

> [AZURE.IMPORTANT] Microsoft führt eine Abfrage Store für alle SQL Azure-Datenbanken (vorhandenen und neuen) aktivieren. 

## <a name="optimal-query-store-configuration"></a>Optimale Abfrage Store Konfiguration

In diesem Abschnitt werden die Standardwerte für optimale Konfiguration, die entwickelt wurden, um sicherzustellen, dass zuverlässigen Betrieb des Abfrage-Store und abhängige Features, wie etwa [SQL-Datenbank Advisor und Leistungsdashboard](https://azure.microsoft.com/updates/sqldatabaseadvisorga/). Standard-Konfiguration ist für kontinuierliche Datensammlung optimiert, die minimalen Zeitaufwand in einem OFF/READ_ONLY ist.

| Konfiguration | Beschreibung | Standard | Kommentar |
| ------------- | ----------- | ------- | ------- |
| MAX_STORAGE_SIZE_MB | Gibt den Grenzwert für den Abstand von Daten, den Abfrage Store innerhalb Z-Datenbank des Kunden ausführen können | 100 | Für neue Datenbanken erzwungen |
| INTERVAL_LENGTH_MINUTES | Definiert die Größe des Zeitfensters während der zusammengestellten Runtime-Statistiken für die Abfrage-Pläne zusammengefasster und beibehalten werden. Jeder Plan aktiven Abfrage weist höchstens eine Zeile für einen Zeitraum definiert mit dieser Konfiguration | 60   | Für neue Datenbanken erzwungen |
| STALE_QUERY_THRESHOLD_DAYS | Zeitbasierte Aufräumen Richtlinie, die steuert der Aufbewahrungszeitraum dauerhaften Runtime-Statistiken und inaktive Abfragen | 30 | Für neue Datenbanken und Datenbanken mit vorherige Standarddomäne (367) erzwungen |
| SIZE_BASED_CLEANUP_MODE | Gibt an, ob die automatische Daten zum Aufräumen stattfindet, wenn die Größe der Abfrage Store Daten die Beschränkung Ansätze | Auto | Für alle Datenbanken erzwungen |
| QUERY_CAPTURE_MODE | Gibt an, ob alle Abfragen oder nur eine Teilmenge der Abfragen werden nachverfolgt. | Auto | Für alle Datenbanken erzwungen |
| FLUSH_INTERVAL_SECONDS | Gibt an, dass die Höchstdauer Runtime erfasst, Statistik im Arbeitsspeicher beibehalten werden, vor dem leeren auf einem Datenträger | 900 | Für neue Datenbanken erzwungen |
||||||

> [AZURE.IMPORTANT] Diese Standardwerte werden automatisch in der letzten Phase der Abfrage Store Aktivierung in allen SQL Azure-Datenbanken (Siehe wichtiger Hinweis vor) angewendet. Nach diesem dünne Linie nach oben wird Azure SQL-Datenbank wird nicht Konfigurationswerte festlegen, indem Sie die Kunden, ändern, es sei denn, sie primäre Arbeitsbelastung oder zuverlässigen Operationen des Abfrage-Speichers beeinträchtigen.

Wenn Sie mit der benutzerdefinierten Einstellungen in Kontakt bleiben möchten, verwenden Sie [ALTER DATABASE mit Optionen zur Abfrage Speicherung](https://msdn.microsoft.com/library/bb522682.aspx) Konfiguration in den vorherigen Status wiederherstellen. Checken Sie [Bewährte Methoden, mit dem Abfrage-Speicher](https://msdn.microsoft.com/library/mt604821.aspx) müssen, um zu erfahren, wie oben optimale Konfigurationsparameter ignoriert.

## <a name="next-steps"></a>Nächste Schritte

[SQL-Datenbank Leistung Einblick](sql-database-performance.md)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Für Weitere Informationen Auschecken den folgenden Artikeln:

- [Aufzeichnung einer Flight-Daten für die Datenbank](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database) 

- [Überwachen der Leistung mit dem Abfrage-Speicher](https://msdn.microsoft.com/library/dn817826.aspx)

- [Abfrage-Store Verwendungsszenarien](https://msdn.microsoft.com/library/mt614796.aspx)

- [Überwachen der Leistung mit dem Abfrage-Speicher](https://msdn.microsoft.com/library/dn817826.aspx) 

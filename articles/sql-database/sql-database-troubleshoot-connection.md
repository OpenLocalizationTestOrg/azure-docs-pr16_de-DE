<properties
    pageTitle="Datenbank auf dem Server ist zurzeit nicht verfügbar, Herstellen einer Verbindung SQL-Datenbank mit | Microsoft Azure"
    description="Behandeln von Problemen mit der Datenbank auf Server ist nicht derzeit verfügbar zurück, wenn eine Anwendung mit SQL-Datenbank verbunden ist."
    services="sql-database"
    documentationCenter=""
    authors="dalechen"
    manager="felixwu"
    editor=""
    keywords="Datenbank auf dem Server ist zurzeit nicht verfügbar, die mit Sql-Datenbank herstellen"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/21/2016"
    ms.author="daleche"/>

# <a name="error-database-on-server-is-not-currently-available-when-connecting-to-sql-database"></a>Fehler "Datenbank auf dem Server ist zurzeit nicht verfügbar" bei der Verbindung mit einer Sql-Datenbank
[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

Wenn eine Anwendung mit einer SQL Azure-Datenbank verbunden ist, erhalten Sie folgende Fehlermeldung angezeigt:

```
Error code 40613: "Database <x> on server <y> is not currently available. Please retry the connection later. If the problem persists, contact customer support, and provide them the session tracing ID of <z>"
```

> [AZURE.NOTE] Diese Fehlermeldung ist normalerweise vorübergehend (kurzlebige).

Dieser Fehler tritt auf, wenn die Azure-Datenbank verschoben (oder neu konfiguriert) und eine Anwendung verliert die Verbindung mit der SQL-Datenbank. SQL-Datenbank neu konfiguriert Ereignisse tritt aufgrund einer geplanten Ereignis (beispielsweise ein Softwareupgrade) oder einer ungeplanten Ereignis (beispielsweise ein Verfahren zum Absturz oder den Lastenausgleich). Die meisten neu konfiguriert Ereignisse sind im Allgemeinen kurzlebige und höchstens in weniger als 60 Sekunden abgeschlossen sein müssen. Jedoch können diese Ereignisse gelegentlich dauert länger fertig sind, wie z. B., wenn eine große Transaktion eine Wiederherstellung langer verursacht.

## <a name="steps-to-resolve-transient-connectivity-issues"></a>Schritte zum vorübergehenden Verbindungsprobleme zu beheben.
1.  Aktivieren Sie das [Microsoft Azure Service Dashboard](https://azure.microsoft.com/status) für alle bekannten Ausfall, die während der Zeit aufgetreten sind, in denen der Fehler durch die Anwendung gemeldet wurden.
2. Wiederholen Sie Logik zum Behandeln dieser Fehler einbinden diese als Anwendungsfehler für Benutzer von Applications, die auf einen Clouddienst verbinden, wie SQL Azure-Datenbank sollte Ereignisse periodisch neu konfiguriert erwarten und implementieren. Überprüfen Sie im Abschnitt [vorübergehende Fehler](sql-database-connectivity-issues.md) und die best Practices und Design, dass Richtlinien unter [Übersicht über die Entwicklung von SQL-Datenbank](sql-database-develop-overview.md) für Weitere Informationen und allgemeine Strategien wiederholen. Klicken Sie dann finden Sie unter Codebeispielen am [Datenverbindungsbibliotheken für SQL-Datenbank und SQL Server](sql-database-libraries.md) Einzelheiten.
3.  Wie eine Datenbank die Grenzwerte für die Ressource erreicht wird, können sie scheint eine vorübergehende Verbindungsproblem sein. Finden Sie unter [Problembehandlung bei Leistungsproblemen](sql-database-troubleshoot-performance.md).
4.  Wenn weiterhin Probleme mit der Konnektivität, oder die Dauer für die der Anwendung den Fehler auftritt 60 Sekunden überschreitet oder mehrere Vorkommen des Fehlers in einem bestimmten Tag angezeigt, Datei eine Supportanfrage Azure, indem Sie auf der Website [Azure unterstützen](https://azure.microsoft.com/support/options) **Abrufen unterstützen** auswählen.

## <a name="next-steps"></a>Nächste Schritte
- Wenn Sie eine andere Fehlermeldung erhalten, ausgewertet werden Sie die [Fehlermeldung](sql-database-develop-error-messages.md) für Hinweise auf die Ursache.
- Wenn das Problem beständigen wird, besuchen Sie die Anleitung im [allgemeinen Verbindungsprobleme behandeln von Problemen mit Azure SQL-Datenbank](sql-database-troubleshoot-common-connection-issues.md)aus.

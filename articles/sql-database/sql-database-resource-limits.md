<properties
    pageTitle="SQL Azure-Datenbank Ressource Beschränkungen"
    description="Diese Seite enthält einige allgemeine Ressource Grenzwerte für Azure SQL-Datenbank."
    services="sql-database"
    documentationCenter="na"
    authors="CarlRabeler"
    manager="jhubbard"
    editor="monicar" />


<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="10/13/2016"
    ms.author="carlrab" />


# <a name="azure-sql-database-resource-limits"></a>Beschränkungen der Ressource für Azure SQL-Datenbank

## <a name="overview"></a>(Übersicht)

Azure SQL-Datenbank verwaltet die Ressourcen einer Datenbank mit zwei verschiedenen Verfahren zur Verfügung: **Ressourcen Governance** und **Durchsetzung von Grenzwerte**. In diesem Thema wird erläutert, diese beiden wichtigsten Verwaltungsbereiche Ressource.

## <a name="resource-governance"></a>Ressource governance
Eines der Ziele beim Entwurf der Dienst leisten Basic, Standard und Premium ist für SQL Azure-Datenbank verhält, als wäre die Datenbank auf einem eigenen Computer, die von anderen Datenbanken vollständig isoliert ausgeführt wird. Ressource Governance emuliert dieses Verhalten. Wenn die Auslastung zusammengefasster Ressource maximale verfügbaren CPU-, Speicher, Log e/a- und die Datenbank zugeordneten Daten e/a-Ressourcen erreicht, wird Ressource Governance Abfragen, bei der Ausführung der Warteschlange und Zuordnen von Ressourcen zu den in der Warteschlange Abfragen, wie diese freizugeben.

Als auf einem dedizierten Computer führt alle verfügbare Ressourcen Nutzung zu einer längeren Ausführung des aktuell Ausführen von Abfragen, was Befehl Zeitlimit auf dem Client kann. Applikationen mit Logik anspruchsvollen Wiederholungsversuche und Anwendungen, die Abfragen für die Datenbank mit einem hoch Häufigkeit ausführen können Nachrichten Fehler auftreten, bei dem Versuch, eine neue Abfragen ausgeführt werden, wenn die gleichzeitige Anforderungen erreicht wurde hat.

### <a name="recommendations"></a>Empfehlungen:
Überwachen Sie die Nutzung der Ressource als auch die durchschnittliche Reaktionszeiten von Abfragen aus, wenn die maximale Auslastung einer Datenbank ist fast. Beim höhere Abfrage Wartezeiten finden gibt es in der Regel drei Optionen aus:

1.  Verringern Sie die Menge der eingehenden Anfragen auf die Datenbank auf das Timeout und den Stapel von der Anfragen zu verhindern.

2.  Zuweisen einer höheren Ebene der Leistung in der Datenbank.

3.  Optimieren von Abfragen, um die Ressource Auslastung jeder Abfrage zu verringern. Weitere Informationen finden Sie im Abschnitt Abfrage optimieren/Hinting im Artikel Leitfaden zur Leistung von Azure SQL-Datenbank.

## <a name="enforcement-of-limits"></a>Durchsetzung von Speichergrenzwerten
Als CPU, Arbeitsspeicher, e/a Log- und Daten e/a-Ressourcen werden durch neue Anfragen verweigern, wenn der Grenzwert erreicht werden erzwungen. Clients erhalten eine [Fehlermeldung angezeigt](sql-database-develop-error-messages.md) , je nach den Grenzwert, die erreicht wurde.

Die Anzahl von Verbindungen mit einer SQL-Datenbank und die Anzahl der gleichzeitige Anforderungen, die verarbeitet werden kann, sind beispielsweise beschränkt. SQL-Datenbank können die Anzahl der Verbindungen, um die Datenbank größer als die Anzahl der gleichzeitige Anforderungen zur Unterstützung von Verbindungspooling sein. Während die Menge des, die zur Verfügung stehenden Verbindungen einfach durch die Anwendung gesteuert werden kann, ist die Menge der parallele Anfragen oft schwieriger zu schätzen und so steuern Sie, wie oft aus. Besonders bei Höchstwert lädt Wenn die Anwendung, die entweder sendet, dass zu viele Anfragen oder die Datenbank, deren Ressource erreicht beschränkt und startet effektives Arbeitsthreads aufgrund länger laufenden Abfragen können Fehler auftreten.

## <a name="service-tiers-and-performance-levels"></a>Dienst Ebenen und Leistung Ebenen

Es gibt Dienst Ebenen und Leistung Berechtigungsstufen für beide eigenständigen Datenbank und elastisch Pools.

### <a name="standalone-databases"></a>Eigenständige Datenbanken

Für eine Datenbank eigenständigen sind die Grenzwerte einer Datenbank mit der Datenbank Service Ebene und Leistung Level definiert. Die folgende Tabelle beschreibt die Merkmale der grundlegende, Standard- und Premium Datenbanken auf unterschiedliche Leistung.

[AZURE.INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

### <a name="elastic-pools"></a>Flexible pools

[Flexible Pools](sql-database-elastic-pool.md) freigeben Ressourcen für Datenbanken im Pool. Die folgende Tabelle beschreibt die Merkmale der Basic, Standard und Premium flexible Datenbank Pools.

[AZURE.INCLUDE [SQL DB service tiers table for elastic databases](../../includes/sql-database-service-tiers-table-elastic-db-pools.md)]

Finden Sie unter einer erweiterten Definition der einzelnen Ressourcen, die in den vorherigen Tabellen aufgeführt die Beschreibungen in [Service-Funktionen, die Ebene und Grenzwerte](sql-database-performance-guidance.md#service-tier-capabilities-and-limits). Übersicht der Dienst leisten finden Sie unter [Azure SQL-Datenbank Dienst Ebenen und Leistung Ebenen](sql-database-service-tiers.md).

## <a name="other-sql-database-limits"></a>Weitere Beschränkungen SQL-Datenbank

| Bereich | Grenzwert | Beschreibung |
|---|---|---|
| Exportieren von Datenbanken mit automatisierten pro Abonnement | 10 | Automatisierte exportieren können Sie einen benutzerdefinierten Zeitplan zum Sichern Ihrer SQL-Datenbanken erstellen. Weitere Informationen finden Sie unter [SQL-Datenbanken: Unterstützung für SQL-Datenbank-Exporte automatisierte](http://weblogs.asp.net/scottgu/windows-azure-july-updates-sql-database-traffic-manager-autoscale-virtual-machines).|
| Datenbank pro server | Bis zu 5000 | Bis zu 5000 Datenbanken sind pro Server auf Servern V12 zulässig. |  
| DTUs pro server | 45000 | 45000 DTUs sind pro Server auf V12 Servern für provisioning Datenbanken, flexible Pools und Daten Lager verfügbar. |



## <a name="resources"></a>Ressourcen

[Azure-Abonnement und Beschränkungen Service, Kontingente und Einschränkungen](../azure-subscription-service-limits.md)

[SQL Azure-Datenbank-Dienst Ebenen und Leistung Ebenen](sql-database-service-tiers.md)

[Fehlermeldungen für Clientprogramme SQL-Datenbank](sql-database-develop-error-messages.md)

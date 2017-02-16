<properties
    pageTitle="Skalierbare Clouddatenbanken erstellen | Microsoft Azure"
    description="Erstellen Sie skalierbare .NET Datenbank-apps mit der flexible Datenbank-Client-Bibliothek"
    services="sql-database"
    documentationCenter=""
    manager="jhubbard"
    authors="ddove"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="ddove"/>

# <a name="building-scalable-cloud-databases"></a>Skalierbare Clouddatenbanken erstellen

Datenbanken Skalierung kann problemlos ausgeführt werden mithilfe von skalierbare Tools und Features für Azure SQL-Datenbank. Insbesondere können Sie die **flexible Datenbank-Client-Bibliothek** erstellen und Verwalten von skalierten Datenbanken verwenden. Mit diesem Feature können Sie problemlos sharded Applikationen mit Hunderte entwickeln – oder sogar Tausende – der SQL Azure-Datenbanken. [Flexible Aufträge](sql-database-elastic-jobs-powershell.md) können dann helfen Center für erleichterte Verwaltung von diesen Datenbanken verwendet werden.

Wechseln Sie zu [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/), um die Bibliothek zu installieren. 

## <a name="documentation"></a>Dokumentation
1. [Erste Schritte mit flexible Datenbanktools](sql-database-elastic-scale-get-started.md)
* [Flexible Datenbankfunktionen](sql-database-elastic-scale-introduction.md)
* [Shard Karte management](sql-database-elastic-scale-shard-map-management.md)
* [Migrieren von vorhandenen Datenbanken zu skalieren möchten](sql-database-elastic-convert-to-use-elastic-tools.md)
* [Daten abhängige routing](sql-database-elastic-scale-data-dependent-routing.md)
* [Mehrere Shard Abfragen](sql-database-elastic-scale-multishard-querying.md)
* [Hinzufügen eines Shard mit flexible Datenbanktools](sql-database-elastic-scale-add-a-shard.md)
* [Mehrere Mandanten Applikationen mit flexible Datenbanktools und Sicherheit auf Benutzerebene Zeile](sql-database-elastic-tools-multi-tenant-row-level-security.md)
* [Aktualisieren von apps für Client-Bibliothek](sql-database-elastic-scale-upgrade-client-library.md) 
* [Flexible Abfragen (Übersicht)](sql-database-elastic-query-overview.md)
* [Flexible Datenbank Tools-Glossar](sql-database-elastic-scale-glossary.md)
* [Flexible Datenbank-Client-Bibliothek mit Entität Framework](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md)
* [Flexible Datenbank-Client-Bibliothek mit Dapper](sql-database-elastic-scale-working-with-dapper.md)
* [Teilen und Zusammenführen-tool](sql-database-elastic-scale-overview-split-and-merge.md)
* [Leistungsindikatoren für Shard Landkarten-manager](sql-database-elastic-database-client-library.md) 
* [Häufig gestellte Fragen zur flexible Datenbanktools](sql-database-elastic-scale-faq.md)

## <a name="client-capabilities"></a>Clientfunktionen

Applikationen *Sharding* mit Skalierung stellt Herausforderung dar für sowohl der Entwickler als auch der Administrator. Die Clientbibliothek vereinfacht die Verwaltungsaufgaben durch Tools, mit denen beide Entwickler und Administratoren skalierten Datenbanken verwalten. In einer normalen Beispiel gibt es viele andere Datenbanken bekannt als "mehrere Shards hinweg," verwalten. In derselben Datenbank gemeinsame Kunden befinden, und es wird eine Datenbank pro Kunde (ein einzelnes-Mandanten Schema). Die Client-Bibliothek bietet folgende Funktionen:

1.  **Shard Karte Management**: eine spezielle Datenbank mit dem Namen "Shard Karte Manager" wird erstellt. Shard Karte Management ist die Möglichkeit für eine Anwendung zum Verwalten von Metadaten über deren mehrere Shards hinweg. Entwickler können dieses Feature verwenden, um Datenbanken als mehrere Shards hinweg registrieren, beschreiben Zuordnungen der einzelnen Sharding Tasten oder Key Bereiche auf diese Datenbanken und verwalten diese Metadaten als die Anzahl und Zusammenstellung der Datenbanken weiterentwickelt, sodass Kapazität Änderungen übernommen wurden. Sie müssen verbringen viel Zeit mit den Management-Code zu schreiben, bei der Implementierung von Sharding, ohne die flexible Datenbank-Client-Bibliothek. Weitere Informationen finden Sie unter [Shard Management zuordnen](sql-database-elastic-scale-shard-map-management.md).

* **Daten abhängige routing**: Stellen Sie sich vor einer Besprechungsanfrage in Kürze in die Anwendung. Basierend auf den Sharding Schlüsselwert der Anfrage, muss die Anwendung die richtige Datenbank basierend auf den Schlüsselwert zu bestimmen. Klicken Sie dann eine Verbindung mit der Datenbank für die Anforderung geöffnet. Daten abhängige routing bietet die Möglichkeit zum Öffnen von Verbindungen mit einer einzelnen einfach Anruf in der Karte Shard der Anwendung. Daten abhängige routing wurde einem anderen Bereich der Infrastrukturcode, der Funktionen in der Datenbank flexible Client-Bibliothek jetzt Gegenstand ist. Weitere Informationen finden Sie unter [Daten abhängige routing](sql-database-elastic-scale-data-dependent-routing.md).

* **Mehrere Shard Abfragen (MSQ)**: Abfragen mit mehreren Shard funktioniert, wenn eine Anforderung mehrere (oder alle) mehrere Shards hinweg umfasst. Eine Abfrage mit mehreren Shard führt derselben T-SQL-Code für alle mehrere Shards hinweg oder eine Reihe von mehrere Shards hinweg. Ein Gesamtergebnis mit UNION ALL Semantik festgelegt werden die Ergebnisse der teilnehmenden mehrere Shards hinweg zusammengeführt. Die Funktionalität wie über den Clientbibliothek verfügbar gemacht behandelt viele Aufgaben, einschließlich: Verwaltung von Verbindung, Thread, Fehlerbehandlung und Zwischenergebnisse verarbeiten. MSQ kann bis zu hundert mehrere Shards hinweg Abfragen. Weitere Informationen finden Sie unter [Abfragen mit mehreren Shard](sql-database-elastic-scale-multishard-querying.md).

Im Allgemeinen erwarten Kunden flexible Datenbanktools können vollständige T-SQL-Funktionen zu erhalten, wenn Shard-lokale Vorgänge im Gegensatz zum Cross-Shard Vorgänge übermitteln, die eigene Semantik aufweisen.

## <a name="next-steps"></a>Nächste Schritte

Versuchen Sie die [Beispiel-app](sql-database-elastic-scale-get-started.md) die veranschaulicht die Clientfunktionen aus. 

Wechseln Sie zu [Flexible Datenbank-Client-Bibliothek]( http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/), um die Bibliothek zu installieren.

Anweisungen mithilfe des Tools für Teilen und Zusammenführen finden Sie unter den [Teilen und Zusammenführen Tool – Übersicht](sql-database-elastic-scale-overview-split-and-merge.md).

[Flexible Datenbank-Client-Bibliothek ist jetzt stammenden öffnen!](https://azure.microsoft.com/blog/elastic-database-client-library-is-now-open-sourced/)

Verwenden Sie [Flexible Abfragen](sql-database-elastic-query-overview.md).

Die Bibliothek ist als open Source-Software auf [GitHub](https://github.com/Azure/elastic-db-tools)verfügbar. 


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Anchors-->
<!--Image references-->
[1]:./media/sql-database-elastic-database-client-library/glossary.png


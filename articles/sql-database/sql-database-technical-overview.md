<properties
    pageTitle="Was ist die SQL-Datenbank? Einführung in die SQL-Datenbank | Microsoft Azure"
    description="Einführung in SQL-Datenbank: technische Details und Funktionen von Microsoft relationalen Datenbank-Management-System (RDBMS) in der Cloud."
    keywords="Einführung in Sql, Einführung in Sql, was Sql-Datenbank ist"
    services="sql-database"
    documentationCenter=""
    authors="shontnew"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="08/16/2016"
   ms.author="shkurhek"/>

# <a name="what-is-sql-database-introduction-to-sql-database"></a>Was ist die SQL-Datenbank? Einführung in SQL-Datenbank

SQL-Datenbank ist einer relationalen Datenbank-Dienst in der Cloud, basierend auf der führende Microsoft SQL Server-Engine mit wichtigen Funktionen. SQL-Datenbank bietet vorhersehbar Performance, Skalierbarkeit ohne Ausfallzeiten, Geschäftskontinuität und Datenschutz – alle mit nahezu null-Verwaltung. Sie können sich auf schnelle app-Entwicklung und Verkürzung der Ihrer Market, anstatt Verwalten von virtuellen Computern und Infrastruktur konzentrieren. Da es auf dem [SQL Server](https://msdn.microsoft.com/library/bb545450.aspx) -Engine basiert, unterstützt SQL-Datenbank vorhandenen SQL Server-Tools, Bibliotheken und APIs, die Sie verschieben, und erweitern in der Cloud vereinfacht.

In diesem Artikel wird eine Einführung in die grundlegenden Konzepte SQL-Datenbank und Features, die im Zusammenhang mit der Leistung und Skalierbarkeit Verwaltbarkeit, mit Links zu den Details auszuwerten. Wenn Sie bereit sind, im springen, können Sie [Erstellen Ihrer ersten SQL-Datenbank](sql-database-get-started.md) oder [Erstellen Sie eine Datenbank flexible Ressourcenpool](sql-database-elastic-pool-create-portal.md) in Minuten. Wenn Sie eingehender möchten, schauen Sie sich dieses Video an, 30 Minuten.

> [AZURE.VIDEO azurecon-2015-get-started-with-azure-sql-database]

## <a name="adjust-performance-and-scale-without-downtime"></a>Anpassen der Leistung und Skalierung ohne Ausfallzeiten

SQL-Datenbanken steht in Basic, Standard und Premium *Service Ebenen*. Pro Dienstebene bietet [verschiedene Detailebenen Leistung und Funktionen](sql-database-service-tiers.md) zur Unterstützung von Lightweight zu schwer Datenbank Auslastung. Sie können Ihre erste Anwendung auf eine kleine Datenbank für ein paar Treuebonus pro Monat, dann [Ändern der Dienstebene](sql-database-scale-up.md) manuell oder programmgesteuert zu einem beliebigen Zeitpunkt erstellen wie Ihre app Viren weltweit, ohne Ausfallzeiten Ihrer app oder Ihren Kunden wechselt.

Für viele Unternehmen und apps reicht die Datenbanken erstellen, und wählen einzelne Datenbank Leistung bei Bedarf nach oben oder unten, insbesondere dann, wenn Verwendungsmuster relativ vorhersehbar sind. Wenn Sie nicht vorhersehbar Verwendungsmustern haben, kann aber es ist schwer zu Kosten und Modell Business verwalten.

[Flexible Pools](sql-database-elastic-pool.md) in SQL-Datenbank dieses Problem zu lösen. Das Konzept ist einfach. Sie reservieren Leistung zu einem Ressourcenpool und die kollektiven Leistung der Ressourcenpool statt einzelnen Datenbank-Performance bezahlen. Sie benötigen keine Datenbank-Performance nach oben oder unten einwählen. Die Datenbanken im Pool, *flexible Datenbanken*aufgerufen skalieren automatisch nach oben und nach unten bis zum erfüllen bei Bedarf. Flexible Datenbanken nutzen, aber nicht die Grenzwerte der Ressourcenpool überschreiten, damit Ihre Kosten vorhersehbar bleibt, auch wenn die Datenbankverwendung nicht. Funktionsumfang, können Sie [Hinzufügen und Entfernen von Datenbanken mit dem Pool](sql-database-elastic-pool-manage-portal.md), dieselbe Skalierung Ihre app aus wenigen Datenbanken Tausendertrennzeichen, alle innerhalb eines Budgets, das Sie steuern. Weitere Informationen zum Entwurfsmuster für SaaS Applikationen flexible Pools verwenden, finden Sie unter [Entwurfmustern für mehrere Mandanten SaaS Applikationen mit Azure SQL-Datenbank](sql-database-design-patterns-multi-tenancy-saas-applications.md).

In beiden Fällen Sie wechseln – einzelne oder flexible – Sie sind nicht gesperrt. Sie können einzelne Datenbanken mit flexible Datenbank Pools angleichen, und ändern die Dienst Ebenen der einzelnen Datenbanken und Pools innovative Designs erstellen. Darüber hinaus können Sie mit dem Power und der Reichweite von Azure-Sortiment Azure Services mit SQL-Datenbank Ihren Anforderungen eindeutige moderne app entwerfen, Kosten und Ressourcen Effizienz und Aufheben der Sperre neuer geschäftlicher Chancen.

Aber wie können Sie die relative Leistung von Datenbanken und Datenbank Pools vergleichen? Woher wissen Sie die klicken Sie mit der rechten Maustaste auf Beenden, wenn Sie nach oben oder unten einwählen? Die Antwort ist die Datenbank Transaktion Einheit (DTU) für die einzelnen Datenbanken und die flexible DTU (eDTU) für flexible Datenbanken und Datenbank Pools. Finden Sie unter [SQL-Datenbank-Optionen und Leistung: zu verstehen, was in jeder Kategorie Dienst verfügbar ist](sql-database-service-tiers.md) Details.

## <a name="keep-your-app-and-business-running"></a>Organisieren Sie Ihre app und Business ausgeführt

Der Azure Branche führenden 99,99 % Verfügbarkeit Vereinbarung zum Servicelevel [(Vereinbarung zum SERVICELEVEL)](http://azure.microsoft.com/support/legal/sla/), betrieben unter einem globalen Netzwerk von Microsoft verwaltete Rechenzentren sorgt Ihre 24/7-app. Mit jeder SQL-Datenbank nutzen Sie integrierte Datenschutz, Fehlertoleranz und Schutz der Daten, die Sie andernfalls hätten entwerfen, kaufen, erstellen und verwalten. Je nach der Auslastung Ihres Unternehmens, können Sie auch Ja, Schutz, um sicherzustellen, dass Ihre app und Ihr Unternehmen schnell bei einem Datenverlust, einen Fehler oder etwas anderes wiederherstellen können werden zusätzliche verlangen. Mit SQL-Datenbank bietet pro Dienstebene ein Menü mit anderes Features, die Sie verwenden können, Einstieg und Ausführung und greifen auf diese Weise. Point-in-Time wiederherstellen können Sie eine Datenbank in einer früheren Zustand, bis zurück zum 35 Tagen zurückzukehren. Wenn Ihre Datenbanken Hostinganbieter Datencenter ein Ausfall auftritt, können Sie darüber hinaus Failover auf Datenbankreplikaten in einem anderen Bereich. Oder Sie können Replikate für Upgrades oder Verlagerung auf die verschiedenen Bereiche verwenden.

![SQL-Geo-Datenbankreplikation](./media/sql-database-technical-overview/azure_sqldb_map.png)


[Geschäftskontinuität](sql-database-business-continuity.md) finden Sie Details zu den verschiedenen Business Continuity-Funktionen für unterschiedliche Service Ebenen verfügbar.

## <a name="secure-your-data"></a>Sichern Sie Ihre Daten
SQL Server weist traditionell einfarbige Daten Sicherheit, dass die SQL-Datenbank mit Features, die Zugriff einschränken, Datenschutz und helfen Ihnen Überwachen der Aktivitäten, Rechtsstaatsprinzip. Eine Auflistung von Sicherheitsoptionen, die Sie in der SQL-Datenbank haben finden Sie unter [Sichern Ihrer SQL-Datenbank](sql-database-security.md) . Finden Sie im [Sicherheitscenter für SQL Server-Datenbank-Engine und SQL-Datenbank](https://msdn.microsoft.com/library/bb510589) für eine umfassendere Ansicht Sicherheitsfeatures aus. Und finden Sie im [Sicherheitscenter Azure](https://azure.microsoft.com/support/trust-center/security/) Informationen zur Azure Plattform Sicherheit.

## <a name="next-steps"></a>Nächste Schritte
Nun haben Sie eine Einführung in die SQL-Datenbank lesen und auf die Frage "Was ist ein SQL-Datenbank?", nun möchten:

- Finden Sie auf der [Seite Preise](https://azure.microsoft.com/pricing/details/sql-database/) für einzelne Datenbank und flexible Datenbank Kosten vergleichen und einsparungen.
- Informationen Sie zu [flexible Pools](sql-database-elastic-pool.md).
- Erste Schritte durch [Erstellen Ihrer erste Datenbank](sql-database-get-started.md).
- [Verbinden und Abfragen mit SSMS](sql-database-connect-query-ssms.md)
- Erstellen Sie Ihre erste Anwendung in c#, Java, Node.js, PHP, Python oder Ruby: [Datenverbindungsbibliotheken für SQL-Datenbank und SQL Server](sql-database-libraries.md)
- Finden eines Indexes der Titel und Beschreibung [aller](sql-database-index-all-articles.md)Themen für Azure Sql-Datenbank-Dienst an.

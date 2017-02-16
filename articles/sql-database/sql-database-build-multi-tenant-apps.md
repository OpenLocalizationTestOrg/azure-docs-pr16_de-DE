<properties
   pageTitle="SQL Azure-Datenbank erstellt, mit mehreren Mandanten-Apps mit Isolation und Effizienz"
   description="Erfahren Sie, wie SQL-Datenbank mit mehreren Mandanten apps erstellt"
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
   ms.date="10/13/2016"
   ms.author="carlrab"/>

# <a name="builds-multi-tenant-apps-with-azure-sql-database-with-isolation-and-efficiency"></a>Erstellt mit mehreren Mandanten-Apps mit Azure-SQL­Datenbank mit Isolation und Effizienz

## <a name="leverage-elastic-pools-and-build-more-efficient-multi-tenant-apps"></a>Flexible Pools nutzen und effizienter mit mehreren Mandanten apps erstellen

Wenn Sie ein Entwickler SaaS schreiben eine app mit mehreren Mandanten und Umgang mit vielen Kunden haben, nehmen Sie Sie häufig Kompromisse Kunden Leistung, Sicherheit und Verwaltung. Mit Azure SQL-Datenbank flexible Datenbank Pools müssen Sie nicht mehr stellen die Kompromisse. Diese Pools helfen Ihnen verwalten und Überwachen von apps mit mehreren Mandanten und Isolation Vorteile eine Kunden pro Datenbank zu erhalten. Finden Sie unter [Entwurfmustern für mehrere Mandanten SaaS Applikationen mit SQL Azure-Datenbank](sql-database-design-patterns-multi-tenancy-saas-applications.md).

![Generator-Multi-Mandanten-apps](./media/sql-database-build-multi-tenant-apps/sql-database-build-multi-tenant-apps.png)

## <a name="auto-scaling-you-control"></a>Automatische Skalierung steuern

Automatisches Skalieren Pools Leistung und Speicherkapazität für flexible Datenbanken im laufenden Betrieb. Sie können steuern, die Leistung zu einem Ressourcenpool zugewiesen, hinzufügen oder entfernen flexible Datenbanken bei Bedarf, und Leistung von flexible Datenbanken ohne Auswirkung auf die Gesamtkosten für den Ressourcenpool definieren. Dies bedeutet, dass Ihnen keine Gedanken über die Verwendung der einzelnen Datenbanken verwalten zu machen.

[Lesen Sie die Dokumentation](sql-database-elastic-pool.md)

## <a name="intelligent-management-of-your-environment"></a>Intelligente Management Ihrer Umgebung

Empfehlungen im Zusammenhang mit integrierten Ziehpunkts identifizieren vorausschauende Datenbanken, die von Pools profitieren würden. Diese Empfehlungen zulassen "Was-wäre-wenn" Analyse für schnellen Optimierung für Ihre Ziele Leistung. Rich-Leistung überwachen und zur Problembehandlung Dashboards zurückliegenden Ressourcenpool Auslastung Visualisierung helfen.

[Lesen Sie die Dokumentation](sql-database-elastic-pool-guidance.md)

## <a name="performance-and-price-to-meet-your-needs"></a>Leistung und Preis zu Ihren Anforderungen

Grundlegende, Standard, und Premium Pools bereitstellen eine Vielfalt von Leistung, Speicher und Preise Optionen. Pools können bis zu 400 flexible Datenbanken enthalten. Flexible Datenbanken können automatische Skalierung bis zu 1000 flexible Datenbank Transaktion Einheiten (eDTU).

[Lesen Sie die Dokumentation](https://azure.microsoft.com/pricing/details/sql-database/?b=16.50)

## <a name="elastic-tools"></a>Flexible tools

Zusätzlich zu flexible Pools gibt es SQL-Datenbank-Features, mit denen Betrieb Aktivitäten über mehrere Datenbanken verwalten:

**Führen Sie Cross-Datenbank Abfragen und Berichte.**  
[Flexible Datenbankabfrage](sql-database-elastic-query-overview.md) können Sie Abfragen oder Berichte über Datenbanken in Ihrem flexible Ressourcenpool ausgeführt und remote gleichzeitig in vielen Datenbanken Ihrer Ressourcenpool gespeicherte Daten zugreifen.

**Führen Sie die Cross Datenbanktransaktionen.**  
[Flexible Datenbanktransaktionen](sql-database-elastic-transactions-overview.md) ermöglichen Ihnen, Transaktionen auszuführen, die mehrere Datenbanken in SQL-Datenbanken erstrecken und Operationen (d. h., wenn finanzielle Transaktionen über Datenbanken Verarbeitung oder beim Aktualisieren von Beständen in einer Datenbank und Bestellungen).

**Führen Sie die gleichen Operationen auf mehrere Datenbanken.**  
[Flexible Datenbank Einzelvorgänge](sql-database-elastic-jobs-overview.md) ausführen administrative Vorgänge wie Neuerstellen von Indizes oder Schemas über jede Datenbank in Ihre flexible Ressourcenpool aktualisieren.

Wechseln Sie zur Homepage finden Sie unter Was sonst SQL-Datenbank zu bieten hat.
[Schauen sie sich diese](https://azure.microsoft.com/services/sql-database/) 

## <a name="next-steps"></a>Nächste Schritte

Erhalten einer [kostenlosen Azure-Abonnement](https://azure.microsoft.com/get-started/) und [Erstellen Ihrer ersten Azure SQL-Datenbank](sql-database-get-started.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Untersuchen Sie die [Funktionen von SQL-Datenbank](https://azure.microsoft.com/services/sql-database/).
 
Lesen Sie die [Technische Übersicht der SQL-Datenbank](sql-database-technical-overview.md).  

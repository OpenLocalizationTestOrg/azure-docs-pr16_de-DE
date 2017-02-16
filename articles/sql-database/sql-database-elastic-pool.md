<properties
    pageTitle="Was ist eine Azure flexible Ressourcenpool? | Microsoft Azure"
    description="Verwalten von Hunderte oder Tausende von Datenbanken mit einem Ressourcenpool. Ein Preis für eine Reihe von Leistungseinheiten kann über den Pool verteilt werden. Verschieben-Datenbanken oder verkleinern am werden."
    keywords="Flexible Datenbank, Sql-Datenbanken"
    services="sql-database"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="07/12/2016"
    ms.author="CarlRabeler"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="what-is-an-azure-elastic-pool"></a>Was ist eine Azure flexible Ressourcenpool?

SQL-DB flexible Pools bieten eine einfache kostengünstige Lösung zum Verwalten der Leistungsziele für mehrere Datenbanken, die Streuung mit wechselnder aufweisen und nicht vorhersehbar Verwendungsmustern.

> [AZURE.NOTE] Flexible Pools sind in der Regel verfügbar (GA) in allen Azure Regionen außer Westen Indien, wo es zurzeit in der Vorschau ist.  GA flexible Pools in diesem Bereich treten so früh wie möglich.

## <a name="how-it-works"></a>So funktioniert es

Ein gemeinsames SaaS Anwendung Muster wird das Datenbankmodell Single-Mandanten: einzelnen Kunden erhält eine eigene Datenbank. Einzelnen Kunden (Datenbank) hat möglicherweise nicht vorhersehbar Ressource Anforderungen für Arbeitsspeicher, CPU und EA. Mit dieser höchsten und niedrigsten Punkt der Demand zuordnen gehen Sie wie folgt wie Sie Ressourcen effizient und kostengünstiger? In der Vergangenheit haben Sie zwei Optionen: (1) Blinde Bereitstellungsressourcen basierend auf Höchstwert Verwendung und über die Bezahlung oder (2) Unzureichende Bereitstellen von Kosten, auf Kosten der Leistung und Kunden zufrieden während Spitzen zu speichern. Flexible Pools lösen dieses Problem, indem Sie sicherstellen, dass die Datenbanken erhalten die Leistung Ressourcen benötigten und wann sie benötigen. Sie bieten die Möglichkeit Zuteilung einfachen Ressource innerhalb eines Budgets vorhersehbar. Weitere Informationen zum entwurfmustern für SaaS Applikationen flexible Pools verwenden, finden Sie unter [Entwurfmustern für mehrere Mandanten SaaS Applikationen mit Azure SQL-Datenbank](sql-database-design-patterns-multi-tenancy-saas-applications.md).

> [AZURE.VIDEO elastic-databases-helps-saas-developers-tame-explosive-growth]

In SQL-Datenbank wird das relative Maß für eine Datenbank Möglichkeit Ressource erfordert verarbeitet in einer anderen Datenbank Transaktion Einheit (DTUs) für einzelne Datenbanken und flexible DTUs (eDTUs) für flexible Datenbanken in einem flexible Pool ausgedrückt. Finden Sie weitere Informationen zu DTUs und eDTUs die [Einführung in SQL-Datenbank](sql-database-technical-overview.md#understand-dtus) .

Ein Ressourcenpool ist eine festgelegte Anzahl von eDTUs, die für einen festgelegten Preis angegeben. Im Pool werden einzelne Datenbanken automatische Skalierung in Festlegen von Parametern die Flexibilität erteilt. Bei hoher Auslastung kann eine Datenbank zum erfüllen bei Bedarf weitere eDTUs nutzen. Datenbanken unter light Belastung nutzen kleiner und Datenbanken keine Auslastung nutzen keine eDTUs. Bereitstellen von Ressourcen für den gesamten Pool und nicht für einzelne Datenbanken vereinfacht Ihre Verwaltungsaufgaben. Außerdem verlangt Sie ein vorhersehbar Budget für den Ressourcenpool haben.

Zusätzliche eDTUs können zu einem vorhandenen Pool mit Störungsfreiheit Datenbank oder keine Auswirkung auf die Datenbanken im flexible Pool hinzugefügt werden. Auf ähnliche Weise, wenn zusätzliche eDTUs nicht mehr benötigt werden können sie von einem vorhandenen Pool an einer beliebigen Stelle Zeitpunkt entfernt werden.

Und Sie können addieren oder Subtrahieren von Datenbanken mit dem Pool. Wenn Sie eine Datenbank vorhersehbar unter-genutzt wird es Ressourcen, zu verschieben.

## <a name="which-databases-go-in-a-pool"></a>Welche Datenbanken in einem Ressourcenpool wechseln?

![SQL-Datenbanken eDTUs in einem Pool flexible Datenbank freigeben.][1]

Gute Kandidaten für flexible Pools in der Regel werden Datenbanken haben Zeiträumen Aktivitäten und anderen Perioden inaktiv. Im obigen Beispiel wird die Aktivität einer einzelnen Datenbank, 4 Datenbanken und schließlich ein flexible Ressourcenpool mit 20 Datenbanken. Datenbanken mit unterschiedlichen Aktivität über einen Zeitraum sind gute Kandidaten für flexible Pools auf, da es nicht zur gleichen Zeit aller aktiven stehen und können eDTUs freigeben. Nicht alle Datenbanken passt dieses Muster. Datenbanken, die eine weitere Konstante Ressource Demand haben eignen sich besser für die Basic, Standard und Premium Service Ebenen wo Ressourcen einzeln zugewiesen werden.

[Preis und Leistung Aspekte für eine flexible Ressourcenpool](sql-database-elastic-pool-guidance.md).

## <a name="edtu-and-storage-limits-for-elastic-pools-and-elastic-databases"></a>Grenzwerte für flexible Pools und flexible Datenbanken eDTU und Speicher.

[AZURE.INCLUDE [SQL DB service tiers table for elastic databases](../../includes/sql-database-service-tiers-table-elastic-db-pools.md)]

Wenn alle DTUs eine flexible Ressourcenpool verwendet werden, erhält jede Datenbank im Pool gleich viel Ressourcen zum Verarbeiten von Abfragen.  Der SQL-Datenbank-Dienst bietet Ressourcen mitbenutzende Fairness zwischen Datenbanken indem sichergestellt gleich Segmente Zeit berechnen. Flexible Ressourcenpool Ressource Freigabe Fairness wird über eine beliebige Anzahl von Ressourcen, die andernfalls wird sichergestellt, dass jede Datenbank, wenn die min DTU pro Datenbank auf einem Wert ungleich NULL festgelegt ist.

## <a name="elastic-pool-and-elastic-database-properties"></a>Flexible Ressourcenpool und flexible Datenbankeigenschaften

### <a name="limits-for-elastic-pools"></a>Grenzwerte für flexible pools

| Eigenschaft | Beschreibung |
| :-- | :-- |
| Dienstebene | Basic, Standard oder Premium. Die Dienstebene bestimmt den Bereich im Leistung und Speicher schränkt die sowie Business Kontinuitätsoptionen konfiguriert werden können. Jede Datenbank in einem Ressourcenpool hat der gleichen Dienst Schicht als dem Pool an. "Dienstebene" wird auch als "Edition." bezeichnet |
| eDTUs pro pool | Die maximale Anzahl von eDTUs, die von Datenbanken im Pool gemeinsam verwendet werden können. Die gesamte eDTUs von Datenbanken im Pool verwendet darf diese Beschränkung an derselben Stelle Zeitpunkt nicht überschreiten. |
| Max-Speicher pro Pool (GB) | Die maximale Größe von Speicher in GB, die von Datenbanken im Pool gemeinsam verwendet werden können. Gesamtspeicher von Datenbanken im Pool verwendete darf nicht diese Beschränkung überschreiten. Dieser Grenzwert wird durch die eDTUs pro Pool bestimmt. Wenn dieser Grenzwert überschritten wird, werden alle Datenbanken schreibgeschützt. |
| Maximale Anzahl von Datenbanken pro Ressourcenpool | Die maximale Anzahl von Datenbanken pro Pool zulässig. |
| Max gleichzeitige Kollegen pro pool | Die maximale Anzahl von gleichzeitigen Arbeitskräften (Anfragen) für alle Datenbanken im Pool verfügbar. |
| Max gleichzeitige Benutzernamen pro pool | Die maximale Anzahl der gleichzeitigen Benutzernamen für alle Datenbanken im Pool. |
| Max gleichzeitige Sitzungen pro pool | Die maximale Anzahl von Sitzungen, die für alle Datenbanken im Pool verfügbar. |


### <a name="limits-for-elastic-databases"></a>Grenzwerte für flexible Datenbanken

| Eigenschaft | Beschreibung |
| :-- | :-- |
| Max eDTUs pro Datenbank | Die maximale Anzahl von eDTUs, dass alle im Pool Datenbankfunktionen möglicherweise verwenden, wenn verfügbar Nutzung von anderen Datenbanken im Pool basierend auf.  Max eDTU pro Datenbank ist keine Garantie für Ressourcen für eine Datenbank.  Diese Einstellung ist eine globale Einstellung, die für alle Datenbanken im Pool gilt. Legen Sie max eDTUs pro Datenbank hoch genug eingestellt und Spitzen in Database Auslastung zu behandeln. In gewissem Umfang overcommitting wird erwartet, da Pool im Allgemeinen wird davon ausgegangen wichtiges und Kalt Verwendungsmuster für Datenbanken, werden alle Datenbanken nicht gleichzeitig einem Höchstwert von. Beispiel: die Nutzung Höchstwert pro Datenbank ist 20 eDTUs und nur 20 % 100 Datenbanken in dem Pool sind Höchstwert zur gleichen Zeit.  Wenn die eDTU pro Datenbank max 20 eDTUs festgelegt ist, ist es sinnvoll, den Ressourcenpool Faktor von 5 overcommit, und legen Sie die eDTUs pro Pool bis 400. |
| Min eDTUs pro Datenbank | Die minimale Anzahl von eDTUs, die eine Datenbank im Pool garantiert ist.  Diese Einstellung ist eine globale Einstellung, die für alle Datenbanken im Pool gilt. Die min eDTU pro Datenbank möglicherweise auf 0 festgelegt, und ist auch der Standardwert. Diese Eigenschaft ist auf eine beliebige Stelle zwischen 0 und die durchschnittliche eDTU Auslastung pro Datenbank festgelegt. Das Produkt aus der Anzahl der Datenbanken in dem Pool und die min eDTUs pro Datenbank kann die eDTUs pro Pool nicht überschreiten.  Beispielsweise muss weist ein Pool 20 Datenbanken und die min eDTU pro Datenbank auf 10 eDTUs festgelegt, die eDTUs pro Pool als 200 eDTUs mindestens so groß sein. |
| Max-Speicher pro Datenbank (GB) | Der maximale Speicherplatz für eine Datenbank in einem Ressourcenpool. Flexible Datenbanken Speicherpools, freigeben, damit Datenbankspeicher auf den kleineren der verbleibenden Speicherpools und max beschränkt ist Speicherplatz pro Datenbank.|


## <a name="elastic-database-jobs"></a>Flexible Datenbankaufträge

Mit einem Ressourcenpool werden Verwaltungsaufgaben durch Ausführen von Skripts in **[flexible Aufträge](sql-database-elastic-jobs-overview.md)**vereinfacht. Eine Position flexible Datenbank beseitigt die meisten zeitlichen Aufwand mit einer großen Anzahl von Datenbanken verknüpft ist. Um zu beginnen, finden Sie unter [Erste Schritte mit flexible Datenbank Aufträge](sql-database-elastic-jobs-getting-started.md).

Weitere Informationen zu anderen Tools finden Sie unter die [flexible Datenbanktools learning Karte](https://azure.microsoft.com/documentation/learning-paths/sql-database-elastic-scale/).

## <a name="business-continuity-features-for-databases-in-a-pool"></a>Business Continuity-Funktionen für Datenbanken in einem Ressourcenpool

Flexible Datenbanken unterstützen im Allgemeinen gleiche [Business Continuity-Funktionen](sql-database-business-continuity.md) , die für einzelne Datenbanken im V12 Server verfügbar sind.


### <a name="point-in-time-restore"></a>Zeitpunkt wiederherstellen

Punkt in Zeit wiederherstellen verwendet automatische Datenbanksicherungskopien in einer Datenbank in einem Ressourcenpool bis zu einem bestimmten Zeitpunkt wiederherstellen. Finden Sie unter [Point-In-Time wiederherstellen](sql-database-recovery-using-backups.md#point-in-time-restore)

### <a name="geo-restore"></a>Geo-wiederherstellen

Geo-Wiederherstellen bietet die Wiederherstellung Standardoption, wenn eine Datenbank aufgrund ein Vorfall in der Region nicht verfügbar ist, in dem die Datenbank gehostet wird. [Wiederherstellen einer Azure SQL-Datenbank oder Failover zu einem sekundären](sql-database-disaster-recovery.md) finden Sie unter 

### <a name="active-geo-replication"></a>Aktive Geo-Replikation

Konfigurieren Sie für Applikationen, die kürzere Wiederherstellung Anforderungen als Geo-wiederherstellen anbieten kann aufweisen, aktiven Geo-Replikation mithilfe der [Azure-Portal](sql-database-geo-replication-portal.md), [PowerShell](sql-database-geo-replication-powershell.md)oder [Transact-SQL](sql-database-geo-replication-transact-sql.md)ein.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Microsoft Virtual Academy videokurses auf flexible Datenbankfunktionen](https://mva.microsoft.com/en-US/training-courses/elastic-database-capabilities-with-azure-sql-db-16554) 

<!--Image references-->
[1]: ./media/sql-database-elastic-pool/databases.png

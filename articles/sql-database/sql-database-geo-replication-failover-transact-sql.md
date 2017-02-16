<properties 
    pageTitle="Einleiten einen geplanten oder ungeplanten Failover für SQL-Datenbank mit Transact-SQL Azure | Microsoft Azure" 
    description="Einleiten eines geplanten oder ungeplanten Failovers für mit Transact-SQL Azure SQL-Datenbank" 
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
    ms.date="08/29/2016"
    ms.author="carlrab"/>

# <a name="initiate-a-planned-or-unplanned-failover-for-azure-sql-database-with-transact-sql"></a>Einleiten eines geplanten oder ungeplanten Failovers für mit Transact-SQL Azure SQL-Datenbank


> [AZURE.SELECTOR]
- [Azure-portal](sql-database-geo-replication-failover-portal.md)
- [PowerShell](sql-database-geo-replication-failover-powershell.md)
- [T-SQL](sql-database-geo-replication-failover-transact-sql.md)


In diesem Artikel wird gezeigt, wie einleiten Failover zu einer sekundären SQL-Datenbank mithilfe von Transact-SQL. Zum Konfigurieren der Geo-Replikation finden Sie unter [Konfigurieren von Geo-Replikation für SQL Azure-Datenbank](sql-database-geo-replication-transact-sql.md).



Um Failoverantwort auszulösen, benötigen Sie Folgendes:

- Ein Benutzername, der DBManager auf dem primären, haben Db_ownership der, dass Sie Geo Replikation, werden lokale Datenbank und den Partner-Servern, dem Sie Geo-Replikation konfigurieren, DBManager sein.
- SQL Server Management Studio (SSMS)


> [AZURE.IMPORTANT] Es wird empfohlen, dass Sie immer die neueste Version von Management Studio verwenden, um mit Microsoft Azure und SQL-Datenbank-Updates synchronisiert werden. [Aktualisieren von SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).




## <a name="initiate-a-planned-failover-promoting-a-secondary-database-to-become-the-new-primary"></a>Einleiten eines geplanten Failovers Heraufstufen einer sekundären Datenbank zur neuen primären

**ALTER DATABASE** -Anweisung können Sie eine sekundäre Datenbank aus, um die neue primäre Datenbank in einer geplanten Art und Weise, werden Höherstufen der vorhandenen Primär, um die Konvertierung in einem sekundären herabstufen. Klicken Sie auf das master-Datenbank auf die logischen Azure SQL-Datenbankserver in dem sich die Geo repliziert sekundäre Datenbank, die höher gestuft wird befindet diese Anweisung ausgeführt. Dieses Feature ist für Geplantes Failover wie vorgesehen, während die DR einen Drilldown, und setzt voraus, dass die primäre Datenbank zur Verfügung.

Der Befehl führt eine der folgenden Workflow aus:

1. Wechselt vorübergehend Replikation zu synchroner Modus, die bewirken, dass alle ausstehende Transaktionen in den sekundären geleert und alle neue Transaktionen blockieren.

2. Wechselt die Rollen der beiden Datenbanken in der Geo-Replikation Zusammenarbeit.  

Diese Sequenz wird sichergestellt, dass die beiden Datenbanken synchronisiert werden, bevor die Rollen wechseln und daher keine Daten verloren. Es gibt eine kurze Zeit, während, die eine beide Datenbanken (der Reihenfolge der zu 25 0 Sekunden) nicht verfügbar sind, während die Rollen aktiviert sind. Wenn die primäre Datenbank mehrere sekundäre Datenbanken aufweist, wird der anderen sekundäre Verbindung mit dem neuen primären automatisch neu der Befehl konfigurieren.  Der gesamte Vorgang sollte weniger als eine Minute unter normalen Umständen dauern. Weitere Informationen finden Sie unter [ALTER DATABASE (Transact-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) und [Dienst Ebenen](sql-database-service-tiers.md).


Gehen Sie folgendermaßen vor, um eine geplante Failoverantwort auszulösen.

1. Schließen Sie in Management Studio an die logische Azure SQL-Datenbankserver in dem sich eine Geo repliziert sekundäre Datenbank befindet.

2. Öffnen Sie den Ordner Datenbanken, erweitern Sie den Ordner **System-Datenbanken** , mit der rechten Maustaste auf **master**, und klicken Sie dann auf **Neue Abfrage**.

3. Verwenden Sie folgende **ALTER DATABASE** -Anweisung, um die sekundäre Datenbank, die der primären Rolle zu wechseln.

        ALTER DATABASE <MyDB> FAILOVER;

4. Klicken Sie auf **Ausführen** , um die Abfrage auszuführen.

>[AZURE.NOTE] In Ausnahmefällen ist es möglich, dass der Vorgang kann nicht abgeschlossen werden und hängen auftreten können. In diesem Fall kann der Benutzer sofort Failover Befehl ausführen und Datenverlust annehmen.


## <a name="initiate-an-unplanned-failover-from-the-primary-database-to-the-secondary-database"></a>Einleiten eines ungeplanten Failovers aus der primären Datenbank auf die sekundäre Datenbank

**ALTER DATABASE** -Anweisung können Sie eine sekundäre Datenbank aus, um die Konvertierung in die neue primäre Datenbank in einer ungeplanten Weise, heraufstufen, erzwingen der Herabstufen der vorhandenen primären einer sekundären nacheinander vorgesehen ist, wenn die primäre Datenbank nicht mehr verfügbar ist. Klicken Sie auf das master-Datenbank auf die logischen Azure SQL-Datenbankserver in dem sich die Geo repliziert sekundäre Datenbank, die höher gestuft wird befindet diese Anweisung ausgeführt.

Dieses Feature ist für die Wiederherstellung vorgesehen, wenn Verfügbarkeit der Datenbank wiederherstellen entscheidend ist, und einige Datenverlust zulässig ist. Wenn erzwungenen Failover aufgerufen wird, wird die primäre Datenbank die angegebene sekundäre Datenbank sofort und beginnt mit dem Schreibtransaktionen akzeptiert. Sobald die ursprüngliche primäre Datenbank Verbindung mit dieser neuen primären Datenbank wiederherstellen kann, eine Sicherung inkrementelle in der ursprünglichen primären Datenbank geöffnet ist, und besteht aus der alten primären Datenbank in eine sekundäre Datenbank für die neue primäre Datenbank; Nachfolgend, ist es lediglich eine Synchronisierung von Kopie des neuen primären.

Jedoch, da Punkt In Zeit Wiederherstellen nicht auf der sekundären Datenbanken unterstützt wird, wenn der Benutzer Wiederherstellen von Daten in die alte primären Datenbank, die mit der neuen primären Datenbank nicht repliziert wurde hatten möchte, vor dem erzwungenen Failover übernommen, muss der Benutzer Support wiederherzustellende dies Datenverlust bei Teilnahme an.

Wenn die primäre Datenbank mehrere sekundäre Datenbanken aufweist, wird der anderen sekundäre Verbindung mit dem neuen primären automatisch neu der Befehl konfigurieren.

Gehen Sie folgendermaßen vor, um einer ungeplanten Failoverantwort auszulösen.

1. Schließen Sie in Management Studio an die logische Azure SQL-Datenbankserver in dem sich eine Geo repliziert sekundäre Datenbank befindet.

2. Öffnen Sie den Ordner Datenbanken, erweitern Sie den Ordner **System-Datenbanken** , mit der rechten Maustaste auf **master**, und klicken Sie dann auf **Neue Abfrage**.

3. Verwenden Sie folgende **ALTER DATABASE** -Anweisung, um die sekundäre Datenbank, die der primären Rolle zu wechseln.

        ALTER DATABASE <MyDB>   FORCE_FAILOVER_ALLOW_DATA_LOSS;

4. Klicken Sie auf **Ausführen** , um die Abfrage auszuführen.

>[AZURE.NOTE] Wenn der Befehl ausgegeben wird, wenn der primären und sekundären sind online die alte primär verwendet werden soll die neue sekundäre sofort ohne Daten Synchronisierung. Übergebenden Transaktionen, wenn der Befehl Datenverlust ausgegeben wird können auftreten, wenn der primäre ist.



## <a name="next-steps"></a>Nächste Schritte   

- Nach einem Failover stellen Sie sicher, dass die authentifizierungsanforderungen für Ihre Server und die Datenbank auf dem neuen primären konfiguriert sind. Details finden Sie unter [Sicherheit der SQL-Datenbank nach der Wiederherstellung](sql-database-geo-replication-security-config.md).
- Erfahren Sie, Wiederherstellen nach einem mit aktiven Geo-Replikation, einschließlich vor Datenverlust und Schritte nach der Wiederherstellung und Durchführung einer Disaster Wiederherstellung Drillup, finden Sie unter [Wiederherstellung](sql-database-disaster-recovery.md)
- Ein Sasha Nosov Blogbeitrag zur aktiven Geo-Replikation finden Sie unter [Spotlight auf neue Geo-Replikation-Funktionen](https://azure.microsoft.com/blog/spotlight-on-new-capabilities-of-azure-sql-database-geo-replication/)
- Informationen zum Entwickeln von Cloud Applikationen aktiven Geo-Replikation verwenden finden Sie unter [Erstellen eines Konzepts Cloud Applications für Geschäftskontinuität Geo-Replikation verwenden](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
- Informationen zur Verwendung von aktiven Geo-Replikation mit flexible Datenbank Pools finden Sie unter [Strategien für flexible Ressourcenpool zur Wiederherstellung](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
- Übersicht über Business Continurity finden Sie unter [Übersicht über die Business Continuity](sql-database-business-continuity.md)

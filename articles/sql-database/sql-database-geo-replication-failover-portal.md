<properties 
    pageTitle="Einleiten einen geplanten oder ungeplanten Failover für Azure SQL-Datenbank mit dem Portal Azure | Microsoft Azure" 
    description="Einleiten eines geplanten oder ungeplanten Failovers für Azure SQL-Datenbank mit dem Azure-portal" 
    services="sql-database" 
    documentationCenter="" 
    authors="stevestein" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-management" 
    ms.date="08/29/2016"
    ms.author="sstein"/>

# <a name="initiate-a-planned-or-unplanned-failover-for-azure-sql-database-with-the-azure-portal"></a>Einleiten eines geplanten oder ungeplanten Failovers für Azure SQL-Datenbank mit dem Azure-portal


> [AZURE.SELECTOR]
- [Azure-portal](sql-database-geo-replication-failover-portal.md)
- [PowerShell](sql-database-geo-replication-failover-powershell.md)
- [T-SQL](sql-database-geo-replication-failover-transact-sql.md)


In diesem Artikel werden die Failover mit einer sekundären SQL-Datenbank mit dem [Portal Azure](http://portal.azure.com)einleiten. Zum Konfigurieren der Geo-Replikation finden Sie unter [Konfigurieren von Geo-Replikation für SQL Azure-Datenbank](sql-database-geo-replication-portal.md).


## <a name="initiate-a-failover"></a>Ein Failover initiiert

Sekundäre Datenbank kann zur primären ausgeschaltet werden.  

1. In der mit der primären Datenbank in der Zusammenarbeit Geo-Replikation [Azure-Portal](http://portal.azure.com) durchsuchen.
2. Wählen Sie in der SQL-Datenbank Blade, **Alle Einstellungen** > **Geo-Replikation**.
3. Wählen Sie in der Liste **sekundäre** die Datenbank, die Konvertierung in die neue primäre, und klicken Sie auf **Failover**werden soll.

    ![Failover][2]

4. Klicken Sie auf **Ja,** um das Failover zu beginnen.

Der Befehl wird sofort die sekundäre Datenbank in die primäre Rolle wechseln. 

Es gibt eine kurze Zeit, während, die eine beide Datenbanken (der Reihenfolge der zu 25 0 Sekunden) nicht verfügbar sind, während die Rollen aktiviert sind. Wenn die primäre Datenbank mehrere sekundäre Datenbanken aufweist, wird der anderen sekundäre Verbindung mit dem neuen primären automatisch neu der Befehl konfigurieren. Der gesamte Vorgang sollte weniger als eine Minute unter normalen Umständen dauern. 

>[AZURE.NOTE] Wenn der primäre online ist und Ausführen der Transaktionen, wenn der Befehl Datenverlust ausgegeben wird, kann auftreten.


## <a name="next-steps"></a>Nächste Schritte   

- Nach einem Failover stellen Sie sicher, dass die authentifizierungsanforderungen für Ihre Server und die Datenbank auf dem neuen primären konfiguriert sind. Details finden Sie unter [Sicherheit der SQL-Datenbank nach der Wiederherstellung](sql-database-geo-replication-security-config.md).
- Erfahren Sie, Wiederherstellen nach einem mit aktiven Geo-Replikation, einschließlich vor Datenverlust und Schritte nach der Wiederherstellung und Durchführung einer Disaster Wiederherstellung Drillup, finden Sie unter [Disaster Wiederherstellung einen Drilldown](sql-database-disaster-recovery.md)
- Ein Sasha Nosov Blogbeitrag zur aktiven Geo-Replikation finden Sie unter [Spotlight auf neue Geo-Replikation-Funktionen](https://azure.microsoft.com/blog/spotlight-on-new-capabilities-of-azure-sql-database-geo-replication/)
- Informationen zum Entwickeln von Cloud Applikationen aktiven Geo-Replikation verwenden finden Sie unter [Erstellen eines Konzepts Cloud Applications für Geschäftskontinuität Geo-Replikation verwenden](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
- Informationen zur Verwendung von aktiven Geo-Replikation mit flexible Datenbank Pools finden Sie unter [Strategien für flexible Ressourcenpool zur Wiederherstellung](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
- Übersicht über Business Continurity finden Sie unter [Übersicht über die Business Continuity](sql-database-business-continuity.md)




<!--Image references-->
[1]: ./media/sql-database-geo-replication-failover-portal/failover.png
[2]: ./media/sql-database-geo-replication-failover-portal/secondaries.png

<properties 
    pageTitle="Einleiten einen geplanten oder ungeplanten Failover für SQL Azure-Datenbank mit PowerShell | Microsoft Azure" 
    description="Einleiten eines geplanten oder ungeplanten Failovers für Azure SQL-Datenbank mithilfe der PowerShell" 
    services="sql-database" 
    documentationCenter="" 
    authors="stevestein" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management" 
    ms.date="08/29/2016"
    ms.author="sstein"/>

# <a name="initiate-a-planned-or-unplanned-failover-for-azure-sql-database-with-powershell"></a>Einleiten eines geplanten oder ungeplanten Failovers für SQL Azure-Datenbank mit PowerShell



> [AZURE.SELECTOR]
- [Azure-portal](sql-database-geo-replication-failover-portal.md)
- [PowerShell](sql-database-geo-replication-failover-powershell.md)
- [T-SQL](sql-database-geo-replication-failover-transact-sql.md)


In diesem Artikel wird gezeigt, wie einen geplanten oder ungeplanten Failover für SQL-Datenbank mit PowerShell einleiten werden kann. Zum Konfigurieren der Geo-Replikation finden Sie unter [Konfigurieren von Geo-Replikation für SQL Azure-Datenbank](sql-database-geo-replication-powershell.md).



## <a name="initiate-a-planned-failover"></a>Einleiten eines geplanten Failovers

Verwenden Sie das Cmdlet " **Set-AzureRmSqlDatabaseSecondary** " mit dem Parameter **- Failover** Höherstufen eine sekundäre Datenbank zum neuen primären Datenbank, die vorhandene Primär, um die Konvertierung in einem sekundären Herabstufen zu machen. Dieses Feature ist bei einem geplanten Failover, wie vorgesehen, während der Wiederherstellung einen Drilldown Disaster, und setzt voraus, dass die primäre Datenbank zur Verfügung.

Der Befehl führt eine der folgenden Workflow aus:

1. Vorübergehend wechseln Sie Replikation zu synchroner Modus Dadurch werden alle ausstehende Transaktionen in den sekundären geleert werden.

2. Wechseln Sie in die Zusammenarbeit Geo-Replikation die Rollen der beiden Datenbanken.  

Diese Sequenz wird sichergestellt, dass die beiden Datenbanken synchronisiert werden, bevor die Rollen wechseln und daher keine Daten verloren. Es gibt eine kurze Zeit, während, die eine beide Datenbanken (der Reihenfolge der zu 25 0 Sekunden) nicht verfügbar sind, während die Rollen aktiviert sind. Der gesamte Vorgang sollte weniger als eine Minute unter normalen Umständen dauern. Weitere Informationen finden Sie unter [Festlegen-AzureRmSqlDatabaseSecondary](https://msdn.microsoft.com/library/mt619393.aspx).




Dieses Cmdlet zurückgegeben werden kann, wenn der Vorgang den Wechsel zwischen der sekundären Datenbank auf Primär abgeschlossen ist.

Mit dem folgende Befehl wechselt die Rollen in der Datenbank mit dem Namen "Mydb" auf den Server "srv2" unter der Ressource Gruppe "rg2" auf Primär. Der ursprünglichen Primär mit dem "db2" zu verbunden wurde wechselt zu sekundäre, nachdem die beiden Datenbanken vollständig synchronisiert werden.

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" –ResourceGroupName "rg2” –ServerName "srv2”
    $database | Set-AzureRmSqlDatabaseSecondary -Failover


> [AZURE.NOTE] In Ausnahmefällen ist es möglich, dass der Vorgang kann nicht abgeschlossen werden möglicherweise nicht reagiert angezeigt. In diesem Fall kann der Benutzer Aufrufen des Befehls Failover Force (ungeplanten Failover) und akzeptieren Daten verloren gehen.


## <a name="initiate-an-unplanned-failover-from-the-primary-database-to-the-secondary-database"></a>Einleiten eines ungeplanten Failovers aus der primären Datenbank auf die sekundäre Datenbank


Sie können das Cmdlet " **Set-AzureRmSqlDatabaseSecondary** " mit **– Failover-** und **AllowDataLoss -** Parameter verwenden, Höherstufen eine sekundäre Datenbank, um die neue primäre Datenbank in einer ungeplanten Weise, werden Erzwingen der Herabstufen der vorhandenen primären einer sekundären nacheinander vorgesehen ist, wenn die primäre Datenbank nicht mehr verfügbar ist.

Dieses Feature ist für die Wiederherstellung vorgesehen, wenn Verfügbarkeit der Datenbank wiederherstellen entscheidend ist, und einige Datenverlust zulässig ist. Wenn erzwungenen Failover aufgerufen wird, wird die primäre Datenbank die angegebene sekundäre Datenbank sofort und beginnt mit dem Schreibtransaktionen akzeptiert. Sobald die ursprüngliche primäre Datenbank mehr mit diesem neuen primären Datenbank nach dem Vorgang erzwungenen Failover herstellen ist, eine Sicherung inkrementelle in der ursprünglichen primären Datenbank geöffnet ist, und besteht aus der alten primären Datenbank in eine sekundäre Datenbank für die neue primäre Datenbank; Nachfolgend, ist es lediglich eine Kopie des neuen primären.

Aber da Punkt In Zeit Wiederherstellen nicht auf sekundären Datenbanken unterstützt wird, wenn Sie möchten diese Daten in die alte primären Datenbank die nicht auf der neuen primären Datenbank repliziert wurde hatten übernommen, sollten Sie zum Wiederherstellen einer Datenbank in der bekannten Sicherung CSS populärer.

> [AZURE.NOTE] Wenn der Befehl ausgegeben wird, wenn der primären und sekundären sind online die alte primär verwendet werden soll die neue sekundäre sofort ohne Daten Synchronisierung. Übergebenden Transaktionen, wenn der Befehl Datenverlust ausgegeben wird können auftreten, wenn der primäre ist.


Wenn die primäre Datenbank mehrere sekundäre, die der Befehl teilweise erfolgreich durchgeführt werden kann enthält. Die Ausführung des Befehls sekundäre werden primären. Die alte primär bleiben jedoch primäre, d. h. die zwei Grundfarben werden in inkonsistenten Zustand einhandeln und durch einen Link angehalten Replikation verbunden sind. Der Benutzer muss diese Konfiguration mithilfe einer "sekundären entfernen"-API auf beiden primären Datenbanken manuell zu reparieren.


Mit dem folgende Befehl wechselt die Rollen in der Datenbank mit dem Namen "Mydb" primäre, wenn die primäre nicht verfügbar ist. Die ursprüngliche Primär mit dem "Mydb" zu verbunden wurde wechselt zu sekundären, nachdem sie wieder online ist. An diesem Punkt kann die Synchronisierung von Datenverlusten führen.

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" –ResourceGroupName "rg2” –ServerName "srv2”
    $database | Set-AzureRmSqlDatabaseSecondary –Failover -AllowDataLoss




## <a name="next-steps"></a>Nächste Schritte   

- Nach einem Failover stellen Sie sicher, dass die authentifizierungsanforderungen für Ihre Server und die Datenbank auf dem neuen primären konfiguriert sind. Details finden Sie unter [Sicherheit der SQL-Datenbank nach der Wiederherstellung](sql-database-geo-replication-security-config.md).
- Erfahren Sie, Wiederherstellen nach einem mit aktiven Geo-Replikation, einschließlich vor Datenverlust und Schritte nach der Wiederherstellung und Durchführung einer Disaster Wiederherstellung Drillup, finden Sie unter [Disaster Wiederherstellung einen Drilldown](sql-database-disaster-recovery.md)
- Ein Sasha Nosov Blogbeitrag zur aktiven Geo-Replikation finden Sie unter [Spotlight auf neue Geo-Replikation-Funktionen](https://azure.microsoft.com/blog/spotlight-on-new-capabilities-of-azure-sql-database-geo-replication/)
- Informationen zum Entwickeln von Cloud Applikationen aktiven Geo-Replikation verwenden finden Sie unter [Erstellen eines Konzepts Cloud Applications für Geschäftskontinuität Geo-Replikation verwenden](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
- Informationen zur Verwendung von aktiven Geo-Replikation mit flexible Datenbank Pools finden Sie unter [Strategien für flexible Ressourcenpool zur Wiederherstellung](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
- Übersicht über Business Continurity finden Sie unter [Übersicht über die Business Continuity](sql-database-business-continuity.md)

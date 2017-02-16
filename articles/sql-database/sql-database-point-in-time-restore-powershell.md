<properties
    pageTitle="Wiederherstellen einer SQL Azure-Datenbank zu einem vorherigen Punkt Zeitpunkt (PowerShell) | Microsoft Azure"
    description="Wiederherstellen einer SQL Azure-Datenbank zu einem vorherigen Punkt Zeitpunkt"
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
    ms.workload="NA"
    ms.date="07/17/2016"
    ms.author="sstein"/>

# <a name="restore-an-azure-sql-database-to-a-previous-point-in-time-with-powershell"></a>Wiederherstellen einer SQL Azure-Datenbank zu einem vorherigen Punkt Zeitpunkt mit PowerShell

> [AZURE.SELECTOR]
- [(Übersicht)](sql-database-recovery-using-backups.md)
- [Point-In-Time wiederherstellen: Azure-portal](sql-database-point-in-time-restore-portal.md)

In diesem Artikel wird gezeigt, wie die Datenbank zu einem früheren Zeitpunkt aus [SQL-Datenbank automatische Sicherungskopien](sql-database-automated-backups.md)wiederherstellen. Hierzu können Sie mithilfe der PowerShell.

[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="restore-your-database-to-a-point-in-time-as-a-standalone-database"></a>Wiederherstellen Sie Ihrer Datenbank an eine Stelle in der Zeit als eigenständige Datenbank

1. Abrufen der Datenbank, die mithilfe von [Get-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt603648(v=azure.300\).aspx) Cmdlet wiederhergestellt werden soll.

        $Database = Get-AzureRmSqlDatabase -ResourceGroupName "resourcegroup01" -ServerName "server01" -DatabaseName "database01"

2. Wiederherstellen der Datenbank bis zu einem Zeitpunkt [wiederherstellen-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt693390(v=azure.300\).aspx) Cmdlet.

        Restore-AzureRmSqlDatabase –FromPointInTimeBackup –PointInTime UTCDateTime -ResourceGroupName $Database.ResourceGroupName -ServerName $Database.ServerName -TargetDatabaseName "RestoredDatabase" –ResourceId $Database.ResourceID -Edition "Standard" -ServiceObjectiveName "S2"


## <a name="restore-your-database-to-a-point-in-time-into-an-elastic-database-pool"></a>Wiederherstellen Sie Ihrer Datenbank bis zu einem Zeitpunkt in einer Datenbank flexible Ressourcenpool

1. Abrufen der Datenbank, die mithilfe von [Get-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt603648(v=azure.300\).aspx) Cmdlet wiederhergestellt werden soll.

        $Database = Get-AzureRmSqlDatabase -ResourceGroupName "resourcegroup01" -ServerName "server01" -DatabaseName "database01"

2. Wiederherstellen der Datenbank bis zu einem Zeitpunkt [wiederherstellen-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt693390(v=azure.300\).aspx) Cmdlet.

        Restore-AzureRmSqlDatabase –FromPointInTimeBackup –PointInTime UTCDateTime -ResourceGroupName $Database.ResourceGroupName -ServerName $Database.ServerName -TargetDatabaseName "RestoredDatabase" –ResourceId $Database.ResourceID –ElasticPoolName "elasticpool01"


## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht über Business-Continuity und Szenarien finden Sie unter [Übersicht über die Business continuity](sql-database-business-continuity.md)
- Weitere Informationen zu Azure SQL-Datenbank automatische Sicherungskopien finden Sie unter [SQL-Datenbank automatische Sicherungskopien](sql-database-automated-backups.md)
- Weitere Informationen zum automatische Sicherungskopien für Wiederherstellung verwenden, finden Sie unter [Wiederherstellen einer Datenbank aus den Dienst initiiert Sicherungskopien](sql-database-recovery-using-backups.md)
- Weitere Informationen zu schneller Wiederherstellungsoptionen finden Sie unter [Aktiv-Geo-Replikation](sql-database-geo-replication-overview.md)  
- Weitere Informationen zum Verwenden automatische Sicherungskopien für Archivierung, finden Sie unter [Datenbank kopieren](sql-database-copy.md)

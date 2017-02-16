<properties
    pageTitle="Wiederherstellen einer SQL Azure-Datenbank aus einer Geo redundante Sicherung (PowerShell) | Microsoft Azure"
    description="Wiederherstellen einer SQL Azure-Datenbank in einem neuen Server aus einer Sicherung Geo redundante"
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

# <a name="restore-an-azure-sql-database-from-a-geo-redundant-backup-by-using-powershell"></a>Wiederherstellen einer SQL Azure-Datenbank aus einer Sicherung Geo redundante mithilfe der PowerShell


> [AZURE.SELECTOR]
- [(Übersicht)](sql-database-recovery-using-backups.md)
- [Geo-wiederherstellen: Azure-Portal](sql-database-geo-restore-portal.md)

In diesem Artikel wird gezeigt, wie Sie die Datenbank in einem neuen Server wiederherstellen Geo-wiederherstellen. Dies kann über PowerShell erfolgen.

[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="geo-restore-your-database-into-a-standalone-database"></a>Die Datenbank in einer Datenbank eigenständigen Geo-wiederherstellen

1. Abrufen der Geo redundante Sicherungskopie der Datenbank, die mithilfe von [Get-AzureRmSqlDatabaseGeoBackup] (https://msdn.microsoft.com/library/azure/mt693388(v=azure.300\).aspx) Cmdlet wiederhergestellt werden soll.

        $GeoBackup = Get-AzureRmSqlDatabaseGeoBackup -ResourceGroupName "resourcegroup01" -ServerName "server01" -DatabaseName "database01"

2. Starten der Wiederherstellung aus der Geo redundante Sicherung mithilfe von [wiederherstellen-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt693390(v=azure.300\).aspx) Cmdlet.

        Restore-AzureRmSqlDatabase –FromGeoBackup -ResourceGroupName "TargetResourceGroup" -ServerName "TargetServer" -TargetDatabaseName "RestoredDatabase" –ResourceId $GeoBackup.ResourceID -Edition "Standard" -RequestedServiceObjectiveName "S2"


## <a name="geo-restore-your-database-into-an-elastic-database-pool"></a>Geo-Wiederherstellen Ihrer Datenbank in einer Datenbank flexible Ressourcenpool

1. Abrufen der Geo redundante Sicherungskopie der Datenbank, die mithilfe von [Get-AzureRmSqlDatabaseGeoBackup] (https://msdn.microsoft.com/library/azure/mt693388(v=azure.300\).aspx) Cmdlet wiederhergestellt werden soll.

        $GeoBackup = Get-AzureRmSqlDatabaseGeoBackup -ResourceGroupName "resourcegroup01" -ServerName "server01" -DatabaseName "database01"

2. Starten der Wiederherstellung aus der Geo redundante Sicherung mithilfe von [wiederherstellen-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt693390(v=azure.300\).aspx) Cmdlet. Geben Sie den Ressourcenpool Namen, dem die Datenbank in wiederhergestellt werden soll.

        Restore-AzureRmSqlDatabase –FromGeoBackup -ResourceGroupName "TargetResourceGroup" -ServerName "TargetServer" -TargetDatabaseName "RestoredDatabase" –ResourceId $GeoBackup.ResourceID –ElasticPoolName "elasticpool01"  


## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht über Business-Continuity und Szenarien finden Sie unter [Übersicht über die Business Continuity](sql-database-business-continuity.md).
- Weitere Informationen zu Azure SQL-Datenbank automatische Sicherungskopien finden Sie in der [SQL-Datenbank automatische Sicherungskopien](sql-database-automated-backups.md).
- Weitere Informationen zum automatische Sicherungskopien für Wiederherstellung verwenden, finden Sie unter [Wiederherstellen einer Datenbank aus den Dienst initiiert Sicherungskopien](sql-database-recovery-using-backups.md).
- Weitere Informationen zu schneller Wiederherstellungsoptionen finden Sie unter [Aktiv-Geo-Replikation](sql-database-geo-replication-overview.md).  
- Weitere Informationen zum Verwenden automatische Sicherungskopien für Archivierung, finden Sie unter [Datenbank kopieren](sql-database-copy.md).

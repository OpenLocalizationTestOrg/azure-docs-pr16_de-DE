<properties
   pageTitle="Wiederherstellen einer SQL Azure Datawarehouse (PowerShell) | Microsoft Azure"
   description="Zum Wiederherstellen einer Azure SQL-Data Warehouse PowerShell-Aufgaben."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="Lakshmi1812"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/21/2016"
   ms.author="lakshmir;barbkess;sonyama"/>

# <a name="restore-an-azure-sql-data-warehouse-powershell"></a>Wiederherstellen einer SQL Azure Datawarehouse (PowerShell)

> [AZURE.SELECTOR]
- [(Übersicht)][]
- [Portal][]
- [PowerShell][]
- [REST][]

In diesem Artikel erfahren Sie, wie eine Azure SQL Data Warehouse mithilfe der PowerShell wiederhergestellt werden.

## <a name="before-you-begin"></a>Vorbemerkung

**Überprüfen Sie Ihre DTU Kapazität aus.** Jede SQL Data Warehouse gehostet wird von einer SQLServer (z. B. myserver.database.windows.net) die ein DTU Kontingent hat.  Bevor Sie eine SQL Data Warehouse wiederherstellen können, überprüfen Sie Folgendes der SQLServer verfügt über genügend verbleibende DTU Kontingent für die Datenbank, die wiederhergestellt wird. Erfahren Sie, wie erforderlich DTU berechnen oder weitere DTU anfordern finden Sie unter [DTU Kontingent Änderung anfordern][].

### <a name="install-powershell"></a>Installieren der PowerShell

Mit Data Warehouse von SQL Azure-PowerShell verwenden möchten, müssen Sie Azure PowerShell Version 1.0 oder höher installieren.  Überprüfen Sie die Version, indem Sie ausführen **Get-Modul - ListAvailable-Namen AzureRM**.  Die neueste Version kann von [Microsoft Web Platform Installer][]installiert werden.  Weitere Informationen zum Installieren der neuesten Version von Informationen Sie [zum Installieren und Konfigurieren von Azure PowerShell][].

## <a name="restore-an-active-or-paused-database"></a>Wiederherstellen einer Datenbank aktiven oder angehalten

Verwenden Sie zum Wiederherstellen eine Datenbank aus einer Momentaufnahme des [Wiederherstellen-AzureRmSqlDatabase][] PowerShell-Cmdlets.

1. Windows PowerShell zu öffnen.
2. Verbinden mit Ihrem Konto Azure und Auflisten aller Abonnements, die mit Ihrem Konto verbunden ist.
3. Wählen Sie das Abonnement mit der Datenbank wiederhergestellt werden.
4. Listen Sie die Wiederherstellungspunkte, für die Datenbank.
5. Wählen Sie den gewünschten Wiederherstellungspunkt mithilfe der RestorePointCreationDate aus.
6. Stellen Sie die Datenbank auf die gewünschte wiederherstellen, zeigen Sie wieder her.
7. Stellen Sie sicher, dass die wiederhergestellte Datenbank online ist.

```Powershell

$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

# List the last 10 database restore points
((Get-AzureRMSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName ($DatabaseName).RestorePointCreationDate)[-10 .. -1]

# Or list all restore points
Get-AzureRmSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Get the specific database to restore
$Database = Get-AzureRmSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Pick desired restore point using RestorePointCreationDate
$PointInTime="<RestorePointCreationDate>"  

# Restore database from a restore point
$RestoredDatabase = Restore-AzureRmSqlDatabase –FromPointInTimeBackup –PointInTime $PointInTime -ResourceGroupName $Database.ResourceGroupName -ServerName $Database.$ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $Database.ResourceID

# Verify the status of restored database
$RestoredDatabase.status

```

>[AZURE.NOTE] Nachdem die Wiederherstellung abgeschlossen ist, können Sie die wiederhergestellte Datenbank nach folgenden [Konfigurieren Ihrer Datenbank nach der Wiederherstellung][]konfigurieren.


## <a name="restore-a-deleted-database"></a>Wiederherstellen einer gelöschten Datenbank

Verwenden Sie zum Wiederherstellen einer gelöschten Datenbank das Cmdlet [AzureRmSqlDatabase wiederherstellen][] .

1. Windows PowerShell zu öffnen.
2. Verbinden mit Ihrem Konto Azure und Auflisten aller Abonnements, die mit Ihrem Konto verbunden ist.
3. Wählen Sie das Abonnement, das wiederhergestellt werden die gelöschte Datenbank enthält.
4. Abrufen von bestimmten gelöschten Datenbank.
5. Wiederherstellen von gelöschten Datenbank.
6. Stellen Sie sicher, dass die wiederhergestellte Datenbank online ist.

```Powershell
$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

# Get the deleted database to restore
$DeletedDatabase = Get-AzureRmSqlDeletedDatabaseBackup -ResourceGroupName $ResourceGroupNam -ServerName $ServerName -DatabaseName $DatabaseName

# Restore deleted database
$RestoredDatabase = Restore-AzureRmSqlDatabase –FromDeletedDatabaseBackup –DeletionDate $DeletedDatabase.DeletionDate -ResourceGroupName $DeletedDatabase.ResourceGroupName -ServerName $DeletedDatabase.ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $DeletedDatabase.ResourceID

# Verify the status of restored database
$RestoredDatabase.status
```

>[AZURE.NOTE] Nachdem die Wiederherstellung abgeschlossen ist, können Sie die wiederhergestellte Datenbank nach folgenden [Konfigurieren Ihrer Datenbank nach der Wiederherstellung][]konfigurieren.


## <a name="restore-from-an-azure-geographical-region"></a>Wiederherstellen Sie aus einer Azure geografische region

Zum Wiederherstellen einer Datenbank, verwenden Sie das Cmdlet [AzureRmSqlDatabase wiederherstellen][] .

1. Windows PowerShell zu öffnen.
2. Verbinden mit Ihrem Konto Azure und Auflisten aller Abonnements, die mit Ihrem Konto verbunden ist.
3. Wählen Sie das Abonnement mit der Datenbank wiederhergestellt werden.
4. Rufen Sie die Datenbank, die Sie wiederherstellen möchten.
5. Erstellen Sie die Anfrage Wiederherstellung für die Datenbank ein.
6. Überprüfen Sie den Status der Datenbank Geo wiederhergestellt werden soll.

```Powershell
Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName "<Subscription_name>"

# Get the database you want to recover
$GeoBackup = Get-AzureRmSqlDatabaseGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourServerName>" -DatabaseName "<YourDatabaseName>"

# Recover database
$GeoRestoredDatabase = Restore-AzureRmSqlDatabase –FromGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourTargetServer>" -TargetDatabaseName "<NewDatabaseName>" –ResourceId $GeoBackup.ResourceID

# Verify that the geo-restored database is online
$GeoRestoredDatabase.status
```

>[AZURE.NOTE] Zum Konfigurieren Ihrer Datenbank nach Abschluss die Wiederherstellung finden Sie in [Ihrer Datenbank nach der Wiederherstellung konfigurieren][]. 


Die wiederhergestellte Datenbank werden TDE aktiviert, wenn die Quelldatenbank TDE aktiviert ist.


## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen über die Business Continuity-Features von Azure SQL-Datenbank-Editionen, lesen Sie die [Azure SQL-Datenbank Business Continuity (Übersicht)][].

<!--Image references-->

<!--Article references-->
[Azure SQL-Datenbank Business Continuity (Übersicht)]: sql-database-business-continuity.md
[Anfordern einer Änderung der DTU Kontingent]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change
[Konfigurieren Sie die Datenbank nach der Wiederherstellung]: ./sql-database-disaster-recovery.md#configure-your-database-after-recovery
[So installieren und Konfigurieren von Azure PowerShell]: powershell-install-configure.md
[(Übersicht)]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md
[Konfigurieren Sie die Datenbank nach der Wiederherstellung]: ./sql-database-disaster-recovery.md#configure-your-database-after-recovery

<!--MSDN references-->
[Wiederherstellen-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx

<!--Other Web references-->
[Azure Portal]: https://portal.azure.com/
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps

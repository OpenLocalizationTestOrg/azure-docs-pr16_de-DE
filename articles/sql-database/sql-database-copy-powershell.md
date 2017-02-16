<properties 
    pageTitle="Kopieren eine SQL Azure-Datenbank mithilfe der PowerShell | Microsoft Azure" 
    description="Erstellen Sie Kopieren einer SQL Azure-Datenbank mithilfe der PowerShell" 
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="09/08/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="copy-an-azure-sql-database-using-powershell"></a>Kopieren einer SQL Azure-Datenbank mithilfe der PowerShell


> [AZURE.SELECTOR]
- [(Übersicht)](sql-database-copy.md)
- [Azure-portal](sql-database-copy-portal.md)
- [PowerShell](sql-database-copy-powershell.md)
- [T-SQL](sql-database-copy-transact-sql.md)

In diesem Artikel wird das Kopieren einer SQL-Datenbank mit PowerShell auf dem gleichen Server, auf einem anderen Server, oder kopieren Sie eine Datenbank in einer [Datenbank flexible Ressourcenpool](sql-database-elastic-pool.md)dargestellt. Der Datenbank Kopiervorgang verwendet das Cmdlet " [New-AzureRmSqlDatabaseCopy](https://msdn.microsoft.com/library/mt603644.aspx) ". 


Wenn Sie in diesem Artikel abgeschlossen haben, benötigen Sie Folgendes:

- Einer SQL Azure-Datenbank (eine Datenbank zum Kopieren). Wenn Sie nicht mit eine SQL-Datenbank verfügen, erstellen Sie eine folgen die Schritten in diesem Artikel: [Erstellen Ihrer ersten Azure SQL-Datenbank](sql-database-get-started.md).
- Die neueste Version von Azure PowerShell. Ausführliche Informationen finden Sie unter [Informationen zum Installieren und konfigurieren Azure PowerShell](../powershell-install-configure.md).


Viele neue Funktionen der SQL-Datenbank werden nur unterstützt, wenn Sie das [Modell zur Bereitstellung von Azure Ressourcenmanager](../azure-resource-manager/resource-group-overview.md), damit die [Azure SQL-Datenbank-PowerShell-Cmdlets](https://msdn.microsoft.com/library/azure/mt574084.aspx) für Ressourcenmanager Beispielen verwenden. Werden das vorhandenen klassischen Modell zur Bereitstellung von [Azure SQL-Datenbank (klassische) Cmdlets](https://msdn.microsoft.com/library/azure/dn546723.aspx) für Abwärtskompatibilität unterstützt, aber es empfiehlt sich, dass Sie die Ressourcenmanager Cmdlets verwenden zu können.


>[AZURE.NOTE] Abhängig von der Größe der Datenbank kann der Kopiervorgang einige Zeit in Anspruch dauern.


## <a name="copy-a-sql-database-to-the-same-server"></a>Kopieren einer SQL-Datenbank auf dem gleichen server

Zum Erstellen der Kopie auf dem gleichen Server Auslassen der `-CopyServerName` Parameter (oder legen Sie ihn auf dem gleichen Server).

    New-AzureRmSqlDatabaseCopy -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -CopyDatabaseName "database1_copy"

## <a name="copy-a-sql-database-to-a-different-server"></a>Kopieren einer SQL-Datenbank auf einem anderen server

Um die neue Kopie auf einem anderen Server erstellt werden, enthalten die `-CopyServerName` Parameter, und legen Sie es auf einem anderen Server. Der Server *Kopieren* , muss bereits vorhanden sein. Es ist in einer anderen Ressourcengruppe, und Sie müssen auch einschließen der `-CopyResourceGroupName` Parameter.

    New-AzureRmSqlDatabaseCopy -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -CopyServerName "server2" -CopyDatabaseName "database1_copy"


## <a name="copy-a-sql-database-into-an-elastic-database-pool"></a>Kopieren einer SQL-Datenbank in einer Datenbank flexible Ressourcenpool

Um eine Kopie einer SQL-Datenbank in einem Ressourcenpool zu erstellen, legen Sie die `-ElasticPoolName` Parameter zu einem vorhandenen Pool.

    New-AzureRmSqlDatabaseCopy -ResourceGroupName "resourcegoup1" -ServerName "server1" -DatabaseName "database1" -CopyResourceGroupName "poolResourceGroup" -CopyServerName "poolServer1" -CopyDatabaseName "database1_copy" -ElasticPoolName "poolName"


## <a name="resolve-logins"></a>Beheben von Benutzernamen

Um den Benutzernamen aufgelöst nach Abschluss des Kopiervorgangs, finden Sie unter [Benutzernamen auflösen](sql-database-copy-transact-sql.md#resolve-logins-after-the-copy-operation-completes)


## <a name="example-powershell-script"></a>Beispiel für PowerShell-Skript

Das folgende Skript wird davon ausgegangen, Ressourcengruppen für alle, Servern, und der Pool bereits vorhanden sind (mit vorhandenen Ressourcen ersetzen Sie die Variablenwerten). Alles muss, eine Ausnahme bilden jedoch die Datenbankkopie vorhanden sein.

    # Sign in to Azure and set the subscription to work with
    # ------------------------------------------------------
    $SubscriptionId = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    Add-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $SubscriptionId
    
    
    # SQL database source (the existing database to copy)
    # ---------------------------------------------------
    $sourceDbName = "db1"
    $sourceDbServerName = "server1"
    $sourceDbResourceGroupName = "rg1"
    
    # SQL database copy (the new db to be created)
    # --------------------------------------------
    $copyDbName = "db1_copy"
    $copyDbServerName = "server2"
    $copyDbResourceGroupName = "rg2"
    
    # Copy a database to the same server
    # ----------------------------------
    New-AzureRmSqlDatabaseCopy -ResourceGroupName $sourceDbResourceGroupName -ServerName $sourceDbServerName -DatabaseName $sourceDbName -CopyDatabaseName $copyDbName
    
    # Copy a database to a different server
    # -------------------------------------
    New-AzureRmSqlDatabaseCopy -ResourceGroupName $sourceDbResourceGroupName -ServerName $sourceDbServerName -DatabaseName $sourceDbName -CopyResourceGroupName $copyDbResourceGroupName -CopyServerName $copyDbServerName -CopyDatabaseName $copyDbName
    
    # Copy a database into an elastic database pool
    # ---------------------------------------------
    $poolName = "pool1"
    
    New-AzureRmSqlDatabaseCopy -ResourceGroupName $sourceDbResourceGroupName -ServerName $sourceDbServerName -DatabaseName $sourceDbName -CopyResourceGroupName $copyDbResourceGroupName -CopyServerName $copyDbServerName -ElasticPoolName $poolName -CopyDatabaseName $copyDbName



    

## <a name="next-steps"></a>Nächste Schritte

- Übersicht über das Kopieren einer Azure SQL-Datenbank finden Sie unter [kopieren eine SQL Azure-Datenbank](sql-database-copy.md) .
- Kopieren eine Datenbank mit dem Azure-Portal [kopieren eine Azure SQL-Datenbank mit dem Azure-Portal](sql-database-copy-portal.md) finden Sie unter.
- [Kopieren einer SQL Azure-Datenbank mithilfe von T-SQL](sql-database-copy-transact-sql.md) kopieren eine Datenbank mithilfe von Transact-SQL finden Sie unter.
- Erfahren Sie, [wie SQL Azure-Datenbank Sicherheit nach der Wiederherstellung verwalten](sql-database-geo-replication-security-config.md) Informationen zum Verwalten von Benutzern und Benutzernamen, beim Kopieren einer Datenbank auf einem anderen logischen Server.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Neue AzureRmSqlDatabase](https://msdn.microsoft.com/library/mt603644.aspx)
- [Get-AzureRmSqlDatabaseActivity](https://msdn.microsoft.com/library/mt603687.aspx)
- [Verwalten von Benutzernamen](sql-database-manage-logins.md)
- [Verbinden mit SQL-Datenbank mit SQL Server Management Studio, und führen Sie eine Stichproben T-SQL-Abfrage](sql-database-connect-query-ssms.md)
- [Exportieren Sie die Datenbank in einer BACPAC](sql-database-export.md)
- [Business Continuity (Übersicht)](sql-database-business-continuity.md)
- [SQL-Datenbank-Dokumentation](https://azure.microsoft.com/documentation/services/sql-database/)
- [SQL Azure Datenbank PowerShell-Cmdlet Bezug](https://msdn.microsoft.com/library/mt574084.aspx)

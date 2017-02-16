<properties
    pageTitle="Erstellen einen neuen Datenbank flexible Pool mit PowerShell | Microsoft Azure"
    description="Informationen zum PowerShell verwenden, um die Skalierung Azure SQL-Datenbank Ressourcen durch das Erstellen eines skalierbare flexible Datenbank Pools so, dass mehrere Datenbanken verwalten."
    services="sql-database"
    documentationCenter=""
    authors="srinia"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management"
    ms.date="05/27/2016"
    ms.author="srinia"/>

# <a name="create-a-new-elastic-database-pool-with-powershell"></a>Erstellen eines neuen Datenbank flexible Pools mit PowerShell

> [AZURE.SELECTOR]
- [Azure-portal](sql-database-elastic-pool-create-portal.md)
- [PowerShell](sql-database-elastic-pool-create-powershell.md)
- [C#](sql-database-elastic-pool-create-csharp.md)


Informationen Sie zum Erstellen einer [Datenbank flexible Ressourcenpool](sql-database-elastic-pool.md) PowerShell-Cmdlets verwenden. 

Allgemeine Fehlercodes, finden Sie unter [SQL-Fehlercodes für SQL-Datenbank-Clientanwendungen: Verbindungsfehler und andere Probleme-Datenbank](sql-database-develop-error-messages.md).

> [AZURE.NOTE] Flexible Pools sind in der Regel verfügbar (GA) in allen Azure Regionen außer US North Central und Westen Indien, wo es zurzeit in der Vorschau ist.  Flexible Pools in diesen Regionen GA wird so früh wie möglich bereitgestellt werden. Darüber hinaus unterstützen flexible Pools derzeit nicht Datenbanken mithilfe von [in-Memory OLTP- oder im Arbeitsspeicher](sql-database-in-memory.md).


Sie müssen Azure PowerShell 1.0 oder höher ausgeführt werden. Ausführliche Informationen finden Sie unter [Informationen zum Installieren und konfigurieren Azure PowerShell](../powershell-install-configure.md).

## <a name="create-a-new-pool"></a>Erstellen einer neuen Ressourcenpool

Das Cmdlet [AzureRmSqlElasticPool neu](https://msdn.microsoft.com/library/azure/mt619378.aspx) erstellt ein neues Pool. Die Werte für eDTU pro Pool, min und max Dtus werden durch den Wert für Dienst Ebene (Basic, Standard oder Premium) eingeschränkt. Finden Sie unter [eDTU und Speicher für flexible Pools und flexible Datenbanken beschränkt](sql-database-elastic-pool.md#eDTU-and-storage-limits-for-elastic-pools-and-elastic-databases).

    New-AzureRmSqlElasticPool -ResourceGroupName "resourcegroup1" -ServerName "server1" -ElasticPoolName "elasticpool1" -Edition "Standard" -Dtu 400 -DatabaseDtuMin 10 -DatabaseDtuMax 100


## <a name="create-a-new-elastic-database-in-a-pool"></a>Erstellen Sie eine neue flexible Datenbank in einem Ressourcenpool

Verwenden Sie das Cmdlet [AzureRmSqlDatabase neu](https://msdn.microsoft.com/library/azure/mt619339.aspx) , und legen Sie den Parameter **ElasticPoolName** zum Zielpool. Wenn Sie eine vorhandene Datenbank in einem Ressourcenpool verschieben möchten, finden Sie unter [Verschieben einer Datenbank in eine flexible Ressourcenpool](sql-database-elastic-pool-manage-powershell.md#Move-a-database-into-an-elastic-pool).

    New-AzureRmSqlDatabase -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"

## <a name="create-a-pool-and-populate-it-with-multiple-new-databases"></a>Erstellen Sie einen Pool und füllen Sie es mit mehreren neuen Datenbanken 

Erstellung einer großen Anzahl von Datenbanken in einem Ressourcenpool kann dauern Abschluss mit dem Portal oder PowerShell-Cmdlets, die jeweils nur eine einzelne Datenbank zu erstellen. Um die Erstellung in einem neuen Ressourcenpool zu automatisieren, finden Sie unter [CreateOrUpdateElasticPoolAndPopulate ](https://gist.github.com/billgib/d80c7687b17355d3c2ec8042323819ae).   

## <a name="example-create-a-pool-using-powershell"></a>Beispiel: Erstellen einer Ressourcenpool mithilfe der PowerShell 

Dieses Skript erstellt neue Azure Ressourcengruppe und einen neuen Server. Wenn Sie dazu aufgefordert werden, geben Sie einen Benutzernamen und das Kennwort für den neuen Server (nicht Ihre Azure Anmeldeinformationen) ein.

    $subscriptionId = '<your Azure subscription id>'
    $resourceGroupName = '<resource group name>'
    $location = '<datacenter location>'
    $serverName = '<server name>'
    $poolName = '<pool name>'
    $databaseName = '<database name>'

    Login-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $subscriptionId

    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    New-AzureRmSqlServer -ResourceGroupName $resourceGroupName -ServerName $serverName -Location $location -ServerVersion "12.0"
    New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $serverName -FirewallRuleName "rule1" -StartIpAddress "192.168.0.198" -EndIpAddress "192.168.0.199"

    New-AzureRmSqlElasticPool -ResourceGroupName $resourceGroupName -ServerName $serverName -ElasticPoolName $poolName -Edition "Standard" -Dtu 400 -DatabaseDtuMin 10 -DatabaseDtuMax 100

    New-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -ElasticPoolName $poolName -MaxSizeBytes 10GB



## <a name="next-steps"></a>Nächste Schritte

- [Verwalten Sie Ihrer Ressourcenpool](sql-database-elastic-pool-manage-powershell.md)
- [Flexible Aufträge erstellen](sql-database-elastic-jobs-overview.md) Flexible Aufträge ermöglichen Ihnen die T-SQL-Skripts für eine beliebige Anzahl von Datenbanken in dem Pool ausgeführt werden.
- [Eine Skalierung mit Azure SQL-Datenbank](sql-database-elastic-scale-introduction.md): Verwenden Sie flexible Datenbanktools zu skalieren möchten.


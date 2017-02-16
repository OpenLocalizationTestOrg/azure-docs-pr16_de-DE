<properties
    pageTitle="Verwalten von Azure SQL-Datenbank mit PowerShell | Microsoft Azure"
    description="Verwaltung der Azure SQL-Datenbank mit PowerShell."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="sstein"/>

# <a name="manage-azure-sql-database-with-powershell"></a>Verwalten von Azure SQL-Datenbank mit PowerShell


> [AZURE.SELECTOR]
- [Azure-portal](sql-database-manage-portal.md)
- [Transact-SQL (SSMS)](sql-database-manage-azure-ssms.md)
- [PowerShell](sql-database-manage-powershell.md)

In diesem Thema wird die PowerShell-Cmdlets, mit denen viele Azure SQL-Datenbank Aufgaben ausführen. Eine vollständige Liste finden Sie unter [Azure SQL-Datenbank-Cmdlets] (https://msdn.microsoft.com/library/mt574084(v=azure.300\).aspx).


## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie eine Ressourcengruppe für unsere SQL-Datenbank und Azure Ressourcenübersicht mit [neu-AzureRmResourceGroup] (https://msdn.microsoft.com/library/azure/mt759837(v=azure.300\).aspx) Cmdlet.

```
$resourceGroupName = "resourcegroup1"
$resourceGroupLocation = "northcentralus"
New-AzureRmResourceGroup -Name $resourceGroupName -Location $resourceGroupLocation
```

Weitere Informationen finden Sie unter [Verwenden von Azure PowerShell Azure Ressourcenmanager](../powershell-azure-resource-manager.md).
Ein Beispiel-Skript finden Sie unter [Erstellen einer SQL-Datenbank PowerShell-Skript](sql-database-get-started-powershell.md#create-a-sql-database-powershell-script).

## <a name="create-a-sql-database-server"></a>Erstellen Sie einen SQL-Datenbankserver

Erstellen Sie einen SQL-Datenbankserver mit [neu-AzureRmSqlServer] (https://msdn.microsoft.com/library/azure/mt603715(v=azure.300\).aspx) Cmdlet. Ersetzen Sie *server1* mit dem Namen des Servers ein. Servernamen müssen über alle Azure SQL-Datenbankserver eindeutig sein. Wenn Sie der Servernamen bereits geöffnet ist, erhalten Sie einen Fehler zurück. Dieser Befehl kann einige Minuten dauern. Die Ressourcengruppe muss bereits in Ihrem Abonnement vorhanden sein.

```
$resourceGroupName = "resourcegroup1"

$sqlServerName = "server1"
$sqlServerVersion = "12.0"
$sqlServerLocation = "northcentralus"
$serverAdmin = "loginname"
$serverPassword = "password" 
$securePassword = ConvertTo-SecureString –String $serverPassword –AsPlainText -Force
$creds = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $serverAdmin, $securePassword
    

$sqlServer = New-AzureRmSqlServer -ServerName $sqlServerName `
 -SqlAdministratorCredentials $creds -Location $sqlServerLocation `
 -ResourceGroupName $resourceGroupName -ServerVersion $sqlServerVersion
```

Weitere Informationen finden Sie unter [Was ist die SQL-Datenbank](sql-database-technical-overview.md). Ein Beispiel-Skript finden Sie unter [Erstellen einer SQL-Datenbank PowerShell-Skript](sql-database-get-started-powershell.md#create-a-sql-database-powershell-script).


## <a name="create-a-sql-database-server-firewall-rule"></a>Erstellen einer SQL-Datenbank Server Firewall-Regel

Erstellen Sie eine Firewall-Regel Zugriff auf den Server mit [neu-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603860(v=azure.300\).aspx) Cmdlet. Führen Sie den folgenden Befehl, ersetzen die Start- und IP-Adressen mit gültigen Werten für Ihren Kunden. Die Ressourcengruppe und Server muss bereits in Ihrem Abonnement vorhanden sein.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

$firewallRuleName = "firewallrule1"
$firewallStartIp = "0.0.0.0"
$firewallEndIp = "255.255.255.255"

New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName `
 -ServerName $sqlServerName -FirewallRuleName $firewallRuleName `
 -StartIpAddress $firewallStartIp -EndIpAddress $firewallEndIp
```

Um anderen Azure Services-Zugriff auf dem Server zu ermöglichen, erstellen Sie eine Firewall-Regel, und legen Sie beide die `-StartIpAddress` und `-EndIpAddress` auf **0.0.0.0**. Mit dieser Firewallregel spezielle lässt Azure Datenverkehr Zugriff auf den Server.

Weitere Informationen finden Sie unter [Azure SQL-Datenbank-Firewall](https://msdn.microsoft.com/library/azure/ee621782.aspx). Ein Beispiel-Skript finden Sie unter [Erstellen einer SQL-Datenbank PowerShell-Skript](sql-database-get-started-powershell.md#create-a-sql-database-powershell-script).


## <a name="create-a-sql-database-blank"></a>Erstellen einer SQL-Datenbank (leer)

Erstellen einer Datenbank mit [neu-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt619339(v=azure.300\).aspx) Cmdlet. Die Ressourcengruppe und Server muss bereits in Ihrem Abonnement vorhanden sein. 

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

$databaseName = "database1"
$databaseEdition = "Standard"
$databaseServiceLevel = "S0"

$currentDatabase = New-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName `
 -ServerName $sqlServerName -DatabaseName $databaseName `
 -Edition $databaseEdition -RequestedServiceObjectiveName $databaseServiceLevel
```

Weitere Informationen finden Sie unter [Was ist die SQL-Datenbank](sql-database-technical-overview.md). Ein Beispiel-Skript finden Sie unter [Erstellen einer SQL-Datenbank PowerShell-Skript](sql-database-get-started-powershell.md#create-a-sql-database-powershell-script).


## <a name="change-the-performance-level-of-a-sql-database"></a>Ändern Sie die Leistungsstufe einer SQL-Datenbank

Skalieren Sie Ihrer Datenbank nach oben oder unten mit [Set-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt619433(v=azure.300\).aspx) Cmdlet. Die Ressourcengruppe und Server-Datenbank müssen bereits in Ihrem Abonnement vorhanden sein. Festlegen der `-RequestedServiceObjectiveName` in ein einzelnes Leerzeichen (wie im folgenden Codeausschnitt) für die grundlegenden Ebene. Legen sie auf *S0*, *S1*, *P1*, *P6*usw., wie im vorherigen Beispiel für andere Schichten.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

$databaseName = "database1"
$databaseEdition = "Basic"
$databaseServiceLevel = " "

Set-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName `
 -ServerName $sqlServerName -DatabaseName $databaseName `
 -Edition $databaseEdition -RequestedServiceObjectiveName $databaseServiceLevel
```

Weitere Informationen finden Sie unter [SQL-Datenbank-Optionen und Leistung: zu verstehen, was in jeder Kategorie Dienst verfügbar ist](sql-database-service-tiers.md). Ein Beispiel-Skript finden Sie unter [Beispiel PowerShell-Skript so ändern Sie das Dienstalter Ebene und Leistung der SQL-Datenbank](sql-database-scale-up-powershell.md#sample-powershell-script-to-change-the-service-tier-and-performance-level-of-your-sql-database).

## <a name="copy-a-sql-database-to-the-same-server"></a>Kopieren einer SQL-Datenbank auf dem gleichen server

Kopieren einer SQL-Datenbank auf demselben Server mit [neu-AzureRmSqlDatabaseCopy] (https://msdn.microsoft.com/library/azure/mt603644(v=azure.300\).aspx) Cmdlet. Festlegen der `-CopyServerName` und `-CopyResourceGroupName` auf dieselben Werte wie die Quelle Datenbank-Server und Ressourcen Gruppe.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"
$databaseName = "database1"

$copyDatabaseName = "database1_copy"
$copyServerName = $sqlServerName
$copyResourceGroupName = $resourceGroupName

New-AzureRmSqlDatabaseCopy -DatabaseName $databaseName `
 -ServerName $sqlServerName -ResourceGroupName $resourceGroupName `
 -CopyDatabaseName $copyDatabaseName -CopyServerName $sqlServerName `
 -CopyResourceGroupName $copyResourceGroupName
```

Weitere Informationen finden Sie unter [Kopieren einer SQL Azure-Datenbank](sql-database-copy.md). Ein Beispiel-Skript finden Sie unter [einer SQL-Datenbank PowerShell-Skript zu kopieren](sql-database-copy-powershell.md#example-powershell-script).


## <a name="delete-a-sql-database"></a>Löschen einer SQL-Datenbank

Löschen einer SQL-Datenbank mit [entfernen-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt619368(v=azure.300\).aspx) Cmdlet. Die Ressourcengruppe und Server-Datenbank müssen bereits in Ihrem Abonnement vorhanden sein.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"
$databaseName = "database1"

Remove-AzureRmSqlDatabase -DatabaseName $databaseName `
 -ServerName $sqlServerName -ResourceGroupName $resourceGroupName
```

## <a name="delete-a-sql-database-server"></a>Löschen Sie einen SQL-Datenbankserver

Löschen eines Servers mit [entfernen-AzureRmSqlServer] (https://msdn.microsoft.com/library/azure/mt603488(v=azure.300\).aspx) Cmdlet.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

Remove-AzureRmSqlServer -ServerName $sqlServerName -ResourceGroupName $resourceGroupName
```

## <a name="create-and-manage-elastic-database-pools-using-powershell"></a>Erstellen und Verwalten von flexible Datenbank Pools mithilfe der PowerShell

Details zum Erstellen von flexible Datenbank Pools mithilfe der PowerShell finden Sie unter [Erstellen einer neuen Datenbank flexible Pool mit PowerShell](sql-database-elastic-pool-create-powershell.md).

Details zum Verwalten von flexible Datenbank Pools mithilfe der PowerShell finden Sie unter [Überwachen und Verwalten einer Datenbank flexible Ressourcenpool mit PowerShell](sql-database-elastic-pool-manage-powershell.md).



## <a name="related-information"></a>Weitere Informationen

- [SQL Azure-Datenbank-Cmdlets] (https://msdn.microsoft.com/library/azure/mt574084(v=azure.300\).aspx)
- [Azure-Cmdlet Verweis] (https://msdn.microsoft.com/library/azure/dn708514(v=azure.300\).aspx)

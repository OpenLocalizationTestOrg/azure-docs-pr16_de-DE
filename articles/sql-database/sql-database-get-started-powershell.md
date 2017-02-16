<properties
    pageTitle="Neue Setup von SQL-Datenbank mit PowerShell | Microsoft Azure"
    description="Sie lernen, wie jetzt mit PowerShell eine SQL-Datenbank zu erstellen. Allgemeine Datenbankaufgaben einrichten können über PowerShell-Cmdlets verwaltet werden."
    keywords="Erstellen Sie neue Sql-Datenbank, Setup der Datenbank"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="hero-article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management"
    ms.date="08/19/2016"
    ms.author="sstein"/>

# <a name="create-a-sql-database-and-perform-common-database-setup-tasks-with-powershell-cmdlets"></a>Erstellen einer SQL-Datenbank und Ausführen von allgemeinen Setup Datenbankaufgaben mit PowerShell-cmdlets


> [AZURE.SELECTOR]
- [Azure-portal](sql-database-get-started.md)
- [PowerShell](sql-database-get-started-powershell.md)
- [C#](sql-database-get-started-csharp.md)



Informationen Sie zum Erstellen einer SQL­Datenbank mithilfe der PowerShell-Cmdlets. (Flexible Datenbanken erstellen, finden Sie unter [Erstellen einer neuen Datenbank flexible Pool mit PowerShell](sql-database-elastic-pool-create-powershell.md).)


[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="database-setup-create-a-resource-group-server-and-firewall-rule"></a>Einrichten der Datenbank: Erstellen einer Ressourcengruppe, Server und Firewall-Regel

Nachdem Sie Zugriff auf die Cmdlets für Ihr Abonnement des ausgewählten Azure ausgeführt haben, wird im nächste Schritt wird die Ressourcengruppe hergestellt, die den Server enthält, in dem die Datenbank erstellt werden soll. Sie können den nächsten Befehl jeden gültigen Speicherort verwenden, wählen Sie bearbeiten. Ausführen von **(Get-AzureRmLocation | Where-Object {$_. Anbieter - Eq "Microsoft.Sql"}). Speicherort** können Sie eine Liste mit gültigen Speicherorten zu gelangen.

Führen Sie den folgenden Befehl aus, um eine Ressourcengruppe zu erstellen:

    New-AzureRmResourceGroup -Name "resourcegroupsqlgsps" -Location "westus"


### <a name="create-a-server"></a>Erstellen eines Servers

SQL-Datenbanken werden in Azure SQL-Datenbankserver erstellt. Führen Sie [neu-AzureRmSqlServer] (https://msdn.microsoft.com/library/azure/mt603715(v=azure.300\).aspx) Server zu erstellen. Der Name für den Server muss für alle Azure SQL-Datenbankserver eindeutig sein. Wenn Sie der Servernamen bereits geöffnet ist, erhalten Sie einen Fehler zurück. Auch beachtet besteht darin, dass dieser Befehl einige Minuten dauern. Sie können den Befehl mit einem beliebigen gültigen Standort, dass Sie auswählen, bearbeiten, aber verwenden Sie den Speicherort fest, den Sie für die im vorherigen Schritt erstellte Ressourcengruppe verwendet haben.

    New-AzureRmSqlServer -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -Location "westus" -ServerVersion "12.0"

Wenn Sie diesen Befehl ausführen, werden Sie für Ihren Benutzernamen und Kennwort aufgefordert. Geben Sie Ihre Anmeldeinformationen Azure nicht. Geben Sie stattdessen, Benutzername und Kennwort als der Serveradministrator erstellen. Das Skript unten in diesem Artikel wird gezeigt, wie die Serveranmeldeinformationen für den Code festgelegt werden.

Die Serverdetails angezeigt werden, nachdem der Server erfolgreich erstellt wurde.

### <a name="configure-a-server-firewall-rule-to-allow-access-to-the-server"></a>Konfigurieren Sie eine Firewall-Regel Server, um auf dem Server zugreifen dürfen

Zugriff auf den Server, müssen Sie eine Firewall-Regel herstellen. Führen Sie [neu-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603860(v=azure.300\).aspx) Befehl aus, die Start- und IP-Adressen mit gültigen Werten für Ihren Computer.

    New-AzureRmSqlServerFirewallRule -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -FirewallRuleName "rule1" -StartIpAddress "192.168.0.0" -EndIpAddress "192.168.0.0"

Die Details der Firewall-Regel angezeigt werden, nachdem die Regel erfolgreich erstellt wurde.

Damit andere Azure Dienste auf den Server zugreifen können, fügen Sie eine Firewall-Regel hinzu und setzen Sie den Start und die EndIpAddress auf 0.0.0.0. Diese Regel ermöglicht Azure Datenverkehr aus einem beliebigen Azure-Abonnement auf den Server zugreifen.

Weitere Informationen finden Sie unter [Azure SQL-Datenbank-Firewall](sql-database-firewall-configure.md).


## <a name="create-a-sql-database"></a>Erstellen einer SQL-Datenbank

Jetzt haben Sie eine Ressourcengruppe, einem Server und einer Firewall-Regel, die so konfiguriert ist, sodass Sie auf den Server zugreifen können.

Die folgenden [neu-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt619339(v=azure.300\).aspx) Befehl erstellt eine SQL-Datenbank (leere) auf der Standard-Service-Stufe, mit einer Leistungsstufe S1:


    New-AzureRmSqlDatabase -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -DatabaseName "database1" -Edition "Standard" -RequestedServiceObjectiveName "S1"


Die Details der Datenbank angezeigt werden, nachdem die Datenbank erfolgreich erstellt wurde.

## <a name="create-a-sql-database-powershell-script"></a>Erstellen einer SQL-Datenbank PowerShell-Skript

Das folgende PowerShell-Skript erstellt eine SQL-Datenbank und alle abhängigen Ressourcen. Ersetzen Sie den gesamten `{variables}` mit Werten, die speziell für Ihr Abonnement und Ressourcen (entfernen die **{}** aus, wenn Sie festlegen, dass die Werte).

    # Sign in to Azure and set the subscription to work with
    $SubscriptionId = "{subscription-id}"

    Add-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $SubscriptionId

    # CREATE A RESOURCE GROUP
    $resourceGroupName = "{group-name}"
    $rglocation = "{Azure-region}"
    
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $rglocation
    
    # CREATE A SERVER
    $serverName = "{server-name}"
    $serverVersion = "12.0"
    $serverLocation = "{Azure-region}"
    
    $serverAdmin = "{server-admin}"
    $serverPassword = "{server-password}" 
    $securePassword = ConvertTo-SecureString –String $serverPassword –AsPlainText -Force
    $serverCreds = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $serverAdmin, $securePassword
    
    $sqlDbServer = New-AzureRmSqlServer -ResourceGroupName $resourceGroupName -ServerName $serverName -Location $serverLocation -ServerVersion $serverVersion -SqlAdministratorCredentials $serverCreds
    
    # CREATE A SERVER FIREWALL RULE
    $ip = (Test-Connection -ComputerName $env:COMPUTERNAME -Count 1 -Verbose).IPV4Address.IPAddressToString
    $firewallRuleName = '{rule-name}'
    $firewallStartIp = $ip
    $firewallEndIp = $ip
    
    $fireWallRule = New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $serverName -FirewallRuleName $firewallRuleName -StartIpAddress $firewallStartIp -EndIpAddress $firewallEndIp
    
    
    # CREATE A SQL DATABASE
    $databaseName = "{database-name}"
    $databaseEdition = "{Standard}"
    $databaseSlo = "{S0}"
    
    $sqlDatabase = New-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -Edition $databaseEdition -RequestedServiceObjectiveName $databaseSlo
    
   
    # REMOVE ALL RESOURCES THE SCRIPT JUST CREATED
    #Remove-AzureRmResourceGroup -Name $resourceGroupName






## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie eine SQL-Datenbank erstellen und Ausführen von Setup für grundlegende Datenbankaufgaben, können Sie Folgendes:

- [Verwalten der SQL-Datenbank mit PowerShell](sql-database-manage-powershell.md)
- [Verbinden mit SQL-Datenbank mit SQL Server Management Studio, und führen Sie eine Stichproben T-SQL-Abfrage](sql-database-connect-query-ssms.md)


## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [SQL Azure-Datenbank-Cmdlets] (https://msdn.microsoft.com/library/azure/mt574084(v=azure.300\).aspx)
- [SQL Azure-Datenbank](https://azure.microsoft.com/documentation/services/sql-database/)

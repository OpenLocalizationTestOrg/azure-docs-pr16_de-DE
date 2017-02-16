<properties
    pageTitle="Konfigurieren von Azure SQL-Datenbank-Server Ebene Firewall-Regeln mithilfe der PowerShell | Microsoft Azure"
    description="Informationen Sie zum Konfigurieren der Firewall für IP-Adressen, die auf SQL Azure-Datenbanken zugreifen."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/09/2016"
    ms.author="sstein"/>


# <a name="configure-azure-sql-database-server-level-firewall-rules-by-using-powershell"></a>Konfigurieren Sie mithilfe der PowerShell Azure SQL-Datenbank-Server Ebene Firewall-Regeln


> [AZURE.SELECTOR]
- [(Übersicht)](sql-database-firewall-configure.md)
- [Azure-portal](sql-database-configure-firewall-settings.md)
- [TSQL](sql-database-configure-firewall-settings-tsql.md)
- [PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [REST-API](sql-database-configure-firewall-settings-rest.md)


Azure SQL-Datenbank verwendet Firewall-Regeln für Verbindungen mit Ihren Servern und Datenbanken zulassen. Sie können Server-Ebene sowie Datenbank Firewalleinstellungen für die master-Datenbank oder einer Benutzerdatenbank in der SQL-Datenbankserver Selektives Zugriff auf die Datenbank definieren.

> [AZURE.IMPORTANT] Um von Azure, Verbindung zu Ihrem Datenbankserver zu ermöglichen, müssen Azure Verbindungen aktiviert sein. Weitere Informationen zu Firewall-Regeln und Aktivieren von Verbindungen aus Azure finden Sie unter [Azure SQL-Datenbank-Firewall](sql-database-firewall-configure.md). Wenn Sie Verbindungen innerhalb der Begrenzungslinie Azure Cloud erstellen möchten, müssen Sie möglicherweise einige zusätzliche TCP-zu öffnen. Weitere Informationen finden Sie unter den "V12 der SQL-Datenbank: außerhalb im Vergleich mit einer innerhalb" Abschnitt [Ports jenseits 1433 für ADO.NET 4.5 und V12 der SQL-Datenbank](sql-database-develop-direct-route-ports-adonet-v12.md).


[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="create-server-firewall-rules"></a>Server-Firewall-Regeln erstellen

Server-Level-Firewall Regeln erstellt werden können, aktualisiert, und mithilfe von Azure PowerShell gelöscht.

Führen Sie zum Erstellen einer neuen Firewallregel auf Server Ebene [neu-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603860(v=azure.300\).aspx) Cmdlet. Im folgende Beispiel können einen Bereich von IP-Adressen auf dem Server "Contoso".

    New-AzureRmSqlServerFirewallRule -ResourceGroupName 'resourcegroup1' -ServerName 'Contoso' -FirewallRuleName "ContosoFirewallRule" -StartIpAddress '192.168.1.1' -EndIpAddress '192.168.1.10'       

Zum Ändern einer vorhandenen Server Ebene Firewallregel führen Sie aus [Set-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603789(v=azure.300\).aspx) Cmdlet. Im folgende Beispiel wird den Bereich der zulässigen IP-Adressen für die Regel, die mit dem Namen ContosoFirewallRule.

    Set-AzureRmSqlServerFirewallRule -ResourceGroupName 'resourcegroup1' –StartIPAddress 192.168.1.4 –EndIPAddress 192.168.1.10 –RuleName 'ContosoFirewallRule' –ServerName 'Contoso'

Zum Löschen einer vorhandenen Server Ebene Firewallregel führen Sie aus [entfernen-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603588(v=azure.300\).aspx) Cmdlet. Im folgende Beispiel wird die Regel, die mit dem Namen ContosoFirewallRule gelöscht.

    Remove-AzureRmSqlServerFirewallRule –RuleName 'ContosoFirewallRule' –ServerName 'Contoso'


## <a name="manage-firewall-rules-by-using-powershell"></a>Verwalten von Firewall-Regeln mithilfe der PowerShell

Sie können auch PowerShell verwenden, Firewall-Regeln verwalten. Weitere Informationen finden Sie unter den folgenden Themen:

* [Neue AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603860(v=azure.300\).aspx)
* [Entfernen-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603588(v=azure.300\).aspx)
* [Set-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603789(v=azure.300\).aspx)
* [Get-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603586(v=azure.300\).aspx)


## <a name="next-steps"></a>Nächste Schritte

Informationen zur Verwendung von Transact-SQL Server-Ebene sowie Datenbank Firewallregeln erstellen finden Sie unter [mit T-SQL Azure SQL-Datenbank konfigurieren Server-Ebene sowie Datenbank Firewall-Regeln](sql-database-configure-firewall-settings-tsql.md).

Informationen dazu, wie Sie mit anderen Methoden Server Ebene Firewallregeln erstellen finden Sie unter:

- [Konfigurieren von Azure SQL-Datenbank Server Ebene Firewallregeln mithilfe des Azure-Portals](sql-database-configure-firewall-settings.md)
- [Konfigurieren von Azure SQL-Datenbank-Server Ebene Firewall-Regeln verwenden die REST-API](sql-database-configure-firewall-settings-rest.md)

Ein Lernprogramm zum Erstellen einer Datenbank finden Sie unter [Erstellen einer SQL-Datenbank in Minuten über das Azure-Portal](sql-database-get-started.md).
Zum Herstellen einer Verbindung mit einer SQL Azure-Datenbank aus offene Quelle oder Drittanbieter-Produkten finden Sie unter [Client Schnellstart Codebeispielen mit SQL-Datenbank](https://msdn.microsoft.com/library/azure/ee336282.aspx).
So navigieren Sie zu der Datenbanken finden Sie unter [Verwalten der Datenbank Zugriff und Login Sicherheit](https://msdn.microsoft.com/library/azure/ee336235.aspx).


## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Sichern der Datenbank](sql-database-security.md)
- [Sicherheitscenter für SQL Server-Datenbank-Engine und SQL Azure-Datenbank](https://msdn.microsoft.com/library/bb510589)


<!--Image references-->
[1]: ./media/sql-database-configure-firewall-settings/AzurePortalBrowseForFirewall.png
[2]: ./media/sql-database-configure-firewall-settings/AzurePortalFirewallSettings.png
<!--anchors-->

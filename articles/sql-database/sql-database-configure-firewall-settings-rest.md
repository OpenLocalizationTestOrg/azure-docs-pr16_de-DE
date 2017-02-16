<properties
    pageTitle="Azure SQL-Datenbank-Server Ebene Firewall-Regeln, die mithilfe der REST-API | Microsoft Azure"
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


#  <a name="configure-azure-sql-database-server-level-firewall-rules-using-the-rest-api"></a>Konfigurieren von Azure SQL-Datenbank-Server Ebene Firewall-Regeln verwenden die REST-API


> [AZURE.SELECTOR]
- [(Übersicht)](sql-database-firewall-configure.md)
- [Azure-portal](sql-database-configure-firewall-settings.md)
- [TSQL](sql-database-configure-firewall-settings-tsql.md)
- [PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [REST-API](sql-database-configure-firewall-settings-rest.md)


Microsoft Azure SQL-Datenbank verwendet Firewall-Regeln für Verbindungen mit Ihren Servern und Datenbanken zulassen. Sie können Server-Ebene sowie Datenbank Firewalleinstellungen für das Master-Shape oder einer Benutzerdatenbank in Ihrer Azure SQL-Datenbankserver Selektives Zugriff auf die Datenbank definieren.

> [AZURE.IMPORTANT] Um von Azure, Verbindung zu Ihrem Datenbankserver zu ermöglichen, müssen Azure Verbindungen aktiviert sein. Weitere Informationen zu Firewall-Regeln und Aktivieren von Verbindungen aus Azure finden Sie unter [Azure SQL-Datenbank-Firewall](sql-database-firewall-configure.md). Wenn Sie Verbindungen innerhalb der Begrenzungslinie Azure Cloud erstellen möchten, müssen Sie möglicherweise einige zusätzliche TCP-zu öffnen. Weitere Informationen finden Sie unter der **V12 der SQL-Datenbank: außerhalb im Vergleich mit einer innerhalb** Abschnitt [Ports jenseits 1433 für ADO.NET 4.5 und V12 der SQL-Datenbank](sql-database-develop-direct-route-ports-adonet-v12.md)


## <a name="manage-server-level-firewall-rules-through-rest-api"></a>Verwalten der Server Ebene Firewallregeln über REST-API
1. Verwalten von Firewall-Regeln über REST-API muss authentifiziert werden. Informationen finden Sie unter [Entwicklers Leitfaden für Autorisierung API Ressourcenmanager Azure](../resource-manager-api-authentication.md).
2. Server-Ebene Regeln können, aktualisierte oder gelöschte mit REST-API erstellt werden

    Führen Sie zum Erstellen oder aktualisieren eine Ebene Server Firewall-Regel, die sich-Methode, die mit den folgenden aus:
 
        https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/firewallRules/{rule-name}?api-version={api-version}
    
    Anforderungstexts

        {
         "properties": { 
            "startIpAddress": "{start-ip-address}", 
            "endIpAddress": "{end-ip-address}
            }
        } 
 

    Führen Sie zum Entfernen einer vorhandenen Server Ebene Firewallregel die DELETE-Methode der folgende aus:
     
        https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/firewallRules/{rule-name}?api-version={api-version}


## <a name="manage-firewall-rules-using-the-rest-api"></a>Verwenden die REST-API Firewall-Regeln verwalten

* [Erstellen Sie oder aktualisieren Sie die Firewall-Regel](https://msdn.microsoft.com/library/azure/mt445501.aspx)
* [Löschen Sie die Firewall-Regel](https://msdn.microsoft.com/library/azure/mt445502.aspx)
* [Abrufen von Firewall-Regel](https://msdn.microsoft.com/library/azure/mt445503.aspx)
* [Liste aller Firewall-Regeln](https://msdn.microsoft.com/library/azure/mt604478.aspx)
 
## <a name="next-steps"></a>Nächste Schritte

Wie in diesem Artikel erfahren Sie, wie mit Transact-SQL Server-Ebene sowie Datenbank Firewall-Regeln erstellen finden Sie unter [mit T-SQL Azure SQL-Datenbank konfigurieren Server-Ebene sowie Datenbank Firewall-Regeln](sql-database-configure-firewall-settings-tsql.md). 

Für wie Artikel zum Erstellen von Firewallregeln Server Ebene mit anderen Methoden, finden Sie unter: 

- [Konfigurieren von Azure SQL-Datenbank Server Ebene Firewallregeln mithilfe des Azure-Portals](sql-database-configure-firewall-settings.md)
- [Konfigurieren von Azure SQL-Datenbank-Server Ebene Firewall-Regeln mithilfe der PowerShell](sql-database-configure-firewall-settings-powershell.md)

Ein Lernprogramm zum Erstellen einer Datenbank finden Sie unter [Erstellen einer SQL-Datenbank in Minuten über das Azure-Portal](sql-database-get-started.md).
Hilfe in Verbindung mit einer SQL Azure-Datenbank aus offene Quelle oder Drittanbieter-Produkten finden Sie unter [Client Schnellstart Codebeispielen mit SQL-Datenbank](https://msdn.microsoft.com/library/azure/ee336282.aspx).
So navigieren Sie zu der Datenbanken finden Sie unter [Verwalten der Datenbank Zugriff und Login Sicherheit](https://msdn.microsoft.com/library/azure/ee336235.aspx).


## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Sichern der Datenbank](sql-database-security.md)
- [Sicherheitscenter für SQL Server-Datenbank-Engine und SQL Azure-Datenbank](https://msdn.microsoft.com/library/bb510589)

<!--Image references-->
[1]: ./media/sql-database-configure-firewall-settings/AzurePortalBrowseForFirewall.png
[2]: ./media/sql-database-configure-firewall-settings/AzurePortalFirewallSettings.png
<!--anchors-->

 

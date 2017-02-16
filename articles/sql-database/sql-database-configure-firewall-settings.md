<properties
    pageTitle="Konfigurieren eine SQL-Datenbank-Server Ebene Firewall-Regel | Microsoft Azure"
    description="Informationen Sie zum Konfigurieren der Firewall für IP-Adressen, die SQL Azure-Server zugreifen."
    services="sql-database"
    documentationCenter=""
    authors="BYHAM"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article" 
    ms.date="08/30/2016"
    ms.author="rickbyh;carlrab"/>


# <a name="configure-an-azure-sql-database-server-level-firewall-rule-using-the-azure-portal"></a>Konfigurieren einer Azure SQL-Datenbank-Server Ebene-Firewall-Regel mithilfe der Azure-Portal


> [AZURE.SELECTOR]
- [(Übersicht)](sql-database-firewall-configure.md)
- [Azure-Portal](sql-database-configure-firewall-settings.md)
- [TSQL](sql-database-configure-firewall-settings-tsql.md)
- [PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [REST-API](sql-database-configure-firewall-settings-rest.md)

Azure SQL-Server verwendet Firewall-Regeln für Verbindungen mit Ihren Servern und Datenbanken zulassen. Sie können Server-Ebene sowie Datenbank Firewalleinstellungen für das Master-Shape oder einer Benutzerdatenbank in Ihrer SQL Azure-logische Server Selektives Zugriff auf die Datenbank definieren. In diesem Thema wird erläutert, Server Ebene Firewall-Regeln.

> [AZURE.IMPORTANT] Um von Azure, Verbindung zu Ihrem SQL Azure-Server zu ermöglichen, müssen Azure Verbindungen aktiviert sein. Um zu verstehen, wie die Firewall-Regeln funktionieren, finden Sie unter [zum Konfigurieren der Firewall eine SQL Azure-Server \- Übersicht](sql-database-firewall-configure.md). Wenn Sie Verbindungen innerhalb der Begrenzungslinie Azure Cloud erstellen möchten, müssen Sie möglicherweise einige zusätzliche TCP-zu öffnen. Weitere Informationen finden Sie unter der **V12 der SQL-Datenbank: außerhalb im Vergleich mit einer innerhalb** Abschnitt [Ports jenseits 1433 für ADO.NET 4.5 und V12 der SQL-Datenbank](sql-database-develop-direct-route-ports-adonet-v12.md)

**Empfehlungen:** Verwenden Sie Server Ebene Firewall-Regeln für Administratoren und, wenn Sie viele Datenbanken, die die gleichen Access-Anforderungen aufweisen, und Sie nicht jede Datenbank einzeln konfigurieren Zeit aufwenden möchten. Microsoft empfiehlt die Verwendung der Datenbank Ebene Firewall-Regeln, wann immer möglich, Sicherheit zu verbessern und die Datenbank leichter portierbar zu machen.

[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-create-new-server-firewall-portal.md)]

## <a name="manage-existing-server-level-firewall-rules-through-the-azure-portal"></a>Verwalten von vorhandenen Server Ebene Firewallregeln über das Azure-portal

Wiederholen Sie die Schritte zum Verwalten der Server Ebene Firewallregeln.

- Um den aktuellen Computer hinzuzufügen, klicken Sie auf Add Client-IP.
- Geben Sie zum Hinzufügen weiterer IP-Adressen in der Regelname, IP-Adresse starten und beenden IP-Adresse ein.
- Wenn Sie eine vorhandene Regel ändern möchten, klicken Sie auf eines der Felder in der Regel, und ändern.
- Um eine vorhandene Regel zu löschen, zeigen Sie auf die Regel, bis das X am Ende der Zeile angezeigt wird. Klicken Sie auf X, um die Regel zu entfernen.

Klicken Sie auf **Speichern** , um die Änderungen zu speichern.

## <a name="next-steps"></a>Nächste Schritte

Eine wie in diesem Artikel erfahren Sie, wie mit Transact-SQL Server-Ebene sowie Datenbank Firewall-Regeln erstellen finden Sie unter [Konfigurieren von Azure SQL-Datenbank-Server-Ebene sowie Datenbank Firewall-Regeln mit T-SQL](sql-database-configure-firewall-settings-tsql.md). 

Für wie Artikel zum Erstellen von Firewallregeln Server Ebene mit anderen Methoden, finden Sie unter: 

- [Konfigurieren von Azure SQL-Datenbank-Server Ebene Firewall-Regeln mithilfe der PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [Konfigurieren von Azure SQL-Datenbank-Server Ebene Firewall-Regeln verwenden die REST-API](sql-database-configure-firewall-settings-rest.md)

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

 

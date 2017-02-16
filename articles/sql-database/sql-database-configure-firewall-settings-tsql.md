<properties
    pageTitle="Azure SQL-Datenbank-Server-Ebene sowie Datenbank Firewall-Regeln, die mithilfe von T-SQL | Microsoft Azure"
    description="Informationen Sie zum Konfigurieren der Firewall für IP-Adressen, die auf SQL Azure-Datenbanken zugreifen."
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
    ms.author="rickbyh"/>


# <a name="configure-azure-sql-database-server-level-and-database-level-firewall-rules-using-t-sql"></a>Konfigurieren Sie mithilfe von T-SQL Azure SQL-Datenbank-Server-Ebene sowie Datenbank Firewall-Regeln


> [AZURE.SELECTOR]
- [(Übersicht)](sql-database-firewall-configure.md)
- [Azure-Portal](sql-database-configure-firewall-settings.md)
- [TSQL](sql-database-configure-firewall-settings-tsql.md)
- [PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [REST-API](sql-database-configure-firewall-settings-rest.md)


Microsoft Azure SQL-Datenbank verwendet Firewall-Regeln für Verbindungen mit Ihren Servern und Datenbanken zulassen. Sie können Server-Ebene sowie Datenbank Firewalleinstellungen für das Master-Shape oder einer Benutzerdatenbank in Ihrer Azure SQL-Datenbankserver Selektives Zugriff auf die Datenbank definieren.

> [AZURE.IMPORTANT] Um von Azure, Verbindung zu Ihrem Datenbankserver zu ermöglichen, müssen Azure Verbindungen aktiviert sein. Weitere Informationen zu Firewall-Regeln und Aktivieren von Verbindungen aus Azure finden Sie unter [Azure SQL-Datenbank-Firewall](sql-database-firewall-configure.md). Wenn Sie Verbindungen innerhalb der Begrenzungslinie Azure Cloud erstellen möchten, müssen Sie möglicherweise einige zusätzliche TCP-zu öffnen. Weitere Informationen finden Sie unter der **V12 der SQL-Datenbank: außerhalb im Vergleich mit einer innerhalb** Abschnitt [Ports jenseits 1433 für ADO.NET 4.5 und V12 der SQL-Datenbank](sql-database-develop-direct-route-ports-adonet-v12.md)


## <a name="server-level-firewall-rules"></a>Server-Level-Firewall-Regeln

Nur die wichtigsten Login Server Ebene oder Azure-Active Directory-Administrator kann eine Firewallregel Server Ebene mithilfe von Transact-SQL erstellen.

1. Starten eines Abfrage-Fensters und in die virtuelle master-Datenbank mithilfe von SQL Server Management Studio verbinden.
2. Server-Level-Firewall-Regeln können aus innerhalb des Abfragefensters aktiviert, erstellt, aktualisiert, oder gelöscht werden.
3. Führen Sie zum Erstellen oder aktualisieren Server Ebene Firewall-Regeln, die `sp_set_firewall_rule` gespeicherte Prozedur. Im folgende Beispiel können einen Bereich von IP-Adressen auf dem Server "Contoso".<br/>Zunächst, sehen, welche Regeln bereits vorhanden sein.

        SELECT * FROM sys.firewall_rules ORDER BY name;

    Fügen Sie eine Firewall-Regel.

        EXECUTE sp_set_firewall_rule @name = N'ContosoFirewallRule',
            @start_ip_address = '192.168.1.1', @end_ip_address = '192.168.1.10'

    Führen Sie zum Löschen einer Firewallregel Server Ebene die Sp_delete_firewall_rule gespeicherte Prozedur ein. Im folgende Beispiel wird die Regel, die mit dem Namen ContosoFirewallRule gelöscht.
 
        EXECUTE sp_delete_firewall_rule @name = N'ContosoFirewallRule'
 
 Weitere Informationen zu diesen gespeicherten Prozeduren finden Sie unter [Sp_set_firewall_rule](https://msdn.microsoft.com/library/dn270017.aspx) und [Sp_delete_firewall_rule](https://msdn.microsoft.com/library/dn270024.aspx).

## <a name="database-level-firewall-rules"></a>Datenbank Ebene Firewall-Regeln

Nur ein Datenbankbenutzer mit der Berechtigung zum **steuern** der Datenbank (beispielsweise den Datenbankbesitzer) kann eine Datenbank Ebene Firewall-Regel erstellen.

1. Starten Sie nach dem Erstellen einer Server-Level-Firewalls für Ihre IP-Adresse, ein Abfragefenster über das Portal Classic oder SQL Server Management Studio aus.
2. Verbinden Sie mit der Datenbank für die Sie eine Datenbank Ebene Firewall-Regel erstellen möchten.

    Führen Sie zum Erstellen einer neuen oder Aktualisieren einer vorhandenen Datenbank Ebene Firewallregel, die `sp_set_database_firewall_rule` gespeicherte Prozedur. Im folgende Beispiel wird eine neue Firewall-Regel, die mit dem Namen ContosoFirewallRule erstellt.
 
        EXEC sp_set_database_firewall_rule @name = N'ContosoFirewallRule', 
            @start_ip_address = '192.168.1.11', @end_ip_address = '192.168.1.11'
 
    Zum Löschen einer vorhandenen Datenbank Ebene Firewallregel Ausführen der `sp_delete_database_firewall_rule` gespeicherte Prozedur. Im folgende Beispiel wird die Regel, die mit dem Namen ContosoFirewallRule gelöscht.
`
   Mitarbeiter-Sp_delete_database_firewall_rule @name = N'ContosoFirewallRule'

Weitere Informationen zu diesen gespeicherten Prozeduren finden Sie unter [Sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) und [Sp_delete_database_firewall_rule](https://msdn.microsoft.com/library/dn270030.aspx).

## <a name="next-steps"></a>Nächste Schritte

Für wie Artikel zum Erstellen von Firewallregeln Server Ebene mit anderen Methoden, finden Sie unter: 

- [Konfigurieren von Azure SQL-Datenbank Server Ebene Firewallregeln mithilfe der Azure-Portal](sql-database-configure-firewall-settings.md)
- [Konfigurieren von Azure SQL-Datenbank-Server Ebene Firewall-Regeln mithilfe der PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [Konfigurieren von Azure SQL-Datenbank-Server Ebene Firewall-Regeln verwenden die REST-API](sql-database-configure-firewall-settings-rest.md)

Ein Lernprogramm zum Erstellen einer Datenbank finden Sie unter [Erstellen einer SQL-Datenbank in Minuten über das Azure-Portal](sql-database-get-started.md).
Hilfe in Verbindung mit einer SQL Azure-Datenbank aus offene Quelle oder Drittanbieter-Produkten finden Sie unter [Client Schnellstart Codebeispielen mit SQL-Datenbank](https://msdn.microsoft.com/library/azure/ee336282.aspx).
So navigieren Sie zu der Datenbanken finden Sie unter [Verwalten der Datenbank Zugriff und Login Sicherheit](https://msdn.microsoft.com/library/azure/ee336235.aspx).


## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Sichern der Datenbank](sql-database-security.md)
- [Sicherheitscenter für SQL Server-Datenbank-Engine und SQL Azure-Datenbank](https://msdn.microsoft.com/library/bb510589)

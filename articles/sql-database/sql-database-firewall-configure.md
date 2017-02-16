<properties
   pageTitle="Konfigurieren eine SQL Server-Firewall Übersicht | Microsoft Azure"
   description="Informationen Sie zum Konfigurieren der Firewall einer SQL-Datenbank mit Server-Ebene sowie Datenbank Firewall-Regeln zum Verwalten des Zugriffs."
   keywords="Datenbank-firewall"
   services="sql-database"
   documentationCenter=""
   authors="BYHAM"
   manager="jhubbard"
   editor="cgronlun"
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/14/2016"
   ms.author="rickbyh"/>

# <a name="configure-azure-sql-database-firewall-rules---overview"></a>Konfigurieren der Firewall-Regeln Azure SQL-Datenbank \- (Übersicht)


> [AZURE.SELECTOR]
- [(Übersicht)](sql-database-firewall-configure.md)
- [Azure-Portal](sql-database-configure-firewall-settings.md)
- [TSQL](sql-database-configure-firewall-settings-tsql.md)
- [PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [REST-API](sql-database-configure-firewall-settings-rest.md)


Microsoft Azure SQL-Datenbank bietet einen relationalen Datenbank für Azure und andere Internet-basierten Programme. Zum Schutz von Daten zu verhindern, dass Firewalls alle Zugriff auf dem Datenbankserver, bis Sie festlegen, welche Computer über die Berechtigung verfügen. Die Firewall gewährt Zugriff auf Datenbanken basierend auf die ursprüngliche IP-Adresse jeder Anforderung.

Zum Konfigurieren der Firewalls, erstellen Sie die Firewallregeln, die die zulässigen IP-Adressbereiche angeben. Sie können auf den Server und die Datenbank Ebenen Firewall-Regeln erstellen.

- **Server-Level-Firewall-Regeln:** Diese Regeln aktivieren Clients Zugriff auf Ihre gesamte SQL Azure-Server, d. h., alle Datenbanken innerhalb der gleichen logischen Server. Diese Regeln werden in der **master** -Datenbank gespeichert. Server-Level-Firewall-Regeln können mithilfe des Portals oder mithilfe eines Transact-SQL-Anweisungen konfiguriert sein.
- **Database-Level-Firewall-Regeln:** Diese Regeln aktivieren Clients Zugriff auf einzelne Datenbanken innerhalb Ihrer Azure SQL-Datenbankserver an. Sie können diese Regeln für jede Datenbank erstellen, und sie in den einzelnen Datenbanken gespeichert werden. (Sie können Datenbank Ebene Firewall-Regeln für die **master** -Datenbank erstellen). Diese Regeln können in Einschränken des Zugriffs auf bestimmte (sicheren) Datenbanken innerhalb der gleichen logischen Server hilfreich sein. Datenbank Ebene Firewall-Regeln können nur mithilfe von Transact-SQL-Anweisungen konfiguriert werden.

**Empfehlungen:** Microsoft empfiehlt die Verwendung der Datenbank Ebene Firewall-Regeln, wann immer möglich, Sicherheit zu verbessern und die Datenbank leichter portierbar zu machen. Verwenden Sie Server Ebene Firewall-Regeln für Administratoren und, wenn Sie viele Datenbanken, die die gleichen Access-Anforderungen haben und Sie nicht jede Datenbank einzeln konfigurieren Zeit aufwenden möchten.


## <a name="firewall-overview"></a>Firewall (Übersicht)

Alle Transact-SQL-Zugriff auf Ihre SQL Azure-Server ist zu Beginn durch die Firewall blockiert. Um Ihren SQL Azure-Server zu beginnen, müssen Sie Azure-Portal wechseln, und geben eine oder mehrere Server Ebene Firewallregeln, mit denen Zugriff auf Ihre SQL Azure-Server. Verwenden Sie die Firewall-Regeln, um anzugeben, welche IP-Adressbereiche aus dem Internet zulässig sind, und ob Azure Applications versuchen können, die Verbindung zu Ihrem SQL Azure-Server.

Zum Gewähren des Zugriffs auf nur einen in Ihrem SQL Azure-Server-Datenbanken Selektives müssen Sie eine Datenbank Ebene Regel für die erforderlichen Datenbank erstellen. Geben Sie einen IP-Adressenbereich für die Datenbank Firewall-Regel, die jenseits des IP-Adressbereichs in der Firewallregel Server Ebene angegeben ist, und stellen Sie sicher, dass die IP-Adresse des Clients des in der Regel Datenbank Ebene angegebenen Bereichs einschließt.

Versucht, eine Verbindung aus dem Internet und Azure müssen zuerst die Firewall passieren, bevor sie Ihre Azure SQL Server oder SQL-Datenbank erreichen können wie in der folgenden Abbildung gezeigt.

   ![Diagramm zur Beschreibung der Konfiguration der Firewall.][1]

## <a name="connecting-from-the-internet"></a>Herstellen einer Verbindung aus dem Internet

Wenn ein Computer versucht, auf dem Datenbankserver aus dem Internet verbinden, überprüft die Firewall zuerst die ursprüngliche IP-Adresse der Anfrage gegen den vollständigen Satz von Firewall-Regeln an:

- Wenn die IP-Adresse der Anfrage innerhalb einer in den Server Ebene Firewallregeln angegebenen Bereiche befindet, wird Ihr Azure SQL-Datenbankserver die Verbindung gewährt.

- Wenn die IP-Adresse der Anforderung nicht innerhalb einer der Bereiche, die in der Firewallregel Server Ebene angegeben ist, werden die Datenbank Ebene Firewallregeln überprüft. Wenn die IP-Adresse der Anfrage innerhalb einer der Bereiche, die in der Datenbank Ebene Firewall-Regeln angegeben ist, wird die Verbindung nur für die Datenbank gewährt, die eine übereinstimmende Datenbank Ebene Regel enthält.

- Wenn die IP-Adresse der Anfrage nicht innerhalb der Bereiche in einem der Server oder Datenbank-Ebene Firewall-Regeln, die Verbindung Anforderung fehlschlägt angegeben ist.

> [AZURE.NOTE] Um Azure SQL-Datenbank von Ihrem lokalen Computer zuzugreifen, stellen Sie sicher, dass die Firewall auf Ihrem Netzwerk und dem lokalen Computer ausgehenden Datenverkehr an Port 1433 zulässt.


## <a name="connecting-from-azure"></a>Herstellen einer Verbindung von Azure

Wenn eine Anwendung von Azure versucht, die Verbindung zu Ihrem Datenbankserver, die Firewall überprüft, ob Azure Verbindungen zulässig sind. Eine Firewall festlegen mit beginnt und endet Adresse 0.0.0.0 gleich Gibt an, dass diese Verbindungen zulässig sind. Wenn der Verbindungsversuch nicht zulässig ist, erreicht die Anfrage Azure SQL-Datenbankserver nicht.

> [AZURE.IMPORTANT] Diese Option konfiguriert die Firewall, um alle Verbindungen aus Azure einschließlich Verbindungen aus der anderen Kunden Abonnements zulässt. Wenn Sie diese Option auswählen, stellen Sie sicher, dass Ihre Anmelde- und Berechtigungen Zugriff auf nur autorisierte Benutzer zu beschränken.

Sie können Verbindungen aus Azure auf zwei Arten aktivieren:

- Wählen Sie im [Portal von Microsoft Azure](https://portal.azure.com/)-das Kontrollkästchen **Zulassen Azure Services mit Access-Server** beim Erstellen eines neuen Servers.

- Klicken Sie auf **Ja** für **Microsoft Azure Services**, im [Portal klassischen](http://go.microsoft.com/fwlink/p/?LinkID=161793)auf der Registerkarte " **Konfigurieren** " auf einem Server, klicken Sie im Abschnitt **Dienste zulässig** .

## <a name="creating-the-first-server-level-firewall-rule"></a>Die erste Ebene Server Firewallregel erstellen

Verwenden des [Azure-Portal](https://portal.azure.com/) oder programmgesteuert mithilfe der REST-API oder Azure PowerShell, kann die erste Ebene Server Firewall-Einstellung erstellt werden. Nachfolgende Server Ebene Firewall-Regeln erstellt werden können und mithilfe dieser Methoden verwaltet wie über Transact-SQL. Um die Leistung zu verbessern, zwischengespeichert Server Ebene Firewall-Regeln werden vorübergehend Ebene der Datenbank. Um den Cache zu aktualisieren, finden Sie unter [DBCC FLUSHAUTHCACHE](https://msdn.microsoft.com/library/mt627793.aspx). Weitere Informationen auf dem Server Ebene Firewall-Regeln finden Sie unter [wie: Konfigurieren der Firewall einer SQL Azure-Server über das Azure-Portal](sql-database-configure-firewall-settings.md).

## <a name="creating-database-level-firewall-rules"></a>Erstellen die Datenbank Ebene Firewall-Regeln

Nachdem Sie die erste Ebene Server Firewall konfiguriert haben, möchten Sie möglicherweise Einschränken des Zugriffs auf bestimmte Datenbanken. Wenn Sie einen IP-Adressenbereich in der Datenbank Ebene Firewallregel, die außerhalb des Bereichs angeben, in der Firewallregel Server Ebene angegeben ist, können nur solchen Clients, die IP-Adressen in der Datenbank Ebene Bereich haben die Datenbank zugreifen. Sie können maximal 128 Datenbank Ebene Firewall-Regeln für eine Datenbank haben. Datenbank-Level-Firewall-Regeln für die Datenbanken Master und Benutzer können erstellt haben und über Transact-SQL verwaltet werden. Weitere Informationen zum Konfigurieren der Datenbank Ebene Firewallregeln finden Sie unter [Sp_set_database_firewall_rule (Azure SQL-Datenbanken)](https://msdn.microsoft.com/library/dn270010.aspx).

## <a name="programmatically-managing-firewall-rules"></a>Verwalten von programmgesteuert Firewall-Regeln

Zusätzlich zu den Azure-Portal können programmgesteuert mithilfe von Transact-SQL, REST-API und Azure PowerShell Firewall-Regeln verwaltet werden. Die folgenden Tabellen beschreiben den Satz von Befehlen für jede Methode zur Verfügung.


### <a name="transact-sql"></a>Transact-SQL

| Katalogansicht oder eine gespeicherte Prozedur                                                           | Ebene     | Beschreibung                                          |
|--------------------------------------------------------------------------------------------|-----------|------------------------------------------------------|
| [Sys.firewall_rules](https://msdn.microsoft.com/library/dn269980.aspx)                   | Server    | Zeigt die aktuelle Ebene Server Firewallregeln     |
| [sp_set_firewall_rule](https://msdn.microsoft.com/library/dn270017.aspx)             | Server    | Erstellt oder aktualisiert Server Ebene Firewall-Regeln       |
| [sp_delete_firewall_rule](https://msdn.microsoft.com/library/dn270024.aspx)          | Server    | Entfernt die Server Ebene Firewall-Regeln                  |
| [Sys.database_firewall_rules](https://msdn.microsoft.com/library/dn269982.aspx)        | Datenbank  | Zeigt die aktuelle Datenbank Ebene Firewallregeln   |
| [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx)    | Datenbank  | Erstellt oder aktualisiert die Datenbank Ebene Firewallregeln |
| [sp_delete_database_firewall_rule](https://msdn.microsoft.com/library/dn270030.aspx) | Datenbanken | Entfernt Datenbank Ebene Firewall-Regeln                |

### <a name="rest-api"></a>REST-API

| API                  | Ebene  | Beschreibung                                                      |
|----------------------|--------|------------------------------------------------------------------|
| [Liste Firewall-Regeln](https://msdn.microsoft.com/library/azure/dn505715.aspx)  | Server | Zeigt die aktuelle Ebene Server Firewallregeln                 |
| [Firewall-Regel erstellen](https://msdn.microsoft.com/library/azure/dn505712.aspx) | Server | Erstellt oder aktualisiert Server Ebene Firewall-Regeln                   |
| [Festlegen der Firewall-Regel](https://msdn.microsoft.com/library/azure/dn505707.aspx)    | Server | Aktualisiert die Eigenschaften einer vorhandenen Server Ebene Firewall-Regel |
| [Löschen Sie die Firewall-Regel](https://msdn.microsoft.com/library/azure/dn505706.aspx) | Server | Entfernt die Server Ebene Firewall-Regeln                              |


### <a name="azure-powershell"></a>Azure PowerShell

| Cmdlet                                                                                              | Ebene  | Beschreibung                                                      |
|-----------------------------------------------------------------------------------------------------|--------|------------------------------------------------------------------|
| [Get-AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546731.aspx)    | Server | Gibt die aktuelle Ebene Server Firewall-Regeln                  |
| [Neue AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546724.aspx)    | Server | Erstellt eine neue Server Ebene Firewallregel                         |
| [Set-AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546739.aspx)    | Server | Aktualisiert die Eigenschaften einer vorhandenen Server Ebene Firewall-Regel |
| [Entfernen-AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546727.aspx) | Server | Entfernt die Server Ebene Firewall-Regeln                              |

> [AZURE.NOTE] Es können werden von wie 5 Minuten verzögern für Änderungen an den Firewalleinstellungen wirksam.

## <a name="troubleshooting-the-database-firewall"></a>Problembehandlung bei der Datenbank firewall

Berücksichtigen Sie die folgenden Punkte, beim Zugriff auf den Dienst Microsoft Azure SQL-Datenbank nicht so verhält wie erwartet:

- **Lokale Firewall-Konfiguration:** Damit Ihr Computer Azure SQL-Datenbank zugreifen kann, müssen Sie eine Firewallausnahme auf Ihrem Computer für TCP-Port 1433 zu erstellen. Wenn Sie innerhalb der Azure-Cloud-Begrenzungslinie Verbindungen herstellen, müssen Sie möglicherweise zusätzliche Ports geöffnet. Weitere Informationen finden Sie unter der **V12 der SQL-Datenbank: außerhalb im Vergleich mit einer innerhalb** Abschnitt [Ports jenseits 1433 für ADO.NET 4.5 und V12 der SQL-Datenbank](sql-database-develop-direct-route-ports-adonet-v12.md).

- **Netzwerk-Adresse (Netzwerkadressübersetzung):** Aufgrund von NAT möglicherweise anders als die IP-Adresse werden auf Ihrem Computer IP-Konfiguration Einstellungen die IP-Adresse von Ihrem Computer für die Verbindung mit Azure SQL-Datenbank verwendet. Um die IP-Adresse anzuzeigen, die Ihr Computer nutzt Verbindung zum Azure, melden Sie sich mit dem Portal, und navigieren Sie zur Registerkarte **Konfigurieren** auf dem Server, der die Datenbank hostet. Klicken Sie im Abschnitt **Zugelassene IP-Adressen** wird die **Aktuelle IP-Adresse** angezeigt. Klicken Sie auf **Hinzufügen** zu den **Zugelassenen IP-Adressen** auf diesem Computer zulassen Zugriff auf den Server.

- **Ändert sich in der Zulassungsliste nicht übernommen wurden noch:** Es gibt möglicherweise wie eine 5 Minuten verzögern für Änderungen an der Konfiguration der Firewall Azure SQL-Datenbank wirksam wird.

- **Der Benutzername ist nicht autorisiert oder ein falschen Kennwort verwendet wurde:** Wenn eine Anmeldung verfügt nicht über Berechtigungen für den Azure SQL-Datenbankserver oder das verwendete Kennwort falsch ist, wird die Verbindung zu den Azure SQL-Datenbankserver verweigert. Erstellen einer Firewall-Einstellung gibt nur Clients mit einer Verkaufschance versucht, Herstellen einer Verbindung mit dem Server. Jeder Client muss die notwendigen Sicherheitsanmeldeinformationen bereitstellen. Weitere Informationen zur Vorbereitung Benutzernamen finden Sie unter Verwalten von Datenbanken, Benutzernamen und Benutzer in Azure SQL-Datenbank.

- **Dynamische IP-Adresse:** Wenn Sie über einen Internetzugang mit dynamischen IP-Adressen und Probleme bei der erste durch die Firewall, können Sie eine Folgendes versuchen:

 - Bitten Sie Ihr Internetdienstanbieter (Internet Service Provider, ISP) für die IP-Adressbereich zugewiesen an die Clientcomputer, die den Azure SQL-Datenbankserver zugreifen, und klicken Sie dann fügen Sie im Bereich IP-Adresse als eine Firewall-Regel hinzu.

 - Erhalten Sie statische IP-Adressen sich stattdessen für Ihre Clientcomputer, und fügen Sie dann die IP-Adressen hinzu, als Firewall-Regeln.

## <a name="next-steps"></a>Nächste Schritte

Wie in folgenden Artikeln auf Server-Ebene sowie Datenbank Firewall-Regeln erstellen, finden Sie unter:

- [Konfigurieren von Azure SQL-Datenbank Server Ebene Firewallregeln mithilfe des Azure-Portals](sql-database-configure-firewall-settings.md)
- [Konfigurieren Sie mithilfe von T-SQL Azure SQL-Datenbank-Server-Ebene sowie Datenbank Firewall-Regeln](sql-database-configure-firewall-settings-tsql.md)
- [Konfigurieren von Azure SQL-Datenbank-Server Ebene Firewall-Regeln mithilfe der PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [Konfigurieren von Azure SQL-Datenbank-Server Ebene Firewall-Regeln verwenden die REST-API](sql-database-configure-firewall-settings-rest.md)

Ein Lernprogramm zum Erstellen einer Datenbank finden Sie unter [Erstellen einer SQL-Datenbank in Minuten über das Azure-Portal](sql-database-get-started.md).
Hilfe in Verbindung mit einer SQL Azure-Datenbank aus offene Quelle oder Drittanbieter-Produkten finden Sie unter [Client Schnellstart Codebeispielen mit SQL-Datenbank](https://msdn.microsoft.com/library/azure/ee336282.aspx).
So navigieren Sie zu der Datenbanken finden Sie unter [Verwalten der Datenbank Zugriff und Login Sicherheit](https://msdn.microsoft.com/library/azure/ee336235.aspx).



## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Sichern der Datenbank](sql-database-security.md)
- [Sicherheitscenter für SQL Server-Datenbank-Engine und SQL Azure-Datenbank](https://msdn.microsoft.com/library/bb510589)

<!--Image references-->
[1]: ./media/sql-database-firewall-configure/sqldb-firewall-1.png

<properties
   pageTitle="Richtlinien für SQL Azure-Datenbank Sicherheit und Einschränkungen | Microsoft Azure"
   description="Informationen Sie zu Microsoft Azure SQL-Datenbank-Richtlinien und Einschränkungen im Zusammenhang mit Sicherheit."
   services="sql-database"
   documentationCenter=""
   authors="BYHAM"
   manager="jhubbard"
   editor=""
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="10/18/2016"
   ms.author="rickbyh"/>

# <a name="azure-sql-database-security-guidelines-and-limitations"></a>Azure SQL-Datenbank Sicherheitsrichtlinien und Einschränkungen

In diesem Thema werden die Richtlinien des Microsoft Azure SQL-Datenbank und Einschränkungen im Zusammenhang mit Sicherheit. Beachten Sie die folgenden Punkte, wenn die Sicherheit Ihrer Azure SQL-Datenbanken verwalten.

## <a name="access-to-the-virtual-master-database"></a>Zugriff auf die virtuelle master-Datenbank

In der Regel benötigen nur Administratoren Zugriff auf die master-Datenbank. Routinemäßige Zugriff auf jede Benutzerdatenbank sollten über enthaltenen nicht-Administrator-Datenbankbenutzer in jeder Datenbank erstellt. Wenn Sie die enthaltenen Datenbankbenutzer verwenden, müssen Sie nicht Benutzernamen in der master-Datenbank zu erstellen. Weitere Informationen finden Sie unter [Enthaltenen Datenbankbenutzer - ausführenden Ihrer Datenbank tragbaren](https://msdn.microsoft.com/library/ff929188.aspx).


## <a name="firewall"></a>Firewall

Die SQL Server-Firewall, für den gesamten Azure SQL Server ausgelegte ist in der Regel über das Portal konfiguriert ist, und geben sollten nur die IP-Adressen, die von Administratoren verwendet. Zum Erstellen einer Datenbank ausgelegte Firewallregel, die die notwendigen IP-Adressen für jede Datenbank geöffnet wird, eine Verbindung mit einer Benutzerdatenbank aus, und verwenden Sie dann [Sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) Transact-SQL-Anweisung.

Der Dienst Azure SQL-Datenbank ist nur über TCP-Port 1433 verfügbar. Zugriff auf eine SQL-Datenbank von Ihrem Computer, stellen Sie sicher, dass Ihr Computer Client-Firewall ausgehenden TCP-Datenverkehr an Port 1433 zulässt. Wenn Sie für andere Programme nicht benötigt, blockieren Sie eingehende Verbindungen TCP-Anschluss 1433. 

Verbindungen aus Azure-virtuellen Computern sind als Teil der Verbindungsprozess auf eine andere IP-Adresse und den Port, für jeden Worker-Rolle eindeutiger umgeleitet. Die Port-Nummer wird im Bereich von 11000 zu 11999. Weitere Informationen zu TCP-finden Sie unter [Ports jenseits 1433 für ADO.NET 4.5 und V12 der SQL-Datenbank](sql-database-develop-direct-route-ports-adonet-v12.md).


## <a name="authentication"></a>Authentifizierung

Active Directory-Authentifizierung verwenden (integrierte Sicherheit) wann immer möglich. Informationen zum Konfigurieren von AD-Authentifizierung finden Sie unter [Herstellen einer Verbindung mit SQL Datenbank durch Verwenden von Azure Active Directory-Authentifizierung](sql-database-aad-authentication.md), und [Auswählen eines Authentifizierungsmodus](https://msdn.microsoft.com/library/ms144284.aspx) in SQL Server-Onlinedokumentation. 

Verwenden Sie enthaltenen Datenbankbenutzer Skalierbarkeit zu verbessern. Weitere Informationen finden Sie unter [Enthaltenen Datenbankbenutzer - ausführenden Ihrer Datenbank tragbaren](https://msdn.microsoft.com/library/ff929188.aspx), [Benutzer erstellen (Transact-SQL)](https://technet.microsoft.com/library/ms173463.aspx)und [Datenbanken enthalten sind](https://technet.microsoft.com/library/ff929071.aspx).

Die Datenbank-Engine schließt Verbindungen, die mehr als 30 Minuten im Leerlauf bleiben. Die Verbindung erneut müssen sich anmelden, bevor er verwendet werden kann.

Kontinuierlich aktive Verbindungen mit SQL-Datenbank (Ausführung durch die Datenbank-Engine) Reauthorization erfordern mindestens alle 10 Stunden. Die Datenbank-Engine versucht, mit dem ursprünglich gesendete Kennwort Reauthorization und ist keine Benutzereingabe erforderlich. Aus Gründen der Leistung beim Zurücksetzen des Kennworts in SQL-Datenbank, die Verbindung ist nicht erneut authentifiziert werden, auch wenn die Verbindung aufgrund von Verbindungspooling zurückgesetzt wird. Dies ist das Verhalten der lokalen SQL Server abweicht. Wenn das Kennwort geändert wurde, da die Verbindung ursprünglich autorisiert wurde die Verbindung muss abgeschlossen sein, und eine neue Verbindung hergestellt werden mit dem neuen Kennwort. Ein Benutzer mit der Berechtigung zum Abbrechen DATABASE CONNECTION kann eine Verbindung mit einer SQL-Datenbank mithilfe des Befehls [Abbrechen](https://msdn.microsoft.com/library/ms173730.aspx) explizit beenden.

## <a name="logins-and-users"></a>Benutzernamen und Benutzern

Beim Verwalten von Benutzernamen und Benutzern in SQL-Datenbank gibt es Einschränkungen.


- Sie müssen mit der **master** -Datenbank verbunden sein, bei der Ausführung der ``CREATE/ALTER/DROP DATABASE`` Anweisungen. -Der Datenbankbenutzer in das master-Datenbank, die wichtigsten Login Server Ebene entspricht, kann nicht geändert oder gelöscht werden soll. 
- US-Englisch ist die Standardsprache für die wichtigsten Server Ebene Login.
- Nur die Administratoren (Server Ebene Hauptbenutzer Login oder Azure AD-Administrator) und die Mitglieder der Rolle der **Dbmanager** in der **master** -Datenbank über die Berechtigung zum Ausführen verfügen der ``CREATE DATABASE`` und ``DROP DATABASE`` Anweisungen.
- Sie müssen mit der master-Datenbank verbunden sein, bei der Ausführung der ``CREATE/ALTER/DROP LOGIN`` Anweisungen. Mithilfe von Benutzernamen ist jedoch überfordert. Verwenden Sie stattdessen die enthaltenen Datenbankbenutzer.
- Informationen zum Verbinden mit einer Benutzerdatenbank müssen Sie den Namen der Datenbank in der Verbindungszeichenfolge angeben.
- Nur die wichtigsten Login Server Ebene und die Mitglieder der Rolle der **Loginmanager** in der **master** -Datenbank über die Berechtigung zum Ausführen verfügen der ``CREATE LOGIN``, ``ALTER LOGIN``, und ``DROP LOGIN`` Anweisungen.
- Bei der Ausführung der ``CREATE/ALTER/DROP LOGIN`` und ``CREATE/ALTER/DROP DATABASE`` Anweisungen in einer ADO.NET-Anwendung, parametrisierte Befehle mit ist nicht zulässig. Weitere Informationen finden Sie unter [Befehle und Parameter](https://msdn.microsoft.com/library/ms254953.aspx).
- Bei der Ausführung der ``CREATE/ALTER/DROP DATABASE`` und ``CREATE/ALTER/DROP LOGIN`` Anweisungen, jede der folgenden Aussagen muss die nur in einem Stapel Transact-SQL-Anweisung. Andernfalls tritt ein Fehler auf. Die folgende Transact-SQL-Anweisung prüft z. B., ob die Datenbank vorhanden ist. Falls vorhanden, eine ``DROP DATABASE`` Anweisung aufgerufen, um die Datenbank zu entfernen. Da die ``DROP DATABASE`` ist nicht die einzige Anweisung im Stapel, die Ausführung der folgenden Transact-SQL-Anweisung führt zu einem Fehler.

```
IF EXISTS (SELECT [name]
           FROM   [sys].[databases]
           WHERE  [name] = N'database_name')
     DROP DATABASE [database_name];
GO
```

- Bei der Ausführung der ``CREATE USER`` -Anweisung mit der ``FOR/FROM LOGIN`` Option muss es, die nur in einem Stapel Transact-SQL-Anweisung.
- Bei der Ausführung der ``ALTER USER`` -Anweisung mit der ``WITH LOGIN`` Option muss es, die nur in einem Stapel Transact-SQL-Anweisung.
- Zu ``CREATE/ALTER/DROP`` muss ein Benutzer die ``ALTER ANY USER`` Berechtigungen in der Datenbank.
- Wenn der Besitzer einer Datenbank-Funktion versucht, hinzufügen oder Entfernen eines anderen Datenbankbenutzers in den oder aus dieser Datenbankrolle, möglicherweise der folgende Fehler auftreten: **Benutzer oder eine Rolle 'Name' ist in dieser Datenbank nicht vorhanden.** Dieser Fehler tritt auf, da der Benutzer nicht auf den Besitzer von angezeigt wird. Um dieses Problem zu beheben, gewähren Sie den Besitzer der ``VIEW DEFINITION`` Berechtigung für die Benutzer. 

Weitere Informationen zu Benutzernamen und Benutzern finden Sie unter [Verwalten von Datenbanken und Benutzernamen in SQL Azure-Datenbank](sql-database-manage-logins.md).


## <a name="see-also"></a>Siehe auch

[SQL Azure-Datenbank Firewall](sql-database-firewall-configure.md)

[How to: Konfigurieren der Firewalleinstellungen (Azure-SQL­Datenbank)](sql-database-configure-firewall-settings.md)

[Verwalten von Datenbanken und Anmeldedaten in Azure SQL-Datenbank](sql-database-manage-logins.md)

[Sicherheitscenter für SQL Server-Datenbank-Engine und SQL Azure-Datenbank](https://msdn.microsoft.com/library/bb510589)

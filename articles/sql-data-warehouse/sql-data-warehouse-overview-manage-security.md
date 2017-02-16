<properties
   pageTitle="Sichern einer Datenbank in SQL Data Warehouse | Microsoft Azure"
   description="Tipps zum Sichern einer Datenbank in Azure SQL-Data Warehouse für die Entwicklung von Lösungen."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="ronortloff"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/24/2016"
   ms.author="rortloff;barbkess;sonyama"/>

# <a name="secure-a-database-in-sql-data-warehouse"></a>Sichern einer Datenbank in SQL Data Warehouse

> [AZURE.SELECTOR]
- [Übersicht über die Sicherheit](sql-data-warehouse-overview-manage-security.md)
- [Authentifizierung](sql-data-warehouse-authentication.md)
- [Verschlüsselung (Portal)](sql-data-warehouse-encryption-tde.md)
- [Verschlüsselung (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)

In diesem Artikel werden die Grundlagen zum Sichern Ihrer Datenbank Azure SQL-Data Warehouse führt. Insbesondere in diesem Artikel erhalten Sie die Schritte mit Ressourcen zum Einschränken des Zugriffs, Datenschutz und Überwachen der Aktivitäten in einer Datenbank.

## <a name="connection-security"></a>Verbindung-Sicherheit

Verbindung Sicherheit bezieht sich auf wie Sie einschränken und sichere Verbindungen mit Ihrer Datenbank über Firewall-Regeln und Verbindung Verschlüsselung.

Firewall-Regeln werden von den Server und die Datenbank verwendet, versucht, eine Verbindung von IP-Adressen ablehnen, die nicht explizit weißen Liste hinzugefügt wurden. Um Verbindungen aus Ihrer Anwendung oder Client-Rechner öffentliche IP-Adresse zulassen möchten, müssen Sie zuerst eine Ebene Server Firewall-Regel, die mithilfe der Azure-Portal, REST-API oder PowerShell erstellen. Als bewährte Methode sollten Sie die IP-Adressbereiche Ihre Serverfirewall so weit wie möglich passieren einschränken.  Um Azure SQL-Data Warehouse von Ihrem lokalen Computer zuzugreifen, stellen Sie sicher, dass die Firewall auf Ihrem Netzwerk und dem lokalen Computer ausgehenden Datenverkehr an Port 1433 zulässt.  Weitere Informationen finden Sie unter [Firewall Azure SQL-Datenbank][], [Sp_set_firewall_rule][]und [Sp_set_database_firewall_rule][].

Verbindungen mit Ihrer SQL Data Warehouse werden standardmäßig verschlüsselt.  Ändern von Verbindungseinstellungen, die Verschlüsselung deaktivieren, werden ignoriert.

## <a name="authentication"></a>Authentifizierung

Authentifizierung bezieht sich auf wie Ihre Identität beweisen, bei der Verbindung mit der Datenbank. SQL Data Warehouse unterstützt derzeit SQL Server-Authentifizierung mit einen Benutzernamen und ein Kennwort sowie eine Azure-Active Directory. 

Wenn Sie den logischen Server für die Datenbank erstellt haben, können Sie eine Anmeldung "Serveradministrator" mit einen Benutzernamen und ein Kennwort angegeben. Verwenden diese Anmeldeinformationen, können Sie mit einer Datenbank auf diesem Server als den Datenbankbesitzer oder "Dbo" durch den SQL Server-Authentifizierung authentifizieren.

Jedoch sollte als bewährte Methode, Benutzer in Ihrer Organisation ein anderes Konto verwenden, um zu authentifizieren. Auf diese Weise können Sie schränken Sie die Berechtigungen der Anwendung erteilten und Reduzieren von Risiken der bösartige Aktivitäten für den Fall, dass Sie den Code Ihrer Anwendung zu einem SQL-Injektionsangriff gefährdet ist. 

Zum Erstellen eines SQL Server authentifizierter Benutzers Verbinden mit der **master** -Datenbank auf dem Server mit Ihrem Server-Administrator-Anmeldung, und erstellen Sie eine neue Server-Anmeldung.  Darüber hinaus ist es eine gute Idee, den einen Benutzer in der master-Datenbank für Benutzer Azure SQL-Data Warehouse erstellen. Erstellen eines Benutzers in das Master-Shape kann ein Benutzer mithilfe von Tools wie SSMS ohne Angabe eines Datenbanknamens anmelden.  Außerdem können sie Objekt-Explorer verwenden, um alle Datenbanken auf einem SQLServer anzuzeigen.

```sql
-- Connect to master database and create a login
CREATE LOGIN ApplicationLogin WITH PASSWORD = 'Str0ng_password';
CREATE USER ApplicationUser FOR LOGIN ApplicationLogin;
```

Klicken Sie dann zu Ihrer **SQL Data Warehouse-Datenbank** mit der Server-Administrator-Anmeldung verbinden und Erstellen eines Datenbankbenutzers basierend auf den Server-Benutzernamen, die, den Sie soeben erstellt haben.

```sql
-- Connect to SQL DW database and create a database user
CREATE USER ApplicationUser FOR LOGIN ApplicationLogin;
```

Wenn ein Benutzer zusätzliche JOIN-Operationen wie Benutzernamen oder erstellen neue Datenbanken ausführen, müssen sie auch zugeordnet werden die `Loginmanager` und `dbmanager` Rollen in der master-Datenbank. Weitere Informationen zu diesen zusätzliche Rollen und Authentifizierung bei einer SQL-Datenbank finden Sie unter [Verwalten von Datenbanken und Benutzernamen in SQL Azure-Datenbank][].  Weitere Informationen zu für die Data Warehouse SQL Azure AD finden Sie unter [Herstellen einer Verbindung mit SQL Data Warehouse durch Verwenden von Azure Active Directory-Authentifizierung][].


## <a name="authorization"></a>Autorisierung

Autorisierung bezieht sich auf mögliche in einer Azure SQL-Data Warehouse-Datenbank Aktionen, und dies wird gesteuert, indem Sie Ihr Benutzerkonto Rollenmitgliedschaften und Berechtigungen. Als bewährte Methode sollten Sie die Benutzer die minimal erforderlichen Berechtigungen erteilen. Azure SQL-Data Warehouse erleichtert dies mit Rollen in T-SQL verwalten:

```sql
EXEC sp_addrolemember 'db_datareader', 'ApplicationUser'; -- allows ApplicationUser to read data
EXEC sp_addrolemember 'db_datawriter', 'ApplicationUser'; -- allows ApplicationUser to write data
```

Server-Admin-Konto, mit dem Sie verbinden, ist ein Mitglied von Db_owner, die Berechtigungen zum nichts innerhalb der Datenbank verfügt. Speichern Sie dieses Konto für die Bereitstellung von Schema Upgrades und andere Management Vorgänge an. Verwenden Sie das Konto "ApplicationUser" mit eingeschränkter Berechtigungen Verbindung aus der Anwendung an die Datenbank mit den minimalen Berechtigungen, die von Ihrer Anwendung benötigt.

Es gibt Methoden zum weiter einschränken, was ein Benutzer mit Azure SQL-Datenbank ausführen kann:

- Fein abgestimmte [Berechtigungen][] können Sie steuern, welche Vorgänge, die Sie ausführen können, auf die einzelnen Spalten, Tabellen, Sichten, Prozeduren und anderen Objekten in der Datenbank. Verwenden Sie genau abgestimmte Berechtigungen, um die größte Kontrolle haben und über die minimalen erforderlichen Berechtigungen erteilen. Detaillierte Rechte System ist etwas kompliziert und erfordert einige Fallstudie effektiv verwenden.
- [Datenbankrollen][] als Db_datareader und Db_datawriter kann verwendet werden, leistungsfähigeren Anwendungsbenutzerkonten oder weniger leistungsfähige Management-Konten erstellen. Die integrierten festen Datenbankrollen bieten eine einfache Möglichkeit, um Berechtigungen zu gewähren, aber Sie können dazu führen, Erteilen von Berechtigungen mehr, als erforderlich sind.
- [Gespeicherte Prozeduren][] können verwendet werden, die Aktionen einschränken, die in der Datenbank geöffnet werden kann.

Verwalten von Datenbanken und logische Server über das klassische Azure-Portal oder mithilfe der Azure Ressourcenmanager-API wird von der Portalseite Benutzerkonto rollenzuweisungen gesteuert. Weitere Informationen zu diesem Thema finden Sie unter [rollenbasierte Access Control Azure-Portal][].

## <a name="encryption"></a>Verschlüsselung

Azure SQL Data Warehouse Transparent Daten Verschlüsselung (TDE) trägt zum Schutz des Risiko von bösartige Aktivitäten nach Durchführung in Echtzeit und Verschlüsselung von Ihrer Daten statisch sind.  Wenn Sie die Datenbank verschlüsseln, sind zugeordneten Sicherungskopien und Transaktionsprotokolldateien verschlüsselt ohne Änderungen an Ihrer Anwendung. TDE verschlüsselt die Speicherung eine gesamte Datenbank mithilfe eines symmetrischen Schlüssels des Verschlüsselungsschlüssels Datenbank bezeichnet. In SQL-Datenbank ist des Verschlüsselungsschlüssels für die Datenbank durch eine integrierte Serverzertifikat geschützt. Das integrierten Serverzertifikat ist eindeutig für jeden SQL-Datenbankserver. Microsoft dreht diese Zertifikate automatisch mindestens alle 90 Tage. Der von SQL Data Warehouse verwendete Verschlüsselungsalgorithmus ist AES-256. Eine allgemeine Beschreibung TDE finden Sie unter [Transparenz Daten der Verschlüsselung][].

Sie können Ihre Datenbank mit dem [Portal Azure] verschlüsseln[ Encryption with Portal] oder [T-SQL-][Encryption with TSQL].

## <a name="next-steps"></a>Nächste Schritte

Details und Beispiele für das Herstellen einer Verbindung mit Ihrem SQL Data Warehouse mit unterschiedlichen Protokollen finden Sie unter [Verbinden mit SQL Data Warehouse][].

<!--Image references-->

<!--Article references-->
[Verbinden Sie mit SQL Datawarehouse]: ./sql-data-warehouse-connect-overview.md
[Encryption with Portal]: ./sql-data-warehouse-encryption-tde.md
[Encryption with TSQL]: ./sql-data-warehouse-encryption-tde-tsql.md
[Herstellen einer Verbindung mit SQL Datawarehouse mithilfe der Azure-Active Directory-Authentifizierung]: ./sql-data-warehouse-authentication.md

<!--MSDN references-->
[Azure SQL-Datenbank-firewall]: https://msdn.microsoft.com/library/ee621782.aspx
[sp_set_firewall_rule]: https://msdn.microsoft.com/library/dn270017.aspx
[sp_set_database_firewall_rule]: https://msdn.microsoft.com/library/dn270010.aspx
[Datenbankrollen]: https://msdn.microsoft.com/library/ms189121.aspx
[Verwalten von Datenbanken und Anmeldedaten in Azure SQL-Datenbank]: https://msdn.microsoft.com/library/ee336235.aspx
[Berechtigungen]: https://msdn.microsoft.com/library/ms191291.aspx
[Gespeicherten Prozeduren]: https://msdn.microsoft.com/library/ms190782.aspx
[Transparent Data-Verschlüsselung]: https://msdn.microsoft.com/library/bb934049.aspx
[Azure portal]: https://portal.azure.com/

<!--Other Web references-->
[Rollenbasierte Access Steuerelement Azure-Portal]: https://azure.microsoft.com/documentation/articles/role-based-access-control-configure

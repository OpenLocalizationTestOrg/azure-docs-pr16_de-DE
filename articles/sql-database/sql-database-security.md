<properties
   pageTitle="Übersicht über die Sicherheit von SQL-Datenbank"
   description="Erfahren Sie mehr über Azure SQL-Datenbank und SQL Server-Sicherheit, einschließlich der Unterschiede zwischen der Cloud und SQL Server lokal wenn Authentifizierung, Autorisierung, Verbindung Sicherheit, Verschlüsselung und Compliance vertrauen."
   services="sql-database"
   documentationCenter=""
   authors="tmullaney"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management"
   ms.date="06/09/2016"
   ms.author="thmullan;jackr"/>


# <a name="securing-your-sql-database"></a>Sichern Ihrer SQL­Datenbank

## <a name="overview"></a>(Übersicht)

In diesem Artikel schrittweise erläutert die Grundlagen zum Schutz der Datenebene einer Anwendung Azure SQL-Datenbank. In diesem Artikel erhalten Sie die Schritte mit Ressourcen zum Einschränken des Zugriffs, Schützen von Daten, insbesondere Überwachen der Aktivitäten in einer Datenbank in das [Erste Schritte mit SQL-Datenbank-Lernprogramm](sql-database-get-started.md)erstellt haben. Eine vollständige Übersicht Sicherheitsfeatures auf alle Arten von SQL zur Verfügung finden Sie im [Sicherheitscenter für SQL Server-Datenbank-Engine und Azure SQL-Datenbank](https://msdn.microsoft.com/library/bb510589). Weitere Informationen sind auch in der [Sicherheit und Azure SQL-Datenbank technisches Whitepaper](https://download.microsoft.com/download/A/C/3/AC305059-2B3F-4B08-9952-34CDCA8115A9/Security_and_Azure_SQL_Database_White_paper.pdf) (PDF) verfügbar.

## <a name="connection-security"></a>Verbindung-Sicherheit

Verbindung Sicherheit bezieht sich auf wie Sie einschränken und sichere Verbindungen mit Ihrer Datenbank über Firewall-Regeln und Verbindung Verschlüsselung.

Firewall-Regeln werden von den Server und die Datenbank verwendet, versucht, eine Verbindung von IP-Adressen ablehnen, die nicht explizit weißen Liste hinzugefügt wurden. Um eine Anwendung oder Client-Rechner öffentliche IP-Adresse Herstellen einer Verbindung mit einer neuen Datenbank versuchen können, müssen Sie zuerst eine Ebene Server Firewall-Regel, die mithilfe der klassischen Azure-Portal, REST-API oder PowerShell erstellen. Als bewährte Methode sollten Sie die IP-Adressbereiche Ihre Serverfirewall so weit wie möglich passieren einschränken. Weitere Informationen finden Sie unter [Azure SQL-Datenbank-Firewall](https://msdn.microsoft.com/library/ee621782).

Alle Verbindungen mit Azure SQL-Datenbank Verschlüsselung (SSL/TLS) bei allen Zeiten während Daten "im Übergang" an und von der Datenbank ist. In der Verbindungszeichenfolge Ihrer Anwendung, müssen Sie Parameter, um die Verbindung verschlüsseln angeben und *nicht* als vertrauenswürdig dem Serverzertifikat (Dies ist für Sie erledigt, wenn Sie die Verbindungszeichenfolge aus der klassischen Azure-Portal kopieren), andernfalls wird die Verbindung wird nicht überprüfen die Identität des Servers "Man-in-the-Mitte" Angriffen ausgesetzt werden. ADO.NET-Treiber beispielsweise diese Parameter für eine Verbindungszeichenfolge werden **Verschlüsseln = WAHR** und **TrustServerCertificate = falsch**. Weitere Informationen finden Sie unter [Azure SQL-Datenbank Verbindung Verschlüsselung und Überprüfung des Zertifikats](https://msdn.microsoft.com/library/azure/ff394108#encryption).


## <a name="authentication"></a>Authentifizierung

Authentifizierung bezieht sich auf wie Ihre Identität beweisen, bei der Verbindung mit der Datenbank. SQL-Datenbank unterstützt zwei Arten der Authentifizierung:

 - **SQL-Authentifizierung**, die einen Benutzernamen und ein Kennwort verwendet.
 - **Azure-Active Directory-Authentifizierung**, wodurch von Azure Active Directory verwaltet Identitäten verwendet wird unterstützt für verwaltete und integrierte Domänen

Wenn Sie den logischen Server für die Datenbank erstellt haben, können Sie eine Anmeldung "Serveradministrator" mit einen Benutzernamen und ein Kennwort angegeben. Verwenden diese Anmeldeinformationen, können Sie mit einer Datenbank auf diesem Server als den Datenbankbesitzer oder "Dbo." authentifizieren Wenn Sie Azure-Active Directory-Authentifizierung verwenden möchten, müssen Sie einen anderen Serveradministrator bezeichnet "" Azure AD-Administration"," Was Azure Active Directory-Benutzer und Gruppen verwalten zulässig ist erstellen. Dieser Administrator kann auch alle Vorgänge ausführen, die eine normale Serveradministrator kann. Finden Sie unter [Herstellen einer Verbindung mit SQL Datenbank durch Verwenden von Azure Active Directory-Authentifizierung](sql-database-aad-authentication.md) für eine exemplarische Vorgehensweise zum Erstellen einer Azure AD-Administrator Azure-Active Directory-Authentifizierung aktivieren.

Als bewährte Methode sollte die Anwendung ein anderes Konto zum Authentifizieren auf diese Weise können die Berechtigungen für die Anwendung beschränken und Risiken der bösartige Aktivitäten für den Fall, dass Sie den Code Ihrer Anwendung zu einem SQL-Injektionsangriff gefährdet ist – verwenden. Es wird empfohlen, eine [Datenbankbenutzer enthalten](https://msdn.microsoft.com/library/ff929188), wodurch Ihre app direkt auf eine einzelne Datenbank authentifizieren erstellen. Sie können einen eigenständigen Datenbankbenutzer erstellen, der SQL-Authentifizierung verwendet wird, indem Sie den folgenden, während der Verbindung zu Ihrer Benutzerdatenbank sich mit dem Server Administrator Benutzernamen T-SQL-Befehl ausführen:

```
CREATE USER ApplicationUser WITH PASSWORD = 'strong_password'; -- SQL Authentication
```

Wenn Sie ein Administrator Azure AD-erstellt haben, können Sie einen eigenständigen Datenbankbenutzer erstellen, der Azure-Active Directory-Authentifizierung verwendet wird, indem Sie den folgenden, während der Verbindung zu Ihrer Benutzerdatenbank als Administrator Azure AD-T-SQL-Befehl ausführen:

```
CREATE USER [Azure_AD_principal_name | Azure_AD_group_display_name] FROM EXTERNAL PROVIDER; -- Azure Active Directory Authentication
```

In beiden Fällen sollten Verbindungszeichenfolge Ihrer Anwendung den Server Admin-Benutzernamen, die Verbindung zu der Datenbank herstellen, anstatt diese Benutzeranmeldeinformationen angeben.

Weitere Informationen zu einer SQL-Datenbank authentifizieren finden Sie unter [Verwalten von Datenbanken und Anmeldedaten in Azure SQL-Datenbank](sql-database-manage-logins.md).


## <a name="authorization"></a>Autorisierung
Autorisierung bezieht sich auf mögliche in einer SQL Azure-Datenbank Aktionen, und dies wird gesteuert, indem Sie Ihr Benutzerkonto Rollenmitgliedschaften und Berechtigungen. Als bewährte Methode sollten Sie die Benutzer die minimal erforderlichen Berechtigungen erteilen. Azure SQL-Datenbank erleichtert dies mit Rollen in T-SQL verwalten:

```
ALTER ROLE db_datareader ADD MEMBER ApplicationUser; -- allows ApplicationUser to read data
ALTER ROLE db_datawriter ADD MEMBER ApplicationUser; -- allows ApplicationUser to write data
```

Server-Admin-Konto, mit dem Sie verbinden, ist ein Mitglied von Db_owner, die Berechtigungen zum nichts innerhalb der Datenbank verfügt. Speichern Sie dieses Konto für die Bereitstellung von Schema Upgrades und andere Management Vorgänge an. Verwenden Sie das Konto "ApplicationUser" mit eingeschränkter Berechtigungen Verbindung aus der Anwendung an die Datenbank mit den minimalen Berechtigungen, die von Ihrer Anwendung benötigt.

Es gibt Methoden zum weiter einschränken, was ein Benutzer mit Azure SQL-Datenbank ausführen kann:

* [Datenbankrollen](https://msdn.microsoft.com/library/ms189121) als Db_datareader und Db_datawriter kann verwendet werden, leistungsfähigeren Anwendungsbenutzerkonten oder weniger leistungsfähige Management-Konten erstellen.
* Fein abgestimmte [Berechtigungen](https://msdn.microsoft.com/library/ms191291) können Sie steuern, welche Vorgänge, die Sie ausführen können, auf die einzelnen Spalten, Tabellen, Sichten, Prozeduren und anderen Objekten in der Datenbank.
* [Identitätswechsel](https://msdn.microsoft.com/library/vstudio/bb669087) und das [Modul Signieren](https://msdn.microsoft.com/library/bb669102) können verwendet werden, auf sichere Weise Berechtigungen vorübergehend zu erhöhen.
* [Sicherheit auf Benutzerebene Zeile](https://msdn.microsoft.com/library/dn765131) verwendet werden können Grenzwert für die einen Benutzer Zeilen zugreifen kann.
* [Daten Masking](sql-database-dynamic-data-masking-get-started.md) kann verwendet werden, zum Anzeigen von sensiblen Daten zu beschränken.
* [Gespeicherte Prozeduren](https://msdn.microsoft.com/library/ms190782) können verwendet werden, die Aktionen einschränken, die in der Datenbank geöffnet werden kann.

Verwalten von Datenbanken und logische Server über das klassische Azure-Portal oder mithilfe der Azure Ressourcenmanager-API wird von der Portalseite Benutzerkonto rollenzuweisungen gesteuert. Weitere Informationen zu diesem Thema finden Sie unter [rollenbasierte Access Control Azure-Portal](../active-directory./role-based-access-control-configure.md).


## <a name="encryption"></a>Verschlüsselung

Azure SQL-Datenbank können Schützen von Daten durch Verschlüsselung Ihrer Daten aus, wenn sie "at Rest" ist, oder im Datenbankdateien und Sicherung mit [Transparenten Daten Verschlüsselung](http://go.microsoft.com/fwlink/?LinkId=526242)gespeichert. Zum Verschlüsseln der Datenbank, als Datenbankbesitzer verbinden und ausführen:

```
ALTER DATABASE [AdventureWorks] SET ENCRYPTION ON;
```

Andere Methoden zum Verschlüsseln Ihre Daten vertraulichen Daten berücksichtigen:

* [Auf Zellenebene Verschlüsselung](https://msdn.microsoft.com/library/ms179331.aspx) so verschlüsseln Sie bestimmte Spalten oder sogar Zellen mit Daten mit anderen Schlüssel.
* Wenn Sie einen Schlüssel oder die zentrale Verwaltung Ihrer Verschlüsselung Key Hierarchie benötigen, sollten erwägen Sie, [Azure Schlüssel Tresor mit SQL Server auf eine Azure-virtuellen Computer](http://blogs.technet.com/b/kv/archive/2015/01/12/using-the-key-vault-for-sql-server-encryption.aspx)zu verwenden.
* [Immer verschlüsselt.](https://msdn.microsoft.com/library/mt163865.aspx) (in der Vorschau) macht Verschlüsselung für Applikationen transparent und ermöglicht es Clients, vertrauliche Daten in Clientanwendungen verschlüsseln, ohne die Freigabe der Schlüssel für die Verschlüsselung mit SQL-Datenbank.

## <a name="auditing"></a>Überwachung

Überwachung und Nachverfolgen Datenbankereignisse hilft Ihnen Einhaltung verwalten und verdächtigen Aktivitäten zu identifizieren. SQL-Datenbank Überwachung ermöglicht es, dass Sie zum Aufzeichnen von Ereignissen in Ihrer Datenbank zu einer Audit in Ihr Konto Azure-Speicher beim Anmelden. SQL-Datenbank Überwachung integriert auch mit Microsoft Power BI um Drilldown-Berichte und Analysen zu vereinfachen. Weitere Informationen finden Sie unter [Erste Schritte mit SQL-Datenbank Überwachung](sql-database-auditing-get-started.md).

SQL-Datenbank Erkennung bietet eine zusätzliche Sicherheitsebene auf Überwachung. Es ermöglicht es Ihnen, Risiken zu reagieren, sobald sie durch die Bereitstellung von Sicherheitshinweisen auf außergewöhnlichen Aktivitäten auftreten. Weitere Informationen finden Sie unter [Erste Schritte mit SQL Datenbank Erkennung](sql-database-threat-detection-get-started.md).  

## <a name="compliance"></a>Compliance

Zusätzlich zu den oben genannten Features und Funktionen, die eine Anwendung verschiedene Sicherheit Compliance Azure SQL-Datenbank auch Bedürfnisse helfen können reguläre Überwachungen beteiligt ist und ist mit einer Reihe von Compliance-Standards zertifiziert wurden. Weitere Informationen finden Sie im [Microsoft Azure-Trust Center](https://azure.microsoft.com/support/trust-center/), wo Sie die aktuelle Liste der [SQL-Datenbank Compliance-Zertifizierung](https://azure.microsoft.com/support/trust-center/services/)vorfinden.

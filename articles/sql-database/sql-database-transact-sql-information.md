<properties
   pageTitle="Nicht unterstützt werden in SQL Azure-Datenbank T-SQL | Microsoft Azure"
   description="Transact-SQL-Anweisungen, die in Azure SQL-Datenbank weniger als vollständig unterstützt werden"
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
   ms.date="08/30/2016"
   ms.author="rick.byham@microsoft.com"/>

# <a name="azure-sql-database-transact-sql-differences"></a>Azure SQL-Datenbank Transact-SQL-Unterschiede


Die meisten Transact-SQL-Features, denen Applikationen abhängig sind, werden in Microsoft SQL Server und Azure SQL-Datenbank unterstützt. Es folgt eine Teilliste der unterstützten Funktionen für Applikationen:

- Datentypen.
- Operatoren.
- Zeichenfolge, arithmetischen, logischen und Cursor Funktionen.

Azure SQL-Datenbank dient jedoch isolieren Features in Abhängigkeit von der **master** -Datenbank. Daher viele Server Ebene Aktivitäten eignen sich nicht für SQL-Datenbank und werden nicht unterstützt. Features, die in SQL Server veraltet sind, werden in SQL-Datenbank im Allgemeinen nicht unterstützt.

> [AZURE.NOTE]
> In diesem Thema werden die Features, die mit SQL-Datenbank, die beim Upgrade auf die aktuelle Version zur Verfügung stehen behandelt. SQL-Datenbank V12. Weitere Informationen zu V12, finden Sie unter [SQL-Datenbank V12-Was ist neu](sql-database-v12-whats-new.md).

In den folgenden Abschnitten finden Sie Features, die sich teilweise unterstützt werden, und die Funktionen, die nicht vollständig unterstützt werden.


## <a name="features-partially-supported-in-sql-database-v12"></a>Features, die in der SQL-Datenbank V12 teilweise unterstützt

SQL-Datenbank V12 unterstützt einiger, jedoch nicht alle Argumente, die vorhanden sind in der entsprechenden SQL Server 2016 Transact-SQL-Anweisung. CREATE PROCEDURE-Anweisung beträgt beispielsweise zur Verfügung, jedoch nicht alle Optionen des Verfahrens erstellen verfügbar sind. Finden Sie ausführliche Informationen zu den unterstützten Bereichen jeder Anweisung verknüpfte Syntax Themen.

- Datenbanken: [Erstellen](https://msdn.microsoft.com/library/dn268335.aspx )/[ALTER](https://msdn.microsoft.com/library/ms174269.aspx)
- DMVs stehen im Allgemeinen für Features, die verfügbar sind.
- Funktionen: [Erstellen](https://msdn.microsoft.com/library/ms186755.aspx)/[ALTER (Funktion)](https://msdn.microsoft.com/library/ms186967.aspx)
- [ABBRECHEN](https://msdn.microsoft.com/library/ms173730.aspx) 
- Benutzernamen: [Erstellen](https://msdn.microsoft.com/library/ms189751.aspx)/[ALTER LOGIN](https://msdn.microsoft.com/library/ms189828.aspx)
- Gespeicherte Prozeduren: [Erstellen](https://msdn.microsoft.com/library/ms187926.aspx)/[ALTER PROCEDURE](https://msdn.microsoft.com/library/ms189762.aspx)
- Tabellen: [Erstellen](https://msdn.microsoft.com/library/dn305849.aspx)/[ALTER](https://msdn.microsoft.com/library/ms190273.aspx)
- Typen (Benutzerdefiniert): [Typ erstellen](https://msdn.microsoft.com/library/ms175007.aspx)
- Benutzer: [Erstellen](https://msdn.microsoft.com/library/ms173463.aspx)/[ALTER Benutzer](https://msdn.microsoft.com/library/ms176060.aspx)
- Ansichten: [Erstellen](https://msdn.microsoft.com/library/ms187956.aspx)/[Ansicht ändern](https://msdn.microsoft.com/library/ms173846.aspx)

## <a name="features-not-supported-in-sql-database"></a>In der SQL-Datenbank nicht unterstützte Features

- Sortierung der Systemobjekte
- Verbindung Zusammenhang: Endpunkt Anweisungen, ORIGINAL_DB_NAME. SQL-Datenbank unterstützt keine Windows-Authentifizierung, aber die ähnliche Azure Active Directory-Authentifizierung unterstützt. Einige Authentifizierungsarten müssen die neueste Version von SSMS. Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit SQL-Datenbank oder SQL Data Warehouse durch Verwenden von Azure Active Directory-Authentifizierung](sql-database-aad-authentication.md).
- Streichen Sie Datenbankabfragen mit drei oder vier Webpart-Namen ein. (Schreibgeschützt Cross-Datenbankabfragen werden unterstützt, mithilfe von [flexible Datenbankabfrage](sql-database-elastic-query-overview.md).)
- Verkettung Cross Besitz, VERTRAUENSWÜRDIGEN Einstellung
- Datensammlung
- Datenbank-Diagrammen
- Database Mail
- DATABASEPROPERTY (verwenden Sie stattdessen DATABASEPROPERTYEX)
- Ausführen von AS-Benutzernamen
- Verschlüsselung: extensible Key-Verwaltung
- Ereignisse: Ereignisse, Ereignis Benachrichtigungen, Abfrage Benachrichtigungen
- Features, die im Zusammenhang mit Platzieren der Datenbankdatei, des Schriftgrads und Datenbankdateien, die automatisch von Microsoft Azure verwaltet werden.
- Features, die Zusammenhang mit der hohen Verfügbarkeit der über Ihr Microsoft Azure-Konto verwaltet wird: Sichern, wiederherstellen, AlwaysOn Datenbank Spiegelung, Log Liefer-, Wiederherstellung Modi. Weitere Informationen finden Sie unter Azure SQL-Datenbank sichern und wiederherstellen.
- Features, die bei der SQL-Datenbank ausgeführt Log Reader Aufsetzen: Pushbenachrichtigungen Replikation, ändern Daten erfassen.
- Features, die in der SQL Server-Agent oder die Datenbank MSDB Aufsetzen: Aufträge, Benachrichtigungen, Operatoren, Policy-basiertes Management, Datenbank e-Mail, zentralen Verwaltung-Servern.
- FILESTREAM
- Funktionen: "fn_get_sql", "fn_virtualfilestats", fn_virtualservernodes
- Globale temporäre Tabellen
- Hardware-bezogene servereinstellungen: Speicher, Arbeitsthreads, CPU Zugehörigkeit, Kennzeichen Spur usw.. Verwenden Sie stattdessen die Service-Level.
- HAS_DBACCESS
- STATS JOB ABBRECHEN
- Verknüpfte Server, OPENQUERY, OPENROWSET, OPENDATASOURCE, Massen einfügen und vierteiliger Namen
- Master/Ziel-Servern
- .NET Framework- [CLR-Integration in SQL Server](http://msdn.microsoft.com/library/ms254963.aspx)
- Ressourcenkontrolle
- Semantische suchen
- Serveranmeldeinformationen. Datenbank verwenden ausgelegte Anmeldeinformationen stattdessen aus.
- Server-Ebene Elemente: Server Rollen, IS_SRVROLEMEMBER, sys.login_token. Berechtigungen der Websitesammlungsebene Server sind nicht verfügbar, obwohl einige durch Berechtigungen der Stufe Datenbank ersetzt werden. Einige Server Ebene DMVs sind nicht verfügbar, obwohl einige durch Datenbank Ebene DMVs ersetzt werden.
- Ohne Server Express: Localdb, Benutzerinstanzen
- Dienst Bank
- REMOTE_PROC_TRANSACTIONS FESTLEGEN
- WAR(EN)
- sp_addmessage
- Sp_configure Optionen und neu konfigurieren. Einige Optionen stehen [ALTER Datenbank ausgelegte Konfiguration](https://msdn.microsoft.com/library/mt629158.aspx)verwenden.
- sp_helpuser
- sp_migrate_user_to_contained
- SQL Server-Überwachungsprotokollen. Verwenden Sie die SQL-Datenbank Überwachung stattdessen.
- SQL Server Profiler
- SQL Server-Spur
- Kennzeichen zu verfolgen. Einige Elemente der Spur Kennzeichnung wurden, die Kompatibilitätsmodi verschoben.
- Transact-SQL-Debuggen
- Trigger: Server ausgelegte oder Anmeldungstrigger
- USE-Anweisung: um den Datenbankkontext zu einer anderen Datenbank zu ändern, müssen Sie eine neue Verbindung mit der neuen Datenbank vornehmen.


## <a name="full-transact-sql-reference"></a>Vollständige Transact-SQL-Referenz

Weitere Informationen zu Transact-SQL-Grammatik, Verwendung und Beispielen finden Sie unter [Transact-SQL-Referenz (Datenbankmodul)](https://msdn.microsoft.com/library/bb510741.aspx) in der SQL Server-Onlinedokumentation. 

### <a name="about-the-applies-to-tags"></a>Über die "Beziehen sich auf" tags

Die Transact-SQL-Referenz enthält Themen im Zusammenhang mit Versionen von SQL Server 2008 zu präsentieren. Unter den Titel des Themas es wird ein Symbol Balken-, wobei die vier SQL Server-Plattformen und Anwendungsmöglichkeiten angibt. Verfügbarkeit von Gruppen wurden beispielsweise in SQL Server 2012 eingeführt werden. Das Thema [AVAILABILTY Gruppe erstellen](https://msdn.microsoft.com/library/ff878399.aspx) zeigt an, dass die Anweisung gilt ** SQL Server (beginnend mit 2012). Die Anweisung gilt nicht für SQL Server 2008, SQL Server 2008 R2, SQL Azure-Datenbank, Azure SQL-Data Warehouse oder Parallel Data Warehouse.

In einigen Fällen kann allgemeine Betreff des Themas in einem Produkt verwendet werden, es gibt jedoch einige kleinere Unterschiede zwischen Produkten. Die Unterschiede sind bei Mittelpunkte im Thema Bedarf angegeben.


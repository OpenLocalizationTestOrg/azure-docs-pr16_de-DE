<properties 
    pageTitle="Ports jenseits 1433 für SQL-Datenbank | Microsoft Azure"
    description="Clientverbindungen aus ADO.NET mit Azure SQL-Datenbank V12 manchmal umgehen den Proxy und direkt mit der Datenbank interagieren. Ports als 1433 werden wichtig."
    services="sql-database"
    documentationCenter=""
    authors="MightyPen"
    manager="jhubbard"
    editor="" />


<tags 
    ms.service="sql-database" 
    ms.workload="drivers"
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/17/2016"
    ms.author="annemill"/>


# <a name="ports-beyond-1433-for-adonet-45-and-sql-database-v12"></a>Ports jenseits 1433 für ADO.NET 4.5 und SQL-Datenbank V12


In diesem Thema werden die Änderungen, die Azure SQL-Datenbank V12 für das Verbindungsverhalten von Clients bereitstellt, die ADO.NET 4.5 oder höher verwenden.


## <a name="v11-of-sql-database-port-1433"></a>V11 der SQL-Datenbank: Port 1433


Wenn Ihr Clientprogramm ADO.NET 4.5, die eine Verbindung herstellen und Abfragen mit SQL-Datenbank V11 verwendet wird, lautet die interne Sequenz wie folgt:


1. ADO.NET versucht, eine Verbindung mit SQL-Datenbank herstellen.

2. ADO.NET verwendet Port 1433 ein Moduls Middleware anrufen möchten, und die Middleware stellt eine Verbindung mit SQL-Datenbank her.

3. SQL-Datenbank sendet seine Antwort an die Middleware, die die Antwort auf ADO.NET Port 1433 weiterleitet.


**Terminologie:** Davor liegenden beschrieben darauf hin, dass die SQL-Datenbank mithilfe der *Proxy weiterleiten*ADO.NET interagiert. Wenn es keine Middleware handelt, möchten wir angenommen, dass die *direkte Routing* verwendet wurde.


## <a name="v12-of-sql-database-outside-vs-inside"></a>V12 der SQL-Datenbank: außerhalb im Vergleich mit einer innerhalb


Verbindungen mit V12 müssen wir bitten Sie, ob Ihr Clientprogramm *außerhalb* oder *innerhalb* die Begrenzungslinie Azure Cloud ausgeführt wird. Die Unterabschnitte diskutieren zwei häufige Szenarien.


#### <a name="outside-client-runs-on-your-desktop-computer"></a>*Außen:* Client wird auf dem Desktopcomputer ausgeführt.


Port 1433 wird nur, die auf dem Desktopcomputer geöffnet sein müssen, die die SQL-Datenbank-Client-Anwendung hostet.


#### <a name="inside-client-runs-on-azure"></a>*Innerhalb:* Client bei Azure ausgeführt wird


Wenn Sie Ihren Kunden in die Begrenzungslinie Azure Cloud ausgeführt wird, wird was wir eine *direkte Routing* , für die Interaktion mit den SQL-Datenbankserver Anrufen verwendet. Nachdem Sie eine Verbindung hergestellt wurde, eine weitere Interaktionen zwischen dem Client und Datenbank kein Proxy Middleware beinhalten.


Die Reihenfolge lautet wie folgt aus:


1. ADO.NET 4.5 (oder höher) eine kurze Interaktion mit der Cloud Azure initiiert und empfängt eine dynamisch identifizierten Port-Nummer.
 - Die dynamisch identifizierten Port-Nummer wird im Bereich 11000-11999 oder 14000-14999.

2. ADO.NET verbindet dann mit der SQL-Datenbankserver direkt mit keine Middleware dazwischen.

3. Abfragen direkt an die Datenbank gesendet werden, und die Ergebnisse direkt an den Client zurückgegeben werden.


Stellen Sie sicher, die den Port, die Bereiche 11000-11999 und 14000-14999 auf dem Clientcomputer Azure für 4.5 ADO.NET Client Interaktionen mit SQL-Datenbank V12 verfügbar bleiben.

- Insbesondere müssen Ports im Bereich von einem beliebigen anderen ausgehenden Popupblocker frei sein.

- Ihre Azure-virtuellen Computers steuert der **Windows Firewall mit erweiterter Sicherheit** den Port zu.
 - Die [Firewall Benutzeroberfläche](http://msdn.microsoft.com/library/cc646023.aspx) können Sie eine Regel hinzufügen, für die Sie mit der Syntax wie **11000-11999**das **TCP-** Protokoll zusammen mit einem Portbereich angeben.


## <a name="version-clarifications"></a>Version clarifications


In diesem Abschnitt verdeutlicht die Moniker, die auf Produktversionen verweisen. Darüber hinaus sind einige Kombinationen von Versionen zwischen Produkte aufgeführt.


#### <a name="adonet"></a>ADO.NET


- ADO.NET 4.0 unterstützt das Protokoll TDS 7.3, aber nicht 7.4.
- ADO.NET 4.5 und höher unterstützt das 7.4 TDS-Protokoll.


#### <a name="sql-database-v11-and-v12"></a>SQL-Datenbank V11 und V12


In diesem Thema werden die Client-Verbindung Unterschiede zwischen der SQL-Datenbank V11 und V12 hervorgehoben.


*Hinweis:* Transact-SQL-Anweisung `SELECT @@version;` gibt einen Wert zurück, die mit einer Zahl zurück, z. B. '11.' beginnen oder '12', und diese entsprechen unseren Version Names V11 und V12 für SQL-Datenbank.


## <a name="related-links"></a>Links zu verwandten Themen


- ADO.NET 4.6 wurde am 20 Juli 2015 veröffentlicht. Eine Ankündigung Blog aus dem Team .NET steht [hier](http://blogs.msdn.com/b/dotnet/archive/2015/07/20/announcing-net-framework-4-6.aspx).


- ADO.NET 4.5 wurde am 15 August 2012 veröffentlicht. Eine Ankündigung Blog aus dem Team .NET steht [hier](http://blogs.msdn.com/b/dotnet/archive/2012/08/15/announcing-the-release-of-net-framework-4-5-rtm-product-and-source-code.aspx).
 - Ein Blogbeitrag zu ADO.NET 4.5.1 steht [hier](http://blogs.msdn.com/b/dotnet/archive/2013/06/26/announcing-the-net-framework-4-5-1-preview.aspx).


- [Liste der TDS-Protokoll version](http://www.freetds.org/userguide/tdshistory.htm)


- [Übersicht über die Entwicklung von SQL-Datenbank](sql-database-develop-overview.md)


- [Azure SQL-Datenbank-firewall](sql-database-firewall-configure.md)


- [So: Konfigurieren der Firewall-Einstellungen auf SQL-Datenbank](sql-database-configure-firewall-settings.md)


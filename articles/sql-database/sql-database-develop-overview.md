<properties
    pageTitle="SQL-Datenbank entwickeln Übersicht | Microsoft Azure"
    description="Informationen Sie zu verfügbaren Connectivity-Bibliotheken und bewährte Methoden für Applikationen Herstellen einer Verbindung mit einer SQL-Datenbank."
    services="sql-database"
    documentationCenter=""
    authors="annemill"
    manager="jhubbard"
    editor="genemi"/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/17/2016"
    ms.author="annemill"/>

# <a name="sql-database-development-overview"></a>Übersicht über die Entwicklung von SQL-Datenbank
In diesem Artikel führt durch die grundlegende Aspekte, denen ein Entwickler beim Schreiben von Code für die Verbindung mit Azure SQL-Datenbank kennen sollten.

## <a name="language-and-platform"></a>Sprache und Plattform
Es gibt Codebeispielen für verschiedene programming Sprachen und Plattformen verfügbar. Sie können Links zu den Codebeispielen am finden: 

* Weitere Informationen: [Datenverbindungsbibliotheken für SQL-Datenbank und SQL Server](sql-database-libraries.md)

## <a name="resource-limitations"></a>Ressource Einschränkungen
Azure SQL-Datenbank verwaltet die Ressourcen einer Datenbank mit zwei verschiedenen Verfahren zur Verfügung: Ressourcen Governance und Durchsetzung von Grenzwerte.

* Weitere Informationen: [die Ressource Grenzwerte Azure SQL-Datenbank](sql-database-resource-limits.md)

## <a name="security"></a>Sicherheit
Azure SQL-Datenbank dient Ressourcen zum Einschränken des Zugriffs, Datenschutz und Überwachen der Aktivitäten in einer SQL-Datenbank.

* Weitere Informationen: [zum Sichern Ihrer SQL­Datenbank](sql-database-security.md)

## <a name="authentication"></a>Authentifizierung
* Azure SQL-Datenbank unterstützt Benutzer der SQL Server-Authentifizierung und Benutzernamen, ebenso wie [Azure-Active Directory-Authentifizierung](sql-database-aad-authentication.md) Benutzer sowohl Benutzernamen.
* Sie müssen eine bestimmte Datenbank, nicht in die *master* -Datenbank angeben.
* Sie können die **MyDatabaseName; verwenden** Sie Transact-SQL-Anweisung auf SQL-Datenbank in eine andere Datenbank wechseln nutzen.
* Weitere Informationen: [Sicherheit SQL-Datenbank: Verwalten der Datenbank Zugriff und Login Sicherheit](sql-database-manage-logins.md)

## <a name="resiliency"></a>Stabilität
Tritt auf ein vorübergehender Fehler beim Herstellen einer Verbindung mit SQL-Datenbank, sollte der Code den Anruf wiederholen.  Es wird empfohlen, dass die Logik verwenden Backoff Logik, versuchen Sie erneut, damit es nicht der SQL-Datenbank mit mehreren Clients gleichzeitig wiederholen überlastet ist.

* Fehlercode Beispiele: Codebeispielen, die veranschaulichen Logik zu wiederholen, finden Sie Beispiele für die Sprache Ihrer Wahl am: [Datenverbindungsbibliotheken für SQL-Datenbank und SQL Server](sql-database-libraries.md)
* Weitere Informationen: [Fehlermeldungen für Clientprogramme SQL-Datenbank](sql-database-develop-error-messages.md)

## <a name="managing-connections"></a>Verwalten von Verbindungen
* Überschreiben Sie in der Client-Verbindungslogik den Standard-Timeoutwert 30 Sekunden benutzerspezifisch.  Die Standardeinstellung von 15 Sekunden ist zu kurz für Verbindungen, die im Internet abhängig sind.
* Wenn Sie eine [Verbindung Ressourcenpool](http://msdn.microsoft.com/library/8xx3tyca.aspx)verwenden, müssen Sie die Verbindung der Chat schließen Sie Ihr Programm nicht aktiv verwenden ist und nicht für sie wiederverwenden vorbereiten.

## <a name="network-considerations"></a>Netzwerkaspekte
* Auf dem Computer, auf dem Ihr Clientprogramm gehostet wird, stellen Sie sicher, dass die Firewall ausgehenden TCP-Kommunikation auf Port 1433 zulässt.  Weitere Informationen: [Konfigurieren einer Firewall Azure SQL-Datenbank](sql-database-configure-firewall-settings.md)
* Wenn Ihr Clientprogramm mit SQL-Datenbank V12 verbunden ist, während Ihr Client für eine Azure-virtuellen Computern virtueller (Computer) ausgeführt wird, müssen Sie bestimmte Anschlussbereiche des virtuellen Computers öffnen. Weitere Informationen: [Ports jenseits 1433 für ADO.NET 4.5 und V12 der SQL-Datenbank](sql-database-develop-direct-route-ports-adonet-v12.md)
* Clientverbindungen mit Azure SQL-Datenbank V12 manchmal umgehen den Proxy und direkt mit der Datenbank interagieren. Ports als 1433 werden wichtig. Weitere Informationen: [Ports jenseits 1433 für ADO.NET 4.5 und V12 der SQL-Datenbank](sql-database-develop-direct-route-ports-adonet-v12.md)

## <a name="data-sharding-with-elastic-scale"></a>Daten Sharding flexible Maßstab
Flexible Skalierung vereinfacht das Skalierung ab (und in). 

* [Entwurfmustern für mehrere Mandanten SaaS Applikationen mit SQL Azure-Datenbank](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* [Daten abhängige routing](sql-database-elastic-scale-data-dependent-routing.md)
* [Erste Schritte mit SQL Azure-Datenbank flexible skalieren Vorschau](sql-database-elastic-scale-get-started.md)

## <a name="next-steps"></a>Nächste Schritte

Untersuchen Sie die [Funktionen von SQL-Datenbank](https://azure.microsoft.com/services/sql-database/).

<properties
    pageTitle="Verbinden mit SQL-Datenbank mithilfe von PHP auf Windows | Microsoft Azure"
    description="Zeigt ein Beispiel für PHP-Programm, das von einem Windows-Client stellt eine Verbindung mit Azure SQL-Datenbank her, und enthält Links zu den erforderlichen Softwarekomponenten vom Client erforderlich."
    services="sql-database"
    documentationCenter=""
    authors="meet-bhagdev"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="drivers"
    ms.tgt_pltfrm="na"
    ms.devlang="php"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="meetb"/>


# <a name="connect-to-sql-database-by-using-php-on-windows"></a>Verbinden Sie mit SQL-Datenbank mithilfe von PHP auf Windows


[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)] 


In diesem Thema wird veranschaulicht, wie Sie eine Verbindung herstellen können mit Azure SQL-Datenbank aus einer Clientanwendung geschrieben in PHP, die unter Windows ausgeführt wird.

## <a name="step-1--configure-development-environment"></a>Schritt 1: Konfigurieren von Entwicklungsumgebung

[Konfigurieren von Entwicklungsumgebung für die Entwicklung von PHP](https://msdn.microsoft.com/library/mt720663.aspx)

## <a name="step-2-create-a-sql-database"></a>Schritt 2: Erstellen einer SQL-Datenbank

Finden Sie auf der [Seite Erste Schritte](sql-database-get-started.md) zum Erstellen einer Beispieldatenbank finden.  Es ist wichtig, dass Sie den Leitfaden zum Erstellen einer **AdventureWorks-Datenbankvorlage**folgen. Die nachfolgend aufgeführten Beispiele funktionieren nur mit dem **Schema für AdventureWorks**.


## <a name="step-3-get-connection-details"></a>Schritt 3: Abrufen Sie Connection Details

[AZURE.INCLUDE [sql-database-include-connection-string-details-20-portalshots](../../includes/sql-database-include-connection-string-details-20-portalshots.md)]


## <a name="step-4-run-sample-code"></a>Schritt 4: Ausführen Beispiel-code

* [Prüfung des Konzepts der Herstellen einer Verbindung zu SQL mithilfe von PHP](https://msdn.microsoft.com/library/mt720665.aspx)
* [Herstellen einer Verbindung mit PHP SQL mit bei](https://msdn.microsoft.com/library/mt720667.aspx)


## <a name="next-steps"></a>Nächste Schritte

* Überprüfen der [SQL-Datenbank Entwicklung (Übersicht)](sql-database-develop-overview.md)
* Weitere Informationen zu den [Microsoft von PHP-Treiber für SQL Server](https://msdn.microsoft.com/library/dn865013.aspx)
* Weitere Informationen zur Installation von PHP und die Verwendung finden Sie unter [Zugreifen auf SQL Server-Datenbanken mit PHP](http://social.technet.microsoft.com/wiki/contents/articles/1258.accessing-sql-server-databases-from-php.aspx).

## <a name="additional-resources"></a>Zusätzliche Ressourcen 

* [Entwurfmustern für mehrere Mandanten SaaS Applikationen mit SQL Azure-Datenbank](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* Untersuchen Sie die [Funktionen von SQL-Datenbank](https://azure.microsoft.com/services/sql-database/)

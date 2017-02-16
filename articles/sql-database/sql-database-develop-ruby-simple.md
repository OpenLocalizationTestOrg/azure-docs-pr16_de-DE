<properties
    pageTitle="Verbinden mit SQL-Datenbank mithilfe von Ruby | Microsoft Azure"
    description="Geben Sie eine Ruby Code Stichprobe, die zum Verbinden mit Azure SQL-Datenbank ausgeführt werden kann."
    services="sql-database"
    documentationCenter=""
    authors="ajlam"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="drivers"
    ms.tgt_pltfrm="na"
    ms.devlang="ruby"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="andrela"/>


# <a name="connect-to-sql-database-by-using-ruby"></a>Verbinden Sie mit SQL-Datenbank mithilfe von Ruby 

[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)] 

In diesem Thema wird gezeigt, wie eine Verbindung herstellen und Abfragen einer Azure SQL-Datenbank mit Ruby wird. Sie können in diesem Beispiel aus Windows, Mac oder Ubuntu Linux Plattformen ausführen.

## <a name="step-1-configure-development-environment"></a>Schritt 1: Konfigurieren von Entwicklungsumgebung

[Voraussetzung für die Verwendung der TinyTDS Ruby-Treiber für SQL Server](https://msdn.microsoft.com/library/mt711041.aspx)

## <a name="step-2-create-a-sql-database"></a>Schritt 2: Erstellen einer SQL-Datenbank

Finden Sie auf der [Seite Erste Schritte](sql-database-get-started.md) zum Erstellen einer Beispieldatenbank finden.  Es ist wichtig, dass Sie den Leitfaden zum Erstellen einer **AdventureWorks-Datenbankvorlage**folgen. Die nachfolgend aufgeführten Beispiele funktionieren nur mit dem **Schema für AdventureWorks**.

## <a name="step-3-get-connection-details"></a>Schritt 3: Abrufen Sie Connection Details

[AZURE.INCLUDE [sql-database-include-connection-string-details-20-portalshots](../../includes/sql-database-include-connection-string-details-20-portalshots.md)]

## <a name="step-4-run-sample-code"></a>Schritt 4: Ausführen Beispiel-code

[Prüfung des Konzepts der Herstellen einer Verbindung mit SQL Ruby mit](http://msdn.microsoft.com/library/mt715797.aspx)

## <a name="next-steps"></a>Nächste Schritte

* Überprüfen der [SQL-Datenbank Entwicklung (Übersicht)](sql-database-develop-overview.md)
* Weitere Informationen zu den [Microsoft Ruby-Treiber für SQL Server](https://msdn.microsoft.com/library/mt691981.aspx)

## <a name="additional-resources"></a>Zusätzliche Ressourcen 

* [Entwurfmustern für mehrere Mandanten SaaS Applikationen mit SQL Azure-Datenbank](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* Untersuchen Sie die [Funktionen von SQL-Datenbank](https://azure.microsoft.com/services/sql-database/)

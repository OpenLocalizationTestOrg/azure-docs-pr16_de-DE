<properties
    pageTitle="Verbinden mit SQL-Datenbank mithilfe von Java mit JDBC auf Windows | Microsoft Azure"
    description="Zeigt ein Beispiel der Java-Code, die Sie verwenden können, um zu Azure SQL-Datenbank herstellen. Im Beispiel verwendet JDBC, und es auf einem Windows-Clientcomputer ausgeführt wird."
    services="sql-database"
    documentationCenter=""
    authors="LuisBosquez"
    manager="jhubbard"
    editor="genemi"/>


<tags
    ms.service="sql-database"
    ms.workload="drivers"
    ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="lbosq"/>


# <a name="connect-to-sql-database-by-using-java-with-jdbc-on-windows"></a>Verbinden Sie mit SQL-Datenbank mithilfe von Java mit JDBC unter Windows


[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)] 


Dieses Thema enthält ein Beispiel der Java-Code, die Sie verwenden können, um zu Azure SQL-Datenbank herstellen. Java-Beispiel basiert auf dem Java Development Kit (JDK) Version 1.8. Im Beispiel stellt eine Verbindung mit einer Azure SQL-Datenbank mithilfe des JDBC-Treibers aus.

## <a name="step-1--configure-development-environment"></a>Schritt 1: Konfigurieren von Entwicklungsumgebung

[Konfigurieren von Entwicklungsumgebung für die Entwicklung von Java](https://msdn.microsoft.com/library/mt720658.aspx)

## <a name="step-2-create-a-sql-database"></a>Schritt 2: Erstellen einer SQL-Datenbank

Finden Sie auf der [Seite Erste Schritte](sql-database-get-started.md) , wie Sie eine Datenbank erstellen können.  

## <a name="step-3-get-connection-string"></a>Schritt 3: Abrufen der Verbindungszeichenfolge

[AZURE.INCLUDE [sql-database-include-connection-string-jdbc-20-portalshots](../../includes/sql-database-include-connection-string-jdbc-20-portalshots.md)]

> [AZURE.NOTE] Sie sind JTDS JDBC-Treiber verwenden, und Sie müssen hinzufügen "Ssl = erforderlich" auf die URL der Verbindung Zeichenfolge und Sie müssen die folgende Option für die JVM festlegen "-Djsse.enableCBCProtection=false". Diese Option JVM wird eine Lösung für ein Sicherheitsrisiko deaktiviert, daher sollten Sie sicherstellen, dass Sie wissen, welche Risiken beteiligt ist, bevor Sie diese Option festlegen.

## <a name="step-4-run-sample-code"></a>Schritt 4: Ausführen Beispiel-code

* [Prüfung des Konzepts der Herstellen einer Verbindung mit SQL mit Java](https://msdn.microsoft.com/library/mt720656.aspx)

## <a name="next-steps"></a>Nächste Schritte

* Besuchen Sie die [Java-Entwicklercenter](/develop/java/).
* Überprüfen der [SQL-Datenbank Entwicklung (Übersicht)](sql-database-develop-overview.md)
* Weitere Informationen zu den [Microsoft-JDBC-Treiber für SQL Server](https://msdn.microsoft.com/library/mt484311.aspx)

## <a name="additional-resources"></a>Zusätzliche Ressourcen 

* [Entwurfmustern für mehrere Mandanten SaaS Applikationen mit SQL Azure-Datenbank](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* Untersuchen Sie die [Funktionen von SQL-Datenbank](https://azure.microsoft.com/services/sql-database/)

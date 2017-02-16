<properties
   pageTitle="SQL Azure-Datenbank Lösung schnellen Starts | Microsoft Azure"
   description="Erfahren Sie mehr über Lösungen für SQL Azure-Datenbank"
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="sqldb-quickstart"
   ms.date="09/06/2016"
   ms.author="carlrab"/>

# <a name="explore-azure-sql-database-solution-quick-starts"></a>Untersuchen der SQL Azure-Datenbank Lösung schnellen Starts

Dieser Artikel enthält eine Übersicht über die Azure SQL-Datenbank Lösung Symbolleiste wird gestartet. Diese Symbolleiste startet im Repository Beispiele GitHub SQL Server befinden und veranschaulichen, wie mit der SQL-Datenbank in eine vollständige Lösung basierend auf reale Szenarios. Einfache Lernprogramme, in denen die Verwendung einer bestimmten SQL-Datenbank-Funktion veranschaulicht, finden Sie unter [Durchsuchen von Azure SQL-Datenbank-Lernprogramme](sql-database-explore-tutorials.md).

## <a name="try-the-wingtiptickets-demo-and-hands-on-lab"></a>Versuchen Sie, die WingTipTickets Demo und Praktikum

Führen Sie die Demo [Azure SQL-Datenbank WingTipTickets](https://github.com/microsoft/wingtiptickets) und Praktikum vor eine Stichprobe suchbasierte Azure-Anwendung, mit der Konzertkarten verkaufen, und Azure SQL-Datenbank.


## <a name="collect-and-monitor-resource-usage-data-across-multiple-pools"></a>Sammeln Sie und überwachen Sie Ressource: Einsatz Daten über mehrere Pools zu.

[Lösung Schnellstart: Flexible Ressourcenpool werden mithilfe der PowerShell](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools) bietet eine Lösung für sammeln und Überwachen von SQL-Datenbank Ressource: Einsatz über mehrere Pools in einem Abonnement. Wenn Sie ein Abonnement eine große Anzahl von Datenbanken haben, ist es schwierig zu jeder flexible Pool separat überwachen.

Um dieses Problem zu beheben, können Sie die SQL-Datenbank-PowerShell-Cmdlets und T-SQL-Abfragen zum Sammeln von Ressourcendaten für die Verwendung von mehreren Pools und deren Datenbanken kombinieren. Auf diese Weise können Sie überwachen und Ressource: Einsatz effizienter zu analysieren.

Dieser Schnellstart, bietet eine Reihe von PowerShell-Skripts und T-SQL-Abfragen sowie Dokumentation auf was bedeutet, dass die Lösung, und wie es implementiert wird.

## <a name="get-started-with-elastic-database-in-an-saas-scenario"></a>Erste Schritte mit flexible Datenbank in einem SaaS-Szenario

 [Lösung Schnellstart: Flexible Ressourcenpool benutzerdefiniertes Dashboard SaaS](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools-custom-dashboard) bietet eine Lösung für ein Software-als-a-Lösung (SaaS) Szenario, das das Feature flexible Datenbank der SQL-Datenbank dafür, dass eine kostengünstiger und skalierbare Datenbank Back-End-Anwendung SaaS nutzt.

In dieser Lösung führt Sie durch die Implementierung von einer Web app. Diese Web app können Sie die Last zu visualisieren, die eine flexible Datenbank von einem Laden-Generator erstellt wird, die ein benutzerdefiniertes Dashboard verwendet, das im Portal Azure ergänzenden Informationen.

Dieser Schnellstart bietet einen Generator laden und Überwachung Online zusammen mit der Dokumentation über die Funktionsweise der app und zur gemeinsamen Nutzung.

## <a name="create-an-azure-sql-database-by-using-code-first-development-and-the-entity-framework"></a>Erstellen einer SQL Azure-Datenbank mithilfe von Code First-Entwicklung und Entität Framework

Im Video und im Beispiel in der [Ersten Code in eine neue Datenbank](https://msdn.microsoft.com/data/jj193542.aspx) stellt eine Einführung in Code First Entwicklung, die auf eine neue Datenbank verweisen. Dieses Szenario richtet sich an eine Datenbank, die nicht vorhanden sind, aber die wird von Code First erstellt werden. Alternativ wird das Szenario eine leere Datenbank, in der Code First neue Tabellen hinzufügt, erstellt.

Code First können Sie Ihr Modell mithilfe von C#- oder Visual Basic-Klassen definieren. Sie können optional zusätzliche Konfiguration mithilfe von Attributen, die auf Ihre Klassen und Eigenschaften oder mithilfe einer fluent-API durchführen.

## <a name="integrate-elastic-database-tools-into-an-entity-framework-application"></a>Integrieren Sie flexible Datenbanktools in der Anwendung Entität Framework

Das [flexible Datenbank-Client-Bibliothek mit Entität Framework](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) Beispiel zeigt die Änderungen, die Sie zur Anwendung Entität Framework mit [flexible Datenbanktools](sql-database-elastic-scale-get-started.md)integriert werden soll vornehmen müssen. Der Fokus befindet sich auf [Shard zuordnen Management](sql-database-elastic-scale-shard-map-management.md) und mit der Entität Framework Code erste Ansatz [Daten-abhängige routing](sql-database-elastic-scale-data-dependent-routing.md) verfassen.

[Code First zu einer neuen Datenbank Stichproben EF](http://msdn.microsoft.com/data/jj193542.aspx) dient als Beispiel in diesem Beispiel. Der Code, der dieses Dokument begleitet zeigt Teil der Beispiele in der Visual Studio-Codebeispielen flexible Datenbank Tools festlegen.

## <a name="integrate-elastic-database-tools-with-row-level-security"></a>Integrieren Sie flexible Datenbanktools mit Sicherheit auf Benutzerebene Zeile

[Mandantenfähigen Applikationen mit flexible Datenbanktools und Sicherheit auf Benutzerebene Zeile](sql-database-elastic-tools-multi-tenant-row-level-security.md) zeigt den Änderungen, dass Sie zur Anwendung Entität Framework für die [Sicherheit auf Benutzerebene Zeile](https://msdn.microsoft.com/library/dn765131) [flexible Datenbanktools](sql-database-elastic-scale-get-started.md) Integration vornehmen müssen. In diesem Beispiel veranschaulicht, wie diese Technologien gemeinsam verwendet werden, um eine Anwendung mit einer hochgradig skalierbare Datenebene zu erstellen, die mehrere Shards mandantenfähigen hinweg unterstützt.

Dies können Sie mithilfe von ADO.NET SqlClient oder Entität Framework ausführen. In diesem Beispiel erweitert die [flexible Datenbank-Client-Bibliothek mit Entität Framework](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) durch Hinzufügen von Unterstützung für mandantenfähigen Shard Datenbanken.
Es wird eine einfache Console-Anwendung zum Erstellen von Blogs und Beiträge, mit vier Mandanten und zwei mandantenfähigen Shard Datenbanken erstellt.

## <a name="create-online-surveys-with-the-tailspin-surveys-application"></a>Erstellen Sie mit der Anwendung Tailspin Umfragen online-Umfragen

Diese [Tailspin Umfragen-Beispiel](https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/docs/running-the-app.md) ist eine mandantenfähigen Anwendung, Umfragen, aufgerufen, die Benutzer online-Umfragen erstellen kann. Im Beispiel Adressen einige Fragen dazu, wie Sie die Verwaltung von Benutzeridentitäten in eine mandantenfähigen Anwendung, einschließlich Anmeldung, Authentifizierung, Autorisierung und app Rollen.

## <a name="learn-about-the-latest-security-features-of-sql-database-with-the-contoso-clinic-demo-application"></a>Erfahren Sie mehr über die neuesten Sicherheitsfeatures der SQL-Datenbank mit der Contoso Technologieüberblick Demo-Anwendung

Dieser [Anwendung Contoso Technologieüberblick Demo](https://github.com/Microsoft/azure-sql-security-sample) zeigt die neuesten Sicherheitsfeatures der SQL-Datenbank.

## <a name="next-steps"></a>Nächste Schritte

[Durchsuchen von Azure SQL-Datenbank-Lernprogramme](sql-database-explore-tutorials.md)

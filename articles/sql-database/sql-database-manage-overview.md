<properties
    pageTitle="Übersicht: Verwaltungstools für SQL-Datenbank | Microsoft Azure"
    description="Vergleicht Tools und Optionen für die Verwaltung von Azure SQL-Datenbank"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="sstein"/>

# <a name="overview-management-tools-for-sql-database"></a>Übersicht: Verwaltungstools für SQL-Datenbank

In diesem Thema untersucht und vergleicht Tools und Optionen für die Verwaltung von SQL Azure-Datenbanken, sodass Sie das richtige Tool für den Auftrag, Ihr Unternehmen, und Sie auswählen können. Auswählen, wie viele Datenbanken des richtigen Tools hängt verwalten Sie, wie oft ein Vorgang ausgeführt wird, und die Aufgabe.

## <a name="azure-portal"></a>Azure-portal

Der [Azure-Portal](https://portal.azure.com) ist eine webbasierte Anwendung, wo Sie können erstellen, aktualisieren und Löschen von Datenbanken und logische Server und Überwachen der Aktivitäten in Datenbanken. Dieses Tool ist hervorragend, wenn Sie nur erste mit Azure Schritte sind, einige Datenbanken verwalten oder einen Vorgang schnell ausführen müssen.

Weitere Informationen zur Verwendung des Portals finden Sie unter [Verwalten der SQL-Datenbanken verwenden Azure-Portal](sql-database-manage-portal.md).

## <a name="sql-server-management-studio-and-sql-server-data-tools-in-visual-studio"></a>SQL Server Management Studio und SQL Server Data Tools in Visual Studio

SQL Server Management Studio (SSMS) und SQL Server Data Tools (SSDT) sind Clienttools, die auf Ihrem Computer für die Verwaltung und Entwicklung Ihrer Datenbank in der Cloud ausgeführt werden. Wenn Sie eine Anwendung Entwicklertools Visual Studio oder anderen integrierten Entwicklung Umgebungen (IDEs), [versuchen Sie es mit SSDT in Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx)vertraut sind. Viele Datenbankadministratoren kennen SSMS, die mit SQL Azure-Datenbanken verwendet werden können. [Die neueste Version von SSMS herunterladen](https://msdn.microsoft.com/library/mt238290) und verwenden Sie die neueste Version immer bei der Arbeit mit Azure SQL-Datenbank. Weitere Informationen zum Verwalten Ihrer Azure SQL-Datenbanken mit SSMS finden Sie unter [Verwalten der SQL-Datenbanken verwenden SSMS](sql-database-manage-azure-ssms.md).

> [AZURE.IMPORTANT] Verwenden Sie immer die neueste Version von [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290) und [SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx) , um mit Microsoft Azure und SQL-Datenbank-Updates synchronisiert werden.


## <a name="powershell"></a>PowerShell

Sie können PowerShell zum Verwalten von Datenbanken und flexible Datenbank Pools und Azure Ressource Bereitstellungen automatisieren. Microsoft empfiehlt dieses Tool für die Verwaltung einer großen Anzahl von Datenbanken und Automatisieren von Bereitstellung und Ressourcen Änderungen in einer Umgebung für die Herstellung.

Weitere Informationen finden Sie unter [Verwalten der SQL-Datenbank mit PowerShell](sql-database-manage-powershell.md)

## <a name="elastic-database-tools"></a>Flexible Datenbanktools
Verwenden Sie die flexible Datenbanktools wie die folgenden Aktionen ausführen 

* T-SQL-Skript anhand einer Reihe von Datenbanken mithilfe einer [flexible Auftrag](sql-database-elastic-jobs-overview.md) ausführen
* Verschieben von Datenbanken mit mehreren Mandanten Modell zu einem Datenmodell in einzelne-Mandanten mit dem [Tool Teilen und Zusammenführen](sql-database-elastic-scale-overview-split-and-merge.md)
* Verwalten von Datenbanken in ein einzelnes-Mandanten Modell oder mithilfe der [flexible skalieren Client-Bibliothek](sql-database-elastic-database-client-library.md)mit mehreren Mandanten Modell aus.
 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Azure Ressourcenmanager](https://azure.microsoft.com/features/resource-manager/)
- [Azure Automatisierung](https://azure.microsoft.com/documentation/services/automation/)
- [Azure Scheduler](https://azure.microsoft.com/documentation/services/scheduler/)
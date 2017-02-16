<properties
    pageTitle="Überwachen und Behandeln von Problemen mit der Migration von Daten (gedehnt Datenbank) | Microsoft Azure"
    description="Erfahren Sie, wie Sie den Status der Datenmigration zu überwachen."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/14/2016"
    ms.author="douglasl"/>

# <a name="monitor-and-troubleshoot-data-migration-stretch-database"></a>Überwachen Sie und Behandeln von Problemen mit der Migration von Daten (gedehnt Datenbank)

Wählen Sie zum Überwachen der Migration von Daten in die Datenbank Monitor gedehnt **Vorgänge | Dehnen | Monitor** für eine Datenbank in SQL Server Management Studio.

## <a name="check-the-status-of-data-migration-in-the-stretch-database-monitor"></a>Überprüfen Sie den Status der Datenmigration in der Datenbank-Monitor gedehnt
Wählen Sie Vorgänge **| Dehnen | Monitor** für eine Datenbank in SQL Server Management Studio gedehnt Datenbank Monitor öffnen und Migration von Daten zu überwachen.

-   Der obere Teil der Monitor zeigt allgemeine Informationen zu den beiden gedehnt\-SQL Server-Datenbank und die Remotedatenbank Azure aktiviert.

-   Im unteren Bereich des Bildschirms zeigt den Status der Datenmigration für jedes gedehnt\-Tabelle in der Datenbank aktiviert.

![Dehnen Datenbank Monitor][StretchMonitorImage1]

## <a name="a-namemigrationacheck-the-status-of-data-migration-in-a-dynamic-management-view"></a><a name="Migration"></a>Überprüfen Sie den Status der Datenmigration in einer dynamischen Management-Ansicht
Öffnen der dynamischen Management **sys.dm\_Db\_rda\_Migration\_Status** sehen, wie viele Blattnamen und Datenzeilen migriert wurden. Weitere Informationen finden Sie unter [sys.dm_db_rda_migration_status (Transact-SQL)](https://msdn.microsoft.com/library/dn935017.aspx).

## <a name="a-namefirewallatroubleshoot-data-migration"></a><a name="Firewall"></a>Problembehandlung bei der Migration von Daten

**Zeilen aus der Tabelle gedehnt aktiviert werden nicht in Azure migriert werden. Worin besteht das Problem?**

Es gibt verschiedene Probleme, die Migration beeinflussen können. Aktivieren Sie die folgenden Elemente.

-   Überprüfen Sie Netzwerkkonnektivität für den SQL Server-Computer an.

-   Überprüfen Sie, dass die Azure-Firewall Ihrer SQL Server herstellen einer Verbindung mit den remote-Endpunkt nicht blockiert.

-   Überprüfen, welche Ansicht dynamische Management **sys.dm\_Db\_rda\_Migration\_Status** für den Status des letzten Blattes. Wenn ein Fehler aufgetreten ist, überprüfen Sie den Fehler\_zu nummerieren, Fehler\_Zustand und die Fehlernummer\_schwere Werte für das Blatt ein.

    -   Weitere Informationen über die Ansicht finden Sie unter [sys.dm_db_rda_migration_status (Transact-SQL)](https://msdn.microsoft.com/library/dn935017.aspx).

    -   Weitere Informationen zu den Inhalt von einer SQL Server-Fehlermeldung finden Sie unter [sys.messages (Transact-SQL)](https://msdn.microsoft.com/library/ms187382.aspx).

**Die Azure-Firewall blockiert Verbindungen aus meinem lokalen Server.**

Möglicherweise müssen Sie eine Regel in der Azure Firewalleinstellungen des Servers Azure, wenn SQL Server für die Kommunikation mit dem Remoteserver Azure hinzufügen.

## <a name="see-also"></a>Siehe auch

[Verwalten von und Problembehandlung bei gedehnt Datenbank](sql-server-stretch-database-manage.md)

<!--Image references-->
[StretchMonitorImage1]: ./media/sql-server-stretch-database-monitor/StretchDBMonitor.png

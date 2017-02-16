<properties
    pageTitle="Wiederherstellen eine SQL Azure-Datenbank aus einer automatischen Sicherung (Azure Portal) | Microsoft Azure"
    description="Wiederherstellen einer SQL Azure-Datenbank aus einer automatischen Sicherung (Azure-Portal)."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="10/18/2016"
    ms.author="sstein"
    ms.workload="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="restore-an-azure-sql-database-from-an-automatic-backup-using-the-azure-portal"></a>Wiederherstellen einer SQL Azure-Datenbank aus einer automatischen Sicherung über das Azure-portal


> [AZURE.SELECTOR]
- [(Übersicht)](sql-database-recovery-using-backups.md#geo-restore)
- [Geo-wiederherstellen: PowerShell](sql-database-geo-restore-powershell.md)

In diesem Artikel wird gezeigt, wie die Datenbank aus einer [automatischen Sicherung](sql-database-automated-backups.md) in einem neuen Server mit [Geo-wiederherstellen](sql-database-recovery-using-backups/.md#geo-restore) wiederherstellen Azure-Portal verwenden.

## <a name="select-a-database-to-restore"></a>Wählen Sie eine Datenbank wiederherstellen

Zum Wiederherstellen einer Datenbank im Azure-Portal führen Sie die folgenden Schritte aus:

1.  Wechseln Sie zum [Azure-Portal](https://portal.azure.com)an.
2.  Wählen Sie auf der linken Seite des Bildschirms **+ neue** > **Datenbanken** > **SQL-Datenbank**:

    ![Wiederherstellen einer SQL Azure-Datenbank](./media/sql-database-geo-restore-portal/new-sql-database.png)

3.  Wählen Sie als Quelle **Sicherungsdatei** aus, und wählen Sie dann auf die Sicherung, die Sie wiederherstellen möchten. Geben Sie einen Datenbanknamen, einem Server, die Datenbank in, wiederhergestellt werden soll, und klicken Sie dann auf **Erstellen**:
  
    ![Wiederherstellen einer SQL Azure-Datenbank](./media/sql-database-geo-restore-portal/geo-restore.png)

Überwachen Sie den Status des Wiederherstellungsvorgangs durch Klicken auf das Benachrichtigungssymbol in der oberen rechten Ecke der Seite ein. 


## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht über Business-Continuity und Szenarien finden Sie unter [Übersicht über die Business continuity](sql-database-business-continuity.md)
- Weitere Informationen zu Azure SQL-Datenbank automatische Sicherungskopien finden Sie unter [SQL-Datenbank automatische Sicherungskopien](sql-database-automated-backups.md)
- Weitere Informationen zum automatische Sicherungskopien für Wiederherstellung verwenden, finden Sie unter [Wiederherstellen einer Datenbank aus den Dienst initiiert Sicherungskopien](sql-database-recovery-using-backups.md)
- Weitere Informationen zu schneller Wiederherstellungsoptionen finden Sie unter [Aktiv-Geo-Replikation](sql-database-geo-replication-overview.md)  
- Weitere Informationen zum Verwenden automatische Sicherungskopien für Archivierung, finden Sie unter [Datenbank kopieren](sql-database-copy.md)

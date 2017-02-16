<properties
    pageTitle="Behandeln von Problemen mit sichern und Wiederherstellen von mit Azure SQL-Datenbank"
    description="Informationen Sie zum Wiederherstellen einer Datenbank Cloud von Fehlern und Ausfall Sicherungskopien und Replikate in Azure SQL-Datenbank verwenden."
    services="sql-database"
    documentationCenter=""
    authors="dalechen"
    manager="felixwu"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/31/2016"
    ms.author="daleche"/>

# <a name="restore-a-database-to-a-previous-point-in-time-restore-a-deleted-database-or-recover-from-a-data-center-outage"></a>Wiederherstellen einer Datenbank zu einem vorherigen Punkt Zeitpunkt, Wiederherstellen einer gelöschten Datenbank oder Wiederherstellen aus einer Data Center einem Dienstausfall

SQL-Datenbank wird Replikate Ihrer Datenbank aus, damit Sie Ausfall und Benutzerfehler beheben können. Verfügbaren Optionen hängen davon ab, die Datenbank Dienstebene und Optionen ausgewählt haben. Finden Sie unter [Übersicht über die Business Continuity](sql-database-business-continuity.md) für Details und gibt.

## <a name="to-restore-a-database-to-a-previous-point-in-time"></a>Zum Wiederherstellen einer Datenbank zu einem vorherigen Punkt Zeitpunkt
1.  Klicken Sie im [Portal Azure](https://azure.microsoft.com/) **SQL-Datenbanken**auf.
2.  Wählen Sie die Datenbank aus der Liste aus, und klicken Sie dann auf **Wiederherstellen**.
3.  Geben Sie einen neuen Namen für die Datenbank, wählen Sie das Datum und Uhrzeit wiederherstellen aus, und klicken Sie dann auf **erstellen.**
4.  Nehmen Sie app-Anpassungen je nach Bedarf in Bezug auf die neue Datenbank vor. [Wiederherstellen eine Datenbank zu einem bestimmten Zeitpunkt](sql-database-recovery-using-backups.md#point-in-time-restore)finden Sie unter.

## <a name="to-restore-an-accidentally-deleted-database"></a>Wiederherstellen eine Datenbank versehentlich gelöschte
1.  Klicken Sie im [Portal Azure](https://azure.microsoft.com/)- **SQL Server**auf.
2.  Wählen Sie den Server, der die Datenbank aus der Liste gehostet.
3.  Führen Sie auf dem Server-Blade einen Bildlauf nach unten, und klicken Sie auf **Datenbanken gelöscht**.
4.  Wählen Sie die Datenbank wiederherstellen und dann auf **Erstellen**.
5.  Nehmen Sie app-Anpassungen je nach Bedarf in Bezug auf die neue Datenbank vor. [Wiederherstellen eine gelöschte Datenbank](sql-database-recovery-using-backups.md#deleted-database-restore)finden Sie unter.

## <a name="to-recover-from-a-regional-datacenter-outage"></a>Zum Wiederherstellen nach einem Ausfall regionale Datenzentrum
Mit Standard- und Premium-Datenbanken Wenn Sie Geo repliziert sekundäre einrichten können Sie wiederherstellen, diese sekundäre verwenden. Dies gibt Ihnen die Möglichkeit zum Wiederherstellen einer Datenbank mit einem weniger möglichen Datenverlust. Weitere Informationen finden Sie unter [Wiederherstellen eine automatische Datenbanksicherungskopien mit Azure SQL-Datenbank](sql-database-disaster-recovery.md) .

Azure bietet auch Sicherungskopien jeder Datenbank in einem anderen Bereich (Geo redundante Sicherung). Sie können eine neue Datenbank erstellen, aus diesen Sicherungskopien, die Geo-wiederherstellen bezeichnet wird, aber wahrscheinlich, dass Sie Datenverlust geboten werden, wenn Sie diese Methode alleine anzuzeigen.

**Um einer Datenbank mit Geo-wiederherstellen wiederherzustellen:**

- Im [Portal Azure](https://azure.microsoft.com/)-klicken Sie auf **neu**, klicken Sie auf **Daten und Speicher**, klicken Sie auf **SQL-Datenbank**, und wählen Sie dann die **Sicherung** als Datenbankquelle. Weitere Informationen finden Sie unter [Wiederherstellen eine SQL Azure-Datenbank nach einem Ausfall](sql-database-disaster-recovery.md) .
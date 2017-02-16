<properties
    pageTitle="Verwalten von Azure SQL-Datenbank mit dem Portal Azure | Microsoft Azure"
    description="Informationen Sie zum Azure-Portal zu verwenden, um einer relationalen Datenbank in der Cloud mithilfe der Azure-Portal zu verwalten."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.date="09/19/2016"
    ms.author="sstein"/>


# <a name="managing-azure-sql-databases-using-the-azure-portal"></a>Verwalten von Azure SQL-Datenbanken, die über das Azure-portal


> [AZURE.SELECTOR]
- [Azure-portal](sql-database-manage-portal.md)
- [SSMS](sql-database-manage-azure-ssms.md)
- [PowerShell](sql-database-manage-powershell.md)

Im [Portal Azure](https://portal.azure.com/) ermöglicht das Erstellen, überwachen und Verwalten von SQL Azure-Datenbanken und -Servern. Dieser Artikel enthält eine schnelle Beschreibung und Links zu den Details der häufigeren Aufgaben.

## <a name="view-your-azure-sql-databases-servers-and-pools"></a>Zeigen Sie Ihrer SQL Azure-Datenbanken, Servern und Pools an

Klicken Sie auf **Weitere Dienste**zum Anzeigen der verfügbaren Dienste SQL-Datenbank, und geben Sie in das Suchfeld ein **SQL** :

![SQL-Datenbank](./media/sql-database-manage-portal/sql-services.png)


## <a name="how-do-i-create-or-view-azure-sql-databases"></a>Wie kann ich erstellen oder anzeigen SQL Azure-Datenbanken?

Um das Blade **SQL-Datenbanken** zu öffnen, klicken Sie auf **SQL-Datenbanken**, und klicken Sie dann klicken Sie auf die Datenbank, die, der Sie mit arbeiten möchten, oder klicken Sie auf **+ Add** zum Erstellen einer SQL-Datenbank. Weitere Informationen finden Sie unter [Erstellen einer SQL-Datenbank in Minuten mithilfe des Azure-Portals](sql-database-get-started.md).


![SQL-Datenbanken](./media/sql-database-manage-portal/sql-databases.png)


## <a name="how-do-i-create-or-view-azure-sql-servers"></a>Wie kann ich erstellen oder anzeigen SQL Azure-Server?

Zum Öffnen des **SQL Server** -Blades klicken Sie auf **SQL Server**, und klicken Sie dann klicken Sie auf dem Server, die, dem Sie mit arbeiten möchten, oder klicken Sie auf **+ Add** zum Erstellen eines SqlServer. Weitere Informationen finden Sie unter [Erstellen einer SQL-Datenbank in Minuten mithilfe des Azure-Portals](sql-database-get-started.md).

![SQL Server](./media/sql-database-manage-portal/sql-servers.png)


## <a name="how-do-i-create-or-view-sql-elastic-pools"></a>Wie kann ich erstellen oder anzeigen SQL flexible Pools?

Zum Öffnen des **SQL-flexible Pools** Blades **SQL flexible Pools**, klicken Sie auf und dann klicken Sie auf die gewünschte für die Arbeit mit Ressourcenpool, oder klicken Sie auf **+ Add** um einem Ressourcenpool zu erstellen. Details finden Sie unter [Erstellen einer Datenbank flexible Ressourcenpool mit Azure-Portal](sql-database-elastic-pool-create-portal.md).

![Flexible SQL-pools](./media/sql-database-manage-portal/elastic-pools.png)



## <a name="how-do-i-update-or-view-sql-database-settings"></a>Wie ich anzeigen SQL-Datenbank-Einstellungen oder aktualisieren?

Zum Anzeigen oder aktualisieren Ihre Datenbank-Einstellungen, klicken Sie auf die gewünschte Einstellung aus, auf dem SQL-Datenbank-Blade:


![SQL-Datenbank-Einstellungen](./media/sql-database-manage-portal/settings.png)


## <a name="how-do-i-find-a-sql-databases-fully-qualified-server-name"></a>Wie finde ich SQL-Datenbanken vollqualifizierte Servernamen?

Um den Servernamen Datenbanken anzuzeigen, klicken Sie auf dem **SQL-Datenbank** -Blade auf **Übersicht** , und notieren Sie den Servernamen:


![SQL-Datenbank-Einstellungen](./media/sql-database-manage-portal/server-name.png)


## <a name="how-do-i-manage-firewall-rules-to-control-access-to-my-sql-server-and-database"></a>Wie verwalte ich Firewall-Regeln zum Steuern des Zugriffs auf Meine SQLServer und die Datenbank?

Zum Anzeigen, erstellen oder aktualisieren die Firewall-Regeln, klicken Sie auf das Blade **SQL-Datenbank** auf **Serverfirewall festlegen** . Weitere Informationen finden Sie unter [Konfigurieren einer Azure SQL-Datenbank-Server Ebene-Firewall-Regel mithilfe des Azure-Portals](sql-database-configure-firewall-settings.md).


![Firewall-Regeln](./media/sql-database-manage-portal/sql-database-firewall.png)


## <a name="how-do-i-change-my-sql-database-service-tier-or-performance-level"></a>Wie ändere ich mein SQL-Datenbank Stufe oder Leistung Dienstalter?


Um das Dienstalter Stufe oder Leistung einer SQL-Datenbank zu aktualisieren, klicken Sie auf **Preise Ebene (Maßstab DTUs)** auf das Blade **SQL-Datenbank** . Weitere Informationen finden Sie unter [Ändern der Ebene und Leistung Dienstalter (Preisgestaltung Ebene) einer SQL-Datenbank](sql-database-scale-up.md).


![Preise Ebenen](./media/sql-database-manage-portal/pricing-tier.png)


## <a name="how-do-i-configure-auditing-and-threat-detection-for-a-sql-database"></a>Wie kann ich Überwachung konfigurieren und Verfahren zum Erstellen von Erkennung für eine SQL­Datenbank?

Klicken Sie auf **Überwachung und Gefahrenprofilen Erkennung** auf dem **SQL-Datenbank** -Blade, Überwachung und Gefahrenprofilen Erkennung für eine SQL­Datenbank um zu konfigurieren. Details finden Sie unter [Erste Schritte mit SQL-Datenbank Überwachung](sql-database-auditing-get-started.md)und [Erste Schritte mit SQL Datenbank Erkennung](sql-database-threat-detection-get-started.md).


## <a name="how-do-i-configure-dynamic-data-masking-for-a-sql-database"></a>Wie konfigurieren kann ich dynamische Daten für eine SQL­Datenbank masking?

Um dynamische Daten für eine SQL­Datenbank masking konfigurieren möchten, klicken Sie auf **Dynamic Data masking** auf das Blade **SQL-Datenbank** . Weitere Informationen finden Sie unter [Erste Schritte mit SQL Datenbank dynamische Daten Masking](sql-database-dynamic-data-masking-get-started.md).


## <a name="how-do-i-configure-transparent-data-encryption-tde-for-a-sql-database"></a>Wie konfigurieren kann ich als transparent-Verschlüsselung (TDE) für eine SQL­Datenbank?

Um transparenten Daten-Verschlüsselung einer SQL-Datenbank zu konfigurieren, klicken Sie auf **Transparent Data-Verschlüsselung** auf dem **SQL-Datenbank** -Blade. Weitere Informationen finden Sie unter [Aktivieren TDE auf einer Datenbank mit dem Portal](https://msdn.microsoft.com/library/dn948096#Anchor_1).

## <a name="how-do-i-view-or-change-the-max-size-of-a-sql-database"></a>Wie ich anzeigen oder ändern die maximale Größe von einer SQL-Datenbank?

Klicken Sie zum Anzeigen oder Ändern der Größe einer SQL-Datenbank, die **SQL-Datenbank** Blade **Datenbankgröße** auf. Aktualisieren Sie die maximale Größe einer Datenbank, indem Sie das Dienstalter Stufe oder Leistung ändern. Weitere Informationen finden Sie unter [Ändern der Ebene und Leistung Dienstalter (Preisgestaltung Ebene) einer SQL-Datenbank](sql-database-scale-up.md).

## <a name="how-do-i-monitor-and-improve-the-performance-of-a-sql-database"></a>Wie kann ich überwachen und verbessern die Leistung von einer SQL-Datenbank?

Klicken Sie zum Überwachen und verbessern Leistungsmerkmale einer SQL-Datenbank, **Performance Overview** auf das Blade **SQL-Datenbank** ein. Weitere Informationen finden Sie unter [SQL-Datenbank Leistung Einblick](sql-database-performance.md).


## <a name="how-do-i-configure-geo-replication"></a>Wie konfigurieren kann ich Geo-Replikation?

Klicken Sie zum Einrichten von Geo-Replikation für eine SQL­Datenbank auf dem **SQL-Datenbank** -Blade auf **Geo-Replikation** . Weitere Informationen finden Sie unter [Geo-Replikation für SQL Azure-Datenbank mit dem Portal Azure konfigurieren](sql-database-geo-replication-portal.md).


## <a name="how-do-i-failover-to-a-geo-replicated-sql-database"></a>Wie kann ich mit einer SQL-Datenbank Geo repliziert Failover?

Klicken Sie Failover auf einem sekundären Geo repliziert klicken Sie auf dem **SQL-Datenbank** -Blade **Geo-Replikation** auf und dann auf **Failover**. Weitere Informationen finden Sie unter [Einleiten einer geplanten oder ungeplanten Failover für SQL Azure-Datenbank mit dem Portal Azure](sql-database-geo-replication-failover-portal.md).


## <a name="how-do-i-copy-a-sql-database"></a>Wie kopiere ich eine SQL-Datenbank?

Klicken Sie zum Kopieren einer SQL-Datenbank auf dem **SQL-Datenbank** -Blade auf **Kopieren** . Weitere Informationen finden Sie unter [kopieren eine Azure SQL-Datenbank mit dem Azure-Portal](sql-database-copy-portal.md).


![SQL-Datenbank-Einstellungen](./media/sql-database-manage-portal/sql-database-copy.png)

## <a name="how-do-i-archive-an-azure-sql-database-to-a-bacpac-file"></a>Wie werden eine Azure SQL-Datenbank in eine Datei BACPAC archiviert?

Um eine BACPAC einer SQL-Datenbank zu erstellen, klicken Sie auf das Blade **SQL-Datenbank** **Exportieren** . Weitere Informationen finden Sie unter [Archiv einer Azure SQL-Datenbank in eine BACPAC-Datei mit der Azure-Portal](sql-database-export.md).


![SQL-Datenbank exportieren](./media/sql-database-manage-portal/sql-database-export.png)



## <a name="how-do-i-restore-a-sql-database-to-a-previous-point-in-time"></a>Wie kann ich wiederherstellen eine SQL-Datenbank zu einem vorherigen Punkt Zeitpunkt?

Klicken Sie zum Wiederherstellen einer SQL-Datenbank auf das Blade **SQL-Datenbank** **Wiederherstellen** . Weitere Informationen finden Sie unter [Wiederherstellen einer Azure SQL-Datenbank bis zu einem vorherigen Zeitpunkt mit dem Portal Azure](sql-database-point-in-time-restore-portal.md).


![SQL-Datenbank-Einstellungen](./media/sql-database-manage-portal/sql-database-restore.png)


## <a name="how-do-i-create-an-azure-sql-database-from-a-bacpac-file"></a>Wie erstelle ich eine SQL Azure-Datenbank aus einer Datei BACPAC?

Klicken Sie zum Erstellen einer SQL-Datenbank aus einer Datei BACPAC auf dem **SQLServer** -Blade auf **Datenbank importieren** . Weitere Informationen finden Sie unter [BACPAC Datei zum Erstellen einer SQL Azure-Datenbank importieren](sql-database-import.md).


![SqlServer](./media/sql-database-manage-portal/server-commands.png)


## <a name="how-do-i-restore-a-deleted-sql-database"></a>Wie wiederherstellen kann ich eine gelöschte SQL­Datenbank?

Klicken Sie zum Wiederherstellen einer gelöschten SQL-Datenbank auf dem **SQLServer** -Blade (der SQLServer, die die Datenbank enthalten, die nicht gelöscht wurde) auf **Datenbanken gelöscht** . Weitere Informationen finden Sie unter [Wiederherstellen einer gelöschte SQL Azure-Datenbank, die mit dem Azure-Portal](sql-database-restore-deleted-database-portal.md).

## <a name="how-do-i-delete-a-sql-database"></a>Wie kann ich eine SQL-Datenbank löschen?

Klicken Sie zum Löschen einer SQL-Datenbank auf dem **SQL-Datenbank** -Blade auf **Löschen** . 

![SQL-Datenbank-Einstellungen](./media/sql-database-manage-portal/sql-database-delete.png)



## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [SQL-Datenbank](sql-database-technical-overview.md)
- [Überwachen Sie und verwalten Sie eines Ressourcenpool flexible Datenbank mit dem Azure-portal](sql-database-elastic-pool-manage-portal.md)

<properties
    pageTitle="Upgrade auf Azure SQL-Datenbank V12 über das Azure-Portal | Microsoft Azure"
    description="Wird erläutert, wie ein Upgrade auf Azure SQL-Datenbank V12 einschließlich zum Web und Business-Datenbanken zu aktualisieren, und wie Sie einen V11 Server Migration von deren Datenbanken direkt in einer Datenbank flexible Ressourcenpool über das Azure-Portal zu aktualisieren."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="08/08/2016"
    ms.author="sstein"/>


# <a name="upgrade-to-azure-sql-database-v12-using-the-azure-portal"></a>Upgrade auf Azure SQL-Datenbank V12 über das Azure-Portal


> [AZURE.SELECTOR]
- [Azure-portal](sql-database-upgrade-server-portal.md)
- [PowerShell](sql-database-upgrade-server-powershell.md)


SQL-Datenbank V12 ist die neueste Version, damit die Aktualisierung von vorhandener Servers auf SQL-Datenbank V12 empfohlen wird.
SQL-Datenbank V12 weist viele [Vorteile gegenüber der vorherigen Version](sql-database-v12-whats-new.md) einschließlich:

- Verbesserte Kompatibilität mit SQL Server.
- Verbesserte Premium Leistung und neue Leistungsmerkmale.
- [Flexible Datenbank Pools](sql-database-elastic-pool.md).

Dieser Artikel enthält Anweisungen für die Aktualisierung von vorhandenen SQL-Datenbank V11 Server und Datenbanken auf SQL-Datenbank V12.

Während der Prozess des Upgrades auf V12 aktualisieren Sie alle Datenbanken Web und Business zu einer neuen Dienstebene, damit erfahren Sie, wie für das Upgrade Web und Business Datenbanken enthalten sind.

Migrieren zu einem [Ressourcenpool flexible Datenbank](sql-database-elastic-pool.md) kann darüber hinaus kostengünstiger als Upgrade auf einzelne Leistungsmerkmale (Preisgestaltung Ebenen) für die einzelnen Datenbanken sein. Pools vereinfachen auch Datenbank-Management, denn Sie nur die Leistung Einstellungen für den Ressourcenpool anstelle von separat verwalten die Leistungsstufe des einzelner Datenbanken verwalten müssen. Wenn Sie Datenbanken auf mehreren Servern erwägen Sie verschieben dieser Daten auf dem gleichen Server und einem Ressourcenpool Inbetriebnahme nutzen. Sie können ganz einfach [Datenbanken von V11 Servern direkt in PowerShell-PoolsVerwendung flexible Datenbank automatisch zu migrieren](sql-database-upgrade-server-powershell.md). Sie können Sie auch im Portal V11 Datenbanken in einem Ressourcenpool migrieren, aber im Portal muss bereits einen V12 Server zu einem Ressourcenpool zu erstellen. Erfahren Sie, wie dienen weiter unten in diesem Artikel, um den Pool nach dem Serverupgrade zu erstellen, wenn Sie [Datenbanken, die von einem Ressourcenpool profitieren kann,](sql-database-elastic-pool-guidance.md)verfügen.

Beachten Sie, dass Ihre Datenbanken online bleibt und innerhalb des Vorgangs Upgrade funktionieren weiterhin. Zum Zeitpunkt der tatsächlichen Übergangs zu den neuen Leistungsstufe temporäre ablegen, die Verbindung zu der Datenbank kann für einen geringen Dauer, die in der Regel ungefähr 90 Sekunden ist geschehen, aber die mehr als 5 Minuten. Wenn die Anwendung [vorübergehende Fehlerbehandlung für Verbindung Beendigungen](sql-database-connectivity-issues.md) reicht es gescannt Verbindungen unterbrochen am Ende des Upgrades aus.

Upgrade auf SQL-Datenbank V12 kann nicht rückgängig gemacht werden. Der Server kann nicht nach einem Upgrade V11 wiederhergestellt werden.

Nach dem Aktualisieren auf V12, sind [Dienst Ebene Empfehlungen](sql-database-service-tier-advisor.md) und [flexible Ressourcenpool Leistungsaspekte](sql-database-elastic-pool-guidance.md) nicht sofort verfügbar bis zu der Dienst Zeit für Ihre Auslastung auf dem neuen Server ausgewertet werden soll. V11 Server Empfehlungen Verlauf gilt nicht für V12-Servern, damit es nicht beibehalten werden.

## <a name="prepare-to-upgrade"></a>Vorbereiten der Aktualisierung

- **Alle Web und Business Datenbanken aktualisieren**: finden Sie unter Abschnitt oder finden Sie unter [Upgrade aller Web und Business-Datenbanken](sql-database-upgrade-server-portal.md#upgrade-all-web-and-business-databases) [Überwachen und Verwalten einer Datenbank flexible Ressourcenpool (PowerShell)](sql-database-elastic-pool-manage-powershell.md).
- **Überprüfen und aussetzen Geo-Replikation**: Wenn Ihre Azure SQL-Datenbank für Geo-Replikation konfiguriert ist sollten Sie dokumentieren, seine aktuelle Konfiguration und [Geo-Replikation beenden](sql-database-geo-replication-portal.md#remove-secondary-database). Nach die Aktualisierung neu zu konfigurieren Sie Ihrer Datenbank Geo-Replikation.
- **Öffnen Sie diese Ports, wenn Sie eine Azure-virtuellen Computers Clients haben**: Wenn Ihr Clientprogramm mit SQL-Datenbank V12 verbunden ist, während Ihr Client für eine Azure-virtuellen Computern virtueller (Computer) ausgeführt wird, müssen Sie Portbereiche 11000-11999 und 14000-14999 des virtuellen Computers öffnen. Weitere Informationen finden Sie unter [Ports für SQL-Datenbank V12](sql-database-develop-direct-route-ports-adonet-v12.md).



## <a name="start-the-upgrade"></a>Starten des Upgrades

1. In der [Azure-Portal](https://portal.azure.com/) Durchsuchen auf dem Server aktualisieren, indem Sie die Option **Durchsuchen**möchten > **SQL Server**, und wählen Sie den Version 2.0-Server, die Sie aktualisieren möchten.
2. Wählen Sie die **Neueste Aktualisierung der SQL-Datenbank**, und wählen Sie dann **diesen Server aktualisieren**.

      ![Upgrade-server][1]

3. Aktualisieren von einem Server auf das neueste SQL-Datenbank-Update ist permanent und nicht rückgängig gemacht werden. Um die Aktualisierung zu bestätigen, geben Sie den Namen des Servers ein, und klicken Sie auf **OK**.

## <a name="upgrade-all-web-and-business-databases"></a>Aktualisieren Sie alle Web- und Business-Datenbanken

Wenn Ihr Server alle Datenbanken Web oder Business hat, müssen Sie diese aktualisieren. Während der Prozess des Upgrades auf SQL-Datenbank V12 aktualisieren Sie alle Web- und Business-Datenbanken zu einer neuen Dienstebene.    

Wenn Sie bei einer Aktualisierung von unterstützen, empfiehlt die SQL-Datenbank eine geeignete Ebene und Leistung Dienstalter (Preisgestaltung Ebene) für jede Datenbank. Der Dienst empfiehlt die beste Ebene für Ihre vorhandene Datenbank Arbeitsbelastung durch die Analyse der zurückliegenden Verwendung für die Datenbank ausgeführt.

3. Wählen Sie in das **Upgrade dieser Server** Blade jede Datenbank aus, um zu überprüfen, und wählen Sie die empfohlene Preisgestaltung gestuft, um auf aktualisieren. Sie können immer durchsuchen die verfügbaren Preisgestaltung leisten und wählen Sie den aus, der Ihre Umgebung am besten passt.


     ![Datenbanken][2]


7. Nach dem Klicken auf die vorgeschlagenen Ebene wird das Blade **Auswählen der Preisgestaltung Ebene** angezeigt werden, wo Sie wählen Sie eine Kategorie aus und klicken Sie dann auf die Schaltfläche **auswählen** , in die Ebene ändern können. Wählen Sie eine neue Kategorie für jede Web oder Business-Datenbank aus.

    Beachten Sie ist, dass die SQL-Datenbank in eine bestimmte Ebene oder Leistung Dienstalter, nicht gesperrt ist, sodass als den Anforderungen Ihrer Datenbank ändern Sie ganz einfach zwischen den verfügbaren Service Ebenen und Leistung Ebenen ändern können. Tatsächlich Basic, Standard- und Premium SQL-Datenbanken pro Stunde berechnet werden, und Sie haben die Möglichkeit, jede Datenbank 4 Mal innerhalb von 24 Stunden nach oben oder unten skalieren.

    ![Empfehlungen][6]


Nachdem alle Datenbanken auf dem Server berechtigt sind, können Sie mit das Upgrade zu beginnen

## <a name="confirm-the-upgrade"></a>Bestätigen Sie die Aktualisierung

3. Wenn alle Datenbanken auf dem Server für die Aktualisierung berechtigt sind müssen Sie **die** Servernamen Typ stellen Sie sicher, dass Sie verwenden möchten, führen Sie das Upgrade, und klicken Sie dann auf **OK**.

    ![Vergewissern Sie sich upgrade][3]


4. Das Upgrade wird gestartet und zeigt den Fortschritt im Infobereich. Der Upgradeprozess wird gestartet. Je nach die Details zu einer bestimmten Datenbank kann Upgrade auf V12 eine Weile dauern. Dieses Zeitraums alle Datenbanken auf dem Server bleibt online aber Server und Datenbank-Management-Aktionen eingeschränkt werden.

    ![Upgrade wird ausgeführt][4]

    Zum Zeitpunkt der tatsächlichen Übergangs auf die neue Performance kann die Verbindung zu der Datenbank ablegen temporäre Ebene für einen geringen Dauer (in der Regel gemessen in Sekunden) erfolgen. Wenn eine Anwendung vorübergehende Fehlerbehandlung (Logik Wiederholungsversuche) für die Verbindung Beendigungen hat, dann es ausreichend ist, gescannt Verbindungen am Ende des Upgrades unterbrochen.

5. Nach Abschluss des Vorgangs Upgrade wird das **Neueste Update** Blade **aktiviert**angezeigt.

    ![V12 aktiviert][5]  

## <a name="move-your-databases-into-an-elastic-database-pool"></a>Verschieben Sie die Datenbanken in einer Datenbank flexible Ressourcenpool

Im [Azure-Portal](https://portal.azure.com/) auf dem Server V12 durchsuchen Sie, und klicken Sie auf **Ressourcenpool hinzufügen**.

– oder –

Wenn eine Meldung angezeigt wird, **Klicken Sie hier zum Anzeigen der Pools empfohlene flexible Datenbank für diesen Server**angezeigt wird, klicken Sie darauf, um einem Ressourcenpool einfach zu erstellen, die für die Datenbanken Ihres Servers optimiert ist. Weitere Informationen finden Sie unter [Preis und Leistung Aspekte für eine Datenbank flexible Ressourcenpool](sql-database-elastic-pool-guidance.md).

![Hinzufügen von Ressourcenpool zu einem server][7]

Führen Sie die Anweisungen im Artikel [Erstellen eines Ressourcenpool flexible Datenbank](sql-database-elastic-pool.md) zum Abschließen der Ressourcenpool zu erstellen.

## <a name="monitor-databases-after-upgrading-to-sql-database-v12"></a>Überwachen Sie nach dem Aktualisieren auf SQL-Datenbank V12 Datenbanken

>[AZURE.IMPORTANT] Aktualisieren Sie auf die neueste Version von SQL Server Management Studio (SSMS) in der neuen v12-Funktionen nutzen. [Herunterladen von SQL Server Management Studio] (https://msdn.microsoft.com/library/mt238290.aspx).

Nach der Aktualisierung aktiv Überwachen der Datenbank, um sicherzustellen, dass eine Anwendung an die gewünschte Leistung ausgeführt werden, und Optimieren Sie die Einstellungen nach Bedarf.

Zusätzlich zur Überwachung einzelner Datenbanken, können Sie flexible Datenbank Pools [überwachen, verwalten und Größe ein Ressourcenpool flexible Datenbank mit dem Portal Azure](sql-database-elastic-pool-manage-portal.md) überwachen oder mit [PowerShell](sql-database-elastic-pool-manage-powershell.md).


**Ressource Verbrauchsdaten:** Für Datenbanken Basic, Standard und Premium ist Verbrauchsdaten Ressource über die [sys.dm_ Db_ Resource_stats](http://msdn.microsoft.com/library/azure/dn800981.aspx) DMV in der Datenbank. Diese DMV bietet nahezu Echtzeit Ressource Verbrauchsinformationen nach 15 zweiten einzelnen für die vorherige Stunde des Vorgangs. Da die Materialverbrauch maximalen Prozentsatz der CPU-EA und der Protokolldateien Dimensionen wird der DTU Prozentsatz Verbrauch für ein Intervall berechnet. Hier ist eine Abfrage, um die durchschnittliche DTU Prozentsatz Verbrauchsanstieg der letzten Stunde zu berechnen:

    SELECT end_time
         , (SELECT Max(v)
             FROM (VALUES (avg_cpu_percent)
                         , (avg_data_io_percent)
                         , (avg_log_write_percent)
           ) AS value(v)) AS [avg_DTU_percent]
    FROM sys.dm_db_resource_stats
    ORDER BY end_time DESC;

Überwachung Zusatzinformationen:

- [Leitfaden zur Azure SQL-Datenbank-Leistung für einzelne Datenbanken](http://msdn.microsoft.com/library/azure/dn369873.aspx).
- [Preis und Leistung Aspekte für eine Datenbank flexible Ressourcenpool](sql-database-elastic-pool-guidance.md).
- [Verwenden von dynamischen Management Ansichten Azure SQL-Datenbank für die Überwachung](sql-database-monitoring-with-dmvs.md)




**Benachrichtigungen:** Richten Sie im Azure-Portal zu Sie benachrichtigt werden, wenn der DTU Verbrauch für eine Datenbank aktualisierten bestimmte hoher Ebene Ansätze 'Benachrichtigungen' ein. Datenbank-Benachrichtigungen können Setup im Azure-Portal für verschiedene Performance-Werte wie DTU, CPU, EA und Log sein. Navigieren Sie zu Ihrer Datenbank aus, und wählen Sie **Warnungsregeln** in den **Einstellungen** Blade.

Beispielsweise können Sie Einrichten einer e-Mail-Benachrichtigung auf "DTU Prozentsatz" überschreitet der DTU Prozentsatz Durchschnittswert 75 % über die letzten 5 Minuten. Finden Sie weitere Informationen zum Konfigurieren der Benachrichtigung [-Benachrichtigung](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) erhalten.





## <a name="next-steps"></a>Nächste Schritte

- [Überprüfen Empfehlungen pool und einem Ressourcenpool zu erstellen](sql-database-elastic-pool-create-portal.md).
- [Ändern der Dienstalter Ebene und Leistung der Datenbank](sql-database-scale-up.md).



## <a name="related-links"></a>Links zu verwandten Themen

- [Was ist neu in der SQL-Datenbank V12](sql-database-v12-whats-new.md)
- [Planen und Vorbereiten auf SQL-Datenbank V12 aktualisieren](sql-database-v12-plan-prepare-upgrade.md)


<!--Image references-->
[1]: ./media/sql-database-upgrade-server-portal/latest-sql-database-update.png
[2]: ./media/sql-database-upgrade-server-portal/upgrade-server2.png
[3]: ./media/sql-database-upgrade-server-portal/upgrade-server3.png
[4]: ./media/sql-database-upgrade-server-portal/online-during-upgrade.png
[5]: ./media/sql-database-upgrade-server-portal/enabled.png
[6]: ./media/sql-database-upgrade-server-portal/recommendations.png
[7]: ./media/sql-database-upgrade-server-portal/new-elastic-pool.png

<properties
    pageTitle="Upgrade auf Azure SQL-Datenbank V12 mithilfe der PowerShell | Microsoft Azure"
    description="Erläutert, wie Sie das upgrade auf Azure SQL-Datenbank V12 einschließlich wie Aktualisierung Web und Business-Datenbanken, und wie Sie einen Migration von deren Datenbanken direkt in einer Datenbank flexible Ressourcenpool mithilfe der PowerShell V11-Server zu aktualisieren."
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
    ms.date="09/19/2016"
    ms.author="sstein"/>

# <a name="upgrade-to-azure-sql-database-v12-using-powershell"></a>Upgrade auf Azure SQL-Datenbank V12 mithilfe der PowerShell


> [AZURE.SELECTOR]
- [Azure-portal](sql-database-upgrade-server-portal.md)
- [PowerShell](sql-database-upgrade-server-powershell.md)


SQL-Datenbank V12 ist die neueste Version, damit ein Upgrade auf SQL-Datenbank V12 empfohlen wird.
SQL-Datenbank V12 weist viele [Vorteile gegenüber der vorherigen Version](sql-database-v12-whats-new.md) einschließlich:

- Verbesserte Kompatibilität mit SQL Server.
- Verbesserte Premium Leistung und neue Leistungsmerkmale.
- [Flexible Datenbank Pools](sql-database-elastic-pool.md).

Dieser Artikel enthält Anweisungen für die Aktualisierung von vorhandenen SQL-Datenbank V11 Server und Datenbanken auf SQL-Datenbank V12.

Während der Prozess des Upgrades auf V12 aktualisieren Sie alle Datenbanken Web und Business zu einer neuen Dienstebene, damit erfahren Sie, wie für das Upgrade Web und Business Datenbanken enthalten sind.

Migrieren zu einem [Ressourcenpool flexible Datenbank](sql-database-elastic-pool.md) kann darüber hinaus kostengünstiger als Upgrade auf einzelne Leistungsmerkmale (Preisgestaltung Ebenen) für die einzelnen Datenbanken sein. Pools vereinfachen auch Datenbank-Management, denn Sie nur die Leistung Einstellungen für den Ressourcenpool anstelle von separat verwalten die Leistungsstufe des einzelner Datenbanken verwalten müssen. Wenn Sie auf mehreren Servern Datenbanken haben, erwägen Sie das Verschieben dieser Daten auf dem gleichen Server, und nutzen Sie einem Ressourcenpool Inbetriebnahme.

Führen Sie die Schritte in diesem Artikel, um die Datenbanken V11 Servern einfach direkt in Pools flexible Datenbank migrieren.

Beachten Sie, dass Ihre Datenbanken online bleibt und innerhalb des Vorgangs Upgrade funktionieren weiterhin. Zum Zeitpunkt der tatsächlichen Übergangs zu den neuen Leistungsstufe temporäre ablegen, die Verbindung zu der Datenbank kann für einen geringen Dauer, die in der Regel ungefähr 90 Sekunden ist geschehen, aber die mehr als 5 Minuten. Wenn die Anwendung [vorübergehende Fehlerbehandlung für Verbindung Beendigungen](sql-database-connectivity-issues.md) reicht es gescannt Verbindungen unterbrochen am Ende des Upgrades aus.

Upgrade auf SQL-Datenbank V12 kann nicht rückgängig gemacht werden. Der Server kann nicht nach einem Upgrade V11 wiederhergestellt werden.

Nach dem Aktualisieren auf V12, sind [Dienst Ebene Empfehlungen](sql-database-service-tier-advisor.md) und [flexible Ressourcenpool Empfehlungen](sql-database-elastic-pool-create-portal.md) nicht sofort verfügbar bis zu der Dienst Zeit für Ihre Auslastung auf dem neuen Server ausgewertet werden soll. V11 Server Empfehlungen Verlauf gilt nicht für V12-Servern, damit es nicht beibehalten werden.  

## <a name="prepare-to-upgrade"></a>Vorbereiten der Aktualisierung

- **Alle Web und Business Datenbanken aktualisieren**: Verwenden Sie das Portal oder [PowerShell-Datenbanken und aktualisieren](sql-database-upgrade-server-powershell.md).
- **Überprüfen und aussetzen Geo-Replikation**: Wenn Ihre Azure SQL-Datenbank für Geo-Replikation konfiguriert ist sollten Sie dokumentieren, seine aktuelle Konfiguration und [Geo-Replikation beenden](sql-database-geo-replication-portal.md#remove-secondary-database). Nach die Aktualisierung neu zu konfigurieren Sie Ihrer Datenbank Geo-Replikation.
- **Öffnen Sie diese Ports, wenn Sie eine Azure-virtuellen Computers Clients haben**: Wenn Ihr Clientprogramm mit SQL-Datenbank V12 verbunden ist, während Ihr Client für eine Azure-virtuellen Computern virtueller (Computer) ausgeführt wird, müssen Sie Portbereiche 11000-11999 und 14000-14999 des virtuellen Computers öffnen. Weitere Informationen finden Sie unter [Ports für SQL-Datenbank V12](sql-database-develop-direct-route-ports-adonet-v12.md).


## <a name="prerequisites"></a>Erforderliche Komponenten

Wenn Sie um einen Server auf V12 mit PowerShell zu aktualisieren, müssen Sie die neuesten Azure PowerShell installiert sein und ausgeführt haben. Ausführliche Informationen finden Sie unter [Informationen zum Installieren und konfigurieren Azure PowerShell](../powershell-install-configure.md).


## <a name="configure-your-credentials-and-select-your-subscription"></a>Konfigurieren Sie Ihre Anmeldeinformationen ein, und wählen Sie Ihr Abonnement

Um PowerShell-Cmdlets für Ihr Abonnement Azure ausführen können, müssen Sie zunächst Access bei Ihrem Azure-Konto herstellen. Führen Sie die folgenden, und es wird mit einer Anmeldebildschirm Ihre Anmeldeinformationen eingeben angezeigt werden. Verwenden Sie den gleichen e-Mail- und das Kennwort, mit denen Sie Azure-Portal anmelden.

    Add-AzureRmAccount

Nach der erfolgreichen Anmeldung sollte einige Informationen auf dem Bildschirm angezeigt werden, die enthält die Id, mit dem Sie angemeldet, und Azure-Abonnements, die, denen Sie Zugriff haben.

Um das Abonnement, die, das Sie mit der Sie arbeiten möchten, wählen müssen Ihr Abonnement-Id (**-SubscriptionId**) oder das Abonnement (**-SubscriptionName**) nennen. Sie können aus dem vorherigen Schritt kopieren, oder wenn Sie mehrere Abonnements haben können, führen Sie das Cmdlet " **Get-AzureRmSubscription** ", und kopieren Sie die gewünschten Abonnementinformationen aus dem Resultset.

Führen Sie das folgende Cmdlet mit Informationen zu Ihrem Abonnement Ihres aktuellen Abonnements festlegen:

    Set-AzureRmContext -SubscriptionId 4cac86b0-1e56-bbbb-aaaa-000000000000

Die folgenden Befehle werden für das Abonnement Ihrer Auswahl einfach über ausgeführt werden.

## <a name="get-recommendations"></a>Lassen Sie sich

So erhalten Sie die Empfehlungen für die Aktualisierung von Server führen Sie das folgende Cmdlet aus:

    $hint = Get-AzureRmSqlServerUpgradeHint -ResourceGroupName “resourcegroup1” -ServerName “server1”

Weitere Informationen finden Sie unter [Erstellen einer Datenbank flexible Ressourcenpool](sql-database-elastic-pool-create-portal.md) und [Azure SQL-Datenbank Preise Ebene Empfehlungen](sql-database-service-tier-advisor.md).



## <a name="start-the-upgrade"></a>Starten des Upgrades

So starten Sie das Upgrade des Servers ausgeführt das folgende Cmdlet aus:

    Start-AzureRmSqlServerUpgrade -ResourceGroupName “resourcegroup1” -ServerName “server1” -ServerVersion 12.0 -DatabaseCollection $hint.Databases -ElasticPoolCollection $hint.ElasticPools  


Beim Ausführen beginnt dieser Befehl Upgradevorgang. Sie können Anpassen der Ausgabe von empfohlen und Bereitstellen von bearbeiteten empfohlen, dieses Cmdlet.


## <a name="upgrade-a-server"></a>Aktualisieren eines Servers


    # Adding the account
    #
    Add-AzureRmAccount

    # Setting the variables
    #
    $SubscriptionName = 'YOUR_SUBSCRIPTION'
    $ResourceGroupName = 'YOUR_RESOURCE_GROUP'
    $ServerName = 'YOUR_SERVER'

    # Selecting the right subscription
    #
    Set-AzureRmContext -SubscriptionName $SubscriptionName

    # Getting the upgrade recommendations
    #
    $hint = Get-AzureRmSqlServerUpgradeHint -ResourceGroupName $ResourceGroupName -ServerName $ServerName

    # Starting the upgrade process
    #
    Start-AzureRmSqlServerUpgrade -ResourceGroupName $ResourceGroupName -ServerName $ServerName -ServerVersion 12.0 -DatabaseCollection $hint.Databases -ElasticPoolCollection $hint.ElasticPools  


## <a name="custom-upgrade-mapping"></a>Benutzerdefinierte upgrade Zuordnung

Wenn die Empfehlungen nicht für Ihre Server und Business Groß-/Kleinschreibung geeignet sind, können Sie auswählen, wie Ihre Datenbanken aktualisiert werden, und können sie einzelne oder flexible Datenbanken zuordnen.

ElasticPoolCollection und DatabaseCollection wurde Parameter sind optional:

    # Creating elastic pool mapping
    #
    $elasticPool = New-Object -TypeName Microsoft.Azure.Management.Sql.Models.UpgradeRecommendedElasticPoolProperties
    $elasticPool.DatabaseDtuMax = 100
    $elasticPool.DatabaseDtuMin = 0
    $elasticPool.Dtu = 800
    $elasticPool.Edition = "Standard"
    $elasticPool.DatabaseCollection = ("DB1", “DB2”, “DB3”, “DB4”)
    $elasticPool.Name = "elasticpool_1"
    $elasticPool.StorageMb = 800

    # Creating single database mapping for 2 databases. DBMain1 mapped to S0 and DBMain2 mapped to S2
    #
    $databaseMap1 = New-Object -TypeName Microsoft.Azure.Management.Sql.Models.UpgradeDatabaseProperties
    $databaseMap1.Name = "DBMain1"
    $databaseMap1.TargetEdition = "Standard"
    $databaseMap1.TargetServiceLevelObjective = "S0"

    $databaseMap2 = New-Object -TypeName Microsoft.Azure.Management.Sql.Models.UpgradeDatabaseProperties
    $databaseMap2.Name = "DBMain2"
    $databaseMap2.TargetEdition = "Standard"
    $databaseMap2.TargetServiceLevelObjective = "S2"

    # Starting the upgrade
    #
    Start-AzureRmSqlServerUpgrade –ResourceGroupName resourcegroup1 –ServerName server1 -ServerVersion 12.0 -DatabaseCollection @($databaseMap1, $databaseMap2) -ElasticPoolCollection @($elasticPool)



## <a name="monitor-databases-after-upgrading-to-sql-database-v12"></a>Überwachen Sie nach dem Aktualisieren auf SQL-Datenbank V12 Datenbanken


Nach der Aktualisierung wird empfohlen, die Datenbank aktiv, um sicherzustellen, Anwendungen ausgeführt werden, auf die gewünschte Leistung und Verwendung optimieren, je nach Bedarf zu überwachen.

Darüber hinaus mit [PowerShell](sql-database-elastic-pool-manage-powershell.md) oder Überwachung einzelne Datenbanken flexible Datenbank Pools [Verwenden des Portals](sql-database-elastic-pool-manage-portal.md) überwacht werden können


**Ressource Verbrauchsdaten:** Für Datenbanken Basic, Standard und Premium ist Verbrauchsdaten Ressource über die [sys.dm_ Db_ Resource_stats](http://msdn.microsoft.com/library/azure/dn800981.aspx) DMV in der Datenbank. Diese DMV bietet nahezu in Echtzeit Verbrauch Ressourceninformationen nach 15 zweiten einzelnen für die vorherige Stunde des Vorgangs. Da die Materialverbrauch maximalen Prozentsatz der CPU-EA und der Protokolldateien Dimensionen wird der DTU Prozentsatz Verbrauch für ein Intervall berechnet. Hier ist eine Abfrage, um die durchschnittliche DTU Prozentsatz Verbrauchsanstieg der letzten Stunde zu berechnen:

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



**Benachrichtigungen:** Richten Sie im Portal Azure Sie benachrichtigt, wenn der DTU Verbrauch für eine Datenbank aktualisierten bestimmte hoher Ebene Ansätze 'Benachrichtigungen' ein. Datenbank-Benachrichtigungen können im Azure-Portal für verschiedene Performance-Werte wie DTU, CPU, EA und Log eingerichtet werden. Navigieren Sie zu Ihrer Datenbank aus, und wählen Sie **Warnungsregeln** in den **Einstellungen** Blade.

Beispielsweise können Sie Einrichten einer e-Mail-Benachrichtigung auf "DTU Prozentsatz" überschreitet der DTU Prozentsatz Durchschnittswert 75 % über die letzten 5 Minuten. Finden Sie weitere Informationen zum Konfigurieren der Benachrichtigung [-Benachrichtigung](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) erhalten.



## <a name="next-steps"></a>Nächste Schritte

- [Erstellen einer Datenbank flexible Ressourcenpool](sql-database-elastic-pool-create-portal.md) und einige oder alle Datenbanken in den Pool hinzufügen.
- [Ändern der Dienstalter Ebene und Leistung der Datenbank](sql-database-scale-up.md).



## <a name="related-information"></a>Weitere Informationen

- [Get-AzureRmSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603582.aspx)
- [Start-AzureRmSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt619403.aspx)
- [Beenden-AzureRmSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603589.aspx)

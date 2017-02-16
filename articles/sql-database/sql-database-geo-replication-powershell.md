<properties 
    pageTitle="Konfigurieren von aktiven Geo-Replikation für Azure SQL-Datenbank mithilfe der PowerShell | Microsoft Azure" 
    description="Konfigurieren von aktiven Geo-Replikation für Azure SQL-Datenbank mithilfe der PowerShell" 
    services="sql-database" 
    documentationCenter="" 
    authors="stevestein" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="powershell"
   ms.workload="NA"
    ms.date="07/14/2016"
    ms.author="sstein"/>

# <a name="configure-geo-replication-for-azure-sql-database-with-powershell"></a>Konfigurieren von Geo-Replikation für Azure-SQL­Datenbank mit PowerShell

> [AZURE.SELECTOR]
- [(Übersicht)](sql-database-geo-replication-overview.md)
- [Azure-Portal](sql-database-geo-replication-portal.md)
- [PowerShell](sql-database-geo-replication-powershell.md)
- [T-SQL](sql-database-geo-replication-transact-sql.md)

In diesem Artikel wird das Konfigurieren der aktiven Geo-Replikation für SQL-Datenbank mit PowerShell veranschaulicht.

Um System durch Verwendung von PowerShell starten zu können, finden Sie unter [ein geplanten oder ungeplanten Failover für SQL Azure-Datenbank mit PowerShell initiiert](sql-database-geo-replication-failover-powershell.md).

>[AZURE.NOTE] Aktive Geo-Replikation (lesbaren sekundäre) steht für alle Datenbanken auf allen Ebenen der Dienst zur Verfügung. In April 2017 nicht lesbaren sekundäre Typ gelöscht werden, und vorhandene nicht lesbaren Datenbanken, um lesbare sekundäre automatisch aktualisiert werden.



So konfigurieren Sie aktiven Geo-Replikation mithilfe der PowerShell, benötigen Sie Folgendes:

- Ein Azure-Abonnement. 
- Ein Azure SQL-Datenbank - die primäre Datenbank, die repliziert werden soll.
- Azure PowerShell 1.0 oder höher. Sie können herunterladen und installieren Sie die Azure PowerShell-Module durch die folgenden [Informationen zum Installieren und konfigurieren Azure PowerShell](../powershell-install-configure.md).


## <a name="configure-your-credentials-and-select-your-subscription"></a>Konfigurieren Sie Ihre Anmeldeinformationen ein, und wählen Sie Ihr Abonnement

Zunächst müssen richten Sie Zugriff auf Ihr Konto Azure also PowerShell starten, und führen Sie dann das folgende Cmdlet aus. Geben Sie im Anmeldebildschirm dieselben e-Mail und Kennwort für die Anmeldung bei der Azure-Portal.


    Login-AzureRmAccount

Nach der erfolgreichen Anmeldung finden Sie einige Informationen auf dem Bildschirm, die enthält die Id, mit dem Sie angemeldet, und Azure-Abonnements, die, denen Sie Zugriff haben.


### <a name="select-your-azure-subscription"></a>Wählen Sie Ihr Abonnement Azure

Zum Auswählen des Abonnements benötigen Sie Ihr Abonnement-ID an. Sie können das Abonnement Id kopieren, aus den Informationen aus dem vorherigen Schritt angezeigt, oder wenn Sie mehrere Abonnements vorhanden sind und Weitere Informationen benötigen, können Sie das Cmdlet " **Get-AzureRmSubscription** " ausführen und kopieren Sie die gewünschten Abonnementinformationen aus dem Resultset. Das folgende Cmdlet verwendet die Abonnement-Id, um das aktuelle Abonnement festzulegen:

    Select-AzureRmSubscription -SubscriptionId 4cac86b0-1e56-bbbb-aaaa-000000000000

Nach dem Ausführen von **Select-AzureRmSubscription** erfolgreich kehren Sie zu der PowerShell auffordern zurück.


## <a name="add-secondary-database"></a>Sekundäre Datenbank hinzufügen


Die folgenden Schritte erstellen eine neue sekundäre Datenbank in eine Zusammenarbeit Geo-Replikation.  
  
Zum Aktivieren eines sekundären müssen Sie der Abonnementbesitzer oder gemeinsame Besitzer sein. 

Das Cmdlet " **New-AzureRmSqlDatabaseSecondary** " können Sie hinzufügen, dass eine sekundäre Datenbank auf einem Partnerserver, um eine lokale Datenbank auf dem Server, den Sie sich befinden (die primäre Datenbank) verbunden. 

Dieses Cmdlet ersetzt **Start-AzureSqlDatabaseCopy** mit dem Parameter **– IsContinuous** .  Es wird ein Objekt **AzureRmSqlDatabaseSecondary** ausgeben, die von anderen Cmdlets verwendet werden kann, um einen bestimmten Replikation Link klar zu identifizieren. Dieses Cmdlet zurückgegeben werden kann, wenn die sekundäre Datenbank erstellt und vollständig gefüllt wird. Abhängig von der Größe der Datenbank kann es von Minuten Stunden dauern.

Die Datenbank replizierte, auf dem sekundären Server haben denselben Namen wie die Datenbank auf dem primären Server und, standardmäßig muss dieselbe Service-Version. Sekundäre Datenbank nicht lesbaren oder gelesen werden kann, und eine einzelne Datenbank oder eine Datenbank flexible werden kann. Weitere Informationen finden Sie unter [Neu-AzureRMSqlDatabaseSecondary](https://msdn.microsoft.com/library/mt603689.aspx) und [Dienst Stufen](sql-database-service-tiers.md).
Nach der sekundären erstellt haben und die sich befinden, werden Daten aus der primären Datenbank auf die neue sekundäre Datenbank repliziert beginnen. Die folgenden Schritte beschreiben, wie zum Ausführen dieser Aufgabe mithilfe der PowerShell nicht lesbaren und lesbar sekundäre entweder mit einer einzelnen Datenbank oder eine flexible Datenbank zu erstellen.

Der Befehl schlägt fehl, wenn die Partner-Datenbank (beispielsweise - als Ergebnis Beenden einer vorherigen Geo-Replikation Beziehung) bereits vorhanden ist.



### <a name="add-a-non-readable-secondary-single-database"></a>Fügen Sie einen nicht lesbaren sekundären (einzelne Datenbank hinzu)

Mit dem folgende Befehl wird eine nicht lesbaren sekundäre Datenbank "Mydb" der Server "srv2" in der Ressource Gruppe "rg2" erstellt:

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" -ResourceGroupName "rg1" -ServerName "srv1"
    $secondaryLink = $database | New-AzureRmSqlDatabaseSecondary –PartnerResourceGroupName "rg2" –PartnerServerName "srv2" -AllowConnections "No"



### <a name="add-readable-secondary-single-database"></a>Hinzufügen von lesbare sekundären (einzelne Datenbank)

Mit dem folgende Befehl wird eine lesbare sekundäre Datenbank "Mydb" der Server "srv2" in der Ressource Gruppe "rg2" erstellt:

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" -ResourceGroupName "rg1" -ServerName "srv1"
    $secondaryLink = $database | New-AzureRmSqlDatabaseSecondary –PartnerResourceGroupName "rg2" –PartnerServerName "srv2" -AllowConnections "All"




### <a name="add-a-non-readable-secondary-elastic-database"></a>Fügen Sie einen nicht lesbaren sekundären (Flexible Datenbank hinzu)

Mit dem folgende Befehl erstellt eine nicht lesbaren sekundäre Datenbank "Mydb" im Pool flexible Datenbank mit dem Namen "ElasticPool1" der Server "srv2" in der Ressource Gruppe "rg2":

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" -ResourceGroupName "rg1" -ServerName "srv1"
    $secondaryLink = $database | New-AzureRmSqlDatabaseSecondary –PartnerResourceGroupName "rg2" –PartnerServerName "srv2" –SecondaryElasticPoolName "ElasticPool1" -AllowConnections "No"


### <a name="add-a-readable-secondary-elastic-database"></a>Fügen Sie einen lesbaren sekundären (Flexible Datenbank hinzu)

Mit dem folgende Befehl erstellt eine lesbare sekundäre Datenbank "Mydb" im Pool flexible Datenbank mit dem Namen "ElasticPool1" der Server "srv2" in der Ressource Gruppe "rg2":

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" -ResourceGroupName "rg1" -ServerName "srv1"
    $secondaryLink = $database | New-AzureRmSqlDatabaseSecondary –PartnerResourceGroupName "rg2" –PartnerServerName "srv2" –SecondaryElasticPoolName "ElasticPool1" -AllowConnections "All"





## <a name="remove-secondary-database"></a>Sekundäre Datenbank entfernen

Verwenden Sie das Cmdlet **AzureRmSqlDatabaseSecondary entfernen** , um die Replikation Zusammenarbeit zwischen einer sekundären Datenbank und primärem dauerhaft zu beenden. Nach Beendigung der Beziehung wird die sekundäre Datenbank einer Datenbank Lese-und Schreibzugriff auf. Wenn die Verbindung zu sekundäre Datenbank fehlerhaft ist der Befehl erfolgreich ausgeführt wird, aber des sekundären werden erst Lese-und Schreibzugriff, nach der Wiederherstellung Connectivity. Weitere Informationen finden Sie unter [Entfernen-AzureRmSqlDatabaseSecondary](https://msdn.microsoft.com/library/mt603457.aspx) und [Dienst Ebenen](sql-database-service-tiers.md).

Dieses Cmdlet ersetzt beenden-AzureSqlDatabaseCopy für die Replikation. 

Das Entfernen entspricht der erzwungenen Beendigung, die entfernt die Replikation Verknüpfung und belässt die ehemalige sekundäre als eigenständige Datenbank, die vor der Terminierung ist nicht vollständig repliziert. Alle Verknüpfen von Daten werden auf dem unter seinem früheren primären und sekundären, ehemalige bereinigt ist oder wenn sie zur Verfügung stehen. Dieses Cmdlet zurückgegeben werden kann, wenn der Link Replikation entfernt wird. 


Um sekundäre zu entfernen, sollte der Benutzer Schreibzugriff auf primären und sekundären Datenbanken entsprechend RBAC haben. Rollenbasierte Access-Steuerelement, um Details finden Sie unter.

Die folgenden entfernt Replikation Link mit einer Datenbank mit dem Namen "Mydb" Server "srv2" von der Ressource "rg2". 

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" -ResourceGroupName "rg1" -ServerName "srv1"
    $secondaryLink = $database | Get-AzureRmSqlDatabaseReplicationLink –SecondaryResourceGroup "rg2" –PartnerServerName "srv2"
    $secondaryLink | Remove-AzureRmSqlDatabaseSecondary 


## <a name="monitor-geo-replication-configuration-and-health"></a>Gesundheit und Monitor Geo-Replikation über den Computer

Überwachung Aufgaben aufnehmen der Konfiguration Geo-Replikation für die Überwachung und Daten Replikationszustand überwachen.  

[Get-AzureRmSqlDatabaseReplicationLink](https://msdn.microsoft.com/library/mt619330.aspx) kann verwendet werden, um die Informationen zu den Weiterleiten Replikation Links in der Katalogansicht sys.geo_replication_links sichtbar abzurufen.

Mit dem folgende Befehl ruft Status der Replikation Verknüpfung zwischen der primären Datenbank "Mydb" und des sekundären auf dem Server "srv2" von der Ressource "rg2" ab.

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" -ResourceGroupName "rg1" -ServerName "srv1"
    $secondaryLink = $database | Get-AzureRmSqlDatabaseReplicationLink –PartnerResourceGroup "rg2” –PartnerServerName "srv2”


## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu aktiven Geo-Replikation finden Sie unter - [Active Geo-Replikation](sql-database-geo-replication-overview.md)
- Eine Übersicht über Business-Continuity und Szenarien finden Sie unter [Übersicht über die Business continuity](sql-database-business-continuity.md)


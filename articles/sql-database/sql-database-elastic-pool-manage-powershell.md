<properties 
    pageTitle="Verwalten einer Datenbank flexible Ressourcenpool (PowerShell) | Microsoft Azure" 
    description="Informationen Sie zum PowerShell verwenden, um eine flexible Datenbank Ressourcenpool zu verwalten."  
    services="sql-database" 
    documentationCenter="" 
    authors="srinia" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management" 
    ms.date="06/22/2016"
    ms.author="srinia"/>

# <a name="monitor-and-manage-an-elastic-database-pool-with-powershell"></a>Überwachen Sie und verwalten Sie einer Datenbank flexible Ressourcenpool mit PowerShell 

> [AZURE.SELECTOR]
- [Azure-portal](sql-database-elastic-pool-manage-portal.md)
- [PowerShell](sql-database-elastic-pool-manage-powershell.md)
- [C#](sql-database-elastic-pool-manage-csharp.md)
- [T-SQL](sql-database-elastic-pool-manage-tsql.md)

Verwalten einer [Datenbank flexible Ressourcenpool](sql-database-elastic-pool.md) PowerShell-Cmdlets verwenden. 

Allgemeine Fehlercodes, finden Sie unter [SQL-Fehlercodes für SQL-Datenbank-Clientanwendungen: Verbindungsfehler und andere Probleme-Datenbank](sql-database-develop-error-messages.md).

Werte für Pools finden Sie im [Grenzwerte für eDTU und Speicher](sql-database-elastic-pool.md#eDTU-and-storage-limits-for-elastic-pools-and-elastic-databases). 

## <a name="prerequisites"></a>Erforderliche Komponenten

* Azure PowerShell 1.0 oder höher. Ausführliche Informationen finden Sie unter [Informationen zum Installieren und konfigurieren Azure PowerShell](../powershell-install-configure.md).
* Flexible Datenbank Pools sind nur mit SQL-Datenbank V12-Servern verfügbar. Wenn Sie einen SQL-Datenbank V11 Server, [PowerShell upgrade auf V12 und erstellen einen Pool verwenden](sql-database-upgrade-server-portal.md) Sie in einem Schritt haben.


## <a name="move-a-database-into-an-elastic-pool"></a>Verschieben einer Datenbank in eine flexible Ressourcenpool

Sie können eine Datenbank in oder aus einem Ressourcenpool mit der [Set-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619433.aspx)verschieben. 

    Set-AzureRmSqlDatabase -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"

## <a name="change-performance-settings-of-a-pool"></a>Ändern der Einstellungen der Leistung von einem Ressourcenpool

Wenn die Leistungsfähigkeit wird, können Sie die Einstellungen im Ressourcenpool an Wachstum ändern. Verwenden Sie das Cmdlet " [Set-AzureRmSqlElasticPool](https://msdn.microsoft.com/library/azure/mt603511.aspx) " ein. Legen Sie den Dtu - Parameter auf die eDTUs pro Pool ein. Finden Sie unter [Grenzwerte für eDTU und Speicher](sql-database-elastic-pool.md#eDTU-and-storage-limits-for-elastic-pools-and-elastic-databases) für mögliche Werte.  

    Set-AzureRmSqlElasticPool –ResourceGroupName “resourcegroup1” –ServerName “server1” –ElasticPoolName “elasticpool1” –Dtu 1200 –DatabaseDtuMax 100 –DatabaseDtuMin 50 


## <a name="get-the-status-of-pool-operations"></a>Erhalten Sie den Status von Vorgängen Ressourcenpool

Erstellen einen Pool kann Zeit in Anspruch nehmen. Um den Status der Ressourcenpool Vorgänge, einschließlich der Erstellung und Updates zu verfolgen, verwenden Sie das Cmdlet " [Get-AzureRmSqlElasticPoolActivity](https://msdn.microsoft.com/library/azure/mt603812.aspx) " ein.

    Get-AzureRmSqlElasticPoolActivity –ResourceGroupName “resourcegroup1” –ServerName “server1” –ElasticPoolName “elasticpool1” 


## <a name="get-the-status-of-moving-an-elastic-database-into-and-out-of-a-pool"></a>Abrufen des Status eine flexible-Datenbank verschieben, in die und aus einem Ressourcenpool

Verschieben einer Datenbank kann Zeit in Anspruch nehmen. Verwenden das Cmdlet " [Get-AzureRmSqlDatabaseActivity](https://msdn.microsoft.com/library/azure/mt603687.aspx) " verschieben Status zu verfolgen.

    Get-AzureRmSqlDatabaseActivity -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"

## <a name="get-resource-usage-data-for-a-pool"></a>Erste Verwendung Ressourcendaten für einen pool

Kriterien, die als Prozentwert für die Ressourcenpool Beschränkung abgerufen werden können:   


| Metrische Namen | Beschreibung |
| :-- | :-- |
| CPU\_Prozent | Mittelwert Auslastung in Prozent, der die Beschränkung der Ressourcenpool zu berechnen. |
| physische\_Daten\_lesen\_Prozent | Durchschnittliche e/a-Nutzung in Prozent basierend auf dem Pool Limit. |
| Log\_schreiben\_Prozent | Mittelwert schreiben Ressource Auslastung in Prozent, der die Beschränkung der Ressourcenpool an. | 
| DTU\_Verbrauch\_Prozent | Durchschnittliche eDTU Auslastung in Prozent der eDTU Grenzwert für den Ressourcenpool | 
| Speicher\_Prozent | Durchschnittliche Speicher Auslastung in Prozent des Speicherlimits im Ressourcenpool. |  
| Kollegen\_Prozent | Maximale gleichzeitige Worker (Anfragen) in Prozent basierend auf dem Pool Limit. |  
| Sitzungen\_Prozent | Maximale gleichzeitige Sitzungen in Prozent basierend auf dem Pool Limit. | 
| eDTU_limit | Aktuelle max flexible Ressourcenpool DTU Einstellung für dieses flexible Ressourcenpool während dieses Intervalls. |
| Speicher\_Grenzwert | Aktuelle max flexible Ressourcenpool Speichergrenzwert für dieses flexible Ressourcenpool in Megabyte während dieses Intervalls festlegen. |
| eDTU\_verwendet | Durchschnittliche eDTUs vom Pool in diesem Intervall verwendet. |
| Speicher\_verwendet | Durchschnittliche vom Pool in diesem Intervall in Byte verwendete Speicher |

**Kennzahlen Genauigkeit/Aufbewahrung Perioden zu Zahlen sind:**

* Daten werden nach 5 Minuten einzelnen zurückgegeben.  
* Daten Aufbewahrungsrichtlinien, 35 Tage.  

Dieses Cmdlet und API schränkt die Anzahl der Zeilen, die in einem Anruf 1000 Zeilen (etwa 3 Tage von Daten nach einzelnen 5 Minuten) abgerufen werden können. Aber dieser Befehl mehrmals mit verschiedenen Anfangs-/Ende Zeitintervalle zum Abrufen von weiteren Daten aufgerufen werden können 

So rufen Sie die Metrik ab:

    $metrics = (Get-AzureRmMetric -ResourceId /subscriptions/<subscriptionId>/resourceGroups/FabrikamData01/providers/Microsoft.Sql/servers/fabrikamsqldb02/elasticPools/franchisepool -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime "4/18/2015" -EndTime "4/21/2015")  


## <a name="get-resource-usage-data-for-an-elastic-database"></a>Erste Verwendung Ressourcendaten für eine flexible Datenbank

Diese APIs sind identisch mit den aktuellen (V12)-APIs für die Ressource Auslastung einer eigenständigen Datenbank, eine Ausnahme bilden jedoch die folgenden semantische Differenz Überwachung verwendet. 

Kennzahlen abgerufen werden ausgedrückt für diese API, als Prozentwert der pro max eDTUs (oder gleichwertiger Linienende für die zugrunde liegende Metrik wie CPU, EA usw.) für diesen Pool festlegen. Beispielsweise 50 % Auslastung einer beliebigen der folgenden Metrik gibt an, dass bestimmte Ressourcenverbrauch auf 50 % der der pro Datenbank Linienende Grenzwert für diese Ressource im übergeordneten Pool. 

So rufen Sie die Metrik ab:

    $metrics = (Get-AzureRmMetric -ResourceId /subscriptions/<subscriptionId>/resourceGroups/FabrikamData01/providers/Microsoft.Sql/servers/fabrikamsqldb02/databases/myDB -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime "4/18/2015" -EndTime "4/21/2015") 

## <a name="add-an-alert-to-a-pool-resource"></a>Hinzufügen einer Benachrichtigung für eine Ressource Ressourcenpool

Hinzufügen von Warnungsregeln zu Ressourcen, die e-Mail-Benachrichtigungen oder benachrichtigen Zeichenfolgen mit [URL Endpunkten](https://msdn.microsoft.com/library/mt718036.aspx) senden, wenn die Ressource einen Schwellenwert Auslastung Treffer, den Sie einrichten. Verwenden Sie das Cmdlet AzureRmMetricAlertRule hinzufügen. 

> [AZURE.IMPORTANT]Ressource Auslastung für flexible Pools Überwachung verfügt über eine Verzögerung von mindestens 20 Minuten. Festlegen von Benachrichtigungen von weniger als 30 Minuten für flexible Pools wird derzeit nicht unterstützt. Legen Sie alle Warnungen für flexible Pools mit einem Punkt (Parameter mit der Bezeichnung "-WindowSize" in PowerShell-API) von weniger als 30 Minuten möglicherweise nicht ausgelöst werden. Stellen Sie sicher, dass alle Benachrichtigungen, die Sie für flexible Pools definieren einen Zeitraum (WindowSize) 30 Minuten oder mehr verwenden.

In diesem Beispiel fügt eine Benachrichtigung für die erste benachrichtigt, wenn ein Ressourcenpool eDTU Verbrauch über bestimmten Schwellenwert überschreitet.

    # Set up your resource ID configurations
    $subscriptionId = '<Azure subscription id>'      # Azure subscription ID
    $location =  '<location'                         # Azure region
    $resourceGroupName = '<resource group name>'     # Resource Group
    $serverName = '<server name>'                    # server name
    $poolName = '<elastic pool name>'                # pool name 

    #$Target Resource ID
    $ResourceID = '/subscriptions/' + $subscriptionId + '/resourceGroups/' +$resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/elasticpools/' + $poolName

    # Create an email action
    $actionEmail = New-AzureRmAlertRuleEmail -SendToServiceOwners -CustomEmail JohnDoe@contoso.com

    # create a unique rule name
    $alertName = $poolName + "- DTU consumption rule"

    # Create an alert rule for DTU_consumption_percent
    Add-AzureRMMetricAlertRule -Name $alertName -Location $location -ResourceGroup $resourceGroupName -TargetResourceId $ResourceID -MetricName "DTU_consumption_percent"  -Operator GreaterThan -Threshold 80 -TimeAggregationOperator Average -WindowSize 00:60:00 -Actions $actionEmail 

## <a name="add-alerts-to-all-databases-in-a-pool"></a>Hinzufügen von Benachrichtigungen für alle Datenbanken in einem Ressourcenpool

Sie können alle Datenbank in eine flexible Ressourcenpool zum Senden von e-Mail-Benachrichtigungen Warnungsregeln hinzufügen oder benachrichtigen Zeichenfolgen mit [URL Endpunkte](https://msdn.microsoft.com/library/mt718036.aspx) bei einer Ressource Treffer einen Auslastung Schwellenwert einrichten, indem Sie die Benachrichtigung. 

> [AZURE.IMPORTANT] Ressource Auslastung für flexible Pools Überwachung verfügt über eine Verzögerung von mindestens 20 Minuten. Festlegen von Benachrichtigungen von weniger als 30 Minuten für flexible Pools wird derzeit nicht unterstützt. Legen Sie alle Warnungen für flexible Pools mit einem Punkt (Parameter mit der Bezeichnung "-WindowSize" in PowerShell-API) von weniger als 30 Minuten möglicherweise nicht ausgelöst werden. Stellen Sie sicher, dass alle Benachrichtigungen, die Sie für flexible Pools definieren einen Zeitraum (WindowSize) 30 Minuten oder mehr verwenden.

In diesem Beispiel fügt eine Benachrichtigung zu den einzelnen Datenbanken in einem Ressourcenpool für die erste benachrichtigt, wenn die Datenbank DTU Verbrauch über bestimmten Schwellenwert überschreitet.

    # Set up your resource ID configurations
    $subscriptionId = '<Azure subscription id>'      # Azure subscription ID
    $location = '<location'                          # Azure region
    $resourceGroupName = '<resource group name>'     # Resource Group
    $serverName = '<server name>'                    # server name
    $poolName = '<elastic pool name>'                # pool name 

    # Get the list of databases in this pool.
    $dbList = Get-AzureRmSqlElasticPoolDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -ElasticPoolName $poolName

    # Create an email action
    $actionEmail = New-AzureRmAlertRuleEmail -SendToServiceOwners -CustomEmail JohnDoe@contoso.com

    # Get resource usage metrics for a database in an elastic database for the specified time interval.
    foreach ($db in $dbList)
    {
    $dbResourceId = '/subscriptions/' + $subscriptionId + '/resourceGroups/' + $resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/databases/' + $db.DatabaseName

    # create a unique rule name
    $alertName = $db.DatabaseName + "- DTU consumption rule"

    # Create an alert rule for DTU_consumption_percent
    Add-AzureRMMetricAlertRule -Name $alertName  -Location $location -ResourceGroup $resourceGroupName -TargetResourceId $dbResourceId -MetricName "dtu_consumption_percent"  -Operator GreaterThan -Threshold 80 -TimeAggregationOperator Average -WindowSize 00:60:00 -Actions $actionEmail

    # drop the alert rule
    #Remove-AzureRmAlertRule -ResourceGroup $resourceGroupName -Name $alertName
    } 



## <a name="collect-and-monitor-resource-usage-data-across-multiple-pools-in-a-subscription"></a>Erfassen und Überwachen von Verwendungsdaten Ressource über mehrere Pools in einem Abonnement

Wenn Sie ein Abonnement eine große Anzahl von Datenbanken haben, ist es schwierig zu jeder flexible Pool separat überwachen. Stattdessen können SQL-Datenbank-PowerShell-Cmdlets und T-SQL-Abfragen Ressource: Einsatz Daten aus mehreren Pools und deren Datenbanken für die Überwachung und Analyse der Ressource: Einsatz sammeln kombiniert werden. Ein [Beispiel für die Implementierung](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools) solche einer Reihe von Powershell-Skripts finden Sie in das GitHub SQL Server-Beispiele Repository neben einer Dokumentation Zweck und zur gemeinsamen Nutzung.

Zum Verwenden dieser Stichprobe Implementierung gehen Sie folgendermaßen vor unten aufgelistet.


1. Laden Sie die [Skripts und der Dokumentation](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools):
2. Ändern Sie die Skripts für Ihre Umgebung an. Geben Sie einen oder mehrere Server, auf dem flexible Pools gehostet werden.
3. Geben Sie eine telemetrieprotokoll Datenbank an, wo sich die zusammengestellten Metrik befinden, gespeichert werden soll. 
4. Passen Sie das Skript, um die Dauer der Ausführung der Skripts angeben.

Führen Sie die Skripts auf hoher Ebene die folgenden:

*   Listet alle Server in einer angegebenen Azure-Abonnements (oder eine angegebene Liste von Servern).
*   Führt einen Hintergrundauftrag für jeden Server. Die Position in einer Schleife ausgeführt wird, in regelmäßigen Abständen und erfasst werden Daten für alle Pools auf dem Server. Dann wird die gesammelten Daten geladen, in der Datenbank angegebenen werden.
*   Zählt eine Liste der Datenbanken in jedem Pool zum Sammeln von Daten in der Datenbank Ressource: Einsatz. Daraufhin lädt er die gesammelten Daten in der Datenbank werden.

Die zusammengestellten Kriterien in der Datenbank werden können zum Überwachen der der Integrität des flexible Pools und die Datenbanken darin analysiert werden. Das Skript installiert auch einer vordefinierten Tabelle-Value-Funktion (Tabellenwertfunktion), in der Datenbank werden Aggregat unterstützen die Kriterien für eine angegebene Zeit-Fenster. Beispielsweise können Resultate der Tabellenwertfunktion dienen "Top N flexible Pools mit der maximalen eDTU Auslastung in einem angegebenen Zeitfenster." angezeigt. Verwenden Sie optional analytisches-Tools, wie Excel oder Power BI Abfragen und Analysieren der gesammelten Daten ein.

## <a name="example-retrieve-resource-consumption-metrics-for-a-pool-and-its-databases"></a>Beispiel: Abrufen von Ressourcen Verbrauch Kennzahlen für einen Pool und die zugehörigen Datenbanken

In diesem Beispiel ruft die Metrik Verbrauch für einen angegebenen flexible Pool und alle zugehörigen Datenbanken ab. Gesammelte Daten formatiert und in eine CSV-Datei formatierten geschrieben. Die Datei kann mit Excel durchsucht werden.

    $subscriptionId = '<Azure subscription id>'       # Azure subscription ID
    $resourceGroupName = '<resource group name>'             # Resource Group
    $serverName = <server name>                              # server name
    $poolName = <elastic pool name>                          # pool name
        
    # Login to Azure account and select the subscription.
    Login-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $subscriptionId
    
    # Get resource usage metrics for an elastic pool for the specified time interval.
    $startTime = '4/27/2016 00:00:00'  # start time in UTC
    $endTime = '4/27/2016 01:00:00'    # end time in UTC
    
    # Construct the pool resource ID and retrive pool metrics at 5 minute granularity.
    $poolResourceId = '/subscriptions/' + $subscriptionId + '/resourceGroups/' + $resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/elasticPools/' + $poolName
    $poolMetrics = (Get-AzureRmMetric -ResourceId $poolResourceId -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime $startTime -EndTime $endTime) 
    
    # Get the list of databases in this pool.
    $dbList = Get-AzureRmSqlElasticPoolDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -ElasticPoolName $poolName
    
    # Get resource usage metrics for a database in an elastic database for the specified time interval.
    $dbMetrics = @()
    foreach ($db in $dbList)
    {
        $dbResourceId = '/subscriptions/' + $subscriptionId + '/resourceGroups/' + $resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/databases/' + $db.DatabaseName
        $dbMetrics = $dbMetrics + (Get-AzureRmMetric -ResourceId $dbResourceId -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime $startTime -EndTime $endTime)
    }
    
    #Optionally you can format the metrics and output as .csv file using the following script block.
    $command = {
    param($metricList, $outputFile)

    # Format metrics into a table.
    $table = @()
    foreach($metric in $metricList) { 
      foreach($metricValue in $metric.MetricValues) {
        $sx = New-Object PSObject -Property @{
            Timestamp = $metricValue.Timestamp.ToString()
            MetricName = $metric.Name; 
            Average = $metricValue.Average;
            ResourceID = $metric.ResourceId 
          }
          $table = $table += $sx
      }
    }
    
    # Output the metrics into a .csv file.
    write-output $table | Export-csv -Path $outputFile -Append -NoTypeInformation
    }
    
    # Format and output pool metrics
    Invoke-Command -ScriptBlock $command -ArgumentList $poolMetrics,c:\temp\poolmetrics.csv
    
    # Format and output database metrics
    Invoke-Command -ScriptBlock $command -ArgumentList $dbMetrics,c:\temp\dbmetrics.csv



## <a name="latency-of-elastic-pool-operations"></a>Wartezeit flexible Ressourcenpool Vorgänge

- Ändern die min eDTUs pro Datenbank oder Max eDTUs pro Datenbank in der Regel führt in 5 Minuten oder weniger.
- Ändern der eDTUs pro Pool, hängt von der Gesamtmenge von allen Datenbanken im Pool belegte Speicherplatz ab. Mittelwert der Änderungen 90 Minuten oder weniger pro 100 GB. Wenn der gesamte Speicherplatz von verwendet alle Datenbanken im Pool beträgt beispielsweise 200 GB, und klicken Sie dann die erwartete Wartezeit zum Ändern der Ressourcenpool eDTU pro Pool 3 Stunden oder weniger.

## <a name="migrate-from-v11-to-v12-servers"></a>Migrieren von V11 zu V12-servers

PowerShell-Cmdlets stehen beginnen, beenden oder ein Upgrade auf Azure SQL-Datenbank V12 V11 oder eine andere Version von Pre-V12 überwachen.

- [Upgrade auf SQL-Datenbank V12 mithilfe der PowerShell](sql-database-upgrade-server-powershell.md)

Dokumentation zu diesen PowerShell-Cmdlets finden Sie unter:


- [Get-AzureRMSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603582.aspx)
- [Start-AzureRMSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt619403.aspx)
- [Beenden-AzureRMSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603589.aspx)


Das Beenden-Cmdlet bedeutet aufheben möchten, halten Sie nicht. Es gibt keine Möglichkeit, ein Upgrade, als erneut beginnend mit der ersten fortsetzen aus. Das Beenden-Cmdlet bereinigt und alle entsprechende Ressourcen frei.

## <a name="next-steps"></a>Nächste Schritte

- [Flexible Aufträge erstellen](sql-database-elastic-jobs-overview.md) Flexible Aufträge ermöglichen Ihnen die T-SQL-Skripts für eine beliebige Anzahl von Datenbanken in dem Pool ausgeführt werden.
- Finden Sie unter [Skalierung heraus mit Azure SQL-Datenbank](sql-database-elastic-scale-introduction.md): Verwenden Sie flexible Datenbanktools Skalierung, Verschieben von Daten, Abfrage, oder erstellen Sie Transaktionen.

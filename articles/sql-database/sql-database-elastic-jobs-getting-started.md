<properties
    pageTitle="Erste Schritte mit flexible Datenbankaufträge"
    description="So Datenbankaufträge flexible verwenden"
    services="sql-database"
    documentationCenter=""  
    manager="jhubbard"
    authors="ddove"/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="ddove" />

# <a name="getting-started-with-elastic-database-jobs"></a>Erste Schritte mit flexible Datenbank Aufträge

Flexible Datenbank Einzelvorgänge (Preview) für SQL Azure-Datenbank ermöglicht Ihnen, Zuverlässigkeit T-SQL-Skripts ausführen, die mehrere Datenbanken umfassen während automatisch wiederholen und tatsächlichen Fertigstellung Garantien bietet. Weitere Informationen zur Funktion Auftrag flexible Datenbank finden Sie unter der [Funktion Übersichtsseite](sql-database-elastic-jobs-overview.md).

In diesem Thema erweitert die Stichprobe in [Erste Schritte mit flexible Datenbanktools](sql-database-elastic-scale-get-started.md)gefunden. Wenn abgeschlossen ist, werden Sie: Informationen zum Erstellen und Verwalten von Aufträgen, die eine Gruppe von verwandten Datenbanken verwalten. Es ist nicht erforderlich, um die flexible skalieren-Tools verwenden, um die Vorteile von flexible Aufträge nutzen.

## <a name="prerequisites"></a>Erforderliche Komponenten

Herunterladen Sie und führen Sie der [Erste Schritte mit flexible Tools-Beispieldatenbank aus](sql-database-elastic-scale-get-started.md).

## <a name="create-a-shard-map-manager-using-the-sample-app"></a>Erstellen einer Shard Karte Manager mithilfe der Stichprobe-app

Hier werden Sie ein Schema Shard Manager zusammen mit mehreren mehrere Shards hinweg, gefolgt von Einfügen von Daten in der mehrere Shards hinweg erstellen. Wenn Sie bereits über mehrere Shards hinweg mit sharded Daten in diese eingerichtet haben, können Sie überspringen Sie die folgenden Schritte und wechseln zum nächsten Abschnitt.

1. Erstellen Sie, und führen Sie die **Erste Schritte mit flexible Datenbanktools** Stichprobe Anwendung. Führen Sie die Schritte erst in Schritt 7 im Abschnitt [herunterladen, und führen Sie die app Stichprobe](sql-database-elastic-scale-get-started.md#Getting-started-with-elastic-database-tools). Am Ende von Schritt 7 sehen Sie die folgenden Befehlszeile eingeben:

    ![Befehlszeile][1]

2.  Geben Sie im Befehlsfenster "1", und drücken Sie die **EINGABETASTE**. Dies erstellt die Shard-Manager die Karte und addiert zwei mehrere Shards hinweg auf dem Server. Dann geben Sie "3" ein, und drücken Sie **EINGABETASTE**; Wiederholen Sie diese Aktion viermal aus. Dadurch wird die Stichprobe Datenzeilen in Ihrer mehrere Shards hinweg eingefügt.

3.  Der [Azure-Portal](https://portal.azure.com) sollte drei neue Datenbanken in Ihrem Server v12 anzeigen:

    ![Visual Studio-Bestätigung][2]

    An diesem Punkt erstellen wir eine Auflistung von benutzerdefinierten Datenbank, die alle Datenbanken in der Karte Shard wiedergibt. Dadurch können wir erstellen und Ausführen eines Auftrags, das Hinzufügen eine neue Tabelle über mehrere Shards hinweg.

Hier möchten wir normalerweise ein Shard Karte verwendet wird, erstellen Sie mithilfe des Cmdlets **New-AzureSqlJobTarget** . Muss die Shard Karte Manager-Datenbank als Ziel der Datenbank festgelegt werden, und klicken Sie dann die bestimmte Shard Map als Ziel angegeben ist. In diesem Fall werden wir Auflisten aller Datenbanken auf dem Server, und fügen die Datenbank in die neue benutzerdefinierte Auflistung mit Ausnahme von master-Datenbank.

##<a name="creates-a-custom-collection-and-adds-all-databases-in-the-server-to-the-custom-collection-target-with-the-exception-of-master"></a>Erstellt eine benutzerdefinierte Auflistung, und fügt alle Datenbanken auf dem Server das Ziel benutzerdefinierte Auflistung mit Ausnahme von Master-Shape hinzu.


    $customCollectionName = "dbs_in_server"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName 
    $ResourceGroupName = "ddove_samples"
    $ServerName = "samples"
    $dbsinserver = Get-AzureRMSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName 
    $dbsinserver | %{
    $currentdb = $_.DatabaseName 
    $ErrorActionPreference = "Stop"
    Write-Output ""

    Try
    {
       New-AzureSqlJobTarget -ServerName $ServerName -DatabaseName $currentdb | Write-Output
    }
    Catch 
    {
        $ErrorMessage = $_.Exception.Message
        $ErrorCategory = $_.CategoryInfo.Reason
                
        if ($ErrorCategory -eq 'UniqueConstraintViolatedException')
        {
             Write-Host $currentdb "is already a database target." 
        }

        else
        {
            throw $_
        }
    
    }

    Try
    {
        if ($currentdb -eq "master")
        {
            Write-Host $currentdb "will not be added custom collection target" $CustomCollectionName "."
        }

        else
        {
            Add-AzureSqlJobChildTarget -CustomCollectionName $CustomCollectionName -ServerName $ServerName -DatabaseName $currentdb
            Write-Host $currentdb "was added to" $CustomCollectionName "."
        }

    }
    Catch 
    {
        $ErrorMessage = $_.Exception.Message
        $ErrorCategory = $_.CategoryInfo.Reason

        if ($ErrorCategory -eq 'UniqueConstraintViolatedException')
        {
             Write-Host $currentdb "is already in the custom collection target" $CustomCollectionName"."
        }

        else
        {
            throw $_
        }
    }
    $ErrorActionPreference = "Continue"
}
    
## <a name="create-a-t-sql-script-for-execution-across-databases"></a>Erstellen Sie ein T-SQL-Skript für die Ausführung über Datenbanken

    $scriptName = "NewTable"
    $scriptCommandText = "
    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'Test')
    BEGIN
        CREATE TABLE Test(
            TestId INT PRIMARY KEY IDENTITY,
            InsertionTime DATETIME2
        );
    END
    GO
    INSERT INTO Test(InsertionTime) VALUES (sysutcdatetime());
    GO"

    $script = New-AzureSqlJobContent -ContentName $scriptName -CommandText $scriptCommandText
    Write-Output $script

##<a name="create-the-job-to-execute-a-script-across-the-custom-group-of-databases"></a>Erstellen Sie den Auftrag zum Ausführen eines Skripts über die benutzerdefinierte Gruppe von Datenbanken

    $jobName = "create on server dbs"
    $scriptName = "NewTable"
    $customCollectionName = "dbs_in_server"
    $credentialName = "ddove66"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job


##<a name="execute-the-job"></a>Führen Sie den Auftrag 

Das folgende PowerShell-Skript kann verwendet werden, um einen vorhandenen Auftrag ausführen:

Aktualisieren Sie die folgende Variable entsprechend den Namen der gewünschten Position ausgeführt haben:

    $jobName = "create on server dbs"
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName 
    Write-Output $jobExecution
 
## <a name="retrieve-the-state-of-a-single-job-execution"></a>Den Status für die Ausführung eines einzelnen Auftrags abrufen

Verwenden Sie das gleiche **Get-AzureSqlJobExecution** -Cmdlet mit dem Parameter **IncludeChildren** , um den Status der untergeordneten Position Ausführungen, nämlich den genauen Zustand für jede Ausführung des Auftrags für jede Datenbank, indem Sie den Auftrag als Ziel anzuzeigen.

    $jobExecutionId = "{Job Execution Id}"
    $jobExecutions = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId -IncludeChildren
    Write-Output $jobExecutions 

## <a name="view-the-state-across-multiple-job-executions"></a>Anzeigen des Status über mehrere Auftrag Ausführungen

Das Cmdlet " **Get-AzureSqlJobExecution** " verfügt über mehrere optionale Parameter, die zum Anzeigen von mehreren Auftrag Ausführungen, bis der bereitgestellten Parameter gefiltert verwendet werden können. Das folgende Beispiel veranschaulicht einige der möglichen Methoden Get-AzureSqlJobExecution verwenden:

Abgerufen Sie alle aktiven obersten Ebene Auftrag Ausführungen werden:

    Get-AzureSqlJobExecution

Rufen Sie alle obersten Ebene Auftrag Ausführungen, einschließlich inaktive Aufträge Ausführungen ab:

    Get-AzureSqlJobExecution -IncludeInactive

Rufen Sie alle untergeordneten Position Ausführungen einer bereitgestellten Auftrag Ausführung-ID, einschließlich inaktive Aufträge Ausführungen ab:

    $parentJobExecutionId = "{Job Execution Id}"
    Get-AzureSqlJobExecution -AzureSqlJobExecution -JobExecutionId $parentJobExecutionId –IncludeInactive -IncludeChildren

Alle Auftrag Ausführungen erstellt einen Zeitplan abrufen / Kombination, einschließlich inaktive Aufträge der beruflichen Position:

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Get-AzureSqlJobExecution -JobName $jobName -ScheduleName $scheduleName -IncludeInactive

Abrufen von alle Aufträge eine angegebenen Shard Karte, einschließlich inaktive Aufträge verwendet:

    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $target = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName
    Get-AzureSqlJobExecution -TargetId $target.TargetId –IncludeInactive

Abrufen von alle Aufträge eine angegebene benutzerdefinierte Auflistung, einschließlich inaktive Aufträge verwendet:

    $customCollectionName = "{Custom Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    Get-AzureSqlJobExecution -TargetId $target.TargetId –IncludeInactive
 
Rufen Sie die Liste der Position Vorgang Ausführungen innerhalb einer bestimmten Position Ausführung:

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Write-Output $jobTaskExecutions 

Abrufen von Auftrag Ausführung Aufgabendetails:

Das folgende PowerShell-Skript kann verwendet werden, können Sie die Details für die Ausführung eines Auftrags Vorgang ist besonders hilfreich, wenn bei der Ausführung Debuggen anzeigen.

    $jobTaskExecutionId = "{Job Task Execution Id}"
    $jobTaskExecution = Get-AzureSqlJobTaskExecution -JobTaskExecutionId $jobTaskExecutionId
    Write-Output $jobTaskExecution

## <a name="retrieve-failures-within-job-task-executions"></a>Abrufen von Fehlern in Auftrag Vorgang Ausführungen

Das JobTaskExecution-Objekt enthält eine Eigenschaft für den Lebenszyklus der Aufgabe sowie eine Eigenschaft. Wenn die Ausführung eines Auftrags Vorgang fehlgeschlagen ist, wird die Eigenschaft Lebenszyklus zu *fehlgeschlagen* festgelegt werden und die Eigenschaft Nachricht wird der resultierende Ausnahme Nachricht und deren Stapel festgelegt werden. Wenn ein Auftrag nicht erfolgreich war, ist es wichtig, um die Details der Projektaufgaben anzuzeigen, die für einen bestimmten Auftrag nicht erfolgreich war.

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Foreach($jobTaskExecution in $jobTaskExecutions) 
        {
        if($jobTaskExecution.Lifecycle -ne 'Succeeded')
            {
            Write-Output $jobTaskExecution
            }
        }

## <a name="waiting-for-a-job-execution-to-complete"></a>Warten auf die Ausführung eines Auftrags ausführen

Das folgende PowerShell-Skript kann für die Durchführung einer Position Aufgabe warten verwendet werden:

    $jobExecutionId = "{Job Execution Id}"
    Wait-AzureSqlJobExecution -JobExecutionId $jobExecutionId 

## <a name="create-a-custom-execution-policy"></a>Erstellen einer Richtlinie für benutzerdefinierte Ausführung

Flexible Datenbankaufträge unterstützt das Erstellen von benutzerdefinierten Ausführungsrichtlinien, die beim Starten von Aufträge angewendet werden können.
  
Ausführungsrichtlinien lässt derzeit für definieren:

* Name: Bezeichner für die Richtlinie Datenausführungsverhinderung.
* Zeitlimit:-Gesamtzeit, bevor Sie ein Projekt durch flexible Datenbankaufträge abgebrochen wird.
* Ursprüngliche Wiederholungsintervalls: Intervall warten, bevor der erste "Wiederholen".
* Maximale Wiederholungsintervalls: Linienende der Intervalle "Wiederholen" zu verwenden.
* Wiederholen Sie Intervall Backoff zurück: Koeffizienten verwendet, um das nächste Intervall zwischen Wiederholungsversuche zu berechnen.  Die folgende Formel verwendet: (Initial Wiederholungsintervalls) * Math.pow ((Intervall Backoff Koeffizienten), (Anzahl der Wiederholungsversuche) – 2). 
* Maximale Versuche: Die maximale Anzahl von "Wiederholen" versucht innerhalb eines Auftrags durchzuführen.

Die Ausführung Standardrichtlinie verwendet die folgenden Werte:

* Name: Ausführung Standardrichtlinie
* Zeitlimit: 1 Woche
* Ursprüngliche Wiederholungsintervalls: 100 Millisekunden
* Maximale Wiederholungsintervalls: 30 Minuten
* Wiederholen Sie Intervall zurück: 2
* Maximale Versuche: 2.147.483.647

Erstellen Sie die gewünschten Ausführungsrichtlinie ein:

    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 10
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $executionPolicy = New-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $executionPolicy

### <a name="update-a-custom-execution-policy"></a>Aktualisieren einer benutzerdefinierten Ausführungsrichtlinie

Aktualisieren Sie die gewünschten Ausführungsrichtlinie zu aktualisieren:

    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 15
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $updatedExecutionPolicy = Set-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $updatedExecutionPolicy
 
## <a name="cancel-a-job"></a>Abbrechen eines Auftrags

Flexible Datenbankaufträge unterstützt Aufträge Absage Serviceanfragen.  Wenn flexible Datenbankaufträge eine Absage für ein Projekt, das aktuell ausgeführte erkennt, wird versucht, den Auftrag zu beenden.

Es gibt zwei verschiedene Arten, flexible Datenbankaufträge einen Abbruch ausführen können:

1. Abbrechen des derzeit ausgeführten Aufgaben: Wenn ein Abbruch erkannt wird, während ein Vorgangs derzeit ausgeführt wird, wird ein Abbruch versucht innerhalb der aktuell ausgeführte Aspekt des Vorgangs.  Beispiel: ist eine Abfrage mit langer aktuell durchgeführt werden, wenn ein Abbruch versucht wird, werden beim Versuch, die Abfrage abzubrechen.
2. Abbrechen Vorgang Wiederholungsversuche: Wenn ein Abbruch durch den Thread Steuerelement erkannt wird, bevor eine Aufgabe für die Ausführung gestartet wird, der Thread Steuerelement wird vermeiden, starten den Vorgang und die Anfrage deklarieren, als abgebrochen.

Wenn ein Auftrag Abbruch für ein Projekt übergeordneten angefordert wird, wird die Kündigung Anforderung für das übergeordnete Projekt sowie für alle untergeordneten Aufträgen beachtet werden.
 
Um eine Absage senden möchten, verwenden Sie das Cmdlet **AzureSqlJobExecution beenden** , und legen Sie den Parameter **JobExecutionId** .

    $jobExecutionId = "{Job Execution Id}"
    Stop-AzureSqlJobExecution -JobExecutionId $jobExecutionId

## <a name="delete-a-job-by-name-and-the-jobs-history"></a>Löschen eines Auftrags nach Name und Verlauf des Auftrags

Flexible Datenbankaufträge unterstützt asynchrone Löschvorgang der Aufträge an. Ein Projekt zum Löschen markiert werden kann und das System wird der Auftrag und alle zugehörigen Historie löschen, nachdem alle Auftrag Ausführungen für das Projekt abgeschlossen haben. Das System wird nicht automatisch aktiven Auftrag Ausführungen abgebrochen.  

Stattdessen muss beenden-AzureSqlJobExecution aufgerufen werden, um aktiven Auftrag Ausführungen abzubrechen.

Damit Auftrag Löschvorgang ausgelöst wird, verwenden Sie das Cmdlet **AzureSqlJob entfernen** , und setzen Sie den Parameter **JobName** .

    $jobName = "{Job Name}"
    Remove-AzureSqlJob -JobName $jobName
 
## <a name="create-a-custom-database-target"></a>Erstellen einer benutzerdefinierten Datenbank Ziel
Benutzerdefinierte Datenbank Ziele können in flexible Datenbank Aufträge definiert werden, die für die Ausführung direkt oder für die Aufnahme innerhalb einer Datenbankgruppe von benutzerdefinierten verwendet werden können. Da **flexible Datenbank Pools** sind noch nicht direkt unterstützt über die PowerShell-APIs, erstellen Sie einfach eine benutzerdefinierte Datenbank Ziel- und eine benutzerdefinierte Datenbank Websitesammlung Ziel das umfasst alle Datenbanken im Pool.

Legen Sie die folgenden Variablen zu wirken sich auf die gewünschte Datenbankinformationen aus:

    $databaseName = "{Database Name}"
    $databaseServerName = "{Server Name}"
    New-AzureSqlJobDatabaseTarget -DatabaseName $databaseName -ServerName $databaseServerName 

## <a name="create-a-custom-database-collection-target"></a>Erstellen einer benutzerdefinierten Datenbank Websitesammlung Ziel
Eine benutzerdefinierte Datenbank Websitesammlung Ziel kann zum Aktivieren der Ausführung in mehreren definierten Datenbank Zielen definiert werden. Nachdem Sie eine Datenbankgruppe erstellt wurde, können Datenbanken an die Zielwebsite benutzerdefinierte Auflistung verknüpft werden.

Legen Sie die folgenden Variablen in der gewünschten benutzerdefinierten Auflistung Zielkonfiguration entsprechen:

    $customCollectionName = "{Custom Database Collection Name}"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName 

### <a name="add-databases-to-a-custom-database-collection-target"></a>Hinzufügen von Inhaltsdatenbanken in einer benutzerdefinierten Datenbank Websitesammlung als Ziel

Datenbank Ziele können benutzerdefinierte Datenbank Websitesammlung Ziele zum Erstellen einer Gruppe von Datenbanken zugeordnet werden. Immer, wenn ein Auftrag, das Ziel eines benutzerdefinierten Datenbank Websitesammlung Ziel hat erstellt, wird es als Ziel die Datenbanken, die der Gruppe zugeordnet sind, zum Zeitpunkt der Ausführung erweitert werden.

Fügen Sie die gewünschte Datenbank auf eine bestimmte benutzerdefinierte hinzu:

    $serverName = "{Database Server Name}"
    $databaseName = "{Database Name}"
    $customCollectionName = "{Custom Database Collection Name}"
    Add-AzureSqlJobChildTarget -CustomCollectionName $customCollectionName -DatabaseName $databaseName -ServerName $databaseServerName 

#### <a name="review-the-databases-within-a-custom-database-collection-target"></a>Überprüfen Sie die Datenbanken in einem benutzerdefinierten Datenbank Websitesammlung Ziel

Verwenden Sie das Cmdlet " **Get-AzureSqlJobTarget** ", um die untergeordneten Datenbanken in einem benutzerdefinierten Datenbank Websitesammlung Ziel abzurufen. 
 
    $customCollectionName = "{Custom Database Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $childTargets = Get-AzureSqlJobTarget -ParentTargetId $target.TargetId
    Write-Output $childTargets

### <a name="create-a-job-to-execute-a-script-across-a-custom-database-collection-target"></a>Erstellen eines Auftrags zum Ausführen eines Skripts über eine benutzerdefinierte Datenbank Websitesammlung Ziel

Verwenden Sie das Cmdlet **Neu-AzureSqlJob** , zum Erstellen eines Auftrags für eine Gruppe von Datenbanken, die von einem benutzerdefinierten Datenbank Websitesammlung Ziel definiert. Flexible Datenbankaufträge werden erweitern Sie den Auftrag in mehreren untergeordneten Einzelvorgänge, die jeweils einer Datenbank entsprechen, das das Ziel des benutzerdefinierten Datenbank Websitesammlung zugeordnet und stellen Sie sicher, dass das Skript in jeder Datenbank ausgeführt wird. In diesem Fall ist es wichtig, dass Skripts Idempotent flexibel in Bezug auf Wiederholungsversuche werden müssen.

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job

## <a name="data-collection-across-databases"></a>Datensammlung in Datenbanken

**Flexible Datenbank Aufträge** unterstützt das Ausführen einer Abfrage über eine Gruppe von Datenbanken und sendet die Ergebnisse an eine bestimmte Datenbanktabelle. Die Tabelle kann nach dem Vorfall zum Anzeigen der Abfrageergebnisse von der jeweiligen Datenbank abgefragt werden. Dadurch wird eine asynchrone Methode zum Ausführen einer Abfrage über mehrere Datenbanken. Fehler Fällen wie eine der Datenbanken derzeit vorübergehend nicht verfügbar ist, werden automatisch über Wiederholungsversuche behandelt.

Die angegebene Zieltabelle wird, wenn er noch nicht das Schema der zurückgegebene Ergebnisgruppe entsprechen vorhanden ist, automatisch erstellt. Wenn die Ausführung eines Skripts mehrere Ergebnismengen zurückgibt, werden flexible Datenbank Aufträge nur der ersten Phase in die Zieltabelle bereitgestellten senden.

Das folgende PowerShell-Skript kann zur Ausführung eines Skripts Sammeln der zugehörigen Ergebnisse in einer angegebenen Tabelle verwendet werden. Dieses Skript wird davon ausgegangen, dass ein T-SQL-Skript die ein einzelnes Resultset gibt erstellt wurde und einer benutzerdefinierten Datenbank Websitesammlung Ziel erstellt wurde.

Legen Sie das gewünschte Skript, Anmeldeinformationen und Ausführungsziel wirken sich aus die folgenden:

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $executionCredentialName = "{Execution Credential Name}"
    $customCollectionName = "{Custom Collection Name}"
    $destinationCredentialName = "{Destination Credential Name}"
    $destinationServerName = "{Destination Server Name}"
    $destinationDatabaseName = "{Destination Database Name}"
    $destinationSchemaName = "{Destination Schema Name}"
    $destinationTableName = "{Destination Table Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName

### <a name="create-and-start-a-job-for-data-collection-scenarios"></a>Erstellen und Starten eines Auftrags für die Websitesammlung Datenszenarien
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $executionCredentialName -ContentName $scriptName -ResultSetDestinationServerName $destinationServerName -ResultSetDestinationDatabaseName $destinationDatabaseName -ResultSetDestinationSchemaName $destinationSchemaName -ResultSetDestinationTableName $destinationTableName -ResultSetDestinationCredentialName $destinationCredentialName -TargetId $target.TargetId
    Write-Output $job
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName
    Write-Output $jobExecution

## <a name="create-a-schedule-for-job-execution-using-a-job-trigger"></a>Erstellen Sie einen Zeitplan für die Ausführung des Auftrags mithilfe eines Triggers Position

Das folgende PowerShell-Skript kann zum Erstellen eines Zeitplans Zugriffsprobleme verwendet werden. Neu-AzureSqlJobSchedule unterstützt auch - DayInterval, - HourInterval, - MonthInterval, und WeekInterval - Parameter, aber dieses Skript verwendet ein Intervall einer Minute. Terminpläne, die nur einmal ausgeführt erstellt werden können von Übergabe - einmalige.

Erstellen eines neuen Projektplans an:

    $scheduleName = "Every one minute"
    $minuteInterval = 1
    $startTime = (Get-Date).ToUniversalTime()
    $schedule = New-AzureSqlJobSchedule -MinuteInterval $minuteInterval -ScheduleName $scheduleName -StartTime $startTime 
    Write-Output $schedule

### <a name="create-a-job-trigger-to-have-a-job-executed-on-a-time-schedule"></a>Erstellen eines Triggers Position, um eine Position auf einem Zeitplan ausgeführt haben

Ein Position Trigger kann definiert werden, um eine Position nach einem Zeitplan ausgeführt haben. Das folgende PowerShell-Skript kann zum Erstellen eines Triggers Auftrag verwendet werden.

Legen Sie die folgenden Variablen an der gewünschten Position und Zeitplan entsprechen:

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    $jobTrigger = New-AzureSqlJobTrigger -ScheduleName $scheduleName –JobName $jobName
    Write-Output $jobTrigger

### <a name="remove-a-scheduled-association-to-stop-job-from-executing-on-schedule"></a>Entfernen einer geplanten Zuordnung So beenden Sie die Position von Zeitplan ausgeführt

Wenn Zugriffsprobleme Ausführung des Auftrags durch einen Trigger Auftrag abbrechen möchten, kann der Trigger Auftrag entfernt werden. Entfernen eines Triggers Auftrag zum Beenden eines Auftrags nach einem Zeitplan mithilfe des Cmdlets **Entfernen-AzureSqlJobTrigger** ausgeführt wird.

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Remove-AzureSqlJobTrigger -ScheduleName $scheduleName -JobName $jobName





## <a name="import-elastic-database-query-results-to-excel"></a>Importieren von Abfrageergebnissen flexible Datenbank in Excel

 Sie können die Ergebnisse einer Abfrage in einer Excel-Datei importieren.

1. Starten Sie Excel 2013.
2.  Navigieren Sie zu dem Menüband **Daten** .
3.  Klicken Sie auf **Aus anderen Quellen** , und klicken Sie auf **Von SQL Server**.

    ![Importieren von Excel aus anderen Quellen][5]
4.  Geben Sie im **Datenverbindungs-Assistenten** die Server sowie die Anmeldeinformationen ein. Klicken Sie dann auf **Weiter**.
5.  Wählen Sie im Dialogfeld **Wählen Sie die Datenbank, die die Daten enthält**die **ElasticDBQuery** -Datenbank ein.
6.  Wählen Sie die Tabelle **Kunden** in der Listenansicht aus, und klicken Sie auf **Weiter**. Klicken Sie dann auf **Fertig stellen**.
7.  Wählen Sie im Datenformular **Importieren** , klicken Sie unter **Wählen Sie aus, wie Sie diese Daten in der Arbeitsmappe anzeigen möchten** **Tabelle** aus, und klicken Sie auf **OK**.

Alle Zeilen aus der **Kundentabelle, bei anderen mehrere Shards hinweg gehörende Kehrmatrix** Auffüllen der Excel-Tabelle.

## <a name="next-steps"></a>Nächste Schritte
Sie können jetzt die Funktionen von Excel Daten verwenden. Verwenden Sie die Verbindungszeichenfolge mit Ihren Servernamen, Datenbanknamen und die Anmeldeinformationen Verbindung Ihrer Integration-Tools BI und Daten in der Abfragedatenbank flexible ein. Stellen Sie sicher, dass SQL Server als Datenquelle für Ihr Tool unterstützt wird. Finden Sie unter flexible Abfragedatenbank und externen Tabellen genau wie jede andere SQL Server-Datenbank und SQL Server-Tabellen, die Sie mit dem Tool eine Verbindung herstellen möchten.

### <a name="cost"></a>Kosten
Es ist keine zusätzliche Kosten für die Verwendung der Funktion flexible Datenbank Abfragen. Zu diesem Zeitpunkt dieses Feature steht nur für Datenbanken Premium als Endpunkt, jedoch die mehrere Shards hinweg der alle Service-Stufe werden können.

Preisinformationen finden Sie unter [SQL-Datenbank Preise Details](https://azure.microsoft.com/pricing/details/sql-database/).


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-query-getting-started/cmd-prompt.png
[2]: ./media/sql-database-elastic-query-getting-started/portal.png
[3]: ./media/sql-database-elastic-query-getting-started/tiers.png
[4]: ./media/sql-database-elastic-query-getting-started/details.png
[5]: ./media/sql-database-elastic-query-getting-started/exel-sources.png
<!--anchors-->

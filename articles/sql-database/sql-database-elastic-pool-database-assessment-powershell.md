<properties
    pageTitle="PowerShell-Skript zum Identifizieren der einzelner Datenbanken für einen Pool geeignet | Microsoft Azure"
    description="Ein Ressourcenpool flexible Datenbank ist eine Auflistung von verfügbaren Ressourcen, die von einer Gruppe von flexible Datenbanken freigegeben wurden. Dieses Dokument enthält ein Powershell-Skript um Hilfe, ob der mit einer Datenbank flexible Ressourcenpool für eine Gruppe von Datenbanken geeignet bewerten."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="09/28/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>

# <a name="powershell-script-for-identifying-databases-suitable-for-an-elastic-database-pool"></a>PowerShell-Skript zum Identifizieren von Datenbanken für eine Datenbank flexible Ressourcenpool geeignet

Das PowerShell-Beispielskript in diesem Artikel schätzt die aggregate eDTU Werte für Benutzer-Datenbanken in einer SQL-Datenbankserver an. Das Skript sammelt Daten, während sie ausgeführt wird und für eine typische Herstellung Arbeitsbelastung, sollten Sie das Skript für mindestens einen Tag ausführen. Idealerweise sollten Sie das Skript für eine Dauer ausführen, die Ihre Datenbanken normalen arbeiten darstellt. Führen Sie das Skript so lange zu erfassen von Daten, die normalen und Spitzenzeiten Auslastung für die Datenbanken darstellt. Eine genauere Schätzung kann das Skript eine Woche oder sogar mehr ausgeführt wahrscheinlich erzielt werden.

Dieses Skript eignet sich für die Auswertung von Datenbanken auf Servern v11, für die Migration zu v12-Servern, wo Pools unterstützt werden. SQL-Datenbank wurde v12-Servern integrierten Intelligence, die analysiert werden zurückliegenden Verwendung und einem Ressourcenpool vorschlägt, wenn er kostengünstiger angezeigt wird. Informationen finden Sie unter [überwachen, verwalten und Größe einer Datenbank flexible Ressourcenpool](sql-database-elastic-pool-manage-portal.md)

> [AZURE.IMPORTANT] Lassen Sie das PowerShell-Fenster geöffnet, beim Ausführen des Skripts. Schließen Sie das PowerShell-Fenster nicht, bis Sie das Skript für den Zeitraum erforderlich ausgeführt haben. 

## <a name="prerequisites"></a>Erforderliche Komponenten 

Installieren Sie die folgenden vor Ausführung des Skripts:

- Die neuesten Azure PowerShell. Ausführliche Informationen finden Sie unter [Informationen zum Installieren und konfigurieren Azure PowerShell](../powershell-install-configure.md).
- Das [SQL Server 2014 Feature Pack](https://www.microsoft.com/download/details.aspx?id=42295).

## <a name="script-details"></a>Skriptdetails

Sie können das Skript von Ihrem lokalen Computer oder eines virtuellen Computers in der Cloud ausführen. Bei der Ausführung von Ihrem lokalen Computer können Sie Daten Ausgang Gebühren entstehen, da das Skript Herunterladen von Daten aus Ihrer Zieldatenbanken muss. Die nachstehende Abbildung zeigt Daten Lautstärke Abschätzung basierend auf der Anzahl der Zieldatenbanken und Dauer der Ausführung des Skripts aus. Transferkosten Azure-Daten finden Sie unter [Daten durchstellen Preise Details](https://azure.microsoft.com/pricing/details/data-transfers/).
       
 -     Eine Datenbank pro Stunde = 38 KB
 -     Eine Datenbank pro Tag = 900 KB
 -     Eine Datenbank pro Woche = 6 MB
 -     100 Datenbanken pro Tag = 90 MB
 -     500 Datenbanken pro Woche = 3 GB

Das Skript kompiliert nicht Informationen für die folgenden Datenbanken:

* Flexible Datenbanken (Datenbanken bereits in einem flexible Pool)
* Der Server master-Datenbank

Wenn Sie weitere Datenbanken aus dem Ziel-Server ausschließen müssen, ändern Sie das Skript, um Ihren Kriterien entsprechen. Standardmäßig.

Das Skript benötigt eine Ausgabedatenbank zum Zwischenspeichern von Daten für die Analyse. Sie können eine neue oder vorhandene Datenbank verwenden. Zwar für das Tool zum Ausführen nicht technisch erforderlich ist, sollten die Ausgabedatenbank in einem anderen Server zu vermeiden, das Ergebnis der Analyse beeinträchtigen. Die Leistungsstufe der Ausgabedatenbank sollten mindestens S0 oder höher. Beim Sammeln von Daten für viele Datenbanken über längere Zeit, sollten Sie das Aktualisieren der Ausgabedatenbank auf einer höheren Ebene der Leistung.

Das Skript benötigt Sie für die Verbindung zu dem Ziel-Server (die flexible Datenbank Ressourcenpool Candidate) mit einem vollständigen Servernamen und die Anmeldeinformationen <*DB-Name*>**. database.windows.net**. Das Skript unterstützt nicht mehr als einem Server nacheinander zu analysieren.

Nachdem Sie die Werte für den ersten Satz von Parametern abgesendet, werden Sie aufgefordert, melden Sie sich bei Ihrem Konto Azure werden soll. Dies ist für die Anmeldung an Ihr Ziel-Server nicht der Ausgabedatenbankserver.
    
Wenn Sie die folgenden Warnhinweise beim Ausführen des Skripts auftreten, können Sie diese ignorieren:

- Warnung: Das Cmdlet Switch-AzureMode wird nicht mehr unterstützt.
- Warnung: SQL Server-Dienst Informationen konnte nicht abgerufen werden. Fehler beim für die Verbindung zu WMI auf 'Microsoft.Azure.Commands.Sql.dll' mit dem folgenden Fehler: der RPC-Server ist nicht verfügbar.

Wenn das Skript abgeschlossen ist, gibt es die geschätzte Anzahl von eDTUs für einen Pool enthalten alle Candidate Datenbanken in der Ziel-Server erforderlich. Folgt eine geschätzte eDTU zum Erstellen und Konfigurieren der Ressourcenpool verwendet werden kann. Nachdem das Pool erstellt wird und Datenbanken, in dem Pool verschoben, überwachen Sie der Ressourcenpool eng ein paar Tage, und nehmen Sie Anpassungen an der Ressourcenpool eDTU Konfiguration nach Bedarf. Finden Sie unter [überwachen, verwalten und Größe ein Ressourcenpool flexible Datenbank](sql-database-elastic-pool-manage-portal.md).


    
```
param (
[Parameter(Mandatory=$true)][string]$AzureSubscriptionName, # Azure Subscription name - can be found on the Azure portal: https://portal.azure.com/
[Parameter(Mandatory=$true)][string]$ResourceGroupName, # Resource Group name - can be found on the Azure portal: https://portal.azure.com/
[Parameter(Mandatory=$true)][string]$servername, # full server name like "abcdefg.database.windows.net"
[Parameter(Mandatory=$true)][string]$username, # user name
[Parameter(Mandatory=$true)][string]$serverPassword, # password
[Parameter(Mandatory=$true)][string]$outputServerName, # metrics collection database for analysis. full server name like "zyxwvu.database.windows.net"
[Parameter(Mandatory=$true)][string]$outputdatabaseName,
[Parameter(Mandatory=$true)][string]$outputDBUsername,
[Parameter(Mandatory=$true)][string]$outputDBpassword,
[Parameter(Mandatory=$true)][int]$duration_minutes # How long to run. Recommend to run for the period of time when your typical workload is running. At least 10 mins.
)

Login-AzureRmAccount
Set-AzureRmContext -SubscriptionName $AzureSubscriptionName

$server = Get-AzureRmSqlServer -ServerName $servername.Split('.')[0] -ResourceGroupName $ResourceGroupName

# Check version/upgrade status of the server
$upgradestatus = Get-AzureRmSqlServerUpgrade -ServerName $servername.Split('.')[0] -ResourceGroupName $ResourceGroupName
$version = ""
if ([string]::IsNullOrWhiteSpace($server.ServerVersion)) 
{
$version = $upgradestatus.Status
}
else
{
$version = $server.ServerVersion
}

# For Elastic database pool candidates, we exclude master, and any databases that are already in a pool. You may add more databases to the excluded list below as needed
$ListOfDBs = Get-AzureRmSqlDatabase -ServerName $servername.Split('.')[0] -ResourceGroupName $ResourceGroupName | Where-Object {$_.DatabaseName -notin ("master") -and $_.CurrentServiceLevelObjectiveName -notin ("ElasticPool") -and $_.CurrentServiceObjectiveName -notin ("ElasticPool")}

$outputConnectionString = "Data Source=$outputServerName;Integrated Security=false;Initial Catalog=$outputdatabaseName;User Id=$outputDBUsername;Password=$outputDBpassword"
$destinationTableName = "resource_stats_output"

# Create a table in output database for metrics collection
$sql = "
IF  NOT EXISTS (SELECT * FROM sys.objects 
WHERE object_id = OBJECT_ID(N'$($destinationTableName)') AND type in (N'U'))

BEGIN
Create Table $($destinationTableName) (database_name varchar(128), slo varchar(20), end_time datetime, avg_cpu float, avg_io float, avg_log float, db_size float);
Create Clustered Index ci_endtime ON $($destinationTableName) (end_time);
END
TRUNCATE TABLE $($destinationTableName);
"
Invoke-Sqlcmd -ServerInstance $outputServerName -Database $outputdatabaseName -Username $outputDBUsername -Password $outputDBpassword -Query $sql -ConnectionTimeout 120 -QueryTimeout 120 

# waittime (minutes) is interval between data collection queries in the loop below.
$Waittime = 10
$end_Time = [DateTime]::UtcNow
$start_time = $end_time.AddMinutes(-$Waittime)
$finish_time = $end_Time.AddMinutes($duration_minutes)

While ($end_time -lt $finish_time)
{
Write-Host "Collecting metrics..." 
foreach ($db in $ListOfDBs)
{
if ($version -in ("12.0", "Completed")) # for V12 databases 
{
$sql = "Declare @dbname varchar(128) = '$($db.DatabaseName)';"
$sql += "Declare @SLO varchar(20) = '$($db.CurrentServiceObjectiveName)';"
$sql+= "
Declare @DTU_cap int, @db_size float;
Select @DTU_cap = CASE @SLO 
WHEN 'Basic' THEN 5
WHEN 'S0' THEN 10
WHEN 'S1' THEN 20
WHEN 'S2' THEN 50
WHEN 'S3' THEN 100
WHEN 'P1' THEN 125
WHEN 'P2' THEN 250
WHEN 'P4' THEN 500
WHEN 'P6' THEN 1000
WHEN 'P11' THEN 1750
WHEN 'P15' THEN 4000
ELSE 50 -- assume Web/Business DBs
END
SELECT @db_size = SUM(reserved_page_count) * 8.0/1024/1024 FROM sys.dm_db_partition_stats
SELECT @dbname as database_name, @SLO as SLO, dateadd(second, round(datediff(second, '2015-01-01', end_time) / 15.0, 0) * 15,'2015-01-01')
as end_time, avg_cpu_percent * (@DTU_cap/100.0) AS avg_cpu, avg_data_io_percent * (@DTU_cap/100.0) AS avg_io, avg_log_write_percent * (@DTU_cap/100.0) AS avg_log, @db_size as db_size FROM sys.dm_db_resource_stats
WHERE end_time > '$($start_time)' and end_time <= '$($end_time)';
" 
}
else
{
$sql = "Declare @dbname varchar(128) = '$($db.DatabaseName)';"
$sql += "Declare @SLO varchar(20) = '$($db.CurrentServiceObjectiveName)';"
$sql+= "
Declare @DTU_cap int, @db_size float;
Select @DTU_cap = CASE @SLO 
WHEN 'Basic' THEN 5
WHEN 'S0' THEN 10
WHEN 'S1' THEN 20
WHEN 'S2' THEN 50
WHEN 'P1' THEN 100
WHEN 'P2' THEN 200
WHEN 'P3' THEN 800
ELSE 50 -- assume Web/Business DBs
END
SELECT @db_size = SUM(reserved_page_count) * 8.0/1024/1024 from sys.dm_db_partition_stats
SELECT @dbname as database_name, @SLO as SLO, dateadd(second, round(datediff(second, '2015-01-01', end_time) / 15.0, 0) * 15,'2015-01-01')
as end_time, avg_cpu_percent * (@DTU_cap/100.0) AS avg_cpu, avg_data_io_percent * (@DTU_cap/100.0) AS avg_io, avg_log_write_percent * (@DTU_cap/100.0) AS avg_log, @db_size as db_size FROM sys.dm_db_resource_stats
WHERE end_time > '$($start_time)' and end_time <= '$($end_time)';
" 
}

$result = Invoke-Sqlcmd -ServerInstance $servername -Database $db.DatabaseName -Username $username -Password $serverPassword -Query $sql -ConnectionTimeout 240 -QueryTimeout 3600 
#bulk copy the metrics to output database
$bulkCopy = new-object ("Data.SqlClient.SqlBulkCopy") $outputConnectionString 
$bulkCopy.BulkCopyTimeout = 600
$bulkCopy.DestinationTableName = "$destinationTableName";
$bulkCopy.WriteToServer($result);

}

$start_time = $start_time.AddMinutes($Waittime)
$end_time = $end_time.AddMinutes($Waittime)
Write-Host $start_time
Write-Host $end_time
do {
Start-Sleep 1
   }
until (([DateTime]::UtcNow) -ge $end_time)
}

Write-Host "Analyzing the collected metrics...."
# Analysis query that does aggregation of the resource metrics to calculate pool size.
$sql1 = 'Declare @DTU_Perf_99 as float, @DTU_Storage as float;
WITH group_stats AS
(
SELECT end_time, SUM(db_size) AS avg_group_Storage, SUM(avg_cpu) AS avg_group_cpu, SUM(avg_io) AS avg_group_io,SUM(avg_log) AS avg_group_log
FROM resource_stats_output 
WHERE slo LIKE '

$sql2 = '
GROUP BY end_time
)
-- calculate aggregate storage and DTUs for all DBs in the group
, group_DTU AS
(
SELECT end_time, avg_group_Storage, 
(SELECT Max(v)
   FROM (VALUES (avg_group_cpu), (avg_group_log), (avg_group_io)) AS value(v)) AS avg_group_DTU
FROM group_stats
)
-- Get top 1 percent of the storage and DTU utilization samples.
, top1_percent AS (
SELECT TOP 1 PERCENT avg_group_Storage, avg_group_dtu FROM group_dtu ORDER BY [avg_group_DTU] DESC
)

-- Max and 99th percentile DTU for the given list of databases if converted into an elastic pool. Storage is increased by factor of 1.25 to accommodate for future growth. Currently storage limit of the pool is determined by the amount of DTUs based on 1GB/DTU.
--SELECT MAX(avg_group_Storage)*1.25/1024.0 AS Group_Storage_DTU, MAX(avg_group_dtu) AS Group_Performance_DTU, MIN(avg_group_dtu) AS Group_Performance_DTU_99th_percentile FROM top1_percent;
SELECT @DTU_Storage = MAX(avg_group_Storage)*1.25/1024.0, @DTU_Perf_99 = MIN(avg_group_dtu) FROM top1_percent;
IF @DTU_Storage > @DTU_Perf_99 
SELECT ''Total number of DTUs dominated by storage: '' + convert(varchar(100), @DTU_Storage)
ELSE 
SELECT ''Total number of DTUs dominated by resource consumption: '' + convert(varchar(100), @DTU_Perf_99)'

#check if there are any web/biz edition dbs in the collected metrics
$checkslo = "SELECT TOP 1 slo FROM resource_stats_output WHERE slo LIKE 'shared%'"
$output = Invoke-Sqlcmd -ServerInstance $outputServerName -Database $outputdatabaseName -Username $outputDBUsername -Password $outputDBpassword -Query $checkslo -QueryTimeout 3600 | select -expand slo
if ($output -like "Shared*")
{
write-host "`nWeb/Business edition:" -BackgroundColor Green -ForegroundColor Black
$sql = $sql1 + "'Shared%'"  + $sql2
$data = Invoke-Sqlcmd -ServerInstance $outputServerName -Database $outputdatabaseName -Username $outputDBUsername -Password $outputDBpassword -Query $sql -QueryTimeout 3600
$data | %{'{0}' -f $_[0]}
}

#check if there are any basic edition dbs in the collected metrics
$checkslo = "SELECT TOP 1 slo FROM resource_stats_output WHERE slo LIKE 'Basic%'"
$output = Invoke-Sqlcmd -ServerInstance $outputServerName -Database $outputdatabaseName -Username $outputDBUsername -Password $outputDBpassword -Query $checkslo -QueryTimeout 3600 | select -expand slo
if ($output -like "Basic*")
{
write-host "`nBasic edition:" -BackgroundColor Green -ForegroundColor Black
$sql = $sql1 + "'Basic%'"  + $sql2
$data = Invoke-Sqlcmd -ServerInstance $outputServerName -Database $outputdatabaseName -Username $outputDBUsername -Password $outputDBpassword -Query $sql -QueryTimeout 3600
$data | %{'{0}' -f $_[0]} 
}

#check if there are any standard edition dbs in the collected metrics
$checkslo = "SELECT TOP 1 slo FROM resource_stats_output WHERE slo LIKE 'S%' AND slo NOT LIKE 'Shared%'"
$output = Invoke-Sqlcmd -ServerInstance $outputServerName -Database $outputdatabaseName -Username $outputDBUsername -Password $outputDBpassword -Query $checkslo -QueryTimeout 3600 | select -expand slo
if ($output -like "S*")
{
write-host "`nStandard edition:" -BackgroundColor Green -ForegroundColor Black
$sql = $sql1 + "'S%' AND slo NOT LIKE 'Shared%'"  + $sql2
$data = Invoke-Sqlcmd -ServerInstance $outputServerName -Database $outputdatabaseName -Username $outputDBUsername -Password $outputDBpassword -Query $sql -QueryTimeout 3600
$data | %{'{0}' -f $_[0]}
}

#check if there are any premium edition dbs in the collected metrics
$checkslo = "SELECT TOP 1 slo FROM resource_stats_output WHERE slo LIKE 'P%'"
$output = Invoke-Sqlcmd -ServerInstance $outputServerName -Database $outputdatabaseName -Username $outputDBUsername -Password $outputDBpassword -Query $checkslo -QueryTimeout 3600 | select -expand slo
if ($output -like "P*")
{
write-host "`nPremium edition:" -BackgroundColor Green -ForegroundColor Black
$sql = $sql1 + "'P%'"  + $sql2
$data = Invoke-Sqlcmd -ServerInstance $outputServerName -Database $outputdatabaseName -Username $outputDBUsername -Password $outputDBpassword -Query $sql -QueryTimeout 3600
$data | %{'{0}' -f $_[0]}
}
```
        


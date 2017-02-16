<properties
    pageTitle="Erstellen und Konfigurieren eines Log Analytics-Arbeitsbereichs mithilfe von PowerShell | Microsoft Azure"
    description="Melden Sie sich bei Analytics verwendet Daten in Ihrer lokalen Servern oder cloud-Infrastruktur. Sie können Computerdaten aus Azure-Speicher Wenn von Azure Diagnose generiert sammeln."
    services="log-analytics"
    documentationCenter=""
    authors="richrundmsft"
    manager="jochan"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="powershell"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="richrund"/>

# <a name="manage-log-analytics-using-powershell"></a>Verwalten Sie Log Analytics mithilfe der PowerShell

Sie können die [Log Analytics PowerShell-Cmdlets] (https://msdn.microsoft.com/library/mt188224(v=azure.300\).aspx) verschiedene Funktionen in Log Analytics über die Befehlszeile oder als Teil eines Skripts auszuführen.  Beispiele für die Aufgaben, die Sie mit PowerShell ausführen können:

+ Erstellen eines Arbeitsbereichs
+ Hinzufügen oder Entfernen einer Lösung
+ Importieren und Exportieren von gespeicherten Suchen
+ Erstellen einer Computergruppe
+ Aktivieren der Sammlung von IIS-Protokollen aus Computern mit der Windows-Agent installiert
+ Sammeln von Datenquellen von Linux und Windows-Computern
+ Sammeln von Ereignissen von Syslog auf Linux-Computern 
+ Sammeln von Ereignissen aus Windows-Ereignisprotokollen
+ Sammeln von benutzerdefinierten Ereignisprotokollen
+ Hinzufügen von des Analytics-Agents zu einer Azure-virtuellen Computern
+ Konfigurieren der Log Analytics zu erfassten Indexdaten werden mit der Azure-Diagnose


Dieser Artikel enthält zwei Codebeispiele für die einige der Funktionen, die von PowerShell ausgeführt werden können.  Sie können auf den [Log Analytics PowerShell-Cmdlet Bezug] verweisen (https://msdn.microsoft.com/library/mt188224(v=azure.300\).aspx) für andere Funktionen.

> [AZURE.NOTE] Log Analytics wurde bereits Betrieb Einblicken, aufgerufen, weshalb sie den Namen in die Cmdlets verwendet wird.

## <a name="prerequisites"></a>Erforderliche Komponenten

Um mit dem Arbeitsbereich Log Analytics PowerShell verwenden möchten, benötigen Sie Folgendes:

+ Ein Azure-Abonnement und 
+ Ihre Azure Log Analytics-Arbeitsbereich zu Ihrem Azure-Abonnement verknüpft.

Wenn einen OMS Workspace erstellt haben, aber es noch nicht es zu einem Azure-Abonnement verknüpft ist können Sie den Link erstellen:

+ Azure-Portal
+ Im Portal OMS oder 
+ Verwenden die Cmdlets Get-AzureRmOperationalInsightsLinkTargets und neu-AzureRmOperationalInsightsWorkspace.


## <a name="create-and-configure-a-log-analytics-workspace"></a>Erstellen und Konfigurieren einer Log Analytics-Arbeitsbereich

Das folgende Skript veranschaulicht so:

1.  Erstellen eines Arbeitsbereichs
2.  Liste der verfügbaren Lösungen
3.  Hinzufügen von Lösungen zu dem Arbeitsbereich
4.  Importieren von gespeicherten Suchvorgänge
5.  Exportieren einer gespeicherten Suchvorgänge
6.  Erstellen einer Computergruppe
7.  Aktivieren der Sammlung von IIS-Protokollen aus Computern mit der Windows-Agent installiert
8.  Logische Datenträger Leitungsindikatoren von Linux Computern sammeln (% Knoten verwendet; MB frei; % Verwendeter Speicherplatz; Datenträger/s; Datenträger/s; Schreibt/s)
9.  Sammeln Sie Syslog-Ereignis von Linux-Computern
10. Sammeln Sie Fehler und Warnung Ereignisse von Ereignisprotokoll der Anwendung von Windows-Computern
11. Sammeln Sie Verfügbare MB Arbeitsspeicher Performance-Zähler von Windows-Computern
12. Sammeln Sie ein benutzerdefiniertes Protokoll 


```

$ResourceGroup = "oms-example"
$WorkspaceName = "log-analytics-" + (Get-Random -Maximum 99999) # workspace names need to be unique - Get-Random helps with this for the example code
$Location = "westeurope"

# List of solutions to enable
$Solutions = "Security", "Updates", "SQLAssessment"

# Saved Searches to import
$ExportedSearches = @"
[
    {
        "Category":  "My Saved Searches",
        "DisplayName":  "WAD Events (All)",
        "Query":  "Type=Event SourceSystem:AzureStorage ",
        "Version":  1
    },
    {        
        "Category":  "My Saved Searches",
        "DisplayName":  "Current Disk Queue Length",
        "Query":  "Type=Perf ObjectName=LogicalDisk InstanceName=\"C:\" CounterName=\"Current Disk Queue Length\"",
        "Version":  1
    }
]
"@ | ConvertFrom-Json

# Custom Log to collect
$CustomLog = @"
{
    "customLogName": "sampleCustomLog1", 
    "description": "Example custom log datasource", 
    "inputs": [
        { 
            "location": { 
            "fileSystemLocations": { 
                "windowsFileTypeLogPaths": [ "e:\\iis5\\*.log" ], 
                "linuxFileTypeLogPaths": [ "/var/logs" ] 
                } 
            }, 
        "recordDelimiter": { 
            "regexDelimiter": { 
                "pattern": "\\n", 
                "matchIndex": 0, 
                "matchIndexSpecified": true, 
                "numberedGroup": null 
                } 
            } 
        }
    ], 
    "extractions": [
        { 
            "extractionName": "TimeGenerated", 
            "extractionType": "DateTime", 
            "extractionProperties": { 
                "dateTimeExtraction": { 
                    "regex": null, 
                    "joinStringRegex": null 
                    } 
                } 
            }
        ] 
    }
"@

# Create the resource group if needed
try {
    Get-AzureRmResourceGroup -Name $ResourceGroup -ErrorAction Stop
} catch {
    New-AzureRmResourceGroup -Name $ResourceGroup -Location $Location
}

# Create the workspace
New-AzureRmOperationalInsightsWorkspace -Location $Location -Name $WorkspaceName -Sku Standard -ResourceGroupName $ResourceGroup

# List all solutions and their installation status
Get-AzureRmOperationalInsightsIntelligencePacks -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Add solutions
foreach ($solution in $Solutions) {
    Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -IntelligencePackName $solution -Enabled $true
}

#List enabled solutions
(Get-AzureRmOperationalInsightsIntelligencePacks -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName).Where({($_.enabled -eq $true)})

# Import Saved Searches
foreach ($search in $ExportedSearches) {
    $id = $search.Category + "|" + $search.DisplayName
    New-AzureRmOperationalInsightsSavedSearch -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -SavedSearchId $id -DisplayName $search.DisplayName -Category $search.Category -Query $search.Query -Version $search.Version
}

# Export Saved Searches
(Get-AzureRmOperationalInsightsSavedSearch -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName).Value.Properties | ConvertTo-Json 

# Create Computer Group
New-AzureRmOperationalInsightsComputerGroup -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -SavedSearchId "My Web Servers" -DisplayName "Web Servers" -Category "My Saved Searches" -Query "Computer=""web*"" | distinct Computer" -Version 1

# Enable IIS Log Collection using agent
Enable-AzureRmOperationalInsightsIISLogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Linux Perf
New-AzureRmOperationalInsightsLinuxPerformanceObjectDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -ObjectName "Logical Disk" -InstanceName "*"  -CounterNames @("% Used Inodes", "Free Megabytes", "% Used Space", "Disk Transfers/sec", "Disk Reads/sec", "Disk Reads/sec", "Disk Writes/sec") -IntervalSeconds 20  -Name "Example Linux Disk Performance Counters"
Enable-AzureRmOperationalInsightsLinuxCustomLogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Linux Syslog
New-AzureRmOperationalInsightsLinuxSyslogDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -Facility "kern" -CollectEmergency -CollectAlert -CollectCritical -CollectError -CollectWarning -Name "Example kernal syslog collection"
Enable-AzureRmOperationalInsightsLinuxSyslogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Windows Event
New-AzureRmOperationalInsightsWindowsEventDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -EventLogName "Application" -CollectErrors -CollectWarnings -Name "Example Application Event Log"

# Windows Perf
New-AzureRmOperationalInsightsWindowsPerformanceCounterDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -ObjectName "Memory" -InstanceName "*" -CounterName "Available MBytes" -IntervalSeconds 20 -Name "Example Windows Performance Counter"

# Custom Logs
New-AzureRmOperationalInsightsCustomLogDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -CustomLogRawJson "$CustomLog" -Name "Example Custom Log Collection"

```

## <a name="configuring-log-analytics-to-index-azure-diagnostics"></a>Konfigurieren von Log Analytics Azure Diagnose indizieren 

Für Agents Azure Ressourcen, müssen die Ressourcen Azure Diagnose aktiviert und Schreiben in einem Speicherkonto konfiguriert haben. Log Analytics können dann sammeln Sie die Protokolle aus dem Speicherkonto konfiguriert werden. Ressourcen, die Sie müssen die vorherige Konfiguration für umfasst:

+ Klassische Cloud Services (Web und Worker Rollen)
+ Reaktionsgruppendienst Fabric Cluster
+ Netzwerk-Sicherheitsgruppen
+ Wichtiger Depots und 
+ Anwendungsgateways

PowerShell können Sie auch einen Protokoll Analytics-Arbeitsbereich ein Azure-Abonnement zum Sammeln von Protokolle von verschiedenen Azure-Abonnements konfigurieren.

Das folgende Beispiel zeigt so:

1.  Liste der vorhandenen Speicherkonten und Orte, denen Daten aus der Log Analytics index daher
2.  Erstellen Sie eine Konfiguration zum Lesen von einem Speicherkonto
3.  Aktualisieren der neu erstellten Konfigurations zum Indizieren von Daten aus zusätzlichen Speicherorte
4.  Löschen der neu erstellten Konfigurations

```
# validTables = "WADWindowsEventLogsTable", "LinuxsyslogVer2v0", "WADServiceFabric*EventTable", "WADETWEventTable" 
$workspace = (Get-AzureRmOperationalInsightsWorkspace).Where({$_.Name -eq "your workspace name"})

# Update these two lines with the storage account resource ID and the storage account key for the storage account you want to Log Analytics to  
$storageId = "/subscriptions/ec11ca60-1234-491e-5678-0ea07feae25c/resourceGroups/demo/providers/Microsoft.Storage/storageAccounts/wadv2storage"
$key = "abcd=="

# List existing insights
Get-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name

# Create a new insight
New-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" -StorageAccountResourceId $storageId -StorageAccountKey $key -Tables @("WADWindowsEventLogsTable") -Containers @("wad-iis-logfiles")

# Update existing insight
Set-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" -Tables @("WADWindowsEventLogsTable", "WADETWEventTable") -Containers @("wad-iis-logfiles", "insights-logs-networksecuritygroupevent/resourceId=/SUBSCRIPTIONS/ec11ca60-1234-491e-5678-0ea07feae25c/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/DEMO")

# Remove the insight
Remove-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" 

```

## <a name="next-steps"></a>Nächste Schritte

- [Überprüfen Log Analytics PowerShell-Cmdlets](http://msdn.microsoft.com/library/mt188224.aspx) für Weitere Informationen zur Verwendung von PowerShell für die Konfiguration von Protokoll Analytics.


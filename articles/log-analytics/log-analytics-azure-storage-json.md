<properties
    pageTitle="Analysieren Azure Diagnoseprotokolle mit Log Analytics | Microsoft Azure"
    description="Log Analytics können die Protokolle aus Azure Services gelesen werden, die Azure Diagnoseprotokolle BLOB-Speicher im JSON-Format zu schreiben."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>


# <a name="analyze-azure-diagnostic-logs-using-log-analytics"></a>Analysieren von Azure Diagnoseprotokolle Analytics Protokoll verwenden

Log Analytics können die Protokolle für die folgenden Azure Dienste sammeln, in die [Diagnoseprotokolle Azure](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) BLOB-Speicher im JSON-Format schreiben:

+ Automatisierung (Preview)
+ Key Tresor (Preview)
+ Application Gateway (Preview)
+ Netzwerk-Sicherheitsgruppe (Preview)

In den folgenden Abschnitten, die Sie mithilfe der PowerShell zu durchgehen:

+ Konfigurieren der Log Analytics zum Sammeln die Protokolle von Speicherplatz für jede Ressource  
+ Aktivieren Sie die Log Analytics-Lösung für den Azure-Dienst

Bevor Log Analytics Sammeln von Daten für diese Ressourcen können, muss Azure Diagnose aktiviert sein. Verwenden der `Set-AzureRmDiagnosticSetting` -Cmdlet zum Aktivieren der Protokollierung.

Finden Sie in den folgenden Artikeln für Weitere Informationen zum Aktivieren der diagnoseprotokollierung:

+ [Key Tresor](../key-vault/key-vault-logging.md)
+ [Application Gateway](../application-gateway/application-gateway-diagnostics.md)
+ [Netzwerk-Sicherheitsgruppe](../virtual-network/virtual-network-nsg-manage-log.md)

Diese Dokumentation enthält auch Details auf:

+ Problembehandlung bei Datensammlung
+ Beenden der Sammlung von Daten

## <a name="configure-log-analytics-to-collect-azure-diagnostic-logs"></a>Konfigurieren der Log Analytics Azure Diagnoseprotokolle sammeln

Erfassung von Protokollen für diese Dienste und Aktivieren der Lösung visualisiert werden sollen, die Protokolle wird mit PowerShell-Skripts ausgeführt.

Im folgenden Beispiel ermöglicht-Befehl alle unterstützten Ressourcen

```
# update to be the storage account that logs will be written to. Storage account must be in the same region as the resource to monitor
# format is similar to "/subscriptions/ec11ca60-ab12-345e-678d-0ea07bbae25c/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount"
$storageAccountId = ""

$supportedResourceTypes = ("Microsoft.Automation/AutomationAccounts", "Microsoft.KeyVault/Vaults", "Microsoft.Network/NetworkSecurityGroups", "Microsoft.Network/ApplicationGateways")

# update location to match your storage account location
$resources = Get-AzureRmResource | where { $_.ResourceType -in $supportedResourceTypes -and $_.Location -eq "westus" }

foreach ($resource in $resources) {
    Set-AzureRmDiagnosticSetting -ResourceId $resource.ResourceId -StorageAccountId $storageAccountId -Enabled $true -RetentionEnabled $true -RetentionInDays 1
}
```


Für einige Ressourcen ist es möglich, die vorherige Konfiguration vom Azure-Portal mithilfe der in [Übersicht der Azure Diagnoseprotokolle](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-diagnostic-logs)beschriebenen Schritte ausführen.

## <a name="configure-log-analytics-to-collect-azure-diagnostic-logs"></a>Konfigurieren der Log Analytics Azure Diagnoseprotokolle sammeln

Wir haben ein PowerShell-Skript-Modul bereitgestellt, die exportiert zwei Cmdlets zum Konfigurieren der Log Analytics unterstützen:

1. `Add-AzureDiagnosticsToLogAnalyticsUI`fordert Sie zur Eingabe und kann einfache Konfigurationen einrichten
2. `Add-AzureDiagnosticsToLogAnalytics`Schaltet Ressourcen zu überwachen als Eingabe, und klicken Sie dann konfiguriert Log Analytics  

### <a name="pre-requisites"></a>Erforderliche Komponenten

1. Azure PowerShell mit Version 1.0.8 oder höher Betrieb Einsichten Cmdlets.
  - [So installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md)
  - Überprüfen Sie die Version des Cmdlets aus:`Import-Module AzureRM.OperationalInsights -MinimumVersion 1.0.8 `
2. Diagnoseprotokolle ist für die Ressource Azure konfiguriert, die Sie überwachen möchten. Verwenden Sie `Set-AzureRmDiagnosticSetting` oder finden Sie unter [Verwenden Log Analytics zum Sammeln von Daten aus Azure-Speicherkonten](log-analytics-azure-storage.md) für Diagnose aktivieren.
3. Ein [Protokoll Analytics](https://portal.azure.com/#create/Microsoft.LogAnalyticsOMS) -Arbeitsbereich  
4. Das AzureDiagnosticsAndLogAnalytics PowerShell-Modul
  - Herunterladen des Moduls [AzureDiagnosticsAndLogAnalytics](https://www.powershellgallery.com/packages/AzureDiagnosticsAndLogAnalytics/) aus PowerShell-Katalog

### <a name="option-1-run-the-interactive-configuration-scripts"></a>Option 1: Führen Sie die Konfigurationsskripts interaktive

PowerShell öffnen und ausführen:

```
# Connect to Azure
Login-AzureRmAccount
# If you have diagnostics logs being written to classic storage you will also need to run
Add-AzureAccount

# Import the module
Install-Module -Name AzureDiagnosticsAndLogAnalytics

# Run the UI configuration script
Add-AzureDiagnosticsToLogAnalyticsUI

```

Sie sind eine Liste der verfügbaren Auswahlmöglichkeiten angezeigt und angegebenen dazu aufgefordert werden, Ihre Auswahl zu treffen.
Sie werden aufgefordert, eine Auswahl für jede der folgenden Aktionen aus:

+ Ressourcentypen (Azure Services) zum Sammeln von Protokollen
+ Ressourceninstanzen zum Sammeln von Protokollen
+ Melden Sie sich Analytics-Arbeitsbereich, um die Daten zu sammeln

Dieses Skript ausgeführt haben, sollten Sie Datensätze in Log Analytics sehen etwa 30 Minuten nach neue diagnostische Daten in Speicher geschrieben werden. Wenn keine Datensätze verfügbar sind, nachdem Sie diesmal finden Sie zur Problembehandlung im Abschnitt unten.

### <a name="option-2-build-a-list-of-resources-and-pass-them-to-the-configuration-cmdlet"></a>Option 2: Erstellen einer Liste mit Ressourcen und an das Cmdlet Konfiguration übergeben

Sie können eine Liste der Ressourcen erstellen, die Azure Diagnose aktiviert haben und dann die Ressourcen an der Konfiguration Cmdlet übergeben.

Finden Sie weitere Informationen über das Cmdlet, durch Ausführen `Get-Help Add-AzureDiagnosticsToLogAnalytics`.


Suchen Sie weitere Details auf OMS [Log Analytics PowerShell-Cmdlets](https://msdn.microsoft.com/library/mt188224.aspx)

>[AZURE.NOTE] Wechseln Sie Ressourcen- und Arbeitsbereich in verschiedenen Azure-Abonnements Wenn, zwischen ihnen je nach verwenden Bedarf`Select-AzureRmSubscription -SubscriptionId <Subscription the resource is in>`

```
# Connect to Azure
Login-AzureRmAccount
# If you have diagnostics logs being written to classic storage you will also need to run
Add-AzureAccount

# Import the module
Install-Module -Name AzureDiagnosticsAndLogAnalytics

# Update the values below with the name of the resource that has Azure diagnostics enabled and the name of your Log Analytics workspace
$resourceName = "***example-resource***"
$workspaceName = "***log-analytics-workspace***"

# Find the resource to monitor:
$resource = Find-AzureRmResource -ResourceType "Microsoft.KeyVault/Vaults" -ResourceNameContains $resourceName

# Find the Log Analytics workspace to configure, for example:
$workspace = Find-AzureRmResource -ResourceType "Microsoft.OperationalInsights/workspaces" -ResourceNameContains $workspaceName

# Perform configuration
Add-AzureDiagnosticsToLogAnalytics $resource $workspace

# Enable the Log Analytics solution
Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -intelligencepackname KeyVault -Enabled $true

```
Dieses Skript ausgeführt haben, sollten Sie Datensätze in Log Analytics sehen etwa 30 Minuten nach neue diagnostische Daten in Speicher geschrieben werden. Wenn keine Datensätze verfügbar sind, nachdem Sie diesmal finden Sie zur Problembehandlung im Abschnitt unten.  

>[AZURE.NOTE] Die Konfiguration ist nicht im Portal Azure sichtbar. Sie können überprüfen, mit der Konfiguration der `Get-AzureRmOperationalInsightsStorageInsight` Cmdlet.  


## <a name="stopping-log-analytics-from-collecting-azure-diagnostic-logs"></a>Beenden der Log Analytics aus Erfassen von Azure Diagnoseprotokolle

Verwenden Sie zum Löschen der Log Analytics-Konfigurations für eine Ressource der `Remove-AzureRmOperationalInsightsStorageInsight` Cmdlet.

## <a name="troubleshooting-configuration-for-azure-diagnostic-logs"></a>Problembehandlung bei der Konfiguration für Azure Diagnoseprotokolle

*Problem*

Beim Konfigurieren einer Ressource, die zum klassischen Speicher Protokollierung ist, wird der folgende Fehler angezeigt:

```
Select-AzureSubscription : The subscription id 7691b0d1-e786-4757-857c-7360e61896c3 doesn't exist.

Parameter name: id
```

*Auflösung*

Melden Sie sich bei der API für das Bereitstellungsmodell klassischen mit`Add-AzureAccount`

## <a name="troubleshooting-data-collection-for-azure-diagnostic-logs"></a>Problembehandlung bei Datensammlung für Azure Diagnoseprotokolle

Wenn die Daten für die Ressource ein Azure in Log Analytics nicht angezeigt werden, können Sie die folgenden Schritte zur Problembehandlung:

+ Vergewissern Sie sich mit dem Speicherkonto Datenfluss
+ Stellen Sie sicher, dass die Log Analytics-Lösung für den Dienst aktiviert ist
+ Stellen Sie sicher, dass aus dem Speicher gelesen Log Analytics konfiguriert ist
+ Aktualisieren Sie den Speicher kontoschlüssel

### <a name="verify-data-is-flowing-to-the-storage-account"></a>Stellen Sie sicher, dass die Daten mit dem Speicherkonto entdeckt wird

Sie können prüfen, ob die Daten im Speicher-Konto mit einem Tool, mit dem Azure-Speicher (beispielsweise Visual Studio) durchsuchen kann oder mithilfe der PowerShell fließt.

Um das Konto Speicher finden ist die Ressource so konfiguriert, dass log aus, um die folgenden PowerShell verwenden möchten:

`Find-AzureRmResource -ResourceType "Microsoft.KeyVault/Vaults" | select ResourceId | Get-AzureRmDiagnosticSetting `

Der Speichercontainer untersuchten Azure-Diagnose weist einen Namen mit einem Format von:  

`insights-logs-<Category>`

Finden Sie unter [Speicher Cmdlets überprüfen ein Containers für zuletzt verwendete Daten verwenden](../storage/storage-powershell-guide-full.md) , um weitere Informationen zum Anzeigen des Inhalts der Firma Speicher.

### <a name="verify-the-log-analytics-solution-for-the-service-is-enabled"></a>Stellen Sie sicher, dass die Log Analytics-Lösung für den Dienst aktiviert ist

Wenn Sie verwenden `Add-AzureDiagnosticsToLogAnalyticsUI`, die richtige Log Analytics-Lösung automatisch für Sie aktiviert ist.

Um zu überprüfen, ob eine Lösung aktiviert ist, führen Sie die folgenden PowerShell aus:

`Get-AzureRmOperationalInsightsIntelligencePacks -ResourceGroupName $logAnalyticsResourceGroup -WorkspaceName $logAnalyticsWorkspaceName`

Wenn die Lösung nicht aktiviert ist, können Sie es mithilfe der folgenden PowerShell aktivieren:

`Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName $logAnalyticsResourceGroup -WorkspaceName $logAnalyticsWorkspaceName -IntelligencePackName $solution -Enabled $true`

Um den Namen der Lösung für jeden Ressourcentyp aktivieren ermitteln möchten, verwenden Sie die folgende PowerShell (diese Variable ist verfügbar, nachdem Sie das Modul importiert haben):

`$MonitorableResourcesToOMSSolutions`

### <a name="verify-that-log-analytics-is-configured-to-read-from-storage"></a>Stellen Sie sicher, dass aus dem Speicher gelesen Log Analytics konfiguriert ist

Wenn Sie zusätzliche Azure Ressourcen hinzufügen, müssen Sie das Diagnoseprotokoll für diese aktivieren und Konfigurieren von Log Analytics für diese.
Um zu überprüfen, welche Ressourcen und Speicherkonten, Protokolle für sammeln Log Analytics konfiguriert ist, verwenden Sie die folgende PowerShell aus:

```
# Find the Workspace ResourceGroup and Name
$logAnalyticsWorkspace = Get-AzureRmOperationalInsightsWorkspace

#Get the configuration for all resources:
Get-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $logAnalyticsWorkspace.ResourceGroupName -WorkspaceName $logAnalyticsWorkspace.Name

#Get the just the resources configured:
Get-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $logAnalyticsWorkspace.ResourceGroupName -WorkspaceName $logAnalyticsWorkspace.Name | select Containers
```

### <a name="update-the-storage-key"></a>Aktualisieren Sie den Speicherschlüssel

Wenn Sie die Taste für das Speicherkonto ändern, muss die Konfiguration Log Analytics auch mit dem neuen Schlüssel aktualisiert werden.
Sie können die Log Analytics-Konfiguration mit einem neuen Schlüssel mithilfe der folgenden PowerShell aktualisieren:

`Set-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $logAnalyticsWorkspace.ResourceGroupName -WorkspaceName $logAnalyticsWorkspace.Name –Name <Storage Insight Name> -StorageAccountKey $newKey `

Um den Namen der Speicher Einblicke zu suchen, verwenden Sie die `Get-AzureRmOperationalInsightsStorageInsight` Cmdlet, wie in den vorherigen Beispielen dargestellt.

## <a name="next-steps"></a>Nächste Schritte

- [Blob-Speicher für IIS verwenden und Tabellenspeicher für Ereignisse](log-analytics-azure-storage-iis-table.md) , die Protokolle für Azure Lesen services die Diagnose schreiben Tabellenspeicher oder IIS-Protokolle geschrieben zu BLOB-Speicher.
- [Aktivieren von Lösungen](log-analytics-add-solutions.md) einen Einblick in die Daten bereitstellen.
- [Verwenden von Suchabfragen](log-analytics-log-searches.md) zum Analysieren der Daten.

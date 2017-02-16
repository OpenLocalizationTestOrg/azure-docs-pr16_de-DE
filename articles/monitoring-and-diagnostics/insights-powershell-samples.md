<properties
    pageTitle="Azure Monitor PowerShell Schnellstart-Beispiele. | Microsoft Azure"
    description="Mithilfe von PowerShell Azure Monitor Features wie automatisch skalieren, Benachrichtigungen, Webhooks und Suche Aktivitätsprotokolle zuzugreifen."
    authors="kamathashwin"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="ashwink"/>

# <a name="azure-monitor-powershell-quick-start-samples"></a>Azure Monitor PowerShell Schnellstart-Beispiele

Dieser Artikel zeigt, hören Sie PowerShell-Befehlen, damit Sie Azure Monitor Features zugreifen können. Azure Monitor können Sie zum Skalieren Cloud Services-virtuellen Computern und Web Apps und benachrichtigen Benachrichtigungen senden, oder rufen Sie die Web-URLs auf der Grundlage der Werte von Daten konfiguriert werden.

>[AZURE.NOTE] Azure Monitor ist den neuen Namen für was "Azure Einsichten" aufgerufen wurde, bis zu 25 % September 2016. Die Namespaces und somit die folgenden Befehle enthalten jedoch weiterhin die "Einsichten".

## <a name="set-up-powershell"></a>Einrichten von PowerShell
Wenn Sie noch nicht geschehen ist, schieben Sie PowerShell auf dem Computer ausgeführt werden. Weitere Informationen finden Sie unter [So installieren und Konfigurieren von PowerShell](../powershell-install-configure.md) .

## <a name="examples-in-this-article"></a>Die Beispiele in diesem Artikel

Die Beispiele im Artikel beschreiben, wie Sie Azure Monitor Cmdlets verwenden. Sie können auch die gesamte Liste der Azure Monitor PowerShell-Cmdlets am [Cmdlets Azure Monitor (Einsichten)](https://msdn.microsoft.com/library/azure/mt282452#40v=azure.200#41.aspx)überprüfen.


## <a name="sign-in-and-use-subscriptions"></a>Melden Sie sich an, und Verwenden von Abonnements

Zuerst, melden Sie sich bei Ihrem Abonnement Azure an.

```PowerShell
Login-AzureRmAccount
```

Dazu müssen Sie anmelden. Nachdem Sie Ihr Konto ausführen, werden TenantId und Standard-Abonnement-Id angezeigt. Alle Azure Cmdlets arbeiten im Zusammenhang mit Ihrem Standardabonnement. Um die Liste der Abonnements anzuzeigen, die Sie Zugriff haben, verwenden Sie den folgenden Befehl aus.

```PowerShell
Get-AzureRmSubscription
```

Um Kontext arbeiten in ein anderes Abonnement zu ändern, verwenden Sie den folgenden Befehl ein.

```PowerShell
Set-AzureRmContext -SubscriptionId <subscriptionid>
```


## <a name="retrieve-audit-logs-for-a-subscription"></a>Abrufen von Überwachungsprotokollen für ein Abonnement
Verwenden der `Get-AzureRmLog` Cmdlet.  Nachstehend sind einige allgemeine Beispiele.

Abrufen von Protokolleinträge aus diesem Uhrzeit/Datum zu präsentieren:

```PowerShell
Get-AzureRmLog -StartTime 2016-03-01T10:30
```

Abrufen von Protokolleinträge zwischen einem Datum/Uhrzeit-Bereich:

```PowerShell
Get-AzureRmLog -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

Abrufen von Protokolleinträge aus einer bestimmten Ressourcengruppe:

```PowerShell
Get-AzureRmLog -ResourceGroup 'myrg1'
```

Rufen Sie Protokolleinträge in eine bestimmte Ressourcenanbieter zwischen einem Datum/Uhrzeit-Bereich:

```PowerShell
Get-AzureRmLog -ResourceProvider 'Microsoft.Web' -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

Erhalten Sie alle Protokolleinträge mit einer bestimmten Anrufer an:

```PowerShell
Get-AzureRmLog -Caller 'myname@company.com'
```

Mit dem folgende Befehl wird die letzte 1000 Ereignisse aus dem Überwachungsprotokoll abgerufen:

```PowerShell
Get-AzureRmLog -MaxEvents 1000
```

`Get-AzureRmLog`unterstützt viele andere Parameter. Finden Sie unter der `Get-AzureRmLog` Bezug für Weitere Informationen.

>[AZURE.NOTE] `Get-AzureRmLog`enthält nur 15 Tage des Verlaufs ein. Mit den **MaxEvents -** Parameter können Sie die letzten N Ereignisse, über 15 Tage Abfragen. Access-Ereignisse älter als 15 Tage verwenden Sie die REST-API oder SDK (C#-Beispiel mit dem SDK). Wenn Sie keine **Startzeit**einfügen, ist der Standardwert **Endzeit** minus 1 Stunde. Wenn Sie nicht **Endzeit**, ist der Standardwert aktuelle Uhrzeit. Bei allen Zeitangaben gilt in UTC.

## <a name="retrieve-alerts-history"></a>Abrufen von Benachrichtigungsverlauf
Um alle benachrichtigen Ereignisse anzeigen möchten, können Sie die Protokolle der Ressourcenmanager Azure mit den folgenden Beispielen Abfragen.

```PowerShell
Get-AzureRmLog -Caller "Microsoft.Insights/alertRules" -DetailedOutput -StartTime 2015-03-01
```

Klicken Sie zum Anzeigen des Verlaufs für eine bestimmte Benachrichtigung Regel können Sie die `Get-AzureRmAlertHistory` Cmdlet, in der die Regel-ID Ressource übergeben.

```PowerShell
Get-AzureRmAlertHistory -ResourceId /subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/alertrules/myalert -StartTime 2016-03-1 -Status Activated
```

Die `Get-AzureRmAlertHistory` Cmdlet unterstützt verschiedene Parameter. Weitere Informationen finden Sie unter [Get-AlertHistory](https://msdn.microsoft.com/library/mt282453.aspx).


## <a name="retrieve-information-on-alert-rules"></a>Abrufen von Informationen zu Warnungsregeln
Alle der folgenden Befehle wirken sich auf eine Ressourcengruppe mit dem Namen "Montest".

Anzeigen der Eigenschaften die Regel:

```PowerShell
Get-AzureRmAlertRule -Name simpletestCPU -ResourceGroup montest -DetailedOutput
```

Rufen Sie alle Benachrichtigungen für eine Ressourcengruppe ab:

```PowerShell
Get-AzureRmAlertRule -ResourceGroup montest
```

Abrufen von alle Warnungsregeln für eine Ressource festlegen. Legen Sie beispielsweise alle Warnungsregeln eines virtuellen Computers.

```PowerShell
Get-AzureRmAlertRule -ResourceGroup montest -TargetResourceId /subscriptions/s1/resourceGroups/montest/providers/Microsoft.Compute/virtualMachines/testconfig
```

`Get-AzureRmAlertRule`unterstützt die anderen Parameter. Weitere Informationen finden Sie unter [Get-AlertRule](https://msdn.microsoft.com/library/mt282459.aspx) .

## <a name="create-alert-rules"></a>Erstellen von Warnungsregeln
Sie können die `Add-AlertRule` -Cmdlet zum Erstellen, aktualisieren oder Deaktivieren einer Regel.

Sie können e-Mail- und Webhook Eigenschaften mit erstellen `New-AzureRmAlertRuleEmail` und `New-AzureRmAlertRuleWebhook`Hilfethemas. Weisen Sie in das Cmdlet benachrichtigen Regel diese als Aktionen auf die Eigenschaft **Aktionen** der Regel benachrichtigen.

Im nächste Abschnitt enthält ein Beispiel, das Sie zum Erstellen einer Benachrichtigung Regel mit verschiedenen Parameter zeigt.

### <a name="alert-rule-on-a-metric"></a>Regel auf einer Metrik
Die folgende Tabelle beschreibt die Parameter und Werte verwendet, um eine Benachrichtigung, die mit einer Metrik erstellen.


|Parameter|Wert|
|---|---|
|Namen|  simpletestdiskwrite|
|Speicherort der diese Regel benachrichtigen|   Ostasiatische US|
|ResourceGroup| montest|
|TargetResourceId|  /Subscriptions/s1/resourceGroups/montest/Providers/Microsoft.Compute/virtualMachines/testconfig|
|MetricName der Warnung, die erstellt wird|   \PhysicalDisk (_Total) \Disk/s. Finden Sie unter der `Get-MetricDefinitions` Cmdlet unter dazu, wie Sie die genauen metrischen Namen abrufen|
|Operator|  Größer als|
|Schwellenwert (Anzahl/sec, in für diese Metrisch)|    1|
|WindowSize (hh: mm: Format)|  00:05:00|
|Aggregator (Statistik der Metrik, die durchschnittliche Anzahl in diesem Fall verwendet)|  Mittelwert|
|angepasste e-Mails (Zeichenfolgenarray)|'foo@example.com','bar@example.com'|
|Senden von e-Mail auf Besitzer, Mitwirkende und Leser|    -SendToServiceOwners|

Erstellen einer e-Mail-Aktion

```PowerShell
$actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
```

Erstellen einer Aktion Webhook

```PowerShell
$actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://example.com?token=mytoken
```

Erstellen Sie die Regel auf die CPU % Metrik einer klassischen virtuellen Computers

```PowerShell
Add-AzureRmMetricAlertRule -Name vmcpu_gt_1 -Location "East US" -ResourceGroup myrg1 -TargetResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.ClassicCompute/virtualMachines/my_vm1 -MetricName "Percentage CPU" -Operator GreaterThan -Threshold 1 -WindowSize 00:05:00 -TimeAggregationOperator Average -Actions $actionEmail, $actionWebhook -Description "alert on CPU > 1%"
```

Abrufen der Regel benachrichtigen.

```PowerShell
Get-AzureRmAlertRule -Name vmcpu_gt_1 -ResourceGroup myrg1 -DetailedOutput
```

Das Cmdlet "Benachrichtigung hinzufügen" aktualisiert zudem die Regel aus, wenn eine Regel für die angegebenen Eigenschaften bereits vorhanden ist. Um eine Regel zu deaktivieren, nehmen Sie den Parameter **- DisableRule**.

### <a name="alert-on-audit-log-event"></a>Klicken Sie auf Audit Log Ereignis benachrichtigen

>[AZURE.NOTE] Dieses Feature ist immer noch in der Seitenansicht.

In diesem Szenario erhalten Sie e-Mails senden, wenn eine Website in mein Abonnement in der Gruppe für die Ressource *abhingrgtest123*erfolgreich gestartet wird.

Einrichten eines e-Mail-Regel

```PowerShell
$actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
```

Einrichten einer Regel webhook

```PowerShell
$actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://example.com?token=mytoken
```

Erstellen Sie die Regel auf das Ereignis

```PowerShell
Add-AzureRmLogAlertRule -Name superalert1 -Location "East US" -ResourceGroup myrg1 -OperationName microsoft.web/sites/start/action -Status Succeeded -TargetResourceGroup abhingrgtest123 -Actions $actionEmail, $actionWebhook
```

Abrufen der Regel benachrichtigen.

```PowerShell
Get-AzureRmAlertRule -Name superalert1 -ResourceGroup myrg1 -DetailedOutput
```

Die `Add-AlertRule` Cmdlet ermöglicht verschiedene andere Parameter. Weitere Informationen finden Sie unter [Hinzufügen-AlertRule](https://msdn.microsoft.com/library/mt282468.aspx).

## <a name="get-a-list-of-available-metrics-for-alerts"></a>Eine Liste der verfügbaren Kennzahlen für Benachrichtigungen erhalten
Sie können die `Get-AzureRmMetricDefinition` -Cmdlet zum Anzeigen der Liste der alle Kriterien für eine bestimmte Ressource.

```PowerShell
Get-AzureRmMetricDefinition -ResourceId <resource_id>
```

Im folgende Beispiel wird eine Tabelle mit der Metrik Namen und die Einheit für sie generiert.

```PowerShell
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

Eine vollständige Liste der verfügbaren Optionen für `Get-AzureRmMetricDefinition` [Get-MetricDefinitions](https://msdn.microsoft.com/library/mt282458.aspx)verfügbar ist.


## <a name="create-and-manage-autoscale-settings"></a>Erstellen und Verwalten von Einstellungen automatisch skalieren
Eine Ressource, beispielsweise eine Web-app, virtuellen Computers, Cloud-Dienst oder virtueller Computer Skalierung festlegen kann nur eine automatisch skalieren Einstellung dafür konfiguriert haben.
Kann jedoch jede Einstellung automatisch skalieren mit mehreren Profilen arbeiten. Beispielsweise basiert eine für ein Profil Performance-basierte skalieren und eine zweite für einen Zeitplan Profil. Jedes Profil kann mehrere Regeln, die darauf konfiguriert haben. Weitere Informationen zu skalieren finden Sie unter [So automatisch Skalieren einer Anwendung](../cloud-services/cloud-services-how-to-scale.md).

Hier sind die Schritte, die wir verwenden werden:

1. Erstellen Sie Regeln.
2. Erstellen Sie Regeln, die Sie erstellt haben zuvor auf die Profile Zuordnung Profile.
3. Optional: Erstellen Sie Benachrichtigungen für automatisch skalieren, indem Sie Webhook und e-Mail-Eigenschaften konfigurieren.
4. Erstellen Sie eine Einstellung automatisch skalieren mit einen Namen für die Zielressource durch Zuordnung der Profile und Benachrichtigungen, die Sie in den vorherigen Schritten erstellt haben.

In den folgenden Beispielen können Sie anzeigen, erstellen Sie eine automatisch skalieren-Einstellung für einen virtuellen Computer Maßstab für ein Windows-Betriebssystem basiert, indem Sie die CPU-Auslastung Metrik festlegen.

Erstellen Sie zuerst eine Regel Skalierung, mit einer Instanz zählen erhöhen.

```PowerShell
$rule1 = New-AzureRmAutoscaleRule -MetricName "\Processor(_Total)\% Processor Time" -MetricResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -Operator GreaterThan -MetricStatistic Average -Threshold 0.01 -TimeGrain 00:01:00 -TimeWindow 00:10:00 -ScaleActionCooldown 00:10:00 -ScaleActionDirection Increase -ScaleActionScaleType ChangeCount -ScaleActionValue 1
```     

Als Nächstes erstellen Sie eine Regel zu skalieren in, mit einer Instanz zählen verringern.

```PowerShell
$rule2 = New-AzureRmAutoscaleRule -MetricName "\Processor(_Total)\% Processor Time" -MetricResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -Operator GreaterThan -MetricStatistic Average -Threshold 2 -TimeGrain 00:01:00 -TimeWindow 00:10:00 -ScaleActionCooldown 00:10:00 -ScaleActionDirection Decrease -ScaleActionScaleType ChangeCount -ScaleActionValue 1
```

Klicken Sie dann erstellen Sie ein Profil für die Regeln.

```PowerShell
$profile1 = New-AzureRmAutoscaleProfile -DefaultCapacity 2 -MaximumCapacity 10 -MinimumCapacity 2 -Rules $rule1,$rule2 -Name "My_Profile"
```

Erstellen einer Webhook-Eigenschaft.

```PowerShell
$webhook_scale = New-AzureRmAutoscaleWebhook -ServiceUri "https://example.com?mytoken=mytokenvalue"
```

Erstellen Sie die Benachrichtigungseigenschaft für die Einstellung automatisch skalieren, einschließlich e-Mail und die Webhook, die Sie zuvor erstellt haben.

```PowerShell
$notification1= New-AzureRmAutoscaleNotification -CustomEmails ashwink@microsoft.com -SendEmailToSubscriptionAdministrators SendEmailToSubscriptionCoAdministrators -Webhooks $webhook_scale
```

Erstellen Sie schließlich die Einstellung automatisch skalieren, um das Profil hinzuzufügen, dem Sie soeben erstellt haben.

```PowerShell
Add-AzureRmAutoscaleSetting -Location "East US" -Name "MyScaleVMSSSetting" -ResourceGroup big2 -TargetResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -AutoscaleProfiles $profile1 -Notifications $notification1
```

Weitere Informationen zum Verwalten von Einstellungen automatisch skalieren finden Sie unter [Get-AutoscaleSetting](https://msdn.microsoft.com/library/mt282461.aspx).

## <a name="autoscale-history"></a>Verlauf automatisch skalieren
Das folgende Beispiel zeigt Sie, wie Sie zuletzt verwendete automatisch skalieren und benachrichtigen Ereignisse anzeigen können. Verwenden Sie die Audit Log-Suche, um den Verlauf automatisch skalieren anzuzeigen.

```PowerShell
Get-AzureRmLog -Caller "Microsoft.Insights/autoscaleSettings" -DetailedOutput -StartTime 2015-03-01
```

Sie können die `Get-AzureRmAutoScaleHistory` -Cmdlet zum Skalieren Verlauf abrufen.

```PowerShell
Get-AzureRmAutoScaleHistory -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/microsoft.insights/autoscalesettings/myScaleSetting -StartTime 2016-03-15 -DetailedOutput
```

Weitere Informationen finden Sie unter [Get-AutoscaleHistory](https://msdn.microsoft.com/library/mt282464.aspx).

### <a name="view-details-for-an-autoscale-setting"></a>Anzeigen von Details für eine Einstellung automatisch skalieren
Sie können die `Get-Autoscalesetting` -Cmdlet zum Abrufen von Informationen über die Einstellung automatisch skalieren.

Im folgenden Beispiel wird die Details zu allen automatisch skalieren Einstellungen in der Ressource 'myrg1'.

```PowerShell
Get-AzureRmAutoscalesetting -ResourceGroup myrg1 -DetailedOutput
```

Das folgende Beispiel zeigt die Details für alle automatisch skalieren Einstellungen für die Ressource 'myrg1' und speziell die automatisch skalieren-Einstellung, die mit dem Namen 'MyScaleVMSSSetting'.

```PowerShell
Get-AzureRmAutoscalesetting -ResourceGroup myrg1 -Name MyScaleVMSSSetting -DetailedOutput
```

### <a name="remove-an-autoscale-setting"></a>Entfernen einer Einstellung automatisch skalieren
Sie können die `Remove-Autoscalesetting` -Cmdlet zum Löschen einer Einstellung automatisch skalieren.

```PowerShell
Remove-AzureRmAutoscalesetting -ResourceGroup myrg1 -Name MyScaleVMSSSetting
```

## <a name="manage-log-profiles-for-audit-logs"></a>Verwalten Sie Log Profile für Überwachungsprotokolle

Sie können ein *Protokoll Profil* erstellen und Exportieren von Daten aus der Überwachungsprotokolle mit einem Speicherkonto und Sie können Daten Aufbewahrung dafür konfigurieren. Optional können Sie auch die Daten zu Ihrem Ereignis Hub streamen. Beachten Sie, dass dieses Feature derzeit in der Vorschau anzeigen und Sie können nur ein Protokoll Profil pro Abonnement erstellen. Sie können die folgenden Cmdlets mit Ihres aktuellen Abonnements erstellen und Verwalten von Benutzerprofilen Log verwenden. Sie können auch ein bestimmtes Abonnement auswählen. Obwohl das aktuelle Abonnement PowerShell standardmäßig verwendet, Sie können jederzeit ändern, dass beim Verwenden `Set-AzureRmContext`. Sie können Überwachungsprotokolle auf Weiterleitung von Daten an alle Speicher-Konto oder ein Ereignis-Hub innerhalb dieser Abonnement konfigurieren. Daten werden als BLOB-Dateien im JSON-Format geschrieben.

### <a name="get-a-log-profile"></a>Abrufen eines Profils log
Zum Abrufen Ihrer vorhandenen Log Profile verwenden Sie die `Get-AzureRmLogProfile` Cmdlet.

### <a name="add-a-log-profile-without-data-retention"></a>Hinzufügen eines Profils Log ohne Daten Aufbewahrungsrichtlinien

```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia
```

### <a name="remove-a-log-profile"></a>Entfernen eines Profils log

```PowerShell
Remove-AzureRmLogProfile -name my_log_profile_s1
```

### <a name="add-a-log-profile-with-data-retention"></a>Hinzufügen eines Profils Log mit Daten Aufbewahrungsrichtlinien

Sie können die **RetentionInDays -** Eigenschaft für die Anzahl der Tage, die als eine positive ganze Zahl, angeben, dass die Daten beibehalten werden.

```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia -RetentionInDays 90
```

### <a name="add-log-profile-with-retention-and-eventhub"></a>Log-Profil mit Aufbewahrungsrichtlinien und EventHub hinzufügen
Zusätzlich zum routing von Daten mit Speicherkonto, können Sie auch auf ein Ereignis Hub streamen. Beachten Sie, dass in dieser Preview-Version und die Speicherung Kontokonfiguration obligatorisch ist aber Ereignis Hub Konfiguration optional ist.

```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia -RetentionInDays 90
```

## <a name="configure-diagnostics-logs"></a>Konfigurieren der Diagnose von Protokollen
Viele Azure Dienste bieten zusätzliche Protokolle und telemetrieprotokoll, einschließlich Azure Netzwerk Sicherheitsgruppen, Software Lastenausgleich, Schlüssel Tresor, Azure Suchdienste und Logik Apps, und sie können zum Speichern von Daten in Ihr Konto Azure-Speicher konfiguriert werden. Dieser Vorgang kann nur auf einer Ressourcenebene ausgeführt werden, und das Speicherkonto sollten präsentieren in derselben Region als die Zielressource, die Einstellung Diagnose so konfiguriert ist.

### <a name="get-diagnostic-setting"></a>Abrufen von Diagnoseprotokollen Einstellung

```PowerShell
Get-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp
```

Deaktivieren Sie die Einstellung Diagnose

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $false
```

Aktivieren von Diagnoseprotokollen Einstellung ohne Aufbewahrung

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $true
```

Aktivieren von Diagnoseprotokollen Einstellung mit Aufbewahrungsrichtlinien

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $true -RetentionEnabled $true -RetentionInDays 90
```

Aktivieren Sie diagnostic Einstellung mit Aufbewahrungsrichtlinien für eine bestimmte Log-Kategorie

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Network/networkSecurityGroups/viruela1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/sakteststorage -Categories NetworkSecurityGroupEvent -Enable $true -RetentionEnabled $true -RetentionInDays 90
```

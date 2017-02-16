<properties
    pageTitle="Azure Monitor CLI Schnellstart-Beispiele. | Microsoft Azure"
    description="Beispiel für CLI-Befehle für Monitor Azure-Features. Azure Monitor ist ein Microsoft Azure-Dienst, der eine Benachrichtigung zu senden, rufen Sie die Web-URLs auf der Grundlage der Werte von konfiguriert werden Daten und automatisch skalieren Cloud Services, virtuellen Computern und Web Apps ermöglicht."
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
    ms.date="09/08/2016"
    ms.author="ashwink"/>

# <a name="azure-monitor--cross-platform-cli-quick-start-samples"></a>Azure Monitor Plattformen CLI Schnellstart-Beispiele

Dieser Artikel zeigt Ihnen Beispiel für line Interface (CLI) Befehle, damit Sie Azure Monitor Features zugreifen können. Azure Monitor können Sie zum Skalieren Cloud Services-virtuellen Computern und Web Apps und benachrichtigen Benachrichtigungen senden, oder rufen Sie die Web-URLs auf der Grundlage der Werte von Daten konfiguriert werden.

>[AZURE.NOTE] Azure Monitor ist den neuen Namen für was "Azure Einsichten" aufgerufen wurde, bis zu 25 % September 2016. Die Namespaces und somit die folgenden Befehle enthalten jedoch weiterhin die "Einsichten".


## <a name="prerequisites"></a>Erforderliche Komponenten

Wenn Sie die CLI Azure bereits installiert haben, finden Sie unter [Installieren der CLI Azure](../xplat-cli-install.md). Wenn Sie mit Azure CLI nicht vertraut sind, erhalten Sie weitere Informationen finden sie unter [Verwenden der Azure CLI für Mac, Linux, und Windows Azure-Ressourcenmanager](../xplat-cli-azure-resource-manager.md).


Installieren Sie in Windows Npm aus der [Node.js-Website](https://nodejs.org/)ein. Nach Abschluss die Installation mit CMD.exe mit Berechtigungen als Administrator ausführen, führen Sie Folgendes aus dem Ordner, in dem Npm installiert ist:

```console
npm install azure-cli --global
```

Als Nächstes navigieren Sie zu einem beliebigen Ordner/Standort möchten, und geben Sie an der Befehlszeile:

```console
azure help
```

## <a name="log-in-to-azure"></a>Melden Sie sich bei Azure

Dieser erste Schritt besteht zum Anmelden bei Ihrem Konto Azure.

```console
azure login
```

Nachdem dieser Befehl ausgeführt haben, müssen Sie über den Anweisungen auf dem Bildschirm anmelden. Danach wird Ihr Konto TenantId und Standard-Abonnement-ID an. Alle Befehle im Kontext der Standardabonnement arbeiten.

Um die Details Ihres aktuellen Abonnements aufzulisten, verwenden Sie den folgenden Befehl ein.

```console
azure account show
```

Um auf ein anderes Abonnement arbeiten Kontext ändern möchten, verwenden Sie den folgenden Befehl ein.

```console
azure account set "subscription ID or subscription name"
```

Wenn Azure Ressourcenmanager und Azure Monitor Befehle verwenden möchten, müssen Sie im Ressourcenmanager Azure-Modus ausgeführt werden.

```console
azure config mode arm
```

Führen Sie folgende Schritte aus, um eine Liste aller unterstützten Azure Monitor Befehle anzeigen.

```console
azure insights
```

## <a name="view-audit-logs-for-a-subscription"></a>Anzeigen der Überwachungsprotokolle für ein Abonnement

Führen Sie folgende Schritte aus, um eine Liste der Überwachungsprotokolle anzeigen.

```console
azure insights logs list [options]
```

Versuchen Sie Folgendes, um alle verfügbaren Optionen anzuzeigen.

```console
azure insights logs list -help
```

Hier ist ein Beispiel zu der Liste Protokolle nach einer resourceGroup

```console
azure insights logs list --resourceGroup "myrg1"
```

Beispiel in der Liste Protokolle vom Anrufer

```console
azure insights logs list --caller "myname@company.com"
```

Beispiel zur Liste, die vom Anrufer auf einen Ressourcentyp innerhalb eines Datums Start- und protokolliert

```console
azure insights logs list --resourceProvider "Microsoft.Web" --caller "myname@company.com" --startTime 2016-03-08T00:00:00Z --endTime 2016-03-16T00:00:00Z
```

## <a name="work-with-alerts"></a>Arbeiten mit Benachrichtigungen
Die Informationen können im Abschnitt für die Arbeit mit Benachrichtigungen.

### <a name="get-alert-rules-in-a-resource-group"></a>Abrufen von Warnungsregeln in einer Ressourcengruppe

```console
azure insights alerts rule list abhingrgtest123
azure insights alerts rule list abhingrgtest123 --ruleName andy0323
```

### <a name="create-a-metric-alert-rule"></a>Erstellen einer Regel für metrischen benachrichtigen

```console
azure insights alerts actions email create --customEmails foo@microsoft.com
azure insights alerts actions webhook create https://someuri.com
azure insights alerts rule metric set andy0323 eastus abhingrgtest123 PT5M GreaterThan 2 /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Web-EastUS/providers/Microsoft.Web/serverfarms/Default1 BytesReceived Total
```

### <a name="create-a-log-alert-rule"></a>Erstellen einer Benachrichtigung Log-Regel

```console
azure insights alerts rule log set ruleName eastus resourceGroupName someOperationName
```

### <a name="create-webtest-alert-rule"></a>Webtest benachrichtigen Regel erstellen

```console
azure insights alerts rule webtest set leowebtestr1-webtestr1 eastus Default-Web-WestUS PT5M 1 GSMT_AvRaw /subscriptions/b67f7fec-69fc-4974-9099-a26bd6ffeda3/resourcegroups/Default-Web-WestUS/providers/microsoft.insights/webtests/leowebtestr1-webtestr1
```

### <a name="delete-an-alert-rule"></a>Löschen einer Regel

```console
azure insights alerts rule delete abhingrgtest123 andy0323
```

## <a name="log-profiles"></a>Log-Profile
Verwenden Sie die Informationen in diesem Abschnitt für die Arbeit mit Log Profile.

### <a name="get-a-log-profile"></a>Abrufen eines Profils log

```console
azure insights logprofile list
azure insights logprofile get -n default
```


### <a name="add-a-log-profile-without-retention"></a>Hinzufügen eines Profils Log ohne Aufbewahrung

```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia
```

### <a name="remove-a-log-profile"></a>Entfernen eines Profils log

```console
azure insights logprofile delete --name default
```

### <a name="add-a-log-profile-with-retention"></a>Hinzufügen eines Profils Log mit Aufbewahrungsrichtlinien

```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia --retentionInDays 90
```

### <a name="add-a-log-profile-with-retention-and-eventhub"></a>Hinzufügen eines Profils Log mit Aufbewahrungsrichtlinien und EventHub

```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --serviceBusRuleId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/testshoeboxeastus/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia --retentionInDays 90
```


## <a name="diagnostics"></a>Diagnose
Verwenden Sie die Informationen in diesem Abschnitt für die Arbeit mit diagnoseeinstellungen.

### <a name="get-a-diagnostic-setting"></a>Abrufen einer Diagnoseprotokollen Einstellung

```console
azure insights diagnostic get --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp
```

### <a name="disable-a-diagnostic-setting"></a>Deaktivieren einer Diagnoseprotokollen Einstellung

```console
azure insights diagnostic set --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp --storageId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/shibanitesting --enabled false
```

### <a name="enable-a-diagnostic-setting-without-retention"></a>Aktivieren einer Diagnoseprotokollen Einstellung ohne Aufbewahrung

```console
azure insights diagnostic set --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp --storageId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/shibanitesting --enabled true
```


## <a name="autoscale"></a>Automatisch skalieren
Verwenden Sie die Informationen in diesem Abschnitt für die Arbeit mit Einstellungen automatisch skalieren. Sie müssen diese Beispiele zu ändern.

### <a name="get-autoscale-settings-for-a-resource-group"></a>Abrufen von automatisch skalieren Einstellungen für eine Ressourcengruppe

```console
azure insights autoscale setting list montest2
```

### <a name="get-autoscale-settings-by-name-in-a-resource-group"></a>Abrufen von Einstellungen automatisch skalieren anhand des Namens in einer Ressourcengruppe

```console
azure insights autoscale setting list montest2 -n setting2
```


### <a name="set-auotoscale-settings"></a>Festlegen der Einstellungen für auotoscale

```console
azure insights autoscale setting set montest2 -n setting2 --settingSpec
```

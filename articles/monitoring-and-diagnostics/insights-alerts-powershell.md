<properties
    pageTitle="Verwenden Sie zum Erstellen von Benachrichtigungen für Azure Services PowerShell | Microsoft Azure"
    description="Verwenden Sie PowerShell Azure Benachrichtigungen erstellen, die Benachrichtigungen oder Automatisierung auslösen können, wenn die angegebenen Bedingungen erfüllt sind."
    authors="rboucher"
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
    ms.date="10/20/2016"
    ms.author="robb"/>

# <a name="use-powershell-to-create-alerts-for-azure-services"></a>Verwenden Sie PowerShell, um Benachrichtigungen für Azure-Dienste zu erstellen.

> [AZURE.SELECTOR]
- [Portal](insights-alerts-portal.md)
- [PowerShell](insights-alerts-powershell.md)
- [CLI](insights-alerts-command-line-interface.md)

## <a name="overview"></a>(Übersicht)

In diesem Artikel wird gezeigt, wie Sie Azure Benachrichtigungen mithilfe der PowerShell einrichten werden kann.  

Sie können eine Benachrichtigung basierend auf Überwachung Kennzahlen zur oder Ereignisse auf Ihre Azure-Dienste erhalten.

- **Metrische Werte** - der Warnung wird ausgelöst, wenn der Wert einer angegebenen Metrik einen Schwellenwert überschreitet, die, den Sie in eine beliebige Richtung zuweisen. D. h., löst beide Wenn zuerst die Bedingung erfüllt ist, und klicken Sie dann später an, die Bedingung ist nicht mehr erfüllt.    
- **Aktivität protokollieren von Ereignissen** – auf *jedes* Ereignis oder, nur, wenn eine bestimmte Anzahl von Ereignissen auftreten, kann eine Benachrichtigung auslösen.

Sie können eine Benachrichtigung, wenn Sie die folgenden Schritte ausführen, wenn es auslöst konfigurieren:

- Senden von e-Mail-Benachrichtigungen an die Dienstadministrator und Co-Administratoren
- Senden Sie e-Mail, um zusätzliche e-Mails, die Sie angeben.
- Anrufen eines webhook
- Starten der Ausführung einer Azure Runbooks (nur vom Azure-Portal)

Sie konfigurieren können, und erhalten Informationen zum Verwenden von Warnungsregeln

- [Azure-portal](insights-alerts-portal.md)
- [PowerShell](insights-alerts-powershell.md)
- [line Interface (CLI)](insights-alerts-command-line-interface.md)
- [Azure Monitor REST-API](https://msdn.microsoft.com/library/azure/dn931945.aspx)


Weitere Informationen finden können Sie immer eingeben ```get-help``` , und klicken Sie dann den PowerShell-Befehl auf Hilfe benötigen.

## <a name="create-alert-rules-in-powershell"></a>Erstellen von Warnungsregeln in PowerShell

1. Melden Sie sich bei Azure.   

    ```PowerShell
    Login-AzureRmAccount

    ```

2. Abrufen einer Liste der Abonnements Ihnen zur Verfügung stehen. Stellen Sie sicher, dass Sie beim Arbeiten mit dem richtigen Abonnement. Wenn nicht, legen Sie ihn auf die richtigen eine unter Verwendung der Ausgabe von `Get-AzureRmSubscription`.

    ```PowerShell
    Get-AzureRmSubscription
    Get-AzureRmContext
    Set-AzureRmContext -SubscriptionId <subscriptionid>
    ```

3.  Um vorhandene Regeln auf einer Ressourcengruppe sind, verwenden Sie den folgenden Befehl aus:

    ```PowerShell
    Get-AzureRmAlertRule -ResourceGroup <myresourcegroup> -DetailedOutput
    ```

4. Um eine Regel zu erstellen, müssen Sie zuerst mehrere wichtige Informationsarten haben. 
    - Die **Ressourcen-ID** für die Ressource festlegen eine Benachrichtigung für möchten
    - Die für diese Ressource verfügbaren **metrischen Definitionen**

    Eine Möglichkeit zum Abrufen der Ressource-ID besteht darin, das Azure-Portal zu verwenden. Wählen sie unter der Voraussetzung, dass die Ressource bereits erstellt wurde, im Portal. Wählen Sie dann in der nächsten Blade *Eigenschaften* im Abschnitt *Settings* . Die Ressource-ID ist ein Feld in der nächsten Blade. Eine weitere Möglichkeit besteht darin, der [Azure-Explorers](https://resources.azure.com/)verwenden.

    Ist eine Beispiel-Ressourcen-Id für eine Web app

    ```
    /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename
    ```

    Sie können `Get-AzureRmMetricDefinition` können Sie die Liste aller metrischen Definitionen für eine bestimmte Ressource anzeigen.

    ```PowerShell
    Get-AzureRmMetricDefinition -ResourceId <resource_id>
    ```

    Im folgende Beispiel wird eine Tabelle mit der Metrik Namen und die Einheit für die Metrik generiert.

    ```PowerShell
    Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit

    ```
    Eine vollständige Liste der verfügbaren Optionen für Get-AzureRmMetricDefinition ist durch Ausführen der Get-MetricDefinitions verfügbar.


5. Im folgenden Beispiel wird von einer Benachrichtigung für eine Ressource für die Website. Die Benachrichtigung Trigger bei jeder konsistente Datenverkehr für 5 Minuten und beim Eingang von keine Datenverkehr für 5 Minuten.

    ```PowerShell
    Add-AzureRmMetricAlertRule -Name myMetricRuleWithWebhookAndEmail -Location "East US" -ResourceGroup myresourcegroup -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename -MetricName "BytesReceived" -Operator GreaterThan -Threshold 2 -WindowSize 00:05:00 -TimeAggregationOperator Total -Description "alert on any website activity"

    ```

6. Zum Erstellen von Webhook, oder Senden von e-Mails, wenn eine Warnung ausgelöst wird, müssen Sie zuerst erstellen Sie die e-Mail- und/oder Webhooks. Erstellen Sie dann sofort die Regel danach mit dem - Aktionen Tag und wie im folgenden Beispiel gezeigt. Sie können keine Webhook oder e-Mail-Nachrichten mit zuordnen Regeln über PowerShell bereits erstellt.


    ```PowerShell
    $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
    $actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://www.contoso.com?token=mytoken

    Add-AzureRmMetricAlertRule -Name myMetricRuleWithWebhookAndEmail -Location "East US" -ResourceGroup myresourcegroup -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename -MetricName "BytesReceived" -Operator GreaterThan -Threshold 2 -WindowSize 00:05:00 -TimeAggregationOperator Total -Actions $actionEmail, $actionWebhook -Description "alert on any website activity"
    ```


7. Verwenden Sie zum Erstellen einer Benachrichtigung, die Trigger auf eine bestimmte Bedingung im Aktivitätsprotokoll Befehle im folgenden Format

    ```PowerShell
    $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
    $actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://www.contoso.com?token=mytoken

    Add-AzureRmLogAlertRule -Name myLogAlertRule -Location "East US" -ResourceGroup myresourcegroup -OperationName microsoft.web/sites/start/action -Status Succeeded -TargetResourceGroup resourcegroupbeingmonitored -Actions $actionEmail, $actionWebhook
    ```

    Die - OperationName entspricht einen Typ von Ereignissen im Aktivitätsprotokoll. Beispiele: *Microsoft.Compute/virtualMachines/delete* und *microsoft.insights/diagnosticSettings/write*.

    Den PowerShell-Befehl [Get-AzureRmProviderOperation](https://msdn.microsoft.com/library/mt603720.aspx) können Sie um eine Liste von möglichen OperationNames zu erhalten. Alternativ können Sie das Azure-Portal der Aktivität Log Abfragen und Auffinden bestimmter ältere Vorgänge, die Sie eine Benachrichtigung erstellen möchten. Die Vorgänge in der Protokollansicht Grafik von den Anzeigenamen angezeigt. Suchen Sie in den JSON für den Eintrag, und den Wert OperationName herausziehen.   

8. Stellen Sie sicher, dass die Benachrichtigungen ordnungsgemäß erstellt wurde, indem Sie die einzelnen Regeln.

    ```PowerShell
    Get-AzureRmAlertRule -Name myMetricRuleWithWebhookAndEmail -ResourceGroup myresourcegroup -DetailedOutput

    Get-AzureRmAlertRule -Name myLogAlertRule -ResourceGroup myresourcegroup -DetailedOutput
    ```

9. Löschen Sie die Benachrichtigungen. Diese Befehle löschen Sie die Regeln, die zuvor in diesem Artikel erstellt.

    ```PowerShell
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myrule
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myMetricRuleWithWebhookAndEmail
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myLogAlertRule
    ```

## <a name="next-steps"></a>Nächste Schritte

* [Eine Übersicht über die Überwachung Azure](monitoring-overview.md) , einschließlich der Arten von Informationen können Sie sammeln und überwachen.
* Weitere Informationen zum [Konfigurieren von Webhooks Benachrichtigungen](insights-webhooks-alerts.md).
* Weitere Informationen zu [Azure Automatisierung Runbooks](..\automation\automation-starting-a-runbook.md).
* Erhalten Sie einen [Überblick über die Diagnoseprotokolle sammeln](monitoring-overview-of-diagnostic-logs.md) zum Sammeln von detaillierter häufig auftretenden Kennzahlen auf dem Dienst.
* Erhalten Sie einen [Überblick Kennzahlen Websitesammlung](insights-how-to-customize-monitoring.md) , um sicherzustellen, dass Ihr Dienst reagiert und verfügbar ist.

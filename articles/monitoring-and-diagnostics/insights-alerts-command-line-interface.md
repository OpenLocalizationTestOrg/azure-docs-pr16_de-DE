<properties
    pageTitle="Die Plattformen Command Line Interface (CLI) verwenden, um Benachrichtigungen für Azure-Dienste zu erstellen | Microsoft Azure"
    description="Verwenden Sie den Befehl Linie Benutzeroberfläche Azure Benachrichtigungen erstellen, die Benachrichtigungen oder Automatisierung auslösen können, wenn die angegebenen Bedingungen erfüllt sind."
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
    ms.date="10/24/2016"
    ms.author="robb"/>

# <a name="use-the-cross-platform-command-line-interface-cli-to-create-alerts-for-azure-services"></a>Verwenden Sie Plattformen Command Line Interface (CLI), um Benachrichtigungen für Azure-Dienste zu erstellen.

> [AZURE.SELECTOR]
- [Portal](insights-alerts-portal.md)
- [PowerShell](insights-alerts-powershell.md)
- [CLI](insights-alerts-command-line-interface.md)

## <a name="overview"></a>(Übersicht)

In diesem Artikel werden die zum Einrichten von Azure Benachrichtigungen Command Line Interface (CLI) verwenden.

>[AZURE.NOTE] Azure Monitor ist den neuen Namen für was "Azure Einsichten" aufgerufen wurde, bis zu 25 % September 2016. Die Namespaces und somit die folgenden Befehle enthalten jedoch weiterhin die "Einsichten".

Sie können eine Benachrichtigung basierend auf Überwachung Kennzahlen zur oder Ereignisse auf Ihre Azure-Dienste erhalten.

- **Metrische Werte** - der Warnung wird ausgelöst, wenn der Wert einer angegebenen Metrik einen Schwellenwert überschreitet, die, den Sie in eine beliebige Richtung zuweisen. D. h., löst beide Wenn zuerst die Bedingung erfüllt ist, und klicken Sie dann später an, die Bedingung ist nicht mehr erfüllt.    
- **Aktivität protokollieren von Ereignissen** – auf *jedes* Ereignis oder, nur, wenn eine bestimmte Anzahl von Ereignissen auftreten, kann eine Benachrichtigung auslösen.

Sie können eine Benachrichtigung, wenn Sie die folgenden Schritte ausführen, wenn es auslöst konfigurieren:

- Senden von e-Mail-Benachrichtigungen an die Dienstadministrator und Co-Administratoren
- Senden Sie e-Mail, um zusätzliche e-Mails, die Sie angeben.
- Anrufen eines webhook
- Starten der Ausführung einer Azure Runbooks (nur vom Azure-Portal zu diesem Zeitpunkt)

Sie konfigurieren können, und erhalten Informationen zum Verwenden von Warnungsregeln

- [Azure-portal](insights-alerts-portal.md)
- [PowerShell](insights-alerts-powershell.md)
- [line Interface (CLI)](insights-alerts-command-line-interface.md)
- [Azure Monitor REST-API](https://msdn.microsoft.com/library/azure/dn931945.aspx)


Sie können die Hilfe für Befehle immer erhalten, durch Eingabe von Befehlen und eingefügt - Hilfe am Ende. Beispiel:

    ```console
    azure insights alerts -help
    azure insights alerts actions email create -help
    ```

## <a name="create-alert-rules-using-the-cli"></a>Erstellen Sie mithilfe der CLI Warnungsregeln

1. Führen Sie die erforderlichen Komponenten und melden Sie sich Azure. Finden Sie unter [Azure Monitor CLI Beispiele](insights-cli-samples.md). Kurz gesagt, installieren Sie die CLI und führen Sie der folgenden Befehle aus. Sie erhalten Sie angemeldet, welches Abonnement Sie verwenden, und Sie Azure Monitor Befehle ausführen vorbereiten anzeigen.


    ```console
    azure login
    azure account show
    azure config mode arm

    ```

2.  Zum Auflisten vorhandener Regeln auf einer Ressourcengruppe verwenden Sie das folgende Formular **Azure Einblicken, dass Benachrichtigungen Liste Regel** *[Optionen] &lt;ResourceGroup&gt; *

    ```console
    azure insights alerts rule list myresourcegroupname

    ```
3. Um eine Regel zu erstellen, müssen Sie zuerst mehrere wichtige Informationsarten haben.
    - Die **Ressourcen-ID** für die Ressource festlegen eine Benachrichtigung für möchten
    - Die für diese Ressource verfügbaren **metrischen Definitionen**

    Eine Möglichkeit zum Abrufen der Ressource-ID besteht darin, das Azure-Portal zu verwenden. Wählen sie unter der Voraussetzung, dass die Ressource bereits erstellt wurde, im Portal. Wählen Sie dann in der nächsten Blade *Eigenschaften* im Abschnitt *Settings* . Die *Ressourcen-ID* ist ein Feld in der nächsten Blade. Eine weitere Möglichkeit besteht darin, der [Azure-Explorers](https://resources.azure.com/)verwenden.

    Ist eine Beispiel-Ressourcen-Id für eine Web app

    ```console
    /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename
    ```

    Um eine Liste der verfügbaren Kennzahlen und Einheiten für diese Kennzahlen für das vorherige Beispiel für die Ressource zu erhalten, verwenden Sie den folgenden CLI-Befehl ein:  

    ```console
    azure insights metrics list /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename PT1M
    ```

    _PT1M_ ist es sich um die Genauigkeit der verfügbaren Maßeinheit (1-Minuten-Intervallen). Verwenden unterschiedliche Granularität der bietet verschiedener metrischer Optionen.


4. Verwenden Sie zum Erstellen einer Metrik-basierten benachrichtigen Regel einen Befehl im folgenden Format ein:

    **Azure Einsichten Benachrichtigungen Regel Metrisch festlegen** *[Optionen] &lt;Regelname&gt; &lt;Speicherort&gt; &lt;ResourceGroup&gt; &lt;windowSize&gt; &lt;Operator&gt; &lt;Schwellenwert&gt; &lt;TargetResourceId&gt; &lt;MetricName&gt; &lt;TimeAggregationOperator&gt; *

    Im folgenden Beispiel wird von einer Benachrichtigung für eine Ressource für die Website. Die Benachrichtigung Trigger bei jeder konsistente Datenverkehr für 5 Minuten und beim Eingang von keine Datenverkehr für 5 Minuten.

    ```console
    azure insights alerts rule metric set myrule eastus myreasourcegroup PT5M GreaterThan 2 /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename BytesReceived Total

    ```

5. Zum Erstellen von Webhook, oder e-Mail senden, wenn eine Warnung ausgelöst wird, müssen Sie zuerst erstellen Sie die e-Mail- und/oder Webhooks. Erstellen Sie dann die Regel sofort danach. Sie können keine Webhook oder e-Mail-Nachrichten mit zuordnen bereits erstellten Regeln mithilfe der CLI.

    ```console
    azure insights alerts actions email create --customEmails myemail@contoso.com

    azure insights alerts actions webhook create https://www.contoso.com

    azure insights alerts rule metric set myrulewithwebhookandemail eastus myreasourcegroup PT5M GreaterThan 2 /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename BytesReceived Total
    ```


6. Verwenden Sie zum Erstellen einer Benachrichtigung, die wird ausgelöst, auf eine bestimmte Bedingung im Aktivitätsprotokoll das Formular aus:

    **Melden Sie Einsichten Benachrichtigungen Regel festlegen** *[Optionen] &lt;Regelname&gt; &lt;Speicherort&gt; &lt;ResourceGroup&gt; &lt;OperationName&gt; *

    Beispielsweise

    ```console
    azure insights alerts rule log set myActivityLogRule eastus myresourceGroupName Microsoft.Storage/storageAccounts/listKeys/action
    ```

    Die OperationName entspricht ein Ereignistyp für einen Eintrag in der Aktivität Log. Beispiele: *Microsoft.Compute/virtualMachines/delete* und *microsoft.insights/diagnosticSettings/write*.

    Den PowerShell-Befehl [Get-AzureRmProviderOperation](https://msdn.microsoft.com/library/mt603720.aspx) können Sie um eine Liste von möglichen OperationNames zu erhalten. Alternativ können Sie das Azure-Portal der Aktivität Log Abfragen und Auffinden bestimmter ältere Vorgänge, die Sie eine Benachrichtigung erstellen möchten. Die Vorgänge in der Protokollansicht Grafik von den Anzeigenamen angezeigt. Suchen Sie in den JSON für den Eintrag, und den Wert OperationName herausziehen.   

7. Um zu überprüfen, ob die Benachrichtigungen ordnungsgemäß erstellt wurde, indem Sie eine einzelne Regel aus.

    ```console
    azure insights alerts rule list myresourcegroup --ruleName myrule
    ```

8. Verwenden Sie einen Befehl des Formulars, um Regeln zu löschen:

    **Einsichten Benachrichtigungen Regel löschen** [Optionen] &lt;ResourceGroup&gt; &lt;Regelname&gt;

    Diese Befehle löschen Sie die Regeln, die zuvor in diesem Artikel erstellt.

    ```console
    azure insights alerts rule delete myresourcegroup myrule
    azure insights alerts rule delete myresourcegroup myrulewithwebhookandemail
    azure insights alerts rule delete myresourcegroup myActivityLogRule
    ```



## <a name="next-steps"></a>Nächste Schritte

* [Eine Übersicht über die Überwachung Azure](monitoring-overview.md) , einschließlich der Arten von Informationen können Sie sammeln und überwachen.
* Weitere Informationen zum [Konfigurieren von Webhooks Benachrichtigungen](insights-webhooks-alerts.md).
* Weitere Informationen zu [Azure Automatisierung Runbooks](..\automation\automation-starting-a-runbook.md).
* Erhalten Sie einen [Überblick über die Diagnoseprotokolle sammeln](monitoring-overview-of-diagnostic-logs.md) zum Sammeln von detaillierter häufig auftretenden Kennzahlen auf dem Dienst.
* Erhalten Sie einen [Überblick Kennzahlen Websitesammlung](insights-how-to-customize-monitoring.md) , um sicherzustellen, dass Ihr Dienst reagiert und verfügbar ist.

<properties
   pageTitle="Log Analytics benachrichtigen REST-API"
   description="Log Analytics benachrichtigen REST API können Sie zum Erstellen und Verwalten von Benachrichtigungen in Vorgänge Management Suite (OMS).  Dieser Artikel enthält Details der-API und mehrere Beispiele zum Ausführen von anderen Vorgängen."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="bwren" />

# <a name="log-analytics-alert-rest-api"></a>Log Analytics benachrichtigen REST-API

Log Analytics benachrichtigen REST API können Sie zum Erstellen und Verwalten von Benachrichtigungen in Vorgänge Management Suite (OMS).  Dieser Artikel enthält Details der-API und mehrere Beispiele zum Ausführen von anderen Vorgängen.

Die Log Analytics Suche REST-API RESTful ist und über die REST-API von Azure Ressourcenmanager zugegriffen werden kann. In diesem Dokument finden Sie Beispiele, die API zugegriffen wird, über die Befehlszeile PowerShell [ARMClient](https://github.com/projectkudu/ARMClient), eine open-Source-Befehlszeile-Tool, erleichtert Aufrufen der Azure Ressourcenmanager-API, verwenden. Die Verwendung von ARMClient und PowerShell ist eine der viele Optionen, um die Log Analytics suchen-API zugreifen. Mit diesen Tools können Sie die Rest Azure Ressourcenmanager API, damit Anrufe an OMS Arbeitsbereiche und Ausführen von Befehlen suchen, darin enthaltenen nutzen. Die API wird Suchergebnisse, um Sie im JSON-Format ausgegeben ermöglicht es Ihnen, die Suchergebnisse programmgesteuert auf verschiedene Weise verwendet.

## <a name="prerequisites"></a>Erforderliche Komponenten
Derzeit können Benachrichtigungen nur mit einer gespeicherten Suche in Log Analytics erstellt werden.  Sie können auf das [Protokoll suchen REST-API](log-analytics-log-search-api.md) Weitere Informationen verweisen.

## <a name="schedules"></a>Zeitpläne
Eine gespeicherte Suche kann einen oder mehrere Zeitpläne haben. Zeitplan definiert, wie oft die Suche ausführen und den Zeitraum aus, für die die Kriterien angegeben ist.
Zeitpläne verfügen über die Eigenschaften in der folgenden Tabelle.

| Eigenschaft  | Beschreibung |
|:--|:--|
| Intervall | Wie oft die Suche ausgeführt wird. Gemessen in Minuten. |
| QueryTimeSpan | Das Zeitintervall für die Kriterien ausgewertet wird. Muss gleich oder größer als Intervall sein. Gemessen in Minuten. |
| Version | Die API-Version verwendet wird.  Derzeit sollte dies immer auf 1 festgelegt werden. |

Angenommen Sie, eine Ereignisabfrage mit einem Intervall von 15 Minuten, und diese Zeitspanne von 30 Minuten. In diesem Fall würde Ausführen der Abfrage alle 15 Minuten, und eine Benachrichtigung würde ausgelöst wird, wenn die Kriterien zum Auflösen in true Over Fortsetzung eine Spanne 30 Minuten.

### <a name="retrieving-schedules"></a>Abrufen von Zeitplänen
Verwenden Sie die Get-Methode, um alle Zeitpläne für eine gespeicherte Suche abzurufen.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search  ID}/schedules?api-version=2015-03-20

Verwenden Sie die Get-Methode für eine ID, um einen bestimmten Zeitplan für eine gespeicherte Suche abzurufen.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}?api-version=2015-03-20

Es folgt eine Stichprobe Antwort für einen Zeitplan.

    {
        "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/MyWorkspace/savedSearches/0f0f4853-17f8-4ed1-9a03-8e888b0d16ec/schedules/a17b53ef-bd70-4ca4-9ead-83b00f2024a8",
        "etag": "W/\"datetime'2016-02-25T20%3A54%3A49.8074679Z'\"",
        "properties": {
        "Interval": 15,
        "QueryTimeSpan": 15
    }

### <a name="creating-a-schedule"></a>Erstellen eines Zeitplans
Verwenden Sie zum Erstellen eines neuen Projektplans verweigern mit der eine eindeutige ID.  Beachten Sie, dass zwei Zeitpläne dieselbe ID haben können, auch wenn sie mit anderen verknüpft sind gespeicherte Suchen.  Wenn Sie in der Verwaltungskonsole OMS einen Zeitplan erstellen, wird eine GUID für die Terminplan-ID erstellt.

    $scheduleJson = "{'properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/mynewschedule?api-version=2015-03-20 $scheduleJson

### <a name="editing-a-schedule"></a>Die Bearbeitung eines Zeitplans
Verwenden Sie verweigern mit einer vorhandenen Terminplan-ID für die gleichen gespeicherte Suche, um die Terminplan ändern.  Hauptteil der Anforderung muss das Etag des Terminplans beinhalten.

    $scheduleJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A49.8074679Z'\""','properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/mynewschedule?api-version=2015-03-20 $scheduleJson


### <a name="deleting-schedules"></a>Löschen von Zeitplänen
Verwenden Sie die Delete-Methode für eine ID, um einen Zeitplan zu löschen.

    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}?api-version=2015-03-20


## <a name="actions"></a>Aktionen
Ein Zeitplan kann mehrere Aktionen haben. Eine Aktion kann einen oder mehrere Prozesse ausführen, beispielsweise eine e-Mail senden oder ein Runbooks beginnen oder es möglicherweise einen Schwellenwert, der bestimmt, wann die Ergebnisse einer Suche einige Kriterien entsprechen definieren.  Einige Aktionen werden beide definieren, sodass die Prozesse ausgeführt werden, wenn der Schwellenwert erfüllt ist.

Alle Aktionen haben die Eigenschaften in der folgenden Tabelle.  Verschiedene Typen von Benachrichtigungen müssen verschiedene zusätzliche Eigenschaften die nachstehend beschrieben werden.

| Eigenschaft | Beschreibung |
|:--|:--|
| Typ | Geben Sie die Aktion ein.  Aktuell sind die möglichen Werte Benachrichtigung und Webhook. |
| Namen | Anzeigenamen für die Benachrichtigung. |
| Version | Die API-Version verwendet wird.  Derzeit sollte dies immer auf 1 festgelegt werden. |

### <a name="retrieving-actions"></a>Abrufen von Aktionen
Verwenden Sie die Get-Methode, um alle Aktionen für einen Zeitplan abzurufen.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search  ID}/schedules/{Schedule ID}/actions?api-version=2015-03-20

Verwenden Sie die Get-Methode mit der Aktion-ID, um eine bestimmte Aktion für einen Zeitplan abzurufen.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}/actions/{Action ID}?api-version=2015-03-20

### <a name="creating-or-editing-actions"></a>Erstellen oder Bearbeiten von Aktionen
Verwenden Sie die sich Methode, mit der Aktion-ID, die in den Terminplan zum Erstellen einer neuen Aktion eindeutig ist.  Wenn Sie eine Aktion in der Verwaltungskonsole OMS erstellen, ist eine GUID für die Aktion-ID.

Verwenden Sie verweigern mit einer vorhandenen Aktions-ID für die gleichen gespeicherte Suche, um die Terminplan ändern.  Hauptteil der Anforderung muss das Etag des Terminplans beinhalten.

Das Format der Anforderung zum Erstellen einer neuen Aktion hängt vom Aktionstyp ab, damit in diesen Beispielen in den folgenden Abschnitten bereitgestellt werden.

### <a name="deleting-actions"></a>Löschen von Aktionen
Verwenden Sie die Delete-Methode mit der Aktion-ID, um eine Aktion zu löschen.

    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}/Actions/{Action ID}?api-version=2015-03-20

### <a name="alert-actions"></a>Benachrichtigen Aktionen
Ein Zeitplan sollten nur einen einzigen benachrichtigen Aktion verfügen.  Benachrichtigen Aktionen verfügen über eine oder mehrere der folgenden Abschnitte in der folgenden Tabelle.  Jede wird im folgenden ausführlich beschrieben.

| Im Abschnitt | Beschreibung |
|:--|:--|
| Schwellenwert | Kriterien für die Ausführung der Aktion. |  
| EmailNotification | E-Mail an mehrere Empfänger zu senden. |
| Behebung | Starten einer Runbooks in Azure Automatisierung Versuch, einen identifizierten Problem zu beheben. |

#### <a name="thresholds"></a>Schwellenwerte
Eine Benachrichtigung Aktion sollte nur einen Schwellenwert haben.  Die Ergebnisse einer gespeicherten Suche den oberen Schwellenwert der Aktion zugeordnet sind, dass die Suche übereinstimmen, werden dann alle anderen Prozessen in die Aktion ausgeführt.  Eine Aktion kann auch nur einen Schwellenwert enthalten, damit sie mit anderen Arten Aktionen verwendet werden kann, die Schwellenwerte enthalten nicht.

Schwellenwerte verfügen über die Eigenschaften in der folgenden Tabelle.

| Eigenschaft | Beschreibung |
|:--|:--|
| Operator | Operator für den Schwellenwert für Vergleich. <br> Gt = größer als <br> Lt = kleiner als |
| Wert | Der Wert für den Schwellenwert. |

Betrachten Sie beispielsweise eine Ereignisabfrage mit einem Intervall von 15 Minuten, diese Zeitspanne von 30 Minuten und einen Schwellenwert von mehr als 10 aus. In diesem Fall würde Ausführen der Abfrage alle 15 Minuten, und eine Benachrichtigung würde ausgelöst wird, wenn er 10 Ereignisse zurückgegeben, die über einen Zeitraum 30 Minuten erstellt wurden.

Es folgt eine Beispielantwort für eine Aktion mit nur einem Schwellenwert.  

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My threshold action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "Version": 1
    }

Verwenden Sie die Methode sich mit einer Aktion eindeutige ID, um eine neue Schwellenwertaktion für einen Zeitplan erstellen.  

    $thresholdJson = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdJson

Verwenden Sie die Methode sich mit einer vorhandenen Aktions-ID, eine Schwellenwertaktion für einen Zeitplan zu ändern.  Hauptteil der Anforderung muss das Etag der Aktion beinhalten.

    $thresholdJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdJson

#### <a name="email-notification"></a>E-Mail-Benachrichtigung
E-Mail-Benachrichtigungen senden e-Mail an einen oder mehrere Empfänger.  Sie umfassen die Eigenschaften in der folgenden Tabelle aus.

| Eigenschaft | Beschreibung |
|:--|:--|
| Empfänger | Liste der e-Mail-Adressen. |
| Betreff | Der Betreff der e-Mail. |
| Anlage | Anlagen werden derzeit nicht unterstützt, sodass diese immer einen Wert von "Keine" besitzt. |

Es folgt eine Beispielantwort für eine e-Mail-Benachrichtigung Aktion mit einem Schwellenwert.  

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My email action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "EmailNotification": {
            "Recipients": [
                "recipient1@contoso.com",
                "recipient2@contoso.com"
            ],
            "Subject": "This is the subject",
            "Attachment": "None"
        },
        "Version": 1
    }

Verwenden Sie die Methode sich mit einer Aktion eindeutige ID, um eine neue e-Mail-Aktion für einen Zeitplan erstellen.  Im folgende Beispiel wird eine e-Mail-Benachrichtigung mit einem Schwellenwert erstellt, damit die e-Mail gesendet wird, wenn die Ergebnisse der gespeicherten Suche den Schwellenwert überschreiten.

    $emailJson = "{'properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myemailaction?api-version=2015-03-20 $ emailJson

Verwenden Sie verweigern mit einer vorhandenen Aktions-ID, um eine e-Mail-Aktion für einen Zeitplan zu ändern.  Hauptteil der Anforderung muss das Etag der Aktion beinhalten.

    $emailJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myemailaction?api-version=2015-03-20 $ emailJson

#### <a name="remediation-actions"></a>Von Behebungsaktionen
Bereinigungsstatus Starten einer Runbooks in Azure-Automatisierung, der versucht, zur Behebung des Problems identifiziert, indem Sie die Benachrichtigung.  Sie Erstellen einer Webhook für des Runbooks in einer Behebungsaktion verwendet werden, und geben Sie dann in der Eigenschaft WebhookUri den URI.  Wenn Sie diese Aktion mithilfe der Verwaltungskonsole OMS erstellen, wird eine neue Webhook für des Runbooks automatisch erstellt.

Bereinigungsstatus gehören die Eigenschaften in der folgenden Tabelle.

| Eigenschaft | Beschreibung |
|:--|:--|
| RunbookName | Name des Runbooks. Dieser muss eine veröffentlichten Runbooks im Automatisierung Konto so konfiguriert, dass die Automatisierung Lösung im Arbeitsbereich OMS übereinstimmen. |
| WebhookUri | URI des der Webhook.
| Ablauf | Das Ablaufdatum und die Uhrzeit von der Webhook.  Wenn die Webhook ein Benutzerkennwort hat, kann dies alle zukünftigen zulässiges Datum sein. |

Es folgt eine Beispielantwort für eine Behebungsaktion mit einem Schwellenwert.

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My remediation action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "Remediation": {
            "RunbookName": "My-Runbook",
            "WebhookUri": "https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d",
            "Expiry": "2018-02-25T18:27:20"
            },
        "Version": 1
    }

Verwenden Sie die Methode sich mit einer Aktion eindeutige ID, um eine neue Behebungsaktion für einen Zeitplan erstellen.  Im folgenden Beispiel wird eine Behebung mit einem Schwellenwert, damit die Runbooks gestartet wird, wenn die Ergebnisse der gespeicherten Suche den Schwellenwert überschreiten.

    $remediateJson = "{'properties': { 'Type':'Alert', 'Name': 'My Remediation Action', 'Version':'1', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'Remediation': {'RunbookName': 'My-Runbook', 'WebhookUri':'https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d', 'Expiry':'2018-02-25T18:27:20Z'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myremediationaction?api-version=2015-03-20 $remediateJson

Verwenden Sie verweigern mit einer vorhandenen Aktions-ID, um eine Behebungsaktion für einen Zeitplan zu ändern.  Hauptteil der Anforderung muss das Etag der Aktion beinhalten.

    $remediateJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Type':'Alert', 'Name': 'My Remediation Action', 'Version':'1', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'Remediation': {'RunbookName': 'My-Runbook', 'WebhookUri':'https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d', 'Expiry':'2018-02-25T18:27:20Z'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myremediationaction?api-version=2015-03-20 $remediateJson

#### <a name="example"></a>Beispiel
Es folgt ein vollständiges Beispiel zum Erstellen einer neuen e-Mail-Benachrichtigung.  Dadurch wird ein neues Projektplans sowie eine Aktion, die eine Schwellenwert und e-Mail-erstellt.

    $subscriptionId = "3d56705e-5b26-5bcc-9368-dbc8d2fafbfc"
    $workspaceId    = "MyWorkspace"
    $searchId       = "51cf0bd9-5c74-6bcb-927e-d1e9080b934e"

    $scheduleJson = "{'properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/$workspaceId/savedSearches/$searchId/schedules/myschedule?api-version=2015-03-20 $scheduleJson

    $thresholdJson = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/$workspaceId/savedSearches/$searchId/schedules/myschedule/actions/mythreshold?api-version=2015-03-20 $thresholdJson

    $emailJson = "{'properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/$workspaceId/savedSearches/$searchId/schedules/myschedule/actions/myemailaction?api-version=2015-03-20 $emailJson

### <a name="webhook-actions"></a>Webhook Aktionen
Webhook Aktionen Starten eines Prozesses durch Aufrufen einer URL und optional Bereitstellen einer Nutzlast gesendet werden.  Sie sind ähnlich wie Behebungsaktionen, außer er für Webhooks vorgesehen sind, die als Azure Automatisierung Runbooks Prozesse aufrufen können.  Darüber hinaus bieten zusätzliche Option zur Verfügung zu stellen eine Nutzlast an den remote-Prozess übermittelt werden konnte.

Webhook Aktionen verfügen nicht über einen Schwellenwert jedoch stattdessen sollte einen Zeitplan, der eine Benachrichtigung Aktion mit einem Schwellenwert hinzugefügt werden.  Sie können mehrere Webhook Aktionen hinzufügen, die alle ausgeführt wird, wenn der Schwellenwert erfüllt ist.

Webhook Aktionen gehören die Eigenschaften in der folgenden Tabelle.

| Eigenschaft | Beschreibung |
|:--|:--|
| WebhookUri | Der Betreff der e-Mail. |
| CustomPayload | Benutzerdefinierte Nutzlast der Webhook gesendet werden.  Das Format hängt davon ab, was der Webhook erwartet. |

Es folgt eine Stichprobe Antwort für Webhook Aktion und einer Benachrichtigung zugeordneten Aktion mit einem Schwellenwert.

    {
        "__metadata": {},
        "value": [
            {
                "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/bwren/savedSearches/2d1b30fb-7f48-4de5-9614-79ee244b52de/schedules/b80f5621-7217-4007-b32d-165d14377093/Actions/72884702-acf9-4653-bb67-f42436b342b4",
                "etag": "W/\"datetime'2016-02-26T20%3A25%3A00.6862124Z'\"",
                "properties": {
                    "Type": "Webhook",
                    "Name": "My Webhook Action",
                    "WebhookUri": "https://oaaswebhookdf.cloudapp.net/webhooks?token=VfkYTIlpk%2fc%2bJBP",
                    "CustomPayload": "{\"fielld1\":\"value1\",\"field2\":\"value2\"}",
                    "Version": 1
                }
            },
            {
                "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/bwren/savedSearches/2d1b30fb-7f48-4de5-9614-79ee244b52de/schedules/b80f5621-7217-4007-b32d-165d14377093/Actions/90a27cf8-71b7-4df2-b04f-54ed01f1e4b6",
                "etag": "W/\"datetime'2016-02-26T20%3A25%3A00.565204Z'\"",
                "properties": {
                    "Type": "Alert",
                    "Name": "Threshold for my webhook action",
                    "Threshold": {
                        "Operator": "gt",
                        "Value": 10
                    },
                    "Version": 1
                }
            }
        ]
    }

#### <a name="create-or-edit-a-webhook-action"></a>Erstellen oder Bearbeiten einer Aktion webhook
Verwenden Sie die Methode sich mit einer Aktion eindeutige ID, um eine neue Webhook Aktion für einen Zeitplan erstellen.  Im folgenden Beispiel wird eine Aktion Webhook und eine Benachrichtigung Aktion mit einem Schwellenwert, damit die Webhook ausgelöst wird, wenn die Ergebnisse der gespeicherten Suche den Schwellenwert überschreiten.

    $thresholdAction = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdAction

    $webhookAction = "{'properties': {'Type': 'Webhook', 'Name': 'My Webhook", 'WebhookUri': 'https://oaaswebhookdf.cloudapp.net/webhooks?token=VrkYTKlhk%2fc%2bKBP', 'CustomPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}', 'Version': 1 }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mywebhookaction?api-version=2015-03-20 $webhookAction

Verwenden Sie die Methode sich mit einer vorhandenen Aktions-ID, eine Aktion Webhook für einen Zeitplan zu ändern.  Hauptteil der Anforderung muss das Etag der Aktion beinhalten.

    $webhookAction = "{'etag': 'W/\"datetime'2016-02-26T20%3A25%3A00.6862124Z'\"','properties': {'Type': 'Webhook', 'Name': 'My Webhook", 'WebhookUri': 'https://oaaswebhookdf.cloudapp.net/webhooks?token=VrkYTKlhk%2fc%2bKBP', 'CustomPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}', 'Version': 1 }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mywebhookaction?api-version=2015-03-20 $webhookAction

## <a name="next-steps"></a>Nächste Schritte

- Verwenden Sie die [REST-API Log Suchvorgänge](log-analytics-log-search-api.md) in Log Analytics ein.

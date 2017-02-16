<properties
    pageTitle="Konfigurieren der Webhooks auf Azure metrischen Benachrichtigungen | Microsoft Azure"
    description="Einzusetzen Sie Azure Benachrichtigungen zu anderen Systemen nicht Azure."
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
    ms.date="09/15/2016"
    ms.author="ashwink"/>

# <a name="configure-a-webhook-on-an-azure-metric-alert"></a>Konfigurieren eines Webhook auf einer Azure metrischen Benachrichtigung

Webhooks ermöglichen es Ihnen, eine Azure Benachrichtigung an andere Systeme nach der Bearbeitung oder benutzerdefinierte Aktionen weiterleiten. Sie können einer Webhook auf eine Benachrichtigung zum Weiterleiten an Diensten, die SMS senden, Melden von Fehlern, ein Team per Chat/messaging-Dienste zu benachrichtigen, oder führen Sie eine beliebige Anzahl anderer Aktionen, verwenden. Dieser Artikel beschreibt, wie Sie eine Webhook Azure metrischen Benachrichtigung festlegen und der Inhalt für die HTTP-POST zu einer Webhook aussieht. Informationen zur Einrichtung und Schema für eine Protokolldatei Azure benachrichtigen (Benachrichtigung für Ereignisse), [stattdessen finden Sie auf dieser Seite](./insights-auditlog-to-webhook-email.md).

Azure Benachrichtigungen HTTP POST des Benachrichtigungssounds Inhalts in JSON formatieren, unten, um eine Webhook URI, die Sie bereitstellen, wenn Sie die Benachrichtigung erstellen definierte Schema. Dieser URI muss eine gültige HTTP oder HTTPS-Endpunkt. Azure Beiträge einen Eintrag pro Anforderung aus, wenn eine Warnung aktiviert ist.

## <a name="configuring-webhooks-via-the-portal"></a>Konfigurieren von Webhooks über das-portal

Sie können hinzufügen oder aktualisieren die Webhook URI im Bildschirm im [Portal](https://portal.azure.com/)Benachrichtigungen erstellen/aktualisieren.

![Hinzufügen einer Regel](./media/insights-webhooks-alerts/Alertwebhook.png)

Sie können auch eine Benachrichtigung für eine Webhook mithilfe der [PowerShell-Cmdlets Azure](./insights-powershell-samples.md#create-alert-rules), [Plattformen CLI](./insights-cli-samples.md#work-with-alerts)oder [Azure Monitor REST-API](https://msdn.microsoft.com/library/azure/dn933805.aspx)URI Posten konfigurieren.

## <a name="authenticating-the-webhook"></a>Die Webhook Authentifizierung

Die Webhook kann authentifizieren mithilfe einer der folgenden Methoden:

1. **Token-basierte Autorisierung** - URI der Webhook ist mit einem token-ID, z. b gespeichert. `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`
2.  **Grundlegende Autorisierung** - URI der Webhook ist mit einem Benutzernamen und Ihr Kennwort ein, z. b gespeichert. `https://userid:password@mysamplealert/webcallback?someparamater=somevalue&foo=bar`

## <a name="payload-schema"></a>Nutzlast schema

Der Vorgang Beitrag enthält die folgenden JSON-Aspekte und Schema für alle metrisch-basierte Alarme.

```JSON
{
"status": "Activated",
"context": {
            "timestamp": "2015-08-14T22:26:41.9975398Z",
            "id": "/subscriptions/s1/resourceGroups/useast/providers/microsoft.insights/alertrules/ruleName1",
            "name": "ruleName1",
            "description": "some description",
            "conditionType": "Metric",
            "condition": {
                        "metricName": "Requests",
                        "metricUnit": "Count",
                        "metricValue": "10",
                        "threshold": "10",
                        "windowSize": "15",
                        "timeAggregation": "Average",
                        "operator": "GreaterThanOrEqual"
                },
            "subscriptionId": "s1",
            "resourceGroupName": "useast",                                
            "resourceName": "mysite1",
            "resourceType": "microsoft.foo/sites",
            "resourceId": "/subscriptions/s1/resourceGroups/useast/providers/microsoft.foo/sites/mysite1",
            "resourceRegion": "centralus",
            "portalLink": "https://portal.azure.com/#resource/subscriptions/s1/resourceGroups/useast/providers/microsoft.foo/sites/mysite1"
},
"properties": {
              "key1": "value1",
              "key2": "value2"
              }
}
```


| Feld | Obligatorisch | Feste Satz von Werten | Notizen |
| :-------------| :-------------   | :-------------   | :-------------   |
|Status|Y|"Aktiviert", "Gelöst"|Status für die Benachrichtigung, wobei ein Bedingungen haben Sie festgelegt werden.|
|Kontextmenü| Y | | Der Kontext der Warnung.|
|Zeitstempel| Y | | Die Uhrzeit, an der die Benachrichtigung ausgelöst wurde.|
|ID | Y | | Jede Regel hat eine eindeutige Id an.|
|Namen               |Y                  |                   | Der Name der Warnung.|
|Beschreibung        |Y                  |                           |Beschreibung der Warnung.|
|conditionType      |Y                  |"Metrischen", "Event"          |Es werden zwei Arten von Benachrichtigungen unterstützt. Auf der Grundlage einer bedingungs metrischen und basierend auf ein Ereignis im Protokoll Aktivität. Verwenden Sie diesen Wert um zu prüfen, ob die Benachrichtigung auf Metrisch oder das Ereignis basiert aus.|
|Bedingung          |Y                  |                           | Bestimmte Felder überprüfen auf der Grundlage der ConditionType.|
|metricName         |für metrischen Benachrichtigungen  |                           |Der Name der Metrik, die definiert, was die Regel überwacht werden soll.|
|metricUnit         |für metrischen Benachrichtigungen  |"Bytes", "BytesPerSecond", "Anzahl", "CountPerSecond", "Prozent", "Sekunden"|     Die Einheit, in die Metrik zulässig sind. [Zulässige Werte sind hier aufgeführt](https://msdn.microsoft.com/library/microsoft.azure.insights.models.unit.aspx).|
|metricValue        |für metrischen Benachrichtigungen  |                           |Der tatsächliche Wert der Metrik, das die Benachrichtigung verursacht.|
|Schwellenwert          |für metrischen Benachrichtigungen  |                           |Der Schwellenwert, an dem die Benachrichtigung aktiviert ist.|
|windowSize         |für metrischen Benachrichtigungen  |                           |Der Zeitraum, die zum Überwachen der Aktivitäten in basierend auf den oberen Schwellenwert der Benachrichtigung verwendet wird. Muss zwischen 5 Minuten und 1 Tag. ISO 8601 Dauer-Format.|
|timeAggregation    |für metrischen Benachrichtigungen  |"Mittelwert", "Last", "Maximum", "Minimum", "Keine", "Summe" | Wie sollte die gesammelten Daten über einen Zeitraum kombiniert werden. Der Standardwert ist Mittelwert. [Zulässige Werte sind hier aufgeführt](https://msdn.microsoft.com/library/microsoft.azure.insights.models.aggregationtype.aspx).|
|Operator           |für metrischen Benachrichtigungen  |                           |Der Operator verwendet, um die aktuellen metrischen Daten dem Schwellenwert festlegen vergleichen.|
|subscriptionId     |Y                  |                           |Azure-Abonnement-ID an.|
|resourceGroupName  |Y                  |                           |Name der Ressourcengruppe für die betroffenen Ressource.|
|Ressourcenname       |Y                  |                           |Ressourcenname der betroffenen Ressource.|
|resourceType       |Y                  |                           |Ressourcenart der betroffenen Ressource.|
|resourceId         |Y                  |                           |Ressourcen-ID des betroffenen Ressource.|
|resourceRegion     |Y                  |                           |Region oder die Position des betroffenen Ressource.|
|portalLink         |Y                  |                           |Direkter Link zur Seite Zusammenfassung Portal Ressource.|
|Eigenschaften         |N                  |Optional                   |Festlegen von `<Key, Value>` Paare (d. h. `Dictionary<String, String>`), die Details zu dem Ereignis enthält. Das Feld "Eigenschaften" ist optional. In einem benutzerdefinierten Benutzeroberfläche oder -Logik app-basierte Workflow können Benutzer Key-Werte eingeben, die über die Nutzlast weitergegeben werden können. Alternative wie benutzerdefinierte Eigenschaften wieder in die Webhook weitergegeben wird über den Webhook-Uri selbst (als Abfrageparameter)|


>[AZURE.NOTE] Das Feld "Eigenschaften" kann nur mithilfe der [Azure Monitor REST-API](https://msdn.microsoft.com/library/azure/dn933805.aspx)festgelegt werden.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu Azure Benachrichtigungen und Webhooks video [Integrieren Azure Benachrichtigungen mit PagerDuty](http://go.microsoft.com/fwlink/?LinkId=627080)
- [Azure Benachrichtigungen Azure Automatisierung Skripts (Runbooks) ausführen](http://go.microsoft.com/fwlink/?LinkId=627081)
- [Verwenden von Logik App per SMS über Twilio aus einer Azure Warnung senden](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app)
- [Verwenden von Logik App aus einer Azure Warnung eine Pufferzeit Nachricht senden](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app)
- [Verwenden Sie zum Senden einer Nachricht mit einer Azure Warteschlange aus einer Azure Warnung Logik-App](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app)

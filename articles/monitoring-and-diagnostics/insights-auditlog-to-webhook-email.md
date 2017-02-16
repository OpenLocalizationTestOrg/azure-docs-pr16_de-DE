<properties
    pageTitle="Konfigurieren eines Webhook auf Azure Aktivität Log Benachrichtigungen | Microsoft Azure"
    description="Erfahren Sie, wie Aktivität Log Benachrichtigungen, mit Webhooks aufzurufen. "
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
    ms.date="10/20/2016"
    ms.author="ashwink"/>

# <a name="configure-a-webhook-on-an-azure-activity-log-alerts"></a>Konfigurieren eines Webhook auf einer Azure Aktivität Log-Benachrichtigungen

Webhooks ermöglichen es Ihnen, eine Azure Benachrichtigung an andere Systeme nach der Bearbeitung oder benutzerdefinierte Aktionen weiterleiten. Sie können einer Webhook auf eine Benachrichtigung zum Weiterleiten an Diensten, die SMS senden, Melden von Fehlern, ein Team per Chat/messaging-Dienste zu benachrichtigen, oder führen Sie eine beliebige Anzahl anderer Aktionen, verwenden. Dieser Artikel beschreibt, wie eine Webhook für eine Benachrichtigung Azure Aktivität Log festgelegt und der Inhalt für die HTTP-POST zu einer Webhook aussieht. Informationen über das Einrichten und das Schema für eine Azure metrischen Benachrichtigung, die [auf dieser Seite stattdessen finden Sie unter](insights-webhooks-alerts.md). Sie können auch eine Warnung aufzeichnen einrichten, zum Senden von e-Mail, wenn aktiviert.

>[AZURE.NOTE] Diese Funktion ist derzeit in der Vorschau, damit erwarten Variable Qualität und Leistung.

Sie können eine Aktivität Log Benachrichtigung mithilfe der [PowerShell-Cmdlets Azure](insights-powershell-samples.md#create-alert-rules), [Plattformen CLI](insights-cli-samples.md#work-with-alerts)oder [Azure Monitor REST-API](https://msdn.microsoft.com/library/azure/dn933805.aspx)einrichten.

## <a name="authenticating-the-webhook"></a>Die Webhook Authentifizierung
TThe Webhook kann Authentifizierung mithilfe einer der folgenden Methoden:

1. **Token-basierte Autorisierung** - URI der Webhook ist mit einem token-ID, z. b gespeichert. `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`
2.  **Grundlegende Autorisierung** - URI der Webhook ist mit einem Benutzernamen und Ihr Kennwort ein, z. b gespeichert. `https://userid:password@mysamplealert/webcallback?someparamater=somevalue&foo=bar`

## <a name="payload-schema"></a>Nutzlast schema
Der Vorgang Beitrag enthält die folgenden JSON-Aspekte und Schema für alle Aktivität Log-basierte Alarme. Dieses Schema ähnelt von metrisch-basierte Alarme verwendet wird.

```
{
        "status": "Activated",
        "context": {
                "resourceProviderName": "Microsoft.Web",
                "event": {
                        "$type": "Microsoft.WindowsAzure.Management.Monitoring.Automation.Notifications.GenericNotifications.Datacontracts.InstanceEventContext, Microsoft.WindowsAzure.Management.Mon.Automation",
                        "authorization": {
                                "action": "Microsoft.Web/sites/start/action",
                                "scope": "/subscriptions/s1/resourcegroups/rg1/providers/Microsoft.Web/sites/leoalerttest"
                        },
                        "eventDataId": "327caaca-08d7-41b1-86d8-27d0a7adb92d",
                        "category": "Administrative",
                        "caller": "myname@mycompany.com",
                        "httpRequest": {
                                "clientRequestId": "f58cead8-c9ed-43af-8710-55e64def208d",
                                "clientIpAddress": "104.43.166.155",
                                "method": "POST"
                        },
                        "status": "Succeeded",
                        "subStatus": "OK",
                        "level": "Informational",
                        "correlationId": "4a40beaa-6a63-4d92-85c4-923a25abb590",
                        "eventDescription": "",
                        "operationName": "Microsoft.Web/sites/start/action",
                        "operationId": "4a40beaa-6a63-4d92-85c4-923a25abb590",
                        "properties": {
                                "$type": "Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage",
                                "statusCode": "OK",
                                "serviceRequestId": "f7716681-496a-4f5c-8d14-d564bcf54714"
                        }
                },
                "timestamp": "Friday, March 11, 2016 9:13:23 PM",
                "id": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/alertrules/alertonevent2",
                "name": "alertonevent2",
                "description": "test alert on event start",
                "conditionType": "Event",
                "subscriptionId": "s1",
                "resourceId": "/subscriptions/s1/resourcegroups/rg1/providers/Microsoft.Web/sites/leoalerttest",
                "resourceGroupName": "rg1"
        },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
}
```

|Elementnamen       |Beschreibung|
|---                |---|
|Status             |Für metrischen Benachrichtigungen verwendet. Immer "aktiviert" für die Aktivität Log Benachrichtigungen festlegen.|
|Kontextmenü            |Kontext des Ereignisses.|
|resourceProviderName|Anbieter für die Ressourcen der betroffenen Ressource.|
|conditionType      |Immer "Event".|
|Namen               |Name der Regel benachrichtigen.|
|ID                 |Ressourcen-ID der Warnung.|
|Beschreibung        |Beschreibung der Warnung gemäß während der Erstellung der Warnung.|
|subscriptionId     |Azure-Abonnement-ID an.|
|Zeitstempel          |Uhrzeit, an dem das Ereignis vom Azure-Dienst erstellt wurde, die die Anforderung verarbeitet.|
|resourceId         |Ressourcen-ID des betroffenen Ressource.|
|resourceGroupName  |Name der Ressourcengruppe für die betroffenen Ressource|
|Eigenschaften         |Festlegen von `<Key, Value>` Paare (d. h. `Dictionary<String, String>`), die Details zu dem Ereignis enthält.|
|Ereignis              |Element mit Metadaten, die über das Ereignis.|
|Autorisierung      |Die RBAC Eigenschaften des Ereignisses. Hierzu gehören normalerweise "Aktion", "Rolle" und "Bereich".|
|Kategorie           |Kategorie des Ereignisses. Unterstützte Werte einzubeziehen: Administrative, benachrichtigen, Sicherheit, ServiceHealth, Empfehlungen.|
|Anrufer             |E-Mail-Adresse des Benutzers, der den Vorgang, eine UPN-Anspruch oder SPN anfordern, die auf Grundlage der Verfügbarkeit ausgeführt. Kann für bestimmte Anrufe System null sein.|
|correlationId      |In der Regel eine GUID im Zeichenfolgenformat. Ereignisse mit CorrelationId dieselbe größere Aktion angehören und in der Regel eine CorrelationId freigeben.|
|eventDescription   |Statischen Text Beschreibung des Ereignisses.|
|eventDataId        |Eindeutiger Bezeichner für das Ereignis.|
|Quelle        |Name des Azure Service oder Infrastruktur, die das Ereignis generiert.|
|httpRequest        |In der Regel enthält "ClientRequestId", "ClientIpAddress" und "Methode" (HTTP-Methode z. B. setzen).|
|Ebene              |Die folgenden Werte: "Kritisch", "Zurück", "Warnung", "Information" und "Ausführlich".|
|operationId        |In der Regel eine GUID Weitergabe an die Ereignisse, die einem Vorgang entspricht.|
|operationName      |Name des Vorgangs.|
|Eigenschaften         |Eigenschaften des Ereignisses.|
|Status             |Zeichenfolge. Der Status des Vorgangs. Allgemeine Werte einzubeziehen: "Schritte", "In Bearbeitung", "Wurde erfolgreich abgeschlossen", "Fehler bei", "Aktiv", "Gelöst".|
|untergeordnete Status          |Normalerweise enthält den HTTP-Status des Anrufs REST entsprechenden Code ein. Es kann auch andere zur Beschreibung einer untergeordneter Zeichenfolgen enthalten. Allgemeine untergeordneter Werte einzubeziehen: OK (HTTP-Status Code: 200), erstellten (HTTP-Status Code: 201), zulässigen (HTTP-Status Code: 202), nicht Inhalt (HTTP-Status Code: 204), ungültige Anforderung (HTTP-Status Code: 400), nicht gefunden (HTTP-Status Code: 404), Konflikt (HTTP-Status Code: 409), interner Serverfehler (HTTP-Status Code: 500), Dienst nicht verfügbar (HTTP-Status Code: 503), Gateway-Timeout (HTTP-Status Code: 504)|

## <a name="next-steps"></a>Nächste Schritte
- [Erfahren Sie mehr über das Protokoll Aktivität](monitoring-overview-activity-logs.md)
- [Azure Benachrichtigungen Azure Automatisierung Skripts (Runbooks) ausführen](http://go.microsoft.com/fwlink/?LinkId=627081)
- [Verwenden von Logik App per SMS über Twilio aus einer Azure Warnung zu senden](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app). In diesem Beispiel ist für metrischen Benachrichtigungen, aber konnte zum Arbeiten mit einer Aktivität Log Benachrichtigung geändert werden.
- [Verwenden von Logik App eine Pufferzeit Nachricht von einer Azure Benachrichtigung gesendet](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app). In diesem Beispiel ist für metrischen Benachrichtigungen, aber konnte zum Arbeiten mit einer Aktivität Log Benachrichtigung geändert werden.
- [Verwenden von Logik App zum Senden einer Nachricht mit einer Azure Warteschlange aus Azure Alert](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app). In diesem Beispiel ist für metrischen Benachrichtigungen, aber konnte zum Arbeiten mit einer Aktivität Log Benachrichtigung geändert werden.

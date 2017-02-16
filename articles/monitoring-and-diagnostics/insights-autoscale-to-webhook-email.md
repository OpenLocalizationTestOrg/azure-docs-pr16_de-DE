<properties
    pageTitle="Verwenden Sie zum Senden von e-Mail- und Webhook-Benachrichtigung automatisch skalieren Aktionen. | Microsoft Azure"
    description="Finden Sie unter Verwenden von Aktionen automatisch skalieren anrufen Web-URLs oder Senden von e-Mail-Benachrichtigungen in Azure überwachen. "
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
    ms.date="07/19/2016"
    ms.author="ashwink"/>

# <a name="use-autoscale-actions-to-send-email-and-webhook-alert-notifications-in-azure-monitor"></a>Verwenden Sie automatisch skalieren Aktionen, um e-Mail- und Webhook-Benachrichtigung in Azure Monitor zu senden.

In diesem Artikel zeigt, wie Trigger eingerichtet, damit Sie bestimmte Web-URLs anrufen oder basierend auf automatisch skalieren Aktionen in Azure-e-Mails senden können.  

## <a name="webhooks"></a>Webhooks
Webhooks können Sie die Azure-Benachrichtigung an andere Systeme nach der Bearbeitung oder benutzerdefinierten Benachrichtigungen weiterleiten. Beispielsweise routing der Benachrichtigung zum Dienste, die eine eingehenden Web Anforderung zum Senden von SMS, Log-Fehlern benachrichtigt, dass ein Team mithilfe von Chat oder messaging services verarbeitet werden können. Die Webhook URI muss ein gültiger HTTP oder HTTPS-Endpunkt.

## <a name="email"></a>E-Mail
E-Mails kann an einer beliebigen gültigen e-Mail-Adresse gesendet werden. Administratoren und Co-Administratoren des Abonnements, in dem die Regel ausgeführt wird, werden auch benachrichtigt werden.


## <a name="cloud-services-and-web-apps"></a>Cloud Services und Web Apps
Sie können vom Azure-Portal für Cloud Services und Server-Farmen (Web Apps) Teilnahme.

- Wählen Sie die Metrik **nach skalieren** aus.

![durch Skalieren](./media/insights-autoscale-to-webhook-email/insights-autoscale-scale-by.png)

## <a name="virtual-machine-scale-sets"></a>Virtuellen Computern skalieren Datensätze
Für neuere erstellte Maschinen mit Ressourcenmanager (virtuellen Computern skalieren Sets) können Sie dies konfigurieren mit REST-API, Ressourcenmanager Vorlagen, PowerShell und CLI. Eine Portal-Oberfläche ist noch nicht verfügbar.
Wenn Sie die REST-API oder Ressourcenmanager Vorlage verwenden, gehören Sie das Element Benachrichtigungen mit den folgenden Optionen.

```
"notifications": [
      {
        "operation": "Scale",
        "email": {
          "sendToSubscriptionAdministrator": false,
          "sendToSubscriptionCoAdministrators": false,
          "customEmails": [
              "user1@mycompany.com",
              "user2@mycompany.com"
              ]
        },
        "webhooks": [
          {
            "serviceUri": "https://foo.webhook.example.com?token=abcd1234",
            "properties": {
              "optional_key1": "optional_value1",
              "optional_key2": "optional_value2"
            }
          }
        ]
      }
    ]
```
|Feld                              |Obligatorisch? |Beschreibung|
|---                                |---        |---|
|Vorgang                          |Ja        |Wert muss "Skalierung"|
|sendToSubscriptionAdministrator    |Ja        |Wert muss auf "true" oder "false" sein.|
|sendToSubscriptionCoAdministrators |Ja        |Wert muss auf "true" oder "false" sein.|
|customEmails                       |Ja        |Wert kann null [] oder String-Array von e-Mails sein.|
|webhooks                           |Ja        |Wert kann null oder eine gültige Uri sein.|
|serviceUri                         |Ja        |eine gültige Https-Uri|
|Eigenschaften                         |Ja        |Wert muss leeren {} oder kann Schlüssel-Wert-Paare enthalten|


## <a name="authentication-in-webhooks"></a>Authentifizierung in webhooks
Es gibt zwei Arten von Authentifizierung URI aus:

1. Token zur Basis Authentifizierung, in dem Sie die Webhook URI mit einem token-ID als Abfrageparameter speichern. Beispielsweise https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue
2. Standardauthentifizierung, wo Sie einen Benutzer-ID und ein Kennwort verwenden. Beispielsweisehttps://userid:password@mysamplealert/webcallback?someparamater=somevalue&parameter=value

## <a name="autoscale-notification-webhook-payload-schema"></a>Automatisch skalieren Benachrichtigung Webhook Nutzlast schema
Wenn die Benachrichtigung automatisch skalieren generiert wird, ist die folgende Metadaten in der Webhook Nutzlast enthalten:

```
{
        "version": "1.0",
        "status": "Activated",
        "operation": "Scale In",
        "context": {
                "timestamp": "2016-03-11T07:31:04.5834118Z",
                "id": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/autoscalesettings/myautoscaleSetting",
                "name": "myautoscaleSetting",
                "details": "Autoscale successfully started scale operation for resource 'MyCSRole' from capacity '3' to capacity '2'",
                "subscriptionId": "s1",
                "resourceGroupName": "rg1",
                "resourceName": "MyCSRole",
                "resourceType": "microsoft.classiccompute/domainnames/slots/roles",
                "resourceId": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.classicCompute/domainNames/myCloudService/slots/Production/roles/MyCSRole",
                "portalLink": "https://portal.azure.com/#resource/subscriptions/s1/resourceGroups/rg1/providers/microsoft.classicCompute/domainNames/myCloudService",
                "oldCapacity": "3",
                "newCapacity": "2"
        },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
}
```


|Feld  |Obligatorisch?|    Beschreibung|
|---|---|---|
|Status |Ja    |Der Status, der angibt, dass eine Aktion automatisch skalieren generiert wurde|
|Vorgang| Ja |Für eine Erhöhung der Instanzen es werden "Skalierung gewählt" und für eine Verschlechterung der Instanzen, werden sich "Skalierung In"|
|Kontextmenü|   Ja |Kontext Aktion automatisch skalieren|
|Zeitstempel| Ja |Zeitstempel, wenn die Aktion automatisch skalieren ausgelöst wurde|
|ID |Ja|   Die Einstellung automatisch skalieren Ressourcenmanager-ID|
|Namen   |Ja|   Der Name der Einstellung automatisch skalieren|
|Details|   Ja |Erläuterung der Aktion, die der Dienst automatisch skalieren durchgeführt wurde und die Änderung in die Anzahl der Instanzen|
|subscriptionId|    Ja |Die Zielressource, die skaliert werden, wird Abonnement-ID|
|resourceGroupName| Ja|    Ressourcengruppe Namen der Zielressource, die skaliert werden, wird|
|Ressourcenname   |Ja|   Namen der Zielressource, die skaliert werden, wird|
|resourceType   |Ja|   Die drei unterstützten Werte: "microsoft.classiccompute/domainnames/slots/roles" - Cloud-Dienst Rollen, "microsoft.compute/virtualmachinescalesets" - virtuellen Computern skalieren Sätze und "Microsoft.Web/serverfarms" - Web App|
|resourceId |Ja|Ressourcenmanager-ID der Zielressource, die skaliert werden, wird|
|portalLink |Ja    |Azure Portals Link zur Seite Zusammenfassung der Zielressource|
|oldCapacity|   Ja |Die aktuelle (alte) Anzahl der Instanzen beim Skalieren eine Maßstab-Aktion ausgeführt hat.|
|newCapacity|   Ja |Die Anzahl der neuen Instanzen, der automatisch skalieren die Ressource skaliert|
|Eigenschaften|    Nein| Optional. Festlegen von < Key Wert > Paare (z. B. Wörterbuch < String, String >). Das Feld "Eigenschaften" ist optional. In einer benutzerdefinierten Benutzeroberfläche oder Logik app-basierte Workflow können Sie eingeben, Schlüssel und Werte, die mit der Nutzlast übergeben werden können. Alternativ können benutzerdefinierte Eigenschaften wieder an den ausgehenden Anruf Webhook weitergegeben besteht darin, die Webhook (als Abfrageparameter) URI verwenden|

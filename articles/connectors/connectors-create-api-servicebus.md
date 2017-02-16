<properties
pageTitle="Erfahren Sie, mit der Azure-Dienstbus Verbinder in Ihrer apps Logik | Microsoft Azure"
description="Erstellen Sie Logik apps mit Azure-App-Dienst an. Verbinden Sie mit Azure Service Bus zu senden und empfangen. Sie können die Aktionen wie Senden zur Warteschlange, zum Thema senden, aus der Warteschlange empfangen, und die von Abonnements erhalten."
services="logic-apps"
documentationCenter=".net,nodejs,java"  
authors="msftman"
manager="erikre"
editor=""
tags="connectors" />

<tags
ms.service="logic-apps"
ms.devlang="multiple"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="08/02/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-azure-service-bus-connector"></a>Erste Schritte mit der Azure-Dienstbus Verbinder

Verbinden Sie mit Azure Service Bus zu senden und empfangen. Sie können die Aktionen wie Senden zur Warteschlange, zum Thema senden, aus der Warteschlange empfangen, und die von Abonnements erhalten.

Um [alle Verbinder](./apis-list.md)verwenden zu können, müssen Sie zuerst eine app Logik zu erstellen. Sie können durch [Erstellen einer Logik app jetzt](../app-service-logic/app-service-logic-create-a-logic-app.md)loslegen.

## <a name="connect-to-service-bus"></a>Herstellen einer Verbindung Dienstbus mit

Bevor Sie Ihre app Logik Dienste zugreifen kann, müssen Sie zuerst eine Verbindung mit dem Dienst herzustellen. Eine [Verbindung](./connectors-overview.md) stellt eine Verbindung zwischen einer app Logik und einem anderen Dienst.  

>[AZURE.INCLUDE [Steps to create a connection to Azure Service Bus](../../includes/connectors-create-api-servicebus.md)]

## <a name="use-a-service-bus-trigger"></a>Verwenden eines Triggers Dienstbus

Ein Trigger ist ein Ereignis, das zum Starten des Workflows in einer app Logik definiert verwendet werden kann. [Erfahren Sie mehr über Trigger](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).  

>[AZURE.INCLUDE [Steps to create a Service Bus trigger](../../includes/connectors-create-api-servicebus-trigger.md)]  

## <a name="use-a-service-bus-action"></a>Verwenden Sie eine Aktion Dienstbus

Eine Aktion ist ein Vorgang durchgeführten durch den Workflow in einer app Logik definiert. [Erfahren Sie mehr über Aktionen](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

[AZURE.INCLUDE [Steps to create a Service Bus action](../../includes/connectors-create-api-servicebus-action.md)]  

## <a name="technical-details"></a>Technische details

Hier sind die Details der Trigger, Aktionen und Antworten, die diese Verbindung unterstützt.

### <a name="service-bus-triggers"></a>Dienstbus Trigger

Dienstbus weist die folgenden Trigger:  

|Trigger | Beschreibung|
|--- | ---|
|[Wenn eine Nachricht in einer Warteschlange empfangen](connectors-create-api-servicebus.md#when-a-message-is-received-in-a-queue)|Dieser Vorgang löst einen Fluss aus, wenn eine Nachricht in einer Warteschlange empfangen wird.|
|[Wenn eine Nachricht in einem Thema Abonnement empfangen](connectors-create-api-servicebus.md#when-a-message-is-received-in-a-topic-subscription)|Dieser Vorgang löst einen Fluss aus, wenn eine Nachricht in einem Thema Abonnement empfangen wird.|


### <a name="service-bus-actions"></a>Dienstbus Aktionen

Dienstbus weist die folgenden Aktionen aus:


|Aktion|Beschreibung|
|--- | ---|
|[Senden der Nachricht](connectors-create-api-servicebus.md#send-message)|Dieser Vorgang sendet eine Nachricht an eine Warteschlange oder ein Thema.|
### <a name="action-and-trigger-details"></a>Details Aktion und trigger

Hier sind die Details für die Aktionen und Trigger für diesen Connector, zusammen mit ihren Antworten aus.



#### <a name="send-message"></a>Senden der Nachricht

|Eigenschaftsname| Anzeigename|Beschreibung|
| ---|---|---|
|ContentData *|Inhalt|Inhalt der Nachricht.|
|ContentType|Inhaltstyp|Inhaltstyp der Inhalt der Nachricht.|
|Eigenschaften|Eigenschaften|Schlüssel / Wert-Paare für jeden vermittelte Eigenschaft.|
|Name *|Name der Warteschlange/Thema|Name der Warteschlange oder des Themas.|

Diese erweiterten Parameter stehen auch zur Verfügung:

|Eigenschaftsname| Anzeigename|Beschreibung|
| ---|---|---|
|Nachrichten-ID|Nachrichten-Id|Ein benutzerdefinierter Wert, den Dienstbus zum Identifizieren von doppelter Nachrichten verwenden können, wenn aktiviert.|
|An|An|Adresse, um Sie zu senden.|
|ReplyTo|Senden einer Antwort an|Die Adresse der Warteschlange antworten möchten.|
|ReplyToSessionId|Antworten Sie auf die Id für eine Sitzung|Bezeichner der Sitzung zu antworten.|
|Beschriftung|Beschriftung|Anwendungsspezifische Bezeichnung.|
|ScheduledEnqueueTimeUtc|ScheduledEnqueueTimeUtc|Datum und Uhrzeit in UTC, wenn die Nachricht in der Warteschlange hinzugefügt werden.|
|SessionId|Id für eine Sitzung|Bezeichner der Sitzung.|
|CorrelationId|Korrelations-Id|Der Korrelations-ID.|
|TimeToLive|Time To Live|Die Dauer in Teilstriche, dass eine Nachricht gültig ist. Die Dauer wird aus, wenn die Nachricht an Dienstbus gesendet wird gestartet.|



Ein * zeigt an, dass eine Eigenschaft erforderlich ist.


#### <a name="when-a-message-is-received-in-a-queue"></a>Wenn eine Nachricht in einer Warteschlange empfangen

|Eigenschaftsname| Anzeigename|Beschreibung|
| ---|---|---|
|QueueName *|Name der Warteschlange|Name der Warteschlange.|


Ein * zeigt an, dass eine Eigenschaft erforderlich ist.


##### <a name="output-details"></a>Die Ausgabedetails

ServiceBusMessage: Dieses Objekt verfügt über den Inhalt und die Eigenschaften einer Nachricht Dienstbus.


| Eigenschaftsname | Datentyp | Beschreibung |
|---|---|---|
|ContentData|Zeichenfolge|Inhalt der Nachricht.|
|ContentType|Zeichenfolge|Inhaltstyp der Inhalt der Nachricht.|
|Eigenschaften|Objekt|Schlüssel / Wert-Paare für jeden vermittelte Eigenschaft.|
|Nachrichten-ID|Zeichenfolge|Ein benutzerdefinierter Wert, den Dienstbus zum Identifizieren von doppelter Nachrichten verwenden können, wenn aktiviert.|
|An|Zeichenfolge|Adresse senden.|
|ReplyTo|Zeichenfolge|Die Adresse der Warteschlange antworten möchten.|
|ReplyToSessionId|Zeichenfolge|Bezeichner der Sitzung zu antworten.|
|Beschriftung|Zeichenfolge|Anwendungsspezifische Bezeichnung.|
|ScheduledEnqueueTimeUtc|Zeichenfolge|Datum und Uhrzeit in UTC, wenn die Nachricht in der Warteschlange hinzugefügt werden.|
|SessionId|Zeichenfolge|Bezeichner der Sitzung.|
|CorrelationId|Zeichenfolge|Der Korrelations-ID.|
|TimeToLive|Zeichenfolge|Die Dauer in Teilstriche, dass eine Nachricht gültig ist. Die Dauer wird aus, wenn die Nachricht an Dienstbus gesendet wird gestartet.|




#### <a name="when-a-message-is-received-in-a-topic-subscription"></a>Wenn eine Nachricht in einem Thema Abonnement empfangen

|Eigenschaftsname| Anzeigename|Beschreibung|
| ---|---|---|
|Themenname *|Name des Themas|Name des Themas.|
|SubscriptionName *|Abonnementname Thema|Name des Abonnements Thema.|


Ein * zeigt an, dass eine Eigenschaft erforderlich ist.


##### <a name="output-details"></a>Die Ausgabedetails

ServiceBusMessage: Dieses Objekt verfügt über den Inhalt und die Eigenschaften einer Nachricht Dienstbus.


| Eigenschaftsname | Datentyp | Beschreibung |
|---|---|---|
|ContentData|Zeichenfolge|Inhalt der Nachricht.|
|ContentType|Zeichenfolge|Inhaltstyp der Inhalt der Nachricht.|
|Eigenschaften|Objekt|Schlüssel / Wert-Paare für jeden vermittelte Eigenschaft.|
|Nachrichten-ID|Zeichenfolge|Ein benutzerdefinierter Wert, den Dienstbus zum Identifizieren von doppelter Nachrichten verwenden können, wenn aktiviert.|
|An|Zeichenfolge|Adresse senden.|
|ReplyTo|Zeichenfolge|Die Adresse der Warteschlange antworten möchten.|
|ReplyToSessionId|Zeichenfolge|Bezeichner der Sitzung zu antworten.|
|Beschriftung|Zeichenfolge|Anwendungsspezifische Bezeichnung.|
|ScheduledEnqueueTimeUtc|Zeichenfolge|Datum und Uhrzeit in UTC, wenn die Nachricht in der Warteschlange hinzugefügt werden.|
|SessionId|Zeichenfolge|Bezeichner der Sitzung.|
|CorrelationId|Zeichenfolge|Der Korrelations-ID.|
|TimeToLive|Zeichenfolge|Die Dauer in Teilstriche, dass eine Nachricht gültig ist. Die Dauer wird aus, wenn die Nachricht an Dienstbus gesendet wird gestartet.|



### <a name="http-responses"></a>HTTP-Antworten

Die vorherige Aktionen und Trigger können eine oder mehrere der folgenden HTTP Statuscodes zurückgeben:

|Namen|Beschreibung|
|---|---|
|200|Okay|
|202|Akzeptiert|
|400|Ungültige Anforderung|
|401|Nicht autorisierte|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Es ist ein Fehler aufgetreten.|
|Standard|Fehler bei Vorgang.|

## <a name="next-steps"></a>Nächste Schritte
[Erstellen einer app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).

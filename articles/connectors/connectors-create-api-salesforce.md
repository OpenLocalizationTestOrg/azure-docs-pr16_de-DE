<properties
pageTitle="Erfahren Sie, den Vertrieb Verbinder in Ihrer apps Logik verwenden | Microsoft Azure"
description="Erstellen Sie Logik apps mit Azure-App-Dienst an. Der Verbinder Vertrieb stellt eine API für die Arbeit mit Vertrieb Objekte bereit."
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
ms.date="10/05/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-salesforce-connector"></a>Erste Schritte mit den Vertrieb Verbinder

Der Verbinder Vertrieb stellt eine API für die Arbeit mit Vertrieb Objekte bereit.

Um [alle Verbinder](./apis-list.md)verwenden zu können, müssen Sie zuerst eine app Logik zu erstellen. Sie können durch [Erstellen einer Logik app jetzt](../app-service-logic/app-service-logic-create-a-logic-app.md)loslegen.

## <a name="connect-to-salesforce-connector"></a>Herstellen einer Verbindung Vertrieb Verbinder mit

Bevor Sie Ihre app Logik Dienste zugreifen kann, müssen Sie zuerst eine *Verbindung* mit dem Dienst erstellen. Eine [Verbindung](./connectors-overview.md) stellt eine Verbindung zwischen einer app Logik und einem anderen Dienst.  

### <a name="create-a-connection-to-salesforce-connector"></a>Herstellen einer Verbindung mit Vertrieb Verbinder

>[AZURE.INCLUDE [Steps to create a connection to Salesforce Connector](../../includes/connectors-create-api-salesforce.md)]

## <a name="use-a-salesforce-connector-trigger"></a>Verwenden eines Vertrieb Verbinder Triggers

Ein Trigger ist ein Ereignis, das zum Starten des Workflows in einer app Logik definiert verwendet werden kann. [Erfahren Sie mehr über Trigger](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

>[AZURE.INCLUDE [Steps to create a Salesforce trigger](../../includes/connectors-create-api-salesforce-trigger.md)]

## <a name="add-a-condition"></a>Hinzufügen einer bedingungs 
>[AZURE.INCLUDE [Steps to create a Salesforce condition](../../includes/connectors-create-api-salesforce-condition.md)]

## <a name="use-a-salesforce-connector-action"></a>Verwenden einer Vertrieb Verbinder Aktion

Eine Aktion ist ein Vorgang durchgeführten durch den Workflow in einer app Logik definiert. [Erfahren Sie mehr über Aktionen](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

>[AZURE.INCLUDE [Steps to create a Salesforce action](../../includes/connectors-create-api-salesforce-action.md)]

## <a name="technical-details"></a>Technische details

Hier sind die Details der Trigger, Aktionen und Antworten, die diese Verbindung unterstützt:

## <a name="salesforce-connector-triggers"></a>Vertrieb Verbinder Trigger

Vertrieb Verbinder besteht aus die folgenden Triggern:  

|Auslösen | Beschreibung|
|--- | ---|
|[Bei der Erstellung eines Objekts](connectors-create-api-salesforce.md#when-an-object-is-created)|Dieser Vorgang löst einen Fluss aus, wenn ein Objekt erstellt wird.|
|[Wenn ein Objekt geändert wird.](connectors-create-api-salesforce.md#when-an-object-is-modified)|Dieser Vorgang löst einen Fluss aus, wenn ein Objekt geändert wird.|


## <a name="salesforce-connector-actions"></a>Vertrieb Verbinder Aktionen

Vertrieb Verbinder weist die folgenden Aktionen aus:


|Aktion|Beschreibung|
|--- | ---|
|[Abrufen von Objekten](connectors-create-api-salesforce.md#get-objects)|Lassen Vorgang ruft Objekte für einen bestimmten Objekttyp wie "Lead" ab.|
|[Objekt erstellen](connectors-create-api-salesforce.md#create-object)|Dieser Vorgang erstellt ein Objekt.|
|[Erste Objekt](connectors-create-api-salesforce.md#get-object)|Dieser Vorgang ruft ein Objekt ab.|
|[Objekt löschen](connectors-create-api-salesforce.md#delete-object)|Dieser Vorgang löscht ein Objekt.|
|[Objekt aktualisieren](connectors-create-api-salesforce.md#update-object)|Dieser Vorgang ist ein Objekt aktualisiert.|
|[Abrufen von Objekttypen](connectors-create-api-salesforce.md#get-object-types)|Dieser Vorgang Listet die verfügbaren Objekttypen.|
### <a name="action-details"></a>Aktionsdetails

Hier sind die Details für die Aktionen und Trigger für diesen Connector, zusammen mit ihren Antworten:



### <a name="get-objects"></a>Abrufen von Objekten
Lassen Vorgang ruft Objekte für einen bestimmten Objekttyp wie "Lead" ab. 


|Eigenschaftsname| Anzeigename|Beschreibung|
| ---|---|---|
|Tabelle *|Objekttyp|Vertrieb Objekttyp wie "Lead"|
|$filter|Filtern der Abfrage|Die Abfrage ein ODATA Filter zum Einschränken der Anzahl von Einträgen|
|$orderby|Sortiert nach|Eine ODATA OrderBy Abfrage zum Festlegen der Reihenfolge von Einträgen|
|$skip|Überspringen zählen|Anzahl von Einträgen, überspringen (Standard = 0)|
|$top|Abrufen der maximalen Anzahl|Maximale Anzahl von Einträgen zum Abrufen (Standard = 256)|

Ein * zeigt an, dass eine Eigenschaft erforderlich ist

#### <a name="output-details"></a>Die Ausgabedetails

ItemsList


| Eigenschaftsname | Datentyp |
|---|---|
|Wert|Matrix|




### <a name="create-object"></a>Objekt erstellen
Dieser Vorgang erstellt ein Objekt. 


|Eigenschaftsname| Anzeigename|Beschreibung|
| ---|---|---|
|Tabelle *|Objekttyp|Objekttyp wie "Lead"|
|Element *|Objekt|Objekt erstellen|

Ein * zeigt an, dass eine Eigenschaft erforderlich ist

#### <a name="output-details"></a>Die Ausgabedetails

Element


| Eigenschaftsname | Datentyp |
|---|---|
|ItemInternalId|Zeichenfolge|




### <a name="get-object"></a>Erste Objekt
Dieser Vorgang ruft ein Objekt ab. 


|Eigenschaftsname| Anzeigename|Beschreibung|
| ---|---|---|
|Tabelle *|Objekttyp|Vertrieb Objekttyp wie "Lead"|
|ID *|Objekt-id|Bezeichner des Objekts zu erhalten|

Ein * zeigt an, dass eine Eigenschaft erforderlich ist

#### <a name="output-details"></a>Die Ausgabedetails

Element


| Eigenschaftsname | Datentyp |
|---|---|
|ItemInternalId|Zeichenfolge|




### <a name="delete-object"></a>Objekt löschen
Dieser Vorgang löscht ein Objekt. 


|Eigenschaftsname| Anzeigename|Beschreibung|
| ---|---|---|
|Tabelle *|Objekttyp|Objekttyp wie "Lead"|
|ID *|Objekt-id|Bezeichner des zu löschenden Objekts|

Ein * zeigt an, dass eine Eigenschaft erforderlich ist




### <a name="update-object"></a>Objekt aktualisieren
Dieser Vorgang ist ein Objekt aktualisiert. 


|Eigenschaftsname| Anzeigename|Beschreibung|
| ---|---|---|
|Tabelle *|Objekttyp|Objekttyp wie "Lead"|
|ID *|Objekt-id|Bezeichner des Objekts zu aktualisieren|
|Element *|Objekt|Objekt mit den geänderten Eigenschaften|

Ein * zeigt an, dass eine Eigenschaft erforderlich ist

#### <a name="output-details"></a>Die Ausgabedetails

Element


| Eigenschaftsname | Datentyp |
|---|---|
|ItemInternalId|Zeichenfolge|




### <a name="when-an-object-is-created"></a>Bei der Erstellung eines Objekts
Dieser Vorgang löst einen Fluss aus, wenn ein Objekt erstellt wird. 


|Eigenschaftsname| Anzeigename|Beschreibung|
| ---|---|---|
|Tabelle *|Objekttyp|Objekttyp wie "Lead"|
|$filter|Filtern der Abfrage|Die Abfrage ein ODATA Filter zum Einschränken der Anzahl von Einträgen|
|$orderby|Sortiert nach|Eine ODATA OrderBy Abfrage zum Festlegen der Reihenfolge von Einträgen|
|$skip|Überspringen zählen|Anzahl von Einträgen, überspringen (Standard = 0)|
|$top|Abrufen der maximalen Anzahl|Maximale Anzahl von Einträgen zum Abrufen (Standard = 256)|

Ein * zeigt an, dass eine Eigenschaft erforderlich ist

#### <a name="output-details"></a>Die Ausgabedetails

ItemsList


| Eigenschaftsname | Datentyp |
|---|---|
|Wert|Matrix|




### <a name="when-an-object-is-modified"></a>Wenn ein Objekt geändert wird.
Dieser Vorgang löst einen Fluss aus, wenn ein Objekt geändert wird. 


|Eigenschaftsname| Anzeigename|Beschreibung|
| ---|---|---|
|Tabelle *|Objekttyp|Objekttyp wie "Lead"|
|$filter|Filtern der Abfrage|Die Abfrage ein ODATA Filter zum Einschränken der Anzahl von Einträgen|
|$orderby|Sortiert nach|Eine ODATA OrderBy Abfrage zum Festlegen der Reihenfolge von Einträgen|
|$skip|Überspringen zählen|Anzahl von Einträgen, überspringen (Standard = 0)|
|$top|Abrufen der maximalen Anzahl|Maximale Anzahl von Einträgen zum Abrufen (Standard = 256)|

Ein * zeigt an, dass eine Eigenschaft erforderlich ist

#### <a name="output-details"></a>Die Ausgabedetails

ItemsList


| Eigenschaftsname | Datentyp |
|---|---|
|Wert|Matrix|




### <a name="get-object-types"></a>Abrufen von Objekttypen
Dieser Vorgang Listet die verfügbaren Objekttypen. 


Es sind keine Parameter für diesen Anruf

#### <a name="output-details"></a>Die Ausgabedetails

TablesList


| Eigenschaftsname | Datentyp |
|---|---|
|Wert|Matrix|



## <a name="http-responses"></a>HTTP-Antworten

Eine oder mehrere der folgenden HTTP Statuscodes können die Aktionen und Trigger oben zurückgegeben werden: 

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
[Erstellen Sie eine app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md)
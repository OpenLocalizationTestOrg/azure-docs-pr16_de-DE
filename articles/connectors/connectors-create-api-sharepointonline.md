<properties
pageTitle="Informationen zum Verwenden des SharePoint Online Verbinders in Logik apps | Microsoft Azure"
description="Erstellen Sie mit der SharePoint Online Verbinder zum Verwalten von Listen auf SharePoint Logik apps."
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
ms.date="07/19/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-sharepoint-online-connector"></a>Erste Schritte mit der SharePoint Online-Verbinder

Verwenden Sie den SharePoint Online Netzwerke zum Verwalten von SharePoint-Listen.  

Um [alle Verbinder](./apis-list.md)verwenden zu können, müssen Sie zuerst eine app Logik zu erstellen. Sie können durch [Erstellen einer Logik app jetzt](../app-service-logic/app-service-logic-create-a-logic-app.md)loslegen.

## <a name="connect-to-sharepoint-online"></a>Verbinden mit SharePoint Online

Bevor Sie Ihre app Logik Dienste zugreifen kann, müssen Sie zuerst eine *Verbindung* mit dem Dienst erstellen. Eine [Verbindung](./connectors-overview.md) stellt eine Verbindung zwischen einer app Logik und einem anderen Dienst.  

### <a name="create-a-connection-to-sharepoint-online"></a>Herstellen einer Verbindung mit SharePoint Online

>[AZURE.INCLUDE [Steps to create a connection to SharePoint](../../includes/connectors-create-api-sharepointonline.md)]

## <a name="use-a-sharepoint-online-trigger"></a>Verwenden eines Triggers SharePoint Online

Ein Trigger ist ein Ereignis, das zum Starten des Workflows in einer app Logik definiert verwendet werden kann. [Erfahren Sie mehr über Trigger](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).  

>[AZURE.INCLUDE [Steps to create a SharePoint Online trigger](../../includes/connectors-create-api-sharepointonline-trigger.md)]

## <a name="use-a-sharepoint-online-action"></a>Verwenden Sie eine SharePoint Online-Aktion

Eine Aktion ist ein Vorgang durchgeführten durch den Workflow in einer app Logik definiert. [Erfahren Sie mehr über Aktionen](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).  

>[AZURE.INCLUDE [Steps to create a SharePoint Online action](../../includes/connectors-create-api-sharepointonline-action.md)]

## <a name="technical-details"></a>Technische Details

Hier sind die Details der Trigger, Aktionen und Antworten, die diese Verbindung unterstützt:

## <a name="sharepoint-online-triggers"></a>SharePoint Online-Triggern

SharePoint besteht aus die folgenden Triggern:  

|Auslösen | Beschreibung|
|--- | ---|
|[Wenn eine Datei erstellt wird](connectors-create-api-sharepointonline.md#when-a-file-is-created)|Dieser Vorgang löst einen Fluss beim Erstellen eine neue Datei in einem SharePoint-Ordner.|
|[Wenn eine Datei geändert wird.](connectors-create-api-sharepointonline.md#when-a-file-is-modified)|Dieser Vorgang löst einen Fluss aus, wenn eine Datei in einem SharePoint-Ordner geändert wird.|
|[Wenn ein neues Element erstellt wird](connectors-create-api-sharepointonline.md#when-a-new-item-is-created)|Dieser Vorgang löst einen Fluss aus, wenn ein neues Element in einer SharePoint-Liste erstellt wird.|
|[Wenn ein vorhandenes Element geändert wird.](connectors-create-api-sharepointonline.md#when-an-existing-item-is-modified)|Dieser Vorgang löst einen Fluss aus, wenn ein vorhandenes Element in einer SharePoint-Liste geändert wird.|


## <a name="sharepoint-online-actions"></a>SharePoint Online-Aktionen

SharePoint weist die folgenden Aktionen aus:


|Aktion|Beschreibung|
|--- | ---|
|[Abrufen von Dateimetadaten](connectors-create-api-sharepointonline.md#get-file-metadata)|Dieser Vorgang ruft Dateimetadaten, die mit dem Datei-Id an.|
|[Update-Datei](connectors-create-api-sharepointonline.md#update-file)|Dieser Vorgang wird der Inhalt der Datei aktualisiert.|
|[Datei löschen](connectors-create-api-sharepointonline.md#delete-file)|Dieser Vorgang löscht eine Datei.|
|[Abrufen von Metadaten mit Pfad der Datei](connectors-create-api-sharepointonline.md#get-file-metadata-using-path)|Dieser Vorgang ruft Dateimetadaten mithilfe des Dateipfades ab.|
|[Abrufen von Dateiinhalt unter Verwendung der Pfad](connectors-create-api-sharepointonline.md#get-file-content-using-path)|Dieser Vorgang ruft Dateiinhalt mithilfe des Dateipfades ab.|
|[Abrufen der Inhalt der Datei](connectors-create-api-sharepointonline.md#get-file-content)|Dieser Vorgang ruft-Dateiinhalt mit dem Datei-Id an.|
|[Datei erstellen](connectors-create-api-sharepointonline.md#create-file)|Dieser Vorgang lädt eine Datei in einer SharePoint-Website hoch.|
|[Kopieren einer Datei](connectors-create-api-sharepointonline.md#copy-file)|Dieser Vorgang kopiert eine Datei in einer SharePoint-Website an.|
|[Liste Ordner](connectors-create-api-sharepointonline.md#list-folder)|Mit diesem Vorgang wird in einem SharePoint-Ordner enthaltene Dateien an.|
|[Liste Stammordner](connectors-create-api-sharepointonline.md#list-root-folder)|Diese Protokollschema Ruft die Dateien in der SharePoint-Stammordner ab.|
|[Ordner zu extrahieren](connectors-create-api-sharepointonline.md#extract-folder)|Dieser Vorgang extrahiert eine Archivdatei in einem SharePoint-Ordner (Beispiel: ZIP).|
|[Abrufen von Elementen](connectors-create-api-sharepointonline.md#get-items)|Dieser Vorgang ruft Elemente aus einer SharePoint-Liste ab.|
|[Erstellen von Element](connectors-create-api-sharepointonline.md#create-item)|Dieser Vorgang erstellt ein neues Element in einer SharePoint-Liste.|
|[Abrufen von Element](connectors-create-api-sharepointonline.md#get-item)|Dieser Vorgang ruft ein einzelnes Element anhand dessen Id aus einer SharePoint-Liste.|
|[Element löschen](connectors-create-api-sharepointonline.md#delete-item)|Dieser Vorgang löscht ein Element aus einer SharePoint-Liste.|
|[Element aktualisieren](connectors-create-api-sharepointonline.md#update-item)|Dieser Vorgang aktualisiert, dass ein Element in einer SharePoint-Liste.|
|[Entität Werte abrufen](connectors-create-api-sharepointonline.md#get-entity-values)|Mit diesem Vorgang wird die möglichen Werte für eine SharePoint-Entität.|
|[Abrufen von Listen](connectors-create-api-sharepointonline.md#get-lists)|Dieser Vorgang ruft SharePoint-Listen auf einer Website ab.|
### <a name="action-details"></a>Aktionsdetails

Hier sind die Details für die Aktionen und Trigger für diesen Connector, zusammen mit ihren Antworten:



### <a name="get-file-metadata"></a>Abrufen von Dateimetadaten
Dieser Vorgang ruft Dateimetadaten, die mit dem Datei-Id an. 


|Eigenschaftsname| Anzeigename|Beschreibung|
| ---|---|---|
|DataSet *|Website-URL|Url der SharePoint-Website wie http://contoso.sharepoint.com/sites/mysite|
|ID *|Datei-ID|Wählen Sie eine Datei|

Ein * zeigt an, dass eine Eigenschaft erforderlich ist

#### <a name="output-details"></a>Die Ausgabedetails

BlobMetadata


| Eigenschaftsname | Datentyp |
|---|---|---|
|ID|Zeichenfolge|
|Namen|Zeichenfolge|
|DisplayName|Zeichenfolge|
|Pfad|Zeichenfolge|
|LastModified|Zeichenfolge|
|Größe|ganze Zahl|
|MediaType|Zeichenfolge|
|IsFolder|Boolesch|
|ETag|Zeichenfolge|
|FileLocator|Zeichenfolge|




### <a name="update-file"></a>Update-Datei
Dieser Vorgang wird der Inhalt der Datei aktualisiert. 


|Eigenschaftsname| Anzeigename|Beschreibung|
| ---|---|---|
|DataSet *|Website-URL|Url der SharePoint-Website wie http://contoso.sharepoint.com/sites/mysite|
|ID *|Datei-ID|Wählen Sie eine Datei|
|Textkörper *|Der Inhalt der Datei|Inhalt der Datei|

Ein * zeigt an, dass eine Eigenschaft erforderlich ist

#### <a name="output-details"></a>Die Ausgabedetails

BlobMetadata


| Eigenschaftsname | Datentyp |
|---|---|---|
|ID|Zeichenfolge|
|Namen|Zeichenfolge|
|DisplayName|Zeichenfolge|
|Pfad|Zeichenfolge|
|LastModified|Zeichenfolge|
|Größe|ganze Zahl|
|MediaType|Zeichenfolge|
|IsFolder|Boolesch|
|ETag|Zeichenfolge|
|FileLocator|Zeichenfolge|




### <a name="delete-file"></a>Datei löschen
Dieser Vorgang löscht eine Datei. 


|Eigenschaftsname| Anzeigename|Beschreibung|
| ---|---|---|
|DataSet *|Website-URL|Url der SharePoint-Website wie http://contoso.sharepoint.com/sites/mysite|
|ID *|Datei-ID|Wählen Sie eine Datei|

Ein * zeigt an, dass eine Eigenschaft erforderlich ist




### <a name="get-file-metadata-using-path"></a>Abrufen von Metadaten mit Pfad der Datei
Dieser Vorgang ruft Dateimetadaten mithilfe des Dateipfades ab. 


|Eigenschaftsname| Anzeigename|Beschreibung|
| ---|---|---|
|DataSet *|Website-URL|Url der SharePoint-Website wie http://contoso.sharepoint.com/sites/mysite|
|Pfad *|Dateipfad|Wählen Sie eine Datei|

Ein * zeigt an, dass eine Eigenschaft erforderlich ist

#### <a name="output-details"></a>Die Ausgabedetails

BlobMetadata


| Eigenschaftsname | Datentyp |
|---|---|---|
|ID|Zeichenfolge|
|Namen|Zeichenfolge|
|DisplayName|Zeichenfolge|
|Pfad|Zeichenfolge|
|LastModified|Zeichenfolge|
|Größe|ganze Zahl|
|MediaType|Zeichenfolge|
|IsFolder|Boolesch|
|ETag|Zeichenfolge|
|FileLocator|Zeichenfolge|




### <a name="get-file-content-using-path"></a>Abrufen von Dateiinhalt unter Verwendung der Pfad
Dieser Vorgang ruft Dateiinhalt mithilfe des Dateipfades ab. 


|Eigenschaftsname| Anzeigename|Beschreibung|
| ---|---|---|
|DataSet *|Website-URL|Url der SharePoint-Website wie http://contoso.sharepoint.com/sites/mysite|
|Pfad *|Dateipfad|Wählen Sie eine Datei|

Ein * zeigt an, dass eine Eigenschaft erforderlich ist




### <a name="get-file-content"></a>Abrufen der Inhalt der Datei
Dieser Vorgang ruft-Dateiinhalt mit dem Datei-Id an. 


|Eigenschaftsname| Anzeigename|Beschreibung|
| ---|---|---|
|DataSet *|Website-URL|Url der SharePoint-Website wie http://contoso.sharepoint.com/sites/mysite|
|ID *|Datei-ID|Wählen Sie eine Datei|

Ein * zeigt an, dass eine Eigenschaft erforderlich ist




### <a name="create-file"></a>Datei erstellen
Dieser Vorgang lädt eine Datei in einer SharePoint-Website hoch. 


|Eigenschaftsname| Anzeigename|Beschreibung|
| ---|---|---|
|DataSet *|Website-URL|Url der SharePoint-Website wie http://contoso.sharepoint.com/sites/mysite|
|Ordnerpfad *|Ordnerpfad|Wählen Sie eine Datei|
|Namen *|Dateiname|Name der Datei|
|Textkörper *|Der Inhalt der Datei|Inhalt der Datei|

Ein * zeigt an, dass eine Eigenschaft erforderlich ist

#### <a name="output-details"></a>Die Ausgabedetails

BlobMetadata


| Eigenschaftsname | Datentyp |
|---|---|---|
|ID|Zeichenfolge|
|Namen|Zeichenfolge|
|DisplayName|Zeichenfolge|
|Pfad|Zeichenfolge|
|LastModified|Zeichenfolge|
|Größe|ganze Zahl|
|MediaType|Zeichenfolge|
|IsFolder|Boolesch|
|ETag|Zeichenfolge|
|FileLocator|Zeichenfolge|




### <a name="copy-file"></a>Kopieren einer Datei
Dieser Vorgang kopiert eine Datei in einer SharePoint-Website an. 


|Eigenschaftsname| Anzeigename|Beschreibung|
| ---|---|---|
|DataSet *|Website-URL|Url der SharePoint-Website wie http://contoso.sharepoint.com/sites/mysite|
|Quelle *|Quelldatei Pfad|Pfad zu der Quelldatei|
|Ziel *|Zieldateipfad|Pfad zu der Zieldatei|
|Überschreiben|Überschreiben der Kennzeichnung|Ob die Zieldatei zu überschreiben, falls vorhanden|

Ein * zeigt an, dass eine Eigenschaft erforderlich ist

#### <a name="output-details"></a>Die Ausgabedetails

BlobMetadata


| Eigenschaftsname | Datentyp |
|---|---|---|
|ID|Zeichenfolge|
|Namen|Zeichenfolge|
|DisplayName|Zeichenfolge|
|Pfad|Zeichenfolge|
|LastModified|Zeichenfolge|
|Größe|ganze Zahl|
|MediaType|Zeichenfolge|
|IsFolder|Boolesch|
|ETag|Zeichenfolge|
|FileLocator|Zeichenfolge|




### <a name="when-a-file-is-created"></a>Wenn eine Datei erstellt wird
Dieser Vorgang löst einen Fluss beim Erstellen eine neue Datei in einem SharePoint-Ordner. 


|Eigenschaftsname| Anzeigename|Beschreibung|
| ---|---|---|
|DataSet *|Website-URL|Url der SharePoint-Website|
|Ordner ID *|Ordner-Id|Wählen Sie einen Ordner aus.|

Ein * zeigt an, dass eine Eigenschaft erforderlich ist




### <a name="when-a-file-is-modified"></a>Wenn eine Datei geändert wird.
Dieser Vorgang löst einen Fluss aus, wenn eine Datei in einem SharePoint-Ordner geändert wird. 


|Eigenschaftsname| Anzeigename|Beschreibung|
| ---|---|---|
|DataSet *|Website-URL|Url der SharePoint-Website|
|Ordner ID *|Ordner-Id|Wählen Sie einen Ordner aus.|

Ein * zeigt an, dass eine Eigenschaft erforderlich ist




### <a name="list-folder"></a>Liste Ordner
Mit diesem Vorgang wird in einem SharePoint-Ordner enthaltene Dateien an. 


|Eigenschaftsname| Anzeigename|Beschreibung|
| ---|---|---|
|DataSet *|Website-URL|Url der SharePoint-Website wie http://contoso.sharepoint.com/sites/mysite|
|ID *|Datei-ID|Eindeutiger Bezeichner des Ordners|

Ein * zeigt an, dass eine Eigenschaft erforderlich ist



#### <a name="output-details"></a>Die Ausgabedetails

BlobMetadata


| Eigenschaftsname | Datentyp |
|---|---|---|
|ID|Zeichenfolge|
|Namen|Zeichenfolge|
|DisplayName|Zeichenfolge|
|Pfad|Zeichenfolge|
|LastModified|Zeichenfolge|
|Größe|ganze Zahl|
|MediaType|Zeichenfolge|
|IsFolder|Boolesch|
|ETag|Zeichenfolge|
|FileLocator|Zeichenfolge|




### <a name="list-root-folder"></a>Liste Stammordner
Diese Protokollschema Ruft die Dateien in der SharePoint-Stammordner ab. 


|Eigenschaftsname| Anzeigename|Beschreibung|
| ---|---|---|
|DataSet *|Website-URL|Url der SharePoint-Website wie http://contoso.sharepoint.com/sites/mysite|

Ein * zeigt an, dass eine Eigenschaft erforderlich ist



#### <a name="output-details"></a>Die Ausgabedetails

BlobMetadata


| Eigenschaftsname | Datentyp |
|---|---|---|
|ID|Zeichenfolge|
|Namen|Zeichenfolge|
|DisplayName|Zeichenfolge|
|Pfad|Zeichenfolge|
|LastModified|Zeichenfolge|
|Größe|ganze Zahl|
|MediaType|Zeichenfolge|
|IsFolder|Boolesch|
|ETag|Zeichenfolge|
|FileLocator|Zeichenfolge|




### <a name="extract-folder"></a>Ordner zu extrahieren
Dieser Vorgang extrahiert eine Archivdatei in einem SharePoint-Ordner (Beispiel: ZIP). 


|Eigenschaftsname| Anzeigename|Beschreibung|
| ---|---|---|
|DataSet *|Website-URL|Url der SharePoint-Website wie http://contoso.sharepoint.com/sites/mysite|
|Quelle *|Quelldatei Pfad|Pfad zu der Quelldatei|
|Ziel *|Zielordnerpfad|Pfad zum Zielordner|
|Überschreiben|Überschreiben der Kennzeichnung|Ob die Zieldatei zu überschreiben, falls vorhanden|

Ein * zeigt an, dass eine Eigenschaft erforderlich ist



#### <a name="output-details"></a>Die Ausgabedetails

BlobMetadata


| Eigenschaftsname | Datentyp |
|---|---|---|
|ID|Zeichenfolge|
|Namen|Zeichenfolge|
|DisplayName|Zeichenfolge|
|Pfad|Zeichenfolge|
|LastModified|Zeichenfolge|
|Größe|ganze Zahl|
|MediaType|Zeichenfolge|
|IsFolder|Boolesch|
|ETag|Zeichenfolge|
|FileLocator|Zeichenfolge|




### <a name="when-a-new-item-is-created"></a>Wenn ein neues Element erstellt wird
Dieser Vorgang löst einen Fluss aus, wenn ein neues Element in einer SharePoint-Liste erstellt wird. 


|Eigenschaftsname| Anzeigename|Beschreibung|
| ---|---|---|
|DataSet *|Website-url|SharePoint-Website-Url (Beispiel: http://contoso.sharepoint.com/sites/mysite)|
|Tabelle *|Listenname|Name der SharePoint-Liste|
|$filter|Filtern der Abfrage|Die Abfrage ein ODATA Filter zum Einschränken der zurückgegebenen Einträge|
|$orderby|Sortiert nach|Eine ODATA OrderBy Abfrage zum Festlegen der Reihenfolge von Einträgen|
|$skip|Überspringen zählen|Anzahl von Einträgen, überspringen (Standard = 0)|
|$top|Abrufen der maximalen Anzahl|Maximale Anzahl von Einträgen zum Abrufen (Standard = 256)|

Ein * zeigt an, dass eine Eigenschaft erforderlich ist

#### <a name="output-details"></a>Die Ausgabedetails

ItemsList


| Eigenschaftsname | Datentyp | 
|---|---|
|Wert|Matrix|




### <a name="when-an-existing-item-is-modified"></a>Wenn ein vorhandenes Element geändert wird.
Dieser Vorgang löst einen Fluss aus, wenn ein vorhandenes Element in einer SharePoint-Liste geändert wird. 


|Eigenschaftsname| Anzeigename|Beschreibung|
| ---|---|---|
|DataSet *|Website-url|SharePoint-Website-Url (Beispiel: http://contoso.sharepoint.com/sites/mysite)|
|Tabelle *|Listenname|Name der SharePoint-Liste|
|$filter|Filtern der Abfrage|Die Abfrage ein ODATA Filter zum Einschränken der zurückgegebenen Einträge|
|$orderby|Sortiert nach|Eine ODATA OrderBy Abfrage zum Festlegen der Reihenfolge von Einträgen|
|$skip|Überspringen zählen|Anzahl von Einträgen, überspringen (Standard = 0)|
|$top|Abrufen der maximalen Anzahl|Maximale Anzahl von Einträgen zum Abrufen (Standard = 256)|

Ein * zeigt an, dass eine Eigenschaft erforderlich ist

#### <a name="output-details"></a>Die Ausgabedetails

ItemsList


| Eigenschaftsname | Datentyp |
|---|---|
|Wert|Matrix|




### <a name="get-items"></a>Abrufen von Elementen
Dieser Vorgang ruft Elemente aus einer SharePoint-Liste ab. 


|Eigenschaftsname| Anzeigename|Beschreibung|
| ---|---|---|
|DataSet *|Website-url|SharePoint-Website-Url (Beispiel: http://contoso.sharepoint.com/sites/mysite)|
|Tabelle *|Listenname|Name der SharePoint-Liste|
|$filter|Filtern der Abfrage|Die Abfrage ein ODATA Filter zum Einschränken der zurückgegebenen Einträge|
|$orderby|Sortiert nach|Eine ODATA OrderBy Abfrage zum Festlegen der Reihenfolge von Einträgen|
|$skip|Überspringen zählen|Anzahl von Einträgen, überspringen (Standard = 0)|
|$top|Abrufen der maximalen Anzahl|Maximale Anzahl von Einträgen zum Abrufen (Standard = 256)|

Ein * zeigt an, dass eine Eigenschaft erforderlich ist

#### <a name="output-details"></a>Die Ausgabedetails

ItemsList


| Eigenschaftsname | Datentyp |
|---|---|
|Wert|Matrix|




### <a name="create-item"></a>Erstellen von Element
Dieser Vorgang erstellt ein neues Element in einer SharePoint-Liste. 


|Eigenschaftsname| Anzeigename|Beschreibung|
| ---|---|---|
|DataSet *|Website-url|SharePoint-Website-Url (Beispiel: http://contoso.sharepoint.com/sites/mysite)|
|Tabelle *|Listenname|Name der SharePoint-Liste|
|Element *|Element|Element zu erstellen|

Ein * zeigt an, dass eine Eigenschaft erforderlich ist

#### <a name="output-details"></a>Die Ausgabedetails

Element


| Eigenschaftsname | Datentyp |
|---|---|
|ItemInternalId|Zeichenfolge|




### <a name="get-item"></a>Abrufen von Element
Dieser Vorgang ruft ein einzelnes Element anhand dessen Id aus einer SharePoint-Liste. 


|Eigenschaftsname| Anzeigename|Beschreibung|
| ---|---|---|
|DataSet *|Website-url|SharePoint-Website-Url (Beispiel: http://contoso.sharepoint.com/sites/mysite)|
|Tabelle *|Listenname|Name der SharePoint-Liste|
|ID *|ID|Eindeutiger Bezeichner des Elements abgerufen werden sollen|

Ein * zeigt an, dass eine Eigenschaft erforderlich ist

#### <a name="output-details"></a>Die Ausgabedetails

Element


| Eigenschaftsname | Datentyp |
|---|---|
|ItemInternalId|Zeichenfolge|




### <a name="delete-item"></a>Element löschen
Dieser Vorgang löscht ein Element aus einer SharePoint-Liste. 


|Eigenschaftsname| Anzeigename|Beschreibung|
| ---|---|---|
|DataSet *|Website-url|SharePoint-Website-Url (Beispiel: http://contoso.sharepoint.com/sites/mysite)|
|Tabelle *|Listenname|Name der SharePoint-Liste|
|ID *|ID|Eindeutiger Bezeichner des zu löschenden Element|

Ein * zeigt an, dass eine Eigenschaft erforderlich ist




### <a name="update-item"></a>Element aktualisieren
Dieser Vorgang aktualisiert, dass ein Element in einer SharePoint-Liste. 


|Eigenschaftsname| Anzeigename|Beschreibung|
| ---|---|---|
|DataSet *|Website-url|SharePoint-Website-Url (Beispiel: http://contoso.sharepoint.com/sites/mysite)|
|Tabelle *|Listenname|Name der SharePoint-Liste|
|ID *|ID|Eindeutiger Bezeichner des Elements, das aktualisiert werden|
|Element *|Element|Element mit den geänderten Eigenschaften|

Ein * zeigt an, dass eine Eigenschaft erforderlich ist

#### <a name="output-details"></a>Die Ausgabedetails

Element


| Eigenschaftsname | Datentyp |
|---|---|
|ItemInternalId|Zeichenfolge|




### <a name="get-entity-values"></a>Entität Werte abrufen
Mit diesem Vorgang wird die möglichen Werte für eine SharePoint-Entität. 


|Eigenschaftsname| Anzeigename|Beschreibung|
| ---|---|---|
|DataSet *|Url der SharePoint-Website|Url der SharePoint-Website|
|Tabelle *|Tabellenname|Tabellenname|
|ID *|Element-id|Element-id|

Ein * zeigt an, dass eine Eigenschaft erforderlich ist

#### <a name="output-details"></a>Die Ausgabedetails





### <a name="get-lists"></a>Abrufen von Listen
Dieser Vorgang ruft SharePoint-Listen auf einer Website ab. 


|Eigenschaftsname| Anzeigename|Beschreibung|
| ---|---|---|
|DataSet *|Website-url|SharePoint-Website-Url (Beispiel: http://contoso.sharepoint.com/sites/mysite)|

Ein * zeigt an, dass eine Eigenschaft erforderlich ist

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
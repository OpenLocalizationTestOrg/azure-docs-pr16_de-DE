<properties
pageTitle="Verwenden den SharePoint Online-Verbinder in Ihrer Apps Logik | Microsoft Azure"
description="Erste Schritte mit der Azure-App-Dienst SharePoint Online Verbinder in Ihrer apps Logik."
services=""    
documentationCenter=""     
authors="msftman"    
manager="erikre"    
editor=""
tags="connectors"/>

<tags
ms.service="multiple"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="na"
ms.date="08/18/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-sharepoint-online-connector"></a>Erste Schritte mit der SharePoint Online-Verbinder 

Der Verbinder SharePoint bietet eine Möglichkeit zum Arbeiten mit Listen in SharePoint.

>[AZURE.NOTE] Diese Version des Artikels gilt Logik apps 2015-08-01-Schema Vorschauversion aus.


Um einen Vorgang in Logik apps hinzuzufügen, finden Sie unter [Erstellen einer app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="lets-talk-about-triggers-and-actions"></a>Wissenswertes zu Trigger und Aktionen

Der SharePoint-Verbinder kann als eine Aktion verwendet werden. Es hat Triggern. Alle Connectors unterstützen die Daten in JSON und XML-Formaten. 

Der SharePoint-Verbinder weist die folgenden Aktion(en) und/oder Triggern zur Verfügung:

### <a name="sharepoint-actions"></a>SharePoint-Aktionen
Sie können folgende Aktionen ausführen:

|Aktion|Beschreibung|
|--- | ---|
|GetFileMetadata|Zum Abrufen von Datei-Metadaten auf Dokumentbibliothek verwendet|
|UpdateFile|Zum Aktualisieren einer Datei in einer Dokumentbibliothek|
|DeleteFile|Zum Löschen einer Datei in einer Dokumentbibliothek|
|GetFileMetadataByPath|Zum Abrufen von Datei-Metadaten auf Dokumentbibliothek verwendet|
|GetFileContentByPath|Zum Abrufen von einer Datei in einer Dokumentbibliothek verwendet|
|GetFileContent|Zum Abrufen von einer Datei in einer Dokumentbibliothek verwendet|
|CreateFile|Zum Hochladen einer Datei in einer Dokumentbibliothek|
|CopyFile|Zum Kopieren einer Datei in einer Dokumentbibliothek|
|ExtractFolderV2|Zum Extrahieren eines Ordners auf Dokumentbibliothek verwendet|
|PostItem|Erstellt ein neues Element in einer SharePoint-Liste|
|GetItem|Ruft ein einzelnes Element aus einer SharePoint-Liste|
|DeleteItem|Löscht ein Element aus einer SharePoint-Liste|
|PatchItem|So aktualisiert, dass ein Element in einer SharePoint-Liste|
### <a name="sharepoint-triggers"></a>SharePoint-Triggern
Sie können diese Ereignisse überwachen:

|Auslösen | Beschreibung|
|--- | ---|
|OnNewFile|Einen Fluss auslöst, beim Erstellen eine neue Datei in einem SharePoint-Ordner|
|OnUpdatedFile|Einen Fluss ausgelöst wird, wenn eine Datei in einem SharePoint-Ordner geändert wird|
|GetOnNewItems|Wenn ein neues Element in einer SharePoint-Liste erstellt wird|
|GetOnUpdatedItems|Wenn ein vorhandenes Element in einer SharePoint-Liste geändert wird.|


## <a name="create-a-connection-to-sharepoint"></a>Herstellen einer Verbindung mit SharePoint
Wenn Sie den SharePoint-Connector verwenden, erstellen Sie zunächst eine **Verbindung** Sie die Details für diese Eigenschaften bereit: 

|Eigenschaft| Erforderlich|Beschreibung|
| ---|---|---|
|Token|Ja|Bereitstellen von SharePoint-Anmeldeinformationen|

Und mit **SharePoint Online**herstellen zu können, müssen Sie Ihre Identität (Benutzername und Kennwort, Smartcard-Anmeldeinformationen usw.) in SharePoint Online bereitstellen. Nachdem Sie authentifiziert wurden, haben, können Sie mit der SharePoint Online-Verbinder in Ihrer app Logik fortfahren. 

Klicken Sie auf den Designer der app Logik, melden Sie sich bei SharePoint erstellen die Verbindung **Verbindung** zur Verwendung in Ihrer app Logik folgendermaßen Sie vor:

1. Geben Sie in das Suchfeld SharePoint und warten Sie, bis die Suche auf alle Einträge mit SharePoint in den Namen zurück:   
![Konfigurieren von SharePoint][1]  
2. Wählen Sie **SharePoint Online - beim Erstellen eine Datei**   
3. Wählen Sie aus, **Melden Sie sich bei SharePoint Online**:   
![Konfigurieren von SharePoint][2]    
4. Geben Sie Ihre Anmeldeinformationen SharePoint melden Sie sich bei SharePoint-Authentifizierung   
![Konfigurieren von SharePoint][3]     
5. Nach Abschluss der Authentifizierung werden Sie zu Ihrer Anwendung Logik es ausführen, indem Sie **beim Erstellen eine Datei** SharePoints-Dialogfeld umgeleitet werden.          
![Konfigurieren von SharePoint][4]  
6. Sie können dann Hinzufügen anderer Trigger und Aktionen, die Sie Ihre app Logik ausführen müssen.   
7. Speichern Sie Ihre Arbeit, indem Sie auf der Menüleiste oben **Speichern** auswählen.  


## <a name="sharepoint-rest-api-reference"></a>SharePoint-REST-API-Referenz
#### <a name="this-documentation-is-for-version-10"></a>Diese Dokumentation ist für Version: 1.0


### <a name="used-for-getting-a-file-metadata-on-document-library"></a>Zum Abrufen von Datei-Metadaten auf Dokumentbibliothek verwendet
**```GET: /datasets/{dataset}/files/{id}```** 



| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|DataSet|Zeichenfolge|Ja|Pfad|keine|URL der SharePoint-Website. Z. B. http://contoso.SharePoint.com/Sites/mysite|
|ID|Zeichenfolge|Ja|Pfad|keine|Eindeutiger Bezeichner der Datei|


### <a name="here-are-the-possible-responses"></a>Hier sind die möglichen Antworten aus:

|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|
------



### <a name="used-for-updating-a-file-on-document-library"></a>Zum Aktualisieren einer Datei in einer Dokumentbibliothek
**```PUT: /datasets/{dataset}/files/{id}```** 



| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|DataSet|Zeichenfolge|Ja|Pfad|keine|URL der SharePoint-Website. Z. B. http://contoso.SharePoint.com/Sites/mysite|
|ID|Zeichenfolge|Ja|Pfad|keine|Eindeutiger Bezeichner der Datei|
|Textkörper| |Ja|Textkörper|keine|Der Inhalt der Datei|


### <a name="here-are-the-possible-responses"></a>Hier sind die möglichen Antworten aus:

|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|
------



### <a name="used-for-deleting-a-file-on-document-library"></a>Zum Löschen einer Datei in einer Dokumentbibliothek
**```DELETE: /datasets/{dataset}/files/{id}```** 



| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|DataSet|Zeichenfolge|Ja|Pfad|keine|URL der SharePoint-Website. Z. B. http://contoso.SharePoint.com/Sites/mysite|
|ID|Zeichenfolge|Ja|Pfad|keine|Eindeutiger Bezeichner der Datei|


### <a name="here-are-the-possible-responses"></a>Hier sind die möglichen Antworten aus:

|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|
------



### <a name="used-for-getting-a-file-metadata-on-document-library"></a>Zum Abrufen von Datei-Metadaten auf Dokumentbibliothek verwendet
**```GET: /datasets/{dataset}/GetFileByPath```** 



| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|DataSet|Zeichenfolge|Ja|Pfad|keine|URL der SharePoint-Website. Z. B. http://contoso.SharePoint.com/Sites/mysite|
|Pfad|Zeichenfolge|Ja|Abfrage|keine|Pfad der Datei|


### <a name="here-are-the-possible-responses"></a>Hier sind die möglichen Antworten aus:

|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|
------



### <a name="used-for-getting-a-file-on-document-library"></a>Zum Abrufen von einer Datei in einer Dokumentbibliothek verwendet
**```GET: /datasets/{dataset}/GetFileContentByPath```** 



| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|DataSet|Zeichenfolge|Ja|Pfad|keine|URL der SharePoint-Website. Z. B. http://contoso.SharePoint.com/Sites/mysite|
|Pfad|Zeichenfolge|Ja|Abfrage|keine|Pfad der Datei|


### <a name="here-are-the-possible-responses"></a>Hier sind die möglichen Antworten aus:

|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|
------



### <a name="used-for-getting-a-file-on-document-library"></a>Zum Abrufen von einer Datei in einer Dokumentbibliothek verwendet
**```GET: /datasets/{dataset}/files/{id}/content```** 



| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|DataSet|Zeichenfolge|Ja|Pfad|keine|URL der SharePoint-Website. Z. B. http://contoso.SharePoint.com/Sites/mysite|
|ID|Zeichenfolge|Ja|Pfad|keine|Eindeutiger Bezeichner der Datei|


### <a name="here-are-the-possible-responses"></a>Hier sind die möglichen Antworten aus:

|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|
------



### <a name="used-for-uploading-a-file-on-document-library"></a>Zum Hochladen einer Datei in einer Dokumentbibliothek
**```POST: /datasets/{dataset}/files```** 



| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|DataSet|Zeichenfolge|Ja|Pfad|keine|URL der SharePoint-Website. Z. B. http://contoso.SharePoint.com/Sites/mysite|
|Ordnerpfad|Zeichenfolge|Ja|Abfrage|keine|Der Pfad zum Ordner|
|Namen|Zeichenfolge|Ja|Abfrage|keine|Name der Datei|
|Textkörper| |Ja|Textkörper|keine|Der Inhalt der Datei|


### <a name="here-are-the-possible-responses"></a>Hier sind die möglichen Antworten aus:

|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|
------



### <a name="used-for-copying-a-file-on-document-library"></a>Zum Kopieren einer Datei in einer Dokumentbibliothek
**```POST: /datasets/{dataset}/copyFile```** 



| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|DataSet|Zeichenfolge|Ja|Pfad|keine|URL der SharePoint-Website. Z. B. http://contoso.SharePoint.com/Sites/mysite|
|Datenquelle|Zeichenfolge|Ja|Abfrage|keine|Pfad zu der Quelldatei|
|Ziel|Zeichenfolge|Ja|Abfrage|keine|Pfad zu der Zieldatei|
|Überschreiben|Boolesch|Nein|Abfrage|falsch|Ob eine vorhandene Datei überschreiben|


### <a name="here-are-the-possible-responses"></a>Hier sind die möglichen Antworten aus:

|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|
------



### <a name="triggers-a-flow-when-a-new-file-is-created-in-a-sharepoint-folder"></a>Einen Fluss auslöst, beim Erstellen eine neue Datei in einem SharePoint-Ordner
**```GET: /datasets/{dataset}/triggers/onnewfile```** 



| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|DataSet|Zeichenfolge|Ja|Pfad|keine|Url der SharePoint-Website|
|Ordner-ID|Zeichenfolge|Ja|Abfrage|keine|Eindeutiger Bezeichner des Ordners in SharePoint|


### <a name="here-are-the-possible-responses"></a>Hier sind die möglichen Antworten aus:

|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|
------



### <a name="triggers-a-flow-when-a-file-is-modified-in-a-sharepoint-folder"></a>Einen Fluss ausgelöst wird, wenn eine Datei in einem SharePoint-Ordner geändert wird
**```GET: /datasets/{dataset}/triggers/onupdatedfile```** 



| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|DataSet|Zeichenfolge|Ja|Pfad|keine|Url der SharePoint-Website|
|Ordner-ID|Zeichenfolge|Ja|Abfrage|keine|Eindeutiger Bezeichner des Ordners in SharePoint|


### <a name="here-are-the-possible-responses"></a>Hier sind die möglichen Antworten aus:

|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|
------



### <a name="used-for-extracting-a-folder-on-document-library"></a>Zum Extrahieren eines Ordners auf Dokumentbibliothek verwendet
**```POST: /datasets/{dataset}/extractFolderV2```** 



| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|DataSet|Zeichenfolge|Ja|Pfad|keine|URL der SharePoint-Website. Z. B. http://contoso.SharePoint.com/Sites/mysite|
|Datenquelle|Zeichenfolge|Ja|Abfrage|keine|Pfad zu der Quelldatei|
|Ziel|Zeichenfolge|Ja|Abfrage|keine|Pfad zum Zielordner|
|Überschreiben|Boolesch|Nein|Abfrage|falsch|Ob eine vorhandene Datei überschreiben|


### <a name="here-are-the-possible-responses"></a>Hier sind die möglichen Antworten aus:

|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|
------



### <a name="when-a-new-item-is-created-in-a-sharepoint-list"></a>Wenn ein neues Element in einer SharePoint-Liste erstellt wird
**```GET: /datasets/{dataset}/tables/{table}/onnewitems```** 



| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|DataSet|Zeichenfolge|Ja|Pfad|keine|SharePoint-Website-Url (Beispiel: http://contoso.sharepoint.com/sites/mysite)|
|Tabelle|Zeichenfolge|Ja|Pfad|keine|Name der SharePoint-Liste|
|$skip|ganze Zahl|Nein|Abfrage|keine|Anzahl von Einträgen, überspringen (Standard = 0)|
|$top|ganze Zahl|Nein|Abfrage|keine|Maximale Anzahl von Einträgen zum Abrufen (Standard = 256)|
|$filter|Zeichenfolge|Nein|Abfrage|keine|Die Abfrage ein ODATA Filter zum Einschränken der Anzahl von Einträgen|
|$orderby|Zeichenfolge|Nein|Abfrage|keine|Eine ODATA OrderBy Abfrage zum Festlegen der Reihenfolge von Einträgen|


### <a name="here-are-the-possible-responses"></a>Hier sind die möglichen Antworten aus:

|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|
------



### <a name="when-an-existing-item-is-modified-in-a-sharepoint-list"></a>Wenn ein vorhandenes Element in einer SharePoint-Liste geändert wird.
**```GET: /datasets/{dataset}/tables/{table}/onupdateditems```** 



| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|DataSet|Zeichenfolge|Ja|Pfad|keine|SharePoint-Website-Url (Beispiel: http://contoso.sharepoint.com/sites/mysite)|
|Tabelle|Zeichenfolge|Ja|Pfad|keine|Name der SharePoint-Liste|
|$skip|ganze Zahl|Nein|Abfrage|keine|Anzahl von Einträgen, überspringen (Standard = 0)|
|$top|ganze Zahl|Nein|Abfrage|keine|Maximale Anzahl von Einträgen zum Abrufen (Standard = 256)|
|$filter|Zeichenfolge|Nein|Abfrage|keine|Die Abfrage ein ODATA Filter zum Einschränken der Anzahl von Einträgen|
|$orderby|Zeichenfolge|Nein|Abfrage|keine|Eine ODATA OrderBy Abfrage zum Festlegen der Reihenfolge von Einträgen|


### <a name="here-are-the-possible-responses"></a>Hier sind die möglichen Antworten aus:

|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|
------



### <a name="creates-a-new-item-in-a-sharepoint-list"></a>Erstellt ein neues Element in einer SharePoint-Liste
**```POST: /datasets/{dataset}/tables/{table}/items```** 



| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|DataSet|Zeichenfolge|Ja|Pfad|keine|SharePoint-Website-Url (Beispiel: http://contoso.sharepoint.com/sites/mysite)|
|Tabelle|Zeichenfolge|Ja|Pfad|keine|Name der SharePoint-Liste|
|Element| |Ja|Textkörper|keine|Element zu erstellen|


### <a name="here-are-the-possible-responses"></a>Hier sind die möglichen Antworten aus:

|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|
------



### <a name="retrieves-a-single-item-from-a-sharepoint-list"></a>Ruft ein einzelnes Element aus einer SharePoint-Liste
**```GET: /datasets/{dataset}/tables/{table}/items/{id}```** 



| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|DataSet|Zeichenfolge|Ja|Pfad|keine|SharePoint-Website-Url (Beispiel: http://contoso.sharepoint.com/sites/mysite)|
|Tabelle|Zeichenfolge|Ja|Pfad|keine|Name der SharePoint-Liste|
|ID|ganze Zahl|Ja|Pfad|keine|Eindeutiger Bezeichner des Elements abgerufen werden sollen|


### <a name="here-are-the-possible-responses"></a>Hier sind die möglichen Antworten aus:

|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|
------



### <a name="deletes-an-item-from-a-sharepoint-list"></a>Löscht ein Element aus einer SharePoint-Liste
**```DELETE: /datasets/{dataset}/tables/{table}/items/{id}```** 



| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|DataSet|Zeichenfolge|Ja|Pfad|keine|SharePoint-Website-Url (Beispiel: http://contoso.sharepoint.com/sites/mysite)|
|Tabelle|Zeichenfolge|Ja|Pfad|keine|Name der SharePoint-Liste|
|ID|ganze Zahl|Ja|Pfad|keine|Eindeutiger Bezeichner des zu löschenden Element|


### <a name="here-are-the-possible-responses"></a>Hier sind die möglichen Antworten aus:

|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|
------



### <a name="updates-an-item-in-a-sharepoint-list"></a>So aktualisiert, dass ein Element in einer SharePoint-Liste
**```PATCH: /datasets/{dataset}/tables/{table}/items/{id}```** 



| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|DataSet|Zeichenfolge|Ja|Pfad|keine|SharePoint-Website-Url (Beispiel: http://contoso.sharepoint.com/sites/mysite)|
|Tabelle|Zeichenfolge|Ja|Pfad|keine|Name der SharePoint-Liste|
|ID|ganze Zahl|Ja|Pfad|keine|Eindeutiger Bezeichner des Elements, das aktualisiert werden|
|Element| |Ja|Textkörper|keine|Element mit den geänderten Eigenschaften|


### <a name="here-are-the-possible-responses"></a>Hier sind die möglichen Antworten aus:

|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|
------



## <a name="object-definitions"></a>Objekt Definitionen: 

 **DataSetsMetadata**:

Erforderlichen Eigenschaften für DataSetsMetadata:


Keiner der Eigenschaften ist erforderlich. 


**Alle Eigenschaften**: 


| Namen | Datentyp |
|---|---|
|tabellarische|nicht definiert|
|BLOB|nicht definiert|



 **TabularDataSetsMetadata**:

Erforderlichen Eigenschaften für TabularDataSetsMetadata:


Keiner der Eigenschaften ist erforderlich. 


**Alle Eigenschaften**: 


| Namen | Datentyp |
|---|---|
|Datenquelle|Zeichenfolge|
|displayName|Zeichenfolge|
|urlEncoding|Zeichenfolge|
|tableDisplayName|Zeichenfolge|
|tablePluralName|Zeichenfolge|



 **BlobDataSetsMetadata**:

Erforderlichen Eigenschaften für BlobDataSetsMetadata:


Keiner der Eigenschaften ist erforderlich. 


**Alle Eigenschaften**: 


| Namen | Datentyp |
|---|---|
|Datenquelle|Zeichenfolge|
|displayName|Zeichenfolge|
|urlEncoding|Zeichenfolge|



 **BlobMetadata**:

Erforderlichen Eigenschaften für BlobMetadata:


Keiner der Eigenschaften ist erforderlich. 


**Alle Eigenschaften**: 


| Namen | Datentyp |
|---|---|
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



 **Objekt**:

Erforderlichen Eigenschaften für das Objekt:


Keiner der Eigenschaften ist erforderlich. 


**Alle Eigenschaften**: 


| Namen | Datentyp |
|---|---|



 **TableMetadata**:

Erforderlichen Eigenschaften für TableMetadata:


Keiner der Eigenschaften ist erforderlich. 


**Alle Eigenschaften**: 


| Namen | Datentyp |
|---|---|
|Namen|Zeichenfolge|
|Titel|Zeichenfolge|
|X-ms-Berechtigung|Zeichenfolge|
|Schema|nicht definiert|



 **DataSetsList**:

Erforderlichen Eigenschaften für DataSetsList:


Keiner der Eigenschaften ist erforderlich. 


**Alle Eigenschaften**: 


| Namen | Datentyp |
|---|---|
|Wert|Matrix|



 **DataSet**:

Erforderlichen Eigenschaften für DataSet:


Keiner der Eigenschaften ist erforderlich. 


**Alle Eigenschaften**: 


| Namen | Datentyp |
|---|---|
|Namen|Zeichenfolge|
|DisplayName|Zeichenfolge|



 **Tabelle**:

Erforderlichen Eigenschaften für die Tabelle:


Keiner der Eigenschaften ist erforderlich. 


**Alle Eigenschaften**: 


| Namen | Datentyp |
|---|---|
|Namen|Zeichenfolge|
|DisplayName|Zeichenfolge|



 **Element**:

Erforderlichen Eigenschaften für das Element:


Keiner der Eigenschaften ist erforderlich. 


**Alle Eigenschaften**: 


| Namen | Datentyp |
|---|---|
|ItemInternalId|Zeichenfolge|



 **ItemsList**:

Erforderlichen Eigenschaften für ItemsList:


Keiner der Eigenschaften ist erforderlich. 


**Alle Eigenschaften**: 


| Namen | Datentyp |
|---|---|
|Wert|Matrix|



 **TablesList**:

Erforderlichen Eigenschaften für TablesList:


Keiner der Eigenschaften ist erforderlich. 


**Alle Eigenschaften**: 


| Namen | Datentyp |
|---|---|
|Wert|Matrix|


## <a name="next-steps"></a>Nächste Schritte
[Erstellen Sie eine app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md)  

[1]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig1.png  
[2]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig2.png 
[3]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig3.png
[4]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig4.png
[5]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig5.png

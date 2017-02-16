<properties
pageTitle="OneDrive for Business | Microsoft Azure"
description="Erstellen Sie Logik apps mit Azure-App-Dienst an. Verbinden Sie mit OneDrive for Business zum Verwalten Ihrer Dateien. Sie können verschiedene Aktionen durchführen wie hochladen, aktualisieren, abrufen, und klicken Sie auf Dateien löschen."
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
ms.date="08/18/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-onedrive-for-business-connector"></a>Erste Schritte mit der OneDrive for Business-connector

Verbinden Sie mit OneDrive for Business zum Verwalten Ihrer Dateien. Sie können verschiedene Aktionen durchführen wie hochladen, aktualisieren, abrufen, und klicken Sie auf Dateien löschen.

>[AZURE.NOTE] Diese Version des Artikels gilt Logik apps 2015-08-01-Schema Vorschauversion aus. 

Sie können beginnen, indem Sie eine app Logik jetzt erstellen, finden Sie unter [Erstellen einer app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Trigger und Aktionen

Die OneDrive for Business Connector kann als eine Aktion verwendet werden. Es hat Triggern. Alle Connectors unterstützen die Daten in JSON und XML-Formaten. 

 Die OneDrive for Business Connector weist die folgenden Aktion(en) und/oder Triggern zur Verfügung:

### <a name="onedrive-for-business-actions"></a>OneDrive für Business-Aktionen
Sie können folgende Aktionen ausführen:

|Aktion|Beschreibung|
|--- | ---|
|[GetFileMetadata](connectors-create-api-onedriveforbusiness.md#getfilemetadata)|Ruft Metadaten einer Datei in OneDrive for Business mit-id|
|[UpdateFile](connectors-create-api-onedriveforbusiness.md#updatefile)|So aktualisiert, dass eine Datei in OneDrive for Business|
|[DeleteFile](connectors-create-api-onedriveforbusiness.md#deletefile)|Löscht eine Datei mit OneDrive for Business|
|[GetFileMetadataByPath](connectors-create-api-onedriveforbusiness.md#getfilemetadatabypath)|Ruft Metadaten einer Datei in OneDrive for Business mit Pfad|
|[GetFileContentByPath](connectors-create-api-onedriveforbusiness.md#getfilecontentbypath)|Inhalt einer Datei in OneDrive for Business mit Pfad ruft|
|[GetFileContent](connectors-create-api-onedriveforbusiness.md#getfilecontent)|Ruft Inhalt einer Datei in OneDrive for Business mit-id|
|[CreateFile](connectors-create-api-onedriveforbusiness.md#createfile)|Führt den Upload einer Datei in OneDrive for Business|
|[CopyFile](connectors-create-api-onedriveforbusiness.md#copyfile)|Kopiert eine Datei zu OneDrive for Business|
|[ListFolder](connectors-create-api-onedriveforbusiness.md#listfolder)|Listet die Dateien in einer OneDrive for Business-Ordner|
|[ListRootFolder](connectors-create-api-onedriveforbusiness.md#listrootfolder)|Listet die Dateien im onedrive for Business-Stamm-Ordner|
|[ExtractFolderV2](connectors-create-api-onedriveforbusiness.md#extractfolderv2)|Extrahiert einen Ordner zu OneDrive for Business|
### <a name="onedrive-for-business-triggers"></a>OneDrive for Business auslöst
Sie können diese Ereignisse überwachen:

|Auslösen | Beschreibung|
|--- | ---|
|Wenn eine Datei erstellt wird|Einen Fluss auslöst, beim Erstellen eine neue Datei in einer OneDrive for Business-Ordner|
|Wenn eine Datei geändert wird.|Einen Fluss ausgelöst wird, wenn eine Datei in einer OneDrive for Business-Ordner geändert wird|


## <a name="create-a-connection-to-onedrive-for-business"></a>Herstellen einer Verbindung mit OneDrive for Business
Um Logik apps mit OneDrive for Business zu erstellen, müssen Sie zunächst eine **Verbindung** erstellen anschließend geben Sie die Details für die folgenden Eigenschaften: 

|Eigenschaft| Erforderlich|Beschreibung|
| ---|---|---|
|Token|Ja|Bereitstellen von OneDrive für Business-Anmeldeinformationen|
Nachdem Sie die Verbindung zu erstellen, können Sie es verwenden, führen Sie die Aktionen aus, und achten Sie auf die in diesem Artikel beschriebenen Trigger. 

>[AZURE.INCLUDE [Steps to create a connection to OneDrive for Business](../../includes/connectors-create-api-onedriveforbusiness.md)]

>[AZURE.TIP] Sie können diese Verbindung in andere apps Logik verwenden.

## <a name="reference-for-onedrive-for-business"></a>Referenz für OneDrive for Business
Version gilt: 1.0

## <a name="getfilemetadata"></a>GetFileMetadata
Abrufen von Dateimetadaten, die mit-Id: Ruft Metadaten einer Datei in OneDrive for Business mit-Id 

```GET: /datasets/default/files/{id}``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|ID|Zeichenfolge|Ja|Pfad|keine|Geben Sie die Datei|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


## <a name="updatefile"></a>UpdateFile
Update-Datei: so aktualisiert, dass eine Datei in OneDrive for Business 

```PUT: /datasets/default/files/{id}``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|ID|Zeichenfolge|Ja|Pfad|keine|Geben Sie die Datei aktualisieren|
|Textkörper| |Ja|Textkörper|keine|Inhalt der Datei in OneDrive for Business aktualisieren|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


## <a name="deletefile"></a>DeleteFile
Datei löschen: Löscht eine Datei mit OneDrive for Business 

```DELETE: /datasets/default/files/{id}``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|ID|Zeichenfolge|Ja|Pfad|keine|Geben Sie die zu löschende Datei|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


## <a name="getfilemetadatabypath"></a>GetFileMetadataByPath
Abrufen von Metadaten mit Pfad der Datei: Ruft Metadaten einer Datei in OneDrive for Business mit Pfad 

```GET: /datasets/default/GetFileByPath``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Pfad|Zeichenfolge|Ja|Abfrage|keine|Eindeutige Pfad zu der Datei in OneDrive for Business|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


## <a name="getfilecontentbypath"></a>GetFileContentByPath
Abrufen von Dateiinhalt unter Verwendung der Pfad: Ruft Inhalt einer Datei in OneDrive for Business mit Pfad 

```GET: /datasets/default/GetFileContentByPath``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Pfad|Zeichenfolge|Ja|Abfrage|keine|Eindeutige Pfad zu der Datei in OneDrive for Business|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


## <a name="getfilecontent"></a>GetFileContent
Inhalt der Datei mit-Id abrufen: Ruft Inhalt einer Datei in OneDrive for Business mit-Id 

```GET: /datasets/default/files/{id}/content``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|ID|Zeichenfolge|Ja|Pfad|keine|Geben Sie die Datei|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


## <a name="createfile"></a>CreateFile
Datei erstellen: führt den Upload einer Datei in OneDrive for Business 

```POST: /datasets/default/files``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Ordnerpfad|Zeichenfolge|Ja|Abfrage|keine|Der Pfad zum Hochladen der Datei auf OneDrive for Business|
|Namen|Zeichenfolge|Ja|Abfrage|keine|Name der Datei in OneDrive for Business erstellen|
|Textkörper| |Ja|Textkörper|keine|Inhalt der Datei zum Hochladen auf OneDrive for Business|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


## <a name="copyfile"></a>CopyFile
Kopieren einer Datei: kopiert eine Datei zu OneDrive for Business 

```POST: /datasets/default/copyFile``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Datenquelle|Zeichenfolge|Ja|Abfrage|keine|URL-Quelldatei|
|Ziel|Zeichenfolge|Ja|Abfrage|keine|Zieldateipfad in OneDrive for Business, einschließlich Zieldateiname|
|Überschreiben|Boolesch|Nein|Abfrage|falsch|Überschreibt die Zieldatei aus, wenn auf "True" gesetzt|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


## <a name="onnewfile"></a>OnNewFile
Wenn eine Datei erstellt: einen Fluss auslöst, beim Erstellen eine neue Datei in einer OneDrive for Business-Ordner 

```GET: /datasets/default/triggers/onnewfile``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Ordner-ID|Zeichenfolge|Ja|Abfrage|keine|Geben Sie einen Ordner|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


## <a name="onupdatedfile"></a>OnUpdatedFile
Wenn eine Datei geändert wird: einen Fluss ausgelöst wird, wenn eine Datei in einer OneDrive for Business-Ordner geändert wird 

```GET: /datasets/default/triggers/onupdatedfile``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Ordner-ID|Zeichenfolge|Ja|Abfrage|keine|Geben Sie einen Ordner|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


## <a name="listfolder"></a>ListFolder
Liste der Dateien im Ordner: Listet die Dateien in einer OneDrive for Business-Ordner 

```GET: /datasets/default/folders/{id}``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|ID|Zeichenfolge|Ja|Pfad|keine|Geben Sie den Ordner|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


## <a name="listrootfolder"></a>ListRootFolder
Liste Stammordner: Listet die Dateien im onedrive for Business-Stamm-Ordner 

```GET: /datasets/default/folders``` 

Es sind keine Parameter für diesen Anruf
#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


## <a name="extractfolderv2"></a>ExtractFolderV2
Ordner extrahieren: extrahiert ein Ordners zu OneDrive for Business 

```POST: /datasets/default/extractFolderV2``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Datenquelle|Zeichenfolge|Ja|Abfrage|keine|Pfad der Archivdatei|
|Ziel|Zeichenfolge|Ja|Abfrage|keine|Pfad in OneDrive for Business zum Extrahieren des Inhalts Archiv|
|Überschreiben|Boolesch|Nein|Abfrage|falsch|Überschreibt die Zieldateien, wenn auf "True" gesetzt|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


## <a name="object-definitions"></a>Objektdefinitionen 

### <a name="datasetsmetadata"></a>DataSetsMetadata


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|tabellarische|nicht definiert|Nein |
|BLOB|nicht definiert|Nein |



### <a name="tabulardatasetsmetadata"></a>TabularDataSetsMetadata


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|Datenquelle|Zeichenfolge|Nein |
|displayName|Zeichenfolge|Nein |
|urlEncoding|Zeichenfolge|Nein |
|tableDisplayName|Zeichenfolge|Nein |
|tablePluralName|Zeichenfolge|Nein |



### <a name="blobdatasetsmetadata"></a>BlobDataSetsMetadata


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|Datenquelle|Zeichenfolge|Nein |
|displayName|Zeichenfolge|Nein |
|urlEncoding|Zeichenfolge|Nein |



### <a name="blobmetadata"></a>BlobMetadata


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|ID|Zeichenfolge|Nein |
|Namen|Zeichenfolge|Nein |
|DisplayName|Zeichenfolge|Nein |
|Pfad|Zeichenfolge|Nein |
|LastModified|Zeichenfolge|Nein |
|Größe|ganze Zahl|Nein |
|MediaType|Zeichenfolge|Nein |
|IsFolder|Boolesch|Nein |
|ETag|Zeichenfolge|Nein |
|FileLocator|Zeichenfolge|Nein |



### <a name="object"></a>Objekt


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|


## <a name="next-steps"></a>Nächste Schritte
[Erstellen Sie eine app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md)
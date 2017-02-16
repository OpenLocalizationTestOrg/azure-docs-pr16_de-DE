<properties
    pageTitle="Hinzufügen den Google Drive Verbinder in Logik apps | Microsoft Azure"
    description="Übersicht über die Google Drive Verbinder mit den Parametern REST-API"
    services=""
    suite=""
    documentationCenter="" 
    authors="MandiOhlinger"
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
   ms.author="mandia"/>

# <a name="get-started-with-the-google-drive-connector"></a>Erste Schritte mit der Verbinder Google Drive
Verbinden Sie mit Google Drive zum Erstellen von Dateien, Get-Zeilen und mehr. Mit Google Drive können Sie folgende Aktionen ausführen: 

- Erstellen Sie Ihr Unternehmen Fluss basierend auf den Daten, die Sie bei der Suche zu erhalten. 
- Verwenden von Aktionen zu Bildern suchen, suchen die Informationen und vieles mehr. Diese Aktionen erhalten eine Antwort, und nehmen Sie die Ausgabe dann für andere Aktionen verfügbar. Sie können beispielsweise für ein Video suchen und verwenden Sie dann Twitter an, die für einen Twitter-feed video Posten.

Um einen Vorgang in Logik apps hinzuzufügen, finden Sie unter [Erstellen einer app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).


## <a name="triggers-and-actions"></a>Trigger und Aktionen
Google Drive umfasst die folgenden Aktionen aus. Es gibt keine Trigger aus. 

Trigger | Aktionen
--- | ---
Keine | <ul><li>Datei erstellen</li><li>Zeile einfügen</li><li>Kopieren einer Datei</li><li>Datei löschen</li><li>Zeile löschen</li><li>Extrahieren Archiv in Ordner</li><li>Abrufen der Dateiinhalt mit-id</li><li>Abrufen von Dateiinhalt unter Verwendung der Pfad</li><li>Abrufen von Dateimetadaten, die mit-id</li><li>Abrufen von Metadaten mit Pfad der Datei</li><li>Erste Zeile</li><li>Update-Datei</li><li>Zeile aktualisieren</li></ul>

Alle Connectors unterstützen die Daten in JSON und XML-Formaten.


## <a name="create-the-connection-to-google-drive"></a>Herstellen der Verbindungs mit Google Drive

Wenn Sie Ihre apps Logik dieser Verbinder hinzugefügt haben, müssen Sie Logik apps für die Verbindung zu Ihrem Google Drive autorisieren.

>[AZURE.INCLUDE [Steps to create a connection to googledrive](../../includes/connectors-create-api-googledrive.md)]

Nachdem Sie die Verbindung zu erstellen, geben Sie die Eigenschaften von Google Drive, wie der Ordnername Pfad oder einer Datei an. Die **REST-API-Referenz** in diesem Thema werden diese Eigenschaften beschrieben.

>[AZURE.TIP] Sie können diese dieselbe Google Drive Verbindung in andere apps Logik verwenden.


## <a name="swagger-rest-api-reference"></a>Swagger REST-API-Referenz
Version gilt: 1.0.

### <a name="create-file"></a>Datei erstellen    
Eine Datei hochgeladen auf Google Drive.  
```POST: /datasets/default/files```

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Ordnerpfad|Zeichenfolge|Ja|Abfrage|keine |Der Pfad zum Hochladen der Datei auf Google Drive|
|Namen|Zeichenfolge|Ja|Abfrage|keine |Name der Datei in Google Drive erstellen|
|Textkörper|String(Binary) |Ja|Textkörper| keine|Inhalt der Datei zum Hochladen auf Google Drive|

#### <a name="response"></a>Antwort
|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


### <a name="insert-row"></a>Zeile einfügen    
Fügt eine Zeile in einem Google-Blatt ein.  
```POST: /datasets/{dataset}/tables/{table}/items```

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|DataSet|Zeichenfolge|Ja|Pfad| keine|Eindeutiger Bezeichner der Datei Google Blatt|
|Tabelle|Zeichenfolge|Ja|Pfad|keine |Eindeutiger Bezeichner des Arbeitsblatts|
|Element|ItemInternalId: Zeichenfolge |Ja|Textkörper|keine |Zeile in dem angegebenen Blatt einfügen|

#### <a name="response"></a>Antwort
|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


### <a name="copy-file"></a>Kopieren einer Datei    
Kopiert eine Datei auf Google Drive.  
```POST: /datasets/default/copyFile```

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Datenquelle|Zeichenfolge|Ja|Abfrage| keine|URL-Quelldatei|
|Ziel|Zeichenfolge|Ja|Abfrage|keine |Zieldateipfad in Google Drive, einschließlich Zieldateiname|
|Überschreiben|Boolesch|Nein|Abfrage|keine |Überschreibt die Zieldatei aus, wenn auf "True" gesetzt|

#### <a name="response"></a>Antwort
|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


### <a name="delete-file"></a>Datei löschen    
Löscht eine Datei aus Google Drive.  
```DELETE: /datasets/default/files/{id}```

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|ID|Zeichenfolge|Ja|Pfad|keine |Eindeutiger Bezeichner der Datei löschen aus Google Drive|

#### <a name="response"></a>Antwort
|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


### <a name="delete-row"></a>Zeile löschen    
Löscht eine Zeile aus einem Google-Blatt.  
```DELETE: /datasets/{dataset}/tables/{table}/items/{id}```

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|DataSet|Zeichenfolge|Ja|Pfad|keine |Eindeutiger Bezeichner der Datei Google Blatt|
|Tabelle|Zeichenfolge|Ja|Pfad|keine |Eindeutiger Bezeichner des Arbeitsblatts|
|ID|Zeichenfolge|Ja|Pfad|keine |Eindeutiger Bezeichner des zu löschenden Zeile|

#### <a name="response"></a>Antwort
|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


### <a name="extract-archive-to-folder"></a>Extrahieren Archiv in Ordner    
Eine Archivdatei in einem anderen Ordner in Google Drive extrahiert (Beispiel: ZIP).  
```POST: /datasets/default/extractFolderV2```

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Datenquelle|Zeichenfolge|Ja|Abfrage|keine |Pfad der Archivdatei|
|Ziel|Zeichenfolge|Ja|Abfrage|keine |Pfad in Google Drive, um den Inhalt Archiv extrahieren|
|Überschreiben|Boolesch|Nein|Abfrage|keine |Überschreibt die Zieldateien, wenn auf "True" gesetzt|

#### <a name="response"></a>Antwort
|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


### <a name="get-file-content-using-id"></a>Abrufen der Dateiinhalt mit-id    
Dateiinhalt abruft von Google Drive mit-Id.  
```GET: /datasets/default/files/{id}/content```

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|ID|Zeichenfolge|Ja|Pfad|keine |Eindeutiger Bezeichner der Datei zum Abrufen von Google Drive|

#### <a name="response"></a>Antwort
|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


### <a name="get-file-content-using-path"></a>Abrufen von Dateiinhalt unter Verwendung der Pfad    
Der Inhalt der Datei aus Google Drive mit Pfad abgerufen.  
```GET: /datasets/default/GetFileContentByPath```

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Pfad|Zeichenfolge|Ja|Abfrage|keine |Pfad der Datei in Google Drive|

#### <a name="response"></a>Antwort
|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


### <a name="get-file-metadata-using-id"></a>Abrufen von Dateimetadaten, die mit-id    
Ruft Dateimetadaten von Google Drive mit-Id an.  
```GET: /datasets/default/files/{id}```

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|ID|Zeichenfolge|Ja|Pfad|keine |Eindeutiger Bezeichner der Datei in Google Drive|

#### <a name="response"></a>Antwort
|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


### <a name="get-file-metadata-using-path"></a>Abrufen von Metadaten mit Pfad der Datei    
Ruft Dateimetadaten von Google Drive mit Pfad ein.  
```GET: /datasets/default/GetFileByPath```

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Pfad|Zeichenfolge|Ja|Abfrage|keine |Pfad der Datei in Google Drive|

#### <a name="response"></a>Antwort
|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


### <a name="get-row"></a>Erste Zeile    
Ruft eine einzelne Zeile aus einem Blatt Google ab.  
```GET: /datasets/{dataset}/tables/{table}/items/{id}```

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|DataSet|Zeichenfolge|Ja|Pfad|keine |Eindeutiger Bezeichner der Datei Google Blatt|
|Tabelle|Zeichenfolge|Ja|Pfad|keine |Eindeutiger Bezeichner des Arbeitsblatts|
|ID|Zeichenfolge|Ja|Pfad| keine|Eindeutiger Bezeichner des abzurufenden Zeile|

#### <a name="response"></a>Antwort
|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


### <a name="update-file"></a>Update-Datei    
Eine Datei in Google Drive aktualisiert.  
```PUT: /datasets/default/files/{id}```

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|ID|Zeichenfolge|Ja|Pfad|keine |Eindeutiger Bezeichner der Datei in Google Drive aktualisieren|
|Textkörper|String(Binary) |Ja|Textkörper| keine|Inhalt der Datei zum Hochladen auf Google Drive|

#### <a name="response"></a>Antwort
|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


### <a name="update-row"></a>Zeile aktualisieren    
Eine Zeile auf einem Blatt Google aktualisiert.  
```PATCH: /datasets/{dataset}/tables/{table}/items/{id}```

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|DataSet|Zeichenfolge|Ja|Pfad|keine |Eindeutiger Bezeichner der Datei Google Blatt|
|Tabelle|Zeichenfolge|Ja|Pfad| keine|Eindeutiger Bezeichner des Arbeitsblatts|
|ID|Zeichenfolge|Ja|Pfad|keine |Eindeutiger Bezeichner für die zu aktualisierende Zeile|
|Element|ItemInternalId: Zeichenfolge |Ja|Textkörper|keine |Zeile mit aktualisierten Werte|

#### <a name="response"></a>Antwort
|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


## <a name="object-definitions"></a>Objektdefinitionen

#### <a name="datasetsmetadata"></a>DataSetsMetadata

|Eigenschaftsname | Datentyp | Erforderlich|
|---|---|---|
|tabellarische|nicht definiert|Nein|
|BLOB|nicht definiert|Nein|

#### <a name="tabulardatasetsmetadata"></a>TabularDataSetsMetadata

|Eigenschaftsname | Datentyp |Erforderlich|
|---|---|---|
|Datenquelle|Zeichenfolge|Nein|
|displayName|Zeichenfolge|Nein|
|urlEncoding|Zeichenfolge|Nein|
|tableDisplayName|Zeichenfolge|Nein|
|tablePluralName|Zeichenfolge|Nein|

#### <a name="blobdatasetsmetadata"></a>BlobDataSetsMetadata

|Eigenschaftsname | Datentyp |Erforderlich|
|---|---|---|
|Datenquelle|Zeichenfolge|Nein|
|displayName|Zeichenfolge|Nein|
|urlEncoding|Zeichenfolge|Nein|

#### <a name="blobmetadata"></a>BlobMetadata

|Eigenschaftsname | Datentyp |Erforderlich|
|---|---|---|
|ID|Zeichenfolge|Nein|
|Namen|Zeichenfolge|Nein|
|DisplayName|Zeichenfolge|Nein|
|Pfad|Zeichenfolge|Nein|
|LastModified|Zeichenfolge|Nein|
|Größe|ganze Zahl|Nein|
|MediaType|Zeichenfolge|Nein|
|IsFolder|Boolesch|Nein|
|ETag|Zeichenfolge|Nein|
|FileLocator|Zeichenfolge|Nein|

#### <a name="tablemetadata"></a>TableMetadata

|Eigenschaftsname | Datentyp |Erforderlich|
|---|---|---|
|Namen|Zeichenfolge|Nein|
|Titel|Zeichenfolge|Nein|
|X-ms-Berechtigung|Zeichenfolge|Nein|
|Schema|nicht definiert|Nein|

#### <a name="tableslist"></a>TablesList

|Eigenschaftsname | Datentyp |Erforderlich|
|---|---|---|
|Wert|Matrix|Nein|

#### <a name="table"></a>Tabelle

|Eigenschaftsname | Datentyp |Erforderlich|
|---|---|---|
|Namen|Zeichenfolge|Nein|
|DisplayName|Zeichenfolge|Nein|

#### <a name="item"></a>Element

|Eigenschaftsname | Datentyp |Erforderlich|
|---|---|---|
|ItemInternalId|Zeichenfolge|Nein|

#### <a name="itemslist"></a>ItemsList

|Eigenschaftsname | Datentyp |Erforderlich|
|---|---|---|
|Wert|Matrix|Nein|


## <a name="next-steps"></a>Nächste Schritte

[Erstellen einer app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).

Kehren Sie zu der [Liste der APIs](apis-list.md).


<!--References-->
[5]: https://console.developers.google.com/
[6]: ./media/connectors-create-api-googledrive/google-developers-console.png
[8]: ./media/connectors-create-api-googledrive/use-google-apis.png
[9]: ./media/connectors-create-api-googledrive/googledrive-api-overview.png
[10]: ./media/connectors-create-api-googledrive/enable-googledrive-api.png
[12]: ./media/connectors-create-api-googledrive/googledrive-api-credentials-add.png
[13]: ./media/connectors-create-api-googledrive/configure-consent-screen.png
[14]: ./media/connectors-create-api-googledrive/create-client-id.png

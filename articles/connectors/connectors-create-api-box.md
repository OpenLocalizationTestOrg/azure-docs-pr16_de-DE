<properties
    pageTitle="Den Feld Verbinder zu Ihrer Apps Logik hinzufügen | Microsoft Azure"
    description="Übersicht über den Verbinder Feld mit den Parametern REST-API"
    services=""
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

# <a name="get-started-with-the-box-connector"></a>Erste Schritte mit dem Feld connector
Herstellen einer Verbindung Feld mit und Erstellen von Dateien, Dateien löschen und vieles mehr. 

>[AZURE.NOTE] Diese Version des Artikels gilt Logik apps 2015-08-01-Schema Vorschauversion aus.

Mit Feld können Sie folgende Aktionen ausführen:

- Erstellen Sie Ihrer Business Fluss basierend auf den Daten, die Sie von Feld zu gelangen. 
- Verwenden Sie Trigger aus, wenn eine Datei erstellt oder aktualisiert wird.
- Verwenden Sie die Aktionen, die das Kopieren einer Datei, und löschen Sie eine Datei und vieles mehr. Diese Aktionen erhalten eine Antwort, und nehmen Sie die Ausgabe dann für andere Aktionen verfügbar. Wenn eine Datei auf Feld geändert wird, können Sie beispielsweise Ausführen dieser Datei und senden Sie per e-Mail mit Office 365.

Um einen Vorgang in Logik apps hinzuzufügen, finden Sie unter [Erstellen einer app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Trigger und Aktionen
Klicken Sie im umfasst die folgenden Trigger und Aktionen.

| Trigger | Aktionen|
| --- | --- |
|<ul><li>Wenn eine Datei erstellt wird</li><li>Wenn eine Datei geändert wird.</li></ul> | <ul><li>Datei erstellen</li><li>Wenn eine Datei erstellt wird</li><li>Kopieren einer Datei</li><li>Datei löschen</li><li>Extrahieren Archiv in Ordner</li><li>Abrufen der Dateiinhalt mit-id</li><li>Abrufen von Dateiinhalt unter Verwendung der Pfad</li><li>Abrufen von Dateimetadaten, die mit-id</li><li>Abrufen von Metadaten mit Pfad der Datei</li><li>Update-Datei</li><li>Wenn eine Datei geändert wird.</li></ul>

Alle Connectors unterstützen die Daten in JSON und XML-Formaten.

## <a name="create-a-connection-to-box"></a>Herstellen einer Verbindung mit dem Feld
Wenn Sie Ihre apps Logik dieser Verbinder hinzugefügt haben, müssen Sie Logik apps für die Verbindung zu Ihrem Postfach autorisieren.

>[AZURE.INCLUDE [Steps to create a connection to box](../../includes/connectors-create-api-box.md)]

Nachdem Sie die Verbindung zu erstellen, geben Sie im Dialogfeld Eigenschaften. Die **REST-API-Referenz** in diesem Thema werden diese Eigenschaften beschrieben.

>[AZURE.TIP] Sie können diese gleiche Feld Verbindung in andere apps Logik verwenden.

## <a name="swagger-rest-api-reference"></a>Swagger REST-API-Referenz
Version gilt: 1.0.

### <a name="create-file"></a>Datei erstellen
Eine Datei hochgeladen auf Feld.  
```POST: /datasets/default/files```

| Namen|Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Ordnerpfad|Zeichenfolge|Ja|Abfrage|Keine |Der Pfad zum Hochladen der Datei auf Feld|
|Namen|Zeichenfolge|Ja|Abfrage|Keine |Name der Datei, die das Erstellen|
|Textkörper|String(Binary) |Ja|Textkörper|Keine |Inhalt der Datei zum Hochladen auf Feld|

#### <a name="response"></a>Antwort
|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


### <a name="when-a-file-is-created"></a>Wenn eine Datei erstellt wird
Auslösen einen Fluss Wenn eine neue Datei in einem Feld Ordner erstellt wird.  
```GET: /datasets/default/triggers/onnewfile```

| Namen|Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Ordner-ID|Zeichenfolge|Ja|Abfrage|Keine |Eindeutiger Bezeichner des Ordners im Feld|

#### <a name="response"></a>Antwort
|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


### <a name="copy-file"></a>Kopieren einer Datei
Kopiert eine Datei im Feld an.  
```POST: /datasets/default/copyFile```

| Namen|Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Datenquelle|Zeichenfolge|Ja|Abfrage|Keine |URL-Quelldatei|
|Ziel|Zeichenfolge|Ja|Abfrage| Keine|Zieldateipfad Feld, einschließlich Zieldateiname|
|Überschreiben|Boolesch|Nein|Abfrage| Keine|Überschreibt die Zieldatei aus, wenn auf "True" gesetzt|

#### <a name="response"></a>Antwort
|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


### <a name="delete-file"></a>Datei löschen
Löscht eine Datei im Feld aus.  
```DELETE: /datasets/default/files/{id}```


| Namen|Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|ID|Zeichenfolge|Ja|Pfad|Keine |Eindeutiger Bezeichner der Datei aus dem Feld löschen|

#### <a name="response"></a>Antwort
|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


### <a name="extract-archive-to-folder"></a>Extrahieren Archiv in Ordner
Eine Archivdatei in einem anderen Ordner im Dialogfeld extrahiert (Beispiel: ZIP).  
```POST: /datasets/default/extractFolderV2```

| Namen|Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Datenquelle|Zeichenfolge|Ja|Abfrage| |Pfad der Archivdatei|
|Ziel|Zeichenfolge|Ja|Abfrage| |Pfad im Feld Extrahieren des Inhalts Archiv|
|Überschreiben|Boolesch|Nein|Abfrage| |Überschreibt die Zieldateien, wenn auf "True" gesetzt|

#### <a name="response"></a>Antwort
|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


### <a name="get-file-content-using-id"></a>Abrufen der Dateiinhalt mit-id
Ruft Dateiinhalt Feld mit-Id an.  
```GET: /datasets/default/files/{id}/content```

| Namen|Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|ID|Zeichenfolge|Ja|Pfad|Keine |Eindeutiger Bezeichner der Datei im Feld|

#### <a name="response"></a>Antwort
|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


### <a name="get-file-content-using-path"></a>Abrufen von Dateiinhalt unter Verwendung der Pfad
Ruft Dateiinhalt von Feld Pfad ein.  
```GET: /datasets/default/GetFileContentByPath```

| Namen|Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Pfad|Zeichenfolge|Ja|Abfrage|Keine |Eindeutige Pfad zu der Datei im Dialogfeld|

#### <a name="response"></a>Antwort
|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


### <a name="get-file-metadata-using-id"></a>Abrufen von Dateimetadaten, die mit-id
Ruft Dateimetadaten aus Feld mit Datei-Id an.  
```GET: /datasets/default/files/{id}```

| Namen|Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|ID|Zeichenfolge|Ja|Pfad| Keine|Eindeutiger Bezeichner der Datei im Feld|

#### <a name="response"></a>Antwort
|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


### <a name="get-file-metadata-using-path"></a>Abrufen von Metadaten mit Pfad der Datei
Ruft Dateimetadaten von Feld Pfad ein.  
```GET: /datasets/default/GetFileByPath```

| Namen|Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Pfad|Zeichenfolge|Ja|Abfrage|Keine |Eindeutige Pfad zu der Datei im Dialogfeld|

#### <a name="response"></a>Antwort
|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


### <a name="update-file"></a>Update-Datei
Eine Datei im Dialogfeld aktualisiert.  
```PUT: /datasets/default/files/{id}```

| Namen|Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|ID|Zeichenfolge|Ja|Pfad| Keine|Eindeutiger Bezeichner der Datei im Dialogfeld aktualisieren|
|Textkörper|String(Binary) |Ja|Textkörper|Keine |Inhalt der Datei im Dialogfeld aktualisieren|

#### <a name="response"></a>Antwort
|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


### <a name="when-a-file-is-modified"></a>Wenn eine Datei geändert wird.
Löst einen Fluss, wenn eine Datei in einem Ordner im Feld geändert wird.  
```GET: /datasets/default/triggers/onupdatedfile```

| Namen|Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Ordner-ID|Zeichenfolge|Ja|Abfrage|Keine |Eindeutiger Bezeichner des Ordners im Feld|

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

## <a name="next-steps"></a>Nächste Schritte

[Erstellen einer app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).

<properties
    pageTitle="Hinzufügen den Azure Blob-Speicher Verbinder in Ihrer Apps Logik | Microsoft Azure"
    description="Übersicht über Azure BLOB-Speicher Verbinder mit den Parametern REST-API"
    services=""
    documentationCenter="" 
    authors="MandiOhlinger"
    manager="anneta"
    editor=""
    tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="10/18/2016"
   ms.author="mandia"/>

# <a name="get-started-with-the-azure-blob-storage-connector"></a>Erste Schritte mit der Azure Blob-Speicher Verbinder
Azure Blob-Speicher ist ein Dienst für große Mengen von unstrukturierten Daten speichern. Führen Sie verschiedene Aktionen wie z. B. hochladen, aktualisieren, abrufen und Löschen von Blobs in Azure Blob-Speicher. 

Mit Azure Blob-Speicher Sie:

- Erstellen Sie den Workflow nach dem Hochladen der neuen Projekte oder Abrufen von Dateien, die zuletzt aktualisiert wurden.
- Verwenden von Aktionen zum Aufrufen der Dateimetadaten Löschen einer Datei, Kopieren von Dateien und mehr. Angenommen, bei der Aktualisierung eines Tools in einer Azure-Website (Trigger), klicken Sie dann Aktualisieren einer Datei im Blob-Speicher (eine Aktion). 

In diesem Thema wird gezeigt, wie den BLOB-Speicher Verbinder in einer app Logik verwenden, und es werden auch die Aktionen aufgelistet.

>[AZURE.NOTE] Diese Version des Artikels gilt Logik Apps allgemeine Verfügbarkeit (GA) aus. 

Weitere Informationen zu Logik Apps finden Sie unter [Was sind die Logik apps](../app-service-logic/app-service-logic-what-are-logic-apps.md) , und [Erstellen Sie eine app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="connect-to-azure-blob-storage"></a>Verbinden Sie mit Azure Blob-Speicher

Bevor Sie Ihre app Logik Dienste zugreifen kann, erstellen Sie zuerst eine *Verbindung* mit dem Dienst an. Eine Verbindung stellt eine Verbindung zwischen einer app Logik und einem anderen Dienst. Beispielsweise erstellen zum Verbinden mit einem Speicherkonto Sie zuerst eine BLOB-Speicher *Verbindung*. Um eine Verbindung herzustellen, geben Sie die Anmeldeinformationen, die Sie normalerweise verwenden, um den Zugriff auf den Dienst, den, dem Sie eine Verbindung herstellen. Geben Sie also mit Azure-Speicher, die Anmeldeinformationen bei Ihrem Speicherkonto, um die Verbindung zu erstellen. 

#### <a name="create-the-connection"></a>Herstellen der Verbindungs

>[AZURE.INCLUDE [Create a connection to Azure blob storage](../../includes/connectors-create-api-azureblobstorage.md)]
 
## <a name="use-a-trigger"></a>Verwenden eines Triggers

Alle Trigger keinen dieser Verbinder. Verwenden Sie andere Trigger So starten Sie die app Logik einer Serie auslösen, eine HTTP-Webhook auslösen, Trigger mit anderen Verbindern und vieles mehr zur Verfügung. [Erstellen einer app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md) enthält ein Beispiel.

## <a name="use-an-action"></a>Verwenden Sie eine Aktion
    
Eine Aktion ist ein Vorgang durchgeführten durch den Workflow in einer app Logik definiert.

1. Wählen Sie das Pluszeichen (+) aus. Die Reihe von Optionen angezeigt: **Hinzufügen einer Aktion**, **Hinzufügen einer Bedingung**oder eine der **Weitere** Optionen.

    ![](./media/connectors-create-api-azureblobstorage/add-action.png)

2. Wählen Sie **eine Aktion hinzufügen**.

3. Geben Sie im Textfeld "blob" aus, um eine Liste aller verfügbaren Aktionen zu erhalten.

    ![](./media/connectors-create-api-azureblobstorage/actions.png) 

4. Wählen Sie in diesem Beispiel **AzureBlob - Metadaten mithilfe der Pfad der Datei zu erhalten**. Wenn Sie bereits eine Verbindung besteht, wählen Sie dann die **...** (Datumsauswahl anzeigen)-Schaltfläche, um eine Datei auszuwählen.

    ![](./media/connectors-create-api-azureblobstorage/sample-file.png)

    Wenn Sie für die Verbindungsinformationen aufgefordert werden, geben Sie dann die Details, um die Verbindung zu erstellen. [Erstellen Sie die Verbindung](connectors-create-api-azureblobstorage.md#create-the-connection) in diesem Thema werden diese Eigenschaften beschrieben. 

    > [AZURE.NOTE] In diesem Beispiel erhalten wir die Metadaten einer Datei an. Wenn die Metadaten sehen zu können, fügen Sie eine andere Aktion, die eine neue Datei mit einem anderen Connector erstellt wird. Angenommen, Hinzufügen einer OneDrive-Aktion, die eine neue "test" Datei auf der Grundlage der Metadaten erstellt wird. 

5. **Speichern** der Änderungen (oberen linken Ecke der Symbolleiste). Ihre app Logik wird gespeichert und automatisch aktiviert werden kann.

> [AZURE.TIP] [Speicher-Explorer](http://storageexplorer.com/) ist ein großartiges Tool zum Verwalten von mehreren Speicherkonten.

## <a name="technical-details"></a>Technische Details

## <a name="storage-blob-actions"></a>Speicher Blob-Aktionen

|Aktion|Beschreibung|
|--- | ---|
|[Abrufen von Dateimetadaten](connectors-create-api-azureblobstorage.md#get-file-metadata)|Dieser Vorgang ruft Dateimetadaten mit Datei-Id an.|
|[Update-Datei](connectors-create-api-azureblobstorage.md#update-file)|Dieser Vorgang aktualisiert eine Datei an.|
|[Datei löschen](connectors-create-api-azureblobstorage.md#delete-file)|Dieser Vorgang löscht eine Datei.|
|[Abrufen von Metadaten mit Pfad der Datei](connectors-create-api-azureblobstorage.md#get-file-metadata-using-path)|Dieser Vorgang ruft Metadaten mit dem Pfad der Datei ab.|
|[Abrufen von Dateiinhalt unter Verwendung der Pfad](connectors-create-api-azureblobstorage.md#get-file-content-using-path)|Dieser Vorgang ruft Dateiinhalt mithilfe des Pfads ab.|
|[Abrufen der Inhalt der Datei](connectors-create-api-azureblobstorage.md#get-file-content)|Dieser Vorgang ruft Dateiinhalt mit-Id an.|
|[Datei erstellen](connectors-create-api-azureblobstorage.md#create-file)|Dieser Vorgang lädt eine Datei hoch.|
|[Kopieren einer Datei](connectors-create-api-azureblobstorage.md#copy-file)|Dieser Vorgang kopiert eine Datei in Azure BLOB-Speicher.|
|[Extrahieren Archiv in Ordner](connectors-create-api-azureblobstorage.md#extract-archive-to-folder)|Dieser Vorgang extrahiert eine Archivdatei in einem anderen Ordner (Beispiel: ZIP).|

### <a name="action-details"></a>Aktionsdetails

In diesem Abschnitt finden Sie unter der bestimmte Details zu jeder Aktion, einschließlich alle erforderlichen oder optionalen von Eigenschaften und eine entsprechende Ausgabe der Verbinder zugeordnet.

#### <a name="get-file-metadata"></a>Abrufen von Dateimetadaten
Dieser Vorgang ruft Dateimetadaten mit Datei-Id an.  

|Eigenschaftsname| Anzeigename|Beschreibung|
| ---|---|---|
|ID *|Datei|Wählen Sie eine Datei|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Die Ausgabedetails
BlobMetadata

| Eigenschaftsname | Datentyp |
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


#### <a name="update-file"></a>Update-Datei
Dieser Vorgang aktualisiert eine Datei an.  

|Eigenschaftsname| Anzeigename|Beschreibung|
| ---|---|---|
|ID *|Datei|Wählen Sie eine Datei|
|Textkörper *|Der Inhalt der Datei|Inhalt der Datei zu aktualisieren|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Die Ausgabedetails
BlobMetadata

| Eigenschaftsname | Datentyp |
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


#### <a name="delete-file"></a>Datei löschen
Dieser Vorgang löscht eine Datei.  

|Eigenschaftsname| Anzeigename|Beschreibung|
| ---|---|---|
|ID *|Datei|Wählen Sie eine Datei|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Die Ausgabedetails
Keine.


#### <a name="get-file-metadata-using-path"></a>Abrufen von Metadaten mit Pfad der Datei
Dieser Vorgang ruft Metadaten mit dem Pfad der Datei ab.  

|Eigenschaftsname| Anzeigename|Beschreibung|
| ---|---|---|
|Pfad *|Dateipfad|Wählen Sie eine Datei|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Die Ausgabedetails
BlobMetadata

| Eigenschaftsname | Datentyp |
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


#### <a name="get-file-content-using-path"></a>Abrufen von Dateiinhalt unter Verwendung der Pfad
Dieser Vorgang ruft Dateiinhalt mithilfe des Pfads ab.  

|Eigenschaftsname| Anzeigename|Beschreibung|
| ---|---|---|
|Pfad *|Dateipfad|Wählen Sie eine Datei|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Die Ausgabedetails
Keine.


#### <a name="get-file-content"></a>Abrufen der Inhalt der Datei
Dieser Vorgang ruft Dateiinhalt mit-Id an.  

|Eigenschaftsname| Datentyp|Beschreibung|
| ---|---|---|
|ID *|Zeichenfolge|Wählen Sie eine Datei|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Die Ausgabedetails
Keine.


#### <a name="create-file"></a>Datei erstellen
Dieser Vorgang lädt eine Datei hoch.  

|Eigenschaftsname| Anzeigename|Beschreibung|
| ---|---|---|
|Ordnerpfad *|Ordnerpfad|Wählen Sie einen Ordner aus.|
|Namen *|Dateiname|Namen der hochzuladenden Datei|
|Textkörper *|Der Inhalt der Datei|Inhalt der hochzuladenden Datei|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Die Ausgabedetails
BlobMetadata

| Eigenschaftsname | Datentyp | 
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


#### <a name="copy-file"></a>Kopieren einer Datei
Dieser Vorgang kopiert eine Datei in Azure BLOB-Speicher.  

|Eigenschaftsname| Anzeigename|Beschreibung|
| ---|---|---|
|Quelle *|Url der Quelle|Geben Sie die Url in der Quelldatei|
|Ziel *|Zieldateipfad|Geben Sie den Zieldateipfad, einschließlich der Zieldatei|
|Überschreiben|Überschreiben?|Eine vorhandenen Zieldatei (wahr/falsch) überschrieben werden sollen?  |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Die Ausgabedetails
BlobMetadata

| Eigenschaftsname | Datentyp |
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

#### <a name="extract-archive-to-folder"></a>Extrahieren Archiv in Ordner
Dieser Vorgang extrahiert eine Archivdatei in einem anderen Ordner (Beispiel: ZIP).  

|Eigenschaftsname| Anzeigename|Beschreibung|
| ---|---|---|
|Quelle *|Der Pfad der Archivdatei Quelle|Wählen Sie eine Archivdatei aus.|
|Ziel *|Zielordnerpfad|Wählen Sie die zu extrahierenden Inhalts|
|Überschreiben|Überschreiben?|Eine vorhandenen Zieldatei (wahr/falsch) überschrieben werden sollen?|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Die Ausgabedetails
BlobMetadata

| Eigenschaftsname | Datentyp |
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


## <a name="http-responses"></a>HTTP-Antworten

Wenn die anderen Aktionen aufrufen, möglicherweise bestimmte Antworten erhalten haben. In der folgenden Tabelle werden die Antworten und deren Beschreibung an:  

|Namen|Beschreibung|
|---|---|
|200|Okay|
|202|Akzeptiert|
|400|Ungültige Anforderung|
|401|Nicht autorisierte|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannter Fehler aufgetreten ist.|
|Standard|Fehler bei Vorgang.|

## <a name="next-steps"></a>Nächste Schritte

[Erstellen einer app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md). Untersuchen der verfügbaren Connectors in Logik Apps auf unserer [APIs Liste](apis-list.md)an.




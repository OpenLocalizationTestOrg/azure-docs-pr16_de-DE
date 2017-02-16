<properties
    pageTitle="Herstellen einer Verbindung DocumentDB mit Azure Search Verwenden von Indexern | Microsoft Azure"
    description="In diesem Artikel wird gezeigt, wie an Azure Suche Index mit DocumentDB als Datenquelle verwendet werden kann."
    services="documentdb"
    documentationCenter=""
    authors="dennyglee"
    manager="jhubbard"
    editor="mimig"/>

<tags
    ms.service="documentdb"
    ms.devlang="rest-api"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-services"
    ms.date="07/08/2016"
    ms.author="denlee"/>

#<a name="connecting-documentdb-with-azure-search-using-indexers"></a>Herstellen einer Verbindung DocumentDB mit Azure Search Indexer verwenden

Wenn Sie zum Implementieren der Suche großartige Erfahrung über Ihre Daten DocumentDB gefunden haben, verwenden Sie für DocumentDB Azure Search Indexer! In diesem Artikel wird gezeigt, wie in Azure Suche Azure DocumentDB integrieren, ohne Schreiben von Code um Indizierung Infrastruktur aufrechterhalten werden!

Um dies einzurichten, müssen Sie das [Einrichten eines Kontos Azure-Suche](../search/search-create-service-portal.md) (nicht müssen Sie nach einem upgrade auf standard-Suche), und rufen Sie die [Azure Suche REST-API](https://msdn.microsoft.com/library/azure/dn798935.aspx) zum Erstellen einer DocumentDB **Datenquelle** und **Indexer** für die Datenquelle.

In der Reihenfolge senden Anfragen mit den restlichen APIs interagieren können Sie [Postman](https://www.getpostman.com/), [Fiddler](http://www.telerik.com/fiddler)oder eines beliebigen Tools für die gewünschte Einstellung.


##<a name="a-idconceptsaazure-search-indexer-concepts"></a><a id="Concepts"></a>Azure Suche Indexer Konzepte

Azure Suche unterstützt das Erstellen und Verwalten von Daten als Quelle (einschließlich DocumentDB) und Indexern, die in diesen Datenquellen verwendet werden.

Eine **Datenquelle** gibt an, welche Daten indiziert werden müssen, Anmeldeinformationen für den Zugriff auf die Daten und Richtlinien für den Azure-Suche auf effizient identifizieren von Änderungen in den Daten (z. B. geändert oder gelöscht Dokumente in Ihrer Websitesammlung). Die Datenquelle ist als eine unabhängige Ressource definiert, damit es von mehrere Indexer verwendet werden kann.

Ein **Indexer** beschrieben, wie die Daten aus der Datenquelle in einer Zielliste Suchindex fließt. Planen Sie eine Indexersteller für jede Kombination Ziel Index und Daten Quelle zu erstellen. Während Sie mehrere Indexer in denselben Index Schreiben verfügen können, kann Indexer nur in einem einzigen Index schreiben. Ein Indexer wird verwendet, um:

- Führen Sie eine einmalige Kopie der Daten ein Indexes gefüllt wird.
- Synchronisieren eines Indexes mit Änderungen in der Datenquelle nach einem Zeitplan. Der Zeitplan ist Bestandteil der Definition Indexer.
- Rufen Sie bei Bedarf Updates an einen Index, je nach Bedarf an.

##<a name="a-idcreatedatasourceastep-1-create-a-data-source"></a><a id="CreateDataSource"></a>Schritt 1: Erstellen einer Datenquelle

So erstellen Sie eine neue Datenquelle in Ihrer Azure Suchdienst, einschließlich der folgenden Überschriften der Anforderung eine HTTP POST-Anforderung ' Problemdetails '.

    POST https://[Search service name].search.windows.net/datasources?api-version=[api-version]
    Content-Type: application/json
    api-key: [Search service admin key]

Die `api-version` ist erforderlich. Gültige Werte sind `2015-02-28` oder einer späteren Version. Besuchen Sie [API Versionen in Azure suchen](../search/search-api-versions.md) , um alle unterstützten Suche API-Versionen finden Sie unter.

Hauptteil der Anforderung enthält, die die DSN-Datei, die die folgenden Felder einbezogen werden sollen:

- **Name**: Wählen Sie einen beliebigen Namen zu Ihrer Datenbank DocumentDB darstellen.

- **Type**: Verwenden Sie `documentdb`.

- **Anmeldeinformationen**:

    - **ConnectionString**: erforderlich. Geben Sie die Verbindungsinformationen zu Ihrer DocumentDB Azure-Datenbank im folgenden Format an:`AccountEndpoint=<DocumentDB endpoint url>;AccountKey=<DocumentDB auth key>;Database=<DocumentDB database id>`

- **Container**:

    - **Name**: erforderlich. Geben Sie die Id der Sammlung DocumentDB indiziert werden sollen.

    - **Abfrage**: Optional. Sie können angeben, dass eine Abfrage, um ein beliebiges JSON-Dokument in einer flachen Schema zu reduzieren, die Suche Azure indizieren können.

- **DataChangeDetectionPolicy**: Optional. Finden Sie unter [Ändern von Daten Erkennung Richtlinie](#DataChangeDetectionPolicy) unten.

- **DataDeletionDetectionPolicy**: Optional. Finden Sie unter [Daten Löschvorgang Erkennung Richtlinie](#DataDeletionDetectionPolicy) unten.

Nachfolgend finden Sie ein [Beispiel Anforderungstexts](#CreateDataSourceExample).

###<a name="a-iddatachangedetectionpolicyacapturing-changed-documents"></a><a id="DataChangeDetectionPolicy"></a>Erfassen geänderte Dokumente

Der Zweck einer Daten ändern Erkennung Richtlinie ist effizient geänderten Datenelemente zu identifizieren. Derzeit die einzige unterstützte Richtlinie ist die `High Water Mark` Richtlinie mithilfe der `_ts` Zeitstempel der letzten Änderung angegebenen Eigenschaft nach DocumentDB - die wie folgt definiert ist:

    {
        "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
        "highWaterMarkColumnName" : "_ts"
    }

Sie müssen außerdem hinzufügen `_ts` in der Projektion und `WHERE` -Klausel für die Abfrage. Beispiel:

    SELECT s.id, s.Title, s.Abstract, s._ts FROM Sessions s WHERE s._ts >= @HighWaterMark


###<a name="a-iddatadeletiondetectionpolicyacapturing-deleted-documents"></a><a id="DataDeletionDetectionPolicy"></a>Erfassen gelöschte Dokumente

Wenn Sie Zeilen aus der Quelltabelle gelöscht haben, sollten Sie die Zeilen aus dem Suchindex auch löschen. Der Zweck einer Daten Löschvorgang Erkennung Richtlinie ist effizient gelöschten Datenelemente zu identifizieren. Derzeit die einzige unterstützte Richtlinie ist die `Soft Delete` Richtlinie (Löschvorgang ist durch eine Kennzeichnung irgendeiner gekennzeichnet), die wie folgt angegeben ist:

    {
        "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
        "softDeleteColumnName" : "the property that specifies whether a document was deleted",
        "softDeleteMarkerValue" : "the value that identifies a document as deleted"
    }

> [AZURE.NOTE] Sie müssen die SoftDeleteColumnName-Eigenschaft in der SELECT-Klausel einbeziehen, wenn Sie eine benutzerdefinierte Projektion verwenden.

###<a name="a-idcreatedatasourceexamplearequest-body-example"></a><a id="CreateDataSourceExample"></a>Beispiel für Textkörper anfordern

Im folgende Beispiel wird eine Datenquelle mit einer benutzerdefinierten Abfrage und Richtlinie Fehlerzeichenfolgen erstellt:

    {
        "name": "mydocdbdatasource",
        "type": "documentdb",
        "credentials": {
            "connectionString": "AccountEndpoint=https://myDocDbEndpoint.documents.azure.com;AccountKey=myDocDbAuthKey;Database=myDocDbDatabaseId"
        },
        "container": {
            "name": "myDocDbCollectionId",
            "query": "SELECT s.id, s.Title, s.Abstract, s._ts FROM Sessions s WHERE s._ts > @HighWaterMark"
        },
        "dataChangeDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
            "highWaterMarkColumnName": "_ts"
        },
        "dataDeletionDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
            "softDeleteColumnName": "isDeleted",
            "softDeleteMarkerValue": "true"
        }
    }

###<a name="response"></a>Antwort

Sie erhalten eine Antwort HTTP 201 erstellt, wenn die Datenquelle erfolgreich erstellt wurde.

##<a name="a-idcreateindexastep-2-create-an-index"></a><a id="CreateIndex"></a>Schritt 2: Erstellen eines Indexes

Wenn Sie eine bereits besitzen, erstellen Sie einen Ziel Azure Suchindex. Dies ist von der [Azure-Portal-Benutzeroberfläche](../search/search-create-index-portal.md) oder mithilfe der [Index-API erstellen](https://msdn.microsoft.com/library/azure/dn798941.aspx)möglich.

    POST https://[Search service name].search.windows.net/indexes?api-version=[api-version]
    Content-Type: application/json
    api-key: [Search service admin key]


Stellen Sie sicher, dass das Schema der der Zielindex für das Schema der Datenquelle JSON Dokumente oder die Ausgabe der benutzerdefinierte Abfrageprojektion kompatibel ist.

>[AZURE.NOTE] Für partitionierte Websitesammlungen, ist die Standard-Dokument-Taste des DocumentDB `_rid` -Eigenschaft, die auf umbenannt ruft `rid` in Azure suchen. Darüber hinaus DocumentDBs `_rid` Werte enthalten Zeichen, die in Azure Suche Tasten; ungültig sind daher der `_rid` Werte sind Base64-codierte.

###<a name="figure-a-mapping-between-json-data-types-and-azure-search-data-types"></a>Abbildung A: Zuordnung zwischen JSON-Datentypen und Datentypen Azure suchen

| JSON-DATENTYP|   INDEX-FELDTYPEN KOMPATIBEL ZIEL|
|---|---|
|Bool|Edm.Boolean, Edm.String|
|Zahlen, die ganze Zahlen aussehen|Edm.Int32, Int64, Edm.String|
|Zahlen, die anscheinend beweglich Punkte|Edm.Double, Edm.String|
|Zeichenfolge|Edm.String|
|Arrays von Element eingibt, z. B. "a", "b", "C" |Collection(EDM.String)|
|Zeichenfolgen, die Datumsangaben aussehen| Edm.DateTimeOffset, Edm.String|
|GeoJSON Objekte, z. B. {"Typ": "Punkt", "Koordinaten": [lange, Lat]} | Edm.GeographyPoint |
|Andere JSON-Objekte|N/V|

###<a name="a-idcreateindexexamplearequest-body-example"></a><a id="CreateIndexExample"></a>Beispiel für Textkörper anfordern

Im folgende Beispiel erstellt einen Index mit einem Feld Kennung und eine Beschreibung an:

    {
       "name": "mysearchindex",
       "fields": [{
         "name": "id",
         "type": "Edm.String",
         "key": true,
         "searchable": false
       }, {
         "name": "description",
         "type": "Edm.String",
         "filterable": false,
         "sortable": false,
         "facetable": false,
         "suggestions": true
       }]
     }

###<a name="response"></a>Antwort

Sie erhalten eine Antwort HTTP 201 erstellt, wenn der Index erfolgreich erstellt wurde.

##<a name="a-idcreateindexerastep-3-create-an-indexer"></a><a id="CreateIndexer"></a>Schritt 3: Erstellen eines Indexers

Sie können einen neuen Indexer innerhalb einer Azure Suchdienst mithilfe einer HTTP POST-Anforderung mit den folgenden Überschriften erstellen.

    POST https://[Search service name].search.windows.net/indexers?api-version=[api-version]
    Content-Type: application/json
    api-key: [Search service admin key]

Hauptteil der Anforderung enthält die Definition Indexer, die die folgenden Felder enthalten sollte:

- **Name**: erforderlich. Der Name des Indexers.

- **DataSourceName**: erforderlich. Der Name einer vorhandenen Datenquelle.

- **TargetIndexName**: erforderlich. Der Name eines vorhandenen Indexes.

- **Zeitplan**: Optional. Finden Sie unter folgenden [Indizierung Terminplan](#IndexingSchedule) .

###<a name="a-idindexingschedulearunning-indexers-on-a-schedule"></a><a id="IndexingSchedule"></a>Indexer für einen Zeitplan ausgeführt

Indexer kann optional einen Zeitplan festlegen. Wenn ein Terminplan vorhanden ist, wird der Indexer regelmäßig gemäß Terminplan ausgeführt. Zeitplan weist die folgenden Attribute:

- **Intervall**: erforderlich. Ein Wert für die Dauer, der angibt, ein Intervall oder den Zeitraum für Indexer ausgeführt wird. Das kleinste zulässige Intervall ist 5 Minuten. der längsten beträgt einen Tag. Es muss als "DayTimeDuration" XSD-Wert (einer Teilmenge eingeschränkten eines Werts [ISO 8601 Dauer](http://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) ) formatiert werden. Das Muster für Dies ist: `P(nD)(T(nH)(nM))`. Beispiele: `PT15M` für alle 15 Minuten, `PT2H` für alle 2 Stunden.

- **Startzeit**: erforderlich. Eine UTC-Datetime, der angibt, wenn der Indexer gestartet werden soll, ausgeführt.

###<a name="a-idcreateindexerexamplearequest-body-example"></a><a id="CreateIndexerExample"></a>Beispiel für Textkörper anfordern

Im folgenden Beispiel wird einen Indexer, der Daten in der Websitesammlung optimiert kopiert die `myDocDbDataSource` Datenquellen auf der `mySearchIndex` Index nach einem Zeitplan, die am 1. Jan 2015 UTC startet und stündlich führt.

    {
        "name" : "mysearchindexer",
        "dataSourceName" : "mydocdbdatasource",
        "targetIndexName" : "mysearchindex",
        "schedule" : { "interval" : "PT1H", "startTime" : "2015-01-01T00:00:00Z" }
    }

###<a name="response"></a>Antwort

Sie erhalten eine Antwort HTTP 201 erstellt, wenn der Indexer erfolgreich erstellt wurde.

##<a name="a-idrunindexerastep-4-run-an-indexer"></a><a id="RunIndexer"></a>Schritt 4: Ausführen ein Indexers

Zusätzlich zur Ausführung von regelmäßig nach einem Zeitplan, kann ein Indexer auch bei Bedarf aufgerufen werden durch die folgenden HTTP POST Anforderung:

    POST https://[Search service name].search.windows.net/indexers/[indexer name]/run?api-version=[api-version]
    api-key: [Search service admin key]

###<a name="response"></a>Antwort

Sie erhalten eine Antwort HTTP 202 akzeptiert, wenn der Indexer erfolgreich aufgerufen wurde.

##<a name="a-namegetindexerstatusastep-5-get-indexer-status"></a><a name="GetIndexerStatus"></a>Schritt 5: Abrufen Sie Indizierungsstatus

Sie können eine HTTP GET-Anforderung zum Abrufen des aktuellen Status und Ausführung Verlaufs eines Indexers ausgeben:

    GET https://[Search service name].search.windows.net/indexers/[indexer name]/status?api-version=[api-version]
    api-key: [Search service admin key]

###<a name="response"></a>Antwort

Sie sehen eine HTTP 200 OK-Antwort zurückgegeben, zusammen mit einer Antwort Textkörper, die Informationen für insgesamt Indizierungsstatus Gesundheit, der letzten Indexer aufrufen sowie den Verlauf der zuletzt verwendete Indexer Aufrufe (falls vorhanden) enthält.

Die Antwort sollte etwa wie folgt aussehen:

    {
        "status":"running",
        "lastResult": {
            "status":"success",
            "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
         },
        "executionHistory":[ {
            "status":"success",
             "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
        }]
    }

Ausführungsverlauf kann bis zu der 50 zuletzt abgeschlossenen Ausführungen, die in umgekehrter chronologische Reihenfolge sortiert sind, (, damit die neueste Ausführung in der Antwort zuerst eintritt) enthalten.

##<a name="a-namenextstepsanext-steps"></a><a name="NextSteps"></a>Nächste Schritte

Herzlichen Glückwunsch! Sie haben gerade gelernt Azure DocumentDB mit Azure Search unter Verwendung des Indexers für DocumentDB integriert werden soll.

 - Erfahren Sie, wie mehr zu Azure DocumentDB, finden Sie unter der [DocumentDB Service-Seite](https://azure.microsoft.com/services/documentdb/).

 - Erfahren Sie, wie Weitere Informationen zu Azure suchen, finden Sie unter der [Dienst Suchseite](https://azure.microsoft.com/services/search/).

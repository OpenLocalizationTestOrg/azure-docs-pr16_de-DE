<properties
pageTitle="Indizierung Azure Table Storage mit Azure suchen"
description="Erfahren Sie, wie Daten aus Azure-Tabellen mit Azure Suche indizieren"
services="search"
documentationCenter=""
authors="chaosrealm"
manager="pablocas"
editor="" />

<tags
ms.service="search"
ms.devlang="rest-api"
ms.workload="search" ms.topic="article"  
ms.tgt_pltfrm="na"
ms.date="08/16/2016"
ms.author="eugenesh" />

# <a name="indexing-azure-table-storage-with-azure-search"></a>Indizierung Azure Table Storage mit Azure suchen

Dieser Artikel beschreibt, wie mit Azure suchen zum Indizieren von Daten in Azure Table Storage gespeichert wird. Der neue Tabelle Azure Suche Indexer können dieses Verfahren, schnell und nahtlose. 

> [AZURE.IMPORTANT] Diese Funktion ist derzeit in der Vorschau. Es ist nur in die REST-API mit Version **2015-02-28-Vorschau** und in der Version 2.0-Vorschau von .NET SDK verfügbar. Bitte denken Sie daran, eine Vorschau APIs dienen zum Testen und bewerten, und nicht in der Herstellung Umgebungen verwendet werden.

## <a name="setting-up-azure-table-indexing"></a>Einrichten von Azure Table Indizierung

Zum Einrichten und Konfigurieren eines Indexers Azure Table, können Sie die REST-API von Azure Suchen erstellen und Verwalten von **Indexer** und **Datenquellen** wie in [Indizierungsvorgänge](https://msdn.microsoft.com/library/azure/dn946891.aspx)beschrieben. Sie können auch [Version 2.0-Vorschau](https://msdn.microsoft.com/library/mt761536%28v=azure.103%29.aspx) von .NET SDK verwenden. In der Zukunft die Unterstützung von Azure-Portal Indizierung Tabelle hinzugefügt werden.

Eine Datenquelle gibt an, welche Daten Sie indizieren, Anmeldeinformationen für den Zugriff auf die Daten und Richtlinien, mit denen Azure-Suche auf effizient identifizieren von Änderungen in den Daten (neu, geändert oder gelöscht Zeilen) auf.

Indexer liest Daten aus einer Datenquelle, und Laden es in einer Zielliste Suchindex.

So richten Sie Tabelle Indizierung

1. Erstellen einer Datenquelle
    - Festlegen der `type` -Parameter`azuretable`
    - In der Verbindungszeichenfolge für Speicher-Konto als übergeben die `credentials.connectionString` Parameter
    - Geben Sie die Tabelle mit den `container.name` Parameter
    - Geben Sie optional eine mithilfe der `container.query` Parameter. Verwenden Sie nach Möglichkeit eines Filters auf PartitionKey zur Optimierung der Systemleistung; Alle anderen Abfrage führt eine vollständige Tabellenscan die Leistung bei großen Tabellen beeinträchtigt werden können.
2. Erstellen Sie einen Suchindex mit dem Schema, das die Spalten in der Tabelle entspricht, die Sie indizieren möchten. 
3. Erstellen des Indexers durch Herstellen einer Verbindung Ihrer Datenquelle an den Suchindex.

### <a name="create-data-source"></a>Erstellen der Datenquelle

    POST https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "table-datasource",
        "type" : "azuretable",
        "credentials" : { "connectionString" : "<my storage connection string>" },
        "container" : { "name" : "my-table", "query" : "PartitionKey eq '123'" }
    }   

Weitere Informationen zur Datenquelle erstellen-API finden Sie unter [Datenquelle erstellen](search-api-indexers-2015-02-28-preview.md#create-data-source).

### <a name="create-index"></a>Index erstellen 

    POST https://[service name].search.windows.net/indexes?api-version=2015-02-28
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-target-index",
        "fields": [
            { "name": "key", "type": "Edm.String", "key": true, "searchable": false },
            { "name": "SomeColumnInMyTable", "type": "Edm.String", "searchable": true }
        ]
    }

Weitere Informationen über die Index-API erstellen finden Sie unter [Create Index](https://msdn.microsoft.com/library/dn798941.aspx)

### <a name="create-indexer"></a>Erstellen von indexer 

Schließlich erstellen Sie, der auf die Datenquelle und den Zielindex verweist Indexer aus. Beispiel:

    POST https://[service name].search.windows.net/indexers?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "table-indexer",
      "dataSourceName" : "table-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" }
    }

Weitere Informationen zu den Indexer-API erstellen checken Sie [Indexer erstellen](search-api-indexers-2015-02-28-preview.md#create-indexer)aus.

Das ist alles schon - Indizierung zufrieden!

## <a name="dealing-with-different-field-names"></a>Umgang mit anderen Feldnamen

Häufig werden sich von den Eigenschaftennamen in der Tabelle die Feldnamen in Ihrer vorhandenen Index. **Feld Zuordnungen** können Sie die Eigenschaftsnamen aus der Tabelle, die die Feldnamen im Suchindex zuordnen. Weitere Informationen zum Feld Zuordnungen finden Sie unter [Suchen Azure Indexer Feld Zuordnungen die Unterschiede zwischen Datenquellen und Indizes durchsuchen zu schließen](search-indexer-field-mappings.md).

## <a name="handling-document-keys"></a>Behandeln von Dokument-Tasten

In Azure suchen identifiziert die Taste Dokument ein Dokument eindeutig. Jeder Suchindex müssen genau ein Feld vom Typ `Edm.String`. Das Feld mit das ist erforderlich für jedes Dokument, das auf den Index hinzugefügt wird (tatsächlich, ist das Feld nur erforderlich).

Da Tabellenzeilen eines zusammengesetzten Schlüssels haben, generiert Azure suchen ein synthetische Feld namens `Key` Dies ist eine Verkettung von Partition Schlüssel und Zeile Schlüsselwerte. Beträgt beispielsweise, wenn eine Zeile PartitionKey enthält `PK1` und RowKey `RK1`, dann `Key` Wert des Felds werden `PK1RK1`. 

> [AZURE.NOTE] Die `Key` Wert enthält Zeichen, die im Dokument Tasten, z. B. Striche ungültig sind. Ungültige Zeichen nachdenken, mithilfe der `base64Encode` [Feld Zuordnung (Funktion)](search-indexer-field-mappings.md#base64EncodeFunction). Wenn Sie dies tun, denken Sie daran, die auch verwenden URL-sichere Base64-Codierung beim übergeben Dokument Tasten in-API z. B. Nachschlagen Anrufe.

## <a name="incremental-indexing-and-deletion-detection"></a>Inkrementell indizieren und Löschvorgang Erkennung
 
Wenn Sie eine Tabelle Indexer einrichten, nach einem Zeitplan ausführen, reindexes Sie nur neue oder aktualisierte Zeilen wie von einer Zeile bestimmt `Timestamp` Wert. Sie müssen keine Änderung Erkennung Richtlinie angeben – inkrementell Indizierung automatisch für Sie aktiviert ist. 

Um anzugeben, dass bestimmte Dokumente aus dem Index entfernt werden müssen, können Sie eine Strategie weiche löschen – verwenden, statt eine Zeile zu löschen, fügen Sie eine Eigenschaft, um anzugeben, dass es gelöscht, und Sie eine weiche Löschvorgang Erkennung Richtlinie auf die Datenquelle richten. Die Richtlinie abgebildet wird angenommen, dass eine Zeile gelöscht wird, wenn es sich um eine Eigenschaft verfügt `IsDeleted` mit dem Wert `"true"`: 

    PUT https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]
    
    {
        "name" : "my-table-datasource",
        "type" : "azuretable",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "table name", "query" : "query" },
        "dataDeletionDetectionPolicy" : { "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy", "softDeleteColumnName" : "IsDeleted", "softDeleteMarkerValue" : "true" }
    }   


## <a name="help-us-make-azure-search-better"></a>Helfen Sie uns Azure-Suche zu verbessern.

Wenn Sie das Feature Serviceanfragen oder mit Vorschlägen zur Verbesserung der haben, wenden Sie sich bitte erreichen Sie uns auf unsere [UserVoice Website](https://feedback.azure.com/forums/263029-azure-search/).
<properties
pageTitle="JSON-Blobs mit Suche Azure Blob Indexer Indizierung"
description="JSON-Blobs mit Suche Azure Blob Indexer Indizierung"
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
ms.date="07/26/2016"
ms.author="eugenesh" />

# <a name="indexing-json-blobs-with-azure-search-blob-indexer"></a>JSON-Blobs mit Suche Azure Blob Indexer Indizierung 

In diesem Artikel wird gezeigt, wie Konfigurieren der Suche Azure Blob Indexer zum Extrahieren strukturierten Inhalt aus Blobs, die JSON enthalten.

## <a name="scenarios"></a>Szenarien

Standardmäßig analysiert [Suche Azure Blob Indexer](search-howto-indexing-azure-blob-storage.md) JSON-Blobs als ein einzelnes Ausschnitt von Text. Sie möchten häufig die Struktur Ihrer JSON-Dokumente beibehalten. Betrachten Sie das JSON-Dokument 

    { 
        "article" : {
            "text" : "A hopefully useful article explaining how to parse JSON blobs",
            "datePublished" : "2016-04-13" 
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

Möglicherweise möchten Sie es in ein Dokument Azure Suche mit "Text", "DatePublished" und "Kategorien" Felder zu analysieren.

Wenn Ihre Blobs ein **Array von JSON-Objekte**enthalten, können Sie jedes Element des Arrays zu Konvertierung in einem separaten Azure suchen Dokument möchten. Betrachten Sie einen Blob mit diesem JSON:  

    [
        { "id" : "1", "text" : "example 1" },
        { "id" : "2", "text" : "example 2" },
        { "id" : "3", "text" : "example 3" }
    ]

Sie können Ihre Azure Suchindex mit 3 separate Dokumente, jede mit Felder "Id" und "Text" auffüllen. 

> [AZURE.IMPORTANT] Diese Funktion ist derzeit in der Vorschau. Es ist nur in die REST-API mit Version **2015-02-28-Vorschau**verfügbar. Bitte denken Sie daran, eine Vorschau APIs dienen zum Testen und bewerten, und nicht in der Herstellung Umgebungen verwendet werden. 

## <a name="setting-up-json-indexing"></a>Einrichten von JSON Indizierung

JSON-Blobs indizieren, legen Sie die `parsingMode` Konfigurationsparameter `json` (um jede Blob als ein einzelnes Dokument indizieren) oder `jsonArray` (Wenn Ihr Blobs JSON-Array enthalten): 

    {
      "name" : "my-json-indexer",
      ... other indexer properties
      "parameters" : { "configuration" : { "parsingMode" : "json" | "jsonArray" } }
    }

Verwenden Sie bei Bedarf **Feld Zuordnungen** , wählen Sie die Eigenschaften des Dokuments JSON Quelle Ziel Suchindex gefüllt wird.  Dies wird im folgenden ausführlich beschrieben. 

> [AZURE.IMPORTANT] Bei Verwendung `json` oder `jsonArray` analysieren Modus, Azure-Suche wird davon ausgegangen, dass alle Blobs in Ihrer Datenquelle JSON ist. Wenn Sie eine Kombination aus JSON und nicht JSON-Blobs in der gleichen Datenquelle unterstützen müssen, informieren Sie uns auf [unserer Website UserVoice](https://feedback.azure.com/forums/263029-azure-search).

## <a name="using-field-mappings-to-build-search-documents"></a>Verwenden Feld Zuordnungen, um Dokumente durchsuchen zu erstellen. 

Azure suchen derzeit können keine zufällige JSON-Dokumente direkt indizieren, da es nur einfache Datentypen, Zeichenfolgen-Arrays und GeoJSON Punkte unterstützt. Allerdings können Sie **Feld Zuordnungen** Teilen Ihres Dokuments JSON wählen und diese in der obersten Ebene der Felder des Dokuments suchen "heben". Weitere Informationen zu Feld Zuordnungen Grundlagen finden Sie unter [Azure-Suche Indexer Feld Pfade die Unterschiede zwischen Datenquellen und Indizes durchsuchen zu schließen](search-indexer-field-mappings.md).

In Kürze wieder auf, um unser Beispiel JSON-Dokument: 

    { 
        "article" : {
            "text" : "A hopefully useful article explaining how to parse JSON blobs",
            "datePublished" : "2016-04-13" 
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

Angenommen, Sie haben einen Suchindex mit den folgenden Feldern: `text` vom Typ Edm.String, `date` vom Typ Edm.DateTimeOffset, und `tags` vom Typ Collection(Edm.String). Verwenden Sie zum Zuordnen der JSON in die gewünschte Form, die folgenden Feld Zuordnungen aus: 

    "fieldMappings" : [ 
        { "sourceFieldName" : "/article/text", "targetFieldName" : "text" },
        { "sourceFieldName" : "/article/datePublished", "targetFieldName" : "date" },
        { "sourceFieldName" : "/article/tags", "targetFieldName" : "tags" }
    ]

Die Quelle Feldnamen im Zuordnungen werden mithilfe der [Zeiger JSON](http://tools.ietf.org/html/rfc6901) -Notation angegeben. Sie beginnen mit einer Schrägstrich auf der Stammebene Ihrer JSON-Dokument verweisen und dann die gewünschte Eigenschaft (auf beliebigen Ebene der Schachtelung) mithilfe von forward Schrägstrich getrennten Pfad Drillinto. 

Sie können auch auf einzelne Arrayelemente verweisen, mithilfe von 0-basierten Index. Um das erste Element des Arrays "Kategorien" aus dem oben genannten Beispiel auszuwählen, verwenden Sie beispielsweise Zuordnung für ein Feld wie folgt:

    { "sourceFieldName" : "/article/tags/0", "targetFieldName" : "firstTag" }

> [AZURE.NOTE] Wenn ein Quellenfeldname in einem Feld Zuordnung Pfad zu einer Eigenschaft, die in JSON nicht vorhanden ist verweist, wird die Zuordnung ohne Fehler übersprungen. Dies geschieht, damit Dokumente mit einem anderen Schema unterstützt werden können (also eine allgemeine Anwendungsfall-). Da es keine Validierung ist, müssen Sie achten Sie Tippfehler in Ihre Feld Zuordnung Spezifikation zu vermeiden. 

Wenn Ihre JSON-Dokumente nur einfache Eigenschaften der obersten Ebene enthalten, möglicherweise Feld Zuordnungen kein überhaupt erforderlich. Beispielsweise, wenn Ihre JSON wie folgt aussieht, werden die Eigenschaften der obersten Ebene "Text", "DatePublished" und "Kategorien" direkt in die entsprechenden Felder in den Suchindex zuordnen: 
 
    { 
       "text" : "A hopefully useful article explaining how to parse JSON blobs",
       "datePublished" : "2016-04-13" 
       "tags" : [ "search", "storage", "howto" ]    
    }

## <a name="indexing-nested-json-arrays"></a>Geschachtelte JSON-Arrays Indizierung

Was geschieht, wenn Sie ein Array von JSON-Objekten, aber die Matrix indizieren möchten ist an einer beliebigen Stelle im Dokument verschachtelt? Sie können auswählen, welche Eigenschaft enthält, die Matrix mithilfe der `documentRoot` Konfigurationseigenschaft. Wenn beispielsweise Ihre Blobs wie folgt aus: 

    { 
        "level1" : {
            "level2" : [
                { "id" : "1", "text" : "Use the documentRoot property" }, 
                { "id" : "2", "text" : "to pluck the array you want to index" },
                { "id" : "3", "text" : "even if it's nested inside the document" }  
            ]
        }
    } 

Verwenden Sie diese Konfiguration zum Indizieren des Arrays in der Eigenschaft "level2" enthalten: 

    {
        "name" : "my-json-array-indexer",
        ... other indexer properties
        "parameters" : { "configuration" : { "parsingMode" : "jsonArray", "documentRoot" : "/level1/level2" } }
    }


## <a name="request-examples"></a>Anfordern von Beispielen

Mit dieser alle zusammen, hier sind die vollständige Fracht Beispiele. 

Datenquelle: 

    POST https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<my storage connection string>" },
        "container" : { "name" : "my-container", "query" : "optional, my-folder" }
    }   

Indexer:

    POST https://[service name].search.windows.net/indexers?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "useJsonParser" : true } }, 
      "fieldMappings" : [ 
        { "sourceFieldName" : "/article/text", "targetFieldName" : "text" },
        { "sourceFieldName" : "/article/datePublished", "targetFieldName" : "date" },
        { "sourceFieldName" : "/article/tags", "targetFieldName" : "tags" }
      ]
    }

## <a name="help-us-make-azure-search-better"></a>Helfen Sie uns Azure-Suche zu verbessern.

Wenn Sie das Feature Serviceanfragen oder mit Vorschlägen zur Verbesserung der haben, wenden Sie sich bitte erreichen Sie uns auf unsere [UserVoice-Website](https://feedback.azure.com/forums/263029-azure-search/).
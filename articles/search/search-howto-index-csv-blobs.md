<properties
pageTitle="CSV-Blobs mit Suche Azure Blob Indexer Indizierung | Microsoft Azure"
description="Informationen Sie zum Indizieren CSV-Blobs mit Azure-Suche"
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
ms.date="07/12/2016"
ms.author="eugenesh" />

# <a name="indexing-csv-blobs-with-azure-search-blob-indexer"></a>CSV-Blobs mit Suche Azure Blob Indexer Indizierung 

Standardmäßig getrennt [Suche Azure Blob Indexer](search-howto-indexing-azure-blob-storage.md) analysiert Blobs Text als ein einzelnes Ausschnitt von Text. Mit Blobs, die CSV-Daten enthält, möchten Sie jedoch häufig jede Zeile in der Blob als ein einzelnes Dokument zu behandeln. Betrachten Sie den folgenden Trennzeichen Text: 

    id, datePublished, tags
    1, 2016-01-12, "azure-search,azure,cloud" 
    2, 2016-07-07, "cloud,mobile" 

Möglicherweise möchten Sie es in 2 Dokumente zu analysieren, die jeder enthält "Id", "DatePublished" und "Kategorien" (Felder).

In diesem Artikel erfahren Sie, wie CSV-Blobs mit einem Suche Azure Blob Indexer analysiert werden. 

> [AZURE.IMPORTANT] Diese Funktion ist derzeit in der Vorschau. Es ist nur in die REST-API mit Version **2015-02-28-Vorschau**verfügbar. Bitte denken Sie daran, eine Vorschau APIs dienen zum Testen und bewerten, und nicht in der Herstellung Umgebungen verwendet werden. 

## <a name="setting-up-csv-indexing"></a>Einrichten der CSV-Indizierung

CSV-Blobs indizieren, erstellen oder Aktualisieren einer Indexer Definition mit der `delimitedText` Modus analysieren:  

    {
      "name" : "my-csv-indexer",
      ... other indexer properties
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "firstLineContainsHeaders" : true } }
    }

Weitere Informationen zu den Indexer-API erstellen checken Sie [Indexer erstellen](search-api-indexers-2015-02-28-preview.md#create-indexer)aus.

`firstLineContainsHeaders`Gibt an, dass die erste Zeile (nicht leer) jedes Blob Kopfzeilen enthält.
Wenn Blobs einer anfänglichen Kopfzeile Verlaufsinformationen, sollte die Überschriften im Indexer Konfiguration angegeben werden: 

    "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } } 

Aktuell wird nur die UTF-8-Codierung unterstützt. Darüber hinaus nur das Komma `','` Zeichen als Trennzeichen unterstützt wird. Wenn Sie Unterstützung für andere Codierungen oder Trennzeichen benötigen, informieren Sie uns auf [unserer Website UserVoice](https://feedback.azure.com/forums/263029-azure-search).

> [AZURE.IMPORTANT] Wenn Sie den Analyse Modus durch Trennzeichen getrennte Text verwenden, setzt voraus Azure Suche auf alle Blobs in Ihrer Datenquelle CSV werden können, die aus. Wenn Sie eine Kombination aus CSV und nicht in der CSV-Blobs in der gleichen Datenquelle unterstützen müssen, informieren Sie uns auf [unserer Website UserVoice](https://feedback.azure.com/forums/263029-azure-search).

## <a name="request-examples"></a>Anfordern von Beispielen

Mit dieser alle nachfolgend zusammen, die komplette Nutzlast Beispiele. 

Datenquelle: 

    POST https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<my storage connection string>" },
        "container" : { "name" : "my-container", "query" : "<optional, my-folder>" }
    }   

Indexer:

    POST https://[service name].search.windows.net/indexers?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-csv-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } }
    }

## <a name="help-us-make-azure-search-better"></a>Helfen Sie uns Azure-Suche zu verbessern.

Wenn Sie das Feature Serviceanfragen oder mit Vorschlägen zur Verbesserung der haben, wenden Sie sich bitte erreichen Sie uns auf unsere [UserVoice-Website](https://feedback.azure.com/forums/263029-azure-search/).
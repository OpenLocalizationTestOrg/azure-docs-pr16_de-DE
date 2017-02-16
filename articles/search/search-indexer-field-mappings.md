<properties
pageTitle="Feld Zuordnungen in Azure Suche Indizes"
description="Konfigurieren der Suche Azure Indexer Feld Zuordnungen Unterschiede in Feldnamen und Darstellung von Daten"
services="search"
documentationCenter=""
authors="chaosrealm"
manager="pablocas"
editor="" />

<tags
ms.service="search"
ms.devlang="rest-api"
ms.workload="search" 
ms.topic="article"  
ms.tgt_pltfrm="na"
ms.date="10/17/2016"
ms.author="eugenesh" />

# <a name="field-mappings-in-azure-search-indexers"></a>Feld Zuordnungen in Azure Suche Indizes

Bei der Verwendung von Azure Suche Indexer finden gelegentlich selbst Sie in Situationen, in dem die Eingabedaten ganz das Schema der der Zielindex übereinstimmt. In diesen Fällen können Sie **Feld Zuordnungen** zum Umwandeln der Daten in die gewünschte Form. 

Einige Situationen, in dem Feld Zuordnungen hilfreich sind:
 
- Die Datenquelle enthält ein Feld `_id`, aber Azure Suche gestattet keine Feldnamen, die mit einem Unterstrich beginnen. Zuordnung für ein Feld können Sie ein Feld "Umbenennen". 
- Möchten Sie mehrere Felder in den Index mit den gleichen Daten Quelldaten, beispielsweise auffüllen, da Sie verschiedene Analyzern an diesen Feldern anwenden möchten. Feld Zuordnungen ermöglichen Ihnen die Quelle Datenfeld "Verzweigung".
- Sie müssen mit Base64 codieren oder Entschlüsseln von Daten. Feld Zuordnungen unterstützt mehrere **Zuordnungsfunktionen**, einschließlich Funktionen für Base64 Codierung und Decodierung.   


> [AZURE.IMPORTANT] Ist derzeit Feld Zuordnungen Funktionalität in der Vorschau an. Es ist nur in die REST-API mit Version **2015-02-28-Vorschau**verfügbar. Bitte denken Sie daran, eine Vorschau APIs dienen zum Testen und bewerten, und nicht in der Herstellung Umgebungen verwendet werden.

## <a name="setting-up-field-mappings"></a>Einrichten von Feld Zuordnungen

Beim Erstellen eines neuen Indexers [Erstellen Indexer](search-api-indexers-2015-02-28-preview.md#create-indexer) API verwenden, können Sie Feld Zuordnungen hinzufügen. Sie können die Verwendung des [Update Indexer](search-api-indexers-2015-02-28-preview.md#update-indexer) API Indizierung Indexer Feld Zuordnungen zu verwalten. 

Zuordnung für ein Feld besteht aus 3 Teilen: 

1. A `sourceFieldName`, der ein Feld in der Datenquelle darstellt. Diese Eigenschaft ist erforderlich. 

2. Eine optionale `targetFieldName`, der ein Feld in der Suchindex darstellt. Wenn nicht angegeben, wird der denselben Namen wie in der Datenquelle verwendet. 

3. Eine optionale `mappingFunction`, die die Daten mit einer mehrere transformieren können vordefinierte Funktionen. Die vollständige Liste der Funktionen ist [unter](#mappingFunctions).

Felder Zuordnungen hinzugefügt werden die `fieldMappings` Array über die Definition der Indexer. 

Hier beträgt beispielsweise wie Unterschiede in Feldnamen aufnehmen werden kann: 

```JSON

PUT https://[service name].search.windows.net/indexers/myindexer?api-version=[api-version]
Content-Type: application/json
api-key: [admin key]
{
    "dataSourceName" : "mydatasource",
    "targetIndexName" : "myindex",
    "fieldMappings" : [ { "sourceFieldName" : "_id", "targetFieldName" : "id" } ] 
} 
```

Indexer kann mehrere Feld Zuordnungen haben. Angenommen, so wie Sie "ein Felds verzweigen können":

```JSON

"fieldMappings" : [ 
    { "sourceFieldName" : "text", "targetFieldName" : "textStandardEnglishAnalyzer" },
    { "sourceFieldName" : "text", "targetFieldName" : "textSoundexAnalyzer" }, 
] 
```

> [AZURE.NOTE] Azure Suche verwendet und Kleinschreibung, um das Feld oder der Funktion Feld Zuordnungen aufgelöst. Dies ist die geeignete (Sie müssen nicht die Groß-/Kleinschreibung Rechte erhalten), aber dies bedeutet, dass die Datenquelle oder Index Felder haben kann, die nur durch Fall unterscheiden.  

<a name="mappingFunctions"></a>
## <a name="field-mapping-functions"></a>Feld Zuordnungsfunktionen

Diese Funktionen werden derzeit unterstützt: 

- [base64Encode](#base64EncodeFunction)
- [base64Decode](#base64DecodeFunction)
- [extractTokenAtPosition](#extractTokenAtPositionFunction)
- [jsonArrayToStringCollection](#jsonArrayToStringCollectionFunction)

<a name="base64EncodeFunction"></a>
### <a name="base64encode"></a>base64Encode 

Führt eine *URL-sichere* Base64-Codierung für die eingegebene Zeichenfolge. Wird davon ausgegangen, dass die Eingabe UTF-8-codierte ist. 

#### <a name="sample-use-case"></a>Beispiel für Anwendungsfall- 

Nur die URL-sichere Zeichen können in ein Suchfeld Azure Dokument Key angezeigt (da Kunden können Sie das Dokument mit den Nachschlage-API, beispielsweise behoben werden müssen). Wenn Ihre URL unsicheren Zeichen enthält, und Sie es verwenden, um ein Feld im Suchindex füllen möchten, verwenden Sie diese Funktion.   

#### <a name="example"></a>Beispiel 

```JSON

"fieldMappings" : [ 
  { 
    "sourceFieldName" : "Path", 
    "targetFieldName" : "UrlSafePath",
    "mappingFunction" : { "name" : "base64Encode" } 
  }] 
```

<a name="base64DecodeFunction"></a>
### <a name="base64decode"></a>base64Decode

Führt Base64 Umwandlung, eines die eingegebene Zeichenfolge. Die Eingabe wird in einen *URL-sichere* Base64-codierte Zeichenfolge angenommen. 

#### <a name="sample-use-case"></a>Beispiel für Anwendungsfall- 

Benutzerdefinierte Metadatenwerte BLOB müssen ASCII-codierte. Sie können Base64-Codierung beliebige Unicode-Zeichenfolgen in den benutzerdefinierten Blob-Metadaten darstellen. Jedoch um Suche sinnvoll zu gestalten, können dieser Funktion Sie die codierten Daten wieder in "reguläre" Zeichenfolgen umgewandelt werden, wenn Ihre Suchindex Auffüllen von.  

#### <a name="example"></a>Beispiel 

```JSON

"fieldMappings" : [ 
  { 
    "sourceFieldName" : "Base64EncodedMetadata", 
    "targetFieldName" : "SearchableMetadata",
    "mappingFunction" : { "name" : "base64Decode" } 
  }] 
```

<a name="extractTokenAtPositionFunction"></a>
### <a name="extracttokenatposition"></a>extractTokenAtPosition

Teilt eine Zeichenfolgenfeld mit der angegebenen Trennzeichen und wählt das Token an der angegebenen Position resultierende teilen.

Beispielsweise ist die Eingabe `Jane Doe`, die `delimiter` ist `" "`(Leerzeichen) und der `position` 0 ist, ist das Ergebnis `Jane`; Wenn die `position` 1, ist das Ergebnis ist `Doe`. Wenn die Position auf ein Token, die nicht vorhanden ist verweist, wird ein Fehler zurückgegeben.

#### <a name="sample-use-case"></a>Beispiel für Anwendungsfall- 

Ihre Datenquelle enthält eine `PersonName` Feld, und Sie möchten es als zwei Separate indizieren `FirstName` und `LastName` Felder. Diese Funktion können Sie die Eingabe mit Leerzeichen als Trennzeichen teilen.

#### <a name="parameters"></a>Parameter

- `delimiter`: eine Zeichenfolge, die als Trennzeichen verwendet werden soll, wenn die eingegebene Zeichenfolge zu teilen.
- `position`: eine ganze Zahl nullbasierte Position das Token auswählen, nachdem die eingegebene Zeichenfolge aufgeteilt ist.    

#### <a name="example"></a>Beispiel

```JSON 

"fieldMappings" : [ 
  { 
    "sourceFieldName" : "PersonName", 
    "targetFieldName" : "FirstName",
    "mappingFunction" : { "name" : "extractTokenAtPosition", "parameters" : { "delimiter" : " ", "position" : 0 } } 
  }, 
  { 
    "sourceFieldName" : "PersonName", 
    "targetFieldName" : "LastName",
    "mappingFunction" : { "name" : "extractTokenAtPosition", "parameters" : { "delimiter" : " ", "position" : 1 } } 
  }] 
```

<a name="jsonArrayToStringCollectionFunction"></a>
### <a name="jsonarraytostringcollection"></a>jsonArrayToStringCollection

Wandelt eine Zeichenfolge formatiert als JSON-Array von Zeichenfolgen in einer Zeichenfolge Matrix zurück, die zum Füllen verwendet werden kann ein `Collection(Edm.String)` im Index-Feld. 

Wenn die eingegebene Zeichenfolge wird beispielsweise `["red", "white", "blue"]`, klicken Sie dann auf das Zielfeld vom Typ `Collection(Edm.String)` wird mit den drei Werten aufgefüllt `red`, `white` und `blue`. Für die Eingabewerte, die als JSON-Zeichenfolgenarrays analysiert werden können, wird ein Fehler zurückgegeben. 

#### <a name="sample-use-case"></a>Beispiel für Anwendungsfall-

Azure SQL-Datenbank verfügt nicht über einen integrierten Datentyp, die die zuordnet `Collection(Edm.String)` Felder in Azure suchen. Zeichenfolge Websitesammlung Felder ausgefüllt, formatieren die Quelldaten als Matrix JSON Zeichenfolge zurück, und verwenden Sie diese Funktion. 

#### <a name="example"></a>Beispiel 

```JSON

"fieldMappings" : [ 
  { "sourceFieldName" : "tags", "mappingFunction" : { "name" : "jsonArrayToStringCollection" } }
] 
```

## <a name="help-us-make-azure-search-better"></a>Helfen Sie uns Azure-Suche zu verbessern.

Wenn Sie das Feature Serviceanfragen oder mit Vorschlägen zur Verbesserung der haben, wenden Sie sich bitte erreichen Sie uns auf unsere [UserVoice Website](https://feedback.azure.com/forums/263029-azure-search/).
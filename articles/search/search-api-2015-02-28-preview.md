<properties
   pageTitle="Azure Service REST API-Version 2015-02-28-Vorschau für Suche | Microsoft Azure | Vorschau der Azure Suche API"
   description="Azure Service REST API-Version 2015-02-28-Vorschau für Suche enthält Versuche Features wie natürlicher Sprache Analyzern und MoreLikeThis suchen."
   services="search"
   documentationCenter="na"
   authors="brjohnstmsft"
   manager="pablocas"
   editor=""/>

<tags
   ms.service="search"
   ms.devlang="rest-api"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="search"
   ms.date="09/07/2016"
   ms.author="brjohnst"/>

# <a name="azure-search-service-rest-api-version-2015-02-28-preview"></a>Suche Azure Service REST-API: Version 2015-02-28-Vorschau

In diesem Artikel wird die Dokumentation zur für `api-version=2015-02-28-Preview`. Dieser Vorschau erweitert die aktuelle Version in der Regel verfügbare, [api-Version 2015-02-28 =](https://msdn.microsoft.com/library/dn798935.aspx), indem Sie die folgenden Versuche Features:

- `moreLikeThis`Abfrage Parameter in die [Dokumente durchsuchen](#SearchDocs) API. Es findet andere Dokumente, die in ein anderes bestimmten Dokument relevant sind.

Ein paar zusätzliche Teile der `2015-02-28-Preview` REST-API dokumentierten. Hierzu gehören:

- [Bewerten von Benutzerprofilen](search-api-scoring-profiles-2015-02-28-preview.md)
- [Indexer](search-api-indexers-2015-02-28-preview.md)

Azure Suchdienst ist in mehreren Versionen zur Verfügung. Einzelheiten finden Sie unter [Suchen Dienst Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) .

## <a name="apis-in-this-document"></a>In diesem Dokument APIs

Azure Suche Service-API unterstützt zwei URL-Syntax für Vorgänge API: einfache und OData (Details finden Sie unter [Unterstützung für OData (Azure Suche API)](http://msdn.microsoft.com/library/azure/dn798932.aspx) ). Die folgende Liste zeigt die einfache Syntax.

[Index erstellen](#CreateIndex)

    POST /indexes?api-version=2015-02-28-Preview

[Index aktualisieren](#UpdateIndex)

    PUT /indexes/[index name]?api-version=2015-02-28-Preview

[Abrufen von Index](#GetIndex)

    GET /indexes/[index name]?api-version=2015-02-28-Preview

[Auflisten von Indizes](#ListIndexes)

    GET /indexes?api-version=2015-02-28-Preview

[Indexstatistik abrufen](#GetIndexStats)

    GET /indexes/[index name]/stats?api-version=2015-02-28-Preview

[Analyzers](#TestAnalyzer)

    POST /indexes/[index name]/analyze?api-version=2015-02-28-Preview

[Löschen eines Indexes](#DeleteIndex)

    DELETE /indexes/[index name]?api-version=2015-02-28-Preview

[Hinzufügen, löschen und Aktualisieren von Daten innerhalb eines Indexes](#AddOrUpdateDocuments)

    POST /indexes/[index name]/docs/index?api-version=2015-02-28-Preview

[Suchen von Dokumenten](#SearchDocs)

    GET /indexes/[index name]/docs?[query parameters]
    POST /indexes/[index name]/docs/search?api-version=2015-02-28-Preview

[Nachschlage-Dokument](#LookupAPI)

     GET /indexes/[index name]/docs/[key]?[query parameters]

[Anzahl von Dokumenten](#CountDocs)

    GET /indexes/[index name]/docs/$count?api-version=2015-02-28-Preview

[Vorschläge](#Suggestions)

    GET /indexes/[index name]/docs/suggest?[query parameters]
    POST /indexes/[index name]/docs/suggest?api-version=2015-02-28-Preview

________________________________________
<a name="IndexOps"></a>
## <a name="index-operations"></a>Index-Vorgänge

Sie können erstellen und Verwalten von Indizes in Azure Suchdienst über einfachen HTTP (bereitstellen, abrufen, sich löschen) gegen eine Ressource angegebenen Index. Um einen Index zu erstellen, Buchen Sie zuerst eine JSON-Dokument, das das IndexSchema beschreibt. Das Schema definiert die Felder für den Index, deren Datentypen und wie sie (beispielsweise in Suchvorgänge, filtern, sortieren oder Faceting) verwendet werden können. Darüber hinaus definiert Punktzahl Profile, Suggesters und andere Attribute, um das Verhalten des Indexes zu konfigurieren.

Im folgende Beispiel stellt eine Abbildung der ein Schema für die Suche auf Hotelinformationen mit dem Feld Beschreibung in zwei Sprachen definiert verwendet. Beachten Sie, wie Attribute steuern, wie das Feld verwendet wird. Beispielsweise die `hotelId` als die Taste Dokument verwendet wird (`"key": true`) und wird von Suchvorgänge ausgeschlossen (`"searchable": false`).

    {
    "name": "hotels",  
    "fields": [
      {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false},
      {"name": "baseRate", "type": "Edm.Double"},
      {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
      {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
      {"name": "hotelName", "type": "Edm.String"},
      {"name": "category", "type": "Edm.String"},
      {"name": "tags", "type": "Collection(Edm.String)"},
      {"name": "parkingIncluded", "type": "Edm.Boolean"},
      {"name": "smokingAllowed", "type": "Edm.Boolean"},
      {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
      {"name": "rating", "type": "Edm.Int32"},
      {"name": "location", "type": "Edm.GeographyPoint"}
     ],
     "suggesters": [
      {
       "name": "sg",
       "searchMode": "analyzingInfixMatching",
       "sourceFields": ["hotelName"]
      }
     ]
    }

Nachdem der Index erstellt wurde, werden Sie Dokumente hochladen, die den Index auffüllen. Dieses nächsten Schritts finden Sie unter [Hinzufügen oder Aktualisieren von Dokumenten](#AddOrUpdateDocuments) .

Ein Einführungsvideo zum Indizierung in Azure suchen finden Sie unter der [Channel 9 Cloud Deckblatt Folge nach der Azure](http://go.microsoft.com/fwlink/p/?LinkId=511509).


<a name="CreateIndex"></a>
## <a name="create-index"></a>Index erstellen

Ein Index ist die primären Option für das Organisieren und Suchen von Dokumenten in Azure suchen, ähnlich wie eine Tabelle mit Datensätzen in einer Datenbank organisiert. Jeder Index hat eine Sammlung von Dokumenten, dass alle IndexSchema (Feldnamen, Datentypen und Eigenschaften) entsprechen, aber Indizes auch zusätzliche Konstrukte (Suggesters, Punktzahl Profile und CORS Optionen), die andere Verhaltensweisen Suche definieren angeben.

Sie können in einem Azure-Suchdienst mithilfe einer Anforderung HTTP POST oder sich einen neuen Index erstellen. Hauptteil der Anforderung ist ein JSON-Schema, der die Index und Konfiguration Informationen angibt.

    POST https://[service name].search.windows.net/indexes?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

Alternativ können Sie sich verwenden und geben Sie den Namen des Indexes auf den URI an. Wenn der Index nicht vorhanden ist, wird er erstellt.

    PUT https://[search service url]/indexes/[index name]?api-version=[api-version]

Erstellen eines Indexes bestimmt die Struktur der Dokumente gespeichert und in Suchvorgängen verwendet. Füllen den Index ist einem separaten Vorgang. Für diesen Schritt können Sie einen [Indexer](https://msdn.microsoft.com/library/azure/mt183328.aspx) (für unterstützte Datenquellen verfügbar) oder einen Vorgang [Hinzufügen, aktualisieren oder Löschen von Dokumenten](https://msdn.microsoft.com/library/azure/dn798930.aspx) . Invertierte Index wird ausgelöst, wenn die Dokumente veröffentlicht werden.

**Hinweis**: die maximale Anzahl von Indizes zulässig hängt vom Preise Ebene ab. Kostenlose Dienst können bis zu 3 Indizes. Standard-Dienst kann 50 Indizes pro Suchdienst. Weitere Informationen finden Sie unter [Grenzwerte und Einschränkungen](http://msdn.microsoft.com/library/azure/dn798934.aspx) .

**Anfordern**

HTTPS ist für alle Serviceanfragen erforderlich. Die Anfrage **Create Index** kann mithilfe entweder einen Beitrag, oder sich Methode erstellt werden. Wenn Sie POST verwenden zu können, müssen Sie einen Indexnamen in den Hauptteil der Anforderung zusammen mit den Index Schemadefinition bereitstellen. Mit sich ist der Name des Indexes Teil der URL. Wenn Sie der Index nicht vorhanden ist, wird er erstellt. Wenn sie bereits vorhanden ist, wird es an die neue Definition aktualisiert.

Den Namen des Indexes muss in Kleinbuchstaben angegeben werden, beginnen mit einem Buchstaben oder eine Zahl, haben keine Bindestriche oder Punkte und weniger als 128 Zeichen lang sein. Nach dem Starten den Namen des Indexes mit einem Buchstaben oder eine Zahl, kann die restlichen den Namen alle Buchstaben, Zahl und Striche, einbeziehen, solange die Striche nicht aufeinander folgen.

Die `api-version` ist erforderlich. Eine Liste der verfügbaren Versionen finden Sie unter [Suchen Dienst Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) .

**Anfordern von Kopfzeilen**

Die folgende Liste beschreibt die erforderlichen und optionalen Anforderung Überschriften.

- `Content-Type`: Erforderlich ist. Legen Sie den Wert`application/json`
- `api-key`: Erforderlich ist. Die `api-key` wird mit
- authentifizieren Sie die Anfrage Suchdienst an. Es ist ein Zeichenfolgenwert mit dem Dienst. Die Anfrage **Index erstellen** darf enthalten eine `api-key` Kopfzeile mit Ihrem Administrator Key (im Gegensatz zu einer Abfrageschlüssel) festlegen.

Sie benötigen außerdem den Dienstnamen, um die Anfrage-URL zu erstellen. Können Sie sowohl den Dienstnamen erhalten und `api-key` aus Ihrem Dienst Dashboard im Portal Azure. Finden Sie unter [Erstellen einer Azure Suchdienst im Portal](search-create-service-portal.md) für die Seitennavigation helfen.

<a name="RequestData"></a>
**Die Syntax der Textkörper anfordern**

Hauptteil der Anforderung enthält eine Schemadefinition, wozu auch die Liste der Datenfelder innerhalb von Dokumenten, die in diesen Index, Datentypen, Attribute sowie eine optionale Liste von Profilen, mit denen übereinstimmenden Dokumente gleichzeitig Abfrage Punktzahl bewerten eingezogen werden sollen.

Beachten Sie, dass für die Anforderung einer Beitrag, den Namen des Indexes im Hauptteil Anforderung angegeben werden muss.

Es kann nur ein Key-Feld im Index sein. Es muss ein Zeichenfolgenfeld sein. Dieses Feld steht für den eindeutigen Bezeichner für jedes Dokument innerhalb des Indexes gespeichert.

Die wichtigsten Teile eines Indexes gehören die folgenden:

- `name`
- `fields`die wird "Herd" in diesen Index, einschließlich Name, Datentyp und Eigenschaften, die für das Feld zulässige Aktionen definieren.
- `suggesters`für das AutoVervollständigen- oder während der Eingabe Abfragen verwendet.
- `scoringProfiles`für benutzerdefinierte Suche Punktzahl Rangfolgen verwendet. Details finden Sie unter [Hinzufügen Punktzahl Profile](https://msdn.microsoft.com/library/azure/dn798928.aspx) .
- `analyzers`, `charFilters`, `tokenizers`, `tokenFilters` verwendet, um zu definieren, wie Ihre Dokumente/Abfragen in indiziert/durchsuchbare Token aufgeteilt werden. Details finden Sie unter [Analyse in Azure suchen](https://aka.ms//azsanalysis) .
- `defaultScoringProfile`verwendet, um die Standardeinstellung bewerten Verhalten zu überschreiben.
- `corsOptions`Um Cross Origin Abfragen für Ihre Index zu ermöglichen.

Die Syntax für die Strukturierung der Anforderungsnutzlast lautet wie folgt aus. Eine Beispiel für eine Anforderung weiter unten in diesem Artikel bereitgestellt wird.

    {
      "name": (optional on PUT; required on POST) "name_of_index",
      "fields": [
        {
          "name": "name_of_field",
          "type": "Edm.String | Collection(Edm.String) | Edm.Int32 | Edm.Int64 | Edm.Double | Edm.Boolean | Edm.DateTimeOffset | Edm.GeographyPoint",
          "searchable": true (default where applicable) | false (only Edm.String and Collection(Edm.String) fields can be searchable),
          "filterable": true (default) | false,
          "sortable": true (default where applicable) | false (Collection(Edm.String) fields cannot be sortable),
          "facetable": true (default where applicable) | false (Edm.GeographyPoint fields cannot be facetable),
          "key": true | false (default, only Edm.String fields can be keys),
          "retrievable": true (default) | false,              
          "analyzer": "name of the analyzer used for search and indexing", (only if 'searchAnalyzer' and 'indexAnalyzer' are not set)
          "searchAnalyzer": "name of the search analyzer", (only if 'indexAnalyzer' is set and 'analyzer' is not set)
          "indexAnalyzer": "name of the indexing analyzer" (only if 'searchAnalyzer' is set and 'analyzer' is not set)
        }
      ],
      "suggesters": [
        {
          "name": "name of suggester",
          "searchMode": "analyzingInfixMatching" (other modes may be added in the future),
          "sourceFields": ["field1", "field2", ...]
        }
      ],
      "scoringProfiles": [
        {
          "name": "name of scoring profile",
          "text": (optional, only applies to searchable fields) {
            "weights": {
              "searchable_field_name": relative_weight_value (positive numbers),
              ...
            }
          },
          "functions": (optional) [
            {
              "type": "magnitude | freshness | distance | tag",
              "boost": # (positive number used as multiplier for raw score != 1),
              "fieldName": "...",
              "interpolation": "constant | linear (default) | quadratic | logarithmic",
              "magnitude": {
                "boostingRangeStart": #,
                "boostingRangeEnd": #,
                "constantBoostBeyondRange": true | false (default)
              },
              "freshness": {
                "boostingDuration": "..." (value representing timespan leading to now over which boosting occurs)
              },
              "distance": {
                "referencePointParameter": "...", (parameter to be passed in queries to use as reference location, see "scoringParameter" for syntax details)
                "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends)
              },
              "tag": {
                "tagsParameter": "..." (parameter to be passed in queries to specify list of tags to compare against target field, see "scoringParameter" for syntax details)
              }
            }
          ],
          "functionAggregation": (optional, applies only when functions are specified)
            "sum (default) | average | minimum | maximum | firstMatching"
        }
      ],
      "analyzers":(optional)[ ... ],
      "charFilters":(optional)[ ... ],
      "tokenizers":(optional)[ ... ],
      "tokenFilters":(optional)[ ... ],
      "defaultScoringProfile": (optional) "...",
      "corsOptions": (optional) {
        "allowedOrigins": ["*"] | ["origin_1", "origin_2", ...],
        "maxAgeInSeconds": (optional) max_age_in_seconds (non-negative integer)
      }
    }

**Indexattributen**

Beim Erstellen eines Indexes, können die folgenden Attribute festgelegt werden. Details zu Punktzahl und bewerten Profilen finden Sie unter [Punktzahl Profile hinzufügen](https://msdn.microsoft.com/library/azure/dn798928.aspx).

`name`-Legt den Namen des Felds.

`type`-Legt den Datentyp für das Feld an. Eine Liste der unterstützten Datentypen finden Sie unter [Unterstützte Datentypen](#DataTypes) .

`searchable`– Markiert das Feld als vollständigen Text suchen können. Dies bedeutet, dass es Analyse, wie Word neuesten während der Indizierung durchläuft. Wenn Sie festlegen einer `searchable` Feld mit einem Wert wie "Sonnenschein Day", intern es wird in den einzelnen Token "Sonnenschein" und "Tag" geteilt werden. Dadurch Suchvorgänge für diesen Ausdrücken. Die Felder vom Typ `Edm.String` oder `Collection(Edm.String)` sind `searchable` standardmäßig. Felder, die von anderen Typen kann nicht `searchable`.

  - **Hinweis**: `searchable` Felder zusätzliche Leerzeichen in Ihrem Index nutzen, da Azure suchen eine zusätzliche Version der Feldwert für Suchvorgänge mit Token gespeichert wird. Wenn Leerzeichen in Ihrem Index gespeichert werden sollen, und Sie nicht erforderlich, dass ein Feld in Suchvorgänge einbezogen werden, legen Sie `searchable` auf `false`.

`filterable`– Ermöglicht das Feld in Bezug `$filter` Abfragen. `filterable`unterscheidet sich von der `searchable` in wie Zeichenfolgen verarbeitet werden. Die Felder vom Typ `Edm.String` oder `Collection(Edm.String)` , die `filterable` Word neuesten nicht werden, damit Vergleiche sind für nur genauen entspricht. Beispielsweise, wenn Sie festlegen, dass ein entsprechendes Feld `f` 'Sonnenschein Day' `$filter=f eq 'sunny'` findet keine Übereinstimmungen hervorgehoben, jedoch `$filter=f eq 'sunny day'` wird. Alle Felder sind `filterable` standardmäßig.

`sortable`-Standardmäßig sortiert das System Ergebnisse nach Faktor, aber in vielen Erfahrung Benutzer werden durch die Felder in der Dokumente sortieren möchten. Die Felder vom Typ `Collection(Edm.String)` kann nicht `sortable`. Alle anderen Felder sind `sortable` standardmäßig.

`facetable`In einer Präsentation von Suchergebnissen, die Anzahl der Zugriffe nach Kategorie (zum Beispiel, Suche nach digitalen Kameras und finden Sie unter Treffer nach Marke, Megapixel, Preis usw.) enthält, in der Regel verwendet. Diese Option kann nicht verwendet werden, mit der Felder vom Typ `Edm.GeographyPoint`. Alle anderen Felder sind `facetable` standardmäßig.

  - **Hinweis**: die Felder vom Typ `Edm.String` , die `filterable`, `sortable`, oder `facetable` können höchstens 32 KB sein lang. Dies ist, da solche Felder werden als einen einzelnen Suchbegriff behandelt, und die maximale Länge der eines Ausdrucks in Azure Suche 32KB beträgt. Wenn Sie mehr Text als dies in einem einzigen Zeichenfolgenfeld speichern müssen, müssen Sie explizit festgelegt `filterable`, `sortable`, und `facetable` zu `false` in die Definition des Indexes.

  - **Hinweis**: Wenn Sie ein Feld weist keine der oben genannten Attribute legen Sie auf `true` (`searchable`, `filterable`, `sortable`, oder`facetable`) das Feld ist effektiv aus dem invertierte Index ausgeschlossen. Diese Option eignet sich für Felder, die nicht in Abfragen verwendet werden, aber in den Suchergebnissen erforderlich sind. Verbessert die Leistung solche Felder aus dem Index ausgeschlossen.

`key`-Feld als, eindeutigen Bezeichner für Dokumente in den Index markiert. Genau ein Feld muss als ausgewählt werden die `key` Feld, und er muss vom Typ `Edm.String`. Schlüsselfelder können Dokumente direkt über den [Nachschlage-API](#LookupAPI)Nachschlagen verwendet werden.

`retrievable`-Legt fest, ob das Feld in einem Suchergebnis zurückgegeben werden kann.  Dies ist nützlich, wenn Sie ein Feld (z. B. Rand) verwenden als Filter, Sortierung oder bewerten Verfahren möchten, aber nicht, das Feld für den Endbenutzer sichtbar sein soll möchten. Dieses Attribut muss `true` für `key` Felder.

`analyzer`-Legt den Namen für die Analyse, um für das Feld Suchen Zeitpunkt und Zeit zum Indizieren verwenden. Der zulässige Satz von Werten finden Sie unter [Analyzern](https://msdn.microsoft.com/library/mt605304.aspx). Diese Option kann nur mit verwendet werden `searchable` Felder und es können nicht zusammen mit entweder festgelegt werden `searchAnalyzer` oder `indexAnalyzer`.  Nachdem Sie der Analyzer ausgewählt ist, kann es für das Feld nicht geändert werden.

`searchAnalyzer`-Legt den Namen des Analyzers gleichzeitig suchen für das Feld verwendet. Der zulässige Satz von Werten finden Sie unter [Analyzern](https://msdn.microsoft.com/library/mt605304.aspx). Diese Option kann nur mit verwendet werden `searchable` Felder. Sie müssen festgelegt werden, zusammen mit `indexAnalyzer` und es kann nicht festgelegt werden, zusammen mit der `analyzer` Option. Diese Analyse kann auf ein bestehendes Feld aktualisiert werden.

`indexAnalyzer`-Legt den Namen des Analyzers Indizierung Zeitpunkt für das Feld verwendet. Der zulässige Satz von Werten finden Sie unter [Analyzern](https://msdn.microsoft.com/library/mt605304.aspx). Diese Option kann nur mit verwendet werden `searchable` Felder. Sie müssen festgelegt werden, zusammen mit `searchAnalyzer` und es kann nicht festgelegt werden, zusammen mit der `analyzer` Option. Nachdem Sie der Analyzer ausgewählt ist, kann es für das Feld nicht geändert werden.

`suggesters`-Suchmodus und Felder, die die Quelle des Inhalts für Vorschläge sind festgelegt. Details finden Sie unter [Suggesters](#Suggesters) .

`scoringProfiles`-Definiert benutzerdefinierte Punktzahl Verhaltensweisen, mit die Sie beeinflussen können welche Elemente höhere in Suchergebnissen angezeigt werden. Punktzahl Profile bestehen aus Feld-Stärken und Funktionen. Weitere Informationen zu den Attributen, die in einem Punktzahl Profil verwendeten finden Sie unter [bewerten Profile hinzufügen](https://msdn.microsoft.com/library/azure/dn798928.aspx) .

<!-- This is a standalone topic in MSDN -->
<a name="LanguageSupport"></a>
**Unterstützung für Sprachen**

Durchsuchbare Felder unterliegen Analyse, dass der größte umfasst häufig Word neuesten, Normalisierung Text und das Ausfiltern von Ausdrücken. Standardmäßig werden durchsuchbare Felder in Azure-Suche mit dem [Apache Lucene Standard Analyzer](http://lucene.apache.org/core/4_9_0/analyzers-common/index.html) analysiert die Umbrüche Text in Elemente, die die Regeln["Unicode Text Segmentierung"](http://unicode.org/reports/tr29/) folgen. Darüber hinaus wandelt der standard-Analyzer alle folgenden Zeichen in ihre Kleinbuchstaben Form. Sowohl indizierten Dokumente und Suchbegriffe gehen Sie beim Indizieren und abfrageverarbeitung durch die Analyse aus.

Azure-Suche unterstützt verschiedene Sprachen. Jede Sprache erfordert eine nicht standardmäßige Text Analyse der Eigenschaften für eine bestimmte Sprache ausmacht. Azure-Suche bietet zwei Arten von Analyzern:

- 35 Analyzern durch Lucene gesichert.
- 50 Analyzern durch eigene Microsoft natürlicher Sprache in Office und Bing verwendeten Technologie Verarbeitung gesichert.

Einige Entwickler möglicherweise die Lösung mehr vertraute, einfache, öffnen-Quelle der Lucene bevorzugen. Lucene Analyzern sind schneller, aber die Microsoft Analyzern haben erweiterte Funktionen, wie z. B. Lemmatisierung, Word decompounding (in Sprachen wie Deutsch, Dänisch, Niederländisch, Schwedisch, Norwegisch, Estnisch, Fertig stellen, Ungarisch, Slowakisch) und Entität Spracherkennung (URLs, e-Mails, Datumsangaben, Zahlen). Falls möglich, sollten Sie ausführen, Vergleiche von der Microsoft- und Lucene Analyzern entscheiden, welche eine einer besser geeignet ist.

***Vergleich***

Der Lucene Analyzer für Englisch erweitert den standardmäßigen Analyzer. Es entfernt Possessivformen (nachgestellte des) von Wörtern, gilt gemäß [Porter Wortstamm Algorithmus](http://tartarus.org/~martin/PorterStemmer/)Wortstamm und englischen [Wörtern beenden](http://en.wikipedia.org/wiki/Stop_words)entfernt.

Im Vergleich führt Microsoft Analyzer Lemmatisierung statt Wortstamm. Dies bedeutet, dass sie gebeugten und unregelmäßiges Word Formulare viel besser welche Ergebnisse in mehr relevante Suchergebnisse (anzeigen Modul 7 [Azure Suche MVA](http://www.microsoftvirtualacademy.com/training-courses/adding-microsoft-azure-search-to-your-websites-and-apps) Präsentation weitere Details) behandelt werden kann.

Mit Microsoft Analyzern Indizierung ist im Durchschnitt zwei bis drei Mal langsamer als den entsprechenden Lucene, abhängig von der Sprache. Suchleistung sollte für die durchschnittliche Größe Abfragen erheblich nicht betroffen.

***Konfiguration***

Für jedes Feld in der Indexdefinition, können Sie festlegen der `analyzer` -Eigenschaft, um einen Analyzer ein, die angibt, welche Sprache und Lieferanten. Der gleiche Analyzer wird angewendet, wenn Indizierung und Suche für dieses Feld.
Beispielsweise können Sie separate Feldern für Englisch, Französisch und Spanisch Hotel Beschreibungen haben, die nebeneinander im selben Index vorhanden sind. Verwenden Sie den [Abfrageparameter 'SearchFields'](#SearchQueryParameters) , um anzugeben, welches Feld sprachspezifische in Abfragen mit suchen. Beispiele für die Abfrage, die enthalten können Sie überprüfen die `analyzer` Eigenschaft in [Dokumente durchsuchen](#SearchDocs). 

***Analyzer-Liste***

Im folgenden wird die Liste der unterstützten Sprachen zusammen mit Lucene und Microsoft Analyzer Namen ein.

<table style="font-size:12">
    <tr>
        <th>Sprache</th>
        <th>Microsoft Analyzer Namen</th>
        <th>Lucene Analyzer Namen</th>
    </tr>
    <tr>
        <td>Arabisch</td>
        <td>ar.Microsoft</td>
        <td>ar.Lucene</td>      
    </tr>
    <tr>
        <td>Armenisch</td>
        <td></td>
        <td>Hy.Lucene</td>
    </tr>
    <tr>
        <td>Bangla</td>
        <td>Bn.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Baskisch</td>
        <td></td>
        <td>EU.Lucene</td>
    </tr>
    <tr>
        <td>Bulgarisch</td>
        <td>BG.Microsoft</td>
        <td>BG.Lucene</td>
    </tr>
    <tr>
        <td>Katalanisch</td>
        <td>ca.Microsoft</td>
        <td>ca.Lucene</td>          
    </tr>
    <tr>
        <td>Chinesisch (Vereinfacht)</td>
        <td>Zh-Hans.microsoft</td>
        <td>Zh-Hans.lucene</td>     
    </tr>
    <tr>
        <td>Chinesisch (Traditionell)</td>
        <td>Zh-Hant.microsoft</td>
        <td>Zh-Hant.lucene</td>     
    <tr>
    <tr>
        <td>Kroatisch</td>
        <td>HR.Microsoft</td>
        <td/></td>
    </tr>
    <tr>
        <td>Tschechisch</td>
        <td>cs.Microsoft</td>
        <td>cs.Lucene</td>      
    </tr>    
    <tr>
        <td>Dänisch</td>
        <td>Da.Microsoft</td>
        <td>Da.Lucene</td>      
    </tr>    
    <tr>
        <td>Holländisch</td>
        <td>NL.Microsoft</td>
        <td>NL.Lucene</td>  
    </tr>    
    <tr>
        <td>Englisch</td>        
        <td>en.Microsoft</td>
        <td>en.Lucene</td>      
    </tr>
    <tr>
        <td>Estnisch</td>
        <td>et.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Finnisch</td>
        <td>Fi.Microsoft</td>
        <td>Fi.Lucene</td>      
    </tr>    
    <tr>
        <td>Französisch</td>
        <td>FR.Microsoft</td>
        <td>FR.Lucene</td>      
    </tr>
    <tr>
        <td>Galizisch</td>
        <td></td>
        <td>GL.Lucene</td>      
    </tr>
    <tr>
        <td>Deutsch</td>
        <td>de.Microsoft</td>
        <td>de.Lucene</td>      
    </tr>
    <tr>
        <td>Griechisch</td>
        <td>EL.Microsoft</td>
        <td>EL.Lucene</td>      
    </tr>
    <tr>
        <td>Gujarati</td>
        <td>gu.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Hebräisch</td>
        <td>HE.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Hindi</td>
        <td>Hi.Microsoft</td>
        <td>Hi.Lucene</td>      
    </tr>
    <tr>
        <td>Ungarisch</td>      
        <td>hu.Microsoft</td>
        <td>hu.Lucene</td>
    </tr>
    <tr>
        <td>Isländisch</td>
        <td>is.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Indonesisch (Bahasa)</td>
        <td>ID.Microsoft</td>
        <td>ID.Lucene</td>      
    </tr>
    <tr>
        <td>Irisch</td>
        <td></td>
        <td>GA.Lucene</td>
    </tr>
    <tr>
        <td>Italienisch</td>
        <td>IT.Microsoft</td>
        <td>IT.Lucene</td>      
    </tr>
    <tr>
        <td>Japanisch</td>
        <td>Ja.Microsoft</td>
        <td>Ja.Lucene</td>
        
    </tr>
    <tr>
        <td>Kannada</td>
        <td>KA.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Koreanisch</td>
        <td>Ko.Microsoft</td>
        <td>Ko.Lucene</td>
    </tr>
    <tr>
        <td>Lettisch</td>        
        <td>LV.Microsoft</td>
        <td>LV.Lucene</td>  
    </tr>
    <tr>
        <td>Litauisch</td>
        <td>lt.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Malayalam</td>
        <td>ml.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Malaiisch (Lateinisch)</td>
        <td>MS.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Marathi</td>
        <td>Mr.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Norwegisch</td>
        <td>NB.Microsoft</td>
        <td>No.Lucene</td>      
    </tr>
    <tr>
        <td>Persisch</td>
        <td></td>
        <td>FA.Lucene</td>      
    </tr>
    <tr>
        <td>Polnisch</td>
        <td>PL.Microsoft</td>
        <td>PL.Lucene</td>      
    </tr>
    <tr>
        <td>Portugiesisch (Brasilien)</td>
        <td>pt-Br.microsoft</td>
        <td>pt-Br.lucene</td>       
    </tr>
    <tr>
        <td>Portugiesisch (Portugal)</td>
        <td>pt-Pt.microsoft</td>        
        <td>pt-Pt.lucene</td>
    </tr>
    <tr>
        <td>Punjabi</td>
        <td>PA.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Rumänisch</td>
        <td>Ro.Microsoft</td>
        <td>Ro.Lucene</td>
    </tr>
    <tr>
        <td>Russisch</td>
        <td>ru.Microsoft</td>
        <td>ru.Lucene</td>  
    </tr>
    <tr>
        <td>Serbisch (Kyrillisch)</td>
        <td>SR-cyrillic.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Serbisch (Lateinisch)</td>
        <td>SR-latin.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Slowakisch</td>
        <td>SK.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Slowenisch</td>
        <td>SL.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Spanisch</td>
        <td>es.Microsoft</td>
        <td>es.Lucene</td>
    </tr>
    <tr>
        <td>Schwedisch</td>
        <td>SV.Microsoft</td>
        <td>SV.Lucene</td>
    </tr>

    <tr>
        <td>Tamil</td>
        <td>TA.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Telugu</td>
        <td>te.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Thailändisch</td>
        <td>th.Microsoft</td>
        <td>th.Lucene</td>
    </tr>
    <tr>
        <td>Türkisch</td>
        <td>tr.Microsoft</td>
        <td>tr.Lucene</td>      
    </tr>
    <tr>
        <td>Ukrainisch</td>
        <td>UK.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Urdu</td>
        <td>Your.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Vietnamesisch</td>
        <td>VI.Microsoft</td>
        <td></td>
    </tr>
    <td colspan="3">Darüber hinaus erhalten Sie Azure Suchumfang sprachunabhängig Analyzer Konfigurationen</td>
    <tr>
        <td>Standard-ASCII-Faltung</td>
        <td>standardasciifolding.Lucene</td>
        <td>
        <ul>
            <li>Unicode Text Segmentierung (Standard Tokenizer)</li>
            <li>ASCII-Falten der Filters - konvertiert Unicode-Zeichen, die in der jeweiligen ASCII-Zeichen der ersten 127 ASCII-Zeichensatz angehören nicht. Dies ist nützlich für das Entfernen von diakritischen Zeichen.</li>
        </ul>
        </td>
    </tr>
</table>

Alle Analyzern mit Namen mit <i>Lucene</i> kommentiert werden durch [den Apache Lucene Sprache Analyzern](http://lucene.apache.org/core/4_9_0/analyzers-common/overview-summary.html)betrieben. Weitere Informationen zu den ASCII Falten der Filters finden Sie [hier](http://lucene.apache.org/core/4_9_0/analyzers-common/org/apache/lucene/analysis/miscellaneous/ASCIIFoldingFilter.html).

**Suggesters**

A `suggester` definiert die Felder in einem Index verwendet werden, um Unterstützung bei der Suche AutoVervollständigen. In der Regel werden teilweise Suchbegriffe an die [Vorschläge API](#Suggestions) gesendet, während der Benutzer eine Suchabfrage eingeben ist und die API eine Reihe von Vorschlägen Ausdrücke gibt. Eine Suggester, die Sie in den Index definieren bestimmt, welche Felder verwendet werden, die während der Eingabe Suchbegriffe zu erstellen. Details zur Konfiguration finden Sie unter [Suggesters](#Suggesters) .

**Bewerten von Benutzerprofilen**

A `scoringProfile` benutzerdefinierte Punktzahl Verhaltensweisen, mit denen Sie beeinflussen können welche Elemente in den Suchergebnissen höhere werden definiert. Punktzahl Profile bestehen aus Feld-Stärken und Funktionen. Wenn sie verwenden möchten, geben Sie ein Profil anhand des Namens in der Abfragezeichenfolge an.

Ein Profil bewerten Standard arbeitet Hintergrundinformationen zum Berechnen einer Suche Bewertung für jedes Element in einem Resultset. Sie können den internen verwenden unbenannten Punktzahl Profil. Legen Sie alternativ `defaultScoringProfile` verwenden Sie ein benutzerdefiniertes Profil als Standard, dann aufgerufen, wenn Sie ein benutzerdefiniertes Profil nicht in der Abfragezeichenfolge angegeben ist.

Details finden Sie unter [Hinzufügen Punktzahl Profile, die eine Suchindex Azure Suche Dienst REST-API ()](search-api-scoring-profiles-2015-02-28-preview.md) .

**CORS Optionen**

Clientseitige Javascript kann keine APIs standardmäßig aufgerufen, da Browser verhindert, alle Cross Origin Anfragen dass. CORS (Cross-Origin gemeinsames Nutzen von Ressourcen) aktivieren, indem Sie die `corsOptions` Attribut, um Cross-Origin Abfragen an Ihre Index zu ermöglichen. Beachten Sie die einzige Abfrage APIs Support CORS Gründen der Sicherheit. Die folgenden Optionen können für CORS festgelegt werden:

- `allowedOrigins`(erforderlich): Dies ist eine Liste der Ursprung, die Zugriff auf Ihre Index erteilt werden soll. Dies bedeutet, die ihre jeweiligen Javascript-Code aus diesen Ursprung zulässig Abfragen Ihrer Index (vorausgesetzt, dass er den richtigen API Schlüssel bietet). Jede Origin ist in der Regel des Formulars `protocol://fully-qualified-domain-name:port` zwar häufig der Port ausgelassen wird. [In diesem Artikel](http://go.microsoft.com/fwlink/?LinkId=330822) Weitere Informationen hierzu finden Sie unter.
 - Wenn Sie Zugriff auf alle Ursprung zulassen möchten, fügen Sie `*` als ein einzelnes Element in der `allowedOrigins` Matrix zurück. Beachten Sie, dass **Methode für die Herstellung Suche Services nicht empfohlen.** Jedoch kann es für die Entwicklung oder Debuggen nützlich sein.
- `maxAgeInSeconds`(optional): Browser verwenden Sie diesen Wert bestimmt die Dauer (in Sekunden) zum Cache CORS Preflight-Antworten. Dies muss eine positive ganze Zahl sein. Größer dieser Wert ist, die eine bessere Leistung werden, aber die länger dauert für CORS Richtlinie Änderungen wirksam werden. Wenn sie nicht festgelegt ist, wird standardmäßig über die Dauer von 5 Minuten verwendet werden.

<a name="CreateUpdateIndexExample"></a>
**Beispiel für Textkörper anfordern**

    {
      "name": "hotels",  
      "fields": [
        {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false},
        {"name": "baseRate", "type": "Edm.Double"},
        {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
        {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
        {"name": "hotelName", "type": "Edm.String"},
        {"name": "category", "type": "Edm.String"},
        {"name": "tags", "type": "Collection(Edm.String)"},
        {"name": "parkingIncluded", "type": "Edm.Boolean"},
        {"name": "smokingAllowed", "type": "Edm.Boolean"},
        {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
        {"name": "rating", "type": "Edm.Int32"},
        {"name": "location", "type": "Edm.GeographyPoint"}
      ],
      "suggesters": [
        {
          "name": "sg",
          "searchMode": "analyzingInfixMatching",
          "sourceFields": ["hotelName"]
        }
      ]
    }

**Antwort**

Für eine erfolgreiche Anforderung: "201 erstellt".

Standardmäßig wird im Textkörper der Antwort des JSON für die Indexdefinition enthalten, die erstellt wurde. Wenn die `Prefer` Anforderung Kopfzeile auf festgelegt ist `return=minimal`, der Hauptteil einer Antwort leer und der Erfolg Statuscode werden "204 kein Inhalt" statt "201 erstellt". Dies gilt unabhängig davon, ob sich oder Beitrag zum Erstellen des Indexes verwendet wurde.

**Hinweise**

Es gibt derzeit eingeschränkte Unterstützung für die Aktualisierung von Indizes Schema. Alle Schema Aktualisierungen, die erneute Indizierung, wie etwa das Ändern von Feldtypen benötigen werden derzeit nicht unterstützt. Obwohl vorhandene Felder geändert noch gelöscht werden können, können auf einen vorhandenen Index zu einem beliebigen Zeitpunkt neuen Felder hinzugefügt werden. Wenn Sie ein neues Feld hinzugefügt wird, werden alle vorhandenen Dokumente in den Index automatisch einen Nullwert für dieses Feld sind. Keine weiteren Speicherplatz wird verwendet, bis neue Dokumente in den Index aufgenommen werden.

<a name="Suggesters"></a>
## <a name="suggesters"></a>Suggesters

Das Feature Vorschläge in Azure-Suche ist eine Ergänzung während der Eingabe oder AutoVervollständigen-Abfrage-Funktion, die Liste der potenziellen Suchbegriffe Reaktion auf teilweise Zeichenfolgeneingaben in ein Suchfeld eingegeben. Sie haben wahrscheinlich bemerkt Abfragevorschläge bei Verwendung von Suchmaschinen kommerzielle Web: Eingabe ".NET" in Bing erzeugt eine Liste von Ausdrücken für ".NET 4.5", ".NET Framework 3.5" usw.. Wenn Sie den Suchdienst REST-API verwenden zu können, benötigen Sie Vorschläge in einer benutzerdefinierten Suche Azure-Anwendung implementieren Folgendes:

- Aktivieren Sie Vorschläge durch Hinzufügen einer **Suggester** Konstruktion in Ihrem Index, den Namen, Suche Modus und einer Liste von Feldern für die während der Eingabe aufgerufen wird zugewiesen. Beispielsweise, wenn Sie "CityName" als ein Quellfeld angeben Eingabe teilweise Suchzeichenfolge "Suche" führt zu Fehlern in "Berlin", "Aussetzen" und "Seatac" (alle drei sind tatsächliche Stadtnamen), das dem Benutzer als Abfragevorschläge von angeboten.

- Rufen Sie Vorschläge durch die [Vorschläge-API](#Suggestions) in Ihrer Anwendungscode aufrufen. In der Regel werden teilweise Suchbegriffe an den Dienst gesendet, während der Benutzer eine Suchabfrage eingeben ist und diese API eine Reihe von Vorschlägen Ausdrücke gibt.

In diesem Artikel wird erläutert, wie eine **Suggester**konfigurieren. Überprüfen Sie außerdem die [Vorschläge API](#Suggestions) Weitere Informationen darüber, wie eine Suggester verwendet wird.

**Verwendung**

`Suggesters`werden in den Index und die Arbeit am besten, wenn verwendet, um bestimmte Dokumente statt an Ausdrücke oder Ausdrücke vorschlagen erstellt. Die beste Candidate Felder sind Titel, den Namen und andere relativ kurze Ausdrücke, die ein Element zu identifizieren. Weniger effektiv sind wiederholende Felder, wie etwa Kategorien und Kategorien, oder sehr lange Felder, wie z. B. Beschreibungen oder Kommentare Felder.

Als Teil der Indexdefinition, können Sie ein einzelnes Suggester zum Hinzufügen der `suggesters` Websitesammlung. Die folgenden: Eigenschaften, die eine Suggester definieren

- `name`: Der Name der Suggester. Verwenden Sie den Namen der Suggester beim Aufrufen der `suggest` API.
- `searchMode`: Die Strategie zum Suchen nach Candidate Ausdrücke verwendet. Ist der einzige Modus derzeit unterstützt `analyzingInfixMatching`, das flexible übereinstimmenden Ausdrücke am Anfang oder in der Mitte Sätze ausführt.
- `sourceFields`: Eine Liste von einem oder mehreren Feldern, die die Quelle des Inhalts für Vorschläge sind. Nur die Felder vom Typ `Edm.String` und `Collection(Edm.String)` möglicherweise Quellen für Vorschläge. Nur die Felder, die eine benutzerdefinierte Sprache Analyse festgelegt haben, nicht können verwendet werden.

**Beispiel für suggester**

Eine Suggester ist Teil des Indexes. Nur eine Suggester vorhanden sein kann, der `suggesters` Websitesammlung in der aktuellen Version, entlang der Sammlung von Feldern und `scoringProfiles`.

        {
          "name": "hotels",
          "fields": [
             . . .
           ],
          "suggesters": [
            {
            "name": "sg",
            "searchMode": "analyzingInfixMatching",
            "sourceFields: ["hotelName", "category"]
            }
          ],
          "scoringProfiles": [
             . . .
          ]
        }

> [AZURE.NOTE]  Wenn Sie den öffentlichen Vorschauversion von Azure Suche verwendet `suggesters` ersetzt eine ältere boolesche Eigenschaft (`"suggestions": false`) unterstützt, die nur Präfix Vorschläge für kurze Zeichenfolgen (3-25 Zeichen). Dem neuen, `suggesters`, unterstützt infix übereinstimmenden, die übereinstimmende Ausdrücke am Anfang oder in der Mitte der Inhalt des Felds, mit der eine bessere Fehlertoleranz für Fehler bei der Suche Zeichenfolgen findet. Beginnend mit der Version in der Regel verfügbar, ist dies jetzt die einzige Implementierung Vorschläge API. Die ältere `suggestions` Eigenschaft, die in eingeführt wurde `api-version=2014-07-31-Preview` befindet sich in dieser Version arbeiten, jedoch kein Betrieb innerhalb der `2015-02-28` oder höhere Versionen von Azure suchen.

<a name="UpdateIndex"></a>
## <a name="update-index"></a>Index aktualisieren

Sie können mit Azure Suche mithilfe einer Anforderung HTTP setzen einen vorhandenen Index aktualisieren. Hinzufügen von neuen Feldern zu vorhandenen Schema, CORS Optionen ändern und das Ändern der Punktzahl Profile können vorgenommene Aktualisierungen umfassen. Weitere Informationen finden Sie unter [Hinzufügen von Profilen bewerten](https://msdn.microsoft.com/library/azure/dn798928.aspx) . Sie geben Sie den Namen des Indexes auf die Anfrage-URI zu aktualisieren:

    PUT https://[search service url]/indexes/[index name]?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

**Wichtige:** Unterstützung für die Aktualisierung von Indizes Schema ist auf Vorgänge, die den Suchindex Neuerstellen erfordern nicht beschränkt. Alle Schema Aktualisierungen, die erneute Indizierung, wie etwa das Ändern von Feldtypen, benötigen werden derzeit nicht unterstützt. Neuen Felder können jederzeit hinzugefügt werden zwar vorhandene Felder geändert noch gelöscht werden können. Dasselbe gilt für `suggesters`. Neue Felder hinzugefügt werden können, eine Suggester zum Zeitpunkt der Felder hinzugefügt werden, aber Felder können nicht aus entfernt werden `suggesters` vorhandene Felder hinzugefügt werden können, um `suggesters`.

Wenn Index ein neues Feld hinzufügen, werden alle vorhandenen Dokumente in den Index automatisch einen Nullwert für dieses Feld sind. Keine weiteren Speicherplatz wird verwendet, bis neue Dokumente in den Index aufgenommen werden.

**Anfordern**

HTTPS ist für alle Serviceanfragen erforderlich. Die Anfrage **Index aktualisieren** wird mit HTTP setzen erstellt. Mit sich ist der Name des Indexes Teil der URL. Wenn Sie der Index nicht vorhanden ist, wird er erstellt. Wenn der Index bereits vorhanden ist, wird es an die neue Definition aktualisiert.

Den Namen des Indexes muss in Kleinbuchstaben angegeben werden, beginnen mit einem Buchstaben oder eine Zahl, haben keine Bindestriche oder Punkte und weniger als 128 Zeichen lang sein. Nach dem Starten den Namen des Indexes mit einem Buchstaben oder eine Zahl, kann die restlichen den Namen alle Buchstaben, Zahl und Striche, einbeziehen, solange die Striche nicht aufeinander folgen.

`api-version=[string]`(erforderlich). Die Preview-Version ist `api-version=2015-02-28-Preview`. Details und alternativen Versionen finden Sie unter [Suchen Dienst Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) .

**Anfordern von Kopfzeilen**

Die folgende Liste beschreibt die erforderlichen und optionalen Anforderung Überschriften.

- `Content-Type`: Erforderlich ist. Legen Sie den Wert`application/json`
- `api-key`: Erforderlich ist. Die `api-key` wird verwendet, um die Anfrage Suchdienst authentifizieren. Es ist ein Zeichenfolgenwert mit dem Dienst. Die Anfrage **Index aktualisieren** darf enthalten eine `api-key` Kopfzeile mit Ihrem Administrator Key (im Gegensatz zu einer Abfrageschlüssel) festlegen.

Sie benötigen außerdem den Dienstnamen, um die Anfrage-URL zu erstellen. Sie können den Dienstnamen erhalten und `api-key` aus Ihrem Dienst Dashboard im Portal Azure. Finden Sie unter [Erstellen einer Azure Suchdienst im Portal](search-create-service-portal.md) für die Seitennavigation helfen.

**Die Syntax der Textkörper anfordern**

Wenn Sie einen vorhandenen Index aktualisieren, muss gegebenenfalls Textkörper der ursprünglichen Schemadefinition plus die neuen Felder, die Sie hinzufügen möchten, als auch die geänderte Punktzahl Profile, Suggesters und CORS Optionen beinhalten. Wenn Sie nicht die Punktzahl Profile und CORS Optionen ändern, müssen Sie die ursprünglichen aus der Erstellung des Indexes einfügen. Im Allgemeinen das beste Muster zum nach Updates suchen wird die Indexdefinition für eine GET abgerufen werden, ändern Sie diesen, und klicken Sie dann mit sich zu aktualisieren.

Die Schemasyntax verwendet, um einen Index zu erstellen, wird hier zur Vereinfachung reproduziert. Weitere Informationen hierzu finden Sie unter [Index erstellen](#CreateIndex) .

    {
      "name": (optional) "name_of_index",
      "fields": [
        {
          "name": "name_of_field",
          "type": "Edm.String | Collection(Edm.String) | Edm.Int32 | Edm.Int64 | Edm.Double | Edm.Boolean | Edm.DateTimeOffset | Edm.GeographyPoint",
          "searchable": true (default where applicable) | false (only Edm.String and Collection(Edm.String) fields can be searchable),
          "filterable": true (default) | false,
          "sortable": true (default where applicable) | false (Collection(Edm.String) fields cannot be sortable),
          "facetable": true (default where applicable) | false (Edm.GeographyPoint fields cannot be facetable),
          "key": true | false (default, only Edm.String fields can be keys),
          "retrievable": true (default) | false, 
          "analyzer": "name of the analyzer used for search and indexing", (only if 'searchAnalyzer' and 'indexAnalyzer' are not set)
          "searchAnalyzer": "name of the search analyzer", (only if 'indexAnalyzer' is set and 'analyzer' is not set)
          "indexAnalyzer": "name of the indexing analyzer" (only if 'searchAnalyzer' is set and 'analyzer' is not set)
        }
      ],
      "suggesters": [
        {
          "name": "name of suggester",
          "searchMode": "analyzingInfixMatching" (other modes may be added in the future),
          "sourceFields": ["field1", "field2", ...]
        }
      ],
      "scoringProfiles": [
        {
          "name": "name of scoring profile",
          "text": (optional, only applies to searchable fields) {
            "weights": {
              "searchable_field_name": relative_weight_value (positive numbers),
              ...
            }
          },
          "functions": (optional) [
            {
              "type": "magnitude | freshness | distance | tag",
              "boost": # (positive number used as multiplier for raw score != 1),
              "fieldName": "...",
              "interpolation": "constant | linear (default) | quadratic | logarithmic",
              "magnitude": {
                "boostingRangeStart": #,
                "boostingRangeEnd": #,
                "constantBoostBeyondRange": true | false (default)
              },
              "freshness": {
                "boostingDuration": "..." (value representing timespan leading to now over which boosting occurs)
              },
              "distance": {
                "referencePointParameter": "...", (parameter to be passed in queries to use as reference location, see "scoringParameter" for syntax details)
                "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends)
              },
              "tag": {
                "tagsParameter": "..." (parameter to be passed in queries to specify list of tags to compare against target field, see "scoringParameter" for syntax details)
              }
            }
          ],
          "functionAggregation": (optional, applies only when functions are specified)
            "sum (default) | average | minimum | maximum | firstMatching"
        }
      ],
      "analyzers":(optional)[ ... ],
      "charFilters":(optional)[ ... ],
      "tokenizers":(optional)[ ... ],
      "tokenFilters":(optional)[ ... ],
      "defaultScoringProfile": (optional) "...",
      "corsOptions": (optional) {
        "allowedOrigins": ["*"] | ["origin_1", "origin_2", ...],
        "maxAgeInSeconds": (optional) max_age_in_seconds (non-negative integer)
      }
    }


**Antwort**

Für eine erfolgreiche Anforderung: "204 kein Inhalt".

Standardmäßig wird die Antwort Textkörper leer sein. Jedoch, wenn die `Prefer` Anforderung Kopfzeile auf festgelegt ist `return=representation`, der Hauptteil einer Antwort wird die JSON für die Indexdefinition, die aktualisiert wurde, enthalten. In diesem Fall werden der Erfolg Statuscode "200 OK".

**Aktualisieren von Indexdefinition mit benutzerdefinierten Analyzern**

Nachdem Sie eine Analyzer, eine Tokenizer, einen token oder ein Zeichen Filter definiert ist, kann es nicht geändert werden. Neue hinzugefügt werden können, einem vorhandenen Index nur, wenn die `allowIndexDowntime` auf die Kennzeichnung festgelegt ist WAHR in der Besprechungsanfrage Index aktualisieren: 

`PUT https://[search service name].search.windows.net/indexes/[index name]?api-version=[api-version]&allowIndexDowntime=true`

Beachten Sie, dass dieser Vorgang Ihrer Index für mindestens ein paar Sekunden, die bewirken, dass Ihre Indizierung und Abfrage Anfragen zum Fehlschlagen offline verwendet werden soll. Leistung und Schreiben Verfügbarkeit des Indexes kann es sich für mehrere Minuten nach der Aktualisierung des Indexes beeinträchtigt oder mehr für sehr großen Indizes.

<a name="ListIndexes"></a>
## <a name="list-indexes"></a>Liste von Indizes

Der Vorgang **Liste Indizes** gibt eine Liste der Indizes derzeit in Azure Suchdienst.

    GET https://[service name].search.windows.net/indexes?api-version=[api-version]
    api-key: [admin key]

**Anfordern**

HTTPS ist für alle Serviceanfragen erforderlich. Die **Liste Indizes** Anforderung kann mithilfe der GET-Methode erstellt werden.

`api-version=[string]`(erforderlich). Die Preview-Version ist `api-version=2015-02-28-Preview`. Details und alternativen Versionen finden Sie unter [Suchen Dienst Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) .

**Anfordern von Kopfzeilen**

Die folgende Liste beschreibt die erforderlichen und optionalen Anforderung Überschriften.

- `api-key`: Erforderlich ist. Die `api-key` wird verwendet, um die Anfrage Suchdienst authentifizieren. Es ist ein Zeichenfolgenwert mit dem Dienst. Die **Liste Indizes** Anforderung darf enthalten eine `api-key` setzen Sie auf ein Administrator (im Gegensatz zu einer Abfrage-Taste).

Sie benötigen außerdem den Dienstnamen, um die Anfrage-URL zu erstellen. Sie können den Dienstnamen erhalten und `api-key` aus Ihrem Dienst Dashboard im Portal Azure. Finden Sie unter [Erstellen einer Azure Suchdienst im Portal](search-create-service-portal.md) für die Seitennavigation helfen.

**Anforderungstexts**

Keine.

**Antwort**

: Status 200 ist OK für eine erfolgreiche Antwort ausgegeben.

Hier ist ein Beispiel für Antwort Textkörper aus:

    {
      "value": [
        {
          "name": "Books",
          "fields": [
            {"name": "ISBN", ...},
            ...
          ]
        },
        {
          "name": "Games",
          ...
        },
        ...
      ]
    }

Beachten Sie, dass Sie die Antwort auf nur die Eigenschaften filtern können, die Sie interessiert. Beispielsweise, wenn Sie nur eine Liste der Indexnamen möchten, verwenden den OData `$select` Option Abfrage:

    GET /indexes?api-version=2015-02-28-Preview&$select=name

In diesem Fall würde die Antwort aus dem oben genannten Beispiel wie folgt aussehen:

    {
      "value": [
        {"name": "Books"},
        {"name": "Games"},
        ...
      ]
    }

Dies ist eine nützliche Technik, um Bandbreite zu sparen, wenn Sie viele Indizes Suchdienst haben.

<a name="GetIndex"></a>
## <a name="get-index"></a>Abrufen von Index

Der **Erste Index** Vorgang ruft die Indexdefinition aus Azure-Suche ab.

    GET https://[service name].search.windows.net/indexes/[index name]?api-version=[api-version]
    api-key: [admin key]

**Anfordern**

HTTPS ist für Serviceanfragen erforderlich. Die **Erste Index** Anforderung kann mithilfe der GET-Methode erstellt werden.

[Indexname] in der Besprechungsanfrage URI Gibt an, welche Index aus der Sammlung von Indizes zurückgegeben.

`api-version=[string]`(erforderlich). Die Preview-Version ist `api-version=2015-02-28-Preview`. Details und alternativen Versionen finden Sie unter [Suchen Dienst Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) .

**Anfordern von Kopfzeilen**

Die folgende Liste beschreibt die erforderlichen und optionalen Anforderung Überschriften.

- `api-key`: Die `api-key` wird verwendet, um die Anfrage Suchdienst authentifizieren. Es ist ein Zeichenfolgenwert mit dem Dienst. Die **Erste Index** Anforderung darf enthalten eine `api-key` setzen Sie auf ein Administrator (im Gegensatz zu einer Abfrage-Taste).

Sie benötigen außerdem den Dienstnamen, um die Anfrage-URL zu erstellen. Sie können den Dienstnamen erhalten und `api-key` aus Ihrem Dienst Dashboard im Portal Azure. Finden Sie unter [Erstellen einer Azure Suchdienst im Portal](search-create-service-portal.md) für die Seitennavigation helfen.

**Anforderungstexts**

Keine.

**Antwort**

: Status 200 ist OK für eine erfolgreiche Antwort ausgegeben.

Siehe Beispiel JSON in [Erstellen und Aktualisieren eines Indexes](#CreateUpdateIndexExample) ein Beispiel für die Antwort Nutzlast.

<a name="DeleteIndex"></a>
## <a name="delete-index"></a>Index löschen

Der Vorgang **Index löschen** entfernt eines Indexes und die zugehörigen Dokumente Azure Suchdienst. Sie können den Namen des Indexes aus dem Dienst Dashboard im Azure-Portal oder aus der-API erhalten möchten. Details finden Sie in der [Liste Indizes](#ListIndexes) .

    DELETE https://[service name].search.windows.net/indexes/[index name]?api-version=[api-version]
    api-key: [admin key]

**Anfordern**

HTTPS ist für Serviceanfragen erforderlich. Die Anfrage **Index löschen** kann mithilfe der Methode löschen erstellt werden.

[Indexname] in der Besprechungsanfrage URI Gibt an, welcher Index aus der Sammlung von Indizes löschen.

`api-version=[string]`(erforderlich). Die Preview-Version ist `api-version=2015-02-28-Preview`. Details und alternativen Versionen finden Sie unter [Suchen Dienst Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) .

**Anfordern von Kopfzeilen**

Die folgende Liste beschreibt die erforderlichen und optionalen Anforderung Überschriften.

- `api-key`: Erforderlich ist. Die `api-key` wird verwendet, um die Anfrage Suchdienst authentifizieren. Es ist ein Zeichenfolgenwert mit Ihrem Dienst-URL aus. Die Anfrage **Index löschen** darf enthalten eine `api-key` Kopfzeile mit Ihrem Administrator Key (im Gegensatz zu einer Abfrageschlüssel) festlegen.

Sie benötigen außerdem den Dienstnamen, um die Anfrage-URL zu erstellen. Sie können den Dienstnamen erhalten und `api-key` aus Ihrem Dienst Dashboard im Portal Azure. Finden Sie unter [Erstellen einer Azure Suchdienst im Portal](search-create-service-portal.md) für die Seitennavigation helfen.

**Anforderungstexts**

Keine.

**Antwort**

: Status 204 ist kein Inhalt für eine erfolgreiche Antwort ausgegeben.

<a name="GetIndexStats"></a>
## <a name="get-index-statistics"></a>Indexstatistik abrufen

Der **Erste Indexstatistik** Vorgang gibt Dokumentanzahl für den aktuellen Index sowie speichernutzung aus Azure suchen aus.

    GET https://[service name].search.windows.net/indexes/[index name]/stats?api-version=[api-version]
    api-key: [admin key]

> [AZURE.NOTE] Statistische Daten zu zählen und Speicher Dokumentgröße werden alle paar Minuten, nicht in Echtzeit erfasst. Die von dieser API zurückgegeben Statistiken möglicherweise nicht spiegeln Änderungen durch aktuelle Indizierung Vorgänge verursacht.

**Anfordern**

HTTPS ist für alle Dienste Anfragen erforderlich. Die **Erste Indexstatistik** Anforderung kann mithilfe der GET-Methode erstellt werden.

[Indexname] in der Besprechungsanfrage URI wird den Dienst Indexstatistik für den angegebenen Index zurückgibt.

`api-version=[string]`(erforderlich). Die Preview-Version ist `api-version=2015-02-28-Preview`. Details und alternativen Versionen finden Sie unter [Suchen Dienst Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) .


**Anfordern von Kopfzeilen**

Die folgende Liste beschreibt die erforderlichen und optionalen Anforderung Überschriften.

- `api-key`: Die `api-key` wird verwendet, um die Anfrage Suchdienst authentifizieren. Es ist ein Zeichenfolgenwert mit dem Dienst. Die **Erste Indexstatistik** Anforderung darf enthalten eine `api-key` setzen Sie auf ein Administrator (im Gegensatz zu einer Abfrage-Taste).

Sie benötigen außerdem den Dienstnamen, um die Anfrage-URL zu erstellen. Sie können den Dienstnamen erhalten und `api-key` aus Ihrem Dienst Dashboard im Portal Azure. Finden Sie unter [Erstellen einer Azure Suchdienst im Portal](search-create-service-portal.md) für die Seitennavigation helfen.

**Anforderungstexts**

Keine.

**Antwort**

: Status 200 ist OK für eine erfolgreiche Antwort ausgegeben.

Antwort Textkörper befindet sich in folgendem Format:

    {
      "documentCount": number,
      "storageSize": number (size of the index in bytes)
    }

<a name="TestAnalyzer"></a>
## <a name="test-analyzer"></a>Analyzers

Die **API analysieren** zeigt an, wie eine Analyzer Text in Token unterteilt.

    POST https://[service name].search.windows.net/indexes/[index name]/analyze?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

**Anfordern**

HTTPS ist für alle Dienste Anfragen erforderlich. Die Anforderung **Analysieren API** kann mithilfe der Methode Beitrag erstellt werden.

`api-version=[string]`(erforderlich). Die Preview-Version ist `api-version=2015-02-28-Preview`. Details und alternativen Versionen finden Sie unter [Suchen Dienst Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) .


**Anfordern von Kopfzeilen**

Die folgende Liste beschreibt die erforderlichen und optionalen Anforderung Überschriften.

- `api-key`: Die `api-key` wird verwendet, um die Anfrage Suchdienst authentifizieren. Es ist ein Zeichenfolgenwert mit dem Dienst. Die Anforderung **Analysieren API** darf enthalten eine `api-key` setzen Sie auf ein Administrator (im Gegensatz zu einer Abfrage-Taste).

Sie benötigen außerdem den Namen des Indexes und den Dienstnamen, um die Anfrage-URL zu erstellen. Sie können den Dienstnamen erhalten und `api-key` aus Ihrem Dienst Dashboard im Portal Azure. Finden Sie unter [Erstellen einer Azure Suchdienst im Portal](search-create-service-portal.md) für die Seitennavigation helfen.

**Anforderungstexts**

    {
      "text": "Text to analyze",
      "analyzer": "analyzer_name"
    }

oder

    {
      "text": "Text to analyze",
      "tokenizer": "tokenizer_name",
      "tokenFilters": (optional) [ "token_filter_name" ],
      "charFilters": (optional) [ "char_filter_name" ]
    }

Die `analyzer_name`, `tokenizer_name`, `token_filter_name` und `char_filter_name` gültige Namen von vordefinierten oder benutzerdefinierten Analyzern, Tokenizer, token Filter und Zeichen Filter für den Index sein müssen. Informationen finden Sie unter Weitere Informationen zu den Prozess der lexikalischen Analyse [Analyse in Azure suchen](https://aka.ms/azsanalysis).

**Antwort**

: Status 200 ist OK für eine erfolgreiche Antwort ausgegeben.

Antwort Textkörper befindet sich in folgendem Format:

    {
      "tokens": [
        {
          "token": string (token),
          "startOffset": number (index of the first character of the token),
          "endOffset": number (index of the last character of the token),
          "position": number (position of the token in the input text)
        },
        ...
      ]
    }

**Beispiel für API analysieren**

**Anfordern**

    {
      "text": "Text to analyze",
      "analyzer": "standard"
    }

**Antwort**

    {
      "tokens": [
        {
          "token": "text",
          "startOffset": 0,
          "endOffset": 4,
          "position": 0
        },
        {
          "token": "to",
          "startOffset": 5,
          "endOffset": 7,
          "position": 1
        },
        {
          "token": "analyze",
          "startOffset": 8,
          "endOffset": 15,
          "position": 2
        }
      ]
    }

________________________________________
<a name="DocOps"></a>
## <a name="document-operations"></a>Dokument-Vorgänge

In Azure suchen ein Index in der Cloud gespeichert ist und aufgefüllt, indem die JSON-Dokumente, die Sie zum Dienst hochladen. Alle Dokumente, die Sie hochladen bestehen die trennen Ihrer Suche Daten aus. Dokumente enthalten Felder, von die einige in Suchbegriffe Token, wie diese hochgeladen werden. Die `/docs` URL Segment in der Azure-Suche-API stellt die Sammlung von Dokumenten in einem Index. Alle Vorgänge ausgeführt wird, klicken Sie auf die Auflistung, z. B. hochladen, zusammenführen, löschen oder Abfragen Dokumente übernehmen platzieren im Kontext eines einzelnen Indexes, also die URLs für diese Vorgänge immer mit anfangen, `/indexes/[index name]/docs` für einen angegebenen Index angezeigt.

Ihrer Anwendungscode muss entweder generieren JSON-Dokumente hochladen in Azure suchen, oder Sie können einen [Indexer](https://msdn.microsoft.com/library/dn946891.aspx) verwenden, um Dokumente zu laden, wenn die Datenquelle entweder Azure SQL-Datenbank oder DocumentDB ist. In der Regel werden Indizes aus einem einzelnen Dataset aufgefüllt, die Sie bereitstellen.

Planen Sie haben ein Dokument für jedes Element, das Sie suchen möchten. Eine Film Miete Anwendung möglicherweise ein Dokument pro Film, eine Anwendung Ladenzeile möglicherweise ein Dokument pro SKU, Anwendung online durchgeführten möglicherweise ein Dokument pro Kurs, Recherchieren Firma haben ein Dokument für jede academic Papier in deren Repository usw..

Dokumente bestehen aus einem oder mehreren Feldern. Text ist Token Azure Suche in Suchbegriffe, als auch nicht mit Token versehen oder nicht-Text-Werte, die in Filter oder Punktzahl Profile verwendet werden können, können Felder enthalten. Die Namen, Datentypen und Suchfunktionen für jedes Feld unterstützt werden durch das IndexSchema bestimmt. Eines der Felder in den beiden Schemas Index muss als eine ID festgelegt werden, und jedes Dokument muss einen Wert für das Feld ID, das das Dokument im Index eindeutig haben. Alle anderen Dokumentfelder sind optional und werden standardmäßig auf einen Nullwert Links ohne Angabe. Beachten Sie, dass null-Werte nicht von Speicherplatz in den Suchindex akzeptieren.

Bevor Sie Dokumente hochladen können, müssen Sie bereits im Index-Dienst erstellt haben. Details zu dieser erste Schritt finden Sie unter [Index erstellen](#CreateIndex) .

<a name="AddOrUpdateDocuments"></a>
## <a name="add-update-or-delete-documents"></a>Hinzufügen, aktualisieren oder Löschen von Dokumenten

Sie können hochladen, zusammenführen, Zusammenführen oder hochladen oder Löschen von Dokumenten aus einem angegebenen Index unter Verwendung von HTTP POST. Für eine große Anzahl von Updates wird von Dokumenten auf (1000 Dokumente pro Stapel) oder ungefähr 16 MB pro Stapel Batchverarbeitung empfohlen.

    POST https://[service name].search.windows.net/indexes/[index name]/docs/index?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

**Anfordern**

HTTPS ist für alle Serviceanfragen erforderlich. Sie können hochladen, zusammenführen, Zusammenführen oder hochladen oder Löschen von Dokumenten aus einem angegebenen Index unter Verwendung von HTTP POST.

Die Anforderung URI enthält [Indexname], welche Index, um Dokumente Posten angeben. Sie können Dokumente an einen Index nur jeweils bereitstellen.

`api-version=[string]`(erforderlich). Die Preview-Version ist `api-version=2015-02-28-Preview`. Details und alternativen Versionen finden Sie unter [Suchen Dienst Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) .

**Anfordern von Kopfzeilen**

Die folgende Liste beschreibt die erforderlichen und optionalen Anforderung Überschriften.

- `Content-Type`: Erforderlich ist. Legen Sie den Wert`application/json`
- `api-key`: Erforderlich ist. Die `api-key` wird verwendet, um die Anfrage Suchdienst authentifizieren. Es ist ein Zeichenfolgenwert mit dem Dienst. Die Anfrage **Dokumente hinzufügen** darf enthalten eine `api-key` Kopfzeile mit Ihrem Administrator Key (im Gegensatz zu einer Abfrageschlüssel) festlegen.

Sie benötigen außerdem den Dienstnamen, um die Anfrage-URL zu erstellen. Sie können den Dienstnamen erhalten und `api-key` aus Ihrem Dienst Dashboard im Portal Azure. Finden Sie unter [Erstellen einer Azure Suchdienst im Portal](search-create-service-portal.md) für die Seitennavigation helfen.

**Anforderungstexts**

Hauptteil der Anforderung enthält ein oder mehrere Dokumente indiziert werden sollen. Dokumente werden durch einen eindeutigen Schlüssel identifiziert. Jedes Dokument wird eine Aktion zugeordnet: hochladen, zusammenführen, MergeOrUpload oder löschen. Upload-Anfragen müssen die Daten als eine Reihe von Schlüssel/Wert-Paare enthalten.

    {
      "value": [
        {
          "@search.action": "upload (default) | merge | mergeOrUpload | delete",
          "key_field_name": "unique_key_of_document", (key/value pair for key field from index schema)
          "field_name": field_value (key/value pairs matching index schema)
            ...
        },
        ...
      ]
    }

> [AZURE.NOTE] Dokument Tasten können nur Buchstaben, Zahlen, Striche enthalten ("-"), Unterstriche ("_") und Gleichheitszeichen ("="). Weitere Informationen hierzu finden Sie unter [Naming Regeln](https://msdn.microsoft.com/library/azure/dn857353.aspx).

**Dokumentenmappen**

- `upload`: Eine Uploadaktion ist vergleichbar mit einer "Upsert", wird das Dokument eingefügt, wenn es ist neu und aktualisiert/ersetzt, sofern vorhanden. Beachten Sie, dass alle Felder in den Fall Update ersetzt werden.
- `merge`: Zusammenführen aktualisiert ein vorhandenes Dokument mit den angegebenen Feldern. Wenn das Dokument nicht vorhanden ist, wird die Zusammenführung fehl. Ein beliebiges Feld in einem Seriendruck angegebenen ersetzt das bestehende Feld im Dokument. Dies umfasst die Felder vom Typ `Collection(Edm.String)`. Beispielsweise, wenn das Dokument ein Feld "Kategorien" mit dem Wert enthält `["budget"]` , und führen Sie einen Seriendruck mit dem Wert `["economy", "pool"]` für "Kategorien", werden der letzte Wert des Felds "Kategorien" `["economy", "pool"]`. Es wird **keine** werden `["budget", "economy", "pool"]`.
- `mergeOrUpload`: verhält sich wie `merge` Wenn ein Dokument mit dem angegebenen Schlüssel im Index bereits vorhanden ist. Wenn das Dokument nicht vorhanden ist verhält sich wie `upload` mit einem neuen Dokument.
- `delete`: Löschen entfernt das angegebene Dokument aus dem Index. Beachten Sie, dass Sie alle Felder im Angeben einer `delete` wird als Feld mit dem Vorgang ignoriert. Wenn Sie ein einzelnes Feld aus einem Dokument entfernen möchten, verwenden Sie `merge` stattdessen und einfach legen Sie das Feld explizit zu `null`.

**Antwort**

Statuscode 200 zurückgegeben (OK) für eine erfolgreiche Antwort, was bedeutet, dass alle Elemente erfolgreich indiziert wurden. Dies wird angegeben, indem die `status` legen Sie die Eigenschaft wird auf für alle Elemente, auch als WAHR der `statusCode` Eigenschaft entweder 201 (für neu hochgeladene Dokumente) oder 200 (für verbundene oder gelöschte Dokumente) festgelegt wird:

    {
      "value": [
        {
          "key": "unique_key_of_new_document",
          "status": true,
          "errorMessage": null,
          "statusCode": 201
        },
        {
          "key": "unique_key_of_merged_document",
          "status": true,
          "errorMessage": null,
          "statusCode": 200
        },
        {
          "key": "unique_key_of_deleted_document",
          "status": true,
          "errorMessage": null,
          "statusCode": 200
        }
      ]
    }  

Statuscode 207 (Multi-Status) wird zurückgegeben, wenn mindestens ein Element nicht erfolgreich indiziert wurde. Elemente, die nicht indiziert wurden haben die `status` Feld auf falsch festgelegt. Die `errorMessage` und `statusCode` Eigenschaften werden die Ursache des Fehlers Indizierung anzugeben:

    {
      "value": [
        {
          "key": "unique_key_of_document_1",
          "status": false,
          "errorMessage": "The search service is too busy to process this document. Please try again later.",
          "statusCode": 503
        },
        {
          "key": "unique_key_of_document_2",
          "status": false,
          "errorMessage": "Document not found.",
          "statusCode": 404
        },
        {
          "key": "unique_key_of_document_3",
          "status": false,
          "errorMessage": "Index is temporarily unavailable because it was updated with the 'allowIndexDowntime' flag set to 'true'. Please try again later.",
          "statusCode": 422
        }
      ]
    }  

Die folgende Tabelle erläutert die verschiedenen pro Dokument Statuscodes, die in der Antwort zurückgegeben werden können. Beachten Sie, dass einige Probleme mit der Anforderung selbst, darauf hinzuweisen, während andere temporäre Fehler hinzuweisen. Letztere, die Sie nach einer kurzen Verzögerung wiederholen soll.

<table style="font-size:12">
    <tr>
        <th>Statuscode</th>
        <th>Bedeutung</th>
        <th>Aufforderung zum Wiederholen</th>
        <th>Notizen</th>
    </tr>
    <tr>
        <td>200</td>
        <td>Dokument wurde erfolgreich geändert oder gelöscht.</td>
        <td>n/v</td>
        <td>Löschen von Vorgängen sind <a href="https://en.wikipedia.org/wiki/Idempotence">Idempotent</a>. D. h., auch wenn ein Dokumentschlüssel im Index nicht vorhanden ist, führt versuchen, einen Löschvorgang mit diesem Schlüssel einen Statuscode 200.</td>
    </tr>
    <tr>
        <td>201</td>
        <td>Dokument wurde erfolgreich erstellt.</td>
        <td>n/v</td>
        <td></td>
    </tr>
    <tr>
        <td>400</td>
        <td>Klicken Sie in das Dokument, das sie indiziert wird verhindert, dass ein Fehler aufgetreten ist.</td>
        <td>Nein</td>
        <td>Die Fehlermeldung in der Antwort weist darauf hin, was mit dem Dokument falsch ist.</td>
    </tr>
    <tr>
        <td>404</td>
        <td>Das Dokument konnte nicht zusammengeführt werden, da der angegebene Schlüssel im Index nicht vorhanden ist.</td>
        <td>Nein</td>
        <td>Dieser Fehler tritt nicht für Uploads, da diese Erstellen neuer Dokumente, und es tritt nicht für Löschvorgänge auf, da sie <a href="https://en.wikipedia.org/wiki/Idempotence">Idempotent</a>sind.</td>
    </tr>
    <tr>
        <td>409</td>
        <td>Beim Versuch, ein Dokument zu indizieren ein Versionskonflikt gefunden.</td>
        <td>Ja</td>
        <td>Dies kann geschehen, wenn Sie versuchen, dasselbe Dokument mehr als einmal gleichzeitig indizieren.</td>
    </tr>
    <tr>
        <td>422</td>
        <td>Der Index ist vorübergehend nicht verfügbar, weil es mit 'AllowIndexDowntime' Kennzeichnung festlegen auf "True" aktualisiert wurde.</td>
        <td>Ja</td>
        <td></td>
    </tr>
    <tr>
        <td>503</td>
        <td>Suchdienst ist vorübergehend nicht verfügbar, möglicherweise aufgrund hoher Auslastung.</td>
        <td>Ja</td>
        <td>Code sollte warten, bevor Sie in diesem Fall wiederholen, oder Sie riskieren verlängern der der Dienst ist nicht verfügbar.</td>
    </tr>
</table> 

**Hinweis**: Wenn im Clientcode häufig Reaktionen 207 auftritt, ist eine mögliche Ursache, dass das System Auslastung. Sie können dies überprüfen, indem Sie überprüfen die `statusCode` Eigenschaft für 503. Wenn dies der Fall ist, wird empfohlen, ***begrenzungsebene Indizierung Serviceanfragen***. Andernfalls Wenn Indizierung Datenverkehr Datenträgerressourcen nicht, konnte das System gestartet werden alle Besprechungsanfragen mit Fehlern 503 ablehnen.

Statuscode 429 gibt an, dass Sie Ihr Kontingent für die Anzahl der Dokumente pro Index überschritten haben. Sie müssen einen neuen Index erstellen oder für höhere Kapazität Grenzwerte aktualisieren.

**Beispiel:**

    {
      "value": [
        {
          "@search.action": "upload",
          "hotelId": "1",
          "baseRate": 199.0,
          "description": "Best hotel in town",
          "description_fr": "Meilleur hôtel en ville",
          "hotelName": "Fancy Stay",
          "category": "Luxury",
          "tags": ["pool", "view", "wifi", "concierge"],
          "parkingIncluded": false,
          "smokingAllowed": false,
          "lastRenovationDate": "2010-06-27T00:00:00Z",
          "rating": 5,
          "location": { "type": "Point", "coordinates": [-122.131577, 47.678581] }
        },
        {
          "@search.action": "upload",
          "hotelId": "2",
          "baseRate": 79.99,
          "description": "Cheapest hotel in town",
          "description_fr": "Hôtel le moins cher en ville",
          "hotelName": "Roach Motel",
          "category": "Budget",
          "tags": ["motel", "budget"],
          "parkingIncluded": true,
          "smokingAllowed": true,
          "lastRenovationDate": "1982-04-28T00:00:00Z",
          "rating": 1,
          "location": { "type": "Point", "coordinates": [-122.131577, 49.678581] }
        },
        {
          "@search.action": "merge",
          "hotelId": "3",
          "baseRate": 279.99,
          "description": "Surprisingly expensive",
          "lastRenovationDate": null
        },
        {
          "@search.action": "delete",
          "hotelId": "4"
        }
      ]
    }
________________________________________
<a name="SearchDocs"></a>
## <a name="search-documents"></a>Suchen von Dokumenten

**Ein Suchvorgang** wird als eine Anforderung GET oder POST ausgegeben und gibt die Parameter, die die Kriterien zum Auswählen von übereinstimmenden Dokumente zu verleihen.

    GET https://[service name].search.windows.net/indexes/[index name]/docs?[query parameters]
    api-key: [admin or query key]

    POST https://[service name].search.windows.net/indexes/[index name]/docs/search?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin or query key]

**Verwenden von Beitrag statt abrufen**

Wenn Sie HTTP-GET zum Aufrufen der **Suche** -API verwenden, müssen Sie beachten, dass die Länge der URL der Anforderung 8 KB überschreiten, kann nicht. Dies ist normalerweise genug für die meisten Applikationen. Sehr großen Abfragen oder Filterausdrücke OData wird jedoch einige Applikationen erzeugt. In diesem Bereich ist die unter Verwendung von HTTP POST besser geeignet, da größere Filter und Abfragen als abrufen können. Mit bereitstellen ist die Anzahl der Begriffe oder in einer Abfrage Klauseln die Beschränkung nicht die Größe der unformatierten Abfrage, da das Anforderung Größenlimit für Beitrag ungefähr 16 MB ist.

> [AZURE.NOTE] Obwohl das Größenlimit der Beitrag Anforderung sehr groß ist, können nicht Suchabfragen und Filterausdrücke beliebig komplex sein. Weitere Informationen zur Suche Abfrage- und Filter Komplexität Einschränkungen finden Sie unter [Lucene Abfragesyntax](https://msdn.microsoft.com/library/mt589323.aspx) und zur [Ausdruckssyntax OData](https://msdn.microsoft.com/library/dn798921.aspx) .

**Anfordern**

HTTPS ist für Serviceanfragen erforderlich. **Die Suchabfrage** kann mithilfe der Methoden GET oder POST erstellt werden.

Die Anfrage-URI Gibt an, welcher Index Abfrage, die für alle Dokumente, die die Parameter entsprechen. Parameter in der Abfragezeichenfolge bei GET-Anfragen angegeben werden, und in der Besprechungsanfrage Stelle in Beitrag anfordert.

Als bewährte Methode GET-Anfragen erstellen Denken Sie daran, [URL codieren](https://msdn.microsoft.com/library/system.uri.escapedatastring.aspx) von bestimmter Abfrageparameter, wenn die REST-API direkt aufrufen. Für **Suchvorgänge** enthält dies:

- `$filter`
- `facet`
- `highlightPreTag`
- `highlightPostTag`
- `search`
- `moreLikeThis`

URL-Codierung wird nur auf die oben angegebenen Abfrageparameter empfohlen. Wenn Sie versehentlich URL codiert die gesamte Abfragezeichenfolge (Alles hinter der?), werden Anfragen unterbrechen.

Darüber hinaus ist URL-Codierung nur erforderlich, wenn die REST-API direkt unter Verwendung von GET aufrufen. Keine URL-Codierung ist erforderlich, beim **Suchen** , die mit Beitrag aufrufen oder bei Verwendung der [.NET Client-Bibliothek](https://msdn.microsoft.com/library/dn951165.aspx), die für Sie die URL-Codierung behandelt.

<a name="SearchQueryParameters"></a>
**Abfrageparameter**

**Suche** akzeptiert mehrere Parameter, die auch angeben Suchverhalten und Bereitstellen von Abfragekriterien. Sie angeben, dass diese Parameter in der URL bei der **Suche** über abrufen und als JSON-Eigenschaften im Hauptteil Anforderung aufrufen, beim **Suchen** per POST aufrufen Zeichenfolge abgefragt. Die Syntax für einige Parameter unterscheidet sich leicht zwischen abrufen und bereitstellen. Diese Unterschiede sind als anwendbar unter gekennzeichnet:

`search=[string]`(optional) – der zu suchenden Text. Alle `searchable` Felder werden standardmäßig durchsucht, es sei denn, `searchFields` angegeben ist. Bei der Suche `searchable` Felder, die den zu suchenden Text selbst Token, damit mehrere Ausdrücke durch ein Leerzeichen getrennt werden können (zum Beispiel: `search=hello world`). Verwenden eines Ausdrucks übereinstimmt, `*` (Dies kann sinnvoll sein für boolesche Filterabfragen). Für diesen Parameter auslassen hat die gleiche Auswirkung, indem sie auf `*`. Einzelheiten auf die Syntax der Suche finden Sie unter [Einfache Abfragesyntax](https://msdn.microsoft.com/library/dn798920.aspx) .

  - **Hinweis**: die Ergebnisse können manchmal überraschende sein, wenn Sie über Abfragen `searchable` Felder. Die Tokenizer enthält Logik zum Behandeln von allgemeinen Englisch Text wie Anführungszeichen, Kommas in Zahlen usw. an. Beispielsweise `search=123,456` entspricht eines einzelnen Ausdrucks 123,456 statt der einzelnen Ausdrücke 123 und 456, da Kommas als tausend-Trennzeichen für eine große Anzahl in Englisch verwendet werden. Aus diesem Grund, wir empfehlen verwenden Satzzeichen, statt Leerzeichen zum Trennen von Ausdrücken in der `search` Parameter.

`searchMode=any|all`(optional, wird standardmäßig `any`) –, ob eine oder alle der Suchbegriffe abgestimmt werden müssen, um das Dokument als eine Übereinstimmung zählen.

`searchFields=[string]`(optional) – der Liste der durch Trennzeichen getrennte Feldnamen So suchen Sie nach dem angegebenen Text. Zielfelder müssen markiert `searchable`.

`queryType=simple|full`(optional, wird standardmäßig `simple`) – Wenn Sie festlegen, um "einfach" Suchtext interpretiert wird mithilfe einer einfachen Abfragesprache, die Symbole wie ermöglicht +, * und "". Abfragen über alle durchsuchbare Felder ausgewertet werden (oder Felder im angegebenen `searchFields`) in jedem Dokument standardmäßig. Wenn der Abfragetyp wird festgelegt, dass `full` zu suchenden Text wird mit dem Feld-spezifische und gewichteten Suchvorgänge kann Lucene Abfragesprache interpretiert. Weitere Informationen darüber, die Syntax der Suche finden Sie unter [Einfache Abfragesyntax](https://msdn.microsoft.com/library/dn798920.aspx) und [Lucene Abfragesyntax](https://msdn.microsoft.com/library/mt589323.aspx) . 
 
> [AZURE.NOTE] Bereichs Suchen in der Lucene Abfragesprache zugunsten $filter seiner ähnliche Funktionalität, nicht unterstützt wird.

`moreLikeThis=[key]`(optional) **Wichtige:** Diese Funktion steht nur in `2015-02-28-Preview`. Diese Option kann nicht verwendet werden, in einer Abfrage, die den Text suchen-Parameter enthält `search=[string]`. Die `moreLikeThis` Parameter findet Dokumente, die das Dokument angegeben haben, indem Sie die Taste Dokument ähneln. Wenn eine Suchabfrage mit besteht `moreLikeThis`, eine Liste der Suchbegriffe basierend auf der Häufigkeit und Seltenheit Ausdrücke im Quelldokument generiert. Diese Begriffe sind dann für die Anforderung verwendet. Standardmäßig, den gesamten Inhalt des `searchable` Felder berücksichtigt werden, es sei denn, `searchFields` wird verwendet, um einzuschränken, welche Felder durchsucht werden.  

`$skip=#`(optional) – die Anzahl der Suchergebnisse zu überspringen; Darf nicht größer als 100,000 sein. Wenn Sie Dokumente in einer Sequenz scannen müssen, aber kann nicht verwendet werden `$skip` aufgrund dieser Einschränkung, bietet `$orderby` auf einem Schlüssel völlig angeordnet und `$filter` mit einem Bereich stattdessen Abfragen.

> [AZURE.NOTE] Beim **Suchen** , die mit Beitrag aufrufen, wird für diesen Parameter namens `skip` anstelle von `$skip`.

`$top=#`(optional) – die Anzahl der Suchergebnisse abzurufen. Hiermit kann in Verbindung mit `$skip` clientseitige Seitennavigation von Suchergebnissen implementiert wird.

> [AZURE.NOTE] Beim **Suchen** , die mit Beitrag aufrufen, wird für diesen Parameter namens `top` anstelle von `$top`.

`$count=true|false`(optional, wird standardmäßig `false`) – gibt an, ob die Gesamtzahl der Ergebnisse abgerufen werden sollen. Dies ist die Anzahl aller Dokumente, die entsprechen den `search` und `$filter` Parameter, ignorieren `$top` und `$skip`. Wenn dieser Wert auf `true` möglicherweise eine Auswirkung auf die Leistung haben. Beachten Sie, dass die zurückgegebene Zählung Näherung ist.

> [AZURE.NOTE] Beim **Suchen** , die mit Beitrag aufrufen, wird für diesen Parameter namens `count` anstelle von `$count`.

`$orderby=[string]`(optional) – eine Liste der durch Trennzeichen getrennte Ausdrücke zum Sortieren der Ergebnisse nach. Jeder Ausdruck werden kann, entweder einen Feldnamen oder einen Anruf an die `geo.distance()` (Funktion). Jeder Ausdruck folgen kann `asc` in aufsteigender, angegeben und `desc` in absteigender Reihenfolge anzugeben. Die Standardeinstellung ist in aufsteigender Reihenfolge. TIES werden durch den Vergleich Bewertungen der Lesbarkeit von Dokumenten umbrochen werden. Wenn keine `$orderby` angegeben ist, wird die Standardsortierreihenfolge ist absteigend nach dem Dokument Übereinstimmung Punktzahl. Es ist maximal 32 Klauseln für `$orderby`.

> [AZURE.NOTE] Beim **Suchen** , die mit Beitrag aufrufen, wird für diesen Parameter namens `orderby` anstelle von `$orderby`.

`$select=[string]`(optional) – eine Liste der durch Trennzeichen getrennte Felder zum Abrufen. Wenn nicht angegeben, werden alle Felder, die im Schema als abrufbaren markiert enthalten. Sie können alle Felder auch explizit anfordern, indem Sie für diesen Parameter auf `*`.

> [AZURE.NOTE] Beim **Suchen** , die mit Beitrag aufrufen, wird für diesen Parameter namens `select` anstelle von `$select`.

`facet=[string]`(0 oder mehr) – ein Feld Vertriebsstrategie durch. Optional die Zeichenfolge zum Anpassen der Faceting ausgedrückt als durch Trennzeichen getrennte Parameter enthalten möglicherweise `name:value` paarweise angegeben werden. Parameter sind gültig:

- `count`(maximale Anzahl Vertriebsstrategie Ausdrücke; Standard ist 10). Kein Maximum vorhanden ist, aber höhere Werte Leistungeinbußen entsprechenden, insbesondere dann, wenn das Feld facettierte eine große Anzahl von eindeutigen Ausdrücke enthält.
  - Beispiel: `facet=category,count:5` fünf Kategorien Vertriebsstrategie Ergebnisse erhält.  
  - **Hinweis**: Wenn die `count` Parameter ist kleiner als die Anzahl der eindeutigen Ausdrücke, die Ergebnisse möglicherweise nicht korrekt. Dies ist aufgrund der Ihrem Erwerb Faceting Abfragen über mehrere Shards hinweg verteilt sind. Zunehmender `count` im Allgemeinen erhöht die Genauigkeit der der Ausdruck zählt, jedoch nicht ganz umsonst Leistung.
- `sort`(eine der `count` zu zählen, indem Sie *Absteigend* sortieren `-count` zu zählen, indem Sie *Aufsteigend* sortieren `value` durch den Wert, *Aufsteigend* sortieren oder `-value` in *absteigender Reihenfolge* nach Wert sortieren)
  - Beispiel: `facet=category,count:3,sort:count` Ruft die oberen drei Kategorien in absteigender Reihenfolge nach der Anzahl von Dokumenten mit jeder Ortsname Vertriebsstrategie führt. Angenommen, wenn die ersten drei Kategorien Budget, Motels und Luxus sind, und Budget 5 Treffer hat, Motels 6 weist und Luxus 4 weist, werden klicken Sie dann die Buckets in der Reihenfolge Motels, Budget, Luxus.
  - Beispiel: `facet=rating,sort:-value` erzeugt Buckets für alle möglichen Bewertungen in absteigender Reihenfolge nach Wert. Angenommen, wenn die Bewertungen von 1 bis 5 sind, werden die Buckets sortiert 5, 4, 3, 2, 1, unabhängig davon, wie viele Dokumente jede Bewertung entsprechen.
- `values`(senkrechter Strich getrennt numerischen oder `Edm.DateTimeOffset` Werte zurück, der eine dynamische Wertemenge Vertriebsstrategie Eintrag angibt)
  - Beispiel: `facet=baseRate,values:10|20` drei Buckets erzeugt: eine für Basis Zins 0 bis zum, aber nicht 10, eine für 10 bis zu einschließlich, aber nicht einschließlich 20 und eine für 20 oder höher.
  - Beispiel: `facet=lastRenovationDate,values:2010-02-01T00:00:00Z` zwei Buckets erzeugt: eine für Hotels renovated vor Februar 2010, und eine für Hotels renovated Februar 1st, 2010 oder höher.
- `interval`(ganze Zahl Intervall größer als 0 für Zahlen oder `minute`, `hour`, `day`, `week`, `month`, `quarter`, `year` für Datum, Uhrzeit-Werten)
  - Beispiel: `facet=baseRate,interval:100` Buckets basierend auf Basis Zins Bereiche Größe 100 erzeugt. Angenommen, wenn alle zwischen 60 Euro und $600 base Sätzen sind, werden Buckets für 0-100, 100-200, 200-300, 300-400, 400-500 und 500-600.
  - Beispiel: `facet=lastRenovationDate,interval:year` erzeugt eine Zelle für jedes Jahr, wenn Hotels renovated wurden.
- `timeoffset`([+-] hh: mm, [+-] hh: mm, oder [+-] Hh) `timeoffset` ist optional. Es kann nur mit kombiniert werden die `interval` Option, und nur, wenn Sie auf ein Feld vom Typ angewendet `Edm.DateTimeOffset`. Der Wert gibt Offset der UTC in Konto in Zeit Grenzen festlegen.
  - Beispiel: `facet=lastRenovationDate,interval:day,timeoffset:-01:00` verwendet die Begrenzungslinie Tag, mit dem beginnt um 01:00:00 UTC (Mitternacht in der Zielliste Zeitzone)
- **Hinweis**: `count` und `sort` können in der gleichen Vertriebsstrategie Spezifikation kombiniert werden, aber sie können nicht mit kombiniert werden `interval` oder `values`, und `interval` und `values` kann nicht miteinander kombiniert werden.
- **Hinweis**: Intervall Facette aus Uhrzeit werden berechnet basierend auf UTC-Zeit, wenn `timeoffset` nicht angegeben ist. Beispiel: für `facet=lastRenovationDate,interval:day`, die den Tag Begrenzungslinie mit 00:00:00 UTC beginnt. 

> [AZURE.NOTE] Beim **Suchen** , die mit Beitrag aufrufen, wird für diesen Parameter namens `facets` anstelle von `facet`. Darüber hinaus geben Sie es als JSON-Array von Zeichenfolgen, wobei jede Zeichenfolge einer separaten Vertriebsstrategie Ausdruck ist.

`$filter=[string]`(optional) – eines strukturierten Suchausdruck in OData-Standardsyntax. [OData-Ausdruckssyntax](#ODataExpressionSyntax) finden Sie Details auf die Teilmenge der OData-Ausdruck Grammatik, die Azure-Suche unterstützt.

> [AZURE.NOTE] Beim **Suchen** , die mit Beitrag aufrufen, wird für diesen Parameter namens `filter` anstelle von `$filter`.

`highlight=[string]`(optional) – hervorgehoben eine Reihe von durch Trennzeichen getrennte Feldnamen für Treffer verwendet. Nur `searchable` Felder für Treffer hervorheben verwendet werden können.

`highlightPreTag=[string]`(optional, wird standardmäßig `<em>`) – eine Zeichenfolge Kategorie, die voran, um die wichtigsten Informationen zu treffen. Muss festgelegt sein, mit `highlightPostTag`.

> [AZURE.NOTE] Wenn **Suchen** , die mit abrufen, reservierte Zeichen in der URL aufrufen Prozentzeichen (z. B. % 23 anstelle von #) codierten muss werden.

`highlightPostTag=[string]`(optional, wird standardmäßig `</em>`) – eine Zeichenfolge Kategorie, die anfügt, um die wichtigsten Informationen zu treffen. Muss festgelegt sein, mit `highlightPreTag`.

> [AZURE.NOTE] Wenn **Suchen** , die mit abrufen, reservierte Zeichen in der URL aufrufen Prozentzeichen (z. B. % 23 anstelle von #) codierten muss werden.

`scoringProfile=[string]`(optional) – der Namen eines Profils zum Auswerten Punktzahl entsprechen Bewertungen der Lesbarkeit nach übereinstimmenden Dokumente akzeptieren, um die Ergebnisse zu sortieren.

`scoringParameter=[string]`(0 oder mehr) – zeigt an, die Werte für jeden Parameter in einer Funktion Punktzahl definiert (z. B. `referencePointParameter`) mit dem Format `name-value1,value2,...`.

- Angenommen, wenn das Profil Punktzahl eine Funktion mit einem Parameter mit der Bezeichnung "meinStandort" definiert die Option Abfrage Zeichenfolge wäre `&scoringParameter=mylocation--122.2,44.8`. Der erste Gedankenstrich trennt den Namen aus der Wertliste, während der zweite Gedankenstrich Teil der ersten Wert (Länge in diesem Beispiel) ist.
- Für Punktzahl Parameter wie für Kategorie verstärken, die Kommas enthalten kann können Sie eine derartige Werte in der Liste mit einfachen Anführungszeichen Escapezeichen versehen. Wenn die Werte selbst einfache Anführungszeichen enthalten, können Sie durch die Verdopplung Escapezeichen versehen.
  - Wenn Sie haben eine Kategorie Parameter mit der Bezeichnung "Mytag" verstärken und klicken Sie auf die Kategorie steigern möchten beispielsweise Werte, "Hallo, O'Brien" und "Schmitt", die Abfrage Zeichenfolgenoption wäre `&scoringParameter=mytag-'Hello, O''Brien',Smith`. Beachten Sie, dass nur Angebote werden nach Werten, die Kommas enthalten erforderlich.

> [AZURE.NOTE] Beim **Suchen** , die mit Beitrag aufrufen, wird für diesen Parameter namens `scoringParameters` anstelle von `scoringParameter`. Darüber hinaus geben Sie ihn als ein JSON-Array von Zeichenfolgen, wobei jede Zeichenfolge eine Separate ist `name-values` Paar.

`minimumCoverage`(optional, Standardeinstellungen und 100) – eine Zahl zwischen 0 und 100 die Angabe des Prozentsatzes des Indexes, der durch eine Suchabfrage, damit die Abfrage als einen Erfolg zu meldende abgedeckt werden muss. Standardmäßig muss der gesamte Index verfügbar sein oder `Search` HTTP-Statuscode 503 zurückgegeben werden kann. Wenn Sie festlegen `minimumCoverage` und `Search` erfolgreich ist, wird es zurückgeben HTTP 200 und enthalten einen `@search.coverage` Wert in der Antwort, die Angabe des Prozentsatzes des Indexes, der in die Abfrage eingeschlossen wurde.

> [AZURE.NOTE] Wenn für diesen Parameter auf einen Wert kleiner als 100 hilfreich für schnelle Suche auch für Services mit nur eine Replikation. Jedoch sind nicht alle übereinstimmenden Dokumente unbedingt in den Suchergebnissen vorhanden sein. Suche Rückruf wird an Ihrer Anwendung wichtiger als die Verfügbarkeit, und es empfiehlt sich, lassen Sie `minimumCoverage` der Standardwert von 100.

`api-version=[string]`(erforderlich). Die Preview-Version ist `api-version=2015-02-28-Preview`. Details und alternativen Versionen finden Sie unter [Suchen Dienst Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) .

Hinweis: für diesen Vorgang der `api-version` als Abfrageparameter in der URL unabhängig davon, ob Sie die **Suche** mit GET oder POST auf angegeben ist.

**Anfordern von Kopfzeilen**

Die folgende Liste beschreibt die erforderlichen und optionalen Anforderung Überschriften.

- `api-key`: Die `api-key` wird verwendet, um die Anfrage Suchdienst authentifizieren. Es ist ein Zeichenfolgenwert mit Ihrem Dienst-URL aus. **Suchabfrage** angeben kann, entweder ein Administrator oder Abfrage Taste für `api-key`.

Sie benötigen außerdem den Dienstnamen, um die Anfrage-URL zu erstellen. Sie können den Dienstnamen erhalten und `api-key` aus Ihrem Dienst Dashboard im Portal Azure. Finden Sie unter [Erstellen einer Azure Suchdienst im Portal](search-create-service-portal.md) für die Seitennavigation helfen.

**Anforderungstexts**

Für GET: keine.

Für Beitrag:

    {
      "count": true | false (default),
      "facets": [ "facet_expression_1", "facet_expression_2", ... ],
      "filter": "odata_filter_expression",
      "highlight": "highlight_field_1, highlight_field_2, ...",
      "highlightPreTag": "pre_tag",
      "highlightPostTag": "post_tag",
      "minimumCoverage": # (% of index that must be covered to declare query successful; default 100),
      "moreLikeThis": "document_key" (mutually exclusive with "search" parameter),
      "orderby": "orderby_expression",
      "scoringParameters": [ "scoring_parameter_1", "scoring_parameter_2", ... ],
      "scoringProfile": "scoring_profile_name",
      "search": "simple_query_expression",
      "searchFields": "field_name_1, field_name_2, ...",
      "searchMode": "any" (default) | "all",
      "select": "field_name_1, field_name_2, ...",
      "skip": # (default 0),
      "top": #
    }

**Fortsetzung des teilweise Suche Antworten**

Manchmal kann nicht Azure Suche alle angeforderten Ergebnisse in einer einzelnen Suche Antwort zurückgegeben. Dies kann geschehen, z. B., wenn die Abfrage zu viele Dokumente anfordert, indem Sie nicht angeben Gründen `$top` oder Angeben eines Werts für `$top` , die zu groß ist. In diesem Fall Azure-Suche schließt die `@odata.nextLink` Anmerkungen in den Textbereich der Antwort und auch `@search.nextPageParameters` , wenn sie eine POST-Anforderung wurde. Die Werte der folgenden Anmerkungen können eine andere Suchabfrage abzurufenden den nächsten Teil der Suche Antwort zu erstellen. Dies ist eine ***Fortsetzung*** der ursprünglichen Suchabfrage bezeichnet, und die Anmerkungen werden im allgemeinen ***Fortsetzung Token***bezeichnet. Siehe [Beispiel unten](#SearchResponse) für Details zur Syntax von diesen Anmerkungen und die Stelle, an der sie in den Textkörper der Antwort angezeigt werden. 

Die Gründe, warum Azure-Suche Fortsetzung Token zurück möglicherweise, sind implementierungsspezifischen und Ankündigung geändert. Robuste Clients sollten immer bereit zum Behandeln von Fällen, wo weniger Dokumente als erwartet zurückgegeben werden und ein Fortsetzungstoken fortsetzen Abrufen von Dokumenten in enthalten ist. Beachten Sie auch, dass Sie die gleiche HTTP-Methode wie die ursprüngliche Anforderung verwenden müssen, um den Vorgang fortzusetzen. Beispielsweise, wenn Sie eine GET-Anforderung gesendet hat, alle Fortsetzung Besprechungsanfragen, die Sie senden müssen auch verwenden GET (und dasselbe gilt für Beitrag).

<a name="SearchResponse"></a>
**Antwort**

: Status 200 ist OK für eine erfolgreiche Antwort ausgegeben.

    {
      "@odata.count": # (if $count=true was provided in the query),
      "@search.coverage": # (if minimumCoverage was provided in the query),
      "@search.facets": { (if faceting was specified in the query)
        "facet_field": [
          {
            "value": facet_entry_value (for non-range facets),
            "from": facet_entry_value (for range facets),
            "to": facet_entry_value (for range facets),
            "count": number_of_documents
          }
        ],
        ...
      },
      "@search.nextPageParameters": { (request body to fetch the next page of results if not all results could be returned in this response and Search was called with POST)
        "count": ... (value from request body if present),
        "facets": ... (value from request body if present),
        "filter": ... (value from request body if present),
        "highlight": ... (value from request body if present),
        "highlightPreTag": ... (value from request body if present),
        "highlightPostTag": ... (value from request body if present),
        "minimumCoverage": ... (value from request body if present),
        "moreLikeThis": ... (value from request body if present),
        "orderby": ... (value from request body if present),
        "scoringParameters": ... (value from request body if present),
        "scoringProfile": ... (value from request body if present),
        "search": ... (value from request body if present),
        "searchFields": ... (value from request body if present),
        "searchMode": ... (value from request body if present),
        "select": ... (value from request body if present),
        "skip": ... (page size plus value from request body if present),
        "top": ... (value from request body if present minus page size),
      },
      "value": [
        {
          "@search.score": document_score (if a text query was provided),
          "@search.highlights": {
            field_name: [ subset of text, ... ],
            ...
          },
          key_field_name: document_key,
          field_name: field_value (retrievable fields or specified projection),
          ...
        },
        ...
      ],
      "@odata.nextLink": (URL to fetch the next page of results if not all results could be returned in this response; Applies to both GET and POST)
    }

**Beispiele:**

Weitere Beispiele finden Sie auf der Seite [OData Ausdruckssyntax für Azure suchen](https://msdn.microsoft.com/library/azure/dn798921.aspx) .

1)  Suchen Sie den Index in absteigender Reihenfolge nach Datum sortiert.


    GET-/indexes/hotels/docs? Suchen = * & $orderby = LastRenovationDate Desc & api-Version = 2015-02-28-Vorschau

    Beitrag /indexes/hotels/docs/search? api-Version 2015-02-28-Vorschau = {"Suchen": "*", "Orderby": "LastRenovationDate Desc"}

2)  Finden Sie in einem facettierten suchen im Index und rufen Sie Aspekte für Kategorien, Bewertung, Kategorien, als auch Elemente mit BaseRate in bestimmten Bereichen ab:


    GET-/indexes/hotels/docs? suchen = Test & Vertriebsstrategie = Kategorie & Vertriebsstrategie = Bewertung & Vertriebsstrategie = Kategorien & Vertriebsstrategie = BaseRate, Werte: 80 | 150 | 220 und api-Version = 2015-02-28-Vorschau

    Beitrag /indexes/hotels/docs/search? api-Version 2015-02-28-Vorschau = {"Suchen": "test", "Aspekte": ["Kategorie", "Bewertung", "Kategorien", "BaseRate, Werte: 80 | 150 | 220"]}

3)  Mithilfe eines Filters, der vorherigen facettierten Abfrageergebnisse einschränken, nachdem der Benutzer auf klickt Bewertung 3 "und" Kategorie "Motels":


    GET-/indexes/hotels/docs? suchen = Test & Vertriebsstrategie = Kategorien & Vertriebsstrategie = BaseRate, Werte: 80 | 150 | 220 & $filter = Bewertung Eq 3 und Kategorie Eq 'Motels' und api-Version = 2015-02-28-Vorschau

    Beitrag /indexes/hotels/docs/search? api-Version 2015-02-28-Vorschau = {"Suchen": "test", "Aspekte": ["Kategorien", "BaseRate, Werte: 80 | 150 | 220"], "Filter": "Bewertung Eq 3 und Category Eq 'Motels'"}

4) Legen Sie eine Obergrenze einer facettierten Suche auf eindeutige Ausdrücken in einer Abfrage zurückgegeben. Die Standardeinstellung ist 10, aber Sie können vergrößern oder verkleinern Sie diesen Wert mit dem `count` Parameter auf die `facet` Attribut:


    GET-/indexes/hotels/docs? suchen = Test & Vertriebsstrategie = Ort, zählen: 5 und api-Version = 2015-02-28-Vorschau

    Beitrag /indexes/hotels/docs/search? api-Version 2015-02-28-Vorschau = {"Suchen": "test", "Aspekte": ["Ort, zählen: 5"]}

5)  Suchen Sie den Index in bestimmte Felder; Beispiel eines Felds sprachspezifischen:


    GET-/indexes/hotels/docs? suchen = Hôtel & SearchFields = Description_fr & api-Version = 2015-02-28-Vorschau

    Beitrag /indexes/hotels/docs/search? api-Version 2015-02-28-Vorschau = {"Suchen": "Hôtel", "SearchFields": "Description_fr"}

6) Suchen Sie den Index in mehreren Feldern. Sie können beispielsweise speichern und Abfragen durchsuchbare Felder in mehreren Sprachen, alle innerhalb der gleichen Index.  Englisch und Französisch Beschreibungen gemeinsame im selben Dokument vorhanden ist, können einige oder alle in den Abfrageergebnissen zurückgegeben werden:


    GET-/indexes/hotels/docs? suchen = Hotel & SearchFields = Beschreibung, Description_fr und api-Version = 2015-02-28-Vorschau

    Beitrag /indexes/hotels/docs/search? api-Version 2015-02-28-Vorschau = {"Suchen": "Hotel", "SearchFields": "Beschreibung, Description_fr"}

Beachten Sie, dass Sie jeweils nur einen Index Abfragen können. Erstellen Sie mehrere Indizes für die einzelnen Sprachen nicht, es sei denn, Sie beabsichtigen, einzeln Abfragen.

7)  Seitennavigation - Abrufen der 1st Seite von Elementen (Seitenformat ist 10):


    GET-/indexes/hotels/docs? Suchen = * & $skip = 0 & $top = 10 & api-Version = 2015-02-28-Vorschau

    Beitrag /indexes/hotels/docs/search? api-Version 2015-02-28-Vorschau = {"Suchen": "*", "Überspringen": "Top" 0: 10}

8)  Seitennavigation - Abrufen der 2. Seite von Elementen (Seitenformat ist 10):


    GET-/indexes/hotels/docs? Suchen = * & $skip = 10 und $top = 10 & api-Version = 2015-02-28-Vorschau

    Beitrag /indexes/hotels/docs/search? api-Version 2015-02-28-Vorschau = {"Suchen": "*", "Überspringen": 10, "Top": 10}

9)  Abrufen einer bestimmten Folge von Feldern an:


    GET-/indexes/hotels/docs? Suchen = * & $select = HotelName, Beschreibung und api-Version = 2015-02-28-Vorschau

    Beitrag /indexes/hotels/docs/search? api-Version 2015-02-28-Vorschau = {"Suchen": "*", "aktivieren": "HotelName Beschreibung"}

10)  Abrufen von Dokumenten, die einen bestimmten Filterausdruck entsprechen:


    GET-/indexes/hotels/docs? $filter = (BaseRate Seite 60 und BaseRate Lt 300) oder HotelName Eq 'Ausgefallener bleiben' und api-Version = 2015-02-28-Vorschau

    Beitrag /indexes/hotels/docs/search? api-Version 2015-02-28-Vorschau = {"Filter": "(BaseRate Seite 60 und BaseRate Lt 300) oder HotelName Eq 'Ausgefallener greifen'"}

11) Suchen Sie die Index und Eingabe Fragmenten mit Treffer Hervorhebungen


    GET-/indexes/hotels/docs? suchen etwas = & Hervorheben = Beschreibung & api-Version = 2015-02-28-Vorschau

    Beitrag /indexes/hotels/docs/search? api-Version 2015-02-28-Vorschau = {"Suchen": "etwas", "Hervorheben": "Beschreibung"}

12) Suchen Sie den Index und Dokumente von näher an Entfernung von einen Verweis Speicherort sortiert zurück


    GET-/indexes/hotels/docs? suchen etwas = & $orderby=geo.distance (Speicherort geography'POINT(-122.12315 47.88121)') & api-Version = 2015-02-28-Vorschau

    Beitrag /indexes/hotels/docs/search? api-Version 2015-02-28-Vorschau = {"Suchen": "etwas", "Orderby": "geo.distance (Speicherort geography'POINT(-122.12315 47.88121)')"}

13) Suchen den Index, vorausgesetzt, es gibt ein Bewertungssystem Profil namens "Geo" mit zwei Abstand Punktzahl Funktionen, eine Definieren eines Parameters namens "CurrentLocation" und eine definieren einen Parameter namens "LastLocation"


    GET-/indexes/hotels/docs? suchen etwas = & ScoringProfile = Geo & ScoringParameter = CurrentLocation – 122.123,44.77233 & ScoringParameter = LastLocation – 121.499,44.2113 und api-Version = 2015-02-28-Vorschau

    Beitrag /indexes/hotels/docs/search? api-Version 2015-02-28-Vorschau = {"Suchen": "etwas", "ScoringProfile": "Geo", "ScoringParameters": ["CurrentLocation – 122.123,44.77233", "LastLocation – 121.499,44.2113"]}

14) Finden von Dokumenten im Index [einfache Abfragesyntax](https://msdn.microsoft.com/library/dn798920.aspx)verwenden. Diese Abfrage gibt Hotels, wo durchsuchbare Felder die Begriffe "Komfort" und "Speicherort" aber nicht "Motels" enthalten:


    GET-/indexes/hotels/docs? suchen = Komfort + Speicherort-Motels & SearchMode = all & api-Version = 2015-02-28-Vorschau

    Beitrag /indexes/hotels/docs/search? api-Version 2015-02-28-Vorschau = {"Suchen": "Komfort + Speicherort-Motels", "SearchMode": "alle"}

Beachten Sie die Verwendung der `searchMode=all` über. Für diesen Parameter einschließlich überschreibt die standardmäßige von `searchMode=any`, Gewährleistung, `-motel` "Und nicht" statt "Oder nicht" bedeutet. Ohne `searchMode=all`"Oder nicht" die erweitert, statt Suchergebnisse beschränkt werden, und dies kann für einige Benutzer Counter-intuitiven sein.

15) Finden von Dokumenten im Index [Lucene Abfragesyntax](https://msdn.microsoft.com/library/mt589323.aspx)verwenden. Diese Abfrage gibt Hotels, wo das Kategoriefeld den Begriff "Budget" enthält, und alle durchsuchbare Felder, die den Ausdruck "zuletzt renovated" enthält. Dokumente, die mit "zuletzt renovated" Wortlaut werden als Ergebnis der Begriff steigern Value (3) höher eingestuft.

    GET-/indexes/hotels/docs?search = Kategorie: Budget und \"zuletzt renovated\"^ 3 & SearchMode = all & api-Version 2015-02-28-Vorschau = & Querytype = vollständig

    Beitrag /indexes/hotels/docs/search?api-version 2015-02-28-Vorschau = {"Suchen": "Kategorie: Budget und \"zuletzt renovated\"^ 3", "QueryType": "full", "SearchMode": "alle"}

<a name="LookupAPI"></a>
## <a name="lookup-document"></a>Nachschlage-Dokument

Der **Nachschlage-Dokument** Vorgang ruft ein Dokuments von Azure suchen. Dies ist nützlich, wenn ein Benutzer auf ein bestimmtes Suchergebnis klickt, und Sie bestimmte Details zu diesem Dokument nachschlagen möchten.

    GET https://[service name].search.windows.net/indexes/[index name]/docs/[key]?[query parameters]
    api-key: [admin or query key]

**Anfordern**

HTTPS ist für Serviceanfragen erforderlich. Die Anfrage **Nachschlagen Dokument** kann wie folgt erstellt werden.

    GET /indexes/[index name]/docs/key?[query parameters]

Alternativ können Sie die herkömmliche OData-Syntax für Key nachschlagen:

    GET /indexes('[index name]')/docs('[key]')?[query parameters]

Die Anforderung URI enthält ein [Indexname] und [Key], welches Dokument zum Abrufen aus welcher Index angeben. Sie können nur jeweils ein Dokument erhalten. Verwenden Sie die **Suche** , um mehrere Dokumente in einer einzigen Anforderung zu gelangen.

**Abfrageparameter**

`$select=[string]`(optional) – eine Liste der durch Trennzeichen getrennte Felder zum Abrufen. Wenn nicht angegeben, oder legen Sie auf `*`, alle Felder, die im Schema als abrufbaren markiert sind in der Projektion enthalten.

`api-version=[string]`(erforderlich). Die Preview-Version ist `api-version=2015-02-28-Preview`. Details und alternativen Versionen finden Sie unter [Suchen Dienst Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) .

Hinweis: für diesen Vorgang der `api-version` als Abfrageparameter angegeben ist.

**Anfordern von Kopfzeilen**

Die folgende Liste beschreibt die erforderlichen und optionalen Anforderung Überschriften.

- `api-key`: Die `api-key` wird verwendet, um die Anfrage Suchdienst authentifizieren. Es ist ein Zeichenfolgenwert mit Ihrem Dienst-URL aus. Die Anfrage **Nachschlage-Dokument** kann angeben, entweder ein Administrator oder Abfrage Taste für `api-key`.

Sie benötigen außerdem den Dienstnamen, um die Anfrage-URL zu erstellen. Sie können den Dienstnamen erhalten und `api-key` aus Ihrem Dienst Dashboard im Portal Azure. Finden Sie unter [Erstellen einer Azure Suchdienst im Portal](search-create-service-portal.md) für die Seitennavigation helfen.

**Anforderungstexts**

Keine.

**Antwort**

: Status 200 ist OK für eine erfolgreiche Antwort ausgegeben.

    {
      field_name: field_value (fields matching the default or specified projection)
    }

**Beispiel**

Nachschlagen des Dokuments, das Schlüssel "2" hat

    GET /indexes/hotels/docs/2?api-version=2015-02-28-Preview

Nachschlagen des Dokuments, das Schlüssel '3' mit OData-Syntax hat:

    GET /indexes('hotels')/docs('3')?api-version=2015-02-28-Preview

<a name="CountDocs"></a>
## <a name="count-documents"></a>Anzahl von Dokumenten

Der **Anzahl von Dokumenten** -Vorgang ruft die Anzahl von Dokumenten in einem Suchindex ab. Die `$count` Syntax ist Bestandteil der OData-Protokoll.

    GET https://[service name].search.windows.net/indexes/[index name]/docs/$count?api-version=[api-version]
    Accept: text/plain
    api-key: [admin or query key]

**Anfordern**

HTTPS ist für Serviceanfragen erforderlich. Die **Anzahl von Dokumenten** Anforderung kann mithilfe der GET-Methode erstellt werden.

[Indexname] in der Besprechungsanfrage URI wird den Dienst Anzahl aller Elemente in der Dokumente-Auflistung des angegebenen Index zurückgibt.

`api-version=[string]`(erforderlich). Die Preview-Version ist `api-version=2015-02-28-Preview`. Details und alternativen Versionen finden Sie unter [Suchen Dienst Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) .

**Anfordern von Kopfzeilen**

Die folgende Liste beschreibt die erforderlichen und optionalen Anforderung Überschriften.

- `Accept`: Dieser Wert muss auf festgelegt sein `text/plain`.
- `api-key`: Die `api-key` wird verwendet, um die Anfrage Suchdienst authentifizieren. Es ist ein Zeichenfolgenwert mit Ihrem Dienst-URL aus. Die **Anzahl von Dokumenten** Anforderung kann entweder ein Administrator oder Abfrage Taste für angeben `api-key`.

Sie benötigen außerdem den Dienstnamen, um die Anfrage-URL zu erstellen. Sie können den Dienstnamen erhalten und `api-key` aus Ihrem Dienst Dashboard im Portal Azure. Finden Sie unter [Erstellen einer Azure Suchdienst im Portal](search-create-service-portal.md) für die Seitennavigation helfen.

**Anforderungstexts**

Keine.

**Antwort**

: Status 200 ist OK für eine erfolgreiche Antwort ausgegeben.

Der Nachrichtentext der Antwort enthält den Count-Wert als ganze Zahl im nur-Text formatiert.

<a name="Suggestions"></a>
## <a name="suggestions"></a>Vorschläge

Der Vorgang **Vorschläge** Ruft Vorschläge basierend auf teilweise Sucheingabe. In der Regel wird in den Feldern suchen Hiermit während der Eingabe Vorschläge angeben, wie Benutzer Suchbegriffe eingeben.

Ziel der Vorschlag Anfragen Ziel Dokumente vorgeschlagen wird, damit die vorgeschlagene Text wiederholt werden kann, wenn mehrere Candidate Dokumente die gleiche Suche Eingabemethoden entsprechen. Sie können `$select` zum Abrufen von anderen Dokumentfelder (einschließlich der Dokument-Taste), damit Sie erkennen können, welches Dokument die Quelle für jeden einzelnen Vorschlag ist.

Ein Vorgang **Vorschläge** wird als eine Anforderung GET oder POST ausgegeben.

    GET https://[service name].search.windows.net/indexes/[index name]/docs/suggest?[query parameters]
    api-key: [admin or query key]

    POST https://[service name].search.windows.net/indexes/[index name]/docs/suggest?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin or query key]

**Verwenden von Beitrag statt abrufen**

Wenn Sie HTTP GET verwenden die **Vorschläge** API aufrufen, müssen Sie bewusst sein, dass die Länge der URL der Anforderung 8 KB nicht überschreiten kann. Dies ist normalerweise genug für die meisten Applikationen. Einige Applikationen wird jedoch sehr große Abfragen, insbesondere OData Filterausdrücke erzeugt. In diesem Bereich ist die unter Verwendung von HTTP POST besser geeignet, da größere Filter als abrufen können. Mit bereitstellen wird die Anzahl der Klauseln in einem Filter die Beschränkung nicht die Größe der Filterzeichenfolge unformatierten, da das Größenlimit Anfrage für Beitrag ungefähr 16 MB ist.

> [AZURE.NOTE] Obwohl das Größenlimit der Beitrag Anforderung sehr groß ist, können nicht Filterausdrücke beliebig komplex werden. Weitere Informationen zum Filtern Komplexität Einschränkungen finden Sie unter [OData Ausdruckssyntax](https://msdn.microsoft.com/library/dn798921.aspx) .

**Anfordern**

HTTPS ist für Serviceanfragen erforderlich. Die Anfrage **Vorschläge** kann mithilfe der Methoden GET oder POST erstellt werden.

Die Anfrage-URI Gibt den Namen des Indexes Abfrage an. Parameter, wie etwa die teilweise Eingabewerte Suchbegriff in der Abfragezeichenfolge bei GET-Anfragen angegeben sind, und in der Besprechungsanfrage Stelle in Beitrag anfordert.

Als bewährte Methode GET-Anfragen erstellen Denken Sie daran, [URL codieren](https://msdn.microsoft.com/library/system.uri.escapedatastring.aspx) von bestimmter Abfrageparameter, wenn die REST-API direkt aufrufen. **Vorschläge** Vorgänge sind dies:

- `$filter`
- `highlightPreTag`
- `highlightPostTag`
- `search`

URL-Codierung wird nur auf die oben angegebenen Abfrageparameter empfohlen. Wenn Sie versehentlich URL codiert die gesamte Abfragezeichenfolge (Alles hinter der?), werden Anfragen unterbrechen.

Darüber hinaus ist URL-Codierung nur erforderlich, wenn die REST-API direkt unter Verwendung von GET aufrufen. Keine URL-Codierung ist erforderlich, vor, wenn **Vorschläge** unter Verwendung von POST aufrufen, oder bei Verwendung der [.NET Client-Bibliothek](https://msdn.microsoft.com/library/dn951165.aspx), die für Sie die URL-Codierung behandelt.

**Abfrageparameter**

**Vorschläge** akzeptiert mehrere Parameter, die auch angeben Suchverhalten und Bereitstellen von Abfragekriterien. Sie angeben, dass diese Parameter in der URL Zeichenfolge Abfragen, wenn Sie **Vorschläge** über abrufen und als JSON-Eigenschaften im Hauptteil Anforderung aufrufen, wenn Sie **Vorschläge** per POST aufrufen. Die Syntax für einige Parameter unterscheidet sich leicht zwischen abrufen und bereitstellen. Diese Unterschiede sind als anwendbar unter gekennzeichnet:

`search=[string]`-den zu suchenden Text zu verwenden, um Abfragen vorschlagen. Mindestens 1 Zeichen und nicht mehr als 100 Zeichen muss sein.

`highlightPreTag=[string]`(optional) – eine Zeichenfolge markieren, die zum Suchen von Treffer voran. Muss festgelegt sein, mit `highlightPostTag`.

> [AZURE.NOTE] Beim Aufrufen **Vorschläge** abrufen, reservierte Zeichen in der URL mit Prozentzeichen (z. B. % 23 anstelle von #) codierten muss werden.

`highlightPostTag=[string]`eine Zeichenfolge (optional) – zu kennzeichnen, fügt um Treffer zu suchen. Muss festgelegt sein, mit `highlightPreTag`.

> [AZURE.NOTE] Beim Aufrufen **Vorschläge** abrufen, reservierte Zeichen in der URL mit Prozentzeichen (z. B. % 23 anstelle von #) codierten muss werden.

`suggesterName=[string]`– den Namen der Suggester gemäß Angabe in den `suggesters` Websitesammlung, die Teil der Indexdefinition ist. A `suggester` bestimmt, welche Felder für die Abfrage vorgeschlagenen Begriffe gescannt werden. Details finden Sie unter [Suggesters](#Suggesters) .

`fuzzy=[boolean]`(optional, Standardeinstellung = False) – beim Festlegen dieser API true Vorschläge findet, auch wenn es eine ersetzte oder fehlende Zeichen in den zu suchenden Text ist. Während dieses optimal in einigen Fällen bietet kommt es bei einer Leistung Kosten wie fuzzy Vorschlag Suchbegriffe langsamer sind und weitere Ressourcen nutzen.

`searchFields=[string]`(optional) – der Liste der durch Trennzeichen getrennte Feldnamen So suchen Sie nach dem angegebenen Suchtext. Zielfelder müssen Vorschläge aktiviert sein.

`$top=#`(optional, Standardeinstellung = 5)-die Anzahl der Vorschläge abzurufen. Sie müssen eine Zahl zwischen 1 und 100.

> [AZURE.NOTE] Wenn Sie **Vorschläge** unter Verwendung von POST aufrufen, wird für diesen Parameter namens `top` anstelle von `$top`.

`$filter=[string]`(optional) – ein Ausdruck, der die Dokumente Filter für Vorschläge berücksichtigt.

> [AZURE.NOTE] Wenn Sie **Vorschläge** unter Verwendung von POST aufrufen, wird für diesen Parameter namens `filter` anstelle von `$filter`.

`$orderby=[string]`(optional) – eine Liste der durch Trennzeichen getrennte Ausdrücke zum Sortieren der Ergebnisse nach. Jeder Ausdruck werden kann, entweder einen Feldnamen oder einen Anruf an die `geo.distance()` (Funktion). Jeder Ausdruck folgen kann `asc` in aufsteigender, angegeben und `desc` in absteigender Reihenfolge anzugeben. Die Standardeinstellung ist in aufsteigender Reihenfolge. Es ist maximal 32 Klauseln für `$orderby`.

> [AZURE.NOTE] Wenn Sie **Vorschläge** unter Verwendung von POST aufrufen, wird für diesen Parameter namens `orderby` anstelle von `$orderby`.

`$select=[string]`(optional) – eine Liste der durch Trennzeichen getrennte Felder zum Abrufen. Wenn nicht angegeben, wird nur der Schlüssel und Vorschläge zur Behebung Dokumenttext zurückgegeben. Sie können alle Felder explizit anfordern, indem Sie für diesen Parameter auf `*`.

> [AZURE.NOTE] Wenn Sie **Vorschläge** unter Verwendung von POST aufrufen, wird für diesen Parameter namens `select` anstelle von `$select`.

`minimumCoverage`(optional, standardmäßig 80) – eine Zahl zwischen 0 und 100 die Angabe des Prozentsatzes des Indexes, die von einer Abfrage Vorschläge, damit die Abfrage als einen Erfolg zu meldende abgedeckt werden muss. Standardmäßig muss mindestens 80 % des Indexes verfügbar sein oder `Suggest` HTTP-Statuscode 503 zurückgegeben werden kann. Wenn Sie festlegen `minimumCoverage` und `Suggest` erfolgreich ist, wird es zurückgeben HTTP 200 und enthalten einen `@search.coverage` Wert in der Antwort, die Angabe des Prozentsatzes des Indexes, der in die Abfrage eingeschlossen wurde.

> [AZURE.NOTE] Wenn für diesen Parameter auf einen Wert kleiner als 100 hilfreich für schnelle Suche auch für Services mit nur eine Replikation. Jedoch sind nicht alle übereinstimmenden Vorschläge unbedingt in den Ergebnissen vorhanden sein. Rückruf wird an Ihrer Anwendung wichtiger als die Verfügbarkeit, und es empfiehlt sich nicht nach unten `minimumCoverage` unterhalb der Standardwert 80.

`api-version=[string]`(erforderlich). Die Preview-Version ist `api-version=2015-02-28-Preview`. Details und alternativen Versionen finden Sie unter [Suchen Dienst Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) .

Hinweis: für diesen Vorgang der `api-version` als Abfrageparameter in der URL unabhängig davon, ob Sie **Vorschläge** mit GET oder POST aufrufen angegeben ist.

**Anfordern von Kopfzeilen**

Die folgende Liste beschreibt die erforderlichen und optionalen Anforderung Kopfzeilen

- `api-key`: Die `api-key` wird verwendet, um die Anfrage Suchdienst authentifizieren. Es ist ein Zeichenfolgenwert mit Ihrem Dienst-URL aus. Die Anfrage **Vorschläge** kann entweder ein Administrator oder Abfrage Taste als angeben der `api-key`.

Sie benötigen außerdem den Dienstnamen, um die Anfrage-URL zu erstellen. Sie können den Dienstnamen erhalten und `api-key` aus Ihrem Dienst Dashboard im Portal Azure. Finden Sie unter [Erstellen einer Azure Suchdienst im Portal](search-create-service-portal.md) für die Seitennavigation helfen.

**Anforderungstexts**

Für GET: keine.

Für Beitrag:

    {
      "filter": "odata_filter_expression",
      "fuzzy": true | false (default),
      "highlightPreTag": "pre_tag",
      "highlightPostTag": "post_tag",
      "minimumCoverage": # (% of index that must be covered to declare query successful; default 80),
      "orderby": "orderby_expression",
      "search": "partial_search_input",
      "searchFields": "field_name_1, field_name_2, ...",
      "select": "field_name_1, field_name_2, ...",
      "suggesterName": "suggester_name",
      "top": # (default 5)
    }

**Antwort**

: Status 200 ist OK für eine erfolgreiche Antwort ausgegeben.

    {
      "@search.coverage": # (if minimumCoverage was provided in the query),
      "value": [
        {
          "@search.text": "...",
          "<key field>": "..."
        },
        ...
      ]
    }

Wenn die Option Projektion verwendet wird, zum Abrufen von Feldern, die sie in jedem Element des Arrays enthalten sind:

    {
      "@search.coverage": # (if minimumCoverage was provided in the query),
      "value": [
        {
          "@search.text": "...",
          "<key field>": "..."
          <other projected data fields>
        },
        ...
      ]
    }

**Beispiel**

Abrufen von 5 Vorschläge, wo finde ich die teilweise Sucheingabe 'Lux'

    GET /indexes/hotels/docs/suggest?search=lux&$top=5&suggesterName=sg&api-version=2015-02-28-Preview

    POST /indexes/hotels/docs/suggest?api-version=2015-02-28-Preview
    {
      "search": "lux",
      "top": 5,
      "suggesterName": "sg"
    }

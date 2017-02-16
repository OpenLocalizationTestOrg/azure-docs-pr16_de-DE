<properties
    pageTitle="Bewerten Profile (Azure REST API-Version 2015-02-28-Vorschau für Suche) | Microsoft Azure | Vorschau der Azure Suche API"
    description="Azure Suche ist ein Cloud gehosteten Suchdienst, der Abstimmung von bewertete Ergebnisse basierend auf User defined Punktzahl Profile unterstützt."
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="search"
    ms.devlang="rest-api"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.author="heidist"
    ms.date="08/29/2016" />

# <a name="scoring-profiles-azure-search-rest-api-version-2015-02-28-preview"></a>Punktzahl Profile (Azure REST API-Version 2015-02-28-Vorschau für Suche)

> [AZURE.NOTE] In diesem Artikel werden die Punktzahl Profile in der [2015-02-28-Vorschau](search-api-2015-02-28-preview.md). Aktuell besteht kein Unterschied zwischen der `2015-02-28` auf [MSDN](http://msdn.microsoft.com/library/azure/mt183328.aspx) dokumentierten Version und die `2015-02-28-Preview` Version hier beschriebenen, aber wir bieten dieses Dokument trotzdem, um das Dokument Konnektivität über die gesamte-API zu gewährleisten.

## <a name="overview"></a>(Übersicht)

Bewerten verweist auf die Berechnung der eine Suche Bewertung für jedes Element in den Suchergebnissen zurückgegeben. Die Punktzahl ist ein Indikator des Elements Relevanz im Zusammenhang mit den aktuellen Suchvorgang an. Je höher der Wert, desto relevanter des Elements. In den Suchergebnissen werden Elemente Rang geordnet von hoch zu niedrig, basierend auf der Suche Punktzahl für jedes Element berechnet.

Azure Suchen verwendet standardmäßig zum Berechnen einer anfänglichen Bewertung bewerten, aber Sie können die Berechnung durch ein Profil Punktzahl anpassen. Punktzahl Profile bieten Ihnen eine bessere Kontrolle über die Rangfolge der Elemente in den Suchergebnissen angezeigt. Sie möchten beispielsweise Elemente basierend auf deren potenzieller Umsatz steigern, heraufstufen neuere Elemente oder zum Steigern der Elemente, die im Lager zu lang wurden.

Ein Bewertungssystem Profil ist Teil der Indexdefinition erstellter Felder, Funktionen und Parameter.

Um eine Vorstellung davon, wie ein Profil Punktzahl aussieht erhalten, zeigt im folgenden Beispiel wird eine einfache Profil mit dem Namen 'Geo'. Diesen Termin steigert die Elemente, die den Suchbegriff, in enthalten der `hotelName` Feld. Außerdem wird mit der `distance` (Funktion), Elemente, die innerhalb des aktuellen Speicherorts zehn Kilometer sind zu bevorzugen. Wenn jemand Ausdruckssatz 'Inn durchsucht' und 'Inn' geschieht Teil des Namens Hotel sein, werden Dokumente, die mit 'Inn' Hotels enthalten höhere in den Suchergebnissen angezeigt.

    "scoringProfiles": [
      {
        "name": "geo",
        "text": {
          "weights": { "hotelName": 5 }
        },
        "functions": [
          {
            "type": "distance",
            "boost": 5,
            "fieldName": "location",
            "interpolation": "logarithmic",
            "distance": {
              "referencePointParameter": "currentLocation",
              "boostingDistance": 10
            }
          }
        ]
      }
    ]

Wenn dieses Bewertungssystem Profil verwenden möchten, ist Ihre Abfrage formuliert, um das Profil in der Abfragezeichenfolge angeben. Beachten Sie in der folgenden Abfrage ist den Abfrageparameter `scoringProfile=geo` in der Besprechungsanfrage.

    GET /indexes/hotels/docs?search=inn&scoringProfile=geo&scoringParameter=currentLocation--122.123,44.77233&api-version=2015-02-28-Preview

Diese Abfrage durchsucht Ausdruckssatz 'Inn' und der aktuellen Position übergibt. Beachten Sie, dass diese Abfrage andere Parameter, z. B. enthält `scoringParameter`. Abfrageparameter werden in [Dokumente durchsuchen (Azure Suche API)](search-api-2015-02-28-preview.md#SearchDocs)beschrieben.

Klicken Sie auf [Beispiel](#example) , um eine ausführlichere Beispiel eines Profils Bewertungssystem zu überprüfen.

## <a name="what-is-default-scoring"></a>Was ist die standardmäßige bewerten?

Bewerten berechnet eine Suche Bewertung für jedes Element in einem Rang Ziffern oder Buchstaben sortierte Resultset. Jedes Element in einem Resultset suchen wird eine Suche Bewertung zugewiesen und anschließend höchsten zum niedrigsten Wert eingestuft. Elemente, wobei höhere Werte werden an die Anwendung zurückgegeben. Standardmäßig die oberen 50 zurückgegeben werden, aber Sie können die `$top` Parameter eine kleinere oder größere Anzahl von Elementen (bis zu 1000 in einer einzelnen Antwort) zurückgegeben.

Standardmäßig wird eine Suche Bewertung statistische Eigenschaften der Daten und der Abfrage berechnet. Azure Suche findet Dokumente, die die Suchbegriffe in der Abfragezeichenfolge enthalten (einige oder alle, je nach `searchMode`), vorzugsweise Dokumente, die viele Instanzen des Suchbegriffs enthalten. Die Suche Punktzahl wechselt nach oben noch höheren ist der Begriff seltene über die Daten trennen, sondern allgemeine innerhalb des Dokuments. Basis für diesen Ansatz zur computing Relevanz als TF-IDF bekannt ist oder (Ausdruck Häufigkeit-Quantil Dokument Häufigkeit).

Unter der Voraussetzung, dass es ist keine benutzerdefiniertes Sortieren, werden Ergebnisse dann von Suchen Punktzahl eingestuft, bevor sie mit der Anwendung einen zurückgegeben werden. Wenn `$top` nicht angegeben ist, 50 Artikel mit dem höchsten suchen Punktzahl zurückgegeben werden.

Suche Punktzahl Werte können in einem Resultset wiederholt werden. Angenommen, Sie müssen möglicherweise 10 Elemente mit einem Faktor von 1.2, 20 Elemente mit einem Faktor von 1,0 und 20 Elemente mit einem Faktor von 0,5. Wenn mehrere Treffer die gleiche Suche Punktzahl haben, die Sortierung der gleichen bewertete Elemente nicht definiert ist, und ist nicht zuverlässig. Die Abfrage erneut aus, und Sie möglicherweise finden Sie unter Ausführen von Elementen UMSCHALT Position. Wenn zwei Artikel mit einer identischen Bewertung, Sie besteht keine Garantie, die der ersten angezeigt wird.

## <a name="when-to-use-custom-scoring"></a>Verwenden von benutzerdefinierten bewerten

Wenn die Standardeinstellung Rangfolgen Verhalten wechseln nicht weit im Besprechung Ihren geschäftlichen Zielsetzungen, sollten Sie ein oder mehrere Punktzahl Profile erstellen. Beispielsweise empfiehlt sich, Suchrelevanz neu hinzugefügte Elemente zuerst. Ebenso müssen Sie ein Feld, das Gewinnspanne enthält, oder ein anderes Feld potenzieller Umsatz angibt. Verstärken Treffer, die Vorteile für Ihr Geschäft bringen kann ein wichtiger Faktor bei der Entscheidung für oder gegen verwenden Bewertungssystem Profile sein.

Relevanz basierenden Sortierung wird auch durch Bewerten Profile implementiert. Erwägen Sie Suchergebnisseiten, die Sie in der Vergangenheit verwendet haben, mit die Sie nach Preis, Datum, Bewertung oder Relevanz sortieren können. Azure-Suche das Laufwerk Punktzahl Profile der Option 'Relevanz'. Die Definition der Relevanz wird gesteuert, von Ihnen basiert auf geschäftliche Ziele und den Typ der Suchfunktion, die Sie vorführen möchten.

<a name="example"></a>
## <a name="example"></a>Beispiel

Wie erwähnt, wird angepassten Bewertung durch Punktzahl in einem IndexSchema definierte Profile implementiert.

Dieses Beispiel zeigt das Schema eines Indexes mit zwei Punktzahl Profile (`boostGenre`, `newAndHighlyRated`). Legen Sie eine Abfrage für diesen Index, der entweder Profil enthält, wie ein Abfrageparameter das Profil verwendet wird, um das Ergebnis zu bewerten.

[Versuchen Sie es in diesem Beispiel wird](search-get-started-scoring-profiles.md).

    {
      "name": "musicstoreindex",
      "fields": [
        { "name": "key", "type": "Edm.String", "key": true },
        { "name": "albumTitle", "type": "Edm.String" },
        { "name": "albumUrl", "type": "Edm.String", "filterable": false },
        { "name": "genre", "type": "Edm.String" },
        { "name": "genreDescription", "type": "Edm.String", "filterable": false },
        { "name": "artistName", "type": "Edm.String" },
        { "name": "orderableOnline", "type": "Edm.Boolean" },
        { "name": "rating", "type": "Edm.Int32" },
        { "name": "tags", "type": "Collection(Edm.String)" },
        { "name": "price", "type": "Edm.Double", "filterable": false },
        { "name": "margin", "type": "Edm.Int32", "retrievable": false },
        { "name": "inventory", "type": "Edm.Int32" },
        { "name": "lastUpdated", "type": "Edm.DateTimeOffset" }
      ],
      "scoringProfiles": [
        {
          "name": "boostGenre",
          "text": {
            "weights": {
              "albumTitle": 1.5,
              "genre": 5,
              "artistName": 2
            }
          }
        },
        {
          "name": "newAndHighlyRated",
          "functions": [
            {
              "type": "freshness",
              "fieldName": "lastUpdated",
              "boost": 10,
              "interpolation": "quadratic",
              "freshness": {
                "boostingDuration": "P365D"
              }
            },
            {
              "type": "magnitude",
              "fieldName": "rating",
              "boost": 10,
              "interpolation": "linear",
              "magnitude": {
                "boostingRangeStart": 1,
                "boostingRangeEnd": 5,
                "constantBoostBeyondRange": false
              }
            }
          ]
        }
      ],
      "suggesters": [
        {
          "name": "sg",
          "searchMode": "analyzingInfixMatching",
          "sourceFields": ["albumTitle", "artistName"]
        }
      ]
    }


## <a name="workflow"></a>Workflow

Um benutzerdefinierte Punktzahl Verhalten implementieren, fügen Sie ein Bewertungssystem Profil auf das Schema, das den Index definiert. Sie können mehrere Punktzahl Profile innerhalb eines Indexes haben, jedoch können Sie nur ein Profil in einer Abfrage angegebenen Zeitpunkt angeben.

Beginnen Sie mit der [Vorlage](#bkmk_template) , die in diesem Artikel bereitgestellt.

Geben Sie einen Namen ein. Der Name ist erforderlich, wenn Sie eine hinzufügen, Punktzahl Profile sind optional Halten Sie sich an den Benennungskonventionen für Felder (beginnt mit einem Buchstaben, vermieden werden Sonderzeichen und reservierte Wörter). Weitere Informationen finden Sie unter [Regeln zur Benennung von](http://msdn.microsoft.com/library/azure/dn857353.aspx) .

Hauptteil der Punktzahl Profil wird anhand gewichteten Felder und Funktionen erstellt.

### <a name="weights"></a>-Stärken ###

Die `weights` -Eigenschaft eines Profils Punktzahl gibt an, Name / Wert-Paare, die ein Feld eine relative Gewichtung zuweisen. Im [Beispiel](#example)sind die Felder AlbumTitle, Genre und ArtistName stärkere 1,5, 5 und 2. Warum ist sehr viel höher als die anderen Genre erhöht? Wenn die Suche auf Daten ausgeführt wird, die etwas einheitlichen ist (wie "Genre" "in zutrifft der `musicstoreindex`), müssen Sie eine größere Varianz der relativen Gewichtung gegebenenfalls. Angenommen, in der `musicstoreindex`, 'Felsen' angezeigt wird, als beide Genre und Beschreibungen Genre identisch formuliert haben. Wenn Sie übersteigen Genre Beschreibung hinzufügen möchten, wird das Feld Genre eine viel höhere relative Gewichtung benötigen.

### <a name="functions"></a>Funktionen ###

Funktionen werden verwendet, wenn zusätzliche Berechnungen für bestimmte Kontexte erforderlich sind. Gültige Funktionstypen sind `freshness`, `magnitude`, `distance` und `tag`. Jede Funktion hat die Parameter, die darauf eindeutig sind.

  - `freshness`sollte verwendet werden, wenn Sie durch fördern möchten ist, wie neue oder das alte ein Element. Diese Funktion kann nur mit Datetime-Felder verwendet werden (`Edm.DataTimeOffset`). Hinweis Die `boostingDuration` Attribut wird nur in Verbindung mit der Funktion Aktualität verwendet.
  - `magnitude`sollte verwendet werden, wenn durch fördern, basierend auf wie hoch soll oder niedrig einen numerischen Wert handelt. Szenarien, die für diese Funktion aufrufen einbeziehen von Gewinnspanne gesamt, höchsten Preis, niedrigste Preis oder Anzahl der Downloads verstärken. Sie können den Bereich hoch zu niedrig, umkehren, Sie bei Bedarf das umgekehrte Muster (beispielsweise zum unteren Preis steigern Elemente größer als Elemente höheren Preis). Ausgehend von einem Zellbereich Preise von 100 $ 1, legen Sie `boostingRangeStart` bei 100 und `boostingRangeEnd` bei 1 an den unteren Preis Elemente durch fördern. Diese Funktion kann nur mit den Feldern Double und ganze Zahl verwendet werden.
  - `distance`sollte verwendet werden, wenn Sie nach dem Näherung oder geographischen Standort steigern möchten. Diese Funktion kann nur verwendet werden, mit `Edm.GeographyPoint` Felder.
  - `tag`sollte verwendet werden, wenn Sie nach Kategorien gemeinsam zwischen Dokumenten und Suchabfragen steigern möchten. Diese Funktion kann nur verwendet werden, mit `Edm.String` und `Collection(Edm.String)` Felder.
  
#### <a name="rules-for-using-functions"></a>Regeln für die Verwendung von Funktionen ####

  - Funktionstyp (Aktualität, Größe, Abstand, Kategorie) muss Kleinbuchstaben.
  - Funktionen dürfen keine null oder eine leere Werte enthalten. Wenn Sie Feldname einschließen, müssen Sie insbesondere auf einen anderen Wert festlegen.
  - Funktionen können nur auf gefiltert Felder angewendet werden. Weitere Informationen zu gefiltert Feldern finden Sie unter [Index erstellen](search-api-2015-02-28-preview.md#CreateIndex) .
  - Funktionen können nur auf Felder angewendet werden, die in der Auflistung Felder eines Indexes definiert sind.

Nachdem der Index definiert ist, erstellen Sie den Index durch die IndexSchema, gefolgt von Dokumente hochladen aus. Weitere Informationen zu diesen Vorgängen finden Sie unter [Index erstellen](search-api-2015-02-28-preview.md#CreateIndex) und [Hinzufügen oder Aktualisieren von Dokumenten](search-api-2015-02-28-preview.md#AddOrUpdateDocuments) . Nachdem der Index erstellt wurde, sollten Sie ein funktionsübergreifendes Punktzahl Profil verfügen, die mit Ihrem Suchdaten passt.

<a name="bkmk_template"></a>
## <a name="template"></a>Vorlage
Dieser Abschnitt listet die Syntax und die Vorlage zur Bewertung von Benutzerprofilen. [Index Attributverweis](#bkmk_indexref) im nächsten Abschnitt eine Beschreibung der Attribute finden Sie unter.

    ...
    "scoringProfiles": [
      {
        "name": "name of scoring profile",
        "text": (optional, only applies to searchable fields) {
          "weights": {
            "searchable_field_name": relative_weight_value (positive #'s),
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
            }

            // (- or -)

            "freshness": {
              "boostingDuration": "..." (value representing timespan over which boosting occurs)
            }

            // (- or -)

            "distance": {
              "referencePointParameter": "...", (parameter to be passed in queries to use as reference location)
              "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends)
            }

            // (- or -)

            "tag": {
              "tagsParameter": "..." (parameter to be passed in queries to specify list of tags to compare against target field)
            }
          }
        ],
        "functionAggregation": (optional, applies only when functions are specified)
            "sum (default) | average | minimum | maximum | firstMatching"
      }
    ],
    "defaultScoringProfile": (optional) "...",
    ...

<a name="bkmk_indexref"></a>
## <a name="scoring-profile-property-reference"></a>Referenz Punktzahl Profil

**Notiz** Eine Funktion Punktzahl kann nur auf Felder angewendet werden, die gefiltert werden.

| Eigenschaft | Beschreibung |
|----------|-------------|
| `name`   | Erforderlich. Dies ist der Name des Profils Punktzahl. Es folgt die gleichen Benennungskonventionen eines Felds aus. Sie müssen mit einem Buchstaben anfangen, keine Punkte, Satzendezeichen enthalten oder @ Symbolen und dürfen nicht mit den Ausdruck "AzureSearch" (Groß-/Kleinschreibung) beginnen. |
| `text` | Enthält die Eigenschaft-Stärken an. |
| `weights` | Optional. Ein Paar der Name-Wert, der einen Feldnamen und relativen Stärke angibt. Relative Gewichtung muss eine positive ganze Zahl oder Gleitkommazahl. Sie können den Feldnamen, ohne eine entsprechende Gewichtung angeben. -Stärken werden verwendet, um die Bedeutung eines Felds relativ zu einem anderen anzugeben. |
| `functions` | Optional. Beachten Sie, dass eine Funktion Punktzahl nur auf Felder angewendet werden kann, die gefiltert werden. |
| `type` | Für die Bewertung der Funktionen erforderlich ist. Gibt den Typ der Funktion zu verwenden. Gültige Werte sind `magnitude`, `freshness`, `distance` und `tag`. Sie können mehr als eine Funktion in jedem Punktzahl Profil einbeziehen. Der Name der Funktion muss Kleinbuchstaben. |
| `boost` | Für die Bewertung der Funktionen erforderlich ist. Eine positive Zahl, die als Multiplikator für unformatierten Punktzahl verwendet wird. Es kann nicht gleich 1 sein. |
| `fieldName` | Für die Bewertung der Funktionen erforderlich ist. Eine Punktzahl-Funktion kann nur angewendet werden auf die Felder, die Teil des Indexes die feldauflistung, aufweisen und gefiltert. Darüber hinaus führt jede Funktionstyp zusätzliche Einschränkungen (Aktualität mit Datetime-Feldern Betrag mit ganze Zahl oder doppelte Felder, Abstand mit Feldern Speicherort und Kategorie mit Zeichenfolge oder Zeichenfolge Websitesammlung Felder verwendet). Sie können nur ein einzelnes Feld pro Funktionsdefinition angeben. Beispielsweise um Ausmaß zweimal in demselben Profil verwenden zu können, müssen Sie zwei Definitionen Ausmaß erhöhen, einen für jedes Feld aufnehmen möchten. |
| `interpolation` | Für die Bewertung der Funktionen erforderlich ist. Definiert die Steigung für die der Punktzahl verstärken erhöht ab dem Anfang des Bereichs bis zum Ende des Bereichs. Gültige Werte sind `linear` (Standard), `constant`, `quadratic`, und `logarithmic`. Weitere Informationen finden Sie unter [Interpolationen festlegen](#bkmk_interpolation) . |
| `magnitude` | Das Ausmaß bewerten-Funktion wird verwendet, um die Rangfolgen aufgrund der Wertebereich für ein numerisches Feld ändern. Einige der am häufigsten verwendeten Verwendungsbeispiele hierfür sind:<ul><li>Sternebewertungen: Ändern der Punktzahl basierend auf dem Wert in das Feld "Sterne-Bewertung". Wenn zwei Elemente relevant sind, wird das Element mit der höheren Bewertung zuerst angezeigt werden.</li><li>Rand: Wenn zwei Dokumente relevant sind, kann ein Einzelhändler möchten Dokumente zu erhöhen, die höhere Ränder zuerst aufweisen.</li><li>Klicken Sie auf zählt: für Applikationen, das Nachverfolgen von bis zu Produkten oder Seiten Aktionen geklickt haben, können Ausmaß zu steigern Elemente, die in der Regel die am häufigsten Datenverkehr zu erhalten.</li><li>Zählt herunterladen: haben Sie für Applikationen, mit dem Ausmaß-Funktion können Sie durch fördern nachverfolgen Downloads von Elementen, die die am häufigsten Downloads zur Verfügung.</li></ul> |
| `magnitude:boostingRangeStart` | Legt den Startwert des Bereichs über der Betrag bewertet wird. Der Wert muss eine ganze Zahl oder Gleitkommazahl sein. Dies würde Stern Bewertungen von 1 bis 4 1 lauten. Seitenränder mehr als 50 % würde dies 50 lauten. |
| `magnitude:boostingRangeEnd` | Legt den Endwert des Bereichs über der Betrag bewertet wird. Der Wert muss eine ganze Zahl oder Gleitkommazahl sein. Dies würde Stern Bewertungen von 1 bis 4 4 lauten. |
| `magnitude:constantBoostBeyondRange` | Gültige Werte sind True oder False (Standard). Bei der Einstellung true, die vollständige steigern weiterhin auf Dokumente anzuwenden, die einen Wert für das Zielfeld aufweisen, die über das obere Ende des Bereichs liegt. Mit FALSCH belegt, wird nicht der steigern dieser Funktion auf einen Wert für das Zielfeld, das außerhalb des Bereichs liegen Probleme Dokumente angewendet werden. |
| `freshness` | Die Funktion bewerten Aktualität wird verwendet, um die Rangfolge der Ergebnisse für Elemente auf der Grundlage der Werte in Feldern DateTimeOffset alter. Beispielsweise kann ein Element mit einem neueren Datum höher, als älterer Elemente eingestuft werden. (Beachten Sie, dass auch möglich, Rang Elemente wie Kalenderereignisse zukünftigen Datum ist, so, dass Elemente näher am aktuellen höher als Elemente weiter in der Zukunft eingestuft werden können.) In der aktuellen Dienstversion wird ein Ende des Bereichs auf die aktuelle Uhrzeit behoben sein. Das andere Ende wird eine Uhrzeit in der Vergangenheit auf der Grundlage der `boostingDuration`. Durch fördern ein Zellbereich im vorkommen, verwenden Sie eine Negative `boostingDuration`. Die Rate, an dem die verstärken Änderungen aus einem Maximum und minimalen Bereich liegt, wird durch die Interpolation bestimmt, das Bewertungssystem Profil angewendet (siehe Abbildung unten). Wenn den steigern Faktor angewendet umdrehen möchten, wählen Sie steigern Faktor kleiner als 1 aus. |
| `freshness:boostingDuration` | Legt fest, nach welcher verstärken ein Ablaufzeitraum für ein bestimmtes Dokument anhält. Finden Sie unter [Menge BoostingDuration] [#Bkmk_boostdur] im folgenden Abschnitt Syntax und Beispiele für. |
| `distance` | Schließen Sie den Abstand Punktzahl-Funktion verwendet, um die Auswirkungen auf die Bewertung der Dokumente basierend auf wie oder ganz diese relativ zu einem Bezug geographischen Standort sind. Die Position der Bezug als Teil der Abfrage in einem Parameter angegeben wird (mit den `scoringParameter` Abfrage Parameter) als eine Lon, Lat Argument. |
| `distance:referencePointParameter` | Ein Parameter zu übergebenden in Abfragen als Referenz Speicherort verwendet werden soll. ScoringParameter ist ein Abfrageparameter. Eine Beschreibung der Abfrageparameter finden Sie unter [Dokumente durchsuchen](search-api-2015-02-28-preview.md#SearchDocs) . |
| `distance:boostingDistance` | Eine Zahl, die den Abstand in Kilometer ab der Position Verweis zeigt an, wo der steigern Bereich endet. |
| `tag` | Die Kategorie bewerten (Funktion) wird verwendet, um der Punktzahl von Dokumenten, die auf Grundlage von Tags in Dokumenten und Suchabfragen beeinflussen. Dokumente, die Tags mit der Suchabfrage gemeinsam haben, werden erhöht werden. Die Tags für die Suchabfrage als Punktzahl Parameter in jeder Suchabfrage bereitgestellt wird (mit den `scoringParameter` Abfrage Parameter). |
| `tag:tagsParameter` | Parameter zu übergebenden in Abfragen Tags für eine bestimmte Anforderung angeben. `scoringParameter`ist ein Abfrageparameter an. Eine Beschreibung der Abfrageparameter finden Sie unter [Dokumente durchsuchen](search-api-2015-02-28-preview.md#SearchDocs) . |
| `functionAggregation` | Optional. Gilt nur, wenn Funktionen angegeben sind. Gültige Werte sind: `sum` (Standard), `average`, `minimum`, `maximum`, und `firstMatching`. Eine Suche Bewertung ist ein einzelner Wert, der aus mehreren Variablen, einschließlich mehrerer Funktionen berechnet wird. Diese Attribute gibt an, wie die Prozessen aller Funktionen in einer einzelnen aggregieren steigern kombiniert werden, die dann auf der Basis Dokument Punktzahl angewendet wird. Basiswert basiert auf der Tf-Idf Wert berechnet aus dem Dokument und der Suchabfrage. |
| `defaultScoringProfile` | Wenn Sie eine Suchabfrage ausgeführt wird, wenn keine Punktzahl Profil angegeben ist, ist dann Standard bewerten verwendeten (Tf-Idf nur). Ein Standardwert bewerten Profilname kann bewirken, dass Azure-Suche auf dieses Profil verwenden, wenn kein bestimmtes Profil in die Suchabfrage angegeben wird hier festgelegt werden. |

<a name="bkmk_interpolation"></a>
## <a name="set-interpolations"></a>Interpolationen festlegen

Interpolationen ermöglichen es Ihnen, für die die Bewertung verstärken erhöht ab dem Anfang des Bereichs bis zum Ende des Bereichs die Steigung definieren. Die folgenden Interpolationen können verwendet werden:

  - `Linear`: Für Elemente, die Sie im Bereich Max und min sind, wird die steigern angewendet werden, um das Element in ständig Abnahme erfolgen. Linear ist die standardmäßige Interpolation für eine Punktzahl Profil.
  - `Constant`: Für Elemente, die innerhalb der Start- und Enddatum Bereich befinden, wird eine Konstante steigern zu den Rang Ergebnissen angewendet.
  - `Quadratic`: Verglichen mit eine lineare Interpolation, die eine ständig abnehmenden steigern hat, quadratisch werden zunächst in kleineren Tempo verkleinern und dann Wenn sie den Bereich Ende erreicht, es verkleinert Intervallen sehr viel höher. Diese Interpolationsoption ist in der Kategorie bewerten Funktionen nicht zulässig.
  - `Logarithmic`: Verglichen mit eine lineare Interpolation, die eine ständig abnehmenden steigern enthält, wählen Sie diese Option Anfangs höhere Tempo verringert, und klicken Sie dann Wenn sie den Bereich Ende erreicht, es verkleinert in kleineren Intervallen viel. Diese Interpolationsoption ist in der Kategorie bewerten Funktionen nicht zulässig.

<a name="Figure1"></a>
 ![][1]

<a name="bkmk_boostdur"></a>
## <a name="set-boostingduration"></a>Festlegen von boostingDuration

`boostingDuration`ist ein Attribut der Aktualität-Funktion. Damit können sie ein Ablaufdatum festlegen Zeitraum, nach welcher verstärken für ein bestimmtes Dokument anhält. Um einer Produktlinie oder Marke für einen Zeitraum von 10 Tagen Werbeaktion zu verbessern, würden Sie beispielsweise den Zeitraum 10 Tagen als "P10D" für diese Dokumente angeben. Oder geben Sie zum steigern bevorstehende Ereignisse in der nächsten Woche "-P7D".

`boostingDuration`muss als "DayTimeDuration" XSD-Wert (eine eingeschränkte Teilmenge der Wert für die Dauer einer ISO 8601) formatiert werden. Das Muster für Dies ist: `[-]P[nD][T[nH][nM][nS]]`.

Die folgende Tabelle enthält einige Beispiele.

| Dauer | boostingDuration |
|----------|------------------|
| 1 Tag | "P1D" |
| 2 Tage und 12 Stunden | "P2DT12H" |
| 15 Minuten | "PT15M" |
| 30 Tage, 5 Stunden 10 Minuten, und 6.334 Sekunden | "P30DT5H10M6.334S" |

Wenn Sie weitere Beispiele finden Sie unter [XML-Schema: Datentypen (W3.org-Website)](http://www.w3.org/TR/xmlschema11-2/).

**Siehe auch**
[Suche Azure Service REST-API](http://msdn.microsoft.com/library/azure/dn798935.aspx) auf MSDN <br/>
Auf der MSDN- [Index (Azure Suche API) erstellen](http://msdn.microsoft.com/library/azure/dn798941.aspx)<br/>
[Hinzufügen einer Punktzahl-Profil, um einen Suchindex](http://msdn.microsoft.com/library/azure/dn798928.aspx) auf MSDN<br/>

<!--Image references-->
[1]: ./media/search-api-scoring-profiles-2015-02-28-Preview/scoring_interpolations.png

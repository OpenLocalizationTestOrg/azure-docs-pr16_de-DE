<properties
    pageTitle="Abfragen mithilfe der REST-API Azure Suchindex | Microsoft Azure | Cloud gehosteten Suchdienst"
    description="Erstellen Sie eine Suchabfrage in Azure suchen und Verwenden von Suchparametern zu filtern und Sortieren von Suchergebnissen."
    services="search"
    documentationCenter=""
    manager="jhubbard"
    authors="ashmaka"
/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="query-your-azure-search-index-using-the-rest-api"></a>Verwenden die REST-API Azure Suchindex Abfragen
> [AZURE.SELECTOR]
- [(Übersicht)](search-query-overview.md)
- [Portal](search-explorer.md)
- [.NET](search-query-dotnet.md)
- [REST](search-query-rest-api.md)

In diesem Artikel wird die Vorgehensweise zum Abfragen eines Indexes mithilfe der [Azure Suche REST-API](https://msdn.microsoft.com/library/azure/dn798935.aspx)angezeigt.

Bevor Sie beginnen diese exemplarische Vorgehensweise, sollten Sie bereits [eine Azure Suchindex erstellt](search-what-is-an-index.md) und [sie mit Daten aufgefüllt](search-what-is-data-import.md)verfügen.

## <a name="i-identify-your-azure-search-services-query-api-key"></a>Ich. Identifizieren Sie Ihrer Azure Suchdienst Abfrage-api-Taste
Eine wichtige Komponente eines jeden Suchvorgangs gegen die Azure Suche REST-API ist die *api-Taste* , die für den Dienst generiert wurde, die Sie nach der Bereitstellung. Gibt es einen gültigen Key stellt vertrauen, auf Basis pro Anforderung, zwischen der Anwendung senden der Anfrage und der Dienst, der die Behandlung ganzer her.

1. Aufrufen des Diensts api-Schlüssel finden müssen Sie sich bei der [Azure-Portal](https://portal.azure.com/) anmelden
2. Wechseln Sie zu Ihrer Suche Azure-Service-Karte vorausgesetzt
3. Klicken Sie auf das Symbol "Tasten"

Der Dienst, *Administrator Tasten* und die *Abfrage Tasten*müssen.

 - Ihre primären und sekundären *Administrator Tasten* erteilen Vollzugriff auf alle Vorgänge, einschließlich der Berechtigung zum Verwalten des Diensts, erstellen und Löschen von Indizes, Indexer und Datenquellen. Es gibt zwei Tasten, damit Sie fortsetzen können, um die sekundäre Key zu verwenden, wenn Sie sich entscheiden, den Primärschlüssel und umgekehrt neu zu generieren.
 - Ihre *Abfrage Tasten* schreibgeschützten Zugriff auf Indizes und Dokumente gewähren, und werden in der Regel-Clientanwendungen, die Suchabfragen ausgeben verteilt.

Im Sinne ein Indexes Abfragen können Sie eine Abfrage Keys verwenden. Ihr Administrator Tasten können auch für Abfragen verwendet werden, aber Sie sollten einen Abfrageschlüssel in der Anwendungscode verwenden, wie das [Prinzip der minimalen Rechte](https://en.wikipedia.org/wiki/Principle_of_least_privilege)Sie besser entspricht.

## <a name="ii-formulate-your-query"></a>II. Erstellen einer Abfrage
Es gibt zwei Möglichkeiten, um [Ihre mithilfe der REST-API Index zu durchsuchen](https://msdn.microsoft.com/library/azure/dn798927.aspx). Eine Möglichkeit besteht eine HTTP POST-Anforderung ausgeben, wo Ihre Abfrageparameter in ein JSON-Objekt im Hauptteil Anforderung definiert wird. Die andere Möglichkeit ist eine HTTP GET-Anforderung, wo Ihre Abfrageparameter innerhalb der Anforderung URL definiert wird. Beachten Sie, dass Beitrag weitere [gelockert Grenzwerte](https://msdn.microsoft.com/library/azure/dn798927.aspx) auf die Größe des Abfrageparameter als GET vorhanden sind. Es wird empfohlen, daher Beitrag verwenden, es sei denn, Sie besondere Umstände vorliegen, unter Verwendung von GET komfortabler wäre.

Bereitstellen und abrufen, müssen Sie Ihren *Namen*, *Indexnamen*und die richtige *Version API* bereitstellen (die aktuelle Version der API ist `2015-02-28` zum Zeitpunkt der Veröffentlichung dieses Dokuments) in der Anfrage-URL. Für abrufen werden am Ende der URL *-Abfragezeichenfolge* , wo Sie die Abfrageparameter erhalten. Im folgenden finden Sie die URL-Format:

    https://[service name].search.windows.net/indexes/[index name]/docs?[query string]&api-version=2015-02-28

Das Format für Beitrag ist den gleichen, jedoch mit nur api-Version die Zeichenfolge Abfrageparameter.



#### <a name="example-queries"></a>Beispiel für Abfragen

Hier sind ein paar Beispielabfragen auf einen Index namens "Hotels" ein. Diese Abfragen werden sowohl GET und POST-Format angezeigt.

Suchen Sie den gesamten Index für den Ausdruck 'Budget' und nur Zurückgeben der `hotelName` Feld:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=budget&$select=hotelName&api-version=2015-02-28

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2015-02-28
{
    "search": "budget",
    "select": "hotelName"
}
```

Anwenden eines Filters auf den Index zu Hotels kostengünstiger als $150 pro Nacht suchen und Zurückgeben der `hotelId` und `description`:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=*&$filter=baseRate lt 150&$select=hotelId,description&api-version=2015-02-28

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2015-02-28
{
    "search": "*",
    "filter": "baseRate lt 150",
    "select": "hotelId,description"
}
```

Suchen Sie den gesamten Index, Reihenfolge nach einem bestimmten Feld (`lastRenovationDate`) in absteigender Reihenfolge an, nehmen Sie die oberen zwei Ergebnisse und nur anzeigen `hotelName` und `lastRenovationDate`:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=*&$top=2&$orderby=lastRenovationDate desc&$select=hotelName,lastRenovationDate&api-version=2015-02-28

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2015-02-28
{
    "search": "*",
    "orderby": "lastRenovationDate desc",
    "select": "hotelName,lastRenovationDate",
    "top": 2
}
```

## <a name="iii-submit-your-http-request"></a>III. Senden Sie Ihre HTTP-Anforderung
Jetzt, da Sie Ihre Abfrage als Teil der URL des HTTP-Anforderung (für GET) oder Textkörper (für POST) formuliert haben, können Sie Ihre Anfrage-Header definieren und übermitteln Sie Ihre Abfrage.

#### <a name="request-and-request-headers"></a>Anfrage und Anfrage-Header
Sie müssen zwei Anforderung Kopfzeilen für GET oder drei für Beitrag definieren:
1. Die `api-key` Kopfzeile muss mit dem Abfrage-Key Sie in Schritt ich oben gefunden festgelegt werden. Beachten Sie, dass Sie ein Administrator Key als auch verwenden, können die `api-key` Kopfzeile, aber es wird empfohlen, dass Sie einen Abfrageschlüssel verwenden, wie er ausschließlich schreibgeschützten Zugriff auf Indizes und Dokumente gewährt.
2. Die `Accept` Kopfzeile muss auf festgelegt sein `application/json`.
3. Für den Beitrag nur die `Content-Type` Kopfzeile muss auch festgelegt werden, um `application/json`.

Im folgenden finden Sie eine HTTP GET-Anforderung mithilfe der Azure Suche REST-API, verwenden eine einfache Abfrage, die für den Ausdruck "Motels" durchsucht "Hotels" Index zu suchen:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=motel&api-version=2015-02-28
Accept: application/json
api-key: [query key]
```

Hier ist die gleiche Beispielabfrage, diesmal unter Verwendung von HTTP POST an:

```
POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2015-02-28
Content-Type: application/json
Accept: application/json
api-key: [query key]

{
    "search": "motel"
}
```

Anforderung einer erfolgreichen Abfrage führt zu Fehlern in einem Status `200 OK` und die Suchergebnisse werden im Textkörper der Antwort als JSON zurückgegeben. Hier sind welche die Ergebnisse für die oben genannten Abfrage aussehen wie, sofern der Index "Hotels" wird mit den Beispieldaten in [Daten importieren in Azure suchen, verwenden die REST-API](search-import-data-rest-api.md) (Beachten Sie, dass die JSON aus Gründen der Übersichtlichkeit formatiert wurde) aufgefüllt.

```JSON
{
    "value": [
        {
            "@search.score": 0.59600675,
            "hotelId": "2",
            "baseRate": 79.99,
            "description": "Cheapest hotel in town",
            "description_fr": "Hôtel le moins cher en ville",
            "hotelName": "Roach Motel",
            "category": "Budget",
            "tags":["motel", "budget"],
            "parkingIncluded": true,
            "smokingAllowed": true,
            "lastRenovationDate": "1982-04-28T00:00:00Z",
            "rating": 1,
            "location": {
                "type": "Point",
                "coordinates": [-122.131577, 49.678581],
                "crs": {
                    "type":"name",
                    "properties": {
                        "name": "EPSG:4326"
                    }
                }
            }
        }
    ]
}
```

Weitere Informationen finden Sie auf den Abschnitt "Antworten" [Dokumente durchsuchen](https://msdn.microsoft.com/library/azure/dn798927.aspx). Weitere Informationen zu anderen HTTP-Status-Codes, die im Fall eines Fehlers zurückgegeben werden kann, finden Sie unter [HTTP Statuscodes (Azure-Suche)](https://msdn.microsoft.com/library/azure/dn798925.aspx).

<properties
    pageTitle="Hochladen von Daten in Azure suchen, verwenden die REST-API | Microsoft Azure | Cloud gehosteten Suchdienst"
    description="Informationen Sie zum Hochladen von Daten in einen Index in Azure suchen, verwenden die REST-API."
    services="search"
    documentationCenter=""
    authors="ashmaka"
    manager="jhubbard"
    editor=""
    tags=""/>

<tags
    ms.service="search"
    ms.devlang="rest-api"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="upload-data-to-azure-search-using-the-rest-api"></a>Hochladen von Daten in die REST-API mit Azure-Suche
> [AZURE.SELECTOR]
- [(Übersicht)](search-what-is-data-import.md)
- [.NET](search-import-data-dotnet.md)
- [REST](search-import-data-rest-api.md)

In diesem Artikel wird gezeigt, wie die [Azure Suche REST-API](https://msdn.microsoft.com/library/azure/dn798935.aspx) zum Importieren von Daten in einer Azure Suchindex verwenden werden.

Bevor Sie beginnen diese exemplarische Vorgehensweise, sollten Sie bereits [erstellt eine Azure Suchindex](search-what-is-an-index.md)verfügen.

Damit Dokumente in den Index verwenden die REST-API Pushbenachrichtigungen, werden Sie HTTP POST-Anforderung an URL-Endpunkt des Indexes ausgeben. Textkörper des HTTP-Anforderungstexts ist ein JSON-Objekt, das mit den Dokumenten hinzugefügt, geändert oder gelöscht werden, wenn Sie an.

## <a name="i-identify-your-azure-search-services-admin-api-key"></a>Ich. Identifizieren Sie Ihrer Azure Suchdienst Admin-api-Taste
Wenn HTTP-Anfragen anhand des Diensts mithilfe der REST-API ausgegeben wird, muss *jeder* API Anforderung den api-Schlüssel enthalten, der für den Suchdienst generiert wurde, die Sie nach der Bereitstellung. Gibt es einen gültigen Key stellt vertrauen, auf Grundlage eines jeden Anforderung zwischen der Anwendung, senden die Anfrage und der Dienst, der die Behandlung ganzer her.

1. Aufrufen des Diensts api-Schlüssel finden müssen Sie sich bei der [Azure-Portal](https://portal.azure.com/) anmelden
2. Wechseln Sie zu Ihrer Suche Azure-Service-Karte vorausgesetzt
3. Klicken Sie auf das Symbol "Tasten"

Der Dienst, *Administrator Tasten* und die *Abfrage Tasten*müssen.

  - Ihre primären und sekundären *Administrator Tasten* erteilen Vollzugriff auf alle Vorgänge, einschließlich der Berechtigung zum Verwalten des Diensts, erstellen und Löschen von Indizes, Indexer und Datenquellen. Es gibt zwei Tasten, damit Sie fortsetzen können, um die sekundäre Key zu verwenden, wenn Sie sich entscheiden, den Primärschlüssel und umgekehrt neu zu generieren.
  - Ihre *Abfrage Tasten* schreibgeschützten Zugriff auf Indizes und Dokumente gewähren, und werden in der Regel-Clientanwendungen, die Suchabfragen ausgeben verteilt.

Im Sinne des Datenimports in einen Index, können Sie mithilfe einer der primären oder sekundären Administrator-Taste.

## <a name="ii-decide-which-indexing-action-to-use"></a>II. Entscheiden Sie, welche Indizierung Aktion verwenden
Wenn Sie die REST-API verwenden, werden Sie HTTP POST-Anfragen JSON Anforderung stellen Endpunkt-URL für Ihre Azure Suchindex ausgeben. Das JSON-Objekt in Ihrem HTTP-Anforderungstexts enthält ein einzelnes JSON-Array mit dem Namen "Wert" mit JSON-Objekten, die Dokumente, die Sie hinzufügen, zur Index, aktualisieren oder löschen möchten darstellen.

Jede JSON-Objekt in der Matrix "Wert" steht für ein Dokument indiziert werden. Jedes dieser Objekte enthält Schlüssel des Dokuments und gibt an, die gewünschten Indizierung (hochladen, zusammenführen, löschen usw.). Je nachdem, welche die unter Aktionen Sie auswählen, nur bestimmte Felder für jedes Dokument einbezogen werden soll:

@search.action | Beschreibung | Für jedes Dokument erforderlichen Felder | Notizen
--- | --- | --- | ---
`upload` | Eine `upload` Aktion ist vergleichbar mit einer "Upsert", wo wird das Dokument eingefügt wird, wenn es neu ist und aktualisiert/ersetzt, wenn sie vorhanden ist. | Schlüssel sowie alle anderen Felder, die Sie definieren möchten. | Beim Aktualisieren/eines vorhandenen Dokuments ersetzen haben jedes Feld, das nicht in der Besprechungsanfrage angegeben wird deren Feld auf `null`. Dies geschieht, selbst wenn das Feld zuvor ein nicht-Null-Wert festgelegt wurde.
`merge` | Aktualisiert ein vorhandenes Dokument mit den angegebenen Feldern. Wenn das Dokument im Index nicht vorhanden ist, wird die Zusammenführung fehl. | Taste und alle anderen Felder, die Sie definieren möchten, | Ein beliebiges Feld in einem Seriendruck angegebenen ersetzt das bestehende Feld im Dokument. Dies umfasst die Felder vom Typ `Collection(Edm.String)`. Beispielsweise, wenn das Dokument ein Feld enthält `tags` mit dem Wert `["budget"]` , und führen Sie einen Seriendruck mit dem Wert `["economy", "pool"]` für `tags`, der Endwert von der `tags` Feld `["economy", "pool"]`. Es werden keine `["budget", "economy", "pool"]`.
`mergeOrUpload` | Diese Aktion verhält sich wie `merge` Wenn ein Dokument mit dem angegebenen Schlüssel im Index bereits vorhanden ist. Wenn das Dokument nicht vorhanden ist, wird er verhält sich wie `upload` mit einem neuen Dokument. | Schlüssel sowie alle anderen Felder, die Sie definieren möchten. | -
`delete` | Das angegebene Dokument entfernt aus dem Index. | nur Schlüssel | Alle Felder Geben Sie mit anderen als Feld mit das ignoriert wird. Wenn Sie ein einzelnes Feld aus einem Dokument entfernen möchten, verwenden Sie `merge` stattdessen und einfach das Feld explizit auf null festgelegt.

## <a name="iii-construct-your-http-request-and-request-body"></a>III. Erstellen Sie Ihre HTTP-Anforderung und fordern Textkörper an
Jetzt, da Sie die erforderlichen Feldwerte für Index Aktionen gesammelt haben, sind Sie bereit sind, erstellen die eigentliche HTTP-Anforderung und JSON-Anforderungstexts, um Ihre Daten zu importieren.

#### <a name="request-and-request-headers"></a>Anfrage und Anfrage-Header
In der URL, Sie müssen Ihren Namen Service, Indexnamen (in diesem Fall "Hotels") als auch die richtige Version der API bereitstellen (die aktuelle Version der API ist `2015-02-28` zum Zeitpunkt der Veröffentlichung dieses Dokuments). Sie müssen definieren die `Content-Type` und `api-key` Kopfzeilen anfordern. Verwenden Sie eine der des Diensts Administrator Tasten für letztere.

    POST https://[search service].search.windows.net/indexes/hotels/docs/index?api-version=2015-02-28
    Content-Type: application/json
    api-key: [admin key]

#### <a name="request-body"></a>Anforderungstexts

```JSON
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
            "@search.action": "mergeOrUpload",
            "hotelId": "3",
            "baseRate": 129.99,
            "description": "Close to town hall and the river"
        },
        {
            "@search.action": "delete",
            "hotelId": "6"
        }
    ]
}
```

In diesem Fall verwenden wir `upload`, `mergeOrUpload`, und `delete` als unsere Aktionen suchen.

Wird davon ausgegangen Sie, dass dieses Beispiel "Hotels" Index bereits mit einer Reihe von Dokumenten gefüllt wird. Beachten Sie, wie wir keinen alle möglichen Dokumentfelder angeben bei Verwendung von `mergeOrUpload` und wie die Taste Dokument nur angegeben (`hotelId`) bei Verwendung von `delete`.

Beachten Sie, dass nur nach Zeitphasen bis zum 1000 Dokumente (oder 16 MB) enthalten sein können in einer Anforderung indizieren.

## <a name="iv-understand-your-http-response-code"></a>IV. Grundlegendes zu Ihren HTTP-Antwort-code
#### <a name="200"></a>200
Nachdem Sie eine erfolgreiche Indizierung Anforderung abgesendet erhalten Sie eine HTTP-Antwort mit Statuscode der `200 OK`. Der JSON Hauptteil der HTTP-Antwort wird wie folgt aussehen:

```JSON
{
    "value": [
        {
            "key": "unique_key_of_document",
            "status": true,
            "errorMessage": null
        },
        ...
    ]
}
```

#### <a name="207"></a>207
Einen Statuscode des `207` wird zurückgegeben, wenn mindestens ein Element nicht erfolgreich indiziert wurde. JSON Hauptteil der HTTP-Antwort enthält Informationen über die nicht erfolgreich Dokumente.

```JSON
{
    "value": [
        {
            "key": "unique_key_of_document",
            "status": false,
            "errorMessage": "The search service is too busy to process this document. Please try again later."
        },
        ...
    ]
}
```

> [AZURE.NOTE] Häufig bedeutet, dass die Belastung Suchdienst einen Punkt erreicht ist, in dem Anfragen Indizierung beginnt mit dem zurückzukehren `503` Antworten. In diesem Fall hochgradig wird empfohlen, die Ihre Client-Code wieder deaktivieren und warten, bevor Sie wiederholen. Dies kann das System einige Zeit erzielt werden zum Wiederherstellen, erhöhen die Chancen, die zukünftige Anfragen erfolgreich durchgeführt werden können. Wiederholen Ihre Anfragen schnell werden nur die Situation verlängert werden.

#### <a name="429"></a>429
Einen Statuscode des `429` wird zurückgegeben, wenn Sie Ihr Kontingent für die Anzahl der Dokumente pro Index überschritten haben.

#### <a name="503"></a>503
Einen Statuscode des `503` zurückgegeben, wenn keines der Elemente in der Besprechungsanfrage erfolgreich indiziert wurden. Dieser Fehler bedeutet, dass das System bei hoher Auslastung und Ihre Anforderung kann nicht zu diesem Zeitpunkt verarbeitet werden.

> [AZURE.NOTE] In diesem Fall hochgradig wird empfohlen, die Ihre Client-Code wieder deaktivieren und warten, bevor Sie wiederholen. Dies kann das System einige Zeit erzielt werden zum Wiederherstellen, erhöhen die Chancen, die zukünftige Anfragen erfolgreich durchgeführt werden können. Wiederholen Ihre Anfragen schnell werden nur die Situation verlängert werden.

Weitere Informationen zum Dokument Aktivitäten und Erfolg/Fehler Antworten können finden Sie unter [Hinzufügen, aktualisieren oder Löschen von Dokumenten](https://msdn.microsoft.com/library/azure/dn798930.aspx). Weitere Informationen zu anderen HTTP-Status-Codes, die im Fall eines Fehlers zurückgegeben werden kann, finden Sie unter [HTTP Statuscodes (Azure-Suche)](https://msdn.microsoft.com/library/azure/dn798925.aspx).

## <a name="next"></a>Weiter
Nach Auffüllen Azure Suchindex, werden Sie Abfragen zu suchenden Dokumente ausgeben beginnen. Details finden Sie in [Ihrer Abfrage Azure Suchindex](search-query-overview.md) .

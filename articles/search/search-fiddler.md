<properties
    pageTitle="Wie Fiddler ausgewertet werden soll, und Testen Sie Azure Suche REST APIs | Microsoft Azure | Cloud gehosteten Suchdienst"
    description="Verwenden Sie Fiddler für einen codefreien Ansatz zum Überprüfen der Verfügbarkeit von Azure suchen und die restlichen APIs ausprobieren."
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="mblythe"
    editor=""/>

<tags
    ms.service="search"
    ms.devlang="rest-api"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="10/17/2016"
    ms.author="heidist"/>

# <a name="use-fiddler-to-evaluate-and-test-azure-search-rest-apis"></a>Verwenden von Fiddler evaluieren und Testen Azure Suche REST-APIs
> [AZURE.SELECTOR]
- [(Übersicht)](search-query-overview.md)
- [Search-Explorer](search-explorer.md)
- [Fiddler](search-fiddler.md)
- [.NET](search-query-dotnet.md)
- [REST](search-query-rest-api.md)

In diesem Artikel wird erläutert, wie Fiddler, [kostenlos von Telerik](http://www.telerik.com/fiddler), auf Problem HTTP-Anfragen und Antworten anzeigen, die mit den REST-API Azure suchen, ohne Schreiben von Code verwenden. Azure suchen ist vollständig Rechte verwaltet, Cloud-Suchdienst auf Microsoft Azure, leichter programmierbare über .NET und die restlichen APIs gehostet. Der Azure-Suchdienst REST-APIs werden auf der [MSDN-](https://msdn.microsoft.com/library/azure/dn798935.aspx)beschrieben.

In den folgenden Schritten werden Sie einen Index erstellen, Dokumente hochladen, Abfrage den Index und Abfrage klicken Sie dann auf das System Service Informationen.

Wenn Sie diese Schritte ausführen zu können, benötigen Sie einen Azure-Suchdienst und `api-key`. Anweisungen für den Einstieg finden Sie unter [Erstellen einer Azure Suchdienst im Portal](search-create-service-portal.md) .

## <a name="create-an-index"></a>Erstellen eines Indexes

1. Starten Sie Fiddler. Deaktivieren Sie im Menü **Datei** **Erfassen Datenverkehr** an irrelevante HTTP-Aktivität ausblenden, die nicht im Zusammenhang mit den aktuellen Vorgang ist ein.

3. Klicken Sie auf der Registerkarte **Composer** erhalten Sie eine Anforderung aufstellen, die folgenden Screenshot aussieht.

    ![][1]

2. Wählen Sie aus, **setzen**.

3. Geben Sie die URL, die die api-Version, die URL und Anforderung Attribute angibt. Einige Informationen zu beachten:
   + Verwenden Sie HTTPS als Präfix ein.
   + Anforderung-Attribut ist "/ Indizes/Hotels". Dies weist suchen, um einen Index namens 'Hotels' zu erstellen.
   + API-Version wird auf Kleinbuchstaben, festgelegte "? api-Version 2015-02-28 =". Die Versionen-API sind wichtig, da Azure Suche regelmäßig Updates bereitstellt. Ein Update-Dienst möglicherweise eine wichtige Änderung an die API vorstellen können seltene Anlässe klicken Sie auf. Aus diesem Grund erfordert Azure suchen eine api-Version bei jeder Anforderung, dass Sie im Feld Vollzugriff sind über der verwendet wird.

    Die vollständige URL sollte ähnlich wie im folgenden Beispiel aus.

            https://my-app.search.windows.net/indexes/hotels?api-version=2015-02-28

4.  Geben Sie an der Anfrage-Header, ersetzen den Host und api-Schlüssel mit Werten, die für Ihren Dienst gültig sind.

            User-Agent: Fiddler
            host: my-app.search.windows.net
            content-type: application/json
            api-key: 1111222233334444

5.  Textkörper anfordern fügen Sie in die Felder, die der Indexdefinition zusammensetzt.
            
             {
            "name": "hotels",  
            "fields": [
              {"name": "hotelId", "type": "Edm.String", "key":true, "searchable": false},
              {"name": "baseRate", "type": "Edm.Double"},
              {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
              {"name": "hotelName", "type": "Edm.String"},
              {"name": "category", "type": "Edm.String"},
              {"name": "tags", "type": "Collection(Edm.String)"},
              {"name": "parkingIncluded", "type": "Edm.Boolean"},
              {"name": "smokingAllowed", "type": "Edm.Boolean"},
              {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
              {"name": "rating", "type": "Edm.Int32"},
              {"name": "location", "type": "Edm.GeographyPoint"}
             ]
            }

6.  Klicken Sie auf **Ausführen**.

In ein paar Sekunden auftreten beim einer Antwort HTTP 201 in der Sitzungsliste, die angibt, dass der Index erfolgreich erstellt wurde.

Wenn Sie HTTP 504 erhalten möchten, stellen Sie sicher, dass die URL HTTPS gibt an. Wenn HTTP 400 oder 404 auftreten, überprüfen Sie den Hauptteil der Anforderung zum Überprüfen, dass keine Fehler kopieren und einfügen. Eine HTTP 403 gibt in der Regel ein Problem mit dem api-Schlüssel (einen ungültigen Schlüssel oder ein Problem Syntax mit, wie die api-Taste angegeben ist).

## <a name="load-documents"></a>Laden Sie Dokumente

Klicken Sie auf der Registerkarte **Composer** wird der Anforderung zum Übermitteln von Dokumenten wie folgt aussehen. Hauptteil der Anforderung enthält die Suchdaten für 4 Hotels.

   ![][2]

1. Wählen Sie **Bereitstellen**.

2.  Geben Sie die URL, die beginnt mit HTTPS, gefolgt von Ihrem Dienst-URL, von gefolgt "/indexes/ < Indexname >/Dokumente/Index? api-Version 2015-02-28 =". Die vollständige URL sollte ähnlich wie im folgenden Beispiel aus.

            https://my-app.search.windows.net/indexes/hotels/docs/index?api-version=2015-02-28

3.  Anfrage-Header sollte identisch sein. Denken Sie daran, dass Sie den Host und api-Taste durch die Werte ersetzt, die für Ihren Dienst gültig sind.

            User-Agent: Fiddler
            host: my-app.search.windows.net
            content-type: application/json
            api-key: 1111222233334444

4.  Textkörper anfordern enthält vier Dokumente, die auf den Index Hotels hinzugefügt werden.

            {
            "value": [
            {
                "@search.action": "upload",
                "hotelId": "1",
                "baseRate": 199.0,
                "description": "Best hotel in town",
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
                "@search.action": "upload",
                "hotelId": "3",
                "baseRate": 279.99,
                "description": "Surprisingly expensive",
                "hotelName": "Dew Drop Inn",
                "category": "Bed and Breakfast",
                "tags": ["charming", "quaint"],
                "parkingIncluded": true,
                "smokingAllowed": false,
                "lastRenovationDate": null,
                "rating": 4,
                "location": { "type": "Point", "coordinates": [-122.33207, 47.60621] }
              },
              {
                "@search.action": "upload",
                "hotelId": "4",
                "baseRate": 220.00,
                "description": "This could be the one",
                "hotelName": "A Hotel for Everyone",
                "category": "Basic hotel",
                "tags": ["pool", "wifi"],
                "parkingIncluded": true,
                "smokingAllowed": false,
                "lastRenovationDate": null,
                "rating": 4,
                "location": { "type": "Point", "coordinates": [-122.12151, 47.67399] }
              }
             ]
            }

8.  Klicken Sie auf **Ausführen**.

Einige Sekunden dauern sollten Sie eine Antwort HTTP 200 in der Sitzungsliste angezeigt. Dies zeigt an, dass die Dokumente erfolgreich erstellt wurden. Wenn Sie eine 207 erhalten, konnte mindestens ein Dokument hochladen. Wenn Sie eine 404 erhalten möchten, müssen Sie einen Syntaxfehler in der Kopfzeile oder Hauptteil der Anforderung aus.

## <a name="query-the-index"></a>Abfrage des Indexes

Nachdem Sie nun einen Index und Dokumente geladen sind, können Sie Abfragen für diese ausgeben.  Klicken Sie auf der Registerkarte **Composer** wird ein **GET** -Befehl, der den Dienst fragt folgenden Screenshot ähneln.

   ![][3]

1.  Wählen Sie **Abrufen**.

2.  Geben Sie die URL, die beginnt mit HTTPS, gefolgt von Ihrem Dienst-URL, von gefolgt "/indexes/ < Indexname > / Docs?", gefolgt von Abfrageparameter. Als Beispiel verwenden Sie den folgenden URL, der Stichprobe Hostname durch einen gültigen des Diensts ersetzt werden.
    
            https://my-app.search.windows.net/indexes/hotels/docs?search=motel&facet=category&facet=rating,values:1|2|3|4|5&api-version=2015-02-28

    Diese Abfrage auf den Ausdruck "Motels" durchsucht und Vertriebsstrategie Kategorien für Bewertungen abruft.

3.  Anfrage-Header sollte identisch sein. Denken Sie daran, dass Sie den Host und api-Taste durch die Werte ersetzt, die für Ihren Dienst gültig sind.

            User-Agent: Fiddler
            host: my-app.search.windows.net
            content-type: application/json
            api-key: 1111222233334444

Der Antwortcode sollte 200 sein, und die Ausgabe der Antwort sollte ähnlich wie im folgenden Screenshot aussehen.

   ![][4]

Die folgende Beispielabfrage befindet sich in den [Suchindex Vorgang (Azure Suche API)](http://msdn.microsoft.com/library/dn798927.aspx) auf MSDN. Viele der Beispielabfragen in diesem Thema enthalten Leerzeichen, die nicht in Fiddler zulässig sind. Jede space mit ein +-Zeichen vor dem Einfügen in der Abfrage Zeichenfolge, bevor Sie versuchen, die Abfrage in Fiddler ersetzen.

**Bevor Sie Leerzeichen ersetzt werden:**

        GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate desc&api-version=2015-02-28

**Nachdem mit Leerzeichen ersetzt werden +:**

        GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate+desc&api-version=2015-02-28

## <a name="query-the-system"></a>Abfragen des Systems

Sie können auch das System zum Abrufen von Dokument zählt und Speichers Abfragen. Klicken Sie auf der Registerkarte **Composer** Anforderung sieht ungefähr wie folgt aus, und die Antwort zurückgegeben Anzahl für die Anzahl der Dokumente und Leerzeichen verwendet werden kann.

 ![][5]

1.  Wählen Sie **Abrufen**.

2.  Geben Sie die URL, die Ihre Service URL ein, gefolgt von enthält "/ Indizes/Hotels/Stats? api-Version 2015-02-28 =":

            https://my-app.search.windows.net/indexes/hotels/stats?api-version=2015-02-28

3.  Geben Sie an der Anfrage-Header, ersetzen den Host und api-Schlüssel mit Werten, die für Ihren Dienst gültig sind.

            User-Agent: Fiddler
            host: my-app.search.windows.net
            content-type: application/json
            api-key: 1111222233334444

4.  Lassen Sie den Text der Anforderung leer.

5.  Klicken Sie auf **Ausführen**. Es sollte einen HTTP 200-Code, der in der Sitzungsliste angezeigt. Wählen Sie den Eintrag, damit der Befehl veröffentlicht.

6.  Klicken Sie auf der Registerkarte **Inspektoren** , klicken Sie auf die Registerkarte **Header** , und wählen Sie dann auf das JSON-Format. Sie sollten die Anzahl und Speicher Dokumentgröße (KB) sehen.

## <a name="next-steps"></a>Nächste Schritte

Finden Sie unter [Verwalten der Suchdienst auf Azure](search-manage.md) für eine ohne Code zu verwalten und mithilfe der Suchfunktion Azure nähern.


<!--Image References-->
[1]: ./media/search-fiddler/AzureSearch_Fiddler1_PutIndex.png
[2]: ./media/search-fiddler/AzureSearch_Fiddler2_PostDocs.png
[3]: ./media/search-fiddler/AzureSearch_Fiddler3_Query.png
[4]: ./media/search-fiddler/AzureSearch_Fiddler4_QueryResults.png
[5]: ./media/search-fiddler/AzureSearch_Fiddler5_QueryStats.png

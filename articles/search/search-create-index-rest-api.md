<properties
    pageTitle="Erstellen einer Azure Suchindex verwenden die REST-API | Microsoft Azure | Cloud gehosteten Suchdienst"
    description="Erstellen eines Indexes mithilfe der Azure Suche HTTP REST-API Code an."
    services="search"
    documentationCenter=""
    authors="ashmaka"
    manager="jhubbard"
    editor=""
    tags="azure-portal"/>

<tags
    ms.service="search"
    ms.devlang="rest-api"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="create-an-azure-search-index-using-the-rest-api"></a>Erstellen einer Azure Suchindex verwenden die REST-API
> [AZURE.SELECTOR]
- [(Übersicht)](search-what-is-an-index.md)
- [Portal](search-create-index-portal.md)
- [.NET](search-create-index-dotnet.md)
- [REST](search-create-index-rest-api.md)


In diesem Artikel werden Sie den Vorgang des Erstellens eines Azure [Index](https://msdn.microsoft.com/library/azure/dn798941.aspx) verwenden die REST-API von Azure suchen durchzuführen.

Vor diesem Handbuch folgen und Erstellen eines Indexes, sollten Sie bereits [erstellt einen Azure Suchdienst](search-create-service-portal.md)verfügen.

Zum Erstellen einer Azure Suchindex verwenden die REST-API werden Sie Anforderung einer einzelne HTTP POST an Ihre Azure Suchdienst URL Endpunkt ausgeben. Die Definition des Indexes wird als wohl geformt JSON-Inhalt im Hauptteil Anforderung enthalten sein.


## <a name="i-identify-your-azure-search-services-admin-api-key"></a>Ich. Identifizieren Sie Ihrer Azure Suchdienst Admin-api-Taste
Jetzt, da Sie einen Suchdienst aus Azure bereitgestellt haben, können Sie den HTTP-Anfragen anhand des Diensts URL-Endpunkt mithilfe der REST-API ausgeben. Allerdings müssen *Alle* API Anfragen den api-Schlüssel enthalten, der für den Suchdienst generiert wurde, die Sie nach der Bereitstellung. Gibt es einen gültigen Key stellt vertrauen, auf Basis pro Anforderung, zwischen der Anwendung senden der Anfrage und der Dienst, der die Behandlung ganzer her.

1. Aufrufen des Diensts api-Schlüssel finden müssen Sie sich bei der [Azure-Portal](https://portal.azure.com/) anmelden
2. Wechseln Sie zu Ihrer Suche Azure-Service-Karte vorausgesetzt
3. Klicken Sie auf das Symbol "Tasten"

Der Dienst, *Administrator Tasten* und die *Abfrage Tasten*müssen.

 - Ihre primären und sekundären *Administrator Tasten* erteilen Vollzugriff auf alle Vorgänge, einschließlich der Berechtigung zum Verwalten des Diensts, erstellen und Löschen von Indizes, Indexer und Datenquellen. Es gibt zwei Tasten, damit Sie fortsetzen können, um die sekundäre Key zu verwenden, wenn Sie sich entscheiden, den Primärschlüssel und umgekehrt neu zu generieren.
 - Ihre *Abfrage Tasten* schreibgeschützten Zugriff auf Indizes und Dokumente gewähren, und werden in der Regel-Clientanwendungen, die Suchabfragen ausgeben verteilt.

Im Sinne des Erstellens eines Indexes, können Sie mithilfe einer der primären oder sekundären Administrator-Taste.

## <a name="ii-define-your-azure-search-index-using-well-formed-json"></a>II. Verwendung von um regelkonformes JSON Azure Suchindex definieren
Eine einzelne HTTP POST-Anforderung an den Dienst werden Ihrer Index zu erstellen. Hauptteil der HTTP POST-Anforderung wird ein einzelnes JSON-Objekt enthalten, das Ihre Azure Suchindex definiert.

1. Die erste Eigenschaft des JSON-Objekts ist der Name des Indexes.
2. Die zweite Eigenschaft dieses JSON-Objekts ist ein JSON-Array mit dem Namen `fields` , die für jedes Feld in Ihrem Index ein separates JSON-Objekt enthält. Jedes dieser Objekte JSON mehrere Namen/Wert-Paare enthalten usw. für jede der Attribute des Felds "name", einschließlich "Typ".

Es ist wichtig, dass Sie beachten Sie Ihre Suche Benutzer Erfahrung und Business-Anforderungen beim Ihrer Index zu entwerfen, wie jedes Feld die [richtigen Attribute](https://msdn.microsoft.com/library/azure/dn798941.aspx)zugewiesen werden muss. Diese Attribute-Steuerelement, das Features (filtern, Faceting, Sortierung voll-Textsuche usw.) suchen beziehen sich auf die Felder. Für ein beliebiges, die Sie nicht angeben, werden die Standardeinstellung die entsprechenden Suchfunktion aktivieren, es sei denn, Sie speziell deaktivieren.

In unserem Beispiel haben wir unserem Index "Hotels" den Namen und unsere Felder wie folgt definiert:

```JSON
{
    "name": "hotels",  
    "fields": [
        {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false, "sortable": false, "facetable": false},
        {"name": "baseRate", "type": "Edm.Double"},
        {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
        {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
        {"name": "hotelName", "type": "Edm.String", "facetable": false},
        {"name": "category", "type": "Edm.String"},
        {"name": "tags", "type": "Collection(Edm.String)"},
        {"name": "parkingIncluded", "type": "Edm.Boolean", "sortable": false},
        {"name": "smokingAllowed", "type": "Edm.Boolean", "sortable": false},
        {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
        {"name": "rating", "type": "Edm.Int32"},
        {"name": "location", "type": "Edm.GeographyPoint"}
    ]
}
```

Wir haben sorgfältig die Indexattribute für jedes Feld basierend auf wie wir denken, dass sie in einer Anwendung verwendet werden. Beispielsweise `hotelId` ist ein eindeutiger Schlüssel für Hotels wahrscheinlich nicht wissen, Suche, damit wir voll-Textsuche für dieses Feld, indem Sie festlegen deaktivieren, die Personen `searchable` auf `false`, der Speicherplatz im Index speichert.

Bitte beachten Sie die genau ein Feld in Ihrem Index vom Typ `Edm.String` muss die vorgesehenen als das Feld 'Key'.

Die obige Indexdefinition verwendet eine benutzerdefinierte Sprache Analyse für die `description_fr` Feld, da sie zum Speichern von französischen Text vorgesehen ist. Finden Sie [die Sprache auf der MSDN-Thema unterstützen](https://msdn.microsoft.com/library/azure/dn879793.aspx) sowie die entsprechenden [Blogbeitrag](https://azure.microsoft.com/blog/language-support-in-azure-search/) für Weitere Informationen zu Sprache Analyzern.

## <a name="iii-issue-the-http-request"></a>III. Die HTTP-Anforderung ausgeben
1. Verwenden die Definition des Indexes als Hauptteil der Anforderung, Emission HTTP POST-Anforderung zu Ihrer Suche Azure-Endpunkt-URL. In der URL, müssen Sie Ihren Dienstnamen als Hostname verwenden, und setzen den richtigen `api-version` als Zeichenfolge Abfrageparameter (die aktuelle Version der API ist `2015-02-28` zum Zeitpunkt der Veröffentlichung dieses Dokuments).
2. Geben Sie die Überschriften Anforderung an das `Content-Type` als `application/json`. Sie müssen außerdem des Diensts Admin Key angeben, die Sie in Schritt I in ermittelt die `api-key` Kopfzeile.


Sie müssen einen eigenen Service Name und api Schlüssel zum Ausführen die Anfrage unten angeben:


    POST https://[service name].search.windows.net/indexes?api-version=2015-02-28
    Content-Type: application/json
    api-key: [api-key]


Für eine erfolgreiche Anforderung sollte Statuscode 201 (erstellt) angezeigt werden. Weitere Informationen zum Erstellen eines Indexes über die REST-API besuchen Sie die API-Referenz auf [MSDN](https://msdn.microsoft.com/library/azure/dn798941.aspx). Weitere Informationen zu anderen HTTP-Status-Codes, die im Fall eines Fehlers zurückgegeben werden kann, finden Sie unter [HTTP Statuscodes (Azure-Suche)](https://msdn.microsoft.com/library/azure/dn798925.aspx).

Wenn Sie mit einem Index fertig sind und sie löschen möchten, nur ausgegeben Sie eine Anforderung HTTP DELETE. Dies ist z. B. wie wir "Hotels" Index löschen möchten:

    DELETE https://[service name].search.windows.net/indexes/hotels?api-version=2015-02-28
    api-key: [api-key]


## <a name="next"></a>Weiter
Nach dem Erstellen eines Indexes Azure suchen, werden Sie zum [Hochladen von Inhalten in den Index](search-what-is-data-import.md) bereit, sodass Sie beginnen können, Ihre Daten zu suchen.

<properties
    pageTitle="Abfrage mit dem .NET SDK Azure Suchindex | Microsoft Azure | Cloud gehosteten Suchdienst"
    description="Erstellen Sie eine Suchabfrage in Azure suchen und Verwenden von Suchparametern zu filtern und Sortieren von Suchergebnissen."
    services="search"
    manager="jhubbard"
    documentationCenter=""
    authors="brjohnstmsft"
/>

<tags
    ms.service="search"
    ms.devlang="dotnet"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="brjohnst"/>

# <a name="query-your-azure-search-index-using-the-net-sdk"></a>Abfrage mit dem .NET SDK Azure Suchindex
> [AZURE.SELECTOR]
- [(Übersicht)](search-query-overview.md)
- [Portal](search-explorer.md)
- [.NET](search-query-dotnet.md)
- [REST](search-query-rest-api.md)

In diesem Artikel wird die Vorgehensweise zum Abfragen eines Indexes mit dem [Azure Suche .NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx)angezeigt.

Bevor Sie beginnen diese exemplarische Vorgehensweise, sollten Sie bereits [eine Azure Suchindex erstellt](search-what-is-an-index.md) und [sie mit Daten aufgefüllt](search-what-is-data-import.md)verfügen.

Beachten Sie, dass alle Stichprobe Code in diesem Artikel in c# geschrieben ist. Sie können die vollständige Quelle suchen [auf GitHub](http://aka.ms/search-dotnet-howto)Fehlercode.

## <a name="i-identify-your-azure-search-services-query-api-key"></a>Ich. Identifizieren Sie Ihrer Azure Suchdienst Abfrage-api-Taste
Jetzt, da Sie eine Azure Suchindex erstellt haben, können Sie fast den Abfragen mit dem .NET SDK. Zunächst müssen Sie eine der Abfrage-api-Tasten, die für den Suchdienst generiert wurde, die Sie nach der Bereitstellung zu erhalten. .NET SDK wird dieser api-Taste bei jeder Anforderung an den Dienst gesendet werden. Gibt es einen gültigen Key stellt vertrauen, auf Basis pro Anforderung, zwischen der Anwendung senden der Anfrage und der Dienst, der die Behandlung ganzer her.

1. Aufrufen des Diensts api-Schlüssel finden müssen Sie sich bei der [Azure-Portal](https://portal.azure.com/) anmelden
2. Wechseln Sie zu Ihrer Suche Azure-Service-Karte vorausgesetzt
3. Klicken Sie auf das Symbol "Tasten"

Der Dienst, *Administrator Tasten* und die *Abfrage Tasten*müssen.

  - Ihre primären und sekundären *Administrator Tasten* erteilen Vollzugriff auf alle Vorgänge, einschließlich der Berechtigung zum Verwalten des Diensts, erstellen und Löschen von Indizes, Indexer und Datenquellen. Es gibt zwei Tasten, damit Sie fortsetzen können, um die sekundäre Key zu verwenden, wenn Sie sich entscheiden, den Primärschlüssel und umgekehrt neu zu generieren.
  - Ihre *Abfrage Tasten* schreibgeschützten Zugriff auf Indizes und Dokumente gewähren, und werden in der Regel-Clientanwendungen, die Suchabfragen ausgeben verteilt.

Im Sinne ein Indexes Abfragen können Sie eine Abfrage Keys verwenden. Ihr Administrator Tasten können auch für Abfragen verwendet werden, aber Sie sollten einen Abfrageschlüssel in der Anwendungscode verwenden, wie das [Prinzip der minimalen Rechte](https://en.wikipedia.org/wiki/Principle_of_least_privilege)Sie besser entspricht.

## <a name="ii-create-an-instance-of-the-searchindexclient-class"></a>II. Erstellen Sie eine Instanz der Klasse SearchIndexClient
Um Abfragen mit Azure Suche .NET SDK registrieren, müssen Sie zum Erstellen einer Instanz von der `SearchIndexClient` Class. Diese Klasse verfügt über mehrere Konstruktoren. Dauert das gewünschte Schema, Ihre Suchbegriff-Dienst, den Namen des Indexes und einer `SearchCredentials` Objekt als Parameter. `SearchCredentials`umgebrochen wird Ihre api-Taste.

Im folgenden Code wird ein neues `SearchIndexClient` für den "Hotels" Index (erstellt in [Erstellen einer Azure Suchindex mit .NET SDK](search-create-index-dotnet.md)) verwenden Sie Werte für die Suche Dienst- und api-Taste, die in der Konfigurationsdatei der Anwendung gespeichert werden (`app.config` oder `web.config`):

```csharp
string searchServiceName = ConfigurationManager.AppSettings["SearchServiceName"];
string queryApiKey = ConfigurationManager.AppSettings["SearchServiceQueryApiKey"];

SearchIndexClient indexClient = new SearchIndexClient(searchServiceName, "hotels", new SearchCredentials(queryApiKey));
```

`SearchIndexClient`verfügt über eine `Documents` Eigenschaft. Diese Eigenschaft enthält alle Methoden Azure Suche Indizes Abfragen.

## <a name="iii-query-your-index"></a>III. Das Kontaktregister Abfragen
Suche mit .NET SDK ist so einfach wie das Aufrufen der `Documents.Search` Methode auf Ihre `SearchIndexClient`. Diese Methode verwendet ein paar Parameter, einschließlich des zu suchenden Texts, zusammen mit einem `SearchParameters` Objekt, das zum Verfeinern der Abfrage verwendet werden kann.

#### <a name="types-of-queries"></a>Typen von Abfragen
Sind die beiden wichtigsten [Abfragetypen](search-query-overview.md#types-of-queries) verwenden Sie `search` und `filter`. A `search` Abfrage durchsucht alle _durchsuchbare_ Felder in Ihrem Index für einen oder mehrere Ausdrücke. A `filter` Abfrage wertet einen booleschen Ausdruck über alle _gefiltert_ Felder in einem Index.

Sowohl suchen als auch Filter mit ausgeführt werden die `Documents.Search` Methode. Eine Suchabfrage übergeben werden kann, der `searchText` Parameter, während Sie in ein Filterausdruck übergeben werden kann die `Filter` Eigenschaft der `SearchParameters` Class. Um ohne zu suchen, filtern, übergeben Sie einfach `"*"` für die `searchText` Parameter. Um ohne Filterung zu durchsuchen, lassen Sie einfach die `Filter` Eigenschaft nicht festgelegt ist, oder übergeben keiner `SearchParameters` Instanz gar fest.

#### <a name="example-queries"></a>Beispiel für Abfragen

Der folgende Code zeigt verschiedene Weise zum Abfragen von des "Hotels" Indexes [Erstellen einer Azure Suchindex mit .NET SDK](search-create-index-dotnet.md#DefineIndex)definiert. Beachten Sie, dass die Dokumente, die mit den Suchergebnissen zurückgegeben Instanzen von sind die `Hotel` Klasse, die in [Daten importieren in Azure suchen, die mit dem .NET SDK](search-import-data-dotnet.md#HotelClass)definiert wurde. Der Code nutzt ein `WriteDocuments` Methode, um die Suchergebnisse auf der Konsole ausgeben. Diese Methode ist im nächsten Abschnitt beschrieben.

```csharp
SearchParameters parameters;
DocumentSearchResult<Hotel> results;

Console.WriteLine("Search the entire index for the term 'budget' and return only the hotelName field:\n");

parameters =
    new SearchParameters()
    {
        Select = new[] { "hotelName" }
    };

results = indexClient.Documents.Search<Hotel>("budget", parameters);

WriteDocuments(results);

Console.Write("Apply a filter to the index to find hotels cheaper than $150 per night, ");
Console.WriteLine("and return the hotelId and description:\n");

parameters =
    new SearchParameters()
    {
        Filter = "baseRate lt 150",
        Select = new[] { "hotelId", "description" }
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);

Console.Write("Search the entire index, order by a specific field (lastRenovationDate) ");
Console.Write("in descending order, take the top two results, and show only hotelName and ");
Console.WriteLine("lastRenovationDate:\n");

parameters =
    new SearchParameters()
    {
        OrderBy = new[] { "lastRenovationDate desc" },
        Select = new[] { "hotelName", "lastRenovationDate" },
        Top = 2
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);

Console.WriteLine("Search the entire index for the term 'motel':\n");

parameters = new SearchParameters();
results = indexClient.Documents.Search<Hotel>("motel", parameters);

WriteDocuments(results);
```

## <a name="iv-handle-search-results"></a>IV. Behandeln von Suchergebnissen
Die `Documents.Search` Methode gibt eine `DocumentSearchResult` Objekt, das die Ergebnisse der Abfrage enthält. Das Beispiel im vorherigen Abschnitt verwendet eine Methode namens `WriteDocuments` in den Suchergebnissen auf der Konsole ausgegeben:

```csharp
private static void WriteDocuments(DocumentSearchResult<Hotel> searchResults)
{
    foreach (SearchResult<Hotel> result in searchResults.Results)
    {
        Console.WriteLine(result.Document);
    }

    Console.WriteLine();
}
```

Hier wird das Ergebnis Aussehen für die Abfragen im vorherigen Abschnitt unter der Voraussetzung, dass der Index "Hotels" mit den Beispieldaten in [Datenimport in Azure suchen, die mit dem .NET SDK](search-import-data-dotnet.md)ausgefüllt wird:

```
Search the entire index for the term 'budget' and return only the hotelName field:

Name: Roach Motel

Apply a filter to the index to find hotels cheaper than $150 per night, and return the hotelId and description:

ID: 2   Description: Cheapest hotel in town
ID: 3   Description: Close to town hall and the river

Search the entire index, order by a specific field (lastRenovationDate) in descending order, take the top two results, and show only hotelName and lastRenovationDate:

Name: Fancy Stay        Last renovated on: 6/27/2010 12:00:00 AM +00:00
Name: Roach Motel       Last renovated on: 4/28/1982 12:00:00 AM +00:00

Search the entire index for the term 'motel':

ID: 2   Base rate: 79.99        Description: Cheapest hotel in town     Description (French): Hôtel le moins cher en ville      Name: Roach Motel       Category: Budget        Tags: [motel, budget]   Parking included: yes   Smoking allowed: yes    Last renovated on: 4/28/1982 12:00:00 AM +00:00 Rating: 1/5     Location: Latitude 49.678581, longitude -122.131577

```

Im obigen Beispielcode verwendet die Konsole Suchergebnisse ausgegeben. Sie müssen entsprechend Suchergebnisse in Ihrer eigenen Anwendung anzuzeigen. [In diesem Beispiel auf GitHub](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetSample) ein Beispiel für das Rendern von Suchergebnissen in einer ASP.NET-MVC-basierten Web-Anwendung finden Sie unter.

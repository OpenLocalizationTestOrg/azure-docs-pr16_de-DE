<properties
   pageTitle="So verwenden Sie Azure Suche von einer | Microsoft Azure | Cloud gehosteten Suchdienst"
   description="So verwenden Sie Azure Suche von einer"
   services="search"
   documentationCenter=""
   authors="brjohnstmsft"
   manager="pablocas"
   editor=""/>

<tags
   ms.service="search"
   ms.devlang="dotnet"
   ms.workload="search"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.date="10/06/2016"
   ms.author="brjohnst"/>

# <a name="how-to-use-azure-search-from-a-net-application"></a>So verwenden Sie Azure Suche von einer

In diesem Artikel wird eine exemplarische Vorgehensweise zum Abrufen und Ausführen der [Azure Suche .NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx). .NET SDK können einer leistungsfähigen Suchfunktion in der Anwendung mithilfe der Suchfunktion Azure implementiert wird.

## <a name="whats-in-the-azure-search-sdk"></a>Neuigkeiten in der Azure SDK suchen

Das SDK besteht aus einer Clientbibliothek `Microsoft.Azure.Search`. Sie können zum Verwalten Ihrer Indizes, Datenquellen und Indexer, als auch hochladen und Verwalten von Dokumenten und Ausführen von Abfragen, alle ohne die Details des HTTP und JSON behandelt.

Die Client-Bibliothek definiert Klassen wie `Index`, `Field`, und `Document`, sowie Operationen wie `Indexes.Create` und `Documents.Search` auf die `SearchServiceClient` und `SearchIndexClient` Klassen. Diese Klassen sind in den folgenden Namespaces unterteilt:

- [Microsoft.Azure.Search](https://msdn.microsoft.com/library/azure/microsoft.azure.search.aspx)
- [Microsoft.Azure.Search.Models](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.aspx)

Die aktuelle Version von Azure Suche .NET SDK ist jetzt in der Regel verfügbar. Wenn Sie für uns zum Einbinden in die nächste Version Feedback geben möchten, besuchen Sie unsere [Seite "Feedback"](https://feedback.azure.com/forums/263029-azure-search/).

.NET SDK unterstützt Version `2015-02-28` der Azure Suche REST-API, auf der [MSDN-](https://msdn.microsoft.com/library/azure/dn798935.aspx)beschrieben. Diese Version umfasst jetzt Unterstützung für die Abfragesyntax Lucene und Microsoft Sprache Analyzern. Neuere Features, die *nicht* Bestandteil von dieser Version, wie etwa die Unterstützung für die `moreLikeThis` Parameter suchen, die in der [Vorschau](search-api-2015-02-28-preview.md) und im SDK noch nicht verfügbar sind. Klicken Sie auf [Suchen Dienst Versioning](https://msdn.microsoft.com/library/azure/dn864560.aspx) Status Updates manuell Features können Sie überprüfen.

Weitere Funktionen, die nicht in diesem SDK unterstützt sind:

  - [Management-Vorgänge](https://msdn.microsoft.com/library/azure/dn832684.aspx). Management-Vorgängen zählen Azure Search Services bereitgestellt, API Schlüssel verwalten. Diese werden in einer separaten Azure Suche .NET Management SDK in der Zukunft unterstützt werden.

## <a name="upgrading-to-the-latest-version-of-the-sdk"></a>Upgrade auf die neueste Version des SDK

[In diesem Artikel](search-dotnet-sdk-migration.md) wird erläutert, wenn Sie bereits eine ältere Version des Azure Suche .NET SDK verwenden und Sie möchte ein Upgrade auf die neue Version des in der Regel verfügbar, wie.

## <a name="requirements-for-the-sdk"></a>Anforderungen für das SDK

1. Visual Studio 2013 oder Visual Studio 2015.

2. Eigene Azure Suchdienst. Um das SDK verwenden zu können, benötigen Sie den Namen des Diensts und eine oder mehrere API Tasten. [Erstellen Sie einen Dienst im Portal](search-create-service-portal.md) hilft Ihnen durch die folgenden Schritte aus.

3. Herunterladen Sie Azure Suche .NET SDK [NuGet-Paket](http://www.nuget.org/packages/Microsoft.Azure.Search) mithilfe von "NuGet-Pakete verwalten" in Visual Studio. Nur für den Paketnamen suchen `Microsoft.Azure.Search` unter "NuGet.org".

Das Azure .NET SDK unterstützt Applikationen verwendet, die .NET Framework 4.5 als auch Windows Store-apps Windows 8.1 und Windows Phone 8.1 verwendet. Silverlight wird nicht unterstützt.

## <a name="core-scenarios"></a>Core Szenarien

Es gibt verschiedene Funktionen, die Sie ausführen in Ihrer Anwendung suchen müssen aus. In diesem Lernprogramm wird diese Szenarios Core Fragestellungen:

- Erstellen eines Indexes
- Füllen den Index mit Dokumenten
- Suchen nach Dokumente über voll-Textsuche und Filter

Der folgende Beispielcode veranschaulicht jede dieser. Die Codeausschnitte in Ihrer eigenen Anwendung verwenden können.

### <a name="overview"></a>(Übersicht)

Beispiel-Anwendung, die wir untersuchen werden wird erstellt eine neue Index mit dem Namen "Hotels", mit wenigen Dokumente aufgefüllt, und klicken Sie dann führt einige Suchabfragen aus. So sieht der Hauptfenster des Programms, mit den allgemeinen Informationsfluss aus:

    // This sample shows how to delete, create, upload documents and query an index
    static void Main(string[] args)
    {
        // Put your search service name here. This is the hostname portion of your service URL.
        // For example, if your service URL is https://myservice.search.windows.net, then your
        // service name is myservice.
        string searchServiceName = "myservice";

        string apiKey = "Put your API admin key here.";

        SearchServiceClient serviceClient = new SearchServiceClient(searchServiceName, new SearchCredentials(apiKey));

        Console.WriteLine("{0}", "Deleting index...\n");
        DeleteHotelsIndexIfExists(serviceClient);

        Console.WriteLine("{0}", "Creating index...\n");
        CreateHotelsIndex(serviceClient);

        ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");

        Console.WriteLine("{0}", "Uploading documents...\n");
        UploadDocuments(indexClient);

        Console.WriteLine("{0}", "Searching documents 'fancy wifi'...\n");
        SearchDocuments(indexClient, searchText: "fancy wifi");

        Console.WriteLine("\n{0}", "Filter documents with category 'Luxury'...\n");
        SearchDocuments(indexClient, searchText: "*", filter: "category eq 'Luxury'");

        Console.WriteLine("{0}", "Complete.  Press any key to end application...\n");
        Console.ReadKey();
    }

Wir werden dies Schritt für Schritt durchzuführen. Zuerst müssen wir zum Erstellen eines neuen `SearchServiceClient`. Dieses Objekt können Sie zum Verwalten von Indizes. Um eine zu erstellen, müssen Sie Ihre Azure Service Suchbegriff als auch ein Administrator API Key angeben.

        // Put your search service name here. This is the hostname portion of your service URL.
        // For example, if your service URL is https://myservice.search.windows.net, then your
        // service name is myservice.
        string searchServiceName = "myservice";

        string apiKey = "Put your API admin key here.";

        SearchServiceClient serviceClient = new SearchServiceClient(searchServiceName, new SearchCredentials(apiKey));

> [AZURE.NOTE] Wenn Sie angeben, dass einen falschen Schlüssel (beispielsweise einer Abfrageschlüssel, in denen ein Administrator Key erforderlich war), die `SearchServiceClient` löst ein `CloudException` mit dem Fehler Nachricht "Verboten" das erste Mal eine Vorgangs Methode daran, wie Aufrufen `Indexes.Create`. Wenn Sie in diesem Fall Testinstanz unsere API-Taste.

Rufen Sie die nächsten Zeilen Methoden zum Erstellen eines Indexes mit dem Namen "Hotels", es zuerst löschen, wenn sie bereits vorhanden ist. Wir werden diese Methoden etwas später durchzuführen.

        Console.WriteLine("{0}", "Deleting index...\n");
        DeleteHotelsIndexIfExists(serviceClient);

        Console.WriteLine("{0}", "Creating index...\n");
        CreateHotelsIndex(serviceClient);

Als Nächstes muss der Index aufgefüllt werden. Dazu müssen wir eine `SearchIndexClient`. Es gibt zwei Möglichkeiten, um einen: bauen es, oder durch Aufrufen `Indexes.GetClient` auf die `SearchServiceClient`. Wir verwenden zur Vereinfachung letztere.

        ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");

> [AZURE.NOTE] Index Verwaltungs- und Population wird in eine normale Suche Anwendung von einer separaten Komponente von Suchabfragen behandelt. `Indexes.GetClient`Eingabe zum Auffüllen eines Indexes, da sie die Probleme beim Bereitstellen einer anderen speichert `SearchCredentials`. Dies geschieht durch Übergabe des Administrator Schlüssels Sie zum Erstellen der `SearchServiceClient` an die neue `SearchIndexClient`. In den Teil Ihrer Anwendung, das Abfragen ausführt, es ist jedoch besser, erstellen Sie die `SearchIndexClient` direkt, damit Sie in einer Abfrageschlüssel statt ein Administrator Key übergeben können. Das Prinzip der minimalen Rechte entspricht, und hilft um Ihrer Anwendung sicherer zu machen. Sie können erfahren Sie mehr über Administrator Tasten und die Abfrage Tasten [hier](https://msdn.microsoft.com/library/azure/dn798935.aspx).

Nun wir verfügen eine `SearchIndexClient`, können wir den Index auffüllen. Dies geschieht durch eine andere Methode, die wir später durchgehen werden.

        Console.WriteLine("{0}", "Uploading documents...\n");
        UploadDocuments(indexClient);

Schließlich ein paar Suchabfragen ausführen und Anzeigen der Ergebnisse erneut mit der `SearchIndexClient`:

        Console.WriteLine("{0}", "Searching documents 'fancy wifi'...\n");
        SearchDocuments(indexClient, searchText: "fancy wifi");

        Console.WriteLine("\n{0}", "Filter documents with category 'Luxury'...\n");
        SearchDocuments(indexClient, searchText: "*", filter: "category eq 'Luxury'");

        Console.WriteLine("{0}", "Complete.  Press any key to end application...\n");
        Console.ReadKey();

Wenn Sie diese Anwendung mit einem gültigen Namen und API-Schlüssel ausführen, sollte das Ergebnis wie folgt aussehen:

    Deleting index...

    Creating index...

    Uploading documents...

    Searching documents 'fancy wifi'...

    ID: 1058-441    Name: Fancy Stay        Category: Luxury        Tags: [pool, view, concierge]
    ID: 956-532     Name: Express Rooms     Category: Budget        Tags: [wifi, budget]

    Filter documents with category 'Luxury'...

    ID: 1058-441    Name: Fancy Stay        Category: Luxury        Tags: [pool, view, concierge]
    ID: 566-518     Name: Surprisingly Expensive Suites     Category: Luxury    Tags: []
    Complete.  Press any key to end application...

Am Ende dieses Artikels wird der vollständige Quellcode der Anwendung bereitgestellt.

Als Nächstes dauert wir Detail der Methoden von aufgerufen `Main`.

### <a name="creating-an-index"></a>Erstellen eines Indexes

Nach dem Erstellen eines `SearchServiceClient`, als Nächstes `Main` bedeutet Löschen des Indexes "Hotels" ist, wenn sie bereits vorhanden ist. Indem Sie die folgende Methode abgeschlossen ist:

    private static void DeleteHotelsIndexIfExists(SearchServiceClient serviceClient)
    {
        if (serviceClient.Indexes.Exists("hotels"))
        {
            serviceClient.Indexes.Delete("hotels");
        }
    }

Diese Methode verwendet den angegebenen `SearchServiceClient` um zu überprüfen, wenn der Index vorhanden ist, und wenn Ja, löschen Sie ihn.

> [AZURE.NOTE] Der Beispielcode in diesem Artikel verwendet die synchronen Methoden der Azure Suche .NET SDK zur Vereinfachung. Es empfiehlt sich, dass Sie die asynchronen Methoden in der Anwendung verwenden, damit diese skalierbare und reagiert beibehalten werden. Angenommen, in der obigen Methode können `ExistsAsync` und `DeleteAsync` anstelle von `Exists` und `Delete`.

Nächste, `Main` erstellt einen neue "Hotels" Index, indem Sie diese Methode:

    private static void CreateHotelsIndex(SearchServiceClient serviceClient)
    {
        var definition = new Index()
        {
            Name = "hotels",
            Fields = new[]
            {
                new Field("hotelId", DataType.String)                       { IsKey = true },
                new Field("hotelName", DataType.String)                     { IsSearchable = true, IsFilterable = true },
                new Field("baseRate", DataType.Double)                      { IsFilterable = true, IsSortable = true },
                new Field("category", DataType.String)                      { IsSearchable = true, IsFilterable = true, IsSortable = true, IsFacetable = true },
                new Field("tags", DataType.Collection(DataType.String))     { IsSearchable = true, IsFilterable = true, IsFacetable = true },
                new Field("parkingIncluded", DataType.Boolean)              { IsFilterable = true, IsFacetable = true },
                new Field("lastRenovationDate", DataType.DateTimeOffset)    { IsFilterable = true, IsSortable = true, IsFacetable = true },
                new Field("rating", DataType.Int32)                         { IsFilterable = true, IsSortable = true, IsFacetable = true },
                new Field("location", DataType.GeographyPoint)              { IsFilterable = true, IsSortable = true }
            }
        };

        serviceClient.Indexes.Create(definition);
    }

Diese Methode erstellt eine neue `Index` Objekt mit einer Liste von `Field` Objekte, die das Schema der neuen Index definiert. Jedes Feld hat einen Namen, Datentyp und mehreren Attributen, die das Verhalten der Suche definieren. Zusätzlich zu Feldern können Sie auch Punktzahl Profile, Suggesters oder CORS-Optionen auf den Index hinzufügen (diese werden aus der Stichprobe aus Platzgründen weggelassen). Weitere Informationen zu den Index-Objekt und dessen Bestandteile finden Sie in der SDK-Referenz auf [MSDN](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.index_members.aspx)und [Azure Suche REST-API-Referenz](https://msdn.microsoft.com/library/azure/dn798935.aspx).

### <a name="populating-the-index"></a>Füllen den index

Die nächste Schritt im `Main` besteht darin, den neu erstellten Index zu füllen. Dies geschieht in der folgenden Methode:

    private static void UploadDocuments(ISearchIndexClient indexClient)
    {
        var documents =
            new Hotel[]
            {
                new Hotel()
                {
                    HotelId = "1058-441",
                    HotelName = "Fancy Stay",
                    BaseRate = 199.0,
                    Category = "Luxury",
                    Tags = new[] { "pool", "view", "concierge" },
                    ParkingIncluded = false,
                    LastRenovationDate = new DateTimeOffset(2010, 6, 27, 0, 0, 0, TimeSpan.Zero),
                    Rating = 5,
                    Location = GeographyPoint.Create(47.678581, -122.131577)
                },
                new Hotel()
                {
                    HotelId = "666-437",
                    HotelName = "Roach Motel",
                    BaseRate = 79.99,
                    Category = "Budget",
                    Tags = new[] { "motel", "budget" },
                    ParkingIncluded = true,
                    LastRenovationDate = new DateTimeOffset(1982, 4, 28, 0, 0, 0, TimeSpan.Zero),
                    Rating = 1,
                    Location = GeographyPoint.Create(49.678581, -122.131577)
                },
                new Hotel()
                {
                    HotelId = "970-501",
                    HotelName = "Econo-Stay",
                    BaseRate = 129.99,
                    Category = "Budget",
                    Tags = new[] { "pool", "budget" },
                    ParkingIncluded = true,
                    LastRenovationDate = new DateTimeOffset(1995, 7, 1, 0, 0, 0, TimeSpan.Zero),
                    Rating = 4,
                    Location = GeographyPoint.Create(46.678581, -122.131577)
                },
                new Hotel()
                {
                    HotelId = "956-532",
                    HotelName = "Express Rooms",
                    BaseRate = 129.99,
                    Category = "Budget",
                    Tags = new[] { "wifi", "budget" },
                    ParkingIncluded = true,
                    LastRenovationDate = new DateTimeOffset(1995, 7, 1, 0, 0, 0, TimeSpan.Zero),
                    Rating = 4,
                    Location = GeographyPoint.Create(48.678581, -122.131577)
                },
                new Hotel()
                {
                    HotelId = "566-518",
                    HotelName = "Surprisingly Expensive Suites",
                    BaseRate = 279.99,
                    Category = "Luxury",
                    ParkingIncluded = false
                }
            };

        try
        {
            var batch = IndexBatch.Upload(documents);
            indexClient.Documents.Index(batch);
        }
        catch (IndexBatchException e)
        {
            // Sometimes when your Search service is under load, indexing will fail for some of the documents in
            // the batch. Depending on your application, you can take compensating actions like delaying and
            // retrying. For this simple demo, we just log the failed document keys and continue.
            Console.WriteLine(
                "Failed to index some of the documents: {0}",
                String.Join(", ", e.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
        }

        // Wait a while for indexing to complete.
        Thread.Sleep(2000);
    }

Diese Methode besteht aus vier Teilen. Die erste erstellt ein Array von `Hotel` Objekte, die als unsere Eingabedaten Hochladen auf den Index dienen soll. Diese Daten werden zur Vereinfachung hartcodierte. In Ihrer eigenen Anwendung werden die Daten wahrscheinlich aus einer externen Datenquelle, z. B. einer SQL-Datenbank stammen.

Die zweite Komponente erstellt ein `IndexBatch` mit den Dokumenten. Geben Sie den Vorgang zum Stapel gleichzeitig anwenden Sie, in diesem Fall durch Aufrufen erstellen möchten `IndexBatch.Upload`. Klicken Sie dann der Stapel an den Suchindex von Azure durch hochgeladen wird die `Documents.Index` Methode.

> [AZURE.NOTE] In diesem Beispiel werden wir nur Dokumente hochladen. Wenn Sie Änderungen in vorhandene Dokumente zusammenführen oder Löschen von Dokumenten wollten, könnten Sie Blattnamen erstellen, indem Sie aufrufen `IndexBatch.Merge`, `IndexBatch.MergeOrUpload`, oder `IndexBatch.Delete` stattdessen. Sie können auch unterschiedliche Vorgänge in einem einzigen Stapel kombinieren, indem Sie aufrufen `IndexBatch.New`, der eine Auflistung von akzeptiert `IndexAction` Objekte, von denen jede Azure-Suche erfahren auf einen bestimmten Vorgang an einem Dokument ausführen. Sie können jede erstellen `IndexAction` mit einem eigenen Vorgang, indem Sie die entsprechende Methode wie Aufrufen `IndexAction.Merge`, `IndexAction.Upload`und so weiter.

Der dritte Teil des diese Methode ist ein CatchBlock, der einen Fall wichtige zurück, für die Indizierung behandelt. Wenn einige der Dokumente in den Stapel indizieren Azure Suchdienst keine `IndexBatchException` wird ausgelöst, indem Sie `Documents.Index`. Dies kann geschehen, wenn Sie Dokumente indizieren während des Diensts bei hoher Auslastung ist. **Es wird dringend empfohlen, explizit diesem Fall im Code behandeln.** Verzögern können, und wiederholen Sie dann auf die Dokumente, die Fehler beim Indizieren oder Sie können protokollieren und fortfahren, wie im Beispiel enthält, oder Sie können etwas anderes je nach Ihrer Anwendung Daten Konsistenz Anforderungen.

Schließlich verspätungen Methode für zwei Sekunden. Indizierung geschieht asynchrone in Ihrem Dienst Azure suchen, damit die Anwendung Stichprobe muss warten Sie kurz, um sicherzustellen, dass die Dokumente für die Suche verfügbar sind. Verspätungen wie folgt sind nur in der Regel in Demos, überprüft und Stichprobe Applikationen erforderlich.

#### <a name="how-the-net-sdk-handles-documents"></a>Wie .NET SDK Dokumente verarbeitet

Sie Fragen wie den Azure .NET SDK Instanzen einer benutzerdefinierten Klasse wie Hochladen ist `Hotel` auf den Index. Wenn Sie die Frage beantworten, sehen wir uns die `Hotel` Klasse:

    [SerializePropertyNamesAsCamelCase]
    public class Hotel
    {
        public string HotelId { get; set; }

        public string HotelName { get; set; }

        public double? BaseRate { get; set; }

        public string Category { get; set; }

        public string[] Tags { get; set; }

        public bool? ParkingIncluded { get; set; }

        public DateTimeOffset? LastRenovationDate { get; set; }

        public int? Rating { get; set; }

        public GeographyPoint Location { get; set; }

        public override string ToString()
        {
            return String.Format(
                "ID: {0}\tName: {1}\tCategory: {2}\tTags: [{3}]",
                HotelId,
                HotelName,
                Category,
                (Tags != null) ? String.Join(", ", Tags) : String.Empty);
        }
    }

Erstes zu erkennen, wird jede öffentliche Eigenschaft des `Hotel` entspricht einem Feld in der Indexdefinition, jedoch mit einem wichtigen Unterschied: der Namen der einzelnen Felder beginnt mit einem Kleinbuchstaben ("Groß-"), während Sie den Namen der einzelnen öffentlichen Eigenschaften des `Hotel` beginnt mit einem Großbuchstaben ("Pascal‑Schreibweise"). Dies ist ein allgemeines Szenario in .NET Applications, die Daten-Bindung ausführen, wo finde ich das Zielschema außerhalb des Steuerelements der Entwickler der Anwendung. Statt die Richtlinien benennen, indem Sie die Eigenschaft Namen Höckerschreibweise für .NET verletzt, können Sie erkennen, das SDK Höckerschreibweise automatisch mit die Eigenschaftennamen Zuordnen der `[SerializePropertyNamesAsCamelCase]` Attribut.

> [AZURE.NOTE] Das .NET SDK Azure verwendet die [NewtonSoft JSON.NET](http://www.newtonsoft.com/json/help/html/Introduction.htm) -Bibliothek serialisiert und Ihre benutzerdefinierten Modellobjekte an und von JSON deserialisiert. Sie können diese Serialisierung anpassen, falls erforderlich. Weitere Informationen hierzu finden Sie unter [Benutzerdefinierte Serialisierung mit JSON.NET](#JsonDotNet).
 
Die zweite wichtiger Hinweis zum der `Hotel` Klasse sind die Datentypen der öffentlichen Eigenschaften. Die .NET Datentypen dieser Eigenschaften Datentypen zugeordnet, deren entsprechende Feld in der Indexdefinition. Angenommen, die `Category` Eigenschaft Maps Zeichenfolge der `category` Feld vom Typ `Edm.String`. Es gibt ähnliche Typ Zuordnungen `bool?` und `Edm.Boolean`, `DateTimeOffset?` und `Edm.DateTimeOffset`usw.. Die bestimmten Regeln für die Zuordnung mit dokumentierten sind die `Documents.Get` Methode auf [MSDN](https://msdn.microsoft.com/library/azure/dn931291.aspx).

Die Möglichkeit, Ihre eigenen Klassen als Dokumente verwendet, funktioniert zweiseitiger; Sie können auch Suchergebnisse abzurufen und das SDK automatisch einen Typ Ihrer Wahl, diese Deserialisieren wie wir im nächsten Abschnitt angezeigt wird.

> [AZURE.NOTE] .NET SDK Azure Suche unterstützt auch dynamisch eingegebene Dokumente mithilfe der `Document` Klasse, die eine Zuordnung Schlüssel/Wert-Feldnamen auf Feldwerte ist. Dies ist in Szenarien sinnvoll, wenn Sie nicht wissen, dass das IndexSchema zur Entwurfszeit oder es zu an bestimmten Modellklassen binden wäre. Alle Methoden im SDK, die Dokumente behandelt haben überladenen, für die Arbeit mit den `Document` Klasse ebenso wie stark eingegebene überladenen, die einen generische Typparameter ausführen. Nur letztere werden im Code in diesem Lernprogramm verwendet. Sie erfahren Sie mehr über die `Document` Klasse [hier](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.document.aspx).

**Wichtiger Hinweis zur Datentypen**

Beim Entwerfen eigener Modellklassen, die eine Azure Suchindex zuordnen, wir empfehlen Eigenschaften Werttypen wie deklarieren `bool` und `int` auf NULL festgelegt werden (z. B. `bool?` anstelle von `bool`). Wenn Sie eine NULL-Eigenschaft verwenden, müssen Sie **sichergestellt ist** , dass keine Dokumente in Ihrem Index einen null-Wert für das entsprechende Feld enthalten. Weder das SDK noch Azure-Suchdienst hilft Ihnen dies zu erzwingen.

Dies ist keine hypothetische Bedeutung: Stellen Sie sich vor einem Szenario, in dem Sie einen vorhandenen Index, die vom Typ ist ein neues Feld hinzufügen `Edm.Int32`. Nach der Aktualisierung der Indexdefinition, werden alle Dokumente verfügen über einen null-Wert für das neue Feld (da alle Typen in Azure suchen auf NULL festgelegt sind). Wenn Sie eine Modellklasse dann mit einem NULL-verwenden `int` Eigenschaft für dieses Feld, erhalten Sie eine `JsonSerializationException` so wie hier, wenn Sie versuchen, die zum Abrufen von Dokumenten:

    Error converting value {null} to type 'System.Int32'. Path 'IntValue'.

Aus diesem Grund wird empfohlen, dass Sie Typen in Ihr Modellklassen als bewährte Methode verwenden.

<a name="JsonDotNet"></a>
#### <a name="custom-serialization-with-jsonnet"></a>Benutzerdefinierte Serialisierung mit JSON.NET

Das SDK verwendet JSON.NET für das Serialisieren und Deserialisieren von Dokumenten. Sie können anpassen Serialisierung und Deserialisierung bei Bedarf durch Definieren eigener `JsonConverter` oder `IContractResolver` (siehe [JSON.NET Dokumentation](http://www.newtonsoft.com/json/help/html/Introduction.htm) Weitere Details). Dies kann hilfreich sein, wenn Sie eine vorhandene Modellklasse aus Ihrer Anwendung zur Verwendung mit Azure suchen und andere erweiterten Szenarien anpassen möchten. Bei einer benutzerdefinierten Serialisierung können Sie beispielsweise:

 - Ein- oder Ausschließen bestimmter Eigenschaften der Modellklasse als Dokumentfelder gespeichert wird.
 - Zwischen Eigenschaft im Code und Feldnamen in Ihrem Index zuordnen.
 - Erstellen von benutzerdefinierten Attributen, die für sowohl Eigenschaften Dokument Felder für die Zuordnung als auch die Indexdefinition der entsprechenden erstellen verwendet werden können.

Finden Sie Beispiele für benutzerdefinierte Serialisierung in die Einheit Tests für das Azure .NET SDK auf GitHub implementieren. Ein guter Ausgangspunkt ist [diesen Ordner](https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/Search/Search.Tests/Tests/Models). Enthaltenen Klassen, indem Sie die benutzerdefinierte Serialisierung Tests verwendet werden.

### <a name="searching-for-documents-in-the-index"></a>Suchen nach Dokumente im index

Der letzte Schritt darin, in diesem Beispiel wird für einige Dokumente im Index zu suchen. Die folgende Methode macht's:

    private static void SearchDocuments(ISearchIndexClient indexClient, string searchText, string filter = null)
    {
        // Execute search based on search text and optional filter
        var sp = new SearchParameters();

        if (!String.IsNullOrEmpty(filter))
        {
            sp.Filter = filter;
        }

        DocumentSearchResult<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
        foreach (SearchResult<Hotel> result in response.Results)
        {
            Console.WriteLine(result.Document);
        }
    }

Diese Methode erstellt zunächst ein neues `SearchParameters` Objekt. Dies wird verwendet, um zusätzliche Optionen für die Abfrage z. B. sortieren, filtern, Seitennavigation und Faceting angegeben werden. In diesem Beispiel haben wir nur Festlegen der `Filter` Eigenschaft.

Im nächsten Schritt wird die Suchabfrage tatsächlich ausführen. Dies ist mit der `Documents.Search` Methode. In diesem Fall übergeben wir den zu suchenden Text als eine Zeichenfolge sowie den zuvor erstellten Suchparametern verwendet. Wir auch angeben `Hotel` als Typ-Parameter für `Documents.Search`, weist das SDK Dokumente in den Suchergebnissen in Objekte des Typs deserialisieren `Hotel`.

Diese Methode durchläuft abschließend alle Treffer in den Suchergebnissen jedes Dokument an die Konsole drucken.

Werfen Sie Detail wie diese Methode aufgerufen wird:

    SearchDocuments(indexClient, searchText: "fancy wifi");

    SearchDocuments(indexClient, searchText: "*", filter: "category eq 'Luxury'");

Im ersten Anruf sind wir für alle Dokumente, die mit der Abfrage Ausdrücke "ausgefallener" oder "Wifi" gefunden. Die zweite Anruf auf der zu suchenden Text festgelegt ist "*", was bedeutet "Alles suchen". Weitere Informationen über die Suche Abfrageausdruckssyntax finden Sie [hier](https://msdn.microsoft.com/library/azure/dn798920.aspx).

Der zweite Anruf verwendet eine OData `$filter` Ausdruck, `category eq 'Luxury'`. Dies schränkt die Suche auf Dokumente nur zurück, in dem die `category` Feld stimmt exakt mit die Zeichenfolge "Luxus". Sie können erfahren Sie mehr über die OData-Syntax, die die Azure-Suche unterstützt [hier](https://msdn.microsoft.com/library/azure/dn798921.aspx).

Nun wissen, was führen Sie diese beiden Aufrufe, sollte es einfacher sein, sehen Sie, warum die Ausgabe wie folgt aussieht:

    Searching documents 'fancy wifi'...

    ID: 1058-441    Name: Fancy Stay        Category: Luxury        Tags: [pool, view, concierge]
    ID: 956-532     Name: Express Rooms     Category: Budget        Tags: [wifi, budget]

    Filter documents with category 'Luxury'...

    ID: 1058-441    Name: Fancy Stay        Category: Luxury        Tags: [pool, view, concierge]
    ID: 566-518     Name: Surprisingly Expensive Suites     Category: Luxury    Tags: []

Bei der ersten Suche gibt zwei Dokumente. Das erste zwar der zweiten "Wifi" verfügt weist "Mit Effekten", in dem Namen der `tags` Feld. Die zweite Suche gibt zwei Dokumente geschehen, die nur die Dokumente in den Index sein, die die `category` Feld auf "Luxus" festgelegt.

Dieser Schritt abgeschlossen ist, das Lernprogramm, aber nicht hier beenden. **Nächste Schritte** enthält zusätzliche Ressourcen für Weitere Informationen zu Azure suchen.

## <a name="next-steps"></a>Nächste Schritte

- Navigieren Sie die Bezüge für [.NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx) und [REST-API](https://msdn.microsoft.com/library/azure/dn798935.aspx) auf MSDN.
- Vertiefen Sie Ihr Wissen mit [Videos und andere Beispiele und Lernprogramme](search-video-demo-tutorial-list.md)an.
- Erfahren Sie mehr über die Features und Funktionen, die in dieser Version des SDK Azure suchen: [Azure-Suche (Übersicht)](https://msdn.microsoft.com/library/azure/dn798933.aspx)
- Überprüfen Sie die [Benennungskonventionen](https://msdn.microsoft.com/library/azure/dn857353.aspx) finden Sie die Regeln für die Benennung von verschiedenen Objekten.
- Überprüfen Sie die [unterstützten Datentypen](https://msdn.microsoft.com/library/azure/dn798938.aspx) in Azure suchen.


## <a name="sample-application-source-code"></a>Beispiel für Anwendung-Quellcode

So sieht der Stichprobe Anwendung verwendet, die in dieser Einführung in der vollständigen Quellcode aus. Beachten Sie, dass Sie der Dienst- und API Key Platzhaltern in Program.cs gegebenenfalls zu erstellen, und führen Sie das Beispiel mit Ihrer eigenen Werte ersetzen müssen.

Program.cs:

```csharp
using System;
using System.Configuration;
using System.Linq;
using System.Threading;
using Microsoft.Azure.Search;
using Microsoft.Azure.Search.Models;
using Microsoft.Spatial;

namespace AzureSearch.SDKHowTo
{
    class Program
    {
        // This sample shows how to delete, create, upload documents and query an index
        static void Main(string[] args)
        {
            // Put your search service name here. This is the hostname portion of your service URL.
            // For example, if your service URL is https://myservice.search.windows.net, then your
            // service name is myservice.
            string searchServiceName = "myservice";

            string apiKey = "Put your API admin key here.";

            SearchServiceClient serviceClient = new SearchServiceClient(searchServiceName, new SearchCredentials(apiKey));

            Console.WriteLine("{0}", "Deleting index...\n");
            DeleteHotelsIndexIfExists(serviceClient);

            Console.WriteLine("{0}", "Creating index...\n");
            CreateHotelsIndex(serviceClient);

            ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");

            Console.WriteLine("{0}", "Uploading documents...\n");
            UploadDocuments(indexClient);

            Console.WriteLine("{0}", "Searching documents 'fancy wifi'...\n");
            SearchDocuments(indexClient, searchText: "fancy wifi");

            Console.WriteLine("\n{0}", "Filter documents with category 'Luxury'...\n");
            SearchDocuments(indexClient, searchText: "*", filter: "category eq 'Luxury'");

            Console.WriteLine("{0}", "Complete.  Press any key to end application...\n");
            Console.ReadKey();
        }

        private static void DeleteHotelsIndexIfExists(SearchServiceClient serviceClient)
        {
            if (serviceClient.Indexes.Exists("hotels"))
            {
                serviceClient.Indexes.Delete("hotels");
            }
        }

        private static void CreateHotelsIndex(SearchServiceClient serviceClient)
        {
            var definition = new Index()
            {
                Name = "hotels",
                Fields = new[]
                {
                    new Field("hotelId", DataType.String)                       { IsKey = true },
                    new Field("hotelName", DataType.String)                     { IsSearchable = true, IsFilterable = true },
                    new Field("baseRate", DataType.Double)                      { IsFilterable = true, IsSortable = true },
                    new Field("category", DataType.String)                      { IsSearchable = true, IsFilterable = true, IsSortable = true, IsFacetable = true },
                    new Field("tags", DataType.Collection(DataType.String))     { IsSearchable = true, IsFilterable = true, IsFacetable = true },
                    new Field("parkingIncluded", DataType.Boolean)              { IsFilterable = true, IsFacetable = true },
                    new Field("lastRenovationDate", DataType.DateTimeOffset)    { IsFilterable = true, IsSortable = true, IsFacetable = true },
                    new Field("rating", DataType.Int32)                         { IsFilterable = true, IsSortable = true, IsFacetable = true },
                    new Field("location", DataType.GeographyPoint)              { IsFilterable = true, IsSortable = true }
                }
            };

            serviceClient.Indexes.Create(definition);
        }

        private static void UploadDocuments(ISearchIndexClient indexClient)
        {
            var documents =
                new Hotel[]
                {
                    new Hotel()
                    {
                        HotelId = "1058-441",
                        HotelName = "Fancy Stay",
                        BaseRate = 199.0,
                        Category = "Luxury",
                        Tags = new[] { "pool", "view", "concierge" },
                        ParkingIncluded = false,
                        LastRenovationDate = new DateTimeOffset(2010, 6, 27, 0, 0, 0, TimeSpan.Zero),
                        Rating = 5,
                        Location = GeographyPoint.Create(47.678581, -122.131577)
                    },
                    new Hotel()
                    {
                        HotelId = "666-437",
                        HotelName = "Roach Motel",
                        BaseRate = 79.99,
                        Category = "Budget",
                        Tags = new[] { "motel", "budget" },
                        ParkingIncluded = true,
                        LastRenovationDate = new DateTimeOffset(1982, 4, 28, 0, 0, 0, TimeSpan.Zero),
                        Rating = 1,
                        Location = GeographyPoint.Create(49.678581, -122.131577)
                    },
                    new Hotel()
                    {
                        HotelId = "970-501",
                        HotelName = "Econo-Stay",
                        BaseRate = 129.99,
                        Category = "Budget",
                        Tags = new[] { "pool", "budget" },
                        ParkingIncluded = true,
                        LastRenovationDate = new DateTimeOffset(1995, 7, 1, 0, 0, 0, TimeSpan.Zero),
                        Rating = 4,
                        Location = GeographyPoint.Create(46.678581, -122.131577)
                    },
                    new Hotel()
                    {
                        HotelId = "956-532",
                        HotelName = "Express Rooms",
                        BaseRate = 129.99,
                        Category = "Budget",
                        Tags = new[] { "wifi", "budget" },
                        ParkingIncluded = true,
                        LastRenovationDate = new DateTimeOffset(1995, 7, 1, 0, 0, 0, TimeSpan.Zero),
                        Rating = 4,
                        Location = GeographyPoint.Create(48.678581, -122.131577)
                    },
                    new Hotel()
                    {
                        HotelId = "566-518",
                        HotelName = "Surprisingly Expensive Suites",
                        BaseRate = 279.99,
                        Category = "Luxury",
                        ParkingIncluded = false
                    }
                };

            try
            {
                var batch = IndexBatch.Upload(documents);
                indexClient.Documents.Index(batch);
            }
            catch (IndexBatchException e)
            {
                // Sometimes when your Search service is under load, indexing will fail for some of the documents in
                // the batch. Depending on your application, you can take compensating actions like delaying and
                // retrying. For this simple demo, we just log the failed document keys and continue.
                Console.WriteLine(
                    "Failed to index some of the documents: {0}",
                    String.Join(", ", e.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
            }

            // Wait a while for indexing to complete.
            Thread.Sleep(2000);
        }

        private static void SearchDocuments(ISearchIndexClient indexClient, string searchText, string filter = null)
        {
            // Execute search based on search text and optional filter
            var sp = new SearchParameters();

            if (!String.IsNullOrEmpty(filter))
            {
                sp.Filter = filter;
            }

            DocumentSearchResult<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
            foreach (SearchResult<Hotel> result in response.Results)
            {
                Console.WriteLine(result.Document);
            }
        }
    }
}
```

Hotel.cs:

```csharp
using System;
using Microsoft.Azure.Search.Models;
using Microsoft.Spatial;

namespace AzureSearch.SDKHowTo
{
    [SerializePropertyNamesAsCamelCase]
    public class Hotel
    {
        public string HotelId { get; set; }

        public string HotelName { get; set; }

        public double? BaseRate { get; set; }

        public string Category { get; set; }

        public string[] Tags { get; set; }

        public bool? ParkingIncluded { get; set; }

        public DateTimeOffset? LastRenovationDate { get; set; }

        public int? Rating { get; set; }

        public GeographyPoint Location { get; set; }

        public override string ToString()
        {
            return String.Format(
                "ID: {0}\tName: {1}\tCategory: {2}\tTags: [{3}]",
                HotelId,
                HotelName,
                Category,
                (Tags != null) ? String.Join(", ", Tags) : String.Empty);
        }
    }
}
```

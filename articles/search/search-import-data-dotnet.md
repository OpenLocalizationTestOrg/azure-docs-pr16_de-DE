<properties
    pageTitle="Hochladen von Daten in Azure suchen, die mit dem .NET SDK | Microsoft Azure | Cloud gehosteten Suchdienst"
    description="Erfahren Sie, wie Daten zu einem Index in Azure suchen, die mit dem .NET SDK hochladen."
    services="search"
    documentationCenter=""
    authors="brjohnstmsft"
    manager="jhubbard"
    editor=""
    tags=""/>

<tags
    ms.service="search"
    ms.devlang="dotnet"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="brjohnst"/>

# <a name="upload-data-to-azure-search-using-the-net-sdk"></a>Hochladen von Daten in mithilfe des .NET SDK Azure-Suche
> [AZURE.SELECTOR]
- [(Übersicht)](search-what-is-data-import.md)
- [.NET](search-import-data-dotnet.md)
- [REST](search-import-data-rest-api.md)

In diesem Artikel wird gezeigt, wie das [Azure Suche .NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx) zum Importieren von Daten in einer Azure Suchindex verwenden werden.

Bevor Sie beginnen diese exemplarische Vorgehensweise, sollten Sie bereits [erstellt eine Azure Suchindex](search-what-is-an-index.md)verfügen. In diesem Artikel wird vorausgesetzt, dass Sie bereits erstellt haben eine `SearchServiceClient` Objekt, wie in [Erstellen einer Azure Suchindex mit .NET SDK](search-create-index-dotnet.md#CreateSearchServiceClient)dargestellt.

Beachten Sie, dass alle Stichprobe Code in diesem Artikel in c# geschrieben ist. Sie können die vollständige Quelle suchen [auf GitHub](http://aka.ms/search-dotnet-howto)Fehlercode.

Um das mit dem .NET SDK Kontaktregister Dokumente abzulegen, müssen Sie:

  1. Erstellen einer `SearchIndexClient` Objekt Verbindung zum Suchindex.
  2. Erstellen einer `IndexBatch` mit den Dokumenten hinzugefügt werden soll, geändert oder gelöscht.
  3. Rufen Sie die `Documents.Index` Methode zum Ihrer `SearchIndexClient` zum Senden der `IndexBatch` zum Suchindex.

## <a name="i-create-an-instance-of-the-searchindexclient-class"></a>Ich. Erstellen Sie eine Instanz der Klasse SearchIndexClient
Zum Importieren von Daten in Ihr Index mit Azure Suche .NET SDK müssen Sie zum Erstellen einer Instanz von der `SearchIndexClient` Class. Können Sie erstellen diese Instanz selbst, aber es einfacher, wenn Sie bereits eine `SearchServiceClient` Instanz aufrufen, deren `Indexes.GetClient` Methode. Hier beispielsweise, wie Sie erhalten würden einer `SearchIndexClient` für den Index namens "Hotels" aus einer `SearchServiceClient` mit dem Namen `serviceClient`:

```csharp
SearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

> [AZURE.NOTE] Index Verwaltungs- und Population wird in eine normale Suche Anwendung von einer separaten Komponente von Suchabfragen behandelt. `Indexes.GetClient`Eingabe zum Auffüllen eines Indexes, da sie die Probleme beim Bereitstellen einer anderen speichert `SearchCredentials`. Dies geschieht durch Übergabe des Administrator Schlüssels Sie zum Erstellen der `SearchServiceClient` an die neue `SearchIndexClient`. In den Teil Ihrer Anwendung, das Abfragen ausführt, es ist jedoch besser, erstellen Sie die `SearchIndexClient` direkt, damit Sie in einer Abfrageschlüssel statt ein Administrator Key übergeben können. Dies ist das [Prinzip der minimalen Rechte](https://en.wikipedia.org/wiki/Principle_of_least_privilege) konsistente und hilft um Ihrer Anwendung sicherer zu machen. Sie können weitere Informationen zu Admin-Tasten und die Abfrage Tasten [Azure Suche REST-API-Referenz auf MSDN](https://msdn.microsoft.com/library/azure/dn798935.aspx)suchen.

`SearchIndexClient`verfügt über eine `Documents` Eigenschaft. Diese Eigenschaft enthält alle Methoden, die Sie hinzufügen, ändern, löschen oder Abfragen von Dokumenten in Ihrem Index müssen.

## <a name="ii-decide-which-indexing-action-to-use"></a>II. Entscheiden Sie, welche Indizierung Aktion verwenden
Zum Importieren von Daten mit dem .NET SDK müssen Sie Ihre Daten in Verpacken einer `IndexBatch` Objekt. Eine `IndexBatch` kapselt eine Auflistung von `IndexAction` Objekte, von denen jede enthält ein Dokument und eine Eigenschaft, die Suche Azure auszuführende Aktion an für dieses Dokument (hochladen, zusammenführen, löschen usw.) erfahren. Je nachdem, welche die unter Aktionen Sie sich entscheiden, nur bestimmte Felder für jedes Dokument einbezogen werden soll:

Aktion | Beschreibung | Für jedes Dokument erforderlichen Felder | Notizen
--- | --- | --- | ---
`Upload` | Eine `Upload` Aktion ist vergleichbar mit einer "Upsert", wo wird das Dokument eingefügt wird, wenn es neu ist und aktualisiert/ersetzt, wenn sie vorhanden ist. | Schlüssel sowie alle anderen Felder, die Sie definieren möchten. | Beim Aktualisieren/eines vorhandenen Dokuments ersetzen haben jedes Feld, das nicht in der Besprechungsanfrage angegeben wird deren Feld auf `null`. Dies geschieht, selbst wenn das Feld zuvor ein nicht-Null-Wert festgelegt wurde.
`Merge` | Aktualisiert ein vorhandenes Dokument mit den angegebenen Feldern. Wenn das Dokument im Index nicht vorhanden ist, wird die Zusammenführung fehl. | Schlüssel sowie alle anderen Felder, die Sie definieren möchten. | Ein beliebiges Feld in einem Seriendruck angegebenen ersetzt das bestehende Feld im Dokument. Dies umfasst die Felder vom Typ `DataType.Collection(DataType.String)`. Beispielsweise, wenn das Dokument ein Feld enthält `tags` mit dem Wert `["budget"]` , und führen Sie einen Seriendruck mit dem Wert `["economy", "pool"]` für `tags`, der Endwert von der `tags` Feld `["economy", "pool"]`. Es werden keine `["budget", "economy", "pool"]`.
`MergeOrUpload` | Diese Aktion verhält sich wie `Merge` Wenn ein Dokument mit dem angegebenen Schlüssel im Index bereits vorhanden ist. Wenn das Dokument nicht vorhanden ist, wird er verhält sich wie `Upload` ein neues Dokument. | Schlüssel sowie alle anderen Felder, die Sie definieren möchten. | -
`Delete` | Das angegebene Dokument entfernt aus dem Index. | nur Schlüssel | Alle Felder Geben Sie mit anderen als Feld mit das ignoriert wird. Wenn Sie ein einzelnes Feld aus einem Dokument entfernen möchten, verwenden Sie `Merge` stattdessen und einfach das Feld explizit auf null festgelegt.

Sie können angeben, welche Aktion, die Sie mit den verschiedenen statischen Methoden der verwenden möchten die `IndexBatch` und `IndexAction` Klassen, wie im nächsten Abschnitt dargestellt.

## <a name="iii-construct-your-indexbatch"></a>III. Erstellen Sie Ihre IndexBatch
Nun, welche Aktionen für Ihre Dokumente durchzuführen wissen, sind Sie bereit zum Erstellen der `IndexBatch`. Im folgenden Beispiel wird gezeigt, wie zu mit ein paar andere Aktionen erstellen. Beachten Sie, dass ein Beispiel eine benutzerdefinierte Klasse namens verwendet `Hotel` , die zu einem Dokument in der "Hotels" Index zugeordnet ist.

```csharp
var actions =
    new IndexAction<Hotel>[]
    {
        IndexAction.Upload(
            new Hotel()
            {
                HotelId = "1",
                BaseRate = 199.0,
                Description = "Best hotel in town",
                DescriptionFr = "Meilleur hôtel en ville",
                HotelName = "Fancy Stay",
                Category = "Luxury",
                Tags = new[] { "pool", "view", "wifi", "concierge" },
                ParkingIncluded = false,
                SmokingAllowed = false,
                LastRenovationDate = new DateTimeOffset(2010, 6, 27, 0, 0, 0, TimeSpan.Zero),
                Rating = 5,
                Location = GeographyPoint.Create(47.678581, -122.131577)
            }),
        IndexAction.Upload(
            new Hotel()
            {
                HotelId = "2",
                BaseRate = 79.99,
                Description = "Cheapest hotel in town",
                DescriptionFr = "Hôtel le moins cher en ville",
                HotelName = "Roach Motel",
                Category = "Budget",
                Tags = new[] { "motel", "budget" },
                ParkingIncluded = true,
                SmokingAllowed = true,
                LastRenovationDate = new DateTimeOffset(1982, 4, 28, 0, 0, 0, TimeSpan.Zero),
                Rating = 1,
                Location = GeographyPoint.Create(49.678581, -122.131577)
            }),
        IndexAction.MergeOrUpload(
            new Hotel()
            {
                HotelId = "3",
                BaseRate = 129.99,
                Description = "Close to town hall and the river"
            }),
        IndexAction.Delete(new Hotel() { HotelId = "6" })
    };

var batch = IndexBatch.New(actions);
```

In diesem Fall verwenden wir `Upload`, `MergeOrUpload`, und `Delete` als unsere Aktionen suchen gemäß Angabe der Methoden, auf die `IndexAction` Class.

Wird davon ausgegangen Sie, dass dieses Beispiel "Hotels" Index bereits mit einer Reihe von Dokumenten gefüllt wird. Beachten Sie, wie wir keinen alle möglichen Dokumentfelder angeben bei Verwendung von `MergeOrUpload` und wie die Taste Dokument nur angegeben (`HotelId`) bei Verwendung von `Delete`.

Beachten Sie, dass nur maximal 1000 Dokumente in einer einzigen Indizierung Anforderung enthalten sein können.

> [AZURE.NOTE] In diesem Beispiel werden wir verschiedene Aktionen zu anderen Dokumenten anwenden. Wenn Sie die gleichen Aktionen ausführen, die allen Dokumenten im Stapel, statt aufrufen wollten `IndexBatch.New`, Sie können die statischen Methoden der `IndexBatch`. Beispielsweise könnten Sie Blattnamen erstellen, indem Sie aufrufen `IndexBatch.Merge`, `IndexBatch.MergeOrUpload`, oder `IndexBatch.Delete`. Diese Methoden nehmen eine Sammlung von Dokumenten (Objekte vom Typ `Hotel` in diesem Beispiel) anstelle von `IndexAction` Objekte.

## <a name="iv-import-data-to-the-index"></a>IV. Importieren von Daten in den index
Nachdem Sie nun ein initialisiertes stehen Ihnen `IndexBatch` Objekt aufrufen, indem Sie auf den Index senden `Documents.Index` auf Ihre `SearchIndexClient` Objekt. Im folgenden Beispiel wird gezeigt, wie Nummer `Index`sowie als einige zusätzliche Schritte müssen Sie zum Ausführen:

```csharp
try
{
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

Console.WriteLine("Waiting for documents to be indexed...\n");
Thread.Sleep(2000);
```

Hinweis Die `try` / `catch` umgebenden den Anruf an die `Index` Methode. CatchBlock übernimmt einen wichtigen Fehler Fall für die Indizierung. Wenn einige der Dokumente in den Stapel indizieren Azure Suchdienst keine `IndexBatchException` wird ausgelöst, indem Sie `Documents.Index`. Dies kann geschehen, wenn Sie Dokumente indizieren während des Diensts bei hoher Auslastung ist. **Es wird dringend empfohlen, explizit diesem Fall im Code behandeln.** Verzögern können, und wiederholen Sie dann auf die Dokumente, die Fehler beim Indizieren oder Sie können protokollieren und fortfahren, wie im Beispiel enthält, oder Sie können etwas anderes je nach Ihrer Anwendung Daten Konsistenz Anforderungen.

Schließlich den Code im Beispiel oben für zwei Sekunden verzögert. Indizierung geschieht asynchrone in Ihrem Dienst Azure suchen, damit die Anwendung Stichprobe muss warten Sie kurz, um sicherzustellen, dass die Dokumente für die Suche verfügbar sind. Verspätungen wie folgt sind nur in der Regel in Demos, überprüft und Stichprobe Applikationen erforderlich.

<a name="HotelClass"></a>
### <a name="how-the-net-sdk-handles-documents"></a>Wie .NET SDK Dokumente verarbeitet

Sie Fragen wie den Azure .NET SDK Instanzen einer benutzerdefinierten Klasse wie Hochladen ist `Hotel` auf den Index. Wenn Sie die Frage beantworten, sehen wir uns die `Hotel` Klasse, die im [Erstellen einer Azure Suchindex mit .NET SDK](search-create-index-dotnet.md#DefineIndex)definierten IndexSchema zugeordnet ist:

```csharp
[SerializePropertyNamesAsCamelCase]
public partial class Hotel
{
    public string HotelId { get; set; }

    public double? BaseRate { get; set; }

    public string Description { get; set; }

    [JsonProperty("description_fr")]
    public string DescriptionFr { get; set; }

    public string HotelName { get; set; }

    public string Category { get; set; }

    public string[] Tags { get; set; }

    public bool? ParkingIncluded { get; set; }

    public bool? SmokingAllowed { get; set; }

    public DateTimeOffset? LastRenovationDate { get; set; }

    public int? Rating { get; set; }

    public GeographyPoint Location { get; set; }

    // ToString() method omitted for brevity...
}
```

Erstes zu erkennen, wird jede öffentliche Eigenschaft des `Hotel` entspricht einem Feld in der Indexdefinition, jedoch mit einem wichtigen Unterschied: der Namen der einzelnen Felder beginnt mit einem Kleinbuchstaben ("Groß-"), während Sie den Namen der einzelnen öffentlichen Eigenschaften des `Hotel` beginnt mit einem Großbuchstaben ("Pascal‑Schreibweise"). Dies ist ein allgemeines Szenario in .NET Applications, die Daten-Bindung ausführen, wo finde ich das Zielschema außerhalb des Steuerelements der Entwickler der Anwendung. Statt die Richtlinien benennen, indem Sie die Eigenschaft Namen Höckerschreibweise für .NET verletzt, können Sie erkennen, das SDK Höckerschreibweise automatisch mit die Eigenschaftennamen Zuordnen der `[SerializePropertyNamesAsCamelCase]` Attribut.

> [AZURE.NOTE] Das .NET SDK Azure verwendet die [NewtonSoft JSON.NET](http://www.newtonsoft.com/json/help/html/Introduction.htm) -Bibliothek serialisiert und Ihre benutzerdefinierten Modellobjekte an und von JSON deserialisiert. Sie können diese Serialisierung anpassen, falls erforderlich. Weitere Informationen hierzu finden Sie in [Aktualisieren auf Azure Suche .NET SDK, Version 1.1](search-dotnet-sdk-migration.md#WhatsNew). Ein Beispiel dafür ist die Verwendung von der `[JsonProperty]` Attribut für die `DescriptionFr` Eigenschaft im obigen Beispielcode.

Die zweite wichtiger Hinweis zum der `Hotel` Klasse sind die Datentypen der öffentlichen Eigenschaften. Die .NET Datentypen dieser Eigenschaften Datentypen zugeordnet, deren entsprechende Feld in der Indexdefinition. Angenommen, die `Category` Eigenschaft Maps Zeichenfolge der `category` Feld vom Typ `DataType.String`. Es gibt ähnliche Typ Zuordnungen `bool?` und `DataType.Boolean`, `DateTimeOffset?` und `DataType.DateTimeOffset`usw.. Die bestimmten Regeln für die Zuordnung mit dokumentierten sind die `Documents.Get` Methode auf [MSDN](https://msdn.microsoft.com/library/azure/dn931291.aspx).

Die Möglichkeit, Ihre eigenen Klassen als Dokumente verwendet, funktioniert zweiseitiger; Sie können auch Suchergebnisse abrufen und das SDK automatisch einen Typ Ihrer Wahl, diese Deserialisieren wie im [nächsten Artikel](search-query-dotnet.md)dargestellt.

> [AZURE.NOTE] .NET SDK Azure Suche unterstützt auch dynamisch eingegebene Dokumente mithilfe der `Document` Klasse, die eine Zuordnung Schlüssel/Wert-Feldnamen auf Feldwerte ist. Dies ist in Szenarien sinnvoll, wenn Sie nicht wissen, dass das IndexSchema zur Entwurfszeit oder es zu an bestimmten Modellklassen binden wäre. Alle Methoden im SDK, die Dokumente behandelt haben überladenen, für die Arbeit mit den `Document` Klasse ebenso wie stark eingegebene überladenen, die einen generische Typparameter ausführen. Im Code in diesem Artikel werden nur letztere verwendet. Sie erfahren Sie mehr über die `Document` [auf der MSDN-](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.document.aspx)Klasse.

**Wichtiger Hinweis zur Datentypen**

Beim Entwerfen eigener Modellklassen, die eine Azure Suchindex zuordnen, wir empfehlen Eigenschaften Werttypen wie deklarieren `bool` und `int` auf NULL festgelegt werden (z. B. `bool?` anstelle von `bool`). Wenn Sie eine NULL-Eigenschaft verwenden, müssen Sie **sichergestellt ist** , dass keine Dokumente in Ihrem Index einen null-Wert für das entsprechende Feld enthalten. Weder das SDK noch Azure-Suchdienst hilft Ihnen dies zu erzwingen.

Dies ist keine hypothetische Bedeutung: Stellen Sie sich vor einem Szenario, in dem Sie einen vorhandenen Index, die vom Typ ist ein neues Feld hinzufügen `DataType.Int32`. Nach der Aktualisierung der Indexdefinition, werden alle Dokumente verfügen über einen null-Wert für das neue Feld (da alle Typen in Azure suchen auf NULL festgelegt sind). Wenn Sie eine Modellklasse dann mit einem NULL-verwenden `int` Eigenschaft für dieses Feld, erhalten Sie eine `JsonSerializationException` so wie hier, wenn Sie versuchen, die zum Abrufen von Dokumenten:

    Error converting value {null} to type 'System.Int32'. Path 'IntValue'.

Aus diesem Grund wird empfohlen, dass Sie Typen in Ihr Modellklassen als bewährte Methode verwenden.

## <a name="next"></a>Weiter
Nach Auffüllen Azure Suchindex, werden Sie beginnen ausgeben Abfragen, um Dokumente zu suchen. Details finden Sie in [Ihrer Abfrage Azure Suchindex](search-query-overview.md) .

<properties
   pageTitle="Upgrade auf Azure Suche .NET SDK, Version 1.1 | Microsoft Azure | Cloud gehosteten Suchdienst"
   description="Upgrade auf Azure Suche .NET SDK, Version 1.1"
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
   ms.date="08/15/2016"
   ms.author="brjohnst"/>

# <a name="upgrading-to-the-azure-search-net-sdk-version-20-preview"></a>Upgrade auf die Azure Suche .NET SDK Version 2.0-Vorschau

Wenn Sie Version 1.1 oder ältere des [Azure Suche .NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx)verwenden, hilft Ihnen in diesem Artikel Sie die Anwendung auf die nächste Hauptversion 2.0-Vorschau verwenden aktualisieren.

Eine weitere allgemeine Exemplarische Vorgehensweise des SDK samt Beispielen finden Sie unter [Azure Suche von einer verwenden](search-howto-dotnet-sdk.md).

Version 2.0-Vorschau von .NET SDK Azure Suche enthält einige Änderungen aus früheren Versionen. Dies sind hauptsächlich Nebenversionen, so ändern den Code nur minimalen Aufwand erforderlich sein soll. [Schritte zum upgrade](#UpgradeSteps) finden Sie Anweisungen zum Ändern von Codes für die neue SDK-Version verwenden.

> [AZURE.NOTE] Wenn Sie Version 0.13-Vorschau oder älter verwenden, sollten Sie auf die Version 1.1 vor- und dann ein Upgrade auf 2.0-Vorschau aktualisieren. Finden Sie unter [Anhang: Schritte zum upgrade auf Version 1.1](#UpgradeStepsV1) Anweisungen.

<a name="WhatsNew"></a>
## <a name="whats-new-in-version-20-preview"></a>Was ist neu in der Version 2.0-Vorschau

Version 2.0-Vorschau ist die erste Version des Azure Suche .NET SDK eine Vorschauversion von Azure Suche REST API, insbesondere 2015-02-28-Vorschau werden sollen. Hierdurch können viele Preview-Features von Azure Suche von einer, einschließlich der folgenden verwenden:

- [Benutzerdefinierte Analyzern](https://aka.ms/customanalyzers)
- [Azure BLOB-Speicher](search-howto-indexing-azure-blob-storage.md) und [Azure Table Storage](search-howto-indexing-azure-tables.md) Indexer-support
- Indexer Anpassung über [Feld Zuordnungen](search-indexer-field-mappings.md)
- ETags support bereit, sicherer gleichzeitige Aktualisieren des Indexdefinitionen, Indexer und Datenquellen
- Unterstützung für .NET Core und .NET tragbaren Profile 111

<a name="UpgradeSteps"></a>
## <a name="steps-to-upgrade"></a>Schritte zum Aktualisieren

Aktualisieren Sie zunächst den Verweis NuGet für `Microsoft.Azure.Search` mithilfe der NuGet-Paket-Manager-Konsole oder von mit der rechten Maustaste auf Ihr Projektverweise und "Verwalten von NuGet-Paketen..." in Visual Studio auswählen. Sicherstellen, dass Sie Vorabversion Pakete, indem Sie in der NuGet "Pakete verwalten" auswählen "Vorabversion einschließen" aktivieren Fenster in Visual Studio oder mithilfe der `-IncludePrerelease` wechseln, wenn Sie NuGet-Paket-Manager-Konsole verwenden.

Nachdem NuGet die neuen Pakete und deren abhängigen Dateien heruntergeladen, erstellen Sie Ihr Projekt neu. Je nach der Strukturierung von Codes kann es erfolgreich neu erstellen. In diesem Fall sind Sie bereit sind, wechseln Sie!

Wenn Ihre eigene fehlschlägt, sollten Sie einen eigene Fehler wie den folgenden sehen:

    Program.cs(31,45,31,86): error CS0266: Cannot implicitly convert type 'Microsoft.Azure.Search.ISearchIndexClient' to 'Microsoft.Azure.Search.SearchIndexClient'. An explicit conversion exists (are you missing a cast?)

Im nächsten Schritt wird zum Beheben dieses Fehlers erstellen. Details zur Ursache des Fehlers ermitteln kann und wie Sie das Problem zu lösen finden Sie in der [Bruchfestigkeit Änderungen in der Version 2.0-Vorschau](#ListOfChanges) .

Möglicherweise werden zusätzliche Buildwarnungen im Zusammenhang mit veralteten Methoden oder Eigenschaften angezeigt. Die Warnungen werden Anweisungen was verwenden Sie statt des nicht mehr unterstützter Features berücksichtigen. Beispielsweise, wenn die Anwendung verwendet die `SearchRequestOptions.RequestId` Eigenschaft, sollten Sie eine Warnung, die besagt, erhalten`"This property is deprecated. Please use ClientRequestId instead."`

Nachdem Sie alle Buildfehler korrigiert haben, können Sie nehmen Sie Änderungen an Ihrer Anwendung zu den neuen Funktionen nutzen, wenn Sie möchten. Neue Features in der SDK werden in [Was ist neu in der Version 2.0-Vorschau](#WhatsNew)detailliert beschrieben.

<a name="ListOfChanges"></a>
## <a name="breaking-changes-in-version-20-preview"></a>Unterbrechen der Änderungen in der Version 2.0-Vorschau

Es gibt nur eine wichtige Änderung in Version 2.0-Vorschau, die über das Neuerstellen Ihrer Anwendungs zu gelangen.

### <a name="indexesgetclient-return-type"></a>Rückgabetyp Indexes.GetClient

Die `Indexes.GetClient` Methode hat einen neuen Rückgabetyp. Früher wurde zurückgegeben `SearchIndexClient`, aber dies wurde geändert in `ISearchIndexClient` in Version 2.0-Vorschau. Dies ist zur Unterstützung von Kunden, die simulieren Sie möchten die `GetClient` Methode für Einheit Tests durch Zurückgeben einer simulierten Implementierung von `ISearchIndexClient`.

#### <a name="example"></a>Beispiel

Wenn der Code wie folgt aussieht:

```csharp
SearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

Sie können sie diese Option, um das Beheben von Fehlern erstellen ändern:

```csharp
ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

## <a name="conclusion"></a>Abschluss
Wenn Sie weitere Informationen zur Verwendung von Azure Suche .NET SDK benötigen, finden Sie unter unseren zuletzt aktualisierten- [unterstützenden](search-howto-dotnet-sdk.md).

Willkommen wir Ihr Feedback, klicken Sie auf das SDK. Wenn Probleme auftreten, können Sie uns im [Forum im MSDN Azure-Suche](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=azuresearch)um Hilfe bitten. Wenn Sie einen Fehler finden, können Sie ein Problem im [Azure .NET SDK GitHub Repository](https://github.com/Azure/azure-sdk-for-net/issues)ablegen. Vergewissern Sie sich an Ihren Problemtitel mit Präfix "Suche SDK:".

Vielen Dank für die Verwendung der Suchfunktion Azure!

<a name="UpgradeStepsV1"></a>
## <a name="appendix-steps-to-upgrade-to-version-11"></a>Anlage: Schritte auf Version 1.1 aktualisieren 

> [AZURE.NOTE] Dieser Abschnitt gilt nur für Benutzer von Azure Suche .NET SDK Version 0.13-Vorschau und ältere.

Aktualisieren Sie zunächst den Verweis NuGet für `Microsoft.Azure.Search` mithilfe der NuGet-Paket-Manager-Konsole oder von mit der rechten Maustaste auf Ihr Projektverweise und "Verwalten von NuGet-Paketen..." in Visual Studio auswählen.

Nachdem NuGet die neuen Pakete und deren abhängigen Dateien heruntergeladen, erstellen Sie Ihr Projekt neu.

Wenn Sie vorher Version 1.0.0-preview, 1.0.1-preview oder 1.0.2-preview verwendet haben, sollte der Build erfolgreich ausgeführt werden, und Sie bereit sind, zu wechseln!

Wenn Sie vorher Version 0.13.0-preview verwendet haben oder älter, sollte angezeigt werden Fehler wie den folgenden erstellen:

    Program.cs(137,56,137,62): error CS0117: 'Microsoft.Azure.Search.Models.IndexBatch' does not contain a definition for 'Create'
    Program.cs(137,99,137,105): error CS0117: 'Microsoft.Azure.Search.Models.IndexAction' does not contain a definition for 'Create'
    Program.cs(146,41,146,54): error CS1061: 'Microsoft.Azure.Search.IndexBatchException' does not contain a definition for 'IndexResponse' and no extension method 'IndexResponse' accepting a first argument of type 'Microsoft.Azure.Search.IndexBatchException' could be found (are you missing a using directive or an assembly reference?)
    Program.cs(163,13,163,42): error CS0246: The type or namespace name 'DocumentSearchResponse' could not be found (are you missing a using directive or an assembly reference?)

Im nächsten Schritt wird die Buildfehler nacheinander beheben. Die meisten benötigen einige Klasse und Methode Namen, die im SDK umbenannt wurden geändert. [Liste der Bruchfestigkeit Änderungen in der Version 1.1](#ListOfChangesV1) enthält eine Liste der folgenden Namensänderungen an.

Wenn Sie benutzerdefinierte Klassen zum Modellieren von Dokumenten verwenden und diese Klassen Eigenschaften des NULL-Grundtypen müssen (z. B. `int` oder `bool` in c#), ist es eine Fehlerkorrektur in der Version 1.1 des SDK der sollten Sie sich bewusst sein. Weitere Informationen finden Sie [in der Version 1.1 Updates](#BugFixesV1) .

Schließlich, nachdem Sie alle Buildfehler korrigiert haben, können Sie nehmen Sie Änderungen an Ihrer Anwendung zu nutzen neue Funktionen, wenn Sie möchten.

<a name="ListOfChangesV1"></a>
### <a name="list-of-breaking-changes-in-version-11"></a>Liste der Bruchfestigkeit Änderungen in der Version 1.1

Die folgende Liste ist nach die Wahrscheinlichkeit, dass sortiert werden, die Änderung den Anwendungscode auswirkt.

#### <a name="indexbatch-and-indexaction-changes"></a>IndexBatch und IndexAction Änderungen

`IndexBatch.Create`wurde in umbenannt `IndexBatch.New` mehr hat eine `params` Argument. Sie können `IndexBatch.New` für Stapel, die verschiedene Arten von Aktionen (Vorlagen für den Seriendruck, löschen usw.) mischen. Darüber hinaus stehen neue statische Methoden zum Erstellen von Stapeln, in dem alle Aktionen gleich sind: `Delete`, `Merge`, `MergeOrUpload`, und `Upload`.

`IndexAction`nicht mehr verfügt über öffentlichen Konstruktoren und seine Eigenschaften sind jetzt unveränderlich. Sie sollten die neuen statischen Methoden zum Erstellen von Aktionen für verschiedene Zwecke verwenden: `Delete`, `Merge`, `MergeOrUpload`, und `Upload`. `IndexAction.Create`wurde entfernt. Wenn Sie die Überladung, die nur ein Dokument akzeptiert verwendet, vergewissern Sie sich mit `Upload` stattdessen.

##### <a name="example"></a>Beispiel

Wenn der Code wie folgt aussieht:

    var batch = IndexBatch.Create(documents.Select(doc => IndexAction.Create(doc)));
    indexClient.Documents.Index(batch);

Sie können sie diese Option, um das Beheben von Fehlern erstellen ändern:

    var batch = IndexBatch.New(documents.Select(doc => IndexAction.Upload(doc)));
    indexClient.Documents.Index(batch);

Wenn Sie möchten, können Sie weiter, vereinfachen:

    var batch = IndexBatch.Upload(documents);
    indexClient.Documents.Index(batch);

#### <a name="indexbatchexception-changes"></a>IndexBatchException Änderungen

Die `IndexBatchException.IndexResponse` Eigenschaft wurde in umbenannt `IndexingResults`, und der Typ ist jetzt `IList<IndexingResult>`.

##### <a name="example"></a>Beispiel

Wenn der Code wie folgt aussieht:

    catch (IndexBatchException e)
    {
        Console.WriteLine(
            "Failed to index some of the documents: {0}",
            String.Join(", ", e.IndexResponse.Results.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

Sie können sie diese Option, um das Beheben von Fehlern erstellen ändern:

    catch (IndexBatchException e)
    {
        Console.WriteLine(
            "Failed to index some of the documents: {0}",
            String.Join(", ", e.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

<a name="OperationMethodChanges"></a>
#### <a name="operation-method-changes"></a>Vorgang Methode Änderungen

Jede Operation in das Azure .NET SDK wird als eine Reihe von überladene Methoden für synchrones und asynchrones Anrufer verfügbar gemacht. Signaturen und Verarbeitung von diese überladene Methoden wurde im Version 1.1 geändert.

Der Vorgang "Abrufen Indexstatistik" in älteren Versionen von SDK bereitgestellt beispielsweise diese Signaturen:

In `IIndexOperations`:

    // Asynchronous operation with all parameters
    Task<IndexGetStatisticsResponse> GetStatisticsAsync(
        string indexName,
        CancellationToken cancellationToken);

In `IndexOperationsExtensions`:

    // Asynchronous operation with only required parameters
    public static Task<IndexGetStatisticsResponse> GetStatisticsAsync(
        this IIndexOperations operations,
        string indexName);

    // Synchronous operation with only required parameters
    public static IndexGetStatisticsResponse GetStatistics(
        this IIndexOperations operations,
        string indexName);

Die Methodensignaturen für den gleichen Vorgang in der Version 1.1 wie folgt aussehen:

In `IIndexesOperations`:

    // Asynchronous operation with lower-level HTTP features exposed
    Task<AzureOperationResponse<IndexGetStatisticsResult>> GetStatisticsWithHttpMessagesAsync(
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions),
        Dictionary<string, List<string>> customHeaders = null,
        CancellationToken cancellationToken = default(CancellationToken));

In `IndexesOperationsExtensions`:

    // Simplified asynchronous operation
    public static Task<IndexGetStatisticsResult> GetStatisticsAsync(
        this IIndexesOperations operations,
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions),
        CancellationToken cancellationToken = default(CancellationToken));

    // Simplified synchronous operation
    public static IndexGetStatisticsResult GetStatistics(
        this IIndexesOperations operations,
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions));

Beginnend mit der Version 1.1 organisiert Azure Suche .NET SDK Vorgang Methoden anders:

 - Optionale Parameter werden nun als Parameter lieber Standard erstellt als zusätzliche überladene. Dadurch wird die Anzahl der überladene Methoden, manchmal erheblich reduziert.
 - Die Erweiterungsmethoden ausblenden nun viele irrelevante Details der HTTP aus dem Anrufer. Ältere Versionen von SDK zurückgegeben beispielsweise ein Antwortobjekt mit einer HTTP-Statuscode, die Sie häufig überprüft werden, da der Vorgang Methoden lösen müssen nicht `CloudException` nach Statuscode, der einen Fehler angibt. Die neue Erweiterungsmethoden zurückgeben nur Modellobjekte, speichern Sie die Probleme der Probleme, die sie in Ihrem Code aufgelöst werden.
 - Das Herzstück Schnittstellen umgekehrt jetzt Methoden verfügbar machen, die bei Bedarf mehr Einfluss auf HTTP-Ebene zu erhalten. Sie können jetzt in benutzerdefinierten HTTP-Header in Besprechungsanfragen und das neue einzubeziehenden übergeben `AzureOperationResponse<T>` Rückgabetyp bietet Ihnen direkten Zugriff auf die `HttpRequestMessage` und `HttpResponseMessage` für den Vorgang. `AzureOperationResponse`wird definiert, der `Microsoft.Rest.Azure` Namespace und ersetzt `Hyak.Common.OperationResponse`.

#### <a name="scoringparameters-changes"></a>ScoringParameters Änderungen

Ein neue Klasse namens `ScoringParameter` in das aktuelle SDK zu bieten Parameter bewerten Profile in eine Suchabfrage erleichtern hinzugefügt wurde. Zuvor die `ScoringProfiles` Eigenschaft den `SearchParameters` Klasse eingegeben wurde, als `IList<string>`; Nachdem er als eingegeben wurde `IList<ScoringParameter>`.

##### <a name="example"></a>Beispiel

Wenn der Code wie folgt aussieht:

    var sp = new SearchParameters();
    sp.ScoringProfile = "jobsScoringFeatured";      // Use a scoring profile
    sp.ScoringParameters = new[] { "featuredParam-featured", "mapCenterParam-" + lon + "," + lat };

Sie können sie diese Option, um das Beheben von Fehlern erstellen ändern: 

    var sp = new SearchParameters();
    sp.ScoringProfile = "jobsScoringFeatured";      // Use a scoring profile
    sp.ScoringParameters =
        new[]
        {
            new ScoringParameter("featuredParam", new[] { "featured" }),
            new ScoringParameter("mapCenterParam", GeographyPoint.Create(lat, lon))
        };

#### <a name="model-class-changes"></a>Modell Klasse Änderungen

Wechselt in beschrieben [Vorgang Methode ändert](#OperationMethodChanges), Anzahl von Klassen im, da die Signatur der `Microsoft.Azure.Search.Models` Namespace umbenannt oder entfernt wurden. Beispiel:

 - `IndexDefinitionResponse`wurde durch ersetzt`AzureOperationResponse<Index>`
 - `DocumentSearchResponse`wurde in umbenannt`DocumentSearchResult`
 - `IndexResult`wurde in umbenannt`IndexingResult`
 - `Documents.Count()`gibt nun eine `long` mit der Dokumentanzahl statt einer`DocumentCountResponse`
 - `IndexGetStatisticsResponse`wurde in umbenannt`IndexGetStatisticsResult`
 - `IndexListResponse`wurde in umbenannt`IndexListResult`

Zusammenfassung, `OperationResponse`-abgeleitete Klassen, die bestanden nur zum Umbrechen ein Modellobjekts entfernt wurden. Die verbleibenden Klassen hatten deren Suffix geändert von `Response` zu `Result`.

##### <a name="example"></a>Beispiel

Wenn der Code wie folgt aussieht:

    IndexerGetStatusResponse statusResponse = null;

    try
    {
        statusResponse = _searchClient.Indexers.GetStatus(indexer.Name);
    }
    catch (Exception ex)
    {
        Console.WriteLine("Error polling for indexer status: {0}", ex.Message);
        return;
    }

    IndexerExecutionResult lastResult = statusResponse.ExecutionInfo.LastResult;

Sie können sie diese Option, um das Beheben von Fehlern erstellen ändern:

    IndexerExecutionInfo status = null;

    try
    {
        status = _searchClient.Indexers.GetStatus(indexer.Name);
    }
    catch (Exception ex)
    {
        Console.WriteLine("Error polling for indexer status: {0}", ex.Message);
        return;
    }

    IndexerExecutionResult lastResult = status.LastResult;

##### <a name="response-classes-and-ienumerable"></a>Antwortklassen und IEnumerable

Eine zusätzliche Änderung, die möglicherweise auf den Code auswirken wird, halten Sie Websitesammlungen Antwortklassen nicht mehr implementieren `IEnumerable<T>`. Stattdessen können Sie die Auflistungseigenschaft direkt zugreifen. Angenommen, Ihr Code wie folgt aussieht:

    DocumentSearchResponse<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
    foreach (SearchResult<Hotel> result in response)
    {
        Console.WriteLine(result.Document);
    }

Sie können sie diese Option, um das Beheben von Fehlern erstellen ändern:

    DocumentSearchResult<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
    foreach (SearchResult<Hotel> result in response.Results)
    {
        Console.WriteLine(result.Document);
    }

##### <a name="important-note-for-web-applications"></a>Wichtiger Hinweis für Webanwendungen

Wenn Sie eine Webanwendung verfügen, die serialisiert `DocumentSearchResponse` direkt um Suchergebnisse an den Browser zu senden, Sie den Code ändern müssen oder die Ergebnisse werden nicht ordnungsgemäß serialisiert. Angenommen, Ihr Code wie folgt aussieht:

    public ActionResult Search(string q = "")
    {
        // If blank search, assume they want to search everything
        if (string.IsNullOrWhiteSpace(q))
            q = "*";

        return new JsonResult
        {
            JsonRequestBehavior = JsonRequestBehavior.AllowGet,
            Data = _featuresSearch.Search(q)
        };
    }

Sie können ihn ändern, indem Sie die erste der `.Results` Eigenschaft der Suchantwort, suchen Ergebnis Rendern zu beheben:

    public ActionResult Search(string q = "")
    {
        // If blank search, assume they want to search everything
        if (string.IsNullOrWhiteSpace(q))
            q = "*";

        return new JsonResult
        {
            JsonRequestBehavior = JsonRequestBehavior.AllowGet,
            Data = _featuresSearch.Search(q).Results
        };
    }

Sie müssen für diesen Fällen im Code selbst suchen; **Der Compiler Sie nicht warnen** da `JsonResult.Data` vom Typ `object`.

#### <a name="cloudexception-changes"></a>CloudException Änderungen

Der `CloudException` Klasse wurde verschoben, von der `Hyak.Common` Namespace, um die `Microsoft.Rest.Azure` Namespace. Darüber hinaus seine `Error` Eigenschaft wurde in umbenannt `Body`.

#### <a name="searchserviceclient-and-searchindexclient-changes"></a>SearchServiceClient und SearchIndexClient Änderungen

Den Typ des der `Credentials` -Eigenschaft geändert hat `SearchCredentials` an ihre Basis Klasse, `ServiceClientCredentials`. Wenn Sie benötigen für den Zugriff auf die `SearchCredentials` von einer `SearchIndexClient` oder `SearchServiceClient`, verwenden Sie den neuen `SearchCredentials` Eigenschaft.

In älteren Versionen von SDK `SearchServiceClient` und `SearchIndexClient` hatten Konstruktoren, die Erstellen einer `HttpClient` Parameter. Diese wurden mit Konstruktoren verwenden, ersetzt eine `HttpClientHandler` und ein Array von `DelegatingHandler` Objekte. Dies vereinfacht das benutzerdefinierte Ereignishandler zum Verarbeiten von HTTP-Anfragen bei Bedarf vorab zu installieren.

Schließlich die Konstruktoren, die Erstellen einer `Uri` und `SearchCredentials` wurden geändert. Angenommen, Sie Code haben, die sieht wie folgt aus:

    var client =
        new SearchServiceClient(
            new SearchCredentials("abc123"),
            new Uri("http://myservice.search.windows.net"));

Sie können sie diese Option, um das Beheben von Fehlern erstellen ändern:

    var client =
        new SearchServiceClient(
            new Uri("http://myservice.search.windows.net"),
            new SearchCredentials("abc123"));

Beachten Sie auch, die auf der Typ des Parameters Anmeldeinformationen geändert wurde `ServiceClientCredentials`. Dies ist wahrscheinlich nicht auf den Code seit auswirken `SearchCredentials` wird abgeleitet von `ServiceClientCredentials`.

#### <a name="passing-a-request-id"></a>Senden eine Anforderung-ID

In älteren Versionen von SDK, könnten Sie Kennung festlegen, klicken Sie auf die `SearchServiceClient` oder `SearchIndexClient` und sie möchten in jeder Anforderung an die REST-API eingeschlossen werden. Dies eignet sich zum Beheben von Problemen mit der Suchdienst aus, wenn Sie mit dem Support benötigen. Es ist jedoch hilfreicher eine Anforderung eindeutige ID für jeden Vorgang festlegen, statt die gleiche ID für alle Vorgänge verwendet werden soll. Aus diesem Grund der `SetClientRequestId` Methoden der `SearchServiceClient` und `SearchIndexClient` entfernt wurden. Sie können stattdessen eine Anforderung-ID an jede Methode Vorgang übergeben, über das optionale `SearchRequestOptions` Parameter.

> [AZURE.NOTE] In zukünftigen Versionen des SDK werden wir eine neue Verfahren zum Festlegen von Kennung Global auf dem Client Objekte hinzufügen, die mit der Vorgehensweise von anderen Azure SDKs konsistent ist.

#### <a name="example"></a>Beispiel

Wenn Sie Code haben, die sieht wie folgt aus:

    client.SetClientRequestId(Guid.NewGuid());
    ...
    long count = client.Documents.Count();

Sie können sie diese Option, um das Beheben von Fehlern erstellen ändern:

    long count = client.Documents.Count(new SearchRequestOptions(requestId: Guid.NewGuid()));

#### <a name="interface-name-changes"></a>Benutzeroberflächen-Namensänderungen

Die Vorgang Benutzeroberfläche Gruppennamen wurden alle geändert, um mit ihrer entsprechenden Eigenschaftsnamen konsistent sein:

 - Den Typ des `ISearchServiceClient.Indexes` umbenannt wurde von `IIndexOperations` zu `IIndexesOperations`.
 - Den Typ des `ISearchServiceClient.Indexers` umbenannt wurde von `IIndexerOperations` zu `IIndexersOperations`.
 - Den Typ des `ISearchServiceClient.DataSources` umbenannt wurde von `IDataSourceOperations` zu `IDataSourcesOperations`.
 - Den Typ des `ISearchIndexClient.Documents` umbenannt wurde von `IDocumentOperations` zu `IDocumentsOperations`.

Diese Änderung ist wahrscheinlich nicht den Code auswirken, es sei denn, Sie Mocks dieser Schnittstellen Testzwecken erstellt haben.

<a name="BugFixesV1"></a>
### <a name="bug-fixes-in-version-11"></a>Updates in Version 1.1

Ältere Versionen von Azure Suche .NET SDK Serialisierungsfehler vom benutzerdefinierten Modellklassen enthielt ein Fehler. Der Fehler kann auftreten, wenn Sie eine Eigenschaft eines Werttyps NULL-eine benutzerdefinierten Modellklasse erstellt.

#### <a name="steps-to-reproduce"></a>Schritte zum Reproduzieren

Erstellen Sie eine benutzerdefinierte Modellklasse mit einer Eigenschaft vom Typ NULL-Werte ein. Fügen Sie beispielsweise eine öffentliche `UnitCount` Eigenschaft vom Typ `int` anstelle von `int?`.

Wenn Sie ein Dokument mit den Standardwert dieses Typs indizieren (z. B. 0 für `int`), werden das Feld null in Azure suchen. Wenn Sie später für dieses Dokument, suchen Sie die `Search` Anruf löst `JsonSerializationException` beschweren, die es konvertieren kann `null` zu `int`.

Darüber hinaus funktionieren Filter nicht wie erwartet, da Sie den Index anstelle der beabsichtigte Wert Null geschrieben wurde.

#### <a name="fix-details"></a>Beheben von details

Wir haben in Version 1.1 des SDK dieses Problem behoben. Jetzt, wenn Sie verfügen über ein Modellklasse wie folgt:

    public class Model
    {
        public string Key { get; set; }

        public int IntValue { get; set; }
    }

und legen Sie `IntValue` 0, dieser Wert ist jetzt ordnungsgemäß als 0, bei der Übertragung serialisiert und als 0 im Index gespeichert. In Schleifenszenarios funktioniert wie erwartet.

Es wird ein mögliches Problem bei diesem Ansatz berücksichtigen: Wenn Sie einen Modelltyp mit einer NULL-Eigenschaft verwenden, müssen Sie **sicherstellen** , dass keine Dokumente in Ihrem Index einen null-Wert für das entsprechende Feld enthalten. Weder das SDK noch die Azure Suche REST-API hilft Ihnen dies zu erzwingen.

Dies ist nicht nur hypothetische problematisch: Stellen Sie sich vor einem Szenario, in dem Sie ein neues Feld hinzufügen, um einen vorhandenen Index, die vom Typ ist `Edm.Int32`. Nach der Aktualisierung der Indexdefinition, werden alle Dokumente verfügen über einen null-Wert für das neue Feld (da alle Typen in Azure suchen auf NULL festgelegt sind). Wenn Sie eine Modellklasse dann mit einem NULL-verwenden `int` -Eigenschaft für dieses Feld, erhalten Sie eine `JsonSerializationException` so wie hier, wenn Sie versuchen, die zum Abrufen von Dokumenten:

    Error converting value {null} to type 'System.Int32'. Path 'IntValue'.

Aus diesem Grund wird empfohlen, dass Sie Typen in Ihr Modellklassen als bewährte Methode verwenden.

Weitere Informationen zu diesem Fehler und Fix finden Sie [dieses Problem auf GitHub](https://github.com/Azure/azure-sdk-for-net/issues/1063).

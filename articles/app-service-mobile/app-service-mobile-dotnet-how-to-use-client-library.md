<properties
    pageTitle="Arbeiten mit der App-Dienst Mobile-Apps verwalteten Client-Bibliothek (Windows | Xamarin) | Microsoft Azure"
    description="Erfahren Sie, wie einen .NET Client für Azure App Dienst Mobile-Apps mit apps für Windows und Xamarin verwendet."
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="how-to-use-the-managed-client-for-azure-mobile-apps"></a>Verwendung von verwalteten Client für Azure Mobile-Apps

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

##<a name="overview"></a>(Übersicht)

In diesem Handbuch wird gezeigt, wie häufige Szenarien mithilfe der verwalteten Client-Bibliothek für Azure App Dienst Mobile-Apps für Windows und Xamarin apps ausführen. Wenn Sie auf Mobile-Apps nicht vertraut sind, sollten Sie zuerst den [Azure Mobile-Apps Schnellstart] durchführen[ 1] Lernprogramm. In diesem Handbuch liegt der Schwerpunkt auf die verwaltete clientseitige SDK. Erfahren Sie mehr über die serverseitige SDKs für Mobile-Apps, finden Sie in der Dokumentation für das [.NET Server SDK] [ 2] oder das [Node.js Server SDK][3].

## <a name="reference-documentation"></a>Dokumentation zur

Der Dokumentation für den Client SDK befindet sich hier: [Azure Mobile Apps .NET Client Bezug][4].
Finden Sie auch mehrere Client-Beispiele im [Azure-Beispiele GitHub Repository][5].

## <a name="supported-platforms"></a>Unterstützte Plattformen

Die .NET Plattform unterstützt die folgenden Betriebssysteme:

* Xamarin Android-Versionen für API 19 über 24 (KitKat bis Nougat)
* IOS Xamarin frei für iOS Versionen 8.0 und höher
* Universal Windows-Plattform
* Windows Phone 8.1
* Windows Phone 8.0 mit Ausnahme der Silverlight-Anwendungen

Die "Server Bewegung"-Authentifizierung verwendet eine WebView für die präsentierten Benutzeroberfläche an.  Wenn das Gerät nicht Vorführen einer WebView UI kann, werden andere Methoden der Authentifizierung erforderlich sind.  Dieses SDK eignet sich daher nicht für anzeigen oder vom Typ auf ähnliche Weise eingeschränkter Geräte.

##<a name="a-namesetupasetup-and-prerequisites"></a><a name="setup"></a>Setup und erforderliche Komponenten

Davon ausgegangen, dass Sie bereits erstellt und Projekt Back-End-Mobile-App, das mindestens eine Tabelle enthält veröffentlicht haben.  In den Code in diesem Thema verwendet werden, die Tabelle heißt `TodoItem` und weist die folgenden Spalten: `Id`, `Text`, und `Complete`. Diese Tabelle ist der gleichen Tabelle erstellt, wenn Sie den [Azure Mobile-Apps Schnellstart] abgeschlossen haben.

Der entsprechende eingegebene clientseitige Typ in c# ist die folgende Klasse:

    public class TodoItem
    {
        public string Id { get; set; }

        [JsonProperty(PropertyName = "text")]
        public string Text { get; set; }

        [JsonProperty(PropertyName = "complete")]
        public bool Complete { get; set; }
    }

Die [JsonPropertyAttribute] [ 6] wird verwendet, um die Zuordnung *Eigenschaftenname* zwischen den Clienttyp und die Tabelle definieren.

Informationen zum Erstellen von Tabellen in Ihre Mobile-Apps Back-End-finden Sie die Informationen unter dem [Thema .NET Server SDK] unter[ 7] oder das [Thema Node.js Server SDK][8]. Wenn Sie Ihre Mobile-App Back-End-im Azure-Portal mit den Schnellstart erstellt haben, können Sie auch die Einstellung **einfache Tabellen** im [Portal Azure]verwenden.

###<a name="how-to-install-the-managed-client-sdk-package"></a>So: Installieren Sie das verwalteten Client SDK-Paket

Verwenden Sie eine der folgenden Methoden zum Installieren des verwalteten Client-SDK-Pakets für Mobile-Apps von [NuGet][9]:

+ **Visual Studio** Mit der rechten Maustaste in Ihrem Projekts, klicken Sie auf **NuGet-Pakete verwalten**, suchen Sie nach der `Microsoft.Azure.Mobile.Client` Verpacken, und klicken Sie auf **Installieren**.

+ **Xamarin Studio** Mit der rechten Maustaste in Ihrem Projekts, klicken Sie auf **Hinzufügen** > **NuGet-Pakete hinzufügen**, suchen Sie nach der `Microsoft.Azure.Mobile.Client `packen, und klicken Sie dann auf **Paket hinzufügen**.

Beachten Sie in der Aktivitätsdatei Hauptfenster die folgende **using** -Anweisung hinzufügen:

    using Microsoft.WindowsAzure.MobileServices;

###<a name="a-namesymbolsourceahow-to-work-with-debug-symbols-in-visual-studio"></a><a name="symbolsource"></a>So: Arbeiten mit Debuggen Symbole in Visual Studio

Die Symbole für den Namespace Microsoft.Azure.Mobile stehen auf [SymbolSource][10].  Schlagen Sie in den [Anweisungen SymbolSource] [ 11] SymbolSource mit Visual Studio integriert werden soll.

##<a name="a-namecreate-clientacreate-the-mobile-apps-client"></a><a name="create-client"></a>Erstellen des Apps für Mobile Clients

Der folgende Code erstellt die [MobileServiceClient] [ 12] Objekt, das zum Zugreifen auf Ihre Mobile-App Back-End-verwendet wird.

    var client = new MobileServiceClient("MOBILE_APP_URL");

Ersetzen Sie im vorhergehenden Code `MOBILE_APP_URL` mit dem Mobile-App Back-End-URL, die in das Blade für Ihre Mobile-App Back-End-im [Portal Azure]gefunden wird. Das Objekt MobileServiceClient sollten sich auf einem einzelnen.

## <a name="work-with-tables"></a>Arbeiten mit Tabellen

Im folgende Abschnitt enthält Informationen zum Suchen und Abrufen von Datensätzen und ändern Sie die Daten in der Tabelle.  Es werden folgende Themen behandelt:

* [Erstellen Sie einen Tabellenverweis](#instantiating)
* [Abfragen von Daten](#querying)
* [Filtern Sie die zurückgegebene Daten](#filtering)
* [Sortieren der Daten zurückgegeben](#sorting)
* [Daten in Seiten zurück](#paging)
* [Wählen Sie bestimmte Spalten aus.](#selecting)
* [Nachschlagen eines Datensatzes nach Id](#lookingup)
* [Umgang mit nicht typisierten Abfragen](#untypedqueries)
* [Einfügen von Daten](#inserting)
* [Aktualisieren von Daten](#updating)
* [Löschen von Daten](#deleting)
* [Lösung von Konflikten und vollständige Parallelität](#optimisticconcurrency)
* [Binden an eine Windows-Benutzeroberfläche](#binding)
* [Ändern des Papierformats](#pagesize)

###<a name="a-nameinstantiatingahow-to-create-a-table-reference"></a><a name="instantiating"></a>So: Erstellen eines 3D-Bezugs Tabelle

Gesamten Code, der Zugriff auf oder Ändern von Daten in einer Back-End-Tabelle ruft Funktionen für die `MobileServiceTable` Objekt. Rufen Sie einen Verweis auf die Tabelle, indem Sie die Methode [GetTable] wie folgt aufrufen:

    IMobileServiceTable<TodoItem> todoTable = client.GetTable<TodoItem>();

Das zurückgegebene Objekt wird dem eingegebenen Serialisierungsmodell verwendet. Ein nicht typisiertes Serialisierungsmodell wird ebenfalls unterstützt. Im folgenden Beispiel [erstellt einen Verweis auf eine nicht typisierte Tabelle]:

    // Get an untyped table reference
    IMobileServiceTable untypedTodoTable = client.GetTable("TodoItem");

In nicht typisierten Abfragen müssen Sie die zugrunde liegenden OData-Abfragezeichenfolge angeben.

###<a name="a-namequeryingahow-to-query-data-from-your-mobile-app"></a><a name="querying"></a>Wie: Abfragen von Daten aus der Mobile-App

In diesem Abschnitt beschrieben, wie Abfragen an die Mobile-App Back-End-ausgeben, wozu auch die folgende Funktionen:

- [Filtern Sie die zurückgegebene Daten](#filtering)
- [Sortieren der Daten zurückgegeben](#sorting)
- [Daten in Seiten zurück](#paging)
- [Wählen Sie bestimmte Spalten aus.](#selecting)
- [Suchen von Daten nach ID](#lookingup)

>[AZURE.NOTE]Ein Server leistungsgesteuert Seitenformat wird erzwungen, um zu verhindern, dass alle Zeilen zurückgegeben werden.  Seitennavigation verhindert, dass Standard-Anfragen für große Datenmengen beeinträchtigen Dienst.  Um mehr als 50 Zeilen zurückzugeben, verwenden Sie die `Skip` und `Take` Methode, die unter [Daten auf Seiten zurückgeben] beschrieben.

###<a name="a-namefilteringahow-to-filter-returned-data"></a><a name="filtering"></a>So: Filter zurückgegebenen Daten

Im folgende Code wird veranschaulicht, wie zum Filtern von Daten nach einschließlich einer `Where` -Klausel in einer Abfrage. Es gibt alle Elemente aus `todoTable` , deren `Complete` Eigenschaft ist gleich `false`. Die [Stelle, an der] Funktion wendet eine Zeile Prädikat der Abfrage für die Tabelle zu filtern.

    // This query filters out completed TodoItems and items without a timestamp.
    List<TodoItem> items = await todoTable
       .Where(todoItem => todoItem.Complete == false)
       .ToListAsync();

Sie können den URI der Anforderung an die Back-End-gesendet werden, mithilfe der Nachricht Prüfung Software, wie etwa Entwicklertools Browser oder [Fiddler]anzeigen. Wenn Sie die Anfrage URI betrachten, beachten Sie, dass die Abfragezeichenfolge geändert wird:

    GET /tables/todoitem?$filter=(complete+eq+false) HTTP/1.1

Diese Anforderung OData wird in einer SQL-Abfrage vom Server SDK übersetzt:

    SELECT *
    FROM TodoItem
    WHERE ISNULL(complete, 0) = 0

Die Funktion, die an der `Where` Methode kann über eine beliebige Anzahl von Bedingungen verfügen.

    // This query filters out completed TodoItems where Text isn't null
    List<TodoItem> items = await todoTable
       .Where(todoItem => todoItem.Complete == false && todoItem.Text != null)
       .ToListAsync();

In diesem Beispiel würde in einer SQL-Abfrage vom Server SDK umgewandelt werden:

    SELECT *
    FROM TodoItem
    WHERE ISNULL(complete, 0) = 0
          AND ISNULL(text, 0) = 0

Diese Abfrage kann auch in mehreren Klauseln aufgeteilt werden:

    List<TodoItem> items = await todoTable
       .Where(todoItem => todoItem.Complete == false)
       .Where(todoItem => todoItem.Text != null)
       .ToListAsync();

Die beiden Methoden sind gleichwertig und synonym verwendet werden können.  Die erste Option&mdash;verketten in einer Abfrage mehrere Prädikate&mdash;Weitere komprimieren und empfohlen wird.

Die `Where` Klausel unterstützt Vorgänge, die die Teilmenge OData übersetzt. Vorgänge umfassen:

* Relationale Operatoren (==,! =, <, < =, >, > =),
* Arithmetische Operatoren (+, -, / *, %),
* Zahl mit doppelter Genauigkeit (Math.Floor, Math.Ceiling)
* Zeichenfolgenfunktionen (Länge, Teilzeichenfolge, ersetzen, IndexOf, StartsWith, EndsWith)
* Datumseigenschaften (Jahr, Monat, Tag, Stunde, Minute, Sekunde)
* Zugriff auf die Eigenschaften eines Objekts, und
* Ausdrücke kombinieren dieser Vorgänge.

Erwägen, was das SDK Server unterstützt, können Sie der [Dokumentation für OData v3]berücksichtigen.

###<a name="a-namesortingahow-to-sort-returned-data"></a><a name="sorting"></a>So: Sortieren zurückgegebenen Daten

Im folgende Code veranschaulicht, wie Daten zu sortieren, indem Sie eine [OrderBy] oder [OrderByDescending] -Funktion in der Abfrage. Es gibt die Elemente aus `todoTable` von aufsteigend sortiert die `Text` Feld.

    // Sort items in ascending order by Text field
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .OrderBy(todoItem => todoItem.Text)
    List<TodoItem> items = await query.ToListAsync();

    // Sort items in descending order by Text field
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .OrderByDescending(todoItem => todoItem.Text)
    List<TodoItem> items = await query.ToListAsync();

###<a name="a-namepagingahow-to-return-data-in-pages"></a><a name="paging"></a>So: in Seiten Daten zurück

Standardmäßig gibt die Back-End-nur die ersten 50 Zeilen an. Sie können die Anzahl zurückgegebener Zeilen erhöhen, durch Aufrufen der Methode [Ausführen] . Verwenden Sie `Take` zusammen mit der [Überspringen] Methode zum Anfordern einer bestimmtes "Seite" des gesamten Datasets von der Abfrage zurückgegeben. Die folgende Abfrage, bei der Ausführung gibt die obersten drei Elemente in der Tabelle.

    // Define a filtered query that returns the top 3 items.
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Take(3);
    List<TodoItem> items = await query.ToListAsync();

Die folgende Abfrage für die überarbeitete überspringt die ersten drei Ergebnisse und den nächsten drei Ergebnisse zurückgegeben. Diese Abfrage erzeugt die zweite "Seite" Data, wobei die Seitengröße drei Elemente ist.

    // Define a filtered query that skips the top 3 items and returns the next 3 items.
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Skip(3)
                    .Take(3);
    List<TodoItem> items = await query.ToListAsync();

Die Methode [IncludeTotalCount] fordert der gesamten Anzahl für _Alle_ an die Datensätze, die zurückgegeben worden wäre ignorieren alle Seitennavigation/Limit-Klausel angegeben:

    query = query.IncludeTotalCount();

In einer realen app können Abfragen ähnlich wie im vorherigen Beispiel mit einem Pagersteuerelement oder vergleichbaren Benutzeroberfläche Sie zwischen den Seiten navigieren.

>[AZURE.NOTE]Zum Außerkraftsetzen der 50 zeilenbeschränkung in einer Back-End-Mobile-App-, müssen Sie auch übernehmen der [EnableQueryAttribute] für die öffentliche GET-Methode und geben das Seitennavigation Verhalten. Wenn Sie auf die Methode angewendet wird, legt vor das Maximum Zeilen auf 1000 zurückgegeben:
>
>    [EnableQuery(MaxTop=1000)]

### <a name="a-nameselectingahow-to-select-specific-columns"></a><a name="selecting"></a>So: Wählen Sie bestimmte Spalten aus.

Sie können angeben, welche Gruppe von Eigenschaften in die Ergebnisse einbezogen werden sollen, durch Hinzufügen einer [Select] -Klausel Ihrer Abfrage. Zum Beispiel veranschaulicht der folgende Code so markieren Sie einfach ein Feld sowie auswählen und Formatieren von mehreren Feldern:

    // Select one field -- just the Text
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Select(todoItem => todoItem.Text);
    List<string> items = await query.ToListAsync();

    // Select multiple fields -- both Complete and Text info
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Select(todoItem => string.Format("{0} -- {1}",
                        todoItem.Text.PadRight(30), todoItem.Complete ?
                        "Now complete!" : "Incomplete!"));
    List<string> items = await query.ToListAsync();

Alle bisher beschriebenen Funktionen sind additiv, damit wir ihnen verketten beibehalten können. Jedes verkettete Anruf wirkt sich auf Weitere der Abfrage aus. Ein weiteres Beispiel:

    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Where(todoItem => todoItem.Complete == false)
                    .Select(todoItem => todoItem.Text)
                    .Skip(3).
                    .Take(3);
    List<string> items = await query.ToListAsync();

### <a name="a-namelookingupahow-to-look-up-data-by-id"></a><a name="lookingup"></a>Wie: Suchen von Daten nach ID

Die [LookupAsync] -Funktion kann verwendet werden, um Objekte mit einer bestimmten ID aus der Datenbank nachschlagen

    // This query filters out the item with the ID of 37BBF396-11F0-4B39-85C8-B319C729AF6D
    TodoItem item = await todoTable.LookupAsync("37BBF396-11F0-4B39-85C8-B319C729AF6D");

### <a name="a-nameuntypedqueriesahow-to-execute-untyped-queries"></a><a name="untypedqueries"></a>So: Ausführen von nicht typisierten Abfragen

Beim Ausführen einer Abfrage mithilfe eines Objekts nicht typisierte Tabelle müssen Sie die OData-Abfragezeichenfolge explizit angeben, indem Sie [ReadAsync], wie im folgenden Beispiel:

    // Lookup untyped data using OData
    JToken untypedItems = await untypedTodoTable.ReadAsync("$filter=complete eq 0&$orderby=text");

Sie erhalten wieder JSON-Werte, die Sie, wie eine Eigenschaftensammlung verwenden können. Weitere Informationen zu JToken und Newtonsoft Json.NET finden Sie unter der [Json.NET] -Website.

### <a name="a-nameinsertingahow-to-insert-data-into-a-mobile-app-backend"></a><a name="inserting"></a>So: Einfügen von Daten in einer Back-End-Mobile-App-

Alle Clienttypen müssen ein Mitglied mit dem Namen **Id**, also standardmäßig eine Zeichenfolge enthalten. Diese **Id** ist erforderlich, um Vorgänge ausführen und für offline synchronisieren. Im folgende Code veranschaulicht, wie die [InsertAsync] Methode zum Einfügen von neuer Zeilen in einer Tabelle verwenden. Der Parameter enthält, die Daten als ein Objekt .NET eingefügt werden.

    await todoTable.InsertAsync(todoItem);

Wenn Sie ein benutzerdefinierter eindeutiger ID-Wert nicht enthalten ist, in der `todoItem` beim Einfügen, wird eine GUID vom Server generiert.
Sie können die generierte Id abrufen, indem Sie prüfen das Objekt aus, nachdem Sie der Anruf gibt.

Wenn nicht typisierte Daten einfügen möchten, können Sie Json.NET nutzen:

    JObject jo = new JObject();
    jo.Add("Text", "Hello World");
    jo.Add("Complete", false);
    var inserted = await table.InsertAsync(jo);

Hier ist ein Beispiel für die Verwendung einer e-Mail-Adresse als eine eindeutige Zeichenfolgen-Id aus:

    JObject jo = new JObject();
    jo.Add("id", "myemail@emaildomain.com");
    jo.Add("Text", "Hello World");
    jo.Add("Complete", false);
    var inserted = await table.InsertAsync(jo);

### <a name="working-with-id-values"></a>Arbeiten mit ID-Werte

Mobile-Apps unterstützt eindeutige benutzerdefinierte Zeichenfolgenwerte für die **ID-** Spalte der Tabelle. Ein Zeichenfolgenwert ist die Anwendung benutzerdefinierte Werte wie e-Mail-Adressen oder Benutzernamen für die ID verwenden  Zeichenfolgen-IDs bieten Ihnen die folgenden Vorteile:

* IDs werden generiert, ohne vorher eine Schleife in die Datenbank zu erstellen.
* Einträge sind leichter aus verschiedenen Tabellen oder Datenbanken zusammenführen.
* IDs-Werte können in einer Anwendung Logik besser integriert werden.

Wenn ein ID Zeichenfolgenwert auf einem eingefügten Datensatz nicht festgelegt ist, generiert die Mobile-App Back-End-einen eindeutigen Wert für die ID an. Sie können mithilfe die [Guid.NewGuid] -Methode eigene ID-Werte, die auf dem Client oder in die Back-End-generieren.

    JObject jo = new JObject();
    jo.Add("id", Guid.NewGuid().ToString("N"));

###<a name="a-namemodifyingahow-to-modify-data-in-a-mobile-app-backend"></a><a name="modifying"></a>So: Ändern von Daten in einer Back-End-Mobile-App-

Im folgende Code veranschaulicht, wie die [UpdateAsync] -Methode verwenden, um einen vorhandenen Datensatz mit der gleichen ID mit neuen Informationen zu aktualisieren. Der Parameter enthält, die Daten als ein Objekt .NET aktualisiert werden.

    await todoTable.UpdateAsync(todoItem);

Um nicht typisierten Daten aktualisieren, können Sie [Json.NET] wie folgt nutzen:

    JObject jo = new JObject();
    jo.Add("id", "37BBF396-11F0-4B39-85C8-B319C729AF6D");
    jo.Add("Text", "Hello World");
    jo.Add("Complete", false);
    var inserted = await table.UpdateAsync(jo);

Eine `id` Feld muss angegeben werden, wenn Sie eine Aktualisierung vornehmen. Die Back-End-verwendet die `id` Feld zu identifizieren die Zeile zu aktualisieren. Die `id` durch das Ergebnis des Felds erzielt werden die `InsertAsync` anrufen. Eine `ArgumentException` wird ausgelöst, wenn Sie versuchen, einen Artikel einzublenden, ohne zu aktualisieren die `id` Wert.

###<a name="a-namedeletingahow-to-delete-data-in-a-mobile-app-backend"></a><a name="deleting"></a>So: Löschen der Daten in einer Back-End-Mobile-App-

Im folgende Code veranschaulicht, wie die Methode [DeleteAsync] verwenden, um eine vorhandene Instanz zu löschen. Die Instanz wird anhand der `id` Feldfamilie auf die `todoItem`.

    await todoTable.DeleteAsync(todoItem);

Wenn nicht typisierte Daten löschen möchten, können Sie Json.NET wie folgt nutzen:

    JObject jo = new JObject();
    jo.Add("id", "37BBF396-11F0-4B39-85C8-B319C729AF6D");
    await table.DeleteAsync(jo);

Wenn Sie eine Besprechungsanfrage löschen vornehmen, muss eine ID angegeben werden. Andere Eigenschaften werden nicht an den Dienst übergeben oder bei dem Dienst ignoriert werden. Das Ergebnis einer `DeleteAsync` Anruf ist in der Regel `null`. Die ID übergeben abgerufen werden kann, durch das Ergebnis des der `InsertAsync` anrufen. A `MobileServiceInvalidOperationException` wird ausgelöst, wenn Sie versuchen, ein Element ohne Angabe der `id` Feld.

###<a name="a-nameoptimisticconcurrencyahow-to-use-optimistic-concurrency-for-conflict-resolution"></a><a name="optimisticconcurrency"></a>So: vollständige Parallelität für Konflikt Auflösung verwenden

Zwei oder mehr Kunden möglicherweise Änderungen in den gleichen Artikel zur gleichen Zeit zu schreiben. Ohne Konflikte erkennen würde der letzte Schreibvorgang alle vorherigen Aktualisierungen überschrieben. **Steuerung der vollständigen Parallelität** wird davon ausgegangen, dass jede Transaktion Commit ausführen kann und können daher keine Ressourcen sperren.  Vor dem Commit einer Transaktions, optimistische Konfliktmodell überprüft, ob keine andere Transaktion die Daten geändert hat. Wenn die Daten geändert wurde, wird die übergebenden Transaktion rückgängig gemacht werden.

Mobile-Apps optimistische Konfliktmodell unterstützt, indem Sie Nachverfolgen von Änderungen auf jedes Element mithilfe der `version` System Eigenschaftenspalte, die für jede Tabelle in der Mobile-App Back-End-definiert ist. Jedes Mal ein Datensatz aktualisiert wird, Mobile-Apps legt die `version` Eigenschaft für den Datensatz einen neuen Wert. Bei jeder Anforderung Aktualisieren der `version` Eigenschaft des Datensatzes Lieferumfang der Anfrage auf dieselbe Eigenschaft für den Datensatz auf dem Server verglichen wird. Wenn die Version mit übergeben die Anfrage entspricht nicht die Back-End-und dann löst die Clientbibliothek aus einer `MobileServicePreconditionFailedException<T>` Ausnahme. Der mit der Ausnahme enthalten ist den Eintrag aus der Back-End-mit der Server-Version des Datensatzes. Die Anwendung kann dann mithilfe dieser Informationen um zu entscheiden, ob beim Ausführen der Anforderung Update erneut mit den richtigen `version` Wert aus der Back-End-, um die Änderungen zu übernehmen.

Definieren Sie eine Spalte in der Tabellenklasse für die `version` System-Eigenschaft vollständigen Parallelität aktivieren. Beispiel:

    public class TodoItem
    {
        public string Id { get; set; }

        [JsonProperty(PropertyName = "text")]
        public string Text { get; set; }

        [JsonProperty(PropertyName = "complete")]
        public bool Complete { get; set; }

        // *** Enable Optimistic Concurrency *** //
        [JsonProperty(PropertyName = "version")]
        public string Version { set; get; }
    }


Nicht typisierte Tabellen mithilfe von Applications vollständigen Parallelität aktivieren, indem Sie die `Version` kennzeichnen, klicken Sie auf die `SystemProperties` der Tabelle wie folgt.

    //Enable optimistic concurrency by retrieving version
    todoTable.SystemProperties |= MobileServiceSystemProperties.Version;

Zusätzlich zum Aktivieren der vollständigen Parallelität, müssen Sie auch abzufangen der `MobileServicePreconditionFailedException<T>` Ausnahme im Code beim [UpdateAsync]aufrufen.  Lösen Sie den Konflikt durch Anwenden des richtigen `version` in den aktualisierten Datensatz und Anruf [UpdateAsync] mit den Eintrag gelöst. Mit dem folgenden Code veranschaulicht, wie ein Schreibkonflikt einmal erkannt beheben:

    private async void UpdateToDoItem(TodoItem item)
    {
        MobileServicePreconditionFailedException<TodoItem> exception = null;

        try
        {
            //update at the remote table
            await todoTable.UpdateAsync(item);
        }
        catch (MobileServicePreconditionFailedException<TodoItem> writeException)
        {
            exception = writeException;
        }

        if (exception != null)
        {
            // Conflict detected, the item has changed since the last query
            // Resolve the conflict between the local and server item
            await ResolveConflict(item, exception.Item);
        }
    }


    private async Task ResolveConflict(TodoItem localItem, TodoItem serverItem)
    {
        //Ask user to choose the resoltion between versions
        MessageDialog msgDialog = new MessageDialog(
            String.Format("Server Text: \"{0}\" \nLocal Text: \"{1}\"\n",
            serverItem.Text, localItem.Text),
            "CONFLICT DETECTED - Select a resolution:");

        UICommand localBtn = new UICommand("Commit Local Text");
        UICommand ServerBtn = new UICommand("Leave Server Text");
        msgDialog.Commands.Add(localBtn);
        msgDialog.Commands.Add(ServerBtn);

        localBtn.Invoked = async (IUICommand command) =>
        {
            // To resolve the conflict, update the version of the item being committed. Otherwise, you will keep
            // catching a MobileServicePreConditionFailedException.
            localItem.Version = serverItem.Version;

            // Updating recursively here just in case another change happened while the user was making a decision
            UpdateToDoItem(localItem);
        };

        ServerBtn.Invoked = async (IUICommand command) =>
        {
            RefreshTodoItems();
        };

        await msgDialog.ShowAsync();
    }

Weitere Informationen finden Sie unter dem Thema [Offline Daten synchronisieren in Azure Mobile-Apps] .

###<a name="a-namebindingahow-to-bind-mobile-apps-data-to-a-windows-user-interface"></a><a name="binding"></a>So: Bindung Mobile-Apps Daten auf einem Windows-Benutzeroberfläche

In diesem Abschnitt wird gezeigt, wie zurückgegebenen Datenobjekte mit Benutzeroberflächenelemente in einen Windows-app anzeigen.  Im folgenden Beispielcode bindet an der Quelle der Liste mithilfe einer Abfrage für unvollständig Elemente. Die [MobileServiceCollection] erstellt eine Mobile Apps-fähige Bindung-Websitesammlung.

    // This query filters out completed TodoItems.
    MobileServiceCollection<TodoItem, TodoItem> items = await todoTable
        .Where(todoItem => todoItem.Complete == false)
        .ToCollectionAsync();

    // itemsControl is an IEnumerable that could be bound to a UI list control
    IEnumerable itemsControl  = items;

    // Bind this to a ListBox
    ListBox lb = new ListBox();
    lb.ItemsSource = items;

Für einige Steuerelemente in die verwaltete Laufzeit unterstützen eine Schnittstelle namens [ISupportIncrementalLoading]. Diese Schnittstelle kann Steuerelemente zusätzliche Daten anfordern, wenn der Benutzer einen Bildlauf an. Es gibt integrierter Unterstützung für diese Schnittstelle für universal Windows-apps über [MobileServiceIncrementalLoadingCollection], die von den Steuerelementen Anrufe automatisch behandelt. Verwenden Sie `MobileServiceIncrementalLoadingCollection` in Windows-apps wie folgt:

    MobileServiceIncrementalLoadingCollection<TodoItem,TodoItem> items;
    items = todoTable.Where(todoItem => todoItem.Complete == false).ToIncrementalLoadingCollection();

    ListBox lb = new ListBox();
    lb.ItemsSource = items;

Verwenden, um die neue Websitesammlung auf apps für Windows Phone 8 und "Silverlight" Verwenden der `ToCollection` Erweiterungsmethoden auf `IMobileServiceTableQuery<T>` und `IMobileServiceTable<T>`. Um Daten zu laden, rufen Sie `LoadMoreItemsAsync()`.

    MobileServiceCollection<TodoItem, TodoItem> items = todoTable.Where(todoItem => todoItem.Complete==false).ToCollection();
    await items.LoadMoreItemsAsync();

Bei Verwendung die Auflistung aufrufen erstellte `ToCollectionAsync` oder `ToCollection`, erhalten Sie eine Auflistung, die auf Benutzeroberflächen-Steuerelemente gebunden werden kann.  Diese Sammlung ist Seitennavigation-bekannt.  Da die Sammlung Daten aus dem Netzwerk geladen wird, schlägt manchmal geladen. Um solche Fehler zu behandeln, außer Kraft setzen die `OnException` Methode auf `MobileServiceIncrementalLoadingCollection` zur Behandlung von Ausnahmen infolge Anrufe an `LoadMoreItemsAsync`.

Erwägen Sie, wenn die Tabelle viele Felder hat, aber nur einige Felder im Steuerelement angezeigt werden soll. Sie können die Anleitung im vorherigen Abschnitt "[Wählen Sie bestimmte Spalten](#selecting)" verwenden, um bestimmte Spalten in der Benutzeroberfläche anzeigen auszuwählen.

###<a name="a-namepagesizeachange-the-page-size"></a><a name="pagesize"></a>Ändern des Seitenformats

Azure Mobile-Apps gibt maximal 50 Elemente pro Anforderung standardmäßig an.  Sie können die Seitengröße ändern, indem Sie die maximale Seitengröße auf Client und Server.  Geben Sie zum Erhöhen der angeforderten Seitengröße `PullOptions` bei Verwendung von `PullAsync()`:

    PullOptions pullOptions = new PullOptions
        {
            MaxPageSize = 100
        };

Angenommen, Sie haben die `PageSize` gleich oder größer als 100 innerhalb des Servers, gibt eine Anforderung bis zu 100 Elemente.

##<a name="a-nameofflinesyncawork-with-offline-tables"></a><a name="#offlinesync"></a>Arbeiten mit Tabellen im Offlinemodus

Offline Tabellen verwenden eines lokalen SQLite zu speichern Daten für die Verwendung im Offlinemodus.  Alle Tabelle, die Vorgänge abgeschlossen haben, auf den lokalen SQLite anstelle der Remoteserver Store gespeichert werden.  Zum Erstellen einer Tabelle offline bereiten Sie zunächst Ihr Projekt vor:

1. In Visual Studio, mit der Maustaste der Lösung > **NuGet-Pakete verwalten, für die Lösung...**, und klicken Sie dann suchen und Installieren der **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet-Paket für alle Projekte in der Lösung.

2. (Optional) Installieren Sie Windows-Geräten unterstützt, eine der folgenden SQLite Laufzeit Pakete aus:

    * **Windows 8.1 Runtime:** Installieren Sie [SQLite für Windows 8.1][3].
    * **Windows Phone 8.1:** Installieren Sie [SQLite für Windows Phone 8.1][4].
    * **Universal Windows-Plattform** Installieren [SQLite für die Universal Windows Universal][5].

3. (Optional). Für Windows-Geräten, klicken Sie auf **Verweise** > **Verweis hinzufügen**, erweitern Sie den **Windows** -Ordner > **Erweiterungen**, und klicken Sie dann auf Aktivieren Sie das entsprechende **SQLite für Windows** SDK zusammen mit **Visual C++ 2013 Runtime für Windows** SDK.
    Die Namen SQLite SDK sind ein wenig anders mit jeder Windows-Plattform.

Bevor Sie ein Tabellenverweis erstellt werden kann, müssen Sie der lokale Speicher vorbereiten:

    var store = new MobileServiceSQLiteStore(Constants.OfflineDbPath);
    store.DefineTable<TodoItem>();

    //Initializes the SyncContext using the default IMobileServiceSyncHandler.
    await this.client.SyncContext.InitializeAsync(store);

Store Initialisierung erfolgt normalerweise unmittelbar nach der Client erstellt wird.  Die **OfflineDbPath** sollten einen Dateinamen für die Verwendung auf allen Plattformen geeignet, die Sie unterstützen.  Wenn der Pfad einen vollqualifizierten Pfad ist (d. h., es beginnt mit einem Schrägstrich), und klicken Sie dann, dass der Pfad verwendet wird.  Wenn der Pfad nicht vollständig qualifiziert ist, wird die Datei an einem Speicherort Plattform-spezifische platziert.

* Für iOS und Android lautet der Pfad standardmäßig im Ordner "Persönliche Dateien".
* Für Windows-Geräten ist der Standardpfad den anwendungsspezifische "AppData" Ordner.

Ein Tabellenverweis erzielt werden mithilfe der `GetSyncTable<>` Methode:

    var table = client.GetSyncTable<TodoItem>();

Sie müssen nicht authentifizieren, um eine Tabelle offline verwenden.  Sie müssen nur authentifizieren, wenn Sie mit dem Back-End-Dienst kommunizieren.

###<a name="a-namesyncofflineasyncing-an-offline-table"></a><a name="syncoffline"></a>Synchronisieren einer Offline-Tabelle

Offline-Tabellen sind mit der Back-End-standardmäßig nicht synchronisiert.  Synchronisierung ist in zwei Bereiche unterteilt.  Sie können Änderungen separat drücken Sie neue Elemente herunterladen.  Hier ist eine Synchronisierungsmethode typische:

    public async Task SyncAsync()
    {
        ReadOnlyCollection<MobileServiceTableOperationError> syncErrors = null;

        try
        {
            await this.client.SyncContext.PushAsync();

            await this.todoTable.PullAsync(
                //The first parameter is a query name that is used internally by the client SDK to implement incremental sync.
                //Use a different query name for each unique query in your program
                "allTodoItems",
                this.todoTable.CreateQuery());
        }
        catch (MobileServicePushFailedException exc)
        {
            if (exc.PushResult != null)
            {
                syncErrors = exc.PushResult.Errors;
            }
        }

        // Simple error/conflict handling. A real application would handle the various errors like network conditions,
        // server conflicts and others via the IMobileServiceSyncHandler.
        if (syncErrors != null)
        {
            foreach (var error in syncErrors)
            {
                if (error.OperationKind == MobileServiceTableOperationKind.Update && error.Result != null)
                {
                    //Update failed, reverting to server's copy.
                    await error.CancelAndUpdateItemAsync(error.Result);
                }
                else
                {
                    // Discard local change.
                    await error.CancelAndDiscardItemAsync();
                }

                Debug.WriteLine(@"Error executing sync operation. Item: {0} ({1}). Operation discarded.", error.TableName, error.Item["id"]);
            }
        }
    }

Wenn das erste Argument für `PullAsync` null ist, und klicken Sie dann inkrementell synchronisieren nicht verwendet wird.  Jede Synchronisierungsoperation Ruft alle Datensätze ab.

Das SDK führt eine implizite `PushAsync()` vor dem Ziehen von Datensätzen.

Konfliktbehandlung passiert in einer `PullAsync()` Methode.  Sie können auf die gleiche Weise wie online Tabellen Konflikte behandelt.  Der Konflikt entsteht, wenn `PullAsync()` heißt statt während der einfügen, aktualisieren oder löschen. Wenn mehrere Konflikte auftreten, werden diese zu einer einzigen MobileServicePushFailedException zusammengefasst.  Behandeln Sie jeder Fehler einzeln.

##<a name="a-namecustomapiawork-with-a-custom-api"></a><a name="#customapi"></a>Arbeiten Sie mit einem benutzerdefinierten-API

Eine benutzerdefinierte API können Sie zum Definieren von benutzerdefinierter Endpunkten, die Serverfunktionalität verfügbar zu machen, die keine zuordnen zu einer einfügen, aktualisieren, löschen, und beim Lesen. Mithilfe einer benutzerdefinierten API haben mehr Kontrolle über messaging, Sie einschließlich lesen und HTTP-Nachrichtenköpfen festlegen und ein Textkörper Nachrichtenformat als JSON definieren.

Eine benutzerdefinierte API können Sie aufrufen, indem Sie eine der Methoden [InvokeApiAsync] auf dem Client. Beispielsweise sendet die folgende Zeile des Codes eine POST-Anforderung an das **CompleteAll** API auf die Back-End:

    var result = await client.InvokeApiAsync<MarkAllResult>("completeAll", System.Net.Http.HttpMethod.Post, null);

Dieses Formular eines Anrufs eingegebenen Methode ist und setzt voraus, dass der Rückgabetyp **MarkAllResult** definiert ist. Getippte und nicht typisierte Methoden werden unterstützt.

##<a name="a-nameauthenticationaauthenticate-users"></a><a name="authentication"></a>Authentifizieren von Benutzern

Mobile-Apps unterstützt authentifizieren und Autorisieren von app-Benutzern mit verschiedenen externe Identitätsanbieter: Facebook, Google, Microsoft Account, Twitter und Azure Active Directory. Sie können Tabellen zum Einschränken des Zugriffs für bestimmte Vorgänge, die nur authentifizierten Benutzern Berechtigungen festlegen. Sie können auch die Identität des authentifizierten Benutzer verwenden, Autorisierungsregeln in Server-Skripts implementiert wird. Weitere Informationen finden Sie im Lernprogramm [Hinzufügen Authentifizierung zu Ihrer Anwendung].

Es werden zwei Authentifizierung Zahlungen unterstützt: _verwaltete Client_ und _Server verwaltete_ Fluss. Illustrieren Server verwaltet stellt die einfachste Authentifizierung Oberfläche an, wie er von der Anbieter Web-Authentifizierung Oberfläche abhängt. Client-verwaltete illustrieren ermöglicht verbesserte Integration in Geräte-spezifischen Funktionen wie von anbieterspezifische Geräte-spezifischen SDKs abhängt.

>[AZURE.NOTE] Wir empfehlen einen Fluss Client verwaltet in Ihrer apps Herstellung verwenden.

Um Authentifizierung eingerichtet haben, müssen Sie Ihre app mit mindestens einem Identitätsanbieter registrieren.  Der Identitätsanbieter generiert eine Client-ID und ein Geheimnis Client für Ihre app.  Diese Werte werden dann in die Back-End-Azure App-Authentifizierung/Autorisierung aktivieren festgelegt.  Weitere Informationen folgen Sie den detaillierten Anweisungen im Lernprogramm [Hinzufügen Authentifizierung zu Ihrer Anwendung]aus.

In diesem Abschnitt werden die folgenden Themen behandelt:

+ [Verwaltete Client Authentifizierung](#clientflow)
+ [Verwaltete Server Authentifizierung](#serverflow)
+ [Das Authentifizierungstoken Zwischenspeichern](#caching)

###<a name="a-nameclientflowaclient-managed-authentication"></a><a name="clientflow"></a>Verwaltete Client Authentifizierung

Ihre app kann unabhängig voneinander wenden Sie sich an den Identitätsanbieter, und geben Sie dann das zurückgegebene Token bei der Anmeldung mit Ihrem Back-End. Diese Client-Fluss können Sie eine einzelne anmelden Erfahrung für Benutzer bereitstellen oder zum Abrufen von weiteren Benutzerdaten aus der Identitätsanbieter. Client-Fluss Authentifizierung wird empfohlen, bei der Verwendung von eines Server Fluss während der Identitätsanbieter SDK eine weitere systemeigenen UX Verhalten bietet und zur weiteren Anpassung ermöglicht.

Beispiele stehen für die folgenden Client-Fluss Authentifizierung Muster zur Verfügung:

+ [Active Directory-Authentifizierungsbibliothek](#adal)
+ [Facebook oder Google](#client-facebook)
+ [Live SDK](#client-livesdk)

#### <a name="a-nameadalaauthenticate-users-with-the-active-directory-authentication-library"></a><a name="adal"></a>Benutzerauthentifizierung mit der Active Directory-Authentifizierung-Bibliothek

Die Active Directory Authentifizierung Bibliothek (ADAL) können zu initiieren Benutzerauthentifizierung im Desktopclient Azure-Active Directory-Authentifizierung verwenden.

1. Konfigurieren Sie Ihre mobile-app Back-End-für AAD anmelden, indem Sie folgende des Lernprogramms [App-Verwaltungsdienst für Active Directory-Konto konfigurieren] . Vergewissern Sie sich in der Registrierung einer systemeigenen Clientanwendung die optional Schritt abgeschlossen haben.
2. In Visual Studio oder Xamarin Studio, öffnen Sie das Projekt, und fügen Sie einen Verweis auf die `Microsoft.IdentityModel.CLients.ActiveDirectory` NuGet-Paket. Bei der Suche einbeziehen Sie Versionen vor.
3. Fügen Sie den folgenden Code an Ihrer Anwendung, entsprechend der von Ihnen verwendeten Plattform. Nehmen Sie in den einzelnen die folgenden Ersatz:

    * Ersetzen Sie **Einfügen-ZERTIFIZIERUNGSSTELLE-HERE** mit dem Namen der den Mandanten, in dem Sie die Anwendung bereitgestellt. Das Format sollte https://login.windows.net/contoso.onmicrosoft.com sein. Dieser Wert kann auf der Registerkarte Domäne in Ihrem Azure Active Directory im [Azure klassischen Portal]kopiert werden.
    * Ersetzen Sie mit der Client-ID für Ihre mobile-app Back-End- **Einfügen-Ressource-ID-HERE** ein. Sie können die Client-ID von der Registerkarte **Erweitert** unter **Azure Active Directory-Einstellungen** im Portal erhalten.
    * Ersetzen Sie **Einfügen-CLIENT-ID-HERE** durch die Client-ID, die Sie in die native Client-Anwendung kopiert haben.
    * **Einfügen-REDIRECT-URI-HERE** mit Ihrer Website _/.auth/login/done_ Endpunkt, mit dem HTTPS-Schema zu ersetzen. Dieser Wert sollte ähnlich wie _https://contoso.azurewebsites.net/.auth/login/done_sein.

    Der Code für jede Plattform umfasst:

    **Windows:**

        private MobileServiceUser user;
        private async Task AuthenticateAsync()
        {
            string authority = "INSERT-AUTHORITY-HERE";
            string resourceId = "INSERT-RESOURCE-ID-HERE";
            string clientId = "INSERT-CLIENT-ID-HERE";
            string redirectUri = "INSERT-REDIRECT-URI-HERE";
            while (user == null)
            {
                string message;
                try
                {
                    AuthenticationContext ac = new AuthenticationContext(authority);
                    AuthenticationResult ar = await ac.AcquireTokenAsync(resourceId, clientId,
                        new Uri(redirectUri), new PlatformParameters(PromptBehavior.Auto, false) );
                    JObject payload = new JObject();
                    payload["access_token"] = ar.AccessToken;
                    user = await App.MobileService.LoginAsync(
                        MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory, payload);
                    message = string.Format("You are now logged in - {0}", user.UserId);
                }
                catch (InvalidOperationException)
                {
                    message = "You must log in. Login Required";
                }
                var dialog = new MessageDialog(message);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }

    **Xamarin.iOS**

        private MobileServiceUser user;
        private async Task AuthenticateAsync(UIViewController view)
        {
            string authority = "INSERT-AUTHORITY-HERE";
            string resourceId = "INSERT-RESOURCE-ID-HERE";
            string clientId = "INSERT-CLIENT-ID-HERE";
            string redirectUri = "INSERT-REDIRECT-URI-HERE";
            try
            {
                AuthenticationContext ac = new AuthenticationContext(authority);
                AuthenticationResult ar = await ac.AcquireTokenAsync(resourceId, clientId,
                    new Uri(redirectUri), new PlatformParameters(view));
                JObject payload = new JObject();
                payload["access_token"] = ar.AccessToken;
                user = await client.LoginAsync(
                    MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory, payload);
            }
            catch (Exception ex)
            {
                Console.Error.WriteLine(@"ERROR - AUTHENTICATION FAILED {0}", ex.Message);
            }
        }

    **Xamarin.Android**

        private MobileServiceUser user;
        private async Task AuthenticateAsync()
        {
            string authority = "INSERT-AUTHORITY-HERE";
            string resourceId = "INSERT-RESOURCE-ID-HERE";
            string clientId = "INSERT-CLIENT-ID-HERE";
            string redirectUri = "INSERT-REDIRECT-URI-HERE";
            try
            {
                AuthenticationContext ac = new AuthenticationContext(authority);
                AuthenticationResult ar = await ac.AcquireTokenAsync(resourceId, clientId,
                    new Uri(redirectUri), new PlatformParameters(this));
                JObject payload = new JObject();
                payload["access_token"] = ar.AccessToken;
                user = await client.LoginAsync(
                    MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory, payload);
            }
            catch (Exception ex)
            {
                AlertDialog.Builder builder = new AlertDialog.Builder(this);
                builder.SetMessage(ex.Message);
                builder.SetTitle("You must log in. Login Required");
                builder.Create().Show();
            }
        }
        protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
        {
            base.OnActivityResult(requestCode, resultCode, data);
            AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);
        }

####<a name="a-nameclient-facebookasingle-sign-on-using-a-token-from-facebook-or-google"></a><a name="client-facebook"></a>Einmaliges Anmelden mit einem Token aus Facebook oder Google

Sie können Client illustrieren verwenden, wie in diesem Codeausschnitt für Facebook oder Google dargestellt.

    var token = new JObject();
    // Replace access_token_value with actual value of your access token obtained
    // using the Facebook or Google SDK.
    token.Add("access_token", "access_token_value");

    private MobileServiceUser user;
    private async Task AuthenticateAsync()
    {
        while (user == null)
        {
            string message;
            try
            {
                // Change MobileServiceAuthenticationProvider.Facebook
                // to MobileServiceAuthenticationProvider.Google if using Google auth.
                user = await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook, token);
                message = string.Format("You are now logged in - {0}", user.UserId);
            }
            catch (InvalidOperationException)
            {
                message = "You must log in. Login Required";
            }

            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
    }

####<a name="a-nameclient-livesdkasingle-sign-in-using-microsoft-account-with-the-live-sdk"></a><a name="client-livesdk"></a>Einzelne Anmeldung mit Microsoft Account mit dem Live SDK

Benutzerauthentifizierung, müssen Sie Ihre app auf der Microsoft-Konto-Entwicklercenter registrieren. Konfigurieren Sie die Registrierungsdetails auf Ihre Mobile-App Back-End-ein. Zum Erstellen der Registrierung eines Microsoft-Konto, und verbinden Sie es mit Ihrem Mobile-App Back-End-, die Schritte in [der app zum Verwenden eines Microsoft-Konto anmelden registrieren]. Wenn Sie sowohl Windows Store und Windows Phone 8/Silverlight-Versionen der app haben, registrieren Sie zuerst die Windows Store-Version.

Mit dem folgende Code Authentifizierung mit Live SDK und verwendet das zurückgegebene Token, um Ihre Mobile-App Back-End-anmelden.

    private LiveConnectSession session;
    //private static string clientId = "<microsoft-account-client-id>";
    private async System.Threading.Tasks.Task AuthenticateAsync()
    {

        // Get the URL the Mobile App backend.
        var serviceUrl = App.MobileService.ApplicationUri.AbsoluteUri;

        // Create the authentication client for Windows Store using the service URL.
        LiveAuthClient liveIdClient = new LiveAuthClient(serviceUrl);
        //// Create the authentication client for Windows Phone using the client ID of the registration.
        //LiveAuthClient liveIdClient = new LiveAuthClient(clientId);

        while (session == null)
        {
            // Request the authentication token from the Live authentication service.
            // The wl.basic scope should always be requested.  Other scopes can be added
            LiveLoginResult result = await liveIdClient.LoginAsync(new string[] { "wl.basic" });
            if (result.Status == LiveConnectSessionStatus.Connected)
            {
                session = result.Session;

                // Get information about the logged-in user.
                LiveConnectClient client = new LiveConnectClient(session);
                LiveOperationResult meResult = await client.GetAsync("me");

                // Use the Microsoft account auth token to sign in to App Service.
                MobileServiceUser loginResult = await App.MobileService
                    .LoginWithMicrosoftAccountAsync(result.Session.AuthenticationToken);

                // Display a personalized sign-in greeting.
                string title = string.Format("Welcome {0}!", meResult.Result["first_name"]);
                var message = string.Format("You are now logged in - {0}", loginResult.UserId);
                var dialog = new MessageDialog(message, title);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
            else
            {
                session = null;
                var dialog = new MessageDialog("You must log in.", "Login Required");
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }
    }

Weitere Informationen finden Sie in der [Windows Live SDK] -Dokumentation.

###<a name="a-nameserverflowaserver-managed-authentication"></a><a name="serverflow"></a>Verwaltete Server Authentifizierung

Nachdem Sie Ihre Identitätsanbieter registriert haben, rufen Sie die Methode [LoginAsync] auf die [MobileServiceClient] mit dem Wert [MobileServiceAuthenticationProvider] von Ihrem Anbieter. Beispielsweise initiiert der folgende Code ein Server Fluss-Anmelden mit Facebook ein.

    private MobileServiceUser user;
    private async System.Threading.Tasks.Task Authenticate()
    {
        while (user == null)
        {
            string message;
            try
            {
                user = await client
                    .LoginAsync(MobileServiceAuthenticationProvider.Facebook);
                message =
                    string.Format("You are now logged in - {0}", user.UserId);
            }
            catch (InvalidOperationException)
            {
                message = "You must log in. Login Required";
            }

            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
    }

Wenn Sie mit einen Identitätsanbieter als Facebook arbeiten, ändern Sie den Wert von [MobileServiceAuthenticationProvider] auf den Wert für Ihren Anbieter.

In einen Fluss Server verwaltet Azure-App-Verwaltungsdienst OAuth Authentifizierung illustrieren informiert der Anmeldeseite von den ausgewählten Anbieter.  Nachdem die Identität Anbieter gibt Azure-App-Verwaltungsdienst generiert ein App-Dienst Authentifizierungstoken. Die [LoginAsync] Methode gibt [MobileServiceUser], die sowohl die [Benutzer-ID] des authentifizierten Benutzer der [MobileServiceAuthenticationToken], als JSON Web Token (JWT) bereitstellt. Dieses Token kann zwischengespeichert werden und wiederverwendet, bis sie abgelaufen ist. Weitere Informationen finden Sie unter [Authentifizierungstoken Zwischenspeichern](#caching).

###<a name="a-namecachingacaching-the-authentication-token"></a><a name="caching"></a>Das Authentifizierungstoken Zwischenspeichern

In einigen Fällen kann der Anruf an die Login-Methode nach der ersten erfolgreichen Authentifizierung vermieden werden durch das Speichern des Authentifizierungstokens vom Anbieter.  Apps für Windows Store und UWP können [PasswordVault] zwischenspeichern, das aktuelle Authentifizierungstoken nach dem erfolgreiche Anmeldung wie folgt verwenden:

    await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook);

    PasswordVault vault = new PasswordVault();
    vault.Add(new PasswordCredential("Facebook", client.currentUser.UserId,
        client.currentUser.MobileServiceAuthenticationToken));

Der Benutzer-ID-Wert wird als Benutzername der Anmeldeinformationen gespeichert und das Token ist die gespeicherte als das Kennwort ein. Nachfolgende neue Mitbewerber können Sie die **PasswordVault** für zwischengespeicherten Anmeldeinformationen überprüfen. Im folgende Beispiel verwendet zwischengespeicherte Anmeldeinformationen an, wenn diese gefunden werden, und andernfalls versucht, erneut mit der Back-End-authentifizieren:

    // Try to retrieve stored credentials.
    var creds = vault.FindAllByResource("Facebook").FirstOrDefault();
    if (creds != null)
    {
        // Create the current user from the stored credentials.
        client.currentUser = new MobileServiceUser(creds.UserName);
        client.currentUser.MobileServiceAuthenticationToken =
            vault.Retrieve("Facebook", creds.UserName).Password;
    }
    else
    {
        // Regular login flow and cache the token as shown above.
    }

Wenn Sie sich ein Benutzer sich anmelden, müssen Sie die gespeicherte Anmeldeinformationen wie folgt entfernen:

    client.Logout();
    vault.Remove(vault.Retrieve("Facebook", client.currentUser.UserId));

[Xamarin.Auth] APIs Formular Xamarin-apps mit um Anmeldeinformationen in Objekt **Konto** sicher zu speichern. Ein Beispiel für die Verwendung dieser APIs finden Sie im [Beispiel für die Freigabe von ContosoMoments Fotos]Codedatei [AuthStore.cs] .

Wenn Sie eine verwaltete Client Authentifizierung verwenden, können Sie auch das Zugriffstoken erhalten von Ihrem Anbieter wie Facebook oder Twitter Zwischenspeichern. Dieses Token kann bereitgestellt werden, um ein neues Authentifizierungstoken wie folgt aus dem Back-End, anfordern:

    var token = new JObject();
    // Replace <your_access_token_value> with actual value of your access token
    token.Add("access_token", "<your_access_token_value>");

    // Authenticate using the access token.
    await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook, token);

##<a name="a-namepushnotificationsapush-notifications"></a><a name="pushnotifications"></a>Pushbenachrichtigungen

Pushbenachrichtigungen Sie in den folgenden Themen:

* [Registrieren für Pushbenachrichtigungen](#register-for-push)
* [Erhalten einer Windows Store-Paket SID](#package-sid)
* [Registrieren Sie sich mit Plattform-Vorlagen](#register-xplat)

###<a name="a-nameregister-for-pushahow-to-register-for-push-notifications"></a><a name="register-for-push"></a>So: Registrieren für Pushbenachrichtigungen

Der Client Mobile-Apps können Sie für Pushbenachrichtigungen mit Azure Benachrichtigung Hubs registrieren. Beim Registrieren, erhalten Sie einen Ziehpunkt, den Sie von der Plattform-spezifische Pushbenachrichtigungen Benachrichtigung Service (PNS) erhalten. Sie geben Sie dann diesen Wert sowie beliebige Tags beim Erstellen der Registrierung. Der folgende Code registriert Ihre Windows-app für Pushbenachrichtigungen mit der Windows-Infobereich-Dienst (WNS):

    private async void InitNotificationsAsync()
    {
        // Request a push notification channel.
        var channel =await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

        // Register for notifications using the new channel.
        await MobileService.GetPush().RegisterNativeAsync(channel.Uri, null);
    }

Wenn Sie zum WNS verlangen, müssen Sie [ein Windows Store-Paket SID zu erhalten](#package-sid).  Weitere Informationen zu Windows-apps, z. B. zum Registrieren für Vorlage Registrierungen, finden Sie unter [Hinzufügen von Pushbenachrichtigungen zu Ihrer Anwendung].

Anfordern von Kategorien von dem Client wird nicht unterstützt.  Kategorie Besprechungsanfragen werden im Hintergrund aus der Registrierung gelöscht.
Wenn Sie Ihr Gerät mit Kategorien registrieren möchten, erstellen Sie eine benutzerdefinierte-API, die die Benachrichtigung Hubs-API verwendet, um die Registrierung in Ihrem Auftrag durchzuführen.  [Rufen Sie die benutzerdefinierte-API](#customapi) statt der `RegisterNativeAsync()` Methode.

###<a name="a-namepackage-sidahow-to-obtain-a-windows-store-package-sid"></a><a name="package-sid"></a>So: erhalten einer Windows Store-Paket SID

Ein Paket SID ist erforderlich, für die Aktivierung von Pushbenachrichtigungen in Windows Store-apps.  Um ein Paket SID zu erhalten, Registrieren der Anwendungs im Windows Store.

Um diesen Wert zu erhalten:

1. In Visual Studio Solution Explorer Maustaste auf das Windows Store-app-Projekt, und klicken Sie auf **Store** > **App mit dem Store... zugeordnet werden soll**.
2. Klicken Sie im Assistenten klicken Sie auf **Weitersuchen**, melden Sie sich mit Ihrem Microsoft-Konto, geben Sie einen Namen für Ihre app in **Reservieren einen neuen Namen für die app**, klicken Sie auf **Reservieren**.
3. Nachdem die app-Registrierung erfolgreich erstellt wurde, wählen Sie den Namen der Anwendung, klicken Sie auf **Weiter**, und klicken Sie dann auf **Verbinden**.
4. Melden Sie sich mit Ihrem Microsoft-Account [Windows Developer Center] . Klicken Sie unter **Meine apps**klicken Sie auf die app-Registrierung, die Sie erstellt haben.
5. Klicken Sie auf die **App Management** > **App Identität**, und klicken Sie dann führen Sie einen Bildlauf nach unten zu Ihrem **Paket SID**gefunden.

Vielen Verwendungsmöglichkeiten des Pakets SID als behandelt einen URI, in diesem Fall müssen Sie verwenden _ms-app: / /_ als Schema. Notieren Sie die Version von Ihrem Paket SID durch Verkettung von diesen Wert als Präfix geformt.

Xamarin apps erfordern einige zusätzlichen Code zum Registrieren eine app für die iOS oder Android Plattformen ausgeführt werden. Weitere Informationen finden Sie unter dem Thema für Ihre Plattform:

* [Xamarin.Android](app-service-mobile-xamarin-android-get-started-push.md#add-push)
* [Xamarin.iOS](app-service-mobile-xamarin-ios-get-started-push.md#add-push)

###<a name="a-nameregister-xplatahow-to-register-push-templates-to-send-cross-platform-notifications"></a><a name="register-xplat"></a>So: Register Pushbenachrichtigungen Vorlagen Plattformen Benachrichtigungen senden

Um Vorlagen zu registrieren, verwenden Sie die `RegisterAsync()` Methode mit Vorlagen, wie folgt:

        JObject templates = myTemplates();
        MobileService.GetPush().RegisterAsync(channel.Uri, templates);

Ihre Vorlagen sollten `JObject` Dateitypen und kann mehrere Vorlagen in der folgenden JSON-Format enthalten:

        public JObject myTemplates()
        {
            // single template for Windows Notification Service toast
            var template = "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(message)</text></binding></visual></toast>";

            var templates = new JObject
            {
                ["generic-message"] = new JObject
                {
                    ["body"] = template,
                    ["headers"] = new JObject
                    {
                        ["X-WNS-Type"] = "wns/toast"
                    },
                    ["tags"] = new JArray()
                },
                ["more-templates"] = new JObject {...}
            };
            return templates;
        }

Die Methode **RegisterAsync()** nimmt auch sekundäre Kacheln:

        MobileService.GetPush().RegisterAsync(string channelUri, JObject templates, JObject secondaryTiles);

Alle Kategorien werden während der Registrierung für Sicherheit abwesend entfernt. Um Kategorien Installationen oder Vorlagen in Installationen hinzuzufügen, finden Sie unter [arbeiten mit der Back-End-Server SDK für Mobile-Apps Azure].

Zum Verwenden von Vorlagen, diese registrierten Benachrichtigungen senden möchten, finden Sie in der [Benachrichtigung Hubs APIs].

##<a name="a-namemiscamiscellaneous-topics"></a><a name="misc"></a>Sonstige Themen

###<a name="a-nameerrorsahow-to-handle-errors"></a><a name="errors"></a>So: Fehler

Tritt ein Fehler in die Back-End-, löst das Client SDK aus einer `MobileServiceInvalidOperationException`.  Im folgenden Beispiel wird gezeigt, wie eine Ausnahme behandelt, die von der Back-End-zurückgegeben wird:

    private async void InsertTodoItem(TodoItem todoItem)
    {
        // This code inserts a new TodoItem into the database. When the operation completes
        // and App Service has assigned an Id, the item is added to the CollectionView
        try
        {
            await todoTable.InsertAsync(todoItem);
            items.Add(todoItem);
        }
        catch (MobileServiceInvalidOperationException e)
        {
            // Handle error
        }
    }

Ein weiteres Beispiel für den Umgang mit Fehlern finden Sie in der [Stichprobe für Mobile Apps Dateien]. Im Beispiel [LoggingHandler] bietet einen Protokollierung Stellvertretung Ereignishandler (vor), die Anforderungen an die Back-End-melden Sie sich an.

###<a name="a-nameheadersahow-to-customize-request-headers"></a><a name="headers"></a>So: Anpassen von Kopfzeilen anfordern

Zur Unterstützung von Ihrem Szenarios bestimmten app müssen Sie die Kommunikation mit dem Mobile-App Back-End anpassen. Angenommen, möchten Sie einen benutzerdefinierten Header jeder ausgehenden Anforderung hinzufügen oder sogar ändern Antworten Statuscodes. Sie können eine benutzerdefinierte [DelegatingHandler], wie im folgenden Beispiel:

    public async Task CallClientWithHandler()
    {
        HttpResponseMessage[]
        MobileServiceClient client = new MobileServiceClient("AppUrl",
            new MyHandler()
            );
        IMobileServiceTable<TodoItem> todoTable = client.GetTable<TodoItem>();
        var newItem = new TodoItem { Text = "Hello world", Complete = false };
        await todoTable.InsertAsync(newItem);
    }

    public class MyHandler : DelegatingHandler
    {
        protected override async Task<HttpResponseMessage>
            SendAsync(HttpRequestMessage request, CancellationToken cancellationToken)
        {
            // Change the request-side here based on the HttpRequestMessage
            request.Headers.Add("x-my-header", "my value");

            // Do the request
            var response = await base.SendAsync(request, cancellationToken);

            // Change the response-side here based on the HttpResponseMessage

            // Return the modified response
            return response;
        }
    }


<!-- Anchors. -->


<!-- Images. -->

<!-- URLs. -->
[1]: app-service-mobile-windows-store-dotnet-get-started.md
[2]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[3]: app-service-mobile-node-backend-how-to-use-server-sdk.md
[4]: https://msdn.microsoft.com/en-us/library/azure/mt419521(v=azure.10).aspx
[5]: https://github.com/Azure-Samples
[6]: http://www.newtonsoft.com/json/help/html/Properties_T_Newtonsoft_Json_JsonPropertyAttribute.htm
[7]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#how-to-define-a-table-controller
[8]: app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations
[9]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/
[10]: http://www.symbolsource.org/
[11]: http://www.symbolsource.org/Public/Wiki/Using
[12]: https://msdn.microsoft.com/en-us/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient(v=azure.10).aspx

[Authentifizierung für Ihre app hinzufügen]: app-service-mobile-windows-store-dotnet-get-started-users.md
[Offline-Daten synchronisieren in Azure Mobile-Apps]: app-service-mobile-offline-data-sync.md
[Hinzufügen von Pushbenachrichtigungen zu Ihrer Anwendung]: app-service-mobile-windows-store-dotnet-get-started-push.md
[Registrieren Sie Ihre app zum Verwenden eines Microsoft-Konto anmelden]: app-service-mobile-how-to-configure-microsoft-authentication.md
[Konfigurieren der App-Dienst für Active Directory-Anmeldung]: app-service-mobile-how-to-configure-active-directory-authentication.md

<!-- Microsoft URLs. -->
[MobileServiceCollection]: https://msdn.microsoft.com/en-us/library/azure/dn250636(v=azure.10).aspx
[MobileServiceIncrementalLoadingCollection]: https://msdn.microsoft.com/en-us/library/azure/dn268408(v=azure.10).aspx
[MobileServiceAuthenticationProvider]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceauthenticationprovider(v=azure.10).aspx
[MobileServiceUser]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser(v=azure.10).aspx
[MobileServiceAuthenticationToken]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser.mobileserviceauthenticationtoken(v=azure.10).aspx
[GetTable]: https://msdn.microsoft.com/en-us/library/azure/jj554275(v=azure.10).aspx
[erstellt einen Verweis auf eine nicht typisierte Tabelle]: https://msdn.microsoft.com/en-us/library/azure/jj554278(v=azure.10).aspx
[DeleteAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296407(v=azure.10).aspx
[IncludeTotalCount]: https://msdn.microsoft.com/en-us/library/azure/dn250560(v=azure.10).aspx
[InsertAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296400(v=azure.10).aspx
[InvokeApiAsync]: https://msdn.microsoft.com/en-us/library/azure/dn268343(v=azure.10).aspx
[LoginAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296411(v=azure.10).aspx
[LookupAsync]: https://msdn.microsoft.com/en-us/library/azure/jj871654(v=azure.10).aspx
[OrderBy]: https://msdn.microsoft.com/en-us/library/azure/dn250572(v=azure.10).aspx
[OrderByDescending]: https://msdn.microsoft.com/en-us/library/azure/dn250568(v=azure.10).aspx
[ReadAsync]: https://msdn.microsoft.com/en-us/library/azure/mt691741(v=azure.10).aspx
[Übernehmen]: https://msdn.microsoft.com/en-us/library/azure/dn250574(v=azure.10).aspx
[Wählen Sie aus]: https://msdn.microsoft.com/en-us/library/azure/dn250569(v=azure.10).aspx
[Überspringen]: https://msdn.microsoft.com/en-us/library/azure/dn250573(v=azure.10).aspx
[UpdateAsync]: https://msdn.microsoft.com/en-us/library/azure/dn250536.(v=azure.10)aspx
[Benutzer-ID]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser.userid(v=azure.10).aspx
[WHERE]: https://msdn.microsoft.com/en-us/library/azure/dn250579(v=azure.10).aspx
[Azure-portal]: https://portal.azure.com/
[Azure klassischen-portal]: https://manage.windowsazure.com/
[EnableQueryAttribute]: https://msdn.microsoft.com/library/system.web.http.odata.enablequeryattribute.aspx
[Guid.NewGuid]: https://msdn.microsoft.com/en-us/library/system.guid.newguid(v=vs.110).aspx
[ISupportIncrementalLoading]: http://msdn.microsoft.com/library/windows/apps/Hh701916.aspx
[Windows Developer Center]: https://dev.windows.com/en-us/overview
[DelegatingHandler]: https://msdn.microsoft.com/library/system.net.http.delegatinghandler(v=vs.110).aspx
[Windows Live SDK]: https://msdn.microsoft.com/en-us/library/bb404787.aspx
[PasswordVault]: http://msdn.microsoft.com/library/windows/apps/windows.security.credentials.passwordvault.aspx
[ProtectedData]: http://msdn.microsoft.com/library/system.security.cryptography.protecteddata%28VS.95%29.aspx
[Benachrichtigung Hubs APIs]: https://msdn.microsoft.com/library/azure/dn495101.aspx
[Beispiel für Mobile Apps-Dateien]: https://github.com/Azure-Samples/app-service-mobile-dotnet-todo-list-files
[LoggingHandler]: https://github.com/Azure-Samples/app-service-mobile-dotnet-todo-list-files/blob/master/src/client/MobileAppsFilesSample/Helpers/LoggingHandler.cs#L63

<!-- External URLs -->
[Dokumentation für OData v3]: http://www.odata.org/documentation/odata-version-3-0/
[Fiddler]: http://www.telerik.com/fiddler
[Json.NET]: http://www.newtonsoft.com/json
[Xamarin.Auth]: https://components.xamarin.com/view/xamarin.auth/
[AuthStore.cs]: (https://github.com/azure-appservice-samples/ContosoMoments/blob/dev/src/Mobile/ContosoMoments/Helpers/AuthStore.cs)
[Beispiel für die Freigabe von ContosoMoments Fotos]: https://github.com/azure-appservice-samples/ContosoMoments
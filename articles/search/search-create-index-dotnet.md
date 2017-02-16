<properties
    pageTitle="Erstellen einer Azure Suchindex mit .NET SDK | Microsoft Azure | Cloud gehosteten Suchdienst"
    description="Erstellen eines Indexes Code mit .NET SDK Azure suchen."
    services="search"
    documentationCenter=""
    authors="brjohnstmsft"
    manager="jhubbard"
    editor=""
    tags="azure-portal"/>

<tags
    ms.service="search"
    ms.devlang="dotnet"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="brjohnst"/>

# <a name="create-an-azure-search-index-using-the-net-sdk"></a>Erstellen einer Azure Suchindex mit .NET SDK
> [AZURE.SELECTOR]
- [(Übersicht)](search-what-is-an-index.md)
- [Portal](search-create-index-portal.md)
- [.NET](search-create-index-dotnet.md)
- [REST](search-create-index-rest-api.md)


In diesem Artikel werden Sie den Vorgang des Erstellens eines Azure [Index](https://msdn.microsoft.com/library/azure/dn798941.aspx) mit dem [Azure Suche .NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx)durchzuführen.

Vor diesem Handbuch folgen und Erstellen eines Indexes, sollten Sie bereits [erstellt einen Azure Suchdienst](search-create-service-portal.md)verfügen.

Beachten Sie, dass alle Stichprobe Code in diesem Artikel in c# geschrieben ist. Sie können die vollständige Quelle suchen [auf GitHub](http://aka.ms/search-dotnet-howto)Fehlercode.

## <a name="i-identify-your-azure-search-services-admin-api-key"></a>Ich. Identifizieren Sie Ihrer Azure Suchdienst Admin-api-Taste
Jetzt, da Sie einen Suchdienst aus Azure bereitgestellt haben, können Sie fast Anfragen anhand Ihrer mit .NET SDK-Endpunkt ausgeben. Zuerst müssen Sie eine der Admin-api-Tasten, die für den Suchdienst generiert wurde, die Sie nach der Bereitstellung zu erhalten. .NET SDK wird dieser api-Taste bei jeder Anforderung an den Dienst gesendet werden. Gibt es einen gültigen Key stellt vertrauen, auf Basis pro Anforderung, zwischen der Anwendung senden der Anfrage und der Dienst, der die Behandlung ganzer her.

1. Aufrufen des Diensts api-Schlüssel finden müssen Sie sich bei der [Azure-Portal](https://portal.azure.com/) anmelden
2. Wechseln Sie zu Ihrer Suche Azure-Service-Karte vorausgesetzt
3. Klicken Sie auf das Symbol "Tasten"

Der Dienst, *Administrator Tasten* und die *Abfrage Tasten*müssen.

  - Ihre primären und sekundären *Administrator Tasten* erteilen Vollzugriff auf alle Vorgänge, einschließlich der Berechtigung zum Verwalten des Diensts, erstellen und Löschen von Indizes, Indexer und Datenquellen. Es gibt zwei Tasten, damit Sie fortsetzen können, um die sekundäre Key zu verwenden, wenn Sie sich entscheiden, den Primärschlüssel und umgekehrt neu zu generieren.
  - Ihre *Abfrage Tasten* schreibgeschützten Zugriff auf Indizes und Dokumente gewähren, und werden in der Regel-Clientanwendungen, die Suchabfragen ausgeben verteilt.

Im Sinne des Erstellens eines Indexes, können Sie mithilfe einer der primären oder sekundären Administrator-Taste.

<a name="CreateSearchServiceClient"></a>
## <a name="ii-create-an-instance-of-the-searchserviceclient-class"></a>II. Erstellen Sie eine Instanz der Klasse SearchServiceClient
Zum Verwenden von .NET SDK Azure suchen, müssen Sie zum Erstellen einer Instanz von der `SearchServiceClient` Class. Diese Klasse verfügt über mehrere Konstruktoren. Eine gewünschte Ihrem Dienst Suchbegriff akzeptiert und eine `SearchCredentials` Objekt als Parameter. `SearchCredentials`umgebrochen wird Ihre api-Taste.

Im folgenden Code wird ein neues `SearchServiceClient` von Werten für den Suchbegriff-Dienst und api-Taste, die in der Konfigurationsdatei der Anwendung gespeichert werden (`app.config` oder `web.config`):

```csharp
string searchServiceName = ConfigurationManager.AppSettings["SearchServiceName"];
string adminApiKey = ConfigurationManager.AppSettings["SearchServiceAdminApiKey"];

SearchServiceClient serviceClient = new SearchServiceClient(searchServiceName, new SearchCredentials(adminApiKey));
```

`SearchServiceClient`verfügt über eine `Indexes` Eigenschaft. Diese Eigenschaft enthält alle Methoden, die Sie erstellen, Liste müssen aktualisieren oder Löschen von Indizes Azure durchsuchen.

> [AZURE.NOTE] Die `SearchServiceClient` Klasse verwaltet Verbindungen mit Ihrem Suchdienst. Um zu vermeiden, zu viele Verbindungen zu öffnen, sollten Sie versuchen zum Freigeben einer einzelnen Instanz des `SearchServiceClient` in Ihrer Anwendung falls möglich. Seine Methoden werden Thread-sicher solche Freigabe zu aktivieren.

<a name="DefineIndex"></a>
## <a name="iii-define-your-azure-search-index-using-the-index-class"></a>III. Definieren Sie Ihre Suche Azure Index mit der `Index` Klasse
Einen einzelnen Anruf an die `Indexes.Create` Methode den Index erstellt. Diese Methode übernimmt als Parameter eine `Index` Objekt, Azure Suchindex definiert. Sie müssen zum Erstellen einer `Index` Objekt und Initialisierung Sie ihn wie folgt:

1. Festlegen der `Name` -Eigenschaft der `Index` Objekt auf den Namen des Indexes.
2. Festlegen der `Fields` -Eigenschaft der `Index` Objekt in ein Array von `Field` Objekte. Jede der `Field` Objekte definiert das Verhalten eines Felds in Ihrem Index. Sie können den Namen des Felds, das den Konstruktor, zusammen mit dem Datentyp (oder Analyzer für Zeichenfolgenfelder) angeben. Sie können auch andere Eigenschaften wie festlegen `IsSearchable`, `IsFilterable`usw..

Es ist wichtig, dass Sie Ihre Suche Benutzer Erfahrung und Business-Anforderungen beim Entwerfen des Indexes wie jede Bedenken `Field` muss die [entsprechende Eigenschaften](https://msdn.microsoft.com/library/azure/dn798941.aspx)zugewiesen werden. Diese Eigenschaften-Steuerelement, das Features (filtern, Faceting, Sortierung voll-Textsuche usw.) suchen beziehen sich auf die Felder. Für jede Eigenschaft, die Sie nicht explizit festlegen, die `Field` Klasse Standardeinstellungen zum Deaktivieren der entsprechenden Suchfunktion, es sei denn, Sie ihn ausdrücklich aktivieren.

In unserem Beispiel haben wir unserem Index "Hotels" den Namen und unsere Felder wie folgt definiert:

```csharp
var definition = new Index()
{
    Name = "hotels",
    Fields = new[]
    {
        new Field("hotelId", DataType.String)                       { IsKey = true, IsFilterable = true },
        new Field("baseRate", DataType.Double)                      { IsFilterable = true, IsSortable = true, IsFacetable = true },
        new Field("description", DataType.String)                   { IsSearchable = true },
        new Field("description_fr", AnalyzerName.FrLucene),
        new Field("hotelName", DataType.String)                     { IsSearchable = true, IsFilterable = true, IsSortable = true },
        new Field("category", DataType.String)                      { IsSearchable = true, IsFilterable = true, IsSortable = true, IsFacetable = true },
        new Field("tags", DataType.Collection(DataType.String))     { IsSearchable = true, IsFilterable = true, IsFacetable = true },
        new Field("parkingIncluded", DataType.Boolean)              { IsFilterable = true, IsFacetable = true },
        new Field("smokingAllowed", DataType.Boolean)               { IsFilterable = true, IsFacetable = true },
        new Field("lastRenovationDate", DataType.DateTimeOffset)    { IsFilterable = true, IsSortable = true, IsFacetable = true },
        new Field("rating", DataType.Int32)                         { IsFilterable = true, IsSortable = true, IsFacetable = true },
        new Field("location", DataType.GeographyPoint)              { IsFilterable = true, IsSortable = true }
    }
};
```

Wir haben für jede sorgfältig die Eigenschaftswerte `Field` basierend auf wie wir glauben, dass diese in einer Anwendung verwendet werden. Beispielsweise ist zu rechnen, dass Benutzer suchen nach Hotels interessiert Schlüsselwort entspricht, klicken Sie auf die `description` Feld, damit wir voll-Textsuche für dieses Feld durch Festlegen aktivieren `IsSearchable` auf `true`.

Bitte beachten Sie die genau ein Feld in Ihrem Index vom Typ `DataType.String` muss durch Festlegen der vorgesehenen als Feld _Schlüssel_ `IsKey` auf `true` (finden Sie unter `hotelId` im obigen Beispiel).

Die obige Indexdefinition verwendet eine benutzerdefinierte Sprache Analyse für die `description_fr` Feld, da sie zum Speichern von französischen Text vorgesehen ist. Finden Sie [die Sprache auf der MSDN-Thema unterstützen](https://msdn.microsoft.com/library/azure/dn879793.aspx) sowie die entsprechenden [Blogbeitrag](https://azure.microsoft.com/blog/language-support-in-azure-search/) für Weitere Informationen zu Sprache Analyzern.

> [AZURE.NOTE]  Beachten Sie, dass durch übergeben `AnalyzerName.FrLucene` im Konstruktor der `Field` werden automatisch vom Typ `DataType.String` und haben `IsSearchable` legen Sie auf `true`.

## <a name="iv-create-the-index"></a>IV. Erstellen des Indexes
Nachdem Sie nun ein initialisiertes stehen Ihnen `Index` Objekt, können Sie den Index erstellen, einfach durch Aufrufen `Indexes.Create` auf Ihre `SearchServiceClient` Objekt:

```csharp
serviceClient.Indexes.Create(definition);
```

Für eine erfolgreiche Anforderung wird die Methode normalerweise zurück. Ist ein Problem mit der Anforderung, wie etwa einen ungültigen Parameter, die Methode löst `CloudException`.

Wenn Sie mit einem Index fertig sind und sie löschen möchten, rufen Sie einfach die `Indexes.Delete` Methode auf Ihre `SearchServiceClient`. Dies ist z. B. wie wir "Hotels" Index löschen möchten:

```csharp
serviceClient.Indexes.Delete("hotels");
```

> [AZURE.NOTE] Der Beispielcode in diesem Artikel verwendet die synchronen Methoden der Azure Suche .NET SDK zur Vereinfachung. Es empfiehlt sich, dass Sie die asynchronen Methoden in der Anwendung verwenden, damit diese skalierbare und reagiert beibehalten werden. Angenommen, in der obigen Beispielen können `CreateAsync` und `DeleteAsync` anstelle von `Create` und `Delete`.

## <a name="next"></a>Weiter
Nach dem Erstellen eines Indexes Azure suchen, werden Sie zum [Hochladen von Inhalten in den Index](search-what-is-data-import.md) bereit, sodass Sie beginnen können, Ihre Daten zu suchen.

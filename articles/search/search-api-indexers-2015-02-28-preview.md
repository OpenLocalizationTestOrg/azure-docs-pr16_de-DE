<properties 
pageTitle="Indizierungsvorgänge (Suche Azure Service REST-API: 2015-02-28-Vorschau) | Vorschau der Azure Suche API" 
description="Indizierungsvorgänge (Suche Azure Service REST-API: 2015-02-28-Preview)" 
services="search" 
documentationCenter="" 
authors="chaosrealm" 
manager="pablocas"
editor="" />

<tags 
ms.service="search" 
ms.devlang="rest-api" 
ms.workload="search" 
ms.topic="article"  
ms.tgt_pltfrm="na" 
ms.date="09/07/2016" 
ms.author="eugenesh" />

#<a name="indexer-operations-azure-search-service-rest-api-2015-02-28-preview"></a>Indizierungsvorgänge (Suche Azure Service REST-API: 2015-02-28-Preview)#

> [AZURE.NOTE] In diesem Artikel werden die Indexer in die [REST-API 2015-02-28-Vorschau](search-api-2015-02-28-preview.md). Diese Version API addiert Preview-Versionen Azure BLOB-Speicher Indexers mit einem Dokument Extraktion und Azure Table Storage Indexer sowie andere Optimierungen an. Die API unterstützt auch allgemein verfügbare (GA) Indexern, einschließlich Indexern Azure SQL-Datenbank, SQL Server auf Azure-virtuellen Computern und Azure DocumentDB.

## <a name="overview"></a>(Übersicht) ##

Azure suchen kann direkt mit einige allgemeine Datenquellen integrieren stattfinden muss Code schreiben, um Ihre Daten zu indizieren. Um dies von eingerichtet haben, können Sie die Suche Azure-API zum Erstellen und Verwalten von **Indexer** und **Datenquellen**aufrufen. 

Ein **Indexer** ist eine Ressource, die Datenquellen mit Ziel Suchindizes eine Verbindung herstellt. Ein Indexer wird auf folgende Weise verwendet: 

- Führen Sie eine einmalige Kopie der Daten ein Indexes gefüllt wird.
- Synchronisieren eines Indexes mit Änderungen in der Datenquelle nach einem Zeitplan. Der Zeitplan ist Bestandteil der Definition Indexer.
- Rufen Sie bei Bedarf aktualisieren ein Indexes, je nach Bedarf an. 

Ein **Indexer** ist nützlich, wenn Sie regelmäßig Updates für einen Index möchten. Sie können entweder einen Zeitplan Inline als Teil eines Indexer Definition einrichten oder es bei Bedarf über [Indexer ausführen](#RunIndexer)ausführen. 

Eine **Datenquelle** gibt an, welche Daten indiziert werden müssen, den Zugriff auf die Daten und Richtlinien für den Azure-Suche auf Änderungen in den Daten (z. B. geändert oder gelöscht Zeilen in einer Datenbanktabelle) effizient identifizieren Anmeldeinformationen. Es wird als eine unabhängige Ressource definiert, damit es von mehrere Indexer verwendet werden kann.

Die folgenden Datenquellen werden derzeit unterstützt:

- **SQL Azure-Datenbank** und **SQLServer auf Azure-virtuellen Computern**. Eine gezielte – Exemplarische Vorgehensweise finden Sie [in diesem Artikel](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md). 
- **Azure DocumentDB**. Eine gezielte – Exemplarische Vorgehensweise finden Sie [in diesem Artikel](../documentdb/documentdb-search-indexer.md). 
- **Azure BLOB-Speicher**, einschließlich der folgenden Dateiformate: PDF, Microsoft Office (DOCX/DOC, XSLX/XLS, PPTX/PPT, MSG), HTML, XML, ZIP und nur-Text-Dateien (einschließlich JSON). Eine gezielte – Exemplarische Vorgehensweise finden Sie [in diesem Artikel](search-howto-indexing-azure-blob-storage.md).
- **Azure Table Storage**. Eine gezielte – Exemplarische Vorgehensweise finden Sie [in diesem Artikel](search-howto-indexing-azure-tables.md).
     
Wir haben die Unterstützung weiterer Datenquellen in der Zukunft hinzufügen in Betracht ziehen. Helfen Sie uns priorisieren Sie diese Entscheidungen, geben Sie Ihr Feedback über das [Feedback-Forum Azure suchen](http://feedback.azure.com/forums/263029-azure-search)aus.

Maximale Beschränkungen im Zusammenhang mit Indexer und Daten Quellressourcen finden Sie unter [Beschränkungen Service](search-limits-quotas-capacity.md) .

## <a name="typical-usage-flow"></a>Typische Verwendung Fluss ##

Sie können erstellen und Verwalten von Indexern und Datenquellen über einfachen HTTP-Anfragen (bereitstellen, abrufen, sich löschen) mit einer angegebenen `data source` oder `indexer` Ressource.

Einrichten von automatischen Indizierung erfolgt in der Regel in vier Schritten:

1. Benennen Sie die Datenquelle, die die Daten enthält, die indiziert werden muss. Beachten Sie, dass Azure-Suche unterstützt nicht alle die Datentypen in Ihrer Datenquelle möglicherweise beibehalten. Finden Sie unter [unterstützte Datentypen](https://msdn.microsoft.com/library/azure/dn798938.aspx) für die Liste ein.

2. Erstellen einer Azure Suchindex, deren Schema mit Ihrer Datenquelle kompatibel ist.
  
3. Erstellen Sie eine Datenquelle Azure suchen, wie in [Der Datenquelle erstellen](#CreateDataSource)beschrieben.
  
4. Erstellen eines Indexers Azure suchen wie beschrieben [Indexer erstellen](#CreateIndexer).

Planen Sie eine Indexersteller für jede Kombination Ziel Index und Daten Quelle zu erstellen. Sie können mehrere Indexer in denselben Index schreiben haben und Sie können dieselbe Datenquelle für mehrere Indexer wiederverwenden. Indexer kann nur eine Datenquelle nacheinander nutzen und kann nur in einem einzigen Index schreiben. 

Nach dem Erstellen eines Indexers, können Sie dessen Ausführungsstatus mithilfe des Vorgangs [Erhalten Indizierungsstatus](#GetIndexerStatus) abrufen. Sie können auch einen Indexer ausführen, zu einem beliebigen Zeitpunkt (anstelle von oder zusätzlich zu dem regelmäßig nach einem Zeitplan ausgeführt) verwenden den Indizierungsvorgang mit [Ausführen](#RunIndexer) .

<!-- MSDN has 2 art files plus a API topic link list -->


## <a name="create-data-source"></a>Erstellen der Datenquelle ##

In Azure suchen wird eine Datenquelle mit Indexern, die Verbindungsinformationen für die ad-hoc- oder geplanten Daten aktualisieren eines Indexes Ziel bereitstellen verwendet. Sie können eine neue Datenquelle in einem Azure-Suchdienst mithilfe einer HTTP POST-Anforderung erstellen.
    
    POST https://[service name].search.windows.net/datasources?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

Alternativ können Sie sich verwenden, und geben Sie den Namen der Datenquelle auf den URI. Wenn die Datenquelle nicht vorhanden ist, wird er erstellt.

    PUT https://[service name].search.windows.net/datasources/[datasource name]?api-version=[api-version]

**Hinweis**: die maximal zulässige Anzahl von Datenquellen hängt vom Preise Ebene ab. Kostenlose Dienst können bis zu 3 Datenquellen. Standard-Dienst kann 50 Datenquellen. Details finden Sie unter [Beschränkungen Service](search-limits-quotas-capacity.md) .

**Anfordern**

HTTPS ist für alle Serviceanfragen erforderlich. Die Anfrage **Datenquelle erstellen** kann mithilfe entweder einen Beitrag, oder sich Methode erstellt werden. Wenn Sie POST verwenden zu können, müssen Sie einen Datenquellennamen in den Hauptteil der Anforderung sowie die DSN-Datei angeben. Mit sich ist der Name Teil der URL. Wenn die Datenquelle nicht vorhanden ist, wird er erstellt. Wenn sie bereits vorhanden ist, wird es an die neue Definition aktualisiert. 

Der Name der Datenquelle muss in Kleinbuchstaben angegeben werden, beginnen mit einem Buchstaben oder eine Zahl, haben keine Bindestriche oder Punkte und weniger als 128 Zeichen lang sein. Nach dem Start der Name der Datenquelle mit einem Buchstaben oder eine Zahl, kann die restlichen den Namen alle Buchstaben, Zahl und Striche, einbeziehen, solange die Striche nicht aufeinander folgen. Details finden Sie unter [Naming Regeln](https://msdn.microsoft.com/library/azure/dn857353.aspx) .

Die `api-version` ist erforderlich. Die aktuelle Version ist `2015-02-28`.

**Anfordern von Kopfzeilen**

Die folgende Liste beschreibt die erforderlichen und optionalen Anforderung Überschriften. 

- `Content-Type`: Erforderlich ist. Legen Sie den Wert`application/json`
- `api-key`: Erforderlich ist. Die `api-key` wird verwendet, um die Anfrage Suchdienst authentifizieren. Es ist ein Zeichenfolgenwert mit dem Dienst. Die **Datenquelle erstellen** Anforderung darf enthalten eine `api-key` Kopfzeile mit Ihrem Administrator Key (im Gegensatz zu einer Abfrageschlüssel) festlegen. 
 
Sie benötigen außerdem den Dienstnamen, um die Anfrage-URL zu erstellen. Können Sie sowohl den Dienstnamen erhalten und `api-key` aus Ihrem Dienst Dashboard im [Portal Azure](https://portal.azure.com/). Finden Sie unter [Erstellen eines Suchdiensts im Portal](search-create-service-portal.md) für die Seitennavigation helfen.

<a name="CreateDataSourceRequestSyntax"></a>
**Die Syntax der Textkörper anfordern**

Hauptteil der Anforderung enthält eine Datendefinitionsabfrage Quelle, wozu auch die Art der Datenquelle, Anmeldeinformationen ein, ändern die Daten als auch eine optionalen Daten Erkennung und Daten Löschvorgang Erkennungsrichtlinien, mit denen effizient identifizieren, geändert oder gelöscht werden Daten in der Datenquelle bei Verwendung mit einem regelmäßig geplanten Formatindexer lesen. 

Die Syntax für die Strukturierung der Anforderungsnutzlast lautet wie folgt aus. Eine Beispiel für eine Anforderung weiter unten in diesem Artikel bereitgestellt wird.

    { 
        "name" : "Required for POST, optional for PUT. The name of the data source",
        "description" : "Optional. Anything you want, or nothing at all",
        "type" : "Required. Must be one of 'azuresql', 'documentdb', 'azureblob', or 'azuretable'",
        "credentials" : { "connectionString" : "Required. Connection string for your data source" },
        "container" : { "name" : "Required. The name of the table, collection, or blob container you wish to index" },
        "dataChangeDetectionPolicy" : { Optional. See below for details }, 
        "dataDeletionDetectionPolicy" : { Optional. See below for details }
    }

Anforderung enthält die folgenden Eigenschaften: 

- `name`: Erforderlich ist. Der Name der Datenquelle. Ein Data Source Name muss nur Kleinbuchstaben, Ziffern oder Bindestriche enthalten, können nicht mit beginnen oder enden Striche und darf maximal 128 Zeichen.
- `description`: Eine optionale Beschreibung. 
- `type`: Erforderlich ist. Die unterstützten Datenquellentypen gültig sind:
    - `azuresql`-Azure SQL-Datenbank oder SQLServer auf Azure-virtuellen Computern
    - `documentdb`-Azure DocumentDB
    - `azureblob`-Azure Blob-Speicher
    - `azuretable`-Azure Table Storage
- `credentials`:
    - Die erforderlichen `connectionString` Eigenschaft gibt die Verbindungszeichenfolge für die Datenquelle an. Das Format der Verbindungszeichenfolge hängt von den Datenquellentyp ab: 
        - Dies ist für SQL Azure die üblichen SQL Server-Verbindungszeichenfolge. Wenn Sie zum Abrufen der Verbindungszeichenfolge Azure-Portal verwenden, verwenden Sie die `ADO.NET connection string` Option.
        - Für DocumentDB, muss die Verbindungszeichenfolge im folgenden Format: `"AccountEndpoint=https://[your account name].documents.azure.com;AccountKey=[your account key];Database=[your database id]"`. Alle Werte sind erforderlich. Sie können diese im [Portal Azure](https://portal.azure.com/)suchen.  
        - Azure Blob und Tabellenspeicher ist dies der Verbindungszeichenfolge für Speicher-Konto an. Beschreibung des Formats [hier](https://azure.microsoft.com/documentation/articles/storage-configure-connection-string/). HTTPS-Endpunkt-Protokoll ist erforderlich.  
- `container`, erforderlich: Gibt die Daten zu indizieren, mit der `name` und `query` Eigenschaften: 
    - `name`Erforderlich:
        - Azure SQL: Gibt die Tabelle oder Sicht. Sie verwenden die Schema qualifizierte Namen, z. B. `[dbo].[mytable]`.
        - DocumentDB: Gibt die Sammlung an. 
        - Azure Blob-Speicher: Gibt den Speichercontainer an.
        - Azure Table Storage: Gibt den Namen der Tabelle an. 
    - `query`, optional:
        - DocumentDB: ermöglicht Ihnen, eine Abfrage angeben, die Layout einer beliebigen JSON-Dokuments in einer flachen Schema fasst zusammen, die Suche Azure indizieren können.  
        - Azure Blob-Speicher: können Sie einen virtuellen Ordner innerhalb des Containers Blob angeben. Zum Beispiel für Blob-Pfad `mycontainer/documents/blob.pdf`, `documents` als virtueller Ordner verwendet werden können.
        - Azure Table Storage: ermöglicht Ihnen, eine Abfrage angeben, die die Menge der zu importierenden Zeilen gefiltert.
        - Azure SQL: Abfrage wird nicht unterstützt. Wenn Sie diese Funktion benötigen, stimmen Sie für [diese Vorschlag](https://feedback.azure.com/forums/263029-azure-search/suggestions/9893490-support-user-provided-query-in-sql-indexer)
- Das optionale `dataChangeDetectionPolicy` und `dataDeletionDetectionPolicy` Eigenschaften werden im folgenden beschrieben.

<a name="DataChangeDetectionPolicies"></a>
**Daten ändern Erkennungsrichtlinien**

Der Zweck einer Daten ändern Erkennung Richtlinie ist effizient geänderten Datenelemente zu identifizieren. Unterstützte Richtlinien variieren basierend auf den Datenquellentyp. In den folgenden Abschnitten wird jede Richtlinie beschrieben. 

***Das Kontingent für ändern Erkennung Richtlinie*** 

Verwenden Sie diese Richtlinie, wenn Ihre Datenquelle enthält eine Spalte oder Eigenschaft, die die folgenden Kriterien erfüllt:
 
- Fügt alle Geben Sie einen Wert für die Spalte. 
- Alle Updates zu einem Element ändern Sie auch den Wert der Spalte. 
- Der Wert in dieser Spalte wird bei jeder Änderung erhöht.
- Abfragen, die eine der folgenden ähnelt Filter-Klausel verwenden `WHERE [High Water Mark Column] > [Current High Water Mark Value]` effizient ausgeführt werden kann.

Beispielsweise beim Verwenden von SQL Azure-Datenquellen, eine indizierte `rowversion` Spalte ist das ideale Kandidat für die Verwendung mit mit der Richtlinie Hochwassermarken. 

Dieser Richtlinie kann wie folgt angegeben werden:

    { 
        "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
        "highWaterMarkColumnName" : "[a row version or last_updated column name]" 
    } 

> [AZURE.NOTE] Wenn DocumentDB Datenquellen verwenden zu können, müssen Sie verwenden die `_ts` Eigenschaft von DocumentDB bereitgestellt. 

> [AZURE.NOTE] Bei Azure Blob-Datenquellen, Azure Suche automatisch verwendet eine hohe Wasserzeichen ändern Erkennung Richtlinie basierend auf der letzten Änderung eines Blob-Timestamp; Sie benötigen keine solche Richtlinie selbst angeben.   

***SQL integriert ändern Erkennung Richtlinie***

Wenn die SQL-Datenbank [Ändern Verlauf](https://msdn.microsoft.com/library/bb933875.aspx)unterstützt, wird empfohlen, SQL integrierte ändern nachverfolgen Richtlinie verwenden. Diese Richtlinie ermöglicht effizientesten änderungsnachverfolgung und Azure-Suche auf gelöschte Zeilen zu identifizieren, ohne dass Sie eine Löschspalte explizit "Weiche" in Ihrem Schema haben.

Integrierte änderungsnachverfolgung wird unterstützt, beginnend mit den folgenden Versionen von SQL Server-Datenbank: 
- SQL Server 2008 R2, wenn Sie SQL Server auf Azure-virtuellen Computern verwenden.
- Azure SQL-Datenbank V12, wenn Sie Azure SQL-Datenbank verwenden.  

Wenn mit SQL integrierte nachverfolgung Richtlinie keine separate Daten Löschvorgang Erkennung Richtlinie angeben – dieser Richtlinie verfügt über eine integrierte Unterstützung zum Identifizieren von gelöschten Zeilen. 

Dieser Richtlinie kann nur mit Tabellen verwendet werden. Sie können mit Sichten verwendet werden. Sie müssen Aktivieren der änderungsnachverfolgung für die Tabelle, die Sie verwenden, bevor Sie diese Richtlinie verwenden können. Anweisungen finden Sie unter [Aktivieren und Deaktivieren von Verlauf ändern](https://msdn.microsoft.com/library/bb964713.aspx) .    
 
Wenn die Anfrage **Datenquelle erstellen** , die SQL integriert ändern Verlauf Richtlinie Strukturierung wie folgt angegeben werden kann:

    { 
        "@odata.type" : "#Microsoft.Azure.Search.SqlIntegratedChangeTrackingPolicy" 
    }

<a name="DataDeletionDetectionPolicies"></a>
**Daten löschen Erkennungsrichtlinien**

Der Zweck einer Daten Löschvorgang Erkennung Richtlinie ist effizient gelöschten Datenelemente zu identifizieren. Derzeit die einzige unterstützte Richtlinie ist die `Soft Delete` Richtlinie, wodurch identifizieren gelöschte Elemente basierend auf dem Wert, der eine `soft delete` Spalte oder Eigenschaft in der Datenquelle. Dieser Richtlinie kann wie folgt angegeben werden:

    { 
        "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
        "softDeleteColumnName" : "the column that specifies whether a row was deleted", 
        "softDeleteMarkerValue" : "the value that identifies a row as deleted" 
    }

**Hinweis:** Nur Spalten mit Zeichenfolge, ganze Zahl oder boolesche Werte werden unterstützt. Der Wert, der als verwendete `softDeleteMarkerValue` muss eine Zeichenfolge, selbst wenn die entsprechende Spalte ganze Zahlen oder boolesche Werte enthält. Wenn der Wert, der in Ihrer Datenquelle 1 ist, beispielsweise für Folgendes verwenden `"1"` als die `softDeleteMarkerValue`.    

<a name="CreateDataSourceRequestExamples"></a>
**Beispiele für Textkörper die Anforderung**

Wenn Sie beabsichtigen, das die Datenquelle mit einem Indexer zu verwenden, die für einen Zeitplan ausgeführt wird, in diesem Beispiel wird veranschaulicht, wie ändern und Löschen von Erkennungsrichtlinien angeben: 

    { 
        "name" : "asqldatasource",
        "description" : "a description",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:....database.windows.net,1433;Database=...;User ID=...;Password=...;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "sometable" },
        "dataChangeDetectionPolicy" : { "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy", "highWaterMarkColumnName" : "RowVersion" }, 
        "dataDeletionDetectionPolicy" : { "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy", "softDeleteColumnName" : "IsDeleted", "softDeleteMarkerValue" : "true" }
    }

Auch wenn Sie nur die Datenquelle für einmalige Kopie der Daten verwenden möchten, können die Richtlinien angegeben werden:

    { 
        "name" : "asqldatasource",
        "description" : "anything you want, or nothing at all",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:....database.windows.net,1433;Database=...;User ID=...;Password=...;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "sometable" }
    } 

**Antwort**

Für eine erfolgreiche Anforderung: 201 erstellt. 

<a name="UpdateDataSource"></a>
## <a name="update-data-source"></a>Aktualisieren der Datenquelle ##

Sie können eine vorhandenen Datenquelle mithilfe einer Anforderung HTTP setzen aktualisieren. Sie geben Sie den Namen der Datenquelle klicken Sie auf die Anfrage-URI zu aktualisieren:

    PUT https://[service name].search.windows.net/datasources/[datasource name]?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

Die `api-version` ist erforderlich. Die aktuelle Version ist `2015-02-28`. [Suche Azure Versioning](https://msdn.microsoft.com/library/azure/dn864560.aspx) enthält Details und Weitere Informationen zu alternativen Versionen.

Die `api-key` muss ein Administrator Key (im Gegensatz zu einer Abfrage-Taste). Finden Sie im Abschnitt Authentifizierung in [Suchen Dienst REST-API](https://msdn.microsoft.com/library/azure/dn798935.aspx) Weitere Informationen zu Tastenkombinationen. [Erstellen eines Suchdiensts im Portal](search-create-service-portal.md) wird erläutert, wie die Dienst-URL und in der Anforderung verwendete Haupteigenschaften zu erhalten.

**Anfordern**

Die Syntax der Anfrage Textkörper entspricht der [Anfragen Datenquelle erstellen](#CreateDataSourceRequestSyntax).

> [AZURE.NOTE] Einige Eigenschaften können nicht auf einer vorhandenen Datenquelle aktualisiert werden. Beispielsweise können Sie den Typ des einer vorhandenen Datenquelle nicht ändern.  

> [AZURE.NOTE] Wenn Sie nicht die Verbindungszeichenfolge für eine vorhandene Datenquelle ändern möchten, können Sie angeben, dass das Literal `<unchanged>` für die Verbindungszeichenfolge. Dies ist insbesondere in Situationen, in dem Sie eine Daten aktualisieren müssen, Datenquelle, jedoch keine einfachen Zugriff auf die Verbindungszeichenfolge aus, da es sich um vertrauliche Daten handelt.

**Antwort**

Für eine erfolgreiche Anforderung: 201 erstellt, wenn Sie eine neue Datenquelle wurde erstellt wurden, und 204 kein Inhalt, wenn eine vorhandenen Datenquelle aktualisiert wurde.

<a name="ListDataSource"></a>
## <a name="list-data-sources"></a>Liste von Datenquellen ##

Der Vorgang **Listendatenquellen** gibt eine Liste der Datenquellen in Azure Suchdienst. 

    GET https://[service name].search.windows.net/datasources?api-version=[api-version]
    api-key: [admin key]

Die `api-version` ist erforderlich. Die aktuelle Version ist `2015-02-28`. 

Die `api-key` muss ein Administrator Key (im Gegensatz zu einer Abfrage-Taste). Finden Sie im Abschnitt Authentifizierung in [Suchen Dienst REST-API](https://msdn.microsoft.com/library/azure/dn798935.aspx) Weitere Informationen zu Tastenkombinationen. [Erstellen eines Suchdiensts im Portal](search-create-service-portal.md) wird erläutert, wie die Dienst-URL und in der Anforderung verwendete Haupteigenschaften zu erhalten.

**Antwort**

Für eine erfolgreiche Anforderung: 200 OK.

Hier ist ein Beispiel für Antwort Textkörper aus:

    {
      "value" : [
        {
          "name": "datasource1",
          "type": "azuresql",
          ... other data source properties
        }]
    }

Beachten Sie, dass Sie die Antwort auf nur die Eigenschaften filtern können, die Sie interessiert. Angenommen, wenn Sie nur eine Liste der Datenquellennamen möchten, verwenden Sie den OData `$select` Option Abfrage:

    GET /datasources?api-version=205-02-28&$select=name

In diesem Fall würde die Antwort aus dem oben genannten Beispiel wie folgt aussehen: 

    {
      "value" : [ { "name": "datasource1" }, ... ]
    }

Dies ist eine nützliche Technik Bandbreite gespeichert, wenn Sie viele Datenquellen Suchdienst haben.

<a name="GetDataSource"></a>
## <a name="get-data-source"></a>Abrufen der Datenquelle ##

Der **Erste Datenquelle** Vorgang ruft die DSN-Datei von Azure suchen.

    GET https://[service name].search.windows.net/datasources/[datasource name]?api-version=[api-version]
    api-key: [admin key]

Die `api-version` ist erforderlich. Die aktuelle Version ist `2015-02-28`. 

Die `api-key` muss ein Administrator Key (im Gegensatz zu einer Abfrage-Taste). Finden Sie im Abschnitt Authentifizierung in [Suchen Dienst REST-API](https://msdn.microsoft.com/library/azure/dn798935.aspx) Weitere Informationen zu Tastenkombinationen. [Erstellen eines Suchdiensts im Portal](search-create-service-portal.md) wird erläutert, wie die Dienst-URL und in der Anforderung verwendete Haupteigenschaften zu erhalten.

**Antwort**

: Status 200 ist OK für eine erfolgreiche Antwort ausgegeben.

Die Antwort ähnelt dem Beispiele in der [Datenquelle erstellen Beispiel Anfragen](#CreateDataSourceRequestExamples)zur Verfügung: 

    { 
        "name" : "asqldatasource",
        "description" : "a description",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:....database.windows.net,1433;Database=...;User ID=...;Password=...;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "sometable" },
        "dataChangeDetectionPolicy" : { 
            "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
            "highWaterMarkColumnName" : "RowVersion" }, 
        "dataDeletionDetectionPolicy" : { 
            "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
            "softDeleteColumnName" : "IsDeleted", 
            "softDeleteMarkerValue" : "true" }
    }

> [AZURE.NOTE] Legen Sie nicht die `Accept` Anforderung Kopfzeile zu `application/json;odata.metadata=none` beim Aufrufen dieser API als auf diese Weise bewirkt, dass `@odata.type` Attribut weggelassen werden, aus der Antwort, und Sie können nicht mehr zwischen Daten ändern und Daten Löschvorgang Erkennungsrichtlinien unterschiedlichen Typs zu unterscheiden. 

<a name="DeleteDataSource"></a>
## <a name="delete-data-source"></a>Löschen der Datenquelle ##

Der Vorgang **Datenquelle löschen** entfernt eine Datenquelle aus Azure Suchdienst.

    DELETE https://[service name].search.windows.net/datasources/[datasource name]?api-version=[api-version]
    api-key: [admin key]

> [AZURE.NOTE] Wenn alle Indexer der Datenquelle auf, die Sie löschen möchten, wird der Löschvorgang weiterhin fortgesetzt. Allerdings werden diese Indexer in einem Fehlerstatus in den nächsten Ausführen Übergabe.  

Die `api-version` ist erforderlich. Die aktuelle Version ist `2015-02-28`. 

Die `api-key` muss ein Administrator Key (im Gegensatz zu einer Abfrage-Taste). Finden Sie im Abschnitt Authentifizierung in [Suchen Dienst REST-API](https://msdn.microsoft.com/library/azure/dn798935.aspx) Weitere Informationen zu Tastenkombinationen. [Erstellen eines Suchdiensts im Portal](search-create-service-portal.md) wird erläutert, wie die Dienst-URL und in der Anforderung verwendete Haupteigenschaften zu erhalten.

**Antwort**

: Status 204 ist kein Inhalt für eine erfolgreiche Antwort ausgegeben.

<a name="CreateIndexer"></a>
## <a name="create-indexer"></a>Erstellen von Indexer ##

Sie können einen neuen Indexer innerhalb einer Azure Suchdienst mithilfe einer HTTP POST-Anforderung erstellen.
    
    POST https://[service name].search.windows.net/indexers?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

Alternativ können Sie sich verwenden, und geben Sie den Namen der Datenquelle auf den URI. Wenn die Datenquelle nicht vorhanden ist, wird er erstellt.

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]

> [AZURE.NOTE] Die maximale Anzahl von Indexern zulässige variieren je nach Ebene Preise. Kostenlose Dienst können bis zu 3 Indexer. Standard-Dienst kann 50 Indexer. Details finden Sie unter [Beschränkungen Service](search-limits-quotas-capacity.md) .

Die `api-version` ist erforderlich. Die aktuelle Version ist `2015-02-28`. 

Die `api-key` muss ein Administrator Key (im Gegensatz zu einer Abfrage-Taste). Finden Sie im Abschnitt Authentifizierung in [Suchen Dienst REST-API](https://msdn.microsoft.com/library/azure/dn798935.aspx) Weitere Informationen zu Tastenkombinationen. [Erstellen eines Suchdiensts im Portal](search-create-service-portal.md) wird erläutert, wie die Dienst-URL und in der Anforderung verwendete Haupteigenschaften zu erhalten.


<a name="CreateIndexerRequestSyntax"></a>
**Die Syntax der Textkörper anfordern**

Hauptteil der Anforderung enthält eine Definition Indexer, die der Datenquelle und den Zielindex für Indizierung, als auch optional Indizierung Zeitplan und Parameter angibt. 


Die Syntax für die Strukturierung der Anforderungsnutzlast lautet wie folgt aus. Eine Beispiel für eine Anforderung weiter unten in diesem Artikel bereitgestellt wird.

    { 
        "name" : "Required for POST, optional for PUT. The name of the indexer",
        "description" : "Optional. Anything you want, or null",
        "dataSourceName" : "Required. The name of an existing data source",
        "targetIndexName" : "Required. The name of an existing index",
        "schedule" : { Optional. See Indexing Schedule below. },
        "parameters" : { Optional. See Indexing Parameters below. },
        "fieldMappings" : { Optional. See Field Mappings below. },
        "disabled" : Optional boolean value indicating whether the indexer is disabled. False by default.  
    }

**Indexer Terminplan**

Indexer kann optional einen Zeitplan festlegen. Wenn ein Terminplan vorhanden ist, wird der Indexer regelmäßig gemäß Terminplan ausgeführt. Zeitplan weist die folgenden Attribute:

- `interval`: Erforderlich ist. Ein Wert für die Dauer, der angibt, ein Intervall oder den Zeitraum für Indexer ausgeführt wird. Das kleinste zulässige Intervall ist 5 Minuten. der längsten beträgt einen Tag. Es muss als "DayTimeDuration" XSD-Wert (einer Teilmenge eingeschränkten eines Werts [ISO 8601 Dauer](http://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) ) formatiert werden. Das Muster für Dies ist: `"P[nD][T[nH][nM]]"`. Beispiele: `PT15M` für alle 15 Minuten, `PT2H` für alle 2 Stunden. 

- `startTime`: Erforderlich ist. Eine UTC-Datetime, Indexer ausgeführt gestartet werden soll. 

**Indexerparameter**

Optional kann Indexer mehrere Parameter angeben, die ihr Verhalten beeinflussen. Alle Parameter sind optional.  

- `maxFailedItems`: Die Anzahl der Elemente, die nicht indiziert werden, bevor eine Indexer ausführen ein Fehlers gilt können. Standardwert ist 0. Informationen zu fehlerhaften Elemente ist des [Indizierungsstatus erste](#GetIndexerStatus) zurückgegeben. 

- `maxFailedItemsPerBatch`: Die Anzahl der Elemente, die in die einzelnen indiziert werden sollen, bevor eine Indexer ausführen ein Fehlers gilt ausgeführt werden kann. Standardwert ist 0.

- `base64EncodeKeys`: Gibt an, ob Dokument Tasten Base-64-codierte gehört. Azure Suche auferlegt Einschränkungen Zeichen, die in einem Dokumentschlüssel vorhanden sein können. Die Werte in den Quelldaten können jedoch Zeichen enthalten, die ungültig sind. Wenn es indizieren Werte wie Dokument Schlüssel erforderlich ist, können dieses Kennzeichen festgelegt werden true. Der Standardwert liegt `false`.

- `batchSize`: Gibt die Anzahl von Elementen, die aus der Datenquelle lesen und indiziert werden als einzelne Stapel an, um die Leistung zu verbessern. Der Standardwert hängt von den Datenquellentyp: Es ist 1000 für SQL Azure und DocumentDB und 10 für Azure BLOB-Speicher.

**Feld Zuordnungen**

Feld Zuordnungen können Sie einen anderen Feldnamen in der Zielliste Index einen Feldnamen in der Datenquelle zugeordnet. Angenommen, Sie verfügen über eine Quelltabelle mit einem Feld `_id`. Azure Suche gestattet keinen Feldnamen beginnen mit einem Unterstrich, damit das Feld umbenannt werden muss. Dies kann mithilfe der `fieldMappings` Eigenschaft des Indexers wie folgt: 
    
    "fieldMappings" : [ { "sourceFieldName" : "_id", "targetFieldName" : "id" } ] 

Sie können mehrere Feld Zuordnungen angeben: 

    "fieldMappings" : [ 
        { "sourceFieldName" : "_id", "targetFieldName" : "id" },
        { "sourceFieldName" : "_timestamp", "targetFieldName" : "timestamp" },
     ]

Sowohl Quell- und Zielwebsites Feldnamen Groß-und Kleinschreibung beachtet.

<a name="FieldMappingFunctions"></a>
***Feld Zuordnungsfunktionen***

Feld Zuordnungen können auch zum Umwandeln der Quelle Feldwerte von *Zuordnungsfunktionen*verwendet werden.

Nur eine solche Funktion wird derzeit unterstützt: `jsonArrayToStringCollection`. Analysiert ein Feld, das eine als JSON-Array in ein Collection(Edm.String)-Feld in der Zielliste Index formatierte Zeichenfolge enthält. Es ist vor allem für die Verwendung mit SQL Azure-Indexer vorgesehen, da SQL einen Datentyp einer systemeigenen Websitesammlung hat. Es kann wie folgt verwendet werden: 

    "fieldMappings" : [ { "sourceFieldName" : "tags", "mappingFunction" : { "name" : "jsonArrayToStringCollection" } } ] 

Beispielsweise, wenn das Quellfeld die Zeichenfolge enthält `["red", "white", "blue"]`, klicken Sie dann auf das Zielfeld vom Typ `Collection(Edm.String)` wird mit den drei Werten aufgefüllt `"red"`, `"white"` und `"blue"`.

Beachten Sie, dass die `targetFieldName` Eigenschaft ist optional; Wenn ausgelassen haben, die `sourceFieldName` Wert verwendet. 

<a name="CreateIndexerRequestExamples"></a>
**Beispiele für Textkörper die Anforderung**

Im folgenden Beispiel wird einen Indexer, der Daten aus der Tabelle optimiert kopiert die `ordersds` Datenquellen, um die `orders` Index nach einem Zeitplan, die am 1. Jan 2015 UTC startet und stündlich führt. Jede Indexer aufrufen werden Wenn nicht mehr als 5 Elemente in die einzelnen indiziert werden nicht erfolgreich, und nicht mehr als 10 Elemente nicht insgesamt indiziert werden. 

    {
        "name" : "myindexer",
        "description" : "a cool indexer",
        "dataSourceName" : "ordersds",
        "targetIndexName" : "orders",
        "schedule" : { "interval" : "PT1H", "startTime" : "2015-01-01T00:00:00Z" },
        "parameters" : { "maxFailedItems" : 10, "maxFailedItemsPerBatch" : 5, "base64EncodeKeys": false }
    }

**Antwort**

201 erstellt für eine erfolgreiche Anforderung.


<a name="UpdateIndexer"></a>
## <a name="update-indexer"></a>Indexer aktualisieren ##

Sie können einen vorhandenen Indexer mithilfe einer Anforderung HTTP setzen aktualisieren. Sie geben Sie den Namen des Indexers auf die Anfrage-URI zu aktualisieren:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

Die `api-version` ist erforderlich. Die aktuelle Version ist `2015-02-28`. 

Die `api-key` muss ein Administrator Key (im Gegensatz zu einer Abfrage-Taste). Finden Sie im Abschnitt Authentifizierung in [Suchen Dienst REST-API](https://msdn.microsoft.com/library/azure/dn798935.aspx) Weitere Informationen zu Tastenkombinationen. [Erstellen eines Suchdiensts im Portal](search-create-service-portal.md) wird erläutert, wie die Dienst-URL und in der Anforderung verwendete Haupteigenschaften zu erhalten.

**Anfordern**

Die Syntax der Anfrage Textkörper entspricht der [Anfragen Indexer erstellen](#CreateIndexerRequestSyntax).

**Antwort**

Für eine erfolgreiche Anforderung: 201 erstellt, wenn ein neuer Indexer wurde erstellt wurden, und 204 keine Inhalte an, wenn ein vorhandener Indexer aktualisiert wurde.


<a name="ListIndexers"></a>
## <a name="list-indexers"></a>Liste Indexern ##

Der Vorgang **Liste Indexer** gibt die Liste der Indexer in Azure Suchdienst. 

    GET https://[service name].search.windows.net/indexers?api-version=[api-version]
    api-key: [admin key]


Die `api-version` ist erforderlich. Die Preview-Version ist `2015-02-28-Preview`. [Suche Azure Versioning](https://msdn.microsoft.com/library/azure/dn864560.aspx) enthält Details und Weitere Informationen zu alternativen Versionen.

Die `api-key` muss ein Administrator Key (im Gegensatz zu einer Abfrage-Taste). Finden Sie im Abschnitt Authentifizierung in [Suchen Dienst REST-API](https://msdn.microsoft.com/library/azure/dn798935.aspx) Weitere Informationen zu Tastenkombinationen. [Erstellen eines Suchdiensts im Portal](search-create-service-portal.md) wird erläutert, wie die Dienst-URL und in der Anforderung verwendete Haupteigenschaften zu erhalten.

**Antwort**

Für eine erfolgreiche Anforderung: 200 OK.

Hier ist ein Beispiel für Antwort Textkörper aus:

    {
      "value" : [
      {
        "name" : "myindexer",
        "description" : "a cool indexer",
        "dataSourceName" : "ordersds",
        "targetIndexName" : "orders",
        ... other indexer properties
      }]
    }

Beachten Sie, dass Sie die Antwort auf nur die Eigenschaften filtern können, die Sie interessiert. Beispielsweise wenn Sie nur eine Liste der Namen Indexer möchten, verwenden Sie den OData `$select` Option Abfrage:

    GET /indexers?api-version=2014-10-20-Preview&$select=name

In diesem Fall würde die Antwort aus dem oben genannten Beispiel wie folgt aussehen: 

    {
      "value" : [ { "name": "myindexer" } ]
    }

Dies ist eine nützliche Technik, um Bandbreite zu sparen, wenn Sie viele Indexer Suchdienst haben.


<a name="GetIndexer"></a>
## <a name="get-indexer"></a>Abrufen von Indexer ##

Der **Erste** Indizierungsvorgang Ruft die Indexer Definition von Azure suchen.

    GET https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]
    api-key: [admin key]

Die `api-version` ist erforderlich. Die Preview-Version ist `2015-02-28-Preview`. 

Die `api-key` muss ein Administrator Key (im Gegensatz zu einer Abfrage-Taste). Finden Sie im Abschnitt Authentifizierung in [Suchen Dienst REST-API](https://msdn.microsoft.com/library/azure/dn798935.aspx) Weitere Informationen zu Tastenkombinationen. [Erstellen eines Suchdiensts im Portal](search-create-service-portal.md) wird erläutert, wie die Dienst-URL und in der Anforderung verwendete Haupteigenschaften zu erhalten.

**Antwort**

: Status 200 ist OK für eine erfolgreiche Antwort ausgegeben.

Die Antwort ähnelt dem [Erstellen Indexer Beispiel Anfragen](#CreateIndexerRequestExamples)Beispiele zur Verfügung: 

    {
        "name" : "myindexer",
        "description" : "a cool indexer",
        "dataSourceName" : "ordersds",
        "targetIndexName" : "orders",
        "schedule" : { "interval" : "PT1H", "startTime" : "2015-01-01T00:00:00Z" },
        "parameters" : { "maxFailedItems" : 10, "maxFailedItemsPerBatch" : 5, "base64EncodeKeys": false }
    }


<a name="DeleteIndexer"></a>
## <a name="delete-indexer"></a>Indexer löschen ##

Der **Löschen** Indizierungsvorgang entfernt ein Indexers Azure Suchdienst.

    DELETE https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]
    api-key: [admin key]

Wenn ein Indexer gelöscht wird, werden die Indexer Ausführungen in Bearbeitung zu diesem Zeitpunkt bis zum Abschluss ausgeführt, aber werden keine weiteren Ausführungen geplant werden. Versucht, einen nicht vorhandene Indexer verwenden, führt zu Fehlern im HTTP-Statuscode 404 nicht gefunden. 
 
Die `api-version` ist erforderlich. Die Preview-Version ist `2015-02-28-Preview`. 

Die `api-key` muss ein Administrator Key (im Gegensatz zu einer Abfrage-Taste). Finden Sie im Abschnitt Authentifizierung in [Suchen Dienst REST-API](https://msdn.microsoft.com/library/azure/dn798935.aspx) Weitere Informationen zu Tastenkombinationen. [Erstellen eines Suchdiensts im Portal](search-create-service-portal.md) wird erläutert, wie die Dienst-URL und in der Anforderung verwendete Haupteigenschaften zu erhalten.

**Antwort**

: Status 204 ist kein Inhalt für eine erfolgreiche Antwort ausgegeben.

<a name="RunIndexer"></a>
## <a name="run-indexer"></a>Indexer ausführen ##

Zusätzlich zur Ausführung von regelmäßig nach einem Zeitplan, kann ein Indexer auch bei Bedarf über den **Ausführen** Indizierungsvorgang aufgerufen werden: 

    POST https://[service name].search.windows.net/indexers/[indexer name]/run?api-version=[api-version]
    api-key: [admin key]

Die `api-version` ist erforderlich. Die Preview-Version ist `2015-02-28-Preview`. 

Die `api-key` muss ein Administrator Key (im Gegensatz zu einer Abfrage-Taste). Finden Sie im Abschnitt Authentifizierung in [Suchen Dienst REST-API](https://msdn.microsoft.com/library/azure/dn798935.aspx) Weitere Informationen zu Tastenkombinationen. [Erstellen eines Suchdiensts im Portal](search-create-service-portal.md) wird erläutert, wie die Dienst-URL und in der Anforderung verwendete Haupteigenschaften zu erhalten.

**Antwort**

Statuscode: 202 akzeptiert für eine erfolgreiche Antwort zurückgegeben.

<a name="GetIndexerStatus"></a>
## <a name="get-indexer-status"></a>Abrufen von Indizierungsstatus ##

Der **Erste Indizierungsstatus** Vorgang ruft den aktuellen Status und Ausführung Verlauf eines Indexers: 

    GET https://[service name].search.windows.net/indexers/[indexer name]/status?api-version=[api-version]
    api-key: [admin key]


Die `api-version` ist erforderlich. Die Preview-Version ist `2015-02-28-Preview`. 

Die `api-key` muss ein Administrator Key (im Gegensatz zu einer Abfrage-Taste). Finden Sie im Abschnitt Authentifizierung in [Suchen Dienst REST-API](https://msdn.microsoft.com/library/azure/dn798935.aspx) Weitere Informationen zu Tastenkombinationen. [Erstellen eines Suchdiensts im Portal](search-create-service-portal.md) wird erläutert, wie die Dienst-URL und in der Anforderung verwendete Haupteigenschaften zu erhalten.

**Antwort**

Statuscode: 200 OK für eine erfolgreiche Antwort.

Der Nachrichtentext der Antwort enthält Informationen für insgesamt Indizierungsstatus Gesundheit, der letzten Indexer aufrufen sowie den Verlauf der zuletzt verwendete Indexer Aufrufe (falls vorhanden). 

Ein Beispiel für Antwort Textkörper sieht wie folgt aus: 

    {
        "status":"running",
        "lastResult": {
            "status":"success",
            "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
         },
        "executionHistory":[ {
            "status":"success",
            "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
        }]
    }

**Indizierungsstatus**

Indizierungsstatus kann eine der folgenden Werte sein:

- `running`Gibt an, dass der Indexer normal ausgeführt wird. Beachten Sie, dass einige der Indexer Ausführungen noch fehlerhafte, daher es eine gute Idee ist, den `lastResult` als auch die Eigenschaft. 

- `error`Gibt an, dass der Indexer ein Fehler aufgetreten, der ohne personenbezogenen Eingriff korrigiert werden kann. Beispielsweise können die Datenquellen-Anmeldeinformationen abgelaufen oder das Schema der Datenquelle oder des Indexes Ziel geändert in einer abgeschnitten werden Weise. 

**Ergebnis der Ausführung des Indexer**

Ein Ergebnis der Ausführung des Indexer enthält Informationen zur Ausführung eines einzelnen Indexer. Das neueste Ergebnis wird angegeben, wie die `lastResult` den Indizierungsstatus-Eigenschaft. Andere zuletzt verwendete, sofern vorhanden, werden Ergebnisse zurückgegeben als die `executionHistory` den Indizierungsstatus-Eigenschaft. 

Ergebnis der Ausführung des Indexer enthält die folgenden Eigenschaften: 

- `status`: der Status einer Ausführung. Details finden Sie unter [Datenausführungsverhinderung Indizierungsstatus](#IndexerExecutionStatus) unter. 

- `errorMessage`: Fehlermeldung bei einer fehlgeschlagener Ausführung. 

- `startTime`: die entsprechende koordinierte Weltzeit, wenn dieser Ausführung gestartet.

- `endTime`: die entsprechende koordinierte Weltzeit, wenn diese Ausführung beendet wurde. Dieser Wert wird nicht festgelegt, wenn die Ausführung noch nicht abgeschlossen ist.

- `errors`: ein Array von Elementebene Fehlern, sofern vorhanden. Jeder Eintrag enthält einen Dokumentschlüssel (`key` Eigenschaft) und eine Fehlermeldung (`errorMessage` Eigenschaft). 

- `itemsProcessed`: die Anzahl der Datenquellen-Elemente (z. B. Tabellenzeilen), bei dem der Indexer versuchten, während dieser Ausführung indizieren. 

- `itemsFailed`: die Anzahl von Elementen, die während dieser Ausführung fehlgeschlagen ist.  
 
- `initialTrackingState`: immer `null` für die erste Ausführung Indexer, oder wenn die Daten Verlauf Richtlinie ändern nicht auf die Datenquelle verwendet aktiviert ist. Wenn eine solche Richtlinie aktiviert ist, gibt dieser Wert in nachfolgenden Ausführungen die erste (niedrigste) Änderung dieser Ausführung verarbeiteten Wert nachverfolgen. 

- `finalTrackingState`: immer `null` , wenn die Daten Verlauf Richtlinie ändern nicht auf die Datenquelle verwendet aktiviert ist. Andernfalls gibt die neuesten (höchste Priorität) änderungsnachverfolgung erfolgreich verarbeiteten dieser Ausführung Wert ein. 

<a name="IndexerExecutionStatus"></a>
**Ausführung des Indizierungsstatus**

Ausführung des Indizierungsstatus erfasst den Status der Ausführung eines einzelnen Indexer. Sie können die folgenden Werte haben:

- `success`Gibt an, dass die Ausführung Indexer erfolgreich abgeschlossen wurde.

- `inProgress`Gibt an, dass die Ausführung Indexer ausgeführt wird. 

- `transientFailure`Gibt an, dass eine Indexer Ausführung fehlgeschlagen ist. Finden Sie unter `errorMessage` zu schaffen. Der Fehler möglicherweise oder personenbezogenen Eingriff zu beheben ist möglicherweise nicht erforderlich – beispielsweise eine Schemainkompatibilität zwischen der Datenquelle und den Zielindex beheben Aktion des Benutzers, erfordert, eine temporäre Datenquelle Ausfallzeiten hingegen nicht. Indexer Aufrufe weiterhin pro Zeitplan, sofern vorhanden ist. 

- `persistentFailure`Gibt an, dass der Indexer auf eine Weise fehlgeschlagen ist, die personenbezogenen Eingriff erforderlich sind. Geplante Indexer Ausführungen beenden. Nach dem Adressieren des Problems, verwenden Sie zurücksetzen Indexer-API, um die geplanten Ausführungen neu zu starten. 

- `reset`Gibt an, dass der Indexer durch einen Anruf an zurücksetzen Indexer API (siehe unten) zurückgesetzt wurde. 

<a name="ResetIndexer"></a>
## <a name="reset-indexer"></a>Indexer zurücksetzen ##

Der **Zurücksetzen** Indizierungsvorgang setzt die änderungsnachverfolgung Indexer zugeordnete Zustand zurück. So können Sie zum Auslösen von Grund erneute Indizierung, (beispielsweise, wenn Ihre Datenquellenschema geändert wurde) oder ändern die Daten ändern Erkennung Richtlinie für eine Datenquelle mit dem Indexer verknüpft ist.   

    POST https://[service name].search.windows.net/indexers/[indexer name]/reset?api-version=[api-version]
    api-key: [admin key]

Die `api-version` ist erforderlich. Die Preview-Version ist `2015-02-28-Preview`. 

Die `api-key` muss ein Administrator Key (im Gegensatz zu einer Abfrage-Taste). Finden Sie im Abschnitt Authentifizierung in [Suchen Dienst REST-API](https://msdn.microsoft.com/library/azure/dn798935.aspx) Weitere Informationen zu Tastenkombinationen. [Erstellen eines Suchdiensts im Portal](search-create-service-portal.md) wird erläutert, wie die Dienst-URL und in der Anforderung verwendete Haupteigenschaften zu erhalten.

**Antwort**

Statuscode: 204 kein Inhalt für eine erfolgreiche Antwort.

## <a name="mapping-between-sql-data-types-and-azure-search-data-types"></a>Zuordnung zwischen SQL-Datentypen und Datentypen Azure suchen ##

<table style="font-size:12">
<tr>
<td>SQL-Datentyp</td>  
<td>Zulässige Zielindex Feldtypen</td>
<td>Notizen</td>
</tr>
<tr>
<td>Bit</td>
<td>Edm.Boolean, Edm.String</td>
<td></td>
</tr>
<tr>
<td>Int, Smallint, tinyint</td>
<td>Edm.Int32, Int64, Edm.String</td>
<td></td>
</tr>
<tr>
<td>bigint</td>
<td>Int64, Edm.String</td>
<td></td>
</tr>
<tr>
<td>Real, Pufferzeiten</td>
<td>Edm.Double, Edm.String</td>
<td></td>
</tr>
<tr>
<td>Smallmoney, Geld<br>Dezimalzahl<br>numerische
</td>
<td>Edm.String</td>
<td>Azure-Suche unterstützt keine Edm.Double decimal Typen umwandeln, da dies Genauigkeit verlieren möchten
</td>
</tr>
<tr>
<td>Zeichen, Nchar, Varchar, nvarchar</td>
<td>Edm.String<br/>Collection(EDM.String)</td>
<td>Finden Sie unter [Feld Zuordnen von Funktionen](#FieldMappingFunctions) in diesem Dokument Details zum Transformieren einer Zeichenfolge Spalteninhalts in einer Collection(Edm.String)</td>
</tr>
<tr>
<td>Smalldatetime "," Datetime "," datetime2 "," Datum "," datetimeoffset</td>
<td>Edm.DateTimeOffset, Edm.String</td>
<td></td>
</tr>
<tr>
<td>uniqueidentifier</td>
<td>Edm.String</td>
<td></td>
</tr>
<tr>
<td>"geography"</td>
<td>Edm.GeographyPoint</td>
<td>Nur Geography Instanzen vom Typ Punkt mit SRID 4326 (der Standard) werden unterstützt.</td>
</tr>
<tr>
<td>RowVersion</td>
<td>N/V</td>
<td>Zeile-Version Spalten können nicht in den Suchindex gespeichert werden, aber sie können für änderungsnachverfolgung verwendet werden</td>
</tr>
<tr>
<td>Zeit timespan<br>Binary, Varbinary, Bild,<br>XML, Anordnung CLR-Datentypen</td>
<td>N/V</td>
<td>Nicht unterstützt</td>
</tr>
</table>

## <a name="mapping-between-json-data-types-and-azure-search-data-types"></a>Zuordnung zwischen JSON-Datentypen und Datentypen Azure suchen ##

<table style="font-size:12">
<tr>
<td>JSON-Datentyp</td> 
<td>Zulässige Zielindex Feldtypen</td>
<td>Notizen</td>
</tr>
<tr>
<td>bool</td>
<td>Edm.Boolean, Edm.String</td>
<td></td>
</tr>
<tr>
<td>Ganzen Zahlen</td>
<td>Edm.Int32, Int64, Edm.String</td>
<td></td>
</tr>
<tr>
<td>Gleitkommazahl Zahlen</td>
<td>Edm.Double, Edm.String</td>
<td></td>
</tr>
<tr>
<td>Zeichenfolge</td>
<td>Edm.String</td>
<td></td>
</tr>
<tr>
<td>Arrays von Element eingibt, z. B. ["a", "b", "C"]</td>
<td>Collection(EDM.String)</td>
<td></td>
</tr>
<tr>
<td>Zeichenfolgen, die Datumsangaben aussehen</td>
<td>Edm.DateTimeOffset, Edm.String</td>
<td></td>
</tr>
<tr>
<td>GeoJSON Point-Objekten</td>
<td>Edm.GeographyPoint</td>
<td>GeoJSON Punkte sind JSON-Objekten in folgendem Format: {"Typ": "Punkt", "Koordinaten": [lange, Lat]} </td>
</tr>
<tr>
<td>Andere JSON-Objekte</td>
<td>N/V</td>
<td>Nicht unterstützt. Azure-Suche unterstützt derzeit nur Primitivtypen und Zeichenfolge Websitesammlungen</td>
</tr>
</table>
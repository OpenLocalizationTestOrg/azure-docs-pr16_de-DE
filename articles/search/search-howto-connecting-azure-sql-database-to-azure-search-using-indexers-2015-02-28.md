<properties 
    pageTitle="SQL Azure-Datenbank für das Herstellen von Verbindungen mit Azure verwenden von Indexern suchen | Microsoft Azure | Indexer" 
    description="Erfahren Sie, wie Daten aus Azure SQL-Datenbank, die eine Azure Suchindex Verwenden von Indexern abrufen." 
    services="search" 
    documentationCenter="" 
    authors="chaosrealm" 
    manager="pablocas" 
    editor=""/>

<tags 
    ms.service="search" 
    ms.devlang="rest-api" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="05/26/2016" 
    ms.author="eugenesh"/>

#<a name="connecting-azure-sql-database-to-azure-search-using-indexers"></a>Herstellen einer Verbindung Azure SQL-Datenbank zum Verwenden von Indexern Azure-Suche

Azure Suchdienst ist ein Cloud gehosteten Suchdienst, der eine hervorragende Suchfunktion bereitstellen erleichtert. Bevor Sie die Suche starten können, müssen Sie eine Azure Suchindex für Ihre Daten zu füllen. Wenn die Daten in einer SQL Azure-Datenbank verfügbar ist, kann die neue **Azure Suche Indexersteller für Azure SQL-Datenbank** (oder **SQL Azure-Indexer** für kurz) in Azure Suchen der Indizierung zu automatisieren. Dies bedeutet, dass Sie weniger Code schreiben und weniger Infrastruktur kümmern.

Indexer funktionieren zurzeit, nur mit Azure SQL-Datenbank, SQL Server auf Azure-virtuellen Computern und [Azure DocumentDB](../documentdb/documentdb-search-indexer.md). In diesem Artikel konzentrieren wir uns auf Indexer, für die Arbeit mit Azure SQL-Datenbank. Wenn Sie, finden Sie unter möchten Unterstützung für Weitere Datenquellen, geben Sie Ihr Feedback über das [Feedback-Forum Azure suchen](https://feedback.azure.com/forums/263029-azure-search/).

In diesem Artikel wird die Funktionsweise der Verwenden von Indexern behandelt, aber wir werden auch eines Drilldowns in Funktionen und-Verhalten, die nur mit SQL-Datenbanken (z. B. integrierte änderungsnachverfolgung) verfügbar sind.

## <a name="indexers-and-data-sources"></a>Indexer und Datenquellen

Zum Einrichten und Konfigurieren eines SQL Azure-Indexers, können Sie die [Azure Suche REST-API](http://go.microsoft.com/fwlink/p/?LinkID=528173) zum Erstellen und Verwalten von **Indexer** und **Datenquellen**aufrufen. 

Können Sie auch der [Indexer Klasse](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.indexer.aspx) [.NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx)oder des Assistenten zum Importieren von der [Azure-Portal](https://portal.azure.com) zu erstellen und planen ein Indexers.

Eine **Datenquelle** gibt an, welche Daten Sie indizieren, Anmeldeinformationen für den Zugriff auf die Daten und Richtlinien, mit denen Azure-Suche auf effizient identifizieren von Änderungen in den Daten (neu, geändert oder gelöscht Zeilen) auf. Es wird als eine unabhängige Ressource definiert, damit es von mehrere Indexer verwendet werden kann.

Ein **Indexer** ist eine Ressource, die Datenquellen mit Ziel Suchindizes eine Verbindung herstellt. Ein Indexer wird auf folgende Weise verwendet:
 
- Führen Sie eine einmalige Kopie der Daten ein Indexes gefüllt wird.
- Aktualisieren eines Indexes mit Änderungen in der Datenquelle nach einem Zeitplan.
- Führen Sie bei Bedarf aktualisieren ein Indexes, je nach Bedarf an. 

## <a name="when-to-use-azure-sql-indexer"></a>Verwenden von SQL Azure-Indexer

Je nach verschiedene Faktoren für die Daten die Verwendung von SQL Azure-Indexer oder möglicherweise nicht geeignet. Wenn Ihre Daten die folgenden Anforderungen entspricht, können Sie SQL Azure-Indexer verwenden: 

- Alle Daten aus einer einzelnen Tabelle oder die Ansicht enthält
    - Wenn die Daten über mehrere Tabellen verteilt ist, können Sie eine Ansicht erstellen und verwenden diese Ansicht mit der Indexer. Bedenken Sie jedoch, wenn Sie eine Ansicht verwenden, SQL Server integriert ändern Erkennung verwenden können Sie nicht. In diesem Abschnitt Weitere Informationen hierzu finden Sie unter. 
- Die Datentypen, die in der Datenquelle verwendet werden vom Indexer unterstützt. Die meisten, aber nicht alle Typen werden von den SQL-Code unterstützt. Weitere Informationen finden Sie unter [Zuordnen von Datentypen in Azure suchen](http://go.microsoft.com/fwlink/p/?LinkID=528105). 
- Sie nicht in Echtzeit Updates an den Index, wenn eine Zeile Änderungen in der Nähe müssen. 
    - Indexer kann die Tabelle höchstens 5 Minuten neu indizieren. Wenn Ihre Daten häufig ändern und die Änderungen im Index in Sekunden oder einzelne Minuten angezeigt werden müssen, empfehlen wir [Azure Suche Index API](https://msdn.microsoft.com/library/azure/dn798930.aspx) direkt verwenden. 
- Wenn Sie eine große Datenmenge haben und planen, Ausführen des Indexers termingerecht ist Ihr Schema uns effizient identifizieren können geändert (und gelöscht, falls zutreffend) Zeilen. Weitere Informationen hierzu finden Sie unter "Erfassen geändert und gelöschte Zeilen" unten. 
- Die Größe des indizierten Feldern in einer Zeile nicht die maximale Größe einer Azure-Suche Indizierung Besprechungsanfrage, also 16 MB nicht überschreiten. 

## <a name="create-and-use-an-azure-sql-indexer"></a>Erstellen und Verwenden eines SQL Azure-Indexers

Erstellen Sie zuerst die Datenquelle aus: 

    POST https://myservice.search.windows.net/datasources?api-version=2015-02-28 
    Content-Type: application/json
    api-key: admin-key
    
    { 
        "name" : "myazuresqldatasource",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:<your server>.database.windows.net,1433;Database=<your database>;User ID=<your user name>;Password=<your password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "name of the table or view that you want to index" }
    }


Sie können die Verbindungszeichenfolge aus dem [Klassischen Azure-Portal](https://portal.azure.com)abrufen; Verwenden Sie die `ADO.NET connection string` Option.

Erstellen Sie dann Ziel Azure Suchindex, wenn Sie eine bereits besitzen. Sie können aus dem [Portal Benutzeroberfläche](https://portal.azure.com) oder mit der [Index-API erstellen](https://msdn.microsoft.com/library/azure/dn798941.aspx)ausführen.  Stellen Sie sicher, dass das Schema der der Zielindex für das Schema der Quelltabelle geeignet ist: [Zuordnung zwischen Datentypen für SQL und Azure-Suche](#TypeMapping) Details finden Sie unter.

Erstellen Sie schließlich Indexer, indem Sie ihm einen Namen und auf den Daten Quell- und Zielwebsites Index verweisen:

    POST https://myservice.search.windows.net/indexers?api-version=2015-02-28 
    Content-Type: application/json
    api-key: admin-key
    
    { 
        "name" : "myindexer",
        "dataSourceName" : "myazuresqldatasource",
        "targetIndexName" : "target index name"
    }

Auf diese Weise erstellten Indexer verfügt nicht über einen Zeitplan. Es wird automatisch ausgeführt, sobald, sobald sie erstellt wurde. Sie können sie jederzeit wieder verwenden eine Anforderung **Ausführen Indexer** ausführen:

    POST https://myservice.search.windows.net/indexers/myindexer/run?api-version=2015-02-28 
    api-key: admin-key

Sie können verschiedene Aspekte des Indexerverhalten, z. B. Stapelgröße und wie viele Dokumente übersprungen werden können, bevor eine Indexer Fehler bei der Ausführung werden wird anpassen. Weitere Informationen hierzu finden Sie unter [Indexer-API erstellen](https://msdn.microsoft.com/library/azure/dn946899.aspx).
 
Sie müssen möglicherweise Azure Services für die Verbindung zu Ihrer Datenbank zulassen. Anweisungen zum erledigen finden Sie unter [Herstellen einer Verbindung von Azure](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) .

Verwenden Sie zum Überwachen der Indizierungsstatus und Ausführung Verlauf (Anzahl von Elementen indiziert, Fehler usw.), eine Anforderung **Indizierungsstatus** : 

    GET https://myservice.search.windows.net/indexers/myindexer/status?api-version=2015-02-28 
    api-key: admin-key

Die Antwort sollte etwa wie folgt aussehen: 

    {
        "@odata.context":"https://myservice.search.windows.net/$metadata#Microsoft.Azure.Search.V2015_02_28.IndexerExecutionInfo",
        "status":"running",
        "lastResult": {
            "status":"success",
            "errorMessage":null,
            "startTime":"2015-02-21T00:23:24.957Z",
            "endTime":"2015-02-21T00:36:47.752Z",
            "errors":[],
            "itemsProcessed":1599501,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null 
        },
        "executionHistory":
        [
            {
                "status":"success",
                "errorMessage":null,
                "startTime":"2015-02-21T00:23:24.957Z",
                "endTime":"2015-02-21T00:36:47.752Z",
                "errors":[],
                "itemsProcessed":1599501,
                "itemsFailed":0,
                "initialTrackingState":null,
                "finalTrackingState":null 
            },
            ... earlier history items 
        ]
    }

Ausführungsverlauf enthält bis zu 50 der zuletzt abgeschlossenen Ausführungen, die in umgekehrter Reihenfolge chronologische sortiert werden (so, dass die neueste Ausführung in der Antwort zuerst eintritt). Weitere Informationen zur Antwort finden Sie im [Indizierungsstatus abrufen](http://go.microsoft.com/fwlink/p/?LinkId=528198)

## <a name="run-indexers-on-a-schedule"></a>Führen Sie eine Indexer für einen Zeitplan

Sie können auch den Indexer regelmäßig nach einem Zeitplan ausführen anordnen. Fügen Sie hierzu die Eigenschaft **Terminplan** beim Erstellen oder aktualisieren den Indexer nur hinzu. Das folgende Beispiel zeigt die Anforderung einer sich Indexer zu aktualisieren:

    PUT https://myservice.search.windows.net/indexers/myindexer?api-version=2015-02-28 
    Content-Type: application/json
    api-key: admin-key 

    { 
        "dataSourceName" : "myazuresqldatasource",
        "targetIndexName" : "target index name",
        "schedule" : { "interval" : "PT10M", "startTime" : "2015-01-01T00:00:00Z" }
    }

Der Parameter **Intervall** ist erforderlich. Das Intervall bezieht sich auf den Zeitraum zwischen dem Start der zwei aufeinanderfolgende Indexer Ausführungen. Das kleinste zulässige Intervall ist 5 Minuten. der längsten beträgt einen Tag. Es muss als "DayTimeDuration" XSD-Wert (einer Teilmenge eingeschränkten eines Werts [ISO 8601 Dauer](http://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) ) formatiert werden. Das Muster für Dies ist: `P(nD)(T(nH)(nM))`. Beispiele: `PT15M` für alle 15 Minuten, `PT2H` für alle 2 Stunden.

Die optional **Startzeit** weist darauf hin, wenn die geplanten Ausführungen begonnen werden soll. Wenn sie ausgelassen wird, wird die aktuelle UTC-Zeit verwendet werden. Diesmal kann in der Vergangenheit – werden in der Fall die erste Ausführung geplant werden, als wäre der Indexer kontinuierlich seit der Startzeit ausgeführt wurde.  

Nur eine Ausführung einer angegebenen Indexers kann nacheinander ausgeführt werden. Wenn ein Indexer bereits ausgeführt wird, wenn es sich bei dem nächsten Vorkommen beginnen soll, die Ausführung übersprungen und Lebensläufe beim nächsten Intervall, keine anderen Indexer Aufträge Voraussetzung ausgeführt wird.

Sehen Sie sich ein Beispiel für diese mehr konkret eingesetzt. Nehmen Sie an wir die folgenden stündlich fest, die so konfiguriert ist: 

    "schedule" : { "interval" : "PT1H", "startTime" : "2015-03-01T00:00:00Z" }

Hier geschieht Folgendes: 

1. Die erste Indexer Ausführung beginnt am oder um 1. März 2015 12:00:00 UTC.
1. Wird davon ausgegangen Sie, dass diese Ausführung dauert, 20 Minuten (oder einem beliebigen Zeitpunkt weniger als 1 Stunde).
1. Die zweite Ausführung beginnt am oder um 1. März 2015 1:00 Uhr 
1. Nehmen Sie jetzt an, dass diese Ausführung mehr als eine Stunde dauert (hierzu muss eine große Anzahl von Dokumenten für diese Option, um tatsächlich auftreten, aber es ist eine nützliche Abbildung) – beispielsweise 70 Minuten –, damit es ungefähr 2:10 Uhr abgeschlossen ist
1. Es ist jetzt 2:00 Uhr Zeit für die Ausführung des dritten starten. Jedoch, da die zweite Ausführung von 1: 00 Uhr immer noch ausgeführt wird, wird die dritte Ausführung übersprungen. Die dritte Ausführung beginnt mit 3: 00 Uhr

Sie können hinzufügen, ändern Sie oder löschen Sie einen Zeitplan für einen vorhandenen Indexer mithilfe einer Anforderung **Indexer setzen** . 

## <a name="capturing-new-changed-and-deleted-rows"></a>Erfassen neu, geändert und gelöschten Zeilen

Wenn Sie einen Zeitplan verwenden und die Tabelle eine wichtige Anzahl von Zeilen enthält, sollten Sie eine Erkennung-Richtlinie Daten ändern, verwenden, damit der Indexer nur die neuen oder geänderten Zeilen aktualisiert werden kann, ohne die gesamte Tabelle erneute Indizierung.

### <a name="sql-integrated-change-tracking-policy"></a>SQL-integriert Änderungsnachverfolgung Richtlinie

Wenn die SQL-Datenbank [Ändern Verlauf](https://msdn.microsoft.com/library/bb933875.aspx)unterstützt, wird empfohlen, **SQL integrierte ändern nachverfolgen Richtlinie**verwenden. Diese Richtlinie ermöglicht effizientesten änderungsnachverfolgung und es auch Azure-Suche auf gelöschte Zeilen zu identifizieren, ohne dass Sie eine explizite "Weiche" Löschspalte Ihrer Tabelle hinzuzufügen.

Integrierte änderungsnachverfolgung wird unterstützt, beginnend mit den folgenden Versionen von SQL Server-Datenbank:
 
- SQL Server 2008 R2 und höher, wenn Sie SQL Server auf Azure-virtuellen Computern verwenden. 
- Azure SQL-Datenbank V12, wenn Sie Azure SQL-Datenbank verwenden.

Wenn SQL integriert änderungsnachverfolgung Richtlinie, mit eine separaten Daten Löschvorgang Erkennung Richtlinie nicht angeben – dieser Richtlinie verfügt über eine integrierte Unterstützung zum Identifizieren von gelöschten Zeilen.

Dieser Richtlinie kann nur mit Tabellen verwendet werden. Sie können mit Sichten verwendet werden. Sie müssen Aktivieren der änderungsnachverfolgung für die Tabelle, die Sie verwenden, bevor Sie diese Richtlinie verwenden können. Anweisungen finden Sie unter [Aktivieren und Deaktivieren von Verlauf ändern](https://msdn.microsoft.com/library/bb964713.aspx) . 

So verwenden Sie diese Richtlinie, erstellen oder Aktualisieren der Datenquelle wie folgt:
 
    { 
        "name" : "myazuresqldatasource",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "connection string" },
        "container" : { "name" : "table or view name" }, 
        "dataChangeDetectionPolicy" : {
           "@odata.type" : "#Microsoft.Azure.Search.SqlIntegratedChangeTrackingPolicy" 
      }
    }

### <a name="high-water-mark-change-detection-policy"></a>Hochwassermarken ändern Erkennung Richtlinie

Während die Richtlinie SQL integrierte nachverfolgung empfohlen wird, können Sie nicht mehr zur gemeinsamen Nutzung von Daten in einer Ansicht ist oder wenn Sie eine ältere Version von SQL Azure-Datenbank verwenden. Erwägen Sie in diesem Fall die Richtlinie Hochwassermarken. Dieser Richtlinie kann verwendet werden, wenn die Tabelle eine Spalte enthält, die die folgenden Kriterien erfüllt:

- Fügt alle Geben Sie einen Wert für die Spalte. 
- Alle Updates zu einem Element ändern Sie auch den Wert der Spalte. 
- Der Wert in dieser Spalte wird bei jeder Änderung erhöht.
- Abfragen, in denen eine `WHERE` Klausel wie `WHERE [High Water Mark Column] > [Current High Water Mark Value]` effizient ausgeführt werden kann.

Eine Spalte indizierten **Rowversion** beträgt beispielsweise der Spalte Hochwassermarken gut geeignet. So verwenden Sie diese Richtlinie, erstellen oder Aktualisieren der Datenquelle wie folgt: 

    { 
        "name" : "myazuresqldatasource",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "connection string" },
        "container" : { "name" : "table or view name" }, 
        "dataChangeDetectionPolicy" : {
           "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
           "highWaterMarkColumnName" : "[a row version or last_updated column name]" 
      }
    }

### <a name="soft-delete-column-deletion-detection-policy"></a>Weiche löschen Spalte Löschvorgang Erkennung Richtlinie

Wenn Sie Zeilen aus der Quelltabelle gelöscht haben, möchten Sie wahrscheinlich auch diese Zeilen aus dem Suchindex ebenfalls gelöscht. Wenn Sie die Richtlinie änderungsnachverfolgung SQL integriert verwenden, ist dies für Sie durchgeführt. Jedoch helfen nicht die Richtlinie änderungsnachverfolgung Hochwassermarken mit gelöschten Zeilen. Was zu tun ist? 

Wenn Sie die Zeilen aus der Tabelle physisch entfernt werden, wohl Abwesenheit – gibt es keine Möglichkeit, das Vorhandensein von Datensätzen abzuleiten, die nicht mehr vorhanden sind.  Die Methode "Weiche-löschen" können Sie jedoch als logisch gelöschten Zeilen markieren, ohne sie zu entfernen aus der Tabelle durch Hinzufügen einer Spalte, und markieren Zeilen aus, wie mit einem Marker Wert in dieser Spalte gelöscht.

Bei Verwendung die weiche löschen Methode können Sie angeben, die weiche löschen Richtlinie wie folgt beim Erstellen oder Aktualisieren der Datenquelle: 

    { 
        …, 
        "dataDeletionDetectionPolicy" : { 
           "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
           "softDeleteColumnName" : "[a column name]", 
           "softDeleteMarkerValue" : "[the value that indicates that a row is deleted]" 
        }
    }

Beachten Sie, dass die **SoftDeleteMarkerValue** einer Zeichenfolge – sein muss die Zeichenfolge-Darstellung der tatsächliche Wert verwenden. Wenn Sie eine ganze Spalte haben, in dem gelöschte Zeilen mit dem Wert 1 markiert sind, beispielsweise für Folgendes verwenden `"1"`; Wenn Sie eine BIT-Spalte, in dem gelöschte Zeilen mit der boolesche Wert true gekennzeichnet, haben, verwenden Sie `"True"`. 

<a name="TypeMapping"></a>
## <a name="mapping-between-sql-data-types-and-azure-search-data-types"></a>Zuordnung zwischen SQL-Datentypen und Datentypen Azure-Suche

|SQL-Datentyp | Zulässige Zielindex Feldtypen |Notizen 
|------|-----|----|
|Bit|Edm.Boolean, Edm.String| |
|Int, Smallint, tinyint |Edm.Int32, Int64, Edm.String| |
| bigint | Int64, Edm.String | |
| Real, Pufferzeiten |Edm.Double, Edm.String | |
| Smallmoney, decimal numerischen Geld | Edm.String| Azure-Suche unterstützt keine Edm.Double decimal Typen umwandeln, da dies Genauigkeit verlieren möchten |
| Zeichen, Nchar, Varchar, nvarchar | Edm.String<br/>Collection(EDM.String)|Eine Zeichenfolgenspalte in Collection(Edm.String) umwandelt erfordert eine Vorschau API Version 2015-02-28-Vorschau verwenden. Informationen finden Sie [in diesem Artikel](search-api-indexers-2015-02-28-preview.md#CreateIndexer)| 
|Smalldatetime "," Datetime "," datetime2 "," Datum "," datetimeoffset |Edm.DateTimeOffset, Edm.String| |
|uniqueidentifier | Edm.String | |
|"geography" | Edm.GeographyPoint | Nur Geography Instanzen vom Typ Punkt mit SRID 4326 (der Standard) werden unterstützt. | | 
|RowVersion| N/V |Zeile-Version Spalten können nicht in den Suchindex gespeichert werden, aber sie können für änderungsnachverfolgung verwendet werden | |
| Zeit Timespan Binärzahl Varbinary Image XML-Geometrie, CLR-Datentypen | N/V |Nicht unterstützt |


## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

**Q:** Kann ich mit SQL-Datenbanken auf IaaS virtuellen Computern in Azure ausgeführt SQL Azure-Indexer verwenden?

A: Ja. Müssen Sie jedoch Ihre Suchdienst, um zu Ihrer Datenbank herstellen, indem Sie die folgenden beiden Schritte ausführen dürfen. Lesen Sie Artikel [Konfigurieren von Azure Suche Indexer mit SQL Server ein Azure-virtuellen Computers aus eine Verbindung](search-howto-connecting-azure-sql-iaas-to-azure-search-using-indexers.md) für Weitere Informationen.

1. Möglicherweise müssen Sie die Datenbank mit einem vertrauenswürdigen Zertifikat so konfigurieren, dass der Suchdienst SSL-Verbindungen, um die Datenbank geöffnet werden kann.
2. Konfigurieren des Firewalls auf die IP-Adresse Ihre Suchdiensts zugreifen dürfen.

**Q:** Kann ich mit laufenden lokalen SQL-Datenbanken SQL Azure-Indexer verwenden? 

A: Wir empfehlen oder nicht unterstützen, wie Sie auf diese Weise müssen Sie die Datenbanken Datenverkehr Internet öffnen möchten. 

**Q:** Kann ich mit Datenbanken als SQL Server auf Windows Azure in IaaS ausgeführte SQL Azure-Indexer verwenden? 

A: Wir unterstützen nicht dieses Szenario zu, da wir den Indexer mit einem beliebigen Datenbanken als SQL Server getestet nicht geschehen ist.  

**Q:** Kann ich mehrere Indexer ausgeführt wird, klicken Sie auf einen Zeitplan erstellen? 

A: Ja. Nur eine Indexer kann jedoch gleichzeitig auf einem Knoten ausgeführt werden. Wenn Sie mehrere Indexer gleichzeitig ausgeführt, empfiehlt sich skalieren Suchdienst zu mehr als einer Suche. 

**Q:** Wirkt Ausführen eines Indexers meine Abfrage Arbeitsbelastung sich? 

A: Ja. Indexer auf einem der Knoten in der Suchdienst ausgeführt wird, und des Knotens Ressourcen zwischen Indizierung und Erstellen von Datenverkehr für Abfragen und sonstige API Anfragen freigegeben werden. Wenn Sie stark Indizierung und Abfrage Auslastung ausführen und eine hohe Rate 503 Fehler oder zunehmenden Reaktionszeiten auftreten, sollten Sie von Ihrem Suchdienst Skalierung.

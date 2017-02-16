<properties
   pageTitle="Entwickeln mit mehreren Bereichen in DocumentDB | Microsoft Azure"
   description="Erfahren Sie, wie Sie Ihre Daten in mehreren Bereichen von Azure DocumentDB, eine vollständig verwaltete NoSQL-Datenbank-Dienst zugreifen."
   services="documentdb"
   documentationCenter=""
   authors="kiratp"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="documentdb"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="kipandya"/>
   
# <a name="developing-with-multi-region-documentdb-accounts"></a>Entwickeln mit mehreren Region DocumentDB Konten

> [AZURE.NOTE] Globale Verteilung der DocumentDB Datenbanken ist in der Regel verfügbar und für alle neu erstellten DocumentDB Konten automatisch aktiviert. Wir arbeiten an globale Verteilung auf alle vorhandenen Konten aktivieren, aber in der Zwischenzeit Bedarf globale Verteilerlisten auf Ihr Konto aktiviert wenden Sie sich bitte [an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) , und wir werden Aktivieren der Arbeitsmappe für Sie jetzt.

Um [globale Verteilerlisten](documentdb-distribute-data-globally.md)nutzen zu können, können Clientanwendungen die Liste der bestellten bevorzugten Regionen verwendet werden, zum Ausführen von Vorgängen Dokument angeben. Dies kann durch Festlegen der Verbindungsrichtlinie erfolgen. Basierend auf der Kontokonfiguration Azure DocumentDB, aktuelle Landes-/ Verfügbarkeit und die Liste der bevorzugten angegeben, wird der Endpunkt am besten geeignete vom SDK zu Lese-Vorgänge ausgewählt werden. 

Bei der Initialisierung einer Verbindungs über den Client DocumentDB SDKs, wird die Liste der bevorzugten angegeben. Die SDKs akzeptieren einen optionalen Parameter "PreferredLocations" heißt eine sortierte Liste von Azure Regionen.

Das SDK sendet automatisch, dass alle in der aktuellen schreibt Region schreiben. 

Alle liest werden an den ersten verfügbaren Bereich in der Liste PreferredLocations gesendet. Wenn die Anforderung fehlschlägt, der Client nach unten zum nächsten Bereich fehl, und usw. auf. 

Der Client, dem SDKs nur zu versuchen werden, Lesen von den Bereichen in angegebenen PreferredLocations. Ja, werden angenommen, wenn das Datenbank-Konto in drei Regionen zur Verfügung ist, aber der Client zwei der Regionen nicht schreiben nur für PreferredLocations gibt, klicken Sie dann keine liest aus der schreiben, auch wenn Failover bereitgestellt werden.

Die Anwendung den aktuellen schreiben Endpunkt zu überprüfen und Endpunkt vom SDK ausgewählt werden, indem Sie die beiden Eigenschaften WriteEndpoint und ReadEndpoint, im SDK Version 1.8 und über lesen. 

Wenn die PreferredLocations-Eigenschaft nicht festgelegt ist, werden alle Besprechungsanfragen aus der aktuellen schreiben Region bereitgestellt werden. 


## <a name="net-sdk"></a>.NET SDK
Das SDK kann ohne Code Änderungen verwendet werden. In diesem Fall, das SDK automatisch weist beide liest und schreibt in der aktuellen schreiben Region. 

In der Version 1,8 und höher von .NET SDK hat der ConnectionPolicy Parameter für den Konstruktor DocumentClient eine Eigenschaft namens Microsoft.Azure.Documents.ConnectionPolicy.PreferredLocations. Diese Eigenschaft ist vom Typ Websitesammlung `<string>` und sollte eine Liste der Regionsnamen enthalten. Die Zeichenfolgenwerte pro der Regionsname Spalte auf den [Azure Regionen] formatiert sind [ regions] ohne Leerzeichen vor oder nach der ersten Seite und jeweils die letzten Zeichen.

Die aktuelle schreiben und gelesen Endpunkte sind jeweils in DocumentClient.WriteEndpoint und DocumentClient.ReadEndpoint verfügbar.

> [AZURE.NOTE] Die URLs für die Endpunkte sollten nicht als langer Lebensdauer Konstanten angesehen. Der Dienst möglicherweise diese zu einem beliebigen Zeitpunkt aktualisieren. Das SDK übernimmt diese Änderung automatisch aus.

    // Getting endpoints from application settings or other configuration location
    Uri accountEndPoint = new Uri(Properties.Settings.Default.GlobalDatabaseUri);
    string accountKey = Properties.Settings.Default.GlobalDatabaseKey;

    //Setting read region selection preference 
    connectionPolicy.PreferredLocations.Add(LocationNames.WestUS); // first preference
    connectionPolicy.PreferredLocations.Add(LocationNames.EastUS); // second preference
    connectionPolicy.PreferredLocations.Add(LocationNames.NorthEurope); // third preference

    // initialize connection
    DocumentClient docClient = new DocumentClient(
        accountEndPoint,
        accountKey,
        connectionPolicy);

    // connect to DocDB 
    await docClient.OpenAsync().ConfigureAwait(false);


## <a name="nodejs-javascript-and-python-sdks"></a>NodeJS, JavaScript und Python SDKs
Das SDK kann ohne Code Änderungen verwendet werden. In diesem Fall wird automatisch das SDK direkte sowohl Lese- und in der aktuellen schreibt Region schreiben. 

In Version 1,8 und höher der einzelnen SDK bezeichnet den ConnectionPolicy Parameter für den Konstruktor DocumentClient DocumentClient.ConnectionPolicy.PreferredLocations eine neue Eigenschaft. Dies ist Parameter ein Array von Zeichenfolgen, die eine Liste von Regionsnamen akzeptiert. Die Namen werden pro Spalte Region Name in die [Regionen Azure] formatiert [ regions] Seite. Sie können auch die vordefinierten Konstanten in das Objekt Komfort AzureDocuments.Regions verwenden.

Die aktuelle schreiben und gelesen Endpunkte sind jeweils in DocumentClient.getWriteEndpoint und DocumentClient.getReadEndpoint verfügbar.

> [AZURE.NOTE] Die URLs für die Endpunkte sollten nicht als langer Lebensdauer Konstanten angesehen. Der Dienst möglicherweise diese zu einem beliebigen Zeitpunkt aktualisieren. Das SDK wird diese Änderung nicht automatisch verarbeiten.

Es folgt ein Beispiels für NodeJS/Javascript. Python und Java wird das gleiche Muster folgen.

    // Creating a ConnectionPolicy object
    var connectionPolicy = new DocumentBase.ConnectionPolicy();
    
    // Setting read region selection preference, in the following order -
    // 1 - West US
    // 2 - East US
    // 3 - North Europe
    connectionPolicy.PreferredLocations = ['West US', 'East US', 'North Europe'];
    
    // initialize the connection
    var client = new DocumentDBClient(host, { masterKey: masterKey }, connectionPolicy);


## <a name="rest"></a>REST 
Nachdem Sie eine Datenbankkonto in mehreren Regionen zur Verfügung gestellt wurde, können Clients eine GET-Anforderung ausführen, klicken Sie auf den folgenden URI seine Verfügbarkeit Abfragen.

    https://{databaseaccount}.documents.azure.com/

Der Dienst wird eine Liste der Regionen und deren entsprechenden DocumentDB Endpunkt URIs für die Replikate zurückgeben. Die aktuelle schreiben Region wird in der Antwort angegeben werden. Der Client kann den entsprechenden Endpunkt für alle weiteren REST-API Anfragen klicken Sie dann wie folgt auswählen.

Beispiel für Antwort

    {
        "_dbs": "//dbs/",
        "media": "//media/",
        "writableLocations": [
            {
                "Name": "West US",
                "DatabaseAccountEndpoint": "https://globaldbexample-westus.documents.azure.com:443/"
            }
        ],
        "readableLocations": [
            {
                "Name": "East US",
                "DatabaseAccountEndpoint": "https://globaldbexample-eastus.documents.azure.com:443/"
            }
        ],
        "MaxMediaStorageUsageInMB": 2048,
        "MediaStorageUsageInMB": 0,
        "ConsistencyPolicy": {
            "defaultConsistencyLevel": "Session",
            "maxStalenessPrefix": 100,
            "maxIntervalInSeconds": 5
        },
        "addresses": "//addresses/",
        "id": "globaldbexample",
        "_rid": "globaldbexample.documents.azure.com",
        "_self": "",
        "_ts": 0,
        "_etag": null
    }


-   Alle sich, bereitstellen und löschen Anfragen müssen in die angegebene schreiben URI wechseln.
-   Ruft alle und andere Anforderungen schreibgeschützt (beispielsweise Abfragen) möglicherweise an einer beliebigen Endpunkt des Clients Wahl geleitet.

Schreiben Sie Anfragen an schreibgeschützte Bereiche mit dem HTTP-Fehlercode 403 ("verboten") fehl.

Wenn die Region schreiben nach den Kundennamen initial Discovery Phase verwandelt hat, tritt nachfolgenden Schreibvorgang in der vorherigen schreiben Region mit HTTP-Fehlercode 403 ("verboten"). Der Client sollte dann die Liste der Gebiete erneut zum Abrufen der aktualisierten schreiben Region erhalten.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Verteilen von Daten Global mit DocumentDB in den folgenden Artikeln:

- [Verteilen von Daten mit DocumentDB Global](documentdb-distribute-data-globally.md)
- [Konsistenz Ebenen](documentdb-consistency-levels.md)
- [Funktionsweise der Durchsatz mit mehreren Bereichen](documentdb-manage.md#how-throughput-works-with-multiple-regions)
- [Hinzufügen von Regionen über das Azure-portal](documentdb-portal-global-replication.md)

[regions]: https://azure.microsoft.com/regions/ 

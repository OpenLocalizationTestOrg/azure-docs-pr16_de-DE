<properties 
    pageTitle="Arbeiten mit Geodaten in Azure DocumentDB | Microsoft Azure" 
    description="Verstehen Sie, wie erstellen, indizieren und räumliche Objekte mit Azure DocumentDB Abfragen." 
    services="documentdb" 
    documentationCenter="" 
    authors="arramac" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="documentdb" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="10/25/2016" 
    ms.author="arramac"/>
    
# <a name="working-with-geospatial-data-in-azure-documentdb"></a>Arbeiten mit Geodaten in Azure DocumentDB

In diesem Artikel wird eine Einführung in die Funktionalität Geodaten in [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/). Nach dem Lesen, werden Sie die folgenden Fragen beantworten:

- Wie speichere ich mit Azure DocumentDB raumgeometrischen Daten?
- Wie kann ich Geodaten in Azure DocumentDB in SQL und LINQ Abfragen?
- Wie ich aktivieren oder Deaktivieren von räumliche Indizierung in DocumentDB?

Finden Sie dieses [Projekt Github](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/Geospatial/Program.cs) für Codebeispielen.

## <a name="introduction-to-spatial-data"></a>Einführung in Raumgeometrische Daten

Räumliche Daten beschreibt die Position und Form von Objekten im Raum. In den meisten Fällen angebracht entsprechen Objekte auf weltweit, d. h. Geodaten diese. Räumliche Daten können zum Darstellen der Position von einer Person, die Begrenzungslinie einer Stadt oder einen See oder Ort relevante verwendet werden. In vielen Fällen betreffen häufige Verwendung Fällen Näherung Abfragen für "suchen z. B. alle einem Café befinden in der Nähe meiner aktuellen Speicherort". 

### <a name="geojson"></a>GeoJSON
DocumentDB unterstützt Indizierung und Abfragen von Punkt Geodaten, die mit dieser [Spezifikation GeoJSON](http://geojson.org/geojson-spec.html)dargestellt wird. GeoJSON Datenstrukturen sind immer gültige JSON-Objekte, damit sie gespeichert werden können und DocumentDB ohne spezielle Tools und Bibliotheken mit abgefragt. Die DocumentDB SDKs bieten Helper Klassen und Methoden, die für die Arbeit mit räumlichen Daten vereinfachen. 

### <a name="points-linestrings-and-polygons"></a>Punkte, Linestrings und Polygone
Einen **Punkt** kennzeichnet eine einzelne Position im Raum. In Geodaten stellt ein Punkt genauen Standort, der eine Adresse ein Lebensmittelgeschäft, einem Kiosk, ein Auto oder eines Orts werden konnte.  Ein Point wird GeoJSON (und DocumentDB), mit deren Koordinate Paar oder Länge und Breite dargestellt. Hier ist ein Beispiel JSON für einen Punkt ein.

**Punkte in DocumentDB**

    {
       "type":"Point",
       "coordinates":[ 31.9, -4.8 ]
    }

>[AZURE.NOTE] Gibt an, die GeoJSON Spezifikation Länge vor- und Breite zweiten. Wie in einer anderen Anwendung Zuordnung Länge und Breite sind Winkel und dargestellt in Bezug auf Grad. Länge-Werte werden aus dem Nullmeridian gemessen und zwischen ‑180 und 180.0 Grad und Breite werden Werte aus der Äquator gemessen werden und zwischen-90.0 und 90.0 Grad. 
>
> DocumentDB interpretiert Koordinaten pro WGS 84 Bezug auf den dargestellt. Finden Sie unter Weitere Details zu koordinieren Bezug Systeme.

Dies kann in einem Dokument DocumentDB, wie in diesem Beispiel für ein Benutzerprofil mit Standortdaten dargestellt eingebettet werden:

**Verwenden Sie Profil mit Speicherort gespeichert in DocumentDB**

    {
       "id":"documentdb-profile",
       "screen_name":"@DocumentDB",
       "city":"Redmond",
       "topics":[ "NoSQL", "Javascript" ],
       "location":{
          "type":"Point",
          "coordinates":[ 31.9, -4.8 ]
       }
    }

Zusätzlich zu den Buchstaben unterstützt GeoJSON auch LineStrings und Polygone. **LineStrings** stehen für eine Reihe von zwei oder mehr Punkte in Leerzeichen und den Liniensegmenten, die sie verbinden. Linestrings werden in Geodaten häufig verwendet, um Autobahnen oder Flüsse darzustellen. Ein **Polygon** ist eine Begrenzungslinie der verbundenen Punkte, die einen geschlossenen LineString bildet. Polygone werden häufig verwendet, um natürlich formationen wie Seen oder politischen Ländern wie Orten und Staaten darzustellen. Hier ist ein Beispiel für ein Polygon in DocumentDB ein. 

**Polygone in DocumentDB**

    {
       "type":"Polygon",
       "coordinates":[
           [ 31.8, -5 ],
           [ 31.8, -4.7 ],
           [ 32, -4.7 ],
           [ 32, -5 ],
           [ 31.8, -5 ]
       ]
    }

>[AZURE.NOTE] Die GeoJSON-Spezifikation erfordert, dass für gültige Polygone, die letzten Koordinaten bereitgestellten das erste, erstellen ein geschlossenes Shape identisch sein sollte.
>
>Punkte innerhalb eines Polygons müssen gegen den Uhrzeigersinn nacheinander angegeben werden. Ein Polygons in Reihenfolge im Uhrzeigersinn festgelegten stellt die Umkehrung der Bereich des dar.

Zusätzlich zu Punkt, LineString und Polygon gibt GeoJSON auch die Darstellung für so gruppieren Sie mehrere Geodaten Speicherorte sowie zum geografischen Standort als ein **Feature**beliebige Eigenschaften zuordnen. Da diese Objekte JSON gültig sind, können sie alle gespeichert und verarbeitet werden in DocumentDB. DocumentDB unterstützt jedoch nur die automatische Indizierung von Punkten.

### <a name="coordinate-reference-systems"></a>Koordinieren Bezug Betriebssysteme

Da das Shape weltweit unregelmäßiges ist, wird der Geodaten Koordinaten in vielen koordinieren Bezugssystemen (CRS), jede mit ihren eigenen Rahmen als Referenz und Maßeinheiten dargestellt. Beträgt beispielsweise der "nationalen Raster von Großbritannien" ein Bezug System ist sehr genau für Großbritannien, aber nicht außerhalb es. 

Die am häufigsten verwendeten CRS verwendet wird heute der World Geodetic System [WGS-84](http://earth-info.nga.mil/GandG/wgs84/). Verwenden WGS 84 GPS-Geräten und viele Zuordnungsdienste einschließlich Google-Karten und Bing Maps-APIs. DocumentDB unterstützt Indizierung und Abfragen von Geodaten nur die WGS 84 CRS verwenden. 

## <a name="creating-documents-with-spatial-data"></a>Erstellen von Dokumenten mit räumlichen Daten
Beim Erstellen von Dokumenten, die GeoJSON Werte enthalten, werden sie automatisch in Übereinstimmung mit der Richtlinie Indizieren der Sammlung mit einem räumlichen Index indiziert. Wenn Sie mit einer DocumentDB SDK in einer anderen Sprache wie Python oder Node.js dynamisch eingegebenen arbeiten, müssen Sie gültige GeoJSON erstellen.

**Dokument mit Geodaten in Node.js erstellen**

    var userProfileDocument = {
       "name":"documentdb",
       "location":{
          "type":"Point",
          "coordinates":[ -122.12, 47.66 ]
       }
    };

    client.createDocument(`dbs/${databaseName}/colls/${collectionName}`, userProfileDocument, (err, created) => {
        // additional code within the callback
    });

Wenn Sie mit der .NET (oder Java) SDKs arbeiten, können Sie die neuen Punkt und Polygon Klassen innerhalb des Namespace Microsoft.Azure.Documents.Spatial verwenden, um Standortinformationen innerhalb Ihrer Anwendungsobjekte einzubetten. Diese Klassen unterstützen die Serialisierung und Deserialisierung räumliche Daten in GeoJSON zu vereinfachen.

**Dokument mit Geodaten in .NET erstellen**

    using Microsoft.Azure.Documents.Spatial;
    
    public class UserProfile
    {
        [JsonProperty("name")]
        public string Name { get; set; }

        [JsonProperty("location")]
        public Point Location { get; set; }
        
        // More properties
    }
    
    await client.CreateDocumentAsync(
        UriFactory.CreateDocumentCollectionUri("db", "profiles"), 
        new UserProfile 
        { 
            Name = "documentdb", 
            Location = new Point (-122.12, 47.66) 
        });

Wenn Sie verfügen nicht über die Breite und Informationen, aber die physische Adressen oder den Namen des Orts wie Ort oder Land haben, können Sie die tatsächlichen Koordinaten Nachschlagen mithilfe eines Diensts geocodierung wie Bing Maps REST-Dienste. Erfahren Sie mehr über die Bing Maps-geocodierung [hier](https://msdn.microsoft.com/library/ff701713.aspx).

## <a name="querying-spatial-types"></a>Abfragen räumliche Datentypen

Jetzt, da wir wollen zum Einfügen von Geodaten geöffnet haben, sehen wir uns an, wie diese Daten mit DocumentDB mit SQL und LINQ Abfragen.

### <a name="spatial-sql-built-in-functions"></a>Räumliche integrierte SQL-Funktionen
DocumentDB unterstützt die folgenden öffnen Geodaten Consortium (OGC) integrierte Funktionen für Geodaten Abfragen. Weitere Informationen hierzu auf die vollständige Menge der integrierte Funktionen in der SQL-Sprache Näheres [Abfrage DocumentDB](documentdb-sql-query.md).

<table>
<tr>
  <td><strong>Verwendung</strong></td>
  <td><strong>Beschreibung</strong></td>
</tr>
<tr>
  <td>ST_DISTANCE (Point_expr, Point_expr)</td>
  <td>Gibt den Abstand zwischen den beiden GeoJSON Punkt Ausdrücken zurück.</td>
</tr>
<tr>
  <td>ST_WITHIN (Point_expr, Polygon_expr)</td>
  <td>Gibt einen booleschen Ausdruck, der angibt, ob der im ersten Argument angegebene GeoJSON Punkt innerhalb der GeoJSON Polygons in das zweite Argument ist.</td>
</tr>
<tr>
  <td>ST_ISVALID</td>
  <td>Gibt einen booleschen Wert, der angibt, ob der angegebene GeoJSON Punkt oder Polygons Ausdruck gültig ist.</td>
</tr>
<tr>
  <td>ST_ISVALIDDETAILED</td>
  <td>Gibt JSON-Werte, enthält ein boolescher Wert, wenn der angegebene GeoJSON Punkt oder Polygons Ausdruck gültig ist, und ungültige, darüber hinaus den Grund als Zeichenfolgenwert.</td>
</tr>
</table>

Räumliche Funktionen können zum Näherung Abfragen raumgeometrischen Daten verwendet werden. Hier wird beispielsweise eine Abfrage, die alle familiäre Dokumente gibt, die innerhalb von 30 km von der angegebenen Position die integrierte ST_DISTANCE-Funktion verwendet werden. 

**Abfrage**

    SELECT f.id 
    FROM Families f 
    WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000

**Ergebnisse**

    [{
      "id": "WakefieldFamily"
    }]

Wenn Sie die räumliche Indizierung in Ihrer Richtlinie Indizierung einbeziehen, werden "Abstand Abfragen" effizient über den Index bereitgestellt. Weitere Informationen zu räumliche Indizierung finden Sie weiter unten im Abschnitt. Wenn Sie einen räumlichen Index für die angegebenen Pfade besitzen, können Sie weiterhin raumbezogener Abfragen durchführen, indem Sie angeben `x-ms-documentdb-query-enable-scan` Anforderungsheader mit den Wert auf "True". In .NET können dazu werden optionale **FeedOptions** als Argument übergeben, um Abfragen mit [EnableScanInQuery](https://msdn.microsoft.com/library/microsoft.azure.documents.client.feedoptions.enablescaninquery.aspx#P:Microsoft.Azure.Documents.Client.FeedOptions.EnableScanInQuery) auf True festgelegt ist. 

Prüfen, ob ein Punkt innerhalb eines Polygons liegt, kann ST_WITHIN verwendet werden. Häufig verwendeten Polygone Grenzen wie Postleitzahlen, Bundesstaat Begrenzungen oder natürlich formationen darstellen. Erneut Wenn Sie räumliche Indizierung in Ihrer Richtlinie Indizierung enthalten, werden dann "in" Abfragen effizient über den Index bereitgestellt werden. 

Polygon Argumente ST_WITHIN können enthalten nur ein einfaches klingeln, d. h. die Polygone müssen nicht Lücken in diese. 

**Abfrage**

    SELECT * 
    FROM Families f 
    WHERE ST_WITHIN(f.location, {
        'type':'Polygon', 
        'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]
    })

**Ergebnisse**

    [{
      "id": "WakefieldFamily",
    }]
    
>[AZURE.NOTE] Ähnlich wie nicht übereinstimmende Typen funktioniert in DocumentDB Abfrage, wenn der Speicherort in angegebene entweder Argument ist falsch formatierte oder ungültige, und klicken Sie dann es ergeben, **nicht definierten** und ausgewerteten Dokument aus den Abfrageergebnissen übersprungen werden. Wenn Ihre Abfrage keine Ergebnisse zurückgibt, ausführen ST_ISVALIDDETAILED zum Debuggen, warum der Spatail ungültig ist.     

DocumentDB unterstützt auch die Durchführung von umgekehrter Abfragen, d. h. können Sie indizieren Polygone oder mehrerer Zeilen DocumentDB und dann Abfrage für die Bereiche, die einen bestimmten Punkt enthalten. Dieses Muster wird häufig in Logistik zum Identifizieren, z. B., wenn ein LKW wechselt oder einen bestimmten Bereich aus. 

**Abfrage**

    SELECT * 
    FROM Areas a 
    WHERE ST_WITHIN({'type': 'Point', 'coordinates':[31.9, -4.8]}, a.location)


**Ergebnisse**

    [{
      "id": "MyDesignatedLocation",
      "location": {
        "type":"Polygon", 
        "coordinates": [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]
      }
    }]

ST_ISVALID und ST_ISVALIDDETAILED kann verwendet werden, prüfen, ob ein räumliche Objekt gültig ist. Die folgende Abfrage überprüft beispielsweise die Gültigkeit eines Punkts mit einem Wert außerhalb des Gültigkeitsbereichs Breite (-132.8). ST_ISVALID gibt nur der boolesche Wert und ST_ISVALIDDETAILED zurück, der boolesche Wert und eine Zeichenfolge mit den Grund, warum es als ungültig angesehen wird.

**Abfrage**

    SELECT ST_ISVALID({ "type": "Point", "coordinates": [31.9, -132.8] })

**Ergebnisse**

    [{
      "$1": false
    }]

Diese Funktionen können auch Polygone überprüft verwendet werden. Beispielsweise verwenden hier wir ST_ISVALIDDETAILED ein Polygons überprüft, das nicht abgeschlossen ist. 

**Abfrage**

    SELECT ST_ISVALIDDETAILED({ "type": "Polygon", "coordinates": [[ 
        [ 31.8, -5 ], [ 31.8, -4.7 ], [ 32, -4.7 ], [ 32, -5 ] 
        ]]})

**Ergebnisse**

    [{
       "$1": { 
          "valid": false, 
          "reason": "The Polygon input is not valid because the start and end points of the ring number 1 are not the same. Each ring of a polygon must have the same start and end points." 
        }
    }]
    
### <a name="linq-querying-in-the-net-sdk"></a>LINQ im .NET SDK Abfragen

DocumentDB .NET SDK auch Anbieter stub Methoden `Distance()` und `Within()` für die Verwendung innerhalb LINQ-Ausdrücke. Der DocumentDB LINQ-Anbieter übersetzt Methodenaufrufen die entsprechende SQL integrierten Funktion Anrufe (ST_DISTANCE und ST_WITHIN Hilfethemas). 

Hier ist ein Beispiel für eine LINQ-Abfrage, die alle Dokumente in der DocumentDB-Auflistung, deren Wert "Speicherort" wird innerhalb eines Radius von 30km des angegebenen zeigen mit LINQ, findet ein.

**LINQ-Abfrage für Abstand**

    foreach (UserProfile user in client.CreateDocumentQuery<UserProfile>(UriFactory.CreateDocumentCollectionUri("db", "profiles"))
        .Where(u => u.ProfileType == "Public" && a.Location.Distance(new Point(32.33, -4.66)) < 30000))
    {
        Console.WriteLine("\t" + user);
    }

Auf ähnliche Weise hier eine Abfrage für die Suche nach allen Dokumenten, deren "Speicherort" das angegebene Feld/Polygon enthalten ist. 

**LINQ-Abfragen für in**

    Polygon rectangularArea = new Polygon(
        new[]
        {
            new LinearRing(new [] {
                new Position(31.8, -5),
                new Position(32, -5),
                new Position(32, -4.7),
                new Position(31.8, -4.7),
                new Position(31.8, -5)
            })
        });

    foreach (UserProfile user in client.CreateDocumentQuery<UserProfile>(UriFactory.CreateDocumentCollectionUri("db", "profiles"))
        .Where(a => a.Location.Within(rectangularArea)))
    {
        Console.WriteLine("\t" + user);
    }


Jetzt, da wir wollen Vorgehensweise zum Abfragen von Dokumenten mithilfe von LINQ und SQL geöffnet haben, werfen Sie einen Blick auf DocumentDB für räumliche Indizierung konfigurieren.

## <a name="indexing"></a>Indizierung

Wie wir in das [Schema unabhängig Indizierung mit Azure DocumentDB](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) Papier beschrieben, wir DocumentDBs Datenbank-Engine werden entworfen wirklich Schema unabhängig und JSON erster Klasse unterstützen. Die Datenbank-Engine optimiert Schreiben von DocumentDB versteht systembedingt raumgeometrischen Daten (Punkte, Polygone und Linien) im Standard GeoJSON dargestellt wird.

Kurz gesagt, ist die Geometrie Vorschau von geodätischen Koordinaten auf einer 2D Ebene dann schrittweise in Zellen mithilfe einer **Quadtree**unterteilt. Diese Zellen sind basierend auf den Speicherort der Zelle innerhalb einer **Hilbert Leerzeichen ausfüllen Kurve**, der Ort der Punkte behält 1D zugeordnet. Darüber hinaus, wenn Standortdaten indiziert ist, erfolgt über einen Prozess als **Tesselierung**bezeichnet, d. h. alle Zellen, die einen Speicherort schneiden erkannt und als Schlüssel im Index DocumentDB gespeichert werden. Zum Zeitpunkt der Abfrage werden Argumente wie Punkten und Polygonen auch die relevanten ID Zellbereiche zum Extrahieren von tesselierten und dann zum Abrufen von Daten aus dem Index verwendet.

Wenn Sie eine Indizierung Richtlinie angeben, die für Raumindexes enthält / * (alle Pfade), und klicken Sie dann alle Punkte, die in der Sammlung gefunden für effiziente raumbezogener Abfragen (ST_WITHIN und ST_DISTANCE) indiziert sind. Indizes keinen Genauigkeitswert enthalten, und verwenden Sie einen Standardwert für die Genauigkeit immer.

>[AZURE.NOTE] DocumentDB unterstützt Punkte, Polygone und LineStrings automatische Indizierung

Im folgenden JSON-Codeausschnitt wird eine Indizierung Richtlinie mit räumliche Indizierung aktiviert, d. h. Indizieren einer beliebigen GeoJSON Stelle innerhalb von Dokumenten für räumliche Abfragen gefunden. Wenn Sie die Indizierung Richtlinie über das Azure-Portal ändern, können Sie die folgende JSON für Indizierung Richtlinie räumliche Indizierung für Ihre Websitesammlung aktivieren angeben.

**Sammlung Indizierung Richtlinie JSON mit räumlichen für Punkten und Polygonen aktiviert**

    {
       "automatic":true,
       "indexingMode":"Consistent",
       "includedPaths":[
          {
             "path":"/*",
             "indexes":[
                {
                   "kind":"Range",
                   "dataType":"String",
                   "precision":-1
                },
                {
                   "kind":"Range",
                   "dataType":"Number",
                   "precision":-1
                },
                {
                   "kind":"Spatial",
                   "dataType":"Point"
                },
                {
                   "kind":"Spatial",
                   "dataType":"Polygon"
                }                
             ]
          }
       ],
       "excludedPaths":[
       ]
    }

Hier ist ein Codeausschnitt in .NET wird gezeigt, wie eine Auflistung mit räumliche Indizierung aktiviert für alle Pfade mit Punkte erstellen, die ein. 

**Erstellen einer Websitesammlung mit räumliche Indizierung**

    DocumentCollection spatialData = new DocumentCollection()
    spatialData.IndexingPolicy = new IndexingPolicy(new SpatialIndex(DataType.Point)); //override to turn spatial on by default
    collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), spatialData);

Und so sieht wie ändern eine vorhandene Websitesammlung zum Nutzen der räumliche Indizierung über alle Punkte, die in die Dokumente gespeichert sind.

**Ändern einer vorhandenen Websitesammlungs mit räumliche Indizierung**

    Console.WriteLine("Updating collection with spatial indexing enabled in indexing policy...");
    collection.IndexingPolicy = new IndexingPolicy(new SpatialIndex(DataType.Point));
    await client.ReplaceDocumentCollectionAsync(collection);

    Console.WriteLine("Waiting for indexing to complete...");
    long indexTransformationProgress = 0;
    while (indexTransformationProgress < 100)
    {
        ResourceResponse<DocumentCollection> response = await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"));
        indexTransformationProgress = response.IndexTransformationProgress;

        await Task.Delay(TimeSpan.FromSeconds(1));
    }

> [AZURE.NOTE] Wenn die Position GeoJSON Wert innerhalb des Dokuments fehlerhaften oder ungültig ist, wird Klicken Sie dann es nicht für räumliche Abfragen indiziert abrufen. Sie können die Position von Werten mit ST_ISVALID und ST_ISVALIDDETAILED überprüfen.
>
> Wenn der Definition Ihrer Websitesammlung Partitionsschlüssel enthält, wird nicht indizieren Transformation Vorgangsfortschritt gemeldet. 

## <a name="next-steps"></a>Nächste Schritte
Jetzt, da Sie Informationen zu den ersten Schritten mit Geodaten Unterstützung in DocumentDB gelernt haben, können Sie folgende Schritte ausführen:

- Beginnen Sie mit den [Codebeispielen auf Github Geodaten .NET](https://github.com/Azure/azure-documentdb-dotnet/blob/fcf23d134fc5019397dcf7ab97d8d6456cd94820/samples/code-samples/Geospatial/Program.cs) Programmieren
- Hände, die auf mit Geodaten Abfragen, bei dem [DocumentDB Abfrage Umgebung](http://www.documentdb.com/sql/demo#geospatial) zu erhalten
- Weitere Informationen zu [DocumentDB Abfrage](documentdb-sql-query.md)
- Weitere Informationen zu [DocumentDB Indizierung Richtlinien](documentdb-indexing-policies.md)

<properties 
    pageTitle="Partitionierung und dieselbe Skalierung in Azure DocumentDB | Microsoft Azure"      
    description="Informationen Sie zu in Azure DocumentDB, so konfigurieren die Partitionierung und partitionieren Tasten und zum Auswählen der richtigen Partitionsschlüssel für eine Anwendung wie vorherigen funktioniert."         
    services="documentdb" 
    authors="arramac" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter=""/> 

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/20/2016" 
    ms.author="arramac"/> 

# <a name="partitioning-and-scaling-in-azure-documentdb"></a>Partitionierung und dieselbe Skalierung in Azure DocumentDB
[Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) soll helfen, Sie schneller, vorhersehbar Performance und Skalierung nahtlos zusammen mit Ihrer Anwendung erzielen, während sie wächst. In diesem Artikel bietet einen Überblick über die wie vorherigen arbeitet in DocumentDB, und es wird beschrieben, wie Sie DocumentDB-Sammlungen, um Ihre Programme effektiv skalieren konfigurieren können.

Nach dem Lesen dieses Artikels, werden Sie die folgenden Fragen beantworten:   

- Wie funktioniert der vorherigen in Azure DocumentDB?
- Wie kann ich die Partitionierung in DocumentDB konfigurieren
- Was sind die Tasten Partition, und wie auswählen ich der richtigen Partitionsschlüssel für meine Anwendung?

Um mit Code anzufangen, Herunterladen von [DocumentDB Leistung testen Treiber Stichprobe](https://github.com/Azure/azure-documentdb-dotnet/tree/a2d61ddb53f8ab2a23d3ce323c77afcf5a608f52/samples/documentdb-benchmark)des Projekts bitte. 

## <a name="partitioning-in-documentdb"></a>Partitionierung in DocumentDB

Sie können in DocumentDB speichern und Schema zu öffnendes JSON-Dokumente mit der Reihenfolge der Millisekunden Reaktionszeiten bei einer beliebigen Skalierung Abfragen. DocumentDB enthält Container zum Speichern von Daten, so genannte **Websitesammlungen**. Websitesammlungen sind logische Ressourcen und physische Partitionen oder Servern umfassen können. Die Anzahl der Partitionen wird durch DocumentDB ausgehend von der Größe und der bereitgestellte Durchsatz der Sammlung bestimmt. Jeder Partition in DocumentDB hat eine bestimmte SSD gesicherten Speicher zugeordnet und hohen Verfügbarkeit repliziert. Verwaltung der Anwendungsverzeichnispartition vollständig von Azure DocumentDB verwaltet wird, und Sie haben keinen zum Schreiben von Code komplexe oder verwalten Ihre Partitionen. DocumentDB Websitesammlungen sind **praktisch unbegrenzte** in Bezug auf die Speicherung und Durchsatz. 

Partitionierung ist vollständig transparent an Ihrer Anwendung. DocumentDB unterstützt schnelle liest und schreibt, SQL und LINQ Abfragen, JavaScript-basierte Transaktionen Logik, Konsistenz Ebenen und abgestimmte Access Kontrolle über die REST-API Anrufe an eine Ressource einzelne Websitesammlung. Der Dienst behandelt Verteilen von Daten über Partitionen und routing Abfrage Anfragen auf die richtigen Partition an. 

Wie funktioniert dies? Wenn Sie eine Auflistung in DocumentDB erstellen, werden Sie feststellen, dass es ist ein **Partition** Konfiguration der Wert, den Sie angeben können. Dies ist das JSON-Eigenschaft (oder die Pfadangabe) in Ihren Dokumenten, die zum Verteilen von Daten zwischen mehreren Servern oder Partitionen von DocumentDB verwendet werden können. DocumentDB werden den wichtigsten Partitionswert Hashing und gestrichelte Ergebnis verwenden, um die Partition zu ermitteln, in der das JSON-Dokument gespeichert werden sollen. Alle Dokumente mit der gleichen Partitionsschlüssel werden in der gleichen Partition gespeichert werden. 

Angenommen Sie, eine Anwendung, die Daten zu Mitarbeitern und deren Abteilungen in DocumentDB gespeichert sind. Wählen Sie uns `"department"` wie die Partition Key-Eigenschaft, um das Layout von Daten nach Abteilung skalieren. Jedem Dokument in DocumentDB muss eine obligatorische enthalten `"id"` Eigenschaft, die für jedes Dokument mit demselben Partition Schlüsselwert, z. B. muss eindeutig sein `"Marketing`". Jedes Dokument gespeichert, die in einer Websitesammlung müssen eine eindeutige Kombination von Partitionsschlüssel und -Id, z. B. `{ "Department": "Marketing", "id": "0001" }`, `{ "Department": "Marketing", "id": "0002" }`, und `{ "Department": "Sales", "id": "0001" }`. Kurzum, die zusammengesetzten Eigenschaft (Partition Key, Id) ist der Primärschlüssel für Ihre Websitesammlung.

### <a name="partition-keys"></a>Partition Tasten
Die Wahl der Partitionsschlüssel ist eine wichtige Entscheidung, die Sie zur Entwurfszeit machen müssen. Sie müssen eine JSON-Eigenschaftsname auswählen, die verfügt über eine Vielzahl von Werten und wahrscheinlich gleichmäßig Access Muster verteilt haben. Der Partitionsschlüssel angegeben als JSON Pfad, z. B. `/department` die Eigenschaft Abteilung darstellt. 

Die folgende Tabelle enthält Beispiele für Key Partitionsdefinitionen und die entsprechenden jede JSON-Werte.

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><strong>Key Partitionspfad</strong></p></td>
            <td valign="top"><p><strong>Beschreibung</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>/Department</p></td>
            <td valign="top"><p>Entspricht dem JSON-Wert der doc.department, in dem Dokument auf das Dokument befindet.</p></td>
        </tr>
        <tr>
            <td valign="top"><p>/ Eigenschaften/name</p></td>
            <td valign="top"><p>Entspricht dem JSON-Wert der doc.properties.name darin Dokument auf das Dokument (geschachtelte Eigenschaft).</p></td>
        </tr>
        <tr>
            <td valign="top"><p>/ ID</p></td>
            <td valign="top"><p>Entspricht dem JSON-Wert der doc.id (-Id und Partitionsgröße Schlüssel sind die gleiche Eigenschaft).</p></td>
        </tr>
        <tr>
            <td valign="top"><p>/ "Abteilungsname"</p></td>
            <td valign="top"><p>Entspricht dem JSON-Wert der Doc ["Abteilungsname"], in dem Dokument auf das Dokument befindet.</p></td>
        </tr>
    </tbody>
</table>

> [AZURE.NOTE] Die Syntax für Key Partitionspfad ähnelt dem die Pfadangabe für Indizierung Richtlinie Netzwerkpfade durch die wichtigsten Differenz, die der Pfad der Eigenschaft anstelle des Werts entspricht, d. h. es ist keine Platzhalter am Ende. Beispielsweise würden Sie/Abteilung/angeben? Wenn Sie die Werte unter Abteilung indizieren, geben Sie /department als die wichtigsten Definition Indexpartition aber an. Die wichtigsten Partitionspfad wird implizit indiziert und kann nicht mithilfe von Indizierung außer Kraft setzt Indizierung ausgeschlossen werden.

Werfen Sie einen Blick auf wie die Wahl der Partitionsschlüssel wirkt sich die Leistung der Anwendung auf ein.

### <a name="partitioning-and-provisioned-throughput"></a>Vorherigen und bereitgestellte Durchsatz
DocumentDB ist vorhersehbar Leistung ausgelegt. Wenn Sie eine Websitesammlung erstellen, reservieren Sie Durchsatz in Bezug auf die ** [Anfrage Einheiten](documentdb-request-units.md) (RU) pro Sekunde**an. Jede Anforderung wird eine Besprechungsanfrage Einheit Gebühr zugewiesen, die auf die Menge an Systemressourcen wie CPU und EA durch den Vorgang verbraucht proportionalen ist. Beim Lesen von ein Dokument 1 kB Sitzung Konsistenz verbraucht 1 Anforderung Einheit. Eine gelesene ist 1 RU unabhängig von der Anzahl der Elemente gespeichert oder die Anzahl der gleichzeitige Anforderungen gleichzeitig ausgeführt. Größere Dokumente erfordern höhere Anforderung Einheiten je nach Größe. Wenn Sie, die Größe der Ihre-Einheiten und die Anzahl der liest, die Sie für eine Anwendung unterstützen müssen wissen, können Sie den genauen Betrag des Durchsatzes erforderlich, für die Anwendung benötigt Lesen der Bereitstellung. 

Wenn DocumentDB Dokumente gespeichert sind, verteilt diese gleichmäßig zwischen Partitionen basierend auf den Schlüsselwert Partition. Der Durchsatz ist auch gleichmäßig verteilte zwischen den verfügbaren h. teilt den Durchsatz pro Partition = (Gesamtdurchsatz pro Websitesammlung) / (Anzahl der Partitionen). 

>[AZURE.NOTE] Um den vollständigen Durchsatz der Sammlung zu erreichen, müssen Sie eine Partitionsschlüssel auswählen, die Sie Besprechungsanfragen zwischen einer Anzahl Schlüsselwerte für unterschiedliche Partition gleichmäßig verteilen können.

## <a name="single-partition-and-partitioned-collections"></a>Einzelpartition und partitionierten Websitesammlungen
DocumentDB unterstützt die Erstellung einer Partition und partitionierten Websitesammlungen. 

- **Partitionierte Websitesammlungen** können mehrere Partitionen erstrecken und sehr große Mengen von Speicher und Durchsatz unterstützen. Geben Sie eine Partitionsschlüssel für die Websitesammlung ein.
- **Einzelnes-Scheibe Websitesammlungen** haben niedrigere Preisoptionen und die Möglichkeit, Abfragen und zum Ausführen von Transaktionen über alle aufgelisteten Daten. Sie haben die Skalierbarkeit und Speicher Begrenzung für eine einzelne Partition aus. Sie müssen nicht für diese Sammlungen Partitionsschlüssel angeben. 

![Partitionierte Websitesammlungen in DocumentDB][2] 

Für Szenarien, die nicht Speicher oder Durchsatz große Datenmengen benötigte, sind Einzelpartition Websitesammlungen gut. Beachten Sie, dass Single-Scheibe Websitesammlungen die Skalierbarkeit und Speichergrenzwerte für eine einzelne Partition, d. h. haben. bis zu 10 GB Speicher und bis zu 10.000 Anforderung Einheiten pro Sekunde. 

Partitionierte Websitesammlungen können sehr große Mengen von Speicher und Durchsatz unterstützen. Die standardmäßige Angebote werden jedoch so konfiguriert, dass bis zu 250 GB Speicher speichern und bis zu 250.000 Anforderung Einheiten pro Sekunde skalieren. Wenn Sie höhere Speicher oder Durchsatz pro Websitesammlung benötigen, wenden Sie sich an den [Azure-Support](documentdb-increase-limits.md) , um diese höhere für Ihr Konto haben.

In der folgenden Tabelle werden die Unterschiede in arbeiten mit einer einzigen-Scheibe und partitionierten Websitesammlungen aufgeführt:

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p></p></td>
            <td valign="top"><p><strong>Einzelpartition Websitesammlung</strong></p></td>
            <td valign="top"><p><strong>Partitionierte Websitesammlung</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>Partitionsschlüssel</p></td>
            <td valign="top"><p>Keine</p></td>
            <td valign="top"><p>Erforderlich</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Primärschlüssel für Dokument</p></td>
            <td valign="top"><p>"Id"</p></td>
            <td valign="top"><p>Zusammengesetzte Schlüssel &lt;Partitionsschlüssel&gt; und "Id"</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Mindestens Speicherplatz</p></td>
            <td valign="top"><p>0 GB</p></td>
            <td valign="top"><p>0 GB</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Maximaler Speicher</p></td>
            <td valign="top"><p>10 GB</p></td>
            <td valign="top"><p>Unbeschränkt (250 GB standardmäßig)</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Minimaler Durchsatz</p></td>
            <td valign="top"><p>400 Anfrage Einheiten pro Sekunde</p></td>
            <td valign="top"><p>10.000 Anforderung Einheiten pro Sekunde</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Maximale Durchsatz</p></td>
            <td valign="top"><p>10.000 Anforderung Einheiten pro Sekunde</p></td>
            <td valign="top"><p>Unbeschränkt (250.000 Anforderung Einheiten pro Sekunde standardmäßig)</p></td>
        </tr>
        <tr>
            <td valign="top"><p>API-Versionen</p></td>
            <td valign="top"><p>Alle</p></td>
            <td valign="top"><p>API 2015-12-16 und höher</p></td>
        </tr>
    </tbody>
</table>

## <a name="working-with-the-sdks"></a>Arbeiten mit den SDKs

Azure DocumentDB hinzugefügt Unterstützung für die automatische Partitionierung [REST](https://msdn.microsoft.com/library/azure/dn781481.aspx)API Version 2015-12-16. Partitionierte Websitesammlungen erstellen zu können, müssen Sie SDK Versionen 1.6.0 herunterladen oder höher in einem der unterstützten SDK Plattformen (.NET, Node.js, Java, Python). 

### <a name="creating-partitioned-collections"></a>Erstellen von partitionierten Sammlungen

Das folgende Beispiel zeigt einen Ausschnitt für .NET eine Auflistung zum Speichern von werden Gerätedaten 20.000 Anforderung Einheiten pro Sekunde Durchsatz zu erstellen. Das SDK legt den Wert OfferThroughput (der wiederum festlegt der `x-ms-offer-throughput` Anforderung Kopfzeile auf die REST-API). Legen wir die `/deviceId` als der Partitionsschlüssel. Die Wahl der Partitionsschlüssel wird zusammen mit dem Rest der Websitesammlung Metadaten wie Name und Indizierung Richtlinie gespeichert.

In diesem Beispiel wir ausgewählten `deviceId` da wir wissen, dass Buchstabe a da es eine große Anzahl von Geräten gibt, schreibt können partitionsübergreifend gleichmäßig verteilte sein und uns erlauben, die Datenbank aus, um große Datenmengen Aufnahme skalieren und (b) viele Anfragen wie das aktuelle Daten für ein Gerät abrufen, die auf einer einzelnen Geräte-ID ausgelegte sind und aus einem einzelnen-Abschnitt abgerufen werden können.

    DocumentClient client = new DocumentClient(new Uri(endpoint), authKey);
    await client.CreateDatabaseAsync(new Database { Id = "db" });

    // Collection for device telemetry. Here the JSON property deviceId will be used as the partition key to 
    // spread across partitions. Configured for 10K RU/s throughput and an indexing policy that supports 
    // sorting against any number or string property.
    DocumentCollection myCollection = new DocumentCollection();
    myCollection.Id = "coll";
    myCollection.PartitionKey.Paths.Add("/deviceId");

    await client.CreateDocumentCollectionAsync(
        UriFactory.CreateDatabaseUri("db"),
        myCollection,
        new RequestOptions { OfferThroughput = 20000 });
        

> [AZURE.NOTE] Partitionierte Websitesammlungen erstellen zu können, müssen Sie eine Durchsatzwert > 10.000 Anforderung Einheiten pro Sekunde angeben. Da Durchsatz ein Vielfaches von 100 ist, hat dies 10,100 oder höher sein.

Diese Methode ist ein REST-API zu DocumentDB anrufen, und der Dienst stellt eine Anzahl von Partitionen basierend auf den angeforderten Durchsatz bereit. Sie können den Durchsatz einer Websitesammlung ändern, wie Ihre Veranstaltung weiterentwickelt muss. Weitere Informationen hierzu finden Sie unter [Leistung Ebenen](documentdb-performance-levels.md) .

### <a name="reading-and-writing-documents"></a>Lesen und Schreiben von Dokumenten

Jetzt, lassen Sie uns Einfügen von Daten in DocumentDB. Eine Beispielklasse mit einem Gerät lesen, wird und es einen Anruf an CreateDocumentAsync zum Einfügen eines neuen Geräts in eine Auflistung gelesen werden.

    public class DeviceReading
    {
        [JsonProperty("id")]
        public string Id;

        [JsonProperty("deviceId")]
        public string DeviceId;

        [JsonConverter(typeof(IsoDateTimeConverter))]
        [JsonProperty("readingTime")]
        public DateTime ReadingTime;

        [JsonProperty("metricType")]
        public string MetricType;

        [JsonProperty("unit")]
        public string Unit;

        [JsonProperty("metricValue")]
        public double MetricValue;
      }

    // Create a document. Here the partition key is extracted as "XMS-0001" based on the collection definition
    await client.CreateDocumentAsync(
        UriFactory.CreateDocumentCollectionUri("db", "coll"),
        new DeviceReading
        {
            Id = "XMS-001-FE24C",
            DeviceId = "XMS-0001",
            MetricType = "Temperature",
            MetricValue = 105.00,
            Unit = "Fahrenheit",
            ReadingTime = DateTime.UtcNow
        });


Lassen Sie uns gelesen des Dokuments durch den Partitionsschlüssel und -Id, aktualisieren und löschen, klicken Sie dann im letzten Schritt, indem Partitionsschlüssel und Id. Beachten Sie, dass die liest eine PartitionKey einzuschließenden (entsprechend der `x-ms-documentdb-partitionkey` Anforderung Kopfzeile auf die REST-API).

    // Read document. Needs the partition key and the ID to be specified
    Document result = await client.ReadDocumentAsync(
      UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
      new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });

    DeviceReading reading = (DeviceReading)(dynamic)result;

    // Update the document. Partition key is not required, again extracted from the document
    reading.MetricValue = 104;
    reading.ReadingTime = DateTime.UtcNow;

    await client.ReplaceDocumentAsync(
      UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
      reading);

    // Delete document. Needs partition key
    await client.DeleteDocumentAsync(
      UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
      new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });



### <a name="querying-partitioned-collections"></a>Abfragen von partitionierten Websitesammlungen

Wenn Sie Daten in partitionierten Websitesammlungen Abfragen, leitet DocumentDB automatisch die Abfrage auf die Partition Schlüsselwerte im Filter angegeben ist (falls vorhanden) entspricht. Diese Abfrage wird beispielsweise nur der Partition, enthält die Partitionsschlüssel "XMS-0001" weitergeleitet.

    // Query using partition key
    IQueryable<DeviceReading> query = client.CreateDocumentQuery<DeviceReading>(
        UriFactory.CreateDocumentCollectionUri("db", "coll"))
        .Where(m => m.MetricType == "Temperature" && m.DeviceId == "XMS-0001");

Die folgende Abfrage verfügt nicht über einen Filter für die Partitionsschlüssel (Geräte-ID), und wo er für den Index der des ausgeführt wird auf alle aufgefächerte ist. Beachten Sie, dass Sie zum Angeben der EnableCrossPartitionQuery (`x-ms-documentdb-query-enablecrosspartition` in die REST-API) auf das SDK zum Ausführen einer Abfrage partitionsübergreifend haben.

    // Query across partition keys
    IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
        UriFactory.CreateDocumentCollectionUri("db", "coll"), 
        new FeedOptions { EnableCrossPartitionQuery = true })
        .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100);

### <a name="parallel-query-execution"></a>Abfragen parallel auszuführen

Die DocumentDB SDKs 1.9.0 und über parallele abfrageausführung Supportoptionen, denen Sie niedrig Wartezeit Abfragen in partitionierten Websitesammlungen, auch bei der benötigten, tippen Sie auf eine große Anzahl von Partitionen ausführen können. Die folgende Abfrage wird beispielsweise so konfiguriert, dass partitionsübergreifend parallel ausgeführt werden.

    // Cross-partition Order By Queries
    IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
        UriFactory.CreateDocumentCollectionUri("db", "coll"), 
        new FeedOptions { EnableCrossPartitionQuery = true, MaxDegreeOfParallelism = 10, MaxBufferedItemCount = 100})
        .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100)
        .OrderBy(m => m.MetricValue);

Sie können Abfragen parallel auszuführen verwalten, indem Sie die folgenden Parameter zu optimieren:

- Durch das Festlegen von `MaxDegreeOfParallelism`, Sie können den Grad der Parallelität, d. h., die maximale Anzahl von gleichzeitigem Netzwerk Verbindungen auf die Auflistung des steuern. Wenn Sie dies festlegen-1, wird der Grad der Parallelität vom SDK verwaltet. Wenn die `MaxDegreeOfParallelism` ist nicht angegebenen oder festlegen auf 0 (null) der Standardwert ist, eine einzelne Netzwerkverbindung auf der Auflistung werden.
- Durch das Festlegen von `MaxBufferedItemCount`, Sie können Handel deaktivieren Abfrage Wartezeit und Client Seite arbeitsspeichernutzung. Wenn Sie diesen Parameter, oder legen Sie diese-1, wird die Anzahl von Elementen, die während der parallele Abfrage gepuffert vom SDK verwaltet.

Bezug auf den gleichen Status der Sammlung, wird eine parallele Abfrage in derselben Reihenfolge wie serielle Ausführung Ergebnisse zurück. Beim Durchführen einer Cross-Scheibe-Abfrage, die sortieren (ORDER BY und/oder oben) enthält, wird das DocumentDB SDK Probleme mit der Abfrage parallel partitionsübergreifend und teilweise sortierte Suchergebnisse in den clientseitigen Global Ziffern oder Buchstaben sortierte Ergebnisse zusammengeführt.

### <a name="executing-stored-procedures"></a>Ausführen von gespeicherten Prozeduren

Sie können auch atomare Transaktionen anhand von Dokumenten mit der gleichen Geräte-ID, z. B. ausführen, wenn Sie Aggregate oder den aktuellen Status eines Geräts in ein einzelnes Dokument verwalten. 

    await client.ExecuteStoredProcedureAsync<DeviceReading>(
        UriFactory.CreateStoredProcedureUri("db", "coll", "SetLatestStateAcrossReadings"),
        new RequestOptions { PartitionKey = new PartitionKey("XMS-001") }, 
        "XMS-001-FE24C");

Im nächsten Abschnitt Prüfen wie partitionierten Sammlungen aus einer Partition Sammlungen verschoben werden können.

<a name="migrating-from-single-partition"></a>
### <a name="migrating-from-single-partition-to-partitioned-collections"></a>Migrieren von einer Partition zu partitionierten Websitesammlungen
Wenn eine Anwendung, die eine einzelne-Scheibe Websitesammlung mit höheren Durchsatz benötigt (> 10.000 RU/s) oder größere Datenspeicher (> 10GB), mithilfe des [Migrationstools für DocumentDB Daten](http://www.microsoft.com/downloads/details.aspx?FamilyID=cda7703a-2774-4c07-adcc-ad02ddc1a44d) können Sie die Daten aus der Sammlung einzeln-Scheibe partitionierten-Auflistung migrieren. 

Migrieren von eine Auflistung einer Partition zu einer partitionierten Websitesammlung

1. Exportieren von Daten aus der Sammlung von einer Partition auf JSON. Weitere Details finden Sie unter [in JSON-Datei exportieren](documentdb-import-data.md#export-to-json-file) .
2. Importieren Sie die Daten in einer partitionierten Websitesammlung erstellt mit Key-Definition einer Indexpartition und mehr als 10.000 Anforderung Einheiten Datendurchsatz pro Sekunde, wie im folgenden Beispiel dargestellt. Weitere Details finden Sie unter [Import DocumentDB](documentdb-import-data.md#DocumentDBSeqTarget) .

![Migrieren von Daten zu einer Websitesammlung partitionierte in DocumentDB][3]  

>[AZURE.TIP] Erwägen Sie für schnelleres importieren erhöhen die Anzahl der parallele Anfragen auf 100 oder höher zum Nutzen der höheren Durchsatz für partitionierte Websitesammlungen zur Verfügung. 

Jetzt, da wir die Grundlagen abgeschlossen haben, sehen wir uns einige wichtige Entwurfsaspekte beim Arbeiten mit Partition Tasten in DocumentDB.

## <a name="designing-for-partitioning"></a>Entwerfen für die Partitionierung
Die Wahl der Partitionsschlüssel ist eine wichtige Entscheidung, die Sie zur Entwurfszeit machen müssen. In diesem Abschnitt werden einige der Nachteile verbindet Partitionsschlüssel für Ihre Websitesammlung auswählen.

### <a name="partition-key-as-the-transaction-boundary"></a>Partitionsschlüssel als Transaktionsgrenze
Verwendeter Partitionsschlüssel sollten die Notwendigkeit, aktivieren die Verwendung der Transaktionen mit der Anforderung zum Verteilen von Ihre-Einheiten auf mehrere Partition Schlüssel, um sicherzustellen, dass eine skalierbare Lösung abstimmen. Eine extrem könnten Sie die gleichen Partitionsschlüssel für alle Dokumente festlegen, aber dies kann der Skalierbarkeit Ihre Lösung einschränken. Bei dem anderen extrem konnte Sie für jedes Dokument einen eindeutigen Partitionsschlüssel zuweisen, die wäre hochgradig skalierbare aber würde verhindern, dass Sie Cross Dokument Transaktionen über gespeicherten Prozeduren und Triggern verwenden. Eine ideale Partitionsschlüssel ist eine, mit dem Sie effiziente Abfragen verwenden und nicht über ausreichend Kardinalität, um sicherzustellen, dass Ihre Lösung skalierbar ist. 

### <a name="avoiding-storage-and-performance-bottlenecks"></a>Vermeiden von Engpässen Speicherplatz und Leistung 
Es ist es wichtig, wählen Sie eine Eigenschaft aus die schreibt über eine Anzahl eindeutiger Werte verteilt werden können. Anfragen an der gleichen Partitionsschlüssel den Durchsatz einer einzelnen Partition können nicht überschreiten, und gedrosselt werden sollen. Daher ist es wichtig, Partitionsschlüssel auszuwählen, die nicht zu **"Hotspot"** in Ihrer Anwendung führt. 10 GB kann im Speicher auch nicht die Gesamtspeichergröße für Dokumente mit der gleichen Partitionsschlüssel überschreiten. 

### <a name="examples-of-good-partition-keys"></a>Beispiele für gute Partition Schlüssel
Hier sind einige Beispiele für das Auswählen der Partitionsschlüssel für eine Anwendung wie ein:

* Wenn Sie einem Benutzer Profil Back-End-implementieren sind, ist die Benutzer-ID für Partitionsschlüssel eine gute Wahl.
* Wenn Sie IoT Daten z. B. Gerät Zustand gespeichert ist, ist eine Geräte-ID eine gute Wahl für Partitionsschlüssel aus.
* Wenn Sie für die Protokollierung Time-Serie Daten DocumentDB verwenden, ist die ID Hostname oder Prozesse eine gute Wahl für Partitionsschlüssel.
* Wenn Sie eine Architektur mit mehreren Mandanten haben, ist die ID des Mandanten eine gute Wahl für Partitionsschlüssel aus.

Beachten Sie, dass in einigen Fällen verwenden (wie etwa die IoT und Benutzerprofilen oben beschriebenen) der Partitionsschlüssel Ihre-Id (Dokumentschlüssel) identisch sein muss. In anderen wie der Reihe Zeitdaten müssen Sie Partitionsschlüssel aus, die die Id unterscheidet.

### <a name="partitioning-and-loggingtime-series-data"></a>Vorherigen und Protokollierung/Zeit-Reihe von Daten
Einer der am häufigsten verwendeten verwenden Fälle von DocumentDB ist für die Protokollierung und werden. Es ist wichtig, ein guter Partitionsschlüssel auswählen, da Sie große Datenmengen schreibgeschützt müssen möglicherweise. Die Option wird abhängig von Ihrer lesen und Schreiben Sie Sätze und Arten von Abfragen, die Sie nicht vorhaben, ausführen. Hier sind einige Tipps zum Auswählen einer guten Partitionsschlüssel ein.

- Wenn Ihr Fall verwenden umfasst eine kleine Zinsfuß schreibt Acculumating über längere Zeit und müssen Abfrage durch Bereiche für Zeitstempel und andere Filter, dann verwenden z. B. einen Rollup von der Zeitstempel Datum als Partitionsschlüssel ein guter Ansatz ist. Dadurch Abfrage über alle Daten für ein Datum aus einem einzelnen-Abschnitt. 
- Ist Ihre Arbeitsbelastung fett schreiben, die im Allgemeinen häufiger ist, sollten Sie eine Partitionsschlüssel verwenden, die nicht auf Timestamp basieren, sodass DocumentDB schreibt gleichmäßig auf eine Anzahl von Partitionen verteilen kann. Hier ist ein Hostname, Prozess-ID, Aktivität-ID oder eine andere Eigenschaft mit hoher Kardinalität eine gute Wahl. 
- Eine dritte Methode ist ein Hybrid eine Stelle, an der Sie mehrere Websitesammlungen, eine für jeden Tag/Monat und der Partitionsschlüssel ist eine detaillierte Eigenschaft wie Hostname. Dies hat den Vorteil, den unterschiedliche Performance Ebenen des Zeitfensters ausgehend von Ihnen festgelegt werden können, z. B. die Auflistung für den aktuellen Monat wird nach der Bereitstellung mit höheren Durchsatz, da sie Lese- und schreibt, dient während der vorherigen Monate mit Durchsatz verringern, da sie nur liest dienen.

### <a name="partitioning-and-multi-tenancy"></a>Partitionierung und mehrere Mandanten
Wenn Sie eine mit mehreren Mandanten Anwendung mit DocumentDB implementieren, gibt es zwei Bereiche Muster zum Implementieren Mandanten mit DocumentDB – eine Partitionsschlüssel pro Mandant und eine Websitesammlung pro Mandant. Hier sind die vor- und Nachteile für jede aus:

* Eine Partitionsschlüssel pro Mandant: In diesem Modell Mandanten in einer einzigen Sammlung zusammengestellt sind. Aber Abfragen und fügt für Dokumente in einem einzelnen Mandanten anhand einer Einzelpartition ausgeführt werden können. Sie können auch Transaktionen Logik allen Dokumenten in einem Mandanten implementieren. Da mehrerer Mandanten eine Websitesammlung freigeben möchten, können Sie Speicherplatz und Durchsatz Kosten mit Ressourcen für den Mandanten innerhalb einer einzelnen Auflistung Verbindungspooling statt zusätzliche Spielraum für jeden Mandanten bereitgestellt speichern. Der Nachteil besteht darin, dass Sie nicht die Leistung Isolation pro Mandant verfügen. Performance-Durchsatz erhöht gelten für die gesamte Websitesammlung im Vergleich mit einer zielgerichteten erhöht sich für den Mandanten.
* Eine Websitesammlung pro Mandant: jede Mandanten verfügt über eine eigene-Auflistung. In diesem Modell können Sie die Leistung pro Mandant reservieren. Dieses Modell ist mit DocumentDBs neue Energieverbrauchs Preisgestaltung Modell kostengünstiger für mehrere Mandanten Applikationen mit einer kleinen Anzahl von Mandanten.

Sie können auch einen Kombination/gestuft Ansatz verwenden, der kleine Mandanten collocates und größere Mandanten eigene Auflistung migriert.

## <a name="next-steps"></a>Nächste Schritte
Wir haben in diesem Artikel beschrieben, wie vorherigen arbeitet in Azure DocumentDB, wie Sie partitionierte Websitesammlungen erstellen können und wie Sie eine gute Partitionsschlüssel für eine Anwendung auswählen können. 

-   Durchführen Sie Maßstab und Performance-Tests mit DocumentDB. Ein Beispiel finden Sie unter [Leistung und Skala mit Azure DocumentDB testen](documentdb-performance-testing.md) .
-   Erste Schritte mit den [SDKs](documentdb-sdk-dotnet.md) oder die [REST-API](https://msdn.microsoft.com/library/azure/dn781481.aspx) Codierung
-   Erfahren Sie mehr über [bereitgestellte Durchsatz in DocumentDB](documentdb-performance-levels.md)
-   Wenn Sie anpassen, wie Ihre Anwendung durchführt partitionieren möchten, können Sie eine eigene clientseitige vorherigen Implementierung anschließen. [Clientseitige Partitionierung Support](documentdb-sharding.md)finden Sie unter.

[1]: ./media/documentdb-partition-data/partitioning.png
[2]: ./media/documentdb-partition-data/single-and-partitioned.png
[3]: ./media/documentdb-partition-data/documentdb-migration-partitioned-collection.png  

 

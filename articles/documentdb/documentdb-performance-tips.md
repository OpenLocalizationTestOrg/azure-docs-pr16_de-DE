<properties 
    pageTitle="DocumentDB Leistungstipps | Microsoft Azure" 
    description="Erfahren Sie, Client-Konfigurationsoptionen für optimale Leistung mit DocumentDB Azure-Datenbank"
    keywords="wie für optimale Leistung der Datenbank"
    services="documentdb" 
    authors="mimig1" 
    manager="jhubbard" 
    editor="" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/17/2016" 
    ms.author="mimig"/>

# <a name="performance-tips-for-documentdb"></a>Leistungstipps für DocumentDB

Azure DocumentDB ist eine schnelle und flexible verteilte Datenbank, die nahtlos mit garantiert Wartezeit und Durchsatz skaliert. Sie müssen nicht Bereiche Architektur vorzunehmen oder Schreiben von komplexen Code, um Ihre Datenbank mit DocumentDB skalieren. Nach oben oder unten skalieren ist so einfach wie das Bereitstellen einer einzelnen API-Anruf oder [SDK-Methode aufrufen](documentdb-performance-levels.md#changing-performance-levels-using-the-net-sdk). Da DocumentDB Netzwerk Anrufe zugegriffen wird gibt es jedoch clientseitige Optimierungen an, die Sie vornehmen können, um optimale Leistung zu erzielen.

Wenn Sie gefragt werden, sind "wie kann ich meine Datenbank Leistung können?" Beachten Sie die folgenden Optionen aus:

## <a name="networking"></a>Netzwerke
<a id="direct-connection"></a>

1. **Verbindungsrichtlinie: direkte Verbindung-Modus verwenden**
    
    Wie ein Client und Azure DocumentDB besteht wirkt sich auf wichtige auf die Leistung, insbesondere im Hinblick auf beobachtete clientseitige Wartezeit. Stehen zwei Konfiguration Schlüssel Einstellungen für das Konfigurieren von Client-Verbindungsrichtlinie – die Verbindung *Modus* und die [Verbindung *Protokoll*](#connection-protocol)zur Verfügung.  Die zwei verfügbaren Modi sind:

    1. Gateway-Modus (Standard)
    2. Direkten Modus

    Da DocumentDB ein Speichersystem verteilt, DocumentDB Ressourcen ist wie Websitesammlungen in vielen Computern aufgeteilt werden und jede Partition hohen Verfügbarkeit repliziert. Die physische Adressübersetzung logische wird in eine routing-Tabelle gespeichert, die auch als eine Ressource intern verfügbar ist.

    Führen Sie im Modus "Gateway" die DocumentDB Gateway-Computer dieses routing, wodurch einfache und compact sein Client-Code ein. Eine Clientanwendung Probleme Besprechungsanfragen für die DocumentDB Gateway Maschinen, die logischen URI in der Besprechungsanfrage auf die physische Adresse des Back-End-Knotens übersetzen, und die Anforderung ordnungsgemäß weiterleiten.  Umgekehrt in Direktmodus müssen Clients verwalten – regelmäßig aktualisieren eine Kopie dieser Tabelle routing und dann direkt in die Back-End-DocumentDB Knoten verbinden.

    Gateway-Modus wird auf allen SDK Plattformen unterstützt und konfiguriert ist.  Wenn eine Anwendung innerhalb eines Unternehmensnetzwerks mit Einschränkungen strict Firewall ausgeführt, wird Gateway-Modus die beste Option dar, da es den HTTPS-Standardport und einen einzelnen Endpunkt verwendet. Der Leistung Nachteil ist jedoch, dass das Gateway Modus einen Abschnitt zusätzliche Netzwerk beinhaltet jedes Mal, wenn Daten zu lesen oder zu DocumentDB geschrieben.   Aus diesem Grund bietet Direktmodus eine bessere Leistung aufgrund weniger Netzwerk Abschnitte.

2. **Verbindungsrichtlinie: Verwenden Sie das TCP-Protokoll**

    Beim Direktmodus nutzen zu können, gibt es zwei Protokolloptionen zur Verfügung:

    - TCP
    - HTTPS

    DocumentDB bietet eine einfache und Rest Modell Programmierung über HTTPS zu öffnen. Darüber hinaus bietet es eine effiziente TCP-Protokoll, das ist auch RESTful in deren Kommunikationsmodell und über den Client .NET SDK verfügbar ist. Direkte TCP und HTTPS verwenden Sie SSL für die erste Authentifizierung und verschlüsseln den Datenverkehr an. Verwenden Sie das TCP-Protokoll nach Möglichkeit zur Optimierung der Systemleistung. 

    Bei Verwendung von TCP in Gateway-Modus TCP Port 443 ist den Port DocumentDB und 10250 den Port MongoDB-API. Wenn TCP im Direktmodus sowie die Ports Gateway verwenden zu können, müssen Sie den Port sicherstellen Bereich zwischen 10000 und 20000 geöffnet ist, da DocumentDB dynamische TCP-Ports verwendet. Wenn diese Ports nicht geöffnet sind und Sie versuchen, TCP verwenden, erhalten Sie einen Fehler mit 503 Dienst nicht verfügbar. 

    Connectivity-Modus wird bei der Erstellung der DocumentClient-Instanz mit dem Parameter ConnectionPolicy konfiguriert. Wenn Direktmodus verwendet wird, kann das Protokoll auch innerhalb des Parameters ConnectionPolicy festgelegt werden.

        var serviceEndpoint = new Uri("https://contoso.documents.net");
        var authKey = new "your authKey from Azure Mngt Portal";
        DocumentClient client = new DocumentClient(serviceEndpoint, authKey, 
        new ConnectionPolicy
        {
            ConnectionMode = ConnectionMode.Direct,
            ConnectionProtocol = Protocol.Tcp
        });

    Da TCP nur in den Direktmodus unterstützt wird, wenn Gateway-Modus verwendet wird, klicken Sie dann das HTTPS-Protokoll wird immer verwendet zur Kommunikation mit dem Gateway und der Protokollwert in der ConnectionPolicy wird ignoriert.

    ![Abbildung der Richtlinie DocumentDB Verbindung](./media/documentdb-performance-tips/azure-documentdb-connection-policy.png)

3. **Rufen Sie OpenAsync zur Vermeidung von Wartezeit beim Starten, bei der ersten Anforderung**

    Standardmäßig wird die erste Anforderung eine höhere Wartezeit enthalten, da es die Adresse routing-Tabelle abgerufen werden sollen. Um diese Wartezeit beim Starten, bei der ersten Anforderung zu vermeiden, sollten Sie wie folgt OpenAsync() einmal bei der Initialisierung aufrufen.

        await client.OpenAsync();
<a id="same-region"></a>
4. **Zusammengestellte Clients in derselben Azure Region für Leistung**

    Platzieren Sie nach Möglichkeit, alle Applikationen aufrufen DocumentDB in derselben Region als DocumentDB-Datenbank. Für einen ungefähren Vergleich, führen Sie Anrufe an DocumentDB innerhalb derselben Region innerhalb von 1-2 ms, aber die Wartezeit zwischen den Westen und Nord in den USA ist > 50 ms. Diese Wartezeit kann wahrscheinlich Anforderung zu Anforderung abweichen, je nach der von der Anforderung geöffnet wird, wenn er im Desktopclient auf die Begrenzungslinie Azure Datacenter bewegt weiterleiten. Die niedrigste mögliche Wartezeit erfolgt durch um sicherzustellen, dass die Anwendung einen innerhalb der gleichen Azure Region als bereitgestellte DocumentDB Endpunkt befindet. Eine Liste der verfügbaren Regionen finden Sie unter [Azure Regionen](https://azure.microsoft.com/regions/#services).

    ![Abbildung der Richtlinie DocumentDB Verbindung](./media/documentdb-performance-tips/azure-documentdb-same-region.png)
<a id="increase-threads"></a>
5. **Erhöhen der Anzahl von Threads/Aufgaben**

    Da Anrufe an DocumentDB über das Netzwerk vorgenommen werden, müssen Sie möglicherweise den Grad an Parallelität Ihrer Anfragen unterschiedlich sein, damit die Clientanwendung nur wenig Zeit zwischen Anfragen warten aufwenden. Angenommen, Sie verwenden. Netz des [Task Parallel Library](https://msdn.microsoft.com//library/dd460717.aspx), in der Reihenfolge der 100 s Aufgaben lesen und Schreiben in DocumentDB erstellen.

## <a name="sdk-usage"></a>SDK Verwendung

1. **Installieren Sie das neueste SDK**

    DocumentDB SDKs sind ständig verbessert werden, um die optimale Leistung bereitstellen. Finden Sie in den zum Ermitteln des letzten SDKS und verbesserte [DocumentDB SDK](documentdb-sdk-dotnet.md) . 

2. **Verwenden Sie einen einzelne DocumentDB-Client für die Gültigkeitsdauer Ihrer Anwendung**
  
    Beachten Sie, dass jede Instanz DocumentClient Thread-sicher ist und effizient Verbindung Management und Adresse Zwischenspeichern beim Betrieb im Direktmodus ausgeführt werden. Um effiziente Verbindung Management und bessere Leistung durch DocumentClient ermöglichen, empfiehlt es sich eine Instanz des DocumentClient pro AppDomain für die Dauer der Anwendung verwendet werden soll.
<a id="max-connection"></a>
3. **Vergrößern von System.Net MaxConnections pro host**

    DocumentDB Besprechungsanfragen standardmäßig über HTTPS/REST vorgenommen wurden, und pro Hostname oder IP-Adresse für die standardmäßige Verbindung maximale untersucht werden. Möglicherweise müssen Sie die MaxConnections einen höheren Wert (100-1000) festlegen, damit die Clientbibliothek mehrere gleichzeitige Verbindungen mit DocumentDB nutzen kann. Im .NET SDK 1.8.0 oberhalb der Standardwert für [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx) ist 50 und um den Wert zu ändern, können Sie die [Documents.Client.ConnectionPolicy.MaxConnectionLimit](https://msdn.microsoft.com/en-us/library/azure/microsoft.azure.documents.client.connectionpolicy.maxconnectionlimit.aspx) auf einen höheren Wert festlegen.  

4. **Parallele Abfragen für partitionierte Websitesammlungen optimieren**

     DocumentDB .NET SDK Version 1.9.0 und über parallele Support-Abfragen, die Ihnen eine partitionierte Sammlung parallel abgefragt ermöglichen (finden Sie unter [Arbeiten mit den SDKs](documentdb-partition-data.md#working-with-the-sdks) und die zugehörigen [Codebeispielen](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/Queries/Program.cs) für Weitere Informationen). Parallele Abfragen dienen zur Verbesserung der Abfragewartezeit und Durchsatz über deren serielle Gegenstück. Parallele Abfragen bieten zwei Parameter, die Benutzer optimieren können, um ihren Anforderungen, Buchstabe a MaxDegreeOfParallelism benutzerdefiniert anpassen: die maximale Anzahl von Partitionen steuern als Parallel und (b) MaxBufferedItemCount abgefragt werden kann: die Anzahl der vorab abgerufenen Ergebnisse zu steuern. 
    
    Buchstabe a ***Optimieren MaxDegreeOfParallelism\: *** 
    parallele Abfrage funktioniert durch Abfragen von mehreren Partitionen parallel. Daten aus einer einzelnen partitionierten sammeln werden jedoch seriell in Bezug auf die Abfrage abgerufen werden. Ja, wirkt sich das Festlegen der MaxDegreeOfParallelism auf die Anzahl der Partitionen die maximale Wahrscheinlichkeit, dass die meisten leistungsfähigen Abfrage erreichen vorausgesetzt, alle anderen System Bedingungen unverändert bleiben. Wenn Sie nicht, dass die Anzahl der Partitionen wissen, können Sie die MaxDegreeOfParallelism einen hohen Wert festlegen, und das System wird der Minimalwert (Anzahl der Partitionen, die vom Benutzer gemäß Eingabe) als die MaxDegreeOfParallelism wählen. 
    
    Es ist zu beachten, dass parallele Abfragen die beste Vorteile erzeugen, wenn die Daten in Bezug auf die Abfrage alle partitionsübergreifend gleichmäßig verteilt ist. Wenn die Sammlung partitionierte so, dass alle oder eine Mehrzahl von einer Abfrage zurückgegebenen Daten in wenigen Partitionen (eine Partition in ungünstigsten Fall) konzentrierte ist, und klicken Sie dann die Leistung der Abfrage durch diese Partitionen Engpässe werden für würde konfiguriert ist. 
    
    Buchstabe b ***Optimieren MaxBufferedItemCount\: *** 
    parallele Abfrage sollen Ergebnisse abzurufen vorab während der aktuelle Stapel von Ergebnissen vom Client verarbeitet wird. Die vorab abgerufen werden kann, im Wartezeit generellen einer Abfrage. MaxBufferedItemCount ist den Parameter, um den Umfang der vorab abgerufenen Ergebnisse beschränken. Festlegen von MaxBufferedItemCount an die erwarteten Ergebnisse zurückgegeben (oder eine höhere Zahl) ermöglicht die Abfrage erhalten maximale Vorteile von vorab abgerufen werden. 
    
    Beachten Sie, dass funktioniert auf gleiche Weise ohne Rücksicht auf das MaxDegreeOfParallelism vorab abgerufen werden, und es ein Puffer für die Daten aus allen Partitionen wird.  

5. **Aktivieren von serverseitigen globalen Katalog**
    
    Verringern der Häufigkeit von Garbagecollection kann in einigen Fällen hilfreich sein. Setzen Sie in .NET [GcServer](https://msdn.microsoft.com/library/ms229357.aspx) auf True.

6. **Implementieren Backoff in RetryAfter Abständen**
 
    Während des Testens Leistung, sollten Sie laden erhöhen, bis eine kleine Zinsfuß Anfragen gedrosselt abrufen. Wenn gedrosselt, sollte die Clientanwendung Backoff auf Steuerung für das Intervall Server festgelegten "Wiederholen" aus. Der Backoff zu beachten: Damit ist sichergestellt, dass Sie minimale Wartezeiten zwischen Wiederholungsversuche Dauer für die. "Wiederholen" Richtlinie Support enthalten, in der Version 1.8.0 und höher ist der DocumentDB [.NET](documentdb-sdk-dotnet.md) und [Java](documentdb-sdk-java.md)und Version 1.9.0 und oberhalb der [Node.js](documentdb-sdk-node.md) und [Python](documentdb-sdk-python.md). Weitere Informationen finden Sie unter [reservierte Durchsatz Exceeding beschränkt](documentdb-request-units.md#exceeding-reserved-throughput-limits) und [RetryAfter](https://msdn.microsoft.com/library/microsoft.azure.documents.documentclientexception.retryafter.aspx).

7. **Ihre Client-Arbeitsbelastung zu skalieren**

    Wenn Sie auf hohen Durchsatz Ebenen testen (> 50.000 RU/s), die Clientanwendung möglicherweise der Engpass aufgrund von maschinellen Cappings heraus auf CPU oder Netzwerk Auslastung. Wenn Sie diesen Punkt erreicht haben, können Sie weiterhin das DocumentDB-Konto Pushbenachrichtigungen durch die Skalierung der Clientanwendungen auf mehrere Server.

8. **Cache Dokument URIs für unteren gelesen Wartezeit**

    Cache Dokument URIs möglichst optimale Leistung finden Sie hier.
<a id="tune-page-size"></a>
9. **Diagrammen Sie die Seitengröße für Abfragen/gelesen-Feeds für eine bessere Leistung von**

    Bei der Durchführung einer Massen lesen Sie Dokumente mithilfe von gelesen feed Funktionalität (d. h., ReadDocumentFeedAsync), oder wenn eine DocumentDB SQL-Abfrage ausgegeben wird, werden die Ergebnisse in einer Weise segmentierter zurückgegeben, wenn das Ergebnis zu groß ist. Standardmäßig werden die Ergebnisse in Blöcken mit 100 Elemente oder 1 MB zurückgegeben, abhängig von dem Limit ist ersten Treffer. 

    Zum Verringern der Anzahl der Netzwerkroundtrips erforderlich, um alle ergänzenden Ergebnisse abzurufen, können Sie die Größe einer Seite mit X-ms-Max--Elementanzahl Anforderung Kopfzeile auf bis zu 1000 erhöhen. In Fällen, in denen müssen Sie nur ein paar Ergebnisse anzuzeigen z. B., wenn Ihre Benutzer-Benutzeroberfläche oder einer Anwendung API nur 10 gibt Suchergebnissen nacheinander, Sie können auch die Seitengröße auf 10 zum Verringern des Durchsatzes für Lese- und Abfragen verbraucht verringern.

    Sie können auch die Größe der Seite mit den verfügbaren DocumentDB SDKs festlegen.  Beispiel:
    
        IQueryable<dynamic> authorResults = client.CreateDocumentQuery(documentCollection.SelfLink, "SELECT p.Author FROM Pages p WHERE p.Title = 'About Seattle'", new FeedOptions { MaxItemCount = 1000 });

10. **Erhöhen der Anzahl von Threads/Aufgaben**

    Finden Sie im Abschnitt Netzwerke [erhöhen Sie die Anzahl der Threads/Aufgaben](#increase-threads) aus.

## <a name="indexing-policy"></a>Richtlinie Indizierung

1. **Verwenden Sie die verzögerte Indizierung für schnellere Höchstwert Aufnahme Tarife**

    DocumentDB können Sie eine Indizierung Richtlinie, – Ebene der Websitesammlung – angeben, die wodurch Sie auswählen, ob Sie die Dokumente in einer Websitesammlung oder nicht automatisch indiziert werden sollen.  Darüber hinaus können Sie auch zwischen synchroner (konsistent) und asynchrone Aktualisierung von Indizes (Lazy) auswählen. Standardmäßig wird der Index synchron auf jedem einfügen, ersetzen oder Löschen eines Dokuments in der Auflistung aktualisiert. Modus können synchron die Abfragen an der gleichen [Ebene Konsistenz](documentdb-consistency-levels.md) wie des Dokuments lautet ohne Verzögerung für den Index, um den "aktuellen Stand" berücksichtigt.
    
    Verzögerte Indizierung für Szenarien, in denen Daten in Bursts geschrieben werden, angesehen werden kann, und die erforderliche Arbeit zum Indizieren von Inhalten über längere Zeit amortisieren werden soll. Verzögerte Indizierung können Sie Ihre bereitgestellten Durchsatz effektiv verwenden und Schreibanfragen Höchstwert Vorkommen mit minimalen Wartezeiten dienen. Es ist wichtig, jedoch, beachten Sie, dass beim verzögerte Indizierung aktiviert ist, Abfrageergebnisse Gelegenheit konsistent unabhängig von der Konsistenz Ebene so konfiguriert, dass für das Konto DocumentDB genommen werden.

    Daher budgetgerecht konsistenten Indizierung Modus (IndexingPolicy.IndexingMode auf konsistent festgelegt ist) die höchsten Anforderung Einheit Gebühr pro schreiben, während der Indizierung Modus (IndexingPolicy.IndexingMode auf Lazy festgelegt ist) und keine Indizierung Lazy (IndexingPolicy.Automatic auf False festgelegt ist) Null Indizierung Kosten zum Zeitpunkt der schreiben haben.

2. **Schließen Sie nicht verwendete Pfade für schnellere schreibt Indizierung aus**

    Indizierung des DocumentDB-Richtlinie können Sie, um anzugeben, welche Wege Dokument ein- oder Ausschließen von Indizierung durch die Nutzung von Indizierung Pfade (IndexingPolicy.IncludedPaths und IndexingPolicy.ExcludedPaths). Die Verwendung von Indizierung Pfade kann schreiben verbesserte Leistung und unteren Index-Speicher für Szenarios, in denen die Abfragemuster im Voraus bekannt sind, anbieten, wie Indizierung Kosten direkt auf die Anzahl der eindeutigen Pfade indiziert in Beziehung gesetzt werden.  Zeigt beispielsweise der folgende Code einer gesamten Abschnitts der Dokumente (auch bekannt als ausschließen eine Unterstruktur) aus Indizierung mithilfe der "*" Platzhalterzeichen.

        var collection = new DocumentCollection { Id = "excludedPathCollection" };
        collection.IndexingPolicy.IncludedPaths.Add(new IncludedPath { Path = "/*" });
        collection.IndexingPolicy.ExcludedPaths.Add(new ExcludedPath { Path = "/nonIndexedContent/*");
        collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), excluded);

    Weitere Informationen finden Sie unter [DocumentDB Indizierung Richtlinien](documentdb-indexing-policies.md).

## <a name="throughput"></a>Durchsatz
<a id="measure-rus"></a>

1. **Messen und Abstimmung für unteren Anforderung Einheiten/Sekunde Verwendung**

    DocumentDB bietet eine umfangreiche Menge von Datenbankvorgänge einschließlich relationale und die hierarchische Abfragen mit UDFs, gespeicherten Prozeduren und Triggern – aller Geschäftsjahre Dokumente in einer Datenbanksammlung. Die Kosten für jede dieser Operationen variieren basierend auf der CPU, EA und Arbeitsspeicher zum Abschließen des Vorgangs erforderlich. Statt Gedanken und Hardware-Ressourcen verwaltet werden können Sie einer Besprechungsanfrage Einheit (RU) als ein einzelnes Measure für verschiedene Datenbankvorgänge durchführen und eine Anwendung Anforderung service erforderlichen Ressourcen vorstellen.

    [Anfordern von Einheiten](documentdb-request-units.md) werden nach der Bereitstellung für jedes Datenbankkonto anhand der Anzahl von Leistungseinheiten, die Sie erwerben. Anforderung Einheitenverbrauch wird als ein Wert pro Sekunde ausgewertet. Programme, die die Anforderung bereitgestellte Einheit Rate überschreiten für ihr Konto beschränkt ist, bis die Rate unterhalb der reservierte Ebene für das Konto fällt. Wenn die Anwendung eine höhere Ebene des Durchsatzes erforderlich ist, können Sie zusätzliche Leistungseinheiten kaufen.

    Die Komplexität der Abfrage wirkt sich auf, wie viele anfordern Einheiten für einen Vorgang verbraucht werden. Die Anzahl der Prädikate, Art der Prädikate, Anzahl der UDFs und die Größe der Datenmenge Quelle alle beeinflussen die Kosten von Vorgängen Abfrage ein.

    Messen, in den Aufwand für jeden Vorgang (erstellen, aktualisieren oder löschen), prüfen Sie die X-ms-Anforderung-Gebühr Kopfzeile (oder die entsprechende RequestCharge-Eigenschaft in ResourceResponse<T> oder FeedResponse<T> im .NET SDK) messen, die Anzahl der Anfrage Einheiten, die von diesen Operationen verbraucht.

        // Measure the performance (request units) of writes
        ResourceResponse<Document> response = await client.CreateDocumentAsync(collectionSelfLink, myDocument);
        Console.WriteLine("Insert of document consumed {0} request units", response.RequestCharge);
        // Measure the performance (request units) of queries
        IDocumentQuery<dynamic> queryable = client.CreateDocumentQuery(collectionSelfLink, queryString).AsDocumentQuery();
        while (queryable.HasMoreResults)
             {
                  FeedResponse<dynamic> queryResponse = await queryable.ExecuteNextAsync<dynamic>();
                  Console.WriteLine("Query batch consumed {0} request units", queryResponse.RequestCharge);
             }
        
    Die Anfrage Gebühr zurückgegeben, die in diese Kopfzeile wird Dezimalbruch der Ihrer bereitgestellte Durchsatz (d. h., 2000 RUs / Sekunde). Angenommen, die obige Abfrage 1000 1 KB Dokumente zurück, sein die Kosten des Vorgangs 1000. Daher berücksichtigt nur zwei solche Anfragen innerhalb einer Sekunde der Server vor dem nachfolgende Anforderungen begrenzungsebene nach. Weitere Informationen finden Sie unter [Anfrage Einheiten](documentdb-request-units.md) und die [Anfrage Einheit Taschenrechner](https://www.documentdb.com/capacityplanner).

2. **Behandeln von Zins einschränken Nachfrage/Rate zu groß**

    Wenn ein Client versucht, den reservierten Durchsatz für ein Konto überschreiten, gibt es keine Leistungsabfall auf dem Server und ohne Verwendung von Durchsatzkapazität jenseits der reservierte Ebene. Der Server präventiv beenden die Anfrage mit RequestRateTooLarge (HTTP-Statuscode 429) und die X-ms-"Wiederholen"-nach-ms Kopfzeile, der angibt, der Zeitdauer in Millisekunden an, die der Benutzer warten muss, vor dem erneuten der Anfrage zurückzukehren.
 
        HTTP Status 429,
        Status Line: RequestRateTooLarge
        x-ms-retry-after-ms :100

    Die SDKs implizit alle diese Antwort abzufangen, berücksichtigen die Server festgelegten "Wiederholen" nach Kopfzeile und wiederholen Sie die Anforderung. Es sei denn, Ihr Konto gleichzeitig von mehreren Clients zugegriffen wird, ist die nächste Wiederholung erfolgreich.

    Wenn Sie mehr als einen Client hoch Betrieb konsistent über die Anforderungsrate haben, kann die Anzahl der Standardeinstellung "Wiederholen" derzeit eingestellt, 9 intern vom Client nicht ausreichend. In diesem Fall löst der Client eine DocumentClientException mit Statuscode 429 zur Anwendung. Die Anzahl der Standardeinstellung "Wiederholen" kann durch Festlegen der RetryOptions auf die Instanz ConnectionPolicy geändert werden. Standardmäßig ist die DocumentClientException mit Statuscode 429 nach einer kumulierte Wartezeit von 30 Sekunden zurückgegeben, wenn die Anfrage weiterhin über die Anforderungsrate ausgeführt werden. In diesem Fall auch, wenn kleiner als die Anzahl der max "Wiederholen" die Anzahl der aktuellen "Wiederholen" ist, werden sie den Standardwert von 9 oder einen benutzerdefinierten Wert.

    Während das automatisierte "Wiederholen" Verhalten um Stabilität und Nutzbarkeit für die meisten Applikationen zu verbessern hindert, können in odds nützlich, wenn-Benchmarktests ausführen, insbesondere dann, wenn Wartezeit messen.  Die Wartezeit Client beobachtet werden kurzzeitigen Anstieg Wenn der Versuch der Einschränkung Server Treffer und bewirkt, dass das Client-SDK im Hintergrund zu wiederholen. Zur Vermeidung von Wartezeit Spitzen während der Leistung Versuche, messen Sie die Gebühr zurückgegeben, die für jeden Vorgang und sicherzustellen Sie, dass Anfragen unterhalb der Rate reservierte Anfragen ausgeführt werden. Weitere Informationen finden Sie unter [Anfordern von Einheiten](documentdb-request-units.md).
   
3. **Entwurf für kleinere Dokumente für höheren Durchsatz**

    Die Anforderung Gebühr (d. h. Verarbeitung von Besprechungsanfragen Kosten) für einen bestimmten Vorgang wird direkt auf die Größe des Dokuments in Beziehung gesetzt. Vorgänge für große Dokumente Kosten mit mehr als Vorgänge für kleine Dokumente.

## <a name="consistency-levels"></a>Konsistenz Ebenen

1. **Verwenden Sie für eine bessere gelesen Wartezeiten schwächere Konsistenz Ebenen**

    Ein weiterer wichtiger Faktor zu berücksichtigen, während die Leistung der Anwendung DocumentDB optimieren Konsistenz Ebene befindet. Die Wahl der Konsistenz Ebene wirkt sich auf Leistung zum sowohl lesen und schreiben. Sie können die Konsistenz Standardstufe für die Datenbankkonto konfigurieren und die ausgewählten Konsistenz Ebene dann gilt für allen Sammlungen des (über alle Datenbanken) innerhalb des DocumentDB-Kontos. Auch mithilfe von Join-Schreiboperationen wird der Einfluss der Änderung Konsistenz Ebene als Anforderung Wartezeit beobachtet. Als sicherere Konsistenz Ebenen verwendet werden, werden die Wartezeiten vergrößern. Andererseits, wird der Einfluss der Konsistenz Ebene zu Vorgängen finden Sie hier im Hinblick auf Durchsatz beobachtet werden. Schwächere Konsistenz, die Ebenen höhere zulassen lesen Durchsatz zum vom Client realisiert werden.

    Standardmäßig werden die Konsistenz-Standardstufe angegeben haben, klicken Sie auf das Datenbankkonto alle Lese- und Abfragen in den benutzerdefinierten Ressourcen verwendet werden. Sie können, jedoch die Ebene Konsistenz Anforderung einer bestimmten gelesen/Abfrage senken, indem Sie die Kopfzeile X-ms-Konsistenz-Ebene Anforderung angeben. Weitere Informationen finden Sie unter [Konsistenz Ebenen in DocumentDB](documentdb-consistency-levels.md).

## <a name="next-steps"></a>Nächste Schritte

Eine Beispiel-Anwendung verwendet, um DocumentDB für leistungsfähige Szenarien auf wenigen Clientcomputern ausgewertet werden soll finden Sie unter [Performance und Skalierung mit Azure DocumentDB testen](documentdb-performance-testing.md).

Siehe auch, um weitere Informationen zu Entwurf Ihrer Anwendung für Skalierung und high Performance [Partitioning und dieselbe Skalierung in Azure DocumentDB](documentdb-partition-data.md)ein.

<properties 
    pageTitle="DocumentDB skalieren und Testen der Leistung | Microsoft Azure" 
    description="Erfahren Sie, wie Maßstab und Performance-Tests mit Azure DocumentDB ausführen"
    keywords="Testen der Leistung"
    services="documentdb" 
    authors="arramac" 
    manager="jhubbard" 
    editor="" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/27/2016" 
    ms.author="arramac"/>

# <a name="performance-and-scale-testing-with-azure-documentdb"></a>Leistung und Skala mit DocumentDB Azure testen
Leistung und Maßstab testen ist ein wichtiger Schritt in der Anwendungsentwicklung. Für viele Clientanwendungen die Datenbankebene wirkt sich deutlich auf die Leistung und Skalierbarkeit, und ist deshalb eine wichtige Komponente der Leistung zu testen. [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) speziell für flexible Skalierung und vorhersehbar Performance ist und daher passt hervorragend für Applikationen, die eine Stufe leistungsfähige Datenbank benötigen. 

Dieser Artikel bietet einen Verweis für Entwickler implementieren Leistung Test-Suites für ihre DocumentDB Auslastung oder eine DocumentDB für leistungsfähige Anwendungsszenarien auswerten. Er konzentriert sich hauptsächlich auf isoliert Leistung Testen der Datenbank, sondern auch enthält bewährte Methoden für die Herstellung Applications.

Nach dem Lesen dieses Artikels, werden Sie die folgenden Fragen beantworten:   

- Wo finde ich eine Beispiel für .NET-Clientanwendung für Testen der Leistung von Azure DocumentDB? 
- Wie erreichen ich hohen Durchsatz Ebenen mit Azure DocumentDB aus meiner Clientanwendung?

Um mit Code anzufangen, Herunterladen von [DocumentDB Leistung testen Stichprobe](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark)des Projekts bitte. 

> [AZURE.NOTE] Das Ziel dieser Anwendung besteht darin bewährte Methoden zum Extrahieren von DocumentDB eine bessere Leistung mit einer kleinen Anzahl von Clientcomputern zu veranschaulichen. Dies wurde nicht über ausreichend Kapazität Höchstwert des Diensts, veranschaulichen, die limitlessly skaliert werden kann.

Wenn Sie für clientseitige Konfigurationsoptionen zur Verbesserung der Systemleistung DocumentDB gefunden haben, finden Sie unter [DocumentDB Leistungstipps](documentdb-performance-tips.md).

## <a name="run-the-performance-testing-application"></a>Führen Sie die Leistung testen Anwendung
Die schnellste Möglichkeit, um anzufangen ist so kompilieren und führen das folgenden Beispiel .NET wie in den folgenden Schritten beschrieben. Auch können Sie den Quellcode überprüfen und ähnliche Konfigurationen für Ihre eigenen-Clientanwendungen implementieren.

**Schritt 1:** Herunterladen des Projekts aus [DocumentDB Leistung testen Stichprobe](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark)oder Github Repository Verzweigung.

**Schritt 2:** Ändern der Einstellungen für EndpointUrl, AuthorizationKey, CollectionThroughput und DocumentTemplate in App.config (optional).

> [AZURE.NOTE] Finden Sie vor der Bereitstellung von Websitesammlungen mit hoher Durchsatz auf der [Seite Preise](https://azure.microsoft.com/pricing/details/documentdb/) Schätzung die Kosten pro Websitesammlung. DocumentDB Rechnung Speicher und Durchsatz unabhängig voneinander auf stündlich aus, damit Sie Kosten durch Löschen oder herabsetzen des Durchsatzes der DocumentDB Sammlungen nach dem Testen speichern können.

**Schritt 3:** Kompilieren Sie und führen Sie die app Console über die Befehlszeile. Folgende Ausgabe sollte angezeigt werden:

    Summary:
    ---------------------------------------------------------------------
    Endpoint: https://docdb-scale-demo.documents.azure.com:443/
    Collection : db.testdata at 50000 request units per second
    Document Template*: Player.json
    Degree of parallelism*: 500
    ---------------------------------------------------------------------

    DocumentDBBenchmark starting...
    Creating database db
    Creating collection testdata
    Creating metric collection metrics
    Retrying after sleeping for 00:03:34.1720000
    Starting Inserts with 500 tasks
    Inserted 661 docs @ 656 writes/s, 6860 RU/s (18B max monthly 1KB reads)
    Inserted 6505 docs @ 2668 writes/s, 27962 RU/s (72B max monthly 1KB reads)
    Inserted 11756 docs @ 3240 writes/s, 33957 RU/s (88B max monthly 1KB reads)
    Inserted 17076 docs @ 3590 writes/s, 37627 RU/s (98B max monthly 1KB reads)
    Inserted 22106 docs @ 3748 writes/s, 39281 RU/s (102B max monthly 1KB reads)
    Inserted 28430 docs @ 3902 writes/s, 40897 RU/s (106B max monthly 1KB reads)
    Inserted 33492 docs @ 3928 writes/s, 41168 RU/s (107B max monthly 1KB reads)
    Inserted 38392 docs @ 3963 writes/s, 41528 RU/s (108B max monthly 1KB reads)
    Inserted 43371 docs @ 4012 writes/s, 42051 RU/s (109B max monthly 1KB reads)
    Inserted 48477 docs @ 4035 writes/s, 42282 RU/s (110B max monthly 1KB reads)
    Inserted 53845 docs @ 4088 writes/s, 42845 RU/s (111B max monthly 1KB reads)
    Inserted 59267 docs @ 4138 writes/s, 43364 RU/s (112B max monthly 1KB reads)
    Inserted 64703 docs @ 4197 writes/s, 43981 RU/s (114B max monthly 1KB reads)
    Inserted 70428 docs @ 4216 writes/s, 44181 RU/s (115B max monthly 1KB reads)
    Inserted 75868 docs @ 4247 writes/s, 44505 RU/s (115B max monthly 1KB reads)
    Inserted 81571 docs @ 4280 writes/s, 44852 RU/s (116B max monthly 1KB reads)
    Inserted 86271 docs @ 4273 writes/s, 44783 RU/s (116B max monthly 1KB reads)
    Inserted 91993 docs @ 4299 writes/s, 45056 RU/s (117B max monthly 1KB reads)
    Inserted 97469 docs @ 4292 writes/s, 44984 RU/s (117B max monthly 1KB reads)
    Inserted 99736 docs @ 4192 writes/s, 43930 RU/s (114B max monthly 1KB reads)
    Inserted 99997 docs @ 4013 writes/s, 42051 RU/s (109B max monthly 1KB reads)
    Inserted 100000 docs @ 3846 writes/s, 40304 RU/s (104B max monthly 1KB reads)

    Summary:
    ---------------------------------------------------------------------
    Inserted 100000 docs @ 3834 writes/s, 40180 RU/s (104B max monthly 1KB reads)
    ---------------------------------------------------------------------
    DocumentDBBenchmark completed successfully.


**Schritt 4 (bei Bedarf):** Der Durchsatz gemeldet (RU/s) auf das Tool sollte identisch sein oder höher als der bereitgestellte Durchsatz der Sammlung. Wenn dies nicht der Fall ist, erhöhen die DegreeOfParallelism in kleinen Schritten kann Ihnen die Beschränkung erreicht haben. Wenn der Durchsatz über Ihre app Client plateaus, hilft ebenso mehrerer Instanzen von der app auf die gleichen oder einem anderen Computer über die verschiedenen Instanzen der bereitgestellte Wert erreicht Ihnen. Wenn Sie diesen Schritt Hilfe benötigen, Schreiben Sie eine e-Mail an askdocdb@microsoft.com oder diese ausfüllt Support-Ticket.

Nachdem Sie die app ausgeführt haben, können Sie versuchen, unterschiedliche [Richtlinien Indizierung](documentdb-indexing-policies.md) und [Konsistenz Ebenen](documentdb-consistency-levels.md) , um deren Einfluss auf die Durchsatz und Wartezeit zu verstehen. Auch können Sie den Quellcode überprüfen und ähnliche Konfigurationen vornehmen, um eigene Test-Suites oder Herstellung Applikationen implementieren.

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel untersucht wie Leistung und Testen mit DocumentDB mit einem .NET Console-app-Skala ausführen können. Lizenzinformationen finden Sie die Links unter Weitere Informationen zum Arbeiten mit DocumentDB.

* [DocumentDB Leistung testen Stichprobe](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark)
* [Client-Konfigurationsoptionen zur Verbesserung der Systemleistung DocumentDB](documentdb-performance-tips.md)
* [Serverseitige Partitionierung in DocumentDB](documentdb-partition-data.md)
* [DocumentDB Websitesammlungen und Leistung Ebenen](documentdb-performance-levels.md)
* [DocumentDB .NET SDK-Dokumentation auf MSDN](https://msdn.microsoft.com/library/azure/dn948556.aspx)
* [Beispiele für .NET DocumentDB](https://github.com/Azure/azure-documentdb-net)
* [DocumentDB Blog auf Performance-Tipps](https://azure.microsoft.com/blog/2015/01/20/performance-tips-for-azure-documentdb-part-1-2/)

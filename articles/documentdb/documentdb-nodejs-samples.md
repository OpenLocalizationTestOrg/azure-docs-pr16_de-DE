<properties
    pageTitle="NoSQL Node.js Beispiele für DocumentDB | Microsoft Azure"
    description="Suchen nach NoSQL Node.js Beispiele auf Github für häufige Aufgaben in DocumentDB, einschließlich der Vorgänge für JSON-Dokumente in nachgeforscht."
    keywords="Node.js-Beispiele"
    services="documentdb"
    authors="moderakh"
    manager="jhubbard"
    editor="monicar"
    documentationCenter="nodejs"/>

<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="moderakh"/>


# <a name="documentdb-nodejs-examples"></a>DocumentDB Node.js-Beispiele

> [AZURE.SELECTOR]
- [Beispiele für .NET](documentdb-dotnet-samples.md)
- [Node.js-Beispiele](documentdb-nodejs-samples.md)
- [Python Beispiele](documentdb-python-samples.md)
- [Azure Code Sample Gallery](https://azure.microsoft.com/documentation/samples/?service=documentdb)

Beispiel-Lösungen, die Vorgänge und andere allgemeine Vorgänge Azure DocumentDB Ressourcen ausführen sind in der [Azure-Documentdb-Nodejs](https://github.com/Azure/azure-documentdb-node/tree/master/samples) GitHub Repository enthalten. Dieser Artikel enthält:

- Links zu den Aufgaben in den einzelnen Projektdateien Node.js Beispiel.
- Links zu verwandten API Inhalt verwiesen werden.

**Erforderliche Komponenten**

1. In diesen Beispielen wird Node.js ein Azure-Konto benötigen:
    - Sie können [ein Azure-Konto kostenlos öffnen](https://azure.microsoft.com/pricing/free-trial/): Abrufen von Gutschriften können Sie kostenpflichtiges Azure Services ausprobieren und sogar nachdem sie es gewohnt sind bis können Sie das Konto behalten und Verwendung frei Azure Dienste, wie z. B. Websites. Ihre Kreditkarte wird nie belastet, sofern Sie explizit der Einstellungen ändern, und bitten um in Rechnung gestellt.
   - Sie können die [Vorteile für Visual Studio Abonnenten aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/): Ihr Visual Studio-Abonnement bietet Ihnen Gutschriften jeden Monat, die Sie für kostenpflichtiges Azure-Dienste verwenden können.
2. Sie benötigen ferner [Node.js SDK](documentdb-sdk-node.md).

    > [AZURE.NOTE] Jedes Beispiel ist eigenständig, sondern richtet sich selbst und bereinigt nach sich selbst. Die Beispiele stellen als solche mehrere Anrufe an [DocumentClient.createCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createCollection)aus. Jedes Mal, wenn dies Ihr Abonnement funktioniert Abrechnung wird für 1 Stunde bei der Verwendung pro der Leistung Ebene der Websitesammlung erstellt wird.

## <a name="database-examples"></a>Beispiele für die Datenbank

Die Datei [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/DatabaseManagement/app.js) des Projekts [Datenbank-Management](https://github.com/Azure/azure-documentdb-node/tree/master/samples/DatabaseManagement) wird gezeigt, wie die folgenden Aufgaben ausführen.

Aufgabe | -API-Referenz
--- | ---
[Erstellen einer Datenbank](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DatabaseManagement/app.js#L121-L131) | [DocumentClient.createDatabase](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createDatabase)
[Abfrage ein Konto für eine Datenbank](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DatabaseManagement/app.js#L146-L171) | [DocumentClient.queryDatabases](http://azure.github.io/azure-documentdb-node/DocumentClient.html#queryDatabases)
[Lesen einer Datenbank nach Id](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DatabaseManagement/app.js#L89-L99) | [DocumentClient.readDatabase](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readDatabase)
[Liste der Datenbanken für ein Konto](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DatabaseManagement/app.js#L111-L119) | [DocumentClient.readDatabases](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readDatabase)
[Löschen einer Datenbank](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DatabaseManagement/app.js#L133-L144) | [DocumentClient.deleteDatabase](http://azure.github.io/azure-documentdb-node/DocumentClient.html#deleteDatabase)

## <a name="collection-examples"></a>Beispiele für die Websitesammlung

Die Datei [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/CollectionManagement/app.js) des Projekts [CollectionManagement](https://github.com/Azure/azure-documentdb-node/tree/master/samples/CollectionManagement) wird gezeigt, wie die folgenden Aufgaben ausführen.

Aufgabe | -API-Referenz
--- | ---
[Erstellen einer Websitesammlung](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L97-L118) | [DocumentClient.createCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createCollection)
[Lesen Sie eine Liste aller Sammlungen in einer Datenbank](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L120-L130) | [DocumentClient.readCollections](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readCollections)
[Abrufen einer Auflistung von _self](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L132-L141) | [DocumentClient.readCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readCollection)
[Abrufen einer Auflistung von-Id](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L143-L156) | [DocumentClient.readCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readCollection)
[Erste Ebene der Leistung einer Websitesammlung](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L158-L186) | [DocumentQueryable.queryOffers](http://azure.github.io/azure-documentdb-node/DocumentClient.html#queryOffers)
[Ändern der Leistung Ebene einer Websitesammlung](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L188-L202) | [DocumentClient.replaceOffer](http://azure.github.io/azure-documentdb-node/DocumentClient.html#replaceOffer)
[Löschen einer Websitesammlungs](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L204-L215) | [DocumentClient.deleteCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#deleteCollection)

## <a name="document-examples"></a>Beispiele für Dokument

Die Datei [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/DocumentManagement/app.js) des Projekts [DocumentManagement](https://github.com/Azure/azure-documentdb-node/tree/master/samples/DocumentManagement) wird gezeigt, wie die folgenden Aufgaben ausführen.

Aufgabe | -API-Referenz
--- | ---
[Erstellen von Dokumenten](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L153-L177) | [DocumentClient.createDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createDocument)
[Lesen Sie das Dokument-Feeds für eine Websitesammlung](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L179-L189) | [DocumentClient.readDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readDocument)
[Lesen eines Dokuments durch ID](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L191-L201) | [DocumentClient.readDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readDocument)
[Dokument lesen, nur, wenn Dokument geändert hat](https://github.com/Azure/azure-documentdb-node/blob/0778eadea7abb2af41e8c22a239dc872c584f421/samples/DocumentManagement/app.js#L79-L107) | [DocumentClient.readDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readDocument)<br/>[RequestOptions.accessCondition](http://azure.github.io/azure-documentdb-node/global.html#RequestOptions)
[Abfrage für Dokumente](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L82-L110) | [DocumentClient.queryDocuments](http://azure.github.io/azure-documentdb-node/DocumentClient.html#queryDocuments)
[Ersetzen eines Dokuments](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L112-L119) |  [DocumentClient.replaceDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#replaceDocument)
[Dokument mit bedingten ETag Kontrollkästchen ersetzen](https://github.com/Azure/azure-documentdb-node/blob/0778eadea7abb2af41e8c22a239dc872c584f421/samples/DocumentManagement/app.js#L147-L164) |  [DocumentClient.replaceDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#replaceDocument)<br/>[RequestOptions.accessCondition](http://azure.github.io/azure-documentdb-node/global.html#RequestOptions)
[Löschen eines Dokuments](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L122-L133) | [DocumentClient.deleteDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#deleteDocument)

## <a name="indexing-examples"></a>Beispiele für Indizierung

Die Datei [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/IndexManagement/app.js) des Projekts [IndexManagement](https://github.com/Azure/azure-documentdb-node/tree/master/samples/IndexManagement) wird gezeigt, wie die folgenden Aufgaben ausführen.

Aufgabe | -API-Referenz
--- | ---
Erstellen einer Websitesammlung mit standardmäßigen Indizierung | [DocumentClient.createDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html)
[Manuelles indizieren Sie ein bestimmtes Dokument](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L185-L238) | [IndexingDirective: 'Einfügen'](http://azure.github.io/azure-documentdb-node/global.html#indexingDirective)
[Schließen Sie ein bestimmtes Dokument manuell aus dem index](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L120-L183) | [RequestOptions.indexingDirective](http://azure.github.io/azure-documentdb-node/global.html#RequestOptions)
[Verwenden Sie die verzögerte Indizierung für Massenimport oder Lesen Sie beanspruchen Websitesammlungen](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L240-L269) | [IndexingMode.Lazy](http://azure.github.io/azure-documentdb-node/global.html#IndexingMode)
[Einschließen von bestimmter Pfade eines Dokuments in Indizierung](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L433-L444) | [IndexingPolicy.IncludedPaths](http://azure.github.io/azure-documentdb-node/global.html#IndexingPolicy)
[Ausschließen von bestimmter Pfade aus Indizierung](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L427-L450) | [ExcludedPath](http://azure.github.io/azure-documentdb-node/global.html#IndexingPolicy)
[Eine Überprüfung auf eine Zeichenfolge Weg während eines Vorgangs Bereich zulassen](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L271-L347)| [ExcludedPath.EnableScanInQuery](http://azure.github.io/azure-documentdb-node/global.html#FeedOptions)
[Erstellen Sie einen Bereich Index auf einer Zeichenfolge](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L349-L425) | [DocumentClient.queryDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#queryDocument)
[Erstellen einer Websitesammlung mit standardmäßigen IndexPolicy, und klicken Sie dann diese online aktualisieren](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L519-L614) | [DocumentClient.createCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createCollection)<br> [DocumentClient.replaceCollection#replaceCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html)

Weitere Informationen zur Indizierung finden Sie unter [DocumentDB Indizierung Richtlinien](documentdb-indexing-policies.md).

## <a name="server-side-programming-examples"></a>Beispiele für die serverseitige Programmierung

Die Datei [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/ServerSideScripts/app.js) des Projekts [ServerSideScripts](https://github.com/Azure/azure-documentdb-node/tree/master/samples/ServerSideScripts) wird gezeigt, wie die folgenden Aufgaben ausführen.

Aufgabe | -API-Referenz
--- | ---
[Erstellen Sie eine gespeicherte Prozedur](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.ServerSideScripts/app.js#L44-L71) | [DocumentClient.createStoredProcedure](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createStoredProcedure)
[Ausführen einer gespeicherten Prozedur](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.ServerSideScripts/app.js#L73-L90) | [DocumentClient.executeStoredProcedure](http://azure.github.io/azure-documentdb-node/DocumentClient.html#executeStoredProcedure)

Weitere Informationen zu serverseitige Programmierung, finden Sie unter [DocumentDB serverseitige Programmierung: gespeicherte Prozeduren, Datenbanktrigger und UDFs](documentdb-programming.md).

## <a name="partitioning-examples"></a>Vorherigen Beispielen

Die Datei [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/Partitioning/app.js) des Projekts [Partitioning](https://github.com/Azure/azure-documentdb-node/tree/master/samples/Partitioning) wird gezeigt, wie die folgenden Aufgaben ausführen.

Aufgabe | -API-Referenz
--- | ---
[Verwenden einer HashPartitionResolver](https://github.com/Azure/azure-documentdb-node/blob/ce0fc3c4e70b0279091a1e03620a668d93a14fc2/samples/Partitioning/app.js#L53-L103) | [HashPartitionResolver](http://azure.github.io/azure-documentdb-node/HashPartitionResolver.html)

Weitere Informationen über das Aufteilen von Daten in DocumentDB finden Sie unter [Partition und Skalieren von Daten in DocumentDB](documentdb-partition-data.md).

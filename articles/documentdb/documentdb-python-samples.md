<properties 
    pageTitle="NoSQL Python Beispiele für DocumentDB | Microsoft Azure" 
    description="Suchen nach NoSQL Python Beispiele auf Github für häufige Aufgaben in DocumentDB, einschließlich der Vorgänge für JSON-Dokumente in nachgeforscht." 
    keywords="Python Beispiele"
    services="documentdb" 
    authors="moderakh" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter="python"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="04/18/2016" 
    ms.author="moderakh"/>


# <a name="documentdb-python-examples"></a>Beispiele für DocumentDB Python

> [AZURE.SELECTOR]
- [Beispiele für .NET](documentdb-dotnet-samples.md)
- [Node.js-Beispiele](documentdb-nodejs-samples.md)
- [Python Beispiele](documentdb-python-samples.md)
- [Azure Code Sample Gallery](https://azure.microsoft.com/documentation/samples/?service=documentdb)

Beispiel-Lösungen, die Vorgänge und andere allgemeine Vorgänge Azure DocumentDB Ressourcen ausführen sind in der [Azure-Documentdb-Python](https://github.com/Azure/azure-documentdb-python/tree/master/samples) GitHub Repository enthalten. Dieser Artikel enthält:

- Links zu den Aufgaben in jedem der Python Beispiel Projektdateien. 
- Links zu verwandten API Inhalt verwiesen werden.

**Erforderliche Komponenten**

1. In diesen Beispielen werden Python ein Azure-Konto benötigen:
    - Sie können [ein Azure-Konto kostenlos öffnen](https://azure.microsoft.com/pricing/free-trial/): Abrufen von Gutschriften können Sie kostenpflichtiges Azure Services ausprobieren und sogar nachdem sie es gewohnt sind bis können Sie das Konto behalten und Verwendung frei Azure Dienste, wie z. B. Websites. Ihre Kreditkarte wird nie belastet, sofern Sie explizit der Einstellungen für ändern und festlegen, dass Sie in Rechnung gestellt.
   - Sie können die [Vorteile für Visual Studio Abonnenten aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/): Ihr Visual Studio-Abonnement bietet Ihnen Gutschriften jeden Monat, die Sie für kostenpflichtiges Azure-Dienste verwenden können.
2. Sie benötigen ferner das [Python SDK](documentdb-sdk-python.md). 

    > [AZURE.NOTE] Jedes Beispiel ist eigenständig, sondern richtet sich selbst und bereinigt nach sich selbst. Die Beispiele stellen als solche mehrere Anrufe an [Document_client aus. CreateCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html). Jedes Mal, wenn dies Ihr Abonnement funktioniert Abrechnung wird für 1 Stunde bei der Verwendung pro der Leistung Ebene der Websitesammlung erstellt wird. 

## <a name="database-examples"></a>Beispiele für die Datenbank

Die Datei [Program.py](https://github.com/Azure/azure-documentdb-python/tree/master/samples/DatabaseManagement/Program.py) des Projekts [Datenbank-Management](https://github.com/Azure/azure-documentdb-python/tree/master/samples/DatabaseManagement) wird gezeigt, wie die folgenden Aufgaben ausführen.

Aufgabe | -API-Referenz
--- | ---
[Erstellen einer Datenbank](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L65-L76) | [Document_client. CreateDatabase](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)
[Abfrage ein Konto für eine Datenbank](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L49-L62) | [Document_client. QueryDatabases](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)
[Lesen einer Datenbank nach Id](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L79-L96) | [Document_client. Schreibgeschützte](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)
[Liste der Datenbanken für ein Konto](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L99-L110) | [Document_client. ReadDatabases](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)
[Löschen einer Datenbank](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L113-L126) | [Document_client. DeleteDatabase](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)

## <a name="collection-examples"></a>Beispiele für die Websitesammlung 

Die Datei [Program.py](https://github.com/Azure/azure-documentdb-python/tree/master/samples/CollectionManagement/Program.py) des Projekts [CollectionManagement](https://github.com/Azure/azure-documentdb-python/tree/master/samples/CollectionManagement) wird gezeigt, wie die folgenden Aufgaben ausführen.

Aufgabe | -API-Referenz
--- | ---
[Erstellen einer Websitesammlung](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L84-L135) | [Document_client. CreateCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[Lesen Sie eine Liste aller Sammlungen in einer Datenbank](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L198-L225) | [Document_client. ListCollections](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[Abrufen einer Auflistung von-Id](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L178-L195) | [Document_client. ReadCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[Erste Ebene der Leistung einer Websitesammlung](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L139-L161) | [DocumentQueryable.QueryOffers](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[Ändern der Leistung Ebene einer Websitesammlung](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L163-L175) | [Document_client. ReplaceOffer](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[Löschen einer Websitesammlungs](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L212-L225) | [Document_client. DeleteCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)

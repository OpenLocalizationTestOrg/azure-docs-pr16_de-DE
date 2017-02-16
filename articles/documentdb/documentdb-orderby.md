<properties 
    pageTitle="Sortieren von DocumentDB Daten mit Order By | Microsoft Azure" 
    description="Erfahren Sie, wie in DocumentDB Abfragen in LINQ und SQL ORDER BY verwenden und wie eine Indizierung Richtlinie für ORDER BY-Abfragen angegeben." 
    services="documentdb" 
    authors="arramac" 
    manager="jhubbard" 
    editor="cgronlun" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/03/2016" 
    ms.author="arramac"/>

# <a name="sorting-documentdb-data-using-order-by"></a>Sortieren von DocumentDB Daten mit Order By
Microsoft Azure DocumentDB unterstützt Abfragen mithilfe von SQL über JSON-Dokumente Dokumente. Verwenden die ORDER BY-Klausel in SQL-Abfrage-Anweisungen können die Abfrageergebnisse sortiert werden.

Nach dem Lesen dieses Artikels, können Sie die folgenden Fragen beantworten ausführen: 

- Wie Abfrage ich mit Order By?
- Wie konfigurieren kann ich eine Indizierung Richtlinie für Order By?
- Was wird als Nächstes behandelt?

Außerdem werden [Beispiele](#samples) und [häufig gestellte Fragen zu](#faq) bereitgestellt.

Eine vollständige Referenz zu SQL-Abfragen finden Sie im [Lernprogramm DocumentDB Abfrage](documentdb-sql-query.md).

## <a name="how-to-query-with-order-by"></a>Vorgehensweise zum Abfragen mit Order By
Wie können in ANSI-SQL Sie nun eine optionale Order By-Klausel in SQL-Anweisungen einbeziehen, wenn DocumentDB Abfragen. Die-Klausel kann ein optionales ASC/DESC-Argument, um die Reihenfolge festzulegen, in der die Ergebnisse abgerufen werden müssen, enthalten. 

### <a name="ordering-using-sql"></a>Sortierung mit SQL
So sieht beispielsweise eine Abfrage zum Abrufen der Top 10-Bücher in absteigender Reihenfolge von deren Titel aus. 

    SELECT TOP 10 * 
    FROM Books 
    ORDER BY Books.Title DESC

### <a name="ordering-using-sql-with-filtering"></a>Verwenden von SQL mit Filterung Sortierung
Sie können die Reihenfolge mithilfe einer beliebigen geschachtelten Eigenschaft innerhalb von Dokumenten wie Books.ShippingDetails.Weight, und geben Sie zusätzliche Filter in die WHERE-Klausel in Kombination mit Order By wie in diesem Beispiel:

    SELECT * 
    FROM Books 
    WHERE Books.SalePrice > 4000
    ORDER BY Books.ShippingDetails.Weight

### <a name="ordering-using-the-linq-provider-for-net"></a>Sortierung mit dem LINQ-Anbieter für .NET
Verwenden von .NET SDK, Version 1.2.0 und höher verwenden, können Sie auch die OrderBy() oder OrderByDescending()-Klausel in LINQ-Abfragen, wie in diesem Beispiel:

    foreach (Book book in client.CreateDocumentQuery<Book>(UriFactory.CreateDocumentCollectionUri("db", "books"))
        .OrderBy(b => b.PublishTimestamp)
        .Take(100))
    {
        // Iterate through books
    }

DocumentDB unterstützt Sortierung mit einem einzelnen numerischen, Zeichenfolge oder boolesche Eigenschaft pro Abfrage mit zusätzlichen Abfragetypen in Kürze verfügbar. Finden Sie unter [Was kommt als Nächstes](#Whats_coming_next) , weitere Details.

## <a name="configure-an-indexing-policy-for-order-by"></a>Konfigurieren einer Richtlinie Indizierung für die Order By

Denken Sie daran, dass DocumentDB zwei Arten von Indizes (Hash und Bereich) unterstützt, die für bestimmte Pfade von Eigenschaften, Datentypen (Zeichenfolgen/Zahlen) und bei anderen Genauigkeitswerte (maximale Genauigkeit oder einen Wert unveränderlicher Genauigkeit) festgelegt werden können. Da DocumentDB Hash Indizierung als Standard verwendet, müssen Sie eine neue Websitesammlung mit einer benutzerdefinierten Indizierung Richtlinie mit Bereich auf Zahlen, Zeichenfolgen oder beides erstellen, um die Order By verwenden. 

>[AZURE.NOTE] Zeichenfolge Bereich Indizes wurden auf 7 Juli 2015 REST API Version 2015-06-03 eingeführt werden. Richtlinien für die Order By gegen Zeichenfolgen zu erstellen, müssen Sie SDK Version 1.2.0 der .NET SDK oder Version 1.1.0 der Python, Node.js oder Java SDK verwenden.
>
>Vor dem REST-API Version 2015-06-03 wurde die Indizierung Websitesammlung Standardrichtlinie Hash für Zeichenfolgen und Zahlen. Dies hat zu hashende Zeichenfolgen und den Bereich für Zahlen geändert wurden. 

Weitere Informationen hierzu finden Sie unter [DocumentDB Indizierung Richtlinien](documentdb-indexing-policies.md).

### <a name="indexing-for-order-by-against-all-properties"></a>Indizierung für Order By gegen alle Eigenschaften
So wie können Erstellen einer Websitesammlung mit "Alle Bereich" Indizierung für Order By gegen sämtliche numerische oder Zeichenfolge Eigenschaften, die in JSON-Dokumente darin angezeigt werden. Hier überschreiben wir die Index-Standardart für Zeichenfolgenwerte in einen Bereich, und klicken Sie auf die maximale Genauigkeit (-1).
                   
    DocumentCollection books = new DocumentCollection();
    books.Id = "books";
    books.IndexingPolicy = new IndexingPolicy(new RangeIndex(DataType.String) { Precision = -1 });
    
    await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), books);  

>[AZURE.NOTE] Beachten Sie, dass Order By nur Ergebnisse der Datentypen (Zeichenfolge und Nummer) zurückgeben, die mit einer RangeIndex indiziert sind. Beispielsweise, wenn Sie die Standardeinstellung Indizierung Richtlinie die RangeIndex nur Zahlen sind haben, wird eine Order By anhand eines Pfads mit Zeichenfolgenwerten keine Dokumente zurückgegeben.

### <a name="indexing-for-order-by-for-a-single-property"></a>Indizierung für Order By für eine einzelne Eigenschaft
Hier sind, wie Sie Erstellen einer Websitesammlung mit Indizierung für Order By gegen nur die Title-Eigenschaft, die eine Zeichenfolge ist. Es gibt zwei Wege, eine für die Title-Eigenschaft ("/ Titel /?)" mit Bereich Indizierung und andere für jede andere Eigenschaft mit der Indizierung Standardschema, also Hash für Zeichenfolgen und Bereich für Zahlen ein.                    
    
    booksCollection.IndexingPolicy.IncludedPaths.Add(
        new IncludedPath { 
            Path = "/Title/?", 
            Indexes = new Collection<Index> { 
                new RangeIndex(DataType.String) { Precision = -1 } } 
            });
    
    await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), booksCollection);  


## <a name="samples"></a>Beispiele
Sehen Sie sich dieses [Github Beispielprojekt](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/Queries) , das mit Order By, einschließlich Erstellen von Richtlinien Indizierung und das seitenweise mit Order By veranschaulicht. Die Beispiele sind Quelle öffnen, und wir empfehlen Ihnen, Abruf Anfragen mit Beiträge, die andere Entwickler DocumentDB profitieren zu übermitteln. Lizenzinformationen finden Sie den [Beitrag Richtlinien](https://github.com/Azure/azure-documentdb-net/blob/master/Contributing.md) für Anleitungen zum mitwirken.  

## <a name="faq"></a>Häufig gestellte Fragen

**Was ist der erwarteten anfordern Einheit (RU) Verbrauch der Order By-Abfragen?**

Da Order By DocumentDB Index für Suchvorgänge verwendet, wird die Anzahl der Anfrage Einheiten belegten Order By-Abfragen mit den entsprechenden-Abfragen ohne Order By ähnlich sein. Die Anzahl der Einheiten Anforderung hängt wie alle anderen Vorgänge DocumentDB aus Größen Shapes Dokumente als auch die Komplexität der Abfrage. 


**Was ist die erwarteten Indizierung Aufwand für Order By?**

Die Indizierung Speicher Verwaltungsaufwand werden proportionalen, um die Anzahl der Eigenschaften. In dem Szenario für den ungünstigsten Fall werden die Index-Verwaltungsaufwand 100 % der Daten aus. Es gibt keinen Unterschied zwischen Durchsatz (anfordern Einheiten) Verwaltungsaufwand zwischen Bereich/Order By Indizierung und die standardmäßigen Hash Indizierung.

**Wie Abfrage ich meine vorhandenen Daten mithilfe von Order By DocumentDB?**

Um mit Order By Abfrageergebnisse zu sortieren, müssen Sie die Richtlinie Indizierung der Auflistung, um einen Bereich Index für die Eigenschaft zum Sortieren verwenden ändern. Finden Sie unter [Ändern der Richtlinie Indizierung](documentdb-indexing-policies.md#modifying-the-indexing-policy-of-a-collection). 

**Was sind die aktuellen Schwächen Order By?**

Order By kann nur in Verbindung mit einer Eigenschaft, die entweder numerischen angegeben werden, oder Zeichenfolge, wenn es Bereich ist mit der maximalen Genauigkeit (-1) indiziert.

Sie können nicht die folgenden Schritte aus:
 
- Order By mit internen Zeichenfolgeneigenschaften wie-Id, _rid und _self (in Kürze verfügbar).
- Order By mit Eigenschaften, die als Ergebnis eine innerhalb des Dokuments-Verknüpfung (in Kürze verfügbar) abgeleitet.
- Bestellen Sie anhand mehrerer Eigenschaften (in Kürze verfügbar).
- Order By mit Abfragen auf Datenbanken Websitesammlungen "," Benutzer "," Berechtigungen "oder" Anlagen (in Kürze verfügbar).
- Sortiert nach mit berechneten Eigenschaften z. B. das Ergebnis eines Ausdrucks oder einer Funktion UDFs/integrierte-in.

Order By ist nicht aktuell für Cross-Scheibe Abfragen unterstützt, wenn Abfrage Explorer Azure-Portal zu verwenden.

## <a name="troubleshooting"></a>Behandlung von Problemen

Wenn Sie eine Fehlermeldung, dass die Order By nicht unterstützt wird, überprüfen Sie, stellen Sie sicher, dass Sie eine Version des [SDK](documentdb-sdk-dotnet.md) verwenden, die Order By unterstützt. 

## <a name="next-steps"></a>Nächste Schritte

[Github Beispielprojekt](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/Queries) Verzweigen und Anordnen Ihrer Daten beginnen! 

## <a name="references"></a>Verweise
* [DocumentDB Abfrage Bezug](documentdb-sql-query.md)
* [DocumentDB Indizierung Richtlinie Bezug](documentdb-indexing-policies.md)
* [DocumentDB SQL-Referenz](https://msdn.microsoft.com/library/azure/dn782250.aspx)
* [DocumentDB Order By Beispiele](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/Queries)
 


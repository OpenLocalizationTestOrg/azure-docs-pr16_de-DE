<properties 
    pageTitle="DocumentDB Programmierung: gespeicherte Prozeduren, Datenbanktrigger und UDFs | Microsoft Azure" 
    description="Informationen Sie zum Schreiben von gespeicherten Prozeduren, Datenbanktrigger und benutzerdefinierte Funktionen (Functions, UDFs) in JavaScript DocumentDB verwenden. Abrufen von Datenbank Programing Tipps und vieles mehr." 
    keywords="Trigger gespeicherte Prozedur gespeicherte Prozedur Datenbankprogramm-Prozedur Documentdb Azure, Microsoft Azure-Datenbank"
    services="documentdb" 
    documentationCenter="" 
    authors="aliuy" 
    manager="jhubbard" 
    editor="mimig"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/14/2016" 
    ms.author="andrl"/>

# <a name="documentdb-server-side-programming-stored-procedures-database-triggers-and-udfs"></a>DocumentDB serverseitige Programmierung: gespeicherte Prozeduren, Datenbanktrigger und UDFs

Erfahren Sie, wie die Sprache des Azure DocumentDB integriert, die Ausführung von JavaScript Transaktionen ermöglicht Entwicklern **gespeicherte Prozeduren**, **Trigger** und **benutzerdefinierte Funktionen (Functions, UDFs)** in JavaScript systembedingt schreiben. So können Sie die Datenbank Programmlogik schreiben, die geliefert und direkt auf die Datenbank Speicher Partitionen ausgeführt werden können 

Es empfiehlt sich das folgende Video an, wo Andrew Liu eine kurze Einführung in die DocumentDBs serverseitigen Datenbankmodell Programmierung bietet heraus erste Schritte. 

> [AZURE.VIDEO azure-demo-a-quick-intro-to-azure-documentdbs-server-side-javascript]

Klicken Sie dann zurück zu diesem Artikel, in dem die Antworten auf die folgenden Fragen erfahren Sie:  

- Wie gehen Sie wie folgt Schreiben einer eine gespeicherte Prozedur, Trigger oder mithilfe von JavaScript UDFs?
- Wie garantiert DocumentDB an?
- Wie arbeite Transaktionen in DocumentDB?
- Was sind vordefinierte auslöst und nach dem auslöst und wie schreibe ich eine?
- Wie kann ich registrieren und Ausführen einer gespeicherten Prozedur, Trigger oder UDFs in einer Weise Rest über HTTP?
- Was DocumentDB SDKs stehen für erstellen und Ausführen von gespeicherten Prozeduren, Triggern und UDFs?

## <a name="introduction-to-stored-procedure-and-udf-programming"></a>Einführung in die gespeicherte Prozedur und UDFs Programmierung

Dieser Ansatz von *"Als eine modernen T-SQL-JavaScript"* werden vom die Komplexität von einen System Typenkonflikt und relationalen Zuordnung Technologien Entwickler freigegeben. Sie hat auch eine Reihe von eingebauten Vorteilen, die genutzt werden können, um Rich-Anwendungen zu erstellen:  

-   **Zu Vorgehensweisen Logik:** JavaScript als eine hohe Programmiersprache, stellt eine umfangreiche und vertraute Benutzeroberfläche Geschäftslogik Ausdrücken. Sie können komplexe Folgen von Vorgängen näher zu den Daten durchführen.

-   **Atomaren Transaktionen:** DocumentDB gewährleistet, dass die Datenbankvorgänge ausgeführt wird, innerhalb einer einzigen gespeicherten Prozedur oder Trigger sind atomar. Dadurch wird eine Anwendung verknüpfte Vorgänge in einem einzigen Stapel kombinieren, dass entweder alle erfolgreich ausgeführt werden kann, oder keine von ihnen ein Fehler aufgetreten. 

-   **Leistung:** Eine Reihe von Optimierungen wie die verzögerte Maß an JSON-Dokumente in dem Pufferpool und bereitstellen, stehen bei Bedarf für die Ausführung von Code ermöglicht, dass JSON systembedingt Javascript Sprache Typsystem zugeordnet ist, und ist auch die Grundeinheit Speicher in DocumentDB. Es gibt mehrere Leistungsvorteile Versand Geschäftslogik in der Datenbank zugeordnet:
    -   Batchverarbeitung – Entwickler Operationen wie fügt gruppieren und diese in Massen übermitteln können. Das Netzwerk Datenverkehr Wartezeit Kosten und den Verwaltungsaufwand Store zum Erstellen von separaten Transaktionen wurden erheblich verringert. 
    -   Kompilierung vor – kompiliert DocumentDB vor, gespeicherten Prozeduren, Trigger und benutzerdefinierte Funktionen (Functions, UDFs) JavaScript Kompilierung Kosten für die einzelnen Aufrufe zu vermeiden. Der Aufwand für das Erstellen des Byte-Codes für die zu Vorgehensweisen Logik ist eine minimale Wert amortisiert.
    -   Sequenzielles – viele Vorgänge müssen einer Seite-Effekt ("Trigger"), der potenziell umfasst eine oder mehrere sekundäre Store Operationen durchzuführen. Abgesehen von Unteilbarkeit ist dies weitere leistungsfähige, wenn auf dem Server verschoben. 
-   **Kapselung:** Gespeicherte Prozeduren können verwendet werden, um Geschäftslogik aus einer zentralen Stelle zu gruppieren. Dies hat zwei Vorteile:
    -   Es wird eine Abstraktionsebene über die unformatierten Daten, wodurch Datenarchitekten weiterentwickelt deren Applikationen unabhängig von den Daten hinzugefügt. Dies ist besonders Vorteil, wenn die Daten aufgrund der spröder Annahmen Schema-kleiner, die in die Anwendung integrierten werden ist, wenn sie Daten direkt behandelt aufweisen müssen.  
    -   Diese Abstraktion ermöglicht Unternehmen, die ihre Daten von Inhalten aus den Skripts Zugriff schützen können.  

Das Erstellen und die Ausführung von Datenbanktrigger, gespeicherten Prozedur und benutzerdefinierte Abfrageoperatoren wird durch die [REST-API](https://msdn.microsoft.com/library/azure/dn781481.aspx), [DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio/releases)und in vielen Plattformen einschließlich .NET, Node.js und JavaScript- [Client SDKs](documentdb-sdk-dotnet.md) unterstützt.

Die Formelsyntax und die Verwendung von gespeicherten Prozeduren, Triggern und UDFs veranschaulichen **dieses Lernprogramms das [Node.js SDK mit F Versprechen](http://azure.github.io/azure-documentdb-node-q/) verwendet** .   

## <a name="stored-procedures"></a>Gespeicherten Prozeduren

### <a name="example-write-a-simple-stored-procedure"></a>Beispiel: Schreiben Sie eine einfache gespeicherte Prozedur 
Beginnen wir mit einer einfachen gespeicherten Prozedur, die eine Antwort "Hallo Welt" zurückgibt.

    var helloWorldStoredProc = {
        id: "helloWorld",
        body: function () {
            var context = getContext();
            var response = context.getResponse();
    
            response.setBody("Hello, World");
        }
    }


Gespeicherte Prozeduren werden pro Websitesammlung erfasst und für alle Dokument Anlage präsentieren in dieser Websitesammlung verwendet werden können. Im folgende Codeausschnitt wird gezeigt, wie die HelloWorld gespeicherte Prozedur mit einer Websitesammlung zu registrieren. 

    // register the stored procedure
    var createdStoredProcedure;
    client.createStoredProcedureAsync('dbs/testdb/colls/testColl', helloWorldStoredProc)
        .then(function (response) {
            createdStoredProcedure = response.resource;
            console.log("Successfully created stored procedure");
        }, function (error) {
            console.log("Error", error);
        });


Nachdem die gespeicherte Prozedur registriert ist, können wir führen Sie sie für die Auflistung, und lesen die Ergebnisse wieder auf dem Client. 

    // execute the stored procedure
    client.executeStoredProcedureAsync('dbs/testdb/colls/testColl/sprocs/helloWorld')
        .then(function (response) {
            console.log(response.result); // "Hello, World"
        }, function (err) {
            console.log("Error", error);
        });


Das Kontextobjekt bietet Zugriff auf alle Vorgänge, die für DocumentDB Speicher ausgeführt werden können und Zugriff auf die Anfrage und Antwort Objekte. In diesem Fall verwendet wir das Antwortobjekt Festlegen des Texts der Antwort, die an den Client gesendet wurde. Weitere Informationen hierzu finden Sie in der [DocumentDB JavaScript-Server SDK-Dokumentation](http://azure.github.io/azure-documentdb-js-server/).  

Lassen Sie uns zu diesem Beispiel erweitern, und fügen Sie weitere verwandten Funktionen, um die gespeicherte Prozedur Database-. Gespeicherte Prozeduren können erstellen, aktualisieren, lesen, Abfragen und Löschen von Dokumenten und Anlagen innerhalb der Websitesammlung.    

### <a name="example-write-a-stored-procedure-to-create-a-document"></a>Beispiel: Schreiben Sie eine gespeicherte Prozedur zum Erstellen eines Dokuments 
Der nächste Ausschnitt wird gezeigt, wie das Kontextobjekt Interaktion mit DocumentDB Ressourcen verwenden.

    var createDocumentStoredProc = {
        id: "createMyDocument",
        body: function createMyDocument(documentToCreate) {
            var context = getContext();
            var collection = context.getCollection();

            var accepted = collection.createDocument(collection.getSelfLink(),
                  documentToCreate,
                  function (err, documentCreated) {
                      if (err) throw new Error('Error' + err.message);
                      context.getResponse().setBody(documentCreated.id)
                  });
            if (!accepted) return;
        }
    }


Diese gespeicherte Prozedur akzeptiert als Eingabe DocumentToCreate, im Textkörper eines Dokuments in der aktuellen Auflistung erstellt werden. Alle Vorgänge sind asynchrone und JavaScript Funktionsrückrufe abhängig. Die Rückruffunktion verfügt über zwei Parameter, die zurück-Objekt in Fällen, die durchgeführt, und das erstellte Objekt. Innerhalb des Rückrufs können entweder die Ausnahme behandeln oder einen Fehler ausgelöst. Falls ein Rückruf nicht angegeben wird, und ein Fehler auftritt, löst die Laufzeit DocumentDB einen Fehler an.   

Im Beispiel oben löst der Rückruf einen Fehler an, wenn Fehler bei diesem Vorgang. Andernfalls wird die Id des erstellten Dokuments als Textkörper der Antwort an den Client. Hier sind, wie diese gespeicherte Prozedur mit Eingabeparameter ausgeführt wird.

    // register the stored procedure
    client.createStoredProcedureAsync('dbs/testdb/colls/testColl', createDocumentStoredProc)
        .then(function (response) {
            var createdStoredProcedure = response.resource;
    
            // run stored procedure to create a document
            var docToCreate = {
                id: "DocFromSproc",
                book: "The Hitchhiker’s Guide to the Galaxy",
                author: "Douglas Adams"
            };
    
            return client.executeStoredProcedureAsync('dbs/testdb/colls/testColl/sprocs/createMyDocument',
                  docToCreate);
        }, function (error) {
            console.log("Error", error);
        })
    .then(function (response) {
        console.log(response); // "DocFromSproc"
    }, function (error) {
        console.log("Error", error);
    });

    
Beachten Sie, dass diese gespeicherte Prozedur geändert werden kann, um ein Array von Stellen Dokument als Eingabe übernehmen und alle in dieselbe gespeicherte Prozedur Ausführung statt jede von ihnen einzeln erstellen mehrere Netzwerkanfragen zu erstellen. Dies kann verwendet werden, einen effiziente Massen Importer für DocumentDB (weiter unten in diesem Lernprogramm) implementiert wird.   

Beschriebenen Beispiel wurde veranschaulicht, wie gespeicherte Prozeduren verwendet werden. Wir werden später im Lernprogramm Trigger und benutzerdefinierte Funktionen (Functions, UDFs) behandelt.

## <a name="database-program-transactions"></a>Datenbank-Programm Transaktionen.
Transaktion in einer Datenbank gewöhnlich kann als eine Abfolge von Vorgängen als einzelne logische Arbeitseinheit ausgeführt definiert werden. Jede Transaktion bietet **garantiert an**. MAN ist ein bekanntes Akronym, der für vier Eigenschaften - Unteilbarkeit, Konsistenz, Isolation und Zuverlässigkeit steht.  

Kurz gesagt Unteilbarkeit ist sichergestellt, dass alle Arbeit innerhalb einer Transaktion als einzelne Einheit behandelt wird, in dem entweder alle es sieht sich verpflichtet oder keiner. Konsistenz wird sichergestellt, dass die Daten immer in einem guten internen Zustand über Transaktionen. Grad der Isolation wird sichergestellt, dass keine zwei Transaktionen beeinträchtigen miteinander – in der Regel, die meisten kommerziellen Systeme bieten, anhand von muss die Anwendung mehrere Isolationsebenen, die verwendet werden können. Zuverlässigkeit: Damit ist sichergestellt, dass die Änderungen, die in der Datenbank belegt wird immer vorhanden sein.   

JavaScript wird in DocumentDB in der selben Speicherbereich wie die Datenbank gehostet wird. Daher Besprechungsanfragen, die innerhalb der gespeicherten Prozeduren und Trigger im gleichen Bereich einer Sitzung für die Datenbank ausgeführt werden. Dadurch DocumentDB zu und für alle Vorgänge sichergestellt ist, die eine einzelne gespeicherte Prozedur/Trigger gehören. Beachten Sie die folgenden Definition der gespeicherten Prozedur ein:

    // JavaScript source code
    var exchangeItemsSproc = {
        name: "exchangeItems",
        body: function (playerId1, playerId2) {
            var context = getContext();
            var collection = context.getCollection();
            var response = context.getResponse();
    
            var player1Document, player2Document;
    
            // query for players
            var filterQuery = 'SELECT * FROM Players p where p.id  = "' + playerId1 + '"';
            var accept = collection.queryDocuments(collection.getSelfLink(), filterQuery, {},
                function (err, documents, responseOptions) {
                    if (err) throw new Error("Error" + err.message);
    
                    if (documents.length != 1) throw "Unable to find both names";
                    player1Document = documents[0];
    
                    var filterQuery2 = 'SELECT * FROM Players p where p.id = "' + playerId2 + '"';
                    var accept2 = collection.queryDocuments(collection.getSelfLink(), filterQuery2, {},
                        function (err2, documents2, responseOptions2) {
                            if (err2) throw new Error("Error" + err2.message);
                            if (documents2.length != 1) throw "Unable to find both names";
                            player2Document = documents2[0];
                            swapItems(player1Document, player2Document);
                            return;
                        });
                    if (!accept2) throw "Unable to read player details, abort ";
                });
    
            if (!accept) throw "Unable to read player details, abort ";
    
            // swap the two players’ items
            function swapItems(player1, player2) {
                var player1ItemSave = player1.item;
                player1.item = player2.item;
                player2.item = player1ItemSave;
    
                var accept = collection.replaceDocument(player1._self, player1,
                    function (err, docReplaced) {
                        if (err) throw "Unable to update player 1, abort ";
    
                        var accept2 = collection.replaceDocument(player2._self, player2,
                            function (err2, docReplaced2) {
                                if (err) throw "Unable to update player 2, abort"
                            });
    
                        if (!accept2) throw "Unable to update player 2, abort";
                    });
    
                if (!accept) throw "Unable to update player 1, abort";
            }
        }
    }
    
    // register the stored procedure in Node.js client
    client.createStoredProcedureAsync(collection._self, exchangeItemsSproc)
        .then(function (response) {
            var createdStoredProcedure = response.resource;
        }
    );

Diese gespeicherte Prozedur verwendet Transaktionen innerhalb einer app Spiele Trade Elemente zwischen zwei Spieler in einem einzigen Vorgang an. Die gespeicherte Prozedur versucht, zwei Dokumente lesen, die jeweils entsprechenden Player-IDs in als Argument übergeben. Wenn beide Dokumente Player, gefunden werden und klicken Sie dann die gespeicherte Prozedur die Dokumente aktualisiert, indem ihre Elemente austauschen. Wenn Sie unterwegs Fehler aufgetreten sind, wird eine JavaScript-Ausnahme, die implizit die Transaktion abgebrochen wird.

Wenn die Sammlung der gespeicherten Prozedur registriert ist gegen besteht aus einer Partition, und klicken Sie dann die Transaktion wird auf alle Docuemnts in der Sammlung ausgelegte. Wenn der Websitesammlung konfiguriert ist, werden gespeicherten Prozeduren in den Transaktionsbereich einer einzelnen Partitionsschlüssel ausgeführt. Jede gespeicherte Prozedur Ausführung muss einbeziehen einen Partition Schlüsselwert entspricht den Umfang die Transaktion ausgeführt werden muss. Weitere Informationen hierzu finden Sie unter [DocumentDB Partitionierung](documentdb-partition-data.md).

### <a name="commit-and-rollback"></a>Commit und Zurücksetzen
Transaktionen sind tief und systembedingt in Programmierung DocumentDBs JavaScript-Modell integriert. Innerhalb einer JavaScript-Funktion werden alle Vorgänge unter einer einzelnen Transaktion automatisch umbrochen. Wenn das JavaScript ohne Ausnahme abgeschlossen ist, sind die Vorgänge in der Datenbank zugesicherte. "Transaktion beginnen" und "COMMIT Transaktion" Anweisungen in relationalen Datenbanken sind, implizit in DocumentDB.  
 
Ist eine Ausnahme, die vom das Skript weitergegeben wird, wird DocumentDBs JavaScript-Laufzeit die gesamte Transaktion zurückzusetzen. Wie im Beispiel oben gezeigt wird, wurde, eine Ausnahme "Zurücksetzen Transaktion" in DocumentDB entspricht.
 
### <a name="data-consistency"></a>Konsistenz der Daten
Gespeicherte Prozeduren und Trigger werden immer auf die primäre Kopie der Auflistung DocumentDB ausgeführt. Wird sichergestellt, dass innerhalb liest Verfahren Angebot signifikante Konsistenz gespeichert. Abfragen mithilfe von benutzerdefinierten Funktionen auf dem primären oder einem beliebigen sekundäre Replikat ausgeführt werden können, aber wir sicher, dass Sie die angeforderten Konsistenz Ebene entsprechen, indem Sie das entsprechende Replikat auswählen.

## <a name="bounded-execution"></a>Begrenzte Ausführung
Alle DocumentDB Vorgänge müssen innerhalb des angegebenen Servers ausführen Timeout Kaufangebots. Diese Einschränkung gilt auch für JavaScript-Funktionen (gespeicherten Prozeduren, Trigger und benutzerdefinierte Funktionen). Wenn ein Vorgang nicht mit dieser Frist abgeschlossen wird, wird die Transaktion rückgängig gemacht werden. JavaScript-Funktionen müssen innerhalb der Frist Ende oder implementieren ein Modells Fortsetzung Grundlage für Stapel/Ausführung Lebenslauf.  

Um die Entwicklung von gespeicherten Prozeduren und Triggern Fristen verarbeitet zu vereinfachen, zurück alle Funktionen unter Websitesammlung Objekts (erstellen, lesen, ersetzen und Löschen von Dokumenten und Anlagen), der angibt, ob für den Vorgang ist, wird der boolesche Wert. Wenn dieser Wert false ist, ist es eine Angabe, dass die Frist demnächst abläuft und, dass Sie die Ausführung die Prozedur umbrechen muss.  Vorgänge in der Warteschlange vor dem ersten Vorgang vom nicht akzeptierten Store befinden sich unbedingt abzuschließen, wenn die gespeicherte Prozedur Zeitpunkt abgeschlossen ist, und keine weitere Anforderungen Warteschlange.  

JavaScript-Funktionen sind auch auf Ressourcenverbrauch begrenzt. DocumentDB reserviert Durchsatz pro Websitesammlung anhand der bereitgestellte Größe eines Datenbank-Kontos. Durchsatz ist auch mithilfe einer standardisierten Einheit CPU, Arbeitsspeicher und EA Verbrauch Anforderung Einheiten oder RUs genannt formuliert. JavaScript-Funktionen können potenziell eine große Anzahl von RUs innerhalb einer kurzen Zeit, und erhalten möglicherweise Zins Beschränkung, wenn der Auflistung erreicht ist. Ressource stark gespeicherte Prozeduren möglicherweise auch zur Sicherstellung der Verfügbarkeit von Datenbankvorgängen primitiven isoliert werden.  

### <a name="example-bulk-importing-data-into-a-database-program"></a>Beispiel: Gruppenweise Importieren von Daten in einem Datenbankprogramm
Es folgt ein Beispiel für eine gespeicherte Prozedur, die für den-Dokumente in einer Websitesammlung Massenimport geschrieben wurde. Beachten Sie, wie die gespeicherte Prozedur begrenzte Ausführung verarbeitet, indem Sie der boolesche Wert Rückgabewert aus CreateDocument, und klicken Sie dann die Anzahl der eingefügten jeden der Aufrufe der gespeicherten Prozedur Dokumente zum Nachverfolgen und Fortsetzen des Vorgangsfortschritts über Blattnamen verwendet.

    function bulkImport(docs) {
        var collection = getContext().getCollection();
        var collectionLink = collection.getSelfLink();
    
        // The count of imported docs, also used as current doc index.
        var count = 0;
    
        // Validate input.
        if (!docs) throw new Error("The array is undefined or null.");
    
        var docsLength = docs.length;
        if (docsLength == 0) {
            getContext().getResponse().setBody(0);
        }
    
        // Call the create API to create a document.
        tryCreate(docs[count], callback);
    
        // Note that there are 2 exit conditions:
        // 1) The createDocument request was not accepted. 
        //    In this case the callback will not be called, we just call setBody and we are done.
        // 2) The callback was called docs.length times.
        //    In this case all documents were created and we don’t need to call tryCreate anymore. Just call setBody and we are done.
        function tryCreate(doc, callback) {
            var isAccepted = collection.createDocument(collectionLink, doc, callback);
    
            // If the request was accepted, callback will be called.
            // Otherwise report current count back to the client, 
            // which will call the script again with remaining set of docs.
            if (!isAccepted) getContext().getResponse().setBody(count);
        }
    
        // This is called when collection.createDocument is done in order to process the result.
        function callback(err, doc, options) {
            if (err) throw err;
    
            // One more document has been inserted, increment the count.
            count++;
    
            if (count >= docsLength) {
                // If we created all documents, we are done. Just set the response.
                getContext().getResponse().setBody(count);
            } else {
                // Create next document.
                tryCreate(docs[count], callback);
            }
        }
    }

## <a name="a-idtriggera-database-triggers"></a><a id="trigger"></a>Datenbanktrigger
### <a name="database-pre-triggers"></a>Vor dem Datenbanktrigger
DocumentDB enthält Trigger, die ausgelöst wurde, indem Sie einen Vorgang an einem Dokument oder ausgeführt werden. Beispielsweise können Sie vor dem Trigger angeben, beim Erstellen eines Dokuments – dieser Vorabversion Trigger wird ausgeführt, bevor das Dokument erstellt wird. Im folgenden finden ein Beispiel für wie vor dem Trigger verwendet werden können, um die Eigenschaften eines Dokuments zu überprüfen, die erstellt wird:

    var validateDocumentContentsTrigger = {
        name: "validateDocumentContents",
        body: function validate() {
            var context = getContext();
            var request = context.getRequest();
    
            // document to be created in the current operation
            var documentToCreate = request.getBody();
    
            // validate properties
            if (!("timestamp" in documentToCreate)) {
                var ts = new Date();
                documentToCreate["my timestamp"] = ts.getTime();
            }
    
            // update the document that will be created
            request.setBody(documentToCreate);
        },
        triggerType: TriggerType.Pre,
        triggerOperation: TriggerOperation.Create
    }


Und die entsprechenden Node.js clientseitige Registrierungscode für den Trigger:

    // register pre-trigger
    client.createTriggerAsync(collection.self, validateDocumentContentsTrigger)
        .then(function (response) {
            console.log("Created", response.resource);
            var docToCreate = {
                id: "DocWithTrigger",
                event: "Error",
                source: "Network outage"
            };
    
            // run trigger while creating above document 
            var options = { preTriggerInclude: "validateDocumentContents" };
    
            return client.createDocumentAsync(collection.self,
                  docToCreate, options);
        }, function (error) {
            console.log("Error", error);
        })
    .then(function (response) {
        console.log(response.resource); // document with timestamp property added
    }, function (error) {
        console.log("Error", error);
    });


Vor dem Trigger eventueller Eingabeparameter nicht möglich. Das Request-Objekt kann verwendet werden, bearbeiten Sie die Anfragenachricht, die dem Vorgang zugeordnet. Hier, der vor dem Trigger mit der Erstellung eines Dokuments ausgeführt wird, und der Anforderung Nachrichtentext enthält das Dokument im JSON-Format erstellt werden.   

Wenn Trigger registriert sind, können Benutzer die Vorgänge festzulegen, denen mit ausgeführt werden kann. Dieser Trigger erstellt wurde mit TriggerOperation.Create, was bedeutet, dass Folgendes nicht zulässig ist.

    var options = { preTriggerInclude: "validateDocumentContents" };
    
    client.replaceDocumentAsync(docToReplace.self,
                  newDocBody, options)
    .then(function (response) {
        console.log(response.resource);
    }, function (error) {
        console.log("Error", error);
    });
    
    // Fails, can’t use a create trigger in a replace operation

### <a name="database-post-triggers"></a>Nach der Datenbanktrigger
Nach der Trigger, wie vor dem Trigger, einen Vorgang an einem Dokument zugeordnet sind und eventueller Eingabeparameter nicht ausführen. Sie führen Sie **nach** , die der Vorgang abgeschlossen ist, und haben Zugriff auf die Antwortnachricht, die an den Client gesendet wird.   

Im folgenden Beispiel wird nach der Trigger in Aktion:

    var updateMetadataTrigger = {
        name: "updateMetadata",
        body: function updateMetadata() {
            var context = getContext();
            var collection = context.getCollection();
            var response = context.getResponse();
    
            // document that was created
            var createdDocument = response.getBody();
    
            // query for metadata document
            var filterQuery = 'SELECT * FROM root r WHERE r.id = "_metadata"';
            var accept = collection.queryDocuments(collection.getSelfLink(), filterQuery,
                updateMetadataCallback);
            if(!accept) throw "Unable to update metadata, abort";
     
            function updateMetadataCallback(err, documents, responseOptions) {
                if(err) throw new Error("Error" + err.message);
                         if(documents.length != 1) throw 'Unable to find metadata document';
                         
                         var metadataDocument = documents[0];
                         
                         // update metadata
                         metadataDocument.createdDocuments += 1;
                         metadataDocument.createdNames += " " + createdDocument.id;
                         var accept = collection.replaceDocument(metadataDocument._self,
                               metadataDocument, function(err, docReplaced) {
                                      if(err) throw "Unable to update metadata, abort";
                               });
                         if(!accept) throw "Unable to update metadata, abort";
                         return;                    
            }                                                                                           
        },
        triggerType: TriggerType.Post,
        triggerOperation: TriggerOperation.All
    }


Der Trigger kann registriert werden, wie im folgenden Beispiel dargestellt.

    // register post-trigger
    client.createTriggerAsync('dbs/testdb/colls/testColl', updateMetadataTrigger)
        .then(function(createdTrigger) { 
            var docToCreate = { 
                name: "artist_profile_1023",
                artist: "The Band",
                albums: ["Hellujah", "Rotators", "Spinning Top"]
            };
    
            // run trigger while creating above document 
            var options = { postTriggerInclude: "updateMetadata" };
        
            return client.createDocumentAsync(collection.self,
                  docToCreate, options);
        }, function(error) {
            console.log("Error" , error);
        })
    .then(function(response) {
        console.log(response.resource); 
    }, function(error) {
        console.log("Error" , error);
    });


Dieser Trigger für das Metadatendokument Abfragen und mit Details zu den neu erstellten Dokument aktualisiert.  

Eine Sache, die beachtet werden, ist die Ausführung von Triggern in DocumentDB von **Transaktionen** . Dieser nach der Trigger wird als Teil der gleichen Transaktion wie die Erstellung des ursprünglichen Dokuments ausgeführt. Daher, wenn wir eine Ausnahme auslösen, aus dem nach der Trigger (angenommen, wenn das Metadatendokument aktualisieren können), die gesamte Transaktion fehl, und rückgängig gemacht werden. Kein Dokument erstellt werden, und eine Ausnahme zurückgegeben wird.  

##<a name="a-idudfauser-defined-functions"></a><a id="udf"></a>Benutzerdefinierte Funktionen
Benutzerdefinierte Funktionen (Functions, UDFs) werden verwendet, um die DocumentDB SQL-Abfrage-Sprache-Grammatik erweitern und benutzerdefinierte Geschäftslogik implementieren. Sie können nur von aufgerufen werden in Abfragen. Sie haben keinen Zugriff auf das Kontextobjekt und berechnen nur JavaScript verwendet werden sollen. Daher können UDFs auf sekundäre Replikate des Diensts DocumentDB ausgeführt werden.  
 
Im folgende Beispiel eine benutzerdefinierte Funktion zum Einkommen Steuer ausgehend von Sätzen für verschiedene Einkommen Klammern berechnen erstellt, und klicken Sie dann in einer Abfrage an um alle Personen zu suchen, die mehr als 20.000 in steuern bezahlt verwendet.

    var taxUdf = {
        name: "tax",
        body: function tax(income) {
    
            if(income == undefined) 
                throw 'no input';
    
            if (income < 1000) 
                return income * 0.1;
            else if (income < 10000) 
                return income * 0.2;
            else
                return income * 0.4;
        }
    }


UDFs kann später in Abfragen wie im folgenden Beispiel verwendet werden:

    // register UDF
    client.createUserDefinedFunctionAsync('dbs/testdb/colls/testColl', taxUdf)
        .then(function(response) { 
            console.log("Created", response.resource);
    
            var query = 'SELECT * FROM TaxPayers t WHERE udf.tax(t.income) > 20000'; 
            return client.queryDocuments('dbs/testdb/colls/testColl',
                   query).toArrayAsync();
        }, function(error) {
            console.log("Error" , error);
        })
    .then(function(response) {
        var documents = response.feed;
        console.log(response.resource); 
    }, function(error) {
        console.log("Error" , error);
    });

## <a name="javascript-language-integrated-query-api"></a>JavaScript-Sprache integriert Abfrage-API
Zusätzlich zu Abfragen mithilfe DocumentDBs SQL-Grammatik ausgegeben wird, können mit das serverseitigen SDK optimierte Abfragen mithilfe einer fluent JavaScript-Benutzeroberfläche ohne Kenntnis der SQL ausführen. Die JavaScript-Abfrage API ermöglicht Ihnen, die programmgesteuert Abfragen erstellen, indem Sie Prädikat Funktionen in chainable (Funktion) Anrufe mit einer vertrauten ECMAScript5s Array den vordefinierten Gruppen und beliebte JavaScript-Bibliotheken wie Lodash Syntax. Abfragen werden durch die JavaScript-Laufzeit auszuführende effizient mithilfe DocumentDBs Indizes analysiert.

> [AZURE.NOTE]`__` (Double-Unterstrich) ist ein Alias in `getContext().getCollection()`.
> <br/>
> Können also `__` oder `getContext().getCollection()` auf die Abfrage JavaScript-API zugreifen.

Unterstützte Funktionen einbeziehen:
<ul>
<li>
<b>... Chain(). Wert ([Rückruf] [, Optionen])</b>
<ul>
<li>
Gespräch verketteten der beendet werden muss beginnt mit value().
</li>
</ul>
</li>
<li>
<b>Filter (PredicateFunction [, Optionen] [, Rückruf])</b>
<ul>
<li>
Filtert die Eingabe mithilfe einer Prädikatfunktion die wahr/falsch um Filtern ein-/von Dokumenten in die resultierende Datensatzgruppe zurückgibt. Dies verhält sich ähnlich wie in einer WHERE-Klausel in SQL.
</li>
</ul>
</li>
<li>
<b>Zuordnung (TransformationFunction [, Optionen] [, Rückruf])</b>
<ul>
<li>
Wendet eine Projektion ausgehend von einer Transformationsfunktion, die jedes Element von einer JavaScript-Objekt oder ein Wert zugeordnet ist. Diese Methode verhält sich ähnlich wie eine SELECT-Klausel in SQL.
</li>
</ul>
</li>
<li>
<b>Pluck ([Eigenschaftenname] [, Optionen] [, Rückruf])</b>
<ul>
<li>
Dies ist eine Abkürzung für ein Schema aus dem den Wert einer einzelnen Eigenschaft aus jeder Eingabewerte Element extrahiert werden.
</li>
</ul>
</li>
<li>
<b>reduzieren ([IsShallow] [, Optionen] [, Rückruf])</b>
<ul>
<li>
Kombiniert und fasst Arrays von jedem Eingabewerte Element in in einem Array. Diese Methode verhält sich ähnlich wie SelectMany in LINQ.
</li>
</ul>
</li>
<li>
<b>SortBy ([Prädikat] [, Optionen] [, Rückruf])</b>
<ul>
<li>
Erstellen Sie einen neuen Satz von Dokumenten, indem Sie sortieren die Dokumente im Dokumentstream Eingabewerte in aufsteigender Reihenfolge, mit dem angegebenen Prädikat. Diese Methode verhält sich ähnlich wie eine ORDER BY-Klausel in SQL.
</li>
</ul>
</li>
<li>
<b>SortByDescending ([Prädikat] [, Optionen] [, Rückruf])</b>
<ul>
<li>
Erstellen Sie einen neuen Satz von Dokumenten, indem Sie sortieren die Dokumente im Dokumentstream Eingabewerte in absteigender Reihenfolge, mit dem angegebenen Prädikat. Diese Methode verhält sich ähnlich einer nach X DESC ORDER BY-Klausel in SQL.
</li>
</ul>
</li>
</ul>


Wenn innerhalb von Prädikat und/oder Ansichtsauswahl Funktionen enthalten, erhalten automatisch die folgenden JavaScript-Konstrukte optimiert, um direkt auf DocumentDB Indizes auszuführen:

* Einfache Operatoren: = + - * / % | ^ &amp; == != === !=== &lt; &gt; &lt;= &gt;= || &amp;&amp; &lt;&lt; &gt;&gt; &gt;&gt;&gt;! ~
* Literalen, einschließlich des Objekts literaler: {}
* Varianz, zurück

Die folgenden JavaScript-Konstrukte nicht für DocumentDB Indizes optimiert erhalten:

* Informationsfluss zu steuern (z. B. If, während)
* Anrufe (Funktion)

Weitere Informationen finden Sie unter unseren [Serverseitigen JSDocs](http://azure.github.io/azure-documentdb-js-server/).

### <a name="example-write-a-stored-procedure-using-the-javascript-query-api"></a>Beispiel: Schreiben Sie eine gespeicherte Prozedur mithilfe der Abfrage JavaScript-API

Im folgenden Beispiel ist ein Beispiel für wie die JavaScript-Abfrage-API im Kontext einer gespeicherten Prozedur verwendet werden kann. Die gespeicherte Prozedur fügt ein Dokument, ausgehend von einem Eingabeparameter, und eine Metadaten aktualisiert Dokument mithilfe der `__.filter()` Methode mit MinSize, MaxSize und TotalSize auf Grundlage der Eingabe des Dokuments Size-Eigenschaft.

    /**
     * Insert actual doc and update metadata doc: minSize, maxSize, totalSize based on doc.size.
     */
    function insertDocumentAndUpdateMetadata(doc) {
      // HTTP error codes sent to our callback funciton by DocDB server.
      var ErrorCode = {
        RETRY_WITH: 449,
      }

      var isAccepted = __.createDocument(__.getSelfLink(), doc, {}, function(err, doc, options) {
        if (err) throw err;

        // Check the doc (ignore docs with invalid/zero size and metaDoc itself) and call updateMetadata.
        if (!doc.isMetadata && doc.size > 0) {
          // Get the meta document. We keep it in the same collection. it's the only doc that has .isMetadata = true.
          var result = __.filter(function(x) {
            return x.isMetadata === true
          }, function(err, feed, options) {
            if (err) throw err;

            // We assume that metadata doc was pre-created and must exist when this script is called.
            if (!feed || !feed.length) throw new Error("Failed to find the metadata document.");

            // The metadata document.
            var metaDoc = feed[0];

            // Update metaDoc.minSize:
            // for 1st document use doc.Size, for all the rest see if it's less than last min.
            if (metaDoc.minSize == 0) metaDoc.minSize = doc.size;
            else metaDoc.minSize = Math.min(metaDoc.minSize, doc.size);

            // Update metaDoc.maxSize.
            metaDoc.maxSize = Math.max(metaDoc.maxSize, doc.size);

            // Update metaDoc.totalSize.
            metaDoc.totalSize += doc.size;

            // Update/replace the metadata document in the store.
            var isAccepted = __.replaceDocument(metaDoc._self, metaDoc, function(err) {
              if (err) throw err;
              // Note: in case concurrent updates causes conflict with ErrorCode.RETRY_WITH, we can't read the meta again 
              //       and update again because due to Snapshot isolation we will read same exact version (we are in same transaction).
              //       We have to take care of that on the client side.
            });
            if (!isAccepted) throw new Error("replaceDocument(metaDoc) returned false.");
          });
          if (!result.isAccepted) throw new Error("filter for metaDoc returned false.");
        }
      });
      if (!isAccepted) throw new Error("createDocument(actual doc) returned false.");
    }

## <a name="sql-to-javascript-cheat-sheet"></a>SQL zu Blatts für Javascript
Die folgende Tabelle zeigt verschiedene SQL-Abfragen und den entsprechenden JavaScript-Abfragen.

Wie mit SQL-Abfragen Eigenschaft Tasten Dokument (z. B. `doc.id`) Groß-/Kleinschreibung beachtet werden.

<br/>
<table border="1" width="100%">
<colgroup>
<col span="1" style="width: 40%;">
<col span="1" style="width: 40%;">
<col span="1" style="width: 20%;">
</colgroup>
<tbody>
<tr>
<th>SQL</th>
<th>JavaScript-Abfrage-API</th>
<th>Details</th>
</tr>
<tr>
<td>
<pre>
Wählen Sie * von Dokumenten
</pre>
</td>
<td>
<pre>
__.Map(Function(DOC) {zurückgegebene Dokument;});
</pre>
</td>
<td>Ergebnisse in allen Dokumenten (paginiert mit Fortsetzungstoken) als ist.</td>
</tr>
<tr>
<td>
<pre>
SELECT docs.id, docs.message als msg, docs.actions von Dokumenten
</pre>
</td>
<td>
<pre>
__.Map(Function(DOC) {zurückgeben {Id: doc.id, msg: doc.message, Aktionen: doc.actions};});
</pre>
</td>
<td>Projekte, die Id, die Nachricht (msg Alias) und die Aktion aus allen Dokumenten.</td>
</tr>
<tr>
<td>
<pre>
Wählen Sie * aus Dokumenten WHERE docs.id="X998_Y998"
</pre>
</td>
<td>
<pre>
__.Filter(Function(DOC) {zurückgeben doc.id === "X998_Y998;"});
</pre>
</td>
<td>Abfragen für Dokumente mit dem Prädikat: Id = "X998_Y998".</td>
</tr>
<tr>
<td>
<pre>
Wählen Sie * aus Dokumenten, wo ARRAY_CONTAINS(docs. Tags, 123)
</pre>
</td>
<td>
<pre>
__.Filter(Function(x) {zurückgeben x.Tags & & x.Tags.indexOf(123) >-1;});
</pre>
</td>
<td>Abfragen für Dokumente, die eine Eigenschaft Kategorien und Tags enthalten ist ein Array mit dem Wert 123.</td>
</tr>
<tr>
<td>
<pre>
SELECT docs.id, docs.message als msg von Dokumenten WHERE docs.id="X998_Y998"
</pre>
</td>
<td>
<pre>
__.Chain() .filter(function(doc) {zurückgeben doc.id === "X998_Y998;"}) .map(function(doc) {zurückgeben {Id: doc.id, msg: doc.message};}) .value();
</pre>
</td>
<td>Abfragen für Dokumente mit einer Prädikate Id = "X998_Y998", und klicken Sie dann auf Projekte die-Id und einer Nachricht (msg Alias).</td>
</tr>
<tr>
<td>
<pre>
Wählen Sie VALUE-Tag von Dokumenten Verknüpfung Kategorie IN Dokumenten. Kategorien sortiert nach docs._ts
</pre>
</td>
<td>
<pre>
__.Chain() .filter(function(doc) {Doc zurückgegeben. Kategorien & & Array.isArray (Doc. Kategorien); }) .sortBy(function(doc) {Absenderadresse Doc._ts;}) .pluck("tags") .flatten() .value()
</pre>
</td>
<td>Filter für Dokumente, denen Array-Eigenschaft, Kategorien, und sortiert die resultierende Dokumente nach der _ts Zeitstempel System-Eigenschaft, und klicken Sie dann Projekte + fasst die Matrix Kategorien.</td>
</tr>
</tbody>
</table>

## <a name="runtime-support"></a>Runtime-Unterstützung
[DocumentDB JavaScript serverseitigen SDK](http://azure.github.io/azure-documentdb-js-server/) bietet Unterstützung für die am häufigsten mainstream JavaScript Sprache Features wie standardisierte durch [ECMA-262](http://www.ecma-international.org/publications/standards/Ecma-262.htm).

### <a name="security"></a>Sicherheit
JavaScript gespeicherten Prozeduren und Trigger sind Sandkasten, sodass die Auswirkungen eines ein-Skript nicht zu einem anderen verloren gehen nicht über die Momentaufnahme Transaktionsisolation Ebene der Datenbank. Laufzeit-Umgebungen sind gepoolte aber bereinigt des Kontexts nach jeder ausführen. Daher stimmen sie sichere aller unbeabsichtigte Seite Effekte voneinander werden.

### <a name="pre-compilation"></a>Kompilierung vor
Gespeicherte Prozeduren, Trigger und UDFs werden implizit in vorkompiliert Codeformat Byte um eine Kompilierung Kosten zum Zeitpunkt der einzelnen Aufrufen des Skripts zu vermeiden. Dadurch wird sichergestellt, Aufrufe von gespeicherten Prozeduren sind schnelle, und weisen einem niedrig Ort zur Verfügung.

## <a name="client-sdk-support"></a>Client-SDK-Unterstützung
Zusätzlich zu den Client [Node.js](documentdb-sdk-node.md) unterstützt DocumentDB [.NET](documentdb-sdk-dotnet.md), [Java](documentdb-sdk-java.md), [JavaScript](http://azure.github.io/azure-documentdb-js/)und [Python SDKs](documentdb-sdk-python.md). Gespeicherte Prozeduren, Trigger und UDFs erstellt werden können und mit einer der folgenden SDKs ebenfalls ausgeführt. Im folgenden Beispiel wird das Erstellen und Ausführen einer gespeicherten Prozedur über den Client .NET. Hinweis wie die Typen von .NET übergeben an die gespeicherte Prozedur als JSON und wieder zu lesen sind.

    var markAntiquesSproc = new StoredProcedure
    {
        Id = "ValidateDocumentAge",
        Body = @"
                function(docToCreate, antiqueYear) {
                    var collection = getContext().getCollection();    
                    var response = getContext().getResponse();    
    
                    if(docToCreate.Year != undefined && docToCreate.Year < antiqueYear){
                        docToCreate.antique = true;
                    }
    
                    collection.createDocument(collection.getSelfLink(), docToCreate, {}, 
                        function(err, docCreated, options) { 
                            if(err) throw new Error('Error while creating document: ' + err.message);                              
                            if(options.maxCollectionSizeInMb == 0) throw 'max collection size not found'; 
                            response.setBody(docCreated);
                    });
            }"
    };
    
    // register stored procedure
    StoredProcedure createdStoredProcedure = await client.CreateStoredProcedureAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"), markAntiquesSproc);
    dynamic document = new Document() { Id = "Borges_112" };
    document.Title = "Aleph";
    document.Year = 1949;
    
    // execute stored procedure
    Document createdDocument = await client.ExecuteStoredProcedureAsync<Document>(UriFactory.CreateStoredProcedureUri("db", "coll", "sproc"), document, 1920);


Dieses Beispiel zeigt, wie Sie mithilfe der [.NET SDK](https://msdn.microsoft.com/library/azure/dn948556.aspx) vor dem Trigger erstellen, und erstellen ein Dokument mit dem Trigger aktiviert. 

    Trigger preTrigger = new Trigger()
    {
        Id = "CapitalizeName",
        Body = @"function() {
            var item = getContext().getRequest().getBody();
            item.id = item.id.toUpperCase();
            getContext().getRequest().setBody(item);
        }",
        TriggerOperation = TriggerOperation.Create,
        TriggerType = TriggerType.Pre
    };
    
    Document createdItem = await client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"), new Document { Id = "documentdb" },
        new RequestOptions
        {
            PreTriggerInclude = new List<string> { "CapitalizeName" },
        });


Und das folgende Beispiel zeigt, wie Sie eine benutzerdefinierte Funktion (UDFs) erstellen und diese in einer [DocumentDB SQL-Abfrage](documentdb-sql-query.md)verwenden.

    UserDefinedFunction function = new UserDefinedFunction()
    {
        Id = "LOWER",
        Body = @"function(input) 
        {
            return input.toLowerCase();
        }"
    };
    
    foreach (Book book in client.CreateDocumentQuery(UriFactory.CreateDocumentCollectionUri("db", "coll"),
        "SELECT * FROM Books b WHERE udf.LOWER(b.Title) = 'war and peace'"))
    {
        Console.WriteLine("Read {0} from query", book);
    }

## <a name="rest-api"></a>REST-API
Alle DocumentDB Vorgänge können in einer Rest Weise durchgeführt werden. Gespeicherte Prozeduren, Trigger und benutzerdefinierte Funktionen können unter einer Websitesammlung mithilfe von HTTP POST registriert werden. Im folgenden finden ein Beispiel für eine gespeicherte Prozedur zu registrieren:

    POST https://<url>/sprocs/ HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:10 GMT
    
    
    var x = {
      "name": "createAndAddProperty",
      "body": function (docToCreate, addedPropertyName, addedPropertyValue) {
                var collectionManager = getContext().getCollection();
                collectionManager.createDocument(
                    collectionManager.getSelfLink(),
                    docToCreate,
                    function(err, docCreated) {
                      if(err) throw new Error('Error:  ' + err.message);
                      docCreated[addedPropertyName] = addedPropertyValue;
                      getContext().getResponse().setBody(docCreated);
                    });
            }
    }


Die gespeicherte Prozedur registriert ist durch Ausführen einer Anforderung Beitrag gegen URI Datenbanken/Testdb/colls/TestColl/Routeninformationen an die Stelle, mit der gespeicherten Prozedur zu erstellen. Trigger und UDFs können auf ähnliche Weise durch einen Beitrag gegen /triggers und /udfs Hilfethemas ausgeben registriert werden.
Diese gespeicherte Prozedur kann dann durch einen Beitrag Anforderung anhand ihrer Ressource Verknüpfung ausgeführt werden:

    POST https://<url>/sprocs/<sproc> HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:20 GMT
    
    
    [ { "name": "TestDocument", "book": "Autumn of the Patriarch"}, "Price", 200 ]


Hier wird die Eingabe für die gespeicherte Prozedur im Hauptteil Anforderung übergeben. Beachten Sie, dass die Eingabe als JSON-Array der Eingabeparameter weitergegeben wird. Die gespeicherte Prozedur übernimmt die erste Eingabe als Dokument, die eine Antworttext darstellt. Die Antwort, die wir erhalten sieht wie folgt aus:

    HTTP/1.1 200 OK
     
    { 
      name: 'TestDocument',
      book: ‘Autumn of the Patriarch’,
      id: ‘V7tQANV3rAkDAAAAAAAAAA==‘,
      ts: 1407830727,
      self: ‘dbs/V7tQAA==/colls/V7tQANV3rAk=/docs/V7tQANV3rAkDAAAAAAAAAA==/’,
      etag: ‘6c006596-0000-0000-0000-53e9cac70000’,
      attachments: ‘attachments/’,
      Price: 200
    }


Trigger, im Gegensatz zu gespeicherten Prozeduren können nicht direkt ausgeführt werden. Stattdessen werden sie als Teil eines Vorgangs an einem Dokument ausgeführt. Wir können die Trigger zum Ausführen mit einer Anforderung mit HTTP-Header angeben. Im folgenden finden die Anfrage zur Erstellung eines Dokuments.

    POST https://<url>/docs/ HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:10 GMT
    x-ms-documentdb-pre-trigger-include: validateDocumentContents 
    x-ms-documentdb-post-trigger-include: bookCreationPostTrigger
    
    
    {
       "name": "newDocument",
       “title”: “The Wizard of Oz”,
       “author”: “Frank Baum”,
       “pages”: 92
    }


Hier ist der Vorabversion Trigger mit der Anforderung ausgeführt werden soll, in der Kopfzeile x-ms-documentdb-pre-trigger-include angegeben. Alle nach der Trigger sind entsprechend in der Kopfzeile x-ms-documentdb-post-trigger-include angegeben. Beachten Sie, dass beide vor und nach der Trigger für eine bestimmte Anforderung angegeben werden können.

## <a name="sample-code"></a>Beispiel-code

Klicken Sie auf unsere [Github Repository](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples)finden Sie weitere serverseitigen Codebeispielen (einschließlich [Massen-löschen](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples/stored-procedures/bulkDelete.js)und [Aktualisieren](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples/stored-procedures/update.js)).

Möchten Sie Ihre großartig gespeicherte Prozedur freigeben? Senden Sie uns bitte, Anforderung eine Abruf! 

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie eine oder mehrere gespeicherte Prozeduren, Trigger und benutzerdefinierte Funktionen erstellt haben, können sie laden und in der Azure-Portal mit Skript-Explorer anzeigen können. Weitere Informationen finden Sie unter [Anzeigen von gespeicherten Prozeduren, Triggern, und benutzerdefinierte Funktionen, die mit dem DocumentDB Skript-Explorer](documentdb-view-scripts.md).

Auch kann die folgenden Verweise und Ressourcen in Ihrem Pfad erfahren Sie mehr über die serverseitige Programmierung DocumentDB hilfreich sein:

- [Azure DocumentDB SDKs](https://msdn.microsoft.com/library/azure/dn781482.aspx)
- [DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio/releases)
- [JSON](http://www.json.org/) 
- [JavaScript ECMA-262](http://www.ecma-international.org/publications/standards/Ecma-262.htm)
- [JavaScript – JSON-Typ-system](http://www.json.org/js.html) 
- [Sichere und Portable Datenbank Erweiterbarkeit](http://dl.acm.org/citation.cfm?id=276339) 
- [Service orientierte Datenbankarchitektur](http://dl.acm.org/citation.cfm?id=1066267&coll=Portal&dl=GUIDE) 
- [Der in Microsoft SQL Server hosten](http://dl.acm.org/citation.cfm?id=1007669)

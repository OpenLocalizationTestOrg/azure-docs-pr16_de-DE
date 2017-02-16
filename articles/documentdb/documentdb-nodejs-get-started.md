<properties
  pageTitle="NoSQL Node.js Lernprogramm für DocumentDB | Microsoft Azure"
  description="Ein NoSQL Node.js-Lernprogramm, die eine Knoten Datenbank und Console-Anwendung mit dem DocumentDB Node.js SDK erstellt wird. DocumentDB ist eine Datenbank NoSQL für JSON."
    keywords="Node.js Lernprogramm Knotendatenbank"
  services="documentdb"
  documentationCenter="node.js"
  authors="AndrewHoh"
  manager="jhubbard"
  editor="monicar"/>

<tags
  ms.service="documentdb"
  ms.workload="data-services"
  ms.tgt_pltfrm="na"
  ms.devlang="node"
  ms.topic="hero-article"
  ms.date="08/11/2016"
  ms.author="anhoh"/>

# <a name="nosql-nodejs-tutorial-documentdb-nodejs-console-application"></a>NoSQL Node.js Lernprogramm: DocumentDB Node.js Console-Anwendung  

> [AZURE.SELECTOR]
- [.NET](documentdb-get-started.md)
- [Node.js](documentdb-nodejs-get-started.md)

Willkommen Sie bei des Node.js Lernprogramms zum Microsoft Azure DocumentDB Node.js SDK! Nach dem Durcharbeiten dieses Lernprogramms, müssen Sie eine Console-Anwendung, die erstellt und Abfragen DocumentDB Ressourcen, einschließlich einer Knotendatenbank.

Wir behandeln:

- Erstellen und Herstellen einer Verbindung mit einem DocumentDB-Konto
- Einrichten Ihrer Anwendung
- Erstellen einer Knotendatenbank
- Erstellen einer Websitesammlungs
- Erstellen von JSON-Dokumenten
- Die Sammlung wird abgefragt
- Ersetzen eines Dokuments
- Löschen eines Dokuments
- Löschen die Knotendatenbank

Sie haben keine Zeit? Macht nichts! Die vollständige Lösung ist auf [GitHub](https://github.com/Azure-Samples/documentdb-node-getting-started)verfügbar. Schnelle Anweisungen finden Sie unter [erhalten Sie die vollständige Lösung](#GetSolution) .

Nachdem Sie das Lernprogramm Node.js abgeschlossen haben, verwenden Sie die Abstimmungsschaltflächen am oberen und unteren Seitenrand Sie uns Feedback geben. Wenn Sie uns, die Sie direkt in Verbindung setzen möchten, können Sie Ihre e-Mail-Adresse in Ihre Kommentare enthalten.

Jetzt kann's losgehen!

## <a name="prerequisites-for-the-nodejs-tutorial"></a>Voraussetzungen für die Node.js Lernprogramm

Stellen Sie sicher, dass Sie über Folgendes verfügen:

- Ein aktives Azure-Konto. Wenn Sie eine besitzen, können Sie sich für eine [Kostenlose Testversion von Azure](https://azure.microsoft.com/pricing/free-trial/)signieren.
- [Node.js](https://nodejs.org/) Version v0.10.29 oder höher.

## <a name="step-1-create-a-documentdb-account"></a>Schritt 1: Erstellen eines DocumentDB-Kontos

Erstellen Sie uns ein DocumentDB-Konto ein. Wenn Sie bereits über ein Konto, die Sie verwenden möchten verfügen, können Sie zum [Einrichten Ihrer Anwendungs Node.js](#SetupNode)springen.

[AZURE.INCLUDE [documentdb-create-dbaccount](../../includes/documentdb-create-dbaccount.md)]

## <a name="a-idsetupnodeastep-2-setup-your-nodejs-application"></a><a id="SetupNode"></a>Schritt 2: Einrichten Sie Ihrer Anwendung Node.js

1. Öffnen Sie Ihre bevorzugte Terminal.
2. Suchen Sie nach dem Ordner oder Verzeichnis, in dem Sie die Anwendung Node.js speichern möchten.
3. Erstellen Sie zwei leere JavaScript-Dateien mit der folgenden Befehle:
  - Windows:
      * ```fsutil file createnew app.js 0```
        * ```fsutil file createnew config.js 0```
  - Linux/OS x
      * ```touch app.js```
        * ```touch config.js```
4. Installieren Sie das Modul Documentdb über Npm an. Verwenden Sie den folgenden Befehl ein:
    * ```npm install documentdb --save```

Großartige! Jetzt, da Sie die Einrichtung abgeschlossen haben, beginnen wir mit entsprechendem Code schreiben.

## <a name="a-idconfigastep-3-set-your-apps-configurations"></a><a id="Config"></a>Schritt 3: Richten Sie Ihrer app Konfigurationen ein

Open ```config.js``` in Ihrem bevorzugten Texteditor.

Dann, kopieren und Einfügen den folgenden Codeausschnitt und Festlegen von Eigenschaften ```config.endpoint``` und ```config.primaryKey``` DocumentDB Endpunkt Uri und Primärschlüssel. Diese beiden Konfigurationen finden Sie in der [Azure-Portal](https://portal.azure.com).

![Node.js Lernprogramm - Screenshot der Azure-Portal mit einem Konto DocumentDB mit der aktiven Hub hervorgehoben, die Tasten Schaltfläche auf das DocumentDB Konto Blade hervorgehoben und die URI, primären und SEKUNDÄRSCHLÜSSEL Werte, die die Tasten Blade - Knotendatenbank hervorgehoben][keys]

    // ADD THIS PART TO YOUR CODE
    var config = {}

    config.endpoint = "~your DocumentDB endpoint uri here~";
    config.primaryKey = "~your primary key here~";

Kopieren und Einfügen der ```database id```, ```collection id```, und ```JSON documents``` zu Ihrem ```config``` Objekt darunter, in dem Sie festlegen, Ihrer ```config.endpoint``` und ```config.authKey``` Eigenschaften. Wenn Sie bereits Daten, die Sie in Ihrer Datenbank speichern möchten enthalten, können Sie DocumentDBs [Migrationstools Daten](documentdb-import-data.md) hinzufügen, sondern die Dokumentdefinitionen.

    config.endpoint = "~your DocumentDB endpoint uri here~";
    config.primaryKey = "~your primary key here~";

    // ADD THIS PART TO YOUR CODE
    config.database = {
        "id": "FamilyDB"
    };

    config.collection = {
        "id": "FamilyColl"
    };

    config.documents = {
        "Andersen": {
            "id": "Anderson.1",
            "lastName": "Andersen",
            "parents": [{
                "firstName": "Thomas"
            }, {
                    "firstName": "Mary Kay"
                }],
            "children": [{
                "firstName": "Henriette Thaulow",
                "gender": "female",
                "grade": 5,
                "pets": [{
                    "givenName": "Fluffy"
                }]
            }],
            "address": {
                "state": "WA",
                "county": "King",
                "city": "Seattle"
            }
        },
        "Wakefield": {
            "id": "Wakefield.7",
            "parents": [{
                "familyName": "Wakefield",
                "firstName": "Robin"
            }, {
                    "familyName": "Miller",
                    "firstName": "Ben"
                }],
            "children": [{
                "familyName": "Merriam",
                "firstName": "Jesse",
                "gender": "female",
                "grade": 8,
                "pets": [{
                    "givenName": "Goofy"
                }, {
                        "givenName": "Shadow"
                    }]
            }, {
                    "familyName": "Miller",
                    "firstName": "Lisa",
                    "gender": "female",
                    "grade": 1
                }],
            "address": {
                "state": "NY",
                "county": "Manhattan",
                "city": "NY"
            },
            "isRegistered": false
        }
    };


Die Datenbank, Websitesammlungen und Dokumentdefinitionen fungiert als Ihre DocumentDB ```database id```, ```collection id```, und die Dokumente.

Schließlich Exportieren Ihrer ```config``` Objekt, sodass Sie es in verweisen können die ```app.js``` Datei.

            },
            "isRegistered": false
        }
    };

    // ADD THIS PART TO YOUR CODE
    module.exports = config;

##<a name="a-idconnecta-step-4-connect-to-a-documentdb-account"></a><a id="Connect"></a>Schritt 4: Verbinden mit einem DocumentDB-Konto

Öffnen Sie Ihre leeren ```app.js``` Datei im Text-Editor. Kopieren und fügen Sie den Code unten, um das Importieren der ```documentdb``` Modul und der neu erstellten ```config``` Modul.

    // ADD THIS PART TO YOUR CODE
    "use strict";

    var documentClient = require("documentdb").DocumentClient;
    var config = require("./config");
    var url = require('url');

Kopieren und Einfügen des Codes zum Verwenden Sie die zuvor gespeicherte ```config.endpoint``` und ```config.primaryKey``` zum Erstellen einer neuen DocumentClient.

    var config = require("./config");
    var url = require('url');

    // ADD THIS PART TO YOUR CODE
    var client = new documentClient(config.endpoint, { "masterKey": config.primaryKey });

Jetzt, da Sie den Code zu Initialisierung des Documentdb Client haben, sehen wir uns bei der Arbeit mit DocumentDB Ressourcen.

## <a name="step-5-create-a-node-database"></a>Schritt 5: Erstellen einer Datenbank mit Knoten
Kopieren Sie und fügen Sie den Code unten, um die HTTP-Status für nicht gefunden, die Datenbank-Url und der Websitesammlungs-Url festgelegt. Diese Urls sind, wie der DocumentDB Client die richtigen Datenbanken und die Websitesammlung finden wird.

    var client = new documentClient(config.endpoint, { "masterKey": config.primaryKey });

    // ADD THIS PART TO YOUR CODE
    var HttpStatusCodes = { NOTFOUND: 404 };
    var databaseUrl = `dbs/${config.database.id}`;
    var collectionUrl = `${databaseUrl}/colls/${config.collection.id}`;

Eine [Datenbank](documentdb-resources.md#databases) kann mithilfe der Funktion [CreateDatabase](https://azure.github.io/azure-documentdb-node/DocumentClient.html) der Klasse **DocumentClient** erstellt werden. Eine Datenbank ist die logischen Container Dokument Speicher über Websitesammlungen aufgeteilt.

Kopieren und Einfügen die **GetDatabase** -Funktion für die neue Datenbank erstellen, in der Datei app.js mit den ```id``` angegebenen in der ```config``` Objekt. Die Funktion prüft, ob die Datenbank mit dem gleichen ```FamilyRegistry``` Id noch nicht vorhanden. Wenn es vorhanden ist, können wir dieser Datenbank anstelle der Erstellung einer neuen zurück.

    var collectionUrl = `${databaseUrl}/colls/${config.collection.id}`;

    // ADD THIS PART TO YOUR CODE
    function getDatabase() {
        console.log(`Getting database:\n${config.database.id}\n`);

        return new Promise((resolve, reject) => {
            client.readDatabase(databaseUrl, (err, result) => {
                if (err) {
                    if (err.code == HttpStatusCodes.NOTFOUND) {
                        client.createDatabase(config.database, (err, created) => {
                            if (err) reject(err)
                            else resolve(created);
                        });
                    } else {
                        reject(err);
                    }
                } else {
                    resolve(result);
                }
            });
        });
    }

Kopieren Sie und fügen Sie den Code unten in dem Sie festlegen der Funktion **GetDatabase** der Helper Funktion **Beenden** hinzufügen, die die Beendingungsnachricht und den Anruf an die Funktion **GetDatabase** gedruckt wird.

                } else {
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART TO YOUR CODE
    function exit(message) {
        console.log(message);
        console.log('Press any key to exit');
        process.stdin.setRawMode(true);
        process.stdin.resume();
        process.stdin.on('data', process.exit.bind(process, 0));
    }

    getDatabase()
    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

Suchen Sie in der Terminal Ihre ```app.js``` ablegen, und führen Sie den Befehl:```node app.js```

Herzlichen Glückwunsch! Sie haben eine Datenbank DocumentDB erfolgreich erstellt.

##<a name="a-idcreatecollastep-6-create-a-collection"></a><a id="CreateColl"></a>Schritt 6: Erstellen einer Websitesammlung  

> [AZURE.WARNING] **CreateDocumentCollectionAsync** erstellt eine neue Websitesammlung, die Preise Auswirkungen hat. Weitere Informationen hierzu finden Sie auf unserer [Seite Preise](https://azure.microsoft.com/pricing/details/documentdb/).

Eine [Auflistung](documentdb-resources.md#collections) kann mithilfe der Funktion [CreateCollection](https://azure.github.io/azure-documentdb-node/DocumentClient.html) der Klasse **DocumentClient** erstellt werden. Eine Auflistung ist ein Container für JSON-Dokumente und zugehörigen JavaScript-Anwendungslogik.

Kopieren und Einfügen von der Funktion **GetCollection** unterhalb der **GetDatabase** -Funktion für Ihre neue Websitesammlung mit Erstellen der ```id``` angegebenen in der ```config``` Objekt. Erneut, wir werden Vergewissern Sie sich eine Auflistung mit demselben ```FamilyCollection``` Id noch nicht vorhanden. Wenn es vorhanden ist, können wir dieser Sammlung statt Erstellen einer neuen zurück.

                } else {
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART TO YOUR CODE
    function getCollection() {
        console.log(`Getting collection:\n${config.collection.id}\n`);

        return new Promise((resolve, reject) => {
            client.readCollection(collectionUrl, (err, result) => {
                if (err) {
                    if (err.code == HttpStatusCodes.NOTFOUND) {
                        client.createCollection(databaseUrl, config.collection, { offerThroughput: 400 }, (err, created) => {
                            if (err) reject(err)
                            else resolve(created);
                        });
                    } else {
                        reject(err);
                    }
                } else {
                    resolve(result);
                }
            });
        });
    }

Kopieren Sie und fügen Sie den Code unter den Anruf an **GetDatabase** zum Ausführen der Funktion **GetCollection** .

    getDatabase()

    // ADD THIS PART TO YOUR CODE
    .then(() => getCollection())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

Suchen Sie in der Terminal Ihre ```app.js``` ablegen, und führen Sie den Befehl:```node app.js```

Herzlichen Glückwunsch! Sie haben eine Auflistung DocumentDB erfolgreich erstellt.

##<a name="a-idcreatedocastep-7-create-a-document"></a><a id="CreateDoc"></a>Schritt 7: Erstellen eines Dokuments
Ein [Dokument](documentdb-resources.md#documents) kann mithilfe der Funktion [CreateDocument](https://azure.github.io/azure-documentdb-node/DocumentClient.html) der Klasse **DocumentClient** erstellt werden. Dokumente sind, dass Benutzer (beliebigen) JSON-Inhalt definiert. Sie können nun ein Dokument in DocumentDB einfügen.

Kopieren und Einfügen von der Funktion **GetFamilyDocument** unterhalb der Funktion **GetCollection** zum Erstellen der Dokumente mit den JSON-Daten gespeichert, die der ```config``` Objekt. Wir wird erneut überprüfen, um sicherzustellen, dass ein Dokument mit der gleichen Id noch nicht vorhanden ist.

                } else {
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART TO YOUR CODE
    function getFamilyDocument(document) {
        let documentUrl = `${collectionUrl}/docs/${document.id}`;
        console.log(`Getting document:\n${document.id}\n`);

        return new Promise((resolve, reject) => {
            client.readDocument(documentUrl, { partitionKey: document.district }, (err, result) => {
                if (err) {
                    if (err.code == HttpStatusCodes.NOTFOUND) {
                        client.createDocument(collectionUrl, document, (err, created) => {
                            if (err) reject(err)
                            else resolve(created);
                        });
                    } else {
                        reject(err);
                    }
                } else {
                    resolve(result);
                }
            });
        });
    };

Kopieren Sie und fügen Sie den Code unter den Anruf an **GetCollection** zum Ausführen der Funktion **GetFamilyDocument** .

    getDatabase()
    .then(() => getCollection())

    // ADD THIS PART TO YOUR CODE
    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

Suchen Sie in der Terminal Ihre ```app.js``` ablegen, und führen Sie den Befehl:```node app.js```

Herzlichen Glückwunsch! Sie haben eine DocumentDB Dokumente erfolgreich erstellt.

![Lernprogramm Node.js - veranschaulichen die hierarchische Beziehung zwischen dem Konto, die Datenbank, die Sammlung und Dokumente - Diagrammknoten-Datenbank](./media/documentdb-nodejs-get-started/node-js-tutorial-account-database.png)

##<a name="a-idqueryastep-8-query-documentdb-resources"></a><a id="Query"></a>Schritt 8: Abfrage DocumentDB Ressourcen

DocumentDB unterstützt [Rich-Abfragen](documentdb-sql-query.md) für JSON-Dokumente, die in jeder Websitesammlung gespeichert. Der folgende Code zeigt eine Abfrage, die Sie für die Dokumente in Ihrer Websitesammlung ausführen können.

Kopieren und Einfügen von der Funktion **QueryCollection** unterhalb der **GetFamilyDocument** -Funktion. DocumentDB unterstützt SQL-ähnliche Abfragen aus, wie unten dargestellt. Weitere Informationen darüber, wie Sie komplexe Abfragen erstellen schauen Sie sich den [Abfrage-Umgebung](https://www.documentdb.com/sql/demo) und der [abfragedokumentation](documentdb-sql-query.md).

                } else {
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART TO YOUR CODE
    function queryCollection() {
        console.log(`Querying collection through index:\n${config.collection.id}`);

        return new Promise((resolve, reject) => {
            client.queryDocuments(
                collectionUrl,
                'SELECT VALUE r.children FROM root r WHERE r.lastName = "Andersen"'
            ).toArray((err, results) => {
                if (err) reject(err)
                else {
                    for (var queryResult of results) {
                        let resultString = JSON.stringify(queryResult);
                        console.log(`\tQuery returned ${resultString}`);
                    }
                    console.log();
                    resolve(results);
                }
            });
        });
    };


Das folgende Diagramm veranschaulicht, wie die DocumentDB SQL-Abfragesyntax gegen die Sammlung Sie aufgerufen wird erstellt.

![Lernprogramm Node.js - zu veranschaulichen des Gültigkeitsbereichs und was bedeutet der Abfrage - Diagrammknoten-Datenbank](./media/documentdb-nodejs-get-started/node-js-tutorial-collection-documents.png)

Das [FROM](documentdb-sql-query.md#from-clause) -Schlüsselwort ist optional in der Abfrage, da DocumentDB Abfragen bereits auf eine einzelne Sammlung beschränkt sind. Daher "Von Familien f" können werden durch ausgetauscht "Von Root R" oder eine anderen Variablen benennen Sie wählen Sie aus. DocumentDB wird ein abzuleiten dieser Familien, Stamm oder dem Variablennamen, die, den Sie ausgewählt haben, die aktuelle Auflistung standardmäßig verweisen.

Kopieren Sie und fügen Sie den Code unter den Anruf an **GetFamilyDocument** zum Ausführen der Funktion **QueryCollection** .

    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))

    // ADD THIS PART TO YOUR CODE
    .then(() => queryCollection())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

Suchen Sie in der Terminal Ihre ```app.js``` ablegen, und führen Sie den Befehl:```node app.js```

Herzlichen Glückwunsch! Sie haben erfolgreich DocumentDB Dokumente abgefragt.

##<a name="a-idreplacedocumentastep-9-replace-a-document"></a><a id="ReplaceDocument"></a>Schritt 9: Ersetzen eines Dokuments
DocumentDB unterstützt das JSON-Dokumente austauschen.

Kopieren und Einfügen von der Funktion **ReplaceDocument** unterhalb der **QueryCollection** -Funktion.

                    }
                    console.log();
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART TO YOUR CODE
    function replaceFamilyDocument(document) {
        let documentUrl = `${collectionUrl}/docs/${document.id}`;
        console.log(`Replacing document:\n${document.id}\n`);
        document.children[0].grade = 6;

        return new Promise((resolve, reject) => {
            client.replaceDocument(documentUrl, document, (err, result) => {
                if (err) reject(err);
                else {
                    resolve(result);
                }
            });
        });
    };

Kopieren Sie und fügen Sie den Code unter den Anruf an **QueryCollection** zum Ausführen der Funktion **ReplaceDocument** . Darüber hinaus Hinzufügen des Codes zum **QueryCollection** erneut aufrufen, um sicherzustellen, dass das Dokument erfolgreich geändert wurde.

    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))
    .then(() => queryCollection())

    // ADD THIS PART TO YOUR CODE
    .then(() => replaceFamilyDocument(config.documents.Andersen))
    .then(() => queryCollection())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

Suchen Sie in der Terminal Ihre ```app.js``` ablegen, und führen Sie den Befehl:```node app.js```

Herzlichen Glückwunsch! Sie haben ein Dokument DocumentDB erfolgreich ersetzt.

##<a name="a-iddeletedocumentastep-10-delete-a-document"></a><a id="DeleteDocument"></a>Schritt 10: Löschen eines Dokuments
DocumentDB unterstützt das JSON-Dokumente löschen.

Kopieren und Einfügen von der Funktion **DeleteDocument** unterhalb der **ReplaceDocument** -Funktion.

                else {
                    resolve(result);
                }
            });
        });
    };

    // ADD THIS PART TO YOUR CODE
    function deleteFamilyDocument(document) {
        let documentUrl = `${collectionUrl}/docs/${document.id}`;
        console.log(`Deleting document:\n${document.id}\n`);

        return new Promise((resolve, reject) => {
            client.deleteDocument(documentUrl, (err, result) => {
                if (err) reject(err);
                else {
                    resolve(result);
                }
            });
        });
    };

Kopieren Sie und fügen Sie den Code unter den Anruf an die zweite **QueryCollection** zum Ausführen der Funktion **DeleteDocument** .

    .then(() => queryCollection())
    .then(() => replaceFamilyDocument(config.documents.Andersen))
    .then(() => queryCollection())

    // ADD THIS PART TO YOUR CODE
    .then(() => deleteFamilyDocument(config.documents.Andersen))
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

Suchen Sie in der Terminal Ihre ```app.js``` ablegen, und führen Sie den Befehl:```node app.js```

Herzlichen Glückwunsch! Sie haben ein Dokument DocumentDB erfolgreich gelöscht.

##<a name="a-iddeletedatabaseastep-11-delete-the-node-database"></a><a id="DeleteDatabase"></a>Schritt 11: Löschen der Datenbank Knoten

Löschen die Datenbank erstellte werden die Datenbank und alle untergeordneten Elementen Ressourcen (Websitesammlungen, Dokumente usw.) entfernt.

Kopieren Sie und fügen Sie der folgenden Codeausschnitt (Funktion **zum Aufräumen**) So entfernen Sie die Datenbank und alle untergeordneten Elementen Ressourcen ein.

                else {
                    resolve(result);
                }
            });
        });
    };

    // ADD THIS PART TO YOUR CODE
    function cleanup() {
        console.log(`Cleaning up by deleting database ${config.database.id}`);

        return new Promise((resolve, reject) => {
            client.deleteDatabase(databaseUrl, (err) => {
                if (err) reject(err)
                else resolve(null);
            });
        });
    }

Kopieren Sie und fügen Sie den Code unter den Anruf an **DeleteDocument** zum Ausführen der Funktion **zum Aufräumen** .

    .then(() => deleteFamilyDocument(config.documents.Andersen))

    // ADD THIS PART TO YOUR CODE
    .then(() => cleanup())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

##<a name="a-idrunastep-12-run-your-nodejs-application-all-together"></a><a id="Run"></a>Schritt 12: Ausführen Ihrer Node.js Anwendung alles zusammen!

Ganz, sollte die Sequenz für das Anrufen von Ihrer Funktionen wie folgt aussehen:

    getDatabase()
    .then(() => getCollection())
    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))
    .then(() => queryCollection())
    .then(() => replaceFamilyDocument(config.documents.Andersen))
    .then(() => queryCollection())
    .then(() => deleteFamilyDocument(config.documents.Andersen))
    .then(() => cleanup())
    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

Suchen Sie in der Terminal Ihre ```app.js``` ablegen, und führen Sie den Befehl:```node app.js```

Die Ausgabe der erste Schritte app sollte angezeigt werden. Die Ausgabe sollte der folgenden Beispieltext übereinstimmen.

    Getting database:
    FamilyDB

    Getting collection:
    FamilyColl

    Getting document:
    Anderson.1

    Getting document:
    Wakefield.7

    Querying collection through index:
    FamilyColl
        Query returned [{"firstName":"Henriette Thaulow","gender":"female","grade":5,"pets":[{"givenName":"Fluffy"}]}]

    Replacing document:
    Anderson.1

    Querying collection through index:
    FamilyColl
        Query returned [{"firstName":"Henriette Thaulow","gender":"female","grade":6,"pets":[{"givenName":"Fluffy"}]}]

    Deleting document:
    Anderson.1

    Cleaning up by deleting database FamilyDB
    Completed successfully
    Press any key to exit

Herzlichen Glückwunsch! Sie haben erstellt, Sie haben das Lernprogramm Node.js abgeschlossen und haben Sie Ihre erste DocumentDB Console-Anwendung!

## <a name="a-idgetsolutionaget-the-complete-nodejs-tutorial-solution"></a><a id="GetSolution"></a>Erhalten Sie die vollständige zusammengehörenden Node.js-Lösung
Wenn Sie die GetStarted-Lösung erstellen, die alle in den Beispielen in diesem Artikel enthält, benötigen Sie Folgendes:

-   [DocumentDB Konto][documentdb-create-account].
-   Die [GetStarted](https://github.com/Azure-Samples/documentdb-node-getting-started) Lösung auf GitHub zur Verfügung.

Installieren Sie das Modul **Documentdb** über Npm an. Verwenden Sie den folgenden Befehl ein:
* ```npm install documentdb --save```

Nächste, in der ```config.js``` ablegen, aktualisieren Sie die Werte config.endpoint und config.authKey in beschriebenen [Schritt 3: Einrichten Ihrer app Konfigurationen](#Config).

## <a name="next-steps"></a>Nächste Schritte

-   Möchten Sie eine komplexere Node.js Stichprobe? Finden Sie unter [erstellen eine Node.js Webanwendung DocumentDB verwenden](documentdb-nodejs-application.md).
-  Erfahren Sie, wie [Monitor ein Konto DocumentDB](documentdb-monitor-accounts.md).
-  Unser Dataset Beispiel, in dem [Abfrage-Umgebung](https://www.documentdb.com/sql/demo)-Abfragen ausführen.
-  Weitere Informationen zum Modell Programmierung im Abschnitt Entwicklung der [DocumentDB Dokumentationsseite](https://azure.microsoft.com/documentation/services/documentdb/).

[documentdb-create-account]: documentdb-create-account.md
[documentdb-manage]: documentdb-manage.md

[keys]: media/documentdb-nodejs-get-started/node-js-tutorial-keys.png

<properties
    pageTitle="So verwenden Sie Azure Tabellenspeicher von Node.js | Microsoft Azure"
    description="Speichern Sie strukturierte Daten in der Cloud Azure Tabellenspeicher, einen Datenspeicher NoSQL verwenden."
    services="storage"
    documentationCenter="nodejs"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="tamram"/>


# <a name="how-to-use-azure-table-storage-from-nodejs"></a>So verwenden Sie Azure Tabellenspeicher von Node.js

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>(Übersicht)

In diesem Thema wird gezeigt, wie häufige Szenarien mithilfe von Service Azure-Tabelle in eine Anwendung Node.js ausführen.

Die Codebeispielen in diesem Thema wird davon ausgegangen, dass Sie bereits eine Anwendung Node.js. Informationen darüber, wie Sie eine Anwendung Node.js in Azure erstellen finden Sie unter folgenden Themen:

- [Erstellen Sie eine Node.js Web app in Azure-App-Verwaltungsdienst](../app-service-web/web-sites-nodejs-develop-deploy-mac.md)
- [Erstellen und Bereitstellen einer Node.js Web app Azure mithilfe von WebMatrix](../app-service-web/web-sites-nodejs-use-webmatrix.md)
- [Erstellen und Bereitstellen eine Anwendung Node.js zu Azure-Cloud-Dienst](../cloud-services/cloud-services-nodejs-develop-deploy-app.md) (mithilfe von Windows PowerShell)


[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]


## <a name="configure-your-application-to-access-azure-storage"></a>Konfigurieren der Anwendungs Zugriff auf Azure-Speicher

Wenn Azure-Speicher verwenden möchten, benötigen Sie den Azure-Speicher SDK für Node.js, die eine Reihe von Komfort Bibliotheken enthält, die mit der Speicher REST-Dienste kommunizieren an.

### <a name="use-node-package-manager-npm-to-install-the-package"></a>Verwenden Sie zum Installieren des Pakets Knoten Paket Manager (NPM)

1.  Verwenden einer Line-Schnittstelle wie **PowerShell** (Windows), **Terminal** (Mac) oder **Bash** (Unix), und navigieren Sie zu dem Ordner, in dem Sie die Anwendung erstellt.

2.  Geben Sie im Befehlsfenster **Npm installieren Azure-Speicher** . Ausgabe des Befehls ist ähnlich wie im folgenden Beispiel.

        azure-storage@0.5.0 node_modules\azure-storage
        +-- extend@1.2.1
        +-- xmlbuilder@0.4.3
        +-- mime@1.2.11
        +-- node-uuid@1.4.3
        +-- validator@3.22.2
        +-- underscore@1.4.4
        +-- readable-stream@1.0.33 (string_decoder@0.10.31, isarray@0.0.1, inherits@2.0.1, core-util-is@1.0.1)
        +-- xml2js@0.2.7 (sax@0.5.2)
        +-- request@2.57.0 (caseless@0.10.0, aws-sign2@0.5.0, forever-agent@0.6.1, stringstream@0.0.4, oauth-sign@0.8.0, tunnel-agent@0.4.1, isstream@0.1.2, json-stringify-safe@5.0.1, bl@0.9.4, combined-stream@1.0.5, qs@3.1.0, mime-types@2.0.14, form-data@0.2.0, http-signature@0.11.0, tough-cookie@2.0.0, hawk@2.3.1, har-validator@1.8.0)

3.  Sie können manuell ausführen des Befehls **ls** zu überprüfen, ob ein **Knoten\_Module** Ordner erstellt wurde. In diesem Ordner finden Sie das Paket **Azure-Speicher** , das die Bibliotheken enthält, die Sie Zugriff auf Speicher müssen.

### <a name="import-the-package"></a>Das Paket importieren

Fügen Sie den folgenden Code an den Anfang der Datei **server.js** in der Anwendung:

    var azure = require('azure-storage');

## <a name="set-up-an-azure-storage-connection"></a>Einrichten einer Verbindung Azure-Speicher

Das Modul Azure liest die Umgebungsvariablen AZURE\_Speicher\_-Konto und AZURE\_Speicher\_ACCESS\_Schlüssel oder AZURE\_Speicher\_Verbindung\_Zeichenfolge Informationen erforderlich, um eine Verbindung mit Ihrem Konto Azure-Speicher. Wenn dieser Variablen nicht festgelegt werden, müssen Sie die Kontoinformationen angeben, wenn Sie **TableService**aufrufen.

Ein Beispiel für das Einrichten der Umgebungsvariablen im [Portal Azure](https://portal.azure.com) für eine Website Azure finden Sie unter [Node.js Web app mithilfe des Diensts für Azure Tabelle].

## <a name="create-a-table"></a>Erstellen einer Tabelle

Mit dem folgende Code ein **TableService** Objekt erstellt und zum Erstellen einer neuen Tabelle verwendet wird. Fügen Sie die folgenden am oberen Rand der **server.js**hinzu.

    var tableSvc = azure.createTableService();

Der Anruf an **CreateTableIfNotExists** wird mit dem angegebenen Namen eine neue Tabelle erstellen, wenn es nicht bereits vorhanden ist. Im folgenden Beispiel wird eine neue Tabelle mit dem Namen 'Mytable', wenn es nicht bereits vorhanden ist:

    tableSvc.createTableIfNotExists('mytable', function(error, result, response){
      if(!error){
        // Table exists or created
      }
    });

Die `result.created` werden `true` , wenn eine neue Tabelle erstellt wurde, und `false` , wenn die Tabelle bereits vorhanden ist. Die `response` werden Informationen über die Anforderung enthalten.

### <a name="filters"></a>Filter

Optionale Filtervorgängen können auf Operationen mit **TableService**angewendet werden. Filtervorgängen kann einbeziehen Protokollierung, automatisch wiederholen, usw. Filter sind Objekte, die eine Methode mit der Signatur implementieren:

    function handle (requestOptions, next)

Nach deren preprocessing auf die Optionen für die Anforderung ausführen, muss die Methode "Weiter", rufen Sie einen Rückruf mit der folgenden Signatur übergeben:

    function (returnObject, finalCallback, next)

In diesem Rückruf und nach der Verarbeitung der ReturnObject (der Antwort aus der Anforderung an den Server) muss der Rückruf entweder als Nächstes aufrufen, falls vorhanden, damit andere Filter Verarbeitung fortzusetzen oder einfach FinalCallback andernfalls zum Beenden der Dienst aufrufen aufzurufen.

Zwei Filter, die "Wiederholen" Logik implementieren sind im Azure SDK für Node.js, **ExponentialRetryPolicyFilter** und **LinearRetryPolicyFilter**enthalten. Die folgenden erstellt ein **TableService** -Objekt, das die **ExponentialRetryPolicyFilter**verwendet:

    var retryOperations = new azure.ExponentialRetryPolicyFilter();
    var tableSvc = azure.createTableService().withFilter(retryOperations);

## <a name="add-an-entity-to-a-table"></a>Hinzufügen einer Entität zu einer Tabelle

Wenn ein Element hinzufügen möchten, erstellen Sie zuerst ein Objekt, Ihre Entitätseigenschaften definiert. Eine **PartitionKey** und eine **RowKey**, die eindeutigen Bezeichner für die Entität sind, müssen alle Elemente enthalten.

* **PartitionKey** – bestimmt die Partition, der in die Entität gespeichert ist

* **RowKey** - identifiziert die Entität innerhalb der partition

Sowohl die **PartitionKey** **RowKey** muss Zeichenfolgenwerte sein. Weitere Informationen finden Sie unter [Grundlegendes zu Tabelle Dienst Datenmodell](http://msdn.microsoft.com/library/azure/dd179338.aspx).

So sieht ein Beispiel für eine Entität definieren. Hinweis dieses **Beispiel** als einen Typ von **Edm.DateTime**definiert ist. Angeben des Typs ist optional und Datentypen werden werden abgeleitet, falls nicht angegeben.

    var task = {
      PartitionKey: {'_':'hometasks'},
      RowKey: {'_': '1'},
      description: {'_':'take out the trash'},
      dueDate: {'_':new Date(2015, 6, 20), '$':'Edm.DateTime'}
    };

> [AZURE.NOTE] Es gibt auch ein **Timestamp** -Feld für jeden Datensatz, die von Azure festgelegt ist, wenn eine Entität eingefügt oder aktualisiert wird.

Die **EntityGenerator** können Sie auch um Personen zu erstellen. Im folgende Beispiel wird die betreffende Aufgabenentität mithilfe der **EntityGenerator**erstellt.

    var entGen = azure.TableUtilities.entityGenerator;
    var task = {
      PartitionKey: entGen.String('hometasks'),
      RowKey: entGen.String('1'),
      description: entGen.String('take out the trash'),
      dueDate: entGen.DateTime(new Date(Date.UTC(2015, 6, 20))),
    };

Um Ihrer Tabelle eine Entität hinzuzufügen, übergeben Sie das Entitätsobjekt an die **InsertEntity** -Methode aus.

    tableSvc.insertEntity('mytable',task, function (error, result, response) {
      if(!error){
        // Entity inserted
      }
    });

Wenn der Vorgang erfolgreich ist, ist `result` wird das [ETag](http://en.wikipedia.org/wiki/HTTP_ETag) des eingefügten Datensatzes enthalten und `response` enthält Informationen über den Vorgang.

Beispielantwort:

    { '.metadata': { etag: 'W/"datetime\'2015-02-25T01%3A22%3A22.5Z\'"' } }

> [AZURE.NOTE] Standardmäßig **InsertEntity** kehrt die eingefügte Entität als Teil der `response` Informationen. Wenn Sie beabsichtigen, Ausführen von anderen Vorgängen auf diese Entität oder die Informationen im cache möchten, es kann nützlich sein, dass es zurückgegeben als Teil der `result`. Sie können dazu **EchoContent** wie folgt zu aktivieren:
>
> `tableSvc.insertEntity('mytable', task, {echoContent: true}, function (error, result, response) {...}`

## <a name="update-an-entity"></a>Aktualisieren einer Entität

Es gibt mehrere Methoden zum Aktualisieren einer vorhandenen Entität zur Verfügung:

* **ReplaceEntity** - aktualisiert eine vorhandene Entität, indem Sie ihn ersetzen

* **MergeEntity** - aktualisiert eine vorhandene Entität durch neue Eigenschaftswerte in die vorhandene Entität zusammenführen

* **InsertOrReplaceEntity** - aktualisiert eine vorhandene Entität durch diesen zu ersetzen. Wenn keine Entität vorhanden ist, wird eine neue eingefügt werden

* **InsertOrMergeEntity** - aktualisiert eine vorhandene Entität durch neue Eigenschaftswerte in den vorhandenen zusammenführen. Wenn keine Entität vorhanden ist, wird eine neue eingefügt werden

Im folgende Beispiel wird veranschaulicht, Aktualisieren einer Entität **ReplaceEntity**verwenden:

    tableSvc.replaceEntity('mytable', updatedTask, function(error, result, response){
      if(!error) {
        // Entity updated
      }
    });

> [AZURE.NOTE] Standardmäßig überprüft Aktualisieren einer Entität nicht um festzustellen, ob die Daten in der Aktualisierung zuvor von einem anderen Prozess geändert wurden. Parallele Updates unterstützen:
>
> 1. Rufen Sie den ETag des Objekts aktualisiert wird ab. Dies wird zurückgegeben, als Teil der `response` für eine beliebige Entität Vorgang Zusammenhang und über abgerufen werden kann `response['.metadata'].etag`.
>
> 2. Beim Durchführen eines Aktualisierungsvorgangs auf einem Element fügen Sie die ETag Informationen, die zuvor auf die neue Entität abgerufen hinzu. Beispiel:
>
>     `entity2['.metadata'].etag = currentEtag;`
>
> 3. Führen Sie den Aktualisierungsvorgang. Wenn die Entität geändert wurde, da ETag-Wert, wie etwa einer anderen Instanz der Anwendung wiederhergestellte, ein `error` zurückgegeben, die besagt, dass die in der Anforderung angegebene Update-Bedingung nicht erfüllt wurde.

Mit **ReplaceEntity** und **MergeEntity**Wenn die Person, die aktualisiert wird, die nicht vorhanden ist, fehl klicken Sie dann der Aktualisierungsvorgang. Daher wird, wenn Sie eine Entität unabhängig von speichern möchten, ob es bereits vorhanden ist, verwenden Sie **InsertOrReplaceEntity** oder **InsertOrMergeEntity**.

Die `result` für die Aktualisierung erfolgreich Vorgänge werden das **Etag** der aktualisierten Entität enthalten.

## <a name="work-with-groups-of-entities"></a>Arbeiten Sie mit Gruppen von Personen

Manchmal ist es sinnvoll übermitteln Sie mehrere Vorgänge zusammen in einem Stapel um atomare Verarbeitung vom Server sicherzustellen. Um die zu erreichen, verwenden Sie die **TableBatch** -Klasse um zu erstellen, und verwenden Sie dann die **ExecuteBatch** -Methode der **TableService** , um die gespeicherten Vorgänge ausführen.

 Das folgende Beispiel veranschaulicht zwei Elemente in einem Stapel senden:

    var task1 = {
      PartitionKey: {'_':'hometasks'},
      RowKey: {'_': '1'},
      description: {'_':'Take out the trash'},
      dueDate: {'_':new Date(2015, 6, 20)}
    };
    var task2 = {
      PartitionKey: {'_':'hometasks'},
      RowKey: {'_': '2'},
      description: {'_':'Wash the dishes'},
      dueDate: {'_':new Date(2015, 6, 20)}
    };

    var batch = new azure.TableBatch();

    batch.insertEntity(task1, {echoContent: true});
    batch.insertEntity(task2, {echoContent: true});

    tableSvc.executeBatch('mytable', batch, function (error, result, response) {
      if(!error) {
        // Batch completed
      }
    });

Für Vorgänge erfolgreich Stapel `result` enthält Informationen für jeden Vorgang in den Stapel.

### <a name="work-with-batched-operations"></a>Arbeiten mit zusammengefasst Vorgänge

Vorgänge zu einem Stapel hinzugefügt, die von der Anzeige überprüft werden können die `operations` Eigenschaft. Sie können auch die folgenden Methoden verwenden, für die Arbeit mit Vorgänge:

* **Löschen** - löscht alle Vorgänge aus einem Stapel

* **GetOperations** - Ruft einen Vorgang aus dem Stapel ab.

* **HasOperations** - Gibt WAHR zurück, wenn der Stapel Vorgänge enthält.

* **RemoveOperations** - entfernt einen Vorgang

* **Größe** - gibt die Anzahl der Vorgänge im Stapel

## <a name="retrieve-an-entity-by-key"></a>Abrufen einer Entität, indem Sie Schlüssel

Verwenden Sie die **RetrieveEntity** -Methode, um eine bestimmte Entität basierend auf dem **PartitionKey** und **RowKey**zurückzukehren.

    tableSvc.retrieveEntity('mytable', 'hometasks', '1', function(error, result, response){
      if(!error){
        // result contains the entity
      }
    });

Nach Abschluss dieses Vorgangs `result` wird die Entität enthalten.

## <a name="query-a-set-of-entities"></a>Abfrage eine Reihe von Personen

Eine Tabelle abgefragt werden soll, verwenden Sie das Objekt **TableQuery** , um eine Abfrageausdruck mit den folgenden Klauseln zu erstellen:

* **Wählen Sie aus** – die Felder, die von der Abfrage zurückgegeben werden

* **wo** – der Where-Klausel

    * **und** - eine `and` Bedingung

    * **oder** – ein `or` Bedingung

* **Top** - die Anzahl von Elementen, die abgerufen werden sollen


Im folgende Beispiel wird eine Abfrage, die die oberen fünf Elemente mit einem PartitionKey von 'Hometasks' zurückgeben wird erstellt.

    var query = new azure.TableQuery()
      .top(5)
      .where('PartitionKey eq ?', 'hometasks');

Da **Wählen** nicht verwendet wird, werden alle Felder zurückgegeben. Verwenden Sie zum Ausführen der Abfrage eine Tabelle **QueryEntities**ein. Im folgende Beispiel wird diese Abfrage zurückzugebenden Elemente aus 'Mytable' verwendet.

    tableSvc.queryEntities('mytable',query, null, function(error, result, response) {
      if(!error) {
        // query was successful
      }
    });

Bei einem erfolgreichen Abschluss `result.entries` enthält Sie ein Array von Elementen, die die Abfrage entsprechen. Wenn die Abfrage nicht alle Elemente zurückgegeben wurde `result.continuationToken` ist nicht*null* und als dritten Parameter der **QueryEntities** verwendet werden können, um weitere Ergebnisse abzurufen. Verwenden Sie bei der anfänglichen Abfrage *null* für den dritten Parameter ein.

### <a name="query-a-subset-of-entity-properties"></a>Abfrage eine Teilmenge der Entitätseigenschaften

Eine Abfrage in einer Tabelle kann ein Paar Felder aus einer Entität abrufen.
Dies reduziert Bandbreite und kann die Leistung von Abfragen, vor allem für große Personen verbessern. Verwenden der **select** -Klausel und übergeben die Namen der Felder, die zurückgegeben werden soll. Die folgende Abfrage gibt beispielsweise nur die Felder **Beschreibung** und **Beispiel** zurück.

    var query = new azure.TableQuery()
      .select(['description', 'dueDate'])
      .top(5)
      .where('PartitionKey eq ?', 'hometasks');

## <a name="delete-an-entity"></a>Löschen einer Entität

Sie können eine Entität mithilfe ihrer Schlüssel Partition und Zeile löschen. In diesem Beispiel enthält das Objekt **task1** die Werte **RowKey** und **PartitionKey** der Entität, die gelöscht werden. Dann wird das Objekt an die Methode **DeleteEntity** übergeben.

    var task = {
      PartitionKey: {'_':'hometasks'},
      RowKey: {'_': '1'}
    };

    tableSvc.deleteEntity('mytable', task, function(error, response){
      if(!error) {
        // Entity deleted
      }
    });

> [AZURE.NOTE] Erwägen Sie ETags beim Löschen von Elementen, um sicherzustellen, dass das Element nicht von einem anderen Prozess geändert wurde. Informationen zur Verwendung von ETags finden Sie unter [Aktualisieren eine Entität](#update-an-entity) .

## <a name="delete-a-table"></a>Löschen einer Tabelle

Mit dem folgende Code Löscht eine Tabelle aus einer Speicher-Konto an.

    tableSvc.deleteTable('mytable', function(error, response){
        if(!error){
            // Table deleted
        }
    });

Wenn Sie unsicher sind, ob die Tabelle vorhanden ist, verwenden Sie **DeleteTableIfExists**.

## <a name="use-continuation-tokens"></a>Fortsetzung Token verwenden

Wenn Sie Tabellen für große Mengen von Ergebnisse Abfragen, suchen Sie nach Fortsetzung Token. Es können große Mengen von Daten für die Abfrage, die Sie möglicherweise nicht einreichen Wenn Erstellung nicht erkannt, wenn ein Fortsetzungstoken vorhanden ist verfügbar sein.

Die Ergebnisse Objekt während Einheiten Sätze Abfragen zurückgegeben einer `continuationToken` Eigenschaft, wenn ein solcher Token vorhanden ist. Klicken Sie dann können diese beim Ausführen einer Abfrage weiterhin über die Partition und Tabelle Elemente zu verschieben.

Bei der Abfrage möglicherweise ContinuationToken Parameter zwischen der Abfrage Objekt-Instanz und die Rückruffunktion bereitgestellt werden:

```
var nextContinuationToken = null;
dc.table.queryEntities(tableName,
    query,
    nextContinuationToken,
    function (error, results) {
        if (error) throw error;

        // iterate through results.entries with results

        if (results.continuationToken) {
            nextContinuationToken = results.continuationToken;
        }

    });
```

Wenn Sie untersuchen der `continuationToken` Objekt, finden Sie Eigenschaften wie `nextPartitionKey`, `nextRowKey` und `targetLocation`, welche können verwendet werden, um alle Ergebnisse durchlaufen.

Es gibt auch eine Stichprobe Fortsetzung innerhalb der Azure-Speicher Node.js Repo auf GitHub. Suchen Sie nach `examples/samples/continuationsample.js`.

## <a name="work-with-shared-access-signatures"></a>Arbeiten Sie mit freigegebenen Access Signaturen

Freigegebene Access Signaturen (SAS) werden auf sichere Weise zu präzise Zugriff auf Tabellen zu ermöglichen, ohne Angabe Ihrer Speicher Kontonamen oder die Tasten. SAS werden häufig verwendet, eingeschränkter Zugriff auf Ihre Daten, wie z. B. eine mobile app Abfrage Datensätze zulassen.

Eine vertrauenswürdige Anwendung wie etwa einen cloudbasierten Dienst generiert eine SAS mithilfe der **GenerateSharedAccessSignature** von den **TableService**und stellt es in einer nicht vertrauenswürdigen oder teilweise vertrauenswürdige Anwendung wie mobile-app. Die SAS wird generiert mithilfe einer Richtlinie, die die Anfangs- und Endtermine beschreibt, bei denen die SAS ungültig ist, als auch die Zugriffsebene für den SAS Inhaber erteilt.

Im folgenden Beispiel wird eine neue freigegebenen Zugriffsrichtlinie, bei dem den Inhaber SAS Abfrage ('R') in der Tabelle wird generiert und läuft ab 100 Minuten nach der Zeit, die sie erstellt wurde.

    var startDate = new Date();
    var expiryDate = new Date(startDate);
    expiryDate.setMinutes(startDate.getMinutes() + 100);
    startDate.setMinutes(startDate.getMinutes() - 100);

    var sharedAccessPolicy = {
      AccessPolicy: {
        Permissions: azure.TableUtilities.SharedAccessPermissions.QUERY,
        Start: startDate,
        Expiry: expiryDate
      },
    };

    var tableSAS = tableSvc.generateSharedAccessSignature('mytable', sharedAccessPolicy);
    var host = tableSvc.host;

Beachten Sie, dass die Hostinformationen auch bereitgestellt werden muss, wie dies erforderlich ist, wenn der Inhaber SAS versucht, Zugriff auf die Tabelle.

Klicken Sie dann die Clientanwendung verwendet die SAS mit **TableServiceWithSAS** zum Ausführen von Vorgängen anhand der Tabelle ein. Im folgende Beispiel wird an der Tabelle und führt eine Abfrage aus.

    var sharedTableService = azure.createTableServiceWithSas(host, tableSAS);
    var query = azure.TableQuery()
      .where('PartitionKey eq ?', 'hometasks');

    sharedTableService.queryEntities(query, null, function(error, result, response) {
      if(!error) {
        // result contains the entities
      }
    });

Da die SAS mit nur Abfrage Access generiert wurden, wenn versucht wurden, einfügen, aktualisieren oder Löschen von Personen, würde ein Fehler zurückgegeben.

### <a name="access-control-lists"></a>Access Control Lists

Eine Liste (ACL) können Sie auch die Zugriffsrichtlinie für eine SAS festlegen. Dies ist sinnvoll, wenn Sie zulassen von mehreren Clients Zugriff auf die Tabelle, aber andere Richtlinien für jeden Client bereitstellen möchten.

Eine ACL ist mithilfe von ein Array von Access Richtlinien mit Personalnummer zugeordnet jede Richtlinie implementiert. Im folgende Beispiel werden zwei Richtlinien, eine für 'user1' und eine für 'Benutzer2' definiert:

    var sharedAccessPolicy = {
      user1: {
        Permissions: azure.TableUtilities.SharedAccessPermissions.QUERY,
        Start: startDate,
        Expiry: expiryDate
      },
      user2: {
        Permissions: azure.TableUtilities.SharedAccessPermissions.ADD,
        Start: startDate,
        Expiry: expiryDate
      }
    };

Im folgende Beispiel wird die aktuelle ACL für die Tabelle **Hometasks** und addiert anschließend die neuen Richtlinien **SetTableAcl**verwenden. Dieser Ansatz ermöglicht:

    var extend = require('extend');
    tableSvc.getTableAcl('hometasks', function(error, result, response) {
    if(!error){
        var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
        tableSvc.setTableAcl('hometasks', newSignedIdentifiers, function(error, result, response){
          if(!error){
            // ACL set
          }
        });
      }
    });

Sobald die ACL festgelegt wurde, können Sie dann eine SAS auf Basis der ID für eine Richtlinie erstellen. Im folgenden Beispiel wird eine neue SAS für 'Benutzer2':

    tableSAS = tableSvc.generateSharedAccessSignature('hometasks', { Id: 'user2' });

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter den folgenden Ressourcen.

-   [Teamblog Azure-Speicher][].
-   [Azure-Speicher SDK für Knoten][] Repository auf GitHub.
-   [Node.js-Entwicklercenter](/develop/nodejs/)

  [Azure-Speicher SDK für Knoten]: https://github.com/Azure/azure-storage-node
  [OData.org]: http://www.odata.org/
  [Using the REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx
  [Azure Portal]: portal.azure.com

  [Node.js Cloud Service]: ../cloud-services-nodejs-develop-deploy-app.md
  [Azure-Speicher-Teamblog]: http://blogs.msdn.com/b/windowsazurestorage/
  [Website with WebMatrix]: ../web-sites-nodejs-use-webmatrix.md
  [Node.js Cloud Service with Storage]: ../storage-nodejs-use-table-storage-cloud-service-app.md
  [Mithilfe von Tabelle Azure Service Node.js Web-app]: ../storage-nodejs-use-table-storage-web-site.md
  [Create and deploy a Node.js application to an Azure website]: ../web-sites-nodejs-develop-deploy-mac.md

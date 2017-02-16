<properties
    pageTitle="So verwenden Sie BLOB-Speicher von Node.js | Microsoft Azure"
    description="Speichern von unstrukturierten Daten in der Cloud mit Azure Blob-Speicher (Objektspeicher)."
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



# <a name="how-to-use-blob-storage-from-nodejs"></a>So verwenden Sie BLOB-Speicher von Node.js

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>(Übersicht)

In diesem Artikel werden häufige Szenarien mit Blob-Speicher durchführen. Die Beispiele sind über die Node.js-API geschrieben werden. Die Szenarios dieser gehören zum Hochladen, Liste, herunterladen und Löschen von Blobs.

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-nodejs-application"></a>Erstellen Sie eine Anwendung Node.js

Anweisungen zum Erstellen einer Anwendungs Node.js finden Sie unter [Erstellen einer Node.js Web app im App-Verwaltungsdienst Azure], [Erstellen und Bereitstellen eine Anwendung Node.js in einer Azure-Cloud-Service] – mithilfe von Windows PowerShell, oder [Erstellen und Bereitstellen einer Node.js Web app in Azure Web Matrix verwenden].

## <a name="configure-your-application-to-access-storage"></a>Konfigurieren der Anwendungs Zugriff auf Speicher

Wenn Azure-Speicher verwenden möchten, benötigen Sie den Azure-Speicher SDK für Node.js, die eine Reihe von Komfort Bibliotheken enthält, die mit der Speicher REST-Dienste kommunizieren an.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Verwenden Sie Knoten Paket Manager (NPM), um das Paket zu erhalten

1.  Verwenden Sie eine Line Schnittstelle wie **PowerShell** (Windows), **Terminal** (Mac) oder **Bash** (Unix), zum Navigieren Sie zu dem Ordner, in dem Sie eine Stichprobe Anwendung erstellt.

2.  Geben Sie im Befehlsfenster **Npm installieren Azure-Speicher** . Ausgabe des Befehls ist ähnlich wie im folgenden Code wird.

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

3.  Sie können manuell ausführen des Befehls **ls** zu überprüfen, ob ein **Knoten\_Module** Ordner erstellt wurde. Suchen Sie in diesem Ordner das Paket **Azure-Speicher** , das die Bibliotheken enthält, die Sie Zugriff auf Speicher benötigen.

### <a name="import-the-package"></a>Das Paket importieren

Mit dem Editor oder einem anderen Text-Editor, fügen Sie Folgendes an den Anfang der Datei **server.js** der Anwendung für Speicher verwenden möchten:

    var azure = require('azure-storage');

## <a name="set-up-an-azure-storage-connection"></a>Einrichten einer Verbindung Azure-Speicher

Das Modul Azure liest die Umgebungsvariablen `AZURE_STORAGE_ACCOUNT` und `AZURE_STORAGE_ACCESS_KEY`, oder `AZURE_STORAGE_CONNECTION_STRING`, Informationen zur Anmeldung bei Ihrem Konto Azure-Speicher benötigt. Wenn dieser Variablen nicht festgelegt werden, müssen Sie die Kontoinformationen angeben, wenn Sie **CreateBlobService**aufrufen.

Ein Beispiel für das Einrichten der Umgebungsvariablen im [Portal Azure](https://portal.azure.com) für eine Azure Web app finden Sie unter [Node.js Web app mithilfe des Diensts für Azure Tabelle].

## <a name="create-a-container"></a>Erstellen eines Containers

Das Objekt **BlobService** können Sie mit Containern und Blobs arbeiten. Der folgende Code erstellt ein **BlobService** Objekt. Fügen Sie am oberen Rand der **server.js**Folgendes ein:

    var blobSvc = azure.createBlobService();

> [AZURE.NOTE] Sie können einen Blob anonym zugreifen, indem Sie mithilfe von **CreateBlobServiceAnonymous** und die Adresse des Hosts angeben. Beispielsweise `var blobSvc = azure.createBlobServiceAnonymous('https://myblob.blob.core.windows.net/');`.

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

Verwenden Sie zum Erstellen eines neuen Containers **CreateContainerIfNotExists**ein. Im folgenden Code wird erstellt, einen neuen Container mit dem Namen 'Mycontainer':

    blobSvc.createContainerIfNotExists('mycontainer', function(error, result, response){
        if(!error){
          // Container exists and is private
        }
    });

Wenn der Container neu erstellt wurde, `result.created` ist wahr. Wenn der Container bereits vorhanden ist, `result.created` ist "false". `response`enthält Informationen zum Vorgang angezeigt, einschließlich der ETag Informationen für den Container.

### <a name="container-security"></a>Container-Sicherheit

Standardmäßig neue Container sind privat und können nicht anonym zugegriffen werden kann. Um den Container veröffentlichen, damit Sie anonym zugreifen können, können Sie die Zugriffsebene des Containers **Blob** oder **Container**festlegen.

* **Blob** - ermöglicht anonyme Lesezugriff auf BLOB-Inhalt und Metadaten, die in diesem Container, jedoch nicht in Metadaten Container, beispielsweise alle Blobs innerhalb eines Containers auflisten

* **Container** - ermöglicht anonyme Lesezugriff auf BLOB-Inhalt und Metadaten sowie Container Metadaten

Im folgenden Code wird veranschaulicht, wie die Zugriffsebene zu **Blob**:

    blobSvc.createContainerIfNotExists('mycontainer', {publicAccessLevel : 'blob'}, function(error, result, response){
        if(!error){
          // Container exists and allows
          // anonymous read access to blob
          // content and metadata within this container
        }
    });

Alternativ können Sie die Zugriffsebene eines Containers ändern, indem Sie **SetContainerAcl** verwenden, um die Zugriffsebene angeben. Im folgenden Code wird die Zugriffsebene Container Änderungen:

    blobSvc.setContainerAcl('mycontainer', null /* signedIdentifiers */, {publicAccessLevel : 'container'} /* publicAccessLevel*/, function(error, result, response){
      if(!error){
        // Container access level set to 'container'
      }
    });

Das Ergebnis enthält Informationen zum Vorgang angezeigt, einschließlich der aktuellen **ETag** für den Container.

### <a name="filters"></a>Filter

Sie können optionale Filtervorgängen auf Operationen mit **BlobService**anwenden. Filtervorgängen kann einbeziehen Protokollierung, automatisch wiederholen, usw. Filter sind Objekte, die eine Methode mit der Signatur implementieren:

    function handle (requestOptions, next)

Nach deren preprocessing auf die Optionen für die Anforderung ausführen, muss die Methode "Weiter", rufen Sie einen Rückruf mit der folgenden Signatur übergeben:

    function (returnObject, finalCallback, next)

In diesem Rückruf und nach der Verarbeitung der ReturnObject (der Antwort aus der Anforderung an den Server) muss der Rückruf entweder als Nächstes aufrufen, falls vorhanden, damit andere Filter Verarbeitung fort, oder wenn einfach aufrufen FinalCallback zum Beenden der Dienst aufrufen.

Zwei Filter, die "Wiederholen" Logik implementieren sind im Azure SDK für Node.js, **ExponentialRetryPolicyFilter** und **LinearRetryPolicyFilter**enthalten. Die folgenden erstellt ein **BlobService** -Objekt, das die **ExponentialRetryPolicyFilter**verwendet:

    var retryOperations = new azure.ExponentialRetryPolicyFilter();
    var blobSvc = azure.createBlobService().withFilter(retryOperations);

## <a name="upload-a-blob-into-a-container"></a>Hochladen eines BLOBs in einen container

Es gibt drei Typen von Blobs: Blobs blockieren, Seite Blobs und Blobs anfügen. Blockieren Blobs zulassen, dass Sie weitere effizient große Daten hochladen. Anfügen von Blobs sind optimiert für Vorgänge anfügen. Seitenblobs sind für Vorgänge/Lese optimiert. Weitere Informationen finden Sie unter [Grundlegendes zu blockieren Blobs, Anfügen Blobs und Seitenblobs](http://msdn.microsoft.com/library/azure/ee691964.aspx).

### <a name="block-blobs"></a>Blockieren blobs

Verwenden Sie zum Hochladen von Daten in ein Blob blockieren, Folgendes ein:

* **CreateBlockBlobFromLocalFile** - erstellt einen neue TextBlock Blob und uploads des Inhalts einer Datei

* **CreateBlockBlobFromStream** - erstellt einen neue TextBlock Blob und uploads des Inhalts eines Streams

* **CreateBlockBlobFromText** - erstellt einen neue TextBlock Blob und uploads des Inhalts einer Zeichenfolge

* **CreateWriteStreamToBlockBlob** - stellt einen Stream Schreiben in ein Blob blockieren

Im folgenden Codebeispiel wird der Inhalt der Datei **test.txt** in **Myblob**hochgeladen.

    blobSvc.createBlockBlobFromLocalFile('mycontainer', 'myblob', 'test.txt', function(error, result, response){
      if(!error){
        // file uploaded
      }
    });

Die `result` von diesen Methoden zurückgegebenen Informationen auf den auswirken, wie etwa das **ETag** des Blob enthält.

### <a name="append-blobs"></a>Anfügen von blobs

Verwenden Sie zum Hochladen von Daten in ein neues anfügen Blob Folgendes ein:

* **CreateAppendBlobFromLocalFile** - erstellt einen neue anfügen Blob und uploads des Inhalts einer Datei

* **CreateAppendBlobFromStream** - erstellt einen neue anfügen Blob und den Inhalt eines Streams uploads

* **CreateAppendBlobFromText** - erstellt einen neues anfügen Blob und uploads des Inhalts einer Zeichenfolge

* **CreateWriteStreamToNewAppendBlob** - erstellt einen neue anfügen Blob, und klicken Sie dann stellt einen Stream zu schreiben

Im folgenden Codebeispiel wird der Inhalt der Datei **test.txt** in **Myappendblob**hochgeladen.

    blobSvc.createAppendBlobFromLocalFile('mycontainer', 'myappendblob', 'test.txt', function(error, result, response){
      if(!error){
        // file uploaded
      }
    });

Wenn Sie einen Textblock an eine vorhandene anfügen Blob angefügt werden soll, verwenden Sie Folgendes ein:

* **AppendFromLocalFile** - den Inhalt einer Datei an eine vorhandene anfügen Blob Anfügen

* **AppendFromStream** - des Inhalts eines Streams zu einer vorhandenen anfügen Blob Anfügen

* **AppendFromText** - den Inhalt einer Zeichenfolge zu einer vorhandenen anfügen Blob Anfügen

* **AppendBlockFromStream** - des Inhalts eines Streams zu einer vorhandenen anfügen Blob Anfügen

* **AppendBlockFromText** - den Inhalt einer Zeichenfolge zu einer vorhandenen anfügen Blob Anfügen

> [AZURE.NOTE] AppendFromXXX APIs kann einige clientseitige Überprüfung, nicht schnelles Unncessary Server Anruf zu vermeiden. AppendBlockFromXXX wird nicht.

Im folgenden Codebeispiel wird der Inhalt der Datei **test.txt** in **Myappendblob**hochgeladen.

    blobSvc.appendFromText('mycontainer', 'myappendblob', 'text to be appended', function(error, result, response){
      if(!error){
        // text appended
      }
    });


### <a name="page-blobs"></a>Seitenblobs

Verwenden Sie zum Hochladen von Daten auf einer Seitenblob Folgendes ein:

* **CreatePageBlob** - erstellt einen neue Seitenblob einer bestimmten Länge

* **CreatePageBlobFromLocalFile** - erstellt einen neue Seitenblob und uploads des Inhalts einer Datei

* **CreatePageBlobFromStream** - erstellt einen neue Seitenblob und uploads des Inhalts eines Streams

* **CreateWriteStreamToExistingPageBlob** - stellt einen Stream schreiben zu einer vorhandenen Seitenblob

* **CreateWriteStreamToNewPageBlob** - erstellt einen neue Seitenblob, und klicken Sie dann stellt einen Stream zu schreiben

Im folgenden Codebeispiel wird der Inhalt der Datei **test.txt** in **Mypageblob**hochgeladen.

    blobSvc.createPageBlobFromLocalFile('mycontainer', 'mypageblob', 'test.txt', function(error, result, response){
      if(!error){
        // file uploaded
      }
    });

> [AZURE.NOTE] Seitenblobs bestehen 512 Byte 'Seiten' aus. Sie erhalten eine Fehlermeldung beim Hochladen von Daten mit einer Größe, die ein Vielfaches von 512 nicht ist.

## <a name="list-the-blobs-in-a-container"></a>Liste der Blobs in einem container

Verwenden Sie die **ListBlobsSegmented** -Methode, um die Liste der Blobs in einem Container. Wenn Sie mit einem bestimmten Präfix Blobs zurückkehren möchten, verwenden Sie **ListBlobsSegmentedWithPrefix**.

    blobSvc.listBlobsSegmented('mycontainer', null, function(error, result, response){
      if(!error){
          // result.entries contains the entries
          // If not all blobs were returned, result.continuationToken has the continuation token.
      }
    });

Die `result` enthält eine `entries` Auflistung, die ein Array von Objekten ist, die jeder Blob zu beschreiben. Wenn alle Blobs zurückgegeben werden können, die `result` auch bietet eine `continuationToken`, die Sie als zweiten Parameter verwenden können, um zusätzliche Einträge abzurufen.

## <a name="download-blobs"></a>Herunterladen von blobs

Verwenden Sie zum Herunterladen von Daten von einem Blob Folgendes ein:

* **GetBlobToLocalFile** - schreibt den Blob-Inhalt in die Datei

* **GetBlobToStream** - schreibt den Blob-Inhalt in einem stream

* **GetBlobToText** - schreibt den Blob-Inhalt in einer Zeichenfolge

* **CreateReadStream** - stellt einen Stream, um aus dem Blob gelesen

Im folgenden Code wird veranschaulicht, wie mit **GetBlobToStream** Herunterladen des Inhalts der **Myblob** Blob und an der Datei **output.txt** mit einem Stream gespeichert werden:

    var fs = require('fs');
    blobSvc.getBlobToStream('mycontainer', 'myblob', fs.createWriteStream('output.txt'), function(error, result, response){
      if(!error){
        // blob retrieved
      }
    });

Die `result` Informationen zu den Blob, einschließlich **ETag** Informationen enthält.

## <a name="delete-a-blob"></a>Löschen eines BLOBs

Rufen Sie zum Löschen eines BLOBs abschließend **DeleteBlob**ein. Im folgenden Codebeispiel löscht das Blob mit dem Namen **Myblob**.

    blobSvc.deleteBlob(containerName, 'myblob', function(error, response){
      if(!error){
        // Blob has been deleted
      }
    });

## <a name="concurrent-access"></a>Gleichzeitigen Zugriff

Um gleichzeitigen Zugriff auf ein Blob von mehreren Clients oder mehrerer Prozessinstanzen unterstützen, können Sie **ETags** oder **Leases**.

* **Etag** - bietet eine Möglichkeit zum erkennen, die das Blob oder von einem anderen Prozess Container geändert wurde

* **Verleasen** – bietet eine Möglichkeit, ausschließlich, kann verlängert zu erhalten, schreiben oder löschen den Zugriff auf ein Blob für einen Zeitraum

### <a name="etag"></a>ETag

ETags verwenden, wenn Sie mehrere Clients oder Instanzen sowie das Schreiben in des Zeitraums Blob oder Seite zulassen müssen Blob gleichzeitig. Das ETag können Sie bestimmen, ob der Container oder Blob geändert wurde, da Sie ursprünglich gelesen oder erstellt, die Sie nicht von einem anderen Client oder Prozess zugesichert Änderungen überschrieben werden können.

Sie können mithilfe der optionalen ETag Bedingungen festlegen `options.accessConditions` Parameter. Im folgenden Code wird nur die Datei **Text.txt** hochlädt, wenn das Blob bereits vorhanden ist, der ETag-Wert enthalten `etagToMatch`.

    blobSvc.createBlockBlobFromLocalFile('mycontainer', 'myblob', 'test.txt', { accessConditions: { EtagMatch: etagToMatch} }, function(error, result, response){
        if(!error){
        // file uploaded
      }
    });

Wenn Sie ETags verwenden, ist das allgemeine Muster:

1. Rufen Sie das ETag als Ergebnis erstellen, Listen oder Get-Vorgang.

2. Ausführen einer Aktion, überprüfen, dass der ETag-Wert nicht geändert wurde.

Wenn der Wert geändert wurde, bedeutet dies, dass es sich bei einem anderen Client oder Instanz der Blob oder im Container, da Sie den ETag-Wert für Ihren Kunden geändert.

### <a name="lease"></a>Verleasen

Sie können eine neue verleasen erhalten, indem Sie mithilfe der Methode **AcquireLease** , Angeben der Blob oder im Container, die Sie auf eine verleasen abrufen möchten. Beispielsweise erhält der folgende Code ein verleasen auf **Myblob**.

    blobSvc.acquireLease('mycontainer', 'myblob', function(error, result, response){
      if(!error) {
        console.log('leaseId: ' + result.id);
      }
    });

Nachfolgende Vorgänge auf **Myblob** verpflichtet, die `options.leaseId` Parameter. Die ID wird als zurückgegeben verleasen `result.id` aus **AcquireLease**.

> [AZURE.NOTE] Standardmäßig ist die Dauer verleasen unbegrenzte. Sie können eine Dauer nicht Infinite (zwischen 15 und 60 Sekunden) angeben, durch die Bereitstellung der `options.leaseDuration` Parameter.

Verwenden Sie zum Entfernen eines verleasen **ReleaseLease**ein. Unterbrechen eines verleasen, aber verhindern, dass andere eine neue verleasen erhalten, bis die ursprüngliche Dauer abgelaufen ist, verwenden Sie **BreakLease**.

## <a name="work-with-shared-access-signatures"></a>Arbeiten Sie mit freigegebenen Access Signaturen

Freigegebenen Access Signaturen (SAS) werden auf sichere Weise zu präzise Zugriff auf Blobs und Container ohne Angabe Ihrer Speicher Kontonamen oder die Tasten bereitstellen. Freigegebene Access Signaturen werden häufig verwendet, eingeschränkter Zugriff auf Ihre Daten, wie z. B. eine mobile app Blobs Zugriff auf zulassen.

> [AZURE.NOTE] Während Sie auch anonymen Zugriff auf Blobs zulassen können, ermöglichen gemeinsamen Zugriff Signaturen mehr gesteuert Access, wie die SAS generiert werden sollen.

Eine vertrauenswürdige Anwendung wie etwa einen cloudbasierten Dienst freigegebenen Access Signaturen mithilfe der **GenerateSharedAccessSignature** von der **BlobService**generiert und stellt es in einer nicht vertrauenswürdigen oder teilweise vertrauenswürdige Anwendung wie mobile-app. Freigegebene Access Signaturen werden generiert mithilfe einer Richtlinie, die beschreibt starten und Beenden von Datumsangaben, die während, die gemeinsamen Zugriff Signaturen ungültig sind, sowie die Zugriffsebene für den Signaturen Inhaber der gemeinsamen Zugriff erteilt.

Im folgenden Code wird eine neue freigegebenen Zugriffsrichtlinie, mit dem den gemeinsamen Zugriff Signaturen Inhaber auf den **Myblob** Blob gelesen Vorgänge durchführen kann generiert und läuft ab 100 Minuten nach der Zeit, die sie erstellt wurde.

    var startDate = new Date();
    var expiryDate = new Date(startDate);
    expiryDate.setMinutes(startDate.getMinutes() + 100);
    startDate.setMinutes(startDate.getMinutes() - 100);

    var sharedAccessPolicy = {
      AccessPolicy: {
        Permissions: azure.BlobUtilities.SharedAccessPermissions.READ,
        Start: startDate,
        Expiry: expiryDate
      },
    };

    var blobSAS = blobSvc.generateSharedAccessSignature('mycontainer', 'myblob', sharedAccessPolicy);
    var host = blobSvc.host;

Beachten Sie, dass die Hostinformationen auch bereitgestellt werden muss, wie dies erforderlich ist, wenn der freigegebene Access Signaturen Inhaber versucht, Zugriff auf den Container.

Klicken Sie dann die Clientanwendung verwendet freigegebenen Access Signaturen mit **BlobServiceWithSAS** zum Ausführen von Vorgängen anhand der Blob. Die folgenden Ruft Informationen **Myblob**ab.

    var sharedBlobSvc = azure.createBlobServiceWithSas(host, blobSAS);
    sharedBlobSvc.getBlobProperties('mycontainer', 'myblob', function (error, result, response) {
      if(!error) {
        // retrieved info
      }
    });

Da Signaturen gemeinsamen Zugriff mit schreibgeschützten Zugriff, generiert wurden, wenn versucht wird, das Blob ändern, wird ein Fehler zurückgegeben.

### <a name="access-control-lists"></a>Listen der Access-Steuerelement

Sie können auch eine Access Control List (ACL) verwenden, die Zugriffsrichtlinie für SAS festlegen. Dies ist sinnvoll, wenn Sie zulassen von mehreren Clients ein Containers zugreifen, aber andere Richtlinien für jeden Client bereitstellen möchten.

Eine ACL ist mithilfe von ein Array von Access Richtlinien mit Personalnummer zugeordnet jede Richtlinie implementiert. Im folgenden Code wird definiert zwei Richtlinien, eine für 'user1' und eine für 'Benutzer2':

    var sharedAccessPolicy = {
      user1: {
        Permissions: azure.BlobUtilities.SharedAccessPermissions.READ,
        Start: startDate,
        Expiry: expiryDate
      },
      user2: {
        Permissions: azure.BlobUtilities.SharedAccessPermissions.WRITE,
        Start: startDate,
        Expiry: expiryDate
      }
    };

Im folgenden Code wird die aktuelle ACL für **Mycontainer**ruft und addiert anschließend die neuen Richtlinien **SetBlobAcl**verwenden. Dieser Ansatz ermöglicht:

    var extend = require('extend');
    blobSvc.getBlobAcl('mycontainer', function(error, result, response) {
      if(!error){
        var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
        blobSvc.setBlobAcl('mycontainer', newSignedIdentifiers, function(error, result, response){
          if(!error){
            // ACL set
          }
        });
      }
    });

Nachdem die ACL festgelegt haben, können Sie dann freigegebene Access Signaturen auf Basis der ID für eine Richtlinie erstellen. Im folgenden Code wird erstellt neuen freigegebenen Access Signaturen für 'Benutzer2':

    blobSAS = blobSvc.generateSharedAccessSignature('mycontainer', { Id: 'user2' });

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter den folgenden Ressourcen.

-   [Azure-Speicher SDK für Knoten-API-Referenz][]
-   [Azure-Speicher-Teamblog][]
-   [Azure-Speicher SDK für Knoten][] Repository auf GitHub
-   [Node.js-Entwicklercenter](/develop/nodejs/)
-   [Übertragen von Daten mit dem Befehlszeilendienstprogramm AzCopy](storage-use-azcopy.md)

[Azure-Speicher SDK für Knoten]: https://github.com/Azure/azure-storage-node

[Erstellen Sie eine Node.js Web app in Azure-App-Verwaltungsdienst]: ../app-service-web/web-sites-nodejs-develop-deploy-mac.md
[Node.js Cloud Service with Storage]: ../cloud-services/storage-nodejs-use-table-storage-cloud-service-app.md
[Mithilfe von Tabelle Azure Service Node.js Web-app]: ../app-service-web/storage-nodejs-use-table-storage-web-site.md
[Erstellen und Bereitstellen einer Node.js Web app Azure mithilfe von Web Matrix]: ../app-service-web/web-sites-nodejs-use-webmatrix.md
[Using the REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx
[Azure Portal]: https://portal.azure.com
[Erstellen und Bereitstellen einer Anwendungs Node.js zu Azure-Cloud-Dienst]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Azure-Speicher-Teamblog]: http://blogs.msdn.com/b/windowsazurestorage/
[Azure-Speicher SDK für Knoten-API-Referenz]: http://dl.windowsazure.com/nodestoragedocs/index.html

<properties
    pageTitle="Wie Warteschlange-Speicher von Node.js verwendet | Microsoft Azure"
    description="Informationen zum Verwenden der Warteschlange Azure Service erstellen und Löschen von Warteschlangen, einfügen, abrufen und Löschen von Nachrichten. Die Beispiele in Node.js geschrieben wurde."
    services="storage"
    documentationCenter="nodejs"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robinsh"/>


# <a name="how-to-use-queue-storage-from-nodejs"></a>So Warteschlange-Speicher von Node.js verwenden

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>(Übersicht)

In diesem Handbuch wird gezeigt, wie häufige Szenarien mithilfe des Diensts für Microsoft Azure Warteschlange ausführen. Die Beispiele werden mithilfe der Node.js-API geschrieben. Die Szenarios dieser gehören **Einfügen**, **Empfang**, **Abrufen**und **Löschen von** Nachrichten in Warteschlange sowie **Erstellen und Löschen von Warteschlangen**.

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-nodejs-application"></a>Erstellen Sie eine Anwendung Node.js

Erstellen Sie eine leere Node.js Anwendung. Erstellen einer Anwendung Node.js Anweisungen finden Sie unter [Erstellen einer Node.js Web app im App-Verwaltungsdienst Azure], [Erstellen und Bereitstellen eine Anwendung Node.js in einer Azure-Cloud-Service] mithilfe von Windows PowerShell, oder [Erstellen und Bereitstellen einer Node.js Web app in Azure Web Matrix verwenden].

## <a name="configure-your-application-to-access-storage"></a>Konfigurieren der Anwendung für Access-Speicher

Wenn Azure-Speicher verwenden möchten, benötigen Sie den Azure-Speicher SDK für Node.js, die eine Reihe von Komfort Bibliotheken enthält, die mit der Speicher REST-Dienste kommunizieren an.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Verwenden Sie Knoten Paket Manager (NPM), um das Paket zu erhalten

1.  Verwenden Sie eine Oberfläche Line z. B. **PowerShell** (Windows), **Terminal** (Mac) oder **Bash** (Unix), navigieren Sie zu dem Ordner, in dem Sie eine Stichprobe Anwendung erstellt.

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

Mit dem Editor oder einem anderen Text-Editor, fügen Sie Folgendes an den Anfang der Datei **server.js** der Anwendung für Speicher verwenden möchten:

    var azure = require('azure-storage');

## <a name="setup-an-azure-storage-connection"></a>Für die Einrichtung einer Verbindungs Azure-Speicher

Das Modul Azure liest die Umgebungsvariablen AZURE\_Speicher\_-Konto und AZURE\_Speicher\_ACCESS\_Schlüssel oder AZURE\_Speicher\_Verbindung\_Zeichenfolge Informationen erforderlich, um eine Verbindung mit Ihrem Konto Azure-Speicher. Wenn dieser Variablen nicht festgelegt werden, müssen Sie die Kontoinformationen angeben, wenn Sie **CreateQueueService**aufrufen.

Ein Beispiel für das Einrichten der Umgebungsvariablen im [Portal Azure](https://portal.azure.com) für eine Website Azure finden Sie unter [Node.js Web app mithilfe des Diensts für Azure Tabelle].

## <a name="how-to-create-a-queue"></a>Gewusst wie: Erstellen eine Warteschlange

Der folgende Code erstellt ein Objekt **QueueService** , womit Sie mit Warteschlangen arbeiten kann.

    var queueSvc = azure.createQueueService();

Verwenden Sie die **CreateQueueIfNotExists** -Methode, die die angegebene Warteschlange zurückgibt, wenn es bereits vorhanden oder erstellt eine neue Warteschlange mit dem angegebenen Namen ein, wenn es nicht bereits vorhanden ist.

    queueSvc.createQueueIfNotExists('myqueue', function(error, result, response){
      if(!error){
        // Queue created or exists
      }
    });

Wenn die Warteschlange erstellt wurde, `result.created` ist wahr. Wenn die Warteschlange vorhanden ist, `result.created` ist "false".

### <a name="filters"></a>Filter

Optionale Filtervorgängen können auf Operationen mit **QueueService**angewendet werden. Filtervorgängen kann einbeziehen Protokollierung, automatisch wiederholen, usw. Filter sind Objekte, die eine Methode mit der Signatur implementieren:

    function handle (requestOptions, next)

Nach deren preprocessing auf die Optionen für die Anforderung ausführen, muss die Methode aufrufen "Weiter", und einen Rückruf mit der folgenden Signatur übergeben:

    function (returnObject, finalCallback, next)

In diesem Rückruf und nach der Verarbeitung der ReturnObject (der Antwort aus der Anforderung an den Server) muss der Rückruf entweder als Nächstes aufrufen, falls vorhanden, damit andere Filter Verarbeitung fortzusetzen oder einfach FinalCallback andernfalls aufzurufen, um den Dienst aufrufen zu beenden.

Zwei Filter, die "Wiederholen" Logik implementieren sind im Azure SDK für Node.js, **ExponentialRetryPolicyFilter** und **LinearRetryPolicyFilter**enthalten. Die folgenden erstellt ein **QueueService** -Objekt, das die **ExponentialRetryPolicyFilter**verwendet:

    var retryOperations = new azure.ExponentialRetryPolicyFilter();
    var queueSvc = azure.createQueueService().withFilter(retryOperations);

## <a name="how-to-insert-a-message-into-a-queue"></a>Gewusst wie: Einfügen eine Nachricht in einer Warteschlange

Verwenden Sie zum Einfügen einer Nachricht in einer Warteschlange die **CreateMessage** -Methode zum Erstellen einer neuen Nachricht, und fügen es an die an.

    queueSvc.createMessage('myqueue', "Hello world!", function(error, result, response){
      if(!error){
        // Message inserted
      }
    });

## <a name="how-to-peek-at-the-next-message"></a>Gewusst wie: Einsehen der nächsten Nachricht

Sie können die Nachricht an eine Warteschlange der Vorderseite einsehen, ohne aus der Warteschlange entfernt werden, indem Sie die Methode **PeekMessages** aufrufen. Standardmäßig sieht **PeekMessages** einer einzelnen Nachricht.

    queueSvc.peekMessages('myqueue', function(error, result, response){
      if(!error){
        // Message text is in messages[0].messageText
      }
    });

Die `result` die Nachricht enthält.

> [AZURE.NOTE] Verwenden **PeekMessages** , wenn vorhanden sind, dass keine Nachrichten in der Warteschlange kein Fehler zurückgegeben werden, werden jedoch keine Nachrichten zurückgegeben.

## <a name="how-to-dequeue-the-next-message"></a>Gewusst wie: Abrufen der nächsten Nachricht

Verarbeiten einer Nachricht umfasst zwei Phasen:

1. Die Nachricht aus Warteschlange entfernt.

2. Löschen Sie die Nachricht ein.

Wenn Sie eine Nachricht aus der Warteschlange entfernt, verwenden Sie **GetMessages**. Dadurch wird die Nachrichten in der Warteschlange unsichtbar, damit keine anderen Clients verarbeitet werden können. Nachdem eine Anwendung eine Nachricht verarbeitet hat, rufen Sie **DeleteMessage** , um ihn in der Warteschlange löschen. Im folgende Beispiel wird eine Meldung angezeigt, und dann wird gelöscht:

    queueSvc.getMessages('myqueue', function(error, result, response){
      if(!error){
        // Message text is in messages[0].messageText
        var message = result[0];
        queueSvc.deleteMessage('myqueue', message.messageId, message.popReceipt, function(error, response){
          if(!error){
            //message deleted
          }
        });
      }
    });

> [AZURE.NOTE] Standardmäßig wird eine Nachricht nur 30 Sekunden ausgeblendet nach dem für andere Clients sichtbar ist. Sie können mit einen anderen Wert angeben `options.visibilityTimeout` mit **GetMessages**.

> [AZURE.NOTE]
> Verwenden **GetMessages** , wenn vorhanden sind, dass keine Nachrichten in der Warteschlange kein Fehler zurückgegeben werden, werden jedoch keine Nachrichten zurückgegeben.

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Gewusst wie: Ändern des Inhalts einer Nachricht in der Warteschlange

Sie können den Inhalt einer Nachricht in-situ in der Warteschlange mit **UpdateMessage**ändern. Im folgenden Beispiel wird aktualisiert, dass Sie den Text einer Nachricht:

    queueSvc.getMessages('myqueue', function(error, result, response){
      if(!error){
        // Got the message
        var message = result[0];
        queueSvc.updateMessage('myqueue', message.messageId, message.popReceipt, 10, {messageText: 'new text'}, function(error, result, response){
          if(!error){
            // Message updated successfully
          }
        });
      }
    });

## <a name="how-to-additional-options-for-dequeuing-messages"></a>Gewusst wie: Zusätzliche Optionen für das Entfernen aus der Warteschlange Nachrichten

Es gibt zwei Möglichkeiten, die Sie anpassen können, Nachrichtenübermittlung aus einer Warteschlange aus:

* `options.numOfMessages`-Abrufen einer Reihe von Nachrichten (bis zu 32.)
* `options.visibilityTimeout`– Legen Sie ein Zeitlimit verlängern oder verkürzen Invisibility ein.

Im folgende Beispiel verwendet die Methode **GetMessages** , um 15 Nachrichten in einem Anruf zu gelangen. Dann verarbeitet jede Nachricht mit einer for-Schleife. Invisibility Timeout wird auch auf fünf Minuten für alle Nachrichten, die von dieser Methode zurückgegebene festgelegt.

    queueSvc.getMessages('myqueue', {numOfMessages: 15, visibilityTimeout: 5 * 60}, function(error, result, response){
      if(!error){
        // Messages retreived
        for(var index in result){
          // text is available in result[index].messageText
          var message = result[index];
          queueSvc.deleteMessage(queueName, message.messageId, message.popReceipt, function(error, response){
            if(!error){
              // Message deleted
            }
          });
        }
      }
    });

## <a name="how-to-get-the-queue-length"></a>Gewusst wie: Abrufen der Warteschlangenlänge

Die **GetQueueMetadata** gibt Metadaten für die Warteschlange, einschließlich der ungefähren Anzahl der Nachrichten in der Warteschlange zurück.

    queueSvc.getQueueMetadata('myqueue', function(error, result, response){
      if(!error){
        // Queue length is available in result.approximateMessageCount
      }
    });

## <a name="how-to-list-queues"></a>Gewusst wie: Liste reiht

Verwenden Sie **ListQueuesSegmented**aus, um eine Liste mit Warteschlangen abzurufen. Verwenden Sie zum Abrufen einer Liste nach einem bestimmten Präfix gefilterten **ListQueuesSegmentedWithPrefix**ein.

    queueSvc.listQueuesSegmented(null, function(error, result, response){
      if(!error){
        // result.entries contains the list of queues
      }
    });

Wenn Sie alle Warteschlangen zurückgegeben werden können, `result.continuationToken` als erster Parameter der **ListQueuesSegmented** oder der zweite Parameter der **ListQueuesSegmentedWithPrefix** verwendet werden können, um weitere Ergebnisse abzurufen.

## <a name="how-to-delete-a-queue"></a>Gewusst wie: Löschen eine Warteschlange

Zum Löschen einer Warteschlange und alle darin enthaltenen Nachrichten rufen Sie die **DeleteQueue** -Methode der Warteschlange-Objekt aus.

    queueSvc.deleteQueue(queueName, function(error, response){
      if(!error){
        // Queue has been deleted
      }
    });

Um alle Nachrichten aus einer Warteschlange deaktivieren, ohne ihn zu löschen, verwenden Sie **ClearMessages**.

## <a name="how-to-work-with-shared-access-signatures"></a>So: Arbeiten mit freigegebenen Access Signaturen

Freigegebene Access Signaturen (SAS) werden auf sichere Weise zu präzise Zugriff auf Warteschlangen ohne Angabe Ihrer Speicher Kontonamen oder die Tasten bereitstellen. SAS werden häufig verwendet, eingeschränkter Zugriff auf Warteschlangen, wie z. B. zulassen einer mobilen app, Nachrichten zu senden.

Eine vertrauenswürdige Anwendung wie etwa einen cloudbasierten Dienst generiert eine SAS mithilfe der **GenerateSharedAccessSignature** von der **QueueService**und für nicht vertrauenswürdige oder teilweise vertrauenswürdige Anwendung bereitgestellt. Beispielsweise mobile-app. Die SAS wird generiert mithilfe einer Richtlinie, die die Anfangs- und Endtermine beschreibt, bei denen die SAS ungültig ist, als auch die Zugriffsebene für den SAS Inhaber erteilt.

Im folgenden Beispiel wird eine neue freigegebenen Zugriffsrichtlinie, mit denen den Inhaber SAS Nachrichten in der Warteschlange hinzufügen kann, wird generiert und läuft ab 100 Minuten nach der Zeit, die sie erstellt wurde.

    var startDate = new Date();
    var expiryDate = new Date(startDate);
    expiryDate.setMinutes(startDate.getMinutes() + 100);
    startDate.setMinutes(startDate.getMinutes() - 100);

    var sharedAccessPolicy = {
      AccessPolicy: {
        Permissions: azure.QueueUtilities.SharedAccessPermissions.ADD,
        Start: startDate,
        Expiry: expiryDate
      }
    };

    var queueSAS = queueSvc.generateSharedAccessSignature('myqueue', sharedAccessPolicy);
    var host = queueSvc.host;

Beachten Sie, dass die Hostinformationen auch, bereitgestellt werden muss, wie dies erforderlich ist, wenn der Inhaber SAS versucht, auf die Warteschlange zugreifen.

Dann die Clientanwendung verwendet die SAS mit **QueueServiceWithSAS** zum Ausführen von Vorgängen anhand der Warteschlange an. Im folgenden Beispiel wird eine Verbindung mit der Warteschlange und erstellt eine Nachricht.

    var sharedQueueService = azure.createQueueServiceWithSas(host, queueSAS);
    sharedQueueService.createMessage('myqueue', 'Hello world from SAS!', function(error, result, response){
      if(!error){
        //message added
      }
    });

Da die SAS mit Access hinzufügen, generiert wurde, wenn es versucht wurden, lesen, aktualisieren und Löschen von Nachrichten, würde ein Fehler zurückgegeben.

### <a name="access-control-lists"></a>Listen der Access-Steuerelement

Eine Liste (ACL) können Sie auch die Zugriffsrichtlinie für eine SAS festlegen. Dies ist sinnvoll, wenn Sie zulassen von mehreren Clients auf die Warteschlange zuzugreifen, aber andere Richtlinien für jeden Client bereitstellen möchten.

Eine ACL ist mithilfe von ein Array von Access Richtlinien mit Personalnummer zugeordnet jede Richtlinie implementiert. Im folgende Beispiel wird die zwei Richtlinien definiert. eine für 'Benutzer1' und eine für 'Benutzer2':

    var sharedAccessPolicy = {
      user1: {
        Permissions: azure.QueueUtilities.SharedAccessPermissions.PROCESS,
        Start: startDate,
        Expiry: expiryDate
      },
      user2: {
        Permissions: azure.QueueUtilities.SharedAccessPermissions.ADD,
        Start: startDate,
        Expiry: expiryDate
      }
    };

Im folgende Beispiel wird die aktuelle ACL für **Myqueue**anschließend die neuen Richtlinien mit **SetQueueAcl**hinzugefügt. Dieser Ansatz ermöglicht:

    var extend = require('extend');
    queueSvc.getQueueAcl('myqueue', function(error, result, response) {
      if(!error){
        var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
        queueSvc.setQueueAcl('myqueue', newSignedIdentifiers, function(error, result, response){
          if(!error){
            // ACL set
          }
        });
      }
    });

Sobald die ACL festgelegt wurde, können Sie dann eine SAS auf Basis der ID für eine Richtlinie erstellen. Im folgenden Beispiel wird eine neue SAS für 'Benutzer2':

    queueSAS = queueSvc.generateSharedAccessSignature('myqueue', { Id: 'user2' });

## <a name="next-steps"></a>Nächste Schritte

Die Grundlagen der Warteschlangenspeicher bearbeitet haben, folgen Sie diesen Links komplexere Speicheraufgaben Lernen aus.

-   Besuchen Sie den [Teamblog Azure-Speicher][].
-   Besuchen Sie das [Azure-Speicher SDK für Knoten][] Repository auf GitHub ein.

  [Azure-Speicher SDK für Knoten]: https://github.com/Azure/azure-storage-node
  [using the REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx
  [Azure Portal]: https://portal.azure.com
  [Erstellen Sie eine Node.js Web app in Azure-App-Verwaltungsdienst]: ../app-service-web/web-sites-nodejs-develop-deploy-mac.md
  [Node.js Cloud Service with Storage]: ../cloud-services/storage-nodejs-use-table-storage-cloud-service-app.md
  [Mithilfe von Tabelle Azure Service Node.js Web-app]: ../app-service-web/storage-nodejs-use-table-storage-web-site.md


  [Queue1]: ./media/storage-nodejs-how-to-use-queues/queue1.png
  [plus-new]: ./media/storage-nodejs-how-to-use-queues/plus-new.png
  [quick-create-storage]: ./media/storage-nodejs-how-to-use-queues/quick-storage.png



  [Erstellen und Bereitstellen einer Anwendungs Node.js zu Azure-Cloud-Dienst]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
  [Azure-Speicher-Teamblog]: http://blogs.msdn.com/b/windowsazurestorage/
  [Erstellen und Bereitstellen einer Node.js Web app Azure mithilfe von Web Matrix]: ../app-service-web/web-sites-nodejs-use-webmatrix.md

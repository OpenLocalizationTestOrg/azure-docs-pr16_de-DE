<properties 
    pageTitle="Verwendung von Servicebuswarteschlangen Node.js | Microsoft Azure" 
    description="Erfahren Sie, wie in einer app Node.js Servicebuswarteschlangen in Azure zu verwenden." 
    services="service-bus" 
    documentationCenter="nodejs" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="10/03/2016" 
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-queues"></a>So verwenden Sie Servicebuswarteschlangen

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Dieser Artikel beschreibt die Verwendung von Servicebuswarteschlangen Node.js. Die Beispiele in JavaScript geschrieben und das Modul Node.js Azure verwenden. Die Szenarios dieser gehören **Erstellen von Warteschlangen**, **Senden und Empfangen von Nachrichten**und **Löschen von Warteschlangen**. Weitere Informationen zum Warteschlangen finden Sie im Abschnitt für die [nächsten Schritte](#next-steps) .

[AZURE.INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

## <a name="create-a-nodejs-application"></a>Erstellen Sie eine Anwendung Node.js

Erstellen Sie eine leere Node.js Anwendung. Anweisungen zum Erstellen einer Node.js-Anwendungs finden Sie unter [Erstellen und Bereitstellen einer Node.js-Anwendung auf einer Website Azure][], oder mithilfe der Windows PowerShell- [Node.js Cloud-Dienst][] .

## <a name="configure-your-application-to-use-service-bus"></a>Konfigurieren der Anwendungs Dienstbus verwendet.

Um Azure-Dienstbus verwenden, herunterladen Sie und verwenden Sie das Paket Node.js Azure. Dieses Paket enthält eine Reihe von Bibliotheken, die Kommunikation mit dem Dienst Bus REST-Dienste.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Verwenden Sie Knoten Paket Manager (NPM), um das Paket zu erhalten

1. Verwenden das **Windows PowerShell für Node.js** Befehlsfenster navigieren Sie zu der **c:\\Knoten\\Sbqueues\\WebRole1** Ordner, in dem Sie eine Stichprobe Anwendung erstellt haben.

2. Geben Sie im Fenster Befehl die Ausgabe ähnlich wie der folgende zur Folge sollte **Npm installieren Azure** :

    ```
    azure@0.7.5 node_modules\azure
        ├── dateformat@1.0.2-1.2.3
        ├── xmlbuilder@0.4.2
        ├── node-uuid@1.2.0
        ├── mime@1.2.9
        ├── underscore@1.4.4
        ├── validator@1.1.1
        ├── tunnel@0.0.2
        ├── wns@0.5.3
        ├── xml2js@0.2.7 (sax@0.5.2)
        └── request@2.21.0 (json-stringify-safe@4.0.0, forever-agent@0.5.0, aws-sign@0.3.0, tunnel-agent@0.3.0, oauth-sign@0.3.0, qs@0.6.5, cookie-jar@0.3.0, node-uuid@1.4.0, http-signature@0.9.11, form-data@0.0.8, hawk@0.13.1)
    ```

3. Sie können manuell ausführen des Befehls **ls** zu überprüfen, ob ein **Knoten\_Module** Ordner erstellt wurde. Suchen Sie in diesem Ordner **Azure** -Paket, das die Bibliotheken enthält, die Sie Servicebuswarteschlangen zugreifen müssen.

### <a name="import-the-module"></a>Importieren des Moduls

Mit dem Editor oder einem anderen Text-Editor, fügen Sie den folgenden an den Anfang der Datei **server.js** der Anwendung:

```
var azure = require('azure');
```

### <a name="set-up-an-azure-service-bus-connection"></a>Einrichten einer Verbindungs Azure Service

Das Azure Modul liest die Umgebungsvariablen AZURE\_SERVICEBUS\_NAMESPACE und AZURE\_SERVICEBUS\_ACCESS\_-Taste, um die Verbindung zu Dienstbus erforderlichen Informationen zu erhalten. Wenn dieser Variablen nicht festgelegt werden, müssen Sie die Kontoinformationen angeben, wenn Sie **CreateServiceBusService**aufrufen.

Ein Beispiel für das Einrichten der Umgebungsvariablen in einer Konfigurationsdatei für Azure-Cloud-Dienst finden Sie unter [Node.js Cloud-Dienst mit Speicher][].

Ein Beispiel für das Einrichten der Umgebungsvariablen im [Azure klassischen Portal][] für eine Website Azure finden Sie unter [Node.js Webanwendung mit Speicher][].

## <a name="create-a-queue"></a>Erstellen einer Warteschlange

Das Objekt **ServiceBusService** ermöglicht es Ihnen für die Arbeit mit Servicebuswarteschlangen. Der folgende Code erstellt ein **ServiceBusService** Objekt. Fügen sie nach der zu importierenden Azure-Modul-Anweisung am oberen Rand der Datei **server.js** hinzu:

```
var serviceBusService = azure.createServiceBusService();
```

Durch Aufrufen von **CreateQueueIfNotExists** für das **ServiceBusService** -Objekt aus, die angegebene Warteschlange zurückgegeben (falls vorhanden), oder eine neue Warteschlange mit dem angegebenen Namen wird erstellt. Im folgenden Code wird **CreateQueueIfNotExists** beim Erstellen oder Verbinden mit der Warteschlange mit dem Namen `myqueue`:

```
serviceBusService.createQueueIfNotExists('myqueue', function(error){
    if(!error){
        // Queue exists
    }
});
```

**CreateServiceBusService** unterstützt auch zusätzliche Optionen, mit die Sie die Standardeinstellungen für Warteschlange z. B. Nachrichtzeit in für live oder maximale Warteschlangengröße außer Kraft setzen können. Im folgenden Beispiel wird die Größe der maximalen 5 GB und eine Uhrzeit live (TTL) Wert von 1 Minute an:

```
var queueOptions = {
      MaxSizeInMegabytes: '5120',
      DefaultMessageTimeToLive: 'PT1M'
    };

serviceBusService.createQueueIfNotExists('myqueue', queueOptions, function(error){
    if(!error){
        // Queue exists
    }
});
```

### <a name="filters"></a>Filter

Optionale Filtervorgängen können auf Operationen mit **ServiceBusService**angewendet werden. Filtervorgängen kann einbeziehen Protokollierung, automatisch wiederholen, usw. Filter sind Objekte, die eine Methode mit der Signatur implementieren:

```
function handle (requestOptions, next)
```

Nach dem Ausführen seiner vorab Verarbeiten der Anforderung Optionen an, die Methode muss Aufrufen `next`, einen Rückruf mit der folgenden Signatur übergeben:

```
function (returnObject, finalCallback, next)
```

In diesem Rückruf und nach der Verarbeitung **ReturnObject** (der Antwort aus der Anforderung an den Server), Aufrufen des Rückrufs muss entweder `next` falls vorhanden, damit andere Filter Verarbeitung fort, oder rufen Sie einfach `finalCallback`, die den Dienst aufrufen endet.

Zwei Filter, die "Wiederholen" Logik implementieren sind im Azure SDK für Node.js, **ExponentialRetryPolicyFilter** und **LinearRetryPolicyFilter**enthalten. Die folgenden erstellt ein **ServiceBusService** -Objekt, das die **ExponentialRetryPolicyFilter**verwendet:

```
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var serviceBusService = azure.createServiceBusService().withFilter(retryOperations);
```

## <a name="send-messages-to-a-queue"></a>Senden von Nachrichten an eine Warteschlange

Zum Senden einer Nachricht an eine Warteschlange Dienstbus Ruft eine Anwendung die Methode **SendQueueMessage** auf das Objekt **ServiceBusService** . Nachrichten gesendete zu (und von erhaltene) Servicebuswarteschlangen **BrokeredMessage** Objekte und besitzen eine Reihe von Standardeigenschaften (z. B. **Bezeichnungsfeld** und **TimeToLive**), ein Wörterbuch, das verwendet wird, um die benutzerdefinierte Eigenschaften, die anwendungsspezifische halten sowie ein Datenblock willkürliche Anwendung. Eine Anwendung kann den Hauptteil der Nachricht eine Zeichenfolge übergeben, wie die Meldung festlegen. Alle erforderlichen Standardeigenschaften werden mit Standardwerten aufgefüllt.

Im folgenden Beispiel wird veranschaulicht, wie eine Testnachricht an die Warteschlange mit dem Namen senden `myqueue` **SendQueueMessage**verwenden:

```
var message = {
    body: 'Test message',
    customProperties: {
        testproperty: 'TestValue'
    }};
serviceBusService.sendQueueMessage('myqueue', message, function(error){
    if(!error){
        // message sent
    }
});
```

Servicebuswarteschlangen unterstützt eine maximale Nachrichtengröße 256 KB im [Standard Ebene](service-bus-premium-messaging.md) und 1 MB im [Premium Ebene](service-bus-premium-messaging.md)an. Die Kopfzeile, die die von Standardfarben und benutzerdefinierten Anwendungseigenschaften enthält, kann eine maximale Größe von 64 KB haben. Es gibt keine Beschränkung für die Anzahl der Nachrichten in einer Warteschlange frei, aber ein Linienende auf die Gesamtgröße der Nachrichten frei, indem Sie eine Warteschlange vorhanden ist. Diese Warteschlangengröße wird zum Zeitpunkt der Erstellung, mit einer Obergrenze von 5 GB definiert. Weitere Informationen zu Kontingenten finden Sie unter [Dienstbus Kontingente][].

## <a name="receive-messages-from-a-queue"></a>Empfangen von Nachrichten aus einer Warteschlange

Nachrichten werden aus einer Warteschlange mithilfe der **ReceiveQueueMessage** -Methode auf das Objekt **ServiceBusService** empfangen. Standardmäßig werden Nachrichten aus der Warteschlange gelöscht, gelesen werden. Sie können jedoch (anheften) lesen und Sperren die Nachricht ohne aus der Warteschlange gelöscht werden, indem Sie die optionalen Parameter **IsPeekLock** auf **true**.

Das Standardverhalten der lesen und löschen die Nachricht als Teil der Receive-Methode ist die einfachste Modell und am besten geeignet für Szenarien, die in denen Anwendung tolerieren kann nicht verarbeiten einer Nachricht im Fall eines Fehlers. Um dies zu verstehen, sollten Sie ein Szenario, in dem der Verbraucher gibt die Anforderung empfangen und dann stürzt ab, bevor Sie ihn aufbereiten, aus. Da Dienstbus die Nachricht als verbraucht markiert haben wird, wird Klicken Sie dann, wenn die Anwendung neu gestartet und beginnt, Nachrichten erneut, es die Nachricht verpasst haben, die vor der Absturz verbraucht wurde.

Wenn der Parameter **IsPeekLock** auf **true**festgelegt ist, wird die empfangen einen Vorgang zwei Phasen, aus der zu Support Applikationen ermöglicht, die fehlende Nachrichten tolerieren. Wenn Dienstbus eine Anforderung empfängt, findet die nächste Nachricht verbraucht werden, wenn es, sperren, um zu verhindern, dass andere Nutzer, die sie empfangen und gibt es dann an die Anwendung. Nach die Anwendung endet Verarbeiten der Nachricht (oder zuverlässig für die Verarbeitung von zukünftigen gespeichert), schließt es den endgültigen des Prozesses empfangen durch Aufrufen von **DeleteMessage** Methode und Bereitstellen von der Nachricht als Parameter gelöscht werden soll. Die Methode **DeleteMessage** kennzeichnen der Nachricht als verbraucht wird und aus der Warteschlange zu entfernen.

Im folgenden Beispiel wird veranschaulicht, wie empfangen und Verarbeiten von Nachrichten mithilfe von **ReceiveQueueMessage**. Im Beispiel zuerst empfängt und Löschen einer Nachricht, und klicken Sie dann erhält eine Nachricht mit **IsPeekLock** auf **true**festgelegt und anschließend löscht die Nachricht **DeleteMessage**verwenden:

```
serviceBusService.receiveQueueMessage('myqueue', function(error, receivedMessage){
    if(!error){
        // Message received and deleted
    }
});
serviceBusService.receiveQueueMessage('myqueue', { isPeekLock: true }, function(error, lockedMessage){
    if(!error){
        // Message received and locked
        serviceBusService.deleteMessage(lockedMessage, function (deleteError){
            if(!deleteError){
                // Message deleted
            }
        });
    }
});
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Behandlung von Anwendung stürzt ab und kann nicht gelesen werden Nachrichten

Dienstbus Funktionsumfang Sie ordnungsgemäß von Fehlern in Ihrer Anwendung oder Ansprechpartner Verarbeiten einer Nachricht wiederherstellen können. Ist eine Anwendung Empfänger zum Verarbeiten der Nachricht aus irgendeinem Grund können nicht genutzt werden, können sie die Methode **UnlockMessage** auf das Objekt **ServiceBusService** aufrufen. Dadurch wird Dienstbus Entsperren die Nachricht in der Warteschlange und Verfügbarmachen erneut, von der gleichen in Anspruch nehmen Anwendung oder von einer anderen in Anspruch nehmen Anwendung empfangen werden.

Es ist ebenfalls ein Timeout einer Nachricht in der Warteschlange gesperrt, und wenn die Anwendung nicht zum Verarbeiten der Nachricht, bevor Sie das Sperrungstimeout läuft ab (z. B., wenn die Anwendung stürzt ab), Dienstbus wird die Nachricht automatisch entsperren und Verfügbarmachen erneut empfangen werden.

Den Fall, dass die Anwendung stürzt ab, nach dem Verarbeiten der Nachricht, aber vor dem die **DeleteMessage** Methode ist, wird dann die Nachricht an die Anwendung zugestellt nach dem Neustart wird. Dies ist häufig bezeichnet, **Sobald Sie mindestens Verarbeitung**, d. h., jeder Nachricht verarbeitet werden mindestens einmal, aber in bestimmten Situationen möglicherweise die gleiche Nachricht erneut werden. Wenn Sie das Szenario doppelte Verarbeitung tolerieren kann, sollten Entwickler an ihrer Anwendung, doppelte Nachrichtenübermittlung behandeln zusätzliche Logik hinzufügen. Dies geschieht häufig mithilfe der **Nachrichten-ID** -Eigenschaft der Nachricht, die über die Übermittlungsversuche konstant bleiben wird.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Warteschlangen finden Sie unter den folgenden Ressourcen.

-   [Warteschlangen, Themen und Abonnements][]
-   [Azure SDK für Knoten][] Repository auf GitHub
-   [Node.js-Entwicklercenter](/develop/nodejs/)

  [Azure SDK für Knoten]: https://github.com/Azure/azure-sdk-for-node
  [Azure klassischen-portal]: http://manage.windowsazure.com
  
  [Node.js Cloud-Dienst]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
  [Warteschlangen, Themen und Abonnements]: service-bus-queues-topics-subscriptions.md
  [Erstellen und Bereitstellen einer Anwendungs Node.js auf eine Website zu Azure]: ../app-service-web/web-sites-nodejs-develop-deploy-mac.md
  [Node.js Cloud-Dienst mit Speicher]: ../cloud-services/storage-nodejs-use-table-storage-cloud-service-app.md
  [Node.js Webanwendung mit Speicher]: ../storage/storage-nodejs-how-to-use-table-storage.md
  [Dienstbus von Kontingenten]: service-bus-quotas.md
 

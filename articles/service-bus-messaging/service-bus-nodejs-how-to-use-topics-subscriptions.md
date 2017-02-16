<properties 
    pageTitle="So verwenden Sie Dienstbus Themen mit Node.js | Microsoft Azure" 
    description="Erfahren Sie, wie in einer app Node.js Dienstbus Themen und Abonnements in Azure verwenden." 
    services="service-bus" 
    documentationCenter="nodejs" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="sethm"/>


# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>So verwenden Sie Dienstbus Themen und Abonnements

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Dieses Handbuch beschreibt das Dienstbus Themen und Abonnements von Applications Node.js verwenden. Die Szenarios dieser gehören **Themen und Abonnements erstellen**, **Erstellen von Abonnements Filtern**, **Senden von Nachrichten** zu einem Thema, **Empfangen von Nachrichten aus einem Abonnement**und **Löschen von Themen und Abonnements**. Weitere Informationen zu Themen und Abonnements finden Sie im Abschnitt für die [nächsten Schritte](#next-steps) .

[AZURE.INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## <a name="create-a-nodejs-application"></a>Erstellen Sie eine Anwendung Node.js

Erstellen Sie eine leere Node.js Anwendung. Anweisungen zum Erstellen einer Node.js-Anwendungs finden Sie unter [Erstellen und Bereitstellen eine Anwendung Node.js auf einer Website von Azure], [Node.js Cloud-Dienst][] mit Windows PowerShell oder Website und WebMatrix.

## <a name="configure-your-application-to-use-service-bus"></a>Konfigurieren der Anwendungs Dienstbus verwendet.

Wenn Dienstbus verwenden möchten, herunterladen Sie das Node.js Azure-Paket. Dieses Paket enthält eine Reihe von Bibliotheken, die Kommunikation mit dem Dienst Bus REST-Dienste.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Verwenden Sie Knoten Paket Manager (NPM), um das Paket zu erhalten

1.  Verwenden Sie eine Oberfläche Line wie z. B. **PowerShell** (Windows), **Terminal** (Mac) oder **Bash** (Unix), navigieren Sie zu dem Ordner, in dem Sie eine Stichprobe Anwendung erstellt.

2.  Geben Sie im Befehlsfenster, die sich in der folgenden Ausgabe ergeben sollte **Npm Azure zu installieren** :

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

3.  Sie können manuell ausführen des Befehls **ls** zu überprüfen, ob ein **Knoten\_Module** Ordner erstellt wurde. Suchen Sie in diesem Ordner **Azure** -Paket, das die Bibliotheken enthält, die Sie Dienstbus Themen zugreifen müssen.

### <a name="import-the-module"></a>Importieren des Moduls

Mit dem Editor oder einem anderen Text-Editor, fügen Sie den folgenden an den Anfang der Datei **server.js** der Anwendung:

```
var azure = require('azure');
```

### <a name="set-up-a-service-bus-connection"></a>Richten Sie eine Verbindung Dienstbus

Das Azure Modul liest die Umgebungsvariablen AZURE\_SERVICEBUS\_NAMESPACE und AZURE\_SERVICEBUS\_ACCESS\_wichtige Informationen für die Verbindung zu Dienstbus erforderlich. Wenn dieser Variablen nicht festgelegt werden, müssen Sie die Kontoinformationen angeben, wenn Sie **CreateServiceBusService**aufrufen.

Ein Beispiel für das Einrichten der Umgebungsvariablen in einer Konfigurationsdatei für Azure-Cloud-Dienst finden Sie unter [Node.js Cloud-Dienst mit Speicher][].

Ein Beispiel für das Einrichten der Umgebungsvariablen im [Azure klassischen Portal][] für eine Website Azure finden Sie unter [Web-Anwendung mit Speicher Node.js][].

## <a name="create-a-topic"></a>Erstellen Sie ein Thema

Das Objekt **ServiceBusService** ermöglicht es Ihnen für die Arbeit mit Themen. Der folgende Code erstellt ein **ServiceBusService** Objekt. Fügen sie nach der zu importierenden Azure-Modul-Anweisung am oberen Rand der Datei **server.js** hinzu:

```
var serviceBusService = azure.createServiceBusService();
```

Mithilfe des **CreateTopicIfNotExists** auf dem **ServiceBusService** -Objekt, das angegebene Thema wird (falls vorhanden) zurückgegeben werden, oder ein neues Thema mit dem angegebenen Namen erstellt werden. Im folgenden Code wird **CreateTopicIfNotExists** beim Erstellen oder Verbinden mit dem Thema mit dem Namen 'MyTopic':

```
serviceBusService.createTopicIfNotExists('MyTopic',function(error){
    if(!error){
        // Topic was created or exists
        console.log('topic created or exists.');
    }
});
```

**CreateServiceBusService** unterstützt auch zusätzliche Optionen, mit die Sie die Standardeinstellungen für Thema wie Nachrichtzeit, live oder Thema maximale Größe außer Kraft setzen können. Im folgenden Beispiel wird die Größe der maximalen Thema bis 5GB mit einer Zeit von 1 Minute Live:

```
var topicOptions = {
        MaxSizeInMegabytes: '5120',
        DefaultMessageTimeToLive: 'PT1M'
    };

serviceBusService.createTopicIfNotExists('MyTopic', topicOptions, function(error){
    if(!error){
        // topic was created or exists
    }
});
```

### <a name="filters"></a>Filter

Optionale Filtervorgängen können auf Operationen mit **ServiceBusService**angewendet werden. Filtervorgängen kann einbeziehen Protokollierung, automatisch wiederholen, usw. Filter sind Objekte, die eine Methode mit der Signatur implementieren:

```
function handle (requestOptions, next)
```

Nach Durchführung preprocessing auf die Optionen für die Anforderung, ruft die Methode `next` einen Rückruf mit der folgenden Signatur übergeben:

```
function (returnObject, finalCallback, next)
```

In diesem Rückruf und nach der Verarbeitung **ReturnObject** (der Antwort aus der Anforderung an den Server) Aufrufen der Rückruf muss entweder weiter, falls vorhanden, damit andere Filter Verarbeitung fortzusetzen oder einfach **FinalCallback** andernfalls aufzurufen, um den Dienst aufrufen zu beenden.

Zwei Filter, die "Wiederholen" Logik implementieren sind im Azure SDK für Node.js, **ExponentialRetryPolicyFilter** und **LinearRetryPolicyFilter**enthalten. Die folgenden erstellt ein **ServiceBusService** -Objekt, das die **ExponentialRetryPolicyFilter**verwendet:

    var retryOperations = new azure.ExponentialRetryPolicyFilter();
    var serviceBusService = azure.createServiceBusService().withFilter(retryOperations);

## <a name="create-subscriptions"></a>Erstellen von Abonnements

Thema Abonnements werden auch mit dem Objekt **ServiceBusService** erstellt. Abonnements können sind benannte und einen optionalen Filter, der Reihe von Nachrichten, die in das Abonnement des virtuelle Warteschlange beschränkt.

> [AZURE.NOTE] Abonnements sind beständig und bleibt weiterhin bestehen bis entweder diese oder das Thema sie sind verknüpft mit werden gelöscht. Wenn die Anwendung Logik zum Erstellen eines Abonnements enthält, wird zunächst überprüft, wenn das Abonnement bereits vorhanden ist, indem Sie mithilfe der Methode **GetSubscription** .

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Erstellen Sie ein Abonnement mit den Standardfilter (MatchAll)

Die **MatchAll** wird der Standardfilter, der verwendet wird, wenn kein Filter angegeben ist, wenn ein neues Abonnement erstellt wird. Wenn der Filters **MatchAll** verwendet wird, werden alle Nachrichten mit dem Thema veröffentlicht in virtuelle Warteschlange das Abonnement des platziert. Im folgenden Beispiel wird ein Abonnement mit dem Namen 'AllMessages' erstellt und den standardmäßigen **MatchAll** Filter verwendet.

```
serviceBusService.createSubscription('MyTopic','AllMessages',function(error){
    if(!error){
        // subscription created
    }
});
```

### <a name="create-subscriptions-with-filters"></a>Erstellen von Abonnements mit Filter

Sie können auch Filter erstellen, mit die Hilfe Sie Umfang der gesendete Nachrichten zu einem Thema in einem bestimmten Thema Abonnement anzeigen soll.

Der am häufigsten flexible Typ des Filters von Abonnements unterstützt wird **SqlFilter**, die eine Teilmenge der SQL92 implementiert. SQL-Filter arbeiten mit den Eigenschaften der Nachrichten, die mit dem Thema veröffentlicht werden. Für weitere Details zu den Ausdrücken, die mit einer SQL-Filter verwendet werden können, Überprüfen der [SqlFilter.SqlExpression] [ SqlFilter.SqlExpression] Syntax.

Filter können auf ein Abonnement mithilfe der Methode **CreateRule** des Objekts **ServiceBusService** hinzugefügt werden. Diese Methode können Sie neue Filter zu einem vorhandenen Abonnement hinzufügen.

> [AZURE.NOTE] Da der Standardfilter automatisch auf alle neuen Abonnements angewendet wird, müssen Sie zuerst den Standardfilter entfernen oder die **MatchAll** überschreibt alle anderen Filter, die Sie angeben können. Sie können die standardmäßigen Regel mithilfe der Methode **DeleteRule** des Objekts **ServiceBusService** entfernen.

Im folgenden Beispiel wird ein Abonnement mit dem Namen `HighMessages` mit einer **SqlFilter** , die nur Nachrichten markiert, die eine benutzerdefinierte **Messagenumber** -Eigenschaft größer als 3 verfügen:

```
serviceBusService.createSubscription('MyTopic', 'HighMessages', function (error){
    if(!error){
        // subscription created
        rule.create();
    }
});
var rule={
    deleteDefault: function(){
        serviceBusService.deleteRule('MyTopic',
            'HighMessages', 
            azure.Constants.ServiceBusConstants.DEFAULT_RULE_NAME, 
            rule.handleError);
    },
    create: function(){
        var ruleOptions = {
            sqlExpressionFilter: 'messagenumber > 3'
        };
        rule.deleteDefault();
        serviceBusService.createRule('MyTopic', 
            'HighMessages', 
            'HighMessageFilter', 
            ruleOptions, 
            rule.handleError);
    },
    handleError: function(error){
        if(error){
            console.log(error)
        }
    }
}
```

Im folgenden Beispiel wird auf ähnliche Weise ein Abonnement mit dem Namen `LowMessages` mit einer **SqlFilter** , die nur Nachrichten markiert, die eine **Messagenumber** -Eigenschaft kleiner oder gleich 3 aufweisen:

```
serviceBusService.createSubscription('MyTopic', 'LowMessages', function (error){
    if(!error){
        // subscription created
        rule.create();
    }
});
var rule={
    deleteDefault: function(){
        serviceBusService.deleteRule('MyTopic',
            'LowMessages', 
            azure.Constants.ServiceBusConstants.DEFAULT_RULE_NAME, 
            rule.handleError);
    },
    create: function(){
        var ruleOptions = {
            sqlExpressionFilter: 'messagenumber <= 3'
        };
        rule.deleteDefault();
        serviceBusService.createRule('MyTopic', 
            'LowMessages', 
            'LowMessageFilter', 
            ruleOptions, 
            rule.handleError);
    },
    handleError: function(error){
        if(error){
            console.log(error)
        }
    }
}
```

Wenn eine Nachricht nun an gesendet `MyTopic`, es immer an Empfänger abonniert übermittelt der `AllMessages` Thema-Abonnement und selektives übermittelt, um Empfänger abonniert die `HighMessages` und `LowMessages` Thema Abonnements (in Abhängigkeit von der Inhalt der Nachricht).

## <a name="how-to-send-messages-to-a-topic"></a>Informationen zum Senden von Nachrichten an ein Thema

Zum Senden einer Nachricht mit einem Thema Dienstbus muss die Anwendung die Methode **SendTopicMessage** des Objekts **ServiceBusService** verwenden.
Zu Dienstbus Themen gesendete Nachrichten werden **BrokeredMessage** Objekte.
**BrokeredMessage** -Objekte weisen eine Reihe von Standardeigenschaften (z. B. **Bezeichnungsfeld** und **TimeToLive**), ein Wörterbuch, das zum Halten von bestimmter Eigenschaften für benutzerdefinierte Anwendung verwendet wird, und eine Zeichenfolge Datenblock. Anwendung kann Hauptteil der Nachricht festlegen, indem Sie einen Zeichenfolgenwert an die **SendTopicMessage** übergeben, und alle erforderlichen Standardeigenschaften werden durch Standardwerten ausgefüllt.

Im folgenden Beispiel wird veranschaulicht, wie fünf Nachrichten an 'MyTopic' testen zu senden. Notiz, die Wert der **Messagenumber** -Eigenschaft jeder Nachricht auf die Iteration der Schleife variieren (Dies legt fest, welche Abonnements erhalten sie):

```
var message = {
    body: '',
    customProperties: {
        messagenumber: 0
    }
}

for (i = 0;i < 5;i++) {
    message.customProperties.messagenumber=i;
    message.body='This is Message #'+i;
    serviceBusService.sendTopicMessage(topic, message, function(error) {
      if (error) {
        console.log(error);
      }
    });
}
```

Dienstbus Themen unterstützen eine maximale Nachrichtengröße 256 KB im [Standard Ebene](service-bus-premium-messaging.md) und 1 MB im [Premium Ebene](service-bus-premium-messaging.md)an. Die Kopfzeile, die die von Standardfarben und benutzerdefinierten Anwendungseigenschaften enthält, kann eine maximale Größe von 64 KB haben. Es gibt keine Beschränkung der Anzahl von Nachrichten, die in einem Thema frei, aber ein Linienende auf die Gesamtgröße der Nachrichten frei, indem Sie ein Thema vorhanden ist. Dieses Thema Größe wird zum Zeitpunkt der Erstellung, mit einer Obergrenze von 5 GB definiert.

## <a name="receive-messages-from-a-subscription"></a>Empfangen von Nachrichten aus einem Abonnement

Nachrichten, die aus einem Abonnement mithilfe der **ReceiveSubscriptionMessage** -Methode auf das Objekt **ServiceBusService** empfangen werden. Standardmäßig werden Nachrichten aus dem Abonnement gelöscht, gelesen werden. Sie können jedoch (anheften) lesen und Sperren die Nachricht, ohne es aus dem Abonnement löschen, indem die optionalen Parameter **IsPeekLock** auf **true**.

Das Standardverhalten der lesen und löschen die Nachricht als Teil der Receive-Methode ist die einfachste Modell und am besten geeignet für Szenarien, die in denen Anwendung tolerieren kann nicht verarbeiten einer Nachricht im Fall eines Fehlers. Um dies zu verstehen, sollten Sie ein Szenario, in dem der Verbraucher gibt die Anforderung empfangen und dann stürzt ab, bevor Sie ihn aufbereiten, aus. Da Dienstbus die Nachricht als verbraucht markiert haben wird, wird Klicken Sie dann, wenn die Anwendung neu gestartet und beginnt, Nachrichten erneut, es die Nachricht verpasst haben, die vor der Absturz verbraucht wurde.

Wenn der Parameter **IsPeekLock** auf **true**festgelegt ist, wird die empfangen einen Vorgang zwei Phasen, aus der zu Support Applikationen ermöglicht, die fehlende Nachrichten tolerieren. Wenn Dienstbus eine Anforderung empfängt, findet die nächste Nachricht verbraucht werden, wenn es, sperren, um zu verhindern, dass andere Nutzer, die sie empfangen und gibt es dann an die Anwendung.
Nach die Anwendung endet Verarbeiten der Nachricht (oder zuverlässig für die Verarbeitung von zukünftigen gespeichert), schließt es den endgültigen des Prozesses empfangen durch Aufrufen von **DeleteMessage** Methode und Bereitstellen von der Nachricht als Parameter gelöscht werden soll. Die Methode **DeleteMessage** kennzeichnen der Nachricht als verbraucht wird und entfernen es aus dem Abonnement.

Im folgende Beispiel wird veranschaulicht, wie Nachrichten empfangen werden können, und mit **ReceiveSubscriptionMessage**verarbeitet. Im Beispiel zuerst empfängt und Löschen einer Nachricht aus dem Abonnement 'LowMessages' und anschließend eine Meldung aus dem 'HighMessages' Abonnement mit **IsPeekLock** Satz true empfangen. Anschließend wird die Mitteilung mithilfe einer **DeleteMessage**gelöscht:

```
serviceBusService.receiveSubscriptionMessage('MyTopic', 'LowMessages', function(error, receivedMessage){
    if(!error){
        // Message received and deleted
        console.log(receivedMessage);
    }
});
serviceBusService.receiveSubscriptionMessage('MyTopic', 'HighMessages', { isPeekLock: true }, function(error, lockedMessage){
    if(!error){
        // Message received and locked
        console.log(lockedMessage);
        serviceBusService.deleteMessage(lockedMessage, function (deleteError){
            if(!deleteError){
                // Message deleted
                console.log('message has been deleted.');
            }
        }
    }
});
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Behandlung von Anwendung stürzt ab und kann nicht gelesen werden Nachrichten

Dienstbus Funktionsumfang Sie ordnungsgemäß von Fehlern in Ihrer Anwendung oder Ansprechpartner Verarbeiten einer Nachricht wiederherstellen können. Ist eine Anwendung Empfänger zum Verarbeiten der Nachricht aus irgendeinem Grund können nicht genutzt werden, können sie die Methode **UnlockMessage** auf das Objekt **ServiceBusService** aufrufen. Dadurch wird Dienstbus zum Aufheben der Sperre der Nachricht innerhalb des Abonnements und Verfügbarmachen erneut, von der gleichen in Anspruch nehmen Anwendung oder von einer anderen in Anspruch nehmen Anwendung empfangen werden.

Es ist ebenfalls ein Timeout einer Nachricht in das Abonnement gesperrt, und Ausfall die Anwendung zum Verarbeiten der Nachricht, bevor Sie das Sperrungstimeout läuft ab (z. B., wenn die Anwendung stürzt ab), Dienstbus die Nachricht automatisch die Sperre aufhebt und erneut empfangen werden bereitgestellt.

Den Fall, dass die Anwendung stürzt ab, nach dem Verarbeiten der Nachricht, aber vor dem die **DeleteMessage** Methode ist, wird dann die Nachricht an die Anwendung zugestellt nach dem Neustart wird. Dies ist häufig bezeichnet, **Sobald Sie mindestens Verarbeitung**, d. h., jede Nachricht wird mindestens einmal verarbeitet werden jedoch in bestimmten Situationen möglicherweise die gleiche Nachricht erneut werden. Wenn Sie das Szenario doppelte Verarbeitung tolerieren kann, sollten Entwickler an ihrer Anwendung, doppelte Nachrichtenübermittlung behandeln zusätzliche Logik hinzufügen. Dies geschieht häufig mithilfe der **Nachrichten-ID** -Eigenschaft der Nachricht, die über die Übermittlungsversuche konstant bleiben wird.

## <a name="delete-topics-and-subscriptions"></a>Löschen von Themen und Abonnements

Themen und Abonnements sind beständig, und müssen über das [Azure klassischen Portal][] oder programmgesteuert explizit gelöscht werden.
Im folgenden Beispiel wird veranschaulicht, wie das Thema mit der Bezeichnung löschen `MyTopic`:

    serviceBusService.deleteTopic('MyTopic', function (error) {
        if (error) {
            console.log(error);
        }
    });

Löschen eines Themas werden alle Abonnements, die mit dem Thema registriert sind, ebenfalls gelöscht. Abonnements können auch unabhängig voneinander gelöscht werden. Im folgenden Beispiel wird veranschaulicht, wie ein Abonnement mit dem Namen löschen `HighMessages` aus der `MyTopic` Thema:

    serviceBusService.deleteSubscription('MyTopic', 'HighMessages', function (error) {
        if(error) {
            console.log(error);
        }
    });

## <a name="next-steps"></a>Nächste Schritte

Die Grundlagen des Dienstbus Themen bearbeitet haben, führen Sie die folgenden Links, um weitere Informationen.

-   Finden Sie unter [Warteschlangen, Themen, und Abonnements][].
-   -API-Referenz für [SqlFilter][].
-   Besuchen Sie das [Azure SDK für Knoten][] Repository GitHub.

  [Azure SDK für Knoten]: https://github.com/Azure/azure-sdk-for-node
  [Azure klassischen-portal]: https://manage.windowsazure.com
  [SqlFilter.SqlExpression]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
  [Warteschlangen, Themen und Abonnements]: service-bus-queues-topics-subscriptions.md
  [SqlFilter]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx
  [Node.js Cloud-Dienst]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
  [Erstellen und Bereitstellen einer Anwendungs Node.js eine Azure-Website]: ../app-service-web/web-sites-nodejs-develop-deploy-mac.md
  [Node.js Cloud-Dienst mit Speicher]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
  [Node.js Webanwendung mit Speicher]: ../cloud-services/storage-nodejs-use-table-storage-cloud-service-app.md
 

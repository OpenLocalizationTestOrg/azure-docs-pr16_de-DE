<properties
    pageTitle="Azure Funktionen Dienstbus Trigger und Bindungen | Microsoft Azure"
    description="Verstehen Sie, wie Azure-Dienstbus Trigger und Bindungen in Azure-Funktionen verwenden."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure-Funktionen, Funktionen Verarbeitung von Ereignissen, dynamische berechnen, ohne Server Architektur"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="08/22/2016"
    ms.author="chrande; glenga"/>

# <a name="azure-functions-service-bus-triggers-and-bindings-for-queues-and-topics"></a>Azure Funktionen Dienstbus Trigger und Bindungen für Warteschlangen und Themen

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

In diesem Artikel wird erläutert, wie konfigurieren und Code Azure-Dienstbus Trigger Bindungen in Azure-Funktionen. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a name="a-idsbtriggera-service-bus-queue-or-topic-trigger"></a><a id="sbtrigger"></a>Dienstbus Warteschlange oder Thema trigger

#### <a name="functionjson"></a>Function.JSON

Die *function.json* legt die folgenden Eigenschaften fest.

- `name`: Der Variablenname Funktion Code für die Warteschlange Thema oder die Warteschlange oder Thema Nachricht verwendet wird. 
- `queueName`: Für Warteschlange lösen Sie nur den Namen der Warteschlange abgefragt werden soll aus.
- `topicName`: Für Thema lösen Sie nur den Namen des Themas abgefragt werden soll aus.
- `subscriptionName`: Für Thema lösen Sie nur den Namen des Abonnements aus.
- `connection`: Der Name einer app-Einstellung, die eine Dienstbus Verbindungszeichenfolge enthält. Die Verbindungszeichenfolge muss für einen Dienstbus Namespace, die nicht zu einer bestimmten Warteschlange oder Thema beschränkt. Wenn die Verbindungszeichenfolge nicht zu Rechte verwalten haben, legen Sie die `accessRights` Eigenschaft. Wenn Sie verlassen `connection` leer ist, der Trigger oder Bindung funktioniert mit der standardmäßigen Dienstbus-Verbindungszeichenfolge für die Funktion-app, die durch die Einstellung der app AzureWebJobsServiceBus angegeben ist.
- `accessRights`: Gibt die Zugriffsrechte für die Verbindungszeichenfolge verfügbar. Ist der Standardwert `manage`. Legen Sie auf `listen` Berechtigungen verwalten, wenn Sie eine Verbindungszeichenfolge verwenden, die zur Verfügung steht. Andernfalls verwalten die Laufzeit, versuchen Sie es möglicherweise Funktionen und Proxy-Aktionen ausführen, die erfordern Rechte.
- `type`: *ServiceBusTrigger*muss festgelegt werden.
- `direction`: Muss *im*festgelegt werden. 

Beispiel für *function.json* für eine Dienstbus Warteschlange auslösen:

```json
{
  "bindings": [
    {
      "queueName": "testqueue",
      "connection": "MyServiceBusConnection",
      "name": "myQueueItem",
      "type": "serviceBusTrigger",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

#### <a name="c-code-example-that-processes-a-service-bus-queue-message"></a>C#-Codebeispiel, bei dem eine Dienstbus Warteschlange Nachricht verarbeitet

```csharp
public static void Run(string myQueueItem, TraceWriter log)
{
    log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
}
```

#### <a name="f-code-example-that-processes-a-service-bus-queue-message"></a>F-Codebeispiel, bei dem eine Dienstbus Warteschlange Nachricht verarbeitet

```fsharp
let Run(myQueueItem: string, log: TraceWriter) =
    log.Info(sprintf "F# ServiceBus queue trigger function processed message: %s" myQueueItem)
```

#### <a name="nodejs-code-example-that-processes-a-service-bus-queue-message"></a>Node.js Code-Beispiel, bei dem eine Dienstbus Warteschlange Nachricht verarbeitet

```javascript
module.exports = function(context, myQueueItem) {
    context.log('Node.js ServiceBus queue trigger function processed message', myQueueItem);
    context.done();
};
```

#### <a name="supported-types"></a>Unterstützte Datentypen

Die Nachricht Dienstbus Warteschlange kann eine der folgenden Typen deserialisiert werden:

* Objekt (in JSON)
* Zeichenfolge
* Byte-array 
* `BrokeredMessage`(C#) 

#### <a name="a-idsbpeeklocka-peeklock-behavior"></a><a id="sbpeeklock"></a>PeekLock Verhalten

Die Funktionen Laufzeit erhält eine Nachricht in `PeekLock` Modus und Anrufe `Complete` auf die Nachricht, wenn die Funktion erfolgreich abgeschlossen wird, oder Anrufe `Abandon` Wenn die Funktion fehlschlägt. Wenn die Funktion mehr als ausgeführt wird die `PeekLock` Timeout, die Sperre wird automatisch erneuert.

#### <a name="a-idsbpoisona-poison-message-handling"></a><a id="sbpoison"></a>Handhabung beschädigter Nachrichten

Dienstbus hat eine eigene beschädigte Nachricht Umgang mit dem kann nicht gesteuert oder in Azure Funktionen Konfiguration oder Coderessource konfiguriert. 

#### <a name="a-idsbsinglethreada-single-threading"></a><a id="sbsinglethread"></a>Singlethreading

Laufzeit verarbeitet mehrere Warteschlangennachrichten standardmäßig die Funktionen gleichzeitig aus. Wenn die Laufzeit jeweils nur eine einzelne Warteschlange oder Nachricht Thema Verarbeitungszeit umgeleitet werden sollen, legen Sie `serviceBus.maxConcurrrentCalls` 1 in der Datei *host.json* . Informationen zur Datei *host.json* finden Sie unter [Ordnerstruktur](functions-reference.md#folder-structure) Entwicklertools Bezug Artikel, und [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) im WebJobs.Script Repository-Wiki.

## <a name="a-idsboutputa-service-bus-queue-or-topic-output-binding"></a><a id="sboutput"></a>Dienstbus Warteschlange oder ein Thema ausgeben Bindung

#### <a name="functionjson"></a>Function.JSON

Die *function.json* legt die folgenden Eigenschaften fest.

- `name`: Der Variablenname Funktion Code für die Warteschlange oder Warteschlange Nachricht verwendet wird. 
- `queueName`: Für Warteschlange lösen Sie nur den Namen der Warteschlange abgefragt werden soll aus.
- `topicName`: Für Thema lösen Sie nur den Namen des Themas abgefragt werden soll aus.
- `subscriptionName`: Für Thema lösen Sie nur den Namen des Abonnements aus.
- `connection`: Gleiche wie für Dienstbus auslösen.
- `accessRights`: Gibt die Zugriffsrechte für die Verbindungszeichenfolge verfügbar. Ist der Standardwert `manage`. Legen Sie auf `send` Berechtigungen verwalten, wenn Sie eine Verbindungszeichenfolge verwenden, die zur Verfügung steht. Andernfalls verwalten die Laufzeit, versuchen Sie es möglicherweise Funktionen und Proxy-Aktionen ausführen, die erfordern Rechte, wie das Erstellen von Warteschlangen.
- `type`: *ServiceBus*müssen festgelegt werden.
- `direction`: Muss *sich*festgelegt werden. 

Beispiel für *function.json* für die Verwendung eines Triggers Zeitmessung Dienstbus Warteschlangennachrichten zu schreiben:

```JSON
{
  "bindings": [
    {
      "schedule": "0/15 * * * * *",
      "name": "myTimer",
      "runsOnStartup": true,
      "type": "timerTrigger",
      "direction": "in"
    },
    {
      "name": "outputSbQueue",
      "type": "serviceBus",
      "queueName": "testqueue",
      "connection": "MyServiceBusConnection",
      "direction": "out"
    }
  ],
  "disabled": false
}
``` 

#### <a name="supported-types"></a>Unterstützte Datentypen

Azure Funktionen können eine Dienstbus Warteschlange-Nachricht von einem beliebigen der folgenden Typen erstellen.

* Objekt (immer erstellt eine JSON-Nachricht, die Nachricht mit einem null-Objekt erstellt, wenn der Wert null ist, nach Beendigung der Funktion)
* Zeichenfolge (erstellt eine Nachricht, wenn der Wert ungleich Null ist, nach Beendigung der Funktion)
* Byte-Array (funktioniert nicht wie Zeichenfolge) 
* `BrokeredMessage`(C#, arbeitet wie Zeichenfolge)

Erstellen mehrerer Nachrichten in einer C#-Funktion können Sie `ICollector<T>` oder `IAsyncCollector<T>`. Wenn Sie anrufen, wird eine Nachricht erstellt die `Add` Methode.

#### <a name="c-code-examples-that-create-service-bus-queue-messages"></a>C#-Codebeispiele, bei die Dienstbus Warteschlangennachrichten zu erstellen.

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log, out string outputSbQueue)
{
    string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.Info(message); 
    outputSbQueue = message;
}
```

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log, ICollector<string> outputSbQueue)
{
    string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.Info(message); 
    outputSbQueue.Add("1 " + message);
    outputSbQueue.Add("2 " + message);
}
```

#### <a name="f-code-example-that-creates-a-service-bus-queue-message"></a>F-Codebeispiel, die eine Dienstbus Warteschlange-Nachricht erstellt wird.

```fsharp
let Run(myTimer: TimerInfo, log: TraceWriter, outputSbQueue: byref<string>) =
    let message = sprintf "Service Bus queue message created at: %s" (DateTime.Now.ToString())
    log.Info(message)
    outputSbQueue = message
```

#### <a name="nodejs-code-example-that-creates-a-service-bus-queue-message"></a>Node.js Code-Beispiel, bei dem eine Dienstbus Warteschlange-Nachricht erstellt.

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();
    
    if(myTimer.isPastDue)
    {
        context.log('Node.js is running late!');
    }
    var message = 'Service Bus queue message created at ' + timeStamp;
    context.log(message);   
    context.bindings.outputSbQueueMsg = message;
    context.done();
};
```

## <a name="next-steps"></a>Nächste Schritte

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)] 

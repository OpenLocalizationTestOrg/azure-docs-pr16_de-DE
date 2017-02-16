<properties
    pageTitle="Azure Funktionen Ereignis Hub Bindungen | Microsoft Azure"
    description="Verstehen Sie, wie Azure Ereignis Hub Bindungen in Azure-Funktionen verwenden."
    services="functions"
    documentationCenter="na"
    authors="wesmc7777"
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
    ms.date="10/17/2016"
    ms.author="wesmc"/>

# <a name="azure-functions-event-hub-bindings"></a>Azure Funktionen Ereignis Hub Bindungen

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

In diesem Artikel wird erläutert, wie so konfigurieren Sie Code [Azure Ereignis Hub](../event-hubs/event-hubs-overview.md) Bindungen für Azure-Funktionen. Azure Funktionen unterstützt Trigger und Ausgabe Bindungen für Azure Ereignis Hubs an.

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

Wenn Sie neu bei Azure Ereignis Hubs sind, finden Sie unter der [Azure Ereignis Hub Übersicht](../event-hubs/event-hubs-overview.md).

## <a name="azure-event-hub-trigger-binding"></a>Azure Ereignis Hub Bindung auslösen

Ein Ereignis-Hub Azure Trigger kann verwendet werden, reagieren auf ein Ereignis an ein Ereignis Hub Ereignisstream gesendet. Sie müssen Lesezugriff auf das Ereignis Hub zum Einrichten einer Bindung auslösen.

#### <a name="functionjson-for-event-hub-trigger-binding"></a>Function.JSON für Ereignis Hub auslösen Bindung

Die Datei *function.json* für ein Ereignis-Hub Azure Trigger gibt die folgenden Eigenschaften an:

- `type`: *EventHubTrigger*muss festgelegt werden.
- `name`: Der Variablenname Funktion Code für das Ereignis Hub Nachricht verwendet wird. 
- `direction`: Muss *im*festgelegt werden. 
- `path`: Der Name des Ereignisses-Hub.
- `consumerGroup`: Dies ist eine optionale Eigenschaft verwendet, um der [Gruppe Consumer](../event-hubs-overview.md#consumer-groups) verwendet, um Ereignisse im Hub abonnieren festzulegen. Wenn nicht angegeben, die `$Default` Consumer Gruppe verwendet wird. 
- `connection`: Der Name einer app-Einstellung, die die Verbindungszeichenfolge auf den Namespace, die enthält in der Ereignis-Hub befindet. Kopieren Sie diese Verbindungszeichenfolge durch Klicken auf die Schaltfläche **Verbindungsinformationen** für den Namespace, nicht im Ereignis Hub selbst.  Diese Verbindungszeichenfolge muss mindestens Berechtigungen zum Aktivieren des Triggers gelesen haben.

        {
          "bindings": [
            {
              "type": "eventHubTrigger",
              "name": "myEventHubMessage",
              "direction": "in",
              "path": "MyEventHub",
              "connection": "myEventHubReadConnectionString"
            }
          ],
          "disabled": false
        }

#### <a name="azure-event-hub-trigger-c-example"></a>Beispiel für einen Trigger c# Azure Ereignis Hub
 
Verwenden die oben genannten Beispiel function.json, werden im Nachrichtentext Ereignis protokolliert C#-Funktion folgenden Code:
 
    using System;
    
    public static void Run(string myEventHubMessage, TraceWriter log)
    {
        log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
    }

#### <a name="azure-event-hub-trigger-f-example"></a>Beispiel für einen Trigger F Nr. Azure Ereignis Hub

Verwenden die oben genannten Beispiel function.json, werden im Nachrichtentext Ereignis protokolliert F-Funktion folgenden Code:

    let Run(myEventHubMessage: string, log: TraceWriter) =
        log.Info(sprintf "F# eventhub trigger function processed work item: %s" myEventHubMessage)

#### <a name="azure-event-hub-trigger-nodejs-example"></a>Azure Ereignis Hub Trigger Node.js-Beispiel
 
Verwenden die oben genannten Beispiel function.json, werden im Nachrichtentext Ereignis protokolliert Node.js Funktion folgenden Code:
 
    module.exports = function (context, myEventHubMessage) {
        context.log('Node.js eventhub trigger function processed work item', myEventHubMessage);    
        context.done();
    };


## <a name="azure-event-hub-output-binding"></a>Azure Ereignis Hub ausgeben Bindung

Ein Ereignis Azure-Hub Ausgabe Bindung wird verwendet, um Ereignisse in ein Ereignis Hub Ereignisstream schreiben. Sie benötigen senden Berechtigung ein Ereignis Hub Ereignisse zu schreiben. 

#### <a name="functionjson-for-event-hub-output-binding"></a>Function.JSON für Ereignis Hub ausgeben Bindung

Die Datei *function.json* für ein Ereignis Azure-Hub ausgeben, dass Bindung gibt die folgenden Eigenschaften an:

- `type`: *EventHub*muss festgelegt werden.
- `name`: Der Variablenname Funktion Code für das Ereignis Hub Nachricht verwendet wird. 
- `path`: Der Name des Ereignisses-Hub.
- `connection`: Der Name einer app-Einstellung, die die Verbindungszeichenfolge auf den Namespace, die enthält in der Ereignis-Hub befindet. Kopieren Sie diese Verbindungszeichenfolge durch Klicken auf die Schaltfläche **Verbindungsinformationen** für den Namespace, nicht im Ereignis Hub selbst.  Diese Verbindungszeichenfolge müssen senden Berechtigungen zum Senden der Nachricht in den Stream Ereignis-Hub an.
- `direction`: Muss *sich*festgelegt werden. 

        {
          "type": "eventHub",
          "name": "outputEventHubMessage",
          "path": "myeventhub",
          "connection": "MyEventHubSend",
          "direction": "out"
        }


#### <a name="azure-event-hub-c-code-example-for-output-binding"></a>Azure Ereignis Hub C#-Codebeispiel für die Ausgabe Bindung
 
Der folgende C#-Funktion aus veranschaulicht das Schreiben eines Ereignisses in einem Ereignis Hub Ereignisstream. Dieses Beispiel entspricht dem Ereignis Hub Bindung abgebildet Ausgeben einer C#-Timer Trigger angewendet.  
 
    using System;
    
    public static void Run(TimerInfo myTimer, out string outputEventHubMessage, TraceWriter log)
    {
        String msg = $"TimerTriggerCSharp1 executed at: {DateTime.Now}";
    
        log.Verbose(msg);   
        
        outputEventHubMessage = msg;
    }

#### <a name="azure-event-hub-f-code-example-for-output-binding"></a>Azure Ereignis Hub F-Codebeispiel für die Ausgabe Bindung

Der folgende F-Beispiel Funktionscode veranschaulicht Schreiben eines Ereignisses in einem Ereignis Hub Ereignisstream. Dieses Beispiel entspricht dem Ereignis Hub Bindung abgebildet ausgeben angewendete einer C#-Zeitgeber auslösen.

    let Run(myTimer: TimerInfo, outputEventHubMessage: byref<string>, log: TraceWriter) =
        let msg = sprintf "TimerTriggerFSharp1 executed at: %s" DateTime.Now.ToString()
        log.Verbose(msg);
        outputEventHubMessage <- msg;

#### <a name="azure-event-hub-nodejs-code-example-for-output-binding"></a>Azure Ereignis Hub Node.js Beispiels für Ausgabe Bindung
 
Der folgende Node.js Funktion aus veranschaulicht das Schreiben eines Ereignisses in einem Ereignis Hub Ereignisstream. Dieses Beispiel entspricht dem Ereignis Hub Bindung abgebildet ausgeben Node.js Timer Trigger angewendet.  
 
    module.exports = function (context, myTimer) {
        var timeStamp = new Date().toISOString();
        
        if(myTimer.isPastDue)
        {
            context.log('TimerTriggerNodeJS1 is running late!');
        }

        context.log('TimerTriggerNodeJS1 function ran!', timeStamp);   
        
        context.bindings.outputEventHubMessage = "TimerTriggerNodeJS1 ran at : " + timeStamp;
    
        context.done();
    };

## <a name="next-steps"></a>Nächste Schritte

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

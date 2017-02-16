<properties
    pageTitle="Azure Funktionen Twilio Bindung | Microsoft Azure"
    description="Verstehen Sie, wie Twilio Bindungen Azure-Funktionen verwenden."
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
    ms.date="10/20/2016"
    ms.author="wesmc"/>

# <a name="azure-functions-twilio-output-binding"></a>Azure Funktionen Twilio ausgeben Bindung

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

In diesem Artikel wird erläutert, wie konfigurieren und Twilio Bindungen mit Azure-Funktionen verwenden. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

Azure Funktionen unterstützt ausgeben Twilio Bindungen zum Aktivieren der Funktionen zum Senden von SMS-Textnachrichten mit wenigen Codezeilen und ein [Twilio](https://www.twilio.com/) -Konto an. 
 

## <a name="functionjson-for-azure-notification-hub-output-binding"></a>Function.JSON für Azure Benachrichtigung Hub ausgeben Bindung

Die Datei function.json bietet folgende Eigenschaften:

- `name`: Variablenname Funktion Code für die Twilio SMS-Textnachricht.
- `type`: muss auf *"TwilioSms"*festgelegt werden.
- `accountSid`: Dieser Wert muss auf den Namen einer App-Einstellung festgelegt werden, die Ihre Twilio Konto Sid enthält.
- `authToken`: Dieser Wert muss auf den Namen einer App-Einstellung festgelegt werden, die Ihre Twilio Authentifizierungstoken enthält.
- `to`: Dieser Wert wird an die Rufnummer festgelegt, die an der SMS-Text gesendet wird.
- `from`: Dieser Wert wird an die Rufnummer festgelegt, die von der SMS-Text gesendet wird.
- `direction`: muss auf *","*festgelegt werden.
- `body`: Dieser Wert kann verwendet werden, um die SMS-Textnachricht codieren, wenn Sie nicht in den Code für Ihre Funktion dynamisch festlegen müssen. 

 
Beispiel für function.json:

    {
      "type": "twilioSms",
      "name": "message",
      "accountSid": "TwilioAccountSid",
      "authToken": "TwilioAuthToken",
      "to": "+1704XXXXXXX",
      "from": "+1425XXXXXXX",
      "direction": "out",
      "body": "Azure Functions Testing"
    }


## <a name="example-c-queue-trigger-with-twilio-output-binding"></a>Beispiel für C#-Warteschlange Trigger mit Twilio ausgeben Bindung

#### <a name="synchronous"></a>Synchroner

Dieser Beispielcode synchron für einen Azure-Speicher Warteschlange Trigger verwendet Out-Parameter Textnachricht an einen Kunden senden, die Ordnung platziert.

    #r "Newtonsoft.Json"
    #r "Twilio.Api"

    using System;
    using Newtonsoft.Json;
    using Twilio;

    public static void Run(string myQueueItem, out SMSMessage message,  TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
    
        // In this example the queue item is a JSON string representing an order that contains the name of a 
        // customer and a mobile number to send text updates to.
        dynamic order = JsonConvert.DeserializeObject(myQueueItem);
        string msg = "Hello " + order.name + ", thank you for your order.";
    
        // Even if you want to use a hard coded message and number in the binding, you must at least 
        // initialize the SMSMessage variable.
        message = new SMSMessage();

        // A dynamic message can be set instead of the body in the output binding. In this example, we use 
        // the order information to personalize a text message to the mobile number provided for
        // order status updates.
        message.Body = msg;
        message.To = order.mobileNumber;
    }

#### <a name="asynchronous"></a>Asynchrone

Dieser Beispielcode asynchrone für einen Azure-Speicher Warteschlange Trigger sendet eine Textnachricht zu einem Kunden, der Ordnung platziert.

    #r "Newtonsoft.Json"
    #r "Twilio.Api"
     
    using System;
    using Newtonsoft.Json;
    using Twilio;
    
    public static async Task Run(string myQueueItem, IAsyncCollector<SMSMessage> message,  TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");

        // In this example the queue item is a JSON string representing an order that contains the name of a 
        // customer and a mobile number to send text updates to.
        dynamic order = JsonConvert.DeserializeObject(myQueueItem);
        string msg = "Hello " + order.name + ", thank you for your order.";
    
        // Even if you want to use a hard coded message and number in the binding, you must at least 
        // initialize the SMSMessage variable.
        SMSMessage smsText = new SMSMessage();

        // A dynamic message can be set instead of the body in the output binding. In this example, we use 
        // the order information to personalize a text message to the mobile number provided for
        // order status updates.
        smsText.Body = msg;
        smsText.To = order.mobileNumber;
        
        await message.AddAsync(smsText);
    }


## <a name="example-nodejs-queue-trigger-with-twilio-output-binding"></a>Beispiel für Node.js Warteschlange Trigger mit Twilio ausgeben Bindung

In diesem Beispiel Node.js sendet eine Textnachricht an einen Kunden, der Ordnung platziert.

    module.exports = function (context, myQueueItem) {
        context.log('Node.js queue trigger function processed work item', myQueueItem);
    
        // In this example the queue item is a JSON string representing an order that contains the name of a 
        // customer and a mobile number to send text updates to.
        var msg = "Hello " + myQueueItem.name + ", thank you for your order.";
    
        // Even if you want to use a hard coded message and number in the binding, you must at least 
        // initialize the message binding.
        context.bindings.message = {};
    
        // A dynamic message can be set instead of the body in the output binding. In this example, we use 
        // the order information to personalize a text message to the mobile number provided for
        // order status updates.
        context.bindings.message = {
            body : msg,
            to : myQueueItem.mobileNumber
        };
    
        context.done();
    };

## <a name="next-steps"></a>Nächste Schritte

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

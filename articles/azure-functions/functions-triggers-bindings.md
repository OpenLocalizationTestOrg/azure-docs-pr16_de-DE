<properties
    pageTitle="Azure Funktionen Trigger und Bindungen | Microsoft Azure"
    description="Verstehen Sie, wie Trigger und Bindungen in Azure-Funktionen verwenden."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure-Funktionen, Funktionen, Ereignisse zu verarbeiten, Webhooks, dynamische berechnen, ohne Server Architektur"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="10/27/2016"
    ms.author="chrande"/>

# <a name="azure-functions-triggers-and-bindings-developer-reference"></a>Azure Funktionen Trigger und Bindungen-Entwicklerreferenz


Dieses Thema bietet allgemeine Referenz für Trigger und Bindungen. Sie enthält einige der Features für die erweiterte Bindung und Syntax von allen Bindungstypen unterstützt.  

Wenn Sie ausführliche Informationen zu versehen konfigurieren und einen bestimmten Typ von Trigger oder Bindung Codierung suchen, möchten Sie möglicherweise durch Klicken auf eine der Trigger oder Bindungen stattdessen aufgeführten:

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)] 

Die folgenden Artikel wird davon ausgegangen, dass Sie der [Entwicklerreferenz Azure-Funktionen](functions-reference.md)und [c#](functions-reference-csharp.md), [F-](functions-reference-fsharp.md)oder [Node.js](functions-reference-node.md) Entwicklertools Bezug Artikel gelesen haben.



## <a name="overview"></a>(Übersicht)

Trigger sind Ereignis Antworten zum Auslösen des benutzerdefinierten Codes verwendet. Sie können Sie über die Azure-Plattform oder lokal auf Ereignisse zu reagieren. Bindungen stellen die erforderlichen Metadaten verwendet, um den gewünschten Trigger oder zugeordneten Eingabe- oder Ausgabedaten Code Verbindung dar.

Um eine bessere Vorstellung davon die anderen Bindungen zu gelangen, die Sie mit der Funktion Azure-app integrieren können, finden Sie in der folgenden Tabelle.

[AZURE.INCLUDE [dynamic compute](../../includes/functions-bindings.md)]  

Zum besseren Verständnis auslöst und Bindungen im Allgemeinen nehmen an, dass einige Code den Prozess ausführen ein neues Element in einer Warteschlange Azure-Speicher abgelegt werden soll. Azure Funktionen bietet einen Azure Warteschlange Trigger Unterstützung. Dazu müssten, die folgende Informationen zum Überwachen der Warteschlange:
 
- Das Speicherkonto, in dem die Warteschlange vorhanden ist.
- Der Name der Warteschlange.
- Ein Variablenname, die von Code verwenden würden, um den neuen Eintrag verweisen, die in der Warteschlange abgelegt wurde.  
 
Eine Warteschlange Trigger Bindung enthält diese Informationen für eine Azure-Funktion. Die Datei *function.json* für jede Funktion enthält alle zugehörige Bindungen.  Hier ein Beispiel *function.json* mit einer Warteschlange auslösen Bindung. 

    {
      "bindings": [
        {
          "name": "myNewUserQueueItem",
          "type": "queueTrigger",
          "direction": "in",
          "queueName": "queue-newusers",
          "connection": "MY_STORAGE_ACCT_APP_SETTING"
        }
      ],
      "disabled": false
    }

Code senden möglicherweise verschiedene Typen von Ausgabe, je nachdem, wie das neue Warteschlangenelement verarbeitet wird. Angenommen, möchten Sie einen neuen Datensatz in einer Tabelle Azure-Speicher zu schreiben.  Um dies zu erreichen, können Sie eine Ausgabe Bindung zu einer Tabelle Azure-Speicher einrichten. Hier ist ein Beispiel für *function.json* , die eine Speicher Tabelle Ausgabe Bindung enthält, die mit einer Warteschlange Trigger verwendet werden kann. 


    {
      "bindings": [
        {
          "name": "myNewUserQueueItem",
          "type": "queueTrigger",
          "direction": "in",
          "queueName": "queue-newusers",
          "connection": "MY_STORAGE_ACCT_APP_SETTING"
        },
        {
          "type": "table",
          "name": "myNewUserTableBinding",
          "tableName": "newUserTable",
          "connection": "MY_TABLE_STORAGE_ACCT_APP_SETTING",
          "direction": "out"
        }
      ],
      "disabled": false
    }


Die folgende C#-Funktion reagiert auf ein neues Element in der Warteschlange gelöscht wird und schreibt einen neuer Benutzereintrag in einer Tabelle Azure-Speicher.

    #r "Newtonsoft.Json"

    using System;
    using Newtonsoft.Json;

    public static async Task Run(string myNewUserQueueItem, IAsyncCollector<Person> myNewUserTableBinding, 
                                    TraceWriter log)
    {
        // In this example the queue item is a JSON string representing an order that contains the name, 
        // address and mobile number of the new customer.
        dynamic order = JsonConvert.DeserializeObject(myNewUserQueueItem);
    
        await myNewUserTableBinding.AddAsync(
            new Person() { 
                PartitionKey = "Test", 
                RowKey = Guid.NewGuid().ToString(), 
                Name = order.name,
                Address = order.address,
                MobileNumber = order.mobileNumber }
            );
    }
    
    public class Person
    {
        public string PartitionKey { get; set; }
        public string RowKey { get; set; }
        public string Name { get; set; }
        public string Address { get; set; }
        public string MobileNumber { get; set; }
    }

Weitere Codebeispiele zu und weitere spezifische Informationen zu Datentypen Azure-Speicher, die unterstützt werden, finden Sie unter [Trigger Azure-Funktionen und Bindungen für Azure-Speicher](functions-bindings-storage.md).


Wenn die erweiterten Bindungsfunktionen Azure-Portal verwenden möchten, klicken Sie auf die Option **Erweiterte-Editor** auf der Registerkarte **integrieren** Ihrer-Funktion. Der erweiterte Editor können Sie die *function.json* direkt im Portal bearbeiten.

## <a name="dynamic-parameter-binding"></a>Dynamische Parameter Bindung 

Statt eine statische Konfiguration für Ihre Ausgabe Bindungseigenschaften festlegen können Sie die Einstellungen dynamisch an Daten gebunden werden, die Teil Ihrer Auslösen der Eingabewerte Bindung ist konfigurieren. Betrachten Sie ein Szenario, wobei neue Bestellungen mithilfe einer Warteschlange Azure-Speicher verarbeitet werden. Jedes Warteschlangenelement der neuen ist eine JSON-Zeichenfolge, die mindestens die folgenden Eigenschaften enthält:

    {
      name : "Customer Name",
      address : "Customer's Address".
      mobileNumber : "Customer's mobile number."
    }

Möglicherweise möchten dem Kunden senden von SMS-Textnachricht mit Ihrem Konto Twilio als ein Update, dass die Reihenfolge empfangen wurde.  Sie konfigurieren können die `body` und `to` Feld mit Ihrer Twilio ausgeben Bindung dynamisch gebunden werden die `name` und `mobileNumber` , die Teil der Eingabe wie folgt wurden.

    {
      "name": "myNewOrderItem",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "queue-newOrders",
      "connection": "orders_STORAGE"
    },
    {
      "type": "twilioSms",
      "name": "message",
      "accountSid": "TwilioAccountSid",
      "authToken": "TwilioAuthToken",
      "to": "{mobileNumber}",
      "from": "+XXXYYYZZZZ",
      "body": "Thank you {name}, your order was received",
      "direction": "out"
    },
 
Funktionscode weist jetzt wie folgt Initialisierung der Output-Parameter. Während der Ausführung werden die Ausgabeeigenschaften an die gewünschten Eingabedaten gebunden werden.

C#

    // Even if you want to use a hard coded message and number in the binding, you must at least 
    // initialize the message variable.
    message = new SMSMessage();

    // When using dynamic parameter binding no need to set this in code.
    // message.body = msg;
    // message.to = myNewOrderItem.mobileNumber

Node.js

    context.bindings.message = {
        // When using dynamic parameter binding no need to set this in code.
        //body : msg,
        //to : myNewOrderItem.mobileNumber
    };




## <a name="random-guids"></a>Zufallszahl GUIDs

Azure Funktionen bietet eine Syntax zufällige GUIDs mit Ihrer Bindungen generieren. Die Bindungssyntax der folgenden wird Ausgabe in einer neuen BLOB mit einem eindeutigen Namen in einem Container Azure-Speicher schreiben: 

    {
      "type": "blob",
      "name": "blobOutput",
      "direction": "out",
      "path": "my-output-container/{rand-guid}"
    }



## <a name="returning-a-single-output"></a>Eine einzelne Ausgabe zurückgeben

In Fällen, in denen der Funktionscode eine einzelne Ausgabe zurückgibt, können Sie eine Ausgabe Bindung mit dem Namen `$return` natürlichere Funktionssignatur in Ihren Code beibehalten. Dies kann nur mit Sprachen verwendet werden, die einen Rückgabewert (c#, Node.js, F Nr.) unterstützen. Die Bindung sollte ähnlich wie die folgenden Blob Ausgabe Bindung sein, mit einer Warteschlange auslösen verwendet wird.

    {
      "bindings": [
        {
          "type": "queueTrigger",
          "name": "input",
          "direction": "in",
          "queueName": "test-input-node"
        },
        {
          "type": "blob",
          "name": "$return",
          "direction": "out",
          "path": "test-output-node/{id}"
        }
      ]
    }


Im folgende C#-Code gibt die Ausgabe besser skalieren, ohne eine `out` Parameter in der Signatur (Funktion).


    public static string Run(WorkItem input, TraceWriter log)
    {
        string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
        log.Info($"C# script processed queue message. Item={json}");
        return json;
    }

    // Async example
    public static Task<string> Run(WorkItem input, TraceWriter log)
    {
        string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
        log.Info($"C# script processed queue message. Item={json}");
        return json;
    }


Diese Vorgehensweise ist unten mit Node.js veranschaulicht.

    module.exports = function (context, input) {
        var json = JSON.stringify(input);
        context.log('Node.js script processed queue message', json);
        context.done(null, json);
    }

F-Beispiel unten.

    let Run(input: WorkItem, log: TraceWriter) =
        let json = String.Format("{{ \"id\": \"{0}\" }}", input.Id)   
        log.Info(sprintf "F# script processed queue message '%s'" json)
        json

Dies kann auch mit mehreren Output-Parameter verwendet werden, indem Sie eine einzelne Ausgabe mit definieren `$return`.


## <a name="route-support"></a>Routing-support

Beim Erstellen einer Funktion für eine HTTP-Trigger oder WebHook, wird die Funktion standardmäßig adressiert mit einer Routing des Formulars:

    http://<yourapp>.azurewebsites.net/api/<funcname> 

Sie können diese Routing mithilfe der optionalen anpassen `route` Eigenschaft für den HTTP-Trigger Eingabe des Bindung. Beispielsweise die folgende *function.json* Datei definiert eine `route` Eigenschaft für ein HTTP-Trigger:

    {
      "bindings": [
        {
          "type": "httpTrigger",
          "name": "req",
          "direction": "in",
          "methods": [ "get" ],
          "route": "products/{category:alpha}/{id:int?}"
        },
        {
          "type": "http",
          "name": "res",
          "direction": "out"
        }
      ]
    }

Mit dieser Konfiguration wird die Funktion jetzt mit den folgenden Routing anstelle der ursprünglichen Routing adressiert.

    http://<yourapp>.azurewebsites.net/api/products/electronics/357

Dadurch wird den Funktionscode zur Unterstützung von zwei Parametern in der Adresse, `category` und `id`. Sie können alle [Web API Routing Einschränkung](https://www.asp.net/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2#constraints) mit Ihrer Parameter verwenden. Im folgenden C#-Funktionscode der beiden Parameter verwendet.

    public static Task<HttpResponseMessage> Run(HttpRequestMessage request, string category, int? id, 
                                                    TraceWriter log)
    {
        if (id == null)
           return  req.CreateResponse(HttpStatusCode.OK, $"All {category} items were requested.");
        else
           return  req.CreateResponse(HttpStatusCode.OK, $"{category} item with id = {id} has been requested.");
    }

So sieht Node.js Funktionscode mit den gleichen Parameter weiterleiten aus.

    module.exports = function (context, req) {

        var category = context.bindingData.category;
        var id = context.bindingData.id;
    
        if (!id) {
            context.res = {
                // status: 200, /* Defaults to 200 */
                body: "All " + category + " items were requested."
            };
        }
        else {
            context.res = {
                // status: 200, /* Defaults to 200 */
                body: category + " item with id = " + id + " was requested."
            };
        }
    
        context.done();
    } 

Standardmäßig werden alle Funktion leitet *api*vorangestellt. Können Sie auch anpassen oder Entfernen der Präfix mithilfe der `http.routePrefix` Eigenschaft in der Datei *host.json* . Im folgende Beispiel entfernt das *api* Routing Präfix mithilfe eine leere Zeichenfolge für das Präfix in der Datei *host.json* .

    {
      "http": {
        "routePrefix": ""
      }
    }

Ausführliche Informationen zum Aktualisieren der Datei *host.json* für Ihre Funktion finden Sie unter [wie Funktion app zu aktualisieren Dateien](functions-reference.md#fileupdate). 

Durch Hinzufügen von dieser Konfiguration, ist die Funktion mit den folgenden Routing adressiert:

    http://<yourapp>.azurewebsites.net/products/electronics/357

Informationen zu anderen Eigenschaften, die Sie in der Datei *host.json* konfigurieren können, finden Sie unter [host.json verweisen](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).





## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter den folgenden Ressourcen:

* [Testen einer Funktion](functions-test-a-function.md)
* [Skalieren einer Funktion](functions-scale.md)

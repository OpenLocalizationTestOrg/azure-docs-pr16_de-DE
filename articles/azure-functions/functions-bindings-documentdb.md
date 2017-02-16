<properties
    pageTitle="Azure Funktionen DocumentDB Bindungen | Microsoft Azure"
    description="Verstehen Sie, wie Azure DocumentDB Bindungen in Azure-Funktionen verwenden."
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

# <a name="azure-functions-documentdb-bindings"></a>Azure Funktionen DocumentDB Bindungen

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

In diesem Artikel wird erläutert, wie so konfigurieren Sie Code Azure DocumentDB Bindungen in Azure-Funktionen. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a name="a-iddocdbinputa-azure-documentdb-input-binding"></a><a id="docdbinput"></a>Azure DocumentDB Eingabemethoden Bindung

Eingabe Bindungen können Laden ein Dokuments aus einer Sammlung DocumentDB und direkt an Ihre Bindung zu übergeben. Die Dokument-Id kann basierend auf den Trigger, der die Funktion aufgerufen ermittelt werden. In einer C#-Funktion werden an den Eintrag vorgenommenen Änderungen automatisch wieder in der Auflistung gesendet werden, wenn die Funktion erfolgreich beendet wird.

#### <a name="functionjson-for-documentdb-input-binding"></a>Function.JSON für die Eingabewerte Bindung DocumentDB

Die Datei *function.json* bietet folgende Eigenschaften:

- `name`: Variablenname Funktion Code für das Dokument verwendet.
- `type`: muss auf "Documentdb" festgelegt werden.
- `databaseName`: Die Datenbank, die das Dokument enthält.
- `collectionName`: Der Websitesammlung, die das Dokument enthält.
- `id`: Die Id des Dokuments abgerufen werden soll. Diese Eigenschaft unterstützt Bindungen ähnlich wie "{QueueTrigger}", welche wird den Zeichenfolgenwert der Warteschlange Nachricht verwenden, wie das Dokument-ID
- `connection`: Diese Zeichenfolge muss eine Anwendungseinstellung Sammlung an den Endpunkt für Ihr Konto DocumentDB. Wenn Sie Ihr Konto auf der Registerkarte integrieren auswählen, wird eine neue Einstellung für die App mit einem Namen für Sie erstellt werden, die das folgende Formular, YourAccount_DOCUMENTDB akzeptiert. Wenn Sie die App-Einstellung manuell zu erstellen müssen, muss die tatsächliche Verbindungszeichenfolge haben folgende Syntax, AccountEndpoint =<Endpoint for your account>; AccountKey =<Your primary access key>;.
- ' Richtung: muss auf *"in"*festgelegt werden.

Beispiel für *function.json*:
 
    {
      "bindings": [
        {
          "name": "document",
          "type": "documentdb",
          "databaseName": "MyDatabase",
          "collectionName": "MyCollection",
          "id" : "{queueTrigger}",
          "connection": "MyAccount_DOCUMENTDB",     
          "direction": "in"
        }
      ],
      "disabled": false
    }

#### <a name="azure-documentdb-input-code-example-for-a-c-queue-trigger"></a>Azure DocumentDB Eingabewerte Beispiels für ein C#-Warteschlange trigger
 
Mithilfe der oben genannten Beispiel function.json, wird die DocumentDB Eingabewerte Bindung das Dokument mit der Id, die Zeichenfolge der Warteschlange entspricht abrufen und an den Parameter 'Dokument' übergeben. Wenn das Dokument nicht gefunden wird, wird der Parameter 'Dokument' null sein. Das Dokument wird dann mit dem neuen Textwert aktualisiert, wenn die Funktion beendet wird.
 
    public static void Run(string myQueueItem, dynamic document)
    {   
        document.text = "This has changed.";
    }

#### <a name="azure-documentdb-input-code-example-for-an-f-queue-trigger"></a>Azure DocumentDB Eingabewerte Beispiels für eine F-Warteschlange auslösen

Verwenden die oben genannten Beispiel function.json, die Eingabewerte DocumentDB Bindung das Dokument mit der Id, die Zeichenfolge der Warteschlange entspricht abruft und an den Parameter 'Dokument' übergeben. Wenn das Dokument nicht gefunden wird, wird der Parameter 'Dokument' null sein. Das Dokument wird dann mit dem neuen Textwert aktualisiert, wenn die Funktion beendet wird.

    open FSharp.Interop.Dynamic
    let Run(myQueueItem: string, document: obj) =
        document?text <- "This has changed."

Benötigen Sie eine `project.json` Datei, die NuGet verwendet, um anzugeben, die `FSharp.Interop.Dynamic` und `Dynamitey` Paketen als Paket Abhängigkeiten, so wie hier:

    {
      "frameworks": {
        "net46": {
          "dependencies": {
            "Dynamitey": "1.0.2",
            "FSharp.Interop.Dynamic": "3.0.0"
          }
        }
      }
    }

Dies verwenden NuGet Ihrer Abhängigkeiten abgerufen werden sollen, und diese in Ihrem Skript verwiesen wird.

#### <a name="azure-documentdb-input-code-example-for-a-nodejs-queue-trigger"></a>Azure DocumentDB Eingabewerte Beispiels für ein Node.js Warteschlange trigger
 
Mit den oben genannten Beispiel function.json, die DocumentDB Eingabewerte Bindung wird das Dokument mit der Id, die Zeichenfolge der Warteschlange entspricht abrufen und übergeben Sie diese an die `documentIn` Binden-Eigenschaft. In Node.js-Funktionen sind die aktualisierten Dokumente wieder in der Auflistung nicht gesendet. Sie können jedoch übergeben die Eingabewerte Bindung direkt in einer DocumentDB Ausgabe Bindung benannten `documentOut` zur Unterstützung von Updates. In diesem Codebeispiel aktualisiert die Texteigenschaft des Dokuments Eingabe- und legt diesen als Ausgabedokument.
 
    module.exports = function (context, input) {   
        context.bindings.documentOut = context.bindings.documentIn;
        context.bindings.documentOut.text = "This was updated!";
        context.done();
    };

## <a name="a-iddocdboutputa-azure-documentdb-output-bindings"></a><a id="docdboutput"></a>Azure DocumentDB ausgeben Bindungen

Ihre Funktionen JSON schreiben können Dokumente mit einer DocumentDB Azure-Datenbank anhand der **Azure DocumentDB Dokument** ausgeben Bindung. Weitere Informationen zum Azure DocumentDB überprüfen Sie die [Einführung zu DocumentDB](../documentdb/documentdb-introduction.md) und das [Erste Schritte-Lernprogramm](../documentdb/documentdb-get-started.md)aus.

#### <a name="functionjson-for-documentdb-output-binding"></a>Function.JSON für DocumentDB ausgeben Bindung

Die Datei function.json bietet folgende Eigenschaften:

- `name`: Variablenname Funktion Code für das neue Dokument.
- `type`: muss auf *"Documentdb"*festgelegt werden.
- `databaseName`: Die Datenbank mit der Websitesammlung, in das neue Dokument erstellt werden soll.
- `collectionName`: Der Websitesammlung, in das neue Dokument erstellt werden soll.
- `createIfNotExists`: Dies ist der boolesche Wert um anzugeben, ob die Sammlung erstellt wird, wenn er nicht vorhanden ist. Die Standardeinstellung ist " *false*". Der Grund zur diese neue Websitesammlungen erstellt werden mit reservierte Durchsatz, die Preise Auswirkungen hat. Weitere Informationen hierzu finden Sie auf der [Seite Preise](https://azure.microsoft.com/pricing/details/documentdb/).
- `connection`: Diese Zeichenfolge muss, dass eine **Anwendungseinstellung** an den Endpunkt für Ihr Konto DocumentDB festgelegt. Wenn Sie Ihr Konto auf der Registerkarte **integrieren** auswählen, wird eine neue App-Einstellung für Sie erstellt werden, mit einem Namen, der das folgende Formular aus, `yourAccount_DOCUMENTDB`. Wenn Sie die App-Einstellung manuell zu erstellen müssen, muss die tatsächliche Verbindungszeichenfolge haben folgende Syntax, `AccountEndpoint=<Endpoint for your account>;AccountKey=<Your primary access key>;`. 
- `direction`: muss auf *","*festgelegt werden. 
 
Beispiel für function.json:

    {
      "bindings": [
        {
          "name": "document",
          "type": "documentdb",
          "databaseName": "MyDatabase",
          "collectionName": "MyCollection",
          "createIfNotExists": false,
          "connection": "MyAccount_DOCUMENTDB",
          "direction": "out"
        }
      ],
      "disabled": false
    }


#### <a name="azure-documentdb-output-code-example-for-a-nodejs-queue-trigger"></a>Azure DocumentDB Ausgabe Beispiels für ein Node.js Warteschlange trigger

    module.exports = function (context, input) {
       
        context.bindings.document = {
            text : "I'm running in a Node function! Data: '" + input + "'"
        }   
     
        context.done();
    };

Die Ausgabedokument:

    {
      "text": "I'm running in a Node function! Data: 'example queue data'",
      "id": "01a817fe-f582-4839-b30c-fb32574ff13f"
    }
 

#### <a name="azure-documentdb-output-code-example-for-an-f-queue-trigger"></a>Azure DocumentDB Ausgabe Beispiels für eine F-Warteschlange auslösen

    open FSharp.Interop.Dynamic
    let Run(myQueueItem: string, document: obj) =
        document?text <- (sprintf "I'm running in an F# function! %s" myQueueItem)

#### <a name="azure-documentdb-output-code-example-for-a-c-queue-trigger"></a>Azure DocumentDB Ausgabe Beispiels für ein C#-Warteschlange trigger


    using System;

    public static void Run(string myQueueItem, out object document, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
       
        document = new {
            text = $"I'm running in a C# function! {myQueueItem}"
        };
    }


#### <a name="azure-documentdb-output-code-example-setting-file-name"></a>Azure DocumentDB Ausgabe Code Beispiel Einstellung Dateiname

Wenn Sie den Namen des Dokuments in der Funktion festlegen möchten, legen Sie einfach die `id` Wert.  Wenn beispielsweise JSON-Inhalt für einen Mitarbeiter in der Warteschlange ähnlich wie der folgende verworfen wurde:

    {
      "name" : "John Henry",
      "employeeId" : "123456",
      "address" : "A town nearby"
    }

Sie können den folgenden C#-Code in einer Warteschlange Trigger-Funktion verwenden: 
    
    #r "Newtonsoft.Json"
    
    using System;
    using Newtonsoft.Json;
    using Newtonsoft.Json.Linq;
    
    public static void Run(string myQueueItem, out object employeeDocument, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
        
        dynamic employee = JObject.Parse(myQueueItem);
        
        employeeDocument = new {
            id = employee.name + "-" + employee.employeeId,
            name = employee.name,
            employeeId = employee.employeeId,
            address = employee.address
        };
    }

Oder den entsprechenden F-Code:

    open FSharp.Interop.Dynamic
    open Newtonsoft.Json

    type Employee = {
        id: string
        name: string
        employeeId: string
        address: string
    }

    let Run(myQueueItem: string, employeeDocument: byref<obj>, log: TraceWriter) =
        log.Info(sprintf "F# Queue trigger function processed: %s" myQueueItem)
        let employee = JObject.Parse(myQueueItem)
        employeeDocument <-
            { id = sprintf "%s-%s" employee?name employee?employeeId
              name = employee?name
              employeeId = employee?id
              address = employee?address }

Beispiel für die Ausgabe:

    {
      "id": "John Henry-123456",
      "name": "John Henry",
      "employeeId": "123456",
      "address": "A town nearby"
    }

## <a name="next-steps"></a>Nächste Schritte

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

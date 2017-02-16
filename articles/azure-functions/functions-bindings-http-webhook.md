<properties
    pageTitle="Azure HTTP-Funktionen und Webhook Bindungen | Microsoft Azure"
    description="Verstehen Sie, wie HTTP und Webhook Trigger und Bindungen in Azure-Funktionen verwenden."
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
    ms.date="08/22/2016"
    ms.author="chrande"/>

# <a name="azure-functions-http-and-webhook-bindings"></a>Azure HTTP-Funktionen und Webhook Bindungen

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

In diesem Artikel wird erläutert, wie konfigurieren und Fehlercode HTTP und Webhook Trigger und Bindungen in Azure-Funktionen. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a name="functionjson-for-http-and-webhook-bindings"></a>Function.JSON für HTTP und Webhook Bindungen

Die Datei *function.json* bietet Eigenschaften, die die Anforderung und die Antwort betreffen.

Eigenschaften der HTTP-Anforderung:

- `name`: Variablenname Funktion Code für das Anforderungsobjekt (oder den Text für den Fall Node.js Funktionen der Anforderung).
- `type`: *HttpTrigger*muss festgelegt werden.
- `direction`: Muss *im*festgelegt werden. 
- `webHookType`: Für WebHook-Trigger sind die Werte gültig *Github*, *Pufferzeit*und *GenericJson*. Legen Sie diese Eigenschaft für einen HTTP-Trigger, der eine WebHook nicht zur Verfügung, um eine leere Zeichenfolge zurück. Weitere Informationen zu WebHooks finden Sie unter den folgenden [WebHook Triggers](#webhook-triggers) -Abschnitt.
- `authLevel`: WebHook Trigger gilt nicht. Legen Sie auf "Funktion" der API-Schlüssel für wichtige Anforderung legen die API oder "Administrator" der master API Schlüssel erfordert "Anonym" erfordert. Weitere Informationen finden Sie unter [API Tasten](#apikeys) unterhalb.

Eigenschaften für die HTTP-Antwort:

- `name`: Variablenname Funktion Code für das Antwortobjekt verwendet.
- `type`: *Http*müssen festgelegt werden.
- `direction`: Muss *sich*festgelegt werden. 
 
Beispiel für *function.json*:

```json
{
  "bindings": [
    {
      "webHookType": "",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "authLevel": "function"
    },
    {
      "name": "res",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

## <a name="webhook-triggers"></a>WebHook Trigger

Ein WebHook-Trigger ist ein HTTP auslösen, die weist die folgenden Features für WebHooks entwickelt:

* Für bestimmte WebHook Provider (aktuell GitHub und Pufferzeit werden unterstützt), die Funktionen Laufzeit überprüft der Anbieter Signatur.
* Die Funktionen Laufzeit bietet Node.js-Funktionen der Anforderungstexts anstelle des Anforderungsobjekts. Es gibt keine spezielle Behandlung für C#-Funktionen, da Sie steuern, welche zur Verfügung steht, durch Angabe den Parametertyp. Wenn Sie angeben `HttpRequestMessage` Sie das Anforderungsobjekt erhalten. Wenn Sie einen Typ POCO angeben, versucht die Laufzeit Funktionen, ein JSON-Objekt in den Textkörper der Anforderung zum Auffüllen von Eigenschaften des Objekts zu analysieren.
* Zum Auslösen eines WebHook muss die Anforderung HTTP-Funktion einen API-Schlüssel beinhalten. Für WebHook HTTP - Trigger ist diese Anforderung optional.

Informationen zum Einrichten einer GitHub WebHook finden Sie unter [GitHub Developer – WebHooks erstellen](http://go.microsoft.com/fwlink/?LinkID=761099&clcid=0x409).

## <a name="url-to-trigger-the-function"></a>URL die Funktion ausgelöst.

Um eine Funktion ausgelöst wird, senden Sie eine HTTP-Anforderung zu einer URL, die eine Kombination der app-URL (Funktion) und den Namen der Funktion ist ein:

```
 https://{function app name}.azurewebsites.net/api/{function name} 
```

## <a name="api-keys"></a>API-Schlüssel

Standardmäßig muss ein API Key in einer HTTP-Anforderung zum Auslösen einer Funktion HTTP oder WebHook enthalten. Die Taste enthalten sein kann, in einer Abfrage Zeichenfolgenvariablen mit dem Namen `code`, oder enthalten sein kann eine `x-functions-key` HTTP-Header. Für nicht WebHook Funktionen können Sie angeben, dass ein API Schlüssel nicht, durch Festlegen erforderlich ist der `authLevel` Eigenschaft "Anonym" in der Datei *function.json* .

Sie können in den Ordner *D:\home\data\Functions\secrets* im Dateisystem der Funktion app API Schlüsselwerte suchen.  Der Master und Funktion Schlüssel werden in der Datei *host.json* festgelegt, wie im folgenden Beispiel gezeigt. 

```json
{
  "masterKey": "K6P2VxK6P2VxK6P2VxmuefWzd4ljqeOOZWpgDdHW269P2hb7OSJbDg==",
  "functionKey": "OBmXvc2K6P2VxK6P2VxK6P2VxVvCdB89gChyHbzwTS/YYGWWndAbmA=="
}
```

Die Funktionstaste aus *host.json* kann verwendet werden, um jede Funktion auslösen jedoch eine Funktion deaktivierte wird nicht auslösen. Die Master-Taste können verwendet werden, um jede Funktion auslösen und wird eine Funktion auslösen, auch wenn es deaktiviert ist. Sie können eine Funktion, um die Taste Master durch Festlegen erfordern konfigurieren die `authLevel` Eigenschaft zu "Administrator". 

Wenn eine JSON-Datei mit demselben Namen wie eine Funktion, die *Kennwörter* enthält die `key` Eigenschaft in dieser Datei kann auch verwendet werden, um die Funktion auslösen und Schlüssel funktionieren nur mit der Funktion bezieht. Angenommen, die API-Taste für eine Funktion namens `HttpTrigger` in *HttpTrigger.json* in den Ordner *Kennwörter* angegeben ist. Hier ist ein Beispiel:

```json
{
  "key":"0t04nmo37hmoir2rwk16skyb9xsug32pdo75oce9r4kg9zfrn93wn4cx0sxo4af0kdcz69a4i"
}
```

> [AZURE.NOTE] Wenn Sie einen WebHook Trigger einrichten, freigeben Sie nicht die Taste Master-Shape mit dem WebHook-Anbieter. Verwenden Sie einen Schlüssel, der nur mit der Funktion funktionieren, die die WebHook verarbeitet.  Die Master-Taste können verwendet werden, um jede Funktion, auslösen auch Funktionen deaktiviert.

## <a name="example-c-code-for-an-http-trigger-function"></a>C#-Beispielcode für eine HTTP-Trigger (Funktion) 

Der Beispielcode sucht nach einer `name` Parameter in der Abfragezeichenfolge oder den Hauptteil der HTTP-Anforderung.

```csharp
using System.Net;
using System.Threading.Tasks;

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
{
    log.Info($"C# HTTP trigger function processed a request. RequestUri={req.RequestUri}");

    // parse query parameter
    string name = req.GetQueryNameValuePairs()
        .FirstOrDefault(q => string.Compare(q.Key, "name", true) == 0)
        .Value;

    // Get request body
    dynamic data = await req.Content.ReadAsAsync<object>();

    // Set name to query string or body data
    name = name ?? data?.name;

    return name == null
        ? req.CreateResponse(HttpStatusCode.BadRequest, "Please pass a name on the query string or in the request body")
        : req.CreateResponse(HttpStatusCode.OK, "Hello " + name);
}
```

## <a name="example-f-code-for-an-http-trigger-function"></a>F-Beispielcode für eine HTTP-Trigger (Funktion)

Der Beispielcode sucht nach einer `name` Parameter in der Abfragezeichenfolge oder den Hauptteil der HTTP-Anforderung.

```fsharp
open System.Net
open System.Net.Http
open FSharp.Interop.Dynamic

let Run(req: HttpRequestMessage) =
    async {
        let q =
            req.GetQueryNameValuePairs()
                |> Seq.tryFind (fun kv -> kv.Key = "name")
        match q with
        | Some kv ->
            return req.CreateResponse(HttpStatusCode.OK, "Hello " + kv.Value)
        | None ->
            let! data = Async.AwaitTask(req.Content.ReadAsAsync<obj>())
            try
                return req.CreateResponse(HttpStatusCode.OK, "Hello " + data?name)
            with e ->
                return req.CreateErrorResponse(HttpStatusCode.BadRequest, "Please pass a name on the query string or in the request body")
    } |> Async.StartAsTask
```

Müssen Sie eine `project.json` Datei, die NuGet wird verwendet, um Bezug auf die `FSharp.Interop.Dynamic` und `Dynamitey` Assemblys, wie folgt:

```json
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
```

Dies verwenden NuGet Ihrer Abhängigkeiten abgerufen werden sollen, und sie in Ihrem Skript verwiesen wird.

## <a name="example-nodejs-code-for-an-http-trigger-function"></a>Beispiel für Node.js Code für eine HTTP-Trigger (Funktion) 

Dieses Beispielcode sucht nach einem `name` Parameter in der Abfragezeichenfolge oder den Hauptteil der HTTP-Anforderung.

```javascript
module.exports = function(context, req) {
    context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);

    if (req.query.name || (req.body && req.body.name)) {
        context.res = {
            // status: 200, /* Defaults to 200 */
            body: "Hello " + (req.query.name || req.body.name)
        };
    }
    else {
        context.res = {
            status: 400,
            body: "Please pass a name on the query string or in the request body"
        };
    }
    context.done();
};
```

## <a name="example-c-code-for-a-github-webhook-function"></a>Beispiel für C#-Code für eine GitHub WebHook-Funktion 

Dieses Beispielcode protokolliert GitHub Problem Kommentare.

```csharp
#r "Newtonsoft.Json"

using System;
using System.Net;
using System.Threading.Tasks;
using Newtonsoft.Json;

public static async Task<object> Run(HttpRequestMessage req, TraceWriter log)
{
    string jsonContent = await req.Content.ReadAsStringAsync();
    dynamic data = JsonConvert.DeserializeObject(jsonContent);

    log.Info($"WebHook was triggered! Comment: {data.comment.body}");

    return req.CreateResponse(HttpStatusCode.OK, new {
        body = $"New GitHub comment: {data.comment.body}"
    });
}
```

## <a name="example-f-code-for-a-github-webhook-function"></a>F-Beispielcode für eine GitHub WebHook-Funktion

Dieses Beispielcode protokolliert GitHub Problem Kommentare.

```fsharp
open System.Net
open System.Net.Http
open FSharp.Interop.Dynamic
open Newtonsoft.Json

type Response = {
    body: string
}

let Run(req: HttpRequestMessage, log: TraceWriter) =
    async {
        let! content = req.Content.ReadAsStringAsync() |> Async.AwaitTask
        let data = content |> JsonConvert.DeserializeObject
        log.Info(sprintf "GitHub WebHook triggered! %s" data?comment?body)
        return req.CreateResponse(
            HttpStatusCode.OK,
            { body = sprintf "New GitHub comment: %s" data?comment?body })
    } |> Async.StartAsTask
```

## <a name="example-nodejs-code-for-a-github-webhook-function"></a>Beispiel für Node.js Code für eine GitHub WebHook-Funktion 

Dieses Beispielcode protokolliert GitHub Problem Kommentare.

```javascript
module.exports = function (context, data) {
    context.log('GitHub WebHook triggered!', data.comment.body);
    context.res = { body: 'New GitHub comment: ' + data.comment.body };
    context.done();
};
```

## <a name="next-steps"></a>Nächste Schritte

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)] 

<properties
    pageTitle="Azure Funktionen NodeJS Entwicklerreferenz | Microsoft Azure"
    description="Verstehen Sie, wie Azure-Funktionen, die mit NodeJS entwickeln."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure-Funktionen, Funktionen, Ereignisse zu verarbeiten, Webhooks, dynamische berechnen, ohne Server Architektur"/>

<tags
    ms.service="functions"
    ms.devlang="nodejs"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="05/13/2016"
    ms.author="chrande"/>

# <a name="azure-functions-nodejs-developer-reference"></a>Azure Funktionen NodeJS Entwicklerreferenz

> [AZURE.SELECTOR]
- [C#-Skript](../articles/azure-functions/functions-reference-csharp.md)
- [F-Skript](../articles/azure-functions/functions-reference-fsharp.md)
- [Node.js](../articles/azure-functions/functions-reference-node.md)

Die Knoten/JavaScript-Benutzeroberfläche für Azure Funktionen erleichtert so exportieren Sie eine Funktion der übergeben wird eine `context` Objekt, für die Kommunikation mit der Laufzeit sowie Daten über Bindungen senden und empfangen.

In diesem Artikel wird vorausgesetzt, dass Sie bereits [Azure Funktionen Entwicklerreferenz](functions-reference.md)gelesen haben.

## <a name="exporting-a-function"></a>Exportieren einer Funktion

Alle JavaScript-Funktionen müssen ein einzelnes exportieren `function` über `module.exports` für die Laufzeit die Funktion suchen, und führen Sie es. Diese Funktion muss immer enthalten eine `context` Objekt.

```javascript
// You must include a context, but other arguments are optional
module.exports = function(context) {
    // Additional inputs can be accessed by the arguments property
    if(arguments.length === 4) {
        context.log('This function has 4 inputs');
    }
};
// or you can include additional inputs in your arguments
module.exports = function(context, myTrigger, myInput, myOtherInput) {
    // function logic goes here :)
};
```

Bindungen von `direction === "in"` als Funktionsargumente, d. h., Sie können weitergegeben werden [`arguments`](https://msdn.microsoft.com/library/87dw3w1k.aspx) dynamisch neue Eingaben verarbeitet (z. B. mithilfe von `arguments.length` durchlaufen Sie alle Ihre Eingaben). Diese Funktion ist sehr praktisch, wenn Sie nur einen Trigger mit kein zusätzlichen Eingaben, haben, wie Sie Ihre Triggerdaten vorhersehbar ohne Verweis auf zugreifen können Ihre `context` Objekt.

Die Argumente werden immer entlang übergeben an die Funktion in der Reihenfolge, dass sie in *function.json*, auftreten, auch wenn Sie diese nicht in Ihrer Exporte-Anweisung angeben. Angenommen, Sie haben `function(context, a, b)` ändern sie die `function(context, a)`, können Sie den Wert von weiterhin aufrufen `b` Funktion Code anhand `arguments[3]`.

Alle Bindungen, unabhängig von der Richtung, werden ebenfalls entlang übergeben, klicken Sie auf die `context` Objekt (siehe unten). 

## <a name="context-object"></a>Kontextobjekt

Die Laufzeit verwendet eine `context` Objekt Daten an und von der Funktion übergeben, und lassen Sie Sie mit der Runtime kommunizieren.

Das Kontextobjekt ist immer erster Parameter zu einer Funktion und sollten immer enthalten sein, da es Methoden, z. B. enthält `context.done` und `context.log` die sind erforderlich, um die Laufzeit ordnungsgemäß zu verwenden. Sie können das Objekt benennen beliebig (d. h. `ctx` oder `c`).

```javascript
// You must include a context, but other arguments are optional
module.exports = function(context) {
    // function logic goes here :)
};
```

## <a name="contextbindings"></a>Context.Bindings

Die `context.bindings` Objekt sammelt alle Eingabe- und Daten. Die Daten werden hinzugefügt, auf die `context.bindings` Objekt über die `name` -Eigenschaft der Bindung. Beispielsweise die folgende Bindungsdefinition in *function.json*angegeben, Sie können auf den Inhalt zugreifen der Warteschlange über `context.bindings.myInput`. 

```json
    {
        "type":"queue",
        "direction":"in",
        "name":"myInput"
        ...
    }
```

```javascript
// myInput contains the input data which may have properties such as "name"
var author = context.bindings.myInput.name;
// Similarly, you can set your output data
context.bindings.myOutput = { 
        some_text: 'hello world', 
        a_number: 1 };
```

## `context.done([err],[propertyBag])`

Die `context.done` Funktion weist die Runtime, dass Sie fertig sind ausgeführt. Dies ist wichtig, anrufen, wenn Sie mit der Funktion fertig sind. Wenn Sie nicht, weiß die Laufzeit noch nie, dass die Funktion abgeschlossen. 

Die `context.done` Funktion können Sie übergeben wieder einen benutzerdefinierter Fehler an die Laufzeit sowie eine Eigenschaftensammlung von Eigenschaften die Eigenschaften überschrieben werden, klicken Sie auf die `context.bindings` Objekt.

```javascript
// Even though we set myOutput to have:
//  -> text: hello world, number: 123
context.bindings.myOutput = { text: 'hello world', number: 123 };
// If we pass an object to the done function...
context.done(null, { myOutput: { text: 'hello there, world', noNumber: true }});
// the done method will overwrite the myOutput binding to be: 
//  -> text: hello there, world, noNumber: true
```

## <a name="contextlogmessage"></a>Context.log(Message)

Die `context.log` Methode können Sie Log-Anweisungen ausgeben, die miteinander in Beziehung für die Protokollierung gesetzt werden. Wenn Sie verwenden `console.log`, Ihre Nachrichten zeigt nur für Prozess-Protokollierung, die nicht als sinnvoll ist.

```javascript
/* You can use context.log to log output specific to this 
function. You can access your bindings via context.bindings */
context.log({hello: 'world'}); // logs: { 'hello': 'world' } 
```

Die `context.log` Methode unterstützt das gleichen Parameterformat, das die Knoten [util.format Methode](https://nodejs.org/api/util.html#util_util_format_format) unterstützt. Ja, beispielsweise code wie folgt:

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=' + req.originalUrl);
context.log('Request Headers = ' + JSON.stringify(req.headers));
```

kann wie folgt geschrieben werden:

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);
context.log('Request Headers = ', JSON.stringify(req.headers));
```

## <a name="http-triggers-contextreq-and-contextres"></a>HTTP-Trigger: context.req und context.res

Im Fall HTTP-Triggern weil es ein gemeinsames Muster mit ist `req` und `res` für die HTTP-Anforderung und Antwort Objekte wir uns entschieden, Empfänger auf das Objekt und Kontextmenü, nicht erzwingen, dass Sie der vollständige Zugriff zu erleichtern `context.bindings.name` Muster.

```javascript
// You can access your http request off of the context ...
if(context.req.body.emoji === ':pizza:') context.log('Yay!');
// and also set your http response
context.res = { status: 202, body: 'You successfully ordered more coffee!' };   
```

## <a name="node-version--package-management"></a>Knoten Version und Verwalten von Paketen

Die Version Knoten ist derzeit gesperrt am `5.9.1`. Wir haben Untersuchung läuft Hinzufügen von Unterstützung für mehrere Versionen und dort konfigurierbare zu machen.

Sie können Pakete durch Hochladen einer Datei *package.json* in der Funktion Ordner im Dateisystem des app Funktion in der Funktion einbeziehen. Datei hochladen Sie Anweisungen, finden Sie im Abschnitt **zum Aktualisieren der app-Dateien (Funktion)** des [Azure Funktionen Entwicklertools Bezug Thema](functions-reference.md#fileupdate). 

Sie können auch `npm install` in der Funktion-app SCM (Kudu) Befehl Linie Benutzeroberflächen:

1. Navigieren Sie zu: `https://<function_app_name>.scm.azurewebsites.net`.

2. Klicken Sie auf **Debuggen Console > CMD**.

3. Navigieren Sie zu `D:\home\site\wwwroot\<function_name>`.

4. Führen Sie `npm install`.

Nachdem die benötigten Pakete installiert sind, Sie importieren sie zu Ihrer Funktion in üblichen weisen (d. h. über `require('packagename')`)

```javascript
// Import the underscore.js library
var _ = require('underscore');
var version = process.version; // version === 'v5.9.1'

module.exports = function(context) {
    // Using our imported underscore.js library
    var matched_names = _
        .where(context.bindings.myInput.names, {first: 'Carla'});
```

## <a name="environment-variables"></a>Umgebungsvariablen

Um eine Umgebungsvariable oder einer app Wert festlegen erhalten möchten, verwenden Sie `process.env`, wie im folgenden Beispiel gezeigt:

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();
    
    context.log('Node.js timer trigger function ran!', timeStamp);   
    context.log(GetEnvironmentVariable("AzureWebJobsStorage"));
    context.log(GetEnvironmentVariable("WEBSITE_SITE_NAME"));
    
    context.done();
};

function GetEnvironmentVariable(name)
{
    return name + ": " + process.env[name];
}
```

## <a name="typescriptcoffeescript-support"></a>Mit Schreibmaschine/CoffeeScript support

Nicht vorhanden ist, aber direkte Unterstützung für die automatische-Kompilierung mit Schreibmaschine/CoffeeScript über die Laufzeit, damit, die alle würde zum Zeitpunkt der Bereitstellung außerhalb der Runtime behandelt werden müssen. 

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter den folgenden Ressourcen:

* [Azure Funktionen Entwicklerreferenz](functions-reference.md)
* [Azure Funktionen C#-Entwicklerreferenz](functions-reference-csharp.md)
* [Azure Funktionen F Nr. Entwicklerreferenz](functions-reference-fsharp.md)
* [Azure Funktionen Trigger und Bindungen](functions-triggers-bindings.md)

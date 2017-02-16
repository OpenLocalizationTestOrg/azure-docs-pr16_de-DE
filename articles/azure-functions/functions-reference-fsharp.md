<properties
    pageTitle="Azure Funktionen F Nr. Entwicklerreferenz | Microsoft Azure"
    description="Verstehen Sie, wie Azure-Funktionen, die mithilfe von F Nr. entwickeln."
    services="functions"
    documentationCenter="fsharp"
    authors="sylvanc"
    manager="jbronsk"
    editor=""
    tags=""
    keywords="Azure-Funktionen, Funktionen, Verarbeitung, Webhooks, dynamische berechnen, ohne Server Architektur, F von Ereignissen#"/>

<tags
    ms.service="functions"
    ms.devlang="fsharp"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="09/09/2016"
    ms.author="syclebsc"/>

# <a name="azure-functions-f-developer-reference"></a>Azure Funktionen F Nr. Entwicklerreferenz

> [AZURE.SELECTOR]
- [C#-Skript](../articles/azure-functions/functions-reference-csharp.md)
- [F-Skript](../articles/azure-functions/functions-reference-fsharp.md)
- [Node.js](../articles/azure-functions/functions-reference-node.md)

F-Funktionen Azure ist eine Lösung für die Ausführung von Code oder "Funktionen" für kleinere einfach in der Cloud. Daten werden in der F-Funktion über Funktionsargumente fließt. Argumentnamen in angegeben sind `function.json`, und es sind vordefinierte Namen für den Zugriff auf Elemente wie die Funktion Protokollierung und Kündigung Token.

In diesem Artikel wird vorausgesetzt, dass Sie bereits [Azure Funktionen Entwicklerreferenz](functions-reference.md)gelesen haben.

## <a name="how-fsx-works"></a>Funktionsweise von ".fsx"

Eine `.fsx` Datei ist ein F-Skript. Es kann als eine F-Projekt vorstellen, die in einer einzigen Datei enthalten ist. Die Datei enthält sowohl den Code für Ihr Programm (in diesem Fall der Azure-Funktion) und Richtlinien für die Verwaltung von Abhängigkeiten.

Bei Verwendung eines `.fsx` für eine Funktion Azure häufig erforderlichen Assemblys automatisch für Sie, so dass Sie auf die Funktion statt "Textbaustein" Code konzentrieren enthalten sind.

## <a name="binding-to-arguments"></a>Binden an Argumente auf:

Jede Bindung unterstützt einige Satz von Argumenten, wie ausführlich [Azure Funktionen Trigger und Bindungen Entwicklerreferenz](functions-triggers-bindings.md). Eine Bindung, die ein Blob-Trigger unterstützt Argument beträgt beispielsweise eine POCO, der mit einer F-Datensatz ausgedrückt werden kann. Beispiel:

```fsharp
type Item = { Id: string }

let Run(blob: string, output: byref<Item>) =
    let item = { Id = "Some ID" }
    output <- item
```

Der F-Funktion Azure dauert ein oder mehrere Argumente. Wenn wir von Azure Funktionen Argumente sprechen, schlagen Sie wir in _übergebenen_ Argumente und _Ausgabe_ Argumente ein. Eingabe Argument ist genau wie klingt: Eingabemethoden an der F-Azure-Funktion. Ein Argument _Ausgabe_ sind veränderlichen Daten oder einer `byref<>` Argument, das als eine Möglichkeit, übergeben fungiert Daten zurück _,_ der der Funktion.

Im Beispiel oben `blob` ist Argument Eingabe- und `output` Argument Ausgabe ist. Benachrichtigung, die wir verwendet `byref<>` für `output` (es ist nicht erforderlich, zum Hinzufügen der `[<Out>]` Anmerkung). Verwenden einer `byref<>` Typ ermöglicht die Funktion zum Ändern der Eintrag oder das Objekt, das Argument bezieht sich auf.

Wenn ein Eintrag F Nr. als eine Eingabe Typ verwendet wird, muss die Eintrag Definition markiert mit `[<CLIMutable>]` akzeptieren, um das Framework Azure-Funktionen in die Felder ordnungsgemäß festgelegt, bevor Sie den Eintrag für Ihre Funktion. Erweiterte Einstellungen `[<CLIMutable>]` Set-Methoden für den Datensatzeigenschaften generiert. Beispiel:

```fsharp
[<CLIMutable>]
type TestObject =
    { SenderName : string
      Greeting : string }

let Run(req: TestObject, log: TraceWriter) =
    { req with Greeting = sprintf "Hello, %s" req.SenderName }
```

Eine F-Klasse kann auch für beide ein- und Auschecken Argumente verwendet werden. Bei einer Klasse benötigen Eigenschaften normalerweise Get und Set. Beispiel:

```fsharp
type Item() =
    member val Id = "" with get,set
    member val Text = "" with get,set

let Run(input: string, item: byref<Item>) =
    let result = Item(Id = input, Text = "Hello from F#!")
    item <- result
```

## <a name="logging"></a>Protokollierung

Um Ausgabe Ihre [streaming Protokolle](../app-service-web/web-sites-streaming-logs-and-console.md) in F-Anmeldung, dauert die Funktion ein Argument des Typs `TraceWriter`. Konsistenz, empfehlen wir für dieses Argument hat den Namen `log`. Beispiel:

```fsharp
let Run(blob: string, output: byref<string>, log: TraceWriter) =
    log.Verbose(sprintf "F# Azure Function processed a blob: %s" blob)
    output <- input
```

## <a name="async"></a>Asynchrone

Die `async` Workflow verwendet werden kann, aber das Ergebnis zurückgeben muss eine `Task`. Dies kann mit `Async.StartAsTask`, beispielsweise:

```fsharp
let Run(req: HttpRequestMessage) =
    async {
        return new HttpResponseMessage(HttpStatusCode.OK)
    } |> Async.StartAsTask
```

## <a name="cancellation-token"></a>Absage Token

Wenn Ihre Funktion war(en) ordnungsgemäß behandelt werden muss, können Sie probieren Sie es ein [`CancellationToken`](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) Argument. Dies kann mit kombiniert werden `async`, beispielsweise:

```fsharp
let Run(req: HttpRequestMessage, token: CancellationToken)
    let f = async {
        do! Async.Sleep(10)
        return new HttpResponseMessage(HttpStatusCode.OK)
    }
    Async.StartAsTask(f, token)
```

## <a name="importing-namespaces"></a>Importieren von namespaces

Namespaces können wie gewohnt geöffnet werden:

```fsharp
open System.Net
open System.Threading.Tasks

let Run(req: HttpRequestMessage, log: TraceWriter) =
    ...
```

Die folgenden Namespaces werden automatisch geöffnet werden:

* `System`
* `System.Collections.Generic`
* `System.IO`
* `System.Linq`
* `System.Net.Http`
* `System.Threading.Tasks`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`.

## <a name="referencing-external-assemblies"></a>Verweisen auf externe Assemblys

Auf ähnliche Weise Frameworkassembly Verweise hinzugefügt werden, mit der `#r "AssemblyName"` Richtlinie.

```fsharp
#r "System.Web.Http"

open System.Net
open System.Net.Http
open System.Threading.Tasks

let Run(req: HttpRequestMessage, log: TraceWriter) =
    ...
```

Die folgenden Assemblys werden automatisch von den Azure-Funktionen hostumgebung hinzugefügt:

* `mscorlib`,
* `System`
* `System.Core`
* `System.Xml`
* `System.Net.Http`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`
* `Microsoft.Azure.WebJobs.Extensions`
* `System.Web.Http`
* `System.Net.Http.Formatting`.

Darüber hinaus stehen die folgenden Assemblys spezielle Groß-/Kleinschreibung beachtet und möglicherweise Simplename optimiert (z. B. `#r "AssemblyName"`):

* `Newtonsoft.Json`
* `Microsoft.WindowsAzure.Storage`
* `Microsoft.ServiceBus`
* `Microsoft.AspNet.WebHooks.Receivers`
* `Microsoft.AspNEt.WebHooks.Common`.

Wenn Sie auf private Assemblys verweisen müssen, können Sie die Datei in Hochladen einer `bin` Ordner relativ zu Ihrer (Funktion) und den Bezug aus, es mithilfe der Datei einen Namen (z. B.  `#r "MyAssembly.dll"`). Informationen zum Hochladen von Dateien in den Ordner (Funktion) finden Sie unter den folgenden Abschnitt auf Paket Management.

## <a name="editor-prelude"></a>Einführung-Editor

Ein Editor, der F-Compiler Services unterstützt wird nicht beachten der Namespaces und Assemblys, die automatisch Azure Funktionen enthält. Als solche kann es nützlich sein explizit Namespaces öffnen und eine Einführung zu enthalten, die den Editor die Assemblys suchen, die Sie verwenden können. Beispiel:

```fsharp
#if !COMPILED
#I "../../bin/Binaries/WebJobs.Script.Host"
#r "Microsoft.Azure.WebJobs.Host.dll"
#endif

open Sytem
open Microsoft.Azure.WebJobs.Host

let Run(blob: string, output: byref<string>, log: TraceWriter) =
    ...
```

Wenn Azure Funktionen den Code ausgeführt wird, verarbeitet die Quelle mit `COMPILED` definiert sind, können Sie die Einführung Editor wird ignoriert.

## <a name="package-management"></a>Verwalten von Paketen

Um NuGet-Pakete in eine F-Funktion verwenden, Hinzufügen einer `project.json` legen Sie die der Funktion Ordner im Dateisystem des app Funktion. Hier ein Beispiel `project.json` Datei, die einen NuGet-Paket Verweis auf hinzufügt `Microsoft.ProjectOxford.Face` Version 1.1.0:

```json
{
  "frameworks": {
    "net46":{
      "dependencies": {
        "Microsoft.ProjectOxford.Face": "1.1.0"
      }
    }
   }
}
```

Nur die .NET Framework 4.6 wird unterstützt, also sicherstellen, dass Ihre `project.json` Datei gibt `net46` wie hier dargestellt.

Wenn Sie Hochladen einer `project.json` Datei, die Laufzeit ruft die Pakete ab und fügt automatisch Verweise auf die Assemblys Paket hinzu. Sie müssen nicht hinzufügen `#r "AssemblyName"` Richtlinien. Fügen Sie einfach die erforderlichen `open` Anweisungen, um Ihre `.fsx` Datei.

Möglicherweise möchten Sie Verweise Assemblys automatisch in Ihrer Einführung Editor, zur Verbesserung des Editors Interaktion mit F Nr. Kompilieren Diensten zu setzen.

### <a name="how-to-add-a-projectjson-file-to-your-azure-function"></a>Zum Hinzufügen einer `project.json` Datei zu Ihrer Azure-Funktion

1. Beginnen, indem Sie die Funktion app sicherstellen wird ausgeführt, wozu können Sie Ihre Funktion Azure-Portal zu öffnen. Dies ermöglicht auch Zugriff auf das streaming von Protokollen in dem Paket Installation Ausgabe angezeigt werden.

2. Hochladen einer `project.json` ablegen, verwenden Sie eine der Methoden [zum Aktualisieren der Funktion app-Dateien](functions-reference.md#fileupdate)beschrieben. Wenn Sie [Kontinuierliche Bereitstellung für Azure-Funktionen](functions-continuous-deployment.md)verwenden, können Sie Hinzufügen eines `project.json` Datei in Ihrer staging Zweig akzeptieren, um mit es experimentieren, bevor Sie Ihre Bereitstellung Verzweigung hinzu.

3. Nach der `project.json` Datei hinzugefügt wird, ähnlich wie im folgenden Beispiel wird in der Funktion Ausgabe der Log streaming werden angezeigt:

```
2016-04-04T19:02:48.745 Restoring packages.
2016-04-04T19:02:48.745 Starting NuGet restore
2016-04-04T19:02:50.183 MSBuild auto-detection: using msbuild version '14.0' from 'D:\Program Files (x86)\MSBuild\14.0\bin'.
2016-04-04T19:02:50.261 Feeds used:
2016-04-04T19:02:50.261 C:\DWASFiles\Sites\facavalfunctest\LocalAppData\NuGet\Cache
2016-04-04T19:02:50.261 https://api.nuget.org/v3/index.json
2016-04-04T19:02:50.261
2016-04-04T19:02:50.511 Restoring packages for D:\home\site\wwwroot\HttpTriggerCSharp1\Project.json...
2016-04-04T19:02:52.800 Installing Newtonsoft.Json 6.0.8.
2016-04-04T19:02:52.800 Installing Microsoft.ProjectOxford.Face 1.1.0.
2016-04-04T19:02:57.095 All packages are compatible with .NETFramework,Version=v4.6.
2016-04-04T19:02:57.189
2016-04-04T19:02:57.189
2016-04-04T19:02:57.455 Packages restored.
```

## <a name="environment-variables"></a>Umgebungsvariablen

Um eine Umgebungsvariable oder einer app Wert festlegen erhalten möchten, verwenden Sie `System.Environment.GetEnvironmentVariable`, beispielsweise:

```fsharp
open System.Environment

let Run(timer: TimerInfo, log: TraceWriter) =
    log.Info("Storage = " + GetEnvironmentVariable("AzureWebJobsStorage"))
    log.Info("Site = " + GetEnvironmentVariable("WEBSITE_SITE_NAME"))
```

## <a name="reusing-fsx-code"></a>Wiederverwenden von Code ".fsx"

Von anderen Code können `.fsx` Dateien mithilfe eines `#load` Richtlinie. Beispiel:

`run.fsx`

```fsharp
#load "logger.fsx"

let Run(timer: TimerInfo, log: TraceWriter) =
    mylog log (sprintf "Timer: %s" DateTime.Now.ToString())
```

`logger.fsx`

```fsharp
let mylog(log: TraceWriter, text: string) =
    log.Verbose(text);
```

Pfade bereitstellt, um die `#load` relativ zum Speicherort der Richtlinie sind Ihre `.fsx` Datei.

* `#load "logger.fsx"`Laden eine Datei im Ordner "Funktion".

* `#load "package\logger.fsx"`Laden Sie eine Datei im die `package` Ordner in den Ordner (Funktion).

* `#load "..\shared\mylogger.fsx"`Laden Sie eine Datei im die `shared` Ordner auf der gleichen Ebene wie der Funktion Ordner d. h., direkt unter `wwwroot`.

Die `#load` Richtlinie funktioniert nur mit `.fsx` Dateien (F-Skript), und nicht mit `.fs` Dateien.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter den folgenden Ressourcen:

* [F-Leitfaden](https://docs.microsoft.com/en-us/dotnet/articles/fsharp/index)
* [Azure Funktionen Entwicklerreferenz](functions-reference.md)
* [Azure Funktionen C#-Entwicklerreferenz](functions-reference-csharp.md)
* [Azure Funktionen NodeJS Entwicklerreferenz](functions-reference-node.md)
* [Azure Funktionen Trigger und Bindungen](functions-triggers-bindings.md)
* [Azure Testen der Funktionen](functions-test-a-function.md)
* [Skalierung Azure-Funktionen](functions-scale.md)

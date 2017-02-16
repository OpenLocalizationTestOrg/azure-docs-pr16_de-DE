<properties
    pageTitle="Azure Funktionen Entwicklerreferenz | Microsoft Azure"
    description="Verstehen Sie, wie Azure-Funktionen, die mit c# entwickeln."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure-Funktionen, Funktionen, Ereignisse zu verarbeiten, Webhooks, dynamische berechnen, ohne Server Architektur"/>

<tags
    ms.service="functions"
    ms.devlang="dotnet"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="05/13/2016"
    ms.author="chrande"/>

# <a name="azure-functions-c-developer-reference"></a>Azure Funktionen C#-Entwicklerreferenz

> [AZURE.SELECTOR]
- [C#-Skript](../articles/azure-functions/functions-reference-csharp.md)
- [F-Skript](../articles/azure-functions/functions-reference-fsharp.md)
- [Node.js](../articles/azure-functions/functions-reference-node.md)
 
Die C#-Benutzeroberfläche für Funktionen Azure basiert auf der Azure WebJobs SDK. Daten werden in Ihrem C#-Funktion über Methodenargumente fließt. Argumentnamen in angegeben sind `function.json`, und es sind vordefinierte Namen für den Zugriff auf Elemente wie die Funktion Protokollierung und Kündigung Token.

In diesem Artikel wird vorausgesetzt, dass Sie bereits [Azure Funktionen Entwicklerreferenz](functions-reference.md)gelesen haben.

## <a name="how-csx-works"></a>Funktionsweise von .csx

Die `.csx` Format weniger "Textbaustein" schreiben und Schreiben Sie einfach eine C#-Funktion konzentrieren ermöglicht. Für Azure-Funktionen, Sie nur alle Verweise auf Assemblys und Namespaces enthalten müssen Sie nach oben, wie gewohnt oben, und Sie können nur definieren, statt umbrechen alles in einem Namespace und Class, Ihre `Run` Methode. Wenn Sie alle Klassen, z. B. zählen müssen um POCO Objekte zu definieren, können Sie eine Klasse in derselben Datei einbeziehen.

## <a name="binding-to-arguments"></a>Binden an Argumente auf:

Die verschiedenen Bindungen an eine Funktion c# über gebunden sind die `name` Eigenschaft in der Konfiguration *function.json* . Jede Bindung weist eine eigene unterstützten Datentypen, die pro Bindung beschrieben ist; ein Blob-Trigger kann beispielsweise eine Zeichenfolge, die eine POCO oder mehrere andere Feldtypen unterstützen. Sie können den Typ der am besten Ihren Bedürfnissen entspricht. 

```csharp
public static void Run(string myBlob, out MyClass myQueueItem)
{
    log.Verbose($"C# Blob trigger function processed: {myBlob}");
    myQueueItem = new MyClass() { Id = "myid" };
}

public class MyClass
{
    public string Id { get; set; }
}
```

## <a name="logging"></a>Protokollierung

Um Ihre streaming Protokolle in c# Ausgabe angemeldet werden, Sie können einbeziehen eines `TraceWriter` Argument eingegeben. Es empfiehlt sich, dass Sie die nennen `log`. Es wird empfohlen, Sie vermeiden `Console.Write` in Azure-Funktionen.

```csharp
public static void Run(string myBlob, TraceWriter log)
{
    log.Info($"C# Blob trigger function processed: {myBlob}");
}
```

## <a name="async"></a>Asynchrone

Verwenden, um eine Funktion asynchrone machen die `async` Schlüsselwort und die Eingabe eines `Task` Objekt.

```csharp
public async static Task ProcessQueueMessageAsync(
        string blobName, 
        Stream blobInput,
        Stream blobOutput)
    {
        await blobInput.CopyToAsync(blobOutput, 4096, token);
    }
```

## <a name="cancellation-token"></a>Absage Token

In bestimmten Fällen müssen Sie Vorgänge möglicherweise die wird beendet und Kleinschreibung beachtet werden. Während es immer empfiehlt sich, Code zu schreiben, die Änderung des kritischen Weges helfen können, in Fällen, wo soll war(en) ordnungsgemäß verarbeitet anfordert, definieren Sie eine [`CancellationToken`](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) Argument eingegeben.  A `CancellationToken` bereitgestellt werden, wenn ein Host war(en) ausgelöst wird. 

```csharp
public async static Task ProcessQueueMessageAsyncCancellationToken(
        string blobName, 
        Stream blobInput,
        Stream blobOutput,
        CancellationToken token)
    {
        await blobInput.CopyToAsync(blobOutput, 4096, token);
    }
```

## <a name="importing-namespaces"></a>Importieren von namespaces

Wenn Sie Namespaces importieren müssen, können Sie also wie gewöhnlich, mit Ausführen der `using` Klausel.

```csharp
using System.Net;
using System.Threading.Tasks;

public static Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
```

Die folgenden Namespaces werden automatisch importiert und sind daher optional:

* `System`
* `System.Collections.Generic`
* `System.IO`
* `System.Linq`
* `System.Net.Http`
* `System.Threading.Tasks`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`.

## <a name="referencing-external-assemblies"></a>Verweisen auf externe Assemblys

Fügen Sie Verweise für Framework-Assemblys mithilfe der `#r "AssemblyName"` Richtlinie.

```csharp
#r "System.Web.Http"

using System.Net;
using System.Net.Http;
using System.Threading.Tasks;

public static Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
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
* `Microsoft.AspNEt.WebHooks.Common`
* `Microsoft.Azure.NotificationHubs`

Wenn Sie auf private Assemblys verweisen müssen, können Sie die Datei in Hochladen einer `bin` Ordner relativ zu Ihrer (Funktion) und den Bezug aus, es mithilfe der Datei einen Namen (z. B.  `#r "MyAssembly.dll"`). Informationen zum Hochladen von Dateien in den Ordner (Funktion) finden Sie unter den folgenden Abschnitt auf Paket Management.

## <a name="package-management"></a>Verwalten von Paketen

Um NuGet-Pakete in einer C#-Funktion verwenden, *project.json* Datei zum Hochladen der der Funktion Ordner im Dateisystem des app Funktion. Hier ist eine Beispiel *project.json* -Datei, die ein Verweis auf Microsoft.ProjectOxford.Face Version 1.1.0 hinzugefügt:

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

Nur die .NET Framework 4.6 wird unterstützt, daher sollten Sie sicherstellen, dass die Datei *project.json* gibt an, `net46` wie hier dargestellt.

Wenn Sie eine *project.json* Datei hochladen, wird die Laufzeit ruft die Pakete ab und fügt automatisch Verweise auf die Assemblys Paket hinzu. Sie müssen nicht hinzufügen `#r "AssemblyName"` Richtlinien. Fügen Sie einfach die erforderlichen `using` Anweisungen zur Datei *run.csx* in den NuGet-Paketen definierten Typen verwenden.


### <a name="how-to-upload-a-projectjson-file"></a>Zum Hochladen einer Datei project.json

1. Beginnen, indem Sie die Funktion app sicherstellen wird ausgeführt, wozu können Sie Ihre Funktion Azure-Portal zu öffnen. 

    Dies ermöglicht auch Zugriff auf das streaming von Protokollen in dem Paket Installation Ausgabe angezeigt werden. 

2. Verwenden Sie eine der im Abschnitt **zum Aktualisieren der app-Dateien (Funktion)** des [Azure Funktionen Entwicklertools Bezug Thema](functions-reference.md#fileupdate)beschriebenen Methoden zum Hochladen einer Datei project.json. 

3. Nachdem die Datei *project.json* hochgeladen wurde, sehen Sie die Ausgabe wie im folgenden Beispiel wird in der Funktion Log streaming ist:

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

Um eine Umgebungsvariable oder einer app Wert festlegen erhalten möchten, verwenden Sie `System.Environment.GetEnvironmentVariable`, wie im folgenden Beispiel gezeigt:

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
    log.Info(GetEnvironmentVariable("AzureWebJobsStorage"));
    log.Info(GetEnvironmentVariable("WEBSITE_SITE_NAME"));
}

public static string GetEnvironmentVariable(string name)
{
    return name + ": " + 
        System.Environment.GetEnvironmentVariable(name, EnvironmentVariableTarget.Process);
}
```

## <a name="reusing-csx-code"></a>Wiederverwenden von .csx code

Sie können Klassen und in andere *.csx* -Dateien in der Datei *run.csx* definierten Methoden verwenden. Verwenden Sie hierzu `#load` Richtlinien in der Datei *run.csx* wie im folgenden Beispiel dargestellt.

Beispiel für *run.csx*:

```csharp
#load "mylogger.csx"

public static void Run(TimerInfo myTimer, TraceWriter log)
{
    log.Verbose($"Log by run.csx: {DateTime.Now}"); 
    MyLogger(log, $"Log by MyLogger: {DateTime.Now}");
}
```

Beispiel für *mylogger.csx*:

```csharp
public static void MyLogger(TraceWriter log, string logtext)
{
    log.Verbose(logtext); 
}
```

Einen relativen Pfad mit können die `#load` Richtlinie:

* `#load "mylogger.csx"`Laden eine Datei im Ordner "Funktion".

* `#load "loadedfiles\mylogger.csx"`Laden eine Datei in einem Ordner in den Ordner (Funktion).

* `#load "..\shared\mylogger.csx"`Laden eine Datei in einem Ordner auf der gleichen Ebene wie der Funktion Ordner d. h., direkt unter *Wwwroot*.
 
Die `#load` Richtlinie funktioniert nur mit Dateien *.csx* (C#-Skript) und nicht mit *cs* -Dateien. 

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter den folgenden Ressourcen:

* [Azure Funktionen Entwicklerreferenz](functions-reference.md)
* [Azure Funktionen F Nr. Entwicklerreferenz](functions-reference-fsharp.md)
* [Azure Funktionen NodeJS Entwicklerreferenz](functions-reference-node.md)
* [Azure Funktionen Trigger und Bindungen](functions-triggers-bindings.md)


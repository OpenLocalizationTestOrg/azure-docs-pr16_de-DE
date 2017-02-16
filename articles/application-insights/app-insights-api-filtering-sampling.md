<properties 
    pageTitle="Filterung und in der Anwendung Einsichten SDK preprocessing | Microsoft Azure" 
    description="Schreiben Sie Prozessoren werden und werden Initialisierung für das SDK zum Filtern oder Eigenschaften zu den Daten hinzufügen, bevor die werden mit dem Portal Anwendung Einsichten gesendet wird." 
    services="application-insights"
    documentationCenter="" 
    authors="beckylino" 
    manager="douge"/>
 
<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="08/30/2016" 
    ms.author="borooji"/>

# <a name="filtering-and-preprocessing-telemetry-in-the-application-insights-sdk"></a>Filterung und Präprozessorlauf werden in der Anwendung Einsichten SDK

*Anwendung Einsichten ist in der Vorschau.*

Sie können schreiben und Konfigurieren von Plug-ins für die Anwendung Einsichten SDK anpassen, wie werden erfasst und verarbeitet, bevor sie die Anwendung Einsichten-Dienst gesendet wird. 

Diese Features sind derzeit für das ASP.NET SDK verfügbar.

* [Stichproben](app-insights-sampling.md) verringert die Lautstärke der werden ohne Auswirkung auf die Statistik. Es wird zusammen Datenpunkte, damit Sie navigieren können Sie zwischen ihnen bei der Diagnose von Problemen im Zusammenhang. Im Portal werden die gesamten zählt multipliziert, um die Größe der Stichprobe zu berücksichtigen.
* [Filtern mit Telemetrieprotokoll Prozessoren](#filtering) können Sie auswählen oder werden im SDK ändern, bevor sie auf dem Server gesendet wird. Beispielsweise könnten Sie die Lautstärke werden reduzieren, indem Sie Anfragen von Robots ausschließen. Filtern ist eine weitere grundlegende Vorgehensweise zum Verringern Datenverkehr als werden jedoch. Beachten Sie, dass die Statistik -, beispielsweise betrifft Wenn Sie alle Erfolgreiche Anfragen herausfiltern müssen, jedoch können Sie besser steuern, welche übertragen wird.
* [Telemetrieprotokoll Initialisierung Eigenschaften hinzufügen](#add-properties) können alle aus der app, einschließlich werden aus der standard-Module gesendet werden. Beispielsweise könnten Sie berechneten Werte hinzufügen. oder Versionsnummern keinesfalls zum Filtern der Daten im Portal.
* [Der SDK-API](app-insights-api-custom-events-metrics.md) wird verwendet, um benutzerdefinierte Ereignisse und Kriterien zu senden.


Bevor Sie beginnen:

* Installieren Sie die [Anwendung Einsichten SDK für ASP.NET Version 2](app-insights-asp-net.md) in Ihrer app. 


<a name="filtering"></a>
## <a name="filtering-itelemetryprocessor"></a>Filtern: ITelemetryProcessor

Dieses Verfahren haben Sie mehr direkte Kontrolle über Was ist oder ausgeschlossen werden Streams. Können Sie sie in Verbindung mit werden, oder getrennt.

Zum Filtern werden, schreiben einen Prozessor werden und mit dem SDK zu registrieren. Werden alle durchläuft Ihren Prozessor, und Sie können auswählen, legen Sie es aus dem Stream ab, oder fügen Sie Eigenschaften. Dies umfasst, werden von der standard-Module wie der HTTP-Anforderung Collection und Abhängigkeit Collection und werden, die Sie selbst geschrieben haben. Sie können, beispielsweise werden zu Anfragen Roboter oder erfolgreich Abhängigkeit Anrufe herausfiltern. 

> [AZURE.WARNING] Filtern der aus dem SDK gesendet werden kann mit Prozessoren Neigung der Statistik, die im Portal angezeigt und ist es schwierig, führen Sie die zugehörigen Elemente.
> 
> Stattdessen Sie verwenden Sie [werden](app-insights-sampling.md).

### <a name="create-a-telemetry-processor"></a>Erstellen eines telemetrieprotokoll-Prozessors

1. Stellen Sie sicher, dass die Anwendung Einsichten SDK in Ihrem Projekt Version 2.0.0 ist oder höher. Mit der rechten Maustaste in Ihrem Projekts Explorer für Visual Studio-Lösung, und wählen Sie NuGet-Pakete verwalten. Aktivieren Sie NuGet-Paket-Manager Microsoft.ApplicationInsights.Web.

1. Zum Erstellen eines Filters, können implementieren Sie ITelemetryProcessor. Dies ist ein anderes Erweiterbarkeitspunkt wie Modul werden, werden Initialisierung und Channel werden. 

    Beachten Sie, dass eine Kette von Verarbeitung werden Prozessoren zu erstellen. Beim Instanziieren eines Prozessors werden übergeben Sie einen Link an den nächsten Prozessor in der Kette. Wenn Sie ein Datenpunkt werden an die Process-Methode übergeben wird, erledigt seine Arbeit und ruft dann in der Kette der nächsten werden Prozessor.

    ``` C#

    using Microsoft.ApplicationInsights.Channel;
    using Microsoft.ApplicationInsights.Extensibility;

    public class SuccessfulDependencyFilter : ITelemetryProcessor
      {
        private ITelemetryProcessor Next { get; set; }

        // You can pass values from .config
        public string MyParamFromConfigFile { get; set; }

        // Link processors to each other in a chain.
        public SuccessfulDependencyFilter(ITelemetryProcessor next)
        {
            this.Next = next;
        }
        public void Process(ITelemetry item)
        {
            // To filter out an item, just return 
            if (!OKtoSend(item)) { return; }
            // Modify the item if required 
            ModifyItem(item);

            this.Next.Process(item);
        }

        // Example: replace with your own criteria.
        private bool OKtoSend (ITelemetry item)
        {
            var dependency = item as DependencyTelemetry;
            if (dependency == null) return true;

            return dependency.Success != true;
        }

        // Example: replace with your own modifiers.
        private void ModifyItem (ITelemetry item)
        {
            item.Context.Properties.Add("app-version", "1." + MyParamFromConfigFile);
        }
    }
    

    ```
2. Fügen Sie diese ApplicationInsights.config: 

```XML

    <TelemetryProcessors>
      <Add Type="WebApplication9.SuccessfulDependencyFilter, WebApplication9">
         <!-- Set public property -->
         <MyParamFromConfigFile>2-beta</MyParamFromConfigFile>
      </Add>
    </TelemetryProcessors>

```

(Dies ist ein Abschnitt derselben Stelle, an der Sie einen Filter werden Initialisierung.)

Sie können Zeichenfolgenwerte aus der Datei config durch Bereitstellen von öffentlichen benannte Eigenschaften in der Klasse übergeben. 

> [AZURE.WARNING] Achten Sie auf den Namen und eine Eigenschaft in der Datei config den Namen Class und Eigenschaften im Code übereinstimmen. Wenn die config-Datei eine nicht vorhandene Typ oder diese Eigenschaft verweist, möglicherweise das SDK im Hintergrund keine telemetrieprotokoll zu senden.

 
**Alternativ** können Sie den Filter Code Initialisierung. Legen Sie in einer Klasse geeignete Initialization – beispielsweise AppStart in Global.asax.cs - Ihren Prozessor in der Kette aus:

```C#

    var builder = TelemetryConfiguration.Active.TelemetryProcessorChainBuilder;
    builder.Use((next) => new SuccessfulDependencyFilter(next));

    // If you have more processors:
    builder.Use((next) => new AnotherProcessor(next));

    builder.Build();

```

TelemetryClients nach diesem Zeitpunkt erstellt wird Ihrer Prozessoren verwendet.

### <a name="example-filters"></a>Beispiel-Filter

#### <a name="synthetic-requests"></a>Synthetische Besprechungsanfragen

Herausfiltern Sie Bots und Web Tests. Obwohl Kennzahlen Explorer die Option zum Filtern von synthetischen Quellen bietet, verringert diese Option Netzwerkverkehr durch Filtern sie bei der SDK.

``` C#

    public void Process(ITelemetry item)
    {
      if (!string.IsNullOrEmpty(item.Context.Operation.SyntheticSource)) {return;}

      // Send everything else: 
      this.Next.Process(item);
    }

```

#### <a name="failed-authentication"></a>Fehler beim Authentifizierung

Herausfiltern Sie Anfragen mit einer Antwort "401". 

```C#

public void Process(ITelemetry item)
{
    var request = item as RequestTelemetry;

    if (request != null &&
    request.ResponseCode.Equals("401", StringComparison.OrdinalIgnoreCase))
    {
        // To filter out an item, just terminate the chain: 
        return;
    }
    // Send everything else: 
    this.Next.Process(item);
}

```

#### <a name="filter-out-fast-remote-dependency-calls"></a>Schnelles remote Abhängigkeit Anrufe herausfiltern

Wenn Sie nur Anrufe diagnostizieren, die langsam möchten, herausfiltern Sie diejenigen machen. 

> [AZURE.NOTE] Dadurch wird die Statistik Neigung, die auf das Portal angezeigt. Das Diagramm Abhängigkeit wird aussehen, als wäre der Abhängigkeit Ruft alle Fehler bei der sind.

``` C#

public void Process(ITelemetry item)
{
    var request = item as DependencyTelemetry;
            
    if (request != null && request.Duration.Milliseconds < 100)
    {
        return;
    }
    this.Next.Process(item);
}

```

#### <a name="diagnose-dependency-issues"></a>Diagnostizieren Sie Abhängigkeitsprobleme

[Diesen Blog](https://azure.microsoft.com/blog/implement-an-application-insights-telemetry-processor/) werden ein Projekt, um die Abhängigkeitsprobleme diagnostizieren per reguläre Pings automatisch zu Abhängigkeiten an.


<a name="add-properties"></a>
## <a name="add-properties-itelemetryinitializer"></a>Fügen Sie Eigenschaften hinzu: ITelemetryInitializer

Verwenden Sie zum Definieren von globaler Eigenschaften, die mit allen werden gesendet werden Initialisierung werden; und ausgewählten Verhalten der standard werden Module außer Kraft setzen. 

Beispielsweise sammelt die Anwendung Einsichten für Web-Paket über HTTP-Anfragen werden. Standardmäßig kennzeichnet als eine Besprechungsanfrage mit einer Antwortcode fehlgeschlagen > = 400. Aber bei 400 als einen Erfolg behandelt werden soll, können Sie angeben, dass eine Initialisierung werden, die die Eigenschaft Success festlegt.

Wenn Sie eine Initialisierung werden bereitstellen, heißt es bei jeder Änderung der Track*() Methoden aufgerufen wird. Dies umfasst Methoden, die von den Modulen standard werden. Durch Messe führen Sie diese Module eine Eigenschaft, die bereits durch eine Initialisierung festgelegt wurde nicht festgelegt. 

**Definieren der Initialisierung**

*C#*

```C#

    using System;
    using Microsoft.ApplicationInsights.Channel;
    using Microsoft.ApplicationInsights.DataContracts;
    using Microsoft.ApplicationInsights.Extensibility;

    namespace MvcWebRole.Telemetry
    {
      /*
       * Custom TelemetryInitializer that overrides the default SDK 
       * behavior of treating response codes >= 400 as failed requests
       * 
       */
      public class MyTelemetryInitializer : ITelemetryInitializer
      {
        public void Initialize(ITelemetry telemetry)
        {
            var requestTelemetry = telemetry as RequestTelemetry;
            // Is this a TrackRequest() ?
            if (requestTelemetry == null) return;
            int code;
            bool parsed = Int32.TryParse(requestTelemetry.ResponseCode, out code);
            if (!parsed) return;
            if (code >= 400 && code < 500)
            {
                // If we set the Success property, the SDK won't change it:
                requestTelemetry.Success = true;
                // Allow us to filter these requests in the portal:
                requestTelemetry.Context.Properties["Overridden400s"] = "true";
            }
            // else leave the SDK to set the Success property      
        }
      }
    }
```

**Laden Sie Ihre Initialisierung**

In ApplicationInsights.config:

    <ApplicationInsights>
      <TelemetryInitializers>
        <!-- Fully qualified type name, assembly name: -->
        <Add Type="MvcWebRole.Telemetry.MyTelemetryInitializer, MvcWebRole"/> 
        ...
      </TelemetryInitializers>
    </ApplicationInsights>

*Sie können auch* die Initialisierung Code, beispielsweise in Global.aspx.cs erstellt werden können:


```C#
    protected void Application_Start()
    {
        // ...
        TelemetryConfiguration.Active.TelemetryInitializers
        .Add(new MyTelemetryInitializer());
    }
```


[Weitere Informationen Sie erhalten, der in diesem Beispiel.](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/MvcWebRole)

<a name="js-initializer"></a>
### <a name="javascript-telemetry-initializers"></a>JavaScript werden Initialisierung

*JavaScript*

Fügen Sie eine Initialisierung werden unmittelbar nach der Initialisierungscode, den Sie aus dem Portal Webhostinganbieter erhalten haben: 

```JS

    <script type="text/javascript">
        // ... initialization code
        ...({
            instrumentationKey: "your instrumentation key"
        });
        window.appInsights = appInsights;


        // Adding telemetry initializer.
        // This is called whenever a new telemetry item
        // is created.

        appInsights.queue.push(function () {
            appInsights.context.addTelemetryInitializer(function (envelope) {
                var telemetryItem = envelope.data.baseData;

                // To check the telemetry item’s type - for example PageView:
                if (envelope.name == Microsoft.ApplicationInsights.Telemetry.PageView.envelopeType) {
                    // this statement removes url from all page view documents
                    telemetryItem.url = "URL CENSORED";
                }

                // To set custom properties:
                telemetryItem.properties = telemetryItem.properties || {};
                telemetryItem.properties["globalProperty"] = "boo";

                // To set custom metrics:
                telemetryItem.measurements = telemetryItem.measurements || {};
                telemetryItem.measurements["globalMetric"] = 100;
            });
        });

        // End of inserted code.

        appInsights.trackPageView();
    </script>
```

Eine Zusammenfassung der nicht benutzerdefinierte Eigenschaften auf die TelemetryItem zur Verfügung finden Sie unter dem [Datenmodell](app-insights-export-data-model.md#lttelemetrytypegt).

Sie können beliebig viele Initialisierung beliebig hinzufügen. 


## <a name="itelemetryprocessor-and-itelemetryinitializer"></a>ITelemetryProcessor und ITelemetryInitializer

Was ist der Unterschied zwischen werden Prozessoren und Initialisierung werden?

* Es gibt einige überlappt in mit ihnen möglich: Beide können verwendet werden, werden Eigenschaften hinzu.
* TelemetryInitializers immer vor dem TelemetryProcessors ausführen.
* TelemetryProcessors ermöglichen es Ihnen, vollständig ersetzen oder löschen ein Element werden.
* TelemetryProcessors Leistung Zähler werden nicht verarbeitet werden.



## <a name="persistence-channel"></a>Beibehaltung Channel 

Ihre app ausgeführt wird, in dem die Verbindung zum Internet nicht immer verfügbar oder langsam ist, sollten Sie den dauerhaften Kanal anstelle des Standard-in-Memory-Kanals verwenden. 

Der Standard-in-Memory-Kanal verliert alle werden, die nicht zu dem Zeitpunkt gesendet wurde die app wird geschlossen. Sie können zwar `Flush()` versucht, alle Daten im Puffer verbleibenden senden und verliert es immer noch Daten ist es keine Verbindung mit dem Internet oder Übertragung abgeschlossen, wenn die app vor beendet wird ist.

Der dauerhaften Kanal puffert hingegen werden in einer Datei, bevor sie mit dem Portal gesendet. `Flush()`Damit ist sichergestellt, dass die Daten in der Datei gespeichert ist. Wenn keine Daten die app schließt nicht bis zu dem Zeitpunkt gesendet wurde, bleibt er in der Datei. Wenn die app gestartet wird, werden die Daten klicken Sie dann gesendet werden, wenn eine Internetverbindung besteht. Wenn sich in der Datei so lange erforderlich ist, bis eine Verbindung verfügbar ist. 

### <a name="to-use-the-persistence-channel"></a>Verwenden Sie den dauerhaften Kanal

1. Importieren Sie das NuGet-Paket [Microsoft.ApplicationInsights.PersistenceChannel](https://www.nuget.org/packages/Microsoft.ApplicationInsights.PersistenceChannel/1.2.3).
2. Nehmen Sie diese Code in Ihrer app in einem geeigneten Initialisierung Speicherort:
 
    ```C# 

      using Microsoft.ApplicationInsights.Channel;
      using Microsoft.ApplicationInsights.Extensibility;
      ...

      // Set up 
      TelemetryConfiguration.Active.InstrumentationKey = "YOUR INSTRUMENTATION KEY";
 
      TelemetryConfiguration.Active.TelemetryChannel = new PersistenceChannel();
    
    ``` 
3. Verwenden Sie `telemetryClient.Flush()` bevor Sie Ihre app wird geschlossen, um sicherzustellen, dass Daten im Portal gesendet oder auf die Datei gespeichert ist.

    Beachten Sie, dass Flush() synchron für den dauerhaften Kanal, aber für andere Kanäle asynchrone ist.

 
Der Beibehaltung Channel ist für Geräte-Szenarien optimiert, Stelle, an der die Anzahl der Ereignisse, durch die Anwendung erzeugt relativ klein ist und die Verbindung ist oft unzuverlässigen. Dieser Kanal schreibt Ereignisse auf den Datenträger in zuverlässigen Speicher zuerst und dann versuchen, sie zu senden. 

#### <a name="example"></a>Beispiel

Angenommen, überwachen Ausnahmefehler werden soll. Abonnieren Sie den `UnhandledException` Ereignis. In der Rückruf fügen Sie einen Anruf an Löschvorgang, um sicherzustellen, dass die telemetrieprotokoll beibehalten wird.
 
```C# 

AppDomain.CurrentDomain.UnhandledException += CurrentDomain_UnhandledException; 
 
... 
 
private void CurrentDomain_UnhandledException(object sender, UnhandledExceptionEventArgs e) 
{ 
    ExceptionTelemetry excTelemetry = new ExceptionTelemetry((Exception)e.ExceptionObject); 
    excTelemetry.SeverityLevel = SeverityLevel.Critical; 
    excTelemetry.HandledAt = ExceptionHandledAt.Unhandled; 
 
    telemetryClient.TrackException(excTelemetry); 
 
    telemetryClient.Flush(); 
} 

``` 

Wenn die app beendet wird, sehen Sie eine Datei im `%LocalAppData%\Microsoft\ApplicationInsights\`, der die komprimierte Ereignisse enthält. 
 
Nächsten dieser Anwendung Start, wird der Kanal wählen Sie diese Datei und werden an die Anwendung Einsichten übermitteln, wenn dies möglich.

#### <a name="test-example"></a>Beispiel für einen Test

```C#

using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.Extensibility;

namespace ConsoleApplication1
{
    class Program
    {
        static void Main(string[] args)
        {
            // Send data from the last time the app ran
            System.Threading.Thread.Sleep(5 * 1000);

            // Set up persistence channel

            TelemetryConfiguration.Active.InstrumentationKey = "YOUR KEY";
            TelemetryConfiguration.Active.TelemetryChannel = new PersistenceChannel();

            // Send some data

            var telemetry = new TelemetryClient();

            for (var i = 0; i < 100; i++)
            {
                var e1 = new Microsoft.ApplicationInsights.DataContracts.EventTelemetry("persistenceTest");
                e1.Properties["i"] = "" + i;
                telemetry.TrackEvent(e1);
            }

            // Make sure it's persisted before we close
            telemetry.Flush();
        }
    }
}

```


Der Code der den dauerhaften Kanal ist auf [Github](https://github.com/Microsoft/ApplicationInsights-dotnet/tree/v1.2.3/src/TelemetryChannels/PersistenceChannel). 


## <a name="reference-docs"></a>Verweis-Dokumente

* [Übersicht über die API](app-insights-api-custom-events-metrics.md)

* [ASP.NET Bezug](https://msdn.microsoft.com/library/dn817570.aspx)


## <a name="sdk-code"></a>SDK-Code

* [ASP.NET Core SDK](https://github.com/Microsoft/ApplicationInsights-dotnet)
* [ASP.NET 5](https://github.com/Microsoft/ApplicationInsights-aspnet5)
* [JavaScript-SDK](https://github.com/Microsoft/ApplicationInsights-JS)


## <a name="a-namenextanext-steps"></a><a name="next"></a>Nächste Schritte


* [Suchen von Ereignissen und Protokollen][diagnostic]
* [Werden](app-insights-sampling.md)
* [Behandlung von Problemen][qna]


<!--Link references-->

[client]: app-insights-javascript.md
[config]: app-insights-configuration-with-applicationinsights-config.md
[create]: app-insights-create-new-resource.md
[data]: app-insights-data-retention-privacy.md
[diagnostic]: app-insights-diagnostic-search.md
[exceptions]: app-insights-asp-net-exceptions.md
[greenbrown]: app-insights-asp-net.md
[java]: app-insights-java-get-started.md
[metrics]: app-insights-metrics-explorer.md
[qna]: app-insights-troubleshoot-faq.md
[trace]: app-insights-search-diagnostic-logs.md
[windows]: app-insights-windows-get-started.md

 

<properties 
    pageTitle="Konfigurieren von Anwendung Einsichten SDK mit ApplicationInsights.config oder XML" 
    description="Aktivieren oder Deaktivieren von Daten Sammlung Module, und Leistungsindikatoren und andere Parameter hinzufügen" 
    services="application-insights"
    documentationCenter="" 
    authors="OlegAnaniev-MSFT"
    editor="alancameronwills" 
    manager="douge"/>
 
<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="03/12/2016" 
    ms.author="awills"/>

# <a name="configuring-the-application-insights-sdk-with-applicationinsightsconfig-or-xml"></a>Konfigurieren die Anwendung Einsichten SDK mit ApplicationInsights.config oder XML

Die Anwendung Einsichten .NET SDK umfasst eine Reihe von NuGet-Pakete. Das [Core-Paket](http://www.nuget.org/packages/Microsoft.ApplicationInsights) stellt die API zum Senden von werden an die Anwendung Einsichten bereit. [Zusätzliche Pakete](http://www.nuget.org/packages?q=Microsoft.ApplicationInsights) bieten werden _Module_ und _Initialisierung_ zum Nachverfolgen von werden automatisch von Ihrer Anwendung und der Kontext an. Durch Anpassen der Konfigurationsdatei, können Sie aktivieren oder deaktivieren, werden Module und Initialisierung und Parameter für einige Felder festlegen.

Konfigurationsdatei heißt `ApplicationInsights.config` oder `ApplicationInsights.xml`, je nach Typ des Ihrer Anwendung. Es wird automatisch zum Projekt hinzugefügt Wenn Sie [die meisten Versionen von SDK installieren][start]. Es wird auch bei einer Web-app hinzugefügt, nach [Status Monitor auf einem IIS-Server][redfield], oder wenn Sie die Appplication Einsichten [Erweiterung für eine Website Azure oder virtuellen Computer](app-insights-azure-web-apps.md)auswählen.

Es wird eine entsprechende Datei Steuern des [SDK in eine Webseite]nicht[client].

Dieses Dokument beschreibt die Abschnitte, die Sie in der Konfiguration finden Sie unter Datei, wie sie die Komponenten des SDK, kontrollieren und welche NuGet Pakete laden Sie diese Komponenten.

## <a name="telemetry-modules-aspnet"></a>Werden Module (ASP.NET)

Jedem Modul werden sammelt einen bestimmten Typ von Daten und das Herzstück API verwendet, um die Daten zu senden. Module werden von verschiedenen NuGet-Paketen installiert, die auch die erforderlichen Zeilen der config-Datei hinzufügen.

Es ist ein Knoten in der Konfigurationsdatei für jedes Modul. Um ein Modul zu deaktivieren, löschen Sie den Knoten oder kommentieren Sie es aus.



### <a name="dependency-tracking"></a>Abhängigkeit nachverfolgen

[Abhängigkeit Überwachung](app-insights-dependencies.md) erfasst werden Informationen zum Aufrufen die app-Datenbanken und externe Dienste und Datenbanken durch. Um dieses Modul für die Arbeit in einem IIS-Server ermöglichen möchten, müssen Sie [Installieren Status Monitor][redfield]. Verwendung in Azure Web apps oder virtuellen Computern, und [Wählen Sie die Anwendung Einsichten Erweiterung](app-insights-azure-web-apps.md).

Sie können auch eigene Abhängigkeit verfolgen mithilfe der [TrackDependency-API](app-insights-api-custom-events-metrics.md#track-dependency)Code schreiben.


* `Microsoft.ApplicationInsights.DependencyCollector.DependencyTrackingTelemetryModule`
* [Microsoft.ApplicationInsights.DependencyCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector) NuGet-Paket.

### <a name="performance-collector"></a>Leistung Collection

Wie CPU, Arbeitsspeicher und Netzwerk [sammelt Systemleistungsindikatoren](app-insights-performance-counters.md) Laden von IIS-Installationen. Sie können angeben, welche Indikatoren zu sammeln, einschließlich der Leistungsindikatoren, die Sie selbst eingerichtet haben.

* `Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule`
* [Microsoft.ApplicationInsights.PerfCounterCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.PerfCounterCollector) NuGet-Paket.


### <a name="application-insights-diagnostics-telemetry"></a>Anwendung Einsichten Diagnose werden

Die `DiagnosticsTelemetryModule` meldet Fehler in der Anwendung Einsichten Instrumentationscode selbst. Angenommen, wenn der Code-Datenquellen Zugriff nicht möglich ist oder eine `ITelemetryInitializer` eine Ausnahme auslösen. Spur werden nach diesem Modul erfasst angezeigt wird, in das Feld [Suchen Diagnostic][diagnostic]. Sendet diagnostische Daten an dc.services.vsallin.net.
 
* `Microsoft.ApplicationInsights.Extensibility.Implementation.Tracing.DiagnosticsTelemetryModule`
* [Microsoft.ApplicationInsights](http://www.nuget.org/packages/Microsoft.ApplicationInsights) NuGet-Paket. Wenn Sie dieses Paket nur installiert haben, wird die ApplicationInsights.config-Datei nicht automatisch erstellt. 

### <a name="developer-mode"></a>Entwicklermodus

`DeveloperModeWithDebuggerAttachedTelemetryModule`Erzwingt die Anwendung Einsichten `TelemetryChannel` zum Senden von Daten sofort, werden ein Element gleichzeitig anzeigt, wenn ein Debugger an der Anwendungsprozess angefügt wird. Dies reduziert die Zeitdauer zwischen dem Moment, wenn Ihrer Anwendung werden nachverfolgt, und wenn sie auf das Portal Anwendung Einsichten angezeigt. Es führt zu erheblichen Verwaltungsaufwand in CPU und Netzwerk Bandbreite.

* `Microsoft.ApplicationInsights.WindowsServer.DeveloperModeWithDebuggerAttachedTelemetryModule`
* [Anwendung Einsichten WindowsServer](http://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) NuGet-Paket

### <a name="web-request-tracking"></a>Web-Anforderung nachverfolgen

Die [Antwort sowie das Ergebnis Code](app-insights-asp-net.md) der HTTP-Anfragen-Berichte. 

* `Microsoft.ApplicationInsights.Web.RequestTrackingTelemetryModule`
* [Microsoft.ApplicationInsights.Web](http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) NuGet-Paket

### <a name="exception-tracking"></a>Ausnahme nachverfolgen

`ExceptionTrackingTelemetryModule`Titel-Ausnahmefehler im Web app. Finden Sie unter [Fehler und Ausnahmen][exceptions].

* `Microsoft.ApplicationInsights.Web.ExceptionTrackingTelemetryModule`
* [Microsoft.ApplicationInsights.Web](http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) NuGet-Paket


* `Microsoft.ApplicationInsights.WindowsServer.UnobservedExceptionTelemetryModule`-verfolgt [nicht überwachten Vorgang Ausnahmen](http://blogs.msdn.com/b/pfxteam/archive/2011/09/28/task-exception-handling-in-net-4-5.aspx).
* `Microsoft.ApplicationInsights.WindowsServer.UnhandledExceptionTelemetryModule`-Ausnahmefehler für Worker-Rollen, Windows-Diensten und Console Applikationen verfolgt.
* [Anwendung Einsichten WindowsServer](http://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) NuGet-Paket.

### <a name="microsoftapplicationinsights"></a>Microsoft.ApplicationInsights

Das Paket Microsoft.ApplicationInsights stellt die [Core-API](https://msdn.microsoft.com/library/mt420197.aspx) des SDK bereit. Die anderen werden Module verwenden Sie diese Option, und Sie können auch [verwenden, um eigene telemetrieprotokoll definieren](app-insights-api-custom-events-metrics.md).

* Kein Eintrag in ApplicationInsights.config.
* [Microsoft.ApplicationInsights](http://www.nuget.org/packages/Microsoft.ApplicationInsights) NuGet-Paket. Wenn Sie nur diese NuGet installiert haben, wird keine config-Datei generiert.

## <a name="telemetry-channel"></a>Channel werden

Der Kanal werden verwaltet Puffer und Übertragung werden in der Anwendung Einsichten Service. 

* `Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.ServerTelemetryChannel`ist der Standardkanal für Dienste. Sie können Daten im Speicher puffert.
* `Microsoft.ApplicationInsights.PersistenceChannel`ist eine Alternative für Applikationen Console. Es kann keine unflushed Daten dauerhaft speichern, wenn Ihre app wird geschlossen, und sendet es, wenn die app erneut gestartet wird.


## <a name="telemetry-initializers-aspnet"></a>Telemetrieprotokoll Initialisierung (ASP.NET)

Telemetrieprotokoll Initialisierung Kontexteigenschaften festlegen, die zusammen mit wird jedes Element der werden gesendet werden. 

Sie können [eigene Initialisierung schreiben](app-insights-api-filtering-sampling.md#add-properties) , um Kontexteigenschaften festlegen.

Der standard-Initialisierung sind entweder über das Web oder WindowsServer NuGet-Paketen festlegen:


* `AccountIdTelemetryInitializer`Legt die Eigenschaft AccountId.
* `AuthenticatedUserIdTelemetryInitializer`Legt die Eigenschaft AuthenticatedUserId als vom JavaScript SDK festlegen.
* `AzureRoleEnvironmentTelemetryInitializer`Updates der `RoleName` und `RoleInstance` Eigenschaften der `Device` Kontext für alle werden Elemente mit Informationen aus der Azure Runtime-Umgebung extrahiert.
* `BuildInfoConfigComponentVersionTelemetryInitializer`Updates der `Version` -Eigenschaft der `Component` Kontext für alle werden Elemente mit dem Wert extrahiert aus der `BuildInfo.config` -Datei von MS-Build erzeugt.
* `ClientIpHeaderTelemetryInitializer`Updates `Ip` Eigenschaft den `Location` Kontext aller werden Elemente auf der Grundlage der `X-Forwarded-For` HTTP-Header der Anforderung.
* `DeviceTelemetryInitializer`aktualisiert die folgenden Eigenschaften von der `Device` Kontext für alle werden Elemente.
 - `Type`Klicken Sie auf "PC" festgelegt ist
 - `Id`wird festgelegt auf den Domänennamen des Computers ein, wenn die Anwendung ausgeführt wird.
 - `OemName`der Wert von extrahiert festgelegt ist die `Win32_ComputerSystem.Manufacturer` Feld WMI verwenden.
 - `Model`der Wert von extrahiert festgelegt ist die `Win32_ComputerSystem.Model` Feld WMI verwenden.
 - `NetworkType`der Wert von extrahiert festgelegt ist die `NetworkInterface`.
 - `Language`Klicken Sie auf den Namen des festgelegt ist die `CurrentCulture`.
* `DomainNameRoleInstanceTelemetryInitializer`Updates der `RoleInstance` -Eigenschaft der `Device` Kontext für alle werden Elemente mit dem Domänennamen des Computers ein, wenn die Anwendung ausgeführt wird.
* `OperationNameTelemetryInitializer`Updates der `Name` Eigenschaft den `RequestTelemetry` und der `Name` -Eigenschaft der `Operation` Kontext aller werden Elemente ausgehend von der HTTP-Methode sowie die Namen von ASP.NET-MVC Controller und Aktion aufgerufen, um die Anfrage zu verarbeiten.
* `OperationIdTelemetryInitializer`oder `OperationCorrelationTelemetryInitializer` Aktualisierungen der `Operation.Id` Kontext-Eigenschaft aller werden Elemente erfasst werden, während der Bearbeitung einer Besprechungsanfrage für die automatisch generierte `RequestTelemetry.Id`.
* `SessionTelemetryInitializer`Updates der `Id` -Eigenschaft der `Session` Kontext für alle werden Elemente mit dem Wert extrahiert aus der `ai_session` Cookies von der Ausführung im Browser des Benutzers ApplicationInsights JavaScript-Ausstattungscode generiert. 
* `SyntheticTelemetryInitializer`oder `SyntheticUserAgentTelemetryInitializer` Aktualisierungen der `User`, `Session` und `Operation` Kontexten Eigenschaften aller werden Elemente nachverfolgt werden soll, wenn eine Anforderung aus einer synthetischen Quelle, wie eine Verfügbarkeit testen oder suchen-Engine Bot zu behandeln. Standardmäßig wird [Kennzahlen Explorer](app-insights-metrics-explorer.md) synthetische werden nicht angezeigt. 

    Die `<Filters>` Festlegen der Eigenschaften des Webparts für die Anfragen zu identifizieren.
* `UserAgentTelemetryInitializer`Updates der `UserAgent` -Eigenschaft der `User` Kontext aller werden Elemente auf der Grundlage der `User-Agent` HTTP-Header der Anforderung.
* `UserTelemetryInitializer`Updates der `Id` und `AcquisitionDate` Eigenschaften `User` Kontext für alle werden Elemente mit Werten aus extrahiert die `ai_user` Cookies von der Ausführung im Browser des Benutzers Anwendung Einsichten JavaScript-Ausstattungscode generiert.
* `WebTestTelemetryInitializer`Benutzer-Id, Id für eine Sitzung und synthetische Datenquelleneigenschaften für HTTP-Anfragen, die von der [Verfügbarkeit Tests](app-insights-monitor-web-app-availability.md)stammen festgelegt.
Die `<Filters>` Festlegen der Eigenschaften des Webparts für die Anfragen zu identifizieren.

## <a name="telemetry-processors-aspnet"></a>Telemetrieprotokoll Prozessoren (ASP.NET)

Telemetrieprotokoll Prozessoren können filtern und jedes Element werden unmittelbar vor dem Senden aus dem SDK-Portal zu ändern.

Sie können [eigene Prozessoren werden schreiben](app-insights-api-filtering-sampling.md#filtering).


#### <a name="adaptive-sampling-telemetry-processor-from-200-beta3"></a>Adaptive Stichproben werden Prozessor (aus 2.0.0-beta3)

Dies ist standardmäßig aktiviert. Wenn Ihre app werden viele sendet, entfernt dieser Prozessor einige davon aus.

```xml

    <TelemetryProcessors>
      <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.AdaptiveSamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
        <MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>
      </Add>
    </TelemetryProcessors>

```

Der Parameter liefert die Zieladresse, die der Algorithmus zu erreichen versucht. Jede Instanz des SDK arbeitet unabhängig voneinander, wenn der Server ein Cluster von mehreren Computern ist, wird die tatsächliche Lautstärke werden entsprechend multipliziert.

[Erfahren Sie mehr über werden](app-insights-sampling.md).



#### <a name="fixed-rate-sampling-telemetry-processor-from-200-beta1"></a>Festverzinslichen Stichproben werden Prozessor (aus 2.0.0-beta1)

Es gibt auch ein standard- [Stichproben werden Prozessor](app-insights-api-filtering-sampling.md#sampling) (von 2.0.1):

```XML

    <TelemetryProcessors>
     <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.SamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">

     <!-- Set a percentage close to 100/N where N is an integer. -->
     <!-- E.g. 50 (=100/2), 33.33 (=100/3), 25 (=100/4), 20, 1 (=100/100), 0.1 (=100/1000) -->
     <SamplingPercentage>10</SamplingPercentage>
     </Add>
   </TelemetryProcessors>

```



## <a name="channel-parameters-java"></a>Channel Parameter (Java)

Diese Parameter beeinflussen, wie die Java SDK speichern und die werden Daten, die es sammelt leeren sollte.

#### <a name="maxtelemetrybuffercapacity"></a>MaxTelemetryBufferCapacity

Die Anzahl der Elemente werden, die in SDKs in-Memory-Speicher gespeichert werden können. Wenn diese Anzahl erreicht ist, werden Puffer wird geleert – d. h., die Elemente werden auf dem Server Anwendung Einsichten gesendet werden.

-   Min: 1
-   Max: 1000
-   Standard: 500

```

  <ApplicationInsights>
      ...
      <Channel>
       <MaxTelemetryBufferCapacity>100</MaxTelemetryBufferCapacity>
      </Channel>
      ...
  </ApplicationInsights>
```

#### <a name="flushintervalinseconds"></a>FlushIntervalInSeconds 

Bestimmt, wie oft die Daten, die in der in-Memory-Speicher gespeichert ist (gesendete zu Anwendung Einsichten) geleert werden soll.

-   Min: 1
-   Max: 300
-   Standard: 5

```

    <ApplicationInsights>
      ...
      <Channel>
        <FlushIntervalInSeconds>100</FlushIntervalInSeconds>
      </Channel>
      ...
    </ApplicationInsights>
```

#### <a name="maxtransmissionstoragecapacityinmb"></a>MaxTransmissionStorageCapacityInMB

Bestimmt die maximale Größe in MB, die in den permanenten Speicher auf der lokalen Festplatte zugewiesen wird. Dieser Speicher wird für beibehalten werden Elemente verwendet, die an den Endpunkt Anwendung Einsichten übermittelt werden konnte. Neue werden Elemente gehen verloren, wenn die Größe des Speichers erfüllt ist.

-   Min: 1
-   Max: 100
-   Standard: 10

```

   <ApplicationInsights>
      ...
      <Channel>
        <MaxTransmissionStorageCapacityInMB>50</MaxTransmissionStorageCapacityInMB>
      </Channel>
      ...
   </ApplicationInsights>
```



## <a name="instrumentationkey"></a>InstrumentationKey

Dadurch werden die Anwendung Einsichten Ressource, in der die Daten angezeigt wird. In der Regel erstellen Sie eine separate Ressource, mit einem separaten Schlüssel, für jede Anwendung ein.

Wenn Sie die Taste festlegen dynamisch – beispielsweise, wenn Sie die Ergebnisse aus Ihrer Anwendung zu anderen Ressourcen – senden möchten möchten können Sie die Taste aus der Konfigurationsdatei auslassen und stattdessen im Code festlegen.

Informationen zum Festlegen des Schlüssels für alle Instanzen von TelemetryClient legen einschließlich standard werden Module, Sie die Taste in TelemetryConfiguration.Active fest. Führen Sie diese in einer Initialisierungsmethode, z. B. global.aspx.cs in einem ASP.NET-Dienst aus:

```C#

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey = 
          // - for example -
          WebConfigurationManager.Settings["ikey"];
      //...
```

Wenn Sie eine bestimmte Gruppe von Ereignissen zu einer anderen Ressource senden möchten, können Sie die Taste für eine bestimmte TelemetryClient festlegen:

```C#

    var tc = new TelemetryClient();
    tc.Context.InstrumentationKey = "----- my key ----";
    tc.TrackEvent("myEvent");
    // ...

```

[Erfahren Sie mehr über die API][api].

Um eine neue Key, [erstellen eine neue Ressource im Portal Anwendung Einsichten]erhalten[new].

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[client]: app-insights-javascript.md
[diagnostic]: app-insights-diagnostic-search.md
[exceptions]: app-insights-asp-net-exceptions.md
[netlogs]: app-insights-asp-net-trace-logs.md
[new]: app-insights-create-new-resource.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[start]: app-insights-overview.md


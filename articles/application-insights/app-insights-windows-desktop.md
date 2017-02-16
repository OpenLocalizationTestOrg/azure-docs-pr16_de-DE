<properties 
    pageTitle="Überwachen der Verwendung und Leistung für Windows-desktop-apps" 
    description="Verwendung und Leistung von Ihrer Windows-desktop-app mit HockeyApp und Anwendung Einsichten zu analysieren." 
    services="application-insights" 
    documentationCenter="windows"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/26/2016" 
    ms.author="awills"/>

# <a name="monitoring-usage-and-performance-in-windows-desktop-apps"></a>Überwachen der Verwendung und Leistung in Windows-Desktop-apps

*Anwendung Einsichten ist in der Vorschau.*

[Visual Studio-Anwendung Einsichten](app-insights-overview.md) und [HockeyApp](https://hockeyapp.net) lassen Sie die bereitgestellte Anwendung für die Verwendung und Leistung zu überwachen.

> [AZURE.IMPORTANT] Es empfiehlt sich [HockeyApp](https://hockeyapp.net) zum Verteilen und Überwachen der Desktop- und Gerät apps. Mit HockeyApp können Sie verwalten Verteilung, live testen und Feedback von Benutzern, sowie Verwendung und Absturz Berichte überwachen. Sie können auch [Exportieren und Ihre werden mit Analytics Abfragen](app-insights-hockeyapp-bridge-app.md).

> Obwohl werden an Anwendung Einsichten aus einer desktop-Anwendung gesendet werden kann, ist dies hauptsächlich zum Debuggen und Versuche hilfreich.


## <a name="to-send-telemetry-to-application-insights-from-a-windows-application"></a>Werden an Anwendung Einsichten aus einer Windows-Anwendung zu senden.

1. Im [Portal Azure](https://portal.azure.com) [eine Ressource Anwendung Einsichten erstellen](app-insights-create-new-resource.md). Wählen Sie für Anwendungstyp ASP.NET app aus.
2. Nehmen Sie eine Kopie der Schlüssel Instrumentation in Anspruch. Suchen Sie die Taste in der Dropdownliste Essentials die neue Ressource, die Sie soeben erstellt haben. 
3. Klicken Sie in Visual Studio bearbeiten Sie NuGet-Pakete Ihres Projekts app, und fügen Sie Microsoft.ApplicationInsights.WindowsServer. (Oder wählen Sie Microsoft.ApplicationInsights aus, wenn Sie nur die absolut erforderlichen wünschen-API, ohne die Standardansicht werden Websitesammlung Module.)
4. Legen Sie der Taste Instrumentation entweder im Code:

    `TelemetryConfiguration.Active.InstrumentationKey = "`*der Schlüssel*`";` 

    oder ApplicationInsights.config (Wenn Sie eines der standard werden Pakete installiert haben):
 
    `<InstrumentationKey>`*der Schlüssel*`</InstrumentationKey>` 

    Wenn Sie ApplicationInsights.config verwenden, stellen Sie sicher, dass ihre Eigenschaften im Explorer-Lösung sind schieben **Build Action = Inhalte Copy to Output Directory = kopieren**.
5. [Verwenden der API](app-insights-api-custom-events-metrics.md) gesendet werden.
6. Führen Sie die app, und finden Sie unter der werden in der Ressource im Portal Azure erstellten.

## <a name="a-nametelemetryaexample-code"></a><a name="telemetry"></a>Beispielcode

```C#

    public partial class Form1 : Form
    {
        private TelemetryClient tc = new TelemetryClient();
        ...
        private void Form1_Load(object sender, EventArgs e)
        {
            // Alternative to setting ikey in config file:
            tc.InstrumentationKey = "key copied from portal";

            // Set session data:
            tc.Context.User.Id = Environment.UserName;
            tc.Context.Session.Id = Guid.NewGuid().ToString();
            tc.Context.Device.OperatingSystem = Environment.OSVersion.ToString();

            // Log a page view:
            tc.TrackPageView("Form1");
            ...
        }

        protected override void OnClosing(CancelEventArgs e)
        {
            stop = true;
            if (tc != null)
            {
                tc.Flush(); // only for desktop apps

                // Allow time for flushing:
                System.Threading.Thread.Sleep(1000);
            }
            base.OnClosing(e);
        }

```

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen Sie ein dashboard](app-insights-dashboards.md)
* [Diagnose suchen](app-insights-diagnostic-search.md)
* [Kennzahlen durchsuchen](app-insights-metrics-explorer.md)
* [Schreiben Sie Analytics-Abfragen](app-insights-analytics.md)
 

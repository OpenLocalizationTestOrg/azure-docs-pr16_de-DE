<properties 
    pageTitle="Untersuchen von Protokollen in der Anwendung Einblicke in .NET Spur" 
    description="Suchen Sie die Protokolle mit Spur, NLog oder Log4Net generiert." 
    services="application-insights" 
    documentationCenter=".net"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/21/2016" 
    ms.author="awills"/>
 
# <a name="explore-net-trace-logs-in-application-insights"></a>Untersuchen von Protokollen in der Anwendung Einblicke in .NET Spur  

Wenn Sie NLog, log4Net oder System.Diagnostics.Trace für diagnostic Tracing in Ihrer Anwendung ASP.NET verwenden können Sie Ihre Protokolle gesendet, um [Visual Studio Anwendung Einsichten]haben[start], wo Sie können untersuchen und durchsuchen. Ihre Protokolle werden mit der anderen werden in Kürze mit aus Ihrer Anwendung, damit können Sie die Spuren gerade jeder Benutzer Anforderung zugeordnet zu identifizieren und können mit anderen Ereignisse und Ausnahme Berichte koordinieren zusammengeführt werden.




> [AZURE.NOTE] Benötigen Sie das Protokolldatei erfassen Modul? Es ist eine hilfreiche Netzwerkadapter für 3rd von Drittanbietern zur Aufzeichnung der Tastatureingabe, aber wenn Sie bereits NLog, log4Net oder System.Diagnostics.Trace verwenden, sollten Sie nur die [Anwendung Einsichten TrackTrace()](app-insights-api-custom-events-metrics.md#track-trace) direkt aufzurufen.


## <a name="install-logging-on-your-app"></a>Installieren der app anmelden

Installieren der ausgewählten Protokollierung Framework im Projekt an. Dies sollte einen Eintrag in app.config oder web.config führen.

Wenn Sie System.Diagnostics.Trace verwenden, müssen Sie einen Eintrag web.config hinzuzufügen:

```XML

    <configuration>
     <system.diagnostics>
       <trace autoflush="false" indentsize="4">
         <listeners>
           <add name="myListener" 
             type="System.Diagnostics.TextWriterTraceListener" 
             initializeData="TextWriterOutput.log" />
           <remove name="Default" />
         </listeners>
       </trace>
     </system.diagnostics>
   </configuration>
```

## <a name="configure-application-insights-to-collect-logs"></a>Konfigurieren der Anwendung Einsichten zum Sammeln von Protokollen

**[Hinzufügen von Anwendung Einsichten zu einem Projekt](app-insights-asp-net.md)** , wenn Sie die noch nicht getan haben. Sie sehen eine Option die Log Collection aufnehmen möchten.

Oder **Anwendung Einsichten konfigurieren** , indem Sie mit der rechten Maustaste in Ihr Projekts im Explorer-Lösung. Wählen Sie die Option **Spur**-Auflistung konfigurieren.

*Keine Anwendung Einsichten Menü oder Log Collection Option?* Versuchen Sie [zur Problembehandlung](#troubleshooting)aus.


## <a name="manual-installation"></a>Manuelle installation

Verwenden Sie diese Methode, wenn der Projekttyp durch die Anwendung Einsichten Installer (beispielsweise ein desktop Windows-Projekt) nicht unterstützt wird. 

1. Wenn Sie beabsichtigen, log4Net oder NLog verwenden, installieren Sie sie in Ihrem Projekt. 
2. Klicken Sie im Explorer-Lösung mit der rechten Maustaste in Ihrem Projekts, und wählen Sie **NuGet-Pakete verwalten**.
3. Suchen nach "Anwendung Einsichten"

    ![Die Vorabversion von der entsprechenden Netzwerkadapter abrufen](./media/app-insights-asp-net-trace-logs/appinsights-36nuget.png)

4. Wählen Sie das entsprechende Paket – eine der:
  + Microsoft.ApplicationInsights.TraceListener (zum Erfassen von System.Diagnostics.Trace Anrufe)
  + Microsoft.ApplicationInsights.NLogTarget
  + Microsoft.ApplicationInsights.Log4NetAppender

NuGet-Paket installiert die notwendigen Assemblys und auch ändert web.config oder app.config.

## <a name="insert-diagnostic-log-calls"></a>Einfügen von Diagnoseprotokoll Anrufe

Wenn Sie System.Diagnostics.Trace verwenden, wäre eine typische anrufen:

    System.Diagnostics.Trace.TraceWarning("Slow response - database01");

Wenn Sie log4net oder NLog bevorzugen:

    logger.Warn("Slow response - database01");


## <a name="using-the-trace-api-directly"></a>Verwenden die Spur API direkt

Sie können die Anwendung Einsichten Spur API direkt aufrufen. Verwenden Sie die Protokollierung Netzwerkadapter dieser API aus. 

Beispiel:

    var telemetry = new Microsoft.ApplicationInsights.TelemetryClient();
    telemetry.TrackTrace("Slow response - database01");

Ein Vorteil der TrackTrace ist, dass Sie die Nachricht relativ lange Daten hinzufügen können. Beispielsweise könnten Sie es POST-Daten codieren. 

Darüber hinaus können Sie eine Ebene schwere zu Ihrer Nachricht hinzufügen. Und wie andere werden, können Sie Immobilienwerte, mit denen Sie Hilfe Filter oder Ihrer Suche für unterschiedliche Optionssätze auf, hinzufügen. Beispiel:


    var telemetry = new Microsoft.ApplicationInsights.TelemetryClient();
    telemetry.TrackTrace("Slow database response",
                   SeverityLevel.Warning,
                   new Dictionary<string,string> { {"database", db.ID} });

Auf diese Weise können Sie im [Feld Suche][diagnostic], um alle Nachrichten von einer bestimmten Sicherheitsebene für eine bestimmte Datenbank einfach herausfiltern.

## <a name="explore-your-logs"></a>Untersuchen der Protokolle

Ihre app im Debuggen-Modus ausgeführt, oder es live bereitstellen.

In Ihrer app Übersicht vorher in [der Anwendung Einsichten Portal][portal], wählen Sie [Suchen]aus[diagnostic].

![Wählen Sie in der Anwendung Einblicken suchen aus.](./media/app-insights-asp-net-trace-logs/020-diagnostic-search.png)

![Suchen](./media/app-insights-asp-net-trace-logs/10-diagnostics.png)

Sie können, beispielsweise:

* Filtern Sie auf Log Spuren oder auf Elemente mit bestimmten Eigenschaften
* Prüfen eines bestimmten Elements im Detail.
* Suchen nach anderen für den gleichen Benutzer Anforderung werden (d. h., mit der gleichen OperationId) 
* Speichern Sie die Konfiguration von dieser Seite als Favorit

> [AZURE.NOTE] **Stichprobe.** Wenn eine Anwendung eine große Datenmenge sendet, und Sie die Anwendung Einsichten SDK für ASP.NET Version 2.0.0-beta3 oder höher verwenden, möglicherweise das Feature adaptive werden die Steuerung und senden nur Prozentwert der werden. [Weitere Informationen zu werden.](app-insights-sampling.md)

## <a name="next-steps"></a>Nächste Schritte

[Diagnostizieren von Fehlern und Ausnahmen in ASP.NET][exceptions]

[Weitere Informationen zum Suchen][diagnostic].



## <a name="troubleshooting"></a>Behandlung von Problemen

### <a name="how-do-i-do-this-for-java"></a>Wie tun ich diese für Java?

Verwenden Sie die [Java Log Netzwerkadapter](app-insights-java-trace-logs.md).

### <a name="theres-no-application-insights-option-on-the-project-context-menu"></a>Es gibt keine Option Anwendung Einsichten in die Project-Kontextmenü

* Überprüfen Sie die Anwendung Einsichten Tools auf diesem Entwicklungscomputer installiert ist. Suchen Sie in Visual Studio im Menü Extras, Erweiterungen und Updates für die Anwendung Einsichten Tools. Wenn es in die Registerkarte installiert ist, öffnen Sie die Registerkarte Online und zu installieren.
* Dies möglicherweise einen Typ des Projekts mithilfe von Tools Einsichten Anwendung nicht unterstützt. Verwenden Sie die [Manuelle Installation](#manual-installation).

### <a name="no-log-adapter-option-in-the-configuration-tool"></a>Die Option keine Log Netzwerkadapter im Konfigurationstool

* Sie müssen zuerst das Protokollierung Framework installieren.
* Wenn Sie System.Diagnostics.Trace verwenden, stellen Sie sicher, dass Sie [so konfiguriert, dass es in `web.config` ](https://msdn.microsoft.com/library/system.diagnostics.eventlogtracelistener.aspx).
* Haben Sie die neueste Version der Anwendung Einsichten Tools? Klicken Sie im Menü Visual Studio- **Tools** wählen Sie **Extensions und Updates aus**, und öffnen Sie die Registerkarte **Updates** . Wenn die Anwendung Einsichten Tools vorhanden ist, klicken Sie auf um ihn zu aktualisieren.


### <a name="a-nameemptykeyai-get-an-error-instrumentation-key-cannot-be-empty"></a><a name="emptykey"></a>Ich erhalte die Fehlermeldung "Instrumentation Schlüssel darf nicht leer sein"

Wie Sie das Protokollierung Netzwerkadapter Nuget-Paket installiert haben, ohne Installation von Anwendung Einsichten sieht so aus.

Explorer-Lösung, mit der Maustaste `ApplicationInsights.config` , und wählen Sie **Die Anwendung Einsichten aktualisieren**. Erhalten Sie ein Dialogfeld, mit der Sie die Anmeldung bei Azure eingeladen, und erstellen Sie entweder eine Anwendung Einsichten Ressource, oder verwenden Sie eine vorhandene erneut. Die sollte das Problem zu lösen.

### <a name="i-can-see-traces-in-diagnostic-search-but-not-the-other-events"></a>Ich sehe Spuren in diagnostic suchen, aber nicht die anderen Ereignisse

Manchmal kann eine Weile für alle Ereignisse und Besprechungsanfragen, um durch die Verkaufspipeline Lizenzierungsoptionen dauern.

### <a name="a-namelimitsahow-much-data-is-retained"></a><a name="limits"></a>Wie viele Daten werden beibehalten?

Bis zu 500 Ereignisse pro Sekunde aus jeder Anwendung. Ereignisse sind sieben Tage lang aufbewahrt.

### <a name="im-not-seeing-some-of-the-log-entries-that-i-expect"></a>Ich bin nicht Teil der Protokolleinträge angezeigt, die ich erwarten

Wenn eine Anwendung eine große Datenmenge sendet, und Sie die Anwendung Einsichten SDK für ASP.NET Version 2.0.0-beta3 oder höher verwenden, möglicherweise das Feature adaptive werden die Steuerung und senden nur Prozentwert der werden. [Weitere Informationen zu werden.](app-insights-sampling.md)

## <a name="a-nameaddanext-steps"></a><a name="add"></a>Nächste Schritte

* [Einrichten von Verfügbarkeit und Reaktionszeiten tests][availability]
* [Behandlung von Problemen][qna]





<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[exceptions]: app-insights-asp-net-exceptions.md
[portal]: https://portal.azure.com/
[qna]: app-insights-troubleshoot-faq.md
[start]: app-insights-overview.md

 

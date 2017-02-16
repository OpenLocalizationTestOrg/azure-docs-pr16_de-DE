<properties 
    pageTitle="Telemetrieprotokoll werden in der Anwendung Einsichten | Microsoft Azure" 
    description="So behalten Sie die Lautstärke der gesteuert werden." 
    services="application-insights" 
    documentationCenter="windows"
    authors="vgorbenko" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/30/2016" 
    ms.author="awills"/>

#  <a name="sampling-in-application-insights"></a>Werden in Anwendung Einsichten

*Anwendung Einsichten ist in der Vorschau.*


Stichproben ist, dass ein Feature in [Visual Studio Anwendung Einsichten](app-insights-overview.md) wird empfohlen, gleichzeitiger eine statistisch richtige Analyse der Anwendungsdaten werden Datenverkehr und Speicherplatz zu verringern. Der Filters markiert Elemente, die verwandt sind, damit Sie zwischen den Elementen, wenn Sie diagnostische Untersuchungen durchführen navigieren können.
Wenn Sie im Portal metrischen zählt dargestellt werden, sind diese renormalisiert um tragen die Stichproben, keine Auswirkung auf die Statistiken zu minimieren.

Stichproben verringert den Datenverkehr, hilft Ihnen innerhalb von Kontingenten monatliche Daten beibehalten und hilft Ihnen begrenzungsebene zu vermeiden.

## <a name="in-brief"></a>Kurz gesagt:

* Stichproben behält 1 *n* Datensätze und verwirft den Rest. Sie können beispielsweise 1: 5 Ereignisse, einen Satz werden von 20 % behalten. 
* Stichproben geschieht automatisch auf, wenn eine Anwendung in ASP.NET Web Server apps viele werden, sendet.
* Sie können auch die Stichprobe manuell festlegen entweder im Portal auf die Preisgestaltung (Seite); oder im SDK ASP.NET in der config-Datei auch den Netzwerkverkehr zu verringern.
* Wenn Sie benutzerdefinierte Ereignisse protokollieren und sicherzustellen, dass eine Reihe von Ereignissen beibehalten oder werden verworfen soll, stellen Sie sicher, dass sie den gleichen OperationId Wert haben.
* Der Divisor werden *n* wird in jedem Datensatz in der Eigenschaft gemeldet `itemCount`, die im Feld Suche angezeigt wird, klicken Sie unter der Anforderung Anzeigename "Anzahl" oder "Ereignis zählen". Wenn werden nicht in Operation `itemCount==1`.
* Wenn Sie Analytics Abfragen schreiben, sollten Sie Folgendes [berücksichtigt werden](app-insights-analytics-tour.md#counting-sampled-data). Vor allem einfach zählen von Datensätzen, verwenden Sie `summarize sum(itemCount)`.


## <a name="types-of-sampling"></a>Typen von werden


Es gibt drei Methoden, alternativen werden:

* **Adaptive werden** Lautstärke automatisch die der aus dem SDK in Ihrer app ASP.NET gesendet werden. Standardmäßig aus SDK V 2.0.0-beta3. Derzeit verfügbar für ASP.NET serverseitigen werden nur. 
* **Fixed-Rate werden** verringert die Lautstärke von Ihrem ASP.NET-Server und vom Browser des Benutzers gesendet werden. Die Geschwindigkeit festlegen. Client und Server, werden deren werden synchronisiert, sodass Sie, dass, in die Suche, zwischen verknüpften Seitenansichten und Besprechungsanfragen navigieren können.
* **Aufnahme werden** verringert die Lautstärke der vom Dienst Anwendung Einsichten mit einer Geschwindigkeit, die Sie festlegen beibehalten werden. Es werden Datenverkehr nicht verringern, hilft Ihnen aber innerhalb der monatliche Kontingent beibehalten. 

Wenn adaptiven oder mit fester Rate werden in Betrieb sind, ist die Aufnahme werden deaktiviert.

## <a name="ingestion-sampling"></a>Aufnahme werden

Dieses Formular in der Stichprobe arbeitet an der Stelle, wo die werden aus Ihrem Webserver, Browsern und Geräten den Anwendung Einsichten Endpunkt erreicht. Obwohl es verringern den Datenverkehr werden aus der app nicht den Umfang reduzieren verarbeitet beibehalten (und für belastet) durch die Anwendung Einsichten.

Verwenden Sie diese Art von werden, wenn Ihre app häufig über das monatliche Kontingent wechselt, und Sie nicht die Möglichkeit, verwenden entweder die SDK-basierte Typen von werden müssen. 

Festlegen der Zins werden in der Kontingente und Preise Blade:

![Klicken Sie aus dem Blade Anwendung Übersicht auf Einstellungen, Kontingent, Beispiele, und wählen Sie eine Rate werden, und klicken Sie auf aktualisieren.](./media/app-insights-sampling/04.png)

Wie andere Arten von Stichproben behält der Algorithmus werden verwandte Elemente. Beispielsweise wenn Sie die werden im Feld Suche verborgenen sind, werden Sie Anmeldung für eine bestimmte Ausnahme suchen können. Metrisch zählt, wie z. B. Rate Anfragen und Ausnahme Zins ordnungsgemäß aufbewahrt werden.

Datenpunkte, die von werden verworfen werden, sind nicht verfügbar, in einer beliebigen Anwendung Einsichten-Features wie [Fortlaufender exportieren](app-insights-export-telemetry.md).

Aufnahme werden nicht ausgeführt werden, während der SDK-basierten adaptive oder festverzinslichen werden in Betrieb genommen wurde. Ist die Effektivverzinsung werden bei der SDK weniger als 100 %, wird die Erfassung werden Zins, die Sie festlegen ignoriert.

> [AZURE.WARNING] Klicken Sie auf die Kachel angezeigte Wert zeigt an, den Wert, den für die Erfassung werden festgelegt. Er darstellen nicht den tatsächlichen werden Zins, SDK werden in der Vorgang ist.


## <a name="adaptive-sampling-at-your-web-server"></a>Adaptive werden bei Ihrem Webserver

Adaptive werden für die Anwendung Einsichten SDK für ASP.NET V 2.0.0-beta3 und höher verfügbar ist, und ist standardmäßig aktiviert. 


Adaptive werden wirkt sich auf die Lautstärke von Server Web app an der Anwendung Einsichten-Dienst gesendet werden. Die Lautstärke wird automatisch angepasst, um in eine bestimmte maximale Rate des Datenverkehrs beibehalten.

Es nicht mit betrieben geringe Mengen an werden, also eine app Debuggen oder eine Website mit niedriger Verwendung davon nicht betroffen.

Um das Ziel-Volume zu erreichen, werden einige der generiert werden verworfen. Aber wie andere Arten von Stichproben, behält der Algorithmus werden verwandte Elemente. Beispielsweise wenn Sie die werden im Feld Suche verborgenen sind, werden Sie Anmeldung für eine bestimmte Ausnahme suchen können. 

Metrisch zählt, wie z. B. Rate Anfragen und Ausnahme Zins um die Rate werden berücksichtigen angepasst, sodass sie im Explorer Metrisch ungefähr richtigen Werte angezeigt.

**Aktualisieren Ihres Projekts NuGet** -Paketen auf die neueste Version der *Vorabversion* der Anwendung Einsichten: mit der rechten Maustaste im Projekts im Solution Explorer, wählen Sie NuGet-Pakete verwalten aus, und aktivieren Sie **Vorabversion enthalten** , und suchen Sie nach Microsoft.ApplicationInsights.Web. 

In [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md), können Sie mehrere Parameter in Anpassen der `AdaptiveSamplingTelemetryProcessor` Knoten. Die Zahlen angezeigt werden, die Standardwerte:

* `<MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>`

    Die Ziel-Rate, die für **jeden Server-Host**der Anpassungsalgorithmus Ziel ist. Wenn Ihre Web app für viele Hosts ausgeführt wird, reduzieren Sie diesen Wert zu bleiben innerhalb Ihrer Ziel Zinsfuß Datenverkehr bei der Anwendung Einsichten portal

* `<EvaluationInterval>00:00:15</EvaluationInterval>` 

    Das Intervall, der aktuelle Zinsfuß werden neu ausgewertet wird. Auswertung erfolgt als einen gleitenden Durchschnitt. Möglicherweise möchten Sie dieses Intervall verkürzen ist Ihre werden gestört zu plötzlich Bursts.

* `<SamplingPercentageDecreaseTimeout>00:02:00</SamplingPercentageDecreaseTimeout>`

    Wenn Prozentsatz Wert Änderungen aufnehmen zu können, wie schnell nach wir zulässig sind, werden Prozentsatz erneut zu erfassen weniger Daten zu verringern.

* `<SamplingPercentageIncreaseTimeout>00:15:00</SamplingPercentageIncreaseTimeout>`

    Wenn Prozentsatz Wert Änderungen aufnehmen zu können, wie schnell nach wir dürfen vergrößern werden Prozentsatz erneut aus, um weitere Daten zu erfassen.

* `<MinSamplingPercentage>0.1</MinSamplingPercentage>`

    Was ist den kleinsten Wert, den wir berechtigt sind, um festzulegen, wie Stichproben Prozentsatz variiert.

* `<MaxSamplingPercentage>100.0</MaxSamplingPercentage>`

    Was ist der maximale Wert, den wir berechtigt sind, um festzulegen, als Prozentsatz Stichproben variiert.

* `<MovingAverageRatio>0.25</MovingAverageRatio>` 

    Bei der Berechnung des gleitenden Durchschnitts die Stärke der letzten Wert zugewiesen. Verwenden Sie den Wert gleich oder kleiner als 1. Kleinere Werte nehmen den Algorithmus auf plötzlich vor.

* `<InitialSamplingPercentage>100</InitialSamplingPercentage>`

    Der Wert zugewiesen, wenn nur die app gestartet hat. Verringern Sie nicht folgt, während Sie testen. 

### <a name="alternative-configure-adaptive-sampling-in-code"></a>Alternative: Konfigurieren von adaptive werden in code

Statt anpassen werden in der config-Datei, können Sie den Code verwenden. So können Sie eine Rückruffunktion angeben, die aufgerufen wird, wenn die Rate werden neu ausgewertet wird. Sie konnte, beispielsweise verwenden, um herauszufinden, welche werden Rate verwendet wird.

Entfernen der `AdaptiveSamplingTelemetryProcessor` Knoten aus der config-Datei.



*C#*

```C#

    using Microsoft.ApplicationInsights;
    using Microsoft.ApplicationInsights.Extensibility;
    using Microsoft.ApplicationInsights.WindowsServer.Channel.Implementation;
    using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel;
    ...

    var adaptiveSamplingSettings = new SamplingPercentageEstimatorSettings();

    // Optional: here you can adjust the settings from their defaults.

    var builder = TelemetryConfiguration.Active.TelemetryProcessorChainBuilder;
    
    builder.UseAdaptiveSampling(
         adaptiveSamplingSettings,

        // Callback on rate re-evaluation:
        (double afterSamplingTelemetryItemRatePerSecond,
         double currentSamplingPercentage,
         double newSamplingPercentage,
         bool isSamplingPercentageChanged,
         SamplingPercentageEstimatorSettings s
        ) =>
        {
          if (isSamplingPercentageChanged)
          {
             // Report the sampling rate.
             telemetryClient.TrackMetric("samplingPercentage", newSamplingPercentage);
          }
      });

    // If you have other telemetry processors:
    builder.Use((next) => new AnotherProcessor(next));

    builder.Build();

```

([Erfahren Sie mehr über Prozessoren werden](app-insights-api-filtering-sampling.md#filtering)).


<a name="other-web-pages"></a>
## <a name="sampling-for-web-pages-with-javascript"></a>Werden für Webseiten mit JavaScript

Sie können Webseiten für festverzinslichen werden von jedem Server konfigurieren. 

Wenn Sie [die Webseiten für die Anwendung Einsichten konfigurieren](app-insights-javascript.md), ändern Sie den Ausschnitt, die Sie von der Anwendung Einsichten-Portal zu gelangen. (In ASP.NET apps geht der Codeausschnitt in der Regel in _Layout.cshtml.)  Einfügen einer Zeile wie `samplingPercentage: 10,` vor der Instrumentation Taste:

    <script>
    var appInsights= ... 
    }({ 


    // Value must be 100/N where N is an integer.
    // Valid examples: 50, 25, 20, 10, 5, 1, 0.1, ...
    samplingPercentage: 10, 

    instrumentationKey:...
    }); 
    
    window.appInsights=appInsights; 
    appInsights.trackPageView(); 
    </script> 

Wählen Sie einen Prozentsatz, der in der Nähe 100/N ist, ist N eine ganze Zahl, für den Prozentsatz werden.  Aktuell Stichproben unterstützt keine anderen Werten.

Wenn Sie auch festverzinslichen werden auf dem Server aktivieren, werden die Clients und Server synchronisiert, so, dass, in die Suche, zwischen verknüpften Seitenansichten und Besprechungsanfragen navigieren können.


## <a name="fixed-rate-sampling-for-aspnet-web-sites"></a>Festverzinslichen werden für ASP.NET-Websites

Festverzinslichen werden reduziert den Datenverkehr auf Ihrem Webserver und Webbrowser gesendet. Im Gegensatz zu adaptive werden, wird es werden, um eine feste Rate entschieden, die von Ihnen verringert. Er synchronisiert darüber hinaus Client und Server werden, sodass die zugehörigen Elemente – beispielsweise beibehalten werden, sodass, wenn Sie eine Ansicht im Feld Suche betrachten, deren verwandte Anforderung finden können.

Der Stichprobe Algorithmus behält verwandte Elemente. Für jede HTTP-Anforderung sind Ereignis, sie und ihre verwandten Ereignisse entweder verworfen oder übertragen. 

Im Explorer Metrik sind Sätzen wie Anfrage und Ausnahme zählt mit dem Faktor für die Stichprobe Rate zukommen lassen multipliziert, damit sie etwa korrekt sind.

1. **Aktualisieren Ihres Projekts NuGet-Pakete** , auf die neueste Version der *Vorabversion* der Anwendung Einsichten. Mit der rechten Maustaste im Projekts im Solution Explorer, wählen Sie NuGet-Pakete verwalten aus, und aktivieren Sie **Vorabversion enthalten** , und suchen Sie nach Microsoft.ApplicationInsights.Web. 

2. **Deaktivieren Sie adaptive werden**: In [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md), entfernen oder kommentieren der `AdaptiveSamplingTelemetryProcessor` Knoten.

    ```xml

    <TelemetryProcessors>
    <!-- Disabled adaptive sampling:
      <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.AdaptiveSamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
        <MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>
      </Add>
    -->
    

    ```

2. **Aktivieren Sie das Modul festverzinslichen werden.** Fügen Sie dieser Ausschnitt [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md)hinzu:

    ```XML

    <TelemetryProcessors>
     <Add  Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.SamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">

      <!-- Set a percentage close to 100/N where N is an integer. -->
     <!-- E.g. 50 (=100/2), 33.33 (=100/3), 25 (=100/4), 20, 1 (=100/100), 0.1 (=100/1000) -->
      <SamplingPercentage>10</SamplingPercentage>
      </Add>
    </TelemetryProcessors>

    ```

> [AZURE.NOTE] Wählen Sie einen Prozentsatz, der in der Nähe 100/N ist, ist N eine ganze Zahl, für den Prozentsatz werden.  Aktuell Stichproben unterstützt keine anderen Werten.



### <a name="alternative-enable-fixed-rate-sampling-in-your-server-code"></a>Alternative: Aktivieren Sie festverzinslichen werden in Ihrem Servercode


Statt den Parameter werden in der config-Datei festlegen, können Sie Code verwenden. 

*C#*

```C#

    using Microsoft.ApplicationInsights.Extensibility;
    using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel;
    ...

    var builder = TelemetryConfiguration.Active.GetTelemetryProcessorChainBuilder();
    builder.UseSampling(10.0); // percentage

    // If you have other telemetry processors:
    builder.Use((next) => new AnotherProcessor(next));

    builder.Build();

```

([Erfahren Sie mehr über Prozessoren werden](app-insights-api-filtering-sampling.md#filtering)).


## <a name="when-to-use-sampling"></a>Wann verwendet werden?

Adaptive Stichproben wird automatisch aktiviert, wenn Sie die ASP.NET SDK Version 2.0.0-beta3 verwenden oder höher. Unabhängig davon, welche SDK-Version Sie verwenden, können Sie die Erfassung werden (bei unserem Server).

Sie benötigen keine werden für die meisten kleine und mittlere Applikationen. Sammeln von Daten auf alle Benutzeraktivitäten werden die am häufigsten hilfreiche Diagnoseinformationen und die genauesten Statistik abgerufen. 

 
Hauptfenster werden folgende Vorteile:

* Anwendung Einsichten Dienst Tropfen ("Drosselungen") vorkommenden Datenpunkte, wenn Ihre app einen sehr hohen Anteil an werden kurz gesagt sendet Zeitintervall an. 
* In das [Kontingent](app-insights-pricing.md) von Datenpunkten für Ihre Preisgestaltung Ebene beibehalten. 
* Um den Netzwerkverkehr aus der Sammlung werden zu reduzieren. 

### <a name="which-type-of-sampling-should-i-use"></a>Welchen Typ von werden sollte ich verwenden?


**Verwenden Sie die Erfassung Stichproben, wenn:**

* Sie gehen Sie häufig durch die monatlichen maximal werden.
* Sie verwenden eine Version von SDK, der werden – beispielsweise unterstützt, die Java SDK oder ASP.NET-Versionen früher als 2.
* Sie sind viele werden aus Ihrer Benutzer für den Webbrowser erhalten.

**Verwenden Sie festverzinslichen Stichproben, wenn:**

* Verwenden Sie die Anwendung Einsichten SDK für ASP.NET Web Services Version 2.0.0 oder höher, und
* Sie möchten synchronisierten Stichproben zwischen Client und Server, damit, wenn Sie Ereignisse im [Suchfeld](app-insights-diagnostic-search.md)untersuchen sind, Sie zwischen verwandten Ereignisse auf dem Client und Server, z. B. Seitenansichten und HTTP-Anfragen navigieren können.
* Sie können für Ihre app des Prozentsatzes geeigneten Stichprobe vertrauen. Es sollte hoch genug eingestellt und genaue Kennzahlen erhalten, aber unterhalb der Zins, die Ihr Kontingent Preisgestaltung und Drosselung Grenzen überschreitet. 


**Verwenden Sie adaptive werden:**

Wir empfehlen andernfalls adaptive werden. Dies ist standardmäßig aktiviert, in dem ASP.NET Server SDK, Version 2.0.0-beta3 oder höher. Es reduzieren nicht bis zu einer bestimmten minimalen Zins Netzwerkverkehr, damit sie eine Website geringer Nutzung davon betroffen werden.


## <a name="how-do-i-know-whether-sampling-is-in-operation"></a>Wie kann ich feststellen, ob werden in Vorgang ist?

Verwenden Sie zum Ermitteln der tatsächlichen werden Zins unabhängig davon, wo sie angewendet wurde, eine [Abfrage Analytics](app-insights-analytics.md) wie folgt aus:

    requests | where timestamp > ago(1d)
  	| summarize 100/avg(itemCount) by bin(timestamp, 1h) 
  	| render areachart 

In den einzelnen beibehalten Datensatz `itemCount` gibt die Anzahl der ursprünglichen Datensätze, die ihn darstellt, gleich 1 + die Anzahl der vorherigen verworfen Datensätze. 


## <a name="how-does-sampling-work"></a>Wie funktioniert die werden?

Sind ein Feature des SDK in ASP.NET-Versionen von 2.0.0 oder höher festverzinslichen und adaptive werden. Aufnahme werden ist ein Feature des Diensts Anwendung Einsichten und kann in Vorgang sein, wenn das SDK werden nicht ausführt. 

Der Algorithmus werden beschließt, welche Elemente werden ablegen, und welche beibehalten (, ob er im SDK oder in der Anwendung Einsichten Dienst ist). Die Entscheidung werden basiert auf mehrere Regeln, die alle Datenpunkte zusammenhängende erhalten bleibt, Verwalten von einer Diagnoseprotokollen berichtet in Anwendung Einblicken, die einige und sogar mit einer reduzierten Datenmenge zuverlässigen ist beibehalten werden sollen. Beispielsweise, wenn Ihre app für eine fehlgeschlagene Anforderung werden weitere Elemente (z. B. Ausnahme und Spuren aus dieser Anforderung angemeldet) sendet, teilt werden nicht diese Anforderung und andere werden. Es behält oder legt diese überhaupt ab. Daher, wenn Sie die Details der Anforderung in Anwendung Einsichten betrachten, können Sie immer die Anforderung zusammen mit den zugehörigen verknüpft werden Elemente sehen. 

Für Applikationen, die "Benutzer" definieren (d. h., am häufigsten verwendete Webanwendungen), die Entscheidung werden basiert auf dem Hash der Benutzer-Id, was bedeutet, dass alle werden für einen bestimmten Benutzer beibehalten oder gelöscht wird. Für die Typen von Applications, die Benutzer (z. B. Webdienste) definieren nicht die Entscheidung werden die Vorgangs-Id der Anfrage basiert. Für die werden Elemente, die weder Benutzer noch Vorgang-Id (zum Beispiel werden Elemente aus asynchrone Threads ohne http-Kontext gemeldet) festgelegt haben erfasst werden schließlich einfach einen Prozentsatz der werden Elemente jedes Typs. 

Werden Sie zurück zur Vorführung passt der Anwendung Einsichten Dienst die Metrik um den gleichen Prozentsatz werden, der zum Zeitpunkt der Websitesammlung, für die fehlenden Datenpunkte zukommen lassen verwendet wurde. Daher werden beim Betrachten der werden in der Anwendung Einblicken, die Benutzer statistisch richtige Näherungswerten angezeigt, die sehr ähnlich reelle Zahlen sind.

Die Genauigkeit der die Annäherung in folgender hängt weitgehend den Prozentsatz konfiguriert werden. Darüber hinaus erhöht die Genauigkeit für Applikationen, die eine große Anzahl von in der Regel ähnliche Anfragen zahlreiche Benutzer behandelt. Andererseits, wird für Applikationen, die nicht mit einer erheblichen Auslastung arbeiten, werden nicht benötigt diese Applications alle ihre werden in der Regel senden können, während innerhalb des Kontingents, ohne Datenverlust aus begrenzungsebene beibehalten. 

Beachten Sie, dass die Anwendung Einsichten hören nicht Metrik und Sitzungen werden Typen seit für diese Dateitypen, Verringerung der Genauigkeit hochgradig unerwünscht sein kann. 

### <a name="adaptive-sampling"></a>Adaptive werden

Adaptive werden addiert eine Komponente, die überwacht die aktuelle Rate der Übertragung aus dem SDK und den Prozentsatz werden ausprobieren, um innerhalb der maximalen Geschwindigkeit Ziel bleiben passt. Die Anpassung wird in regelmäßigen Abständen neu berechnet, und auf einen gleitenden Durchschnitt des ausgehenden Übertragung Satzes basiert.

## <a name="sampling-and-the-javascript-sdk"></a>Werden und das JavaScript-SDK

Clientseitige (JavaScript) SDK beteiligt festverzinslichen werden in Verbindung mit dem serverseitigen SDK aus. Die instrumentierten Seiten werden nur clientseitige werden dieselben Benutzer senden für die der serverseitige ihre Entscheidung "Beispiel in" gebucht Diese Logik soll Integrität der Benutzer Sitzung über Client und Server-Seiten verwalten. Daher können Sie von jedem beliebigen Element bestimmten werden in der Anwendung Einsichten alle anderen werden Elemente, die für diesen Benutzer oder Sitzung suchen. 

*Meine Client- und serverseitigen werden Beispielen nicht aufeinander abgestimmte, wie Sie über beschreiben.*

* Stellen Sie sicher, dass Sie festverzinslichen werden sowohl auf dem Server und Client aktiviert.
* Stellen Sie sicher, dass die SDK Version 2.0 oder höher.
* Überprüfen Sie, dass Sie den gleichen werden Prozentsatz Client und Server festlegen.


## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen 

*Warum eine einfache "sammeln X Prozent jedes Typs werden" Stichproben nicht zur Verfügung?*

 *  Während dieser Ansatz Stichproben mit sehr hoher Genauigkeit in metrischen Näherungswerten zur Verfügung stellen möchten, würden sie Möglichkeit, zu koordinieren diagnostische Daten pro Benutzer, die Sitzung und Anforderung an, die für Diagnose kritische ist aufgehoben. Stichprobe daher funktioniert besser mit "alle werden Elemente für X % der app-Benutzer sammeln" oder "alle werden für X % der app-Anfragen sammeln" Logik. Für die Anfragen (z. B. Hintergrund asynchrone Verarbeitung) zugeordnet werden Elemente, ist Herbst wieder zu "sammeln X Prozent aller Elemente für jeden werden." 

*Kann der Prozentsatz werden im Zeitverlauf ändern?*

 * Ja, wechselt adaptive werden schrittweise den Stichproben Prozentsatz, der ausgehend von der aktuell beobachteten Lautstärke der werden.

 

*Wenn ich festverzinslichen Stichproben verwenden, wie erkenne ich, welche werden Prozentsatz funktionieren die beste Wahl für meine app?*

* Eine Möglichkeit ist zunächst adaptive werden, finden Sie heraus, was bewerten sie gleicht auf (siehe die oben genannte Frage), und wechseln Sie dann zur festverzinslichen Stichprobe mit diesem Satz. 

    Andernfalls müssen Sie raten. Ihre aktuelle werden Verwendung in AI analysieren, alle begrenzungsebene erfolgt die Aktionen beobachten und Schätzen der Lautstärke der zusammengestellten werden. Diese drei Eingaben, zusammen mit der ausgewählten Preisgestaltung Ebene vorschlagen, wie viel Sie vielleicht verringern die Lautstärke der zusammengestellten werden möchten. Ein Erhöhen der Anzahl der Benutzer oder einige andere UMSCHALT in der Lautstärke werden möglicherweise die geschätzte ungültig.

*Was geschieht bei Konfigurieren von Stichproben Prozentsatz zu niedrig?*

* Sehr niedrig werden Prozentsatz (over-aggressive werden) reduziert die Genauigkeit der der Näherungswerten, bei der Anwendung Einsichten versucht, die Visualisierung von Daten für die Daten Lautstärke Verringerung angepasst. Darüber hinaus möglicherweise diagnostic Erfahrung beeinträchtigt werden als Teil der Anfragen selten defekte oder langsamen, entnommen werden kann.

*Was geschieht bei Konfigurieren von Stichproben Prozentsatz zu hoch?*

* Konfigurieren zu hoch werden Prozentsatz (nicht anspruchsvollen genug) angegeben, ergibt sich eine nicht genügend Verringerung der Lautstärke der zusammengestellten werden. Treten möglicherweise weiterhin im Zusammenhang mit begrenzungsebene werden Daten verloren gehen, und die Kosten der Anwendung Einsichten Verwendung möglicherweise größer als aufgrund von veralteten Gebühren geplant.

*Auf welche Plattformen können Stichproben werden verwendet?*

* Das SDK werden nicht ausführt, kann Aufnahme werden automatisch für alle werden über ein bestimmtes Volumen auftreten. Dies würde beispielsweise funktioniert, wenn Ihre app einen Java-Server verwendet, oder wenn Sie eine ältere Version des ASP.NET SDK verwenden.

* Wenn Sie ASP.NET SDK, Version 2.0.0 verwenden und oberhalb (gehostet in Azure oder auf Ihrem eigenen Server), erhalten Sie standardmäßig Stichproben adaptive, aber Sie können in festverzinslichen wechseln, wie zuvor beschrieben. Mit festverzinslichen werden, werden im Browser SDK für die Stichprobe verwandten Ereignisse automatisch synchronisiert. 

*Es gibt bestimmte seltene Ereignisse, die ich immer anzeigen möchten. Wie erhalte ich sie ältere das Modul werden?*

 * Initialisierung eine separate Instanz von TelemetryClient mit einer neuen TelemetryConfiguration (nicht die Standardeinstellung aktiven Registerkarten). Verwenden Sie, um Ihre seltene Ereignisse zu senden.



## <a name="next-steps"></a>Nächste Schritte

* [Filtern](app-insights-api-filtering-sampling.md) können mehr strengen Kontrolle über was Ihre SDK sendet bereitstellen.

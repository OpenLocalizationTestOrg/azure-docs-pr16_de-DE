<properties 
    pageTitle="Anwendung Einsichten Datenmodell" 
    description="Beschreibt die Eigenschaften von fortlaufender exportieren in JSON exportiert und als Filter verwendet." 
    services="application-insights" 
    documentationCenter=""
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="03/21/2016" 
    ms.author="awills"/>

# <a name="application-insights-export-data-model"></a>Anwendung Einsichten exportieren-Datenmodell

Diese Tabelle listet die Eigenschaften von der [Anwendung Einsichten](app-insights-overview.md) SDKs Portal gesendet werden. Sehen Sie diese Eigenschaften in der Ausgabe von [Fortlaufender exportieren](app-insights-export-telemetry.md)Daten ein.
Außerdem werden in Eigenschaftenfiltern in [Metrisch-Explorer](app-insights-metrics-explorer.md) , und [Suchen Sie Diagnostic](app-insights-diagnostic-search.md)angezeigt.

Punkte beachten:

* `[0]`in diesen Tabellen kennzeichnet einen Punkt in den Pfad, in dem Sie einen Index einfügen müssen; Es ist nicht immer 0.
* Zeit Dauer einer Mikrosekunde, also 10000000 Uhrzeitdaten sind == 1 Sekunde.
* Datums- und Uhrzeitangaben UTC sind und in der ISO-Format angegeben sind`yyyy-MM-DDThh:mm:ss.sssZ`

Es gibt mehrere [Beispiele](app-insights-export-telemetry.md#code-samples) , die veranschaulichen, wie sie verwendet werden.



## <a name="example"></a>Beispiel

    // A server report about an HTTP request
    {
    "request": [ 
      {
        "urlData": { // derived from 'url'
          "host": "contoso.org",
          "base": "/",
          "hashTag": "" 
        },
        "responseCode": 200, // Sent to client
        "success": true, // Default == responseCode<400
        // Request id becomes the operation id of child events 
        "id": "fCOhCdCnZ9I=",  
        "name": "GET Home/Index",
        "count": 1, // 100% / sampling rate
        "durationMetric": {
          "value": 1046804.0, // 10000000 == 1 second
          // Currently the following fields are redundant:
          "count": 1.0,
          "min": 1046804.0,
          "max": 1046804.0,
          "stdDev": 0.0,
          "sampledValue": 1046804.0
        },
        "url": "/"
      }
    ],
    "internal": {
      "data": {
        "id": "7f156650-ef4c-11e5-8453-3f984b167d05",
        "documentVersion": "1.61"
      }
    },
    "context": {
      "device": { // client browser
        "type": "PC",
        "screenResolution": { },
        "roleInstance": "WFWEB14B.fabrikam.net"
      },
      "application": { },
      "location": { // derived from client ip
        "continent": "North America",
        "country": "United States",
        // last octagon is anonymized to 0 at portal:
        "clientip": "168.62.177.0", 
        "province": "",
        "city": ""
      },
      "data": {
        "isSynthetic": true, // we identified source as a bot
        // percentage of generated data sent to portal:
        "samplingRate": 100.0, 
        "eventTime": "2016-03-21T10:05:45.7334717Z" // UTC
      },
      "user": {
        "isAuthenticated": false,
        "anonId": "us-tx-sn1-azr", // bot agent id
        "anonAcquisitionDate": "0001-01-01T00:00:00Z",
        "authAcquisitionDate": "0001-01-01T00:00:00Z",
        "accountAcquisitionDate": "0001-01-01T00:00:00Z"
      },
      "operation": {
        "id": "fCOhCdCnZ9I=",
        "parentId": "fCOhCdCnZ9I=",
        "name": "GET Home/Index"
      },
      "cloud": { },
      "serverDevice": { },
      "custom": { // set by custom fields of track calls
        "dimensions": [ ],
        "metrics": [ ]
      },
      "session": {
        "id": "65504c10-44a6-489e-b9dc-94184eb00d86",
        "isFirst": true
      }
    }
  }




## <a name="context"></a>Kontextmenü

Einen Kontextabschnitt werden alle Typen von telemetrieprotokoll beizufügen. Nicht alle dieser Felder werden mit jeder Datenpunkt übertragen.



|Pfad|Typ|Notizen|
|---|---|---|
| Context.Custom.Dimensions [0]  | Objekt]  | Schlüssel-Wert-Zeichenfolgenpaare von benutzerdefinierten Eigenschaften, die Parameter festgelegt. Maximale Länge 100, maximale Länge von Werten 1024. Mehr als 100 eindeutige Werte, die Eigenschaft durchsucht werden kann, aber nicht Segmentierungsalgorithmus verwendet werden. Max 200 Tasten pro Ikey.  |
| Context.Custom.Metrics [0]  | Objekt]  | Schlüssel-Wert-Paare festlegen, indem Sie benutzerdefinierte Maße Parameter und TrackMetrics. Maximale Länge des Schlüssels 100 Werte möglicherweise numerisch sein. |
| context.data.eventTime | Zeichenfolge | UTC |
| context.data.isSynthetic | Boolesch | Anforderung wird, stammen aus einem Bot oder Web Test angezeigt. |
| context.data.samplingRate | Zahl | Prozentsatz der vom SDK, die Portal gesendet wird generiert werden. Bereich 0,0-100,0.|
| Context.Device | Objekt | Client-Gerät |
| Context.Device.Browser | Zeichenfolge | IE, Chrome... |
| context.device.browserVersion | Zeichenfolge | Chrome: 48,0... |
| context.device.deviceModel | Zeichenfolge | |
| context.device.deviceName | Zeichenfolge | |
| Context.Device.ID | Zeichenfolge | |
| Context.Device.Locale | Zeichenfolge | En-GB, de-DE... |
| Context.Device.Network | Zeichenfolge | |
| context.device.oemName | Zeichenfolge | |
| context.device.osVersion | Zeichenfolge | Host-Betriebssystem |
| context.device.roleInstance | Zeichenfolge | Server-Host-ID |
| context.device.roleName | Zeichenfolge | |
| Context.Device.Type | Zeichenfolge | PC Browser... |
| Context.Location | Objekt | Abgeleitet von Clientip. |
| Context.Location.City | Zeichenfolge | Abgeleitet von Clientip, falls bekannt  |
| Context.Location.ClientIP | Zeichenfolge | Letzte Achteck ist 0 anonymisiert. |
| Context.Location.Continent | Zeichenfolge | |
| Context.Location.Country | Zeichenfolge | |
| Context.Location.Province | Zeichenfolge | Bundesland oder Kanton |
| Context.Operation.ID | Zeichenfolge | Elemente, die die gleichen Vorgang-Id haben, werden als verwandte Elemente im Portal angezeigt. In der Regel die Id anfordern. |
| Context.Operation.Name | Zeichenfolge | URL oder Anforderung |
| context.operation.parentId | Zeichenfolge | Ermöglicht die geschachtelte verwandte Elemente. |
| Context.Session.ID | Zeichenfolge | ID des eine Gruppe von Vorgängen aus derselben Quelle. Ein Zeitraum von 30 Minuten ohne einen Vorgang zeigt das Ende einer Sitzung. |
| context.session.isFirst | Boolesch | |
| context.user.accountAcquisitionDate | Zeichenfolge | |
| context.user.anonAcquisitionDate | Zeichenfolge | |
| context.user.anonId | Zeichenfolge | |
| context.user.authAcquisitionDate | Zeichenfolge | [Authentifizierte Benutzer](app-insights-api-custom-events-metrics.md#authenticated-users) |
| context.user.isAuthenticated | Boolesch | |
| internal.data.documentVersion | Zeichenfolge | |
| Internal.Data.ID | Zeichenfolge | |



## <a name="events"></a>Ereignisse

Benutzerdefinierte Ereignisse von [TrackEvent()](app-insights-api-custom-events-metrics.md#track-event)generiert. 


|Pfad|Typ|Notizen|
|---|---|---|
| Ereignisanzahl [0] | ganze Zahl | 100 / ([Stichprobe](app-insights-sampling.md) Rate). Beispiel 4 =&gt; 25 %. |
| Name des Ereignisses [0] | Zeichenfolge | Name des Ereignisses.  Die Maximallänge 250. |
| Ereignis [0]-url | Zeichenfolge | |
| urlData.base Ereignis [0] | Zeichenfolge | |
| urlData.host Ereignis [0] | Zeichenfolge | |

## <a name="exceptions"></a>Ausnahmen

Berichte [Ausnahmen](app-insights-asp-net-exceptions.md) auf dem Server und im Browser. 


|Pfad|Typ|Notizen|
|---|---|---|
| BasicException [0] assembly | Zeichenfolge | |
| Anzahl der BasicException [0] | ganze Zahl | 100 / ([Stichprobe](app-insights-sampling.md) Rate). Beispiel 4 =&gt; 25 %. |
| ExceptionGroup BasicException [0] | Zeichenfolge | |
| ExceptionType BasicException [0] | Zeichenfolge | |Zeichenfolge | |
| FailedUserCodeMethod BasicException [0] | Zeichenfolge | |
| FailedUserCodeAssembly BasicException [0] | Zeichenfolge | |
| HandledAt BasicException [0] | Zeichenfolge | |
| HasFullStack BasicException [0] | Boolesch | |
| BasicException [0]-id | Zeichenfolge | |
| BasicException [0]-Methode | Zeichenfolge | |
| Nachricht BasicException [0] | Zeichenfolge | Die Ausnahmemeldung. Die Maximallänge 10k.|
| OuterExceptionMessage BasicException [0] | Zeichenfolge | |
| OuterExceptionThrownAtAssembly BasicException [0] | Zeichenfolge | |
| OuterExceptionThrownAtMethod BasicException [0] | Zeichenfolge | |
| OuterExceptionType BasicException [0] | Zeichenfolge | |
| OuterId BasicException [0] | Zeichenfolge | |
| BasicException [0] ParsedStack [0] assembly | Zeichenfolge | |
| BasicException [0] ParsedStack [0] Dateiname | Zeichenfolge | |
| BasicException [0] ParsedStack [0] Ebene | ganze Zahl | |
| BasicException [0] ParsedStack [0] Linie | ganze Zahl | |
| BasicException [0] ParsedStack [0]-Methode | Zeichenfolge | |
| Stapel BasicException [0] | Zeichenfolge | Die Maximallänge 10k|
| TypeName BasicException [0] | Zeichenfolge | |



## <a name="trace-messages"></a>Verfolgen von Nachrichten

Gesendet, indem Sie [TrackTrace](app-insights-api-custom-events-metrics.md#track-trace)und der [Protokollierung Netzwerkadapter](app-insights-asp-net-trace-logs.md).


|Pfad|Typ|Notizen|
|---|---|---|
| Nachricht [0] loggerName | Zeichenfolge ||
| [0] Nachrichtenparameter | Zeichenfolge ||
| unformatierte Nachricht [0] | Zeichenfolge | Die Nachricht, maximale Länge 10k. |
| Nachricht [0] severityLevel | Zeichenfolge | |



## <a name="remote-dependency"></a>Remote Abhängigkeit

Von TrackDependency gesendet. Verwendet, um die Leistung von Berichten und die Verwendung der [Anrufe an Abhängigkeiten](app-insights-asp-net-dependencies.md) auf dem Server, und AJAX Anrufe im Browser.

|Pfad|Typ|Notizen|
|---|---|---|
| asynchrone RemoteDependency [0] | Boolesch | |
| BaseName RemoteDependency [0] | Zeichenfolge |  |
| RemoteDependency [0] commandName | Zeichenfolge | Beispielsweise "Start/Index" |
| Anzahl der RemoteDependency [0] | ganze Zahl | 100 / ([Stichprobe](app-insights-sampling.md) Rate). Beispiel 4 =&gt; 25 %. |
| DependencyTypeName RemoteDependency [0] | Zeichenfolge | HTTP SQL... |
| durationMetric.value RemoteDependency [0] | Zahl | Zeit bis zum Abschluss der Antwort von Abhängigkeit Anruf aus. |
| RemoteDependency [0]-id | Zeichenfolge | |
| Namen RemoteDependency [0] | Zeichenfolge | URL. Die Maximallänge 250.|
| RemoteDependency [0] "ResultCode" | Zeichenfolge | in Abhängigkeit von HTTP |
| RemoteDependency [0] Erfolg | Boolesch | |
| Typ RemoteDependency [0] | Zeichenfolge | HTTP Sql... |
| RemoteDependency [0]-url | Zeichenfolge |  Die Maximallänge 2000 |
| urlData.base RemoteDependency [0] | Zeichenfolge | Die Maximallänge 2000 |
| urlData.hashTag RemoteDependency [0] | Zeichenfolge | |
| urlData.host RemoteDependency [0] | Zeichenfolge | Die Maximallänge 200 |


## <a name="requests"></a>Besprechungsanfragen

Von [TrackRequest](app-insights-api-custom-events-metrics.md#track-request)gesendet. Der standard-Module verwenden Sie diese Berichte Server der Zeit, gemessen auf dem Server. 


|Pfad|Typ|Notizen|
|---|---|---|
| Anzahl der Anfrage [0] | ganze Zahl | 100 / ([Stichprobe](app-insights-sampling.md) Rate). Beispiel: 4 =&gt; 25 %. |
| [0] durationMetric.value anfordern | Zahl | Zeit von Anforderung, Antwort empfangen. 1e7 == 1 s |
| Anforderung [0]-id | Zeichenfolge | Vorgangs-id |
| Name der Anfrage [0] | Zeichenfolge | GET-Beitrag + Basis-Url.  Die Maximallänge 250 |
| [0] ResponseCode anfordern | ganze Zahl | HTTP-Antwort an den Client gesendet werden |
| [0] Erfolg anfordern | Boolesch | Standard == (ResponseCode &lt; 400) |
| Url der Anforderung [0] | Zeichenfolge | Host nicht eingeschlossen |
| [0] urlData.base anfordern | Zeichenfolge | |
| [0] urlData.hashTag anfordern | Zeichenfolge |  |
| [0] urlData.host anfordern | Zeichenfolge | |


## <a name="page-view-performance"></a>Datenzugriffsseiten-Ansicht Leistung

Vom Browser gesendet. Gibt die Zeit zum Verarbeiten von einer Seite Benutzers Einleiten der Anforderung abgeschlossen (ohne asynchrone AJAX-Aufrufe) angezeigt werden.

Kontextwerte anzeigen Client-Betriebssystem und Browserversion. 


|Pfad|Typ|Notizen|
|---|---|---|
| clientProcess.value ClientPerformance [0] | ganze Zahl | Zeit von Ende den HTML-Code auf die Anzeige von der Seite empfangen. |
| Namen ClientPerformance [0] | Zeichenfolge | |
| networkConnection.value ClientPerformance [0] | ganze Zahl | Zeit in einem Netzwerk herzustellen. |
| receiveRequest.value ClientPerformance [0] | ganze Zahl | Zeit von Ende Senden der Anforderung an den HTML-Code in Antworten empfangen. |
| sendRequest.value ClientPerformance [0] | ganze Zahl | Anzeigedauer aus zum Senden der HTTP-Anforderung geöffnet. |
| total.value ClientPerformance [0] | ganze Zahl | Zeit zum Senden der Anforderung zum Anzeigen der Seite nicht bei jeder. |
| ClientPerformance [0]-url | Zeichenfolge | URL dieser Anforderung |
| urlData.base ClientPerformance [0] | Zeichenfolge | |
| urlData.hashTag ClientPerformance [0] | Zeichenfolge | |
| urlData.host ClientPerformance [0] | Zeichenfolge | |
| urlData.protocol ClientPerformance [0] | Zeichenfolge | |

## <a name="page-views"></a>Seitenansichten

Gesendet durch trackPageView() oder [stopTrackPage](app-insights-api-custom-events-metrics.md#page-view)

|Pfad|Typ|Notizen|
|---|---|---|
| Ansichtenanzahl [0] | ganze Zahl | 100 / ([Stichprobe](app-insights-sampling.md) Rate). Beispiel 4 =&gt; 25 %. |
| [0] durationMetric.value anzeigen | ganze Zahl | Legen Sie optional in trackPageView() oder startTrackPage() - Wert stopTrackPage(). Nicht identisch mit ClientPerformance Werte. |
| Ansichtsname [0] | Zeichenfolge | Seitentitel.  Die Maximallänge 250 |
| [0] Url anzeigen | Zeichenfolge | |
| [0] urlData.base anzeigen | Zeichenfolge | |
| [0] urlData.hashTag anzeigen | Zeichenfolge | |
| [0] urlData.host anzeigen | Zeichenfolge | |



## <a name="availability"></a>Verfügbarkeit

[Verfügbarkeit von Webtests](app-insights-monitor-web-app-availability.md)-Berichte.

|Pfad|Typ|Notizen|
|---|---|---|
| Verfügbarkeit [0] availabilityMetric.name | Zeichenfolge | Verfügbarkeit |
| Verfügbarkeit [0] availabilityMetric.value | Zahl |1.0 oder 0,0 |
| Anzahl der Verfügbarkeit [0] | ganze Zahl | 100 / ([Stichprobe](app-insights-sampling.md) Rate). Beispiel 4 =&gt; 25 %. |
| Verfügbarkeit [0] dataSizeMetric.name | Zeichenfolge | |
| Verfügbarkeit [0] dataSizeMetric.value | ganze Zahl | |
| Verfügbarkeit [0] durationMetric.name | Zeichenfolge | |
| Verfügbarkeit [0] durationMetric.value | Zahl | Dauer des Tests. 1e7 == 1 s |
| Verfügbarkeit [0] Nachricht | Zeichenfolge | Fehler bei der Diagnose |
| Verfügbarkeit [0] Ergebnis | Zeichenfolge | Bestanden wurden oder |
| Verfügbarkeit [0] runLocation | Zeichenfolge | Geo Quellcodes http gefordertes |
| Verfügbarkeit [0] testName | Zeichenfolge | |
| Verfügbarkeit [0] testRunId | Zeichenfolge | |
| Verfügbarkeit [0] testTimestamp | Zeichenfolge | |




## <a name="metrics"></a>Kennzahlen

Durch TrackMetric() generiert.

Der metrische Wert befindet sich im context.custom.metrics[0]

Beispiel:

    {
     "metric": [ ],
     "context": {
     ...
     "custom": {
        "dimensions": [
          { "ProcessId": "4068" }
        ],
        "metrics": [
          {
            "dispatchRate": {
              "value": 0.001295,
              "count": 1.0,
              "min": 0.001295,
              "max": 0.001295,
              "stdDev": 0.0,
              "sampledValue": 0.001295,
              "sum": 0.001295
            }
          }
         } ] }
    }

## <a name="about-metric-values"></a>Informationen zu metrischen Werte

Metrischen Werte, die sowohl in metrischen Berichten und an anderen Stellen, werden mit einer standard Objektstruktur gemeldet. Beispiel:

      "durationMetric": {
        "name": "contoso.org",
        "type": "Aggregation",
        "value": 468.71603053650279,
        "count": 1.0,
        "min": 468.71603053650279,
        "max": 468.71603053650279,
        "stdDev": 0.0,
        "sampledValue": 468.71603053650279
      }

Aktuell – durch diese ändert sich möglicherweise in der Zukunft – alle Werte aus der standard SDK Module gemeldet `count==1` und nur die `name` und `value` Felder sind hilfreich. Wenn Sie Ihre eigenen TrackMetric Anrufe schreiben ist der einzige Fall, in dem sie unterschiedlich sein sollte, wäre in dem Sie die anderen Parameter festgelegt. 

Der Zweck der anderen Felder ist Kennzahlen im SDK, um den Datenverkehr-Portal zu verringern aggregiert sein dürfen. Beispielsweise könnten Sie mehrere aufeinanderfolgende Werte Mittelwert der vor dem Senden jeder metrischen Bericht. Klicken Sie dann würden Sie berechnen der min, Max, die Standardabweichung und Aggregatwert (Summe oder Mittelwert) und zählen der Anzahl der Werte dargestellt werden, indem Sie den Bericht festlegen. 

In der obigen Tabelle haben wir die Anzahl der Felder kaum verwendet, min, Max, StdDev und SampledValue weggelassen.

Statt vorab aggregieren Kennzahlen können Sie [werden](app-insights-sampling.md) , wenn Sie zum Verringern der Lautstärke werden müssen.


### <a name="durations"></a>Dauer

Mit Ausnahme der anders angegeben werden Dauer einer Mikrosekunde, Uhrzeitdaten dargestellt, sodass 10000000.0 1 Sekunde bedeutet.



## <a name="see-also"></a>Siehe auch

* [Anwendung Einsichten](app-insights-overview.md) 
* [Fortlaufender exportieren](app-insights-export-telemetry.md)
* [Codebeispielen](app-insights-export-telemetry.md#code-samples)



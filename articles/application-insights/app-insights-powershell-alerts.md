<properties
    pageTitle="Verwenden Sie zum Festlegen von Benachrichtigungen in Anwendung Einsichten Powershell"
    description="Konfiguration von Anwendung Einsichten abzurufenden e-Mails über metrischen Änderungen zu automatisieren."
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
    ms.date="02/19/2016"
    ms.author="awills"/>

# <a name="use-powershell-to-set-alerts-in-application-insights"></a>Verwenden Sie zum Festlegen von Benachrichtigungen in Anwendung Einsichten PowerShell

Sie können die Konfiguration von [Benachrichtigungen](app-insights-alerts.md) in [Visual Studio Anwendung Einsichten](app-insights-overview.md)automatisieren.

Darüber hinaus können Sie [festlegen Webhooks, um Ihre Antwort auf eine Warnung zu automatisieren](../monitoring-and-diagnostics/insights-webhooks-alerts.md).

## <a name="one-time-setup"></a>Einmalige Einrichtung

Wenn Sie mit Ihrem Azure-Abonnement vor dem PowerShell verwendet haben:

Installieren Sie das Azure Powershell-Modul auf dem Computer, in dem die Skripts ausgeführt werden soll.

 * Installieren von [Microsoft Web Platform Installer (v5 oder höher)](http://www.microsoft.com/web/downloads/platform.aspx).
 * Verwenden sie zum Installieren von Microsoft Azure Powershell


## <a name="connect-to-azure"></a>Herstellen einer Verbindung Azure mit

Starten Sie Azure PowerShell und [Herstellen einer Verbindung Ihres Abonnements mit](../powershell-install-configure.md):

```PowerShell

    Add-AzureAccount
    Switch-AzureMode AzureResourceManager
```


## <a name="get-alerts"></a>Erhalten von Benachrichtigungen

    Get-AlertRule -ResourceGroup "Fabrikam" [-Name "My rule"] [-DetailedOutput]

## <a name="add-alert"></a>Hinzufügen einer Benachrichtigung


    Add-AlertRule  -Name "{ALERT NAME}" -Description "{TEXT}" `
     -ResourceGroup "{GROUP NAME}" `
     -ResourceId "/subscriptions/{SUBSCRIPTION ID}/resourcegroups/{GROUP NAME}/providers/microsoft.insights/components/{APP RESOURCE NAME}" `
     -MetricName "{METRIC NAME}" `
     -Operator GreaterThan  `
     -Threshold {NUMBER}   `
     -WindowSize {HH:MM:SS}  `
     [-SendEmailToServiceOwners] `
     [-CustomEmails "EMAIL1@X.COM","EMAIL2@Y.COM" ] `
     -Location "East US"
     -RuleType Metric



## <a name="example-1"></a>Beispiel 1

E-Mail-Benachrichtigung senden, wenn die Antwort des Servers auf HTTP-Anfragen, Durchschnitt über 5 Minuten langsamer als 1 Sekunde ist. Meine Anwendung Einsichten Ressource heißt IceCreamWebApp, und es ist in Ressourcengruppe Fabrikam. Ich bin der Besitzer des Azure-Abonnements.

Die GUID ist die Abonnement-ID (nicht die Taste Instrumentation der Anwendung).

    Add-AlertRule -Name "slow responses" `
     -Description "email me if the server responds slowly" `
     -ResourceGroup "Fabrikam" `
     -ResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Fabrikam/providers/microsoft.insights/components/IceCreamWebApp" `
     -MetricName "request.duration" `
     -Operator GreaterThan `
     -Threshold 1 `
     -WindowSize 00:05:00 `
     -SendEmailToServiceOwners `
     -Location "East US" -RuleType Metric

## <a name="example-2"></a>Beispiel 2

Ich habe eine Anwendung, in denen ich [TrackMetric()](app-insights-api-custom-events-metrics.md#track-metric) mit Berichten eine Metrik mit dem Namen "SalesPerHour". Senden Sie eine e-Mail an meinen Kollegen Durchschnitt Wenn "SalesPerHour" unter 100, fällt mehr als 24 Stunden.

    Add-AlertRule -Name "poor sales" `
     -Description "slow sales alert" `
     -ResourceGroup "Fabrikam" `
     -ResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Fabrikam/providers/microsoft.insights/components/IceCreamWebApp" `
     -MetricName "salesPerHour" `
     -Operator LessThan `
     -Threshold 100 `
     -WindowSize 24:00:00 `
     -CustomEmails "satish@fabrikam.com","lei@fabrikam.com" `
     -Location "East US" -RuleType Metric

Die gleiche Regel kann für die Metrik gemeldet mithilfe der [Maße Parameter](app-insights-api-custom-events-metrics.md#properties) ein anderes Verlauf Anrufe wie TrackEvent oder TrackPageView verwendet werden.

## <a name="metric-names"></a>Metrische Namen

Metrische Namen | Namen des Bildschirms | Beschreibung
---|---|---
`basicExceptionBrowser.count`|Browser Ausnahmen|Anzahl der nicht abgefangen Ausnahmen in den Browser.
`basicExceptionServer.count`|Serverausnahmen|Anzahl der Ausnahmefehler ausgelöst wird, von der app
`clientPerformance.clientProcess.value`|Clientverarbeitungszeit|Zeit zwischen dem letzte Byte eines Dokuments empfangen, bis das DOM geladen wird. Asynchrone Anfragen möglicherweise noch verarbeitet werden.
`clientPerformance.networkConnection.value`|Seite laden Netzwerk Verbindungszeit| Wie lange dauert im Browser zu mit dem Netzwerk herzustellen. 0 kann sein, wenn der Cache.
`clientPerformance.receiveRequest.value`|Empfangen Reaktionszeiten| Zeit zwischen Browser senden Anforderung an beginnen Sie mit der Antwort zu erhalten.
`clientPerformance.sendRequest.value`|Senden Sie die Anfrage Zeit| Zeit, Browser zum Senden der Besprechungsanfrage.
`clientPerformance.total.value`|Browser Seite laden Zeit|Zeit von Benutzer Anforderung bis DOM, Stylesheets, Skripts und Bilder geladen sind.
`performanceCounter.available_bytes.value`|Verfügbaren Arbeitsspeicher|Arbeitsspeicher für einen Prozess oder für das System sofort verfügbar.
`performanceCounter.io_data_bytes_per_sec.value`|EA-Prozess (Disagio)|Insgesamt Byte pro Sekunde gelesen und geschrieben auf Dateien, Netzwerk und Geräte.
`performanceCounter.number_of_exceps_thrown_per_sec`|Ausnahme Zins|Ausnahmen pro Sekunde.
`performanceCounter.percentage_processor_time.value`|CPU-Prozess|Der Prozentsatz der verstrichenen Zeit von allen Prozessthreads vom Prozessor zu Ausführung Anweisungen für den Applications-Prozess verwendet.
`performanceCounter.percentage_processor_total.value`|CPU-Zeit|Der Prozentsatz der Zeit, die der Prozessor in belegten Threads benötigt.
`performanceCounter.process_private_bytes.value`|Privaten bytes|Arbeitsspeicher ausschließlich die überwachte Anwendung Prozesse zugewiesen.
`performanceCounter.request_execution_time.value`|ASP.NET Anforderung Ausführung dauert|Ausführung Uhrzeit der letzten Anforderung.
`performanceCounter.requests_in_application_queue.value`|ASP.NET-Anfragen in Warteschlange|Länge der Warteschlange Anforderung Anwendung.
`performanceCounter.requests_per_sec`|ASP.NET Anforderung Zins|Die Anzahl der alle Anfragen zur Anwendung pro Sekunde ASP.NET.
`remoteDependencyFailed.durationMetric.count`|Abhängigkeit von Fehlern|Anzahl der von der Server-Anwendung auf externe Ressourcen fehlgeschlagene Aufrufe.
`request.duration`|Server-Antwort-Zeit|Zeit zwischen eine HTTP-Anforderung empfangen und die Antwort gesendet werden.
`request.rate`|Zins anfordern|Die Anzahl der alle Anfragen zur Anwendung pro Sekunde.
`requestFailed.count`|Fehler beim Besprechungsanfragen|Anzahl von HTTP-Anfragen, die eine Antwortcode geführt haben > = 400
`view.count`|Seitenansichten|Anzahl der Benutzer-Anfragen für eine Webseite anzuzeigen. Synthetische Datenverkehr wird herausgefiltert.
{Ihre benutzerdefinierten metrischen Name}|{Ihr Name metrischen}|Ein metrische Wert gemeldet durch [TrackMetric](app-insights-api-custom-events-metrics.md#track-metric) oder in den [Maßen Parameter der Gespräch Verlauf](app-insights-api-custom-events-metrics.md#properties).

Die Metrik werden von verschiedenen werden Module gesendet:

Metrische Gruppe | Collection-Modul
---|---
BasicExceptionBrowser,<br/>ClientPerformance,<br/>Ansicht | [Browser JavaScript](app-insights-javascript.md)
performanceCounter | [Leistung](app-insights-configuration-with-applicationinsights-config.md#nuget-package-3)
remoteDependencyFailed| [Abhängigkeit](app-insights-configuration-with-applicationinsights-config.md#nuget-package-1)
Anforderung<br/>Fehler bei Anforderung|[Server-Anforderung](app-insights-configuration-with-applicationinsights-config.md#nuget-package-2)

## <a name="webhooks"></a>Webhooks

Sie können [Ihre Antwort auf eine Warnung zu automatisieren](../monitoring-and-diagnostics/insights-webhooks-alerts.md). Azure Ruft eine Webadresse Ihrer Wahl, wenn eine Warnung ausgelöst wird.

## <a name="see-also"></a>Siehe auch


* [So konfigurieren Sie die Anwendung Einsichten Skript](app-insights-powershell-script-create-resource.md)
* [Erstellen Sie Anwendung Einsichten und Web Testressourcen auf Grundlage von Vorlagen](app-insights-powershell.md)
* [Automatisieren Sie, wodurch Microsoft Azure-Diagnose mit Anwendung Einsichten](app-insights-powershell-azure-diagnostics.md)
* [Automatisieren Sie Ihre Antwort auf eine Warnung](../monitoring-and-diagnostics/insights-webhooks-alerts.md)

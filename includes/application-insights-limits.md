Es gibt einige Einschränkungen auf die Anzahl der Maße und Ereignisse pro Anwendung (d. h., pro Instrumentation-Taste). 

Grenzwerte hängen davon ab, die [Preise Ebene](https://azure.microsoft.com/pricing/details/application-insights/) , die Sie auswählen.

**Ressource** | **Standardmäßige Grenzwert** | **Obergrenze**
-------- | ------------- | -------------
Datenpunkte Sitzung<sup>1, 2</sup> pro Monat | unbegrenzte | 
Total Datenpunkte pro Monat für Anforderung, Ereignis, Abhängigkeit, Spur, Ausnahme und Datenzugriffsseiten-Ansicht | 5 Millionen | 50 Millionen<sup>3</sup>
[Spur und melden Sie sich](../articles/application-insights/app-insights-search-diagnostic-logs.md) Daten Zins | 200 dp/s | 500 dp/s
[Ausnahme](../articles/application-insights/app-insights-asp-net-exceptions.md) Daten Zins | 50 dp/s | 50 dp/s
Addieren von Daten Abschlag (Disagio) anfordern, Ereignis, Abhängigkeit und Seite Ansicht werden | 200 dp/s | 500 dp/s
Unformatierte Daten Aufbewahrungsrichtlinien für [Such-](../articles/application-insights/app-insights-diagnostic-search.md) und [Analytics](../articles/application-insights/app-insights-analytics.md) | 7 Tage
Aggregierte Daten Aufbewahrungsrichtlinien für [Kennzahlen explorer](../articles/application-insights/app-insights-metrics-explorer.md) | 90 Tage
[Eigenschaft](../articles/application-insights/app-insights-api-custom-events-metrics.md#properties) Namen zählen | 100 |
Die Länge von Abfragenamen | 150 | 
Eigenschaft Wertlänge | 8192 | 
Spur und Ausnahme Nachrichtenlänge | 10000 |
[Metrisch](../articles/application-insights/app-insights-api-custom-events-metrics.md#properties) Namen zählen | 100 |
Metrische Namenlänge |  150 | 
[Verfügbarkeit von tests](../articles/application-insights/app-insights-monitor-web-app-availability.md) | 10 | 

<sup>1</sup> ein Datenpunkt ist eine einzelne metrischen Wert oder das zugeordnete Ereignis, angefügte Eigenschaften und Maßeinheiten.

<sup>2</sup> A-Sitzung Datenpunkt am Anfang oder Ende einer Sitzung protokolliert und Benutzeridentität protokolliert.

<sup>3</sup> können Sie zusätzlichen Kapazität über 50 Millionen erwerben.
 
[Informationen zu Preise und Anwendung Einsichten von Kontingenten](../articles/application-insights/app-insights-pricing.md)

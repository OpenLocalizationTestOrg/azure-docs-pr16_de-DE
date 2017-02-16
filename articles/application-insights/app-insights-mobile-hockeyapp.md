<properties
    pageTitle="Für mobile Web apps mit Entwicklertools Analytics-Systemleistung | Microsoft Azure"
    description="Leistung der Anwendung und die Verwendung Überwachung für mobile-app-Entwickler. , Desktop, Webdienst und Back-End-apps mit HockeyApp und Einblicken Anwendung."
    authors="alancameronwills"
    services="application-insights"
    documentationCenter=""
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="09/19/2016"
    ms.author="awills"/>

# <a name="mobile-analytics-with-hockeyapp-and-application-insights"></a>Mobile Analytics mit HockeyApp und Einblicken Anwendung

Überwachen Sie die Leistung und die Verwendung Ihrer Beta-Test und bereitgestellte mobile als auch desktop-apps mit [HockeyApp](https://hockeyapp.net/). Überwachen der unterstützenden Web Dienst und Back-End-apps mit [Visual Studio-Anwendung Einsichten](app-insights-overview.md)an. Analysieren von Daten aus dem Client und Server-apps in Analytics, und zeigen Sie die Diagramme nebeneinander in einer Azure Dashboard.

Microsoft Entwicklertools Analytics bietet zwei Lösungen für Performance-Überwachung:

* **HockeyApp für Mobile** und Desktop-apps.
 * Verteilen Sie Testversionen für ausgewählte Benutzer.
 * Stürzt ab Analyse.
 * Benutzerdefinierte werden für die Verwendungsanalyse.
* **Anwendung Einsichten für Websites** und-Diensten sowie Back-End-Applikationen.
 * Leistung und die Verwendung Kennzahlen und Benachrichtigungen.
 * Ausnahme reporting und diagnostic Tracing.
 * Diagnose suchen mit zum Filtern und den zugehörigen werden Links.

Bieten beide Lösungen:

 * Leistungsfähige **[analytisches Abfragesprache](app-insights-analytics.md)** für Diagnose und Analyse.
 * **[Exportieren von Daten](app-insights-export-telemetry.md)** in Ihrem eigenen Storage.
 * **Integrierte Dashboard** -Anzeige von Analysediagrammen und Tabellen.

## <a name="monitor-your-app-components"></a>Überwachen Sie Ihre app-Komponenten

Folgen Sie den Anweisungen in folgenden Seiten in Ihrem Code das SDK installieren und mit dem überwachen Ihre app ein.

### <a name="web-apps---application-insights"></a>Web apps - Anwendung Einsichten

* [ASP.NET Web app](app-insights-asp-net.md) 
* [Java Web app](app-insights-java-get-started.md)
* [Node.js Web app](https://github.com/Microsoft/ApplicationInsights-node.js)
* [Azure Cloud Services](app-insights-cloudservices.md)

### <a name="mobile-apps---hockeyapp"></a>Mobile apps - HockeyApp

* [iOS-app](https://support.hockeyapp.net/kb/client-integration-ios-mac-os-x-tvos/hockeyapp-for-ios)
* [Mac OS X-app](https://support.hockeyapp.net/kb/client-integration-ios-mac-os-x-tvos/hockeyapp-for-mac-os-x)
* [Android-app](https://support.hockeyapp.net/kb/client-integration-android/hockeyapp-for-android-sdk)
* [Universal Windows-app](https://support.hockeyapp.net/kb/client-integration-windows-and-windows-phone/how-to-create-an-app-for-uwp)
* [App für Windows Phone 8 und Windows 8.1](https://support.hockeyapp.net/kb/client-integration-windows-and-windows-phone/hockeyapp-for-windows-phone-silverlight-apps-80-and-81)
* [Windows Presentation Foundation-app](https://support.hockeyapp.net/kb/client-integration-windows-and-windows-phone/hockeyapp-for-windows-wpf-apps)

Windows-desktop-apps empfehlen wir HockeyApp. Aber Sie können auch [werden in einer Windows-app zu Anwendung Einsichten zu senden](app-insights-windows-desktop.md). Möglicherweise möchten zum Experimentieren Sie mit der Anwendung Einsichten erledigen.


## <a name="analytics-and-export-for-hockeyapp-telemetry"></a>Analytics und Export für HockeyApp werden

Sie können HockeyApp benutzerdefinierte ermitteln und melden Sie sich mit den Funktionen Analytics und fortlaufender Exportieren der Anwendung Einsichten durch die [Einrichtung von eine Verbindung](app-insights-hockeyapp-bridge-app.md)werden.





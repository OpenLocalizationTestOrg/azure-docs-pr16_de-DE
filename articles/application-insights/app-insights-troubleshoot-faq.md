<properties 
    pageTitle="Behandlung von Problemen und Fragen zur Anwendung Einsichten" 
    description="Etwas in Visual Studio-Anwendung Einsichten unklar oder nicht arbeiten? Versuchen Sie hier." 
    services="application-insights" 
    documentationCenter=".net"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/24/2016" 
    ms.author="awills"/>
 
# <a name="questions---application-insights-for-aspnet"></a>Fragen - Anwendung Einsichten für ASP.NET

## <a name="configuration-problems"></a>Diagnostizieren

*Ich habe Probleme beim Festlegen von meinem:*

* [.NET-app](app-insights-asp-net-troubleshoot-no-data.md)
* [Überwachen von einer bereits ausgeführten-app](app-insights-monitor-performance-live-website-now.md#troubleshooting)
* [Azure-Diagnose](app-insights-azure-diagnostics.md)
* [Java Web app](app-insights-java-troubleshoot.md)
* [Andere Plattformen](app-insights-platforms.md)

*Ich erhalte keine Daten von meinem server*

* [Die Firewallausnahmen festlegen](app-insights-ip-addresses.md)
* [Richten Sie einen ASP.NET-Server](app-insights-monitor-performance-live-website-now.md)
* [Richten Sie einen Java-server](app-insights-java-agent.md)


## <a name="can-i-use-application-insights-with-"></a>Kann ich Anwendung Einsichten mit verwenden...?

[Finden Sie unter Plattformen][platforms]


## <a name="is-it-free"></a>Handelt es sich kostenlose?

* Ja, wenn Sie die kostenlose [Preise Ebene](app-insights-pricing.md)auswählen. Sie erhalten die meisten Features und ein viele Kontingent von Daten. 
* Sie müssen Ihre Kreditkartendaten mit Microsoft Azure registrieren angeben, aber keine Gebühren durchgeführt werden, es sei denn, Sie verwenden eine andere kostenpflichtige Azure Service oder auf eine Stufe zahlenden explizit aktualisieren.
* Wenn Ihre app mehr Daten als das monatliche Kontingent für die kostenlose Leiste sendet, stoppt das Programm angemeldet sein. In diesem Fall können Sie festlegen, dass Ihre Zahlungsmethode beginnen oder warten, bis das Kontingent am Ende des Monats zurückgesetzt wird.
* Einfache Verwendung und Sitzung Daten unterliegt keinem Kontingent.
* Es gibt auch eine 30-Tage-Testversion, während, die Sie die bezahlte Features kostenlos erhalten.
* Jede Anwendungsressource weist eine separate Kontingent, und legen Sie deren Preisgestaltung Ebene unabhängig von einem beliebigen anderen.

#### <a name="what-do-i-get-if-i-pay"></a>Wie erhalte ich, wenn ich Zahlen?

* Ein größeres [monatliche Kontingent von Daten](https://azure.microsoft.com/pricing/details/application-insights/).
* Option zum veralteten fortsetzen Sammeln von Daten über das monatliche Kontingent bezahlen. Wenn Ihre Daten über Kontingent bewegt wird, sind Sie pro Mb angerechnet.
* [Fortlaufend zu exportieren](app-insights-export-telemetry.md).


## <a name="a-nameq14awhat-does-application-insights-modify-in-my-project"></a><a name="q14"></a>Was ändern Anwendung Einsichten in meinem Projekt?

Die Details abhängig vom Typ des Projekts aus. Für eine Webanwendung:


+ Diese Dateien hinzugefügt dem Projekt:

 + ApplicationInsights.config. 
 + AI.js


+ Diese Pakete NuGet-Installationen:

 -  *Anwendung Einsichten API* - das Herzstück API

 -  *Anwendung Einsichten API for Web Applications* - verwendet, werden vom Server zu senden.

 -  *Anwendung Einsichten API for Applications JavaScript* - verwendet, werden vom Client zu senden.

    Die Pakete enthalten die folgenden Assemblys:

 - Microsoft.ApplicationInsights

 - Microsoft.ApplicationInsights.Platform

+ Fügt Elemente in:

 - Web.config

 - Packages.config

+ (Neue nur - Projekten, wenn Sie die [Anwendung Einsichten zu einem vorhandenen Projekt hinzufügen][start], müssen Sie dies manuell vornehmen.) Fügt Codeausschnitte in den Client- und Server-Code zur Initialisierung mit der Anwendung Einsichten Ressource-ID. Beispielsweise wird in einer app MVC Code in die Gestaltungsvorlage Views/Shared/_Layout.cshtml eingefügt


## <a name="how-do-i-upgrade-from-older-sdk-versions"></a>Wie kann ich von älteren Versionen von SDK aktualisieren?

Je nach Typ der Anwendung SDK finden Sie unter der [Versionsinformationen](app-insights-release-notes.md) . 



## <a name="a-nameupdateahow-can-i-change-which-azure-resource-my-project-sends-data-to"></a><a name="update"></a>Wie kann ich ändern, welche Azure Ressource Mein Projekt Daten sendet?

Explorer-Lösung, mit der Maustaste `ApplicationInsights.config` , und wählen Sie **Die Anwendung Einsichten aktualisieren**. Sie können die Daten auf einer vorhandenen oder neuen Ressource in Azure senden. Die Update-Assistent ändert die Taste Instrumentation in ApplicationInsights.config, wodurch bestimmt wird, wo der Server SDK Ihre Daten sendet. Solange Sie "Alle aktualisieren" deaktivieren, wird es auch die Taste ändern, wo er in Ihren Webseiten angezeigt wird.


#### <a name="a-namedataahow-long-is-data-retained-in-the-portal-is-it-secure"></a><a name="data"></a>Wie lange bleibt die Daten im Portal? Ist es sicher?

Schauen Sie sich die [Daten Aufbewahrung und Ihre Privatsphäre][data].

## <a name="logging"></a>Protokollierung

#### <a name="a-namepostahow-do-i-see-post-data-in-diagnostic-search"></a><a name="post"></a>Wie zeige ich POST-Daten in einer Diagnoseprotokollen suchen?

Wir nicht POST-Daten nicht automatisch angemeldet, aber Sie können einen Anruf TrackTrace: setzen die Daten in der Nachricht Parameter. Größe maximal länger als die Grenzwerte für die Zeichenfolgeneigenschaften, verfügt Obwohl Sie dort filtern können 

## <a name="security"></a>Sicherheit

#### <a name="is-my-data-secure-in-the-portal-how-long-is-it-retained"></a>Ist meine Daten im Portal sicher? Wie lange bleibt?

Finden Sie unter [Daten Aufbewahrung und Ihre Privatsphäre][data].


## <a name="a-nameq17a-have-i-enabled-everything-in-application-insights"></a><a name="q17"></a>Aktiviert kann ich alle Elemente in der Anwendung Einsichten haben?

<table border="1">
<tr><th>Was sollte angezeigt werden</th><th>Bezugsarten</th><th>Warum er soll</th></tr>
<tr><td>Verfügbarkeit von Diagrammen</td><td><a href="../app-insights-monitor-web-app-availability/">Webtests</a></td><td>Wissen Sie, dass die Web-app von stammt</td></tr>
<tr><td>Server-app Perf: Reaktionszeiten,...
</td><td><a href="../app-insights-asp-net/">Hinzufügen der Anwendung Einsichten zu einem Projekt</a><br/>oder <br/><a href="../app-insights-monitor-performance-live-website-now/">Installieren von AI Status Monitor auf Server</a> (oder Schreiben von eigenem Code zum <a href="../app-insights-api-custom-events-metrics/#track-dependency">Nachverfolgen von Abhängigkeiten</a>)</td><td>Perf Probleme erkennen</td></tr>
<tr><td>Abhängigkeit werden</td><td><a href="../app-insights-monitor-performance-live-website-now/">AI Status Monitor auf dem Server zu installieren.</a></td><td>Diagnostizieren Sie Probleme mit Datenbanken oder andere externen Komponenten</td></tr>
<tr><td>Abrufen von Spuren Stapel von Ausnahmen</td><td><a href="../app-insights-search-diagnostic-logs/#exceptions">TrackException Anrufe in den Code einfügen</a> (jedoch einige werden automatisch gemeldet)</td><td>Erkennen und zu diagnostizieren Ausnahmen</td></tr>
<tr><td>Suche Log Spuren</td><td><a href="../app-insights-search-diagnostic-logs/">Fügen Sie einen Protokollierung Netzwerkadapter hinzu</a></td><td>Diagnostizieren von Ausnahmen, Perf Probleme bei</td></tr>
<tr><td>Grundlagen der Client-Verwendung: Seitenansichten, Sitzungen,...</td><td><a href="../app-insights-javascript/">JavaScript-Initialisierung in Webseiten</a></td><td>Nutzungsanalysen</td></tr>
<tr><td>Benutzerdefinierte Metrik Client</td><td><a href="../app-insights-api-custom-events-metrics/">Verlauf Anrufe in Webseiten</a></td><td>Verbessern der Benutzerfunktionalität</td></tr>
<tr><td>Benutzerdefinierte Metrik Server</td><td><a href="../app-insights-api-custom-events-metrics/">Verlauf Anrufe im Server-code</a></td><td>Business intelligence</td></tr>
</table>


## <a name="automation"></a>Automatisierung

Sie können zum Erstellen und aktualisieren die Anwendung Einsichten Ressourcen [PowerShell-Skripts schreiben](app-insights-powershell.md) .

## <a name="more-answers"></a>Weitere Antworten

* [Anwendung Einsichten-forum](https://social.msdn.microsoft.com/Forums/vstudio/en-US/home?forum=ApplicationInsights)


<!--Link references-->

[data]: app-insights-data-retention-privacy.md
[platforms]: app-insights-platforms.md
[start]: app-insights-overview.md
[windows]: app-insights-windows-get-started.md

 
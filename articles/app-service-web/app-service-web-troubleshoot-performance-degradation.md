<properties
    pageTitle="Web app verlangsamen im App-Dienst | Microsoft Azure"
    description="In diesem Artikel soll Ihnen langsam Web app Leistungsprobleme in Azure-App-Verwaltungsdienst zu behandeln."
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""
    tags="top-support-issue"
    keywords="Web app Leistung, langsam app app langsam"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/06/2016"
    ms.author="cephalin"/>

# <a name="troubleshoot-slow-web-app-performance-issues-in-azure-app-service"></a>Behandeln von Problemen mit langsam Web app Leistungsprobleme in Azure-App-Verwaltungsdienst

In diesem Artikel soll Ihnen langsam Web app Leistungsprobleme in [Azure-App-Verwaltungsdienst](http://go.microsoft.com/fwlink/?LinkId=529714)zu behandeln.

Wenn Sie an einer beliebigen Stelle in diesem Artikel weitere Hilfe benötigen, können Sie die Azure-Experten auf [der MSDN-Azure und den Stapelüberlauf Foren](https://azure.microsoft.com/support/forums/)kontaktieren. Alternativ können Sie auch einen Supportvorfall Azure ablegen. Wechseln Sie zur [Azure-Support-Website](https://azure.microsoft.com/support/options/) , und klicken Sie auf **Anfordern von Support**.

## <a name="symptom"></a>Problem

Wenn Sie die Web-app, die Seiten laden langsam und manchmal Timeout navigieren.

## <a name="cause"></a>Ursache

Dieses Problem wird durch die Anwendung Ebene Probleme, wie oft verursacht:

-   Anfragen sehr lange dauert
-   Anwendung mit hoher Arbeitsspeicher/CPU-Auslastung
-   die Anwendung aufgrund einer Ausnahme abstürzt.

## <a name="troubleshooting-steps"></a>Schritte zur Problembehandlung

Problembehandlung bei kann in drei unterschiedliche Aufgaben nacheinander unterteilt werden:

1.  [Beobachten und Verhalten der Anwendung überwachen](#observe)
2.  [Sammeln von Daten](#collect)
3.  [Minimieren Sie das Problem](#mitigate)

[App Dienst Web Apps](/services/app-service/web/) bietet verschiedene Optionen bei jedem Schritt.

<a name="observe" />
### <a name="1-observe-and-monitor-application-behavior"></a>1. beobachten und Verhalten der Anwendung überwachen

#### <a name="track-service-health"></a>Nachverfolgen von Dienststatus

Microsoft Azure publicizes jedes Mal, wenn eine Unterbrechung oder Leistung dienstbeeinträchtigung vorhanden ist. Sie können die Integrität des Diensts in [Azure-Portal](https://portal.azure.com/)verfolgen. Weitere Informationen finden Sie unter [Nachverfolgen Dienststatus](../monitoring-and-diagnostics/insights-service-health.md).

#### <a name="monitor-your-web-app"></a>Überwachen der Web-app

Diese Option ermöglicht es Ihnen, um herauszufinden, ob es sich bei Ihrer Anwendung Probleme auftreten. Klicken Sie in der Web-app Blade auf die Kachel **Anfragen und Fehler** . Das Blade **Metrik** zeigt Ihnen die alle Kriterien, die Sie hinzufügen können.

Einige der Kriterien, die Sie für Ihre Web app überwachen möglicherweise sind

-   Durchschnittliche Arbeitsspeicher arbeiten festlegen
-   Durchschnittliche Reaktionszeiten
-   CPU-Zeit
-   Arbeitsseite Arbeitsspeicher
-   Besprechungsanfragen

![Überwachen der Leistung von Web app](./media/app-service-web-troubleshoot-performance-degradation/1-monitor-metrics.png)

Weitere Informationen finden Sie unter:

-   [Überwachen der Web Apps in Azure App-Verwaltungsdienst](web-sites-monitor.md)
-   [Lassen Sie sich benachrichtigen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

#### <a name="monitor-web-endpoint-status"></a>Überwachen der Web-Endpunkt status

Wenn Sie die Web-app in **Standard** Preise Ebene ausführen, können Web Apps Sie 2 Endpunkte 3 geografischen Orten zu überwachen.

Überwachen der Endpunkt konfiguriert Webtests Geo verteilt Orten, die Antwortzeit und Verfügbarkeit der Web-URLs zu testen. Der Test führt einen HTTP GET Vorgang auf die Web-URL zum Bestimmen der Antwortzeit und die Verfügbarkeit von jedem Standort aus. Jede konfigurierten Speicherort ausgeführt wird, einen Testanruf fünf Minuten.

Verfügbarkeit wird mit HTTP-Antwortcodes überwacht und Reaktionszeiten in Millisekunden gemessen wird. Ein Überwachung Test fehlschlägt, ist der HTTP-Antwortcode größer als oder gleich 400 oder wenn die Antwort mehr als 30 Sekunden dauert. Ein Endpunkt gilt zur Verfügung, wenn seine Überwachung Tests alle angegebenen Orten erfolgreich ausgeführt werden kann.

Zum Einrichten, finden Sie unter [wie: Überwachen der Web-Endpunkt Status](web-sites-monitor.md#webendpointstatus).

Siehe auch [planmäßigen Azure Websites von plus Endpunkt Überwachung - mit Stefan Schackow](/documentation/videos/azure-web-sites-endpoint-monitoring-and-staying-up/) für ein Video zum Überwachen der Endpunkt ein.

#### <a name="application-performance-monitoring-using-extensions"></a>Anwendung Leistung Überwachung Erweiterungen verwenden

Sie können auch die Leistung Ihrer Anwendung überwachen, durch die Nutzung von _Website-Erweiterungen_.

Jede App Dienst Web app bietet einen extensible Management-Endpunkt, mit dem Sie eine Reihe von Tools bereitgestellt, die als Website Erweiterungen leistungsfähige nutzen können. Diese Tools Bereich aus der Quelle Code-Editoren wie [Visual Studio Team Services](https://www.visualstudio.com/products/what-is-visual-studio-online-vs.aspx) Verwaltungstools für verbundenen Ressourcen, wie etwa einer MySQL-Datenbank mit einem Web-app verbunden.

[Azure Anwendung Einsichten](/services/application-insights/) und [Neue Relic](/marketplace/partners/newrelic/newrelic/) sind zwei der Leistung Überwachung von Website-Erweiterungen, die verfügbar sind. Wenn Sie neue Relic verwenden möchten, installieren Sie einen Agent zur Laufzeit aus. Um Azure Anwendung Einsichten zu verwenden, Sie erstellen Ihre Code mit einem SDK neu, und können Sie auch eine Erweiterung für den Zugriff auf weitere Daten installieren. Das SDK können Sie das Schreiben von Code, um die Verwendung und Leistung der app ausführlicher überwachen.

Um Einsichten Anwendung verwenden zu können, finden Sie unter [Überwachen der Leistung in Webanwendungen](../application-insights/app-insights-web-monitor-performance.md).

Wenn Sie neue Relic verwenden möchten, finden Sie unter [Neue Relic Leistung Anwendungsverwaltung auf Azure](../store-new-relic-cloud-services-dotnet-application-performance-management.md).

<a name="collect" />
### <a name="2-collect-data"></a>2. Sammeln von Daten

####    <a name="enable-diagnostics-logging-for-your-web-app"></a>Aktivieren Sie Diagnoseprotokolle für Web app

Die Web Apps-Umgebung bietet diagnostic Funktionen für die Protokollinformationen aus den Webserver und der Anwendung. Diese sind logisch in Web Server Diagnose und Anwendung Diagnose getrennt.

##### <a name="web-server-diagnostics"></a>Web Server-Diagnose

Sie können aktivieren oder deaktivieren die folgenden Arten von Protokollen:

-   **Ausführliche Protokollierung-Fehler** - detaillierte Fehlerinformationen für HTTP Statuscodes, die einen Fehler (Statuscode 400 oder höher). Dies kann Informationen enthalten, die Ihnen festzustellen helfen, warum der Server den Fehlercode zurückgegeben.
-   **Failed Anforderung Tracing** - ausführliche Informationen zu fehlgeschlagener Anfragen, einschließlich einer Spur zum Verarbeiten von Anfrage und die Verarbeitungszeit in jede Komponente verwendet IIS-Komponenten. Dies kann sinnvoll, wenn Sie versuchen, Verbessern der Leistung der Web-app oder isolieren, was mit einen bestimmten HTTP-Fehler verursacht sein.
-   **Web Server Protokollierung** - Informationen über HTTP-Transaktionen mit dem W3C erweiterte Log-Dateiformat. Dies ist hilfreich beim Bestimmen der übergeordneten Web app Unternehmensdaten, beispielsweise die Anzahl der Anfragen verarbeitet oder wie viele Anfragen von einer bestimmten IP-Adresse sind.

##### <a name="application-diagnostics"></a>Anwendung-Diagnose

Anwendung Diagnose können Sie von einer Webanwendung erzeugte erfassen. ASP.NET Applications können die `System.Diagnostics.Trace` Klasse Informationen in der Anwendung Diagnose Ereignisprotokoll.

Weitere Informationen zum Konfigurieren Ihrer Anwendungs für die Protokollierung finden Sie unter [Diagnoseprotokoll für Web apps in Azure-App-Verwaltungsdienst aktivieren](web-sites-enable-diagnostic-log.md).

#### <a name="use-remote-profiling"></a>Verwenden Sie Remote Profil erstellen.

In Azure App Dienst können Web Apps, Apps-API und WebJobs Remote ein Profil erstellt werden. Wenn der Prozess langsamer läuft als erwartet, oder die Wartezeit des HTTP-Anfragen höher als normal und CPU-Auslastung des Prozesses ebenfalls hoch, können Sie Remote Profil Prozesses und erhalten die Anruf Stapeln CPU werden, zum Analysieren von Verarbeitungsaktivität sowie Code wichtiges Pfade.

Weitere Informationen finden Sie unter [Remote-Profil Unterstützung in Azure-App-Dienst](/blog/remote-profiling-support-in-azure-app-service).


#### <a name="use-the-azure-app-service-support-portal"></a>Verwenden des App-Verwaltungsdienst Azure Support-Portals

Web Apps bietet Ihnen die Möglichkeit zum Behandeln von Problemen im Zusammenhang mit der Web-app anhand HTTP-Protokolle, Ereignisprotokollen, Prozess speichert und mehr. Sie können alle diese Informationen über unsere Support-Portal unter zugreifen **http://&lt;den Namen der Anwendung >.scm.azurewebsites.net/Support**

Im Portal Unterstützung zu Azure App-Dienst bietet Ihnen drei separate Registerkarten, um die drei Schritte zur Problembehandlung häufig zu unterstützen:

1.  Beobachten Sie aktuelle Verhalten
2.  Analysieren von Diagnoseinformationen zu sammeln und die integrierten Analyzern ausgeführt
3.  Minimieren

Wenn das Problem sofort weiterhin ist, klicken Sie auf **Analysieren** > **Diagnose** > **Diagnostizieren jetzt** zum Erstellen einer Diagnoseprotokollen Sitzung für Sie dem HTTP sammeln wird protokolliert, Protokolle der Ereignisanzeige, Arbeitsspeicher speichert, Fehlerprotokolle von PHP und PHP Bericht zu verarbeiten.

Nachdem die Daten gesammelt werden, wird auch auf der Registerkarte Daten eine Analyse ausgeführt und bieten Ihnen einen HTML-Bericht.

Für den Fall, dass Sie die Daten standardmäßig herunterladen möchten, würden sie im Ordner D:\home\data\DaaS gespeichert werden.

Weitere Informationen über das Azure App Service Support-Portal finden Sie unter [Neuen Updates an Support Website Erweiterung für Azure-Websites](/blog/new-updates-to-support-site-extension-for-azure-websites).

#### <a name="use-the-kudu-debug-console"></a>Verwenden Sie die Kudu Debuggen Konsole

Web Apps verfügt über eine Debuggen-Konsole, die Sie für das Debuggen, untersuchen, Hochladen von Dateien als auch JSON-Endpunkte zum Abrufen von Informationen über Ihre Umgebung verwenden können. Dies ist der _Kudu Console_ oder dem _SCM Dashboard_ für Ihre Web app bezeichnet.

Sie können dieses Dashboard zugreifen, indem Sie auf den Link **https://&lt;den Namen der Anwendung >.scm.azurewebsites.net/**.

Einige der Dinge, die Kudu bietet sind:

-   umgebungseinstellungen für eine Anwendung
-   Log-stream
-   Diagnose Abbild
-   Debuggen Sie Console in der Powershell-Cmdlets und Basisbefehlen DOS ausgeführt werden kann.


Ein anderes nützliches Feature von Kudu besteht darin, für den Fall, dass eine Anwendung Auslösen von Ausnahmen der ersten Chance ist, Sie Kudu können und das Tool SysInternals Procdump Arbeitsspeicher zu erstellen sichert. Diese Weise Arbeitsspeicher sind Momentaufnahmen des Prozesses und häufig Ihnen helfen können kompliziertere Probleme mit der Web-app zu behandeln.

Weitere Informationen zu den Features, die in Kudu verfügbar finden Sie unter [Azure Websites Team Services-Tools, die, denen Sie kennen sollten](/blog/windows-azure-websites-online-tools-you-should-know-about/).

<a name="mitigate" />
### <a name="3-mitigate-the-issue"></a>3. das Problem zu verringern

####    <a name="scale-the-web-app"></a>Skalieren des Web app

In Azure App Dienst können für höhere Performance und Durchsatz – Sie den Maßstab anpassen an dem Sie die Anwendung ausführen. Einrichten einer Web app Skalierung umfasst zwei verknüpfte Aktionen: Ihre App-Serviceplan in eine höhere Preisgestaltung Stufe ändern möchten, und Konfigurieren bestimmter Einstellungen aus, nachdem Sie in der höheren Ebene des Preisgestaltung gewechselt haben.

Weitere Informationen zu skalieren finden Sie unter [Skalieren einer Web-app in Azure-App-Dienst](web-sites-scale.md).

Darüber hinaus können Sie auswählen, auf Ihrer Anwendung auf mehr als eine Instanz ausgeführt werden. Dies nicht nur bietet Ihnen weitere Verarbeitung Videofunktionen, sondern auch erhalten Sie einige Menge Fehlertoleranz. Wenn der Vorgang einer einzelnen Instanz fällt aus, weiterhin die andere Instanz weiterhin Anfragen erstellen.

Sie können festlegen, die Skalierung benutzerspezifisch manuell oder automatisch.

####    <a name="use-autoheal"></a>Verwenden von AutoHeal

AutoHeal Wiederverwendung Worker-Prozess für Ihre app basierend auf Einstellungen (wie Änderungen Konfiguration, Besprechungsanfragen, Speicher basierende Grenzwerte oder die Zeit, die zum Ausführen einer Anforderung erforderlich sind) ausgewählt haben. In den meisten Fällen, ist Papierkorb des Prozesses die schnellste Möglichkeit, ein Problem zu beheben. Obwohl Sie die Web-app aus direkt im Portal Azure immer gestartet werden können, wird AutoHeal es automatisch für Sie ausführen. Sie müssen lediglich einige Trigger in diese Datei aus der Web app hinzufügen. Beachten Sie, dass diese Einstellungen auf die gleiche Weise arbeiten möchten, auch wenn eine Anwendung keiner .net eine ist.

Weitere Informationen finden Sie unter [Automatische Korrektur Azure-Websites](/blog/auto-healing-windows-azure-web-sites/).

####    <a name="restart-the-web-app"></a>Starten des Web app

Dies ist oft die einfachste Methode zum Wiederherstellen von einmaligen Probleme. Klicken Sie im [Portal Azure](https://portal.azure.com/)in der Web-app Blade, werden die Optionen zum Beenden oder Neustarten Ihre app haben.

 ![Starten Sie erneut Web app, um Leistungsprobleme zu lösen.](./media/app-service-web-troubleshoot-performance-degradation/2-restart.png)

Sie können auch mithilfe der Powershell Azure Web app verwalten. Weitere Informationen finden Sie unter [Verwenden von Azure PowerShell Azure Ressourcenmanager](../powershell-azure-resource-manager.md).

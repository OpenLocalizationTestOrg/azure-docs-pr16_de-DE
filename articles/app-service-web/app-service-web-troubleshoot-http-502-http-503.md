<properties
    pageTitle="502 Ungültiger Gateway beheben, 503 Dienst nicht verfügbar Fehler | Microsoft Azure"
    description="Behandeln von Problemen mit 502 Ungültiger Gateway und 503 Dienst nicht verfügbar Fehler in Ihrer Web-app in der App-Verwaltungsdienst Azure gehostet wird."
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""
    tags="top-support-issue"
    keywords="502 Ungültiger Gateway,-503 Dienst nicht verfügbar, Fehler 503, Fehler 502"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/06/2016"
    ms.author="cephalin"/>

# <a name="troubleshoot-http-errors-of-502-bad-gateway-and-503-service-unavailable-in-your-azure-web-apps"></a>Behandeln von Problemen mit HTTP-Fehler "502 Ungültiger Gateway" und "503 Dienst nicht verfügbar" in Ihrer Azure Web apps

"502 Ungültiger Gateway" und "503 Dienst nicht verfügbar" werden häufige Fehler in Web app in der [App-Verwaltungsdienst Azure](http://go.microsoft.com/fwlink/?LinkId=529714)gehostet wird. In diesem Artikel soll Ihnen dieser Fehler zu beheben.

Wenn Sie an einer beliebigen Stelle in diesem Artikel weitere Hilfe benötigen, können Sie die Azure-Experten auf [der MSDN-Azure und den Stapelüberlauf Foren](https://azure.microsoft.com/support/forums/)kontaktieren. Alternativ können Sie auch einen Supportvorfall Azure ablegen. Wechseln Sie zur [Azure-Support-Website](https://azure.microsoft.com/support/options/) , und klicken Sie auf **Anfordern von Support**.

## <a name="symptom"></a>Problem

Wenn Sie bei der Web-app navigieren, gibt es eine HTTP "502 Ungültiger Gateway" Fehler oder ein HTTP-Fehler "503 Dienst nicht".

## <a name="cause"></a>Ursache

Dieses Problem wird durch die Anwendung Ebene Probleme, wie oft verursacht:

-   Anfragen sehr lange dauert
-   Anwendung mit hoher Arbeitsspeicher/CPU-Auslastung
-   die Anwendung aufgrund einer Ausnahme abstürzt.

## <a name="troubleshooting-steps-to-solve-502-bad-gateway-and-503-service-unavailable-errors"></a>Schritte zum Lösen von "502 Ungültiger Gateway" und "503 Dienst nicht verfügbar" Fehler bei der Problembehandlung

Problembehandlung bei kann in drei unterschiedliche Aufgaben nacheinander unterteilt werden:

1.  [Beobachten und Verhalten der Anwendung überwachen](#observe)
2.  [Sammeln von Daten](#collect)
3.  [Minimieren Sie das Problem](#mitigate)

[App Dienst Web Apps](/services/app-service/web/) bietet verschiedene Optionen bei jedem Schritt.

<a name="observe" />
### <a name="1-observe-and-monitor-application-behavior"></a>1. beobachten und Verhalten der Anwendung überwachen

####    <a name="track-service-health"></a>Nachverfolgen von Dienststatus

Microsoft Azure publicizes jedes Mal, wenn eine Unterbrechung oder Leistung dienstbeeinträchtigung vorhanden ist. Sie können die Integrität des Diensts in [Azure-Portal](https://portal.azure.com/)verfolgen. Weitere Informationen finden Sie unter [Nachverfolgen Dienststatus](../monitoring-and-diagnostics/insights-service-health.md).

####    <a name="monitor-your-web-app"></a>Überwachen der Web-app

Diese Option ermöglicht es Ihnen, um herauszufinden, ob es sich bei Ihrer Anwendung Probleme auftreten. Klicken Sie in der Web-app Blade auf die Kachel **Anfragen und Fehler** . Das Blade **Metrik** zeigt Ihnen die alle Kriterien, die Sie hinzufügen können.

Einige der Kriterien, die Sie für Ihre Web app überwachen möglicherweise sind

-   Durchschnittliche Arbeitsspeicher arbeiten festlegen
-   Durchschnittliche Reaktionszeiten
-   CPU-Zeit
-   Arbeitsseite Arbeitsspeicher
-   Besprechungsanfragen

![Überwachen der Web app bei der Behebung von HTTP-Fehler 502 Ungültiger Gateway und 503 Dienst nicht verfügbar](./media/app-service-web-troubleshoot-HTTP-502-503/1-monitor-metrics.png)

Weitere Informationen finden Sie unter:

-   [Überwachen der Web Apps in Azure App-Verwaltungsdienst](web-sites-monitor.md)
-   [Lassen Sie sich benachrichtigen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

<a name="collect" />
### <a name="2-collect-data"></a>2. Sammeln von Daten

####    <a name="use-the-azure-app-service-support-portal"></a>Verwenden des App-Verwaltungsdienst Azure Support-Portals

Web Apps bietet Ihnen die Möglichkeit zum Behandeln von Problemen im Zusammenhang mit der Web-app anhand HTTP-Protokolle, Ereignisprotokollen, Prozess speichert und mehr. Sie können alle diese Informationen über unsere Support-Portal unter zugreifen **http://&lt;den Namen der Anwendung >.scm.azurewebsites.net/Support**

Im Portal Unterstützung zu Azure App-Dienst bietet Ihnen drei separate Registerkarten, um die drei Schritte zur Problembehandlung häufig zu unterstützen:

1.  Beobachten Sie aktuelle Verhalten
2.  Analysieren von Diagnoseinformationen zu sammeln und die integrierten Analyzern ausgeführt
3.  Minimieren

Wenn das Problem sofort weiterhin ist, klicken Sie auf **Analysieren** > **Diagnose** > **Diagnostizieren jetzt** zum Erstellen einer Diagnoseprotokollen Sitzung für Sie dem HTTP sammeln wird protokolliert, Protokolle der Ereignisanzeige, Arbeitsspeicher speichert, Fehlerprotokolle von PHP und PHP Bericht zu verarbeiten.

Nachdem die Daten gesammelt werden, wird auch auf der Registerkarte Daten eine Analyse ausgeführt und bieten Ihnen einen HTML-Bericht.

Für den Fall, dass Sie die Daten standardmäßig herunterladen möchten, würden sie im Ordner D:\home\data\DaaS gespeichert werden.

Weitere Informationen über das Azure App Service Support-Portal finden Sie unter [Neuen Updates an Support Website Erweiterung für Azure-Websites](/blog/new-updates-to-support-site-extension-for-azure-websites).

####    <a name="use-the-kudu-debug-console"></a>Verwenden Sie die Kudu Debuggen Konsole

Web Apps verfügt über eine Debuggen-Konsole, die Sie für das Debuggen, untersuchen, Hochladen von Dateien als auch JSON-Endpunkte zum Abrufen von Informationen über Ihre Umgebung verwenden können. Dies ist der _Kudu Console_ oder dem _SCM Dashboard_ für Ihre Web app bezeichnet.

Sie können dieses Dashboard zugreifen, indem Sie auf den Link **https://&lt;den Namen der Anwendung >.scm.azurewebsites.net/**.

Einige der Dinge, die Kudu bietet sind:

-   umgebungseinstellungen für eine Anwendung
-   Log-stream
-   Diagnose Abbild
-   Debuggen Sie Console in der Powershell-Cmdlets und Basisbefehlen DOS ausgeführt werden kann.


Ein anderes nützliches Feature von Kudu besteht darin, für den Fall, dass eine Anwendung Auslösen von Ausnahmen der ersten Chance ist, Sie Kudu können und das Tool SysInternals Procdump Arbeitsspeicher zu erstellen sichert. Diese Weise Arbeitsspeicher sind Momentaufnahmen des Prozesses und häufig Ihnen helfen können kompliziertere Probleme mit der Web-app zu behandeln.

Weitere Informationen zu den Features, die in Kudu verfügbar finden Sie unter [Onlinetools Azure-Websites, die, denen Sie kennen sollten](/blog/windows-azure-websites-online-tools-you-should-know-about/).

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

 ![Starten Sie die app, um Sie zu lösen HTTP-Fehler 502 Ungültiger Gateway und 503 Dienst nicht verfügbar](./media/app-service-web-troubleshoot-HTTP-502-503/2-restart.png)

Sie können auch mithilfe der Powershell Azure Web app verwalten. Weitere Informationen finden Sie unter [Verwenden von Azure PowerShell Azure Ressourcenmanager](../powershell-azure-resource-manager.md).

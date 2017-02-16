<properties 
    pageTitle="Problembehandlung bei Analytics - die leistungsfähige Suchfunktion der Anwendung Einsichten | Microsoft Azure" 
    description="Probleme mit der Anwendung Einsichten Analytics? Beginnen Sie hier. " 
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
    ms.date="07/11/2016" 
    ms.author="awills"/>


# <a name="troubleshoot-analytics-in-application-insights"></a>Behandeln von Problemen mit Analytics in Anwendung Einsichten


Probleme mit der [Anwendung Einsichten Analytics](app-insights-analytics.md)? Beginnen Sie hier. Analytics ist die leistungsfähige Suchfunktion des Visual Studio-Anwendung Einsichten.



## <a name="limits"></a>Grenzwerte

* Gegenwärtig sind Abfrageergebnisse auf über eine Woche vergangenen Datenseite nur eingeschränkt.
* Wir testen auf Browser: neuesten Editionen von Chrome, Kante und Internet Explorer.


## <a name="known-incompatible-browser-extensions"></a>Bekannte inkompatiblen Browser extensions

* Ghostery

Deaktivieren Sie die Erweiterung oder verwenden Sie einen anderen Browser.


##<a name="a-namee-aa-unexpected-error"></a><a name="e-a"></a>"Unerwarteter Fehler"

![Unerwarteter Fehlerbildschirm](./media/app-insights-analytics-troubleshooting/010.png)

Interner Fehler beim Portal Runtime – Ausnahmefehler.

* Bereinigen Sie den Cache des Browsers ein. 

## <a name="a-namee-ba403--please-try-to-reload"></a><a name="e-b"></a>403... versuchen Sie es bitte erneut laden

![403... versuchen Sie es bitte erneut laden](./media/app-insights-analytics-troubleshooting/020.png)

Eine Authentifizierung verwandten Fehler (während der Authentifizierung oder während Access token Generation). Im Portal möglicherweise keine Möglichkeit, ohne die Browsereinstellungen ändern wiederherzustellen.

* Vergewissern Sie sich im Browser [Cookies von Drittanbietern aktiviert sind](#cookies) . 


## <a name="a-nameauthenticationa403--verify-security-zone"></a><a name="authentication"></a>403... Sicherheitszone überprüfen

![403... .verify Sicherheitszone](./media/app-insights-analytics-troubleshooting/030.png)

Eine Authentifizierung verwandten Fehler (während der Authentifizierung oder während Access token Generation). Im Portal möglicherweise keine Möglichkeit, ohne die Browsereinstellungen ändern wiederherzustellen.

1. Vergewissern Sie sich im Browser [Cookies von Drittanbietern aktiviert sind](#cookies) . 

2. Haben Sie zum Öffnen des Portals Analytics Favoriten, Textmarke oder gespeicherte Verknüpfung verwendet? Sind Sie angemeldet mit anderen Anmeldeinformationen als Sie verwendet, wenn Sie den Link gespeichert?

2. Versuchen Sie es mit einer privaten/Incognito Browserfenster (nach dem schließen alle solche Fenster). Sie müssen Ihre Anmeldeinformationen ein. 

2. Öffnen einer anderen (normalen) Browserfenster, und wechseln Sie zur [Azure](https://portal.azure.com). Melden Sie sich ab. Öffnen Sie Ihre Link und melden Sie sich mit den richtigen Anmeldeinformationen.

2. Rand und Internet Explorer-Benutzer können auch dieser Fehler zurückgegeben, wenn vertrauenswürdigen Zone Einstellungen nicht unterstützt werden.

    Überprüfen Sie, ob sowohl [Analytics-Portal](https://analytics.applicationinsights.io) und [Azure-Active Directory-Portal](https://portal.azure.com) in der gleichen Sicherheitszone:

 * Öffnen Sie in Internet Explorer **Internetoptionen**"," **Sicherheit**"," **Vertrauenswürdige Sites**"," **Websites**:

    ![Hinzufügen einer Website auf vertrauenswürdige Sites Dialogfeld Internetoptionen](./media/app-insights-analytics-troubleshooting/033.png)

    Klicken Sie in der Liste Websites ist eine der folgenden URLs enthalten sind, stellen Sie sicher, dass die anderen auch enthalten sind:

    https://Analytics.applicationinsights.IO<br/>
   https://Login.microsoftonline.com<br/>
   https://Login.Windows.NET


## <a name="a-namee-da404--resource-not-found"></a><a name="e-d"></a>404... Ressource wurde nicht gefunden

![404... Ressource wurde nicht gefunden](./media/app-insights-analytics-troubleshooting/040.png)

Ressource der Anwendung wurde von Anwendung Einsichten gelöscht und nicht mehr verfügbar ist. Dies kann geschehen, wenn Sie die URL zu der Seite Analytics gespeichert.


## <a name="a-namee-ea403--no-authorization"></a><a name="e-e"></a>403... Keine Autorisierung

![403... nicht autorisiert](./media/app-insights-analytics-troubleshooting/050.png)

Sie müssen nicht über die Berechtigung zum dieser Anwendung in Analytics zu öffnen.

* Haben Sie den Link aus einer anderen Person erhalten? Bitten Sie sie, um sicherzustellen, dass Sie in der [Leser oder Mitwirkenden für diese Ressourcengruppe](app-insights-resources-roles-access-control.md)befinden.
* Speichern Sie die Verknüpfung mit anderen Anmeldeinformationen? Öffnen Sie das [Azure-Portal](https://portal.azure.com), melden Sie sich ab, und probieren Sie diesen Link erneut, die richtigen Anmeldeinformationen bereitstellen.

## <a name="a-namehtml-storagea403--html5-storage"></a><a name="html-storage"></a>403... HTML5-Speicher

Unser Portal verwendet HTML5 LocalStorage und SessionStorage.

* Chrome: Einstellungen, Datenschutz, inhaltseinstellungen.
* InternetExplorer: Internetoptionen, Registerkarte Erweitert Sicherheit, DOM-Speicher aktivieren


![403... versuchen, die HTML5-Speicher aktivieren](./media/app-insights-analytics-troubleshooting/060.png)

## <a name="a-namee-ga404--subscription-not-found"></a><a name="e-g"></a>404... Abonnement wurde nicht gefunden


![404... Abonnement wurde nicht gefunden](./media/app-insights-analytics-troubleshooting/070.png)

Die URL ist ungültig. 

* Öffnen Sie die app-Ressource [Anwendung Einsichten](https://portal.azure.com)-Portal an. Verwenden Sie dann die Schaltfläche Analytics ein.

## <a name="a-namee-ha404--page-doesnt-exist"></a><a name="e-h"></a>404... Seite nicht vorhanden.

![404... Seite ist nicht vorhanden.](./media/app-insights-analytics-troubleshooting/080.png)

Die URL ist ungültig.

* Öffnen Sie die app-Ressource [Anwendung Einsichten](https://portal.azure.com)-Portal an. Verwenden Sie dann die Schaltfläche Analytics ein.

## <a name="a-namecookiesaenable-third-party-cookies"></a><a name="cookies"></a>Aktivieren Sie Cookies von Drittanbietern

  Informationen Sie [zum Deaktivieren von Cookies von Drittanbietern](http://www.digitalcitizen.life/how-disable-third-party-cookies-all-major-browsers), aber beachten Sie, dass wir **Aktivieren** zu machen.

## <a name="a-namee-xaif-all-else-fails"></a><a name="e-x"></a>Wenn alle sonst schlägt fehl    

[Wenden Sie sich an uns](app-insights-get-dev-support.md).
 
[AZURE.INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]


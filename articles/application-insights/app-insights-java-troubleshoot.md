<properties 
    pageTitle="Problembehandlung bei Anwendung Einsichten in einem Java-Web-Projekt" 
    description="Problembehandlungsleitfadens - live Java-apps mit Anwendung Einsichten zu überwachen." 
    services="application-insights" 
    documentationCenter="java"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="03/01/2016" 
    ms.author="awills"/>
 
# <a name="troubleshooting-and-q-and-a-for-application-insights-for-java"></a>Problembehandlung und f & A für Anwendung Einsichten für Java

Fragen oder Probleme mit [Visual Studio Anwendung Einsichten in Java][java]? Hier sind einige Tipps.


## <a name="build-errors"></a>Erstellen von Fehlern

*Klicken Sie in "Ellipse", wenn die Anwendung Einsichten SDK über Maven oder Gradle hinzufügen erhalte ich erstellen oder Prüfsumme Überprüfungsfehler.*

* Wenn die Abhängigkeit <version> Element ist ein Muster mit Platzhalterzeichen verwenden (z. B. (Maven) `<version>[1.0,)</version>` oder (Gradle) `version:'1.0.+'`), versuchen Sie, eine bestimmte Version stattdessen angeben, wie `1.0.2`. Finden Sie unter das [Freigeben von Notizen](https://github.com/Microsoft/ApplicationInsights-Java#release-notes) für die neueste Version.

## <a name="no-data"></a>Keine Daten 

*Ich habe Anwendung Einsichten erfolgreich hinzugefügt und ist meine app, aber ich habe nie gesehen Daten im Portal.*

* Warten Sie eine Minute, und klicken Sie auf aktualisieren. Die Diagramme selbst regelmäßig aktualisieren, aber Sie können auch manuell aktualisieren. Das Aktualisierungsintervall hängt von den Zeitbereich des Diagramms.
* Überprüfen Sie, ob ein Instrumentation Key in der Datei ApplicationInsights.xml (im Ressourcenordner im Projekt) definiert
* Stellen Sie sicher, dass es ist keine `<DisableTelemetry>true</DisableTelemetry>` Knoten in der XML-Datei.
* In Ihrer Firewall müssen Sie TCP-Ports 80 und 443 für ausgehenden Datenverkehr an dc.services.visualstudio.com zu öffnen. Finden Sie die [vollständige Liste der Firewallausnahmen](app-insights-ip-addresses.md)
* Starten Sie in der Microsoft Azure Brett, schauen Sie sich die Service Status Karte. Wenn es einige Angaben benachrichtigen gibt, warten Sie, bis er, klicken Sie auf OK zurückgegeben haben, und klicken Sie dann schließen und erneut Ihrer Anwendung Einsichten Anwendung Blade öffnen.
* Aktivieren der Protokollierung im Fenster IDE Konsole durch Hinzufügen einer `<SDKLogger />` Element unter dem Stammknoten in der ApplicationInsights.xml-Datei (in den Ressourcenordner im Projekt), und aktivieren Sie für Einträge [Fehler] vorangestellt.
* Stellen Sie sicher, dass die richtige ApplicationInsights.xml Datei anhand der Konsole Ausgabenachrichten für eine Anweisung "Konfigurationsdatei wurde erfolgreich gefunden" erfolgreich vom Java SDK, geladen wurde.
* Wenn die Config-Datei nicht gefunden wird, überprüfen Sie die Ausgabenachrichten angezeigt, in dem die Datei Config gesucht wird, und stellen Sie sicher, dass die ApplicationInsights.xml sich in einem der beiden Suchspeicherorte befindet. Eine Faustregel können Sie die Datei Config in der Nähe der Anwendung Einsichten SDK JARs platzieren. Beispiel: Dies bedeutet in Tomcat, den WEB-Info/Bibliothek Ordner.



#### <a name="i-used-to-see-data-but-it-has-stopped"></a>Ich verwendet, um die Daten anzuzeigen, aber es wurde beendet

* Überprüfen Sie den [Status Blog](http://blogs.msdn.com/b/applicationinsights-status/).
* Haben Sie Ihre monatliche Kontingent von Datenpunkten erreicht? Öffnen Sie die Einstellungen/Kontingent und Preise, um herauszufinden. Ist dies der Fall ist, können Sie Ihr Plan aktualisieren oder zusätzliche Kapazität bezahlen. Finden Sie die [Preise Farbschema](https://azure.microsoft.com/pricing/details/application-insights/)aus.

#### <a name="i-dont-see-all-the-data-im-expecting"></a>Kann ich sehen nicht alle Daten, die ich erhalte erwartet

* Öffnen Sie die Kontingente und Preise Blade- und überprüfen, ob [werden](app-insights-sampling.md) in der Vorgang ist ein. (100 % Übertragung weist darauf hin, dass bei Stichproben nicht ist.) Der Anwendung Einsichten Dienst kann festgelegt werden, um nur einen Teil der telemetrieprotokoll zu akzeptieren, die von der app eintreffen. Auf diese Weise können Sie innerhalb der monatliche Kontingent von werden beibehalten. 

## <a name="no-usage-data"></a>Keine Verwendungsdaten

*Die Daten zu Anfragen und Reaktionszeiten, aber keine Seitenansicht, Browser oder Benutzerdaten angezeigt.*

Sie richten Sie erfolgreich Ihre app werden vom Server zu senden. Nun im nächste Schritt zum [Einrichten von Ihren Webseiten werden im Webbrowser senden]gibt[usage].

Sie können auch Ihren Kunden ist eine app in einem [Telefon oder ein anderes Gerät][platforms], Sie werden von dort aus senden können. 

Verwenden Sie die gleichen Instrumentation-Taste zum Einrichten Ihrer Client- und werden. Die Daten in der gleichen Anwendung Einsichten Ressource angezeigt werden, und zwar Ereignisse von Client und Server zu koordinieren können.



## <a name="disabling-telemetry"></a>Deaktivieren von telemetrieprotokoll

*Wie kann ich werden Websitesammlung deaktivieren?*

Code:

    TelemetryConfiguration config = TelemetryConfiguration.getActive();
    config.setTrackingIsDisabled(true);


**Oder** 

Aktualisieren von ApplicationInsights.xml (in den Ressourcenordner im Projekt). Fügen Sie unter dem Stammknoten Folgendes ein:

    <DisableTelemetry>true</DisableTelemetry>

Mithilfe der XML-Methode, müssen Sie die Anwendung neu starten, wenn Sie den Wert ändern.

## <a name="changing-the-target"></a>Ändern des Ziels

*Wie kann ich ändern, welche Azure Ressource Mein Projekt Daten sendet?*

* [Abrufen des Instrumentation Schlüssels der neuen Ressource an.][java]
* Wenn Sie Anwendung Einsichten dem Projekt hinzugefügt, mit dem Azure-Toolkit für "Ellipse", nach rechts Webprojekt klicken, **Azure**, **Anwendung Einsichten konfigurieren**, wählen Sie aus und ändern Sie die Taste.
* Aktualisieren Sie andernfalls den Schlüssel in ApplicationInsights.xml in den Ressourcenordner im Projekt.


## <a name="the-azure-start-screen"></a>Der Azure-Startbildschirm

*Ich bin [Azure-Portal](https://portal.azure.com)betrachten. Teilt die Karte eines Beitrags zu meiner app mir?*

Nein, wird die Integrität des Azure-Servern auf der ganzen Welt angezeigt.

*Finden vom Brett Azure Start (Startbildschirm) wie lassen sich Daten zu meiner app?*

Vorausgesetzt, [Richten Sie Ihre app für die Anwendung Einsichten][java], klicken Sie auf Durchsuchen, wählen Sie die Anwendung Einsichten, und wählen Sie die app-Ressource, die Sie für Ihre app erstellt haben. Zum Abrufen können es schneller in Zukunft Sie Ihre app an der Karte Start anheften.

## <a name="intranet-servers"></a>Intranet-Server

*Können einen Server auf Meine Intranet werden überwacht?*

Ja, sofern, Ihr Server werden mit dem Portal Anwendung Einsichten über das öffentliche Internet senden kann. 

Klicken Sie in Ihrer Firewall müssen Sie TCP-Ports 80 und 443 für ausgehenden Datenverkehr dc.services.visualstudio.com und f5.services.visualstudio.com zu öffnen.

## <a name="data-retention"></a>Beibehaltung der Daten 

*Wie lange bleibt die Daten im Portal? Ist es sicher?*

Finden Sie unter [Beibehaltung der Daten und Ihre Privatsphäre][data].

## <a name="next-steps"></a>Nächste Schritte

*Anwendung Einsichten für meine Java-Server-app eingerichtet. Was kann ich tun?*

* [Überwachen der Verfügbarkeit von Webseiten][availability]
* [Überwachen Sie die Verwendung der Webseite][usage]
* [Protokollieren Sie der Verwendung und diagnostizieren Sie Probleme in Ihrem Gerät apps][platforms]
* [Schreiben von Code, um die Verwendung der app zu beobachten][track]
* [Erfassen von Diagnoseprotokollen][javalogs]


## <a name="get-help"></a>Anfordern von Hilfe

* [Stapelüberlauf](http://stackoverflow.com/questions/tagged/ms-application-insights)

<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[data]: app-insights-data-retention-privacy.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[platforms]: app-insights-platforms.md
[track]: app-insights-api-custom-events-metrics.md
[usage]: app-insights-web-track-usage.md

 
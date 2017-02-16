<properties
    pageTitle="Überwachen der Apps in Azure App-Verwaltungsdienst"
    description="Informationen Sie zum Überwachen der Apps in Azure-App-Verwaltungsdienst mithilfe der Azure-Portal."
    services="app-service"
    documentationCenter=""
    authors="btardif"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/07/2016"
    ms.author="byvinyal"/>

# <a name="how-to-monitor-apps-in-azure-app-service"></a>So: Überwachen der Apps in Azure App-Verwaltungsdienst

[App-Dienst](http://go.microsoft.com/fwlink/?LinkId=529714) bietet integrierten Überwachungsfunktionen im [Azure-Portal](https://portal.azure.com)an.
Dies umfasst die Möglichkeit, **Kontingente** und **Kennzahlen** für eine app als auch der App-Serviceplan, Einrichten von **Benachrichtigungen** und sogar **dieselbe Skalierung** basierend auf folgenden Kriterien automatisch zu überprüfen.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="understanding-quotas-and-metrics"></a>Grundlegendes zu Kontingenten und Kriterien

### <a name="quotas"></a>Kontingente

Applications in der App-Dienst gehostet werden bestimmten *Grenzwerte* für Ressourcen, die sie verwenden können. **App-Serviceplan** der app zugeordnet sind die Grenzwerte definiert.

Wenn die Anwendung in einem Plan **Free** oder **freigegebene** gehostet wird, sind die Grenzwerte für die Ressourcen, die die app verwenden, kann von **Kontingenten**definiert.

Wenn die Anwendung in **einfache**, **Standard** oder **Premium** Plan, und klicken Sie dann die Grenzwerte gehostet wird, klicken Sie auf sind die Ressourcen, die sie verwenden können, indem Sie die **Größe** (klein, Mittel, groß) und die **Anzahl der Instanzen** (1, 2, 3,...) des **App-Serviceplan**festgelegt.

**Kontingente** für **Free** oder **freigegebene** apps sind:

* **CPU(short)**
   * Umfang der CPU für diese Anwendung in einer 3-Minuten-Intervall zulässig. Dieses Kontingent legt erneut alle 3 Minuten.
* **CPU(Day)**
   * Gesamtbetrag der CPU für diese Anwendung in einem Tag zulässig. Dieses Kontingent legt erneut alle 24 Stunden Mitternacht UTC.
* **Arbeitsspeicher**
   * Die Gesamtmenge des Arbeitsspeichers für diese Anwendung zulässig.
* **Bandbreite**
   * Gesamtmenge ausgehende Bandbreite für diese Anwendung in einem Tag zulässig.
   Dieses Kontingent legt erneut alle 24 Stunden Mitternacht UTC.
* **Dateisystem**
   * Gesamtbetrag der zulässigen Speicherplatz.

Nur Kontingents anwendbar apps auf **einfache**, **Standard-** und **Premium** -Pläne gehostet ist **Dateisystem**.

Weitere Informationen zu bestimmten Kontingente, Grenzwerte und Features verfügbar, um die verschiedenen App-Dienst SKUs finden Sie hier: [Azure Abonnement Dienst Beschränkungen](../azure-subscription-service-limits.md#app-service-limits)

#### <a name="quota-enforcement"></a>Speicherkontingent Durchsetzung

Überschreitet eine Anwendung in die Verwendung der **CPU (short)**, wird **CPU (Tag)**oder **Bandbreite** Kontingent klicken Sie dann die Anwendung angehalten, bis das Kontingent erneut legt fest. Während dieses Zeitraums führt eine **HTTP 403**alle eingehende Anfragen.
![][http403]

Wenn das Anwendung **Arbeitsspeicher** Kontingent überschritten wird, wird die Anwendung neu gestartet werden.

Wenn Sie das **Dateisystem** -Kontingent überschritten wird, schreiben einen Vorgang schlägt fehl, dies umfasst das Schreiben in Protokolle.

Kontingente können erhöht oder Upgrade Ihrer App Serviceplan aus der app entfernt werden.

### <a name="metrics"></a>Kennzahlen

**Kennzahlen** finden Sie Informationen über die app oder Verhalten der App-Service-Plan.

Für eine **Anwendung**sind die Metrik zur Verfügung:

* **Durchschnittliche Reaktionszeiten**
   * Die durchschnittliche Zeit für die app Besprechungsanfragen in ms dienen.
* **Durchschnittliche Arbeitsspeicher arbeiten festlegen**
   * Die durchschnittliche Speichermenge MiBs von der app verwendet.
* **CPU-Zeit**
   * Der Umfang der CPU in Sekunden von der app verbraucht. Weitere Informationen zu diesem metrischen finden Sie unter: [CPU-Zeit im Vergleich mit einer CPU-Prozentsatz](#cpu-time-vs-cpu-percentage)
* **Daten In**
   * Der Umfang der eingehende Bandbreite, die die app in MiBs.
* **Daten ab**
   * Der Umfang der ausgehende Bandbreite, die die app in MiBs.
* **HTTP-2xx**
   * Anzahl der Anfragen in einer http-Code, der sich ergebenden > = 200 aber < 300.
* **HTTP-3xx**
   * Anzahl der Anfragen in einer http-Code, der sich ergebenden > = 300 aber < 400.
* **HTTP 401**
   * Anzahl der Anfragen resultierender HTTP 401 Status Code.
* **HTTP 403**
   * Anzahl der Anfragen resultierender HTTP 403 Status Code.
* **HTTP 404**
   * Anzahl der Anfragen resultierender HTTP 404 Status Code.
* **HTTP 406**
   * Anzahl der Anfragen resultierender HTTP 406 Status Code.
* **HTTP-4xx**
   * Anzahl der Anfragen in einer http-Code, der sich ergebenden > = 400 aber < 500.
* **Fehler bei der HTTP-Server**
   * Anzahl der Anfragen in einer http-Code, der sich ergebenden > = 500 aber < 600.
* **Arbeitsseite Arbeitsspeicher**
   * Aktuelle Speichermenge von der app in MiBs verwendet.
* **Besprechungsanfragen**
   * Die Gesamtzahl der Anfragen unabhängig von deren resultierende HTTP-Statuscode.

Für eine **App-Serviceplan**sind die Metrik zur Verfügung:

>[AZURE.NOTE] Kriterien für App-Service-Plan sind nur für Pläne in **einfache**, **Standard-** und **Premium** SKU verfügbar.

* **Prozentsatz der CPU**
   * Die durchschnittliche CPU-Verwendung über alle Instanzen des Plans.
* **Arbeitsspeicher Prozentsatz**
   * Der Mittelwert für alle Instanzen des Plans verwendete Speicherplatz.
* **Daten In**
   * Die durchschnittliche eingehende Bandbreite über alle Instanzen von den Plan verwendet.
* **Daten ab**
   * Der Mittelwert ausgehende Bandbreite über alle Instanzen von den Plan verwendet wird.
* **Warteschlange**
   * Durchschnittliche Anzahl der lesen sowohl Speicher beschriftet Besprechungsanfragen, die in der Warteschlange hatten. Eine hohe Warteschlange ist Angabe der Anwendung, die aufgrund eines übermäßige Datenträger e/a verlangsamt werden kann.
* **HTTP-Warteschlange Länge**
   * Durchschnittliche Anzahl der HTTP-Anfragen, die in der Warteschlange ungefähr, bevor Sie gerade erledigt aufwiesen. Eine hohe oder zunehmende HTTP-Länge ist ein auftretendes Problem eines Plans bei hoher Auslastung.

### <a name="cpu-time-vs-cpu-percentage"></a>CPU-Zeit im Vergleich mit einer CPU-Prozentsatz
<!-- To do: Fix Anchor (#CPU-time-vs.-CPU-percentage) -->

Es gibt 2 Kennzahlen, die CPU-Auslastung wiedergeben. **CPU-Zeit** und den **Prozentsatz der CPU**

**CPU-Zeit** eignet sich für apps **kostenlos** oder **freigegebene** Pläne gehostet werden, da ein ihrer Quote in der app verwendeten CPU-Minuten definiert ist.

**Prozentsatz der CPU** eignet sich andererseits für apps in **grundlegende**, **Standard-** und **Premium** -Pläne gehostet werden, da sie sich skaliert werden können und diese Metrik ein guter Indikator für die Verwendung des über alle Instanzen ist.

##<a name="metrics-granularity-and-retention-policy"></a>Kennzahlen Genauigkeit und Aufbewahrungsrichtlinie

Kriterien für eine Anwendung und der app-Serviceplan werden protokolliert und vom Dienst mit den folgenden Granularität der und Aufbewahrungsrichtlinien zusammengefasster:

 * **Minute** Genauigkeit Kennzahlen verbleiben für **48 Stunden**
 * **Stunde** Genauigkeit Kennzahlen werden **30** Tage lang aufbewahrt
 * **Tag** Genauigkeit Kennzahlen werden **90** Tage lang aufbewahrt

## <a name="monitoring-quotas-and-metrics-in-the-azure-portal"></a>Überwachen von Kontingenten und Kennzahlen Azure-Portal an.

Sie können den Status der verschiedenen **Kontingente** und **Kennzahlen** Auswirkungen der Anwendung im [Portal Azure](https://portal.azure.com)überprüfen.

![][quotas]
**Kontingente** finden Sie unter Einstellungen >**Kontingente**. Bedienen ermöglicht es Ihnen, um zu überprüfen: (1) die Namen von Kontingenten, (2) seine Intervall zurücksetzen, (3) seine aktuelle Begrenzung und (4) der aktuelle Wert.

![][metrics]
**Kennzahlen** können Zugriff direkt aus dem Ressource Blade sein. Sie können auch das Diagramm durch Anpassen: (1) **Klicken Sie auf** , und wählen Sie (2) **Diagramm bearbeiten**.
Hier können Sie **Zeitraums**(3) und (4) **Diagrammtyp**(5) **Kennzahlen** anzuzeigenden ändern.  

Weitere Informationen finden Sie Informationen zu den hier Kennzahlen: [Kriterien für Monitor Service](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

## <a name="alerts-and-autoscale"></a>Benachrichtigungen und automatisch skalieren
Kriterien benachrichtigt für ein App oder einen bestimmten Dienst App Plan bis zu eingebunden werden soll, kann, zum Weitere Informationen hierzu finden Sie unter [-Benachrichtigung erhalten](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

App-Service-apps in einfache, standard oder Premium App Service-Pläne gehostet unterstützen **automatisch skalieren**. Dadurch können Sie Regeln, die die Kriterien der App-Service-Plan überwachen und können erhöhen oder verringern die Anzahl der Instanzen zusätzliche Ressourcen bereitstellen, je nach Bedarf oder Geld speichern, wenn die Anwendung übermäßige bereitstellen ist. Weitere Informationen finden Sie Informationen zu den hier automatisch skalieren: [So skalieren](../monitoring-and-diagnostics/insights-how-to-scale.md) und hier [bewährte Methoden für die Azure Monitor automatische Skalierung](../monitoring-and-diagnostics/insights-autoscale-best-practices.md)

>[AZURE.NOTE] Wenn Sie mit Azure-App-Verwaltungsdienst Schritte vor dem für ein Azure-Konto anmelden möchten, wechseln Sie zu [App-Verwaltungsdienst versuchen](http://go.microsoft.com/fwlink/?LinkId=523751), in dem Sie eine kurzlebige Starter Web app sofort im App-Dienst erstellen können. Keine Kreditkarten erforderlich; keine Zusagen.

## <a name="whats-changed"></a>Was hat sich geändert
* Ein Leitfaden zum Ändern von Websites-App-Dienst finden Sie unter: [Azure-App-Dienst und seinen Einfluss auf die vorhandenen Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)

[fzilla]:http://go.microsoft.com/fwlink/?LinkId=247914
[vmsizes]:http://go.microsoft.com/fwlink/?LinkID=309169



<!-- Images. -->
[http403]: ./media/web-sites-monitor/http403.png
[quotas]: ./media/web-sites-monitor/quotas.png
[metrics]: ./media/web-sites-monitor/metrics.png

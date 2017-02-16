<properties 
    pageTitle="Festlegen von Benachrichtigungen in Anwendung Einsichten | Microsoft Azure" 
    description="Benachrichtigung zu längeren Reaktionszeiten, Ausnahmen, und andere Leistung oder Verwendung von Änderungen im Web app." 
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
    ms.date="10/14/2016" 
    ms.author="awills"/>
 
# <a name="set-alerts-in-application-insights"></a>Festlegen von Benachrichtigungen in Anwendung Einsichten

[Visual Studio-Anwendung Einsichten] [ start] können Sie Änderungen in der Leistung oder Einsatz Kennzahlen im Web app benachrichtigen. 

Anwendung Einsichten überwacht Ihre live-app auf einer [Vielzahl von Plattformen] [ platforms] , damit Sie Leistungsprobleme diagnostizieren und Verwendungsmustern zu verstehen.

Es gibt drei Arten von Benachrichtigungen aus:

* **Metrische Benachrichtigungen** informieren Sie bei eine Metrik einen Schwellenwert für einen bestimmten Zeitraum – beispielsweise Reaktionszeiten, Ausnahme zählt, CPU-Auslastung oder Seitenansichten überschreitet. 
* [**Web Tests** ] [ availability] informieren, wenn Ihre Website im Internet nicht verfügbar oder reagiert langsam ist. [Erfahren Sie mehr][availability].
* [**Proaktive Diagnose**](app-insights-proactive-diagnostics.md) sind so konfiguriert, dass automatisch zu ungewöhnliche Leistung Mustern benachrichtigt.

Wir möchten metrischen Benachrichtigungen in diesem Artikel.

## <a name="set-a-metric-alert"></a>Festlegen einer Benachrichtigung metrischen

Öffnen Sie das Blade Warnungsregeln, und verwenden Sie dann auf die Schaltfläche hinzufügen. 

![Wählen Sie das Blade Warnungsregeln Alert hinzufügen aus. Legen Sie Ihre app als Ressource messen, geben Sie einen Namen für die Warnung, und wählen Sie eine Metrik ein.](./media/app-insights-alerts/01-set-metric.png)

* Festlegen der Ressource, bevor Sie die anderen Eigenschaften an. **Auswählen der Ressource "(Komponenten)"** , wenn Sie Benachrichtigungen auf der Leistung oder Einsatz Kennzahlen festlegen möchten.
* Der Name, mit denen Sie auf die Benachrichtigung muss innerhalb der Ressourcengruppe (nicht nur die Anwendung) eindeutig sein.
* Achten Sie darauf, dass die Einheiten zu beachten, in denen Sie aufgefordert werden, den Schwellenwert eingeben.
* Wenn Sie das Kontrollkästchen "E-Mail-Besitzer..." aktivieren, werden Benachrichtigungen für jede Person per e-Mail gesendet werden, die auf dieser Ressourcengruppe zugreifen. Wenn Sie um diese Reihe von Personen zu erweitern, der [Ressourcengruppe oder das Abonnement](app-insights-resources-roles-access-control.md) (nicht die Ressource) hinzuzufügen.
* Wenn Sie "Weitere e-Mails" angeben, werden Benachrichtigungen gesendet werden, auf die Personen oder Gruppen (unabhängig davon, ob Sie das Feld "e-Mail-Besitzer..." aktiviert). 
* Legen Sie eine [Webhook Adresse](../monitoring-and-diagnostics/insights-webhooks-alerts.md) , wenn Sie eine Web app eingerichtet haben, die auf Benachrichtigungen reagiert. Es wird aufgerufen, wenn die Benachrichtigung aktiviert ist (die ausgelöst wird,) und wenn es gelöst ist. (Beachten Sie aber, dass bei präsentieren Abfrageparameter nicht durch als Webhook Eigenschaften weitergegeben werden.)
* Sie können deaktivieren oder aktivieren Sie die Benachrichtigung: finden Sie unter den Schaltflächen im Kopfbereich des Blades.

*Die Benachrichtigung hinzufügen Schaltfläche wird nicht angezeigt.* 

- Verwenden Sie ein Organisations-Konto? Wenn Sie Besitzer oder Mitwirkender Zugriff auf diese Anwendungsressource verfügen, können Sie Benachrichtigungen festlegen. Sehen Sie sich das Access Control Blade. [Erfahren Sie mehr über die Steuerung des Benutzerzugriffs][roles].

> [AZURE.NOTE] In das Blade Benachrichtigungen wird angezeigt, dass es bereits eine Benachrichtigung Festlegen von: [Proaktive Diagnose](app-insights-proactive-failure-diagnostics.md). Dies ist eine automatische Benachrichtigung, die einem bestimmten metrischen, Anforderungsfehlerrate überwacht. Es sei denn, Sie die proaktive Benachrichtigung deaktivieren möchten, müssen Sie nicht eigene Benachrichtigung auf Anforderungsfehlerrate festlegen. 

## <a name="see-your-alerts"></a>Die Benachrichtigungen finden Sie unter

Sie erhalten eine e-Mail-Nachricht, wenn eine Benachrichtigung ändert den Zustand zwischen inaktiv und aktiv. 

Das Blade Warnungsregeln wird im aktuelle Zustand jeder Warnung angezeigt.

Es wird eine Zusammenfassung der aktuellen Aktivitäten Benachrichtigungen Dropdown:

![Benachrichtigungen Dropdown-Liste](./media/app-insights-alerts/010-alert-drop.png)

Des Verlaufs von Änderungen der Status wird in der Aktivität Log:

![Klicken Sie auf das Blade Übersicht auf Einstellungen, Überwachungsprotokolle](./media/app-insights-alerts/09-alerts.png)



## <a name="how-alerts-work"></a>Funktionsweise von Benachrichtigungen

* Eine Warnung weist drei Zustände: "Nicht aktiviert", "Aktiviert" und "Gelöst". Aktivierten bedeutet, dass die Bedingung, die Sie angegeben haben wurde true, wenn sie als letzte ausgewertet wurde.

* Eine Benachrichtigung wird ausgelöst, wenn eine Warnung Zustand ändert. (Wenn die benachrichtigen Bedingung bereits wahr ist, wenn Sie die Benachrichtigung erstellt haben, erhalten Sie möglicherweise keine Benachrichtigung, bis die Bedingung falsch wechselt.)

* Jede Benachrichtigung generiert eine e-Mail-Nachricht an, wenn Sie das Kontrollkästchen e-Mails aktiviert oder e-Mail-Adressen bereitgestellt. Sie können auch die Dropdownliste Benachrichtigungen anzeigen.

* Eine Warnung wird jedes Mal ausgewertet, wenn eine Metrik, aber nicht andernfalls eintrifft.

* Auswertung aggregiert die Metrik über den vorherigen Zeitraum, und klicken Sie dann mit den Schwellenwert für den neuen Status des verglichen.

* Periode, die Sie auswählen, das Intervall gibt an, über die sind Kennzahlen aggregiert. Es hat keine Auswirkung auf, wie oft die Benachrichtigung ausgewertet wird: Dies hängt von der Häufigkeit von Eintreffen der Kennzahlen.

* Wenn keine Daten für eine bestimmte Metrik für einige Zeit eintrifft, weist die Lücke unterschiedliche Effekte auf benachrichtigen Auswertung und auf die Diagramme in metrischen Explorer. In metrischen Explorer keine Daten für mehr als Stichprobe-Intervall des Diagramms, sichtbar ist, wird das Diagramm einen Wert von 0. Eine Benachrichtigung basierend auf dieselbe Metrik, nicht jedoch erneut ausgewertet, und die Benachrichtigung für den Bundesstaat bleibt unverändert. 

    Wenn Daten später ankommt, springt das Diagramm zurück zu einem Wert ungleich NULL. Wertet die Benachrichtigung basierend auf den Daten, die für die angegebene Periode zur Verfügung. Wenn der neuen Datenpunkt der einzige, der in den Zeitraum verfügbar ist, basiert das Aggregat nur aktivieren, zeigen Sie die Daten.

* Eine Benachrichtigung kann häufig zwischen benachrichtigen und fehlerfrei Staaten Flackern, auch wenn Sie über längere Zeiträume festlegen. Dies kann geschehen, wenn der metrische Wert um den Schwellenwert bewegt wird. Es gibt keine Hysterese in den oberen Schwellenwert: der Übergang zur Benachrichtigung bei den gleichen Wert wie der Übergang fehlerfrei geschieht.



## <a name="what-are-good-alerts-to-set"></a>Was sind eine gute Benachrichtigungen festlegen?

Das hängt von der Anwendungs ab. Start mit, empfiehlt es sich nicht die zu viele Kennzahlen. Einige Zeit metrischen Diagramme betrachtet, während die app ausgeführt wird, um verschaffen Sie sich einen Eindruck für wie gewohnt verhält. Dies hilft Ihnen finden Methoden, um die Leistung zu verbessern. Klicken Sie dann richten Sie Benachrichtigungen, können Sie feststellen, wenn die Metrik außerhalb der normalen Zone wechseln. 

Beliebte Alarme:

* [Kennzahlen Browser][client], insbesondere von Browser **Seitenladezeiten**, eignen sich für Webanwendungen. Wenn Ihre Seite viele Skripts wurden, sollten Sie für **Browser Ausnahmen**Achten Sie auf. Damit diese Kennzahlen und Benachrichtigungen erhalten möchten, müssen Sie [für die Überwachung Webseite]einrichten[client].
* Für das serverseitige Webanwendungen **Server Antwortzeit** . Sowie Einrichten von Benachrichtigungen, achten Sie auf diese Metrik, um festzustellen, ob unproportional mit hoher Anforderung Sätzen variiert:, die möglicherweise darauf hinzuweisen, dass die app nicht mehr genügend Ressourcen ausgeführt wird. 
* **Serverausnahmen** - sehen können, müssen Sie nur einige [zusätzliche einrichten](app-insights-asp-net-exceptions.md).

Vergessen Sie nicht, diese [proaktive Fehler Zins Diagnose](app-insights-proactive-failure-diagnostics.md) automatisch die Rate, Ihre app Anfragen mit Fehlercodes beantwortet, überwachen. 

## <a name="automation"></a>Automatisierung

* [Einrichten von Benachrichtigungen automatisieren mithilfe von PowerShell](app-insights-powershell-alerts.md)
* [Verwenden von Webhooks zum Automatisieren von Benachrichtigungen beantworten](../monitoring-and-diagnostics/insights-webhooks-alerts.md)

## <a name="see-also"></a>Siehe auch

* [Verfügbarkeit von Webtests](app-insights-monitor-web-app-availability.md)
* [Einrichten von Benachrichtigungen automatisieren](app-insights-powershell-alerts.md)
* [Proaktive Diagnose](app-insights-proactive-diagnostics.md) 



<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[client]: app-insights-javascript.md
[platforms]: app-insights-platforms.md
[roles]: app-insights-resources-roles-access-control.md
[start]: app-insights-overview.md

 
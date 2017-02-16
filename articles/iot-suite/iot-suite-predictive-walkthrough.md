<properties
 pageTitle="Vorhersagen Wartung Exemplarische Vorgehensweise | Microsoft Azure"
 description="Eine exemplarische Vorgehensweise der Azure IoT Vorhersage Wartung vorkonfiguriert Lösung."
 services=""
 suite="iot-suite"
 documentationCenter=""
 authors="aguilaaj"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-suite"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/17/2016"
 ms.author="araguila"/>

# <a name="predictive-maintenance-preconfigured-solution-walkthrough"></a>Vorhersagen Wartung vorkonfiguriert Lösung Exemplarische Vorgehensweise

## <a name="introduction"></a>Einführung

Die Lösung IoT Suite Vorhersage Wartung vorkonfiguriert ist eine End-to-End-Lösung für ein Business-Szenario, das den Punkt geschätzt, wenn Fehler zu rechnen ist. Sie können diese vorkonfigurierten Lösung für Aktivitäten wie optimieren Wartung vorausschauende verwenden. Die Lösung kombiniert Key Azure IoT Suite Dienste einschließlich einer [Azure maschinellen Learning] [ lnk_machine_learning] Arbeitsbereich. In diesem Arbeitsbereich enthält Versuche, basierend auf einer öffentlichen Stichprobe Datengruppe zurück, die verbleibende hilfreiche Nutzungsdauer (RUL) von einem Flugzeug-Engine Vorhersagen. Die Lösung implementiert das IoT Business-Szenario vollständig als Ausgangspunkt für Ihre Planung und Implementierung einer Lösung, die Ihren eigenen bestimmte Business Anforderungen entspricht.

## <a name="logical-architecture"></a>Logische Architektur

Das folgende Diagramm skizziert die logischen Komponenten der vorkonfigurierten Lösung:

![][img-architecture]

Die blauen Elemente sind Azure Dienste, die in den Speicherort, die Sie auswählen bereitgestellt werden, wenn Sie die vorkonfigurierte Lösung bereitstellen. Sie können die vorkonfigurierte Lösung in ostasiatischen US, North Europa oder Ostasien Region bereitstellen.

Einige Ressourcen sind in der Stelle, an der Sie die vorkonfigurierte Lösung bereitstellen Regionen nicht verfügbar. "Orange" Elemente im Diagramm darstellen der Azure-Dienste nach der Bereitstellung in der nächsten verfügbaren Region (Süd zentralen USA, Europa – Westen oder Südwesten Asien) angegebenen den ausgewählten Bereich.

Das grüne Element ist ein simuliertes Gerät, das eine Flugzeug-Engine darstellt. Sie können über diesen simulierten Geräten die folgenden Abschnitte enthalten weitere Informationen.

Die graue Elemente repräsentieren Komponenten, *die Gerät* Verwaltungsfunktionen implementieren. Die aktuelle Version der Lösung Vorhersage Wartung vorkonfiguriert stellt diese Ressourcen nicht bereit. Um weitere Informationen zur Verwaltung von Gerät finden Sie in der- [remote vorkonfiguriertes Lösung für die Überwachung][lnk-remote-monitoring].

## <a name="simulated-devices"></a>Simuliertes Geräte

In den vorkonfigurierten Lösung stellt ein simuliertes Gerät eine Flugzeug-Engine. Die Lösung wird mit zwei TTS bereitgestellt, die eine einzelne Flugzeug zuordnen. Jedes Modul gibt vier Arten von telemetrieprotokoll: Sensor 9, 11 Sensor, Sensor 14 und 15 Sensor bieten Daten für das Modell maschinellen Schulung zur Berechnung der verbleibenden hilfreiche Nutzungsdauer (RUL) für die-Engine erforderlich sind. Jedes simulierten Gerät sendet die folgenden werden Nachrichten an IoT-Hub an:

*Kreis zählen*. Ein Zyklus stellt einen fertigen Flug Variable Länge zwischen 2 bis 10 Stunden, die in denen werden Daten stündlich halbe Flug der erfasst werden.

*Werden*. Es gibt vier Sensoren, die Engine Attribute darstellen. Die Sensoren sind generisch Sensor 9, 11 Sensor, Sensor 14 und 15 Sensor gekennzeichnet. Diese 4 Sensoren darstellen werden ausreichend, um sinnvolle Ergebnisse aus dem Modell maschinellen Learning für RUL zu erhalten. Dieses Modell wird von einer öffentlichen Datenmenge erstellt, real-Engine Sensordaten enthält. Weitere Informationen darüber, wie das Modell aus dem ursprünglichen Dataset erstellt wurde, finden Sie unter der [Cortana Intelligence Katalog Vorhersage Wartung Vorlage][lnk-cortana-analytics].

Die simulierten Geräte können die folgenden Befehle, die von einem Hub IoT gesendeten behandeln:

| Befehl | Beschreibung |
|---------|-------------|
| StartTelemetry | Steuert den Status der Simulation.<br/>Startet das Gerät gesendet werden     |
| StopTelemetry  | Steuert den Status der Simulation.<br/>Stoppt das Gerät gesendet werden |

IoT Hub bietet Gerät Befehl Bestätigung.

## <a name="azure-stream-analytics-job"></a>Azure Stream Analytics Position

**Auftrag: werden** auf eingehende Gerät werden Streams mit zwei Anweisungen in Betrieb sind. Die erste markiert alle werden von den Geräten und sendet diese Daten zu BLOB-Speicher aus, in dem sie in der Web-app visualisiert ist. Die zweite Anweisung Mittelwert Sensorwerte über ein zwei-minütiges Zeitfenster berechnet und sendet diese Daten über das Ereignis Hub ein **Ereignisprozessor**.

## <a name="event-processor"></a>Ereignisprozessor

Das **Ereignisprozessor** erfolgt die durchschnittliche Sensorwerte für einen fertigen Zyklus an. Es der übergibt diese Werte an eine API, die der Computer Learning macht gelernt Modell zum Berechnen der RUL für ein Modul.

## <a name="azure-machine-learning"></a>Learning Azure-Computern

Weitere Informationen darüber, wie das Modell aus dem ursprünglichen Dataset erstellt wurde, finden Sie unter der [Cortana Intelligence Katalog Vorhersage Wartung Vorlage][lnk-cortana-analytics].

## <a name="lets-start-walking"></a>Beginnen wir durchlaufen

In diesem Abschnitt führt Sie durch die Komponenten der Lösung, werden der beabsichtigte Verwendung Groß-/Kleinschreibung und finden Sie Beispiele hierzu.

### <a name="predictive-maintenance-dashboard"></a>Vorhersagen Wartung Dashboard

Diese Seite in der Anwendung verwendet PowerBI JavaScript-Steuerelemente (finden Sie unter [PowerBI-visuelle Objekte Repository][lnk-powerbi]) visualisiert werden sollen:

- Die Ausgabedaten von den Stream Analytics Stellen im BLOB-Speicher.
- Die RUL und Zyklus zählen pro Flugzeug-Engine.

### <a name="observing-the-behavior-of-the-cloud-solution"></a>Beobachten das Verhalten der Cloudlösung

Navigieren Sie im Portal Azure der Ressourcengruppe mit dem Lösungsnamen, die, den Sie ausgewählt haben, um Ihre bereitgestellten Ressourcen anzuzeigen.

![][img-resource-group]

Wenn Sie die vorkonfigurierte Lösung bereitstellen, erhalten Sie eine e-Mail mit einem Link zu dem Computer Learning-Arbeitsbereich. Sie können auch auf den Computer Learning Arbeitsbereich navigieren, aus der [azureiotsuite.com] [ lnk-azureiotsuite] Seite für Ihre bereitgestellte Lösung, wenn sie die **bereit** ist.

![][img-machine-learning]

Im Portal Lösung können Sie sehen, dass die Stichprobe mit vier simulierten Geräten zur Darstellung von zwei Flugzeug mit zwei Module pro Flugzeug, jede mit vier Sensoren bereitgestellt wird. Wenn Sie zuerst auf das Portal Lösung navigieren, wird die Simulation angehalten.

![][img-simulation-stopped]

Klicken Sie auf **Start Simulation** , um die Simulation zu beginnen, in dem Sie den Verlauf Sensor, RUL, Zyklen, finden Sie unter und RUL Verlauf füllen das Dashboard.

![][img-simulation-running]

Wenn RUL weniger als 160 (eine zufällige Schwellenwert für die Vorführung ausgewählt) ist, wird im Portal Lösung zeigt eine Warnung Symbol neben der Anzeige RUL und die Flugzeug-Engine gelb hervorgehoben. Beachten Sie, wie die Werte RUL einen allgemeinen abwärts gerichtete Trend generelle haben, aber in der Regel nach oben oder unten springen. Dieses Verhalten ergibt sich aus der variabler Zyklus Länge und die Genauigkeit Modell.

![][img-simulation-warning]

Die vollständige Simulation dauert ungefähr 35 Minuten 148 Zyklen. Die 160 RUL Schwellenwert zum ersten Mal bei ungefähr 5 Minuten erfüllt ist, und beide TTS drücken Sie den Schwellenwert für ungefähr 8 Minuten.

Die Simulation durchläuft der vollständigen Dataset für 148 Zyklen und auf endgültige RUL und Zyklus Werten gleicht.

Sie können die Simulation zu einem beliebigen Zeitpunkt beenden, aber auf **Starten Simulation** gibt die Simulation ab dem Anfang des Datasets wieder.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie die Lösung Vorhersage Wartung vorkonfiguriert Sie möchten es ändern ausführen haben, finden Sie unter [Leitfäden zum Anpassen von vorkonfigurierten Lösungen][lnk-customize].

Im Blogbeitrag [IoT Suite - Details - Vorhersage Wartung](http://social.technet.microsoft.com/wiki/contents/articles/33527.iot-suite-under-the-hood-predictive-maintenance.aspx) TechNet bietet zusätzlichen Details zur Lösung Vorhersage vorkonfiguriert Wartung.

Sie können auch einige andere Features und Funktionen von den IoT Suite vorkonfiguriert Lösungen vertraut machen:

- [Häufig gestellte Fragen zu IoT Suite][lnk-faq]
- [IoT Sicherheit von Grund auf][lnk-security-groundup]


[img-architecture]: media/iot-suite-predictive-walkthrough/architecture.png
[img-resource-group]: media/iot-suite-predictive-walkthrough/resource-group.png
[img-machine-learning]: media/iot-suite-predictive-walkthrough/machine-learning.png
[img-simulation-stopped]: media/iot-suite-predictive-walkthrough/simulation-stopped.png
[img-simulation-running]: media/iot-suite-predictive-walkthrough/simulation-running.png
[img-simulation-warning]: media/iot-suite-predictive-walkthrough/simulation-warning.png

[lnk-powerbi]: https://www.github.com/Microsoft/PowerBI-visuals
[lnk_machine_learning]: https://azure.microsoft.com/services/machine-learning/
[lnk-remote-monitoring]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-cortana-analytics]: http://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Template-3
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md

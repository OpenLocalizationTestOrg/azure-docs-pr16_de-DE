<properties
 pageTitle="Vorhersagen Wartung vorkonfiguriert Lösung | Microsoft Azure"
 description="Eine Beschreibung der Vorhersage Wartung Azure IoT vorkonfiguriert Lösung."
 services=""
 suite="iot-suite"
 documentationCenter=""
 authors="stevehob"
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

# <a name="predictive-maintenance-preconfigured-solution-overview"></a>Vorhersagen Wartung vorkonfiguriert Lösung (Übersicht)

Die Lösung *Vorhersage Wartung* vorkonfiguriert ist eine der [vorkonfigurierten Lösungen] [ lnk_preconfigured_solutions] als Teil des [Microsoft Azure IoT Suite]freigegeben[lnk_iot_suite]. Diese Lösung integriert das Gerät in Echtzeit werden Websitesammlung mit einem Vorhersage Modell erstellt [Azure maschinellen Learning][lnk_machine_learning].


Mit Azure IoT Suite können Unternehmen schnell und einfach Herstellen einer Verbindung mit Ressourcen überwachen und Analysieren von Daten in Echtzeit. Die Lösung Vorhersage Wartung vorkonfiguriert verwendet die Daten und Rich-Dashboards und Visualisierungen Unternehmen mit neuen Intelligence bereitstellen, die Effizienz und Umsatz Streams verbessern können.

## <a name="the-scenario"></a>Das Szenario

Fabrikam ist eine regionale Fluggesellschaft, die auf hervorragende Customer Experience Mitbewerber Preisen konzentriert. Eine Flight Delays verursacht Wartungsprobleme und Flugzeug-Engine Wartung besonders anspruchsvoll ist. Engine Fehler Flug muss unter allen Umständen vermieden werden, damit Fabrikam seine Suchmaschinen regelmäßig untersucht und geplante Wartungsprogramm befolgt. Jedoch tragen nicht Flugzeugmotoren immer die gleiche. Einige unnötige Wartung erfolgt auf Module. Wichtiger ist, treten Probleme, die ein Flugzeug gemahlen können, bis Wartung durchgeführt wird. Dadurch wird teure Verzögerungen, insbesondere dann, wenn ein Flugzeug an einer Position befindet, in dem die richtigen Technikern oder freie Teile nicht verfügbar sind.

Die Module der Fabrikams Flugzeug werden bei bestimmt auch bei instrumentiert die Engine Bedingungen Flug zu überwachen. Azure IoT Suite Formular Fabrikam mit der Flug gesammelten Sensordaten werden sammeln. Nachdem der Betrieb-Engine sammeln Jahre und Fehlerdaten haben Fabrikams Daten Wissenschaftler eine Möglichkeit, die verbleibende hilfreiche Nutzungsdauer (RUL) von einem Flugzeug-Engine Vorhersagen erstellt. Was sie gekennzeichnet haben, wird eine Beziehung zwischen der Daten aus vier der Engine Sensoren mit der Engine Verschleiß, die auf Fehler bei der tatsächlichen leads. Fabrikam fort, um Sicherheit zu gewährleisten regelmäßige Kontrolle ausführen, können sie jetzt die Modelle zum Berechnen der RUL für jede-Engine aus, nach der Flug Module jeder Flight mithilfe der werden entnommen verwenden. Jetzt können Sie Fabrikam Vorhersagen zukünftigen interessante Fehler und Plan für die Wartung und Reparieren im voraus.

> [AZURE.NOTE] Das Modell Lösung verwendet die ist-Engine Verschleiß Daten.

Durch den Punkt Vorhersage von Wartung erforderlich ist, kann Fabrikam seine Vorgänge aus, um die Verwaltungskosten optimieren. Wartung Koordinatoren arbeiten mit Planer Planung Wartung mit ein Flugzeug an einer bestimmten Position beenden und Sicherstellung genügend Zeit für die Flugzeug außer Betrieb sein, ohne dass Terminplan Unterbrechung ausgerichtet. Fabrikam kann Technikern entsprechend planen; Sicherstellung Flugzeug werden ohne Wartezeit effizient muss. Inventory Steuerelement Projektmanager erhalten Wartungspläne, damit sie ihre Bestellung durch optimieren und Webparts Inventory Ersatzteile können. All dies ermöglicht Fabrikam Flugzeug Grund Zeit minimieren und Geschäftsjahre Kosten senken und um die Sicherheit von Personen und Besatzung.

Zu verstehen, wie [Azure IoT Suite] [ lnk_iot_suite] bietet Kunden Funktionen müssen Ergebnisse erzielen mit Vorhersage Wartung, informieren Sie sich über diese [Infographic][lnk_infographic].

## <a name="how-the-predictive-maintenance-solution-is-built"></a>Erstellungsweise die Lösung Vorhersage Wartung

Die Lösung nutzt ein vorhandenes Azure maschinellen Learning-Modell als Vorlage, um diese Funktionen, die vom Gerät werden über IoT Suite Services erfassten anzeigen zur Verfügung. Microsoft hat eine [Regressionsanalyse Modell] erstellt[ lnk_regression_model] der ein Flugzeug-Engine und die vollständige Vorlage, Daten veröffentlicht<sup>\[1\]</sup>, und schrittweise Anleitung zum Verwenden des Modells.

Die Azure IoT Vorhersage Wartung vorkonfiguriert Lösung verwendet Regressionsmodell anhand dieser Vorlage erstellt. Es wird in Ihrem Abonnement Azure bereitgestellt und durch eine automatisch generierte API verfügbar gemacht werden. Die Lösung umfasst eine Teilmenge der Daten testen, 4 (von 100 total) darstellt TTS und die 4 (von 21 total) Sensor Datenstreams die ein richtiges Ergebnis aus dem Modell ausgebildeten zur Verfügung zu stellen.

*\[1\] Saxena a und K. Goebel (2008). "Turbofan-Engine Verschlechterung Simulation Datenmenge", NASA Ames Prognostics Datenrepository (http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/), NASA Ames Research Center, Moffett Feld, CA*

## <a name="next-steps"></a>Nächste Schritte

Informationen darüber, wie Azure IoT Vorhersage Wartung Szenarien ermöglicht Informationen hierzu [erfassen Wert aus dem Internet der Dinge][lnk_capture_value].

Nehmen Sie eine [Exemplarische Vorgehensweise] [ lnk-predictive-walkthrough] der Vorhersage Wartung vorkonfiguriert Lösung.

[lnk-predictive-walkthrough]: iot-suite-predictive-walkthrough.md
[lnk_preconfigured_solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk_iot_suite]: iot-suite-overview.md
[lnk_machine_learning]: https://azure.microsoft.com/services/machine-learning/
[lnk_infographic]: https://www.microsoft.com/server-cloud/predictivemaintenance/Index.html
[lnk_regression_model]: http://gallery.cortanaanalytics.com/Collection/Predictive-Maintenance-Template-3
[lnk_capture_value]: http://download.microsoft.com/download/0/7/D/07D394CE-185D-4B96-AC3C-9B61179F7080/Capture_value_from_the_Internet%20of%20Things_with_Predictive_Maintenance.PDF

Sie können auch einige andere Features und Funktionen von den IoT Suite vorkonfiguriert Lösungen vertraut machen:

- [Häufig gestellte Fragen zu IoT Suite][lnk-faq]
- [IoT Sicherheit von Grund auf][lnk-security-groundup]

[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md

<properties
    pageTitle="Automatische Skalierung und App-Service-Umgebung | Microsoft Azure"
    description="Automatische Skalierung und App-Service-Umgebung"
    services="app-service"
    documentationCenter=""
    authors="btardif"
    manager="wpickett"
    editor=""
/>

<tags
    ms.service="app-service"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="byvinyal"
/>

# <a name="autoscaling-and-app-service-environment"></a>Automatische Skalierung und App-Service-Umgebung

Azure App Service-Umgebungen unterstützen die *Automatische Skalierung*. Sie können automatisch skalieren einzelne Worker Pools basierend auf Kennzahlen oder planen.

![Automatisch skalieren Optionen für einen Pool Arbeitskollegen.][intro]

Automatische Skalierung optimiert die Nutzung der Ressource, indem automatisch vergrößern und verkleinern eine App-Service-Umgebung zum Anpassen des Budgets und oder Profil zu laden.

## <a name="configure-worker-pool-autoscale"></a>Konfigurieren von Worker Ressourcenpool automatisch skalieren

Sie können die Funktionalität automatisch skalieren auf der Registerkarte **Einstellungen** der Worker Ressourcenpool zugreifen.

![Registerkarte "Einstellungen" im Ressourcenpool Arbeitskollegen.][settings-scale]

Hier ist die Benutzeroberfläche sollten ziemlich vertraut, da er mit der gleichen, Sie sehen, wenn Sie eine App Serviceplan skalieren. 

![Manuelle skalierungseinstellungen.][scale-manual]

Sie können auch ein Profil automatisch Skalieren konfigurieren.

![Automatisch skalieren-Einstellungen.][scale-profile]

Automatisch skalieren-Profile sind nützlich, Grenzwerte für Ihren Maßstab festzulegen. Auf diese Weise können Sie eine konsistente Leistung erzielen Sie durch Festlegen eines Untergrenze Maßstab-Wert (1) und eine vorhersehbar ausgeben Linienende durch Festlegen einer Obergrenze (2) haben.

![Skalierungseinstellungen im Profil.][scale-profile2]

Nachdem Sie ein Profil definiert haben, können Sie automatisch Skalieren von Regeln zum nach oben oder unten die Anzahl der Instanzen im Pool Arbeitskollegen innerhalb der Grenzen definiert werden, indem Sie das Profil skalieren hinzufügen. Automatisch skalieren Regeln basieren auf Kennzahlen.

![Maßstab Regel.][scale-rule]

 Alle Worker Ressourcenpool oder Front-End-Kennzahlen können verwendet werden, automatisch skalieren Regeln definiert. Diese Metrik sind die gleichen Metrik, die können Sie in den Ressourcen Blade Diagrammen überwachen oder Festlegen von Benachrichtigungen für.

## <a name="autoscale-example"></a>Beispiel automatisch skalieren

Automatisch Skalieren einer App-Service-Umgebung kann am besten durch ein Szenario durchgehen dargestellt werden.

Dieser Artikel erläutert die alle erforderlichen Aspekte beim Einrichten von automatisch skalieren. Im Artikel führt Sie durch die Aktivitäten, die kommen, wenn automatische Skalierung App-Service-Umgebungen berücksichtigt werden, die in der App-Service-Umgebung gehostet werden.

### <a name="scenario-introduction"></a>Szenario Einführung

Frank ist ein Systemadministrator für ein Unternehmen, die einen Teil der Auslastung, die er verwaltet zu einer App-Service-Umgebung migriert wurde.

Die App-Service-Umgebung wird wie folgt mit dem Maßstab manuelle konfiguriert:

* **Vorder-enden:** 3
* **Worker Pool 1**: 10
* **Worker Pool 2**: 5
* **Worker Pool 3**: 5

Worker Pool 1 wird für Produktionsarbeitslasten, verwendet, während Worker Pool 2 und Worker Pool 3 für Qualität (f & a) und Entwicklung Auslastung verwendet werden.

Die App Dienstpläne für f & a und Entwickler sind für die manuelle Skalierung konfiguriert. Die Herstellung App-Serviceplan wird auf automatisch skalieren festgelegt, Variationen laden und den Datenverkehr behandelt.

Frank ist mit der Anwendung sehr vertraut. Er weiß, dass die Höchstwert Stunden für Laden zwischen 9:00 Uhr und 6:00 Uhr sind, da es sich um eine Linie-of-Business (LOB) Anwendung, die Mitarbeiter verwenden handelt, während Sie sich im Büro befinden. Anschließend legt Verwendung ab, wenn Benutzer für den Tag fertig sind. Außerhalb der Höchstwert Stunden besteht weiterhin einige laden, da Benutzer die app Remotezugriff können mit ihren mobilen Geräten oder privaten PCs. Die Herstellung App-Serviceplan ist zu skalieren basierend auf CPU-Auslastung mit den folgenden Regeln bereits konfiguriert:

![Bestimmte Einstellungen für LOB-app.][asp-scale]

|   **Automatisch skalieren Profil – Arbeitstagen – App-Serviceplan**     |   **Automatisch skalieren Profil – Wochenenden – App-Serviceplan**     |
|   ----------------------------------------------------    |   ----------------------------------------------------    |
|   **Name:** Weekday-Profil                               |   **Name:** Wochenende Profil                               |
|   **Durch Skalieren:** Zeitplan und Leistung von Regeln            |   **Durch Skalieren:** Zeitplan und Leistung von Regeln            |
|   **Profil:** Wochentage                                   |   **Profil:** Wochenende                                    |
|   **Typ:** Serie                                    |   **Typ:** Serie                                    |
|   **Zielbereich:** 5 bis 20 Instanzen                     |   **Zielbereich:** 3 bis 10 Instanzen                     |
|   **Tage:** Montag, Dienstag, Mittwoch, Donnerstag, Freitag  |   **Tage:** Samstag, Sonntag                              |
|   **Startzeit:** 9:00 Uhr                                 |   **Startzeit:** 9:00 Uhr                                 |
|   **Zeitzone:** UTC-08                                   |   **Zeitzone:** UTC-08                                   |
|                                                           |                                                           |
|   **Automatisch skalieren Regel (Skalierung nach oben)**                           |   **Automatisch skalieren Regel (Skalierung nach oben)**                           |
|   **Ressource:** Herstellung (App-Service-Umgebung)      |   **Ressource:** Herstellung (App-Service-Umgebung)      |
|   **Metrisch:** CPU%                                       |   **Metrisch:** CPU%                                       |
|   **Vorgang:** Mehr als 60 %                         |   **Vorgang:** Größer als 80 %                         |
|   **Dauer:** 5 Minuten                                 |   **Dauer:** 10 Minuten                                |
|   **Aggregation Anzeigedauer:** Mittelwert                           |   **Aggregation Anzeigedauer:** Mittelwert                           |
|   **Aktion:** Erhöhen der Anzahl von 2                         |   **Aktion:** Erhöhen der Anzahl von 1                         |
|   **Aussagekräftige nach unten (Minuten):** 15                             |   **Aussagekräftige nach unten (Minuten):** 20                             |
|                                                           |                                                           |
  	|   **Automatisch skalieren Regel (Maßstab abwärts)**                     |   **Automatisch skalieren Regel (Maßstab abwärts)**                         |
|   **Ressource:** Herstellung (App-Service-Umgebung)      |   **Ressource:** Herstellung (App-Service-Umgebung)      |
|   **Metrisch:** CPU%                                       |   **Metrisch:** CPU%                                       |
|   **Vorgang:** Weniger als 30 %                            |   **Vorgang:** Weniger als 20 %                            |
|   **Dauer:** 10 Minuten                                |   **Dauer:** 15 Minuten                                |
|   **Aggregation Anzeigedauer:** Mittelwert                           |   **Aggregation Anzeigedauer:** Mittelwert                           |
|   **Aktion:** Verringern der Anzahl von 1                         |   **Aktion:** Verringern der Anzahl von 1                         |
|   **Aussagekräftige nach unten (Minuten):** 20                             |   **Aussagekräftige nach unten (Minuten):** 10                             |

### <a name="app-service-plan-inflation-rate"></a>App-Dienst Plan Inflationsrate

App-Dienst Pläne, die so konfiguriert sind, automatisch skalieren tun Sie dies mit maximal pro Stunde. Diese Rate kann basierend auf den Werten der Regel automatisch skalieren bereitgestellten berechnet werden.

Grundlegendes zu und Berechnung des *App-Dienst Plan Inflationsrate* ist wichtig für App-Service-Umgebung, die automatisch skalieren Maßstab Änderungen an einem Ressourcenpool Worker nicht erfolgt sind.

Die App-Dienst Plan Inflationsrate wird wie folgt berechnet:

![App-Dienst Plan Inflationsrate Berechnung.][ASP-Inflation]

Anhand der automatisch skalieren – skalieren Regel für das Profil Weekday der Herstellung Serviceplan App:

![App-Dienst Plan Inflationsrate für Wochentage basierend auf automatisch skalieren – skalieren Regel.][Equation1]

Bei der automatisch skalieren – skalieren Regel für das Profil Wochenende der Herstellung App-Serviceplan, würde die Formel zu beheben:

![App-Dienst Plan Inflationsrate für Wochenenden basierend auf automatisch skalieren – skalieren Regel.][Equation2]

Dieser Wert kann auch für Maßstab-Down-Vorgänge berechnet werden.

Anhand der automatisch skalieren – Maßstab unten Regel für das Profil Weekday der Herstellung App-Serviceplan würde dies wie folgt aussehen:

![App-Dienst Plan Inflationsrate für Wochentage basierend auf automatisch skalieren – Maßstab unten Regel.][Equation3]

Bei der automatisch skalieren – Maßstab unten Regel für das Profil Wochenende der Herstellung App-Serviceplan, würde die Formel zu beheben:  

![App-Dienst Plan Inflationsrate für Wochenenden basierend auf automatisch skalieren – Maßstab unten Regel.][Equation4]

Die Herstellung App-Serviceplan kann eine maximale Rate von acht Instanzen pro Stunde in der Woche und vier Instanzen/Stunde während der Wochenenden zunehmen. Sie können Instanzen von vier Instanzen pro Stunde in der Woche mit maximal und sechs Instanzen/Stunde während der Wochenenden lassen.

Wenn mehrere Dienstpläne für die App in einem Ressourcenpool Worker gehostet werden, müssen Sie die *gesamte Inflationsrate* als Summe der die Inflationsrate für die App Dienstpläne zu berechnen, die gerade in diesem Pool Worker hosten.

![Total Inflationsrate Berechnung für mehrere App-Service-Pläne, die in einem Ressourcenpool Worker gehostet wird.][ASP-Total-Inflation]

### <a name="use-the-app-service-plan-inflation-rate-to-define-worker-pool-autoscale-rules"></a>Verwenden Sie die App-Dienst Plan Inflationsrate zum Definieren von Arbeitskollegen Ressourcenpool automatisch skalieren Regeln

Worker Pool, hosten App-Dienst Pläne, die so konfiguriert sind, automatisch skalieren ein Puffer Kapazität zugewiesen werden müssen. Der Puffer ermöglicht für die Vorgänge automatisch skalieren vergrößern und Verkleinern der App-Serviceplan, je nach Bedarf. Der minimale Puffer wäre die berechnete Summe App Dienst planen Inflationsrate.

Da Vorgänge in der App-Dienst Umgebung Maßstab gelten einige Zeit dauern, sollte geändert weiteren Demand Änderungen berücksichtigt werden, die ausgeführt werden kann, während einer Skalierung ausgeführt wird. Um diese Wartezeit zu unterstützen, empfehlen wir, die berechnete Summe App Dienst planen Inflationsrate als die minimale Anzahl der Instanzen, die hinzugefügt werden für jeden Vorgang automatisch skalieren verwenden.

Mithilfe dieser Informationen kann Frank die folgenden automatisch skalieren Profil und Regeln definieren:

![Automatisch skalieren Profil Regeln beispielsweise LOB.][Worker-Pool-Scale]

|   **Automatisch skalieren Profil – Arbeitstagen**                        |   **Automatisch skalieren Profil – Wochenenden**                |
|   ----------------------------------------------------    |   --------------------------------------------    |
|   **Name:** Weekday-Profil                               |   **Name:** Wochenende Profil                       |
|   **Durch Skalieren:** Zeitplan und Leistung von Regeln            |   **Durch Skalieren:** Zeitplan und Leistung von Regeln    |
|   **Profil:** Wochentage                                   |   **Profil:** Wochenende                            |
|   **Typ:** Serie                                    |   **Typ:** Serie                            |
|   **Zielbereich:** 13 zu 25 Instanzen                    |   **Zielbereich:** 6 bis 15 Instanzen             |
|   **Tage:** Montag, Dienstag, Mittwoch, Donnerstag, Freitag  |   **Tage:** Samstag, Sonntag                      |
|   **Startzeit:** 7:00 Uhr                                 |   **Startzeit:** 9:00 Uhr                         |
|   **Zeitzone:** UTC-08                                   |   **Zeitzone:** UTC-08                           |
|                                                           |                                                   |
|   **Automatisch skalieren Regel (Skalierung nach oben)**                           |   **Automatisch skalieren Regel (Skalierung nach oben)**                   |
|   **Ressource:** Worker Pool 1                             |   **Ressource:** Worker Pool 1                     |
|   **Metrisch:** WorkersAvailable                            |   **Metrisch:** WorkersAvailable                    |
|   **Vorgang:** Kleiner als 8                              |   **Vorgang:** Weniger als 3                      |
|   **Dauer:** 20 Minuten                                |   **Dauer:** 30 Minuten                        |
|   **Aggregation Anzeigedauer:** Mittelwert                           |   **Aggregation Anzeigedauer:** Mittelwert                   |
|   **Aktion:** Erhöhen der Anzahl von 8                         |   **Aktion:** Erhöhen der Anzahl von 3                 |
|   **Aussagekräftige nach unten (Minuten):** 180                            |   **Aussagekräftige nach unten (Minuten):** 180                    |
|                                                           |                                                   |
|   **Automatisch skalieren Regel (Maßstab abwärts)**                         |   **Automatisch skalieren Regel (Maßstab abwärts)**                 |
|   **Ressource:** Worker Pool 1                             |   **Ressource:** Worker Pool 1                     |
|   **Metrisch:** WorkersAvailable                            |   **Metrisch:** WorkersAvailable                    |
|   **Vorgang:** Größer als 8                           |   **Vorgang:** Größer als 3                   |
|   **Dauer:** 20 Minuten                                |   **Dauer:** 15 Minuten                        |
|   **Aggregation Anzeigedauer:** Mittelwert                           |   **Aggregation Anzeigedauer:** Mittelwert                   |
|   **Aktion:** Verringern der Anzahl von 2                         |   **Aktion:** Verringern der Anzahl von 3                 |
|   **Aussagekräftige nach unten (Minuten):** 120                            |   **Aussagekräftige nach unten (Minuten):** 120                    |

Zielbereich im Profil definierte wird von den minimalen Instanzen definiert das Profil für die App-Serviceplan + Puffer berechnet.

Der Bereich Maximum wäre die Summe der maximalen Bereiche für alle App-Service-Pläne, die in dem Pool Worker gehostet wird.

Die Anzahl der erhöhen für die Skalierung der Regeln sollte mindestens 1 X die Inflationsrate App Service-Plan für die Skalierung eingerichtet werden.

Verkleinern zählen kann angepasst werden, auf einen Wert zwischen 1/2 X oder 1 X die Inflationsrate App Service-Plan für die Skalierung nach unten.

### <a name="autoscale-for-front-end-pool"></a>Automatisch skalieren für Front-End-Ressourcenpool

Regeln für die Front-End-automatisch skalieren sind einfacher als für Worker Pools. Hauptsächlich, sollten Sie  
Stellen Sie sicher, dass die Dauer der Maßeinheit und die Cooldown Zeitgeber berücksichtigen, dass nicht skalieren Vorgänge für eine App Serviceplan erfolgt sind.

In diesem Szenario weiß Frank an, dass die Effektivverzinsung zurück erhöht nach front-End CPU-Auslastung 80 % erreicht haben, und die Regel automatisch skalieren Instanzen erhöhen, wie folgt:

![Automatisch skalieren Einstellungen für Front-End-Pool.][Front-End-Scale]

|   **Automatisch skalieren Profil – endet den Vordergrund**              |
|   --------------------------------------------    |
|   **Name:** Automatisch skalieren – endet den Vordergrund                |
|   **Durch Skalieren:** Zeitplan und Leistung von Regeln    |
|   **Profil:** Jeden Tag                           |
|   **Typ:** Serie                            |
|   **Zielbereich:** 3 bis 10 Instanzen             |
|   **Tage:** Jeden Tag                              |
|   **Startzeit:** 9:00 Uhr                         |
|   **Zeitzone:** UTC-08                           |
|                                                   |
|   **Automatisch skalieren Regel (Skalierung nach oben)**                   |
|   **Ressource:** Front-End-Ressourcenpool                    |
|   **Metrisch:** CPU%                               |
|   **Vorgang:** Mehr als 60 %                 |
|   **Dauer:** 20 Minuten                        |
|   **Aggregation Anzeigedauer:** Mittelwert                   |
|   **Aktion:** Erhöhen der Anzahl von 3                 |
|   **Aussagekräftige nach unten (Minuten):** 120                    |
|                                                   |
|   **Automatisch skalieren Regel (Maßstab abwärts)**                 |
|   **Ressource:** Worker Pool 1                     |
|   **Metrisch:** CPU%                               |
|   **Vorgang:** Weniger als 30 %                    |
|   **Dauer:** 20 Minuten                        |
|   **Aggregation Anzeigedauer:** Mittelwert                   |
|   **Aktion:** Verringern der Anzahl von 3                 |
|   **Aussagekräftige nach unten (Minuten):** 120                    |

<!-- IMAGES -->
[intro]: ./media/app-service-environment-auto-scale/introduction.png
[settings-scale]: ./media/app-service-environment-auto-scale/settings-scale.png
[scale-manual]: ./media/app-service-environment-auto-scale/scale-manual.png
[scale-profile]: ./media/app-service-environment-auto-scale/scale-profile.png
[scale-profile2]: ./media/app-service-environment-auto-scale/scale-profile-2.png
[scale-rule]: ./media/app-service-environment-auto-scale/scale-rule.png
[asp-scale]: ./media/app-service-environment-auto-scale/asp-scale.png
[ASP-Inflation]: ./media/app-service-environment-auto-scale/asp-inflation-rate.png
[Equation1]: ./media/app-service-environment-auto-scale/equation1.png
[Equation2]: ./media/app-service-environment-auto-scale/equation2.png
[Equation3]: ./media/app-service-environment-auto-scale/equation3.png
[Equation4]: ./media/app-service-environment-auto-scale/equation4.png
[ASP-Total-Inflation]: ./media/app-service-environment-auto-scale/asp-total-inflation-rate.png
[Worker-Pool-Scale]: ./media/app-service-environment-auto-scale/wp-scale.png
[Front-End-Scale]: ./media/app-service-environment-auto-scale/fe-scale.png

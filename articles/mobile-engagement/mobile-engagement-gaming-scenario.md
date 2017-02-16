<properties 
    pageTitle="Azure Mobile Engagement Implementierung für Spiele-App"
    description="Spiele app Szenario Azure Mobile Engagement implementiert wird." 
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-engagement"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="mobile-multiple"
    ms.workload="mobile" 
    ms.date="08/19/2016"
    ms.author="piyushjo"/>

#<a name="implement-mobile-engagement-with-gaming-app"></a>Implementieren der mobilen Engagement mit Spiele-App

## <a name="overview"></a>(Übersicht)

Eine Spiele Start hat eine neue Grundlage ausgesprochen role-play Strategie Spiel app gestartet. Das Spiel seit 6 Monaten einsatzbereit. Dieses Spiel riesigen erfolgreich ist und sie hat Millionen von Downloads und die Aufbewahrung ist im Vergleich zu anderen Spiel Start-apps sehr hoch. Stimmen bei der Besprechung vierteljährlichen überprüfen Projektbeteiligte benötigten Mittelwert Umsatz pro Benutzer (ARPU) zu erhöhen. Premium im Spiel Pakete sind als spezielle Angebote verfügbar. Diese Spiele Packs können Benutzer die Darstellung und Leistung von deren ausgesprochen Linien und Köder oder Tackles im Spiel aktualisieren. Es gibt jedoch Paket Sales sehr niedrig. Daher müssen sie entscheiden, ob zuerst die Kunden Erfahrung mit einem Analytics-Tool analysieren, und klicken Sie dann Erweiterte Programm sales mit vergrößern zum Entwickeln einer Engagement Segmentierung.

Basierend auf den [Azure Mobile Engagement - Leitfaden für erste Schritte mit Best Practices](mobile-engagement-getting-started-best-practices.md) erstellen sie eine Strategie Engagement.

##<a name="objectives-and-kpis"></a>Ziele und KPIs

Hauptverantwortliche für das Spiel entsprechen. Alle stimmen auf ein Hauptfenster Ziel - zur Premium Paket mit 15 % Steigerung. Wie sie erstellen dieses Ziel Business Key Performance Indicators (KPIs) Measure und Laufwerk

* Klicken Sie auf der Ebene der Spiel werden diese Pakete erworben?
* Was ist der Umsatz pro Benutzer pro Sitzung, pro Woche und pro Monat?
* Was sind die Typen bevorzugten erwerben?

Teil 1 des [Leitfaden für erste Schritte](mobile-engagement-getting-started-best-practices.md) wird erläutert, wie die Ziele und KPIs definieren. 

Mit der Business KPIs jetzt definiert ist wird der Produktmanager Mobile Engagement KPIs Ermittlung neuer Benutzertrends und Beibehaltung erstellt.

* Überwachen der Aufbewahrungsrichtlinien und verwenden Sie für die folgenden Intervalle: täglich, alle 2 Tage, wöchentlich, monatlich und alle 3 Monate
* Anzahl der aktiven Benutzer
* Die app-Bewertung im Speicher

Ausgehend vom IT-Team Empfehlungen, wurden die folgenden technischen KPIs hinzugefügt, die folgenden Fragen beantworten:

* Was Meine Benutzerpfad ist (der Seite besucht wird, wie viel Zeit Benutzer investieren daran)
* Anzahl der Absturz oder Fehler aufgetreten pro Sitzung
* Was OS Versionen meiner Benutzer ausgeführt werden?
* Was ist die durchschnittliche Größe des Bildschirms für meine Benutzer?
* Welche Arten von Internet Connectivity habe meine Benutzer?

Der Produktmanager Mobile für jeden KPI gibt an, dass die Daten Anna benötigt und wo befindet sich in dem Playbook.

## <a name="engagement-program-and-integration"></a>Programm Engagement und integration

Vor dem Erstellen einer erweiterten Engagement Programm, sollte der Leiter Mobile Projekt für das Projekt zuständige Kennenlernen wie und wenn Produkte von den Benutzern genutzt werden müssen.

Nach 3 Monate hat der Leiter Mobile Projekt genügend Daten, um seine innerhalb der app Pushbenachrichtigungen Benachrichtigung Verkäufe optimieren zusammengestellt. Er lernfähig, die:

* Der erste Kauf geschieht in der Regel auf der Ebene 14. Für 90 % aller Fälle ist der Kauf neue legendären Waffen für $3 ein.
* 80 % aller Fälle welche Benutzer gekauft haben, fahren Sie mit dem Produkt, und nehmen Sie weitere Einkäufe.
* Benutzer, die die Ebene 20; vergangenen beginnen Ausgeben von mehr als $10/Woche.
* Benutzer in der Regel Premium-Paketen Ebene 16, 24 und 32 kaufen.

Dank dieser Analyse beschließt, sondern die Mobile Projekt Leiter bestimmte Pushbenachrichtigungen folgen der Benachrichtigung zum Erhöhen app "Sales" erstellen. Er erstellt drei Pushbenachrichtigungen folgen die er aufgerufen: Willkommen Programm, Sales Programm- und inaktive Programm. Weitere Informationen finden Sie in der [Playbooks](https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks)
    ![][1]

<!--Image references-->

[1]: ./media/mobile-engagement-game-scenario/notification-scenario.png

<!--Link references-->

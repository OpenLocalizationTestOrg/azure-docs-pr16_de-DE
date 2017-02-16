<properties 
    pageTitle="Azure Mobile Engagement Implementierung für Medien-App"
    description="Medien app Szenario Azure Mobile Engagement implementiert wird." 
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

#<a name="implement-mobile-engagement-with-media-app"></a>Implementieren der mobilen Engagement mit Medienapp

## <a name="overview"></a>(Übersicht)

Johann ist mobile Projektmanager für eine große Medienunternehmen. Er gestartet zuletzt eine neue app, die Anzahl sehr hoch Download enthält. Hat er seine Ziele zum Download Treffer aber weiterhin seinem zurückgeben auf Investment(ROI) pro Benutzer nicht seinen Bedarf an entsprechen. 

Johann hat bereits identifiziert, warum seine Rendite zu niedrig ist. Benutzer häufig beenden mit seinem app nach nur 2 Wochen und die meisten von ihnen stammen nie wieder. Er möchte die Speicherung Ihrer App zu vergrößern.

Nachdem einige Anfangsbuchstaben testen, er Kenntnisse hat, wenn er seine Benutzer mit Pushbenachrichtigungen, aktiviert wird, sind Sie oft weiterhin seinem app verwenden zu können. Benutzer inaktive wurden werden auch häufig bei der app je nach Benachrichtigungen zurückgeben, die er sendet. Johann beschließt, die in einer bestimmten Engagement-Programm für seine app investieren, die mit Pushbenachrichtigungen verwendet erweiterte verwendet.

Johann [Azure Mobile Engagement - Leitfaden für erste Schritte mit Best Practices](mobile-engagement-getting-started-best-practices.md) wurde zuletzt gelesen und entschieden hat, die Empfehlungen zwischen der Führungslinie implementiert wird.

##<a name="objectives-and-kpis"></a>Ziele und KPIs

Hauptverantwortliche für Johanns app entsprechen. Business wird von Werbung generiert, während der Benutzer seinen Medien nutzen. Durch erhöhen verbraucht pro Benutzer Inhalt, tragen Johann seine Umsätze. Alle stimmen auf ein Hauptfenster Ziel: zur Steigerung von Werbung um 25 %. Wie sie erstellen dieses Ziel Business Key Performance Indicators (KPIs) Measure und Laufwerk

* Anzahl der pro Benutzer geklickt anzeigen
* Wie viele Artikelseiten besuchten (pro Benutzer / pro Sitzung / pro Woche / pro Monat...)
* Was sind die bevorzugte Kategorien

Basierend auf Johanns Besprechung mit den wichtigsten Projektbeteiligten hat er seine Business KPIs definiert. Er folgt Teil 1 [Azure Mobile Engagement - Leitfaden für erste Schritte mit Best Practices](mobile-engagement-getting-started-best-practices.md). 

Als Nächstes erstellt er die folgenden Engagement KPIs, um sicherzustellen, dass die Ziele erreicht werden:

* Überwachen von Aufbewahrungsrichtlinien für die folgenden Intervalle: täglich, wöchentlich, Bi-wöchentlich oder monatlich.
* Aktive Benutzer zählt
* Speichert die app-Bewertung in der app

Ausgehend vom IT-Team Empfehlungen, wurden die folgenden technischen KPIs hinzugefügt, die folgenden Fragen beantworten:

* Was Meine Benutzerpfad ist (der Seite besucht wird, wie viele Mal, wenn Benutzer daran aufwenden)
* Anzahl der Absturz oder Fehler aufgetreten pro Sitzung?
* Was OS Versionen meiner Benutzer ausgeführt werden?
* Was ist die durchschnittliche Größe des Bildschirms für meine Benutzer?
* Welche Arten von internetverbindungen habe meine Benutzer?

Für jeden KPI denen die benötigten Daten klassifiziert und er in der richtigen Speicherort der seine Playbook Einträge.

## <a name="engagement-program-and-integration"></a>Programm Engagement und integration

Jetzt die Johann definieren seiner KPIs abgeschlossen ist, beginnt er seine Engagement Strategiephase 4 Engagement Programme und ihre Ziele definiert:    ![][1]

Klicken Sie dann voranschreitet Johann tieferen Pushbenachrichtigungen für jedes Programm aufgeführt. Pushbenachrichtigung werden von fünf Elementen definiert:

1. Ziel: Was ist das Ziel der Benachrichtigung
2. Wie das Ziel erreicht werden
3. Ziel: Wer soll die Benachrichtigung erhalten?
4. Content: Was ist die Wortwahl und das Format der Benachrichtigung (In App/Out der App)
5. Wann: Was beste Zeitpunkt zum Senden der Pushbenachrichtigung von diesem ist

    ![][2]

Weitere Informationen finden Sie in der [Playbooks](https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks).

Gemäß dem Teil 2 Whitepaper Johann Abschnitt Target verwendet, welche Daten zu definieren, er sammeln muss, und schreibt seine Kategorie gemeinsam mit IT-Team die Lösung implementieren möchten. Nach 1 Woche Implementierung und Testen der Benutzerakzeptanz kann Johann schließlich seine Programme starten.

##<a name="program-results"></a>Programmergebnisse

4 Monate später prüft Johann aufführungen Programme. Willkommen-Programme und wöchentliche sind seine Ziele Besprechung. Verringert die Anzahl der Benutzer nur eine Sitzung, weitere Features der app verwendet werden, und die Anzahl der Verbindungen pro Woche hat verdoppelt.

**Inaktive Programm** trägt dazu Johann Benutzer Tendenzen zu verstehen. Er angezeigt wird, dass 15 % der inaktive Benutzer wieder zu der app gelangen. Jedoch nicht die meisten der aktiven mehr als 1 Monat aussetzen. Johann vorgesehen ist eine mögliche Optimierung dieser Sequenz mit zusätzlichen Benachrichtigungen und seinen Inhalten Auswahlmöglichkeiten zu erweitern.

Das **Programm entdecken Sie** funktioniert gut nicht. Es erhöht die cross verkaufen, aber nicht genügend um seine Ziele erreicht haben. Johann kennzeichnet, dass er genügend stellen relevante Daten nicht verwendet, und die entsprechenden Inhalt vorschlagen. Er reagiert dieses Programm und liegt der Schwerpunkt auf "editorial Pushbenachrichtigungen" mit Azure Mobile Engagement senden. Seine Journalisten bereits eine Lösung CMS, Pushbenachrichtigungen zu senden und nicht geändert werden soll.

Johann beschließt, die API erreicht haben verwenden, das eine HTTP-REST-API verwalten Reichweite massensendungen zu ermitteln ermöglicht, ohne AZME Web-Oberfläche verwenden. Bei diesem Ansatz kann Johann Datensammlung er muss und seine Autoren die Lösung CMS weiterhin mit zulassen.

Um sicherzustellen, dass die Funktion ordnungsgemäß funktioniert, bittet Michael IT-Team auf die folgenden Punkte wachsam sein:

1. **Betriebssysteme** : alle eigene Regeln zum Pushbenachrichtigungen, verwalten, damit Johann beschließt, die allen Fällen Liste und überprüft, ob die APIs es verarbeitet haben.
Z. B.: Android Pushbenachrichtigungen System kann große Bild, nicht der Fall bei iOS ist.

2. **Zeitrahmen**: Johann möchte, dass eine API, die den Zeitrahmen, und legen Sie sicherstellen, dass ein Ende. Er möchte, dass Benutzer eine Benachrichtigung Unterbrechung bombing beibehalten.

3. **Kategorien**: Marketingteam vorbereitet Vorlage für jeden Typ der Warnung. Johann fragt IT-Team Kategorien innerhalb der API festlegen.

Nach einigen Tests ist Johann zufrieden. Dank dieses API können Journalisten immer noch senden Pushbenachrichtigungen mit ihren CMS und Azure Mobile Engagement sammelt alle Verhalten Daten für diese

Nach dem folgenden 4 zuerst Monaten und Ergebnisse geben eine gute allgemeine Leistung und bietet KONFIDENZ für Johann und seine Brett, Rendite pro Benutzer erhöht pro 15 % und mobile Sales darstellen 17,5 % des Gesamtumsatzes, eine Steigerung von 7.5 % in nur vier Monaten.

<!--Image references-->
[1]: ./media/mobile-engagement-media-scenario/engagement-strategy.png
[2]: ./media/mobile-engagement-media-scenario/push-scenarios.png

<!--Link references-->
[Media Playbook link]: https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks

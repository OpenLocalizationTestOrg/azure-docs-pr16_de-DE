<properties 
    pageTitle="Dienstbus Preise und Abrechnung | Microsoft Azure"
    description="Übersicht über die Struktur Preise Dienstbus."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/06/2016"
    ms.author="sethm" />

# <a name="service-bus-pricing-and-billing"></a>Dienstbus Preise und Abrechnung

Dienstbus wird in Basic, Standard und [Premium](service-bus-premium-messaging.md) -Versionen angeboten. Sie können auswählen, eine Stufe Dienst für jeden Dienstbus Dienstnamespace, die Sie erstellen, und diese Stufe Auswahl gilt für alle Personen, die in diesem Namespace erstellt.

>[AZURE.NOTE] Ausführliche Informationen zu den aktuellen Dienstbus Preise finden Sie unter der [Azure-Dienstbus Preise Seite](https://azure.microsoft.com/pricing/details/service-bus/)und der [Dienst Bus häufig gestellte Fragen](service-bus-faq.md#service-bus-pricing).

Dienstbus verwendet die folgenden zwei Meter für Warteschlangen und Themen/Abonnements:

1. **Vorgänge Messaging**: als API-Aufrufe an Warteschlange oder das Thema/Abonnement-Endpunkte definiert. Diese Anzeige ersetzt Nachrichten gesendet oder empfangen, als primäre Einheit für berechenbare Verwendung für Warteschlangen und Themen/Abonnements.

2. **Vermittelte Verbindungen**: definiert werden, wie die maximale Anzahl der beständigen Verbindungen mit Warteschlangen, Themen oder Abonnements während eines Zeitraums angegebenen einer Stunde werden öffnen. Diese Anzeige gilt nur in der Standardansicht Ebene, in dem Sie zusätzliche Verbindungen öffnen können (früher Verbindungen waren auf 100 pro Warteschlange/Thema/Abonnement beschränkt) für eine Gebühr pro Verbindung nominal.

Die **Standard** -Ebene führt Farbverläufe Preise für Operationen mit Warteschlangen und Themen/Abonnements, wodurch Volume-basierte senken von bis zu 80 % auf der obersten Ebene für die Verwendung. Es gibt auch eine Standard Ebene Basis Gebühr von 10 € pro Monat, womit Sie bis zu 12,5 Millionen Vorgänge pro Monat ohne zusätzliche Kosten ausführen kann.

Die Ebene **Premium** bietet Ressource Isolation auf der Ebene CPU- und an, sodass jeder Kunden Arbeitsbelastung isoliert ausgeführt wird. Diese Ressource Container wird als eine *Einheit messaging*bezeichnet. Jeder Namespace Premium mindestens eine messaging Einheit zugeordnet sind. Sie können 1, 2 oder 4 per Einheiten für jeden Dienst Bus Premium Namespace kaufen. Eine einzelne Arbeitsbelastung oder juristische kann mehrere messaging Einheiten umfassen, und die Anzahl der Einheiten messaging kann bei wird, geändert werden, zwar Abrechnung im 24-Stunden- oder tägliche Zins Gebühren ist. Das Ergebnis ist vorhersehbar und wiederholt Leistung für Ihre Dienstbus-basierte Lösung. Nicht nur diese Leistung ist, vorhersehbar und zur Verfügung, aber es sieht auch schneller. Azure Service Bus Premium messaging basiert auf der Speicher-Engine in Azure Ereignis Hubs eingeführt werden. Mit messaging Premium ist Höchstwert Leistung viel schneller als der Standard-Ebene.

Beachten Sie, dass der standard Basis Gebühr nur einmal pro Monat und Azure-Abonnement belastet wird. Dies bedeutet, dass nach dem Erstellen einer einzelnen Standard oder Premium Ebene Dienstbus Namespace Sie imstande sein sollen, erstellen Sie so viele zusätzliche Standard oder Premium Ebene Namespaces beliebig unter die gleichen Azure-Abonnement, ohne dass weitere base Gebühren anfallen.

Alle vorhandenen Dienstbus Namespaces vor 1 November 2014 erstellt wurden in der Standardansicht Ebene automatisch platziert. Dadurch wird sichergestellt, dass Sie weiterhin Zugriff auf alle Funktionen, die derzeit verfügbar mit Dienstbus haben. Anschließend können Sie das [Azure klassischen Portal][] , um die grundlegende Ebene heruntergestuft gegebenenfalls aus.

In der folgenden Tabelle werden die Funktionsunterschiede zwischen Basic und Standard/Premium Ebenen zusammengefasst.

|Funktion|Grundlegende|Standard-Premium|
|---|---|---|
|Warteschlangen|Ja|Ja|
|Geplante Nachrichten|Ja|Ja|
|Themen-Abonnements|Nein|Ja|
|Relays|Nein|Ja|
|Transaktionen.|Nein|Ja|
|Deduplizierung|Nein|Ja|
|Sitzungen|Nein|Ja|
|Große Nachrichten|Nein|Ja|
|ForwardTo|Nein|Ja|
|SendVia|Nein|Ja|
|Vermittelten Verbindungen (enthalten)|100 pro Dienstbus namespace|1.000 pro Azure-Abonnement|
|Vermittelten Verbindungen (Überschuss zulässig)|Nein|Ja (berechenbar)|

## <a name="messaging-operations"></a>Vorgänge Messaging

Als Teil der neuen Preisgestaltung Modell Abrechnung für Warteschlangen und Themen/Abonnements geändert hat. Diese Elemente Wechsel von Abrechnung pro Nachricht Abrechnungsinformationen pro Vorgang. Eine "Operation" bezieht sich auf eine beliebige API Anrufes gegen einen Endpunkt Warteschlange oder das Thema/Abonnement. Dies umfasst die Verwaltung, senden/empfangen und Sitzung Zustand Vorgänge.

|Vorgangstyp|Beschreibung|
|---|---|
|Projektmanagement|Erstellen Sie, lesen, aktualisieren, löschen (CRUD) gegen Warteschlangen oder Themen/Abonnements.|
|Messaging|Senden und Empfangen von Nachrichten mit Warteschlangen oder Themen/Abonnements.|
|Bundesstaat Sitzung|Erste oder Sitzungszustand auf eine Warteschlange oder ein Thema/Abonnement festlegen.|

Die folgenden Preise wurden effektiven 1 November 2014 beginnen:

|Grundlegende|Kosten|
|---|---|
|Vorgänge|0,05 $ pro Millionen Vorgänge|

|Standard|Kosten|
|---|---|
|Basis Gebühr|$10 pro Monat|
|Zuerst 12.5 Millionen Vorgänge pro Monat|Im Lieferumfang von|
|12.5-100 Millionen Vorgänge pro Monat|0,80 $ pro Millionen Vorgänge|
|100 Millionen – 2.500 Millionen Vorgänge pro Monat|0,50 $ pro Millionen Vorgänge|
|Über 2.500 Millionen Vorgänge pro Monat|0,20 US-Dollar pro Millionen Vorgänge|

|Premium|Kosten|
|---|---|
|Tägliche|$11.13 feste Zinssatz pro Einheit Nachricht|

## <a name="brokered-connections"></a>Vermittelten Verbindungen

*Brokered Verbindungen* Tabellenbereich Kunden Verwendungsmustern, die eine große Anzahl der Absender/Empfänger gegen Warteschlangen, Themen oder Abonnements "dauerhaft verbunden" an. Dauerhaft verbundenen Absender/Empfänger sind diejenigen, die eine Verbindung mit entweder AMQP oder HTTP mit einem Wert ungleich NULL Timeout (z. B. HTTP lange abrufen) erhalten. HTTP-Absender und Empfänger mit einer direkten Timeout generieren vermittelten Verbindungen nicht.

Wie zuvor Warteschlangen und Themen/Abonnements maximal 100 aktiven Verbindungen pro URL. Das aktuelle Abrechnung Schema entfernt die Beschränkung-URL für Warteschlangen und Themen/Abonnements und Kontingente und Erfassung auf vermittelten Verbindungen am Dienstbus Namespace und Azure-Abonnementebenen implementiert.

Die grundlegende Ebene enthält, und ist auf, 100 vermittelten Verbindungen pro Dienstbus Namespace grundsätzlich beschränkt. Die grundlegende Ebene werden Verbindungen über diese Nummer zurückgewiesen. Der Standard Ebene entfernt die Beschränkung pro Namespace und zählt aggregieren vermittelten Verbindung Verwendung über das Azure-Abonnement. In der Standardansicht Ebene 1.000 vermittelten Verbindungen pro Azure Abonnement ohne zusätzliche Kosten (neben der Basis Gebühr) zulässig. Mit mehr als 1.000 vermittelten Verbindungen insgesamt über Dienstbus Standard-Ebenen werden Namespaces in einem Azure-Abonnement Farbverläufe termingerecht Abrechnung wie in der folgenden Tabelle aufgelistet.

|Vermittelten Verbindungen (Standard Schicht)|Kosten|
|---|---|
|Ersten 1.000 pro Monat|Lieferumfang Basis Gebühr|
|1.000-100,000/Monat|$0,03 pro Verbindung/Monat|
|100,000-500.000/Monat|0,025 $ pro Verbindung/Monat|
|Über 500.000/Monat|$0.015 pro Verbindung/Monat|

>[AZURE.NOTE] 1.000 vermittelten Verbindungen sind in der Standardansicht messaging-Schicht (über die Basis Gebühr) enthalten und können über alle Warteschlangen, Themen und Abonnements innerhalb der zugeordneten Azure-Abonnement freigegeben werden.

<br />

>[AZURE.NOTE] Abrechnung basiert auf die maximale Anzahl der aktiven Verbindungen und ist anteilig stündlich basierend auf 744 Stunden pro Monat.

|Premium Ebene
|---|
|Vermittelten Verbindungen unterliegen nicht in der Ebene Premium.|

Weitere Informationen zu vermittelten Verbindungen finden Sie im Abschnitt " [häufig gestellte Fragen zu](#faq) " weiter unten in diesem Thema.

## <a name="relay"></a>Relay

Relays stehen nur in der Standardansicht Ebene Namespaces zur Verfügung. Preise und Verbindung Kontingente für Relays Andernfalls bleiben unverändert. Dies bedeutet, dass Relays weiterhin die Anzahl der Nachrichten (nicht-Vorgänge) belastet und Stunden weiterzuleiten.

|Preise weiterleiten|Kosten|
|---|---|
|Relay Stunden|$0.10 für alle 100 Relay Stunden|
|Nachrichten|0,01 US-Dollar für jede 10.000 Nachrichten|

## <a name="faq"></a>Häufig gestellte Fragen

### <a name="how-is-the-relay-hours-meter-calculated"></a>Wie wird die Anzeige Relay Stunden berechnet?

[In diesem Thema](service-bus-faq.md#how-is-the-relay-hours-meter-calculated)wird angezeigt.

### <a name="what-are-brokered-connections-and-how-do-i-get-charged-for-them"></a>Was sind vermittelte Verbindungen, und wie ich erhalten belastet für diese?

Eine vermittelten Verbindung wird als eine der folgenden definiert:

1. Eine AMQP-Verbindung von einem Client zu einem Dienstbus Warteschlange oder Thema/Abonnement.

2. Eine HTTP-Anruf erhalten Sie eine Nachricht von einer Dienstbus Thema oder Warteschlange mit einem Timeoutwert empfangen größer als 0 (null).

Dienstbus Gebühren für die maximale Anzahl der aktiven vermittelten Verbindungen, die die darin enthaltenen Menge (in der Standardansicht Ebene 1.000) überschreiten. Spitzen werden stündlich gemessen, anteilig werden durch 744 Stunden in einem Monat und addiert, über den monatlichen Abrechnungszeitraum. Die darin enthaltenen Menge (1.000 vermittelten Verbindungen pro Monat) wird am Ende der Abrechnungszeitraum gegen die Summe der anteilige stündlich Spitzen angewendet.

Beispiel:

1. Jeder der 10.000 Geräte über eine einzelne AMQP Verbindung verbindet, und Befehle aus einem Thema Dienstbus empfängt. Die Geräte senden werden Ereignisse an ein Ereignis Hub aus. Wenn alle Geräte für 12 Stunden täglich mit herstellen, die folgenden Verbindung anfallen (sowie alle anderen Dienstbus Thema Gebühren): 10.000 Verbindungen *12 Stunden* 31 Tage / 744 = 5.000 vermittelte Verbindungen. Nach den monatlichen Rabatt von 1.000 vermittelten Verbindungen möchten Sie für 4.000 vermittelten Verbindungen mit einer Geschwindigkeit von 0,03 $ pro vermittelten Verbindungstyp für insgesamt 120 $ in Rechnung gestellt.

2. 10.000 Geräte empfangen von Nachrichten aus einer Warteschlange Dienstbus über HTTP, einen nicht-NULL-Timeout angeben. Wenn eine alle Geräte für 12 Stunden täglich Verbindung, sehen Sie die folgenden Verbindungsgebühren (sowie alle anderen Dienstbus Gebühren): 10.000 erhalten HTTP-Verbindungen *12 Stunden pro Tag* 31 Tage / 744 Stunden = 5.000 vermittelte Verbindungen.

### <a name="do-brokered-connection-charges-apply-to-queues-and-topicssubscriptions"></a>Gelten vermittelten Verbindungsgebühren für Warteschlangen und Themen/Abonnements?

Ja. Es gibt keine Verbindungsgebühren für das Senden von Ereignissen über HTTP, unabhängig von der Anzahl der Betriebssysteme oder Geräte senden aus. Empfang von Ereignissen mit HTTP mithilfe eines Timeouts größer als 0 (null), auch als "langes abrufen," bezeichnet generiert vermittelten Verbindungsgebühren. AMQP Verbindungen generieren vermittelten Verbindungsgebühren unabhängig davon, ob die Verbindungen zum Senden oder Empfangen von verwendet werden. Beachten Sie, dass 100 vermittelten Verbindungen kostenlos in einem grundlegende Namespace zulässig sind. Dies ist auch die maximale Anzahl von vermittelten Verbindungen für das Abonnement Azure zulässig. Die ersten 1.000 vermittelten Verbindungen über alle Standard-Namespaces in einem Azure-Abonnement sind kostenlos (neben der Basis Gebühr) enthalten. Da diese softwarebegrenzungen genug, um viele messaging-Dienst-Szenarien Deckblatt sind, werden vermittelten Verbindungsgebühren normalerweise nur relevant, wenn Sie beabsichtigen, AMQP oder HTTP Long-Umfragen mit einer großen Anzahl von Clients verwenden; Wenn Sie beispielsweise erzielen effizienter Ereignis streaming oder bidirektionale Kommunikation mit vielen Geräten oder Anwendungsinstanzen zu aktivieren.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zur Preisgestaltung Dienstbus finden Sie unter der [Azure-Dienstbus Preise Seite](https://azure.microsoft.com/pricing/details/service-bus/).

- Finden Sie im [Service Bus häufig gestellte Fragen](service-bus-faq.md#service-bus-pricing) für einige allgemeine FAQs um Dienstbus Preise und Abrechnung ein.

[Azure klassischen-portal]: http://manage.windowsazure.com
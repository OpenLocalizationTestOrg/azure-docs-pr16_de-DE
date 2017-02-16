<properties 
    pageTitle="Azure Dienstbus | Microsoft Azure" 
    description="Eine Einführung in Dienstbus Verbindung Azure Applications zu anderen Software verwenden." 
    services="service-bus" 
    documentationCenter=".net" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/31/2016" 
    ms.author="sethm"/>

# <a name="azure-service-bus"></a>Azure Dienstbus

Ob eine Anwendung oder einen bestimmten Dienst in der Cloud oder lokal ausgeführt wird, muss es häufig mit anderen Anwendungen oder Dienste interagieren. Um gestreut hilfreiche Möglichkeit dazu, bietet Microsoft Azure Dienstbus. In diesem Artikel hat einen Blick auf diese Technologie, Beschreibung Zweck und warum Sie möglicherweise sie verwenden möchten.

## <a name="service-bus-fundamentals"></a>Dienstbus-Grundlagen

In unterschiedlichen Situationen für verschiedene Formate für die Kommunikation. Manchmal ist das ganz Applikationen senden und Empfangen von Nachrichten über eine einfache Warteschlange können die bestmögliche Lösung. In anderen Situationen reicht eine einfache Warteschlange; eine Warteschlange mit einem veröffentlichen und abonnieren, ist es besser. In einigen Fällen wirklich erforderlich, stellt eine Verbindung zwischen Clientanwendungen; Warteschlangen nicht erforderlich. Dienstbus enthält alle drei Optionen, aktivieren die Anwendung auf verschiedene Arten interagieren.

Dienstbus ist eine mit mehreren Mandanten Cloud-Dienst, was bedeutet, dass der Dienst von mehreren Benutzern gemeinsam genutzt wird. Jeder Benutzer, wie eine Anwendung Developer erstellt einen *Namespace*, und dann die Kommunikationsmechanismen, die sie innerhalb dieses Namespaces muss definiert. Abbildung 1 zeigt an, wie folgt aussieht.

![][1]
 
**Abbildung 1: Dienstbus bietet einen Mandanten mit mehreren für Applikationen über der Cloud verbinden.**

Innerhalb eines Namespace können Sie eine oder mehrere Instanzen von vier verschiedene Kommunikationsmechanismen verwenden, von die jede Applikationen auf andere Weise eine Verbindung herstellt. Die Auswahlmöglichkeiten sind:

- *Warteschlangen*, die eine Richtung möglich Kommunikation ermöglichen. Jede Warteschlange fungiert als Vermittler (auch als eine *Bank*bezeichnet), der speichert Nachrichten gesendet, bis sie empfangen werden. Jede Nachricht wird durch einen einzelnen Empfänger empfangen.
- *Themen*, die eine Richtung möglich Kommunikation mit *Abonnements*- ein einzelnes Thema zur Verfügung stellen können mehrere Abonnements besitzen. Wie eine Warteschlange ein Thema fungiert als eine Bank, aber jedes Abonnement können optional einen Filter nur Nachrichten empfangen werden sollen, die bestimmte Kriterien entsprechen.
- *Relays*, die bidirektionale Kommunikation zur Verfügung zu stellen. Im Gegensatz zu Warteschlangen und Themen speichern nicht Relay Flight-Nachrichten. Es ist keiner Bank. In diesem Fall übergeben es nur diese an die Ziel-Anwendung.

Wenn Sie eine Warteschlange, Thema oder Relay erstellen, geben Sie einen Namen. In Kombination mit was Sie Ihren Namespace bezeichnet, wird dieser Name einen eindeutigen Bezeichner für das Objekt erstellt. Applikationen können Dienstbus stellen Sie diesem Namen und dann verwenden Sie die Warteschlange, Thema oder Relay miteinander kommunizieren. 

Wenn Sie eins dieser Objekte im Szenario Relay verwenden, können Windows Applications Windows Communication Foundation (WCF). Warteschlangen und Themen können Windows Applications messaging APIs Dienstbus definiert. Um diese Objekte mit nicht-Windows Applications erleichtern, stellt Microsoft SDKs für Java, Node.js und anderen Sprachen. Sie können auch Warteschlangen und Themen mithilfe der REST-APIs über HTTP(s) zugreifen. 

Es ist wichtig, dass es sich, obwohl Service Bus selbst ausgeführt in der Cloud (d. h., in den Rechenzentren von Microsoft Azure), Anwendungen, die sie verwenden können überall ausführen. Dienstbus können Verbindung von Applications Azure, z. B. ausgeführt oder der Anwendung, die in Ihrem eigenen Datencenter ausgeführt. Sie können Sie auch unter Azure oder einer anderen Anwendung Verbindung verwenden Cloud-Plattform mit einer lokalen Anwendung oder Tablets und Telefone. Es ist auch möglich Haushalts, Sensoren und andere Geräte an einer zentralen Anwendung oder auf einen anderen zu verbinden. Dienstbus sind eine Communication in der Cloud, die überall ziemlich zugegriffen werden kann. Wie Sie verwenden, hängt was Ihre Applikationen tun müssen.

## <a name="queues"></a>Warteschlangen

Nehmen Sie an, dass Sie zwei Clientanwendungen mithilfe einer Warteschlange Dienstbus eine Verbindung herstellen möchten. Abbildung 2 veranschaulicht diese Situation.

![][2]
 
**Abbildung 2: Servicebuswarteschlangen bieten unidirektionale asynchrone queuing.**

Der Vorgang ist einfach: ein Absender sendet eine Nachricht an eine Warteschlange Dienstbus und ein Empfänger nimmt die Nachricht zu einem späteren Zeitpunkt. Eine Warteschlange kann nur einem einzelnen Empfänger haben, wie in Abbildung 2 dargestellt. Oder mehrere aus der gleichen Warteschlange lesen können. Im letzten Fall wird jede Nachricht von nur einem Empfänger gelesen. Mehrere cast Dienst sollten Sie ein Thema verwenden.

Jede Nachricht besteht aus zwei Teilen: eine Reihe von Eigenschaften, jedes Schlüssel/Wert-Paar und eine binäre Nachrichtentext. Wie sie es gewohnt sind, hängt davon ab, was zu tun ist eine Anwendung versucht. Senden einer Nachricht über ein Verkauf zuletzt verwendete Anwendung möglicherweise kann beispielsweise die Eigenschaften *Verkäufer = "Ava"* und *Betrag = 10000*. Nachrichtentext möglicherweise ein gescanntes Bild von den Verkauf des signierte Vertrag enthalten oder wenn nicht vorhanden ist, einfach leer bleiben.

Ein Empfänger kann eine Nachricht aus einer Warteschlange Dienstbus auf zwei verschiedene Arten gelesen werden. Die erste Option, mit der Bezeichnung *ReceiveAndDelete*, entfernt eine Nachricht in der Warteschlange und sofort gelöscht. Dies ist einfach, aber wenn der Empfänger stürzt ab, bevor er endet Verarbeiten der Nachricht, die Nachricht geht verloren. Da aus der Warteschlange entfernt wurden, kann keine anderen Empfänger darauf zugreifen. 

Die zweite Option, *PeekLock*, sollte bei diesem Problem helfen. Wie **ReceiveAndDelete**entfernt eine **PeekLock** lesen eine Nachricht in der Warteschlange an. Es wird jedoch nicht die Nachricht gelöscht. Stattdessen, ihn sperrt die Nachricht, sodass es für andere Empfänger, unsichtbar und wartet dann auf eine der drei Ereignisse:

- Wenn der Empfänger die Nachricht erfolgreich verarbeitet, es ruft **abgeschlossen**, und die Warteschlange löscht die Nachricht. 
- Wenn der Empfänger feststellen, dass die Nachricht erfolgreich verarbeitet werden kann, ruft **aufgeben**. Die Warteschlange entfernt die Sperre die Nachricht dann und für andere Empfänger zur Verfügung.
- Wenn der Empfänger keine davon in eine konfigurierbare Zeitspanne (standardmäßig 60 Sekunden) aufgerufen wird, wird die Warteschlange davon ausgegangen, dass der Empfänger ein Fehler aufgetreten ist. In diesem Fall verhält Sie sich, als wäre der Empfänger **aufgeben**, die Nachricht an andere Empfänger zur Verfügung stellen aufgerufen hätte.

Beachten Sie, was es passieren kann: dieselbe Nachricht möglicherweise übermittelt werden zweimal, vielleicht an zwei verschiedenen Empfänger. Applikationen Servicebuswarteschlangen verwenden, müssen für diese vorbereitet werden. Um die vereinfachen Erkennung von Duplikaten, jede Nachricht verfügt über eine eindeutige **Nachrichten-ID** -Eigenschaft, die standardmäßig gleich unabhängig davon, wie oft bleibt ist die Nachricht aus einer Warteschlange gelesen. 

Warteschlangen sind ein paar Situationen hilfreich. Sie können Anwendungen kommunizieren, selbst wenn beide zur gleichen Zeit, etwas ausgeführt nicht zur Verfügung, die besonders praktischen mit Stapel und mobile Clientanwendungen ist. Eine Warteschlange mit mehreren Empfängern enthält auch automatischen Lastenausgleich, da gesendete Nachrichten über diesen Empfänger verteilt sind.

## <a name="topics"></a>Themen

Hilfreich, wie sie sind, nicht Warteschlangen immer die richtige Lösung. In manchen Fällen sind Dienstbus Themen besser geeignet. Abbildung 3 veranschaulicht diese Idee.

![][3]
 
**Abbildung 3: Basierend auf den Filter, die, den eine Anwendung Abonnement gibt, können sie einige oder alle e-Mails von einem Thema Dienstbus erhalten.**

Ein *Thema* ist in vielen Punkten an eine Warteschlange vergleichbar. Absender übermitteln Nachrichten zu einem Thema in die gleiche Weise wie Übermitteln von Nachrichten an eine Warteschlange und die Nachrichten suchen ebenso wie bei Warteschlangen. Der große Unterschied ist, dass Themen jede empfangen Anwendung in einem eigenen *Abonnement* zu erstellen, indem Sie einen *Filter*definieren können. Ein Abonnent wird dann nur die Nachrichten angezeigt, die diesem Filter entsprechen. Abbildung 3 zeigt beispielsweise einen Absender und ein Thema mit drei Abonnenten, jede mit einem eigenen filtern:

- Abonnent 1 empfängt nur Nachrichten, die die Eigenschaft enthalten *Verkäufer = "Ava"*.
- Abonnent 2 empfängt Nachrichten, die die Eigenschaft enthalten *Verkäufer = "Ruby"* und/oder eine *Menge* Eigenschaft, deren Wert größer als 100,000 enthalten. Vielleicht ist Ruby im sales-Manager, damit sie finden Sie unter sowohl ein eigenes Sales und alle große Sales unabhängig davon, die sie macht möchte.
- Abonnenten 3 hat der Filters auf *True*festgelegt, d. h., dass sie alle Nachrichten erhält. Angenommen, dieser Anwendung möglicherweise für die Verwaltung einer Prüfliste zuständig und muss deshalb um alle Nachrichten anzuzeigen.

Wie bei Warteschlangen, können Abonnenten zu einem Thema mit **ReceiveAndDelete** oder **PeekLock**Nachrichten lesen. Im Gegensatz zu Warteschlangen kann eine einzelne Nachricht gesendet zu einem Thema jedoch, indem Sie mehrere Abonnements empfangen werden. Dieser Ansatz, der so genannte *Veröffentlichen und abonnieren* (oder *Pub/Sub*) ist nützlich, wenn mehrere in der gleichen interessiert sind. Durch den richtigen Filter definiert haben, kann jedem Abonnenten in nur den Teil der Nachricht Stream Tippen Sie auf, die angezeigt werden muss.

## <a name="relays"></a>Relays

Sowohl die Warteschlangen Themen bieten unidirektionale asynchrone Kommunikation bei einer Bank. Datenfluss in nur eine Richtung, und es besteht keine direkte Verbindung zwischen Absender und Empfänger. Doch was passiert, wenn Sie dies möchten nicht? Nehmen Sie an Ihrer Anwendung senden und Empfangen von Nachrichten müssen oder vielleicht eine direkte Verknüpfung zwischen gewünschten und nicht benötigten eine Bank zum Speichern von Nachrichten. Auf Adresse Szenarien wie dieser hier Dienstbus bietet *weiterleitet*, wie in Abbildung 4 dargestellt.

![][4]
 
**Abbildung 4: Dienstbus Relay bietet synchron, bidirektionale Kommunikation zwischen Applikationen.**

Handelt es sich um die wichtigste Frage Relays Fragen: Warum sollte ich eine verwenden? Auch wenn ich Warteschlangen nicht benötigen, warum stellen Applikationen über einen Clouddienst und nicht nur kommunizieren direkt interagieren? Die Antwort ist, dass ein Gespräch direkt kann schwieriger als Sie vielleicht denken.

Angenommen, Sie zwei lokale Clientanwendungen eine Verbindung herstellen möchten, beide ausgeführt innerhalb Ihres Unternehmens Rechenzentren. Jede dieser Anwendung befindet sich hinter einem Firewall, und jedes Rechenzentrum verwendet wahrscheinlich Netzwerk-Adresse (Netzwerkadressübersetzung). Die Firewall blockiert eingehende Daten auf alle jedoch einige Ports und NAT setzt voraus, dass der Computer, den jede Anwendung ausgeführt wird, klicken Sie auf, eine feste IP-Adresse aufweist, die Sie direkt von außerhalb des Datencenters erreichen können. Verbinden diese Applications über das öffentliche Internet ist ohne einige zusätzliche Hilfe beim problematisch.

Relay Dienstbus helfen kann. Bidirektionale Kommunikation über eine Relay kommunizieren, jede Anwendung stellt eine ausgehende TCP-Verbindung mit Dienstbus her, und klicken Sie dann geöffnet hält. Die gesamte Kommunikation zwischen den beiden Clientanwendungen für die Übertragung über diese Verbindungen. Da jede Verbindung im Datenzentrum hergestellt wurde, kann die Firewall eingehenden Datenverkehr jede Anwendung ohne neue Ports öffnen zu müssen. Dieser Ansatz ruft auch um das Problem NAT, da jede Anwendung einen konsistenten Endpunkt in der Cloud überall die Kommunikation hat. Durch den Austausch von Daten über das Relay, können die Probleme nach Möglichkeit nicht, die andernfalls Kommunikation erschweren würde. 

Um Dienstbus Relays verwenden, aufsetzen Applikationen auf Windows Communication Foundation (WCF). Dienstbus bietet WCF-Bindung, die es für Windows Applications zu interagieren über Relays recht einfach zu machen. Anwendungen, die bereits WCF verwenden können in der Regel nur Geben Sie eine der folgenden Bindungen, und dann miteinander durch eine Relay. Im Gegensatz zu Warteschlangen und Themen erfordert jedoch mit Relays nicht Windows Applications während möglich, einige Aufwand verbunden; keine standard-Bibliotheken stehen zur Verfügung.

Im Gegensatz zu Warteschlangen und Themen Erstellen nicht Applikationen explizit Relays. Wenn eine Anwendung, die Nachrichten empfangen möchte eine TCP-Verbindung mit Dienstbus herstellt, wird stattdessen Relay automatisch erstellt. Wenn die Verbindung getrennt ist, wird das Relay gelöscht. Um eine Anwendung zum Suchen eines bestimmten Zuhörer erstellten Relays zu ermöglichen, bietet Dienstbus eine Registrierung, mit dem Anwendungen zu einem bestimmten Relay nach Namen suchen kann.

Relays sind die richtige Lösung, wenn direkten Kommunikation zwischen Clientanwendungen benötigt wird. Angenommen Sie, eine Fluglinie Reservierungssystem ausgeführt wird, in einer lokalen Datencenters, der die Einchecken Kioske, mobilen Geräten und anderen Computern zugegriffen werden muss. Dadurch wird auf alle diese Systeme konnte auf Dienstbus Relays in der Cloud zu kommunizieren, verlassen, wo sie möglicherweise ausgeführt werden.

## <a name="summary"></a>Zusammenfassung

Verbinden von Applications war immer Teil der Erstellung von Lösungen abgeschlossen, und der Zellbereich, der Szenarien, die Anwendungen und Dienste kommunizieren müssen, auf die als weitere Applikationen vergrößern festgelegt ist, und Geräte mit dem Internet verbunden sind. Durch die Bereitstellung cloudbasierte Technologies erreichen Sie dies über Warteschlangen, Themen und Relays, soll Dienstbus diese wesentliche Funktion einfacher zu implementieren und breiterer Basis zur Verfügung stellen.

## <a name="next-steps"></a>Nächste Schritte

Die Grundlagen der Azure-Dienstbus bearbeitet haben, führen Sie die folgenden Links, um weitere Informationen.

- So verwenden Sie [Servicebuswarteschlangen](service-bus-dotnet-get-started-with-queues.md)
- So verwenden Sie [Dienstbus Themen](service-bus-dotnet-how-to-use-topics-subscriptions.md)
- So verwenden Sie [Dienstbus relay](../service-bus-relay/service-bus-dotnet-how-to-use-relay.md)
- [Beispiele für Dienstbus](service-bus-samples.md)

[1]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_01_architecture.png
[2]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_02_queues.png
[3]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_03_topicsandsubscriptions.png
[4]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_04_relay.png

<properties
    pageTitle="Übersicht über Dienstbus messaging | Microsoft Azure"
    description="Service Bus Messaging: flexible Datenübertragung in der cloud"
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="get-started-article"
    ms.date="09/27/2016"
    ms.author="sethm"/>


# <a name="service-bus-messaging-flexible-data-delivery-in-the-cloud"></a>Dienstbus messaging: flexible Datenübertragung in der Cloud

Microsoft Azure-Dienstbus ist ein verlässliche Informationen-Übermittlung-Dienst. Der Zweck dieses Diensts ist Kommunikation zu erleichtern. Wenn zwei oder mehr Parteien Informationen austauschen möchten, benötigen sie einen Kommunikationsmechanismus. Dienstbus wird vermittelten oder Drittanbieter-Kommunikation. Dies ist ähnlich wie in der physischen Welt postalische Dienstleistungen. Dienste erleichtern sehr verschiedene Arten von Briefe und Pakete mit einer Vielzahl von Garantien Übermittlung an einer beliebigen Stelle in der Welt senden.

Ähnelt den postalischen Dienst vorführen Buchstaben und Dienstbus ist flexible Informationsübermittlung aus sowohl der Absender als auch der Empfänger. Messaging-Dienstes wird sichergestellt, dass die Informationen bereitgestellt wird, auch wenn die beiden Parteien nie beide online zur gleichen Zeit sind, oder wenn sie gleichzeitig die genauen nicht verfügbar sind. Auf diese Weise ist ein messaging ähnelt dem Senden eines Buchstabens, während der Kommunikation mit nicht-vermittelte ist vergleichbar einen Anruf (oder ein Anruf wie verwendet, werden Sie vor dem Anruf warten und Anrufer-ID, die sind viel mehr wie vermittelten messaging -).

Der Absender der Nachricht kann auch eine Vielzahl von Übermittlung Merkmale, einschließlich Transaktionen, Erkennung von Duplikaten, zeitbasierte Ablauf und Batchverarbeitung erforderlich. Diese Muster haben ebenfalls postalische Analogien: Wiederholen Sie die Übermittlung, erforderliche Signatur, Adresse ändern oder Rückruf.

Dienstbus unterstützt zwei unterschiedlichen messaging Muster: *Relay* und *vermittelten messaging*.

## <a name="service-bus-relay"></a>Service Bus Relay

Die Komponente [Relay](../service-bus-relay/service-bus-relay-overview.md) Service-ist ein zentrales (aber hochgradig Lastenausgleich)-Dienst, der eine Vielzahl von anderen Transportprotokolle und Standards-Webdiensten unterstützt. Dies umfasst SOAP, ws-*, und sogar REST. [Relay Service](../service-bus-relay/service-bus-dotnet-how-to-use-relay.md) bietet eine Vielzahl von Optionen für die verschiedenen Relay Netzwerkkonnektivität und direkte Peer-to-Peer-Verbindungen handeln, wenn dies möglich ist, helfen kann. Dienstbus ist für .NET-Entwickler, die verwenden die Windows Communication Foundation (WCF), beide im Hinblick auf die Leistung und Nutzbarkeit, optimiert und Vollzugriff auf deren Relay Service über SOAP und die übrigen Schnittstellen enthält. Dadurch kann SOAP oder REST programming Umgebung Dienstbus integriert werden soll.

Der Relaydienst unterstützt herkömmliche unidirektionale messaging, Anforderung/Antwort messaging und Peer-to-Peer-messaging. Es unterstützt auch die Verteilung von Ereignissen im Internet-Gültigkeitsbereich aktivieren Veröffentlichen-Abonnieren Szenarien und bidirektionale Socketkommunikation für höhere Punkt Effizienz. Im weitergeleitete messaging Muster ein lokalen Dienst an den Relaydienst über einen ausgehenden Port verbindet und ein Sockets bidirektionale Kommunikation verknüpft an eine bestimmte Rendezvous Adresse erstellt. Der Client kann dann mit dem lokalen Dienst kommunizieren, durch Senden von Nachrichten an den Relaydienst verwendet die Adresse Rendezvous. Der Relay Service, klicken Sie dann "Nachrichten an den lokalen Dienst über bidirektionale Sockets bereits direkte leiten". Der Client benötigt eine direkte Verbindung mit dem lokalen Dienst nicht noch ist es erforderlich, wissen, wo sich der Dienst befindet, und der lokalen Dienst benötigt keine eingehenden Ports in der Firewall geöffnet.

Sie initiieren die Verbindung zwischen Ihrem lokalen Dienst und den Relaydienst mithilfe einer Suite von WCF "Relay" Bindungen. Hintergrundinformationen ordnen Sie die Relay Bindungen zum Transportieren Bindungselemente so ausgelegt, dass WCF-Kanal-Komponenten, die mit Dienstbus in der Cloud zu integrieren.

Service Bus Relay bietet viele Vorteile, aber erfordert, dass der Server und Client auf beide gleichzeitig um senden und Empfangen von Nachrichten online sein. Dies ist nicht optimal, für die HTTP-Schreibweise Kommunikation, in dem die Anfragen möglicherweise nicht in der Regel langer Lebensdauer sind noch für Clients, die eine Verbindung nur gelegentlich, z. B. Browser, mobile Anwendungen und so weiter. Vermittelte messaging unterstützt Entkoppelter Kommunikation und hat einen eigenen Vorteile; Wenn erforderlich, und führen Sie die Vorgänge auf asynchrone Weise, können Clients und Server verbinden.

## <a name="brokered-messaging"></a>Vermittelte messaging

Im Gegensatz zu den Farbschema Relay können [vermittelten messaging](service-bus-queues-topics-subscriptions.md) oder werden als asynchrone vorstellen, "temporär abgekoppelt." Hersteller (Absender) und Verbraucher (der Empfänger) müssen nicht gleichzeitig online sein. Die messaging-Infrastruktur speichert Nachrichten in einem "Bank" zuverlässig (beispielsweise eine Warteschlange), bis die in Anspruch nehmen Partei erhalten sie bereit ist. Dadurch wird die Komponenten einer verteilten Anwendung, entweder freiwillig getrennt werden; Angenommen, für die Wartung oder aufgrund einer Komponente Absturz, ohne Auswirkungen im gesamte System. Darüber hinaus möglicherweise die Anwendung empfangende nur online bestimmten Zeiten des Tages, wie etwa ein Inventory Management-System stammen, die nur zum Ausführen am Ende des Arbeitstags erforderlich ist.

Die Hauptkomponenten von der Dienstbus vermittelten messaging-Infrastruktur sind Warteschlangen, Themen und Abonnements.  Der primäre Unterschied ist, dass Themen Veröffentlichen/Abonnieren-Funktionen unterstützt, die für anspruchsvolle Content-basierte Weiterleitung und Zustellung Logik, einschließlich senden an mehrere Empfänger verwendet werden können. Diese Komponenten neue asynchrone messaging-Szenarien, z. B. zeitliche Entkopplung aktivieren, Veröffentlichen/Abonnieren und Lastenausgleich. Weitere Informationen zu diesen Einheiten messaging finden Sie unter [Servicebuswarteschlangen, Themen und Abonnements](service-bus-queues-topics-subscriptions.md).

Wie bei der Infrastruktur Relay wird die vermittelten messaging Videofunktionen für WCF und .NET Framework-Programmierer und auch über REST bereitgestellt.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Dienstbus messaging zu erhalten, finden Sie unter den folgenden Themen.

- [Dienstbus-Grundlagen](service-bus-fundamentals-hybrid-solutions.md)
- [Servicebuswarteschlangen, Themen und Abonnements](service-bus-queues-topics-subscriptions.md)
- [So verwenden Sie Servicebuswarteschlangen](service-bus-dotnet-get-started-with-queues.md)
- [Verwenden von Dienstbus Themen und Abonnements](./service-bus-dotnet-how-to-use-topics-subscriptions.md)
 

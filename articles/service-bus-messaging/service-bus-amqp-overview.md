<properties 
    pageTitle="Übersicht über die Service Bus AMQP | Microsoft Azure" 
    description="Informationen Sie zur Verwendung der erweiterte Message Queuing-Protokoll (AMQP) 1.0 in Azure." 
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
    ms.topic="article" 
    ms.date="09/29/2016" 
    ms.author="sethm"/>



# <a name="amqp-10-support-in-service-bus"></a>AMQP 1.0-Unterstützung in Dienstbus

Die Dienstbus Azure-Cloud-Dienst und die lokale [Service Bus für Windows Server (Service Bus 1.1)](https://msdn.microsoft.com/library/dn282144.aspx) Unterstützung für die erweiterte Nachricht einfügen in die Warteschlange Protokoll (AMQP) 1.0. AMQP können Sie zwischen Plattformen, Hybrid Applications mithilfe eines open standard-Protokolls zu erstellen. Sie können mithilfe von Komponenten, die mit verschiedenen Sprachen und Framework integriert sind, und die auf verschiedenen Betriebssystemen ausgeführt, Applications erstellen. Alle diese Komponenten Dienstbus und nahtlos können eine Verbindung austauschen strukturierter Nachrichten effizient und am vollständige Genauigkeit.

## <a name="introduction-what-is-amqp-10-and-why-is-it-important"></a>Einführung: Was ist AMQP 1.0 und warum es wichtig ist?

In der Vergangenheit haben Nachricht Orientierung Middleware Produkte eigene Protokolle für die Kommunikation zwischen Clientanwendungen und Makler verwendet. Dies bedeutet, nachdem Sie per Bank eines bestimmten Herstellers ausgewählt haben, müssen Sie Clientanwendungen auf die Bank Verbindung des Herstellers Bibliotheken verwenden. Das Ergebnis ein Maß in Abhängigkeit von dem Anbieter, da Portieren einer Anwendung zu einem anderen Produkt Code Änderungen in der alle verbundenen Clientanwendungen erforderlich ist. 

Darüber hinaus ist das Herstellen einer Verbindung per Makler von anderen Herstellern knifflig. Hierfür wird in der Regel Anwendung Ebene bridging Nachrichten von einem System in einen anderen wechseln und ihre eigene Nachrichtenformate übersetzen. Dies ist eine allgemeine Anforderung; beispielsweise wenn Sie müssen eine neue unified-Benutzeroberfläche für ältere unterschiedliche Systeme bereitstellen oder folgen einer Beurteilung von Fusionen IT-Systeme integrieren.

Der Software-Branche ist ein Geschäft machen verschieben. neue programming Sprachen und Anwendungsframeworks sind in einer manchmal verwirrend Tempo eingeführt werden. Entsprechend die Anforderungen der IT-Systeme über einen Zeitraum weiterentwickelt und Entwickler die neuesten Plattformfunktionen nutzen möchten. Jedoch unterstützt manchmal ausgewählten messaging Lieferanten diese keine Plattformen. Da messaging-Protokolle eigene haben, ist es nicht möglich, für andere Bibliotheken für diese neuen Plattformen bieten. Daher müssen Sie verwenden Ansätze erstellen Gateways oder Brücken, mit die Sie weiterhin das messaging Produkt verwenden können.

Die Entwicklung von der erweiterten Nachricht Queuing-Protokoll (AMQP) 1.0 wurde durch diese Probleme davon. Er stammt am JP Morgan Chase, die, wie die meisten financial Services Firmen, beanspruchen Benutzer der Nachricht Orientierung Middleware sind. Das Ziel war einfach: ein Öffnen Standard-Protokoll zu erstellen, die es ermöglicht Nachrichten basierende Applikationen integrierten Komponenten mit verschiedenen Sprachen, Framework und Betriebssysteme, erstellen, alle am besten aller Zeiten Komponenten aus einer Reihe von Lieferanten verwenden.

## <a name="amqp-10-technical-features"></a>AMQP 1.0 technische Funktionen

AMQP 1.0 ist ein effizienten, zuverlässigen und auf niedriger Ebene messaging-Protokoll, mit denen Sie erstellen robuste, Plattform-messaging Applications. Das Protokoll verfügt über ein einfaches Ziel: die Funktionsweise der sicheren, zuverlässigen und effizienten Übertragung von Nachrichten zwischen zwei Parteien definieren. Die Nachrichten selbst werden codiert mit einer Darstellung der tragbaren Daten, die heterogene Absender und Empfänger zum Austauschen von strukturierten Business Nachrichten bei vollständiger Genauigkeit ermöglicht. Im folgenden finden eine Zusammenfassung der wichtigsten Features:

*    **Effizient**: AMQP 1.0 ist eine Verbindung Orientierung Protokoll, verwendet eine binäre Codierung für das Protokoll Anweisungen und die geschäftliche Nachrichten über die Schaltfläche übertragen. Es übernimmt anspruchsvolle strömungsregelung Schemas für die Nutzung des Netzwerks und die verbundenen Komponenten maximieren. Dies bedeutet, dass das Protokoll entwickelt wurde, eine Balance zwischen Effizienz, Flexibilität und Interoperabilität zu finden.
*    **Zuverlässig**: der AMQP 1.0-Protokoll ermöglicht Nachrichten mit einem Spektrum Zuverlässigkeitsgarantien, aus auslösen und vergessen, zuverlässig und genau ausgetauscht werden – einmal Übermittlung bestätigt.
*    **Flexible**: AMQP 1.0 ist ein flexible Protokoll, das zur Unterstützung von verschiedenen Topologies verwendet werden kann. Dasselbe Protokoll kann für Client-zu-Client, Client-zu-Makler, und Bank-Bank-Kommunikation verwendet werden.
*    **Bank-Modell unabhängige**: der AMQP 1.0-Spezifikation keine Anforderungen stellen Sie auf die e-Mail-Modell bei einer Bank verwendet. Dies bedeutet, dass es möglich so einfach AMQP 1.0-Unterstützung vorhandenen messaging Makler hinzu.

## <a name="amqp-10-is-a-standard-with-a-capital-s"></a>AMQP 1.0 ist ein Standard (mit eines Großbuchstaben ')

AMQP 1.0 ist ein internationale Standard, von ISO und IEC als ISO/IEC 19464:2014 genehmigt.

AMQP 1.0 wurde in der Entwicklung seit 2008 von einer zentralen Gruppe von mehr als 20 Unternehmen, sowohl die Technologie Lieferanten Endbenutzer Firmen. Während dieses Zeitraums Benutzer Firmen haben ihre Anforderungen praktisches Business beigetragen und die IT-Anbieter haben entwickeltes das Protokoll aus, um diese zu erfüllen. Über den gesamten Prozess haben Lieferanten in Workshops teilgenommen in denen sie gemeinsam im Zusammenhang mit die Interoperabilität von deren Implementierungen überprüfen.

Oktober 2011 wurde die Entwicklungsarbeit auf einer Fachausschuß innerhalb der Organisation für die Advancement of Structured Information Standards (OASIS) und den Standardversionen OASIS AMQP 1.0 umgestellt wurden im Oktober 2012 veröffentlicht. Die folgenden Unternehmen teilgenommen der Fachausschuß bei der Entwicklung des Standards:

*    **IT-Anbieter**: Axway Software, Huawei Technologien, IT Software, INETCO Systeme, Kaazing, Microsoft, Mitre Deutschland GmbH, Primeton Technologien, den Fortschritt Software, Rot Hat, SITA, Software AG, Solace Systeme, VMware, WSO2, Zenika.
*    **Benutzer Unternehmen**: Bank von Nordamerika, Kreditkarte Suisse, Deutsche Boerse, Goldman Sachs, JPMorgan Chase.

Einige der Vorteile von open Standards häufig einbeziehen möchten:

*    Geringer die Wahrscheinlichkeit von Lieferanten
*    Interoperabilität
*    Allgemeinen Verfügbarkeit von Bibliotheken und Tools
*    Schutz vor dem neuesten Stand
*    Verfügbarkeit von erfahrenen Mitarbeiter
*    Unteren und verwaltbare Risiko

## <a name="amqp-10-and-service-bus"></a>AMQP 1.0 und Service Bus

AMQP 1.0-Unterstützung in Azure Service Bus bedeutet, dass können Sie jetzt die Dienstbus queuing nutzen und Veröffentlichen/Abonnieren von vermittelten messaging Features aus einer Reihe von Plattformen über eine effiziente binäre Protokoll. Darüber hinaus können Sie Applikationen durch eine Mischung von Sprachen, Framework und Betriebssysteme erstellte Komponenten besteht aus erstellen.

Die folgende Abbildung zeigt eine Beispiel für Bereitstellung in der Java-Clients unter Linux, mit dem standardmäßigen Java Message Service (JMS) API und .NET-Clients, die unter Windows, geschrieben Nachrichten über Dienstbus AMQP 1.0 mit exchange.

![][0]

**Abbildung 1: Beispiel Bereitstellungsszenario mit Plattformen messaging Dienstbus und AMQP 1.0 verwenden**

Zu diesem Zeitpunkt sind die folgenden Clientbibliotheken für die Arbeit mit Dienstbus bekannt:

| Sprache | Bibliothek                                                                       |
|----------|-------------------------------------------------------------------------------|
| Java     | Apache Qpid Java Message Service (JMS) client<br/>IT-Software SwiftMQ Java-client |
| C        | Apache Qpid Proton-C                                                          |
| PHP      | Apache Qpid Proton-PHP                                                        |
| Python   | Apache Qpid Proton-Python                                                     |
| C#       | AMQP .net Lite                                                                |

**Abbildung 2: Im Inhaltsverzeichnis AMQP 1.0-Client-Bibliotheken**

## <a name="summary"></a>Zusammenfassung

*    AMQP 1.0 ist ein öffnen, zuverlässigen messaging-Protokoll, mit denen Sie Plattformen, Hybrid Applications erstellen. AMQP 1.0 ist ein OASIS-Standard.
*    Unterstützung für AMQP 1.0 steht jetzt in Azure Service Bus als auch Service Bus für Windows Server (Service Bus 1.1). Preise entspricht dem für die vorhandenen Protokolle.

## <a name="next-steps"></a>Nächste Schritte

Möchten Sie mehr erfahren? Besuchen Sie die folgenden Links:

- [Verwenden von .NET Dienstbus mit AMQP]
- [Verwenden von Java Dienstbus mit AMQP]
- [Verwenden von Python Dienstbus mit AMQP]
- [Verwenden von PHP Dienstbus mit AMQP]
- [Installieren von Apache Qpid Proton-C ein Azure Linux virtuellen Computers]
- [AMQP in Dienstbus für WindowsServer]

[0]: ./media/service-bus-amqp-overview/service-bus-amqp-1.png
[Verwenden von .NET Dienstbus mit AMQP]: service-bus-amqp-dotnet.md
[Verwenden von Java Dienstbus mit AMQP]: service-bus-amqp-java.md
[Verwenden von Python Dienstbus mit AMQP]: service-bus-amqp-python.md
[Verwenden von PHP Dienstbus mit AMQP]: service-bus-amqp-php.md
[Installieren von Apache Qpid Proton-C ein Azure Linux virtuellen Computers]: service-bus-amqp-apache.md
[AMQP in Dienstbus für WindowsServer]: https://msdn.microsoft.com/library/dn574799.aspx
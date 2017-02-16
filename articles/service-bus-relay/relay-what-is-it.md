<properties
    pageTitle="Was ist Azure Relay? | Microsoft Azure"
    description="Übersicht über Azure Relay"
    services="service-bus"
    documentationCenter=".net"
    authors="banisadr"
    manager="timlt"
    editor="" />

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/28/2016"
    ms.author="babanisa" />

# <a name="what-is-azure-relay"></a>Was ist Azure Relay?

Azure Relay Service erleichtert Ihre Hybrid Applications ermöglicht es Ihnen sicher Services verfügbar machen, die befinden sich in einem Netzwerk Ihres Unternehmens Enterprise für die öffentliche Cloud, ohne eine Firewall-Verbindung zu öffnen oder muss eine Infrastruktur Unternehmensnetzwerk Einfluss geändert werden. Relay unterstützt eine Vielzahl von anderen Transportprotokolle und Standards-Webdiensten.

Der Relaydienst unterstützt herkömmliche unidirektionale, Anforderung/Antwort und Peer-to-Peer-Datenverkehr an. Darüber hinaus unterstützt Verteilung von Ereignissen im Internet-Gültigkeitsbereich Szenarien veröffentlichen/abonnieren und bidirektionale Socketkommunikation für höhere Punkt Effizienz aktivieren. 

Im Muster Übertragung weitergeleitete Daten ein lokalen Dienst an den Relaydienst über einen ausgehenden Port verbindet und ein Sockets bidirektionale Kommunikation verknüpft an eine bestimmte Rendezvous Adresse erstellt. Der Client kann dann mit dem lokalen Dienst kommunizieren, durch den Datenverkehr an den Relaydienst verwendet, die Rendezvous Adresse senden. Der Relay Service, klicken Sie dann "Daten vom lokalen Dienst durch eine bidirektionale Sockets an jeden Client dedizierter leiten". Der Client benötigt eine direkte Verbindung mit dem lokalen Dienst nicht, ist es nicht erforderlich zu wissen, wo sich der Dienst befindet, und der lokalen Dienst benötigt keine eingehenden Ports in der Firewall geöffnet.

Die Hauptfunktionen Elemente von Relay bereitgestellt werden bidirektionale, nicht zwischengespeicherten Kommunikation über Netzwerk hinweg mit TCP-ähnliche begrenzungsebene, Endpunkt Discovery, Connectivity Status und überlagert an Endpunkt Sicherheit. Funktionen, die den Relay unterscheiden sich von Netzwerk Ebene Integration Technologien wie VPN in, dass es kann an einen einzelnen Anwendungsendpunkt auf einem einzelnen Computer, begrenzt sein während der VPN-Technologie hat viel mehr Einfluss, da es zum Ändern von Datenbankobjekten im Netzwerk Umgebung abhängig.

Azure Relay verfügt über zwei Features:

1. [Hybrid Verbindungen](#hybrid-connections) - verwendet open standard Web Sockets für mehrere Plattformen Szenarien aktivieren

2. [Relays WCF](#wcf-relays) - verwendet Windows Communication Foundation Remote zu Vorgehensweisen Anrufe aktivieren

Aktivieren Hybrid Verbindungen und WCF Relays sichere Verbindung mit Assests, die in einem Netzwerk Ihres Unternehmens Enterprise vorhanden sind. Verwendung der übereinander liegende ist Ihre Bedürfnisse unter detaillierte abhängig:

|                                    | WCF-Relay | Hybrid-Verbindungen |
| ---------------------------------- |:---------:|:------------------:|
| **WCF**                            |     x     |                    |
| **.NET core**                      |           |         x          |
| **.NET framework**                 |     x     |         x          |
| **JavaScript/NodeJS**              |           |         x          |
| **Java***                          |           |         x          |
| **Offenes Protokoll standardisierten**  |           |         x          |
| **Mehrere RPC Programing Modelle** |           |         x          |
*<sub>Durch die allgemeine Verfügbarkeit</sub>

## <a name="hybrid-connections"></a>Hybrid-Verbindungen

Azure Relay Hybrid Verbindungen Videofunktionen eine sichere, öffnen-Protokoll Entwicklung Relay vorhandenen Features ist, die werden können implementiert auf einer beliebigen Plattform und in einer beliebigen Sprache, die eine einfache WebSocket-Funktion enthält, wozu auch explizit die WebSocket-API gemeinsam Webbrowser. Hybrid Verbindungen basiert auf HTTP und WebSockets.

## <a name="wcf-relays"></a>WCF Relays

Der WCF-Relay funktioniert für den vollständigen .NET Framework (NETFX) und für WCF. Sie einleiten die Verbindung zwischen Ihrem Dienst lokal und den Relaydienst mit einer Reihe von WCF "Relay" Bindungen. Hintergrundinformationen ordnen Sie die Relay Bindungen neue Transport Bindungselemente so ausgelegt, dass WCF-Kanal-Komponenten, die in der Cloud Dienstbus integriert werden soll.

## <a name="service-history"></a>Dienst Verlauf

Hybrid Verbindungen transplants den früheren, gleichmäßig mit der Bezeichnung "BizTalk-Dienste" Features, die auf dem Azure Service Bus WCF-Relay erstellt wurde. Der neuen Funktionen zum Hybrid Verbindungen Ergänzung der vorhandenen WCF-Relay, und diese zwei Service-Funktionen bleiben erhalten nebeneinander in den Relaydienst für die Zukunft vorhersehbar; Sie einen allgemeinen Gateway freigeben, sind jedoch andernfalls anderen Implementierungen.

WCF-Relay ist das Angebot, das mit ihrer WCF-Modelle Programing möglicherweise bereits viele Kunden verwenden älterer Relay.

## <a name="next-steps"></a>Nächste Schritte:

- [Relay häufig gestellte Fragen](relay-faq.md)
- [Erstellen Sie einen namespace](relay-create-namespace-portal.md)
- [Erste Schritte mit .NET](relay-hybrid-connections-dotnet-get-started.md)
- [Erste Schritte mit Knoten](relay-hybrid-connections-node-get-started.md)
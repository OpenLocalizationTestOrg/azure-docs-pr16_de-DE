<properties 
    pageTitle="Dienst Busarchitektur | Microsoft Azure"
    description="Beschreibt die Nachricht und Relay Azure Service-Architektur verarbeitet."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="07/11/2016"
    ms.author="sethm" />

# <a name="service-bus-architecture"></a>Dienst Busarchitektur

In diesem Artikel werden die Nachricht und Relay Azure Service-Architektur verarbeitet werden.

## <a name="service-bus-scale-units"></a>Maßeinheiten Dienstbus

Dienstbus ist nach *Maßeinheiten*angeordnet. Eine Skalierungseinheit ist eine Einheit von Bereitstellung und enthält alle erforderliche Komponenten, führen Sie den Dienst. Jeder Region bereitstellt, eine oder mehrere Dienstbus Maßstab Einheiten.

Ein Namespace Dienstbus wird eine Einheit Maßstab zugeordnet. Die Mengen Einheit verarbeitet Dienstbus Elementtypen: Relays und vermittelten messaging Einheiten (Warteschlangen, Themen, Abonnements). Eine Dienstbus Mengen Einheit besteht aus den folgenden Komponenten:

- **Eine Reihe von Gateway-Knoten.** Gateway-Knoten authentifizieren eingehende Anfragen und Relay Anfragen behandelt. Jede Gateway-Knoten verfügt über eine öffentliche IP-Adresse.

- **Eine Reihe von messaging Bank Knoten.** Messaging Bank Knoten verarbeiten Vorschläge bezüglich messaging Einheiten.

- **Ein Gateway-Store.** Der Gateway-Store enthält die Daten für jede Person, die in diese Mengen Einheit definiert ist. Der Gateway-Store wird über eine SQL Azure-Datenbank implementiert.

- **Mehrere messaging Stores.** Messaging Stores halten Sie die Nachrichten aller Warteschlangen, Themen und Abonnements, die in diese Mengen Einheit definiert sind. Darüber hinaus werden alle Abonnementdaten enthält. Es sei denn, [partitionierte messaging Personen](service-bus-partitioning.md) aktiviert ist, wird eine Warteschlange oder ein Thema ein messaging Store zugeordnet. Abonnements sind in der gleichen messaging als ihren übergeordneten Thema gespeichert. Eine Ausnahme bilden jedoch Dienstbus [Premium Messaging](service-bus-premium-messaging.md)sind die messaging Stores über SQL Azure-Datenbanken implementiert.

## <a name="containers"></a>Container

Jede messaging Entität, die einen bestimmten Container zugeordnet ist. Ein Container ist eine logische erstellen, die genau ein messaging zu speichern alle relevante Daten für diesen Container verwendet. Jeder Container wird ein messaging Bank Knoten zugewiesen. Es gibt in der Regel Weitere Container als messaging Bank Knoten. Daher lädt jeder messaging Bank Knoten mehrere Container. So, dass alle per Bank Knoten gleichmäßig geladen wurden, ist die Verteilung der Container zu einem messaging Bank Knoten angezeigt. Wenn die Last Muster Änderungen (beispielsweise eine sehr beschäftigt werden vom Container abgerufen), oder wenn ein messaging Bank Knoten vorübergehend nicht verfügbar ist, werden die Container zwischen den messaging Bank Knoten verteilt.

## <a name="processing-of-incoming-messaging-requests"></a>Verarbeitung von eingehenden messaging Anforderungen

Wenn ein Client eine Anforderung an Dienstbus sendet, leitet Sie Azure Lastenausgleich an allen Knoten Gateway weiter. Der Gateway-Knoten autorisiert die Anfrage. Wenn die Anforderung eine messaging Entität (Warteschlange, Thema, Abonnement) betrifft, wird der Gateway-Knoten sucht nach der Entität im Speicher Gateway und bestimmt, in welchem messaging Speicher die Entität befindet. Es sieht so aus dann von welcher messaging Bank Knoten dieser Container wird derzeit Wartung und sendet die Anforderung an diesen messaging Bank Knoten. Der messaging Bank-Knoten verarbeitet die Anforderung und den Status der Entität im Container Store aktualisiert. Der messaging Bank Knoten sendet die Antwort an den Gateway-Knoten, der eine entsprechende Antwort an den Client sendet, die die ursprüngliche Anforderung ausgegeben.

![Verarbeitung von eingehenden Messaging Anforderungen](./media/service-bus-architecture/IC690644.png)

## <a name="processing-of-incoming-relay-requests"></a>Verarbeitung von Besprechungsanfragen für eingehende relay

Wenn ein Client eine Anforderung an Dienstbus sendet, leitet Sie Azure Lastenausgleich an allen Knoten Gateway weiter. Ist die Anforderung einer überwachenden Anforderung, wird der Gateway-Knoten eines neuen Relay erstellt. Ist die Anforderung die Anforderung einer Verbindung zu einem bestimmten Relay, leitet der Gateway-Knoten Anforderung der Verbindung mit dem Gateway-Knoten, der das Relay besitzt. Der Gateway-Knoten, der das Relay besitzt, sendet eine Anforderung Rendezvous an den überwachenden Client, fragt der Zuhörer einen temporären Kanal zu den Gateway-Knoten erstellen, die die Anforderung Verbindung empfangen.

Wenn die Relay-Verbindung hergestellt wurde, können die Clients Nachrichten über den Gateway-Knoten austauschen, die für die Rendezvous verwendet wird.

![Verarbeitung von Besprechungsanfragen für eingehende Relay](./media/service-bus-architecture/IC690645.png)

## <a name="next-steps"></a>Nächste Schritte

Jetzt, da Sie gelesen haben eine Übersicht über Dienstbus Architektur, um anzufangen finden Sie auf die folgenden Links:

- [Übersicht über Dienstbus messaging](service-bus-messaging-overview.md)
- [Dienstbus-Grundlagen](service-bus-fundamentals-hybrid-solutions.md)
- [Eine Lösung für in der Warteschlange messaging Servicebuswarteschlangen verwenden](service-bus-dotnet-multi-tier-app-using-service-bus-queues.md)

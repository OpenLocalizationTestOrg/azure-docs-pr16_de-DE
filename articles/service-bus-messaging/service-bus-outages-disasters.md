<properties 
    pageTitle="Isoliert Dienstbus Applikationen gegen Ausfall und Schäden | Microsoft Azure"
    description="Beschreibt Techniken, die Sie zum Schutz von Applications gegen ein potenzieller Dienstbus Ausfall verwenden können."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="tysonn" /> 
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/02/2016"
    ms.author="sethm" />

# <a name="best-practices-for-insulating-applications-against-service-bus-outages-and-disasters"></a>Bewährte Methoden für Applikationen gegen Dienstbus Ausfall und Schäden isoliert

Wichtiger Applikationen müssen kontinuierlich, auch in Verbindung mit ungeplanten Ausfall oder Datenverluste ausgeführt werden. In diesem Thema werden Techniken, mit denen Sie Dienstbus Applikationen gegen eine mögliche Dienstausfall oder einer schützen können.

Ein Ausfall ist als der vorübergehenden Ausfall der Azure-Dienstbus definiert. Der Ausfall kann einige Komponenten des Dienstbus, beispielsweise einem messaging-Speicher oder sogar das gesamte Rechenzentrum auswirken. Nachdem das Problem behoben wurde, wird wieder Dienstbus verfügbar. Ein Ausfall bewirkt in der Regel keine Nachrichten oder andere Daten verloren gehen. Ein Beispiel für ein Komponentenfehler ist der Ausfall des einer bestimmten messaging Store. Ein Beispiel für ein Datacenter organisationsweite Ausfall ist ein Fehler Power Datencenter oder eine fehlerhafte Datacenter Netzwerk wechseln. Ein Ausfall kann auf ein paar Tage in wenigen Minuten dauern.

Einem Datenverlust wird als gehen unwiderruflich verloren einer Dienstbus Mengen Einheit oder Datacenter definiert. Datencenter möglicherweise oder möglicherweise nicht verfügbar erneut. In der Regel führt zu einem Datenverlust einige oder alle Nachrichten oder andere Daten verloren gehen. Beispiele für Datenverluste sind Fire, überfluten oder Erdbeben.

## <a name="current-architecture"></a>Aktuelle Architektur

Dienstbus verwendet mehrere messaging Stores zum Speichern von Nachrichten, die an Warteschlangen oder Themen gesendet werden. Eine nicht aufgeteilt Warteschlange oder ein Thema wird eine messaging Store zugewiesen. Wenn diese messaging Store nicht verfügbar ist, werden alle Vorgänge in der Warteschlange oder das Thema fehl.

Alle Dienstbus messaging Personen (Warteschlangen, Themen Relays) befinden sich in einem Dienstnamespace, in denen ein Datencenter zugeordnet ist. Dienstbus ermöglicht keine automatische Geo-Replikation der Daten, noch beinhaltet einen Namespace mehrere Rechenzentren umfassen.

## <a name="protecting-against-acs-outages"></a>Schutz vor ACS-Ausfall

Wenn Sie die ACS-Anmeldeinformationen verwenden und ACS nicht verfügbar ist mehr, können Clients nicht mehr Token abrufen. Clients, die ein Token gleichzeitig, die ACS-fällt enthalten aus können weiterhin Dienstbus verwenden, bis die Token abläuft. Die standardmäßige token Lebensdauer ist 3 Stunden.

Verwenden Sie zum Schutz vor ACS-Ausfall Token freigegeben Access Signatur (SAS) ein. In diesem Fall authentifiziert sich der Client direkt mit Dienstbus durch ein Self minted Token mit einem geheimen Schlüssel signieren. Anrufe an ACS sind nicht mehr erforderlich. Weitere Informationen zu SAS Token finden Sie unter [Dienstbus Authentifizierung][].

## <a name="protecting-queues-and-topics-against-messaging-store-failures"></a>Schützen von Warteschlangen und Themen für messaging Store-Fehler

Eine nicht aufgeteilt Warteschlange oder ein Thema wird eine messaging Store zugewiesen. Wenn diese messaging Store nicht verfügbar ist, werden alle Vorgänge in der Warteschlange oder das Thema fehl. Eine partitionierte Warteschlange, besteht andererseits, mehrere Fragmente aus. Jedes Fragment wird in einer anderen messaging gespeichert. Wenn eine Nachricht an eine partitionierte Warteschlange oder ein Thema gesendet wird, weist Dienstbus die Nachricht einer Fragment. Wenn der entsprechende messaging Store nicht verfügbar ist, gibt Dienstbus die Meldung zu einem anderen Fragment, falls möglich ein. Weitere Informationen zu den partitionierten Personen finden Sie unter [messaging Personen aufgeteilt][].

## <a name="protecting-against-datacenter-outages-or-disasters"></a>Schutz vor Datacenter Ausfall eines oder Datenverluste

Um ein Failover zwischen zwei Datencenter zulässig ist, können Sie einen Namespace des Dienstbus Dienst in jedem Datencenter erstellen. Angenommen, die Dienstbus Dienst Namespace **contosoPrimary.servicebus.windows.net** möglicherweise in den Vereinigten Staaten North/zentrale Region befinden, und **contosoSecondary.servicebus.windows.net** möglicherweise in der Region uns Süden/Mitte befinden. Wenn eine Entität messaging Dienstbus in einem Datacenter Ausfall Anwesenheit barrierefreien bleiben muss, können Sie diese Person in beiden Namespaces erstellen.

Weitere Informationen finden Sie im Abschnitt "Fehler bei der Dienstbus innerhalb einer Azure Datencenters" in [asynchronen messaging Mustern und hohen Verfügbarkeit][].

## <a name="protecting-relay-endpoints-against-datacenter-outages-or-disasters"></a>Schützen von Relay Endpunkte gegen Datacenter Ausfall eines oder Datenverluste

Geo-Replikation von Endpunkten Relay ermöglicht einen Dienst, der einen Endpunkt Relay in Anwesenheit Dienstbus Ausfall erreichbar macht. Um Geo-Replikation zu erreichen, muss der Dienst zwei Relay Endpunkten in verschiedenen Namespaces erstellen. Die Namespaces muss in verschiedenen Rechenzentren befinden, und die beiden Endpunkte müssen unterschiedliche Namen. Beispielsweise kann ein primärer Endpunkt unter **contosoPrimary.servicebus.windows.net/myPrimaryService**, erreicht werden, während der sekundäre Gegenstück unter **contosoSecondary.servicebus.windows.net/mySecondaryService**erreicht werden kann.

Der Dienst überwacht dann beide Endpunkte und ein Client Dienst über Endpunkten aufrufen kann. Eine Clientanwendung zufällig wählt einen der Relay als Endpunkt des primären und sendet die Anforderung an den Endpunkt des aktiven. Wenn der Vorgang mit einem Fehlercode fehlschlägt, zeigt diesen Fehler an, der Endpunkt Relay ist nicht verfügbar. Die Anwendung öffnet einen Kanal an den Endpunkt Sicherung und die Anfrage Datenbankquelle gesendet werden muss. Wechseln zu diesem Zeitpunkt der aktiv ist und die zusätzliche Endpunkte Rollen: die Clientanwendung betrachtet den alten aktiven Endpunkt den Endpunkt des neuen Sicherung und den alten Sicherung Endpunkt werden die neuen aktiven Endpunkt sein. Wenn beide Vorgänge Fail senden, die Rollen von den beiden Einheiten unverändert bleiben und ein Fehler zurückgegeben wird.

Im Beispiel [Geo-Replikation mit Service Bus weitergeleitet Nachrichten][] veranschaulicht Relays repliziert.

## <a name="protecting-queues-and-topics-against-datacenter-outages-or-disasters"></a>Schützen von Warteschlangen und Themen gegen Datacenter Ausfall eines oder Datenverluste

Zum Erreichen gegen Ausfall Datacenter Stabilität beim Verwenden von vermittelte messaging Dienstbus unterstützt zwei Verfahren: *aktiven* und *passiven* Replikation. Für jede Vorgehensweise Wenn eine bestimmte Warteschlange oder ein Thema in einem Ausfall Datacenter Anwesenheit barrierefreien bleiben muss können Sie sie in beiden Namespaces erstellen. Beide Personen können denselben Namen haben. Beispielsweise kann eine primäre Warteschlange unter **contosoPrimary.servicebus.windows.net/myQueue**, erreicht werden, während Gegenstück sekundäre unter **contosoSecondary.servicebus.windows.net/myQueue**erreicht werden kann.

Wenn die Anwendung keine permanente Absender-Empfänger-Kommunikation erforderlich sind, kann die Anwendung eine dauerhafte clientseitige Warteschlange Nachrichtenverlust zu vermeiden und den Absender aus eine vorübergehende Dienstbus Fehler vermieden werden, implementieren.

## <a name="active-replication"></a>Aktive Replikation

Aktive Replikation verwendet Elemente in beiden Namespaces für jeden Vorgang ein. Jeder Client, der eine Nachricht sendet sendet zwei Kopien der gleichen Nachricht an. Die erste Kopie wird gesendet, um die primäre Entität (z. B. **contosoPrimary.servicebus.windows.net/sales**), und die zweite Kopie der Nachricht wird gesendet, um die sekundäre Entität (z. B. **contosoSecondary.servicebus.windows.net/sales**).

Ein Client erhält Nachrichten aus beiden Warteschlangen an. Der Empfänger verarbeitet die erste Kopie einer Nachricht, und die zweite Instanz wird unterdrückt. Um doppelte Nachrichten zu unterdrücken, muss der Absender jeder Nachricht mit einem eindeutigen Bezeichner kategorisieren. Mit den gleichen Bezeichner müssen beide Kopien der Nachricht markiert sein. Die Eigenschaften [BrokeredMessage.MessageId][] oder [BrokeredMessage.Label][] oder eine benutzerdefinierte Eigenschaft können so kategorisieren Sie die Nachricht. Der Empfänger muss eine Liste von Nachrichten zu verwalten, die sie bereits erhalten hat.

Das [Geo-Replikation mit Service Bus vermittelte Nachrichten][] -Beispiel veranschaulicht aktiven Replikation Messaging Einheiten.

> [AZURE.NOTE] Der aktiven Replikationsansatz verdoppelt die Anzahl der Vorgänge, daher dieser Ansatz kann dazu führen, dass höhere Kosten.

## <a name="passive-replication"></a>Passive Replikation

Im Fall Fehlerstrukturanalyse-free verwendet passiven Replikation nur eine zwei messaging Entität. Ein Client sendet die Nachricht an die aktive Entität. Wenn der Vorgang in der aktiven Entität mit einem Fehlercode, der angibt, dass die Datencenters, die die aktive Entität hostet möglicherweise nicht verfügbar fehlschlägt, sendet der Client eine Kopie der Nachricht an die Sicherung Entität an. Wechseln zu diesem Zeitpunkt der aktiven und die Sicherung Personen Rollen: der sendende Client betrachtet die alte aktive Entität die neue Sicherung Entität sein, und die alte Sicherung Entität ist die neue aktiven Entität. Wenn beide Vorgänge Fail senden, die Rollen von den beiden Einheiten unverändert bleiben und ein Fehler zurückgegeben wird.

Ein Client erhält Nachrichten aus beiden Warteschlangen an. Da besteht die Möglichkeit, dass der Empfänger zwei Kopien der gleichen Nachricht empfängt, muss der Empfänger doppelte Nachrichten unterdrücken. Sie können auf die gleiche Weise Duplikate unterdrücken, beschriebenen aktive Replikations.

Passive Replikation ist im Allgemeinen effizienter als die aktive Replikation, da in den meisten Fällen nur ein Vorgang ausgeführt wird. Wartezeit, Durchsatz und monetäre Kosten sind identisch mit dem Szenario nicht repliziert.

Bei Verwendung von passiven Replikation in den folgenden Szenarien können werden verloren gegangen sind oder empfangenen Nachrichten zweimal:

-   **Nachricht verzögern oder Verlust**: setzen voraus, dass der Absender einer Nachricht m1 erfolgreich an die primäre Warteschlange gesendet, und klicken Sie dann die Warteschlange zur Verfügung steht, bevor der Empfänger m1 empfängt. Der Absender sendet eine m2 nachfolgende Nachricht an die sekundäre Warteschlange an. Wenn die primäre Warteschlange vorübergehend nicht verfügbar ist, empfängt der Empfänger m1 aus, nachdem die Warteschlange wieder zur Verfügung steht. Bei einem Ausfall kann der Empfänger m1 nie erhalten.

-   **Doppelte Empfang**: setzen voraus, dass der Absender eine Nachricht m an die primäre Warteschlange sendet. Dienstbus erfolgreich verarbeitet m jedoch schlägt fehl, um eine Antwort zu senden. Nachdem der Sendevorgang Timeout, sendet der Absender eine identische Kopie des m an die sekundäre Warteschlange an. Wenn der Empfänger die erste Kopie der m empfangen wird, bevor die primäre Warteschlange nicht mehr verfügbar ist, erhält der Empfänger beide Kopien der m ungefähr zur gleichen Zeit. Wenn der Empfänger nicht die erste Kopie der m empfangen wird, bevor die primäre Warteschlange nicht mehr verfügbar ist, der Empfänger empfängt anfangs nur die zweite Instanz von m, aber dann eine zweite Instanz von m empfangen werden, wenn die primäre Warteschlange verfügbar ist.

Das [Geo-Replikation mit Service Bus vermittelte Nachrichten][] -Beispiel veranschaulicht passiven Replikation Messaging Einheiten.

## <a name="next-steps"></a>Nächste Schritte

Um weitere Informationen zur Wiederherstellung finden Sie in folgenden Artikeln:

- [Geschäftskontinuität für SQL Azure-Datenbank][]
- [Technische Anleitung Azure Stabilität][]

  [Service Bus-Authentifizierung]: service-bus-authentication-and-authorization.md
  [Partitionierte messaging Einheiten]: service-bus-partitioning.md
  [Asynchrone messaging Muster und hohe Verfügbarkeit]: service-bus-async-messaging.md#failure-of-service-bus-within-an-azure-datacenter
  [Geo-Replikation mit Dienstbus weitergeleitet Nachrichten]: http://code.msdn.microsoft.com/Geo-replication-with-16dbfecd
  [BrokeredMessage.MessageId]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx
  [BrokeredMessage.Label]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx
  [Geo-Replikation mit Dienstbus vermittelte Nachrichten]: http://code.msdn.microsoft.com/Geo-replication-with-f5688664
  [Geschäftskontinuität für SQL Azure-Datenbank]: ../sql-database/sql-database-business-continuity.md
  [Technische Anleitung Azure Stabilität]: ../resiliency/resiliency-technical-guidance.md

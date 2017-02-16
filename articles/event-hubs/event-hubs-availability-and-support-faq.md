<properties 
    pageTitle="Ereignis Hubs häufig gestellte Fragen (FAQ) | Microsoft Azure"
    description="Ereignis Hubs häufig gestellte Fragen."
    services="event-hubs"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="event-hubs"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/01/2016"
    ms.author="sethm" />

# <a name="event-hubs-faq"></a>Ereignis Hubs häufig gestellte Fragen

Ereignis Hubs stellt umfangreiche Aufnahme, Beibehaltung und Verarbeitung von Ereignissen von Daten aus Datenquellen mit hohem Durchsatz und/oder Millionen von Geräten. Wenn mit Servicebuswarteschlangen und Themen gepaart, ermöglicht Ereignis Hubs beständige Befehl und Steuerung Bereitstellungen für Szenarien [Internet der Dinge (IoT)](https://azure.microsoft.com/services/iot-hub/) .

In diesem Artikel wird erläutert, Preisinformationen und finden Sie Antworten auf einige häufig gestellten Fragen zum Ereignis Hubs:

## <a name="pricing-information"></a>Preisinformationen

Vollständige Informationen zu Ereignis Hubs Preise finden Sie unter [Informationen zur Preisgestaltung des Ereignisses Hubs](https://azure.microsoft.com/pricing/details/event-hubs/).

## <a name="how-are-event-hubs-ingress-events-calculated"></a>Wie werden Ereignis Hubs eingehende Ereignisse berechnet?

Jedes Ereignis gesendet, um ein Ereignis Hub zählt als berechenbare Nachricht. Ein *Ereignis eingehende* wird als eine Dateneinheit definiert, der kleiner als oder gleich 64 KB sein. Ein beliebiges Ereignis, der kleiner als oder gleich 64 KB groß ist eine kostenpflichtig gilt. Wenn das Ereignis größer als 64 KB ist, wird die Anzahl der berechenbaren Ereignisse Größe Ereignis ein Vielfaches von 64 KB berechnet. Beispielsweise ein 8 KB-Ereignis an den Hub Ereignis gesendet wird als ein Ereignis in Rechnung gestellt, aber eine 96 KB Nachricht, die mit dem Ereignis Hub ist als der beiden folgenden Ereignisse in Rechnung gestellt.

Ereignisse, über ein Ereignis-Hub als verbraucht sowie Management-Vorgänge und steuern, wie etwa Kontrollpunkten Anrufe, werden als berechenbare eingehende Ereignisse nicht berücksichtigt, aber auf den Durchsatz Einheit Rabatt fällig.

## <a name="what-are-event-hubs-throughput-units"></a>Was sind Ereignis Hubs Durchsatz Einheiten?

Sie wählen explizit Ereignis Hubs Durchsatz Einheiten, entweder über den Azure-Portal oder Ereignis Hubs Ressource-Manager Vorlagen aus. Durchsatz Einheiten, die für alle Ereignis Hubs in ein Ereignis Hubs Namespace gelten, und ein Stück Durchsatz berechtigt Namespace zum folgenden Funktionen:

- Ruft oben auf 1 MB pro Sekunde eingehende Ereignisse (Ereignisse in ein Ereignis Hub gesendet), aber keine mehr als 1000 eingehende Ereignisse, Management Vorgänge oder Steuerelement API pro Sekunde.

- Bis zu 2 MB pro Sekunde Ausgang Ereignisse (Ereignisse, über ein Ereignis Hub verbraucht).

- Bis zu 84 GB Ereignis Speicher (ausreichend für die standardmäßige Aufbewahrungszeitraum 24-Stunden-Format).

Ereignis Hubs Durchsatz Einheiten werden stündlich, belastet basierend auf die maximale Anzahl der Einheiten, die während der angegebenen Stunde ausgewählt.

## <a name="how-are-event-hubs-throughput-unit-limits-enforced"></a>Wie werden Ereignis Hubs Durchsatz Einheit Grenzwerte erzwungen?

Wenn der gesamte eingehende Durchsatz oder die gesamte eingehende Ereignis Rate über alle Ereignis Hubs in einem Namespace die aggregierte Durchsatz Einheit softwarebegrenzungen überschreitet, Absender gedrosselt werden sollen, und erhalten Fehlermeldungen, die angibt, dass das eingehende Kontingent überschritten wurde.

Überschreitet die Summe Ausgang Durchsatz oder die gesamte Ereignis Ausgang Rate über alle Ereignis Hubs in einem Namespace die aggregierte Durchsatz Einheit softwarebegrenzungen, Empfänger gedrosselt werden und erhalten Fehlermeldungen, die angibt, dass das Ausgang Kontingent überschritten wird. Eingehende und Ausgang Kontingente werden separat erzwungen, damit keine Absender kann dazu führen, dass verlangsamen Ereignis Verbrauch, noch kann ein Empfänger verhindern, dass Ereignisse in ein Ereignis Hub gesendet werden.

Beachten Sie, dass die Durchsatz Einheit Auswahl unabhängig von der Anzahl von Partitionen Hubs Ereignis ist. Jede Partition einen maximalen Durchsatz von 1 MB pro zweiten eingehende (mit maximal 1000 Ereignisse pro Sekunde) und 2 MB pro zweiten Ausgang anbietet, gibt aber es keine feste Gebühr für die Partitionen selbst. Die Gebühr ist für die Einheiten aggregierte Durchsatz auf alle Ereignis Hubs in ein Ereignis Hubs Namespace. Mit diesem Muster können Sie genügend Partitionen unterstützt die erwartete maximale Belastung für ihre Systeme, ohne eine Durchsatz Einheit Gebühren anfallen, bis die Ereignis Auslastung des Systems tatsächlich höhere Durchsatz Zahlen erfordert, und tragen ohne Ändern der Struktur und Architektur Ihrer Systeme als die Auslastung des Systems erstellen.

## <a name="is-there-a-limit-on-the-number-of-throughput-units-that-can-be-selected"></a>Gibt es eine Beschränkung auf die Anzahl der Einheiten Durchsatz, die ausgewählt werden können?

Es wird ein Kontingent von 20 Durchsatz Einheiten pro Namespace aus. Sie können ein größeres Kontingent Durchsatz Einheiten Ablage Support-Ticket anfordern. Die Beschränkung 20 Durchsatz Einheit sind Pakete in 20 und 100 Durchsatz Einheiten verfügbar. Beachten Sie, dass die Möglichkeit, die Anzahl der Einheiten Durchsatz zu ändern, ohne eine Support-Ticket Ablage mit mehr als 20 Durchsatz Einheiten entfernt werden.

## <a name="is-there-a-charge-for-retaining-event-hubs-events-for-more-than-24-hours"></a>Gibt es eine Gebühr für Aufbewahrung Ereignis Hubs Ereignisse für mehr als 24 Stunden?

Die Ereignis Hubs Standard Ebene beinhaltet Nachricht Aufbewahrung Perioden mehr als 24 Stunden, bis zu 30 Tage. Überschreitet die Größe der Gesamtzahl der gespeicherten Ereignisse den Rabatt Speicher für die Anzahl der ausgewählten Durchsatz Einheiten (84 GB pro Einheit Durchsatz), wird die Größe, die den Rabatt überschreitet die veröffentlichte Azure BLOB-Speicher Rate berechnet. Der Speicher Rabatt pro Einheit Durchsatz umfasst alle Speicherkosten für die Aufbewahrungszeiträume von 24 Stunden (die Standardeinstellung), auch wenn der Maßeinheit Durchsatz zu den maximalen eingehende Rabatt verbraucht ist.

## <a name="what-is-the-maximum-retention-period"></a>Was ist die maximale Aufbewahrungszeitraum?

Ereignis Hubs Standard Ebene unterstützt derzeit einen maximale Aufbewahrungszeitraum von 7 Tagen. Beachten Sie, dass Ereignis Hubs nicht als permanente Datenspeicher vorgesehen sind. Aufbewahrungsrichtlinien Zeiträume größer ist als 24 Stunden für Szenarios vorgesehen sind, in denen es einfach wiedergeben ein Streams von Ereignissen in denselben Systemen ist; Wenn Sie beispielsweise Schulen oder ein neuen Computer Learning-Modell auf vorhandenen Daten zu überprüfen.

## <a name="how-is-the-event-hubs-storage-size-calculated-and-charged"></a>Wie wird die Größe des Ereignisses Hubs Speichers berechnet und belastet?

Die Gesamtgröße aller gespeicherten Ereignisse, einschließlich internen Verwaltungsaufwand Ereignis Kopf- oder auf dem Datenträger Speicherstrukturen in allen Ereignis Hubs wird im Verlauf des Tages gemessen. Am Ende des Tages wird die Größe des Speichers Höchstwert berechnet. Der täglichen Speicher Rabatt wird basierend auf die minimale Anzahl von Durchsatz Einheiten berechnet, die während des Tages ausgewählt wurden (jede Einheit Durchsatz bietet eine Berichtigung 84 GB). Überschreitet die Gesamtgröße den berechneten tägliche Speicher Rabatt, ist der übermäßige Speicher in Rechnung gestellt Azure BLOB-Speicher Sätzen (Rate **Lokal redundante Speicherung** ) verwenden.

## <a name="can-i-use-a-single-amqp-connection-to-send-and-receive-from-event-hubs-and-service-bus-queuestopics"></a>Kann ich mithilfe eine einzige AMQP-Verbindung senden und Empfangen von Ereignis Hubs und Dienstbus Warteschlangen/Themen?

Ja, solange die Ereignis Hubs, Warteschlangen und Themen im selben Namespace befinden. Daher können Sie bidirektionaler, vermittelten Verbindung zu viele Geräten mit Subsecond Wartezeiten, kostengünstiger und hochgradig skalierbare Weise implementieren.

## <a name="do-brokered-connection-charges-apply-to-event-hubs"></a>Gelten vermittelten Verbindungsgebühren Ereignis Hubs?

Wenden Sie Verbindungsgebühren für Absender nur, wenn das Protokoll AMQP verwendet wird. Es gibt keine Verbindungsgebühren für das Senden von Ereignissen über HTTP, unabhängig von der Anzahl der Betriebssysteme oder Geräte senden aus. Wenn Sie beabsichtigen, AMQP (z. B. effizienter Ereignis streaming erzielen oder bidirektionale Kommunikation in IoT Befehl und Steuerung Szenarien aktivieren) verwenden, finden Sie auf der Seite [Dienstbus Preisinformationen](https://azure.microsoft.com/pricing/details/service-bus/) Informationen Worin vermittelten Verbindung besteht, und wie sie getaktete sind.

## <a name="what-is-the-difference-between-event-hubs-basic-and-standard-tiers"></a>Was ist der Unterschied zwischen Ereignis Hubs Standard- und Standard Ebenen?

Ereignis Hubs Standard Ebene enthält Features, die über das im Ereignis Hubs grundlegende, und einige vergleichbare Systeme zur Verfügung. Diese Features umfassen Aufbewahrungszeiten von mehr als 24 Stunden und die Möglichkeit, eine einzelne AMQP Verbindung Befehle an einer großen Anzahl von Geräten mit Subsecond Wartezeiten senden sowie werden diese Geräte in Ereignis Hubs senden verwenden. Finden Sie unter der Liste der Features die [Informationen zur Preisgestaltung der Hubs Ereignis](https://azure.microsoft.com/pricing/details/event-hubs/).

## <a name="geographic-availability"></a>Geografische Verfügbarkeit

Ereignis Hubs steht in den folgenden Regionen zur Verfügung:

|Geo|Regionen|
|---|---|
|USA|Zentrale US, ostasiatischen US, ostasiatischen US 2, Süd zentralen US, Westen US|
|Europa|North Europe, Westen Europa|
|Asiatisch-pazifischen Raum|Ostasien, oder Asien|
|Japan|Japan Osten, Westen Japan|
|Brazilien|Brasilien Süd|
|Australien|Australien Osten und Australien oder|

## <a name="support-and-sla"></a>Support und Vereinbarung zum SERVICELEVEL

Technischen Support für Ereignis Hubs ist über die [Community-Foren](https://social.msdn.microsoft.com/forums/azure/home)verfügbar. Unterstützung für abrechnungs- und Abonnementsupport Management ist kostenlos zur Verfügung gestellt.

Weitere Informationen zu unseren Vereinbarung zum SERVICELEVEL finden Sie unter der Seite [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/) .

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Ereignis Hubs finden Sie in den folgenden Artikeln:

- [Ereignis Hubs Übersicht][].
- Eine vollständige [Beispiel-Anwendung, die Ereignis Hubs verwendet][].

[Ereignis Hubs (Übersicht)]: event-hubs-overview.md
[Beispiel-Anwendung, Ereignis Hubs verwendet]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097

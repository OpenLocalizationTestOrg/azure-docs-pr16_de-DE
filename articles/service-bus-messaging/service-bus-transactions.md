<properties 
    pageTitle="Dienstbus Transaktionen | Microsoft Azure" 
    description="Übersicht über Azure-Dienstbus atomaren Transaktionen und per senden" 
    services="service-bus" 
    documentationCenter=".net" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na" 
    ms.date="10/04/2016"
    ms.author="clemensv;sethm"/>

# <a name="overview-of-service-bus-transaction-processing"></a>Übersicht über die Verarbeitung von Transaktionen Dienstbus

In diesem Artikel werden die Transaktionsfunktionen Azure Service. Viele der Diskussion wird durch die [Atomare Transaktionen mit Dienstbus Beispiel](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/AtomicTransactions)veranschaulicht. In diesem Artikel beträgt einen Überblick über die Verarbeitung von Transaktionen und dem *Senden per* Feature in Dienstbus, während die Stichprobe atomaren Transaktionen umfassendere und komplexere im Bereich befindet.

## <a name="transactions-in-service-bus"></a>Transaktionen in Dienstbus

Eine [Transaktion](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/AtomicTransactions#what-are-transactions) gruppiert zwei oder mehr Vorgänge in einem *Gültigkeitsbereich der Ausführung*zusammen. Art muss eine solche Transaktion sicherstellen, dass alle Vorgänge, die zu einer bestimmten Gruppe von JOIN-Operationen gehören entweder funktioniert oder es fehlschlägt. In diesem Zusammenhang fungieren, Transaktionen als eine Einheit, die häufig als *Unteilbarkeit*bezeichnet wird. 

Dienstbus ist eine Bank Transaktionen Nachricht und stellt sicher Transaktionsintegrität für alle internen Operationen mit dem zugehörigen Nachricht Stores. Alle Übertragung von Nachrichten innerhalb Dienstbus, wie etwa das Verschieben von Nachrichten in einer [unzustellbare](service-bus-dead-letter-queues.md) oder [für die automatische Weiterleitung](service-bus-auto-forwarding.md) von Nachrichten zwischen Elementen, sind Transaktionen. Als solche Dienstbus eine Nachricht akzeptiert, es bereits gespeichert und mit der Bezeichnung und einer laufenden Nummer. Von dann an eine beliebige Nachricht-Übertragung innerhalb Dienstbus sind aufeinander abgestimmte Vorgänge verschiedenen Personen und führt keine zum Verlust (Quelle erfolgreich ist und Ziel schlägt fehl) oder Duplikate (Quelle schlägt fehl, und Ziel war erfolgreich) der Nachricht.

Dienstbus unterstützt Gruppierung Operationen für eine einzelne messaging Entität (Warteschlange, Thema, Abonnement) innerhalb des Gültigkeitsbereichs einer Transaktion. Beispielsweise können Sie mehrere Nachrichten an eine Warteschlange von innerhalb eines Transaktionsbereichs senden, und die Nachrichten werden nur der Warteschlange Log sich verpflichtet, wenn die Transaktion erfolgreich abgeschlossen ist.

## <a name="operations-within-a-transaction-scope"></a>Vorgänge innerhalb eines Transaktionsbereichs 

Die Vorgänge, die innerhalb eines Transaktionsbereichs erfolgen können sind wie folgt aus:

- ** [QueueClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx), [NachrichtSender](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx), [TopicClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx)**: senden, SendAsync, SendBatch, SendBatchAsync 

- **[BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx)**: fertig sind, CompleteAsync, Abandon, AbandonAsync, für unzustellbare Nachrichten, DeadletterAsync, zurückstellen, DeferAsync, RenewLock, RenewLockAsync 

Empfangen Vorgänge sind nicht enthalten, da angenommen wird, dass die Anwendung Nachrichten abgerufenen mit dem Modus [ReceiveMode.PeekLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) innerhalb erhalten Sie einige Schleife oder mit einer [OnMessage](https://msdn.microsoft.com/library/azure/dn369601.aspx) Rückruf und nur dann öffnet Transaktionsbereich für die Verarbeitung der Nachricht.

Die Anordnung der Nachricht (vollständig, Abandon, unzustellbare, Zurückstellen) tritt innerhalb des Gültigkeitsbereichs von, und abhängige auf Gesamtergebnis der Transaktion.

## <a name="transfers-and-send-via"></a>Übertragen und "über senden"

Klicken Sie zum Aktivieren von Daten aus einer Warteschlange auf einen Prozessor, und klicken Sie dann an eine andere Warteschlange Transaktionen Übergabe unterstützt Dienstbus *übertragen*. In einem Vorgang durchstellen ein Absenders sendet zuerst eine Nachricht an eine Warteschlange"Transfer" und die Warteschlange Transfer verschiebt die Nachricht sofort an das gewünschte Zielwarteschlange mithilfe derselben robuste durchstellen Implementierung, der von der automatischen Weiterleiten Videofunktionen genutzt. Die Nachricht wird nie verpflichtet die Schlange Log so, dass sie für die Schlange Nutzer angezeigt wird.

Mit dieser Funktion Transaktionen wird sichtbar, wenn die Schlange selbst die Quelle für eingehende Nachrichten des Absenders ist. Kurzum, Dienstbus können die Nachricht an die Zielwarteschlange übertragen "via" die Schlange, bei der Durchführung einer vollständigen (oder zurückstellen, oder unzustellbare) Vorgang für die Eingabe Nachricht, alle in einem atomaren Vorgang. 

### <a name="see-it-in-code"></a>Diese in Code angezeigt werden

Um die Übertragung eingerichtet haben, erstellen Sie den Absender einer Nachricht, der die Zielwarteschlange über die Schlange ausgerichtet. Haben Sie einen Empfänger, der Nachrichten aus dieser dieselbe Warteschlange abruft. Beispiel:

```
var sender = this.messagingFactory.CreateMessageSender(destinationQueue, myQueueName);
var receiver = this.messagingFactory.CreateMessageReceiver(myQueueName);
```

Klicken Sie dann eine einfache Transaktion verwendet diese Elemente, wie im folgenden Beispiel gezeigt:

```
var msg = receiver.Receive();

using (scope = new TransactionScope())
{
    // Do whatever work is required 

    var newmsg = ... // package the result 

    msg.Complete(); // mark the message as done
    sender.Send(newmsg); // forward the result

    scope.Complete(); // declare the transaction done
} 
```

## <a name="next-steps"></a>Nächste Schritte

Finden Sie weitere Informationen zu Servicebuswarteschlangen die folgenden Artikeln:

- [Verketten Dienstbus Einheiten mit automatische Weiterleitung](service-bus-auto-forwarding.md)
- [Beispiel für die automatische-weiterleiten](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/AutoForward)
- [Atomare Transaktionen mit Dienstbus Stichprobe](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/AtomicTransactions)
- [Azure Warteschlangen und Dienstbus Warteschlangen im Vergleich](service-bus-azure-and-service-bus-queues-compared-contrasted.md)
- [So verwenden Sie Servicebuswarteschlangen](service-bus-dotnet-get-started-with-queues.md)
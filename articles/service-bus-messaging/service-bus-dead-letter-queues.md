<properties 
    pageTitle="Unzustellbare Servicebuswarteschlangen | Microsoft Azure" 
    description="Übersicht über Azure unzustellbare Servicebuswarteschlangen" 
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
    ms.date="10/03/2016"
    ms.author="clemensv;sethm"/>

# <a name="overview-of-service-bus-dead-letter-queues"></a>Übersicht über Servicebuswarteschlangen unzustellbare

Servicebuswarteschlangen und Thema Abonnements bieten eine sekundäre untergeordnete Warteschlange genannte *unzustellbare* (DLQ). Die unzustellbare Warteschlange muss nicht explizit erstellt werden und nicht gelöscht oder anderweitig verwaltet unabhängig von der Hauptseite Entität.

Der Zweck der Warteschlange unzustellbare ist halten an alle Empfänger übermittelt werden kann, oder einfach Nachrichten, die nicht bearbeitet werden können. Nachrichten können dann aus der DLQ entfernt und geprüft. Eine Anwendung bisweilen mit Hilfe eines Operators, Probleme zu beheben und übermitteln Sie die Nachricht erneut, melden Sie sich die Fakultät, die einen Fehler enthielt und/oder Maßnahme ausführen. 

Aus einer Perspektive API und das Protokoll ähnelt der DLQ hauptsächlich an eine beliebige andere Warteschlange, mit dem Unterschied, dass Nachrichten nur können, können Sie über die unzustellbare Bewegung der übergeordneten Entität gesendet werden. Darüber hinaus Time-to-live nicht beobachtet wird, und Sie können Sie eine Nachricht von einer DLQ unzustellbare. Die unzustellbare Warteschlange unterstützt vollständig anheften-Sperren Übermittlungs- und Vorgänge Transaktionen.

Beachten Sie, dass es keine automatische Aufräumen von der DLQ. Nachrichten verbleiben in der DLQ, bis Sie explizit rufen sie aus der DLQ, und rufen Sie [Complete()](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.completeasync.aspx) die unzustellbare Nachricht.

## <a name="moving-messages-to-the-dlq"></a>Verschieben von Nachrichten in der DLQ

Es gibt mehrere Aktivitäten in Dienstbus, die dazu führen, dass Nachrichten in der DLQ von innerhalb der messaging-Engine selbst abgelegt zu erhalten. Eine Anwendung kann auch explizit Nachrichten an die DLQ ablegen. 

Während die Nachricht von der Bank verschoben wurde, werden zwei Eigenschaften für die Nachricht hinzugefügt, wie der Makler seine interne Version der Methode [für unzustellbare Nachrichten](https://msdn.microsoft.com/library/azure/hh291941.aspx) klicken Sie auf die Nachricht ruft: `DeadLetterReason` und `DeadLetterErrorDescription`.

Definieren von Applications können eigene Codes für die `DeadLetterReason` Eigenschaft, aber das System legt die folgenden Werte.

| Bedingung                                                                                                                             | DeadLetterReason            | DeadLetterErrorDescription                                                       |
|---------------------------------------------------------------------------------------------------------------------------------------|-----------------------------|----------------------------------------------------------------------------------|
| Immer                                                                                                                                | HeaderSizeExceeded          | Das Größenkontingent für diesen Stream wurde überschritten.                                |
| ! TopicDescription.<br />EnableFilteringMessagesBeforePublishing und SubscriptionDescription.<br />EnableDeadLetteringOnFilterEvaluationExceptions | Ausnahme. Systemeigene GetType(). Namen    | Ausnahme. Nachricht                                                                |
| EnableDeadLetteringOnMessageExpiration                                                                                                | TTLExpiredException         | Die Nachricht ist abgelaufen und wurde leer mit einem Buchstaben gekennzeichneten.                                       |
| SubscriptionDescription.RequiresSession                                                                                               | Id für eine Sitzung ist null.         | Sitzung aktiviert Entität ist eine Nachricht, dessen Sitzungsbezeichner null ist, nicht möglich. |
| ! Warteschlange für unzustellbare                                                                                                                    | MaxTransferHopCountExceeded | NULL-Werten                                                                             |
| Explizite leer Beschriftung Anwendung                                                                                                   | Durch die Anwendung angegeben    | Durch die Anwendung angegeben                                                         |

## <a name="exceeding-maxdeliverycount"></a>MaxDeliveryCount überschritten

Warteschlangen und Abonnements haben jeweils eine [QueueDescription.MaxDeliveryCount](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.maxdeliverycount.aspx) und [SubscriptionDescription.MaxDeliveryCount](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptiondescription.maxdeliverycount.aspx) Eigenschaft. Der Standardwert ist 10. Wenn eine Nachricht unter Schloss ([ReceiveMode.PeekLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx)) übermittelt wurde, aber entweder explizit abgebrochen wurde oder die Sperre ist abgelaufen, wird die Meldung [BrokeredMessage.DeliveryCount](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.deliverycount.aspx) erhöht. Wenn [DeliveryCount](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.deliverycount.aspx) [MaxDeliveryCount](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.maxdeliverycount.aspx)überschreitet, wird die Nachricht in der DLQ verschoben angeben der `MaxDeliveryCountExceeded` Grund Code.

Dieses Verhalten kann nicht deaktiviert werden, aber Sie können [MaxDeliveryCount](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.maxdeliverycount.aspx) festlegen, um eine große Anzahl.

## <a name="exceeding-timetolive"></a>TimeToLive überschreiten

Wenn die Eigenschaft [QueueDescription.EnableDeadLetteringOnMessageExpiration](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enabledeadletteringonmessageexpiration.aspx) oder [SubscriptionDescription.EnableDeadLetteringOnMessageExpiration](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptiondescription.enabledeadletteringonmessageexpiration.aspx) auf **true** festgelegt ist (die Standardeinstellung ist " **false**"), alle ablaufende Nachrichten in das DLQ verschoben werden angeben der `TTLExpiredException` Grund Code.

Beachten Sie, dass abgelaufene Nachrichten werden nur endgültig gelöscht und daher in der DLQ verschoben, wenn mindestens eine aktive Empfänger herausziehen im Hauptfenster Warteschlange oder Abonnement vorhanden ist; Dieses Verhalten ist beabsichtigt.

## <a name="errors-while-processing-subscription-rules"></a>Fehler beim Verarbeiten von Abonnementregeln

Wenn die [SubscriptionDescription.EnableDeadLetteringOnFilterEvaluationExceptions](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptiondescription.enabledeadletteringonfilterevaluationexceptions.aspx) -Eigenschaft für ein Abonnement aktiviert ist, werden alle aufgetretenen während eines Abonnements SQL-Regel ausgeführt wird in der DLQ sowie die anstößige Nachricht erfasst.

## <a name="application-level-dead-lettering"></a>Anwendung Ebene Dead-Textformats

Zusätzlich zu den Features System bereitgestellten Dead-Textformats können Applications der DLQ Sie explizit inakzeptabel Nachrichten ablehnen. Dies kann einschließen, Nachrichten, die aufgrund einer beliebigen Sortieren des Problems mit dem nicht ordnungsgemäß verarbeitet werden können, halten Sie den fehlerhaften Fracht, oder Nachrichten, die Authentifizierung ein Fehler auftreten, wenn einige Schema Sicherheit auf Benutzerebene-Nachricht verwendet wird.

## <a name="example"></a>Beispiel

Im folgenden Codeausschnitt wird als Nachrichtenempfänger erstellt. In der Empfangsschleife für das Hauptfenster Warteschlange ermittelt den Code die Nachricht mit [Receive(TimeSpan.Zero)](https://msdn.microsoft.com/library/azure/dn130350.aspx), der aufgefordert wird der Makler, um sofort alle Nachrichten verfügbarer zurückzukehren, oder mit kein Ergebnis zurück. Wenn der Code eine Nachricht eingeht, es sofort verwirft sie, welche Schritten die `DeliveryCount`. Nachdem das System die Nachricht in der DLQ verschiebt, die Hauptfenster Warteschlange leer ist, und die Schleife beendet wird, wie [ReceiveAsync](https://msdn.microsoft.com/library/azure/dn130350.aspx) **null**zurückgibt.

```
var receiver = await receiverFactory.CreateMessageReceiverAsync(queueName, ReceiveMode.PeekLock);
while(true)
{
    var msg = await receiver.ReceiveAsync(TimeSpan.Zero);
    if (msg != null)
    {
        Console.WriteLine("Picked up message; DeliveryCount {0}", msg.DeliveryCount);
        await msg.AbandonAsync();
    }
    else
    {
        break;
    }
}
```

## <a name="next-steps"></a>Nächste Schritte

Finden Sie weitere Informationen zu Servicebuswarteschlangen die folgenden Artikeln:

- [Erste Schritte mit Servicebuswarteschlangen](service-bus-dotnet-get-started-with-queues.md)
- [Azure Warteschlangen und Dienstbus Warteschlangen im Vergleich](service-bus-azure-and-service-bus-queues-compared-contrasted.md)

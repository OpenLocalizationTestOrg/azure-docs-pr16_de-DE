<properties 
    pageTitle="Warteschlangen und Themen aufgeteilt | Microsoft Azure"
    description="Beschreibt, wie Servicebuswarteschlangen und Themen mithilfe von mehreren Nachrichtenmakler unterteilen."
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
    ms.date="09/02/2016"
    ms.author="sethm;hillaryc" />

# <a name="partitioned-queues-and-topics"></a>Partitionierte Warteschlangen und Themen

Azure Dienstbus beschäftigt mehrere Nachrichtenmakler zum Verarbeiten von Nachrichten und mehrere messaging Stores, um Nachrichten zu speichern. Eine konventionelle Warteschlange oder ein Thema Behandlung von einer einzelnen Nachricht Bank und in einem per Store gespeichert. Dienstbus ermöglicht auch Warteschlangen oder Themen zur über mehrere Nachrichtenmakler und messaging Stores aufgeteilt werden. Dies bedeutet, dass der Gesamtdurchsatz einer partitionierten Warteschlange oder das Thema nicht mehr durch die Leistung von einer einzelnen Nachricht Bank oder einem messaging Store begrenzt ist. Darüber hinaus gibt eine temporäre Ausfall eines messaging Store keine partitionierte Warteschlange oder das Thema nicht verfügbar wieder. Partitionierte Warteschlangen und Themen können alle erweiterte Dienstbus-Funktionen, wie z. B. Unterstützung für Transaktionen und Sitzungen enthalten.

Weitere Details in Bezug auf die Dienstbus finden Sie im [Dienst Busarchitektur][] Thema.

## <a name="how-it-works"></a>So funktioniert es

Jede partitionierte Warteschlange oder ein Thema besteht aus mehreren Fragmenten. Jedes Fragment ist in einem anderen messaging-Speicher gespeichert und bei einer anderen Nachricht Bank behandelt. Wenn eine Nachricht an eine partitionierte Warteschlange oder ein Thema gesendet wird, weist Dienstbus die Nachricht einer Fragment. Die Auswahl erfolgt zufällig Dienstbus oder mithilfe eines Partitionsschlüssel, die der Absender angeben können.

Wenn ein Client eine Nachricht von einer partitionierten Warteschlange oder aus einem Abonnement des partitionierten Themas Dienstbus Abfragen alle Fragmente für Nachrichten erhalten möchte, gibt dann die erste Nachricht, die an den Empfänger aus einem beliebigen messaging Speicher ermittelt wird. Dienstbus Caches, die anderen Nachrichten und fügt Sie wieder bei Erhalt des zusätzlichen, erhalten Besprechungsanfragen. Ein empfangen Client ist keine der Aufteilung bekannt; das Verhalten Client zugänglichen einer partitionierten Warteschlange oder ein Thema (z. B. lesen, abgeschlossen ist, zurückstellen, für unzustellbare Nachrichten, vorab) mit dem Verhalten einer regulären Entität identisch ist.

Es ist keine zusätzliche Kosten, wenn eine Nachricht zu senden oder Empfangen einer Nachricht von einer partitionierten Warteschlange oder ein Thema aus.

## <a name="enable-partitioning"></a>Teilung aktivieren

Zum Verwenden von partitionierten Warteschlangen und Themen mit Azure-Dienstbus Azure SDK, Version 2.2 oder höher verwenden, oder geben Sie `api-version=2013-10` in Ihrer HTTP anfordert.

Sie können Servicebuswarteschlangen und Themen in 1, 2, 3, 4 und 5 GB Größen erstellen (die Standardeinstellung ist 1 GB). Mit Verteilung aktiviert, erstellt Dienstbus 16 Partitionen für jedes GB, die Sie angeben. Wenn Sie eine Warteschlange, die Größe 5 GB ist erstellen, mit 16 Partitionen die maximale Größe wird (5 \* 16) = 80 GB. Sie können die maximale Größe von partitionierten Warteschlange oder Thema anzeigen, indem Sie den Eintrag im [Portal Azure][]betrachtet.

Es gibt mehrere Methoden zum Erstellen einer partitionierten Warteschlange oder ein Thema ein. Wenn Sie die Warteschlange oder ein Thema aus Ihrer Anwendung erstellen, können Sie die Verteilung für die Warteschlange oder das Thema, indem die Eigenschaft [QueueDescription.EnablePartitioning][] oder [TopicDescription.EnablePartitioning][] Hilfethemas **Wahr**aktivieren. Diese Eigenschaften müssen zum Zeitpunkt die Warteschlange oder Thema wird erstellt. Es ist nicht möglich, diese Eigenschaften auf einem vorhandenen Warteschlange oder ein Thema zu ändern. Beispiel:

```
// Create partitioned topic
NamespaceManager ns = NamespaceManager.CreateFromConnectionString(myConnectionString);
TopicDescription td = new TopicDescription(TopicName);
td.EnablePartitioning = true;
ns.CreateTopic(td);
```

Alternativ können Sie eine partitionierte Warteschlange oder ein Thema in Visual Studio oder im [Azure-Portal][]erstellen. Wenn Sie eine neue Warteschlange oder ein Thema im Portal erstellen, legen Sie die Option **Aktivieren Partitionierung** , in das Blade **Allgemeine Einstellungen** der Warteschlange oder Thema **Einstellungen** -Fenster auf **true**. Klicken Sie in Visual Studio auf das Kontrollkästchen **Partitionierung aktivieren Sie** im Dialogfeld **Neue Warteschlange** oder **Neue Thema** .

## <a name="use-of-partition-keys"></a>Verwenden von Tastenkombinationen partition

Wenn eine Nachricht in der Warteschlange befindliche in eine partitionierte Warteschlange oder ein Thema ist, prüft, ob der Partitionsschlüssel Dienstbus. Wenn sie eine findet, markiert das Fragment basierend auf diesem Schlüssel. Wenn eine Partitionsschlüssel nicht gefunden wird, markiert das ausgehend von eines internen Algorithmus Fragment.

### <a name="using-a-partition-key"></a>Verwenden einer Partitionsschlüssel

Einige Szenarien, z. B. Sitzungen oder Transaktionen, erfordern Nachrichten in einem bestimmten Fragment gespeichert werden soll. Alle folgenden Szenarien erfordern die Verwendung von Partitionsschlüssel aus. Alle Nachrichten, die in der gleichen Partitionsschlüssel verwendet werden die gleichen Fragment zugewiesen. Wenn das Fragment vorübergehend nicht verfügbar ist, gibt Dienstbus einen Fehler zurück.

Je nach dem Szenario werden verschiedene Message Eigenschaften als Partitionsschlüssel verwendet:

**SessionId**: Wenn eine Nachricht [BrokeredMessage.SessionId][] -Eigenschaft festgelegt wurde, und klicken Sie dann Dienstbus wird diese Eigenschaft, als die Partitionsschlüssel verwendet. Auf diese Weise werden alle Nachrichten, die in der gleichen Sitzung gehören von der gleichen Nachricht Bank behandelt. Dadurch Dienstbus zu Nachricht sowie die Konsistenz der Sitzung Staaten Sortierung sichergestellt ist.

**PartitionKey**: Wenn eine Nachricht aufweist, die Eigenschaft [BrokeredMessage.PartitionKey][] , aber nicht die [BrokeredMessage.SessionId][] Eigenschaft, Dienstbus verwendet die Eigenschaft [PartitionKey][] als der Partitionsschlüssel. Wenn die Meldung sowohl die [SessionId][] und die [PartitionKey][] Dokumenteigenschaften festgelegt ist, müssen beide Eigenschaften identisch sein. Wenn die Eigenschaft [PartitionKey][] auf einen anderen Wert als die Eigenschaft ["SessionId"][] festgelegt ist, gibt Dienstbus eine Ausnahme **InvalidOperationException** aus. Die Eigenschaft [PartitionKey][] sollte verwendet werden, wenn der Absender nicht Sitzung bewusst Transaktionen Nachrichten sendet. Der Partitionsschlüssel wird sichergestellt, dass alle Nachrichten, die innerhalb einer Transaktion gesendet werden, die von der gleichen messaging Bank behandelt werden.

**Nachrichten-ID**: die Warteschlange oder das Thema die [QueueDescription.RequiresDuplicateDetection][] -Eigenschaft auf **true** festlegen weist und die [BrokeredMessage.SessionId][] oder [BrokeredMessage.PartitionKey][] Eigenschaften werden nicht festgelegt, und die [BrokeredMessage.MessageId][] Eigenschaft dient als der Partitionsschlüssel. (Beachten Sie, dass die Bibliotheken Microsoft .NET und AMQP automatisch eine Nachricht-ID zuweisen, wenn die sendende Anwendung nicht der Fall ist.) In diesem Fall werden alle Kopien der gleichen Nachricht von der gleichen Nachricht Bank behandelt. Dadurch wird die Dienstbus zu erkennen und Beseitigen von doppelten Nachrichten. Wenn die Eigenschaft [QueueDescription.RequiresDuplicateDetection][] nicht auf **true**festgelegt ist, wird Dienstbus die [Nachrichten-ID][] -Eigenschaft nicht als Partitionsschlüssel berücksichtigt.

### <a name="not-using-a-partition-key"></a>Partitionsschlüssel verwenden nicht

In das Fehlen eines Partitionsschlüssel verteilt Dienstbus Nachrichten in einem alle Fragmente des partitionierten Warteschlange oder Thema Roundrobin. Wenn das ausgewählte Fragment nicht verfügbar ist, weist Dienstbus die Nachricht zu einem anderen Fragment an. Auf diese Weise erfolgreich der Sendevorgang trotz der vorübergehenden Ausfall eines messaging Store.

Dienstbus verleihen müssen genügend Zeit in Warteschlange die Nachricht in einem anderen Fragment, von dem Client, der die Nachricht sendet angegebene [MessagingFactorySettings.OperationTimeout][] Wert größer als 15 Sekunden. Es wird empfohlen, dass Sie die [OperationTimeout][] -Eigenschaft auf den Standardwert von 60 Sekunden festlegen.

Beachten Sie, dass eine Partitionsschlüssel eine Nachricht zu einem bestimmten Fragment "fixiert". Wenn der per Store, der diesem Fragment enthält nicht verfügbar ist, gibt Dienstbus einen Fehler zurück. Wenn kein Partitionsschlüssel Dienstbus können einer anderen Fragment auswählen, und der Vorgang erfolgreich ist. Daher, empfiehlt es sich, dass Sie eine Partitionsschlüssel nicht angeben, es sei denn, dies erforderlich ist.

## <a name="advanced-topics-use-transactions-with-partitioned-entities"></a>Erweiterte Themen: Verwenden von Transaktionen mit partitionierten Einheiten

Nachrichten, die als Teil einer Transaktion gesendet werden, müssen Partitionsschlüssel angeben. Dies kann eine der folgenden Eigenschaften: [BrokeredMessage.SessionId][], [BrokeredMessage.PartitionKey][]oder [BrokeredMessage.MessageId][]. Alle Nachrichten, die als Teil der gleichen Transaktion gesendet werden, müssen den gleichen Partitionsschlüssel angeben. Wenn Sie versuchen, eine Nachricht ohne Partitionsschlüssel innerhalb einer Transaktion senden, gibt Dienstbus eine Ausnahme **InvalidOperationException** aus. Wenn Sie versuchen, mehrere Nachrichten innerhalb derselben Transaktion gesendet werden, die andere Partition Schlüssel enthalten, gibt Dienstbus eine Ausnahme **InvalidOperationException** aus. Beispiel:

```
CommittableTransaction committableTransaction = new CommittableTransaction();
using (TransactionScope ts = new TransactionScope(committableTransaction))
{
    BrokeredMessage msg = new BrokeredMessage("This is a message");
    msg.PartitionKey = "myPartitionKey";
    messageSender.Send(msg); 
    ts.Complete();
}
committableTransaction.Commit();
```

Wenn die Eigenschaften, die als Partitionsschlüssel dienen festgelegt sind, die Nachricht zu einem bestimmten Fragment, Dienstbus fixiert. Dieses Verhalten tritt auf, und zwar unabhängig davon, ob eine Transaktion verwendet wird. Es wird empfohlen, dass Sie eine Partitionsschlüssel nicht angeben, wenn es nicht erforderlich ist.

## <a name="using-sessions-with-partitioned-entities"></a>Verwenden von Sitzungen mit partitionierten Einheiten

Zum Senden einer Nachricht Transaktionen zu einer Sitzung-fähige Thema oder Warteschlange müssen die Nachricht die [BrokeredMessage.SessionId][] Eigenschaft festlegen. Wenn die Eigenschaft [BrokeredMessage.PartitionKey][] ebenfalls angegeben ist, müssen sie auf die Eigenschaft [SessionId][] identisch sein. Wenn sie sich unterscheiden, gibt Dienstbus eine Ausnahme **InvalidOperationException** aus.

Im Gegensatz zu regulären (nicht unterteilt) Warteschlangen oder Themen ist es nicht möglich, eine einzelne Transaktion verwenden, um mehrere Nachrichten zu anderen Sitzungen zu senden. Wenn versucht, gibt Dienstbus eine Ausnahme **InvalidOperationException **aus. Beispiel:

```
CommittableTransaction committableTransaction = new CommittableTransaction();
using (TransactionScope ts = new TransactionScope(committableTransaction))
{
    BrokeredMessage msg = new BrokeredMessage("This is a message");
    msg.SessionId = "mySession";
    messageSender.Send(msg); 
    ts.Complete();
}
committableTransaction.Commit();
```

## <a name="automatic-message-forwarding-with-partitioned-entities"></a>Automatische Weiterleitung mit Nachricht aufgeteilt Einheiten

Azure Dienstbus unterstützt automatischen Weiterleitung von, an oder zwischen partitionierten Einheiten. Um automatische Nachricht weiterleiten aktivieren zu können, legen Sie die Eigenschaft [QueueDescription.ForwardTo][] auf die Quellwarteschlange oder das Abonnement. Wenn die Meldung Partitionsschlüssel ([SessionId][], [PartitionKey][]oder [Nachrichten-ID][]) angegeben ist, wird dieser Partitionsschlüssel für die Zielentität verwendet.

## <a name="considerations-and-guidelines"></a>Wichtige Aspekte und Richtlinien

- **Hohe Konsistenz-Features**: eine Entität Features wie Sitzungen, Erkennung von Duplikaten oder explizite Steuerung Aufteilung Schlüssel verwendet, und die messaging Vorgänge zu bestimmten Fragmenten immer weitergeleitet werden. Wenn die Fragmente hohen Auslastung auftreten oder zugrunde liegenden Speicher fehlerhaft ist, wird diese Vorgänge fehl, und Verfügbarkeit wird verringert. Insgesamt ist die Konsistenz weiterhin sehr viel höher als nicht aufgeteilt Personen; nur eine Teilmenge der Datenverkehr ist im Gegensatz zu den gesamten Verkehr Probleme aufgetreten.
- **Projektmanagement**: JOIN-Operationen wie erstellen, aktualisieren und löschen auf alle Fragmente der Entität ausgeführt werden müssen. Wenn alle Fragment problematisch ist ergeben es Fehlern bei der für diese Vorgänge. Für den Ladevorgang muss Informationen wie Nachricht zählt über alle Fragmente aggregiert sein. Wenn alle Fragment problematisch ist, wird die Entität Verfügbarkeitsstatus als Beschränkung gemeldet.
- **Geringe Nachricht Szenarien**: solchen Situationen, besonders, wenn das HTTP-Protokoll verwenden, müssen Sie möglicherweise ein Vielfaches ausführen Vorgänge erhalten, um alle Nachrichten zu erhalten. Der front-End führt eine empfangen auf alle Fragmente empfangen Besprechungsanfragen und speichert alle Antworten erhalten. Die Anforderung einer nachfolgenden empfangen auf derselben Verbindung von diesem Cache profitieren und erhalten möchten Wartezeiten niedriger sein werden. Wenn Sie mehrere Verbindungen haben oder HTTP verwenden, erstellt, die jedoch eine neue Verbindung für jede Anforderung an. Es ist daher nicht gewährleistet, die es auf dem gleichen Knoten landen würden. Wenn alle vorhandene Nachrichten werden gesperrt und in einem anderen front-End-Cache, gibt der Receive-Methode **null**zurück. Schließlich Nachrichten ablaufen, und Sie können diese erneut erhalten. HTTP beibehalten aktiv wird empfohlen.
- **Durchsuchen/anheften Nachrichten**: PeekBatch gibt die Anzahl der Nachrichten, die in der [Eigenschaft MessageCount](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.messagecount.aspx)angegebenen nicht immer zurück. Es gibt zwei häufige Gründe dafür ein. Ein Grund darin, dass die zusammengefasste Größe der Sammlung von Nachrichten die maximale Größe von 256 KB überschreitet. Ein weiterer Grund ist, dass, wenn die Warteschlange oder das Thema der [Eigenschaft EnablePartitioning](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enablepartitioning.aspx) **Wahr**enthält, ist eine Partition möglicherweise nicht genügend Nachrichten, um die angeforderte Anzahl von Nachrichten abzuschließen. Im Allgemeinen ist eine Anwendung eine bestimmte Anzahl von Nachrichten empfangen werden sollen, muss sie aufrufen [PeekBatch](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.peekbatch.aspx) wiederholt, bis es die Anzahl der Nachrichten wird, oder es sind keine weiteren Nachrichten zum Einsehen aus. Weitere Informationen, einschließlich Codebeispielen finden Sie unter [QueueClient.PeekBatch](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.peekbatch.aspx) oder [SubscriptionClient.PeekBatch](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.peekbatch.aspx).

## <a name="latest-added-features"></a>Neueste hinzugefügten features

- Hinzufügen oder Entfernen von Regel wird nun mit partitionierten Einheiten unterstützt. Abweicht Elemente nicht aufgeteilt, werden diese Vorgänge nicht unter Transaktionen unterstützt. 
- AMQP wird nun zum Senden und Empfangen von Nachrichten an und aus einer partitionierten Entität unterstützt.
- AMQP wird nun für die folgenden Vorgänge unterstützt: [Stapel senden](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.sendbatch.aspx), [Stapel erhalten](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.receivebatch.aspx), [Empfangen von laufende Nummer](https://msdn.microsoft.com/library/azure/hh330765.aspx), [einsehen](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.peek.aspx), [Sperren erneuern](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.renewmessagelock.aspx), [Terminplan Nachricht](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.schedulemessageasync.aspx), [Geplant Nachricht abbrechen](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.cancelscheduledmessageasync.aspx), [Regel hinzufügen](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.ruledescription.aspx), [Regel entfernen](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.ruledescription.aspx), [Sitzung erneuern Sperren](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.renewlock.aspx), [Die Sitzung Status festlegen](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.setstate.aspx), [Get Session State](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.getstate.aspx), [Einsehen Sitzung Nachrichten](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.peek.aspx)und [Sitzungen auflisten](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.getmessagesessionsasync.aspx).

## <a name="partitioned-entities-limitations"></a>Einschränkungen partitionierten Einheiten

Aktuell auferlegt Dienstbus die folgenden Einschränkungen auf partitionierten Warteschlangen und Themen:

-   Partitionierte Warteschlangen und Themen unterstützt nicht sendende von Nachrichten, die zu anderen Sitzungen in einer einzigen Transaktion gehören.
-   Dienstbus ermöglicht aktuell bis zu 100 partitionierten Warteschlangen oder Themen pro Namespace. Jede partitionierte Warteschlange oder Thema ermittelt in Richtung das Kontingent 10.000 Elemente pro Namespace (gilt nicht für Premium Ebene).

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die Beiträge [AMQP 1.0-Unterstützung für Dienstbus aufgeteilt Warteschlangen und Themen][] , um weitere Informationen über das Aufteilen von Personen per aus. 

  [Dienst Busarchitektur]: service-bus-architecture.md
  [Azure-portal]: https://portal.azure.com
  [QueueDescription.EnablePartitioning]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enablepartitioning.aspx
  [TopicDescription.EnablePartitioning]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicdescription.enablepartitioning.aspx
  [BrokeredMessage.SessionId]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.sessionid.aspx
  [BrokeredMessage.PartitionKey]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.partitionkey.aspx
  [SessionId]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.sessionid.aspx
  [PartitionKey]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.partitionkey.aspx
  [QueueDescription.RequiresDuplicateDetection]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.requiresduplicatedetection.aspx
  [BrokeredMessage.MessageId]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx
  [Nachrichten-ID]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx
  [QueueDescription.RequiresDuplicateDetection]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.requiresduplicatedetection.aspx
  [MessagingFactorySettings.OperationTimeout]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx
  [OperationTimeout]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx
  [QueueDescription.ForwardTo]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.forwardto.aspx
  [AMQP 1.0-Unterstützung für aufgeteilt Servicebuswarteschlangen und Themen]: service-bus-partitioned-queues-and-topics-amqp-overview.md

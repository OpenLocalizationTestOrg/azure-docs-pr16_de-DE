<properties 
    pageTitle="Bewährte Methoden zum Verbessern der Leistung mit Dienstbus | Microsoft Azure"
    description="Beschreibt, wie Azure-Dienstbus zu verwenden, um die Leistung zu optimieren, wenn vermittelten Nachrichten austauschen."
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
    ms.date="10/25/2016"
    ms.author="sethm" />

# <a name="best-practices-for-performance-improvements-using-service-bus-messaging"></a>Bewährte Methoden zur Steigerung der Leistung mit Service Bus Messaging

In diesem Thema beschrieben, wie Sie die Azure Service Bus Messaging verwenden, um die Leistung zu optimieren, wenn vermittelten Nachrichten austauschen. Im erste Teil in diesem Thema werden die verschiedenen Methoden, die zur Verfügung stehen, um zu Leistung zu verbessern. Die zweite Komponente enthält Anleitungen zum Dienstbus auf eine Weise zu verwenden, die die optimale Leistung in einem bestimmten Szenario anbieten können.

In diesem Thema bezieht sich der Begriff "Client" auf eine beliebige Entität, die Dienstbus greift auf ein. Ein Client kann die Rolle des einen Absender oder eines Empfängers dauern. Der Begriff "Absender" wird für einen Dienstbus Warteschlange oder Thema Client verwendet, die Nachrichten an eine Dienstbus Warteschlange oder ein Thema sendet. Der Begriff "Empfänger" bezieht sich auf einen Dienstbus Warteschlange oder das Abonnement Client, der Nachrichten aus einer Warteschlange Dienstbus oder das Abonnement empfängt.

Diese Abschnitte erläutern mehrere Konzepte, die Dienstbus verwendet, um die Performance zu steigern.

## <a name="protocols"></a>Protokolle

Dienstbus ermöglicht Clients zum Senden und Empfangen von Nachrichten über drei Protokolle

1. Erweiterte Nachrichtenwarteschlangen-Protokoll (AMQP)

2. Dienstbus Messaging-Protokoll (SBMP)

3. HTTP

Sowohl die AMQP SBMP sind effizienter, da sie die Verbindung zu Dienstbus beibehalten, solange die messaging Factory vorhanden ist. Auch implementiert Batchverarbeitung und vorab. Sofern nicht ausdrücklich angegeben ist, wird alle Inhalte dieses Themas die Verwendung von AMQP oder SBMP angenommen.

## <a name="reusing-factories-and-clients"></a>Wiederverwenden von Factory und -clients

Dienstbus Client-Objekte, z. B. [QueueClient][] oder [NachrichtSender][], werden durch ein Objekt [MessagingFactory][] erstellt, die auch interne Verwaltung von Verbindungen bietet. Sie sollten nicht messaging Factory oder Warteschlange, Thema und Abonnement Clients schließen, nachdem Sie einer Nachricht senden, und klicken Sie dann neu werden erstellt, wenn Sie die nächste Nachricht zu senden. Schließen einer messaging Factory löscht die Verbindung zum Dienst Dienstbus und eine neue Verbindung wird hergestellt, wenn die Factory neu zu erstellen. Herstellen einer Verbindung ist ein teurer Vorgang, den Sie erneut mit der gleichen Factory und Client-Objekte für mehrere Vorgänge vermeiden können. Sie können das Objekt [QueueClient][] sicheres zum Senden von Nachrichten von gleichzeitige asynchrone Vorgänge und mehrere Threads verwenden. 

## <a name="concurrent-operations"></a>Gleichzeitige Vorgänge

Ausführen eines Vorgangs (senden, empfangen, löschen, usw.) dauert einige Zeit. Diesmal enthält der Vorgang vom Dienst Dienstbus sowie die Wartezeit der Anfrage und die Antwort an. Klicken Sie zum Erhöhen der Anzahl der Vorgänge pro Zeit müssen Vorgänge gleichzeitig ausgeführt werden. Dies kann auf verschiedene Weise erfolgen:

-   **Asynchrone Vorgänge**: der Client plant Vorgänge durch asynchrone Vorgänge ausführen. Die nächste Anforderung wird gestartet, bevor die vorherige Anforderung abgeschlossen ist. Im folgenden finden ein Beispiel für einen asynchronen Sendevorgang:

    ```
    BrokeredMessage m1 = new BrokeredMessage(body);
    BrokeredMessage m2 = new BrokeredMessage(body);
    
    Task send1 = queueClient.SendAsync(m1).ContinueWith((t) => 
      {
        Console.WriteLine("Sent message #1");
      });
    Task send2 = queueClient.SendAsync(m2).ContinueWith((t) => 
      {
        Console.WriteLine("Sent message #2");
      });
    Task.WaitAll(send1, send2);
    Console.WriteLine("All messages sent");
    ```

    Dies ist ein Beispiel für eine asynchrone receive-Methode:
    
    ```
    Task receive1 = queueClient.ReceiveAsync().ContinueWith(ProcessReceivedMessage);
    Task receive2 = queueClient.ReceiveAsync().ContinueWith(ProcessReceivedMessage);
    
    Task.WaitAll(receive1, receive2);
    Console.WriteLine("All messages received");
    
    async void ProcessReceivedMessage(Task<BrokeredMessage> t)
    {
      BrokeredMessage m = t.Result;
      Console.WriteLine("{0} received", m.Label);
      await m.CompleteAsync();
      Console.WriteLine("{0} complete", m.Label);
    }
    ```

-   **Mehrere Factory**: eine TCP-Verbindung zum Freigeben von alle Clients (zusätzlich zu den Empfängern Absender), die von der gleichen Factory erstellt wurden. Die maximale Nachrichtendurchsatz wird durch die Anzahl der Vorgänge eingeschränkt, die über diese Verbindung TCP wechseln können. Der Durchsatz, der mit einer einzelnen Factory abgerufen werden kann variiert stark mit Zeitangaben TCP und Nachrichtengröße. Um höhere Durchsatzraten zu erhalten, sollten Sie mehrere messaging Factory verwenden.

## <a name="receive-mode"></a>Erhalten von Modus

Wenn einen Client Warteschlange oder das Abonnement zu erstellen, können Sie angeben, dass einen Empfangsmodus: *Anheften-Sperren* oder *erhalten und löschen*. Die Standardeinstellung erhalten Modus ist [PeekLock][]. Wenn in diesem Modus ausgeführt wird, sendet der Client eine Anforderung an eine Nachricht von Dienstbus zu erhalten. Nachdem der Client die Nachricht empfangen wurde, wird eine Anforderung zum Abschließen der Nachricht gesendet.

Wenn den Empfangsmodus [ReceiveAndDelete][]festlegen möchten, werden beide Schritte in einer Anforderung kombiniert. Dies verringert die Anzahl der Vorgänge und kann die Nachricht Gesamtdurchsatz verbessern. Erhöhung der Leistung enthält bereits oben erwähnt Nachrichten verlieren.

Dienstbus unterstützt keine Transaktionen für erhalten und Löschen von Vorgängen. Darüber hinaus stehen anheften-Sperren Semantik [unzustellbare](service-bus-dead-letter-queues.md) einer Nachricht oder in dem der Client zurückstellen möchte Szenarien erforderlich.

## <a name="client-side-batching"></a>Clientseitige Batchverarbeitung

Clientseitige Batchverarbeitung kann ein Client Warteschlange oder Thema verzögern das Senden einer Nachricht, die für einen bestimmten Zeitraum. Wenn der Client zusätzliche Nachrichten während dieses Zeitraums sendet, übermittelt sie die Nachrichten in einem einzigen Stapel. Clientseitige Batchverarbeitung veranlasst auch einen Warteschlange/Abonnement Client an die Stapelverarbeitung mehrere **abgeschlossen** Anfragen in einer einzelnen Anforderung aus. Batchverarbeitung ist nur für asynchrone **Sende-** und **abgeschlossen** Vorgänge zur Verfügung. Synchroner Vorgänge werden sofort an den Dienstbus-Dienst gesendet. Stapeln keine treten für anheften oder Vorgänge erhalten, noch über Clients tritt Batchverarbeitung.

Wenn der Stapel die maximale Nachrichtengröße überschreitet, wird die letzte Nachricht aus dem Stapel entfernt und der Client sendet sofort den Stapel. Die letzte Nachricht wird die erste Nachricht der nächsten Gruppe an. Standardmäßig verwendet einen Client einem Stapel Zeitintervall 20 ms. Sie können das Intervall Stapel ändern, indem Sie vor dem Erstellen der messaging Factory [BatchFlushInterval][] Eigenschaft. Diese Einstellung wirkt sich auf alle Clients, die von dieser Factory erstellt wurden.

Um Batchverarbeitung zu deaktivieren, legen Sie die Eigenschaft [BatchFlushInterval][] **TimeSpan.Zero**aus. Beispiel:

```
MessagingFactorySettings mfs = new MessagingFactorySettings();
mfs.TokenProvider = tokenProvider;
mfs.NetMessagingTransportSettings.BatchFlushInterval = TimeSpan.FromSeconds(0.05);
MessagingFactory messagingFactory = MessagingFactory.Create(namespaceUri, mfs);
```

Batchverarbeitung wirkt sich nicht auf die Anzahl der berechenbaren messaging Vorgänge und steht nur für das Dienstbus Client-Protokoll. Das HTTP-Protokoll unterstützt keine Batchverarbeitung.

## <a name="batching-store-access"></a>Batchverarbeitung Store access

Klicken Sie zum Erhöhen des Durchsatzes Warteschlange/Thema/Abonnementtyp zusammenfasst Dienstbus mehrerer Nachrichten aus, wenn sie in ihren internen Speicher schreibt. Wenn Sie auf eine Warteschlange oder ein Thema aktiviert ist, wird Verfassen von Nachrichten in den Speicher zusammengefasst werden. Wenn auf einer Warteschlange oder das Abonnement aktiviert ist, wird das Löschen von Nachrichten aus dem Speicher zusammengefasst werden. Wenn Access gespeicherten Store für eine Entität aktiviert ist, wird die Dienstbus einen Schreibvorgang Store zu dieser Entität von bis zu 20 ms verzögert. Zusätzlicher Speichervorgänge, die während dieses Intervalls auftreten werden zum Stapel hinzugefügt. Access gespeicherten Store wirkt sich nur auf **Sende-** und **abgeschlossen** Vorgänge. empfangen Vorgänge sind hiervon nicht betroffen. Access gespeicherten Store ist eine Eigenschaft für eine Entität. Tritt auf, Batchverarbeitung für alle Personen, mit die gespeicherten Store zugreifen können.

Beim Erstellen einer neuen Warteschlange, Thema oder das Abonnement ist gespeicherten Store Access standardmäßig aktiviert. Um gespeicherten Store Zugriff zu deaktivieren, legen Sie die Eigenschaft [EnableBatchedOperations][] auf **false** vor dem Erstellen der Entität ein. Beispiel:

```
QueueDescription qd = new QueueDescription();
qd.EnableBatchedOperations = false;
Queue q = namespaceManager.CreateQueue(qd);
```

Access gespeicherten Store wirkt sich nicht auf die Anzahl der berechenbaren messaging Vorgänge und ist eine Eigenschaft eines Warteschlange, Thema oder Abonnements. Es ist unabhängig von den Empfangsmodus und das Protokoll, das zwischen einem Client und der Dienst Dienstbus verwendet wird.

## <a name="prefetching"></a>Vorab

Vorab kann den Client Warteschlange oder das Abonnement zusätzliche Nachrichten aus dem Dienst laden, wenn sie einen Receive-Vorgang ausführt. Der Client speichert diese Nachrichten in einem lokalen Cache. Die Größe des Caches wird durch die Eigenschaften [QueueClient.PrefetchCount][] oder [SubscriptionClient.PrefetchCount][] bestimmt. Jeder Client, die vorab ermöglicht verwaltet einen eigenen Cache. Ein Cache ist nicht über Clients freigegeben. Wenn der Client einen Receive-Vorgang initiiert und seinen Cache leer ist, übermittelt der Dienst eine Reihe von Nachrichten an. Die Größe des Stapels entspricht der Größe der Cache oder 256KB, je nachdem, was kleiner ist. Wenn der Client einen Receive-Vorgang initiiert, und der Cache eine Nachricht enthält, wird die Nachricht aus dem Cache übernommen.

Wenn eine Nachricht vorab ist, sperrt der Dienst die vorab abgerufene Nachricht aus. Auf diese Weise kann nicht von einem anderen Empfänger die vorab abgerufene Nachricht empfangen werden. Wenn der Empfänger die Nachricht ausführen kann, bevor die Sperre abläuft, wird die Nachricht an andere Empfänger verfügbar. Die vorab abgerufene Kopie der Nachricht bleibt im Cache. Der Empfänger, der das abgelaufene zwischengespeicherte, erhalten eine Ausnahme beim Versuch, die Nachricht fertigzustellen. Standardmäßig läuft die Sperre Nachricht nach 60 Sekunden. Dieser Wert kann auf 5 Minuten erweitert werden. Um zu verhindern, dass den Verbrauch abgelaufenen Nachrichten, sollten die Cachegröße immer kleiner als die Anzahl der Nachrichten, die von einem Client innerhalb des Intervalls der Sperren Timeout genutzt werden können.

Wenn Sie den standardmäßigen Sperren Ablauf von 60 Sekunden verwenden zu können, ist ein guter Wert für [SubscriptionClient.PrefetchCount][] 20 Mal die maximale Verarbeitung Sätze von Empfängern mit der Factory alle aus. Angenommen, eine Fabrik erstellt 3 Empfänger und jeder Empfänger kann bis zu 10 Nachrichten pro Sekunde verarbeiten. Die Anzahl der Prefetch sollte nicht überschreiten 20\*3\*10 = 600. Standardmäßig wird [QueueClient.PrefetchCount][] auf 0 festgelegt, was bedeutet, dass keine weiteren Nachrichten aus dem Dienst abgerufen werden.

Vorab Nachrichten erhöht der Gesamtdurchsatz für eine Warteschlange oder das Abonnement, da es sich um die Gesamtzahl der Nachrichtenvorgänge oder den Schleifen verringert. Abrufen von die erste Nachricht aus, allerdings dauert (aufgrund der höhere Nachrichtengröße) länger. Vorab abgerufene Nachrichten empfangen werden schneller sein, da diese Nachrichten vom Client bereits heruntergeladen wurden.

Die Eigenschaft Time to live (TTL) einer Nachricht ist vom Server gleichzeitig aktiviert, die der Server die Nachricht an den Client sendet. Der Client überprüft nicht die Gültigkeitsdauer-Eigenschaft der Meldung, wenn die Nachricht empfangen wurde. Stattdessen kann die Nachricht empfangen werden, auch wenn die Nachricht die Gültigkeitsdauer abgelaufen ist, während die Nachricht vom Client zwischengespeichert wurde.

Vorab wirkt sich nicht auf die Anzahl der berechenbaren messaging Vorgänge und steht nur für das Dienstbus Client-Protokoll. Das HTTP-Protokoll unterstützt keine vorab. Vorab steht zur Verfügung für synchroner und asynchrone Vorgänge erhalten.

## <a name="express-queues-and-topics"></a>Express Warteschlangen und Themen

Express Personen aktivieren hohen Durchsatz und Wartezeit Szenarien verringert. Mit express-Einheiten Wenn eine Nachricht an eine Warteschlange oder ein Thema gesendet werden wird die Nachricht nicht sofort im messaging Store gespeichert. In diesem Fall wird er im Speicher zwischengespeichert. Wenn eine Nachricht in der Warteschlange für mehr als ein paar Sekunden bleibt, wird sie automatisch, sodass zum Schutz vor Datenverlust aufgrund von einem Ausfall stabilen geschrieben. Schreiben die Nachricht in einem Speicher-Cache erhöht den Durchsatz und Wartezeit verringert, da kein Zugriff gleichzeitig stabilen, die die Nachricht gesendet wird. Nachrichten, die innerhalb von ein paar Sekunden verbraucht werden, werden nicht zum messaging Store geschrieben. Im folgende Beispiel wird eine ausdrückliche Thema erstellt.

```
TopicDescription td = new TopicDescription(TopicName);
td.EnableExpress = true;
namespaceManager.CreateTopic(td);
```

Wenn eine Nachricht mit wichtige Informationen, der nicht verloren muss auf eine ausdrückliche Entität gesendet wird, kann der Absender Dienstbus sofort die Nachricht mithilfe der [ForcePersistence][] -Eigenschaft auf **true**stabilen beibehalten werden erzwingen.

## <a name="use-of-partitioned-queues-or-topics"></a>Verwenden von partitionierten Warteschlangen oder Themen

Intern Dienstbus verwendet den gleichen Knoten und messaging zum Verarbeiten und speichern alle Nachrichten für messaging Entität (Warteschlange oder Thema) speichern. Eine partitionierte Warteschlange oder das Thema wird auf mehreren Knoten und messaging Stores andererseits, verteilt. Partitionierte Warteschlangen und Themen Rendite nicht nur auf eines höheren Durchsatzes als der reguläre Warteschlangen und Themen, die sie auch eine hohe Verfügbarkeit aufweisen. Um eine partitionierte Entität zu erstellen, legen Sie die Eigenschaft [EnablePartitioning][] **Wahr**, wie im folgenden Beispiel dargestellt. Weitere Informationen zu den partitionierten Personen finden Sie unter [messaging Personen aufgeteilt][].

```
// Create partitioned queue.
QueueDescription qd = new QueueDescription(QueueName);
qd.EnablePartitioning = true;
namespaceManager.CreateQueue(qd);
```

## <a name="use-of-multiple-queues"></a>Verwendung der mehrerer

Wenn es nicht möglich ist, eine partitionierte Warteschlange oder ein Thema zu verwenden oder die erwartete Auslastung kann nicht von einem einzelnen partitionierten Warteschlange oder Thema behandelt werden, müssen Sie mehrere messaging Personen verwenden. Erstellen Sie einen dedizierten Client für jede Entität, statt mit dem gleichen Client für alle Personen, bei der Verwendung von mehreren Personen.

## <a name="development--testing-features"></a>Entwicklung und Testen der Funktionen

Dienstbus besitzt eine Funktion, die speziell für die Entwicklung zu verwenden, welche **nie in Herstellung Konfigurationen verwendet werden soll**.

[TopicDescription.EnableFilteringMessagesBeforePublishing](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicdescription.enablefilteringmessagesbeforepublishing.aspx)
- Wenn neue Regeln oder Filter zum Thema hinzugefügt haben, kann EnableFilteringMessagesBeforePublishing verwendet werden, zu überprüfen, ob der neue Filterausdruck wie erwartet funktioniert.

## <a name="scenarios"></a>Szenarien

In den folgenden Abschnitten beschreiben typische messaging-Szenarien und Gliedern Sie die bevorzugten Dienstbus-Einstellungen. Durchsatzraten sind (weniger als 1 Nachricht/Sekunde) so klein klassifiziert, moderieren (1 Nachricht/Sekunde oder größer, aber weniger als 100 Nachrichten/Sekunde) hohen (100 Nachrichten/zweite oder höher). Die Anzahl der Clients als kleinen klassifiziert werden (5 oder weniger), Mittel (mehr als 5 aber kleiner oder gleich 20), und großen (mehr als 20).

### <a name="high-throughput-queue"></a>Hohem Durchsatz Warteschlange

Ziel: Maximieren des Durchsatzes einer einzelnen Warteschlange. Die Anzahl der Absender und Empfänger ist klein.

-   Verwenden Sie eine partitionierte Warteschlange für verbesserte Leistung und Verfügbarkeit.

-   Klicken Sie zum Erhöhen der globalen Prozentsatzes senden in der Warteschlange Formular mit mehreren Nachricht Factory Absender erstellen. Verwenden Sie für jeden Absender asynchrone Vorgänge oder mehrere Threads aus.

-   Verwenden Sie zum Erhöhen der globalen empfangen Prozentsatzes aus der Warteschlange mehrere Nachricht Factory, um Empfänger zu erstellen.

-   Verwenden Sie asynchrone Vorgänge, um clientseitige Batchverarbeitung nutzen.

-   Legen Sie das Intervall Batchverarbeitung auf 50 ms zum Verringern der Anzahl der Dienstbus Client-Protokoll Übertragung an. Wenn mehrere Absender verwendet werden, vergrößern Sie das Batchverarbeitung Intervall zu 100 ms

-   Lassen Sie die gespeicherten Store Zugriff aktiviert. Dies erhöht die generelle Rate, mit der Nachrichten in der Warteschlange geschrieben werden können.

-   Legen Sie die Anzahl der Prefetch 20 Mal die maximale Verarbeitung Sätze von Empfängern mit einer Factory alle fest. Dadurch wird die Anzahl der Dienstbus Client-Protokoll Übertragung reduziert.

### <a name="multiple-high-throughput-queues"></a>Mehrere hohem Durchsatz Warteschlangen

Ziel: Maximieren des mehrerer Gesamtdurchsatz an. Der Durchsatz eine einzelne Warteschlange ist Mittel oder hoch.

Verwenden Sie die Einstellungen, um einer einzelnen Warteschlange liegenden umrandet, um maximalen Durchsatz jede mehrere Warteschlange zu erhalten. Verwenden Sie darüber hinaus andere Factory Clients zu erstellen, die senden oder Empfangen von unterschiedliche Warteschlange, ein.

### <a name="low-latency-queue"></a>Geringe Wartezeit Warteschlange

Ziel: Minimieren Sie End-to-End-Wartezeit einer Warteschlange oder ein Thema. Die Anzahl der Absender und Empfänger ist klein. Der Durchsatz der Warteschlange ist klein oder Mittel.

-   Verwenden Sie eine partitionierte Warteschlange für verbesserte Verfügbarkeit ein.

-   Clientseitige Batchverarbeitung zu deaktivieren. Der Client sendet sofort eine Nachricht.

-   Deaktivieren Sie gespeicherten Store Zugriff. Der Dienst schreibt die Nachricht sofort zum Store.

-   Wenn einen einzelnen Client verwenden zu können, legen Sie die Anzahl der Prefetch 20 Mal Verarbeitung der Rate der Empfänger. Wenn mehrere Nachrichten in der Warteschlange gleichzeitig eintreffen, werden Sie von das Dienstbus Client-Protokoll alle zur gleichen Zeit übermittelt. Wenn der Client die nächste Nachricht empfängt, wird diese Nachricht bereits im lokalen Cache. Der Cache sollte klein sein.

-   Wenn mehrere Clients verwenden zu können, legen Sie die Anzahl der Prefetch auf 0 ein. Auf diese Weise kann der zweite Client die zweite Nachricht erhalten, während der erste Client die erste Nachricht noch verarbeitet wird.

### <a name="queue-with-a-large-number-of-senders"></a>Mit einer großen Anzahl von Absender Warteschlange

Ziel: Maximieren Sie Durchsatz einer Warteschlange oder ein Thema mit einer großen Anzahl von Absender. Jeden Absender sendet Nachrichten mit einem moderieren Stundensatz. Die Anzahl der Empfänger ist klein.

Dienstbus ermöglicht maximal 1000 aktiven Verbindungen zu einer Person per (oder 5000 AMQP verwenden). Dieser Grenzwert wird auf der Namespaceebene erzwungen und Warteschlangen/Themen/Abonnements sind die Beschränkung der aktiven Verbindungen pro Namespace Pfeilspitze endet. Für Warteschlangen werden diese Zahl zwischen Absender und Empfänger freigegeben. Wenn alle 1000 Verbindungen für Absender erforderlich sind, sollten Sie die Warteschlange mit einem Thema und ein einzelnes Abonnement ersetzen. Ein Thema akzeptiert maximal 1000 aktiven Verbindungen von Absendern, während das Abonnement einer zusätzlichen 1000 aktiven Verbindungen von den Empfängern akzeptiert. Wenn mehr als 1000 gleichzeitige Absender erforderlich sind, sollte der Absender Nachrichten an das Protokoll Dienstbus über HTTP senden.

Um liegenden, führen Sie folgende Schritte aus:

-   Verwenden Sie eine partitionierte Warteschlange für verbesserte Leistung und Verfügbarkeit.

-   Wenn jeden Absender in einem anderen Prozess befindet, verwenden Sie nur eine einzelne Factory pro Prozess.

-   Verwenden Sie asynchrone Vorgänge, um clientseitige Batchverarbeitung nutzen.

-   Verwenden Sie die Standardeinstellung Batchverarbeitung Intervall von 20 ms zum Verringern der Anzahl der Dienstbus Client-Protokoll Übertragung aus.

-   Lassen Sie die gespeicherten Store Zugriff aktiviert. Dies erhöht die generelle Rate, mit der Nachrichten in der Warteschlange oder das Thema geschrieben werden können.

-   Legen Sie die Anzahl der Prefetch 20 Mal die maximale Verarbeitung Sätze von Empfängern mit einer Factory alle fest. Dadurch wird die Anzahl der Dienstbus Client-Protokoll Übertragung reduziert.

### <a name="queue-with-a-large-number-of-receivers"></a>Mit einer großen Anzahl von Empfängern mit Warteschlange

Ziel: Maximieren der Kostensatz empfangen eine Warteschlange oder das Abonnement mit einer großen Anzahl von Empfängern mit. Jeder Empfänger erhält Nachrichten mit einer mittleren Geschwindigkeit. Die Anzahl der Absender ist klein.

Dienstbus ermöglicht maximal 1000 gleichzeitige Verbindungen mit einer Entität. Wenn eine Warteschlange mehr als 1000 Empfänger erforderlich ist, sollten Sie die Warteschlange mit einem Thema und mehrere Abonnements ersetzen. Jedes Abonnement kann bis zu 1000 aktiven Verbindungen unterstützen. Alternativ können die Empfänger die Warteschlange über HTTP zugreifen.

Um liegenden, führen Sie folgende Schritte aus:

-   Verwenden Sie eine partitionierte Warteschlange für verbesserte Leistung und Verfügbarkeit.

-   Wenn sich jeder Empfänger in einem anderen Prozess befindet, verwenden Sie nur eine einzelne Factory pro Prozess.

-   Empfänger können synchron oder asynchrone Vorgänge. Ausgehend von einem einzelnen Empfänger der Kostensatz moderieren empfangen, wirkt clientseitige Batchverarbeitung eines vollständigen Antrags Empfänger Durchsatz sich nicht.

-   Lassen Sie die gespeicherten Store Zugriff aktiviert. Dadurch wird die gesamte Laden der Entität verringert. Es verringert auch die generelle Rate, mit der Nachrichten in der Warteschlange oder das Thema geschrieben werden können.

-   Legen Sie die Anzahl die Prefetch auf einen kleinen Wert (z. B. PrefetchCount = 10). Dadurch wird verhindert, dass Empfänger Leerlauf, während andere Empfänger großen Anzahl von Nachrichten Cache haben.

### <a name="topic-with-a-small-number-of-subscriptions"></a>Thema mit einer kleinen Anzahl von Abonnements

Ziel: Maximieren des Durchsatzes eines Themas mit einer kleinen Anzahl von Abonnements. Eine Meldung vom viele Abonnements, was bedeutet, dass die kombinierten empfangen Rate über alle Abonnements größer als die Rate senden. Die Anzahl der Absender ist klein. Die Anzahl der Empfänger pro Abonnement ist klein.

Um liegenden, führen Sie folgende Schritte aus:

-   Verwenden Sie ein Thema partitionierten für verbesserte Leistung und Verfügbarkeit.

-   Verwenden Sie zum Erhöhen der globalen senden Prozentsatzes in das Thema mehrere Nachricht Factory Absender zu erstellen. Verwenden Sie für jeden Absender asynchrone Vorgänge oder mehrere Threads aus.

-   Verwenden Sie zum Erhöhen der globalen empfangen Prozentsatzes aus einem Abonnement mehrere Nachricht Factory, um Empfänger zu erstellen. Verwenden Sie für jeden Empfänger asynchrone Vorgänge oder mehrere Threads aus.

-   Verwenden Sie asynchrone Vorgänge, um clientseitige Batchverarbeitung nutzen.

-   Verwenden Sie die Standardeinstellung Batchverarbeitung Intervall von 20 ms zum Verringern der Anzahl der Dienstbus Client-Protokoll Übertragung aus.

-   Lassen Sie die gespeicherten Store Zugriff aktiviert. Dies erhöht die generelle Rate, mit der Nachrichten in das Thema geschrieben werden können.

-   Legen Sie die Anzahl der Prefetch 20 Mal die maximale Verarbeitung Sätze von Empfängern mit einer Factory alle fest. Dadurch wird die Anzahl der Dienstbus Client-Protokoll Übertragung reduziert.

### <a name="topic-with-a-large-number-of-subscriptions"></a>Thema mit einer großen Anzahl von Abonnements

Ziel: Maximieren des Durchsatzes eines Themas mit einer großen Anzahl von Abonnements. Durch viele Abonnements, was bedeutet, dass die kombinierten empfangen Rate über alle Abonnements viel größer als die Rate senden ist eine Nachricht empfangen. Die Anzahl der Absender ist klein. Die Anzahl der Empfänger pro Abonnement ist klein.

Themen mit einer großen Anzahl von Abonnements verfügbar machen in der Regel eine niedrige Gesamtdurchsatz an, wenn alle Nachrichten an alle Abonnements weitergeleitet werden. Dies ist zurückzuführen der Fakultät, die jeder Nachricht empfangen oft, und alle Nachrichten, die in einem Thema enthalten sind und alle zugehörigen Abonnements im gleichen Speicher gespeichert werden. Es wird vorausgesetzt, dass die Anzahl der Absender und die Anzahl der Empfänger pro Abonnement klein ist. Dienstbus unterstützt bis zu 2.000 Abonnements pro Thema.

Um liegenden, führen Sie folgende Schritte aus:

-   Verwenden Sie ein Thema partitionierten für verbesserte Leistung und Verfügbarkeit.

-   Verwenden Sie asynchrone Vorgänge, um clientseitige Batchverarbeitung nutzen.

-   Verwenden Sie die Standardeinstellung Batchverarbeitung Intervall von 20 ms zum Verringern der Anzahl der Dienstbus Client-Protokoll Übertragung aus.

-   Lassen Sie die gespeicherten Store Zugriff aktiviert. Dies erhöht die generelle Rate, mit der Nachrichten in das Thema geschrieben werden können.

-   Legen Sie die Anzahl der Prefetch auf 20 Mal der erwarteten empfangen Zins in Sekunden an. Dadurch wird die Anzahl der Dienstbus Client-Protokoll Übertragung reduziert.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Optimieren der Leistung Dienstbus finden Sie unter [partitionierte messaging Einheiten][].

  [QueueClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx
  [NachrichtSender]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx
  [MessagingFactory]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx
  [PeekLock]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx
  [ReceiveAndDelete]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx
  [BatchFlushInterval]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.netmessagingtransportsettings.batchflushinterval.aspx
  [EnableBatchedOperations]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enablebatchedoperations.aspx
  [QueueClient.PrefetchCount]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.prefetchcount.aspx
  [SubscriptionClient.PrefetchCount]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.prefetchcount.aspx
  [ForcePersistence]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.forcepersistence.aspx
  [EnablePartitioning]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enablepartitioning.aspx
  [Partitionierte messaging Einheiten]: service-bus-partitioning.md
  
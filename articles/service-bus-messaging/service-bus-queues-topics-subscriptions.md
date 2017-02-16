<properties 
    pageTitle="Servicebuswarteschlangen, Themen und Abonnements | Microsoft Azure"
    description="Übersicht über die Dienstbus messaging Einheiten."
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
    ms.date="10/14/2016"
    ms.author="sethm" />

# <a name="service-bus-queues-topics-and-subscriptions"></a>Servicebuswarteschlangen, Themen und Abonnements

Microsoft Azure-Dienstbus unterstützt eine Reihe von cloudbasierten, Nachricht Orientierung Middleware Technologien, einschließlich Nachrichtenwarteschlangen zuverlässigen und dauerhaften Veröffentlichen/Abonnieren messaging. Diese vermittelten messaging-Funktionen können als Entkoppelter messaging-Funktionen, die Unterstützung veröffentlichen-abonnieren, zeitliche Entkopplung und Lastenausgleich Szenarien mit messaging Fabric Dienstbus betrachtet werden. Entkoppelter Kommunikation bietet viele Vorteile; Beispielsweise können Clients und Servern eine Verbindung nach Bedarf und deren Operationen in einer asynchronen Weise.

Die messaging Elemente, die das Herzstück der vermittelten messaging-Funktionen in Dienstbus bilden sind Warteschlangen Themen/Abonnements und Regeln/Aktionen.

## <a name="queues"></a>Warteschlangen

Warteschlangen bieten ersten, Nachrichtenübermittlung für einen oder mehrere verschiedenen Consumer ersten Out (FIFO). D. h., werden Nachrichten in der Regel erwartet empfangen und von den Empfängern in der Reihenfolge, in der er hinzugefügt wurden, in der Warteschlange und jede Nachricht empfangen und verarbeiteten nur eine Nachricht Consumer, verarbeitet werden. Ein wesentlicher Vorteil der Verwendung von Warteschlangen ist "zeitliche Entkopplung" der Anwendungskomponenten erzielen. Kurzum, müssen Herstellern (Absender) und Nutzer (der Empfänger) nicht senden und Empfangen von Nachrichten zur gleichen Zeit, da Nachrichten als in der Warteschlange gespeichert werden. Darüber hinaus hat der Hersteller keine Antwort vom Consumer warten, um weiterhin verarbeiten und Nachrichten zu senden.

Ein verwandter Vorteil ist "Laden Abgleich," wodurch Hersteller und Verbraucher zu senden und Empfangen von Nachrichten mit unterschiedlichen Zinssätzen ermöglicht. Ändert sich in vielen Programmen die laden System über einen Zeitraum; die für jede Arbeitseinheit benötigte Verarbeitungszeit ist jedoch in der Regel Konstante. Intermediating Nachricht Hersteller und Nutzer mit einer Warteschlange bedeutet, dass nur die in Anspruch nehmen Anwendung bereitgestellt werden, um die durchschnittliche Auslastung statt die Belastung verarbeiten können. Die Tiefe der Warteschlange wächst und reduziert, wenn die eingehende laden wird aktualisiert. Dadurch wird direkt Geld in Bezug auf die Menge der Infrastruktur erforderlich, um die Anwendung laden service gespeichert. Zunehmender die laden können weitere Arbeitsprozesse Lesen aus der Warteschlange hinzugefügt werden. Jede Nachricht wird nur von einem der Arbeitsprozesse verarbeitet werden. Darüber hinaus ermöglicht diese Abruf-basierten Lastenausgleich für optimale Nutzung der Computer Worker selbst wenn die Worker Computer im Hinblick auf Verarbeitung Power, unterscheiden sich diese Nachrichten mit eigene maximale Rate ziehen werden. Dieses Muster wird häufig als das Muster "Mitbewerber auftritt Consumer" bezeichnet.

Mithilfe von Warteschlangen, um zwischen den Hersteller der Nachricht und Nutzer mittlere bietet einen gehörende Kopplung zwischen den Komponenten. Da Hersteller und Verbraucher nicht miteinander bekannt sind, kann ein Consumer ohne Auswirkung auf die Producer aktualisiert werden.

Erstellen einer Warteschlange ist mehreren Schritten. Sie Operationen Management für messaging Einheiten (Warteschlangen und Themen) Dienstbus über die [Microsoft.ServiceBus.NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) -Klasse, indem Sie die Basisadresse des Namespace Dienstbus und die Anmeldeinformationen des Benutzers erstellt wird. [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) bietet Methoden zum Erstellen, auflisten und messaging Elemente löschen. Nach dem Erstellen eines Objekts [Microsoft.ServiceBus.TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx) aus den Namen der SAS und Schlüssel und ein Dienst Namespace Management-Objekt, können Sie die [Microsoft.ServiceBus.NamespaceManager.CreateQueue](https://msdn.microsoft.com/library/azure/hh293157.aspx) Methode zum Erstellen der Warteschlange. Beispiel:

```
// Create management credentials
TokenProvider credentials = TokenProvider. CreateSharedAccessSignatureTokenProvider(sasKeyName,sasKeyValue);
// Create namespace client
NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", ServiceNamespace, string.Empty), credentials);
```

Sie können dann ein Warteschlangenobjekt und eine messaging Factory mit dem Dienst Bus URI als Argument erstellen. Beispiel:

```
QueueDescription myQueue;
myQueue = namespaceClient.CreateQueue("TestQueue");
MessagingFactory factory = MessagingFactory.Create(ServiceBusEnvironment.CreateServiceUri("sb", ServiceNamespace, string.Empty), credentials); 
QueueClient myQueueClient = factory.CreateQueueClient("TestQueue");
```

Sie können dann Nachrichten an die Warteschlange senden. Angenommen, Sie verfügen über eine Liste der vermittelten Nachrichten so genannte `MessageList`, wird der Code wie folgt angezeigt:

```
for (int count = 0; count < 6; count++)
{
    var issue = MessageList[count];
    issue.Label = issue.Properties["IssueTitle"].ToString();
    myQueueClient.Send(issue);
}
```

Sie können dann Nachrichten empfangen aus der Warteschlange, wie folgt:

```
while ((message = myQueueClient.Receive(new TimeSpan(hours: 0, minutes: 0, seconds: 5))) != null)
    {
        Console.WriteLine(string.Format("Message received: {0}, {1}, {2}", message.SequenceNumber, message.Label, message.MessageId));
        message.Complete();

        Console.WriteLine("Processing message (sleeping...)");
        Thread.Sleep(1000);
    }
```

In den Modus [ReceiveAndDelete](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) ist der Receive-Methode Single-Screenshot. d. h., wenn Dienstbus die Anforderung empfängt, es kennzeichnet die Nachricht als verbraucht wird, und gibt es mit der Anwendung. [ReceiveAndDelete](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) -Modus ist die einfachste Modell und am besten geeignet für Szenarien, in denen die Anwendung tolerieren kann nicht verarbeiten einer Nachricht im Fall eines Fehlers. Um dies zu verstehen, sollten Sie ein Szenario, in dem der Verbraucher gibt die Anforderung empfangen und dann stürzt ab, bevor Sie ihn aufbereiten, aus. Da Dienstbus die Nachricht kennzeichnet als verbraucht, wenn die Anwendung neu gestartet und erneut Verarbeitung von Nachrichten beginnt, wird es die Nachricht verpasst haben, die vor der Absturz verbraucht wurde.

Im Modus " [PeekLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) " wird der Receive-Methode zwei Phasen, ermöglicht die Unterstützung von Applications, die fehlende Nachrichten tolerieren. Wenn Dienstbus die Anforderung empfängt, es findet die nächste Nachricht und genutzt werden sollen, sperrt, um zu verhindern, dass andere Nutzer erhalten es, und gibt es dann an die Anwendung. Nach die Anwendung endet Verarbeiten der Nachricht (oder zuverlässig für die Verarbeitung von zukünftigen gespeichert), abgeschlossen durch Einwahl [abgeschlossen](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) auf die empfangene Nachricht den endgültigen des Prozesses empfangen wird. Wenn Dienstbus der [abgeschlossen](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) anrufen sieht, kennzeichnet die Nachricht als verarbeitet werden.

Wenn die Anwendung zum Verarbeiten der Nachricht aus irgendeinem Grund nicht kann, kann er [aufgeben](https://msdn.microsoft.com/library/azure/hh181837.aspx) -Methode für die empfangene Nachricht (statt [abgeschlossen](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx)) aufrufen. Dadurch Dienstbus zum Aufheben der Sperre der Nachricht und Verfügbarmachen erneut vom gleichen Consumer oder von einem anderen verschiedenen Consumer empfangen werden sollen. Als Nächstes ein Timeout die Sperre zugeordnet ist und bei der Anwendung, zum Verarbeiten der Nachricht auftritt vor dem Sperren Timeout läuft ab (z. B., wenn die Anwendung stürzt ab) und dann Dienstbus die Nachricht die Sperre aufhebt, und es kann erneut empfangen werden sollen (im Wesentlichen eines Vorgangs [aufgeben](https://msdn.microsoft.com/library/azure/hh181837.aspx) standardmäßig).

Beachten Sie, dass den Fall, dass die Anwendung stürzt ab, nach dem Verarbeiten der Nachricht, aber bevor die Anforderung [abgeschlossen](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) ausgegeben wird, wird nach dem Neustart wird die Nachricht an die Anwendung erneut zugestellt. Dies wird häufig *Mindestenseinmal *Verarbeitung bezeichnet; d. h., wird jede Nachricht mindestens einmal verarbeitet werden. Jedoch möglicherweise in bestimmten Situationen dieselbe Nachricht zugestellt. Wenn Sie das Szenario tolerieren doppelte Verarbeitung, und klicken Sie dann zusätzliche Logik, in der Anwendung erforderlich ist zu erkennen, dass anhand Duplikate die erreicht werden können die **Nachrichten-ID** -Eigenschaft der Nachricht, die über die Übermittlungsversuche konstant bleibt. Dies wird als *Einmalig* bezeichnet verarbeiten.

Weitere Informationen und ein Arbeitsbeispiel zum Erstellen und Senden von Nachrichten an und von Warteschlangen finden Sie unter der [Dienstbus vermittelte messaging .NET Lernprogramm](service-bus-brokered-tutorial-dotnet.md).

## <a name="topics-and-subscriptions"></a>Themen und Abonnements

Im Gegensatz zu Warteschlangen, in denen jede Nachricht von einer einzelnen Consumer, verarbeitet wird bieten *Themen* und *Abonnements* eine 1: n-Formular Kommunikationsmethode, in ein Muster *Veröffentlichen/Abonnieren* . Nützlich für die Skalierung für eine große Anzahl der Empfänger, wird jede Nachricht veröffentlichten für jedes Abonnement mit dem Thema registriert geführt. Nachrichten werden gesendet zu einem Thema und übermittelt, um einen oder mehrere zugeordneten Abonnements, je nach der Filterregeln, die auf Basis pro Abonnement festgelegt werden können. Zusätzliche Filter können die Abonnements die Nachrichten einschränken möchten, die sie erhalten möchten. Nachrichten werden gesendet zu einem Thema auf die gleiche Weise, die sie an eine Warteschlange gesendet werden, aber die Nachrichten werden nicht direkt aus dem Thema empfangen. Stattdessen werden sie von Abonnements erhalten. Ein Thema Abonnement ähnelt eine virtuelle Warteschlange, die Kopien der Nachrichten empfangen werden, die mit dem Thema gesendet werden. Nachrichten werden aus einem Abonnement genauso wie die Art und Weise empfangen, die sie aus einer Warteschlange empfangen werden.

Klicken Sie zum Vergleich die Funktionalität Nachricht senden einer Warteschlange Zuordnungen direkt zu einem Thema, und seine Funktionalität Nachricht empfangen ordnet ein Abonnement. Unter anderem Dies bedeutet, dass Abonnements in diesem Abschnitt im Hinblick auf Warteschlangen zuvor beschriebenen dieselben Muster unterstützen: verschiedenen Consumer, zeitliche Entkopplung, Abgleich laden und den Lastenausgleich.

Erstellen ein Thema ähnelt dem Erstellen einer Warteschlange, wie im Beispiel im vorherigen Abschnitt dargestellt. Erstellen Sie den Dienst-URI, und klicken Sie dann verwenden Sie die [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) Klasse, um den Namespace Client zu erstellen. Anschließend können Sie ein Thema mithilfe der Methode [CreateTopic](https://msdn.microsoft.com/library/azure/hh293080.aspx) erstellen. Beispiel:

```
TopicDescription dataCollectionTopic = namespaceClient.CreateTopic("DataCollectionTopic");
```

Fügen Sie als Nächstes Abonnements wie gewünscht hinzu:

```
SubscriptionDescription myAgentSubscription = namespaceClient.CreateSubscription(myTopic.Path, "Inventory");
SubscriptionDescription myAuditSubscription = namespaceClient.CreateSubscription(myTopic.Path, "Dashboard");
```

Anschließend können Sie einen Thema Client erstellen. Beispiel:

```
MessagingFactory factory = MessagingFactory.Create(serviceUri, tokenProvider);
TopicClient myTopicClient = factory.CreateTopicClient(myTopic.Path)
```

Der Absender der Nachricht können Sie senden und empfangen zu und aus dem Thema, wie im vorherigen Abschnitt dargestellt. Beispiel:

```
foreach (BrokeredMessage message in messageList)
{
    myTopicClient.Send(message);
    Console.WriteLine(
    string.Format("Message sent: Id = {0}, Body = {1}", message.MessageId, message.GetBody<string>()));
}
```

Ähnlich wie Warteschlangen, werden Nachrichten aus einem Abonnement mithilfe eines Objekts [SubscriptionClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.aspx) anstelle eines Objekts [QueueClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx) empfangen. Erstellen des Abonnement-Clients, den Namen des Themas, den Namen des Abonnements und (optional) den Empfangsmodus als Parameter zu übergeben. Angenommen, mit dem Abonnement **Inventory** :

```
// Create the subscription client
MessagingFactory factory = MessagingFactory.Create(serviceUri, tokenProvider); 

SubscriptionClient agentSubscriptionClient = factory.CreateSubscriptionClient("IssueTrackingTopic", "Inventory", ReceiveMode.PeekLock);
SubscriptionClient auditSubscriptionClient = factory.CreateSubscriptionClient("IssueTrackingTopic", "Dashboard", ReceiveMode.ReceiveAndDelete); 

while ((message = agentSubscriptionClient.Receive(TimeSpan.FromSeconds(5))) != null)
{
    Console.WriteLine("\nReceiving message from Inventory...");
    Console.WriteLine(string.Format("Message received: Id = {0}, Body = {1}", message.MessageId, message.GetBody<string>()));
    message.Complete();
}          

// Create a receiver using ReceiveAndDelete mode
while ((message = auditSubscriptionClient.Receive(TimeSpan.FromSeconds(5))) != null)
{
    Console.WriteLine("\nReceiving message from Dashboard...");
    Console.WriteLine(string.Format("Message received: Id = {0}, Body = {1}", message.MessageId, message.GetBody<string>()));
}
```

### <a name="rules-and-actions"></a>Regeln und Aktionen

In vielen Szenarios müssen Nachrichten mit bestimmten Merkmale auf unterschiedliche Weise verarbeitet werden. Um dies zu aktivieren, können Sie Abonnements, um Nachrichten zu finden, die Eigenschaften haben gewünscht, und führen Sie dann auf bestimmte Änderungen an diesen Eigenschaften konfigurieren. Während Dienstbus Abonnements alle Nachrichten mit dem Thema angezeigt wird, können Sie nur eine Teilmenge dieser Nachrichten an die Warteschlange virtuelle Abonnement kopieren. Dies geschieht Abonnementfilter verwenden. Solche Änderungen werden *Filteraktionen*bezeichnet. Wenn ein Abonnement erstellt wurde, können Sie einen Filterausdruck bereitstellen, der für die Eigenschaften der Nachricht, die Systemeigenschaften (z. B. **Bezeichnung**) und die benutzerdefinierte Anwendungseigenschaften (z. B. **StoreName**.) ausgeführt wird In diesem Fall ist der SQL-Filterausdruck optional; ohne einen SQL-Filterausdruck wird auf ein Abonnement definierte Filteraktion auf alle Nachrichten für dieses Abonnement ausgeführt werden.

Im vorherigen Beispiel, zum Filtern von Nachrichten in Kürze nur von **1**, würden Sie im Dashboard-Abonnement folgendermaßen erstellen:

```
namespaceManager.CreateSubscription("IssueTrackingTopic", "Dashboard", new SqlFilter("StoreName = 'Store1'"));
```

Mit diesem Abonnementfilter an Ort, nur Nachrichten mit einem der `StoreName` -Eigenschaft festgelegt `Store1` werden in die virtuelle Warteschlange für kopiert die `Dashboard` Abonnement.

Weitere Informationen zu den möglichen Filterwerte finden Sie in der Dokumentation für die Klassen [SqlFilter](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx) und [SqlRuleAction](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlruleaction.aspx) . Sehen Sie sich auch die [vermittelte Messaging: Erweiterte Filter](http://code.msdn.microsoft.com/Brokered-Messaging-6b0d2749) und Beispielen [Thema Filter](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/TopicFilters) .

## <a name="next-steps"></a>Nächste Schritte

Finden Sie die folgenden erweiterten Themen für Weitere Informationen und Beispiele für die Verwendung von Dienstbus vermittelte messaging Elemente aus.

- [Übersicht über Dienstbus messaging](service-bus-messaging-overview.md)
- [Vermittelte Dienstbus messaging .NET Lernprogramm](service-bus-brokered-tutorial-dotnet.md)
- [Vermittelte Dienstbus messaging REST Lernprogramm](service-bus-brokered-tutorial-rest.md)
- [Beispiel für Thema Filter](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/TopicFilters)
- [Vermittelten Messaging: Erweiterte Filter Stichprobe](http://code.msdn.microsoft.com/Brokered-Messaging-6b0d2749)


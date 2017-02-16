<properties 
    pageTitle="AMQP 1.0-Unterstützung für Dienstbus aufgeteilt Warteschlangen und Themen | Microsoft Azure" 
    description="Informationen Sie zur Verwendung von Erweiterte Message Queuing-Protokoll (AMQP) 1.0 mit Dienstbus aufgeteilt Warteschlangen und Themen." 
    services="service-bus" 
    documentationCenter=".net" 
    authors="hillaryc" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="10/14/2016" 
    ms.author="hillaryc;sethm"/>

# <a name="amqp-10-support-for-service-bus-partitioned-queues-and-topics"></a>AMQP 1.0-Unterstützung für aufgeteilt Servicebuswarteschlangen und Themen 

Azure Dienstbus unterstützt jetzt die erweiterte Message Queuing Protocol (**AMQP**) 1.0 für Dienstbus **Warteschlangen aufgeteilt und Themen.**

**AMQP** ist ein Warteschlange Nachricht öffnen-Standard-Protokoll, zur Plattform-Anwendung mit verschiedenen Sprachen ermöglicht. Allgemeine Informationen zu AMQP in Dienstbus unterstützen Sie, finden Sie unter [AMQP 1.0 in Dienstbus unterstützen](service-bus-amqp-overview.md).

**Partitionierte Warteschlangen und Themen**, auch bekannt als *partitionierte Einheiten*, bieten höhere Verfügbarkeit, Zuverlässigkeit und Durchsatz als konventionelle nicht aufgeteilt Warteschlangen und Themen. Weitere Informationen zu den partitionierten Personen finden Sie unter [Partitionierten Messaging Einheiten](service-bus-partitioning.md).

Das Hinzufügen von AMQP 1.0 als ein Protokoll für die Kommunikation mit partitionierten Warteschlangen und Themen ermöglicht es Ihnen, Sie Interoperabilität von Applications zu erstellen, die können die höhere Verfügbarkeit, Zuverlässigkeit, nutzen und in der gesamten Angeboten von Personen Dienstbus aufgeteilt, werden soll.

Eine detaillierte auf niedriger Ebene AMQP 1.0 Protokoll Anleitung, die erläutert, wie Dienstbus implementiert, und klicken Sie auf der OASIS AMQP technische Spezifikation erstellt, finden Sie die [AMQP 1.0 in Azure Service und Ereignis Hubs Protokoll Leitfaden](service-bus-amqp-protocol-guide.md).    

## <a name="use-amqp-with-partitioned-queues"></a>Verwenden von AMQP mit partitionierten Warteschlangen

Warteschlangen sind nützlich für Szenarien, die zeitliche Entkopplung, Abgleich laden, den Lastenausgleich, und die Kopplung erfordern. Mit einer Warteschlange Herausgeber Senden von Nachrichten an die Warteschlange und Nutzer empfangen von Nachrichten aus der Warteschlange, in Fällen, wo eine Nachricht nur einmal empfangen werden kann. Ein gute Beispiel hierfür ist ein Inventory System in der Kassenterminals Daten an eine Warteschlange statt direkt an das Inventory Managementsystem veröffentlichen. Das Inventory Managementsystem nutzt dann die Daten zu einem beliebigen Zeitpunkt Teile am verwalten. Dies hat mehrere Vorteile, einschließlich des Inventory Management Systems nicht jederzeit online sein müssen. Weitere Details Servicebuswarteschlangen finden Sie unter [Erstellen von Applications, die Servicebuswarteschlangen verwenden](service-bus-create-queues.md). 

Eine partitionierte Warteschlange weiter erhöht die Verfügbarkeit, Zuverlässigkeit und Durchsatz für Applikationen, wie diese Warteschlangen über mehrere Nachrichtenmakler und messaging Stores formatiert sind.     

### <a name="create-partitioned-queues"></a>Partitionierte Warteschlange erstellen

Sie können eine partitionierte Warteschlange mit [Azure klassischen Portal][] und dem Dienst Bus SDK erstellen. Um eine partitionierte Warteschlange zu erstellen, legen Sie die Eigenschaft [EnablePartitioning](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enablepartitioning.aspx) **Wahr** in der [QueueDescription](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.aspx) -Instanz aus. Mit dem folgende Code veranschaulicht, wie eine partitionierte Warteschlange mit dem Dienst Bus SDK zu erstellen. 
 
```
// Create partitioned queue
var nm = NamespaceManager.CreateFromConnectionString(myConnectionString);
var queueDescription = new QueueDescription("myQueue");
queueDescription.EnablePartitioning = true;
nm.CreateQueue(queueDescription);
```

### <a name="send-and-receive-messages-using-amqp"></a>Senden und Empfangen von Nachrichten mithilfe von AMQP

Sie können Nachrichten zu senden und Empfangen von Nachrichten aus einer partitionierten Warteschlange AMQP als Protokoll verwenden, indem Sie die Eigenschaft [TransportType](https://msdn.microsoft.com/library/azure/microsoft.servicebus.servicebusconnectionstringbuilder.transporttype.aspx) auf [TransportType.Amqp](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.transporttype.aspx) festlegen, wie im folgenden Code dargestellt.  

```
// Sending and receiving messages to and from a queue
var myConnectionStringBuilder = new ServiceBusConnectionStringBuilder(myConnectionString);
myConnectionStringBuilder.TransportType = TransportType.Amqp;
string amqpConnectionString = myConnectionStringBuilder.ToString();
var queueClient = QueueClient.CreateFromConnectionString(amqpConnectionString, "myQueue");

BrokeredMessage message = new BrokeredMessage("Hello AMQP");
Console.WriteLine("Sending message {0}...", message.MessageId);
queueClient.Send(message);

var receivedMessage = queueClient.Receive();
Console.WriteLine("Received message: {0}", receivedMessage.GetBody<string>());
receivedMessage.Complete();
```

## <a name="use-amqp-with-partitioned-topics"></a>Verwenden von AMQP mit partitionierten Themen

Themen ähneln Warteschlangen, aber Themen können eine Kopie der dieselbe Nachricht an mehrere *Abonnements*weiterleiten. In einem Thema Herausgeber Senden von Nachrichten mit dem Thema und Nutzer von Abonnements empfangen. Im Szenario POS-System Inventory veröffentlichen umsteigebahnhöfe Daten mit dem Thema fort. Das Inventory Managementsystem empfängt dann Nachrichten aus einem Abonnement. Darüber hinaus kann ein Überwachung System dieselbe Nachricht aus einem anderen Abonnement empfangen. Weitere Details Dienstbus Themen und Abonnements finden Sie unter [Erstellen von Applications, die Dienstbus Themen und Abonnements verwenden](service-bus-create-topics-subscriptions.md). 

Wie bei Warteschlangen, vergrößern partitionierte Themen finden Sie weitere Verfügbarkeit, Zuverlässigkeit und Durchsatz für Applikationen, wie den folgenden Themen und ihre Abonnements über mehrere Nachrichtenmakler und messaging Stores formatiert sind. 

### <a name="create-partitioned-topics"></a>Erstellen von partitionierten Themen

Sie können einem partitionierten Thema mit [Azure klassischen Portal][] und dem Dienst Bus SDK erstellen. Um ein Thema partitionierten zu erstellen, legen Sie die Eigenschaft [EnablePartitioning](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicdescription.enablepartitioning.aspx) **Wahr** in der [TopicDescription](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicdescription.aspx) -Instanz aus. Mit dem folgende Code veranschaulicht, wie ein partitionierten Thema mit dem Dienst Bus SDK zu erstellen.
    
```
// Create partitioned topic
var nm = NamespaceManager.CreateFromConnectionString(myConnectionString);
var topicDescription = new TopicDescription("myTopic");
topicDescription.EnablePartitioning = true;
nm.CreateTopic(topicDescription);

var subscriptionDescription = new SubscriptionDescription("myTopic", "mySubscription");
nm.CreateSubscription(subscriptionDescription);
```

### <a name="send-and-receive-messages-using-amqp"></a>Senden und Empfangen von Nachrichten mithilfe von AMQP

Sie können Nachrichten zu senden und Empfangen von Nachrichten aus einem partitionierten Thema Abonnement mit AMQP als ein Protokoll mithilfe der Eigenschaft [TransportType](https://msdn.microsoft.com/library/azure/microsoft.servicebus.servicebusconnectionstringbuilder.transporttype.aspx) zu [TransportType.Amqp](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.transporttype.aspx), wie im folgenden Code dargestellt.  

```
// Sending and receiving messages to a topic and from a subscription
var myConnectionStringBuilder = new ServiceBusConnectionStringBuilder(myConnectionString);
myConnectionStringBuilder.TransportType = TransportType.Amqp;
string amqpConnectionString = myConnectionStringBuilder.ToString();
    
var topicClient = TopicClient.CreateFromConnectionString(amqpConnectionString, "myTopic");
BrokeredMessage message = new BrokeredMessage("Hello AMQP");
Console.WriteLine("Sending message {0}...", message.MessageId);
topicClient.Send(message);
    
var subcriptionClient = SubscriptionClient.CreateFromConnectionString(amqpConnectionString, "myTopic", "mySubscription");
var receivedMessage = subcriptionClient.Receive();
Console.WriteLine("Received message: {0}", receivedMessage.GetBody<string>());
receivedMessage.Complete();
```

## <a name="next-steps"></a>Nächste Schritte

Finden Sie weitere Informationen zu partitionierten messaging Personen als auch AMQP folgende zusätzliche Informationen ein.

*    [Partitionierte messaging Einheiten](service-bus-partitioning.md)
*    [OASIS erweiterte Message Queuing-Protokoll (AMQP), Version 1.0](http://docs.oasis-open.org/amqp/core/v1.0/os/amqp-core-complete-v1.0-os.pdf)
*    [AMQP 1.0-Unterstützung in Dienstbus](service-bus-amqp-overview.md)
*    [AMQP 1.0 Azure-Dienstbus und Ereignis Hubs Protocol-Leitfaden](service-bus-amqp-protocol-guide.md)
*    [Verwenden Sie die API Java Message Service (JMS) mit Dienstbus und AMQP 1.0](service-bus-java-how-to-use-jms-api-amqp.md)
*    [Verwenden von AMQP 1.0 mit der Bus .NET API](service-bus-dotnet-advanced-message-queuing.md)

[Azure klassischen-portal]: http://manage.windowsazure.com

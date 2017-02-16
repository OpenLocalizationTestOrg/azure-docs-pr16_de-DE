<properties 
    pageTitle="Automatische Weiterleitung Dienstbus messaging Einheiten | Microsoft Azure"
    description="So Kette eine Warteschlange oder das Abonnement weiterleiten oder Thema."
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
    ms.date="09/29/2016"
    ms.author="sethm" />

# <a name="chaining-service-bus-entities-with-auto-forwarding"></a>Verketten Dienstbus Einheiten mit automatische Weiterleitung

Das Feature für die *Automatische Weiterleitung* ermöglicht es Ihnen, eine Warteschlange oder das Abonnement weiterleiten oder Thema, das Bestandteil der gleichen Namespace ist zu verketten. Wenn die automatische Weiterleitung aktiviert ist, wird Dienstbus automatisch entfernt Nachrichten, die in der ersten Warteschlange oder Abonnements (Quelle) platziert werden und in der zweiten Warteschlange oder Thema (Ziel) verschoben. Beachten Sie, dass es immer noch senden eine Nachricht auf die Zielentität direkt möglich ist. Darüber hinaus ist es nicht möglich, eine Unterwarteschlange, beispielsweise eine Warteschlange für unzustellbare Nachrichten weiterleiten oder Thema zu verketten.

## <a name="using-auto-forwarding"></a>Verwenden die automatische Weiterleitung

Sie können die automatische Weiterleitung aktivieren, durch Festlegen der [QueueDescription.ForwardTo][] oder [SubscriptionDescription.ForwardTo][] Eigenschaften für die Objekte [QueueDescription][] oder [SubscriptionDescription][] für die Quelle, wie im folgenden Beispiel gezeigt.

```
SubscriptionDescription srcSubscription = new SubscriptionDescription (srcTopic, srcSubscriptionName);
srcSubscription.ForwardTo = destTopic;
namespaceManager.CreateSubscription(srcSubscription));
```

Die Zielentität muss gleichzeitig vorhanden sein, die die Quellentität erstellt wird. Wenn die Zielentität nicht vorhanden ist, gibt Dienstbus eine Ausnahme, wenn Sie aufgefordert werden, um die Quellentität erstellen.

Automatische Weiterleitung können Sie um ein einzelnes Thema zu skalieren. Dienstbus wird die [Anzahl von Abonnements zu einem bestimmten Thema](service-bus-quotas.md) zu 2.000 beschränkt. Durch das Erstellen der zweiten Ebene Themen können Sie zusätzliche Abonnements aufnehmen können. Beachten Sie, dass auch wenn Sie nicht durch die Einschränkung Dienstbus die Anzahl der Abonnements gebunden sind, Hinzufügen einer zweiten Ebene Themen den Gesamtdurchsatz von Thema verbessert werden kann.

![Automatische Weiterleitung Szenario][0]

Sie können auch die automatische Weiterleitung verwenden, um Nachrichtenabsender von den Empfängern entkoppeln. Angenommen, Sie verfügen über ein ERP-System, der drei Module besteht: Order Processing, Inventory Management und Customer Beziehungen Management. Jedes dieser Module generiert Nachrichten, die in einem entsprechenden Thema Warteschlange sind. Alice und Bob sind Vertriebsmitarbeiter, die in allen Nachrichten interessiert sind, die an ihre Kunden beziehen. Erstellen Sie zum Empfangen von Nachrichten Alice und Bob eine Persönliche Warteschlange und ein Abonnement auf jedes der ERP-Themen, die alle Nachrichten automatisch an ihre Warteschlange weiterleiten aus.

![Automatische Weiterleitung Szenario][1]

Wenn Alice auf Urlaub, ihre Persönliche Warteschlange statt der ERP-Thema geht, voll ist. In diesem Szenario da ein Vertriebsmitarbeiter keine Nachrichten erhalten hat erreichen keines der ERP-Themen jemals Kontingent.

## <a name="auto-forwarding-considerations"></a>Automatische Weiterleitung Aspekte

Wenn die Zielentität weist viele Nachrichten Gesamtzeit und überschreitet das Kontingent oder die Zielentität deaktiviert ist, hinzugefügt, die Quellentität Nachrichten seine [unzustellbare](service-bus-dead-letter-queues.md) bis in das Ziel Platz ist (oder die Entität wieder aktiviert ist). Die Nachrichten werden weiterhin in der Warteschlange unzustellbare live, damit Sie müssen explizit erhalten und aus der Warteschlange unzustellbare zu verarbeiten.

Wenn der einzelne Themen, um eine zusammengesetzte Thema mit vielen Abonnements erhalten miteinander zu verketten, empfiehlt es sich, dass Sie eine überschaubare Anzahl an Abonnements auf der ersten Ebene Thema und viele Abonnements auf der zweiten Ebene Themen haben. Beispielsweise ermöglicht ein Thema ersten Ebene mit 20 Abonnements, jede mit einem zweiten Ebene Thema mit 200 Abonnements, verkettet höhere Durchsatz als erste Ebene Themas mit 200 Abonnements, jede mit einem zweiten Ebene Thema mit 20 Abonnements verkettet.

Dienstbus Abrechnung von einem Vorgang für jede weitergeleiteten Nachricht. Beispielsweise ist das Senden einer Nachricht mit einem Thema mit 20 Abonnements, jede von ihnen so konfiguriert, dass die automatische Weiterleitung von Nachrichten weiterleiten oder Thema als 21 Vorgänge in Rechnung gestellt, wenn alle Abonnements der ersten Ebene eine Kopie der Nachricht erhalten.

Wenn Sie ein Abonnement erstellen, die mit einer anderen Warteschlange oder Thema verkettet ist, muss der Ersteller des Abonnements **Verwalten** der Quell- und die Zielentität berechtigt. Senden von Nachrichten mit dem Thema Quelle erfordert nur Berechtigungen auf dem Thema Quelle **zu senden** .

## <a name="next-steps"></a>Nächste Schritte

Ausführliche Informationen zu automatische Weiterleitung finden Sie unter den folgenden Themen:

- [SubscriptionDescription.ForwardTo][]
- [QueueDescription][]
- [SubscriptionDescription][]

Weitere Informationen zu Dienstbus leistungsverbesserungen finden Sie unter [partitionierte messaging Einheiten][].

  [QueueDescription.ForwardTo]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.forwardto.aspx
  [SubscriptionDescription.ForwardTo]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptiondescription.forwardto.aspx
  [QueueDescription]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.aspx
  [SubscriptionDescription]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptiondescription.aspx
  [0]: ./media/service-bus-auto-forwarding/IC628631.gif
  [1]: ./media/service-bus-auto-forwarding/IC628632.gif
  [Partitionierte messaging Einheiten]: service-bus-partitioning.md
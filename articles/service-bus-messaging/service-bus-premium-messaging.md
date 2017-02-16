<properties
    pageTitle="Service Bus Premium und Standard Messaging Preise Ebenen Übersicht | Microsoft Azure"
    description="Service Bus Premium und Standard Messaging"
    services="service-bus"
    documentationCenter=".net"
    authors="djrosanova"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/02/2016"
    ms.author="darosa;sethm"/>

# <a name="service-bus-premium-and-standard-messaging-tiers"></a>Service Bus Premium und Standard messaging Ebenen 

Dienstbus messaging, einschließlich Einheiten wie Warteschlangen messaging und Themen, kombiniert Enterprise messaging-Funktionen, die mit Rich Semantik bei Cloud veröffentlichen-abonnieren. Dienstbus messaging wird als Backbone Kommunikation für viele anspruchsvolle Cloudlösungen verwendet.

Die Ebene *Premium* Dienstbus Messaging Adressen allgemeine Kundenanfragen um skalieren, Leistung und Verfügbarkeit für wichtiger Applikationen. Obwohl das Feature fast identisch sind, werden diese zwei Ebenen Dienstbus Messaging verschiedene dienen soll.

In der folgenden Tabelle sind einige Unterschiede auf hoher Ebene hervorgehoben.

| Premium                               | Standard                       |
|---------------------------------------|--------------------------------|
| Hoher Durchsatz                       | Variable Durchsatz            |
| Vorhersehbar Leistung               | Variable Wartezeit               |
| Vorhersehbar Preise                   | Quellenbesteuerung Variable Preise |
| Nach oben und unten Arbeitsbelastung skalieren | N/V                            |
| Nachricht Größe > 256KB                  | Nachrichtengröße beträgt 256KB          |

**Service Bus Premium Messaging** bietet Ressource Isolation auf der Ebene CPU- und an, sodass die einzelnen Kunden Arbeitsbelastung isoliert ausgeführt wird. Diese Ressource Container wird als eine *Einheit messaging*bezeichnet. Jeder Namespace Premium mindestens eine messaging Einheit zugeordnet sind. Sie können 1, 2 oder 4 per Einheiten für jeden Dienst Bus Premium Namespace kaufen. Eine einzelne Arbeitsbelastung oder juristische kann mehrere messaging Einheiten umfassen, und die Anzahl der Einheiten messaging kann bei wird, geändert werden, zwar Abrechnung im 24-Stunden- oder tägliche Zins Gebühren ist. Das Ergebnis ist vorhersehbar und wiederholt Leistung für Ihre Dienstbus-basierte Lösung.

Nicht nur diese Leistung ist, vorhersehbar und zur Verfügung, aber es sieht auch schneller. Service Bus Premium messaging basiert auf der Speicher-Engine in [Azure Ereignis Hubs](https://azure.microsoft.com/services/event-hubs/)eingeführt werden. Mit messaging Premium ist Höchstwert Leistung viel schneller als mit der standardmäßigen Ebene.

## <a name="premium-messaging-technical-differences"></a>Premium Messaging technischen Unterschiede

Es folgen einige Unterschiede zwischen Premium und Standard messaging Ebenen.

### <a name="partitioned-queues-and-topics"></a>Partitionierte Warteschlangen und Themen

Partitionierte Warteschlangen und Themen werden in Premium messaging unterstützt, aber funktionieren nicht die gleiche Weise wie in der Standard- und Basic Ebenen Dienstbus Messaging. Premium messaging nicht SQL als Datenspeicher verwenden und nicht mehr verfügt über mögliche Ressource Mitbewerber zugeordnete mit einer freigegebenen Plattform integriert. Daher ist es nicht erforderlich, die Partitionierung. Darüber hinaus wurde die Partitionsanzahl von 16 Partitionen in Standard messaging auf 2 in Premium geändert. Gibt es zwei Partitionen Verfügbarkeit sichergestellt und ist eine weitere entsprechende Zahl für die Premium Runtime-Umgebung. Weitere Informationen über das Aufteilen finden Sie unter [partitionierte Warteschlangen und Themen](service-bus-partitioning.md).

### <a name="express-entities"></a>Express-Elemente

Da Messaging Premium in einer vollständig isoliert Runtime-Umgebung ausgeführt wird, werden die express Personen in Premium Namespaces nicht unterstützt. Weitere Informationen zu den express-Features finden Sie unter der Eigenschaft [Microsoft.ServiceBus.Messaging.QueueDescription.EnableExpress](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enableexpress.aspx) .

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Dienstbus messaging zu erhalten, finden Sie unter den folgenden Themen.

- [Einführung in Azure Service Bus Premium messaging (Blogbeitrag)](http://azure.microsoft.com/blog/introducing-azure-service-bus-premium-messaging/)
- [Einführung in Azure Service Bus Premium messaging (Channel9)](https://channel9.msdn.com/Blogs/Subscribe/Introducing-Azure-Service-Bus-Premium-Messaging)
- [Übersicht über Dienstbus messaging](service-bus-messaging-overview.md)
- [So verwenden Sie Servicebuswarteschlangen](service-bus-dotnet-get-started-with-queues.md)

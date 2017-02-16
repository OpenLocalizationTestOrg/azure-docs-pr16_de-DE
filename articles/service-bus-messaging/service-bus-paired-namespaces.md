<properties 
    pageTitle="Dienstbus gepaart Namespaces | Microsoft Azure"
    description="Implementierung für Namespace und Kosten"
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
    ms.date="10/04/2016"
    ms.author="sethm" />

# <a name="paired-namespace-implementation-details-and-cost-implications"></a>Bei abhängigen Namespace Implementierungsdetails und Auswirkungen Kosten

Die [PairNamespaceAsync][] -Methode, die mit einer Instanz [SendAvailabilityPairedNamespaceOptions][] führt sichtbare Aufgaben in Ihrem Auftrag an. Da beim Verwenden des Features Aspekte Kosten sind, ist es sinnvoll, diese Aufgaben zu verstehen, damit das Verhalten erwarten, wenn dies geschieht. Die API aktiviert wird, in Ihrem Auftrag folgende automatische Verhalten:

-   Erstellung von Warteschlangen Rückstand.
-   Die Erstellung eines Objekts, das mit Warteschlangen oder Themen kommuniziert [NachrichtSender][] .
-   Wenn eine messaging Entität nicht mehr verfügbar ist, pingen sendet Nachrichten, um die Entität in dem Versuch, zu erkennen, wenn die Entität wieder verfügbar ist.
-   Optional eine Reihe von "Nachricht Pumpen" erstellt, die Nachrichten aus den Warteschlangen Rückstand an die primären Warteschlangen verschieben.
-   Koordiniert schließende/fehlgeschlagene der primären und sekundären [MessagingFactory][] Instanzen.

Auf hoher Ebene, funktioniert das Feature wie folgt: Wenn die primäre Entität fehlerfrei ist, keine Änderungen Verhalten auftreten. Wenn die [FailoverInterval][] Dauer verstrichen ist, und der primären Entität sieht nicht erfolgreich sendet nach einer dauerhaftes [MessagingException][] oder eine [TimeoutException][], tritt folgendes Verhalten auf:

1.  Senden Sie Vorgänge mit der primären Entität deaktiviert sind und das System fragt die primäre Entität bis Pings erfolgreich übermittelt werden können.

2.  Eine zufällige Rückstand Warteschlange ausgewählt ist.

3.  [BrokeredMessage][] Objekte werden an die ausgewählten Rückstand Warteschlange weitergeleitet.

1.  Wenn ein Sendevorgang an die ausgewählten Rückstand Warteschlange fehlschlägt, wird dieser Warteschlange aus die Drehung gezogen wird, und eine neue Warteschlange aktiviert ist. Klicken Sie auf die Instanz [MessagingFactory][] alle Absender des Fehlers erfahren.

Die folgenden Zahlen sind die Sequenz dargestellt. Zunächst sendet der Absender Nachrichten.

![Für Namespaces][0]

Bei einem Ausfall zum Senden an die primäre Warteschlange beginnt der Absender senden von Nachrichten an eine Warteschlange zufällig ausgewählten Rückstand. Gleichzeitiges, einen Ping-Vorgang wird gestartet.

![Für Namespaces][1]

An diesem Punkt die Nachrichten werden weiterhin in der Warteschlange sekundäre und nicht in die primäre Warteschlange geliefert wurden. Nachdem Sie die primäre Warteschlange erneut fehlerfrei ist, sollte mindestens einen Prozess der Syphon ausgeführt werden. Die Syphon übermittelt die Nachricht aus den verschiedenen Rückstand Warteschlangen für die richtigen Ziel Personen (Warteschlangen und Themen).

![Für Namespaces][2]

Im weiteren Verlauf dieses Themas wird erläutert, die bestimmte Details über die Funktionsweise dieser Textstellen.

## <a name="creation-of-backlog-queues"></a>Erstellung von Warteschlangen Rückstand

Das [SendAvailabilityPairedNamespaceOptions][] -Objekt, das an die Methode [PairNamespaceAsync][] übergeben zeigt an, die Anzahl der Rückstand Warteschlangen, die Sie verwenden möchten. Jede Warteschlange Rückstand wird dann explizit mit den folgenden Eigenschaften erstellt (alle anderen Werte werden auf die Standardeinstellungen [QueueDescription][] festgelegt) festlegen:

| Pfad                                   | [primären Namespace] / X-Servicebus-Transfer / [index], [Index] [0, BacklogQueueCount) eines Werts ist |
|----------------------------------------|------------------------------------------------------------------------------------------------------|
| MaxSizeInMegabytes                     | 5120                                                                                                 |
| MaxDeliveryCount                       | int-Wert. MaxValue                                                                                         |
| DefaultMessageTimeToLive               | TimeSpan.MaxValue                                                                                    |
| AutoDeleteOnIdle                       | TimeSpan.MaxValue                                                                                    |
| LockDuration                           | 1 minute                                                                                             |
| EnableDeadLetteringOnMessageExpiration | WAHR                                                                                                 |
| EnableBatchedOperations                | WAHR                                                                                                 |

Die erste Rückstand Warteschlange, die für Namespace **Contoso** benannt wird beispielsweise erstellt `contoso/x-servicebus-transfer/0`.

Wenn die beim Erstellen, überprüft der Code zuerst um festzustellen, ob eine solche Warteschlange vorhanden ist. Wenn die Warteschlange nicht vorhanden ist, wird die Warteschlange erstellt. Der Code hingegen nicht "extra" Rückstand Warteschlangen bereinigen. Insbesondere, wenn die Anwendung mit der primären Namespace **Contoso** anfordert fünf Iterationsbacklog Warteschlangen jedoch eine Rückstand Warteschlange durch den Pfad `contoso/x-servicebus-transfer/7` vorhanden ist, die zusätzliche Rückstand Warteschlange ist immer noch vorhanden, jedoch nicht verwendet. Das System ermöglicht explizit zusätzliche Rückstand Warteschlangen vorhanden sein, die nicht verwendet werden sollen. Als Besitzer des Namespace sind Sie alle Rückstand nicht verwendete/unerwünschte Warteschlangen bereinigen verantwortlich. Der Grund für diese Entscheidung ist, dass Dienstbus nicht bekannt ist, welche Zwecke für alle Warteschlangen in Ihrem Namespace vorhanden sind. Darüber hinaus, wenn eine Warteschlange mit dem angegebenen Namen vorhanden ist, aber der angenommenen [QueueDescription][]nicht erfüllt, sind Ihre Gründe eigene für das Standardverhalten ändern. Keine für Änderungen an den Rückstand Warteschlangen vom Code Garantien werden. Stellen Sie sicher, Ihre Änderungen erwägen.

## <a name="custom-messagesender"></a>Benutzerdefinierte NachrichtSender

Beim Senden, navigieren alle Nachrichten in einem internen [NachrichtSender][] -Objekt, das sich normalerweise verhält, wenn alles funktioniert, und leitet an die Warteschlangen Rückstand unterbrochene Punkte"." Startet eingeht, einen Fehler dauerhaftes, einen Zeitgeber ein. Nach einem [TimeSpan][] -Zeitraum aus der [FailoverInterval][] Eigenschaftswert während, den keine erfolgreichen Nachrichten gesendet werden, ist das Failover nicht nachlässt. Zu diesem Zeitpunkt geschieht Folgendes für jede Entität ein:

- Eine Aufgabe Ping führt jeder [PingPrimaryInterval][] zu überprüfen, ob die Entität verfügbar ist. Nachdem dieser Vorgang erfolgreich ist, startet alle Clientcode, der die Entität sofort verwendet auf den primären Namespace neue Nachrichten senden.

- Zukünftige Anfragen zu senden, dass die [BrokeredMessage][] geändert werden gesendet, in der Warteschlange Rückstand ungefähr Entität von einem beliebigen anderen Absendern führt. Die Änderung entfernt einige Eigenschaften aus dem [BrokeredMessage][] -Objekt und speichert diese an anderer Stelle. Die folgenden Eigenschaften werden deaktiviert und unter einen neuen Alias, gleicht Dienstbus und das SDK zum Verarbeiten von Nachrichten einheitlich hinzugefügt:

| Alter Eigenschaftsname       | Neue Eigenschaftsname |
|-------------------------|-------------------|
| SessionId               | X-ms-sessionid    |
| TimeToLive              | X-ms-timetolive   |
| ScheduledEnqueueTimeUtc | X-ms-Pfad         |

Der ursprünglichen Zielpfad wird auch in der Nachricht als eine Eigenschaft namens X-ms-Path gespeichert. Dies ermöglicht Nachrichten für viele Personen in einer einzelnen Rückstand Warteschlange verwendet werden können. Die Eigenschaften werden wieder von der Syphon übersetzt.

Das benutzerdefinierte [NachrichtSender][] Objekt kann Probleme auftreten, wenn Nachrichten, die Beschränkung 256 KB Ansatz und Failover ist nicht nachlässt. Das benutzerdefinierte [NachrichtSender][] Objekt speichert Nachrichten für alle Warteschlangen und Themen zusammen in der Warteschlange Rückstand. Dieses Objekt gemischt Nachrichten von vielen Primärfarben zusammen innerhalb der Rückstand Warteschlangen sind. Behandeln von Lastenausgleich zwischen einer Vielzahl von Clients, die nicht miteinander kennen wählt das SDK zufällig ein Rückstand Warteschlange für jede [QueueClient][] oder [TopicClient][] im Code zu erstellen.

## <a name="pings"></a>Pings

Eine ist eine leere [BrokeredMessage][] mit seiner Eigenschaft [ContentType][] Anwendung/vnd.ms-Servicebus-Ping und [TimeToLive][] Wert 1 Sekunde. Dieser Ping hat eine spezielle Eigenschaft Dienstbus: der Server bietet einen Ping nie, wenn alle Anrufer ein [BrokeredMessage][]anfordert. Daher müssen Sie nie erfahren, wie Sie empfangen und diese Nachrichten ignorieren. Jede Entität (eindeutige Warteschlange oder Thema) pro [MessagingFactory][] Instanz pro Client wird an gesendet werden, wenn sie nicht mehr verfügbar sein gelten. Standardmäßig geschieht dies einmal pro Minute. Ping-Nachrichten werden als reguläre Dienstbus Nachrichten, und können dazu führen, Gebühren für Bandbreite und Nachrichten. Sobald die Clients zu erkennen, dass das System verfügbar ist, beenden die Nachrichten aus.

## <a name="the-syphon"></a>Die syphon

Mindestens ein Programm in der Anwendung sollte aktiv die Syphon ausgeführt werden. Die Syphon führt eine lange-Umfrage erhalten, die 15 Minuten dauert. Wenn alle Personen verfügbar sind, und Sie 10 Rückstand Warteschlangen haben, ruft die Anwendung, die die Syphon hostet der Receive-Methode 40 Mal pro Stunde, 960 Uhrzeiten pro Tag und 28800 Zeiten in 30 Tage an. Wenn die Syphon aktiv Nachrichten von Rückstand in die primäre Warteschlange verschoben wird, auftritt jede Nachricht die folgenden Gebühren (standard für Nachrichtengröße und Bandbreite anfallen in allen Phasen):

1.  Senden Sie an den Rückstand.

2.  Von den Rückstand erhalten.

3.  Mit dem primären senden.

4.  Von der primären erhalten.

## <a name="closefault-behavior"></a>Schließen/Fehlerstrukturanalyse-Verhalten

Innerhalb einer Anwendung, die die Syphon, nachdem die primäre oder sekundäre [MessagingFactory][] Fehlern durch hostet oder wird geschlossen, ohne deren Partner erkennt auch die fehlerhaft/geschlossen und der Syphon dieser Status, die Syphon Handlungen aus. Wenn die anderen [MessagingFactory][] nicht innerhalb von 5 Sekunden geschlossen ist, wird die Syphon der noch geöffnet [MessagingFactory][]fehl.

## <a name="next-steps"></a>Nächste Schritte

Ausführliche Informationen zu Dienstbus asynchrones messaging finden Sie unter [asynchrone messaging Muster und hohe Verfügbarkeit][] . 

  [PairNamespaceAsync]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.pairnamespaceasync.aspx
  [SendAvailabilityPairedNamespaceOptions]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions.aspx
  [NachrichtSender]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx
  [MessagingFactory]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx
  [FailoverInterval]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.pairednamespaceoptions.failoverinterval.aspx
  [MessagingException]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingexception.aspx
  [TimeoutException]: https://msdn.microsoft.com/library/azure/system.timeoutexception.aspx
  [BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
  [QueueDescription]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.aspx
  [TimeSpan]: https://msdn.microsoft.com/library/azure/system.timespan.aspx
  [PingPrimaryInterval]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions.pingprimaryinterval.aspx
  [QueueClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx
  [TopicClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx
  [ContentType]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.contenttype.aspx
  [TimeToLive]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx
  [Asynchrone messaging Muster und hohe Verfügbarkeit]: service-bus-async-messaging.md
  [0]: ./media/service-bus-paired-namespaces/IC673405.png
  [1]: ./media/service-bus-paired-namespaces/IC673406.png
  [2]: ./media/service-bus-paired-namespaces/IC673407.png

<properties 
    pageTitle="Dienstbus asynchrones messaging | Microsoft Azure"
    description="Beschreibung der Dienstbus asynchrones messaging."
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

# <a name="asynchronous-messaging-patterns-and-high-availability"></a>Asynchrone messaging Muster und hohe Verfügbarkeit

Asynchrone Nachrichtenübermittlung kann in einer Vielzahl von verschiedene Arten implementiert werden. Mit Warteschlangen, Themen und Abonnements unterstützt Azure-Dienstbus einer über eine Store und Weiterleiten Verfahren. Im normalen Betrieb (synchron) Senden von Nachrichten an Warteschlangen und Themen und Empfangen von Nachrichten aus Warteschlangen und Abonnements. Programme, die Sie schreiben, abhängig von diesen Einheiten nicht immer verfügbar sein. Wenn die Integrität Entität, aufgrund einer Vielzahl von Umständen ändert, benötigen Sie eine Möglichkeit eine reduzierten Videofunktionen Entität bereitstellen, die meisten Bedürfnisse erfüllen können.

Applikationen verwenden in der Regel asynchrone messaging Mustern, um eine Reihe von Kommunikationsszenarios ermöglichen. Sie können Applications erstellen, in denen Clients Nachrichten senden können mit Services auch, wenn der Dienst nicht ausgeführt wird. Für Applikationen die Benutzeroberfläche, die Kommunikation einer Warteschlange unerwartet helfen können die laden können, indem Sie einen Ort Puffer Kommunikation abgleichen. Schließlich können Sie eine einfache, aber effektive Lastenausgleich zum Verteilen von Nachrichten auf mehreren Computern erhalten.

Um die Verfügbarkeit von eine der folgenden Personen zu gewährleisten, erwägen Sie eine Reihe von verschiedenen Methoden, die diese Elemente nicht verfügbar für ein dauerhaftes messaging-System angezeigt werden können. Im Allgemeinen sehen wir die Entität auf Anwendungen verfügbar, die wir in verschiedenen folgendermaßen schreiben:

- Senden von Nachrichten nicht möglich.

- Keine Nachrichten empfangen.

- Kann nicht zum Verwalten von Personen (erstellen, abrufen, aktualisieren oder Löschen von Personen).

- Wenden Sie sich an den Dienst können nicht genutzt werden.

Für jede der folgenden Fehlern vorhanden sein, bei der anderen Modi, mit die eine Anwendung auszuführende Arbeit an einem gewissen Grad reduzierte Videofunktionen fortsetzen können. Beispielsweise ein System, das Senden von Nachrichten kann jedoch nicht empfangen kann weiterhin Aufträge von Kunden erhalten aber die Bestellungen kann nicht verarbeitet werden. In diesem Thema wird erläutert, potenzielle Probleme, die auftreten können, und wie diese Probleme verringert werden. Dienstbus weist eine Anzahl von Problembehebungen, die Sie in Suchbegriffen müssen eingeführt, und in diesem Thema werden auch die Regeln für die Verwendung von diese Teilnahme Problembehebungen.

## <a name="reliability-in-service-bus"></a>Zuverlässigkeit in Dienstbus

Es gibt mehrere Methoden, um Probleme mit der Nachricht und Entität verarbeitet, und es werden Richtlinien, die die geeignete Verwendung des betreffenden Problembehebungen. Um die Richtlinien zu verstehen, müssen Sie zuerst wissen, was in Dienstbus ausgeführt werden kann. Alle diese Probleme häufig aufgrund des Entwurfs Azure Systeme kurzlebig sind. Auf hoher Ebene die verschiedenen Ursachen nicht verfügbar wie folgt angezeigt:

-   Begrenzungsebene von einem externen System Dienstbus abhängt. Tritt auf, begrenzungsebene von Interaktionen mit Speicher und Berechnen von Ressourcen.

-   Problem eines Systems Dienstbus abhängt. Beispielsweise kann ein angegebenen Teil Speicher Probleme auftreten.

-   Fehler beim Dienst-auf einzelne Subsystem. In diesem Fall ein Knoten berechnen bekomme inkonsistent und muss neu starten Kopien bewirken, dass alle Personen, die sie für den Lastenausgleich an andere Knoten dient. Dies kann wiederum kurzer langsam Nachricht Verarbeitung verursachen.

-   Fehler beim Dienst-innerhalb einer Azure Datencenters. Dies ist eine "Fehler" bei dem das System für viele Minuten oder ein paar Stunden nicht erreichbar ist.

> [AZURE.NOTE] Der Ausdrucks- **Speicher** kann sowohl Azure-Speicher und SQL Azure Mittelwert.

Dienstbus enthält eine Anzahl von Problembehebungen für diese Probleme. Den folgenden Abschnitten werden die einzelnen Probleme und ihre jeweiligen Problembehebungen.

### <a name="throttling"></a>Begrenzungsebene

Mit Dienstbus ermöglicht begrenzungsebene gemeinsame Nachricht Zins Management aus. Jede einzelne Dienstbus Knoten-Website beinhaltet viele Personen. Jede dieser Personen macht erfordert System in Bezug auf CPU, Arbeitsspeicher, Speicherplatz und andere Aspekte. Wenn Sie eine der folgenden Aspekte erkennt Verwendung, die Schwellenwerte überschreitet, Dienstbus können eine bestimmte Anforderung verweigern. Anrufer empfängt eine [ServerBusyException][] und nach 10 Sekunden wiederholt.

Als eine Lösung an muss der Code des Fehlers lesen und alle Wiederholungsversuche der Nachricht für mindestens 10 Sekunden anzuhalten. Da der Fehler verteilt auf der Anwendung Kunden auftreten kann, wird davon ausgegangen, dass jedes Stück die Logik Wiederholungsversuche unabhängig voneinander ausgeführt wird. Der Code kann die Wahrscheinlichkeit wird durch das aktivieren, klicken Sie auf eine Warteschlange oder ein Thema Partitionierung gedrosselt verringern.

### <a name="issue-for-an-azure-dependency"></a>Problem für eine Azure Abhängigkeit

Andere unsichere Komponenten in Azure können gelegentlich Dienstprobleme haben. Beispielsweise, wenn ein System, das Dienstbus aktualisiert wird, kann dieses System vorübergehend reduzierte Funktionen auftreten. Diese Art von Problemen umgehen, Dienstbus regelmäßig untersucht und Problembehebungen implementiert. Diese Problembehebungen Auswirkungen Seite führen Sie angezeigt. Behandeln von vorübergehende Probleme mit Speicher implementiert beispielsweise Dienstbus Nachricht Sendevorgänge konsistent arbeiten können. Aufgrund der Art der Behebung von kann eine gesendete Nachricht in der Warteschlange betroffenen oder das Abonnement angezeigt und bereit, der für einen Empfangsvorgang bis zu 15 Minuten dauern. Im Allgemeinen werden die meisten Einheiten dieses Problem nicht. Jedoch wird die Anzahl von Elementen in Dienstbus in Azure angegeben, diese Reduzierung manchmal für eine kleine Gruppe von Kunden Dienstbus benötigt.

### <a name="service-bus-failure-on-a-single-subsystem"></a>Fehler bei der Dienstbus auf einem einzelnen subsystem

Bei jeder Anwendung können Umstände dazu führen, dass eine interne Komponente der Dienstbus inkonsistent wird. Wenn dies Dienstbus Programm erkennt, sammelt es Daten aus der Anwendung zur Unterstützung bei der Diagnose von den Vorkommnissen. Nachdem die Daten gesammelt werden, wird die Anwendung versucht, einen konsistenten Zustand wiederherzustellen neu gestartet. Dieser Vorgang relativ schnell wird, und die Ergebnisse in einer Entität für bis zu ein paar Minuten nicht verfügbar sein, obwohl typische unten Zeiten angezeigte sind viel kürzer.

In diesen Fällen wird die Clientanwendung eine Ausnahme [System.TimeoutException][] oder [MessagingException][] generiert. Dienstbus enthält eine Lösung für dieses Problem in Form von automatisierten Client "Wiederholen" Logik an. Sobald die Periode "Wiederholen" ausgelastet ist und die Nachricht wurde nicht übermittelt, können Sie die von anderen Features wie [für Namespaces][]durchsuchen. Für Namespaces haben andere Vorsichtsmaßnahmen, die in diesem Artikel erläutert werden.

### <a name="failure-of-service-bus-within-an-azure-datacenter"></a>Ausfall der Dienstbus innerhalb einer Azure Datencenters

Die am häufigsten mögliche Ursache für einen Fehler in einer Azure Datacenter ist ein Fehler beim Upgrade Bereitstellung von einem abhängige System oder Dienst genutzten. Wie im weiteren Verlauf die Plattform weist weist die Wahrscheinlichkeit, dass diese Art von Fehler verringert. Ein Fehler Datacenter kann auch Gründen auftreten, die Folgendes enthalten:

-   Plan für Elektrik Ausfall (Power Lieferung und Generieren von Power verschwinden).
-   Konnektivität (Internet Seitenumbruch zwischen den Clients und Azure).

In beiden Fällen verursacht einem Ausfall natürliche oder Menschen vom. Wenn Sie diese Einschränkung umgehen, und stellen Sie sicher, dass Sie Nachrichten immer noch senden können, können Sie [für Namespaces][] Aktivieren von Nachrichten an einem zweiten Ort gesendet werden, während der primäre Speicherort erneut fehlerfrei erfolgt. Weitere Informationen finden Sie unter [bewährte Methoden für Hohlkörperelementen Applikationen gegen Dienstbus Ausfall und Schäden][].

## <a name="paired-namespaces"></a>Für namespaces

Das Feature [für Namespaces][] Szenarien unterstützt, in der eine Dienstbus Entität oder Bereitstellung in einem Data Center ist nicht mehr verfügbar. Während dieses selten Ereignisses müssen verteilte Systeme weiterhin ungünstigsten Fall verarbeitet vorbereitet werden. In der Regel geschieht dieses Ereignis, da einige abhängt Dienstbus Element einer kurzfristig dieses Problem auftritt. Wenn bei einem Ausfall Verfügbarkeit der Anwendung beibehalten möchten, können Dienstbus Benutzer zwei separate Namespaces vorzugsweise in separaten Data Center, Sie um deren messaging Personen zu hosten. Der Rest der in diesem Abschnitt wird die folgende Terminologie verwendet:

-   Primäre Namespace: der Namespace mit der Ihrer Anwendung interagiert, für das Senden und Empfangen von Vorgängen.

-   Sekundäre Namespace: der Namespace, der als Sicherung für die primäre Namespace fungiert. Die Logik interagiert nicht mit diesem Namespace.

-   Failover Intervall: die Zeitdauer zum normale Fehlern annehmen, bevor Sie die Anwendung aus dem primären Namespace auf den sekundären Namespace wechselt.

Bei abhängigen Namespaces Support *Verfügbarkeit senden*aus. Send Verfügbarkeit die Möglichkeit zum Senden von Nachrichten behält. Zum Senden Verfügbarkeit verwenden zu können, muss eine Anwendung die folgenden Anforderungen erfüllen:

1.  Nachrichten werden nur aus dem primären Namespace empfangen.

2.  Nachrichten an eine bestimmte Warteschlange gesendet oder Thema möglicherweise falschen Reihenfolge eintreffen.

3.  Wenn die Anwendung Sitzungen verwendet, möglicherweise Nachrichten innerhalb einer Sitzung angeordnet werden. Hierbei handelt es sich um einen Umbruch von Sitzungen normale Funktionalität. Dies bedeutet, dass die Anwendung Sitzungen logisch Gruppe Nachrichten verwendet. Sitzungsstatus ist nur auf dem primären Namespace beibehalten.

4.  Nachrichten innerhalb einer Sitzung möglicherweise angeordnet werden. Hierbei handelt es sich um einen Umbruch von Sitzungen normale Funktionalität. Dies bedeutet, dass die Anwendung Sitzungen logisch Gruppe Nachrichten verwendet.

5.  Sitzungsstatus ist nur auf dem primären Namespace beibehalten.

6.  Die primäre Warteschlange kann online und akzeptiert Nachrichten vor die sekundäre Warteschlange aller Nachrichten in der Warteschlange primären stellt starten.

Den folgenden Abschnitten werden die APIs, wie die APIs implementiert werden, und Beispielcode, dass zeigt verwendet die Funktion. Beachten Sie, dass es Abrechnung Auswirkungen dieses Feature zugeordnet sind.

### <a name="the-messagingfactorypairnamespaceasync-api"></a>Die MessagingFactory.PairNamespaceAsync-API

Das Feature für Namespaces umfasst die [PairNamespaceAsync][] Methode für die [Microsoft.ServiceBus.Messaging.MessagingFactory][] -Klasse an:

```
public Task PairNamespaceAsync(PairedNamespaceOptions options);
```

Wenn der Vorgang abgeschlossen ist, das Namespace bündeln ist abgeschlossen und bereit sind, die für alle [MessageReceiver][], [QueueClient][], wirken sich auch oder [TopicClient][] mit der [MessagingFactory][] -Instanz erstellt. [Microsoft.ServiceBus.Messaging.PairedNamespaceOptions][] ist die Basis-Klasse für die verschiedenen Typen von Verbindung, die mit einem Objekt [MessagingFactory][] verfügbar sind. Die einzige abgeleitete Klasse ist derzeit eine benannte [SendAvailabilityPairedNamespaceOptions][], die die Anforderungen der senden-Verfügbarkeit implementiert. [SendAvailabilityPairedNamespaceOptions][] verfügt über eine Reihe von Konstruktoren, die aufeinander aufbauen. Betrachten den Konstruktor aus, mit der meisten Parameter, können Sie das Verhalten der anderen Konstruktoren verstehen.

```
public SendAvailabilityPairedNamespaceOptions(
    NamespaceManager secondaryNamespaceManager,
    MessagingFactory messagingFactory,
    int backlogQueueCount,
    TimeSpan failoverInterval,
    bool enableSyphon)
```

Diese Parameter haben folgende Bedeutung:

-   *SecondaryNamespaceManager*: keine initialisierte [NamespaceManager][] -Instanz für den sekundären Namespace, die die [PairNamespaceAsync][] Methode zum Einrichten des sekundären Namespaces verwenden können. Der Namespace-Manager wird verwendet, um eine Liste der Warteschlangen im Namespace erhalten und um sicherzustellen, dass die erforderlichen Rückstand Warteschlangen vorhanden sind. Wenn die Warteschlangen nicht vorhanden sind, werden sie erstellt. [NamespaceManager][] erfordert die Möglichkeit, ein Token mit den Anspruch **Verwalten** zu erstellen.

-   *MessagingFactory*: die [MessagingFactory][] -Instanz für den sekundären Namespace. Das Objekt [MessagingFactory][] wird zum Senden und, wenn die Eigenschaft [EnableSyphon][] auf **true**festgelegt ist, empfangen Nachrichten aus den Warteschlangen Rückstand verwendet.

-   *BacklogQueueCount*: die Anzahl der Rückstand Warteschlangen zu erstellen. Dieser Wert muss mindestens 1 sein. Beim Senden von Nachrichten an den Rückstand ist eine der folgenden Warteschlangen zufällig ausgewählt. Wenn Sie den Wert 1 festgelegt, kann nur eine Warteschlange jemals verwendet werden. Wenn dies der Fall ist, und die Warteschlange ein Rückstand Fehler generiert, der Client auf eine andere Rückstand Warteschlange versuchen Sie es nicht möglich ist, und kann ein Fehler auftreten, um Ihre Nachricht zu senden. Es empfiehlt sich, diesen Wert auf einige größere Wert und Standardwert den Wert 10 festlegen. Sie können dies eine höhere oder niedrigere Wert je nachdem wie viele Daten ändern, die pro Tag Ihrer Anwendung sendet. Jede Warteschlange Rückstand kann bis zu 5 GB Nachrichten enthalten.

-   *FailoverInterval*: die Zeitdauer, während die akzeptiert Sie Fehler auf dem primären Namespace, bevor Sie eine einzelne Entität auf den sekundären Namespace Umstellung. Failovers auftreten, auf der Grundlage Entität-von-Entität. Elemente in einem einzelnen Namespace live häufig in verschiedenen Knoten innerhalb Dienstbus. Ein Fehler in einer Entität impliziert einen Fehler in einer anderen nicht. Sie können diesen Wert auf [System.TimeSpan.Zero][] Failover auf den sekundären unmittelbar nach der ersten, dauerhaftes Fehler festlegen. Fehler, die den Zeitgeber Failover auslösen, sind alle [MessagingException][] , in denen die Eigenschaft [IsTransient][] falsch ist, oder eine [System.TimeoutException][]. Andere Ausnahmen, wie z. B. [UnauthorizedAccessException][] bewirken keine Failover, da diese darauf hinzuweisen, dass der Client nicht richtig konfiguriert ist. Ein [ServerBusyException][] bewirkt keine Failover, da die korrekte Vorgehensweise besteht darin, warten Sie 10 Sekunden, senden Sie die Nachricht erneut.

-   *EnableSyphon*: Gibt an, dass bestimmte Kombination auch Nachrichten aus dem sekundären Namespace wieder in die primäre Namespace syphon sollte. Im Allgemeinen sollten Applications, die Nachrichten senden diesen Wert auf **false**festgelegt. Programme, die Nachrichten empfangen sollten diesen Wert auf **true**festgelegt. Der Grund dafür ist, dass häufig, weniger Nachricht Empfänger als Nachrichtenabsender stehen. Je nach der Anzahl der Empfänger können Sie eine einzelne Anwendungsinstanz Syphon Aufgaben verarbeitet haben. Mit der Anzahl der Empfänger wirkt sich auf den für jede Warteschlange Rückstand.

Zum Verwenden des Codes, erstellen Sie eine Instanz der primären [MessagingFactory][] , eine sekundäre [MessagingFactory][] Instanz, eine sekundäre [NamespaceManager][] Instanz und eine [SendAvailabilityPairedNamespaceOptions][] -Instanz aus. Der Anruf möglich so einfach wie folgt aus:

```
SendAvailabilityPairedNamespaceOptions sendAvailabilityOptions = new SendAvailabilityPairedNamespaceOptions(secondaryNamespaceManager, secondary);
primary.PairNamespaceAsync(sendAvailabilityOptions).Wait();
```

Klicken Sie nach Abschluss die Aufgabe, die von der Methode [PairNamespaceAsync][] zurückgegeben wird alles von und zur Verwendung bereit festgelegt. Bevor Sie die Aufgabe zurückgegeben wird, Sie möglicherweise nicht alle Hintergrund erforderlichen Schritte für die Verbindung zum rechts Arbeiten abgeschlossen haben. Daher sollten Sie nicht gestartet werden Nachrichten senden, bis die Aufgabe zurückgibt. Wenn Fehler aufgetreten sind, wie z. B. fehlerhafte Anmeldeinformationen oder Fehlern beim Erstellen der Warteschlangen Rückstand werden diese Ausnahmen ausgelöst werden, sobald die Aufgabe abgeschlossen ist. Sobald die Aufgabe zurückgegeben wird, stellen Sie sicher, dass die Warteschlangen gefunden oder durch Untersuchen der Eigenschaft [BacklogQueueCount][] Instanz der [SendAvailabilityPairedNamespaceOptions][] erstellt wurden. Für den vorherigen Code wird dieser Vorgang wie folgt angezeigt:

```
if (sendAvailabilityOptions.BacklogQueueCount < 1)
{
    // Handle case where no queues were created.
}
```

## <a name="next-steps"></a>Nächste Schritte

Jetzt, da Sie die Grundlagen des asynchrones messaging in Dienstbus beherrschen, lesen Sie weitere Details [für Namespaces][].

  [ServerBusyException]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.serverbusyexception.aspx
  [System.TimeoutException]: https://msdn.microsoft.com/library/system.timeoutexception.aspx
  [MessagingException]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingexception.aspx
  [Bewährte Methoden für Applikationen gegen Dienstbus Ausfall und Schäden isoliert]: service-bus-outages-disasters.md
  [Microsoft.ServiceBus.Messaging.MessagingFactory]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx
  [MessageReceiver]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx
  [QueueClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx
  [TopicClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx
  [Microsoft.ServiceBus.Messaging.PairedNamespaceOptions]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.pairednamespaceoptions.aspx
  [MessagingFactory]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx
  [SendAvailabilityPairedNamespaceOptions]:https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions.aspx
  [NamespaceManager]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx
  [PairNamespaceAsync]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.pairnamespaceasync.aspx
  [EnableSyphon]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions.enablesyphon.aspx
  [System.TimeSpan.Zero]: https://msdn.microsoft.com/library/azure/system.timespan.zero.aspx
  [IsTransient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingexception.istransient.aspx
  [UnauthorizedAccessException]: https://msdn.microsoft.com/library/azure/system.unauthorizedaccessexception.aspx
  [BacklogQueueCount]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions.backlogqueuecount.aspx
  [für namespaces]: service-bus-paired-namespaces.md
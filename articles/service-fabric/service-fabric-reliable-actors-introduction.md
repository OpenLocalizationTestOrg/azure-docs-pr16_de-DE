<properties
   pageTitle="Service-Fabric zuverlässigen Akteuren Übersicht | Microsoft Azure"
   description="Einführung in das Modell Dienst Fabric zuverlässigen Akteuren Programmierung."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/19/2016"
   ms.author="vturecek"/>

# <a name="introduction-to-service-fabric-reliable-actors"></a>Einführung in Service Fabric zuverlässigen Akteuren

Zuverlässige Akteuren ist ein Dienst Fabric Anwendungsframework auf Grundlage des [Virtuellen Akteur](http://research.microsoft.com/en-us/projects/orleans/) . Die zuverlässigen Akteuren-API bietet ein Programmierung Single threaded Modell auf Skalierbarkeit und Zuverlässigkeit Garantien Dienst Fabric erstellt.

## <a name="what-are-actors"></a>Was sind Akteuren?
Ein Akteur ist eine Dateneinheit isoliert, unabhängig berechnen und Bundesstaat mit Single-threaded Datenausführungsverhinderung. Das [Akteur Muster](https://en.wikipedia.org/wiki/Actor_model) ist eine Berechnung Modell für gleichzeitige oder verteilte Systeme, in denen eine große Anzahl von diesen Akteuren gleichzeitig und unabhängig voneinander ausgeführt werden kann. Akteuren miteinander kommunizieren können, und sie können weitere Akteuren erstellen.

### <a name="when-to-use-reliable-actors"></a>Verwenden von zuverlässigen Akteuren

Dienst Fabric zuverlässigen Akteuren ist eine Implementierung des Musters Akteur Entwurf. Wie bei einer beliebigen Software Entwurfsmuster, passt die Entscheidung, ob mit einem bestimmten Muster basierend auf eine Software entwerfen, unabhängig davon, ob Problem gemacht ist das Muster.

Zwar der Akteur entwerfen Muster können sich gut werden in eine Reihe von verteilten Systemen Probleme und Szenarien, die eine genaue Aspekte der Nebenbedingungen die Muster und implementieren es Framework gemacht werden muss. Betrachten Sie als allgemeinen Leitfaden für die des Akteur Musters zu Ihrem Problem oder Ihrem Szenario Wenn modellieren aus:

 - Ihr Problem Speicherplatz umfasst eine große Anzahl (Tausende oder mehr) klein, unabhängig und isoliert Einheiten von Zustand und Logik.

 - Arbeiten mit Single-threaded Objekte, die keine wesentlichen Eingaben aus externen Komponenten, einschließlich der Status für eine Reihe von Akteuren Abfragen erfordern werden soll.

 - Ihre Akteur Instanzen blockieren nicht Anrufer mit unvorhergesehene verspätungen, indem Sie e/a-Vorgänge.

## <a name="actors-in-service-fabric"></a>Akteuren Dienst Struktur

Dienst Struktur, Akteuren in zuverlässigen Akteuren Framework implementiert werden: eine Akteur-Muster-basierten Anwendungsframework auf [Zuverlässigen Dienst Fabric-Dienste](service-fabric-reliable-services-introduction.md)erstellt. Jeder zuverlässigen Akteur-Dienst, die Sie schreiben ist tatsächlich ein partitionierten, dynamische zuverlässigen Dienst.

Jeder Akteur wird als eine Instanz eines Typs Akteur, identisch mit der Art definiert, die ein Objekt .NET eine Instanz eines Typs .NET ist. Angenommen, möglicherweise ein Akteurtyp, der die Funktionalität der einem Taschenrechner implementiert und viele Akteuren dieses Typs, die auf verschiedenen Knoten in einem Cluster verteilt werden könnte. Jeder solche Akteur wird durch ein Akteur-ID eindeutig identifiziert.

### <a name="actor-lifetime"></a>Lebensdauer Akteur

Dienst Fabric Akteuren sind virtuelle, was bedeutet, dass ihre Lebensdauer nicht in die entsprechende in-Memory-Darstellung verknüpft ist. Sie müssen daher nicht explizit erstellt oder gelöscht werden zu können. Die zuverlässigen Akteuren Laufzeit wird automatisch eine Akteur das erste Mal eine für die betreffende Akteur-ID eingeht aktiviert. Wenn ein Akteur für einen Zeitraum nicht verwendet wird, Garbage-sammelt die zuverlässigen Akteuren Laufzeit in-Memory-Objekt. Auch aufrechterhalten Kenntnisse über den Akteur das Vorhandensein es sollte später erneut aktiviert werden muss. Weitere Informationen hierzu finden Sie unter [Lebenszyklus und Garbage Akteur-Websitesammlung](service-fabric-reliable-actors-lifecycle.md).

Diese virtuelle Akteur Lebensdauer Abstraktion führt einige Vorsichtsmaßnahmen als Ergebnis virtuelle Akteur Modell und sogar die Implementierung zuverlässigen Akteuren abweicht zu Zeiten aus diesem Modell speichert.

 - Ein Akteur (bewirken, dass ein Objekt Akteur erstellt werden) wird automatisch aktiviert, beim ersten eine Nachricht gesendet wird, um seine Akteur-ID an. Nach einer gewissen Zeitspanne ist das Objekt Akteur Garbagecollection. In der Zukunft bewirkt, dass die Akteur-ID erneut zu verwenden, ein neue Akteur Objekt erstellt werden. Zustand des Akteur outlives Lebensdauer des Objekts beim Speichern in der Status-Manager.
 
 - Aufrufen einer beliebigen Akteur-Methode für eine Akteur-ID wird die Akteur aktiviert. Daher müssen Akteur-Typen ihres Konstruktors implizit von der Laufzeit aufgerufen. Deshalb übergeben nicht Client-Code Parameter an den Akteurtyp, obwohl Parameter vom Dienst selbst an den Akteur des Konstruktor übergeben werden können. Das Ergebnis ist, dass Akteuren erstellt werden möglicherweise in einem teilweisen Initialisierung Zustand durch die Zeit, die diese andere Methoden aufgerufen werden, wenn der Akteur Initialisierungsparameter vom Client erfordert. Es gibt keine einzelnen Einstiegspunkt für die Aktivierung ein Akteur vom Client aus.

 - Obwohl zuverlässigen Akteuren implizit Akteur-Objekte erstellen. Sie haben die Möglichkeit, ein Akteur und Zustand explizit zu löschen. 

### <a name="distribution-and-failover"></a>Verteilung und failover

Skalierbarkeit und Zuverlässigkeit zum Bereitstellen Dienst Fabric Akteuren über das Cluster verteilt und migriert diese automatisch vom ausgefallenen Knoten zu fehlerfrei nach Bedarf. Dies ist eine Abstraktion über eine [partitionierte, Stateful zuverlässigen Dienst](./service-fabric-concepts-partitioning.md)an. Verteilung, Skalierbarkeit, Zuverlässigkeit und automatisches Failover werden alle aufgrund der Fakultät bereitgestellt, die innerhalb einer dynamische zuverlässigen Dienst mit der Bezeichnung *Akteur-Dienst*Akteuren ausgeführt werden. 

Akteuren partitionsübergreifend des Diensts Akteur verteilt werden, und diese Partitionen über die Knoten in einem Cluster Service Fabric. Jede Servicepartition enthält eine Reihe von Akteuren dar. Dienst Fabric verwaltet Verteilung und Failover der Dienst Partitionen. 

Beispielsweise würde ein Akteur-Dienst mit neun Partitionen auf drei Knoten mithilfe der Akteur Partition Standardposition bereitgestellt, die diese verteilt werden:

![Zuverlässigen Akteuren Verteilung][2]

Der Akteur Framework verwaltet Partition des Farbschemas und Schlüssel Bereich-Einstellungen für Sie. Dies vereinfacht einige Auswahlmöglichkeiten aber auch führt einige Aspekte:

 - Zuverlässigen Services ermöglicht Ihnen, wählen Sie eine Partitionierungsschema, Key Bereich (Wenn Sie einen Bereich Partitionsschema verwenden zu können), und partitionieren zählen. Zuverlässige Akteuren beschränkt ist, auf den Bereich Partitionsschema (uniform Int64 Schema) und erfordert, dass Sie den vollständigen Int64 Bereich Key verwenden.
 
 - Standardmäßig werden Akteuren zufällig in Partitionen in gleichmäßige Verteilung resultierender platziert. 
 
 - Da zufällig Akteuren sollte davon ausgegangen werden, dass Akteur Vorgänge immer Netzwerkkommunikation, einschließlich der Serialisierung und Deserialisierung von Methode Anruf Daten anfallen Wartezeit und Verwaltungsaufwand erforderlich ist.
 
 - In erweiterten Szenarien ist es möglich, Steuerelement Akteur Partition Platzierung mithilfe von Int64 Akteur-IDs, die auf bestimmte zuordnen. Allerdings können eine ungleiche Verteilung der Akteuren partitionsübergreifend führen. 

Weitere Informationen wie Akteur Services formatiert sind finden Sie in der [Partitionierung Konzepte für Akteuren dar](service-fabric-reliable-actors-platform.md#service-fabric-partition-concepts-for-actors). 

### <a name="actor-communication"></a>Akteur-Kommunikation
Akteur Interaktionen werden in einer Schnittstelle definiert, die freigegeben werden, indem Sie den Akteur, der die Schnittstelle implementiert und den Client, der einen Proxy einem Akteur über dieselbe Schnittstelle erreicht wird. Da diese Schnittstelle zum asynchronen Aufrufen von Methoden Akteur verwendet wird, muss jeder Methode für die Schnittstelle Aufgabe zurückgeben.

Methodenaufrufe und ihrer Antworten schließlich Ergebnis im Netzwerk-Anfragen über den Cluster, also die Argumente und die Ergebnistypen die Aufgaben, die sie zurückgeben muss von der Plattform serialisierbar. Vor allem müssen diese [Daten Vertrag serialisierbar](service-fabric-reliable-actors-notes-on-actor-type-serialization.md)sein.

#### <a name="the-actor-proxy"></a>Der Akteur-proxy
Die zuverlässigen Akteuren-Client-API bietet Kommunikation zwischen einer Instanz Akteur und einem Akteur-Client. Für die Kommunikation mit einem Akteur erstellt ein Client ein Akteur Proxy-Objekt, das Akteur-Schnittstelle implementiert. Der Client interagiert mit der Akteur durch Proxy-Objekts auf Methoden aufrufen. Der Akteur Proxy kann für die Client-zu-Akteur und Akteur-Akteur-Kommunikation verwendet werden. 

```csharp
// Create a randomly distributed actor ID
ActorId actorId = ActorId.CreateRandom();

// This only creates a proxy object, it does not activate an actor or invoke any methods yet.
IMyActor myActor = ActorProxy.Create<IMyActor>(actorId, new Uri("fabric:/MyApp/MyActorService"));

// This will invoke a method on the actor. If an actor with the given ID does not exist, it will be activated by this method call.
await myActor.DoWorkAsync();
```

Beachten Sie, dass die zwei Angaben zum Erstellen der Akteur Proxy-Objekts verwendet die Akteur-ID und dem Namen der Anwendung sind. Die Akteur-ID identifiziert eindeutig den Akteur, während der Name der Anwendung der [Fabric Service-Anwendung](service-fabric-reliable-actors-platform.md#service-fabric-application-model-concepts-for-actors) identifiziert, wo der Akteur bereitgestellt wird.

Die `ActorProxy` Klasse clientseitig führt die notwendigen Auflösung den Akteur-ID suchen und öffnen einen Channel für die Kommunikation mit. Die `ActorProxy` auch Wiederholungsversuche zum Suchen des Akteurs in Fällen der Kommunikationsfehler und Failovers. Nachrichtenübermittlung daher weist folgende Merkmale auf:

 - Nachrichtenübermittlung ist die optimale Leistung.
 - Akteuren möglicherweise doppelte Nachrichten vom selben Client angezeigt.

### <a name="concurrency"></a>Parallelität

Die Laufzeit zuverlässigen Akteuren bietet ein einfaches aktivieren-basierten Zugriff-Modell für den Zugriff auf Akteur Methoden an. Dies bedeutet, dass nicht mehr als ein Thread innerhalb eines Objekts Akteur Code aktiv, jederzeit sein kann. Access-basierten aktivieren vereinfacht gleichzeitige Systeme wie es keine Notwendigkeit Verfahren für den Zugriff auf Daten besteht. Dies bedeutet auch, dass Systeme mit für die Art der einzelnen Thread Access Besonderheiten der einzelnen Instanzen Akteur entworfen werden müssen.

 - Eine Instanz der einzelnen Akteur kann nicht mehr als eine Anforderung nacheinander verarbeitet werden. Eine Instanz Akteur kann Durchsatzengpass verursachen, wenn es gleichzeitige Anforderungen verarbeitet erwartet wird. 
 - Akteuren können auf untereinander Deadlock auftreten, ist es eine Anforderung kreisförmige zwischen zwei Akteuren während einer externen eine von der Akteuren gleichzeitig angefordert wird. Die Laufzeit Akteur automatisch abläuft auf Akteur Anrufe und eine Ausnahme verwenden, um dem Anrufer Deadlocksituationen unterbrechen.

![Zuverlässigen Akteuren Kommunikation][3]

#### <a name="turn-based-access"></a>Access-basierten aktivieren

Eine aktivieren besteht aus abgeschlossen Ausführung einer Methode Akteur als Antwort auf eine Anforderung von anderen Akteuren oder Kunden oder die vollständige Ausführung eines Rückrufs [Zeitmessung/Erinnerung](service-fabric-reliable-actors-timers-reminders.md) . Obwohl diese Methoden und Rückrufe asynchrone sind, wird die Laufzeit Akteuren diese nicht verschachteln. Eine aktivieren muss vollständig abgeschlossen sein, bevor eine neue aktivieren zulässig ist. Zählung ausnehmen ein Akteur-Abschreibung oder Zeitmessung/Erinnerung Rückruf, der gerade ausgeführt wird, muss vollständig abgeschlossen ist, bevor Sie einen neuen Anruf an eine Methode oder Rückruf zulässig ist. Eine Methode oder Rückruf wird als abgeschlossen haben, wenn die Ausführung von der Methode zurückgegeben wurde oder Rückruf und die Aufgabe, die von der Methode oder Rückruf zurückgegeben wurde abgeschlossen. Es ist extrem, dass die Parallelität aktivieren-basierten auch über die unterschiedlichen Methoden, Zeitgeber und Rückrufe beachtet wird.

Die Laufzeit Akteuren erzwingt aktivieren-basierten Parallelität durch Anfordern einer Sperre pro Akteur am Anfang einer aktivieren und Aufheben der Sperre am Ende der aktivieren. Auf diese Weise wird Parallelität aktivieren basierend auf Basis pro Akteur und nicht über Akteuren erzwungen. Akteur Methoden und Zeitgeber/Erinnerung Rückrufe können im Auftrag von anderen Akteuren gleichzeitig ausführen.

Das folgende Beispiel veranschaulicht die oben genannten Konzepte. Erwägen Sie einen Akteurtyp, der zwei asynchrone Methoden (z. B. *Method1* und *Methode2*), einen Zeitgeber und eine Erinnerung implementiert. Im nachstehenden Diagramm zeigt ein Beispiel für eine Zeitachse für die Ausführung der folgenden Methoden und Rückrufe für zwei Akteuren (*ActorId1* und *ActorId2*), die zu diesem Akteurtyp gehören.

![Zuverlässigen Akteuren Laufzeit aktivieren-basierten Parallelität und Zugriff][1]

Dieses Diagramm umfasst die folgenden Konventionen:

- Jede vertikale Linie zeigt die logischen Ablauf der Ausführung einer Methode oder einen Rückruf für einen bestimmten Akteur.
- Klicken Sie auf jede vertikale Linie markiert Ereignisse eintreten chronologisch, mit neueren Ereignisse unter ältere.
- Unterschiedliche Farben dienen Zeitachsen, anderen Akteuren entspricht.
- Hervorhebung wird verwendet, um die Dauer anzugeben, für die die pro Akteur Sperre für eine Methode oder Rückruf verwendet wird.

Einige wichtige Punkte zu beachten:

- Während *Method1* für *ActorId2* in der Antwort auf Client Anforderung *xyz789*und ausgeführt wird Eintreffen einer anderen Clientanforderung (*abc123*), die auch *Method1* vom *ActorId2*ausgeführt werden muss. Die zweite Ausführung der *Method1* beginnt jedoch nicht, bis die vorherige Ausführung abgeschlossen ist. Auf ähnliche Weise eine Erinnerung registriert von *ActorId2* wird ausgelöst, während *Method1* Reaktion auf Client Anforderung *xyz789*ausgeführt wird. Nur, wenn beide Ausführungen der *Method1* abgeschlossen sind, wird der Erinnerung Rückruf ausgeführt. All dies wird durch Aktivieren-basierten Parallelität für *ActorId2*umgesetzt.
- Aktivieren-basierten Parallelität wird auf ähnliche Weise auch für *ActorId1*, erzwungen, wie die Ausführung von *Method1* *Methode2*und Timer-Rückruf für *ActorId1* weiterhin seriell veranschaulicht.
- Ausführung von *Method1* im Auftrag *ActorId1* überlappt mit der Ausführung im Auftrag *ActorId2*. Dies liegt daran aktivieren-basierten Parallelität nur innerhalb einer Akteur und nicht über Akteuren erzwungen wird.
- In einigen der Ausführungen Methode/Rückruf der `Task` zurückgegebene die Methode/Rückruf abgeschlossen ist, nach Beendigung der Methode. In einige andere die `Task` wurde bereits abgeschlossen bis zu dem Zeitpunkt der Methode-Rückruf gibt. In beiden Fällen die Sperre pro Akteur erst sowohl der Methode/Rückruf gibt freigegeben ist und die `Task` endet.

#### <a name="reentrancy"></a>Reentranz

Die Laufzeit Akteuren ermöglicht Reentranz standardmäßig an. Dies bedeutet, dass, wenn Ruft eine Methode zum Akteur *Akteur A* eine Methode auf *Akteur B*, wodurch wiederum eine andere Methode auf *Akteur A*, aufgerufen, die Methode ausgeführt werden darf. Dies ist, da sie Teil des gleichen logischen Anruf-Kette Kontexts ist. Alle Zeitgeber und Erinnerung Anrufe beginnen Sie mit dem neuen logischen Anruf Kontext. [Zuverlässigen Akteuren Reentranz](service-fabric-reliable-actors-reentrancy.md) Weitere Informationen hierzu finden Sie unter.

#### <a name="scope-of-concurrency-guarantees"></a>Umfang der Parallelität Garantien

Die Laufzeit Akteuren bietet diese Garantien Parallelität in Situationen, in dem sie die Aufrufen dieser Methoden gesteuert. Beispielsweise stellt diese Garantien für die Methodenaufrufe, die als Antwort auf eine Besprechungsanfrage Client fertig sind, sowie das Timer und Erinnerung Rückruf. Wenn der Akteur-Code direkt diese Methoden außerhalb der von der Runtime Akteuren bereitgestellten Verfahren Ruft, kann keine die Laufzeit jedoch alle Garantien Parallelität bereitstellen. Wenn die Methode im Zusammenhang mit einigen Vorgang, die nicht mit dem Vorgang die Akteur Methoden zurückgegebene verknüpft ist aufgerufen wird, kann nicht die Laufzeit beispielsweise Parallelität Garantien bereitstellen. Wenn die Methode aus einem Thread, die der Akteur eigenständig erstellt wird aufgerufen wird, kann nicht die Laufzeit auch Parallelität Garantien bieten. Daher sollten zum Ausführen von Hintergrundvorgängen Akteuren verwenden [Akteur Zeitgeber und Akteur Erinnerungen](service-fabric-reliable-actors-timers-reminders.md) , die Parallelität aktivieren-basierten berücksichtigen.

## <a name="next-steps"></a>Nächste Schritte
 - [Erste Schritte mit zuverlässigen Akteuren](service-fabric-reliable-actors-get-started.md)
 - [Verwendung von zuverlässigen Akteuren die Dienst Fabric-Plattform](service-fabric-reliable-actors-platform.md)
 - [Akteur Zustandsmanagement](service-fabric-reliable-actors-state-management.md)
 - [Lebenszyklus und Garbage Akteur-Websitesammlung](service-fabric-reliable-actors-lifecycle.md)
 - [Akteur Zeitgeber und Erinnerungen](service-fabric-reliable-actors-timers-reminders.md)
 - [Akteur Ereignisse](service-fabric-reliable-actors-events.md)
 - [Akteur Reentranz](service-fabric-reliable-actors-reentrancy.md)
 - [Akteur Polymorphismus und objektorientierte entwurfmustern](service-fabric-reliable-actors-polymorphism.md)
 - [Akteur-Diagnose und Überwachen der Leistung](service-fabric-reliable-actors-diagnostics.md)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-introduction/concurrency.png
[2]: ./media/service-fabric-reliable-actors-introduction/distribution.png
[3]: ./media/service-fabric-reliable-actors-introduction/actor-communication.png

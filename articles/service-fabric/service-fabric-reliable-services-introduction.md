<properties
   pageTitle="Übersicht über die Programmierung Dienst Fabric zuverlässigen Service-Modell | Microsoft Azure"
   description="Erfahren Sie mehr über die Programmierung Service-Fabric zuverlässigen Service-Modell, und legen Sie los Schreiben Ihrer eigenen Dienste."
   services="Service-Fabric"
   documentationCenter=".net"
   authors="masnider"
   manager="timlt"
   editor="vturecek; mani-ramaswamy"/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="03/25/2016"
   ms.author="masnider;vturecek"/>

# <a name="reliable-services-overview"></a>Zuverlässigen Dienste (Übersicht)
Azure Service-Struktur vereinfacht das Schreiben und Verwalten von zustandslosen und dynamische zuverlässigen Diensten. Dieses Dokument wird zu sprechen:

- Die für statusfreie und dynamische Dienste zuverlässigen Services-Modell Programmierung.
- Die Auswahlmöglichkeiten, die Sie beim Verfassen einer zuverlässigen Service gemacht müssen.
- Einige Szenarien und Beispiele für zuverlässigen Services verwenden und wie sie geschrieben werden.

Zuverlässigen Services ist eines der Programmierung Modelle auf Fabric Service zur Verfügung. Weitere Informationen zum Modell zuverlässigen Akteuren-Programmierung finden Sie unter [Einführung in Service Fabric zuverlässigen Akteuren dar](service-fabric-reliable-actors-introduction.md).

Dienst Struktur ein Dienst besteht aus Code der Anwendung, Konfiguration und Status (optional).

Dienst Fabric verwaltet die Lebensdauer der Dienste bereitgestellt und Entwicklung bis zum Upgrade und löschen, über den [Dienst Fabric anwendungsverwaltung](service-fabric-deploy-remove-applications.md).

## <a name="what-are-reliable-services"></a>Was sind zuverlässigen Services?
Zuverlässigen Services bietet Ihnen eine einfache, leistungsfähige und auf oberster Ebene programming Modell, mit denen Sie express, was an Ihrer Anwendung wichtig ist. Bei der zuverlässigen Services-Modell Programmierung erhalten Sie folgende Aktionen ausführen:

- Für dynamische Dienste können mit das zuverlässigen Services-Modell Programmierung konsistent und zuverlässig speichern Ihrer Zustand rechts innerhalb des Diensts mithilfe einer zuverlässigen Websitesammlungen. Dies ist ein einfacher Satz von hochgradig verfügbar Sammlung von Klassen, die an eine Person vertraut sind, C#-Sammlungen verwendet hat. In der Vergangenheit erforderlich Services externe Systeme für die Verwaltung von zuverlässigen Zustand. Mit zuverlässigen Websitesammlungen können Sie Ihr Status neben Ihrer berechnen speichern, mit der gleichen hohen Verfügbarkeit und Zuverlässigkeit, Sie von hoch verfügbaren externen Stores erwarten haben und über die zusätzliche Wartezeit Innovationen, die Positionierung von der berechnen und der Status bereitstellen.

- Ein einfaches Modell für die Ausführung von eigenem Codes sieht wie programming Modelle auf Inanspruchnahme aus. Der Code enthält einen klar definierten Einstiegspunkt und einfach zu verwaltenden Lebenszyklus.

- Ein Kommunikationsmodell austauschbare. Verwenden Sie den Transport Ihrer Wahl, z. B. HTTP mit [Web-API](service-fabric-reliable-services-communication-webapi.md), WebSockets, benutzerdefinierte TCP Protokolle an. Zuverlässigen Services bieten einige großartige Out-of-Box-Optionen, die Sie verwenden können, oder Ihren eigenen bereitstellen.

## <a name="what-makes-reliable-services-different"></a>Was ist das zuverlässigen Services?
Zuverlässigen Services Dienst Struktur unterscheidet sich von Dienstleistungen, die Sie vor dem eingegeben haben. Fabric-Dienst bietet Zuverlässigkeit, Verfügbarkeit, Konsistenz und Skalierbarkeit.  

- **Zuverlässigkeit**– des Diensts verbleibt von auch in unzuverlässigen Umgebungen, wo Ihre Computer möglicherweise fehl oder Netzwerkproblemen Treffer.

- **Verfügbarkeit**– der Dienst werden, erreicht werden kann und reagiert. (Dies bedeutet nicht, dass Sie Dienste, die nicht gefunden oder von erreicht haben können außerhalb.)

- **Skalierbarkeit**– Services sind von bestimmter Hardware abgekoppelt, und sie können vergrößert oder verkleinert wird, je nach Bedarf durch das Hinzufügen oder Entfernen von Hardware oder virtuelle Ressourcen. Services sind einfach (besonders in der dynamische Groß-/Kleinschreibung) aufgeteilt, um sicherzustellen, dass unabhängige Teile des Diensts skalieren und unabhängig voneinander zu Fehlern reagieren können. Schließlich-Visualisierung Dienst Fabric Services lightweight werden durch gleicht Tausende von Diensten in einem einzigen Prozess bereitgestellt werden statt mit Anforderung oder gesamte OS Instanzen mit einer einzelnen Instanz von einer bestimmten Arbeitsbelastung gilt.

- **Konsistenz**– in diesem Dienst gespeicherten konsistent sein garantiert werden kann (gilt nur für dynamische Services – mehr dazu später)

## <a name="service-lifecycle"></a>Service-Lebenszyklus
Ob Ihr Dienst dynamische oder statusfrei ist, bieten zuverlässigen Services einen einfachen Lebenszyklus, mit dem Sie schnell schließen Sie den Code, und legen Sie los.  Es gibt nur eine oder zwei Methoden, die Sie benötigen, um Ihrem Dienst funktionsfähig machen implementiert wird.

- **CreateServiceReplicaListeners/CreateServiceInstanceListeners** – Dies ist die Stelle, an der der Dienst den Kommunikationsstapel definiert, die verwendet werden sollen. Der Kommunikationsstapel, wie z. B. [Web-API](service-fabric-reliable-services-communication-webapi.md), wird definiert die überwachenden Endpunkt oder die Endpunkte für den Dienst (wie Clients erreicht werden). Darüber hinaus werden wie die Nachrichten, die Ende von interagieren mit dem Rest des Codes Dienst angezeigt werden.

- **RunAsync** – Dies ist der Dienst seine Geschäftslogik ausgeführt wird. Das Kündigung Token, das bereitgestellt wird ist ein Signal für wann das Arbeit beendet werden soll. Beispielsweise, wenn Sie einen Dienst, der ständig Nachrichten aus einer zuverlässigen Warteschlange abrufen und verarbeitet werden muss verfügen, ist dies, wo das Arbeit passiert.

### <a name="service-startup"></a>Dienst starten

Die wichtigsten Ereignisse im Lebenszyklus von einer zuverlässigen Service sind:

1. Das Dienstobjekt (das Objekt, das von des statusfreien oder dynamische Diensts abgeleitet wird) wird erstellt.

2. Die `CreateServiceReplicaListeners` / `CreateServiceInstanceListeners` wird aufgerufen, zugewiesen Dienst Möglichkeit, einen oder mehrere Kommunikation Listener seiner Wahl zurückzukehren.
  - Beachten Sie, dass dies ist optional, obwohl die meisten Dienste einige Endpunkt direkt angezeigt werden.

3. Nachdem die Kommunikation Listener erstellt werden, wird es geöffnet.
  - Kommunikation haben, eine Methode namens `OpenAsync()`, die zu diesem Zeitpunkt aufgerufen wird und welche die überwachende Adresse für den Dienst gibt. Wenn Ihre zuverlässigen Dienst eines der integrierten ICommunicationListeners verwendet, wird dies für Sie durchgeführt.

4. Sobald die Kommunikation Zuhörer geöffnet ist, wird die `RunAsync()` Hauptfenster-Dienst wird aufgerufen.
  - Beachten Sie, dass `RunAsync()` ist optional. Der Dienst seine Arbeit direkt in die Antwort an den Benutzer nur Anrufe alle bedeutet, es ist nicht erforderlich, um zu implementieren `RunAsync()`.

### <a name="service-shutdown"></a>Dienst war(en)

Wenn Sie der Dienst beenden (, aktualisierten oder verschobene gelöscht werden sollen) wird die Anruf Reihenfolge gespiegelt: zuerst das Kündigung Token frei, `RunAsync()` abgebrochen wird; Klicken Sie dann `CloseAsync()` für die Kommunikation Listener aufgerufen wird.

Es gibt einige wichtige Hinweise zu war(en) für dynamische Dienste:

- Dienst Fabric werden nicht Heraufstufen einer anderen Kopie des Diensts zu primären Status bis `CloseAsync` und `RunAsync` zurückgegeben. Wenn Sie eine integrierte Kommunikation Zuhörer verwenden die `CloseAsync` Methode für Sie verarbeitet wird.

- Es gibt zwar keine Frist zur Rückgabe von diesen Methoden, Sie sofort verlieren die Möglichkeit zum Schreiben zuverlässigen Websitesammlungen und können daher nicht abgeschlossen wirkliche Arbeit. Es wird empfohlen, dass Sie so schnell wie möglich die Kündigung Anfrage eingeht zurückkehren.

## <a name="example-services"></a>Beispiel-Dienste
Kennen dieses Modell Programmierung, werfen wir einen Blick auf zwei verschiedenen Diensten zu sehen, wie diese Teile zusammen.

### <a name="stateless-reliable-services"></a>Zustandsloser zuverlässigen Dienste
Ein statusfreier Dienst ist eine Where Literal kein Zustand, die innerhalb des Diensts gepflegt vorhanden ist, oder der Status, die vorhanden ist vollständig freigegeben ist und setzt voraus, Synchronisierung, Replikation, Beibehaltung oder hohen Verfügbarkeit.

Angenommen Sie, einen Taschenrechner, der keine Arbeitsspeicher sowie empfängt alle Ausdrücke und Vorgänge auf einmal ausgeführt.

In diesem Fall können die RunAsync() des Diensts leer sein, da es ist kein Hintergrund Vorgang-Verarbeitung, die der Dienst führen muss. Beim Dienst Rechner erstellt wurde, wird ein ICommunicationListener (beispielsweise [Web-API](service-fabric-reliable-services-communication-webapi.md)) zurückgegeben, die von einem überwachenden Endpunkt auf einige Port wird geöffnet. Diesen überwachenden Endpunkt an die verschiedenen Methoden eingebunden werden (Beispiel: "Hinzufügen (n1, n2)"), die den Rechner öffentliche API definieren.

Wenn von einem Client aufgerufen wird, wird die entsprechende Methode aufgerufen und der Rechner-Dienst führt die Vorgänge auf den angegebenen Daten und gibt als Ergebnis zurück. Es wird nicht einen Zustand speichern.

Nicht Speichern von internen Status wird die mit diesem Beispiel Rechner sehr einfach. Die meisten Dienste wirklich statusfreie werden jedoch. In diesem Fall externalisieren sie ihren Status zu einem anderen Speicher. (Beispielsweise ist eine Web-app, die beruht auf planmäßigen Sitzungszustand in einem Datenträger zur Speicherung oder Cache nicht vollständig statusfreien.)

Ein gängiges Beispiel für wie zustandsloser Dienste verwendet Dienst ist, ist als Front-End, die die öffentlich zugänglichen API für eine Webanwendung verfügbar gemacht. Der Front-End-Dienst kommuniziert dann mit dynamische Dienste die Anforderung Benutzer durchführen. In diesem Fall sind Aufrufe von Clients an einen bekannten Anschluss, wie z. B. 80; weitergeleitet, wo der statusfreie Dienst wartet. Dieser statusfreie Dienst erhält den Anruf und bestimmt, ob der Anruf aus einer vertrauenswürdigen Party als auch als welche Dienst für bestimmt ist.  Klicken Sie dann der statusfreie Dienst leitet den Anruf an die richtige Teil der dynamische Dienst und wartet auf eine Antwort. Wenn der statusfreie Dienst eine Antwort erhält, Antworten sie wieder zur ursprünglichen Client.

### <a name="stateful-reliable-services"></a>Dynamische zuverlässigen Services
Ein dynamische Dienst ist eine, die einen Teil des Zustand beibehalten konsistent sowie präsentieren, damit der Dienst Funktion benötigen. Erwägen Sie einen Dienst, der ständig einen gleitenden Durchschnitt der einige basierend auf Updates empfangenen Wert berechnet. Dazu müssen sie den aktuellen Satz von eingehenden Anfragen Prozess als auch den aktuellen Mittelwert benötigten. Jedem Dienst, der Ruft, verarbeitet und speichert die Informationen in einem externen Speicher (beispielsweise einer Azure Blob oder Tabelle Store heute) ist dynamische. Das Programm speichert lediglich Zustand im externen Zustand Store.

Die meisten Dienste speichern heute ihren Status extern, da der externe Speicher ist, was für diesen Zustand Zuverlässigkeit, Verfügbarkeit, Skalierbarkeit und Konsistenz bietet. Dienst Struktur zwar ihren Zustand extern speichern dynamische Services müssen Sie; Dienst Fabric sorgt für diese Anforderungen für den Code und den Dienststatus.

Angenommen, Sie möchten einen Dienst zu schreiben, der Besprechungsanfragen für eine Reihe von Konvertierungen annimmt, die auf ein Bild, und das Bild, das die zu konvertierende muss ausgeführt werden müssen.  Für diesen Dienst, möchten sie einen Kommunikation befinden (angenommen Web-API), der einen Kommunikationsport angezeigt wird und wie Übermittlungen über eine API ermöglicht zurückgeben `ConvertImage(Image i, IList<Conversion> conversions)`. Der Dienst konnte in dieser API machen Sie die Informationen und speichern die Anfrage in einer zuverlässigen Warteschlange und einige Token dann an den Client zurückgeben, damit sie die Anfrage verfolgen konnte (da die Anforderungen einige Zeit in Anspruch nehmen können).

In diesem Dienst könnte komplexere RunAsync. Der Dienst konnte eine Schleife innerhalb der RunAsync, die abruft Anfragen außerhalb des IReliableQueue, führt die Konvertierungen aufgeführt, und speichert die Ergebnisse in IReliableDictionary, haben, damit bei der Client zurückkehrt, deren konvertierte Bilder aufrufen können. Um sicherzustellen, dass auch wenn etwas schlägt, das Bild fehl ist nicht verloren geht, dieser zuverlässigen Dienst ziehen Sie aus der Warteschlange, die bei Konvertierungen des ausführen und speichern Sie das Ergebnis in einer Transaktion möchten. In diesem Fall die Nachricht tatsächlich nur aus der Warteschlange entfernt, und die Ergebnisse werden im Ergebnis Wörterbuch gespeichert, wenn die Konvertierungen abgeschlossen sind. Wenn ein Fehler auftritt, in der Mitte (wie der Computer, auf den diese Instanz des Codes ausgeführt wird), bleibt die Anfrage in der Warteschlange warten auf erneut verarbeitet werden.

Eine ist zu diesem Dienst zu beachten, dass es wie ein normales .NET Dienst klingt. Der einzige Unterschied ist, dass die Datenstrukturen verwendet wird (IReliableQueue und IReliableDictionary) vom Dienst Fabric bereitgestellt werden und hochgradig zuverlässigen, verfügbar und konsistente daher vorgenommen wurden.

## <a name="when-to-use-reliable-services-apis"></a>Verwenden von zuverlässigen Services-APIs
Wenn Sie eine der folgenden Indexeigenschaften dienstanwendung kennzeichnen, sollten Sie zuverlässigen Services-APIs berücksichtigen:

- Sie müssen Anwendungsverhalten über mehrere Einheiten des Status (z. B. Orders und Order Line Items) bereitzustellen.

- Bundesstaat Ihrer Anwendung kann wird als zuverlässigen Wörterbücher und Warteschlangen natürlich behandelt.

- Der Zustand muss mit niedriger Wartezeit Access hochgradig verfügbar sein.

- Eine Anwendung muss die Parallelität oder Genauigkeit der durchgeführte Vorgänge über eine oder mehrere zuverlässigen Websitesammlungen zu steuern.

- Sie möchten die Kommunikation verwalten oder das Partitionierungsschema des Diensts steuern.

- Code benötigt eine Runtime-threaded-Umgebung.

- Die Anwendung muss dynamisch erstellen oder Löschen von zuverlässigen Wörterbücher oder Warteschlangen zur Laufzeit.

- Sie müssen programmgesteuert steuern bereitgestellter Dienst Fabric sichern und Wiederherstellen von Features für des Diensts Zustand *.

- Die Anwendung muss Änderungsverlauf für die Einheiten von Bundesstaat * beibehalten.

- Entwickeln oder nutzen dritte Partei entwickelt, benutzerdefinierte Zustand Anbieter * werden soll.

> [AZURE.NOTE] * Features zur allgemeinen Verfügbarkeit SDK zur Verfügung.


## <a name="next-steps"></a>Nächste Schritte
+ [Zuverlässigen Services Schnellstart](service-fabric-reliable-services-quick-start.md)
+ [Zuverlässigen Services erweiterte Verwendung](service-fabric-reliable-services-advanced-usage.md)
+ [Das Modell zuverlässigen Akteuren Programmierung](service-fabric-reliable-actors-introduction.md)

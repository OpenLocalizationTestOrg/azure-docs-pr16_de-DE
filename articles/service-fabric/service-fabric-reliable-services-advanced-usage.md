<properties
   pageTitle="Verwendung der Dienste zuverlässigen erweiterte | Microsoft Azure"
   description="Informationen Sie zu erweiterten Nutzung des Diensts Fabric zuverlässigen Dienste flexibler Ihrer Dienste."
   services="Service-Fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor="masnider"/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/19/2016"
   ms.author="vturecek"/>

# <a name="advanced-usage-of-the-reliable-services-programming-model"></a>Verwendung der Dienste zuverlässigen programming Modell erweitert
Azure Service-Struktur vereinfacht das Schreiben und Verwalten von zuverlässigen zustandslosen und dynamische Services. Mit diesem Leitfaden wird über erweiterte Verwendungen zuverlässigen Dienstleistungen zu gewinnen mehr Kontrolle und Flexibilität über Ihre Dienste sprechen. Machen Sie sich vor dem Lesen diesen Leitfaden, mit [Den zuverlässigen Diensten programming Modell](service-fabric-reliable-services-introduction.md)vertraut.

Dynamische und zustandsfrei Services stehen zwei primäre Felder und Optionen für Benutzer-Code:

 - `RunAsync`ist ein allgemeine Einstiegspunkt für Ihr Service-Code.
 - `CreateServiceReplicaListeners`und `CreateServiceInstanceListeners` ist für die Kommunikation Listener für Client-Anfragen zu öffnen.
 
Für die meisten Dienste sind diese Eintrittspunkte zwei ausreichend. Wenn Sie mehr Kontrolle über eine Service-Lebenszyklus erforderlich ist, sind zusätzliche Lebenszyklusereignisse in Ausnahmefällen verfügbar.

## <a name="stateless-service-instance-lifecycle"></a>Statusfreie Dienst Instanz Lebenszyklus

Ein statusfreie Dienst Lebenszyklus ist sehr einfach. Ein statusfreier Dienst kann nur geöffnet, geschlossen oder abgebrochen werden. `RunAsync`in ein statusfreier Dienst wird ausgeführt, wenn eine Instanz wird geöffnet und abgebrochen, wenn eine Dienstinstanz geschlossen oder abgebrochen wird. 

Obwohl `RunAsync` sollten in ausreichend fast allen Fällen den öffnen, schließen und Abbruch Ereignisse in einem statusfreie Dienst stehen auch zur Verfügung:

- `Task OnOpenAsync(IStatelessServicePartition, CancellationToken)`
  OnOpenAsync wird aufgerufen, wenn die Instanz des Diensts statusfreie gerade verwendet werden. Erweiterte Service Initialisierungsaufgaben können zu diesem Zeitpunkt gestartet werden.

- `Task OnCloseAsync(CancellationToken)`
  OnCloseAsync wird aufgerufen, wenn die Instanz des Diensts statusfreie ordnungsgemäß beendet werden soll. Dies kann auftreten, wenn des Diensts Code aktualisiert wird gerade, die Instanz des Diensts aufgrund Lastenausgleich verschoben wird oder ein vorübergehender Fehler gefunden wird. OnCloseAsync kann verwendet werden, sicheres Ressourcen schließen, alle Hintergrund Verarbeitung beenden, Ende externen Zustand gespeichert oder vorhandene Verbindungen zu schließen.

- `void OnAbort()`
  OnAbort wird aufgerufen, wenn die Instanz des Diensts statusfreie erzwingen beenden. Dies ist im Allgemeinen bezeichnet, wenn ein permanenter Fehler auf dem Knoten gefunden wird, oder Dienst Fabric Instanz des Diensts Lebenszyklus aufgrund interner Fehler nicht zuverlässig verwalten kann.

## <a name="stateful-service-replica-lifecycle"></a>Dynamische Dienst Replikat Lebenszyklus

Eine dynamische Service-Replikat Lebenszyklus ist viel komplexer als eine Dienstinstanz statusfreie. Darüber hinaus Deklaration ein Replikats dynamische Dienst Rolle Änderungen zum Öffnen, schließen und Ereignisse abzubrechen, während ihrer Lebensdauer. Wenn eine dynamische Dienst Replikat Rolle, ändert sich die `OnChangeRoleAsync` Ereignis ausgelöst wird:

- `Task OnChangeRoleAsync(ReplicaRole, CancellationToken)`
  OnChangeRoleAsync wird aufgerufen, wenn das dynamische Dienst Replikat Rolle, beispielsweise primäre oder sekundäre geändert hat. Primäre Replikate sind, erhalten Sie Schreibstatus (sind zum Erstellen und Schreiben zuverlässigen Websitesammlungen zulässig). Sekundäre Replikate erhalten Lesestatus (nur aus vorhandenen zuverlässigen Sammlungen lesen). Die meisten wird in eine dynamische Dienst am primäres Replikat ausgeführt. Sekundäre Replikate können schreibgeschützte Validierung, Berichten, Datamining oder andere schreibgeschützt Aufträge ausführen.

In eine dynamische Dienst nur primäre Replikat verfügt über Schreibzugriff auf Status, und somit ist im Allgemeinen beim Dienst ist-Arbeit ausführt. Die `RunAsync` Methode in eine dynamische Dienst wird ausgeführt, nur, wenn das dynamische Dienst Replikat primäre ist. Die `RunAsync` Methode beim Ändern von einer primäres Replikat-Funktion nicht am primären und während der Ereignisse schließen und Abbruch abgebrochen wird. 

Verwenden der `OnChangeRoleAsync` Ereignis können Sie Aufgaben je nach Replikat Rolle auch als Antwort auf Rolle ändern.

Ein dynamische Dienst bietet auch die gleichen vier Lebenszyklusereignisse als statusfreie Dienst, mit dem gleichen Semantik und Fällen verwenden:

- `Task OnOpenAsync(IStatefulServicePartition, CancellationToken)`
- `Task OnCloseAsync(CancellationToken)`
- `void OnAbort()`



## <a name="next-steps"></a>Nächste Schritte
Erweiterte Themen im Zusammenhang mit dem Dienst Fabric finden Sie unter den folgenden Artikeln:

- [Konfigurieren von dynamische zuverlässigen Diensten](service-fabric-reliable-services-configuration.md)

- [Einführung in Dienst Fabric-Dienststatus](service-fabric-health-introduction.md)

- [Verwenden Verwendungsberichte System zur Behandlung dieses Problems](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

- [Konfigurieren von Diensten mit dem Dienst Fabric Cluster-Ressource-Manager](service-fabric-cluster-resource-manager-configure-services.md)

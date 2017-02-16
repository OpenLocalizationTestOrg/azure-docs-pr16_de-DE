<properties
   pageTitle="Prüfbarkeit: Service Kommunikation | Microsoft Azure"
   description="Dienst-Kommunikation stellt einen kritischen Integration einer Fabric Service-Anwendung. In diesem Artikel wird erläutert, gibt und Testen Verfahren."
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
   ms.date="07/06/2016"
   ms.author="vturecek"/>

# <a name="service-fabric-testability-scenarios-service-communication"></a>Dienst Fabric Prüfbarkeit Szenarien: Service Kommunikation

Microservices und Service-orientierte Architektur Formatvorlagen Fläche natürlich in Azure Service Architektur. In diesen Abfragearten verteilte Architekturen gegliederte Microservice Applications in der Regel aus mehreren Diensten bestehen, die miteinander kommunizieren müssen. In auch die einfachsten Fällen müssen Sie in der Regel mindestens eine statusfreie Webdienst und eine dynamische Daten Speicherdienst, die Kommunikation benötigen.

Dienst-Kommunikation stellt einen kritischen Integration Anwendung, da jeden Dienst eine remote-API mit anderen Diensten macht. Arbeiten mit einer Reihe von API Begrenzung, die im Allgemeinen e/a beinhaltet erfordert einige Vorsicht, mit einem guten Zeitraum Tests und Validierung.

Es gibt zahlreiche Aspekte, die Sie vornehmen, wenn Sie diesen Dienst Zuständigkeitsbereich in einem verteilten System miteinander verbunden sind:

 - *Transportregel-Protokoll*. Verwenden Sie HTTP, um einer größeren Interoperabilität oder ein benutzerdefiniertes binäre Protokoll Durchsatzes?
 - *Fehlerbehandlung*. Wie werden permanente oder temporäre Fehler werden behandelt? Was geschieht, wenn ein Dienst an einen anderen Knoten verschiebt?
 - *Zeitlimit und Wartezeit*. In Clientanwendungen mehrstufige wird jede Serviceebene wie Wartezeit durch den Stapel und dem Benutzer behandeln?

Ob Sie mit einer der integrierten Kommunikationskomponenten des vom Dienst Fabric bereitgestellt, oder Sie Erstellen eigener, Testen der Interaktionen zwischen Ihrer Dienste ist entscheidend, Sicherstellung der Stabilität in Ihrer Anwendung.

## <a name="prepare-for-services-to-move"></a>Bereiten Sie für die verschieben-Dienste vor

Instanzen der Dienst möglicherweise über einen Zeitraum navigieren. Dies gilt besonders, wenn sie mit laden Kennzahlen für optimale Ressource-abgestimmt Lastenausgleich konfiguriert sind. Dienst Fabric bewegt sich die Dienstinstanzen zu ihrer Verfügbarkeit maximieren, auch während des Upgrades, Failovers, Skalierung und andere Situationen, in denen über die Lebensdauer eines verteilten Systems auftreten.

Wie Dienste im Cluster navigieren, sollte Kunden und andere Dienste, zwei Szenarien zu behandeln, wenn sie an einen Dienst sprechen vorbereitet werden:

- Der Dienst Instanz oder Partition-Kopie wurde seit dem letzten verschoben, die Sie damit gesprochen haben. Dies ist ein normaler Bestandteil des Service-Lebenszyklus, und es sollte erwartet werden, während der Lebensdauer der Anwendung auftritt.
- Der Dienst Instanz oder Partition-Kopie wird gerade verschieben. Zwar Failover eines Diensts von einem Knoten zu einem anderen sehr schnell Dienst Struktur auftritt, kann es jedoch eine Verzögerung bei der Verfügbarkeit ist die Kommunikationskomponente von Ihrem Dienst langsam zu beginnen.

Es ist wichtig, eines Systems reibungslos laufende, diese Szenarios ordnungsgemäß zu behandeln. Hierzu Folgendes beachten:

- Jeder Dienst, der mit verbunden sein kann verfügt über eine *Adresse* , die sie (beispielsweise HTTP oder WebSockets) überwacht. Wenn ein Dienstinstanz oder Partition verschoben wird, dessen Adresse Endpunkt ändert. (Es wird in einen anderen Knoten mit einer anderen IP-Adresse verschoben.) Wenn Sie die integrierten Kommunikationskomponenten verwenden, werden sie erneut Beheben von Service-Adressen für Sie behandeln.
- Möglicherweise gibt es eine vorübergehende Erhöhung Dienst Wartezeit als der Instanz gestartet von deren Zuhörer erneut. Dies hängt davon ab, wie schnell der Dienst die Zuhörer öffnet, nachdem die Instanz des Diensts verschoben wird.
- Eine beliebige vorhandenen Verbindungen müssen geschlossen und neu geöffnet, nachdem der Dienst auf einem neuen Knoten geöffnet werden. Ein sicheres Knoten war(en) oder Restart können die Zeit für vorhandene Verbindungen ordnungsgemäß beendet werden soll.

### <a name="test-it-move-service-instances"></a>Testen Sie ihn: Dienstinstanzen verschieben

Durch Service-Fabric Prüfbarkeit-Tools verwenden, können Sie ein Testszenario Sie zum Testen der folgenden Situationen auf verschiedene Arten erstellen:

1. Verschieben des Diensts eine dynamische primäres Replikat an.

    Die primäre Kopie eine dynamische Servicepartition kann eine Reihe von Gründen verschoben werden. Verwenden Sie diese Option, die primäre Kopie einer bestimmten Partition Ihrer Dienste wie auf dem Verschieben sehr kontrolliert zu reagieren angezeigt werden sollen.

    ```powershell

    PS > Move-ServiceFabricPrimaryReplica -PartitionId 6faa4ffa-521a-44e9-8351-dfca0f7e0466 -ServiceName fabric:/MyApplication/MyService

    ```

2. Beenden eines Knotens.

    Bei ein Knoten beendet wird, verschiebt Dienst Fabric alle Dienstinstanzen oder Partitionen, die auf diesem Knoten auf einen der anderen verfügbaren Knoten im Cluster wurden. Hiermit können Sie eine Situation testen, wobei ein Knoten aus Ihren Cluster und alle Dienstinstanzen verloren und müssen Replikate auf diesem Knoten verschieben.

    Sie können einen Knoten beenden, mithilfe des Cmdlets PowerShell **ServiceFabricNode beenden** :

    ```powershell

    PS > Restart-ServiceFabricNode -NodeName Node_1

    ```

## <a name="maintain-service-availability"></a>Verwalten der Verfügbarkeit von Diensten

Als Plattform Dienst Fabric soll hohen Verfügbarkeit Ihrer Dienste bereitzustellen. Aber in schwerwiegenden Fällen können Probleme mit der zugrunde liegenden Infrastruktur dennoch Ausfall verursachen. Es ist wichtig, die für diese Szenarios zu testen.

Dynamische Dienste verwenden ein Quorum-System Zustand hohen Verfügbarkeit repliziert. Dies bedeutet, dass ein Quorum von Replikaten schreiben Operationen verfügbar sein muss. In Ausnahmefällen, z. B. ein Hardwarefehler weit verbreitet ein Quorum Replikate möglicherweise nicht zur Verfügung. In diesen Fällen nicht Schreiboperationen ausgeführt werden, aber Sie können weiterhin zum Ausführen von Vorgängen finden Sie hier.

### <a name="test-it-write-operation-unavailability"></a>Testen Sie ihn: Schreiben Vorgang ist nicht verfügbar

Mithilfe der Tools Prüfbarkeit Dienst Struktur können Sie einen Fehler einfügen, der Quorum Verlust als Test hervorruft. Obwohl seltene dies der Fall ist, ist es wichtig, dass Clients und Dienste, die eine dynamische Dienst abhängig, zur Behandlung von Situationen vorbereitet sind, in dem sie vornehmen können keine Anfragen zu schreiben. Es ist es wichtig, dass der dynamische Dienst selbst diese Möglichkeit bekannt ist und kann ordnungsgemäß für Anrufer kommunizieren.

Sie können mithilfe des Cmdlets PowerShell **Aufrufen-ServiceFabricPartitionQuorumLoss** Quorum Verlust auslösen:

```powershell

PS > Invoke-ServiceFabricPartitionQuorumLoss -ServiceName fabric:/Myapplication/MyService -QuorumLossMode QuorumReplicas -QuorumLossDurationInSeconds 20

```

In diesem Beispiel wir festlegen `QuorumLossMode` auf `QuorumReplicas` um darauf hinzuweisen, dass wir Quorum Verlust auslösen, ohne nach unten alle Replikate aufzeichnen möchten. Auf diese Weise gelesen Vorgänge sind weiterhin möglich. Um ein Szenario zu testen, wo eine gesamte Partition nicht verfügbar ist, können Sie diesen Schalter auf festlegen `AllReplicas`.

## <a name="next-steps"></a>Nächste Schritte

[Weitere Informationen zu Prüfbarkeit Aktionen](service-fabric-testability-actions.md)

[Weitere Informationen zu Prüfbarkeit Szenarien](service-fabric-testability-scenarios.md)

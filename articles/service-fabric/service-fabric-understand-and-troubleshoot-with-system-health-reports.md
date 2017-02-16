<properties
   pageTitle="Behandeln von Problemen mit mit Integritätsberichte System | Microsoft Azure"
   description="Beschreibt die Integritätsberichte für zur Problembehandlung Cluster oder Anwendungsprobleme vom Azure Service Fabric-Komponenten und deren Verwendung gesendet."
   services="service-fabric"
   documentationCenter=".net"
   authors="oanapl"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/28/2016"
   ms.author="oanapl"/>

# <a name="use-system-health-reports-to-troubleshoot"></a>Verwenden Sie Verwendungsberichte System zum Behandeln von Problemen mit

Azure Service Fabric Komponenten Bericht im Paket auf alle Elemente im Cluster. Der [Dienststatus speichern](service-fabric-health-introduction.md#health-store) erstellt und löscht Elemente, basierend auf den Systemberichten. Es organisiert auch diese in einer Hierarchie, die Entität Interaktionen erfasst werden.

> [AZURE.NOTE] Um Dienststatus-bezogene Konzepte zu verstehen, Weitere Informationen am [Dienst Fabric Gesundheit Modell](service-fabric-health-introduction.md).

System Verwendungsberichte bieten Einblick in Cluster und Anwendungsfunktionalität und Kennzeichnung Probleme über Dienststatus. Für Anwendungen und Dienste Besitz System Integritätsberichte-Einheiten implementiert werden und werden aus der Dienst Fabric Perspektive ordnungsgemäß verhält. Die Berichte bieten keine Gesundheit Überwachung der Geschäftslogik des Diensts oder Erkennung von Prozessen nicht mehr reagiert. Benutzerdienste können zum Erweitern der Dienststatus Daten mit Informationen, die speziell für ihre Logik.

> [AZURE.NOTE] Integritätsberichte Watchdogs sind nur *nach* die Systemkomponenten eine Entität erstellen sichtbar. Wenn eine Entität gelöscht wird, löscht den Dienststatus-Speicher automatisch alle Integritätsberichte zugeordnet. Dasselbe gilt, wenn eine neue Instanz der Entität erstellt wurde (z. B. ein neuen Dienst Instanzreplikats erstellt wird). Alle Berichte, die die alte Instanz zugeordnet werden gelöscht und aus dem Speicher bereinigt.

Die Systemkomponente Berichte werden durch die Quelle, der mit beginnt identifiziert den "**System.**" Präfix. Watchdogs können dasselbe Präfix für ihren Quellen, wie Berichte mit ungültigen Parametern abgelehnt werden verwenden.
Sehen wir uns einige Systemberichte zu verstehen, was sie auslöst und so die mögliche Probleme zu beheben, die sie darstellen.

> [AZURE.NOTE] Dienst Fabric weiterhin Berichte auf Bedingungen relevante zu addieren, die Sichtbarkeit in was in den Cluster und die Anwendung passiert zu verbessern.

## <a name="cluster-system-health-reports"></a>Cluster System Verwendungsberichte
Die Cluster Health Entität wird im Speicher Dienststatus automatisch erstellt. Wenn alles ordnungsgemäß funktioniert, wird nicht es ein Berichts System haben.

### <a name="neighborhood-loss"></a>Verlust der Umgebung
**System.Federation** meldet einen Fehler an, wenn einen Verlust Netzwerk-Umgebung erkannt. Der Bericht aus einzelnen Knoten ist, und die Knoten-ID befindet sich auf den Namen der Eigenschaft. Wenn eine Umgebung in der gesamten Dienst Fabric anrufen verloren ist, können Sie die beiden folgenden Ereignisse (beide Seiten des Berichts Lücke) in der Regel erwarten. Wenn Sie weitere Themenwelten verloren gegangen sind, sind weitere Ereignisse.

Der Bericht gibt das Timeout globale verleasen als die Time to live an. Der Bericht wird jeder Hälfte der Vorgangsdauer TTL umgeleitet, solange die Bedingung aktiv bleibt. Das Ereignis wird automatisch entfernt, wenn es abgelaufen ist. Entfernen Sie beim abgelaufenen Verhalten wird sichergestellt, dass der Bericht aus dem Dienststatus Speicher ordnungsgemäß, bereinigt ist, selbst wenn der reporting Knoten ausgefallen ist.

- **SourceId**: System.Federation
- **Eigenschaft**: beginnt mit dem **Netzwerk-Umgebung** und Knoteninformationen enthält
- **Nächste Schritte**: ermitteln, warum der Umgebung verloren geht (z. B. überprüfen die Kommunikation zwischen Cluster-Knoten).

## <a name="node-system-health-reports"></a>Verwendungsberichte Knoten system
**System.FM**, die den Failover-Manager-Dienst darstellt, ist die Zertifizierungsstelle, die Informationen zu Cluster-Knoten verwaltet. Jeder Knoten sollte einen Bericht aus System.FM Zustand mit verfügen. Die Knoten Elemente werden entfernt, wenn Zustand des Knotens entfernt wird (siehe [RemoveNodeStateAsync](https://msdn.microsoft.com/library/azure/mt161348.aspx)).

### <a name="node-updown"></a>Knoten nach oben/unten
System.FM Berichte als OK, wenn der Knoten anrufen Beitritt (ist einsatzbereit). Es meldet einen Fehler auf, wenn der Knoten meldet sich anrufen ab (unten entweder ist, für das Upgrade oder einfach, da es fehlgeschlagen ist). Die Dienststatus Hierarchie erstellt, indem der Dienststatus Store übernimmt Aktion bereitgestellten Personen in Verbindung mit System.FM Knoten Berichte. Es hält den Knoten virtuelle ein übergeordnetes Element aller bereitgestellten Elemente. Die bereitgestellten Elemente auf diesem Knoten werden durch Abfragen verfügbar, wenn der Knoten als aktiv gemeldet wird durch System.FM, mit der gleichen Instanz wie die Instanz, die Personen zugewiesen sind. Wenn System.FM meldet, dass der Knoten nach unten wird oder neu gestartet (eine neue Instanz), bereinigt der Dienststatus Store automatisch die bereitgestellten Elemente, die nur auf den nach unten weisenden Knoten oder auf die vorherige Instanz des Knotens vorhanden sind, können ein.

- **SourceId**: System.FM
- **Eigenschaft**: Zustand
- **Nächste Schritte**: Wenn der Knoten nach unten für ein Upgrade ist, es sollte im Zusammenhang wieder, nachdem sie das Upgrade durchgeführt hat. In diesem Fall sollte der Integritätsstatus wieder auf OK gewechselt werden. Wenn der Knoten nicht zurückkommen oder diese nicht, benötigt das Problem weitere Untersuchung aus.

Im folgenden Beispiel wird das Ereignis System.FM mit einer Integritätsstatus OK für Knoten einrichten:

```powershell

PS C:\> Get-ServiceFabricNodeHealth -NodeName Node.1
NodeName              : Node.1
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 2
                        SentAt                : 4/24/2015 5:27:33 PM
                        ReceivedAt            : 4/24/2015 5:28:50 PM
                        TTL                   : Infinite
                        Description           : Fabric node is up.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 5:28:50 PM

```


### <a name="certificate-expiration"></a>Ablauf von Zertifikaten
**System.FabricNode** Berichte eine Warnung aus, wenn vom Knoten verwendeten Zertifikate in der Nähe Ablauf sind. Es gibt drei Zertifikate pro Knoten: **Certificate_cluster**, **Certificate_server**und **Certificate_default_client**. Der Ablauf wird mindestens zwei Wochen verbleiben, der Bericht Integritätsstatus OK. Wenn der Ablauf innerhalb von zwei Wochen ist, ist der Typ des Berichts eine Warnung aus. TTL dieser Ereignisse unbegrenzte ist, und sie werden entfernt, wenn ein Knoten Cluster verlässt.

- **SourceId**: System.FabricNode
- **Eigenschaft**: beginnt mit **Zertifikat** und enthält weitere Informationen zu den Zertifikattyp
- **Nächste Schritte**: Aktualisieren Sie die Zertifikate aus, wenn Sie sich in der Nähe Ablauf befinden.

### <a name="load-capacity-violation"></a>Laden Sie Kapazität Verstoß
Dienst Fabric Lastenausgleich Berichte eine Warnung aus, wenn es sich um einen Knoten Kapazität Verstoß erkennt.

 - **SourceId**: System.PLB
 - **Eigenschaft**: bei **Kapazität** beginnt
 - **Nächste Schritte**: bereitgestellten Kennzahlen überprüfen und die aktuelle Kapazität auf dem Knoten anzeigen.

## <a name="application-system-health-reports"></a>Anwendung System Verwendungsberichte
**System.CM**, die den Dienst-Manager die Cluster darstellt, ist die Zertifizierungsstelle, die Informationen über eine Anwendung verwaltet werden.

### <a name="state"></a>Bundesstaat
System.CM Berichte als OK, wenn die Anwendung erstellt oder aktualisiert wurde. Darüber informiert den Dienststatus Store, wenn die Anwendung gelöscht wurden, damit es Store entfernt werden kann.

- **SourceId**: System.CM
- **Eigenschaft**: Zustand
- **Nächste Schritte**: Wenn die Anwendung erstellt wurde, darf den Cluster Manager Gesundheit Bericht enthalten. Überprüfen Sie den Status der Anwendung andernfalls, indem Sie eine Abfrage (beispielsweise der PowerShell-Cmdlet * *Get-ServiceFabricApplication-ApplicationName *ApplicationName***).

Im folgenden Beispiel wird das Ereignis Zustand auf dem **Fabric: / WordCount** Anwendung:

```powershell
PS C:\> Get-ServiceFabricApplicationHealth fabric:/WordCount -ServicesFilter None -DeployedApplicationsFilter None

ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Ok
ServiceHealthStates             : None
DeployedApplicationHealthStates : None
HealthEvents                    :
                                  SourceId              : System.CM
                                  Property              : State
                                  HealthState           : Ok
                                  SequenceNumber        : 82
                                  SentAt                : 4/24/2015 6:12:51 PM
                                  ReceivedAt            : 4/24/2015 6:12:51 PM
                                  TTL                   : Infinite
                                  Description           : Application has been created.
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : ->Ok = 4/24/2015 6:12:51 PM
```

## <a name="service-system-health-reports"></a>Dienst System Verwendungsberichte
**System.FM**, die den Failover-Manager-Dienst darstellt, ist die Zertifizierungsstelle, die Informationen zu den Services verwaltet werden.

### <a name="state"></a>Bundesstaat
System.FM Berichte als OK, wenn der Dienst erstellt wurde. Wenn der Dienst gelöscht wurde, werden die Entität aus den Dienststatus Store gelöscht.

- **SourceId**: System.FM
- **Eigenschaft**: Zustand

Im folgenden Beispiel wird das Ereignis Bundesstaat-Dienst **Fabric: / WordCount/WordCountService**:

```powershell
PS C:\> Get-ServiceFabricServiceHealth fabric:/WordCount/WordCountService

ServiceName           : fabric:/WordCount/WordCountService
AggregatedHealthState : Ok
PartitionHealthStates :
                        PartitionId           : 875a1caa-d79f-43bd-ac9d-43ee89a9891c
                        AggregatedHealthState : Ok

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 3
                        SentAt                : 4/24/2015 6:12:51 PM
                        ReceivedAt            : 4/24/2015 6:13:01 PM
                        TTL                   : Infinite
                        Description           : Service has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:13:01 PM
```

### <a name="unplaced-replicas-violation"></a>Nicht positionierten Replikate Verstoß
**System.PLB** Berichte eine Warnung aus, wenn eine Position für einen oder mehrere Dienst Replikate gefunden werden kann. Der Bericht wird entfernt, wenn es abgelaufen ist.

- **SourceId**: System.FM
- **Eigenschaft**: Zustand
- **Nächste Schritte**: Überprüfen der Dienst Einschränkungen und den aktuellen Status der Anordnung.

Das folgende Beispiel zeigt einen Verstoß für einen Dienst konfiguriert mit 7 Ziel Replikaten in einen Cluster mit 5 Knoten an:

```xml
PS C:\> Get-ServiceFabricServiceHealth fabric:/WordCount/WordCountService


ServiceName           : fabric:/WordCount/WordCountService
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.PLB',
                        Property='ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b', HealthState='Warning',
                        ConsiderWarningAsError=false.

PartitionHealthStates :
                        PartitionId           : a1f83a35-d6bf-4d39-b90d-28d15f39599b
                        AggregatedHealthState : Warning

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 10
                        SentAt                : 3/22/2016 7:56:53 PM
                        ReceivedAt            : 3/22/2016 7:57:18 PM
                        TTL                   : Infinite
                        Description           : Service has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM

                        SourceId              : System.PLB
                        Property              : ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b
                        HealthState           : Warning
                        SequenceNumber        : 131032232425505477
                        SentAt                : 3/23/2016 4:14:02 PM
                        ReceivedAt            : 3/23/2016 4:14:03 PM
                        TTL                   : 00:01:05
                        Description           : The Load Balancer was unable to find a placement for one or more of the Service's Replicas:
                        fabric:/WordCount/WordCountService Secondary Partition a1f83a35-d6bf-4d39-b90d-28d15f39599b could not be placed, possibly,
                        due to the following constraints and properties:  
                        Placement Constraint: N/A
                        Depended Service: N/A

                        Constraint Elimination Sequence:
                        ReplicaExclusionStatic eliminated 4 possible node(s) for placement -- 1/5 node(s) remain.
                        ReplicaExclusionDynamic eliminated 1 possible node(s) for placement -- 0/5 node(s) remain.

                        Nodes Eliminated By Constraints:

                        ReplicaExclusionStatic:
                        FaultDomain:fd:/0 NodeName:_Node_0 NodeType:NodeType0 UpgradeDomain:0 UpgradeDomain: ud:/0 Deactivation Intent/Status:
                        None/None
                        FaultDomain:fd:/1 NodeName:_Node_1 NodeType:NodeType1 UpgradeDomain:1 UpgradeDomain: ud:/1 Deactivation Intent/Status:
                        None/None
                        FaultDomain:fd:/3 NodeName:_Node_3 NodeType:NodeType3 UpgradeDomain:3 UpgradeDomain: ud:/3 Deactivation Intent/Status:
                        None/None
                        FaultDomain:fd:/4 NodeName:_Node_4 NodeType:NodeType4 UpgradeDomain:4 UpgradeDomain: ud:/4 Deactivation Intent/Status:
                        None/None

                        ReplicaExclusionDynamic:
                        FaultDomain:fd:/2 NodeName:_Node_2 NodeType:NodeType2 UpgradeDomain:2 UpgradeDomain: ud:/2 Deactivation Intent/Status:
                        None/None


                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 3/22/2016 7:57:48 PM, LastOk = 1/1/0001 12:00:00 AM
```

## <a name="partition-system-health-reports"></a>Partition System Verwendungsberichte
**System.FM**, die den Failover-Manager-Dienst darstellt, ist die Zertifizierungsstelle, die Informationen zu den Servicepartitionen verwaltet werden.

### <a name="state"></a>Bundesstaat
System.FM Berichte als OK, wenn der erstellt wurde und fehlerfrei ist. Wenn die Partition gelöscht wird, werden die Entität aus den Dienststatus Store gelöscht.

Wenn die unter der minimalen Replikat Anzahl ist, wird ein Fehler erzeugt. Wenn die Partition nicht unter der minimalen Replikat zählen ist, aber es unterhalb der Anzahl der Ziel-Replikat ist, wird eine Warnung gemeldet. Wenn die Quorum Verlust ist, Berichte System.FM einen Fehler auf.

Weitere wichtige Ereignisse einbeziehen eine Warnung aus, wenn die Neukonfiguration länger dauert als erwartet und wenn der Build länger als erwartet dauert. Die erwarteten Zeiten für das Erstellen und neu konfiguriert konfiguriert werden auf der Grundlage Dienst Szenarien. Wenn ein Dienst eine TB des Status, beispielsweise SQL-Datenbank weist dauert der Build mehr als für einen Dienst mit einigen wenigen Zustand.

- **SourceId**: System.FM
- **Eigenschaft**: Zustand
- **Nächste Schritte**: der Integritätsstatus nicht OK ist, ist es möglich, dass einige Replikate nicht erstellt, geöffnet, oder wurden die Kundendaten in primären oder sekundären ordnungsgemäß. In vielen Fällen ist die Ursache eines Fehlers Dienst in der Implementierung öffnen oder Rolle ändern.

Im folgenden Beispiel wird eine fehlerfreie Partition:

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/StatelessPiApplication/StatelessPiService | Get-ServiceFabricPartitionHealth
PartitionId           : 29da484c-2c08-40c5-b5d9-03774af9a9bf
AggregatedHealthState : Ok
ReplicaHealthStates   : None
HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 38
                        SentAt                : 4/24/2015 6:33:10 PM
                        ReceivedAt            : 4/24/2015 6:33:31 PM
                        TTL                   : Infinite
                        Description           : Partition is healthy.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:33:31 PM
```

Im folgenden Beispiel wird die Integrität des eine Partition, die unter Ziel Replikat zählen ist. Im nächsten Schritt wird die Beschreibung Partition abgerufen werden, das zeigt, wie es konfiguriert ist: **MinReplicaSetSize** ist zwei und **TargetReplicaSetSize** sieben. Klicken Sie dann die Anzahl der Knoten im Cluster abrufen: fünf. Daher darf in diesem Fall zwei Replikate befinden.

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricPartitionHealth -ReplicasFilter None

PartitionId           : 875a1caa-d79f-43bd-ac9d-43ee89a9891c
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.

ReplicaHealthStates   : None
HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Warning
                        SequenceNumber        : 37
                        SentAt                : 4/24/2015 6:13:12 PM
                        ReceivedAt            : 4/24/2015 6:13:31 PM
                        TTL                   : Infinite
                        Description           : Partition is below target replica or instance count.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Ok->Warning = 4/24/2015 6:13:31 PM

PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService

PartitionId            : 875a1caa-d79f-43bd-ac9d-43ee89a9891c
PartitionKind          : Int64Range
PartitionLowKey        : 1
PartitionHighKey       : 26
PartitionStatus        : Ready
LastQuorumLossDuration : 00:00:00
MinReplicaSetSize      : 2
TargetReplicaSetSize   : 7
HealthState            : Warning
DataLossNumber         : 130743727710830900
ConfigurationNumber    : 8589934592


PS C:\> @(Get-ServiceFabricNode).Count
5
```

### <a name="replica-constraint-violation"></a>Verletzung von Replikat Einschränkung
**System.PLB** Berichte eine Warnung aus, falls es einen Replikat Einschränkung Verstoß erkennt und kann nicht Replikate der Partition platzieren.

- **SourceId**: System.PLB
- **Eigenschaft**: beginnt mit: **ReplicaConstraintViolation**

## <a name="replica-system-health-reports"></a>Replikat System Verwendungsberichte
**System.RA**, die die neu konfiguriert Agent-Komponente darstellt, ist die Berechtigungen für den Bundesstaat Replikat.

### <a name="state"></a>Bundesstaat
**System.RA** Berichte als OK, wenn das Replikat erstellt wurde.

- **SourceId**: System.RA
- **Eigenschaft**: Zustand

Im folgenden Beispiel wird ein fehlerfrei Replikat:

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricReplica | where {$_.ReplicaRole -eq "Primary"} | Get-ServiceFabricReplicaHealth
PartitionId           : 875a1caa-d79f-43bd-ac9d-43ee89a9891c
ReplicaId             : 130743727717237310
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 130743727718018580
                        SentAt                : 4/24/2015 6:12:51 PM
                        ReceivedAt            : 4/24/2015 6:13:02 PM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:13:02 PM
```

### <a name="replica-open-status"></a>Status für Replikat öffnen
Die Beschreibung dieses Berichts Gesundheit enthält die Startzeit (Coordinated Universal Time) aus, wenn der Anruf API aufgerufen wurde.

**System.RA** Berichte eine Warnung aus, wenn das Replikat öffnen länger als die konfigurierte Periode dauert (Standard: 30 Minuten). Wenn die API wirkt sich auf dienstverfügbarkeit, wird der Bericht viel schneller (Standardwert 30 Sekunden eine konfigurierbare Intervall) ausgegeben. Die Zeit gemessen enthält die Zeit für die Replicator öffnen und den Dienst öffnen. Die Eigenschaft ändert auf OK, wenn die öffnen abgeschlossen ist.

- **SourceId**: System.RA
- **Eigenschaft**: **ReplicaOpenStatus**
- **Nächste Schritte**: Wenn der Integritätsstatus nicht zulässig, ermitteln, warum das Replikat öffnen länger als erwartet dauert.

### <a name="slow-service-api-call"></a>Langsam API Serviceanfrage
**System.RAP** und **System.Replicator** Bericht eine Warnung, wenn der Anruf an die Benutzer dienstleistungscode dauert länger als die konfigurierte Zeit. Die Warnung ist deaktiviert, wenn der Anruf abgeschlossen ist.

- **SourceId**: System.RAP oder System.Replicator
- **Eigenschaft**: der Name der langsam API. Die Beschreibung enthält weitere Informationen über die Zeit, die die API wurde steht noch aus.
- **Nächste Schritte**: ermitteln, warum der Anruf länger als erwartet dauert.

Im folgenden Beispiel wird eine Partition Quorum verloren gehen, und die gerichtliche Schritte fertig, um zu ermitteln, warum. Eines der Replikate gibt Integritätsstatus eines Warnung, sodass Sie deren Status erhalten. Es zeigt, dass die Service-Operation mehr Zeit als erwartet, ein Ereignis von System.RAP gemeldet hat. Nachdem Sie diese Informationen eingegangen sind, besteht der nächste Schritt prüfen Sie den Code, und es zu ermitteln. Für diesem Fall löst die **RunAsync** -Implementierung des Diensts dynamische Ausnahmefehler. Die Replikate sind Wiederverwendung, damit alle Replikate im Zustand Warnung möglicherweise nicht angezeigt. Sie können den Integritätsstatus erste wiederholen und suchen Sie nach alle Unterschiede zwischen der Replikat-ID In bestimmten Fällen können Sie die Wiederholungsversuche Hinweise geben.

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/HelloWorldStatefulApplication/HelloWorldStateful | Get-ServiceFabricPartitionHealth

PartitionId           : 72a0fb3e-53ec-44f2-9983-2f272aca3e38
AggregatedHealthState : Error
UnhealthyEvaluations  :
                        Error event: SourceId='System.FM', Property='State'.

ReplicaHealthStates   :
                        ReplicaId             : 130743748372546446
                        AggregatedHealthState : Ok

                        ReplicaId             : 130743746168084332
                        AggregatedHealthState : Ok

                        ReplicaId             : 130743746195428808
                        AggregatedHealthState : Warning

                        ReplicaId             : 130743746195428807
                        AggregatedHealthState : Ok

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Error
                        SequenceNumber        : 182
                        SentAt                : 4/24/2015 7:00:17 PM
                        ReceivedAt            : 4/24/2015 7:00:31 PM
                        TTL                   : Infinite
                        Description           : Partition is in quorum loss.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Warning->Error = 4/24/2015 6:51:31 PM

PS C:\> Get-ServiceFabricPartition fabric:/HelloWorldStatefulApplication/HelloWorldStateful

PartitionId            : 72a0fb3e-53ec-44f2-9983-2f272aca3e38
PartitionKind          : Int64Range
PartitionLowKey        : -9223372036854775808
PartitionHighKey       : 9223372036854775807
PartitionStatus        : InQuorumLoss
LastQuorumLossDuration : 00:00:13
MinReplicaSetSize      : 2
TargetReplicaSetSize   : 3
HealthState            : Error
DataLossNumber         : 130743746152927699
ConfigurationNumber    : 227633266688

PS C:\> Get-ServiceFabricReplica 72a0fb3e-53ec-44f2-9983-2f272aca3e38 130743746195428808

ReplicaId           : 130743746195428808
ReplicaAddress      : PartitionId: 72a0fb3e-53ec-44f2-9983-2f272aca3e38, ReplicaId: 130743746195428808
ReplicaRole         : Primary
NodeName            : Node.3
ReplicaStatus       : Ready
LastInBuildDuration : 00:00:01
HealthState         : Warning

PS C:\> Get-ServiceFabricReplicaHealth 72a0fb3e-53ec-44f2-9983-2f272aca3e38 130743746195428808

PartitionId           : 72a0fb3e-53ec-44f2-9983-2f272aca3e38
ReplicaId             : 130743746195428808
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.RAP', Property='ServiceOpenOperationDuration', HealthState='Warning', ConsiderWarningAsError=false.

HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 130743756170185892
                        SentAt                : 4/24/2015 7:00:17 PM
                        ReceivedAt            : 4/24/2015 7:00:33 PM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 7:00:33 PM

                        SourceId              : System.RAP
                        Property              : ServiceOpenOperationDuration
                        HealthState           : Warning
                        SequenceNumber        : 130743756399407044
                        SentAt                : 4/24/2015 7:00:39 PM
                        ReceivedAt            : 4/24/2015 7:00:59 PM
                        TTL                   : Infinite
                        Description           : Start Time (UTC): 2015-04-24 19:00:17.019
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Warning = 4/24/2015 7:00:59 PM
```

Wenn Sie die fehlerhafte Anwendung unter dem Debugger starten, Anzeigen der Ereignisse, die Windows aus RunAsync ausgelöste Ausnahme aus:

![Visual Studio 2015 diagnostic Ereignisse: RunAsync Fehler in Architektur: / HelloWorldStatefulApplication.][1]

Visual Studio 2015 diagnostic Ereignisse: RunAsync Setupfehler in **Fabric: / HelloWorldStatefulApplication**.

[1]: ./media/service-fabric-understand-and-troubleshoot-with-system-health-reports/servicefabric-health-vs-runasync-exception.png


### <a name="replication-queue-full"></a>Vollständige Replikationswarteschlange
**System.Replicator** Berichte eine Warnung aus, wenn die Replikationswarteschlange voll ist. Auf dem primären geschieht dies in der Regel, da eine oder mehrere sekundäre Replikate langsam, bestätigen Sie Vorgänge sind. Des sekundären geschieht dies in der Regel, wenn der Dienst langsam die Vorgänge angewendet wird. Wenn die Warteschlange nicht mehr voll ist, wird die Warnung deaktiviert.

- **SourceId**: System.Replicator
- **Eigenschaft**: **PrimaryReplicationQueueStatus** oder **SecondaryReplicationQueueStatus**, abhängig von der Rolle Replikat

### <a name="slow-naming-operations"></a>Benennen Vorgänge langsam

**System.NamingService** Berichte Dienststatus auf deren primäres Replikat, wenn ein Vorgang Naming länger als die zulässigen dauert. Beispiele für Naming Vorgänge sind [CreateServiceAsync](https://msdn.microsoft.com/library/azure/mt124028.aspx) oder [DeleteServiceAsync](https://msdn.microsoft.com/library/azure/mt124029.aspx). Weitere Methoden können unter FabricClient, beispielsweise unter [Management](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.propertymanagementclient.aspx) [Service Management Methoden](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.servicemanagementclient.aspx) oder Eigenschaft gefunden werden.

> [AZURE.NOTE] Der Dienst Naming Service Namen an einem Speicherort im Cluster aufgelöst und ermöglicht Benutzern, die Namen der Dienste und Eigenschaften zu verwalten. Es ist, dass eine Service-Struktur aufgeteilt Dienst beibehalten. Eine der Partitionen stellt den Besitzer Zertifizierungsstelle, die Metadaten für alle Namen Dienst Fabric und Dienste enthält. Die Namen der Dienst Fabric sind unterschiedliche Partitionen, Besitzer des Namens Partitionen, aufgerufen, also der Dienst extensible zugeordnet. Weitere Informationen zum [Naming Service](service-fabric-architecture.md).

Wenn ein Vorgang Naming länger dauert als erwartet, wird der Vorgang mit einem Bericht Warnung auf dem *primären Kopie der Naming Servicepartition, die den Vorgang fungiert,*gekennzeichnet. Wenn der Vorgang erfolgreich abgeschlossen ist, wird die Warnung deaktiviert. Wenn der Vorgang mit einem Fehler abgeschlossen ist, enthält der Bericht Gesundheit Details zu dem Fehler an.

- **SourceId**: System.NamingService
- **Eigenschaft**: beginnt mit Präfix **Duration_** und identifiziert langsamer Vorgang und den Namen des Service Fabrics, an dem der Vorgang angewendet wird. Angenommen, wenn Dienst am Namen Fabric erstellen: / Anwendung/MyService dauert zu lange, die Eigenschaft ist Duration_AOCreateService.fabric:/MyApp/MyService. AO verweist auf die Rolle der Naming Partition für diese Namen und den Vorgang.
- **Nächste Schritte**: Überprüfen, warum die Naming durchgeführt. Jede Operation kann verschiedene Ursachen haben. Löschen Sie zum Beispiel Dienst möglicherweise auf einem Knoten hängen, da der Anwendungshost auf einem Knoten auf einen Benutzer Fehler im Dienstcode abstürzt behält.

Im folgenden Beispiel wird ein Dienst Erstellungsvorgang. Der Vorgang benötigte länger als die konfigurierte Dauer. AO wiederholt und sendet Arbeit auf Nein fest. NICHT den letzten Vorgang mit Timeout abgeschlossen. In diesem Fall ist der gleichen Replikatgruppe primär für AO und keine Rollen.

```powershell
PartitionId           : 00000000-0000-0000-0000-000000001000
ReplicaId             : 131064359253133577
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.NamingService', Property='Duration_AOCreateService.fabric:/MyApp/MyService', HealthState='Warning', ConsiderWarningAsError=false.

HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 131064359308715535
                        SentAt                : 4/29/2016 8:38:50 PM
                        ReceivedAt            : 4/29/2016 8:39:08 PM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 4/29/2016 8:39:08 PM, LastWarning = 1/1/0001 12:00:00 AM

                        SourceId              : System.NamingService
                        Property              : Duration_AOCreateService.fabric:/MyApp/MyService
                        HealthState           : Warning
                        SequenceNumber        : 131064359526778775
                        SentAt                : 4/29/2016 8:39:12 PM
                        ReceivedAt            : 4/29/2016 8:39:38 PM
                        TTL                   : 00:05:00
                        Description           : The AOCreateService started at 2016-04-29 20:39:08.677 is taking longer than 30.000.
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 4/29/2016 8:39:38 PM, LastOk = 1/1/0001 12:00:00 AM

                        SourceId              : System.NamingService
                        Property              : Duration_NOCreateService.fabric:/MyApp/MyService
                        HealthState           : Warning
                        SequenceNumber        : 131064360657607311
                        SentAt                : 4/29/2016 8:41:05 PM
                        ReceivedAt            : 4/29/2016 8:41:08 PM
                        TTL                   : 00:00:15
                        Description           : The NOCreateService started at 2016-04-29 20:39:08.689 completed with FABRIC_E_TIMEOUT in more than 30.000.
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 4/29/2016 8:39:38 PM, LastOk = 1/1/0001 12:00:00 AM
```

## <a name="deployedapplication-system-health-reports"></a>DeployedApplication System Verwendungsberichte
**System.Hosting** ist die Zertifizierungsstelle auf dem bereitgestellten Elemente.

### <a name="activation"></a>Aktivierung
System.Hosting Berichte als OK, wenn eine Anwendung auf dem Knoten erfolgreich aktiviert wurde. Andernfalls wird ein Fehler erzeugt.

- **SourceId**: System.Hosting
- **Eigenschaft**: Aktivierung, einschließlich der Einführung Version
- **Nächste Schritte**: Wenn die Anwendung fehlerhaft ist, untersuchen, warum die Aktivierung fehlgeschlagen ist.

Im folgenden Beispiel wird die erfolgreichen Aktivierung:

```powershell
PS C:\> Get-ServiceFabricDeployedApplicationHealth -NodeName Node.1 -ApplicationName fabric:/WordCount

ApplicationName                    : fabric:/WordCount
NodeName                           : Node.1
AggregatedHealthState              : Ok
DeployedServicePackageHealthStates :
                                     ServiceManifestName   : WordCountServicePkg
                                     NodeName              : Node.1
                                     AggregatedHealthState : Ok

HealthEvents                       :
                                     SourceId              : System.Hosting
                                     Property              : Activation
                                     HealthState           : Ok
                                     SequenceNumber        : 130743727751144415
                                     SentAt                : 4/24/2015 6:12:55 PM
                                     ReceivedAt            : 4/24/2015 6:13:03 PM
                                     TTL                   : Infinite
                                     Description           : The application was activated successfully.
                                     RemoveWhenExpired     : False
                                     IsExpired             : False
                                     Transitions           : ->Ok = 4/24/2015 6:13:03 PM
```

### <a name="download"></a>Herunterladen
**System.Hosting** meldet einen Fehler an, wenn der Anwendung Paketdownload fehlschlägt.

- **SourceId**: System.Hosting
- **Eigenschaft**: * *herunterladen:*RolloutVersion***
- **Nächste Schritte**: ermitteln, warum der Download auf dem Knoten fehlgeschlagen ist.

## <a name="deployedservicepackage-system-health-reports"></a>DeployedServicePackage System Verwendungsberichte
**System.Hosting** ist die Zertifizierungsstelle auf dem bereitgestellten Elemente.

### <a name="service-package-activation"></a>Aktivieren des Service-Pakets
System.Hosting Berichte als OK, wenn die Aktivierung des Service-Paket auf dem Knoten erfolgreich ist. Andernfalls wird ein Fehler erzeugt.

- **SourceId**: System.Hosting
- **Eigenschaft**: Aktivierung
- **Nächste Schritte**: ermitteln, warum die Aktivierung fehlgeschlagen ist.

### <a name="code-package-activation"></a>Aktivieren des Pakets Code
**System.Hosting** Berichte als OK für jedes Paket Code, wenn die Aktivierung erfolgreich ist. Die Aktivierung schlägt fehl, wird eine Warnung gemeldet wie konfiguriert. Wenn **CodePackage** Aktivierung fehlschlägt oder mit einem Fehler größer als der konfigurierte **CodePackageHealthErrorThreshold**beendet, Berichte Hostinganbieter einen Fehler zurück. Wenn ein Service-Paket mehrere Code Pakete enthält, wird ein Aktivierung Bericht für jedes ausgeschlossene generiert.

- **SourceId**: System.Hosting
- **Eigenschaft**: verwendet das Präfix **CodePackageActivation** und enthält den Namen des Pakets Code und den Einstiegspunkt als * *CodePackageActivation:*CodePackageName*:*SetupEntryPoint/Einstiegspunkt* ** (z. B. **CodePackageActivation:Code:SetupEntryPoint**)

### <a name="service-type-registration"></a>Typ-Dienste registrieren
**System.Hosting** Berichte als OK, wenn der Diensttyp erfolgreich registriert wurde. Es meldet einen Fehler, wenn die Registrierung Zeitpunkt durchgeführt wurde nicht (wie konfiguriert mithilfe von **ServiceTypeRegistrationTimeout**). Wenn der Diensttyp aus dem Knoten aufgehoben wird, ist dies, da die Laufzeit geschlossen wurde. Hostinganbieter Berichte eine Warnung.

- **SourceId**: System.Hosting
- **Eigenschaft**: verwendet das Präfix **ServiceTypeRegistration** und enthält den Dienst Typnamen (beispielsweise **ServiceTypeRegistration:FileStoreServiceType**)

Im folgenden Beispiel wird ein Paket fehlerfrei bereitgestellte Dienst an:

```powershell
PS C:\> Get-ServiceFabricDeployedServicePackageHealth -NodeName Node.1 -ApplicationName fabric:/WordCount -ServiceManifestName WordCountServicePkg


ApplicationName       : fabric:/WordCount
ServiceManifestName   : WordCountServicePkg
NodeName              : Node.1
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.Hosting
                        Property              : Activation
                        HealthState           : Ok
                        SequenceNumber        : 130743727751456915
                        SentAt                : 4/24/2015 6:12:55 PM
                        ReceivedAt            : 4/24/2015 6:13:03 PM
                        TTL                   : Infinite
                        Description           : The ServicePackage was activated successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:13:03 PM

                        SourceId              : System.Hosting
                        Property              : CodePackageActivation:Code:EntryPoint
                        HealthState           : Ok
                        SequenceNumber        : 130743727751613185
                        SentAt                : 4/24/2015 6:12:55 PM
                        ReceivedAt            : 4/24/2015 6:13:03 PM
                        TTL                   : Infinite
                        Description           : The CodePackage was activated successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:13:03 PM

                        SourceId              : System.Hosting
                        Property              : ServiceTypeRegistration:WordCountServiceType
                        HealthState           : Ok
                        SequenceNumber        : 130743727753644473
                        SentAt                : 4/24/2015 6:12:55 PM
                        ReceivedAt            : 4/24/2015 6:13:03 PM
                        TTL                   : Infinite
                        Description           : The ServiceType was registered successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:13:03 PM
```

### <a name="download"></a>Herunterladen
**System.Hosting** meldet einen Fehler an, wenn der Dienst Paketdownload fehlschlägt.

- **SourceId**: System.Hosting
- **Eigenschaft**: * *herunterladen:*RolloutVersion***
- **Nächste Schritte**: ermitteln, warum der Download auf dem Knoten fehlgeschlagen ist.

### <a name="upgrade-validation"></a>Prüfung der Aktualisierung
**System.Hosting** meldet einen Fehler an, wenn während des Upgrades Validierung fehlschlägt oder das Upgrade auf dem Knoten fehlschlägt.

- **SourceId**: System.Hosting
- **Eigenschaft**: verwendet das Präfix **FabricUpgradeValidation** und enthält die Upgrade-Version
- **Beschreibung**: Zeigt auf den Fehler

## <a name="next-steps"></a>Nächste Schritte
[Dienst Fabric-Integritätsberichte anzeigen](service-fabric-view-entities-aggregated-health.md)

[So Berichten und Kontrollkästchen-Dienststatus](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Überwachen und Dienste lokal diagnostizieren](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Upgrade der Fabric Service-Anwendung](service-fabric-application-upgrade.md)

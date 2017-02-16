<properties
   pageTitle="Problembehandlung bei Upgrades der Anwendung | Microsoft Azure"
   description="Dieser Artikel behandelt einige häufig auftretende Probleme, um das Upgrade einer Fabric Service-Anwendung und wie Sie sie auflösen."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/14/2016"
   ms.author="subramar"/>

# <a name="troubleshoot-application-upgrades"></a>Behandeln von Problemen mit Upgrades der Anwendung

In diesem Artikel werden einige der auftretenden Probleme im Zusammenhang Upgrade Anwendung Azure Service Fabric und deren Behebung beschrieben.

## <a name="troubleshoot-a-failed-application-upgrade"></a>Behandeln von Problemen mit einer Fehler beim Anwendungsupgrade

Wenn ein Upgrade fehlschlägt, enthält die Ausgabe des Befehls **Get-ServiceFabricApplicationUpgrade** zusätzliche Informationen für das Debuggen des Fehlers an.  Die folgende Liste gibt an, wie die zusätzliche Informationen verwendet werden kann:

1. Identifizieren der Fehlertyp an.
2. Grund für den Fehler zu identifizieren.
3. Identifizieren Sie eine oder mehrere fehlerhafte Komponenten zur weiteren Untersuchung an.

Diese Informationen sind verfügbar, wenn Dienst Fabric erkennt den Fehler unabhängig davon, ob die **FailureAction** zurücksetzen oder Anhalten der Aktualisierung.

### <a name="identify-the-failure-type"></a>Identifizieren der Fehlertyp

In der Ausgabe von **Get-ServiceFabricApplicationUpgrade**identifiziert **FailureTimestampUtc** den Zeitstempel (in UTC), einen Fehler bei der Aktualisierung wurde vom Dienst Fabric erkannt und **FailureAction** ausgelöst wurde. **FailureReason** identifiziert eine der drei mögliche auf hoher Ebene Ursachen des Fehlers:

1. UpgradeDomainTimeout - gibt an, dass eine bestimmte Upgrade Domäne hat zu lange gedauert und **UpgradeDomainTimeout** abgelaufen sind.
2. OverallUpgradeTimeout - gibt an, dass der gesamten Upgrades hat zu lange gedauert und **UpgradeTimeout** abgelaufen sind.
3. HealthCheck - gibt an, dass nach einem Upgrade von einer Domäne aktualisieren, die Anwendung entsprechend der angegebenen Gesundheit Richtlinien fehlerhaften geblieben und **HealthCheckRetryTimeout** abgelaufen sind.

Diese Einträge angezeigt nur in der Ausgabe bei die Aktualisierung schlägt fehl, und startet zurückzukehren. Weitere Informationen werden je nach Typ des Fehlers angezeigt.

### <a name="investigate-upgrade-timeouts"></a>Ermitteln der Upgrade Zeitlimit

Upgrade Timeout, Fehlern durch Verfügbarkeit Dienstprobleme am häufigsten verursacht werden. Die Ausgabe nach diesem Absatz ist für die Aktualisierung der normalen, auf denen fehlschlägt Dienst Replikate oder Instanzen in die neue Codeversion zu starten. Das Feld **UpgradeDomainProgressAtFailure** zeichnet eine Momentaufnahme der ausstehende Beginn der Arbeiten zum Zeitpunkt des Fehlers.

~~~
PS D:\temp> Get-ServiceFabricApplicationUpgrade fabric:/DemoApp

ApplicationName                : fabric:/DemoApp
ApplicationTypeName            : DemoAppType
TargetApplicationTypeVersion   : v2
ApplicationParameters          : {}
StartTimestampUtc              : 4/14/2015 9:26:38 PM
FailureTimestampUtc            : 4/14/2015 9:27:05 PM
FailureReason                  : UpgradeDomainTimeout
UpgradeDomainProgressAtFailure : MYUD1

                                 NodeName            : Node4
                                 UpgradePhase        : PostUpgradeSafetyCheck
                                 PendingSafetyChecks :
                                     WaitForPrimaryPlacement - PartitionId: 744c8d9f-1d26-417e-a60e-cd48f5c098f0

                                 NodeName            : Node1
                                 UpgradePhase        : PostUpgradeSafetyCheck
                                 PendingSafetyChecks :
                                     WaitForPrimaryPlacement - PartitionId: 4b43f4d8-b26b-424e-9307-7a7a62e79750
UpgradeState                   : RollingBackCompleted
UpgradeDuration                : 00:00:46
CurrentUpgradeDomainDuration   : 00:00:00
NextUpgradeDomain              :
UpgradeDomainsStatus           : { "MYUD1" = "Completed";
                                 "MYUD2" = "Completed";
                                 "MYUD3" = "Completed" }
UpgradeKind                    : Rolling
RollingUpgradeMode             : UnmonitoredAuto
ForceRestart                   : False
UpgradeReplicaSetCheckTimeout  : 00:00:00
~~~

In diesem Beispiel die Aktualisierung fehlgeschlagen bei Upgrade Domäne *MYUD1* und zwei Partitionen (*744c8d9f-1d26-417e-a60e-cd48f5c098f0* und *4b43f4d8-b26b-424e-9307-7a7a62e79750*) hängen wurden. Die Partitionen wurden hängen, da die Laufzeit kann nicht primäre Replikate (*WaitForPrimaryPlacement*) auf den Zielknoten *Knoten 1* und *Knoten 4*platziert wurde.

Der Befehl **Get-ServiceFabricNode** kann verwendet werden, um sicherzustellen, dass diese beiden Knoten in Upgrade Domäne *MYUD1*entsprechen. Die *UpgradePhase* besagt *PostUpgradeSafetyCheck*, was bedeutet, dass diese Sicherheit Prüfungen auftreten können, nachdem alle Knoten in der Domäne Upgrade Upgrade abgeschlossen haben. Alle diese Informationen verweist auf ein Problem mit der neuen Version von Code der Anwendung. Die am häufigsten auftretenden Probleme sind Dienst Fehler in der öffnen oder Heraufstufen zu primären Codepfade.

Eine *UpgradePhase* der *PreUpgradeSafetyCheck* bedeutet es wurden Probleme Vorbereiten der Upgrade Domäne aus, bevor sie durchgeführt wurde. In diesem Fall sind die am häufigsten auftretenden Probleme Service-Fehler in der schließen oder das Herabstufen von primären Codepfaden aus.

Der aktuelle **UpgradeState** ist *RollingBackCompleted*, damit die ursprüngliche Aktualisierung mit zurücksetzen **FailureAction**, ausgeführt wurde muss, die automatisch rückgängig das Upgrade nach einem Fehler gemacht. Wenn mit einem manuellen **FailureAction**das ursprüngliche Upgrade durchgeführt wurde, würde dann das Upgrade stattdessen in einem angehaltenen Zustand live dürfen der Anwendung für das Debuggen werden.

### <a name="investigate-health-check-failures"></a>Ermitteln der Health Check-Fehler

Health Check-Fehler können durch verschiedene Probleme ausgelöst werden, die ausgeführt werden können, nachdem alle Knoten in einer Domäne Upgrade abgeschlossen haben, aktualisieren und alle Sicherheit Prüfungen übergeben. Die Ausgabe, folgen diesem Absatz ist ein Upgrade Fehler aufgrund fehlgeschlagener integritätsprüfungen typische. Das Feld **UnhealthyEvaluations** erfasst eine Momentaufnahme der integritätsprüfungen, die zum Zeitpunkt des Upgrades abhängig von der angegebenen [Gesundheit Richtlinie](service-fabric-health-introduction.md)fehlgeschlagen ist.

~~~
PS D:\temp> Get-ServiceFabricApplicationUpgrade fabric:/DemoApp

ApplicationName                         : fabric:/DemoApp
ApplicationTypeName                     : DemoAppType
TargetApplicationTypeVersion            : v4
ApplicationParameters                   : {}
StartTimestampUtc                       : 4/24/2015 2:42:31 AM
UpgradeState                            : RollingForwardPending
UpgradeDuration                         : 00:00:27
CurrentUpgradeDomainDuration            : 00:00:27
NextUpgradeDomain                       : MYUD2
UpgradeDomainsStatus                    : { "MYUD1" = "Completed";
                                          "MYUD2" = "Pending";
                                          "MYUD3" = "Pending" }
UnhealthyEvaluations                    :
                                          Unhealthy services: 50% (2/4), ServiceType='PersistedServiceType', MaxPercentUnhealthyServices=0%.

                                          Unhealthy service: ServiceName='fabric:/DemoApp/Svc3', AggregatedHealthState='Error'.

                                              Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                              Unhealthy partition: PartitionId='3a9911f6-a2e5-452d-89a8-09271e7e49a8', AggregatedHealthState='Error'.

                                                  Error event: SourceId='Replica', Property='InjectedFault'.

                                          Unhealthy service: ServiceName='fabric:/DemoApp/Svc2', AggregatedHealthState='Error'.

                                              Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                              Unhealthy partition: PartitionId='744c8d9f-1d26-417e-a60e-cd48f5c098f0', AggregatedHealthState='Error'.

                                                  Error event: SourceId='Replica', Property='InjectedFault'.

UpgradeKind                             : Rolling
RollingUpgradeMode                      : Monitored
FailureAction                           : Manual
ForceRestart                            : False
UpgradeReplicaSetCheckTimeout           : 49710.06:28:15
HealthCheckWaitDuration                 : 00:00:00
HealthCheckStableDuration               : 00:00:10
HealthCheckRetryTimeout                 : 00:00:10
UpgradeDomainTimeout                    : 10675199.02:48:05.4775807
UpgradeTimeout                          : 10675199.02:48:05.4775807
ConsiderWarningAsError                  :
MaxPercentUnhealthyPartitionsPerService :
MaxPercentUnhealthyReplicasPerPartition :
MaxPercentUnhealthyServices             :
MaxPercentUnhealthyDeployedApplications :
ServiceTypeHealthPolicyMap              :
~~~

Untersuchung läuft Health Check-Fehler, müssen Sie zuerst verstehen des Modells Gesundheit Dienst Fabric. Aber auch ohne solche Kenntnisse, können wir sehen, dass zwei Dienste fehlerhaft sind: *Fabric: / DemoApp/Svc3* und *Fabric: / DemoApp/Svc2*, zusammen mit den Integritätsberichte zurück (in diesem Fall "InjectedFault"). In diesem Beispiel werden zwei von vier Services fehlerhaften, welche unter dem standardmäßigen Ziel von 0 % fehlerhaften (*MaxPercentUnhealthyServices*) ist.

Die Aktualisierung wurde bei fehlerhaften durch Angeben von einer **FailureAction** des Handbuchs beim Starten des Upgrades unterbrochen. In diesem Modus können uns live-System in den fehlerhaften Zustand ermitteln, bevor eine weitere Aktion.

### <a name="recover-from-a-suspended-upgrade"></a>Wiederherstellen eines angehaltenen Upgrades

Es gibt keine Wiederherstellung erforderlich, da die Aktualisierung automatisch bei fehlerhaften zurückgesetzt, mit einem **FailureAction**zurücksetzen. Mit einem manuellen **FailureAction**gibt es mehrere Wiederherstellungsoptionen:

1. Manuelles lösen Sie zurücksetzen aus
2. Führen Sie den Rest der Aktualisierung manuell
3. Fortsetzen des überwachten Upgrades

Zurücksetzen der Anwendungs gestartet wird, kann zu einem beliebigen Zeitpunkt der Befehl **Start-ServiceFabricApplicationRollback** verwendet werden. Nachdem Sie der Befehl erfolgreich zurückgegeben wird, wird die Anforderung zurücksetzen wurde im System registriert und kurz danach beginnt.

Der Befehl **Lebenslauf-ServiceFabricApplicationUpgrade** kann verwendet werden, um durch den Rest der Aktualisierung manuell, fahren ein Upgrade Domäne nacheinander. In diesem Modus werden nur Sicherheit Prüfungen vom System ausgeführt. Es werden keine weiteren integritätsprüfungen ausgeführt. Dieser Befehl kann nur verwendet werden, wenn die *UpgradeState* zeigt *RollingForwardPending*, was bedeutet, dass die aktuelle Upgrade Domäne Upgrade abgeschlossen ist, aber dem nächsten Vorkommen nicht gestartet wurde (Ausstehend).

Der **Update-ServiceFabricApplicationUpgrade** Befehl zum Fortsetzen des überwachten Upgrades mit beide Sicherheit verwendet werden kann und Gesundheit überprüft durchgeführt werden.

~~~
PS D:\temp> Update-ServiceFabricApplicationUpgrade fabric:/DemoApp -UpgradeMode Monitored

UpgradeMode                             : Monitored
ForceRestart                            :
UpgradeReplicaSetCheckTimeout           :
FailureAction                           :
HealthCheckWaitDuration                 :
HealthCheckStableDuration               :
HealthCheckRetryTimeout                 :
UpgradeTimeout                          :
UpgradeDomainTimeout                    :
ConsiderWarningAsError                  :
MaxPercentUnhealthyPartitionsPerService :
MaxPercentUnhealthyReplicasPerPartition :
MaxPercentUnhealthyServices             :
MaxPercentUnhealthyDeployedApplications :
ServiceTypeHealthPolicyMap              :

PS D:\temp>
~~~

Das Upgrade wird weiterhin aus dem Upgrade-Domäne, in der letzten unterbrochen, und der Parameter und Gesundheit Richtlinien als vor dem upgrade verwenden. Bei Bedarf können beliebige Upgrade Parameter und Dienststatus-Richtlinien angezeigt, die in der vorherigen Ausgabe in denselben Befehl geändert werden, wenn die Aktualisierung von Lebensläufen. In diesem Beispiel wurde das Upgrade im Modus überwacht, mit der Parameter und die Integritätsrichtlinien unverändert fortgesetzt wird.

## <a name="further-troubleshooting"></a>Weitere Maßnahmen zur Problembehandlung

### <a name="service-fabric-is-not-following-the-specified-health-policies"></a>Dienst Fabric ist nicht die angegebene Gesundheit Richtlinien folgen.

Mögliche Ursache 1:

Dienst Fabric übersetzt alle Prozentsätze in die tatsächliche Anzahl der Einheiten (z. B. Replikate, Partitionen und Services) für den Systemzustand Auswertung und für gesamte Personen immer aufrundet. Angenommen, wenn die maximale _MaxPercentUnhealthyReplicasPerPartition_ 21 % und fünf Replikate vorhanden sind, klicken Sie dann Service Fabric können bis zu zwei fehlerhaften Replikate (die is,'Math.Ceiling (5\*0,21)). Gesundheit Richtlinien sollten daher entsprechend festgelegt werden.

Mögliche Ursache 2:

Gesundheit Richtlinien werden als Prozentsätze total Dienste und nicht bestimmten Diensteinstanzen angegeben. Beispielsweise vor der Aktualisierung, wenn eine Anwendung vier hat Instanzen A, B, C und D, service, wobei Dienst D fehlerhaft ist jedoch mit wenig Einfluss auf die Anwendung. Wir möchten bekannten fehlerhaften Dienst D während des Upgrades und Festlegen der Parameter *MaxPercentUnhealthyServices* 25 %, unter der Voraussetzung nur A, B und C werden fehlerfrei sein müssen ignorieren.

Jedoch möglicherweise während des Upgrades D fehlerfrei werden während C fehlerhaften wird. Die Aktualisierung erfolgreich sein können, da nur 25 % der Dienste fehlerhaft sind. Jedoch kann es unerwarteter Fehler aufgrund wird unerwartet fehlerhaften statt d C führen In diesem Fall D erstellt werden sollte, als einen anderen Diensttyp von A, B und C. Da Gesundheit Richtlinien pro Diensttyp angegeben sind, können andere fehlerhaften Prozentsatz Schwellenwerte für andere Dienste angewendet werden. 

### <a name="i-did-not-specify-a-health-policy-for-application-upgrade-but-the-upgrade-still-fails-for-some-time-outs-that-i-never-specified"></a>Ich hat keiner Richtlinie Gesundheit für die Aktualisierung der Anwendung, aber die Aktualisierung fehlschlägt weiterhin für einige Timeouts, die ich nie angegeben haben.

Wenn Gesundheit Richtlinien für das Upgrade Anforderung bereitgestellt werden nicht, werden diese aus den *ApplicationManifest.xml* der Version der aktuellen Anwendung übernommen. Beispielsweise, wenn Sie ein Upgrade X Anwendung von Version 1.0 auf Version 2.0, Anwendung Gesundheit Richtlinien für Version 1.0 angegeben sind werden verwendet. Wenn für das Upgrade eine anderen Gesundheit Richtlinie verwendet werden soll, muss die Richtlinie als Teil der Anwendung Upgrade API Anruf angegeben werden muss. Während des Upgrades beziehen sich nur die Richtlinien des Anrufs API angegeben. Sobald die Aktualisierung abgeschlossen ist, werden die Richtlinien, die in der *ApplicationManifest.xml* angegebenen verwendet.

### <a name="incorrect-time-outs-are-specified"></a>Fehlerhafte Timeouts werden angegeben.

Sie können gefragt haben, zu was passiert, wenn Timeouts inkonsistent festgelegt sind. Sie möglicherweise beispielsweise eine *UpgradeTimeout* , die kleiner als der *UpgradeDomainTimeout*ist. Die Antwort ist, dass ein Fehler zurückgegeben wird. Kleiner als die Summe der *HealthCheckWaitDuration* und *HealthCheckRetryTimeout*ist die *UpgradeDomainTimeout* oder *UpgradeDomainTimeout* kleiner als die Summe der *HealthCheckWaitDuration* und *HealthCheckStableDuration*ist, werden Fehler zurückgegeben.

### <a name="my-upgrades-are-taking-too-long"></a>Mein Upgrades abzubrechen zu lang

Die Zeit für ein Upgrade auf durchführen hängt davon ab, die integritätsprüfungen und Timeouts angegeben haben. Integritätsprüfungen und Timeouts hängen davon ab, wie lange dauert es zu kopieren, bereitstellen und Stabilisieren der Anwendung. Wird ebenfalls mit Timeouts anspruchsvollen möglicherweise weitere Fehler beim Upgrades bedeutet, dass, damit wir empfehlen mehr Timeouts konservativ angefangen.

Hier ist eine schnelle Auffrischung auf die Timeouts mit dem Upgrade Zeiten Interaktion aus:

Upgrades eine Upgrade Domäne schneller als *HealthCheckWaitDuration*ausgeführt werden + *HealthCheckStableDuration*.

Fehler beim Aktualisieren der schneller als *HealthCheckWaitDuration*auftreten kann keine + *HealthCheckRetryTimeout*.

Die Upgrade Zeit für ein Upgrade Domäne wird durch *UpgradeDomainTimeout*begrenzt.  Wenn *HealthCheckRetryTimeout* und *HealthCheckStableDuration* sowohl ungleich 0 sind, und die Integrität der Anwendung wechseln behält, und klicken Sie dann das Upgrade später auf *UpgradeDomainTimeout*Timeout erreicht. *UpgradeDomainTimeout* beginnt nach unten zu zählen, nachdem das Upgrade für die aktuelle Upgrade Domäne beginnt.

## <a name="next-steps"></a>Nächste Schritte

[Aktualisieren Ihrer Anwendung mithilfe von Visual Studio](service-fabric-application-upgrade-tutorial.md) führt Sie durch eine Anwendung Aktualisierung mit Visual Studio.

[Aktualisieren Ihrer Anwendung mithilfe von Powershell](service-fabric-application-upgrade-tutorial-powershell.md) führt Sie durch eine Anwendung Aktualisierung mithilfe der PowerShell.

Steuern Sie, wie eine Anwendung upgrades mithilfe von [Parametern zu aktualisieren](service-fabric-application-upgrade-parameters.md).

Machen Sie Ihre Upgrades der Anwendung kompatibel, indem Sie lernen, wie Sie [Datenserialisierung](service-fabric-application-upgrade-data-serialization.md)verwenden.

Informationen Sie zum erweiterten Funktionen, die während des Upgrades Ihrer Anwendungs von Verweisen auf [Erweiterte Themen](service-fabric-application-upgrade-advanced.md)verwenden.

Beheben von häufig auftretenden Problemen in Upgrades der Anwendung von Verweisen auf die Schritte in der [Anwendungsupgrades Problembehandlung](service-fabric-application-upgrade-troubleshooting.md).
 

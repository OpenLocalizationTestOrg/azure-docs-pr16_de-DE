
<properties
   pageTitle="Anwendungsupgrade: Aktualisieren der Parameter | Microsoft Azure"
   description="Beschreibt Parameter, die im Zusammenhang mit Aktualisieren einer Fabric Service-Anwendung, einschließlich integritätsprüfungen ausführen und Richtlinien für das Upgrade automatisch rückgängig zu machen."
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



# <a name="application-upgrade-parameters"></a>Upgrade Anwendungsparameter

In diesem Artikel werden die verschiedenen Parameter, die bei der Aktualisierung der Anwendung Azure Service Fabric anwenden. Die Parameter umfassen den Namen und die Version der Anwendung. Sie sind Regler, die Steuern der Timeouts und integritätsprüfungen, die während des Upgrades angewendet werden, und geben Sie die Richtlinien, die angewendet werden müssen, wenn ein Upgrade fehlschlägt.


<br>

| Parameter | Beschreibung |
| --- | --- |
| ApplicationName | Name der Anwendung, die aktualisiert werden. Beispiele: Fabric: / VisualObjects, Fabric: / ClusterMonitor  |
| TargetApplicationTypeVersion | Geben Sie die Version der Anwendung, die das Upgrade Ziele. |
| FailureAction | Die Aktion, die vom Dienst Fabric geöffnet wird, wenn die Aktualisierung fehlschlägt. Die Anwendung kann rückgängig gemacht werden, auf die Version vor der Aktualisierung (zurücksetzen) oder das Upgrade bei der aktuellen Upgrade Domäne beendet werden kann. In diesem Fall wird der Upgrade Modus auch auf manuell geändert. Zulässige Werte sind zurücksetzen "oder" manuell. |
| HealthCheckWaitDurationSec | Die Zeit (in Sekunden) warten Sie nach Abschluss des Upgrades auf die Upgrade-Domäne vor dem Dienst Fabric wertet die Integrität der Anwendung. Diese Dauer kann auch als die Zeit angesehen werden, die eine Anwendung ausgeführt werden sollen, bevor er fehlerfrei betrachtet werden kann. Wenn die integritätsprüfung erfolgreich ist, wird das Upgrade auf die nächste Upgrade Domäne.  Wenn die integritätsprüfung schlägt fehl, wartet Dienst Fabric nach einem Intervall (die UpgradeHealthCheckInterval), bevor Sie die integritätsprüfung erneut wiederholen, bis die HealthCheckRetryTimeout erreicht ist. Der standardmäßige und empfohlene Wert ist 0 Sekunden. |
| HealthCheckRetryTimeoutSec | Die Dauer (in Sekunden) weiterhin diesen Dienst Fabric Gesundheit Auswertung vor das Upgrade deklarieren, als fehlerhaft ausführen. Die Standardeinstellung ist 600 Sekunden. Diese Dauer wird gestartet, nachdem HealthCheckWaitDuration erreicht ist. In diesem HealthCheckRetryTimeout möglicherweise Dienst Fabric mehrere integritätsprüfungen der Anwendung Gesundheit ausführen. Der Standardwert ist 10 Minuten und für eine Anwendung ordnungsgemäß angepasst werden sollte. |
| HealthCheckStableDurationSec | Die Dauer (in Sekunden) um sicherzustellen, dass die Anwendung vor dem Umstieg auf die nächste Upgrade Domäne oder Durchführen des Upgrades unveränderliche ist. Diese Wartedauer wird verwendet, um zu verhindern, dass nicht erkannte Änderungen der richtigen Gesundheit, wenn die integritätsprüfung durchgeführt wurde. Der Standardwert ist 120 Sekunden und für eine Anwendung entsprechend angepasst werden sollte. |
| UpgradeDomainTimeoutSec | Maximale Uhrzeit (in Sekunden) für das Upgrade einer einzelnen Upgrade Domäne. Wenn dieses Timeout erreicht ist, wird das Upgrade beendet und basierend auf der Einstellung für UpgradeFailureAction verläuft. Der Standardwert ist niemals (unbegrenzte) und für eine Anwendung ordnungsgemäß angepasst werden sollte. |
| UpgradeTimeout | Ein Timeout (in Sekunden), der für die gesamte Aktualisierung angewendet wird. Wenn dieses Timeout erreicht ist, wird das Upgrade Stopps und UpgradeFailureAction ausgelöst. Der Standardwert ist niemals (unbegrenzte) und für eine Anwendung ordnungsgemäß angepasst werden sollte. |
| UpgradeHealthCheckInterval | Die Häufigkeit, der Status aktiviert ist. Für diesen Parameter im Abschnitt ClusterManager _manifest_ _Cluster_ angegeben ist, und nicht als Teil des Upgrades Cmdlets angegeben ist. Der Standardwert ist 60 Sekunden.  |






<br>
## <a name="service-health-evaluation-during-application-upgrade"></a>Dienst Gesundheit Auswertung während des Upgrades der Anwendung

<br>
Die Dienststatus Bewertungskriterien sind optional. Die Dienststatus Bewertungskriterien beim Start von einer Aktualisierung nicht angegeben, verwendet Dienst Fabric die Anwendung Gesundheit Richtlinien, die in der ApplicationManifest.xml der Anwendungsinstanz angegeben.


<br>

| Parameter | Beschreibung |
| --- | --- |
| ConsiderWarningAsError | Standardwert ist falsch. Behandeln Sie die Warnung Systemereignisse für die Anwendung als fehlerhaft an, wenn die Integrität der Anwendung während des Upgrades Auswertung. Standardmäßig wird Dienst Fabric nicht ausgewertet Warnung Systemereignisse benutzerspezifisch Fehlern (Fehler), damit das Upgrade durchgeführt werden kann, auch wenn Warnungsereignisse vorhanden sind.   |
| MaxPercentUnhealthyDeployedApplications | Standard- und empfohlene Wert ist 0. Geben Sie die maximale Anzahl von bereitgestellten Applikationen (finden Sie im [Abschnitt Dienststatus](service-fabric-health-introduction.md)), die fehlerhaften sein kann, bevor die Anwendung als fehlerhaft betrachtet und schlägt die Aktualisierung fehl. Dieser Parameter definiert die Integrität der Anwendung auf dem Knoten und hilft Erkennen von Problemen beim Upgrade. In der Regel, erhalten die Replikate der Anwendung mit Lastenausgleich auf den anderen Knoten, der die Anwendung fehlerfrei, wodurch die Aktualisierung fortgesetzt werden kann. Durch die Angabe einer strengen MaxPercentUnhealthyDeployedApplications Gesundheit, können Fabric Service ein Problem mit der Anwendungspaket schnell erkennen und helfen, ein schnelles Upgrade Fail zu erzeugen. |
| MaxPercentUnhealthyServices | Standard- und empfohlene Wert ist 0. Geben Sie die maximale Anzahl von Diensten in die Instanz der Anwendung, die fehlerhaften sein kann, bevor die Anwendung als fehlerhaft betrachtet und schlägt die Aktualisierung fehl. |
| MaxPercentUnhealthyPartitionsPerService | Standard- und empfohlene Wert ist 0. Geben Sie die maximale Anzahl von Partitionen in einem Webdienst, der fehlerhaften werden kann, bevor der Dienst fehlerhaften gilt. |
| MaxPercentUnhealthyReplicasPerPartition | Standard- und empfohlene Wert ist 0. Geben Sie die maximale Anzahl von Replikaten in Partition, bevor die Partition fehlerhaften gilt fehlerhaft sein kann. |
| UpgradeReplicaSetCheckTimeout | **Statusfreie Service**- innerhalb einer einzelnen Upgrade Domäne versucht Dienst Fabric um sicherzustellen, dass weitere Instanzen des Diensts verfügbar sind. Ist die Anzahl der Instanzen Ziel mehrere, wartet Dienst Fabric mehr als eine Instanz verfügbar ist, auf eine maximale Timeoutwert sein. Dieses Timeout wird mithilfe der Eigenschaft UpgradeReplicaSetCheckTimeout angegeben. Wenn das Timeout abläuft, wird Dienst Fabric mit dem Upgrade, unabhängig von der Anzahl der Dienstinstanzen fortgesetzt. Wenn die Anzahl der Instanzen Ziel eines ist, Fabric Service wartet nicht, und sofort mit dem Upgrade ausgeführt wird. **Dynamische Service**- innerhalb einer einzelnen Upgrade Domäne versucht Dienst Fabric sichergestellt, dass die Replikatgruppe ein Quorum verfügt. Dienst Fabric wartet ein Quorum auf eine maximale Timeoutwert (angegeben durch die Eigenschaft UpgradeReplicaSetCheckTimeout) verfügbar sein. Wenn das Timeout abläuft, wird der Dienst Fabric mit dem Upgrade, unabhängig davon Quorum fortgesetzt. Diese Einstellung ist wie nie festlegen (unendlich), wenn Rollen weiterleiten und 900 Sekunden Wenn zurückgesetzt. |
| ForceRestart | Wenn Sie eine Konfiguration oder Paket mit Daten aktualisieren, ohne aktualisieren den Code, der Dienst neu gestartet wird, wenn Sie die Eigenschaft ForceRestart festgelegt ist true. Wenn die Aktualisierung abgeschlossen ist, benachrichtigt Dienst Fabric Dienst, dass eine neue Konfiguration oder Datenpaket verfügbar ist. Der Dienst ist verantwortlich für die Änderungen anwenden. Falls erforderlich, kann der Dienst selbst neu starten. |



<br>
<br>
Pro Diensttyp für eine Instanz der Anwendung können die Kriterien MaxPercentUnhealthyServices, MaxPercentUnhealthyPartitionsPerService und MaxPercentUnhealthyReplicasPerPartition angegeben werden. Diese Parameter pro Service festlegen ermöglicht eine Anwendung auf verschiedenen Diensten Arten mit anderen Auswertung Richtlinien enthalten. Beispielsweise kann ein Diensttyp statusfreie Gateway ein MaxPercentUnhealthyPartitionsPerService unterschiedlich sein aus eine dynamische-Engine Diensttyp für eine bestimmte Anwendungsinstanz.

## <a name="next-steps"></a>Nächste Schritte

[Aktualisieren Ihrer Anwendung mithilfe von Visual Studio](service-fabric-application-upgrade-tutorial.md) führt Sie durch eine Anwendung Aktualisierung mit Visual Studio.

[Aktualisieren Ihrer Anwendung mithilfe von Powershell](service-fabric-application-upgrade-tutorial-powershell.md) führt Sie durch eine Anwendung Aktualisierung mithilfe der PowerShell.

Machen Sie Ihre Upgrades der Anwendung kompatibel, indem Sie lernen, wie Sie [Datenserialisierung](service-fabric-application-upgrade-data-serialization.md)verwenden.

Informationen Sie zum erweiterten Funktionen, die während des Upgrades Ihrer Anwendungs von Verweisen auf [Erweiterte Themen](service-fabric-application-upgrade-advanced.md)verwenden.

Beheben von häufig auftretenden Problemen in Upgrades der Anwendung von Verweisen auf die Schritte in der [Anwendungsupgrades Problembehandlung](service-fabric-application-upgrade-troubleshooting.md).
 

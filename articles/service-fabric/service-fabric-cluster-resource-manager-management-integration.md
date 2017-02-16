<properties
   pageTitle="Dienst Fabric Cluster Ressourcenmanager - Integration Management | Microsoft Azure"
   description="Eine Übersicht über die Integrationspunkte zwischen Cluster Ressourcenmanager und Fabric Servicemanagement."
   services="service-fabric"
   documentationCenter=".net"
   authors="masnider"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/19/2016"
   ms.author="masnider"/>


# <a name="cluster-resource-manager-integration-with-service-fabric-cluster-management"></a>Integration von Cluster Ressource-Manager mit Service Fabric Cluster management
Der Dienst Fabric Cluster Ressourcenmanager nicht die zentrale Komponente von Fabric-Dienst, der Management-Vorgänge (wie Upgrades der Anwendung) behandelt, aber es einbezogen wird. Die erste Möglichkeit, die der Ressourcenmanager Cluster mit Management unterstützen wird durch das Verfolgen von Cluster und den Diensten darin enthaltenen Ressourcen und Saldo im Hinblick auf des gewünschten Status, und versenden Verwendungsberichte, wenn es keine Cluster, in die gewünschte Konfiguration setzen kann (und Beispiel wäre, ist es nicht ausreichend Kapazität oder Konflikte verursachende Regeln zur Stelle, an der ein Dienst platziert werden soll). Eine andere Integration mit Funktionsweise von Updates zu tun, hat: während des Upgrades der Ressourcenmanager Cluster ändert deren Verhalten. Wir werden diese beiden folgenden sprechen.

## <a name="health-integration"></a>Dienststatus-integration
Ressourcenmanager Cluster ständig verfolgt die Regeln, die Sie für Ihre Dienstleistungen als auch die Kapazität verfügbar ist, auf den Knoten und im Cluster definiert haben und Gesundheit Warnungen und Fehler ausgegeben, wenn sie diese Regeln erfüllen kann, oder es ist nicht ausreichend Kapazität. Beispielsweise wenn ein Knoten über Kapazität und Ressourcenmanager Cluster nicht die Situation korrigieren gibt es eine Gesundheit Warnung, der angibt, welche Knoten über Kapazität, und bei denen metrische ist.

Ein weiteres Beispiel einer Uhrzeit beim wird der Ausgabe von Warnungen Gesundheit Ressourcenmanager angezeigt wird, wenn Sie eine Einschränkung Platzierung definiert haben (wie "NodeColor == Blau") und der Ressourcenmanager erkennt einen Verstoß gegen die Einschränkung. Dies erfolgt sowohl für benutzerdefinierte Einschränkungen als auch die Einschränkungen (wie Fehlerstrukturanalyse und der Upgradeeinstellungen Domäne Einschränkungen), die automatisch erzwungen werden.

Hier ist ein Beispiel für einen Bericht solche Dienststatus. In diesem Fall ist der Dienststatus Bericht für eine der Systemdienst Partitionen aus folgenden Gründen die Replikate dieser Partition vorübergehend in zu wenige Upgrade Domänen verpackt sind wie auftreten konnte wäre es eine Zeichenfolge mit Fehlern:

```posh
PS C:\Users\User > Get-WindowsFabricPartitionHealth -PartitionId '00000000-0000-0000-0000-000000000001'


PartitionId           : 00000000-0000-0000-0000-000000000001
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.PLB', Property='ReplicaConstraintViolation_UpgradeDomain', HealthState='Warning', ConsiderWarningAsError=false.

ReplicaHealthStates   :
                        ReplicaId             : 130766528804733380
                        AggregatedHealthState : Ok

                        ReplicaId             : 130766528804577821
                        AggregatedHealthState : Ok

                        ReplicaId             : 130766528854889931
                        AggregatedHealthState : Ok

                        ReplicaId             : 130766528804577822
                        AggregatedHealthState : Ok

                        ReplicaId             : 130837073190680024
                        AggregatedHealthState : Ok

HealthEvents          :
                        SourceId              : System.PLB
                        Property              : ReplicaConstraintViolation_UpgradeDomain
                        HealthState           : Warning
                        SequenceNumber        : 130837100116930204
                        SentAt                : 8/10/2015 7:53:31 PM
                        ReceivedAt            : 8/10/2015 7:53:33 PM
                        TTL                   : 00:01:05
                        Description           : The Load Balancer has detected a Constraint Violation for this Replica: fabric:/System/FailoverManagerService Secondary Partition 00000000-0000-0000-0000-000000000001 is
                        violating the Constraint: UpgradeDomain Details: Node -- 3d1a4a68b2592f55125328cd0f8ed477  Policy -- Packing
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Ok->Warning = 8/10/2015 7:13:02 PM, LastError = 1/1/0001 12:00:00 AM
```

Hier ist, was diese Meldung Gesundheit teilt uns ist ist:

1.  Alle Replikate selbst sind fehlerfrei (Dies ist ein Dienst Fabric des ersten Priorität)
2.  Dass die Aktualisierung Domäne Verteilung Einschränkung aktuell verletzt, wird wird (d. h., dass eine bestimmte Domäne aktualisieren Weitere Replikate für diese Partition weist als sonst)
3.  Welche Knoten enthält, das bewirken, dass des Verstoßes Replikat (den Knoten mit ID: 3d1a4a68b2592f55125328cd0f8ed477)
4.  Wenn diese alle Vorkommnissen (8/10/2015 7:13:02 PM)

Dies ist eine großartige Daten für eine Benachrichtigung, die bei erfolgreicher etwas schiefgelaufen und möchten Sie wahrscheinlich einen Blick wechseln Herstellung wird ausgelöst. In diesem Fall würde beispielsweise wir möchten, um festzustellen, ob ermittelt werden kann, warum der Ressourcenmanager Eindruck haben, wie sie die Wahl gelassen würde, aber die Replikate in der Domäne aktualisieren packen. Möglicherweise aller Knoten in der anderen Upgrade Domänen wurden nach unten, und es waren nicht ausreichend Ersatzteile anderen Domänen oder wäre es genügend Domänen von etwas anderem die verursacht Knoten in diesen anderen Domänen Upgrade (wie die Richtlinie einige Position auf dem Dienst oder nicht ausreichend Kapazität, beispielsweise) ungültig zu sein.

Einmal Angenommen jedoch, ein Diensts erstellen möchten oder Ressourcenmanager versucht, einen Speicherort zum Platzieren Sie einige Dienste suchen, aber es keine werden alle Lösungen angezeigt, die arbeiten. Normalerweise ist wegen einer der folgenden zwei Konditionen, jedoch möglicherweise viele verschiedene Ursachen haben:

1.  Einige vorübergehende Bedingung hat es nicht möglich, dieser Dienstinstanz oder Replikat ordnungsgemäß platzieren gemacht
2.  Des Diensts Anforderungen werden auf eine Weise, die die Anforderungen unsatisfiable werden bewirkt, dass, falsch konfiguriert.

In jedem der folgenden Bedingungen sehen Sie ein Bericht Gesundheit aus der Cluster Ressourcenmanager, die Sie feststellen, welche Hilfe zur bieten stattfindet und warum nicht der Dienst abgelegt werden. Wir nennen diesen Prozess "Einschränkung Beseitigung Sequenz". Das System durchläuft während, aus der konfigurierten Einschränkungen den Dienst und Datensätze beeinträchtigen, was sie beseitigen. Auf diese Weise Wenn Services nicht sehen können, platziert werden, Sie können finden Sie unter welche Knoten entfernt wurden und warum.

## <a name="constraint-types"></a>Einschränkungstypen
Sprechen wir über jede der anderen Einschränkungen, die in diese Berichte Gesundheit und was überprüft angezeigt werden können. Beachten Sie, dass in den meisten Fällen Sie einige dieser Einschränkungen nicht sehen Knoten unterdrücken, da die Einschränkungen bei der weiche sind oder optimieren Sie Ebene standardmäßig (es gibt weitere Informationen zu weiteren Einschränkung Prioritäten entlang in diesem Artikel) zu. Sie könnten sehen jedoch Gesundheit Nachrichten im Zusammenhang mit dieser schränkt, wenn sie als harte Einschränkungen oder in Ausnahmefällen, die diese Knoten beseitigt werden, verursachen, damit wir ihnen hier Vollständigkeit präsentieren konfiguriert sind:

-   Dies ist eine interne Einschränkung, die angibt, dass bei der Suche wir in einer Situation ausgeführt wurde, in dem zwei dynamische Replikate oder statusfreie Instanzen aus dem gleichen-Abschnitt müssten auf demselben Knoten platziert werden (das nicht zulässig ist), ReplicaExclusionStatic und ReplicaExclusionDynamic –. ReplicaExclusionStatic und ReplicaExclusionDynamic sind fast genau die gleiche Regel ein. Die Einschränkung ReplicaExclusionDynamic besagt "wir dieses Replikat hier platzieren konnten nicht, da der einzige Lösungsvorschlag hier noch ein Replikat platziert hatten". Dies unterscheidet sich von der ReplicaExclusionStatic Ausschluss der festlegt, keinen vorgeschlagenen Konflikt aber eine tatsächliche – ist es ein Replikat bereits auf dem Knoten. Stiftet dieser Verwirrung? Ja. Ist es sehr wichtig? Nein. Genügt es zu sagen, dass, wenn Sie eine Einschränkung Beseitigung Sequenz sehen, mit dem ReplicaExclusionStatic oder der ReplicaExclusionDynamic Einschränkung, dass der Cluster Ressourcenmanager hält, die es nicht zur Verfügung genügend Knoten, um alle Replikate zu platzieren. Die weiteren Einschränkungen können normalerweise uns mitteilen, wie wir mit zu wenig an erster Stelle zu landen sind.
-   PlacementConstraint: Wenn diese Meldung angezeigt wird, bedeutet das, dass wir einige Knoten beseitigt, da diese des Diensts Platzierung Einschränkungen übereinstimmen haben. Wir verfolgen, die derzeit konfigurierten Platzierung Einschränkungen als Teil dieser Nachricht. Dies ist in der Regel normal, wenn Sie Nebenbedingungen Platzierung verfügen, jedoch ist es, dass ein Fehler in der Position Einschränkung verursacht werden zu viele Knoten dieses Problem beseitigt ist, in dem Sie das Ergebnis sehen können.
-   NodeCapacity: Wenn Sie sehen, diese Einschränkung, dies bedeutet, dass wir die Replikate auf die angegebenen Knoten platzieren konnten nicht, da dies, den Knoten wechseln über Kapazität würde
-   Zugehörigkeit: Diese Einschränkung gibt an, dass wir das Replikat auf den betroffenen Knoten platzieren konnten nicht, da dadurch einen Verstoß gegen die Zugehörigkeit Einschränkung wäre.
-   FaultDomain & UpgradeDomain: Diese Einschränkung Knoten entfällt, wenn das Replikat für die angegebenen Knoten Vermarktung wäre Verpacken in einer bestimmten Fehlerstrukturanalyse oder Upgrade Domäne. Beispiele für diese Einschränkung besprochen werden im Thema [Fehlerstrukturanalyse und Upgrade Domäne Einschränkungen](service-fabric-cluster-resource-manager-cluster-description.md) und sich daraus ergebende Verhalten dargestellt.
-   PreferredLocation: Sie dürfen nicht normal finden Sie unter diese Einschränkung Knoten, aus der Lösung, da es Optimierung wird entfernt werden, werden standardmäßig verursacht. Weiter, die gewünschte Position Einschränkung ist normalerweise nur vorhanden bei Upgrades (Wenn sie zum Verschieben von Replika zurück zu Beginn die Aktualisierung an ihrem ursprünglichen verwendet wird), ist es jedoch möglich.

### <a name="constraint-priorities"></a>Einschränkung Prioritäten
In allen dieser Einschränkungen möglicherweise Denken Sie wurde haben "Hallo – ich denke Platzierung Einschränkungen der wichtigste in meinem System sind. Ich möchte an gegen örtlich anderen Einschränkungen, die gerade Aspekten wie Zugehörigkeit und Kapazität, wenn es ist sichergestellt, dass die Platzierung Einschränkungen jemals verletzt nicht zur Verfügung."

Gut herausstellt es, dass wir dies tun können! Einschränkungen können mit wenigen verschiedene Detailebenen Durchsetzung konfiguriert werden, aber sie Kernpunkte "harte" (0), "Weiche" (1), "Optimierung" (2), und "deaktiviert" (-1). Die meisten der Nebenbedingungen wir als harte standardmäßig definiert haben (da, beispielsweise die meisten Personen nicht normal überlegen Kapazität als etwas sie bereit sind, entspannen), und fast alle sind entweder harte oder weiche. Jedoch in erweiterte Situationen diese geändert werden können. Angenommen, was passiert, wenn Sie sicherstellen, dass die Zugehörigkeit würden immer um Knoten Kapazitätsprobleme lösen verletzt, können Sie die Priorität der Einschränkung Zugehörigkeit zu "Weiche" (1), und lassen Sie die Nebenbedingung Kapazität wollten legen Sie auf "Festplatte" (0). Die anderen Einschränkung Prioritäten sind auch warum Sie einige Einschränkung Violation Warnungen häufiger als andere - sehen, da es bestimmte Einschränkungen, die wir gibt, lockern bekannt sind (gegen örtlich) vorübergehend. Beachten Sie, dass diese Ebenen wirklich bedeuten nicht, dass eine gegebene Einschränkung werden oder wird nicht immer verletzt, nur auftritt Reihenfolge, in der sie sind vorzugsweise erzwungen, damit wir die richtigen vor-und Nachteile vornehmen können, ist es nicht möglich, die alle von ihnen erfüllen.

Die Konfiguration und Priorität von Standardwerten für die anderen Einschränkungen werden nachfolgend aufgeführt:

ClusterManifest.xml

```xml
        <Section Name="PlacementAndLoadBalancing">
            <Parameter Name="PlacementConstraintPriority" Value="0" />
            <Parameter Name="CapacityConstraintPriority" Value="0" />
            <Parameter Name="AffinityConstraintPriority" Value="0" />
            <Parameter Name="FaultDomainConstraintPriority" Value="0" />
            <Parameter Name="UpgradeDomainConstraintPriority" Value="1" />
            <Parameter Name="PreferredLocationConstraintPriority" Value="2" />
        </Section>
```

Sie sehen hier, dass es Einschränkungen für Upgrade und Fehlerstrukturanalyse Domänen definiert werden, und auch die Aktualisierung Domäne weiche wurde. Es gibt auch diese merkwürdig "PreferredLocation" Einschränkung mit einer Priorität. Was ist die von diesem Inhalten?

Deaktivieren modellieren wir zuerst den Wunsch Services unter anderem Fehlerstrukturanalyse und der Upgradeeinstellungen Domänen als Einschränkung innerhalb der Ressourcenmanager-Engine ausdehnen beibehalten. In der Vergangenheit gab es ein paar oft, wobei wir benötigten entweder zur Platzierung im Hinblick auf Fehler und der Upgradeeinstellungen Domänen wirklich strict abrufen und auch ein paar Fälle, in denen, enthielt einige Problem, wo wir gerade gebraucht Sie sie ignorieren möchten (kurz!) zwar vollständig, grundsätzlich wir haben mich für die Wahl Entwurf sowohl für die Flexibilität der Einschränkung Infrastruktur wurde. In den meisten Fällen befindet sich jedoch Upgrade Domäne Einschränkung als Einschränkung weiche, d. h., wenn der Ressourcenmanager vorübergehend einige Replikate in ein Upgrade Domäne packen, um, sagen Sie behandelt muss ein Upgrade vertraut, oder eine Reihe von gleichzeitigen Fehlern oder andere Einschränkung Verstöße (über die Festplatten Einschränkungen) klicken Sie dann, die ok ist. Dies wird nicht normalerweise geschehen, es sei denn, es gibt zahlreiche Fehlern oder andere Änderung im System verhindern die richtige Platzierung und die Umgebung ordnungsgemäß konfiguriert ist im Laufe Zustand ist immer Upgrade Domäne vollständig eingehalten werden.

Die Einschränkung PreferredLocation läuft ein wenig anders, und daher ist es der einzige Einschränkung "Optimierung" festlegen. Wir verwenden diese Einschränkung während Upgrades in Flight aus, und versuchen Sie, möchten Sie lieber mit Services zurück, in dem gefunden wir sie vor dem Upgrade. Alle Arten von Gründe warum dies funktioniert möglicherweise nicht in der Praxis, aber es sieht eine übersichtliche Optimierung, damit es gibt es hier befindet sich. Erfahren Sie mehr zu erhalten, wenn wir über Ressourcenmanager Cluster wie mit Upgrades unterstützt sprechen.

## <a name="upgrades"></a>Upgrades
Ressourcenmanager Cluster können auch während der Anwendung und Cluster-Upgrades, um sicherzustellen, dass das Upgrade reibungslos verläuft, sondern auch um sicherzustellen, dass die Regeln und Leistung von Cluster nicht beeinträchtigt werden.

### <a name="keep-enforcing-the-rules"></a>Beibehalten die Regeln erzwingen
Das wichtigste zu berücksichtigen ist, dass die Regeln – die strengen Einschränkungen um Dinge Platzierung wie Einschränkungen bei Upgrades weiterhin erzwungen werden. Sie glauben, dass dies ohne sagen geht, jedoch sind kurz gesagt: es trotzdem, einfach explizite dagegen werden. Die positive Seite diesen ist, können Sie sicherstellen, dass Falls gewünscht, um sicherzustellen, dass bestimmte Auslastung auf bestimmte Knoten ausführen nicht diese Regeln auch während der Aktualisierung automatisch erzwungen werden. Wenn Ihre Umgebung hochgradig beschränkt ist, kann dadurch Upgrades auf sehr lange dauern, da es gibt nur ein paar Optionen für die Stelle, an der ein Dienst wechseln können sie (oder die sie befindet sich auf Knoten) muss für ein Update zum Absturz bringen.

### <a name="smart-replacements"></a>Intelligentes Ersatz
Bei eine Aktualisierung beginnt, Ressourcenmanager eine Momentaufnahme der die aktuelle Anordnung von Cluster und versucht, Elemente in diesen Status zurückzugeben, wenn die Aktualisierung abgeschlossen ist. Die Logik hinter Dies ist einfach – zuerst stellt sicher, dass es als Teil des Upgrades nur einige Übergänge für jeden Dienst Replikat oder Instanz sind (Verschieben von den betroffenen Knoten und Verschieben wieder einzuchecken). Als Nächstes wird sichergestellt, dass es sich bei die Aktualisierung selbst viel Einfluss auf das Layout des Cluster aufweist; Wenn der Cluster auch vor dem Upgrade angeordnet wird es auch nach, oder mindestens keine schlechter angeordnet werden.

### <a name="reduced-churn"></a>Reduzierte Änderung
Die während des Upgrades passiert ist, dass der Cluster Ressourcenmanager Lastenausgleich für die zu aktualisierenden Entität deaktiviert. Ja, wenn Sie zwei verschiedene Anwendungsinstanzen haben, und beginnen Sie ein Upgrade auf eine von ihnen, ist dann Lastenausgleich für diese Anwendungsinstanz, aber nicht den anderen aus angehalten. Verhindern reaktivieren Lastenausgleich wird verhindert, dass unnötige Reaktionen zu des eigentlichen Upgrades ("Oh keine! Ein leerer Knoten! Besser füllen Sie es mit allen Arten von Inhalten!") und daher wird verhindert, dass in viele zusätzliche abgestimmt für Dienste im Cluster, die gerade würde müssen nicht rückgängig gemacht werden, wenn die Dienste müssen wieder zu den Knoten verschieben, nachdem die Aktualisierung abgeschlossen ist. Das Upgrade in Frage ist ein Upgrade Cluster, und der gesamte Cluster für Lastenausgleich für die Dauer der Aktualisierung angehalten ist (Einschränkung Prüfungen – Sicherstellung die Regeln werden erzwungen – aktiv bleiben, nur proaktive Qualifikationsprofilen ist deaktiviert).

### <a name="relaxed-rules"></a>Niedrige Regeln
Eines der Dinge, die während des Upgrades auftauchen ist im Allgemeinen, dass Sie möchten die Aktualisierung abgeschlossen ist, auch wenn Cluster insgesamt hat eine Einschränkung lieber oder vollständigen. Während des Upgrades ist ein wird die Kapazität der Cluster verwalten noch wichtiger als gewöhnlich, da es in der Regel zwischen 5 und 20 % der Kapazität ist nach unten jeweils während des Upgrades über die Cluster ein für, und die Arbeitsbelastung normalerweise weist um ein anderes zu wechseln. Hier kommt das Konzept eines [zwischengespeicherten Kapazität](service-fabric-cluster-resource-manager-cluster-description.md#buffered-capacity) wirklich ins Spiel – während der zwischengespeicherten Kapazität während des normalen Betriebs (verlassen zusätzlichen Aufwand) gewahrt wird, wird der Cluster Ressourcenmanager bei Upgrades bis zum die gesamte Kapazität (Aufzeichnen von Puffer) gefüllt.

## <a name="next-steps"></a>Nächste Schritte
- Beginnen Sie am Anfang und [erhalten Sie eine Einführung in die Ressourcenmanager Dienst Fabric Cluster](service-fabric-cluster-resource-manager-introduction.md)

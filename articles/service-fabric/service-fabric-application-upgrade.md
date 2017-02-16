<properties
   pageTitle="Upgrade der Fabric Service-Anwendung | Microsoft Azure"
   description="Dieser Artikel enthält eine Einführung in Aktualisieren einer Fabric Service-Anwendung, einschließlich Auswahl Upgrade Modi und Leistung integritätsprüfungen."
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


# <a name="service-fabric-application-upgrade"></a>Upgrade der Fabric Service-Anwendung

Eine Anwendung Azure Service Fabric ist eine Sammlung von Diensten. Während einer Aktualisierung Dienst Fabric vergleicht die neuen [Anwendungsmanifest](service-fabric-application-model.md#describe-an-application) mit der vorherigen Version und bestimmt, die in die Anwendung erforderlichen Updates services. Dienst Fabric vergleicht die Zahlen in den Dienst mit den Versionsnummern in der Vorgängerversion-Manifeste Version. Wenn ein Dienst nicht geändert hat, wird dieser Dienst nicht aktualisiert werden.

## <a name="rolling-upgrades-overview"></a>Paralleles Upgrades (Übersicht)

Bei einer aktiven Aktualisierung der Anwendung wird das Update in Phasen ausgeführt. In jeder Phase wird das Upgrade auf eine Teilmenge der Knoten im Cluster, eine Domäne aktualisieren aufgerufen angewendet. Daher bleibt die Anwendung in das Upgrade zur Verfügung. Während des Upgrades möglicherweise Cluster eine Mischung aus der alten und neuen Versionen enthalten.

Aus diesem Grund zwei Versionen muss vorwärts und rückwärts kompatibel. Wenn sie nicht kompatibel sind, ist der Anwendungsadministrator verantwortlich für das staging eines Upgrades Vielfache-Phase zum Verwalten der Verfügbarkeit. Ein Upgrade Vielfache-Phase aktualisieren im erste Schritt auf eine mittlere Version der Anwendung, die mit der vorherigen Version kompatibel ist. Im zweite Schritt ist die endgültige Version aktualisieren, die Kompatibilität mit der Version vor dem Update Umbrüche, aber ist mit der mittlere Version kompatibel.

Update Domänen werden im Cluster Manifest angegeben, wenn Sie den Cluster konfigurieren. Aktualisieren von Domänen erhalten Updates nicht in einer bestimmten Reihenfolge. Eine Update-Domäne ist eine logische Einheit der Bereitstellung für eine Anwendung. Update Domänen zulassen der Dienste bei hohen Verfügbarkeit bei einer Aktualisierung bleiben.

Wenn Sie das Upgrade auf allen Knoten im Cluster, angewendet wird, was der Fall ist, wenn die Anwendung nur eine Update-Domäne verfügt, sind nicht Aktualisierungen möglich. Dieser Ansatz wird nicht empfohlen, da der Dienst fällt aus, und ist nicht verfügbar zum Zeitpunkt der Aktualisierung. Darüber hinaus zur nicht Azure alle Garantien Verfügung, wenn ein Cluster mit nur einem einzigen Update Domäne eingerichtet ist.

## <a name="health-checks-during-upgrades"></a>Integritätsprüfungen bei upgrades

Bei einer Aktualisierung Gesundheit Richtlinien festgelegt werden müssen (oder Standardwerten verwendet werden können). Ein Upgrade spricht erfolgreich, wenn alle aktualisieren Domänen innerhalb der angegebenen Timeouts aktualisiert werden, oder wenn alle aktualisieren Domänen als sicher eingestuft werden.  Eine Domäne fehlerfrei aktualisieren bedeutet, dass die Domäne aktualisieren alle in der Richtlinie Gesundheit angegebenen integritätsprüfungen übergeben. Beispielsweise möglicherweise eine Richtlinie Gesundheit festgelegt werden, dass alle Dienste in eine Instanz der Anwendung *fehlerfrei*, sein müssen wie Gesundheit vom Dienst Fabric definiert ist.

Richtlinien für Gesundheit und Prüfungen während des Upgrades vom Dienst Fabric sind Dienstes sowie die Anwendung unabhängig. D. h., werden keine Service-spezifische Tests fertig.  Angenommen, der Dienst möglicherweise eine Durchsatzes, aber Dienst Fabric hat keinen die Informationen Durchsatz überprüfen. Schlagen Sie in den [Artikeln Gesundheit](service-fabric-health-introduction.md) für die Prüfungen, die ausgeführt werden. Die Prüfungen, die während eines Upgrades einschließen Tests für, ob das Anwendungspaket korrekt, kopiert wurde ausgeführt werden, ob die Instanz gestartet wurde, und So weiter.

Die Anwendung Integrität ist eine Aggregation der untergeordneten Elemente der Anwendung. Kurz gesagt, wertet Dienst Fabric die Integrität des der Anwendung über die Integrität, die für die Anwendung gemeldet wird. Es werden auch die Integrität des alle Dienste für die Anwendung auf diese Weise. Weiteren Dienst Fabric wertet die Integrität des die Anwendungsdienste durch die Integrität ihrer untergeordneten Elemente, wie etwa das Replikat Dienst aggregieren. Nachdem Sie die Anwendung Gesundheit Richtlinie erfüllt ist, kann das Upgrade fortgesetzt werden. Wenn die Integritätsrichtlinie verletzt wird, schlägt die Aktualisierung der Anwendung.

## <a name="upgrade-modes"></a>Upgrade Modi

Der Modus, in dem es wird, für die Aktualisierung der Anwendung empfohlen wird der überwachten Modus die häufig verwendete Modus ist. Überwachten Modus führt das Upgrade auf eine Update-Domäne, und wenn alle Gesundheit überprüft Pass (gemäß der Richtlinie angegeben) bewegt auf auf die nächste Update Domäne automatisch.  Wenn ein Fehler, integritätsprüfungen auftreten und/oder Timeouts erreicht werden, das Upgrade ist entweder für die Domäne aktualisieren rückgängig gemacht oder der Modus auf nicht überwacht manuell geändert wird. Sie können das Upgrade, damit diese zwei Modi für fehlgeschlagene Upgrades auswählen konfigurieren. 

Nicht überwacht manuellen Modus benötigt manuellen Eingriff nach jeder Upgrade auf eine Domäne aktualisieren zum Deaktivieren des Upgrades auf die nächste Update Domäne Starten eines. Keine Fabric Service-integritätsprüfungen werden ausgeführt. Der Administrator führt Prüfungen Zustand oder Status vor der Aktualisierung in der nächsten Domäne aktualisieren.

## <a name="application-upgrade-flowchart"></a>Upgrade Anwendungsflussdiagramm

Folgen diesem Absatz Flussdiagramm helfen Ihnen den Upgradevorgang einer Fabric Service-Anwendung zu verstehen. Insbesondere beschrieben, illustrieren wie die Timeouts, einschließlich *HealthCheckStableDuration*, *HealthCheckRetryTimeout*und *UpgradeHealthCheckInterval*, Steuerelement bei die Aktualisierung in eine Update-Domäne einen Erfolg oder ein Fehler gilt.

![Den Upgradevorgang für eine Fabric Service-Anwendung][image]


## <a name="next-steps"></a>Nächste Schritte

[Aktualisieren Ihrer Anwendung mithilfe von Visual Studio](service-fabric-application-upgrade-tutorial.md) führt Sie durch eine Anwendung Aktualisierung mit Visual Studio.

[Aktualisieren Ihrer Anwendung mithilfe von Powershell](service-fabric-application-upgrade-tutorial-powershell.md) führt Sie durch eine Anwendung Aktualisierung mithilfe der PowerShell.

Steuern Sie, wie eine Anwendung upgrades mithilfe von [Parametern zu aktualisieren](service-fabric-application-upgrade-parameters.md).

Machen Sie Ihre Upgrades der Anwendung kompatibel, indem Sie lernen, wie Sie [Datenserialisierung](service-fabric-application-upgrade-data-serialization.md)verwenden.

Informationen Sie zum erweiterten Funktionen, die während des Upgrades Ihrer Anwendungs von Verweisen auf [Erweiterte Themen](service-fabric-application-upgrade-advanced.md)verwenden.

Beheben von häufig auftretenden Problemen in Upgrades der Anwendung von Verweisen auf die Schritte in der [Anwendungsupgrades Problembehandlung](service-fabric-application-upgrade-troubleshooting.md).
 


[image]: media/service-fabric-application-upgrade/service-fabric-application-upgrade-flowchart.png

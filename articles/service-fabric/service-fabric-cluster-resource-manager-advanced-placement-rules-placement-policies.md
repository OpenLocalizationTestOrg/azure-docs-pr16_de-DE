<properties
   pageTitle="Dienst Fabric Cluster Ressourcenmanager - Platzierungsrichtlinien | Microsoft Azure"
   description="Übersicht über weitere Platzierungsrichtlinien und Regeln für Dienst Fabric Services"
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

# <a name="placement-policies-for-service-fabric-services"></a>Platzierungsrichtlinien für Dienst Fabric services
Es gibt viele verschiedene zusätzliche Regeln, die Sie über eine Rolle spielt, wenn Sie Ihren Cluster Service Fabric erstreckt ist einhandeln über einem geografischen Abstände, sagen Sie mehrere Rechenzentren oder Azure Regionen, oder wenn Ihre Umgebung mehrere Bereiche geopolitische Steuerelement (oder einige andere Fall umfasst, in dem Sie Legal oder Richtlinie Grenzen, die Sie interessiert haben oder die Abstände die tatsächliche Leistung Wartezeit für wirken sich). Die meisten dieser über die Eigenschaften des Knotens und Platzierung Einschränkungen konfiguriert werden, jedoch sind einige komplizierter. Um Punkte zu vereinfachen stellen wir diese zusätzlichen Commmands. Wie können Platzierungsrichtlinien mit anderen Einschränkungen Platzierung auf Basis Instanz pro benannte Dienst konfiguriert sein.

## <a name="specifying-invalid-domains"></a>Ungültige Domänen angeben
Die InvalidDomain Platzierungsrichtlinie können Sie angeben, dass eine bestimmte Fehlerstrukturanalyse-Domäne für diese Arbeitsbelastung ungültig ist. Dieser Richtlinie wird sichergestellt, dass in einem bestimmten Bereich, beispielsweise Gründen geopolitische oder corporate Richtlinie nie ein bestimmter Dienst ausgeführt wird. Über separate Richtlinien können mehrere ungültige Domänen angegeben werden.

![Beispiel für ungültige Domäne][Image1]

Code:

```csharp
ServicePlacementInvalidDomainPolicyDescription invalidDomain = new ServicePlacementInvalidDomainPolicyDescription();
invalidDomain.DomainName = "fd:/DCEast"; //regulations prohibit this workload here
serviceDescription.PlacementPolicies.Add(invalidDomain);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 2 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("InvalidDomain,fd:/DCEast”)
```
## <a name="specifying-required-domains"></a>Angeben von erforderlichen Domänen
Die erforderliche Domäne Platzierungsrichtlinie erfordert, dass alle dynamische Replikate oder statusfreie Dienstinstanzen für den Dienst in der angegebenen Domäne vorhanden sein. Über separate Richtlinien können mehrere erforderliche Domänen angegeben werden.

![Beispiel für die erforderliche Domäne][Image2]

Code:

```csharp
ServicePlacementRequiredDomainPolicyDescription requiredDomain = new ServicePlacementRequiredDomainPolicyDescription();
requiredDomain.DomainName = "fd:/DC01/RK03/BL2";
serviceDescription.PlacementPolicies.Add(requiredDomain);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 2 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("RequiredDomain,fd:/DC01/RK03/BL2")
```

## <a name="specifying-a-preferred-domain-for-the-primary-replicas"></a>Angeben eines bevorzugten Domänennamens für die primäre Replikate
Die bevorzugte primäre Domäne ist ein interessantes Steuerelement, da dies Auswahl der Domäne Fehlerstrukturanalyse zulässt, in dem die primäre platziert werden soll, wenn es möglich ist, können. Wenn alle fehlerfrei ist wird die primäre in dieser Domäne einhandeln. Sollten die Domäne oder die Fail primäres Replikat oder aus irgendeinem Grund, dass die primäre an einen anderen Speicherort migriert werden beendet werden. Wenn diesen Speicherort in der bevorzugten Domäne nicht, wird dann nach Möglichkeit Ressourcenmanager Cluster es wieder auf die bevorzugte Domäne verschoben. Natürlich sinnvoll diese Einstellung nur für dynamische Dienste. Diese Richtlinie ist besonders hilfreich in Cluster die über Azure Regionen oder mehrere Rechenzentren erstreckt. In den folgenden Situationen Sie alle Speicherorte Redundanzgründen verwenden aber lieber, dass die primäre Replikate an einem bestimmten Speicherort platziert werden, um unteren Wartezeit für Vorgänge bereitstellen, die mit dem primären wechseln (schreibt und standardmäßig werden alle liest auch durch die primäre served).

![Bevorzugte primären Domänen und Failover][Image3]

```csharp
ServicePlacementPreferPrimaryDomainPolicyDescription primaryDomain = new ServicePlacementPreferPrimaryDomainPolicyDescription();
primaryDomain.DomainName = "fd:/EastUS/";
serviceDescription.PlacementPolicies.Add(invalidDomain);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 2 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("PreferredPrimaryDomain,fd:/EastUS")
```

## <a name="requiring-replicas-to-be-distributed-among-all-domains-and-disallowing-packing"></a>Mit Anforderung der Replikate zwischen allen Domänen und Deaktivierung Verpackung verteilt werden soll
Eine andere Richtlinie, die Sie angeben können, besteht darin, erfordern Replikate immer zwischen den verfügbaren Fehlerstrukturanalyse-Domänen verteilt werden soll. Dies tritt standardmäßig in den meisten Fällen, wo finde ich der Cluster fehlerfrei, stehen jedoch fehlerhaften Fällen, wo Replikate für eine bestimmte Partition vorübergehend einhandeln möglicherweise verpackt in einer einzelnen Fehlerstrukturanalyse oder Upgrade Domäne sind. Beispielsweise angenommen, die zwar der Cluster hat 9-Knoten in 3 Fehlerstrukturanalyse-Domänen (0, 1 und 2), und der Dienst über 3 Replikate verfügt, ausgefallen Knoten, die für diese Replikate Fehlerstrukturanalyse Domänen 1 und 2 verwendet wurden und aufgrund von Problemen Kapazität keine anderen Knoten in diesen Domänen gültig waren. Wäre Dienst Fabric Ersatz für diese Replikate zu erstellen, der Cluster Ressourcenmanager nutzen, müssen Sie diese in Fehlerstrukturanalyse-Domäne 0 setzen, aber, die eine Situation Stelle, an der Verletzung von der Beschränkung Fehlerstrukturanalyse-Domäne erstellt. Es erhöht auch, dass die gesamte Replikatgruppe verloren werden konnte (wäre FD 0 sein Permananently verloren gegangen sind). (Weitere Informationen zu Einschränkungen und Einschränkung Auschecken Prioritäten im allgemeinen [in diesem Thema](service-fabric-cluster-resource-manager-management-integration.md#constraint-priorities) )

Wenn Sie eine Warnung Gesundheit wie je gesehen haben "Lastenausgleich hat eine Einschränkung-Verletzung für dieses Replikat: Fabric festgestellt: /<some service name> sekundären Partition <some partition ID> ist die Einschränkung verletzt: FaultDomain" Sie haben erreichen, diese Bedingung oder ungefähr wie es. Sind in der Regel folgenden Situationen vorübergehend (nicht die Knoten unten lange bleiben, oder wenn Kontrollkästchenoptionen und zu Beginn wir erstellen müssen sind Ersatz es andere Knoten in der richtigen Fehlerstrukturanalyse-Domänen die gültig sind), aber es gibt einige Auslastung, die lieber Verfügbarkeit für das Risiko, verlieren alle ihre Replikationen Handel würden. Hierzu können wir angeben "RequireDomainDistribution" Richtlinien für den sichergestellt ist, dass keine zwei Replikate aus dem gleichen-Abschnitt jemals in derselben Domäne Fehlerstrukturanalyse oder Upgrade zulässig sind.

Einige Auslastung müssten die Ziel-Anzahl der Replikationen (Kopien von Bundesstaat) lieber gar-Zeiten (zu diesem Zweck für insgesamt Domäne Fehlern und wissen, dass sie normalerweise lokalen Zustand wiederhergestellt werden können), während andere lieber der Ausfall zuvor die Richtigkeit und Products Aspekte enttäuschen ausführen möchten. Da die meisten Produktionsarbeitslasten mit mehr als 3 Replikaten ausführen zu können, ist der Standardwert nicht erforderlich Verteilung der Domäne und dem Benutzer Lastenausgleich und Failover Fälle, auch wenn dies bedeutet, dass vorübergehend eine Domäne mehrere Replikate darin verpackt weist normalerweise zu behandeln.

Code:

```csharp
ServicePlacementRequireDomainDistributionPolicyDescription distributeDomain = new ServicePlacementRequireDomainDistributionPolicyDescription();
serviceDescription.PlacementPolicies.Add(distributeDomain);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 2 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("RequiredDomainDistribution")
```

Nun ist es möglich verwenden diese Konfigurationen für Dienste in einem Cluster nicht geografischen erstreckt? Sicher können Sie folgende Aktionen! Aber es kein guten Grund auch gibt – besonders die benötigten, ungültige und bevorzugte Domänenkonfigurationen vermieden werden, sollte es sei denn, Sie sind tatsächlich ausgeführt, einen geografischen übergreifenden Cluster – es keine sinnvoll, versuchen, eine bestimmte Auslastung in einer einzelnen den Shapes für Gestelle ausführen oder einige Ihrer lokalen Cluster Segment über ein anderes bevorzugen, es sei denn, es gibt verschiedene Typen von Hardware oder Arbeitsbelastung Segmentierung vertraut auf erzwingen , und diesen Fällen können über normalen Platzierung Einschränkungen behandelt werden.

## <a name="next-steps"></a>Nächste Schritte
- Für Weitere Informationen zu anderen Optionen für das Konfigurieren verfügbar Auschecken des Themas auf die anderen Cluster Ressourcenmanager Konfigurationen services verfügbare [Informationen zum Konfigurieren von Diensten](service-fabric-cluster-resource-manager-configure-services.md)

[Image1]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-invalid-placement-domain.png
[Image2]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-required-placement-domain.png
[Image3]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-preferred-primary-domain.png

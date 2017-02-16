<properties
   pageTitle="Dienst Fabric Cluster Ressourcenmanager - Zugehörigkeit | Microsoft Azure"
   description="Übersicht über die Zugehörigkeit für Dienst Fabric Dienste konfigurieren"
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

# <a name="configuring-and-using-service-affinity-in-service-fabric"></a>Konfigurieren und Verwenden von Dienst Zugehörigkeit Dienst Struktur

Zugehörigkeit ist ein Steuerelement, die hauptsächlich bereitgestellt wird, helfen den Übergang der größere monolithische Anwendungen in der Cloud und Microservices weltweit zu erleichtern. Dies bedeutet, dass es auch in bestimmten Fällen als eine seriösen Optimierung verwendet werden kann zum Verbessern der Leistung von Diensten, obwohl diese Seite Effekte haben kann.

Angenommen, Sie einbinden sind eine größere app oder eine, die nicht nur mit Microservices berücksichtigen, um Dienst Fabric konzipiert. Übergang tatsächlich allgemeine ist, und wir haben mehrere Kunden (internen und externen) in diesem Fall hatten. Starten Sie heben die gesamte App in das erste es verpackt-Umgebung und ausführen. Beginnen Sie unten in verschiedenen kleinere Diensten, dass alle miteinander sprechen unterteilt wird.

Es folgt eine "Oops...". Die "Oops" kann in der Regel in eine der folgenden Kategorien:

1. Einige Komponente X in der app monolithische hatte eine nicht dokumentierte Abhängigkeit auf Komponente Y, und wir die in den einzelnen Diensten nur aktiviert. Da diese jetzt auf verschiedenen Knoten im Cluster ausgeführt werden, sind diese fehlerhafte.
2.  Diese Dinge Kommunikation über (lokale named Pipes | freigegebenen Speicher | Dateien auf einem Datenträger), aber ich wirklich benötigen, kann es Vorgang zu beschleunigen ein bisschen unabhängig voneinander zu aktualisieren. Ich werde später harte Abhängigkeit entfernen.
3.  Alles ist in Ordnung, aber wie sich herausstellt, dass diese zwei Komponenten tatsächlich sehr detailliert, sondern grob/Leistung vertrauliche. Wenn sie diese in separate Dienste verschoben wurde insgesamt die Leistung der Anwendung tanked oder Wartezeit erhöht. Daher ist die gesamte Anwendung nicht erwartet Besprechung.

In diesen Fällen wir nicht unsere Umgestaltung Arbeit verloren geht, und nicht die Monolith zurückkehren möchten, aber benötigen wir einige Verständlichkeit Ort. Dies wird beibehalten, bis wir die Komponenten entwickelt natürlich als Services Umgestaltung können oder bis wir die erwartete Leistung andere Weise, falls möglich gelöst werden können.

Was zu tun ist? Auch könnten Sie versuchen, Zugehörigkeit einschalten.

## <a name="how-to-configure-affinity"></a>So konfigurieren Sie die Zugehörigkeit
Um die Zugehörigkeit eingerichtet haben, definieren Sie eine Zugehörigkeit Beziehung zwischen zwei verschiedenen Diensten. Sie können sich vorstellen der Zugehörigkeit wie "zeigen" einen Dienst an einem anderen und sagen "diesen Dienst nur ausgeführt werden kann, dass Dienst ausgeführt wird." Manchmal finden Sie wir Zugehörigkeit als Beziehung zwischen übergeordneten und untergeordneten (Stelle, an der Sie das untergeordnete am übergeordneten zeigen). Zugehörigkeit wird sichergestellt, dass die Replikate oder Instanzen von einem Dienst auf den gleichen Knoten wie die Replikate oder eines anderen Instanzen platziert werden.

``` csharp
ServiceCorrelationDescription affinityDescription = new ServiceCorrelationDescription();
affinityDescription.Scheme = ServiceCorrelationScheme.Affinity;
affinityDescription.ServiceName = new Uri("fabric:/otherApplication/parentService");
serviceDescription.Correlations.Add(affinityDescription);
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

## <a name="different-affinity-options"></a>Optionen für verschiedene Zugehörigkeit
Zugehörigkeit per eines mehrere Korrelationskoeffizienten Schemas dargestellt ist, und gibt es zwei verschiedene Modi. Der am häufigsten verwendete Modus der Zugehörigkeit ist wir NonAlignedAffinity anrufen. Die Replikate oder Instanzen von den verschiedenen Diensten werden NonAlignedAffinity auf denselben Knoten platziert. Der andere Modus kommt AlignedAffinity an. Ausgerichtet Zugehörigkeit eignet sich nur auf eine dynamische Dienste. Konfigurieren von zwei dynamische Services, um die Zugehörigkeit ausgerichtet haben: Damit ist sichergestellt, dass die Grundfarben dieser Dienste auf den gleichen Knoten als untereinander platziert werden. Veranlasst auch jedes Paar der sekundäre für Dienste auf denselben Knoten platziert werden. Es ist auch so konfigurieren Sie NonAlignedAffinity für dynamische Dienste (obwohl weniger Allgemein) möglich. Für NonAlignedAffinity würde die anderen Replikate der beiden dynamische Dienste auf denselben Knoten zusammengestellt werden, aber nicht versucht würde So richten Sie ihre Primärfarben oder sekundäre aus.

![Zugehörigkeit Modi und deren Effekten][Image1]

### <a name="best-effort-desired-state"></a>Optimale Leistung gewünscht Zustand
Es gibt einige Unterschiede zwischen Zugehörigkeit und monolithische Architekturen aus. Viele davon sind, weil eine Beziehung Zugehörigkeit optimale Leistung ist. Die Dienste in einer Beziehung Zugehörigkeit sind grundlegend Bereiche, die können ein Fehler auftreten und unabhängig voneinander verschoben werden. Es gibt auch Ursachen für warum eine Beziehung Zugehörigkeit unterbrechen könnte. Beispielsweise Kapazität Einschränkungen, in dem nur einige der Dienst Objekte in der Beziehung Zugehörigkeit auf einem bestimmten Knoten dargestellt werden können. In diesen Fällen zwar eine Zugehörigkeit Beziehung an Ort steht, kann es aufgrund der anderen Einschränkungen erzwungen werden. Ist es möglich, alle anderen Einschränkungen und Zugehörigkeit zu einem späteren Zeitpunkt erzwingen wird der Verstoß gegen die Zugehörigkeit Einschränkung automatisch korrigiert werden.  

### <a name="chains-vs-stars"></a>Ketten im Vergleich zu Sterne
Heute können wir nicht Modell sodass Zugehörigkeit Beziehungen. Was dies bedeutet, die einen Dienst ist, der ein untergeordnetes Element in einem Zugehörigkeit Beziehung sein kein übergeordnetes Element in einer anderen Zugehörigkeit Beziehung. Wenn Sie diese Art von Beziehung modellieren möchten, müssen Sie effektiv es als einen Stern, statt eine Kette modellieren. Um dies zu tun, würde die unterste untergeordneten der "Mittlere" übergeordneten Elements stattdessen übergeordnet werden. Je nach der Anordnung Ihrer Dienste kann erforderlich sein Erstellen eines Diensts "Platzhalter" als das übergeordnete Element für mehrere untergeordnete dienen soll.

![Ketten im Vergleich zu Sterne im Kontext Zugehörigkeit Beziehungen][Image2]

Eine andere ist zu Zugehörigkeit Beziehungen heute zu beachten, dass sie gerichtete sind. Dies bedeutet, dass die Regel "Zugehörigkeit" nur erzwingt, dass die untergeordneten ist, in dem das übergeordnete Element ist. Wenn beispielsweise die übergeordnete plötzlich über auf einen anderen Knoten fehlschlägt denken nicht dann Ressourcenmanager Cluster tatsächlich, dass nichts falschen vorhanden ist, bis es bemerkt, dass die untergeordneten nicht mit einem übergeordneten gefunden wird; die Beziehung wird nicht sofort erzwungen.

### <a name="partitioning-support"></a>Partitionierungsunterstützung
Über die Zugehörigkeit zu beachten Sie die endgültige als ist die Zugehörigkeit, die Beziehungen nicht unterstützt werden, in dem die übergeordnete konfiguriert ist. Dies ist ein Element, das wir möglicherweise später unterstützt, aber heute ist nicht zulässig.

## <a name="next-steps"></a>Nächste Schritte
- Für Weitere Informationen zu anderen Optionen für das Konfigurieren verfügbar Auschecken des Themas auf die anderen Cluster Ressourcenmanager Konfigurationen services verfügbare [Informationen zum Konfigurieren von Diensten](service-fabric-cluster-resource-manager-configure-services.md)
- Viele Gründe, wo Zugehörigkeit benutzt, z. B. Dienste für ein kleines einschränken, Maschinen festlegen und versuchen, die Laden von eine Sammlung von Diensten, aggregieren werden durch die Anwendungsgruppen besser unterstützt. Schauen Sie sich die [Anwendungsgruppen](service-fabric-cluster-resource-manager-application-groups.md)

[Image1]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-affinity/cluster-resrouce-manager-affinity-modes.png
[Image2]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-affinity/cluster-resource-manager-chains-vs-stars.png

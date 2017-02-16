<properties
   pageTitle="Verwalten von Kriterien mit Azure Service Fabric Cluster Ressourcenmanager | Microsoft Azure"
   description="Informationen Sie zum Konfigurieren und Verwenden von Kriterien Service-Struktur."
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

# <a name="managing-resource-consumption-and-load-in-service-fabric-with-metrics"></a>Verwalten von Ressourcenverbrauch und Laden Dienst Struktur mit Kennzahlen
Kennzahlen sind die allgemeinen Begriff in Fabric-Dienst für die Ressourcen, die Dienste gepflegt zu und die von den Knoten im Cluster bereitgestellt werden. Im Allgemeinen ist eine Metrik alles, was Sie akzeptieren, um die Leistung Ihrer Dienste behandelt verwalten möchten.

Dinge wie Arbeitsspeicher, Datenträger, CPU-Auslastung – all dies sind Beispiele für Kriterien. Hierbei handelt es sich um physische Kennzahlen, Ressourcen, die entsprechen physischen Ressourcen auf dem Knoten, die verwaltet werden müssen. Kennzahlen können außerdem sein (und häufig sind) logische Kennzahlen, Aspekten wie "MyWorkQueueDepth", die Anwendung definiert sind und die zu einem gewissen Grad Ressourcenverbrauch entsprechen (aber, in dem oder der Anwendung nicht wirklich wissen sie wissen, wie Sie es Messen). Die meisten Kennzahlen, die wir verwenden Personen angezeigt werden logische Kennzahlen. Es gibt verschiedene Gründe dafür, aber der am häufigsten verwendeten ist, dass heute viele unserer Kunden ihre Dienste in verwaltetem Code schreiben, von innerhalb einer bestimmten statusfreie Dienstinstanz oder dynamische Dienst Replikatobjekt ist eigentlich ziemlich schwierig sein, messen und Ihre Verbrauch der tatsächlichen physischen Ressourcen melden. Die Komplexität der reporting-eigene Metrik ist auch an, warum wir einige Standard-Metrik im Paket bereitstellen.

## <a name="default-metrics"></a>Standard-Kennzahlen
Lassen Sie uns angenommen, Sie nur die ersten Schritte möchten und nicht wissen, welche Ressourcen, die Sie nutzen oder sogar, welche für Sie wichtigen wäre abgelegt werden. So wechseln Sie implementieren und erstellen dann ohne Angabe Metrik Ihrer Dienste. Das ist in Ordnung! Wir erhalten Sie einige Kennzahlen auswählen. Der Standard-Metrik, die wir für Sie heute verwenden, wenn Sie eine eigene nicht angeben, werden PrimaryCount, ReplicaCount, bezeichnet und (etwas vaguely, wir einreichen) zählen. Die folgende Tabelle zeigt, wie viel Laden für jeden der folgenden Kennzahlen jedes Dienstobjekt zugeordnet ist:

| Metrisch | Statusfreie Instanz laden |    Dynamische sekundäre laden |   Dynamische primären laden |
|--------|--------------------------|-------------------------|-----------------------|
| PrimaryCount | 0 |    0 | 1 |
| ReplicaCount | 0  | 1 | 1 |
| Zählen |   1 | 1 | 1 |

OK, also mit folgenden standardmäßigen Kennzahlen, was erhalten Sie? Gut herausstellt es, dass für grundlegende Auslastung eine gute Verteilung der Arbeit werden. In diesem Beispiel unten uns finden Sie unter Was passiert, wenn wir eine dynamische Dienst mit drei Partitionen erstellen und eine Zielreplikat Festlegen von Größenänderungs-drei auch einen einzelnen statusfreie Dienst mit einer Anzahl der Instanzen von drei - erhalten Sie ungefähr wie folgt aus!

![Cluster Layout mit Standard-Kennzahlen][Image1]

In diesem Beispiel sehen wir
-   Primäre Replikate für den Dienst dynamische werden nicht auf einem einzelnen Knoten gestapelte, nach oben
-   Replikate für diese Partition sind nicht auf dem gleichen Knoten
-   Die Gesamtzahl der Primärfarben und sekundäre wird auch im Cluster verteilt.
-   Die Gesamtzahl der Dienst Objekte (statusfreie und dynamische) werden auf den einzelnen Knoten gleichmäßig zugewiesen.

Gar nicht so schlecht!  

Dies funktioniert hervorragend, bis Sie beginnen, denken: Was ist die Wahrscheinlichkeit, die das Partitionierungsschema Ihnen ausgewählte durch alle Partitionen über einen Zeitraum in ganz gerade Auslastung Ergebnis wird? In Verbindung mit dem, was ist die Wahrscheinlichkeit, dass die Last für einen bestimmten Dienst ist konstant über einen Zeitraum oder sogar genauso sofort? Verwandelt, verkleinern für alle schwerwiegenden Arbeitsbelastung die Chancen aller Replikate gleichwertig ist tatsächlich eher niedrig, wenn Sie die optimale Nutzung Ihrer Cluster interessiert Sie wahrscheinlich müssen beginnen Sie Ihre Suche in benutzerdefinierten Metrik.

Realistischerweise, können Sie mit nur die standardmäßigen Kennzahlen absolut ausführen, aber dazu in der Regel bedeutet, dass Ihre Nutzung Cluster Sie liegt möchte (da Berichterstattung ist nicht adaptive und setzt voraus, dass alles entspricht); in den ungünstigsten Fall können sie auch overscheduled Knoten in Leistungsprobleme resultierender führen. Wir können mehr erreichen mit benutzerdefinierten Metrik und dynamische Laden-Berichte als Nächstes befassen.

## <a name="custom-metrics"></a>Benutzerdefinierte Metrik
Wir haben bereits erläutert, dass es kann physische und logische Kennzahlen und, um Personen festzulegen, deren eigenen Kriterien. Großartige! Aber wie? Die Wahrscheinlichkeit ist eigentlich ziemlich einfach! Nur die Metrik und die standardmäßigen anfänglichen Laden konfigurieren, wenn vom Dienst erstellt und war's schon! Eine Wertemenge Metrik und Standard darstellt, wie viel der Dienst erwartet wird, zu nutzen kann auf Basis pro mit dem Namen-Service-Instanz konfiguriert werden, wenn Sie den Dienst erstellen.

Beachten Sie, dass beim Starten von benutzerdefinierten Metrik definieren müssen Sie explizit wieder in die standardmäßige Metrik hinzufügen, wenn Sie uns, die sie verwenden, um dem Dienst auch verteilen möchten. Dies ist, da wir Sie sein, dass Sie belegt oder WorkQueueDepth Möglichkeit mehr als Sie interessieren primären Verteilung interessieren vielleicht über die Beziehung zwischen den standardmäßigen Metrik und benutzerdefinierten metrischen – löschen möchten.

Angenommen, Sie möchten einen Dienst Konfigurieren der eine Metrik namens "Arbeitsspeicher" (zusätzlich zu den standardmäßigen Kennzahlen) gemeldet möchten. Lassen Sie für den Speicher uns angenommen, Sie einige grundlegende Maße vorgenommen haben und wissen, dass die primäre Kopie diesen Dienst dauert in der Regel 20 MB Arbeitsspeicher, während der sekundäre für diesen Dienst 5 Mb in Anspruch nehmen werden. Sie wissen, dass Arbeitsspeicher ist der wichtigste Metrik im Hinblick auf die Leistung von diesen bestimmten Dienst verwalten, aber Sie weiterhin primären Replikate verteilt werden, damit der Verlust der einige Knoten oder Fehlerstrukturanalyse-Domäne eine Vielzahl von primären Replikate zusammen mit dem nutzen nicht verwenden möchten. Zahlen, die nun werfen Sie die Standardeinstellungen.

Hier ist, was Sie tun möchten:

Code:

```csharp
StatefulServiceDescription serviceDescription = new StatefulServiceDescription();
StatefulServiceLoadMetricDescription memoryMetric = new StatefulServiceLoadMetricDescription();
memoryMetric.Name = "MemoryInMb";
memoryMetric.PrimaryDefaultLoad = 20;
memoryMetric.SecondaryDefaultLoad = 5;
memoryMetric.Weight = ServiceLoadMetricWeight.High;

StatefulServiceLoadMetricDescription primaryCountMetric = new StatefulServiceLoadMetricDescription();
primaryCountMetric.Name = "PrimaryCount";
primaryCountMetric.PrimaryDefaultLoad = 1;
primaryCountMetric.SecondaryDefaultLoad = 0;
primaryCountMetric.Weight = ServiceLoadMetricWeight.Medium;

StatefulServiceLoadMetricDescription replicaCountMetric = new StatefulServiceLoadMetricDescription();
replicaCountMetric.Name = "ReplicaCount";
replicaCountMetric.PrimaryDefaultLoad = 1;
replicaCountMetric.SecondaryDefaultLoad = 1;
replicaCountMetric.Weight = ServiceLoadMetricWeight.Low;

StatefulServiceLoadMetricDescription totalCountMetric = new StatefulServiceLoadMetricDescription();
totalCountMetric.Name = "Count";
totalCountMetric.PrimaryDefaultLoad = 1;
totalCountMetric.SecondaryDefaultLoad = 1;
totalCountMetric.Weight = ServiceLoadMetricWeight.Low;

serviceDescription.Metrics.Add(memoryMetric);
serviceDescription.Metrics.Add(primaryCountMetric);
serviceDescription.Metrics.Add(replicaCountMetric);
serviceDescription.Metrics.Add(totalCountMetric);

await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 2 -TargetReplicaSetSize 3 -PartitionSchemeSingleton –Metric @("Memory,High,20,5”,"PrimaryCount,Medium,1,0”,"ReplicaCount,Low,1,1”,"Count,Low,1,1”)
```

(Erinnerung: Wenn Sie die standardmäßigen Metrik verwenden möchten, müssen Sie nicht tippen Sie auf die Sammlung Kennzahlen gar oder besonderen Schritte beim Erstellen des Diensts.)

Jetzt, da wir Sie definieren eigener Kennzahlen gelernt haben, Wissenswertes über die anderen Eigenschaften, die Kennzahlen enthalten können. Wir haben sie bereits für Sie angezeigt, aber es ist Zeit für zunächst tatsächlich Mittelwert! Es gibt vier verschiedene Eigenschaften, die eine Metrik heute haben kann:

-   Metrische Name: Dies ist der Name der Metrik. Dies ist ein eindeutiger Bezeichner für die Metrik im Cluster aus der Ressourcenmanager Perspektive.
- Standard-laden: Die Standardeinstellung laden wird je nachdem, ob der Dienst statusfreie oder dynamische anders dargestellt.
  - Für zustandsloser Dienste weist jede Metrik nur eine einzelne Eigenschaft namens Standard laden
  - Für dynamische Dienste definieren Sie können
    -   PrimaryDefaultLoad: Die Standarddaten laden, die diesen Dienst für diese Metrik als primärer gewährt wird
    -   SecondaryDefaultLoad: Die Standarddaten laden, die diesen Dienst für diese Metrik als sekundäre Replikat gewährt werden  
-   Gewicht: Dies ist wie wichtig die Metrik relativ zum die anderen konfigurierten Metrik für diesen Dienst an.

## <a name="load"></a>Beim Laden
Beim Laden ist das allgemeine Konzept von wie viel von einer Metrik von einigen Dienstinstanz oder auf einen bestimmten Knoten verwendet wird.

## <a name="default-load"></a>Standard-laden
Standard-Laden ist ähnlich wie beim Laden der Ressourcenmanager Cluster sollten jede Dienstinstanz annehmen oder Kopie dieser Dienst wird nutzen, bis sie alle Updates aus der ist-Dienstinstanzen oder Replikate empfängt. Für einfacher Dienste Endes dies einer statischen Definition, die nie dynamisch aktualisiert und daher für die Gültigkeitsdauer des Diensts verwendet werden. Dieser funktioniert hervorragend für einfache Kapazität, Planung, da es ist genau das, was wir verwendet werden, um Aktionen – bestimmte Ressourcen auf bestimmte Auslastung gilt, aber der Vorteil besteht darin, dass mindestens jetzt wir die Microservices Stelle, wo Ressourcen werden nicht tatsächlich statisch bestimmten Auslastung zugewiesen, und Personen, in dem, Betrieb sind nicht in der Schleife Entscheidung.

Wir können dynamische Dienste an Standard Laden für ihre Primärfarben und die sekundäre – realistischerweise für zahlreiche Services diese Zahlen unterscheiden sich aufgrund der verschiedenen Auslastung ausgeführt Replikate primären und sekundären der und da Primärfarben dienen normalerweise beide liest und schreibt (sowie die meisten der berechnete Belastung) die Standardeinstellung Laden für ein primäres Replikat für sekundäre Replikate größer ist.

Aber jetzt wir haben, dass Sie ausgeführt wurde, des Diensts für eine Weile, und Sie haben haben schon festgestellt, dass einige Instanzen oder Replikate von Ihrem Dienst als andere Weise Weitere Ressourcen beanspruchen oder deren Verbrauch über die Zeit variieren – sie mit einem bestimmten Kunden verknüpft sind, vielleicht sie einfach entsprechen, Auslastung, die im Verlauf des Tages wie messaging Datenverkehr variieren , Telefonanrufe oder Aktualität. Auf jeden Fall Beachten Sie, dass es keine einzelnen "Zahl", die Sie für die Last verwenden können ist, ohne deaktiviert wird, indem Sie sehr viel für mindestens einen Teil der Zeit. Außerdem stellen Sie fest, dass "wird deaktivieren" in die ursprüngliche Schätzung Ergebnisse im Cluster-Manager über oder unter Zuweisen von Ressourcen zu dem Dienst und daher Sie Knoten, die verfügen über oder unter genutzt.

Was zu tun ist? Tja, konnte der Dienst laden im laufenden Betrieb reporting werden!

## <a name="dynamic-load"></a>Dynamisches Laden
Dynamisches Laden-Berichte können Replikate oder Instanzen ihre Zuteilung/gemeldet Verwendung von Kriterien im Cluster über ihre Lebensdauer anpassen. Eine Dienst Replikat oder Instanz, die Haut und keine Arbeit erledigen wurde würde, dass es geringe Mengen von einer Metrik während beschäftigt Replikate oder Instanzen Bericht verwendet wurde, die mehr verwendeten normalerweise melden. Diese allgemeinen Ebene der Änderung im Cluster können wir die Replikate Dienst Neuorganisieren und Instanzen im Cluster im laufenden Betrieb akzeptieren, um sicherzustellen, dass die erste die Ressourcen, die sie – facto benötigen beschäftigt Services sind für das Freigeben von Ressourcen aus anderen Replikate oder Instanzen die sind derzeit Haut oder weniger Arbeit erledigen können. Reporting laden im laufenden Betrieb kann über die ReportLoad-Methode auf die ServicePartition, als Eigenschaft auf der Basis StatefulService oder StatelessService Klasse über die Programmierung zuverlässigen Services-Modell verfügbar vorgenommen werden. Innerhalb des Diensts würde der Code wie folgt aussehen:

Code:

```csharp
this.ServicePartition.ReportLoad(new List<LoadMetric> { new LoadMetric("Memory", 1234), new LoadMetric("metric1", 42) });
```

Services Replikate oder Instanzen Mai nur Bericht Laden für die Metrik, dass sie zum Verwenden von konfiguriert wurden. Die metrische Liste wird festgelegt, wenn jeden Dienst erstellt und später aktualisiert werden kann. Wenn ein Dienst Replikat oder Instanz zum Laden des Berichts für eine Metrik versucht, die nicht aktuell konfiguriert haben, Fabric Service protokolliert den Bericht aber ignoriert, was bedeutet, dass wir nicht, wenn Sie berechnen oder Berichte über den Status der Cluster verwenden. Dies ist Stapel, da es größer experimentieren ermöglicht – der Code messen und alles, was es weiß melden kann, wie und der Operator konfigurieren, gibt und die Ressource Lastenausgleich Regeln für diesen Dienst im laufenden Betrieb ohne jemals so ändern Sie den Code aktualisieren kann. Dies kann beispielsweise enthalten eine Metrik mit einem Bericht Fehlerhafte deaktivieren, die-Stärken der Kennzahlen basierend auf Verhalten oder die Aktivierung von einer neuen Metrik erst der Code bereits bereitgestellt und über andere Methoden überprüft neu zu konfigurieren.

## <a name="mixing-default-load-values-and-dynamic-load-reports"></a>Kombinieren von Standardwerten für Laden und dynamisches Laden Berichte
Ist es sinnvoll, dass eine Standard-Auslastung für einen Dienst, auf Laden dynamisch reporting werden dem angegebenen? Absolut! In diesem Fall fungiert die standardmäßigen laden als eine Schätzung, bis die real Berichte beginnen, aus der ist-Dienst Replikat oder Instanz angezeigt. Dies ist nützlich, da es Ressourcenmanager Cluster nun bietet mit – Schätzung der Standard-laden, er die Dienstinstanzen oder Replikate gute Stellen rechts vom Anfang versehen kann. Wenn keine laden Standardinformationen Platzierung des Services bereitgestellt wird zum Zeitpunkt der Erstellung effektiv Zufallszahl ist, und wenn lädt später ändern der Cluster Ressourcenmanager fast immer müssten Dinge zu navigieren.

Daher müssen wir unserem vorherigen Beispiel ausführen, und sehen, was passiert hinzufügen Wenn wir einige benutzerdefinierte Last und wenn nach der Erstellung des Diensts es dynamisch aktualisiert wird. In diesem Beispiel wir verwenden "Arbeitsspeicher" als Beispiel und lassen Sie uns davon aus, dass wir dynamische Dienst zunächst mit den folgenden Befehl erstellt:

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 2 -TargetReplicaSetSize 3 -PartitionSchemeSingleton –Metric @("Memory,High,21,11”,"PrimaryCount,Medium,1,0”,"ReplicaCount,Low,1,1”,"Count,Low,1,1”)
```

Wir über diese Syntax einer früheren Version (MetricName, MetricWeight, PrimaryDefaultLoad, SecondaryDefaultLoad) gesprochen haben, aber erfahren Sie mehr über die Bedeutung des bestimmten Werts für die Stärke später.

Lassen Sie uns finden welche mögliche Cluster Layout aussehen könnte:

![Cluster abgestimmt mit sowohl Standard- und benutzerdefinierte Kennzahlen][Image2]

Einige Aktionen, die zu beachten sind:

-   Da Replikate oder Instanzen des Diensts Standard Laden verwenden möchten, bis er die eigene Laden melden, wissen wir, dass die Replikate innerhalb 1 Teil der dynamische Dienst Laden für eigene gemeldet noch nicht
-   Sekundäre Replikate innerhalb einer Partition können eigene laden haben.
-   Insgesamt die Metrik Gestaltung ziemlich gut, mit dem Unterschied zwischen den maximalen und minimalen Auslastung auf einem Knoten (für Arbeitsspeicher – die benutzerdefinierten Metrik wir am besten über größere erwähnt) von nur einem Faktor 1.75 (den Knoten mit besonders Laden für der Speicher N3, ist der kleinste ist N2 und 28/16 = 1,75) – ziemlich angeglichene!

Es gibt einige Aktionen, die nach wie vor Beginn wir erläutern müssen

-   Was bestimmt, ob das Verhältnis der 1,75 angemessenen wurde? Woher wissen wir das reicht oder es ist zu tun?
-   Wann tritt Lastenausgleich?
-   Was bedeutet es, dass Arbeitsspeicher "Hoch" gewichteten wurde?

## <a name="metric-weights"></a>Metrische-Stärken
Metrische-Stärken gibt an, was zwei verschiedene Diensten gleicher Metrik melden, sondern die Wichtigkeit dieser Metrik anders Lastenausgleich anzeigen ermöglicht. Betrachten Sie beispielsweise eine im Arbeitsspeicher-Engine und eine beständige Datenbank; Beide interessieren wahrscheinlich die Metrik "Arbeitsspeicher", aber der in-Memory-Dienst wahrscheinlich nicht interessiert viel die Metrik "Datenträger" – möglicherweise etwas nutzen, aber ist nicht entscheidend, die Leistung des Diensts, damit es wahrscheinlich auch dies melden, nicht. Zum Nachverfolgen von gleicher Metrik in verschiedenen Diensten eignet sich hervorragend, da dies ist, was ermöglicht Cluster Ressourcenmanager real Gebrauch in Cluster nachverfolgen, stellen Sie sicher, dass Knoten wechseln nicht über Kapazität usw..

Metrische-Stärken zulassen auch Ressourcenmanager Cluster entscheiden Informationen zum Cluster verteilen, wenn keine perfekte Antwort (also viele der Zeit). Kennzahlen können vier unterschiedliche Gewichtung Ebenen haben: 0 (null), Niedrig, Mittel und hoch. Eine Metrik mit einem Gewicht von 0 (null) beiträgt nichts, wenn in Betracht ziehen, ob Dinge paarweise oder nicht vorhanden sind, aber deren Laden immer noch zur Aspekten wie Kapazität Maße trägt.

Die eigentliche Auswirkung der anderen metrischen-Stärken im Cluster ist, dass wir bei anderen Regelung der Dienste, eintreffen da der Cluster Ressourcenmanager, in Aggregat, aufgefordert wurde, dass bestimmte Kennzahlen wichtiger als andere sind. Da er dies Weiß beim Kennzahlen die verschiedene-Stärken haben in mit einer anderen Konflikt können Ressourcenmanager Cluster Lösungen bevorzugen, die was höheren gewichteten Kennzahlen besser zu verteilen.

Werfen Sie einen Blick auf ein einfaches Beispiel für einige Berichte laden, und wie verschiedene Metrisch-Stärken in der anderen Verteilung im Cluster führen können. In diesem Beispiel sehen wir, dass der Wechsel der relativen Stärke der Kennzahlen Ergebnisse in der Ressourcenmanager Bevorzugung von bestimmter Lösungen durch andere Regelung Dienste zu erstellen.

![Beispiel metrischen Stärke und deren Einfluss auf die Lastenausgleich Lösungen][Image3]

In diesem Beispiel stehen vier verschiedene Dienste, alle anderen reporting-Werte für zwei unterschiedliche Maße A und b In einem Fall alle Dienste definieren Metrisch A ist der wichtigste (Stärke = hoch) und MetricB als relativ (Stärke = "Niedrig"), und tatsächlich um sehen wir, dass der Cluster Ressourcenmanager Dienste platziert, sodass die MetricA besser mittig eingestellt ist (verfügt über eine unteren Standardabweichung) als MetricB. Im zweiten Fall wir die metrischen-Stärken umkehren, und sehen, dass der Cluster Ressourcenmanager wahrscheinlich austauschen möchten Services A und B akzeptieren, um eine Zuweisung ergibt, in dem MetricB besser als MetricA mittig eingestellt ist.

### <a name="global-metric-weights"></a>Globale metrischen-Stärken
Wenn ServiceA MetricA als wichtigsten definiert und ServiceB nicht wichtig es überhaupt ist, was ist der tatsächliche Stärke, die von endet wiederholt verwendet?

Es gibt auch tatsächlich zwei-Stärken, die wir für jede Metrik – verfolgen die Stärke der Dienst selbst definiert und der globalen Mittelwert Gewichtung über alle Dienste, die die Metrik kenne. Wir verwenden diese beiden-Stärken bei der Berechnung der Ergebnisse der Lösung, die wir generieren, da es ist wichtig, um sicherzustellen, dass sowohl ein Dienst mit einem eigenen Prioritäten, sondern auch mittig eingestellt ist, dass der Cluster als Ganzes ordnungsgemäß zugeordnet ist.

Was geschieht, wenn wir globale und lokale Saldo kümmern haben? Es ist sehr einfach zu Lösungen, die Global paarweise vorhanden sind, aber die dazu führen, dass sehr beeinträchtigt Saldo und Ressourcen Zuteilung für einzelne Dienste, zu erstellen. Im folgenden Beispiel wir erwägen Sie die standardmäßigen Kennzahlen, denen mit ein dynamische Dienst konfiguriert ist, PrimaryCount, ReplicaCount und zählen, und sehen, was passiert, wenn wir nur globale Saldo in Betracht ziehen:

![Die Wirkung einer globalen nur Lösung][Image4]

Im oberen Beispiel, in dem wir nur bei globalen Saldo vergeblich, Cluster als Ganzes wird tatsächlich um verteilt – alle Knoten über dieselbe Anzahl von Primärfarben und total Replikate verfügen. Wenn Sie die tatsächliche Auswirkung dieser Mittel betrachten es jedoch nicht so gut ist: Verlust von einem beliebigen Knoten wirkt sich auf eine bestimmte Arbeitsbelastung unproportional geänderter, wie sich alle zugehörigen Primärfarben dauert. Nehmen Sie beispielsweise wäre wir den ersten Knoten verlieren möchten. Wenn dies die drei Primärfarben für die drei verschiedenen Partitionen des Diensts Kreis alle gleichzeitig verloren gehen würden Vorkommnissen. Umgekehrt verfügen die anderen zwei Dienste (Dreieck und Sechseck), deren Partitionen eine Replikation verlieren, die wodurch keine Unterbrechung (mit Ausnahme das nach unten Replikat wiederherstellen müssen).

Im Beispiel unten haben wir die basierend auf der globalen und jeden Dienst Saldo Replikate verteilt. Bei der Berechnung die Bewertung der Lösung gewähren wir Mehrzahl des Gewichts auf die globale Lösung anzeigt, jedoch ein Teil (konfigurierbarer), um sicherzustellen, dass die Dienste auch in selbst so weit wie möglich verteilt werden. Daher wäre wir den gleichen ersten Knoten verlieren möchten, zu sehen, wird des Verlust der Primärfarben (und sekundäre) alle partitionsübergreifend aller Dienste und der Einfluss auf die einzelnen übereinstimmt.

Berücksichtigung metrische-Stärken, wird der globale Saldo basierend auf den Mittelwert der metrischen-Stärken für jeden der Dienste konfiguriert berechnet. Wir zu einen Dienst in Bezug auf einen eigenen definierten metrischen-Stärken verteilen.

## <a name="next-steps"></a>Nächste Schritte
- Für Weitere Informationen zu anderen Optionen für das Konfigurieren verfügbar Auschecken des Themas auf die anderen Cluster Ressourcenmanager Konfigurationen services verfügbare [Informationen zum Konfigurieren von Diensten](service-fabric-cluster-resource-manager-configure-services.md)
- Definieren von Defragmentierung Metrik ist eine Möglichkeit zum Konsolidieren von Last auf Knoten, statt ihn verteilen. Informationen zum Konfigurieren der Defragmentierung, schlagen Sie in [diesem Artikel](service-fabric-cluster-resource-manager-defragmentation-metrics.md)
- Um herauszufinden, darüber, wie der Ressourcenmanager Cluster verwaltet und verteilt Last im Cluster, schauen Sie sich im Artikel auf [Last](service-fabric-cluster-resource-manager-balancing.md)
- Beginnen Sie am Anfang und [erhalten Sie eine Einführung in die Ressourcenmanager Dienst Fabric Cluster](service-fabric-cluster-resource-manager-introduction.md)
- Bewegung Kosten ist eine Möglichkeit zum Cluster-Manager Auslösen der, dass bestimmte Dienste teurer als andere zu verschieben. Um weitere Informationen zur Bewegung Kosten finden Sie in [diesem Artikel](service-fabric-cluster-resource-manager-movement-cost.md)

[Image1]:./media/service-fabric-cluster-resource-manager-metrics/cluster-resource-manager-cluster-layout-with-default-metrics.png
[Image2]:./media/service-fabric-cluster-resource-manager-metrics/Service-Fabric-Resource-Manager-Dynamic-Load-Reports.png
[Image3]:./media/service-fabric-cluster-resource-manager-metrics/cluster-resource-manager-metric-weights-impact.png
[Image4]:./media/service-fabric-cluster-resource-manager-metrics/cluster-resource-manager-global-vs-local-balancing.png

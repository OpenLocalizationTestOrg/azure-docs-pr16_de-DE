<properties
   pageTitle="Dienst Fabric Cluster Ressourcenmanager: Bewegung Kosten | Microsoft Azure"
   description="Übersicht über Bewegung Kosten für Dienst Fabric-services"
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

# <a name="service-movement-cost-for-influencing-cluster-resource-manager-choices"></a>Dienst Bewegung Kosten zur Steuerung der Auswahlmöglichkeiten Cluster Ressourcenmanager
Ein wichtiger Faktor zu berücksichtigen ist, wenn Sie versuchen, Sie entscheiden können, welche Änderungen an einem Cluster vornehmen und die Bewertung einer Lösung ist die Gesamtkosten für die Lösung zu erreichen.

Verschieben Sie Dienst Instanzen oder Replikate Kosten CPU Zeit und Netzwerk Bandbreite mindestens ein. Für dynamische Dienste Kosten es auch den Speicherplatz auf dem Datenträger, die Sie benötigen, eine Kopie des Status zu erstellen, bevor Sie alte Replikate beenden aus. Klar möchten Sie die Kosten einer Lösung zu minimieren, die Cluster Ressourcenmanager von Azure Service Fabric mit auftauchen. Aber auch nicht Lösungen ignorieren möchten, die erheblich die Zuordnung von Ressourcen im Cluster verbessern möchten.

Ressourcenmanager Cluster weist zwei Arten von computing Kosten und einschränken können, auch während versucht wird, der Cluster nach anderen seine Ziele verwalten. Die erste ist, dass bei der Planung Cluster Ressourcenmanager eines neuen Layouts für den Cluster wird jeder verschieben gezählt wird, die sie vornehmen möchten. In einem einfachen Fall Wenn Sie zwei Lösungen mit etwa gleich erhalten Saldo insgesamt am Ende (Ergebnis), und nehmen Sie dann diejenige mit den niedrigsten Kosten (Gesamtzahl der wechselt).

Dies ist ziemlich geeignet. Aber wie bei Standard oder statischen lädt, es wahrscheinlich nicht in einem komplexen System, dass alle verschiebt gleich sind. Einige sind wahrscheinlich viel teurer sein.

## <a name="changing-a-replicas-move-cost-and-factors-to-consider"></a>Ändern des Replikats verschieben Kosten und zu berücksichtigende Faktoren
Als Stellen mit laden (ein weiteres Feature von Cluster Ressourcenmanager) melden, Sie dem Dienst eine Möglichkeit der Self Berichterstattung wie teure Dienst besteht darin, zu einem bestimmten Zeitpunkt zu verschieben.

Code:

```csharp
this.ServicePartition.ReportMoveCost(MoveCost.Medium);
```

MoveCost hat vier Ebenen: 0 (null), Niedrig, Mittel und hoch. Dies sind relativ zu anderen, eine Ausnahme bilden jedoch 0 (null). 0 (null) bedeutet, dass ein Replikat ist kostenlos und sollte nicht für die Bewertung der Lösung zählen verschieben. Festlegen der Kosten verschieben auf hoch ist *keine* Garantie für die wird nicht lediglich, dass es verschoben wird nicht, es sei denn, es gibt einen guten Grund, um das Replikat verschieben.

![Verschieben von Kosten als Faktor bei der Auswahl Replikate für Bewegung][Image1]

MoveCost kann die Lösungen zu finden, die dazu führen, dass die minimalen Unterbrechung generelle und sind die einfachste Möglichkeit zum erzielen bei Ankunft weiterhin auf Saldo entspricht. Ein Dienst Konzept von Kosten kann relativ zu viele Merkmale werden. Die am häufigsten verwendeten Faktoren bei der Berechnung der Kosten verschieben sind:

- Der Umfang der Bundesland oder Daten, die der Dienst wurde zu verschieben.
- Die Kosten der Trennung der Clients. Die Kosten ein primäres Replikat zu verschieben, ist in der Regel höher als die Kosten einer sekundären Replikat zu verschieben.
- Die Kosten für einen Flugzeug Vorgang unterbrechen. Einige Vorgänge, die Daten speichern Ebene oder Operationen im Client von Vorschlägen teure sind. Nach einem bestimmten Punkt soll nicht, wenn das nicht unbedingt beenden. Daher anderem für die Dauer des Vorgangs, Sie von der Kosten, um die Wahrscheinlichkeit zu verringern, die den Dienst Replikat oder die Instanz verschieben möchten. Wenn der Vorgang abgeschlossen ist, setzen Sie ihn wieder auf Normal.

## <a name="next-steps"></a>Nächste Schritte
- Dienst Fabric Cluster Resource Manager verwendet Kennzahlen Verbrauch und Kapazität im Cluster verwaltet. Weitere Informationen zum Metrik und so konfigurieren checken Sie [Ressourcenverbrauch Verwaltung und Dienst Struktur mit Kennzahlen Laden aus](service-fabric-cluster-resource-manager-metrics.md).
- Informationen darüber, wie der Ressourcenmanager Cluster verwaltet und verteilt Last im Cluster checken Sie [Ihren Cluster Service Fabric Lastenausgleich aus](service-fabric-cluster-resource-manager-balancing.md).

[Image1]:./media/service-fabric-cluster-resource-manager-movement-cost/service-most-cost-example.png

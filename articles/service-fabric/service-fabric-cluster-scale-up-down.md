<properties
   pageTitle="Einen Dienst Fabric Cluster vergrößern oder skalieren | Microsoft Azure"
   description="Skalieren Sie einen Dienst Fabric Cluster vergrößern oder bei Bedarf entsprechend zu durch automatische Skalierung Regeln für jeden Knoten Typ/virtueller Computer Maßstab Satz festlegen. Hinzufügen oder Entfernen von Knoten zu einem Cluster Service Fabric"
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/09/2016"
   ms.author="chackdan"/>


# <a name="scale-a-service-fabric-cluster-in-or-out-using-auto-scale-rules"></a>Skalieren Sie einen Dienst Fabric Cluster oder Verkleinern von Regeln für automatische Skalierung

Virtuellen Computern sind skalieren eine Azure berechnen Ressource, die Sie bereitstellen und Verwalten einer Gruppe von virtuellen Computern als Gruppe verwenden können. Jede Art von Knoten, die in einem Dienst Fabric Cluster definiert ist wird als separate virtueller Computer Maßstab Satz eingerichtet. Einzelnen Knoten können dann skaliert werden oder out unabhängig voneinander, unterschiedliche Optionssätze Ports geöffnet haben und können andere Speicherkapazität Kennzahlen haben. Lesen Sie mehr über es im [Dienst Fabric Nodetypes](service-fabric-cluster-nodetypes.md) Dokument. Da der Dienst Textur Typen von Knoten im Cluster virtuellen Computer Skala vorgenommen werden bei der Back-End-festlegt, müssen Sie automatische Skalierung Regeln für jeden Knoten Typ/virtueller Computer Maßstab Satz einrichten.

>[AZURE.NOTE] Ihr Abonnement müssen genügend Kerne die neuen virtuellen Computern hinzufügen, die diesem Cluster bilden. Ist keine Modell Validierung aktuell, damit ein Bereitstellungsfehler Zeit, erhalten, wenn keines der Kontingent Grenzwerte erreicht werden.

## <a name="choose-the-node-typevm-scale-set-to-scale"></a>Wählen Sie den Knoten Typ/virtueller Computer Maßstab zu skalieren festlegen

Sie sind derzeit nicht die automatische Skalierung Regeln für virtuellen Computer Maßstab Datensätze mit dem Portal angeben, also lassen Sie uns Azure PowerShell (1.0 +) verwenden, um die Knotentypen Liste und dann Regeln für automatische Skalierung zu ihnen hinzufügen können.

Führen Sie die folgenden Cmdlets, um der Liste der virtuellen Computer Maßstab festlegen, die Ihren Cluster bilden zu erhalten:

```powershell
Get-AzureRmResource -ResourceGroupName <RGname> -ResourceType Microsoft.Compute/VirtualMachineScaleSets

Get-AzureRmVmss -ResourceGroupName <RGname> -VMScaleSetName <VM Scale Set name>
```

## <a name="set-auto-scale-rules-for-the-node-typevm-scale-set"></a>Festlegen Sie automatische Skalierung Regeln für die Knoten Typ/virtueller Computer Maßstab Menge

Wenn der Cluster mehrere Typen von Knoten aufweist, wiederholen Sie dies für jedes Knoten Typen/virtueller Computer Maßstab Sets, dass Sie (oder verkleinern) skalieren möchten. Ziehen Sie der Anzahl der Knoten, die Sie benötigen, bevor Sie die automatische Skalierung einrichten. Die minimale Anzahl von Knoten, die Sie für den Typ des primären Knoten benötigen, wird durch die Ebene Zuverlässigkeit gesteuert, die Sie ausgewählt haben. Weitere Informationen zum [Zuverlässigkeit Ebenen](service-fabric-cluster-capacity.md).

>[AZURE.NOTE]  Skalierung ab dem primären Knoten geben Sie ein, der kleiner als der kleinsten Zahl Tabellenerstellungsabfrage Cluster instabil oder bringen Sie ihn nach unten. Dies kann Datenverlust für die Anwendung und für die Dienste führen.

Das Feature für die automatische Skalierung wird derzeit nicht nach dem Laden des gesteuert, an den Dienst Fabric Ihrer Applikationen Berichten werden möglicherweise. Zu diesem Zeitpunkt, die die automatische Skalierung, denen, die Sie kommunizieren, rein durch die Leistung gesteuert wird skalieren Indikatoren, die von den einzelnen den virtuellen Computer ausgegeben werden also Instanzen festlegen.  

Führen Sie diese Anweisungen [zum Einrichten der automatische Skalierung für jeden virtuellen Computer Maßstab Satz](../virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-overview.md).

>[AZURE.NOTE] In einem Maßstab unten Szenario es sei denn, Sie dem Knotentyp Ihrer eine Ebene Zuverlässigkeit Gold oder Silber weist müssen Sie das [Cmdlet entfernen-ServiceFabricNodeState](https://msdn.microsoft.com/library/azure/mt125993.aspx) mit dem Namen entsprechenden Knoten aufrufen.

## <a name="manually-add-vms-to-a-node-typevm-scale-set"></a>Virtuellen Computern manuell zu einem Knoten Typ/virtueller Computer skalieren hinzufügen

Folgen Sie den Stichprobe/Anweisungen im [Vorlagenkatalog Schnellstart](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) so ändern Sie die Anzahl der virtuellen Computern in jeder Nodetype. 

>[AZURE.NOTE] Hinzufügen von virtuellen Computern dauert erneut, sodass keine erwarten die Ergänzungen erfolgt sein. Planen der also von Zeit, um zu ermöglichen, über 10 Minuten anzugeben, bevor die Kapazität virtueller Computer für die Replikate verfügbar ist auch Kapazität hinzufügen / service Instanzen platziert abrufen.

## <a name="manually-remove-vms-from-the-primary-node-typevm-scale-set"></a>Entfernen von virtuellen Computern manuell aus den Maßstab der primären Knoten Typ/virtueller Computer

>[AZURE.NOTE] Der Dienst Fabric Systemdienste, die in den Typ der primären Knoten im Cluster ausgeführt werden. Damit sollten nie beendet wird oder die Anzahl der Instanzen in diesen Abfragearten Knoten verkleinern Teamwebsite rechtfertigt kleiner als welche die Zuverlässigkeit Ebene aus. Beziehen sich auf [die Zuverlässigkeit Ebenen hier Details](service-fabric-cluster-capacity.md). 

Sie müssen die folgende Schritte eine Instanz virtueller Computer gleichzeitig durchgeführt werden. Dadurch wird für die Dienste (und Ihre dynamische Dienste) ordnungsgemäß beendet werden soll auf die Instanz virtueller Computer, die Sie entfernen möchten, und die neue Replikate, die auf anderen Knoten erstellt.

1. Führen Sie [Deaktivieren-ServiceFabricNode](https://msdn.microsoft.com/library/mt125852.aspx) mit 'RemoveNode' So deaktivieren Sie den Knoten (die höchste Instanz dieses Knotentyps) entfernt werden soll.

2. Führen Sie [Get-ServiceFabricNode](https://msdn.microsoft.com/library/mt125856.aspx) , um sicherzustellen, dass der Knoten tatsächlich um deaktiviert den Übergang in ein. Wenn dies nicht der Fall ist, warten, bis der Knoten deaktiviert ist. Sie können keine dieses Schritts synchronisierungssymbol.

2. Folgen Sie den Stichprobe/Anweisungen im [Vorlagenkatalog Schnellstart](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) so ändern Sie die Anzahl der virtuellen Computern mit einem in dieser Nodetype. Die Instanz entfernt ist die Instanz der höchsten virtueller Computer an. 

3. Wiederholen Sie die Schritte 1 bis 3 bei Bedarf, aber nie verkleinern Sie die Anzahl der Instanzen in der primären Knotentypen kleiner als was die Zuverlässigkeit Ebene Teamwebsite rechtfertigt. Beziehen sich auf [die Zuverlässigkeit Ebenen hier Details](service-fabric-cluster-capacity.md). 

## <a name="manually-remove-vms-from-the-non-primary-node-typevm-scale-set"></a>Manuell daraus nicht primäre Knoten Typ/virtueller Computer Maßstab festlegen auf virtuellen Computern

>[AZURE.NOTE] Für eine dynamische Dienst benötigen Sie eine bestimmte Anzahl von Knoten Verfügbarkeit werden immer bis zu verwalten und Beibehalten des Status Ihres Dienstes an. Zumindest benötigen Sie die Anzahl der Knoten gleich der Anzahl Ziel Replikat festlegen, der den Partition-Dienst. 

Sie benötigen das Ausführen der folgenden Schritte eine Instanz der virtueller Computer jeweils ein. Dadurch wird für die Dienste (und Ihre dynamische Dienste), ordnungsgemäß auf die Instanz virtueller Computer beendet werden soll, die Sie entfernen möchten, und neue Replikate erstellt sonst Where.

1. Führen Sie [Deaktivieren-ServiceFabricNode](https://msdn.microsoft.com/library/mt125852.aspx) mit 'RemoveNode' So deaktivieren Sie den Knoten (die höchste Instanz dieses Knotentyps) entfernt werden soll.

2. Führen Sie [Get-ServiceFabricNode](https://msdn.microsoft.com/library/mt125856.aspx) , um sicherzustellen, dass der Knoten tatsächlich um deaktiviert den Übergang in ein. Wenn dies nicht warten, bis der Knoten deaktiviert ist. Sie können keine dieses Schritts synchronisierungssymbol.

2. Folgen Sie den Stichprobe/Anweisungen im [Vorlagenkatalog Schnellstart](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) so ändern Sie die Anzahl der virtuellen Computern mit einem in dieser Nodetype. Hiermit wird jetzt die Instanz der höchste virtueller Computer entfernt. 

3. Wiederholen Sie die Schritte 1 bis 3 bei Bedarf, aber nie verkleinern Sie die Anzahl der Instanzen in der primären Knotentypen kleiner als was die Zuverlässigkeit Ebene Teamwebsite rechtfertigt. Beziehen sich auf [die Zuverlässigkeit Ebenen hier Details](service-fabric-cluster-capacity.md).

## <a name="behaviors-you-may-observe-in-service-fabric-explorer"></a>Verhalten im Dienst Fabric Explorer Druckeroptionen

Wenn Sie von einem Cluster skalieren wider der Dienst Fabric Explorer die Anzahl der Knoten (virtueller Computer Maßstab festgelegt Instanzen), die den Cluster gehören.  Beim Skalieren werden jedoch ein Cluster nach unten, Sie sehen die entfernte Knoten virtueller Computer-Instanz aus einem fehlerhaften Zustand angezeigt, wenn Sie mit dem Namen entsprechenden Knoten [Entfernen-ServiceFabricNodeState Cmd](https://msdn.microsoft.com/library/mt125993.aspx) aufrufen.   

Hier sind die Erklärung für dieses Verhalten.

Die Knoten Dienst Fabric Explorer aufgeführt sind eine Spiegelung, welche Dienst Fabric System-Dienste (FM speziell) kennt die Anzahl der Knoten Cluster hatten/hat. Wenn Sie die virtuellen Computer Skalierung festlegen nach unten skalieren, der virtuellen Computer wurde gelöscht, aber FM Systemdienst weiterhin der Meinung, dass der Knoten (, die den virtuellen Computer, die nicht gelöscht wurde zugeordnet wurde) zurückgegeben wird. Daher weiterhin Dienst Fabric Explorer diesen Knoten an (obwohl der Integritätsstatus Fehler "oder" unbekannt sein kann).

Um sicherzustellen, dass ein Knoten entfernt wird, wenn ein virtueller Computer entfernt wird, haben Sie zwei Optionen:

1) Wählen Sie eine Ebene Zuverlässigkeit Gold oder Silber (verfügbar bald) für die Knotentypen im Cluster und so erhalten Sie die Integration Infrastruktur. Die dann entfernt automatisch der Knoten aus unserer Dienste (FM) Systemstatus beim Skalieren nach unten.
Verweisen Sie auf [die Zuverlässigkeit Ebenen hier details](service-fabric-cluster-capacity.md)

2) Nachdem die Instanz virtueller Computer nach unten skaliert wurde, müssen Sie das [Cmdlet entfernen-ServiceFabricNodeState](https://msdn.microsoft.com/library/mt125993.aspx)anrufen.

>[AZURE.NOTE] Dienst Fabric Cluster erfordern eine bestimmte Anzahl von Knoten von werden Sie gleichzeitig alle um Verfügbarkeit und Beibehalten des Status - genannt "Verwalten von Quorum". Ja, ist es in der Regel unsicheren auf alle Computer im Cluster beenden, es sei denn, Sie zuerst eine [vollständige Sicherung Ihres](service-fabric-reliable-services-backup-restore.md)durchgeführt haben.

## <a name="next-steps"></a>Nächste Schritte
Lesen Sie auch Informationen zu planen Cluster Kapazität, einen Cluster aktualisieren und Partitionierung Services mit die folgenden:

- [Planung der Kapazität cluster](service-fabric-cluster-capacity.md)
- [Cluster-upgrades](service-fabric-cluster-upgrade.md)
- [Partition dynamische Dienste für maximalen Dezimalstellen](service-fabric-concepts-partitioning.md)

<!--Image references-->
[BrowseServiceFabricClusterResource]: ./media/service-fabric-cluster-scale-up-down/BrowseServiceFabricClusterResource.png
[ClusterResources]: ./media/service-fabric-cluster-scale-up-down/ClusterResources.png

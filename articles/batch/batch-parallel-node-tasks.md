<properties
    pageTitle="Maximieren Stapel Knoten verwenden mit parallelen Aufgaben | Microsoft Azure"
    description="Erhöhen der Effizienz und geringere Kosten mithilfe weniger Datenverarbeitungsknoten und Einstieg gleichzeitige Aufgaben auf den einzelnen Knoten in einem Stapel Azure-pool"
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor="" />

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="big-compute"
    ms.date="10/25/2016"
    ms.author="marsma" />

# <a name="maximize-azure-batch-compute-resource-usage-with-concurrent-node-tasks"></a>Maximieren Sie Azure Stapel berechnen Ressource: Einsatz bei gleichzeitiger Knoten Aufgaben

Maximieren Sie gleichzeitig ausführen von mehr als eine Aufgabe auf den einzelnen Knoten berechnen im Pool Azure Stapel, Ressource: Einsatz auf eine kleinere Anzahl von Knoten im Pool. Für einige Auslastung kann dies schnellere Position und kostengünstiger führen.

Mehrere Situationen nutzbringend während einige Szenarien gilt alle Ressourcen von einem Knoten mit einer einzelnen Aufgabe nutzbringend, gleicht von mehreren Vorgängen zum Freigeben von diesen Ressourcen:

 - **Minimieren Datenübertragung** , wenn Vorgänge Daten verwenden können. In diesem Szenario können Sie Daten durchstellen Gebühren erheblich, freigegebene Daten in eine kleinere Anzahl von Knoten kopieren und Ausführen von Aufgaben auf den einzelnen Knoten parallel reduzieren. Dies gilt besonders, wenn die Daten in den einzelnen Knoten kopiert werden zwischen geografischen Regionen übertragen werden müssen.

 - **Arbeitsspeicherauslastung Maximizing** Wenn Aufgaben sehr viel Speicher, aber nur in kurzen Zeiträumen Zeit und Variable Vorkommen während der Ausführung erforderlich. Sie können weniger, aber vergrößern, berechnen Knoten mit mehr Speicher effizient solche Spitzen verarbeitet einsetzen. Diese Knoten würde mehrere Aufgaben, die auf den einzelnen Knoten parallel ausgeführt haben, aber jeden Vorgang würde nutzen Sie die Knoten ausreichendem Arbeitsspeicher zu unterschiedlichen Zeiten.

 - **Verringerung der Zahl Grenzwerte Knoten** , wenn der Kommunikation zwischen den Knoten in einem Ressourcenpool erforderlich ist. Pools für die Kommunikation zwischen den Knoten konfiguriert sind derzeit auf 50 Datenverarbeitungsknoten beschränkt. Wenn die einzelnen Knoten in einem solchen Ressourcenpool Aufgaben parallel ausgeführt werden kann, kann eine größere Anzahl von Aufgaben gleichzeitig ausgeführt werden.

 - **Eine lokale Replikation Cluster berechnen**, beispielsweise wenn Sie zuerst eine berechnen Umgebung zu Azure wechseln. Wenn Ihre aktuelle lokale Lösung mehrere Aufgaben pro berechnen Knoten ausgeführt wird, können Sie die maximale Anzahl von Knoten Aufgaben an, um die Konfiguration genauer annähernd erhöhen.

## <a name="example-scenario"></a>Beispielszenario

Erläutern Sie die Vorteile der Ausführung der Aufgabe parallele beispielhaft angenommen, die Aufgabe Anwendung CPU- und Anforderungen so, dass [Standard\_D1](../cloud-services/cloud-services-sizes-specs.md#general-purpose-d) Knoten sind ausreichend. Aber, um den Auftrag in die erforderliche Zeit abgeschlossen haben, 1.000 dieser Knoten erforderlich sind.

Statt Zeitstandard\_D1-Knoten, 1 CPU Core verfügen, können Sie verwenden [Standard\_D14](../cloud-services/cloud-services-sizes-specs.md#memory-intensive-d) Knoten, 16 Kerne haben, und aktivieren Ausführung der parallele Aufgabe. Daher *16 Mal weniger Knoten* verwendet werden kann – anstelle von 1.000 Knoten, nur 63 wäre erforderlich. Darüber hinaus, wenn umfangreiche Anwendungsdateien oder Bezug Daten für die einzelnen Knoten erforderlich sind, sind Dauer Projekts und Effizienz erneut verbessert, da die Daten in nur 16 Knoten kopiert werden.

## <a name="enable-parallel-task-execution"></a>Aktivieren Sie Ausführung der parallele Aufgabe

Sie konfigurieren Datenverarbeitungsknoten für die Ausführung der Aufgabe parallele Ebene der Ressourcenpool aus. Mit der Bibliothek Stapel .NET Festlegen der [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] Eigenschaft, wenn Sie einen Pool zu erstellen. Wenn Sie die Stapel REST-API verwenden, legen Sie die [MaxTasksPerNode] [ rest_addpool] Element im Hauptteil Anforderung, während der Erstellung der Ressourcenpool.

Stapel Azure ermöglicht es Ihnen, die maximale Vorgänge pro Knoten bis zu vier Mal festgelegt (X 4) die Anzahl der Knoten Kerne. Angenommen, wenn der Pool mit Knoten des konfiguriert ist Größe des Steuerelements "Groß" (vier Kerne), klicken Sie dann `maxTasksPerNode` kann auf 16 festgelegt werden. Details auf die Anzahl der Kerne für jede der Größen Knoten finden Sie unter [Größen für Cloud-Dienste](../cloud-services/cloud-services-sizes-specs.md). Weitere Informationen zum Beschränkungen Service finden Sie unter [Kontingente und Grenzwerte für den Stapel Azure-Dienst](batch-quota-limit.md).

> [AZURE.TIP] Berücksichtigen Sie unbedingt die `maxTasksPerNode` Wert beim Erstellen einer [Formel automatisch skalieren] [ enable_autoscaling] für Ihre Ressourcenpool. Angenommen, eine Formel, berechnet `$RunningTasks` Erhöhung Vorgänge pro Knoten könnte erheblich beeinträchtigt werden. Weitere Informationen finden Sie unter [automatisch skalieren Knoten in einem Stapel Azure-Pool zu berechnen](batch-automatic-scaling.md) .

## <a name="distribution-of-tasks"></a>Verteilung der Aufgaben

Wenn die Datenverarbeitungsknoten in einem Ressourcenpool Vorgänge gleichzeitig ausgeführt werden können, ist es wichtig, um anzugeben, wie Sie die Aufgaben über die Knoten im Pool verteilt werden sollen.

Mithilfe der [CloudPool.TaskSchedulingPolicy] [ task_schedule] Eigenschaft können Sie angeben, dass Aufgaben gleichmäßig über alle Knoten im Pool ("Zuweisung") zugewiesen werden sollen. Oder Sie können angeben, dass viele Vorgänge wie möglich an jeden Knoten zugewiesen werden sollen, bevor Sie Aufgaben auf einen anderen Knoten im Pool ("Verpackung") zugewiesen werden.

Ein Beispiel dafür, wie dieses Feature wertvolle ist, sollten Sie den Pool von [Standard\_D14](../cloud-services/cloud-services-sizes-specs.md#memory-intensive-d) Knoten (im Beispiel oben), die mit einem [CloudPool.MaxTasksPerComputeNode] konfiguriert ist[ maxtasks_net] Wert von 16. Wenn die [CloudPool.TaskSchedulingPolicy] [ task_schedule] ist so konfiguriert, dass mit einer [ComputeNodeFillType] [ fill_type] *Pack*, möchten sie maximieren Verwendung der alle 16 Kerne für die einzelnen Knoten und eine [Automatische Skalierung Ressourcenpool](batch-automatic-scaling.md) zu löschen nicht verwendete Knoten aus dem Pool (Knoten ohne alle zugeordneten Vorgänge) zulassen. Dies minimiert Ressource: Einsatz hinzu, und Geld speichert.

## <a name="batch-net-example"></a>Stapel .NET Beispiel

Diese [Stapel .NET] [ api_net] API Codeausschnitt zeigt eine Besprechungsanfrage zu einem Ressourcenpool zu erstellen, die vier große Knoten mit bis zu vier Vorgänge pro Knoten enthält. Es gibt eine Aufgabe, die Richtlinie, die einzelnen Knoten mit Aufgaben, die vor dem Zuweisen von Aufgaben an einen anderen Knoten im Pool ausfüllt Planung an. Weitere Informationen zum Hinzufügen von Pools mithilfe der Stapel .NET API finden Sie unter [BatchClient.PoolOperations.CreatePool][poolcreate_net].

```csharp
CloudPool pool =
    batchClient.PoolOperations.CreatePool(
        poolId: "mypool",
        targetDedicated: 4
        virtualMachineSize: "large",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));

pool.MaxTasksPerComputeNode = 4;
pool.TaskSchedulingPolicy = new TaskSchedulingPolicy(ComputeNodeFillType.Pack);
pool.Commit();
```

## <a name="batch-rest-example"></a>Stapel REST Beispiel

Diese [Stapel REST] [ api_rest] API Codeausschnitt zeigt eine Besprechungsanfrage zu einem Ressourcenpool zu erstellen, die zwei große Knoten mit bis zu vier Vorgänge pro Knoten enthält. Weitere Informationen zum Hinzufügen von Pools mithilfe der REST-API finden Sie unter [Hinzufügen einer Ressourcenpool mit einer Firma][rest_addpool].

```json
{
  "odata.metadata":"https://myaccount.myregion.batch.azure.com/$metadata#pools/@Element",
  "id":"mypool",
  "vmSize":"large",
  "cloudServiceConfiguration": {
    "osFamily":"4",
    "targetOSVersion":"*",
  }
  "targetDedicated":2,
  "maxTasksPerNode":4,
  "enableInterNodeCommunication":true,
}
```

> [AZURE.NOTE] Sie können festlegen, die `maxTasksPerNode` Element- und [MaxTasksPerComputeNode] [ maxtasks_net] Eigenschaft nur zum Zeitpunkt der Erstellung Ressourcenpool. Sie können nicht geändert werden, nachdem ein Ressourcenpool bereits erstellt wurde.

## <a name="code-sample"></a>Beispiel für Code

Die [ParallelNodeTasks] [ parallel_tasks_sample] Projekt auf GitHub veranschaulicht die Verwendung von der [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] Eigenschaft.

Dieser C#-Console-Anwendung verwendet die [Stapel .NET] [ api_net] -Bibliothek mit einem Ressourcenpool mit einem oder mehreren berechnen Knoten zu erstellen. Sie führt eine konfigurierbare Anzahl von Aufgaben auf diesen Knoten Variable Auslastung simulieren. Ausgabe der Anwendung gibt an, welche Knoten jeden Vorgang ausgeführt wird. Die Anwendung bietet auch eine Zusammenfassung der Parameter Position und Dauer. Der Zusammenfassung Teil der Ausgabe aus zwei verschiedenen Strecken von der Stichprobe Anwendung wird unter angezeigt.

```
Nodes: 1
Node size: large
Max tasks per node: 1
Tasks: 32
Duration: 00:30:01.4638023
```

Die erste Ausführung der Anwendung Beispiel zeigt, die mit einem einzelnen Knoten in der Ressourcenpool und die Standardeinstellung der jeweils eine Aufgabe pro Knoten, die Dauer des Projekts über 30 Minuten ist.

```
Nodes: 1
Node size: large
Max tasks per node: 4
Tasks: 32
Duration: 00:08:48.2423500
```

Die zweite Ausführen des im Beispiel gezeigt Dauer Projekts erheblich verringert. Dies ist, da der Pool mit vier Vorgänge pro Knoten konfiguriert wurde die Ausführung der parallele Aufgabe zum Abschließen des Auftrags in fast ein Viertel der Zeit ermöglicht.

> [AZURE.NOTE] Die Dauer Position in der obigen Zusammenfassung enthalten nicht Erstellungszeit Ressourcenpool. Jeden der oben genannten Einzelvorgänge wurde zuvor erstellten Pools übermittelt, deren Knoten berechnen im *Leerlauf* Zustand zum Zeitpunkt der Übermittlung waren.

## <a name="next-steps"></a>Nächste Schritte

### <a name="batch-explorer-heat-map"></a>Explorer-Wärmebilds Stapel

Der [Azure Stapel Explorer][batch_explorer], eine von Azure Stapel [Stichprobe Applications][github_samples], enthält eine *Wärmebilds* -Funktion, die Visualisierung der Ausführung der Aufgabe enthält. Wenn Sie die [ParallelTasks] aufgeführte sind[ parallel_tasks_sample] Beispiel-Anwendung, Sie können das Feature Wärmebilds auf einfache Weise die Ausführung von Parallele Aufgaben auf den einzelnen Knoten visualisieren verwenden.

![Explorer-Wärmebilds Stapel][1]

*Stapel Explorer-Wärmebilds mit einem Ressourcenpool vier Knoten, mit den einzelnen Knoten aktuell vier Aufgaben ausführen*

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[enable_autoscaling]: https://msdn.microsoft.com/library/azure/dn820173.aspx
[fill_type]: https://msdn.microsoft.com/library/microsoft.azure.batch.common.computenodefilltype.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[maxtasks_net]: http://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.maxtaskspercomputenode.aspx
[rest_addpool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[parallel_tasks_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/ParallelTasks
[poolcreate_net]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[task_schedule]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudpool.taskschedulingpolicy.aspx

[1]: ./media/batch-parallel-node-tasks\heat_map.png

<properties
    pageTitle="Anordnungsbeziehungen in Azure Stapel | Microsoft Azure"
    description="Erstellen von Aufgaben, die den erfolgreichen Abschluss der anderen Aufgaben für die Verarbeitung von MapReduce Stil und ähnliche große Daten abhängig sind in Azure Stapel Auslastung."
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
    ms.date="09/28/2016"
    ms.author="marsma" />

# <a name="task-dependencies-in-azure-batch"></a>Anordnungsbeziehungen in Azure Stapel

Das Feature "Abhängigkeiten" Azure Blattnamen ist gut verarbeitet werden soll:

- MapReduce-Schreibweise Auslastung in der Cloud.
- Aufträge, deren Datenverarbeitung Vorgänge als gerichteten acyclische Graphen (so) ausgedrückt werden können.
- Andere Position in der untergeordnete Aufgaben die Ausgabe der übergeordneten Aufgaben abhängig sind.

Stapel anordnungsbeziehungen können Sie zum Erstellen von Aufgaben, die für die Ausführung auf Datenverarbeitungsknoten nur nach dem erfolgreichen Abschluss der eine oder mehrere Aufgaben angesetzt werden. Beispielsweise können Sie ein Projekt erstellen, die jeder Frame eines Films 3D mit separaten, parallele Aufgaben werden gerendert. Die endgültige Vorgang – die "Seriendruck Aufgabe" – Vorlagen für den Seriendruck gerenderten Frames in den vollständigen Film erst alle Bilder erfolgreich gerendert wurden.

Sie können Aufgaben erstellen, die andere Aufgaben in einer 1: 1 oder 1: n-Beziehung abhängig sind. Sie können auch eine Abhängigkeit Bereich erstellen, die den erfolgreichen Abschluss einer Gruppe von Vorgängen in einem bestimmten Bereich der Vorgangsnummern, in dem ein Vorgang abhängt. Sie können diese drei grundlegende Szenarios zum Erstellen von m: n-Beziehungen kombinieren.

## <a name="task-dependencies-with-batch-net"></a>Anordnungsbeziehungen mit Stapel .NET

In diesem Artikel besprochen, wie anordnungsbeziehungen konfiguriert mithilfe der [Stapel .NET] [ net_msdn] Bibliothek. Zuerst Artikel erfahren Sie, wie Sie für Ihre Projekte, [anordnungsbeziehung aktivieren](#enable-task-dependencies) , und führen Sie dann vor so [Konfigurieren Sie eine Aufgabe mit Abhängigkeiten](#create-dependent-tasks). Schließlich erläutern wir die [Abhängigkeit Szenarien](#dependency-scenarios) , die Stapel unterstützt.

## <a name="enable-task-dependencies"></a>Aktivieren von anordnungsbeziehungen

Um anordnungsbeziehungen in Ihrer Anwendung Stapel verwenden zu können, müssen Sie zuerst dem Stapel Dienst mitteilen, dass der Auftrag anordnungsbeziehungen verwendet. In .NET Stapel, aktivieren Sie es auf Ihrer [CloudJob] [ net_cloudjob] durch Festlegen der zugehörigen [UsesTaskDependencies] [ net_usestaskdependencies] Eigenschaft zu `true`:

```csharp
CloudJob unboundJob = batchClient.JobOperations.CreateJob( "job001",
    new PoolInformation { PoolId = "pool001" });

// IMPORTANT: This is REQUIRED for using task dependencies.
unboundJob.UsesTaskDependencies = true;
```

In der vorhergehenden Codeausschnitt "BatchClient" ist eine Instanz der [BatchClient] [ net_batchclient] Class.

## <a name="create-dependent-tasks"></a>Abhängige Aufgaben erstellen

Wenn Sie einen Vorgang erstellen, der den erfolgreichen Abschluss der eine oder mehrere Aufgaben abhängig ist, informieren Sie Stapel, dass die Aufgabe "von den anderen Vorgängen abhängt". Konfigurieren Sie die [CloudTask]in .NET Stapel[net_cloudtask]. [DependsOn] [net_dependson] Eigenschaft mit einer Instanz von der [TaskDependencies] [ net_taskdependencies] Klasse:

```csharp
// Task 'Flowers' depends on completion of both 'Rain' and 'Sun'
// before it is run.
new CloudTask("Flowers", "cmd.exe /c echo Flowers")
{
    DependsOn = TaskDependencies.OnIds("Rain", "Sun")
},
```

Dieser Codeausschnitt erstellt eine Aufgabe mit der ID von "Blumen", die auf einem Knoten berechnen ausführen, nur, wenn die Aufgaben mit IDs "Niederschlag" und "Sonne" erfolgreich ausgeführt haben geplant ist.

 > [AZURE.NOTE] Ein Vorgang gilt als abgeschlossen sein soll, wenn es in den Zustand **abgeschlossen** ist und seine **Beenden Code** ist `0`. In .NET Stapel, bedeutet dies eine [CloudTask][net_cloudtask]. [Bundesstaat] [net_taskstate] Wert der Eigenschaft `Completed` und der CloudTasks [TaskExecutionInformation][net_taskexecutioninformation]. [ExitCode] [net_exitcode] Eigenschaftswert ist `0`.

## <a name="dependency-scenarios"></a>Abhängigkeit von Szenarien

Es gibt drei grundlegende Vorgang Abhängigkeit Szenarien, die Sie in Azure Stapel verwenden können: 1: 1, 1: n- und Vorgangsnummer Bereich Abhängigkeit. Diese können kombiniert werden, um eine vierte m: n-Szenario bereitzustellen.

 Szenario&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | Beispiel | |
 :-------------------: | ------------------- | -------------------
 [1: 1](#one-to-one) | *TaskB* hängt *taskA* <p/> *TaskB* wird erst zur Ausführung geplant werden, wenn *TaskA* erfolgreich abgeschlossen wurde | ![Diagramm: Aufgabe 1: 1-Beziehung][1]
 [1: n-](#one-to-many) | *TaskC* hängt von sowohl *TaskA* und *taskB* <p/> *TaskC* wird erst zur Ausführung geplant werden, wenn Sie sowohl die *TaskA* *TaskB* erfolgreich abgeschlossen haben | ![Diagramm: Aufgabe 1: n-Beziehung][2]
 [Vorgangs-ID-Bereich](#task-id-range) | *TaskD* hängt einen Bereich von Aufgaben <p/> *TaskD* wird erst zur Ausführung geplant werden, wenn die Aufgaben mit IDs *1* bis *10* erfolgreich abgeschlossen haben | ![Diagramm: Anordnungsbeziehung-ID-Bereich][3]

>[AZURE.TIP] Sie können die **m: n -** Beziehungen, z. B., wo abhängig Aufgaben, C, D, E und F jedes Aufgaben A und b sind erstellen Dies ist sinnvoll, beispielsweise in parallelisierte vorverarbeitenden Szenarien, die Stelle, an der die Ausgabe von mehreren übergeordneten Aufgaben Ihrer untergeordneten Aufgaben abhängig sind.

### <a name="one-to-one"></a>1: 1

Zum Erstellen einer Aufgabe, die vom erfolgreichen Abschluss von einem anderen Vorgang abhängig ist, geben Sie eine einzelne Aufgabe-ID, um die [TaskDependencies][net_taskdependencies]. [OnId] [net_onid] statische Methode, wenn Sie die [DependsOn] Auffüllen[ net_dependson] Eigenschaft [CloudTask][net_cloudtask].

```csharp
// Task 'taskA' doesn't depend on any other tasks
new CloudTask("taskA", "cmd.exe /c echo taskA"),

// Task 'taskB' depends on completion of task 'taskA'
new CloudTask("taskB", "cmd.exe /c echo taskB")
{
    DependsOn = TaskDependencies.OnId("taskA")
},
```

### <a name="one-to-many"></a>1: n-

Zum Erstellen einer Aufgabe, die vom erfolgreichen Abschluss von mehreren Vorgängen abhängig ist, geben Sie eine Auflistung von Aufgaben-IDs in der [TaskDependencies][net_taskdependencies]. [OnIds] [net_onids] statische Methode, wenn Sie die [DependsOn] Auffüllen[ net_dependson] Eigenschaft [CloudTask][net_cloudtask].

```csharp
// 'Rain' and 'Sun' don't depend on any other tasks
new CloudTask("Rain", "cmd.exe /c echo Rain"),
new CloudTask("Sun", "cmd.exe /c echo Sun"),

// Task 'Flowers' depends on completion of both 'Rain' and 'Sun'
// before it is run.
new CloudTask("Flowers", "cmd.exe /c echo Flowers")
{
    DependsOn = TaskDependencies.OnIds("Rain", "Sun")
},
```

### <a name="task-id-range"></a>Vorgangs-ID-Bereich

Zum Erstellen einer Aufgabe, die abhängig vom erfolgreichen Abschluss einer Gruppe von Aufgaben, deren IDs liegen in einem Bereich ist, Sie angeben der ersten und letzten Vorgang IDs in den Bereich aus, um die [TaskDependencies][net_taskdependencies]. [OnIdRange] [net_onidrange] statische Methode, wenn Sie die [DependsOn] Auffüllen[ net_dependson] Eigenschaft [CloudTask][net_cloudtask].

>[AZURE.IMPORTANT] Wenn Sie für Ihre Abhängigkeiten Task-ID-Bereiche verwenden, sein Vorgangsnummern in den Bereich *muss* Zeichenfolge Darstellungen von ganzzahlige Werte. Darüber hinaus muss alle Aufgaben im Bereich für den abhängige Vorgang zur Ausführung geplant werden erfolgreich abgeschlossen.

```csharp
// Tasks 1, 2, and 3 don't depend on any other tasks. Because
// we will be using them for a task range dependency, we must
// specify string representations of integers as their ids.
new CloudTask("1", "cmd.exe /c echo 1"),
new CloudTask("2", "cmd.exe /c echo 2"),
new CloudTask("3", "cmd.exe /c echo 3"),

// Task 4 depends on a range of tasks, 1 through 3
new CloudTask("4", "cmd.exe /c echo 4")
{
    // To use a range of tasks, their ids must be integer values.
    // Note that we pass integers as parameters to TaskIdRange,
    // but their ids (above) are string representations of the ids.
    DependsOn = TaskDependencies.OnIdRange(1, 3)
},
```

## <a name="code-sample"></a>Beispiel für Code

Die [TaskDependencies] [ github_taskdependencies] Beispielprojekt ist eine der [Azure Stapel Codebeispielen] [ github_samples] auf GitHub. Die Lösung in Visual Studio 2015 veranschaulicht aktivieren anordnungsbeziehung für ein Projekt, Erstellen von Aufgaben, die von anderen Aufgaben abhängen, und führen Sie diese Aufgaben auf einem Ressourcenpool von Computeknoten.

## <a name="next-steps"></a>Nächste Schritte

### <a name="application-deployment"></a>Bereitstellung der Anwendung

Das Feature [Anwendungspakete](batch-application-packages.md) des Blatts bietet eine einfache Möglichkeit zum Bereitstellen und Version der Anwendung, die auf Ihre Aufgaben ausführen Knoten zu berechnen.

### <a name="installing-applications-and-staging-data"></a>Installieren von Applications und das staging von Daten

Schauen Sie sich das [Installieren von Applications und staging Daten auf Blatt berechnen Knoten] [ forum_post] Posten im Stapel Azure-Forum für einen Überblick über die verschiedenen Methoden zum Vorbereiten Ihrer Knoten Aufgaben ausführen. Mit einem der Teammitglieder Azure Stapel geschrieben, ist diesen Beitrag gute Einführung in den verschiedenen Methoden zum Abrufen von Dateien (einschließlich sowohl Applikationen und Vorgang Eingabedaten) auf Ihre Knoten berechnen.

[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[github_taskdependencies]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/TaskDependencies
[github_samples]: https://github.com/Azure/azure-batch-samples
[net_batchclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_dependson]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.dependson.aspx
[net_exitcode]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskexecutioninformation.exitcode.aspx
[net_msdn]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_onid]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onid.aspx
[net_onids]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onids.aspx
[net_onidrange]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onidrange.aspx
[net_taskexecutioninformation]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskexecutioninformation.aspx
[net_taskstate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.taskstate.aspx
[net_usestaskdependencies]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.usestaskdependencies.aspx
[net_taskdependencies]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskdependencies.aspx

[1]: ./media/batch-task-dependency/01_one_to_one.png "Diagramm: 1: 1-Beziehung"
[2]: ./media/batch-task-dependency/02_one_to_many.png "Diagramm: 1: n-Beziehung"
[3]: ./media/batch-task-dependency/03_task_id_range.png "Diagramm: anordnungsbeziehung-ID-Bereich"

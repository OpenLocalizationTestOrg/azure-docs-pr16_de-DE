<properties
    pageTitle="MPI Applications in Azure Stapel mit mit mehreren Instanzen Aufgaben ausführen | Microsoft Azure"
    description="Erfahren Sie, wie in der Nachricht übergeben Interface (MPI) Programme mit der Vorgangsart mit mehreren Instanzen in Azure Stapel ausgeführt."
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
    ms.date="10/21/2016"
    ms.author="marsma" />

# <a name="use-multi-instance-tasks-to-run-message-passing-interface-mpi-applications-in-azure-batch"></a>Verwenden von Aufgaben mit mehreren Instanzen zum Ausführen von Applications Nachricht übergeben Interface (MPI) in Azure Stapel

Mehrere Instanz Aufgaben können Sie eine Aufgabe Azure Stapel gleichzeitig auf mehreren berechnen Knoten ausführen. Die folgenden Aufgaben aktivieren hohen Performance computing Szenarien wie Nachricht übergeben Interface (MPI) Applications in Stapel. In diesem Artikel erfahren Sie, wie Sie mit mehreren Instanzen von Aufgaben mit [Stapel .NET] ausführen[ api_net] Bibliothek.

>[AZURE.NOTE] Während die Beispiele in diesem Artikel beziehen sich auf Stapel .NET, MS-MPI und Windows Knoten zu berechnen, gelten die hier beschriebenen mit mehreren Instanzen Vorgang-Konzepte zu anderen Plattformen und Technologien (Python und Intel MPI auf Linux Knoten, beispielsweise).

## <a name="multi-instance-task-overview"></a>Übersicht über die Aufgabe mit mehreren Instanzen

Im Stapel, jede Aufgabe ist normalerweise übermitteln Sie mehrere Aufgaben für ein Projekt auf einem einzigen Rechner-Knoten ausgeführt, und der Dienst Stapel plant jeden Vorgang für die Ausführung auf einem Knoten. Durch Konfigurieren einer Aufgabe **mit mehreren Instanzen Einstellungen**an, erfahren Sie jedoch Stapel stattdessen Erstellen einer primären Vorgangs- und mehrere Teilvorgänge, die Sie dann auf mehreren Knoten ausgeführt werden.

![Übersicht über die Aufgabe mit mehreren Instanzen][1]

Wenn Sie eine Aufgabe mit Einstellungen eines Auftrags mit mehreren Instanzen senden, führt Stapel mehrere Schritte aus eindeutigen mit mehreren Instanzen Vorgängen:

1. Der Dienst Stapel erstellt einen **primären** und mehrere **Teilvorgänge** basierend auf der Einstellungen für mehrere Instanzen. Die Gesamtzahl der Aufgaben (plus alle Teilvorgänge primär) entspricht die Anzahl der **Instanzen** (Datenverarbeitungsknoten) Sie in den Einstellungen mit mehreren Instanzen angeben.
1. Stapel weist eine Computeknoten als **master**und für das Master-Shape nicht ausführen der primären Vorgang geplant. Es wird die Teilvorgänge auf den verbleibenden Computeknoten zugeordnet wird, die dem Vorgang mit mehreren Instanzen, eine Teilvorgangs pro Knoten auszuführende geplant.
1. Der primären und alle Teilvorgänge herunterladen **gemeinsame Ressourcendateien** , die Sie in den Einstellungen mit mehreren Instanzen angeben.
1. Nach der gemeinsame Ressource Dateien heruntergeladen wurden, die primäre und Teilvorgänge Sie, in den Einstellungen mit mehreren Instanzen angeben **Koordinierung Befehl** ausführen. Der Befehl Koordinierung wird in der Regel zum Knoten Vorbereiten für das Ausführen der Aufgabe. Dies kann das Starten von Diensten Hintergrund beinhalten (wie etwa [Microsoft MPI][msmpi_msdn]des `smpd.exe`) und überprüfen, ob die Knoten zum Verarbeiten von Nachrichten zwischen den Knoten bereit sind.
1. Die primäre Aufgabe führt den **Anwendungsbefehl** auf das master-Knoten *nach* , die der Befehl Koordinierung von der primären und alle Teilvorgänge erfolgreich abgeschlossen wurde. Der Anwendungsbefehl ist die Befehlszeile des Vorgangs mit mehreren Instanzen selbst und wird nur von der primären Vorgang ausgeführt. In einer [MS MPI][msmpi_msdn]-basierte Lösung, dies ist, in dem Sie Ihre MPI-fähige Anwendung mit ausführen `mpiexec.exe`.

> [AZURE.NOTE] Obwohl sie funktional distinct ist, der Vorgang"mit mehreren Instanzen" ist nicht eindeutigen Vorgangsart wie die [StartTask] [ net_starttask] oder [JobPreparationTask][net_jobprep]. Der Vorgang mit mehreren Instanzen ist einfach ein Standardkatalogs Stapel ([CloudTask] [ net_task] in Stapel .NET), dessen Einstellungen mit mehreren Instanzen konfiguriert wurden. In diesem Artikel verwenden wir Sie als **Aufgabe mit mehreren Instanzen**.

## <a name="requirements-for-multi-instance-tasks"></a>Anforderungen für Vorgänge mit mehreren Instanzen

Vorgänge mit mehreren Instanzen erfordern einen Pool mit der **Kommunikation zwischen den Knoten, die aktiviert**, und klicken Sie mit der **Ausführung der gleichzeitigen Aufgabe deaktiviert**. Wenn Sie versuchen, eine Aufgabe mit mehreren Instanzen in einem Ressourcenpool mit Symbol Kommunikation deaktiviert auszuführen oder mit einem *MaxTasksPerNode* -Wert größer als 1, nie geplant wird – verbleibt endlos in den Zustand "aktiv". Dieser Codeausschnitt zeigt die Erstellung einer solchen einem Ressourcenpool mithilfe der Bibliothek Stapel .NET.

```csharp
CloudPool myCloudPool =
    myBatchClient.PoolOperations.CreatePool(
        poolId: "MultiInstanceSamplePool",
        targetDedicated: 3
        virtualMachineSize: "small",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));

// Multi-instance tasks require inter-node communication, and those nodes
// must run only one task at a time.
myCloudPool.InterComputeNodeCommunicationEnabled = true;
myCloudPool.MaxTasksPerComputeNode = 1;
```

Darüber hinaus können mit mehreren Instanzen Aufgaben *nur* auf Knoten im **Pools erstellt nach 14 Dezember 2015**ausführen.

### <a name="use-a-starttask-to-install-mpi"></a>Verwenden einer StartTask, um MPI zu installieren.

Um MPI Applikationen über eine Aufgabe mit mehreren Instanzen ausführen zu können, müssen Sie zuerst eine MPI-Implementierung (MS-MPI oder Intel MPI, beispielsweise) auf den Knoten berechnen, in dem Pool zu installieren. Dies ist ein guter Zeitpunkt mit einer [StartTask][net_starttask], das bei jedem ein Knoten einem Ressourcenpool verknüpft oder neu gestartet wird ausgeführt. Dieser Codeausschnitt erstellt ein, die angibt, das MS MPI Setup-Paket als [Ressourcendatei]StartTask[net_resourcefile]. Der Start-Aufgabe Befehlszeile wird ausgeführt, nachdem die Ressourcendatei auf den Knoten heruntergeladen wurde. In diesem Fall führt die Befehlszeile eine unbeaufsichtigte Installation von MS-MPI.

```csharp
// Create a StartTask for the pool which we use for installing MS-MPI on
// the nodes as they join the pool (or when they are restarted).
StartTask startTask = new StartTask
{
    CommandLine = "cmd /c MSMpiSetup.exe -unattend -force",
    ResourceFiles = new List<ResourceFile> { new ResourceFile("https://mystorageaccount.blob.core.windows.net/mycontainer/MSMpiSetup.exe", "MSMpiSetup.exe") },
    RunElevated = true,
    WaitForSuccess = true
};
myCloudPool.StartTask = startTask;

// Commit the fully configured pool to the Batch service to actually create
// the pool and its compute nodes.
await myCloudPool.CommitAsync();
```

### <a name="remote-direct-memory-access-rdma"></a>Zugriff auf den Remote direkte Speicher (RDMA)

Wenn Sie eine [RDMA-fähigen Größe](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md) wie A9 in Ihrem Stapel Ressourcenpool für die berechnen-Knoten auswählen, kann Ihrer Anwendung MPI des Azure leistungsfähige und niedrig Wartezeiten remote direkte Arbeitsspeicher Access (RDMA) Netzwerk nutzen.

Suchen Sie nach der Größen angegebenen als "RDMA in" in den folgenden Artikeln:

* **CloudServiceConfiguration** pools

  * [Größen für Cloud-Dienste](../cloud-services/cloud-services-sizes-specs.md) (Nur Windows)

* **VirtualMachineConfiguration** pools

  * [Größen für virtuellen Computern in Azure](../virtual-machines/virtual-machines-linux-sizes.md) (Linux)

  * [Größen für virtuellen Computern in Azure](../virtual-machines/virtual-machines-windows-sizes.md) (Windows)

>[AZURE.NOTE] Um RDMA auf [Linux berechnen Knoten](batch-linux-nodes.md)nutzen zu können, müssen Sie auf den Knoten **Intel MPI** verwenden. Weitere Informationen über CloudServiceConfiguration und VirtualMachineConfiguration Pools finden Sie unter Abschnitt [Pool](batch-api-basics.md#Pool) der Übersicht über die Stapel-Features.

## <a name="create-a-multi-instance-task-with-batch-net"></a>Erstellen Sie eine Aufgabe mit mehreren Instanzen mit Stapel .NET

Jetzt, da wir die Ressourcenpool Anforderungen und MPI Paketinstallation gesprochen haben, uns die Aufgabe mit mehreren Instanzen erstellen. In diesem Codeausschnitt erstellen wir eine standard [CloudTask][net_task], konfigurieren Sie deren [MultiInstanceSettings] [ net_multiinstance_prop] Eigenschaft. Wie zuvor schon erwähnt, ist die Aufgabe mit mehreren Instanzen nicht distinct Vorgangsart, aber einen standard Stapel Vorgang mit Einstellungen mit mehreren Instanzen konfiguriert.

```csharp
// Create the multi-instance task. Its command line is the "application command"
// and will be executed *only* by the primary, and only after the primary and
// subtasks execute the CoordinationCommandLine.
CloudTask myMultiInstanceTask = new CloudTask(id: "mymultiinstancetask",
    commandline: "cmd /c mpiexec.exe -wdir %AZ_BATCH_TASK_SHARED_DIR% MyMPIApplication.exe");

// Configure the task's MultiInstanceSettings. The CoordinationCommandLine will be executed by
// the primary and all subtasks.
myMultiInstanceTask.MultiInstanceSettings =
    new MultiInstanceSettings(numberOfNodes) {
    CoordinationCommandLine = @"cmd /c start cmd /c ""%MSMPI_BIN%\smpd.exe"" -d",
    CommonResourceFiles = new List<ResourceFile> {
    new ResourceFile("https://mystorageaccount.blob.core.windows.net/mycontainer/MyMPIApplication.exe",
                     "MyMPIApplication.exe")
    }
};

// Submit the task to the job. Batch will take care of splitting it into subtasks and
// scheduling them for execution on the nodes.
await myBatchClient.JobOperations.AddTaskAsync("mybatchjob", myMultiInstanceTask);
```

## <a name="primary-task-and-subtasks"></a>Primäre Vorgangs- und Teilvorgänge

Wenn Sie die Einstellungen mit mehreren Instanzen für einen Vorgang erstellen, geben Sie die Anzahl der Knoten berechnen, die zum Ausführen der Aufgabe sind. Wenn Sie den Vorgang mit einem Projekt übermitteln, erstellt der Stapel-Dienst eine **primäre** Aufgabe und genügend **Teilvorgänge** , die die Anzahl der Knoten zusammen zu entsprechen, die Sie angegeben haben.

Diese Aufgaben werden *NumberOfInstances* - 1 Personalnummer ganze Zahl im Bereich von 0 zugewiesen. Die Aufgabe mit der Id 0 ist die primäre, und alle anderen Ids sind Teilvorgänge. Wenn Sie die folgenden mit mehreren Instanzen Einstellungen für einen Vorgang erstellen, die primäre Aufgabe müssten Personalnummer 0 ein, und die Teilvorgänge müssten Ids 1 bis 9.

```csharp
int numberOfNodes = 10;
myMultiInstanceTask.MultiInstanceSettings = new MultiInstanceSettings(numberOfNodes);
```

### <a name="master-node"></a>Master-Knoten

Wenn Sie eine Aufgabe mit mehreren Instanzen übermitteln, Stapel Dienst kennzeichnet eine Computeknoten als "master"-Knoten, und wird der primären Vorgang auszuführende auf master-Knoten geplant. Der Teilvorgänge sind für den Rest der Knoten zugewiesen, die dem Vorgang mit mehreren Instanzen ausgeführt werden.

## <a name="coordination-command"></a>Koordinierung Befehl

Der **Koordinierung Befehl** wird ausgeführt, indem Sie den Primär- und Teilvorgängen.

Das Aufrufen des Befehls Koordinierung blockiert – Stapelverarbeitung nicht den Anwendungsbefehl ausgeführt, bis der Befehl Koordinierung für alle Teilvorgänge erfolgreich zurückgegeben wurde. Der Befehl Koordinierung sollten daher alle erforderlichen Hintergrunddienste beginnen, stellen Sie sicher, dass sie zur Verwendung bereit sind, und beenden Sie dann. Angenommen, dieser Befehl Koordinierung für eine Lösung mit MS MPI Version, dass 7 der SMPD-Dienst auf dem Knoten gestartet wird dann beendet wird:

```
cmd /c start cmd /c ""%MSMPI_BIN%\smpd.exe"" -d
```

Beachten Sie die Verwendung der `start` in dieser Befehl Koordinierung. Dies ist erforderlich, da die `smpd.exe` Anwendung nicht sofort nach ihrer Ausführung zurück. Ohne Verwendung von [Starten] [ cmd_start] Befehl, diesen Befehl Koordinierung nicht zurück, und den Anwendungsbefehl würde daher blockieren.

## <a name="application-command"></a>Befehl ' Anwendung '

Sobald die primäre Vorgangs- und alle Teilvorgänge Ausführen des Befehls Koordinierung abgeschlossen haben, wird durch die primäre Aufgabe *nur*mit mehreren Instanzen des Vorgangs Befehlszeile ausgeführt. Wir nennen dies den **Anwendungsbefehl** von den Befehl Koordinierung zu unterscheiden.

Für MS-MPI Applikationen, mithilfe des Befehls Anwendung ausführen Ihrer Anwendung MPI aktiviert mit `mpiexec.exe`. So sieht beispielsweise ein Anwendungsbefehl für eine Lösung mit MS-MPI Version 7 aus:

```
cmd /c ""%MSMPI_BIN%\mpiexec.exe"" -c 1 -wdir %AZ_BATCH_TASK_SHARED_DIR% MyMPIApplication.exe
```

>[AZURE.NOTE] Da MS-MPI `mpiexec.exe` verwendet die `CCP_NODES` Variable von Standard im Beispiel (siehe [Umgebungsvariablen](#environment-variables)) Befehlszeile oben ausgeschlossen.

## <a name="environment-variables"></a>Umgebungsvariablen

Stapel erstellt mehrere [Umgebungsvariablen] [ msdn_env_var] speziell für mehrere Instanzen Aufgaben auf den berechnen Knoten zu einem Vorgang mit mehreren Instanzen zugewiesen. Ihre Koordinierung und Anwendung Befehl Linien können diese Umgebungsvariablen verweisen, wie die Skripts und Programme, die sie ausführen können.

Die folgenden Umgebungsvariablen werden vom Dienst Stapel für die Verwendung von Aufgaben mit mehreren Instanzen erstellt:

* `CCP_NODES`
* `AZ_BATCH_NODE_LIST`
* `AZ_BATCH_HOST_LIST`
* `AZ_BATCH_MASTER_NODE`
* `AZ_BATCH_TASK_SHARED_DIR`
* `AZ_BATCH_IS_CURRENT_NODE_MASTER`

Vollständige Details zu diesen und die anderen Stapel berechnen Knoten Umgebungsvariablen, einschließlich ihrer Inhalt und Sichtbarkeit finden Sie unter [berechnen Knoten Umgebungsvariablen][msdn_env_var].

>[AZURE.TIP] Im Beispiel Stapel Linux MPI enthält ein Beispiel, wie mehrere dieser Variablen verwendet werden können. Die [Koordinierung-Cmd] [ coord_cmd_example] Bash Skript downloads allgemeine Anwendung und Eingabe-Dateien aus dem Azure-Speicher, ermöglicht eine Freigabe (NFS = Network File System) auf das master-Knoten und konfiguriert die anderen Knoten als NFS-Clients, die dem Vorgang mit mehreren Instanzen zugeordnet.

## <a name="resource-files"></a>Ressourcendateien

Es gibt zwei Sätze von Ressourcendateien für Vorgänge mit mehreren Instanzen berücksichtigen: **gemeinsame Ressourcendateien** , die *Alle* Vorgänge herunterladen (Primär- und Teilvorgänge), und die **Ressourcendateien** für mehrere Instanzen angegebenen selbst, welche *nur der primäre* Aufgabe downloads des Vorgangs.

Sie können eine oder mehrere **gemeinsame Ressourcendateien** in den Einstellungen mit mehreren Instanzen für einen Vorgang angeben. Diese allgemeine Ressourcendateien werden in des Knotens **freigegebene Verzeichnis Aufgabe** nach der primären und alle Teilvorgänge aus [Azure-Speicher](./../storage/storage-introduction.md) heruntergeladen. Können Sie Vorgang freigegebenen Verzeichnis mithilfe von Anwendung und koordinieren Befehl Linien zugreifen der `AZ_BATCH_TASK_SHARED_DIR` Umgebungsvariable. Die `AZ_BATCH_TASK_SHARED_DIR` Pfad auf den einzelnen Knoten, die dem Vorgang mit mehreren Instanzen zugeordnet ist, daher können Sie einen einzelnen Koordinierung Befehl zwischen der primären und alle Teilvorgänge freigeben. Stapel nicht "freigeben" Verzeichnis in einem remote Access sinnvoll, aber Sie können nicht als eine Zuordnung verwendet oder Punkt wie weiter oben in der QuickInfo auf Umgebungsvariablen erwähnt freigeben.

Ressourcendateien, die Sie angeben, für den Vorgang mit mehreren Instanzen selbst in geöffneten Verzeichnis des Vorgangs, heruntergeladen werden `AZ_BATCH_TASK_WORKING_DIR`, standardmäßig. Wie erwähnt, im Gegensatz zu gemeinsame Ressourcendateien downloads nur der primäre Vorgang Ressourcendateien, die für den Vorgang mit mehreren Instanzen selbst angegeben.

> [AZURE.IMPORTANT] Verwenden Sie die Umgebungsvariablen immer `AZ_BATCH_TASK_SHARED_DIR` und `AZ_BATCH_TASK_WORKING_DIR` verweisen auf diese Verzeichnisse in der Befehl Linien. Führen Sie die Pfade manuell zu erstellen.

## <a name="task-lifetime"></a>Aufgabe Lebensdauer

Die Gültigkeitsdauer des primären Vorgangs steuert die Gültigkeitsdauer des gesamten mit mehreren Instanzen Vorgangs. Wenn die primäre beendet wird, werden alle Teilvorgänge beendet. Der Beendigungscode der primären ist der Beendigungscode des Vorgangs und wird daher zum Bestimmen der Erfolg oder das Fehlschlagen des Vorgangs "Wiederholen" Zwecken verwendet.

Wenn einer der Teilvorgänge fehlschlägt, schlägt Beenden einer ungleich NULL-Code zurück, beispielsweise der gesamten mit mehreren Instanzen Vorgang. Der Vorgang mit mehreren Instanzen ist dann beendet und wiederholt, bis die maximale Anzahl der erneuten.

Wenn Sie eine Aufgabe mit mehreren Instanzen löschen, werden die primäre und alle Teilvorgänge durch den Stapel-Dienst ebenfalls gelöscht. Alle Unteraufgabe Verzeichnisse durchsuchen, und ihre Dateien werden von den Knoten berechnen, genau wie bei einer Standardkatalogs gelöscht.

[TaskConstraints] [ net_taskconstraints] für einen Vorgang mit mehreren Instanzen, wie etwa die [MaxTaskRetryCount][net_taskconstraint_maxretry], [MaxWallClockTime][net_taskconstraint_maxwallclock], und [RetentionTime] [ net_taskconstraint_retention] Eigenschaften, werden berücksichtigt werden, sobald sie für einen Vorgang standard sind, und beziehen sich auf dem primären und alle Teilvorgänge. Jedoch, wenn Sie die [RetentionTime] ändern[ net_taskconstraint_retention] Eigenschaft nach dem Hinzufügen eines Vorgangs mit mehreren Instanzen dem Projekt diese Änderung nur auf die primäre Aufgabe angewendet wird. Alle Teilvorgänge weiterhin verwenden, die ursprüngliche [RetentionTime][net_taskconstraint_retention].

Liste der zuletzt verwendeten Vorgang einen Computeknoten spiegeln die Id eines Teilvorgangs, wenn die aktuelle Aufgabe Teil eines Vorgangs mit mehreren Instanzen wurde.

## <a name="obtain-information-about-subtasks"></a>Abrufen von Informationen zu Teilvorgängen

Um Informationen zu Teilvorgängen erhalten mithilfe der Objektbibliothek Stapel .NET, rufen Sie die [CloudTask.ListSubtasks] [ net_task_listsubtasks] Methode. Diese Methode wird auf alle Teilvorgänge, und Weitere Informationen zum Berechnungsknoten, der die Aufgaben ausgeführt. Anhand dieser Informationen können Sie bestimmen, des Teilvorgangs Stammverzeichnis, die Pool-Id, aktuellen Zustand, Beendigungscode und mehr. Mithilfe dieser Informationen können Sie in Kombination mit der [PoolOperations.GetNodeFile] [ poolops_getnodefile] Methode, um die Dateien des Teilvorgangs zu erhalten. Beachten Sie, dass diese Methode keine Informationen für den primären Vorgang (Id 0) zurückgibt.

> [AZURE.NOTE] Sofern nicht anders angegeben, Stapel .NET Methoden, die die Steuerung auf die mit mehreren Instanzen [CloudTask] [ net_task] selbst beziehen sich *nur* auf die primäre Aufgabe. Wenn Sie beispielsweise die [CloudTask.ListNodeFiles] anrufen[ net_task_listnodefiles] Methode für einen Vorgang mit mehreren Instanzen, nur die primäre Aufgabe Dateien werden zurückgegeben.

Der folgende Codeausschnitt veranschaulicht, wie Teilvorgangsinformationen zu erhalten, als auch die Grundlage ihrer Ausführung Knoten Dateiinhalt beantragen.

```csharp
// Obtain the job and the multi-instance task from the Batch service
CloudJob boundJob = batchClient.JobOperations.GetJob("mybatchjob");
CloudTask myMultiInstanceTask = boundJob.GetTask("mymultiinstancetask");

// Now obtain the list of subtasks for the task
IPagedEnumerable<SubtaskInformation> subtasks = myMultiInstanceTask.ListSubtasks();

// Asynchronously iterate over the subtasks and print their stdout and stderr
// output if the subtask has completed
await subtasks.ForEachAsync(async (subtask) =>
{
    Console.WriteLine("subtask: {0}", subtask.Id);
    Console.WriteLine("exit code: {0}", subtask.ExitCode);

    if (subtask.State == TaskState.Completed)
    {
        ComputeNode node =
            await batchClient.PoolOperations.GetComputeNodeAsync(subtask.ComputeNodeInformation.PoolId,
                                                                 subtask.ComputeNodeInformation.ComputeNodeId);

        NodeFile stdOutFile = await node.GetNodeFileAsync(subtask.ComputeNodeInformation.TaskRootDirectory + "\\" + Constants.StandardOutFileName);
        NodeFile stdErrFile = await node.GetNodeFileAsync(subtask.ComputeNodeInformation.TaskRootDirectory + "\\" + Constants.StandardErrorFileName);
        stdOut = await stdOutFile.ReadAsStringAsync();
        stdErr = await stdErrFile.ReadAsStringAsync();

        Console.WriteLine("node: {0}:", node.Id);
        Console.WriteLine("stdout.txt: {0}", stdOut);
        Console.WriteLine("stderr.txt: {0}", stdErr);
    }
    else
    {
        Console.WriteLine("\tSubtask {0} is in state {1}", subtask.Id, subtask.State);
    }
});
```

## <a name="code-sample"></a>Beispiel für Code

Die [MultiInstanceTasks] [ github_mpi] Code Stichprobe auf GitHub veranschaulicht, wie eine Aufgabe mit mehreren Instanzen zum Ausführen einer [MS MPI] [ msmpi_msdn] Anwendung Blattnamen Knoten zu berechnen. Führen Sie die Schritte [zur Vorbereitung](#preparation) und [Ausführung](#execution) , um das Beispiel auszuführen.

### <a name="preparation"></a>Vorbereitung

1. Führen Sie die ersten beiden Schritte [so ein einfaches MS MPI Programm kompilieren und Ausführen][msmpi_howto]. Dies erfüllt die Prerequesites für die folgenden Schritte aus.
1. Erstellen eine *freigegebenen* Version der [MPIHelloWorld] [ helloworld_proj] Stichprobe MPI-Programm. Dies ist das Programm, das von der Aufgabe mit mehreren Instanzen auf Datenverarbeitungsknoten ausgeführt wird.
1. Erstellen einer Zip-Datei mit `MPIHelloWorld.exe` (die Sie erstellt Schritt 2) und `MSMpiSetup.exe` (die Sie heruntergeladen Schritt 1). Sie können diese Zip-Datei als ein Anwendungspaket im nächsten Schritt hochladen.
1. Verwenden der [Azure-Portal] [ portal] zum Erstellen einer Stapel [Anwendung](batch-application-packages.md) mit dem Namen "MPIHelloWorld", und geben Sie die Zip-Datei erstellen im vorherigen Schritt als Version "1.0" des Anwendungspakets. Weitere Informationen finden Sie unter [Hochladen und Verwalten von Applications](batch-application-packages.md#upload-and-manage-applications) .

>[AZURE.TIP] Erstellen Sie eine *veröffentlichte* Version von `MPIHelloWorld.exe` , damit Sie keine zusätzlichen Abhängigkeiten aufnehmen möchten (z. B. `msvcp140d.dll` oder `vcruntime140d.dll`) in Ihrem Anwendungspaket.

### <a name="execution"></a>Ausführung

1. Herunterladen der [Azure-Stapel-Beispiele] [ github_samples_zip] aus GitHub.
1. Öffnen Sie die MultiInstanceTasks- **Lösung** in Visual Studio 2015. Die `MultiInstanceTasks.sln` Lösungsdatei befindet sich:

    `azure-batch-samples\CSharp\ArticleProjects\MultiInstanceTasks\`

1. Geben Sie Ihre Anmeldeinformationen ein Stapel und Speicher-Konto in `AccountSettings.settings` im Projekt **Microsoft.Azure.Batch.Samples.Common** .
1. **Erstellen und führen Sie** die Lösung MultiInstanceTasks zum Ausführen der MPI-Beispiel auf Knoten in einem Stapel Ressourcenpool zu berechnen.
1. *Optional*: Verwenden der [Azure-Portal] [ portal] oder den [Stapel Explorer] [ batch_explorer] Untersuchen der Stichprobe Ressourcenpool, Position und Aufgabe ("MultiInstanceSamplePool", "MultiInstanceSampleJob", "MultiInstanceSampleTask") aus, bevor Sie Ressourcen löschen.

>[AZURE.TIP] Sie können [Visual Studio-Community] herunterladen[ visual_studio] kostenlos, wenn Sie nicht Visual Studio verfügen.

Ausgabe von `MultiInstanceTasks.exe` ähnelt dem folgenden:

```
Creating pool [MultiInstanceSamplePool]...
Creating job [MultiInstanceSampleJob]...
Adding task [MultiInstanceSampleTask] to job [MultiInstanceSampleJob]...
Awaiting task completion, timeout in 00:30:00...

Main task [MultiInstanceSampleTask] is in state [Completed] and ran on compute node [tvm-1219235766_1-20161017t162002z]:
---- stdout.txt ----
Rank 2 received string "Hello world" from Rank 0
Rank 1 received string "Hello world" from Rank 0

---- stderr.txt ----

Main task completed, waiting 00:00:10 for subtasks to complete...

---- Subtask information ----
subtask: 1
        exit code: 0
        node: tvm-1219235766_3-20161017t162002z
        stdout.txt:
        stderr.txt:
subtask: 2
        exit code: 0
        node: tvm-1219235766_2-20161017t162002z
        stdout.txt:
        stderr.txt:

Delete job? [yes] no: yes
Delete pool? [yes] no: yes

Sample complete, hit ENTER to exit...
```

## <a name="next-steps"></a>Nächste Schritte

- Der Microsoft HPC und Azure Stapel Team-Blog erläutert [MPI Unterstützung für Linux Azure Blattnamen][blog_mpi_linux], und enthält Informationen zur Verwendung von [OpenFOAM] [ openfoam] mit Stapel. Python Codebeispielen finden Sie die [OpenFOAM Beispiel auf GitHub][github_mpi].

- Erfahren Sie, wie für die Verwendung in Ihrer Azure Stapel MPI Lösungen [Linux berechnen Hierarchieknoten zu erstellen](batch-linux-nodes.md) .

[helloworld_proj]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/MultiInstanceTasks/MPIHelloWorld

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[blog_mpi_linux]: https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/
[cmd_start]: https://technet.microsoft.com/library/cc770297.aspx
[coord_cmd_example]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/article_samples/mpi/data/linux/openfoam/coordination-cmd
[github_mpi]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/MultiInstanceTasks
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_zip]: https://github.com/Azure/azure-batch-samples/archive/master.zip
[msdn_env_var]: https://msdn.microsoft.com/library/azure/mt743623.aspx
[msmpi_msdn]: https://msdn.microsoft.com/library/bb524831.aspx
[msmpi_sdk]: http://go.microsoft.com/FWLink/p/?LinkID=389556
[msmpi_howto]: http://blogs.technet.com/b/windowshpc/archive/2015/02/02/how-to-compile-and-run-a-simple-ms-mpi-program.aspx
[openfoam]: http://www.openfoam.com/
[visual_studio]: https://www.visualstudio.com/vs/community/

[net_jobprep]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.aspx
[net_multiinstance_class]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.aspx
[net_multiinstance_prop]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.multiinstancesettings.aspx
[net_multiinsance_commonresfiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.commonresourcefiles.aspx
[net_multiinstance_coordcmdline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.coordinationcommandline.aspx
[net_multiinstance_numinstances]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.numberofinstances.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_pool_create]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[net_pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_resourcefile]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.resourcefile.aspx
[net_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.aspx
[net_starttask_cmdline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.commandline.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_taskconstraints]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.aspx
[net_taskconstraint_maxretry]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.maxtaskretrycount.aspx
[net_taskconstraint_maxwallclock]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.maxwallclocktime.aspx
[net_taskconstraint_retention]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.retentiontime.aspx
[net_task_listsubtasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listsubtasks.aspx
[net_task_listnodefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listnodefiles.aspx
[poolops_getnodefile]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.getnodefile.aspx

[portal]: https://portal.azure.com
[rest_multiinstance]: https://msdn.microsoft.com/library/azure/mt637905.aspx

[1]: ./media/batch-mpi/batch_mpi_01.png "Mit mehreren Instanzen (Übersicht)"

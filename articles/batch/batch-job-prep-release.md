<properties
    pageTitle="Vorbereitung und Aufräumen in Stapel der beruflichen Position | Microsoft Azure"
    description="Verwenden Auftrag Ebene-vorbereitende Aufgaben zu minimieren Datenübertragung Azure Stapel Knoten zu berechnen, und lassen Sie wieder los Aufgaben für Knoten Aufräumen bei Auftragsabschluss."
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
    ms.date="09/16/2016"
    ms.author="marsma" />

# <a name="run-job-preparation-and-completion-tasks-on-azure-batch-compute-nodes"></a>Ausführen Vorbereitung und Fertigstellung Projektaufgaben Azure Blattnamen berechnen Knoten

 Eine Azure Stapelverarbeitung erfordert häufig eine Art des Installationsprogramms vor ihrer Aufgaben ausgeführt werden, und nach der beruflichen Position Wartung, wenn seine Vorgänge abgeschlossen sind. Möglicherweise müssen allgemeine Aufgabe eingegebenen Daten zu Ihrer Datenverarbeitungsknoten herunterladen oder Hochladen Ausgabedaten Vorgang zum Azure-Speicher nach Auftragsabschluss. **Auftrag zur Vorbereitung** und **Auftrag freizugeben** Aufgaben können zum Ausführen dieser Vorgänge.

## <a name="what-are-job-preparation-and-release-tasks"></a>Was sind Auftrag zur Vorbereitung und lassen Sie wieder los Aufgaben?

Vor dem des Projekts Aufgaben ausführen zu können, führt des Auftrag zur Vorbereitung Vorgangs auf allen berechnen Knoten so führen Sie mindestens einen Vorgang geplant. Nachdem Sie der Auftrag abgeschlossen ist, wird der Task freigeben Position auf den einzelnen Knoten im Pool, der mindestens einen Vorgang ausgeführt ausgeführt. Wie bei normalen Stapelverarbeitungsaufgaben können Sie angeben, eine Befehlszeile aufgerufen, wenn eine Aufgabe zur Vorbereitung oder Version ausgeführt wird.

Projektaufgaben Vorbereitung und Release bieten vertraute Stapel Vorgang Features wie Dateidownload ([Ressourcendateien][net_job_prep_resourcefiles]), erhöhten Ausführung, benutzerdefinierte Umgebungsvariablen, Dauer der maximalen Ausführung, "Wiederholen" zählen und Datei Aufbewahrungszeit.

In den folgenden Abschnitten erfahren Sie, wie Sie verwenden die [JobPreparationTask] [ net_job_prep] und [JobReleaseTask] [ net_job_release] Klassen gefunden in [.NET Stapel] [ api_net] Bibliothek.

> [AZURE.TIP] Vorbereitung und Release Projektaufgaben sind besonders hilfreich, im Umgebungen "Pool gemeinsam", die ein Pool von Computeknoten zwischen Auftrag ausgeführt bleibt und von vielen Aufträge verwendet.

## <a name="when-to-use-job-preparation-and-release-tasks"></a>Wann Auftrag zur Vorbereitung und Freigeben von Aufgaben

Auftrag zur Vorbereitung und Release Projektaufgaben sind gut für den folgenden Situationen:

**Allgemeine Aufgabendaten herunterladen**

Stapelverarbeitung erfordern häufig einen gemeinsamen Satz von Daten als Eingabe für Vorgänge des Projekts. Marktdaten ist in tägliche Risiko Analyse Berechnungen, z. B. bestimmte, alle Aufgaben in den Auftrag noch gemeinsam. Diese Marktdaten, oft mehrere GB groß, sollten nur einmal an jeden Knoten berechnen heruntergeladen werden, damit alle Aufgaben, die auf dem Knoten ausgeführt wird, die sie verwenden kann. Verwenden eines **Auftrags Vorbereitung Aufgabe** , diese Daten an jeden Knoten herunterzuladen, bevor die Ausführung eines Auftrags anderen Vorgängen ist.

**Löschen von Projekt- und Aufgabenzeilen Ausgabe**

In einer "Pool freigegeben"-Umgebung, in dem ein Ressourcenpool Datenverarbeitungsknoten nicht zwischen Aufträge außer Betrieb gesetzt sind, müssen Sie die Position von Daten zwischen ausführen zu löschen. Möglicherweise müssen zum Freigeben von Speicherplatz auf den Knoten oder den Richtlinien Ihrer Organisation Sicherheit erfüllen. Verwenden Sie einen **Task freigeben Auftrag** zum Löschen von Daten, die durch einen Auftrag Vorbereitung Vorgang heruntergeladen wurde, oder während der Ausführung der Aufgabe generiert.

**Log Aufbewahrungsrichtlinien**

Möglicherweise möchten eine Kopie der Protokolldateien, die Ihre Aufgaben generieren oder vielleicht abstürzen Sicherungsdateien, die von Applications fehlgeschlagene generiert werden können. Verwenden eines **Task freigeben Position** in diesen Fällen zum Komprimieren und diese Daten in einer [Azure-Speicher] hochladen[ azure_storage] Konto.

>[AZURE.TIP] Eine weitere Möglichkeit zum Beibehalten von Protokollen und anderen Projekt- und Aufgabenzeilen ausgeben, dass Daten besteht darin, die Bibliothek [Azure Stapel Datei Konventionen](batch-task-output.md) verwenden.

## <a name="job-preparation-task"></a>Position Vorbereitung Aufgabe

Vor der Ausführung eines Auftrags Aufgaben führt Stapel den Auftrag zur Vorbereitung Vorgang auf jedem Computeknoten, die zum Ausführen eines Vorgangs geplant wurde. Standardmäßig wartet der Dienst Stapel für den Auftrag zur Vorbereitung Vorgang abgeschlossen sein, bevor Sie die Vorgänge geplant, auf dem Knoten ausgeführt wird. Sie können jedoch den Dienst nicht zu warten konfigurieren. Wenn Sie der Knoten neu gestartet wurde, der Auftrag zur Vorbereitung Vorgang erneut ausgeführt, jedoch können Sie auch dieses Verhalten deaktivieren.

Die Position Vorbereitung Aufgabe wird nur für Knoten ausgeführt, die zum Ausführen eines Vorgangs angesetzt werden. Dadurch wird verhindert, dass die unnötige Ausführung eines Vorgangs Vorbereitung, für den Fall, dass ein Knoten nicht auf eine Aufgabe zugewiesen ist. Dies kann auftreten, wenn die Anzahl der Aufgaben für ein Projekt kleiner als die Anzahl der Knoten in einem Ressourcenpool ist. Dies gilt auch bei [Ausführung der gleichzeitigen Aufgabe](batch-parallel-node-tasks.md) aktiviert ist, ist die Aufgabenanzahl niedriger als Summe möglichen gleichzeitige Vorgänge zu einigen Knoten im Leerlauf verwerfen. Durch den Auftrag zur Vorbereitung Vorgang nicht auf Knoten im Leerlauf ausgeführt wird, können Sie auf Daten durchstellen Gebühren weniger Geld ausgeben.

> [AZURE.NOTE] [JobPreparationTask] [ net_job_prep_cloudjob] unterscheidet sich von der [CloudPool.StartTask] [ pool_starttask] JobPreparationTask am Anfang jeder Aufgabe, ausgeführt wird, wohingegen StartTask nur bei einem Knoten berechnen ausgeführt wird, zum ersten Mal einen Pool verknüpft oder neu gestartet wird.

## <a name="job-release-task"></a>Task freigeben Position

Sobald ein Auftrag als abgeschlossen gekennzeichnet ist, wird der Task freigeben Position auf den einzelnen Knoten im Pool ausgeführt, die mindestens einen Vorgang ausgeführt. Sie markieren als abgeschlossen, indem Sie die Anforderung einer Abschluss ein Auftrags. Stapel Dienst dann verwenden: Legt den Job-Status auf *Beenden*, beendet alle aktiven oder laufenden Aufgaben im Zusammenhang mit den Auftrag und ausgeführt wird die Position Task freigeben. Der Auftrag verschiebt klicken Sie dann auf den Status *abgeschlossen* .

> [AZURE.NOTE] Position Löschvorgang führt auch den Auftrag Task freigeben. Wenn jedoch ein Projekt bereits beendet wurde, wird der Task freigeben kein zweites Mal ausführen der Auftrag später gelöscht wird.

## <a name="job-prep-and-release-tasks-with-batch-net"></a>Vorbereiten der beruflichen Position, und lassen Sie wieder los Aufgaben mit Stapel .NET

Um eine Position Vorbereitung Aufgabe verwenden möchten, weisen Sie einem [JobPreparationTask] [ net_job_prep] Objekt des Projekts [CloudJob.JobPreparationTask] [ net_job_prep_cloudjob] Eigenschaft. Auf ähnliche Weise Initialisierung einer [JobReleaseTask] [ net_job_release] und Zuweisen des Projekts [CloudJob.JobReleaseTask] [ net_job_prep_cloudjob] Eigenschaft Task Freigeben des Projekts festlegen.

In diesem Codeausschnitt `myBatchClient` ist eine Instanz der [BatchClient][net_batch_client], und `myPool` einer vorhandenen Ressourcenpool innerhalb des Kontos Stapel ist.

```csharp
// Create the CloudJob for CloudPool "myPool"
CloudJob myJob =
    myBatchClient.JobOperations.CreateJob(
        "JobPrepReleaseSampleJob",
        new PoolInformation() { PoolId = "myPool" });

// Specify the command lines for the job preparation and release tasks
string jobPrepCmdLine =
    "cmd /c echo %AZ_BATCH_NODE_ID% > %AZ_BATCH_NODE_SHARED_DIR%\\shared_file.txt";
string jobReleaseCmdLine =
    "cmd /c del %AZ_BATCH_NODE_SHARED_DIR%\\shared_file.txt";

// Assign the job preparation task to the job
myJob.JobPreparationTask =
    new JobPreparationTask { CommandLine = jobPrepCmdLine };

// Assign the job release task to the job
myJob.JobReleaseTask =
    new JobPreparationTask { CommandLine = jobReleaseCmdLine };

await myJob.CommitAsync();
```

Wie zuvor schon erwähnt, wird der Task freigeben ausgeführt, wenn ein Projekt beendet oder gelöscht wird. Beenden ein Auftrags mit [JobOperations.TerminateJobAsync][net_job_terminate]. Löschen Sie ein Projekt mit [JobOperations.DeleteJobAsync][net_job_delete]. Klicken Sie in der Regel zu beenden oder Löschen eines Auftrags aus, wenn seine Vorgänge abgeschlossen sind, oder wenn, den von Ihnen definierten Timeout erreicht wurde.

```csharp
// Terminate the job to mark it as Completed; this will initiate the
// Job Release Task on any node that executed job tasks. Note that the
// Job Release Task is also executed when a job is deleted, thus you
// need not call Terminate if you typically delete jobs after task completion.
await myBatchClient.JobOperations.TerminateJobAsy("JobPrepReleaseSampleJob");
```

## <a name="code-sample-on-github"></a>Beispiel für Code auf GitHub

Um finden Sie unter Vorbereitung Position, und lassen Sie Aufgaben in der Aktion, schauen Sie sich die [JobPrepRelease] [ job_prep_release_sample] Projekt auf GitHub Beispiel. Dieser Console-Anwendung geschieht Folgendes:

1. Erstellt einen Pool mit zwei "klein" Knoten.
2. Erstellt ein Projekt mit Auftrag Vorbereitung, Version und standard Aufgaben an.
3. Wird die Position Vorbereitung Aufgabe, die zuerst die Knoten-ID in eine Textdatei in einem Knoten "freigegebenen" Verzeichnis schreibt ausgeführt.
4. Führt eine Aufgabe auf den einzelnen Knoten, die zugehörigen Aufgaben-ID in derselben Textdatei schreibt.
5. Nachdem alle Vorgänge abgeschlossen werden (oder das Timeout erreicht ist), wird der Inhalt des Knotens Textdatei an die Konsole gedruckt.
6. Wenn das Projekt abgeschlossen ist, wird den Task Auftrag freigeben, um die Datei aus dem Knoten löschen ausgeführt.
6. Die Beendigungscodes von der Projektaufgaben Vorbereitung und Freigabe für die einzelnen Knoten Grundlage ihrer Ausführung gedruckt werden.
7. Pausen Ausführung Bestätigung Ihrer Position und/oder Ressourcenpool Löschvorgang dürfen.

Ausgaben der Anwendung Beispiel ist ähnlich wie die folgende:

```
Attempting to create pool: JobPrepReleaseSamplePool
Created pool JobPrepReleaseSamplePool with 2 small nodes
Checking for existing job JobPrepReleaseSampleJob...
Job JobPrepReleaseSampleJob not found, creating...
Submitting tasks and awaiting completion...
All tasks completed.

Contents of shared\job_prep_and_release.txt on tvm-2434664350_1-20160623t173951z:
-------------------------------------------
tvm-2434664350_1-20160623t173951z tasks:
  task001
  task004
  task005
  task006

Contents of shared\job_prep_and_release.txt on tvm-2434664350_2-20160623t173951z:
-------------------------------------------
tvm-2434664350_2-20160623t173951z tasks:
  task008
  task002
  task003
  task007

Waiting for job JobPrepReleaseSampleJob to reach state Completed
...

tvm-2434664350_1-20160623t173951z:
  Prep task exit code:    0
  Release task exit code: 0

tvm-2434664350_2-20160623t173951z:
  Prep task exit code:    0
  Release task exit code: 0

Delete job? [yes] no
yes
Delete pool? [yes] no
yes

Sample complete, hit ENTER to exit...
```

>[AZURE.NOTE] Aufgrund der Variablen erstellen und Start Zeit Knoten in einem neuen Pool (einige Knoten für Aufgaben, bevor Sie andere bereit sind), wird möglicherweise andere Ausgabe angezeigt. Insbesondere, da die Aufgaben schnell abgeschlossen haben, möglicherweise einen Pool den Knoten alle Vorgänge des Projekts ausführen. In diesem Fall werden Sie feststellen, dass der Auftrag vorbereiten und Release Aufgaben gibt es nicht für den Knoten, die keine Aufgaben ausgeführt.

### <a name="inspect-job-preparation-and-release-tasks-in-the-azure-portal"></a>Prüfen von Auftrag zur Vorbereitung und Release Aufgaben im Azure-portal

Wenn Sie die Beispiel-Anwendung ausführen, können die [Azure-Portal] [ portal] zum Anzeigen der Eigenschaften eines Auftrags und seine Aufgaben oder sogar Herunterladen der freigegebenen Textdatei, die von der Projektaufgaben des geändert wird.

Das Bildschirmabbild unten zeigt das **Vorbereitung Aufgaben Blade** Azure-Portal nach der Ausführung der Anwendung Stichprobe. Navigieren Sie auf die Eigenschaften *JobPrepReleaseSampleJob* , nachdem Ihre Vorgänge abgeschlossen haben (aber vor dem Löschen Ihrer Arbeit und Ressourcenpool), und klicken Sie auf **vorbereitende Aufgaben** oder **Release Vorgänge** , um ihre Eigenschaften anzuzeigen.

![Position Vorbereitung Eigenschaften Azure-Portal][1]

## <a name="next-steps"></a>Nächste Schritte

### <a name="application-packages"></a>Anwendungspaketen

Zusätzlich zu den Auftrag zur Vorbereitung Vorgang können Sie auch das Feature [Anwendungspakete](batch-application-packages.md) des Blatts zur Ausführung der Aufgabe Datenverarbeitungsknoten Vorbereitung verwenden. Dieses Feature ist besonders hilfreich für die Bereitstellung von Applications, die keine erfordern ein Installationsprogramm, Anwendungen, die viele (100 +) Dateien enthalten oder Anwendungen, die nur Versionskontrolle erfordern ausgeführt.

### <a name="installing-applications-and-staging-data"></a>Installieren von Applications und das staging von Daten

Diese MSDN-Forumsbeitrag bietet einen Überblick über verschiedene Methoden zur Vorbereitung Ihrer Knoten für die Ausführung von Aufgaben:

[Installieren von Applications und das staging von Daten auf dem Blatt berechnen Knoten][forum_post]

Mit einem der Teammitglieder Azure Stapel geschrieben, werden mehrere Techniken, mit denen Sie Programme und Daten zum Berechnen von Knoten bereitstellen, erläutert.

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_listjobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[azure_storage]: https://azure.microsoft.com/services/storage/
[portal]: https://portal.azure.com
[job_prep_release_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/JobPrepRelease
[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[net_batch_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]:https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_job_prep]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.aspx
[net_job_prep_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobpreparationtask.aspx
[net_job_prep_resourcefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.resourcefiles.aspx
[net_job_delete]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.deletejobasync.aspx
[net_job_terminate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.terminatejobasync.aspx
[net_job_release]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobreleasetask.aspx
[net_job_release_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobreleasetask.aspx
[pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx

[net_list_certs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificateoperations.listcertificates.aspx
[net_list_compute_nodes]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listcomputenodes.aspx
[net_list_job_schedules]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobschedules.aspx
[net_list_jobprep_status]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobpreparationandreleasetaskstatus.aspx
[net_list_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[net_list_nodefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listnodefiles.aspx
[net_list_pools]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listpools.aspx
[net_list_schedule_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobs.aspx
[net_list_task_files]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listnodefiles.aspx
[net_list_tasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listtasks.aspx

[1]: ./media/batch-job-prep-release/portal-jobprep-01.png

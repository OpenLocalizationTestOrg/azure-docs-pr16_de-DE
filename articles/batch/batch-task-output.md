<properties
    pageTitle="Projekt- und Aufgabenzeilen ausgeben in Azure Blattnamen dauerhaften | Microsoft Azure"
    description="Informationen Sie zum Verwenden von Azure-Speicher in ein permanenten Speicher für Ihre Stapel Vorgang und den Auftrag ausgeben, und Aktivieren dieses dauerhaften Ausgabe Azure-Portal anzeigen."
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
    ms.date="09/07/2016"
    ms.author="marsma" />

# <a name="persist-azure-batch-job-and-task-output"></a>Azure Stapel Projekt- und Aufgabenzeilen Ausgabe beibehalten

Die Aufgaben, die Sie in der Regel in Stapel ausführen erstellt Ausgabe, die gespeichert und dann später abgerufen von anderen Aufgaben im Auftrag, der Clientanwendung, die den Auftrag oder beides ausgeführt werden muss. Diese Ausgabe möglicherweise Dateien Eingabedaten Verarbeitung erstellte oder Protokolldateien Ausführung der Aufgabe zugeordnet. In diesem Artikel wird eine .NET Class-Bibliothek, die eine Methode Konventionen basierende mithilfe des beibehält solche Vorgang Ausgabe an den Azure BLOB-Speicher verfügbar zu machen auch nach dem Löschen der Pools, Aufträge, und Knoten zu berechnen.

Mithilfe der Methode in diesem Artikel können Sie auch die Ausgabe der Aufgabe anzeigen, **gespeicherte Ausgabedateien** und **Gespeicherte Protokolle** im [Portal Azure][portal].

![Gespeicherte Ausgabedateien und gespeicherte Protokolle Selektoren-Portal][1]

>[AZURE.NOTE] Die [Azure Stapel Datei Konventionen] [ nuget_package] .NET Class-Bibliothek, die in diesem Artikel beschriebenen gibt es zurzeit in der Vorschau. Einige der hier beschriebenen Features können vor der allgemeinen Verfügbarkeit geändert werden.

## <a name="task-output-considerations"></a>Aufgabe Ausgabe Aspekte

Wenn Sie Ihre Lösung Stapel entwerfen, müssen Sie verschiedene Faktoren, die im Zusammenhang mit Ausgaben für Projekt- und Aufgabenzeilen berücksichtigen.

* **Berechnen von Knoten Lebensdauer**: Knoten sind besonders in Pools automatisch skalieren aktiviert häufig vorübergehend, zu berechnen. Die Ausgaben für die Aufgaben, die auf einem Knoten ausgeführt stehen nur während der Knoten vorhanden ist, und nur innerhalb der Datei Aufbewahrungsrichtlinien-Zeit, die Sie für den Vorgang festgelegt haben. Um sicherzustellen, dass die Ausgabe der Aufgabe gewahrt wird, müssen Ihre Vorgänge daher deren Ausgabedateien in einen permanenten Speicher, z. B. Azure-Speicher hochladen.

* **Die Ausgabe Speicher**: behalten Sie Vorgang Ausgabedaten zu dauerhaften Speicher können die [Azure Speicher SDK](../storage/storage-dotnet-how-to-use-blobs.md) im Aufgabenbereich Code die Ausgabe der Aufgabe zu einem BLOB-Speichercontainer hochladen. Wenn Sie einen Container und Konventionen für Dateinamen implementieren, können der Clientanwendung oder andere Aufgaben in den Auftrag dann suchen und diese Ausgabe basierend auf der Messe herunterladen.

* **Abrufen der Ausgabe**: Sie können abrufen Vorgang Ausgabe direkt aus den Datenverarbeitungsknoten in Ihrem Ressourcenpool oder aus Azure-Speicher, wenn Ihre Vorgänge deren Ausgabe beibehalten werden. Zum Abrufen eines Vorgangs Ausgabe direkt von einem Knoten berechnen benötigen Sie einen Namen und die Ausgabe Position auf dem Knoten. Wenn Sie die Ausgabe an den untergeordneten Aufgaben Azure Speicher beibehalten oder die Clientanwendung den vollständigen Pfad zu der Datei in Azure-Speicher sie mithilfe der Azure-Speicher SDK herunterladen müssen.

* **Die Ausgabe anzeigen**: Wenn Sie zu einem Vorgang Stapel Azure-Portal navigieren und Auswählen von **Dateien auf Knoten**, Ihnen werden alle Dateien, die mit dem Vorgang verbundenen angezeigt, nicht nur die Ausgabedateien Sie interessiert. In diesem Fall stehen Dateien auf Datenverarbeitungsknoten nur während der Knoten vorhanden ist, und nur innerhalb der Datei Aufbewahrungszeit, die Sie für den Vorgang festgelegt haben. Aufgabe Ausgabe anzeigen möchten, die Sie zum Azure-Speicher im Portal oder eine Anwendung wie der [Azure-Speicher-Explorer]beibehalten haben[storage_explorer], müssen Sie wissen, die Position und direkt zu der Datei zu navigieren.

## <a name="help-for-persisted-output"></a>Hilfe für dauerhaften Ausgabe

Sie können mehrere einfach Projekt- und Aufgabenzeilen ausgeben beibehalten werden, hat das Team Stapel definiert und implementiert eine Reihe von Benennungskonventionen ebenso wie eine .NET Class-Bibliothek, die [Azure Stapel Datei Konventionen] [ nuget_package] Bibliothek, die Sie in Ihrem Stapel Clientanwendungen verwenden können. Darüber hinaus ist das Azure-Portal diese Benennungskonventionen bekannt, so, dass Sie die Dateien problemlos finden können, die Sie mithilfe der Bibliothek gespeichert haben.

## <a name="using-the-file-conventions-library"></a>Verwenden die Datei Konventionen-Bibliothek

[Azure Stapel Datei Konventionen] [ nuget_package] ist eine .NET Class-Bibliothek, die Ihren Stapel auf einfache Weise speichern und Vorgang Ausgaben aus und in Azure-Speicher abrufen verwenden können. Es ist vorgesehen zur Verwendung in sowohl die Aufgabe als auch Code – Vorgang Code für das Speichern von Dateien, und klicken Sie im Clientcode aufzulisten und dann abrufen. Aufgaben können auch die Bibliothek für die Ausgaben der übergeordneten Aufgaben, wie in einem Szenario [anordnungsbeziehungen](batch-task-dependencies.md) abrufen.

Die Bibliothek Konventionen erledigt um sicherzustellen, dass Speicher Containern und Vorgang ausgeben Dateien werden gemäß der Messe benannt und an den richtigen Ort beim beibehalten in Azure-Speicher hochgeladen werden. Beim Abrufen von Ausgaben können Sie einfach die Ausgaben für einen angegebenen Position oder Aufgabe suchen, indem Sie auflisten oder die Ausgaben nach-ID und Zweck, anstatt Sie wissen, wo im Speicher vorhanden ist oder Dateinamen abrufen.

Beispielsweise können Sie die Bibliothek "Liste aller temporäre Dateien für Vorgang 7" oder "Hol mir die Miniaturbildansicht für Position *Mymovie*," verwenden, ohne den Dateinamen oder Speicherort in Ihr Konto Speicher kennen.

### <a name="get-the-library"></a>Abrufen der Bibliothek

Sie können die Bibliothek, die neue Klassen enthält und erweitert die [CloudJob] abrufen[ net_cloudjob] und [CloudTask] [ net_cloudtask] Klassen mit neuen Methoden, von [NuGet][nuget_package]. Sie können Ihre Visual Studio-Projekt mit dem [NuGet Bibliothek Package Manager]hinzufügen[nuget_manager].

>[AZURE.TIP] Sie können den [Quellcode] finden[ github_file_conventions] für die Bibliothek Azure Stapel Datei Konventionen auf GitHub im Microsoft Azure SDK für .NET Repository.

## <a name="requirement-linked-storage-account"></a>Anforderung: verknüpfte Speicher-Konto

Um Ausgaben in dauerhaften Speicher mithilfe der Datei Konventionen-Bibliothek speichern, und klicken Sie in der Azure-Portal anzeigen können, müssen Sie den [Link ein Konto Azure-Speicher](batch-application-packages.md#link-a-storage-account) bei Ihrem Konto Stapel. Wenn Sie noch nicht geschehen ist, verknüpfen Sie ein Speicherkonto bei Ihrem Konto Stapel mithilfe des Azure-Portals:

Blade **Stapel Konto** > **Einstellungen** > **Speicher-Konto** > **Speicher-Konto** (keine) > Wählen Sie ein Speicherkonto in Ihrem Abonnement

Eine ausführlichere – Exemplarische Vorgehensweise auf ein Speicherkonto Verknüpfen finden Sie unter [Anwendung Bereitstellung mit Azure Stapel Anwendungspakete](batch-application-packages.md).

## <a name="persist-output"></a>Beibehalten der Ausgabe

Es gibt zwei primäre Aktionen ausführen, wenn Projekt- und Aufgabenzeilen Ausgabe mit der Datei Konventionen Bibliothek speichern: den Speichercontainer erstellen und Speichern der Ausgabe in den Container.

>[AZURE.WARNING] Da alle Ausgaben für Projekt- und Aufgabenzeilen im selben Container gespeichert sind, möglicherweise [Speicher begrenzungsebene Grenzwerte](../storage/storage-performance-checklist.md#blobs) erzwungen, wenn eine große Anzahl von Aufgaben versuchen, um Dateien zur gleichen Zeit beizubehalten.

### <a name="create-storage-container"></a>Erstellen von Speichercontainer

Zuerst Ihre Vorgänge beibehalten von Ausgabe an Speicherplatz daher müssen Sie einen BLOB-Speichercontainer erstellen, in dem sie ihre Ausgabe hochgeladen werden. Rufen [CloudJob]dazu[net_cloudjob]. [PrepareOutputStorageAsync] [net_prepareoutputasync]. Diese Erweiterungsmethode arbeitet mit einem [CloudStorageAccount] [ net_cloudstorageaccount] als Parameter-Objekt, und erstellt einen Container mit dem Namen so, dass deren Inhalt erkannt werden durch das Azure-Portal und später in diesem Artikel beschriebenen Abrufmethoden zum.

In der Regel platzieren Sie diesen Code in der Clientanwendung – die Anwendung, Ihre Pools, Projekten und Aufgaben erstellt.

```csharp
CloudJob job = batchClient.JobOperations.CreateJob(
    "myJob",
    new PoolInformation { PoolId = "myPool" });

// Create reference to the linked Azure Storage account
CloudStorageAccount linkedStorageAccount =
    new CloudStorageAccount(myCredentials, true);

// Create the blob storage container for the outputs
await job.PrepareOutputStorageAsync(linkedStorageAccount);
```

### <a name="store-task-outputs"></a>Speichern Sie die Aufgabenausgaben

Jetzt, da Sie einen Container im Blob-Speicher vorbereitet haben, Aufgaben können speichern Ausgabe an den Container mithilfe der [TaskOutputStorage] [ net_taskoutputstorage] Klasse in der Datei Konventionen Bibliothek gefunden.

Im Vorgang Code, erstellen Sie zuerst eine [TaskOutputStorage] [ net_taskoutputstorage] Objekt, und klicken Sie dann, wenn der Vorgang seine Arbeit abgeschlossen hat, rufen Sie die [TaskOutputStorage][net_taskoutputstorage]. [SaveAsync] [net_saveasync] Methode, um deren Ausgabe Azure-Speicher zu speichern.

```csharp
CloudStorageAccount linkedStorageAccount = new CloudStorageAccount(myCredentials);
string jobId = Environment.GetEnvironmentVariable("AZ_BATCH_JOB_ID");
string taskId = Environment.GetEnvironmentVariable("AZ_BATCH_TASK_ID");

TaskOutputStorage taskOutputStorage = new TaskOutputStorage(
    linkedStorageAccount, jobId, taskId);

/* Code to process data and produce output file(s) */

await taskOutputStorage.SaveAsync(TaskOutputKind.TaskOutput, "frame_full_res.jpg");
await taskOutputStorage.SaveAsync(TaskOutputKind.TaskPreview, "frame_low_res.jpg");
```

Der Parameter "ausgeben Art" kategorisiert die dauerhaften Dateien. Es gibt vier vordefinierte [TaskOutputKind] [ net_taskoutputkind] Typen: "TaskOutput", "TaskPreview", "TaskLog" und "TaskIntermediate". Sie können auch benutzerdefinierte Arten definieren, wenn sie in den Workflow nützlich wäre.

Diese Ausgabe Dateitypen können Sie angeben, welche Art von Ausgaben in der Liste die dauerhaften Ausgaben für einen bestimmten Vorgang später Stapel abzufragen. Wenn Sie die Ausgaben für einen Vorgang auflisten, können Sie also die Liste auf einen der Ausgabe Tabstopptypen filtern. Beispielsweise "brauche die Ausgabe der *Vorschau* für den Vorgang *109*." Weitere werden auf auflisten und Abrufen von Ausgaben in [Abrufen Ausgabe](#retrieve-output) später in diesem Artikel.

>[AZURE.TIP] Auch die Art der Ausgabe angibt, wo der Azure-Portal eine bestimmte Datei angezeigt wird: *TaskOutput*-kategorisierte Dateien in "Vorgang Ausgabedateien" angezeigt wird und *TaskLog* Dateien werden in "Task-Protokolle".

### <a name="store-job-outputs"></a>Speichern Sie die Ausgaben für Position

Neben dem Speichern von Vorgang Ausgaben, können Sie die Ausgaben einer gesamten Auftrag zugeordneten speichern. In den Vorgang Zusammenführen eines Auftrags Rendern Film konnte Sie beispielsweise den vollständig gerenderten Film als eine Position Ausgabe beibehalten. Wenn Ihre Arbeit abgeschlossen ist, die Clientanwendung kann einfach Liste und die Ausgaben für das Projekt, abrufen und muss nicht die einzelnen Vorgänge Abfragen.

Speichern Auftragsausgabe, indem Sie die [JobOutputStorage][net_joboutputstorage]. [SaveAsync] [net_joboutputstorage_saveasync] Methode, und geben Sie die [JobOutputKind] [ net_joboutputkind] und Dateiname:

```
CloudJob job = await batchClient.JobOperations.GetJobAsync(jobId);
JobOutputStorage jobOutputStorage = job.OutputStorage(linkedStorageAccount);

await jobOutputStorage.SaveAsync(JobOutputKind.JobOutput, "mymovie.mp4");
await jobOutputStorage.SaveAsync(JobOutputKind.JobPreview, "mymovie_preview.mp4");
```

Wie mit TaskOutputKind für Aufgabenausgaben, die [JobOutputKind] [ net_joboutputkind] Parameter ein Auftrags Kategorisieren der Dateien beibehalten. Für diesen Parameter können Sie eine höhere Abfrage für einen bestimmten Typ von Ausgabe-(Liste). Die JobOutputKind sowohl Ausgabe und Vorschau Typen enthält, und unterstützt das Erstellen von benutzerdefinierten Datentypen.

### <a name="store-task-logs"></a>Task-Protokolle speichern

Neben dem Speichern einer Datei zum dauerhaften Speicher an, wenn ein Vorgang oder eine Aufgabe abgeschlossen ist, Sie unter Umständen müssen Dateien beibehalten, die während der Ausführung eines Vorgangs – Protokolldateien aktualisiert werden oder `stdout.txt` und `stderr.txt`, beispielsweise. Zu diesem Zweck die Bibliothek Azure Stapel Datei Konventionen bietet die [TaskOutputStorage][net_taskoutputstorage]. [SaveTrackedAsync] [net_savetrackedasync] Methode. Mit [SaveTrackedAsync][net_savetrackedasync], können diese Aktualisierungen Azure-Speicher beibehalten und Nachverfolgen von Aktualisierungen in eine Datei auf dem Knoten (in Zeitabständen, die Sie angeben).

Im folgenden Codeausschnitt verwenden wir [SaveTrackedAsync] [ net_savetrackedasync] aktualisieren `stdout.txt` in Azure-Speicher alle 15 Sekunden während der Ausführung des Vorgangs:

```csharp
TimeSpan stdoutFlushDelay = TimeSpan.FromSeconds(3);
string logFilePath = Path.Combine(
    Environment.GetEnvironmentVariable("AZ_BATCH_TASK_DIR"), "stdout.txt");

// The primary task logic is wrapped in a using statement that sends updates to
// the stdout.txt blob in Storage every 15 seconds while the task code runs.
using (ITrackedSaveOperation stdout =
        await taskStorage.SaveTrackedAsync(
        TaskOutputKind.TaskLog,
        logFilePath,
        "stdout.txt",
        TimeSpan.FromSeconds(15)))
{
    /* Code to process data and produce output file(s) */

    // We are tracking the disk file to save our standard output, but the
    // node agent may take up to 3 seconds to flush the stdout stream to
    // disk. So give the file a moment to catch up.
    await Task.Delay(stdoutFlushDelay);
}
```

`Code to process data and produce output file(s)`ist einfach ein Platzhalter für den Code, den die Aufgabe normalerweise durchführen möchten. Beispielsweise müssen Sie Code, der mithilfe von Daten aus Azure-Speicher heruntergeladen und führt eine Transformation oder eine Berechnung daran. Der wichtige Teil dieser Ausschnitt ist veranschaulichen, wie diese Art von Code in umbrochen werden kann ein `using` blockieren regelmäßig eine Datei mit [SaveTrackedAsync]aktualisieren[net_savetrackedasync].

Die `Task.Delay` ist erforderlich, am Ende dieses `using` blockieren, um sicherzustellen, dass der Knoten-Agent Zeit ist für den Inhalt der standard sofort zu der Datei stdout.txt auf dem Knoten leeren (der Knoten-Agent ist ein Programm, das auf den einzelnen Knoten im Pool ausgeführt wird und stellt die Benutzeroberfläche Befehl and Control zwischen dem Knoten und den Stapel Dienst). Ohne diese Verzögerung ist es möglich, die letzten Sekunden der Ausgabe verpasst haben. Diese Verzögerung möglicherweise nicht für alle Dateien erforderlich.

>[AZURE.NOTE] Wenn Sie die Datei mit SaveTrackedAsync Überwachung aktivieren, werden nur an die nachverfolgten Datei *angefügt* in Azure-Speicher beibehalten. Verwenden Sie diese Methode nur zum Nachverfolgen von Protokolldateien nicht drehen oder andere Dateien, die,, d. h. angefügt sind, die Daten nur an das Ende der Datei hinzugefügt wird, wenn diese aktualisiert wird.

## <a name="retrieve-output"></a>Abrufen der Ausgabe

Beim Abrufen Ihrer dauerhaften Ausgabe mithilfe der Bibliothek Azure Stapel Datei Konventionen verwenden Sie auf Vorgang und Position orientierte Weise. Sie können die Ausgabe anfordern, für die angegebenen Vorgang oder eine Position ohne Pfadangabe BLOB-Speicher oder sogar des Dateinamens wissen zu müssen. Sie können einfach sagen Sie "mich geben die Ausgabedateien für den Vorgang *109*."

Der folgende Codeausschnitt durchläuft alle Vorgänge des Projekts, werden einige Informationen über die Ausgabedateien für den Vorgang ausgegeben und dann downloads enthaltenen Dateien aus dem Speicher.

```csharp
foreach (CloudTask task in myJob.ListTasks())
{
    foreach (TaskOutputStorage output in
        task.OutputStorage(storageAccount).ListOutputs(
            TaskOutputKind.TaskOutput))
    {
        Console.WriteLine($"output file: {output.FilePath}");

        output.DownloadToFileAsync(
            $"{jobId}-{output.FilePath}",
            System.IO.FileMode.Create).Wait();
    }
}
```

## <a name="task-outputs-and-the-azure-portal"></a>Aufgabenausgaben und Azure-portal

Zeigt das Azure-Portal eines verknüpften Azure-Speicher-Kontos, die mit den Benennungskonventionen in der [Azure Stapel Datei Konventionen Infos]gefunden Vorgang Ausgaben und Protokolle, die beibehalten werden[github_file_conventions_readme]. Sie können diese Konventionen selbst implementieren in einer anderen Sprache Ihrer Wahl oder können Sie die Datei Konventionen Bibliothek in Ihren.

### <a name="enable-portal-display"></a>Aktivieren des Portals anzeigen

Um die Anzeige von Ihrer Ausgaben im Portal aktivieren möchten, müssen Sie die folgenden Anforderungen erfüllen:

 1. [Verknüpfen eines Kontos Azure-Speicher](#requirement-linked-storage-account) bei Ihrem Konto Stapel.
 2. Befolgen Sie die vordefinierten Benennungskonventionen für Speichercontainer und Dateien, wenn Sie Ausgaben beibehalten. Die Definition der diese Konventionen finden Sie in der Datei Konventionen Bibliothek [Infos zu][github_file_conventions_readme]. Wenn Sie die [Azure Stapel Datei Konventionen] verwenden[ nuget_package] Bibliothek zum Beibehalten der Ausgabe, diese Anforderung erfüllt ist.

### <a name="view-outputs-in-the-portal"></a>Anzeigen von Ausgaben im Portal

Navigieren Sie zum Anzeigen der Aufgabenausgaben und Protokolle Azure-Portal an die Aufgabe, deren Ausgabe Sie interessiert sind, und klicken Sie auf **gespeicherte Ausgabedateien** oder **Protokolle gespeichert**. Diese Abbildung zeigt die **gespeicherte Ausgabedateien** für den Vorgang mit der ID "007":

![Aufgabe Ausgaben vorher in der Azure-portal][2]

## <a name="code-sample"></a>Beispiel für Code

Die [PersistOutputs] [ github_persistoutputs] Beispielprojekt ist eine der [Azure Stapel Codebeispielen] [ github_samples] auf GitHub. Die Lösung in Visual Studio 2015 veranschaulicht, wie die Bibliothek Azure Stapel Datei Konventionen Vorgang Ausgabe an dauerhaften Speicher beibehalten werden. Wenn Sie das Beispiel ausführen, gehen Sie folgendermaßen vor:

1. Öffnen Sie das Projekt in **Visual Studio 2015**.
2. Fügen Sie Ihre Stapel und Speicher **Anmeldeinformationen für das Konto** zu **AccountSettings.settings** im Projekt Microsoft.Azure.Batch.Samples.Common.
3. **Erstellen** (aber nicht ausgeführt wird) die Lösung. Wiederherstellen Sie alle NuGet-Pakete, wenn Sie dazu aufgefordert werden.
4. Verwenden des Azure-Portals ein [Anwendungspaket](batch-application-packages.md) für **PersistOutputsTask**hochladen. Einschließen der `PersistOutputsTask.exe` und deren abhängigen Assemblys im ZIP-Paket, legen Sie die Anwendung-ID "PersistOutputsTask", und die Version der Anwendung Paket "1.0".
5. **Starten** (ausführen) die **PersistOutputs** Projekt.

## <a name="next-steps"></a>Nächste Schritte

### <a name="application-deployment"></a>Bereitstellung der Anwendung

Das Feature [Anwendungspakete](batch-application-packages.md) des Blatts bietet eine einfache Möglichkeit zum Bereitstellen und Version der Anwendung, die auf Ihre Aufgaben ausführen Knoten zu berechnen.

### <a name="installing-applications-and-staging-data"></a>Installieren von Applications und das staging von Daten

Schauen Sie sich das [Installieren von Applications und staging Daten auf Blatt berechnen Knoten] [ forum_post] Posten im Stapel Azure-Forum für einen Überblick über die verschiedenen Methoden zur Vorbereitung Ihrer Knoten für die Ausführung von Aufgaben. Mit einem der Teammitglieder Azure Stapel geschrieben, ist diesen Beitrag gute Einführung in den verschiedenen Methoden zum Abrufen von Dateien (einschließlich sowohl Applikationen und Vorgang Eingabedaten) auf Ihre Datenverarbeitungsknoten und einige Besonderheiten für jede Methode.

[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[github_file_conventions]: https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/Batch/FileConventions
[github_file_conventions_readme]: https://github.com/Azure/azure-sdk-for-net/blob/AutoRest/src/Batch/FileConventions/README.md
[github_persistoutputs]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/PersistOutputs
[github_samples]: https://github.com/Azure/azure-batch-samples
[net_batchclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_cloudstorageaccount]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.cloudstorageaccount.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_fileconventions_readme]: https://github.com/Azure/azure-sdk-for-net/blob/AutoRest/src/Batch/FileConventions/README.md
[net_joboutputkind]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputkind.aspx
[net_joboutputstorage]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputstorage.aspx
[net_joboutputstorage_saveasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputstorage.saveasync.aspx
[net_msdn]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_prepareoutputasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.cloudjobextensions.prepareoutputstorageasync.aspx
[net_saveasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.saveasync.aspx
[net_savetrackedasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.savetrackedasync.aspx
[net_taskoutputkind]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputkind.aspx
[net_taskoutputstorage]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.aspx
[nuget_manager]: https://docs.nuget.org/consume/installing-nuget
[nuget_package]: https://www.nuget.org/packages/Microsoft.Azure.Batch.Conventions.Files
[portal]: https://portal.azure.com
[storage_explorer]: http://storageexplorer.com/

[1]: ./media/batch-task-output/task-output-01.png "Gespeicherte Ausgabedateien und gespeicherte Protokolle Selektoren-Portal"
[2]: ./media/batch-task-output/task-output-02.png "Aufgabe Ausgaben vorher in der Azure-portal"
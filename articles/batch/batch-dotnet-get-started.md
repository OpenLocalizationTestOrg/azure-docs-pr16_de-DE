<properties
    pageTitle="Lernprogramm - Erste Schritte mit der Bibliothek Azure Stapel .NET | Microsoft Azure"
    description="Hier erfahren Sie die grundlegenden Konzepte von Azure Stapel und wie Sie für den Dienst Stapel mit einem Beispielszenario entwickeln."
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-compute"
    ms.date="08/15/2016"
    ms.author="marsma"/>

# <a name="get-started-with-the-azure-batch-library-for-net"></a>Erste Schritte mit der Bibliothek Azure Stapel für .NET

> [AZURE.SELECTOR]
- [.NET](batch-dotnet-get-started.md)
- [Python](batch-python-tutorial.md)

Erlernen Sie die Grundlagen des [Blatts Azure] [ azure_batch] und den [Stapel .NET] [ net_api] Bibliothek in diesem Artikel wie eine C#-Beispiel Anwendung Schritt für Schritt erläutert. Wir werden wie diese Anwendung den Stapel Dienst zum Verarbeiten von einer parallele Arbeitsbelastung in der Cloud, ebenfalls nutzt wie Interaktion mit [Azure-Speicher](../storage/storage-introduction.md) für Datei Staging und abrufen. Lernen Sie allgemeine Stapel Anwendung Workflow Techniken ein. Sie auch einen Überblick über die wichtigsten Komponenten des Blatts, z. B. Projekte, Vorgänge, Pools, Basis erlangen und Knoten zu berechnen.

![Stapel Lösung Workflow (Basic)][11]<br/>

## <a name="prerequisites"></a>Erforderliche Komponenten

In diesem Artikel wird vorausgesetzt, dass Sie über Kenntnisse von c# und Visual Studio verfügen. Außerdem wird davon ausgegangen, dass Sie die Konto Erstellung Anforderungen erfüllen, die für Azure und den Stapel und Speicher Services sind unter angegeben werden können.

### <a name="accounts"></a>Konten

- **Azure-Konto**: Wenn Sie bereits über ein Azure-Abonnement, [Erstellen Sie ein kostenloses Azure-Konto]besitzen[azure_free_account].
- **Stapel Konto**: Nachdem Sie ein Azure-Abonnement, [Erstellen Sie ein Stapel Azure-Konto](batch-account-create-portal.md)haben.
- **Speicher-Konto**: finden Sie unter [Erstellen eines Kontos Speicher](../storage/storage-create-storage-account.md#create-a-storage-account) in [zur Azure-Speicher-Konten](../storage/storage-create-storage-account.md).

> [AZURE.IMPORTANT] Stapel aktuell unterstützt *nur* **Allgemeine** Speicher Kontotyp in Schritt 5 # beschriebenen [Speicherkonto erstellen](../storage/storage-create-storage-account.md#create-a-storage-account) in [zur Azure-Speicher-Konten](../storage/storage-create-storage-account.md).

### <a name="visual-studio"></a>Visual Studio

Sie müssen **Visual Studio 2015** zum Erstellen des Projekts Stichprobe. Freie / testen Versionen von Visual Studio finden Sie in der [Übersicht über Visual Studio 2015 Produkte][visual_studio].

### <a name="dotnettutorial-code-sample"></a>Beispiel für *DotNetTutorial* code

Die [DotNetTutorial] [ github_dotnettutorial] Beispiel ist eine viele Codebeispielen [Azure-Stapel-Beispiele] gefunden[ github_samples] Repository auf GitHub. Sie können die Stichprobe herunterladen, mit der Schaltfläche **ZIP-herunterladen** auf der Startseite Repository oder indem Sie auf der [Azure-Stapel-Beispiele-master.zip] [ github_samples_zip] Link direktes Herunterladen. Nachdem Sie den Inhalt der ZIP-Datei extrahiert haben, finden Sie die Lösung in den folgenden Ordner:

`\azure-batch-samples\CSharp\ArticleProjects\DotNetTutorial`

### <a name="azure-batch-explorer-optional"></a>Azure Stapel Explorer (optional)

Der [Azure Stapel Explorer] [ github_batchexplorer] ist ein kostenlos Programm, das in den [Azure-Stapel-Beispiele] enthalten ist[ github_samples] Repository auf GitHub. Obwohl nicht zum Bearbeiten dieses Lernprogramms erforderlich, kann es hilfreich sein, beim Entwickeln und Debuggen von Ihrem Stapel Lösungen.

## <a name="dotnettutorial-sample-project-overview"></a>DotNetTutorial Stichprobe Projektübersicht

Im Beispiel *DotNetTutorial* ist eine 2015 für Visual Studio-Lösung, die besteht aus zwei Projekte: **DotNetTutorial** und **TaskApplication**.

- **DotNetTutorial** ist die Clientanwendung, die mit den Diensten Stapel und Speicher auf eine parallele Arbeitsbelastung auszuführende interagiert Knoten (virtuelle Maschinen) zu berechnen. DotNetTutorial, die auf Ihrem lokalen Computer ausgeführt wird.

- **TaskApplication** ist das Programm, das auf Datenverarbeitungsknoten in Azure zum Ausführen der ist-Arbeit ausgeführt wird. In der Stichprobe `TaskApplication.exe` analysiert der Text in einer Datei aus Azure-Speicher (die Eingabe-Datei heruntergeladen). Klicken Sie dann werden eine Textdatei (die Ausgabedatei), die eine Liste der obersten drei Wörter enthält, die die Eingabe Datei angezeigt. Nachdem sie die Ausgabedatei erstellt hat, uploads TaskApplication die Datei zu Azure-Speicher. Dadurch-Clientanwendung zum Download zur Verfügung. TaskApplication führt parallel auf mehreren berechnen-Knoten in der Stapel-Dienst.

Das folgende Diagramm veranschaulicht die primäre Vorgänge, die von der Clientanwendung, *DotNetTutorial*und die Anwendung, die ausgeführt wird, indem Sie die Aufgaben *TaskApplication*ausgeführt werden. Dieser grundlegende Workflow ist normalerweise der viele berechnen Lösungen, die mit der Stapelverarbeitung erstellt wurden. Während sie jeder Stapel Dienst verfügbar Feature nicht zu veranschaulichen, umfasst nahezu jedem Stapel Szenario ähnliche Prozesse an.

![Stapel Beispielworkflows][8]<br/>

[**Schritt 1.**](#step-1-create-storage-containers) Erstellen Sie **Container** in Azure BLOB-Speicher.<br/>
[**Schritt 2.**](#step-2-upload-task-application-and-data-files) Hochladen von Vorgang Anwendungsdateien und von Dateien zum Container.<br/>
[**Schritt 3.**](#step-3-create-batch-pool) Erstellen Sie einen Stapel **Ressourcenpool**an.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**3a.** Pool **StartTask** downloads die Aufgabe binäre Dateien (TaskApplication) Knoten an, wie er im Ressourcenpool teilnehmen an.<br/>
[**Schritt 4.**](#step-4-create-batch-job) Erstellen Sie eine **Position**ein.<br/>
[**Schritt 5 fort.**](#step-5-add-tasks-to-job) Hinzufügen von **Vorgängen** im Projekt.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**5a.** Die Vorgänge werden planmäßig auf Knoten ausführen.<br/>
    &nbsp;&nbsp;&nbsp;&nbsp;**5 b.** Jede Aufgabe downloads seiner Eingabedaten aus Azure-Speicher, und klicken Sie dann mit der Ausführung beginnt.<br/>
[**Schritt 6 fort.**](#step-6-monitor-tasks) Überwachen von Aufgaben.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**6a.** Wenn Vorgänge abgeschlossen sind, Hochladen diese ihre Ausgabedaten auf Azure-Speicher.<br/>
[**Schritt 7.**](#step-7-download-task-output) Laden Sie Vorgang Ausgabe von Speicher.

Wie erwähnt, nicht jeder Stapel-Lösung genauen Schritte, und viele weitere möglicherweise enthalten, aber die Anwendung *DotNetTutorial* Beispiel veranschaulicht allgemeine Prozesse in einem Stapel Lösung gefunden.

## <a name="build-the-dotnettutorial-sample-project"></a>Erstellen Sie das Beispiel-Projekt *DotNetTutorial*

Bevor Sie das Beispiel erfolgreich ausführen können, müssen Sie sowohl Stapel und Speicher Anmeldeinformationen in der *DotNetTutorial* des Projekts angeben `Program.cs` Datei. Wenn Sie dies noch nicht getan haben, öffnen Sie die Lösung in Visual Studio durch Doppelklicken auf die `DotNetTutorial.sln` Lösungsdatei. Oder öffnen Sie sie in Visual Studio mithilfe der **Datei > Öffnen > Project/Lösung** Menü.

Open `Program.cs` innerhalb des Projekts *DotNetTutorial* . Fügen Sie anschließend Ihre Anmeldeinformationen gemäß Angabe im oberen Bereich der Datei ein:

```
// Update the Batch and Storage account credential strings below with the values
// unique to your accounts. These are used when constructing connection strings
// for the Batch and Storage client objects.

// Batch account credentials
private const string BatchAccountName = "";
private const string BatchAccountKey  = "";
private const string BatchAccountUrl  = "";

// Storage account credentials
private const string StorageAccountName = "";
private const string StorageAccountKey  = "";
```

> [AZURE.IMPORTANT] Wie zuvor erwähnt, müssen Sie die Anmeldeinformationen für eine **Allgemeine** Speicherkonto derzeit in Azure-Speicher angeben. Die Stapel Applikationen verwenden BLOB-Speicher innerhalb der **Allgemeine** Speicher-Konto an. Geben Sie die Anmeldeinformationen für ein Speicherkonto, das erstellt wurde, indem Sie den *Blob-Speicher* Kontotyp auswählen.

Ihre Anmeldeinformationen Stapel und Speicher in jedem Dienst Falz Konto finden Sie im [Portal Azure][azure_portal]:

![Stapelverarbeitung Anmeldeinformationen im Portal][9]
![Speicher Anmeldeinformationen im Portal][10]<br/>

Jetzt, da Sie das Projekt mit Ihren Anmeldeinformationen aktualisiert haben, mit der rechten Maustaste in der Lösung im Explorer-Lösung, und klicken Sie auf die **Lösung erstellen**. Bestätigen Sie, wenn Sie aufgefordert werden, die Wiederherstellung des NuGet-Pakete.

> [AZURE.TIP] NuGet-Pakete nicht automatisch wiederhergestellt werden, oder wenn Fehler in Bezug auf einen Fehler bei der Pakete wiederherstellen nicht angezeigt, stellen Sie sicher, dass Sie die [NuGet-Paket-Manager] haben[ nuget_packagemgr] installiert. Aktivieren Sie dann das Herunterladen von fehlenden Pakete. Finden Sie unter [Aktivieren von Paket Wiederherstellen während erstellen] [ nuget_restore] Download-Pakets aktivieren.

In den folgenden Abschnitten wir die Anwendung Beispiel in die Schritte, die sie zum Verarbeiten von einer Arbeitsbelastung im Dienst Stapel ausführt Teile aufzuteilen und diese Schritte im Detail besprechen. Wir empfehlen Ihnen finden Sie in der geöffneten Lösung in Visual Studio, während Sie hochzuarbeiten im weiteren Verlauf dieses Artikels, da Sie nicht jede Zeile Code in der Stichprobe erläutert wird.

Navigieren Sie zu den oberen Rand der `MainAsync` Methode im *DotNetTutorial* -Projekt `Program.cs` Datei so starten Sie mit Schritt 1. Jeden Schritt im folgenden dann ungefähr folgt die Entwicklung von Methode Anrufe in `MainAsync`.

## <a name="step-1-create-storage-containers"></a>Schritt 1: Erstellen von Speichercontainer

![Erstellen von Containern in Azure-Speicher][1]
<br/>

Stapel enthält integrierten Unterstützung für die Interaktion mit Azure-Speicher. Container in Ihr Konto Speicher bietet die Dateien erforderlich, indem Sie die Aufgaben, die in Ihrem Konto Stapel ausgeführt werden. Die Container stellen auch einen Ort zum Speichern der Ausgabedaten, die die Aufgaben zu erstellen. Erstes führt die *DotNetTutorial* -Clientanwendung ist drei Container in [Azure BLOB-Speicher](../storage/storage-introduction.md)zu erstellen:

- **Anwendung**: dieses Containers werden die Anwendung ausführen, indem Sie die Aufgaben sowie einer Abhängigkeit, z. B. dll-Dateien gespeichert.
- **Eingabe**: Aufgaben werden aus dem *Eingabewerte* Container Verarbeitungszeit Datendateien herunterladen.
- **Ergebnis**: Wenn Vorgänge eingegebenen Datei Verarbeitung abgeschlossen haben, werden diese Hochladen der Ergebnisse an den Container *Ausgabe* .

Um ein Speicherkonto interagieren und Container erstellen, verwenden wir [Azure-Speicher-Client-Bibliothek für .NET][net_api_storage]. Wir Erstellen eines 3D-Bezugs auf das Konto aus, mit [CloudStorageAccount][net_cloudstorageaccount], und erstellen Sie eine [CloudBlobClient]aus, die[net_cloudblobclient]:

```csharp
// Construct the Storage account connection string
string storageConnectionString = String.Format(
    "DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}",
    StorageAccountName,
    StorageAccountKey);

// Retrieve the storage account
CloudStorageAccount storageAccount =
    CloudStorageAccount.Parse(storageConnectionString);

// Create the blob client, for use in obtaining references to
// blob storage containers
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
```

Wir verwenden die `blobClient` verwiesen werden in der gesamten Anwendung und auf mehrere Methoden als Parameter zu übergeben. Ein Beispiel dafür ist im Codeblock, die unmittelbar genannten folgt, wo wir nennen `CreateContainerIfNotExistAsync` tatsächlich den Container erstellen.

```csharp
// Use the blob client to create the containers in Azure Storage if they don't
// yet exist
const string appContainerName    = "application";
const string inputContainerName  = "input";
const string outputContainerName = "output";
await CreateContainerIfNotExistAsync(blobClient, appContainerName);
await CreateContainerIfNotExistAsync(blobClient, inputContainerName);
await CreateContainerIfNotExistAsync(blobClient, outputContainerName);
```

```csharp
private static async Task CreateContainerIfNotExistAsync(
    CloudBlobClient blobClient,
    string containerName)
{
        CloudBlobContainer container =
            blobClient.GetContainerReference(containerName);

        if (await container.CreateIfNotExistsAsync())
        {
                Console.WriteLine("Container [{0}] created.", containerName);
        }
        else
        {
                Console.WriteLine("Container [{0}] exists, skipping creation.",
                    containerName);
        }
}
```

Nachdem der Container erstellt wurden, kann die Anwendung nun die Dateien hochladen, die von Aufgaben verwendet werden.

> [AZURE.TIP] [So verwenden Sie BLOB-Speicher von .NET](../storage/storage-dotnet-how-to-use-blobs.md) bietet einen guten Überblick über das Arbeiten mit Azure-Speicher Container und Blobs. Sie sollten am oberen Rand der Liste lesen, wie Sie mit der Stapelverarbeitung zu arbeiten beginnen.

## <a name="step-2-upload-task-application-and-data-files"></a>Schritt 2: Hochladen Sie Vorgang Anwendung und Datendateien

![Hochladen von Vorgang Anwendung und Eingabe (Daten) Dateien zu Containern][2]
<br/>

In der Datei hochladen Operation definiert *DotNetTutorial* zuerst Websitesammlungen **Anwendung** und **Eingabewerte** Datei Wege, wie sie auf dem lokalen Computer vorhanden sind. Und sie diese Dateien auf die Container hochgeladen wird, die Sie im vorherigen Schritt erstellt haben.

```
// Paths to the executable and its dependencies that will be executed by the tasks
List<string> applicationFilePaths = new List<string>
{
    // The DotNetTutorial project includes a project reference to TaskApplication,
    // allowing us to determine the path of the task application binary dynamically
    typeof(TaskApplication.Program).Assembly.Location,
    "Microsoft.WindowsAzure.Storage.dll"
};

// The collection of data files that are to be processed by the tasks
List<string> inputFilePaths = new List<string>
{
    @"..\..\taskdata1.txt",
    @"..\..\taskdata2.txt",
    @"..\..\taskdata3.txt"
};

// Upload the application and its dependencies to Azure Storage. This is the
// application that will process the data files, and will be executed by each
// of the tasks on the compute nodes.
List<ResourceFile> applicationFiles = await UploadFilesToContainerAsync(
    blobClient,
    appContainerName,
    applicationFilePaths);

// Upload the data files. This is the data that will be processed by each of
// the tasks that are executed on the compute nodes within the pool.
List<ResourceFile> inputFiles = await UploadFilesToContainerAsync(
    blobClient,
    inputContainerName,
    inputFilePaths);
```

Es gibt zwei Methoden in `Program.cs` , während des Uploads beteiligt sind:

- `UploadFilesToContainerAsync`: Diese Methode gibt eine Auflistung von [ResourceFile] [ net_resourcefile] Objekte (siehe unten) und intern Anrufe `UploadFileToContainerAsync` jede Datei hochladen, die den Parameter *FilePaths* übergeben.
- `UploadFileToContainerAsync`: Dies ist die Methode, die tatsächlich führt die Datei hochladen und erstellt die [ResourceFile] [ net_resourcefile] Objekte. Nach dem Hochladen der Datei, erhält eine freigegebene Access-Signatur (SAS) für die Datei, und gibt eine ResourceFile Objekt, die ihn darstellt. Freigegebene Zugriff Signaturen auch weiter unten beschrieben werden.

```
private static async Task<ResourceFile> UploadFileToContainerAsync(
    CloudBlobClient blobClient,
    string containerName,
    string filePath)
{
        Console.WriteLine(
            "Uploading file {0} to container [{1}]...", filePath, containerName);

        string blobName = Path.GetFileName(filePath);

        CloudBlobContainer container = blobClient.GetContainerReference(containerName);
        CloudBlockBlob blobData = container.GetBlockBlobReference(blobName);
        await blobData.UploadFromFileAsync(filePath, FileMode.Open);

        // Set the expiry time and permissions for the blob shared access signature.
        // In this case, no start time is specified, so the shared access signature
        // becomes valid immediately
        SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy
        {
                SharedAccessExpiryTime = DateTime.UtcNow.AddHours(2),
                Permissions = SharedAccessBlobPermissions.Read
        };

        // Construct the SAS URL for blob
        string sasBlobToken = blobData.GetSharedAccessSignature(sasConstraints);
        string blobSasUri = String.Format("{0}{1}", blobData.Uri, sasBlobToken);

        return new ResourceFile(blobSasUri, blobName);
}
```

### <a name="resourcefiles"></a>ResourceFiles

Eine [ResourceFile] [ net_resourcefile] bietet Aufgaben in Stapel mit der URL einer Datei in Azure-Speicher, die mit einem Knoten berechnen heruntergeladen wurde, bevor Sie diesen Vorgang ausgeführt wird. Die [ResourceFile.BlobSource] [ net_resourcefile_blobsource] Eigenschaft gibt die vollständige URL der Datei aus, wie sie in Azure-Speicher vorhanden sind. Die URL kann auch eine Signatur freigegebenen Access (SAS) enthalten, die sicheren Zugriff auf die Datei enthält. Die meisten Typen von Vorgängen in .NET Stapel enthalten eine *ResourceFiles* -Eigenschaft, einschließlich:

- [CloudTask][net_task]
- [StartTask][net_pool_starttask]
- [JobPreparationTask][net_jobpreptask]
- [JobReleaseTask][net_jobreltask]

Die Anwendung DotNetTutorial Beispiel die Vorgangsarten JobPreparationTask oder JobReleaseTask nicht verwendet, jedoch können Sie weitere Informationen über diese [Ausführen Vorbereitung und Fertigstellung Projektaufgaben Azure Blattnamen Knoten zu berechnen](batch-job-prep-release.md).

### <a name="shared-access-signature-sas"></a>Freigegebene Access Signatur (SAS)

Freigegebene Access Signaturen sind Zeichenfolgen die – Wenn als Teil einer URL enthalten – bieten sicheren Zugriff auf Container und Blobs in Azure-Speicher. Die Anwendung sowohl Blob-Container freigegeben verwendet DotNetTutorial veranschaulicht, wie diese Zeichenfolgen für den freigegebenen Zugriff Signatur aus der Speicherdienst abgerufen und Signatur URLs zugreifen.

- **BLOB freigegeben Access Signaturen**: des Pool StartTask in DotNetTutorial verwendet Blob freigegeben Access Signaturen aus, wenn sie die Anwendungsbinärdateien und Dateien Eingabedaten aus Speicher downloads (siehe Schritt 3 unten). Die `UploadFileToContainerAsync` Methode in der DotNetTutorial `Program.cs` enthält den Code, der einzelnen Blob des freigegebenen Access Signatur abruft. Bedeutet dies, indem Sie [CloudBlob.GetSharedAccessSignature][net_sas_blob].

- **Container freigegeben Access Signaturen**: wie jeden Vorgang seine Arbeit auf dem Berechnungsknoten beendet, wird deren Ausgabedatei auf den Container *Ausgabe* in Azure-Speicher hochgeladen. Dazu verwendet TaskApplication eine Container freigegebenen Access-Signatur, die als Teil der Pfad Schreibzugriff auf den Container bereitstellt, wenn sie die Datei hochgeladen wird. Abrufen der Container gemeinsamen Zugriff Signatur erfolgt auf ähnliche Weise wie beim Abrufen des BLOBs Access Signatur freigegeben. In DotNetTutorial, finden Sie, die `GetContainerSasUrl` Helper Methode ruft [CloudBlobContainer.GetSharedAccessSignature] [ net_sas_container] vergeblich. Lesen Sie mehr zur Verwendung von TaskApplication auf des Containers freigegebene Access-Signatur in "Schritt 6: Überwachen von Aufgaben."

> [AZURE.TIP] Schauen Sie sich die auf freigegebenen Access Signaturen zweiteiligen Reihe [Teil 1: Grundlegendes zu freigegebenen Access Signatur (SAS) Modell](../storage/storage-dotnet-shared-access-signature-part-1.md) und [Teil 2: Erstellen und verwenden eine freigegebenen Access-Signatur (SAS) mit Blob-Speicher](../storage/storage-dotnet-shared-access-signature-part-2.md), um mehr über den sicheren Zugriff auf Daten in Ihr Konto Speicher zu erfahren.

## <a name="step-3-create-batch-pool"></a>Schritt 3: Erstellen von Stapel Ressourcenpool

![Erstellen von einem Stapel Ressourcenpool][3]
<br/>

Einem Stapel **Ressourcenpool** ist eine Auflistung von Computeknoten (virtuelle Maschinen), auf denen Stapel Vorgänge des Projekts ausgeführt wird.

Nachdem sie die Anwendung und Datendateien mit dem Konto Speicher hochgeladen wird, beginnt *DotNetTutorial* seine Interaktion mit dem Dienst Stapel mithilfe der Objektbibliothek Stapel .NET an. Eine [BatchClient] dazu[ net_batchclient] wird zuerst erstellt:

```csharp
BatchSharedKeyCredentials cred = new BatchSharedKeyCredentials(
    BatchAccountUrl,
    BatchAccountName,
    BatchAccountKey);

using (BatchClient batchClient = BatchClient.Open(cred))
{
    ...
```

Anschließend wird in den Stapel Konto mit einem Anruf, um ein Pool von Computeknoten erstellt `CreatePoolAsync`. `CreatePoolAsync`verwendet die [BatchClient.PoolOperations.CreatePool] [ net_pool_create] Methode, um einen Pool tatsächlich im Dienst Stapel zu erstellen.

```csharp
private static async Task CreatePoolAsync(
    BatchClient batchClient,
    string poolId,
    IList<ResourceFile> resourceFiles)
{
    Console.WriteLine("Creating pool [{0}]...", poolId);

    // Create the unbound pool. Until we call CloudPool.Commit() or CommitAsync(),
    // no pool is actually created in the Batch service. This CloudPool instance is
    // therefore considered "unbound," and we can modify its properties.
    CloudPool pool = batchClient.PoolOperations.CreatePool(
            poolId: poolId,
            targetDedicated: 3,           // 3 compute nodes
            virtualMachineSize: "small",  // single-core, 1.75 GB memory, 224 GB disk
            cloudServiceConfiguration:
                new CloudServiceConfiguration(osFamily: "4")); // Win Server 2012 R2

    // Create and assign the StartTask that will be executed when compute nodes join
    // the pool. In this case, we copy the StartTask's resource files (that will be
    // automatically downloaded to the node by the StartTask) into the shared
    // directory that all tasks will have access to.
    pool.StartTask = new StartTask
    {
        // Specify a command line for the StartTask that copies the task application
        // files to the node's shared directory. Every compute node in a Batch pool
        // is configured with several pre-defined environment variables that you can
        // reference by using commands or applications run by tasks.

        // Since a successful execution of robocopy can return a non-zero exit code
        // (e.g. 1 when one or more files were successfully copied) we need to
        // manually exit with a 0 for Batch to recognize StartTask execution success.
        CommandLine = "cmd /c (robocopy %AZ_BATCH_TASK_WORKING_DIR% %AZ_BATCH_NODE_SHARED_DIR%) ^& IF %ERRORLEVEL% LEQ 1 exit 0",
        ResourceFiles = resourceFiles,
        WaitForSuccess = true
    };

    await pool.CommitAsync();
}
```

Beim Erstellen einer Ressourcenpool mit [CreatePool][net_pool_create], wie etwa die Anzahl der Knoten berechnen, die [Größe der Knoten](../cloud-services/cloud-services-sizes-specs.md)und die Knoten Betriebssystem mehrere Parameter angeben. In *DotNetTutorial*, verwenden wir [CloudServiceConfiguration] [ net_cloudserviceconfiguration] Windows Server 2012 R2 aus der [Cloud Services](../cloud-services/cloud-services-guestos-update-matrix.md)angeben. Jedoch durch die Angabe einer [VirtualMachineConfiguration] [ net_virtualmachineconfiguration] Sie stattdessen können erstellen Speicherpools Knoten Marketplace-Bilder erstellt, Windows und Linux Bilder einschließlich – finden Sie weitere Informationen [Bereitstellen von Linux Knoten in Azure Stapel Pools zu berechnen](batch-linux-nodes.md) .

> [AZURE.IMPORTANT] Sie unterliegen für Ressourcen Blatt berechnen. Um Kosten zu minimieren, können Sie tieferzustufen `targetDedicated` 1, bevor Sie das Beispiel ausführen.

Zusammen mit diesen Eigenschaften physischen Knoten, Sie können auch angeben einer [StartTask] [ net_pool_starttask] für den Pool. Die StartTask führt auf den einzelnen Knoten, wie der Knoten Pool Beitritt und jedes Mal ein Knoten neu gestartet wird. Die StartTask ist besonders hilfreich, für die Installation von Applications auf Datenverarbeitungsknoten vor der Ausführung von Aufgaben. Angenommen, wenn Ihre Vorgänge Daten verarbeiten mithilfe von Python, können eine StartTask Sie um Python auf den Knoten berechnen zu installieren.

In diesem Beispiel-Anwendung, die StartTask kopiert die Dateien, die sie aus dem Speicher downloads (die werden mithilfe der [StartTask]angegeben[net_starttask].[ ResourceFiles] [ net_starttask_resourcefiles] Eigenschaft) aus dem Verzeichnis StartTask arbeiten, auf das freigegebene Verzeichnis, die *Alle* Vorgänge auf dem Knoten zugreifen können. Dadurch wird im Wesentlichen kopiert `TaskApplication.exe` und auf das freigegebene Verzeichnis auf den einzelnen Knoten Abhängigkeit davon, wie der Knoten im Ressourcenpool beitritt, damit alle Aufgaben ausführen, die auf dem Knoten darauf zugreifen können.

> [AZURE.TIP] Das Feature **Anwendungspakete** des Blatts Azure bietet eine weitere Möglichkeit, eine Anwendung auf die Datenverarbeitungsknoten in einem Ressourcenpool zu gelangen. Details finden Sie unter [Bereitstellung der Anwendung mit Azure Stapel Anwendungspakete](batch-application-packages.md) .

Auch wichtigste im obigen Codeausschnitt Änderung betrifft die Verwendung von zwei Umgebungsvariablen in der Eigenschaft *Befehlszeile* die StartTask: `%AZ_BATCH_TASK_WORKING_DIR%` und `%AZ_BATCH_NODE_SHARED_DIR%`. Jeder Computeknoten in einem Stapel Pool wird automatisch mit mehreren Umgebungsvariablen konfiguriert werden, die zum Stapel spezifisch sind. Jeder Prozess, der durch einen Vorgang ausgeführt wird hat Zugriff auf diese Umgebungsvariablen.

> [AZURE.TIP] Wenn Sie mehr über die Umgebung erfahren finden Sie unter Variablen, die auf Datenverarbeitungsknoten in einem Stapel Ressourcenpool und Informationen zum Vorgang arbeiten Verzeichnisse durchsuchen, verfügbar sind in den Abschnitten [-Umgebung, die Einstellungen für Vorgänge](batch-api-basics.md#environment-settings-for-tasks) und [Dateien und Verzeichnissen](batch-api-basics.md#files-and-directories) im [Stapel Übersicht über die Features für Entwickler](batch-api-basics.md).

## <a name="step-4-create-batch-job"></a>Schritt 4: Erstellen von Stapelverarbeitung

![Stapelverarbeitung erstellen][4]<br/>

Eine **Position** ist eine Sammlung von Vorgängen und einem Pool von Computeknoten zugeordnet ist. Die Aufgaben eines Projekts auf die zugeordneten Ressourcenpool berechnen Knoten ausgeführt werden.

Sie können ein Projekt nicht nur zum Organisieren und Nachverfolgen von Aufgaben in verknüpften Auslastung, sondern auch zur Erhebung von bestimmter Einschränkungen – wie etwa die maximale Laufzeit für das Projekt (und von Erweiterung, deren Aufgaben) sowie Priorität in Bezug auf die anderen Projekten in den Stapel-Konto verwenden. In diesem Beispiel ist die Aufgabe jedoch nur mit dem Pool, der in Schritt 3 # erstellte verknüpft. Es sind keine zusätzlichen Eigenschaften konfiguriert.

Alle Stapelverarbeitungsaufträge stehen im Zusammenhang mit einem bestimmten Pool. Diese Zuordnung gibt an, welche Knoten auf Vorgänge des Projekts ausgeführt werden. Sie angeben mithilfe der [CloudJob.PoolInformation] [ net_job_poolinfo] Eigenschaft, wie im folgenden Codeausschnitt dargestellt.

```csharp
private static async Task CreateJobAsync(
    BatchClient batchClient,
    string jobId,
    string poolId)
{
    Console.WriteLine("Creating job [{0}]...", jobId);

    CloudJob job = batchClient.JobOperations.CreateJob();
    job.Id = jobId;
    job.PoolInformation = new PoolInformation { PoolId = poolId };

    await job.CommitAsync();
}
```

Nachdem Sie nun ein Auftrag erstellt wurde, werden Aufgaben für die Arbeitsschritte hinzugefügt.

## <a name="step-5-add-tasks-to-job"></a>Schritt 5: Hinzufügen von Aufgaben zum Projekt

![Hinzufügen von Aufgaben zum Projekt][5]<br/>
*(1) Vorgänge werden dem Projekt hinzugefügt, (2) die Vorgänge werden planmäßig auf Knoten ausführen und (3) die Aufgaben herunterladen, die zu verarbeitenden Datendateien*

Stapel **Aufgaben** sind die einzelnen Einheiten von Arbeit, die auf den Knoten berechnen ausführen. Eine Aufgabe verfügt über eine Befehlszeile und führt die Skripts oder ausführbare Dateien, die Sie in die Befehlszeile angeben.

Um die Arbeit tatsächlich ausführen zu können, müssen Aufgaben mit einem Projekt hinzugefügt werden. Jede [CloudTask] [ net_task] ist so konfiguriert, dass mithilfe einer Befehlszeile Eigenschaft und [ResourceFiles] [ net_task_resourcefiles] (wie mit dem Pool des StartTask), die die Aufgabe downloads auf den Knoten, bevor die Befehlszeile automatisch ausgeführt wird. In der Stichprobe Project *DotNetTutorial* verarbeitet jeden Vorgang nur eine Datei. Auf diese Weise enthält seiner ResourceFiles-Auflistung ein einzelnes Element.

```csharp
private static async Task<List<CloudTask>> AddTasksAsync(
    BatchClient batchClient,
    string jobId,
    List<ResourceFile> inputFiles,
    string outputContainerSasUrl)
{
    Console.WriteLine("Adding {0} tasks to job [{1}]...", inputFiles.Count, jobId);

    // Create a collection to hold the tasks that we'll be adding to the job
    List<CloudTask> tasks = new List<CloudTask>();

    // Create each of the tasks. Because we copied the task application to the
    // node's shared directory with the pool's StartTask, we can access it via
    // the shared directory on the node that the task runs on.
    foreach (ResourceFile inputFile in inputFiles)
    {
        string taskId = "topNtask" + inputFiles.IndexOf(inputFile);
        string taskCommandLine = String.Format(
            "cmd /c %AZ_BATCH_NODE_SHARED_DIR%\\TaskApplication.exe {0} 3 \"{1}\"",
            inputFile.FilePath,
            outputContainerSasUrl);

        CloudTask task = new CloudTask(taskId, taskCommandLine);
        task.ResourceFiles = new List<ResourceFile> { inputFile };
        tasks.Add(task);
    }

    // Add the tasks as a collection, as opposed to issuing a separate AddTask call
    // for each. Bulk task submission helps to ensure efficient underlying API calls
    // to the Batch service.
    await batchClient.JobOperations.AddTaskAsync(jobId, tasks);

    return tasks;
}
```

> [AZURE.IMPORTANT] Wenn sie Zugriff auf Umgebungsvariablen wie `%AZ_BATCH_NODE_SHARED_DIR%` oder Ausführen der Anwendung nicht befindet sich der Knoten `PATH`, Befehl Aufgabenzeilen muss mit dem Präfix `cmd /c`. Dadurch wird explizit den Befehlsinterpreter ausgeführt, und weisen Sie es nach Durchführung des Befehls beenden. Diese Anforderung ist nicht erforderlich, wenn Ihre Aufgaben, eine Anwendung im des Knotens ausführen `PATH` (z. B. *robocopy.exe* oder *powershell.exe*) und keine Umgebungsvariablen verwendet werden.

Innerhalb der `foreach` Schleife im obigen Codeausschnitt, können Sie sehen, dass die Befehlszeile für den Vorgang erstellt wird, sodass drei Befehlszeilenargumente an *TaskApplication.exe*übergeben werden:

1. Das **erste Argument** ist der Pfad der Datei zu verarbeiten. Dies ist der lokale Pfad zu der Datei aus, wie sie auf dem Knoten vorhanden sind. Wenn die ResourceFile Objekt `UploadFileToContainerAsync` erstmalig erstellt wurde, der Dateinamen (als Parameter an den Konstruktor ResourceFile) für diese Eigenschaft verwendet wurde. Dies zeigt an, dass die Datei im selben Verzeichnis als *TaskApplication.exe*gefunden werden kann.

2. Das **zweite Argument** gibt an, dass die obersten *N* Wörter in der Ausgabedatei geschrieben werden sollen. In der Stichprobe ist dies hartcodierte, damit die oberen drei Wörter in der Ausgabedatei geschrieben werden.

3. Das **dritte Argument** ist die freigegebene Access Signatur (SAS), die Schreibzugriff auf den Container **Ausgabe** in Azure-Speicher bereitstellt. *TaskApplication.exe* verwendet diese gemeinsamen Zugriff Signatur URL aus, wenn sie die Ausgabedatei zu Azure-Speicher hochgeladen wird. Suchen Sie den Code für hierzu finden Sie in der `UploadFileToContainer` Methode im TaskApplication-Projekt `Program.cs` Datei:

```csharp
// NOTE: From project TaskApplication Program.cs

private static void UploadFileToContainer(string filePath, string containerSas)
{
        string blobName = Path.GetFileName(filePath);

        // Obtain a reference to the container using the SAS URI.
        CloudBlobContainer container = new CloudBlobContainer(new Uri(containerSas));

        // Upload the file (as a new blob) to the container
        try
        {
                CloudBlockBlob blob = container.GetBlockBlobReference(blobName);
                blob.UploadFromFile(filePath, FileMode.Open);

                Console.WriteLine("Write operation succeeded for SAS URL " + containerSas);
                Console.WriteLine();
        }
        catch (StorageException e)
        {

                Console.WriteLine("Write operation failed for SAS URL " + containerSas);
                Console.WriteLine("Additional error information: " + e.Message);
                Console.WriteLine();

                // Indicate that a failure has occurred so that when the Batch service
                // sets the CloudTask.ExecutionInformation.ExitCode for the task that
                // executed this application, it properly indicates that there was a
                // problem with the task.
                Environment.ExitCode = -1;
        }
}
```

## <a name="step-6-monitor-tasks"></a>Schritt 6: Überwachen von Aufgaben

![Überwachen von Aufgaben][6]<br/>
*Die Clientanwendung (1) überwacht die Vorgänge zur Beendigung und Status Erfolg und (2) die Aufgaben Ergebnisdaten auf Azure-Speicher hochladen*

Wenn Aufgaben mit einem Projekt hinzugefügt haben, werden sie automatisch in der Warteschlange und auf Datenverarbeitungsknoten im Pool zugeordnet den Auftrag für die Ausführung geplant. Je nach den Einstellungen, die Sie angeben, übernimmt Stapel alle Vorgang queuing, Planung, Wiederholung und anderen Vorgang Administration Aufgaben für Sie an. Es gibt viele Vorgehensweisen für die Überwachung der Ausführung der Aufgabe ein. DotNetTutorial zeigt ein einfaches Beispiel, das nur auf Fehler bei der Fertigstellung und die Aufgabe oder Erfolg Staaten Berichte.

Innerhalb der `MonitorTasks` Methode in der DotNetTutorial `Program.cs`, es gibt drei Stapel .NET Konzepte, die Diskussion rechtfertigt. Sie können in der Reihenfolge der Darstellung werden nachfolgend aufgeführt:

1. **ODATADetailLevel**: angeben [ODATADetailLevel] [ net_odatadetaillevel] Vorgänge (z. B., erhalten eine Liste der Vorgänge des Projekts) in der Liste ist entscheidend Stapel Anwendung Leistung zu gewährleisten. Fügen Sie [den Stapel Azure-Dienst effizient Abfragen](batch-efficient-list-queries.md) zu Ihrer Liste der Lesebereich fest, wenn Sie beabsichtigen, klicken Sie auf eine beliebige Sortieren der Status für die Überwachung in Ihrer Anwendung Stapel ausführen.

2. **TaskStateMonitor**: [TaskStateMonitor] [ net_taskstatemonitor] Stapel .NET Applications mit Helper Dienstprogramme zum Überwachen der Aufgabe Staaten enthält. In `MonitorTasks`, *DotNetTutorial* für alle Vorgänge [TaskState.Completed] erreichen wartet[ net_taskstate] innerhalb einer Frist. Klicken Sie dann den Auftrag beendet wird.

3. **TerminateJobAsync**: beendet ein Projekt mit [JobOperations.TerminateJobAsync] [ net_joboperations_terminatejob] (oder die blockierenden JobOperations.TerminateJob) die Löschung markiert, als abgeschlossen. Es ist wichtig, vergeblich, wenn Ihre Lösung Stapel eine [JobReleaseTask]verwendet[net_jobreltask]. Dies ist eine spezielle Art der Aufgabe, bei der [Vorbereitung und Fertigstellung Projektaufgaben](batch-job-prep-release.md)beschrieben ist.

Die `MonitorTasks` Methode aus *DotNetTutorial* `Program.cs` unter angezeigt wird:

```csharp
private static async Task<bool> MonitorTasks(
    BatchClient batchClient,
    string jobId,
    TimeSpan timeout)
{
    bool allTasksSuccessful = true;
    const string successMessage = "All tasks reached state Completed.";
    const string failureMessage = "One or more tasks failed to reach the Completed state within the timeout period.";

    // Obtain the collection of tasks currently managed by the job. Note that we use
    // a detail level to  specify that only the "id" property of each task should be
    // populated. Using a detail level for all list operations helps to lower
    // response time from the Batch service.
    ODATADetailLevel detail = new ODATADetailLevel(selectClause: "id");
    List<CloudTask> tasks =
        await batchClient.JobOperations.ListTasks(JobId, detail).ToListAsync();

    Console.WriteLine("Awaiting task completion, timeout in {0}...",
        timeout.ToString());

    // We use a TaskStateMonitor to monitor the state of our tasks. In this case, we
    // will wait for all tasks to reach the Completed state.
    TaskStateMonitor taskStateMonitor
        = batchClient.Utilities.CreateTaskStateMonitor();

    try
    {
        await taskStateMonitor.WhenAll(tasks, TaskState.Completed, timeout);
    }
    catch (TimeoutException)
    {
        await batchClient.JobOperations.TerminateJobAsync(jobId, failureMessage);
        Console.WriteLine(failureMessage);
        return false;
    }

    await batchClient.JobOperations.TerminateJobAsync(jobId, successMessage);

    // All tasks have reached the "Completed" state, however, this does not
    // guarantee all tasks completed successfully. Here we further check each task's
    // ExecutionInfo property to ensure that it did not encounter a scheduling error
    // or return a non-zero exit code.

    // Update the detail level to populate only the task id and executionInfo
    // properties. We refresh the tasks below, and need only this information for
    // each task.
    detail.SelectClause = "id, executionInfo";

    foreach (CloudTask task in tasks)
    {
        // Populate the task's properties with the latest info from the
        // Batch service
        await task.RefreshAsync(detail);

        if (task.ExecutionInformation.SchedulingError != null)
        {
            // A scheduling error indicates a problem starting the task on the node.
            // It is important to note that the task's state can be "Completed," yet
            // still have encountered a scheduling error.

            allTasksSuccessful = false;

            Console.WriteLine("WARNING: Task [{0}] encountered a scheduling error: {1}",
                task.Id,
                task.ExecutionInformation.SchedulingError.Message);
        }
        else if (task.ExecutionInformation.ExitCode != 0)
        {
            // A non-zero exit code may indicate that the application executed by
            // the task encountered an error during execution. As not every
            // application returns non-zero on failure by default (e.g. robocopy),
            // your implementation of error checking may differ from this example.

            allTasksSuccessful = false;

            Console.WriteLine("WARNING: Task [{0}] returned a non-zero exit code - this may indicate task execution or completion failure.", task.Id);
        }
    }

    if (allTasksSuccessful)
    {
        Console.WriteLine("Success! All tasks completed successfully within the specified timeout period.");
    }

    return allTasksSuccessful;
}
```

## <a name="step-7-download-task-output"></a>Schritt 7: Herunterladen Sie Vorgang Ausgabe

![Herunterladen der Aufgabe Ausgabe von Speicher][7]<br/>

Jetzt, da das Projekt abgeschlossen ist, kann die Ausgabe von Aufgaben aus Azure-Speicher heruntergeladen werden. Dies erfolgt mit einem Anruf, um `DownloadBlobsFromContainerAsync` in *DotNetTutorial* `Program.cs`:

```csharp
private static async Task DownloadBlobsFromContainerAsync(
    CloudBlobClient blobClient,
    string containerName,
    string directoryPath)
{
        Console.WriteLine("Downloading all files from container [{0}]...", containerName);

        // Retrieve a reference to a previously created container
        CloudBlobContainer container = blobClient.GetContainerReference(containerName);

        // Get a flat listing of all the block blobs in the specified container
        foreach (IListBlobItem item in container.ListBlobs(
                    prefix: null,
                    useFlatBlobListing: true))
        {
                // Retrieve reference to the current blob
                CloudBlob blob = (CloudBlob)item;

                // Save blob contents to a file in the specified folder
                string localOutputFile = Path.Combine(directoryPath, blob.Name);
                await blob.DownloadToFileAsync(localOutputFile, FileMode.Create);
        }

        Console.WriteLine("All files downloaded to {0}", directoryPath);
}
```

> [AZURE.NOTE] Den Anruf an die `DownloadBlobsFromContainerAsync` in der *DotNetTutorial* Anwendung gibt an, dass die Dateien sollen, um heruntergeladen werden Ihre `%TEMP%` Ordner. Diese Ausgabeort ändern können.

## <a name="step-8-delete-containers"></a>Schritt 8: Löschen Container

Da Sie Daten, die in Azure Storage gespeichert ist berechnet werden, ist es immer eine gute Idee, alle Blobs zu entfernen, die für Ihre Aufträge Stapel nicht mehr benötigt werden. In den DotNetTutorial `Program.cs`, dies geschieht mit drei Anrufe an die Methode Helper `DeleteContainerAsync`:

```csharp
// Clean up Storage resources
await DeleteContainerAsync(blobClient, appContainerName);
await DeleteContainerAsync(blobClient, inputContainerName);
await DeleteContainerAsync(blobClient, outputContainerName);
```

Die Methode selbst erhält lediglich einen Verweis auf den Container, und klicken Sie dann ruft [CloudBlobContainer.DeleteIfExistsAsync][net_container_delete]:

```csharp
private static async Task DeleteContainerAsync(
    CloudBlobClient blobClient,
    string containerName)
{
    CloudBlobContainer container = blobClient.GetContainerReference(containerName);

    if (await container.DeleteIfExistsAsync())
    {
        Console.WriteLine("Container [{0}] deleted.", containerName);
    }
    else
    {
        Console.WriteLine("Container [{0}] does not exist, skipping deletion.",
            containerName);
    }
}
```

## <a name="step-9-delete-the-job-and-the-pool"></a>Schritt 9: Löschen Sie den Auftrag und dem pool

Im letzten Schritt wird der Benutzer aufgefordert, löschen Sie den Auftrag und dem Pool, die von der Anwendung DotNetTutorial erstellt wurden. Obwohl Sie für Projekte und Vorgänge selbst, Sie *sind* keine entrichten müssen Datenverarbeitungsknoten in Rechnung gestellt. Auf diese Weise wird empfohlen, dass Sie Knoten bei Bedarf nur zuweisen. Löschen nicht verwendeter Pools kann Teil Ihrer Verwaltungsvorgang sein.

Die BatchClients [JobOperations] [ net_joboperations] und [PoolOperations] [ net_pooloperations] beide entsprechenden Löschvorgang Methoden, mit denen aufgerufen werden, wenn der Benutzer den Löschvorgang bestätigt haben:

```csharp
// Clean up the resources we've created in the Batch account if the user so chooses
Console.WriteLine();
Console.WriteLine("Delete job? [yes] no");
string response = Console.ReadLine().ToLower();
if (response != "n" && response != "no")
{
    await batchClient.JobOperations.DeleteJobAsync(JobId);
}

Console.WriteLine("Delete pool? [yes] no");
response = Console.ReadLine();
if (response != "n" && response != "no")
{
    await batchClient.PoolOperations.DeletePoolAsync(PoolId);
}
```

> [AZURE.IMPORTANT] Denken Sie daran, die Sie für Ressourcen berechnen belastet werden – löschen nicht verwendeter Pools Kosten minimiert werden. Beachten Sie auch, dass ein Ressourcenpool alle Computeknoten in diesem Pool löschen, löschen und nach dem Löschen der Ressourcenpool alle Daten auf den Knoten können nicht wiederhergestellt werden können, die.

## <a name="run-the-dotnettutorial-sample"></a>Führen Sie das Beispiel *DotNetTutorial*

Wenn Sie die Beispiel-Anwendung ausführen, werden die Ausgabe Console ähnlich wie der folgende. Während der Ausführung erzielen Sie eine Pause am `Awaiting task completion, timeout in 00:30:00...` während des Pool berechnen Knoten gestartet werden. Verwenden der [Azure-Portal] [ azure_portal] zum Überwachen der Ressourcenpool zu berechnen Knoten, Position und Aufgaben während und nach der Ausführung. Verwenden der [Azure-Portal] [ azure_portal] oder der [Azure-Speicher-Explorer] [ storage_explorers] die Ressourcen Speicher (Container und Blobs) anzeigen möchten, die von der Anwendung erstellt werden.

Typische dauert Ausführung **etwa 5 Minuten** beim Ausführen der Anwendungs in der Standardkonfiguration.

```
Sample start: 1/8/2016 09:42:58 AM

Container [application] created.
Container [input] created.
Container [output] created.
Uploading file C:\repos\azure-batch-samples\CSharp\ArticleProjects\DotNetTutorial\bin\Debug\TaskApplication.exe to container [application]...
Uploading file Microsoft.WindowsAzure.Storage.dll to container [application]...
Uploading file ..\..\taskdata1.txt to container [input]...
Uploading file ..\..\taskdata2.txt to container [input]...
Uploading file ..\..\taskdata3.txt to container [input]...
Creating pool [DotNetTutorialPool]...
Creating job [DotNetTutorialJob]...
Adding 3 tasks to job [DotNetTutorialJob]...
Awaiting task completion, timeout in 00:30:00...
Success! All tasks completed successfully within the specified timeout period.
Downloading all files from container [output]...
All files downloaded to C:\Users\USERNAME\AppData\Local\Temp
Container [application] deleted.
Container [input] deleted.
Container [output] deleted.

Sample end: 1/8/2016 09:47:47 AM
Elapsed time: 00:04:48.5358142

Delete job? [yes] no: yes
Delete pool? [yes] no: yes

Sample complete, hit ENTER to exit...
```

## <a name="next-steps"></a>Nächste Schritte

*DotNetTutorial* und *TaskApplication* so experimentieren Sie mit unterschiedlichen berechnen Szenarien ändern können. Probieren Sie beispielsweise eine Verzögerung Ausführung zu *TaskApplication*, z. B. mit [Thread.Sleep]hinzufügen[net_thread_sleep], um zu reproduzieren langer Aufgaben und im Portal zu überwachen. Versuchen Sie es weitere Aufgaben hinzufügen oder die Anzahl der berechnen Knoten anpassen. Hinzufügen von Logik zum Überprüfen und die Verwendung von einem vorhandenen Pool zum schnellen Ausführung zulassen (*Hinweis*: Auschecken `ArticleHelpers.cs` in der [Microsoft.Azure.Batch.Samples.Common] [ github_samples_common] Projekt in [Azure-Stapel-Beispiele][github_samples]).

Nachdem Sie nun mit den grundlegenden Workflow einer Lösung Stapel vertraut sind, ist es Zeit, auf die zusätzliche Features des Diensts Stapel befasst.

- Lesen Sie [Stapel Übersicht über die Features für Entwickler](batch-api-basics.md), die für alle neuen Stapel Benutzer sollten.
- Starten Sie auf den anderen Stapel Entwicklung Artikeln in der **Entwicklung detaillierter** in den [Stapel learning Path][batch_learning_path].
- Auschecken einer anderen Implementierung Verarbeitung die Arbeitsbelastung "Top N Wörter" in der [TopNWords] mithilfe von Stapel[ github_topnwords] Stichprobe.

[azure_batch]: https://azure.microsoft.com/services/batch/
[azure_free_account]: https://azure.microsoft.com/free/
[azure_portal]: https://portal.azure.com
[batch_learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/
[github_batchexplorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[github_dotnettutorial]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/DotNetTutorial
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_common]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/Common
[github_samples_zip]: https://github.com/Azure/azure-batch-samples/archive/master.zip
[github_topnwords]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/TopNWords
[net_api]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[net_api_storage]: https://msdn.microsoft.com/library/azure/mt347887.aspx
[net_batchclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudblobclient]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobclient.aspx
[net_cloudblobcontainer]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.aspx
[net_cloudstorageaccount]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.cloudstorageaccount.aspx
[net_cloudserviceconfiguration]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudserviceconfiguration.aspx
[net_container_delete]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.deleteifexistsasync.aspx
[net_job]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_job_poolinfo]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.protocol.models.cloudjob.poolinformation.aspx
[net_joboperations]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.joboperations
[net_joboperations_terminatejob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.terminatejobasync.aspx
[net_jobpreptask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobpreparationtask.aspx
[net_jobreltask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobreleasetask.aspx
[net_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.aspx
[net_odatadetaillevel]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_pool_create]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[net_pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_pooloperations]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.pooloperations
[net_resourcefile]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.resourcefile.aspx
[net_resourcefile_blobsource]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.resourcefile.blobsource.aspx
[net_sas_blob]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblob.getsharedaccesssignature.aspx
[net_sas_container]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblobcontainer.getsharedaccesssignature.aspx
[net_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.aspx
[net_starttask_resourcefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.resourcefiles.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_task_resourcefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.resourcefiles.aspx
[net_taskstate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.taskstate.aspx
[net_taskstatemonitor]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskstatemonitor.aspx
[net_thread_sleep]: https://msdn.microsoft.com/library/274eh01d(v=vs.110).aspx
[net_virtualmachineconfiguration]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.virtualmachineconfiguration.aspx
[nuget_packagemgr]: https://docs.nuget.org/consume/installing-nuget
[nuget_restore]: https://docs.nuget.org/consume/package-restore/msbuild-integrated#enabling-package-restore-during-build
[storage_explorers]: http://storageexplorer.com/
[visual_studio]: https://www.visualstudio.com/products/vs-2015-product-editions

[1]: ./media/batch-dotnet-get-started/batch_workflow_01_sm.png "Erstellen von Containern in Azure-Speicher"
[2]: ./media/batch-dotnet-get-started/batch_workflow_02_sm.png "Hochladen von Vorgang Anwendung und Eingabe (Daten) Dateien zu Containern"
[3]: ./media/batch-dotnet-get-started/batch_workflow_03_sm.png "Erstellen Sie Stapel Ressourcenpool"
[4]: ./media/batch-dotnet-get-started/batch_workflow_04_sm.png "Stapelverarbeitung erstellen"
[5]: ./media/batch-dotnet-get-started/batch_workflow_05_sm.png "Hinzufügen von Aufgaben zum Projekt"
[6]: ./media/batch-dotnet-get-started/batch_workflow_06_sm.png "Überwachen von Aufgaben"
[7]: ./media/batch-dotnet-get-started/batch_workflow_07_sm.png "Herunterladen der Aufgabe Ausgabe von Speicher"
[8]: ./media/batch-dotnet-get-started/batch_workflow_sm.png "Stapel Lösung Workflow (vollständige Diagramm)"
[9]: ./media/batch-dotnet-get-started/credentials_batch_sm.png "Stapel Anmeldeinformationen-Portal"
[10]: ./media/batch-dotnet-get-started/credentials_storage_sm.png "Speicher Anmeldeinformationen-Portal"
[11]: ./media/batch-dotnet-get-started/batch_workflow_minimal_sm.png "Stapel Lösung Workflow (minimale Diagramm)"

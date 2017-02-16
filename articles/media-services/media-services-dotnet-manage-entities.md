
<properties 
    pageTitle="Verwalten von Ressourcen und verwandte Elemente mit Media-Dienste .NET SDK" 
    description="Informationen Sie zum Verwalten von Bestand und die zugehörigen Elemente mit dem Media Services SDK für .NET." 
    authors="juliako" 
    manager="dwrede" 
    editor="" 
    services="media-services" 
    documentationCenter=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016"
    ms.author="juliako"/>


#<a name="managing-assets-and-related-entities-with-media-services-net-sdk"></a>Verwalten von Ressourcen und verwandte Elemente mit Media-Dienste .NET SDK


> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-manage-entities.md)
- [REST](media-services-rest-manage-entities.md)


In diesem Thema veranschaulicht, wie die folgenden Media-Dienste Management Aufgaben ausführen:

- Abrufen von einem Objekt Bezug 
- Rufen Sie einen Verweis Position 
- Liste aller Anlagen 
- Liste Aufträge und Anlagen 
- Liste aller Richtlinien 
- Liste aller Locator
- Auflisten von großen Sammlungen von Personen
- Löschen einer Anlageguts 
- Löschen eines Auftrags 
- Löschen einer Zugriffsrichtlinie 

##<a name="prerequisites"></a>Erforderliche Komponenten 

Finden Sie unter [Einrichten der Umgebung](media-services-set-up-computer.md)

##<a name="get-an-asset-reference"></a>Abrufen von einem Objekt Bezug

Eine häufige Aufgabe besteht darin, einen Verweis auf einer vorhandenen Anlage in Media-Dienste zu erhalten. Im folgenden Code wird veranschaulicht, wie Sie ein Objekt Bezug aus der Sammlung von Anlagen auf dem Server Kontextobjekt, basierend auf einer Anlage ID abrufen
Im folgenden Code wird verwendet eine Linq-Abfrage in einen Verweis auf ein vorhandenes IAsset Objekt erhalten möchten.

    static IAsset GetAsset(string assetId)
    {
        // Use a LINQ Select query to get an asset.
        var assetInstance =
            from a in _context.Assets
            where a.Id == assetId
            select a;
        // Reference the asset as an IAsset.
        IAsset asset = assetInstance.FirstOrDefault();
    
        return asset;
    }

##<a name="get-a-job-reference"></a>Rufen Sie einen Verweis Position

Beim Arbeiten mit Aufgaben in Media Services Code Verarbeitung, müssen Sie häufig einen Verweis auf einen vorhandenen Auftrag auf der Grundlage einer ID erhalten Im folgenden Code wird gezeigt, wie in einen Verweis auf ein IJob Objekt aus der Sammlung von Aufträgen erhalten möchten.
WarningWarning Sie müssen möglicherweise einen Verweis Auftrag beim Start von eines langer codieren Auftrags erhalten und müssen Sie den Projektstatus in einem Thread zu überprüfen. In Fällen wie folgt Beendigung die Methode von einem anderen Thread, müssen Sie einen aktualisierten Verweis auf eine Stelle abrufen.

    static IJob GetJob(string jobId)
    {
        // Use a Linq select query to get an updated 
        // reference by Id. 
        var jobInstance =
            from j in _context.Jobs
            where j.Id == jobId
            select j;
        // Return the job reference as an Ijob. 
        IJob job = jobInstance.FirstOrDefault();
    
        return job;
    }

##<a name="list-all-assets"></a>Liste aller Anlagen

Während die Anzahl der Objekte, die Sie im Speicher haben immer umfangreicher wird, ist es hilfreich, Ihre Bestände jederzeit Liste. Im folgenden Code wird gezeigt, wie die Sammlung Anlagen auf dem Server Kontextobjekt durchlaufen. Für jede Anlage im Codebeispiel auch einige seiner Eigenschaft Werte in die Konsole geschrieben. Beispielsweise kann jede Anlage viele Mediendateien enthalten. Im Codebeispiel schreibt alle Dateien, die mit den jeweiligen Ressourcen verbunden ist.



    static void ListAssets()
    {
        string waitMessage = "Building the list. This may take a few "
            + "seconds to a few minutes depending on how many assets "
            + "you have."
            + Environment.NewLine + Environment.NewLine
            + "Please wait..."
            + Environment.NewLine;
        Console.Write(waitMessage);
    
        // Create a Stringbuilder to store the list that we build. 
        StringBuilder builder = new StringBuilder();
    
        foreach (IAsset asset in _context.Assets)
        {
            // Display the collection of assets.
            builder.AppendLine("");
            builder.AppendLine("******ASSET******");
            builder.AppendLine("Asset ID: " + asset.Id);
            builder.AppendLine("Name: " + asset.Name);
            builder.AppendLine("==============");
            builder.AppendLine("******ASSET FILES******");
    
            // Display the files associated with each asset. 
            foreach (IAssetFile fileItem in asset.AssetFiles)
            {
                builder.AppendLine("Name: " + fileItem.Name);
                builder.AppendLine("Size: " + fileItem.ContentFileSize);
                builder.AppendLine("==============");
            }
        }
    
        // Display output in console.
        Console.Write(builder.ToString());
    }

##<a name="list-jobs-and-assets"></a>Liste Aufträge und Anlagen

Eine wichtige verwandte Aufgabe besteht darin, Liste Anlagen mit seine zugeordneten Arbeit an Media-Dienste. Im folgenden Codebeispiel wird gezeigt, wie in der Liste der einzelnen Objekte IJob und klicken Sie dann für jedes Projekt, über den Auftrag Eigenschaften angezeigt, alle Aufgaben im Zusammenhang mit, alle Eingabemethoden Bestand und alle Ausgabe Anlagen. Der Code in diesem Beispiel kann für zahlreiche andere Aufgaben hilfreich sein. Wenn Sie möchten die Ausgabe-Ressourcen über einen oder mehrere Codierung Einzelvorgänge Liste, die Sie zuvor ausgeführt haben, zeigt dieser Code beispielsweise zum Zugreifen auf der Ausgabe Anlagen. Wenn Sie einen Verweis auf eine Anlage Ausgabe haben, können Sie nach dem Herunterladen oder und URLs klicken Sie dann den Inhalt an andere Benutzer oder eine Anwendung vorführen. 

Weitere Informationen zu den Optionen für die Bereitstellung von Anlagen finden Sie unter [Anlagen mit dem Media Services SDK für .NET vorführen](media-services-deliver-streaming-content.md).

    // List all jobs on the server, and for each job, also list 
    // all tasks, all input assets, all output assets.

    static void ListJobsAndAssets()
    {
        string waitMessage = "Building the list. This may take a few "
            + "seconds to a few minutes depending on how many assets "
            + "you have."
            + Environment.NewLine + Environment.NewLine
            + "Please wait..."
            + Environment.NewLine;
        Console.Write(waitMessage);
    
        // Create a Stringbuilder to store the list that we build. 
        StringBuilder builder = new StringBuilder();
    
        foreach (IJob job in _context.Jobs)
        {
            // Display the collection of jobs on the server.
            builder.AppendLine("");
            builder.AppendLine("******JOB*******");
            builder.AppendLine("Job ID: " + job.Id);
            builder.AppendLine("Name: " + job.Name);
            builder.AppendLine("State: " + job.State);
            builder.AppendLine("Order: " + job.Priority);
            builder.AppendLine("==============");
    
    
            // For each job, display the associated tasks (a job  
            // has one or more tasks). 
            builder.AppendLine("******TASKS*******");
            foreach (ITask task in job.Tasks)
            {
                builder.AppendLine("Task Id: " + task.Id);
                builder.AppendLine("Name: " + task.Name);
                builder.AppendLine("Progress: " + task.Progress);
                builder.AppendLine("Configuration: " + task.Configuration);
                if (task.ErrorDetails != null)
                {
                    builder.AppendLine("Error: " + task.ErrorDetails);
                }
                builder.AppendLine("==============");
            }
    
            // For each job, display the list of input media assets.
            builder.AppendLine("******JOB INPUT MEDIA ASSETS*******");
            foreach (IAsset inputAsset in job.InputMediaAssets)
            {
    
                if (inputAsset != null)
                {
                    builder.AppendLine("Input Asset Id: " + inputAsset.Id);
                    builder.AppendLine("Name: " + inputAsset.Name);
                    builder.AppendLine("==============");
                }
            }
    
            // For each job, display the list of output media assets.
            builder.AppendLine("******JOB OUTPUT MEDIA ASSETS*******");
            foreach (IAsset theAsset in job.OutputMediaAssets)
            {
                if (theAsset != null)
                {
                    builder.AppendLine("Output Asset Id: " + theAsset.Id);
                    builder.AppendLine("Name: " + theAsset.Name);
                    builder.AppendLine("==============");
                }
            }
    
        }
    
        // Display output in console.
        Console.Write(builder.ToString());
    }

##<a name="list-all-access-policies"></a>Liste aller Richtlinien

Media-Dienste können Sie eine Zugriffsrichtlinie auf eine Anlage oder deren Dateien definieren. Eine Zugriffsrichtlinie definiert die Berechtigungen für eine Datei oder eines Anlageguts (welche Art von Access, und die Dauer). Im Code Media-Dienste definieren Sie in der Regel eine Zugriffsrichtlinie, indem Sie ein Objekt IAccessPolicy erstellen und es dann mit einer vorhandenen Anlage zuordnen. Klicken Sie dann erstellen Sie ein ILocator-Objekt, dem Sie Direktzugriff Anlagen in Media-Dienste zur Verfügung stellen kann. Visual Studio-Projekt, das dieser Reihe Dokumentation begleitet enthält mehrere Codebeispiele, bei die das Erstellen und Zuweisen von Richtlinien und Locator zu Posten anzeigen.

Im folgenden Code wird gezeigt, wie in der Liste alle Richtlinien für den Zugriff auf dem Server, und zeigt den Typ der einzelnen zugeordneten Berechtigungen. Hilfreiche-Richtlinien angezeigt werden in der Liste alle ILocator Objekte auf dem Server, und klicken Sie dann für jedes Locator, Sie können mithilfe einer Liste der zugehörigen Zugriffsrichtlinie seine Eigenschaft AccessPolicy.

    static void ListAllPolicies()
    {
        foreach (IAccessPolicy policy in _context.AccessPolicies)
        {
            Console.WriteLine("");
            Console.WriteLine("Name:  " + policy.Name);
            Console.WriteLine("ID:  " + policy.Id);
            Console.WriteLine("Permissions: " + policy.Permissions);
            Console.WriteLine("==============");
    
        }
    }

##<a name="list-all-locators"></a>Liste aller Locator

Ein Locator ist eine URL, die einen direkten Pfad zum Posten, zusammen mit den Berechtigungen für die Ressource zuzugreifen, durch die Locator des zugeordneten Zugriffsrichtlinie definierten bereitstellt. Jede Anlage kann eine Auflistung von ILocator Objekte auf seine Eigenschaft Locator zugeordnet haben. Server-Kontext, weist ebenfalls eine Locator-Auflistung, die alle Locator enthält.

Im folgenden Code wird Listet alle Locator auf dem Server. Für jede Locator wird die Id für die verknüpfte Objekt und Access-Richtlinie. Es wird auch den Typ von Berechtigungen, das Ablaufdatum und den vollständigen Pfad für die Ressource angezeigt.

Beachten Sie, dass ein Locator-Pfad zu einer Anlage nur eine Basis-URL für die Ressource ist. Zum Erstellen eines direkten Pfads auf einzelne Dateien, die ein Benutzer oder eine Anwendung zu navigieren kann, muss Code den bestimmten Dateipfad den Pfad Locator hinzufügen. Weitere Informationen hierzu finden im Thema [Anlagen mit dem Media Services SDK für .NET vorführen](media-services-deliver-streaming-content.md).

    static void ListAllLocators()
    {
        foreach (ILocator locator in _context.Locators)
        {
            Console.WriteLine("***********");
            Console.WriteLine("Locator Id: " + locator.Id);
            Console.WriteLine("Locator asset Id: " + locator.AssetId);
            Console.WriteLine("Locator access policy Id: " + locator.AccessPolicyId);
            Console.WriteLine("Access policy permissions: " + locator.AccessPolicy.Permissions);
            Console.WriteLine("Locator expiration: " + locator.ExpirationDateTime);
            // The locator path is the base or parent path (with included permissions) to access  
            // the media content of an asset. To create a full URL to a specific media file, take 
            // the locator path and then append a file name and info as needed.  
            Console.WriteLine("Locator base path: " + locator.Path);
            Console.WriteLine("");
        }
    }

## <a name="enumerating-through-large-collections-of-entities"></a>Auflisten von großen Sammlungen von Personen

Bei der Abfrage Elemente besteht maximal 1000 Elemente gleichzeitig zurückgegeben werden, da Öffentliche REST Version 2 Abfrageergebnisse auf 1000 Ergebnisse begrenzt. Sie müssen überspringen und übernehmen, verwenden beim Durchlaufen großer Sammlungen mit Personen. 
    
Die folgende Funktion durchläuft alle Einzelvorgänge in das bereitgestellte Medien Services-Konto aus. Media-Dienste gibt 1000 Aufträge Aufträge Websitesammlung. Die Funktion nutzt überspringen und übernehmen, um sicherzustellen, dass alle Aufträge werden (für den Fall, dass Sie Ihr Konto mehr als 1000 Einzelvorgänge haben) aufgelistet.
    
    static void ProcessJobs()
    {
        try
        {
    
            int skipSize = 0;
            int batchSize = 1000;
            int currentBatch = 0;
    
            while (true)
            {
                // Loop through all Jobs (1000 at a time) in the Media Services account
                IQueryable _jobsCollectionQuery = _context.Jobs.Skip(skipSize).Take(batchSize);
                foreach (IJob job in _jobsCollectionQuery)
                {
                    currentBatch++;
                    Console.WriteLine("Processing Job Id:" + job.Id);
                }
    
                if (currentBatch == batchSize)
                {
                    skipSize += batchSize;
                    currentBatch = 0;
                }
                else
                {
                    break;
                }
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine(ex.Message);
        }
    }

##<a name="delete-an-asset"></a>Löschen einer Anlageguts

Im folgende Beispiel wird eine Anlage gelöscht.

    static void DeleteAsset( IAsset asset)
    {
        // delete the asset
        asset.Delete();
    
        // Verify asset deletion
        if (GetAsset(asset.Id) == null)
            Console.WriteLine("Deleted the Asset");
    
    }

##<a name="delete-a-job"></a>Löschen eines Auftrags

Zum Löschen eines Auftrags müssen Sie den Status des Projekts einchecken, wie in der Eigenschaft Zustand angegeben. Einzelvorgänge, die abgeschlossen oder abgebrochen werden können gelöscht werden, während Aufträge, die in anderen Staaten, wie in der Warteschlange, geplanten oder Verarbeitung sind, zuerst storniert werden müssen, und klicken Sie dann sie gelöscht werden können.
Im folgenden Codebeispiel wird eine Methode zum Löschen eines Auftrags nach Position Staaten aktivieren, und klicken Sie dann auf Löschen, wenn der Status abgeschlossen oder abgebrochen wird. Dieser Code abhängig von dem vorherigen Abschnitt in diesem Thema zum Abrufen eines Verweises auf ein Projekt: Abrufen eines Verweises Position.

    static void DeleteJob(string jobId)
    {
        bool jobDeleted = false;
    
        while (!jobDeleted)
        {
            // Get an updated job reference.  
            IJob job = GetJob(jobId);
    
            // Check and handle various possible job states. You can 
            // only delete a job whose state is Finished, Error, or Canceled.   
            // You can cancel jobs that are Queued, Scheduled, or Processing,  
            // and then delete after they are canceled.
            switch (job.State)
            {
                case JobState.Finished:
                case JobState.Canceled:
                case JobState.Error:
                    // Job errors should already be logged by polling or event 
                    // handling methods such as CheckJobProgress or StateChanged.
                    // You can also call job.DeleteAsync to do async deletes.
                    job.Delete();
                    Console.WriteLine("Job has been deleted.");
                    jobDeleted = true;
                    break;
                case JobState.Canceling:
                    Console.WriteLine("Job is cancelling and will be deleted "
                        + "when finished.");
                    Console.WriteLine("Wait while job finishes canceling...");
                    Thread.Sleep(5000);
                    break;
                case JobState.Queued:
                case JobState.Scheduled:
                case JobState.Processing:
                    job.Cancel();
                    Console.WriteLine("Job is scheduled or processing and will "
                        + "be deleted.");
                    break;
                default:
                    break;
            }
    
        }
    }


##<a name="delete-an-access-policy"></a>Löschen einer Zugriffsrichtlinie

Im folgenden Code wird gezeigt, wie einen Verweis auf eine Zugriffsrichtlinie basierend auf einer Richtlinie-Id abrufen und dann die Richtlinie zu löschen.

    static void DeleteAccessPolicy(string existingPolicyId)
    {
        // To delete a specific access policy, get a reference to the policy.  
        // based on the policy Id passed to the method.
        var policyInstance =
                from p in _context.AccessPolicies
                where p.Id == existingPolicyId
                select p;
        IAccessPolicy policy = policyInstance.FirstOrDefault();
    
        policy.Delete();
    
    }
    


##<a name="media-services-learning-paths"></a>Media-Dienste Learning Wege

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

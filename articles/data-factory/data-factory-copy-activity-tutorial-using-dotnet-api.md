<properties 
    pageTitle="Lernprogramm: Erstellen einer Verkaufspipeline mit Kopieren Aktivität mit .NET API | Microsoft Azure" 
    description="In diesem Lernprogramm erstellen Sie eine Verkaufspipeline Azure Data Factory mit einer Kopie Aktivität mithilfe von .NET-API ein." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="10/27/2016" 
    ms.author="spelluru"/>

# <a name="tutorial-create-a-pipeline-with-copy-activity-using-net-api"></a>Lernprogramm: Erstellen einer Verkaufspipeline mit Kopieren Aktivität mit .NET-API
> [AZURE.SELECTOR]
- [Übersicht und erforderliche Komponenten](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Assistent zum Kopieren von](data-factory-copy-data-wizard-tutorial.md)
- [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [Azure Ressourcenmanager Vorlage](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
- [REST-API](data-factory-copy-activity-tutorial-using-rest-api.md)
- [.NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md)


In diesem Lernprogramm erfahren Sie, wie erstellen und eine Azure-Daten Factory mithilfe der .NET API überwachen. Der Verkaufspipeline in den Daten Factory verwendet eine Kopie Aktivität zum Kopieren von Daten aus Azure BLOB-Speicher mit Azure SQL-Datenbank.

Die Kopie Aktivität führt das Verschieben von Daten in Azure Data Factory. Die Aktivität wird von einer global verfügbaren Service bereitgestellt, die Daten zwischen verschiedenen Datenspeicher sicheren, zuverlässigen und skalierbare So kopieren können. [Aktivitäten zum Verschieben von Daten](data-factory-data-movement-activities.md) finden Sie im Artikel für Details zur Aktivität kopieren.   

> [AZURE.NOTE] 
> In diesem Artikel befasst sich nicht auf alle Daten Factory .NET API aus. Details zu den Daten Factory .NET SDK finden Sie unter [Daten Factory .NET-API-Referenz](https://msdn.microsoft.com/library/mt415893.aspx) . 

## <a name="prerequisites"></a>Erforderliche Komponenten
- Durchgehen Sie [Lernprogramm Übersicht und erforderlichen Komponenten](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) erhalten Sie einen Überblick des Lernprogramms, und führen Sie die Schritte **Voraussetzung** . 
- Visual Studio 2012 oder 2013 oder 2015
- [Azure.NET SDK](http://azure.microsoft.com/downloads/) herunterladen und installieren
- Azure PowerShell. Anweisungen Sie [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) Artikel Azure PowerShell auf Ihrem Computer installieren. Verwenden Sie Azure PowerShell, um eine Azure Active Directory-Anwendung zu erstellen.

### <a name="create-an-application-in-azure-active-directory"></a>Erstellen Sie eine Anwendung in Azure Active Directory
Erstellen einer Azure Active Directory-Anwendung, ein Dienst Tilgungsanteile für die Anwendung erstellen und die **Daten Factory** Teilnehmerrolle zuweisen.  

1. Starten Sie **PowerShell**. 
1. Führen Sie den folgenden Befehl aus, und geben Sie den Benutzernamen und das Kennwort für die Anmeldung bei der Azure-Portal.
    
        Login-AzureRmAccount   
2. Führen Sie den folgenden Befehl zum Anzeigen aller Abonnements für dieses Konto ein.

        Get-AzureRmSubscription 
3. Führen Sie den folgenden Befehl aus, um das Abonnement auszuwählen, dem Sie mit arbeiten möchten. Ersetzen Sie ** &lt;NameOfAzureSubscription** &gt; durch den Namen Ihres Abonnements Azure. 

        Get-AzureRmSubscription -SubscriptionName <NameOfAzureSubscription> | Set-AzureRmContext

    > [AZURE.IMPORTANT] Beachten Sie von der Ausgabe dieser Befehl unten **SubscriptionId** und **TenantId** . 
4. Erstellen einer Azure Ressourcengruppe mit dem Namen **ADFTutorialResourceGroup** , indem Sie den folgenden Befehl ausführen, in der PowerShell.  

        New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"

    Wenn die Ressourcengruppe bereits vorhanden ist, geben Sie, ob es (Y) aktualisieren oder als (N) beibehalten. 

    Wenn Sie eine andere Ressourcengruppe verwenden, müssen Sie den Namen der Ressourcengruppe anstelle von ADFTutorialResourceGroup in diesem Lernprogramm verwenden.
5. Erstellen Sie eine Anwendung Azure Active Directory. 

        $azureAdApplication = New-AzureRmADApplication -DisplayName "ADFCopyTutotiralApp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.adfcopytutorialapp.org/example" -Password "Pass@word1"

    Wenn Sie die folgende Fehlermeldung erhalten, geben Sie einen anderen URL ein, und führen Sie den Befehl erneut aus. 

        Another object with the same value for property identifierUris already exists.

6. Erstellen Sie die Anzeige Dienst Tilgungsanteile. 

        New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

7. Dienst Tilgungsanteile **Daten Factory Mitwirkender** Rolle hinzufügen. 

        New-AzureRmRoleAssignment -RoleDefinitionName "Data Factory Contributor" -ServicePrincipalName $azureAdApplication.ApplicationId.Guid

8. Abrufen der Anwendung-ID.

        $azureAdApplication

    Notieren Sie die Anwendung-ID (**p** aus der Ausgabe).

Sollte stehen Ihnen folgende vier Werte aus der folgenden Schritte aus: 

- Mandanten-ID
- Abonnement-ID
- ID der Anwendung 
- Kennwort (in den ersten Befehl angegeben)   

## <a name="walkthrough"></a>Exemplarische Vorgehensweise
1. Verwenden Visual Studio 2012/2013/2015, erstellen Sie eine c# .NET.
    1. Starten Sie **Visual Studio** 2012/2013/2015.
    2. Klicken Sie auf **Datei**, zeigen Sie auf **neu**, und klicken Sie auf **Projekt**.
    3. Erweitern Sie **Vorlagen**, und wählen Sie **C#**. In dieser Anleitung erfahren Sie Sie können c# können jeder
    4. Wählen Sie aus der Liste mit Projekttypen auf der rechten Seite **Console-Anwendung** aus.
    5. Geben Sie **DataFactoryAPITestApp** für den Namen ein.
    6. Wählen Sie **C:\ADFGetStarted** für den Speicherort aus.
    7. Klicken Sie auf **OK** , um das Projekt zu erstellen.
2. Klicken Sie auf **Extras**, zeigen Sie auf **Nuget-Paket-Manager**, und klicken Sie auf **Paket-Manager-Konsole**.
3.  Führen Sie der **Paket-Manager-Konsole**die folgenden Schritte aus: 
    1.  Führen Sie den folgenden Befehl aus, um Daten Factory-Paket zu installieren:`Install-Package Microsoft.Azure.Management.DataFactories`       
    2.  Führen Sie den folgenden Befehl Azure-Active Directory-Paket installieren (Sie verwenden Active Directory-API im Code):`Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.19.208020213`
6. Fügen Sie den folgenden **AppSetttings** Abschnitt, um **die App** . Diese Einstellungen werden von der Methode Helper verwendet: **GetAuthorizationHeader**. 

    Ersetzen von Werten für ** &lt;-ID für&gt;**, ** &lt;Kennwort&gt;**, ** &lt;Abonnement-ID&gt;**, und ** &lt;Mandanten ID&gt; ** mit Ihren eigenen Werten. 

        <?xml version="1.0" encoding="utf-8" ?>
        <configuration>
            <startup> 
                <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2" />
            </startup>
            <appSettings>
                <add key="ActiveDirectoryEndpoint" value="https://login.windows.net/" />
                <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
                <add key="WindowsManagementUri" value="https://management.core.windows.net/" />

                <add key="ApplicationId" value="your application ID" />
                <add key="Password" value="Password you used while creating the AAD application" />
                <add key="SubscriptionId" value= "Subscription ID" />
                <add key="ActiveDirectoryTenantId" value="Tenant ID" />
            </appSettings>
        </configuration>
6. Fügen Sie die folgenden Aussagen zur **Verwendung** mit der Quelldatei (Program.cs) im Projekt hinzu.

        using System.Threading;
        using System.Configuration;
        using System.Collections.ObjectModel;
        
        using Microsoft.Azure.Management.DataFactories;
        using Microsoft.Azure.Management.DataFactories.Models;
        using Microsoft.Azure.Management.DataFactories.Common.Models;
        
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Azure;
6. Fügen Sie den folgenden Code, der eine Instanz der **DataPipelineManagementClient** -Klasse zur Methode **Main** erstellt wird. Verwenden Sie dieses Objekt, um eine Factory Daten, eine verknüpfte Service, Eingabe- und Datasets und eine Verkaufspipeline erstellen. Sie verwenden Sie dieses Objekt um Segmente eines Datasets zur Laufzeit zu überwachen.    

            // create data factory management client
            string resourceGroupName = "ADFTutorialResourceGroup";
            string dataFactoryName = "APITutorialFactory";

            TokenCloudCredentials aadTokenCredentials =
                new TokenCloudCredentials(
                    ConfigurationManager.AppSettings["SubscriptionId"],
                    GetAuthorizationHeader());

            Uri resourceManagerUri = new Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

            DataFactoryManagementClient client = new DataFactoryManagementClient(aadTokenCredentials, resourceManagerUri);

    > [AZURE.IMPORTANT] 
    > Ersetzen Sie den Wert von **ResourceGroupName** mit dem Namen der Ressourcengruppe Azure ein. 
    > 
    > Aktualisieren Sie die Namen der Daten Factory (**DataFactoryName**) eindeutig sein. Name der Daten Factory muss global eindeutig sein. Finden Sie unter [Data Factory - Regeln zur Benennung von](data-factory-naming-rules.md) Thema Benennungskonventionen für Daten Factory Elemente. 

7. Fügen Sie den folgenden Code, der eine **Fabrik Daten** zur Methode **Main** erstellt wird.

            // create a data factory
            Console.WriteLine("Creating a data factory");
            client.DataFactories.CreateOrUpdate(resourceGroupName,
                new DataFactoryCreateOrUpdateParameters()
                {
                    DataFactory = new DataFactory()
                    {
                        Name = dataFactoryName,
                        Location = "westus",
                        Properties = new DataFactoryProperties() { }
                    }
                }
            );

8. Fügen Sie den folgenden Code, der eine **Azure-Speicher verknüpft Dienst** zur Methode **Main** erstellt wird. 

    > [AZURE.IMPORTANT] Ersetzen Sie **Storageaccountname** und **Accountkey** mit Namen und den Schlüssel Ihres Kontos Azure-Speicher ein. 

            // create a linked service for input data store: Azure Storage
            Console.WriteLine("Creating Azure Storage linked service");
            client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new LinkedServiceCreateOrUpdateParameters()
                {
                    LinkedService = new LinkedService()
                    {
                        Name = "AzureStorageLinkedService",
                        Properties = new LinkedServiceProperties
                        (
                            new AzureStorageLinkedService("DefaultEndpointsProtocol=https;AccountName=<storageaccountname>;AccountKey=<accountkey>")
                        )
                    }
                }
            );
9. Fügen Sie den folgenden Code, der eine **SQL Azure verknüpft Dienst** zur Methode **Main** erstellt wird.
 
    > [AZURE.IMPORTANT] Ersetzen Sie **Servername**, **Datenbankname**, **Benutzername**und **Kennwort** mit den Namen der SQL Azure-Server, Datenbank, Benutzername und Kennwort ein.  

            // create a linked service for output data store: Azure SQL Database
            Console.WriteLine("Creating Azure SQL Database linked service");
            client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new LinkedServiceCreateOrUpdateParameters()
                {
                    LinkedService = new LinkedService()
                    {
                        Name = "AzureSqlLinkedService",
                        Properties = new LinkedServiceProperties
                        (
                            new AzureSqlDatabaseLinkedService("Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>;Password=<password>;Integrated Security=False;Encrypt=True;Connect Timeout=30")
                        )
                    }
                }
            );
10. Fügen Sie den folgenden Code, der **Eingabe- und Datasets** zur Methode **Main** erstellt wird. 

            // create input and output datasets
            Console.WriteLine("Creating input and output datasets");
            string Dataset_Source = "DatasetBlobSource";
            string Dataset_Destination = "DatasetAzureSqlDestination";

            Console.WriteLine("Creating input dataset of type: Azure Blob");
            client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,


            new DatasetCreateOrUpdateParameters()
            {
                Dataset = new Dataset()
                {
                    Name = Dataset_Source,
                    Properties = new DatasetProperties()
                    {
                        Structure = new List<DataElement>()
                        {
                            new DataElement() { Name = "FirstName", Type = "String" },
                            new DataElement() { Name = "LastName", Type = "String" }
                        },
                        LinkedServiceName = "AzureStorageLinkedService",
                        TypeProperties = new AzureBlobDataset()
                        {
                            FolderPath = "adftutorial/",
                            FileName = "emp.txt"
                        },
                        External = true,
                        Availability = new Availability()
                        {
                            Frequency = SchedulePeriod.Hour,
                            Interval = 1,
                        },

                        Policy = new Policy()
                        {
                            Validation = new ValidationPolicy()
                            {
                                MinimumRows = 1
                            }
                        }
                    }
                }
            });


            Console.WriteLine("Creating output dataset of type: Azure SQL");
            client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new DatasetCreateOrUpdateParameters()
                {
                    Dataset = new Dataset()
                    {
                        Name = Dataset_Destination,
                        Properties = new DatasetProperties()
                        {
                            Structure = new List<DataElement>()
                            {
                                new DataElement() { Name = "FirstName", Type = "String" },
                                new DataElement() { Name = "LastName", Type = "String" }
                            },
                            LinkedServiceName = "AzureSqlLinkedService",
                            TypeProperties = new AzureSqlTableDataset()
                            {
                                TableName = "emp"
                            },

                            Availability = new Availability()
                            {
                                Frequency = SchedulePeriod.Hour,
                                Interval = 1,
                            },
                        }
                    }
                });

11. Fügen Sie den folgenden Code zur Methode **Main** dieser **erstellt und aktiviert eine Verkaufspipeline** . Diese Verkaufspipeline weist eine **CopyActivity** , die **BlobSource** als Quelle akzeptiert und **BlobSink** als ein Empfänger.

            // create a pipeline
            Console.WriteLine("Creating a pipeline");
            DateTime PipelineActivePeriodStartTime = new DateTime(2016, 8, 9, 0, 0, 0, 0, DateTimeKind.Utc);
            DateTime PipelineActivePeriodEndTime = PipelineActivePeriodStartTime.AddMinutes(60);
            string PipelineName = "ADFTutorialPipeline";

            client.Pipelines.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new PipelineCreateOrUpdateParameters()
                {
                    Pipeline = new Pipeline()
                    {
                        Name = PipelineName,
                        Properties = new PipelineProperties()
                        {
                            Description = "Demo Pipeline for data transfer between blobs",

                    // Initial value for pipeline's active period. With this, you won't need to set slice status
                    Start = PipelineActivePeriodStartTime,
                            End = PipelineActivePeriodEndTime,

                            Activities = new List<Activity>()
                            {
                                new Activity()
                                {
                                    Name = "BlobToAzureSql",
                                    Inputs = new List<ActivityInput>()
                                    {
                                        new ActivityInput() {
                                            Name = Dataset_Source
                                        }
                                    },
                                    Outputs = new List<ActivityOutput>()
                                    {
                                        new ActivityOutput()
                                        {
                                            Name = Dataset_Destination
                                        }
                                    },
                                    TypeProperties = new CopyActivity()
                                    {
                                        Source = new BlobSource(),
                                        Sink = new BlobSink()
                                        {
                                            WriteBatchSize = 10000,
                                            WriteBatchTimeout = TimeSpan.FromMinutes(10)
                                        }
                                    }
                                }
                            },
                        }
                    }
                }); 

12. Fügen Sie den folgenden Code für **Main** -Methode, um den Status eines Segments des Datasets Ausgabe Daten abzurufen. Es ist nur in diesem Beispiel erwartet Segments aus.   
 
            // Pulling status within a timeout threshold
            DateTime start = DateTime.Now;
            bool done = false;

            while (DateTime.Now - start < TimeSpan.FromMinutes(5) && !done)
            {
                Console.WriteLine("Pulling the slice status");
                // wait before the next status check
                Thread.Sleep(1000 * 12);

                var datalistResponse = client.DataSlices.List(resourceGroupName, dataFactoryName, Dataset_Destination,
                    new DataSliceListParameters()
                    {
                        DataSliceRangeStartTime = PipelineActivePeriodStartTime.ConvertToISO8601DateTimeString(),
                        DataSliceRangeEndTime = PipelineActivePeriodEndTime.ConvertToISO8601DateTimeString()
                    });

                foreach (DataSlice slice in datalistResponse.DataSlices)
                {
                    if (slice.State == DataSliceState.Failed || slice.State == DataSliceState.Ready)
                    {
                        Console.WriteLine("Slice execution is done with status: {0}", slice.State);
                        done = true;
                        break;
                    }
                    else
                    {
                        Console.WriteLine("Slice status is: {0}", slice.State);
                    }
                }
            }

14. Fügen Sie den folgenden Code ein, um die Details für ein Segment Daten zur Methode **Main** auszuführen abrufen.

            Console.WriteLine("Getting run details of a data slice");

            // give it a few minutes for the output slice to be ready
            Console.WriteLine("\nGive it a few minutes for the output slice to be ready and press any key.");
            Console.ReadKey();

            var datasliceRunListResponse = client.DataSliceRuns.List(
                    resourceGroupName,
                    dataFactoryName,
                    Dataset_Destination,
                    new DataSliceRunListParameters()
                    {
                        DataSliceStartTime = PipelineActivePeriodStartTime.ConvertToISO8601DateTimeString()
                    }
                );

            foreach (DataSliceRun run in datasliceRunListResponse.DataSliceRuns)
            {
                Console.WriteLine("Status: \t\t{0}", run.Status);
                Console.WriteLine("DataSliceStart: \t{0}", run.DataSliceStart);
                Console.WriteLine("DataSliceEnd: \t\t{0}", run.DataSliceEnd);
                Console.WriteLine("ActivityId: \t\t{0}", run.ActivityName);
                Console.WriteLine("ProcessingStartTime: \t{0}", run.ProcessingStartTime);
                Console.WriteLine("ProcessingEndTime: \t{0}", run.ProcessingEndTime);
                Console.WriteLine("ErrorMessage: \t{0}", run.ErrorMessage);
            }

            Console.WriteLine("\nPress any key to exit.");
            Console.ReadKey();

13. Fügen Sie die folgende Helper-Methode, die von der **Main** -Methode der Klasse **Programm** verwendet.  
 
        public static string GetAuthorizationHeader()
        {
            AuthenticationResult result = null;
            var thread = new Thread(() =>
            {
                try
                {
                    var context = new AuthenticationContext(ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] + ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);

                    ClientCredential credential = new ClientCredential(ConfigurationManager.AppSettings["ApplicationId"], ConfigurationManager.AppSettings["Password"]);
                    result = context.AcquireToken(resource: ConfigurationManager.AppSettings["WindowsManagementUri"], clientCredential: credential);
                }
                catch (Exception threadEx)
                {
                    Console.WriteLine(threadEx.Message);
                }
            });

            thread.SetApartmentState(ApartmentState.STA);
            thread.Name = "AcquireTokenThread";
            thread.Start();
            thread.Join();

            if (result != null)
            {
                return result.AccessToken;
            }

            throw new InvalidOperationException("Failed to acquire token");
        }  


15. Explorer-Lösung erweitern Sie das Projekt (**DataFactoryAPITestApp**), mit der rechten Maustaste **Verweise**, und klicken Sie auf **Verweis hinzufügen**. Wählen Sie das Kontrollkästchen für "**System.Configuration**" Assembly aus, und klicken Sie auf **OK**. 
16. Erstellen Sie die Console-Anwendung. Klicken Sie auf **Erstellen** , klicken Sie auf das Menü, und klicken Sie auf die **Lösung erstellen**. 
16. Bestätigen Sie, dass mindestens eine Datei im Container **Adftutorial** in Ihrem Azure Blob Storage. Wenn dies nicht der Fall ist, **Emp.txt** -Datei in Editor mit dem folgenden Inhalt erstellen und auf den Container Adftutorial hochladen.

        John, Doe
        Jane, Doe
     
17. Führen Sie das Beispiel, indem Sie auf **Debuggen** -> im Menü**Debuggen starten** . Wenn Sie die **erste ausführen Details eines Segments Daten**angezeigt wird, warten Sie einige Minuten, und drücken Sie die **EINGABETASTE**. 
18. Verwenden des Azure-Portals um zu überprüfen, ob die Daten Factory **APITutorialFactory** mit der folgenden Elemente erstellt wurde: 
    - Dienst verknüpft: **LinkedService_AzureStorage** 
    - DataSet: **DatasetBlobSource** und **DatasetBlobDestination**.
    - Verkaufspipeline: **PipelineBlobSample** 
18. Stellen Sie sicher, dass die zwei Mitarbeiterdatensätze in der Tabelle "**emp**" in der angegebenen SQL Azure-Datenbank erstellt werden.

## <a name="next-steps"></a>Nächste Schritte

- Lesen Sie die detaillierte Informationen zur Aktivität kopieren bereitstellt, die im Lernprogramm verwendeten [Daten Bewegung Aktivitäten](data-factory-data-movement-activities.md) Artikel.
- Details zu den Daten Factory .NET SDK finden Sie unter [Daten Factory .NET-API-Referenz](https://msdn.microsoft.com/library/mt415893.aspx) . In diesem Artikel befasst sich nicht auf alle Daten Factory .NET API aus. 

 

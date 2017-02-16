<properties 
    pageTitle="Erstellen, überwachen und Verwalten von Factory Azure-Daten mithilfe von Data Factory SDK | Microsoft Azure" 
    description="Erfahren Sie, wie programmgesteuert erstellen, überwachen und Verwalten von Factory Azure-Daten mithilfe von Data Factory SDK." 
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
    ms.topic="article" 
    ms.date="09/14/2016" 
    ms.author="spelluru"/>

# <a name="create-monitor-and-manage-azure-data-factories-using-data-factory-net-sdk"></a>Erstellen, überwachen und Verwalten von Azure Daten Factory mit Daten Factory .NET SDK
## <a name="overview"></a>(Übersicht)
Sie können erstellen, überwachen und Verwalten von Azure Daten Factory programmgesteuert mit Daten Factory .NET SDK. Dieser Artikel enthält eine exemplarische Vorgehensweise, die Sie folgen können, um eine Stichprobe .NET Console-Anwendung zu erstellen, die erstellt und überwacht eine Factory Daten. Details zu den Daten Factory .NET SDK finden Sie unter [Daten Factory Class Bibliothek verweisen](https://msdn.microsoft.com/library/mt415893.aspx) . 

## <a name="prerequisites"></a>Erforderliche Komponenten

- Visual Studio 2012 oder 2013 oder 2015
- Herunterladen und Installieren von [Azure.NET SDK](http://azure.microsoft.com/downloads/).
- Hinzufügen einer systemeigenen Clientanwendung zum Azure Active Directory. Finden Sie unter [Integration von Applications mit Azure Active Directory](../active-directory/active-directory-integrating-applications.md) für Schritte, die die Anwendung hinzufügen. Beachten Sie die **CLIENT-ID** und **URI UMLEITEN** auf der Seite **Konfigurieren** .
- Erhalten Sie Ihre Azure- **Abonnement-ID** und **Mandanten-ID**an. Weitere Informationen finden Sie in [erste Azure-Abonnement und Mandanten IDs](#get-azure-subscription-and-tenant-ids) . 
- Herunterladen und Installieren von NuGet-Paketen für Azure Data Factory. Hinweise finden Sie in der Anleitung erfahren.  

## <a name="walkthrough"></a>Exemplarische Vorgehensweise
1. Verwenden Visual Studio 2012 oder 2013, erstellen Sie eine c# .NET.
    1. Starten Sie **Visual Studio 2012/2013/2015**.
    2. Klicken Sie auf **Datei**, zeigen Sie auf **neu**, und klicken Sie auf **Projekt**.
    3. Erweitern Sie **Vorlagen**, und wählen Sie **C#**. In dieser Anleitung erfahren Sie Sie können c# können jeder
    4. Wählen Sie aus der Liste mit Projekttypen auf der rechten Seite **Console-Anwendung** aus.
    5. Geben Sie **DataFactoryAPITestApp** für den **Namen**ein.
    6. Wählen Sie **C:\ADFGetStarted** für den **Speicherort**aus.
    7. Klicken Sie auf **OK** , um das Projekt zu erstellen.
2. Klicken Sie auf **Extras**, zeigen Sie auf **NuGet-Paket-Manager**, und klicken Sie auf **Paket-Manager-Konsole**.
3.  Führen Sie die folgenden Befehle nacheinander, in der **Paket-Manager-Konsole**. 

            Install-Package Microsoft.Azure.Management.DataFactories
            Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.19.208020213
4. Fügen Sie den folgenden **AppSetttings** Abschnitt, um **die App** . Diese Konfigurationswerte werden von der Methode **GetAuthorizationHeader** verwendet. 

    > [AZURE.IMPORTANT] Ersetzen von Werten für **AdfClientId**, **RedirectUri**, **SubscriptionId**und **ActiveDirectoryTenantId** mit Ihren eigenen Werten.  
 
        <appSettings>
            <add key="ActiveDirectoryEndpoint" value="https://login.windows.net/" />
            <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
            <add key="WindowsManagementUri" value="https://management.core.windows.net/" />

            <!-- Replace the following values with your own -->
            <add key="AdfClientId" value="Your AAD application ID" />
            <add key="RedirectUri" value="Your AAD application's redirect URI" />
            <add key="SubscriptionId" value="your subscription ID" />
            <add key="ActiveDirectoryTenantId" value="your tenant ID" />
        </appSettings>
5. Fügen Sie die folgenden Aussagen zur **Verwendung** mit der Quelldatei (Program.cs) im Projekt hinzu.

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
        string resourceGroupName = "resourcegroupname";
        string dataFactoryName = "APITutorialFactorySP";

        TokenCloudCredentials aadTokenCredentials =
            new TokenCloudCredentials(
                ConfigurationManager.AppSettings["SubscriptionId"],
                GetAuthorizationHeader());

        Uri resourceManagerUri = new Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

        DataFactoryManagementClient client = new DataFactoryManagementClient(aadTokenCredentials, resourceManagerUri);

    > [AZURE.NOTE] Ersetzen Sie die **Resourcegroupname** mit dem Namen der Ressourcengruppe Azure ein. Sie können eine Ressourcengruppe mithilfe des Cmdlets [New-AzureResourceGroup](https://msdn.microsoft.com/library/Dn654594.aspx) erstellen.

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

8. Fügen Sie den folgenden Code, der eine **verknüpfte Dienst** zur Methode **Main** erstellt wird. 

    > [AZURE.NOTE] Verwenden Sie für die **ConnectionString** **Kontonamen** und **kontoschlüssel** Ihres Kontos Azure-Speicher. 

        // create a linked service
        Console.WriteLine("Creating a linked service");
        client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
            new LinkedServiceCreateOrUpdateParameters()
            {
                LinkedService = new LinkedService()
                {
                    Name = "LinkedService-AzureStorage",
                    Properties = new LinkedServiceProperties
                    (
                        new AzureStorageLinkedService("DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>")
                    )
                }
            }
        );
9. Fügen Sie den folgenden Code, der **Eingabe- und Datasets** zur Methode **Main** erstellt wird. 

    Auf den **Ordnerpfad** für die Eingabewerte Blob festgelegt ist **Adftutorial /** **Adftutorial** ist, in dem der Name des Containers in Ihrem Blob Storage. Wenn dieser Container in Ihrer Azure Blob-Speicher nicht vorhanden ist, erstellen Sie einen Container mit diesem Namen: **Adftutorial** und Hochladen einer Text-Datei in den Container.
    
    Auf den Ordnerpfad für die Ausgabe Blob festgelegt ist: **Adftutorial/Apifactoryoutput / {segmentieren}** , in dem **Segment** dynamisch berechnet wird basierend auf dem Wert des **SliceStart** (Start Datum und Uhrzeit der einzelnen Segmente.)  
 
        // create input and output datasets
        Console.WriteLine("Creating input and output datasets");
        string Dataset_Source = "DatasetBlobSource";
        string Dataset_Destination = "DatasetBlobDestination";

        client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,
            new DatasetCreateOrUpdateParameters()
            {
                Dataset = new Dataset()
                {
                    Name = Dataset_Source,
                    Properties = new DatasetProperties()
                    {
                        LinkedServiceName = "LinkedService-AzureStorage",
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

        client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,
            new DatasetCreateOrUpdateParameters()
            {
                Dataset = new Dataset()
                {
                    Name = Dataset_Destination,
                    Properties = new DatasetProperties()
                    {

                        LinkedServiceName = "LinkedService-AzureStorage",
                        TypeProperties = new AzureBlobDataset()
                        {
                            FolderPath = "adftutorial/apifactoryoutput/{Slice}",
                            PartitionedBy = new Collection<Partition>()
                            {
                                new Partition()
                                {
                                    Name = "Slice",
                                    Value = new DateTimePartitionValue()
                                    {
                                        Date = "SliceStart",
                                        Format = "yyyyMMdd-HH"
                                    }
                                }
                            }
                        },

                        Availability = new Availability()
                        {
                            Frequency = SchedulePeriod.Hour,
                            Interval = 1,
                        },
                    }
                }
            });

10. Fügen Sie den folgenden Code zur Methode **Main** dieser **erstellt und aktiviert eine Verkaufspipeline** . Diese Verkaufspipeline weist eine **CopyActivity** , die **BlobSource** als Quelle akzeptiert und **BlobSink** als ein Empfänger.

    Die Kopie Aktivität führt das Verschieben von Daten in Azure Data Factory. Die Aktivität wird von einer global verfügbaren Service bereitgestellt, die Daten zwischen verschiedenen Datenspeicher sicheren, zuverlässigen und skalierbare So kopieren können. [Aktivitäten zum Verschieben von Daten](data-factory-data-movement-activities.md) finden Sie im Artikel für Details zur Aktivität kopieren. 

            // create a pipeline
        Console.WriteLine("Creating a pipeline");
        DateTime PipelineActivePeriodStartTime = new DateTime(2014, 8, 9, 0, 0, 0, 0, DateTimeKind.Utc);
        DateTime PipelineActivePeriodEndTime = PipelineActivePeriodStartTime.AddMinutes(60);
        string PipelineName = "PipelineBlobSample";

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
                                Name = "BlobToBlob",
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

11. Fügen Sie die folgende Helper-Methode, die von der **Main** -Methode der Klasse **Programm** verwendet. Diese Methode wird ein Dialogfeld, das können Sie bereitstellen, **Benutzernamen** und **Ihr Kennwort ein** , mit denen Sie sich bei Azure-Portal anmelden. 
 
        public static string GetAuthorizationHeader()
        {
            AuthenticationResult result = null;
            var thread = new Thread(() =>
            {
                try
                {
                    var context = new AuthenticationContext(ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] + ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);

                    result = context.AcquireToken(
                        resource: ConfigurationManager.AppSettings["WindowsManagementUri"],
                        clientId: ConfigurationManager.AppSettings["AdfClientId"],
                        redirectUri: new Uri(ConfigurationManager.AppSettings["RedirectUri"]),
                        promptBehavior: PromptBehavior.Always);
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
 
13. Fügen Sie den folgenden Code für **Main** -Methode, um den Status eines Segments des Datasets Ausgabe Daten abzurufen. Es ist nur in diesem Beispiel erwartet Segments aus.   
 
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

13. **(optional)** Fügen Sie den folgenden Code ein, um die Details für ein Segment Daten zur Methode **Main** auszuführen abrufen.

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

14. Explorer-Lösung erweitern Sie das Projekt (**DataFactoryAPITestApp**), mit der rechten Maustaste **Verweise**, und klicken Sie auf **Verweis hinzufügen**. Für das Kontrollkästchen SELECT `System.Configuration` Assembly, und klicken Sie auf **OK**. 
15. Erstellen Sie die Console-Anwendung. Klicken Sie auf **Erstellen** , klicken Sie auf das Menü, und klicken Sie auf die **Lösung erstellen**. 
16. Bestätigen Sie, dass mindestens eine Datei im Container Adftutorial in Ihrem Azure Blob Storage. Wenn dies nicht der Fall ist, Emp.txt-Datei in Editor mit dem folgenden Inhalt erstellen und auf den Container Adftutorial hochladen.

        John, Doe
        Jane, Doe 
17. Führen Sie das Beispiel, indem Sie auf **Debuggen** -> im Menü**Debuggen starten** . Wenn Sie die **erste ausführen Details eines Segments Daten**angezeigt wird, warten Sie einige Minuten, und drücken Sie die **EINGABETASTE**. 
18. Verwenden des Azure-Portals um zu überprüfen, ob die Daten Factory **APITutorialFactory** mit der folgenden Elemente erstellt wurde: 
    - Dienst verknüpft: **LinkedService_AzureStorage** 
    - DataSet: **DatasetBlobSource** und **DatasetBlobDestination**.
    - Verkaufspipeline: **PipelineBlobSample** 
18. Stellen Sie sicher, dass eine Ausgabedatei in den Ordner **Apifactoryoutput** im Container **Adftutorial** erstellt wird.

## <a name="log-in-without-popup-dialog-box"></a>Melden Sie sich ohne Popup-Dialogfeld 
Beispiel-Code in die exemplarische Vorgehensweise wird ein Dialogfeld zur Eingabe von Anmeldeinformationen Azure gestartet. Wenn Sie ohne Verwendung eines Dialogfelds programmgesteuert anmelden müssen, finden Sie unter [Authentifizierung ein Dienst Tilgungsanteile Azure Ressourcenmanager](resource-group-authenticate-service-principal.md#authenticate-service-principal-with-certificate---powershell). 

> [AZURE.IMPORTANT] Hinzufügen einer Webanwendung Azure Active Directory und notieren Sie sich die Client-ID und Client geheim der Anwendung.  

### <a name="example"></a>Beispiel

Erstellen Sie GetAuthorizationHeaderNoPopup Methode.  

    public static string GetAuthorizationHeaderNoPopup()
    {
        var authority = new Uri(new Uri("https://login.windows.net"), ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);
        var context = new AuthenticationContext(authority.AbsoluteUri);
        var credential = new ClientCredential(ConfigurationManager.AppSettings["AdfClientId"], ConfigurationManager.AppSettings["AdfClientSecret"]);
        AuthenticationResult result = context.AcquireTokenAsync(ConfigurationManager.AppSettings["WindowsManagementUri"], credential).Result;
        if (result != null)
            return result.AccessToken;

        throw new InvalidOperationException("Failed to acquire token");
    }

Ersetzen Sie **GetAuthorizationHeader** Anruf mit einem Anruf, um **GetAuthorizationHeaderNoPopup** in der Funktion **Main** aus:  

        TokenCloudCredentials aadTokenCredentials =
            new TokenCloudCredentials(
            ConfigurationManager.AppSettings["SubscriptionId"],
            GetAuthorizationHeaderNoPopup());

Hier, wie Sie die Active Directory-Anwendung, Dienst Hauptbenutzer, erstellen und dann die Daten Factory Teilnehmerrolle zuweisen: 

1. Erstellen der AD-Anwendung. 

        $azureAdApplication = New-AzureRmADApplication -DisplayName "MyADAppForADF" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.myadappforadf.org/example" -Password "Pass@word1"

2. Erstellen Sie die Anzeige Dienst Tilgungsanteile. 

        New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

3. Dienst Tilgungsanteile Daten Factory Mitwirkender Rolle hinzufügen. 

        New-AzureRmRoleAssignment -RoleDefinitionName "Data Factory Contributor" -ServicePrincipalName $azureAdApplication.ApplicationId.Guid

4. Abrufen der Anwendung-ID.

        $azureAdApplication


Notieren Sie die Anwendung-ID und das Kennwort (Client geheim), und verwenden Sie diese in die exemplarische Vorgehensweise zu. 

## <a name="get-azure-subscription-and-tenant-ids"></a>Abrufen von Azure-Abonnement und Mandanten IDs
Wenn Sie nicht neueste Version von PowerShell auf Ihrem Computer installiert haben, führen Sie die Schritte [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) -Artikel zur Installation.

1. Starten Sie der PowerShell Azure und führen den folgenden Befehl aus
2. Führen Sie den folgenden Befehl aus, und geben Sie den Benutzernamen und das Kennwort für die Anmeldung bei der Azure-Portal.

        Login-AzureRmAccount

    Wenn Sie nur ein mit diesem Konto verknüpfte Azure-Abonnement haben, müssen Sie nicht die folgenden beiden Schritte ausführen.  
3. Führen Sie den folgenden Befehl zum Anzeigen aller Abonnements für dieses Konto ein.

        Get-AzureRmSubscription
4. Führen Sie den folgenden Befehl aus, um das Abonnement auszuwählen, dem Sie mit arbeiten möchten. Ersetzen Sie **NameOfAzureSubscription** durch den Namen Ihres Abonnements Azure ein.

        Get-AzureRmSubscription -SubscriptionName NameOfAzureSubscription | Set-AzureRmContext 

Beachten Sie die **SubscriptionId** und **TenantId** Werte.


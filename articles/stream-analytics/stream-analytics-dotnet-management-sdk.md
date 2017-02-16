<properties
    pageTitle=".NET SDK Verwaltungsoption Stream Analytics | Microsoft Azure"
    description="Erste Schritte mit Stream Analytics Management .NET SDK. Informationen zum Einrichten und Ausführen von Analytics Aufträge: Erstellen eines Projekts, Eingaben, Ausgaben und Transformationen."
    keywords=".NET SDK, Analytics-API"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>


# <a name="management-net-sdk-set-up-and-run-analytics-jobs-using-the-azure-stream-analytics-api-for-net"></a>Verwaltung von .NET SDK: Einrichten und Ausführen von Analytics Aufträge mit der Azure Stream Analytics-API für .NET

Erfahren Sie, wie eine ausführen Analytics Projekte mithilfe der Stream Analytics-API für .NET mit dem Management .NET SDK einrichten. Einrichten eines Projekts, Erstellen von Eingabe- und Quellen, Transformationen und Start- und Aufträge beenden. Die für Ihre Arbeit Analytics können Sie Daten aus Blob-Speicher oder ein Ereignis Hub streamen.

Finden Sie in der [Dokumentation zur Verwaltung für die Stream Analytics-API für .NET](https://msdn.microsoft.com/library/azure/dn889315.aspx).

Azure Stream Analytics ist eine vollständig verwaltete Dienst Tiefst-Wartezeit, hoch verfügbare, skalierbare und komplexe Ereignis Verarbeitung über streaming-Daten in der Cloud bereitstellen. Stream Analytics ermöglicht Benutzern das streaming Aufträge zum Analysieren von Datenstreams einrichten, und ermöglicht es ihnen, die in der Nähe in Echtzeit Analytics versorgen.  


## <a name="prerequisites"></a>Erforderliche Komponenten
Vorbemerkung in diesem Artikel müssen Sie Folgendes:

- Installieren Sie Visual Studio 2012 oder 2013.
- Herunterladen und Installieren von [Azure.NET SDK](https://azure.microsoft.com/downloads/).
- Erstellen Sie eine Ressourcengruppe Azure in Ihr Abonnement. So sieht ein Beispiel Azure PowerShell-Skript. Azure PowerShell Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md);  


        # Log in to your Azure account
        Add-AzureAccount

        # Select the Azure subscription you want to use to create the resource group
        Select-AzureSubscription -SubscriptionName <subscription name>

            # If Stream Analytics has not been registered to the subscription, remove the remark symbol (#) to run the Register-AzureRMProvider cmdlet to register the provider namespace
            #Register-AzureRMProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'

        # Create an Azure resource group
        New-AzureResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>


-   Richten Sie eine Eingabe Quell- und Ausgabe verwenden. Anweisungen finden Sie unter Weitere [Eingaben hinzufügen](stream-analytics-add-inputs.md) , um eine Stichprobe Eingabe einzurichten und [Hinzufügen Ausgaben](stream-analytics-add-outputs.md) für die Einrichtung einer Stichprobe Ausgabe.


## <a name="set-up-a-project"></a>Einrichten eines Projekts

Zum Erstellen ein Auftrags Analytics der Stream Analytics-API für .NET, richten Sie Ihr Projekt zuerst verwenden.

1. Erstellen Sie eine Visual Studio c#.
2. In der Paket-Manager-Konsole, führen Sie die folgenden Befehle, die NuGet-Pakete zu installieren. Die erste Azure Stream Analytics Management .NET SDK ist. Der zweite ist der Azure-Active Directory-Client, der für die Authentifizierung verwendet werden soll.

        Install-Package Microsoft.Azure.Management.StreamAnalytics
        Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory

4. Fügen Sie den folgenden **AppSettings** -Abschnitt auf der App aus:

        <appSettings>
          <!--CSM Prod related values-->
          <add key="ActiveDirectoryEndpoint" value="https://login.windows.net/" />
          <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
          <add key="WindowsManagementUri" value="https://management.core.windows.net/" />
          <add key="AsaClientId" value="1950a258-227b-4e31-a9cf-717495945fc2" />
          <add key="RedirectUri" value="urn:ietf:wg:oauth:2.0:oob" />
          <add key="SubscriptionId" value="YOUR AZURE SUBSCRIPTION" />
          <add key="ActiveDirectoryTenantId" value="YOU TENANT ID" />
        </appSettings>


    Ersetzen von Werten für **SubscriptionId** und **ActiveDirectoryTenantId** mit Ihrem Azure-Abonnement und Mandanten IDs. Sie können diese Werte durch Ausführen des folgenden Azure PowerShell-Cmdlets erhalten:

        Get-AzureAccount

5. Fügen Sie die folgenden Aussagen zur **Verwendung** mit der Quelldatei (Program.cs) im Projekt hinzu:

        using System;
        using System.Configuration;
        using System.Threading;
        using Microsoft.Azure;
        using Microsoft.Azure.Management.StreamAnalytics;
        using Microsoft.Azure.Management.StreamAnalytics.Models;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;

6.  Fügen Sie eine Helper Authentifizierungsmethode hinzu:

        public static string GetAuthorizationHeader()
        {
            AuthenticationResult result = null;
            var thread = new Thread(() =>
            {
                try
                {
                    var context = new AuthenticationContext(
                        ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] +
                        ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);

                    result = context.AcquireToken(
                        resource: ConfigurationManager.AppSettings["WindowsManagementUri"],
                        clientId: ConfigurationManager.AppSettings["AsaClientId"],
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


## <a name="create-a-stream-analytics-management-client"></a>Erstellen eines Streams Analytics Management-Clients

Ein Objekt **StreamAnalyticsManagementClient** können Sie den Auftrag und den Auftrag Komponenten, wie Eingabe, Ausgabe und Transformation verwalten.

Fügen Sie den folgenden Code an den Anfang der **Main** -Methode:

    string resourceGroupName = "<YOUR AZURE RESOURCE GROUP NAME>";
    string streamAnalyticsJobName = "<YOUR STREAM ANALYTICS JOB NAME>";
    string streamAnalyticsInputName = "<YOUR JOB INPUT NAME>";
    string streamAnalyticsOutputName = "<YOUR JOB OUTPUT NAME>";
    string streamAnalyticsTransformationName = "<YOUR JOB TRANSFORMATION NAME>";

    // Get authentication token
    TokenCloudCredentials aadTokenCredentials =
        new TokenCloudCredentials(
            ConfigurationManager.AppSettings["SubscriptionId"],
            GetAuthorizationHeader());

    // Create Stream Analytics management client
    StreamAnalyticsManagementClient client = new StreamAnalyticsManagementClient(aadTokenCredentials);

Wert der **ResourceGroupName** -Variablen sollten identisch mit den Namen der Ressourcengruppe Sie erstellt oder in den erforderlichen Schritten entnommen.

Zum Automatisieren von des Anmeldeinformationen Präsentation Aspekts der Position Erstellung finden Sie unter [Authentifizierung ein Dienst Tilgungsanteile Azure Ressourcenmanager](../resource-group-authenticate-service-principal.md).

Die restlichen Abschnitte in diesem Artikel wird davon ausgegangen, dass dieser Code am Anfang der Methode **Main** ist.

## <a name="create-a-stream-analytics-job"></a>Erstellen eines Auftrags für Stream Analytics

Der folgende Code erstellt einen Stream Analytics Auftrag unter der Ressourcengruppe, die Sie definiert haben. Sie werden eine Eingabe, Ausgabe und Transformation den Auftrag später hinzufügen.

    // Create a Stream Analytics job
    JobCreateOrUpdateParameters jobCreateParameters = new JobCreateOrUpdateParameters()
    {
        Job = new Job()
        {
            Name = streamAnalyticsJobName,
            Location = "<LOCATION>",
            Properties = new JobProperties()
            {
                EventsOutOfOrderPolicy = EventsOutOfOrderPolicy.Adjust,
                Sku = new Sku()
                {
                    Name = "Standard"
                }
            }
        }
    };

    JobCreateOrUpdateResponse jobCreateResponse = client.StreamingJobs.CreateOrUpdate(resourceGroupName, jobCreateParameters);


## <a name="create-a-stream-analytics-input-source"></a>Erstellen Sie eine Eingabe Stream Analytics-Quelle

Mit dem folgende Code erstellt eine Eingabe Stream Analytics-Quelle, mit dem Eingabewerte Quellentyp Blob und CSV-Serialisierung. Verwenden Sie zum Erstellen einer Quelle Eingabewerte Hub, **EventHubStreamInputDataSource** anstelle von **BlobStreamInputDataSource**ein. Auf ähnliche Weise können Sie die Serialisierungstyp der Eingabewerte Quelle anpassen.

    // Create a Stream Analytics input source
    InputCreateOrUpdateParameters jobInputCreateParameters = new InputCreateOrUpdateParameters()
    {
        Input = new Input()
        {
            Name = streamAnalyticsInputName,
            Properties = new StreamInputProperties()
            {
                Serialization = new CsvSerialization
                {
                    Properties = new CsvSerializationProperties
                    {
                        Encoding = "UTF8",
                        FieldDelimiter = ","
                    }
                },
                DataSource = new BlobStreamInputDataSource
                {
                    Properties = new BlobStreamInputDataSourceProperties
                    {
                        StorageAccounts = new StorageAccount[]
                        {
                            new StorageAccount()
                            {
                                AccountName = "<YOUR STORAGE ACCOUNT NAME>",
                                AccountKey = "<YOUR STORAGE ACCOUNT KEY>"
                            }
                        },
                        Container = "samples",
                        PathPattern = ""
                    }
                }
            }
        }
    };

    InputCreateOrUpdateResponse inputCreateResponse =
        client.Inputs.CreateOrUpdate(resourceGroupName, streamAnalyticsJobName, jobInputCreateParameters);

Eingabewerte Quellen, sind von Blob-Speicher oder ein Ereignis-Hub an einer bestimmten Position gebunden. Um die Eingabe derselben Quelle für unterschiedliche Stellen verwenden zu können, müssen Sie rufen die Methode erneut, und geben einen anderen Namen.


## <a name="test-a-stream-analytics-input-source"></a>Testen einer Eingabe-Stream Analytics-Quelle

Die Methode **TestConnection** überprüft, ob der Stream Analytics Auftrag Verbindung zu der Datenquelle sowie weitere Aspekte, die speziell für die Eingabewerte Quellentyp hergestellt ist. Beispielsweise wird in der Eingabewerte Blob-Quelle, die Sie in einem vorherigen Schritt erstellt haben, die Methode überprüfen Sie, ob der Speicher Kontonamen auch einen Schlüssel zum Verbinden mit dem Konto Speicher als auch überprüfen, dass der angegebene Container vorhanden ist verwendet werden können.

    // Test input source connection
    DataSourceTestConnectionResponse inputTestResponse =
        client.Inputs.TestConnection(resourceGroupName, streamAnalyticsJobName, streamAnalyticsInputName);

## <a name="create-a-stream-analytics-output-target"></a>Erstellen Sie ein Stream Analytics Ausgabeziel

Erstellen einer Ausgabeziel ist das Erstellen einer Eingabewerte Stream Analytics-Quelle sehr ähnlich. Wie Eingabewerte Quellen sind Ausgabe Ziele mit einer bestimmten Position verknüpft. Um das gleiche Ergebnis Ziel für unterschiedliche Stellen verwenden zu können, müssen Sie rufen die Methode erneut, und geben einen anderen Namen.

Der folgende Code erstellt ein Ausgabeziel (SQL Azure-Datenbank). Sie können den Zieldatentyp der Ausgabe und/oder Serialisierungstyp anpassen.

    // Create a Stream Analytics output target
    OutputCreateOrUpdateParameters jobOutputCreateParameters = new OutputCreateOrUpdateParameters()
    {
        Output = new Output()
        {
            Name = streamAnalyticsOutputName,
            Properties = new OutputProperties()
            {
                DataSource = new SqlAzureOutputDataSource()
                {
                    Properties = new SqlAzureOutputDataSourceProperties()
                    {
                        Server = "<YOUR DATABASE SERVER NAME>",
                        Database = "<YOUR DATABASE NAME>",
                        User = "<YOUR DATABASE LOGIN>",
                        Password = "<YOUR DATABASE LOGIN PASSWORD>",
                        Table = "<YOUR DATABASE TABLE NAME>"
                    }
                }
            }
        }
    };

    OutputCreateOrUpdateResponse outputCreateResponse =
        client.Outputs.CreateOrUpdate(resourceGroupName, streamAnalyticsJobName, jobOutputCreateParameters);

## <a name="test-a-stream-analytics-output-target"></a>Testen Sie ein Stream Analytics Ausgabeziel

Ein Stream Analytics Ausgabeziel enthält auch die **TestConnection** Methode zum Testen der Verbindungen.

    // Test output target connection
    DataSourceTestConnectionResponse outputTestResponse =
        client.Outputs.TestConnection(resourceGroupName, streamAnalyticsJobName, streamAnalyticsOutputName);

## <a name="create-a-stream-analytics-transformation"></a>Erstellen einer Transformation Stream Analytics

Der folgende Code erstellt eine Transformation Stream Analytics mit der Abfrage "Wählen Sie * aus der Eingabe" und gibt an, dass eine streaming Einheit für den Auftrag Stream Analytics zugewiesen werden. Weitere Informationen zum Anpassen von streaming Einheiten finden Sie unter [Skalieren Azure Stream Analytics Aufträge](stream-analytics-scale-jobs.md).


    // Create a Stream Analytics transformation
    TransformationCreateOrUpdateParameters transformationCreateParameters = new TransformationCreateOrUpdateParameters()
    {
        Transformation = new Transformation()
        {
            Name = streamAnalyticsTransformationName,
            Properties = new TransformationProperties()
            {
                StreamingUnits = 1,
                Query = "select * from Input"
            }
        }
    };

    var transformationCreateResp =
        client.Transformations.CreateOrUpdate(resourceGroupName, streamAnalyticsJobName, transformationCreateParameters);

Wie Eingabe und Ausgabe eines Transformationen ist auch verknüpft, die bestimmte Stream Analytics Auftrag, unter der Sie erstellt wurde.

## <a name="start-a-stream-analytics-job"></a>Starten eines Auftrags Stream Analytics
Nach dem Erstellen eines Auftrags Stream Analytics und deren Aussteuerungsanzeige, Output(s) und Transformation, können Sie den Auftrag, indem Sie die **Start** -Methode beginnen.

Im folgenden Beispiel Code Starts ein Auftrags Stream Analytics mit einer benutzerdefinierten Ausgabe Startzeit bis 12 Dezember 2012, 12:12:12 festlegen UTC:

    // Start a Stream Analytics job
    JobStartParameters jobStartParameters = new JobStartParameters
    {
        OutputStartMode = OutputStartMode.CustomTime,
        OutputStartTime = new DateTime(2012, 12, 12, 0, 0, 0, DateTimeKind.Utc)
    };

    LongRunningOperationResponse jobStartResponse = client.StreamingJobs.Start(resourceGroupName, streamAnalyticsJobName, jobStartParameters);



## <a name="stop-a-stream-analytics-job"></a>Beenden eines Auftrags Stream Analytics
Sie können einen laufenden Stream Analytics Auftrag beenden, durch Aufrufen der Methode **Beenden** .

    // Stop a Stream Analytics job
    LongRunningOperationResponse jobStopResponse = client.StreamingJobs.Stop(resourceGroupName, streamAnalyticsJobName);

## <a name="delete-a-stream-analytics-job"></a>Löschen eines Auftrags Stream Analytics
Die Methode **Löschen** wird den Auftrag als auch die zugrunde liegende untergeordnete Ressourcen, einschließlich Aussteuerungsanzeige, Output(s) und Transformation des Projekts zu löschen.

    // Delete a Stream Analytics job
    LongRunningOperationResponse jobDeleteResponse = client.StreamingJobs.Delete(resourceGroupName, streamAnalyticsJobName);


## <a name="get-support"></a>Anfordern von Unterstützung
Versuchen Sie für weitere Unterstützung zu erhalten unseren [Azure Stream Analytics-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).


## <a name="next-steps"></a>Nächste Schritte

Sie haben, lernen die Grundlagen der Verwendung einer .NET SDK zu erstellen und Ausführen von Analytics Aufträge. Weitere Informationen finden Sie hier:

- [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
- [Erste Schritte mit Azure Stream Analytics](stream-analytics-get-started.md)
- [Skalieren Sie Azure Stream Analytics Aufträge](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Management .NET SDK](https://msdn.microsoft.com/library/azure/dn889315.aspx).
- [Azure Stream Analytics Query Language Bezug](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST-API-Referenz](https://msdn.microsoft.com/library/azure/dn835031.aspx)


<!--Image references-->
[5]: ./media/markdown-template-for-new-articles/octocats.png
[6]: ./media/markdown-template-for-new-articles/pretty49.png
[7]: ./media/markdown-template-for-new-articles/channel-9.png


<!--Link references-->
[azure.blob.storage]: http://azure.microsoft.com/documentation/services/storage/
[azure.blob.storage.use]: http://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/

[azure.event.hubs]: http://azure.microsoft.com/services/event-hubs/
[azure.event.hubs.developer.guide]: http://msdn.microsoft.com/library/azure/dn789972.aspx

[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.forum]: http://go.microsoft.com/fwlink/?LinkId=512151

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.developer.guide]: stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301

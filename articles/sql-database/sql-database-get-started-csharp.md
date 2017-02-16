<properties
    pageTitle="Versuchen Sie SQL-Datenbank: Verwenden Sie c# zum Erstellen einer SQL­Datenbank | Microsoft Azure"
    description="Versuchen Sie SQL-Datenbank für die Entwicklung von apps für SQL und c#, und erstellen Sie eine SQL Azure-Datenbank mit c# mit der SQL-Datenbank-Bibliothek für .NET."
    keywords="Versuchen Sie es Sql, Sql-c#"   
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="csharp"
   ms.workload="data-management"
   ms.date="10/04/2016"
   ms.author="sstein"/>

# <a name="try-sql-database-use-c-to-create-a-sql-database-with-the-sql-database-library-for-net"></a>Versuchen Sie SQL-Datenbank: Verwenden Sie c# zum Erstellen einer SQL­Datenbank mit der SQL-Datenbank-Bibliothek für .NET


> [AZURE.SELECTOR]
- [Azure-portal](sql-database-get-started.md)
- [C#](sql-database-get-started-csharp.md)
- [PowerShell](sql-database-get-started-powershell.md)

Erfahren Sie, wie c# verwenden Sie zum Erstellen einer SQL Azure-Datenbank mit [Microsoft Azure SQL-Verwaltungsbibliothek für .NET](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql). Dieser Artikel beschreibt, wie Sie eine einzelne Datenbank mit SQL und c# erstellen. Flexible Datenbank Pools erstellen zu können, finden Sie unter [Erstellen einer Datenbank flexible Ressourcenpool](sql-database-elastic-pool-create-csharp.md).

Der Bibliothek Azure SQL-Datenbank Management für .NET bietet eine [Ressourcenmanager Azure](../azure-resource-manager/resource-group-overview.md)-basierte API, die die [REST-API für SQL Datenbank Ressourcenmanager-basierten](https://msdn.microsoft.com/library/azure/mt163571.aspx)umgebrochen wird.

>[AZURE.NOTE] Viele neue Funktionen der SQL-Datenbank werden nur unterstützt, wenn Sie das [Modell zur Bereitstellung von Azure Ressourcenmanager](../azure-resource-manager/resource-group-overview.md), verwenden, damit Sie immer die neuesten verwenden sollten **Azure SQL-Datenbank-Management-Bibliothek für .NET ([Dokumente](https://msdn.microsoft.com/library/azure/mt349017.aspx) | [NuGet-Paket](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql))**. Die ältere [Modell zur klassischen Bereitstellung basierende Bibliotheken](https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Sql) werden unterstützt nur, zur Abwärtskompatibilität, damit es empfiehlt sich, dass Sie die neueren Ressourcenmanager Grundlage Bibliotheken verwenden.

Um die Schritte in diesem Artikel ausführen zu können, benötigen Sie Folgendes:

- Ein Azure-Abonnement. Wenn Sie ein Abonnement Azure lediglich auf **Kostenloses Konto** oben auf dieser Seite und dann wieder zum Ende dieses Artikels.
- Visual Studio. Eine kostenlose Kopie von Visual Studio finden Sie auf der Seite [Visual Studio-Downloads](https://www.visualstudio.com/downloads/download-visual-studio-vs) .

>[AZURE.NOTE] In diesem Artikel wird eine neue, leere SQL-Datenbank erstellt. Ändern Sie die *CreateOrUpdateDatabase(...)* -Methode im folgenden Beispiel kopieren Datenbanken skalieren Datenbanken, erstellen Sie eine Datenbank in einem Ressourcenpool usw. ein. Weitere Informationen finden Sie unter [DatabaseCreateMode](https://msdn.microsoft.com/library/microsoft.azure.management.sql.models.databasecreatemode.aspx) und [DatabaseProperties](https://msdn.microsoft.com/library/microsoft.azure.management.sql.models.databaseproperties.aspx) Klassen.



## <a name="create-a-console-app-and-install-the-required-libraries"></a>Erstellen Sie eine Console-app, und installieren Sie die erforderlichen Bibliotheken

1. Starten Sie Visual Studio.
2. Klicken Sie auf **Datei** > **neue** > **Projekt**.
3. Erstellen einer C#- **Console-Anwendung** , und nennen Sie es: *SqlDbConsoleApp*


Zum Erstellen einer SQL-Datenbank mit c#, laden Sie die erforderlichen Management-Bibliotheken (mithilfe der [Verwaltungskonsole für Paket](http://docs.nuget.org/Consume/Package-Manager-Console)):

1. Klicken Sie auf **Extras** > **NuGet-Paket-Manager** > **Paket-Manager-Konsole**.
2. Typ `Install-Package Microsoft.Azure.Management.Sql –Pre` , die neuesten [Microsoft Azure SQL-Management-Bibliothek](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql)zu installieren.
3. Typ `Install-Package Microsoft.Azure.Management.ResourceManager –Pre` , [Microsoft Azure Ressourcenmanager Bibliothek](https://www.nuget.org/packages/Microsoft.Azure.Management.ResourceManager)zu installieren.
4. Typ `Install-Package Microsoft.Azure.Common.Authentication –Pre` , [Microsoft Azure allgemeine Authentifizierungsbibliothek](https://www.nuget.org/packages/Microsoft.Azure.Common.Authentication)zu installieren. 



> [AZURE.NOTE] Die Beispiele in diesem Artikel mit einer synchroner Form der einzelnen API Anforderung und bis zum Abschluss der übrigen Anrufen auf dem zugrunde liegenden Dienst blockieren. Asynchrone Methoden stehen zur Verfügung.


## <a name="create-a-sql-database-server-firewall-rule-and-sql-database---c-example"></a>Erstellen einer SQL-Datenbankserver, Firewall-Regel und SQL-Datenbank - C#-Beispiel

Im folgende Beispiel wird eine Ressourcengruppe, Server, Firewall-Regel und einer SQL-Datenbank erstellt. Finden Sie unter, [Erstellen Sie einen Dienst Hauptbenutzer Zugriff auf Ressourcen](#create-a-service-principal-to-access-resources) zum Abrufen der `_subscriptionId, _tenantId, _applicationId, and _applicationSecret` Variablen.

Ersetzen Sie den Inhalt von **"Program.cs"** mit den folgenden, und Aktualisieren der `{variables}` mit Ihren app-Werten (enthalten nicht die `{}`).


    using Microsoft.Azure;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.Azure.Management.Sql;
    using Microsoft.Azure.Management.Sql.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System;
    
    namespace SqlDbConsoleApp
    {
    class Program
       {
        // For details about these four (4) values, see
        // https://azure.microsoft.com/documentation/articles/resource-group-authenticate-service-principal/
        static string _subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}";
        static string _tenantId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}";
        static string _applicationId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}";
        static string _applicationSecret = "{your-password}";

        // Create management clients for the Azure resources your app needs to work with.
        // This app works with Resource Groups, and Azure SQL Database.
        static ResourceManagementClient _resourceMgmtClient;
        static SqlManagementClient _sqlMgmtClient;

        // Authentication token
        static AuthenticationResult _token;

        // Azure resource variables
        static string _resourceGroupName = "{resource-group-name}";
        static string _resourceGrouplocation = "{Azure-region}";

        static string _serverlocation = _resourceGrouplocation;
        static string _serverName = "{server-name}";
        static string _serverAdmin = "{server-admin-login}";
        static string _serverAdminPassword = "{server-admin-password}";

        static string _firewallRuleName = "{firewall-rule-name}";
        static string _startIpAddress = "{0.0.0.0}";
        static string _endIpAddress = "{255.255.255.255}";

        static string _databaseName = "{dbfromcsarticle}";
        static string _databaseEdition = DatabaseEditions.Basic;
        static string _databasePerfLevel = ""; // "S0", "S1", and so on here for other tiers


        static void Main(string[] args)
        {
            // Authenticate:
            _token = GetToken(_tenantId, _applicationId, _applicationSecret);
            Console.WriteLine("Token acquired. Expires on:" + _token.ExpiresOn);

            // Instantiate management clients:
            _resourceMgmtClient = new ResourceManagementClient(new Microsoft.Rest.TokenCredentials(_token.AccessToken));
            _sqlMgmtClient = new SqlManagementClient(new TokenCloudCredentials(_subscriptionId, _token.AccessToken));


            Console.WriteLine("Resource group...");
            ResourceGroup rg = CreateOrUpdateResourceGroup(_resourceMgmtClient, _subscriptionId, _resourceGroupName, _resourceGrouplocation);
            Console.WriteLine("Resource group: " + rg.Id);


            Console.WriteLine("Server...");
            ServerGetResponse sgr = CreateOrUpdateServer(_sqlMgmtClient, _resourceGroupName, _serverlocation, _serverName, _serverAdmin, _serverAdminPassword);
            Console.WriteLine("Server: " + sgr.Server.Id);

            Console.WriteLine("Server firewall...");
            FirewallRuleGetResponse fwr = CreateOrUpdateFirewallRule(_sqlMgmtClient, _resourceGroupName, _serverName, _firewallRuleName, _startIpAddress, _endIpAddress);
            Console.WriteLine("Server firewall: " + fwr.FirewallRule.Id);

            Console.WriteLine("Database...");
            DatabaseCreateOrUpdateResponse dbr = CreateOrUpdateDatabase(_sqlMgmtClient, _resourceGroupName, _serverName, _databaseName, _databaseEdition, _databasePerfLevel);
            Console.WriteLine("Database: " + dbr.Database.Id);


            Console.WriteLine("Press any key to continue...");
            Console.ReadKey();
        }

        static ResourceGroup CreateOrUpdateResourceGroup(ResourceManagementClient resourceMgmtClient, string subscriptionId, string resourceGroupName, string resourceGroupLocation)
        {
            ResourceGroup resourceGroupParameters = new ResourceGroup()
            {
                Location = resourceGroupLocation,
            };
            resourceMgmtClient.SubscriptionId = subscriptionId;
            ResourceGroup resourceGroupResult = resourceMgmtClient.ResourceGroups.CreateOrUpdate(resourceGroupName, resourceGroupParameters);
            return resourceGroupResult;
        }

        static ServerGetResponse CreateOrUpdateServer(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverLocation, string serverName, string serverAdmin, string serverAdminPassword)
        {
            ServerCreateOrUpdateParameters serverParameters = new ServerCreateOrUpdateParameters()
            {
                Location = serverLocation,
                Properties = new ServerCreateOrUpdateProperties()
                {
                    AdministratorLogin = serverAdmin,
                    AdministratorLoginPassword = serverAdminPassword,
                    Version = "12.0"
                }
            };
            ServerGetResponse serverResult = sqlMgmtClient.Servers.CreateOrUpdate(resourceGroupName, serverName, serverParameters);
            return serverResult;
        }


        static FirewallRuleGetResponse CreateOrUpdateFirewallRule(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverName, string firewallRuleName, string startIpAddress, string endIpAddress)
        {
            FirewallRuleCreateOrUpdateParameters firewallParameters = new FirewallRuleCreateOrUpdateParameters()
            {
                Properties = new FirewallRuleCreateOrUpdateProperties()
                {
                    StartIpAddress = startIpAddress,
                    EndIpAddress = endIpAddress
                }
            };
            FirewallRuleGetResponse firewallResult = sqlMgmtClient.FirewallRules.CreateOrUpdate(resourceGroupName, serverName, firewallRuleName, firewallParameters);
            return firewallResult;
        }



        static DatabaseCreateOrUpdateResponse CreateOrUpdateDatabase(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverName, string databaseName, string databaseEdition, string databasePerfLevel)
        {
            // Retrieve the server that will host this database
            Server currentServer = sqlMgmtClient.Servers.Get(resourceGroupName, serverName).Server;

            // Create a database: configure create or update parameters and properties explicitly
            DatabaseCreateOrUpdateParameters newDatabaseParameters = new DatabaseCreateOrUpdateParameters()
            {
                Location = currentServer.Location,
                Properties = new DatabaseCreateOrUpdateProperties()
                {
                    CreateMode = DatabaseCreateMode.Default,
                    Edition = databaseEdition,
                    RequestedServiceObjectiveName = databasePerfLevel
                }
            };
            DatabaseCreateOrUpdateResponse dbResponse = sqlMgmtClient.Databases.CreateOrUpdate(resourceGroupName, serverName, databaseName, newDatabaseParameters);
            return dbResponse;
        }



        private static AuthenticationResult GetToken(string tenantId, string applicationId, string applicationSecret)
        {
            AuthenticationContext authContext = new AuthenticationContext("https://login.windows.net/" + tenantId);
            _token = authContext.AcquireToken("https://management.core.windows.net/", new ClientCredential(applicationId, applicationSecret));
            return _token;
        }
      }
    }





## <a name="create-a-service-principal-to-access-resources"></a>Erstellen Sie einen Dienst Zugriff auf die Ressourcen Hauptbenutzer

Das folgende PowerShell-Skript erstellt die Anwendung Active Directory (AD) und der Dienst der Tilgungsanteile, die wir unsere C#-app authentifizieren müssen. Das Skript gibt Werte, die das vorherige Beispiel c# wir müssen aus. Ausführliche Informationen finden Sie unter [Verwenden von Azure PowerShell keinen Zugriff auf Ressourcen Hauptbenutzer Dienst erstellen](../resource-group-authenticate-service-principal.md).

   
    # Sign in to Azure.
    Add-AzureRmAccount
    
    # If you have multiple subscriptions, uncomment and set to the subscription you want to work with.
    #$subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}"
    #Set-AzureRmContext -SubscriptionId $subscriptionId
    
    # Provide these values for your new AAD app.
    # $appName is the display name for your app, must be unique in your directory.
    # $uri does not need to be a real uri.
    # $secret is a password you create.
    
    $appName = "{app-name}"
    $uri = "http://{app-name}"
    $secret = "{app-password}"
    
    # Create a AAD app
    $azureAdApplication = New-AzureRmADApplication -DisplayName $appName -HomePage $Uri -IdentifierUris $Uri -Password $secret
    
    # Create a Service Principal for the app
    $svcprincipal = New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId
    
    # To avoid a PrincipalNotFound error, I pause here for 15 seconds.
    Start-Sleep -s 15
    
    # If you still get a PrincipalNotFound error, then rerun the following until successful. 
    $roleassignment = New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $azureAdApplication.ApplicationId.Guid
    
    
    # Output the values we need for our C# application to successfully authenticate
    
    Write-Output "Copy these values into the C# sample app"
    
    Write-Output "_subscriptionId:" (Get-AzureRmContext).Subscription.SubscriptionId
    Write-Output "_tenantId:" (Get-AzureRmContext).Tenant.TenantId
    Write-Output "_applicationId:" $azureAdApplication.ApplicationId.Guid
    Write-Output "_applicationSecret:" $secret



## <a name="next-steps"></a>Nächste Schritte
Jetzt, da Sie versucht, SQL-Datenbank und Einrichten einer Datenbank mit c# haben, können Sie für den folgenden Artikeln:

- [Verbinden mit SQL-Datenbank mit SQL Server Management Studio, und führen Sie eine Stichproben T-SQL-Abfrage](sql-database-connect-query-ssms.md)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [SQL-Datenbank](https://azure.microsoft.com/documentation/services/sql-database/)
- [Datenbankklasse](https://msdn.microsoft.com/library/azure/microsoft.azure.management.sql.models.database.aspx)




<!--Image references-->
[1]: ./media/sql-database-get-started-csharp/aad.png
[2]: ./media/sql-database-get-started-csharp/permissions.png
[3]: ./media/sql-database-get-started-csharp/getdomain.png
[4]: ./media/sql-database-get-started-csharp/aad2.png
[5]: ./media/sql-database-get-started-csharp/aad-applications.png
[6]: ./media/sql-database-get-started-csharp/add.png
[7]: ./media/sql-database-get-started-csharp/add-application.png
[8]: ./media/sql-database-get-started-csharp/add-application2.png
[9]: ./media/sql-database-get-started-csharp/clientid.png

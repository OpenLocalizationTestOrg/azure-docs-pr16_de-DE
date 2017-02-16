<properties
    pageTitle="Erstellen einer Datenbank flexible Ressourcenpool mit c# | Microsoft Azure"
    description="Verwenden Sie C#-Datenbank Entwicklungstechniken, um einem Ressourcenpool skalierbare flexible Datenbank in Azure SQL-Datenbank zu erstellen, damit Sie Ressourcen über viele Datenbanken gemeinsam nutzen können."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="csharp"
    ms.workload="data-management"
    ms.date="10/04/2016"
    ms.author="sstein"/>

# <a name="create-an-elastic-database-pool-with-cx23"></a>Erstellen einer Datenbank flexible Ressourcenpool mit C & #x 23;

> [AZURE.SELECTOR]
- [Azure-portal](sql-database-elastic-pool-create-portal.md)
- [PowerShell](sql-database-elastic-pool-create-powershell.md)
- [C#](sql-database-elastic-pool-create-csharp.md)


Dieser Artikel beschreibt, wie Sie C#-zum Erstellen einer SQL Azure-Datenbank flexible Ressourcenpool mit [Azure SQL-Datenbank-Bibliothek für .NET](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql). Zum Erstellen einer eigenständigen SQL-Datenbank finden Sie unter [C#-verwenden Sie zum Erstellen einer SQL-Datenbank mit der SQL-Datenbank-Bibliothek für .NET](sql-database-get-started-csharp.md).

Der Azure SQL-Datenbank-Bibliothek für .NET bietet eine [Ressourcenmanager Azure](../azure-resource-manager/resource-group-overview.md)-basierte API, die die [REST-API für SQL Datenbank Ressourcenmanager-basierten](https://msdn.microsoft.com/library/azure/mt163571.aspx)umgebrochen wird.

>[AZURE.NOTE] Viele neue Funktionen der SQL-Datenbank werden nur unterstützt, wenn Sie das [Modell zur Bereitstellung von Azure Ressourcenmanager](../azure-resource-manager/resource-group-overview.md), verwenden, damit Sie immer die neuesten verwenden sollten **Azure SQL-Datenbank-Management-Bibliothek für .NET ([Dokumente](https://msdn.microsoft.com/library/azure/mt349017.aspx) | [NuGet-Paket](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql))**. Die ältere [Modell zur klassischen Bereitstellung basierende Bibliotheken](https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Sql) werden unterstützt nur, zur Abwärtskompatibilität, damit es empfiehlt sich, dass Sie die neueren Ressourcenmanager Grundlage Bibliotheken verwenden.

Um die Schritte in diesem Artikel ausführen zu können, benötigen Sie Folgendes:

- Ein Azure-Abonnement. Wenn Sie ein Abonnement Azure lediglich auf **Kostenloses Konto** oben auf dieser Seite und dann wieder zum Ende dieses Artikels.
- Visual Studio. Eine kostenlose Kopie von Visual Studio finden Sie auf der Seite [Visual Studio-Downloads](https://www.visualstudio.com/downloads/download-visual-studio-vs) .


## <a name="create-a-console-app-and-install-the-required-libraries"></a>Erstellen Sie eine Console-app, und installieren Sie die erforderlichen Bibliotheken

1. Starten Sie Visual Studio.
2. Klicken Sie auf **Datei** > **neue** > **Projekt**.
3. Erstellen einer C#- **Console-Anwendung** , und nennen Sie es: *SqlElasticPoolConsoleApp*


Zum Erstellen einer SQL-Datenbank mit c#, laden Sie die erforderlichen Management-Bibliotheken (mithilfe der [Verwaltungskonsole für Paket](http://docs.nuget.org/Consume/Package-Manager-Console)):

1. Klicken Sie auf **Extras** > **NuGet-Paket-Manager** > **Paket-Manager-Konsole**.
2. Typ `Install-Package Microsoft.Azure.Management.Sql –Pre` , der [Bibliothek von Microsoft Azure SQL-Verwaltung](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql)zu installieren.
3. Typ `Install-Package Microsoft.Azure.Management.ResourceManager –Pre` , [Microsoft Azure Ressourcenmanager Bibliothek](https://www.nuget.org/packages/Microsoft.Azure.Management.ResourceManager)zu installieren.
4. Typ `Install-Package Microsoft.Azure.Common.Authentication –Pre` , [Microsoft Azure allgemeine Authentifizierungsbibliothek](https://www.nuget.org/packages/Microsoft.Azure.Common.Authentication)zu installieren. 



> [AZURE.NOTE] Die Beispiele in diesem Artikel mit einer synchroner Form der einzelnen API Anforderung und bis zum Abschluss der übrigen Anrufen auf dem zugrunde liegenden Dienst blockieren. Asynchrone Methoden stehen zur Verfügung.


## <a name="create-a-sql-elastic-database-pool---c-example"></a>Erstellen Sie ein SQL-Datenbank flexible Pool - C#-Beispiel

Im folgende Beispiel erstellt eine Ressourcengruppe Server, Firewall-Regel, flexible Ressourcenpool und erstellt dann im Pool eine SQL-Datenbank. Finden Sie unter, [Erstellen Sie einen Dienst Hauptbenutzer Zugriff auf Ressourcen](#create-a-service-principal-to-access-resources) zum Abrufen der `_subscriptionId, _tenantId, _applicationId, and _applicationSecret` Variablen.

Ersetzen Sie den Inhalt von **"Program.cs"** mit den folgenden, und Aktualisieren der `{variables}` mit Ihren app-Werten (enthalten nicht die `{}`).


```
using Microsoft.Azure;
using Microsoft.Azure.Management.ResourceManager;
using Microsoft.Azure.Management.ResourceManager.Models;
using Microsoft.Azure.Management.Sql;
using Microsoft.Azure.Management.Sql.Models;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using System;

namespace SqlElasticPoolConsoleApp
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

        static string _poolName = "{pool-name}";
        static string _poolEdition = "{Standard}";
        static int _poolDtus = {100};
        static int _databaseMinDtus = {10};
        static int _databaseMaxDtus = {100};

        static string _databaseName = "{elasticdbfromcsarticle}";



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

            Console.WriteLine("Elastic pool...");
            ElasticPoolCreateOrUpdateResponse epr = CreateOrUpdateElasticDatabasePool(_sqlMgmtClient, _resourceGroupName, _serverName, _poolName, _poolEdition, _poolDtus, _databaseMinDtus, _databaseMaxDtus);
            Console.WriteLine("Elastic pool: " + epr.ElasticPool.Id);

            Console.WriteLine("Database...");
            DatabaseCreateOrUpdateResponse dbr = CreateOrUpdateDatabase(_sqlMgmtClient, _resourceGroupName, _serverName, _databaseName, _poolName);
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



        static ElasticPoolCreateOrUpdateResponse CreateOrUpdateElasticDatabasePool(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverName, string poolName, string poolEdition, int poolDtus, int databaseMinDtus, int databaseMaxDtus)
        {
            // Retrieve the server that will host this elastic pool
            Server currentServer = sqlMgmtClient.Servers.Get(resourceGroupName, serverName).Server;

            // Create elastic pool: configure create or update parameters and properties explicitly
            ElasticPoolCreateOrUpdateParameters newPoolParameters = new ElasticPoolCreateOrUpdateParameters()
            {
                Location = currentServer.Location,
                Properties = new ElasticPoolCreateOrUpdateProperties()
                {
                    Edition = poolEdition,
                    Dtu = poolDtus,
                    DatabaseDtuMin = databaseMinDtus,
                    DatabaseDtuMax = databaseMaxDtus
                }
            };

            // Create the pool
            var newPoolResponse = sqlMgmtClient.ElasticPools.CreateOrUpdate(resourceGroupName, serverName, poolName, newPoolParameters);
            return newPoolResponse;
        }




        static DatabaseCreateOrUpdateResponse CreateOrUpdateDatabase(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverName, string databaseName, string poolName)
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
                    ElasticPoolName = poolName
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
```





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

- [Verwalten Sie Ihrer Ressourcenpool](sql-database-elastic-pool-manage-csharp.md)
- [Flexible Aufträge erstellen](sql-database-elastic-jobs-overview.md): Flexible Aufträge ermöglichen Ihnen die T-SQL-Skripts für eine beliebige Anzahl von Datenbanken in einem Ressourcenpool ausgeführt werden.
- [Skalierung mit Azure SQL-Datenbank](sql-database-elastic-scale-introduction.md): Verwenden von flexible Datenbanktools skalieren.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [SQL-Datenbank](https://azure.microsoft.com/documentation/services/sql-database/)
- [Azure Ressource Management-APIs](https://msdn.microsoft.com/library/azure/dn948464.aspx)

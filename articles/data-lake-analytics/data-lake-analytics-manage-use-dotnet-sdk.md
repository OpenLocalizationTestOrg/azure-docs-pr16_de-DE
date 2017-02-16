<properties 
   pageTitle="Verwalten von Azure Daten dem Analytics mit Azure .NET SDK | Azure" 
   description="Erfahren Sie, wie Daten dem Analytics Aufträge, Datenquellen, Benutzer verwalten. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/23/2016"
   ms.author="jgao"/>

# <a name="manage-azure-data-lake-analytics-using-azure-net-sdk"></a>Verwalten von Azure Daten dem Analytics mit Azure .NET SDK

[AZURE.INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Informationen Sie zum Verwalten von Azure Daten dem Analytics-Konten, Datenquellen, Benutzern und Aufträge mithilfe von Azure .NET SDK. Um Management-Themen mit anderen Tools anzuzeigen, klicken Sie auf der Registerkarte wählen Sie oben.

**Erforderliche Komponenten**

Bevor Sie dieses Lernprogramm beginnen, benötigen Sie Folgendes:

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion Azure abrufen](https://azure.microsoft.com/pricing/free-trial/).


<!-- ################################ -->
<!-- ################################ -->


## <a name="connect-to-azure-data-lake-analytics"></a>Verbinden Sie mit Azure Daten Lake Analytics

Sie benötigen die folgenden Nuget-Paketen:

    Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Pre
    Install-Package Microsoft.Azure.Common 
    Install-Package Microsoft.Azure.Management.ResourceManager -Pre
    Install-Package Microsoft.Azure.Management.DataLake.Analytics -Pre


Im folgenden Beispiel wird gezeigt, wie Sie eine Verbindung zur Azure und Liste die vorhandenen Daten dem Analytics-Konten unter Ihrem Azure-Abonnement.

    using System;
    using System.Collections.Generic;
    using System.Threading;

    using Microsoft.Rest;
    using Microsoft.Rest.Azure.Authentication;

    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.DataLake.Store;
    using Microsoft.Azure.Management.DataLake.Analytics;
    using Microsoft.Azure.Management.DataLake.Analytics.Models;

    namespace ConsoleAcplication1
    {
        class Program
        {

            private const string SUBSCRIPTIONID = "<Enter Your Azure Subscription ID>";
            private const string CLIENTID = "1950a258-227b-4e31-a9cf-717495945fc2";
            private const string DOMAINNAME = "common"; // Replace this string with the user's Azure Active Directory tenant ID or domain name, if needed.

            private static DataLakeAnalyticsAccountManagementClient _adlaClient;

            private static void Main(string[] args)
            {

                var creds = AuthenticateAzure(DOMAINNAME, CLIENTID);

                _adlaClient = new DataLakeAnalyticsAccountManagementClient(creds);
                _adlaClient.SubscriptionId = SUBSCRIPTIONID;

                var adlaAccounts = ListADLAAccounts();

                Console.WriteLine("You have %i Data Lake Analytics account(s).", adlaAccounts.Count);
                for (int i = 0; i < adlaAccounts.Count; i ++)
                {
                    Console.WriteLine(adlaAccounts[i].Name);
                }

                System.Console.WriteLine("Press ENTER to continue");
                System.Console.ReadLine();
            }

            public static ServiceClientCredentials AuthenticateAzure(
            string domainName,
            string nativeClientAppCLIENTID)
            {
                // User login via interactive popup
                SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
                // Use the client ID of an existing AAD "Native Client" application.
                var activeDirectoryClientSettings = ActiveDirectoryClientSettings.UsePromptOnly(nativeClientAppCLIENTID, new Uri("urn:ietf:wg:oauth:2.0:oob"));
                return UserTokenProvider.LoginWithPromptAsync(domainName, activeDirectoryClientSettings).Result;
            }

            public static List<DataLakeAnalyticsAccount> ListADLAAccounts()
            {
                var response = _adlaClient.Account.List();
                var accounts = new List<DataLakeAnalyticsAccount>(response);

                while (response.NextPageLink != null)
                {
                    response = _adlaClient.Account.ListNext(response.NextPageLink);
                    accounts.AddRange(response);
                }

                return accounts;
            }
        }
    }


## <a name="manage-accounts"></a>Verwalten von Konten

Vor dem Ausführen von Daten dem Analytics Aufträge, müssen Sie ein Daten dem Analytics-Konto verfügen. Im Gegensatz zu Azure-HDInsight bezahlen nicht Sie für ein Konto Analytics, wenn sie ein Projekt nicht ausgeführt wird.  Sie Zahlen nur für die Zeit an, wenn sie ein Projekt ausgeführt wird.  Weitere Informationen finden Sie unter [Azure Daten dem Analytics Übersicht](data-lake-analytics-overview.md).  

###<a name="create-accounts"></a>Erstellen von Konten

Sie benötigen eine Azure Ressourcenmanagement Group unter, und ein Konto Lake Datenspeicher, bevor Sie im folgende Beispiel ausgeführt werden können.

Im folgende Code veranschaulicht, wie eine Ressourcengruppe erstellen:

    public static async Task<ResourceGroup> CreateResourceGroupAsync(
        ServiceClientCredentials credential,
        string groupName,
        string subscriptionId,
        string location)
    {

        Console.WriteLine("Creating the resource group...");
        var resourceManagementClient = new ResourceManagementClient(credential)
        { SubscriptionId = subscriptionId };
        var resourceGroup = new ResourceGroup { Location = location };
        return await resourceManagementClient.ResourceGroups.CreateOrUpdateAsync(groupName, resourceGroup);
    }

Mit dem folgende Code veranschaulicht, wie ein Konto Lake Datenspeicher zu erstellen:

    var adlsParameters = new DataLakeStoreAccount(location: _location);
    _adlsClient.Account.Create(_resourceGroupName, _adlsAccountName, adlsParameters);

Im folgende Code veranschaulicht, wie ein Daten dem Analytics-Konto zu erstellen:

    var defaultAdlsAccount = new List<DataLakeStoreAccountInfo> { new DataLakeStoreAccountInfo(adlsAccountName, new DataLakeStoreAccountInfoProperties()) };
    var adlaProperties = new DataLakeAnalyticsAccountProperties(defaultDataLakeStoreAccount: adlsAccountName, dataLakeStoreAccounts: defaultAdlsAccount);
    var adlaParameters = new DataLakeAnalyticsAccount(properties: adlaProperties, location: location);
    var adlaAccount = _adlaClient.Account.Create(resourceGroupName, adlaAccountName, adlaParameters);

###<a name="list-accounts"></a>Liste Konten

Finden Sie unter [Verbinden mit Azure Daten dem Analytics](#connect_to_azure_data_lake_analytics).

###<a name="find-an-account"></a>Suchen nach einer Firma

Nach dem Abrufen eines Objekts in einer Liste von Daten dem Analytics-Konten können Sie Folgendes verwenden, um ein Konto zu ermitteln:

    Predicate<DataLakeAnalyticsAccount> accountFinder = (DataLakeAnalyticsAccount a) => { return a.Name == adlaAccountName; };
    var myAdlaAccount = adlaAccounts.Find(accountFinder);

###<a name="delete-data-lake-analytics-accounts"></a>Löschen Sie die Daten dem Analytics-Konten

Im folgenden Codeausschnitt Löscht ein Daten dem Analytics-Konto an:

    _adlaClient.Account.Delete(resourceGroupName, adlaAccountName);

<!-- ################################ -->
<!-- ################################ -->
## <a name="manage-account-data-sources"></a>Verwalten von Datenquellen Konto

Daten Lake Analytics unterstützt derzeit die folgenden Datenquellen:

- [Azure Lake Datenspeicher](../data-lake-store/data-lake-store-overview.md)
- [Azure-Speicher](../storage/storage-introduction.md)

Wenn Sie ein Konto Analytics erstellen, müssen Sie ein Azure Daten dem Speicher Konto werden im Speicher Standardkonto festlegen. Das Lake Datenspeicher Standardkonto dient zum Auftrag Metadaten und Position Überwachungsprotokolle speichern. Nachdem Sie ein Konto Analytics erstellt haben, können Sie zusätzliche dem Datenspeicher Konten und/oder Speicher Azure-Konto hinzufügen. 

### <a name="find-the-default-data-lake-store-account"></a>Suchen nach dem Lake Datenspeicher Standardkonto

Finden Sie unter Suchen ein Konto in diesem Artikel, um die Daten dem Analytics-Konto zu suchen. Verwenden Sie dann Folgendes ein:

    string adlaDefaultDataLakeStoreAccountName = myAccount.Properties.DefaultDataLakeStoreAccount;


## <a name="use-azure-resource-manager-groups"></a>Verwenden von Azure Ressourcenmanager Gruppen

Applikationen bestehen normalerweise aus vielen Komponenten, beispielsweise eine Web app, Datenbank, Datenbankserver, Speicher und 3rd Party-Dienste. Azure Ressourcenmanager können Sie für die Arbeit mit den Ressourcen in der Anwendung als Gruppe, als eine Ressourcengruppe Azure bezeichnet. Bereitstellen können, aktualisieren, überwachen oder alle Ressourcen für eine Anwendung in einem einzigen, koordinierte Vorgang löschen. Verwenden Sie eine Vorlage für Bereitstellung und dieser Vorlage für die verschiedenen Umgebungen z. B. Tests, Staging und Herstellung arbeiten kann. Sie können die Abrechnung für Ihre Organisation verdeutlichen möchten, indem Sie die Rollup-Kosten für die gesamte Gruppe anzeigen. Weitere Informationen finden Sie unter [Azure Ressourcenmanager Übersicht](../azure-resource-manager/resource-group-overview.md). 

Ein Daten dem Analytics-Dienst kann die folgenden Komponenten umfassen:

- Azure Daten Lake Analytics-Konto
- Erforderliche Azure Daten dem Speicher Standardkonto
- Zusätzliche Azure Daten Lake Speicher-Konten
- Zusätzlicher Speicher Azure-Konten

Sie können alle diese Komponenten im Rahmen einer Ressourcenmanagement Group unter leichter verwalten erstellen.

![Azure Daten Lake Analytics-Konto und Speicher](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-arm-structure.png)

Ein Daten dem Analytics-Konto und die Speicherkonten abhängige müssen in derselben Azure Data Center platziert werden.
Die Ressourcenmanagement Group unter kann jedoch in einem anderen Data Center befinden.  

##<a name="see-also"></a>Siehe auch 

- [Übersicht über Microsoft Azure-Daten Lake Analytics](data-lake-analytics-overview.md)
- [Erste Schritte mit Daten dem Analytics mit Azure-portal](data-lake-analytics-get-started-portal.md)
- [Verwalten von Azure Daten dem Analytics mit Azure-portal](data-lake-analytics-manage-use-portal.md)
- [Überwachen Sie und Behandeln von Problemen mit Azure Daten dem Analytics-Aufträge mithilfe von Azure-portal](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)


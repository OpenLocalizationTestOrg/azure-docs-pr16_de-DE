<properties
    pageTitle="Account-Ressourcenmanagement mit Stapel Management .NET | Microsoft Azure"
    description="Erstellen, löschen und Azure Stapel Konto Ressourcen mit den Stapel Management .NET Bibliothek ändern."
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="big-compute"
    ms.date="10/19/2016"
    ms.author="marsma"/>

# <a name="manage-azure-batch-accounts-and-quotas-with-batch-management-net"></a>Verwalten von Azure Stapel Konten und Kontingente mit Stapel Management .NET

> [AZURE.SELECTOR]
- [Azure-portal](batch-account-create-portal.md)
- [Stapel Management .NET](batch-management-dotnet.md)

Sie können senken Wartung Aufwand in Clientanwendungen Azure Stapel mithilfe der [Stapel Management .NET] [ api_mgmt_net] -Bibliothek mit der Erstellung des Kontos Stapel, löschen, Key-Verwaltung und Kontingent Discovery automatisieren.

- **Erstellen und Löschen von Konten Stapel** innerhalb jeder Region. Wenn als eine unabhängige Software Softwareanbietern () beispielsweise einen Dienst für Kunden Ihnen in denen jede Abrechnung Zwecken ein separates Stapel-Konto zugeordnet ist bieten, können Sie Ihre Kunden-Portal Konto erstellen und Löschen von Funktionen hinzufügen.
- **Abrufen und neu generieren Konto Schlüssel** programmgesteuert nach jedem der Ihre Konten Stapel. Dies kann Ihnen Sicherheitsrichtlinien einhalten, die periodische Rollover oder Ablauf des Kontos Tasten erzwingen. Wenn Sie mehrere Stapel Konten in verschiedenen Azure Regionen haben, erhöht die Automatisierung dieses Prozesses Rollover Ihre Lösung des Effizienz.
- **Konto Kontingente überprüfen** und Übernehmen der Testversion und Fehler Ungenauigkeit von bestimmen die Stapel Konten welcher Grenzen haben. Überprüfen Sie Ihr Konto Kontingente vor dem Starten Aufträge, Pools erstellen oder Hinzufügen von Knoten berechnen, können Sie die vorausschauende Where anpassen oder Ressourcen werden erstellt, wenn diese zu berechnen. Sie können festlegen, welche Konten erfordern, dass das Kontingent erhöht, bevor das Zuordnen zusätzlicher Ressourcen in diese Konten.
- **Kombinieren von Features von anderen Diensten Azure** für einen mit vollständigen Funktionen ausgestatteten Verwaltungsoption – mit Stapel Management .NET, [Azure Active Directory][aad_about], und die [Ressourcenmanager Azure] [ resman_overview] in der gleichen Anwendung nicht trennen. Diese Features und deren APIs verwenden, können Sie eine frictionless Authentifizierung zur Verfügung steht, die Möglichkeit zum Erstellen und Löschen von Ressourcengruppen und die Funktionen, die für eine End-to-End-Lösung oben beschrieben sind bereitstellen.

> [AZURE.NOTE] Während Sie auf der programmgesteuerten Verwaltung von Konten Stapel, Tasten und Kontingente in diesem Artikel liegt der Schwerpunkt, können Sie diesen Aktivitäten mithilfe der [Azure-Portal]ausführen[azure_portal]. Weitere Informationen finden Sie unter [Erstellen einer Stapel Azure-Konto mithilfe des Azure-Portals](batch-account-create-portal.md) und [Kontingente und Grenzwerte für den Stapel Azure-Dienst](batch-quota-limit.md).

## <a name="create-and-delete-batch-accounts"></a>Erstellen und Löschen von Stapel-Konten

Wie erwähnt, ist eines der primären Features der Stapel Management-API zum Erstellen und Löschen von Stapel Konten in einer Azure Region. Verwenden Sie hierzu [BatchManagementClient.Account.CreateAsync] [ net_create] und [DeleteAsync][net_delete], oder ihren synchronen Gegenstücken.

Der folgende Codeausschnitt ein Konto erstellt, erhält das Konto neu erstellte vom Stapel-Dienst und dann gelöscht. In dieser Ausschnitt und die anderen in diesem Artikel `batchManagementClient` ist eine vollständig initialisierte Instanz der [BatchManagementClient][net_mgmt_client].

```csharp
// Create a new Batch account
await batchManagementClient.Account.CreateAsync("MyResourceGroup",
    "mynewaccount",
    new BatchAccountCreateParameters() { Location = "West US" });

// Get the new account from the Batch service
AccountResource account = await batchManagementClient.Account.GetAsync(
    "MyResourceGroup",
    "mynewaccount");

// Delete the account
await batchManagementClient.Account.DeleteAsync("MyResourceGroup", account.Name);
```

> [AZURE.NOTE] Anwendungen, die die Bibliothek Stapel Management .NET und seine BatchManagementClient Klasse verwenden erfordern **Dienstadministrator** oder **Coadministrator** Zugriff auf das Abonnement, das das Konto Stapel zu verwaltende besitzt. Weitere Informationen finden Sie im Abschnitt [Azure Active Directory](#azure-active-directory) und den [AccountManagement] [ acct_mgmt_sample] Code Stichprobe.

## <a name="retrieve-and-regenerate-account-keys"></a>Rufen Sie ab und erneut generieren Sie Konto Tasten

Tasten der primären und sekundären Konto aus einem beliebigen Konto Stapel innerhalb Ihres Abonnements mithilfe von [ListKeysAsync]ermitteln[net_list_keys]. Sie können diese Tasten erneut generieren, mithilfe von [RegenerateKeyAsync][net_regenerate_keys].

```csharp
// Get and print the primary and secondary keys
BatchAccountListKeyResult accountKeys =
    await batchManagementClient.Account.ListKeysAsync(
        "MyResourceGroup",
        "mybatchaccount");
Console.WriteLine("Primary key:   {0}", accountKeys.Primary);
Console.WriteLine("Secondary key: {0}", accountKeys.Secondary);

// Regenerate the primary key
BatchAccountRegenerateKeyResponse newKeys =
    await batchManagementClient.Account.RegenerateKeyAsync(
        "MyResourceGroup",
        "mybatchaccount",
        new BatchAccountRegenerateKeyParameters() {
            KeyName = AccountKeyType.Primary
            });
```

> [AZURE.TIP] Sie können für Ihre Programme für die Verwaltung ein Workflows optimierten Verbindung erstellen. Lassen Sie sich zunächst eine kontoschlüssel für das Blatt-Konto, das Sie mit [ListKeysAsync]verwalten möchten[net_list_keys]. Klicken Sie dann verwenden Sie diesen Schlüssel bei der Initialisierung der Stapel .NET-Bibliothek [BatchSharedKeyCredentials] [ net_sharedkeycred] Klasse, die verwendet wird, bei der Initialisierung [BatchClient][net_batch_client].

## <a name="check-azure-subscription-and-batch-account-quotas"></a>Überprüfen von Azure-Abonnement und Stapel Konto von Kontingenten

Azure-Abonnements und der einzelnen Azure Dienste wie alle Stapel haben Standardkontingente, die die Anzahl der bestimmte Auftraggeber darin enthaltenen begrenzen. Die standardmäßige Kontingente für Azure-Abonnements finden Sie unter [Azure-Abonnement und Beschränkungen Service, Kontingente, und Einschränkungen](./../azure-subscription-service-limits.md). Die standardmäßige Kontingente des Diensts Stapel finden Sie unter [Kontingente und Grenzwerte für den Stapel Azure-Dienst](batch-quota-limit.md). Mithilfe der Bibliothek Stapel Management .NET können Sie diese Kontingente in Ihrer Programme überprüfen. So können Sie Zuteilung Entscheidungen zu treffen, bevor Sie Konten hinzufügen oder Ressourcen wie Pools zu berechnen und Knoten zu berechnen.

### <a name="check-an-azure-subscription-for-batch-account-quotas"></a>Aktivieren Sie ein Azure-Abonnement für Stapel Konto von Kontingenten

Vor dem Erstellen eines Kontos Stapel in einem Bereich, können Sie überprüfen Ihres Abonnements Azure, um festzustellen, ob Sie ein Konto in dieser Region hinzufügen können.

In den folgenden Codeausschnitt verwenden wir zunächst [BatchManagementClient.Account.ListAsync] [ net_mgmt_listaccounts] können Sie eine Reihe von Konten für alle Stapel zu gelangen, die innerhalb eines Abonnements sind. Nachdem wir diese Sammlung erhalten haben, bestimmen wir mit wie vielen Konten im Zielbereich enthalten sind. Anschließend wir [BatchManagementClient.Subscriptions verwenden] [ net_mgmt_subscriptions] das Kontingent für die Stapel zu erhalten und bestimmen, wie vielen Konten (falls vorhanden) in diesem Bereich erstellt werden können.

```csharp
// Get a collection of all Batch accounts within the subscription
BatchAccountListResponse listResponse =
        await batchManagementClient.Account.ListAsync(new AccountListParameters());
IList<AccountResource> accounts = listResponse.Accounts;
Console.WriteLine("Total number of Batch accounts under subscription id {0}:  {1}",
    creds.SubscriptionId,
    accounts.Count);

// Get a count of all accounts within the target region
string region = "westus";
int accountsInRegion = accounts.Count(o => o.Location == region);

// Get the account quota for the specified region
SubscriptionQuotasGetResponse quotaResponse = await batchManagementClient.Subscriptions.GetSubscriptionQuotasAsync(region);
Console.WriteLine("Account quota for {0} region: {1}", region, quotaResponse.AccountQuota);

// Determine how many accounts can be created in the target region
Console.WriteLine("Accounts in {0}: {1}", region, accountsInRegion);
Console.WriteLine("You can create {0} accounts in the {1} region.", quotaResponse.AccountQuota - accountsInRegion, region);
```

In der obigen Codeausschnitt `creds` ist eine Instanz der [TokenCloudCredentials][azure_tokencreds]. Ein Beispiel für dieses Objekt zu erstellen, finden Sie unter der [AccountManagement] [ acct_mgmt_sample] Code Stichprobe auf GitHub.

### <a name="check-a-batch-account-for-compute-resource-quotas"></a>Überprüfen eines Stapel-Kontos zum Berechnen von Serverressourcen

Bevor Sie erhöhen berechnen von Ressourcen in der Stapel-Lösung, können Sie überprüfen, um die Ressourcen, die Sie zuweisen möchten, sicherzustellen des Kontos Kontingente überschreiten wird nicht. Im folgenden Codeausschnitt, Drucken wir die Kontingentinformationen für den Stapel Konto namens `mybatchaccount`. Diese Informationen können Sie in Ihrer Anwendung um zu bestimmen, ob das Konto helfen kann zusätzlichen Ressourcen erstellt werden kann.

```csharp
// First obtain the Batch account
BatchAccountGetResponse getResponse =
    await batchManagementClient.Account.GetAsync("MyResourceGroup", "mybatchaccount");
AccountResource account = getResponse.Resource;

// Now print the compute resource quotas for the account
Console.WriteLine("Core quota: {0}", account.Properties.CoreQuota);
Console.WriteLine("Pool quota: {0}", account.Properties.PoolQuota);
Console.WriteLine("Active job and job schedule quota: {0}", account.Properties.ActiveJobAndJobScheduleQuota);
```

> [AZURE.IMPORTANT] Während der Standardkontingente für Azure-Abonnements und Dienste können zahlreiche diese Grenzwerte durch eine Anforderung der [Azure-Portal]ausgelöst werden[azure_portal]. Beispielsweise finden Sie unter [Kontingente und Grenzwerte für den Dienst Azure Stapel](batch-quota-limit.md) Anweisungen Ihrer Stapel Konto Kontingente erhöhen.

## <a name="batch-management-net-azure-ad-and-resource-manager"></a>Stapelverarbeitung Management .NET, Azure AD und Ressourcenmanager

[Beim Arbeiten mit den Stapel Management .NET Bibliothek, in der Regel auch Azure Active Directory] verwenden,[ aad_about] (Azure AD) und dem [Ressourcenmanager Azure][resman_overview]. Das Beispielprojekt unten erläutert verwendet Azure Active Directory und Ressourcenmanager, während die Stapel Management .NET-API veranschaulicht.

### <a name="azure-active-directory"></a>Azure-Active Directory

Azure selbst Azure AD für die Authentifizierung seiner Kunden, Dienstadministratoren und organisationsinterne Benutzer verwendet. Im Kontext Stapel Management .NET verwenden Sie Azure AD ein Abonnementadministrator oder gemeinsame Administrator authentifizieren. Dadurch wird die Bibliothek Management Abfrage den Stapel-Dienst, und führen Sie die in diesem Artikel beschriebenen Vorgänge.

Im Beispielprojekt erläuterten unterhalb der Azure- [Active Directory-Authentifizierungsbibliothek] [ aad_adal] (ADAL) wird verwendet, um den Benutzer zur Eingabe ihrer Anmeldeinformationen Microsoft aufzufordern. Wenn Service-Administrator oder Coadministrator-Anmeldeinformationen angegeben werden, kann die Anwendung Azure für eine Liste der Abonnements – Abfragen und können erstellen und löschen sowohl Ressourcengruppen und Stapel Konten.

### <a name="resource-manager"></a>Ressourcenmanager

Wenn Sie mit der Bibliothek Stapel Management .NET Stapel Konten erstellen, Sie in der Regel erstellen sie innerhalb einer [Ressourcengruppe][resman_overview]. Sie können die Ressourcengruppe programmgesteuert erstellen, mit der [ResourceManagementClient] [ resman_client] Klasse in [.NET Ressourcenmanager] [ resman_api] Bibliothek. Oder Sie können ein Konto hinzufügen, um eine vorhandene Ressourcengruppe, die Sie zuvor durch die Verwendung der [Azure-Portal]erstellt[azure_portal].

## <a name="sample-project-on-github"></a>Beispiel für Project auf GitHub

Um Stapel Management .NET in Aktion sehen möchten, schauen Sie sich die [AccountManagment] [ acct_mgmt_sample] Projekt auf GitHub Beispiel. Dieser Console-Anwendung zeigt die Erstellung und die Verwendung der [BatchManagementClient] [ net_mgmt_client] und [ResourceManagementClient][resman_client]. Es auch veranschaulicht die Verwendung der Azure- [Active Directory-Authentifizierungsbibliothek] [ aad_adal] (ADAL), die beide Clients erforderlich ist.

Um die Stichprobe Anwendung erfolgreich ausführen zu können, müssen Sie zuerst mit Azure AD registrieren, mithilfe des Azure-Portals. Führen Sie die Schritte im Abschnitt [Hinzufügen einer Anwendung](../active-directory/active-directory-integrating-applications.md#adding-an-application) in [Integration von Applications mit Azure Active Directory] [ aad_integrate] die Stichprobe Anwendung in Ihrem eigenen-Konto registrieren der Standard-Verzeichnis. Achten Sie darauf, um **Systemeigenen Clientanwendung** für den Typ der Anwendung auszuwählen, und Sie können angeben, dass ein beliebiger gültigen URI (z. B. `http://myaccountmanagementsample`) für den **URI umleiten**– er muss kein real Endpunkt.

Nach dem Hinzufügen der Anwendungs, die Stellvertretung der Berechtigung **Access Azure Servicemanagement als Organisation** mit der *Windows Azure Service Management-API* -Anwendung in die Einstellungen der Anwendung im Portal aus:

![Anwendungsberechtigungen Azure-Portal][2]

> [AZURE.TIP] Wenn **Windows Azure Service Management-API** unter *Berechtigungen in anderen Programmen*nicht angezeigt wird, klicken Sie auf **Anwendung hinzufügen**, wählen Sie aus **Windows Azure Service Management-API**, dann klicken Sie auf die Schaltfläche Häkchen. Klicken Sie dann delegieren Sie gemäß Angabe über Berechtigungen aus.

Nachdem Sie die Anwendung hinzugefügt haben, wie zuvor beschrieben, aktualisieren `Program.cs` in der [AccountManagment] [ acct_mgmt_sample] Stichprobe Projekt mit Ihrer Anwendungs URI umleiten und Client-ID Sie können diese Werte auf der Registerkarte **Konfigurieren** Ihrer Anwendung finden:

![Anwendungskonfiguration Azure-Portal][3]

Die [AccountManagment] [ acct_mgmt_sample] Beispiel-Anwendung veranschaulicht die folgenden Vorgänge:

1. Erfassen Sie ein Sicherheitstoken aus Azure Active Directory mithilfe von [ADAL][aad_adal]. Wenn der Benutzer noch nicht angemeldet ist, werden diese für ihre Azure Anmeldeinformationen aufgefordert.
2. Erstellen Sie mithilfe des vom Azure AD-Sicherheitstokens eines [SubscriptionClient] [ resman_subclient] Abfrage Azure für eine Liste der Abonnements, mit dem Konto verknüpft sind. Dadurch wird die Auswahl eines Abonnements, wenn mehrere gefunden werden.
3. Erstellen eines Anmeldeinformationenobjekts mit der ausgewählten Abonnement verknüpft ist.
4. Erstellen von [ResourceManagementClient] [ resman_client] mithilfe der Anmeldeinformationen.
5. Verwenden Sie [ResourceManagementClient] [ resman_client] können Sie eine Ressourcengruppe erstellen.
6. Verwenden Sie [BatchManagementClient] [ net_mgmt_client] mehrere Stapel Konto Vorgänge ausführen:
  - Erstellen Sie ein Konto Stapel in die neue Ressourcengruppe ein.
  - Abrufen des neu erstellten Kontos vom Stapel-Dienst.
  - Drucken Sie das Konto Schlüssel für das neue Konto ein.
  - Einen neuen Primärschlüssel für das Konto neu zu generieren.
  - Drucken Sie die Kontingentinformationen für das Konto ein.
  - Drucken Sie die Kontingentinformationen für das Abonnement.
  - Drucken Sie alle Konten innerhalb des Abonnements.
  - Neu erstelltes Konto zu löschen.
7. Löschen Sie die Ressourcengruppe aus.

Vor dem Löschen der neu erstellten Stapel-Konto und Ressource Gruppe, prüfen Sie beide im [Portal Azure][azure_portal]:

![Azure-Portal zur Anzeige der Ressourcengruppe und Stapel-Konto][1]
<br />
*Azure-Portal zur Anzeige der neuen Ressourcengruppe und Stapel-Konto*

[aad_about]: ../active-directory/active-directory-whatis.md "Was ist Azure Active Directory?"
[aad_adal]: ../active-directory/active-directory-authentication-libraries.md
[aad_auth_scenarios]: ../active-directory/active-directory-authentication-scenarios.md "Authentifizierungsszenarien Azure AD-"
[aad_integrate]: ../active-directory/active-directory-integrating-applications.md "Integrieren von Applications in Azure-Active Directory"
[acct_mgmt_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/AccountManagement
[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_mgmt_net]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[azure_portal]: http://portal.azure.com
[azure_storage]: https://azure.microsoft.com/services/storage/
[azure_tokencreds]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.tokencloudcredentials.aspx
[batch_explorer_project]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[net_batch_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_list_keys]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.listkeysasync.aspx
[net_create]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.createasync.aspx
[net_delete]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.deleteasync.aspx
[net_regenerate_keys]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.regeneratekeyasync.aspx
[net_sharedkeycred]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.auth.batchsharedkeycredentials.aspx
[net_mgmt_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.batchmanagementclient.aspx
[net_mgmt_subscriptions]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.batchmanagementclient.subscriptions.aspx
[net_mgmt_listaccounts]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.iaccountoperations.listasync.aspx
[resman_api]: https://msdn.microsoft.com/library/azure/mt418626.aspx
[resman_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.resourcemanagementclient.aspxs
[resman_subclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.subscriptions.subscriptionclient.aspx
[resman_overview]: ../azure-resource-manager/resource-group-overview.md

[1]: ./media/batch-management-dotnet/portal-01.png
[2]: ./media/batch-management-dotnet/portal-02.png
[3]: ./media/batch-management-dotnet/portal-03.png

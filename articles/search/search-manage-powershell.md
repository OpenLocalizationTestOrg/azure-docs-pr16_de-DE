<properties 
    pageTitle="Verwalten von Azure suchen mit Powershell-Skripts | Microsoft Azure | Cloud gehosteten Suchdienst" 
    description="Verwalten Sie Ihre Azure Suchdienst mit PowerShell-Skripts. Oder aktualisieren Sie einen Azure Suchdienst Schlüssel erstellen Sie und verwalten Sie Azure Suche Administrator" 
    services="search" 
    documentationCenter="" 
    authors="seansaleh" 
    manager="mblythe" 
    editor=""
    tags="azure-resource-manager"/>

<tags 
    ms.service="search" 
    ms.devlang="na" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="powershell" 
    ms.date="08/15/2016" 
    ms.author="seasa"/>

# <a name="manage-your-azure-search-service-with-powershell"></a>Verwalten Sie Ihrer Azure Suchdienst mit PowerShell
> [AZURE.SELECTOR]
- [Portal](search-manage.md)
- [PowerShell](search-manage-powershell.md)
- [REST-API](search-get-started-management-api.md)

In diesem Thema werden die Befehle PowerShell zahlreiche Verwaltungsaufgaben für die Suche Azure Services ausführen. Wir werden durch Erstellen eines Suchdiensts, skalieren und Verwalten ihrer API Schlüssel geführt.
Diese Befehle parallel die Verwaltungsoptionen in der [Azure Suche Management REST-API](http://msdn.microsoft.com/library/dn832684.aspx)zur Verfügung.

## <a name="prerequisites"></a>Erforderliche Komponenten
 
- Sie müssen Azure PowerShell 1.0 oder größer. Anweisungen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).
- Sie müssen Ihre Azure-Abonnement in PowerShell wie nachstehend beschrieben angemeldet sein.

Zuerst müssen Sie melden Sie sich Azure mit dem folgenden Befehl:

    Login-AzureRmAccount

Geben Sie im Dialogfeld Microsoft Azure Login die e-Mail-Adresse Ihre Azure-Konto und das Kennwort ein.

Alternativ können Sie die [Melden Sie sich mit einem Dienst Hauptbenutzer nicht interaktiv](../resource-group-authenticate-service-principal.md).

Wenn Sie mehrere Azure-Abonnements haben, müssen Sie Ihr Abonnement Azure festlegen. Führen Sie diesen Befehl zum Anzeigen einer Liste von Ihrer aktuellen Abonnements.

    Get-AzureRmSubscription | sort SubscriptionName | Select SubscriptionName

Wenn Sie das Abonnement angeben möchten, führen Sie den folgenden Befehl ein. Im folgenden Beispiel wird der Namen des Abonnements `ContosoSubscription`.

    Select-AzureRmSubscription -SubscriptionName ContosoSubscription

## <a name="commands-to-help-you-get-started"></a>Befehle, die Ihnen helfen, erste Schritte

    $serviceName = "your-service-name-lowercase-with-dashes"
    $sku = "free" # or "basic" or "standard" for paid services
    $location = "West US"
    # You can get a list of potential locations with
    # (Get-AzureRmResourceProvider -ListAvailable | Where-Object {$_.ProviderNamespace -eq 'Microsoft.Search'}).Locations
    $resourceGroupName = "YourResourceGroup" 
    # If you don't already have this resource group, you can create it with 
    # New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    # Register the ARM provider idempotently. This must be done once per subscription
    Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Search" -Force

    # Create a new search service
    # This command will return once the service is fully created
    New-AzureRmResourceGroupDeployment `
        -ResourceGroupName $resourceGroupName `
        -TemplateUri "https://gallery.azure.com/artifact/20151001/Microsoft.Search.1.0.9/DeploymentTemplates/searchServiceDefaultTemplate.json" `
        -NameFromTemplate $serviceName `
        -Sku $sku `
        -Location $location `
        -PartitionCount 1 `
        -ReplicaCount 1
    
    # Get information about your new service and store it in $resource
    $resource = Get-AzureRmResource `
        -ResourceType "Microsoft.Search/searchServices" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19
    
    # View your resource
    $resource
    
    # Get the primary admin API key
    $primaryKey = (Invoke-AzureRmResourceAction `
        -Action listAdminKeys `
        -ResourceId $resource.ResourceId `
        -ApiVersion 2015-08-19).PrimaryKey

    # Regenerate the secondary admin API Key
    $secondaryKey = (Invoke-AzureRmResourceAction `
        -ResourceType "Microsoft.Search/searchServices/regenerateAdminKey" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19 `
        -Action secondary).SecondaryKey

    # Create a query key for read only access to your indexes
    $queryKeyDescription = "query-key-created-from-powershell"
    $queryKey = (Invoke-AzureRmResourceAction `
        -ResourceType "Microsoft.Search/searchServices/createQueryKey" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19 `
        -Action $queryKeyDescription).Key
    
    # View your query key
    $queryKey

    # Delete query key
    Remove-AzureRmResource `
        -ResourceType "Microsoft.Search/searchServices/deleteQueryKey/$($queryKey)" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19
        
    # Scale your service up
    # Note that this will only work if you made a non "free" service
    # This command will not return until the operation is finished
    # It can take 15 minutes or more to provision the additional resources
    $resource.Properties.ReplicaCount = 2
    $resource | Set-AzureRmResource
    
    # Delete your service
    # Deleting your service will delete all indexes and data in the service
    $resource | Remove-AzureRmResource
    
## <a name="next-steps"></a>Nächste Schritte
    
Nun der Dienst erstellt wurde, können Sie die nächsten Schritte ausführen: Erstellen eines [Index](search-what-is-an-index.md), [Abfrage einen Index](search-query-overview.md), und klicken Sie abschließend erstellen und Verwalten von Eigene Suchen-Anwendung, die Suche Azure verwendet.

- [Erstellen einer Azure Suchindex Azure-Portal](search-create-index-portal.md)

- [Abfrage eine Azure Suchindex mit Search Explorer Azure-Portal](search-explorer.md)

- [Einrichten eines Indexers zum Laden von Daten aus anderen Diensten](search-indexer-overview.md)

- [Verwenden von Azure Suchen in .NET](search-howto-dotnet-sdk.md)

- [Analysieren Sie Ihre Suche Azure Datenverkehr](search-traffic-analytics.md)

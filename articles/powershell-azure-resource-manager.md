<properties 
    pageTitle="Azure PowerShell mit Ressourcenmanager | Microsoft Azure" 
    description="Einführung in die mit Azure PowerShell mehrere Ressourcen als eine Ressourcengruppe in Azure bereitstellen." 
    services="azure-resource-manager" 
    documentationCenter="" 
    authors="tfitzmac" 
    manager="timlt" 
    editor="tysonn"/>

<tags 
    ms.service="azure-resource-manager" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="powershell" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/18/2016" 
    ms.author="tomfitz"/>

# <a name="using-azure-powershell-with-azure-resource-manager"></a>Mithilfe der Azure PowerShell mit Azure Ressourcenmanager

> [AZURE.SELECTOR]
- [Portal](azure-portal/resource-group-portal.md) 
- [Azure CLI](xplat-cli-azure-resource-manager.md)
- [Azure PowerShell](powershell-azure-resource-manager.md)
- [REST-API](resource-manager-rest-api.md)


Azure Ressourcenmanager implementiert einen modernen Ansatz zum Azure Ressourcen Lebenszyklus Steuerelement. Anstatt zu erstellen und Verwalten von einzelnen Ressourcen, beginnen Sie, indem Sie so vor einer gesamten Lösung, wie z. B. eines Blogs, eine Fotogalerie, eines SharePoint-Portals oder ein Wiki. Sie mithilfe einer Vorlage – eine deklarative Darstellung der Lösung – eine Ressourcengruppe definieren, die alle Ressourcen zur Unterstützung der Lösung benötigten enthält. Klicken Sie dann bereitstellen und Verwalten von dieser Ressourcengruppe als eine logische Einheit. 

In diesem Lernprogramm erfahren Sie, wie Azure PowerShell Azure Ressourcenmanager verwenden. Es führt Sie durch das Verfahren zum Bereitstellen einer Lösung und Arbeiten mit dieser Lösung. Sie werden bereitstellen Azure PowerShell und Ressourcenmanager Vorlage verwenden:

- SqlServer - Datenbank hosten
- SQL-Datenbank - zum Speichern der Daten
- Firewallregeln, um das Web app zum Herstellen der Datenbank zu erlauben
- App Serviceplan - für die Definition der Funktionen und Kosten des Web app
- Website - zum Ausführen des Web app
- Web Config - für das Speichern der Verbindungszeichenfolge in der Datenbank 
- Warnungsregeln - zum Überwachen der Leistung und Fehlern.
- App Einsichten - Einstellungen für automatische Skalierung für

## <a name="prerequisites"></a>Erforderliche Komponenten

Um dieses Lernprogramms abgeschlossen haben, müssen Sie folgende Aktionen ausführen:

- Ein Azure-Konto
  + Sie können [ein Azure-Konto kostenlos öffnen](/pricing/free-trial/?WT.mc_id=A261C142F): Abrufen von Gutschriften können Sie kostenpflichtiges Azure Services ausprobieren und sogar nachdem sie es gewohnt sind bis können Sie das Konto behalten und Verwendung frei Azure Dienste, wie z. B. Websites. Ihre Kreditkarte wird nie belastet, sofern Sie explizit der Einstellungen für ändern und festlegen, dass Sie in Rechnung gestellt.
  
  + Können Sie die [Vorteile der MSDN-Abonnent aktivieren](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F): Ihr MSDN-Abonnement bietet Ihnen Gutschriften jeden Monat, die Sie für kostenpflichtiges Azure-Dienste verwenden können.
- Azure PowerShell 1.0. Informationen zu dieser Version und wie Sie diese installieren finden Sie unter [Informationen zum Installieren und konfigurieren Azure PowerShell](powershell-install-configure.md).

In diesem Lernprogramm für Anfänger PowerShell ausgelegt ist, aber es wird davon ausgegangen, dass Sie die grundlegende Konzepte, wie Module, Cmdlets und Sitzungen verstehen.

## <a name="get-help-for-cmdlets"></a>Erhalten von Hilfe zu cmdlets

Verwenden Sie ausführliche Hilfe für alle Cmdlet, die Sie in diesem Lernprogramm finden Sie unter, um das Cmdlet Hilfe. 

    Get-Help <cmdlet-name> -Detailed

Geben Sie beispielsweise für das Cmdlet "Get-AzureRmResource" Hilfe hierzu:

    Get-Help Get-AzureRmResource -Detailed

Um eine Liste der Cmdlets im Modul "Ressourcen" mit einer Zusammenfassung Hilfe erhalten möchten, geben Sie Folgendes ein: 

    Get-Command -Module AzureRM.Resources | Get-Help | Format-Table Name, Synopsis

Das Ergebnis sieht in etwa wie im folgenden Ausschnitt aus:

    Name                                   Synopsis
    ----                                   --------
    Find-AzureRmResource                   Searches for resources using the specified parameters.
    Find-AzureRmResourceGroup              Searches for resource group using the specified parameters.
    Get-AzureRmADGroup                     Filters active directory groups.
    Get-AzureRmADGroupMember               Get a group members.
    ...

Zum Abrufen der vollständigen Hilfe für ein Cmdlet Geben Sie einen Befehl mit dem Format:

    Get-Help <cmdlet-name> -Full
  
## <a name="login-to-your-azure-account"></a>Melden Sie sich bei Ihrem Konto Azure

Bevor Sie Ihre Lösung, müssen Sie bei Ihrem Konto anmelden.

Zum Anmelden bei Ihrem Konto Azure, verwenden Sie das Cmdlet **AzureRmAccount hinzufügen** .

    Add-AzureRmAccount

Das Cmdlet werden Sie aufgefordert, die Anmeldeinformationen für Ihr Konto Azure. Nach der Anmeldung lädt diese Einstellungen für Ihr Konto, damit sie für Azure PowerShell verfügbar sind. 

Die kontoeinstellungen ablaufen, sodass Sie diese gelegentlich aktualisieren müssen. Führen Sie zum Aktualisieren der Konten-Einstellungen **Hinzufügen-AzureRmAccount** erneut aus. 

>[AZURE.NOTE] Die Ressourcenmanager Module erfordert AzureRmAccount hinzufügen. Eine veröffentlichen Einstellungsdatei ist nicht ausreichend.     

Wenn Sie mehr als ein Abonnement besitzen, geben Sie die Abonnement-Id, die Sie für die Bereitstellung mit das Cmdlet " **Set-AzureRmContext** " verwenden möchten.

    Set-AzureRmContext -SubscriptionID <YourSubscriptionId>

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Vor der Bereitstellung von Ressourcen zu Ihrem Abonnement, müssen Sie eine Ressourcengruppe erstellen, die die Ressourcen enthalten sollen. 

Verwenden Sie zum Erstellen einer Ressourcengruppe das Cmdlet " **New-AzureRmResourceGroup** " ein.

Der Befehl verwendet den Parameter **Name** einen Namen für die Ressourcengruppe und den **Speicherort** Parameter an die Position angeben. Basierend auf was wir im vorherigen Abschnitt festgestellt, werden wir für den Speicherort "Westen US" verwenden.

    New-AzureRmResourceGroup -Name TestRG1 -Location "West US"
    
Die Ausgabe werden ähnlich wie:

    ResourceGroupName : TestRG1
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/{guid}/resourceGroups/TestRG1

Der Ressourcengruppe wurde erfolgreich erstellt.

## <a name="deploy-your-solution"></a>Bereitstellen der Lösung

In diesem Thema wird nicht gezeigt, wie Ihre Vorlage erstellen oder die Struktur der Vorlage diskutieren. Diese Informationen finden Sie unter [Azure Ressourcenmanager Authoring-Vorlagen](resource-group-authoring-templates.md) und [Ressourcenmanager Vorlage Exemplarische Vorgehensweise](resource-manager-template-walkthrough.md). Stellen Sie die vordefinierte Vorlage [Bereitstellen einer Web App mit einer SQL-Datenbank](https://azure.microsoft.com/documentation/templates/201-web-app-sql-database/) aus [Azure Schnellstart Vorlagen](https://azure.microsoft.com/documentation/templates/)bereit.

Sie haben Ihre Ressourcengruppe und, sodass Sie jetzt die in der Vorlage auf die Ressourcengruppe definierte Infrastruktur bereitstellen möchten, müssen Ihre Vorlage. Bereitstellen von Ressourcen mit dem **New-AzureRmResourceGroupDeployment** -Cmdlet. Die Vorlage gibt viele Standardwerte, den wir verwenden möchten, damit Sie nicht benötigen, um Werte für diesen Parameter bereitzustellen. Die grundlegende Syntax sieht wie folgt aus:

    New-AzureRmResourceGroupDeployment -ResourceGroupName TestRG1 -administratorLogin exampleadmin -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-sql-database/azuredeploy.json 

Sie angeben der Ressourcengruppe und den Speicherort der Vorlage ein. Wenn Ihre Vorlage eine lokale Datei ist, verwenden Sie den Parameter **- TemplateFile** , und Sie geben Sie den Pfad zu der Vorlage. Sie können festlegen, die **-Modus** Parameter entweder **inkrementell** oder **abgeschlossen**. Standardmäßig führt Ressourcenmanager inkrementelle Aktualisierung während der Bereitstellung. Daher ist es nicht unbedingt erforderlich festlegen **-Modus** Wenn **inkrementell**angezeigt werden soll. Die Unterschiede zwischen Modi Bereitstellung finden Sie unter [Bereitstellen einer Anwendung mit Ressourcenmanager Azure-Vorlage](resource-group-template-deploy.md). 

###<a name="dynamic-template-parameters"></a>Dynamische Vorlagenparameter

Wenn Sie mit PowerShell vertraut sind, wissen Sie, dass Sie durch die verfügbaren Parameter für ein Cmdlet wechseln können, indem Sie ein Minuszeichen (-) eingeben und dann die TAB-Taste drücken. Diese Funktionalität funktioniert auch mit den Parametern, die Sie in der Vorlage definieren. Sobald Sie den Namen der Vorlage eingeben, wird das Cmdlet die Vorlage abgerufen, analysiert und fügt die Vorlagenparameter zum Befehl dynamisch. Dies erleichtert die Vorlage Parameterwerte festlegen.

Wenn Sie den Befehl eingeben, werden Sie aufgefordert, den fehlenden obligatorische Parameter **AdministratorLoginPassword**. Und wenn Sie das Kennwort eingeben, wird der sichere Zeichenfolgenwert verdeckt. Diese Strategie entfällt das Risiko ein Kennwort im nur-Text zur Verfügung zu stellen.

    cmdlet New-AzureRmResourceGroupDeployment at command pipeline position 1
    Supply values for the following parameters:
    (Type !? for Help.)
    administratorLoginPassword: ********

Wenn die Vorlage ein Parameters mit einem Namen, die mit einem der Parameter in den Befehl zur Bereitstellung der Vorlage enthält (z. B. einschließlich eines Parameters mit dem Namen **ResourceGroupName** in der Vorlage, die den **ResourceGroupName** -Parameter in das [Neue AzureRmResourceGroupDeployment](https://msdn.microsoft.com/library/azure/mt679003.aspx) Cmdlet identisch ist) übereinstimmt, werden Sie aufgefordert Bereitstellen eines Werts für einen Parameter mit dem Suffix **FromTemplate** (z. B. **ResourceGroupNameFromTemplate**). Im Allgemeinen sollten Sie diese Verwirrung vermeiden, indem Sie nicht Benennen von Parametern mit demselben Namen als Parameter für Bereitstellung Operationen verwendet.

Der Befehl führt und gibt Nachrichten aus, wie die Ressourcen erstellt werden. Schließlich wird das Ergebnis der Bereitstellung.

    DeploymentName    : azuredeploy
    ResourceGroupName : TestRG1
    ProvisioningState : Succeeded
    Timestamp         : 4/11/2016 7:26:11 PM
    Mode              : Incremental
    TemplateLink      :
                Uri            : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-sql-database/azuredeploy.json
                ContentVersion : 1.0.0.0
    Parameters        :
                Name             Type                       Value
                ===============  =========================  ==========
                skuName          String                     F1
                skuCapacity      Int                        1
                administratorLogin  String                  exampleadmin
                administratorLoginPassword  SecureString
                databaseName     String                     sampledb
                collation        String                     SQL_Latin1_General_CP1_CI_AS
                edition          String                     Basic
                maxSizeBytes     String                     1073741824
                requestedServiceObjectiveName  String       Basic

    Outputs           :
                Name             Type                       Value
                ===============  =========================  ==========
                siteUri          String                     websites5wdai7p2k2g4.azurewebsites.net
                sqlSvrFqdn       String                     sqlservers5wdai7p2k2g4.database.windows.net
                    
    DeploymentDebugLogLevel :

In wenigen Schritten werden erstellt und die benötigten Ressourcen für eine komplexe Website bereitgestellt. 

### <a name="log-debug-information"></a>Debuggen Protokollinformationen

Wenn Sie eine Vorlage bereitstellen, können Sie weitere Informationen über die Anforderung und Antwort durch den **DeploymentDebugLogLevel -** Parameter angeben, wenn **Neue-AzureRmResourceGroupDeployment**ausgeführt protokollieren. Diese Informationen möglicherweise Bereitstellungsfehler bei der Problembehandlung helfen. Der Standardwert ist **keiner** was keine Anforderung bedeutet oder Antwortinhalt angemeldet ist. Sie können die Protokollierung des Inhalts aus der Anforderung, Antwort oder beides angeben.  Weitere Informationen zur Problembehandlung Bereitstellungen und Informationen zum Debuggen Protokollierung finden Sie unter [Problembehandlung Ressource Gruppe Bereitstellungen mit Azure PowerShell](resource-manager-troubleshoot-deployments-powershell.md). Das folgende Beispiel protokolliert die Anfrage Inhalt und die Antwortinhalt für die Bereitstellung.

    New-AzureRmResourceGroupDeployment -ResourceGroupName TestRG1 -DeploymentDebugLogLevel All -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-sql-database/azuredeploy.json 

> [AZURE.NOTE] Wenn Sie den Parameter DeploymentDebugLogLevel festlegen, überlegen Sie die Art der Informationen, die Sie während der Bereitstellung in übergeben. Protokollieren von Informationen über die Anforderung oder Antwort könnten Sie potenziell vertrauliche Daten verfügbar machen, die über die Bereitstellungsvorgänge abgerufen werden. 


## <a name="get-information-about-your-resource-groups"></a>Erhalten von Informationen zu Ihrer Ressourcengruppen

Nach dem Erstellen einer Ressourcengruppe, können Sie die Cmdlets im Modul Ressourcenmanager Ihrer Ressourcengruppen verwalten.

- Verwenden Sie eine Ressourcengruppe in Ihrem Abonnement, um das Cmdlet " **Get-AzureRmResourceGroup** ":

        Get-AzureRmResourceGroup -ResourceGroupName TestRG1
    
    Gibt die folgende Informationen zurück:

        ResourceGroupName : TestRG1
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/{guid}/resourceGroups/TestRG
        
        ...

    Wenn Sie einen Ressourcengruppennamen nicht angeben, gibt das Cmdlet aller Ressourcengruppen in Ihr Abonnement.

- Verwenden Sie die Ressourcen in der Ressourcengruppe, um das Cmdlet **AzureRmResource suchen** und ihre **ResourceGroupNameContains** Parameter. Ohne Parameter ruft suchen-AzureRmResource alle Ressourcen in Ihrem Azure-Abonnement.

        Find-AzureRmResource -ResourceGroupNameContains TestRG1
    
     Gibt die eine Liste von Ressourcen wie formatiert:
        
        Name              : sqlservers5wdai7p2k2g4
        ResourceId        : /subscriptions/{guid}/resourceGroups/TestRG1/providers/Microsoft.Sql/servers/sqlservers5wdai7p2k2g4
        ResourceName      : sqlservers5wdai7p2k2g4
        ResourceType      : Microsoft.Sql/servers
        Kind              : v2.0
        ResourceGroupName : TestRG1
        Location          : westus
        SubscriptionId    : {guid}
        Tags              : {System.Collections.Hashtable}
        ...
            
- Kategorien können Sie logisch organisieren Sie die Ressourcen in Ihrem Abonnement und Ressourcen mit den **Suchen-AzureRmResource** und **Suchen-AzureRmResourceGroup** Cmdlets abrufen.

        Find-AzureRmResource -TagName displayName -TagValue Website

        Name              : webSites5wdai7p2k2g4
        ResourceId        : /subscriptions/{guid}/resourceGroups/TestRG1/providers/Microsoft.Web/sites/webSites5wdai7p2k2g4
        ResourceName      : webSites5wdai7p2k2g4
        ResourceType      : Microsoft.Web/sites
        ResourceGroupName : TestRG1
        Location          : westus
        SubscriptionId    : {guid}
                
      There is much more you can do with tags. For more information, see [Using tags to organize your Azure resources](resource-group-using-tags.md).

## <a name="add-to-a-resource-group"></a>Hinzufügen einer Ressourcengruppe

Um eine Ressource der Ressourcengruppe hinzugefügt haben, können Sie das Cmdlet " **New-AzureRmResource** " verwenden. Jedoch möglicherweise beim Hinzufügen einer Ressource auf diese Weise, übersichtlich verursachen, da die neue Ressource in der Vorlage nicht vorhanden ist. Wenn Sie die alte Vorlage erneut bereitgestellt, würden Sie eine Lösung unvollständige bereitstellen. Wenn Sie häufig bereitstellen, finden Sie es einfacher und die Zuverlässigkeit Ihrer Vorlage die neue Ressource hinzufügen, und stellen es erneut.

## <a name="move-a-resource"></a>Verschieben einer Ressource

Sie können vorhandene Ressourcen in einer neuen Ressourcengruppe verschieben. Beispiele finden Sie unter [Verschieben von Ressourcen zu neuen Ressourcengruppe oder das Abonnement](resource-group-move-resources.md).

## <a name="export-template"></a>Exportieren der Vorlage

Für eine vorhandene Ressourcengruppe (über PowerShell oder eine der anderen Methoden wie im Portal bereitgestellt) können Sie die Vorlage Ressourcenmanager für die Ressourcengruppe anzeigen. Exportieren der Vorlage bietet zwei Vorteile:

1. Sie können einfach zukünftige Bereitstellungen der Lösung automatisieren, da alle der Infrastruktur ist in der Vorlage definiert.

2. Sie können mit vertraut Vorlagensyntax anhand der bei der JSON JavaScript Object Notation (), die Ihre Lösung darstellt.

Sie können über die PowerShell Vorlage generieren, die den aktuellen Zustand der Ressourcengruppe darstellt oder Abrufen die Vorlage, die für eine bestimmte Bereitstellung verwendet wurde.

Exportieren der Vorlage für eine Ressourcengruppe ist hilfreich, wenn Sie eine Ressourcengruppe Änderungen vorgenommen haben, und die JSON-Darstellung des aktuellen Zustand abgerufen werden müssen. Die generierte Vorlage wird jedoch nur eine minimale Anzahl von Parametern und keine Variablen enthält. Die meisten Werte in der Vorlage werden hartcodierte. Vor der Bereitstellung der Vorlage generierten, möchten Sie möglicherweise Konvertieren von mehreren Werten in Parameter, damit Sie die Bereitstellung für den verschiedenen Umgebungen anpassen können.

Exportieren der Vorlage für eine bestimmte Bereitstellung ist hilfreich, wenn Sie müssen die ist-Vorlage anzeigen möchten, die zum Bereitstellen von Ressourcen verwendet wurde. Die Vorlage wird alle Parameter und für die ursprüngliche Bereitstellung definierten Variablen enthalten. Jedoch, wenn eine Person in Ihrer Organisation der Ressourcengruppe außerhalb von was in der Vorlage definiert ist Änderungen vorgenommen hat, wird dieser Vorlage nicht im aktuellen Zustand der Ressourcengruppe darstellen.

> [AZURE.NOTE] Die Vorlage Exportfunktion ist in der Vorschau und zurzeit nicht alle Ressourcentypen unterstützt Exportieren einer Vorlage. Wenn Sie versuchen, eine Vorlage exportieren, wird möglicherweise einen Fehler angezeigt, der besagt, dass einige Ressourcen nicht exportiert wurden. Bei Bedarf können Sie diese Ressourcen manuell in Ihre Vorlage definieren, nach dem herunterladen.

###<a name="export-template-from-resource-group"></a>Exportieren von Vorlage aus Ressourcengruppe

Wenn die Vorlage für eine Ressourcengruppe anzeigen möchten, führen Sie das **Exportieren-AzureRmResourceGroup** -Cmdlet.

    Export-AzureRmResourceGroup -ResourceGroupName TestRG1 -Path c:\Azure\Templates\Downloads\TestRG1.json
    
###<a name="download-template-from-deployment"></a>Herunterladen der Vorlage aus Bereitstellung

Wenn die Vorlage für eine bestimmte Bereitstellung herunterladen möchten, führen Sie das **Speichern-AzureRmResourceGroupDeploymentTemplate** -Cmdlet.

    Save-AzureRmResourceGroupDeploymentTemplate -DeploymentName azuredeploy -ResourceGroupName TestRG1 -Path c:\Azure\Templates\Downloads\azuredeploy.json

## <a name="delete-resources-or-resource-group"></a>Löschen von Ressourcen oder Ressourcengruppe

- Verwenden Sie zum Löschen einer Ressource aus der Ressourcengruppe das Cmdlet **AzureRmResource entfernen** aus. Dieses Cmdlet die Ressource löscht, jedoch nicht die Ressourcengruppe.

    Dieser Befehl entfernt die Website "Testsite" aus der Ressourcengruppe TestRG1.

        Remove-AzureRmResource -Name TestSite -ResourceGroupName TestRG1 -ResourceType "Microsoft.Web/sites" -ApiVersion 2015-08-01

- Verwenden Sie zum Löschen einer Ressourcengruppe das Cmdlet **Entfernen-AzureRmResourceGroup** ein. Dieses Cmdlet löscht die Ressourcengruppe und seine Ressourcen.

        Remove-AzureRmResourceGroup -Name TestRG1
        
    Sie werden aufgefordert, den Löschvorgang zu bestätigen.
        
        Confirm
        Are you sure you want to remove resource group 'TestRG1'
        [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y

## <a name="deployment-script"></a>Bereitstellungsskript

Bereitstellen von Ressourcen in Azure früheren Bereitstellung Beispielen in diesem Thema angezeigt wurden nur die einzelnen Cmdlets erforderlich sind. Im folgenden Beispiel wird ein Bereitstellungsskript vor, die die Ressourcengruppe erstellt, und die Ressourcen bereitstellt.

    <#
      .SYNOPSIS
      Deploys a template to Azure
      
      .DESCRIPTION
      Deploys an Azure Resource Manager template

      .PARAMETER subscriptionId
      The subscription id where the template will be deployed.

      .PARAMETER resourceGroupName
      The resource group where the template will be deployed. Can be the name of an existing or a new resource group.

      .PARAMETER resourceGroupLocation
      Optional, a resource group location. If specified, will try to create a new resource group in this location. If not specified, assumes resource group is existing.

      .PARAMETER deploymentName
      The deployment name.

      .PARAMETER templateFilePath
      Optional, path to the template file. Defaults to template.json.

      .PARAMETER parametersFilePath
      Optional, path to the parameters file. Defaults to parameters.json. If file is not found, will prompt for parameter values based on template.
    #>

    param(
      [Parameter(Mandatory=$True)]
      [string]
      $subscriptionId,

      [Parameter(Mandatory=$True)]
      [string]
      $resourceGroupName,

      [string]
      $resourceGroupLocation,

      [Parameter(Mandatory=$True)]
      [string]
      $deploymentName,

      [string]
      $templateFilePath = "template.json",

      [string]
      $parametersFilePath = "parameters.json"
    )

    #******************************************************************************
    # Script body
    # Execution begins here 
    #******************************************************************************
    $ErrorActionPreference = "Stop"

    # sign in
    Write-Host "Logging in...";
    Add-AzureRmAccount;

    # select subscription
    Write-Host "Selecting subscription '$subscriptionId'";
    Set-AzureRmContext -SubscriptionID $subscriptionId;

    #Create or check for existing resource group
    $resourceGroup = Get-AzureRmResourceGroup -Name $resourceGroupName -ErrorAction SilentlyContinue
    if(!$resourceGroup)
    {
      Write-Host "Resource group '$resourceGroupName' does not exist. To create a new resource group, please enter a location.";
      if(!$resourceGroupLocation) {
        $resourceGroupLocation = Read-Host "resourceGroupLocation";
      }
      Write-Host "Creating resource group '$resourceGroupName' in location '$resourceGroupLocation'";
      New-AzureRmResourceGroup -Name $resourceGroupName -Location $resourceGroupLocation
    }
    else{
      Write-Host "Using existing resource group '$resourceGroupName'";
    }

    # Start the deployment
    Write-Host "Starting deployment...";
    if(Test-Path $parametersFilePath) {
      New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath -TemplateParameterFile $parametersFilePath;
    } else {
      New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath;
    }

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum Erstellen von Vorlagen für Ressourcenmanager, finden Sie unter [Authoring Azure Ressourcenmanager Vorlagen](./resource-group-authoring-templates.md).
- Weitere Informationen zum Bereitstellen von Vorlagen, finden Sie unter [Bereitstellen einer Anwendung mit Azure Ressourcenmanager Vorlage](./resource-group-template-deploy.md).
- Eine ausführliche Beispiele Bereitstellen eines Projekts finden Sie unter [Bereitstellen Microservices vorhersehbar in Azure](app-service-web/app-service-deploy-complex-application-predictably.md).
- Weitere Informationen zur Problembehandlung bei einer Bereitstellung, die fehlgeschlagen ist, finden Sie unter [Problembehandlung Ressource Gruppe Bereitstellungen in Azure](./resource-manager-troubleshoot-deployments-powershell.md).


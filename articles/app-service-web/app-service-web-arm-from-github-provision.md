<properties 
    pageTitle="Bereitstellen einer Web-app, die mit einem GitHub Repository verknüpft ist" 
    description="Verwenden einer Vorlage Ressourcenmanager Azure eine Web app bereitstellen, die ein Projekt aus einer GitHub Repository enthält." 
    services="app-service" 
    documentationCenter="" 
    authors="cephalin" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="04/27/2016" 
    ms.author="cephalin"/>

# <a name="deploy-a-web-app-linked-to-a-github-repository"></a>Bereitstellen einer Web app mit einem GitHub Repository verknüpft sind

In diesem Thema erfahren Sie, wie eine Ressourcenmanager Azure-Vorlage erstellen, die eine Web app bereitstellt, die mit einem Projekt in ein Repository GitHub verknüpft ist. Sie erfahren, wie um zu definieren, welche Ressourcen bereitgestellt werden und wie Sie Parameter zu definieren, die sind angegeben, wann die Bereitstellung ausgeführt wird. Sie können mithilfe dieser Vorlage für Ihre eigenen Bereitstellungen oder anpassen, um Ihren Anforderungen entsprechen.

Weitere Informationen zum Erstellen von Vorlagen finden Sie unter [Authoring Azure Ressourcenmanager Vorlagen](../resource-group-authoring-templates.md).

Vollständige Vorlage finden Sie unter [Web App verknüpfte GitHub Vorlage](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-github-deploy/azuredeploy.json).

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="what-you-will-deploy"></a>Was Sie bereitstellen möchten

Mit dieser Vorlage stellen Sie eine Web app bereit, die den Code eines Projekts in GitHub enthält.

Wenn die Bereitstellung automatisch ausführen möchten, klicken Sie auf die folgende Schaltfläche:

[![Bereitstellen für Azure](./media/app-service-web-arm-from-github-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-github-deploy%2Fazuredeploy.json)

## <a name="parameters"></a>Parameter

[AZURE.INCLUDE [app-service-web-deploy-web-parameters](../../includes/app-service-web-deploy-web-parameters.md)]

### <a name="repourl"></a>repoURL

Die URL für GitHub Repository mit dem Projekt bereitstellen. Für diesen Parameter enthält einen Standardwert, sondern diesen Wert soll nur zeigen, wie Sie die URL für Repository bereitstellen. Wenn Sie die Vorlage zu testen, verwenden Sie diesen Wert können aber möchten Sie der URL Ihrer eigenen Repository bereitzustellen, bei der Arbeit mit der Vorlage.

    "repoURL": {
        "type": "string",
        "defaultValue": "https://github.com/davidebbo-test/Mvc52Application.git"
    }

### <a name="branch"></a>Verzweigung

Der Zweig des Repositorys zu verwenden, wenn Sie die Anwendung bereitstellen. Der Standardwert ist master, aber Sie können den Namen des keiner Verzweigung im Repository, das Sie bereitstellen möchten bereitstellen.

    "branch": {
        "type": "string",
        "defaultValue": "master"
    }
    
## <a name="resources-to-deploy"></a>Ressourcen für die Bereitstellung

[AZURE.INCLUDE [app-service-web-deploy-web-host](../../includes/app-service-web-deploy-web-host.md)]

### <a name="web-app"></a>Web app

Erstellt die Web-app, die mit dem Projekt in GitHub verknüpft ist. 

Sie geben Sie den Namen der Web-app durch den Parameter **SiteName** und den Speicherort der Web-app durch den Parameter **SiteLocation** . Im **DependsOn** -Element definiert die Vorlage das Web-app als Dienst Hostinganbieter Plan abhängig. Weil es hängt von den Plan Hostinganbieter ist, wird das Web app erst der Hostinganbieter Plan abgeschlossen ist die Erstellung erstellt. Das Element **DependsOn** dient nur Bereitstellungsreihenfolge angeben. Wenn Sie nicht das Web app als abhängig von dem Plan Hostinganbieter aktivieren, Azure Ressource-Manager unter versucht, beide Ressourcen zur gleichen Zeit zu erstellen, und Sie erhalten eine Fehlermeldung, wenn das Web app vor dem Hostinganbieter Plan erstellt wird.

Das Web app hat auch eine untergeordnete Ressource die **Ressourcen** weiter unten im Abschnitt definiert ist. Untergeordnete Ressource definiert Datenquellen-Steuerelements für das Projekt mit der Web-app bereitgestellt. In dieser Vorlage wird das Datenquellen-Steuerelement mit einem bestimmten GitHub Repository verknüpft. GitHub Repository wird durch den Code definiert **"RepoUrl": "https://github.com/davidebbo-test/Mvc52Application.git"** Sie möglicherweise programmiert, die Repository-URL, wenn Sie eine Vorlage erstellen, die ein einzelnes Projekt wiederholt bereitstellt, während die minimale Anzahl von Parametern mit Anforderung der möchten.
Statt Programmieren Repository-URL, können Sie Hinzufügen eines Parameters für die Repository-URL, und verwenden diesen Wert für die Eigenschaft **RepoUrl** .

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('siteName')]",
      "type": "Microsoft.Web/sites",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[resourceId('Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
      ],
      "properties": {
        "serverFarmId": "[parameters('hostingPlanName')]"
      },
      "resources": [
        {
          "apiVersion": "2015-08-01",
          "name": "web",
          "type": "sourcecontrols",
          "dependsOn": [
            "[resourceId('Microsoft.Web/Sites', parameters('siteName'))]"
          ],
          "properties": {
            "RepoUrl": "[parameters('repoURL')]",
            "branch": "[parameters('branch')]",
            "IsManualIntegration": true
          }
        }
      ]
    }

## <a name="commands-to-run-deployment"></a>Befehle zum Ausführen der Bereitstellung

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell

    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-github-deploy/azuredeploy.json -siteName ExampleSite -hostingPlanName ExamplePlan -siteLocation "West US" -ResourceGroupName ExampleDeployGroup

### <a name="azure-cli"></a>Azure CLI

    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-github-deploy/azuredeploy.json


 

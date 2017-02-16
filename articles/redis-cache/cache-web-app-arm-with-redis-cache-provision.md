<properties 
    pageTitle="Bereitstellen von Web App mit Redis Cache" 
    description="Verwenden Sie Ressourcenmanager Azure-Vorlage, um Web app mit Cache Redis bereitstellen." 
    services="app-service" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erickson-doug" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="create-a-web-app-plus-redis-cache-using-a-template"></a>Erstellen einer Web App plus Redis Cache mithilfe einer Vorlage

In diesem Thema erfahren Sie, wie eine Ressourcenmanager Azure-Vorlage erstellen, die einer Azure Web App mit Redis Cache bereitstellt. Sie erfahren, wie um zu definieren, welche Ressourcen bereitgestellt werden und wie Sie Parameter zu definieren, die sind angegeben, wann die Bereitstellung ausgeführt wird. Sie können mithilfe dieser Vorlage für Ihre eigenen Bereitstellungen oder anpassen, um Ihren Anforderungen entsprechen.

Weitere Informationen zum Erstellen von Vorlagen finden Sie unter [Authoring Azure Ressourcenmanager Vorlagen](../resource-group-authoring-templates.md).

Vollständige Vorlage finden Sie unter [Web-App mit Cache Redis Vorlage](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-with-redis-cache/azuredeploy.json).

## <a name="what-you-will-deploy"></a>Was Sie bereitstellen möchten

In dieser Vorlage können Sie bereitstellen:

- Azure Web App
- Azure Redis Cache.

Wenn die Bereitstellung automatisch ausführen möchten, klicken Sie auf die folgende Schaltfläche:

[![Bereitstellen für Azure](./media/cache-web-app-arm-with-redis-cache-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-with-redis-cache%2Fazuredeploy.json)

## <a name="parameters-to-specify"></a>Parameter angeben

[AZURE.INCLUDE [app-service-web-deploy-web-parameters](../../includes/app-service-web-deploy-web-parameters.md)]

[AZURE.INCLUDE [cache-deploy-parameters](../../includes/cache-deploy-parameters.md)]

## <a name="variables-for-names"></a>Variablen für Namen

Diese Vorlage wird mit Variablen um Namen für die Ressourcen zu erstellen. Es wird die Funktion [UniqueString](../resource-group-template-functions.md#uniquestring) Erstellen eines Werts auf Grundlage der Ressource Gruppe Id verwendet.

    "variables": {
      "hostingPlanName": "[concat('hostingplan', uniqueString(resourceGroup().id))]",
      "webSiteName": "[concat('webSite', uniqueString(resourceGroup().id))]",
      "cacheName": "[concat('cache', uniqueString(resourceGroup().id))]"
    },


## <a name="resources-to-deploy"></a>Ressourcen für die Bereitstellung

[AZURE.INCLUDE [app-service-web-deploy-web-host](../../includes/app-service-web-deploy-web-host.md)]

### <a name="redis-cache"></a>Redis Cache

Erstellt die Azure Redis Cache, die mit der Web-app verwendet wird. Der Name des Caches wird in der Variablen **CacheName** angegeben.

Die Vorlage erstellt den Cache am selben Speicherort wie die Ressourcengruppe. 

    {
      "name": "[variables('cacheName')]",
      "type": "Microsoft.Cache/Redis",
      "location": "[resourceGroup().location]",
      "apiVersion": "2015-08-01",
      "dependsOn": [ ],
      "tags": {
        "displayName": "cache"
      },
      "properties": {
        "sku": {
          "name": "[parameters('cacheSKUName')]",
          "family": "[parameters('cacheSKUFamily')]",
          "capacity": "[parameters('cacheSKUCapacity')]"
        }
      }
    }


### <a name="web-app"></a>Web app

Erstellt die Web app mit Namen in der Variablen **WebSiteName** angegeben.

Beachten Sie, dass das Web app mit app Festlegen von Eigenschaften so, die sie konfiguriert ist für die Arbeit mit den Cache Redis aktivieren. Diese app, die Einstellungen dynamisch erstellt werden, basierend auf Werte liegen, während der Bereitstellung.
        
    {
      "apiVersion": "2015-08-01",
      "name": "[variables('webSiteName')]",
      "type": "Microsoft.Web/sites",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Web/serverFarms/', variables('hostingPlanName'))]",
        "[concat('Microsoft.Cache/Redis/', variables('cacheName'))]"
      ],
      "tags": {
        "[concat('hidden-related:', resourceGroup().id, '/providers/Microsoft.Web/serverfarms/', variables('hostingPlanName'))]": "empty",
        "displayName": "Website"
      },
      "properties": {
        "name": "[variables('webSiteName')]",
        "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]"
      },
      "resources": [
        {
          "apiVersion": "2015-08-01",
          "type": "config",
          "name": "appsettings",
          "dependsOn": [
            "[concat('Microsoft.Web/Sites/', variables('webSiteName'))]",
            "[concat('Microsoft.Cache/Redis/', variables('cacheName'))]"
          ],
          "properties": {
            "CacheConnection": "[concat(variables('cacheName'),'.redis.cache.windows.net,abortConnect=false,ssl=true,password=', listKeys(resourceId('Microsoft.Cache/Redis', variables('cacheName')), '2015-08-01').primaryKey)]"
          }
        }
      ]
    }

## <a name="commands-to-run-deployment"></a>Befehle zum Ausführen der Bereitstellung

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell

    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-with-redis-cache/azuredeploy.json -ResourceGroupName ExampleDeployGroup

### <a name="azure-cli"></a>Azure CLI

    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-with-redis-cache/azuredeploy.json -g ExampleDeployGroup



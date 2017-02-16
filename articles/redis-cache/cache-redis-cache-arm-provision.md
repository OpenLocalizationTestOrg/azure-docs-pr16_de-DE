<properties 
    pageTitle="Bereitstellen einen Redis Cache | Microsoft Azure" 
    description="Verwenden Sie Ressourcenmanager Azure-Vorlage, um einen Azure Redis Cache bereitstellen." 
    services="app-service" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="web" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/27/2016" 
    ms.author="sdanie"/>

# <a name="create-a-redis-cache-using-a-template"></a>Erstellen Sie einen Redis Cache mithilfe einer Vorlage

In diesem Thema erfahren Sie, wie eine Ressourcenmanager Azure-Vorlage zu erstellen, die einen Azure Redis Cache bereitstellt. Der Cache kann mit einem vorhandenen Speicherkonto verwendet werden, diagnostische Daten beibehalten. Sie erfahren Sie, wie um zu definieren, welche Ressourcen bereitgestellt werden und wie Parameter zu definieren, die werden angegeben, wann die Bereitstellung ausgeführt wird. Sie können mithilfe dieser Vorlage für Ihre eigenen Bereitstellungen oder anpassen, um Ihren Anforderungen entsprechen.

Diagnoseeinstellungen sind derzeit für alle Caches in derselben Region für ein Abonnement freigegeben. Aktualisieren einer Cache in der Region wirkt sich auf alle anderen Caches in der Region aus.

Weitere Informationen zum Erstellen von Vorlagen finden Sie unter [Authoring Azure Ressourcenmanager Vorlagen](../resource-group-authoring-templates.md).

Vollständige Vorlage finden Sie unter [Cache Redis Vorlage](https://github.com/Azure/azure-quickstart-templates/blob/master/101-redis-cache/azuredeploy.json).

>[AZURE.NOTE] Ressourcenmanager Vorlagen für den neuen [Premium Ebene](cache-premium-tier-intro.md) sind verfügbar. 
>
>-    [Erstellen Sie einen Premium Redis Cache mit Cluster](https://azure.microsoft.com/documentation/templates/201-redis-premium-cluster-diagnostics/)
>-    [Erstellen von Premium Redis Cache mit Beibehaltung der Daten](https://azure.microsoft.com/documentation/templates/201-redis-premium-persistence/)
>-    [Erstellen Sie mit VNet und optional Cluster Premium Redis Cache](https://azure.microsoft.com/documentation/templates/201-redis-premium-vnet-cluster-diagnostics/)
>
>Zum Überprüfen der neuesten Vorlagen finden Sie unter [Azure Schnellstart Vorlagen](https://azure.microsoft.com/documentation/templates/) , und suchen Sie nach `Redis Cache`.

## <a name="what-you-will-deploy"></a>Was Sie bereitstellen möchten

In dieser Vorlage stellen Sie einen Azure Redis Cache bereit, die ein vorhandenes Speicherkonto für diagnostic Daten verwendet.

Wenn die Bereitstellung automatisch ausführen möchten, klicken Sie auf die folgende Schaltfläche:

[![Bereitstellen für Azure](./media/cache-redis-cache-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-redis-cache%2Fazuredeploy.json)

## <a name="parameters"></a>Parameter

Mit Azure Ressourcenmanager, definieren Sie Parameter für Werte, die Sie bei der Bereitstellung der Vorlage angeben möchten. Die Vorlage enthält einen Abschnitt als Parameter bezeichnet, der alle Parameterwerte enthält.
Sie sollten einen Parameter für diese Werte definieren, die variieren basierend auf das Projekt, das Sie bereitstellen oder basierend auf der Umgebung, die, der Sie bereitstellen. Legen Sie keine Parameter für Werte, die immer unverändert bleibt. Jeden Parameterwert wird in der Vorlage verwendet, um die Ressourcen zu definieren, die bereitgestellt werden. 


[AZURE.INCLUDE [app-service-web-deploy-redis-parameters](../../includes/cache-deploy-parameters.md)]

### <a name="rediscachelocation"></a>redisCacheLocation

Die Position des Caches Redis. Verwenden Sie zur Optimierung der Systemleistung am selben Speicherort als die app, mit dem Cache verwendet werden soll.

    "redisCacheLocation": {
      "type": "string"
    }

### <a name="existingdiagnosticsstorageaccountname"></a>existingDiagnosticsStorageAccountName

Der Name des Kontos vorhandenen Speicher für Diagnose verwendet werden soll. 

    "existingDiagnosticsStorageAccountName": {
      "type": "string"
    }

### <a name="enablenonsslport"></a>enableNonSslPort

Der boolesche Wert, der angibt, ob Sie über nicht SSL-Ports zugreifen dürfen.

    "enableNonSslPort": {
      "type": "bool"
    }

### <a name="diagnosticsstatus"></a>diagnosticsStatus

Ein Wert, der angibt, ob die Diagnose aktiviert ist. Verwenden Sie aktivieren oder deaktivieren.

    "diagnosticsStatus": {
      "type": "string",
      "defaultValue": "ON",
      "allowedValues": [
            "ON",
            "OFF"
        ]
    }
    
## <a name="resources-to-deploy"></a>Ressourcen für die Bereitstellung

### <a name="redis-cache"></a>Redis Cache

Wird den Cache Azure Redis erstellt.

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('redisCacheName')]",
      "type": "Microsoft.Cache/Redis",
      "location": "[parameters('redisCacheLocation')]",
      "properties": {
        "enableNonSslPort": "[parameters('enableNonSslPort')]",
        "sku": {
          "capacity": "[parameters('redisCacheCapacity')]",
          "family": "[parameters('redisCacheFamily')]",
          "name": "[parameters('redisCacheSKU')]"
        }
      },
      "resources": [
        {
          "apiVersion": "2015-07-01",
          "type": "Microsoft.Cache/redis/providers/diagnosticsettings",
          "name": "[concat(parameters('redisCacheName'), '/Microsoft.Insights/service')]",
          "location": "[parameters('redisCacheLocation')]",
          "dependsOn": [
            "[concat('Microsoft.Cache/Redis/', parameters('redisCacheName'))]"
          ],
          "properties": {
            "status": "[parameters('diagnosticsStatus')]",
            "storageAccountName": "[parameters('existingDiagnosticsStorageAccountName')]"
          }
        }
      ]
    }



## <a name="commands-to-run-deployment"></a>Befehle zum Ausführen der Bereitstellung

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)] 

### <a name="powershell"></a>PowerShell

    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-redis-cache/azuredeploy.json -ResourceGroupName ExampleDeployGroup -redisCacheName ExampleCache -redisCacheLocation "West US"

### <a name="azure-cli"></a>Azure CLI

    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-redis-cache/azuredeploy.json -g ExampleDeployGroup



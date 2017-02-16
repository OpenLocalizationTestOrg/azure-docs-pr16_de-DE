<properties 
    pageTitle="Erstellen Sie eine Logik App mithilfe von Azure Ressourcenmanager Vorlagen im App-Verwaltungsdienst Azure | Microsoft Azure" 
    description="Verwenden einer Vorlage Azure Ressourcenmanager einer leeren App der Logik zum Definieren von Workflows bereitstellen." 
    services="logic-apps" 
    documentationCenter="" 
    authors="MSFTMan" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/25/2016" 
    ms.author="deonhe"/>

# <a name="create-a-logic-app-using-a-template"></a>Erstellen Sie eine Logik App mithilfe einer Vorlage

Verwenden einer Vorlage Azure Ressourcenmanager eine leeren Logik app zu erstellen, die zum Definieren des Workflows verwendet werden können. Sie können festlegen, welche Ressourcen bereitgestellt werden und wie Sie Parameter zu definieren, die angegeben werden, wenn die Bereitstellung ausgeführt wird. Sie können mithilfe dieser Vorlage für Ihre eigenen Bereitstellungen oder anpassen, um Ihren Anforderungen entsprechen.

Weitere Informationen zu den Eigenschaften, die app Logik finden Sie unter [Logik App Workflow Management API](https://msdn.microsoft.com/library/azure/mt643788.aspx). 

Beispiele für die Definition selbst finden Sie unter [Autor Logik App Definitionen](app-service-logic-author-definitions.md). 

Weitere Informationen zum Erstellen von Vorlagen finden Sie unter [Authoring Azure Ressourcenmanager Vorlagen](../resource-group-authoring-templates.md).

Vollständige Vorlage finden Sie unter [Logik App-Vorlage](https://github.com/Azure/azure-quickstart-templates/blob/master/101-logic-app-create/azuredeploy.json).

## <a name="what-you-will-deploy"></a>Was Sie bereitstellen möchten

Mit dieser Vorlage stellen Sie eine app Logik bereit.

Wenn die Bereitstellung automatisch ausführen möchten, wählen Sie die folgende Schaltfläche:  

[![Bereitstellen für Azure](media/app-service-logic-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-logic-app-create%2Fazuredeploy.json)

## <a name="parameters"></a>Parameter

[AZURE.INCLUDE [app-service-logic-deploy-parameters](../../includes/app-service-logic-deploy-parameters.md)]

### <a name="testuri"></a>testUri

     "testUri": {
        "type": "string",
        "defaultValue": "http://azure.microsoft.com/en-us/status/feed/"
      }
    
## <a name="resources-to-deploy"></a>Ressourcen für die Bereitstellung

### <a name="logic-app"></a>Logik-app

Wird die app Logik erstellt.

Die Vorlagen verwendet einen Parameterwert für den Namen der Anwendung Logik. Die Position der app Logik festgelegt auf am selben Speicherort wie die Ressourcengruppe. 

Diese bestimmten Definition ausgeführt wird einmal pro Stunde und fragt den Speicherort, in den Parameter **TestUri** angegeben. 

    {
      "type": "Microsoft.Logic/workflows",
      "apiVersion": "2016-06-01",
      "name": "[parameters('logicAppName')]",
      "location": "[resourceGroup().location]",
      "tags": {
        "displayName": "LogicApp"
      },
      "properties": {
        "definition": {
          "$schema": "http://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "testURI": {
              "type": "string",
              "defaultValue": "[parameters('testUri')]"
            }
          },
          "triggers": {
            "recurrence": {
              "type": "recurrence",
              "recurrence": {
                "frequency": "Hour",
                "interval": 1
              }
            }
          },
          "actions": {
            "http": {
              "type": "Http",
              "inputs": {
                "method": "GET",
                "uri": "@parameters('testUri')"
              },
              "runAfter": {}
            }
          },
          "outputs": {}
        },
        "parameters": {}
      }
    }


## <a name="commands-to-run-deployment"></a>Befehle zum Ausführen der Bereitstellung

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell

    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json -ResourceGroupName ExampleDeployGroup

### <a name="azure-cli"></a>Azure CLI

    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json -g ExampleDeployGroup


 

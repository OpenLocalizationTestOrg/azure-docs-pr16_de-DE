<properties
    pageTitle="Erstellen Sie einen Ereignis Hubs Namespace mit Ereignis Hub und Consumer Gruppe mithilfe einer Vorlage Ressourcenmanager Azure | Microsoft Azure"
    description="Erstellen Sie einen Ereignis Hubs Namespace mit Ereignis Hub und Consumer Gruppe mit Ressourcenmanager Azure-Vorlage"
    services="event-hubs"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.devlang="tbd"
    ms.topic="article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="08/31/2016"
    ms.author="sethm;shvija"/>

# <a name="create-an-event-hubs-namespace-with-event-hub-and-consumer-group-using-an-azure-resource-manager-template"></a>Erstellen Sie einen Ereignis Hubs Namespace mit Ereignis Hub und Consumer Gruppe mithilfe einer Vorlage Azure Ressourcenmanager

In diesem Artikel veranschaulicht, wie eine Vorlage Ressourcenmanager Azure verwenden möchten, die einen Ereignis Hubs Namespace mit einem Ereignis-Hub und einer Gruppe Consumer erstellt wird. Sie erfahren, wie um zu definieren, welche Ressourcen bereitgestellt werden und wie Sie Parameter zu definieren, die sind angegeben, wann die Bereitstellung ausgeführt wird. Sie können mithilfe dieser Vorlage für Ihre eigenen Bereitstellungen oder anpassen, um Ihren Anforderungen entsprechen.

Weitere Informationen zum Erstellen von Vorlagen finden Sie unter [Azure Ressourcenmanager Authoring-Vorlagen][].

Vollständige Vorlage finden Sie unter [Ereignis Hub und Consumer Gruppenvorlage][] auf GitHub.

>[AZURE.NOTE]
>Um nach der neuesten Vorlagen überprüfen zu können, finden Sie auf den Katalog [Azure Schnellstart Vorlagen][] und suchen Sie nach Ereignis Hubs.

## <a name="what-will-you-deploy"></a>Was möchten Sie bereitstellen?

Mit dieser Vorlage stellen Sie einen Ereignis Hubs Namespace mit einem Ereignis-Hub und einer Gruppe Consumer bereit.

[Hubs Ereignis](../event-hubs/event-hubs-what-is-event-hubs.md) ist ein Ereignis Verarbeitung Service zur Ereignis und werden eingehende Azure bei umfangreichen Skalieren mit niedriger Wartezeit und hohe Zuverlässigkeit zur Verfügung stellen.

Wenn die Bereitstellung automatisch ausführen möchten, klicken Sie auf die folgende Schaltfläche:

[![Bereitstellen für Azure](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-event-hubs-create-event-hub-and-consumer-group%2Fazuredeploy.json)

## <a name="parameters"></a>Parameter

Mit Azure Ressourcenmanager, definieren Sie Parameter für Werte, die Sie bei der Bereitstellung der Vorlage angeben möchten. Die Vorlage enthält einen Abschnitt namens `Parameters` , die alle Parameterwerte enthält. Sie sollten einen Parameter für diese Werte definieren, die variiert basierend auf das Projekt, das Sie bereitstellen oder basierend auf der Umgebung, die, der Sie bereitstellen. Legen Sie keine Parameter für Werte, die immer unverändert bleiben soll. Jeden Parameterwert wird in der Vorlage verwendet, um die Ressourcen zu definieren, die bereitgestellt werden.

Die Vorlage definiert die folgenden Parameter.

### <a name="eventhubnamespacename"></a>eventHubNamespaceName

Der Name des Ereignisses Hubs Namespace zu erstellen.

```
"eventHubNamespaceName": {
"type": "string"
}
```

### <a name="eventhubname"></a>eventHubName

Der Name des Ereignisses Hub erstellt im Hubs Namespace.

```
"eventHubName": {
"type": "string"
}
```

### <a name="eventhubconsumergroupname"></a>eventHubConsumerGroupName

Der Name der Gruppe Consumer für das Ereignis Hub erstellt.

```
"eventHubConsumerGroupName": {
"type": "string"
}
```

### <a name="apiversion"></a>apiVersion

Die API-Version der Vorlage.

```
"apiVersion": {
"type": "string"
}
```

## <a name="resources-to-deploy"></a>Ressourcen für die Bereitstellung

Erstellt einen Namespace des Typs **EventHubs**, mit einem Ereignis-Hub und einer Gruppe Consumer an.

```
"resources":[  
      {  
         "apiVersion":"[variables('ehVersion')]",
         "name":"[parameters('namespaceName')]",
         "type":"Microsoft.EventHub/Namespaces",
         "location":"[variables('location')]",
         "sku":{  
            "name":"Standard",
            "tier":"Standard"
         },
         "resources":[  
            {  
               "apiVersion":"[variables('ehVersion')]",
               "name":"[parameters('eventHubName')]",
               "type":"EventHubs",
               "dependsOn":[  
                  "[concat('Microsoft.EventHub/namespaces/', parameters('namespaceName'))]"
               ],
               "properties":{  
                  "path":"[parameters('eventHubName')]"
               },
               "resources":[  
                  {  
                     "apiVersion":"[variables('ehVersion')]",
                     "name":"[parameters('consumerGroupName')]",
                     "type":"ConsumerGroups",
                     "dependsOn":[  
                        "[parameters('eventHubName')]"
                     ],
                     "properties":{  

                     }
                  }
               ]
            }
         ]
      }
   ],
```

## <a name="commands-to-run-deployment"></a>Befehle zum Ausführen der Bereitstellung

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShell

```
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-event-hubs-create-event-hub-and-consumer-group/azuredeploy.json
```

## <a name="azure-cli"></a>Azure CLI

```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri [https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-event-hubs-create-event-hub-and-consumer-group/azuredeploy.json][]
```

[Authoring Ressourcenmanager Azure-Vorlagen]: ../resource-group-authoring-templates.md
[Schnellstart Azure-Vorlagen]:  https://azure.microsoft.com/documentation/templates/?term=event+hubs
[Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
[Ereignis Hub und Consumer Gruppe-Vorlage]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-event-hubs-create-event-hub-and-consumer-group/

<properties
    pageTitle="Erstellen Sie einen Namespace Dienstbus mit Warteschlange mithilfe einer Vorlage Ressourcenmanager Azure | Microsoft Azure"
    description="Erstellen Sie einen Namespace Dienstbus und einer Warteschlange, indem Ressourcenmanager Azure-Vorlage"
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="tbd"
    ms.topic="article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="10/14/2016"
    ms.author="sethm;shvija"/>

# <a name="create-a-service-bus-namespace-and-a-queue-using-an-azure-resource-manager-template"></a>Erstellen Sie einen Namespace Dienstbus und einer Warteschlange, indem eine Vorlage Azure Ressourcenmanager

In diesem Artikel veranschaulicht, wie eine Vorlage Ressourcenmanager Azure verwenden möchten, die einen Namespace Dienstbus und eine Warteschlange erstellt wird. Sie erfahren, wie um zu definieren, welche Ressourcen bereitgestellt werden und wie Sie Parameter zu definieren, die sind angegeben, wann die Bereitstellung ausgeführt wird. Sie können mithilfe dieser Vorlage für Ihre eigenen Bereitstellungen oder anpassen, um Ihren Anforderungen entsprechen.

Weitere Informationen zum Erstellen von Vorlagen finden Sie unter [Azure Ressourcenmanager Authoring-Vorlagen][].

Vollständige Vorlage finden Sie unter der [Dienstbus Namespace und Warteschlange Vorlage][] auf GitHub.

>[AZURE.NOTE] Die folgenden Azure Ressourcenmanager Vorlagen stehen zum Download zur Verfügung.
>
>-    [Erstellen Sie einen Namespace Dienstbus mit Warteschlange und Autorisierung Regel](service-bus-resource-manager-namespace-auth-rule.md)
>-    [Erstellen Sie einen Namespace Dienstbus mit Thema und Abonnement](service-bus-resource-manager-namespace-topic.md)
>-    [Erstellen Sie einen Namespace Dienstbus](service-bus-resource-manager-namespace.md)
>-    [Erstellen Sie einen Ereignis Hubs Namespace mit einer Gruppe Ereignis Hub und consumer](../event-hubs/event-hubs-resource-manager-namespace-event-hub.md)
>
>Um nach der neuesten Vorlagen überprüfen zu können, finden Sie auf den Katalog [Azure Schnellstart Vorlagen][] und suchen Sie nach "Dienstbus".

## <a name="what-will-you-deploy"></a>Was möchten Sie bereitstellen?

Mit dieser Vorlage stellen Sie einen Dienstbus Namespace mit einer Warteschlange bereit.

[Servicebuswarteschlangen](service-bus-queues-topics-subscriptions.md#queues) Angebot First In Nachrichtenübermittlung für einen oder mehrere verschiedenen Consumer ersten Out (FIFO).

Wenn die Bereitstellung automatisch ausführen möchten, klicken Sie auf die folgende Schaltfläche:

[![Bereitstellen für Azure](./media/service-bus-resource-manager-namespace-queue/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-queue%2Fazuredeploy.json)

## <a name="parameters"></a>Parameter

Mit Azure Ressourcenmanager, definieren Sie Parameter für Werte, die Sie bei der Bereitstellung der Vorlage angeben möchten. Die Vorlage enthält einen Abschnitt namens `Parameters` , die alle Parameterwerte enthält. Sie sollten einen Parameter für diese Werte definieren, die variiert basierend auf das Projekt, das Sie bereitstellen oder basierend auf der Umgebung, die, der Sie bereitstellen. Legen Sie keine Parameter für Werte, die immer unverändert bleiben soll. Jeden Parameterwert wird in der Vorlage verwendet, um die Ressourcen zu definieren, die bereitgestellt werden.

Die Vorlage definiert die folgenden Parameter.

### <a name="servicebusnamespacename"></a>serviceBusNamespaceName

Der Name des Namespace Dienstbus zu erstellen.

```
"serviceBusNamespaceName": {
"type": "string",
"metadata": { 
    "description": "Name of the Service Bus namespace" 
    }
}
```

### <a name="servicebusqueuename"></a>serviceBusQueueName

Der Name der Warteschlange in Dienstbus Namespace erstellt.

```
"serviceBusQueueName": {
"type": "string"
}
```

### <a name="servicebusapiversion"></a>serviceBusApiVersion

Der Dienst Bus API Version der Vorlage.

```
"serviceBusApiVersion": {
"type": "string"
}
```

## <a name="resources-to-deploy"></a>Ressourcen für die Bereitstellung

Erstellt einen standard Dienstbus Namespace des Typs **Messaging**, mit einer Warteschlange an.

```
"resources ": [{
        "apiVersion": "[variables('sbVersion')]",
        "name": "[parameters('serviceBusNamespaceName')]",
        "type": "Microsoft.ServiceBus/Namespaces",
        "location": "[variables('location')]",
        "kind": "Messaging",
        "sku": {
            "name": "StandardSku",
            "tier": "Standard"
        },
        "resources": [{
            "apiVersion": "[variables('sbVersion')]",
            "name": "[parameters('serviceBusQueueName')]",
            "type": "Queues",
            "dependsOn": [
                "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
            ],
            "properties": {
                "path": "[parameters('serviceBusQueueName')]",
            }
        }]
    }]
```

## <a name="commands-to-run-deployment"></a>Befehle zum Ausführen der Bereitstellung

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShell

```
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-queue/azuredeploy.json>
```

## <a name="azure-cli"></a>Azure CLI

```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-queue/azuredeploy.json>
```

## <a name="next-steps"></a>Nächste Schritte

Jetzt, da Sie erstellt und Ressourcen mithilfe von Azure Ressourcenmanager bereitgestellt haben, erfahren Sie, wie diese Ressourcen verwalten, indem Sie die folgenden Artikel:

- [Verwalten von Dienstbus mit PowerShell](service-bus-powershell-how-to-provision.md)
- [Verwalten von Dienstbus Ressourcen mit dem Dienst Bus-Explorer](https://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)


  [Authoring Ressourcenmanager Azure-Vorlagen]: ../resource-group-authoring-templates.md
  [Dienstbus Namespace und Warteschlange-Vorlage]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/
  [Schnellstart Azure-Vorlagen]: https://azure.microsoft.com/documentation/templates/?term=service+bus
  [Learn more about Service Bus queues]: service-bus-queues-topics-subscriptions.md
  [Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
  [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md

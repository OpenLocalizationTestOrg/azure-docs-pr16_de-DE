<properties
    pageTitle="Erstellen einer Dienstbus Autorisierungsregel mithilfe einer Vorlage Ressourcenmanager Azure | Microsoft Azure"
    description="Erstellen einer Dienstbus Autorisierungsregel für Namespace und Warteschlange mit Ressourcenmanager Azure-Vorlage"
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

# <a name="create-a-service-bus-authorization-rule-for-namespace-and-queue-using-an-azure-resource-manager-template"></a>Erstellen einer Dienstbus Autorisierungsregel für Namespace und Warteschlange mithilfe einer Vorlage Azure Ressourcenmanager

In diesem Artikel veranschaulicht, wie eine Vorlage Ressourcenmanager Azure verwenden möchten, die eine [Autorisierungsregel](service-bus-authentication-and-authorization.md#shared-access-signature-authentication) für einen Namespace Dienstbus und Warteschlange erstellt wird. Sie erfahren, wie um zu definieren, welche Ressourcen bereitgestellt werden und wie Sie Parameter zu definieren, die sind angegeben, wann die Bereitstellung ausgeführt wird. Sie können mithilfe dieser Vorlage für Ihre eigenen Bereitstellungen oder anpassen, um Ihren Anforderungen entsprechen.

Weitere Informationen zum Erstellen von Vorlagen finden Sie unter [Azure Ressourcenmanager Authoring-Vorlagen][].

Vollständige Vorlage finden Sie unter der [Dienstbus autorisierende Regelvorlage][] auf GitHub.

>[AZURE.NOTE] Die folgenden Azure Ressourcenmanager Vorlagen stehen zum Download zur Verfügung.
>
>-    [Erstellen Sie einen Ereignis Hubs Namespace mit einer Gruppe Ereignis Hub und consumer](../event-hubs/event-hubs-resource-manager-namespace-event-hub.md)
>-    [Erstellen Sie einen Namespace Dienstbus mit Warteschlange](service-bus-resource-manager-namespace-queue.md)
>-    [Erstellen Sie einen Namespace Dienstbus mit Thema und Abonnement](service-bus-resource-manager-namespace-topic.md)
>-    [Erstellen Sie einen Namespace Dienstbus](service-bus-resource-manager-namespace.md)
>
>Um nach der neuesten Vorlagen überprüfen zu können, finden Sie auf den Katalog [Azure Schnellstart Vorlagen][] und suchen Sie nach "Dienstbus".

## <a name="what-will-you-deploy"></a>Was möchten Sie bereitstellen?

Stellen Sie bei Verwendung dieser Vorlage eine Dienstbus Autorisierungsregel für einen Namespace und messaging Entität (in diesem Fall einer Warteschlange) bereit.

Diese Vorlage verwendet [Freigegebene Access Signatur (SAS)](service-bus-sas-overview.md) für die Authentifizierung ein. SAS ermöglicht Applikationen unter Verwendung einer Zugriffstaste so konfiguriert, dass auf den Namespace oder in der messaging Entität (Warteschlange oder Thema) denen bestimmte Rechte zugeordnet sind mit Dienst authentifizieren. Dieser Taste können dann ein Token SAS generieren, die Clients wiederum Dienstbus authentifizieren verwenden können.

Wenn die Bereitstellung automatisch ausführen möchten, klicken Sie auf die folgende Schaltfläche:

[![Bereitstellen für Azure](./media/service-bus-resource-manager-namespace-auth-rule/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F301-servicebus-create-authrule-namespace-and-queue%2Fazuredeploy.json)

## <a name="parameters"></a>Parameter

Mit Azure Ressourcenmanager, definieren Sie Parameter für Werte, die Sie bei der Bereitstellung der Vorlage angeben möchten. Die Vorlage enthält einen Abschnitt namens `Parameters` , die alle Parameterwerte enthält. Sie sollten einen Parameter für diese Werte definieren, die variiert basierend auf das Projekt, das Sie bereitstellen oder basierend auf der Umgebung, die, der Sie bereitstellen. Legen Sie keine Parameter für Werte, die immer unverändert bleiben soll. Jeden Parameterwert wird in der Vorlage verwendet, um die Ressourcen zu definieren, die bereitgestellt werden.

Die Vorlage definiert die folgenden Parameter.

### <a name="servicebusnamespacename"></a>serviceBusNamespaceName

Der Name des Namespace Dienstbus zu erstellen.

```
"serviceBusNamespaceName": {
"type": "string"
}
```

### <a name="namespaceauthorizationrulename"></a>namespaceAuthorizationRuleName 

Der Name der Autorisierungsregel für den Namespace.

```
"namespaceAuthorizationRuleName ": {
"type": "string"
}
```

### <a name="servicebusqueuename"></a>serviceBusQueueName

Der Name der Warteschlange im Dienstbus Namespace.

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

Erstellt einen standard Dienstbus Namespace des Typs **Messaging**und eine Dienstbus Autorisierungsregel für Namespace und Entität.

```
"resources": [
        {
            "apiVersion": "[variables('sbVersion')]",
            "name": "[parameters('serviceBusNamespaceName')]",
            "type": "Microsoft.ServiceBus/namespaces",
            "location": "[variables('location')]",
            "kind": "Messaging",
            "sku": {
                "name": "StandardSku",
                "tier": "Standard"
            },
            "resources": [
                {
                    "apiVersion": "[variables('sbVersion')]",
                    "name": "[parameters('serviceBusQueueName')]",
                    "type": "Queues",
                    "dependsOn": [
                        "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
                    ],
                    "properties": {
                        "path": "[parameters('serviceBusQueueName')]"
                    },
                    "resources": [
                        {
                            "apiVersion": "[variables('sbVersion')]",
                            "name": "[parameters('queueAuthorizationRuleName')]",
                            "type": "authorizationRules",
                            "dependsOn": [
                                "[parameters('serviceBusQueueName')]"
                            ],
                            "properties": {
                                "Rights": ["Listen"]
                            }
                        }
                    ]
                }
            ]
        }, {
            "apiVersion": "[variables('sbVersion')]",
            "name": "[variables('namespaceAuthRuleName')]",
            "type": "Microsoft.ServiceBus/namespaces/authorizationRules",
            "dependsOn": ["[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"],
            "location": "[resourceGroup().location]",
            "properties": {
                "Rights": ["Send"]
            }
        }
    ]
```

## <a name="commands-to-run-deployment"></a>Befehle zum Ausführen der Bereitstellung

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell

```
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/301-servicebus-create-authrule-namespace-and-queue/azuredeploy.json>
```

## <a name="azure-cli"></a>Azure CLI

```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/301-servicebus-create-authrule-namespace-and-queue/azuredeploy.json>
```

## <a name="next-steps"></a>Nächste Schritte

Jetzt, da Sie erstellt und Ressourcen mithilfe von Azure Ressourcenmanager bereitgestellt haben, erfahren Sie, wie diese Ressourcen verwalten, indem Sie die folgenden Artikel:

- [Verwalten von Dienstbus mit PowerShell](service-bus-powershell-how-to-provision.md)
- [Verwalten von Dienstbus Ressourcen mit dem Dienst Bus-Explorer](https://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)
- [Dienstbus Authentifizierung und Autorisierung](service-bus-authentication-and-authorization.md)

  [Authoring Ressourcenmanager Azure-Vorlagen]: ../resource-group-authoring-templates.md
  [Schnellstart Azure-Vorlagen]: https://azure.microsoft.com/documentation/templates/?term=service+bus
  [Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
  [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
  [Dienstbus autorisierende Regelvorlage]: https://github.com/Azure/azure-quickstart-templates/blob/master/301-servicebus-create-authrule-namespace-and-queue/

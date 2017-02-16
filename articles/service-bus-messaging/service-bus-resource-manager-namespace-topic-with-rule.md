<properties
    pageTitle="Erstellen Sie einen Namespace Dienstbus mit Thema Abonnement, und Regel mithilfe einer Vorlage Ressourcenmanager Azure | Microsoft Azure"
    description="Erstellen Sie einen Namespace Dienstbus mit Thema, Abonnement und Regel Ressourcenmanager Azure-Vorlage"
    services="service-bus"
    documentationCenter=".net"
    authors="ShubhaVijayasarathy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="tbd"
    ms.topic="article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="10/25/2016"
    ms.author="ShubhaVijayasarathy"/>

# <a name="create-a-service-bus-namespace-with-topic-subscription-and-rule-using-an-azure-resource-manager-template"></a>Erstellen Sie einen Namespace Dienstbus mit Thema, Abonnement und Regel mithilfe einer Vorlage Azure Ressourcenmanager

In diesem Artikel veranschaulicht, wie eine Ressourcenmanager Azure-Vorlage verwenden, die einen Dienstbus Namespace mit einem Thema, Abonnement und Regel (Filter) erstellt. Sie erfahren, wie um zu definieren, welche Ressourcen bereitgestellt werden und wie Sie Parameter zu definieren, die sind angegeben, wann die Bereitstellung ausgeführt wird. Sie können mithilfe dieser Vorlage für Ihre eigenen Bereitstellungen oder anpassen, um Ihren Anforderungen entsprechen.

Weitere Informationen zum Erstellen von Vorlagen finden Sie unter [Azure Ressourcenmanager Authoring-Vorlagen][].

Weitere Informationen zum Übungsbeispiel und Muster werden für Ressourcen Azure Benennungskonventionen, finden Sie unter [Azure Ressourcen Benennungskonventionen][].

Vollständige Vorlage finden Sie unter der Vorlage [Dienstbus Namespace mit Thema, Abonnements, und Regel][] .

>[AZURE.NOTE] Die folgenden Azure Ressourcenmanager Vorlagen werden zum Download zur Verfügung.
>
>-    [Erstellen Sie einen Namespace Dienstbus mit Warteschlange und Autorisierung Regel](service-bus-resource-manager-namespace-auth-rule.md)
>-    [Erstellen Sie einen Namespace Dienstbus mit Warteschlange](service-bus-resource-manager-namespace-queue.md)
>-    [Erstellen Sie einen Namespace Dienstbus](service-bus-resource-manager-namespace.md)
>-    [Erstellen Sie einen Namespace Dienstbus mit Thema und Abonnement](service-bus-resource-manager-namespace-topic.md)
>
>Zum Überprüfen der neuesten Vorlagen finden Sie auf den Katalog [Azure Schnellstart Vorlagen][] und suchen Sie nach Dienstbus.

## <a name="what-will-you-deploy"></a>Was möchten Sie bereitstellen?

Mit dieser Vorlage stellen Sie einen Namespace Dienstbus mit Thema, Abonnement und Regel (Filter).

[Dienstbus Themen und Abonnements](service-bus-queues-topics-subscriptions.md#topics-and-subscriptions) bieten eine 1: n-Formular Kommunikationsmethode, in einem Muster *Veröffentlichen/Abonnieren* . Bei Verwendung von Themen und Abonnements, die Komponenten einer verteilten Anwendung kommunizieren nicht direkt miteinander, stattdessen exchange diese Nachrichten über das Thema, das als ein. Ein Abonnement zu einem Thema ähnelt eine virtuelle Warteschlange, die Kopien der Nachrichten empfangen werden, die im Thema gesendet wurden. Eines Filters auf Abonnement ermöglicht sollten Sie festlegen, welche Nachrichten gesendet zu einem Thema innerhalb eines bestimmten Thema Abonnements angezeigt.

## <a name="what-are-rules-filters"></a>Wie lauten die Regeln (Filter)?

In vielen Szenarios müssen Nachrichten mit bestimmten Merkmale auf unterschiedliche Weise verarbeitet werden. Um dies zu aktivieren, können Sie Abonnements, um Nachrichten zu finden, die Eigenschaften haben gewünscht, und führen Sie dann auf bestimmte Änderungen an diesen Eigenschaften konfigurieren. Während Dienstbus Abonnements alle Nachrichten mit dem Thema angezeigt wird, können Sie nur eine Teilmenge dieser Nachrichten an die Warteschlange virtuelle Abonnement kopieren. Dies geschieht Abonnementfilter verwenden. Weitere Informationen zum rules(filters) finden Sie unter [Servicebuswarteschlangen, Themen, und Abonnements][].

Wenn die Bereitstellung automatisch ausführen möchten, klicken Sie auf die folgende Schaltfläche:

[![Bereitstellen für Azure](./media/service-bus-resource-manager-namespace-topic/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-topic-subscription-rule%2Fazuredeploy.json)

## <a name="parameters"></a>Parameter

Mit Azure Ressourcenmanager, sollten Sie Parameter für Werte definieren, die Sie bei der Bereitstellung der Vorlage angeben möchten. Die Vorlage enthält einen Abschnitt namens `Parameters` , die alle Parameterwerte enthält. Sie sollten einen Parameter für diese Werte definieren, die variieren basierend auf das Projekt, das Sie bereitstellen oder basierend auf der Umgebung, die, der Sie bereitstellen. Legen Sie keine Parameter für Werte, die immer unverändert bleibt. Jeden Parameterwert wird in der Vorlage verwendet, um die Ressourcen zu definieren, die bereitgestellt werden.

Die Vorlage definiert die folgenden Parameter:

### <a name="servicebusnamespacename"></a>serviceBusNamespaceName

Der Name des Namespace Dienstbus erstellen.

```
"serviceBusNamespaceName": {
"type": "string"
}
```

### <a name="servicebustopicname"></a>serviceBusTopicName

Der Name des Themas in Dienstbus Namespace erstellt.

```
"serviceBusTopicName": {
"type": "string"
}
```

### <a name="servicebussubscriptionname"></a>serviceBusSubscriptionName

Der Name des Abonnements in Dienstbus Namespace erstellt.

```
"serviceBusSubscriptionName": {
"type": "string"
}
```
### <a name="servicebusrulename"></a>serviceBusRuleName

Der Name der rule(filter) in Dienstbus Namespace erstellt.

```
   "serviceBusRuleName": {
   "type": "string",
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

Erstellt einen standard Dienstbus Namespace des Typs **Messaging**, mit dem Thema und Abonnement und Regeln.

```
 "resources": [{
        "apiVersion": "[variables('sbVersion')]",
        "name": "[parameters('serviceBusNamespaceName')]",
        "type": "Microsoft.ServiceBus/Namespaces",
        "location": "[variables('location')]",
        "sku": {
            "name": "Standard",
            "tier": "Standard"
        },
        "resources": [{
            "apiVersion": "[variables('sbVersion')]",
            "name": "[parameters('serviceBusTopicName')]",
            "type": "Topics",
            "dependsOn": [
                "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
            ],
            "properties": {
                "path": "[parameters('serviceBusTopicName')]"
            },
            "resources": [{
                "apiVersion": "[variables('sbVersion')]",
                "name": "[parameters('serviceBusSubscriptionName')]",
                "type": "Subscriptions",
                "dependsOn": [
                    "[parameters('serviceBusTopicName')]"
                ],
                "properties": {},
                "resources": [{
                    "apiVersion": "[variables('sbVersion')]",
                    "name": "[parameters('serviceBusRuleName')]",
                    "type": "Rules",
                    "dependsOn": [
                        "[parameters('serviceBusSubscriptionName')]"
                    ],
                    "properties": {
                        "filter": {
                            "sqlExpression": "StoreName = 'Store1'"
                        },
                        "action": {
                            "sqlExpression": "set FilterTag = 'true'"
                        }
                    }
                }]
            }]
        }]
    }]
```

## <a name="commands-to-run-deployment"></a>Befehle zum Ausführen der Bereitstellung

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShell

```
New-AzureResourceGroupDeployment -Name \<deployment-name\> -ResourceGroupName \<resource-group-name\> -TemplateUri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-subscription-rule/azuredeploy.json>
```

## <a name="azure-cli"></a>Azure CLI

```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-subscription-rule/azuredeploy.json>
```

## <a name="next-steps"></a>Nächste Schritte

Jetzt, da Sie erstellt und Ressourcen mithilfe von Azure Ressourcenmanager bereitgestellt haben, erfahren Sie, wie diese Ressourcen verwalten, indem Sie die folgenden Artikel:

- [Verwalten von Azure Service Bus mit Azure Automatisierung](service-bus-automation-manage.md)
- [Verwalten von Dienstbus mit PowerShell](service-bus-powershell-how-to-provision.md)
- [Verwalten von Dienstbus Ressourcen mit dem Dienst Bus-Explorer](https://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)


  [Authoring Ressourcenmanager Azure-Vorlagen]: ../resource-group-authoring-templates.md
  [Schnellstart Azure-Vorlagen]: https://azure.microsoft.com/documentation/templates/?term=service+bus
  [Learn more about Service Bus topics and subscriptions]: service-bus-queues-topics-subscriptions.md
  [Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
  [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
  [Azure Benennungskonventionen Ressourcen]: https://azure.microsoft.com/en-us/documentation/articles/guidance-naming-conventions/
  [Dienstbus Namespace mit Thema, Abonnement und Regel]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-topic-subscription-rule/
  [Servicebuswarteschlangen, Themen und Abonnements]:service-bus-queues-topics-subscriptions.md
  

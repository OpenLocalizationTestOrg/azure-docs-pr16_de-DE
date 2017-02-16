<properties
    pageTitle="Dienstbus Ressourcen mithilfe von Azure Ressourcenmanager Vorlagen erstellen | Microsoft Azure"
    description="Verwenden Sie Ressourcenmanager Azure-Vorlagen, um die Erstellung von Ressourcen Dienstbus automatisieren"
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
    ms.author="sethm"/>

# <a name="create-service-bus-resources-using-azure-resource-manager-templates"></a>Erstellen von Dienstbus Ressourcen mithilfe von Ressourcenmanager Azure-Vorlagen

Dieser Artikel beschreibt das Erstellen und Bereitstellen von Dienstbus und Ereignis Hubs Ressourcen mithilfe von Vorlagen, PowerShell oder der Anbieter für Ressourcen Dienstbus Azure Ressourcenmanager.

Azure Ressourcenmanager Vorlagen helfen Sie definieren die Ressourcen für eine Lösung bereitstellen, und um anzugeben, Parameter und Variablen, mit die Sie Werte für die verschiedenen Umgebungen eingeben können. Die Vorlage besteht aus JSON und Ausdrücke, die Sie verwenden können, um Werte für die Bereitstellung zu erstellen. Ausführliche Informationen zum Schreiben von Azure Ressourcenmanager Vorlagen und eine Beschreibung des Formats Vorlage finden Sie unter [Azure Ressourcenmanager Authoring-Vorlagen](../resource-group-authoring-templates.md). 

>[AZURE.NOTE] In den Beispielen in diesem Artikel wird die so Ressourcenmanager Azure verwenden, um einen Namespace Dienstbus und messaging Entität (Warteschlange) zu erstellen. Andere Beispiele Vorlage finden Sie auf den [Katalog Azure Schnellstart Vorlagen][] und suchen Sie nach "Dienstbus".

## <a name="service-bus-and-event-hubs-resource-manager-templates"></a>Dienstbus und Ereignis Hubs Ressourcenmanager Vorlagen

Diese Vorlagen Dienstbus und Ereignis Hubs Azure Ressourcenmanager stehen zum Download zur Verfügung. Klicken Sie auf die folgenden Links für jede Datei mit Links zu den Vorlagen in GitHub Details: 

- [Erstellen Sie einen Namespace Dienstbus](service-bus-resource-manager-namespace.md)
- [Erstellen Sie einen Namespace Dienstbus mit Warteschlange](service-bus-resource-manager-namespace-queue.md)
- [Erstellen Sie einen Namespace Dienstbus mit Thema und Abonnement](service-bus-resource-manager-namespace-topic.md)
- [Erstellen Sie einen Namespace Dienstbus mit Warteschlange und Autorisierung Regel](service-bus-resource-manager-namespace-auth-rule.md)
- [Erstellen Sie einen Ereignis Hubs Namespace mit einer Gruppe Ereignis Hub und consumer](../event-hubs/event-hubs-resource-manager-namespace-event-hub.md)

## <a name="deploy-with-powershell"></a>Bereitstellen mit PowerShell

Das folgende Verfahren beschreibt das PowerShell verwenden, um eine Vorlage Ressourcenmanager Azure bereitgestellt werden, die einem **Standard** Ebene Dienstbus Namespace und einer Warteschlange innerhalb dieses Namespaces erstellt wird. In diesem Beispiel basiert auf der Vorlage [Erstellen Sie einen Namespace Dienstbus mit Warteschlange](https://github.com/Azure/azure-quickstart-templates/tree/master/201-servicebus-create-queue) . Der Workflow ungefähre lautet wie folgt aus:

1. Installieren der PowerShell.
2. Erstellen Sie die Vorlage und (optional) eine Parameterdatei ein.
2. In PowerShell melden Sie sich bei Ihrem Azure-Konto an.
3. Erstellen einer neuen Ressourcengruppe an, wenn eine nicht vorhanden ist.
4. Testen der Bereitstellung.
5. Wenn gewünscht, legen Sie den Modus für die Bereitstellung.
6. Stellen Sie die Vorlage ein.

Ausführliche Informationen zum Bereitstellen von Vorlagen Azure Ressourcenmanager finden Sie unter [Bereitstellen von Ressourcen mit Azure Ressourcenmanager Vorlagen][].

### <a name="install-powershell"></a>Installieren der PowerShell

Installieren Sie anhand der Anweisungen in [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md)Azure PowerShell.

### <a name="create-a-template"></a>Erstellen einer Vorlage

Klonen Sie, oder kopieren Sie die Vorlage [201 Servicebus erstellen Warteschlange](https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/azuredeploy.json) aus GitHub:

```
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "serviceBusNamespaceName": {
            "type": "string",
            "metadata": {
                "description": "Name of the Service Bus namespace"
            }
        },
        "serviceBusQueueName": {
            "type": "string",
            "metadata": {
                "description": "Name of the Queue"
            }
        },
        "serviceBusApiVersion": {
            "type": "string",
            "defaultValue": "2015-08-01",
            "metadata": {
                "description": "Service Bus ApiVersion used by the template"
            }
        }
    },
    "variables": {
        "location": "[resourceGroup().location]",
        "sbVersion": "[parameters('serviceBusApiVersion')]",
        "defaultSASKeyName": "RootManageSharedAccessKey",
        "authRuleResourceId": "[resourceId('Microsoft.ServiceBus/namespaces/authorizationRules', parameters('serviceBusNamespaceName'), variables('defaultSASKeyName'))]"
    },
    "resources": [{
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
                "path": "[parameters('serviceBusQueueName')]"
            }
        }]
    }],
    "outputs": {
        "NamespaceConnectionString": {
            "type": "string",
            "value": "[listkeys(variables('authRuleResourceId'), variables('sbVersion')).primaryConnectionString]"
        },
        "SharedAccessPolicyPrimaryKey": {
            "type": "string",
            "value": "[listkeys(variables('authRuleResourceId'), variables('sbVersion')).primaryKey]"
        }
    }
}
```

### <a name="create-a-parameters-file-optional"></a>Erstellen Sie eine Parameterdatei (optional)

Wenn eine Datei optionale Parameter verwenden möchten, kopieren Sie die [201 Servicebus erstellen Warteschlange](https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/azuredeploy.parameters.json) -Datei ein. Ersetzen Sie den Wert der `serviceBusNamespaceName` mit dem Namen des Namespace Dienstbus in dieser Bereitstellung zu erstellen, und Ersetzen Sie den Wert der gewünschten `serviceBusQueueName` mit dem Namen der zu erstellenden Warteschlange. 

```
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "serviceBusNamespaceName": {
            "value": "<myNamespaceName>"
        },
        "serviceBusQueueName": {
            "value": "<myQueueName>"
        },
        "serviceBusApiVersion": {
            "value": "2015-08-01"
        }
    }
}
```

Weitere Informationen finden Sie unter dem Thema [Parameter-Datei](../resource-group-template-deploy.md#parameter-file) .

### <a name="log-in-to-azure-and-set-the-azure-subscription"></a>Melden Sie sich bei Azure und Festlegen des Azure-Abonnements

Führen Sie aus einer PowerShell dazu aufgefordert werden den folgenden Befehl aus:

```
Login-AzureRmAccount
```

Aufgefordert werden, melden Sie sich bei Ihrem Konto Azure. Führen Sie nach der Anmeldung den folgenden Befehl zum Anzeigen Ihrer Abonnements zur Verfügung.

```
Get-AzureRMSubscription
```

Dieser Befehl gibt eine Liste der verfügbaren Azure-Abonnements. Wählen Sie ein Abonnement für die aktuelle Sitzung, indem Sie den folgenden Befehl ausführen. Ersetzen Sie `<YourSubscriptionId>` mit der GUID für das Azure Abonnement, die Sie verwenden möchten.

```
Set-AzureRmContext -SubscriptionID <YourSubscriptionId>
```

### <a name="set-the-resource-group"></a>Festlegen der Ressourcengruppe

Wenn Sie nicht über eine vorhandene Ressourcengruppe verfügen, erstellen Sie eine neue Ressourcengruppe mit dem Befehl **New-AzureRmResourceGroup** . Geben Sie den Namen der Ressourcengruppe und Speicherort, die Sie verwenden möchten. Beispiel:

```
New-AzureRmResourceGroup -Name MyDemoRG -Location "West US"
```

Wenn der Vorgang erfolgreich ist, wird eine Zusammenfassung der neuen Ressourcengruppe angezeigt.

```
ResourceGroupName : MyDemoRG
Location          : westus
ProvisioningState : Succeeded
Tags              :
ResourceId        : /subscriptions/<GUID>/resourceGroups/MyDemoRG
```

### <a name="test-the-deployment"></a>Testen der Bereitstellung

Überprüfen Sie Ihre Bereitstellung durch Ausführen der `Test-AzureRmResourceGroupDeployment` Cmdlet. Wenn Sie die Bereitstellung zu testen, geben Sie Parameter genau wie beim Ausführen der bereitstellungs.

```
Test-AzureRmResourceGroupDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json
```

### <a name="create-the-deployment"></a>Erstellen der bereitstellungs

Führen Sie zum Erstellen der neuen bereitstellungs der `New-AzureRmResourceGroupDeployment` Befehl aus, und geben Sie die erforderlichen Parameter, wenn Sie dazu aufgefordert werden. Die Parameter umfassen einen Namen für die Bereitstellung, den Namen der Ressourcengruppe und den Pfad oder URL der Vorlagendatei. Wenn der Parameter **Modus** nicht angegeben ist, wird der Standardwert **inkrementell** verwendet. Weitere Informationen finden Sie unter [inkrementell und Bereitstellungen abgeschlossen](../resource-group-template-deploy.md#incremental-and-complete-deployments).

Mit dem folgende Befehl werden Sie aufgefordert, die drei Parameter im PowerShell-Fenster:

```
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json
```

Wenn Sie eine Parameterdatei stattdessen angeben möchten, verwenden Sie den folgenden Befehl ein.

```
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json -TemplateParameterFile <path to parameters file>\azuredeploy.parameters.json
```

Sie können auch Inline-Parameter verwenden, wenn Sie das Cmdlet Bereitstellung ausführen. Der Befehl ist wie folgt aus:

```
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json -parameterName "parameterValue"
```

Führen Sie eine [umfassende](../resource-group-template-deploy.md#incremental-and-complete-deployments) Bereitstellung wird den **Modus** -Parameter auf **abgeschlossen**fest.

```
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -Mode Complete -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json 
```

### <a name="verify-the-deployment"></a>Überprüfen der bereitstellungs

Wenn die Ressourcen erfolgreich bereitgestellt werden, wird eine Zusammenfassung der Bereitstellung im PowerShell-Fenster angezeigt:

```
DeploymentName    : MyDemoDeployment
ResourceGroupName : MyDemoRG
ProvisioningState : Succeeded
Timestamp         : 4/19/2016 10:38:30 PM
Mode              : Incremental
TemplateLink      :
Parameters        :
                    Name             Type                       Value
                    ===============  =========================  ==========
                    serviceBusNamespaceName  String             <namespaceName>
                    serviceBusQueueName  String                 <queueName>
                    serviceBusApiVersion  String                2015-08-01

```

## <a name="next-steps"></a>Nächste Schritte

Jetzt haben Sie die grundlegende Workflow und die Befehle für die Bereitstellung von einer Vorlage Azure Ressourcenmanager angezeigt. Ausführlichere Informationen finden Sie auf die folgenden Links:

- [Azure Ressourcenmanager (Übersicht)][]
- [Bereitstellen von Ressourcen mit Azure Ressourcenmanager Vorlagen][]
- [Erstellung von Vorlagen](../resource-group-authoring-templates.md)


[Azure Ressourcenmanager (Übersicht)]: ../resource-group-overview.md
[Bereitstellen von Ressourcen mit Azure Ressourcenmanager Vorlagen]: ../resource-group-template-deploy.md
[Vorlagenkatalog Azure Schnellstart]: https://azure.microsoft.com/documentation/templates/?term=service+bus
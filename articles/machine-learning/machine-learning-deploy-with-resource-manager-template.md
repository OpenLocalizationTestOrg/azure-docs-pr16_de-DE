<properties
    pageTitle="Computer mit Azure Ressourcenmanager Vorlage Arbeitsbereich Learning bereitstellen | Microsoft Azure"
    description="Zum Bereitstellen eines Arbeitsbereichs für Azure maschinellen Learning mit Ressourcenmanager Azure-Vorlage"
    services="machine-learning"
    documentationCenter=""
    authors="ahgyger"
    manager="haining"
    editor="garye"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/23/2016"
    ms.author="ahgyger"/>
# <a name="deploy-machine-learning-workspace-using-azure-resource-manager"></a>Bereitstellen von maschinellen Learning Arbeitsbereich mit Azure Ressourcenmanager

## <a name="introduction"></a>Einführung
Verwenden einer Vorlage für Bereitstellung Zeit speichert, indem Sie eine skalierbare Möglichkeit zum Erteilen Azure-Ressourcenmanager bereitstellen Sie verbundene Komponenten mit einer Überprüfung, und wiederholen Sie Verfahren. Um Azure maschinellen Learning Arbeitsbereiche eingerichtet haben, müssen Sie beispielsweise zuerst ein Konto Azure-Speicher konfigurieren und Bereitstellen klicken Sie dann auf den Arbeitsbereich. Stellen Sie sich vor Hiermit für hundert Arbeitsbereiche manuell. Einfachere Alternative ist die Verwendung eine Vorlage Ressourcenmanager Azure ein Arbeitsbereich Azure Computer lernen und sämtlichen abhängigen bereitstellen. In diesem Artikel führt Sie durch diesen Schritt für Schritt. Eine gute Übersicht der Azure Ressourcenmanager finden Sie unter [Übersicht Azure Ressourcenmanager](../azure-resource-manager/resource-group-overview.md).

## <a name="step-by-step-create-a-machine-learning-workspace"></a>Schrittweise: Erstellen eines Computer Learning-Arbeitsbereichs
Wir wird eine Azure Ressourcengruppe erstellen und dann ein neues Azure-Speicher-Konto und einen neuen Azure maschinellen Learning Arbeitsbereich mithilfe einer Vorlage Ressourcenmanager bereitstellen. Nachdem die Bereitstellung abgeschlossen ist, werden wir wichtige Informationen zu den Arbeitsbereichen ausdrucken, die (den Primärschlüssel, die WorkspaceID und die URL für den Arbeitsbereich) erstellt wurden.

### <a name="create-an-azure-resource-manager-template"></a>Erstellen einer Vorlage für Azure Ressourcenmanager
Einen Computer Learning Arbeitsbereich erfordert ein Konto Azure-Speicher, damit das Dataset verknüpft sein.
Die folgende Vorlage verwendet den Namen der Ressourcengruppe den Kontonamen Speicher generieren und den Arbeitsbereichsnamen.  Auch verwendet den Kontonamen Speicher als eine Eigenschaft, wenn den Arbeitsbereich erstellen.

```
{
    "contentVersion": "1.0.0.0",
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "variables": {
        "namePrefix": "[resourceGroup().name]",
        "location": "[resourceGroup().location]",
        "mlVersion": "2016-04-01",
        "stgVersion": "2015-06-15",
        "storageAccountName": "[concat(variables('namePrefix'),'stg')]",
        "mlWorkspaceName": "[concat(variables('namePrefix'),'mlwk')]",
        "mlResourceId": "[resourceId('Microsoft.MachineLearning/workspaces', variables('mlWorkspaceName'))]",
        "stgResourceId": "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]",
        "storageAccountType": "Standard_LRS"
    },
    "resources": [
        {
            "apiVersion": "[variables('stgVersion')]",
            "name": "[variables('storageAccountName')]",
            "type": "Microsoft.Storage/storageAccounts",
            "location": "[variables('location')]",
            "properties": {
                "accountType": "[variables('storageAccountType')]"
            }
        },
        {
            "apiVersion": "[variables('mlVersion')]",
            "type": "Microsoft.MachineLearning/workspaces",
            "name": "[variables('mlWorkspaceName')]",
            "location": "[variables('location')]",
            "dependsOn": ["[variables('stgResourceId')]"],
            "properties": {
                "UserStorageAccountId": "[variables('stgResourceId')]"
            }
        }
    ],
    "outputs": {
        "mlWorkspaceObject": {"type": "object", "value": "[reference(variables('mlResourceId'), variables('mlVersion'))]"},
        "mlWorkspaceToken": {"type": "string", "value": "[listWorkspaceKeys(variables('mlResourceId'), variables('mlVersion')).primaryToken]"},
        "mlWorkspaceWorkspaceID": {"type": "string", "value": "[reference(variables('mlResourceId'), variables('mlVersion')).WorkspaceId]"},
        "mlWorkspaceWorkspaceLink": {"type": "string", "value": "[concat('https://studio.azureml.net/Home/ViewWorkspace/', reference(variables('mlResourceId'), variables('mlVersion')).WorkspaceId)]"}
    }
}

```
Speichern Sie diese Vorlage als mlworkspace.json-Datei unter c:\temp\.

### <a name="deploy-the-resource-group-based-on-the-template"></a>Bereitstellen der Ressourcengruppe, basierend auf der Vorlage
* Öffnen der PowerShell
* Installieren Sie Module für Azure Ressourcenmanager und Azure Servicemanagement  

```
# Install the Azure Resource Manager modules from the PowerShell Gallery (press “A”)
Install-Module AzureRM -Scope CurrentUser

# Install the Azure Service Management modules from the PowerShell Gallery (press “A”)
Install-Module Azure -Scope CurrentUser
```

   Diese Schritte herunterladen und installieren die Module erforderlich sind, um die verbleibenden Schritte ausführen. Dieses Dokument muss nur einmal in der Umgebung ausgeführt werden, wenn Sie die Befehle PowerShell ausgeführt werden.   

* In Azure authentifizieren  

```
# Authenticate (enter your credentials in the pop-up window)
Add-AzureRmAccount
```
Diesen Schritt muss für jede Sitzung wiederholt werden. Nach der Authentifizierung, sollen Informationen zu Ihrem Abonnement angezeigt werden.

![Azure-Konto][1]

Nun wir Zugriff auf Azure verfügen, können wir die Ressourcengruppe erstellen.

* Erstellen einer Ressourcengruppe

```
$rg = New-AzureRmResourceGroup -Name "uniquenamerequired523" -Location "South Central US"
$rg
```

Stellen Sie sicher, dass die Ressourcengruppe ordnungsgemäß bereitgestellt ist. **ProvisioningState** sollte "wurde erfolgreich abgeschlossen werden."
Der Namen der Ressource Gruppe wird durch die Vorlage für den Kontonamen Speicher generieren verwendet. Der Kontonamen Speicher muss zwischen 3 und 24 Zeichen lang sein und Zahlen und nur Kleinbuchstaben verwenden.

![Ressourcengruppe][2]

* Verwenden die Ressource Gruppe Bereitstellung, Bereitstellen eines neuen Computer Learning-Arbeitsbereichs.

```
# Create a Resource Group, TemplateFile is the location of the JSON template.
$rgd = New-AzureRmResourceGroupDeployment -Name "demo" -TemplateFile "C:\temp\mlworkspace.json" -ResourceGroupName $rg.ResourceGroupName
```

Nachdem die Bereitstellung abgeschlossen ist, ist es recht einfach zu Access-Eigenschaften des Arbeitsbereichs, die Sie bereitgestellt. Beispielsweise können Sie die primäre Key Token zugreifen.

```
# Access Azure ML Workspace Token after its deployment.
$rgd.Outputs.mlWorkspaceToken.Value
```

Eine weitere Möglichkeit zum Abrufen der vorhandenen Arbeitsbereich Token besteht darin, den Befehl aufrufen-AzureRmResourceAction verwenden. Beispielsweise können Sie die primären und sekundären Token aller Arbeitsbereiche aufzulisten.

```  
# List the primary and secondary tokens of all workspaces
Get-AzureRmResource |? { $_.ResourceType -Like "*MachineLearning/workspaces*"} |% { Invoke-AzureRmResourceAction -ResourceId $_.ResourceId -Action listworkspacekeys -Force}  
```
Nachdem der Arbeitsbereich bereitgestellt wird, können Sie auch viele Azure maschinellen Learning Studio Aufgaben mithilfe der [PowerShell-Modul für Azure maschinellen Learning](http://aka.ms/amlps)automatisieren.

## <a name="next-steps"></a>Nächste Schritte 
* Weitere Informationen zu [Authoring Azure Ressourcenmanager Vorlagen](../resource-group-authoring-templates.md). 
* Haben Sie einen Blick auf die [Azure Schnellstart Vorlagen Repository](https://github.com/Azure/azure-quickstart-templates)ein. 
* Schauen Sie sich dieses Video über [Azure Ressourcenmanager](https://channel9.msdn.com/Events/Ignite/2015/C9-39). 
 
<!--Image references-->
[1]: ../media/machine-learning-deploy-with-resource-manager-template/azuresubscription.png
[2]: ../media/machine-learning-deploy-with-resource-manager-template/resourcegroupprovisioning.png


<!--Link references-->

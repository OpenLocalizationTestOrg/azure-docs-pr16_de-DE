<properties
   pageTitle="Erstellen von Windows-basiertem Hadoop Cluster in mithilfe von Vorlagen Ressourcenmanager Azure HDInsight | Microsoft Azure"
    description="Erfahren Sie, wie Cluster für Azure HDInsight mithilfe von Azure Ressourcenmanager Vorlagen erstellen."
   services="hdinsight"
   documentationCenter=""
   tags="azure-portal"
   authors="mumian"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/19/2016"
   ms.author="jgao"/>

# <a name="create-windows-based-hadoop-clusters-in-hdinsight-using-azure-resource-manager-templates"></a>Erstellen von Windows-basiertem Hadoop Cluster in mithilfe von Vorlagen Ressourcenmanager Azure HDInsight

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Erfahren Sie, wie Sie mithilfe von Vorlagen Ressourcenmanager Azure HDInsight-Cluster erstellen. Weitere Informationen finden Sie unter [Bereitstellen einer Anwendung mit Ressourcenmanager Azure-Vorlage](../resource-group-template-deploy.md). Für die Clustererstellung von anderen Tools und Features klicken Sie auf der Registerkarte wählen Sie am oberen Rand dieser Seite oder finden Sie unter [Methoden zur Erstellung](hdinsight-provision-clusters.md#cluster-creation-methods).

##<a name="prerequisites"></a>Voraussetzungen für:

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


Bevor Sie die Anweisungen in diesem Artikel beginnen, müssen Sie die folgenden verfügen:

- [Azure-Abonnement](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Azure PowerShell oder Azure CLI

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell-and-cli.md)] 

### <a name="access-control-requirements"></a>Anforderungen für Access-Steuerelement

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="resource-manager-templates"></a>Ressourcenmanager-Vorlagen

Ressourcenmanager Vorlage erleichtert die HDInsight Cluster, deren abhängigen Ressourcen (wie etwa das Standardkonto Speicher) und anderen Ressourcen (z. B. Azure SQL-Datenbank mit Apache Sqoop) für eine Anwendung in einem einzigen, koordinierte Vorgang erstellen. In der Vorlage definieren Sie die Ressourcen, die für die Anwendung erforderlich sind und Bereitstellungsparameter, um Werte für die verschiedenen Umgebungen Eingabemethoden angeben. Die Vorlage besteht aus JSON und Ausdrücke, die Sie verwenden können, um Werte für die Bereitstellung zu erstellen.

Ressourcenmanager Vorlage zum Erstellen einer HDInsight Cluster und das abhängige Speicher Azure-Konto finden Sie im [Anhang-A](#appx-a-arm-template). Verwenden Sie einen Text-Editor, um die Vorlage in einer Datei auf Ihrem Computer zu speichern. Sie lernen So rufen Sie die Vorlage, die verschiedene Tools verwenden.

Weitere Informationen zu Ressourcenmanager Vorlage finden Sie unter

- [Autor Ressourcenmanager Azure-Vorlagen](../resource-group-authoring-templates.md)
- [Bereitstellen einer Anwendung mit Ressourcenmanager Azure-Vorlage](../resource-group-template-deploy.md)


## <a name="deploy-with-powershell"></a>Bereitstellen mit PowerShell

Das folgende Verfahren wird einen HDInsight Cluster erstellt.

**Um einen Cluster mit Ressourcenmanager Vorlage bereitstellen**

1. Speichern Sie die Datei Json in [Anhang A](#appx-a-arm-template) , um Ihre Arbeitsstationen.
2. Legen Sie die Parameter ein, falls erforderlich.
3. Führen Sie die Vorlage mit dem folgenden PowerShell-Skript aus:

        ####################################
        # Set these variables
        ####################################
        #region - used for creating Azure service names
        $nameToken = "<Enter an Alias>" 
        $templateFile = "C:\HDITutorials-ARM\hdinsight-arm-template.json"
        #endregion

        ####################################
        # Service names and varialbes
        ####################################
        #region - service names
        $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

        $resourceGroupName = $namePrefix + "rg"
        $hdinsightClusterName = $namePrefix + "hdi"
        $defaultStorageAccountName = $namePrefix + "store"
        $defaultBlobContainerName = $hdinsightClusterName

        $location = "East US 2"

        $armDeploymentName = $namePrefix
        #endregion

        ####################################
        # Connect to Azure
        ####################################
        #region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        try{Get-AzureRmContext}
        catch{Login-AzureRmAccount}
        #endregion

        # Create a resource group
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $Location

        # Create cluster and the dependent storage accounge
        $parameters = @{clusterName="$hdinsightClusterName";clusterStorageAccountName="$defaultStorageAccountName"}

        New-AzureRmResourceGroupDeployment `
            -Name $armDeploymentName `
            -ResourceGroupName $resourceGroupName `
            -TemplateFile $templateFile `
            -TemplateParameterObject $parameters

        # List cluster
        Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName 

    PowerShell-Skript konfiguriert nur die Cluster und der Speicher Kontonamens.  Sie können andere Werte in der Vorlage Ressourcenmanager festlegen. 
    
Weitere Informationen finden Sie unter [Bereitstellen mit PowerShell](../resource-group-template-deploy.md#deploy-with-powershell).

## <a name="deploy-with-azure-cli"></a>Mit Azure CLI bereitstellen

Im folgende Beispiel wird ein Cluster abhängige Speicher-Konto und der zugehörigen Container, indem Sie eine Vorlage Ressourcenmanager erstellt:

    azure login
    azure config mode arm
    azure group create -n hdi1229rg -l "East US 2"
    azure group deployment create "hdi1229rg" "hdi1229" --template-file "C:\HDITutorials-ARM\hdinsight-arm-template.json" -p "{\"clusterName\":{\"value\":\"hdi1229win\"},\"clusterStorageAccountName\":{\"value\":\"hdi1229store\"},\"location\":{\"value\":\"East US 2\"},\"clusterLoginPassword\":{\"value\":\"Pass@word1\"}}"





## <a name="deploy-with-rest-api"></a>REST API bereitstellen

Finden Sie unter [mit die REST-API bereitstellen](../resource-group-template-deploy.md#deploy-with-the-rest-api).

## <a name="deploy-with-visual-studio"></a>Mit Visual Studio bereitstellen

Mit Visual Studio können Sie einem Gruppenprojekt Ressource erstellen und in Azure über die Benutzeroberfläche bereitstellen. Wählen Sie den Typ von Ressourcen in Ihr Projekt aufnehmen möchten, und diese Ressourcen automatisch Ressourcenmanager Vorlage hinzugefügt werden. Das Projekt bietet auch ein PowerShell-Skript, um die Vorlage bereitzustellen.

Eine Einführung in Visual Studio mit Ressourcengruppen verwenden finden Sie unter [Erstellen und Bereitstellen von Azure-Ressourcengruppen über Visual Studio](../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).

##<a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie mehrere Methoden zum Erstellen eines HDInsight Clusters erhalten. Weitere Informationen finden Sie unter den folgenden Artikeln:


- Ein Beispiel für die Bereitstellung von Ressourcen über die .NET Client-Bibliothek finden Sie unter [Bereitstellen von Ressourcen mithilfe von .NET Bibliotheken und einer Vorlage](../virtual-machines/virtual-machines-windows-csharp-template.md).
- Genaue Beispiel für eine Anwendung bereitzustellen, finden Sie unter [Bereitstellen und Bereitstellen Microservices vorhersehbar in Azure](../app-service-web/app-service-deploy-complex-application-predictably.md).
- Bereitstellen der Lösung in verschiedenen Umgebungen Anleitungen finden Sie unter [Entwicklung und Test-Umgebungen in Microsoft Azure](../solution-dev-test-environments.md).
- Weitere Informationen zu den Abschnitten der Ressourcenmanager Azure Vorlage finden Sie unter [Authoring-Vorlagen](../resource-group-authoring-templates.md).
- Eine Liste der Funktionen, die Sie in einer Vorlage Ressourcenmanager Azure verwenden können, finden Sie unter [Vorlagenfunktionen](../resource-group-template-functions.md).



##<a name="appx-a-resource-manager-template"></a>Ressourcenmanager AppX A: Vorlage

Die folgende Ressource-Manager Azure-Vorlage erstellt einen Windows-basiertem Hadoop-Cluster mit dem Konto abhängige Azure-Speicher.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
    "parameters": {
        "location": {
        "type": "string",
        "defaultValue": "East US 2",
        "allowedValues": [
            "Central US",
            "North Europe",
            "East US",
            "East US 2",
            "North Central US",
            "South Central US",
            "West US",
            "North Europe",
            "West Europe",
            "East Asia",
            "Southeast Asia",
            "Japan East",
            "Japan West",
            "Brizil South",
            "Australia East",
            "Australia Southeast",
            "Central India"
        ],
        "metadata": {
            "description": "The location where all azure resources will be deployed."
        }
        },
        "clusterName": {
        "type": "string",
        "metadata": {
            "description": "The name of the HDInsight cluster to create."
        }
        },
        "clusterLoginUserName": {
        "type": "string",
        "defaultValue": "admin",
        "metadata": {
            "description": "These credentials can be used to submit jobs to the cluster and to log into cluster dashboards."
        }
        },
        "clusterLoginPassword": {
        "type": "securestring",
        "metadata": {
            "description": "The password for the cluster login."
        }
        },
        "clusterStorageAccountName": {
        "type": "string",
        "metadata": {
            "description": "The name of the storage account to be created and be used as the cluster's storage."
        }
        },
        "clusterStorageType": {
        "type": "string",
        "defaultValue": "Standard_LRS",
        "allowedValues": [
            "Standard_LRS",
            "Standard_GRS",
            "Standard_ZRS"
        ]
        },
        "clusterWorkerNodeCount": {
        "type": "int",
        "defaultValue": 4,
        "metadata": {
            "description": "The number of nodes in the HDInsight cluster."
        }
        }
    },
        "variables": {},
        "resources": [
            {
            "name": "[parameters('clusterStorageAccountName')]",
            "type": "Microsoft.Storage/storageAccounts",
            "location": "[parameters('location')]",
            "apiVersion": "2015-05-01-preview",
            "dependsOn": [],
            "tags": {},
            "properties": {
                "accountType": "[parameters('clusterStorageType')]"
            }
            },
            {
            "name": "[parameters('clusterName')]",
            "type": "Microsoft.HDInsight/clusters",
            "location": "[parameters('location')]",
            "apiVersion": "2015-03-01-preview",
            "dependsOn": [
                "[concat('Microsoft.Storage/storageAccounts/',parameters('clusterStorageAccountName'))]"
            ],
            "tags": {},
            "properties": {
                "clusterVersion": "3.2",
                "osType": "Windows",
                "clusterDefinition": {
                "kind": "hadoop",
                "configurations": {
                    "gateway": {
                    "restAuthCredential.isEnabled": true,
                    "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
                    "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
                    }
                }
                },
                "storageProfile": {
                "storageaccounts": [
                    {
                    "name": "[concat(parameters('clusterStorageAccountName'),'.blob.core.windows.net')]",
                    "isDefault": true,
                    "container": "[parameters('clusterName')]",
                    "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('clusterStorageAccountName')), '2015-05-01-preview').key1]"
                    }
                ]
                },
                "computeProfile": {
                "roles": [
                    {
                    "name": "headnode",
                    "targetInstanceCount": "1",
                    "hardwareProfile": {
                        "vmSize": "Large"
                    }
                    },
                    {
                    "name": "workernode",
                    "targetInstanceCount": "[parameters('clusterWorkerNodeCount')]",
                    "hardwareProfile": {
                        "vmSize": "Large"
                    }
                    }
                ]
                }
            }
            }
        ],
        "outputs": {
            "cluster": {
            "type": "object",
            "value": "[reference(resourceId('Microsoft.HDInsight/clusters',parameters('clusterName')))]"
            }
        }
    }


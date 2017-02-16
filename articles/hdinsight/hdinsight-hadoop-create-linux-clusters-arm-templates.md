<properties
   pageTitle="Erstellen von Linux-basierten Hadoop Cluster in mithilfe von Vorlagen Ressourcenmanager Azure HDInsight | Microsoft Azure"
    description="Erfahren Sie, wie Cluster für Azure HDInsight mithilfe von Azure Azure Ressourcenmanager Vorlagen erstellen."
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
   ms.date="09/02/2016"
   ms.author="jgao"/>

# <a name="create-linux-based-hadoop-clusters-in-hdinsight-using-azure-resource-manager-templates"></a>Erstellen von Linux-basierten Hadoop Cluster in mithilfe von Vorlagen Ressourcenmanager Azure HDInsight

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Erfahren Sie, wie Sie mithilfe von Vorlagen für die Ressource Manager(ARM) Azure HDInsight-Cluster erstellen. Weitere Informationen finden Sie unter [Bereitstellen einer Anwendung mit Ressourcenmanager Azure-Vorlage](../resource-group-template-deploy.md). Für die Clustererstellung von anderen Tools und Features klicken Sie auf der Registerkarte wählen Sie am oberen Rand dieser Seite oder finden Sie unter [Methoden zur Erstellung](hdinsight-provision-clusters.md#cluster-creation-methods).

##<a name="prerequisites"></a>Voraussetzungen für:

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Bevor Sie die Anweisungen in diesem Artikel beginnen, müssen Sie die folgenden verfügen:

- [Azure-Abonnement](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Azure PowerShell und/oder Azure CLI

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell-and-cli.md)]

### <a name="access-control-requirements"></a>Anforderungen für Access-Steuerelement

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="resource-manager-templates"></a>Ressourcenmanager-Vorlagen

Ressourcenmanager Vorlage erleichtert die HDInsight Cluster, deren abhängigen Ressourcen (wie etwa das Standardkonto Speicher) und anderen Ressourcen (z. B. Azure SQL-Datenbank mit Apache Sqoop) für eine Anwendung in einem einzigen, koordinierte Vorgang erstellen. In der Vorlage definieren Sie die Ressourcen, die für die Anwendung erforderlich sind und Bereitstellungsparameter, um Werte für die verschiedenen Umgebungen Eingabemethoden angeben. Die Vorlage besteht aus JSON und Ausdrücke, die Sie verwenden können, um Werte für die Bereitstellung zu erstellen.

Ressourcenmanager Vorlage zum Erstellen einer HDInsight Cluster und das abhängige Speicher Azure-Konto finden Sie im [Anhang-A](#appx-a-arm-template). Verwenden Sie Plattformen [VSCode](https://code.visualstudio.com/#alt-downloads) mit der [Erweiterung Ressourcenmanager](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools) oder einen Text-Editor, um die Vorlage in einer Datei auf Ihrem Computer zu speichern. Lernen Sie so rufen Sie die Vorlage mithilfe verschiedene Methoden ein.

Weitere Informationen zu Ressourcenmanager Vorlage finden Sie unter

- [Autor Ressourcenmanager Azure-Vorlagen](../resource-group-authoring-templates.md)
- [Bereitstellen einer Anwendung mit Ressourcenmanager Azure-Vorlage](../resource-group-template-deploy.md)

Um das JSON-Schema für bestimmte Elemente zu finden, können Sie das folgende Verfahren ausführen:

1. Öffnen Sie [Azure-Portal](https://porta.azure.com) , um ein HDInsight Cluster erstellen.  Finden Sie unter [Erstellen von Linux-basierten Cluster in Verwenden des Portals Azure HDInsight](hdinsight-hadoop-create-linux-clusters-portal.md).
2. Konfigurieren Sie die erforderlichen Elemente und Elemente, die Sie das Schema JSON benötigen.
3. Klicken Sie auf **Automatisierungsoptionen** , bevor Sie auf **Erstellen**, wie im folgenden Screenshot gezeigt:

    ![HDInsight Hadoop erstellen Cluster Ressourcenmanager Vorlagenoptionen Schema automation](./media/hdinsight-hadoop-create-linux-clusters-arm-templates/hdinsight-create-cluster-resource-manager-template-automation-option.png)

    Im Portal erstellt einen Ressourcenmanager je nach Konfiguration.
## <a name="deploy-with-powershell"></a>Bereitstellen mit PowerShell

Das folgende Verfahren erstellt HDInsight Linux-basierten Cluster.

**Um einen Cluster mit Ressourcenmanager Vorlage bereitstellen**

1. Speichern Sie die Datei Json in [Anhang A](#appx-a-arm-template) , um Ihre Arbeitsstationen. In der PowerShell-Skript lautet der Dateiname *C:\HDITutorials-ARM\hdinsight-arm-template.json*.
2. Legen Sie bei Bedarf die Parameter und Variablen.
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
        $parameters = @{clusterName="$hdinsightClusterName"}

        New-AzureRmResourceGroupDeployment `
            -Name $armDeploymentName `
            -ResourceGroupName $resourceGroupName `
            -TemplateFile $templateFile `
            -TemplateParameterObject $parameters

        # List cluster
        Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName 

    PowerShell-Skript wird nur den Clusternamen konfiguriert. Der Name des Speicher ist hartcodierte in der Vorlage. Sie werden aufgefordert, das Kennwort für Benutzer eingeben (der Standard-Benutzername ist *Admin*); und das Kennwort SSH (der Standard-Benutzername SSH ist *Sshuser*).  
    
Weitere Informationen finden Sie unter [Bereitstellen mit PowerShell](../resource-group-template-deploy.md#deploy-with-powershell).

## <a name="deploy-with-azure-cli"></a>Mit Azure CLI bereitstellen

Im folgende Beispiel wird ein Cluster abhängige Speicher-Konto und der zugehörigen Container, indem Sie eine Vorlage Ressourcenmanager erstellt:

    azure login
    azure config mode arm
    azure group create -n hdi1229rg -l "East US"
    azure group deployment create --resource-group "hdi1229rg" --name "hdi1229" --template-file "C:\HDITutorials-ARM\hdinsight-arm-template.json"
    
Sie werden aufgefordert, den Clusternamen, Cluster Benutzerkennwort (der Standard-Benutzername ist *Admin*) und das Kennwort SSH (der Standard-Benutzername SSH ist *Sshuser*) eingeben. Um Inline-Parameter bereitzustellen:

    azure group deployment create --resource-group "hdi1229rg" --name "hdi1229" --template-file "c:\Tutorials\HDInsightARM\create-linux-based-hadoop-cluster-in-hdinsight.json" --parameters '{\"clusterName\":{\"value\":\"hdi1229\"},\"clusterLoginPassword\":{\"value\":\"Pass@word1\"},\"sshPassword\":{\"value\":\"Pass@word1\"}}'

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

Die folgende Ressource-Manager Azure Vorlage erstellt einen Linux-basierten Hadoop-Cluster mit dem Konto abhängige Azure-Speicher. 

> [AZURE.NOTE] Im Beispiel enthält Informationen für Struktur Metastore und Oozie Metastore.  Entfernen Sie den Abschnitt oder konfigurieren Sie des Abschnitts, bevor Sie die Vorlage verwenden.

    {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
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
            "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
        }
        },
        "sshUserName": {
        "type": "string",
        "defaultValue": "sshuser",
        "metadata": {
            "description": "These credentials can be used to remotely access the cluster."
        }
        },
        "sshPassword": {
        "type": "securestring",
        "metadata": {
            "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
        }
        },
        "location": {
        "type": "string",
        "defaultValue": "East US",
        "allowedValues": [
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
            "Australia East",
            "Australia Southeast"
        ],
        "metadata": {
            "description": "The location where all azure resources will be deployed."
        }
        },
        "clusterType": {
        "type": "string",
        "defaultValue": "hadoop",
        "allowedValues": [
            "hadoop",
            "hbase",
            "storm",
            "spark"
        ],
        "metadata": {
            "description": "The type of the HDInsight cluster to create."
        }
        },
        "clusterWorkerNodeCount": {
        "type": "int",
        "defaultValue": 2,
        "metadata": {
            "description": "The number of nodes in the HDInsight cluster."
        }
        }
    },
    "variables": {
        "defaultApiVersion": "2015-05-01-preview",
        "clusterApiVersion": "2015-03-01-preview",
        "clusterStorageAccountName": "[concat(parameters('clusterName'),'store')]"
    },
    "resources": [
        {
        "name": "[variables('clusterStorageAccountName')]",
        "type": "Microsoft.Storage/storageAccounts",
        "location": "[parameters('location')]",
        "apiVersion": "[variables('defaultApiVersion')]",
        "dependsOn": [ ],
        "tags": { },
        "properties": {
            "accountType": "Standard_LRS"
        }
        },
        {
        "name": "[parameters('clusterName')]",
        "type": "Microsoft.HDInsight/clusters",
        "location": "[parameters('location')]",
        "apiVersion": "[variables('clusterApiVersion')]",
        "dependsOn": [ "[concat('Microsoft.Storage/storageAccounts/',variables('clusterStorageAccountName'))]" ],
        "tags": {

        },
        "properties": {
            "clusterVersion": "3.4",
            "osType": "Linux",
            "tier": "standard",
            "clusterDefinition": {
            "kind": "[parameters('clusterType')]",
            "configurations": {
                "gateway": {
                "restAuthCredential.isEnabled": true,
                "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
                "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
                },
                "hive-site": {
                    "javax.jdo.option.ConnectionDriverName": "com.microsoft.sqlserver.jdbc.SQLServerDriver",
                    "javax.jdo.option.ConnectionURL": "jdbc:sqlserver://myadla0901dbserver.database.windows.net;database=myhive20160901;encrypt=true;trustServerCertificate=true;create=false;loginTimeout=300",
                    "javax.jdo.option.ConnectionUserName": "johndole",
                    "javax.jdo.option.ConnectionPassword": "myPassword$"
                },
                "hive-env": {
                    "hive_database": "Existing MSSQL Server database with SQL authentication",
                    "hive_database_name": "myhive20160901",
                    "hive_database_type": "mssql",
                    "hive_existing_mssql_server_database": "myhive20160901",
                    "hive_existing_mssql_server_host": "myadla0901dbserver.database.windows.net",
                    "hive_hostname": "myadla0901dbserver.database.windows.net"
                },
                "oozie-site": {
                    "oozie.service.JPAService.jdbc.driver": "com.microsoft.sqlserver.jdbc.SQLServerDriver",
                    "oozie.service.JPAService.jdbc.url": "jdbc:sqlserver://myadla0901dbserver.database.windows.net;database=myhive20160901;encrypt=true;trustServerCertificate=true;create=false;loginTimeout=300",
                    "oozie.service.JPAService.jdbc.username": "johndole",
                    "oozie.service.JPAService.jdbc.password": "myPassword$",
                    "oozie.db.schema.name": "oozie"
                },
                "oozie-env": {
                    "oozie_database": "Existing MSSQL Server database with SQL authentication",
                    "oozie_database_name": "myhive20160901",
                    "oozie_database_type": "mssql",
                    "oozie_existing_mssql_server_database": "myhive20160901",
                    "oozie_existing_mssql_server_host": "myadla0901dbserver.database.windows.net",
                    "oozie_hostname": "myadla0901dbserver.database.windows.net"
                }            
            }
            },
            "storageProfile": {
            "storageaccounts": [
                {
                "name": "[concat(variables('clusterStorageAccountName'),'.blob.core.windows.net')]",
                "isDefault": true,
                "container": "[parameters('clusterName')]",
                "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('clusterStorageAccountName')), variables('defaultApiVersion')).key1]"
                }
            ]
            },
            "computeProfile": {
            "roles": [
                {
                "name": "headnode",
                "targetInstanceCount": "2",
                "hardwareProfile": {
                    "vmSize": "Standard_D3"
                },
                "osProfile": {
                    "linuxOperatingSystemProfile": {
                    "username": "[parameters('sshUserName')]",
                    "password": "[parameters('sshPassword')]"
                    }
                }
                },
                {
                "name": "workernode",
                "targetInstanceCount": "[parameters('clusterWorkerNodeCount')]",
                "hardwareProfile": {
                    "vmSize": "Standard_D3"
                },
                "osProfile": {
                    "linuxOperatingSystemProfile": {
                    "username": "[parameters('sshUserName')]",
                    "password": "[parameters('sshPassword')]"
                    }
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

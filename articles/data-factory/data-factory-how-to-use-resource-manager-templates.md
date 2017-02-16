<properties 
    pageTitle="Ressourcenmanager verwenden Vorlagen in Daten Factory | Microsoft Azure" 
    description="Informationen Sie zum Erstellen und Verwenden von Vorlagen Azure Ressourcenmanager Daten Factory Personen erstellen." 
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor=""/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/24/2016" 
    ms.author="shlo"/>

# <a name="use-templates-to-create-azure-data-factory-entities"></a>Verwenden von Vorlagen zum Erstellen Azure Data Factory-Elemente

## <a name="overview"></a>(Übersicht)
Wiederverwenden von während der Verwendung von Azure Data Factory für Ihren Bedürfnissen Integration, Sie bietet sich das gleiche Muster über verschiedenen Umgebungen oder die gleiche Aufgabe wiederholt in der gleichen Lösung implementieren. Vorlagen helfen Ihnen implementieren und diese Szenarios auf einfache Weise verwalten. Vorlagen in Azure Data Factory sind ideal für Szenarien, die Wiederverwendung und Wiederholung umfassen.
 
Erwägen Sie die Situation, wo eine Organisation 10 des Fertigungsanlagen auf der ganzen Welt verfügt. Die Protokolle aus jeder Pflanze werden in einer separaten lokalen SQL Server-Datenbank gespeichert. Das Unternehmen möchte ein einzelnes Datawarehouse in der Cloud für Ad-hoc-Analytics erstellen. Darüber hinaus sollen die gleiche Logik aber verschiedene Konfigurationen für Entwicklung, testen und Herstellung Umgebungen haben. 

In diesem Fall muss ein Vorgangs innerhalb der gleichen Umgebung jedoch mit anderen Werten über die Factory 10 Daten für jedes Fertigungsanlage wiederholt werden. **Wiederholung** , ist vorhanden. Vorlagenoptionen ermöglicht die Abstraktion dieser generische Flusses (d. h., müssen die gleichen Aktivitäten in jede Factory Daten Rohrleitungen), verwendet jedoch eine separate Parameterdatei für jede Fertigungsanlage.

Darüber hinaus, wie die Organisation diese Factory 10 Daten in verschiedenen Umgebungen mehrmals bereitstellen möchte, diese **Wiederverwendung** mithilfe von Vorlagen durch die Nutzung von separaten Parameterdateien für die Entwicklung, testen und Herstellung Umgebungen.

## <a name="templating-with-azure-resource-manager"></a>Vorlagen mit Azure Ressourcenmanager
[Ressourcenmanager Azure Vorlagen](../azure-resource-manager/resource-group-overview.md#template-deployment) eignen sich hervorragend Vorlagenoptionen in Azure Data Factory erzielen. Ressourcenmanager Vorlagen definieren die Infrastruktur und die Konfiguration von Azure-Lösung über eine JSON-Datei ein. Da Azure Ressourcenmanager Vorlagen mit den Diensten von Azure alle/am häufigsten arbeiten möchten, können sie weit verwendet werden, alle Ressourcen des Azure Ihre Bestände jederzeit problemlos verwalten. Finden Sie unter [Azure Ressourcenmanager Authoring-Vorlagen](../resource-group-authoring-templates.md) , erfahren Sie mehr über die Ressourcenmanager Vorlagen im Allgemeinen. 

## <a name="tutorials"></a>Lernprogramme
Die folgenden Lernprogramme für eine schrittweise Anleitung zum Erstellen von Daten Factory-Elemente mithilfe von Ressourcenmanager Vorlagen finden Sie unter:

- [Lernprogramm: Erstellen einer Verkaufspipeline zum Kopieren von Daten mithilfe von Ressourcenmanager Azure-Vorlage](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
- [Lernprogramm: Erstellen einer Verkaufspipeline zum Verarbeiten von Daten mithilfe von Ressourcenmanager Azure-Vorlage](data-factory-build-your-first-pipeline.md)

## <a name="data-factory-templates-on-github"></a>Daten Factory-Vorlagen auf Github
Schauen Sie sich die folgenden Azure Schnellstart-Vorlagen auf Github: 

- [Erstellen Sie eine Factory Daten zum Kopieren von Daten aus Azure BLOB-Speicher mit Azure SQL-Datenbank](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-blob-to-sql-copy)
- [Erstellen Sie eine Factory Daten mit Struktur Aktivität auf Cluster Azure HDInsight](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-hive-transformation)
- [Erstellen Sie eine Factory Daten, um Daten aus Vertrieb in Azure Blobs kopieren](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-salesforce-to-blob-copy)

Ihre Vorlagen Azure Data Factory bei [Azure-Schnellstart](https://azure.microsoft.com/documentation/templates/)freigeben können. Verweisen Sie auf den [Beitrag Leitfaden](https://github.com/Azure/azure-quickstart-templates/tree/master/1-CONTRIBUTION-GUIDE) bei der Entwicklung von Vorlagen, die über dieses Repository freigegeben werden können. 

Die folgenden Abschnitte enthalten Details zu den Daten Factory-Ressourcen in einer Vorlage Ressourcenmanager definieren. 

## <a name="defining-data-factory-resources-in-templates"></a>Definieren von Daten Factory-Ressourcen in Vorlagen
Vorlage für eine Fabrik Daten definieren auf oberster Ebene ist:

    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": { ...
    },
    "variables": { ...
    },
    "resources": [
    {
        "name": "[parameters('dataFactoryName')]",
        "apiVersion": "[variables('apiVersion')]",
        "type": "Microsoft.DataFactory/datafactories",
        "location": "westus",
        "resources": [
        { "type": "linkedservices",
            ...
        },
        {"type": "datasets",
            ...
        },
        {"type": "dataPipelines",
            ...
        }
    }

### <a name="define-data-factory"></a>Definieren von Daten factory

Definieren Sie eine Factory Daten in der Vorlage Ressourcenmanager, wie im folgenden Beispiel gezeigt:

    "resources": [
    {
        "name": "[variables('<mydataFactoryName>')]",
        "apiVersion": "2015-10-01",
        "type": "Microsoft.DataFactory/datafactories",
        "location": "East US"
    }

Die DataFactoryName ist in "Variablen" wie folgt definiert:

    "dataFactoryName": "[concat('<myDataFactoryName>', uniqueString(resourceGroup().id))]",

### <a name="define-linked-services"></a>Definieren von verknüpften Diensten 
    
    "type": "linkedservices",
    "name": "[variables('<LinkedServiceName>')]",
    "apiVersion": "2015-10-01",
    "dependsOn": [ "[variables('<dataFactoryName>')]" ],
    "properties": {
        ...
    }


Ausführliche Informationen zu den JSON-Eigenschaften für den bestimmten verknüpfte Dienst, die, den Sie bereitstellen möchten, finden Sie unter [Verknüpfte Speicherdienst](data-factory-azure-blob-connector.md#azure-storage-linked-service) oder [Verknüpften Diensten zu berechnen](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) . Der Parameter "DependsOn" gibt die Namen der entsprechenden Daten Factory an. In der folgenden JSON-Definition ist ein Beispiel für eine verknüpfte Dienst Azure-Speicher definieren dargestellt:

### <a name="define-datasets"></a>Definieren von datasets

    "type": "datasets",
    "name": "[variables('<myDatasetName>')]",
    "dependsOn": [
        "[variables('<dataFactoryName>')]",
        "[variables('<myDatasetLinkedServiceName>')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
        ...
    }

Einzelheiten zu den JSON-Eigenschaften für den Typ von bestimmter Dataset, die, den Sie bereitstellen möchten, finden Sie unter [Datenspeicher unterstützt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) . Hinweis, dass der Parameter "DependsOn" gibt an, die Namen der entsprechenden Daten Factory und Speicher verknüpft Dienst an. In der folgenden JSON-Definition ist ein Beispiel für definieren Dataset Typ Azure Blob-Speicher dargestellt:

    "type": "datasets",
    "name": "[variables('storageDataset')]",
    "dependsOn": [
        "[variables('dataFactoryName')]",
        "[variables('storageLinkedServiceName')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "[variables('storageLinkedServiceName')]",
    "typeProperties": {
        "folderPath": "[concat(parameters('sourceBlobContainer'), '/')]",
        "fileName": "[parameters('sourceBlobName')]",
        "format": {
            "type": "TextFormat"
        }
    },
    "availability": {
        "frequency": "Hour",
        "interval": 1
    }

### <a name="define-pipelines"></a>Definieren von pipelines

    "type": "dataPipelines",
    "name": "[variables('<mypipelineName>')]",
    "dependsOn": [
        "[variables('<dataFactoryName>')]",
        "[variables('<inputDatasetLinkedServiceName>')]",
        "[variables('<outputDatasetLinkedServiceName>')]",
        "[variables('<inputDataset>')]",
        "[variables('<outputDataset>')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
        activities: {
            ...
        }
    }

Finden Sie unter [definieren Pipelines](data-factory-create-pipelines.md#pipeline-json) Details zu den JSON-Eigenschaften für die Definition von bestimmter Verkaufspipeline und Aktivitäten, die Sie bereitstellen möchten. Notieren Sie der Parameter "DependsOn" gibt die Namen der Factory Daten an, und alle entsprechenden Services oder Datasets verknüpft. Ein Beispiel für eine Verkaufspipeline, die Daten aus Azure BLOB-Speicher mit Azure SQL-Datenbank kopiert wird in den folgenden JSON-Codeausschnitt dargestellt:

    "type": "datapipelines",
    "name": "[variables('pipelineName')]",
    "dependsOn": [
        "[variables('dataFactoryName')]",
        "[variables('azureStorageLinkedServiceName')]",
        "[variables('azureSqlLinkedServiceName')]",
        "[variables('blobInputDatasetName')]",
        "[variables('sqlOutputDatasetName')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
        "activities": [
        {
            "name": "CopyFromAzureBlobToAzureSQL",
            "description": "Copy data frm Azure blob to Azure SQL",
            "type": "Copy",
            "inputs": [
                {
                    "name": "[variables('blobInputDatasetName')]"
                }
            ],
            "outputs": [
                {
                    "name": "[variables('sqlOutputDatasetName')]"
                }
            ],
            "typeProperties": {
                "source": {
                    "type": "BlobSource"
                },
                "sink": {
                    "type": "SqlSink",
                    "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM {0}', 'emp')"
                },
                "translator": {
                    "type": "TabularTranslator",
                    "columnMappings": "Column0:FirstName,Column1:LastName"
                }
            },
            "Policy": {
                "concurrency": 1,
                "executionPriorityOrder": "NewestFirst",
                "retry": 3,
                "timeout": "01:00:00"
            }
        }
        ],
        "start": "2016-10-03T00:00:00Z",
        "end": "2016-10-04T00:00:00Z"

## <a name="parameterizing-data-factory-template"></a>Parametrisieren Daten Factory-Vorlage
Bewährte Methoden zur Parametrisierung, finden Sie unter [bewährte Methoden zum Erstellen von Vorlagen Azure Ressourcenmanager](../resource-manager-template-best-practices.md#parameters) Artikel. Parameterverwendung sollte im Allgemeinen, insbesondere dann, wenn Variablen stattdessen verwendet werden können minimiert werden. Bieten Sie nur Parameter in den folgenden Szenarien:

- Einstellungen variieren je nach Umgebung (Beispiel: Entwicklung, testen und Herstellung)
- Kennwörter (zum Beispiel Kennwörter)

Wenn Sie geheimen Daten aus [Azure Schlüssel Tresor](../key-vault/key-vault-get-started.md) ziehen Sie beim Bereitstellen von Azure Data Factory Elemente mithilfe von Vorlagen müssen, geben Sie den **Key Tresor** und **geheimen Namen** wie im folgenden Beispiel gezeigt:

    "parameters": {
        "storageAccountKey": { 
            "reference": {
                "keyVault": {
                    "id":"/subscriptions/<subscriptionID>/resourceGroups/<resourceGroupName>/providers/Microsoft.KeyVault/vaults/<keyVaultName>",
                },
                "secretName": "<secretName>"
            }, 
        },
        ...
    }

> [AZURE.NOTE] Beim Exportieren von Vorlagen für vorhandene Daten Factory aktuell noch nicht unterstützt wird, ist es im Works. 



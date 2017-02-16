<properties
   pageTitle="Bereitstellen mehrerer Instanzen von Ressourcen | Microsoft Azure"
   description="Verwenden von Kopiervorgang und Matrizen in einer Vorlage Azure Ressourcenmanager durchlaufen mehrmals beim Bereitstellen von Ressourcen."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="wpickett"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/30/2016"
   ms.author="tomfitz"/>

# <a name="create-multiple-instances-of-resources-in-azure-resource-manager"></a>Erstellen Sie mehrerer Instanzen von Ressourcen in Azure Ressourcenmanager

In diesem Thema wird gezeigt, wie in der Vorlage Azure Ressourcenmanager Erstellen mehrerer Instanzen einer Ressource zu durchlaufen.

## <a name="copy-copyindex-and-length"></a>Kopieren, CopyIndex und Länge

Innerhalb der Ressource mehrmals erstellen können Sie ein Objekt **Kopieren** definieren, die angibt, wie oft durchlaufen. Die Kopie weist das folgende Format:

    "copy": { 
        "name": "websitescopy", 
        "count": "[parameters('count')]" 
    } 

Sie können den aktuellen Iterationswert mit der Funktion **copyIndex()** zugreifen, wie unten dargestellt, in der Funktion verketten.

    [concat('examplecopy-', copyIndex())]

Beim Erstellen von mehreren Ressourcen aus der ein Array von Werten können mit der Funktion **Länge** Sie die Anzahl die angeben. Als Parameter für die Funktion Länge geben Sie die Matrix ein.

    "copy": {
        "name": "websitescopy",
        "count": "[length(parameters('siteNames'))]"
    }

## <a name="use-index-value-in-name"></a>Verwenden Sie die Indexwert im Feld name

Sie können die Kopie Vorgang Erstellen mehrerer Instanzen einer Ressource, die eindeutig lauten basierend auf den Index erhöht. Beispielsweise möchten Sie möglicherweise eine eindeutige Nummer an das Ende jeder Ressourcenname hinzufügen, die bereitgestellt wird. Drei Websites, die mit dem Namen bereitgestellt:

- Examplecopy-0
- Examplecopy-1
- Examplecopy Basis 2 zurück.

Verwenden Sie die folgende Vorlage ein:

    "parameters": { 
      "count": { 
        "type": "int", 
        "defaultValue": 3 
      } 
    }, 
    "resources": [ 
      { 
          "name": "[concat('examplecopy-', copyIndex())]", 
          "type": "Microsoft.Web/sites", 
          "location": "East US", 
          "apiVersion": "2015-08-01",
          "copy": { 
             "name": "websitescopy", 
             "count": "[parameters('count')]" 
          }, 
          "properties": {
              "serverFarmId": "hostingPlanName"
          }
      } 
    ]

## <a name="offset-index-value"></a>Versatz Indexwert

Sie sehen im vorherigen Beispiel, die der Indexwert von 0 bis 2 geht. Wenn Sie den Indexwert hinaus versetzt wird, können Sie einen Wert in der **copyIndex()** -Funktion, wie z. B. **copyIndex(1)**übergeben. Die Anzahl der Iterationen ausführen weiterhin im Copy-Element angegeben ist, jedoch wird der Wert von CopyIndex durch den angegebenen Wert versetzt. Ja, mithilfe derselben Vorlage wie im vorherigen Beispiel, jedoch **copyIndex(1)** angeben würden drei Websites, die mit dem Namen bereitstellen:

- Examplecopy-1
- Examplecopy Basis 2 zurück
- Examplecopy-3

## <a name="use-copy-with-array"></a>Verwenden Sie kopieren mit array
   
Der Kopiervorgang ist besonders hilfreich, bei der Arbeit mit Matrizen zurück, da Sie jedes Element in der Matrix durchlaufen können. Drei Websites, die mit dem Namen bereitgestellt:

- Contoso-examplecopy
- Examplecopy-Fabrikam
- Examplecopy-Coho

Verwenden Sie die folgende Vorlage ein:

    "parameters": { 
      "org": { 
         "type": "array", 
             "defaultValue": [ 
             "Contoso", 
             "Fabrikam", 
             "Coho" 
          ] 
      }
    }, 
    "resources": [ 
      { 
          "name": "[concat('examplecopy-', parameters('org')[copyIndex()])]", 
          "type": "Microsoft.Web/sites", 
          "location": "East US", 
          "apiVersion": "2015-08-01",
          "copy": { 
             "name": "websitescopy", 
             "count": "[length(parameters('org'))]" 
          }, 
          "properties": {
              "serverFarmId": "hostingPlanName"
          } 
      } 
    ]

Natürlich, legen Sie die Anzahl der Exemplare auf einen anderen Wert als die Länge des Arrays. Beispielsweise könnten Sie Erstellen eines Arrays mit vielen Werten und übergeben dann einen Parameter-Wert, der angibt, wie viele der Arrayelemente bereitstellen. In diesem Fall legen Sie die Anzahl der Exemplare, wie im ersten Beispiel gezeigt. 

## <a name="depending-on-resources-in-a-loop"></a>Abhängig von Ressourcen in einer Schleife

Sie können angeben, dass eine Ressource mithilfe des Elements **DependsOn** nach einer anderen Ressource bereitgestellt werden. Wenn Sie eine Ressource bereitstellen, die die Sammlung von Ressourcen in einer Schleife abhängt müssen, können Sie den Namen der Schleife kopieren im Element **DependsOn** angeben. Im folgenden Beispiel wird gezeigt, wie 3 Speicherkonten vor der Bereitstellung des virtuellen Computers bereitstellen. Die vollständige virtuellen Computern Definition wird nicht angezeigt. Beachten Sie, dass das Element kopieren **Namen** , die auf **Storagecopy** festgelegt ist weist und das Element **DependsOn** für den virtuellen Computern auch auf **Storagecopy**festgelegt ist.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {},
        "resources": [
            {
                "apiVersion": "2015-06-15",
                "type": "Microsoft.Storage/storageAccounts",
                "name": "[concat('storage', uniqueString(resourceGroup().id), copyIndex())]",
                "location": "[resourceGroup().location]",
                "properties": {
                    "accountType": "Standard_LRS"
                 },
                "copy": { 
                    "name": "storagecopy", 
                    "count": 3 
                }
            },
           {
               "apiVersion": "2015-06-15", 
               "type": "Microsoft.Compute/virtualMachines", 
               "name": "[concat('VM', uniqueString(resourceGroup().id))]",  
               "dependsOn": ["storagecopy"],
               ...
           }
        ],
        "outputs": {}
    }

## <a name="looping-on-a-nested-resource"></a>Für eine Ressource geschachtelte Schleifen

Sie keine Kopie endlos wiedergegeben wird für eine Ressource geschachtelte verwenden. Wenn Sie mehrere Instanzen einer Ressource zu erstellen, die Sie in der Regel als geschachtelt in eine andere Ressource definieren müssen, müssen Sie stattdessen die Ressource als Ressource auf oberster Ebene erstellen und die Beziehung für die übergeordnete Ressource über die Eigenschaften **Typ** und **Namen** definieren.

Nehmen Sie beispielsweise an, dass Sie in der Regel ein Dataset als geschachtelte Ressource innerhalb einer Factory Daten definieren.

    "parameters": {
        "dataFactoryName": {
            "type": "string"
         },
         "dataSetName": {
            "type": "string"
         }
    },
    "resources": [
    {
        "type": "Microsoft.DataFactory/datafactories",
        "name": "[parameters('dataFactoryName')]",
        ...
        "resources": [
        {
            "type": "datasets",
            "name": "[parameters('dataSetName')]",
            "dependsOn": [
                "[parameters('dataFactoryName')]"
            ],
            ...
        }
    }]
    
Wenn Sie mehrere Instanzen von Datasets erstellen, müssen Sie Ihre Vorlage ändern, wie unten dargestellt. Beachten Sie die vollständig qualifizierte Typ sowie den Namen enthält den Daten Factory-Name.

    "parameters": {
        "dataFactoryName": {
            "type": "string"
         },
         "dataSetName": {
            "type": "array"
         }
    },
    "resources": [
    {
        "type": "Microsoft.DataFactory/datafactories",
        "name": "[parameters('dataFactoryName')]",
        ...
    },
    {
        "type": "Microsoft.DataFactory/datafactories/datasets",
        "name": "[concat(parameters('dataFactoryName'), '/', parameters('dataSetName')[copyIndex()])]",
        "dependsOn": [
            "[parameters('dataFactoryName')]"
        ],
        "copy": { 
            "name": "datasetcopy", 
            "count": "[length(parameters('dataSetName'))]" 
        } 
        ...
    }]

## <a name="create-multiple-instances-when-copy-wont-work"></a>Erstellen Sie mehrerer Instanzen, wenn kopieren funktioniert

Sie können nur **Kopieren** auf Ressourcentypen, nicht auf die Eigenschaften in einen Ressourcentyp. Dadurch möglicherweise Probleme für Sie erstellt, wenn Sie mehrere Instanzen von einem Element zu erstellen, der eine Ressource gehört, möchten. Ein gängiges Szenario besteht darin mehrere Daten Datenträger für einen virtuellen Computer zu erstellen. Sie verwenden keine **Kopie** mit Festplatten mit den Daten, da **DataDisks** eine Eigenschaft des virtuellen Computers, nicht in einem eigenen Ressourcenart ist. In diesem Fall erstellen Sie ein Array mit viele Daten Datenträger, wie Sie benötigen, und die tatsächliche Anzahl der Daten zum Erstellen Datenträger übergeben. In der Definition virtuellen Computern verwenden Sie die Funktion **Übernehmen** abzurufenden nur die Anzahl der Elemente, die gewünschte tatsächlich aus dem Array aus.

Ein vollständiges Beispiel für dieses Muster ist in der Vorlage zum [Erstellen eines virtuellen Computers für eine dynamische Auswahl der Datenträger Daten](https://azure.microsoft.com/documentation/templates/201-vm-dynamic-data-disks-selection/) anzeigen.

Nachfolgend die entsprechenden Abschnitten der Bereitstellungsvorlage. Viele der Vorlage wurde entfernt, um in den Abschnitten verbindet dynamisch Erstellen einer Anzahl von Datenträger Daten hervorzuheben. Beachten Sie die Parameter **NumDataDisks** , mit dem Sie die Anzahl der zum Erstellen Datenträger übergeben kann. 

```
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    ...
    "numDataDisks": {
      "type": "int",
      "maxValue": 64,
      "metadata": {
        "description": "This parameter allows you to select the number of disks you want"
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'dynamicdisk')]",
    "sizeOfDataDisksInGB": 100,
    "diskCaching": "ReadWrite",
    "diskArray": [
      {
        "name": "datadisk1",
        "lun": 0,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk1.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      },
      {
        "name": "datadisk2",
        "lun": 1,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk2.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      },
      {
        "name": "datadisk3",
        "lun": 2,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk3.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      },
      {
        "name": "datadisk4",
        "lun": 3,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk4.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      },
      ...
      {
        "name": "datadisk63",
        "lun": 62,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk63.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      },
      {
        "name": "datadisk64",
        "lun": 63,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk64.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      }
    ]
  },
  "resources": [
    ...
    {
      "type": "Microsoft.Compute/virtualMachines",
      "properties": {
        ...
        "storageProfile": {
          ...
          "dataDisks": "[take(variables('diskArray'),parameters('numDataDisks'))]"
        },
        ...
      }
      ...
    }
  ]
}
```


## <a name="next-steps"></a>Nächste Schritte
- Wenn Sie wissen möchten, in den Abschnitten einer Vorlage möchten, finden Sie unter [Authoring Azure Ressourcenmanager Vorlagen](./resource-group-authoring-templates.md).
- Alle Funktionen, die Sie in eine Vorlage verwenden können, finden Sie unter [Azure Ressourcenmanager Vorlage Funktionen](./resource-group-template-functions.md).
- Gewusst wie: Bereitstellen eine Vorlage finden Sie unter [Bereitstellen einer Anwendung mit Azure Ressourcenmanager Vorlage](resource-group-template-deploy.md).

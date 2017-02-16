<properties
   pageTitle="Vorlagen mit Ressourcenmanager verknüpft | Microsoft Azure"
   description="Beschreibt, wie von verknüpften Vorlagen in einer Ressourcenmanager Azure-Vorlage zum Erstellen einer Lösung modulare Vorlage. Zeigt, wie Parameterwerte übergeben, geben Sie eine Parameterdatei, und dynamisch erstellte URLs."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/02/2016"
   ms.author="tomfitz"/>

# <a name="using-linked-templates-with-azure-resource-manager"></a>Verwenden von verknüpften Vorlagen Azure Ressourcenmanager

Aus können innerhalb einer Vorlage Azure Ressourcenmanager, Sie link in eine andere Vorlage, womit Sie die Bereitstellung in eine Reihe von Gliedern als Ziel kann, Zweck-spezifische Vorlagen. Ebenso wie zerlegen Anwendung in mehreren Codeklassen, bietet Gliederung Vorteile im Hinblick auf testen, Wiederverwendung und Lesbarkeit.  

Sie können Parameter zu einer verknüpften Vorlage aus einer Vorlage Hauptfenster übergeben, und diese Parameter können direkt Parameter oder Variablen verfügbar gemacht werden, durch die Vorlage für einen ordnen. Die verknüpfte Vorlage kann auch eine Ausgabevariable übergeben wieder auf die Quellvorlage aktivieren eine bidirektionale Datenaustausch zwischen Vorlagen.

## <a name="linking-to-a-template"></a>Verknüpfen mit einer Vorlage

Sie erstellen eine Verknüpfung zwischen zwei Vorlagen durch Hinzufügen einer Ressource Bereitstellung innerhalb der Hauptfenster Vorlage, die auf die verknüpfte Vorlage verweist. Sie legen Sie die **TemplateLink** -Eigenschaft an den URI der verknüpfte Vorlage. Sie können Parameterwerte für die verknüpfte Vorlage entweder durch Angeben der Werte in der Vorlage direkt oder durch Verknüpfen einer Datei Parameter angeben. Im folgende Beispiel wird die Eigenschaft **Parameter** direkt an einen Parameterwert verwendet.

    "resources": [ 
      { 
         "apiVersion": "2015-01-01", 
         "name": "linkedTemplate", 
         "type": "Microsoft.Resources/deployments", 
         "properties": { 
           "mode": "incremental", 
           "templateLink": {
              "uri": "https://www.contoso.com/AzureTemplates/newStorageAccount.json",
              "contentVersion": "1.0.0.0"
           }, 
           "parameters": { 
              "StorageAccountName":{"value": "[parameters('StorageAccountName')]"} 
           } 
         } 
      } 
    ] 

Ressourcenmanager Dienst muss die verknüpfte Vorlage zugreifen können. Sie können keine angeben einer lokalen Datei oder einer Datei, die nur in Ihrem lokalen Netzwerk für die verknüpfte Vorlage zur Verfügung steht. Sie können nur einen URI-Wert bereitstellen, der entweder **http** oder **Https**enthält. Eine Möglichkeit ist, platzieren Sie Ihre Vorlage verknüpfte in einem Speicherkonto, und verwenden Sie den URI für dieses Element, wie im folgenden Beispiel gezeigt.

    "templateLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
        "contentVersion": "1.0.0.0",
    }

Obwohl die verknüpfte Vorlage extern verfügbar sein muss, muss es nicht in der Regel für die Öffentlichkeit zur Verfügung. Sie können Ihre Vorlage mit einem privaten Speicherkonto hinzufügen, die nur der Besitzer der Speicher-Konto zugegriffen werden kann. Klicken Sie dann erstellen Sie ein freigegebenes Access Signatur (SAS) Token zum Aktivieren des Zugriffs während der Bereitstellung. Die SAS Token hinzugefügt den URI für die verknüpfte Vorlage. Schritte zum Einrichten einer Vorlage in einem Speicherkonto und ein Token SAS generieren finden Sie unter Bereitstellen mit Ressourcenmanager Vorlagen und Azure PowerShell oder [Ressourcen](resource-group-template-deploy.md) [Bereitstellen mit Ressourcenmanager Vorlagen und Azure CLI](resource-group-template-deploy-cli.md). 

Im folgenden Beispiel wird einer übergeordnete Vorlage mit einer Verknüpfung in eine andere Vorlage. Die verknüpfte Vorlage wird mit einem Token SAS zugegriffen werden, die als Parameter übergeben.

    "parameters": {
        "sasToken": { "type": "securestring" }
    },
    "resources": [
        {
            "apiVersion": "2015-01-01",
            "name": "linkedTemplate",
            "type": "Microsoft.Resources/deployments",
            "properties": {
              "mode": "incremental",
              "templateLink": {
                "uri": "[concat('https://storagecontosotemplates.blob.core.windows.net/templates/helloworld.json', parameters('sasToken'))]",
                "contentVersion": "1.0.0.0"
              }
            }
        }
    ],

Obwohl das Token als eine sichere Zeichenfolge übergeben wird, wird der URI des verknüpften Vorlage, einschließlich des Tokens SAS in die Bereitstellungsvorgänge dieser Ressourcengruppe protokolliert. Zum Anzeigen zu beschränken, legen Sie ein Ablaufdatum für das Token.

## <a name="linking-to-a-parameter-file"></a>Verknüpfen mit einer Parameterdatei

Im nächste Beispiel wird die Eigenschaft **ParametersLink** zur Verknüpfung mit einer Parameterdatei verwendet.

    "resources": [ 
      { 
         "apiVersion": "2015-01-01", 
         "name": "linkedTemplate", 
         "type": "Microsoft.Resources/deployments", 
         "properties": { 
           "mode": "incremental", 
           "templateLink": {
              "uri":"https://www.contoso.com/AzureTemplates/newStorageAccount.json",
              "contentVersion":"1.0.0.0"
           }, 
           "parametersLink": { 
              "uri":"https://www.contoso.com/AzureTemplates/parameters.json",
              "contentVersion":"1.0.0.0"
           } 
         } 
      } 
    ] 

URI-Werts für die Parameterdatei verknüpften keine lokale Datei sein, und Sie müssen entweder **http** oder **Https**einschließen. Die Parameterdatei kann auch beschränkter Zugriff über ein SAS Token sein.

## <a name="using-variables-to-link-templates"></a>Verwenden von Variablen zum Verknüpfen von Vorlagen

In den vorherigen Beispielen gezeigt hartcodierte URL-Werte für die Vorlage Links. Dieser Ansatz für eine einfache Vorlage funktionieren möglicherweise, aber es funktioniert nicht, auch bei der Arbeit mit einer großen Anzahl modulare Vorlagen. Stattdessen können erstellen Sie eine statische Variable, in dem eine base-URL für die wichtigsten Vorlage gespeichert und erstellen dann dynamisch URLs für die verknüpften Vorlagen aus dieser Basis-URL. Die Vorteile von diesem Ansatz ist, können Sie einfach verschieben oder Verzweigung die Vorlage aus, denn Sie nur die statische Variable in das Hauptfenster Vorlage ändern müssen. Die Vorlage Hauptfenster übergibt die richtige URIs in der gesamten zerlegte Vorlage.

Im folgenden Beispiel wird gezeigt, wie eine base-URL zum Erstellen von zwei URLs für verknüpfte Vorlagen (**SharedTemplateUrl** und **VmTemplate**) verwenden. 

    "variables": {
        "templateBaseUrl": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/postgresql-on-ubuntu/",
        "sharedTemplateUrl": "[concat(variables('templateBaseUrl'), 'shared-resources.json')]",
        "tshirtSizeSmall": {
            "vmSize": "Standard_A1",
            "diskSize": 1023,
            "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
            "vmCount": 2,
            "slaveCount": 1,
            "storage": {
                "name": "[parameters('storageAccountNamePrefix')]",
                "count": 1,
                "pool": "db",
                "map": [0,0],
                "jumpbox": 0
            }
        }
    }

Sie können auch [Freigabe deklarieren()](resource-group-template-functions.md#deployment) verwenden, um die Basis-URL für die aktuelle Vorlage zu erhalten und verwenden, können Sie um die URL für die anderen Vorlagen an derselben Stelle zu gelangen. Dieser Ansatz ist hilfreich, wenn der Speicherort für die Vorlage (möglicherweise aufgrund einer Versioning ändert) oder vermeiden codieren URLs in die Vorlagendatei verwendet werden soll. 

    "variables": {
        "sharedTemplateUrl": "[uri(deployment().properties.templateLink.uri, 'shared-resources.json')]"
    }

## <a name="conditionally-linking-to-templates"></a>Bedingt Verknüpfen mit Vorlagen

Sie können verschiedene Vorlagen durch die Übergabe einen Parameterwert verknüpfen, die zum Erstellen des URI der verknüpfte Vorlage verwendet wird. Dieser Ansatz funktioniert gut, wenn Sie während der Bereitstellung angeben, die zu verwendende Vorlage verknüpft. Beispielsweise können Sie eine Vorlage für ein vorhandenes Speicherkonto verwendet werden soll, und eine andere Vorlage für ein neues Speicherkonto angeben.

Das folgende Beispiel zeigt einen Parameter für einen Kontonamen Speicher sowie einen Parameter angeben, ob das Speicherkonto neuen oder vorhandenen ist.

    "parameters": {
        "storageAccountName": {
            "type": "String"
        },
        "newOrExisting": {
            "type": "String",
            "allowedValues": [
                "new",
                "existing"
            ]
        }
    },

Sie erstellen eine Variable für die Vorlage URI, der den Wert des neuen oder vorhandenen Parameters enthält.

    "variables": {
        "templatelink": "[concat('https://raw.githubusercontent.com/exampleuser/templates/master/',parameters('newOrExisting'),'StorageAccount.json')]"
    },

Sie bieten Variable Werts für die Bereitstellung Ressource.

    "resources": [
        {
            "apiVersion": "2015-01-01",
            "name": "linkedTemplate",
            "type": "Microsoft.Resources/deployments",
            "properties": {
                "mode": "incremental",
                "templateLink": {
                    "uri": "[variables('templatelink')]",
                    "contentVersion": "1.0.0.0"
                },
                "parameters": {
                    "StorageAccountName": {
                        "value": "[parameters('storageAccountName')]"
                    }
                }
            }
        }
    ],

Der URI aufgelöst zu einer Vorlage mit der **existingStorageAccount.json** oder **newStorageAccount.json**. Erstellen von Vorlagen für diese URIs.

Im folgenden Beispiel wird die **existingStorageAccount.json** -Vorlage.

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "storageAccountName": {
          "type": "String"
        }
      },
      "variables": {},
      "resources": [],
      "outputs": {
        "storageAccountInfo": {
          "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName')),providers('Microsoft.Storage', 'storageAccounts').apiVersions[0])]",
          "type" : "object"
        }
      }
    }

Im nächste Beispiel wird die Vorlage **newStorageAccount.json** . Beachten Sie, dass wie den vorhandenen Speicher Kontovorlage im Speicher Kontoobjekt in die Ausgaben zurückgegeben wird. Die Gestaltungsvorlage Vorlage funktioniert mit entweder verknüpfte Vorlage.

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "storageAccountName": {
          "type": "string"
        }
      },
      "resources": [
        {
          "type": "Microsoft.Storage/storageAccounts",
          "name": "[parameters('StorageAccountName')]",
          "apiVersion": "2016-01-01",
          "location": "[resourceGroup().location]",
          "sku": {
            "name": "Standard_LRS"
          },
          "kind": "Storage",
          "properties": {
          }
        }
      ],
      "outputs": {
        "storageAccountInfo": {
          "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('StorageAccountName')),providers('Microsoft.Storage', 'storageAccounts').apiVersions[0])]",
          "type" : "object"
        }
      }
    }

## <a name="complete-example"></a>Vollständiges Beispiel

Im folgenden Beispielvorlagen anzeigen eine vereinfachte Anordnung von verknüpften Vorlagen Erläutern Sie unterschiedliche die Konzepte in diesem Artikel. Es wird davon ausgegangen, dass die Vorlagen auf den gleichen Container in einem Speicherkonto mit öffentlichem Zugriff deaktiviert hinzugefügt wurden. Die verknüpfte Vorlage übergibt einen Wert wieder in die wichtigsten Vorlage im Abschnitt **ausgegeben** .

Die Datei **parent.json** besteht aus:

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "containerSasToken": { "type": "string" }
      },
      "resources": [
        {
          "apiVersion": "2015-01-01",
          "name": "linkedTemplate",
          "type": "Microsoft.Resources/deployments",
          "properties": {
            "mode": "incremental",
            "templateLink": {
              "uri": "[concat(uri(deployment().properties.templateLink.uri, 'helloworld.json'), parameters('containerSasToken'))]",
              "contentVersion": "1.0.0.0"
            }
          }
        }
      ],
      "outputs": {
        "result": {
          "type": "object",
          "value": "[reference('linkedTemplate').outputs.result]"
        }
      }
    }

Die Datei **helloworld.json** besteht aus:

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {},
      "variables": {},
      "resources": [],
      "outputs": {
        "result": {
            "value": "Hello World",
            "type" : "string"
        }
      }
    }
    
In PowerShell ein Token für den Container abrufen und Bereitstellen von Vorlagen mit:

    Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates
    $token = New-AzureStorageContainerSASToken -Name templates -Permission r -ExpiryTime (Get-Date).AddMinutes(30.0)
    New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateUri ("https://storagecontosotemplates.blob.core.windows.net/templates/parent.json" + $token) -containerSasToken $token

In Azure CLI erhalten ein Token für den Container und Bereitstellen von Vorlagen mit den folgenden Code. Aktuell, müssen Sie einen Namen für die Bereitstellung eine Vorlage URI nutzen können, die ein SAS Token enthält.  

    expiretime=$(date -I'minutes' --date "+30 minutes")  
    azure storage container sas create --container templates --permissions r --expiry $expiretime --json | jq ".sas" -r
    azure group deployment create -g ExampleGroup --template-uri "https://storagecontosotemplates.blob.core.windows.net/templates/parent.json?{token}" -n tokendeploy  

Sie werden aufgefordert, das SAS Token als Parameter bereitzustellen. Sie müssen das Token mit **?**voran.

## <a name="next-steps"></a>Nächste Schritte
- Weitere Informationen zum Definieren der Bereitstellungsreihenfolge für Ressourcen, finden Sie unter [Definieren von Abhängigkeiten in Azure Ressourcenmanager Vorlagen](resource-group-define-dependencies.md)
- Wenn Sie erfahren, wie Sie eine Ressource definieren, aber viele Instanzen erstellen, finden Sie unter [Erstellen mehrerer Instanzen von Ressourcen in Azure Ressourcenmanager](resource-group-create-multiple.md)

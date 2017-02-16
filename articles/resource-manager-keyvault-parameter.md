<properties
   pageTitle="Taste Tresor geheim Ressourcenmanager Vorlage | Microsoft Azure"
   description="Zeigt, wie ein Geheimnis während der Bereitstellung von einer Key Tresor als Parameter zu übergeben."
   services="azure-resource-manager,key-vault"
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
   ms.date="06/23/2016"
   ms.author="tomfitz"/>

# <a name="pass-secure-values-during-deployment"></a>Sichere Werte übergeben, während der Bereitstellung

Wenn Sie während der Bereitstellung einen sicheren Wert (wie etwa ein Kennwort) als Parameter zu übergeben müssen, können Sie Wert als Geheimnis in einer [Azure Schlüssel Tresor](./key-vault/key-vault-whatis.md) zu speichern und den Wert in anderen Ressourcenmanager Vorlagen verweisen. Sie einbeziehen nur einen Verweis auf den Schlüssel, damit die geheim ist niemals offen gelegt, und Sie müssen nicht manuell den Wert für den Schlüssel jedes Mal eingeben Bereitstellen von Ressourcen in Ihre Vorlage. Sie angeben, welche Benutzer oder Dienst Hauptbenutzer die geheim zugreifen können.  

## <a name="deploy-a-key-vault-and-secret"></a>Bereitstellen einer Key Tresor und geheim

Um Key Tresor erstellen, die von anderen Ressourcenmanager Vorlagen verwiesen werden kann, müssen Sie die Eigenschaft **EnabledForTemplateDeployment** auf **true**festlegen, und müssen Sie Zugriff gewähren, dem Benutzer oder Dienst Tilgungsanteile, die die Bereitstellung ausgeführt wird, die die geheim verweist.

Weitere Informationen zum Bereitstellen einer Key Tresor und geheim, finden Sie unter [Key Tresor Schema](resource-manager-template-keyvault.md) und [Schlüssel Tresor geheimen Schema](resource-manager-template-keyvault-secret.md).

## <a name="reference-a-secret-with-static-id"></a>Bezug auf ein Geheimnis mit statischen-id

Das Geheimnis von innerhalb einer Parameterdatei, die Werte Ihrer Vorlage übergibt verwiesen werden. Sie verweisen die geheim und den Resource Identifier für die wichtigsten Tresor sowie den Namen der geheim übergeben. In diesem Beispiel die wichtigsten Tresor geheim muss bereits vorhanden sein, und Sie verwenden ein statischer Wert dafür Ressource-Id.

    "parameters": {
      "adminPassword": {
        "reference": {
          "keyVault": {
            "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
          }, 
          "secretName": "sqlAdminPassword" 
        } 
      }
    }

Wie kann eine Datei für die gesamte Parameter aussehen:

    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "sqlsvrAdminLogin": {
          "value": ""
        },
        "sqlsvrAdminLoginPassword": {
          "reference": {
            "keyVault": {
              "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
            },
            "secretName": "adminPassword"
          }
        }
      }
    }

Der Parameter, der das Geheimnis akzeptiert sollte eine **Securestring**sein. Das folgende Beispiel zeigt die entsprechenden Abschnitten einer Vorlage, die einen SQLServer bereitstellt, der ein Administratorkennwort erforderlich sind.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "sqlsvrAdminLogin": {
                "type": "string",
                "minLength": 4
            },
            "sqlsvrAdminLoginPassword": {
                "type": "securestring"
            },
            ...
        },
        "resources": [
        {
            "name": "[variables('sqlsvrName')]",
            "type": "Microsoft.Sql/servers",
            "location": "[resourceGroup().location]",
            "apiVersion": "2014-04-01-preview",
            "properties": {
                "administratorLogin": "[parameters('sqlsvrAdminLogin')]",
                "administratorLoginPassword": "[parameters('sqlsvrAdminLoginPassword')]"
            },
            ...
        }],
        "variables": {
            "sqlsvrName": "[concat('sqlsvr', uniqueString(resourceGroup().id))]"
        },
        "outputs": { }
    }

## <a name="reference-a-secret-with-dynamic-id"></a>Bezug auf einen Schlüssel mit den dynamischen-id

Im vorherigen Abschnitt gezeigt, wie eine statische Ressource-Id für die wichtigsten Tresor geheim übergeben. In einigen Fällen müssen Sie jedoch ein Key Tresor Geheimnis, deren variiert, basierend auf der aktuellen Bereitstellung verweisen. In diesem Fall nicht programmiert die Ressourcen-Id in der Parameterdatei. Leider können Sie dynamisch die Ressourcen-Id in der Parameterdatei generiert Vorlagenausdrücke in der Parameterdatei nicht zulässig sind.

Um die Ressourcen-Id für einen geheimen Key Tresor dynamisch zu generieren, müssen Sie die Ressource verschieben, die die geheim in eine verschachtelte Vorlage zu erledigen. In der master-Vorlage fügen Sie die verschachtelte Vorlage und übergeben Sie Parameter, der die dynamisch generierte Ressourcen-Id enthält.

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "vaultName": {
          "type": "string"
        },
        "secretName": {
          "type": "string"
        }
      },
      "resources": [
        {
          "apiVersion": "2015-01-01",
          "name": "nestedTemplate",
          "type": "Microsoft.Resources/deployments",
          "properties": {
            "mode": "incremental",
            "templateLink": {
              "uri": "https://www.contoso.com/AzureTemplates/newVM.json",
              "contentVersion": "1.0.0.0"
            },
            "parameters": {
              "adminPassword": {
                "reference": {
                  "keyVault": {
                    "id": "[concat(resourceGroup().id, '/providers/Microsoft.KeyVault/vaults/', parameters('vaultName'))]"
                  },
                  "secretName": "[parameters('secretName')]"
                }
              }
            }
          }
        }
      ],
      "outputs": {}
    }


## <a name="next-steps"></a>Nächste Schritte

- Allgemeine Informationen zu den wichtigsten Depots finden Sie unter [Erste Schritte mit Azure Schlüssel Tresor](./key-vault/key-vault-get-started.md).
- Informationen zur Verwendung eines Key Tresor mit einem virtuellen Computer finden Sie unter [Sicherheitsaspekte für Azure Ressourcenmanager](best-practices-resource-manager-security.md).
- Vollständige Beispiele für Key geheimen Informationen verweisen finden Sie unter [Key Tresor Beispielen](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples).


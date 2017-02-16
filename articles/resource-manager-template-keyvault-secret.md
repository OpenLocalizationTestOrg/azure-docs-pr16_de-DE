<properties
   pageTitle="Ressourcenmanager-Vorlage für ein Geheimnis in einem Key Tresor | Microsoft Azure"
   description="Zeigt das Schema Ressourcenmanager für die Bereitstellung von Key Tresor geheimen Daten über eine Vorlage."
   services="azure-resource-manager,key-vault"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/23/2016"
   ms.author="tomfitz"/>

# <a name="key-vault-secret-template-schema"></a>Key Tresor geheimen Vorlage schema

Erstellt ein Geheimnis, die in einem Key Tresor gespeichert ist. Dieser Ressourcentyp wird häufig als Ressource untergeordnetes Element des [Key Tresor](resource-manager-template-keyvault.md)bereitgestellt.

## <a name="schema-format"></a>Schemaformat

Um einen geheimen Schlüssel Tresor zu erstellen, fügen Sie das folgende Schema zu Ihrer Vorlage aus. Das Geheimnis kann als eine untergeordnete Ressource einer Key Wölbung oder Ressourcen auf oberster Ebene definiert werden. Sie können ihn als untergeordnete Ressource definieren, wenn der wichtige Tresor in derselben Vorlage bereitgestellt wird. Sie müssen den geheimen als Ressource auf oberster Ebene zu definieren, wenn der wichtige Tresor nicht in derselben Vorlage bereitgestellt wird oder wenn müssen Sie mehrere Kennwörter erstellen, indem Sie auf die Ressourcenart Schleifen. 

    {
        "type": enum,
        "apiVersion": "2015-06-01",
        "name": string,
        "properties": {
            "value": string
        },
        "dependsOn": [ array values ]
    }

## <a name="values"></a>Werte

In den folgenden Tabellen werden die Werte, die Sie in das Schema festlegen müssen.

| Namen | Wert |
| ---- | ---- | 
| Typ | Aufzählung<br />Erforderlich<br />**Kennwörter** (Wenn als Ressource untergeordnetes Element des Key Tresor bereitgestellt) oder<br /> **Microsoft.KeyVault/vaults/secrets** (Wenn als auf oberster Ebene Ressource bereitgestellt)<br /><br />Die Ressourcenart zu erstellen. |
| apiVersion | Aufzählung<br />Erforderlich<br />**2015-06-01** oder **2014-12-19-Vorschau**<br /><br />Die API-Version für die Ressource zu erstellen. | 
| Namen | Zeichenfolge<br />Erforderlich<br />Ein einzelnes Wort wird als eine untergeordnete Ressource einer Key Wölbung oder im Format bereitgestellt **{Schlüssel Tresor Name} / {geheim – Name}** wird als eine auf oberster Ebene Ressource einer vorhandenen Key Tresor hinzugefügt werden bereitgestellt.<br /><br />Der Name des geheimen Schlüssels zu erstellen. |
| Eigenschaften | Objekt<br />Erforderlich<br />[Properties-Objekt](#properties)<br /><br />Ein Objekt, die angibt, den Wert für die geheimen zu erstellen. |
| dependsOn | Matrix<br />Optional<br />Eine durch Trennzeichen getrennte Liste einer Ressource Namen oder Ressource eindeutigen Bezeichner.<br /><br />Die Auflistung von Ressourcen, die, denen diesen Link abhängt. Wenn die wichtigsten Tresor für das Geheimnis in derselben Vorlage bereitgestellt wird, fügen Sie den Namen der die wichtigsten Tresor in dieses Element aus, um sicherzustellen, dass es zuerst bereitgestellt wird ein. |

<a id="properties" />
### <a name="properties-object"></a>Properties-Objekt

| Namen | Wert |
| ---- | ---- | 
| Wert | Zeichenfolge<br />Erforderlich<br /><br />Der geheimen Wert in die wichtigsten Tresor gespeichert. Wenn Sie einen Wert für diese Eigenschaft zu übergeben, verwenden Sie einen Parameter vom Typ **Securestring**aus.  |

    
## <a name="examples"></a>Beispiele

Im erste Beispiel wird ein Geheimnis als untergeordnete Ressource einer Key Wölbung bereitgestellt.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "keyVaultName": {
                "type": "string",
                "metadata": {
                    "description": "Name of the vault"
                }
            },
            "tenantId": {
                "type": "string",
                "metadata": {
                   "description": "Tenant ID for the subscription and use assigned access to the vault. Available from the Get-AzureRmSubscription PowerShell cmdlet"
                }
            },
            "objectId": {
                "type": "string",
                "metadata": {
                    "description": "Object ID of the AAD user or service principal that will have access to the vault. Available from the Get-AzureRmADUser or the Get-AzureRmADServicePrincipal cmdlets"
                }
            },
            "keysPermissions": {
                "type": "array",
                "defaultValue": [ "all" ],
                "metadata": {
                    "description": "Permissions to grant user to keys in the vault. Valid values are: all, create, import, update, get, list, delete, backup, restore, encrypt, decrypt, wrapkey, unwrapkey, sign, and verify."
                }
            },
            "secretsPermissions": {
                "type": "array",
                "defaultValue": [ "all" ],
                "metadata": {
                    "description": "Permissions to grant user to secrets in the vault. Valid values are: all, get, set, list, and delete."
                }
            },
            "vaultSku": {
                "type": "string",
                "defaultValue": "Standard",
                "allowedValues": [
                    "Standard",
                    "Premium"
                ],
                "metadata": {
                    "description": "SKU for the vault"
                }
            },
            "enabledForDeployment": {
                "type": "bool",
                "defaultValue": false,
                "metadata": {
                    "description": "Specifies if the vault is enabled for VM or Service Fabric deployment"
                }
            },
            "enabledForTemplateDeployment": {
                "type": "bool",
                "defaultValue": false,
                "metadata": {
                    "description": "Specifies if the vault is enabled for ARM template deployment"
                }
            },
            "enableVaultForVolumeEncryption": {
                "type": "bool",
                "defaultValue": false,
                "metadata": {
                    "description": "Specifies if the vault is enabled for volume encryption"
                }
            },
            "secretName": {
                "type": "string",
                "metadata": {
                    "description": "Name of the secret to store in the vault"
                }
            },
            "secretValue": {
                "type": "securestring",
                "metadata": {
                    "description": "Value of the secret to store in the vault"
                }
            }
        },
        "resources": [
        {
            "type": "Microsoft.KeyVault/vaults",
            "name": "[parameters('keyVaultName')]",
            "apiVersion": "2015-06-01",
            "location": "[resourceGroup().location]",
            "tags": {
                "displayName": "KeyVault"
            },
            "properties": {
                "enabledForDeployment": "[parameters('enabledForDeployment')]",
                "enabledForTemplateDeployment": "[parameters('enabledForTemplateDeployment')]",
                "enabledForVolumeEncryption": "[parameters('enableVaultForVolumeEncryption')]",
                "tenantId": "[parameters('tenantId')]",
                "accessPolicies": [
                {
                    "tenantId": "[parameters('tenantId')]",
                    "objectId": "[parameters('objectId')]",
                    "permissions": {
                        "keys": "[parameters('keysPermissions')]",
                        "secrets": "[parameters('secretsPermissions')]"
                    }
                }],
                "sku": {
                    "name": "[parameters('vaultSku')]",
                    "family": "A"
                }
            },
            "resources": [
            {
                "type": "secrets",
                "name": "[parameters('secretName')]",
                "apiVersion": "2015-06-01",
                "properties": {
                    "value": "[parameters('secretValue')]"
                },
                "dependsOn": [
                    "[concat('Microsoft.KeyVault/vaults/', parameters('keyVaultName'))]"
                ]
            }]
        }]
    }

Das zweite Beispiel bereitstellt das Geheimnis als Ressource auf oberster Ebene, die in einer vorhandenen Key Tresor gespeichert ist.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "keyVaultName": {
                "type": "string",
                "metadata": {
                    "description": "Name of the existing vault"
                }
            },
            "secretName": {
                "type": "string",
                "metadata": {
                    "description": "Name of the secret to store in the vault"
                }
            },
            "secretValue": {
                "type": "securestring",
                "metadata": {
                    "description": "Value of the secret to store in the vault"
                }
            }
        },
        "variables": {},
        "resources": [
            {
                "type": "Microsoft.KeyVault/vaults/secrets",
                "apiVersion": "2015-06-01",
                "name": "[concat(parameters('keyVaultName'), '/', parameters('secretName'))]",
                "properties": {
                    "value": "[parameters('secretValue')]"
                }
            }
        ],
        "outputs": {}
    }


## <a name="next-steps"></a>Nächste Schritte

- Allgemeine Informationen zu den wichtigsten Depots finden Sie unter [Erste Schritte mit Azure Schlüssel Tresor](./key-vault/key-vault-get-started.md).
- Ein Beispiel für ein Geheimnis Key Tresor verweisen, beim Bereitstellen von Vorlagen finden Sie unter [secure Werte während der Bereitstellung zu übergeben](resource-manager-keyvault-parameter.md).



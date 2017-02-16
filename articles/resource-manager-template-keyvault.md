<properties
   pageTitle="Ressourcenmanager Vorlage für Key Tresor | Microsoft Azure"
   description="Zeigt das Schema Ressourcenmanager für die Bereitstellung von Key Depots über eine Vorlage."
   services="azure-resource-manager,key-vault"
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
   ms.date="06/23/2016"
   ms.author="tomfitz"/>

# <a name="key-vault-template-schema"></a>Key Tresor Vorlage schema

Erstellt eine Key Tresor.

## <a name="schema-format"></a>Schemaformat

Um eine Key Tresor zu erstellen, fügen Sie das folgende Schema zum Ressourcenabschnitt der Vorlage ein.

    {
        "type": "Microsoft.KeyVault/vaults",
        "apiVersion": "2015-06-01",
        "name": string,
        "location": string,
        "properties": {
            "enabledForDeployment": bool,
            "enabledForTemplateDeployment": bool,
            "enabledForVolumeEncryption": bool,
            "tenantId": string,
            "accessPolicies": [
                {
                    "tenantId": string,
                    "objectId": string,
                    "permissions": {
                        "keys": [ keys permissions ],
                        "secrets": [ secrets permissions ]
                    }
                }
            ],
            "sku": {
                "name": enum,
                "family": "A"
            }
        },
        "resources": [
             child resources
        ]
    }

## <a name="values"></a>Werte

In den folgenden Tabellen werden die Werte, die Sie in das Schema festlegen müssen.

| Namen | Wert |
| ---- | ---- | 
| Typ | Aufzählung<br />Erforderlich<br />**Microsoft.KeyVault/vaults**<br /><br />Die Ressourcenart zu erstellen. |
| apiVersion | Aufzählung<br />Erforderlich<br />**2015-06-01** oder **2014-12-19-Vorschau**<br /><br />Die API-Version für die Ressource zu erstellen. | 
| Namen | Zeichenfolge<br />Erforderlich<br />Einen Namen, der Azure eindeutig ist.<br /><br />Der Name der die wichtigsten Tresor zu erstellen. Sollten Sie die Funktion [UniqueString](resource-group-template-functions.md#uniquestring) mit Ihrer Benennungskonvention So erstellen einen eindeutigen Namen ein, wie im folgenden Beispiel dargestellt. |
| Speicherort | Zeichenfolge<br />Erforderlich<br />Ein gültiger Bereich für Key Depots. Um gültige Regionen zu ermitteln, finden Sie unter [Regionen unterstützt](resource-manager-supported-services.md#supported-regions).<br /><br />Der Bereich den Key Tresor gehostet werden soll. |
| Eigenschaften | Objekt<br />Erforderlich<br />[Properties-Objekt](#properties)<br /><br />Ein Objekt, das den Typ des Key Tresor erstellen angibt. |
| Ressourcen | Matrix<br />Optional<br />Werte zulässig: [Key Tresor geheimen Ressourcen](resource-manager-template-keyvault-secret.md)<br /><br />Untergeordnete Ressourcen für die wichtigsten Tresor. |

<a id="properties" />
### <a name="properties-object"></a>Properties-Objekt

| Namen | Wert |
| ---- | ---- | 
| enabledForDeployment | Boolesch<br />Optional<br />**true** oder **false**<br /><br />Gibt an, ob der Tresor für die Bereitstellung von virtuellen Computern oder Dienst Fabric aktiviert ist. |
| enabledForTemplateDeployment | Boolesch<br />Optional<br />**true** oder **false**<br /><br />Gibt an, ob der Tresor zur Verwendung in Ressourcenmanager Vorlage Bereitstellungen aktiviert ist. Weitere Informationen finden Sie unter [übergeben secure Werte während der Bereitstellung](resource-manager-keyvault-parameter.md) |
| enabledForVolumeEncryption | Boolesch<br />Optional<br />**true** oder **false**<br /><br />Gibt an, ob der Tresor für die Verschlüsselung der Lautstärke aktiviert ist. |
| tenantId | Zeichenfolge<br />Erforderlich<br />**Global eindeutiger Bezeichner**<br /><br />Der Mandant Bezeichner für das Abonnement. Sie können mit der PowerShell-Cmdlet [Get-AzureRmSubscription](https://msdn.microsoft.com/library/azure/mt619284.aspx) oder der **Azure-Konto anzeigen** Azure CLI-Befehl abgerufen werden. |
| accessPolicies | Matrix<br />Erforderlich<br />[AccessPolicies Objekt](#accesspolicies)<br /><br />Ein Array von bis zu 16 Objekte, die die Berechtigungen für den Benutzer oder Dienst Tilgungsanteile an. |
| SKU | Objekt<br />Erforderlich<br />[Objekt SKU](#sku)<br /><br />Die SKU für die wichtigsten Tresor. |

<a id="accesspolicies" />
### <a name="propertiesaccesspolicies-object"></a>properties.accessPolicies Objekt

| Namen | Wert |
| ---- | ---- | 
| tenantId | Zeichenfolge<br />Erforderlich<br />**Global eindeutiger Bezeichner**<br /><br />Der Mandant Bezeichner der Azure-Active Directory-Mandanten, enthält die **ObjectId** in dieser Zugriffsrichtlinie |
| objectId | Zeichenfolge<br />Erforderlich<br />**Global eindeutiger Bezeichner**<br /><br />Die Objekt-ID des Benutzers Azure Active Directory oder Dienst Tilgungsanteile, die Zugriff auf die Tresor. Sie können den Wert von der [Get-AzureRmADUser](https://msdn.microsoft.com/library/azure/mt679001.aspx) oder die [Get-AzureRmADServicePrincipal](https://msdn.microsoft.com/library/azure/mt678992.aspx) PowerShell-Cmdlets oder die **Benutzer Azure AD-** oder **Azure AD-sp** Azure CLI-Befehle abrufen. |
| Berechtigungen | Objekt<br />Erforderlich<br />[Berechtigungen Objekt](#permissions)<br /><br />Die Berechtigungen auf diesen Tresor zu Active Directory-Objekts. |

<a id="permissions" />
### <a name="propertiesaccesspoliciespermissions-object"></a>properties.accessPolicies.permissions Objekt

| Namen | Wert |
| ---- | ---- | 
| Tasten | Matrix<br />Erforderlich<br />**Alle** **Sicherungskopie**, **Erstellen**, **Entschlüsseln**, **Löschen**, **Verschlüsseln**, **erhalten**, **Importieren**, **Liste**, **Wiederherstellen**, **Melden Sie sich**, **Unwrapkey**, **Aktualisieren**, **Stellen Sie sicher**, **wrapkey**<br /><br />Die Berechtigungen auf Tasten in diesen Tresor zu diesem Active Directory-Objekt. Dieser Wert muss als Array von einem oder mehreren zulässigen Werten angegeben werden. |
| Kennwörter | Matrix<br />Erforderlich<br />**Alle** **Löschen**, **erhalten**, **Liste**, **festlegen**<br /><br />Die Berechtigungen auf vertrauliche Informationen in diesen Tresor zu diesem Active Directory-Objekt. Dieser Wert muss als Array von einem oder mehreren zulässigen Werten angegeben werden. |

<a id="sku" />
### <a name="propertiessku-object"></a>Properties.SKU Objekt

| Namen | Wert |
| ---- | ---- | 
| Namen | Aufzählung<br />Erforderlich<br />**Standard**oder **premium** <br /><br />Der Dienstebene der KeyVault verwenden.  Standard unterstützt Kennwörter und Schlüssel Software geschützt.  Premium addiert HSM geschützt werden Tastenkombinationen unterstützt. |
| Familie | Aufzählung<br />Erforderlich<br />**A** <br /><br />Die Sku Familie zu verwenden. |
 
    
## <a name="examples"></a>Beispiele

Im folgende Beispiel wird eine Key Tresor und geheim bereitgestellt.

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

## <a name="quickstart-templates"></a>Schnellstart-Vorlagen

Die folgende Schnellstart Vorlage bereitstellt, einem Key Tresor.

- [Erstellen von Key Tresor](https://azure.microsoft.com/documentation/templates/101-key-vault-create/)


## <a name="next-steps"></a>Nächste Schritte

- Allgemeine Informationen zu den wichtigsten Depots finden Sie unter [Erste Schritte mit Azure Schlüssel Tresor](./key-vault/key-vault-get-started.md).
- Ein Beispiel für ein Geheimnis Key Tresor verweisen, beim Bereitstellen von Vorlagen finden Sie unter [secure Werte während der Bereitstellung zu übergeben](resource-manager-keyvault-parameter.md).


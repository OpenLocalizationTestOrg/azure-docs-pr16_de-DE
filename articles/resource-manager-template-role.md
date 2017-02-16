<properties
   pageTitle="Ressourcenmanager Vorlage für rollenzuweisungen | Microsoft Azure"
   description="Zeigt das Schema Ressourcenmanager für die Bereitstellung von einer rollenzuweisung über eine Vorlage."
   services="azure-resource-manager"
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
   ms.date="10/03/2016"
   ms.author="tomfitz"/>

# <a name="role-assignments-template-schema"></a>Rolle Zuordnungen Vorlage schema

Ein Benutzer, die Gruppe oder den Dienst Tilgungsanteile weist einer Rolle mit einem bestimmten Bereich.

## <a name="resource-format"></a>Formatieren von Ressourcen

Um eine rollenzuweisung zu erstellen, fügen Sie das folgende Schema zum Ressourcenabschnitt der Vorlage ein.
    
    {
        "type": string,
        "apiVersion": "2015-07-01",
        "name": string,
        "dependsOn": [ array values ],
        "properties":
        {
            "roleDefinitionId": string,
            "principalId": string,
            "scope": string
        }
    }

## <a name="values"></a>Werte

In den folgenden Tabellen werden die Werte, die Sie in das Schema festlegen müssen.

| Namen | Erforderlich | Beschreibung |
| ---- | -------- | ----------- |
| Typ | Ja    | Die Ressourcenart zu erstellen.<br /><br /> Für Ressourcengruppe:<br />**Microsoft.Authorization/roleAssignments**<br /><br />Für Ressourcen:<br />**{Provider-Namespace} / {Ressourcenart} / Anbieter/RoleAssignments** |
| apiVersion |Ja | Die API-Version für die Ressource zu erstellen.<br /><br /> Verwenden Sie **2015-07-01**. | 
| Namen | Ja | Ein global eindeutigen Bezeichner für die neue Rolle Zuordnung. |
| dependsOn | Nein | Ein Array durch Listentrennzeichen getrenntes, einer Ressource Namen oder Ressource eindeutigen Bezeichner.<br /><br />Die Auflistung von Ressourcen, die, denen diese Rolle Zuordnung abhängt. Wenn eine Rolle zuweisen, die auf eine Ressource ausgelegte und, die Ressource in derselben Vorlage bereitgestellt wird, gehören Sie dem Ressourcennamen in diesem Element, um sicherzustellen, dass die Ressource zuerst bereitgestellt wird. |
| Eigenschaften | Ja | Die Eigenschaftenobjekt, das die Rollendefinition, Tilgungsanteile und Bereich identifiziert. |

### <a name="properties-object"></a>Properties-Objekt

| Namen | Erforderlich | Beschreibung |
| ---- | -------- | ----------- |
| roleDefinitionId | Ja |  Der Bezeichner für eine vorhandene Rollendefinition in der rollenzuweisung verwendet werden.<br /><br /> Verwenden Sie das folgende Format ein:<br /> **/ subscriptions/{subscription-id}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}** |
| principalId | Ja | Die globally unique Identifier für eine vorhandene Hauptbenutzer. Dieser Wert auf die Id innerhalb Verzeichnis Karten und kann ein Benutzer, der Tilgungsanteile Dienst oder Sicherheitsgruppe zeigen. |
| Bereich | Nein | Der Bereich, an dem diese Rolle Zuordnung gilt.<br /><br />Verwenden Sie für die Ressourcengruppen:<br />**/Subscriptions/ {Abonnement-Id} /resourceGroups/ {Ressource-Gruppennamen}**  <br /><br />Für Ressourcen verwenden:<br />**/ subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/{provider-namespace}/{resource-type}/{resource-name}** |  |


## <a name="how-to-use-the-role-assignment-resource"></a>So verwenden Sie die Rolle Zuordnung Ressource

Sie hinzufügen eine rollenzuweisung Ihrer Vorlage, wenn Sie einen Benutzer, die Gruppe oder den Dienst, die wichtigsten während der Bereitstellung zu einer Rolle hinzufügen müssen. Rollenzuweisungen höherer Ebenen des Gültigkeitsbereichs, geerbt werden, wenn Sie bereits eine Tilgungsanteile zu einer Rolle auf der Abonnementebene hinzugefügt haben, Sie nicht die Ressourcengruppe oder Ressource zuweisen müssen.

Es gibt viele Bezeichnerwerte, die Sie beim Arbeiten mit rollenzuweisungen angeben müssen. Sie können die Werte über PowerShell oder Azure CLI abrufen.

### <a name="powershell"></a>PowerShell

Der Name des rollenzuweisung erfordert einen globally unique Identifier. Sie können einen neuen Bezeichner für **Namen** mit zu erstellen:

    $name = [System.Guid]::NewGuid().toString()

Sie können die ID für Rollendefinition mit abrufen:

    $roledef = (Get-AzureRmRoleDefinition -Name Reader).id

Sie können die ID für den Hauptbenutzer mit einem der folgenden Befehle abrufen.

Für eine Gruppe namens **Auditoren**:

    $principal = (Get-AzureRmADGroup -SearchString Auditors).id

Für einen Benutzer mit dem Namen **Exampleperson**:

    $principal = (Get-AzureRmADUser -SearchString exampleperson).id

Für einen Dienst mit dem Namen der Tilgungsanteile **Exampleapp**:

    $principal = (Get-AzureRmADServicePrincipal -SearchString exampleapp).id 

### <a name="azure-cli"></a>Azure CLI

Sie können die ID für Rollendefinition mit abrufen:

    azure role show Reader --json | jq .[].Id -r

Sie können die ID für den Hauptbenutzer mit einem der folgenden Befehle abrufen.

Für eine Gruppe namens **Auditoren**:

    azure ad group show --search Auditors --json | jq .[].objectId -r

Für einen Benutzer mit dem Namen **Exampleperson**:

    azure ad user show --search exampleperson --json | jq .[].objectId -r

Für einen Dienst mit dem Namen der Tilgungsanteile **Exampleapp**:

    azure ad sp show --search exampleapp --json | jq .[].objectId -r

## <a name="examples"></a>Beispiele

Die folgende Vorlage erhält ein Bezeichner für eine Rolle und ein Bezeichner für einen Benutzer, die Gruppe oder den Dienst Tilgungsanteile. Die Rolle auf der Ebene der Ressource Gruppe zugewiesen wird.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "roleDefinitionId": {
                "type": "string"
            },
            "roleAssignmentId": {
                "type": "string"
            },
            "principalId": {
                "type": "string"
            }
        },
        "resources": [
            {
              "type": "Microsoft.Authorization/roleAssignments",
              "apiVersion": "2015-07-01",
              "name": "[parameters('roleAssignmentId')]",
              "properties":
              {
                "roleDefinitionId": "[concat(subscription().id, '/providers/Microsoft.Authorization/roleDefinitions/', parameters('roleDefinitionId'))]",
                "principalId": "[parameters('principalId')]",
                "scope": "[concat(subscription().id, '/resourceGroups/', resourceGroup().name)]"
              }
            }
        ],
        "outputs": {}
    }



Die nächste Vorlage erstellt ein Speicherkonto und dem Speicherkonto die Leserrolle zugewiesen. Die Bezeichner für zwei Gruppen und die Leserrolle haben in der Vorlage zur Vereinfachung der Bereitstellung enthalten. Diese Werte könnten zum Zeitpunkt der Bereitstellung bis in Skript abgerufen und als Parameter übergeben.

    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "roleName": {
          "type": "string"
        },
        "groupToAssign": {
          "type": "string",
          "allowedValues": [
            "Auditors",
            "Limited"
          ]
        }
      },
      "variables": {
        "readerRole": "[concat('/subscriptions/',subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7')]",
        "storageName": "[concat('storage', uniqueString(resourceGroup().id))]",
        "scope": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Storage/storageAccounts/', variables('storageName'))]",
        "Auditors": "1c272299-9729-462a-8d52-7efe5ece0c5c",
        "Limited": "7c7250f0-7952-441c-99ce-40de5e3e30b5",
        "fullRoleName": "[concat(variables('storageName'), '/Microsoft.Authorization/', parameters('roleName'))]"
      },
      "resources": [
        {
          "apiVersion": "2016-01-01",
          "type": "Microsoft.Storage/storageAccounts",
          "name": "[variables('storageName')]",
          "location": "[resourceGroup().location]",
          "sku": {
            "name": "Standard_LRS"
          },
          "kind": "Storage",
          "tags": {
            "displayName": "MyStorageAccount"
          },
          "properties": {}
        },
        {
          "type": "Microsoft.Storage/storageAccounts/providers/roleAssignments",
          "apiVersion": "2015-07-01",
          "name": "[variables('fullRoleName')]",
          "dependsOn": [
            "[variables('storageName')]"
          ],
          "properties": {
            "roleDefinitionId": "[variables('readerRole')]",
            "principalId": "[variables(parameters('groupToAssign'))]"
          }
        }
      ]
    }

## <a name="quickstart-templates"></a>Schnellstart-Vorlagen

Die folgenden Vorlagen zeigen, wie die Rolle Zuordnung Ressource verwenden:

- [Ressourcengruppe integrierten Rolle zuweisen](https://azure.microsoft.com/documentation/templates/101-rbac-builtinrole-resourcegroup)
- [Vorhandenen virtuellen Computer integrierten Rolle zuweisen](https://azure.microsoft.com/documentation/templates/101-rbac-builtinrole-virtualmachine)
- [Mehrere vorhandene virtuellen Computern integrierten Rolle zuweisen](https://azure.microsoft.com/documentation/templates/201-rbac-builtinrole-multipleVMs)

## <a name="next-steps"></a>Nächste Schritte

- Informationen über die Vorlagenstruktur finden Sie unter [Azure Ressourcenmanager Authoring-Vorlagen](resource-group-authoring-templates.md).
- Weitere Informationen zu Access rollenbasierte Steuerelement finden Sie unter [Azure Active Directory rollenbasierte Access Control](./active-directory/role-based-access-control-configure.md).

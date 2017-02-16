<properties
    pageTitle="Verwalten von Access rollenbasierte Steuerelements in die REST-API"
    description="Verwalten von Access rollenbasierte Steuerelements in die REST-API"
    services="active-directory"
    documentationCenter="na"
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="multiple"
    ms.tgt_pltfrm="rest-api"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="managing-role-based-access-control-with-the-rest-api"></a>Verwalten von Access rollenbasierte Steuerelements in die REST-API

> [AZURE.SELECTOR]
- [PowerShell](role-based-access-control-manage-access-powershell.md)
- [Azure CLI](role-based-access-control-manage-access-azure-cli.md)
- [REST-API](role-based-access-control-manage-access-rest.md)

Rollenbasierte Access Steuerelement (RBAC) in der Azure-Portal und Azure Ressourcenmanager API hilft Ihnen der Zugriff auf Ihr Abonnement und Ressourcen auf einer abgestimmte Ebene verwalten. Mit diesem Feature können Sie für Benutzer, Gruppen oder Dienst Hauptbenutzer Active Directory Access erteilen, indem Sie diese mit einem bestimmten Bereich einige Rollen zuweisen.

## <a name="list-all-role-assignments"></a>Liste aller rollenzuweisungen

Listet die rollenzuweisungen bei dem angegebenen Bereich und Subscopes an.

In der Liste rollenzuweisungen müssen Sie Zugriff auf `Microsoft.Authorization/roleAssignments/read` Vorgang in den Bereich. Die Standardrollen werden Zugriff auf diesen Vorgang erteilt. Weitere Informationen zu rollenzuweisungen und Verwalten von Access für Azure Ressourcen finden Sie unter [Azure_Role-Based Access Control](role-based-access-control-configure.md).

### <a name="request"></a>Anfordern

Verwenden Sie die **erste** Methode, mit dem folgenden URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments?api-version={api-version}&$filter={filter}

Innerhalb der URI stellen Sie die folgenden Platzhalter Anforderung anpassen:

1. Ersetzen Sie *{Bereich}* , mit dem Bereich für den Liste der rollenzuweisungen werden soll. So geben Sie den Bereich für die verschiedenen Ebenen Sie in den folgenden Beispielen:

  - Abonnements: /subscriptions/ {Abonnement-Id}  
  - Ressourcengruppe: /subscriptions/ {Abonnement-Id} / ResourceGroups/myresourcegroup1  
  - Ressource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. Ersetzen Sie *{api-Version}* mit 2015-07-01.

3. Ersetzen Sie *{Filter}* , mit der Bedingung, die Sie zum Filtern der Liste der Rolle Zuordnung anwenden möchten:

  - Liste rollenzuweisungen für nur den angegebenen Bereich, die rollenzuweisungen am Subscopes nicht eingeschlossen werden:`atScope()`    
  - Liste der rollenzuweisungen für einen bestimmten Benutzer, die Gruppe oder die Anwendung:`principalId%20eq%20'{objectId of user, group, or service principal}'`  
  - Liste der rollenzuweisungen für einen bestimmten Benutzer, auch von Gruppen geerbt |`assignedTo('{objectId of user}')`

### <a name="response"></a>Antwort

Statuscode: 200

```
{
  "value": [
    {
      "properties": {
        "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7",
        "principalId": "2f9d4375-cbf1-48e8-83c9-2a0be4cb33fb",
        "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
        "createdOn": "2015-10-08T07:28:24.3905077Z",
        "updatedOn": "2015-10-08T07:28:24.3905077Z",
        "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
        "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
      },
      "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleAssignments/baa6e199-ad19-4667-b768-623fde31aedd",
      "type": "Microsoft.Authorization/roleAssignments",
      "name": "baa6e199-ad19-4667-b768-623fde31aedd"
    }
  ],
  "nextLink": null
}

```

## <a name="get-information-about-a-role-assignment"></a>Erhalten von Informationen zu einer rollenzuweisung

Ruft Informationen zu einer einzelnen rollenzuweisung angegeben, die durch die Rolle Zuordnungs-ID an.

Wenn Sie eine rollenzuweisung erhalten, haben Sie Zugriff auf `Microsoft.Authorization/roleAssignments/read` Vorgang. Die Standardrollen werden Zugriff auf diesen Vorgang erteilt. Weitere Informationen zu rollenzuweisungen und Verwalten von Access für Azure Ressourcen finden Sie unter [Azure_Role-Based Access Control](role-based-access-control-configure.md).

### <a name="request"></a>Anfordern

Verwenden Sie die **erste** Methode, mit dem folgenden URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

Innerhalb der URI stellen Sie die folgenden Platzhalter Anforderung anpassen:

1. Ersetzen Sie *{Bereich}* , mit dem Bereich für den Liste der rollenzuweisungen werden soll. So geben Sie den Bereich für die verschiedenen Ebenen Sie in den folgenden Beispielen:

  - Abonnements: /subscriptions/ {Abonnement-Id}  
  - Ressourcengruppe: /subscriptions/ {Abonnement-Id} / ResourceGroups/myresourcegroup1  
  - Ressource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. Ersetzen Sie *{Rolle-Zuordnungs-Id}* mit dem Bezeichner GUID der Rolle Zuordnung an.

3. Ersetzen Sie *{api-Version}* mit 2015-07-01 ein.

### <a name="response"></a>Antwort

Statuscode: 200

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c",
    "principalId": "672f1afa-526a-4ef6-819c-975c7cd79022",
    "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
    "createdOn": "2015-10-05T08:36:26.4014813Z",
    "updatedOn": "2015-10-05T08:36:26.4014813Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleAssignments/196965ae-6088-4121-a92a-f1e33fdcc73e",
  "type": "Microsoft.Authorization/roleAssignments",
  "name": "196965ae-6088-4121-a92a-f1e33fdcc73e"
}

```

## <a name="create-a-role-assignment"></a>Erstellen einer Zuordnungs Rolle

Erstellen Sie eine rollenzuweisung bei den angegebenen Bereich für die angegebene Tilgungsanteile, die die angegebene Rolle erteilen.

Um eine rollenzuweisung zu erstellen, müssen Sie Zugriff auf `Microsoft.Authorization/roleAssignments/write` Vorgang. Der integrierten Rollen werden nur *Besitzer* und *Benutzer Access-Administrator* Zugriff auf diesen Vorgang erteilt. Weitere Informationen zu rollenzuweisungen und Verwalten von Access für Azure Ressourcen finden Sie unter [Azure_Role-Based Access Control](role-based-access-control-configure.md).

### <a name="request"></a>Anfordern

Verwenden Sie die Methode **setzen** , mit dem folgenden URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

Innerhalb der URI stellen Sie die folgenden Platzhalter Anforderung anpassen:

1. Ersetzen Sie *{Bereich}* , mit dem Bereich, an dem Sie die rollenzuweisungen erstellen möchten. Wenn Sie eine rollenzuweisung mit einem übergeordneten Bereich erstellen, erben alle untergeordneten Bereiche dieselbe Rolle-Zuordnung an. So geben Sie den Bereich für die verschiedenen Ebenen Sie in den folgenden Beispielen:

  - Abonnements: /subscriptions/ {Abonnement-Id}  
  - Ressourcengruppe: /subscriptions/ {Abonnement-Id} / ResourceGroups/myresourcegroup1   
  - Ressource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. Ersetzen Sie *{Rolle-Zuordnungs-Id}* mit einer neuen GUID, der der neue Zuordnung Rolle der GUID-Bezeichner wird.

3. Ersetzen Sie *{api-Version}* mit 2015-07-01.

Geben Sie für den Textbereich der Besprechungsanfrage die Werte in folgendem Format ein:

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
    "principalId": "5ac84765-1c8c-4994-94b2-629461bd191b"
  }
}

```

| Elementnamen     | Erforderlich | Typ   | Beschreibung |
|------------------|----------|--------|-------------|
| roleDefinitionId | Ja      | Zeichenfolge | Der Bezeichner der Rolle. Das Format des Bezeichners lautet:`{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id-guid}` |
| principalId      | Ja      | Zeichenfolge | ObjectId des der Azure AD-Hauptbenutzer (Benutzer, Gruppe oder Dienst Tilgungsanteile), denen die Rolle zugeordnet ist. |

### <a name="response"></a>Antwort

Statuscode: 201

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
    "principalId": "5ac84765-1c8c-4994-94b2-629461bd191b",
    "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND",
    "createdOn": "2015-12-16T00:27:19.6447515Z",
    "updatedOn": "2015-12-16T00:27:19.6447515Z",
    "createdBy": null,
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND/providers/Microsoft.Authorization/roleAssignments/2e9e86c8-0e91-4958-b21f-20f51f27bab2",
  "type": "Microsoft.Authorization/roleAssignments",
  "name": "2e9e86c8-0e91-4958-b21f-20f51f27bab2"
}

```

## <a name="delete-a-role-assignment"></a>Löschen einer Rollenzuweisung

Löschen einer Rolle Zuordnungs angegebenen Bereich.

Zum Löschen einer rollenzuweisung haben Sie Zugriff auf die `Microsoft.Authorization/roleAssignments/delete` Vorgang. Der integrierten Rollen werden nur *Besitzer* und *Benutzer Access-Administrator* Zugriff auf diesen Vorgang erteilt. Weitere Informationen zu rollenzuweisungen und Verwalten von Access für Azure Ressourcen finden Sie unter [Azure_Role-Based Access Control](role-based-access-control-configure.md).

### <a name="request"></a>Anfordern

Verwenden Sie die Methode **Löschen** , mit dem folgenden URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

Innerhalb der URI stellen Sie die folgenden Platzhalter Anforderung anpassen:

1. Ersetzen Sie *{Bereich}* , mit dem Bereich, an dem Sie die rollenzuweisungen erstellen möchten. So geben Sie den Bereich für die verschiedenen Ebenen Sie in den folgenden Beispielen:

  - Abonnements: /subscriptions/ {Abonnement-Id}  
  - Ressourcengruppe: /subscriptions/ {Abonnement-Id} / ResourceGroups/myresourcegroup1  
  - Ressource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. Ersetzen Sie mit der Rolle Zuordnung Id GUID *{Rolle-Zuordnungs-Id}* .

3. Ersetzen Sie *{api-Version}* mit 2015-07-01.

### <a name="response"></a>Antwort

Statuscode: 200

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
    "principalId": "5ac84765-1c8c-4994-94b2-629461bd191b",
    "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND",
    "createdOn": "2015-12-17T23:21:40.8921564Z",
    "updatedOn": "2015-12-17T23:21:40.8921564Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND/providers/Microsoft.Authorization/roleAssignments/5eec22ee-ea5c-431e-8f41-82c560706fd2",
  "type": "Microsoft.Authorization/roleAssignments",
  "name": "5eec22ee-ea5c-431e-8f41-82c560706fd2"
}

```

## <a name="list-all-roles"></a>Liste aller Rollen

Listen die Rollen aus, die für die Zuordnung am angegebenen Bereich verfügbar sind.

Liste Rollen müssen Sie Zugriff auf `Microsoft.Authorization/roleDefinitions/read` Vorgang in den Bereich. Die Standardrollen werden Zugriff auf diesen Vorgang erteilt. Weitere Informationen zu rollenzuweisungen und Verwalten von Access für Azure Ressourcen finden Sie unter [Azure_Role-Based Access Control](role-based-access-control-configure.md).

### <a name="request"></a>Anfordern

Verwenden Sie die **erste** Methode, mit dem folgenden URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions?api-version={api-version}&$filter={filter}

Innerhalb der URI stellen Sie die folgenden Platzhalter Anforderung anpassen:

1. Ersetzen Sie *{Bereich}* , mit dem Bereich für den Liste der Rollen werden soll. So geben Sie den Bereich für die verschiedenen Ebenen Sie in den folgenden Beispielen:

  - Abonnements: /subscriptions/ {Abonnement-Id}  
  - Ressourcengruppe: /subscriptions/ {Abonnement-Id} / ResourceGroups/myresourcegroup1  
  - Ressource /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. Ersetzen Sie *{api-Version}* mit 2015-07-01.

3. Ersetzen Sie *{Filter}* , mit der Bedingung, die Sie zum Filtern der Liste der Rollen anwenden möchten:

  - Liste der Rollen für die Zuordnung am angegebenen Bereich und beliebiger seiner untergeordneten Bereiche zur Verfügung:`atScopeAndBelow()`
  - Suchen nach einer Rolle genauen Anzeigenamen verwenden: `roleName%20eq%20'{role-display-name}'`. Mithilfe des Formulars URL-codierte des genauen Anzeigenamens der Rolle. Beispielsweise`$filter=roleName%20eq%20'Virtual%20Machine%20Contributor'` |

### <a name="response"></a>Antwort

Statuscode: 200

```
{
  "value": [
    {
      "properties": {
        "roleName": "Virtual Machine Contributor",
        "type": "BuiltInRole",
        "description": "Lets you manage virtual machines, but not access to them, and not the virtual network or storage account they\u2019re connected to.",
        "assignableScopes": [
          "/"
        ],
        "permissions": [
          {
            "actions": [
              "Microsoft.Authorization/*/read",
              "Microsoft.Compute/availabilitySets/*",
              "Microsoft.Compute/locations/*",
              "Microsoft.Compute/virtualMachines/*",
              "Microsoft.Compute/virtualMachineScaleSets/*",
              "Microsoft.Insights/alertRules/*",
              "Microsoft.Network/applicationGateways/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatRules/join/action",
              "Microsoft.Network/loadBalancers/read",
              "Microsoft.Network/locations/*",
              "Microsoft.Network/networkInterfaces/*",
              "Microsoft.Network/networkSecurityGroups/join/action",
              "Microsoft.Network/networkSecurityGroups/read",
              "Microsoft.Network/publicIPAddresses/join/action",
              "Microsoft.Network/publicIPAddresses/read",
              "Microsoft.Network/virtualNetworks/read",
              "Microsoft.Network/virtualNetworks/subnets/join/action",
              "Microsoft.Resources/deployments/*",
              "Microsoft.Resources/subscriptions/resourceGroups/read",
              "Microsoft.Storage/storageAccounts/listKeys/action",
              "Microsoft.Storage/storageAccounts/read",
              "Microsoft.Support/*"
            ],
            "notActions": []
          }
        ],
        "createdOn": "2015-06-02T00:18:27.3542698Z",
        "updatedOn": "2015-12-08T03:16:55.6170255Z",
        "createdBy": null,
        "updatedBy": null
      },
      "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
      "type": "Microsoft.Authorization/roleDefinitions",
      "name": "9980e02c-c2be-4d73-94e8-173b1dc7cf3c"
    }
  ],
  "nextLink": null
}

```

## <a name="get-information-about-a-role"></a>Erhalten von Informationen zu einer Rolle

Ruft Informationen zu einer bestimmten Rolle über die Rolle Definition Bezeichner angegeben wird. Informationen zu einer bestimmten Rolle den Anzeigenamen verwenden können, finden Sie unter [Liste aller Rollen](role-based-access-control-manage-access-rest.md#list-all-roles).

Um Informationen zu einer Rolle zu gelangen, haben Sie Zugriff auf `Microsoft.Authorization/roleDefinitions/read` Vorgang. Die Standardrollen werden Zugriff auf diesen Vorgang erteilt. Weitere Informationen zu rollenzuweisungen und Verwalten von Access für Azure Ressourcen finden Sie unter [Azure_Role-Based Access Control](role-based-access-control-configure.md).

### <a name="request"></a>Anfordern

Verwenden Sie die **erste** Methode, mit dem folgenden URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

Innerhalb der URI stellen Sie die folgenden Platzhalter Anforderung anpassen:

1. Ersetzen Sie *{Bereich}* , mit dem Bereich für den Liste der rollenzuweisungen werden soll. So geben Sie den Bereich für die verschiedenen Ebenen Sie in den folgenden Beispielen:

  - Abonnements: /subscriptions/ {Abonnement-Id}  
  - Ressourcengruppe: /subscriptions/ {Abonnement-Id} / ResourceGroups/myresourcegroup1  
  - Ressource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. Ersetzen Sie *{Rolle-Definition-Id}* mit dem Bezeichner GUID der Rollendefinition aus.

3. Ersetzen Sie *{api-Version}* mit 2015-07-01.

### <a name="response"></a>Antwort

Statuscode: 200

```
{
  "value": [
    {
      "properties": {
        "roleName": "Virtual Machine Contributor",
        "type": "BuiltInRole",
        "description": "Lets you manage virtual machines, but not access to them, and not the virtual network or storage account they\u2019re connected to.",
        "assignableScopes": [
          "/"
        ],
        "permissions": [
          {
            "actions": [
              "Microsoft.Authorization/*/read",
              "Microsoft.Compute/availabilitySets/*",
              "Microsoft.Compute/locations/*",
              "Microsoft.Compute/virtualMachines/*",
              "Microsoft.Compute/virtualMachineScaleSets/*",
              "Microsoft.Insights/alertRules/*",
              "Microsoft.Network/applicationGateways/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatRules/join/action",
              "Microsoft.Network/loadBalancers/read",
              "Microsoft.Network/locations/*",
              "Microsoft.Network/networkInterfaces/*",
              "Microsoft.Network/networkSecurityGroups/join/action",
              "Microsoft.Network/networkSecurityGroups/read",
              "Microsoft.Network/publicIPAddresses/join/action",
              "Microsoft.Network/publicIPAddresses/read",
              "Microsoft.Network/virtualNetworks/read",
              "Microsoft.Network/virtualNetworks/subnets/join/action",
              "Microsoft.Resources/deployments/*",
              "Microsoft.Resources/subscriptions/resourceGroups/read",
              "Microsoft.Storage/storageAccounts/listKeys/action",
              "Microsoft.Storage/storageAccounts/read",
              "Microsoft.Support/*"
            ],
            "notActions": []
          }
        ],
        "createdOn": "2015-06-02T00:18:27.3542698Z",
        "updatedOn": "2015-12-08T03:16:55.6170255Z",
        "createdBy": null,
        "updatedBy": null
      },
      "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
      "type": "Microsoft.Authorization/roleDefinitions",
      "name": "9980e02c-c2be-4d73-94e8-173b1dc7cf3c"
    }
  ],
  "nextLink": null
}

```

## <a name="create-a-custom-role"></a>Erstellen Sie eine benutzerdefinierte Rolle
Erstellen Sie eine benutzerdefinierte Rolle.

Zum Erstellen einer benutzerdefinierten Rolle haben Sie Zugriff auf `Microsoft.Authorization/roleDefinitions/write` Vorgang für alle die `AssignableScopes`. Der integrierten Rollen werden nur *Besitzer* und *Benutzer Access-Administrator* Zugriff auf diesen Vorgang erteilt. Weitere Informationen zu rollenzuweisungen und Verwalten von Access für Azure Ressourcen finden Sie unter [Azure_Role-Based Access Control](role-based-access-control-configure.md).

### <a name="request"></a>Anfordern

Verwenden Sie die Methode **setzen** , mit dem folgenden URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

Innerhalb der URI stellen Sie die folgenden Platzhalter Anforderung anpassen:

1. Ersetzen Sie *{Bereich}* mit der ersten *AssignableScope* der benutzerdefinierten Rolle aus. So geben Sie den Bereich für die verschiedenen Ebenen Sie in den folgenden Beispielen.

  - Abonnements: /subscriptions/ {Abonnement-Id}  
  - Ressourcengruppe: /subscriptions/ {Abonnement-Id} / ResourceGroups/myresourcegroup1  
  - Ressource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. Ersetzen Sie *{Rolle-Definition-Id}* mit einer neuen GUID, die der GUID-Bezeichner für die neue benutzerdefinierte Rolle wird.

3. Ersetzen Sie *{api-Version}* mit 2015-07-01.

Geben Sie für den Textbereich der Besprechungsanfrage die Werte in folgendem Format ein:

```
{
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "properties": {
    "roleName": "Virtual Machine Operator",
    "description": "Lets you monitor virtual machines and restart them.",
    "type": "CustomRole",
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ]
  }
}

```

| Elementnamen | Erforderlich | Typ | Beschreibung |
|--------------|----------|------|-------------|
| Namen         | Ja | Zeichenfolge   | GUID-Bezeichner für die benutzerdefinierte Rolle.    |
| properties.roleName               | Ja | Zeichenfolge   | Anzeigenamen Sie für die benutzerdefinierte Rolle. Maximale Größe 128 Zeichen.                        |
| Properties.Description            | Nein  | Zeichenfolge   | Beschreibung der benutzerdefinierten Rolle. Maximale Größe von 1024 Zeichen.                                               |
| Properties.Type                   | Ja | Zeichenfolge   | Legen Sie auf "CustomRole".                                         |
| Properties.Permissions.Actions    | Ja | String] | Ein Array von Aktion Zeichenfolgen, durch die benutzerdefinierte Rolle gewährten Operationen angibt.             |
| properties.permissions.notActions | Nein  | String] | Ein Array von Aktion Zeichenfolgen, die die Vorgänge aus den Vorgängen, die durch die benutzerdefinierte Rolle gewährten ausschließen angibt. |
| properties.assignableScopes       | Ja | String] | Ein Array von Bereichen, in denen die benutzerdefinierte Rolle verwendet werden kann.   |

### <a name="response"></a>Antwort

Statuscode: 201

```
{
  "properties": {
    "roleName": "Virtual Machine Operator",
    "type": "CustomRole",
    "description": "Lets you monitor virtual machines and restart them.",
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ],
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "createdOn": "2015-12-18T00:10:51.4662695Z",
    "updatedOn": "2015-12-18T00:10:51.4662695Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "type": "Microsoft.Authorization/roleDefinitions",
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7"
}

```

## <a name="update-a-custom-role"></a>Aktualisieren einer benutzerdefinierten Rolle

Ändern Sie eine benutzerdefinierte Rolle.

Wenn Sie eine benutzerdefinierte Rolle ändern möchten, müssen Sie Zugriff auf `Microsoft.Authorization/roleDefinitions/write` Vorgang für alle die `AssignableScopes`. Der integrierten Rollen werden nur *Besitzer* und *Benutzer Access-Administrator* Zugriff auf diesen Vorgang erteilt. Weitere Informationen zu rollenzuweisungen und Verwalten von Access für Azure Ressourcen finden Sie unter [Azure_Role-Based Access Control](role-based-access-control-configure.md).

### <a name="request"></a>Anfordern

Verwenden Sie die Methode **setzen** , mit dem folgenden URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

Innerhalb der URI stellen Sie die folgenden Platzhalter Anforderung anpassen:

1. Ersetzen Sie *{Bereich}* mit der ersten *AssignableScope* der benutzerdefinierten Rolle aus. So geben Sie den Bereich für die verschiedenen Ebenen Sie in den folgenden Beispielen:

  - Abonnements: /subscriptions/ {Abonnement-Id}  
  - Ressourcengruppe: /subscriptions/ {Abonnement-Id} / ResourceGroups/myresourcegroup1  
  - Ressource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. Ersetzen Sie *{Rolle-Definition-Id}* mit der GUID-Bezeichner für die benutzerdefinierte Rolle aus.

3. Ersetzen Sie *{api-Version}* mit 2015-07-01.

Geben Sie für den Textbereich der Besprechungsanfrage die Werte in folgendem Format ein:

```
{
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "properties": {
    "roleName": "Virtual Machine Operator",
    "description": "Lets you monitor virtual machines and restart them.",
    "type": "CustomRole",
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ]
  }
}

```

| Elementnamen | Erforderlich | Typ | Beschreibung |
|--------------|----------|------|-------------|
| Namen         | Ja      | Zeichenfolge | GUID-Bezeichner für die benutzerdefinierte Rolle. |
| properties.roleName | Ja | Zeichenfolge | Anzeigename der aktualisierten benutzerdefinierten Rolle. |
| Properties.Description | Nein | Zeichenfolge | Beschreibung der aktualisierten benutzerdefinierten Rolle. |
| Properties.Type | Ja | Zeichenfolge | Legen Sie auf "CustomRole". |
| Properties.Permissions.Actions | Ja | String] | Ein Array von Aktion Zeichenfolgen, zu dem die aktualisierte benutzerdefinierte Rolle Zugriff gewährt, Operationen angibt. |
| properties.permissions.notActions | Nein | String] | Ein Array von Aktion Zeichenfolgen, die Vorgänge ausgeschlossen werden, die die aktualisierte benutzerdefinierte Rolle gewährt Operationen angibt. |
| properties.assignableScopes | Ja | String] | Ein Array von Bereichen, in denen die aktualisierte benutzerdefinierte Rolle verwendet werden kann. |

### <a name="response"></a>Antwort

Statuscode: 201

```
{
  "properties": {
    "roleName": "Virtual Machine Operator",
    "type": "CustomRole",
    "description": "Lets you monitor virtual machines and restart them.",
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ],
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "createdOn": "2015-12-18T00:10:51.4662695Z",
    "updatedOn": "2015-12-18T00:10:51.4662695Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "type": "Microsoft.Authorization/roleDefinitions",
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7"
}

```

## <a name="delete-a-custom-role"></a>Löschen einer benutzerdefinierten Rolle

Löschen einer benutzerdefinierten Rolle.

Um eine benutzerdefinierte Rolle löschen zu können, müssen Sie Zugriff auf `Microsoft.Authorization/roleDefinitions/delete` Vorgang für alle die `AssignableScopes`. Der integrierten Rollen werden nur *Besitzer* und *Benutzer Access-Administrator* Zugriff auf diesen Vorgang erteilt. Weitere Informationen zu rollenzuweisungen und Verwalten von Access für Azure Ressourcen finden Sie unter [Azure_Role-Based Access Control](role-based-access-control-configure.md).

### <a name="request"></a>Anfordern

Verwenden Sie die Methode **Löschen** , mit dem folgenden URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

Innerhalb der URI stellen Sie die folgenden Platzhalter Anforderung anpassen:

1. Ersetzen Sie *{Bereich}* , mit dem Bereich, an dem Sie die Rollendefinition löschen möchten. So geben Sie den Bereich für die verschiedenen Ebenen Sie in den folgenden Beispielen:

  - Abonnements: /subscriptions/ {Abonnement-Id}  
  - Ressourcengruppe: /subscriptions/ {Abonnement-Id} / ResourceGroups/myresourcegroup1  
  - Ressource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. Ersetzen Sie *{Rolle-Definition-Id}* die GUID Rolle Definition ID der benutzerdefinierten Rolle aus.

3. Ersetzen Sie *{api-Version}* mit 2015-07-01.

### <a name="response"></a>Antwort

Statuscode: 200

```
{
  "properties": {
    "roleName": "Virtual Machine Operator",
    "type": "CustomRole",
    "description": "Lets you monitor virtual machines and restart them.",
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ],
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "createdOn": "2015-12-16T00:07:02.9236555Z",
    "updatedOn": "2015-12-16T00:07:02.9236555Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/0bd62a70-e1b8-4e0b-a7c2-75cab365c95b",
  "type": "Microsoft.Authorization/roleDefinitions",
  "name": "0bd62a70-e1b8-4e0b-a7c2-75cab365c95b"
}

```


[AZURE.INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]

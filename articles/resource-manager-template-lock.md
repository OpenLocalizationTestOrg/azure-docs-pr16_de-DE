<properties
   pageTitle="Ressourcenmanager Vorlage für Ressourcen Sperren | Microsoft Azure"
   description="Zeigt das Schema Ressourcenmanager für die Bereitstellung von Ressourcen Sperren über eine Vorlage."
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

# <a name="resource-locks-template-schema"></a>Ressourcenschema Sperren-Vorlage

Erstellt eine Sperre einer Ressource und deren untergeordnete Ressourcen.

## <a name="schema-format"></a>Schemaformat

Um eine Sperre zu erstellen, fügen Sie das folgende Schema zum Ressourcenabschnitt der Vorlage ein.
    
    {
        "type": enum,
        "apiVersion": "2015-01-01",
        "name": string,
        "dependsOn": [ array values ],
        "properties":
        {
            "level": enum,
            "notes": string
        }
    }



## <a name="values"></a>Werte

In den folgenden Tabellen werden die Werte, die Sie in das Schema festlegen müssen.

| Namen | Erforderlich | Beschreibung |
| ---- | -------- | ----------- |
| Typ | Ja | Die Ressourcenart zu erstellen.<br /><br />Für Ressourcen:<br />**{Namespace} / {type} / Anbieter/Sperren**<br /><br/>Für die Ressourcengruppen:<br />**Microsoft.Authorization/locks** |
| apiVersion | Ja | Die API-Version für die Ressource zu erstellen.<br /><br />Verwenden:<br />**2015-01-01**<br /><br /> |
| Namen | Ja | Ein Wert, der sowohl die Ressource sperren und einen Namen für die Sperre angibt. Kann bis zu 64 Zeichen, und kann nicht enthalten <>; %, &,?, oder einem beliebigen Steuerelement Zeichen.<br /><br />Für Ressourcen:<br />**{resource}/Microsoft.Authorization/{lockname}**<br /><br />Für die Ressourcengruppen:<br />**{Lockname}** |
| dependsOn | Nein | Eine durch Trennzeichen getrennte Liste einer Ressource Namen oder Ressource eindeutigen Bezeichner.<br /><br />Die Auflistung von Ressourcen, die, denen diese Sperre abhängt. Wenn die Ressource, die Sie Sperren sind in derselben Vorlage bereitgestellt wird, gehören Sie die Ressourcenname in diesem Element, um sicherzustellen, dass die Ressource zuerst bereitgestellt wird. | 
| Eigenschaften | Ja | Ein Objekt, das den Typ des Sperren und Notizen zu der Sperre bezeichnet.<br /><br />Finden Sie unter [Properties-Objekt](#properties-object). |  

### <a name="properties-object"></a>Properties-Objekt

| Namen | Erforderlich | Beschreibung |
| ---- | -------- | ----------- |
| Ebene   | Ja | Die Art des auf den Bereich anzuwendende sperren.<br /><br />**CannotDelete** - Benutzer können Ressourcen ändern, aber nicht gelöscht werden.<br />**Schreibgeschützt** - Benutzer können aus einer Ressource lesen, aber nicht löschen oder auf diese Aktionen ausführen. |
| Notizen   | Nein | Beschreibung der Sperre. Bis zu 512 Zeichen kann umfassen. |


## <a name="how-to-use-the-lock-resource"></a>So verwenden Sie die Sperrressource

Sie hinzufügen diese Ressource zu einer Vorlage, um zu verhindern, dass bestimmte Aktionen für eine Ressource. Die Sperre gilt für alle Benutzer und Gruppen.

Zum Erstellen oder Löschen von Management sperren möchten, müssen Sie Zugriff auf **Microsoft.Authorization/** * oder * *Microsoft.Authorization/locks/* ** Aktionen. Der integrierten Rollen, nur **Besitzer** und **Benutzer Access Administrator ** diese Aktionen gewährt werden. Informationen zu Access rollenbasierte Steuerelement finden Sie unter [Rollenbasierte Azure Access Control](./active-directory/role-based-access-control-configure.md).

Die Sperre wird auf die angegebene Ressource und alle untergeordneten Ressourcen angewendet.

Sie können eine Sperre für den PowerShell-Befehl **Entfernen-AzureRmResourceLock** oder der [Vorgang zum Löschen](https://msdn.microsoft.com/library/azure/mt204562.aspx) der REST-API entfernen.

## <a name="examples"></a>Beispiele

Im folgende Beispiel gilt für eine Sperre kann nicht löschen einer Web app.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "hostingPlanName": {
                "type": "string"
            }
        },
        "variables": {
            "siteName": "[concat('site',uniqueString(resourceGroup().id))]"
        },
        "resources": [
            {
                "apiVersion": "2015-08-01",
                "name": "[variables('siteName')]",
                "type": "Microsoft.Web/sites",
                "location": "[resourceGroup().location]",
                "properties": {
                    "serverFarmId": "[parameters('hostingPlanName')]"
                },
            },
            {
                "type": "Microsoft.Web/sites/providers/locks",
                "apiVersion": "2015-01-01",
                "name": "[concat(variables('siteName'),'/Microsoft.Authorization/MySiteLock')]",
                "dependsOn": [ "[variables('siteName')]" ],
                "properties":
                {
                    "level": "CannotDelete",
                    "notes": "my notes"
                }
             }
        ],
        "outputs": {}
    }

Im nächste Beispiel gilt für eine Sperre kann nicht löschen der Ressourcengruppe.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {},
        "variables": {},
        "resources": [
            {
                "type": "Microsoft.Authorization/locks",
                "apiVersion": "2015-01-01",
                "name": "MyGroupLock",
                "properties":
                {
                    "level": "CannotDelete",
                    "notes": "my notes"
                }
            }
        ],
        "outputs": {}
    }

## <a name="next-steps"></a>Nächste Schritte

- Informationen über die Vorlagenstruktur finden Sie unter [Azure Ressourcenmanager Authoring-Vorlagen](resource-group-authoring-templates.md).
- Weitere Informationen zu Sperren finden Sie unter [Sperrenressourcen Azure Ressourcenmanager](resource-group-lock-resources.md).

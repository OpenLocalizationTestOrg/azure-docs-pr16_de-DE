<properties
   pageTitle="Ressourcenmanager Vorlage zum Verknüpfen von Ressourcen | Microsoft Azure"
   description="Zeigt das Schema Ressourcenmanager für die Bereitstellung von Verknüpfungen zwischen zugehörige Ressourcen über eine Vorlage."
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
   ms.date="04/05/2016"
   ms.author="tomfitz"/>

# <a name="resource-links-template-schema"></a>Ressourcenschema Links-Vorlage

Erstellt eine Verknüpfung zwischen zwei Ressourcen. Der Link gilt für eine Ressource, die als Quelle Ressource bezeichnet. Die zweite Ressource in den Link wird als Zielressource bezeichnet.

## <a name="schema-format"></a>Schemaformat

Um einen Link zu erstellen, fügen Sie das folgende Schema zum Ressourcenabschnitt der Vorlage ein.
    
    {
        "type": enum,
        "apiVersion": "2015-01-01",
        "name": string,
        "dependsOn": [ array values ],
        "properties":
        {
            "targetId": string,
            "notes": string
        }
    }



## <a name="values"></a>Werte

In den folgenden Tabellen werden die Werte, die Sie in das Schema festlegen müssen.

| Namen | Wert |
| ---- | ---- |
| Typ | Aufzählung<br />Erforderlich<br />**{Namespace} / {type} / Anbieter/Links**<br /><br />Die Ressourcenart zu erstellen. {Namespace} und {Type} Werte beziehen sich auf den Anbieter Namespace und Ressourcen den Typ der Quelle Ressource. |
| apiVersion | Aufzählung<br />Erforderlich<br />**2015-01-01**<br /><br />Die API-Version für die Ressource zu erstellen. |  
| Namen | Zeichenfolge<br />Erforderlich<br />**{resouce}/Microsoft.Resources/{linkname}**<br /> bis zu 64 Zeichen und keine enthalten <>; %, &,?, oder einem beliebigen Steuerelement Zeichen.<br /><br />Ein Wert, der angibt, den Namen der Ressource Quelle, und einen Namen für den Link. |
| dependsOn | Matrix<br />Optional<br />Eine durch Trennzeichen getrennte Liste einer Ressource Namen oder Ressource eindeutigen Bezeichner.<br /><br />Die Auflistung von Ressourcen, die, denen diesen Link abhängt. Wenn die Ressourcen, die Sie verknüpfen, in der gleichen Vorlage bereitgestellt werden, nehmen Sie diese Ressourcennamen in dieses Element aus, um sicherzustellen, dass sie zunächst bereitgestellt werden. | 
| Eigenschaften | Objekt<br />Erforderlich<br />[Properties-Objekt](#properties)<br /><br />Ein Objekt, die Ressource, mit der eine Verknüpfung identifiziert, und Hinweise zur Verknüpfung. |  

<a id="properties" />
### <a name="properties-object"></a>Properties-Objekt

| Namen | Wert |
| ------- | ---- |
| targetId | Zeichenfolge<br />Erforderlich<br />**{Ressourcen-Id}**<br /><br />Der Bezeichner der Zielressource, mit der eine Verknüpfung. |
| Notizen | Zeichenfolge<br />Optional<br />bis zu 512 Zeichen<br /><br />Beschreibung der Sperre. |


## <a name="how-to-use-the-link-resource"></a>So verwenden Sie die Link Ressource

Sie anwenden eine Verknüpfung zwischen zwei Ressourcen, wenn die Ressourcen Abhängigkeit aufweisen, die nach der Bereitstellung weiterhin Beispielsweise kann eine app zu einer Datenbank in einer anderen Ressourcengruppe verbinden. Sie können diese Abhängigkeit durch Erstellen eines Links aus der app in der Datenbank definieren. Links ermöglichen Ihnen, die Beziehung zwischen zwei Ressourcen Dokument. Später können Sie oder andere Personen in Ihrer Organisation Abfragen eine Ressource für Links zu ermitteln, wie die Ressource mit anderen Ressourcen funktioniert.

Alle verknüpfte Ressourcen müssen den gleichen Abonnement gehören. Jede Ressource kann auf 50 Weitere Ressourcen verknüpft werden. Wenn eine oder mehrere der verknüpften Ressourcen gelöscht oder verschoben werden, muss die verbleibende Link der Link Besitzer bereinigen.

Zum Arbeiten mit Links durch REST finden Sie unter [Verknüpfte Ressourcen](https://msdn.microsoft.com/library/azure/mt238499.aspx).

Verwenden Sie den folgenden Befehl aus Azure PowerShell, um alle Hyperlinks in Ihrem Abonnement anzuzeigen. Sie können andere Parametern zum Einschränken der Ergebnisse bereitstellen.

    Get-AzureRmResource -ResourceType Microsoft.Resources/links -isCollection -ResourceGroupName <YourResourceGroupName>

## <a name="examples"></a>Beispiele

Im folgende Beispiel wird eine Web app eine schreibgeschützte Sperre gilt.

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
                "type": "Microsoft.Web/serverfarms",
                "name": "[parameters('hostingPlanName')]",
                "location": "[resourceGroup().location]",
                "sku": {
                    "tier": "Free",
                    "name": "f1",
                    "capacity": 0
                },
                "properties": {
                    "numberOfWorkers": 1
                }
            },
            {
                "apiVersion": "2015-08-01",
                "name": "[variables('siteName')]",
                "type": "Microsoft.Web/sites",
                "location": "[resourceGroup().location]",
                "dependsOn": [ "[parameters('hostingPlanName')]" ],
                "properties": {
                    "serverFarmId": "[parameters('hostingPlanName')]"
                }
            },
            {
                "type": "Microsoft.Web/sites/providers/links",
                "apiVersion": "2015-01-01",
                "name": "[concat(variables('siteName'),'/Microsoft.Resources/SiteToStorage')]",
                "dependsOn": [ "[variables('siteName')]" ],
                "properties": {
                    "targetId": "[resourceId('Microsoft.Storage/storageAccounts','storagecontoso')]",
                    "notes": "This web site uses the storage account to store user information."
                }
            }
        ],
        "outputs": {}
    }

## <a name="quickstart-templates"></a>Schnellstart-Vorlagen

Die folgenden Schnellstart Vorlagen Bereitstellen von Ressourcen mit einem Link.

- [Mit Logik app in Warteschlange benachrichtigen](https://azure.microsoft.com/documentation/templates/201-alert-to-queue-with-logic-app)
- [Benachrichtigung zum Pufferzeit mit Logik-app](https://azure.microsoft.com/documentation/templates/201-alert-to-slack-with-logic-app)
- [Bereitstellen einer API-app mit einem vorhandenen gateway](https://azure.microsoft.com/documentation/templates/201-api-app-gateway-existing)
- [Bereitstellen einer API-app mit einem neuen gateway](https://azure.microsoft.com/documentation/templates/201-api-app-gateway-new)
- [Erstellen Sie eine app Logik App- plus -API, die mithilfe einer Vorlage](https://azure.microsoft.com/documentation/templates/201-logic-app-api-app-create)
- [Logik-app, die eine Textnachricht sendet, wenn eine Warnung wird ausgelöst](https://azure.microsoft.com/documentation/templates/201-alert-to-text-message-with-logic-app)


## <a name="next-steps"></a>Nächste Schritte

- Informationen über die Vorlagenstruktur finden Sie unter [Azure Ressourcenmanager Authoring-Vorlagen](resource-group-authoring-templates.md).

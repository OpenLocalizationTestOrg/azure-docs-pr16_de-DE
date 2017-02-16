<properties
    pageTitle="Verwenden von Kategorien zum Organisieren Ihrer Azure Ressourcen | Microsoft Azure"
    description="Anwenden von Kategorien zum Organisieren von Ressourcen für Abrechnung und Verwalten von veranschaulicht."
    services="azure-resource-manager"
    documentationCenter=""
    authors="tfitzmac"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="azure-resource-manager"
    ms.workload="multiple"
    ms.tgt_pltfrm="AzurePortal"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/08/2016"
    ms.author="tomfitz"/>


# <a name="using-tags-to-organize-your-azure-resources"></a>Verwenden von Kategorien zum Organisieren Ihrer Azure Ressourcen

Ressourcenmanager können Sie Ressourcen logisch organisieren, indem Sie Kategorien anwenden. Die Tags bestehen aus Schlüssel/Wert-Paare, die Ressourcen mit den Eigenschaften zu identifizieren, die Sie definieren. Um die gleiche Kategorie gehören als Ressourcen zu kennzeichnen, wenden Sie das gleiche Tag auf diese Ressourcen aus.

Wenn Sie Ressourcen mit einer bestimmten Kategorie anzeigen, finden Sie Ressourcen aus allen Ressourcengruppen. Sie sind nicht auf nur Ressourcen in derselben Ressourcengruppe, wodurch Sie zum Organisieren von Ressourcen in eine Möglichkeit, die von der Bereitstellung Beziehungen unabhängig ist eingeschränkt. Kategorien können hilfreich sein, wenn Sie Ressourcen für Abrechnung oder Management organisieren müssen.

Jede Kategorie, die Sie einer Ressource oder Ressourcengruppe hinzufügen, wird automatisch zur Taxonomie Abonnement organisationsweite hinzugefügt. Sie können auch die Taxonomie vorab, für Ihr Abonnement mit Tagnamen und Werte, die Sie verwenden möchten, wie Ressourcen in der Zukunft markiert sind.

Jeder Ressource oder Ressourcengruppe kann maximal 15 Kategorien haben. Der Tagname auf 512 Zeichen beschränkt ist, und der Kategorie Wert ist auf 256 Zeichen beschränkt.

> [AZURE.NOTE] Sie können nur Kategorien auf Ressourcen anwenden, die Ressourcenmanager Operationen unterstützen. Wenn Sie einen virtuellen Computern, virtuellen Netzwerk- oder über das Bereitstellungsmodell klassischen erstellt (wie über das klassische Portal), Sie können eine Kategorie auf diese Ressource anwenden. Erneut bereitstellen Sie, kategorisieren unterstützt, diese Ressourcen durch Ressourcenmanager aus. Alle anderen Ressourcen unterstützen kategorisieren.

## <a name="templates"></a>Vorlagen

Wenn Sie eine Ressource während der Bereitstellung markieren möchten, einfach fügen Sie das Element **Kategorien hinzu** , die der Ressource, die Sie bereitstellen, und geben Sie den Namen der Kategorie und den Wert. Die Tagname und Value müssen nicht vorab in Ihrem Abonnement vorhanden sind. Sie können bis zu 15 Kategorien für jede Ressource bereitstellen.

Im folgenden Beispiel wird mit einer Kategorie ein Speicherkonto an.

    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2015-06-15",
            "name": "[concat('storage', uniqueString(resourceGroup().id))]",
            "location": "[resourceGroup().location]",
            "tags": {
                "dept": "Finance"
            },
            "properties": 
            {
                "accountType": "Standard_LRS"
            }
        }
    ]

Ressourcenmanager unterstützt Verarbeitung eines Objekts für die Namen der Kategorie und Werte derzeit nicht. Übergeben Sie stattdessen ein Objekt für die Kategorie Werte, aber Sie müssen immer noch die Tagnamen angeben, wie im folgenden Beispiel dargestellt.

    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "tagvalues": {
          "type": "object",
          "defaultValue": {
            "dept": "Finance",
            "project": "Test"
          }
        }
      },
      "resources": [
      {
        "apiVersion": "2015-06-15",
        "type": "Microsoft.Storage/storageAccounts",
        "name": "examplestorage",
        "tags": {
          "dept": "[parameters('tagvalues').dept]",
          "project": "[parameters('tagvalues').project]"
        },
        "location": "[resourceGroup().location]",
        "properties": {
          "accountType": "Standard_LRS"
        }
      }]
    }


## <a name="portal"></a>Portal

[AZURE.INCLUDE [resource-manager-tag-resource](../includes/resource-manager-tag-resources.md)]

## <a name="powershell"></a>PowerShell

[AZURE.INCLUDE [resource-manager-tag-resources](../includes/resource-manager-tag-resources-powershell.md)]

## <a name="azure-cli"></a>Azure CLI

[AZURE.INCLUDE [resource-manager-tag-resources-cli](../includes/resource-manager-tag-resources-cli.md)]

## <a name="rest-api"></a>REST-API

Dem Portal und dem PowerShell verwenden beide die [REST-API Ressourcenmanager](https://msdn.microsoft.com/library/azure/dn848368.aspx) Hintergrundinformationen. Wenn Sie in einer anderen Umgebung kategorisieren integrieren müssen, können Sie Tags mit GET auf die Ressourcen-Id abrufen und Festlegen von Tags mit einem Anruf PATCH aktualisieren.


## <a name="tags-and-billing"></a>Kategorien und Abrechnung

Kategorien können Sie für die unterstützten Dienste Ihre Abrechnung Daten gruppieren. Beispielsweise aktivieren [virtuellen Computern integriert Azure Ressourcenmanager](./virtual-machines/virtual-machines-windows-compare-deployment-models.md) Sie definieren und Anwenden von Kategorien zum Organisieren von der Abrechnung Verwendung für virtuelle Computer. Wenn Sie mehrere virtuelle Computer für verschiedene Organisationen ausgeführt werden, können Sie nach Kostenstelle die Kategorien Gruppe: Einsatz hinzu.  
Kategorien können Sie auch Kosten von Runtime-Umgebung kategorisieren; wie die Abrechnung Verwendung für virtuelle Computer in Herstellung Umgebung ausgeführt.

Sie können Informationen zu Tags über die [RateCard-APIs und Azure Ressource: Einsatz](billing-usage-rate-card-overview.md) oder die Verwendung durch Trennzeichen getrennte Werte (CSV) Datei abrufen. Sie können die Verwendung-Datei aus dem [Portal Azure Konten](https://account.windowsazure.com/) oder [EA-Portal](https://ea.azure.com)herunterladen. Weitere Informationen zu den programmgesteuerten Zugriff auf Abrechnungsinformationen finden Sie unter [Einblicke in Ihrer Microsoft Azure Ressourcenverbrauch zu erhalten](billing-usage-rate-card-overview.md). REST-API Vorgänge finden Sie unter [Azure Abrechnung REST-API-Referenz](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c).

Wenn Sie die Verwendung CSV für Services, die Kategorien mit Abrechnung unterstützen herunterladen, werden die Kategorien in der Spalte **Kategorien** angezeigt. Weitere Informationen hierzu finden Sie unter [Grundlegendes zu Ihrer Rechnung für Microsoft Azure](billing/billing-understand-your-bill.md).

![Finden Sie unter Tags in Abrechnung](./media/resource-group-using-tags/billing_csv.png)

## <a name="next-steps"></a>Nächste Schritte

- Sie können über Ihr Abonnement mit angepassten Richtlinien Einschränkungen und Konventionen anwenden. Die Richtlinie, die Sie definieren zusätzliche erforderlich sein, dass alle Ressourcen den Wert für ein bestimmtes Tag. Weitere Informationen finden Sie unter [Verwenden von Richtlinien zum Verwalten von Ressourcen und Steuern des Zugriffs](resource-manager-policy.md).
- Eine Einführung in Azure PowerShell verwenden, wenn Sie Ressourcen bereitstellen finden Sie unter [Verwenden von Azure PowerShell Azure Ressourcenmanager](./powershell-azure-resource-manager.md).
- Eine Einführung in Azure CLI verwenden, wenn Sie Ressourcen bereitstellen finden Sie unter [Verwenden der Azure CLI für Mac, Linux, und Windows Azure Ressource Verwaltung](./xplat-cli-azure-resource-manager.md).
- Eine Einführung in das Portal verwenden finden Sie unter [Verwenden des Azure-Portals zum Verwalten Ihrer Azure-Ressourcen](./azure-portal/resource-group-portal.md)  

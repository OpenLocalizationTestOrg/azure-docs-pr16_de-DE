<properties 
    pageTitle="Verknüpfen von Ressourcen in Azure Ressourcenmanager | Microsoft Azure" 
    description="Erstellen einer Verknüpfung zwischen zugehörige Ressourcen in anderen Ressourcengruppen in Azure Ressourcenmanager." 
    services="azure-resource-manager" 
    documentationCenter="" 
    authors="tfitzmac" 
    manager="timlt" 
    editor="tysonn"/>

<tags 
    ms.service="azure-resource-manager" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/01/2016" 
    ms.author="tomfitz"/>

# <a name="linking-resources-in-azure-resource-manager"></a>Verknüpfen von Ressourcen in Azure Ressourcenmanager

Während der Bereitstellung können Sie eine Ressource als abhängig von einer anderen Ressource festgelegt, aber dieses Lebenszyklus endet, bei der Bereitstellung. Nach der Bereitstellung gibt es keine identifizierten Beziehung zwischen abhängigen Ressourcen. Ressourcenmanager bietet ein Feature namens Ressource verknüpfen möchten um beständige Beziehungen zwischen Ressourcen zu erstellen.

Mit der Ressource verknüpfen möchten, können Sie Beziehungen dokumentieren, die Ressourcengruppen umfassen. Sie beträgt beispielsweise häufig, um eine Datenbank mit ein eigenen Lebenszyklus befinden sich in eine Ressourcengruppe und eine app mit einem anderen Lebenszyklus befinden sich in einer anderen Ressourcengruppe. Die app verbindet mit der Datenbank, damit Sie eine Verknüpfung zwischen der app und die Datenbank kennzeichnen möchten. 

Alle verknüpfte Ressourcen müssen den gleichen Abonnement gehören. Jede Ressource kann auf 50 Weitere Ressourcen verknüpft werden. Die einzige Möglichkeit zum Abfragen von Ressourcenübersicht erfolgt über die REST-API. Wenn eine oder mehrere der verknüpften Ressourcen gelöscht oder verschoben werden, muss die verbleibende Link der Link Besitzer bereinigen. Sie sind **nicht** gewarnt Situationen, löschen eine Ressource, die mit anderen Ressourcen verknüpft ist.

## <a name="linking-in-templates"></a>Verknüpfen von Vorlagen

Um einen Link in einer Vorlage zu definieren, fügen Sie einen Ressourcentyp, der den Ressource Anbieternamespace kombiniert und Typ der Quelle Ressource mit **/providers/links**. Der Name muss den Namen der Ressource Quelle enthalten. Die Ressourcen-Id der Zielressource Geben Sie ein. Im folgenden Beispiel wird eine Verbindung zwischen einer Website und ein Speicherkonto an.

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


Eine vollständige Beschreibung des Formats Vorlage finden Sie unter [Ressourcen-Links - Vorlage Schema](resource-manager-template-links.md).

## <a name="linking-with-rest-api"></a>Verknüpfen mit REST-API

Wenn Sie um eine Verknüpfung zwischen bereitgestellten Ressourcen zu definieren, führen Sie Folgendes aus:

    PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/{provider-namespace}/{resource-type}/{resource-name}/providers/Microsoft.Resources/links/{link-name}?api-version={api-version}

Ersetzen Sie {Abonnement-Id} mit Ihrem Abonnement-Id an. Ersetzen Sie {Ressourcengruppe}, {Namespace Anbieter, {Ressourcenart}, und {Ressourcenname} mit den Werten, die die erste Ressource in den Link zu identifizieren. {Link-Name} mit dem Namen des zu erstellenden Links zu ersetzen. Verwenden von 2015-01-01 für die api-Version.

Gehören Sie in der Besprechungsanfrage ein Objekt, die zweite Ressource in den Link definiert:

    {
        "name": "{new-link-name}",
        "properties": {
            "targetId": "subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/{provider-namespace}/{resource-type}/{resource-name}/",
            "notes": "{link-description}",
        }
    }

Das Eigenschaftselement enthält den Bezeichner für die zweite Ressource an.

Sie können Links in Ihr Abonnement mit Abfragen:

    https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Resources/links?api-version={api-version}

Wenn Sie weitere Beispiele, z. B. zum Abrufen von Informationen zu Links, finden Sie unter [Verknüpfte Ressourcen](https://msdn.microsoft.com/library/azure/mt238499.aspx).

## <a name="next-steps"></a>Nächste Schritte

- Sie können auch Ihre Ressourcen mit Kategorien organisieren. Weitere Informationen zu communitytags Ressourcen finden Sie unter [Verwenden von Kategorien, um Ihre Ressourcen zu organisieren](resource-group-using-tags.md).
- Eine Beschreibung zum Erstellen von Vorlagen und definieren die Ressourcen bereitgestellt werden finden Sie unter [Authoring-Vorlagen](resource-group-authoring-templates.md).

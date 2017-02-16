<properties
   pageTitle="Bereitstellen von Ressourcen für REST-API und Vorlage | Microsoft Azure"
   description="Formular mit Azure Ressourcenmanager und Ressourcenmanager REST-API eine Ressourcen Azure bereitstellen. Die Ressourcen werden in einer Vorlage Ressourcenmanager definiert."
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
   ms.date="07/11/2016"
   ms.author="tomfitz"/>

# <a name="deploy-resources-with-resource-manager-templates-and-resource-manager-rest-api"></a>Bereitstellen von Ressourcen mit Ressourcenmanager Vorlagen und Ressourcenmanager REST-API

> [AZURE.SELECTOR]
- [PowerShell](resource-group-template-deploy.md)
- [Azure CLI](resource-group-template-deploy-cli.md)
- [Portal](resource-group-template-deploy-portal.md)
- [REST-API](resource-group-template-deploy-rest.md)

In diesem Artikel wird erläutert, wie die Ressourcenmanager REST-API mit Ressourcenmanager Vorlagen zu verwenden, um Ihre Ressourcen Azure bereitstellen.  

> [AZURE.TIP] Hilfe für das Debuggen ein Fehler während der Bereitstellung finden Sie unter:
>
> - [Ansicht Bereitstellungsvorgänge mit REST-API](resource-manager-troubleshoot-deployments-rest.md) Informationen dazu, wie Sie Informationen, die Ihnen dabei hilft Behandeln von Problemen mit der Fehler
> - [Behandeln von häufigen Fehlern beim Bereitstellen von Ressourcen in Azure Azure Ressourcenmanager](resource-manager-common-deployment-errors.md) Informationen zum Beheben häufig auftretender Bereitstellungsfehler

Ihre Vorlage kann entweder eine lokale Datei oder eine externe Datei, die durch einen URI zur Verfügung. Wenn Sie Ihre Vorlage in einem Speicherkonto befindet, können Sie einschränken des Zugriffs auf die Vorlage und Bereitstellen einer freigegebenen Access Signatur (SAS) Token während der Bereitstellung.

[AZURE.INCLUDE [resource-manager-deployments](../includes/resource-manager-deployments.md)]

## <a name="deploy-with-the-rest-api"></a>Bereitstellen Sie für die REST-API
1. Legen Sie [Allgemeine Parameter und Überschriften](https://msdn.microsoft.com/library/azure/8d088ecc-26eb-42e9-8acc-fe929ed33563#bk_common), einschließlich Authentifizierungstoken.
2. Wenn Sie nicht über eine vorhandene Ressourcengruppe verfügen, erstellen Sie eine Ressourcengruppe aus. Geben Sie Ihre Abonnement-Id, den Namen des neuen Ressourcengruppe und Speicherort, die Sie für Ihre Lösung benötigen. Weitere Informationen finden Sie unter [Erstellen einer Ressourcengruppe](https://msdn.microsoft.com/library/azure/dn790525.aspx).

        PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>?api-version=2015-01-01
          <common headers>
          {
            "location": "West US",
            "tags": {
               "tagname1": "tagvalue1"
            }
          }
   
3. Überprüfen Sie Ihre Bereitstellung vor der Ausführung durch Ausführen des Vorgangs [Überprüfen einer Vorlage Bereitstellung](https://msdn.microsoft.com/library/azure/dn790547.aspx) . Wenn Sie die Bereitstellung zu testen, geben Sie Parameter genau wie beim Ausführen der bereitstellungs (Siehe im nächsten Schritt).

3. Erstellen einer bereitstellungs. Geben Sie Ihre Abonnement-Id, den Namen der Ressourcengruppe bereitstellen, den Namen der Bereitstellung und einen Link zu Ihrer Vorlage. Informationen über die Vorlagendatei finden Sie unter [Parameter-Datei](#parameter-file). Weitere Informationen über die REST-API zum Erstellen einer Ressourcengruppe finden Sie unter [Erstellen einer Vorlage Bereitstellung](https://msdn.microsoft.com/library/azure/dn790564.aspx). Beachten Sie, dass der **Modus** **inkrementell**festgelegt ist. Führen Sie eine umfassende Bereitstellung legen Sie **Modus** auf **abgeschlossen**fest. Achten Sie bei den vollständigen Modus zu verwenden, wie Sie Ressourcen, die nicht in der Vorlage enthalten sind, versehentlich löschen können.
    
        PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2015-01-01
          <common headers>
          {
            "properties": {
              "templateLink": {
                "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
                "contentVersion": "1.0.0.0",
              },
              "mode": "Incremental",
              "parametersLink": {
                "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
                "contentVersion": "1.0.0.0",
              }
            }
          }
   
      Wenn Sie eine Antwort Inhalte melden möchten, enthalten Inhalte angefordert oder beides, in der Besprechungsanfrage **DebugSetting** .

        "debugSetting": {
          "detailLevel": "requestContent, responseContent"
        }

      Sie können Einrichten Ihres Speicherkontos Token einer freigegebenen Access-Signatur (SAS) verwenden. Weitere Informationen finden Sie unter [Delegieren von Access mit einer Access-Signatur freigegeben](https://msdn.microsoft.com/library/ee395415.aspx).

4. Erhalten Sie den Status der Bereitstellung Vorlage ein. Weitere Informationen finden Sie unter [erhalten von Informationen zu einer Vorlage Bereitstellung](https://msdn.microsoft.com/library/azure/dn790565.aspx).

          GET https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2015-01-01
           <common headers>

[AZURE.INCLUDE [resource-manager-parameter-file](../includes/resource-manager-parameter-file.md)]

## <a name="next-steps"></a>Nächste Schritte
- Ein Beispiel für die Bereitstellung von Ressourcen über die .NET Client-Bibliothek finden Sie unter [Bereitstellen von Ressourcen mithilfe von .NET Bibliotheken und einer Vorlage](virtual-machines/virtual-machines-windows-csharp-template.md).
- Zum Definieren von Parametern in einer Vorlage finden Sie unter [Authoring-Vorlagen](resource-group-authoring-templates.md#parameters).
- Bereitstellen der Lösung in verschiedenen Umgebungen Anleitungen finden Sie unter [Entwicklung und Test-Umgebungen in Microsoft Azure](solution-dev-test-environments.md).
- Details zur Verwendung von secure Werte übergeben eines Verweis KeyVault finden Sie unter [secure Werte während der Bereitstellung übergeben](resource-manager-keyvault-parameter.md).

<properties
   pageTitle="Anzeigen der Bereitstellung von Vorgängen mit REST-API | Microsoft Azure"
   description="Beschreibt, wie die Azure Ressourcenmanager REST-API verwenden, um Probleme aus Ressourcenmanager Bereitstellung zu erkennen."
   services="azure-resource-manager,virtual-machines"
   documentationCenter=""
   tags="top-support-issue"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-multiple"
   ms.workload="infrastructure"
   ms.date="06/13/2016"
   ms.author="tomfitz"/>

# <a name="view-deployment-operations-with-azure-resource-manager-rest-api"></a>Ansicht Bereitstellungsvorgänge mit Azure Ressourcenmanager REST-API

> [AZURE.SELECTOR]
- [Portal](resource-manager-troubleshoot-deployments-portal.md)
- [PowerShell](resource-manager-troubleshoot-deployments-powershell.md)
- [Azure CLI](resource-manager-troubleshoot-deployments-cli.md)
- [REST-API](resource-manager-troubleshoot-deployments-rest.md)

Wenn Sie einen Fehler bei der Bereitstellung Ressourcen für Azure erhalten haben, möchten Sie möglicherweise weitere Details zur Bereitstellungsvorgänge anzuzeigen, die ausgeführt wurden. Die REST-API bietet Vorgänge, mit denen Sie den Fehler suchen und mögliche Updates ermitteln.

[AZURE.INCLUDE [resource-manager-troubleshoot-introduction](../includes/resource-manager-troubleshoot-introduction.md)]

Sie können einige Fehler vermeiden, indem Sie Ihre Vorlage und Infrastruktur vor der Bereitstellung überprüfen. Sie können auch protokollieren zusätzliche Anfrage und Antwortinformationen während der Bereitstellung, die später zur Behandlung dieses Problems hilfreich sein können. Weitere Informationen zu überprüfen und Protokollierungsinformationen, die Anfrage und Antwort finden Sie unter [Bereitstellen einer Ressourcengruppe Ressourcenmanager Azure-Vorlage](resource-group-template-deploy-rest.md).

## <a name="troubleshoot-with-rest-api"></a>Behandeln von Problemen mit REST-API

1. Bereitstellen von Ressourcen für den Vorgang zum [Erstellen einer Vorlage Bereitstellung](https://msdn.microsoft.com/library/azure/dn790564.aspx) . Um Informationen zu behalten, die für das Debuggen hilfreich sein können, legen Sie die Eigenschaft **DebugSetting** in JSON-Anforderung an **RequestContent** und/oder **ResponseContent**. 

        PUT https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}?api-version={api-version}
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
              },
              "debugSetting": {
                "detailLevel": "requestContent, responseContent"
              }
            }
          }

    Standardmäßig wird der Wert **DebugSetting** **keine**festgelegt. Wenn Sie den Wert für **DebugSetting** angeben, überlegen Sie genau, die Art der Informationen, die Sie während der Bereitstellung in übergeben. Protokollieren von Informationen über die Anforderung oder Antwort könnten Sie potenziell vertrauliche Daten verfügbar machen, die über die Bereitstellungsvorgänge abgerufen werden. 

2. Erhalten von Informationen zu einer Bereitstellung mit dem Vorgang [erhalten von Informationen zu einer Vorlage Bereitstellung](https://msdn.microsoft.com/library/azure/dn790565.aspx) .

        GET https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}?api-version={api-version}

    Beachten Sie in der Antwort die Elemente **ProvisioningState** , **CorrelationId** und **Fehler** im besonderen. **CorrelationId** wird verwendet, um verwandte Ereignisse nachverfolgen und hilfreich sein kann, bei der Arbeit mit den technischen Support, um ein Problem zu beheben.
    
        { 
          ...
          "properties": {
            "provisioningState":"Failed",
            "correlationId":"d5062e45-6e9f-4fd3-a0a0-6b2c56b15757",
            ...
            "error":{
              "code":"DeploymentFailed","message":"At least one resource deployment operation failed. Please list deployment operations for details. Please see http://aka.ms/arm-debug for usage details.",
              "details":[{"code":"Conflict","message":"{\r\n  \"error\": {\r\n    \"message\": \"Conflict\",\r\n    \"code\": \"Conflict\"\r\n  }\r\n}"}]
            }  
          }
        }

3. Erhalten Sie Informationen zur Bereitstellung von Vorgängen mit der [Liste alle Vorgänge der Vorlage Bereitstellung](https://msdn.microsoft.com/library/azure/dn790518.aspx) Vorgang an. 

        GET https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}/operations?$skiptoken={skiptoken}&api-version={api-version}

    Die Antwort enthält Angaben Anforderung und/oder Antwort basierend auf was Sie während der Bereitstellung in der Eigenschaft **DebugSetting** angegeben.
    
        {
          ...
          "properties": 
          {
            ...
            "request":{
              "content":{
                "location":"West US",
                "properties":{
                  "accountType": "Standard_LRS"
                }
              }
            },
            "response":{
              "content":{
                "error":{
                  "message":"Conflict","code":"Conflict"
                }
              }
            }
          }
        }

4. Abrufen von Ereignissen aus der Überwachungsprotokolle für die Bereitstellung mit dem Vorgang [Listet die Verwaltungsereignisse in einem Abonnement](https://msdn.microsoft.com/library/azure/dn931934.aspx) .

        GET https://management.azure.com/subscriptions/{subscription-id}/providers/microsoft.insights/eventtypes/management/values?api-version={api-version}&$filter={filter-expression}&$select={comma-separated-property-names}


## <a name="next-steps"></a>Nächste Schritte

- Hilfe bei der Behebung von bestimmter Bereitstellungsfehler finden Sie unter [Beheben häufig auftretender Fehler beim Bereitstellen von Ressourcen in Azure Azure Ressourcenmanager](resource-manager-common-deployment-errors.md).
- Weitere Informationen zum Verwenden der Überwachungsprotokolle um zu anderen Typen von Aktionen zu überwachen, finden Sie unter [Überwachen von Vorgängen mit Ressourcen-Manager](resource-group-audit.md).
- Um Ihre Bereitstellung vor der Ausführung überprüfen zu können, finden Sie unter [Bereitstellen einer Ressourcengruppe Ressourcenmanager Azure-Vorlage](resource-group-template-deploy.md).

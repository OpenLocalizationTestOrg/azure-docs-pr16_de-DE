<properties
   pageTitle="Anzeigen der Bereitstellung von Vorgängen mit Azure CLI | Microsoft Azure"
   description="Beschreibt, wie die CLI Azure verwenden, um Probleme aus Ressourcenmanager Bereitstellung zu erkennen."
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
   ms.date="08/15/2016"
   ms.author="tomfitz"/>

# <a name="view-deployment-operations-with-azure-cli"></a>Bereitstellung-Vorgänge mit Azure CLI anzeigen

> [AZURE.SELECTOR]
- [Portal](resource-manager-troubleshoot-deployments-portal.md)
- [PowerShell](resource-manager-troubleshoot-deployments-powershell.md)
- [Azure CLI](resource-manager-troubleshoot-deployments-cli.md)
- [REST-API](resource-manager-troubleshoot-deployments-rest.md)

Wenn Sie einen Fehler bei der Bereitstellung Ressourcen für Azure erhalten haben, möchten Sie möglicherweise weitere Details zur Bereitstellungsvorgänge anzuzeigen, die ausgeführt wurden. Azure CLI bietet Befehle, mit denen Sie den Fehler suchen und mögliche Updates ermitteln.

[AZURE.INCLUDE [resource-manager-troubleshoot-introduction](../includes/resource-manager-troubleshoot-introduction.md)]

Sie können einige Fehler vermeiden, indem Sie Ihre Vorlage und Infrastruktur vor der Bereitstellung überprüfen. Sie können auch protokollieren zusätzliche Anfrage und Antwortinformationen während der Bereitstellung, die später zur Behandlung dieses Problems hilfreich sein können. Weitere Informationen zu überprüfen und Protokollierungsinformationen, die Anfrage und Antwort finden Sie unter [Bereitstellen einer Ressourcengruppe Ressourcenmanager Azure-Vorlage](resource-group-template-deploy-cli.md).

## <a name="use-audit-logs-to-troubleshoot"></a>Verwenden der Überwachungsprotokolle zur Problembehandlung

[AZURE.INCLUDE [resource-manager-audit-limitations](../includes/resource-manager-audit-limitations.md)]

Wenn Fehler für eine Bereitstellung anzeigen möchten, gehen Sie folgendermaßen vor:

1. Führen Sie zum Anzeigen der Überwachungsprotokolle Befehl **Azure Gruppe Protokoll anzeigen** aus. Sie können einschließen der **– letzten-Bereitstellung** Option, um nur das Protokoll für die letzte Bereitstellung abrufen.

        azure group log show ExampleGroup --last-deployment

2. Der Befehl **Azure Gruppe Log anzeigen** gibt eine große Datenmenge. Zur Behandlung dieses Problems, Sie normalerweise auf Vorgänge konzentrieren möchten, die fehlgeschlagen ist. Das folgende Skript verwendet die Option **– Json** und dem [Jq](https://stedolan.github.io/jq/) JSON-Programm, das Protokoll für Bereitstellung Fehlern suchen.

        azure group log show ExampleGroup --json | jq '.[] | select(.status.value == "Failed")'
        
        {
        "claims": {
          "aud": "https://management.core.windows.net/",
          "iss": "https://sts.windows.net/<guid>/",
          "iat": "1442510510",
          "nbf": "1442510510",
          "exp": "1442514410",
          "ver": "1.0",
          "http://schemas.microsoft.com/identity/claims/tenantid": "{guid}",
          "http://schemas.microsoft.com/identity/claims/objectidentifier": "{guid}",
          "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress": "someone@example.com",
          "puid": "XXXXXXXXXXXXXXXX",
          "http://schemas.microsoft.com/identity/claims/identityprovider": "example.com",
          "altsecid": "1:example.com:XXXXXXXXXXX",
          "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "<hash string="">",
          "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "Tom",
          "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "FitzMacken",
          "name": "Tom FitzMacken",
          "http://schemas.microsoft.com/claims/authnmethodsreferences": "pwd",
          "groups": "{guid}",
          "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": "example.com#someone@example.com",
          "wids": "{guid}",
          "appid": "{guid}",
          "appidacr": "0",
          "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
          "http://schemas.microsoft.com/claims/authnclassreference": "1",
          "ipaddr": "000.000.000.000"
        },
        "properties": {
          "statusCode": "Conflict",
          "statusMessage": "{\"Code\":\"Conflict\",\"Message\":\"Website with given name mysite already exists.\",\"Target\":null,\"Details\":[{\"Message\":\"Website with given name
            mysite already exists.\"},{\"Code\":\"Conflict\"},{\"ErrorEntity\":{\"Code\":\"Conflict\",\"Message\":\"Website with given name mysite already exists.\",\"ExtendedCode\":
            \"54001\",\"MessageTemplate\":\"Website with given name {0} already exists.\",\"Parameters\":[\"mysite\"],\"InnerErrors\":null}}],\"Innererror\":null}"
        },
        ...

    Sie können sehen, dass die **Eigenschaften** Informationen in Json zu den fehlerhaften Vorgang enthält.

    Können die **– ausführliche** und **Vv -** Optionen aus, um weitere Informationen über die Protokolle anzuzeigen.  Verwenden der **– ausführliche** Option, um die Schritte anzuzeigen, die Vorgänge aufzurufen, klicken Sie auf `stdout`. Verwenden Sie für eine vollständige Anforderung Verlauf **Vv -** Option aus. Die Nachrichten stellen häufig wichtige Hinweise über die Ursache der Fehler zur Verfügung.

3. Um auf die Meldung zum Verbindungsstatus für fehlgeschlagene Einträge konzentrieren, verwenden Sie den folgenden Befehl aus:

        azure group log show ExampleGroup --json | jq -r ".[] | select(.status.value == \"Failed\") | .properties.statusMessage"


## <a name="use-deployment-operations-to-troubleshoot"></a>Verwenden Sie zum Behandeln von Problemen mit Bereitstellungsvorgänge

1. Abrufen von den allgemeinen Status einer Bereitstellung mit dem Befehl **Azure Gruppe Bereitstellung anzeigen** . Im folgenden Beispiel ist die Bereitstellung fehlgeschlagen.

        azure group deployment show --resource-group ExampleGroup --name ExampleDeployment
        
        info:    Executing command group deployment show
        + Getting deployments
        data:    DeploymentName     : ExampleDeployment
        data:    ResourceGroupName  : ExampleGroup
        data:    ProvisioningState  : Failed
        data:    Timestamp          : 2015-08-27T20:03:34.9178576Z
        data:    Mode               : Incremental
        data:    Name             Type    Value
        data:    ---------------  ------  ------------
        data:    siteName         String  ExampleSite
        data:    hostingPlanName  String  ExamplePlan
        data:    siteLocation     String  West US
        data:    sku              String  Free
        data:    workerSize       String  0
        info:    group deployment show command OK

2. Wenn die Nachricht für fehlgeschlagene Vorgänge für eine Bereitstellung anzeigen möchten, verwenden Sie ein:

        azure group deployment operation list --resource-group ExampleGroup --name ExampleDeployment --json  | jq ".[] | select(.properties.provisioningState == \"Failed\") | .properties.statusMessage.Message"


## <a name="next-steps"></a>Nächste Schritte

- Hilfe bei der Behebung von bestimmter Bereitstellungsfehler finden Sie unter [Beheben häufig auftretender Fehler beim Bereitstellen von Ressourcen in Azure Azure Ressourcenmanager](resource-manager-common-deployment-errors.md).
- Weitere Informationen zum Verwenden der Überwachungsprotokolle um zu anderen Typen von Aktionen zu überwachen, finden Sie unter [Überwachen von Vorgängen mit Ressourcen-Manager](resource-group-audit.md).
- Um Ihre Bereitstellung vor der Ausführung überprüfen zu können, finden Sie unter [Bereitstellen einer Ressourcengruppe Ressourcenmanager Azure-Vorlage](resource-group-template-deploy.md).

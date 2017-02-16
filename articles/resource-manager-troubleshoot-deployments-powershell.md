<properties
   pageTitle="Anzeigen der Bereitstellung von Vorgängen mit PowerShell | Microsoft Azure"
   description="Beschreibt, wie die Azure PowerShell verwenden, um Probleme aus Ressourcenmanager Bereitstellung zu erkennen."
   services="azure-resource-manager,virtual-machines"
   documentationCenter=""
   tags="top-support-issue"
   authors="tfitzmac"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-multiple"
   ms.workload="infrastructure"
   ms.date="06/14/2016"
   ms.author="tomfitz"/>

# <a name="view-deployment-operations-with-azure-powershell"></a>Bereitstellung-Vorgänge mit Azure PowerShell anzeigen

> [AZURE.SELECTOR]
- [Portal](resource-manager-troubleshoot-deployments-portal.md)
- [PowerShell](resource-manager-troubleshoot-deployments-powershell.md)
- [Azure CLI](resource-manager-troubleshoot-deployments-cli.md)
- [REST-API](resource-manager-troubleshoot-deployments-rest.md)

Sie können die Vorgänge für eine Bereitstellung über die Azure PowerShell anzeigen. Sie können möglicherweise am interessantesten anzeigen die Vorgänge aus, wenn Sie einen Fehler während der Bereitstellung erhalten haben, damit dieser Artikel befasst sich Vorgänge, die nicht anzeigen. PowerShell bietet Cmdlets, mit denen Sie auf einfache Weise finden die Fehler und mögliche Updates ermitteln.

[AZURE.INCLUDE [resource-manager-troubleshoot-introduction](../includes/resource-manager-troubleshoot-introduction.md)]

Sie können einige Fehler vermeiden, indem Sie Ihre Vorlage und Infrastruktur vor der Bereitstellung überprüfen. Sie können auch protokollieren zusätzliche Anfrage und Antwortinformationen während der Bereitstellung, die später zur Behandlung dieses Problems hilfreich sein können. Weitere Informationen zu überprüfen und Protokollierungsinformationen, die Anfrage und Antwort finden Sie unter [Bereitstellen einer Ressourcengruppe Ressourcenmanager Azure-Vorlage](resource-group-template-deploy.md).

## <a name="use-deployment-operations-to-troubleshoot"></a>Verwenden Sie zum Behandeln von Problemen mit Bereitstellungsvorgänge

1. Um den allgemeinen Status einer Bereitstellung erhalten möchten, verwenden Sie den Befehl **Get-AzureRmResourceGroupDeployment** aus. Sie können die Ergebnisse für nur diese Bereitstellungen filtern, die fehlgeschlagen ist.

        Get-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup | Where-Object ProvisioningState -eq Failed
        
    Die gibt der fehlerhaften Bereitstellungen in folgendem Format ein:
        
        DeploymentName          : Microsoft.Template
        ResourceGroupName       : ExampleGroup
        ProvisioningState       : Failed
        Timestamp               : 6/14/2016 9:55:21 PM
        Mode                    : Incremental
        TemplateLink            :
        Parameters              :
                    Name                Type                 Value
                    ===============     ===================  ==========
                    storageAccountName  String               tfstorage9855
                    adminUsername       String               tfadmin
                    adminPassword       SecureString
                    dnsNameforLBIP      String               dns
                    vmNamePrefix        String               myVM
                    imagePublisher      String               MicrosoftWindowsServer
                    imageOffer          String               WindowsServer
                    imageSKU            String               2012-R2-Datacenter
                    lbName              String               myLB
                    nicNamePrefix       String               nic
                    publicIPAddressName String               myPublicIP
                    vnetName            String               myVNET
                    vmSize              String               Standard_D1

        Outputs                 :
        DeploymentDebugLogLevel :

2. Jede Bereitstellung besteht in der Regel von mehreren Vorgängen, für jeden Vorgang einen Schritt im Bereitstellungsprozess darstellt. Um zu ermitteln, was mit einer Bereitstellung des Problems, müssen Sie in der Regel Details über die Bereitstellungsvorgänge anzuzeigen. Sie können den Status der Vorgänge mit **Get-AzureRmResourceGroupDeploymentOperation**anzeigen.

        Get-AzureRmResourceGroupDeploymentOperation -ResourceGroupName ExampleGroup -DeploymentName Microsoft.Template
        
    Die mehrere Vorgänge für jede Zeile in folgendem Format zurückgegeben:
        
        Id             : /subscriptions/{guid}/resourceGroups/ExampleGroup/providers/Microsoft.Resources/deployments/Microsoft.Template/operations/A3EB2DA598E0A780
        OperationId    : A3EB2DA598E0A780
        Properties     : @{provisioningOperation=Create; provisioningState=Succeeded; timestamp=2016-06-14T21:55:15.0156208Z;
                         duration=PT23.0227078S; trackingId=11d376e8-5d6d-4da8-847e-6f23c6443fbf;
                         serviceRequestId=0196828d-8559-4bf6-b6b8-8b9057cb0e23; statusCode=OK; targetResource=}
        PropertiesText : {duration:PT23.0227078S, provisioningOperation:Create, provisioningState:Succeeded,
                         serviceRequestId:0196828d-8559-4bf6-b6b8-8b9057cb0e23...}

3. Um weitere Details zu fehlgeschlagene Vorgänge zu gelangen, können ermitteln Sie die Eigenschaften für Vorgänge mit Status **Fehler** .

        (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName Microsoft.Template -ResourceGroupName ExampleGroup).Properties | Where-Object ProvisioningState -eq Failed
        
    Die gibt alle fehlgeschlagene Vorgänge für jede Zeile in folgendem Format:
        
        provisioningOperation : Create
        provisioningState     : Failed
        timestamp             : 2016-06-14T21:54:55.1468068Z
        duration              : PT3.1449887S
        trackingId            : f4ed72f8-4203-43dc-958a-15d041e8c233
        serviceRequestId      : a426f689-5d5a-448d-a2f0-9784d14c900a
        statusCode            : BadRequest
        statusMessage         : @{error=}
        targetResource        : @{id=/subscriptions/{guid}/resourceGroups/ExampleGroup/providers/
                                Microsoft.Network/publicIPAddresses/myPublicIP;
                                resourceType=Microsoft.Network/publicIPAddresses; resourceName=myPublicIP}

    Beachten Sie die Verlauf-ID für den Vorgang an. Sie werden, die im nächsten Schritt verwenden, klicken Sie auf einen bestimmten Vorgang vereinfacht.

4. Wenn die Meldung zum Verbindungsstatus eines bestimmten Fehler beim Vorgangs erhalten möchten, verwenden Sie den folgenden Befehl aus:

        ((Get-AzureRmResourceGroupDeploymentOperation -DeploymentName Microsoft.Template -ResourceGroupName ExampleGroup).Properties | Where-Object trackingId -eq f4ed72f8-4203-43dc-958a-15d041e8c233).StatusMessage.error
        
    Diese Funktion gibt zurück:
        
        code           message                                                                        details
        ----           -------                                                                        -------
        DnsRecordInUse DNS record dns.westus.cloudapp.azure.com is already used by another public IP. {}

## <a name="use-audit-logs-to-troubleshoot"></a>Verwenden der Überwachungsprotokolle zur Problembehandlung

[AZURE.INCLUDE [resource-manager-audit-limitations](../includes/resource-manager-audit-limitations.md)]

Wenn Fehler für eine Bereitstellung anzeigen möchten, gehen Sie folgendermaßen vor:

1. Führen Sie zum Abrufen von Protokolleinträge **Get-AzureRmLog** Befehl ein. Die Parameter **ResourceGroup** und **Status** können nur Ereignisse zurück, die für eine einzelne Ressourcengruppe fehlgeschlagen ist. Wenn Sie eine Start- und Zeit nicht angeben, werden die Einträge für die letzte Stunde zurückgegeben.
Beispiel zum Abrufen der fehlgeschlagene Vorgänge für die vergangenen Stunde ausführen:

        Get-AzureRmLog -ResourceGroup ExampleGroup -Status Failed

    Sie können bestimmten Zeitspanne angeben. Im nächsten Beispiel werden fehlgeschlagene Aktionen für den letzten Tag erläutert. 

        Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-1) -Status Failed
      
    Alternativ können Sie eine genaue Start- und Endzeit fehlgeschlagene Aktionen festlegen:

        Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime 2015-08-28T06:00 -EndTime 2015-09-10T06:00 -Status Failed

2. Wenn dieser Befehl zu viele Einträge und Eigenschaften zurückgibt, können Sie sich ü konzentrieren durch die **Properties** -Eigenschaft abrufen. Wir werden ebenfalls den Parameter **DetailedOutput** , um die Fehlermeldungen finden Sie unter sind.

        (Get-AzureRmLog -Status Failed -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-1) -DetailedOutput).Properties
        
    Gibt die Eigenschaften des Webparts für die Protokolleinträge, in folgendem Format:
        
        Content
        -------
        {} 
        {[statusCode, BadRequest], [statusMessage, {"error":{"code":"DnsRecordInUse","message":"DNS record dns.westus.clouda...
        {[statusCode, BadRequest], [serviceRequestId, a426f689-5d5a-448d-a2f0-9784d14c900a], [statusMessage, {"error":{"code...

3. Konzentrieren auf Basis dieser Ergebnisse, wir uns auf das zweite Element. Sie können die Ergebnisse verfeinern, indem Sie die Meldung zum Verbindungsstatus für diesen Eintrag.

        ((Get-AzureRmLog -Status Failed -ResourceGroup ExampleGroup -DetailedOutput -StartTime (Get-Date).AddDays(-1)).Properties[1].Content["statusMessage"] | ConvertFrom-Json).error
        
    Diese Funktion gibt zurück:
        
        code           message                                                                        details
        ----           -------                                                                        -------
        DnsRecordInUse DNS record dns.westus.cloudapp.azure.com is already used by another public IP. {}



## <a name="next-steps"></a>Nächste Schritte

- Hilfe bei der Behebung von bestimmter Bereitstellungsfehler finden Sie unter [Beheben häufig auftretender Fehler beim Bereitstellen von Ressourcen in Azure Azure Ressourcenmanager](resource-manager-common-deployment-errors.md).
- Weitere Informationen zum Verwenden der Überwachungsprotokolle um zu anderen Typen von Aktionen zu überwachen, finden Sie unter [Überwachen von Vorgängen mit Ressourcen-Manager](resource-group-audit.md).
- Um Ihre Bereitstellung vor der Ausführung überprüfen zu können, finden Sie unter [Bereitstellen einer Ressourcengruppe Ressourcenmanager Azure-Vorlage](resource-group-template-deploy.md).


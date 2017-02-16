<properties
   pageTitle="Bereitstellen von Ressourcen mit PowerShell und Vorlage | Microsoft Azure"
   description="Formular mit Azure Ressourcenmanager und Azure PowerShell Azure einer Ressourcen bereitstellen. Die Ressourcen werden in einer Vorlage Ressourcenmanager definiert."
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
   ms.date="08/15/2016"
   ms.author="tomfitz"/>

# <a name="deploy-resources-with-resource-manager-templates-and-azure-powershell"></a>Bereitstellen von Ressourcen mit Ressourcenmanager Vorlagen und Azure-PowerShell

> [AZURE.SELECTOR]
- [PowerShell](resource-group-template-deploy.md)
- [Azure CLI](resource-group-template-deploy-cli.md)
- [Portal](resource-group-template-deploy-portal.md)
- [REST-API](resource-group-template-deploy-rest.md)

In diesem Thema wird erläutert, wie Azure PowerShell mit Ressourcenmanager Vorlagen zu verwenden, um Ihre Ressourcen Azure bereitstellen.  

> [AZURE.TIP] Hilfe für das Debuggen ein Fehler während der Bereitstellung finden Sie unter:
>
> - [Ansicht Bereitstellungsvorgänge mit Azure PowerShell](resource-manager-troubleshoot-deployments-powershell.md) Informationen dazu, wie Sie Informationen, die Ihnen dabei hilft Behandeln von Problemen mit der Fehler
> - [Behandeln von häufigen Fehlern beim Bereitstellen von Ressourcen in Azure Azure Ressourcenmanager](resource-manager-common-deployment-errors.md) Informationen zum Beheben häufig auftretender Bereitstellungsfehler

Ihre Vorlage kann entweder eine lokale Datei oder eine externe Datei, die durch einen URI zur Verfügung. Wenn Sie Ihre Vorlage in einem Speicherkonto befindet, können Sie einschränken des Zugriffs auf die Vorlage und Bereitstellen einer freigegebenen Access Signatur (SAS) Token während der Bereitstellung.

## <a name="quick-steps-to-deployment"></a>QuickSteps mit-Bereitstellung

In diesem Artikel werden alle anderen Optionen, die Sie während der Bereitstellung zur Verfügung. Jedoch beliebig oft zwei einfache Befehle. Um schnell mit der Bereitstellung beginnen, verwenden Sie die folgenden Befehle:

    New-AzureRmResourceGroup -Name ExampleResourceGroup -Location "West US"
    New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate> -TemplateParameterFile <PathToParameterFile>

Um weitere Informationen zu Optionen für die Bereitstellung, die in Ihrem Szenario besser geeignet werden möglicherweise weiterhin in diesem Artikel.

[AZURE.INCLUDE [resource-manager-deployments](../includes/resource-manager-deployments.md)]

## <a name="deploy-with-powershell"></a>Bereitstellen mit PowerShell

1. Melden Sie sich bei Ihrem Azure-Konto an.

        Add-AzureRmAccount

     Eine Übersicht über Ihr Konto wird zurückgegeben.

        Environment : AzureCloud
        Account     : someone@example.com
        ...

2. Wenn Sie mehrere Abonnements verfügen, geben Sie die Abonnement-ID, die Sie für die Bereitstellung mit dem Befehl **Set-AzureRmContext** verwenden möchten. 

        Set-AzureRmContext -SubscriptionID <YourSubscriptionId>

3. In der Regel, wenn Sie eine neue Vorlage bereitstellen, Sie eine Ressourcengruppe aus, um die Ressourcen enthalten erstellen möchten. Wenn Sie eine vorhandene Ressourcengruppe, der Sie bereitstellen möchten verfügen, können Sie diesen Schritt überspringen und die Ressourcengruppe verwenden. 

     Zum Erstellen einer Ressourcengruppe geben Sie einen Namen und einen Speicherort für Ihre Ressourcengruppe aus. Sie müssen einen Speicherort für die Ressourcengruppe bereitstellen, weil die Ressourcengruppe Metadaten zu Ressourcen speichert. Aus Gründen der Compliance möchten Sie möglicherweise angeben, dass Metadaten gespeichert ist. Es wird empfohlen, dass Sie einen Speicherort angeben, in dem meisten Ressourcen gespeichert werden soll. Mithilfe derselben Stelle können Sie Ihre Vorlage vereinfachen.

        New-AzureRmResourceGroup -Name ExampleResourceGroup -Location "West US"
   
     Es wird eine Zusammenfassung der neuen Ressourcengruppe zurückgegeben.
   
        ResourceGroupName : ExampleResourceGroup
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
             Actions  NotActions
             =======  ==========
             *
        ResourceId        : /subscriptions/######/resourceGroups/ExampleResourceGroup

4. Vor die Bereitstellung ausgeführt wird, können Sie Ihre Einstellungen für die Bereitstellung überprüfen. Das Cmdlet " **Test-AzureRmResourceGroupDeployment** " können Sie Probleme vor dem Erstellen der aktuelle Ressourcen suchen. Im folgenden Beispiel wird gezeigt, wie eine Bereitstellung überprüft.

        Test-AzureRmResourceGroupDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate>

5. Um Ressourcen der Ressourcengruppe bereitzustellen, führen Sie den Befehl **Neu-AzureRmResourceGroupDeployment** , und geben Sie die erforderlichen Parameter. Die Parameter umfassen einen Namen für die Bereitstellung, den Namen der Ressourcengruppe, den Pfad oder URL der Vorlage, die Sie erstellt haben und alle anderen Parameter für Ihr Szenario erforderlich. Wenn der Parameter **Modus** nicht angegeben ist, wird der Standardwert **inkrementell** verwendet. Führen Sie eine umfassende Bereitstellung legen Sie **Modus** auf **abgeschlossen**fest. Achten Sie bei den vollständigen Modus zu verwenden, wie Sie Ressourcen, die nicht in der Vorlage enthalten sind, versehentlich löschen können.

     Um eine lokale Vorlage bereitstellen möchten, verwenden Sie den **TemplateFile** -Parameter ein:

        New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate>

     Verwenden Sie zum Bereitstellen einer externen Vorlage **TemplateUri** Parameter ein:

        New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateUri <LinkToTemplate>
   
     Sie haben die folgenden Optionen für die Bereitstellung von Parameterwerte: 
   
     1. Verwenden von Inline-Parametern.

            New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate> -myParameterName "parameterValue"

     2. Verwenden Sie eine Parameterobjekt.

            $parameters = @{"<ParameterName>"="<Parameter Value>"}
            New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate> -TemplateParameterObject $parameters

     3. Verwenden Sie eine lokale Parameterdatei ein. Informationen über die Vorlagendatei finden Sie unter [Parameter-Datei](#parameter-file).

            New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate> -TemplateParameterFile <PathToParameterFile>

     4. Verwenden einer externen Parameterdatei. Informationen über die Vorlagendatei finden Sie unter [Parameter-Datei](#parameter-file). 

            New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateUri <LinkToTemplate> -TemplateParameterUri <LinkToParameterFile>

        Wenn Sie eine Parameterdatei externe verwenden, Sie können keine anderen Werte übergeben entweder Inline oder aus einer lokalen Datei. Weitere Informationen finden Sie unter [Parameter Vorrang](#parameter-precendence).

     Nachdem die Ressourcen bereitgestellt wurden, sehen Sie eine Zusammenfassung der Bereitstellung.

        DeploymentName    : ExampleDeployment
        ResourceGroupName : ExampleResourceGroup
        ProvisioningState : Succeeded
        Timestamp         : 4/14/2015 7:00:27 PM
        Mode              : Incremental
        ...

     Wenn Sie Ihre Vorlage in der PowerShell-Befehl ein Parameters mit demselben Namen wie einer der Parameter enthält, werden Sie aufgefordert, einen Wert für diesen Parameter bereitzustellen. Der Parameter aus der Vorlage werden dem Suffix **FromTemplate**enthalten. Den Namen eines Parameters beispielsweise **ResourceGroupName** in Ihrer Vorlage Konflikte mit dem Parameter **ResourceGroupName** in das Cmdlet " [New-AzureRmResourceGroupDeployment](https://msdn.microsoft.com/library/azure/mt679003.aspx) ". Sie werden aufgefordert, einen Wert für **ResourceGroupNameFromTemplate**bereitzustellen. Im Allgemeinen sollten Sie diese Verwirrung vermeiden, indem Sie nicht Benennen von Parametern mit demselben Namen als Parameter für Bereitstellung Operationen verwendet.

6. Wenn Sie weitere Informationen zur Bereitstellung melden Sie sich, die Ihnen helfen können, Problembehandlung bei Bereitstellungsfehlern, verwenden Sie den Parameter **DeploymentDebugLogLevel** möchten. Sie können angeben, dass Inhalte angefordert, Antwortinhalt oder beides mit dem Bereitstellungsvorgang angemeldet sein.

        New-AzureRmResourceGroupDeployment -Name ExampleDeployment -DeploymentDebugLogLevel All -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate>
        
     Weitere Informationen zur Verwendung dieses Inhaltstyps Debuggen Bereitstellungen Problembehandlung finden Sie unter [Problembehandlung Ressource Gruppe Bereitstellungen mit Azure PowerShell](resource-manager-troubleshoot-deployments-powershell.md).

## <a name="deploy-template-from-storage-with-sas-token"></a>Bereitstellen von Speicher mit Token SAS-Vorlage

Sie können Ihre Vorlagen eine Speicher-Konto und einen Link zu während der Bereitstellung mit einem SAS Token hinzufügen.

> [AZURE.IMPORTANT] Das mit der Vorlage Blob zugegriffen werden anhand der folgenden Schritte aus, nur mit dem Konto Besitzer. Wenn Sie ein SAS Token für das Blob erstellen, ist das Blob für jede Person mit diesen URI zugänglich. Wenn ein anderer Benutzer den URI hört ab, ist diese Benutzer auf die Vorlage zugreifen. Mit einem Token SAS ist eine gute Möglichkeit, Einschränken des Zugriffs auf Ihre Vorlagen, aber nehmen Sie vertrauliche Daten wie Kennwörter nicht direkt in die Vorlage.

### <a name="add-private-template-to-storage-account"></a>Hinzufügen von privaten Vorlage zu Speicher-Konto

Den folgenden Schritten Einrichten eines Kontos Speicher für Vorlagen:

1. Erstellen Sie eine Ressourcengruppe aus.

        New-AzureRmResourceGroup -Name ManageGroup -Location "West US"

2. Erstellen eines Speicher-Kontos an. Den Namen des Kontos Speicher müssen für Azure eindeutig sein, also Geben Sie Ihren eigenen Name für das Konto.

        New-AzureRmStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates -Type Standard_LRS -Location "West US"

3. Einrichten des Speicherkontos des aktuellen an.

        Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates

4. Erstellen eines Containers an. Die Berechtigung ist auf **Off** festgelegt, was bedeutet, dass der Container nur an den Besitzer zugegriffen werden.

        New-AzureStorageContainer -Name templates -Permission Off
        
5. Fügen Sie Ihre Vorlage in den Container hinzu.

        Set-AzureStorageBlobContent -Container templates -File c:\Azure\Templates\azuredeploy.json
        
### <a name="provide-sas-token-during-deployment"></a>Bereitstellen von SAS Token während der Bereitstellung

Zum Bereitstellen von privaten Vorlage in einem Speicherkonto abzurufen Sie ein SAS Token, und fügen Sie es in den URI für die Vorlage.

1. Wenn Sie das aktuelle Speicherkonto geändert haben, schieben Sie das aktuellen Speicherkonto mit, der Ihre Vorlagen.

        Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates

2. Erstellen Sie ein SAS Token mit Leseberechtigungen und einem Ablaufdatum Zugriff einschränken. Rufen Sie die vollständige URI der Vorlage, einschließlich des SAS Tokens ab.

        $templateuri = New-AzureStorageBlobSASToken -Container templates -Blob azuredeploy.json -Permission r -ExpiryTime (Get-Date).AddHours(2.0) -FullUri

3. Die Vorlage, indem Bereitstellen des URIS, der das SAS Token enthält.

        New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleResourceGroup -TemplateUri $templateuri

Ein Beispiel für ein Token SAS mit verknüpften Vorlagen verwenden finden Sie unter [Verwendung von verknüpften Vorlagen Azure Ressourcenmanager](resource-group-linked-templates.md).

[AZURE.INCLUDE [resource-manager-parameter-file](../includes/resource-manager-parameter-file.md)]

## <a name="parameter-precedence"></a>Parameter der Rangfolge

Sie können Inline-Parameter und eine lokale Parameterdatei in der gleichen Bereitstellungsvorgang. Beispielsweise können Sie einige Werte in der Parameterdatei lokale angeben und andere Werte Inline während der Bereitstellung hinzufügen. Wenn Sie Werte für einen Parameter in die lokale Parameterdatei und Inline bereitstellen, hat der Inlinewert Vorrang vor.

Sie können keine jedoch Inline-Parameter mit einer externen Parameterdatei verwenden. Wenn Sie eine Parameterdatei in den **TemplateParameterUri** -Parameter angeben, werden alle Inline-Parameter ignoriert. Sie müssen alle Parameterwerte in der externen Datei angeben. Wenn Sie Ihre Vorlage einen vertraulichen Wert enthält, den Sie in der Parameterdatei aufnehmen können, entweder fügen Sie Werts zu einem Key Tresor hinzu und verwiesen Sie werden den wichtigsten Tresor in der externen Parameterdatei oder dynamisch zur Verfügung stellen Sie alle Parameter Werte Inline.

Details zur Verwendung von secure Werte übergeben eines Verweis KeyVault finden Sie unter [secure Werte während der Bereitstellung übergeben](resource-manager-keyvault-parameter.md).

## <a name="next-steps"></a>Nächste Schritte
- Ein Beispiel für die Bereitstellung von Ressourcen über die .NET Client-Bibliothek finden Sie unter [Bereitstellen von Ressourcen mithilfe von .NET Bibliotheken und einer Vorlage](virtual-machines/virtual-machines-windows-csharp-template.md).
- Zum Definieren von Parametern in einer Vorlage finden Sie unter [Authoring-Vorlagen](resource-group-authoring-templates.md#parameters).
- Bereitstellen der Lösung in verschiedenen Umgebungen Anleitungen finden Sie unter [Entwicklung und Test-Umgebungen in Microsoft Azure](solution-dev-test-environments.md).


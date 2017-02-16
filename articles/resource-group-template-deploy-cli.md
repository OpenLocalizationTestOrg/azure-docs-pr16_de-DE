<properties
   pageTitle="Bereitstellen von Ressourcen mit Azure CLI und Vorlage | Microsoft Azure"
   description="Formular mit Azure Ressourcenmanager und Azure CLI eine Ressourcen Azure bereitstellen. Die Ressourcen werden in einer Vorlage Ressourcenmanager definiert."
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

# <a name="deploy-resources-with-resource-manager-templates-and-azure-cli"></a>Bereitstellen von Ressourcen mit Ressourcenmanager Vorlagen und Azure CLI

> [AZURE.SELECTOR]
- [PowerShell](resource-group-template-deploy.md)
- [Azure CLI](resource-group-template-deploy-cli.md)
- [Portal](resource-group-template-deploy-portal.md)
- [REST-API](resource-group-template-deploy-rest.md)

In diesem Thema wird erläutert, wie Azure CLI mit Ressourcenmanager Vorlagen zu verwenden, um Ihre Ressourcen Azure bereitstellen.  

> [AZURE.TIP] Hilfe für das Debuggen ein Fehler während der Bereitstellung finden Sie unter:
>
> - [Ansicht Bereitstellungsvorgänge mit Azure CLI](resource-manager-troubleshoot-deployments-cli.md) Informationen dazu, wie Sie Informationen, die Ihnen dabei hilft Behandeln von Problemen mit der Fehler
> - [Behandeln von häufigen Fehlern beim Bereitstellen von Ressourcen in Azure Azure Ressourcenmanager](resource-manager-common-deployment-errors.md) Informationen zum Beheben häufig auftretender Bereitstellungsfehler

Ihre Vorlage kann entweder eine lokale Datei oder eine externe Datei, die durch einen URI zur Verfügung. Wenn Sie Ihre Vorlage in einem Speicherkonto befindet, können Sie einschränken des Zugriffs auf die Vorlage und Bereitstellen einer freigegebenen Access Signatur (SAS) Token während der Bereitstellung.

## <a name="quick-steps-to-deployment"></a>QuickSteps mit-Bereitstellung

In diesem Artikel werden alle anderen Optionen, die Sie während der Bereitstellung zur Verfügung. Jedoch beliebig oft zwei einfache Befehle. Um schnell mit der Bereitstellung beginnen, verwenden Sie die folgenden Befehle:

    azure group create -n ExampleResourceGroup -l "West US"
    azure group deployment create -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

Um weitere Informationen zu Optionen für die Bereitstellung, die in Ihrem Szenario besser geeignet werden möglicherweise weiterhin in diesem Artikel.

[AZURE.INCLUDE [resource-manager-deployments](../includes/resource-manager-deployments.md)]

## <a name="deploy-with-azure-cli"></a>Mit Azure CLI bereitstellen

Wenn Sie nicht zuvor Azure CLI mit Ressourcenmanager verwendet haben, finden Sie unter [Verwendung der CLI Azure für Mac, Linux, und Windows Azure Ressource Verwaltung](xplat-cli-azure-resource-manager.md).

1. Melden Sie sich bei Ihrem Azure-Konto an. Nach der Bereitstellung Ihrer Anmeldeinformationen, gibt der Befehl Ihren Benutzernamen als Ergebnis zurück.

        azure login
  
        ...
        info:    login command OK

2. Wenn Sie mehrere Abonnements verfügen, geben Sie die Abonnement-Id, die Sie für die Bereitstellung verwenden möchten.

        azure account set <YourSubscriptionNameOrId>

3. Wechseln Sie zur Azure Ressourcenmanager Modul. Sie erhalten eine Bestätigung der neuen Modus.

        azure config mode arm
   
        info:     New mode is arm

4. Wenn Sie nicht über eine vorhandene Ressourcengruppe verfügen, erstellen Sie eine Ressourcengruppe aus. Geben Sie den Namen der Ressourcengruppe und Speicherort, die Sie für Ihre Lösung benötigen. Sie müssen einen Speicherort für die Ressourcengruppe bereitstellen, weil die Ressourcengruppe Metadaten zu Ressourcen speichert. Aus Gründen der Compliance möchten Sie möglicherweise angeben, dass Metadaten gespeichert ist. Es wird empfohlen, dass Sie einen Speicherort angeben, in dem meisten Ressourcen gespeichert werden soll. Mithilfe derselben Stelle können Sie Ihre Vorlage vereinfachen.

        azure group create -n ExampleResourceGroup -l "West US"

     Es wird eine Zusammenfassung der neuen Ressourcengruppe zurückgegeben.
   
        info:    Executing command group create
        + Getting resource group ExampleResourceGroup
        + Creating resource group ExampleResourceGroup
        info:    Created resource group ExampleResourceGroup
        data:    Id:                  /subscriptions/####/resourceGroups/ExampleResourceGroup
        data:    Name:                ExampleResourceGroup
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags:
        data:
        info:    group create command OK

5. Überprüfen Sie Ihre Bereitstellung vor der Ausführung durch Ausführen des Befehls **Azure Gruppenvorlage zu bestätigen** . Wenn Sie die Bereitstellung zu testen, geben Sie Parameter genau wie beim Ausführen der bereitstellungs (Siehe im nächsten Schritt).

        azure group template validate -f <PathToTemplate> -p "{\"ParameterName\":{\"value\":\"ParameterValue\"}}" -g ExampleResourceGroup

5. Um Ressourcen der Ressourcengruppe bereitzustellen, führen Sie den folgenden Befehl aus, und geben Sie die erforderlichen Parameter. Die Parameter umfassen einen Namen für die Bereitstellung, den Namen der Ressourcengruppe, den Pfad oder URL der Vorlage, und alle anderen Parameter für Ihr Szenario erforderlich. 
   
     Sie haben die folgenden drei Optionen für die Bereitstellung von Parameterwerte: 

     1. Verwenden von Parametern Inline und eine lokale Vorlage. Jeder Parameter wird im Format: `"ParameterName": { "value": "ParameterValue" }`. Im folgenden Beispiel wird die Parameter mit Escape-Zeichen.

            azure group deployment create -f <PathToTemplate> -p "{\"ParameterName\":{\"value\":\"ParameterValue\"}}" -g ExampleResourceGroup -n ExampleDeployment

     2. Verwenden von Parametern Inline und einen Link zu einer Vorlage.

            azure group deployment create --template-uri <LinkToTemplate> -p "{\"ParameterName\":{\"value\":\"ParameterValue\"}}" -g ExampleResourceGroup -n ExampleDeployment

     3. Verwenden Sie eine Parameterdatei ein. Informationen über die Vorlagendatei finden Sie unter [Parameter-Datei](#parameter-file).
    
            azure group deployment create -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

     Nachdem Sie die Ressourcen über einen der oben genannten drei Methoden bereitgestellt wurden, sehen Sie eine Zusammenfassung der Bereitstellung.
  
        info:    Executing command group deployment create
        + Initializing template configurations and parameters
        + Creating a deployment
        ...
        info:    group deployment create command OK

     Führen Sie eine umfassende Bereitstellung legen Sie **Modus** auf **abgeschlossen**fest. Achten Sie darauf, wenn dieser Modus verwenden, wie Sie Ressourcen, die nicht in der Vorlage enthalten sind, versehentlich löschen können.

        azure group deployment create --mode Complete -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

6. Wenn Sie weitere Informationen zur Bereitstellung melden Sie sich, die Ihnen helfen können, Problembehandlung bei Bereitstellungsfehlern, verwenden Sie den Parameter **Einstellung Debuggen** möchten. Sie können angeben, dass Inhalte angefordert, Antwortinhalt oder beides mit dem Bereitstellungsvorgang angemeldet sein.

        azure group deployment create --debug-setting All -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

## <a name="deploy-template-from-storage-with-sas-token"></a>Bereitstellen von Speicher mit Token SAS-Vorlage

Sie können Ihre Vorlagen eine Speicher-Konto und einen Link zu während der Bereitstellung mit einem SAS Token hinzufügen.

> [AZURE.IMPORTANT] Das mit der Vorlage Blob zugegriffen werden anhand der folgenden Schritte aus, nur mit dem Konto Besitzer. Wenn Sie ein SAS Token für das Blob erstellen, ist das Blob für jede Person mit diesen URI zugänglich. Wenn ein anderer Benutzer den URI hört ab, ist diese Benutzer auf die Vorlage zugreifen. Mit einem Token SAS ist eine gute Möglichkeit, Einschränken des Zugriffs auf Ihre Vorlagen, aber nehmen Sie vertrauliche Daten wie Kennwörter nicht direkt in die Vorlage.

### <a name="add-private-template-to-storage-account"></a>Hinzufügen von privaten Vorlage zu Speicher-Konto

Den folgenden Schritten Einrichten eines Kontos Speicher für Vorlagen:

1. Erstellen Sie eine Ressourcengruppe aus.

        azure group create -n "ManageGroup" -l "westus"

2. Erstellen eines Speicher-Kontos an. Den Namen des Kontos Speicher müssen für Azure eindeutig sein, also Geben Sie Ihren eigenen Name für das Konto.

        azure storage account create -g ManageGroup -l "westus" --sku-name LRS --kind Storage storagecontosotemplates

3. Setzen Sie Variablen für den Speicher-Konto und -Taste.

        export AZURE_STORAGE_ACCOUNT=storagecontosotemplates
        export AZURE_STORAGE_ACCESS_KEY={storage_account_key}

4. Erstellen eines Containers an. Die Berechtigung ist auf **Off** festgelegt, was bedeutet, dass der Container nur an den Besitzer zugegriffen werden.

        azure storage container create --container templates -p Off 
        
4. Fügen Sie Ihre Vorlage in den Container hinzu.

        azure storage blob upload --container templates -f c:\Azure\Templates\azuredeploy.json
        
### <a name="provide-sas-token-during-deployment"></a>Bereitstellen von SAS Token während der Bereitstellung

Zum Bereitstellen von privaten Vorlage in einem Speicherkonto abzurufen Sie ein SAS Token, und fügen Sie es in den URI für die Vorlage.

1. Erstellen Sie ein SAS Token mit Leseberechtigungen und einem Ablaufdatum Zugriff einschränken. Legen Sie die Ablaufzeit genügend Zeit für die Bereitstellung ausführen dürfen. Rufen Sie die vollständige URI der Vorlage, einschließlich des SAS Tokens ab.

        expiretime=$(date -I'minutes' --date "+30 minutes")
        fullurl=$(azure storage blob sas create --container templates --blob azuredeploy.json --permissions r --expiry $expiretimetime --json  | jq ".url")

2. Die Vorlage, indem Bereitstellen des URIS, der das SAS Token enthält.

        azure group deployment create --template-uri $fullurl -g ExampleResourceGroup

Ein Beispiel für ein Token SAS mit verknüpften Vorlagen verwenden finden Sie unter [Verwendung von verknüpften Vorlagen Azure Ressourcenmanager](resource-group-linked-templates.md).

[AZURE.INCLUDE [resource-manager-parameter-file](../includes/resource-manager-parameter-file.md)]

## <a name="next-steps"></a>Nächste Schritte
- Ein Beispiel für die Bereitstellung von Ressourcen über die .NET Client-Bibliothek finden Sie unter [Bereitstellen von Ressourcen mithilfe von .NET Bibliotheken und einer Vorlage](virtual-machines/virtual-machines-windows-csharp-template.md).
- Zum Definieren von Parametern in einer Vorlage finden Sie unter [Authoring-Vorlagen](resource-group-authoring-templates.md#parameters).
- Bereitstellen der Lösung in verschiedenen Umgebungen Anleitungen finden Sie unter [Entwicklung und Test-Umgebungen in Microsoft Azure](solution-dev-test-environments.md).
- Details zur Verwendung von secure Werte übergeben eines Verweis KeyVault finden Sie unter [secure Werte während der Bereitstellung übergeben](resource-manager-keyvault-parameter.md).


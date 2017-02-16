<properties
    pageTitle="Einrichten von Key Tresor für virtuellen Computern in Azure Ressourcenmanager | Microsoft Azure"
    description="Informationen zum Schlüssel Tresor für die Verwendung mit einem Ressourcenmanager Azure-virtuellen Computern einrichten."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/31/2016"
    ms.author="singhkay"/>

# <a name="set-up-key-vault-for-virtual-machines-in-azure-resource-manager"></a>Einrichten von Key Tresor für virtuellen Computern in Azure Ressourcenmanager

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]Klassische Bereitstellungsmodell

In einem Stapel mit Azure Ressourcenmanager werden die Kennwörter/Zertifikate als Ressourcen erstellt, die vom Ressourcenanbieter der Schlüssel Tresor bereitgestellt werden. Weitere Informationen zum Schlüssel Tresor finden Sie unter [Neuigkeiten Azure-Taste Tresor?](../key-vault/key-vault-whatis.md)

>[AZURE.NOTE] 
>
>1. Damit Schlüssel Tresor mit Ressourcenmanager Azure-virtuellen Computern verwendet werden kann, müssen Sie die Eigenschaft **EnabledForDeployment** auf-Taste Tresor festlegen auf True. Sie können dies in verschiedenen Clients ausführen.
>
>2. Die Taste Tresor muss auf dem gleichen Abonnement und Speicherort des virtuellen Computers erstellt werden.

## <a name="use-powershell-to-set-up-key-vault"></a>Verwenden von PowerShell-Taste Tresor einrichten
Zum Erstellen eines Key Tresor mithilfe der PowerShell finden Sie unter [Erste Schritte mit Azure Schlüssel Tresor](../key-vault/key-vault-get-started.md#vault).

Neue Key Depots können Sie diese PowerShell-Cmdlet:

    New-AzureRmKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia' -EnabledForDeployment

Für vorhandene Key Depots können Sie diese PowerShell-Cmdlet verwenden:

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -EnabledForDeployment

## <a name="us-cli-to-set-up-key-vault"></a>Uns CLI Schlüssel Tresor einrichten
Zum Erstellen eines Key Tresor mithilfe der Line Interface (CLI) finden Sie unter [Verwalten Schlüssel Tresor über die Befehlszeilenschnittstelle](../key-vault/key-vault-manage-with-cli.md#create-a-key-vault).

Für CLI müssen Sie den Taste Tresor erstellen, bevor Sie die Bereitstellungsrichtlinie zuweisen. Sie können dazu verwenden den folgenden Befehl aus:

    azure keyvault set-policy ContosoKeyVault –enabled-for-deployment true

## <a name="use-templates-to-set-up-key-vault"></a>Verwenden von Vorlagen zum Schlüssel Tresor einrichten
Während Sie eine Vorlage verwenden, müssen Sie festlegen der `enabledForDeployment` Eigenschaft zu `true` für die Ressource Tresor-Taste.

    {
      "type": "Microsoft.KeyVault/vaults",
      "name": "ContosoKeyVault",
      "apiVersion": "2015-06-01",
      "location": "<location-of-key-vault>",
      "properties": {
        "enabledForDeployment": "true",
        ....
        ....
      }
    }

Um weitere Optionen anzuzeigen, die Sie konfigurieren können, wenn Sie eine Key Tresor mithilfe von Vorlagen erstellen, finden Sie unter [Erstellen eines Key Tresor](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).

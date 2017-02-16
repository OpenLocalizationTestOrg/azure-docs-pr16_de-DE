<properties
   pageTitle="Benutzerdefinierte Skripts auf Windows-virtuellen Computern mithilfe von Vorlagen | Microsoft Azure"
   description="Automatisieren von Aufgaben zur virtuellen Windows-Computer mithilfe der Erweiterung benutzerdefinierte Skripts mit Ressourcenmanager Vorlagen"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="kundanap"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="03/29/2016"
   ms.author="kundanap"/>

# <a name="windows-vm-custom-script-extensions-with-azure-resource-manager-templates"></a>Windows virtueller Computer benutzerdefinierte Skripts Erweiterungen mit Azure Ressourcenmanager Vorlagen

[AZURE.INCLUDE [virtual-machines-common-extensions-customscript](../../includes/virtual-machines-common-extensions-customscript.md)]

## <a name="template-example-for-a-windows-vm"></a>Beispiel der Vorlage für einen virtuellen Windows

Definieren Sie den folgenden Ressourcen im Abschnitt Ressource der Vorlage ein.

       {
       "type": "Microsoft.Compute/virtualMachines/extensions",
       "name": "MyCustomScriptExtension",
       "apiVersion": "2015-05-01-preview",
       "location": "[parameters('location')]",
       "dependsOn": [
           "[concat('Microsoft.Compute/virtualMachines/',parameters('vmName'))]"
       ],
       "properties": {
           "publisher": "Microsoft.Compute",
           "type": "CustomScriptExtension",
           "typeHandlerVersion": "1.7",
           "autoUpgradeMinorVersion":true,
           "settings": {
               "fileUris": [
               "http://Yourstorageaccount.blob.core.windows.net/customscriptfiles/start.ps1"
           ],
           "commandToExecute": "powershell.exe -ExecutionPolicy Unrestricted -File start.ps1"
         }
       }
     }

Ersetzen Sie im vorherigen Beispiel die Datei-URL und den Dateinamen durch Ihre eigenen Einstellungen aus.
Nach der Erstellung der Vorlage, können Sie es mithilfe der PowerShell Azure bereitstellen.

Wenn Sie das Skript-URLs und Parameter privat bleiben möchten, können Sie die URL des Skripts als **Privat**festlegen. Wenn das Skript URL als **Privat**festgelegt ist, können sie nur mit einem Speicher Kontonamen und als Einstellungen für die geschützte gesendeten Schlüssels zugegriffen werden. Die Skriptparameter können auch als geschützten Einstellungen mit Version 1.7 oder höher der benutzerdefinierte Skripts Erweiterung bereitgestellt werden.

## <a name="template-example-for-a-windows-vm-with-protected-settings"></a>Beispiel der Vorlage für ein Windows virtueller Computer mit Einstellungen für die geschützte

        {
        "publisher": "Microsoft.Compute",
        "type": "CustomScriptExtension",
        "typeHandlerVersion": "1.7",
        "settings": {
        "fileUris": [
        "http: //Yourstorageaccount.blob.core.windows.net/customscriptfiles/start.ps1"
        ]
        },
        "protectedSettings": {
        "commandToExecute": "powershell.exe -ExecutionPolicy Unrestricted -start.ps1",
        "storageAccountName": "yourStorageAccountName",
        "storageAccountKey": "yourStorageAccountKey"
        }
        }
Informationen über das Schema über die neuesten Versionen der Erweiterung benutzerdefinierte Skripts finden Sie unter [Azure Windows virtueller Computer Erweiterung Konfiguration Beispiele](virtual-machines-windows-extensions-configuration-samples.md).

Beispiele für Anwendungskonfiguration eines virtuellen Computers benutzerdefiniertes Skript-Erweiterung verwenden finden Sie [benutzerdefinierte Skripts Erweiterung einer Windows virtuellen Computers](https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/201-list-storage-keys-windows-vm/azuredeploy.json/).

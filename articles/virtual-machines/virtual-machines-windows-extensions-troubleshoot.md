<properties
   pageTitle="Problembehandlung bei Windows virtueller Computer Erweiterung Fehlern | Microsoft Azure"
   description="Erfahren Sie mehr über die Problembehandlung Azure Windows virtueller Computer Erweiterung Fehlern"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="kundanap"
   manager="timlt"
   editor=""
   tags="top-support-issue,azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="03/29/2016"
   ms.author="kundanap"/>

# <a name="troubleshooting-azure-windows-vm-extension-failures"></a>Problembehandlung bei Azure Windows virtueller Computer Erweiterung Fehlern

[AZURE.INCLUDE [virtual-machines-common-extensions-troubleshoot](../../includes/virtual-machines-common-extensions-troubleshoot.md)]

## <a name="viewing-extension-status"></a>Zum Anzeigen des Erweiterungsstatus
Azure Ressourcenmanager Vorlagen können von Azure PowerShell ausgeführt werden. Nachdem Sie die Vorlage ausgeführt wird, kann der Erweiterungsstatus aus Azure Ressource Explorer oder die Befehlszeilentools angezeigt werden.

Hier ist ein Beispiel:

Azure PowerShell:

      Get-AzureRmVM -ResourceGroupName $RGName -Name $vmName -Status

So sieht die Ausgabe der Stichprobe aus:

      Extensions:  {
      "ExtensionType": "Microsoft.Compute.CustomScriptExtension",
      "Name": "myCustomScriptExtension",
      "SubStatuses": [
        {
          "Code": "ComponentStatus/StdOut/succeeded",
          "DisplayStatus": "Provisioning succeeded",
          "Level": "Info",
          "Message": "    Directory: C:\\temp\\n\\n\\nMode                LastWriteTime     Length Name
              \\n----                -------------     ------ ----                              \\n-a---          9/1/2015   2:03 AM         11
              test.txt                          \\n\\n",
                      "Time": null
          },
        {
          "Code": "ComponentStatus/StdErr/succeeded",
          "DisplayStatus": "Provisioning succeeded",
          "Level": "Info",
          "Message": "",
          "Time": null
        }
    }
  ]

## <a name="troubleshooting-extension-failures"></a>Problembehandlung von Fehlern Erweiterung

### <a name="re-running-the-extension-on-the-vm"></a>Erneutes Ausführen der Erweiterung des virtuellen Computers

Wenn Sie Skripts des virtuellen Computers benutzerdefinierte Skripts Erweiterung verwenden ausgeführt werden, können Sie manchmal ausführen in einen Fehler zurück, in dem virtuellen Computer wurde erfolgreich erstellt, aber das Skript fehlgeschlagen ist. Diese Conditons ist das empfohlene Verfahren zum Wiederherstellen eines aus dieser Fehler die Erweiterung entfernen, und führen die Vorlage erneut.
Hinweis: In Zukunft möchten diese Funktionalität erweitert werden um die Notwendigkeit, deinstallieren die Erweiterung zu entfernen.


#### <a name="remove-the-extension-from-azure-powershell"></a>Entfernen Sie die Erweiterung aus Azure PowerShell

    Remove-AzureRmVMExtension -ResourceGroupName $RGName -VMName $vmName -Name "myCustomScriptExtension"

Sobald die Erweiterung entfernt wurde, kann die Vorlage erneut ausgeführt die Skripts des virtuellen Computers ausgeführt werden.

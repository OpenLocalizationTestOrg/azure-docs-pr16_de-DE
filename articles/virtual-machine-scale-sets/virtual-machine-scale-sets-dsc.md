<properties
   pageTitle="Bundesland Konfiguration mit virtuellen Computern Skala mit gewünscht | Microsoft Azure"
   description="Mit der Erweiterung für Azure DSC legt mit Skalierung virtuellen Computern"
   services="virtual-machine-scale-sets"
   documentationCenter=""
   authors="zjalexander"
   manager="timlt"
   editor=""
   tags="azure-service-management,azure-resource-manager"
   keywords=""/>

<tags
   ms.service="virtual-machine-scale-sets"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="na"
   ms.date="09/15/2016"
   ms.author="zachal"/>

# <a name="using-virtual-machine-scale-sets-with-the-azure-dsc-extension"></a>Mit der Erweiterung für Azure DSC legt mit Skalierung virtuellen Computern

[Virtuellen Computern skalieren Datensätze (VMSS)](virtual-machine-scale-sets-overview.md) können mit dem [Azure gewünscht Zustand Konfiguration (DSC)](../virtual-machines/virtual-machines-windows-extensions-dsc-overview.md) Erweiterung Ereignishandler verwendet werden. VMSS bietet eine Möglichkeit zum Bereitstellen und Verwalten von großen Anzahl von virtuellen Computern und kann als Antwort auf Laden elastisch ein-und skalieren. DSC dient zum Konfigurieren der virtuellen Computern als sie online geschaltet, damit sie die Herstellung Software ausgeführt werden.

## <a name="differences-between-deploying-to-vm-and-vmss"></a>Unterschiede zwischen dem Bereitstellen virtueller Computer und VMSS

Die Vorlagenstruktur der zugrunde liegenden für VMSS unterscheidet sich leicht von eines einzelnen virtuellen Computers. Insbesondere bereitstellt ein einzelnes virtuellen Computers Erweiterungen unter dem Knoten "VirtualMachines". Es ist ein Eintrag vom Typ "Extensions", in dem die Vorlage DSC hinzugefügt wird

```
"resources": [
          {
              "name": "Microsoft.Powershell.DSC",
              "type": "extensions",
              "location": "[resourceGroup().location]",
              "apiVersion": "2015-06-15",
              "dependsOn": [
                  "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
              ],
              "tags": {
                  "displayName": "dscExtension"
              },
              "properties": {
                  "publisher": "Microsoft.Powershell",
                  "type": "DSC",
                  "typeHandlerVersion": "2.20",
                  "autoUpgradeMinorVersion": false,
                  "forceUpdateTag": "[parameters('dscExtensionUpdateTagVersion')]",
                  "settings": {
                      "configuration": {
                          "url": "[concat(parameters('_artifactsLocation'), '/', variables('dscExtensionArchiveFolder'), '/', variables('dscExtensionArchiveFileName'))]",
                          "script": "DscExtension.ps1",
                          "function": "Main"
                      },
                      "configurationArguments": {
                          "nodeName": "[variables('vmName')]"
                      }
                  },
                  "protectedSettings": {
                      "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                  }
              }
          }
      ]
```

Ein VMSS-Knoten hat einen Eigenschaftenabschnitt "" mit der "VirtualMachineProfile", "ExtensionProfile" Attribut. DSC ist unter "Extensions" hinzugefügt.

```
"extensionProfile": {
            "extensions": [
                {
                    "name": "Microsoft.Powershell.DSC",
                    "properties": {
                        "publisher": "Microsoft.Powershell",
                        "type": "DSC",
                        "typeHandlerVersion": "2.20",
                        "autoUpgradeMinorVersion": false,
                        "forceUpdateTag": "[parameters('DscExtensionUpdateTagVersion')]",
                        "settings": {
                            "configuration": {
                                "url": "[concat(parameters('_artifactsLocation'), '/', variables('DscExtensionArchiveFolder'), '/', variables('DscExtensionArchiveFileName'))]",
                                "script": "DscExtension.ps1",
                                "function": "Main"
                            },
                            "configurationArguments": {
                                "nodeName": "localhost"
                            }
                        },
                        "protectedSettings": {
                            "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                        }
                    }
                }
            ]
```

## <a name="behavior-for-vmss"></a>Verhalten für VMSS

Das Verhalten für VMSS ist mit dem Verhalten eines einzelnen virtuellen Computers identisch. Beim Erstellen ein neues virtuellen Computers ist, wird es automatisch mit der Erweiterung DSC bereitgestellt. Wenn eine neuere Version von WMF die Erweiterung erforderlich ist, startet der virtuellen Computer neu, bevor er online geschaltet. Nachdem sie online ist, der DSC Konfiguration ZIP-downloads und Provisioning: des virtuellen Computers. Weitere Informationen hierzu finden Sie in [Der Übersicht über die Azure DSC Erweiterung](../virtual-machines/virtual-machines-windows-extensions-dsc-overview.md).

## <a name="next-steps"></a>Nächste Schritte ##
Untersuchen der [Ressourcenmanager Azure-Vorlage für die Erweiterung DSC](../virtual-machines/virtual-machines-windows-extensions-dsc-template.md)an.

Erfahren Sie, wie die [Erweiterung DSC verarbeitet sichere Anmeldeinformationen](../virtual-machines/virtual-machines-windows-extensions-dsc-credentials.md). 

Weitere Informationen zu den Azure DSC Erweiterung Ereignishandler finden Sie unter [Einführung in die Konfiguration des Azure gewünscht Erweiterung Ereignishandler](../virtual-machines/virtual-machines-windows-extensions-dsc-overview.md). 

Weitere Informationen zu PowerShell DSC, [finden Sie auf der Mitte der PowerShell-Dokumentation](https://msdn.microsoft.com/powershell/dsc/overview). 



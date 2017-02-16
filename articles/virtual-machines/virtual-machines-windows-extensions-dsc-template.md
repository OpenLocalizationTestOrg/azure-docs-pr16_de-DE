<properties
   pageTitle="Bundesland Konfiguration Ressource-Manager Vorlage gewünscht | Microsoft Azure"
   description="Ressourcenmanager Vorlage Definition für gewünscht Zustand Konfiguration in Azure mit Beispielen und zur Problembehandlung"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="zjalexander"
   manager="timlt"
   editor=""
   tags="azure-service-management,azure-resource-manager"
   keywords=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="na"
   ms.date="09/15/2016"
   ms.author="zachal"/>

# <a name="windows-vmss-and-desired-state-configuration-with-azure-resource-manager-templates"></a>Windows-VMSS und gewünscht Zustand Konfiguration mit Azure Ressourcenmanager Vorlagen
Dieser Artikel beschreibt die Vorlage Ressourcenmanager für den [Bundesstaat Konfiguration gewünschte Erweiterung Ereignishandler](virtual-machines-windows-extensions-dsc-overview.md). 

## <a name="template-example-for-a-windows-vm"></a>Beispiel der Vorlage für einen virtuellen Windows

Im folgende Codeausschnitt wechselt in der Abschnitt Ressource der Vorlage.

```json
            "name": "Microsoft.Powershell.DSC",
            "type": "extensions",
             "location": "[resourceGroup().location]",
             "apiVersion": "2015-06-15",
             "dependsOn": [
                  "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
              ],
              "properties": {
                  "publisher": "Microsoft.Powershell",
                  "type": "DSC",
                  "typeHandlerVersion": "2.20",
                  "autoUpgradeMinorVersion": true,
                  "forceUpdateTag": "[parameters('dscExtensionUpdateTagVersion')]",
                  "settings": {
                      "configuration": {
                          "url": "[concat(parameters('_artifactsLocation'), '/', variables('dscExtensionArchiveFolder'), '/', variables('dscExtensionArchiveFileName'))]",
                          "script": "dscExtension.ps1",
                          "function": "Main"
                      },
                      "configurationArguments": {
                          "nodeName": "[variables('vmName')]"
                      }
                  },
                  "protectedSettings": {
                      "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                  }

```

## <a name="template-example-for-windows-vmss"></a>Beispiel der Vorlage für Windows-VMSS

Ein VMSS-Knoten hat einen Eigenschaftenabschnitt "" mit der "VirtualMachineProfile", "ExtensionProfile" Attribut. DSC ist unter "Extensions" hinzugefügt. 

```json
"extensionProfile": {
            "extensions": [
                {
                    "name": "Microsoft.Powershell.DSC",
                    "properties": {
                        "publisher": "Microsoft.Powershell",
                        "type": "DSC",
                        "typeHandlerVersion": "2.20",
                        "autoUpgradeMinorVersion": true,
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

## <a name="detailed-settings-information"></a>Für detaillierte Einstellungsinformationen

Das folgende Schema ist für den Einstellungen Teil der Erweiterung Azure DSC in einer Ressourcenmanager Azure-Vorlage.

```json

"settings": {
  "wmfVersion": "latest",
  "configuration": {
    "url": "http://validURLToConfigLocation",
    "script": "ConfigurationScript.ps1",
    "function": "ConfigurationFunction"
  },
  "configurationArguments": {
    "argument1": "Value1",
    "argument2": "Value2"
  },
  "configurationData": {
    "url": "https://foo.psd1"
  },
  "privacy": {
    "dataCollection": "enable"
  },
  "advancedOptions": {
    "downloadMappings": {
      "customWmfLocation": "http://myWMFlocation"
    }
  } 
},
"protectedSettings": {
  "configurationArguments": {
    "parameterOfTypePSCredential1": {
      "userName": "UsernameValue1",
      "password": "PasswordValue1"
    },
    "parameterOfTypePSCredential2": {
      "userName": "UsernameValue2",
      "password": "PasswordValue2"
    }
  },
  "configurationUrlSasToken": "?g!bber1sht0k3n",
  "configurationDataUrlSasToken": "?dataAcC355T0k3N"
}

```

## <a name="details"></a>Details
| Eigenschaftsname | Typ | Beschreibung |
| --- | --- | --- |
| settings.wmfVersion | Zeichenfolge | Gibt die Version von der Windows Management Framework, die auf Ihre virtuellen Computer installiert werden soll. Wenn diese Eigenschaft auf 'letzte' Installationen die aktuellste Version der WMF. Sind die möglichen nur aktuelle Werte für diese Eigenschaft **'4.0', '5.0', ' 5.0PP' und'spätesten'**. Diese möglichen Werte werden Updates. Der Standardwert ist 'letzte'.|
| Settings.Configuration.URL | Zeichenfolge | Gibt den URL-Speicherort aus, für die DSC Konfiguration Zip-Datei herunterladen. Wenn die bereitgestellte URL ein SAS Token für den Zugriff erforderlich ist, müssen Sie die Eigenschaft protectedSettings.configurationUrlSasToken auf den Wert für das Token SAS festgelegt. Diese Eigenschaft ist erforderlich, wenn settings.configuration.script und/oder settings.configuration.function definiert sind. |
| Settings.Configuration.Script | Zeichenfolge | Gibt den Dateinamen des Skripts, das die Definition der Konfiguration DSC enthält. Dieses Skript muss im Stammverzeichnis der Zip-Datei von der URL angegeben haben, indem Sie die Eigenschaft configuration.url heruntergeladen werden. Diese Eigenschaft ist erforderlich, wenn settings.configuration.url und/oder settings.configuration.script definiert sind. |
| Settings.Configuration.Function | Zeichenfolge | Gibt den Namen der Konfiguration DSC. Die Konfiguration mit dem Namen muss im vom configuration.script definierten Skript enthalten sein. Diese Eigenschaft ist erforderlich, wenn settings.configuration.url und/oder settings.configuration.function definiert sind. |
| settings.configurationArguments | Websitesammlung | Definiert die Parameter ein, die Sie an der Konfiguration DSC übergeben möchten. Diese Eigenschaft wird nicht verschlüsselt. |
| settings.configurationData.url | Zeichenfolge | Gibt die URL die Konfiguration-Datendatei (.pds1) download von als Eingabe für Ihre Konfiguration DSC verwenden. Wenn die bereitgestellte URL ein SAS Token für den Zugriff erforderlich ist, müssen Sie die Eigenschaft protectedSettings.configurationDataUrlSasToken auf den Wert für das Token SAS festgelegt.|
| settings.privacy.dataEnabled | Zeichenfolge | Aktiviert oder deaktiviert werden Websitesammlung. Nur mögliche Werte für diese Eigenschaft sind **'Aktivieren', 'Deaktivieren', '', oder $null**. Lassen dieser Eigenschaft leer oder null aktiviert werden. Ist der Standardwert ''. [Weitere Informationen](https://blogs.msdn.microsoft.com/powershell/2016/02/02/azure-dsc-extension-data-collection-2/) |
| settings.advancedOptions.downloadMappings | Websitesammlung | Definiert alternative Pfade für die WMF herunterladen. [Weitere Informationen](http://blogs.msdn.com/b/powershell/archive/2015/10/21/azure-dsc-extension-2-2-amp-how-to-map-downloads-of-the-extension-dependencies-to-your-own-location.aspx) |
| protectedSettings.configurationArguments | Websitesammlung | Definiert die Parameter ein, die Sie an der Konfiguration DSC übergeben möchten. Diese Eigenschaft ist verschlüsselt. |
| protectedSettings.configurationUrlSasToken | Zeichenfolge | Gibt das Token SAS Zugriff auf die URL, die durch configuration.url definiert. Diese Eigenschaft ist verschlüsselt. |
| protectedSettings.configurationDataUrlSasToken | Zeichenfolge | Gibt das Token SAS Zugriff auf die URL, die durch configurationData.url definiert. Diese Eigenschaft ist verschlüsselt. |

## <a name="settings-vs-protectedsettings"></a>Einstellungen im Vergleich zu ProtectedSettings
Alle Einstellungen werden in eine Textdatei Einstellungen des virtuellen Computers gespeichert.
Eigenschaften unter 'Einstellungen' sind öffentliche Eigenschaften aus, da sie nicht in der Textdatei Einstellungen verschlüsselt sind.
Eigenschaften unter 'ProtectedSettings' mit einem Zertifikat verschlüsselt sind und nicht im nur-Text in dieser Datei des virtuellen Computers angezeigt werden.

Wenn die Konfiguration Anmeldeinformationen erforderlich ist, können sie in ProtectedSettings aufgenommen werden:

```json
"protectedSettings": {
    "configurationArguments": {
        "parameterOfTypePSCredential1": {
            "userName": "UsernameValue1",
            "password": "PasswordValue1"
        }
    }
}
```

## <a name="example"></a>Beispiel

Im folgende Beispiel wird aus dem Abschnitt "Erste Schritte" der [DSC Erweiterung Ereignishandler Übersichtsseite](virtual-machines-windows-extensions-dsc-overview.md)abgeleitet.
In diesem Beispiel wird Ressourcenmanager Vorlagen anstelle von Cmdlets für die Erweiterung bereitzustellen. Speichern Sie die Konfiguration "IisInstall.ps1", platzieren Sie es in ein. ZIP-Datei, und Laden Sie die Datei in einer URL zugegriffen werden. In diesem Beispiel wird die Azure Blob-Speicher, aber es ist möglich, herunterladen. ZIP-Dateien von jedem beliebigen Standort aus.

In der Vorlage Azure Ressourcenmanager weist der folgende Code den virtuellen Computer, um die richtige Datei herunterladen, und führen Sie die entsprechende PowerShell-Funktion:

```json
"settings": {
    "configuration": {
        "url": "https://demo.blob.core.windows.net/",
        "script": "IisInstall.ps1",
        "function": "IISInstall"
    }
    } 
},
"protectedSettings": {
    "configurationUrlSasToken": "odLPL/U1p9lvcnp..."
}
```

## <a name="updating-from-the-previous-format"></a>Aktualisieren von älteren Format
Änderungen an den Einstellungen im vorherigen Format (mit den öffentlichen Eigenschaften, ModulesUrl, ConfigurationFunction, SasToken oder Eigenschaften) automatisch in das aktuelle Format anpassen, und führen Sie einfach wie vor.

Das folgende Schema ist vom vorherigen Einstellungsschema vergeblich wie:

```json
"settings": {
    "WMFVersion": "latest",
    "ModulesUrl": "https://UrlToZipContainingConfigurationScript.ps1.zip",
    "SasToken": "SAS Token if ModulesUrl points to private Azure Blob Storage",
    "ConfigurationFunction": "ConfigurationScript.ps1\\ConfigurationFunction",
    "Properties":  {
        "ParameterToConfigurationFunction1":  "Value1",
        "ParameterToConfigurationFunction2":  "Value2",
        "ParameterOfTypePSCredential1": {
            "UserName": "UsernameValue1",
            "Password": "PrivateSettingsRef:Key1" 
        },
        "ParameterOfTypePSCredential2": {
            "UserName": "UsernameValue2",
            "Password": "PrivateSettingsRef:Key2"
        }
    }
},
"protectedSettings": { 
    "Items": {
        "Key1": "PasswordValue1",
        "Key2": "PasswordValue2"
    },
    "DataBlobUri": "https://UrlToConfigurationDataWithOptionalSasToken.psd1"
}
```

So sieht wie das vorherige Format in das aktuelle Format passt an:

| Eigenschaftsname | Vorherige Schema Entsprechung |
| --- | --- |
| settings.wmfVersion | Einstellungen. WMFVersion |
| Settings.Configuration.URL | Einstellungen. ModulesUrl |
| Settings.Configuration.Script | Ersten Teils der Einstellungen. ConfigurationFunction (vor dem '\\\\") |
| Settings.Configuration.Function | Zweiten Teils Einstellungen. ConfigurationFunction (nach '\\\\") |
| settings.configurationArguments | Einstellungen. Eigenschaften |
| settings.configurationData.url | protectedSettings.DataBlobUri (ohne SAS Token) |
| settings.privacy.dataEnabled | Einstellungen. Privacy.DataEnabled |
| settings.advancedOptions.downloadMappings | Einstellungen. AdvancedOptions.DownloadMappings |
| protectedSettings.configurationArguments | protectedSettings.Properties |
| protectedSettings.configurationUrlSasToken | Einstellungen. SasToken |
| protectedSettings.configurationDataUrlSasToken | SAS Token aus protectedSettings.DataBlobUri |


## <a name="troubleshooting---error-code-1100"></a>Problembehandlung - Fehlercode 1100
Fehlercode 1100 gibt an, dass es ein Problem mit der Eingabe des Benutzers an die Erweiterung DSC liegt.
Der Text dieser Fehler ist Variable und kann geändert werden.
Hier sind einige der Fehler, die, denen Sie möglicherweise auftreten, und wie Sie diese beheben können.

### <a name="invalid-values"></a>Ungültige Werte
"Privacy.dataCollection ist"{0}". Die einzige mögliche Werte sind '', 'Aktivieren' und 'Deaktivieren' ""WmfVersion ist "{0}". Nur die möglichen Werte sind... und 'letzte' "

Problem: Ein bereitgestellte Wert ist nicht zulässig.

Lösung: Ändern des über einen ungültigen Werts auf einen gültigen Wert an. Finden Sie in der Tabelle im Abschnitt Details.

### <a name="invalid-url"></a>Ungültige URL
"ConfigurationData.url ist"{0}". Dies ist keine gültige URL""DataBlobUri ist "{0}". Dies ist keine gültige URL""Configuration.url ist "{0}". Dies ist keine gültige URL"

Problem: A bereitgestellten-URL ist ungültig.

Lösung: Überprüfen Sie alle Ihre bereitgestellten URLs. Stellen Sie sicher, dass alle URLs auf gültige Speicherorte beheben, dass die Erweiterung auf dem Remotecomputer zugreifen kann.

### <a name="invalid-configurationargument-type"></a>Ungültige ConfigurationArgument Typ
"Ungültige ConfigurationArguments Geben Sie ein" {0}

Problem: Die Eigenschaft ConfigurationArguments kann nicht zu einem Objekt Hashtable aufgelöst werden. 

Lösung: Nehmen Sie Ihre ConfigurationArguments Eigenschaft Hashtable. Führen Sie das Format im Beispiel bereitgestellt. Achten Sie auf Angebote, Kommas und geschweiften Klammern.

### <a name="duplicate-configurationarguments"></a>Doppelte ConfigurationArguments
"Gefunden Sie doppelte Argumente"{0}"in öffentlichen und geschützten ConfigurationArguments"

Problem: Der ConfigurationArguments in öffentlichen Einstellungen und die ConfigurationArguments in den Einstellungen für die geschützte enthalten Eigenschaften mit demselben Namen.

Lösung: Entfernen Sie eine der doppelten Eigenschaften.

### <a name="missing-properties"></a>Fehlende Eigenschaften
"Configuration.function erfordert, dass configuration.url oder configuration.module angegeben ist"

"Configuration.url erfordert, dass die configuration.script angegeben ist"

"Configuration.script erfordert, dass die configuration.url angegeben ist"

"Configuration.url erfordert, dass die configuration.function angegeben ist"

"ConfigurationUrlSasToken erfordert, dass die configuration.url angegeben ist"

"ConfigurationDataUrlSasToken erfordert, dass die configurationData.url angegeben ist"

Problem: Eine definierte Eigenschaft benötigt eine weitere Eigenschaft, die nicht vorhanden ist.

Lösungen: 
- Bieten Sie die Eigenschaft fehlende.
- Entfernen Sie die Eigenschaft, die die fehlende Eigenschaft benötigt.


## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr über DSC und virtuellen Computern Maßstab legt fest, in der [Mithilfe von virtuellen Computern Maßstab legt mit der Erweiterung Azure DSC](../virtual-machine-scale-sets/virtual-machine-scale-sets-dsc.md)

Finden Sie weitere Details auf [den DSC sichere Verwaltung von Anmeldeinformationen](virtual-machines-windows-extensions-dsc-credentials.md). 

Weitere Informationen zu den Azure DSC Erweiterung Ereignishandler finden Sie unter [Einführung in die Konfiguration des Azure gewünscht Erweiterung Ereignishandler](virtual-machines-windows-extensions-dsc-overview.md). 

Weitere Informationen zu PowerShell DSC, [finden Sie auf der Mitte der PowerShell-Dokumentation](https://msdn.microsoft.com/powershell/dsc/overview). 

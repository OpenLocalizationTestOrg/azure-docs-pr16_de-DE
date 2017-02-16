<properties
   pageTitle="Benutzerdefinierte Skripts auf Linux virtuellen Computern | Microsoft Azure"
   description="Automatisieren von Aufgaben zur Linux VM mithilfe der benutzerdefinierte Skripts Erweiterung"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="neilpeterson"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="09/22/2016"
   ms.author="nepeters"/>

# <a name="using-the-azure-custom-script-extension-with-linux-virtual-machines"></a>Verwenden die Erweiterung Azure benutzerdefinierter Skripts mit Linux virtuellen Computern

Die benutzerdefinierte Skripts Erweiterung downloads und führt Skripts auf Azure-virtuellen Computern. Diese Erweiterung ist nützlich für Beitrag Bereitstellungskonfiguration, Installation der Software oder einer anderen Konfiguration / Management-Aufgabe. Skripts können von Azure-Speicher oder anderen barrierefreien Internetspeicherort heruntergeladen, oder an die Erweiterung Laufzeit bereitgestellt. Die Erweiterung benutzerdefinierte Skripts Azure Ressourcenmanager Vorlagen integriert, und kann auch mithilfe der CLI Azure, PowerShell, Azure-Portal oder die REST--API Azure-virtuellen Computern ausgeführt werden.

Dieses Dokument ausführlich erläutert, wie Sie die benutzerdefinierte Skripts Erweiterung aus Azure CLI und einer Ressourcenmanager Azure-Vorlage verwenden, und auch ausführlich Schritte zur Problembehandlung bei Linux-Systemen.

## <a name="extension-configuration"></a>Erweiterung Konfiguration

Die Konfiguration benutzerdefinierte Skripts Erweiterung gibt an, beispielsweise Skript Speicherort und der Befehl ausgeführt werden. Dieser Konfiguration kann in Konfigurationsdateien gespeichert werden, auf die Befehlszeile oder in einer Vorlage Azure Ressourcenmanager angegeben. Vertrauliche Daten können in einer geschützten Konfiguration, die verschlüsselt und entschlüsselt nur innerhalb des virtuellen Computers, gespeichert werden. Geschützte Konfiguration ist nützlich, wenn der Ausführungsbefehl geheimen Informationen, wie etwa ein Kennwort enthält.

### <a name="public-configuration"></a>Öffentliche Konfiguration

Schema:

- **CommandToExecute**: (erforderlich, Zeichenfolge) zum Ausführen der Eintrag Punkt Skripts
- **FileUris**: (optional, Zeichenfolgenarray) die URLs für Dateien, die heruntergeladen werden.
- **Zeitstempel** (optional, Integer) verwenden Sie dieses Feld nur für eine erneut ausführen des Skripts durch Ändern der Wert dieses Felds auslösen.

```none
{
  "fileUris": ["<url>"],
  "commandToExecute": "<command-to-execute>"
}
```

### <a name="protected-configuration"></a>Geschützte Konfiguration

Schema:

- **CommandToExecute**: (optional, Zeichenfolge) den Eintrag Punkt auszuführende Skript fest. Verwenden Sie dieses Feld, wenn der Befehl geheimen Informationen, wie z. B. Kennwörter enthält.
- **StorageAccountName**: (optional, Zeichenfolge) den Namen des Speicherkontos. Wenn Sie Speicherplatz Anmeldeinformationen angeben möchten, müssen alle FileUris URLs für Azure-Blobs sein.
- **StorageAccountKey**: (optional, Zeichenfolge) die Zugriffstaste für Speicher-Konto.


```json
{
  "commandToExecute": "<command-to-execute>",
  "storageAccountName": "<storage-account-name>",
  "storageAccountKey": "<storage-account-key>"
}
```

## <a name="azure-cli"></a>Azure CLI

Wenn die CLI Azure verwenden, um die benutzerdefinierte Skripts Erweiterung ausführen, Erstellen einer Konfigurationsdatei oder Dateien, die mindestens den Datei-Uri und den Befehl der Ausführung Skript enthalten.

```none
azure vm extension set <resource-group> <vm-name> CustomScript Microsoft.Azure.Extensions 2.0 --auto-upgrade-minor-version --public-config-path /scirpt-config.json
```

Optional können Sie der Befehl ausgeführt werden kann mithilfe der `--public-config` und `--private-config` Option, wodurch die Konfiguration Sie während der Ausführung und ohne eine separate Konfigurationsdatei angegeben werden muss.

```none
azure vm extension set <resource-group> <vm-name> CustomScript Microsoft.Azure.Extensions 2.0 --auto-upgrade-minor-version --public-config '{"fileUris": ["https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"],"commandToExecute": "./hello.sh"}'
```

### <a name="azure-cli-examples"></a>Beispiele für Azure CLI

**Beispiel 1** : öffentliche Konfiguration mit Skriptdatei.

```json
{
  "fileUris": ["https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"],
  "commandToExecute": "./hello.sh"
}
```

Azure CLI-Befehl:

```none
azure vm extension set <resource-group> <vm-name> CustomScript Microsoft.Azure.Extensions 2.0 --auto-upgrade-minor-version --public-config-path /public.json
```

**Beispiel 2** : öffentliche Konfiguration mit keine Skriptdatei.

```json
{
  "commandToExecute": "apt-get -y update && apt-get install -y apache2"
}
```

Azure CLI-Befehl:

```none
azure vm extension set <resource-group> <vm-name> CustomScript Microsoft.Azure.Extensions 2.0 --auto-upgrade-minor-version --public-config-path /public.json
```

**Beispiel 3** : einer öffentlichen Konfigurationsdatei wird verwendet, um die Skriptdatei URI angeben und eine geschützten Konfigurationsdatei wird verwendet, um die auszuführenden Befehl angeben.

Öffentliche Konfigurationsdatei:

```json
{
  "fileUris": ["https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"],
}
```

Geschützte Konfigurationsdatei:  

```json
{
  "commandToExecute": "./hello.sh <password>"
}
```

Azure CLI-Befehl:

```none
azure vm extension set <resource-group> <vm-name> CustomScript Microsoft.Azure.Extensions 2.0 --auto-upgrade-minor-version --public-config-path ./public.json --private-config-path ./protected.json
```

## <a name="resource-manager-template"></a>Ressourcenmanager-Vorlage

Zum Zeitpunkt der Bereitstellung virtuellen Computern mithilfe einer Vorlage Ressourcenmanager kann die Erweiterung Azure benutzerdefinierte Skripts ausgeführt werden. Fügen Sie hierzu ordnungsgemäß formatierten JSON zur Bereitstellungsvorlage ein.

### <a name="resource-manager-examples"></a>Beispiele für Ressourcenmanager

**Beispiel 1** : öffentliche Konfiguration.

```json
{
    "name": "scriptextensiondemo",
    "type": "extensions",
    "location": "[resourceGroup().location]",
    "apiVersion": "2015-06-15",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', parameters('scriptextensiondemoName'))]"
    ],
    "tags": {
        "displayName": "scriptextensiondemo"
    },
    "properties": {
        "publisher": "Microsoft.Azure.Extensions",
        "type": "CustomScript",
        "typeHandlerVersion": "2.0",
        "autoUpgradeMinorVersion": true,
      "settings": {
        "fileUris": [
          "https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"
        ],
        "commandToExecute": "sh hello.sh"
      }
    }
}
```

**Beispiel 2** : Ausführungsbefehl geschützten Konfiguration.

```json
{
  "name": "config-app",
  "type": "extensions",
  "location": "[resourceGroup().location]",
  "apiVersion": "2015-06-15",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh
      ]              
    },
    "protectedSettings": {
      "commandToExecute": "sh hello.sh <password>"
    }
  }
}
```

Finden Sie unter .net Core Musik Store Demo für ein vollständiges Beispiel - [Musik Store Demo](https://github.com/neilpeterson/nepeters-azure-templates/tree/master/dotnet-core-music-linux-vm-sql-db).

## <a name="troubleshooting"></a>Behandlung von Problemen

Wenn die benutzerdefinierte Skript-Erweiterung ausgeführt wird, ist das Skript erstellt oder heruntergeladen wurde, in einer ähnlich wie im folgenden Beispiel. Die Ausgabe des Befehls wird ebenfalls in dieses Verzeichnis in gespeichert `stdout` und `stderr` Datei.

```none
/var/lib/azure/custom-script/download/0/
```

Die Erweiterung Azure Skript erzeugt ein Protokoll, die hier gefunden werden kann.

```none
/var/log/azure/customscript/handler.log
```

Bundesstaat der Ausführung der benutzerdefinierte Skripts Erweiterung kann auch mit der Azure CLI abgerufen werden.

```none
azure vm extension get <resource-group> <vm-name>
```

Das Ergebnis sieht wie den folgenden Text aus:

```none
info:    Executing command vm extension get
+ Looking up the VM "scripttst001"
data:    Publisher                   Name                                      Version  State
data:    --------------------------  ----------------------------------------  -------  ---------
data:    Microsoft.Azure.Extensions  CustomScript                              2.0      Succeeded
data:    Microsoft.OSTCExtensions    Microsoft.Insights.VMDiagnosticsSettings  2.3      Succeeded
info:    vm extension get command OK
```

## <a name="next-steps"></a>Nächste Schritte

Informationen zu anderen virtuellen Computer Skript Erweiterungen finden Sie unter [Übersicht über das Skript-Erweiterung Azure Linux](./virtual-machines-linux-extensions-features.md).
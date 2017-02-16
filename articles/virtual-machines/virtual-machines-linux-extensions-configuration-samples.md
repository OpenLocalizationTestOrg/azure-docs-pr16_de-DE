<properties
   pageTitle="Beispiel für eine Konfiguration für Linux VM Erweiterungen | Microsoft Azure"
   description="Beispiel für eine Konfiguration zum Entwickeln von Vorlagen mit der Erweiterung für Linux virtuellen Computern"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="kundanap"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="09/13/2016"
   ms.author="kundanap"/>

# <a name="linux-vm-extension-configuration-samples"></a>Beispiele für Linux VM Erweiterung Konfiguration

> [AZURE.SELECTOR]
- [PowerShell - Vorlage](virtual-machines-windows-extensions-configuration-samples.md)
- [CLI - Vorlage](virtual-machines-linux-extensions-configuration-samples.md)

<br>

Dieser Artikel enthält Beispiel-Konfiguration für Azure-virtuellen Computer Erweiterungen für Linux virtuelle Computer konfigurieren.

Lernen Sie weitere Informationen zu den folgenden Erweiterungen klicken Sie hier: [Azure-Erweiterungen – Überblick über.](virtual-machines-windows-extensions-features.md)

Weitere Informationen zu authoring Erweiterung Vorlagen klicken Sie hier: [Erweiterung Vorlagen Authoring.](virtual-machines-windows-extensions-authoring-templates.md)

In diesem Artikel sind die erwarteten Konfigurationswerte für einige der Linux Extensions aufgelistet.

## <a name="sample-template-snippet-for-vm-extensions"></a>Beispiel für Vorlage Codeausschnitt für Erweiterungen virtueller Computer.
Die Vorlage Codeausschnitt für die Bereitstellung von Erweiterungen sieht wie folgt:

      {
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "name": "MyExtension",
      "apiVersion": "2015-05-01-preview",
      "location": "[parameters('location')]",
      "dependsOn": ["[concat('Microsoft.Compute/virtualMachines/',parameters('vmName'))]"],
      "properties":
      {
      "publisher": "Publisher Namespace",
      "type": "extension Name",
      "typeHandlerVersion": "extension version",
      "autoUpgradeMinorVersion":true,
      "settings": {
      // Extension specific configuration goes in here.
      }
      }
      }

## <a name="sample-template-snippet-for-vm-extensions-with-vm-scale-sets"></a>Beispiel für Vorlage Codeausschnitt für virtuellen Computer Erweiterungen mit Skalierung virtueller Computer.

          {
           "type":"Microsoft.Compute/virtualMachineScaleSets",
          ....
                 "extensionProfile":{
                 "extensions":[
                   {
                     "name":"extension Name",
                     "properties":{
                       "publisher":"Publisher Namespace",
                       "type":"extension Name",
                       "typeHandlerVersion":"extension version",
                       "autoUpgradeMinorVersion":true,
                       "settings":{
                       // Extension specific configuration goes in here.
                       }
                     }
                    }
                  }
                }

Vor der Bereitstellung der Erweiterung, überprüfen Sie die neueste Erweiterungsversion, und Ersetzen Sie den "TypeHandlerVersion" mit der aktuellen neueste Version aus.

Rest dieses Artikels bietet Stichprobe Konfigurationen für Linux virtueller Computer Erweiterungen an.

### <a name="cloudlink-securevm-agent"></a>CloudLink SecureVM-Agents
          {
            "publisher": "CloudLinkEMC.SecureVM",
            "type": "CloudLinkSecureVMLinuxAgent",
            "typeHandlerVersion": "4.0",
            "settings": {
              "CloudLinkCenter" : "specify valid IP/FQDN to CloudLinkCenter"
            }
          }

### <a name="customscript-extension-for-linux"></a>CustomScript-Erweiterung für Linux.
    {
        "publisher": " Microsoft.Azure.Extensions",
        "type": "CustomScript",
        "typeHandlerVersion": "2.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "fileUris": [
                "http: //Yourstorageaccount.blob.core.windows.net/customscriptfiles/start.ps1"
            ],
            "commandToExecute": "powershell.exe-ExecutionPolicyUnrestricted-Filestart.ps1"
        }
    }


### <a name="datadog-agent"></a>Datadog-Agent
        {
          "publisher": "Datadog.Agent",
          "type": "DatadogLinuxAgent",
          "typeHandlerVersion": "0.4",
          "settings": {
            "api_key" : "API Key from https://app.datadoghq.com/account/settings#api"
          }
        }

### <a name="chef-agent"></a>Verwaltungsangestellte Agent
        {
          "publisher": "Chef.Bootstrap.WindowsAzure",
          "type": "CentosChefClient|LinuxChefClient",
          "typeHandlerVersion": "1210.12",
          "settings": {
            "validation_key" : " Validation key",
            "client_rb" : "client_rb file",
            "runlist" : "Optional runlist"
          }
        }

### <a name="vm-access-extension-password-reset"></a>Virtueller Computer Access Erweiterung (das Zurücksetzen von Kennwörtern)
Aktualisierte Schema finden Sie in der [Dokumentation VMAccessForLinux](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess)

        {
          "publisher": "Microsoft.OSTCExtensions",
          "type": "VMAccessForLinux",
          "typeHandlerVersion": "1.2",
          "protectedSettings": {
            "username": "(required, string) the name of the user",
            "password": "(optional, string) the password of the user",
            "reset_ssh": "(optional, boolean) whether or not reset the ssh",
            "ssh_key": "(optional, string) the public key of the user, base64 encoded pem",
            "remove_user": "(optional, string) the user name to remove"
          }
        }

### <a name="os-patching"></a>OS Patch
Aktualisierte Schema finden Sie in der [Dokumentation OSPatching](https://github.com/Azure/azure-linux-extensions/tree/master/OSPatching)

        {
        "publisher": "Microsoft.OSTCExtensions",
        "type": "OSPatchingForLinux",
        "typeHandlerVersion": "2.9",
        "Settings": {
          "disabled": false,
          "stop": false,
          "rebootAfterPatch": "RebootIfNeed|Required|NotRequired|Auto",
          "category": "Important|ImportantAndRecommended",
          "installDuration": "<hr:min>",
          "oneoff": false,
          "intervalOfWeeks": "<number>",
          "dayOfWeek": "Sunday|Monday|Tuesday|Wednesday|Thursday|Friday|Saturday|Everyday",
          "startTime": "<hr:min>",
          "vmStatusTest": {
              "local": false,
              "idleTestScript": "<path_to_idletestscript>",
              "healthyTestScript": "<path_to_healthytestscript>"
          }
        }
        }

### <a name="docker-extension"></a>Docker-Erweiterung
Aktualisierte Schema finden Sie in der [Dokumentation von Docker Erweiterung](https://github.com/Azure/azure-docker-extension/blob/master/README.md#1-configuration-schema)

        {
          "publisher": "Microsoft.Azure.Extensions ",
          "type": "DockerExtension ",
          "typeHandlerVersion": "1.0",
          "Settings": {
            "docker":{
                "port": "2376",
                "options": ["-D", "--dns=8.8.8.8"]
            },
            "compose": {
                "cache" : {
                    "image" : "memcached",
                    "ports" : ["11211:11211"]
                },
                "blog": {
                    "image": "ghost",
                    "ports": ["80:2368"]
                }
            }
            }
        }

        ### Linux Diagnostics Extension
        {
        "storageAccountName": "storage account to receive data",
        "storageAccountKey": "key of the account",
        "perfCfg": [
        {
            "query": "SELECT PercentAvailableMemory, AvailableMemory, UsedMemory ,PercentUsedSwap FROM SCX_MemoryStatisticalInformation",
            "table": "LinuxMemory"
        }
        ],
        "fileCfg": [
        {
            "file": "/var/log/mysql.err",
            "table": "mysqlerr"
        }
        ]
        }

Ersetzen Sie in den Beispielen die Versionsnummer mit der neuesten Version Zahl ein.

Hier ist eine vollständige virtueller Computer-Vorlage zum Erstellen einer Linux VM mit der Erweiterung:

[Benutzerdefiniertes Skript Erweiterung einer Linux virtuellen Computers](https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/mongodb-on-ubuntu/azuredeploy.json/)

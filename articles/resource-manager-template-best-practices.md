<properties
    pageTitle="Bewährte Methoden Ressourcenmanager Vorlage | Microsoft Azure"
    description="Richtlinien zur Vereinfachung der Ressourcenmanager Azure-Vorlagen."
    services="azure-resource-manager"
    documentationCenter=""
    authors="tfitzmac"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="azure-resource-manager"
    ms.workload="multiple"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="tomfitz"/>

# <a name="best-practices-for-creating-azure-resource-manager-templates"></a>Bewährte Methoden zum Erstellen von Vorlagen Azure Ressourcenmanager

Die folgenden Richtlinien können Sie Ressourcenmanager Vorlagen erstellen, die zuverlässig und einfach zu verwendendes sind. Diese Richtlinien sind nur als Vorschläge, nicht absolut im Vordergrund vorgesehen. Ihr Szenario erfordern möglicherweise Variationen aus diesen Richtlinien.

## <a name="resource-names"></a>Ressourcennamen

Es gibt in der Regel drei Arten von Ressourcennamen, mit denen Sie arbeiten:

1. Ressourcennamen, die eindeutig sein müssen.
2. Ressourcennamen, die nicht eindeutig sein müssen, aber Sie möchten einen Namen zu geben, der im Kontext identifiziert werden.
3. Ressourcennamen, die generisch sein können.

Hilfe zum Festlegen einer Benennungskonvention finden Sie unter [Richtlinien für Infrastruktur](./virtual-machines/virtual-machines-windows-infrastructure-naming-guidelines.md). Informationen zu Ressourcen Namen Einschränkungen finden Sie unter [Benennungskonventionen für Azure Ressourcen empfohlen](./guidance/guidance-naming-conventions.md).

### <a name="unique-resource-names"></a>Eindeutige Ressourcennamen

Sie müssen einen eindeutigen Ressourcennamen für alle Ressourcenart angeben, die einen Access-Endpunkt Daten enthält. Einige gängige Typen, die einen eindeutigen Namen erfordern umfassen:

- Speicher-Konto
- Website
- SqlServer
- Key Tresor
- Redis cache
- Stapel-Konto
- Datenverkehr-manager
- Suchdienst
- HDInsight cluster

Darüber hinaus muss Speicher Kontonamen Kleinbuchstaben, 24 Zeichen oder weniger, und keine Bindestriche enthalten.

Wenn Sie einen Parameter für diese Ressourcennamen bereitstellen, müssen Sie einen eindeutigen Namen während der Bereitstellung von schätzen. Stattdessen können Sie eine Variable erstellen, die die [uniqueString()](resource-group-template-functions.md#uniquestring) -Funktion verwendet, um einen Namen zu generieren. Häufig, möchten Sie auch ein Präfix hinzufügen oder postfix zum Ergebnis **UniqueString** , damit Sie die Ressourcenart einfacher feststellen können, indem Sie den Namen. Beispielsweise generieren Sie einen eindeutigen Namen für ein Speicherkonto mit den folgenden Variablen:

    "variables": {
        "storageAccountName": "[concat(uniqueString(resourceGroup().id),'storage')]"
    }

Speicherkonten mit dem Präfix UniqueString nicht auf der gleichen Gestelle gruppierte abrufen.

### <a name="resource-names-for-identification"></a>Ressourcennamen für Kennung

Ressourcentypen, die Namen, aber Sie werden soll keine um Eindeutigkeit zu gewährleisten, einfach Geben Sie einen Namen, der den Kontext und die Ressource Typ bezeichnet. Möchten Sie einen aussagekräftigen Namen zu geben, der Sie es aus einer Liste von Ressourcennamen erkennen können. Wenn Sie den Namen der Ressource während Bereitstellungen unterschiedlich sein müssen, verwenden Sie für den Namen eines Parameters:

    "parameters": {
        "vmName": { 
            "type": "string",
            "defaultValue": "demoLinuxVM",
            "metadata": {
                "description": "The name of the VM to create."
            }
        }
    }

Wenn Sie nicht benötigen, um einen Namen während der Bereitstellung zu übergeben, verwenden Sie eine Variable: 

    "variables": {
        "vmName": "demoLinuxVM"
    }

Oder einen codierten Wert:

    {
      "type": "Microsoft.Compute/virtualMachines",
      "name": "demoLinuxVM",
      ...
    }

### <a name="generic-resource-names"></a>Generische Ressourcennamen

Für Ressourcentypen, die über eine andere Ressource weitgehend zugegriffen werden, können Sie einen allgemeinen Namen aus, der in der Vorlage programmiert ist. Beispielsweise möchten wahrscheinlich nicht Sie einen anpassbaren Namen Firewall-Regeln auf einem SQL Server anzugeben.

    {
        "type": "firewallrules",
        "name": "AllowAllWindowsAzureIps",
        ...
    }

## <a name="parameters"></a>Parameter

1. Minimieren Sie Parameter, wann immer möglich. Wenn Sie eine Variable oder ein Literal verwenden können, tun Sie dies. Geben Sie nur Parameter für ein:
 - Einstellungen aus, je nach Umgebung (z. B. Sku, Schriftgrad oder Kapazität) unterschiedlich sein soll.
 - Ressourcennamen, die Sie zur einfacheren Identifikation angeben möchten.
 - Werte, die Sie häufig zu anderen Aufgaben (z. B. Administratorbenutzernamen) verwenden.
 - Kennwörter (zum Beispiel Kennwörter)
 - Die Zahl oder einem Array von Werten zu verwenden, wenn mehrere Instanzen von einen Ressourcentyp zu erstellen.

1. Parameternamen sollten **CamelCasing**folgen.

1. Geben Sie eine Beschreibung der Metadaten für jeden Parameter aus.

        "parameters": {
            "storageAccountType": {
                "type": "string",
                "metadata": {
                    "description": "The type of the new storage account created to store the VM disks"
                }
            }
        }

1. Definieren von Standardwerten für Parameter (mit Ausnahme von Kennwörtern und SSH Tasten).

        "parameters": {
            "storageAccountType": {
                "type": "string",
                "defaultValue": "Standard_GRS",
                "metadata": {
                    "description": "The type of the new storage account created to store the VM disks"
                }
            }
        }

1. Verwenden Sie **Securestring** für alle Kennwörter und vertrauliche Informationen ein. 

        "parameters": {
            "secretValue": {
                "type": "securestring",
                "metadata": {
                    "description": "Value of the secret to store in the vault"
                }
            }
        }
 
1. Vermeiden Sie nach Möglichkeit ein Parameters verwenden, um den **Speicherort**anzugeben. Verwenden Sie stattdessen die Positionseigenschaft der Ressourcengruppe ein. Mithilfe des Ausdrucks **ResourceGroup () .location** für alle Ressourcen werden die Ressourcen in der Vorlage am selben Speicherort wie die Ressourcengruppe bereitgestellt.

        "resources": [
          {
              "name": "[variables('storageAccountName')]",
              "type": "Microsoft.Storage/storageAccounts",
              "apiVersion": "2016-01-01",
              "location": "[resourceGroup().location]",
              ...
          }
        ]
  
     Wenn nur eine eingeschränkte Anzahl von Speicherorte ein Ressourcentyp unterstützt wird, sollten Sie einen gültigen Speicherort direkt in der Vorlage angeben. Wenn Sie einen Speicherort Parameter verwenden müssen, freigeben Sie diesen Parameterwert so weit wie möglich mit Ressourcen, die vermutlich an derselben Stelle vorhanden sind. Dieser Ansatz minimiert Benutzer müssen Positionen für jede Ressourcenart bereitstellen.

1. Vermeiden Sie einen Parameter oder eine Variable für die API-Version für einen Ressourcentyp. Eigenschaften für die Ressource und Werten können je nach Versionsnummer variieren. IntelliSense in Code-Editoren ist nicht das richtige Schema feststellen, wenn die API-Version auf ein Parameter oder Variable festgelegt ist. In diesem Fall programmiert die API Version in der Vorlage.

## <a name="variables"></a>Variablen 

1. Verwenden Sie Variablen nach Werten, die Sie in einer Vorlage mehrmals verwenden müssen. Wenn Sie ein Wert nur einmal verwendet wird, wird ein Wert hartcodierte Ihrer Vorlage besser lesbar.

1. Sie können nicht die [Verweis](resource-group-template-functions.md#reference) -Funktion im Variablenabschnitt verwenden. Die Verweis-Funktion abgeleitet wird, dessen Wert aus den Status der Ressource Laufzeit, aber Variablen gelöst werden, während der anfänglichen Analyse der Vorlage. Erstellen Sie stattdessen Werte, die die **Verweis** -Funktion direkt im Abschnitt **Ressourcen** oder **ausgegeben** wird der Vorlage benötigen.

1. Einschließen von Variablen für Ressourcennamen, die die müssen eindeutig sein, da in [Ressourcennamen](#resource-names)angezeigt.

1. Sie können Variablen in komplexe Objekte gruppieren. Sie können einen Wert aus einer komplexen Objekts in das Format **variable.subentry**verweisen. Gruppieren von Variablen hilft Ihnen der zugehörige Variablen verfolgen und verbessern Sie die Lesbarkeit der Vorlage.

        "variables": {
            "storage": {
                "name": "[concat(uniqueString(resourceGroup().id),'storage')]",
                "type": "Standard_LRS"
            }
        },
        "resources": [
          {
              "type": "Microsoft.Storage/storageAccounts",
              "name": "[variables('storage').name]",
              "apiVersion": "2016-01-01",
              "location": "[resourceGroup().location]",
              "sku": {
                  "name": "[variables('storage').type]"
              },
              ...
          }
        ]
 
     > [AZURE.NOTE] Ein komplexes Objekt kann keinen Ausdruck enthalten, der einen Wert aus einer komplexen Objekts verweist. Definieren Sie eigenständige Variable zu diesem Zweck.

     Erweiterte Beispiele für komplexe Objekte als Variablen verwenden finden Sie unter [Freigabe-Zustand in Azure Ressourcenmanager Vorlagen](best-practices-resource-manager-state.md).

## <a name="resources"></a>Ressourcen

1. Geben Sie **Kommentare** für jede Ressource in der Vorlage können Sie zu anderen Mitwirkenden Zweck der Ressource ein.

        "resources": [
          {
              "name": "[variables('storageAccountName')]",
              "type": "Microsoft.Storage/storageAccounts",
              "apiVersion": "2016-01-01",
              "location": "[resourceGroup().location]",
              "comments": "This storage account is used to store the VM disks",
              ...
          }
        ]

1. Verwenden Sie Kategorien, um die Metadaten zu Ressourcen hinzufügen, mit denen Sie weitere Informationen zu Ressourcen hinzufügen. Beispielsweise können Sie die Metadaten, die einer Ressource für Abrechnung Detail Zwecke hinzufügen. Weitere Informationen finden Sie unter [Verwenden von Kategorien, um Ihre Azure Ressourcen zu organisieren](resource-group-using-tags.md).

1. Wenn Sie einen **öffentlichen Endpunkt** in der Vorlage (beispielsweise eine BLOB-Speicher öffentlichen Endpunkt), verwenden Sie **keine** Namespace. Verwenden Sie die **Verweis** -Funktion, um den Namespace dynamisch abzurufen. Dieser Ansatz ermöglicht Ihnen, die Vorlage auf anderen öffentlichen Namespace-Umgebungen bereitzustellen, ohne manuell den Endpunkt in der Vorlage ändern. Legen Sie die ApiVersion auf dieselbe Version, die Sie in der Vorlage für die StorageAccount verwenden.

        "osDisk": {
            "name": "osdisk",
            "vhd": {
                "uri": "[concat(reference(concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2016-01-01').primaryEndpoints.blob, variables('vmStorageAccountContainerName'), '/',variables('OSDiskName'),'.vhd')]"
            }
        }

     Wenn das Speicherkonto in derselben Vorlage bereitgestellt wird, müssen Sie nicht den Anbieternamespace angeben, wenn die Ressource zu verweisen. Die vereinfachte Syntax lautet:
     
        "osDisk": {
            "name": "osdisk",
            "vhd": {
                "uri": "[concat(reference(variables('storageAccountName'), '2016-01-01').primaryEndpoints.blob, variables('vmStorageAccountContainerName'), '/',variables('OSDiskName'),'.vhd')]"
            }
        }

     Wenn Sie andere Werte in der Vorlage mit einem öffentlichen Namespace konfiguriert haben, ändern Sie diese Werte entsprechend die gleichen Verweis-Funktion. Angenommen, die StorageUri Eigenschaft der DiagnosticsProfile virtuellen Computers.

        "diagnosticsProfile": {
            "bootDiagnostics": {
                "enabled": "true",
                "storageUri": "[reference(concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2016-01-01').primaryEndpoints.blob]"
            }
        }
 
     Sie können auch den **Bezug** eines vorhandenen Kontos von Speicher in einer anderen Ressourcengruppe.


        "osDisk": {
            "name": "osdisk", 
            "vhd": {
                "uri":"[concat(reference(resourceId(parameters('existingResourceGroup'), 'Microsoft.Storage/storageAccounts/', parameters('existingStorageAccountName')), '2016-01-01').primaryEndpoints.blob,  variables('vmStorageAccountContainerName'), '/', variables('OSDiskName'),'.vhd')]"
            }
        }

1. Zuweisen eines virtuellen Computers nur, wenn für eine Anwendung erforderlichen PublicIPAddresses hinzu. Verwenden Sie eine Verbindung für das Debuggen, Management oder administrative Zwecke, InboundNatRules, VirtualNetworkGateways oder eine Jumpbox aus.

     Weitere Informationen zum Herstellen einer Verbindung mit virtuellen Computern finden Sie unter:
     - [Ausführen von virtuellen Computern für eine mehrstufige Architektur auf Azure](./guidance/guidance-compute-n-tier-vm.md)
     - [Einrichten von WinRM Zugriff für virtuellen Computern in Azure Ressourcenmanager](./virtual-machines/virtual-machines-windows-winrm.md)
     - [Den externen Zugriff auf Ihre virtuellen Computer mit dem Azure-portal](./virtual-machines/virtual-machines-windows-nsg-quickstart-portal.md)
     - [Den externen Zugriff auf Ihre virtuellen Computer mithilfe der PowerShell](./virtual-machines/virtual-machines-windows-nsg-quickstart-powershell.md)
     - [Öffnen von Ports und Endpunkte](./virtual-machines/virtual-machines-linux-nsg-quickstart.md)

1. Die Eigenschaft **DomainNameLabel** für PublicIPAddresses muss eindeutig sein. DomainNameLabel ist erforderlich, um zwischen 3 und 63 Zeichen lang sein und führen Sie die Regeln, die von diesem regulären Ausdruck angegebenen `^[a-z][a-z0-9-]{1,61}[a-z0-9]$`. Wie die UniqueString-Funktion eine Zeichenfolge, die 13 Zeichen lang ist generiert, ist der DnsPrefixString-Parameter auf nicht mehr als 50 Zeichen beschränkt.

        "parameters": {
            "dnsPrefixString": {
                "type": "string",
                "maxLength": 50,
                "metadata": {
                    "description": "DNS Label for the Public IP. Must be lowercase. It should match with the following regular expression: ^[a-z][a-z0-9-]{1,61}[a-z0-9]$ or it will raise an error."
                }
            }
        },
        "variables": {
            "dnsPrefix": "[concat(parameters('dnsPrefixString'),uniquestring(resourceGroup().id))]"
        }

1. Beim Hinzufügen eines Kennworts zu einer **CustomScriptExtension**führen Sie die Eigenschaft **CommandToExecute** in ProtectedSettings.

        "properties": {
            "publisher": "Microsoft.Azure.Extensions",
            "type": "CustomScript",
            "typeHandlerVersion": "2.0",
            "autoUpgradeMinorVersion": true,
            "settings": {
                "fileUris": [
                    "[concat(variables('template').assets, '/lamp-app/install_lamp.sh')]"
                ]
            },
            "protectedSettings": {
                "commandToExecute": "[concat('sh install_lamp.sh ', parameters('mySqlPassword'))]"
            }
        }

     > [AZURE.NOTE] Um sicherzustellen, dass Kennwörter verschlüsselt werden, wenn es als Parameter zu VirtualMachines/Erweiterungen übermittelt, verwenden Sie die Eigenschaft ProtectedSettings die relevanten Erweiterungen.

## <a name="outputs"></a>Ausgaben

Wenn Sie eine Vorlage **PublicIPAddresses**erstellt, sollten sie einen Abschnitt **gibt** aufweisen, der Details die IP-Adresse und den vollqualifizierten Domänennamen zurückgibt. Diese Ausgabewerte aktivieren Sie diese Details einfach nach der Bereitstellung abrufen. Wenn die Ressource verwiesen wird, verwenden Sie die API-Version, die verwendet wurde, um sie zu erstellen. 

```
"outputs": {
    "fqdn": {
        "value": "[reference(resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName')), '2016-07-01').dnsSettings.fqdn]",
        "type": "string"
    },
    "ipaddress": {
        "value": "[reference(resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName')), '2016-07-01').ipAddress]",
        "type": "string"
    }
}
```

## <a name="single-template-or-nested-templates"></a>Dokumentvorlage oder verschachtelte Vorlagen

Wenn Ihre Lösung bereitstellen, können Sie entweder eine einzelne Vorlage oder eine Vorlage Hauptfenster mit mehreren geschachtelte Vorlagen. Verschachtelte Vorlagen gelten für komplexere Szenarios. Verschachtelte Vorlagen enthalten die folgenden Vorteile:

1. Lösung können in den Komponenten gliedern werden.
2. Verschachtelte Vorlagen mit anderen Hauptfenster Vorlagen können wieder verwenden.

Wenn Sie den Vorlagenentwurf in mehreren geschachtelte Vorlagen Gliedern möchten, können die folgenden Richtlinien Standardisierung das Design. Diesen Richtlinien basieren auf der Dokumentation [Muster zum Entwerfen von Azure Ressourcenmanager Vorlagen](best-practices-resource-manager-design-templates.md) . Der empfohlene Entwurf umfasst die folgenden Vorlagen:

+ **Primär-Vorlage** (azuredeploy.json). Für den Eingabeparameter verwendet.
+ **Vorlage für freigegebene Ressourcen**. Bereitstellt, die freigegebenen Ressourcen, die alle anderen Ressourcen (z. B. virtuelles Netzwerk Verfügbarkeit Sätze) verwenden. Der Ausdruck DependsOn erzwingt, dass diese Vorlage vor anderen Vorlagen bereitgestellt wird.
+ **Optional Ressourcenvorlage**. Bedingt bereitstellt Ressourcen auf Grundlage eines Parameters (beispielsweise einer Jumpbox)
+ **Mitglied Ressourcen Vorlagen**. Jedes Typs Instanz innerhalb einer Schicht Anwendung verfügt über eine eigene Konfiguration. Innerhalb einer Kategorie und andere Instanz Typen definiert werden können (wie erste Instanz erstellt einen Cluster, zusätzliche Instanzen werden hinzugefügt zu dem vorhandenen Cluster). Jeder Instanz verfügt über eine eigene Bereitstellungsvorlage.
+ **Skripts**. Stark wieder verwendbaren Skripts gelten für jede Instanz ein (beispielsweise Initialisierung und Format zusätzliche Datenträger). Benutzerdefinierte Skripts werden erstellt, für die Anpassung von bestimmten Zweck pro Instanztyp unterscheiden.

![verschachtelte Vorlage](./media/resource-manager-template-best-practices/nestedTemplateDesign.png)

Weitere Informationen finden Sie unter [Verwendung von verknüpften Vorlagen Azure Ressourcenmanager](resource-group-linked-templates.md).

## <a name="conditionally-link-to-nested-template"></a>Verschachtelte Vorlage bedingt verknüpfen.

Sie können bedingte link zur geschachtelte Vorlagen mithilfe eines Parameters, das Teil des URIS für die Vorlage wird.

    "parameters": {
        "newOrExisting": {
            "type": "String",
            "allowedValues": [
                "new",
                "existing"
            ]
        }
    },
    "variables": {
        "templatelink": "[concat('https://raw.githubusercontent.com/Contoso/Templates/master/',parameters('newOrExisting'),'StorageAccount.json')]"
    },
    "resources": [
        {
            "apiVersion": "2015-01-01",
            "name": "nestedTemplate",
            "type": "Microsoft.Resources/deployments",
            "properties": {
                "mode": "incremental",
                "templateLink": {
                    "uri": "[variables('templatelink')]",
                    "contentVersion": "1.0.0.0"
                },
                "parameters": {
                }
            }
        }
    ]

## <a name="template-format"></a>Vorlagenformat

1. Es empfiehlt sich Ihre Vorlage durch eine Bestätigung JSON Entfernen überflüssiger Kommas, Klammer, Klammern, die möglicherweise einen Fehler bei der Bereitstellung verursachen übergeben. Versuchen Sie es [JSONlint](http://jsonlint.com/) oder ein Linter Paket für Ihre bevorzugten Bearbeitungssprache-Umgebung (Visual Studio-Code, Atom, Sublime Text, Visual Studio usw.).
1. Es ist auch eine gute Idee, Ihre JSON zur besseren Lesbarkeit zu formatieren. Sie können JSON-Pakets Formatierungsprogramm für lokale Editor verwenden. Formatieren Sie das Dokument mit **STRG + K, STRG + D**, in Visual Studio. Verwenden Sie im Vergleich mit einer Code **Alt + Umschalt + F**. Wenn der lokale Editor Formatieren des Dokuments nicht, können Sie eine [online Formatierungsprogramm](https://www.bing.com/search?q=json+formatter)verwenden.

## <a name="next-steps"></a>Nächste Schritte

1. Leitfaden für die an der Architektur Ihre Lösung für virtuellen Computern finden Sie unter [Ausführen eines Windows virtuellen Computers auf Azure](./guidance/guidance-compute-single-vm.md) und [eines Linux virtuellen Computers auf Azure ausgeführt](./guidance/guidance-compute-single-vm-linux.md).
2. Leitfaden für die Einstellung Speicher-Konto finden Sie unter [Microsoft Azure-Speicher Leistung und Skalierbarkeit Checkliste](./storage/storage-performance-checklist.md).
3. Hilfe mit virtuelle Netzwerke finden Sie unter [Networking Infrastructure Richtlinien](./virtual-machines/virtual-machines-windows-infrastructure-networking-guidelines.md).

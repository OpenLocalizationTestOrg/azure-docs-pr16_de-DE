<properties
   pageTitle="Automatisierung Ressourcen OMS Lösungen | Microsoft Azure"
   description="Lösungen in OMS werden in der Regel Runbooks in Azure Automatisierung Prozesse wie sammeln und Verarbeiten von Überwachung Daten automatisieren sind.  Dieser Artikel beschreibt, wie in einer Lösung Runbooks und deren zugehörige Ressourcen aufgenommen werden sollen."
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/19/2016"
   ms.author="bwren" />

# <a name="automation-resources-in-oms-solutions-preview"></a>Automatisierung Ressourcen OMS Lösungen (Preview)

>[AZURE.NOTE]Dies ist über der vorläufigen Dokumentation zum Erstellen von Lösungen Management in OMS die derzeit in der Vorschau sind. Alle nachfolgend beschriebenen Schema unterliegt ändern.   

[Projektmanagement-Lösungen in OMS](operations-management-suite-solutions.md) wird in der Regel Runbooks in Azure Automatisierung Prozesse wie sammeln und Verarbeiten von Überwachung Daten automatisieren sind.  Klicken Sie neben Runbooks Automatisierung Konten wie Variablen und Terminpläne, die die Runbooks verwendet, in die Lösung unterstützen enthält.  Dieser Artikel beschreibt, wie in einer Lösung Runbooks und deren zugehörige Ressourcen aufgenommen werden sollen.

>[AZURE.NOTE]In den Beispielen in diesem Artikel verwenden, Parameter und Variablen, die sind entweder erforderlich oder gemeinsam Management-Lösungen und beschrieben [Erstellen Management Solutions in Vorgänge Management Suite (OMS)](operations-management-suite-solutions-creating.md) 


## <a name="prerequisites"></a>Erforderliche Komponenten
In diesem Artikel wird vorausgesetzt, dass Sie bereits [eine Lösung erstellen](operations-management-suite-solutions-creating.md) und die Struktur einer Lösung Datei wie vertraut sind.

## <a name="samples"></a>Beispiele
Sie können Ressourcenmanager Beispielvorlagen für Automatisierung Ressourcen aus den [Schnellstart Vorlagen in GitHub](https://github.com/azureautomation/automation-packs/tree/master/101-sample-automation-resource-templates)erhalten.

## <a name="automation-account"></a>Automatisierung-Konto
Alle Ressourcen in Azure Automatisierung sind in einem [Automatisierung Konto](../automation/automation-security-overview.md#automation-account-overview)enthalten.  Beschriebenen in [OMS Workspace und Automatisierung Konto](operations-management-suite-solutions-creating.md#oms-workspace-and-automation-account) das Konto Automatisierung ist kein Bestandteil der Management-Lösung, aber muss vorhanden sein, bevor Sie die Lösung installiert ist.  Wenn sie nicht verfügbar ist, tritt die Lösung installieren.

Der Name ihres Kontos Automatisierung ist den Namen jeder Ressource Automatisierung.  Dies ist die Lösung mit dem Parameter **Kontoname** wie im folgenden Beispiel einer Ressource Runbooks durchgeführt.
    
    "name": "[concat(parameters('accountName'), '/MyRunbook'))]"


## <a name="runbooks"></a>Runbooks
[Azure Automatisierung Runbooks](../automation/automation-runbook-types.md) Ressourcen haben einen Typ von **Microsoft.Automation/automationAccounts/runbooks** und die Eigenschaften in der folgenden Tabelle.

| Eigenschaft | Beschreibung |
|:--|:--|
| runbookType | Gibt die Typen von des Runbooks an. <br><br> Skript - PowerShell-Skript <br>PowerShell - PowerShell-workflow <br> GraphPowerShell - grafisch PowerShell Skript Runbooks <br> GraphPowerShellWorkflow - grafisch PowerShell Workflow Runbooks   |
| logProgress | Gibt an, ob [Einträge den Fortschritt](../automation/automation-runbook-output-and-messages.md) für die Runbooks generiert werden soll. |
| logVerbose  | Gibt an, ob [ausführliche Datensätze](../automation/automation-runbook-output-and-messages.md) für des Runbooks generiert werden soll. |
| Beschreibung | Optionale Beschreibung für die Runbooks. |
| publishContentLink | Gibt den Inhalt des Runbooks an. <br><br>URI - Uri auf den Inhalt des Runbooks.  Dies wird eine Datei ps1 für PowerShell und Skript Runbooks und eine Datei exportierten grafisch Runbooks für ein Diagramm Runbooks sein.  <br> Version - Version des Runbooks für Ihre eigenen Verlauf. |

Ein Beispiel für eine Ressource Runbooks ist unter.  In diesem Fall ruft des Runbooks aus dem [Katalog der PowerShell](https://www.powershellgallery.com)ab.

    "name": "[concat(parameters('accountName'), '/Start-AzureV2VMs'))]",
    "type": "Microsoft.Automation/automationAccounts/runbooks",
    "apiVersion": "[variables('AutomationApiVersion')]",
    "location": "[parameters('regionId')]",
    "dependsOn": [
        "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('startVmsRunbookName'))]"
    ],
    "tags": { },
    "properties": {
        "runbookType": "PowerShell",
        "logProgress": "true",
        "logVerbose": "true",
        "description": "",
        "publishContentLink": {
            "uri": "https://www.powershellgallery.com/api/v2/items/psscript/package/Update-ModulesInAutomationToLatestVersion/1.0/Start-AzureV2VMs.ps1",
            "version": "1.0"
        }
    }

### <a name="starting-a-runbook"></a>Starten einer Runbooks
Es gibt zwei Methoden, um eine Runbooks in eine Lösung zu starten.

- Um des Runbooks starten, wenn die Lösung installiert ist, erstellen Sie eine [Position Ressource](#automation-jobs) , wie unten beschrieben.
- Zum Starten des Runbooks auf einen Zeitplan erstellen Sie eine [Ressource planen](#schedules) , wie unten beschrieben. 


## <a name="automation-jobs"></a>Automatisierung Aufträge
Um eine Runbooks starten, wenn die Management-Lösung installiert ist, erstellen Sie eine Ressource **Position** ein.  Position Ressourcen haben einen Typ von **Microsoft.Automation/automationAccounts/jobs** und die Eigenschaften in der folgenden Tabelle.

| Eigenschaft | Beschreibung |
|:--|:--|
| Runbooks    | Einzelne **Namen** Entität mit dem Namen der des Runbooks.  |
| Parameter | Entität für jeden Parameter, des Runbooks erforderlich machen. |


Der Auftrag enthält den Namen des Runbooks und beliebige Parameterwerte des Runbooks gesendet werden.  Das Projekt muss [hängen davon ab,](operations-management-suite-solutions-creating.md#resources) bevor Sie den Auftrag des Runbooks, der es seit des Runbooks gestartet wird erstellt werden muss.  Sie erstellen auch Abhängigkeiten für andere Einzelvorgänge Runbooks, die vor der aktuellen Zeile abgeschlossen sein soll.

Der Name einer Ressource Auftrag muss eine GUID enthalten in der Regel durch einen Parameter zugeordnet ist.  Weitere Informationen zu GUID-Parameter in [Erstellen Lösungen in Vorgänge Management Suite (OMS)](operations-management-suite-solutions-creating.md#parameters).  

Es folgt ein Beispiel für eine Position Ressource, die eine Runbooks geöffnet wird, wenn die Management-Lösung installiert ist.  Eine, die andere Runbooks müssen, bevor diese ausgeführt werden gestartet wird eine, damit sie die Einzelvorgänge für diese Runbooks abhängig ist.  Durch Parameter und Variablen statt direkt angegeben werden die Details für das Projekt bereitgestellt.

    {
        "name": "[concat(parameters('accountName'), '/', parameters('startVmsRunbookGuid'))]",
        "type": "Microsoft.Automation/automationAccounts/jobs",
        "apiVersion": "[variables('AutomationApiVersion')]",
        "location": "[parameters('regionId')]",
        "dependsOn": [
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('startVmsRunbookName'))]",
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/jobs/', parameters('initialPrepRunbookGuid'))]"
        ],
        "tags": { },
        "properties": {
            "runbook": {
                "name": "[variables('startVmsRunbookName')]"
            },
            "parameters": {
                "AzureConnectionAssetName": "[variables('AzureConnectionAssetName')]",
                "ResourceGroupName": "[variables('ResourceGroupName')]"
            }
        }
    }


## <a name="certificates"></a>Zertifikate
[Azure Automatisierung Zertifikate](../automation/automation-certificates.md) haben einen Typ von **Microsoft.Automation/automationAccounts/certificates** und die Eigenschaften in der folgenden Tabelle.

| Eigenschaft | Beschreibung |
|:--|:--|
| base64Value   | 64-Basiswert für das Zertifikat. |
| Fingerabdruck    | Fingerabdruck für das Zertifikat. |

Ein Beispiel für eine Ressource Zertifikat ist unter.

    "name": "[concat(parameters('accountName'), '/MyCertificate')]",
    "type": "certificates",
    "apiVersion": "2015-01-01-preview",
    "location": "[parameters('regionId')]",
    "tags": {},
    "dependsOn": [
    ],
    "properties": {
        "base64Value": "MIIC1jCCAA8gAwIAVgIYJY4wXCXH/YAHMtALh7qEFzANAgkqhkiG5w0AAQUFGDAUMRIwEBYDVQQDEwlsA3NhAGevc3QqHhcNMTZwNjI5MHQxMjAsWhcNOjEwNjI5NKWwMDAwWjAURIwEAYDVQQBEwlsA2NhAGhvc3QwggEiMA0GCSqGSIA3DQEAAQUAA4IADwAwggEKAoIAAQDIyzv2A0RUg1/AAryI9W1DGAHAqqGdlFfTkUSDfv+hEZTAwKv0p8daqY6GroT8Du7ctQmrxJsy8JxIpDWxUaWwXtvv1kR9eG9Vs5dw8gqhjtOwgXvkOcFdKdQwA82PkcXoHlo+NlAiiPPgmHSELGvcL1uOgl3v+UFiiD1ro4qYqR0ITNhSlq5v2QJIPnka8FshFyPHhVtjtKfQkc9G/xDePW8dHwAhfi8VYRmVMmJAEOLCAJzRjnsgAfznP8CZ/QUczPF8LuTZ/WA/RaK1/Arj6VAo1VwHFY4AZXAolz7xs2sTuHplVO7FL8X58UvF7nlxq48W1Vu0l8oDi2HjvAgMAAAGjJDAiMAsGA1UdDwREAwIEsDATAgNVHSUEDDAKAggrAgEFNQcDATANAgkqhkiG9w0AAQUFAAOCAQEAk8ak2A5Ug4Iay2v0uXAk95qdAthJQN5qIVA13Qay8p4MG/S5+aXOVz4uMXGt18QjGds1A7Q8KDV4Slnwn95sVgA5EP7akvoGXhgAp8Dm90sac3+aSG4fo1V7Y/FYgAgpEy4C/5mKFD1ATeyyhy3PmF0+ZQRJ7aLDPAXioh98LrzMZr1ijzlAAKfJxzwZhpJamAwjZCYqiNZ54r4C4wA4QgX9sVfQKd5e/gQnUM8gTQIjQ8G2973jqxaVNw9lZnVKW3C8/QyLit20pNoqX2qQedwsqg3WCUcPRUUqZ4NpQeHL/AvKIrt158zAfU903yElAEm2Zr3oOUR4WfYQ==",
        "thumbprint": "F485CBE5569F7A5019CB68D7G6D987AC85124B4C"
    }

## <a name="credentials"></a>Anmeldeinformationen
[Azure Automatisierung Anmeldeinformationen](../automation/automation-credentials.md) verfügen über einen Typ von **Microsoft.Automation/automationAccounts/credentials** und verfügen über die Eigenschaften in der folgenden Tabelle.

| Eigenschaft | Beschreibung |
|:--|:--|
| Benutzername   | Der Benutzername für die Anmeldeinformationen. |
| Kennwort   | Kennwort für die Anmeldeinformationen. |

Ein Beispiel für eine Ressource Anmeldeinformationen ist unter.

    "name": "[concat(parameters('accountName'), '/', variables('credentialName'))]",
    "type": "Microsoft.Automation/automationAccounts/runbooks/credentials",
    "apiVersion": "[variables('AutomationApiVersion')]",
    "location": "[parameters('regionId')]",
    "tags": { },
    "dependsOn": [
    ],
    "properties": {
        "userName": "User01",
        "password": "password"
    }


## <a name="schedules"></a>Zeitpläne
[Azure Automatisierung Zeitpläne](../automation/automation-schedules.md) haben einen Typ von **Microsoft.Automation/automationAccounts/schedules** und die Eigenschaften in der folgenden Tabelle.

| Eigenschaft | Beschreibung |
|:--|:--|
| Beschreibung | Optionale Beschreibung für den Zeitplan. |
| Startzeit   | Gibt die Startzeit für einen Zeitplan als DateTime-Objekt. Eine Zeichenfolge kann bereitgestellt werden, wenn er in gültiger DateTime-Wert konvertiert werden kann. |
| isEnabled   | Gibt an, ob der Terminplan aktiviert ist. |
| Intervall    | Die Art des Intervalls für den Zeitplan.<br><br>Tag<br>Stunde |
| Häufigkeit   | Häufigkeit, mit der Anzahl von Tagen oder Stunden des Zeitplans ausgelöst werden soll. |

Ein Beispiel für eine Ressource Terminplan ist unter.

    "name": "[concat(parameters('accountName'), '/', variables('variableName'))]",
    "type": "microsoft.automation/automationAccounts/schedules",
    "apiVersion": "[variables('AutomationApiVersion')]",
    "tags": { },
    "dependsOn": [
    ],
    "properties": {
        "description": "Schedule for StartByResourceGroup runbook",
        "startTime": "10/30/2016 12:00:00",
        "isEnabled": "true",
        "interval": "day",
        "frequency": "1"
    }


## <a name="variables"></a>Variablen
[Azure Automatisierung Variablen](../automation/automation-variables.md) haben einen Typ von **Microsoft.Automation/automationAccounts/variables** und die Eigenschaften in der folgenden Tabelle.

| Eigenschaft | Beschreibung |
|:--|:--|
| Beschreibung | Optionale Beschreibung für die Variable. |
| isEncrypted | Gibt an, ob die Variable verschlüsselt werden soll. |
| Typ        | Der Datentyp für die Variable. |
| Wert       | Der Wert für die Variable. |

Ein Beispiel für eine Variable Ressource ist unter.

    "name": "[concat(parameters('accountName'), '/', variables('StartTargetResourceGroupsAssetName')) ]",
    "type": "microsoft.automation/automationAccounts/variables",
    "apiVersion": "[variables('AutomationApiVersion')]",
    "tags": { },
    "dependsOn": [
    ],
    "properties": {
        "description": "Description for the variable.",
        "isEncrypted": "true",
        "type": "string",
        "value": "'This is a string value.'"
    }


## <a name="modules"></a>Module
Ihre Management-Lösung muss nicht [globalen Module](../automation/automation-integration-modules.md) von Ihrem Runbooks verwendet werden, da sie immer verfügbar sind, definieren.  Sie müssen eine Ressource für alle anderen Modul untersuchten Ihrer Runbooks einbeziehen möchten, und des Runbooks sollte abhängen, die Ressource Modul, um sicherzustellen, dass sie vor dem des Runbooks erstellt wurde.

[Integrationsmodule](../automation/automation-integration-modules.md) verfügen über einen Typ von **Microsoft.Automation/automationAccounts/modules** und verfügen über die Eigenschaften in der folgenden Tabelle.

| Eigenschaft | Beschreibung |
|:--|:--|
| contentLink | Gibt den Inhalt des Moduls. <br><br>URI - Uri auf den Inhalt des Runbooks.  Dies wird eine Datei ps1 für PowerShell und Skript Runbooks und eine Datei exportierten grafisch Runbooks für ein Diagramm Runbooks sein.  <br> Version - Version des Runbooks für Ihre eigenen Verlauf. |

Ein Beispiel für eine Ressource Modul ist unter.

    {       
        "name": "[concat(parameters('accountName'), '/', variables('OMSIngestionModuleName'))]",
        "type": "Microsoft.Automation/automationAccounts/modules",
        "apiVersion": "[variables('AutomationApiVersion')]",
        "dependsOn": [
        ],
        "properties": {
            "contentLink": {
                "uri": "https://devopsgallerystorage.blob.core.windows.net/packages/omsingestionapi.1.3.0.nupkg"
            }
        }
    }

### <a name="updating-modules"></a>Module aktualisieren
Wenn Sie eine Lösung, die eine Runbooks enthält, die einen Zeitplan verwendet aktualisieren und die neue Version Ihrer Lösung ein neues Modul untersuchten Diese Runbooks bietet, möglicherweise des Runbooks die alte Version des Moduls verwenden.  Beziehen Sie die folgenden Runbooks in Ihre Lösung, und erstellen ein Projekt, um sie auszuführen, bevor Sie eine beliebige andere Runbooks.  Dadurch wird sichergestellt, dass alle Module als aktualisiert werden müssen, bevor die Runbooks geladen sind.

- [Update-ModulesinAutomationToLatestVersion](https://www.powershellgallery.com/packages/Update-ModulesInAutomationToLatestVersion/1.03/DisplayScript) wird sicherstellen, dass alle Module von Runbooks in Ihre Lösung verwendet die neueste Version.  
- [ReRegisterAutomationSchedule-MS-Management](https://www.powershellgallery.com/packages/ReRegisterAutomationSchedule-MS-Mgmt/1.0/DisplayScript) wird registrieren aller Ressourcen Terminplan um sicherzustellen, dass die Runbooks können mit verwenden die neuesten Modulen verknüpft.


Es folgt ein Beispiel für die erforderlichen Elemente einer Lösung zur Unterstützung von des Modul aktualisieren.
    
    "parameters": {
        "ModuleImportGuid": {
            "type": "string",
            "metadata": {
                "control": "guid"
            }
        },
        "ReRegisterRunbookGuid": {
            "type": "string",
            "metadata": {
                "control": "guid"
            }
        }
    },

    "variables": {
        "ModuleImportRunbookName": "Update-ModulesInAutomationToLatestVersion",
        "ModuleImportRunbookUri": "https://www.powershellgallery.com/packages/Update-ModulesInAutomationToLatestVersion/1.03/Content/Update-ModulesInAutomationToLatestVersion.ps1",
        "ModuleImportRunbookDescription": "Imports modules and also updates all Azure dependent modules that are in the same Automation Account.",
        "ReRegisterSchedulesRunbookName": "Update-ModulesInAutomationToLatestVersion",
        "ReRegisterSchedulesRunbookUri": "https://www.powershellgallery.com/packages/ReRegisterAutomationSchedule-MS-Mgmt/1.0/Content/ReRegisterAutomationSchedule-MS-Mgmt.ps1",
        "ReRegisterSchedulesRunbookDescription": "Reregisters schedules to ensure that they use latest modules."
    },

    "resources": [
        {
            "name": "[concat(parameters('accountName'), '/', variables('ModuleImportRunbookName'))]",
            "type": "Microsoft.Automation/automationAccounts/runbooks",
            "apiVersion": "[variables('AutomationApiVersion')]",
            "dependsOn": [
            ],
            "location": "[parameters('regionId')]",
            "tags": { },
            "properties": {
                "runbookType": "PowerShell",
                "logProgress": "true",
                "logVerbose": "true",
                "description": "[variables('ModuleImportRunbookDescription')]",
                "publishContentLink": {
                    "uri": "[variables('ModuleImportRunbookUri')]",
                    "version": "1.0.0.0"
                }
            }
        },
        {
            "name": "[concat(parameters('accountName'), '/', parameters('ModuleImportGuid'))]",
            "type": "Microsoft.Automation/automationAccounts/jobs",
            "apiVersion": "[variables('AutomationApiVersion')]",
            "location": "[parameters('regionId')]",
            "dependsOn": [
                "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('ModuleImportRunbookName'))]"
            ],
            "tags": { },
            "properties": {
                "runbook": {
                    "name": "[variables('ModuleImportRunbookName')]"
                },
                "parameters": {
                    "ResourceGroupName": "[resourceGroup().name]",
                    "AutomationAccountName": "[parameters('accountName')]",
                    "NewModuleName": "AzureRM.Insights"
                }
            }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('ReRegisterSchedulesRunbookName'))]",
          "type": "Microsoft.Automation/automationAccounts/runbooks",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "dependsOn": [
          ],
          "location": "[parameters('regionId')]",
          "tags": { },
          "properties": {
            "runbookType": "PowerShell",
            "logProgress": "true",
            "logVerbose": "true",
            "description": "[variables('ReRegisterSchedulesRunbookDescription')]",
            "publishContentLink": {
              "uri": "[variables('ReRegisterSchedulesRunbookUri')]",
              "version": "1.0.0.0"
            }
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', parameters('reRegisterSchedulesGuid'))]",
          "type": "Microsoft.Automation/automationAccounts/jobs",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "location": "[parameters('regionId')]",
          "dependsOn": [
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('ReRegisterSchedulesRunbookName'))]"
          ],
          "tags": { },
          "properties": {
            "runbook": {
              "name": "[variables('ReRegisterSchedulesRunbookName')]"
            },
            "parameters": {
              "targetSubscriptionId": "[subscription().subscriptionId]",
              "resourcegroup": "[resourceGroup().name]",
              "automationaccount": "[parameters('accountName')]"
            }
        }
    ]


## <a name="next-steps"></a>Nächste Schritte

- [Hinzufügen einer Ansicht zu Ihrer Lösung](operations-management-suite-solutions-resources-views.md) gesammelte Daten visualisiert werden sollen.
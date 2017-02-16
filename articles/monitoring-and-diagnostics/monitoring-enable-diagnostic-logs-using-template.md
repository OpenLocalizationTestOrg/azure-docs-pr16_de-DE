<properties
    pageTitle="Automatisch aktivieren mithilfe einer Vorlage Ressourcenmanager Diagnoseeinstellungen | Microsoft Azure"
    description="Erfahren Sie, wie Sie mithilfe einer Vorlage Ressourcenmanager diagnoseeinstellungen erstellen, mit denen Sie Ihre Diagnoseprotokolle an Ereignis Hubs übertragen oder in einem Speicherkonto zu speichern."
    authors="johnkemnetz"
    manager="rboucher"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="johnkem"/>

# <a name="automatically-enable-diagnostic-settings-at-resource-creation-using-a-resource-manager-template"></a>Aktivieren Sie Diagnoseeinstellungen automatisch in das Erstellen von Ressourcen mithilfe einer Vorlage Ressourcenmanager
In diesem Artikel zeigen wir die Verwendung einer [Vorlage Azure Ressourcenmanager](../resource-group-authoring-templates.md) Diagnoseeinstellungen für eine Ressource konfigurieren, wenn er erstellt wird. Dadurch können Sie für den automatischen start streaming Ihre Diagnoseprotokolle und Kennzahlen an Ereignis Hubs, archiviert werden in einem Speicher-Konto oder beim Erstellen eine Ressource an Log Analytics senden.

Die Verfahren zum Aktivieren der Diagnoseprotokolle mithilfe einer Vorlage Ressourcenmanager hängt die Ressourcenart aus.

- **Nicht-berechnen** Ressourcen (z. B. Netzwerk Sicherheitsgruppen, Logik Apps, Automatisierung) verwenden Sie [Diagnoseeinstellungen in diesem Artikel beschriebenen](./monitoring-overview-of-diagnostic-logs.md#diagnostic-settings).
- **Berechnen** Ressourcen (WAD/LAD-basiert) verwenden Sie die [in diesem Artikel beschriebenen WAD/LAD-Konfigurationsdatei](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md).

In diesem Artikel beschrieben wir mit einer der Diagnose konfigurieren.

Die grundlegenden Schritte sind wie folgt aus:

1. Erstellen Sie eine Vorlage als JSON-Datei, die beschreibt, wie Sie die Ressource zu erstellen, und aktivieren die Diagnose.
2. [Bereitstellen der Vorlage mit einer beliebigen Bereitstellungsmethode](../resource-group-template-deploy.md).

Im folgenden geben wir ein Beispiel für die JSON-Vorlagendatei, die Sie für nicht berechnen und berechnen Ressourcen generieren müssen.

## <a name="non-compute-resource-template"></a>Nicht-berechnen Ressourcenvorlage
Für Ressourcen nicht berechnen müssen Sie zwei Schritte ausführen:

1. Hinzufügen von Parametern zu der Parameter Blob für den Speicher Kontonamen, Service Bus Regel-ID und/oder OMS Log Analytics Arbeitsbereich-ID (Aktivieren der Diagnoseprotokolle in einem Speicherkonto, Protokolle Ereignis Hubs streaming und/oder Senden von Protokollen auf Log Analytics Archivierung).

    ```json
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Storage Account in which Diagnostic Logs should be saved."
      }
    },
    "serviceBusRuleId": {
      "type": "string",
      "metadata": {
        "description": "Service Bus Rule Id for the Service Bus Namespace in which the Event Hub should be created or streamed to."
      }
    },
    "workspaceId":{
      "type": "string",
      "metadata": {
        "description": "Log Analytics workspace ID for the Log Analytics workspace to which logs will be sent."
      }
    }
    ```
2. In der Matrix Ressourcen der Ressource, für die Sie Diagnoseprotokolle aktivieren möchten, fügen Sie eine Ressource der Art `[resource namespace]/providers/diagnosticSettings`.

    ```json
    "resources": [
      {
        "type": "providers/diagnosticSettings",
        "name": "Microsoft.Insights/service",
        "dependsOn": [
          "[/*resource Id for which Diagnostic Logs will be enabled>*/]"
        ],
        "apiVersion": "2015-07-01",
        "properties": {
          "storageAccountId": "[resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName'))]",
          "serviceBusRuleId": "[parameters('serviceBusRuleId')]",
          "workspaceId": "[parameters('workspaceId')]",
          "logs": [ 
            {
              "category": "/* log category name */",
              "enabled": true,
              "retentionPolicy": {
                "days": 0,
                "enabled": false
              }
            }
          ]
        }
      }
    ]
    ```

Das Eigenschaften Blob für die Diagnose Einstellung folgt [das Format, die in diesem Artikel beschrieben](https://msdn.microsoft.com/library/azure/dn931931.aspx).

Hier ist ein vollständiges Beispiel, bei das eine Sicherheitsgruppe Netzwerk erstellt und aktiviert streaming Ereignis Hubs und Speicher in einem Speicherkonto ein.

```json

{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "nsgName": {
      "type": "string",
      "metadata": {
        "description": "Name of the NSG that will be created."
      }
    },
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Storage Account in which Diagnostic Logs should be saved."
      }
    },
    "serviceBusRuleId": {
      "type": "string",
      "metadata": {
        "description": "Service Bus Rule Id for the Service Bus Namespace in which the Event Hub should be created or streamed to."
      }
    },
    "workspaceId": {
      "type": "string",
      "metadata": {
        "description": "Log Analytics workspace ID for the Log Analytics workspace to which logs will be sent."
      }
    }
  },
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Network/networkSecurityGroups",
      "name": "[parameters('nsgName')]",
      "apiVersion": "2016-03-30",
      "location": "westus",
      "properties": {
        "securityRules": []
      },
      "resources": [
        {
          "type": "providers/diagnosticSettings",
          "name": "Microsoft.Insights/service",
          "dependsOn": [
            "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('nsgName'))]"
          ],
          "apiVersion": "2015-07-01",
          "properties": {
            "storageAccountId": "[resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName'))]",
            "serviceBusRuleId": "[parameters('serviceBusRuleId')]",
            "workspaceId": "[parameters('workspaceId')]",
            "logs": [
              {
                "category": "NetworkSecurityGroupEvent",
                "enabled": true,
                "retentionPolicy": {
                  "days": 0,
                  "enabled": false
                }
              },
              {
                "category": "NetworkSecurityGroupRuleCounter",
                "enabled": true,
                "retentionPolicy": {
                  "days": 0,
                  "enabled": false
                }
              }
            ]
          }
        }
      ],
      "dependsOn": []
    }
  ]
}

```

## <a name="compute-resource-template"></a>Berechnen der Ressourcenvorlage
Klicken Sie zum Aktivieren der Diagnose für eine Ressource berechnen müssen beispielsweise einem Cluster virtuellen Computern oder Dienst Fabric Sie:

1. Die Definition der Ressource virtueller Computer die Diagnose Azure-Erweiterung hinzugefügt.
2. Geben Sie einen Speicher Konto und/oder Ereignis Hub als Parameter ein.
3. Fügen Sie den Inhalt Ihrer WADCfg XML-Datei in der Eigenschaft XMLCfg Escapezeichen für alle XML-Zeichen ordnungsgemäß an.

> [AZURE.WARNING] Der letzte Schritt kann wenig schwierig sein. Sie [finden Sie im Artikel](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md#diagnostics-configuration-variables) ein Beispiel, bei dem die Diagnose Konfigurationsschema in Variablen aufgeteilt ist, die das Escapezeichen und richtig formatiert.

Der gesamte Prozess, einschließlich Beispiele, ist beschrieben [in diesem Dokument](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md).


## <a name="next-steps"></a>Nächste Schritte
- [Weitere Informationen zum Azure Diagnoseprotokolle](./monitoring-overview-of-diagnostic-logs.md)
- [Streamen Azure Diagnoseprotokolle an Hubs Ereignis.](./monitoring-stream-diagnostic-logs-to-event-hubs.md)

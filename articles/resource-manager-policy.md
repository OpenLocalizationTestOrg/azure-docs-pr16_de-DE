<properties
    pageTitle="Azure Ressourcenmanager Richtlinie | Microsoft Azure"
    description="Beschreibt, wie Azure Ressourcenmanager Richtlinie zu verwenden, um zu verhindern, dass Verstöße mit verschiedenen Bereichen wie Abonnement, Ressourcengruppen oder einzelne Ressourcen."
    services="azure-resource-manager"
    documentationCenter="na"
    authors="ravbhatnagar"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="azure-resource-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/25/2016"
    ms.author="gauravbh;tomfitz"/>

# <a name="use-policy-to-manage-resources-and-control-access"></a>Verwenden Sie die Richtlinie zum Verwalten von Ressourcen und Steuern des Zugriffs

Ressourcenmanager Azure ermöglicht jetzt Steuern des Benutzerzugriffs mithilfe der benutzerdefinierten Richtlinien. Sie können mit Richtlinien Benutzer in Ihrer Organisation von Konventionen, die erforderlich sind, zum Verwalten Ihrer Organisation Ressourcen für den Umbruch verhindern. 

Sie erstellen die Richtliniendefinitionen, in denen die Aktionen oder Ressourcen, die speziell verweigert werden beschrieben. Sie weisen diese Richtliniendefinitionen in den gewünschten Bereich, z. B. das Abonnement, Ressourcengruppe oder eine bestimmte Ressource. 

In diesem Artikel erläutern wir die grundlegende Struktur der Richtlinie Definitionssprache, die Sie verwenden können, um Richtlinien zu erstellen. Wir beschreiben Sie anschließend, wie Sie die folgenden Richtlinien in verschiedenen Bereichen anwenden können.

## <a name="how-is-it-different-from-rbac"></a>Was sind die RBAC abweicht?

Es gibt einige wesentliche Unterschiede zwischen Richtlinie und rollenbasierte Access-Steuerelement, aber als Erstes zu verstehen ist, dass Richtlinien und RBAC zusammenarbeiten. Um die Richtlinien zu verwenden, müssen Sie über RBAC authentifiziert werden. Im Gegensatz zu RBAC, Richtlinie ist ein Standard zulassen und explizite System verweigern. 

RBAC Schwerpunkt auf der Registerkarte Aktionen, die **Benutzer** , die in anderen Bereichen ausführen kann. Beispielsweise wird ein bestimmtes Benutzers zu der Rolle "Mitwirkender" für eine Ressourcengruppe in den gewünschten Bereich, addiert, sodass der Benutzer dieser Ressourcengruppe ändern kann. 

Richtlinie Schwerpunkt auf **Ressource** Aktionen in verschiedenen Bereichen. Beispielsweise können Sie über die Richtlinien, die Arten von Ressourcen steuern, die bereitgestellt werden kann, oder beschränken die jeweiligen Speicherorten, in denen die Ressourcen bereitgestellt werden können.

## <a name="common-scenarios"></a>Häufige Szenarien

Ein gängiges Szenario besteht darin, Abteilung Kategorien für Chargeback Zweck erforderlich. Eine Organisation möglicherweise möchten Sie Vorgänge können nur, wenn die entsprechende Kostenstelle verknüpft ist; Andernfalls wird Anforderung verweigern. Diese Richtlinie hilft, belasten die entsprechenden Kostenstelle für die Vorgänge ausgeführt werden.

Ein weiteres gängiges Szenario ist, dass die Organisation möglicherweise Speicherorte im steuern, wo die Ressourcen erstellt werden, sollen. Oder möchten sie möglicherweise Zugriff auf die Ressourcen steuern, indem Sie nur bestimmte Arten von Ressourcen bereitgestellt werden.

Auf ähnliche Weise eine Organisation steuern Service-Katalog oder die gewünschten Benennungskonventionen für die Ressourcen erzwingen.

Verwenden von Richtlinien können diese Szenarios einfach erzielt werden.

## <a name="policy-definition-structure"></a>Struktur der Richtlinie

Richtliniendefinition wird die Verwendung von JSON erstellt. Es besteht aus mindestens Bedingungen/logische Operatoren, die die Aktionen zu definieren, und ein Effekt, was soll geschieht, wenn die Bedingungen erfüllt sind. Das Schema wird am [http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json](http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json)veröffentlicht. 

Eine Richtlinie enthält im Wesentlichen die folgenden Elemente:

**Bedingung/logische Operatoren:** eine Reihe von Bedingungen, die über eine Reihe von logischen Operatoren bearbeitet werden kann.

**Effekt:** was passiert, wenn die Bedingung erfüllt ist – entweder verweigern oder überwachen. Ein Effekt Audit gibt eine Warnung Dienst Ereignisprotokoll aus. Beispielsweise kann ein Administrator eine Richtlinie erstellen, die ein Ereignis Audit verursacht, wenn eine Person ein großes virtuellen Computers erstellt. Der Administrator kann die Protokolle später zu überprüfen.

    {
      "if" : {
          <condition> | <logical operator>
      },
      "then" : {
          "effect" : "deny | audit | append"
      }
    }
    
## <a name="policy-evaluation"></a>Auswertung von Richtlinien

Richtlinien werden ausgewertet, wenn Ressourcen erstellt werden. Richtlinien werden bei der Bereitstellung der Vorlage während der Erstellung der einzelnen Ressourcen in der Vorlage ausgewertet. 

> [AZURE.NOTE] Richtlinie wird derzeit nicht Ressourcentypen ausgewertet, die keine Kategorien und Art Speicherort, beispielsweise die Ressourcenart Microsoft.Resources/deployments unterstützen. Diese Unterstützung wird zu einem späteren Zeitpunkt hinzugefügt werden. Um Abwärtskompatibilitätsprobleme zu vermeiden, sollten Sie explizit Typ beim Erstellen von Richtlinien angeben. Beispielsweise wird eine Kategorie-Richtlinie, die keine Datentypen angegeben ist für alle Diagrammtypen angewendet. In diesem Fall eine Vorlage Bereitstellung kann ein Fehler auftreten, es ist eine geschachtelte Ressource, die Kategorien nicht unterstützt, und die Bereitstellung Ressourcenart Auswertung von Richtlinien hinzugefügt wurde. 

## <a name="logical-operators"></a>Logische Operatoren

Unterstützten logische Operatoren zusammen mit der Syntax sind:

| Operatorname     | Syntax         |
| :------------- | :------------- |
| Nicht            | "nicht": {&lt;Bedingung oder Operator &gt;}             |
| Und           | "AllOf": [{&lt;Bedingung oder Operator &gt;}, {&lt;Bedingung oder Operator &gt;}] |
| Oder                         | "AnyOf": [{&lt;Bedingung oder Operator &gt;}, {&lt;Bedingung oder Operator &gt;}] |

Ressourcenmanager können Sie komplexe Logik in Ihrer Richtlinie durch geschachtelte Operatoren angeben. Beispielsweise können Sie das Erstellen von Ressourcen an einem bestimmten Speicherort für einen bestimmten Ressourcentyp verweigern. Nachfolgend finden Sie ein Beispiel für geschachtelte Operatoren.

## <a name="conditions"></a>Bedingungen

Eine Bedingung ausgewertet wird, ob ein **Feld** oder **Quelle** bestimmten Kriterien entspricht. Die Namen der unterstützten Bedingung und Syntax sind:

| Name der Bedingung | Syntax                |
| :------------- | :------------- |
| Gleich             | "ist gleich": "&lt;Wert&gt;"               |
| Wie                  | "gefällt mir": "&lt;Wert&gt;"                   |
| Enthält          | "enthält": "&lt;Wert&gt;"|
| In                        | "in": ["&lt;Wert1&gt;","&lt;Wert2&gt;"]|
| ContainsKey    | "ContainsKey": "&lt;Schlüssel&gt;" |
| Vorhanden ist     | "exists": "&lt;Bool&gt;" |

### <a name="fields"></a>(Felder)

Bedingungen werden geformt mithilfe von Feldern und Quellen verwendet. Ein Feld darstellt, Eigenschaften in der Ressource Anforderungsnutzlast, die verwendet wird, um den Status der Ressource zu beschreiben. Eine Datenquelle darstellt Merkmale der Anfrage selbst. 

Die folgenden Felder und Datenquellen werden unterstützt:

Felder: **Name**, **Art**, **Typ**, **Speicherort**, **Kategorien**, **Tags.** *, and * *Eigenschaft Alias**. 

### <a name="property-aliases"></a>Eigenschaft Aliase 
Eigenschaft Alias ist ein Name, der Zugriff auf die Ressource Typ bestimmte Eigenschaften, wie z. B. Einstellungen und Skus in einer Richtliniendefinition verwendet werden kann. Funktionsweise in allen API-Versionen, in dem die Eigenschaft vorhanden ist. Aliase können abgerufen werden, mithilfe der REST-API abgebildet (Powershell-Unterstützung in der Zukunft hinzugefügt werden):

    GET /subscriptions/{id}/providers?$expand=resourceTypes/aliases&api-version=2015-11-01
    
Nachfolgend finden Sie die Definition eines Alias. Wie Sie sehen können, definiert ein Alias Pfade in unterschiedlichen Versionen von API, auch wenn eine Eigenschaft Namensänderung vorhanden ist. 

    "aliases": [
        {
          "name": "Microsoft.Storage/storageAccounts/sku.name",
          "paths": [
            {
              "path": "properties.accountType",
              "apiVersions": [
                "2015-06-15",
                "2015-05-01-preview"
              ]
            },
            {
              "path": "sku.name",
              "apiVersions": [
                "2016-01-01"
              ]
            }
          ]
        }
    ]

Aktuell sind die unterstützten Aliase aus:

| Aliasnamen ein. | Beschreibung |
| ---------- | ----------- |
| {resourceType}/sku.name | Unterstützte Ressourcentypen sind: Microsoft.Compute/virtualMachines,<br />Microsoft.Storage/storageAccounts,<br />Microsoft.Web/serverFarms,<br /> Microsoft.Scheduler/jobcollections,<br />Microsoft.DocumentDB/databaseAccounts,<br />Microsoft.Cache/Redis,<br />Microsoft.CDN/profiles |
| {resourceType}/sku.family | Unterstützte Ressourcentyp ist Microsoft.Cache/Redis |
| {resourceType}/sku.capacity | Unterstützte Ressourcentyp ist Microsoft.Cache/Redis |
| Microsoft.Compute/virtualMachines/imagePublisher |  |
| Microsoft.Compute/virtualMachines/imageOffer  |  |
| Microsoft.Compute/virtualMachines/imageSku  |  |
| Microsoft.Compute/virtualMachines/imageVersion  |  |
| Microsoft.Cache/Redis/enableNonSslPort |  |
| Microsoft.Cache/Redis/shardCount |  |
| Microsoft.SQL/servers/version |  |
| Microsoft.SQL/servers/databases/requestedServiceObjectiveId |  |
| Microsoft.SQL/servers/databases/requestedServiceObjectiveName |  |
| Microsoft.SQL/servers/databases/edition |  |
| Microsoft.SQL/servers/databases/elasticPoolName |  |
| Microsoft.SQL/servers/elasticPools/dtu |  |
| Microsoft.SQL/servers/elasticPools/edition |  |

Derzeit funktioniert nur Richtlinie auf sich Serviceanfragen. 

## <a name="effect"></a>Effekt
Richtlinie unterstützt drei Arten von Effekt - **Verweigern**, **Überwachen**und **Anfügen**. 

- Verweigern generiert ein Ereignis im Überwachungsprotokoll, wobei die Anforderung fehlschlägt
- Audit ein Ereignis im Überwachungsprotokoll generiert, aber nicht die Anforderung schlägt fehl
- Anfügen der Anforderung den definierten Satz von Feldern hinzugefügt 

Für die **angefügt werden soll**müssen Sie die folgenden Details angeben:

    ....
    "effect": "append",
    "details": [
      {
        "field": "field name",
        "value": "value of the field"
      }
    ]

Der Wert kann entweder eine Zeichenfolge oder ein JSON-Formatobjekt sein. 

## <a name="policy-definition-examples"></a>Beispiele für die Richtlinie Definition

Jetzt sehen wir uns wie wir die Richtlinie, um den oben beschriebenen Szenarien erzielen definieren.

### <a name="chargeback-require-departmental-tags"></a>Chargeback: Erfordern Sie Abteilung Kategorien

Die folgende Richtlinie verweigert Besprechungsanfragen, die noch nicht über eine Kategorie, die mit "CostCenter" Schlüssel verfügen.

    {
      "if": {
        "not" : {
          "field" : "tags",
          "containsKey" : "costCenter"
        }
      },
      "then" : {
        "effect" : "deny"
      }
    }

Die folgende Richtlinie fügt CostCenter Tag mit einem vordefinierten Wert an, wenn keine Tags vorhanden sind. 

    {
      "if": {
        "field": "tags",
        "exists": "false"
      },
      "then": {
        "effect": "append",
        "details": [
          {
            "field": "tags",
            "value": {"costCenter":"myDepartment" }
          }
        ]
      }
    }
    
Die folgende Richtlinie fügt CostCenter Tag mit einem vordefinierten Wert an, wenn das Kennzeichen CostCenter nicht vorhanden ist, aber andere Tags vorhanden sind. 

    {
      "if": {
        "allOf": [
          {
            "field": "tags",
            "exists": "true"
          },
          {
            "field": "tags.costCenter",
            "exists": "false"
          }
        ]
    
      },
      "then": {
        "effect": "append",
        "details": [
          {
            "field": "tags.costCenter",
            "value": "myDepartment"
          }
        ]
      }
    }


### <a name="geo-compliance-ensure-resource-locations"></a>: Geo Einhaltung Ressource Speicherorte

Im folgenden Beispiel wird eine Richtlinie, die Anfragen verweigert, in dem kein Speicherort North Europa oder Westen Europe.

    {
      "if" : {
        "not" : {
          "field" : "location",
          "in" : ["northeurope" , "westeurope"]
        }
      },
      "then" : {
        "effect" : "deny"
      }
    }

### <a name="service-curation-select-the-service-catalog"></a>Dienst Curation: Wählen Sie den Dienstkatalog

Im folgenden Beispiel wird eine Richtlinie, die nur auf die Dienste vom Typ Microsoft.Resources/ Aktionen explizit gestattet\*, Microsoft.Compute/\*, Microsoft.Storage/\*, Microsoft.Network/\* zulässig sind. Sonstiges verweigert.

    {
      "if" : {
        "not" : {
          "anyOf" : [
            {
              "field" : "type",
              "like" : "Microsoft.Resources/*"
            },
            {
              "field" : "type",
              "like" : "Microsoft.Compute/*"
            },
            {
              "field" : "type",
              "like" : "Microsoft.Storage/*"
            },
            {
              "field" : "type",
              "like" : "Microsoft.Network/*"
            }
          ]
        }
      },
      "then" : {
        "effect" : "deny"
      }
    }

### <a name="use-approved-skus"></a>Verwenden von genehmigten SKUs

Im folgenden Beispiel wird die Verwendung der Eigenschaft Alias SKUs zu beschränken. Im Beispiel werden nur Standard_LRS und Standard_GRS für Speicherkonten genehmigt.

    {
      "if": {
        "allOf": [
          {
            "field": "type",
            "equals": "Microsoft.Storage/storageAccounts"
          },
          {
            "not": {
              "allof": [
                {
                  "field": "Microsoft.Storage/storageAccounts/sku.name",
                  "in": ["Standard_LRS", "Standard_GRS"]
                }
              ]
            }
          }
        ]
      },
      "then": {
        "effect": "deny"
      }
    }
    

### <a name="naming-convention"></a>Benennungskonvention

Im folgenden Beispiel wird die Verwendung von Platzhalterzeichen, die von der Bedingung "wie" unterstützt wird. Die Bedingung, die besagt ist der Name das erwähnte Muster stehenden (NamePrefix\*NameSuffix) verweigern klicken Sie dann auf die Anfrage.

    {
      "if" : {
        "not" : {
          "field" : "name",
          "like" : "namePrefix*nameSuffix"
        }
      },
      "then" : {
        "effect" : "deny"
      }
    }
    
### <a name="tag-requirement-just-for-storage-resources"></a>Kategorie Anforderung nur für Speicher-Ressourcen

Im folgenden Beispiel wird gezeigt, wie logische Operatoren, um eine Kategorie Anwendung nur für Speicherressourcen erfordern geschachtelt werden.

    {
        "if": {
            "allOf": [
              {
                "not": {
                  "field": "tags",
                  "containsKey": "application"
                }
              },
              {
                "field": "type",
                "equals": "Microsoft.Storage/storageAccounts"
              }
            ]
        },
        "then": {
            "effect": "audit"
        }
    }

## <a name="policy-assignment"></a>Richtlinie Zuordnung

Richtlinien werden in verschiedenen Bereichen wie Abonnement, Ressourcengruppen und einzelne Ressourcen angewendet. Ressourcen für alle untergeordneten erben Richtlinien. Ja, wenn Sie eine Richtlinie Ressourcengruppe angewendet wird, ist es für alle Ressourcen in dieser Ressourcengruppe anwendbar.

## <a name="creating-a-policy"></a>Erstellen einer Richtlinie

Dieser Abschnitt enthält Informationen über wie eine Richtlinie mit REST-API erstellt werden kann.

### <a name="create-policy-definition-with-rest-api"></a>Erstellen von Richtliniendefinition mit REST-API

Sie können eine Richtlinie mit den [REST-API Richtlinie Definitionen](https://msdn.microsoft.com/library/azure/mt588471.aspx)erstellen. Die REST-API ermöglicht es Ihnen zu erstellen und Löschen von Policy-Definitionen und erhalten von Informationen zu vorhandenen Definitionen.

Zum Erstellen einer Richtlinie, führen Sie Folgendes aus:

    PUT https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.authorization/policydefinitions/{policyDefinitionName}?api-version={api-version}

Mit einer Anforderungstexts ähnlich wie im folgenden Beispiel:

    {
      "properties":{
        "policyType":"Custom",
        "description":"Test Policy",
        "policyRule":{
          "if" : {
            "not" : {
              "field" : "tags",
              "containsKey" : "costCenter"
            }
          },
          "then" : {
            "effect" : "deny"
          }
        }
      },
      "name":"testdefinition"
    }


Verwenden Sie für api-Version *2016-04-01*ein. Beispiele und Weitere Informationen hierzu finden Sie unter [REST-API Richtlinie Definitionen](https://msdn.microsoft.com/library/azure/mt588471.aspx).

### <a name="create-policy-definition-using-powershell"></a>Erstellen der Richtliniendefinition mithilfe der PowerShell

Sie können mithilfe des Cmdlets New-AzureRmPolicyDefinition Richtliniendefinition erstellen. Im folgende Beispiel wird eine Richtlinie für die Ressourcen nur in Europa North und Westen Europa erstellt.

    $policy = New-AzureRmPolicyDefinition -Name regionPolicyDefinition -Description "Policy to allow resource creation only in certain regions" -Policy '{  
      "if" : {
        "not" : {
          "field" : "location",
          "in" : ["northeurope" , "westeurope"]
        }
      },
      "then" : {
        "effect" : "deny"
      }
    }'          

Die Ausgabe der Ausführung im $policy Objekt gespeichert ist, und Verlauf der Richtlinie Zuordnung verwendet werden kann. Für den Richtlinienparameter kann auch der Pfad zu einer .json-Datei, die mit der Richtlinie anstelle den Richtlinie Inline bereitgestellt werden.

    New-AzureRmPolicyDefinition -Name regionPolicyDefinition -Description "Policy to allow resource creation only in certain    regions" -Policy "path-to-policy-json-on-disk"

### <a name="create-policy-definition-using-azure-cli"></a>Erstellen der Richtliniendefinition Azure CLI verwenden

Sie können mithilfe der Azure CLI mit der Richtlinie Definition-Befehl wie unten dargestellt Richtliniendefinition erstellen. Im folgende Beispiel wird eine Richtlinie für die Ressourcen nur in Europa North und Westen Europa erstellt.

    azure policy definition create --name regionPolicyDefinition --description "Policy to allow resource creation only in certain regions" --policy-string '{   
      "if" : {
        "not" : {
          "field" : "location",
          "in" : ["northeurope" , "westeurope"]
        }
      },
      "then" : {
        "effect" : "deny"
      }
    }'    
    

Es ist möglich, geben Sie den Pfad zu einer .json-Datei, die mit der Richtlinie anstelle den Richtlinie Inline wie unten dargestellt.

    azure policy definition create --name regionPolicyDefinition --description "Policy to allow resource creation only in certain regions" --policy "path-to-policy-json-on-disk"


## <a name="applying-a-policy"></a>Anwenden einer Richtlinie

### <a name="policy-assignment-with-rest-api"></a>Richtlinie Zuordnung mit REST-API

Sie können die Richtliniendefinition auf den gewünschten Bereich durch die [REST-API für Zuordnungen Richtlinie](https://msdn.microsoft.com/library/azure/mt588466.aspx)anwenden. Die REST-API ermöglicht es Ihnen zu erstellen und Löschen von zugewiesenen Richtlinien und erhalten von Informationen zu vorhandenen Zuordnungen.

Zum Erstellen einer Zuordnungs der Richtlinie, führen Sie Folgendes aus:

    PUT https://management.azure.com /subscriptions/{subscription-id}/providers/Microsoft.authorization/policyassignments/{policyAssignmentName}?api-version={api-version}

{Richtlinie-Zuordnung} ist der Name der Richtlinie Zuordnung an. Verwenden Sie für api-Version *2016-04-01*ein. 

Mit einer Anforderungstexts ähnlich wie im folgenden Beispiel:

    {
      "properties":{
        "displayName":"VM_Policy_Assignment",
        "policyDefinitionId":"/subscriptions/########/providers/Microsoft.Authorization/policyDefinitions/testdefinition",
        "scope":"/subscriptions/########-####-####-####-############"
      },
      "name":"VMPolicyAssignment"
    }

Beispiele und Weitere Informationen hierzu finden Sie unter [REST-API für Zuordnungen Richtlinie](https://msdn.microsoft.com/library/azure/mt588466.aspx).

### <a name="policy-assignment-using-powershell"></a>Mithilfe der PowerShell Richtlinie-Zuordnung

Sie können die oben auf den gewünschten Bereich mithilfe des Cmdlets New-AzureRmPolicyAssignment über PowerShell erstellte Richtlinie anwenden:

    New-AzureRmPolicyAssignment -Name regionPolicyAssignment -PolicyDefinition $policy -Scope    /subscriptions/########-####-####-####-############/resourceGroups/<resource-group-name>
        
Hier ist $policy der Richtlinienobjekt, das durch das Ausführen des Cmdlets New-AzureRmPolicyDefinition obigen zurückgegeben wurde. Dem Bereich hier handelt es sich um den Namen der Ressourcengruppe, die Sie angeben.

Wenn Sie die oben angegebenen Richtlinie Zuordnung entfernen möchten, können Sie ihn wie folgt ausführen:

    Remove-AzureRmPolicyAssignment -Name regionPolicyAssignment -Scope /subscriptions/########-####-####-####-############/resourceGroups/<resource-group-name>

Sie können erhalten, ändern oder Richtliniendefinitionen durch Get-AzureRmPolicyDefinition-AzureRmPolicyDefinition festlegen und entfernen-AzureRmPolicyDefinition Cmdlets Hilfethemas entfernen.

Sie können auf ähnliche Weise abrufen, ändern oder entfernen die Richtlinie Zuordnungen durch die Get-AzureRmPolicyAssignment, Set-AzureRmPolicyAssignment und entfernen-AzureRmPolicyAssignment Cmdlets Hilfethemas.

### <a name="policy-assignment-using-azure-cli"></a>Richtlinie Zuordnung mit Azure CLI

Sie können die oben auf den gewünschten Bereich mithilfe des Befehls der Richtlinie Zuordnung über Azure CLI erstellte Richtlinie anwenden:

    azure policy assignment create --name regionPolicyAssignment --policy-definition-id /subscriptions/########-####-####-####-############/providers/Microsoft.Authorization/policyDefinitions/<policy-name> --scope    /subscriptions/########-####-####-####-############/resourceGroups/<resource-group-name>
        
Dem Bereich hier handelt es sich um den Namen der Ressourcengruppe, die Sie angeben. Wenn der Wert der Parameter Richtlinie-Definition-Id unbekannt ist, ist es möglich, über die CLI Azure zu erhalten, wie unten dargestellt: 

    azure policy definition show <policy-name>

Wenn Sie die oben angegebenen Richtlinie Zuordnung entfernen möchten, können Sie ihn wie folgt ausführen:

    azure policy assignment delete --name regionPolicyAssignment --scope /subscriptions/########-####-####-####-############/resourceGroups/<resource-group-name>

Sie können erhalten, ändern oder entfernen Richtlinie Definitionen durch Richtliniendefinition anzeigen, festlegen möchten, und löschen Sie Befehle Hilfethemas.

Sie können auf ähnliche Weise erhalten, ändern oder entfernen Richtlinie Zuordnungen, bis die Richtlinie Zuordnung anzeigen und löschen die Befehle Hilfethemas.

##<a name="policy-audit-events"></a>Richtlinie von Ereignissen

Nachdem Sie die Richtlinie angewendet haben, beginnen Sie Ereignisse im Zusammenhang mit der Richtlinie angezeigt. Sie können jetzt entweder-Portal führen PowerShell oder der Azure CLI, um diese Daten zu gelangen. 

### <a name="policy-audit-events-using-powershell"></a>Richtlinie Audit Ereignisse mithilfe der PowerShell

Um alle Ereignisse anzuzeigen, die im Zusammenhang mit Effekt verwehren möchten, können Sie den folgenden PowerShell-Befehl:

    Get-AzureRmLog | where {$_.OperationName -eq "Microsoft.Authorization/policies/deny/action"} 

Wenn alle Ereignisse im Zusammenhang mit Effekt überwachenden anzeigen möchten, können Sie den folgenden Befehl aus:

    Get-AzureRmLog | where {$_.OperationName -eq "Microsoft.Authorization/policies/audit/action"} 

### <a name="policy-audit-events-using-azure-cli"></a>Richtlinie Audit Ereignisse mit Azure CLI

Zum Anzeigen aller Ereignisse aus einer Ressource zu gruppieren, die im Zusammenhang mit Effekt verweigern, können Sie den folgenden CLI-Befehl verwenden:

    azure group log show ExampleGroup --json | jq ".[] | select(.operationName.value == \"Microsoft.Authorization/policies/deny/action\")"

Wenn alle Ereignisse im Zusammenhang mit Effekt überwachenden anzeigen möchten, können Sie den folgenden Befehl aus CLI:

    azure group log show ExampleGroup --json | jq ".[] | select(.operationName.value == \"Microsoft.Authorization/policies/audit/action\")"

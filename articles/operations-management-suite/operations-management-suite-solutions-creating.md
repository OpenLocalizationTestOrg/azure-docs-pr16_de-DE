<properties
   pageTitle="Erstellen von Lösungen Management in Vorgänge Management Suite (OMS) | Microsoft Azure"
   description="Management-Lösungen erweitern die Funktionalität der Vorgänge Management Suite (OMS) durch die Bereitstellung verpackt Management Szenarien, die Kunden ihre OMS-Arbeitsbereich hinzugefügt werden können.  Dieser Artikel enthält ausführliche Informationen zum Erstellen Sie Management Solutions in Ihrer eigenen Umgebung verwendet werden oder an Ihre Kunden zur Verfügung gestellt."
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
   ms.date="10/27/2016"
   ms.author="bwren" />

# <a name="creating-management-solutions-in-operations-management-suite-oms-preview"></a>Erstellen von Lösungen Management in Vorgänge Management Suite (OMS) (Preview)

>[AZURE.NOTE]Dies ist über der vorläufigen Dokumentation zum Erstellen von Lösungen Management in OMS die derzeit in der Vorschau sind. Alle nachfolgend beschriebenen Schema unterliegt ändern.  

Management-Lösungen erweitern die Funktionalität der Vorgänge Management Suite (OMS) durch die Bereitstellung verpackt Management Szenarien, die Kunden ihre OMS-Arbeitsbereich hinzugefügt werden können.  Dieser Artikel enthält Details zum Erstellen Ihrer eigenen Management-Lösungen, dass Sie in Ihrer eigenen Umgebung verwenden oder Kunden durch die Community zur Verfügung stellen können.

## <a name="planning-your-management-solution"></a>Planen der Verwaltung Lösung
Projektmanagement-Lösungen in OMS sind mehrere Ressourcen zur Unterstützung von einer bestimmten Management-Szenario.  Wenn Ihre Lösung planen, sollten Sie sich auf dem Szenario, das Sie erzielen möchten und alle erforderlichen Ressourcen für die Unterstützung konzentrieren.  Jede Lösung sollte eigenständig und definieren jede Ressource, die dies erforderlich macht, auch wenn eine oder mehrere Ressourcen auch durch andere Lösungen definiert sind.  Bei eine Lösung installiert ist, wird jeder Ressource erstellt, es sei denn, sie bereits vorhanden ist, und Sie können definieren, was mit Ressourcen geschieht, wenn eine Lösung entfernt wird.  

Beispielsweise enthalten eine Lösung möglicherweise eine [Azure Automatisierung Runbooks](../automation/automation-intro.md) , die sammelt Daten in das Protokoll Analytics Repository mit einem [Zeitplan](../automation/automation-schedules.md) und einer [Ansicht](../log-analytics/log-analytics-view-designer.md) , die verschiedene Visualisierungen gesammelten Daten enthält.  Eine andere Lösung möglicherweise demselben Zeitplan verwendet werden.  Als Autor Management Lösung können Sie alle drei Ressourcen definieren, aber angeben, dass die Runbooks und Ansicht automatisch entfernt werden soll, wenn die Lösung entfernt wird.    Sie auch den Zeitplan definieren, aber angeben, dass es im Ort bleiben soll, wenn die Lösung durch die anderen Lösung für den Fall, dass es immer noch verwendet wurde entfernt wurden.

## <a name="management-solution-files"></a>Projektmanagement-Lösung-Dateien
Management-Lösungen werden als [Ressourcenverwaltung Vorlagen](../resource-manager-template-walkthrough.md)implementiert.  Ist die Hauptfenster Aufgabe in lernen, wie Sie Management-Lösungen verfassen learning zum [Erstellen einer Vorlage](../resource-group-authoring-templates.md).  Dieser Artikel enthält eindeutige Details von Vorlagen für Lösungen und so definieren gebräuchliche Lösung Ressourcen verwendet werden.

Die grundlegende Struktur einer Management Lösung Datei entspricht dem [Ressourcenmanager Vorlage](resource-group-authoring-templates.md#template-format) also wie folgt.  Jede in den folgenden Abschnitten werden die Elemente der obersten Ebene und und deren Inhalt in einer Lösung.  

    {
       "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "",
       "parameters": {  },
       "variables": {  },
       "resources": [  ],
       "outputs": {  }
    }

## <a name="parameters"></a>Parameter

[Parameter](../resource-group-authoring-templates.md#parameters) sind Werte, die Sie den Benutzer bei der Installation von Management-Lösung erfordern.  Standard-Parameter, die alle Lösungen vorhanden sind, und Sie können weitere Parameter für Ihre bestimmten Lösung nach Bedarf hinzufügen.  Wie Benutzer Parameterwerte bei der Installation von Ihrer Lösung bereitstellen hängt von bestimmter Parameter und wie die Lösung installiert wird.


Bei der Installation Ihre Management-Lösung über den [Azure Marketplace](operations-management-suite-solutions.md#finding-and-installing-solutions) oder [Azure Schnellstart Vorlagen](operations-management-suite-solutions.md#finding-and-installing-solutions) werden sie aufgefordert, eine [OMS Arbeitsbereich und Automatisierung Konto](operations-management-suite-solutions-creating.md#oms-workspace-and-automation-account)auszuwählen.  Diese werden verwendet, die Werte jedes der standard-Parameter gefüllt wird.  Der Benutzer wird nicht aufgefordert, um direkt Werte für die standardmäßige Parameter bereitzustellen, aber sie werden aufgefordert, um Werte für alle weiteren Parameter bereitzustellen.

Wenn der Benutzer Ihre Lösung [eine andere Methode](operations-management-suite-solutions.md#finding-and-installing-solutions)installiert, müssen sie einen Wert für alle standard-Parameter und alle weiteren Parameter angeben.

Nachfolgend finden Sie ein Beispiel für Parameter.

    "Daily Start Time": {
        "type": "string",
        "metadata": {
            "description": "Enter time for starting VMs by resource group.",
            "control": "datetime",
            "category": "Schedule"
        }

Die folgende Tabelle beschreibt die Attribute eines Parameters an.

| Attribut | Beschreibung |
|:--|:--|
| Typ        | Der Datentyp für den Parameter. Das Eingabewerte Steuerelement für den Benutzer angezeigt wird, hängt von den Datentyp.<br><br>Bool - Dropdown-Feld<br>String - Textfeld<br>Int - Textfeld<br>SecureString - Feld Kennwort<br> |
| Kategorie    | Optional Category für den Parameter.  Parameter in der gleichen Kategorie werden gruppiert. |
| Steuerelement     | Zusätzliche Funktionen für die Zeichenfolgenparameter.<br><br>DateTime - Datetime-Steuerelement wird angezeigt.<br>GUID - Guid-Wert wird automatisch generiert und der Parameter wird nicht angezeigt. |
| Beschreibung | Optionale Beschreibung für den Parameter.  In einer Informationssymbol neben dem Parameter angezeigt. |


### <a name="standard-parameters"></a>Standard-Parameter
Der folgenden Tabelle sind die standardmäßigen Parameter für alle Management-Lösungen.  Für den Benutzer für sie aufgefordert werden, wenn Ihre Lösung aus dem Azure Marketplace oder Schnellstart Vorlagen installiert ist, werden diese Werten aufgefüllt.  Der Benutzer muss für diese Werte angeben, wenn die Lösung mit einer anderen Methode installiert ist.

>[AZURE.NOTE]Die Benutzeroberfläche in der Azure Marketplace und Schnellstart-Vorlagen erwartet in der Tabelle die Parameternamen.  Wenn Sie andere Parameternamen verwenden dann der Benutzer zur aufgefordert werden, und sie werden nicht automatisch ausgefüllt.


| Parameter | Typ | Beschreibung |
|:--|:--|:--|
| Kontoname       | Zeichenfolge |  Azure Automatisierung Kontonamen ein. |
| pricingTier       | Zeichenfolge | Preise Ebene aus sowohl Log Analytics Arbeitsbereich und Automatisierung Azure-Konto an. |
| regionId          | Zeichenfolge | Region des Kontos Azure Automatisierung. |
| solutionName      | Zeichenfolge | Name der Lösung. |
| Ein     | Zeichenfolge | Melden Sie sich Analytics Arbeitsbereichsnamen. |
| workspaceRegionId | Zeichenfolge | Region des Arbeitsbereichs Log Analytics. |





### <a name="sample"></a>Beispiel
Es folgt ein Beispiel für Parameterentität für eine Lösung.  Dies umfasst alle standard-Parameter und zwei zusätzliche Parameter in der gleichen Kategorie.

    "parameters": {
        "workspaceName": {
            "type": "string",
            "metadata": {
                "description": "A valid Log Analytics workspace name"
            }
        },
        "accountName": {
            "type": "string",
            "metadata": {
                "description": "A valid Azure Automation account name"
            }
        },
        "workspaceRegionId": {
            "type": "string",
            "metadata": {
                "description": "Region of the Log Analytics workspace"
            }
        },
        "regionId": {
            "type": "string",
            "metadata": {
                "description": "Region of the Azure Automation account"
            }
        },
        "pricingTier": {
            "type": "string",
            "metadata": {
                "description": "Pricing tier of both Log Analytics workspace and Azure Automation account"
            }
        },
        "jobIdGuid": {
        "type": "string",
            "metadata": {
                "description": "GUID for a runbook job",
                "control": "guid",
                "category": "Schedule"
            }
        },
        "startTime": {
            "type": "string",
            "metadata": {
                "description": "Time for starting the runbook.",
                "control": "datetime",
                "category": "Schedule"
            }
        }


Zum Verweisen auf Parameterwerte in andere Elemente der Lösung mit der Syntax **Parameter ("Parametername")**.  Angenommen, für den Zugriff auf den Arbeitsbereichsnamen **parameters('workspaceName')** , verwenden Sie 

## <a name="variables"></a>Variablen

Das Element **Variablen** enthält Werte, die Sie verwenden möchten, in den Rest der Management-Lösung.  Diese Werte sind für den Benutzer Installieren der Lösung nicht verfügbar.  Sie sind bestimmt des Autors mit einem einzigen Speicherort bereitstellen, in dem sie Werte verwalten können, die in der Lösung mehrmals verwendet werden kann. Alle Werte sollten zu Ihrer Lösung in Variablen statt hartzucodieren diese im Element **Ressourcen** bestimmten setzen.  Dies ist der Code besser lesbar und ermöglicht Ihnen, diese Werte in höhere Versionen auf einfache Weise ändern.

Es folgt ein Beispiel für ein Element **Variablen** mit normalen Parameter in Lösungen verwendet werden.

    "variables": { 
        "SolutionVersion": "1.1", 
        "SolutionPublisher": "Contoso", 
        "SolutionName": "My Solution",
        "LogAnalyticsApiVersion": "2015-11-01-preview",
        "AutomationApiVersion": "2015-10-31"
    },

Zum Verweisen auf Variablenwerten durch die Lösung mit der Syntax **Variablen ("Variablenname")**.  Angenommen, für den Zugriff auf die Variable SolutionName **variables('solutionName')** , verwenden Sie 


## <a name="resources"></a>Ressourcen

Das **Ressourcen** -Element definiert die verschiedenen Ressourcen, die in der Lösung enthalten.  Dies wird den größten und komplexe Teil der Vorlage sein.  Ressourcen werden mit der folgenden Struktur definiert.  

    "resources": [
        {
            "name": "<name-of-the-resource>",           
            "apiVersion": "<api-version-of-resource>",
            "type": "<resource-provider-namespace/resource-type-name>",     
            "location": "<location-of-resource>",
            "tags": "<name-value-pairs-for-resource-tagging>",
            "comments": "<your-reference-notes>",
            "dependsOn": [
                "<array-of-related-resource-names>"
            ],
            "properties": "<unique-settings-for-the-resource>",
            "resources": [
                "<array-of-child-resources>"
            ]
        }
    ]

### <a name="dependencies"></a>Abhängigkeiten
Die Elemente **DependsOn** gibt eine [Abhängigkeit](../resource-group-define-dependencies.md) von einer anderen Ressource an.  Wenn die Lösung installiert ist, wird eine Ressource nicht erstellt, bis alle Abhängigkeit erstellt wurden.  Kann beispielsweise Ihre Lösung [eine Runbooks starten](operations-management-suite-solutions-resources-automation.md#runbooks) , wenn es installiert ist, verwenden eine [Ressource Position](operations-management-suite-solutions-resources-automation.md#automation-jobs).  Die Position Ressource wäre hängt von der Ressource Runbooks, um sicherzustellen, dass die Runbooks erstellt wird, bevor der Auftrag erstellt wurde.

### <a name="oms-workspace-and-automation-account"></a>OMS Arbeitsbereich und Automatisierung-Konto
Management-Lösungen ist ein [Arbeitsbereich OMS](../log-analytics/log-analytics-manage-access.md) Ansichten enthalten und ein [Konto Automatisierung](../automation/automation-security-overview.md#automation-account-overview) Runbooks und zugehörige Ressourcen enthalten erforderlich.  Diese müssen verfügbar sein, bevor die Ressourcen in die Lösung erstellt werden und nicht in die Lösung selbst definiert werden.  Der Benutzer wird [Geben Sie einen Arbeitsbereich und das Konto](operations-management-suite-solutions.md#oms-workspace-and-automation-account) , wenn sie Ihre Lösung bereitstellen, aber als Autor sollten Sie die folgenden Punkte.


## <a name="solution-resource"></a>Lösung resource
Jede Lösung erfordert einen Eintrag in der **Ressourcen** -Element, das die Lösung selbst definiert.  Hat dies einen Typ von **Microsoft.OperationsManagement/solutions**.  Es folgt ein Beispiel für eine Ressource Lösung.  Die anderen Elemente werden in den folgenden Abschnitten beschrieben.

    "name": "[concat(variables('SolutionName'), '[ ' ,parameters('workspacename'), ' ]')]",
    "location": "[parameters('workspaceRegionId')]",
    "tags": { },
    "type": "Microsoft.OperationsManagement/solutions",
    "apiVersion": "[variables('LogAnalyticsApiVersion')]",
    "dependsOn": [
        "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('RunbookName'))]",
        "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/schedules/', variables('ScheduleName'))]",
        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/views/', variables('ViewName'))]"
    ]
    "properties": {
        "workspaceResourceId": "[concat(resourceGroup().id, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]",
        "referencedResources": [
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/schedules/', variables('ScheduleName'))]"
        ],
        "containedResources": [
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('RunbookName'))]",
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/views/', variables('ViewName'))]"
        ]
    },
    "plan": {
        "name": "[concat(variables('SolutionName'), '[' ,parameters('workspacename'), ']')]",
        "Version": "[variables('SolutionVersion')]",
        "product": "AzureSQLAnalyticSolution",
        "publisher": "[variables('SolutionPublisher')]",
        "promotionCode": ""
    }

### <a name="solution-name"></a>Lösungsnamen
Der Lösungsnamen muss in Ihrem Abonnement Azure eindeutig sein. Verwenden der empfohlene Wert ist vor.  Hier wird eine Variable namens **SolutionName** für den Namen und den Parameter **ein** , um sicherzustellen, dass der Name eindeutig ist.

    [concat(variables('SolutionName'), ' [' ,parameters('workspaceName'), ']')]

Dies würde einen Namen wie folgt aufgelöst.

    My Solution Name [MyWorkspace]
 

### <a name="dependencies"></a>Abhängigkeiten
Die Lösung Resource müssen eine [Abhängigkeit](../resource-group-define-dependencies.md) für jede andere Ressource in die Lösung, da diese müssen vorhanden sein, damit die Lösung erstellt werden kann.  Dazu müssen Sie einen Eintrag für jede Ressource im **DependsOn** Element hinzufügen.


### <a name="properties"></a>Eigenschaften
Die Lösung Resource verfügt über die Eigenschaften in der folgenden Tabelle.  Dies umfasst die Ressourcen, auf die verwiesen wird, und enthalten sind, indem Sie die Lösung, die definiert, wie die Ressource verwaltet wird, wenn die Lösung installiert ist.  Jede Ressource in die Lösung sollte entweder die **ReferencedResources** oder die Eigenschaft **ContainedResources** aufgeführt sein.

| Eigenschaft | Beschreibung |
|:--|:--|
| workspaceResourceId | ID des Arbeitsbereichs OMS im Formular * <Resource Group ID>/providers/Microsoft.OperationalInsights/workspaces/\<Arbeitsbereichsnamen\>*. |
| referencedResources   | Liste der Ressourcen in die Lösung, die nicht entfernt werden soll, wenn die Lösung entfernt wird. |
| containedResources   | Liste der Ressourcen in die Lösung, die entfernt werden soll, wenn die Lösung entfernt wird. |

Im oben genannten Beispiel ist für eine Lösung mit einer Runbooks, einen Zeitplan und anzeigen.  Zeitplan und Runbooks werden *referenzierte* **Eigenschaften** im Element, sodass sie nicht entfernt werden, wenn die Lösung entfernt wird.  Die Sicht ist *enthalten* , damit sie entfernt wird, wenn die Lösung entfernt wird.


### <a name="plan"></a>Planen
**Planen** der Lösung Ressource Entität verfügt über die Eigenschaften in der folgenden Tabelle. 

| Eigenschaft | Beschreibung |
|:--|:--|
| Namen          | Name der Lösung. |
| Version       | Version der Lösung wie vom Autor bestimmt. |
| Produkt       | Eindeutige Zeichenfolge zum Identifizieren der Lösung. |
| Publisher     | Herausgeber der Lösung. |


## <a name="other-resources"></a>Weitere Ressourcen
Sie können die Details und Beispiele für Ressourcen, die in den folgenden Artikeln Management-Lösungen feldbezogene erhalten.

- [Ansichten und dashboards](operations-management-suite-solutions-resources-views.md)
- [Automatisierung Ressourcen](operations-management-suite-solutions-resources-automation.md)

## <a name="testing-a-management-solution"></a>Testen einer Lösung
Vor der Bereitstellung Ihrer Lösung, empfiehlt es sich, dass Sie sie mithilfe von [Test-AzureRmResourceGroupDeployment](../resource-group-template-deploy.md#deploy-with-powershell)testen.  Dies wird Überprüfen Ihrer Lösung Datei- und die Hilfe, die Sie Probleme erkennen, bevor Sie versuchen, diese bereitstellen.



## <a name="next-steps"></a>Nächste Schritte

- Hier erfahren Sie die Details der [Azure Ressourcenmanager Authoring-Vorlagen](../resource-group-authoring-templates.md).
- Suche [Azure Schnellstart Vorlagen](https://azure.microsoft.com/documentation/templates) Beispiele für unterschiedliche Ressourcenmanager Vorlagen.
- Zeigen Sie die Details zum [Hinzufügen von Ansichten in einer Lösung](operations-management-suite-solutions-resources-views.md).
- Zeigen Sie die Details zum [Hinzufügen von Ressourcen Automatisierung einer Lösung](operations-management-suite-solutions-resources-automation.md).
 
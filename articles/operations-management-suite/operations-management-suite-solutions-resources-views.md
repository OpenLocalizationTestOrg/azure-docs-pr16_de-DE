<properties
   pageTitle="Ansichten in Management-Lösungen für Vorgänge Management Suite (OMS) | Microsoft Azure"
   description="Projektmanagement-Lösungen in Vorgänge Management Suite (OMS) werden in der Regel eine oder mehrere Ansichten zum Darstellen der Daten sind.  Dieser Artikel beschreibt, wie eine Ansicht der Ansicht-Designer erstellte exportieren und in einer Lösung aufnehmen. "
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
   ms.date="10/17/2016"
   ms.author="bwren" />

# <a name="views-in-operations-management-suite-oms-management-solutions-preview"></a>Ansichten in Management-Lösungen für Vorgänge Management Suite (OMS) (Preview)

>[AZURE.NOTE]Dies ist über der vorläufigen Dokumentation zum Erstellen von Lösungen Management in OMS die derzeit in der Vorschau sind. Alle nachfolgend beschriebenen Schema unterliegt ändern.    

[Projektmanagement-Lösungen in Vorgänge Management Suite (OMS)](operations-management-suite-solutions.md) wird in der Regel eine oder mehrere Ansichten zum Darstellen der Daten sind.  Dieser Artikel beschreibt, wie eine Ansicht der [Ansicht-Designer](../log-analytics/log-analytics-view-designer.md) erstellte exportieren und in einer Lösung einzuschließen.  

>[AZURE.NOTE]In den Beispielen in diesem Artikel verwenden, Parameter und Variablen, die sind entweder erforderlich oder gemeinsam Management-Lösungen und beschrieben [Erstellen Management Solutions in Vorgänge Management Suite (OMS)](operations-management-suite-solutions-creating.md) 


## <a name="prerequisites"></a>Erforderliche Komponenten
In diesem Artikel wird vorausgesetzt, dass Sie bereits [eine Lösung erstellen](operations-management-suite-solutions-creating.md) und die Struktur einer Lösung Datei wie vertraut sind.


## <a name="overview"></a>(Übersicht)

Wenn eine Ansicht in einer Lösung einbeziehen möchten, erstellen Sie eine **Ressource** dafür in der [Lösungsdatei](operations-management-suite-solutions-creating.md)ein.  Die JSON, die detaillierte Konfiguration der Ansicht beschreibt ist in der Regel komplexe durch und nicht eines Beitrags, dass ein typische Lösung Autor manuell erstellt wäre.  Die am häufigsten verwendete Methode ist in der Ansicht mit dem [Ansicht-Designer](../log-analytics/log-analytics-view-designer.md)erstellen, exportieren es und fügen Sie anschließend die detaillierte Konfiguration der Lösung. 

Die grundlegenden Schritte zum Hinzufügen einer Ansicht in eine Lösung werden wie folgt aus.  Jeder Schritt wird ausführlicher in den folgenden Abschnitten beschrieben.

1. Exportieren Sie die Ansicht in eine Datei.
2. Erstellen der Ansicht Ressource in die Lösung.
3. Fügen Sie die Details anzeigen.

## <a name="export-the-view-to-a-file"></a>Die Ansicht in eine Datei exportieren
Folgen Sie den Anweisungen am [Log Analytics Ansicht-Designer](../log-analytics/log-analytics-view-designer.md) zu eine Ansicht in eine Datei exportieren.  Die exportierte Datei werden im JSON-Format mit den gleichen [Elemente wie die Lösungsdatei](operations-management-suite-solutions-creating.md#management-solution-files).  

Das Element **Ressourcen** der Datei anzeigen, eine Ressource für einen Typ von **Microsoft.OperationalInsights/workspaces** müssen, die den Arbeitsbereich OMS darstellt.  Dieses Element wird ein Unterelement mit einem Typ von **Ansichten** verfügen, die die Ansicht darstellt und die ausführliche Konfiguration enthält.  Sie die Details dieses Element kopieren und kopieren Sie ihn dann in der Lösung.


## <a name="create-the-view-resource-in-the-solution"></a>Erstellen der Ansicht Ressource in die Lösung
Die folgende Ansicht Ressource zum Element **Ressourcen** Ihrer Lösung-Datei hinzufügen.  Dies wird verwendet, Variablen, die unter beschrieben sind, müssen Sie auch hinzufügen.  Beachten Sie, dass die Eigenschaften **Dashboard** und **OverviewTile** Platzhalter sind, die Sie mit den entsprechenden Eigenschaften aus der exportierte Ansichtsdatei überschrieben werden.
 
    {
        "apiVersion": "[variables('LogAnalyticsApiVersion')]",
        "name": "[concat(parameters('workspaceName'), '/', variables('ViewName'))]",
        "type": "Microsoft.OperationalInsights/workspaces/views",
        "location": "[parameters('workspaceregionId')]",
        "id": "[Concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'),'/views/', variables('ViewName'))]",
        dependson": [
            ],
        "properties": {
            "Id": "[variables('ViewName')]",
            "Name": "[variables('ViewName')]",
            "DisplayName": "[variables('ViewName')]",
            "Description": "",
            "Author": "[variables('ViewAuthor')]",
            "Source": "Local",
            "Dashboard": ,
            "OverviewTile": 
        }
    }

Das Element [Variablen](operations-management-suite-solutions-creating.md#variables) der Lösungsdatei die folgenden Variablen hinzu, und Ersetzen Sie die Werte auf, die für Ihre Lösung.

    "LogAnalyticsApiVersion": "2015-11-01-preview",
    "ViewAuthor": "Your name."
    "ViewDescription": "Optional description of the view."
    "ViewName": "Provide a name for the view here."


Beachten Sie, dass Sie können die gesamte Ansicht Ressource aus der exportierte Ansichtsdatei kopieren, aber Sie dafür entwickelt in Ihre Lösung folgende Änderungen vornehmen müssen.  

- Für die Ansicht Ressource der **Art** muss von **Ansichten** in **Microsoft.OperationalInsights/workspaces**geändert werden.
- Die **Name** -Eigenschaft für die Ansicht Ressource muss geändert werden, um den Arbeitsbereich einfügen.
- Die Abhängigkeit von der Arbeitsbereich muss entfernt werden, da die Arbeitsbereich Ressource in die Lösung definiert nicht zur Verfügung.
- **DisplayName** -Eigenschaft muss zur Ansicht hinzugefügt werden.  Die **Id**, **Name**und **DisplayName** müssen alle entsprechen.
- Parameternamen müssen geändert werden, um die erforderlichen Gruppe von Parametern entsprechen.
- Variablen sollten in die Lösung definiert und in den entsprechenden Eigenschaften verwendet werden.

## <a name="add-the-view-details"></a>Hinzufügen der Anzeigen von details
Die Ansicht Ressource in der exportierte Ansichtsdatei werden zwei Elemente in der **Properties** -Element mit dem Namen **Dashboard** und **OverviewTile** enthalten, die die detaillierte Konfiguration der Ansicht umfassen.  Kopieren Sie diese beiden Elemente und deren Inhalt in das Element **Eigenschaften** der Ansicht Ressource in der Datei Lösung. 

## <a name="example"></a>Beispiel
Im folgende Beispiel zeigt beispielsweise eine einfache Lösung-Datei mit einer Ansicht.  Drei Punkte (...) für den Inhalt **Dashboard** und **OverviewTile** Gründen Leerzeichen dargestellt werden.


    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "workspaceName": {
                "type": "string"
            },
            "accountName": {
                "type": "string"
            },
            "workspaceRegionId": {
                "type": "string"
            },
            "regionId": {
                "type": "string"
            },
            "pricingTier": {
                "type": "string"
            }
        },
        "variables": {
            "SolutionVersion": "1.1",
            "SolutionPublisher": "Contoso",
            "SolutionName": "Contoso Solution",
            "LogAnalyticsApiVersion": "2015-11-01-preview",
            "ViewAuthor":  "user@contoso.com",
            "ViewDescription":  "This is a sample view.",
            "ViewName":  "Contoso View"
        },
        "resources": [
            {
                "name": "[concat(variables('SolutionName'), '(' ,parameters('workspacename'), ')')]",
                "location": "[parameters('workspaceRegionId')]",
                "tags": { },
                "type": "Microsoft.OperationsManagement/solutions",
                "apiVersion": "[variables('LogAnalyticsApiVersion')]",
                "dependsOn": [
                    "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspacename'), '/views/', variables('ViewName'))]"
                ],
                "properties": {
                    "workspaceResourceId": "[concat(resourceGroup().id, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspacename'))]",
                    "referencedResources": [
                    ],
                    "containedResources": [
                        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/views/', variables('ViewName'))]"
                    ]
                },
                "plan": {
                    "name": "[concat(variables('SolutionName'), '(' ,parameters('workspaceName'), ')')]",
                    "Version": "[variables('SolutionVersion')]",
                    "product": "ContosoSolution",
                    "publisher": "[variables('SolutionPublisher')]",
                    "promotionCode": ""
                }
            },
            {
                "apiVersion": "[variables('LogAnalyticsApiVersion')]",
                "name": "[concat(parameters('workspaceName'), '/', variables('ViewName'))]",
                "type": "Microsoft.OperationalInsights/workspaces/views",
                "location": "[parameters('workspaceregionId')]",
                "id": "[Concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'),'/views/', variables('ViewName'))]",
                "dependson": [
                ],
                "properties": {
                    "Id": "[variables('ViewName')]",
                    "Name": "[variables('ViewName')]",
                    "DisplayName": "[variables('ViewName')]",
                    "Description": "[variables('ViewDescription')]",
                    "Author": "[variables('ViewAuthor')]",
                    "Source": "Local",
                    "Dashboard": ...,
                    "OverviewTile": ...
                }
            }
        ]
    }




## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, vollständige Details zum Erstellen von [Management-Lösungen](operations-management-suite-solutions-creating.md).
- Einschließen Sie [Automatisierung Runbooks in Ihre Management-Lösung](operations-management-suite-solutions-resources-automation.md).
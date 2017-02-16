<properties 
   pageTitle="Azure Automatisierung Runbooks Typen"
   description="Beschreibt die verschiedenen Typen von Runbooks, die Sie in Azure Automatisierung und Aspekte, die Sie berücksichtigen sollten beim Bestimmen der um zu verwendenden Typs verwenden können. "
   services="automation"
   documentationCenter=""
   authors="mgoedtel"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/12/2016"
   ms.author="bwren" />

# <a name="azure-automation-runbook-types"></a>Azure Automatisierung Runbooks Typen

Azure-Automatisierung unterstützt vier Arten von Runbooks, die kurz beschrieben sind in der folgenden Tabelle.  In den folgenden Abschnitten bieten weitere Informationen zu jedem einschließlich Aspekte auf wann verwendet.


| Typ |  Beschreibung |
|:---|:---|
| [Grafische](#graphical-runbooks) | Basierend auf Windows PowerShell und erstellt und bearbeitet vollständig in grafisch Editor Azure-Portal. | 
| [Grafische PowerShell-Workflow](#graphical-runbooks) | Basierend auf der Windows PowerShell-Workflow erstellt und bearbeitet vollständig im grafischen-Editor Azure-Portal. 
| [PowerShell](#powershell-runbooks) | Text Runbooks basierend auf der Windows PowerShell-Skript.
| [PowerShell-Workflow](#powershell-workflow-runbooks) | Text Runbooks basierend auf der Windows PowerShell-Workflow. |


## <a name="graphical-runbooks"></a>Grafische runbooks

[Graphical](automation-runbook-types.md#graphical-runbooks) und grafisch PowerShell Workflow Runbooks werden erstellt und mit dem grafischen-Editor im Portal Azure bearbeitet.  Sie können in eine Datei exportieren und dann in ein anderes Automatisierung Konto importieren, aber keine erstellen oder bearbeiten Sie sie mit einem anderen Programm.  Grafische Runbooks PowerShell-Code generieren, aber nicht direkt anzeigen oder ändern Sie den Code. Grafische Runbooks in eines der [Textformate](automation-runbook-types.md)konvertiert werden kann, noch kann in grafischen Format ein Runbooks Text konvertiert werden. Grafische Runbooks können in grafisch PowerShell Workflow Runbooks während importieren und umgekehrt konvertiert werden.

### <a name="advantages"></a>Vorteile

- Erstellen von Runbooks mit minimalen Kenntnisse [PowerShell](automation-powershell-workflow.md).
- Werden Sie visuell dargestellt Management-Prozesse aus.
- Schließen Sie anderer Runbooks als untergeordnetes Element Runbooks auf hoher Ebene Workflows erstellen ein.


### <a name="limitations"></a>Einschränkungen

- Runbooks außerhalb Azure-Portal kann nicht bearbeitet werden.
- Möglicherweise eine Code-Aktivität mit PowerShell-Code zum Durchführen von komplexe Logik erforderlich.
- Anzeigen oder bearbeiten direkt auf den PowerShell-Code, der vom Workflow grafisch erstellt wird. Beachten Sie, dass Sie den Code anzeigen können, die, den Sie in anderen Code Aktivitäten zu erstellen.


## <a name="powershell-runbooks"></a>PowerShell runbooks

PowerShell Runbooks basieren auf Windows PowerShell.  Direkt bearbeiten Sie den Code der des Runbooks Text-Editor im Portal Azure verwenden.  Sie können auch alle offline-Text-Editor und [Importieren des Runbooks](http://msdn.microsoft.com/library/azure/dn643637.aspx) in Azure Automatisierung verwenden.

### <a name="advantages"></a>Vorteile

- Implementieren Sie alle komplexe Logik mit PowerShell-Code ohne die zusätzliche Komplexität der PowerShell-Workflow an. 
- Runbooks beginnt schneller als Graphical oder PowerShell Workflow Runbooks, da es nicht vor dem Ausführen kompiliert werden muss.

### <a name="limitations"></a>Einschränkungen

- Muss mit PowerShell-Skripting vertraut sein.
- Können [parallele Verarbeitung](automation-powershell-workflow.md#parallel-processing) mehrere Aktionen parallel ausführen.
- Können [Kontrollpunkten](automation-powershell-workflow.md#checkpoints) Runbooks bei Fehler fortsetzen.
- PowerShell-Workflow Runbooks und grafisch Runbooks nur als angegeben werden untergeordnete Runbooks mithilfe des Start-AzureAutomationRunbook Cmdlets der erstellt eine neue Position.

### <a name="known-issues"></a>Bekannte Probleme
Im folgenden werden die aktuellen bekannte Probleme mit PowerShell Runbooks.

- PowerShell Runbooks kann keines unverschlüsselter [Variable Anlage](automation-variables.md) mit einem Nullwert abrufen können.
- PowerShell Runbooks kann nicht abgerufen werden eine [Variable Anlage](automation-variables.md) mit *~* in den Namen.
- Get-Prozess in einer Schleife in einer PowerShell zum Absturz Runbooks nach ungefähr 80 Iterationen. 
- Eine PowerShell Runbooks kann ein Fehler auftreten, wenn versucht wird, eine große Datenmenge in der Ausgabestream gleichzeitig zu schreiben.   Sie können in der Regel umgehen dieses Problem mit der Ausgabe nur der Informationen, die Sie beim Arbeiten mit großen Objekte müssen.  Ausgeben Sie beispielsweise ausgeben ungefähr wie folgt *Get-Prozess*, sondern nur die erforderlichen Felder mit *Get-Process | Wählen Sie ProcessName, CPU*.

## <a name="powershell-workflow-runbooks"></a>PowerShell-Workflow runbooks

PowerShell-Workflow Runbooks sind Text Runbooks basierend auf der [Windows PowerShell-Workflow](automation-powershell-workflow.md)an.  Direkt bearbeiten Sie den Code der des Runbooks Text-Editor im Portal Azure verwenden.  Sie können auch alle offline-Text-Editor und [Importieren des Runbooks](http://msdn.microsoft.com/library/azure/dn643637.aspx) in Azure Automatisierung verwenden.

### <a name="advantages"></a>Vorteile

- Implementieren Sie alle komplexe Logik mit PowerShell-Workflow-Code.
- Verwenden Sie zum Fortsetzen des Runbooks bei Fehler [Kontrollpunkten](automation-powershell-workflow.md#checkpoints) .
- Verwenden Sie [parallele Verarbeitung](automation-powershell-workflow.md#parallel-processing) , um mehrere Aktionen parallel ausführen.
- Können andere grafisch Runbooks und PowerShell Workflow Runbooks als untergeordnete Runbooks zum Erstellen von Workflows mit hoher Ebene aufnehmen möchten.


### <a name="limitations"></a>Einschränkungen

- Autor muss mit PowerShell Workflow vertraut sein.
- Die zusätzliche Komplexität der PowerShell-Workflow wie [deserialisiert Objekte](automation-powershell-workflow.md#code-changes)muss Runbooks behandelt.
- Runbooks dauert länger als PowerShell Runbooks starten, da es werden, bevor er ausgeführt wird kompiliert muss.
- PowerShell Runbooks nur als angegeben werden untergeordnete Runbooks mithilfe des Start-AzureAutomationRunbook Cmdlets der erstellt eine neue Position.


## <a name="considerations"></a>Aspekte

Sie sollten zusätzliche Folgendes berücksichtigen beim bestimmen, welche Geben Sie für einen bestimmten Runbooks verwendet werden soll.

- Sie können keine Runbooks aus grafisch in Textform Typ oder umgekehrt konvertieren.
- Es gibt mit unterschiedlichen Typs Runbooks als eine untergeordnete Runbooks Einschränkungen aus.  Weitere Informationen finden Sie unter [untergeordneten Runbooks in Azure Automatisierung](automation-child-runbooks.md) .

  
## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zur Erstellung von grafisch Runbooks finden Sie unter [Graphical authoring in Azure Automatisierung](automation-graphical-authoring-intro.md)
- Die Unterschiede zwischen der PowerShell und PowerShell kennen Workflows für Runbooks, finden Sie unter [Schulung Windows PowerShell-Workflow](automation-powershell-workflow.md)
- Weitere Informationen zum Erstellen oder Importieren einer Runbooks finden Sie unter [Erstellen oder Importieren einer Runbooks](automation-creating-importing-runbook.md)




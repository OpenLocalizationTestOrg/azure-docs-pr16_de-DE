<properties 
   pageTitle="Untergeordnete Runbooks in Azure Automatisierung | Microsoft Azure"
   description="Beschreibt die verschiedenen Methoden zum Starten einer Runbooks in Azure Automatisierung aus einer anderen Runbooks und Freigabe von Informationen zwischen ihnen an."
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
   ms.date="08/17/2016"
   ms.author="magoedte;bwren" />

# <a name="child-runbooks-in-azure-automation"></a>Untergeordnete Runbooks in Azure Automatisierung

Es ist eine bewährte Methode in Azure Automatisierung wieder verwendbare, modulare Runbooks mit einer Funktion diskrete schreiben, die von anderen Runbooks verwendet werden kann. Eine übergeordnete Runbooks ruft oft eine oder mehrere untergeordnete Runbooks um erforderliche Funktionalität ausführen. Es gibt zwei Möglichkeiten, um eine untergeordnete Runbooks anrufen, und jeder hat deutliche Unterschiede, die Sie kennen sollten, damit Sie ermitteln können, die sich am besten für die verschiedenen Szenarios gehört.

##  <a name="invoking-a-child-runbook-using-inline-execution"></a>Aufrufen eines untergeordneten Runbooks mit Inline ausführen

Um eine Runbooks Inline aus einem anderen Runbooks aufzurufen, verwenden Sie den Namen des Runbooks und geben Sie Werte für Funktionsparameter, genau wie Sie einer Aktivität oder Cmdlet verwenden würden.  Alle Runbooks in demselben Automatisierung Konto stehen alle anderen auf diese Weise verwendet werden. Des übergeordneten Runbooks wartet der untergeordneten Runbooks ausführen, bevor Sie in die nächste Zeile verschieben und keine Ausgabe direkt an das übergeordnete Element zurückgegeben.

Wenn Sie eine Runbooks Inline aufrufen, wird es in derselben Stelle als des übergeordneten Runbooks ausgeführt. Es werden keine Angabe in der Historie der des untergeordneten Runbooks, die es ausgeführt haben. Alle Ausnahmen und jeder Ausgabe aus der untergeordneten Runbooks Stream werden das übergeordnete Element zugeordnet werden. Dadurch ergibt sich weniger Aufträge und diese einfacher nachverfolgen und beheben, da alle Ausnahmen von der untergeordneten Runbooks und keines deren Streamausgabe der übergeordnete Auftrag zugeordnet sind.

Wenn eine Runbooks veröffentlicht wird, müssen alle untergeordneten Runbooks, die es ruft bereits veröffentlicht werden. Dies liegt daran Azure Automatisierung erstellt eine Verknüpfung mit einem beliebigen untergeordneten Runbooks aus, wenn eine Runbooks kompiliert wurde. Wenn sie nicht sind, des übergeordneten Runbooks veröffentlichen ordnungsgemäß angezeigt wird, aber generiert eine Ausnahme aus, wenn er gestartet wird. In diesem Fall können Sie den übergeordneten Runbooks akzeptieren, um die untergeordneten Runbooks ordnungsgemäß verweisen erneut veröffentlichen. Sie müssen nicht erneut veröffentlichen des übergeordneten Runbooks, wenn keines der untergeordneten Runbooks geändert werden, da die Zuordnung wird bereits erstellt wurden.

Die Parameter eines untergeordneten Runbooks Inline aufgerufen können einen beliebigen Datentyp aufweisen, einschließlich komplexe Objekte, und es wird beim Starten des Runbooks mithilfe der Azure-Verwaltungsportal oder mit dem Start-AzureRmAutomationRunbook-Cmdlet keine [JSON-Serialisierung](automation-starting-a-runbook.md#runbook-parameters) vorliegt.


### <a name="runbook-types"></a>Runbooks Typen

Welche Dateitypen können untereinander anrufen:

- Eine [PowerShell Runbooks](automation-runbook-types.md#powershell-runbooks) und [grafisch Runbooks](automation-runbook-types.md#graphical-runbooks) können jede andere Inline aufrufen (beide sind PowerShell basierend).
- Ein [PowerShell Workflow Runbooks](automation-runbook-types.md#powershell-workflow-runbooks) und grafisch PowerShell Workflow Runbooks kann jede andere Inline aufrufen (beide sind PowerShell-Workflow basiert)
- Der PowerShell und die Typen von PowerShell-Workflow können nicht miteinander Inline anrufen, und müssen Start-AzureRmAutomationRunbook verwenden.
    
Wenn Reihenfolge Angelegenheit veröffentlichen:

- Die Reihenfolge veröffentlichen Runbooks ist nur für die PowerShell-Workflow und grafisch PowerShell Runbooks.


Wenn Sie eine Graphical oder PowerShell Workflow untergeordneten Runbooks mit eingebetteten Ausführung anrufen möchten, verwenden Sie nur den Namen des Runbooks aus.  Wenn Sie eine PowerShell untergeordneten Runbooks anrufen möchten, müssen Sie deren Namen mit vorangestellt *.\\ * um anzugeben, dass das Skript im lokalen Verzeichnis befindet. 

### <a name="example"></a>Beispiel

Im folgenden Beispiel wird eine Test untergeordneten Runbooks, der drei Parameter, ein komplexes Objekt, eine ganze Zahl und ein boolescher akzeptiert. Die Ausgabe der untergeordneten Runbooks ist einer Variablen zugeordnet.  In diesem Fall ist der untergeordneten Runbooks einer Runbooks PowerShell-Workflow

    $vm = Get-AzureRmVM –ResourceGroupName "LabRG" –Name "MyVM"
    $output = PSWF-ChildRunbook –VM $vm –RepeatCount 2 –Restart $true

Im folgenden sehen die gleichen Beispiel für die Verwendung eines PowerShell Runbooks als untergeordnetes Element.

    $vm = Get-AzureRmVM –ResourceGroupName "LabRG" –Name "MyVM"
    $output = .\PS-ChildRunbook –VM $vm –RepeatCount 2 –Restart $true



##  <a name="starting-a-child-runbook-using-cmdlet"></a>Starten Sie eine untergeordnete Runbooks Cmdlet verwenden

Sie können mit dem [Start-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx) -Cmdlet verwenden, um eine Runbooks beginnen, in [einer Runbooks mit Windows PowerShell starten](../automation-starting-a-runbook.md#starting-a-runbook-with-windows-powershell)beschriebenen. Es gibt zwei Modi für dieses Cmdlet verwenden.  In einem einzigen Modus gibt das Cmdlet die Auftrags-Id, sobald der untergeordneten Auftrag für die untergeordneten Runbooks erstellt wird.  In den anderen Modus, die es Ihnen, indem Sie angeben ermöglichen der **-Warten** Parameter, das Cmdlet wird warten, bis der untergeordneten beruflichen Position abgeschlossen ist, und gibt das Ergebnis aus der untergeordneten Runbooks zurück.

Die Position aus einer untergeordneten Runbooks Schritte mit einem Cmdlet wird in einem separaten Auftrag von der übergeordneten Runbooks ausgeführt. Dadurch ergibt sich weitere Aufträge als den Runbooks Inline aufrufen und diese schwieriger zu verfolgen. Das übergeordnete Element kann mehrere untergeordnete Runbooks asynchrone beginnen, ohne warten auf jede ausführen. Für die gleiche Art der parallelen Ausführung den untergeordneten Runbooks Inline aufrufen müssten des übergeordneten Runbooks das [Schlüsselwort parallele](automation-powershell-workflow.md#parallel-processing)verwenden.

Parameter für eine untergeordnete Runbooks Schritte mit einem Cmdlet dienen als Hashtable, wie in [Runbooks Parameter](automation-starting-a-runbook.md#runbook-parameters)beschrieben. Nur einfache Datentypen können verwendet werden. Wenn des Runbooks ein Parameters mit einem komplexen Datentyp hat, muss es Inline aufgerufen werden.

### <a name="example"></a>Beispiel

Im folgende Beispiel wird eine untergeordnete Runbooks mit den Parametern gestartet und wartet dann, bis er mit dem Start-AzureRmAutomationRunbook abgeschlossen-Parameter warten. Nachdem abgeschlossen ist, werden deren Ausgabe aus der untergeordneten Runbooks erfasst.

    $params = @{"VMName"="MyVM";"RepeatCount"=2;"Restart"=$true} 
    $joboutput = Start-AzureRmAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-ChildRunbook" -ResouceGroupName "LabRG" –Parameters $params –wait


## <a name="comparison-of-methods-for-calling-a-child-runbook"></a>Vergleich der verschiedenen Methoden zum Aufrufen von einer untergeordneten Runbooks

In der folgenden Tabelle werden die Unterschiede zwischen den beiden Methoden zum Aufrufen von einer Runbooks aus einem anderen Runbooks zusammengefasst.

| | Inline| Cmdlet|
|:---|:---|:---|
|Position|Untergeordnete Runbooks ausführen in derselben Stelle als übergeordnetes Element.|Ein separaten Auftrag wird für die untergeordneten Runbooks erstellt.|
|Ausführung|Übergeordnete Runbooks wartet der untergeordneten Runbooks zum Abschließen, bevor Sie fortfahren.|Übergeordnete Runbooks weiterhin sofort nach untergeordneten Runbooks gestartet wird, dass wartet *oder* übergeordneten Runbooks für das Projekt untergeordneten auf Fertig stellen.|
|Die Ausgabe|Übergeordnete Runbooks kann Ausgabe direkt von untergeordneten Runbooks erhalten.|Übergeordnete Runbooks abrufen muss Ausgabe aus der untergeordneten Runbooks Stelle *oder* übergeordneten Runbooks direkt bekomme Ausgabe von untergeordneten Runbooks.|
|Parameter|Werte für die untergeordneten Runbooks Parameter getrennt angegeben werden, und können Sie einen beliebigen Datentyp aufweisen.|Array-Werte für die untergeordneten Runbooks Parameter in einer einzelnen Hashtable kombiniert werden müssen und können nur einfache, enthalten, und die Nutzung von JSON-Serialisierung für Objekt-Datentypen.|
|Automatisierung-Konto|Übergeordnete Runbooks kann nur untergeordnete Runbooks in demselben Automatisierung Konto verwenden.|Übergeordnete Runbooks können untergeordnete Runbooks aus einem beliebigen Konto Automatisierung aus dem gleichen Azure-Abonnement und sogar ein anderes Abonnement aus, wenn Sie eine Verbindung dazu haben.|
|Veröffentlichen|Untergeordnete Runbooks muss veröffentlicht werden, bevor übergeordneten Runbooks veröffentlicht wird.|Untergeordnete Runbooks muss die jederzeit vor dem Starten der übergeordneten Runbooks veröffentlicht werden.|

## <a name="next-steps"></a>Nächste Schritte

- [Starten Sie eine Runbooks in Azure Automatisierung](automation-starting-a-runbook.md)
- [Runbooks Ausgabe und Nachrichten in Azure Automatisierung](automation-runbook-output-and-messages.md)

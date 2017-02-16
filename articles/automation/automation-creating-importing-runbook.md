<properties
    pageTitle="Erstellen oder Importieren einer Runbooks in Azure Automatisierung"
    description="In diesem Artikel werden die Informationen zum Erstellen einer neuen Runbooks in Azure Automatisierung eins aus einer Datei importieren."
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
    ms.author="magoedte;bwren" />

# <a name="creating-or-importing-a-runbook-in-azure-automation"></a>Erstellen oder Importieren einer Runbooks in Azure Automatisierung

Sie können eine Runbooks Azure Automatisierung indem entweder [einen neuen zu erstellen](#creating-a-new-runbook) oder importieren eine vorhandene Runbooks aus einer Datei oder aus dem [Katalog Runbooks](automation-runbook-gallery.md)hinzufügen. Dieser Artikel enthält Informationen zum Erstellen und Runbooks aus einer Datei importieren.  Sie können alle Details zum Zugriff auf Community Runbooks und Module in [Runbooks und Modul Kataloge für Azure Automatisierung](automation-runbook-gallery.md)erhalten.

## <a name="creating-a-new-runbook"></a>Erstellen einer neuen Runbooks

Sie können eine neue Runbooks in Azure Automatisierung mithilfe einer der Azure Portals oder Windows PowerShell erstellen. Sobald die Runbooks erstellt wurde, können Sie sie mithilfe von Informationen in [Learning PowerShell Workflow](automation-powershell-workflow.md) und [grafisch authoring in Azure Automatisierung](automation-graphical-authoring-intro.md)bearbeiten.

### <a name="to-create-a-new-azure-automation-runbook-with-the-azure-classic-portal"></a>So erstellen Sie eine neue Azure Automatisierung Runbooks mit klassischen Azure-portal

Sie können nur mit [PowerShell Workflow Runbooks](automation-runbook-types.md#powershell-workflow-runbooks) Azure-Portal arbeiten.

1. In der klassischen Azure-Portal klicken Sie auf, **neue** **App Services**, **Automatisierung**, **Runbooks**, **Schnellen Erstellen**.
2. Geben Sie die erforderlichen Informationen ein, und klicken Sie dann auf **Erstellen**. Der Name des Runbooks kann muss mit einem Buchstaben anfangen und Buchstaben, Zahlen, Unterstriche und Striche.
3. Wenn Sie als Nächstes bearbeiten Sie die Runbooks möchten, klicken Sie dann auf **Runbooks bearbeiten**. Klicken Sie andernfalls auf **OK**.
4. Klicken Sie auf der Registerkarte **Runbooks** wird der neue Runbooks angezeigt.


### <a name="to-create-a-new-azure-automation-runbook-with-the-azure-portal"></a>So erstellen Sie eine neue Azure Automatisierung Runbooks mit Azure-portal

1. Öffnen Sie Ihr Konto Automatisierung der Azure-Portal.
2. Klicken Sie auf die Kachel **Runbooks** zum Öffnen der Liste von Runbooks.
3. Klicken Sie auf die Schaltfläche **Hinzufügen einer Runbooks** und klicken Sie dann auf **Erstellen einer neuen Runbooks**.
2. Geben Sie einen **Namen** für die Runbooks, und wählen Sie seinen [Typ](automation-runbook-types.md). Der Name des Runbooks kann muss mit einem Buchstaben anfangen und Buchstaben, Zahlen, Unterstriche und Striche.
3. Klicken Sie auf **Erstellen** , um des Runbooks erstellen, und öffnen Sie den Editor.


### <a name="to-create-a-new-azure-automation-runbook-with-windows-powershell"></a>So erstellen Sie eine neue Azure Automatisierung Runbooks mit Windows PowerShell

Das Cmdlet " [New-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt619376.aspx) " können Sie um eine leere [Runbooks PowerShell Workflow](automation-runbook-types.md#powershell-workflow-runbooks)zu erstellen. Sie können entweder den **Namen** einer leeren Runbooks zu erstellen, die Sie später bearbeiten können Parameter angeben, oder Sie können angeben, dass des **Pfad** Parameters eine Runbooks-Datei importieren. **Type** -Parameter sollten auch zum Angeben einer von vier Typen Runbooks aufgenommen werden.

Im folgenden Beispielbefehle anzeigen zum Erstellen einer neuen leeren Runbooks

    New-AzureRmAutomationRunbook -AutomationAccountName MyAccount `
    -Name NewRunbook -ResourceGroupName MyResourceGroup -Type PowerShell

## <a name="importing-a-runbook-from-a-file-into-azure-automation"></a>Importieren einer Runbooks aus einer Datei in Azure Automatisierung

Sie können eine neue Runbooks in Azure Automatisierung durch Importieren einer PowerShell-Skript oder PowerShell Workflow (mit der Erweiterung. ps1) oder eine exportierte grafisch Runbooks (.graphrunbook) erstellen.  Sie müssen den [Typ des Runbooks](automation-runbook-types.md) angeben, die aus dem Import Berücksichtigung unter folgenden Aspekten erstellt wird.

- Eine Datei .graphrunbook kann nur in einer neuen [grafischen Runbooks](automation-runbook-types.md#graphical-runbooks)importiert werden, und grafisch Runbooks können nur aus einer Datei .graphrunbook erstellt werden.
- Eine. ps1-Datei mit einem PowerShell Workflow kann nur in einer [PowerShell Workflow Runbooks](automation-runbook-types.md#powershell-workflow-runbooks)importiert werden.  Wenn die Datei mehrere PowerShell Workflows enthält, wird der Import fehl. Sie müssen jeden Workflow in einem eigenen Datei speichern und jeder separat importieren.
- Eine PS1-Datei, die nicht mit einen Workflow enthält, kann in entweder eine [PowerShell Runbooks](automation-runbook-types.md#powershell-runbooks) oder einen [PowerShell Workflow Runbooks](automation-runbook-types.md#powershell-workflow-runbooks)importiert werden.  Wenn es nach einer PowerShell Workflow Runbooks importiert werden, klicken Sie dann an einen Workflow konvertiert werden sollen, und Kommentare werden in der Angabe der vorgenommenen Änderungen Runbooks enthalten sein.

### <a name="to-import-a-runbook-from-a-file-with-the-azure-classic-portal"></a>So importieren Sie eine Runbooks aus einer Datei mit dem klassischen Azure-portal
Das folgende Verfahren können um eine Skriptdatei in Azure Automatisierung zu importieren.  Beachten Sie, dass Sie nur eine. ps1-Datei in ein PowerShell Workflow Runbooks mithilfe dieses Portal importieren können.  Verwenden Sie für andere Arten das Azure-Portal an.

1. Aktivieren Sie im Azure-Verwaltungsportal **Automatisierung,** und wählen Sie das Konto Automatisierung.
2. Klicken Sie auf **Importieren**.
3. Klicken Sie auf **Durchsuchen, für die Datei** , und suchen Sie die zu importierende Skriptdatei.
4. Wenn Sie als Nächstes bearbeiten Sie die Runbooks möchten, klicken Sie dann auf **Runbooks bearbeiten**. Klicken Sie andernfalls auf OK.
5. Die neue Runbooks wird auf der Registerkarte **Runbooks** für das Konto Automatisierung angezeigt.
6. Sie müssen [Veröffentlichen des Runbooks](#publishing-a-runbook) , bevor er ausgeführt werden kann.


### <a name="to-import-a-runbook-from-a-file-with-the-azure-portal"></a>So importieren Sie eine Runbooks aus einer Datei mit der Azure-portal
Das folgende Verfahren können um eine Skriptdatei in Azure Automatisierung zu importieren.  

>[AZURE.NOTE] Beachten Sie, dass Sie nur eine. ps1-Datei in einer PowerShell Workflow Runbooks mit dem Portal importieren können.

1. Öffnen Sie Ihr Konto Automatisierung der Azure-Portal.
2. Klicken Sie auf die Kachel **Runbooks** zum Öffnen der Liste von Runbooks.
3. Klicken Sie auf die Schaltfläche **Hinzufügen einer Runbooks** und dann auf **Importieren**.
4. Klicken Sie auf **Datei Runbooks** , um die zu importierende Datei auswählen
2. Wenn das Feld " **Name** " aktiviert ist, müssen Sie die Option, um ihn zu ändern.  Der Name des Runbooks kann muss mit einem Buchstaben anfangen und Buchstaben, Zahlen, Unterstriche und Striche.
3. Den [Typ des Runbooks](automation-runbook-types.md) automatisch ausgewählt haben, aber Sie können die Art ändern, unter Berücksichtigung der anwendbare Einschränkungen berücksichtigt. 
3. Die neue Runbooks wird in der Liste der Runbooks für das Konto Automatisierung angezeigt.
4. Sie müssen [Veröffentlichen des Runbooks](#publishing-a-runbook) , bevor er ausgeführt werden kann.

>[AZURE.NOTE] Nachdem Sie eine grafisch Runbooks oder einer grafisch PowerShell Workflow Runbooks importiert haben, haben Sie die Möglichkeit, auf den anderen Typ zu konvertieren, wenn gewünscht. Sie können nicht in Text konvertieren.

### <a name="to-import-a-runbook-from-a-script-file-with-windows-powershell"></a>So importieren Sie eine Runbooks aus einem mit Windows PowerShell-Skriptdatei

Mit dem [Import-AzureRMAutomationRunbook](https://msdn.microsoft.com/library/mt603735.aspx) -Cmdlet können Sie eine Skriptdatei als Entwurf Runbooks PowerShell Workflow importieren. Des Runbooks bereits vorhanden ist, schlägt der Import fehl, es sei denn, Sie verwenden die *-Force* Parameter. 

Im folgenden Beispielbefehle zeigen, wie eine Skriptdatei in einer Runbooks importieren.

    $automationAccountName =  "AutomationAccount"
    $runbookName = "Sample_TestRunbook"
    $scriptPath = "C:\Runbooks\Sample_TestRunbook.ps1"
    $RGName = "ResourceGroup"

    Import-AzureRMAutomationRunbook -Name $runbookName -Path $scriptPath `
    -ResourceGroupName $RGName -AutomationAccountName $automationAccountName `
    -Type PowerShellWorkflow 


## <a name="publishing-a-runbook"></a>Veröffentlichen einer Runbooks

Beim Erstellen oder importieren eine neue Runbooks, müssen Sie es veröffentlichen, bevor er ausgeführt werden kann.  Jede Runbooks in Automatisierung hat einen Entwurf und eine veröffentlichte Version. Steht nur für die veröffentlichte Version ausgeführt werden soll, und nur die Entwurfsversion bearbeitet werden kann. Die veröffentlichte Version ist von Änderungen an der Entwurfsversion nicht betroffen. Wenn die Entwurfsversion verfügbar gemacht werden sollen, veröffentlichen Sie ihn die veröffentlichte Version mit der Entwurfsversion überschreibt.

## <a name="to-publish-a-runbook-using-the-azure-classic-portal"></a>Veröffentlichen einer Runbooks über das klassische Azure-portal

1. Öffnen des Runbooks im klassischen Azure-Portal an.
1. Klicken Sie am oberen Rand des Bildschirms auf **Autor**.
1. Klicken Sie am unteren Rand des Bildschirms die Überprüfungsnachricht **Veröffentlichen** und dann auf **Ja** .

## <a name="to-publish-a-runbook-using-the-azure-portal"></a>Veröffentlichen einer Runbooks über das Azure-portal

1. Öffnen des Runbooks Azure-Portal an.
1. Klicken Sie auf die Schaltfläche **Bearbeiten** .
1. Klicken Sie auf das **Veröffentlichen** Schaltfläche und dann auf **Ja** , um die Überprüfungsnachricht.


## <a name="to-publish-a-runbook-using-windows-powershell"></a>Veröffentlichen einer Runbooks mithilfe von Windows PowerShell

Mit dem [Veröffentlichen-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603705.aspx) -Cmdlet können Sie um eine Runbooks mit Windows PowerShell zu veröffentlichen. Im folgenden Beispielbefehle zeigen, wie eine Stichprobe Runbooks veröffentlichen.

    $automationAccountName =  AutomationAccount"
    $runbookName = "Sample_TestRunbook"
    $RGName = "ResourceGroup"

    Publish-AzureRmAutomationRunbook -AutomationAccountName $automationAccountName `
    -Name $runbookName -ResourceGroupName $RGName


## <a name="next-steps"></a>Nächste Schritte
- Informationen darüber, wie Sie aus dem Runbooks und PowerShell-Modul Katalog profitieren können, finden Sie unter [Runbooks und Modul Kataloge für Azure Automatisierung](automation-runbook-gallery.md)
- Weitere Informationen zum Bearbeiten der PowerShell und PowerShell Workflow Runbooks mit einem Text-Editor finden Sie unter [Bearbeiten von Textform Runbooks in Azure Automatisierung](automation-edit-textual-runbook.md)
- Weitere Informationen zur Erstellung von grafisch Runbooks finden Sie unter [Graphical authoring in Azure Automatisierung](automation-graphical-authoring-intro.md)

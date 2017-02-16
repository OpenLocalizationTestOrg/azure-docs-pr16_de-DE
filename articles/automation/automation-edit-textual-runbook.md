<properties 
    pageTitle="Bearbeiten von Textform Runbooks in Azure Automatisierung"
    description="Dieser Artikel bietet verschiedene Verfahren für das Arbeiten mit PowerShell und PowerShell Workflow Runbooks in Azure Automatisierung mit dem Text-Editor."
    services="automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="stevenka"
    editor="tysonn" />
<tags 
    ms.service="automation"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="02/23/2016"
    ms.author="magoedte;bwren" />

# <a name="editing-textual-runbooks-in-azure-automation"></a>Bearbeiten von Textform Runbooks in Azure Automatisierung

Im Texte-Editor in Azure Automatisierung kann zum Bearbeiten der [PowerShell Runbooks](automation-runbook-types.md#powershell-runbooks) und [PowerShell Workflow Runbooks](automation-runbook-types.md#powershell-workflow-runbooks)verwendet werden. Dies hat der Standardfeatures von anderen Code-Editoren wie Intellisense und Syntax in Farbe mit zusätzlichen speziellen Features zur Unterstützung bei der Zugriff auf Ressourcen gemeinsam Runbooks.  Dieser Artikel enthält die detaillierten Schritte zur Durchführung verschiedene Funktionen, die mit diesem Editor.

Im Texte-Editor umfasst eine Funktion zum Einfügen von Code für Aktivitäten, Ressourcen und untergeordneten Runbooks in einer Runbooks. Anstatt mit der Eingabe im Code selbst, können Sie aus einer Liste der verfügbaren Ressourcen auswählen und den entsprechenden Code in des Runbooks eingefügt haben.

Jede Runbooks in Azure Automatisierung verfügt über zwei Versionen, Text- und veröffentlicht. Sie bearbeiten die Entwurfsversion der des Runbooks, und klicken Sie dann veröffentlichen, damit es ausgeführt werden kann. Die veröffentlichte Version kann nicht bearbeitet werden. Weitere Informationen finden Sie unter [Veröffentlichen einer Runbooks](automation-creating-importing-runbook.md#publishing-a-runbook) .

Zum Arbeiten mit [Grafiken Runbooks](automation-runbook-types.md#graphical-runbooks)finden Sie unter [Graphical authoring in Azure Automatisierung](automation-graphical-authoring-intro.md).

## <a name="to-edit-a-runbook-with-the-azure-portal"></a>So bearbeiten Sie eine Runbooks mit Azure-portal

Gehen Sie folgendermaßen vor, um eine Runbooks zur Bearbeitung im Text-Editor zu öffnen.

1. Wählen Sie im Portal Azure Ihr Konto Automatisierung aus.
2. Klicken Sie auf die Kachel **Runbooks** , um die Liste der Runbooks zu öffnen.
3. Klicken Sie auf den Namen des Runbooks, die Sie bearbeiten, und klicken Sie dann auf die Schaltfläche **Bearbeiten** möchten.
6. Führen Sie die erforderlichen bearbeiten.
7. Wenn die Bearbeitung abgeschlossen haben, klicken Sie auf **Speichern** .
8. Wenn Sie die neueste Entwurfsversion der des Runbooks veröffentlicht werden soll, klicken Sie auf **Veröffentlichen** .

### <a name="to-insert-a-cmdlet-into-a-runbook"></a>So fügen Sie einer Runbooks ein cmdlet

2. Positionieren Sie den Cursor in den Zeichenbereich im Text-Editor in dem Sie das Cmdlet platzieren möchten.
3. Erweitern Sie in der Bibliothek Steuerelement **Cmdlets** . 
3. Erweitern Sie das Modul, enthält das Cmdlet aus, die, das Sie verwenden möchten.
4. Klicken Sie mit der nach rechts auf das Cmdlet zum Einfügen, und wählen Sie **zur Zeichnungsbereich hinzufügen**.  Wenn Sie das Cmdlet mehrere Parameter festgelegt wurde, wird die Standardgruppe hinzugefügt werden.  Sie können auch das Cmdlet zum Auswählen einer anderen Parameter Gruppe erweitern.
4. Der Code für das Cmdlet wird für die gesamte Liste der Parameter eingefügt.
5. Geben Sie einen geeigneten Wert anstelle der Datentyp, der von geschweiften Klammern <> für alle erforderlichen Parameter ein.  Entfernen Sie alle Parameter, die Sie nicht benötigen.

### <a name="to-insert-code-for-a-child-runbook-into-a-runbook"></a>Zum Einfügen von Code für eine untergeordnete Runbooks in einer Runbooks

2. Positionieren Sie den Cursor in den Zeichenbereich im Text-Editor in dem Sie den Code für die [untergeordneten Runbooks](automation-child-runbooks.md)platzieren möchten.
3. Erweitern Sie in der Bibliothek Steuerelement **Runbooks** . 
3. Klicken Sie mit der nach rechts auf des Runbooks, einfügen, und wählen Sie **zu Zeichnungsbereich hinzufügen**.
4. Der Code für die untergeordneten Runbooks mit alle Platzhalter für alle Parameter Runbooks eingefügt.
5. Ersetzen Sie die Platzhalter mit den entsprechenden Werten für jeden Parameter aus.

### <a name="to-insert-an-asset-into-a-runbook"></a>Zum Einfügen einer Anlageguts in einer Runbooks

2. Positionieren Sie den Cursor in den Zeichenbereich im Text-Editor in dem Sie den Code für die untergeordneten Runbooks platzieren möchten.
3. Erweitern Sie in der Bibliothek Steuerelement **Posten** . 
4. Erweitern Sie für die Art der Anlage, die Sie möchten.
3. Nach rechts klicken Sie auf die Anlage, einfügen, und wählen Sie **zu Zeichnungsbereich hinzufügen**.  Wählen Sie für [Variable Posten](automation-variables.md)je nachdem, ob Sie erhalten, oder legen Sie die Variable **Hinzufügen "erste Variable" in den Zeichnungsbereich** oder **"Workflowvariable festlegen", um den Zeichnungsbereich hinzufügen** .
4. Der Code für die Anlage wird in der Runbooks eingefügt.



## <a name="to-edit-a-runbook-with-the-azure-portal"></a>So bearbeiten Sie eine Runbooks mit Azure-portal

Gehen Sie folgendermaßen vor, um eine Runbooks zur Bearbeitung im Text-Editor zu öffnen.

1. Aktivieren Sie **Automatisierung,** und klicken Sie dann auf den Namen eines Kontos Automatisierung, im Azure-Portal.
2. Wählen Sie die Registerkarte **Runbooks** aus.
3. Klicken Sie auf den Namen des Runbooks, die Sie bearbeiten, und wählen Sie dann auf der Registerkarte **Erstellen** möchten.
5. Klicken Sie auf die Schaltfläche " **Bearbeiten** " am unteren Rand des Bildschirms.
6. Führen Sie die erforderlichen bearbeiten.
7. Wenn die Bearbeitung abgeschlossen haben, klicken Sie auf **Speichern** .
8. Wenn Sie möchten, dass die neueste Entwurfsversion der des Runbooks veröffentlicht werden soll, klicken Sie auf **Veröffentlichen** .

### <a name="to-insert-an-activity-into-a-runbook"></a>So fügen Sie einer Runbooks eine Aktivität

1. Positionieren Sie den Cursor in den Zeichenbereich im Text-Editor in dem Sie die Aktivität platzieren möchten.
1. Am unteren Rand des Bildschirms klicken Sie auf **Einfügen** und dann auf **Aktivität**.
1. Wählen Sie in der Spalte **Integrationsmodul** das Modul, das die Aktivität enthält ein.
1. Wählen Sie im Bereich **Aktivität** eine Aktivität aus.
1. Beachten Sie in der Spalte **Beschreibung** die Beschreibung der Aktivität ein. Sie können optional, klicken Sie auf Ansicht detaillierte Hilfe, um Hilfe für die Aktivität im Browser zu starten.
1. Klicken Sie auf den Pfeil nach rechts.  Wenn die Aktivität Parameter verfügt, werden diese für Ihre Informationen aufgelistet.
1. Klicken Sie auf die Schaltfläche überprüfen.  Die Aktivität auszuführenden Code wird in des Runbooks eingefügt werden.
1. Wenn die Aktivität Parameter erfordert, geben Sie einen entsprechenden Wert anstelle des Datentyps von geschweiften Klammern <> umgeben.

### <a name="to-insert-code-for-a-child-runbook-into-a-runbook"></a>Zum Einfügen von Code für eine untergeordnete Runbooks in einer Runbooks

1. Positionieren Sie den Cursor in den Zeichenbereich im Text-Editor in dem Sie die [untergeordneten Runbooks](automation-child-runbooks.md)platzieren möchten.
2. Am unteren Rand des Bildschirms klicken Sie auf **Einfügen** und dann auf **Runbooks**.
3. Auswählen des Runbooks aus der Mitte Spalte einfügen, und klicken Sie auf den Pfeil nach rechts.
4. Wenn die Runbooks Parameter verfügt, werden diese für Ihre Informationen aufgelistet.
5. Klicken Sie auf die Schaltfläche überprüfen.  Zum Ausführen des ausgewählten Runbooks Code wird in der aktuellen Runbooks eingefügt werden.
7. Wenn die Runbooks Parameter erfordert, geben Sie einen entsprechenden Wert anstelle des Datentyps von geschweiften Klammern <> umgeben.

### <a name="to-insert-an-asset-into-a-runbook"></a>Zum Einfügen einer Anlageguts in einer Runbooks

1. Positionieren Sie den Cursor in den Zeichenbereich im Text-Editor in dem Sie die Aktivität, um die Anlage abrufen platzieren möchten.
1. Am unteren Rand des Bildschirms klicken Sie auf **Einfügen** und dann auf **Einstellungen**.
1. Wählen Sie in der Spalte **Einstellung Aktion** die gewünschte Aktion.
1. Wählen Sie aus der Liste der verfügbaren Elemente in der Spalte Center aus.
1. Klicken Sie auf die Schaltfläche überprüfen.  Code zum Abrufen oder Festlegen der Anlage wird in der Runbooks eingefügt werden.



## <a name="to-edit-an-azure-automation-runbook-using-windows-powershell"></a>So bearbeiten Sie eine Azure Automatisierung Runbooks mithilfe von Windows PowerShell

Zum Bearbeiten einer Runbooks mit Windows PowerShell verwenden Sie den Editor Ihrer Wahl und es in einer PS1-Datei speichern. Sie können das Cmdlet " [Get-AzureAutomationRunbookDefinition](http://aka.ms/runbookauthor/cmdlet/getazurerunbookdefinition) " zum Abrufen des Inhalts der Runbooks und anschließend [Set-AzureAutomationRunbookDefinition](http://aka.ms/runbookauthor/cmdlet/setazurerunbookdefinition) -Cmdlet des vorhandenen Entwurf Runbooks durch das geänderte Schema zu ersetzen.

### <a name="to-retrieve-the-contents-of-a-runbook-using-windows-powershell"></a>Zum Abrufen des Inhalts einer Runbooks mithilfe von Windows PowerShell

Im folgenden Beispielbefehle zeigen, wie das Skript für eine Runbooks abgerufen werden, und klicken Sie auf eine Skriptdatei gespeichert. In diesem Beispiel wird die Entwurfsversion abgerufen. Es ist auch möglich, die veröffentlichte Version des des Runbooks abrufen, auch wenn diese Version nicht geändert werden kann.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $scriptPath = "c:\runbooks\Sample-TestRunbook.ps1"
    
    $runbookDefinition = Get-AzureAutomationRunbookDefinition -AutomationAccountName $automationAccountName -Name $runbookName -Slot Draft
    $runbookContent = $runbookDefinition.Content

    Out-File -InputObject $runbookContent -FilePath $scriptPath

### <a name="to-change-the-contents-of-a-runbook-using-windows-powershell"></a>So ändern Sie den Inhalt einer Runbooks mithilfe von Windows PowerShell

So ersetzen Sie den vorhandenen Inhalt einer Runbooks mit dem Inhalt einer Skriptdatei anzeigen die folgenden Beispiele für Befehle Beachten Sie, dass dies dasselbe Verfahren, das Beispiel wie in [einer Runbooks aus einem mit Windows PowerShell-Skriptdatei importieren](../automation-creating-or-importing-a-runbook#ImportRunbookScriptPS)ist.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $scriptPath = "c:\runbooks\Sample-TestRunbook.ps1"

    Set-AzureAutomationRunbookDefinition -AutomationAccountName $automationAccountName -Name $runbookName -Path $scriptPath -Overwrite
    Publish-AzureAutomationRunbook –AutomationAccountName $automationAccountName –Name $runbookName

## <a name="related-articles"></a>Verwandte Artikel

- [Erstellen oder Importieren einer Runbooks in Azure Automatisierung](automation-creating-importing-runbook.md)
- [Learning PowerShell-workflow](automation-powershell-workflow.md)
- [Grafische in Azure Automatisierung authoring](automation-graphical-authoring-intro.md)
- [Zertifikate](automation-certificates.md)
- [Verbindungen](automation-connections.md)
- [Anmeldeinformationen](automation-credentials.md)
- [Zeitpläne](automation-schedules.md)
- [Variablen](automation-variables.md)
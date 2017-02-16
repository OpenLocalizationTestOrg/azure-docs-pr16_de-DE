<properties 
   pageTitle="Planen einer Runbooks in Azure Automatisierung | Microsoft Azure"
   description="Beschreibt das Erstellen eines Projektplans in Azure Automatisierung, damit eine Runbooks automatisch zu einem bestimmten Zeitpunkt oder nach einem periodischen Zeitplan beginnen können."
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
   ms.date="08/05/2016"
   ms.author="bwren" />

# <a name="scheduling-a-runbook-in-azure-automation"></a>Planen einer Runbooks in Azure Automatisierung

Zum Planen einer Runbooks in Azure Automatisierung zu einem bestimmten Zeitpunkt zu starten, verknüpfen Sie es mit einen oder mehrere Zeitpläne zu. Ein Zeitplan kann entweder einmal oder eine erneut auftritt stündlich ausführen oder tägliche Projektplan für Runbooks im klassischen Azure-Portal und Runbooks Azure-Portal konfiguriert werden, Sie können außerdem planen, wöchentlich, monatlich, bestimmte Tage der Woche oder Tage des Monats oder einen bestimmten Tag des Monats.  Eine Runbooks kann mehrere Zeitpläne verknüpft werden kann, und ein Zeitplan mehrere Runbooks verknüpft sein.


## <a name="creating-a-schedule"></a>Erstellen eines Zeitplans

Sie können einen neuen Zeitplan für Runbooks im Azure-Portal im Portal klassischen oder mit Windows PowerShell erstellen. Sie haben auch die Möglichkeit, einen neuen Zeitplan erstellen, wenn Sie eine Runbooks mit einen Zeitplan mit dem Azure klassischen oder Azure-Portal verknüpfen.

>[AZURE.NOTE] Wenn Sie einen Zeitplan mit einem Runbooks zuordnen, Automatisierung speichert die aktuellen Versionen der Module in Ihr Konto, und verknüpft diese mit dieser Terminplan.  Dies bedeutet, dass wenn Sie ein Modul mit Version 1.0 in Ihr Konto hatten, wenn Sie einen Zeitplan erstellt und dann auf Version 2.0 das Modul aktualisieren, der Terminplan weiterhin 1.0 verwenden.  Die Modulversion aktualisierte verwenden möchten, müssen Sie einen neuen Zeitplan erstellen. 

### <a name="to-create-a-new-schedule-in-the-azure-classic-portal"></a>Zum Erstellen eines neuen Projektplans in der klassischen Azure-portal

1. Im Portal Azure klassischen aktivieren Sie Automatisierung, und wählen Sie dann den Namen eines Kontos Automatisierung.
1. Wählen Sie die Registerkarte **Anlagen** aus.
1. Am unteren Rand des Fensters klicken Sie auf **Hinzufügen**.
1. Klicken Sie auf **Terminplan hinzufügen**.
1. Geben Sie einen **Namen** und optional eine **Beschreibung** für den neuen schedule.your Terminplan **Einmaliges**, **stündlich**, **täglich**, **wöchentlich**oder **monatlich**ausgeführt wird.
1. Geben Sie eine **Anfangszeit** und andere Optionen je nach Typ des Zeitplans, den Sie ausgewählt haben.

### <a name="to-create-a-new-schedule-in-the-azure-portal"></a>Zum Erstellen eines neuen Projektplans im Azure-Portal

1. Klicken Sie im Portal Azure aus Ihrem Konto Automatisierung auf die Kachel **Posten** , um das Blade **Anlagen** zu öffnen.
2. Klicken Sie auf die Kachel **Terminpläne** , um das Blade **Zeitpläne** zu öffnen.
3. Klicken Sie auf **Hinzufügen, einen Zeitplan** am oberen Rand der Blade.
4. Geben Sie in der **neuen Projektplan** Blade einen **Namen** und optional eine **Beschreibung** für den neuen Terminplan ein.
5. Wählen Sie aus, ob der Terminplan einmal ausgeführt wird, oder nach einem Zugriffsprobleme Zeitplan, indem Sie **einmal** oder **Serie**auswählen.  Wenn Sie **einmal** Auswählen einer **Startzeit** angeben, und klicken Sie dann auf **Erstellen**.  Wenn Sie **Serie**auswählen, geben Sie die **Startzeit** und die Häufigkeit für wie oft des Runbooks wiederholen soll – indem **Stunde**, **Tag**, **Woche**oder **Monat**.  Wenn Sie **Woche** oder **Monat** aus der Dropdownliste auswählen, die **Option Serie** wird angezeigt, in das Blade und nach der Auswahl wird das **Option Serientyp** Blade behandelt werden und Sie können den Tag der Woche auswählen, wenn Sie **Woche**ausgewählt haben.  Wenn Sie **Monat**ausgewählt haben, können Sie auswählen, indem **Wochentage** oder bestimmte Tage des Monats im Kalender und führen Sie schließlich möchten, führen Sie es am letzten Tag des Monats oder nicht, und klicken Sie dann auf **OK**.   

### <a name="to-create-a-new-schedule-with-windows-powershell"></a>Zum Erstellen eines neuen Projektplans mit Windows PowerShell

Sie können die [Neu-AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690271.aspx) -Cmdlet zum Erstellen eines neuen Projektplans in Azure Automatisierung für klassische Runbooks oder [Neu-AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603577.aspx) -Cmdlet für Runbooks Azure-Portal verwenden. Geben Sie die Startzeit für den Zeitplan und die Häufigkeit, die ausgeführt werden soll.

Im folgenden Beispielbefehle anzeigen zum Erstellen eines neuen Projektplans, das jeden Tag 3:30 Uhr mit einer Azure Servicemanagement Cmdlet 20 Januar 2015 beginnt ausgeführt wird.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    New-AzureAutomationSchedule –AutomationAccountName $automationAccountName –Name `
    $scheduleName –StartTime "1/20/2016 15:30:00" –DayInterval 1

Im folgenden Beispielbefehle wird gezeigt, wie einen Zeitplan für die 15. und 30. Tag des Monats mithilfe einer Ressourcenmanager Azure-Cmdlets zu erstellen.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
    New-AzureRMAutomationSchedule –AutomationAccountName $automationAccountName –Name `
    $scheduleName -StartTime "7/01/2016 15:30:00" -MonthInterval 1 `
    -DaysOfMonth Fifteenth,Thirtieth -ResourceGroupName "ResourceGroup01"
    

## <a name="linking-a-schedule-to-a-runbook"></a>Verknüpfen einen Zeitplan mit einem Runbooks

Eine Runbooks kann mehrere Zeitpläne verknüpft werden kann, und ein Zeitplan mehrere Runbooks verknüpft sein. Wenn eine Runbooks Parameter verfügt, können Sie die Werte für diese bereitstellen. Sie müssen Werte für alle erforderlichen Parameter sowie vorsehen Werte für optionale Parameter.  Diese Werte werden jedes Mal verwendet werden, die von diesen Zeitplan des Runbooks gestartet wird.  Sie können Anfügen des gleichen Runbooks zu einem anderen Zeitplan und verschiedene Parameterwerte angeben.


### <a name="to-link-a-schedule-to-a-runbook-with-the-azure-classic-portal"></a>So verknüpfen Sie einen Zeitplan mit einem Runbooks mit dem klassischen Azure-portal

1. Im Portal Azure klassischen aktivieren Sie **Automatisierung,** und klicken Sie dann auf den Namen eines Kontos Automatisierung.
2. Wählen Sie die Registerkarte **Runbooks** aus.
3. Klicken Sie auf den Namen des Runbooks zu planen.
4. Klicken Sie auf die Registerkarte **Zeitplan** .
5. Wenn die Runbooks einem Terminplan aktuell nicht verknüpft ist, klicken Sie dann die Option **Link zu einem neuen Projektplan** oder einer **Verknüpfung zu einer vorhandenen Terminplan**erhalten Sie.  Wenn des Runbooks aktuell einem Terminplan verknüpft ist, klicken Sie auf **Link** am unteren Rand des Fensters zum Zugreifen auf diese Optionen.
6. Wenn des Runbooks Parameter verfügt, werden Sie aufgefordert, deren Werte anzugeben.  

### <a name="to-link-a-schedule-to-a-runbook-with-the-azure-portal"></a>So verknüpfen Sie einen Zeitplan mit einem Runbooks mit Azure-portal

1. Klicken Sie im Portal Azure aus Ihrem Konto Automatisierung auf die Kachel **Runbooks** , um das Blade **Runbooks** zu öffnen.
2. Klicken Sie auf den Namen des Runbooks zu planen.
3. Wenn Sie einem Terminplan aktuell nicht des Runbooks verknüpft ist, klicken Sie dann die Option zum Erstellen eines neuen Projektplan oder einen Link zu einer vorhandenen Zeitplan erhalten Sie.  
4. Wenn des Runbooks Parameter verfügt, können Sie die Option **ändern, führen Sie die Einstellungen (Standard: Azure)** auswählen, und das Blade **Parameter** wird angezeigt, in dem Sie die Informationen entsprechend eingeben können.  

### <a name="to-link-a-schedule-to-a-runbook-with-windows-powershell"></a>So verknüpfen Sie einen Zeitplan mit einem Runbooks mit Windows PowerShell

Der [Register-AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/azure/dn690265.aspx) können Sie um einen Zeitplan mit einem klassischen Runbooks oder [Register-AzureRmAutomationScheduledRunbook](https://msdn.microsoft.com/library/mt603575.aspx) -Cmdlet für Runbooks Azure-Portal zu verknüpfen.  Sie können Werte für Parameter des Runbooks des mit dem Parameter Parameter angeben. Weitere Informationen zum Angeben der Parameterwerte finden Sie unter [Starten eines Runbooks in Azure Automatisierung](automation-starting-a-runbook.md) .

Im folgenden Beispielbefehle anzeigen einen Zeitplan mit einer Azure Servicemanagement Cmdlet mit den Parametern verknüpfen.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
    –Name $runbookName –ScheduleName $scheduleName –Parameters $params

Im folgenden Beispielbefehle zeigen, wie einen Zeitplan mit einer Runbooks mithilfe einer Ressourcenmanager Azure-Cmdlets mit den Parametern verknüpfen.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureRmAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
    –Name $runbookName –ScheduleName $scheduleName –Parameters $params `
    -ResourceGroupName "ResourceGroup01"

## <a name="disabling-a-schedule"></a>Deaktivieren eines Projektplans

Wenn Sie einen Zeitplan deaktivieren, werden alle Runbooks verknüpft sein nicht mehr auf dieser Zeitplan ausgeführt werden. Sie können manuell kein Ablaufdatum für Terminpläne mit einer Häufigkeit festlegen, wenn Sie diese erstellen oder einen Zeitplan deaktivieren. Wenn das Ablaufdatum erreicht ist, wird der Terminplan deaktiviert.

### <a name="to-disable-a-schedule-from-the-azure-classic-portal"></a>So deaktivieren Sie einen Zeitplan vom klassischen Azure-portal

Sie können einen Zeitplan im klassischen Azure-Portal auf der Detailseite der Terminplan für den Zeitplan deaktivieren.

1. Im Portal Azure klassischen aktivieren Sie Automatisierung, und klicken Sie dann auf den Namen eines Kontos Automatisierung.
1. Wählen Sie die Registerkarte Anlagen aus.
1. Klicken Sie auf den Namen des einen Zeitplan, um deren Detailseite zu öffnen.
2. Ändern Sie **aktiviert** auf **Nein**fest.

### <a name="to-disable-a-schedule-from-the-azure-portal"></a>So deaktivieren Sie einen Zeitplan vom Azure-portal

1. Klicken Sie im Portal Azure aus Ihrem Konto Automatisierung auf die Kachel **Posten** , um das Blade **Anlagen** zu öffnen.
2. Klicken Sie auf die Kachel **Terminpläne** , um das Blade **Zeitpläne** zu öffnen.
2. Klicken Sie auf den Namen des einen Zeitplan, um das Blade Details zu öffnen.
3. Ändern Sie **aktiviert** auf **Nein**fest.

### <a name="to-disable-a-schedule-with-windows-powershell"></a>So deaktivieren Sie einen Zeitplan mit Windows PowerShell

Das Cmdlet " [Set-AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690270.aspx) " können Sie die Eigenschaften eines vorhandenen Zeitplans für eine klassische Runbooks oder [Set-AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603566.aspx) -Cmdlet für Runbooks Azure-Portal zu ändern. Geben Sie zum Deaktivieren des Zeitplans **falsch** für den **IsEnabled** Parameter ein.

Im folgenden Beispielbefehle zeigen, wie einen Zeitplan mithilfe des Cmdlets Azure Servicemanagement deaktivieren.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    Set-AzureAutomationSchedule –AutomationAccountName $automationAccountName `
    –Name $scheduleName –IsEnabled $false

So deaktivieren Sie einen Zeitplan für eine Runbooks mithilfe einer Ressourcenmanager Azure-Cmdlets anzeigen den folgenden Beispiele für Befehle

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
    Set-AzureRmAutomationSchedule –AutomationAccountName $automationAccountName `
    –Name $scheduleName –IsEnabled $false -ResourceGroupName "ResourceGroup01"


## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum Arbeiten mit Zeitpläne finden Sie unter [Planen von Anlagen in Azure Automatisierung](http://msdn.microsoft.com/library/azure/dn940016.aspx)
- Um mit Runbooks in Azure Automatisierung anzufangen, finden Sie in [einem Runbooks in Azure Automatisierung ab](automation-starting-a-runbook.md) 
<properties 
   pageTitle="Runbooks Einstellungen"
   description="Beschreibt die Konfiguration Einstellungen für eine Runbooks Azure Automatisierung und erfahren, wie sie die Verwendung der Azure-Verwaltungsportal und die Windows PowerShell ändern."
   services="automation"
   documentationCenter=""
   authors="bwren"
   manager="stevenka"
   editor="tysonn" />
<tags 
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/09/2016"
   ms.author="bwren" />

# <a name="runbook-settings"></a>Runbooks Einstellungen

Jede Runbooks in Azure Automatisierung verfügt über mehrere Einstellungen, mit die Hilfe identifiziert werden und deren Protokollierungsverhalten zu ändern. Jeder diese Einstellungen wird nachstehend beschrieben, gefolgt von Verfahren zum ändern können.

## <a name="settings"></a>Einstellungen

### <a name="name-and-description"></a>Name und Beschreibung

Sie können nicht den Namen einer Runbooks ändern, nachdem es erstellt wurde. Die Beschreibung kann ist optional und bis zu 512 Zeichen.

### <a name="tags"></a>Kategorien

Kategorien können Sie distinct Wörter und Ausdrücke zum Identifizieren einer Runbooks zuweisen. Beim Übermitteln einer Runbooks zur [Runbooks Katalog](https://msdn.microsoft.com/library/dn781422.aspx), geben Sie beispielsweise bestimmte Kategorien, um die Kategorien Identifizieren des Runbooks in aufgelistet werden soll. Sie können mehrere Tags für eine Runbooks angeben, indem Sie diese durch Semikolons trennen.

### <a name="logging"></a>Protokollierung

Standardmäßig werden ausführlich und den Fortschritt Einträge nicht in Historie geschrieben. Sie können die Einstellungen für eine bestimmte Runbooks zum Melden Sie diese Datensätze ändern. Weitere Informationen zu dieser Datensätze finden Sie unter [Runbooks Ausgabe und Nachrichten](https://msdn.microsoft.com/library/dn879148.aspx).

## <a name="changing-runbook-settings"></a>Runbooks Einstellungen ändern

### <a name="changing-runbook-settings-with-the-azure-management-portal"></a>Ändern des Runbooks Einstellungen mit den Azure-Verwaltungsportal

Sie können die Einstellungen für eine Runbooks im Azure-Verwaltungsportal von der Seite " **Konfigurieren** " für die Runbooks ändern.

1. Aktivieren Sie im Verwaltungsportal Azure **Automatisierung,** und klicken Sie dann auf den Namen eines Kontos Automatisierung.
1. Wählen Sie die Registerkarte **Runbooks** aus.
1. Klicken Sie auf den Namen einer Runbooks.
1. Wählen Sie die Registerkarte **Konfigurieren** .

### <a name="changing-runbook-settings-with-windows-powershell"></a>Ändern des Runbooks Einstellungen mit Windows PowerShell

Das Cmdlet " [Set-AzureAutomationRunbook](https://msdn.microsoft.com/library/dn690275.aspx) " können Sie die Einstellungen für eine Runbooks ändern. Wenn Sie mehrere Kategorien angeben möchten, können Sie entweder ein Array oder eine einzelne Zeichenfolge mit durch Kommas getrennte Werte an den Parameter Kategorien bereitstellen. Sie können den aktuellen Tags mit der [Get-AzureAutomationRunbook](https://msdn.microsoft.com/library/dn690278.aspx)erhalten.

Im folgenden Beispielbefehle anzeigen zum Festlegen der Eigenschaften für eine Runbooks In diesem Beispiel die vorhandenen Kategorien drei Kategorien hinzugefügt und gibt an, dass ausführlichen Einträge protokolliert werden sollen.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $tags = (Get-AzureAutomationRunbook –AutomationAccountName $automationAccountName –Name $runbookName).Tags
    $tags += "Tag1,Tag2,Tag3"
    Set-AzureAutomationRunbook –AutomationAccountName $automationAccountName –Name $runbookName –LogVerbose $true –Tags $tags

## <a name="related-articles"></a>Verwandte Artikel
- [Runbooks Ausgabe und Nachrichten](../automation-runbook-output-and-messages) 
- [Erstellen oder Importieren einer Runbooks](https://msdn.microsoft.com/library/dn643637.aspx) 
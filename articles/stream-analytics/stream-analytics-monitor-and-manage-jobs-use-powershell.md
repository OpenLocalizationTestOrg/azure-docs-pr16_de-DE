<properties 
    pageTitle="Überwachen und Verwalten von Stream Analytics Aufträge mit PowerShell | Microsoft Azure" 
    description="Informationen Sie zum Verwenden von Azure PowerShell und Cmdlets zum Überwachen und Verwalten von Stream Analytics Aufträge." 
    keywords="Azure Powershell, Azure Powershell-Cmdlets, Powershell-Befehl Powershell-Skripting" 
    services="stream-analytics" 
    documentationCenter="" 
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>


# <a name="monitor-and-manage-stream-analytics-jobs-with-azure-powershell-cmdlets"></a>Überwachen und Verwalten von Stream Analytics Aufträge mit Azure PowerShell-cmdlets

Informationen Sie zum Überwachen und Verwalten von Stream Analytics Ressourcen mit Azure PowerShell-Cmdlets und Powershell-Skripting, die grundlegende Stream Analytics Aufgaben ausführen.

## <a name="prerequisites-for-running-azure-powershell-cmdlets-for-stream-analytics"></a>Erforderliche Komponenten für die Ausführung von Azure PowerShell-Cmdlets für Stream Analytics

 - Erstellen Sie eine Ressourcengruppe Azure in Ihr Abonnement. So sieht ein Beispiel Azure PowerShell-Skript. Azure PowerShell Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md);  

Azure PowerShell 0.9.8:  

        # Log in to your Azure account
        Add-AzureAccount

        # Select the Azure subscription you want to use to create the resource group if you have more than one subscription on your account.
        Select-AzureSubscription -SubscriptionName <subscription name>
 
        # If Stream Analytics has not been registered to the subscription, remove remark symbol below (#) to run the Register-AzureProvider cmdlet to register the provider namespace.
        #Register-AzureProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'

        # Create an Azure resource group
        New-AzureResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>

Azure PowerShell 1.0:  

        # Log in to your Azure account
        Login-AzureRmAccount

        # Select the Azure subscription you want to use to create the resource group.
        Get-AzureRmSubscription –SubscriptionName “your sub” | Select-AzureRmSubscription

        # If Stream Analytics has not been registered to the subscription, remove remark symbol below (#) to run the Register-AzureProvider cmdlet to register the provider namespace.
        #Register-AzureRmResourceProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'
        
        # Create an Azure resource group
        New-AzureRMResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>
        


> [AZURE.NOTE] Stream Analytics Aufträge programmgesteuert erstellt haben keine Überwachung standardmäßig aktiviert.  Sie können manuell aktivieren, Überwachen von Azure-Portal mit Navigieren zu den Auftrag des Monitor-Seite, und klicken auf die Schaltfläche aktivieren oder Sie können dies programmgesteuert gemäß die Anweisungen am [Azure Stream Analytics - Monitor Stream Analytics Aufträge Programmgesteuertes](stream-analytics-monitor-jobs.md).

## <a name="azure-powershell-cmdlets-for-stream-analytics"></a>Azure PowerShell-Cmdlets für Stream Analytics
Die folgenden Azure PowerShell-Cmdlets kann zum Überwachen und Verwalten von Azure Stream Analytics Aufträge verwendet werden. Beachten Sie, dass Azure PowerShell verschiedene Versionen aufweist. 
**In den Beispielen aufgeführt ist der erste Befehl für Azure PowerShell 0.9.8, der zweite Befehl für Azure PowerShell 1.0 ist.** Die Befehle Azure PowerShell 1.0 haben immer "AzureRM" in den Befehl.

### <a name="get-azurestreamanalyticsjob--get-azurermstreamanalyticsjob"></a>Get-AzureStreamAnalyticsJob | Get-AzureRMStreamAnalyticsJob
Listet alle Stream Analytics Aufträge in der Azure-Abonnement oder angegebenen Ressourcengruppe definiert oder Auftragsinformationen zu einer bestimmten Position innerhalb einer Ressourcengruppe erhält.

**Beispiel 1**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsJob

Dieser Befehl PowerShell gibt Informationen über alle Stream Analytics Einzelvorgänge im Azure-Abonnement.

**Beispiel 2**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US 

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US 

Dieser PowerShell-Befehl gibt Informationen über alle Stream Analytics Einzelvorgänge in der Ressourcengruppe StreamAnalytics-Standard-Central-US an.

**Beispiel 3**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob

Dieser Befehl PowerShell gibt Informationen zu den Auftrag Stream Analytics StreamingJob in der Ressourcengruppe StreamAnalytics-Standard-Central-US an.

### <a name="get-azurestreamanalyticsinput--get-azurermstreamanalyticsinput"></a>Get-AzureStreamAnalyticsInput | Get-AzureRMStreamAnalyticsInput
Zeigt eine Liste aller Eingaben, die in einem angegebenen Stream Analytics Stelle definiert sind, oder ruft Informationen zu einer bestimmten Eingabesprache ab.

**Beispiel 1**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

Dieser Befehl PowerShell gibt Informationen zur alle Eingaben im Auftrag StreamingJob definierten zurück.

**Beispiel 2**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name EntryStream

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name EntryStream

Dieser Befehl PowerShell gibt Informationen zur eingegebenen EntryStream im Auftrag StreamingJob definierten benannten zurück.

### <a name="get-azurestreamanalyticsoutput--get-azurermstreamanalyticsoutput"></a>Get-AzureStreamAnalyticsOutput | Get-AzureRMStreamAnalyticsOutput
Zeigt eine Liste aller die Ausgaben, die in einem angegebenen Stream Analytics Stelle definiert sind, oder ruft Informationen zu einer bestimmten Ausgabe ab.

**Beispiel 1**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

Dieser Befehl PowerShell gibt Informationen über die Ausgaben in den Auftrag StreamingJob definiert.

**Beispiel 2**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name Output

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name Output

Dieser Befehl PowerShell gibt Informationen zur Ausgabe Ausgabe im Auftrag StreamingJob definierten Namen zurück.

### <a name="get-azurestreamanalyticsquota--get-azurermstreamanalyticsquota"></a>Get-AzureStreamAnalyticsQuota | Get-AzureRMStreamAnalyticsQuota
Ruft Informationen über das Kontingent streaming Einheiten in einem bestimmten Bereich ab.

**Beispiel 1**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsQuota –Location "Central US" 

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsQuota –Location "Central US" 

Dieser Befehl PowerShell gibt Informationen über das Kontingent und die Verwendung der streaming Einheiten in der zentralen US-Region.

### <a name="get-azurestreamanalyticstransformation--getazurermstreamanalyticstransformation"></a>Get-AzureStreamAnalyticsTransformation | GetAzureRMStreamAnalyticsTransformation
Ruft Informationen zu einer bestimmten Transformation in einem Stream Analytics Auftrag definiert.

**Beispiel 1**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name StreamingJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name StreamingJob

Dieser Befehl PowerShell gibt Informationen über die Transformation StreamingJob in den Auftrag StreamingJob bezeichnet.

### <a name="new-azurestreamanalyticsinput--new-azurermstreamanalyticsinput"></a>Neue AzureStreamAnalyticsInput | Neue AzureRMStreamAnalyticsInput
Erstellt eine neue Eingabe innerhalb eines Auftrags Stream Analytics oder eine vorhandene angegebene Eingabe aktualisiert.
  
Der Name der Eingabe kann in der Datei .json oder in der Befehlszeile angegeben werden. Wenn beide angegeben sind, muss der Name in der Befehlszeile identisch mit der in der Datei.

Wenn Sie angeben eine Eingabe, die bereits vorhanden ist, und Sie keine geben der – Parameter erzwingen, dass das Cmdlet fragt, ob er die vorhandene Eingabe ersetzen.

Wenn Sie angeben der – erzwingen Parameter, und geben Sie eine vorhandene Geben Sie ein, ohne Bestätigung die Eingabe ersetzt werden.

Ausführliche Informationen zu JSON-Dateistruktur und Inhalt, verweisen Sie mit der [Eingabe erstellen (Azure Stream Analytics)] [ msdn-rest-api-create-stream-analytics-input] der [Stream Analytics Management REST API Bezug Bibliothek]im Abschnitt[stream.analytics.rest.api.reference].

**Beispiel 1**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" 

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" 

Dieser Befehl PowerShell erstellt eine neue Eingabe aus der Datei Input.json. Wenn bereits eine vorhandene Eingabe mit dem Namen in der Eingabewerte Formulardefinitionsdatei angegebenen definiert ist, fragt das Cmdlet unabhängig davon zu ersetzen.

**Beispiel 2**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream

Dieser Befehl PowerShell erstellt eine neue Eingabe in den Auftrag als EntryStream bezeichnet. Wenn Sie eine vorhandene Eingabe mit diesem Namen bereits definiert ist, fragt das Cmdlet unabhängig davon zu ersetzen.

**Beispiel 3**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream -Force

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream -Force

Dieser Befehl PowerShell ersetzt die Definition der vorhandenen Eingabewerte Datenquelle mit der Bezeichnung EntryStream mit der Definition aus der Datei.

### <a name="new-azurestreamanalyticsjob--new-azurermstreamanalyticsjob"></a>Neue AzureStreamAnalyticsJob | Neue AzureRMStreamAnalyticsJob
Erstellt einen neuen Stream Analytics Auftrag in Microsoft Azure oder die Definition einer vorhandenen angegebenen Auftrag aktualisiert.

Der Name des Auftrags kann in der Datei .json oder in der Befehlszeile angegeben werden muss. Wenn beide angegeben sind, muss der Name in der Befehlszeile identisch mit der in der Datei.

Wenn Sie geben Sie einen Auftrag an, die bereits vorhanden ist, und Sie keine geben der – Parameter erzwingen, dass das Cmdlet fragt, ob er den vorhandenen Auftrag zu ersetzen.

Wenn Sie angeben der – erzwingen Parameter, und geben Sie einen vorhandenen Auftrag an, ohne Bestätigung die Position Definition ersetzt werden.

Ausführliche Informationen zu JSON-Dateistruktur und Inhalt, schlagen Sie in den [Neuen Stream Analytics Job] [ msdn-rest-api-create-stream-analytics-job] der [Stream Analytics Management REST API Bezug Bibliothek]im Abschnitt[stream.analytics.rest.api.reference].

**Beispiel 1**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" 

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" 

Dieser Befehl PowerShell erstellt ein neues Projekt aus der Definition in JobDefinition.json. Wenn Sie ein vorhandener Auftrag mit dem Namen in der Formulardefinitionsdatei Position angegebene bereits definiert ist, fragt das Cmdlet unabhängig davon zu ersetzen.

**Beispiel 2**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" –Name StreamingJob -Force

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" –Name StreamingJob -Force

Dieser Befehl PowerShell ersetzt die Position Definition für StreamingJob.

### <a name="new-azurestreamanalyticsoutput--new-azurermstreamanalyticsoutput"></a>Neue AzureStreamAnalyticsOutput | Neue AzureRMStreamAnalyticsOutput
Erstellt eine neue Ausgabe innerhalb eines Auftrags Stream Analytics oder eine vorhandene Ausgabe aktualisiert.  

Der Name der Ausgabe kann in der Datei .json oder in der Befehlszeile angegeben werden. Wenn beide angegeben sind, muss der Name in der Befehlszeile identisch mit der in der Datei.

Wenn Sie eine, der bereits Ausgabe nicht angeben der – Parameter erzwingen, dass das Cmdlet fragt, ob er die Ausgabe die vorhandene zu ersetzen.

Wenn Sie angeben der – erzwingen Parameter, und geben Sie einen vorhandenen Namen ausgeben, die Ausgabe ohne Bestätigung ersetzt werden.

Ausführliche Informationen zu JSON-Dateistruktur und Inhalt, schlagen Sie in der [Ausgabe erstellen (Azure Stream Analytics)] [ msdn-rest-api-create-stream-analytics-output] der [Stream Analytics Management REST API Bezug Bibliothek]im Abschnitt[stream.analytics.rest.api.reference].

**Beispiel 1**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output

Dieser PowerShell-Befehl erstellt eine neue Ausgabe "Ausgabe" in den Auftrag StreamingJob bezeichnet. Wenn Sie eine vorhandene Ausgabe mit diesem Namen bereits definiert ist, fragt das Cmdlet unabhängig davon zu ersetzen.

**Beispiel 2**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output -Force

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output -Force

Dieser Befehl PowerShell ersetzt die Definition für "Ausgabe" in den Auftrag StreamingJob.

### <a name="new-azurestreamanalyticstransformation--new-azurermstreamanalyticstransformation"></a>Neue AzureStreamAnalyticsTransformation | Neue AzureRMStreamAnalyticsTransformation
Erstellt eine neue Transformation innerhalb eines Auftrags Stream Analytics oder die vorhandene Transformation aktualisiert.
  
Der Name der Transformation kann in der Datei .json oder in der Befehlszeile angegeben werden. Wenn beide angegeben sind, muss der Name in der Befehlszeile identisch mit der in der Datei.

Wenn Sie eine Transformation, die bereits vorhanden ist nicht angeben der – Parameter erzwingen, dass das Cmdlet fragt, ob er die vorhandene Transformation zu ersetzen.

Wenn Sie angeben der – erzwingen Parameter, und geben Sie den Namen einer vorhandenen Transformation ohne Bestätigung die Transformation ersetzt werden.

Ausführliche Informationen zu JSON-Dateistruktur und Inhalt, schlagen Sie in der [Transformation erstellen (Azure Stream Analytics)] [ msdn-rest-api-create-stream-analytics-transformation] der [Stream Analytics Management REST API Bezug Bibliothek]im Abschnitt[stream.analytics.rest.api.reference].

**Beispiel 1**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform

Dieser PowerShell-Befehl erstellt eine neue Transformation StreamingJobTransform in den Auftrag StreamingJob bezeichnet. Wenn Sie eine vorhandene Transformation bereits mit diesem Namen definiert ist, fragt das Cmdlet unabhängig davon zu ersetzen.

**Beispiel 2**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform -Force

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform -Force

 Dieser Befehl PowerShell ersetzt die Definition der StreamingJobTransform in den Auftrag StreamingJob.

### <a name="remove-azurestreamanalyticsinput--remove-azurermstreamanalyticsinput"></a>Entfernen-AzureStreamAnalyticsInput | Entfernen-AzureRMStreamAnalyticsInput
Asynchrone Löscht eine bestimmte Eingabe aus einem Stream Analytics Auftrag in Microsoft Azure ein.  
Wenn Sie angeben der – Parameter erzwingen, dass die Eingabe ohne Bestätigung gelöscht.

**Beispiel 1**

Azure PowerShell 0.9.8:  

    Remove-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EventStream

Azure PowerShell 1.0:  

    Remove-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EventStream

Dieser Befehl PowerShell entfernt die Eingabewerte EventStream in den Auftrag StreamingJob.  

### <a name="remove-azurestreamanalyticsjob--remove-azurermstreamanalyticsjob"></a>Entfernen-AzureStreamAnalyticsJob | Entfernen-AzureRMStreamAnalyticsJob
Asynchrone Löscht einen bestimmten Stream Analytics Auftrag in Microsoft Azure ein.  
Wenn Sie angeben der – Force-Parameter, der Auftrag ohne Bestätigung gelöscht.

**Beispiel 1**

Azure PowerShell 0.9.8:  

    Remove-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Azure PowerShell 1.0:  

    Remove-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Dieser PowerShell-Befehl entfernt den Auftrag StreamingJob.  

### <a name="remove-azurestreamanalyticsoutput--remove-azurermstreamanalyticsoutput"></a>Entfernen-AzureStreamAnalyticsOutput | Entfernen-AzureRMStreamAnalyticsOutput
Asynchrone Löscht eine bestimmte Ausgabe aus einem Stream Analytics Auftrag in Microsoft Azure ein.  
Wenn Sie angeben der – Parameter erzwingen, dass die Ausgabe ohne Bestätigung gelöscht.

**Beispiel 1**

Azure PowerShell 0.9.8:  

    Remove-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Azure PowerShell 1.0:  

    Remove-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Die Ausgabe im Auftrag StreamingJob ausgeben entfernt diese PowerShell-Befehl.  

### <a name="start-azurestreamanalyticsjob--start-azurermstreamanalyticsjob"></a>Start-AzureStreamAnalyticsJob | Start-AzureRMStreamAnalyticsJob
Asynchrone bereitstellt, und startet für ein Projekt Stream Analytics in Microsoft Azure.

**Beispiel 1**

Azure PowerShell 0.9.8:  

    Start-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob -OutputStartMode CustomTime -OutputStartTime 2012-12-12T12:12:12Z

Azure PowerShell 1.0:  

    Start-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob -OutputStartMode CustomTime -OutputStartTime 2012-12-12T12:12:12Z

Dieser PowerShell-Befehl startet den Auftrag StreamingJob mit einer benutzerdefinierten Ausgabe Startzeit bis 12 Dezember 2012, 12:12:12 festlegen UTC.


### <a name="stop-azurestreamanalyticsjob--stop-azurermstreamanalyticsjob"></a>Beenden-AzureStreamAnalyticsJob | Beenden-AzureRMStreamAnalyticsJob
Asynchrone Tabstopps ein Auftrags Stream Analytics in Microsoft Azure ausgeführt wird, und heben weist Ressourcen, die Waren, die aktuell verwendet wurden. Die Definition der Position und Metadaten bleibt innerhalb Ihres Abonnements über die Azure-Portal und den Management-APIs verfügbar, dass der Auftrag bearbeitet und neu gestartet werden kann. Sie werden nicht für ein Projekt im angehaltenen Status in Rechnung gestellt.

**Beispiel 1**

Azure PowerShell 0.9.8:  

    Stop-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Azure PowerShell 1.0:  

    Stop-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Dieser PowerShell-Befehl beendet den Auftrag StreamingJob.  

### <a name="test-azurestreamanalyticsinput--test-azurermstreamanalyticsinput"></a>Test-AzureStreamAnalyticsInput | Test-AzureRMStreamAnalyticsInput
Testen, ob der Stream Analytics Verbindung zu einem angegebenen Eingabe.

**Beispiel 1**

Azure PowerShell 0.9.8:  

    Test-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EntryStream

Azure PowerShell 1.0:  

    Test-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EntryStream

Dieser Befehl PowerShell überprüft den Status der Verbindung mit der Eingabe EntryStream in StreamingJob.  

###<a name="test-azurestreamanalyticsoutput--test-azurermstreamanalyticsoutput"></a>Test-AzureStreamAnalyticsOutput | Test-AzureRMStreamAnalyticsOutput
Testen, ob der Stream Analytics Verbindung zu einem angegebenen Ausgabe.

**Beispiel 1**

Azure PowerShell 0.9.8:  

    Test-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Azure PowerShell 1.0:  

    Test-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Diese PowerShell-Befehl-Tests der Verbindungsstatus der Ausgabe in StreamingJob ausgeben.  

## <a name="get-support"></a>Anfordern von Unterstützung
Versuchen Sie für weitere Unterstützung zu erhalten unseren [Azure Stream Analytics-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics). 


## <a name="next-steps"></a>Nächste Schritte

- [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
- [Erste Schritte mit Azure Stream Analytics](stream-analytics-get-started.md)
- [Skalieren Sie Azure Stream Analytics Aufträge](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Query Language Bezug](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST-API-Referenz](https://msdn.microsoft.com/library/azure/dn835031.aspx)
 



[msdn-switch-azuremode]: http://msdn.microsoft.com/library/dn722470.aspx
[powershell-install]: http://azure.microsoft.com/documentation/articles/powershell-install-configure/
[msdn-rest-api-create-stream-analytics-job]: https://msdn.microsoft.com/library/dn834994.aspx
[msdn-rest-api-create-stream-analytics-input]: https://msdn.microsoft.com/library/dn835010.aspx
[msdn-rest-api-create-stream-analytics-output]: https://msdn.microsoft.com/library/dn835015.aspx
[msdn-rest-api-create-stream-analytics-transformation]: https://msdn.microsoft.com/library/dn835007.aspx

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
 

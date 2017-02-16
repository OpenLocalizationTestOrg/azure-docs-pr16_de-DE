<properties 
    pageTitle="Behandeln von Problemen mit Azure Data Factory" 
    description="Informationen Sie zum Behandeln von Problemen im Zusammenhang mit Azure Data Factory." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/31/2016" 
    ms.author="spelluru"/>

# <a name="troubleshoot-data-factory-issues"></a>Behandeln von Problemen mit Daten Factory
Dieser Artikel enthält Tipps zur Problembehandlung für Probleme bei Verwendung von Azure Data Factory. In diesem Artikel werden nicht alle möglichen Probleme bei Verwendung des Diensts aufgelistet, aber es behandelt einige Probleme und allgemeine Tipps zur Problembehandlung.   

## <a name="troubleshooting-tips"></a>Tipps zur Problembehandlung

### <a name="error-the-subscription-is-not-registered-to-use-namespace-microsoftdatafactory"></a>Fehler: Das Abonnement ist nicht registriert, um Namespace 'Microsoft.DataFactory' verwenden
Wenn dieser Fehler angezeigt wird, wurde der Azure-Daten Factory-Anbieter für Ressourcen auf Ihrem Computer nicht registriert. Gehen Sie wie folgt vor: 

1. Starten Sie Azure PowerShell. 
2. Melden Sie sich bei Ihrem Azure-Konto mit dem folgenden Befehl aus.
        Login-AzureRmAccount 
3. Führen Sie zum Registrieren des Azure-Daten Factory-Anbieters den folgenden Befehl ein.
        Register-AzureRmResourceProvider - ProviderNamespace Microsoft.DataFactory

### <a name="problem-unauthorized-error-when-running-a-data-factory-cmdlet"></a>Problem: Nicht autorisierte zurück, wenn ein Data Factory-Cmdlet ausgeführt
Sie sind wahrscheinlich nicht die rechts Azure-Konto oder das Abonnement mit der PowerShell Azure verwenden. Verwenden Sie die folgenden Cmdlets, um die Rechte Azure-Konto und das Abonnement zur Verwendung mit der PowerShell Azure auszuwählen. 

1. Login-AzureRmAccount - verwenden Sie die richtigen Benutzer-ID und Ihr Kennwort ein
2. Get-AzureRmSubscription - alle Abonnements für das Konto anzeigen. 
3. Wählen Sie-AzureRmSubscription &lt;Abonnementname&gt; -wählen Sie das richtige Abonnement aus. Verwenden Sie dasselbe, das Sie verwenden, um eine Factory Daten im Azure-Portal zu erstellen.

### <a name="problem-fail-to-launch-data-management-gateway-express-setup-from-azure-portal"></a>Problem: Ein Fehler treten Sie auf, um Daten Management Gateway Express einrichten aus Azure-Portal zu starten.
Express-Setup für das Datenverwaltungsgateway ist Internet Explorer oder einen kompatiblen Microsoft ClickOnce-Webbrowser erforderlich. Wenn die Express-Installation kann nicht gestartet werden, führen Sie eine der folgenden Aktionen aus: 

- Verwenden Sie Internet Explorer oder einen kompatiblen Webbrowser Microsoft ClickOnce.

    Wenn Sie Chrome verwenden, wechseln Sie zum [Chrome Web Store](https://chrome.google.com/webstore/), suchen mit "ClickOnce" Schlüsselwort, wählen Sie eine Erweiterung ClickOnce und zu installieren. 
    
    Führen Sie den gleichen für Firefox (installieren-add-in). Klicken Sie auf Menü öffnen Schaltfläche auf der Symbolleiste (drei horizontale Linien in der oberen rechten Ecke), klicken Sie auf Add-ons, mit dem Schlüsselwort "ClickOnce" Suchen Sie, wählen Sie eine Erweiterung ClickOnce, und installieren Sie es. 

- Verwenden des **Manuellen Einrichten** links angezeigt wird, klicken Sie auf der gleichen vorher in das Portal. Dieser Ansatz verwenden zum Herunterladen der Datei für die Installation und manuell ausführen. Nachdem die Installation erfolgreich ist, wird das Dialogfeld Daten Management Gateway-Konfiguration. Kopieren Sie die **Taste** auf dem Portal Bildschirm, und verwenden Sie es im Konfigurations-Manager, um das Gateway mit dem Dienst manuell zu registrieren.  

### <a name="problem-fail-to-connect-to-on-premises-sql-server"></a>Problem: Bei lokalen SQL Server Verbindung herstellen 
Starten Sie auf dem Gatewaycomputer **Daten Management Gateway-Konfigurations-Manager** , und verwenden Sie die Registerkarte **Problembehandlung** zum Testen der Verbindung mit SQL Server aus dem Gatewaycomputer. Finden Sie unter [Behandeln von Gateway Probleme](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) Tipps zur Behebung von Verbindung/Gateway zusammenhängende Probleme.   
 

### <a name="problem-input-slices-are-in-waiting-state-for-ever"></a>Problem: Eingabe Segmente sind Zustand für jemals warten

Die Segmente könnte Zustand unterschiedlichen Gründen **Warten** . Auftretende Ursache ist, dass die **externen** Eigenschaft nicht auf **true**festgelegt ist. Beliebiges Dataset, der außerhalb des Gültigkeitsbereichs von Azure Data Factory erstellt wird, sollten mit **externen** Eigenschaft gekennzeichnet werden. Diese Eigenschaft zeigt an, dass die Daten externen und nicht gesicherten durch Pipelines innerhalb der Factory Daten. Die Datensegmente werden als **bereit** gekennzeichnet, sobald die Daten im entsprechenden Store erhältlich ist. 

Finden Sie im folgende Beispiel für die Verwendung der **externen** Eigenschaft aus. Optional können Sie angeben, **ExternalData*** beim Festlegen von externen true.

[Datasets](data-factory-create-datasets.md) finden Sie im Artikel weitere Details zu dieser Eigenschaft.
    
    {
      "name": "CustomerTable",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "MyLinkedService",
        "typeProperties": {
          "folderPath": "MyContainer/MySubFolder/",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "rowDelimiter": ";"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          }
        }
      }
    }

Um den Fehler zu beheben, Hinzufügen von **externen** Eigenschaft und optional **ExternalData** im Abschnitt zu den JSON-Definition der Eingabewerte Tabelle und die Tabelle neu zu erstellen. 

### <a name="problem-hybrid-copy-operation-fails"></a>Problem: Hybrid Kopiervorgang schlägt fehl
Finden Sie unter [Behandeln von Problemen mit Gateway](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) für Schritte zum Behandeln von Problemen mit Kopieren/aus einem lokalen Datenspeicher verwenden das Datenverwaltungsgateway. 

### <a name="problem-on-demand-hdinsight-provisioning-fails"></a>Problem: Bei Bedarf HDInsight provisioning fehlschlägt
Wenn Sie einen verknüpften Dienst des Typs HDInsightOnDemand verwenden zu können, müssen Sie eine LinkedServiceName angeben, die auf einer Azure BLOB-Speicher verweist. Factory Datendienst verwendet diese Speicher zum Speichern von Protokollen und Hilfsdateien für Ihren bei Bedarf HDInsight Cluster an.  Manchmal Bereitstellung von einer bei Bedarf HDInsight Cluster schlägt fehl, und der folgende Fehler:

        Failed to create cluster. Exception: Unable to complete the cluster create operation. Operation failed with code '400'. Cluster left behind state: 'Error'. Message: 'StorageAccountNotColocated'.

Dieser Fehler gibt normalerweise an, dass die Position des Speicherkontos in der LinkedServiceName angegebenen nicht an derselben Stelle Data Center ist, in dem die Bereitstellung von HDInsight ansteht. Beispiel: Wenn Ihre Daten Factory sich Westen US befindet und Azure-Speicher im südostasiatischen US, schlägt die Bereitstellung nach Bedarf in Westen US.

Darüber hinaus besteht eine zweite JSON-Eigenschaft AdditionalLinkedServiceNames, zusätzlichen Speicherkonten in bei Bedarf HDInsight angegeben werden können. Diese zusätzlichen verknüpfte Speicherkonten an derselben Stelle als HDInsight Cluster sollten, oder diese nicht mit dem gleichen Fehler.

### <a name="problem-custom-net-activity-fails"></a>Problem: Benutzerdefinierte .NET Aktivität schlägt fehl
Die detaillierten Schritte finden Sie unter [Debuggen einer Verkaufspipeline mit benutzerdefinierter Aktivität](data-factory-use-custom-activities.md#debug-the-pipeline) . 

## <a name="use-azure-portal-to-troubleshoot"></a>Verwenden Sie zum Behandeln von Problemen mit Azure-portal 

### <a name="using-portal-blades"></a>Verwenden des Portals blades
Schritte finden Sie unter [Monitor Verkaufspipeline](data-factory-build-your-first-pipeline-using-editor.md#monitor-pipeline) . 

### <a name="using-monitor-and-manage-app"></a>Verwenden Sie überwachen und Verwalten von App
Finden Sie unter [Überwachen und Verwalten von Factory Datenpipelines verwenden, überwachen und Verwalten von App](data-factory-monitor-manage-app.md) Details. 

## <a name="use-azure-powershell-to-troubleshoot"></a>Verwenden Sie zum Behandeln von Problemen mit Azure PowerShell

### <a name="use-azure-powershell-to-troubleshoot-an-error"></a>Verwenden von Azure PowerShell zur Problembehandlung  
Details finden Sie unter [Monitor Daten Factory Pipelines Azure PowerShell verwenden](data-factory-build-your-first-pipeline-using-powershell.md#monitor-pipeline) . 


[adfgetstarted]: data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[use-custom-activities]: data-factory-use-custom-activities.md
[troubleshoot]: data-factory-troubleshoot.md
[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456
[json-scripting-reference]: http://go.microsoft.com/fwlink/?LinkId=516971

[azure-portal]: https://portal.azure.com/

[image-data-factory-troubleshoot-with-error-link]: ./media/data-factory-troubleshoot/DataFactoryWithErrorLink.png

[image-data-factory-troubleshoot-datasets-with-errors-blade]: ./media/data-factory-troubleshoot/DatasetsWithErrorsBlade.png

[image-data-factory-troubleshoot-table-blade-with-problem-slices]: ./media/data-factory-troubleshoot/TableBladeWithProblemSlices.png

[image-data-factory-troubleshoot-activity-run-with-error]: ./media/data-factory-troubleshoot/ActivityRunDetailsWithError.png

[image-data-factory-troubleshoot-dataslice-blade-with-active-runs]: ./media/data-factory-troubleshoot/DataSliceBladeWithActivityRuns.png

[image-data-factory-troubleshoot-walkthrough2-with-errors-link]: ./media/data-factory-troubleshoot/Walkthrough2WithErrorsLink.png

[image-data-factory-troubleshoot-walkthrough2-datasets-with-errors]: ./media/data-factory-troubleshoot/Walkthrough2DataSetsWithErrors.png

[image-data-factory-troubleshoot-walkthrough2-table-with-problem-slices]: ./media/data-factory-troubleshoot/Walkthrough2TableProblemSlices.png

[image-data-factory-troubleshoot-walkthrough2-slice-activity-runs]: ./media/data-factory-troubleshoot/Walkthrough2DataSliceActivityRuns.png

[image-data-factory-troubleshoot-activity-run-details]: ./media/data-factory-troubleshoot/Walkthrough2ActivityRunDetails.png
 
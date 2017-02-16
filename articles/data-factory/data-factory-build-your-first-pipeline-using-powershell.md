<properties
    pageTitle="Erstellen Ihrer erste Daten Factory (PowerShell) | Microsoft Azure"
    description="In diesem Lernprogramm erstellen Sie eine Stichprobe Azure Data Factory Verkaufspipeline Azure PowerShell verwenden."
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="monicar"
/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/16/2016"
    ms.author="spelluru"/>

# <a name="tutorial-build-your-first-azure-data-factory-using-azure-powershell"></a>Lernprogramm: Erstellen Sie Ihrer erste Azure-Daten Factory mithilfe der PowerShell Azure
> [AZURE.SELECTOR]
- [Übersicht und erforderliche Komponenten](data-factory-build-your-first-pipeline.md)
- [Azure-portal](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [Ressourcenmanager-Vorlage](data-factory-build-your-first-pipeline-using-arm.md)
- [REST-API](data-factory-build-your-first-pipeline-using-rest-api.md)

In diesem Artikel verwenden Sie Azure PowerShell zum Erstellen Ihrer ersten Azure-Daten Factory. 

## <a name="prerequisites"></a>Erforderliche Komponenten

- [Lernprogramm Übersicht](data-factory-build-your-first-pipeline.md) Artikel lesen Sie, und führen Sie die Schritte **Voraussetzung** aus.
- Anweisungen Sie [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) -Artikel auf die neueste Version von Azure PowerShell auf Ihrem Computer installieren.
- (optional) In diesem Artikel befasst sich nicht auf alle Daten Factory Cmdlets aus. Umfassende Dokumentation auf Daten Factory-Cmdlets finden Sie unter [Daten Factory Cmdlet verweisen](https://msdn.microsoft.com/library/dn820234.aspx) . 

## <a name="create-data-factory"></a>Erstellen von Daten factory

In diesem Schritt verwenden Sie Azure PowerShell zum Erstellen einer Azure-Daten Factory mit dem Namen **FirstDataFactoryPSH**. Eine Factory Daten kann eine oder mehrere Rohrleitungen haben. Eine Verkaufspipeline kann eine oder mehrere Aktivitäten enthalten. Angenommen, eine Kopie Aktivität zum Kopieren von Daten aus einer Quelle zu einem Ziel-Datenspeicher und eine HDInsight Struktur Aktivität Struktur Skript aus, um die Eingabedaten transformieren ausführen. Beginnen wir mit die Daten Factory in diesem Schritt erstellen. 

1. Starten Sie Azure PowerShell, und führen Sie den folgenden Befehl aus. Lassen Sie Azure PowerShell bis zum Ende des Lernprogramms geöffnet. Wenn Sie schließen und erneut öffnen, müssen Sie diese Befehle erneut ausführen.
    - Führen Sie `Login-AzureRmAccount` , und geben Sie den Benutzernamen und das Kennwort für die Anmeldung bei der Azure-Portal.  
    - Führen Sie `Get-AzureRmSubscription` alle Abonnements für dieses Konto anzeigen.
    - Führen Sie `Get-AzureRmSubscription -SubscriptionName <SUBSCRIPTION NAME> | Set-AzureRmContext` um das Abonnement auszuwählen, die Sie mit arbeiten möchten. Abonnement sollte der identisch sein, die Sie in der Azure-Portal verwendet haben.
3. Erstellen einer Azure Ressourcengruppe mit dem Namen **ADFTutorialResourceGroup** , indem Sie den folgenden Befehl ausführen:

        New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"

    Einige der Schritte in diesem Lernprogramm wird davon ausgegangen, dass Sie die Ressourcengruppe mit dem Namen ADFTutorialResourceGroup verwenden. Wenn Sie eine andere Ressourcengruppe verwenden, müssen Sie es anstelle von ADFTutorialResourceGroup in diesem Lernprogramm verwenden.
4. Führen Sie das Cmdlet **Neu-AzureRmDataFactory** aus, das mit dem Namen **FirstDataFactoryPSH**Factory Daten erstellt wird.  

        New-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name FirstDataFactoryPSH –Location "West US"


Beachten Sie die folgenden Punkte:
 
- Der Name der Factory Daten Azure muss global eindeutig sein. Wenn die Fehlermeldung **Factory Data Source Name "FirstDataFactoryPSH" ist nicht verfügbar**angezeigt wird, ändern Sie den Namen (beispielsweise YournameFirstDataFactoryPSH). Verwenden Sie diesen Namen anstelle von ADFTutorialFactoryPSH während der Ausführung der Schritte in diesem Lernprogramm. Finden Sie unter [Data Factory - Regeln zur Benennung von](data-factory-naming-rules.md) Thema Benennungskonventionen für Daten Factory Elemente.
- Um Daten Factory-Instanzen erstellen zu können, müssen Sie ein Mitwirkender/Administrator des Abonnements Azure sein
- Der Name der Factory Daten möglicherweise als DNS-Namen in der Zukunft und somit werden öffentlich sichtbar registriert werden.
- Wenn Sie die Fehlermeldung: "**Abonnement ist nicht registriert, um den Namespace Microsoft.DataFactory verwenden**", führen Sie eine der folgenden Aktionen, und versuchen Sie erneut veröffentlichen: 

    - Führen Sie in Azure PowerShell zum Registrieren des Daten Factory-Anbieters den folgenden Befehl aus: 
        
            Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    
        Sie können dem folgenden Befehl aus, um zu bestätigen, dass die Daten Factory ausführen, wenn Anbieter registriert ist: 
    
            Get-AzureRmResourceProvider
    - Melden Sie sich mit dem Azure Abonnement in der [Azure-Portal](https://portal.azure.com) und navigieren Sie zu einer Blade Daten Factory (oder) erstellen Sie eine Factory Daten im Azure-Portal. Diese Aktion registriert automatisch den Anbieter für Sie.

Vor dem Erstellen einer Verkaufspipeline, müssen Sie zuerst ein paar Daten Factory-Elemente zu erstellen. Zuerst erstellen Sie ein verknüpftes Services, um Daten Stores/berechnet an den Datenspeicher verknüpfen, Eingabe definieren und ausgeben Datasets ein-/Ausgabe von Daten in verknüpften Datenspeicher darstellen, und erstellen Sie dann die Verkaufspipeline mit einer Aktivität, die diese Datasets verwendet. 

## <a name="create-linked-services"></a>Erstellen von verknüpften Diensten 
In diesem Schritt verknüpfen Sie Ihr Konto Azure-Speicher und eine bei Bedarf Azure HDInsight Cluster mit Ihrer Daten Factory ein. Das Konto Azure-Speicher enthält die Eingabe- und Daten für die Verkaufspipeline, in diesem Beispiel. Der Dienst HDInsight verknüpft dient zum Ausführen von Struktur Skript in die Aktivität von der Verkaufspipeline in diesem Beispiel angegeben. Welche Daten identifizieren Store/berechnen Services werden in Ihrem Szenario verwendet und Dienste Fabrik Daten durch Erstellen von verknüpften Diensten verknüpfen.

### <a name="create-azure-storage-linked-service"></a>Erstellen von Azure verknüpft Speicherdienst
In diesem Schritt verknüpfen Sie Ihr Konto Azure-Speicher mit Ihrer Daten Factory an. Sie verwenden das gleiche Speicher Azure-Konto zum Speichern/Ausgang Daten und der HQL-Skriptdatei aus.

1. Erstellen einer JSON-Datei mit dem Namen StorageLinkedService.json im Ordner "C:\ADFGetStarted" mit dem folgenden Inhalt. Erstellen Sie den Ordner ADFGetStarted aus, wenn es nicht bereits vorhanden ist.

        {
            "name": "StorageLinkedService",
            "properties": {
                "type": "AzureStorage",
                "description": "",
                "typeProperties": {
                    "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
                }
            }
        }

    Ersetzen Sie **Kontonamen** durch den Namen Ihrer Azure-Speicher-Konto und **kontoschlüssel** zusammen mit der Tastenkombination für das Konto ein Azure-Speicher. So erhalten Sie Ihre Zugriffstaste Speicher finden Sie unter [anzeigen, kopieren und neu generieren Speicher Zugriffstasten](https://azure.microsoft.com/documentation/articles/storage-create-storage-account/#view-copy-and-regenerate-storage-access-keys).

2. Azure PowerShell wechseln Sie zu dem Ordner ADFGetStarted.
3. Sie können das Cmdlet " **New-AzureRmDataFactoryLinkedService** " verwenden, das einen verknüpften Dienst erstellt wird. Dieses Cmdlet und anderen Daten Factory-Cmdlets, die Sie in diesem Lernprogramm verwenden erfordert Werte für die Parameter *ResourceGroupName* und *DataFactoryName* übergeben. Alternativ können Sie **Get-AzureRmDataFactory** zum Abrufen von **DataFactory** -Objekt, und das Objekt zu übergeben, ohne die Eingabe von *ResourceGroupName* und *DataFactoryName* jedes Mal, wenn Sie ein Cmdlet ausführen. Führen Sie den folgenden Befehl die Ausgabe das Cmdlet " **Get-AzureRmDataFactory** " eine Variable **$df** zuweisen.

        $df=Get-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name FirstDataFactoryPSH

4. Führen Sie nun das **Neu AzureRmDataFactoryLinkedService** Cmdlet verknüpften **StorageLinkedService** Dienst erstellt.

        New-AzureRmDataFactoryLinkedService $df -File .\StorageLinkedService.json

    Wenn Sie Sie das Cmdlet " **Get-AzureRmDataFactory führen hadn't** ", und die Ausgabe der Variablen **$df** zugewiesen, Sie die Werte für die Parameter *ResourceGroupName* und *DataFactoryName* wie folgt angeben müssen.

        New-AzureRmDataFactoryLinkedService -ResourceGroupName ADFTutorialResourceGroup -DataFactoryName FirstDataFactoryPSH -File .\StorageLinkedService.json

    Wenn Sie in der Mitte des Lernprogramms Azure PowerShell geschlossen haben, müssen Sie führen Sie das Cmdlet " **Get-AzureRmDataFactory** " beim nächsten start Azure PowerShell, um das Lernprogramm abzuschließen.

### <a name="create-azure-hdinsight-linked-service"></a>Erstellen von Azure HDInsight verknüpft-Dienst
In diesem Schritt verknüpfen Sie einen bei Bedarf HDInsight Cluster mit Ihrer Daten Factory an. HDInsight Cluster wird automatisch zur Laufzeit erstellt und gelöscht, nachdem es für die angegebene Zeitspanne Verarbeitung und im Leerlauf ausgeführt werden. Sie können eigene HDInsight Cluster anstelle von einem bei Bedarf HDInsight Cluster verwenden. Details finden Sie unter [Verknüpfte Services zu berechnen](data-factory-compute-linked-services.md) .  

1. Erstellen einer JSON-Datei mit dem Namen **HDInsightOnDemandLinkedService**.json im Ordner " **C:\ADFGetStarted** " mit dem folgenden Inhalt.

        {
          "name": "HDInsightOnDemandLinkedService",
          "properties": {
            "type": "HDInsightOnDemand",
            "typeProperties": {
              "version": "3.2",
              "clusterSize": 1,
              "timeToLive": "00:30:00",
              "linkedServiceName": "StorageLinkedService"
            }
          }
        }

    Die folgende Tabelle enthält eine Beschreibung für die JSON-Eigenschaften in der Codeausschnitt verwendet werden:

  	| Eigenschaft | Beschreibung |
  	| :------- | :---------- |
  	| Version | Gibt an, dass die Version von der HDInsight benutzerspezifisch 3,2 erstellt. | 
  	| ClusterSize | Gibt die Größe des HDInsight Cluster an. | 
  	| TimeToLive | Gibt an, dass die im Leerlauf Zeit für den Cluster HDInsight, bevor sie gelöscht wird. |
  	| linkedServiceName | Gibt das Speicherkonto, mit dem die Protokolle speichern, die von HDInsight generiert werden |

    Beachten Sie die folgenden Punkte: 
    
    - Die Daten Factory erstellt einen **Windows-basierten** HDInsight Cluster, mit der JSON. Sie auch haben diese erstellen Sie einen **Linux-basierten** HDInsight Cluster. Details finden Sie [Bei Bedarf HDInsight verknüpfte Dienst](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) . 
    - Sie können **Eigene HDInsight Cluster** anstelle von einem bei Bedarf HDInsight Cluster verwenden. Details finden Sie unter [Verknüpfte HDInsight-Dienst](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) .
    - HDInsight Cluster erstellt einen **standardmäßige Container** in den Blob-Speicher, die, den Sie in das JSON (**LinkedServiceName**) angegeben haben. HDInsight wird dieser Container nicht gelöscht, wenn der Cluster gelöscht wird. Dieses Verhalten ist beabsichtigt. Mit dem Dienst bei Bedarf HDInsight verknüpft wird ein HDInsight Cluster erstellt jedes Mal, wenn ein Segment verarbeitet wird, es sei denn, es ist ein vorhandener live Cluster (**TimeToLive**). Cluster wird automatisch gelöscht, wenn die Verarbeitung abgeschlossen ist.
    
        Als weitere Segmente verarbeitet werden, wird in Ihrem Azure Blob Storage viele Container. Wenn Sie nicht zur Behandlung dieses Problems der Einzelvorgänge benötigen, möchten Sie möglicherweise löschen, um die Speicherkosten für reduzieren. Führen Sie die Namen dieser Container ein Muster: "Adf**Yourdatafactoryname**-**Linkedservicename**- Datetimestamp". Mit Tools wie [Microsoft Speicher-Explorer](http://storageexplorer.com/) zum Container in Ihrer Azure Blob-Speicher löschen.

    Details finden Sie [Bei Bedarf HDInsight verknüpfte Dienst](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) . 
2. Führen Sie das **New-AzureRmDataFactoryLinkedService** -Cmdlet, das verknüpften Dienst namens HDInsightOnDemandLinkedService erstellt wird.

        New-AzureRmDataFactoryLinkedService $df -File .\HDInsightOnDemandLinkedService.json


## <a name="create-datasets"></a>Datasets erstellen
In diesem Schritt erstellen Sie Datasets zum Darstellen der Eingabe und Ausgabedaten für die Verarbeitung von Struktur. Diese Datasets finden Sie in der **StorageLinkedService** , die Sie zuvor in diesem Lernprogramm erstellt haben. Die verknüpfte Dienstpunkte einer Azure-Speicher-Konto und Datasets angeben Container, Ordner, Dateiname in der Speicher, der Eingabe und Ausgabedaten.   

### <a name="create-input-dataset"></a>Erstellen von dataset
1. Erstellen einer JSON-Datei mit dem Namen **InputTable.json** im Ordner " **C:\ADFGetStarted** " mit dem folgenden Inhalt:

        {
            "name": "AzureBlobInput",
            "properties": {
                "type": "AzureBlob",
                "linkedServiceName": "StorageLinkedService",
                "typeProperties": {
                    "fileName": "input.log",
                    "folderPath": "adfgetstarted/inputdata",
                    "format": {
                        "type": "TextFormat",
                        "columnDelimiter": ","
                    }
                },
                "availability": {
                    "frequency": "Month",
                    "interval": 1
                },
                "external": true,
                "policy": {}
            }
        } 

    Die JSON definiert ein Dataset namens **AzureBlobInput**, die Eingabedaten für eine Aktivität in der Verkaufspipeline darstellt. Darüber hinaus gibt es, dass die Eingabedaten in den Blob-Container **Adfgetstarted** bezeichnet und Ordner mit dem Namen **Inputdata**befindet.

    Die folgende Tabelle enthält eine Beschreibung für die JSON-Eigenschaften in der Codeausschnitt verwendet werden:

  	| Eigenschaft | Beschreibung |
  	| :------- | :---------- |
  	| Typ | Die Eigenschaft wird auf AzureBlob festgelegt, da die Daten in Azure Blob-Speicher befinden. |  
  	| linkedServiceName | verweist auf die StorageLinkedService, die Sie zuvor erstellt haben. |
  	| Dateiname | Diese Eigenschaft ist optional. Wenn Sie diese Eigenschaft nicht angeben, werden alle Dateien aus den Ordnerpfad entnommen. In diesem Fall wird nur das input.log verarbeitet. |
  	| Typ | Die Protokolldateien werden im Text-Format, damit wir TextFormat verwenden. | 
  	| columnDelimiter | Spalten in die Protokolldateien werden durch das Komma (,) getrennt. |
  	| Häufigkeit/Intervall | Legen Sie auf den Monat und das Intervall Häufigkeit ist 1, was bedeutet, dass die Eingabewerte Segmente monatlich verfügbar sind. | 
  	| externe | True, wenn die Eingabedaten nicht vom Dienst Daten Factory generiert werden, wird diese Eigenschaft festgelegt. | 

2. Führen Sie den folgenden Befehl in Azure PowerShell Daten Factory Dataset zu erstellen:

        New-AzureRmDataFactoryDataset $df -File .\InputTable.json

### <a name="create-output-dataset"></a>Die Ausgabe Dataset erstellen
Erstellen Sie jetzt das Ausgabe-Dataset aus, um die Ausgabedaten Azure Blob-Speicher gehörende Kehrmatrix darstellen.

1. Erstellen einer JSON-Datei mit dem Namen **OutputTable.json** im Ordner " **C:\ADFGetStarted** " mit dem folgenden Inhalt:

        {
          "name": "AzureBlobOutput",
          "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
              "folderPath": "adfgetstarted/partitioneddata",
              "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
              }
            },
            "availability": {
              "frequency": "Month",
              "interval": 1
            }
          }
        }

    Die JSON definiert ein Dataset namens **AzureBlobOutput**, die Ausgabedaten für eine Aktivität in der Verkaufspipeline darstellt. Darüber hinaus gibt es, dass die Ergebnisse in der Blob-Container **Adfgetstarted** bezeichnet und Ordner mit dem Namen **Partitioneddata**gespeichert werden. Im Abschnitt **Verfügbarkeit** der gibt an, dass das Ausgabe Dataset monatlich erzeugt wird.

2. Führen Sie den folgenden Befehl in Azure PowerShell Daten Factory Dataset zu erstellen:

        New-AzureRmDataFactoryDataset $df -File .\OutputTable.json

## <a name="create-pipeline"></a>Erstellen der Verkaufspipeline
In diesem Schritt erstellen Sie Ihre erste Verkaufspipeline mit einer Aktivität **HDInsightHive** aus. Eingabe Segment steht monatlich (Häufigkeit: Monat, Intervall: 1), Ausgabe Segment monatlich erstellt wird und die Scheduler-Eigenschaft für die Aktivität auch monatlich festgelegt ist. Die Einstellungen für die Ausgabe Dataset und die Aktivität Scheduler müssen übereinstimmen. Derzeit ist Ausgabe Dataset, was den Terminplan veranlasst, müssen Sie ein Dataset Ausgabe erstellen, selbst wenn die Aktivität keine Ausgabe erzeugt. Wenn die Aktivität jede Eingabe übernehmen möchten nicht, können Sie überspringen des Eingabe-Dataset zu erstellen. Am Ende des in diesem Abschnitt werden die Eigenschaften in der folgenden JSON verwendete erläutert. 


1. Erstellen einer JSON-Datei mit dem Namen MyFirstPipelinePSH.json im Ordner "C:\ADFGetStarted" mit dem folgenden Inhalt:

    > [AZURE.IMPORTANT] Ersetzen Sie **Storageaccountname** durch den Namen Ihres Kontos Speicher in den JSON.
        
        {
            "name": "MyFirstPipeline",
            "properties": {
                "description": "My first Azure Data Factory pipeline",
                "activities": [
                    {
                        "type": "HDInsightHive",
                        "typeProperties": {
                            "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                            "scriptLinkedService": "StorageLinkedService",
                            "defines": {
                                "inputtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/inputdata",
                                "partitionedtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/partitioneddata"
                            }
                        },
                        "inputs": [
                            {
                                "name": "AzureBlobInput"
                            }
                        ],
                        "outputs": [
                            {
                                "name": "AzureBlobOutput"
                            }
                        ],
                        "policy": {
                            "concurrency": 1,
                            "retry": 3
                        },
                        "scheduler": {
                            "frequency": "Month",
                            "interval": 1
                        },
                        "name": "RunSampleHiveActivity",
                        "linkedServiceName": "HDInsightOnDemandLinkedService"
                    }
                ],
                "start": "2016-04-01T00:00:00Z",
                "end": "2016-04-02T00:00:00Z",
                "isPaused": false
            }
        }

    In den JSON-Codeausschnitt erstellen Sie eine Verkaufspipeline, der eine einzelne Aktivität besteht, die Struktur an Prozess Daten auf einem Cluster HDInsight verwendet.
    
    Die Struktur-Skriptdatei, **partitionweblogs.hql**, wird in der Azure-Speicher-Konto (angegeben durch die ScriptLinkedService, die als **StorageLinkedService**bezeichnet), und klicken Sie im Ordner " **Skript** " in den Container **Adfgetstarted**gespeichert.

    Im Abschnitt **definiert** wird verwendet, um die Einstellungen für die Laufzeit angeben, an das Skript Struktur als Struktur Konfigurationswerte übergeben werden (z. B. ${Hiveconf: inputtable}, ${Hiveconf:partitionedtable}).

    Die Eigenschaften **Starten** und **Beenden** der Verkaufspipeline gibt den aktiven Zeitraum der Verkaufspipeline an.

    In der Aktivität JSON Geben Sie an, dass das Skript Struktur für das Berechnen von der **LinkedServiceName** – **HDInsightOnDemandLinkedService**angegebenen ausgeführt wird.

    > [ACOM. Hinweis] finden Sie unter [Aufbau einer Verkaufspipeline](data-factory-create-pipelines.md#anatomy-of-a-pipeline) Details im Beispiel verwendete JSON-Eigenschaften. 
2.  Bestätigen Sie, dass Sie die Datei **input.log** in den Ordner **Adfgetstarted/Inputdata** Azure Blob-Speicher finden Sie unter, und führen Sie den folgenden Befehl zum Bereitstellen der Verkaufspipeline. Da die ** **Starten** und** Endzeiten werden in der Vergangenheit und **IsPaused** auf falsch festgelegt ist, wird der Verkaufspipeline (Aktivität in der Verkaufspipeline) unmittelbar nach der Bereitstellung ausgeführt. 

        New-AzureRmDataFactoryPipeline $df -File .\MyFirstPipelinePSH.json
5. Herzlichen Glückwunsch, Sie haben Ihre erste Verkaufspipeline mithilfe der PowerShell Azure erfolgreich erstellt!

## <a name="monitor-pipeline"></a>Monitor Verkaufspipeline
In diesem Schritt verwenden Sie Azure PowerShell zum Überwachen der Aktivitäten in einer Factory Azure-Daten in ein.

1. **Get-AzureRmDataFactory** ausführen und die Ausgabe einer Variablen **$df** zuzuweisen.

        $df=Get-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name FirstDataFactoryPSH

2. Führen Sie **Get-AzureRmDataFactorySlice** , um die Details zu der der **EmpSQLTable**, alle Segmente abzurufen, die Ausgabetabelle für die der Verkaufspipeline ist.  

        Get-AzureRmDataFactorySlice $df -DatasetName AzureBlobOutput -StartDateTime 2016-04-01

    Beachten Sie, dass die StartDateTime Sie hier angeben, dass der in der Verkaufspipeline JSON angegebene Zeit beginnen. Es sollte ähnlich wie der folgende Ausgabe angezeigt.

        ResourceGroupName : ADFTutorialResourceGroup
        DataFactoryName   : FirstDataFactoryPSH
        DatasetName       : AzureBlobOutput
        Start             : 4/1/2016 12:00:00 AM
        End               : 4/2/2016 12:00:00 AM
        RetryCount        : 0
        State             : InProgress
        SubState          :
        LatencyStatus     :
        LongRetryCount    : 0


3. Ausführen **Get-AzureRmDataFactoryRun** können Sie die Details der Aktivität zu gelangen, die für einen bestimmten Speicherbereich ausgeführt wird.

        Get-AzureRmDataFactoryRun $df -DatasetName AzureBlobOutput -StartDateTime 2016-04-01

    Es sollte ähnlich wie der folgende Ausgabe angezeigt.
        
        Id                  : 0f6334f2-d56c-4d48-b427-d4f0fb4ef883_635268096000000000_635292288000000000_AzureBlobOutput
        ResourceGroupName   : ADFTutorialResourceGroup
        DataFactoryName     : FirstDataFactoryPSH
        DatasetName         : AzureBlobOutput
        ProcessingStartTime : 12/18/2015 4:50:33 AM
        ProcessingEndTime   : 12/31/9999 11:59:59 PM
        PercentComplete     : 0
        DataSliceStart      : 4/1/2016 12:00:00 AM
        DataSliceEnd        : 4/2/2016 12:00:00 AM
        Status              : AllocatingResources
        Timestamp           : 12/18/2015 4:50:33 AM
        RetryAttempt        : 0
        Properties          : {}
        ErrorMessage        :
        ActivityName        : RunSampleHiveActivity
        PipelineName        : MyFirstPipeline
        Type                : Script

    Sie können dieses Cmdlet ausgeführt beibehalten, bis Sie das Segment im **bereit** oder **fehlgeschlagen** Zustand angezeigt wird. Wenn das Segment eingeschaltet ist, überprüfen Sie den Ordner **Partitioneddata** im Container **Adfgetstarted** in Ihrem BLOB-Speicher für die Ausgabedaten.  Erstellung einer bei Bedarf HDInsight Cluster dauert normalerweise einige Zeit.

    ![Ausgabedaten](./media/data-factory-build-your-first-pipeline-using-powershell/three-ouptut-files.png)


> [AZURE.IMPORTANT] 
> Erstellung einer bei Bedarf HDInsight Cluster normalerweise dauert einige Zeit (ungefähr 20 Minuten). Daher erwarten der Verkaufspipeline werden müssen, **etwa 30 Minuten** das Segment Verarbeitungszeit.  
> 
> Wenn das Segment erfolgreich verarbeitet wird, erhält die Eingabe Datei gelöscht. Daher, wenn Sie das Segment erneut ausführen, oder führen Sie das Lernprogramm erneut möchten, Hochladen Sie eingegebenen Datei (input.log) in den Ordner Inputdata des Containers Adfgetstarted.

## <a name="summary"></a>Zusammenfassung 
In diesem Lernprogramm haben Sie eine Fabrik Azure-Daten zum Verarbeiten von Daten durch Ausführen der Struktur Skripts auf einem HDInsight Hadoop Cluster erstellt. Sie verwendet die Daten Factory-Editor im Azure-Portal an, um führen Sie die folgenden Schritte aus:  

1.  Erstellt eine Azure- **Daten Factory**.
2.  Erstellt zwei **verknüpften Diensten**:
    1.  **Azure-Speicher** verknüpft Dienst Ihre Azure Blob-Speicher zu verknüpfen, der ein-/Ausgabe Dateien auf die Factory Daten enthält.
    2.  **Azure HDInsight** bei Bedarf verknüpfte Dienst zum Verknüpfen von eines bei Bedarf HDInsight Hadoop Clusters Fabrik Daten. Azure Data Factory erstellt eine HDInsight Hadoop Cluster lediglich-Time um Verarbeitung Eingabedaten und Ausgabedaten. 
3.  Erstellt zwei **Datasets**, mit denen Eingabe- und Daten für HDInsight Struktur Aktivität in der Verkaufspipeline beschrieben. 
4.  Erstellt eine **Verkaufspipeline** mit einer Aktivität **HDInsight Struktur** . 

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie eine Verkaufspipeline mit einer Transformation Aktivität (HDInsight-Aktivität) erstellt, die eine Struktur Skript für eine bei Bedarf Azure HDInsight Cluster ausgeführt wird. So verwenden Sie eine Kopie Aktivität zum Kopieren von Daten aus einer Azure Blob zu SQL Azure finden Sie unter [Lernprogramm: Kopieren von Daten aus einer Azure Blob in SQL Azure](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

## <a name="see-also"></a>Siehe auch
| Thema | Beschreibung |
| :---- | :---- |
| [Daten Factory-Cmdlet Bezug](https://msdn.microsoft.com/library/azure/dn820234.aspx) |  Finden Sie unter umfassenden Dokumentation auf Daten Factory-cmdlets |
| [Daten Transformationsaktivitäten](data-factory-data-transformation-activities.md) | Dieser Artikel enthält eine Liste der Daten Transformationsaktivitäten (z. B. HDInsight Struktur Transformation, die Sie in diesem Lernprogramm verwendeten) von Azure Daten Factory unterstützt. |
| [Planung und Ausführung](data-factory-scheduling-and-execution.md) | In diesem Artikel wird erläutert, die Planung und Ausführung Aspekte des Modells für Azure Data Factory-Anwendung. |
| [Pipelines](data-factory-create-pipelines.md) | In diesem Artikel können Sie die Pipelines und Aktivitäten in Azure Data Factory und deren Verwendung zum Erstellen von End-to-End-Daten basierende Workflows für Ihre Szenario oder Ihr Unternehmen zu verstehen. |
| [Datasets](data-factory-create-datasets.md) | In diesem Artikel können Sie die Datasets in Azure Data Factory zu verstehen.
| [Überwachen und Verwalten von Pipelines mit Azure Portals blades](data-factory-monitor-manage-pipelines.md) | Dieser Artikel beschreibt, wie überwachen, verwalten und Debuggen mit Azure Portals Blades Pipelines. |
| [Überwachen und Verwalten von Pipelines mit App für die Überwachung](data-factory-monitor-manage-app.md) | Dieser Artikel beschreibt, wie überwachen, verwalten und Debuggen Pipelines die Überwachung und Verwaltung App verwenden. 


<properties
    pageTitle="Erstellen Ihrer erste Daten Factory (REST) | Microsoft Azure"
    description="In diesem Lernprogramm erstellen Sie eine Stichprobe Azure Data Factory Verkaufspipeline Data Factory REST-API verwenden."
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

# <a name="tutorial-build-your-first-azure-data-factory-using-data-factory-rest-api"></a>Lernprogramm: Erstellen Sie Ihrer erste Azure-Daten Factory mit Daten Factory REST-API
> [AZURE.SELECTOR]
- [Übersicht und erforderliche Komponenten](data-factory-build-your-first-pipeline.md)
- [Azure-portal](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [Ressourcenmanager-Vorlage](data-factory-build-your-first-pipeline-using-arm.md)
- [REST-API](data-factory-build-your-first-pipeline-using-rest-api.md)

In diesem Artikel verwenden Sie die Daten Factory REST-API zum Erstellen Ihrer ersten Azure-Daten Factory.

## <a name="prerequisites"></a>Erforderliche Komponenten
- [Lernprogramm Übersicht](data-factory-build-your-first-pipeline.md) Artikel lesen Sie, und führen Sie die Schritte **Voraussetzung** aus.
- Installieren Sie [Aufrollen](https://curl.haxx.se/dlwiz/) auf Ihrem Computer aus. Sie können das Tool CURL mit weiteren Befehlen verwenden, um eine Factory Daten zu erstellen. 
- Führen Sie die Schritte in [diesem Artikel](../resource-group-create-service-principal-portal.md) , um: 
    1. Erstellen einer Webanwendung mit dem Namen **ADFGetStartedApp** in Azure Active Directory.
    2. Abrufen von **Client-ID** und **geheimen Schlüssel**. 
    3. Erste **Mandanten-ID**an. 
    4. Weisen Sie die Anwendung **ADFGetStartedApp** der **Daten Factory** Teilnehmerrolle.  
- Installieren der [Azure PowerShell](../powershell-install-configure.md).  
- Starten Sie **PowerShell** , und führen Sie den folgenden Befehl aus. Lassen Sie Azure PowerShell bis zum Ende des Lernprogramms geöffnet. Wenn Sie schließen und erneut öffnen, müssen Sie die Befehle erneut ausführen.
    1. Führen Sie **Login-AzureRmAccount aus** , und geben Sie den Benutzernamen und das Kennwort für die Anmeldung bei der Azure-Portal.  
    2. Führen Sie **Get-AzureRmSubscription** zum Anzeigen aller Abonnements für dieses Konto ein.
    3. Ausführen von **Get-AzureRmSubscription - SubscriptionName NameOfAzureSubscription | Set-AzureRmContext** das Abonnement auswählen, die Sie mit arbeiten möchten. Ersetzen Sie **NameOfAzureSubscription** durch den Namen Ihres Abonnements Azure ein. 
3. Erstellen einer Azure Ressourcengruppe mit dem Namen **ADFTutorialResourceGroup** , indem Sie den folgenden Befehl ausführen, in der PowerShell:  

        New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"

    Einige der Schritte in diesem Lernprogramm wird davon ausgegangen, dass Sie die Ressourcengruppe mit dem Namen ADFTutorialResourceGroup verwenden. Wenn Sie eine andere Ressourcengruppe verwenden, müssen Sie den Namen der Ressourcengruppe anstelle von ADFTutorialResourceGroup in diesem Lernprogramm verwenden.

## <a name="create-json-definitions"></a>Erstellen von JSON-Definitionen
Erstellen Sie die folgenden JSON-Dateien in den Ordner, in dem curl.exe befindet. 

### <a name="datafactoryjson"></a>DataFactory.JSON 
> [AZURE.IMPORTANT] Name muss global eindeutig sein, damit Sie möglicherweise Präfix/Suffix ADFCopyTutorialDF ausreicht einen eindeutigen Namen möchten. 

    {  
        "name": "FirstDataFactoryREST",  
        "location": "WestUS"
    }  

### <a name="azurestoragelinkedservicejson"></a>azurestoragelinkedservice.JSON
> [AZURE.IMPORTANT] Ersetzen von **Kontoname** und **Accountkey** mit Namen und den Schlüssel Ihres Kontos Azure-Speicher. So erhalten Sie Ihre Zugriffstaste Speicher finden Sie unter [anzeigen, kopieren und neu generieren Speicher Zugriffstasten](../storage/storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).

    {
        "name": "AzureStorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
        }
    }


### <a name="hdinsightondemandlinkedservicejson"></a>hdinsightondemandlinkedservice.JSON

    {
        "name": "HDInsightOnDemandLinkedService",
        "properties": {
            "type": "HDInsightOnDemand",
            "typeProperties": {
                "version": "3.2",
                "clusterSize": 1,
                "timeToLive": "00:30:00",
                "linkedServiceName": "AzureStorageLinkedService"
            }
        }
    }

Die folgende Tabelle enthält eine Beschreibung für die JSON-Eigenschaften in der Codeausschnitt verwendet werden:

| Eigenschaft | Beschreibung |
| :------- | :---------- |
| Version | Gibt an, dass die Version von der HDInsight benutzerspezifisch 3,2 erstellt. | 
| ClusterSize | Größe des HDInsight Cluster. | 
| TimeToLive | Gibt an, dass die im Leerlauf Zeit für den Cluster HDInsight, bevor sie gelöscht wird. |
| linkedServiceName | Gibt das Speicherkonto, mit dem die Protokolle speichern, die von HDInsight generiert werden |

Beachten Sie die folgenden Punkte: 

- Die Daten Factory erstellt einen **Windows-basierten** HDInsight Cluster, mit den oben angegebenen JSON. Sie auch haben diese erstellen Sie einen **Linux-basierten** HDInsight Cluster. Details finden Sie [Bei Bedarf HDInsight verknüpfte Dienst](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) . 
- Sie können **Eigene HDInsight Cluster** anstelle von einem bei Bedarf HDInsight Cluster verwenden. Details finden Sie unter [Verknüpfte HDInsight-Dienst](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) .
- HDInsight Cluster erstellt einen **standardmäßige Container** in den Blob-Speicher, die, den Sie in das JSON (**LinkedServiceName**) angegeben haben. HDInsight wird dieser Container nicht gelöscht, wenn der Cluster gelöscht wird. Dieses Verhalten ist beabsichtigt. Bei Bedarf verknüpft HDInsight Dienst eine HDInsight Cluster erstellt wird jedes Mal, wenn ein Segment verarbeitet wird, es sei denn, es ist ein vorhandenes live Cluster (**TimeToLive**) und wird gelöscht, wenn die Verarbeitung abgeschlossen ist.

    Als weitere Segmente verarbeitet werden, wird in Ihrem Azure Blob Storage viele Container. Wenn Sie nicht zur Behandlung dieses Problems der Einzelvorgänge benötigen, möchten Sie möglicherweise löschen, um die Speicherkosten für reduzieren. Führen Sie die Namen dieser Container ein Muster: "Adf**Yourdatafactoryname**-**Linkedservicename**- Datetimestamp". Mit Tools wie [Microsoft Speicher-Explorer](http://storageexplorer.com/) zum Container in Ihrer Azure Blob-Speicher löschen.

Details finden Sie [Bei Bedarf HDInsight verknüpfte Dienst](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) . 

### <a name="inputdatasetjson"></a>inputdataset.JSON

    {
        "name": "AzureBlobInput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
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
| columnDelimiter | Spalten in die Protokolldateien werden durch ein Komma (,) getrennt. |
| Häufigkeit/Intervall | Legen Sie auf den Monat und das Intervall Häufigkeit ist 1, was bedeutet, dass die Eingabewerte Segmente monatlich verfügbar sind. | 
| externe | True, wenn die Eingabedaten nicht vom Dienst Daten Factory generiert werden, wird diese Eigenschaft festgelegt. | 

### <a name="outputdatasetjson"></a>outputdataset.JSON

    {
        "name": "AzureBlobOutput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
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

### <a name="pipelinejson"></a>Pipeline.JSON
> [AZURE.IMPORTANT] Ersetzen Sie **Storageaccountname** mit Namen Ihres Kontos Azure-Speicher. 


    {
        "name": "MyFirstPipeline",
        "properties": {
            "description": "My first Azure Data Factory pipeline",
            "activities": [{
                "type": "HDInsightHive",
                "typeProperties": {
                    "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                    "scriptLinkedService": "AzureStorageLinkedService",
                    "defines": {
                        "inputtable": "wasb://adfgetstarted@<stroageaccountname>.blob.core.windows.net/inputdata",
                        "partitionedtable": "wasb://adfgetstarted@<stroageaccountname>t.blob.core.windows.net/partitioneddata"
                    }
                },
                "inputs": [{
                    "name": "AzureBlobInput"
                }],
                "outputs": [{
                    "name": "AzureBlobOutput"
                }],
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
            }],
            "start": "2016-07-10T00:00:00Z",
            "end": "2016-07-11T00:00:00Z",
            "isPaused": false
        }
    }

In den JSON-Codeausschnitt erstellen Sie eine Verkaufspipeline, der eine einzelne Aktivität besteht, die Struktur zum Verarbeiten von Daten in einem Cluster HDInsight verwendet.

Die Struktur-Skriptdatei, **partitionweblogs.hql**, wird in der Azure-Speicher-Konto (angegeben durch die ScriptLinkedService, die als **StorageLinkedService**bezeichnet), und klicken Sie im Ordner " **Skript** " in den Container **Adfgetstarted**gespeichert.

Im Abschnitt **definiert** gibt Runtime-Einstellungen, die als Struktur Konfigurationswerte an das Skript Struktur übergeben werden (z. B. ${Hiveconf: inputtable}, ${Hiveconf:partitionedtable}).

Die Eigenschaften **Starten** und **Beenden** der Verkaufspipeline gibt den aktiven Zeitraum der Verkaufspipeline an.

In der Aktivität JSON Geben Sie an, dass das Skript Struktur für das Berechnen von der **LinkedServiceName** – **HDInsightOnDemandLinkedService**angegebenen ausgeführt wird.

> [AZURE.NOTE] [Aufbau einer Verkaufspipeline](data-factory-create-pipelines.md#anatomy-of-a-pipeline) finden Sie Details JSON-Eigenschaften, die im vorhergehenden Beispiel verwendet. 

## <a name="set-global-variables"></a>Festlegen globaler Variablen

Führen Sie die folgenden Befehle in Azure PowerShell Nachdem Sie die Werte durch ein eigenes ersetzen:

> [AZURE.IMPORTANT] Finden Sie unter [Voraussetzungen für](#prerequisites) Anweisungen im Abschnitt erhalten geheim Client, Client-ID-Mandanten-ID und Abonnement-ID.   

    $client_id = "<client ID of application in AAD>"
    $client_secret = "<client key of application in AAD>"
    $tenant = "<Azure tenant ID>";
    $subscription_id="<Azure subscription ID>";

    $rg = "ADFTutorialResourceGroup"
    $adf = "FirstDataFactoryREST"



## <a name="authenticate-with-aad"></a>Mit AAD authentifizieren

    $cmd = { .\curl.exe -X POST https://login.microsoftonline.com/$tenant/oauth2/token  -F grant_type=client_credentials  -F resource=https://management.core.windows.net/ -F client_id=$client_id -F client_secret=$client_secret };
    $responseToken = Invoke-Command -scriptblock $cmd;
    $accessToken = (ConvertFrom-Json $responseToken).access_token;
    
    (ConvertFrom-Json $responseToken) 



## <a name="create-data-factory"></a>Erstellen von Daten factory

In diesem Schritt erstellen Sie eine Azure Data Factory mit dem Namen **FirstDataFactoryREST**. Eine Factory Daten kann eine oder mehrere Rohrleitungen haben. Eine Verkaufspipeline kann eine oder mehrere Aktivitäten enthalten. Angenommen, eine Kopie Aktivität zum Kopieren von Daten aus einer Quelle zu einem Ziel-Datenspeicher und eine HDInsight Struktur Aktivität zu Struktur Skript Datentransformation ausführen, um. Führen Sie die folgenden Befehle zum Erstellen der Factory Daten: 

1. Weisen Sie den Befehl mit dem Namen **Cmd**Variablen aus. 

    Bestätigen, dass der Name der Factory Daten, die Sie hier angeben (ADFCopyTutorialDF) entspricht der Name in der **datafactory.json**angegeben. 

        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@datafactory.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/FirstDataFactoryREST?api-version=2015-10-01};
2. Führen Sie den Befehl **Aufrufen-Befehl**verwenden.

        $results = Invoke-Command -scriptblock $cmd;
3. Anzeigen der Ergebnisse an. Wenn die Daten Factory erfolgreich erstellt wurde, wird die JSON für die Factory Daten in die **Ergebnisse**; Andernfalls wird eine Fehlermeldung angezeigt.  

        Write-Host $results

Beachten Sie die folgenden Punkte:
 
- Der Name der Factory Daten Azure muss global eindeutig sein. Wenn Sie den Fehler in Suchergebnissen angezeigt: **Factory Data Source Name "FirstDataFactoryREST" ist nicht verfügbar**, führen Sie die folgenden Schritte aus:  
    1. Ändern Sie den Namen (beispielsweise YournameFirstDataFactoryREST) in der Datei **datafactory.json** . Finden Sie unter [Data Factory - Regeln zur Benennung von](data-factory-naming-rules.md) Thema Benennungskonventionen für Daten Factory Elemente.
    2. Ersetzen Sie in den ersten Befehl, wo die Variable **$cmd** einen Wert zugewiesen ist FirstDataFactoryREST durch den neuen Namen, und führen Sie den Befehl. 
    3. Führen Sie die folgenden beiden Befehle zum Aufrufen der REST-API, um die Daten Factory erstellen und Drucken der Resultate des Vorgangs. 
- Um Daten Factory-Instanzen erstellen zu können, müssen Sie ein Mitwirkender/Administrator des Abonnements Azure sein
- Der Name der Factory Daten möglicherweise als DNS-Namen in der Zukunft und somit werden öffentlich sichtbar registriert werden.
- Wenn Sie die Fehlermeldung: "**Abonnement ist nicht registriert, um den Namespace Microsoft.DataFactory verwenden**", führen Sie eine der folgenden Aktionen, und versuchen Sie erneut veröffentlichen: 

    - Führen Sie in Azure PowerShell zum Registrieren des Daten Factory-Anbieters den folgenden Befehl aus: 
        
            Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    
        Sie können dem folgenden Befehl aus, um zu bestätigen, dass die Daten Factory ausführen, wenn Anbieter registriert ist: 
    
            Get-AzureRmResourceProvider
    - Melden Sie sich mit dem Azure Abonnement in der [Azure-Portal](https://portal.azure.com) und navigieren Sie zu einer Blade Daten Factory (oder) erstellen Sie eine Factory Daten im Azure-Portal. Diese Aktion registriert automatisch den Anbieter für Sie.

Vor dem Erstellen einer Verkaufspipeline, müssen Sie zuerst ein paar Daten Factory-Elemente zu erstellen. Zuerst erstellen Sie ein verknüpfte Diensten zum Verknüpfen Ihrer Datenspeichers Daten Stores/berechnet, Eingabe definieren und Datasets zur Darstellung von Daten in verknüpften Datenspeicher ausgeben. 

## <a name="create-linked-services"></a>Erstellen von verknüpften Diensten 
In diesem Schritt verknüpfen Sie Ihr Konto Azure-Speicher und eine bei Bedarf Azure HDInsight Cluster mit Ihrer Daten Factory ein. Das Konto Azure-Speicher enthält die Eingabe- und Daten für die Verkaufspipeline, in diesem Beispiel. Der Dienst HDInsight verknüpft dient zum Ausführen von Struktur Skript in die Aktivität von der Verkaufspipeline in diesem Beispiel angegeben. 

### <a name="create-azure-storage-linked-service"></a>Erstellen von Azure verknüpft Speicherdienst
In diesem Schritt verknüpfen Sie Ihr Konto Azure-Speicher mit Ihrer Daten Factory an. Mit diesem Lernprogramm verwenden Sie das gleiche Speicher Azure-Konto zum Speichern/Ausgang Daten und der HQL-Skriptdatei aus.

1. Weisen Sie den Befehl mit dem Namen **Cmd**Variablen aus. 

        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@azurestoragelinkedservice.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/AzureStorageLinkedService?api-version=2015-10-01};
2. Führen Sie den Befehl **Aufrufen-Befehl**verwenden.
 
        $results = Invoke-Command -scriptblock $cmd;
3. Anzeigen der Ergebnisse an. Wenn der verknüpfte Dienst erfolgreich erstellt wurde, wird die JSON für den verknüpften Dienst in die **Ergebnisse**; Andernfalls wird eine Fehlermeldung angezeigt.
  
        Write-Host $results

### <a name="create-azure-hdinsight-linked-service"></a>Erstellen von Azure HDInsight verknüpft-Dienst
In diesem Schritt verknüpfen Sie einen bei Bedarf HDInsight Cluster mit Ihrer Daten Factory an. HDInsight Cluster wird automatisch zur Laufzeit erstellt und gelöscht, nachdem es für die angegebene Zeitspanne Verarbeitung und im Leerlauf ausgeführt werden. Sie können eigene HDInsight Cluster anstelle von einem bei Bedarf HDInsight Cluster verwenden. Details finden Sie unter [Verknüpfte Services zu berechnen](data-factory-compute-linked-services.md) .  

1. Weisen Sie den Befehl mit dem Namen **Cmd**Variablen aus.
 
        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@hdinsightondemandlinkedservice.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/hdinsightondemandlinkedservice?api-version=2015-10-01};
2. Führen Sie den Befehl **Aufrufen-Befehl**verwenden.

        $results = Invoke-Command -scriptblock $cmd;
3. Anzeigen der Ergebnisse an. Wenn der verknüpfte Dienst erfolgreich erstellt wurde, wird die JSON für den verknüpften Dienst in die **Ergebnisse**; Andernfalls wird eine Fehlermeldung angezeigt.  

        Write-Host $results

## <a name="create-datasets"></a>Datasets erstellen
In diesem Schritt erstellen Sie Datasets zum Darstellen der Eingabe und Ausgabedaten für die Verarbeitung von Struktur. Diese Datasets finden Sie in der **StorageLinkedService** , die Sie zuvor in diesem Lernprogramm erstellt haben. Die verknüpfte Dienstpunkte einer Azure-Speicher-Konto und Datasets angeben Container, Ordner, Dateiname in der Speicher, der Eingabe und Ausgabedaten.   

### <a name="create-input-dataset"></a>Erstellen von dataset
In diesem Schritt erstellen Sie das Eingabe-Dataset zum Darstellen der eingegebenen Daten in der Azure Blob-Speicher gespeichert.

1. Weisen Sie den Befehl mit dem Namen **Cmd**Variablen aus. 

        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@inputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureBlobInput?api-version=2015-10-01};
2. Führen Sie den Befehl **Aufrufen-Befehl**verwenden.

        $results = Invoke-Command -scriptblock $cmd;
3. Anzeigen der Ergebnisse an. Wenn das Dataset erfolgreich erstellt wurde, wird die JSON für das Dataset in die **Ergebnisse**; Andernfalls wird eine Fehlermeldung angezeigt.
  
        Write-Host $results
### <a name="create-output-dataset"></a>Die Ausgabe Dataset erstellen
In diesem Schritt erstellen Sie das Dataset Ausgabe zum Darstellen der Azure Blob-Speicher gehörende Kehrmatrix Ausgabedaten.

1. Weisen Sie den Befehl mit dem Namen **Cmd**Variablen aus.
 
        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@outputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureBlobOutput?api-version=2015-10-01};
2. Führen Sie den Befehl **Aufrufen-Befehl**verwenden.

        $results = Invoke-Command -scriptblock $cmd;
3. Anzeigen der Ergebnisse an. Wenn das Dataset erfolgreich erstellt wurde, wird die JSON für das Dataset in die **Ergebnisse**; Andernfalls wird eine Fehlermeldung angezeigt.
  
        Write-Host $results 

## <a name="create-pipeline"></a>Erstellen der Verkaufspipeline
In diesem Schritt erstellen Sie Ihre erste Verkaufspipeline mit einer Aktivität **HDInsightHive** aus. Eingabe Segment steht monatlich (Häufigkeit: Monat, Intervall: 1), Ausgabe Segment monatlich erstellt wird und die Scheduler-Eigenschaft für die Aktivität auch monatlich festgelegt ist. Die Einstellungen für die Ausgabe Dataset und die Aktivität Scheduler müssen übereinstimmen. Derzeit ist Ausgabe Dataset, was den Terminplan veranlasst, müssen Sie ein Dataset Ausgabe erstellen, selbst wenn die Aktivität keine Ausgabe erzeugt. Wenn die Aktivität jede Eingabe übernehmen möchten nicht, können Sie überspringen des Eingabe-Dataset zu erstellen.  

Bestätigen Sie, dass Sie die Datei **input.log** in den Ordner **Adfgetstarted/Inputdata** Azure Blob-Speicher finden Sie unter, und führen Sie den folgenden Befehl zum Bereitstellen der Verkaufspipeline. Da die ** **Starten** und** Endzeiten werden in der Vergangenheit und **IsPaused** auf falsch festgelegt ist, wird der Verkaufspipeline (Aktivität in der Verkaufspipeline) unmittelbar nach der Bereitstellung ausgeführt. 

1. Weisen Sie den Befehl mit dem Namen **Cmd**Variablen aus.
 
        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@pipeline.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datapipelines/MyFirstPipeline?api-version=2015-10-01};
2. Führen Sie den Befehl **Aufrufen-Befehl**verwenden.

        $results = Invoke-Command -scriptblock $cmd;
3. Anzeigen der Ergebnisse an. Wenn das Dataset erfolgreich erstellt wurde, wird die JSON für das Dataset in die **Ergebnisse**; Andernfalls wird eine Fehlermeldung angezeigt.  

        Write-Host $results
5. Herzlichen Glückwunsch, Sie haben Ihre erste Verkaufspipeline mithilfe der PowerShell Azure erfolgreich erstellt!

## <a name="monitor-pipeline"></a>Monitor Verkaufspipeline
In diesem Schritt verwenden Sie Data Factory REST-API zum Überwachen der Verkaufspipeline adressierte Segmente aus.

    $ds ="AzureBlobOutput"

    $cmd = {.\curl.exe -X GET -H "Authorization: Bearer $accessToken" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/$ds/slices?start=1970-01-01T00%3a00%3a00.0000000Z"&"end=2016-08-12T00%3a00%3a00.0000000Z"&"api-version=2015-10-01};

    $results2 = Invoke-Command -scriptblock $cmd;

    IF ((ConvertFrom-Json $results2).value -ne $NULL) {
        ConvertFrom-Json $results2 | Select-Object -Expand value | Format-Table
    } else {
            (convertFrom-Json $results2).RemoteException
    }


> [AZURE.IMPORTANT] 
> Erstellung einer bei Bedarf HDInsight Cluster normalerweise dauert einige Zeit (ungefähr 20 Minuten). Daher erwarten der Verkaufspipeline werden müssen, **etwa 30 Minuten** das Segment Verarbeitungszeit.  

Führen Sie die aufrufen-Befehl und dem nächsten Vorkommen, bis das Segment im **bereit** oder **fehlgeschlagen** Zustand angezeigt wird. Wenn das Segment eingeschaltet ist, überprüfen Sie den Ordner **Partitioneddata** im Container **Adfgetstarted** in Ihrem BLOB-Speicher für die Ausgabedaten.  Die Erstellung einer bei Bedarf HDInsight Cluster dauert normalerweise einige Zeit.

![Ausgabedaten](./media/data-factory-build-your-first-pipeline-using-rest-api/three-ouptut-files.png)

> [AZURE.IMPORTANT] Wenn das Segment erfolgreich verarbeitet wird, erhält die Eingabe Datei gelöscht. Daher, wenn Sie das Segment erneut ausführen, oder führen Sie das Lernprogramm erneut möchten, Hochladen Sie eingegebenen Datei (input.log) in den Ordner Inputdata des Containers Adfgetstarted.

Sie können auch Azure-Portal verwenden, um Segmente überwachen und beheben Sie alle Probleme. Anzeigen von Details [Pipelines mit Azure-Portal zu überwachen](data-factory-build-your-first-pipeline-using-editor.md##monitor-pipeline) .  

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
| [Daten Factory REST-API-Referenz](https://msdn.microsoft.com/library/azure/dn906738.aspx) |  Finden Sie unter umfassenden Dokumentation auf Daten Factory-cmdlets |
| [Daten Transformationsaktivitäten](data-factory-data-transformation-activities.md) | Dieser Artikel enthält eine Liste der Daten Transformationsaktivitäten (z. B. HDInsight Struktur Transformation, die Sie in diesem Lernprogramm verwendeten) von Azure Daten Factory unterstützt. |
| [Planung und Ausführung](data-factory-scheduling-and-execution.md) | In diesem Artikel wird erläutert, die Planung und Ausführung Aspekte des Modells für Azure Data Factory-Anwendung. |
| [Pipelines](data-factory-create-pipelines.md) | In diesem Artikel können Sie die Pipelines und Aktivitäten in Azure Data Factory und deren Verwendung zum Erstellen von End-to-End-Daten basierende Workflows für Ihre Szenario oder Ihr Unternehmen zu verstehen. |
| [Datasets](data-factory-create-datasets.md) | In diesem Artikel können Sie die Datasets in Azure Data Factory zu verstehen.
| [Überwachen und Verwalten von Pipelines mit Azure Portals blades](data-factory-monitor-manage-pipelines.md) | Dieser Artikel beschreibt, wie überwachen, verwalten und Debuggen mit Azure Portals Blades Pipelines. |
| [Überwachen und Verwalten von Pipelines mit App für die Überwachung](data-factory-monitor-manage-app.md) | Dieser Artikel beschreibt, wie überwachen, verwalten und Debuggen Pipelines die Überwachung und Verwaltung App verwenden. 


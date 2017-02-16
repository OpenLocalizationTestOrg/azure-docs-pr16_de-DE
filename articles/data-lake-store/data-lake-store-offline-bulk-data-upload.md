<properties
   pageTitle="Hochladen große Datenmengen in Lake Datenspeicher mithilfe offline Methoden | Microsoft Azure"
   description="Verwenden von AdlCopy Tool zum Kopieren von Daten aus Azure-Speicher Blobs Lake Datenspeicher"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/07/2016"
   ms.author="nitinme"/>

# <a name="use-azure-import-export-service-for-offline-copy-of-data-to-data-lake-store"></a>Verwenden von Azure Import / Export-Dienst für offline-Kopie der Daten in Lake Datenspeicher

In diesem Artikel erfahren Sie Informationen zum Kopieren von großen Datasets (> 200GB) in einen Azure Lake Datenspeicher Offlineversion Methoden, wie [Azure Import/Export-Dienst](../storage/storage-import-export-service.md)verwenden. Die Datei, die als ein Beispiel in diesem Artikel verwendeten ist insbesondere 339,420,860,416 Bytes, d. h., etwa 319GB auf dem Datenträger. Rufen Sie uns diese Datei 319GB.tsv.

Azure Import/Export-Dienst können Sie große Datenmengen auf Azure Blob-Speicher sicher übertragen werden soll, indem Sie Liefer-Festplatten auf einer Azure Data Center.

## <a name="prerequisites"></a>Erforderliche Komponenten

Vorbemerkung in diesem Artikel müssen Sie Folgendes:

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion Azure abrufen](https://azure.microsoft.com/pricing/free-trial/).

- **Speicher Azure-Konto**.

- **Azure Daten dem Analytics-Konto (optional)** – finden Sie unter [Erste Schritte mit Azure Daten dem Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md) Anweisungen zum Erstellen eines Kontos Lake Datenspeicher.


## <a name="preparing-the-data"></a>Vorbereiten der Daten

Bevor Sie mit dem Import/Export-Dienst, muss die Datendatei benutzerspezifisch übertragenen **Kopien, die weniger als 200 GB sind, in** Größe getrennt werden. Dies liegt daran mit Dateien, die größer als 200 GB nicht das Tool importieren funktioniert. Mit dieser Option entsprechen in diesem Lernprogramm wir teilen Sie die Datei in Blöcke von 100GB Bytes. Sie können dazu einfach [Cygwin](https://cygwin.com/install.html). Cygwin unterstützt Linux-Befehl die Benutzer einfach Aktion ermöglicht. Für unserem Fall verwenden wir den folgenden Befehl aus.

    split -b 100m 319GB.tsv

Diese Teilung erstellt Dateien mit den folgenden Namen ein.

    319GB.tsv-part-aa

    319GB.tsv-part-ab

    319GB.tsv-part-ac

    319GB.tsv-part-ad

## <a name="get-disks-ready-with-data"></a>Fertigstellen Datenträger mit Daten

Folgen Sie den Anweisungen am [Azure Import/Export-Dienst verwenden](../storage/storage-import-export-service.md) (klicken Sie im Abschnitt **Vorbereiten Ihren Laufwerken zu reduzieren** ) zum Vorbereiten Ihrer Festplatten. Hier sind der allgemeine Informationsfluss erfahren Sie, wie das Laufwerk vorbereiten ein.

1. Beschaffen Sie eine Festplatte, die die Anforderung für den Import/Export-Dienst Auzre verwendet werden entspricht.

2. Identifizieren ein Speicher Azure-Konto, in dem die Daten werden, einmal kopiert es Si Azure Data Center geliefert.

3. Verwenden Sie die [Azure Import/Export-Tool](http://go.microsoft.com/fwlink/?LinkID=301900&clcid=0x409), ein Programm Befehlszeile. Hier ein Beispiel Ausschnitt erfahren Sie, wie Sie das Tool verwenden.

    ````
    WAImportExport PrepImport /sk:<StorageAccountKey> /t: <TargetDriveLetter> /format /encrypt /logdir:e:\myexportimportjob\logdir /j:e:\myexportimportjob\journal1.jrn /id:myexportimportjob /srcdir:F:\demo\ExImContainer /dstdir:importcontainer/vf1/
    ````
    Weitere Stichprobe Codeausschnitte zur Verwendung des **Import/Export-Tool Azure**finden Sie unter [Azure Import/Export-Dienst verwenden](../storage/storage-import-export-service.md) .

4. Der obige Befehl erstellt eine Journaldatei an der angegebenen Position ein. Sie werden diese Journaldatei verwenden, um eine Position importieren vom [Klassischen Azure-Portal](https://manage.windowsazure.com)zu erstellen.

## <a name="create-an-import-job"></a>Erstellen eines Auftrags importieren

Sie können nun einen Importauftrag anhand der Anweisungen bei [Azure Import/Export-Dienst verwenden](../storage/storage-import-export-service.md) (klicken Sie im Abschnitt **Erstellen Sie den Importvorgang** ) erstellen. Für diesen Importvorgang mit anderen Details auch bereitstellen Sie, die Journal-Datei, die bei der Vorbereitung der Laufwerke erstellt.

## <a name="physically-ship-the-disks"></a>Liefern Sie physisch Datenträger aus

Sie können jetzt physisch zu einem Azure Data Center Datenträger verschicken, in dem die Daten über in der Azure-Speicher Blobs kopiert wird, die Ihnen beim Erstellen des Importauftrags erteilt. Auch beim Erstellen der Position, wenn Sie sich später im Verlaufsinformationen bereitstellen entschieden, Sie können jetzt kehren Sie zu Ihrem Projekt importieren und aktualisiert die laufender Nummer.

## <a name="copy-data-from-azure-storage-blobs-to-azure-data-lake-store"></a>Kopieren von Daten aus Azure-Speicher Blobs in Azure Lake Datenspeicher

Nachdem Sie der Status des Projekts importieren abgeschlossen angezeigt wird, können Sie überprüfen, ob die Daten in der Azure-Speicher Blobs verfügbar ist, die Sie angegeben haben. Eine Vielzahl von Methoden können dann die Daten von der Azure-Speicher Blobs Azure Lake Datenspeicher wechseln. Alle verfügbaren Optionen für das Hochladen von Daten finden Sie unter [Ingesting Daten in dem Datenspeicher](data-lake-store-data-scenarios.md#ingest-data-into-data-lake-store).

In diesem Abschnitt bieten wir Ihnen mit den JSON-Definitionen, die Sie zum Erstellen einer Azure Data Factory Verkaufspipeline zum Kopieren von Daten verwenden können. Sie können diese JSON-Definitionen aus dem [Azure-Portal](../data-factory/data-factory-copy-activity-tutorial-using-azure-portal.md) oder [Visual Studio](../data-factory/data-factory-copy-activity-tutorial-using-visual-studio.md) oder [Azure PowerShell](../data-factory/data-factory-copy-activity-tutorial-using-powershell.md)verwenden.

### <a name="source-linked-service-azure-storage-blob"></a>Verknüpfte Datenquelle Service (Speicher Azure Blob)

````
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "description": "",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
    }
}
````

### <a name="target-linked-service-azure-data-lake-store"></a>Ziel verknüpft Service (Azure Datenspeicher Lake)

````
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "description": "",
        "typeProperties": {
            "authorization": "<Click 'Authorize' to allow this data factory and the activities it runs to access this Data Lake Store with your access rights>",
            "dataLakeStoreUri": "https://<adls_account_name>.azuredatalakestore.net/webhdfs/v1",
            "sessionId": "<OAuth session id from the OAuth authorization session. Each session id is unique and may only be used once>"
        }
    }
}
````
### <a name="input-data-set"></a>Einer Datengruppe zurück
````
{
    "name": "InputDataSet",
    "properties": {
        "published": false,
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "importcontainer/vf1/"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
````
### <a name="output-data-set"></a>Die Ausgabe Datengruppe zurück
````
{
"name": "OutputDataSet",
"properties": {
  "published": false,
  "type": "AzureDataLakeStore",
  "linkedServiceName": "AzureDataLakeStoreLinkedService",
  "typeProperties": {
    "folderPath": "/importeddatafeb8job/"
    },
  "availability": {
    "frequency": "Hour",
    "interval": 1
    }
  }
}
````
### <a name="pipeline-copy-activity"></a>Verkaufspipeline (Kopieren Aktivität)
````
{
    "name": "CopyImportedData",
    "properties": {
        "description": "Pipeline with copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "AzureDataLakeStoreSink",
                        "copyBehavior": "PreserveHierarchy",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "InputDataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "OutputDataSet"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "AzureBlobtoDataLake",
                "description": "Copy Activity"
            }
        ],
        "start": "2016-02-08T22:00:00Z",
        "end": "2016-02-08T23:00:00Z",
        "isPaused": false,
        "pipelineMode": "Scheduled"
    }
}
````
Weitere Informationen zur Verwendung von Azure Data Factory zum Verschieben von Daten aus Azure-Speicher Blob und Azure Lake Datenspeicher finden Sie unter [Verschieben von Daten aus Azure Speicher Blob zu Azure dem Datenspeicher Azure Data Factory verwenden](../data-factory/data-factory-azure-datalake-connector.md#sample-copy-data-from-azure-blob-to-azure-data-lake-store).

## <a name="reconstruct-the-data-files-in-azure-data-lake-store"></a>Rekonstruieren Sie die Datendateien in Azure Lake Datenspeicher

Wir beginnen mit einer Datei, die wurde 319GB Größe und eingestellt wurde es in geringerer Größe-Dateien, damit sie mit dem Import/Export-Dienst Azure übertragen werden kann. Jetzt, da die Daten in Azure Lake Datenspeicher sind, können wir Reconstrcut seiner ursprünglichen Größe die Datei an. Sie können die folgenden Azure PowerShell Cmldts vergeblich verwenden.

````
# Login to our account
Login-AzureRmAccount

# List your subscriptions
Get-AzureRmSubscription

# Switch to the subscription you want to work with
Set-AzureRmContext –SubscriptionId
Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

# Join  the files
Join-AzureRmDataLakeStoreItem -AccountName "<adls_account_name" -Paths "/importeddatafeb8job/319GB.tsv-part-aa","/importeddatafeb8job/319GB.tsv-part-ab", "/importeddatafeb8job/319GB.tsv-part-ac", "/importeddatafeb8job/319GB.tsv-part-ad" -Destination "/importeddatafeb8job/MergedFile.csv”
````

## <a name="next-steps"></a>Nächste Schritte

- [Schützen von Daten in Lake Datenspeicher](data-lake-store-secure-data.md)
- [Verwenden von Azure Daten Lake Analytics mit Lake Datenspeicher](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Verwenden von Azure HDInsight mit Lake Datenspeicher](data-lake-store-hdinsight-hadoop-use-portal.md)

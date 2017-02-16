<properties 
    pageTitle="Hadoop Streaming Aktivität" 
    description="Erfahren Sie, wie Sie die Hadoop Streaming Aktivität in einer Factory Azure-Daten verwenden können, Programme Hadoop Streaming auf einem auf – Nachfrage/Ihre eigenen HDInsight Cluster ausführen." 
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/20/2016" 
    ms.author="shlo"/>

# <a name="hadoop-streaming-activity"></a>Hadoop Streaming Aktivität
> [AZURE.SELECTOR]
[Struktur](data-factory-hive-activity.md)  
[Schwein](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Hadoop Streaming](data-factory-hadoop-streaming-activity.md)
[Computer Learning](data-factory-azure-ml-batch-execution-activity.md) 
[Gespeicherte Prozedur](data-factory-stored-proc-activity.md)
[Daten dem Analytics U SQL](data-factory-usql-activity.md)
[benutzerdefinierten .NET](data-factory-use-custom-activities.md)

Sie können die Aktivität HDInsightStreamingActivity ein Auftrags Hadoop Streaming aus einer Azure Data Factory Verkaufspipeline aufzurufen. Der folgende JSON-Ausschnitt zeigt die Syntax für die Verwendung der HDInsightStreamingActivity in einer Verkaufspipeline JSON-Datei. 

Die HDInsight Streaming Aktivität in einer Daten Factory [Verkaufspipeline](data-factory-create-pipelines.md) führt Hadoop Streaming Programme für [Eigene](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) oder [bei Bedarf](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows, Linux-basierten HDInsight Cluster. In diesem Artikel wird im Artikel [Daten Transformationsaktivitäten](data-factory-data-transformation-activities.md) , die bietet eine allgemeine Übersicht über Datentransformation und der unterstützten Transformationsaktivitäten erstellt.

## <a name="json-sample"></a>JSON-Beispiel
HDInsight Cluster wird mit Beispielprogramme (wc.exe und cat.exe) und Daten (davinci.txt) automatisch ausgefüllt. Standardmäßig ist Name des Containers, die vom HDInsight Cluster verwendet wird der Name der Cluster selbst. Beispielsweise ist Ihr Clustername Myhdicluster, wäre Name des Containers Blob verknüpften Myhdicluster. 

    {
        "name": "HadoopStreamingPipeline",
        "properties": {
            "description": "Hadoop Streaming Demo",
            "activities": [
                {
                    "type": "HDInsightStreaming",
                    "typeProperties": {
                        "mapper": "cat.exe",
                        "reducer": "wc.exe",
                        "input": "wasb://<nameofthecluster>@spestore.blob.core.windows.net/example/data/gutenberg/davinci.txt",
                        "output": "wasb://<nameofthecluster>@spestore.blob.core.windows.net/example/data/StreamingOutput/wc.txt",
                        "filePaths": [
                            "<nameofthecluster>/example/apps/wc.exe",
                            "<nameofthecluster>/example/apps/cat.exe"
                        ],
                        "fileLinkedService": "StorageLinkedService",
                        "getDebugInfo": "Failure"
                    },
                    "outputs": [
                        {
                            "name": "StreamingOutputDataset"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "name": "RunHadoopStreamingJob",
                    "description": "Run a Hadoop streaming job",
                    "linkedServiceName": "HDInsightLinkedService"
                }
            ],
            "start": "2014-01-04T00:00:00Z",
            "end": "2014-01-05T00:00:00Z"
        }
    }

Beachten Sie die folgenden Punkte:

1. Legen Sie die **LinkedServiceName** auf den Namen des Diensts verknüpfte, die auf Ihre HDInsight Cluster verweist für das streaming Mapreduce Auftrags ausgeführt wird.
2. Legen Sie den Typ der Aktivität auf **HDInsightStreaming**ein.
3. Geben Sie den Namen des Mapper ausführbare für die Eigenschaft **Mapper** . Im Beispiel ist cat.exe der ausführbare Zuordnung an.
4. Geben Sie den Namen des Übergangsstücks ausführbare, für die Eigenschaft **Reduzierstücks** . Im Beispiel ist wc.exe des Reduzierstücks ausführbare.
5. Geben Sie für die **Eingabewerte** Typeigenschaft eingegebenen Datei (einschließlich der Position) für die Zuordnung ein. Im Beispiel: "wasb://adfsample@ <account name>.blob.core.windows.net/example/data/gutenberg/davinci.txt ": Adfsample ist der Blob-Container, Beispiel/Daten/Gutenberg ist der Ordner und davinci.txt ist der Blob.
6. Geben Sie für die **Ausgabe** Type-Eigenschaft die Ausgabedatei (einschließlich der Position) für die Reduzierstücks ein. Die Ausgabe des Projekts Hadoop Streaming wird in den Speicherort für diese Eigenschaft angegebene geschrieben.
7. Geben Sie im Abschnitt **FilePaths** für die Programmdateien Mapper und Reduzierstücks Pfade aus. Im Beispiel: "adfsample/example/apps/wc.exe", Adfsample ist der Blob-Container, Beispiel/apps ist der Ordner und wc.exe ist die ausführbare Datei.
8. Geben Sie für die Eigenschaft **FileLinkedService** der Azure verknüpft Speicherdienst, die den Azure-Speicher darstellt, der im Abschnitt FilePaths angegebenen Dateien enthält.
9. Geben Sie für die Eigenschaft **Argumente** die Argumente für das streaming Projekt aus.
10. Die Eigenschaft **GetDebugInfo** ist ein optionales Element. Wenn sie auf Fehler bei der festgelegt ist, werden die Protokolle nur bei Fehlern heruntergeladen. Wenn sie alle festgelegt ist, werden Protokolle immer unabhängig von der Ausführungsstatus heruntergeladen.

> [AZURE.NOTE] Wie im Beispiel gezeigt wird, geben Sie ein Dataset Ausgabe für die Hadoop Streaming Aktivität für die Eigenschaft **ausgegeben** . Dieses Dataset ist ein-platzhalterprodukt Dataset, die erforderlich ist, um den Terminplan Verkaufspipeline versorgen. Sie müssen keine Eingabe-Dataset für die Aktivität für die Eigenschaft **Eingaben** angeben.  

    
## <a name="example"></a>Beispiel
Der Verkaufspipeline in dieser Anleitung erfahren bei Ihren Cluster Azure HDInsight das Wortanzahl streaming Karte/verringern Programm ausgeführt wird. 

### <a name="linked-services"></a>Verknüpfte services

#### <a name="azure-storage-linked-service"></a>Azure verknüpft Speicherdienst
Im ersten Schritt erstellen Sie einen verknüpften Dienst, um den Azure-Speicher zu verknüpfen, die vom Cluster Azure HDInsight Fabrik Azure-Daten verwendet wird. Wenn Sie kopieren und den folgenden Code einfügen, vergessen Sie nicht Kontonamen und kontoschlüssel mit den Namen und Schlüssel Ihrer Azure-Speicher zu ersetzen. 

    {
        "name": "StorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>"
            }
        }
    }

#### <a name="azure-hdinsight-linked-service"></a>Azure HDInsight verknüpft-Dienst
Als Nächstes erstellen Sie eine verknüpfte Service um Ihren Cluster Azure HDInsight Fabrik Azure-Daten zu verknüpfen. Wenn Sie kopieren und den folgenden Code einfügen, ersetzen Sie HDInsight Clusternamen mit dem Namen der Cluster HDInsight, und ändern Sie die Werte für Benutzernamen und Ihr Kennwort ein. 
    
    {
        "name": "HDInsightLinkedService",
        "properties": {
            "type": "HDInsight",
            "typeProperties": {
                "clusterUri": "https://<HDInsight cluster name>.azurehdinsight.net",
                "userName": "admin",
                "password": "**********",
                "linkedServiceName": "StorageLinkedService"
            }
        }
    }

### <a name="datasets"></a>Datasets

#### <a name="output-dataset"></a>Die Ausgabe dataset
Der Verkaufspipeline in diesem Beispiel wird keine Eingaben verwendet. Sie haben ein Dataset Ausgabe für die HDInsight Streaming Aktivität angeben. Dieses Dataset ist ein-platzhalterprodukt Dataset, die erforderlich ist, um den Terminplan Verkaufspipeline versorgen. 

    {
        "name": "StreamingOutputDataset",
        "properties": {
            "published": false,
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "adftutorial/streamingdata/",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                },
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

### <a name="pipeline"></a>Verkaufspipeline

Der Verkaufspipeline in diesem Beispiel ist nur eine Aktivität, die vom Typ ist: **HDInsightStreaming**. 

HDInsight Cluster wird mit Beispielprogramme (wc.exe und cat.exe) und Daten (davinci.txt) automatisch ausgefüllt. Standardmäßig ist Name des Containers, die vom HDInsight Cluster verwendet wird der Name der Cluster selbst. Beispielsweise ist Ihr Clustername Myhdicluster, wäre Name des Containers Blob verknüpften Myhdicluster.  

    {
        "name": "HadoopStreamingPipeline",
        "properties": {
            "description": "Hadoop Streaming Demo",
            "activities": [
                {
                    "type": "HDInsightStreaming",
                    "typeProperties": {
                        "mapper": "cat.exe",
                        "reducer": "wc.exe",
                        "input": "wasb://<blobcontainer>@spestore.blob.core.windows.net/example/data/gutenberg/davinci.txt",
                        "output": "wasb://<blobcontainer>@spestore.blob.core.windows.net/example/data/StreamingOutput/wc.txt",
                        "filePaths": [
                            "<blobcontainer>/example/apps/wc.exe",
                            "<blobcontainer>/example/apps/cat.exe"
                        ],
                        "fileLinkedService": "StorageLinkedService"
                    },
                    "outputs": [
                        {
                            "name": "StreamingOutputDataset"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "name": "RunHadoopStreamingJob",
                    "description": "Run a Hadoop streaming job",
                    "linkedServiceName": "HDInsightLinkedService"
                }
            ],
            "start": "2014-01-04T00:00:00Z",
            "end": "2014-01-05T00:00:00Z"
        }
    }

## <a name="see-also"></a>Siehe auch
- [Struktur Aktivität](data-factory-hive-activity.md)
- [Schwein Aktivität](data-factory-pig-activity.md)
- [MapReduce Aktivität](data-factory-map-reduce.md)
- [Rufen Sie Spark-Programme](data-factory-spark.md)
- [Rufen Sie R Skripts](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)


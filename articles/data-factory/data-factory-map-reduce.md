<properties 
    pageTitle="Aufrufen eines MapReduce Programms von Factory Azure-Daten" 
    description="Erfahren Sie, wie Daten zu verarbeiten, indem MapReduce Programme aus einer Factory Azure-Daten auf einem Cluster Azure HDInsight ausgeführt." 
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
    ms.date="09/12/2016" 
    ms.author="shlo"/>

# <a name="invoke-mapreduce-programs-from-data-factory"></a>Rufen Sie MapReduce Programme von Daten Factory
> [AZURE.SELECTOR]
[Struktur](data-factory-hive-activity.md)  
[Schwein](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Hadoop Streaming](data-factory-hadoop-streaming-activity.md)
[Computer Learning](data-factory-azure-ml-batch-execution-activity.md) 
[Gespeicherte Prozedur](data-factory-stored-proc-activity.md)
[Daten dem Analytics U SQL](data-factory-usql-activity.md)
[benutzerdefinierten .NET](data-factory-use-custom-activities.md)

Die Aktivität HDInsight MapReduce in einer Daten Factory [Verkaufspipeline](data-factory-create-pipelines.md) führt MapReduce Programme für [Eigene](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) oder [bei Bedarf](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows, Linux-basierten HDInsight Cluster an. In diesem Artikel wird im Artikel [Daten Transformationsaktivitäten](data-factory-data-transformation-activities.md) , die bietet eine allgemeine Übersicht über Datentransformation und der unterstützten Transformationsaktivitäten erstellt.

## <a name="introduction"></a>Einführung 
Eine Verkaufspipeline in einer Factory Azure-Daten verarbeitet Daten in verknüpften Speicherservices unter Verwendung von verknüpften berechnen Services. Sie enthält eine Abfolge von Aktivitäten, wobei jede Aktivität eine bestimmte Verarbeitung ausführt. In diesem Artikel werden die HDInsight MapReduce Aktivität verwenden.
 
Ausführliche Informationen zum Ausführen von Schwein/Struktur Skripts auf einem Windows, Linux-basierten HDInsight Cluster aus einem Verkaufspipeline mithilfe von HDInsight Schwein und Struktur Aktivitäten finden Sie unter [Schwein](data-factory-pig-activity.md) und [Struktur](data-factory-hive-activity.md) . 

## <a name="json-for-hdinsight-mapreduce-activity"></a>JSON für HDInsight MapReduce Aktivität 

In der JSON-Definition für die Aktivität HDInsight: 
 
1. Legen Sie den **Typ** der **Aktivität** auf **HDInsight**.
3. Geben Sie den Namen der Klasse für **Objektname** Eigenschaft.
4. Geben Sie den Pfad für die JAR-Datei, einschließlich des Dateinamens für **JarFilePath** Eigenschaft an.
5. Geben Sie an der verknüpfte Dienst, der auf die Azure BLOB-Speicher verweist, die die JAR-Datei für **JarLinkedService** Eigenschaft enthält.   
6. Geben Sie im Abschnitt **Argumente** Argumente für das Programm MapReduce ein. Zur Laufzeit finden Sie einige zusätzliche Argumente (zum Beispiel: mapreduce.job.tags) aus dem MapReduce Framework. Um Ihre Argumente mit den Argumenten MapReduce unterscheiden zu können, sollten Sie sowohl Option und Wert als Argumente verwenden, wie im folgenden Beispiel dargestellt (- s,--Eingabe, – Ausgabe usw., werden sofort gefolgt von deren Werte Optionen).

        {
            "name": "MahoutMapReduceSamplePipeline",
            "properties": {
                "description": "Sample Pipeline to Run a Mahout Custom Map Reduce Jar. This job calcuates an Item Similarity Matrix to determine the similarity between 2 items",
                "activities": [
                    {
                        "type": "HDInsightMapReduce",
                        "typeProperties": {
                            "className": "org.apache.mahout.cf.taste.hadoop.similarity.item.ItemSimilarityJob",
                            "jarFilePath": "adfsamples/Mahout/jars/mahout-examples-0.9.0.2.2.7.1-34.jar",
                            "jarLinkedService": "StorageLinkedService",
                            "arguments": [
                                "-s",
                                "SIMILARITY_LOGLIKELIHOOD",
                                "--input",
                                "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/input",
                                "--output",
                                "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/output/",
                                "--maxSimilaritiesPerItem",
                                "500",
                                "--tempDir",
                                "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/temp/mahout"
                            ]
                        },
                        "inputs": [
                            {
                                "name": "MahoutInput"
                            }
                        ],
                        "outputs": [
                            {
                                "name": "MahoutOutput"
                            }
                        ],
                        "policy": {
                            "timeout": "01:00:00",
                            "concurrency": 1,
                            "retry": 3
                        },
                        "scheduler": {
                            "frequency": "Hour",
                            "interval": 1
                        },
                        "name": "MahoutActivity",
                        "description": "Custom Map Reduce to generate Mahout result",
                        "linkedServiceName": "HDInsightLinkedService"
                    }
                ],
                "start": "2014-01-03T00:00:00Z",
                "end": "2014-01-04T00:00:00Z",
                "isPaused": false,
                "hubName": "mrfactory_hub",
                "pipelineMode": "Scheduled"
            }
        }
    
    

Die HDInsight MapReduce Aktivität können Sie alle MapReduce JAR-Datei in einem Cluster HDInsight ausführen. In der folgenden Beispiel JSON Definition einer Verkaufspipeline sieht die Aktivität HDInsight Mahout JAR-Datei ausführen.

## <a name="sample-on-github"></a>Beispiel für GitHub
Sie können ein Beispiel für die Verwendung der HDInsight MapReduce Aktivitäten herunterladen: [Daten Factory Beispiele auf GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSON/MapReduce_Activity_Sample).  

## <a name="running-the-word-count-program"></a>Ausführen des Programms Wörter zählen
Der Verkaufspipeline in diesem Beispiel wird das Programm Wörter zählen Karte/verringern auf Ihren Cluster Azure HDInsight ausgeführt.   

### <a name="linked-services"></a>Verknüpfte Services
Im ersten Schritt erstellen Sie einen verknüpften Dienst, um den Azure-Speicher zu verknüpfen, die vom Cluster Azure HDInsight Fabrik Azure-Daten verwendet wird. Wenn Sie kopieren und den folgenden Code einfügen, vergessen Sie nicht **Kontonamen** und **kontoschlüssel** mit den Namen und Schlüssel Ihrer Azure-Speicher zu ersetzen. 

#### <a name="azure-storage-linked-service"></a>Azure verknüpft Speicherdienst

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
Als Nächstes erstellen Sie eine verknüpfte Service um Ihren Cluster Azure HDInsight Fabrik Azure-Daten zu verknüpfen. Wenn Sie kopieren und den folgenden Code einfügen, ersetzen Sie **HDInsight Clusternamen** mit dem Namen der Cluster HDInsight, und ändern Sie die Werte für Benutzernamen und Ihr Kennwort ein.   

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
Der Verkaufspipeline in diesem Beispiel wird keine Eingaben verwendet. Sie haben ein Dataset Ausgabe für die HDInsight MapReduce Aktivität angeben. Dieses Dataset ist ein-platzhalterprodukt Dataset, die erforderlich ist, um den Terminplan Verkaufspipeline versorgen.  

    {
        "name": "MROutput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "fileName": "WordCountOutput1.txt",
                "folderPath": "example/data/",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

### <a name="pipeline"></a>Verkaufspipeline
Der Verkaufspipeline in diesem Beispiel ist nur eine Aktivität, die vom Typ ist: HDInsightMapReduce. Einige der wichtigen Eigenschaften in den JSON sind: 

Eigenschaft | Notizen
:-------- | :-----
Typ | **HDInsightMapReduce**muss der Typ festgelegt werden. 
Objektname | Name der Klasse ist: **Wordcount**
jarFilePath | Pfad für die JAR-Datei, die das enthält. Wenn Sie kopieren und den folgenden Code einfügen, vergessen Sie nicht, den Namen der Cluster ändern. 
jarLinkedService | Azure-Speicher verknüpft Dienst, der die JAR-Datei enthält. Dieser verknüpfte Dienst bezieht sich auf die Speicherung, die dem Cluster HDInsight zugeordnet ist. 
Argumente auf: | Das Programm Wordcount akzeptiert zwei Argumente: eine Eingabe und eine Ausgabe. Die Datei ist die davinci.txt-Datei.
Häufigkeit/Intervall | Das Ausgabe Dataset mit den Werten für diese Eigenschaften übereinstimmen. 
linkedServiceName | bezieht sich auf den Dienst HDInsight verknüpft, die Sie zuvor erstellt haben.   

    {
        "name": "MRSamplePipeline",
        "properties": {
            "description": "Sample Pipeline to Run the Word Count Program",
            "activities": [
                {
                    "type": "HDInsightMapReduce",
                    "typeProperties": {
                        "className": "wordcount",
                        "jarFilePath": "<HDInsight cluster name>/example/jars/hadoop-examples.jar",
                        "jarLinkedService": "StorageLinkedService",
                        "arguments": [
                            "/example/data/gutenberg/davinci.txt",
                            "/example/data/WordCountOutput1"
                        ]
                    },
                    "outputs": [
                        {
                            "name": "MROutput"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1,
                        "retry": 3
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "name": "MRActivity",
                    "linkedServiceName": "HDInsightLinkedService"
                }
            ],
            "start": "2014-01-03T00:00:00Z",
            "end": "2014-01-04T00:00:00Z"
        }
    }

## <a name="run-spark-programs"></a>Spark Programme ausführen
MapReduce Aktivitäten können Sie Spark auf Ihren Cluster HDInsight Spark Programme ausgeführt werden. Details finden Sie unter [aufrufen Spark Programme von Azure Data Factory](data-factory-spark.md) .  

[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456


[adfgetstarted]: data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[adfgetstartedmonitoring]:data-factory-copy-data-from-azure-blob-storage-to-sql-database.md#monitor-pipelines 

[Developer Reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[Azure Portal]: http://portal.azure.com
 
## <a name="see-also"></a>Siehe auch
- [Struktur Aktivität](data-factory-hive-activity.md)
- [Schwein Aktivität](data-factory-pig-activity.md)
- [Hadoop Streaming Aktivität](data-factory-hadoop-streaming-activity.md)
- [Rufen Sie Spark-Programme](data-factory-spark.md)
- [Rufen Sie R Skripts](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)

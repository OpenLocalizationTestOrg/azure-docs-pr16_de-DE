<properties 
    pageTitle="Rufen Sie Spark Programme von Azure Data Factory" 
    description="Informationen Sie zum Aufrufen Spark Programme von einer Azure Daten Factory mithilfe der MapReduce Aktivität." 
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
    ms.date="08/25/2016" 
    ms.author="spelluru"/>

# <a name="invoke-spark-programs-from-data-factory"></a>Rufen Sie Spark Programme von Daten Factory
## <a name="introduction"></a>Einführung
Die Aktivität MapReduce können in einer Daten Factory Verkaufspipeline Sie Spark auf Ihren Cluster HDInsight Spark Programme ausgeführt werden. Finden Sie im Artikel [MapReduce Aktivität](data-factory-map-reduce.md) detaillierte Informationen zur Verwendung von der Aktivität vor dem in diesem Artikel lesen. 

## <a name="spark-sample-on-github"></a>Spark Stichprobe auf GitHub
Die [Spark - Daten Factory Stichprobe auf GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/Spark) wird gezeigt, wie MapReduce Aktivität zu verwenden, um ein Programm Spark aufzurufen. Das Programm Spark kopiert nur Daten aus einem Azure Blob-Container in einen anderen ein. 

## <a name="data-factory-entities"></a>Daten Factory Einheiten
Der Ordner **Spark-ADF/Src/ADFJsons** enthält Dateien für Daten Factory Personen (verknüpften Diensten, Datasets Verkaufspipeline).  

Es gibt zwei **verknüpften Diensten** in diesem Beispiel: Azure-Speicher und Azure HDInsight. Geben Sie Ihre Azure-Speicher Namen und Schlüsselwerte in **StorageLinkedService.json** und ClusterUri, Benutzername und Kennwort in **HDInsightLinkedService.json**an.

Es gibt zwei **Datasets** in diesem Beispiel: **input.json** und **output.json**. Diese Dateien befinden sich in den Ordner **Datasets** .  Diese Dateien darstellen Eingabe- und Datasets für die Aktivität MapReduce

Sie finden Stichprobe Rohrleitungen im Ordner **ADFJsons/Verkaufspipeline** . Überprüfen Sie eine Verkaufspipeline, um zu verstehen, wie ein Programm Spark aufrufen, indem Sie die Aktivität MapReduce verwenden. 

Rufen Sie **com.adf.sparklauncher.jar** im Container **Adflibs** in Ihrer Azure-Speicher (in der StorageLinkedService.json angegeben) ist die Aktivität MapReduce konfiguriert. Der Quellcode für dieses Programm ist in Spark-ADF/Src/primär/Java/com/Adf/Ordner und ruft Spark übermitteln und Spark Aufträge ausführen. 

> [AZURE.IMPORTANT] 
> Lesen Sie [Infos. TXT](https://github.com/Azure/Azure-DataFactory/blob/master/Samples/Spark/README.txt) für die neuesten und zusätzliche Informationen vor der Verwendung der Stichprobe. 
>  
> Verwenden Sie Ihre eigenen HDInsight Spark Cluster bei diesem Ansatz, um Spark Programme mithilfe der Aktivität MapReduce aufzurufen. Verwenden eines bei Bedarf HDInsight Clusters wird nicht unterstützt.   


## <a name="see-also"></a>Siehe auch
- [Struktur Aktivität](data-factory-hive-activity.md)
- [Schwein Aktivität](data-factory-pig-activity.md)
- [MapReduce Aktivität](data-factory-map-reduce.md)
- [Hadoop Streaming Aktivität](data-factory-hadoop-streaming-activity.md)
- [Rufen Sie R Skripts](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)

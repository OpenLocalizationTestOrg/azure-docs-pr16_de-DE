Azure Data Factory unterstützt die folgenden Transformationsaktivitäten, die können hinzugefügt Pipelines entweder einzeln oder verkettet mit einer anderen Aktivität.

Daten Transformation Aktivität |  Berechnen der Umgebung 
:----------------------- | :--------------------
[Struktur](../articles/data-factory/data-factory-hive-activity.md) | HDInsight [Hadoop] 
[Schwein](../articles/data-factory/data-factory-pig-activity.md) | HDInsight [Hadoop]  
[MapReduce](../articles/data-factory/data-factory-map-reduce.md) | HDInsight [Hadoop]  
[Hadoop Streaming](../articles/data-factory/data-factory-hadoop-streaming-activity.md) | HDInsight [Hadoop]
[Maschinelle Learning Aktivitäten: Stapel Ausführung und Aktualisieren von Ressourcen](../articles/data-factory/data-factory-azure-ml-batch-execution-activity.md) | Azure-virtuellen Computer 
[Gespeicherte Prozedur](../articles/data-factory/data-factory-stored-proc-activity.md) | SQL Azure, SQL Azure Datawarehouse oder SQLServer |
[Daten Lake Analytics U-SQL](../articles/data-factory/data-factory-usql-activity.md) | Azure-Daten Lake Analytics 
[DotNet](../articles/data-factory/data-factory-use-custom-activities.md) | HDInsight [Hadoop] oder Azure Stapel
   
> [AZURE.NOTE] 
> MapReduce Aktivitäten können Sie Spark auf Ihren Cluster HDInsight Spark Programme ausgeführt werden. Details finden Sie unter [aufrufen Spark Programme von Azure Data Factory](../articles/data-factory/data-factory-spark.md) .
> Sie können eine benutzerdefinierte Aktivität zum Ausführen von R Skripts auf Ihren Cluster HDInsight mit R installiert erstellen. Finden Sie unter [Ausführen R Skript mithilfe von Azure Data Factory](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample).
<properties 
    pageTitle="Datentransformation: Prozess und transformieren Daten | Microsoft Azure" 
    description="Erfahren Sie, wie Daten oder Verarbeiten von Daten in Azure Data Factory von Hadoop, Computer Learning oder Azure Daten dem Analytics transformieren." 
    keywords="Datentransformation, Daten zu verarbeiten, transformieren Daten Transformation Aktivität"
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
    ms.date="09/23/2016" 
    ms.author="shlo"/>

# <a name="transform-data-in-azure-data-factory"></a>Daten in Azure Data Factory transformieren
> [AZURE.SELECTOR]
[Struktur](data-factory-hive-activity.md)  
[Schwein](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Hadoop Streaming](data-factory-hadoop-streaming-activity.md)
[Computer Learning](data-factory-azure-ml-batch-execution-activity.md) 
[Gespeicherte Prozedur](data-factory-stored-proc-activity.md)
[Daten dem Analytics U SQL](data-factory-usql-activity.md)
[benutzerdefinierten .NET](data-factory-use-custom-activities.md)
   

## <a name="overview"></a>(Übersicht) 
In diesem Artikel wird erläutert, Daten Transformationsaktivitäten in Azure Data Factory, die Sie verwenden können, um transformieren und die unformatierten Daten in Vorhersagen und Einblicken verarbeitet werden. In einer Umgebung z. B. Azure HDInsight Cluster oder einem Stapel Azure führt eine Transformation Aktivität aus. Es enthält Links zu Artikeln, mit ausführliche Informationen zu jeder Transformation Aktivität.
 
Daten Factory unterstützt folgende Daten Transformationsaktivitäten, die können hinzugefügt [Pipelines](data-factory-create-pipelines.md) entweder einzeln oder verkettet mit einer anderen Aktivität.

> [AZURE.NOTE] Eine exemplarische Vorgehensweise mit schrittweise Anweisungen finden Sie im Artikel [Erstellen eines Verkaufspipeline mit Struktur Transformation](data-factory-build-your-first-pipeline.md) .  

## <a name="hdinsight-hive-activity"></a>HDInsight Struktur Aktivität
Die Struktur HDInsight Aktivität in einer Daten Factory Verkaufspipeline führt Struktur Abfragen auf Ihrer eigenen oder bei Bedarf Windows, Linux-basierten HDInsight Cluster aus. [Struktur Aktivität](data-factory-hive-activity.md) finden Sie im Artikel Details dieser Aktivität. 

## <a name="hdinsight-pig-activity"></a>HDInsight Schwein Aktivität
Die Aktivität HDInsight Schwein in einer Daten Factory Verkaufspipeline führt Schwein Abfragen auf Ihrer eigenen oder bei Bedarf Windows, Linux-basierten HDInsight Cluster aus. [Schwein Aktivität](data-factory-pig-activity.md) finden Sie im Artikel Details dieser Aktivität. 

## <a name="hdinsight-mapreduce-activity"></a>HDInsight MapReduce Aktivität
Die Aktivität HDInsight MapReduce in einer Daten Factory Verkaufspipeline führt MapReduce Programme auf Ihrer eigenen oder bei Bedarf Windows, Linux-basierten HDInsight Cluster aus. Finden Sie im Artikel [MapReduce Aktivität](data-factory-map-reduce.md) Details dieser Aktivität.

MapReduce Aktivitäten können Sie Spark auf Ihren Cluster HDInsight Spark Programme ausgeführt werden. Details finden Sie unter [aufrufen Spark Programme von Azure Data Factory](data-factory-spark.md) .

## <a name="hdinsight-streaming-activity"></a>HDInsight Streaming Aktivität
Die HDInsight Streaming Aktivität in einer Daten Factory Verkaufspipeline führt Hadoop Streaming-Programme auf Ihrer eigenen oder bei Bedarf Windows, Linux-basierten HDInsight Cluster. Details zu diesen Aktivitäten finden Sie unter [HDInsight Streaming Aktivität](data-factory-hadoop-streaming-activity.md) .

## <a name="machine-learning-activities"></a>Maschinelle Learning Aktivitäten
Azure Data Factory ermöglicht es Ihnen leicht Pipelines erstellen, die einen veröffentlichten Azure maschinellen Learning Webdienst für Vorhersageanalytik verwenden. Die [Ausführung Aktivität](data-factory-azure-ml-batch-execution-activity.md#invoking-a-web-service-using-batch-execution-activity) in einer Azure Data Factory Verkaufspipeline verwenden, können Sie einen Webdienst maschinellen Schulung, um die Daten in den Stapel Vorhersagen anzeigen aufrufen.

Im Laufe der Zeit müssen die Vorhersage Modelle in den Computer Learning bewerten Versuche einarbeiten neue Eingabewerte Datasets verwenden. Wenn Sie fertig sind, mit Umschulung, den Punktzahl Webdienst mit dem retrained Computer Learning Modell aktualisieren möchten. Die [Ressource Aktivität aktualisieren](data-factory-azure-ml-batch-execution-activity.md#updating-models-using-update-resource-activity) können Sie um den Webdienst mit dem neu ausgebildeten Modell zu aktualisieren.  

Details zu diesen Computer Learning Aktivitäten finden Sie unter [verwenden maschinelle Learning Aktivitäten](data-factory-azure-ml-batch-execution-activity.md) . 

## <a name="stored-procedure-activity"></a>Gespeicherte Prozedur Aktivität
Können die Aktivität SQL Server gespeicherte Prozedur in einer Daten Factory Verkaufspipeline zum Aufrufen einer gespeicherten Prozedur in einem der folgenden Datenspeicher: Azure SQL-Datenbank Azure SQL-Data Warehouse, SQL Server-Datenbank in Ihrem Unternehmen oder eine Azure-virtuellen Computer. [Gespeicherte Prozedur für die Aktivität](data-factory-stored-proc-activity.md) finden Sie im Artikel Details.  

## <a name="data-lake-analytics-u-sql-activity"></a>Daten dem Analytics U-SQL-Aktivität
Daten dem Analytics U-SQL-Aktivität führt ein U-SQL-Skript in einem Cluster Azure Daten dem Analytics. [Daten Analytics U-SQL-Aktivität](data-factory-usql-activity.md) finden Sie im Artikel Details. 

## <a name="net-custom-activity"></a>.NET benutzerdefinierter Aktivität
Wenn Sie benötigen für die Datentransformation auf eine Weise, die nicht von Daten Factory unterstützt wird, können Sie mit Ihrer eigenen Logik Datenverarbeitung eine benutzerdefinierte Aktivität erstellen und verwenden die Aktivität in der Verkaufspipeline. Sie können die benutzerdefinierte .NET Aktivität ausführen mithilfe eines Stapel Azure-Diensts oder eine Azure HDInsight Cluster konfigurieren. [Verwenden Sie benutzerdefinierte Aktivitäten](data-factory-use-custom-activities.md) finden Sie im Artikel Details. 

Sie können eine benutzerdefinierte Aktivität zum Ausführen von R Skripts auf Ihren Cluster HDInsight mit R installiert erstellen. Finden Sie unter [Ausführen R Skript mithilfe von Azure Data Factory](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample). 

## <a name="compute-environments"></a>Berechnen von Umgebungen
Erstellen einen verknüpften Dienst für die Umgebung berechnen und verwenden Sie dann den verknüpften Dienst beim Definieren einer Transformation Aktivität. Es gibt zwei Arten von berechnen Umgebungen von Daten Factory unterstützt. 

1. **Bei Bedarf**: In diesem Fall wird die Umgebung vollständig verwaltet von Daten Factory. Es wird automatisch vom Dienst Daten Factory erstellt werden, bevor ein Auftrags zum Verarbeiten von Daten vorgeschlagen und entfernt, wenn Sie der Auftrag abgeschlossen ist. Sie können konfigurieren und steuern präzise Einstellungen der Umgebung für die Ausführung Auftrags, Cluster Management und Aktionen Neustart bei Bedarf berechnen. 
2. **Zeigen Sie Ihre eigenen**: Sie können In diesem Fall Ihrer eigenen IT-Umgebung (beispielsweise HDInsight Cluster) als verknüpfte Dienst in Daten Factory registrieren. Ist die computing-Umgebung, die von Ihnen verwaltete und der Daten Factory-Dienst wird es verwendet, um die Aktivitäten ausführen. 

[Berechnen von verknüpften Diensten](data-factory-compute-linked-services.md) finden Sie im Artikel lernen berechnen von Daten Factory unterstützten Dienste. 


## <a name="summary"></a>Zusammenfassung
Azure Data Factory unterstützt die folgenden Daten Transformationsaktivitäten und berechnen-Umgebungen für die Aktivitäten. Die Transformationsaktivitäten können hinzugefügt Pipelines entweder einzeln oder mit einer anderen Aktivität verkettet werden.

Daten Transformation Aktivität |  Berechnen der Umgebung 
:----------------------- | :--------------------
[Struktur](data-factory-hive-activity.md) | HDInsight [Hadoop] 
[Schwein](data-factory-pig-activity.md) | HDInsight [Hadoop]  
[MapReduce](data-factory-map-reduce.md) | HDInsight [Hadoop]  
[Hadoop Streaming](data-factory-hadoop-streaming-activity.md) | HDInsight [Hadoop]
[Maschinelle Learning Aktivitäten: Stapel Ausführung und Aktualisieren von Ressourcen](data-factory-azure-ml-batch-execution-activity.md) | Azure-virtuellen Computer 
[Gespeicherte Prozedur](data-factory-stored-proc-activity.md) | SQL Azure, SQL Azure Datawarehouse oder SQLServer |
[Daten Lake Analytics U-SQL](data-factory-usql-activity.md) | Azure-Daten Lake Analytics 
[DotNet](data-factory-use-custom-activities.md) | HDInsight [Hadoop] oder Azure Stapel
   


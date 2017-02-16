<properties
    pageTitle="Debuggen und Analysieren von Hadoop-Diensten mit Heapdumps | Microsoft Azure"
    description="Automatisch sammeln Sie Heapdumps für Hadoop-Dienste, und setzen Sie in dem Azure BLOB-Speicherkonto für das Debuggen und Analyse."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="jgao"/>


# <a name="collect-heap-dumps-in-blob-storage-to-debug-and-analyze-hadoop-services"></a>Sammeln Heap bildet im BLOB-Speicher Debuggen und Analysieren Hadoop Services ab.

[AZURE.INCLUDE [heapdump-selector](../../includes/hdinsight-selector-heap-dump.md)]

Heapdumps enthalten eine Momentaufnahme der Anwendung Speicher, einschließlich der Werte von Variablen zum Zeitpunkt das Abbild erstellt wurde. Damit sie sehr nützlich zum Diagnostizieren von Problemen, die bei der Ausführung auftreten sind. Heapdumps automatisch für Hadoop Dienste gesammelt und platziert innerhalb des Kontos Azure BLOB-Speicher ein Benutzer unter HDInsightHeapDumps /. 

Die Sammlung von Heapdumps für verschiedene Dienste muss für Dienste auf einem einzelnen Cluster aktiviert sein. Das Standardformat für dieses Feature ist für einen Cluster deaktiviert werden. Diese Heapdumps können groß sein, damit es ist ratsam, überwachen das Blob-Speicher-Konto, werden diese gespeichert, nachdem die Sammlung aktiviert wurde.

> [AZURE.NOTE] Die Informationen in diesem Artikel gilt nur für Windows-basierten HDInsight. Informationen auf Linux-basierten HDInsight finden Sie unter [Aktivieren Heapdumps für Hadoop Dienste auf Linux-basierten HDInsight](hdinsight-hadoop-collect-debug-heap-dump-linux.md)

## <a name="eligible-services-for-heap-dumps"></a>Dienste für Heapdumps

Sie können für die folgenden Dienste Heapdumps aktivieren:

*  **Hcatalog** - tempelton
*  **Struktur** - hiveserver2, Metastore, derbyserver
*  **Mapreduce** - jobhistoryserver
*  **aus** - Ressourcen-Manager, Nodemanager, timelineserver
*  **Hdfs** - Datanode, Secondarynamenode, namenode

## <a name="configuration-elements-that-enable-heap-dumps"></a>Konfigurationselemente, die Heapdumps aktivieren

Um Heapdumps für einen Dienst zu aktivieren, müssen Sie die entsprechenden Konfigurationselemente legen Sie im Abschnitt für diesen Dienst, die durch **Service_name**angegeben wird.

    "javaargs.<service_name>.XX:+HeapDumpOnOutOfMemoryError" = "-XX:+HeapDumpOnOutOfMemoryError",
    "javaargs.<service_name>.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\Dumps\<service_name>_%date:~4,2%%date:~7,2%%date:~10,2%%time:~0,2%%time:~3,2%%time:~6,2%.hprof"

Der Wert von **Service_name** kann die oben aufgeführten Dienste: Tempelton, hiveserver2, Metastore, Derbyserver, Jobhistoryserver, Ressourcen-Manager, Nodemanager, Timelineserver, Datanode, Secondarynamenode, oder Namenode.

## <a name="enable-using-azure-powershell"></a>Aktivieren mithilfe der PowerShell Azure

Zum Aktivieren Heapdumps mithilfe der PowerShell Azure für Jobhistoryserver möchten Sie beispielsweise die folgendermaßen vorgehen:

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    $MapRedConfigValues = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightMapReduceConfiguration'

    $MapRedConfigValues.Configuration = @{ "javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError"="-XX:+HeapDumpOnOutOfMemoryError" ; "javaargs.jobhistoryserver.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof" }

## <a name="enable-using-net-sdk"></a>Verwenden von .NET SDK aktivieren

Zum Aktivieren Heapdumps mithilfe von Azure HDInsight .NET SDK für Jobhistoryserver möchten Sie beispielsweise die folgendermaßen vorgehen:

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError", "-XX:+HeapDumpOnOutOfMemoryError"));

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:HeapDumpPath", "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof"));

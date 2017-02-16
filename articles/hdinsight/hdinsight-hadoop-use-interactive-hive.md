<properties
    pageTitle="Verwenden der interaktiven Struktur in HDInsight | Microsoft Azure"
    description="Erfahren Sie, wie mit interaktiven Struktur (Struktur auf LLAP) in HDInsight."
    keywords=""
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
    ms.date="10/27/2016"
    ms.author="jgao"/>


# <a name="use-interactive-hive-in-hdinsight-preview"></a>Verwenden der interaktiven Struktur in HDInsight (Preview)

Interaktive (auch bekannt als Struktur [Live lange und Prozess]( https://cwiki.apache.org/confluence/display/Hive/LLAP)) ist eine neue HDInsight [Clustertyp]( hdinsight-hadoop-provision-linux-clusters.md#cluster-types).  Interaktive Struktur kann im Arbeitsspeicher zwischenspeichern, das Struktur Abfragen viel mehr interaktive oder schnellerer herstellt. Dieses neue Feature ermöglicht HDInsight eine der weltweit die meisten leistungsfähige, flexible, und öffnen Big Data-Lösung in der Cloud mit im Speicher zwischengespeichert (mit Struktur und Spark) und erweiterte Analytics durch eine Tiefe Integration Services R. 

Interaktive Struktur Cluster unterscheidet sich von der Hadoop Cluster. Sie enthält nur den Struktur-Dienst. 

> [AZURE.NOTE] MapReduce, Schwein, Sqoop, Oozie und andere Dienste werden von diesem Clustertyp bald entfernt.
Der Dienst Struktur im Cluster interaktive Struktur zugegriffen werden nur über die Ambari Struktur anzeigen, Beeline und Struktur ODBC. Es kann nicht über die Struktur Console, Templeton, Azure CLI und Azure PowerShell zugegriffen werden. 


 


## <a name="create-an-interactive-hive-cluster"></a>Erstellen eines interaktiven Struktur Clusters

Interaktive Struktur Cluster wird nur auf Linux-basierten Cluster unterstützt. Informationen zum Erstellen von HDInsight Cluster finden Sie unter [Erstellen von Linux-basierten Hadoop Cluster in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).


## <a name="execute-hive-from-interactive-hive"></a>Führen Sie die Struktur von interaktiven Struktur

Es gibt verschiedene Optionen wie Struktur Abfragen ausgeführt werden kann:

- Führen Sie in der Ansicht Ambari Struktur Struktur

    Informationen zur Verwendung der Ansicht Struktur finden Sie unter [Verwenden der Struktur Ansicht mit Hadoop in HDInsight]( hdinsight-hadoop-use-hive-ambari-view.md).

- Führen Sie die Struktur mit Beeline

    Informationen zum Verwenden von Beeline auf HDInsight finden Sie unter [Verwendung mit Hadoop in HDInsight mit Beeline Struktur](hdinsight-hadoop-use-hive-beeline.md).

    Verwenden Sie Beeline aus der Headnode oder einer leeren Kantenknoten.  Verwenden von Beeline aus einem leeren Kantenknoten wird empfohlen.  Informationen zum Erstellen eines HDInsight Clusters mit einer leeren Edgenode finden Sie unter [Verwenden von leeren Kante Knoten in HDInsight](hdinsight-apps-use-edge-node.md).

- Führen Sie mit der Struktur ODBC-Struktur

    Informationen zur Verwendung von ODBC Struktur finden Sie unter [Verbinden von Excel mit Hadoop mit dem Microsoft-Struktur ODBC-Treiber](hdinsight-connect-excel-hive-odbc-driver.md).

**So suchen Sie die Verbindungszeichenfolge JDBC**

1.  Melden Sie sich auf Ambari mithilfe der folgenden URL: https://<ClusterName>. AzureHDInsight.net.
2.  Klicken Sie im Menü links auf **Struktur** .
3.  Klicken Sie auf das hervorgehobene Symbol zum Kopieren der URL:

    ![HDInsight Hadoop interaktive Struktur LLAP JDBC](./media/hdinsight-hadoop-use-interactive-hive/hdinsight-hadoop-use-interactive-hive-jdbc.png)

## <a name="see-also"></a>Siehe auch
-   [Erstellen von Linux-basierten Hadoop Cluster in HDInsight](hdinsight-hadoop-provision-linux-clusters.md): erfahren Sie, wie interaktive Struktur Cluster in HDInsight zu erstellen.
-   [Verwenden Sie mit Hadoop in HDInsight mit Beeline Struktur](hdinsight-hadoop-use-hive-beeline.md): erfahren Sie, wie zum Beeline Struktur Abfragen senden.
-   [Verbinden von Excel mit Hadoop mit dem Microsoft-Struktur ODBC-Treiber](hdinsight-connect-excel-hive-odbc-driver.md): erfahren Sie, wie Excel mit Struktur zu verbinden.

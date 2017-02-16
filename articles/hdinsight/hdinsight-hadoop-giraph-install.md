<properties
    pageTitle="Installieren und Verwenden von Giraph auf Hadoop Cluster in HDInsight | Microsoft Azure"
    description="Erfahren Sie, wie HDInsight Cluster mit Giraph anpassen, und wie Sie Giraph verwenden."
    services="hdinsight"
    documentationCenter=""
    authors="nitinme"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/05/2016"
    ms.author="nitinme"/>

# <a name="install-and-use-giraph-in-hdinsight"></a>Installieren und Verwenden von Giraph in HDInsight


Erfahren Sie, wie Windows basierend HDInsight Cluster mit Giraph mithilfe der Aktion Skript anpassen, und wie Sie Giraph verwenden, um umfangreiche Diagramme zu verarbeiten. Informationen zum Verwenden von Giraph mit einem Linux-basierten Cluster finden Sie unter [Installieren von Giraph auf HDInsight Hadoop Cluster (Linux)](hdinsight-hadoop-giraph-install-linux.md).
 
Sie können mithilfe der *Aktion Skript*Giraph auf einem beliebigen Cluster (Hadoop Storm, HBase, Spark) auf Azure HDInsight installieren. Ein Beispiel-Skript zum Installieren von Giraph in einem HDInsight Cluster steht aus einer schreibgeschützten Azure-Speicher Blob am [https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1). Das Beispielskript funktioniert nur mit HDInsight Clusterversion 3.1. Weitere Informationen zum HDInsight Cluster Versionen finden Sie unter [HDInsight Cluster Versionen](hdinsight-component-versioning.md).

**Verwandte Artikel**

- [Installieren von Giraph auf HDInsight Hadoop Cluster (Linux)](hdinsight-hadoop-giraph-install-linux.md)
- [Hadoop erstellen Cluster in HDInsight](hdinsight-provision-clusters.md): Allgemeine Informationen zum Erstellen von HDInsight Cluster.
- [Anpassen von HDInsight Cluster mithilfe der Aktion Skript][hdinsight-cluster-customize]: Allgemeine Informationen zum Anpassen von HDInsight Cluster mithilfe der Aktion Skript.
- [Skript für Aktion entwickeln Skripts für HDInsight](hdinsight-hadoop-script-actions.md).

## <a name="what-is-giraph"></a>Was ist Giraph?

<a href="http://giraph.apache.org/" target="_blank">Apache Giraph</a> können Sie mithilfe von Hadoop Verarbeitung Graph ausführen, und kann mit Azure HDInsight verwendet werden. Diagramme modellieren Beziehungen zwischen Objekten, wie die Verbindungen zwischen Routern in einem großen Netzwerk wie das Internet oder Beziehungen zwischen Personen in sozialen Netzwerken (auch als sozialen Diagramm bezeichnet). Diagramm-Verarbeitung ermöglicht es Ihnen, Grund über die Beziehungen zwischen Objekten in einem Diagramm, z. B.:

- Identifizieren potenzielle Freunde basierend auf Ihrer aktuellen Beziehungen.
- Identifizieren den kürzesten Weg zwischen zwei Computern in einem Netzwerk.
- Den Seite Rang Webseiten zu berechnen.


## <a name="install-giraph-using-portal"></a>Installieren von Giraph mithilfe von portal

1. Starten Sie, einen Cluster mithilfe der Option **Benutzerdefinierte erstellen** erstellen, wie bei [Erstellen Hadoop Cluster in HDInsight](hdinsight-provision-clusters.md#portal)beschrieben.
2. Klicken Sie auf der Seite des Assistenten **Skript-Aktionen** auf **Skriptaktion hinzufügen** , um die Details zu der Skriptaktion angeben, wie unten dargestellt:

    ![Verwenden Sie Skript für Aktion in einem Cluster anpassen] (./media/hdinsight-hadoop-giraph-install/hdi-script-action-giraph.png "Verwenden Sie Skript für Aktion in einem Cluster anpassen")

    <table border='1'>
        <tr><th>Eigenschaft</th><th>Wert</th></tr>
        <tr><td>Namen</td>
            <td>Geben Sie einen Namen für die Skriptaktion ein. Beispielsweise <b>Giraph installieren</b>.</td></tr>
        <tr><td>URI-Skript</td>
            <td>Geben Sie an der URI Uniform Resource Identifier () an das Skript, das zum Anpassen des Clusters aufgerufen wird. Beispielsweise <i>https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1</i></td></tr>
        <tr><td>Knotentyp</td>
            <td>Geben Sie die Knoten, auf denen das Skript Anpassung ausgeführt wird. Sie können <b>alle Knoten</b>, <b>nur Kopf Knoten</b>oder <b>nur Worker-Knoten</b>auswählen.
        <tr><td>Parameter</td>
            <td>Geben Sie den Parameter aus, wenn das Skript erforderlich. Das Skript Giraph installieren sind von Parametern verwenden, nicht erforderlich, damit Sie dieses Feld leer lassen können.</td></tr>
    </table>

    Sie können mehr als eine Skriptaktion zum Installieren von mehreren Komponenten auf dem Cluster hinzufügen. Nachdem Sie die Skripts hinzugefügt haben, klicken Sie auf das Häkchen zum Starten den Cluster erstellen.

## <a name="use-giraph"></a>Verwenden von Giraph

Wir verwenden im Beispiel SimpleShortestPathsComputation, um die grundlegende <a href = "http://people.apache.org/~edwardyoon/documents/pregel.pdf">Pregel</a> Implementierung für die Suche nach der Suche nach kürzester Pfad zwischen Objekten in einem Diagramm zu veranschaulichen. Gehen Sie folgendermaßen vor, die Beispieldaten und die Stichprobe JAR-Datei hochladen, führen Sie einen Auftrag anhand des Beispiels SimpleShortestPathsComputation, und klicken Sie dann zeigen Sie die Ergebnisse an.

1. Hochladen einer Stichprobe Datendatei zu Azure Blob-Speicher. Klicken Sie auf Ihrem lokalen Arbeitsstationen erstellen Sie eine neue Datei namens **tiny_graph.txt**. Sie sollten die folgenden Zeilen enthalten:

        [0,0,[[1,1],[3,3]]]
        [1,0,[[0,1],[2,2],[3,1]]]
        [2,0,[[1,2],[4,4]]]
        [3,0,[[0,3],[1,1],[4,4]]]
        [4,0,[[3,4],[2,4]]]

    Laden Sie die Datei tiny_graph.txt in primären Speicher für Ihren Cluster HDInsight hoch. Anweisungen zum Hochladen von Daten finden Sie unter [Daten für Hadoop Aufträge in HDInsight hochladen](hdinsight-upload-data.md).

    Diese Daten beschreibt eine Beziehung zwischen Objekten in einem gerichteten Diagramm, mit dem Format [Quelle\_-Id, Quelle\_Wert [[Ziel\_Id], [Kante\_Wert];...]]. Jede Zeile steht für eine Beziehung zwischen einer **Quelle\_Id** Objekt und einem oder mehreren **Ziel\_Id** Objekte. Die **Kante\_Wert** (oder Stärke) können als die Stärke oder Abstand der Verbindung zwischen **Source_id** vorstellen und **Ziel\_Id**.

    Gezeichnet ab, und verwenden den Wert (oder Stärke) als des Abstands zwischen Objekten, könnte die oben angegebenen Daten wie folgt aussehen:

    ![tiny_graph.txt gezeichnet als Kreise mit unterschiedlichen Abstand zwischen Zeilen](./media/hdinsight-hadoop-giraph-install/giraph-graph.png)



4. Führen Sie das Beispiel SimpleShortestPathsComputation. Verwenden Sie die folgenden Azure PowerShell-Cmdlets zum Ausführen des Beispiels mithilfe der tiny_graph.txt-Datei als Eingabe. 

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

        $clusterName = "clustername"
        # Giraph examples jar
        $jarFile = "wasbs:///example/jars/giraph-examples.jar"
        # Arguments for this job
        $jobArguments = "org.apache.giraph.examples.SimpleShortestPathsComputation",
                        "-ca", "mapred.job.tracker=headnodehost:9010",
                        "-vif", "org.apache.giraph.io.formats.JsonLongDoubleFloatDoubleVertexInputFormat",
                        "-vip", "wasbs:///example/data/tiny_graph.txt",
                        "-vof", "org.apache.giraph.io.formats.IdWithValueTextOutputFormat",
                        "-op",  "wasbs:///example/output/shortestpaths",
                        "-w", "2"
        # Create the definition
        $jobDefinition = New-AzureHDInsightMapReduceJobDefinition
          -JarFile $jarFile
          -ClassName "org.apache.giraph.GiraphRunner"
          -Arguments $jobArguments

        # Run the job, write output to the Azure PowerShell window
        $job = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $jobDefinition
        Write-Host "Wait for the job to complete ..." -ForegroundColor Green
        Wait-AzureHDInsightJob -Job $job
        Write-Host "STDERR"
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $job.JobId -StandardError
        Write-Host "Display the standard output ..." -ForegroundColor Green
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $job.JobId -StandardOutput

    Ersetzen Sie im obigen Beispiel **Clustername** mit dem Namen der Cluster HDInsight, der Giraph installiert wurde.

5. Anzeigen der Ergebnisse an. Nachdem Sie der Auftrag abgeschlossen ist, werden die Ergebnisse in zwei Ausgabedateien im gespeichert der __Wasbs: / / / Beispiel/Out/Shotestpaths__ Ordner. Die Dateien werden als __Teil-m-00001__ und __Webpart-m-00002__bezeichnet. Führen Sie die folgenden Schritte aus, um herunterzuladen, und zeigen Sie die Ausgabe an:

        $subscriptionName = "<SubscriptionName>"       # Azure subscription name
        $storageAccountName = "<StorageAccountName>"   # Azure Storage account name
        $containerName = "<ContainerName>"             # Blob storage container name

        # Select the current subscription
        Select-AzureSubscription $subscriptionName

        # Create the Storage account context object
        $storageAccountKey = Get-AzureStorageKey $storageAccountName | %{ $_.Primary }
        $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey

        # Download the job output to the workstation
        Get-AzureStorageBlobContent -Container $containerName -Blob example/output/shortestpaths/part-m-00001 -Context $storageContext -Force
        Get-AzureStorageBlobContent -Container $containerName -Blob example/output/shortestpaths/part-m-00002 -Context $storageContext -Force

    Die __Beispiel/Ausgabe/Shortestpaths__ Directory-Struktur im aktuellen Verzeichnis auf Ihrem Computer erstellt, und Laden Sie die zwei Ausgabedateien zu diesem Speicherort.

    Verwenden Sie das Cmdlet __Katze__ , um den Inhalt der Dateien anzuzeigen:

        Cat example/output/shortestpaths/part*

    Die Ausgabe sollte ähnlich wie die folgende angezeigt:


        0   1.0
        4   5.0
        2   2.0
        1   0.0
        3   1.0

    Das Beispiel harte ist zunächst codierten SimpleShortestPathComputation Objekt-ID 1 und suchen Sie den Suche nach kürzester Pfad an anderen Objekten. Damit die Ausgabe als gelesen werden sollen `destination_id distance`, wobei Abstand den Wert (oder Stärke) der Ränder zwischen Objekt-ID 1 und die Ziel-ID getunnelt ist

    Visualisieren folgt, können Sie die Ergebnisse durch die geschätzte Pfade zwischen 1-ID und alle anderen Objekten unterwegs überprüfen. Beachten Sie, dass es sich bei der Suche nach kürzester Pfad-ID 1 bis 4-ID 5 ist. Dies ist der gesamte Abstand zwischen <span style="color:orange">ID 1 und 3</span>, und klicken Sie dann <span style="color:red">ID 3 und 4</span>.

    ![Zeichnen von Elementen als Kreise mit geschätzte Pfaden zwischen](./media/hdinsight-hadoop-giraph-install/giraph-graph-out.png)

## <a name="install-giraph-using-aure-powershell"></a>Installieren Sie mithilfe der Aure PowerShell Giraph

Finden Sie unter [Anpassen HDInsight Cluster mithilfe der Aktion Skript](hdinsight-hadoop-customize-cluster.md#call_scripts_using_powershell).  Im Beispiel veranschaulicht, wie mithilfe der PowerShell Azure Spark installieren. Sie müssen Sie das Skript zum Verwenden von [https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1)anpassen.

## <a name="install-giraph-using-net-sdk"></a>Verwenden von .NET SDK Giraph installieren

Finden Sie unter [Anpassen HDInsight Cluster mithilfe der Aktion Skript](hdinsight-hadoop-customize-cluster.md#call_scripts_using_azure_powershell). Im Beispiel veranschaulicht, wie mithilfe des .NET SDK Spark installieren. Sie müssen Sie das Skript zum Verwenden von [https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1)anpassen.


## <a name="see-also"></a>Siehe auch

- [Installieren von Giraph auf HDInsight Hadoop Cluster (Linux)](hdinsight-hadoop-giraph-install-linux.md)
- [Hadoop erstellen Cluster in HDInsight](hdinsight-provision-clusters.md): Allgemeine Informationen zum Erstellen von HDInsight Cluster.
- [Anpassen von HDInsight Cluster mithilfe der Aktion Skript][hdinsight-cluster-customize]: Allgemeine Informationen zum Anpassen von HDInsight Cluster mithilfe der Aktion Skript.
- [Skript für Aktion entwickeln Skripts für HDInsight](hdinsight-hadoop-script-actions.md).
- [Installieren und Verwenden von Spark auf HDInsight Cluster][hdinsight-install-spark]: Aktion Skript Beispiel zum Installieren von Spark.
- [Installieren von R auf HDInsight Cluster][hdinsight-install-r]: Aktion Skript Beispiel zum Installieren von R.
- [Installieren von Solr auf HDInsight Cluster](hdinsight-hadoop-solr-install.md): Aktion Skript Stichprobe Informationen zur Installation von Solr.



[tools]: https://github.com/Blackmist/hdinsight-tools
[aps]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/

[powershell-install]: ../powershell-install-configure.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md

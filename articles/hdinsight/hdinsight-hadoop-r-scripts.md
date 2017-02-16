<properties
    pageTitle="Verwenden Sie R in HDInsight Cluster anpassen | Microsoft Azure"
    description="Erfahren Sie, wie Sie mithilfe der Aktion Skript R installieren, und verwenden Sie R auf HDInsight Cluster."
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
    ms.date="09/14/2016"
    ms.author="jgao"/>

# <a name="install-and-use-r-on-hdinsight-hadoop-clusters"></a>Installieren und Verwenden von R auf HDInsight Hadoop Cluster

Erfahren Sie, wie Windows anpassen basierend HDInsight Cluster mit R mithilfe der Aktion Skript, und Cluster so R auf HDInsight verwenden. Der Geschenk für HDInsight [Premium Stufe](https://azure.microsoft.com/pricing/details/hdinsight/) umfasst R Server als Teil Ihrer HDInsight Cluster. Dadurch wird die R Skripts MapReduce und Spark verwenden, um verteilte Berechnungen auszuführen. Weitere Informationen finden Sie unter [Erste Schritte mit R Server auf HDInsight](hdinsight-hadoop-r-server-get-started.md). Informationen zum Verwenden von R mit einem Linux-basierten Cluster finden Sie unter [Installieren und Verwenden von R auf HDinsight Hadoop Cluster (Linux)](hdinsight-hadoop-r-scripts-linux.md).
 
Sie können mithilfe der *Aktion Skript*R auf einem beliebigen Cluster (Hadoop Storm, HBase, Spark) auf Azure HDInsight installieren. Ein Beispiel-Skript R auf einem Cluster HDInsight installiert ist aus einer schreibgeschützten Azure-Speicher Blob am [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1)verfügbar. 

**Verwandte Artikel**

- [Installieren und Verwenden von R auf HDinsight Hadoop Cluster (Linux)](hdinsight-hadoop-r-scripts-linux.md)
- [Hadoop erstellen Cluster in HDInsight](hdinsight-provision-clusters.md): Allgemeine Informationen zum Erstellen von HDInsight Cluster
- [Anpassen von HDInsight Cluster mithilfe der Aktion Skript][hdinsight-cluster-customize]: Allgemeine Informationen zum Anpassen von HDInsight Cluster mithilfe der Aktion Skript
- [Entwickeln Sie die Aktion Skript Skripts für HDInsight](hdinsight-hadoop-script-actions.md)

## <a name="what-is-r"></a>Was ist die R?

<a href="http://www.r-project.org/" target="_blank">R Projekt für statistische Computing</a> ist einer geöffneten Quelle Sprache und Umgebung für statistische computing. R bietet mehrere hundert integrierte statistische Funktionen und einem eigenen Programmiersprache, die kombiniert Aspekte der funktionsübergreifendes und objektorientierten Programmierung. Darüber hinaus umfangreiche grafisch bereit. R ist die bevorzugte programming Umgebung für die meisten professional Statistiker und Wissenschaftler in einer Vielzahl von Feldern.

R ist kompatibel mit Azure BLOB-Speicher (WASB), sodass es gespeicherten Daten mithilfe von R auf HDInsight verarbeitet werden können.  

## <a name="install-r"></a>Installieren von R

Ein [Beispiel-Skript](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1) R auf einem Cluster HDInsight installiert ist aus einer schreibgeschützten Blob in Azure-Speicher verfügbar. Dieser Abschnitt enthält Anweisungen zur Verwendung von Skript hilft beim Erstellen des Clusters über das Azure-Portal an.

> [AZURE.NOTE] Das Beispielskript wurde mit HDInsight Clusterversion 3.1 eingeführt werden. Weitere Informationen zu HDInsight Cluster Versionen finden Sie unter [HDInsight Cluster Versionen](hdinsight-component-versioning.md).

1. Bei der Erstellung eines HDInsight Clusters aus dem Portal klicken Sie auf **Optional Konfiguration**, und klicken Sie dann auf **Skript-Aktionen**.
2. Geben Sie auf der Seite **Skriptaktionen** die folgenden Werte ein:

    ![Verwenden Sie Skript für Aktion in einem Cluster anpassen] (./media/hdinsight-hadoop-r-scripts/hdi-r-script-action.png "Verwenden Sie Skript für Aktion in einem Cluster anpassen")

    <table border='1'>
        <tr><th>Eigenschaft</th><th>Wert</th></tr>
        <tr><td>Namen</td>
            <td>Geben Sie einen Namen für die Skriptaktion, z. B. <b>R installieren</b>.</td></tr>
        <tr><td>URI-Skript</td>
            <td>Geben Sie den URI an das Skript, das zum Anpassen des Clusters, beispielsweise <i>https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1</i> aufgerufen wird</td></tr>
        <tr><td>Knotentyp</td>
            <td>Geben Sie die Knoten, auf denen das Skript Anpassung ausgeführt wird. Sie können <b>Alle Knoten</b>, <b>nur Knoten Kopf</b>oder <b>Arbeitskollegen Knoten</b> nur auswählen.
        <tr><td>Parameter</td>
            <td>Geben Sie den Parameter aus, wenn das Skript erforderlich. Das Skript zum Installieren von R ist jedoch keine Parameter, erforderlich, damit Sie dieses Feld leer lassen können.</td></tr>
    </table>

    Sie können mehr als eine Skriptaktion zum Installieren von mehreren Komponenten auf dem Cluster hinzufügen. Nachdem Sie die Skripts hinzugefügt haben, klicken Sie auf das Häkchen um crating Cluster zu starten.

Das Skript können Sie auch um mithilfe von Azure PowerShell oder HDInsight .NET SDK R auf HDInsight zu installieren. Anweisungen für diese Verfahren werden später in diesem Artikel bereitgestellt.

## <a name="run-r-scripts"></a>Führen Sie R Skripts
In diesem Abschnitt beschrieben, wie Sie ein Skript R für Hadoop Cluster mit HDInsight ausgeführt wird.

1. **Eine Remotedesktop Verbindung mit dem Cluster herstellen**: aus im Portal Remotedesktop für Sie erstellt, mit R installiert haben Cluster aktivieren, und klicken Sie dann auf den Cluster verbinden. Anweisungen finden Sie unter [Verbinden auf HDInsight Cluster RDP verwenden](hdinsight-administer-use-management-portal.md#rdp).

2. **Öffnen Sie die Konsole R**: das R Installation zusammengeführt werden einen Link zur Konsole R auf dem Desktop des am Knotens. Klicken Sie darauf, um die R-Verwaltungskonsole zu öffnen.

3. **Führen Sie das Skript R**: R das Skript direkt von der Konsole R von einfügen, indem Sie ihn auswählen und Drücken der EINGABETASTE ausgeführt werden kann. Hier ist ein einfaches Beispiel-Skript, die die Zahlen 1 und 100 generiert und multipliziert sie dann mit 2 aus.

        library(rmr2)
        library(rhdfs)
        ints = to.dfs(1:100)
        calc = mapreduce(input = ints, map = function(k, v) cbind(v, 2*v))
        from.dfs(calc)

Rufen Sie die ersten beiden Zeilen die RHadoop-Bibliotheken, die mit r installiert werden Die letzte Zeile druckt die Ergebnisse in der Konsole an. Die Ausgabe sollte wie folgt aussehen:

    [1,]  1 2
    [2,]  2 4
    .
    .
    .
    [98,]  98 196
    [99,]  99 198
    [100,] 100 200


## <a name="install-r-using-aure-powershell"></a>Installieren Sie mithilfe der Aure PowerShell R

Finden Sie unter [Anpassen HDInsight Cluster mithilfe der Aktion Skript](hdinsight-hadoop-customize-cluster.md#call_scripts_using_powershell).  Im Beispiel veranschaulicht, wie mithilfe der PowerShell Azure Spark installieren. Sie müssen Sie das Skript zum Verwenden von [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1)anpassen.

## <a name="install-r-using-net-sdk"></a>Verwenden von .NET SDK R installieren

Finden Sie unter [Anpassen HDInsight Cluster mithilfe der Aktion Skript](hdinsight-hadoop-customize-cluster.md#call_scripts_using_azure_powershell). Im Beispiel veranschaulicht, wie mithilfe des .NET SDK Spark installieren. Sie müssen Sie das Skript zum Verwenden von [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps11)anpassen.


## <a name="see-also"></a>Siehe auch

- [Installieren und Verwenden von R auf HDinsight Hadoop Cluster (Linux)](hdinsight-hadoop-r-scripts-linux.md)
- [Hadoop erstellen Cluster in HDInsight](hdinsight-provision-clusters.md): Allgemeine Informationen zum Erstellen von HDInsight Cluster
- [Anpassen von HDInsight Cluster mithilfe der Aktion Skript][hdinsight-cluster-customize]: Allgemeine Informationen zum Anpassen von HDInsight Cluster mithilfe der Aktion Skript
- [Entwickeln Sie die Aktion Skript Skripts für HDInsight](hdinsight-hadoop-script-actions.md)
- [Installieren und Verwenden von Spark auf HDInsight Cluster][hdinsight-install-spark]: Aktion Skript Beispiel zum Installieren von Spark
- [Installieren von Giraph auf HDInsight Cluster](hdinsight-hadoop-giraph-install.md): Aktion Skript Beispiel zum Installieren von Giraph
- [Installieren von Solr auf HDInsight Cluster](hdinsight-hadoop-solr-install-linux.md): Aktion Skript Stichprobe Informationen zur Installation von Solr.

[powershell-install-configure]: powershell-install-configure.md
[hdinsight-provision]: ../hdinsight-provision-clusters/
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
[hdinsight-install-spark]: hdinsight-apache-spark-jupyter-spark-sql.md

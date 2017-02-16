<properties
    pageTitle="Installieren und Verwenden von Giraph auf Linux-basierten HDInsight (Hadoop) | Microsoft Azure"
    description="Informationen Sie zum Installieren von Giraph auf HDInsight Linux-basierten Cluster mit Skript-Aktionen. Skript-Aktionen können Sie den Cluster während der Erstellung anpassen, indem Sie Cluster-Konfiguration ändern oder Dienste und Dienstprogramme zu installieren."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="larryfr"/>

# <a name="install-giraph-on-hdinsight-hadoop-clusters-and-use-giraph-to-process-large-scale-graphs"></a>Installieren von Giraph auf HDInsight Hadoop Cluster und Giraph verwenden, um umfangreiche Diagramme zu verarbeiten

Sie können Giraph auf einem beliebigen Cluster in Hadoop auf Azure HDInsight mithilfe der **Aktion Skript** zum Anpassen eines Clusters installieren.

In diesem Thema erfahren Sie, wie Giraph mithilfe der Aktion Skript installiert. Nachdem Sie Giraph installiert haben, erfahren Sie auch, wie Sie Giraph für häufigste Applications, welche umfangreiche Diagramme Verarbeitungszeit ist.

> [AZURE.NOTE] Die Informationen in diesem Artikel sind HDInsight Linux-basierten Cluster-spezifischen. Informationen zum Arbeiten mit Windows-basierten Cluster finden Sie unter [Installieren von Giraph auf HDInsight Hadoop Cluster (Windows)](hdinsight-hadoop-giraph-install.md)

## <a name="a-namewhatisawhat-is-giraph"></a><a name="whatis"></a>Was ist Giraph?

[Apache Giraph](http://giraph.apache.org/) können Sie mithilfe von Hadoop Verarbeitung Graph ausführen, und kann mit Azure HDInsight verwendet werden. Diagramme modellieren Beziehungen zwischen Objekten, wie die Verbindungen zwischen Routern in einem großen Netzwerk wie das Internet oder Beziehungen zwischen Personen in sozialen Netzwerken (auch als sozialen Diagramm bezeichnet). Diagramm-Verarbeitung ermöglicht es Ihnen, Grund über die Beziehungen zwischen Objekten in einem Diagramm, z. B.:

- Identifizieren potenzielle Freunde basierend auf Ihrer aktuellen Beziehungen.
- Identifizieren den kürzesten Weg zwischen zwei Computern in einem Netzwerk.
- Den Seite Rang Webseiten zu berechnen.

> [AZURE.WARNING] Komponenten, die mit dem HDInsight Cluster bereitgestellt werden vollständig unterstützt, und hilft Microsoft Support zum Isolieren und Beheben von Problemen im Zusammenhang mit dieser Komponenten.
>
> Benutzerdefinierte Komponenten, wie z. B. Giraph, erhalten im Handel angemessenen Support, um Sie dabei unterstützen, das Problem zu beheben. Dadurch kann das Problem zu beheben, oder werden Sie aufgefordert, die verfügbaren Kanäle für das open-Source-Technologien populärer, wo eingehender Erfahrung für diese Technologie gefunden wird. Angenommen, es gibt viele Communitywebsites, die, wie verwendet werden können: [MSDN-Forum für HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Apache Projekte außerdem Projektwebsites auf [http://apache.org](http://apache.org), zum Beispiel: [Hadoop](http://hadoop.apache.org/).

##<a name="what-the-script-does"></a>Was bedeutet, dass das Skript

Dieses Skript führt die folgenden Aktionen aus:

* Giraph zu Installationen`/usr/hdp/current/giraph`
* Kopien der `giraph-examples.jar` Datei Standardspeicher (WASB) für Ihren Cluster:`/example/jars/giraph-examples.jar`

## <a name="a-nameinstallainstall-giraph-using-script-actions"></a><a name="install"></a>Installieren von Giraph mit Skript-Aktionen

Ein Beispiel-Skript zum Installieren von Giraph in einem HDInsight Cluster steht an folgendem Speicherort.

    https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh

Dieser Abschnitt enthält Anweisungen zum Skript hilft beim Erstellen des Clusters mithilfe der Azure-Portal zu verwenden. 

> [AZURE.NOTE] Azure PowerShell, die CLI Azure, HDInsight .NET SDK oder Azure Ressourcenmanager Vorlagen können auch verwendet werden, Skriptaktionen anwenden. Sie können auch Skriptaktionen anwenden, um Cluster bereits ausgeführt. Weitere Informationen finden Sie unter [Anpassen HDInsight Cluster mit Skript-Aktionen](hdinsight-hadoop-customize-cluster-linux.md).

1. Erstes Erstellen eines Clusters anhand der Schritte in [Erstellen Linux-basierten HDInsight Cluster](hdinsight-hadoop-create-linux-clusters-portal.md), aber nicht abschließen der Erstellung.

2. Klicken Sie auf das Blade **Optionale Konfiguration** wählen Sie **Skript-Aktionen**aus, und geben Sie die folgenden Informationen:

    * __NAME__: Geben Sie einen Anzeigenamen für die Skriptaktion.
    * __Skript-URI__: https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh
    * __Kopf__: Aktivieren Sie diese Option
    * __WORKER__: deaktiviert lassen
    * __ZOOKEEPER__: deaktiviert lassen
    * __Parameter__: lassen Sie dieses Feld leer

3. Am unteren Rand der **Skript-Aktionen**verwenden Sie die Schaltfläche **auswählen** zum Speichern der Konfiguration aus. Verwenden Sie schließlich die Schaltfläche **auswählen** am unteren Rand der Blade **Optionale Konfiguration** , um die optionale Konfigurationsinformationen zu speichern.

4. Fortsetzen Sie den Cluster erstellen, wie in [HDInsight erstellen Linux-basierten Cluster](hdinsight-hadoop-create-linux-clusters-portal.md)beschrieben.

## <a name="a-nameusegiraphahow-do-i-use-giraph-in-hdinsight"></a><a name="usegiraph"></a>Wie verwende ich Giraph in HDInsight?

Sobald der Cluster erstellen abgeschlossen ist, gehen Sie folgendermaßen vor, im Lieferumfang von Giraph SimpleShortestPathsComputation-Beispiel ausführen. Dadurch wird die grundlegende <a href = "http://people.apache.org/~edwardyoon/documents/pregel.pdf">Pregel</a> Implementierung für die Suche nach der Suche nach kürzester Pfad zwischen Objekten in einem Diagramm implementiert.

1. Verbinden Sie mit dem HDInsight Cluster SSH verwenden:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    Weitere Informationen zum Verwenden von SSH mit HDInsight finden Sie unter den folgenden:

    * [Verwenden von SSH mit Linux-basierten Hadoop auf HDInsight von Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [Verwenden von SSH mit Linux-basierten Hadoop auf HDInsight von Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

1. Erstellen einer neuen Datei mit dem Namen __tiny_graph.txt__anhand der folgenden:

        nano tiny_graph.txt

    Verwenden Sie die folgenden als den Inhalt dieser Datei ein:

        [0,0,[[1,1],[3,3]]]
        [1,0,[[0,1],[2,2],[3,1]]]
        [2,0,[[1,2],[4,4]]]
        [3,0,[[0,3],[1,1],[4,4]]]
        [4,0,[[3,4],[2,4]]]

    Diese Daten beschreibt eine Beziehung zwischen Objekten in einem gerichteten Diagramm, mit dem Format [Quelle\_-Id, Quelle\_Wert [[Ziel\_Id], [Kante\_Wert];...]]. Jede Zeile steht für eine Beziehung zwischen einer **Quelle\_Id** Objekt und einem oder mehreren **Ziel\_Id** Objekte. Die **Kante\_Wert** (oder Stärke) können als die Stärke oder Abstand der Verbindung zwischen **Source_id** vorstellen und **Ziel\_Id**.

    Gezeichnet ab, und verwenden den Wert (oder Stärke) als des Abstands zwischen Objekten, könnte die oben angegebenen Daten wie folgt aussehen:

    ![tiny_graph.txt gezeichnet als Kreise mit unterschiedlichen Abstand zwischen Zeilen](./media/hdinsight-hadoop-giraph-install-linux/giraph-graph.png)

2. Um die Datei speichern möchten, verwenden Sie __STRG + X__, und klicken Sie dann __Y__, und klicken Sie abschließend __EINGABETASTE__ auf den Dateinamen ein.

3. Verwenden Sie zum Speichern der Daten in der primären Speicher für Ihren Cluster HDInsight Folgendes ein:

        hdfs dfs -put tiny_graph.txt /example/data/tiny_graph.txt

4. Führen Sie im SimpleShortestPathsComputation wird mit dem folgenden Befehl ein.

         yarn jar /usr/hdp/current/giraph/giraph-examples.jar org.apache.giraph.GiraphRunner org.apache.giraph.examples.SimpleShortestPathsComputation -ca mapred.job.tracker=headnodehost:9010 -vif org.apache.giraph.io.formats.JsonLongDoubleFloatDoubleVertexInputFormat -vip /example/data/tiny_graph.txt -vof org.apache.giraph.io.formats.IdWithValueTextOutputFormat -op /example/output/shortestpaths -w 2

    Die mit diesem Befehl verwendete Parameter werden in der folgenden Tabelle beschrieben.

  	| Parameter | Was bedeutet |
  	| --------- | ------------ |
  	| `jar /usr/hdp/current/giraph/giraph-examples.jar` | Die JAR-Datei, die in den Beispielen enthält. |
  	| `org.apache.giraph.GiraphRunner` | Die Klasse verwendet, um die Beispiele zu starten. |
  	| `org.apache.giraph.examples.SimpleShortestPathsCoputation` | Im Beispiel, die ausgeführt werden sollen. In diesem Fall wird den Suche nach kürzester Pfad zwischen ID 1 und alle anderen IDs im Diagramm berechnet werden. |
  	| `-ca mapred.job.tracker=headnodehost:9010` | Die Headnode für den Cluster. |
  	| `-vif org.apache.giraph.io.formats.JsonLongDoubleFloatDoubleVertexInputFromat` | Das Eingabeformat für die eingegebenen Daten verwendet werden soll. |
  	| `-vip /example/data/tiny_graph.txt` | Die Eingabedaten-Datei. |
  	| `-vof org.apache.giraph.io.formats.IdWithValueTextOutputFormat` | Das Ausgabeformat. In diesem Fall-ID und Wert als nur-Text. |
  	| `-op /example/output/shortestpaths` | Die Ausgabe Position. |
  	| `-w 2` | Die Anzahl der Kollegen zu verwenden. In diesem Fall 2. |

    Weitere Informationen zu diesen und anderen Parameter zusammen mit Giraph Beispiele finden Sie unter der [Giraph Schnellstart](http://giraph.apache.org/quick_start.html).

5. Sobald das Projekt abgeschlossen ist, werden die Ergebnisse gespeichert werden, der __Wasbs: / / / Beispiel/Out/Shotestpaths__ Directory. Die erstellten Dateien beginnen mit __Webpart-m -__ und die Endzeit für eine Zahl, die dem ersten, zweiten, usw. Datei angibt. Verwenden Sie zum Anzeigen der Ausgabe Folgendes ein:

        hdfs dfs -text /example/output/shortestpaths/*

    Die Ausgabe sollte ähnlich wie die folgende angezeigt:

        0   1.0
        4   5.0
        2   2.0
        1   0.0
        3   1.0

    Das Beispiel harte ist zunächst codierten SimpleShortestPathComputation Objekt-ID 1 und suchen Sie den Suche nach kürzester Pfad an anderen Objekten. Damit die Ausgabe als gelesen werden sollen `destination_id distance`, wobei Abstand den Wert (oder Stärke) der Ränder zwischen Objekt-ID 1 und die Ziel-ID getunnelt ist

    Visualisieren folgt, können Sie die Ergebnisse durch die geschätzte Pfade zwischen 1-ID und alle anderen Objekten unterwegs überprüfen. Beachten Sie, dass es sich bei der Suche nach kürzester Pfad-ID 1 bis 4-ID 5 ist. Dies ist der gesamte Abstand zwischen <span style="color:orange">ID 1 und 3</span>, und klicken Sie dann <span style="color:red">ID 3 und 4</span>.

    ![Zeichnen von Elementen als Kreise mit geschätzte Pfaden zwischen](./media/hdinsight-hadoop-giraph-install-linux/giraph-graph-out.png)


## <a name="next-steps"></a>Nächste Schritte

- [Installieren und verwenden Sie Cluster Farbton auf HDInsight](hdinsight-hadoop-hue-linux.md). Farbton ist ein Web-Benutzeroberfläche, die erleichtert das Erstellen, ausführen und speichern Schwein und Struktur Aufträge sowie Durchsuchen der Standard-Speicherplatz für Ihr HDInsight cluster.

- [Installieren von R auf HDInsight Cluster](hdinsight-hadoop-r-scripts-linux.md): cluster Anpassung zum Installieren und Verwenden von R auf HDInsight Hadoop Cluster-Anweisungen zu verwenden. R ist ein offener Quelle Sprache und Umgebung für statistische computing. Darüber hundert integrierten statistische Funktionen und einem eigenen Programmiersprache, die kombiniert Aspekte der funktionsübergreifendes und objektorientierten Programmierung. Darüber hinaus umfangreiche grafisch bereit.

- [Installieren von Solr auf HDInsight Cluster](hdinsight-hadoop-solr-install-linux.md). Verwenden Sie Cluster Anpassung, um Solr auf HDInsight Hadoop Cluster zu installieren. Solr können Sie leistungsfähige Suchvorgänge auf gespeicherten Daten durchführen.

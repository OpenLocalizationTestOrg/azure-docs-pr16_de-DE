<properties
    pageTitle="Installieren Sie R auf Linux-basierten HDInsight | Microsoft Azure"
    description="Informationen Sie zum Installieren und Verwenden von R Hadoop Linux-basierten Cluster anpassen."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="larryfr"/>

# <a name="install-and-use-r-on-hdinsight-hadoop-clusters"></a>Installieren und Verwenden von R auf HDInsight Hadoop Cluster

Sie können mithilfe der **Aktion Skript** Cluster Anpassung R auf einem beliebigen Cluster in Hadoop auf HDInsight installieren. Dadurch können Daten Wissenschaftler und Analysten R verwenden, um die leistungsstarken MapReduce/aus Framework Programmierung bereitstellen, um große Datenmengen auf Hadoop Cluster zu verarbeiten, die in HDInsight bereitgestellt werden.

> [AZURE.IMPORTANT] Der Geschenk für HDInsight [Premium Stufe](https://azure.microsoft.com/pricing/details/hdinsight/) umfasst R Server als Teil Ihrer HDInsight Cluster. Dadurch wird die R Skripts MapReduce und Spark verwenden, um verteilte Berechnungen auszuführen. Weitere Informationen finden Sie unter [Erste Schritte mit R Server auf HDInsight](hdinsight-hadoop-r-server-get-started.md). 


## <a name="what-is-r"></a>Was ist die R?

<a href="http://www.r-project.org/" target="_blank">R Projekt für statistische Computing</a> ist einer geöffneten Quelle Sprache und Umgebung für statistische computing. R bietet mehrere hundert integrierte statistische Funktionen und einem eigenen Programmiersprache, die kombiniert Aspekte der funktionsübergreifendes und objektorientierten Programmierung. Darüber hinaus umfangreiche grafisch bereit. R ist die bevorzugte programming Umgebung für die meisten professional Statistiker und Wissenschaftler in einer Vielzahl von Feldern.

R Skripts können auf Hadoop Cluster in HDInsight ausgeführt werden, die mit Skriptaktion beim Erstellen die Umgebung R installieren angepasst wurden. R ist kompatibel mit Azure BLOB-Speicher (WASB), sodass es gespeicherten Daten mithilfe von R auf HDInsight verarbeitet werden können.

## <a name="what-the-script-does"></a>Was bedeutet, dass das Skript

Die Skriptaktion zur Installation von R auf Ihren Cluster HDInsight verwendet die folgenden Ubuntu-Paketen, die eine einfache Installation von R bieten-Installationen:

* [R-Base](http://packages.ubuntu.com/precise/r-base): Basis GNU R-Paket
* [R-Basis-Entwickler](http://packages.ubuntu.com/precise/r-base-dev): Auxilliary GNU R-Paketen

Die folgenden RHadoop Pakete werden auch installiert und die Integration mit MapReduce und HDFS bereitstellen:

* [rmr2](https://github.com/RevolutionAnalytics/rmr2): ermöglicht Entwicklern das R Hadoop MapReduce verwenden
* [Rhdfs](https://github.com/RevolutionAnalytics/rhdfs): ermöglicht Entwicklern das R Hadoop HDFS (WASB für HDInsight) verwenden.

Darüber hinaus werden die folgenden R Pakete installiert:

| R-Paket | Was es enthält. |
| --------- | ---------------- |
| [rJava](https://cran.r-project.org/web/packages/rJava/index.html) | Eine niedrige Sicherheitsstufe R zu Java-Schnittstelle. |
| [RCPP](https://cran.r-project.org/web/packages/Rcpp/index.html) | Integration von R und C++. |
| [RJSONIO](https://cran.r-project.org/web/packages/RJSONIO/index.html) | JSON Objekte serialisieren/Deserialisieren R |
| [bitops](https://cran.r-project.org/web/packages/bitops/index.html) | Funktionen für die ganze Zahl Vektoren bitweises Vorgänge. |
| [Übersicht](https://cran.r-project.org/web/packages/digest/index.html) | Cryptographic Hash Übersichten R Objekte zu erstellen. |
| [Funktionsübergreifendes](https://cran.r-project.org/web/packages/functional/index.html) | Curry, Verfassen und anderen Funktionen höherer Ordnung |
| [reshape2](https://cran.r-project.org/web/packages/reshape2/index.html) | Flexible umstrukturieren und Aggregieren von Daten. |
| [stringr](https://cran.r-project.org/web/packages/stringr/index.html) | Einfache und konsistente Wrapper für allgemeine Zeichenfolgenoperationen. |
| [plyr](https://cran.r-project.org/web/packages/plyr/index.html) | Tools zum Teilen, anwenden und Daten zu kombinieren. |
| [caTools](https://cran.r-project.org/web/packages/caTools/index.html) | Tools zum Verschieben der Fenster Statistik, GIF, Base64, ROC AUC, usw.. |
| [stringdist](https://cran.r-project.org/web/packages/stringdist/index.html) | Ungefähre übereinstimmenden Zeichenfolge und Abstand Zeichenfolgenfunktionen. |

## <a name="install-r-using-script-actions"></a>Installieren von R mit Skript-Aktionen

Die folgende Skriptaktion wird verwendet, um die R auf einem Cluster HDInsight zu installieren. https://hdiconfigactions.BLOB.Core.Windows.NET/linuxrconfigactionv01/r-Installer-v01.sh
    
Dieser Abschnitt enthält Informationen dazu, wie Sie das Skript beim Erstellen eines neuen Clusters über das Azure-Portal zu verwenden. 

> [AZURE.NOTE] Azure PowerShell, die CLI Azure, HDInsight .NET SDK oder Azure Ressourcenmanager Vorlagen können auch verwendet werden, Skriptaktionen anwenden. Sie können auch Skriptaktionen anwenden, um Cluster bereits ausgeführt. Weitere Informationen finden Sie unter [Anpassen HDInsight Cluster mit Skript-Aktionen](hdinsight-hadoop-customize-cluster-linux.md).

1. Starten Sie einen Cluster anhand der Schritte in [HDInsight Bereitstellen von Linux-basierten Cluster](hdinsight-hadoop-provision-linux-clusters.md#portal)bereitgestellt, aber nicht abschließen Sie provisioning.

2. Klicken Sie auf das Blade **Optionale Konfiguration** wählen Sie **Skript-Aktionen**aus, und geben Sie die folgenden Informationen:

    * __NAME__: Geben Sie einen Anzeigenamen für die Skriptaktion.
    * __Skript-URI__: https://hdiconfigactions.blob.core.windows.net/linuxrconfigactionv01/r-installer-v01.sh
    * __Kopf__: Aktivieren Sie diese Option
    * __WORKER__: Aktivieren Sie diese Option
    * __ZOOKEEPER__: Aktivieren Sie diese Option, um auf dem Zookeeper Knoten installieren.
    * __Parameter__: lassen Sie dieses Feld leer

3. Am unteren Rand der **Skript-Aktionen**verwenden Sie die Schaltfläche **auswählen** zum Speichern der Konfiguration aus. Verwenden Sie schließlich die Schaltfläche **auswählen** am unteren Rand der Blade **Optionale Konfiguration** , um die optionale Konfigurationsinformationen zu speichern.

4. Fahren Sie Cluster bereitgestellt, wie in [HDInsight Bereitstellen von Linux-basierten Cluster](hdinsight-hadoop-provision-linux-clusters.md#portal)beschrieben.

## <a name="run-r-scripts"></a>Führen Sie R Skripts

Nachdem der Cluster bereitgestellt hat, gehen Sie folgendermaßen vor, um R verwenden, um einen Vorgang MapReduce im Cluster ausführen.

1. Verbinden Sie mit dem HDInsight Cluster SSH verwenden:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    Weitere Informationen zum Verwenden von SSH mit HDInsight finden Sie unter den folgenden:

    * [Verwenden von SSH mit Linux-basierten Hadoop auf HDInsight von Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [Verwenden von SSH mit Linux-basierten Hadoop auf HDInsight von Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

2. Aus der `username@hn0-CLUSTERNAME:~$` dazu aufgefordert werden, geben Sie den folgenden Befehl aus, um eine interaktive R-Sitzung zu starten:

        R

3. Geben Sie das folgende R Programm aus. Dies generiert die Zahlen 1 bis 100 und multipliziert sie dann mit 2.

        library(rmr2)
        ints = to.dfs(1:100)
        calc = mapreduce(input = ints, map = function(k, v) cbind(v, 2*v))

    Die erste Zeile ruft die Bibliothek rmr2 RHadoop, die für Vorgänge MapReduce verwendet wird.

    Die zweite Zeile generiert Werte 1-100, und klicken Sie dann speichert sie auf der Hadoop Dateisystem unter Verwendung `to.dfs`.

    Die dritte Zeile erstellt einen MapReduce Prozess mit Toolbar die über rmr2 und Verarbeitung beginnt. Finden Sie unter mehrere Zeilen ältere Bildlauf während die Verarbeitung beginnt.

4. Als Nächstes verwenden Sie die folgenden den temporären Pfad angezeigt, dem die Ausgabe MapReduce abgelegt wurde:

        print(calc())

    Dies sollten ungefähr `/tmp/file5f615d870ad2`. Wenn die tatsächliche Ausgabe anzeigen möchten, verwenden Sie Folgendes ein:

        print(from.dfs(calc))

    Die Ausgabe sollte wie folgt aussehen:

        [1,]  1 2
        [2,]  2 4
        .
        .
        .
        [98,]  98 196
        [99,]  99 198
        [100,] 100 200

5. Zum Beenden R Geben Sie Folgendes ein:

        q()


## <a name="next-steps"></a>Nächste Schritte

- [Installieren und verwenden Sie Cluster Farbton auf HDInsight](hdinsight-hadoop-hue-linux.md). Farbton ist ein Web-Benutzeroberfläche, die erleichtert das Erstellen, ausführen und speichern Schwein und Struktur Aufträge sowie Durchsuchen der Standard-Speicherplatz für Ihr HDInsight cluster.

- [Installieren von Giraph auf HDInsight Cluster](hdinsight-hadoop-giraph-install.md). Verwenden Sie Cluster Anpassung, um Giraph auf HDInsight Hadoop Cluster zu installieren. Giraph ermöglicht es Ihnen, mit Hadoop Graph-Verarbeitung ausführen, und kann mit Azure HDInsight verwendet werden.

- [Installieren von Solr auf HDInsight Cluster](hdinsight-hadoop-solr-install.md). Verwenden Sie Cluster Anpassung, um Solr auf HDInsight Hadoop Cluster zu installieren. Solr können Sie leistungsfähige Suchvorgänge auf gespeicherten Daten durchführen.

- [Installieren von Farbton auf HDInsight Cluster](hdinsight-hadoop-hue-linux.md). Verwenden Sie Cluster Anpassung, um Farbton auf HDInsight Hadoop Cluster zu installieren. Farbton ist eine Reihe von Webanwendungen Interaktion mit einem Hadoop Cluster verwendet.

[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md

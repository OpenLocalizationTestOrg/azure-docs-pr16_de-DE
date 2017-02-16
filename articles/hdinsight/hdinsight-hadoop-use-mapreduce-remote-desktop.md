<properties
   pageTitle="MapReduce und Remotedesktop mit Hadoop in HDInsight | Microsoft Azure"
   description="Informationen Sie zum Verwenden von Remotedesktop zum Verbinden mit Hadoop auf HDInsight und MapReduce Aufträge ausführen."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/27/2016"
   ms.author="larryfr"/>

# <a name="use-mapreduce-in-hadoop-on-hdinsight-with-remote-desktop"></a>Verwenden von MapReduce in Hadoop auf HDInsight mithilfe von Remotedesktop

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

In diesem Artikel erfahren Sie, wie Verbinden mit einem Hadoop auf HDInsight Cluster mithilfe des Remote Desktop, und führen Sie dann mithilfe des Befehls Hadoop MapReduce Aufträge.

##<a name="a-idprereqaprerequisites"></a><a id="prereq"></a>Erforderliche Komponenten

Um die Schritte in diesem Artikel ausführen zu können, benötigen Sie Folgendes:

* Ein Windows-basiertes HDInsight (Hadoop auf HDInsight) cluster

* Ein Clientcomputer unter Windows 7, Windows 8 oder Windows-10

##<a name="a-idconnectaconnect-with-remote-desktop"></a><a id="connect"></a>Verbinden mit Remotedesktop

Damit anhand der Anweisungen in [Verbindung herstellen mit HDInsight Cluster mit RDP](hdinsight-administer-use-management-portal.md#rdp)herstellen, dann Remotedesktop für den Cluster HDInsight aktivieren.

##<a name="a-idhadoopause-the-hadoop-command"></a><a id="hadoop"></a>Verwenden Sie den Befehl Hadoop

Wenn Sie auf dem Desktop für den HDInsight Cluster verbunden sind, gehen Sie folgendermaßen vor, um eine MapReduce mithilfe des Befehls Hadoop auszuführen:

1. Starten Sie die **Befehlszeile Hadoop**aus den Desktop HDInsight. Daraufhin wird eine neue Befehlszeile in der **c:\apps\dist\hadoop-&lt;Versionsnummer >** Directory.

    > [AZURE.NOTE] Die Versionsnummer wird als Hadoop aktualisiert wird. Die **HADOOP_HOME** Umgebungsvariable kann verwendet werden, um den Pfad zu finden. Beispielsweise `cd %HADOOP_HOME%` wechselt Verzeichnisse zum Hadoop-Verzeichnis, ohne dass Sie die Versionsnummer kennen.

2. Um die **Hadoop** -Befehl verwenden, um ein Beispiel MapReduce Auftrag ausgeführt werden, verwenden Sie den folgenden Befehl aus:

        hadoop jar hadoop-mapreduce-examples.jar wordcount wasbs:///example/data/gutenberg/davinci.txt wasbs:///example/data/WordCountOutput

    Hierdurch wird die **Wordcount** -Klasse, die in der Datei **Hadoop-Mapreduce-examples.jar** im aktuellen Verzeichnis enthalten ist. Als Eingabe, verwendet sie das Dokument **wasbs://example/data/gutenberg/davinci.txt** und Ausgabe wird am Speicherorten abgelegt: **Wasbs: / / / Beispiel/Daten/WordCountOutput**.

    > [AZURE.NOTE] Weitere Informationen zu diesen Auftrag MapReduce und den Beispieldaten finden Sie unter <a href="hdinsight-use-mapreduce.md">Verwenden von MapReduce in HDInsight Hadoop</a>.

2. Wie wird Sie verarbeitet, und es Informationen ähnlich wie der folgende gibt, wenn das Projekt abgeschlossen ist, gibt der Auftrag Details aus:

        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623

3. Wenn das Projekt abgeschlossen ist, verwenden Sie den folgenden Befehl die Ausgabedateien an **Wasbs://example/data/WordCountOutput**gespeicherte aufgelistet:

        hadoop fs -ls wasbs:///example/data/WordCountOutput

    Dadurch sollte die beiden Dateien **_SUCCESS** und **Webpart-R-00000**angezeigt. Die **Webpart-R-00000** -Datei enthält die Ausgabe für dieses Projekt.

    > [AZURE.NOTE] Einige MapReduce Einzelvorgänge können die Ergebnisse auf mehrere **Webparts-R-###** Dateien aufgeteilt. Wenn dies der Fall ist, verwenden Sie die ### Suffix an, dass die Reihenfolge der Dateien.

4. Wenn die Ausgabe die anzeigen möchten, verwenden Sie den folgenden Befehl aus:

        hadoop fs -cat wasbs:///example/data/WordCountOutput/part-r-00000

    Dies zeigt eine Liste der Wörter, die in der Datei **wasbs://example/data/gutenberg/davinci.txt** zusammen mit der Anzahl, wie oft enthalten sind, die jedem Wort aufgetreten ist. Im folgenden finden ein Beispiel für die Daten, die in der Datei enthalten sein sollen:

        wreathed        3
        wreathing       1
        wreaths         1
        wrecked         3
        wrenching       1
        wretched        6
        wriggling       1

##<a name="a-idsummaryasummary"></a><a id="summary"></a>Zusammenfassung

Wie Sie sehen können, bietet es sich bei der Hadoop Befehl um eine einfache Möglichkeit, eine HDInsight Cluster MapReduce Aufträge ausgeführt, und zeigen Sie dann die Ausgabe der Position.

##<a name="a-idnextstepsanext-steps"></a><a id="nextsteps"></a>Nächste Schritte

Allgemeine Informationen zu MapReduce Aufträge in HDInsight:

* [Verwenden von MapReduce auf HDInsight Hadoop](hdinsight-use-mapreduce.md)

Weitere Informationen zu anderen Verfahren können Sie mit Hadoop auf HDInsight arbeiten:

* [Verwenden Sie die Struktur mit Hadoop auf HDInsight](hdinsight-use-hive.md)

* [Schwein mit Hadoop auf HDInsight verwenden](hdinsight-use-pig.md)

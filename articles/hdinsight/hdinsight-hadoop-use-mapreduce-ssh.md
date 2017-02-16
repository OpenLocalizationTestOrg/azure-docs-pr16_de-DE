<properties
   pageTitle="MapReduce und SSH Verbindung mit Hadoop in HDInsight | Microsoft Azure"
   description="Erfahren Sie, wie SSH MapReduce Aufträge mit Hadoop auf HDInsight ausgeführt werden."
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
   ms.date="08/23/2016"
   ms.author="larryfr"/>

# <a name="use-mapreduce-with-hadoop-on-hdinsight-with-ssh"></a>Verwenden von MapReduce mit Hadoop auf HDInsight mit SSH

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

In diesem Artikel erfahren Sie, wie Secure Shell (SSH) zum Verbinden mit einem Hadoop auf HDInsight Cluster und senden dann mithilfe der Befehle Hadoop MapReduce Aufträge verwendet werden.

> [AZURE.NOTE] Wenn Sie bereits mit der Verwendung von Linux-basierten Hadoop-Servern vertraut sind, aber Sie neu bei HDInsight sind, finden Sie unter [Tipps Linux-basierten HDInsight](hdinsight-hadoop-linux-information.md).

##<a name="a-idprereqaprerequisites"></a><a id="prereq"></a>Erforderliche Komponenten

Um die Schritte in diesem Artikel ausführen zu können, benötigen Sie Folgendes:

* Ein Cluster Linux-basierten HDInsight (Hadoop auf HDInsight)

* SSH-Client. Linux, Unix und Mac-Betriebssysteme sollten im Zusammenhang mit einem SSH-Client. Windows-Benutzer müssen einen Client, z. B. [kitten](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)herunterladen.

##<a name="a-idsshaconnect-with-ssh"></a><a id="ssh"></a>Verbinden mit SSH

Schließen Sie an den vollqualifizierten Domänennamen (FULLY) von Ihren Cluster HDInsight mithilfe des Befehls SSH. Der vollqualifizierten Domänennamen werden auf dem Namen den Cluster, gefolgt von zugewiesen **. azurehdinsight.net**. Beispielsweise würde Folgendes zu einem Cluster mit dem Namen **Myhdinsight**verbinden:

    ssh admin@myhdinsight-ssh.azurehdinsight.net

**Wenn Sie einen Zertifikatschlüssel für die Authentifizierung SSH bereitgestellt** , wenn Sie den HDInsight Cluster erstellt haben, müssen Sie den Speicherort der privaten Schlüssel beispielsweise auf Ihrem Clientsystem anzugeben:

    ssh -i ~/mykey.key admin@myhdinsight-ssh.azurehdinsight.net

**Wenn Sie ein Kennwort für die Authentifizierung SSH angegeben** , wenn Sie den HDInsight Cluster erstellt haben, müssen Sie das Kennwort ein bereitstellen.

Weitere Informationen zum Verwenden von SSH mit HDInsight finden Sie unter [Verwenden SSH mit Linux-basierten Hadoop auf HDInsight Linux, OS X und Unix](hdinsight-hadoop-linux-use-ssh-unix.md).

###<a name="putty-windows-clients"></a>Kitten (Windows-Clients)

Windows bietet keinen integrierten SSH-Client. Es empfiehlt sich, mit **kitten**, die von [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)heruntergeladen werden kann.

Weitere Informationen zur Verwendung von kitten finden Sie unter [Verwenden SSH mit Linux-basierten Hadoop auf HDInsight von Windows ](hdinsight-hadoop-linux-use-ssh-windows.md).

##<a name="a-idhadoopause-hadoop-commands"></a><a id="hadoop"></a>Hadoop Befehle verwenden

1. Nachdem Sie mit dem HDInsight Cluster verbunden sind, verwenden Sie den folgenden Befehl aus **Hadoop** zum Starten eines Auftrags MapReduce:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount wasb:///example/data/gutenberg/davinci.txt wasb:///example/data/WordCountOutput

    Hierdurch wird die **Wordcount** -Klasse, die in der Datei **Hadoop-Mapreduce-examples.jar** enthalten ist. Als Eingabe, verwendet sie das Dokument **wasbs://example/data/gutenberg/davinci.txt** und Ausgabe wird am Speicherorten **Wasbs: / / / Beispiel/Daten/WordCountOutput**.

    > [AZURE.NOTE] Weitere Informationen zu diesen Auftrag MapReduce und den Beispieldaten finden Sie unter [Verwenden von MapReduce in Hadoop auf HDInsight](hdinsight-use-mapreduce.md).

2. Der Auftrag gibt Details aus, wie es verarbeitet, und es Informationen ähnlich wie der folgende gibt bei Auftragsabschluss:

        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623

3. Wenn der Auftrag abgeschlossen ist, verwenden Sie den folgenden Befehl die Ausgabedateien aufgelistet, die bei **Wasbs://example/data/WordCountOutput**gespeichert werden:

        hdfs dfs -ls wasbs:///example/data/WordCountOutput

    Dadurch sollte die beiden Dateien **_SUCCESS** und **Webpart-R-00000**angezeigt. Die **Webpart-R-00000** -Datei enthält die Ausgabe für dieses Projekt.

    > [AZURE.NOTE] Einige MapReduce Einzelvorgänge können die Ergebnisse auf mehrere **Webparts-R-###** Dateien aufgeteilt. Wenn dies der Fall ist, verwenden Sie die ### Suffix an, dass die Reihenfolge der Dateien.

4. Wenn die Ausgabe die anzeigen möchten, verwenden Sie den folgenden Befehl aus:

        hdfs dfs -cat wasbs:///example/data/WordCountOutput/part-r-00000

    Dies zeigt eine Liste der Wörter, die in der Datei **wasbs://example/data/gutenberg/davinci.txt** und die Anzahl der Häufigkeit jedes Worts enthalten sind. Im folgenden finden ein Beispiel für die Daten, die in der Datei enthalten sein sollen:

        wreathed        3
        wreathing       1
        wreaths         1
        wrecked         3
        wrenching       1
        wretched        6
        wriggling       1

##<a name="a-idsummaryasummary"></a><a id="summary"></a>Zusammenfassung

Wie Sie sehen können, bieten Hadoop Befehle MapReduce Aufträge in einem Cluster HDInsight ausführen, und zeigen Sie dann die Ausgabe der Position auf einfache Weise an.

##<a name="a-idnextstepsanext-steps"></a><a id="nextsteps"></a>Nächste Schritte

Allgemeine Informationen zu MapReduce Aufträge in HDInsight:

* [Verwenden von MapReduce auf HDInsight Hadoop](hdinsight-use-mapreduce.md)

Weitere Informationen zu anderen Verfahren können Sie mit Hadoop auf HDInsight arbeiten:

* [Verwenden Sie die Struktur mit Hadoop auf HDInsight](hdinsight-use-hive.md)

* [Schwein mit Hadoop auf HDInsight verwenden](hdinsight-use-pig.md)

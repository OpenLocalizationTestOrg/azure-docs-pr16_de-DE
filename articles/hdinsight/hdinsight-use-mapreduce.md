<properties
   pageTitle="MapReduce mit Hadoop auf HDInsight | Microsoft Azure"
   description="Erfahren Sie, wie auf HDInsight Cluster MapReduce Aufträge auf Hadoop ausgeführt werden. Sie erhalten eine grundlegende Word zählen Vorgangs als Java MapReduce Auftrag implementiert ausführen."
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

# <a name="use-mapreduce-in-hadoop-on-hdinsight"></a>Verwenden von MapReduce in Hadoop auf HDInsight

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

In diesem Artikel erfahren Sie, wie auf HDInsight Cluster MapReduce Aufträge auf Hadoop ausgeführt werden. Wir führen Sie eine grundlegende Word zählen Vorgangs als Java MapReduce Auftrag implementiert.

##<a name="a-idwhatisawhat-is-mapreduce"></a><a id="whatis"></a>Was ist MapReduce?

Hadoop MapReduce ist ein Softwareframework für das Schreiben von Aufträgen, die große Datenmengen verarbeiten. Eingabedaten unterteilt unabhängigen Abschnitte, die dann über den Knoten in Ihrem Cluster parallel verarbeitet werden. Ein Auftrag MapReduce bestehen aus zwei Funktionen:

* **Mapper**: Es verbraucht Eingabedaten, analysiert (normalerweise mit Filtern und Sortieren von Vorgängen) und gibt Tupel (Schlüssel / Wert-Paare)
* **Reduzierstücks**: Es verbraucht Tupel von der Zuordnung ausgegeben und führt eine Zusammenfassung aus, die eine kleinere, kombinierte Ergebnis aus den Mapper Daten erstellt wird

Ein einfaches Word Count MapReduce Auftrag Beispiel wird in der folgenden Abbildung dargestellt:

![HDI. WordCountDiagram][image-hdi-wordcountdiagram]

Die Ausgabe dieses Auftrags ist die Anzahl der wie oft jedes Worts im Text aufgetreten ist, die analysiert wurde.

* Der Zuordnung jeder Zeile aus dem eingegebenen Text als Eingabe akzeptiert und teilt ihn auf Wörter. Es gibt eine Schlüsselwert Paar jedes Mal ein Wort tritt auf, der das Wort wird gefolgt von 1. Die Ausgabe ist vor dem Senden an Reduzierstücks sortiert.

* Die Übergangsstück summiert diese einzelne zählt für jedes Wort und gibt ein paar Einzelwert Schlüssel, die das Wort, gefolgt von der Summe der Vorkommen enthält.

MapReduce kann in einer Vielzahl von Sprachen implementiert werden. Java ist die am häufigsten verwendeten Implementierung und Demo Zwecken in diesem Dokument verwendet wird.

### <a name="hadoop-streaming"></a>Hadoop Streaming

Sprachen oder Framework, die auf Java und die Java-virtuellen Computern (z. B. Scalding oder verknüpften,) basieren können werden direkt als MapReduce vornehmen, ähnlich wie eine Java-Anwendung ist. Andere, müssen z. B. c# oder Python oder eigenständigen Programmdateien, Hadoop streaming verwenden.

Hadoop streaming kommuniziert mit den Mapper und Reduzierstücks über STDIN und STDOUT - die Mapper Reduzierstücks Daten von STDIN jeweils eine Zeile gelesen und die Ausgabe an STDOUT. Jede Zeile lesen oder die Mapper und Reduzierstücks ausgegeben muss in das Format des Schlüssel/Wert-Paar, durch eine Registerkarte Charaacter getrennt werden:

    [key]/t[value]

Weitere Informationen finden Sie unter [Hadoop Streaming](http://hadoop.apache.org/docs/r1.2.1/streaming.html).

Beispiele für die Verwendung mit HDInsight streaming Hadoop finden Sie hier:

* [Entwickeln Sie Python MapReduce Aufträge](hdinsight-hadoop-streaming-python.md)

##<a name="a-iddataaabout-the-sample-data"></a><a id="data"></a>Zu den Beispieldaten

In diesem Beispiel wird Sie für die Beispieldaten, die Notizbücher von Leonardo Da Vinci, verwendet die eines Textdokuments in Ihren Cluster HDInsight bereitgestellt werden.

Die Beispieldaten werden in Azure Blob-Speicher gespeichert, die HDInsight als Standard-Dateisystem für Hadoop Cluster verwendet. HDInsight kann mit dem Präfix **Wasb** Blob-Speicher gespeicherte Dateien zugreifen. Zugriff auf die Datei sample.log verwenden Sie beispielsweise die folgende Syntax:

    wasbs:///example/data/gutenberg/davinci.txt

Da Azure Blob-Speicher der Standardspeicher für HDInsight ist, können Sie die Datei auch mithilfe von **/example/data/gutenberg/davinci.txt**zugreifen.

> [AZURE.NOTE] In der vorherigen Syntax **Wasbs: / / /** wird verwendet, um den Zugriff auf Dateien im Standard-Speicher für Ihren Cluster HDInsight Container gespeichert sind. Wenn Sie zusätzlichen Speicherkonten angegeben, wenn Sie nach der Bereitstellung Ihrer Clusters und Zugriff auf Dateien, die in diese Konten gespeichert werden soll, können Sie die Daten durch Angabe den Adresse Container Name und Speicher Konto zugreifen. Beispielsweise **wasbs://mycontainer@mystorage.blob.core.windows.net/example/data/gutenberg/davinci.txt**.

##<a name="a-idjobaabout-the-example-mapreduce"></a><a id="job"></a>Zum Beispiel MapReduce

Der MapReduce Auftrag, der in diesem Beispiel verwendet wird, befindet sich unter **wasbs://example/jars/hadoop-mapreduce-examples.jar**und erfolgt mit Ihren Cluster HDInsight. Diese Datei enthält ein Beispiel für Word-zählen, die für **davinci.txt**ausgeführt wird.

> [AZURE.NOTE] Auf einem Cluster HDInsight 2.1 ist den Pfad der Datei **wasbs:///example/jars/hadoop-examples.jar**.

Als Referenz sind folgende Java-Code für das Word Count MapReduce Projekt:

    package org.apache.hadoop.examples;

    import java.io.IOException;
    import java.util.StringTokenizer;

    import org.apache.hadoop.conf.Configuration;
    import org.apache.hadoop.fs.Path;
    import org.apache.hadoop.io.IntWritable;
    import org.apache.hadoop.io.Text;
    import org.apache.hadoop.mapreduce.Job;
    import org.apache.hadoop.mapreduce.Mapper;
    import org.apache.hadoop.mapreduce.Reducer;
    import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
    import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;
    import org.apache.hadoop.util.GenericOptionsParser;

    public class WordCount {

      public static class TokenizerMapper
           extends Mapper<Object, Text, Text, IntWritable>{

        private final static IntWritable one = new IntWritable(1);
        private Text word = new Text();

        public void map(Object key, Text value, Context context
                        ) throws IOException, InterruptedException {
          StringTokenizer itr = new StringTokenizer(value.toString());
          while (itr.hasMoreTokens()) {
            word.set(itr.nextToken());
            context.write(word, one);
          }
        }
      }

      public static class IntSumReducer
           extends Reducer<Text,IntWritable,Text,IntWritable> {
        private IntWritable result = new IntWritable();

        public void reduce(Text key, Iterable<IntWritable> values,
                           Context context
                           ) throws IOException, InterruptedException {
          int sum = 0;
          for (IntWritable val : values) {
            sum += val.get();
          }
          result.set(sum);
          context.write(key, result);
        }
      }

      public static void main(String[] args) throws Exception {
        Configuration conf = new Configuration();
        String[] otherArgs = new GenericOptionsParser(conf, args).getRemainingArgs();
        if (otherArgs.length != 2) {
          System.err.println("Usage: wordcount <in> <out>");
          System.exit(2);
        }
        Job job = new Job(conf, "word count");
        job.setJarByClass(WordCount.class);
        job.setMapperClass(TokenizerMapper.class);
        job.setCombinerClass(IntSumReducer.class);
        job.setReducerClass(IntSumReducer.class);
        job.setOutputKeyClass(Text.class);
        job.setOutputValueClass(IntWritable.class);
        FileInputFormat.addInputPath(job, new Path(otherArgs[0]));
        FileOutputFormat.setOutputPath(job, new Path(otherArgs[1]));
        System.exit(job.waitForCompletion(true) ? 0 : 1);
      }
    }

Anweisungen, um eigene MapReduce Auftrag schreiben finden Sie unter [entwickeln Java MapReduce-Programmen für HDInsight](hdinsight-develop-deploy-java-mapreduce-linux.md).

##<a name="a-idrunarun-the-mapreduce"></a><a id="run"></a>Führen Sie die MapReduce

HDInsight kann HiveQL Aufträge mithilfe verschiedener Methoden ausführen. Anhand der folgenden Tabelle entscheiden, welche Methode für Sie richtig ist, und klicken Sie dann auf den Link für eine exemplarische Vorgehensweise.

| **Verwenden Sie diese Option**...                                                    | **... Aktion in hierher ziehen**                                       | ... .with dieses **Cluster-Betriebssystem** | ... .from dieses **Client-Betriebssystem** |
|:-------------------------------------------------------------------|:--------------------------------------------------------|:------------------------------------------|:-----------------------------------------|
| [SSH](hdinsight-hadoop-use-mapreduce-ssh.md)                       | Verwenden Sie den Befehl Hadoop über **SSH**                  | Linux                                     | Linux, Unix, Mac OS X oder Windows        |
| [Aufrollen](hdinsight-hadoop-use-mapreduce-curl.md)                     | Senden Sie den Auftrag Remote mithilfe von **REST**               | Linux oder Windows                          | Linux, Unix, Mac OS X oder Windows        |
| [Windows PowerShell](hdinsight-hadoop-use-mapreduce-powershell.md) | Senden Sie den Auftrag Remote mithilfe von **Windows PowerShell** | Linux oder Windows                          | Windows                                  |
| [Remotedesktop](hdinsight-hadoop-use-mapreduce-remote-desktop)    | Verwenden Sie den Befehl Hadoop über **Remote Desktop**       | Windows                                   | Windows                                  |

##<a name="a-idnextstepsanext-steps"></a><a id="nextsteps"></a>Nächste Schritte

Obwohl MapReduce leistungsfähige diagnostische Funktionen bereitstellt, kann es ein bisschen Master-Shape so schwierig sein. Es gibt verschiedene Java-basierte Framework, die zum Definieren von Applications MapReduce sowie Technologien wie Schwein und Struktur, die ein einfacheres Verfahren zum Arbeiten mit Daten in HDInsight bereitstellen zu erleichtern. Weitere Informationen finden Sie unter den folgenden Artikeln:

* [Entwickeln Sie MapReduce Java-Programme für HDInsight](hdinsight-develop-deploy-java-mapreduce-linux.md)

* [Entwickeln Sie Python streaming MapReduce Programme für HDInsight](hdinsight-hadoop-streaming-python.md)

* [Entwickeln Sie Scalding MapReduce Aufträge mit Apache Hadoop auf HDInsight](hdinsight-hadoop-mapreduce-scalding.md)

* [Verwenden Sie die Struktur mit HDInsight][hdinsight-use-hive]

* [Schwein mit HDInsight verwenden][hdinsight-use-pig]

* [Führen Sie die Beispiele HDInsight][hdinsight-samples]


[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-develop-mapreduce-jobs]: hdinsight-develop-deploy-java-mapreduce-linux.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-samples]: hdinsight-run-samples.md
[hdinsight-provision]: hdinsight-provision-clusters.md

[powershell-install-configure]: ../powershell-install-configure.md

[image-hdi-wordcountdiagram]: ./media/hdinsight-use-mapreduce/HDI.WordCountDiagram.gif

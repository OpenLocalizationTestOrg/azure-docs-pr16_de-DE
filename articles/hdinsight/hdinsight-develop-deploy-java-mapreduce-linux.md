<properties
    pageTitle="Entwickeln Sie MapReduce Java-Programme für Linux-basierte HDInsight | Microsoft Azure"
    description="Informationen Sie zum Entwickeln MapReduce Java-Programme und auf Linux-basierten HDInsight bereitstellen."
    services="hdinsight"
    editor="cgronlun"
    manager="jhubbard"
    authors="Blackmist"
    documentationCenter=""
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="10/11/2016"
    ms.author="larryfr"/>

# <a name="develop-java-mapreduce-programs-for-hadoop-on-hdinsight-linux"></a>Entwickeln Sie MapReduce Java-Programme für Hadoop auf HDInsight Linux

Diese Dokumente führt Sie durch die Verwendung von Apache Maven zum Erstellen einer MapReduce-Anwendung, und klicken Sie dann bereitstellen, und führen Sie es auf einem Linux-basierten Hadoop auf HDInsight Cluster.

##<a name="a-nameprerequisitesaprerequisites"></a><a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie dieses Lernprogramm beginnen, benötigen Sie Folgendes:

- [Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/) 7 oder höher (oder einer ähnlichen, z. B. OpenJDK)

- [Apache Maven](http://maven.apache.org/)

- **Ein Azure-Abonnement**

- **Azure CLI**

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

##<a name="configure-environment-variables"></a>Konfigurieren der Umgebungsvariablen

Die folgenden Umgebungsvariablen möglicherweise beim Installieren von Java und JDK festgelegt werden. Sie sollten, dass sie vorhanden sind und die richtigen Werte für Ihr System enthalten.

* **JAVA_HOME** - sollte zeigen, zu dem Verzeichnis, in dem die Java Runtime-Umgebung (JRE) installiert ist. Angenommen, in einem OS X, Unix oder Linux-System, es sollte einen Wert enthalten ähnliche `/usr/lib/jvm/java-7-oracle`. Unter Windows möchten sie einen Wert ähnlich wie haben.`c:\Program Files (x86)\Java\jre1.7`

* **PATH** - sollte die folgenden Pfade enthalten:

    * **JAVA_HOME** (oder die entsprechende Pfadangabe)

    * **JAVA_HOME\bin** (oder die entsprechende Pfadangabe)

    * Das Verzeichnis, in dem Maven installiert ist

##<a name="create-a-new-maven-project"></a>Erstellen eines neuen Projekts von Maven

1. Aus einer terminal Sitzung oder in Ihrer Entwicklungsumgebung Befehlszeile wechseln Sie zu dem Speicherort, den dieses Projekt gespeichert werden sollen.

3. Verwenden Sie den Befehl __Mvn__ , die mit Maven installiert wird, um das Gerüst für das Projekt zu generieren.

        mvn archetype:generate -DgroupId=org.apache.hadoop.examples -DartifactId=wordcountjava -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    Dies wird Erstellen eines neuen Verzeichnisses im aktuellen Verzeichnis, mit dem Namen angegeben haben, indem Sie den Parameter __ArtifactID__ (**Wordcountjava** in diesem Beispiel). Dieses Verzeichnis enthält die folgenden Elemente:

    * __pom.xml__ – das [Projekt Objekt Modell (POM)](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html) , die enthält Informationen und Konfiguration Details zum Erstellen des Projekts verwendet.

    * __Src__ - Verzeichnis, die das Verzeichnis __Primär/Java/Organigramm/Apache/Hadoop/Beispiele__ enthält, in dem Sie die Anwendung erstellen werden.

3. Löschen Sie die Datei __src/test/java/org/apache/hadoop/examples/apptest.java__ , wie sie in diesem Beispiel nicht verwendet werden.

##<a name="add-dependencies"></a>Hinzufügen von Abhängigkeiten

1. Bearbeiten Sie die Datei __pom.xml__ , und fügen Sie den folgenden innerhalb der `<dependencies>` Abschnitt:

        <dependency>
          <groupId>org.apache.hadoop</groupId>
          <artifactId>hadoop-mapreduce-examples</artifactId>
          <version>2.5.1</version>
          <scope>provided</scope>
        </dependency>
        <dependency>
          <groupId>org.apache.hadoop</groupId>
          <artifactId>hadoop-mapreduce-client-common</artifactId>
          <version>2.5.1</version>
          <scope>provided</scope>
        </dependency>
        <dependency>
          <groupId>org.apache.hadoop</groupId>
          <artifactId>hadoop-common</artifactId>
          <version>2.5.1</version>
          <scope>provided</scope>
        </dependency>

    Dies weist Maven, dass das Projekt die Bibliotheken erfordert (aufgeführt in &lt;ArtifactId\>) mit einer bestimmten Version (aufgeführt, die in &lt;Version\>). Zum Zeitpunkt der Kompilierung wird dies von Maven Standard-Repository heruntergeladen werden. Sie können die [Maven Repository Suche](http://search.maven.org/#artifactdetails%7Corg.apache.hadoop%7Chadoop-mapreduce-examples%7C2.5.1%7Cjar) verwenden, um weitere anzuzeigen.

    Die `<scope>provided</scope>` Maven, dass diese Abhängigkeiten nicht soll, können Sie mit der Anwendung verpackt werden, erfahren, wie sie vom HDInsight Cluster zur Laufzeit bereitgestellt werden.

2. Fügen Sie zu der Datei __pom.xml__ folgenden ein. Dies muss innerhalb der `<project>...</project>` Tags in der Datei; beispielsweise zwischen `</dependencies>` und `</project>`.

        <build>
          <plugins>
            <plugin>
              <groupId>org.apache.maven.plugins</groupId>
              <artifactId>maven-shade-plugin</artifactId>
              <version>2.3</version>
              <configuration>
                <transformers>
                  <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                  </transformer>
                </transformers>
              </configuration>
              <executions>
                <execution>
                  <phase>package</phase>
                    <goals>
                      <goal>shade</goal>
                    </goals>
                </execution>
              </executions>
            </plugin>
            <plugin>
              <groupId>org.apache.maven.plugins</groupId>
              <artifactId>maven-compiler-plugin</artifactId>
              <configuration>
               <source>1.7</source>
               <target>1.7</target>
              </configuration>
            </plugin>
          </plugins>
        </build>

    Das erste Plug-in konfiguriert [Maven schattieren-Plug-Ins](http://maven.apache.org/plugins/maven-shade-plugin/), die zur Erstellung von einer Uberjar (manchmal als eine Fatjar bezeichnet), die Abhängigkeiten von der Anwendung benötigte enthält verwendet wird. Es wird verhindert, dass Kopien von Lizenzen innerhalb des Pakets JAR-Datei, die auf einigen Systemen Probleme verursachen können.

    Das zweite Plug-in konfiguriert den Compiler Maven, die zum Einrichten der Version von Java erforderlich, die auf dem HDInsight Cluster verwendete Version der Anwendung verwendet wird.

3. Speichern Sie die Datei __pom.xml__ .

##<a name="create-the-mapreduce-application"></a>Erstellen Sie die Anwendung MapReduce

1. Wechseln Sie zu dem Verzeichnis __Wordcountjava/Src/primär/Java/Organigramm/Apache/Hadoop/Beispiele__ , und benennen Sie die Datei __App.java__ in __WordCount.java__.

2. Öffnen Sie die __WordCount.java__ -Datei in einem Text-Editor, und Ersetzen Sie den Inhalt mit den folgenden:

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

    Beachten Sie der Paketnamen ist **org.apache.hadoop.examples** und der Klassennamen **WordCount**. Wenn Sie den Auftrag MapReduce Absenden, wird Sie diese Namen verwendet.

3. Speichern Sie die Datei ein.

##<a name="build-the-application"></a>Erstellen Sie die Anwendung

1. Wechseln Sie zum Verzeichnis __Wordcountjava__ , wenn Sie nicht bereits vorhanden sind.

2. Verwenden Sie den folgenden Befehl zur Erstellung einer JAR-Datei mit der Anwendung:

        mvn clean package

    Dies wird alle vorherigen Build Elemente bereinigen, Abhängigkeiten, die nicht bereits installiert wurden, und klicken Sie dann erstellen und Packen Sie die Anwendung herunterladen.

3. Nachdem Sie der Befehl abgeschlossen ist, wird Verzeichnis __Wordcountjava/Ziel__ eine Datei namens __Wordcountjava-1.0-SNAPSHOT.jar__enthalten.

    > [AZURE.NOTE] Die Datei __Wordcountjava-1.0-SNAPSHOT.jar__ ist eine Uberjar, die nicht nur die WordCount Position, sondern auch Abhängigkeiten, die der Auftrag zur Laufzeit erfordert enthält.


##<a name="a-iduploadaupload-the-jar"></a><a id="upload"></a>Hochladen der jar

Verwenden Sie den folgenden Befehl aus, um JAR-Datei in die HDInsight Headnode hochladen:

    scp wordcountjava-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:

    Replace __USERNAME__ with your SSH user name for the cluster. Replace __CLUSTERNAME__ with the HDInsight cluster name.

Dadurch wird die Dateien aus dem lokalen System zum am Knoten kopiert.

> [AZURE.NOTE] Wenn Sie ein Kennwort verwendet, um Ihr Konto SSH zu sichern, werden Sie aufgefordert, das Kennwort anzugeben. Wenn Sie einen Schlüssel SSH verwendet haben, müssen Sie möglicherweise verwenden Sie die `-i` Parameter und den Pfad für den privaten Schlüssel. Beispielsweise `scp -i /path/to/private/key wordcountjava-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:`.

##<a name="a-namerunarun-the-mapreduce-job"></a><a name="run"></a>Führen Sie die Stapelverarbeitung MapReduce

1. Verbinden Sie mit HDInsight mithilfe von SSH wie in den folgenden Artikeln beschrieben:

    - [Verwenden von SSH mit Linux-basierten Hadoop auf HDInsight von Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    - [Verwenden von SSH mit Linux-basierten Hadoop auf HDInsight von Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

2. Verwenden Sie den folgenden Befehl aus der Sitzung SSH zum Ausführen der Anwendung MapReduce:

        yarn jar wordcountjava.jar org.apache.hadoop.examples.WordCount wasbs:///example/data/gutenberg/davinci.txt wasbs:///example/data/wordcountout

    Dies mithilfe der Anwendung WordCount MapReduce zählen der Wörter in der Datei davinci.txt, und speichern Sie die Ergebnisse auf __Wasbs: / / / Beispiel/Daten/Wordcountout__. Die Eingabe- und die Ausgabe werden auf den Standardspeicher für den Cluster gespeichert.

3. Nachdem Sie der Auftrag abgeschlossen ist, verwenden Sie folgende zum Anzeigen der Ergebnisse aus:

        hdfs dfs -cat wasbs:///example/data/wordcountout/*

    Sie sollten eine Liste der Wörter und zählt, mit Werten ähnlich wie der folgende angezeigt:

        zeal    1
        zelus   1
        zenith  2

##<a name="a-idnextstepsanext-steps"></a><a id="nextsteps"></a>Nächste Schritte

In diesem Dokument haben Sie gelernt ein Auftrags Java MapReduce entwickeln möchte. Die folgenden Dokumente für andere Methoden für die Arbeit mit HDInsight finden Sie unter.

- [Verwenden Sie die Struktur mit HDInsight][hdinsight-use-hive]
- [Schwein mit HDInsight verwenden][hdinsight-use-pig]
- [Verwenden von MapReduce mit HDInsight](hdinsight-use-mapreduce.md)

Weitere Informationen finden Sie auch im [Java Developer Center](https://azure.microsoft.com/develop/java/).

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-ODBC]: hdinsight-connect-excel-hive-ODBC-driver.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md

[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md

[powershell-PSCredential]: http://social.technet.microsoft.com/wiki/contents/articles/4546.working-with-passwords-secure-strings-and-credentials-in-windows-powershell.aspx


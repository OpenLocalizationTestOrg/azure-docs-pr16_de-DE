<properties
   pageTitle="Entwickeln Sie Java-basierte Topologien für Apache Storm | Microsoft Azure"
   description="Informationen Sie zum Erstellen von Storm Topologien in Java durch Erstellen einer einfachen Word Count Suchtopologie."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/14/2016"
   ms.author="larryfr"/>

#<a name="develop-java-based-topologies-for-a-basic-word-count-application-with-apache-storm-and-maven-on-hdinsight"></a>Entwickeln Sie Java-basierte Topologien für eine Anwendung grundlegende Wortanzahl mit Apache Storm und Maven auf HDInsight

Erfahren Sie, wie Sie ein Suchtopologie Java-basierte für Apache Storm auf HDInsight mithilfe von Maven zu erstellen. Sie werden durch das Verfahren zum Erstellen einer einfachen Wortanzahl Anwendung mithilfe von Maven und Java, wo der Suchtopologie in Java definiert ist geführt. Klicken Sie dann erfahren Sie, wie der Suchtopologie mit Wärmefluss Framework definiert.

> [AZURE.NOTE] Das Framework Wärmefluss ist oder höher Storm 0.10.0 verfügbar. Storm 0.10.0 ist HDInsight 3.3 und 3.4 erhältlich.

Nachdem Sie die Schritte in diesem Dokument, haben Sie eine einfache Suchtopologie, die auf Apache Storm auf HDInsight bereitgestellt werden kann.

> [AZURE.NOTE] Eine fertige Version Topologien erstellt in diesem Dokument steht unter [https://github.com/Azure-Samples/hdinsight-java-storm-wordcount](https://github.com/Azure-Samples/hdinsight-java-storm-wordcount)zur Verfügung.

##<a name="prerequisites"></a>Erforderliche Komponenten

* <a href="https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html" target="_blank">Java Developer Kit (JDK) Version 7</a>

* <a href="https://maven.apache.org/download.cgi" target="_blank">Maven</a>: Maven ist ein Project-Generator-System zum Java-Projekte.

* Einen Text-Editor wie Editor, <a href="http://www.gnu.org/software/emacs/" target="_blank">Emacs<a>, <a href="http://www.sublimetext.com/" target="_blank">Sublime Text</a>, <a href="https://atom.io/" target="_blank">Atom.io</a>, <a href="http://brackets.io/" target="_blank">Brackets.io</a>. Oder Sie können eine integrierte Entwicklungsumgebung (IDE) wie <a href="https://eclipse.org/" target="_blank">"Ellipse"</a> (Version Luna oder höher).

    > [AZURE.NOTE] Der Editor oder IDE möglicherweise bestimmte Funktionen für die Arbeit mit Maven, die nicht in diesem Dokument adressiert ist. Informationen zu den Funktionen Ihrer Umgebung bearbeiten finden Sie in der Dokumentation für das Produkt, das Sie verwenden.

##<a name="configure-environment-variables"></a>Konfigurieren der Umgebungsvariablen

Die folgenden Umgebungsvariablen möglicherweise beim Installieren von Java und JDK festgelegt werden. Sie sollten, dass sie vorhanden sind und die richtigen Werte für Ihr System enthalten.

* **JAVA_HOME** - sollte zeigen, zu dem Verzeichnis, in dem die Java Runtime-Umgebung (JRE) installiert ist. Angenommen, in einer Verteilung Unix oder Linux es sollte einen Wert enthalten ähnliche `/usr/lib/jvm/java-7-oracle`. Unter Windows möchten sie einen Wert ähnlich wie haben.`c:\Program Files (x86)\Java\jre1.7`

* **PATH** - sollte die folgenden Pfade enthalten:

    * **JAVA_HOME** (oder die entsprechende Pfadangabe)

    * **JAVA_HOME\bin** (oder die entsprechende Pfadangabe)

    * Das Verzeichnis, in dem Maven installiert ist

##<a name="create-a-new-maven-project"></a>Erstellen eines neuen Projekts von Maven

Verwenden Sie den folgenden Code über die Befehlszeile um ein neues Maven Projekt mit dem Namen **WordCount**zu erstellen:

    mvn archetype:generate -DarchetypeArtifactId=maven-archetype-quickstart -DgroupId=com.microsoft.example -DartifactId=WordCount -DinteractiveMode=false

Dies erstellt ein neues Verzeichnis **WordCount** an der aktuellen Position aus, die ein grundlegendes Maven Projekt enthält.

Im Verzeichnis **WordCount** enthält die folgenden Elemente:

* **pom.XML**: die Einstellungen für das Projekt Maven enthält.

* **Src\main\java\com\microsoft\example**: Ihrer Anwendungscode enthält.

* **Src\test\java\com\microsoft\example**: Tests für die Anwendung enthält. In diesem Beispiel werden wir nicht erstellen, überprüft werden.

###<a name="remove-the-example-code"></a>Entfernen Sie den Beispielcode

Da wir unsere Anwendung erstellen, löschen Sie die generierten Test und Dateien der Anwendung:

*  **src\test\java\com\microsoft\example\AppTest.Java**

*  **src\main\java\com\microsoft\example\App.Java**

##<a name="add-properties"></a>Hinzufügen von Eigenschaften

Maven können Sie Eigenschaften auf Projektebene Wertemenge definieren. Fügen Sie Folgendes nach der `<url>http://maven.apache.org</url>` Zeile:

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <!--
        Storm 0.10.0 is for HDInsight 3.3 and 3.4.
        To find the version information for earlier HDInsight cluster
        versions, see https://azure.microsoft.com/en-us/documentation/articles/hdinsight-component-versioning/
        -->
        <storm.version>0.10.0</storm.version>
    </properties>

Wir können jetzt diese Werte in anderen Abschnitten verwenden. Beispielsweise, wenn Sie die Version von Storm Komponenten angeben, wir können `${storm.version}` anstelle ein Werts programmieren.

##<a name="add-dependencies"></a>Hinzufügen von Abhängigkeiten

Da es sich um eine Storm Suchtopologie handelt, müssen Sie eine Abhängigkeit für Storm Komponenten hinzufügen. Öffnen Sie die Datei **pom.xml** , und fügen Sie den folgenden Code in die ** &lt;Abhängigkeiten >** Abschnitt:

    <dependency>
      <groupId>org.apache.storm</groupId>
      <artifactId>storm-core</artifactId>
      <version>${storm.version}</version>
      <!-- keep storm out of the jar-with-dependencies -->
      <scope>provided</scope>
    </dependency>

Zum Zeitpunkt der Kompilierung verwendet Maven diese Informationen **Storm-Core** im Repository Maven nachzuschlagen. Es sieht so aus zuerst im Repository auf dem lokalen Computer. Wenn die Dateien nicht vorhanden sind, wird diese aus dem öffentlichen Maven Repository herunterladen und im lokalen Repository zu speichern.

> [AZURE.NOTE] Hinweis Die `<scope>provided</scope>` Zeile im Abschnitt wir hinzugefügt. Dies weist Maven ausgeschlossen werden **Storm-Core** JAR-Dateien, den Sie erstellen, da das System bereitgestellt wird. Dadurch wird die Pakete Sie erstellen, um ein wenig kleiner sein, und es wird sichergestellt, dass diese die **Storm-Core** Bits verwenden, die in der Storm auf HDInsight Cluster enthalten sind.

##<a name="build-configuration"></a>Erstellen der Konfiguration

Maven-Plug-ins können Sie zum Anpassen der Build Phasen des Projekts, wie etwa wie das Projekt kompiliert wird oder wie Sie es in eine JAR-Datei packen. Öffnen Sie die Datei **pom.xml** , und fügen Sie den folgenden Code direkt oberhalb der `</project>` Linie.

    <build>
      <plugins>
      </plugins>
      <resources>
      </resources>
    </build>

In diesem Abschnitt wird zum Hinzufügen von-Plug-ins, Ressourcen und anderen Build Konfigurationsoptionen verwendet werden. Eine vollständige Referenz der Datei __pom.xml__ finden Sie unter [http://maven.apache.org/pom.html](http://maven.apache.org/pom.html).

###<a name="add-plug-ins"></a>Hinzufügen von-Plug-ins

Für Storm Topologien eignet sich die <a href="http://mojo.codehaus.org/exec-maven-plugin/" target="_blank">Mitarbeiter Maven-Plug-in</a> , da es Ihnen ermöglicht, einfach die Suchtopologie lokal in Ihrer Entwicklungsumgebung ausführen. Fügen Sie die folgenden Optionen, um die `<plugins>` Abschnitt der Datei **pom.xml** der Mitarbeiter Maven-Plug-Ins aufnehmen möchten:

    <plugin>
      <groupId>org.codehaus.mojo</groupId>
      <artifactId>exec-maven-plugin</artifactId>
      <version>1.4.0</version>
      <executions>
        <execution>
        <goals>
          <goal>exec</goal>
        </goals>
        </execution>
      </executions>
      <configuration>
        <executable>java</executable>
        <includeProjectDependencies>true</includeProjectDependencies>
        <includePluginDependencies>false</includePluginDependencies>
        <classpathScope>compile</classpathScope>
        <mainClass>${storm.topology}</mainClass>
      </configuration>
    </plugin>

> [AZURE.NOTE] Beachten Sie, dass die `<mainClass>` Eintrag verwendet `${storm.topology}`. Wir haben dies zuvor im Abschnitt Eigenschaften definieren (aber haben wir.) Wir werden stattdessen diesen Wert über die Befehlszeile festlegen, wenn der Suchtopologie in Ihrer Entwicklungsumgebung in einem späteren Schritt ausführen.

Eine andere nützliche-Plug-in ist <a href="http://maven.apache.org/plugins/maven-compiler-plugin/" target="_blank">Apache Maven Compiler-Plug-Ins</a>, die zum Ändern von Kompilierungsoptionen für eine verwendet wird. Primäre deswegen Sinn hierin so ändern Sie die Java-Version, die Maven für Quell- und Zielwebsites für eine Anwendung verwendet. Wir wollen, Version 1.7.

Fügen Sie den folgenden in der `<plugins>` Abschnitt der Datei **pom.xml** eingeschlossen werden sollen die Apache Maven Compiler-Plug-in, und legen Sie die Quell- und Zielwebsites Versionen auf 1.7.

    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-compiler-plugin</artifactId>
      <version>3.3</version>
      <configuration>
        <source>1.7</source>
        <target>1.7</target>
      </configuration>
    </plugin>

###<a name="configure-resources"></a>Konfigurieren von Ressourcen

Im Ressourcenabschnitt ermöglicht das Einbeziehen von nicht-Code-Ressourcen, wie z. B. Konfigurationsdateien von Komponenten in der Suchtopologie erforderlich. In diesem Beispiel fügen Sie den folgenden in der `<resources>` Abschnitt der Datei **pom.xml** .

    <resource>
        <directory>${basedir}/resources</directory>
        <filtering>false</filtering>
        <includes>
          <include>log4j2.xml</include>
        </includes>
    </resource>

Diese Ressourcenverzeichnis im Stammverzeichnis des Projekts addiert (`${basedir}`) als einen Speicherort, die Ressourcen enthält, und die Datei mit dem Namen __log4j2.xml__enthält. Diese Datei wird verwendet, zu konfigurieren, welche Informationen von der Suchtopologie angemeldet ist.

##<a name="create-the-topology"></a>Erstellen der Suchtopologie

Eine Java-basierte Storm Suchtopologie besteht aus drei Komponenten, die Sie verfassen müssen (oder ein Verweis) als Abhängigkeit.

* **Spouts**: liest Daten aus externen Quellen, und gibt Streams von Daten in der Suchtopologie aus.

* **Bolts**: führt Verarbeitung auf Streams von Spouts oder anderen Schrauben ausgegeben und gibt eine oder mehrere Streams.

* **Suchtopologie**: definiert, wie die Spouts und Schrauben angeordnet sind, und stellt den Einstiegspunkt für die Suchtopologie bereit.

###<a name="create-the-spout"></a>Erstellen der Schnauze

Klicken Sie zum Verringern Anforderungen zum Einrichten von externen Datenquellen gibt die folgenden Schnauze einfach zufällige Sätze aus. Es ist eine geänderte Version einer Schnauze, die mit den [Storm Starter Beispiele](https://github.com/apache/storm/blob/0.10.x-branch/examples/storm-starter/src/jvm/storm/starter)bereitgestellt wird.

> [AZURE.NOTE] Ein Beispiel einer Schnauze, die aus einer externen Datenquelle liest, finden Sie in den folgenden Beispielen:
>
> * [TwitterSampleSPout](https://github.com/apache/storm/blob/0.10.x-branch/examples/storm-starter/src/jvm/storm/starter/spout/TwitterSampleSpout.java): ein Beispiel Schnauze, die aus Twitter liest
>
> * [Storm-Kafka](https://github.com/apache/storm/tree/0.10.x-branch/external/storm-kafka): ein, der aus Kafka liest Schnauze

Erstellen Sie eine neue Datei namens **RandomSentenceSpout.java** im Verzeichnis **Src\main\java\com\microsoft\example** Schnauze und verwenden Sie die folgenden als Inhalt:

    /**
     * Licensed to the Apache Software Foundation (ASF) under one
     * or more contributor license agreements.  See the NOTICE file
     * distributed with this work for additional information
     * regarding copyright ownership.  The ASF licenses this file
     * to you under the Apache License, Version 2.0 (the
     * "License"); you may not use this file except in compliance
     * with the License.  You may obtain a copy of the License at
     *
     * http://www.apache.org/licenses/LICENSE-2.0
     *
     * Unless required by applicable law or agreed to in writing, software
     * distributed under the License is distributed on an "AS IS" BASIS,
     * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
     * See the License for the specific language governing permissions and
     * limitations under the License.
     */

     /**
      * Original is available at https://github.com/apache/storm/blob/master/examples/storm-starter/src/jvm/storm/starter/spout/RandomSentenceSpout.java
      */

    package com.microsoft.example;

    import backtype.storm.spout.SpoutOutputCollector;
    import backtype.storm.task.TopologyContext;
    import backtype.storm.topology.OutputFieldsDeclarer;
    import backtype.storm.topology.base.BaseRichSpout;
    import backtype.storm.tuple.Fields;
    import backtype.storm.tuple.Values;
    import backtype.storm.utils.Utils;

    import java.util.Map;
    import java.util.Random;

    //This spout randomly emits sentences
    public class RandomSentenceSpout extends BaseRichSpout {
      //Collector used to emit output
      SpoutOutputCollector _collector;
      //Used to generate a random number
      Random _rand;

      //Open is called when an instance of the class is created
      @Override
      public void open(Map conf, TopologyContext context, SpoutOutputCollector collector) {
      //Set the instance collector to the one passed in
        _collector = collector;
        //For randomness
        _rand = new Random();
      }

      //Emit data to the stream
      @Override
      public void nextTuple() {
      //Sleep for a bit
        Utils.sleep(100);
        //The sentences that will be randomly emitted
        String[] sentences = new String[]{ "the cow jumped over the moon", "an apple a day keeps the doctor away",
            "four score and seven years ago", "snow white and the seven dwarfs", "i am at two with nature" };
        //Randomly pick a sentence
        String sentence = sentences[_rand.nextInt(sentences.length)];
        //Emit the sentence
        _collector.emit(new Values(sentence));
      }

      //Ack is not implemented since this is a basic example
      @Override
      public void ack(Object id) {
      }

      //Fail is not implemented since this is a basic example
      @Override
      public void fail(Object id) {
      }

      //Declare the output fields. In this case, an sentence
      @Override
      public void declareOutputFields(OutputFieldsDeclarer declarer) {
        declarer.declare(new Fields("sentence"));
      }
    }

Betrachten Sie wir den Kommentaren zu verstehen, wie diese Schnauze funktioniert durchlesen.

> [AZURE.NOTE] Obwohl diese Suchtopologie nur eine Schnauze verwendet wird, haben andere mehrere, die in der Suchtopologie aus verschiedenen Quellen Datenfeed.

###<a name="create-the-bolts"></a>Erstellen der Schrauben

Schrauben verarbeitet die Datenverarbeitung. Für diese Suchtopologie gibt es zwei Schrauben:

* **SplitSentence**: teilt Sätze von **RandomSentenceSpout** in einzelne Wörter ausgegeben.

* **WordCount**: zählt, wie oft jedes Worts aufgetreten.

> [AZURE.NOTE] Schrauben können Literal nichts, z. B. Berechnung, Beibehaltung oder ein Gespräch mit externen Komponenten ausführen.

Erstellen Sie neue Dateien, **SplitSentence.java** und **WordCount.Java** im Verzeichnis **Src\main\java\com\microsoft\example** . Verwenden Sie die folgenden als Inhalt für die Dateien ein:

**SplitSentence**

    package com.microsoft.example;

    import java.text.BreakIterator;

    import backtype.storm.topology.BasicOutputCollector;
    import backtype.storm.topology.OutputFieldsDeclarer;
    import backtype.storm.topology.base.BaseBasicBolt;
    import backtype.storm.tuple.Fields;
    import backtype.storm.tuple.Tuple;
    import backtype.storm.tuple.Values;

    //There are a variety of bolt types. In this case, we use BaseBasicBolt
    public class SplitSentence extends BaseBasicBolt {

      //Execute is called to process tuples
      @Override
      public void execute(Tuple tuple, BasicOutputCollector collector) {
        //Get the sentence content from the tuple
        String sentence = tuple.getString(0);
        //An iterator to get each word
        BreakIterator boundary=BreakIterator.getWordInstance();
        //Give the iterator the sentence
        boundary.setText(sentence);
        //Find the beginning first word
        int start=boundary.first();
        //Iterate over each word and emit it to the output stream
        for (int end=boundary.next(); end != BreakIterator.DONE; start=end, end=boundary.next()) {
          //get the word
          String word=sentence.substring(start,end);
          //If a word is whitespace characters, replace it with empty
          word=word.replaceAll("\\s+","");
          //if it's an actual word, emit it
          if (!word.equals("")) {
            collector.emit(new Values(word));
          }
        }
      }

      //Declare that emitted tuples will contain a word field
      @Override
      public void declareOutputFields(OutputFieldsDeclarer declarer) {
        declarer.declare(new Fields("word"));
      }
    }

**WordCount**

    package com.microsoft.example;

    import java.util.HashMap;
    import java.util.Map;
    import java.util.Iterator;

    import backtype.storm.Constants;
    import backtype.storm.topology.BasicOutputCollector;
    import backtype.storm.topology.OutputFieldsDeclarer;
    import backtype.storm.topology.base.BaseBasicBolt;
    import backtype.storm.tuple.Fields;
    import backtype.storm.tuple.Tuple;
    import backtype.storm.tuple.Values;
    import backtype.storm.Config;

    // For logging
    import org.apache.logging.log4j.Logger;
    import org.apache.logging.log4j.LogManager;

    //There are a variety of bolt types. In this case, we use BaseBasicBolt
    public class WordCount extends BaseBasicBolt {
        //Create logger for this class
        private static final Logger logger = LogManager.getLogger(WordCount.class);
        //For holding words and counts
        Map<String, Integer> counts = new HashMap<String, Integer>();
        //How often we emit a count of words
        private Integer emitFrequency;

        // Default constructor
        public WordCount() {
            emitFrequency=5; // Default to 60 seconds
        }

        // Constructor that sets emit frequency
        public WordCount(Integer frequency) {
            emitFrequency=frequency;
        }

        //Configure frequency of tick tuples for this bolt
        //This delivers a 'tick' tuple on a specific interval,
        //which is used to trigger certain actions
        @Override
        public Map<String, Object> getComponentConfiguration() {
            Config conf = new Config();
            conf.put(Config.TOPOLOGY_TICK_TUPLE_FREQ_SECS, emitFrequency);
            return conf;
        }

        //execute is called to process tuples
        @Override
        public void execute(Tuple tuple, BasicOutputCollector collector) {
            //If it's a tick tuple, emit all words and counts
            if(tuple.getSourceComponent().equals(Constants.SYSTEM_COMPONENT_ID)
                    && tuple.getSourceStreamId().equals(Constants.SYSTEM_TICK_STREAM_ID)) {
                for(String word : counts.keySet()) {
                    Integer count = counts.get(word);
                    collector.emit(new Values(word, count));
                    logger.info("Emitting a count of " + count + " for word " + word);
                }
            } else {
                //Get the word contents from the tuple
                String word = tuple.getString(0);
                //Have we counted any already?
                Integer count = counts.get(word);
                if (count == null)
                    count = 0;
                //Increment the count and store it
                count++;
                counts.put(word, count);
            }
        }

        //Declare that we will emit a tuple containing two fields; word and count
        @Override
        public void declareOutputFields(OutputFieldsDeclarer declarer) {
            declarer.declare(new Fields("word", "count"));
        }
    }

Betrachten Sie wir den Kommentaren zu verstehen, wie jede herstellt funktioniert durchlesen.

###<a name="define-the-topology"></a>Definieren der Suchtopologie

Der Suchtopologie bindet das Spouts und bolts zusammen in einem Diagramm, die definiert, wie Daten zwischen den Komponenten fließt. Darüber hinaus Parallelism hinweisen, die Storm beim Erstellen von Instanzen der Komponenten im Cluster verwendet.

Im folgenden finden ein einfaches Diagramm des Diagramms Komponenten für diese Suchtopologie.

![Diagram, in dem die Spouts und Schrauben Anordnung](./media/hdinsight-storm-develop-java-topology/wordcount-topology.png)

Zum Implementieren der Suchtopologie erstellen Sie eine neue Datei namens **WordCountTopology.java** im Verzeichnis **Src\main\java\com\microsoft\example** . Verwenden Sie die folgenden als Inhalt für die Datei ein:

    package com.microsoft.example;

    import backtype.storm.Config;
    import backtype.storm.LocalCluster;
    import backtype.storm.StormSubmitter;
    import backtype.storm.topology.TopologyBuilder;
    import backtype.storm.tuple.Fields;

    import com.microsoft.example.RandomSentenceSpout;

    public class WordCountTopology {

      //Entry point for the topology
      public static void main(String[] args) throws Exception {
      //Used to build the topology
        TopologyBuilder builder = new TopologyBuilder();
        //Add the spout, with a name of 'spout'
        //and parallelism hint of 5 executors
        builder.setSpout("spout", new RandomSentenceSpout(), 5);
        //Add the SplitSentence bolt, with a name of 'split'
        //and parallelism hint of 8 executors
        //shufflegrouping subscribes to the spout, and equally distributes
        //tuples (sentences) across instances of the SplitSentence bolt
        builder.setBolt("split", new SplitSentence(), 8).shuffleGrouping("spout");
        //Add the counter, with a name of 'count'
        //and parallelism hint of 12 executors
        //fieldsgrouping subscribes to the split bolt, and
        //ensures that the same word is sent to the same instance (group by field 'word')
        builder.setBolt("count", new WordCount(), 12).fieldsGrouping("split", new Fields("word"));

        //new configuration
        Config conf = new Config();
        //Set to false to disable debug information
        // when running in production mode.
        conf.setDebug(false);

        //If there are arguments, we are running on a cluster
        if (args != null && args.length > 0) {
          //parallelism hint to set the number of workers
          conf.setNumWorkers(3);
          //submit the topology
          StormSubmitter.submitTopology(args[0], conf, builder.createTopology());
        }
        //Otherwise, we are running locally
        else {
          //Cap the maximum number of executors that can be spawned
          //for a component to 3
          conf.setMaxTaskParallelism(3);
          //LocalCluster is used to run locally
          LocalCluster cluster = new LocalCluster();
          //submit the topology
          cluster.submitTopology("word-count", conf, builder.createTopology());
          //sleep
          Thread.sleep(10000);
          //shut down the cluster
          cluster.shutdown();
        }
      }
    }

Betrachten Sie wir durchlesen mit den Kommentaren zu verstehen, wie der Suchtopologie definiert ist, und klicken Sie dann auf den Cluster übermittelt.

###<a name="configure-logging"></a>Konfigurieren der Protokollierung

Storm verwendet Apache Log4j, um Informationen protokollieren. Wenn Sie die Protokollierung nicht konfiguriert ist, gibt der Suchtopologie zahlreiche Diagnoseinformationen, die schwer zu lesen sein können. Um zu steuern, welche angemeldet ist, erstellen Sie eine Datei namens __log4j2.xml__ im Verzeichnis __Ressourcen__ . Verwenden Sie die folgenden als den Inhalt der Datei ein.

    <?xml version="1.0" encoding="UTF-8"?>
    <Configuration>
    <Appenders>
        <Console name="STDOUT" target="SYSTEM_OUT">
            <PatternLayout pattern="%d{HH:mm:ss} [%t] %-5level %logger{36} - %msg%n"/>
        </Console>
    </Appenders>
    <Loggers>
        <Logger name="com.microsoft.example" level="trace" additivity="false">
            <AppenderRef ref="STDOUT"/>
        </Logger>
        <Root level="error">
            <Appender-Ref ref="STDOUT"/>
        </Root>
    </Loggers>
    </Configuration>

Dadurch wird eine neue Protokollierung für die Klasse __com.microsoft.example__ , wozu auch die Komponenten in diesem Beispiel Suchtopologie konfiguriert. Die Ebene wird auf Spur zur dieser Protokollierung, Erfassen von Protokollierungsinformationen, die von Komponenten in diesem Suchtopologie ausgegeben wird festgelegt. Wenn Sie über den Code für dieses Projekt wieder anschauen, sehen Sie sich, dass nur die WordCount.java Datei Protokollierung implementiert; Es wird die Anzahl der jedes Worts protokolliert.

Die `<Root level="error">` Abschnitt konfiguriert die Stammebene der Protokollierung (nicht in __com.microsoft.example__, alles) um nur Fehlerinformationen protokolliert.

> [AZURE.IMPORTANT] Während dies beim Testen einer Suchtopologie in Ihrer Entwicklungsumgebung protokollierten Informationen erheblich reduziert, wird es nicht alle gefertigt beim Ausführen auf einem Cluster Herstellung Debuggen Informationen entfernt. Um diese Informationen zu verringern, müssen Sie auch die für das Debuggen auf False wird in der Konfiguration zum Cluster übermittelten festlegen. Finden Sie im WordCountTopology.java-Code in diesem Dokument ein Beispiel für ein. 

Weitere Informationen zum Konfigurieren der Protokollierung für Log4j finden Sie unter [http://logging.apache.org/log4j/2.x/manual/configuration.html](http://logging.apache.org/log4j/2.x/manual/configuration.html).

> [AZURE.NOTE] Storm Version 0.10.0 verwendet Log4j 2.x. Ältere Versionen von Storm verwendet Log4j 1.x, die ein anderes Format für die bei der Konfiguration verwendet. Klicken Sie auf die ältere Konfiguration Informationen finden Sie unter [http://wiki.apache.org/logging-log4j/Log4jXmlFormat](http://wiki.apache.org/logging-log4j/Log4jXmlFormat).

##<a name="test-the-topology-locally"></a>Testen der Suchtopologie lokal

Nachdem Sie die Dateien gespeichert haben, verwenden Sie den folgenden Befehl aus der Suchtopologie lokal testen.

    mvn compile exec:java -Dstorm.topology=com.microsoft.example.WordCountTopology

Während der Ausführung, wird der Suchtopologie Startinformationen angezeigt. Und sie Linien wie die folgende angezeigt werden, wie Sätze von der Schnauze ausgegeben sind und die Schrauben verarbeiteten beginnt.

    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word snow
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word white
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 112 for word seven
    17:33:27 [Thread-16-count] INFO  com.microsoft.example.WordCount - Emitting a count of 195 for word the
    17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 113 for word and
    17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word dwarfs
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word snow

Anhand der Protokollierung von der WordCount herstellt ausgegeben, wir sehen, die ' und ' weist 113 Zeiten ausgegeben wurde. Die Anzahl die weiterhin steigen, solange die Suchtopologie ausgeführt wird, da der Schnauze kontinuierlich gibt sich wiederholende Sätze aus.

Zwischen Wörtern Emission und zählt wird auch der Zeitraum von 5 Sekunde sein. Dies geschieht, da die __WordCount__ -Komponente konfiguriert ist, um nur Informationen ausgeben, wenn ein Häkchen Tupel eintreffen und es anfordert, dass solche Tupel nur 5 Sekunden standardmäßig übermittelt werden.

## <a name="convert-the-topology-to-flux"></a>Konvertieren Sie die Suchtopologie in Wärmefluss

Wärmefluss handelt es sich um eine neue Framework erhältlich Storm 0.10.0, dem Sie die Konfiguration von Implementierung zu trennen kann. Ihre Komponenten (Schrauben und Spouts,) weiterhin in Java definiert sind, jedoch die Suchtopologie mithilfe einer Datei YAML definiert ist.

Die Datei YAML definiert die Komponenten für die Suchtopologie verwendet wie Datenfluss zwischen ihnen, und welche Werte für die bei der Initialisierung der Komponenten verwendet. Sie können eine Datei YAML als Teil der JAR-Datei, die Ihr Projekt enthält, wenn Sie es, oder Bereitstellen eine externe YAML Datei können beim Starten von der Suchtopologie einbeziehen.

1. Verschieben Sie die Datei __WordCountTopology.java__ aus dem Projekt. In früheren Versionen dieser definiert der Suchtopologie, aber wir nicht verwenden sie für Wärmefluss.

2. Erstellen Sie eine neue Datei namens __topology.yaml__im Verzeichnis __Ressourcen__ . Verwenden Sie die folgenden als den Inhalt dieser Datei ein.

        # topology definition

        # name to be used when submitting. This is what shows up...
        # in the Storm UI/storm command-line tool as the topology name
        # when submitted to Storm
        name: "wordcount"

        # Topology configuration
        config:
        # Hint for the number of workers to create
        topology.workers: 1

        # Spout definitions
        spouts:
        - id: "sentence-spout"
            className: "com.microsoft.example.RandomSentenceSpout"
            # parallelism hint
            parallelism: 1

        # Bolt definitions
        bolts:
        - id: "splitter-bolt"
            className: "com.microsoft.example.SplitSentence"
            parallelism: 1

        - id: "counter-bolt"
            className: "com.microsoft.example.WordCount"
            constructorArgs:
            - 10
            parallelism: 1

        # Stream definitions
        streams:
        - name: "Spout --> Splitter" # name isn't used (placeholder for logging, UI, etc.)
            # The stream emitter
            from: "sentence-spout"
            # The stream consumer
            to: "splitter-bolt"
            # Grouping type
            grouping:
            type: SHUFFLE

        - name: "Splitter -> Counter"
            from: "splitter-bolt"
            to: "counter-bolt"
            grouping:
            type: FIELDS
            # field(s) to group on
            args: ["word"]

    Betrachten Sie wir durchlesen und verstehen Funktionsweise auf jeden Abschnitt und deren Bedeutung der Java-basierte Definition in der Datei __WordCountTopology.java__ .

3. Nehmen Sie folgenden Änderungen an der Datei __pom.xml__ .

    * Fügen Sie die folgende neue Abhängigkeit in der `<dependencies>` Abschnitt:

            <!-- Add a dependency on the Flux framework -->
            <dependency>
                <groupId>org.apache.storm</groupId>
                <artifactId>flux-core</artifactId>
                <version>${storm.version}</version>
            </dependency>

    * Fügen Sie das folgende-Plug-in der `<plugins>` Abschnitt. Dieses Plug-in übernimmt die Erstellung eines Pakets (JAR-Datei) für das Projekt, und Wärmefluss einige bestimmte Transformationen gilt für das Paket zu erstellen.

            <!-- build an uber jar -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-shade-plugin</artifactId>
                <version>2.3</version>
                <configuration>
                    <transformers>
                        <!-- Keep us from getting a "can't overwrite file error" -->
                        <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer" />
                        <transformer implementation="org.apache.maven.plugins.shade.resource.ServicesResourceTransformer" />
                        <!-- We're using Flux, so refer to it as main -->
                        <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                            <mainClass>org.apache.storm.flux.Flux</mainClass>
                        </transformer>
                    </transformers>
                    <!-- Keep us from getting a bad signature error -->
                    <filters>
                        <filter>
                            <artifact>*:*</artifact>
                            <excludes>
                                <exclude>META-INF/*.SF</exclude>
                                <exclude>META-INF/*.DSA</exclude>
                                <exclude>META-INF/*.RSA</exclude>
                            </excludes>
                        </filter>
                    </filters>
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

    * In der __Mitarbeiter-Maven--Plug-Ins__ `<configuration>` Abschnitt, ändern Sie den Wert für `<mainClass>` zu `org.apache.storm.flux.Flux`. Dadurch wird die Wärmefluss verarbeitet der Suchtopologie ausgeführt, wenn wir dann in der Entwicklung lokal ausführen.

    * In der `<resources>` Abschnitt, fügen Sie die folgenden Optionen, um die `<includes>`. Dies umfasst die YAML-Datei, die Suchtopologie als Teil des Projekts definiert.
    
            <include>topology.yaml</include>

## <a name="test-the-flux-topology-locally"></a>Testen der Wärmefluss Suchtopologie lokal

1. Anhand der folgenden kompilieren und Ausführen der Wärmefluss Suchtopologie Maven verwenden.

        mvn compile exec:java -Dexec.args="--local -R /topology.yaml"
    
    Wenn Sie mit PowerShell arbeiten, verwenden Sie Folgendes:
    
        mvn compile exec:java "-Dexec.args=--local -R /topology.yaml"

    Wenn Sie auf einem Linux/Unix/OS X-System sind und [in Ihrer Entwicklungsumgebung Storm installiert](http://storm.apache.org/releases/0.10.0/Setting-up-development-environment.html)haben, können Sie stattdessen die folgenden Befehle:

        mvn compile package
        storm jar target/WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local -R /topology.yaml

    Die `--local` Parameter wird auf Ihrer Entwicklungsumgebung der Suchtopologie im lokalen Modus ausgeführt. Die `-R /topology.yaml` Parameter verwendet die `topology.yaml` Datei Ressourcen aus der JAR-Datei, zu der Suchtopologie definieren.

    Während der Ausführung, wird der Suchtopologie Startinformationen angezeigt. Und sie Linien wie die folgende angezeigt werden, wie Sätze von der Schnauze ausgegeben sind und die Schrauben verarbeiteten beginnt.

        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word snow
        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word white
        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 112 for word seven
        17:33:27 [Thread-16-count] INFO  com.microsoft.example.WordCount - Emitting a count of 195 for word the
        17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 113 for word and
        17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word dwarfs
    
    Es werden eine Verzögerung von 10 Sekunden zwischen Stapeln von protokollierten Informationen, wie die `topology.yaml` Datei übergibt einen Wert der `10` beim Erstellen der WordCount-Komponente. Dadurch wird das Verzögerungsintervall für Teilstriche Tupels auf 10 Sekunden.

2.  Stellen Sie eine Kopie der `topology.yaml` Datei aus dem Projekt. Rufen Sie ihn ungefähr wie folgt `newtopology.yaml`. Klicken Sie in der Datei finden Sie im folgenden Abschnitt, und ändern Sie den Wert der `10` auf `5`. Hiermit ändern Sie den Abstand zwischen den Ausgeben von Stapeln von Wörter von 10 Sekunden auf 5.

          - id: "counter-bolt"
            className: "com.microsoft.example.WordCount"
            constructorArgs:
            - 5
            parallelism: 1

3. Führen Sie die Suchtopologie mit den folgenden Befehl aus:

        mvn exec:java -Dexec.args="--local /path/to/newtopology.yaml"

    Oder, wenn Sie auf Ihrer Entwicklungsumgebung Linux/Unix/OS X Storm haben:

        storm jar target/WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local /path/to/newtopology.yaml

    Ändern der `/path/to/newtopology.yaml` auf den Pfad zu der newtopology.yaml-Datei, die Sie im vorherigen Schritt erstellt haben. Dieser Befehl wird der newtopology.yaml wie die Definition der Suchtopologie verwendet. Da wir aufgenommen habe die `compile` Parameter, wiederverwenden Maven die Version des Projekts in den vorherigen Schritten erstellt.

    Nachdem der Suchtopologie gestartet wird, beachten Sie, dass die Zeit zwischen Ausgabe Blattnamen geändert hat, um den Wert in newtopology.yaml wiederzugeben. Damit Sie sehen können, dass Sie Ihre Konfiguration durch eine YAML-Datei ändern können, ohne dass der Suchtopologie neu kompiliert.

Es gibt einige andere Features, dass Wärmefluss, die hier nicht behandelt werden SFA wie Ersetzen von Variablen in der YAML-Datei, die auf Grundlage von Parametern zur Laufzeit oder von Umgebungsvariablen übergeben. Weitere Informationen zu diesen und anderen Features von Wärmefluss Framework finden Sie unter [Wärmefluss (https://storm.apache.org/releases/0.10.0/flux.html)](https://storm.apache.org/releases/0.10.0/flux.html).

##<a name="trident"></a>Trident

Trident ist eine hohe Abstraktionsebene, die Sprachen bereitgestellt wird. Dynamische Verarbeitung unterstützt. Der primäre Vorteil von Trident ist, dass es kann sichergestellt ist, dass jede Nachricht, die der Suchtopologie gibt nur ein Mal verarbeitet wird. Dies ist schwierig, die in einer unformatierten Java-Suchtopologie erzielen mindestens einmal sicherstellen des, dass die Nachrichten verarbeitet werden. Es gibt auch andere Unterschiede, wie z. B. integrierten Komponenten, die verwendet werden können, statt Schrauben zu erstellen. Tatsächlich werden Schrauben vollständig durch kleiner generische Komponenten, wie z. B. Filter, Projektionen und Funktionen ersetzt.

Trident Applikationen können mithilfe von Maven Projekte erstellt werden. Verwenden Sie die gleichen grundlegenden Schritte wie zuvor in diesem Artikel vorgestellten – nur der Code unterscheidet. Trident kann auch mit der Wärmefluss Framework (aktuell) verwendet werden.

Weitere Informationen zu Trident finden Sie unter der <a href="http://storm.apache.org/documentation/Trident-API-Overview.html" target="_blank">Trident API Overview</a>.

Ein Beispiel für eine Anwendung Trident finden Sie unter [Twitter-beliebte Themen mit Apache Storm auf HDInsight](hdinsight-storm-twitter-trending.md).

##<a name="next-steps"></a>Nächste Schritte

Sie haben gelernt einer Suchtopologie Storm mithilfe von Java erstellt. Jetzt erfahren Sie, wie Sie:

* [Bereitstellen und Verwalten von Apache Storm Topologien auf HDInsight](hdinsight-storm-deploy-monitor-topology.md)

* [Entwickeln Sie C#-Topologien für Apache Storm auf HDInsight mithilfe von Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md)

Weitere Beispiel können Storm Topologien finden Sie auf [Beispiel Topologien für Storm auf HDInsight](hdinsight-storm-example-topology.md).

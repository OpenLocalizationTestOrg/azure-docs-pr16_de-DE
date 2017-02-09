## <a name="receive-messages-with-apache-storm"></a>Empfangen von Nachrichten mit Apache Storm

[**Apache Storm**](https://storm.incubator.apache.org) handelt es sich um ein System verteilt in Echtzeit Berechnung, die zuverlässigen Verarbeitung ungebundener Streams von Daten vereinfacht. In diesem Abschnitt zeigt, wie ein Ereignis Hubs Storm Schnauze um Ereignis Hubs Ereignisse zu empfangen. Apache Storm können Sie Ereignisse für mehrere Prozesse in verschiedenen Knoten gehostet aufteilen. Die Ereignis Hubs-Integration in Storm vereinfacht Ereignis Verbrauch durch transparent geänderte Fortschrittsberichte mithilfe des Storm Zookeeper Installation, beständige Kontrollpunkten verwalten und Parallel von Ereignis Hubs empfängt.

Weitere Informationen zum Ereignis Hubs erhalten Sie Mustern, finden Sie unter dem [Ereignis Hubs Übersicht][].

In diesem Lernprogramm verwendet eine Installation [HDInsight Storm][] ist Ereignis Hubs Schnauze bereits zur Verfügung Lieferumfang.

1. Gehen Sie wie [HDInsight Storm - erste Schritte](../articles/hdinsight/hdinsight-storm-overview.md) zum Erstellen eines neuen HDInsight Clusters und damit über Remote Desktop verbinden.

2. Kopieren der `%STORM_HOME%\examples\eventhubspout\eventhubs-storm-spout-0.9-jar-with-dependencies.jar` Datei zu Ihrem lokalen Entwicklungsumgebung. Diese Datei enthält die Ereignisse Storm Schnauze.

3. Verwenden Sie den folgenden Befehl zur Installation des Pakets in den lokalen Maven Speicher ein. So können Sie es als Referenz im Projekt Storm in einem späteren Schritt hinzufügen.

        mvn install:install-file -Dfile=target\eventhubs-storm-spout-0.9-jar-with-dependencies.jar -DgroupId=com.microsoft.eventhubs -DartifactId=eventhubs-storm-spout -Dversion=0.9 -Dpackaging=jar

4. In "Ellipse", erstellen Sie ein neues Maven-Projekt (klicken Sie auf **Datei**, und klicken Sie dann **neu**und dann auf **Projekt**).

    ![][12]

5. Wählen Sie **Arbeitsbereich-Standardspeicherort verwenden**und dann auf **Weiter**

6. Wählen Sie aus der **Maven-Urtyp-Schnellstart** Urtyp und dann auf **Weiter**

7. Fügen Sie ein **Gruppen-ID** und **ArtifactId**, und klicken Sie auf **Fertig stellen**

8. In **pom.xml**, fügen Sie die folgenden Abhängigkeiten in der `<dependency>` Knoten.

        <dependency>
            <groupId>org.apache.storm</groupId>
            <artifactId>storm-core</artifactId>
            <version>0.9.2-incubating</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>com.microsoft.eventhubs</groupId>
            <artifactId>eventhubs-storm-spout</artifactId>
            <version>0.9</version>
        </dependency>
        <dependency>
            <groupId>com.netflix.curator</groupId>
            <artifactId>curator-framework</artifactId>
            <version>1.3.3</version>
            <exclusions>
                <exclusion>
                    <groupId>log4j</groupId>
                    <artifactId>log4j</artifactId>
                </exclusion>
                <exclusion>
                    <groupId>org.slf4j</groupId>
                    <artifactId>slf4j-log4j12</artifactId>
                </exclusion>
            </exclusions>
            <scope>provided</scope>
        </dependency>

9. Klicken Sie im Ordner **Src** erstellen Sie eine Datei namens **Config.properties** , und kopieren Sie den folgenden Inhalt, ersetzen die folgenden Werte:

        eventhubspout.username = ReceiveRule

        eventhubspout.password = {receive rule key}

        eventhubspout.namespace = ioteventhub-ns

        eventhubspout.entitypath = {event hub name}

        eventhubspout.partitions.count = 16

        # if not provided, will use storm's zookeeper settings
        # zookeeper.connectionstring=localhost:2181

        eventhubspout.checkpoint.interval = 10

        eventhub.receiver.credits = 10

    Der Wert für **eventhub.receiver.credits** bestimmt, wie viele Ereignisse vor der Freigabe von in der Verkaufspipeline Storm zusammengefasst werden. Aus Gründen der Vereinfachung wird in diesem Beispiel wird der Wert auf 10. In der Herstellung sollten sie normalerweise auf höhere Werte festgelegt werden. beispielsweise 1024.

10. Erstellen Sie eine neue Klasse namens **LoggerBolt** mit den folgenden Code ein:

        import java.util.Map;
        import org.slf4j.Logger;
        import org.slf4j.LoggerFactory;
        import backtype.storm.task.OutputCollector;
        import backtype.storm.task.TopologyContext;
        import backtype.storm.topology.OutputFieldsDeclarer;
        import backtype.storm.topology.base.BaseRichBolt;
        import backtype.storm.tuple.Tuple;

        public class LoggerBolt extends BaseRichBolt {
            private OutputCollector collector;
            private static final Logger logger = LoggerFactory
                      .getLogger(LoggerBolt.class);

            @Override
            public void execute(Tuple tuple) {
                String value = tuple.getString(0);
                logger.info("Tuple value: " + value);

                collector.ack(tuple);
            }

            @Override
            public void prepare(Map map, TopologyContext context, OutputCollector collector) {
                this.collector = collector;
                this.count = 0;
            }

            @Override
            public void declareOutputFields(OutputFieldsDeclarer declarer) {
                // no output fields
            }

        }

    Diese Storm herstellt protokolliert den Inhalt der empfangenen Ereignisse. Dies kann leicht zum Speichern von Tupel in einer Speicherdienst erweitert werden. Das [HDInsight Sensor Analyse Lernprogramm] verwendet dasselbe Verfahren zum Speichern von Daten in HBase.

11. Erstellen Sie eine Klasse namens **LogTopology** mit den folgenden Code ein:

        import java.io.FileReader;
        import java.util.Properties;
        import backtype.storm.Config;
        import backtype.storm.LocalCluster;
        import backtype.storm.StormSubmitter;
        import backtype.storm.generated.StormTopology;
        import backtype.storm.topology.TopologyBuilder;
        import com.microsoft.eventhubs.samples.EventCount;
        import com.microsoft.eventhubs.spout.EventHubSpout;
        import com.microsoft.eventhubs.spout.EventHubSpoutConfig;

        public class LogTopology {
            protected EventHubSpoutConfig spoutConfig;
            protected int numWorkers;

            protected void readEHConfig(String[] args) throws Exception {
                Properties properties = new Properties();
                if (args.length > 1) {
                    properties.load(new FileReader(args[1]));
                } else {
                    properties.load(EventCount.class.getClassLoader()
                            .getResourceAsStream("Config.properties"));
                }

                String username = properties.getProperty("eventhubspout.username");
                String password = properties.getProperty("eventhubspout.password");
                String namespaceName = properties
                        .getProperty("eventhubspout.namespace");
                String entityPath = properties.getProperty("eventhubspout.entitypath");
                String zkEndpointAddress = properties
                        .getProperty("zookeeper.connectionstring"); // opt
                int partitionCount = Integer.parseInt(properties
                        .getProperty("eventhubspout.partitions.count"));
                int checkpointIntervalInSeconds = Integer.parseInt(properties
                        .getProperty("eventhubspout.checkpoint.interval"));
                int receiverCredits = Integer.parseInt(properties
                        .getProperty("eventhub.receiver.credits")); // prefetch count
                                                                    // (opt)
                System.out.println("Eventhub spout config: ");
                System.out.println("  partition count: " + partitionCount);
                System.out.println("  checkpoint interval: "
                        + checkpointIntervalInSeconds);
                System.out.println("  receiver credits: " + receiverCredits);

                spoutConfig = new EventHubSpoutConfig(username, password,
                        namespaceName, entityPath, partitionCount, zkEndpointAddress,
                        checkpointIntervalInSeconds, receiverCredits);

                // set the number of workers to be the same as partition number.
                // the idea is to have a spout and a logger bolt co-exist in one
                // worker to avoid shuffling messages across workers in storm cluster.
                numWorkers = spoutConfig.getPartitionCount();

                if (args.length > 0) {
                    // set topology name so that sample Trident topology can use it as
                    // stream name.
                    spoutConfig.setTopologyName(args[0]);
                }
            }

            protected StormTopology buildTopology() {
                TopologyBuilder topologyBuilder = new TopologyBuilder();

                EventHubSpout eventHubSpout = new EventHubSpout(spoutConfig);
                topologyBuilder.setSpout("EventHubsSpout", eventHubSpout,
                        spoutConfig.getPartitionCount()).setNumTasks(
                        spoutConfig.getPartitionCount());
                topologyBuilder
                        .setBolt("LoggerBolt", new LoggerBolt(),
                                spoutConfig.getPartitionCount())
                        .localOrShuffleGrouping("EventHubsSpout")
                        .setNumTasks(spoutConfig.getPartitionCount());
                return topologyBuilder.createTopology();
            }

            protected void runScenario(String[] args) throws Exception {
                boolean runLocal = true;
                readEHConfig(args);
                StormTopology topology = buildTopology();
                Config config = new Config();
                config.setDebug(false);

                if (runLocal) {
                    config.setMaxTaskParallelism(2);
                    LocalCluster localCluster = new LocalCluster();
                    localCluster.submitTopology("test", config, topology);
                    Thread.sleep(5000000);
                    localCluster.shutdown();
                } else {
                    config.setNumWorkers(numWorkers);
                    StormSubmitter.submitTopology(args[0], config, topology);
                }
            }

            public static void main(String[] args) throws Exception {
                LogTopology topology = new LogTopology();
                topology.runScenario(args);
            }
        }


    Diese Klasse erstellt ein neues Ereignis Hubs Schnauze, mit den Eigenschaften, die in der Konfiguration ablegen zu Instatiate. Es ist wichtig, beachten Sie, dass in diesem Beispiel viele Spouts Aufgaben als die Anzahl der Partitionen im Hub Ereignis erstellt, damit die von diesem Ereignis Hub zulässige maximale Parallelität verwenden können.

<!-- Links -->
[Ereignis Hubs (Übersicht)]: ../articles/event-hubs/event-hubs-overview.md
[HDInsight Storm]: ../articles/hdinsight/hdinsight-storm-overview.md
[HDInsight Sensor Analysis-Lernprogramm]: ../articles/hdinsight/hdinsight-storm-sensor-data-analysis.md

<!-- Images -->

[12]: ./media/service-bus-event-hubs-get-started-receive-storm/create-storm1.png
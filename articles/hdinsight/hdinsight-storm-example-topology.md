<properties
 pageTitle="Beispiel für Apache Storm Topologien auf HDInsight | Microsoft Azure"
 description="Eine Liste der Storm Topologien erstellt und getestet mit Apache Storm auf HDInsight einschließlich c# und Java Standardtopologien und Arbeiten mit Ereignis Hubs."
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

# <a name="example-storm-toplogies-and-components-for-apache-storm-on-hdinsight"></a>Beispiel für Storm Toplogies und Komponenten für Apache Storm auf HDInsight

Im folgenden finden eine Liste von Beispielen erstellt und von Microsoft zur Verwendung mit Apache Storm auf HDInsight verwaltet. In diesen Beispielen hervorgehen eine Vielzahl von Themen, grundlegende c# und Java Topologien für die Arbeit mit Azure Diensten wie Ereignis Hubs, DocumentDB, Power BI, SQL-Datenbank, HBase HDInsight und Azure-Speicher zu erstellen. Einige Beispiele veranschaulichen auch zum Arbeiten mit nicht Azure oder auch nicht-Microsoft-Produkte, wie etwa SignalR und Socket.IO

| Beschreibung                                                                                             | Veranschaulicht                                         | Sprache/Framework         |
|:--------------------------------------------------------------------------------------------------------|:-----------------------------------------------------|:---------------------------|
| [Schreiben Sie von Apache Storm Azure Lake Datenspeicher](hdinsight-storm-write-data-lake-store.md) | Schreiben in Azure Lake Datenspeicher | Java |
| [Ereignis-Hub Spout und an Quelle](https://github.com/apache/storm/tree/master/external/storm-eventhubs) | Quelle für das Ereignis Hub Schnauze und herstellt | Java |
| [Entwickeln Sie Java-basierte Topologien für Apache Storm auf HDInsight][5797064f]                                 | Maven                                                | Java                       |
| [Entwickeln Sie C#-Topologien für Apache Storm auf HDInsight mithilfe von Visual Studio][16fce2d1]                     | HDInsight Tools für Visual Studio                    | Java c#                   |
| [Erstellen Sie mehrerer Datenstreams in einer C#-Storm Suchtopologie][ec5a4064]                                         | Mehrere streams                                     | C#                         |
| [Bestimmen Sie, beliebte Twitter-Themen mit Storm auf HDInsight][3c86c7c8]                                   | Trident                                              | Java, Trident              |
| [Verarbeiten von Ereignissen aus Azure Ereignis Hubs mit Storm auf HDInsight (c#)][844d1d81]                                | Ereignis Hubs                                           | C# und Java                |
| [Verarbeiten von Ereignissen aus Azure Ereignis Hubs mit Storm auf HDInsight (Java)](hdinsight-storm-develop-java-event-hub-topology.md) | Ereignis Hubs | Java |
| [Verwenden von Power Bi zur Visualisierung von Daten aus einer Suchtopologie Storm][94d15238]                              | Power BI                                             | C#                         |
| [Analysieren von Sensordaten mit Storm und HBase in HDInsight][ab894747]                                     | Ereignis Hubs, HBase, Socket.IO, Web-dashboard          | C#, Java, JavaScript, HTML |
| [Prozess Fahrzeug Sensordaten von Ereignis Hubs Storm auf HDInsight verwenden][246ee964]                        | Ereignis Hubs, DocumentDb, Speicher Azure Blob (WASB)    | Java c#                   |
| [Extrahieren, Transformieren und laden (ETL) von Azure Ereignis Hubs HBase, verwenden Storm auf HDInsight][b4b68194] | Ereignis Hubs, HBase                                    | C#                         |
| [Vorlage c# Storm Suchtopologie Projekt für das Arbeiten mit Azure-Dienste von Storm auf HDInsight][ce0c02a2]  | Ereignis Hubs, DocumentDb, SQL-Datenbank, HBase, SignalR | Java c#                   |
| [Skalierbarkeit Benchmarks für das Lesen aus Azure Ereignis Hubs Storm auf HDInsight verwenden][d6c540e3]           | Nachrichtendurchsatz, Ereignis Hubs, SQL-Datenbank         | Java c#                   |
| [Zuordnen von Ereignissen verwenden Storm und HBase auf HDInsight](hdinsight-storm-correlation-topology.md) | HBase | C# |
| [Verwenden von Python mit Storm auf HDInsight](hdinsight-storm-develop-python-topology.md) | Python-Komponenten mit Java und Clojure Grundlage Storm Topologien | Python |

## <a name="next-steps"></a>Nächste Schritte

* [Erste Schritte mit Apache Storm auf HDInsight][2b8c3488]

* [Informationen Sie zum Bereitstellen und Verwalten von Storm Topologien mit Storm auf HDInsight][6eb0d3b8]

  [2b8c3488]: hdinsight-apache-storm-tutorial-get-started-linux.md "Informationen Sie zum Erstellen eines Sturms auf Cluster HDInsight und mithilfe von Dashboard Storm Beispiel Topologien bereitstellen."
  [6eb0d3b8]: hdinsight-storm-deploy-monitor-topology.md "Informationen Sie zum Bereitstellen und Verwalten von Topologien mit dem webbasierten Storm Dashboard und Storm-Benutzeroberfläche oder die HDInsight-Tools für Visual Studio."
  [16fce2d1]: hdinsight-storm-develop-csharp-visual-studio-topology.md "Informationen Sie zum C#-Storm Topologien mithilfe der HDInsight-Tools für Visual Studio erstellen."
  [5797064f]: hdinsight-storm-develop-java-topology.md "Informationen Sie zum Erstellen von Storm Topologien in Java Maven, durch das Erstellen eines grundlegenden Wordcount Suchtopologie verwenden."
  [94d15238]: hdinsight-storm-power-bi-topology.md "Veranschaulicht das Schreiben von Daten in Power BI aus einer Suchtopologie c#, und klicken Sie dann aus den Daten ein Diagramm und Dashboards erstellen."
  [ec5a4064]: https://github.com/Blackmist/csharp-storm-example "Veranschaulicht eine grundlegende Storm Suchtopologie, die einer Wordcount, in c# implementiert ausführt. Dadurch wird veranschaulicht, wie mehrere Datenstreams innerhalb einer Suchtopologie c# zu erstellen."
  [844d1d81]: hdinsight-storm-develop-csharp-event-hub-topology.md "Informationen Sie zum Lesen und Schreiben von Daten aus Azure Ereignis Hubs mit Storm auf HDInsight."
  [ab894747]: hdinsight-storm-sensor-data-analysis.md "Informationen Sie zum Verwenden von Apache Storm auf HDInsight zum Verarbeiten von Azure Ereignis Hubs Sensordaten visualisieren sie mithilfe von D3.js, und speichern Sie sie (optional) HBase."
  [3c86c7c8]: hdinsight-storm-twitter-trending.md "Erfahren Sie, wie Trident mithilfe einer Suchtopologie Storm erstellen, der bestimmt, beliebte Themen (basierend auf Hashtags,) auf Twitter."
  [246ee964]: hdinsight-storm-iot-eventhub-documentdb.md "Informationen Sie zum Verwenden einer Suchtopologie Storm gelesenen Nachrichten aus Azure Ereignis Hubs, Lesen von Dokumenten, die für Verweise Daten aus Azure DocumentDB und Speichern von Daten in Azure-Speicher."
  [d6c540e3]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/EventCountExample "Mehrere Topologien Durchsatz beim Lesen von Azure Ereignis Hubs und Speichern von mit SQL-Datenbank mithilfe von Apache Storm auf HDInsight zu veranschaulichen."
  [b4b68194]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/RealTimeETLExample "Informationen Sie zum Lesen von Daten aus Azure Ereignis Hubs, aggregieren und Transformieren der Daten, und klicken Sie dann auf HBase auf HDInsight zu speichern."
  [ce0c02a2]: https://github.com/hdinsight/hdinsight-storm-examples/tree/master/templates/HDInsightStormExamples "Dieses Projekt enthält Vorlagen für Spouts, Schrauben und Topologien Interaktion mit verschiedenen Azure-Diensten wie Ereignis Hubs, DocumentDB und SQL-Datenbank."
 

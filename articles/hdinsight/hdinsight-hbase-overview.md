<properties
    pageTitle="Was ist HBase in HDInsight? | Microsoft Azure"
    description="Einführung in die Apache HBase in HDInsight, eine Datenbank NoSQL aufeinander Hadoop. Informationen zum Verwenden von Fällen und HBase im Vergleich zu anderen Hadoop Cluster."
    keywords="BigTable Nosql, was Hbase ist"
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
    ms.topic="get-started-article"
    ms.date="09/14/2016"
    ms.author="jgao"/>



# <a name="what-is-hbase-in-hdinsight-a-nosql-database-that-provides-bigtable-like-capabilities-for-hadoop"></a>Neuigkeiten in HDInsight HBase: A NoSQL-Datenbank, die BigTable-ähnliche Funktionen für die Hadoop bereitstellt

Apache HBase ist eine Open-Source-NoSQL-Datenbank, die auf Hadoop erstellt und nach Google BigTable erstellt wird. HBase bietet Direktzugriff und sicherer Konsistenz für große Mengen von unstrukturierten und semistructured Daten in einer Spalte Familien geordnet schemaless-Datenbank.

Daten werden in den Zeilen einer Tabelle gespeichert, und Daten in einer Zeile nach Spalte Familie gruppiert ist. HBase ist eine Datenbank schemaless, d. h., weder die Spalten noch den Typ der Daten, die sie gehörende Kehrmatrix definiert werden, bevor Sie sie verwenden müssen. Öffnen-Quellcode wird linear skaliert, um Petabyte von Daten auf Tausende von Knoten behandeln. Sie können abhängig Datenredundanz, Stapelverarbeitung und andere Features, die von verteilten Applications in der Hadoop-Netz bereitgestellt werden.

## <a name="how-is-hbase-implemented-in-azure-hdinsight"></a>Wie wird HBase in Azure HDInsight implementiert?

HDInsight HBase wird als verwalteten Cluster angeboten, die in der Azure-Umgebung integriert ist. Zuordnungseinheiten werden so konfiguriert, dass Daten direkt in Azure Blob-Speicher speichern das niedrige Wartezeit und höhere Elastizität in Leistung und Kosten Auswahlmöglichkeiten bereitstellt. Dies kann Kunden interaktiven Websites erstellen, die Arbeiten mit großen Datasets, die zur Erstellung von Diensten, die Sensor und werden Daten aus mehreren Millionen Endpunkte speichern, und klicken Sie zum Analysieren der Daten mit Hadoop Aufträge. HBase und Hadoop sind gute Ausgangspunkte für big Data Project in Azure; Insbesondere kann in Echtzeit Applikationen für die Arbeit mit großen Datasets aktiviert werden.

Die Implementierung HDInsight nutzt die Skalierung Architektur HBase automatische Sharding von Tabellen, sicherer Konsistenz zum Lesen und Schreiben und automatisches Failover bereitstellen. Performance wird verbessert, indem Sie im Speicher für Lese- und hohem Durchsatz für schreibt streaming Zwischenspeichern. Virtuelle Netzwerk provisioning steht auch HDInsight HBase. Details finden Sie unter [Bereitstellen von HDInsight Cluster virtuellen Netzwerk Azure] [hbase-provision-vnet].

## <a name="how-is-data-managed-in-hdinsight-hbase"></a>Wie werden die Daten in HDInsight HBase verwaltet?

Daten können in HBase verwaltet werden, mithilfe der `create`, `get`, `put`, und `scan` die Verwaltungsshell HBase Befehle. Daten in der Datenbank mithilfe von geschrieben `put` und Zusammenfassungsdaten mithilfe `get`. Die `scan` Befehl zum Abrufen von Daten aus mehreren Zeilen in einer Tabelle verwendet wird. Daten können auch mit HBase C#-API, die Clientbibliothek über die HBase REST-API bietet verwaltet werden. Eine HBase Datenbank kann auch mithilfe der Struktur abgefragt werden. Eine Einführung in diese programming Modelle, finden Sie unter [Erste Schritte mit HBase mit Hadoop in HDInsight][hbase-get-started]. Co-Prozessoren stehen auch zur Verfügung, die Verarbeitung von Daten in den Knoten, die als Host der Datenbank zulassen.


## <a name="scenarios-use-cases-for-hbase"></a>Szenarien: Verwenden von Fällen für HBase
Die kanonische Anwendungsfall-, für welche BigTable (und von Erweiterung, HBase) wurde erstellt wurde Web suchen. Suchmaschinen Erstellen von Indizes, die die Webseiten Ausdrücke zuordnen, die diese enthalten. Es gibt jedoch einige vielen verwenden Fällen, die HBase für geeignet ist – von denen einige in diesem Abschnitt aufgeführt sind.

- Schlüsselwert store

    HBase als Schlüsselwert Speicher verwendet werden kann, und sie eignet sich für die Verwaltung von Nachrichtensysteme. Facebook verwendet HBase für messaging-System, und es ist ideal zum Speichern und Verwalten von Internet-Kommunikation. WebTable verwendet HBase zum Suchen nach und Verwalten von Tabellen, die von Webseiten extrahiert werden.

- Sensordaten

    HBase eignet sich zum Sammeln von Daten, die aus verschiedenen Quellen inkrementell erfasst werden. Dies umfasst sozialen Analytics, Zeitreihe Stand interaktive Dashboards mit Trends und Indikatoren und Audit Log Systeme verwalten. Beispiele: Bloomberg Händler terminal und die Uhrzeit Reihe Datenbank öffnen (OpenTSDB), das speichert und ermöglicht den Zugriff auf gesammelte Informationen zu den Integritätsstatus Serverbetriebssysteme Kennzahlen.

- Abfragen in Echtzeit

    [Karlsruhe](http://phoenix.apache.org/) ist eine SQL-Abfrage-Engine für Apache HBase. Es wird als-Treiber zugegriffen und Abfragen und Verwalten von HBase Tabellen mithilfe von SQL ermöglicht.

- HBase als Plattform

    Auf HBase ausgeführt werden können, indem Sie ihn als einen Datenspeicher verwenden. Beispiele für Karlsruhe, OpenTSDB, Kiji und Titan. Applikationen können auch in HBase integriert. Beispiele: Struktur, Schwein, Solr, Sturm, Flume, Impala, Spark, genehmigt und Drillup.


##<a name="a-namenext-stepsanext-steps"></a><a name="next-steps"></a>Nächste Schritte

- [Erste Schritte mit HBase mit Hadoop in HDInsight][hbase-get-started]
- [Bereitstellen von HDInsight Cluster virtuellen Netzwerk Azure] [hbase-provision-vnet]
- [Konfigurieren der Replikation HBase HDInsight](hdinsight-hbase-geo-replication.md)
- [Analysieren Sie Twitter-Grüße mit HBase in HDInsight][hbase-twitter-sentiment]
- [Verwenden von Maven Java Applications erstellen, die HBase mit HDInsight (Hadoop) verwenden][hbase-build-java-maven]

##<a name="a-namesee-alsoasee-also"></a><a name="see-also"></a>Siehe auch

- [Apache HBase](https://hbase.apache.org/)
- [BigTable: Ein verteilten Speichersystem für strukturierte Daten](http://research.google.com/archive/bigtable.html)




[hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md

[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md

[hbase-build-java-maven]: hdinsight-hbase-build-java-maven.md

[hdinsight-use-hive]: hdinsight-use-hive.md

[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md

[hbase-get-started]: http://azure.microsoft.com/documentation/articles/hdinsight-hbase-get-started/

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]: ../storage-create-storage-account.md

[apache-hadoop]: http://hadoop.apache.org/

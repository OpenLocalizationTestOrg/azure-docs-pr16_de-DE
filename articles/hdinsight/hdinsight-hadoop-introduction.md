 <properties
    pageTitle="Einführung in die Hadoop - was Hadoop auf HDInsight ist? | Microsoft Azure"
    description="Erhalten Sie eine Einführung in Hadoop, verteilte große Datenverarbeitung und Analyse und die Komponenten der Hadoop-Netz in der Cloud auf HDInsight."
    keywords="große Datenanalyse Einführung Hadoop, was ist Hadoop, Hadoop Technologie Stapel, Hadoop-Netz"
    services="hdinsight"
    documentationCenter=""
    authors="cjgronlund"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/21/2016"
   ms.author="cgronlun"/>


# <a name="an-introduction-to-the-hadoop-ecosystem-on-azure-hdinsight"></a>Eine Einführung in die Hadoop-Netz auf Azure HDInsight

 Dieser Artikel enthält eine Einführung in Hadoop Azure HDInsight, deren Netz und große Daten. Informationen Sie zu Hadoop Komponenten, allgemeine Terminologie und Szenarien für große Datenanalyse.

## <a name="what-is-hadoop-on-hdinsight"></a>Was ist Hadoop auf HDInsight?

Hadoop bezieht sich auf eine Netz von Open Source-Software, die einen Rahmen für verteilten Verarbeitung, die Speicherung ist und Analyse von große Datenmengen auf Computern Cluster. Azure HDInsight bereitgestellt, die Hadoop-Komponenten von der Verteilung **Hortonworks Daten Plattform (HDP)** in der Cloud, bereitstellt und Vorschriften verwalteten Cluster mit hoher Zuverlässigkeit und Verfügbarkeit.  

Hadoop Apache wurde das ursprüngliche öffnen Quellprojekt für große Datenverarbeitung. Folgende wurde die Entwicklung von verknüpften Software- und Dienstprogramme als Teil des Stapels Technologie Hadoop, einschließlich Apache Struktur, Apache HBase, Apache Spark und vielem mehr. Details finden Sie unter [Übersicht über die Hadoop-Netz in HDInsight](#overview) .

## <a name="what-is-big-data"></a>Was ist eine große Daten?

Große Daten beschreibt alle großen Textkörper des digitalen Informationen aus dem Text in einen Twitter-Feeds, mit der Sensorinformationen aus industrielle Geräte mit Informationen zu Kunden durchsuchen und Einkäufe auf einer Website. Große Daten können es sich um zurückliegenden (d. h. gespeicherte Daten) oder in Echtzeit (d. h., gestreamt direkt aus der Quelle). Große Daten ist erfasst wird, in einer erweitern und -eskalieren Datenmengen bei zunehmend höheren Geschwindigkeitsinformationen, verschiedene Formate.

Für große Daten einige Intelligence oder einen Einblick bereitstellen müssen Sie relevante Daten sammeln und die richtigen Fragen. Sie müssen außerdem sicherstellen, dass die Daten bereinigt, analysiert, und klicken Sie dann in eine gute Möglichkeit präsentiert zugegriffen werden, ist. Das war, in dem große Datenanalyse auf Hadoop in HDInsight helfen können.

## <a name="a-nameoverviewaoverview-of-the-hadoop-ecosystem-in-hdinsight"></a><a name="overview"></a>Übersicht über die Hadoop-Netz in HDInsight

HDInsight ist eine Cloud-Verteilung auf Microsoft Azure des schnell wachsende Apache Hadoop Technologie Stapels für große Datenanalyse. Er enthält Implementierung von Apache Spark, HBase, Sturm, Schwein, Struktur, Sqoop, Oozie, Ambari und So weiter. HDInsight kann auch in Business Intelligence (BI)-Tools, wie Power BI, Excel, SQL Server Analysis Services und SQL Server Reporting Services integriert.

### <a name="hadoop-hbase-spark-storm-and-customized-clusters"></a>Hadoop, HBase, Spark, Storm und angepasste Cluster

HDInsight bietet Clusterkonfigurationen für Apache Hadoop, Spark, HBase oder Storm. Alternativ können Sie [Cluster mit Skript-Aktionen anpassen](hdinsight-hadoop-customize-cluster-linux.md).

* **Hadoop**: bietet zuverlässigen Datenspeicher mit [HDFS](#hdfs)und ein einfaches [MapReduce](#mapreduce) programming Modell zum Verarbeiten und Analysieren von Daten in Parallel.

* **<a target="_blank" href="http://spark.apache.org/">Apache Spark</a>**: ein parallele Verarbeitung Framework, das in-Memory Verarbeitung vor, um die Leistung der big Data Analysis-Anwendung, durch fördern unterstützt aufzeigen funktioniert für SQL, streaming-Daten und Schulung Computer. Finden Sie unter [Übersicht: Was ist, dass Apache Spark in HDInsight?](hdinsight-apache-spark-overview.md)

* **<a target="_blank" href="http://hbase.apache.org/">HBase</a>**: der das Eineinhalbfache Millionen von Spalten einer NoSQL-Datenbank basierend auf Hadoop, die Direktzugriff und sicherer Konsistenz für große Datenmengen unstrukturierten und teilweise strukturierter - potenziell Milliarden Zeilen enthält. Finden Sie unter [Übersicht über HBase auf HDInsight](hdinsight-hbase-overview.md).

* **<a  target="_blank" href="https://storm.incubator.apache.org/">Apache Storm</a>**: ein System verteilt, in Echtzeit Berechnung für die Verarbeitung von fast umfangreicher Streams mit Daten. Storm wird als verwalteten Cluster in HDInsight angeboten. Finden Sie unter [Analysieren in Echtzeit Sensordaten Storm und Hadoop verwenden](hdinsight-storm-sensor-data-analysis.md).

### <a name="example-customization-scripts"></a>Beispiel für Anpassung Skripts

Skript-Aktionen handelt es sich um Skripts, die während der Bereitstellung Cluster ausführen, und können verwendet werden, um weitere Komponenten auf dem Cluster installieren. Für Linux-basierten Cluster sind Bash Skripts.

Im folgenden Beispielskripts werden durch das Team HDInsight bereitgestellt:

* [Farbton](hdinsight-hadoop-hue-linux.md): eine Gruppe von Webanwendungen Interaktion mit einem Cluster verwendet. Nur Linux Cluster.

* [Giraph](hdinsight-hadoop-giraph-install-linux.md): Graph zu Modell Beziehungen zwischen Elementen oder Personen verarbeiten.

* [R](hdinsight-hadoop-r-scripts-linux.md): ein offener Quelle Sprache und Umgebung für maschinelle interessante verwendet statistische computing.

* [Solr](hdinsight-hadoop-solr-install-linux.md): eine unternehmensweite Suche Plattform, die voll-Textsuche Daten zulässt.

Informationen zur Entwicklung von Skript-Aktionen finden Sie unter [Aktion Skript Development mit HDInsight](hdinsight-hadoop-script-actions-linux.md).


## <a name="what-are-the-hadoop-components-and-utilities"></a>Was sind die Komponenten Hadoop und Dienstprogramme?

Die folgenden Komponenten und Dienstprogramme sind auf HDInsight Cluster enthalten.

* **[Ambari](#ambari)**: Cluster bereitgestellt, Verwaltung, Überwachung und Dienste.

* **[Avro](#avro)** (Microsoft .NET Bibliothek für Avro): Datenserialisierung für die Microsoft .NET-Umgebung.

* **[Struktur und HCatalog](#hive)**: Structured Query Language (SQL) – wie Abfragen und einer Tabelle und Speicher Management-Layer.

* **[Mahout](#mahout)**: für skalierbare Computer Erlernen von Applications.

* **[MapReduce](#mapreduce)**: Legacy-Framework für Hadoop distributed Verarbeitung und der ressourcenverwaltung. Finden Sie unter [aus](#yarn), der nächsten Generation Ressource Framework.

* **[Oozie](#oozie)**: workflowverwaltung.

* **[Karlsruhe](#phoenix)**: relationalen Datenbank Layer über HBase.

* **[Schwein](#pig)**: einfacher Skripting für MapReduce Transformationen.

* **[Sqoop](#sqoop)**: Daten importieren und exportieren.

* **[Tez](#tez)**: können Daten ankommt Prozesse effizient bei ausgeführt werden.

* **[Aus](#yarn)**: Teil der Hadoop Core-Bibliothek und der nächsten Generation von Software MapReduce Framework.

* **[ZooKeeper](#zookeeper)**: Koordinierung von Prozessen in verteilten Systemen.

> [AZURE.NOTE] Informationen zu den bestimmten Komponenten und Versionsinformationen finden Sie unter [Hadoop Komponenten, versionsverwaltung, und Service-Angebote in HDInsight][component-versioning]

### <a name="a-nameambariaambari"></a><a name="ambari"></a>Ambari

Apache Ambari ist für die Bereitstellung, Verwaltung und Überwachung Apache Hadoop Cluster. Sie enthält eine intuitive Sammlung von Tools Operator und eine umfangreiche Sammlung von APIs, die die Komplexität der Hadoop, vereinfachen den Vorgang der Cluster ausblenden. Linux-basierte HDInsight, die Cluster beide bieten die Ambari web Benutzeroberfläche und die Ambari REST-API, während der Windows-basierten Cluster eine Teilmenge der die REST-API bieten. Ambari Ansichten auf HDInsight Cluster zulassen-Plug-in-Funktionen, die Benutzeroberfläche an.

Finden Sie unter [Verwalten von HDInsight Cluster Ambari verwenden](hdinsight-hadoop-manage-ambari.md) (nur Linux), [Monitor Hadoop Cluster in HDInsight mithilfe der Ambari-API](hdinsight-monitor-use-ambari-api.md)und <a target="_blank" href="https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md">Apache Ambari-API-Referenz</a>.

### <a name="a-nameavroaavro-microsoft-net-library-for-avro"></a><a name="avro"></a>Avro (Microsoft .NET Bibliothek für Avro)

Microsoft .NET Bibliothek für Avro implementiert das Apache Avro compact binäre Data Interchange-Format aus, für die Serialisierung für die Microsoft .NET-Umgebung. Er verwendet <a target="_blank" href="http://www.json.org/">JavaScript Object Notation (JSON)</a> aus, um eine Sprache-unabhängige Schema zu definieren, die Sprache Interoperability abschließt, Bedeutung Daten in einer Sprache serialisiert können in einer anderen gelesen werden. Ausführliche Informationen zum Format finden Sie der < ein Ziel = _ "leere"-Referenz = "http://avro.apache.org/docs/current/spec.html" > Apache Avro Spezifikation</a>.
Das Format von Avro Dateien unterstützt das verteilte MapReduce Modell Programmierung. Dateien sind "es", d. h., Sie können einer beliebigen Stelle in einer Datei zu suchen und Lesebereich ab einem bestimmten Block beginnen. Um herauszufinden, wie, finden Sie unter [Serialize Daten mit der Bibliothek Microsoft .NET für Avro](hdinsight-dotnet-avro-serialization.md).


### <a name="a-namehdfsahdfs"></a><a name="hdfs"></a>HDFS

Hadoop Distributed Datei System (HDFS) wird eines verteilten Dateisystems, das mit MapReduce und aus, das Herzstück der Hadoop-Netz ist. HDFS ist der standard für Dateisystem Hadoop Cluster auf HDInsight.

### <a name="a-namehiveahive--hcatalog"></a><a name="hive"></a>Struktur und HCatalog

<a target="_blank" href="http://hive.apache.org/">Apache Struktur</a> ist Datawarehouse Software auf Hadoop, die es ermöglicht, Abfragen und Verwalten von großen Datasets in verteilten Speicher mithilfe einer SQL-ähnliche Sprache namens HiveQL erstellt. Struktur, wie Schwein, ist eine Abstraktion auf MapReduce. Beim Ausführen übersetzt Struktur Abfragen in eine Reihe von MapReduce Aufträge an. Struktur ist im Prinzip näher an einer relationalen Datenbank Management-System als Schwein und daher für die Verwendung mit mehr strukturierte Daten geeignet ist. Schwein ist unstrukturierte Daten die besser geeignet. [Verwenden Sie die Struktur mit Hadoop in HDInsight](hdinsight-use-hive.md)finden Sie unter.

<a target="_blank" href="https://cwiki.apache.org/confluence/display/Hive/HCatalog/">Apache HCatalog</a> stellt eine Tabelle und Speicher Management für Hadoop, das mit einem relationale Ansicht der Daten bereitstellt. Sie können in HCatalog lesen und Schreiben von Dateien in einem beliebigen Format für die eine Struktur SerDe (Serialisierungsprogramm-überspringt) geschrieben werden können.

### <a name="a-namemahoutamahout"></a><a name="mahout"></a>Mahout

<a target="_blank" href="https://mahout.apache.org/">Apache Mahout</a> ist eine skalierbare Bibliothek des Computers learning Algorithmen, die auf Hadoop ausgeführt werden. Mithilfe von Statistiken Prinzipien, vermitteln maschinellen Learning Applikationen Systeme aus Daten sowie ältere Aufgabenergebnisse verwenden, um zukünftigen Verhalten zu bestimmen. Finden Sie unter [generieren Film Empfehlungen Mahout auf Hadoop verwenden](hdinsight-mahout.md).

### <a name="a-namemapreduceamapreduce"></a><a name="mapreduce"></a>MapReduce
MapReduce ist das Framework älterer Software für Hadoop zum Schreiben von Applications an die Stapelverarbeitung große Datenmengen parallel Prozess. Ein Auftrag MapReduce wird geteilt großen Datasets und verwaltet die Daten in Schlüssel / Wert-Paare für die Verarbeitung.

[Aus](#yarn) der Manager und Anwendung Framework Hadoop nächsten Generation Ressource ist und MapReduce 2.0 genannt wird. MapReduce Aufträge ausgeführt aus.

Weitere Informationen zum MapReduce finden Sie unter <a target="_blank" href="http://wiki.apache.org/hadoop/MapReduce">MapReduce</a> im Hadoop Wiki.

### <a name="a-nameoozieaoozie"></a><a name="oozie"></a>Oozie
<a target="_blank" href="http://oozie.apache.org/">Apache Oozie</a> ist ein Workflow Koordinierung-System, das Hadoop Aufträge verwaltet werden. Es ist in den Stapel Hadoop integriert und unterstützt Hadoop Aufträge MapReduce, Schwein, Struktur und Sqoop. Sie können auch zum Planen von Aufträgen eines Systems, wie Java-Programme oder Shellskripts-spezifischen verwendet werden. Finden Sie unter [Verwenden von Oozie mit Hadoop](hdinsight-use-oozie.md).

### <a name="a-namephoenixaphoenix"></a><a name="phoenix"></a>Karlsruhe
<a  target="_blank" href="http://phoenix.apache.org/">Apache Karlsruhe</a> ist einer relationalen Datenbankebene über HBase. Karlsruhe enthält, mit dem Benutzer, Abfragen und Verwalten von SQL-Tabellen direkt-Treiber. Karlsruhe übersetzt Abfragen und anderen Anweisungen in einer systemeigenen NoSQL-API-Aufrufe - statt MapReduce verwenden – wodurch schnellere Applikationen auf NoSQL Stores. Finden Sie unter [verwenden Apache Karlsruhe und Eichhörnchen mit HBase Cluster](hdinsight-hbase-phoenix-squirrel.md).


### <a name="a-namepigapig"></a><a name="pig"></a>Schwein
<a  target="_blank" href="http://pig.apache.org/">Apache Schwein</a> ist eine auf hoher Ebene Plattform, die Sie komplexe MapReduce Transformationen mithilfe von einfachen scripting-Sprache Lateinisch Schwein mit sehr großen Datasets ausführen kann. Schwein übersetzt die Skripts Schwein Lateinisch, damit sie innerhalb Hadoop ausgeführt werden. Sie können benutzerdefinierte Funktionen (Functions, UDFs) erweitern Schwein Lateinisch erstellen. Finden Sie unter [Verwenden von Schwein mit Hadoop](hdinsight-use-pig.md).

### <a name="a-namesqoopasqoop"></a><a name="sqoop"></a>Sqoop
<a  target="_blank" href="http://sqoop.apache.org/">Apache Sqoop</a> ist Tool, dass die Übertragung von Daten zwischen Hadoop und relationalen Datenbanken solche einer SQL oder andere Stores strukturierte Daten möglichst effiziente gruppenweise. Finden Sie unter [Verwenden von Sqoop mit Hadoop](hdinsight-use-sqoop.md).

### <a name="a-nametezatez"></a><a name="tez"></a>Tez
<a  target="_blank" href="http://tez.apache.org/">Apache Tez</a> ist eine Anwendungsframework integriert auf Hadoop aus, die komplexe, azyklische Diagramme der allgemeinen Datenverarbeitung ausgeführt wird. Es ist eine flexiblere und leistungsfähige Nachfolger MapReduce Rahmen, der Daten ankommt Prozesse, z. B. Struktur, an, die bei effizienter ausführen kann. Finden Sie unter ["Verwenden Apache Tez zum Verbessern der Leistung" in Struktur verwenden und HiveQL](hdinsight-use-hive.md#usetez).

### <a name="a-nameyarnayarn"></a><a name="yarn"></a>AUS
Apache aus ist die nächste Generation der MapReduce (MapReduce 2.0 oder MRv2) und Datenverarbeitung Szenarien mit mehr als MapReduce Stapelverarbeitung mit breiter skalierbar und in Echtzeit Verarbeitung unterstützt. AUS bietet ressourcenverwaltung und einer verteilten Anwendungsframework. MapReduce Aufträge ausgeführt aus.

Weitere Informationen zu aus finden Sie unter <a target="_blank" href="http://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html">Apache Hadoop NextGen MapReduce (aus)</a>.


### <a name="a-namezookeeperazookeeper"></a><a name="zookeeper"></a>ZooKeeper
<a  target="_blank" href="http://zookeeper.apache.org/">Apache ZooKeeper</a> koordiniert Prozesse in großen verteilten Systemen über einen freigegebenen hierarchische Namespace der Datenregister mit (Znodes). Znodes enthalten, um geringe Mengen von Metatag Informationen zum koordinieren Prozesse erforderlich sind: Status, Speicherort, Konfiguration und So weiter.

## <a name="programming-languages-on-hdinsight"></a>Programmierung Sprachen auf HDInsight

HDInsight Zuordnungseinheiten, d.h. Hadoop, HBase, Storm und Spark Zuordnungseinheiten, d.h. unterstützt eine Reihe von programming Sprachen, jedoch einige werden nicht standardmäßig installiert. Verwenden Sie für Bibliotheken, Module oder Pakete standardmäßig nicht installiert werden eine Skriptaktion zum Installieren der Komponente ein. Finden Sie unter [Skript Aktion Entwicklung mit HDInsight](hdinsight-hadoop-script-actions-linux.md).

### <a name="default-programming-language-support"></a>Unterstützung für Sprachen programming Standard

Standardmäßig Cluster HDInsight Support:

* Java

* Python

Zusätzliche Sprachen können mithilfe von Skript-Aktionen installiert werden: [Skript Aktion Entwicklung mit HDInsight](hdinsight-hadoop-script-actions-linux.md).

### <a name="java-virtual-machine-jvm-languages"></a>Java-virtuellen Computern (JVM) Sprachen

Vielen anderen Sprachen als Java können mithilfe eines Java virtuellen Computers (JVM); ausgeführt werden Einige dieser Sprachen ausgeführt erfordern jedoch möglicherweise weitere Komponenten im Cluster installiert.

Diese JVM-basierten Sprachen werden auf HDInsight Cluster unterstützt:

* Clojure

* Jython (Python für Java)

* Scala

### <a name="hadoop-specific-languages"></a>Hadoop-spezifische Sprachen

HDInsight Cluster unterstützen die folgenden Sprachen, die Teil des Hadoop Netzwerks spezifisch sind:

* Schwein Lateinisch für Schwein Aufträge

* HiveQL für Aufträge Struktur und SparkSQL


## <a name="a-nameadvantageaadvantages-of-hadoop-in-the-cloud"></a><a name="advantage"></a>Vorteile von Hadoop in der cloud

Als Teil der Azure-Cloud-Netz bietet Hadoop in HDInsight eine Reihe von Vorteilen, zwischen ihnen:

* Automatische Bereitstellung von Hadoop Cluster. HDInsight Zuordnungseinheiten sind viel einfacher, als manuell konfigurieren Hadoop Cluster erstellen. Details finden Sie unter [Bereitstellen von Hadoop Cluster in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

* Die Art Hadoop Komponenten. Details finden Sie unter [Hadoop Komponenten, versionsverwaltung, und Service-Angebote in HDInsight][component-versioning].

* Hohe Verfügbarkeit und Zuverlässigkeit der Cluster. Weitere Informationen finden Sie unter [Verfügbarkeit und Zuverlässigkeit der Hadoop Cluster in HDInsight](hdinsight-high-availability-linux.md) .

* Effizient und preisgünstige Datenspeicher mit Azure Blob-Speicher, eine Hadoop kompatiblen aus. Details finden Sie unter [verwenden Azure Blob-Speicher mit Hadoop in HDInsight](hdinsight-hadoop-use-blob-storage.md) .

* Die Integration mit anderen Azure-Diensten, einschließlich [Web apps](https://azure.microsoft.com/documentation/services/app-service/web/) und [SQL-Datenbank](https://azure.microsoft.com/documentation/services/sql-database/).

* Zusätzliche virtueller Computer Größen und Datentypen für HDInsight Cluster ausgeführt. Finden Sie unter [Hadoop Komponenten, versionsverwaltung, und Service-Angebote in HDInsight] [ component-versioning] Details.

* Skalierung gruppiert werden. Cluster Skalierung ermöglicht es Ihnen so ändern Sie die Anzahl der Knoten einer laufenden HDInsight Cluster ohne zu löschen oder neu erstellen.

* Virtuelle Netzwerk-Support. HDInsight Cluster können mit Azure-virtuellen Netzwerk zur Unterstützung von Isolation Cloudressourcen oder Hybrid Szenarien, die Cloudressourcen, mit denen Ihr Datencenter verknüpft verwendet werden.

* Niedrig Eintrag Kosten. Starten Sie eine [kostenlose Testversion](https://azure.microsoft.com/free/), oder wenden Sie sich an [HDInsight Preisgestaltung Details](https://azure.microsoft.com/pricing/details/hdinsight/).

Weitere Informationen zu den Vorteilen auf Hadoop in HDInsight, finden Sie auf der [Seite Azure-Features für HDInsight][marketing-page].

## <a name="hdinsight-standard-and-hdinsight-premium"></a>HDInsight Standard- und HDInsight Premium

HDInsight bietet Cloud-Angebote für große Daten in zwei Kategorien, Standard- und Premium. HDInsight Standard stellt einen unternehmensweite Cluster, den Organisationen verwenden können, um ihre Daten große Auslastung auszuführen. HDInsight Premium auf, die erstellt und stellt erweiterte analytical und Sicherheitsfunktionen, die für einen Cluster HDInsight. Weitere Informationen finden Sie unter [Azure HDInsight Premium](hdinsight-component-versioning.md#hdinsight-standard-and-hdinsight-premium)


## <a name="a-idresourcesaresources-for-learning-more-about-big-data-analysis-hadoop-and-hdinsight"></a><a id="resources"></a>Ressourcen für Weitere Informationen zu groß Datenanalyse, Hadoop und HDInsight

In dieser Einführung in die Cloud und große Datenanalysen mit den folgenden Ressourcen Hadoop erstellen.


### <a name="hadoop-documentation-for-hdinsight"></a>Hadoop-Dokumentation HDInsight

* [HDInsight Dokumentation](https://azure.microsoft.com/documentation/services/hdinsight/): der Dokumentationsseite für Hadoop auf Azure HDInsight mit Links zu Artikeln, Videos und weitere Ressourcen.

* [Erste Schritte mit Hadoop in HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md): ein Schnellstart-Lernprogramm für HDInsight Hadoop Cluster bereitgestellt und Beispiele für Struktur Abfragen ausgeführt.

* [Erste Schritte mit Spark in HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md): ein Schnellstart-Lernprogramm für einen Cluster Spark erstellen und Ausführen von interaktiven Spark SQL-Abfragen.

* [Verwenden von R Server auf HDInsight](hdinsight-hadoop-r-server-get-started.md): Erste Schritte mit R Server in HDInsight Premium.

* [Bereitstellen von HDInsight Cluster](hdinsight-hadoop-provision-linux-clusters.md): erfahren Sie, wie einen HDInsight Hadoop Cluster erfolgt über die Azure-Portal, Azure CLI oder Azure PowerShell nicht bereitstellen.


### <a name="apache-hadoop"></a>Hadoop Apache

* <a target="_blank" href="http://hadoop.apache.org/">Hadoop Apache</a>: erfahren Sie mehr über die Apache Hadoop Softwarebibliothek, einen Rahmen, die über Cluster der Computer die verteilte Verarbeitung von großen Datasets ermöglicht.

* <a target="_blank" href="http://hadoop.apache.org/docs/r1.0.4/hdfs_design.html">HDFS</a>: erfahren Sie mehr über die Architektur und den Entwurf von der Hadoop Distributed File System, das die primäre Speichersystem von Applications Hadoop verwendet.

* <a target="_blank" href="http://hadoop.apache.org/docs/r1.2.1/mapred_tutorial.html">MapReduce Lernprogramm</a>: erfahren Sie mehr über die Programmierung Framework zum Schreiben von Hadoop-Anwendungen, die sich schnell große Datenmengen parallel auf einem großen Cluster von Computeknoten verarbeiten.


### <a name="microsoft-business-intelligence"></a>Microsoft Business intelligence

Vertraute Business Intelligence (BI)-Tools – wie Excel, PowerPivot, SQL Server Analysis Services und SQL Server Reporting Services - abrufen, analysieren und Melden von Daten mit HDInsight mithilfe des Power Query-add-Ins oder Microsoft Struktur ODBC-Treiber integriert.

Mithilfe dieser BI-Tools können in einer großen Datenanalyse können:

* [Verbinden von Excel mit Hadoop mit Power Query](hdinsight-connect-excel-power-query.md): erfahren Sie, wie in Excel zu den Azure-Speicher-Konto herzustellen, das Daten Ihrer HDInsight Cluster zugeordnet sind, mithilfe von Microsoft Power Query für Excel speichert. Windows-Arbeitsstationen erforderlich. Funktioniert mit Windows oder Linux-basierten Cluster.

* [Verbinden von Excel mit Hadoop mit Microsoft Struktur ODBC-Treiber](hdinsight-connect-excel-hive-odbc-driver.md): Informationen zum Importieren von Daten aus HDInsight mit Microsoft Struktur ODBC-Treiber. Windows-Arbeitsstationen erforderlich. Funktioniert mit Windows oder Linux-basierten Cluster.

* [Microsoft-Cloud-Plattform](http://www.microsoft.com/server-cloud/solutions/business-intelligence/default.aspx): Informationen zu Power BI für Office 365, die SQL Server-Testversion herunterladen und Einrichten von SharePoint Server 2013 und SQL Server BI.

* [SQL Server Analysis Services](http://msdn.microsoft.com/library/hh231701.aspx).

* [SQLServer Reporting Services](http://msdn.microsoft.com/library/ms159106.aspx).




[marketing-page]: https://azure.microsoft.com/services/hdinsight/
[component-versioning]: hdinsight-component-versioning.md
[zookeeper]: http://zookeeper.apache.org/

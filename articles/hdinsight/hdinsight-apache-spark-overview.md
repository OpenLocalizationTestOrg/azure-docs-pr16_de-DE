<properties 
    pageTitle="Übersicht über Apache Spark in HDInsight | Microsoft Azure" 
    description="Eine Einführung in Apache Spark in HDInsight und Szenarien, in denen Spark auf HDInsight in Ihrer Anwendung verwenden." 
    services="hdinsight" 
    documentationCenter="" 
    authors="nitinme" 
    manager="jhubbard" 
    editor="cgronlun"
    tags="azure-portal"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/25/2016" 
    ms.author="nitinme"/>

# <a name="overview-apache-spark-on-hdinsight-linux"></a>Übersicht: Apache Spark auf HDInsight Linux
 
<a href="http://spark.apache.org/" target="_blank">Apache Spark</a> ist ein offener Quelle Parallel Verarbeitung Framework, das in-Memory Verarbeitung, um die Leistung der analytischen Anwendung große Daten zu verbessern. Spark Verarbeitung-Engine wird für Geschwindigkeit, erleichterte Bedienung verwenden und anspruchsvolle Analytics erstellt. Funktionen, die in-Memory Berechnung des Spark machen Sie es eine gute Wahl für iterative Algorithmen in Computer lernen und Graph Berechnungen. Spark ist auch kompatibel mit Azure Blob-Speicher (WASB), damit die vorhandenen Daten gehörende Kehrmatrix Azure einfach über Spark verarbeitet werden können.

Wenn Sie einen Cluster Spark in HDInsight erstellen, erstellen Sie Azure berechnen Ressourcen mit Spark installiert und konfiguriert. Es dauert nur ungefähr zehn Minuten, um zu einen Cluster Spark in HDInsight zu erstellen. Die zu verarbeitenden Daten werden in Azure Blob Storage gespeichert. Finden Sie unter [Verwenden von Azure BLOB-Speicher mit HDInsight][hdinsight-storage].

![Apache Spark auf Azure HDInsight] (./media/hdinsight-apache-spark-overview/hdispark.architecture.png  "Apache Spark auf Azure HDInsight")


**Möchten Sie den ersten Schritten mit Apache Spark auf Azure HDInsight?** Finden Sie unter [Schnellstart: einen Spark Cluster auf HDInsight Linux erstellen und Ausführen Stichprobe Applikationen mit Jupyter](hdinsight-apache-spark-jupyter-spark-sql.md).

>[AZURE.NOTE] Eine Liste der bekannten Probleme und Einschränkungen mit der aktuellen Version finden Sie unter [bekannte Probleme von Apache Spark in Azure HDInsight (Linux)](hdinsight-apache-spark-jupyter-spark-sql.md).


## <a name="why-use-spark-on-azure-hdinsight"></a>Gründe für das Verwenden von Spark auf Azure HDInsight 

Azure HDInsight bietet einen vollständig verwalteten Spark-Dienst an. Vorteile der Verwendung von Spark auf HDInsight sind:

| Feature                             | Beschreibung       |
|-------------------------------------|-------------------|
| Erstellen von Cluster für erleichterte Bedienung            | Sie können einen neuen Spark Cluster auf HDInsight in Minuten mithilfe der Azure-Verwaltungsportal, Azure PowerShell oder HDInsight .NET SDK erstellen. Finden Sie unter [Erste Schritte mit Spark Cluster in HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md) |
| Center für erleichterte Bedienung                     | Spark in HDInsight Cluster enthält Jupyter Notizbücher vorkonfiguriert ist. Sie können diese für interaktive Datenverarbeitung und Visualisierung. Die URLs für die https://CLUSTERNAME.azurehdinsight.net/jupyter ist. Ersetzen Sie __CLUSTERNAME__ mit dem Namen der Spark HDInsight Cluster ein.|
| REST-APIs                       | Spark in HDInsight umfasst [Livius](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server), eine REST-API Grundlage Spark Auftragsserver, um Remote senden und laufenden Aufträge überwachen. |
| Unterstützung für Azure Lake Datenspeicher | Spark auf HDInsight kann Azure Lake Datenspeicher als einer zusätzlichen Speicher verwenden konfiguriert werden. Weitere Informationen zum Lake Datenspeicher finden Sie unter [Übersicht der Azure dem Datenspeicher](../data-lake-store/data-lake-store-overview.md).
| Die Integration mit Azure services | Klicken Sie auf HDInsight Spark im Lieferumfang von eines Verbinders an Azure Ereignis Hubs. Kunden ganz streaming Applikationen mit den Ereignis untergeordneten Servern, sowie [Kafka](http://kafka.apache.org/), die bereits zur Verfügung steht im Rahmen der Spark. |
| Unterstützung für R Server  | Sie können einen Server R HDInsight Spark Cluster eingerichtet, verteilte R Berechnungen mit der Geschwindigkeit, mit einem Cluster Spark Zugesagtes ausführen. Weitere Informationen finden Sie unter [Erste Schritte mit R Server auf HDInsight](hdinsight-hadoop-r-server-get-started.md).   |
| Integration in IntelliJ IDEE  | Sie können das HDInsight Plugin für IntelliJ zum Erstellen und Übermitteln von Applications zu einem HDInsight Spark Cluster verwenden. Weitere Informationen finden Sie unter [Verwenden HDInsight Tools-Plug-In für IntelliJ IDEE Spark Applikationen für HDInsight Spark Linux Cluster erstellen](hdinsight-apache-spark-intellij-tool-plugin.md). |
| Gleichzeitige Abfragen              | Spark in HDInsight unterstützt gleichzeitige Abfragen. Dies ermöglicht mehreren Abfragen aus einen Benutzer oder mehreren Abfragen aus verschiedenen Benutzern und Applikationen die gleichen Clusterressourcen gemeinsam nutzen. |
| Klicken Sie auf SSDs Zwischenspeichern                 | Sie können zum Zwischenspeichern von Daten im Speicher oder in dem Cluster Knoten angefügt SSDs auswählen. Im Speicher Zwischenspeichern bietet die optimale Leistung von Abfragen, aber könnte teure; Zwischenspeichern in SSDs bietet eine gute Option zum Verbessern der abfrageleistung, ohne dass einen Cluster mit einer Größe zu erstellen, die erforderlich ist, um das gesamte Dataset im Speicher anpassen.|
| Die Integration mit BI-Tools       | Spark für HDInsight bietet Connectors für BI-Tools wie [Power BI](http://www.powerbi.com/) und [Tableaus](http://www.tableau.com/products/desktop) für Daten Analytics.|
| Vorab geladen Anaconda-Bibliotheken        | Spark auf HDInsight Cluster im Zusammenhang mit Anaconda Bibliotheken vorinstalliert ist. [Anaconda](http://docs.continuum.io/anaconda/) bietet ähnlich 200 Bibliotheken für maschinelle Schulung, Datenanalyse, Visualisierung.|
| Skalierbarkeit                     | Obwohl Sie die Anzahl der Knoten im Cluster während der Erstellung angeben können, sollten Sie vergrößert oder verkleinert Cluster Arbeitsbelastung übereinstimmt. Alle HDInsight Cluster können Sie die Anzahl der Knoten im Cluster ändern. Darüber hinaus können Spark Cluster gelöscht ohne Verlust von Daten, da alle Daten in Azure Blob Storage gespeichert ist. |
| 24/7-Unterstützung                    | Spark auf HDInsight verfügt über 24/7-Unterstützung für Enterprise-Ebene und eine Vereinbarung zum SERVICELEVEL 99,9 % nach-oben-Zeit.|



## <a name="what-are-the-use-cases-for-spark-on-hdinsight"></a>Was sind die Fällen wird Spark auf HDInsight?

Apache Spark in HDInsight ermöglicht die folgenden Schlüsselszenarien.

### <a name="interactive-data-analysis-and-bi"></a>Interaktive Datenanalyse und BI

[Schauen Sie sich ein Lernprogramm](hdinsight-apache-spark-use-bi-tools.md)

Apache Spark in HDInsight speichert Daten in Azure-Blobs an. Business-Experten können wichtigsten Entscheidungsträger analysieren und Erstellen von Berichten über die Daten und Verwenden von Microsoft Power BI Erstellen interaktiver Berichte aus den analysierten Daten. Analysten können aus unstrukturierten/halb strukturierten Daten in Azure-Speicher starten, definieren ein Schema für die Daten mithilfe von Notizbüchern und erstellen dann Datenmodelle mit Microsoft Power BI. Spark in HDInsight unterstützt auch eine Reihe von Drittanbietern BI-Tools, wie z. B. Tableaus, Qlikview und somit eine ideale Plattform für Daten Analysten, Business-Experten und wichtigsten Entscheidungsträger SAP-Lumira.

### <a name="iterative-machine-learning"></a>Iterative Computer-Schulung

[Schauen Sie sich ein Lernprogramm: Vorhersagen erstellen Temperaturen Einzelhandelsabonnements HKL-Daten](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

[Schauen Sie sich ein Lernprogramm: Lebensmittel Prüfungsergebnissen Vorhersagen](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

Apache Spark im Lieferumfang von [MLlib](http://spark.apache.org/mllib/), einem Computer learning-Bibliothek auf Spark erstellt. Darüber hinaus enthält Spark auf HDInsight auch Anaconda, eine Python-Verteilung mit einer Vielzahl von Paketen für maschinelle Schulung. Verbinden Sie diese mit einer integrierten Unterstützung für Jupyter Notizbücher, und Sie haben eine Umgebung Anfang der Zeile für den maschinellen Learning Applications erstellen.  

### <a name="streaming-and-real-time-data-analysis"></a>Streaming und in Echtzeit Datenanalyse

[Schauen Sie sich ein Lernprogramm](hdinsight-apache-spark-eventhub-streaming.md)

Datenanalyse in Echtzeit wird für Szenarien, die im Bereich von verringert die Daten Einblicke durch die Verarbeitung von Daten, wie es zum Erstellen einer Lösung streaming WAHR eingeht, verwendet. Spark in HDInsight bietet eine umfassende Unterstützung für die Erstellung von Lösungen in Echtzeit Analytics. Während Spark bereits Verbinder, um Daten aus mehreren Quellen wie Kafka, Flume, Twitter, ZeroMQ oder TCP-Sockets Aufnahme hat, fügt Spark in HDInsight erster Klasse Unterstützung für die Daten aus Azure Ereignis Hubs Aufnahme hinzu. Ereignis Hubs sind die am häufigsten verwendete Warteschlange Dienst auf Azure. Mit einer Out-of-Box-Unterstützung für Ereignis Hubs aufzeigen ist in HDInsight eine ideale Plattform für Echtzeit Analytics Verkaufspipeline erstellen.

##<a name="a-namenext-stepsawhat-components-are-included-as-part-of-a-spark-cluster"></a><a name="next-steps"></a>Welche Komponenten sind in einem Cluster Spark enthalten?

Spark in HDInsight umfasst die folgenden Komponenten, die auf die Cluster standardmäßig verfügbar sind.

- [Spark Core](https://spark.apache.org/docs/1.5.1/). Enthält Spark Core, Spark SQL, Spark streaming-APIs, GraphX und MLlib.
- [Anaconda](http://docs.continuum.io/anaconda/)
- [Livius](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server)
- [Jupyter Notizbuch](https://jupyter.org)

Außerdem stellt Spark in HDInsight einen [ODBC-Treiber](http://go.microsoft.com/fwlink/?LinkId=616229) für Konnektivität in Spark Cluster in HDInsight von BI-Tools, wie etwa Microsoft Power BI und Tableaus.

## <a name="where-do-i-start"></a>Wo beginnen ich?

Beginnen Sie mit einen Cluster Spark HDInsight Linux erstellen. Finden Sie unter [Schnellstart: einen Spark Cluster auf HDInsight Linux erstellen und Ausführen Stichprobe Applikationen mit Jupyter](hdinsight-apache-spark-jupyter-spark-sql.md). 

## <a name="next-steps"></a>Nächste Schritte

### <a name="scenarios"></a>Szenarien

* [Spark mit BI: Ausführen interaktiven Datenanalyse mithilfe von Spark in HDInsight mit BI-Tools](hdinsight-apache-spark-use-bi-tools.md)

* [Spark mit maschinellen Schulung: Verwenden Sie Spark in HDInsight zum Analysieren von Gebäude Temperatur HKL-Daten verwenden](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

* [Spark mit maschinellen Schulung: verwenden Spark in HDInsight Lebensmittel Prüfungsergebnissen Vorhersagen](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

* [Spark Streaming: Verwenden Sie Spark in HDInsight zum Erstellen von in Echtzeit streaming Clientanwendungen](hdinsight-apache-spark-eventhub-streaming.md)

* [Website-Protokoll-Datenanalyse mithilfe von Spark in HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Erstellen und Ausführen von applications

* [Erstellen Sie eine eigenständige Anwendung Scala](hdinsight-apache-spark-create-standalone-application.md)

* [Führen Sie Aufträge Remote auf einem Spark Cluster Livius verwenden](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Tools und Erweiterungen

* [Verwenden Sie zum Erstellen und übermitteln Spark Scala Applikationen HDInsight Tools-Plug-In für IntelliJ IDEE](hdinsight-apache-spark-intellij-tool-plugin.md)

* [Verwenden von HDInsight Tools-Plug-In für IntelliJ IDEE Spark Applikationen Remote-Debuggen](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)

* [Verwenden von Zeppelin Notizbücher mit einem Spark Cluster auf HDInsight](hdinsight-apache-spark-use-zeppelin-notebook.md)

* [Kernels für Jupyter Notizbuch in Spark Cluster für HDInsight verfügbar](hdinsight-apache-spark-jupyter-notebook-kernels.md)

* [Verwenden von externen Paketen mit Jupyter-Notizbüchern](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

* [Jupyter auf Ihrem Computer installieren und Verbinden mit einem HDInsight Spark cluster](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Verwalten von Ressourcen

* [Verwalten von Ressourcen für den Apache Spark Cluster in Azure HDInsight](hdinsight-apache-spark-resource-manager.md)

* [Verfolgen und Debuggen Aufträge in einem Apache Spark Cluster in HDInsight](hdinsight-apache-spark-job-debugging.md)


[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

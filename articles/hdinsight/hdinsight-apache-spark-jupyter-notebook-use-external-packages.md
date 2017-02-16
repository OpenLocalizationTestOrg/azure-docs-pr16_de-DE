<properties 
    pageTitle="Verwenden von externen Paketen mit Jupyter Notizbücher in Apache Spark Cluster auf HDInsight | Azure"
    description="Eine schrittweise Anleitung zum Konfigurieren von Jupyter Notizbücher verfügbar mit HDInsight Spark Cluster externe Spark Pakete verwenden." 
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
    ms.topic="article" 
    ms.date="10/28/2016" 
    ms.author="nitinme"/>


# <a name="use-external-packages-with-jupyter-notebooks-in-apache-spark-clusters-on-hdinsight-linux"></a>Verwenden von externen Paketen mit Jupyter Notizbücher in Apache Spark Cluster auf HDInsight Linux

>[AZURE.NOTE] In diesem Artikel gilt für Spark 1.5.2 auf HDInsight 3.3 und Spark 1.6.2 auf HDInsight 3.4. 

Informationen zum Konfigurieren eines Notizbuchs Jupyter Apache Spark Cluster auf HDInsight (Linux) mit externen, Community beigetragen Pakete, die nicht im Lieferumfang von Out-of-Box im Cluster. 

Sie können das [Maven Repository](http://search.maven.org/) für die vollständige Liste der Pakete suchen, die verfügbar sind. Sie können auch eine Liste der verfügbaren Paketen aus anderen Quellen abrufen. Eine vollständige Liste der Community beigetragen Pakete beträgt beispielsweise [Spark Pakete](http://spark-packages.org/)verfügbar.

In diesem Artikel erfahren Sie, wie das [Spark-Csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) -Paket mit dem Notizbuch Jupyter verwendet werden.

##<a name="prerequisites"></a>Erforderliche Komponenten

Sie müssen die folgenden:

- Ein Azure-Abonnement. Finden Sie [kostenlose Testversion Azure abrufen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Eine Apache Spark Cluster auf HDInsight Linux. Anweisungen finden Sie unter [Erstellen von Apache Spark Cluster in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="use-external-packages-with-jupyter-notebooks"></a>Verwenden von externen Paketen mit Jupyter-Notizbüchern 

1. Klicken Sie auf die Kachel für Ihren Cluster Spark, aus dem [Azure-Portal](https://portal.azure.com/), mithilfe der Startboard (Wenn Sie es an die Startboard angeheftet). Sie können auch navigieren Sie zu Ihren Cluster unter **Alle durchsuchen** > **HDInsight Cluster**.   

2. Klicken Sie aus dem Spark Cluster Blade auf **Quicklinks**, und klicken Sie dann aus dem Blade **Cluster Dashboard** auf **Jupyter Notizbuch**. Wenn Sie dazu aufgefordert werden, geben Sie die Administrator-Anmeldeberechtigungen für den Cluster aus.

    > [AZURE.NOTE] Sie möglicherweise auch das Notizbuch Jupyter für Ihren Cluster erreichen, indem Sie den folgenden URL in Ihrem Browser öffnen. Ersetzen Sie __CLUSTERNAME__ mit dem Namen der Cluster aus:
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Erstellen eines neuen Notizbuchs an. Klicken Sie auf **neu**, und klicken Sie dann auf **Spark**.

    ![Erstellen eines neuen Jupyter Notizbuchs] (./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/hdispark.note.jupyter.createnotebook.png "Erstellen eines neuen Jupyter Notizbuchs")

3. Ein neues Notizbuch erstellt und mit dem Namen Untitled.pynb geöffnet. Klicken Sie auf den Notizbuchnamen oben, und geben Sie einen Anzeigenamen ein.

    ![Bereitstellen einen Namen für das Notizbuch] (./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/hdispark.note.jupyter.notebook.name.png "Bereitstellen einen Namen für das Notizbuch")

4. Verwenden Sie die `%%configure` magische so konfigurieren Sie das Notizbuch, um eine externe Paket verwenden. Notizbücher, die externe Pakete verwenden, stellen Sie sicher, rufen Sie die `%%configure` in der ersten Zelle der Code magische. Dadurch wird sichergestellt, dass der Kernel konfiguriert ist, um das Paket zu verwenden, bevor die Sitzung gestartet wird.

        %%configure
        { "packages":["com.databricks:spark-csv_2.10:1.4.0"] }


    >[AZURE.IMPORTANT] Wenn Sie vergessen, in der ersten Zelle den Kernel zu konfigurieren, können Sie mithilfe der `%%configure` mit den `-f` Parameter, aber, die neu die Sitzung gestartet, und alle Fortschritt gehen verloren.

5. In der obigen Codeausschnitt `packages` erwartet wird eine Liste der Maven Koordinaten in Maven zentralen Repository gespeichert. In diesem Codeausschnitt `com.databricks:spark-csv_2.10:1.4.0` ist die Maven Koordinate für **Spark-Csv** -Paket. Hier sind, wie Sie die Koordinaten für ein Paket erstellen.

    ein. Suchen Sie im Repository Maven das Paket ein. In diesem Lernprogramm verwenden wir [Spark-Csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).
    
    b. Sammeln Sie die Werte aus dem Repository für die **Gruppen-ID**, **ArtifactId**und **Version**.

    ![Verwenden von externen Pakete Jupyter Notizbuch] (./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/use-external-packages-with-jupyter.png "Verwenden von externen Pakete Jupyter Notizbuch")

    c. Verketten Sie die drei Werte, die von einem Doppelpunkt (**:**) getrennt ein.

        com.databricks:spark-csv_2.10:1.4.0

6. Führen Sie die Zelle Code mit der `%%configure` magische. Dadurch wird die zugrunde liegende Livius Sitzung, um das Paket zu verwenden, die, das Sie zur Verfügung gestellt, konfiguriert. In der nachfolgenden Zellen im Notizbuch können Sie jetzt das Paket verwenden, wie unten dargestellt.

        val df = sqlContext.read.format("com.databricks.spark.csv").
        option("header", "true").
        option("inferSchema", "true").
        load("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

7. Anschließend können Sie die Codeausschnitte ausführen, wie unten, um die Daten aus der Dataframe anzuzeigen, Sie im vorherigen Schritt erstellt, gezeigt.

        df.show()

        df.select("Time").count()


## <a name="a-nameseealsoasee-also"></a><a name="seealso"></a>Siehe auch


* [Übersicht: Apache Spark auf Azure HDInsight](hdinsight-apache-spark-overview.md)

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

* [Kernels für Jupyter-Notizbuch in Spark Cluster für HDInsight verfügbar](hdinsight-apache-spark-jupyter-notebook-kernels.md)

* [Jupyter auf Ihrem Computer installieren und Verbinden mit einem HDInsight Spark cluster](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Verwalten von Ressourcen

* [Verwalten von Ressourcen für den Apache Spark Cluster in Azure HDInsight](hdinsight-apache-spark-resource-manager.md)

* [Verfolgen und Debuggen Aufträge in einem Apache Spark Cluster in HDInsight](hdinsight-apache-spark-job-debugging.md)

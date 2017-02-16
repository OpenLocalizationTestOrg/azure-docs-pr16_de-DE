<properties 
    pageTitle="Verwenden Sie Apache Spark maschinellen Learning Applikationen auf HDInsight erstellen | Microsoft Azure" 
    description="Eine schrittweise Anleitung zum Verwenden von Notizbüchern mit Apache Spark maschinellen Learning Applications erstellen" 
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
    ms.date="10/05/2016" 
    ms.author="nitinme"/>


# <a name="build-machine-learning-applications-to-run-on-apache-spark-clusters-on-hdinsight-linux"></a>Erstellen von maschinellen Learning Applikationen für Apache Spark Cluster auf HDInsight Linux ausgeführt

Informationen Sie zum Erstellen von eines Computers mit einem Apache Spark Cluster in HDInsight Anwendung erlernen. In diesem Artikel veranschaulicht, wie das Jupyter Notizbuch verfügbar mit dem Cluster zu erstellen und testen die Anwendung. Die Anwendung verwendet die HVAC.csv Beispieldaten, die auf alle Cluster standardmäßig zur Verfügung.

**Voraussetzungen für:**

Sie müssen die folgenden:

- Ein Azure-Abonnement. Finden Sie [kostenlose Testversion Azure abrufen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Eine Apache Spark Cluster auf HDInsight Linux. Anweisungen finden Sie unter [Erstellen von Apache Spark Cluster in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md). 

##<a name="a-namedataashow-me-the-data"></a><a name="data"></a>Anzeigen der Daten

Bevor Sie beginnen die Anwendung erstellen, verstehen Sie lassen Sie uns die Struktur der Daten und die Art der Analyse, die wir auf der Registerkarte Daten ausführen können. 

In diesem Artikel verwenden wir die Stichprobe **HVAC.csv** -Datendatei, die in dem Konto Azure-Speicher zur Verfügung, die Sie mit dem Cluster HDInsight verknüpft. In dem Speicherkonto befindet sich die Datei am **\HdiSamples\HdiSamples\SensorSampleData\hvac**. Herunterladen Sie und öffnen Sie die CSV-Datei, um eine Momentaufnahme der Daten zu erhalten.  

![HKL-Daten-snapshot] (./media/hdinsight-apache-spark-ipython-notebook-machine-learning/hdispark.ml.show.data.png "Snapshot der HKL-Daten")

Die Daten zeigen die Ziel-Temperatur und die tatsächliche Temperatur von Gebäuden, die HKL-Betriebssystemen installiert wurde. Angenommen, die **System** Spalte die System-ID und die **SystemAge** Spalte darstellt die Anzahl der Jahre, in denen, die das HKL-System direkte bei der Erstellung wurde.

Wir verwenden diese Daten zum prognostizieren, ob ein Gebäude heißeren oder colder basierend auf der Ziel-Temperatur, ausgehend von einer System-ID und das System Alter angezeigt wird.

##<a name="a-nameappawrite-a-machine-learning-application-using-spark-mllib"></a><a name="app"></a>Schreiben Sie eine Computer-Learning-Anwendung Spark MLlib verwenden

In dieser Anwendung verwenden wir eine Spark ML Verkaufspipeline zum Ausführen einer Klassifizierung von Dokumenten aus. In der Verkaufspipeline wir Teilen des Dokuments in Wörter, die Wörter in einem Vektor numerischen Feature konvertieren und schließlich eine Vorhersagemodell, das mit dem Feature Vektoren und Etiketten erstellen. Führen Sie die folgenden Schritte aus, um die Anwendung zu erstellen.

1. Klicken Sie auf die Kachel für Ihren Cluster Spark, aus dem [Azure-Portal](https://portal.azure.com/), mithilfe der Startboard (Wenn Sie es an die Startboard angeheftet). Sie können auch navigieren Sie zu Ihren Cluster unter **Alle durchsuchen** > **HDInsight Cluster**.   

2. Klicken Sie aus dem Spark Cluster Blade auf **Cluster Dashboard**, und klicken Sie dann auf **Jupyter Notizbuch**. Wenn Sie dazu aufgefordert werden, geben Sie die Administrator-Anmeldeberechtigungen für den Cluster aus.

    > [AZURE.NOTE] Sie möglicherweise auch das Notizbuch Jupyter für Ihren Cluster erreichen, indem Sie den folgenden URL in Ihrem Browser öffnen. Ersetzen Sie __CLUSTERNAME__ mit dem Namen der Cluster aus:
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Erstellen eines neuen Notizbuchs an. Klicken Sie auf **neu**, und klicken Sie dann auf **PySpark**.

    ![Erstellen eines neuen Jupyter Notizbuchs] (./media/hdinsight-apache-spark-ipython-notebook-machine-learning/hdispark.note.jupyter.createnotebook.png "Erstellen eines neuen Jupyter Notizbuchs")

3. Ein neues Notizbuch erstellt und mit dem Namen Untitled.pynb geöffnet. Klicken Sie auf den Notizbuchnamen oben, und geben Sie einen Anzeigenamen ein.

    ![Bereitstellen einen Namen für das Notizbuch] (./media/hdinsight-apache-spark-ipython-notebook-machine-learning/hdispark.note.jupyter.notebook.name.png "Bereitstellen einen Namen für das Notizbuch")

3. Da Sie ein Notizbuch, mit dem die PySpark Kernel erstellt haben, müssen Sie keine Kontexte explizit zu erstellen. Die Kontexte Spark und Struktur werden automatisch für Sie erstellt werden, wenn Sie die erste Zelle der Code ausführen. Sie können Sie zunächst importieren die Typen, die in diesem Szenario erforderlich sind. Fügen Sie den folgenden Codeausschnitt in eine leere Zelle, und drücken Sie dann **UMSCHALT + EINGABETASTE**. 

        from pyspark.ml import Pipeline
        from pyspark.ml.classification import LogisticRegression
        from pyspark.ml.feature import HashingTF, Tokenizer
        from pyspark.sql import Row
        
        import os
        import sys
        from pyspark.sql.types import *
        
        from pyspark.mllib.classification import LogisticRegressionWithSGD
        from pyspark.mllib.regression import LabeledPoint
        from numpy import array
        
        
     
4. Sie müssen jetzt laden Sie die Daten (hvac.csv), analysieren und verwenden, um das Modell Schulen. Zu diesem Zweck definieren Sie eine Funktion, die überprüft, ob die tatsächliche Temperatur Gebäude größer als die Ziel-Temperatur ist. Die tatsächliche Temperatur ist größer, das Gebäude angesagt, durch den Wert **1.0**gekennzeichnet. Ist die tatsächliche Temperatur geringere, ist das Gebäude kalt gekennzeichnet durch den Wert **0,0**. 

    Fügen Sie den folgenden Codeausschnitt in eine leere Zelle, und drücken Sie **UMSCHALT + EINGABETASTE**.

        
        # List the structure of data for better understanding. Becuase the data will be
        # loaded as an array, this structure makes it easy to understand what each element
        # in the array corresponds to

        # 0 Date
        # 1 Time
        # 2 TargetTemp
        # 3 ActualTemp
        # 4 System
        # 5 SystemAge
        # 6 BuildingID

        LabeledDocument = Row("BuildingID", "SystemInfo", "label")

        # Define a function that parses the raw CSV file and returns an object of type LabeledDocument
        
        def parseDocument(line):
            values = [str(x) for x in line.split(',')]
            if (values[3] > values[2]):
                hot = 1.0
            else:
                hot = 0.0        
    
            textValue = str(values[4]) + " " + str(values[5])
    
            return LabeledDocument((values[6]), textValue, hot)

        # Load the raw HVAC.csv file, parse it using the function
        data = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

        documents = data.filter(lambda s: "Date" not in s).map(parseDocument)
        training = documents.toDF()


5. Konfigurieren der Verkaufspipeline Spark maschinellen Schulung, die besteht aus drei Phasen: Tokenizer, HashingTF und Lr. Weitere Informationen zu Was ist eine Verkaufspipeline und deren Funktionsweise finden Sie unter <a href="http://spark.apache.org/docs/latest/ml-guide.html#how-it-works" target="_blank">Spark maschinellen learning Verkaufspipeline</a>.

    Fügen Sie den folgenden Codeausschnitt in eine leere Zelle, und drücken Sie **UMSCHALT + EINGABETASTE**.

        tokenizer = Tokenizer(inputCol="SystemInfo", outputCol="words")
        hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
        lr = LogisticRegression(maxIter=10, regParam=0.01)
        pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])

6. Anpassen der Verkaufspipeline die Schulung Dokument an. Fügen Sie den folgenden Codeausschnitt in eine leere Zelle, und drücken Sie **UMSCHALT + EINGABETASTE**.

        model = pipeline.fit(training)

7. Vergewissern Sie sich dem Dokument Schulung Wissensstand den Fortschritt Ihres mit der Anwendung. Fügen Sie den folgenden Codeausschnitt in eine leere Zelle, und drücken Sie **UMSCHALT + EINGABETASTE**.

        training.show()

    Dies sollte die Ausgabe ähnlich wie der folgende bieten:

        +----------+----------+-----+
        |BuildingID|SystemInfo|label|
        +----------+----------+-----+
        |         4|     13 20|  0.0|
        |        17|      3 20|  0.0|
        |        18|     17 20|  1.0|
        |        15|      2 23|  0.0|
        |         3|      16 9|  1.0|
        |         4|     13 28|  0.0|
        |         2|     12 24|  0.0|
        |        16|     20 26|  1.0|
        |         9|      16 9|  1.0|
        |        12|       6 5|  0.0|
        |        15|     10 17|  1.0|
        |         7|      2 11|  0.0|
        |        15|      14 2|  1.0|
        |         6|       3 2|  0.0|
        |        20|     19 22|  0.0|
        |         8|     19 11|  0.0|
        |         6|      15 7|  0.0|
        |        13|      12 5|  0.0|
        |         4|      8 22|  0.0|
        |         7|      17 5|  0.0|
        +----------+----------+-----+


    Wechseln Sie zurück, und überprüfen Sie die Ausgabe anhand der unformatierten CSV-Datei. Angenommen, hat die erste Zeile der CSV-Datei diese Daten:

    ![HKL-Daten-snapshot] (./media/hdinsight-apache-spark-ipython-notebook-machine-learning/hdispark.ml.show.data.first.row.png "Snapshot der HKL-Daten")

    Beachten Sie, dass die tatsächliche Temperatur kleiner als Ziel Temperatur Lösungsvorschläge gemacht werden, dass das Gebäude kalt ist. In der Ausgabe Schulung ist der Wert für die **Beschriftung** in der ersten Zeile daher **0,0**, was bedeutet, dass das Gebäude nicht wichtiges ist.

8.  Vorbereiten einer Datengruppe zurück, das ausgebildete Modell in Bezug auf ausführen. Hierzu möchten wir auf einen System-ID und das System Alter (in der Ausgabe Schulung als **SystemInfo** gekennzeichnet) übergeben, und das Modell möchten, ob das Gebäude mit dieser System-ID und das System Alter heißeren wäre Vorhersagen (gekennzeichnet durch 1.0) oder Kühlgerät (gekennzeichnet durch 0.0).

    Fügen Sie den folgenden Codeausschnitt in eine leere Zelle, und drücken Sie **UMSCHALT + EINGABETASTE**.
        
        # SystemInfo here is a combination of system ID followed by system age
        Document = Row("id", "SystemInfo")
        test = sc.parallelize([(1L, "20 25"),
                      (2L, "4 15"),
                      (3L, "16 9"),
                      (4L, "9 22"),
                      (5L, "17 10"),
                      (6L, "7 22")]) \
            .map(lambda x: Document(*x)).toDF() 

9. Stellen Sie schließlich Vorhersagen auf der Registerkarte Testdaten ein. Fügen Sie den folgenden Codeausschnitt in eine leere Zelle, und drücken Sie **UMSCHALT + EINGABETASTE**.

        # Make predictions on test documents and print columns of interest
        prediction = model.transform(test)
        selected = prediction.select("SystemInfo", "prediction", "probability")
        for row in selected.collect():
            print row

10. Sie sollte eine Ausgabe ähnlich der folgenden angezeigt:

        Row(SystemInfo=u'20 25', prediction=1.0, probability=DenseVector([0.4999, 0.5001]))
        Row(SystemInfo=u'4 15', prediction=0.0, probability=DenseVector([0.5016, 0.4984]))
        Row(SystemInfo=u'16 9', prediction=1.0, probability=DenseVector([0.4785, 0.5215]))
        Row(SystemInfo=u'9 22', prediction=1.0, probability=DenseVector([0.4549, 0.5451]))
        Row(SystemInfo=u'17 10', prediction=1.0, probability=DenseVector([0.4925, 0.5075]))
        Row(SystemInfo=u'7 22', prediction=0.0, probability=DenseVector([0.5015, 0.4985]))

    Aus der ersten Zeile in der Vorhersage, können Sie sehen, dass für ein HKL-System mit 20-ID und das System Alter von 25 Jahre, das Gebäude angesagt werden (**Vorhersage = 1,0**). Die Vorhersage 0,0 entspricht der erste Wert für DenseVector (0.49999) und der zweite Wert (0.5001) entspricht der vorhersagefeatures 1.0. In der Ausgabe, obwohl der zweite Wert nur marginal höher ist, ist das Modell zeigt **Vorhersage = 1,0**.

11. Nachdem Sie die Ausführung der Anwendung abgeschlossen haben, sollten Sie war(en) das Notizbuch, um die Ressourcen freizugeben. Klicken Sie dazu im Menü **Datei** auf dem Notizbuch, auf **Schließen und Anhalten**. Diese wird geschlossen und schließen Sie das Notizbuch.
           

##<a name="a-nameanacondaause-anaconda-scikit-learn-library-for-machine-learning"></a><a name="anaconda"></a>Verwenden Sie Anaconda Scikit-Bibliothek für maschinelle Learning erfahren

Apache Spark Cluster auf HDInsight gehören Anaconda Bibliotheken. Auch umfasst die **Scikit-Informationen** Bibliothek für maschinelle Schulung. Die Bibliothek enthält darüber hinaus verschiedene Datensätze, die Sie verwenden können, Stichprobe von Applications direkt aus einem Jupyter Notizbuch erstellen. Beispiele für die Verwendung der Scikit-Bibliothek zu erfahren, finden Sie unter [http://scikit-learn.org/stable/auto_examples/index.html](http://scikit-learn.org/stable/auto_examples/index.html).

##<a name="a-nameseealsoasee-also"></a><a name="seealso"></a>Siehe auch

* [Übersicht: Apache Spark auf Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Szenarien

* [Spark mit BI: Ausführen interaktiven Datenanalyse mithilfe von Spark in HDInsight mit BI-Tools](hdinsight-apache-spark-use-bi-tools.md)

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


[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-weblogs-sample]: hdinsight-hive-analyze-website-log.md
[hdinsight-sensor-data-sample]: hdinsight-hive-analyze-sensor-data.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://manage.windowsazure.com/
[azure-create-storageaccount]: storage-create-storage-account.md

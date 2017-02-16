<properties 
    pageTitle="Mithilfe von benutzerdefinierten Bibliotheken mit einem HDInsight Spark Cluster Protokolle Website analysieren | Microsoft Azure" 
    description="Verwenden Sie benutzerdefinierte Bibliotheken mit einem HDInsight Spark Cluster Protokolle Website analysieren" 
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

# <a name="analyze-website-logs-using-a-custom-library-with-apache-spark-cluster-on-hdinsight-linux"></a>Analysieren von Website-Protokolle, verwenden eine benutzerdefinierte Bibliothek mit Apache Spark Cluster auf HDInsight Linux

Dieses Notizbuch wird veranschaulicht, wie Daten mithilfe einer benutzerdefinierten Bibliothek mit Spark auf HDInsight zu analysieren. Die benutzerdefinierte Bibliothek, die wir verwenden ist eine Python-Bibliothek, die als **iislogparser.py**bezeichnet.

> [AZURE.TIP] In diesem Lernprogramm steht auch als Jupyter Notizbuch auf einer Cluster Spark (Linux), den Sie in HDInsight zu erstellen. Die Notizbuch-Oberfläche können Sie die Python Codeausschnitte aus dem Notizbuch selbst ausführen. Wenn Sie das Lernprogramm aus in einem Notizbuch ausführen zu können, erstellen Sie einen Cluster Spark, starten Sie ein Notizbuch Jupyter (`https://CLUSTERNAME.azurehdinsight.net/jupyter`), und führen Sie dann unter dem Ordner **PySpark** das Notizbuch **Analysieren mit Spark mithilfe einer benutzerdefinierten library.ipynb protokolliert** .

**Voraussetzungen für:**

Sie müssen die folgenden:

- Ein Azure-Abonnement. Finden Sie [kostenlose Testversion Azure abrufen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Eine Apache Spark Cluster auf HDInsight Linux. Anweisungen finden Sie unter [Erstellen von Apache Spark Cluster in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="save-raw-data-as-an-rdd"></a>Speichern von unformatierten Daten als ein RDD

In diesem Abschnitt verwenden wir das [Jupyter](https://jupyter.org) Notizbuch eine Apache Spark Cluster in HDInsight zugeordnet Auftrag ausführen, die Ihre unformatierten Beispieldaten zu verarbeiten und speichern Sie es als eine strukturtabelle aus. Die Beispieldaten ist eine CSV-Datei (hvac.csv) verfügbar auf alle Cluster standardmäßig an.

Nachdem die Daten als strukturtabelle gespeichert wurde, wird im nächsten Abschnitt wir die strukturtabelle mit BI-Tools, wie Power BI und Tableaus Verbindung.

1. Klicken Sie auf die Kachel für Ihren Cluster Spark, aus dem [Azure-Portal](https://portal.azure.com/), mithilfe der Startboard (Wenn Sie es an die Startboard angeheftet). Sie können auch navigieren Sie zu Ihren Cluster unter **Alle durchsuchen** > **HDInsight Cluster**.   

2. Klicken Sie aus dem Spark Cluster Blade auf **Cluster Dashboard**, und klicken Sie dann auf **Jupyter Notizbuch**. Wenn Sie dazu aufgefordert werden, geben Sie die Administrator-Anmeldeberechtigungen für den Cluster aus.

    > [AZURE.NOTE] Sie möglicherweise auch das Notizbuch Jupyter für Ihren Cluster erreichen, indem Sie den folgenden URL in Ihrem Browser öffnen. Ersetzen Sie __CLUSTERNAME__ mit dem Namen der Cluster aus:
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Erstellen eines neuen Notizbuchs an. Klicken Sie auf **neu**, und klicken Sie dann auf **PySpark**.

    ![Erstellen eines neuen Jupyter Notizbuchs] (./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdispark.note.jupyter.createnotebook.png "Erstellen eines neuen Jupyter Notizbuchs")

3. Ein neues Notizbuch erstellt und mit dem Namen Untitled.pynb geöffnet. Klicken Sie auf den Notizbuchnamen oben, und geben Sie einen Anzeigenamen ein.

    ![Bereitstellen einen Namen für das Notizbuch] (./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdispark.note.jupyter.notebook.name.png "Bereitstellen einen Namen für das Notizbuch")

4. Da Sie ein Notizbuch, mit dem die PySpark Kernel erstellt haben, müssen Sie keine Kontexte explizit zu erstellen. Die Kontexte Spark und Struktur werden automatisch für Sie erstellt werden, wenn Sie die erste Zelle der Code ausführen. Sie können Sie zunächst importieren die Typen, die in diesem Szenario erforderlich sind. Fügen Sie den folgenden Codeausschnitt in eine leere Zelle, und drücken Sie dann **UMSCHALT + EINGABETASTE**.


        from pyspark.sql import Row
        from pyspark.sql.types import *


5. Erstellen Sie eine mit der Log-Beispieldaten, die bereits verfügbar auf dem Cluster RDD ein. Sie können die Daten in den Speicher Standardkonto Cluster am **\HdiSamples\HdiSamples\WebsiteLogSampleData\SampleLog\909f2b.log**zugeordnet zugreifen.


        logs = sc.textFile('wasbs:///HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b.log')


6. Abrufen einer Stichprobe Log festlegen, um zu überprüfen, ob im vorherigen Schritt wurde erfolgreich abgeschlossen.

        logs.take(5)

    Sie sollte eine Ausgabe ähnlich der folgenden angezeigt:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [u'#Software: Microsoft Internet Information Services 8.0',
         u'#Fields: date time s-sitename cs-method cs-uri-stem cs-uri-query s-port cs-username c-ip cs(User-Agent) cs(Cookie) cs(Referer) cs-host sc-status sc-substatus sc-win32-status sc-bytes cs-bytes time-taken',
         u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step2.png X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 53175 871 46',
         u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step3.png X-ARR-LOG-ID=9eace870-2f49-4efd-b204-0d170da46b4a 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 51237 871 32',
         u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step4.png X-ARR-LOG-ID=4bea5b3d-8ac9-46c9-9b8c-ec3e9500cbea 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 72177 871 47']

## <a name="analyze-log-data-using-a-custom-python-library"></a>Analysieren von Daten mithilfe einer benutzerdefinierten Python-Bibliothek

7. In der Ausgabe die ersten paar Zeilen die Kopfzeileninformationen einbeziehen, und jedes verbleibende Linie entspricht das Schema in die Kopfzeile. Analysieren solche Protokolle kann kompliziert sein. Ja, verwenden wir eine benutzerdefinierte Python-Bibliothek (**iislogparser.py**), die vereinfacht die Analyse solche Protokolle viel einfacher. Standardmäßig ist dieser Bibliothek in Ihrem Cluster Spark auf HDInsight bei **/HdiSamples/HdiSamples/WebsiteLogSampleData/iislogparser.py**enthalten.

    Diese Bibliothek ist jedoch nicht in der `PYTHONPATH` , damit wir es verwenden können, wie eine Import-Anweisung mit `import iislogparser`. Wenn Sie diese Bibliothek verwenden zu können, müssen wir es an alle Worker Knoten verteilen. Führen Sie den folgenden Codeausschnitt.


        sc.addPyFile('wasbs:///HdiSamples/HdiSamples/WebsiteLogSampleData/iislogparser.py')


9. `iislogparser`Stellt eine Funktion `parse_log_line` gibt, die `None` Wenn eine Linie Log eine Kopfzeile ist und eine Instanz von gibt der `LogLine` Klasse, wenn sie eine Linie Log stößt. Verwenden der `LogLine` Class, um nur die Log Zeilen aus der RDD extrahieren:

        def parse_line(l):
            import iislogparser
            return iislogparser.parse_log_line(l)
        logLines = logs.map(parse_line).filter(lambda p: p is not None).cache()


10. Abrufen von ein paar extrahierten Log Linien zu überprüfen, ob den Schritt wurde erfolgreich abgeschlossen.

        logLines.take(2)

    Die Ausgabe sollte etwa wie folgt aussehen:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step2.png X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 53175 871 46,
        2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step3.png X-ARR-LOG-ID=9eace870-2f49-4efd-b204-0d170da46b4a 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 51237 871 32]


11. Die `LogLine` Class, wiederum enthält einige nützlichen Methoden, wie `is_error()`, die zurückgibt, ob ein Eintrag einen Fehlercode hat. Hiermit können Sie um die Anzahl der Fehler in der extrahierten Log Linien zu berechnen, und melden Sie sich dann alle Fehler in einer anderen Datei.

        errors = logLines.filter(lambda p: p.is_error())
        numLines = logLines.count()
        numErrors = errors.count()
        print 'There are', numErrors, 'errors and', numLines, 'log entries'
        errors.map(lambda p: str(p)).saveAsTextFile('wasbs:///HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b-2.log')

    Sie sollte eine Ausgabe wie die folgende angezeigt:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        There are 30 errors and 646 log entries

12. Sie können auch **Matplotlib** verwenden, um eine Visualisierung von Daten zu erstellen. Wenn Sie die Ursache des Anfragen isolieren, die längere Zeit ausführen möchten, sollten Sie die Dateien zu finden, die die am häufigsten im Durchschnitt dienen dauern.
Der folgenden Codeausschnitt Ruft die obersten 25 Ressourcen, die meisten Zeit für eine Anforderung dienen genommen haben.

        def avgTimeTakenByKey(rdd):
            return rdd.combineByKey(lambda line: (line.time_taken, 1),
                                    lambda x, line: (x[0] + line.time_taken, x[1] + 1),
                                    lambda x, y: (x[0] + y[0], x[1] + y[1]))\
                      .map(lambda x: (x[0], float(x[1][0]) / float(x[1][1])))
            
        avgTimeTakenByKey(logLines.map(lambda p: (p.cs_uri_stem, p))).top(25, lambda x: x[1])

    Sie sollte eine Ausgabe wie die folgende angezeigt:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [(u'/blogposts/mvc4/step13.png', 197.5),
         (u'/blogposts/mvc2/step10.jpg', 179.5),
         (u'/blogposts/extractusercontrol/step5.png', 170.0),
         (u'/blogposts/mvc4/step8.png', 159.0),
         (u'/blogposts/mvcrouting/step22.jpg', 155.0),
         (u'/blogposts/mvcrouting/step3.jpg', 152.0),
         (u'/blogposts/linqsproc1/step16.jpg', 138.75),
         (u'/blogposts/linqsproc1/step26.jpg', 137.33333333333334),
         (u'/blogposts/vs2008javascript/step10.jpg', 127.0),
         (u'/blogposts/nested/step2.jpg', 126.0),
         (u'/blogposts/adminpack/step1.png', 124.0),
         (u'/BlogPosts/datalistpaging/step2.png', 118.0),
         (u'/blogposts/mvc4/step35.png', 117.0),
         (u'/blogposts/mvcrouting/step2.jpg', 116.5),
         (u'/blogposts/aboutme/basketball.jpg', 109.0),
         (u'/blogposts/anonymoustypes/step11.jpg', 109.0),
         (u'/blogposts/mvc4/step12.png', 106.0),
         (u'/blogposts/linq8/step0.jpg', 105.5),
         (u'/blogposts/mvc2/step18.jpg', 104.0),
         (u'/blogposts/mvc2/step11.jpg', 104.0),
         (u'/blogposts/mvcrouting/step1.jpg', 104.0),
         (u'/blogposts/extractusercontrol/step1.png', 103.0),
         (u'/blogposts/sqlvideos/sqlvideos.jpg', 102.0),
         (u'/blogposts/mvcrouting/step21.jpg', 101.0),
         (u'/blogposts/mvc4/step1.png', 98.0)]


13. Sie können auch diese Informationen in Form von Zeichnungsfläche darstellen. Als ersten Schritt zum Erstellen einer Zeichnung erstellen Sie eine temporäre Tabelle **AverageTime**lassen Sie uns zuerst. Die Tabelle werden die Protokolle nach Zeit, um festzustellen, ob es irgendwelche Spitzen ungewöhnliche Wartezeit zu einem bestimmten Zeitpunkt wurden gruppiert.

        avgTimeTakenByMinute = avgTimeTakenByKey(logLines.map(lambda p: (p.datetime.minute, p))).sortByKey()
        schema = StructType([StructField('Minutes', IntegerType(), True),
                             StructField('Time', FloatType(), True)])
                             
        avgTimeTakenByMinuteDF = sqlContext.createDataFrame(avgTimeTakenByMinute, schema)
        avgTimeTakenByMinuteDF.registerTempTable('AverageTime')

14. Anschließend können Sie die folgende SQL-Abfrage, um alle Datensätze in der Tabelle **AverageTime** erhalten ausführen.

        %%sql -o averagetime
        SELECT * FROM AverageTime

    Die `%%sql` magische gefolgt von `-o averagetime` wird sichergestellt, dass die Ausgabe der Abfrage lokal auf dem Server Jupyter (in der Regel die Headnode Cluster) beibehalten werden. Die Ausgabe wird als eine [Pandas](http://pandas.pydata.org/) Dataframe mit der angegebenen Namen **Averagetime**beibehalten.

    Sie sollte eine Ausgabe wie die folgende angezeigt:

    ![Die Ausgabe der SQL-Abfrage] (./media/hdinsight-apache-spark-custom-library-website-log-analysis/sql.output.png "Die Ausgabe der SQL-Abfrage")

    Weitere Informationen zu den `%%sql` magische sowie andere zur Verfügung, mit dem Kernel PySpark Magics finden Sie unter [Kerneln Jupyter Notizbücher mit Spark HDInsight Cluster zur Verfügung](hdinsight-apache-spark-jupyter-notebook-kernels.md#why-should-i-use-the-new-kernels).

15. Matplotlib, eine Bibliothek verwendet, um die Visualisierung von Daten erstellen können nun um eine Zeichnung zu erstellen. Da der Zeichnung aus der lokal gespeicherten **Averagetime** Dataframe erstellt werden muss, muss der Codeausschnitt beginnen, mit der `%%local` magische. Dadurch wird sichergestellt, dass der Code lokal auf dem Server Jupyter ausgeführt wird.

        %%local
        %matplotlib inline
        import matplotlib.pyplot as plt
        
        plt.plot(averagetime['Minutes'], averagetime['Time'], marker='o', linestyle='--')
        plt.xlabel('Time (min)')
        plt.ylabel('Average time taken for request (ms)')

    Sie sollte eine Ausgabe wie die folgende angezeigt:

    ![Matplotlib Ausgabe] (./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdi-apache-spark-web-log-analysis-plot.png "Matplotlib Ausgabe")

16. Nachdem Sie die Ausführung der Anwendung abgeschlossen haben, sollten Sie war(en) das Notizbuch, um die Ressourcen freizugeben. Klicken Sie dazu im Menü **Datei** auf dem Notizbuch, auf **Schließen und Anhalten**. Diese wird geschlossen und schließen Sie das Notizbuch.
    

## <a name="a-nameseealsoasee-also"></a><a name="seealso"></a>Siehe auch


* [Übersicht: Apache Spark auf Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Szenarien

* [Spark mit BI: Ausführen interaktiven Datenanalyse mithilfe von Spark in HDInsight mit BI-Tools](hdinsight-apache-spark-use-bi-tools.md)

* [Spark mit maschinellen Schulung: Verwenden Sie Spark in HDInsight zum Analysieren von Gebäude Temperatur HKL-Daten verwenden](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

* [Spark mit maschinellen Schulung: verwenden Spark in HDInsight Lebensmittel Prüfungsergebnissen Vorhersagen](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

* [Spark Streaming: Verwenden Sie Spark in HDInsight zum Erstellen von in Echtzeit streaming Clientanwendungen](hdinsight-apache-spark-eventhub-streaming.md)

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

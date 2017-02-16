<properties
    pageTitle="Übermitteln Spark Aufträge mit Remote Livius | Microsoft Azure"
    description="Informationen Sie zur Verwendung von Livius mit HDInsight Cluster Spark Aufträge Remote senden."
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


# <a name="submit-spark-jobs-remotely-to-an-apache-spark-cluster-on-hdinsight-linux-using-livy"></a>Übermitteln Sie Spark Aufträge Remote an einem Apache Spark Cluster auf HDInsight Linux Livius verwenden

Apache Spark Cluster auf Azure HDInsight enthält Livius, ein REST-Schnittstelle zum Senden von Aufträgen per Remotezugriff zu einem Spark Cluster. Eine ausführliche Dokumentation finden Sie unter [Livius](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server).

Sie können Livius ausführen interaktive Spark Schalen oder übermitteln Stapelverarbeitung abgeschlossen auf Spark ausgeführt werden. In diesem Artikel spricht zur Verwendung von Livius Stapelverarbeitungsaufträge senden. Die folgende Syntax verwendet Curl um REST den Endpunkt Livius anzurufen.

**Voraussetzungen für:**

Sie müssen die folgenden:

- Ein Azure-Abonnement. Finden Sie [kostenlose Testversion Azure abrufen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Eine Apache Spark Cluster auf HDInsight Linux. Anweisungen finden Sie unter [Erstellen von Apache Spark Cluster in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="submit-a-batch-job-the-cluster"></a>Senden einer Stapelverarbeitung cluster

Bevor Sie eine Stapelverarbeitung übermitteln, müssen Sie die JAR-Anwendung auf dem Cluster-Speicher Cluster zugeordnet hochladen. Sie können [**AzCopy**](../storage/storage-use-azcopy.md), ein Programm Befehlszeile dazu verwenden. Es gibt zahlreiche andere Clients, die Sie zum Hochladen von Daten verwenden können. Sie können mehr über diese am [Hochladen von Daten für Hadoop Aufträge in HDInsight](hdinsight-upload-data.md)suchen.

    curl -k --user "<hdinsight user>:<user password>" -v -H <content-type> -X POST -d '{ "file":"<path to application jar>", "className":"<classname in jar>" }' 'https://<spark_cluster_name>.azurehdinsight.net/livy/batches'

**Beispiele**:

* Ist die JAR-Datei auf dem Cluster-Speicher (WASB)

        curl -k --user "admin:mypassword1!" -v -H 'Content-Type: application/json' -X POST -d '{ "file":"wasbs://mycontainer@mystorageaccount.blob.core.windows.net/data/SparkSimpleTest.jar", "className":"com.microsoft.spark.test.SimpleFile" }' "https://mysparkcluster.azurehdinsight.net/livy/batches"

* Wenn Sie den JAR-Dateinamen und den Klassennamen als Teil des Eingabe-übergeben möchten (in diesem Beispiel Eingabe.txt)

        curl -k  --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\input.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

## <a name="get-information-on-batches-running-on-the-cluster"></a>Abrufen von Informationen im Cluster ausgeführten Blattnamen

    curl -k --user "<hdinsight user>:<user password>" -v -X GET "https://<spark_cluster_name>.azurehdinsight.net/livy/batches"

**Beispiele**:

* Wenn Sie alle im Cluster ausgeführten Blattnamen abrufen möchten:

        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches"

* Wenn Sie einen bestimmten Stapel mit einer angegebenen BatchId abgerufen werden sollen

        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches/{batchId}"


## <a name="delete-a-batch-job"></a>Löschen einer Stapelverarbeitung

    curl -k --user "<hdinsight user>:<user password>" -v -X DELETE "https://<spark_cluster_name>.azurehdinsight.net/livy/batches/{batchId}"

**Beispiel**:

    curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/{batchId}"

## <a name="livy-and-high-availability"></a>Livius und hoher Verfügbarkeit

Livius bietet hohe Verfügbarkeit für Spark Aufträge, die auf dem Cluster ausgeführt. Hier sind ein paar Beispiele für ein.

* Wenn der Dienst Livius fällt aus, nachdem Sie ein Projekt Remote zu einem Spark Cluster gesendet haben, wird der Auftrag weiterhin im Hintergrund ausgeführt. Wenn Livius wieder verfügbar ist, stellt es den Status der Position und Berichte, die es wieder.

* Jupyter Notizbücher für HDInsight werden durch Livius in die Back-End-betrieben. Wenn ein Notizbuch ein Auftrags Spark ausgeführt wird, und der Livius-Dienst wird neu gestartet, wird das Notizbuch weiterhin die Zellen Code ausführen. 

## <a name="show-me-an-example"></a>Beispiel anzeigen

In diesem Abschnitt betrachten wir Beispiele zum Livius verwenden, um eine Anwendung Spark übermitteln, den Fortschritt der Anwendung überwachen und löschen Sie den Auftrag aus. Die Anwendung, die wir, in diesem Beispiel verwenden ist eine entwickelt im Artikel [Erstellen eines eigenständigen Scala Anwendung und auf HDInsight Spark Cluster ausführen](hdinsight-apache-spark-create-standalone-application.md). Die folgenden Schritte aus, wird Folgendes vorausgesetzt:

* Sie haben bereits mit dem Speicherkonto zugeordnet Cluster über der Anwendung Jar kopiert.
* Sie haben CuRL auf die Stelle, an der Sie diese Schritte versuchen Computer installiert.

Führen Sie die folgenden Schritte aus.

1. Lassen Sie uns zuerst sicher, dass auf dem Cluster Livius ausgeführt wird. Wir können dies tun, indem Sie eine Liste der ausgeführten Blattnamen abrufen. Wenn dies das erste Mal ist, die ein Projekt mit Livius ausgeführt werden, sollte 0 (null) zurückgegeben.

        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches"

    Sie sollten eine Ausgabe ähnlich der folgenden erhalten:

        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:47:53 GMT
        < Content-Length: 34
        <
        {"from":0,"total":0,"sessions":[]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact

    Beachten Sie, wie die letzte Zeile in der Ausgabe für **insgesamt: 0**, besagt die vorgeschlagenen keine laufenden Stapel.

2. Lassen Sie uns jetzt übermitteln einer Stapelverarbeitung. Der folgenden Codeausschnitt verwendet eine Datei (Eingabe.txt) der JAR-Name übergeben und den Klassennamen als Parameter ein. Dies wird empfohlen, wenn Sie diese Schritte auf einem Windows-Computer ausgeführt werden.

        curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\input.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

    Die Parameter in der Datei **Eingabe.txt** sind wie folgt definiert:

        { "file":"wasbs:///example/jars/SparkSimpleApp.jar", "className":"com.microsoft.spark.example.WasbIOTest" }

    Sie sollte eine Ausgabe ähnlich der folgenden angezeigt:

        < HTTP/1.1 201 Created
        < Content-Type: application/json; charset=UTF-8
        < Location: /0
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:51:30 GMT
        < Content-Length: 36
        <
        {"id":0,"state":"starting","log":[]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact

    Beachten Sie, wie die letzte Zeile der Ausgabe besagt **Zustand: beginnend**. Es auch angezeigt wird, **Id: 0**. Dies ist der Stapel ID ein.

3. Sie können jetzt abrufen, die den Status dieser bestimmten Stapel mit den Stapel ID ein.

        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches/0"

    Sie sollte eine Ausgabe ähnlich der folgenden angezeigt:

        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:54:42 GMT
        < Content-Length: 509
        <
        {"id":0,"state":"success","log":["\t diagnostics: N/A","\t ApplicationMaster host: 10.0.0.4","\t ApplicationMaster RPC port: 0","\t queue: default","\t start time: 1448063505350","\t final status: SUCCEEDED","\t tracking URL: http://hn0-myspar.lpel1gnnvxne3gwzqkfq5u5uzh.jx.internal.cloudapp.net:8088/proxy/application_1447984474852_0002/","\t user: root","15/11/20 23:52:47 INFO Utils: Shutdown hook called","15/11/20 23:52:47 INFO Utils: Deleting directory /tmp/spark-b72cd2bf-280b-4c57-8ceb-9e3e69ac7d0c"]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact

    Die Ausgabe zeigt jetzt **Zustand: Erfolg**, anzeigt, dass der Auftrag erfolgreich abgeschlossen wurde.

4. Wenn Sie möchten, können Sie jetzt den Stapel löschen.

        curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/0"

    Sie sollte eine Ausgabe ähnlich der folgenden angezeigt:

        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Sat, 21 Nov 2015 18:51:54 GMT
        < Content-Length: 17
        <
        {"msg":"deleted"}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact

    Die letzte Zeile der Ausgabe zeigt, dass der Stapel erfolgreich gelöscht wurde. Wenn Sie während der Ausführung ein Auftrags löschen, wird es im Wesentlichen den Auftrag abbrechen. Wenn Sie ein Projekt löschen, die erfolgreich oder auf andere Weise durchgeführt wurde werden die Auftragsinformationen vollständig gelöscht.

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

### <a name="tools-and-extensions"></a>Tools und Erweiterungen

* [Verwenden Sie zum Erstellen und übermitteln Spark Scala Applikationen HDInsight Tools-Plug-In für IntelliJ IDEE](hdinsight-apache-spark-intellij-tool-plugin.md)

* [Verwenden von HDInsight Tools-Plug-In für IntelliJ IDEE Spark Applikationen Remote-Debuggen](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)

* [Verwenden von Zeppelin Notizbücher mit einem Spark Cluster auf HDInsight](hdinsight-apache-spark-use-zeppelin-notebook.md)

* [Kernels für Jupyter-Notizbuch in Spark Cluster für HDInsight verfügbar](hdinsight-apache-spark-jupyter-notebook-kernels.md)

* [Verwenden von externen Paketen mit Jupyter-Notizbüchern](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

* [Jupyter auf Ihrem Computer installieren und Verbinden mit einem HDInsight Spark cluster](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Verwalten von Ressourcen

* [Verwalten von Ressourcen für den Apache Spark Cluster in Azure HDInsight](hdinsight-apache-spark-resource-manager.md)

* [Verfolgen und Debuggen Aufträge in einem Apache Spark Cluster in HDInsight](hdinsight-apache-spark-job-debugging.md)

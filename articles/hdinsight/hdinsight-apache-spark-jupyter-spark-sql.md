<properties
    pageTitle="Erstellen Sie einen Spark Cluster auf HDInsight Linux und Spark SQL aus Jupyter für interaktive Analyse verwenden | Microsoft Azure"
    description="Eine schrittweise Anleitung, wie Sie schnell eine Apache Spark erstellen in HDInsight cluster und verwenden Sie Spark SQL aus Jupyter Notizbücher, um interaktive Abfragen zu starten."
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
    ms.date="10/28/2016"
    ms.author="nitinme"/>


# <a name="get-started-create-apache-spark-cluster-on-hdinsight-linux-and-run-interactive-queries-using-spark-sql"></a>Erste Schritte: Apache Spark Cluster auf HDInsight Linux erstellen und Ausführen interaktive mit Spark SQL-Abfragen

Erfahren Sie, wie einen Apache Spark Cluster im HDInsight erstellen und verwenden Sie dann [Jupyter](https://jupyter.org) Notizbuch, um interaktive Spark SQL-Abfragen auf Spark Cluster ausgeführt werden.

   ![Erste Schritte mit Apache Spark in HDInsight] (./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.getstartedflow.png  "Erste Schritte mit Apache Spark in HDInsight Lernprogramm. Dargestellten Schritte: Erstellen eines Speicher-Kontos Erstellen Sie einen Cluster; Ausführen von Spark SQL-Anweisungen")

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="prerequisites"></a>Erforderliche Komponenten

- **Ein Azure-Abonnement**. Bevor Sie dieses Lernprogramm beginnen, müssen Sie ein Azure-Abonnement verfügen. Finden Sie [kostenlose Testversion Azure abrufen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **Client A Secure Shell (SSH)**: Linux, Unix und OS X Systeme angegeben einer SSH Client über die `ssh` Befehl. Für Windows-Betriebssysteme empfehlen wir [kitten](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).
    
- **Secure Shell (SSH) Tasten (optional)**: Sie können das Verbindung mit dem Cluster verwenden entweder ein Kennwort oder ein öffentlicher Schlüssel verwendete SSH-Konto sichern. Mit einem Kennwort wird Ihnen einen schnellen Einstieg und sollten Sie diese Option verwenden, wenn Sie schnell einen Cluster erstellen und Ausführen von einige Einzelvorgänge testen möchten. Mit einem Schlüssel ist sicherer, doch es zusätzlichen Setup erfordert. Möglicherweise möchten diesem Ansatz beim Erstellen eines Herstellung Clusters. In diesem Artikel verwenden wir den Ansatz Kennwort ein. Anweisungen zum Erstellen und Verwenden von SSH Tasten mit HDInsight finden Sie in den folgenden Artikeln:

    -  Von einem Linux Computer - [Verwenden SSH mit Linux-basierten HDInsight (Hadoop) von Linux, Unix, oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md).
    
    -  Auf einem Windows-Computer - [Verwenden SSH mit Linux-basierten HDInsight (Hadoop) von Windows](hdinsight-hadoop-linux-use-ssh-windows.md).

>[AZURE.NOTE] In diesem Artikel verwendet eine Azure Ressource-Manager-Vorlage, um eine Spark Cluster zu erstellen, der [Azure-Speicher Blobs als dem Cluster-Speicher](hdinsight-hadoop-use-blob-storage.md)verwendet. Sie können auch einen Spark Cluster erstellen, der als einer zusätzlichen Speicher, zusätzlich zur Azure-Speicher Blobs als Standardspeicher [Azure dem Datenspeicher](../data-lake-store/data-lake-store-overview.md) verwendet wird. Anweisungen finden Sie unter [Erstellen einer HDInsight Cluster mit dem Datenspeicher](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).

### <a name="access-control-requirements"></a>Anforderungen für Access-Steuerelement

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-spark-cluster"></a>Erstellen von Spark cluster

In diesem Abschnitt erstellen Sie einen HDInsight Version 3.4 Cluster (Spark Version 1.6.1) mit einer Ressource Azure-Manager-Vorlage. Informationen zu HDInsight Versionen und ihre SLAs finden Sie unter [HDInsight Komponente Versioning](hdinsight-component-versioning.md). Weitere Methoden zur Erstellung Cluster finden Sie unter [Erstellen von HDInsight Cluster](hdinsight-hadoop-provision-linux-clusters.md).

1. Klicken Sie auf die folgende Abbildung, um die Vorlage im Azure-Portal zu öffnen.         

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-spark-cluster-in-hdinsight.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>
    
    Die Vorlage befindet sich in einem öffentlichen Blob-Container *https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-spark-cluster-in-hdinsight.json*. 
   
2. Geben Sie aus dem Parameter Blade Folgendes ein:

    - **ClusterName**: Geben Sie einen Namen für den Hadoop Cluster, die Sie erstellen.
    - **Cluster-Benutzernamen und Ihr Kennwort**: der Standard-Anmeldename ist Administrator.
    - **SSH-Benutzernamen und Ihr Kennwort ein**.
    
    Bitte notieren Sie sich diese Werte an.  Sie benötigen diese später im Lernprogramm.

    > [AZURE.NOTE] SSH wird verwendet, um die Remotezugriff auf HDInsight Cluster mithilfe einer Befehlszeile. Benutzername und Kennwort ein, das Sie hier verwenden, wird bei der Verbindung mit Cluster erfolgt über SSH verwendet. Darüber hinaus muss der Benutzernamen SSH eindeutig ist, beim Erstellen eines Benutzerkontos auf allen HDInsight Clusterknoten. Die folgenden sind einige der den Kontonamen für die Verwendung von Diensten auf dem Cluster reserviert und kann nicht als Benutzername SSH verwendet werden:
    >
    > Quadratwurzel, Hdiuser, Sturm, Hbase, Ubuntu, Zookeeper, Hdfs, aus, Mapred, Hbase, Struktur, Oozie, Falcon, Sqoop, Administrator, Tez, Hcat, Hdinsight-Zookeeper.

    > Weitere Informationen zum Verwenden von SSH mit HDInsight finden Sie unter den folgenden Artikeln:

    > * [Verwenden von SSH mit Linux-basierten Hadoop auf HDInsight von Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md)
    > * [Verwenden von SSH mit Linux-basierten Hadoop auf HDInsight von Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

    
3.Klicken klicken Sie auf **OK** , um die Parameter zu speichern.

4.konfigurieren aus dem **benutzerdefinierte Bereitstellung** -Blade klicken Sie auf die Dropdown-Feld **Ressourcengruppe** , und klicken Sie dann auf **neu** , um eine neue Ressourcengruppe erstellen. Die Ressourcengruppe ist ein Container, der Cluster, das Konto abhängige Speicherplatz und andere verknüpfte Ressource gruppiert.

5 ° Klicken Sie auf die **Vertragsbedingungen**, und klicken Sie dann auf **Erstellen**.

6. Klicken Sie auf **Erstellen**. Sehen Sie eine neue Kachel mit dem Titel Submitting Bereitstellung für die Bereitstellung der Vorlage ein. Es dauert zu ungefähr 20 Minuten der Cluster und SQL-Datenbank zu erstellen.



## <a name="run-spark-sql-queries-using-a-jupyter-notebook"></a>Führen Sie die Verwendung eines Notizbuchs Jupyter Spark SQL-Abfragen

In diesem Abschnitt verwenden Sie Jupyter Notizbuch Spark SQL-Abfragen für Spark Cluster ausführen. HDInsight Spark Cluster bieten zwei Kernels, die mit dem Notizbuch Jupyter verwendet werden können. Dies sind:

* **PySpark** (für Applikationen geschrieben in Python)
* **Spark** (für Applikationen geschrieben in Scala)

In diesem Artikel verwenden Sie den PySpark Kernel. Im Artikel [Kernels verfügbar Jupyter Notizbücher mit Spark HDInsight Cluster](hdinsight-apache-spark-jupyter-notebook-kernels.md#why-should-i-use-the-new-kernels) können Sie im Detail über die Vorteile der Verwendung von PySpark Kernel lesen. Es bestehen jedoch verschiedene Hauptvorteile von PySpark Kernel:

* Sie müssen nicht die Kontexten für Spark und Struktur festlegen. Diese werden automatisch für Sie festgelegt werden.
* Sie verwenden die Zelle Magics, z. B. `%%sql`, um Ihre Abfragen SQL oder Struktur, ohne eine vorherige Codeausschnitte direkt ausführen.
* Die Ausgabe für die SQL- oder Struktur Abfragen automatisch visuell dargestellt.

### <a name="create-jupyter-notebook-with-pyspark-kernel"></a>Erstellen von Jupyter Notizbuch mit PySpark kernel 

1. Klicken Sie auf die Kachel für Ihren Cluster Spark, aus dem [Azure-Portal](https://portal.azure.com/), mithilfe der Startboard (Wenn Sie es an die Startboard angeheftet). Sie können auch navigieren Sie zu Ihren Cluster unter **Alle durchsuchen** > **HDInsight Cluster**.   

2. Klicken Sie aus dem Spark Cluster Blade auf **Cluster Dashboard**, und klicken Sie dann auf **Jupyter Notizbuch**. Wenn Sie dazu aufgefordert werden, geben Sie die Administrator-Anmeldeberechtigungen für den Cluster aus.

    > [AZURE.NOTE] Sie möglicherweise auch das Notizbuch Jupyter für Ihren Cluster erreichen, indem Sie den folgenden URL in Ihrem Browser öffnen. Ersetzen Sie __CLUSTERNAME__ mit dem Namen der Cluster aus:
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Erstellen eines neuen Notizbuchs an. Klicken Sie auf **neu**, und klicken Sie dann auf **PySpark**.

    ![Erstellen eines neuen Jupyter Notizbuchs] (./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.note.jupyter.createnotebook.png "Erstellen eines neuen Jupyter Notizbuchs")

3. Ein neues Notizbuch erstellt und mit dem Namen Untitled.pynb geöffnet. Klicken Sie auf das Notizbuchnamen oben, und geben Sie einen Anzeigenamen ein.

    ![Bereitstellen einen Namen für das Notizbuch] (./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.note.jupyter.notebook.name.png "Bereitstellen einen Namen für das Notizbuch")

4. Da Sie ein Notizbuch, mit dem die PySpark Kernel erstellt haben, müssen Sie keine Kontexte explizit zu erstellen. Die Kontexte Spark und Struktur werden automatisch für Sie erstellt werden, wenn Sie die erste Zelle der Code ausführen. Sie können beginnen, durch Importieren der Datentypen für dieses Szenario erforderlich ist. Dazu fügen Sie den folgenden Codeausschnitt in einer Zelle, und drücken Sie **UMSCHALT + EINGABETASTE**.

        from pyspark.sql.types import *
        
    Jedes Mal, wenn Sie ein Projekt in Jupyter ausgeführt haben, wird der Web-Fenster Browsertitel zusammen mit den notizbuchtitel Status **(gebucht)** angezeigt. Außerdem sehen Sie einen durchgezogenen Kreis neben dem Text **PySpark** in der oberen rechten Ecke. Nachdem Sie der Auftrag abgeschlossen ist, wird dies in einen leeren Kreis ändern.

     ![Status eines Auftrags Jupyter Notizbuch] (./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.jupyter.job.status.png "Status eines Auftrags Jupyter Notizbuch")

4. Laden Sie Beispieldaten in eine temporäre Tabelle ein. Wenn Sie einen Cluster Spark in HDInsight erstellen, wird die Daten Beispieldatei **hvac.csv**, auf das zugehörige Speicher-Konto unter **\HdiSamples\HdiSamples\SensorSampleData\hvac**kopiert.

    Fügen Sie den folgenden Code wird, in eine leere Zelle und drücken Sie **UMSCHALT + EINGABETASTE**. In diesem Codebeispiel registriert die Daten in eine temporäre Tabelle **HKL-System**bezeichnet.

        # Load the data
        hvacText = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
        
        # Create the schema
        hvacSchema = StructType([StructField("date", StringType(), False),StructField("time", StringType(), False),StructField("targettemp", IntegerType(), False),StructField("actualtemp", IntegerType(), False),StructField("buildingID", StringType(), False)])
        
        # Parse the data in hvacText
        hvac = hvacText.map(lambda s: s.split(",")).filter(lambda s: s[0] != "Date").map(lambda s:(str(s[0]), str(s[1]), int(s[2]), int(s[3]), str(s[6]) ))
        
        # Create a data frame
        hvacdf = sqlContext.createDataFrame(hvac,hvacSchema)
        
        # Register the data fram as a table to run queries against
        hvacdf.registerTempTable("hvac")

5. Da Sie einen PySpark Kernel verwenden, Sie können jetzt direkt ausführen eine SQL-Abfrage auf die temporäre Tabelle **HKL-System** , die Sie soeben erstellt, mithilfe haben der `%%sql` magische. Weitere Informationen zu den `%%sql` magische sowie andere zur Verfügung, mit dem Kernel PySpark Magics finden Sie unter [Kerneln Jupyter Notizbücher mit Spark HDInsight Cluster zur Verfügung](hdinsight-apache-spark-jupyter-notebook-kernels.md#why-should-i-use-the-new-kernels).
        
        %%sql
        SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"

5. Nachdem Sie der Auftrag erfolgreich abgeschlossen wurde, wird die folgende tabellarische Ausgabe standardmäßig angezeigt.

    ![Tabellenausgabe Abfrageergebnis] (./media/hdinsight-apache-spark-jupyter-spark-sql/tabular.output.png "Tabellenausgabe Abfrageergebnis")

    Sie können auch die Ergebnisse in anderen Visualisierungen ebenfalls anzeigen. Beispielsweise würde ein Bereichsdiagramm für das gleiche Ergebnis wie folgt aussehen.

    ![Flächendiagramm der Abfrageergebnis] (./media/hdinsight-apache-spark-jupyter-spark-sql/area.output.png "Flächendiagramm der Abfrageergebnis")


6. Nachdem Sie die Ausführung der Anwendung abgeschlossen haben, sollten Sie war(en) das Notizbuch, um die Ressourcen freizugeben. Klicken Sie dazu im Menü **Datei** auf dem Notizbuch, auf **Schließen und Anhalten**. Diese wird geschlossen und schließen Sie das Notizbuch.

##<a name="delete-the-cluster"></a>Löschen des Clusters

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


## <a name="see-also"></a>Siehe auch


* [Übersicht: Apache Spark auf Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Szenarien

* [Spark mit BI: Ausführen interaktiven Datenanalyse mithilfe von Spark in HDInsight mit BI-Tools](hdinsight-apache-spark-use-bi-tools.md)

* [Spark mit maschinellen Schulung: Verwenden Sie Spark in HDInsight zum Analysieren von Gebäude Temperatur HKL-Daten verwenden](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

* [Spark mit maschinellen Schulung: verwenden Spark in HDInsight Lebensmittel Prüfungsergebnissen Vorhersagen](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

* [Spark Streaming: Verwenden Sie Spark in HDInsight zum Erstellen von in Echtzeit streaming Clientanwendungen](hdinsight-apache-spark-eventhub-streaming.md)

* [Website-Protokoll-Datenanalyse mithilfe von Spark in HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

* [Analyse der Anwendung Einblicke werden Daten mit Spark in HDInsight](hdinsight-spark-analyze-application-insight-logs.md)

### <a name="create-and-run-applications"></a>Erstellen und Ausführen von applications

* [Erstellen Sie eine eigenständige Anwendung Scala](hdinsight-apache-spark-create-standalone-application.md)

* [Führen Sie Aufträge Remote auf einem Spark Cluster Livius verwenden](hdinsight-apache-spark-livy-rest-interface.md)

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


[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://manage.windowsazure.com/
[azure-create-storageaccount]: storage-create-storage-account.md

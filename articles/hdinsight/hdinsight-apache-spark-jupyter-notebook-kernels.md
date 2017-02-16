<properties 
    pageTitle="Kerneln Jupyter Notizbücher auf HDInsight Spark erhältlich unter Linux Cluster | Microsoft Azure" 
    description="Informationen Sie zu den zusätzlichen Jupyter Notizbuch Kernels Spark Cluster auf HDInsight Linux erhältlich." 
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


# <a name="kernels-available-for-jupyter-notebooks-with-apache-spark-clusters-on-hdinsight-linux"></a>Verfügbar für Jupyter Notizbücher mit Apache Spark Cluster auf HDInsight Linux Kernels

Apache Spark Cluster auf HDInsight (Linux) enthält Jupyter Notizbücher, die Sie zum Testen einer Anwendung verwenden können. Der Kernel ist ein Programm, das ausgeführt wird und den Code interpretiert. HDInsight Spark Cluster bieten zwei Kernels, die mit dem Notizbuch Jupyter verwendet werden können. Dies sind:

1. **PySpark** (für Applikationen geschrieben in Python)
2. **Spark** (für Applikationen geschrieben in Scala)

In diesem Artikel lernen Sie so verwenden Sie diese Kernels, und welche Vorteile sind, denen, die Sie mit ihnen kommunizieren.

**Voraussetzungen für:**

Sie müssen die folgenden:

- Ein Azure-Abonnement. Finden Sie [kostenlose Testversion Azure abrufen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Eine Apache Spark Cluster auf HDInsight Linux. Anweisungen finden Sie unter [Erstellen von Apache Spark Cluster in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="how-do-i-use-the-kernels"></a>Wie verwende ich die Kernel? 

1. Klicken Sie auf die Kachel für Ihren Cluster Spark, aus dem [Azure-Portal](https://portal.azure.com/), mithilfe der Startboard (Wenn Sie es an die Startboard angeheftet). Sie können auch navigieren Sie zu Ihren Cluster unter **Alle durchsuchen** > **HDInsight Cluster**.   

2. Klicken Sie aus dem Spark Cluster Blade auf **Cluster Dashboard**, und klicken Sie dann auf **Jupyter Notizbuch**. Wenn Sie dazu aufgefordert werden, geben Sie die Administrator-Anmeldeberechtigungen für den Cluster aus.

    > [AZURE.NOTE] Sie möglicherweise auch das Notizbuch Jupyter für Ihren Cluster erreichen, indem Sie den folgenden URL in Ihrem Browser öffnen. Ersetzen Sie __CLUSTERNAME__ mit dem Namen der Cluster aus:
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Erstellen Sie ein neues Notizbuch mit den neuen Kernels. Klicken Sie auf **neu**, und klicken Sie dann auf **Pyspark** oder **Spark**. Sie sollten die Spark Kernel für Applikationen Scala und PySpark Kernel für Applikationen Python verwenden.

    ![Erstellen eines neuen Jupyter Notizbuchs] (./media/hdinsight-apache-spark-jupyter-notebook-kernels/jupyter-kernels.png "Erstellen eines neuen Jupyter Notizbuchs") 

3. Dadurch sollte ein neues Notizbuch mit dem Kernel geöffnet, die Sie ausgewählt haben.

## <a name="why-should-i-use-the-pyspark-or-spark-kernels"></a>Warum sollte ich die PySpark oder Spark Kernels werden verwendet?

Hier sind einige Vorteile der Verwendung der neuen Kernels aus.

1. **Voreingestellte Kontexten**. Mit der **PySpark** oder **Spark** Kernels, die mit Jupyter-Notizbüchern bereitgestellt werden, müssen Sie nicht die Kontexte Spark oder Struktur explizit festlegen, bevor Sie beginnen können, arbeiten mit der Anwendung, die Sie entwickeln; Dies sind die standardmäßig für Sie verfügbar. Diese Kontexte sind:

    * **sc** – für Spark Kontext
    * **zur SqlContext** - für Struktur Kontext


    Ja, verfügen Sie möglicherweise nicht wie die Kontexte festlegen mit den folgenden Anweisungen ausführen:

        ###################################################
        # YOU DO NOT NEED TO RUN THIS WITH THE NEW KERNELS
        ###################################################
        sc = SparkContext('yarn-client')
        sqlContext = HiveContext(sc)

    Stattdessen können Sie die voreingestellten Kontexten direkt in Ihrer Anwendung verwenden.
    
2. **Zelle Magics**. PySpark Kernel enthält einige vordefinierte "magics", welche sind spezielle Befehle, die Sie aufrufen können, mit `%%` (z. B. `%%MAGIC` <args>). Der magische Befehl muss das erste Wort in einer Zelle Code werden und mehrere Zeilen von Inhalt zu ermöglichen. Das Wort magische sollte das erste Wort in der Zelle. Nichts vor der magische, auch Kommentare hinzufügen, wird einen Fehler verursachen.   Weitere Informationen Magics, finden Sie [hier](http://ipython.readthedocs.org/en/stable/interactive/magics.html).

    Die folgende Tabelle enthält die verschiedenen Magics über die Kernel verfügbar.

  	| Magische     | Beispiel                         | Beschreibung  |
  	|-----------|---------------------------------|--------------|
  	| Hilfe      | `%%help`                            | Generiert eine Tabelle mit verfügbaren Magics mit Beispiel und Beschreibung |
  	| Info      | `%%info`                          | Ausgaben Sitzungsinformationen für den aktuellen Livius Endpunkt |
  	| Konfigurieren | `%%configure -f`<br>`{"executorMemory": "1000M"`,<br>`"executorCores": 4`} | Konfiguriert die Parameter für das Erstellen einer Sitzung. Die Kennzeichnung Force (-f) obligatorisch ist, wenn bereits eine Sitzung erstellt wurde, und die Sitzung gelöscht und neu erstellt werden. Prüfen Sie [Liviuss Beitrag /sessions Textkörper anfordern](https://github.com/cloudera/livy#request-body) für eine Liste mit gültigen Parametern aus. Parameter müssen als eine JSON-Zeichenfolge übergeben werden und in der nächsten Zeile nach der magische werden müssen, wie in der Beispielspalte dargestellt. |
  	| SQL       |  `%%sql -o <variable name>`<br> `SHOW TABLES`    | Führt eine Abfrage Struktur gegen die SqlContext. Wenn die `-o` Parameter übergeben wird, ist das Ergebnis der Abfrage im beibehalten der % lokalen Python Kontext als eine [Pandas](http://pandas.pydata.org/) Dataframe.   |
  	| lokale     |     `%%local`<br>`a=1`              | Gesamten Code in nachfolgenden Zeilen wird lokal ausgeführt werden. Code muss gültigen Python-Code sein. |
  	| Protokolle      | `%%logs`                        | Gibt die Protokolle für die aktuelle Sitzung Livius aus.  |
  	| Löschen    | `%%delete -f -s <session number>` | Löscht eine bestimmte Sitzung des aktuellen Livius Endpunkts an. Beachten Sie, dass Sie die Sitzung für den Kernel selbst initiiert wird nicht löschen können. |
  	| Aufräumen   | `%%cleanup -f`                    | Löscht alle Sitzungen für den aktuellen Livius Endpunkt, einschließlich des Notizbuchs Sitzung an. Die Kraft Kennzeichnung -f ist erforderlich.  |

    >[AZURE.NOTE] Zusätzlich zu den Magics durch den PySpark Kernel hinzugefügt, können Sie auch die [integrierten IPython Magics](https://ipython.org/ipython-doc/3/interactive/magics.html#cell-magics), einschließlich `%%sh`. Sie können die `%%sh` magische Skripts und Codeblock auf die Cluster Headnode ausführen. 

3. **Automatische Visualisierung**. **Pyspark** Kernel Visualisierung automatisch die Ausgabe der Struktur und SQL-Abfragen. Sie haben die Möglichkeit, die zwischen verschiedenen Typen von Visualisierungen, einschließlich der Tabelle, Kreis-, Linien-, Bereich, Leiste auswählen.

## <a name="parameters-supported-with-the-sql-magic"></a>Parameter unterstützt, mit der % Sql magische

Die % Sql magische unterstützt verschiedene Parameter, die Sie verwenden können, um die Art der Ausgabe zu steuern, die Sie erhalten, wenn Sie Abfragen ausführen. Die folgende Tabelle enthält die Ausgabe an.

| Parameter     | Beispiel                         | Beschreibung  |
|-----------|---------------------------------|--------------|
| -o      | `-o <VARIABLE NAME>`                          | Verwenden Sie diesen Parameter das Ergebnis der Abfrage beibehalten werden die % lokale Python Kontext als eine [Pandas](http://pandas.pydata.org/) Dataframe. Der Name der Variablen Dataframe ist der Name, den Sie angeben. |
| f-      | `-q`                          | Verwenden Sie diese Visualisierungen für die Zelle zu deaktivieren. Wenn Sie nicht automatisch Visualisieren von den Inhalt einer Zelle und einfach erfassen Sie es als eine Dataframe, und klicken Sie dann verwenden möchten möchten `-q -o <VARIABLE>`. Wenn Sie Visualisierungen deaktivieren, ohne die Ergebnisse erfassen möchten (z. B. für SQL-Abfrage ausführen, mit der Seite Effekte wie einer `CREATE TABLE` Anweisung), verwenden Sie einfach `-q` ohne Angabe eines `-o` Argument. |
| m-       |  `-m <METHOD>`    | Darin **Methode** **machen** oder **Stichprobe** (der Standardwert liegt **Optimieren**). Ist die Methode **Übernehmen**, wählt der Kernel Elemente im oberen Bereich der Datenmenge Ergebnis durch die maximale Anzahl der Zeilen (später in dieser Tabelle beschrieben) angegeben. Wenn die Methode **Beispiel**ist, hören Kernel zufällig Elemente der Datenmenge entsprechend `-r` Parameter, die als Nächstes in dieser Tabelle beschrieben.   |
| -r     |     `-r <FRACTION>`            | Hier ist **TEILER** eine Gleitkommazahl zwischen 0,0 und 1,0. Wenn die Stichprobe Methode für die SQL-Abfrage ist `sample`, und klicken Sie dann der Kernel zufällig den angegebenen Teil der Elemente das Resultset für Sie; Beispiele z. B., wenn Sie eine SQL-Abfrage mit den Argumenten ausführen `-m sample -r 0.01`, und klicken Sie dann 1 % Ergebniszeilen zufällig aufgenommen werden sollen. |
| -n      | `-n <MAXROWS>`                        | **Maximale Anzahl der Zeilen** ist eine ganze Zahl. Der Kernel wird die Anzahl der Ausgabezeilen, die **maximale Anzahl der Zeilen**zu beschränken. Wenn die **maximale Anzahl der Zeilen** wie **-1**eine negative Zahl ist, wird die Anzahl der Zeilen im Resultset nicht beschränkt werden. |

**Beispiel:**

    %%sql -q -m sample -r 0.1 -n 500 -o query2 
    SELECT * FROM hivesampletable

Die oben angegebenen Anweisung geschieht Folgendes:

* Wählt alle Datensätze aus **Hivesampletable**.
* Da wir verwenden Sie - Q, schaltet er Auto-Visualisierung.
* Da wir verwenden `-m sample -r 0.1 -n 500` zufällig Beispiele 10 % der Zeilen in der Hivesampletable und begrenzt die Größe des Ergebnisses 500 Zeilen festlegen.
* Schließlich, da wir verwendet `-o query2` auch speichert die Ausgabe in einer Dataframe **query2**bezeichnet.
    

## <a name="considerations-while-using-the-new-kernels"></a>Aspekte beim Verwenden der neuen kernels

Dem Kernel, die Sie verwenden (PySpark oder Spark) verlassen Notizbücher ausgeführt werden verbraucht Clusterressourcen.  Mit den folgenden Kerneln, da die Kontexte voreingestellte sind, beenden einfach die Notizbücher nicht abbrechen im Kontext und somit auch die Cluster-Ressourcen weiterhin verwendet werden. Mit den Kerneln PySpark und Spark empfiehlt sich das wäre des Notizbuchs Menü ' **Datei** ' die Option **Schließen und Halt** verwenden. Bricht ab, im Kontext, und dann das Notizbuch beendet.    


## <a name="show-me-some-examples"></a>Einige Beispiele anzeigen

Wenn Sie ein Notizbuch Jupyter öffnen, sehen Sie zwei verfügbaren Ordner auf der Stammebene.

* Der Ordner **PySpark** weist Stichprobe Notizbücher, die den neuen **Python** Kernel verwenden.
* Der Ordner **Scala** weist Stichprobe Notizbücher, die den neuen **Spark** Kernel verwenden.

Sie können das Notizbuch **00 - [lesen zuerst] Spark magische Kernelfeatures** aus dem Ordner **PySpark** oder **Spark** der verschiedenen verfügbaren Magics lernen öffnen. Sie können auch die anderen Stichprobe Notizbücher verfügbar unter den beiden Ordnern verwenden, erfahren, wie in anderen Szenarien verwenden Jupyter Notizbücher mit HDInsight Spark Cluster zu erzielen.

## <a name="where-are-the-notebooks-stored"></a>Wo sind die Notizbücher gespeichert?

Mit dem Speicherkonto unter dem Ordner **/HdiNotebooks** Cluster zugeordnet werden Jupyter Notizbüchern gespeichert.  Notizbücher, Textdateien und Ordnern, die Sie in Jupyter erstellen zugänglich von WASB.  Beispielsweise, wenn Sie Jupyter verwenden, um einen Ordner **festgelegten** und einem Notizbuch **myfolder/mynotebook.ipynb**erstellen, können Sie Aufrufen dieses Notizbuch am `wasbs:///HdiNotebooks/myfolder/mynotebook.ipynb`.  Auch im umgekehrten Fall WAHR, d. h., wenn Sie ein Notizbuch direkt in Ihrem Speicherkonto bei hochladen, `/HdiNotebooks/mynotebook1.ipynb`, das Notizbuch wird ebenfalls von Jupyter sichtbar sein.  Notizbücher bleibt im Speicherkonto auch nach der Cluster gelöscht wird.

Der Ihrem Erwerb Notizbücher mit dem Konto Storage gespeichert sind, ist mit HDFS kompatibel. Dies, wenn Sie in verwendbare Cluster SSH Management Befehle wie in der folgenden Datei:

    hdfs dfs -ls /HdiNotebooks                            # List everything at the root directory – everything in this directory is visible to Jupyter from the home page
    hdfs dfs –copyToLocal /HdiNotebooks                 # Download the contents of the HdiNotebooks folder
    hdfs dfs –copyFromLocal example.ipynb /HdiNotebooks   # Upload a notebook example.ipynb to the root folder so it’s visible from Jupyter


Falls es Probleme, die Zugriff auf das Konto Speicherplatz für den Cluster liegen, die Notizbücher auch auf die Headnode gespeichert `/var/lib/jupyter`.

## <a name="supported-browser"></a>Unterstützte browser
Jupyter Notizbücher gegen HDInsight Spark Cluster ausgeführt werden nur auf Google Chrome unterstützt.

## <a name="feedback"></a>Feedback

Die neue Kernels sind in der Entwicklung einer Phase, und es werden über einen Zeitraum enger. Dies bedeutet auch, dass APIs wie diese Kernels enger ändern kann. Wir möchten Feedback schätzen, die Sie während der Verwendung dieser neuen Kernels aufweisen. Dies wird in der endgültigen Version von diese Kernels Strukturierung sehr nützlich sein. Sie können Ihre Kommentare/Feedback unter dem Abschnitt " **Kommentare** " am Ende dieses Artikels lassen.


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

* [Verwenden von externen Paketen mit Jupyter-Notizbüchern](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

* [Jupyter auf Ihrem Computer installieren und Verbinden mit einem HDInsight Spark cluster](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Verwalten von Ressourcen

* [Verwalten von Ressourcen für den Apache Spark Cluster in Azure HDInsight](hdinsight-apache-spark-resource-manager.md)

* [Verfolgen und Debuggen Aufträge in einem Apache Spark Cluster in HDInsight](hdinsight-apache-spark-job-debugging.md)

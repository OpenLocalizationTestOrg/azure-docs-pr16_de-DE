<properties 
    pageTitle="Jupyter Notizbuch auf Ihrem Computer installieren und verbinden Sie es mit einem Cluster HDInsight Spark | Microsoft Azure" 
    description="Informationen Sie zum Notizbuch Jupyter lokal auf Ihrem Computer installieren und verbinden Sie es mit einem Apache Spark Cluster auf Azure HDInsight." 
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
    ms.date="09/26/2016" 
    ms.author="nitinme"/>


# <a name="install-jupyter-notebook-on-your-computer-and-connect-to-apache-spark-cluster-on-hdinsight-linux"></a>Jupyter Notizbuch auf Ihrem Computer installieren und Verbinden mit Apache Spark Cluster auf HDInsight Linux

In diesem Artikel erfahren Sie, wie Jupyter Notizbuch mit den benutzerdefinierten PySpark (für Python) und (für Scala) Spark Kernels mit Spark magische installieren und verbinden das Notizbuch zu einem Cluster HDInsight. Es kann eine Reihe von Gründen Jupyter auf Ihrem lokalen Computer installiert sein, und kann auch einige Nachteile. Eine Liste der Gründe und Herausforderung finden Sie im Abschnitt, die am Ende dieses Artikels [Warum sollten Sie Jupyter auf meinem Computer installieren](#why-should-i-install-jupyter-on-my-computer) .

Es gibt drei wichtige Schritte bei der Installation von Jupyter und die magische Spark auf Ihrem Computer.

* Installieren von Jupyter Notizbuch
* Installieren Sie die PySpark und Spark Kernel mit der Spark magische
* Konfigurieren von Spark magische Spark Cluster auf HDInsight zugreifen

Weitere Informationen zu den benutzerdefinierten Kernels und die Spark magische für Jupyter Notizbücher mit HDInsight Cluster verfügbar finden Sie unter [Kernels für Jupyter Notizbücher mit Apache Spark Linux Cluster auf HDInsight verfügbar](hdinsight-apache-spark-jupyter-notebook-kernels.md).

##<a name="prerequisites"></a>Erforderliche Komponenten

Die hier aufgeführten Vorkenntnisse sind nicht für die Installation von Jupyter. Dies sind für das Notizbuch Jupyter zu einem HDInsight Cluster verbinden, nachdem Sie das Notizbuch installiert ist.

- Ein Azure-Abonnement. Finden Sie [kostenlose Testversion Azure abrufen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Eine Apache Spark Cluster auf HDInsight Linux. Anweisungen finden Sie unter [Erstellen von Apache Spark Cluster in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="install-jupyter-notebook-on-your-computer"></a>Installieren Sie Jupyter Notizbuch auf Ihrem Computer.

Sie müssen Python installieren, bevor Sie Jupyter Notizbücher installieren können. Sowohl Python und Jupyter stehen als Bestandteil der [Ananconda Verteilung zurück](https://www.continuum.io/downloads). Wenn Sie Anaconda installiert haben, installieren Sie tatsächlich eine Verteilung der Python. Nachdem Anaconda installiert wurde, fügen Sie die Installation Jupyter, indem Sie einen Befehl ausführen. Dieser Abschnitt enthält die Anweisungen, die Sie folgen müssen.

1. Laden Sie das [Installationsprogramm Anaconda](https://www.continuum.io/downloads) für Ihre Plattform, und führen Sie Setup. Stellen Sie beim Ausführen des Setupassistenten sicher, dass Sie die Option zum Anaconda der PATH-Variable hinzuzufügen.

2. Führen Sie den folgenden Befehl aus, um Jupyter zu installieren.

        conda install jupyter

    Weitere Informationen zu Installting Jupyter finden Sie unter [Installieren von Jupyter Anaconda verwenden](http://jupyter.readthedocs.io/en/latest/install.html).

## <a name="install-the-kernels-and-spark-magic"></a>Installieren Sie die Kernels und Spark magische

Anweisungen zum Installieren der Spark magische PySpark und Spark-Kernels finden Sie unter der [Dokumentation Sparkmagic](https://github.com/jupyter-incubator/sparkmagic#installation) auf GitHub.

## <a name="configure-spark-magic-to-access-the-hdinsight-spark-cluster"></a>Konfigurieren von Spark magische Cluster HDInsight Spark zugreifen

In diesem Abschnitt Konfigurieren Sie die Spark magische, die Sie zuvor zum Verbinden mit einem Apache Spark Cluster, die Sie bereits in Azure HDInsight erstellt haben müssen installiert haben.

1. Die Jupyter Konfiguration-Informationen werden in der Regel im Verzeichnis Start Benutzer gespeichert. Um auf einer beliebigen Plattform OS home-Verzeichnis zu suchen, geben Sie die folgenden Befehle aus.

    Starten Sie die Verwaltungsshell Python. Geben Sie in einem Befehlsfenster Folgendes ein:

        python

    Geben Sie den folgenden Befehl aus, um herauszufinden, private Verzeichnis auf die Verwaltungsshell Python.

        import os
        print(os.path.expanduser('~'))

2. Navigieren Sie zu Start Verzeichnis aus, und erstellen Sie einen Ordner namens **.sparkmagic** , wenn es nicht bereits vorhanden ist.

3. Innerhalb des Ordners erstellen Sie eine Datei mit der Bezeichnung **config.json** , und fügen Sie den folgenden JSON-Codeausschnitt darin enthaltenen.

        {
          "kernel_python_credentials" : {
            "username": "{USERNAME}",
            "base64_password": "{BASE64ENCODEDPASSWORD}",
            "url": "https://{CLUSTERDNSNAME}.azurehdinsight.net/livy"
          },
          "kernel_scala_credentials" : {
            "username": "{USERNAME}",
            "base64_password": "{BASE64ENCODEDPASSWORD}",
            "url": "https://{CLUSTERDNSNAME}.azurehdinsight.net/livy"
          }
        }

4. Ersetzen Sie **{USERNAME}**, **{CLUSTERDNSNAME}**, **{BASE64ENCODEDPASSWORD}** und mit den entsprechenden Werten aus. Eine Reihe von Dienstprogrammen in Ihrer bevorzugten Programmiersprache oder online können Sie ein base64-codierte Kennwort für Ihr Kennwort verbindenden generieren. Ein einfacher Python Ausschnitt über die Befehlszeile ausführen würde so aussehen:

        python -c "import base64; print(base64.b64encode('{YOURPASSWORD}'))"

5. Starten Sie Jupyter. Verwenden Sie den folgenden Befehl über die Befehlszeile.

        jupyter notebook

6. Stellen Sie sicher, dass Sie den Cluster mithilfe des Notizbuchs Jupyter verbinden können und die magische Spark verfügbar mit den Kerneln verwendet werden können. Führen Sie die folgenden Schritte aus.

    1. Erstellen eines neuen Notizbuchs an. Klicken Sie in der rechten Ecke auf **neu**. Der Standard-Kernel **Python2** und die zwei neue Kernels, die Sie installieren, **PySpark** und **Spark**auftreten.

        ![Erstellen eines neuen Jupyter Notizbuchs] (./media/hdinsight-apache-spark-jupyter-notebook-install-locally/jupyter-kernels.png "Erstellen eines neuen Jupyter Notizbuchs")

    
        Klicken Sie auf **PySpark**.


    2. Führen Sie den folgenden Codeausschnitt.

            %%sql
            SELECT * FROM hivesampletable LIMIT 5

        Wenn Sie die Ausgabe erfolgreich abrufen können, wird eine Verbindung mit dem Cluster HDInsight getestet.

    >[AZURE.TIP] Wenn Sie die Konfiguration Notizbuch an einen anderen Cluster Verbindung aktualisieren möchten, aktualisieren Sie die config.json mit den neuen Satz von Werten, wie in Schritt 3 oben gezeigt. 

## <a name="why-should-i-install-jupyter-on-my-computer"></a>Warum sollte ich Jupyter auf meinem Computer installieren?

Es kann eine Reihe von Gründen, warum Sie möchten möglicherweise Jupyter auf Ihrem Computer installieren und verbinden Sie diese mit einem Spark Cluster auf Hdinsight, sein.

* Obwohl Jupyter Notizbücher bereits auf dem Cluster Spark in Azure HDInsight verfügbar sind, stellt die Jupyter auf Ihrem Computer installieren Sie die Option zum Erstellen Ihrer Notizbücher lokal, testen Sie die Anwendung anhand einer laufenden Cluster und Laden Sie die Notizbücher mit dem Cluster. Sie können zum Hochladen der Notizbücher auf dem Cluster hochladen, die mit dem Jupyter-Notizbuch, das ausgeführt wird oder die Cluster oder diese in den Ordner /HdiNotebooks Cluster zugeordnete Speicherplatz Konto speichern. Weitere Informationen zum Speichern von Notizbüchern auf dem Cluster finden Sie unter [wo Jupyter Notizbücher gespeichert werden](hdinsight-apache-spark-jupyter-notebook-kernels.md#where-are-the-notebooks-stored)?
* Mit der verfügbaren Notizbücher können lokal, Sie auf verschiedene Spark Cluster basierend auf Ihrer Anwendung Anforderung verbinden.
* GitHub können Sie ein Datenquellen-Steuerelement System implementieren und Versionskontrolle für die Notizbücher haben. Sie können auch eine Umgebung für die Zusammenarbeit lassen, in dem mehrere Benutzer am selben Notizbuch bearbeiten können.
* Sie können mit Notizbüchern lokal arbeiten, ohne selbst einen Cluster nach oben. Sie benötigen nur dann einen Cluster So testen Sie Ihre Notizbücher gegen, um nicht zu Ihrer Notizbücher oder eine Entwicklungsumgebung manuell zu verwalten.
* Möglicherweise einfacher zu Ihrer eigenen lokalen Entwicklungsumgebung konfigurieren, als es ist, die Jupyter Installation auf dem Cluster konfigurieren.  Sie können alle der Software nutzen, die Sie lokal installiert haben, ohne einen oder mehrere remote Cluster konfigurieren.

>[AZURE.WARNING] Mit Jupyter auf Ihrem lokalen Computer installiert ist können mehrere Benutzer gleichzeitig am selben Notizbuch auf dem gleichen Spark Cluster ausführen. In diesem Fall werden mehrere Livius Sitzungen erstellt. Wenn Sie ein Problem stoßen und die debuggen möchten, werden, dass eine komplexe Aufgabe, welche Livius Sitzung nachzuverfolgen, welche Benutzer gehört.




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

* [Verwenden von externen Paketen mit Jupyter-Notizbüchern](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

### <a name="manage-resources"></a>Verwalten von Ressourcen

* [Verwalten von Ressourcen für den Apache Spark Cluster in Azure HDInsight](hdinsight-apache-spark-resource-manager.md)

* [Verfolgen und Debuggen Aufträge in einem Apache Spark Cluster in HDInsight](hdinsight-apache-spark-job-debugging.md)

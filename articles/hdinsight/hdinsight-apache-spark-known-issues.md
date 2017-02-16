<properties 
    pageTitle="Bekannte Probleme bei der Apache Spark in HDInsight | Microsoft Azure" 
    description="Bekannte Probleme von Apache Spark in HDInsight." 
    services="hdinsight" 
    documentationCenter="" 
    authors="mumian" 
    manager="jhubbard" 
    editor="cgronlun"
    tags="azure-portal"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/25/2016" 
    ms.author="nitinme"/>

# <a name="known-issues-for-apache-spark-cluster-on-hdinsight-linux"></a>Bekannte Probleme bei der Apache Spark Cluster auf HDInsight Linux

Dieses Dokument behält Überblick über die bekannten Probleme für die öffentliche HDInsight Spark-Vorschau.  

##<a name="livy-leaks-interactive-session"></a>Livius verliert interaktive Sitzung
 
Wenn Livius mit einer interaktiven Sitzung (aus Ambari oder aufgrund Headnode 0 virtuellen Computern Neustart) noch aktiv neu gestartet wird, wird eine Sitzung interaktiven Auftrag Verluste auftreten. Aus diesem Grund neue Aufträge können hängen im Zustand akzeptiert und können nicht gestartet werden.

**Reduzierung:**

Gehen Sie zum Umgehen dieses Problem folgendermaßen vor:

1. SSH in Headnode. 
2. Führen Sie den folgenden Befehl aus, um die Anwendung IDs der interaktiven Einzelvorgänge bis Livius Schritte finden. 

        yarn application –list

    Die Namen, dass Livius, wenn die Einzelvorgänge mit einer Livius interaktive Sitzung ohne explizite Namen gestartet wurden angegeben für Livius Sitzung durch Jupyter Notizbuch gestartet, sind Standard-Position, beginnt der Auftragsnamen mit Remotesparkmagics_ *. 

3. Führen Sie den folgenden Befehl, diese Aufträge zu beenden. 

        yarn application –kill <Application ID>

Neue Aufträge werden gestartet, ausgeführt. 

##<a name="spark-history-server-not-started"></a>Spark Verlauf Server nicht gestartet 

Spark Verlauf Server wird nicht automatisch gestartet werden, nachdem ein Cluster erstellt wird.  

**Reduzierung:** 

Starten Sie den Verlauf-Server manuell aus Ambari.

## <a name="permission-issue-in-spark-log-directory"></a>Berechtigungsproblem in Spark Log-Verzeichnis 

Wenn Hdiuser ein Projekt mit Spark-submit übermittelt, ist es ein Fehler java.io.FileNotFoundException: /var/log/spark/sparkdriver_hdiuser.log (Zugriff verweigert) und das Protokoll Treiber nicht geschrieben. 

**Reduzierung:**
 
1. Fügen Sie in der Gruppe Hadoop Hdiuser hinzu. 
2. Bieten Sie 777 Berechtigungen auf /var/log/spark nach der Clustererstellung. 
3. Aktualisieren der Standort Spark Ambari verwenden, um ein Verzeichnis mit Berechtigungen 777 sein.  
4. Ausführen Spark-submit als Sudo.  

## <a name="issues-related-to-jupyter-notebooks"></a>Probleme bei der Jupyter Notizbücher

Im folgenden werden einige bekannte Probleme bei der Jupyter Notizbücher.


### <a name="notebooks-with-non-ascii-characters-in-filenames"></a>Notizbücher mit ASCII-Zeichen in Dateinamen

Jupyter-Notizbücher, die in Spark HDInsight Cluster verwendet werden können, darf keine ASCII-Zeichen in Dateinamen haben. Wenn Sie versuchen, eine Datei über die Jupyter UI hochladen, die weist einen nicht-ASCII-Dateinamen, wird es automatisch fehl (d. h., Jupyter verhindert, dass Sie die Datei hochladen, aber es wird keinen sichtbaren Fehler entweder auslösen). 

### <a name="error-while-loading-notebooks-of-larger-sizes"></a>Fehler beim Laden der größere Notizbücher

Möglicherweise wird einen Fehler angezeigt **`Error loading notebook`** beim Laden des Notizbücher, die umfangreicher sind.  

**Reduzierung:**

Wenn dieser Fehler zurückgegeben wird, bedeutet dies nicht, dass Ihre Daten beschädigt oder verloren.  Notizbücher werden weiterhin auf dem Datenträger in `/var/lib/jupyter`, und Sie SSH zum Cluster darauf zugreifen können. Sie können Ihre Notizbücher aus Ihrem Cluster als Sicherung für zur Vermeidung von Datenverlust wichtigen Daten im Notizbuch auf Ihrem Computer (mit SCP oder WinSCP) kopieren. Anschließend können Sie SSH Tunnel in Ihrer Headnode am Port 8001 zu Jupyter der Zugriff auf das Gateway ein.  Hier können Sie die Ausgabe Ihres Notizbuchs deaktivieren und erneut speichern, um die Größe des Notizbuchs zu minimieren.

Um diesen Fehler zukünftig zu verhindern, müssen Sie einige bewährte Methoden folgen:

* Es ist wichtig, um die Größe des Notizbuchs gering zu halten. Keine Ausgabe aus Ihrer Spark Aufträge, die an Jupyter gesendet wurde, wird im Notizbuch beibehalten.  Es ist eine bewährte Methode mit Jupyter im Allgemeinen Ausführung zu vermeiden `.collect()` auf große RDD oder Dataframes; Wenn Sie eine RDDs Inhalts einsehen möchten, stattdessen Ausführung `.take()` oder `.sample()` , damit Ihre Ausgabe nicht zu groß geraten.
* Auch wenn Sie ein Notizbuch speichern, ausgeben deaktivieren alle Zellen klicken, um die Größe zu verringern.

### <a name="notebook-initial-startup-takes-longer-than-expected"></a>Notizbuch Starts dauert länger als erwartet 

Erste Code-Anweisung in Jupyter Notizbuch Spark magische kann mehr als eine Minute dauern.  

**Erläuterung:**
 
Dies geschieht, da bei die erste Zelle der Code ausgeführt wird. Im Hintergrund initiiert diese Sitzungskonfiguration und Spark, SQL und Struktur Kontexten festgelegt werden. Nach dem folgenden Kontexten festgelegt sind, die erste Anweisung ausgeführt wird, und dies den Eindruck, die erzeugt die Anweisung sehr lange gedauert ausführen.

### <a name="jupyter-notebook-timeout-in-creating-the-session"></a>Jupyter Notizbuch Timeout in die Sitzung erstellen

Wenn keine Ressourcen Spark Cluster ist, werden die Spark und Pyspark Kernel im Notizbuch Jupyter Timeout versuchen, die Sitzung zu erstellen. 

**Problembehebungen:** 

1. Geben Sie Ihren Cluster Spark, indem Sie einige Ressourcen frei:

    - Beenden andere Notizbücher Spark durch das Schließen und Halt Menü öffnen, oder klicken Sie im Notizbuch-Explorer war(en) auf ein.
    - Beenden andere Programme Spark aus aus.

2. Starten Sie erneut das Notizbuch, das Sie starten möchten. Genügend Ressourcen sollten Sie zum Erstellen einer Sitzung jetzt verfügbar sein.

##<a name="see-also"></a>Siehe auch

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

* [Jupyter auf Ihrem Computer installieren und Verbinden mit einem HDInsight Spark cluster](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Verwalten von Ressourcen

* [Verwalten von Ressourcen für den Apache Spark Cluster in Azure HDInsight](hdinsight-apache-spark-resource-manager.md)

* [Verfolgen und Debuggen Aufträge in einem Apache Spark Cluster in HDInsight](hdinsight-apache-spark-job-debugging.md)

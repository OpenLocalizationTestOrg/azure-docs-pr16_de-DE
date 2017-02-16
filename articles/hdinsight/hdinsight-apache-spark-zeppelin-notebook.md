<properties 
    pageTitle="Verwenden von Zeppelin Notizbücher mit Spark Cluster auf HDInsight Linux | Microsoft Azure" 
    description="Eine schrittweise Anleitung zum Zeppelin Notizbücher mit Spark Cluster auf HDInsight Linux verwenden." 
    services="hdinsight" 
    documentationCenter="" 
    authors="nitinme" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/05/2016" 
    ms.author="nitinme"/>


# <a name="use-zeppelin-notebooks-with-apache-spark-cluster-on-hdinsight-linux"></a>Verwenden von Zeppelin Notizbücher mit Apache Spark Cluster auf HDInsight Linux

HDInsight Spark Cluster enthalten, Zeppelin-Notizbücher, die Sie verwenden können, um Spark Aufträge ausgeführt wird. In diesem Artikel erfahren Sie, wie Sie das Notizbuch Zeppelin auf eine HDInsight Cluster verwenden.


**Voraussetzungen für:**

* Ein Azure-Abonnement. Finden Sie [kostenlose Testversion Azure abrufen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

* Ein Cluster Apache Spark. Anweisungen finden Sie unter [Erstellen von Apache Spark Cluster in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="launch-a-zeppelin-notebook"></a>Starten eines Notizbuchs Zeppelin

1. Klicken Sie aus dem Spark Cluster Blade auf **Cluster Dashboard**, und klicken Sie dann auf **Zeppelin Notizbuch**. Wenn Sie dazu aufgefordert werden, geben Sie die Administrator-Anmeldeberechtigungen für den Cluster aus.

    > [AZURE.NOTE] Sie möglicherweise auch das Notizbuch Zeppelin für Ihren Cluster erreichen, indem Sie den folgenden URL in Ihrem Browser öffnen. Ersetzen Sie __CLUSTERNAME__ mit dem Namen der Cluster aus:
    >
    > `https://CLUSTERNAME.azurehdinsight.net/zeppelin`

2. Erstellen eines neuen Notizbuchs an. Klicken Sie auf **Notizbuch**aus dem Kopfzeilenbereich, und klicken Sie dann auf **Neue Notiz erstellen**.

    ![Erstellen eines neuen Zeppelin Notizbuchs] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.createnewnote.png "Erstellen eines neuen Zeppelin Notizbuchs")

    Geben Sie einen Namen für das Notizbuch, und klicken Sie dann auf **Notiz zu erstellen**.

3. Darüber hinaus stellen Sie sicher, dass die Kopfzeile Notizbuch einen verbundenen Status zeigt. Es wird durch einen grünen Punkt in der oberen rechten Ecke gekennzeichnet.

    ![Zeppelin Notizbuch status] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.newnote.connected.png "Zeppelin Notizbuch status")

4. Laden Sie Beispieldaten in eine temporäre Tabelle ein. Wenn Sie einen Cluster Spark in HDInsight erstellen, wird die Daten Beispieldatei **hvac.csv**, auf das zugehörige Speicher-Konto unter **\HdiSamples\SensorSampleData\hvac**kopiert.

    Fügen Sie in den leeren Absatz, der standardmäßig in das neue Notizbuch erstellt wird den folgenden Codeausschnitt.

        %livy.spark
        //The above magic instructs Zeppelin to use the Livy Scala interpreter

        // Create an RDD using the default Spark context, sc
        val hvacText = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
        
        // Define a schema
        case class Hvac(date: String, time: String, targettemp: Integer, actualtemp: Integer, buildingID: String)
        
        // Map the values in the .csv file to the schema
        val hvac = hvacText.map(s => s.split(",")).filter(s => s(0) != "Date").map(
            s => Hvac(s(0), 
                    s(1),
                    s(2).toInt,
                    s(3).toInt,
                    s(6)
            )
        ).toDF()
        
        // Register as a temporary table called "hvac"
        hvac.registerTempTable("hvac")
        
    Drücken Sie **UMSCHALT + EINGABETASTE** , oder klicken Sie auf **die Wiedergabeschaltfläche für den Codeausschnitt Ausführen des Absatzes** . Der Status auf der rechten Ecke des Absatzes sollte Fortschreiten sofort, Ausstehend Ausführung in beendet. Die Ausgabe wird am unteren Rand des gleichen Absatzes. Der Screenshot sieht wie folgt aus:

    ![Erstellen Sie eine temporäre Tabelle aus unformatierten Daten] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.note.loaddDataintotable.png "Erstellen Sie eine temporäre Tabelle aus unformatierten Daten")

    Sie können auch einen Titel jeder Absatz bereitstellen. Klicken Sie auf das Symbol **Einstellungen** , und klicken Sie dann auf **Achsentitel anzeigen**, aus der rechten Ecke.

5. Sie können jetzt Spark SQL-Anweisungen für die Tabelle **HKL-System** ausführen. Fügen Sie die folgende Abfrage in einem neuen Absatz ein. Die Abfrage ruft die Gebäude-ID sowie die Differenz zwischen dem Ziel und den tatsächlichen Temperaturen für jedes Gebäude auf ein angegebenes Datum zurück. Drücken Sie **UMSCHALT + EINGABETASTE**.

        %sql
        select buildingID, (targettemp - actualtemp) as temp_diff, date from hvac where date = "6/1/13" 

    Die **% Sql** -Anweisung am Anfang weist das Notizbuch mit den Interpreter Livius Scala verwenden.

    Das folgende Bildschirmabbild zeigt die Ausgabe an.

    ![Ausführen eine Spark SQL-Anweisung, die mit dem Notizbuch] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.note.sparksqlquery1.png "Ausführen eine Spark SQL-Anweisung, die mit dem Notizbuch")

     Klicken Sie auf Anzeigeoptionen (Rechteck hervorgehoben) zum Umschalten zwischen verschiedenen Darstellungen für das gleiche Ergebnis. Klicken Sie auf **Einstellungen** , um welche stellt der Schlüssel und die Werte in der Ausgabe auszuwählen. Über das Bildschirmfoto verwendet **BuildingID** als Schlüssel und den Mittelwert der **Temp_diff** als Wert ein.

    
6. Sie können auch Spark-SQL-Anweisungen verwenden von Variablen in der Abfrage ausführen. Der nächste Ausschnitt wird gezeigt, wie Definieren einer Variablen zu speichern, **Temp**, in der Abfrage mit den möglichen Werten, dass Sie mit Abfragen möchten. Beim Ausführen der Abfrage wird automatisch eine Dropdownliste mit den Werten aufgefüllt, die Sie für die Variable angegeben haben.

        %sql
        select buildingID, date, targettemp, (targettemp - actualtemp) as temp_diff from hvac where targettemp > "${Temp = 65,65|75|85}" 

    Fügen Sie einen neuen Absatz dieser Ausschnitt, und drücken Sie **UMSCHALT + EINGABETASTE**. Das folgende Bildschirmabbild zeigt die Ausgabe an.

    ![Ausführen eine Spark SQL-Anweisung, die mit dem Notizbuch] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.note.sparksqlquery2.png "Ausführen eine Spark SQL-Anweisung, die mit dem Notizbuch")

    Für spätere Abfragen können Sie wählen Sie einen neuen Wert aus der Dropdownliste aus und führen Sie die Abfrage erneut aus. Klicken Sie auf **Einstellungen** , um welche stellt der Schlüssel und die Werte in der Ausgabe auszuwählen. Die oben aufgeführten Bildschirmaufnahme verwendet **BuildingID** als Schlüssel, den Mittelwert der **Temp_diff** als Wert und **Targettemp** wie die Gruppe ein.

7. Starten den Livius Interpreter zum Beenden der Anwendung erneut. Hierzu öffnen Sie Interpreter Einstellungen, indem Sie auf dem für die Anmeldung im Feld Benutzername in der oberen rechten Ecke, und klicken Sie dann auf **Interpreter**.

    ![Starten Sie interpreter] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Die Ausgabe Struktur")

2. Ausführen eines Bildlaufs zur Livius Interpreter Einstellungen, und klicken Sie dann auf **neu starten**.

    ![Starten Sie erneut die Livius intepreter] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.zeppelin.restart.interpreter.png "Starten der Zeppelin intepreter")

## <a name="how-do-i-use-external-packages-with-the-notebook"></a>Wie verwende ich externe Pakete mit dem Notizbuch?

Sie können das Notizbuch Zeppelin in Apache Spark Cluster auf HDInsight (Linux) konfigurieren, um externe, Community beigetragen Pakete verwenden, die nicht eingeschlossen Out-of-the-Box im Cluster sind. Sie können das [Maven Repository](http://search.maven.org/) für die vollständige Liste der Pakete suchen, die verfügbar sind. Sie können auch eine Liste der verfügbaren Paketen aus anderen Quellen abrufen. Eine vollständige Liste der Community beigetragen Pakete beträgt beispielsweise [Spark Pakete](http://spark-packages.org/)verfügbar.

In diesem Artikel sehen Sie, wie das [Spark-Csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) -Paket mit dem Notizbuch Jupyter verwendet werden.

1. Öffnen Sie die Interpreter-Einstellungen. Klicken Sie in der oberen rechten Ecke im Feld Benutzername auf dem für die Anmeldung, und klicken Sie dann auf **Interpreter**.

    ![Starten Sie interpreter] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Die Ausgabe Struktur")

2. Ausführen eines Bildlaufs zur Livius Interpreter Einstellungen, und klicken Sie dann auf **Bearbeiten**.

    ![Die Interpreter Einstellungen ändern] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-use-external-package-1.png "Die Interpreter Einstellungen ändern")

3. Fügen Sie einen neuen Product Key, aufgerufen **livy.spark.jars.packages** hinzu, und legen Sie den Wert in das Format `group:id:version`. Ja, wenn Sie das [Spark-Csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) -Paket verwenden möchten, müssen Sie festlegen die EINGABETASTE, um den Wert `com.databricks:spark-csv_2.10:1.4.0`.

    ![Die Interpreter Einstellungen ändern] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-use-external-package-2.png "Die Interpreter Einstellungen ändern")

    Klicken Sie auf **Speichern** , und starten Sie den Interpreter Livius.

4. **Tipp**: Wenn Sie zum Einblenden der zugehörigen verstehen möchten der Wert für den Schlüssel oben eingegebenen, hier wie.

    ein. Suchen Sie im Repository Maven das Paket ein. In diesem Lernprogramm verwendet haben wir [Spark-Csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).
    
    b. Sammeln Sie die Werte aus dem Repository für die **Gruppen-ID**, **ArtifactId**und **Version**.

    ![Verwenden von externen Pakete Jupyter Notizbuch] (./media/hdinsight-apache-spark-zeppelin-notebook/use-external-packages-with-jupyter.png "Verwenden von externen Pakete Jupyter Notizbuch")

    c. Verketten Sie die drei Werte, die von einem Doppelpunkt (**:**) getrennt ein.

        com.databricks:spark-csv_2.10:1.4.0

## <a name="where-are-the-zeppelin-notebooks-saved"></a>Wo sind die Zeppelin Notizbücher gespeichert?

Die Zeppelin Notizbücher werden in der Cluster Headnodes gespeichert. Ja, wenn Sie den Cluster löschen, werden die Notizbücher ebenfalls gelöscht werden. Wenn Sie Ihre Notizbücher zur späteren Verwendung auf einem anderen Cluster beibehalten möchten, müssen Sie diese exportieren, nachdem Sie die Einzelvorgänge ausgeführt haben. Klicken Sie auf das Symbol " **Exportieren** ", um ein Notizbuch zu exportieren, wie in der nachstehenden Abbildung gezeigt.

![Herunterladen des Notizbuchs] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-download-notebook.png "Herunterladen des Notizbuchs")

Dies speichert das Notizbuch als JSON-Datei in Ihrem Downloadspeicherort.

## <a name="livy-session-management"></a>Livius sitzungsverwaltung

Wenn Sie in Ihrem Notizbuch Zeppelin des ersten Absatzes mit Code ausführen, wird eine neue Livius Sitzung in Ihren Cluster HDInsight Spark erstellt. Diese Sitzung ist in allen Zeppelin Notizbüchern freigegeben, die Sie anschließend erstellen. Wenn Sie die Sitzung ist Livius aus irgendeinem Grund abgebrochen (Cluster Neustart usw.), werden Sie nicht zum Ausführen der Aufträge aus dem Notizbuch Zeppelin sein.

In diesem Fall müssen Sie die folgenden Schritte ausführen, bevor Sie beginnen können, Ausführen von Aufträgen aus einem Notizbuch Zeppelin. 

1. Starten den Interpreter Livius aus dem Notizbuch Zeppelin erneut. Hierzu öffnen Sie Interpreter Einstellungen, indem Sie auf dem für die Anmeldung im Feld Benutzername in der oberen rechten Ecke, und klicken Sie dann auf **Interpreter**.

    ![Starten Sie interpreter] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Die Ausgabe Struktur")

2. Ausführen eines Bildlaufs zur Livius Interpreter Einstellungen, und klicken Sie dann auf **neu starten**.

    ![Starten Sie erneut die Livius intepreter] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.zeppelin.restart.interpreter.png "Starten der Zeppelin intepreter")

3. Führen Sie eine Zelle Code aus einer vorhandenen Zeppelin Notizbuch aus. Dadurch wird eine neue Livius Sitzung im Cluster HDInsight erstellt.

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








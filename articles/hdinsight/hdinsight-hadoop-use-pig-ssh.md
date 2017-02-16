<properties
   pageTitle="Verwenden von Hadoop Schwein mit SSH auf einem Cluster HDInsight | Microsoft Azure"
   description="Erfahren Sie, wie eine Verbindung zu einem Hadoop Linux-basierten Cluster mit SSH, und klicken Sie dann mit dem Befehl Schwein Schwein Lateinisch Anweisungen interaktiv ausgeführt, oder als Stapelverarbeitung her."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/11/2016"
   ms.author="larryfr"/>

#<a name="run-pig-jobs-on-a-linux-based-cluster-with-the-pig-command-ssh"></a>Ausführen von Schwein Aufträge auf einem Linux-basierten Cluster mit dem Befehl Schwein (SSH)

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

In diesem Dokument werden Sie das Verfahren zum Herstellen einer Verbindung zu einem Cluster Linux-basierten Azure HDInsight mithilfe von Secure Shell (SSH), klicken Sie dann mit dem Befehl Schwein Schwein Lateinisch Anweisungen interaktiv auszuführen oder als Stapelverarbeitung durchzuführen.

Die Programmiersprache Schwein Lateinisch können Sie Transformationen beschreiben, die auf die Eingabedaten, um das gewünschte Ergebnis zu erzeugen angewendet werden.

> [AZURE.NOTE] Wenn Sie bereits mit der Verwendung von Linux-basierten Hadoop-Servern vertraut sind, aber noch neu bei HDInsight sind, finden Sie unter [Tipps für Linux-basierten HDInsight](hdinsight-hadoop-linux-information.md).

##<a name="a-idprereqaprerequisites"></a><a id="prereq"></a>Erforderliche Komponenten

Um die Schritte in diesem Artikel ausführen zu können, benötigen Sie Folgendes.

* Ein Cluster Linux-basierten HDInsight (Hadoop auf HDInsight).

* SSH-Client. Linux, Unix und Mac OS sollte mit einem SSH-Client nützlich sein. Windows-Benutzer müssen einen Client, z. B. [kitten](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)herunterladen.

##<a name="a-idsshaconnect-with-ssh"></a><a id="ssh"></a>Verbinden mit SSH

Schließen Sie an den vollqualifizierten Domänennamen (FULLY) von Ihren Cluster HDInsight mithilfe des Befehls SSH. Der vollqualifizierten Domänennamen werden auf dem Namen den Cluster dann zugewiesen **. azurehdinsight.net**. Beispielsweise würde Folgendes zu einem Cluster mit dem Namen **Myhdinsight**verbinden.

    ssh admin@myhdinsight-ssh.azurehdinsight.net

**Wenn Sie einen Zertifikatschlüssel für die Authentifizierung SSH bereitgestellt** , wenn Sie den HDInsight Cluster erstellt haben, müssen Sie den Speicherort der private Schlüssel auf Ihrem Clientsystem anzugeben.

    ssh admin@myhdinsight-ssh.azurehdinsight.net -i ~/mykey.key

**Wenn Sie ein Kennwort für die Authentifizierung SSH angegeben** , wenn Sie den HDInsight Cluster erstellt haben, müssen Sie das Kennwort ein bereitstellen.

Weitere Informationen zum Verwenden von SSH mit HDInsight finden Sie unter [Verwenden SSH mit Linux-basierten Hadoop auf HDInsight Linux, OS X und Unix](hdinsight-hadoop-linux-use-ssh-unix.md).

###<a name="putty-windows-based-clients"></a>Kitten (Windows-basierten Clients)

Windows bietet keinen integrierten SSH-Client. Es empfiehlt sich, mit **kitten**, die von [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)heruntergeladen werden kann.

Weitere Informationen zur Verwendung von kitten finden Sie unter [Verwenden SSH mit Linux-basierten Hadoop auf HDInsight von Windows ](hdinsight-hadoop-linux-use-ssh-windows.md).

##<a name="a-idpigause-the-pig-command"></a><a id="pig"></a>Verwenden Sie den Befehl Schwein

2. Nachdem die Verbindung hergestellt wurde, starten Sie die Schwein line Interface (CLI) mit dem folgenden Befehl ein.

        pig

    Nach kurzer Auftreten einer `grunt>` auffordern.

3. Geben Sie die folgende Anweisung ein.

        LOGS = LOAD 'wasbs:///example/data/sample.log';

    Dieser Befehl lädt den Inhalt der Datei sample.log in Protokolle an. Sie können den Inhalt der Datei mithilfe der folgenden anzeigen.

        DUMP LOGS;

4. Als Nächstes Transformieren der Daten durch Anwenden eines regulären Ausdrucks nur die Protokollstufe mithilfe der folgenden aus jedem Datensatz zu extrahieren.

        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;

    **Sichern** können Sie die Daten nach der Transformation angezeigt. Verwenden Sie in diesem Fall `DUMP LEVELS;`.

5. Weiterhin mithilfe der folgenden Aussagen Transformationen anzuwenden. Verwenden Sie `DUMP` das Ergebnis der Transformation nach jedem Schritt anzeigen.

    <table>
    <tr>
    <th>Anweisung</th><th>Was bedeutet</th>
    </tr>
    <tr>
    <td>FILTEREDLEVELS = FILTEREBENEN durch LOGLEVEL ist nicht null;</td><td>Entfernt Zeilen, die einen Nullwert für die Ebene enthalten und speichert die Ergebnisse in FILTEREDLEVELS.</td>
    </tr>
    <tr>
    <td>GROUPEDLEVELS = Gruppe FILTEREDLEVELS durch LOGLEVEL;</td><td>Die Zeilen gruppiert nach Logebene, und speichert die Ergebnisse in GROUPEDLEVELS.</td>
    </tr>
    <tr>
    <td>HÄUFIGKEITEN Foreach = GROUPEDLEVELS generieren Gruppe als LOGLEVEL, zählen (FILTEREDLEVELS. LOGLEVEL) als zählen;</td><td>Erstellt eine neue Gruppe jedes eindeutige Protokoll enthaltenen Daten Ebene Wert und wie oft auftritt. Dies wird in HÄUFIGKEITEN gespeichert.</td>
    </tr>
    <tr>
    <td>Ergebnis = Reihenfolge HÄUFIGKEITEN nach Anzahl Desc;</td><td>Bestellungen Log Ebenen nach Anzahl (absteigend), und speichert in Ergebnis.</td>
    </tr>
    </table>

6. Sie können auch die Ergebnisse einer Transformation speichern, mithilfe der `STORE` Anweisung. Angenommen, die folgenden speichert die `RESULT` in das Verzeichnis **/example/data/pigout** auf den standardmäßigen Speichercontainer für Ihren Cluster.

        STORE RESULT into 'wasbs:///example/data/pigout';

    > [AZURE.NOTE] Die Daten werden im angegebenen Verzeichnis in Dateien, die mit dem Namen **Webpart-00001...nnnnn**gespeichert. Wenn das Verzeichnis bereits vorhanden ist, erhalten Sie einen Fehler zurück.

7. Um die Aufforderung mühselige beenden möchten, geben Sie die folgende Anweisung ein.

        QUIT;

###<a name="pig-latin-batch-files"></a>Schwein Lateinisch Stapelverarbeitungsdateien

Den Befehl Schwein können Sie auch Schwein Lateinisch in einer Datei enthaltenen ausführen.

3. Nach dem Beenden der mühselige auffordern, verwenden Sie den folgenden Befehl zum Pipe STDIN in einer Datei namens **pigbatch.pig**. Diese Datei wird im Verzeichnis Start für das Konto erstellt werden, die Sie in der SSH-Sitzung angemeldet sind.

        cat > ~/pigbatch.pig

4. Geben Sie oder fügen Sie die folgenden Zeilen, und verwenden Sie dann STRG + D, wenn Sie fertig sind.

        LOGS = LOAD 'wasbs:///example/data/sample.log';
        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
        FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
        GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
        FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
        RESULT = order FREQUENCIES by COUNT desc;
        DUMP RESULT;

5. Führen Sie die Datei **pigbatch.pig** mithilfe des Befehls Schwein anhand der folgenden.

        pig ~/pigbatch.pig

    Nachdem die Stapelverarbeitung abgeschlossen ist, sollten Sie finden Sie unter der folgenden Ausgabe identisch mit, wann Sie verwendet werden sollte `DUMP RESULT;` in den vorherigen Schritten.

        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

##<a name="a-idsummaryasummary"></a><a id="summary"></a>Zusammenfassung

Wie Sie sehen können, kann der Befehl Schwein Sie interaktiv ausführen MapReduce Vorgänge mithilfe von Schwein Lateinisch sowie Ausführen Anweisungen in einer Batchdatei gespeichert.

##<a name="a-idnextstepsanext-steps"></a><a id="nextsteps"></a>Nächste Schritte

Allgemeine Informationen zu Schwein in HDInsight.

* [Schwein mit Hadoop auf HDInsight verwenden](hdinsight-use-pig.md)

Informationen zu anderen Methoden können Sie mit Hadoop auf HDInsight arbeiten.

* [Verwenden Sie die Struktur mit Hadoop auf HDInsight](hdinsight-use-hive.md)

* [Verwenden von MapReduce mit Hadoop auf HDInsight](hdinsight-use-mapreduce.md)

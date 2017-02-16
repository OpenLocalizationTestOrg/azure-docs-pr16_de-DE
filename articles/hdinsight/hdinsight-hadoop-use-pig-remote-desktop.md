<properties
   pageTitle="Verwenden von Hadoop Schwein mit Remotedesktop in HDInsight | Microsoft Azure"
   description="Erfahren Sie, wie Sie mithilfe des Befehls Schwein Schwein Lateinisch-Anweisungen eine Remotedesktop-Verbindung zu einem Windows-basierten Hadoop Cluster in HDInsight auszuführen."
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

#<a name="run-pig-jobs-from-a-remote-desktop-connection"></a>Schwein Aufträge aus eine Verbindung Remotedesktop ausführen

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

Dieses Dokument enthält eine exemplarische Vorgehensweise zur Verwendung des Befehls Schwein Schwein Lateinisch Anweisungen aus einer Remotedesktop-Verbindung zu einem Windows-basierten HDInsight Cluster ausführen. Schwein Lateinisch können Sie MapReduce Applications erstellen, mit der Beschreibung Datentransformationen, anstatt zuordnen und Funktionen zu verringern.

In diesem Dokument erfahren Sie, wie Sie

##<a name="a-idprereqaprerequisites"></a><a id="prereq"></a>Erforderliche Komponenten

Um die Schritte in diesem Artikel ausführen zu können, benötigen Sie Folgendes.

* Ein Windows-basiertes HDInsight (Hadoop auf HDInsight) cluster

* Ein Clientcomputer unter Windows 7, Windows 8 oder Windows-10

##<a name="a-idconnectaconnect-with-remote-desktop"></a><a id="connect"></a>Verbinden mit Remotedesktop

Damit anhand der Anweisungen in [Verbindung herstellen mit HDInsight Cluster mit RDP](hdinsight-administer-use-management-portal.md#rdp)herstellen, dann Remotedesktop für den Cluster HDInsight aktivieren.

##<a name="a-idpigause-the-pig-command"></a><a id="pig"></a>Verwenden Sie den Befehl Schwein

2. Nachdem Sie eine Remotedesktop Verbindung haben, starten Sie die **Befehlszeile Hadoop** mithilfe des Symbols auf dem Desktop.

2. Starten Sie den Befehl Schwein anhand der folgenden:

        %pig_home%\bin\pig

    Sie behandelt werden mit einem `grunt>` auffordern.

3. Geben Sie die folgende Anweisung aus:

        LOGS = LOAD 'wasbs:///example/data/sample.log';

    Dieser Befehl lädt den Inhalt der Datei sample.log in die Protokolle Datei an. Sie können den Inhalt der Datei anzeigen, indem Sie mit den folgenden Befehl aus:

        DUMP LOGS;

4. Transformieren der Daten durch Anwenden eines regulären Ausdrucks, um nur die Protokollierungsebene aus jedem Datensatz zu extrahieren:

        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;

    **Sichern** können Sie die Daten nach der Transformation angezeigt. In diesem Fall `DUMP LEVELS;`.

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
    <td>Ergebnis = Reihenfolge HÄUFIGKEITEN nach Anzahl Desc;</td><td>Bestellungen Log Ebenen nach Anzahl (absteigend) und speichert in Ergebnis</td>
    </tr>
    </table>

6. Sie können auch die Ergebnisse einer Transformation speichern, mithilfe der `STORE` Anweisung. Beispielsweise den folgenden Befehl speichert die `RESULT` in das Verzeichnis **/example/data/pigout** im Standardcontainer-Speicher für Ihren Cluster:

        STORE RESULT into 'wasbs:///example/data/pigout'

    > [AZURE.NOTE] Die Daten werden im angegebenen Verzeichnis in Dateien, die mit dem Namen **Webpart-00001...nnnnn**gespeichert. Wenn das Verzeichnis bereits vorhanden ist, erhalten Sie eine Fehlermeldung angezeigt.

7. Um die Aufforderung mühselige beenden möchten, geben Sie die folgende Anweisung ein.

        QUIT;

###<a name="pig-latin-batch-files"></a>Schwein Lateinisch Stapelverarbeitungsdateien

Den Befehl Schwein können Sie auch Schwein Lateinisch ausführen, die in einer Datei enthalten ist.

3. Nach dem Beenden der mühselige auffordern, öffnen Sie **Editor** , und erstellen Sie eine neue Datei namens **pigbatch.pig** im Verzeichnis **"% PIG_HOME"** .

4. Geben Sie oder fügen Sie die folgenden Zeilen in der Datei **pigbatch.pig** , und speichern Sie es dann:

        LOGS = LOAD 'wasbs:///example/data/sample.log';
        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
        FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
        GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
        FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
        RESULT = order FREQUENCIES by COUNT desc;
        DUMP RESULT;

5. Verwenden Sie die folgenden, um die **pigbatch.pig** -Datei, die mit dem Befehl Schwein auszuführen.

        pig %PIG_HOME%\pigbatch.pig

    Wenn die Stapelverarbeitung abgeschlossen ist, sollten Sie finden Sie unter der folgenden Ausgabe identisch mit, wann Sie verwendet werden sollte `DUMP RESULT;` in den vorherigen Schritten:

        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

##<a name="a-idsummaryasummary"></a><a id="summary"></a>Zusammenfassung

Wie Sie sehen können, kann der Befehl Schwein Sie interaktiv MapReduce Vorgänge ausführen, oder führen Sie Schwein Lateinisch Aufträge, die in einer Batchdatei gespeichert werden.

##<a name="a-idnextstepsanext-steps"></a><a id="nextsteps"></a>Nächste Schritte

Allgemeine Informationen zu Schwein in HDInsight:

* [Schwein mit Hadoop auf HDInsight verwenden](hdinsight-use-pig.md)

Weitere Informationen zu anderen Verfahren können Sie mit Hadoop auf HDInsight arbeiten:

* [Verwenden Sie die Struktur mit Hadoop auf HDInsight](hdinsight-use-hive.md)

* [Verwenden von MapReduce mit Hadoop auf HDInsight](hdinsight-use-mapreduce.md)

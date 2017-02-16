<properties
   pageTitle="Hadoop Struktur verwenden, klicken Sie auf die Abfrage-Konsole unter HDInsight | Microsoft Azure"
   description="Informationen Sie zum Verwenden der Web-basierten Abfrage-Verwaltungskonsole zum Struktur Abfragen für eine HDInsight Hadoop Cluster im Browser ausgeführt."
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
   ms.date="09/20/2016"
   ms.author="larryfr"/>

# <a name="run-hive-queries-using-the-query-console"></a>Führen Sie Struktur Abfragen mithilfe der Verwaltungskonsole Abfrage aus

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

In diesem Artikel erfahren Sie, wie die HDInsight Abfrage Konsole Struktur Abfragen für eine HDInsight Hadoop Cluster im Browser ausgeführt wird.

> [AZURE.IMPORTANT] Der Abfrage-Konsole HDInsight ist nur verfügbar, auf Windows basierende HDInsight Cluster. Wenn Sie einen Linux-basierten HDInsight Cluster verwenden, finden Sie unter [Ausführen Struktur Abfragen, die in der Ansicht Struktur](hdinsight-hadoop-use-hive-ambari-view.md).


##<a name="a-idprereqaprerequisites"></a><a id="prereq"></a>Erforderliche Komponenten

Um die Schritte in diesem Artikel ausführen zu können, benötigen Sie Folgendes.

* Ein Windows-basiertes HDInsight Hadoop cluster

* Eine moderne Webbrowser

##<a name="a-idruna-run-hive-queries-using-the-query-console"></a><a id="run"></a>Führen Sie Struktur Abfragen mithilfe der Verwaltungskonsole Abfrage aus

1. Öffnen Sie einen Webbrowser, und navigieren Sie zu __https://CLUSTERNAME.azurehdinsight.net__, darin __CLUSTERNAME__ auf den Namen der Cluster HDInsight. Wenn Sie dazu aufgefordert werden, geben Sie den Benutzernamen und das Kennwort, die Sie verwendet werden, wenn Sie den Cluster erstellt haben.


2. Wählen Sie über die Links am oberen Rand der Seite **Struktur Editor**ein. Hierdurch werden angezeigt, ein Formulars, das verwendet werden kann, die Anweisungen HiveQL eingeben, die im Cluster HDInsight ausgeführt werden soll.

    ![die Struktur-editor](./media/hdinsight-hadoop-use-hive-query-console/queryconsole.png)

    Ersetzen Sie den Text `Select * from hivesampletable` mit folgenden Aussagen HiveQL:

        set hive.execution.engine=tez;
        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    Diese Anweisungen führen Sie die folgenden Aktionen aus:

    * **Tabelle löschen**: Löscht die Tabelle und der Datendatei, wenn die Tabelle bereits vorhanden ist.
    * **Externe Tabelle erstellen**: erstellt eine neue 'externe' Tabelle in die Struktur. Externe Tabellen speichern nur die Definition der Tabelle, in Struktur. die Daten an der ursprünglichen Stelle bleibt.

    > [AZURE.NOTE] Externe Tabellen sollten verwendet werden, wenn Sie erwarten, dass die zugrunde liegenden Daten von einer externen Quelle (z. B. eine automatisierte Daten Uploadprozess) oder von einem anderen MapReduce Vorgang aktualisiert werden, aber immer Struktur Abfragen die neuesten Daten verwendet werden soll.
    >
    > Enthält eine externe Tabelle ablegen **nicht** Löschen der Daten, nur die Definition der Tabelle.

    * **ZEILENFORMAT**: erfahren Struktur wie die Daten formatiert ist. In diesem Fall sind die Felder in jedem Protokoll durch ein Leerzeichen getrennt.
    * **AS Textdatei-Speicherort gespeichert**: erfahren Struktur, wo die Daten sind (das Verzeichnis/Beispieldaten) gespeichert und, die sie als Text gespeichert ist
    * **Wählen Sie aus**: Wählen Sie die Anzahl aller Zeilen Spalte **t4** enthalten, in dem den Wert **[Fehler]**. Dies sollte einen Wert von **3** zurück, da es gibt drei Zeilen, die diesen Wert enthalten.
    * **INPUT__FILE__NAME wie '%.log'** - teilt Struktur, die wir nur Daten aus, die in Dateien zurückgeben sollte. Log. Hiermit legen Sie das Feld Suchen die sample.log-Datei, die die Daten enthält, und verhindert, dass Sie zurückgeben von Daten aus anderen Beispiel Datendateien, die nicht das Schema entsprechen, die, das wir definiert.

2. Klicken Sie auf **Absenden**. Ausführliche Informationen zu den Auftrag sollte die **Sitzung Job** am unteren Rand der Seite angezeigt werden.

3. Wenn das Feld **Status** auf **abgeschlossen**geändert wird, wählen Sie **Anzeigen von Details** für das Projekt ein. Klicken Sie auf der Detailseite enthält die **Position Ausgabe** `[ERROR]   3`. Können die Schaltfläche **Download** unter diesem Feld zum Herunterladen einer Datei, die die Ausgabe des Projekts enthält.


##<a name="a-idsummaryasummary"></a><a id="summary"></a>Zusammenfassung

Wie Sie sehen können, stellt der Abfrage-Konsole eine einfache Möglichkeit, Struktur Abfragen in einem HDInsight Cluster ausführen, den Status überwachen und Abrufen der Ausgabe aus.

Weitere Informationen zur Verwendung von Struktur Abfrage Console Struktur Auftrag ausführen, wählen Sie am oberen Rand der Abfrage Konsole **Erste Schritte** aus, und verwenden Sie dann die Beispielen, die zur Verfügung gestellt werden. Jede Probe führt durch die Verwendung der Struktur zum Analysieren von Daten, einschließlich erläuterungen zu den HiveQL Anweisungen in der Stichprobe verwendet.

##<a name="a-idnextstepsanext-steps"></a><a id="nextsteps"></a>Nächste Schritte

Allgemeine Informationen zu Struktur in HDInsight:

* [Verwenden Sie die Struktur mit Hadoop auf HDInsight](hdinsight-use-hive.md)

Weitere Informationen zu anderen Verfahren können Sie mit Hadoop auf HDInsight arbeiten:

* [Schwein mit Hadoop auf HDInsight verwenden](hdinsight-use-pig.md)

* [Verwenden von MapReduce mit Hadoop auf HDInsight](hdinsight-use-mapreduce.md)

Wenn Sie mit der Struktur Tez verwenden, finden Sie die folgenden Dokumente für das Debuggen von Informationen:

* [Mithilfe der Tez Benutzeroberfläche auf Windows basierende HDInsight](hdinsight-debug-tez-ui.md)

* [Verwenden der Ansicht Ambari Tez auf Linux-basierten HDInsight](hdinsight-debug-ambari-tez-view.md)

[1]: ../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md



[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[Powershell-install-configure]: powershell-install-configure.md
[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx


[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png

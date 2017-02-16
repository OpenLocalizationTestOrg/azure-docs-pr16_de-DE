<properties
   pageTitle="Verwenden von Hadoop Struktur und Remotedesktop in HDInsight | Microsoft Azure"
   description="Erfahren Sie, wie Sie mithilfe von Remotedesktop mit Hadoop Cluster in HDInsight verbinden, und führen Sie dann die Struktur Abfragen über die Benutzeroberfläche für die Struktur Line."
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
   ms.date="09/06/2016"
   ms.author="larryfr"/>

# <a name="use-hive-with-hadoop-on-hdinsight-with-remote-desktop"></a>Verwenden Sie die Struktur mit Hadoop auf HDInsight mithilfe von Remotedesktop

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

In diesem Artikel werden Sie erfahren, wie Sie eine Verbindung mit einem Cluster HDInsight mithilfe des Remote Desktop, und führen Sie dann die Struktur Abfragen mithilfe der Struktur Line Interface (CLI).

> [AZURE.NOTE] Dieses Dokument bietet keine detaillierte Beschreibung der Möglichkeiten, die der HiveQL Aussagen, die in den Beispielen verwendet werden. Informationen zu den HiveQL, die in diesem Beispiel verwendet wird, finden Sie unter [Verwendung mit Hadoop auf HDInsight Struktur](hdinsight-use-hive.md).

##<a name="a-idprereqaprerequisites"></a><a id="prereq"></a>Erforderliche Komponenten

Um die Schritte in diesem Artikel ausführen zu können, benötigen Sie Folgendes:

* Ein Windows-basiertes HDInsight (Hadoop auf HDInsight) cluster

* Ein Clientcomputer unter Windows 10, Fenster 8 oder Windows 7

##<a name="a-idconnectaconnect-with-remote-desktop"></a><a id="connect"></a>Verbinden mit Remotedesktop

Damit anhand der Anweisungen in [Verbindung herstellen mit HDInsight Cluster mit RDP](hdinsight-administer-use-management-portal.md#rdp)herstellen, dann Remotedesktop für den Cluster HDInsight aktivieren.

##<a name="a-idhiveause-the-hive-command"></a><a id="hive"></a>Verwenden Sie den Befehl Struktur

Wenn Sie auf dem Desktop für den HDInsight Cluster verbunden haben, gehen Sie folgendermaßen vor, für die Arbeit mit Struktur:

1. Starten Sie die **Befehlszeile Hadoop**aus den Desktop HDInsight.

2. Geben Sie den folgenden Befehl aus, um die Struktur CLI zu starten:

        %hive_home%\bin\hive

    Wenn die CLI gestartet hat, werden die Struktur CLI-Aufforderung angezeigt: `hive>`.

3. Geben Sie die CLI mithilfe der folgenden Aussagen zum Erstellen einer neuen Tabelle mit dem Namen **log4jLogs** mit Beispieldaten aus:

        set hive.execution.engine=tez;
        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    Diese Anweisungen führen Sie die folgenden Aktionen aus:

    * **Tabelle löschen**: Löscht die Tabelle und der Datendatei, wenn die Tabelle bereits vorhanden ist.

    * **Externe Tabelle erstellen**: erstellt eine neue 'externe' Tabelle in die Struktur. Externe Tabellen werden nur die Definition der Tabelle in die Struktur (die Daten an der ursprünglichen Stelle bleibt) gespeichert.

        > [AZURE.NOTE] Externe Tabellen sollten verwendet werden, wenn Sie erwarten, dass die zugrunde liegenden Daten von einer externen Quelle (z. B. eine automatisierte Daten Uploadprozess) oder von einem anderen MapReduce Vorgang aktualisiert werden, aber immer Struktur Abfragen die neuesten Daten verwendet werden soll.
        >
        > Enthält eine externe Tabelle ablegen **nicht** Löschen der Daten, nur die Definition der Tabelle.

    * **ZEILENFORMAT**: erfahren Struktur wie die Daten formatiert ist. In diesem Fall sind die Felder in jedem Protokoll durch ein Leerzeichen getrennt.

    * **AS Textdatei-Speicherort gespeichert**: erfahren Struktur, wo die Daten sind (das Verzeichnis/Beispieldaten) gespeichert und, die sie als Text gespeichert ist.

    * **Wählen Sie aus**: Anzahl aller Zeilen, in der Spalte **t4** den Wert **[Fehler enthält]**markiert. Dies sollte einen Wert von **3** zurück, da es gibt drei Zeilen, die diesen Wert enthalten.

    * **INPUT__FILE__NAME wie '%.log'** - teilt Struktur, die wir nur Daten aus, die in Dateien zurückgeben sollte. Log. Hiermit legen Sie das Feld Suchen die sample.log-Datei, die die Daten enthält, und verhindert, dass Sie zurückgeben von Daten aus anderen Beispiel Datendateien, die nicht das Schema entsprechen, die, das wir definiert.


4. Verwenden Sie die folgenden Anweisungen zum Erstellen einer neuen 'internen' Tabelle mit dem Namen **Fehlerprotokolle von.**:

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    Diese Anweisungen führen Sie die folgenden Aktionen aus:

    * **Erstellen der Tabelle IF NOT EXISTS**: erstellt eine Tabelle aus, wenn es nicht bereits vorhanden ist. Da das **EXTERNEN** Schlüsselwort nicht verwendet wird, ist dies eine interne Tabelle, die im Stock Datawarehouse gespeichert ist und vollständig von Struktur verwaltet wird.

        > [AZURE.NOTE] Im Gegensatz zu **EXTERNEN** Tabellen löscht eine interne Tabelle ablegen auch die zugrunde liegenden Daten.

    * **Gespeicherte AS ORC**: speichert die Daten in einspaltigen optimierten Zeile (ORC)-Format. Dies ist ein hochgradig optimierte und effiziente Format zum Speichern von Daten Struktur.

    * ÜBERSCHREIBEN der **Einfügen... Wählen Sie**: Fügt wählt Zeilen aus der Tabelle **log4jLogs** , die enthalten **[Fehler]**, klicken Sie dann die Daten in der Tabelle **Fehlerprotokolle von.** ein.

    Um zu überprüfen, dass nur die Zeilen, die enthalten **[Fehler]** in der Spalte t4 zur Tabelle **Fehlerprotokolle von.** gespeichert wurden, verwenden Sie die folgende Anweisung **Fehlerprotokolle von.**alle Zeilen zurück:

        SELECT * from errorLogs;

    Drei Zeilen mit Daten zurückgegeben werden soll alle enthaltenden **[Fehler]** in der Spalte t4.

##<a name="a-idsummaryasummary"></a><a id="summary"></a>Zusammenfassung

Wie Sie sehen können, die der Befehl Struktur bietet eine einfache Möglichkeit, interaktiv Struktur Abfragen in einem HDInsight Cluster ausführen, den Status überwachen und Abrufen der Ausgabe.

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





[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md


[Powershell-install-configure]: ../powershell-install-configure.md
[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx


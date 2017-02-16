<properties
   pageTitle="Abfrage mit Hadoop-Tools für Visual Studio Struktur | Microsoft Azure"
   description="Informationen Sie zur Verwendung von Struktur mit Hadoop in HDInsight mithilfe von Tools für Visual Studio Hadoop."
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

#<a name="run-hive-queries-using-the-hdinsight-tools-for-visual-studio"></a>Führen Sie die Struktur Abfragen mithilfe der HDInsight-Tools für Visual Studio

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

In diesem Artikel erfahren Sie, wie Struktur Abfragen zu einem HDInsight Cluster mit den HDInsight Tools für Visual Studio übermittelt.

> [AZURE.NOTE] Dieses Dokument bietet keine detaillierte Beschreibung der Möglichkeiten, der in den Beispielen verwendete HiveQL Anweisungen. Informationen zu den HiveQL, die in diesem Beispiel verwendet wird, finden Sie unter [Verwendung mit Hadoop auf HDInsight Struktur](hdinsight-use-hive.md).

##<a name="a-idprereqaprerequisites"></a><a id="prereq"></a>Erforderliche Komponenten

Um die Schritte in diesem Artikel ausführen zu können, benötigen Sie Folgendes.

* Ein Azure HDInsight (Hadoop auf HDInsight) Cluster (Linux oder Windows-basiertem)

* Visual Studio (eine der folgenden Versionen):

    Visual Studio 2013 Community/Professional/Premium/Ultimate mit [4 aktualisieren](https://www.microsoft.com/download/details.aspx?id=44921)

    Visual Studio 2015 (Community/Enterprise)

- HDInsight Tools für Visual Studio. Informationen zum Installieren und konfigurieren die Tools finden Sie unter [Erste Schritte mit Visual Studio Hadoop-Tools für HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md) .

##<a name="a-idruna-run-hive-queries-using-the-hdinsight-tools-for-visual-studio"></a><a id="run"></a>Führen Sie die Struktur Abfragen mithilfe der HDInsight-Tools für Visual Studio

1. Öffnen Sie **Visual Studio** , und wählen Sie **neu** > **Project** > **HDInsight** > **Struktur Anwendung**. Geben Sie einen Namen für dieses Projekt ein.

2. Öffnen Sie die **Script.hql** -Datei, die mit diesem Projekt erstellt wird, und fügen Sie in der folgenden Aussagen HiveQL:

        set hive.execution.engine=tez;
        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    Diese Anweisungen führen Sie die folgenden Aktionen aus:

    * **Tabelle löschen**: Löscht die Tabelle und der Datendatei, wenn die Tabelle bereits vorhanden ist.
    * **Externe Tabelle erstellen**: erstellt eine neue 'externe' Tabelle in die Struktur. Externe Tabellen speichern nur die Definition der Tabelle in die Struktur (die Daten an der ursprünglichen Stelle bleibt).

        > [AZURE.NOTE] Externe Tabellen sollten verwendet werden, wenn Sie erwarten, dass die zugrunde liegenden Daten von einer externen Quelle (z. B. eine automatisierte Daten Uploadprozess) oder von einem anderen MapReduce Vorgang aktualisiert werden, aber immer Struktur Abfragen die neuesten Daten verwendet werden soll.
        >
        > Enthält eine externe Tabelle ablegen **nicht** Löschen der Daten, nur die Definition der Tabelle.

    * **ZEILENFORMAT**: erfahren Struktur wie die Daten formatiert ist. In diesem Fall sind die Felder in jedem Protokoll durch ein Leerzeichen getrennt.
    * **AS Textdatei-Speicherort gespeichert**: erfahren Struktur, wo die Daten sind (das Verzeichnis/Beispieldaten) gespeichert und, die sie als Text gespeichert ist.
    * **Wählen Sie aus**: Wählen Sie die Anzahl aller Zeilen Spalte **t4** enthalten, in dem den Wert **[Fehler]**. Dies sollte einen Wert von **3** zurück, da es gibt drei Zeilen, die diesen Wert enthalten.
    * **INPUT__FILE__NAME wie '%.log'** - teilt Struktur, die wir nur Daten aus, die in Dateien zurückgeben sollte. Log. Hiermit legen Sie das Feld Suchen die sample.log-Datei, die die Daten enthält, und verhindert, dass Sie zurückgeben von Daten aus anderen Beispiel Datendateien, die nicht das Schema entsprechen, die, das wir definiert.

3. Wählen Sie den **HDInsight Cluster** , die Sie für diese Abfrage verwenden möchten, und wählen Sie dann auf **senden, um WebHCat** , die Anweisungen als Struktur Auftrag WebHCat verwenden, aus der Symbolleiste. Sie können auch den Auftrag mithilfe der Schaltfläche __Ausführen über HiveServer2__ ist HiveServer2 auf Ihrer Clusterversion zur Verfügung senden. Die **Struktur Auftrag Zusammenfassung** wird eingeblendet und zeigt Informationen zu den Auftrag ausgeführt. Verwenden Sie den Link **Aktualisieren** , die Informationen zur Tätigkeit aktualisieren, bis der **Status des** **abgeschlossen**wird.

4. Verwenden Sie den **Auftragsausgabe** Link, um die Ausgabe dieses Auftrags anzuzeigen. Sie sollten anzeigen `[ERROR] 3`, welche die SELECT-Anweisung zurückgegebene Wert ist.

5. Sie können auch die Struktur Abfragen ausführen, ohne ein Projekt zu erstellen. Mit dem **Server-Explorer**, erweitern Sie **Azure** > **HDInsight**, mit der rechten Maustaste in Ihren HDInsight Servers, und wählen Sie **eine Struktur Abfrage schreiben**.

6. Im **temp.hql** -Dokument, das angezeigt wird, fügen Sie die folgenden Aussagen HiveQL hinzu:

        set hive.execution.engine=tez;
        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    Diese Anweisungen führen Sie die folgenden Aktionen aus:

    * **Erstellen der Tabelle IF NOT EXISTS**: erstellt eine Tabelle aus, wenn es nicht bereits vorhanden ist. Da das **EXTERNEN** Schlüsselwort nicht verwendet wird, ist dies eine interne Tabelle, die im Stock Datawarehouse gespeichert ist und vollständig von Struktur verwaltet wird.

        > [AZURE.NOTE] Im Gegensatz zu **EXTERNEN** Tabellen löscht eine interne Tabelle ablegen auch die zugrunde liegenden Daten.

    * **Gespeicherte AS ORC**: speichert die Daten in einspaltigen optimierten Zeile (ORC)-Format. Dies ist ein hochgradig optimierte und effiziente Format zum Speichern von Daten Struktur.
    * ÜBERSCHREIBEN der **Einfügen... Wählen Sie**: Fügt wählt Zeilen aus der Tabelle **log4jLogs** , die enthalten **[Fehler]**, klicken Sie dann die Daten in der Tabelle **Fehlerprotokolle von.** ein.

7. Wählen Sie aus der Symbolleiste **Absenden** , um die Aufgabe durchzuführen. Verwenden Sie den **Projektstatus** bestimmen, um der Auftrag erfolgreich abgeschlossen wurde.

8. Um zu überprüfen, dass das Projekt abgeschlossen ist, und eine neue Tabelle erstellt, verwenden Sie **Server-Explorer** , und blenden Sie **Azure** > **HDInsight** > HDInsight Cluster > **Datenbanken Struktur** > und **Standardwert**. Die **Fehlerprotokolle von.** Tabelle und der Tabelle **log4jLogs** auftreten.

##<a name="a-idsummaryasummary"></a><a id="summary"></a>Zusammenfassung

Wie Sie sehen können, die die HDInsight-Tools für Visual Studio bieten eine einfache Möglichkeit, ein HDInsight Cluster Struktur Abfragen ausgeführt, den Status überwachen und Abrufen der Ausgabe.

##<a name="a-idnextstepsanext-steps"></a><a id="nextsteps"></a>Nächste Schritte

Allgemeine Informationen zu Struktur in HDInsight:

* [Verwenden Sie die Struktur mit Hadoop auf HDInsight](hdinsight-use-hive.md)

Weitere Informationen zu anderen Verfahren können Sie mit Hadoop auf HDInsight arbeiten:

* [Schwein mit Hadoop auf HDInsight verwenden](hdinsight-use-pig.md)

* [Verwenden von MapReduce mit Hadoop auf HDInsight](hdinsight-use-mapreduce.md)

Weitere Informationen zu den HDInsight Tools für Visual Studio:

* [Erste Schritte mit HDInsight Tools für Visual Studio](../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md)


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

[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx

[image-hdi-hive-powershell]: ./media/hdinsight-use-hive/HDI.HIVE.PowerShell.png
[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png
[image-hdi-hive-architecture]: ./media/hdinsight-use-hive/HDI.Hive.Architecture.png

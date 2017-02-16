<properties
   pageTitle="Verwenden Sie die Struktur-Verwaltungsshell in HDInsight (Hadoop) | Microsoft Azure"
   description="Erfahren Sie, wie die Struktur mit einem HDInsight Linux-basierten Cluster Verwaltungsshell. Sie werden erfahren Sie, wie die Verbindung zum Cluster HDInsight mithilfe von SSh und dann verwenden Sie die Verwaltungsshell Struktur, interaktiv Abfragen auszuführen."
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
   ms.date="10/04/2016"
   ms.author="larryfr"/>

# <a name="use-hive-with-hadoop-in-hdinsight-with-ssh"></a>Verwenden Sie die Struktur mit Hadoop in HDInsight mit SSH

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

In diesem Artikel erfahren Sie, wie Sie mithilfe von Secure Shell (SSH) eine Verbindung mit einer Hadoop auf Azure HDInsight Cluster und dann interaktiv übermitteln Struktur Abfragen mithilfe der Struktur line Interface (CLI).

> [AZURE.IMPORTANT] Während der Befehl Struktur HDInsight Linux-basierten Cluster verfügbar ist, sollten Sie Beeline in Betracht ziehen. Beeline ist eine neuere für das Arbeiten mit der Struktur und in Ihren Cluster HDInsight enthalten ist. Weitere Informationen zu diesen Befehl verwenden finden Sie unter [Verwendung mit Hadoop in HDInsight mit Beeline Struktur](hdinsight-hadoop-use-hive-beeline.md).

##<a name="a-idprereqaprerequisites"></a><a id="prereq"></a>Erforderliche Komponenten

Um die Schritte in diesem Artikel ausführen zu können, benötigen Sie Folgendes:

* Eine Linux-basierten Hadoop auf HDInsight Cluster.

* SSH-Client. Linux, Unix und Mac OS sollte mit einem SSH-Client nützlich sein. Windows-Benutzer müssen einen Client, z. B. [kitten](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)herunterladen.

##<a name="a-idsshaconnect-with-ssh"></a><a id="ssh"></a>Verbinden mit SSH

Schließen Sie an den vollqualifizierten Domänennamen (FULLY) von Ihren Cluster HDInsight mithilfe des Befehls SSH. Der vollqualifizierten Domänennamen werden auf dem Namen den Cluster dann zugewiesen **. azurehdinsight.net**. Beispielsweise würde Folgendes zu einem Cluster mit dem Namen **Myhdinsight**verbinden:

    ssh admin@myhdinsight-ssh.azurehdinsight.net

**Wenn Sie einen Zertifikatschlüssel für die Authentifizierung SSH bereitgestellt** , wenn Sie den HDInsight Cluster erstellt haben, müssen Sie den Speicherort der private Schlüssel auf Ihrem Clientsystem anzugeben:

    ssh -i ~/mykey.key admin@myhdinsight-ssh.azurehdinsight.net

**Wenn Sie ein Kennwort für die Authentifizierung SSH angegeben** , wenn Sie den HDInsight Cluster erstellt haben, müssen Sie das Kennwort ein bereitstellen.

Weitere Informationen zum Verwenden von SSH mit HDInsight finden Sie unter [Verwenden SSH mit Linux-basierten Hadoop auf HDInsight Linux, OS X und Unix](hdinsight-hadoop-linux-use-ssh-unix.md).

###<a name="putty-windows-based-clients"></a>Kitten (Windows-basierten Clients)

Windows bietet keinen integrierten SSH-Client. Es empfiehlt sich, mit **kitten**, die von [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)heruntergeladen werden kann.

Weitere Informationen zur Verwendung von kitten finden Sie unter [Verwenden SSH mit Linux-basierten Hadoop auf HDInsight von Windows ](hdinsight-hadoop-linux-use-ssh-windows.md).

##<a name="a-idhiveause-the-hive-command"></a><a id="hive"></a>Verwenden Sie den Befehl Struktur

2. Nachdem die Verbindung hergestellt wurde, starten Sie die Struktur CLI mit dem folgenden Befehl:

        hive

3. Geben Sie die CLI mithilfe der folgenden Aussagen zum Erstellen einer neuen Tabelle namens **log4jLogs** mit den Beispieldaten aus:

        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    Diese Anweisungen führen Sie die folgenden Aktionen aus:

    * **DROP TABLE** - löscht die Tabelle und der Datendatei für den Fall, dass die Tabelle bereits vorhanden ist.
    * **Externe Tabelle erstellen** - erstellt eine neue 'externe' Tabelle in die Struktur. Externe Tabellen speichern nur die Definition der Tabelle in die Struktur. Die Daten an der ursprünglichen Stelle bleibt.
    * **ZEILENFORMAT** : erfahren Struktur wie die Daten formatiert ist. In diesem Fall sind die Felder in jedem Protokoll durch ein Leerzeichen getrennt.
    * **AS Textdatei-Speicherort gespeichert** – erfahren Struktur Wo finde ich die Daten gespeichert (das Verzeichnis/Beispieldaten), und die It wird als Text gespeichert.
    * **Wählen Sie aus** – wählt Anzahl aller Zeilen, in denen Spalte **t4** den Wert **[Fehler]**enthält. Es gibt drei Zeilen, die diesen Wert enthalten sollte dadurch der Wert **3** zurückgegeben.
    * **INPUT__FILE__NAME wie '%.log'** - teilt Struktur, die wir nur Daten aus, die in Dateien zurückgeben sollte. Log. Hiermit legen Sie das Feld Suchen die sample.log-Datei, die die Daten enthält, und verhindert, dass Sie zurückgeben von Daten aus anderen Beispiel Datendateien, die nicht das Schema entsprechen, die, das wir definiert.

    > [AZURE.NOTE] Externe Tabellen sollte verwendet werden, wenn Sie erwarten, die zugrunde liegenden Daten mit einer externen Datenquelle, beispielsweise eine automatisierte Daten Uploadprozess oder von einem anderen Vorgang MapReduce, aktualisiert werden, aber immer Struktur Abfragen dass, um die neuesten Daten verwenden möchten.
    >
    > Enthält eine externe Tabelle ablegen **nicht** Löschen der Daten, nur die Definition der Tabelle.

4. Verwenden Sie die folgenden Anweisungen zum Erstellen einer neuen 'internen' Tabelle mit dem Namen **Fehlerprotokolle von.**:

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    Diese Anweisungen führen Sie die folgenden Aktionen aus:

    * **Erstellen der Tabelle IF NOT EXISTS** - erstellt eine Tabelle, wenn es nicht bereits vorhanden ist. Da das **externe** Schlüsselwort nicht verwendet wird, ist dies eine interne Tabelle, die im Stock Datawarehouse gespeichert ist und vollständig von Struktur verwaltet wird.
    * **Gespeicherte AS ORC** - speichert die Daten im Format optimiert Zeile einspaltigen (ORC). Dies ist ein hochgradig optimierte und effiziente Format zum Speichern von Daten Struktur.
    * ÜBERSCHREIBEN der **Einfügen... Wählen Sie** – wählt Zeilen aus der Tabelle " **log4jLogs** ", die enthalten **[Fehler]**aus, und klicken Sie dann fügt die Daten in der Tabelle **Fehlerprotokolle von.** .

    Um zu überprüfen, dass nur die Zeilen, die in der Spalte t4 enthält, **[Fehler]** der Tabelle **Fehlerprotokolle von.** gespeichert wurden, verwenden Sie die folgende Anweisung alle Zeilen von **Fehlerprotokolle von.**zurückgegeben:

        SELECT * from errorLogs;

    Drei Zeilen mit Daten zurückgegeben werden soll alle enthaltenden **[Fehler]** in der Spalte t4.

    > [AZURE.NOTE] Im Gegensatz zu externen Tabellen werden die zugrunde liegenden Daten ablegen eine interne Tabelle gelöscht werden.

##<a name="a-idsummaryasummary"></a><a id="summary"></a>Zusammenfassung

Wie Sie sehen können, bietet es sich bei der Struktur Befehl um eine einfache Möglichkeit, interaktiv Struktur Abfragen in einem HDInsight Cluster ausführen, den Status überwachen und Abrufen der Ausgabe.

##<a name="a-idnextstepsanext-steps"></a><a id="nextsteps"></a>Nächste Schritte

Allgemeine Informationen zur Struktur in HDInsight:

* [Verwenden Sie die Struktur mit Hadoop auf HDInsight](hdinsight-use-hive.md)

Informationen zu anderen Methoden können Sie mit Hadoop auf HDInsight arbeiten:

* [Schwein mit Hadoop auf HDInsight verwenden](hdinsight-use-pig.md)

* [Verwenden von MapReduce mit Hadoop auf HDInsight](hdinsight-use-mapreduce.md)

Wenn Sie mit der Struktur Tez verwenden, finden Sie die folgenden Dokumente für das Debuggen von Informationen:

* [Mithilfe der Tez Benutzeroberfläche auf Windows basierende HDInsight](hdinsight-debug-tez-ui.md)

* [Verwenden der Ansicht Ambari Tez auf Linux-basierten HDInsight](hdinsight-debug-ambari-tez-view.md)

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

[putty]: http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md



[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx


[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png


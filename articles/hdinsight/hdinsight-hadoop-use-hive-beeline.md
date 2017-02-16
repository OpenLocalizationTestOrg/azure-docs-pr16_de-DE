<properties
   pageTitle="Verwenden Sie für die Arbeit mit Struktur auf HDInsight (Hadoop) Beeline | Microsoft Azure"
   description="Informationen Sie zum Verwenden von SSH zum Verbinden mit einem Cluster Hadoop in HDInsight, und senden dann interaktiv Struktur Abfragen mithilfe von Beeline. Beeline ist ein Programm für die Arbeit mit HiveServer2 über JDBC."
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
   ms.date="10/10/2016"
   ms.author="larryfr"/>

#<a name="use-hive-with-hadoop-in-hdinsight-with-beeline"></a>Verwenden Sie die Struktur mit Hadoop in HDInsight mit Beeline

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

In diesem Artikel erfahren Sie, wie Secure Shell (SSH) zum Verbinden mit einem HDInsight Linux-basierten Cluster, und übermitteln dann interaktiv Struktur Abfragen mithilfe des [Beeline](https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Clients#HiveServer2Clients-Beeline–NewCommandLineShell) Befehlszeile Tools verwendet werden.

> [AZURE.NOTE] Beeline verwendet JDBC zum Herstellen von Struktur. Weitere Informationen zur Verwendung von JDBC mit Struktur finden Sie unter [Verbinden Struktur auf Azure HDInsight Struktur JDBC-Treiber verwenden](hdinsight-connect-hive-jdbc-driver.md).

##<a name="a-idprereqaprerequisites"></a><a id="prereq"></a>Erforderliche Komponenten

Um die Schritte in diesem Artikel ausführen zu können, benötigen Sie Folgendes:

* Eine Linux-basierten Hadoop auf HDInsight Cluster.

* SSH-Client. Linux, Unix und Mac OS sollte mit einem SSH-Client nützlich sein. Windows-Benutzer müssen einen Client, z. B. [kitten](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)herunterladen.

##<a name="a-idsshaconnect-with-ssh"></a><a id="ssh"></a>Verbinden mit SSH

Schließen Sie an den vollqualifizierten Domänennamen (FULLY) von Ihren Cluster HDInsight mithilfe des Befehls SSH. Der vollqualifizierten Domänennamen werden auf dem Namen den Cluster dann zugewiesen **. azurehdinsight.net**. Beispielsweise würde Folgendes zu einem Cluster mit dem Namen **Myhdinsight**verbinden:

    ssh admin@myhdinsight-ssh.azurehdinsight.net

**Wenn Sie einen Zertifikatschlüssel für die Authentifizierung SSH bereitgestellt** , wenn Sie den HDInsight Cluster erstellt haben, müssen Sie den Speicherort der private Schlüssel auf Ihrem Clientsystem anzugeben:

    ssh admin@myhdinsight-ssh.azurehdinsight.net -i ~/mykey.key

**Wenn Sie ein Kennwort für die Authentifizierung SSH angegeben** , wenn Sie den HDInsight Cluster erstellt haben, müssen Sie das Kennwort ein bereitstellen.

Weitere Informationen zum Verwenden von SSH mit HDInsight finden Sie unter [Verwenden SSH mit Linux-basierten Hadoop auf HDInsight Linux, OS X und Unix](hdinsight-hadoop-linux-use-ssh-unix.md).

###<a name="putty-windows-based-clients"></a>Kitten (Windows-basierten Clients)

Windows bietet keinen integrierten SSH-Client. Es empfiehlt sich, mit **kitten**, die von [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)heruntergeladen werden kann.

Weitere Informationen zur Verwendung von kitten finden Sie unter [Verwenden SSH mit Linux-basierten Hadoop auf HDInsight von Windows ](hdinsight-hadoop-linux-use-ssh-windows.md).

##<a name="a-idbeelineause-the-beeline-command"></a><a id="beeline"></a>Verwenden Sie den Befehl Beeline

1. Nachdem die Verbindung hergestellt wurde, anhand der folgenden um Beeline zu starten:

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin

    Dies wird starten Sie den Beeline Client, und Verbinden mit JDBC-Url. Hier `localhost` wird verwendet, da HiveServer2 ausgeführt, klicken Sie auf beide am Knoten im Cluster wird, und wir sind Beeline direkt auf die primäre Headnode ausgeführt.
    
    Sobald der Befehl abgeschlossen ist, werden bei einem `jdbc:hive2://localhost:10001/>` auffordern.

3. Beeline Befehle in der Regel beginnen mit einer `!` Zeichen, beispielsweise `!help` wird die Hilfe angezeigt. Jedoch die `!` häufig weggelassen werden können. Beispielsweise `help` auch funktionieren.

    Wenn Sie Hilfe anzeigen, sehen Sie `!sql`, der auszuführende HiveQL Anweisungen verwendet wird. HiveQL wird jedoch so häufig verwendet, können Sie die obige weglassen `!sql`. Die beiden folgenden Aussagen haben genau die gleichen Ergebnisse; Anzeigen von Tabellen bis Struktur derzeit verfügbar:
    
        !sql show tables;
        show tables;
    
    Klicken Sie auf einen neuen Cluster, sollten Sie nur eine Tabelle aufgelistet: __Hivesampletable__.

4. Verwenden Sie die folgenden, um das Schema für die Hivesampletable anzuzeigen:

        describe hivesampletable;
        
    Dadurch wird die folgende Informationen zurückgegeben:
    
        +-----------------------+------------+----------+--+
        |       col_name        | data_type  | comment  |
        +-----------------------+------------+----------+--+
        | clientid              | string     |          |
        | querytime             | string     |          |
        | market                | string     |          |
        | deviceplatform        | string     |          |
        | devicemake            | string     |          |
        | devicemodel           | string     |          |
        | state                 | string     |          |
        | country               | string     |          |
        | querydwelltime        | double     |          |
        | sessionid             | bigint     |          |
        | sessionpagevieworder  | bigint     |          |
        +-----------------------+------------+----------+--+

    Die Spalten in der Tabelle angezeigt. Während wir einige Abfragen in diesen Daten ausführen können, lassen Sie uns stattdessen Erstellen einer ganz neuen Tabelle um veranschaulichen, wie Daten in die Struktur laden und Anwenden einer Schema.
    
5. Geben Sie die folgenden Aussagen zum Erstellen einer neuen Tabelle namens **log4jLogs** mit Beispieldaten, die mit dem HDInsight Cluster bereitgestellt:

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
    * **INPUT__FILE__NAME wie '%.log'** - teilt Struktur, die wir nur Daten aus, die in Dateien zurückgeben sollte. Log. Normalerweise, nur müssen Daten mit dem Schema in denselben Ordner bei der Abfrage mit Struktur, jedoch mit anderen Datenformaten dieses Beispiel Protokolldatei gespeichert ist.

    > [AZURE.NOTE] Externe Tabellen sollte verwendet werden, wenn Sie erwarten, die zugrunde liegenden Daten mit einer externen Datenquelle, beispielsweise eine automatisierte Daten Uploadprozess oder von einem anderen Vorgang MapReduce, aktualisiert werden, aber immer Struktur Abfragen dass, um die neuesten Daten verwenden möchten.
    >
    > Enthält eine externe Tabelle ablegen **nicht** Löschen der Daten, nur die Definition der Tabelle.
    
    Die Ausgabe dieses Befehls sollte ähnlich wie der folgende aussehen:
    
        INFO  : Tez session hasn't been created yet. Opening session
        INFO  :
        
        INFO  : Status: Running (Executing on YARN cluster with App id application_1443698635933_0001)
        
        INFO  : Map 1: -/-      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0(+1)/1  Reducer 2: 0/1
        INFO  : Map 1: 0(+1)/1  Reducer 2: 0/1
        INFO  : Map 1: 1/1      Reducer 2: 0/1
        INFO  : Map 1: 1/1      Reducer 2: 0(+1)/1
        INFO  : Map 1: 1/1      Reducer 2: 1/1
        +----------+--------+--+
        |   sev    | count  |
        +----------+--------+--+
        | [ERROR]  | 3      |
        +----------+--------+--+
        1 row selected (47.351 seconds)

4. Verwenden Sie zum Beenden Beeline `!quit`.

##<a name="a-idfilearun-a-hiveql-file"></a><a id="file"></a>Führen Sie eine HiveQL-Datei

Beeline kann auch verwendet werden, eine Datei auszuführen, die HiveQL Anweisungen enthält. Gehen Sie folgendermaßen vor, und erstellen Sie eine Datei, dann Beeline auszuführen.

1. Verwenden Sie den folgenden Befehl, um eine neue Datei namens __query.hql__zu erstellen:

        nano query.hql
        
2. Sobald der-Editor geöffnet ist, verwenden Sie die folgenden als den Inhalt der Datei ein. Diese Abfrage erstellt eine neue 'interne' Tabelle mit dem Namen **Fehlerprotokolle von.**:

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    Diese Anweisungen führen Sie die folgenden Aktionen aus:

    * **Erstellen der Tabelle IF NOT EXISTS** - erstellt eine Tabelle, wenn es nicht bereits vorhanden ist. Da das **externe** Schlüsselwort nicht verwendet wird, ist dies eine interne Tabelle, die im Stock Datawarehouse gespeichert ist und vollständig von Struktur verwaltet wird.
    * **Gespeicherte AS ORC** - speichert die Daten im Format optimiert Zeile einspaltigen (ORC). Dies ist ein hochgradig optimierte und effiziente Format zum Speichern von Daten Struktur.
    * ÜBERSCHREIBEN der **Einfügen... Wählen Sie** – wählt Zeilen aus der Tabelle " **log4jLogs** ", die enthalten **[Fehler]**aus, und klicken Sie dann fügt die Daten in der Tabelle **Fehlerprotokolle von.** .
    
    > [AZURE.NOTE] Im Gegensatz zu externen Tabellen werden die zugrunde liegenden Daten ablegen eine interne Tabelle gelöscht werden.
    
3. Um die Datei speichern möchten, verwenden Sie __STRG__+ ___X__, und geben Sie dann __Y__, und klicken Sie abschließend auf __EINGABETASTE__.

4. Verwenden Sie die folgenden, um die Datei mithilfe von Beeline auszuführen. Ersetzen Sie __HOSTNAME__ mit dem Namen für die am Knoten und __das Kennwort__ mit dem Kennwort für das Administratorkonto zuvor abgerufen:

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin -i query.hql

    > [AZURE.NOTE] Die `-i` Parameter Beeline beginnt, führt die Anweisungen in der Datei query.hql und bleibt im Beeline bei der `jdbc:hive2://localhost:10001/>` auffordern. Sie können auch starten, indem Sie eine Datei im `-f` Parameter, die Sie zum Bash zurückgibt, nachdem die Datei verarbeitet wurde.

5. Um zu überprüfen, dass die Tabelle **Fehlerprotokolle von.** erstellt wurde, verwenden Sie die folgende Anweisung alle Zeilen von **Fehlerprotokolle von.**zurückgegeben:

        SELECT * from errorLogs;

    Drei Zeilen mit Daten zurückgegeben werden soll, alle enthaltenden **[Fehler]** in der Spalte t4:
    
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | errorlogs.t1  | errorlogs.t2  | errorlogs.t3  | errorlogs.t4  | errorlogs.t5  | errorlogs.t6  | errorlogs.t7  |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | 2012-02-03    | 18:35:34      | SampleClass0  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 18:55:54      | SampleClass1  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 19:25:27      | SampleClass4  | [ERROR]       | incorrect     | id            |               |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        3 rows selected (1.538 seconds)

## <a name="more-about-beeline-connectivity"></a>Weitere Informationen zu Beeline Konnektivität

Die Schritte in diesem Dokument verwenden `localhost` Verbindung zum HiveServer2 für die Cluster Headnode ausgeführt. Zwar Sie auch die Hostname können oder den vollqualifizierten Domänennamen des der Headnode die zusätzliche Schritte für den Prozess (Schritte zum Suchen den Hostname oder FQDN) erforderlich. Verwenden von `localhost` ist ausreichend, wenn Beeline aus der Headnode verwenden.

Wenn Sie einen Kantenknoten in Ihrem Cluster mit Beeline installiert haben, müssen Sie den Hostname oder FQDN von der Headnode die Verbindung zu verwenden.

Wenn Sie Beeline auf einem Cluster-Client installiert haben, können Sie eine Verbindung herstellen, mit dem folgenden Befehl. Ersetzen Sie __CLUSTERNAME__ mit dem Namen der HDInsight Cluster ein. Ersetzen Sie __PASSWORD__ durch das Kennwort für das Administratorkonto (HTTP-Anmeldung).

    beeline -u 'jdbc:hive2://CLUSTERNAME.azurehdinsight.net:443/default;ssl=true?hive.server2.transport.mode=http;hive.server2.thrift.http.path=hive2' -n admin -p PASSWORD

Beachten Sie, dass der Parameter-URI beim Ausführen direkt auf eine Headnode oder von einem Rand-Knoten im Cluster unterscheidet. Dies liegt daran einen öffentlichen Gateway, der Weiterleiten des Datenverkehrs über den Port 443, Herstellen einer Verbindung mit dem Cluster aus dem Internet verwendet werden. Darüber hinaus werden mehrere andere Dienste verfügbar gemacht über das öffentliche Gateway Port 443, damit der URI sich beim Herstellen einer Verbindung direkt unterscheidet. Wenn Sie aus dem Internet verbinden die Sitzung authentifizieren werden muss, indem Sie das Kennwort ein.

##<a name="a-idsummaryaa-idnextstepsanext-steps"></a><a id="summary"></a><a id="nextsteps"></a>Nächste Schritte

Wie Sie sehen können, bietet der Befehl Beeline auf einfache Weise interaktiv Struktur Abfragen in einem Cluster HDInsight ausführen.

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


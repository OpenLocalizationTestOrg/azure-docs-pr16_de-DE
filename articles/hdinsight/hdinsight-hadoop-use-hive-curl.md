<properties
   pageTitle="Verwenden Sie Hadoop Struktur mit Kringel in HDInsight | Microsoft Azure"
   description="Erfahren Sie, wie Remote Schwein Aufträge mit HDInsight mithilfe von Curl senden."
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
   ms.date="09/07/2016"
   ms.author="larryfr"/>

#<a name="run-hive-queries-with-hadoop-in-hdinsight-with-curl"></a>Führen Sie die Struktur Abfragen mit Hadoop in HDInsight mit Kringel

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

In diesem Dokument erfahren Sie, wie Curl Struktur Abfragen auf einer Hadoop auf Azure HDInsight Cluster ausgeführt werden.

Curl wird verwendet, um zu veranschaulichen, wie Sie interagieren können HDInsight mithilfe von unformatierten HTTP-Anfragen ausführen, überwachen und rufen Sie die Ergebnisse der Struktur Abfragen. Dies funktioniert, indem Sie mit der WebHCat REST-API (ehemals Templeton) durch Ihre HDInsight Cluster bereitgestellt.

> [AZURE.NOTE] Wenn Sie bereits mit der Verwendung von Linux-basierten Hadoop-Servern vertraut sind, aber noch neu bei HDInsight sind, finden Sie unter [Was Sie Hadoop auf Linux-basierten HDInsight kennen müssen](hdinsight-hadoop-linux-information.md).

##<a name="a-idprereqaprerequisites"></a><a id="prereq"></a>Erforderliche Komponenten

Um die Schritte in diesem Artikel ausführen zu können, benötigen Sie Folgendes:

* Eine Hadoop auf HDInsight Cluster (Linux oder Windows-basiertem)

* [Aufrollen](http://curl.haxx.se/)

* [jq](http://stedolan.github.io/jq/)

##<a name="a-idcurlarun-hive-queries-by-using-curl"></a><a id="curl"></a>Führen Sie Struktur Abfragen mithilfe von Curl aus

> [AZURE.NOTE] Wenn Curl oder einer beliebigen anderen REST-Kommunikation mit WebHCat verwenden zu können, müssen Sie die Anfragen können, indem Sie den Benutzernamen und das Kennwort für den HDInsight Clusteradministrator authentifizieren. Sie müssen den Clusternamen auch als Teil der der URI Uniform Resource Identifier () verwendet, um die Anforderungen an den Server gesendeten verwenden.
>
> Die Befehle in diesem Abschnitt ersetzen Sie **Benutzernamen** für den Benutzer mit dem Cluster authentifiziert, und Ersetzen Sie **PASSWORD** durch das Kennwort für das Benutzerkonto. Ersetzen Sie **CLUSTERNAME** mit dem Namen der Cluster aus.
>
> Die REST-API wird über [Standardauthentifizierung](http://en.wikipedia.org/wiki/Basic_access_authentication)gesichert. Stellen Sie Besprechungsanfragen immer, mithilfe von Secure HTTP (HTTPS), um sicherzustellen, dass Ihre Anmeldeinformationen ein, auf dem Server eine sichere Verbindung gesendet werden.

1. Verwenden Sie den folgenden Befehl von einer Befehlszeile aus um sicherzustellen, dass Sie auf Ihren Cluster HDInsight zugreifen können:

        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status

    Sie erhalten eine Antwort ähnlich wie der folgende aus:

        {"status":"ok","version":"v1"}

    Die Parameter in dieser Befehl verwendet werden wie folgt aus:

    * **-u** - Benutzername und Kennwort zum Authentifizieren der Anforderung verwendet.
    * **-G** - gibt an, dass dies eine GET-Anforderung ist.

    Anfang der URL, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, wird für alle Anfragen identisch sein. Der Pfad, **standardoption/Status**, gibt an, dass die Anforderung einen Status WebHCat (auch bekannt als Templeton) zurück für den Server. Sie können auch die Version der Struktur anfordern, mit den folgenden Befehl aus:

        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/version/hive

    Dadurch sollte eine Antwort ähnlich wie der folgende zurückgegeben:

        {"module":"hive","version":"0.13.0.2.1.6.0-2103"}

2. Verwenden Sie zum Erstellen einer neuen Tabelle mit dem Namen **log4jLogs**Folgendes ein:

        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="set+hive.execution.engine=tez;DROP+TABLE+log4jLogs;CREATE+EXTERNAL+TABLE+log4jLogs(t1+string,t2+string,t3+string,t4+string,t5+string,t6+string,t7+string)+ROW+FORMAT+DELIMITED+FIELDS+TERMINATED+BY+' '+STORED+AS+TEXTFILE+LOCATION+'wasbs:///example/data/';SELECT+t4+AS+sev,COUNT(*)+AS+count+FROM+log4jLogs+WHERE+t4+=+'[ERROR]'+AND+INPUT__FILE__NAME+LIKE+'%25.log'+GROUP+BY+t4;" -d statusdir="wasbs:///example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/hive

    Die Parameter in dieser Befehl verwendet werden wie folgt aus:

    * **-d** - da `-G` nicht verwendet wird, wird standardmäßig die Anforderung der POST-Methode. `-d`Gibt die Datenwerte, die gesendet werden mit der Anforderung an.

        * **Benutzer.Name** - der Benutzer, der den Befehl ausgeführt wird.

        * **Führen Sie** -HiveQL der auszuführenden Anweisungen.

        * **Statusdir** - Verzeichnis, die der Status für dieses Projekt geschrieben werden sollen.

    Diese Anweisungen führen Sie die folgenden Aktionen aus:

    * **DROP TABLE** - löscht die Tabelle und der Datendatei, wenn die Tabelle bereits vorhanden ist.

    * **Externe Tabelle erstellen** - erstellt eine neue 'externe' Tabelle in die Struktur. Externe Tabellen werden nur die Definition der Tabelle in die Struktur gespeichert. Die Daten an der ursprünglichen Stelle bleibt.

        > [AZURE.NOTE] Externe Tabellen sollte verwendet werden, wenn Sie erwarten, die zugrunde liegenden Daten mit einer externen Datenquelle, beispielsweise eine automatisierte Daten Uploadprozess oder von einem anderen Vorgang MapReduce, aktualisiert werden, aber immer Struktur Abfragen dass, um die neuesten Daten verwenden möchten.
        >
        > Enthält eine externe Tabelle ablegen **nicht** Löschen der Daten, nur die Definition der Tabelle.

    * **ZEILENFORMAT** : erfahren Struktur wie die Daten formatiert ist. In diesem Fall sind die Felder in jedem Protokoll durch ein Leerzeichen getrennt.

    * **AS Textdatei-Speicherort gespeichert** – erfahren Struktur Wo finde ich die Daten gespeichert (das Verzeichnis/Beispieldaten), und die It wird als Text gespeichert.

    * **Wählen Sie aus** – wählt Anzahl aller Zeilen, in denen Spalte **t4** den Wert **[Fehler]**enthält. Es gibt drei Zeilen, die diesen Wert enthalten sollte dadurch der Wert **3** zurückgegeben.

    > [AZURE.NOTE] Benachrichtigung, die durch die Leerzeichen zwischen HiveQL Anweisungen ersetzt werden die `+` bei Verwendung mit Kringel Zeichen. Werte in Anführungszeichen, die ein Leerzeichen ein, wie etwa das Trennzeichen enthalten sollte nicht durch ersetzt werden `+`.

    * **INPUT__FILE__NAME wie '% 25.log'** - diese Grenzwerte verwenden Sie die Suche auf nur enden. Log. Wenn dies nicht vorhanden ist, versucht Struktur Durchsuchen aller Dateien in dieses Verzeichnis und die zugehörigen Unterverzeichnisse, einschließlich Dateien, die für diese Tabelle definierte Spaltenschema nicht entsprechen.

    > [AZURE.NOTE] Hinweis, dass diese % 25 ist die URL codiert %, Formular, damit die ist-Bedingung ist `like '%.log'`. Die % muss URL codiert, werden, wie es als Sonderzeichen in URLs behandelt.

    Dieser Befehl sollte eine Auftrags-ID zurück, die zum Überprüfen des Status des Projekts verwendet werden können.

        {"id":"job_1415651640909_0026"}

3. Um den Status des Projekts zu überprüfen, verwenden Sie den folgenden Befehl ein. Ersetzen Sie **AUFTRAGS** mit dem Wert im vorherigen Schritt zurückgegeben. Beispielsweise, wenn der Rückgabewert war `{"id":"job_1415651640909_0026"}`, und klicken Sie dann **AUFTRAGS** wäre `job_1415651640909_0026`.

        curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state

    Wenn Sie der Auftrag abgeschlossen ist, wird der Status **erfolgreich**sein.

    > [AZURE.NOTE] Diese Anforderung Curl gibt Informationen über das Projekt ein Dokument JavaScript Object Notation (JSON); Jq wird verwendet, um nur den Bundesstaat Wert abzurufen.

4. Sobald der Zustand des Projekts zu **erfolgreich**geändert hat, können Sie die Ergebnisse des Projekts aus Azure Blob-Speicher abrufen. Die `statusdir` mit der Abfrage übergebene Parameter enthält den Speicherort der Ausgabedatei. In diesem Fall **Wasbs: / / / Beispiel/Curl**. Diese Adresse speichert die Ausgabe des Projekts im **Beispiel/Curl** Verzeichnis für den standardmäßigen Speichercontainer von Ihren Cluster HDInsight verwendet.

    Sie können Listen und mithilfe der [Azure CLI](../xplat-cli-install.md)auf diese Dateien herunterladen. Verwenden Sie den folgenden Befehl aus, um der Listendateien in **Beispiel/Curl**, beispielsweise:

        azure storage blob list <container-name> example/curl

    Wenn eine Datei herunterladen möchten, verwenden Sie Folgendes ein:

        azure storage blob download <container-name> <blob-name> <destination-file>

    > [AZURE.NOTE] Geben Sie entweder den Namen des Speicher-Kontos an, die mithilfe das Blob enthält die `-a` und `-k` Parameter oder Festlegen der **AZURE\_Speicher\_Konto** und **AZURE\_Speicher\_ACCESS\_KEY** Umgebungsvariablen. Finden Sie unter < ein Href = "Hdinsight Upload data.md" Target = "_blank" für Weitere Informationen.

6. Verwenden Sie die folgenden Anweisungen zum Erstellen einer neuen 'internen' Tabelle mit dem Namen **Fehlerprotokolle von.**:

        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="set+hive.execution.engine=tez;CREATE+TABLE+IF+NOT+EXISTS+errorLogs(t1+string,t2+string,t3+string,t4+string,t5+string,t6+string,t7+string)+STORED+AS+ORC;INSERT+OVERWRITE+TABLE+errorLogs+SELECT+t1,t2,t3,t4,t5,t6,t7+FROM+log4jLogs+WHERE+t4+=+'[ERROR]'+AND+INPUT__FILE__NAME+LIKE+'%25.log';SELECT+*+from+errorLogs;" -d statusdir="wasbs:///example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/hive

    Diese Anweisungen führen Sie die folgenden Aktionen aus:

    * **Erstellen der Tabelle IF NOT EXISTS** - erstellt eine Tabelle, wenn es nicht bereits vorhanden ist. Da das **externe** Schlüsselwort nicht verwendet wird, ist dies eine interne Tabelle, die im Stock Datawarehouse gespeichert ist und vollständig von Struktur verwaltet wird.

        > [AZURE.NOTE] Im Gegensatz zu externen Tabellen werden die zugrunde liegenden Daten ablegen eine interne Tabelle gelöscht werden.

    * **Gespeicherte AS ORC** - speichert die Daten im Format optimiert Zeile einspaltigen (ORC). Dies ist ein hochgradig optimierte und effiziente Format zum Speichern von Daten Struktur.
    * ÜBERSCHREIBEN der **Einfügen... Wählen Sie** – wählt Zeilen aus der Tabelle " **log4jLogs** ", die enthalten **[Fehler]**aus, und klicken Sie dann fügt die Daten in der Tabelle **Fehlerprotokolle von.** .
    * **Wählen Sie aus** – wählt alle Zeilen aus der neuen Tabelle **Fehlerprotokolle von.** .

7. Verwenden Sie die Auftrags-ID Überprüfen des Status des Projekts zurückgegeben. Sobald sie erfolgreich abgeschlossen ist, verwenden Sie Azure CLI für Mac, Linux und Windows beschriebenen zuvor herunterladen und Anzeigen der Ergebnisse. Die Ausgabe sollte drei Zeilen enthalten, denen **[Fehler]**enthalten.


##<a name="a-idsummaryasummary"></a><a id="summary"></a>Zusammenfassung

Wie in diesem Dokument gezeigt wird, können Sie eine unformatierte HTTP-Anforderung ausführen, überwachen und Anzeigen der Ergebnisse der Struktur Aufträge im HDInsight Cluster enthalten sind.

Weitere Informationen zu den in diesem Artikel verwendeten REST-Schnittstelle finden Sie unter den <a href="https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference" target="_blank">WebHCat verweisen</a>.

##<a name="a-idnextstepsanext-steps"></a><a id="nextsteps"></a>Nächste Schritte

Allgemeine Informationen zur Struktur mit HDInsight:

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




[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md

[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx



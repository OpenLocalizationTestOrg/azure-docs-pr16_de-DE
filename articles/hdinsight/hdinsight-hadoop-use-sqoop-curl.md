<properties
   pageTitle="Verwenden von Hadoop Sqoop mit Kringel in HDInsight | Microsoft Azure"
   description="Informationen Sie zum Sqoop Aufträge mit HDInsight mithilfe von Curl Remote zu übermitteln."
   services="hdinsight"
   documentationCenter=""
   authors="mumian"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/21/2016"
   ms.author="jgao"/>

#<a name="run-sqoop-jobs-with-hadoop-in-hdinsight-with-curl"></a>Führen Sie Sqoop Aufträge mit Hadoop in HDInsight mit Kringel

[AZURE.INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Erfahren Sie, wie mit Curl Sqoop Aufträge auf einem Cluster Hadoop in HDInsight ausgeführt werden.

Curl wird verwendet, um zu veranschaulichen, wie Sie interagieren können HDInsight mithilfe von unformatierten HTTP-Anfragen ausführen, überwachen und die Ergebnisse der Sqoop Aufträge abgerufen werden sollen. Dies funktioniert mit der WebHCat REST-API (ehemals Templeton) durch Ihre HDInsight Cluster bereitgestellt.

> [AZURE.NOTE] Wenn Sie bereits mit der Verwendung von Linux-basierten Hadoop-Servern vertraut sind, aber noch neu bei HDInsight sind, finden Sie unter [Informationen zur Verwendung von HDInsight unter Linux](hdinsight-hadoop-linux-information.md).

##<a name="prerequisites"></a>Erforderliche Komponenten

Um die Schritte in diesem Artikel ausführen zu können, benötigen Sie Folgendes:

* Eine Hadoop auf HDInsight Cluster (Linux oder Windows-basiertem)

* [Aufrollen](http://curl.haxx.se/)

* [jq](http://stedolan.github.io/jq/)

##<a name="submit-sqoop-jobs-by-using-curl"></a>Senden Sie Sqoop Aufträge mithilfe von Curl

> [AZURE.NOTE] Wenn Curl oder einer beliebigen anderen REST-Kommunikation mit WebHCat verwenden zu können, müssen Sie die Anfragen können, indem Sie den Benutzernamen und das Kennwort für den HDInsight Clusteradministrator authentifizieren. Sie müssen den Clusternamen auch als Teil der der URI Uniform Resource Identifier () verwendet, um die Anforderungen an den Server gesendeten verwenden.
>
> Die Befehle in diesem Abschnitt ersetzen Sie **Benutzernamen** für den Benutzer mit dem Cluster authentifiziert, und Ersetzen Sie **PASSWORD** durch das Kennwort für das Benutzerkonto. Ersetzen Sie **CLUSTERNAME** mit dem Namen der Cluster aus.
>
> Die REST-API wird über [Standardauthentifizierung](http://en.wikipedia.org/wiki/Basic_access_authentication)gesichert. Stellen Sie Besprechungsanfragen immer, mithilfe von Secure HTTP (HTTPS), um sicherzustellen, dass Ihre Anmeldeinformationen ein, auf dem Server eine sichere Verbindung gesendet werden.

1. Verwenden Sie den folgenden Befehl aus einer Befehlszeile aus um sicherzustellen, dass Ihre HDInsight Cluster hergestellt werden kann:

        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status

    Sie erhalten eine Antwort ähnlich wie der folgende aus:

        {"status":"ok","version":"v1"}

    Die Parameter in dieser Befehl verwendet werden wie folgt aus:

    * **-u** - Benutzername und Kennwort zum Authentifizieren der Anforderung verwendet.
    * **-G** - gibt an, dass dies eine GET-Anforderung ist.

    Anfang der URL, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, wird für alle Anfragen identisch sein. Der Pfad, **standardoption/Status**, gibt an, dass die Anforderung einen Status WebHCat (auch bekannt als Templeton) zurück für den Server. 

2. Senden ein Auftrags Sqoop anhand der folgenden:


        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d command="export --connect jdbc:sqlserver://SQLDATABASESERVERNAME.database.windows.net;user=USERNAME@SQLDATABASESERVERNAME;password=PASSWORD;database=SQLDATABASENAME --table log4jlogs --export-dir /tutorials/usesqoop/data --input-fields-terminated-by \0x20 -m 1" -d statusdir="wasbs:///example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/sqoop

    Die Parameter in dieser Befehl verwendet werden wie folgt aus:

    * **-d** - da `-G` nicht verwendet wird, wird standardmäßig die Anforderung der POST-Methode. `-d`Gibt die Datenwerte, die gesendet werden mit der Anforderung an.

        * **Benutzer.Name** - der Benutzer, der den Befehl ausgeführt wird.

        * **Befehl** - Sqoop der Befehl ausgeführt.

        * **Statusdir** - Verzeichnis, die der Status für dieses Projekt geschrieben werden sollen.

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

##<a name="limitations"></a>Einschränkungen

* Massen Exportieren - mit Linux-basierten HDInsight, der Sqoop Verbinder verwendet, um Daten in Microsoft SQL Server oder SQL Azure-Datenbank exportieren unterstützt derzeit nicht Massen eingefügt.

* Batchverarbeitung von-mit Linux-basierten HDInsight bei Verwendung der `-batch` wechseln, wenn die Durchführung fügt, Sqoop führt mehrere fügt anstelle der Batchvorgängen einfügen.

##<a name="summary"></a>Zusammenfassung

Wie in diesem Dokument gezeigt wird, können Sie eine unformatierte HTTP-Anforderung ausführen, überwachen und Anzeigen der Ergebnisse der Sqoop Aufträge auf Ihren Cluster HDInsight.

Weitere Informationen zu den in diesem Artikel verwendeten REST-Schnittstelle finden Sie unter der <a href="https://sqoop.apache.org/docs/1.99.3/RESTAPI.html" target="_blank">Sqoop REST-API Leitfaden</a>.

##<a name="next-steps"></a>Nächste Schritte

Allgemeine Informationen zur Struktur mit HDInsight:

* [Verwenden von Sqoop mit Hadoop auf HDInsight](hdinsight-use-sqoop.md)

Informationen zu anderen Methoden können Sie mit Hadoop auf HDInsight arbeiten:

* [Verwenden Sie die Struktur mit Hadoop auf HDInsight](hdinsight-use-hive.md)

* [Schwein mit Hadoop auf HDInsight verwenden](hdinsight-use-pig.md)

* [Verwenden von MapReduce mit Hadoop auf HDInsight](hdinsight-use-mapreduce.md)

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



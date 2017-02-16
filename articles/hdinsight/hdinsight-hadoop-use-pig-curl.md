<properties
   pageTitle="Verwenden von Hadoop Schwein mit Kringel in HDInsight | Microsoft Azure"
   description="Erfahren Sie, wie mit Curl Schwein Lateinisch Aufträge auf einem Cluster Hadoop in Azure HDInsight ausgeführt werden."
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
   ms.date="08/23/2016"
   ms.author="larryfr"/>

#<a name="run-pig-jobs-with-hadoop-on-hdinsight-by-using-curl"></a>Führen Sie Aufträge Schwein mit Hadoop auf HDInsight mithilfe von Curl

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

In diesem Dokument erfahren Sie, wie Curl Schwein Lateinisch Aufträge auf einem Cluster Azure HDInsight ausgeführt werden. Die Programmiersprache Schwein Lateinisch können Sie Transformationen beschreiben, die auf die Eingabedaten, um das gewünschte Ergebnis zu erzeugen angewendet werden.

Curl wird verwendet, um zu veranschaulichen, wie Sie interagieren können HDInsight mithilfe von unformatierten HTTP-Anfragen ausführen, überwachen und die Ergebnisse der Schwein Aufträge abgerufen werden sollen. Dies funktioniert, indem Sie mit der WebHCat REST-API (vormals als Templeton bezeichnet), die Ihren Cluster HDInsight zur Verfügung.

> [AZURE.NOTE] Wenn Sie bereits mit der Verwendung von Linux-basierten Hadoop-Servern vertraut sind, aber noch neu bei HDInsight sind, finden Sie unter [Tipps für Linux-basierten HDInsight](hdinsight-hadoop-linux-information.md).

##<a name="a-idprereqaprerequisites"></a><a id="prereq"></a>Erforderliche Komponenten

Um die Schritte in diesem Artikel ausführen zu können, benötigen Sie Folgendes:

* Ein Azure HDInsight (Hadoop auf HDInsight) Cluster (Linux-basierten oder Windows-basiertem)

* [Aufrollen](http://curl.haxx.se/)

* [jq](http://stedolan.github.io/jq/)

##<a name="a-idcurlarun-pig-jobs-by-using-curl"></a><a id="curl"></a>Führen Sie Aufträge Schwein mit Kringel

> [AZURE.NOTE] Wenn Curl oder einer beliebigen anderen REST-Kommunikation mit WebHCat verwenden zu können, müssen Sie die Anfragen können, indem Sie den Administrator-Benutzernamen und das Kennwort für den Cluster HDInsight authentifizieren. Sie müssen den Clusternamen auch als Teil der der URI Uniform Resource Identifier () verwenden, mit dem die Anforderungen an den Server gesendeten.
>
> Die Befehle in diesem Abschnitt ersetzen Sie **Benutzernamen** für den Benutzer mit dem Cluster authentifiziert, und Ersetzen Sie **PASSWORD** durch das Kennwort für das Benutzerkonto. Ersetzen Sie **CLUSTERNAME** mit dem Namen der Cluster aus.
>
> Die REST-API wird über die [einfachen Access-Authentifizierung](http://en.wikipedia.org/wiki/Basic_access_authentication)gesichert. Stellen Sie Besprechungsanfragen immer, mithilfe von Secure HTTP (HTTPS), um sicherzustellen, dass Ihre Anmeldeinformationen ein, auf dem Server eine sichere Verbindung gesendet werden.

1. Verwenden Sie den folgenden Befehl von einer Befehlszeile aus um sicherzustellen, dass Sie auf Ihren Cluster HDInsight zugreifen können:

        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status

    Sie erhalten eine Antwort ähnlich wie der folgende aus:

        {"status":"ok","version":"v1"}

    Die Parameter in dieser Befehl verwendet werden wie folgt aus:

    * **-u**: Benutzername und Kennwort verwendet, um die Anfrage authentifizieren
    * **-G**: Gibt an, dass dies eine GET-Anforderung ist

    Anfang der URL, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, wird für alle Anfragen identisch sein. Der Pfad, **standardoption/Status**, gibt an, dass die Anfrage den Status der WebHCat (auch bekannt als Templeton) zurückzugeben für den Server.

2. Verwenden Sie den folgenden Code einen Schwein Lateinisch Auftrag zum Cluster übermitteln an:

        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d execute="LOGS=LOAD+'wasbs:///example/data/sample.log';LEVELS=foreach+LOGS+generate+REGEX_EXTRACT($0,'(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)',1)+as+LOGLEVEL;FILTEREDLEVELS=FILTER+LEVELS+by+LOGLEVEL+is+not+null;GROUPEDLEVELS=GROUP+FILTEREDLEVELS+by+LOGLEVEL;FREQUENCIES=foreach+GROUPEDLEVELS+generate+group+as+LOGLEVEL,COUNT(FILTEREDLEVELS.LOGLEVEL)+as+count;RESULT=order+FREQUENCIES+by+COUNT+desc;DUMP+RESULT;" -d statusdir="wasbs:///example/pigcurl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/pig

    Die Parameter in dieser Befehl verwendet werden wie folgt aus:

    * **-d**: Da `-G` nicht verwendet wird, wird standardmäßig die Anforderung der POST-Methode. `-d`Gibt die Datenwerte, die gesendet werden mit der Anforderung an.

        * **Benutzer.Name**: den Benutzer, der den Befehl ausgeführt wird
        * **Ausführen**: das Schwein Lateinisch auszuführenden Anweisungen
        * **Statusdir**: das Verzeichnis, das der Status für dieses Projekt geschrieben werden können

    > [AZURE.NOTE] Benachrichtigung, die durch die Leerzeichen Schwein Lateinisch Anweisungen ersetzt werden die `+` bei Verwendung mit Kringel Zeichen.

    Dieser Befehl sollte eine Auftrags-ID zurück, die verwendet werden können, prüfen Sie den Status des Projekts, beispielsweise:

        {"id":"job_1415651640909_0026"}

3. Um den Status des Projekts zu überprüfen, verwenden Sie den folgenden Befehl ein. Ersetzen Sie **AUFTRAGS** mit dem Wert im vorherigen Schritt zurückgegeben. Beispielsweise, wenn der Rückgabewert war `{"id":"job_1415651640909_0026"}`, und klicken Sie dann **AUFTRAGS** wäre `job_1415651640909_0026`.

        curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state

    Wenn Sie der Auftrag abgeschlossen ist, wird der Status **erfolgreich**sein.

    > [AZURE.NOTE] Diese Anforderung Curl gibt ein Dokument JavaScript Object Notation (JSON) Informationen über das Projekt, und Jq wird verwendet, um nur den Statuswert abzurufen.

##<a name="a-idresultsaview-results"></a><a id="results"></a>Anzeigen der Ergebnisse

Wenn der Zustand des Projekts zu **erfolgreich**geändert hat, können Sie die Ergebnisse des Projekts aus Azure Blob-Speicher abrufen. Die `statusdir` mit der Abfrage übergebene Parameter enthält den Speicherort der Ausgabedatei. In diesem Fall **Wasbs: / / / Beispiel/Pigcurl**. Diese Adresse speichert die Ausgabe des Projekts im Verzeichnis **Beispiel/Pigcurl** in den standardmäßigen Speichercontainer von Ihren Cluster HDInsight verwendet.

Sie können Listen und mithilfe der [Azure CLI](../xplat-cli-install.md)auf diese Dateien herunterladen. Verwenden Sie den folgenden Befehl aus, um der Listendateien in **Beispiel/Pigcurl**, beispielsweise:

    azure storage blob list <container-name> example/pigcurl

Wenn eine Datei herunterladen möchten, verwenden Sie Folgendes ein:

    azure storage blob download <container-name> <blob-name> <destination-file>

> [AZURE.NOTE] Geben Sie den Namen des Speicher-Kontos an, die mithilfe das Blob enthält die `-a` und `-k` Parameter oder Festlegen der **AZURE\_Speicher\_Konto** und **AZURE\_Speicher\_ACCESS\_KEY** Umgebungsvariablen.

##<a name="a-idsummaryasummary"></a><a id="summary"></a>Zusammenfassung

Wie in diesem Dokument gezeigt wird, können Sie eine unformatierte HTTP-Anforderung ausführen, überwachen und die Ergebnisse der Aufträge Schwein auf Ihren Cluster HDInsight anzeigen.

Weitere Informationen zu den in diesem Artikel verwendeten REST-Schnittstelle finden Sie unter den [WebHCat verweisen](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).

##<a name="a-idnextstepsanext-steps"></a><a id="nextsteps"></a>Nächste Schritte

Allgemeine Informationen zu Schwein auf HDInsight:

* [Schwein mit Hadoop auf HDInsight verwenden](hdinsight-use-pig.md)

Weitere Informationen zu anderen Verfahren können Sie mit Hadoop auf HDInsight arbeiten:

* [Verwenden Sie die Struktur mit Hadoop auf HDInsight](hdinsight-use-hive.md)

* [Verwenden von MapReduce mit Hadoop auf HDInsight](hdinsight-use-mapreduce.md)

<properties
   pageTitle="Verwenden Sie MapReduce und Curl mit Hadoop in HDInsight | Microsoft Azure"
   description="Informationen Sie zum MapReduce Aufträge mit Hadoop auf Remote HDInsight mithilfe von Curl ausgeführt werden."
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
   ms.date="09/27/2016"
   ms.author="larryfr"/>

#<a name="run-mapreduce-jobs-with-hadoop-on-hdinsight-using-curl"></a>HDInsight mithilfe von Curl MapReduce Aufträge mit Hadoop ausgeführt

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

In diesem Dokument erfahren Sie, wie Curl MapReduce Aufträge auf einem Hadoop auf HDInsight Cluster ausgeführt werden.

Curl wird verwendet, um zu veranschaulichen, wie Sie mit HDInsight interagieren können, mithilfe von unformatierten HTTP-Anfragen MapReduce Auftrag ausführen. Dies funktioniert, indem Sie mit der WebHCat REST-API (ehemals Templeton) durch Ihre HDInsight Cluster bereitgestellt.

> [AZURE.NOTE] Wenn Sie bereits mit der Verwendung von Linux-basierten Hadoop-Servern vertraut sind, aber Sie neu bei HDInsight sind, finden Sie unter [Was Sie Linux-basierten Hadoop auf HDInsight kennen müssen](hdinsight-hadoop-linux-information.md).

##<a name="a-idprereqaprerequisites"></a><a id="prereq"></a>Erforderliche Komponenten

Um die Schritte in diesem Artikel ausführen zu können, benötigen Sie Folgendes:

* Eine Hadoop auf HDInsight Cluster (Linux oder Windows-basiertem)

* [Aufrollen](http://curl.haxx.se/)

* [jq](http://stedolan.github.io/jq/)

##<a name="a-idcurlarun-mapreduce-jobs-using-curl"></a><a id="curl"></a>Ausführen von MapReduce Aufträge mit Kringel

> [AZURE.NOTE] Wenn Sie Curl oder einer beliebigen anderen REST-Kommunikation mit WebHCat verwenden, müssen Sie die Anforderungen authentifizieren, durch die Bereitstellung der HDInsight Cluster Administrator-Benutzernamen und Ihr Kennwort ein. Sie müssen den Clusternamen auch als Teil des URIS verwenden, mit dem die Anforderungen an den Server gesendeten.
>
> Ersetzen Sie für die Befehle in diesem Abschnitt **USERNAME** durch den Benutzer für den Cluster, und das **Kennwort** mit dem Kennwort für das Benutzerkonto authentifiziert aus. Ersetzen Sie **CLUSTERNAME** mit dem Namen der Cluster aus.
>
> Die REST-API wird mithilfe einer [einfachen Access-Authentifizierung](http://en.wikipedia.org/wiki/Basic_access_authentication)gesichert. Stellen Sie Besprechungsanfragen immer, um sicherzustellen, dass Ihre Anmeldeinformationen sicher, auf dem Server gesendet werden mithilfe von HTTPS.

1. Verwenden Sie den folgenden Befehl mithilfe der Befehlszeile um sicherzustellen, dass Sie auf Ihren Cluster HDInsight zugreifen können:

        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status

    Sie erhalten eine Antwort ähnlich wie der folgende aus:

        {"status":"ok","version":"v1"}

    Die Parameter in dieser Befehl verwendet werden wie folgt aus:

    * **-u**: Zeigt an, den Benutzernamen und das Kennwort verwendet, um die Anfrage authentifizieren
    * **-G**: Gibt an, dass dies eine GET-Anforderung ist

    Anfang des URIS, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, ist für alle Anfragen.

2. Um einen MapReduce Auftrag zu senden, verwenden Sie den folgenden Befehl aus:

        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d jar=wasbs:///example/jars/hadoop-mapreduce-examples.jar -d class=wordcount -d arg=wasbs:///example/data/gutenberg/davinci.txt -d arg=wasbs:///example/data/CurlOut https://CLUSTERNAME.azurehdinsight.net/templeton/v1/mapreduce/jar

    Ende der URI (/ Mapreduce/Jar) erfahren WebHCat, dass diese Anforderung ein Auftrags MapReduce von einer Klasse in einer JAR-Datei gestartet werden kann. Die Parameter in dieser Befehl verwendet werden wie folgt aus:

    * **-d**: `-G` nicht verwendet wird, sodass die Anforderung der POST-Methode standardmäßig verwendet. `-d`Gibt die Datenwerte, die gesendet werden mit der Anforderung an.

        * **Benutzer.Name**: den Benutzer, der den Befehl ausgeführt wird
        * **JAR**: ist der Speicherort der JAR-Datei, die Klasse benutzerspezifisch enthält
        * **Klasse**: Klasse, die die MapReduce Logik enthält.
        * **Argument**: die an den Auftrag MapReduce; zu übergebenden Argumente auf: In diesem Fall, die Eingabewerte Textdatei und Verzeichnis, die für die Ausgabe verwendet werden

    Dieser Befehl sollte eine Auftrags-ID zurück, die zum Überprüfen des Status des Projekts verwendet werden können:

        {"id":"job_1415651640909_0026"}

3. Um den Status des Projekts zu überprüfen, verwenden Sie den folgenden Befehl ein. Ersetzen Sie den **AUFTRAGS** mit dem Wert im vorherigen Schritt zurückgegeben. Beispielsweise, wenn der Rückgabewert war `{"id":"job_1415651640909_0026"}`, und klicken Sie dann die AUFTRAGS wäre `job_1415651640909_0026`.

        curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state

    Wenn das Projekt abgeschlossen ist, wird der Status "wurde erfolgreich abgeschlossen werden".

    > [AZURE.NOTE] Diese Anforderung Curl gibt eine JSON-Dokument mit Informationen zu den Auftrag; Jq wird verwendet, um nur den Bundesstaat Wert abzurufen.

4. Wenn der Zustand des Projekts zu **erfolgreich**geändert hat, können Sie die Ergebnisse des Projekts aus Azure Blob-Speicher abrufen. Die `statusdir` Parameter, die mit der Abfrage weitergegeben wird enthält den Speicherort der Ausgabedatei. In diesem Fall **Wasbs: / / / Beispiel/Curl**. Diese Adresse speichert die Ausgabe des Projekts im **Beispiel/Curl** Verzeichnis in den standardmäßigen Speichercontainer von Ihren Cluster HDInsight verwendet.

Sie können Listen und mithilfe der [Azure CLI](../xplat-cli-install.md)auf diese Dateien herunterladen. Verwenden Sie in der Listendateien in dem **Beispiel/Curl**, beispielsweise den folgenden Befehl aus:

    azure storage blob list <container-name> example/curl

Wenn eine Datei herunterladen möchten, verwenden Sie Folgendes ein:

    azure storage blob download <container-name> <blob-name> <destination-file>

> [AZURE.NOTE] Geben Sie den Namen des Speicher-Kontos an, die mithilfe das Blob enthält die `-a` und `-k` Parameter oder Festlegen der **AZURE\_Speicher\_Konto** und **AZURE\_Speicher\_ACCESS\_KEY** Umgebungsvariablen. [Zum Hochladen von Daten mit HDInsight](hdinsight-upload-data.md) Weitere Informationen finden Sie unter.

##<a name="a-idsummaryasummary"></a><a id="summary"></a>Zusammenfassung

Wie in diesem Dokument gezeigt wird, können Sie unformatierte HTTP-Anforderung ausführen, überwachen und die Ergebnisse der Struktur Aufträge in Ihren Cluster HDInsight anzeigen.

Weitere Informationen zu den REST-Benutzeroberfläche, die in diesem Artikel verwendet wird, finden Sie unter den [WebHCat verweisen](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).

##<a name="a-idnextstepsanext-steps"></a><a id="nextsteps"></a>Nächste Schritte

Allgemeine Informationen zu MapReduce Aufträge in HDInsight:

* [Verwenden von MapReduce mit Hadoop auf HDInsight](hdinsight-use-mapreduce.md)

Weitere Informationen zu anderen Verfahren können Sie mit Hadoop auf HDInsight arbeiten:

* [Verwenden Sie die Struktur mit Hadoop auf HDInsight](hdinsight-use-hive.md)

* [Schwein mit Hadoop auf HDInsight verwenden](hdinsight-use-pig.md)

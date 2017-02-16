<properties
   pageTitle="Verwenden von Hadoop Schwein in HDInsight | Microsoft Azure"
   description="Informationen Sie zur Verwendung von Schwein mit Hadoop auf HDInsight."
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
   ms.date="09/14/2016"
   ms.author="larryfr"/>

# <a name="use-pig-with-hadoop-on-hdinsight"></a>Schwein mit Hadoop auf HDInsight verwenden

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

[Apache Schwein](http://pig.apache.org/) ist eine Plattform zum Erstellen von Programmen für Hadoop mithilfe einer zu Vorgehensweisen Sprache als *Schwein Lateinisch*bezeichnet. Schwein ist eine Alternative zum Java zum Erstellen von Lösungen *MapReduce* , und es ist im Lieferumfang von Azure HDInsight.

In diesem Artikel erfahren Sie, wie Schwein mit HDInsight verwendet werden können.

##<a name="a-idwhyawhy-use-pig"></a><a id="why"></a>Gründe für die Verwendung von Schwein

Eine große Herausforderung der Verarbeitung von Daten mithilfe von MapReduce in Hadoop ist Ihre Verarbeitungslogik Implementieren der mithilfe von nur ein Schema und einer Funktion verringern. Für die komplexe Verarbeitung häufig müssen Sie verkettet werden mehrere MapReduce Vorgänge Verarbeitung aufteilen zusammen, um das gewünschte Ergebnis erzielen.

Verwenden Sie nur die Karte und Funktionen zu verringern, sondern ermöglicht Schwein Verarbeitung als eine Reihe von Transformationen definieren, die die Daten in zu gelangen, um das gewünschte Ergebnis zu erzeugen.

Die Sprache Lateinisch Schwein können Sie den Datenfluss vom unformatierte Eingaben, durch eine oder mehrere Umwandlungen, um das gewünschte Ergebnis zu erzeugen zu beschreiben. Schwein Lateinisch Programme führen Sie diese allgemeinen Muster:

- **Laden**: Lesen von Daten aus dem Dateisystem bearbeitet werden soll
- **Transformieren**: Ändern der Daten
- **Abbild oder Store**: Ausgeben von Daten auf dem Bildschirm oder speichern Sie sie für die Verarbeitung

Schwein Lateinisch unterstützt auch benutzerdefinierte Funktionen (UDFs), wodurch Sie externe Komponenten aufzurufen, die Logik implementieren, die schwer zu Modell Schwein Latin ist.

Weitere Informationen zu Schwein Lateinisch finden Sie unter [Schwein Lateinisch Bezug manuellen 1](http://pig.apache.org/docs/r0.7.0/piglatin_ref1.html) und [2, Schwein Lateinisch Bezug manuell](http://pig.apache.org/docs/r0.7.0/piglatin_ref2.html).

Ein Beispiel für UDFs mit Schwein verwenden finden Sie die folgenden Dokumente:

* [Verwenden von DataFu mit Schwein in HDInsight](hdinsight-hadoop-use-pig-datafu-udf.md) - DataFu folgt eine Zusammenstellung von hilfreiche UDFs von Apache verwaltet

* [Verwenden von Python mit Schwein und Struktur in HDInsight](hdinsight-python.md)

* [Verwenden von c# mit Struktur und Schwein in HDInsight](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)

##<a name="a-iddataaabout-the-sample-data"></a><a id="data"></a>Zu den Beispieldaten

In diesem Beispiel wird eine Beispieldatei *log4j* , die am **/example/data/sample.log** in Ihrem BLOB-Speichercontainer gespeichert ist. Jedes Protokoll innerhalb der Datei besteht aus einer Zeile für die Felder, die enthält eine `[LOG LEVEL]` Feld, um die Art und die schwere, beispielsweise anzuzeigen:

    2012-02-03 20:26:41 SampleClass3 [ERROR] verbose detail for id 1527353937

Im vorherigen Beispiel ist die Ebene zurück.

> [AZURE.NOTE] Sie können auch mithilfe des Tools [Apache Log4j](http://en.wikipedia.org/wiki/Log4j) Protokollierung generieren eine log4j-Datei und Laden Sie die Datei in Ihrer Blob. Weitere Informationen finden Sie in der [Daten mit HDInsight hochladen](hdinsight-upload-data.md) . Weitere Informationen zur Verwendung von Blobs in Azure-Speicher mit HDInsight finden Sie unter [Verwenden von Azure BLOB-Speicher mit HDInsight](hdinsight-hadoop-use-blob-storage.md).

Die Beispieldaten werden in Azure Blob-Speicher gespeichert, die HDInsight als Standard-Dateisystem für Hadoop Cluster verwendet. HDInsight kann mit dem Präfix **Wasb** Blobs gespeicherte Dateien zugreifen. Zugriff auf die Datei sample.log verwenden Sie beispielsweise die folgende Syntax:

    wasbs:///example/data/sample.log

Da WASB der Standardspeicher für HDInsight ist, können Sie die Datei auch mithilfe von **/example/data/sample.log** aus Schwein Lateinisch zugreifen.

> [AZURE.NOTE] Die Syntax der, **Wasbs: / / /**, wird verwendet, um die Dateien, die im Standardcontainer-Speicher für Ihren Cluster HDInsight zugreifen. Wenn Sie zusätzlichen Speicherkonten angegeben, wenn Sie nach der Bereitstellung Ihrer Clusters und Zugriff auf Dateien, die in diese Konten gespeichert werden soll, können Sie die Daten zugreifen, indem Sie den Container und Speicher Kontoadresse, beispielsweise angeben: **wasbs://mycontainer@mystorage.blob.core.windows.net/example/data/sample.log**.


##<a name="a-idjobaabout-the-sample-job"></a><a id="job"></a>Informationen zu den Auftrag Stichprobe

Der folgende Schwein Lateinisch Auftrag lädt die **sample.log** -Datei aus dem Standardspeicher für Ihren Cluster HDInsight. Klicken Sie dann führt eine Reihe von Transformationen, die Anzahl der wie oft führen jede Logebene in die Eingabedaten aufgetreten. Die Ergebnisse sind in STDOUT ausgegeben.

    LOGS = LOAD 'wasbs:///example/data/sample.log';
    LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
    FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
    GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
    FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
    RESULT = order FREQUENCIES by COUNT desc;
    DUMP RESULT;

Die folgende Abbildung zeigt einen Überblick über die Funktionsweise von jede Transformation zu den Daten.

![Grafische Darstellung der Transformationen][image-hdi-pig-data-transformation]

##<a name="a-idrunarun-the-pig-latin-job"></a><a id="run"></a>Führen Sie die Stapelverarbeitung Schwein Lateinisch

HDInsight kann Schwein Lateinisch Aufträge ausgeführt werden mithilfe verschiedener Methoden. Anhand der folgenden Tabelle entscheiden, welche Methode für Sie richtig ist, und klicken Sie dann auf den Link für eine exemplarische Vorgehensweise.

| **Verwenden Sie diese Option** , wenn Sie möchten...                                   | ... .ar **interaktiven** Shell | ... **Stapelverarbeitung** | ... .with dieses **Cluster-Betriebssystem** | ... .from dieses **Client-Betriebssystem** |
|:--------------------------------------------------------------|:---------------------------:|:-----------------------:|:------------------------------------------|:-----------------------------------------|
| [SSH](hdinsight-hadoop-use-pig-ssh.md)                        |              ✔              |            ✔            | Linux                                     | Linux, Unix, Mac OS X oder Windows        |
| [Aufrollen](hdinsight-hadoop-use-pig-curl.md)                      |           &nbsp;            |            ✔            | Linux oder Windows                          | Linux, Unix, Mac OS X oder Windows        |
| [.NET SDK für Hadoop](hdinsight-hadoop-use-pig-dotnet-sdk.md) |           &nbsp;            |            ✔            | Linux oder Windows                          | Windows (für jetzt)                        |
| [Windows PowerShell](hdinsight-hadoop-use-pig-powershell.md)  |           &nbsp;            |            ✔            | Linux oder Windows                          | Windows                                  |
| [Remotedesktop](hdinsight-hadoop-use-pig-remote-desktop.md)  |              ✔              |            ✔            | Windows                                   | Windows                                  |


## <a name="running-pig-jobs-on-azure-hdinsight-using-on-premises-sql-server-integration-services"></a>Ausführen von Schwein Aufträgen auf Azure HDInsight mithilfe der lokalen SQL Server Integration Services

Sie können auch SQL Server Integration Services (SSIS) verwenden, um eine Schwein auszuführen. Das Feature Pack Azure für SSIS bietet die folgenden Komponenten, mit denen Schwein Aufträge auf HDInsight zusammenarbeiten.


- [Azure HDInsight Schwein Aufgabe][pigtask]
- [Azure Abonnement Verbindungs-Managers][connectionmanager]


Weitere Informationen über das Azure Feature Pack für SSIS [hier][ssispack].


##<a name="a-idnextstepsanext-steps"></a><a id="nextsteps"></a>Nächste Schritte

Jetzt, da Sie zur Verwendung von Schwein mit HDInsight vertraut gemacht haben, verwenden Sie die folgenden Links, um andere Methoden für die Arbeit mit Azure HDInsight untersuchen.

* [Hochladen von Daten mit HDInsight][hdinsight-upload-data]
* [Verwenden Sie die Struktur mit HDInsight][hdinsight-use-hive]
* [Verwenden von Sqoop mit HDInsight](hdinsight-use-sqoop.md)
* [Verwenden von Oozie mit HDInsight](hdinsight-use-oozie.md)
* [Verwenden von MapReduce Aufträge mit HDInsight][hdinsight-use-mapreduce]

[check]: ./media/hdinsight-use-pig/hdi.checkmark.png

[apachepig-home]: http://pig.apache.org/
[putty]: http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html
[curl]: http://curl.haxx.se/
[pigtask]: http://msdn.microsoft.com/library/mt146781(v=sql.120).aspx
[connectionmanager]: http://msdn.microsoft.com/library/mt146773(v=sql.120).aspx
[ssispack]: http://msdn.microsoft.com/library/mt146770(v=sql.120).aspx

[hdinsight-storage]: hdinsight-use-blob-storage.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: ../hdinsight-get-started.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md#mapreduce-sdk

[Powershell-install-configure]: ../powershell-install-configure.md

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx

[image-hdi-log4j-sample]: ./media/hdinsight-use-pig/HDI.wholesamplefile.png
[image-hdi-pig-data-transformation]: ./media/hdinsight-use-pig/HDI.DataTransformation.gif
[image-hdi-pig-powershell]: ./media/hdinsight-use-pig/hdi.pig.powershell.png
[image-hdi-pig-architecture]: ./media/hdinsight-use-pig/HDI.Pig.Architecture.png

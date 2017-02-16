<properties
    pageTitle="Erfahren Sie, was Struktur ist und zum Verwenden von HiveQL | Microsoft Azure"
    description="Informationen Sie zu Apache Struktur und wie Sie es mit Hadoop in HDInsight verwenden. Wählen Sie zum Ausführen Ihrer Position Struktur und HiveQL analysieren eine Apache log4j Beispieldatei verwenden."
    keywords="Hiveql, was Struktur ist"
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
    ms.date="09/19/2016"
    ms.author="larryfr"/>

# <a name="use-hive-and-hiveql-with-hadoop-in-hdinsight-to-analyze-a-sample-apache-log4j-file"></a>Verwenden von Struktur und HiveQL mit Hadoop in HDInsight zum Analysieren einer Apache log4j-Beispieldatei

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]


In diesem Lernprogramm erhalten Sie Informationen zum Verwenden von Apache-Struktur in Hadoop auf HDInsight und auswählen, wie Sie Ihre Arbeit Struktur ausführen. Sie lernen auch über HiveQL und wie eine Beispieldatei für Apache log4j analysieren.

##<a name="a-idwhyawhat-is-hive-and-why-use-it"></a><a id="why"></a>Was ist die Struktur und warum verwenden?
[Apache Struktur](http://hive.apache.org/) ist eine Warehouse Datensystem für Hadoop, wodurch Zusammenfassung der Daten, die Abfrage und Analyse von Daten mithilfe von HiveQL (eine ähnliche SQL Abfragesprache). Struktur kann Ihre Daten interaktiv zu durchsuchen oder wieder verwendbaren Blattnamen Verarbeitungsaufträge verwendet werden.

Struktur ermöglicht Ihnen das Projektstruktur weitgehend unstrukturierte Daten. Nachdem Sie die Struktur definiert haben, können Sie Struktur, die Daten ohne Kenntnisse über Java oder MapReduce Abfragen. **HiveQL** (die Struktur-Abfragesprache) können Sie Abfragen mit Anweisungen zu schreiben, die ähnliche T-SQL sind.

Struktur weiß, wie mit strukturierter und teilweise strukturierter Daten, wie z. B. Textdateien arbeiten, in dem die Felder durch Zeichen getrennt werden. Struktur unterstützt auch benutzerdefinierte **Serialisierungsprogramm/Deserialisierer (SerDe)** für komplexe oder unregelmäßigen strukturierte Daten. Weitere Informationen finden Sie unter [Verwenden eines benutzerdefinierten JSON-SerDe mit HDInsight](http://blogs.msdn.com/b/bigdatasupport/archive/2014/06/18/how-to-use-a-custom-json-serde-with-microsoft-azure-hdinsight.aspx).

## <a name="user-defined-functions-udf"></a>Benutzerdefinierte Funktionen (UDFs)

Struktur kann auch durch **benutzerdefinierte Funktionen (UDFs)**erweitert werden. UDFs können Sie Funktionen oder -Logik, die einfach erstellt wird nicht in HiveQL implementieren. Ein Beispiel der Verwendung von UDFs mit Struktur Probieren Sie Folgendes ein:

* [Verwenden eines Benutzers Java benutzerdefinierte Funktion mit Struktur](hdinsight-hadoop-hive-java-udf.md)

* [Verwenden von Python mit Struktur und Schwein in HDInsight](hdinsight-python.md)

* [Verwenden von c# mit Struktur und Schwein in HDInsight](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)

* [Zum Hinzufügen einer benutzerdefinierten Struktur UDFs mit HDInsight](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/14/how-to-add-custom-hive-udfs-to-hdinsight.aspx)

* [Benutzerdefinierte Struktur UDFs Beispiel Datums-/Uhrzeitformate Struktur Zeitstempel konvertieren](https://github.com/Azure-Samples/hdinsight-java-hive-udf)

## <a name="hive-internal-tables-vs-external-tables"></a>Struktur internen Tabellen im Vergleich mit einer externen Tabellen

Es gibt ein paar Punkte, die Sie kennen die strukturtabelle internen und externen Tabelle müssen:

- Der Befehl **Tabelle erstellen** , wird eine interne Tabelle erstellt. Die Datendatei muss in der standardmäßige Container befinden.
- Der Befehl ' **Tabelle erstellen** ' verschiebt die Datendatei /hive/Warehouse /<TableName> Ordner.
- Der Befehl **Externe Tabelle erstellen** wird eine externe Tabelle erstellt. Die Datendatei kann außerhalb der standardmäßige Container befinden.
- Der Befehl **Externe Tabelle erstellen** wird die Datendatei nicht verschoben.
- Der Befehl **Externe Tabelle erstellen** zulassen keine Ordner in den Speicherort. Dies ist der Grund, warum das Lernprogramm eine Kopie der Datei sample.log erstellt.

Weitere Informationen finden Sie unter [HDInsight: Struktur internen und externen Tabellen Einführung][cindygross-hive-tables].


##<a name="a-iddataaabout-the-sample-data-an-apache-log4j-file"></a><a id="data"></a>Zu den Beispieldaten, eine Apache log4j-Datei

In diesem Beispiel wird eine Beispieldatei *log4j* , die am **/example/data/sample.log** in Ihrem BLOB-Speichercontainer gespeichert ist. Jedes Protokoll innerhalb der Datei besteht aus einer Zeile für die Felder, die enthält eine `[LOG LEVEL]` Feld, um die Art und die schwere, beispielsweise anzuzeigen:

    2012-02-03 20:26:41 SampleClass3 [ERROR] verbose detail for id 1527353937

Im vorherigen Beispiel ist die Ebene zurück.

> [AZURE.NOTE] Sie können auch mithilfe des Tools [Apache Log4j](http://en.wikipedia.org/wiki/Log4j) Protokollierung generieren eine Datei log4j und Laden Sie die Datei in den Container Blob. Weitere Informationen finden Sie in der [Daten mit HDInsight hochladen](hdinsight-upload-data.md) . Weitere Informationen darüber, wie Azure Blob-Speicher mit HDInsight verwendet wird finden Sie unter [Verwenden von Azure BLOB-Speicher mit HDInsight](hdinsight-hadoop-use-blob-storage.md).

Die Beispieldaten werden in Azure Blob-Speicher gespeichert, die HDInsight als Standard-Dateisystem verwendet. HDInsight kann mit dem Präfix **Wasb** Blobs gespeicherte Dateien zugreifen. Zugriff auf die Datei sample.log verwenden Sie beispielsweise die folgende Syntax:

    wasbs:///example/data/sample.log

Da Azure Blob-Speicher der Standardspeicher für HDInsight ist, können Sie die Datei auch mithilfe von **/example/data/sample.log** aus HiveQL zugreifen.

> [AZURE.NOTE] Die Syntax der, **Wasbs: / / /**, wird verwendet, um die Dateien, die im Standardcontainer-Speicher für Ihren Cluster HDInsight zugreifen. Wenn Sie zusätzlichen Speicherkonten angegeben, wenn Sie nach der Bereitstellung Ihrer Clusters und Zugriff auf Dateien, die in diese Konten gespeichert werden soll, können Sie die Daten zugreifen, indem Sie den Container und Speicher Kontoadresse, beispielsweise angeben **wasbs://mycontainer@mystorage.blob.core.windows.net/example/data/sample.log**.

##<a name="a-idjobasample-job-project-columns-onto-delimited-data"></a><a id="job"></a>Beispiel für Position: Project Spalten auf durch Trennzeichen getrennte Daten

Die folgenden Aussagen HiveQL werden Spalten auf durch Trennzeichen getrennte Daten, die in gespeichert ist project die **Wasbs: / / / / Beispieldaten** Verzeichnis:

    set hive.execution.engine=tez;
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
    SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

Im vorherigen Beispiel führen Sie die Anweisungen HiveQL die folgenden Aktionen aus:

* __Festlegen von hive.execution.engine=tez;__: Legt die Execution-Engine Tez verwenden. Mithilfe von Tez statt MapReduce kann eine Erhöhung der Leistung von Abfragen bereitstellen. Weitere Informationen Tez finden Sie unter Abschnitt [Verwenden Apache Tez Leistung zu verbessern](#usetez) .

    > [AZURE.NOTE] Diese Anweisung ist nur erforderlich, wenn Sie mit einem Windows-basierten HDInsight Cluster; Tez ist der standardmäßigen Execution-Engine für Linux-basierten HDInsight.

* **Tabelle löschen**: Löscht die Tabelle und der Datendatei, wenn die Tabelle bereits vorhanden ist.
* **Externe Tabelle erstellen**: erstellt eine neue **externe** Tabelle in die Struktur. Externe Tabellen speichern nur die Definition der Tabelle in Struktur. die Daten in der ursprünglichen Position und im ursprünglichen Format verbleiben.
* **ZEILENFORMAT**: erfahren Struktur wie die Daten formatiert ist. In diesem Fall sind die Felder in jedem Protokoll durch ein Leerzeichen getrennt.
* **AS Textdatei-Speicherort gespeichert**: erfahren Struktur, wo die Daten sind (das Verzeichnis/Beispieldaten) gespeichert und, die sie als Text gespeichert ist. Die Daten können auf mehrere Dateien im Systemverzeichnis verteilt oder in einer Datei werden.
* **Wählen Sie aus**: Anzahl aller Zeilen, in denen die Spalte **t4** den Wert **[Fehler enthält]**markiert. Dies sollte einen Wert von **3** zurück, da es gibt drei Zeilen, die diesen Wert enthalten.
* **INPUT__FILE__NAME wie '%.log'** - teilt Struktur, die wir nur Daten aus, die in Dateien zurückgeben sollte. Log. Hiermit legen Sie das Feld Suchen die sample.log-Datei, die die Daten enthält, und verhindert, dass Sie zurückgeben von Daten aus anderen Beispiel Datendateien, die nicht das Schema entsprechen, die, das wir definiert.

> [AZURE.NOTE] Externe Tabellen sollte verwendet werden, wenn die zugrunde liegenden Daten aktualisiert werden, indem Sie einer externen Quelle, wie etwa eine automatisierte Daten hochladen, oder ein anderer MapReduce Vorgang erwartet, und immer Struktur Abfragen, um die neuesten Daten verwenden möchten.
>
> Ablegen einer externen Tabelle enthält **nicht** die Daten zu löschen, sondern nur die Definition der Tabelle.

Nach dem Erstellen einer externen Tabelle werden die folgenden Aussagen verwendet, um eine **interne** Tabelle erstellen.

    set hive.execution.engine=tez;
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs
    SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]';

Diese Anweisungen führen Sie die folgenden Aktionen aus:

* **Erstellen der Tabelle IF NOT EXISTS**: erstellt eine Tabelle, wenn es nicht bereits vorhanden ist. Da das **EXTERNEN** Schlüsselwort nicht verwendet wird, ist dies eine interne Tabelle, die im Stock Datawarehouse gespeichert ist und vollständig von Struktur verwaltet wird.
* **Gespeicherte AS ORC**: speichert die Daten im Format optimiert Zeile einspaltigen (ORC). Dies ist ein hochgradig optimierte und effiziente Format zum Speichern von Daten Struktur.
* ÜBERSCHREIBEN der **Einfügen... Wählen Sie**: Wählt Zeilen aus der Tabelle **log4jLogs** , die **[Fehler]**enthält, und klicken Sie dann fügt die Daten in der Tabelle **Fehlerprotokolle von.** .

> [AZURE.NOTE] Im Gegensatz zu externen Tabellen löscht eine interne Tabelle ablegen auch die zugrunde liegenden Daten.

##<a name="a-idusetezause-apache-tez-for-improved-performance"></a><a id="usetez"></a>Verwenden von Apache Tez für verbesserte Leistung

[Apache Tez](http://tez.apache.org) ist ein Framework, das Daten stark Clientanwendungen, z. B. Struktur, an, die bei effizienter ausführen zu können. In der neuesten Version von HDInsight unterstützt Struktur Tez ausgeführt. Tez ist standardmäßig für HDInsight Linux-basierten Cluster aktiviert.

> [AZURE.NOTE] Tez ist derzeit deaktiviert standardmäßig für Windows-basiertem HDInsight Cluster und muss aktiviert sein. Um Tez nutzen zu können, muss der folgende Wert für eine Abfrage Struktur festgelegt sein:
>
> ```set hive.execution.engine=tez;```
>
>Dies kann auf Basis pro Abfrage unter Ähnlichkeit am Anfang der Abfrage gesendet werden. Sie können auch dies festlegen auf werden standardmäßig in einem Cluster Konfigurationswert festlegen, wenn Sie den Cluster erstellen. Weitere Informationen hierzu finden Sie in [HDInsight Cluster bereitgestellt](hdinsight-provision-clusters.md).

Die [Struktur auf Tez Entwerfen von Dokumenten](https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez) umfassen viele Details Implementierung Auswahlmöglichkeiten und Videogeräten Konfigurationen.

Aufträge Debuggen zur Makrofunktion war mit Tez, HDInsight bietet das folgende Web Benutzeroberflächen, mit denen Sie die Details der Tez Aufträge anzeigen können:

* [Mithilfe der Tez Benutzeroberfläche auf Windows basierende HDInsight](hdinsight-debug-tez-ui.md)

* [Verwenden der Ansicht Ambari Tez auf Linux-basierten HDInsight](hdinsight-debug-ambari-tez-view.md)

##<a name="a-idrunachoose-how-to-run-the-hiveql-job"></a><a id="run"></a>Wählen Sie den Auftrag HiveQL ausführen

HDInsight kann HiveQL Aufträge mithilfe verschiedener Methoden ausgeführt werden. Anhand der folgenden Tabelle entscheiden, welche Methode für Sie richtig ist, und klicken Sie dann auf den Link für eine exemplarische Vorgehensweise.

| **Verwenden Sie diese Option** , wenn Sie möchten...                                                     | ... .ar **interaktiven** Shell | ... **Stapelverarbeitung** | ... .with dieses **Cluster-Betriebssystem** | ... .from dieses **Client-Betriebssystem** |
|:--------------------------------------------------------------------------------|:---------------------------:|:-----------------------:|:------------------------------------------|:-----------------------------------------|
| [Struktur anzeigen](hdinsight-hadoop-use-hive-ambari-view.md) | ✔ | ✔ | Linux | Alle (browserbasierte) |
| [Beeline-Befehl (über eine Sitzung SSH)](hdinsight-hadoop-use-hive-beeline.md)                                         |              ✔              |            ✔            | Linux                                     | Linux, Unix, Mac OS X oder Windows        |
| [Struktur-Befehl (über eine Sitzung SSH)](hdinsight-hadoop-use-hive-ssh.md)                                         |              ✔              |            ✔            | Linux                                     | Linux, Unix, Mac OS X oder Windows        |
| [Aufrollen](hdinsight-hadoop-use-hive-curl.md)                                       |           &nbsp;            |            ✔            | Linux oder Windows                          | Linux, Unix, Mac OS X oder Windows        |
| [Abfrage-Konsole](hdinsight-hadoop-use-hive-query-console.md)                     |           &nbsp;            |            ✔            | Windows                                   | Alle (browserbasierte)                            |
| [HDInsight Tools für Visual Studio](hdinsight-hadoop-use-hive-visual-studio.md) |           &nbsp;            |            ✔            | Linux oder Windows                          | Windows                                  |
| [Windows PowerShell](hdinsight-hadoop-use-hive-powershell.md)                   |           &nbsp;            |            ✔            | Linux oder Windows                          | Windows                                  |
| [Remotedesktop](hdinsight-hadoop-use-hive-remote-desktop.md)                   |              ✔              |            ✔            | Windows                                   | Windows                                  |

## <a name="running-hive-jobs-on-azure-hdinsight-using-on-premises-sql-server-integration-services"></a>Ausführen von Struktur Aufträgen auf Azure HDInsight mithilfe der lokalen SQL Server Integration Services

Sie können auch SQL Server Integration Services (SSIS) verwenden, um eine Struktur auszuführen. Das Feature Pack Azure für SSIS bietet die folgenden Komponenten, mit denen Struktur Aufträge auf HDInsight zusammenarbeiten.


- [Aufgabe Azure HDInsight Struktur][hivetask]
- [Azure Abonnement Verbindungs-Managers][connectionmanager]


Weitere Informationen über das Azure Feature Pack für SSIS [hier][ssispack].


##<a name="a-idnextstepsanext-steps"></a><a id="nextsteps"></a>Nächste Schritte

Jetzt, da Sie beherrschen, was Struktur ist und wie Sie es mit Hadoop in HDInsight verwenden, verwenden Sie die folgenden Links, um andere Methoden für die Arbeit mit Azure HDInsight untersuchen.


- [Hochladen von Daten mit HDInsight][hdinsight-upload-data]
- [Schwein mit HDInsight verwenden][hdinsight-use-pig]
- [Verwenden von Sqoop mit HDInsight](hdinsight-use-sqoop.md)
- [Verwenden von Oozie mit HDInsight](hdinsight-use-oozie.md)
- [Verwenden von MapReduce Aufträge mit HDInsight][hdinsight-use-mapreduce]

[check]: ./media/hdinsight-use-hive/hdi.checkmark.png

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/
[hivetask]: http://msdn.microsoft.com/library/mt146771(v=sql.120).aspx
[connectionmanager]: http://msdn.microsoft.com/library/mt146773(v=sql.120).aspx
[ssispack]: http://msdn.microsoft.com/library/mt146770(v=sql.120).aspx

[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md


[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-get-started.md

[Powershell-install-configure]: ../powershell-install-configure.md
[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx

[image-hdi-hive-powershell]: ./media/hdinsight-use-hive/HDI.HIVE.PowerShell.png
[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png
[image-hdi-hive-architecture]: ./media/hdinsight-use-hive/HDI.Hive.Architecture.png


[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

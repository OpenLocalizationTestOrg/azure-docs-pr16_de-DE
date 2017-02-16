<properties
    pageTitle="Skript-Aktion verwenden, um Solr auf Hadoop Cluster zu installieren | Microsoft Azure"
    description="Erfahren Sie, wie HDInsight Cluster mit Solr mithilfe der Aktion Skript angepasst."
    services="hdinsight"
    documentationCenter=""
    authors="nitinme"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/05/2016"
    ms.author="nitinme"/>

# <a name="install-and-use-solr-on-hdinsight-hadoop-clusters"></a>Installieren und Verwenden von Solr auf HDInsight Hadoop Cluster

Erfahren Sie, wie Windows basierend HDInsight Cluster mit Solr mithilfe der Aktion Skript anpassen, und wie Sie Solr verwenden, um Daten zu suchen. Informationen zum Verwenden von Solr mit einem Linux-basierten Cluster finden Sie unter [Installieren und Verwenden von Solr auf HDinsight Hadoop Cluster (Linux)](hdinsight-hadoop-solr-install-linux.md).
 
Sie können mithilfe der *Aktion Skript*Solr auf einem beliebigen Cluster (Hadoop Storm, HBase, Spark) auf Azure HDInsight installieren. Ein Beispiel-Skript zum Installieren von Solr in einem HDInsight Cluster steht aus einer schreibgeschützten Azure-Speicher Blob am [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1). 

Das Beispielskript funktioniert nur mit HDInsight Clusterversion 3.1. Weitere Informationen zum HDInsight Cluster Versionen finden Sie unter [HDInsight Cluster Versionen](hdinsight-component-versioning.md).

In diesem Thema verwendete Skript erstellt einen Windows-basierten Solr Cluster mit einer bestimmten Konfiguration. Wenn Sie Solr Cluster mit anderen Websitesammlungen, mehrere Shards hinweg, Schemas, Replikate usw. konfigurieren möchten, müssen Sie das Skript und Solr Binärdateien entsprechend ändern.

**Verwandte Artikel**

- [Installieren und Verwenden von Solr auf HDinsight Hadoop Cluster (Linux)](hdinsight-hadoop-solr-install-linux.md)
- [Hadoop erstellen Cluster in HDInsight](hdinsight-provision-clusters.md): Allgemeine Informationen zum Erstellen von HDInsight Cluster.
- [Anpassen von HDInsight Cluster mithilfe der Aktion Skript][hdinsight-cluster-customize]: Allgemeine Informationen zum Anpassen von HDInsight Cluster mithilfe der Aktion Skript.
- [Skript für Aktion entwickeln Skripts für HDInsight](hdinsight-hadoop-script-actions.md).


## <a name="what-is-solr"></a>Was ist Solr?

<a href="http://lucene.apache.org/solr/features.html" target="_blank">Apache Solr</a> ist eine Enterprise-Suche-Plattform, die leistungsfähige voll-Textsuche auf Daten ermöglicht. Während der Hadoop ermöglicht das Speichern und Verwalten von große Datenmengen, bietet Apache Solr die Suchfunktionen, um schnell die Daten abzurufen. 

## <a name="install-solr-using-portal"></a>Installieren von Solr mithilfe von portal

1. Starten Sie, einen Cluster mithilfe der Option **Benutzerdefinierte erstellen** erstellen, wie bei [Erstellen Hadoop Cluster in HDInsight](hdinsight-provision-clusters.md#portal)beschrieben.
2. Klicken Sie auf der Seite des Assistenten **Skript-Aktionen** auf **Skriptaktion hinzufügen** , um die Details zu der Skriptaktion angeben, wie unten dargestellt:

    ![Verwenden Sie Skript für Aktion in einem Cluster anpassen] (./media/hdinsight-hadoop-solr-install/hdi-script-action-solr.png "Verwenden Sie Skript für Aktion in einem Cluster anpassen")

    <table border='1'>
        <tr><th>Eigenschaft</th><th>Wert</th></tr>
        <tr><td>Namen</td>
            <td>Geben Sie einen Namen für die Skriptaktion ein. Beispielsweise <b>Solr installieren</b>.</td></tr>
        <tr><td>URI-Skript</td>
            <td>Geben Sie an der URI Uniform Resource Identifier () an das Skript, das zum Anpassen des Clusters aufgerufen wird. Beispielsweise <i>https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1</i></td></tr>
        <tr><td>Knotentyp</td>
            <td>Geben Sie die Knoten, auf denen das Skript Anpassung ausgeführt wird. Sie können <b>alle Knoten</b>, <b>nur Kopf Knoten</b>oder <b>nur Worker-Knoten</b>auswählen.
        <tr><td>Parameter</td>
            <td>Geben Sie den Parameter aus, wenn das Skript erforderlich. Das Skript Solr installieren sind von Parametern verwenden, nicht erforderlich, damit Sie dieses Feld leer lassen können.</td></tr>
    </table>

    Sie können mehr als eine Skriptaktion zum Installieren von mehreren Komponenten auf dem Cluster hinzufügen. Nachdem Sie die Skripts hinzugefügt haben, klicken Sie auf das Häkchen zum Starten den Cluster erstellen.


## <a name="use-solr"></a>Verwenden von Solr

Sie müssen mit Solr mit einigen Datendateien indizieren beginnen. Solr können dann Suchabfragen auf die indizierten Daten ausgeführt werden. Führen Sie die folgenden Schritte aus, um Solr in einem HDInsight Cluster verwenden:

1. **Verwenden (Remotedesktopprotokoll) zu remote zum HDInsight Cluster mit Solr installiert**. Aktivieren Sie den Remotedesktop aus dem Azure-Portal für den Cluster, die mit Solr installiert und dann Remote zum Cluster erstellt haben. Anweisungen finden Sie unter [Verbinden auf HDInsight Cluster RDP verwenden](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).

2. **Index Solr durch Hochladen Datendateien**. Wenn Sie Solr indizieren, setzen Sie Dokumente darin, die Sie suchen möchten. Zum Indizieren Solr verwenden Sie RDP zu remote zum Cluster, navigieren Sie zu den Desktop, öffnen Sie die Befehlszeile Hadoop, und navigieren Sie zu **C:\apps\dist\solr-4.7.2\example\exampledocs**. Führen Sie den folgenden Befehl ein:

        java -jar post.jar solr.xml monitor.xml

    Klicken Sie auf die Verwaltungskonsole wird die folgende Ausgabe angezeigt:

        POSTing file solr.xml
        POSTing file monitor.xml
        2 files indexed.
        COMMITting Solr index changes to http://localhost:8983/solr/update..
        Time spent: 0:00:01.624

    Das Programm post.jar indiziert Solr mit zwei Stichproben Dokumente, **solr.xml** und **monitor.xml**. Das Programm post.jar und der Stichprobe Dokumente sind mit Solr Installation verfügbar.

3. **Verwenden der Solr-Dashboard, um in den indizierten Dokumenten suchen**. In der RDP-Sitzung zum Cluster HDInsight, öffnen Sie Internet Explorer, und starten Sie das Dashboard Solr am **Http://headnodehost:8983/Solr / #/**. Im linken Bereich aus der Dropdownliste **Core Ansichtsauswahl** wählen Sie **collection1 aus**und innerhalb dieses, klicken Sie auf **Abfrage**. Bieten Sie beispielhaft um auszuwählen, und die alle Dokumente in Solr zurück, die folgenden Werte ein:

    * Geben Sie in das Textfeld **f** ** \*:**\*. Dadurch wird die Dokumente aus, die indiziert sind in Solr zurückgegeben. Wenn Sie nach einer bestimmten Zeichenfolge innerhalb der Dokumente suchen möchten, können Sie diese Zeichenfolge hier eingeben.
    
    * Wählen Sie in das Textfeld **wt** das Ausgabeformat aus. Standardmäßig ist **Json**. Klicken Sie auf die **Abfrage auszuführen**.

    ![Verwenden Sie Skript für Aktion in einem Cluster anpassen] (./media/hdinsight-hadoop-solr-install/hdi-solr-dashboard-query.png "Ausführen einer Abfrage auf Solr dashboard")
    
    Die Ausgabe gibt die zwei Dokumente, die wir für die Indizierung Solr verwendet. Die Ausgabe sieht folgendermaßen aus:

            "response": {
                "numFound": 2,
                "start": 0,
                "maxScore": 1,
                "docs": [
                  {
                    "id": "SOLR1000",
                    "name": "Solr, the Enterprise Search Server",
                    "manu": "Apache Software Foundation",
                    "cat": [
                      "software",
                      "search"
                    ],
                    "features": [
                      "Advanced Full-Text Search Capabilities using Lucene",
                      "Optimized for High Volume Web Traffic",
                      "Standards Based Open Interfaces - XML and HTTP",
                      "Comprehensive HTML Administration Interfaces",
                      "Scalability - Efficient Replication to other Solr Search Servers",
                      "Flexible and Adaptable with XML configuration and Schema",
                      "Good unicode support: héllo (hello with an accent over the e)"
                    ],
                    "price": 0,
                    "price_c": "0,USD",
                    "popularity": 10,
                    "inStock": true,
                    "incubationdate_dt": "2006-01-17T00:00:00Z",
                    "_version_": 1486960636996878300
                  },
                  {
                    "id": "3007WFP",
                    "name": "Dell Widescreen UltraSharp 3007WFP",
                    "manu": "Dell, Inc.",
                    "manu_id_s": "dell",
                    "cat": [
                      "electronics and computer1"
                    ],
                    "features": [
                      "30\" TFT active matrix LCD, 2560 x 1600, .25mm dot pitch, 700:1 contrast"
                    ],
                    "includes": "USB cable",
                    "weight": 401.6,
                    "price": 2199,
                    "price_c": "2199,USD",
                    "popularity": 6,
                    "inStock": true,
                    "store": "43.17614,-90.57341",
                    "_version_": 1486960637584081000
                  }
                ]
              }


4. **Empfohlen: Sichern Sie indizierten Daten aus Solr Azure Blob-Speicher zugeordnet HDInsight Cluster**. Eine empfiehlt sich jedoch sollten Sie die indizierten Daten aus dem Solr Clusterknoten auf Azure Blob-Speicher sichern. Führen Sie hierzu die folgenden Schritte:

    1. Die RDP-Sitzung öffnen Sie Internet Explorer, und zeigen Sie auf den folgenden URL:

            http://localhost:8983/solr/replication?command=backup

        Sie sollten eine Antwort wie folgt angezeigt werden:

            <?xml version="1.0" encoding="UTF-8"?>
            <response>
              <lst name="responseHeader">
                <int name="status">0</int>
                <int name="QTime">9</int>
              </lst>
              <str name="status">OK</str>
            </response>

    2. Navigieren Sie in der remote-Sitzung zu {SOLR_HOME}\{Auflistung} \data. Für den Cluster über das Beispielskript erstellt haben sollte das **C:\apps\dist\solr-4.7.2\example\solr\collection1\data**sein. An dieser Stelle auftreten einen Momentaufnahme Ordner mit einem Namen ähnlich wie *erstellt *Momentaufnahme.* Timestamp***.

    3. ZIP-Snapshot-Ordner, und klicken Sie auf Azure Blob-Speicher hochladen können. Navigieren Sie über die Befehlszeile Hadoop zum Speicherort des Ordners Momentaufnahme mit den folgenden Befehl aus:

              hadoop fs -CopyFromLocal snapshot._timestamp_.zip /example/data

        Dieser Befehl kopiert den Snapshot zu /example/data/unter dem Container innerhalb der standardmäßigen Speicherkonto Cluster zugeordnet.

## <a name="install-solr-using-aure-powershell"></a>Installieren Sie mithilfe der Aure PowerShell Solr

Finden Sie unter [Anpassen HDInsight Cluster mithilfe der Aktion Skript](hdinsight-hadoop-customize-cluster.md#call_scripts_using_powershell).  Im Beispiel veranschaulicht, wie mithilfe der PowerShell Azure Spark installieren. Sie müssen Sie das Skript zum Verwenden von [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1)anpassen.

## <a name="install-solr-using-net-sdk"></a>Verwenden von .NET SDK Solr installieren

Finden Sie unter [Anpassen HDInsight Cluster mithilfe der Aktion Skript](hdinsight-hadoop-customize-cluster.md#call_scripts_using_azure_powershell). Im Beispiel veranschaulicht, wie mithilfe des .NET SDK Spark installieren. Sie müssen Sie das Skript zum Verwenden von [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1)anpassen.



## <a name="see-also"></a>Siehe auch

- [Installieren und Verwenden von Solr auf HDinsight Hadoop Cluster (Linux)](hdinsight-hadoop-solr-install-linux.md)
- [Hadoop erstellen Cluster in HDInsight](hdinsight-provision-clusters.md): Allgemeine Informationen zum Erstellen von HDInsight Cluster.
- [Anpassen von HDInsight Cluster mithilfe der Aktion Skript][hdinsight-cluster-customize]: Allgemeine Informationen zum Anpassen von HDInsight Cluster mithilfe der Aktion Skript.
- [Skript für Aktion entwickeln Skripts für HDInsight](hdinsight-hadoop-script-actions.md).
- [Installieren und Verwenden von Spark auf HDInsight Cluster][hdinsight-install-spark]: Aktion Skript Beispiel zum Installieren von Spark.
- [Installieren von R auf HDInsight Cluster][hdinsight-install-r]: Aktion Skript Beispiel zum Installieren von R.
- [Installieren von Giraph auf HDInsight Cluster](hdinsight-hadoop-giraph-install.md): Aktion Skript Stichprobe Informationen zur Installation von Giraph.


[powershell-install-configure]: powershell-install-configure.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md

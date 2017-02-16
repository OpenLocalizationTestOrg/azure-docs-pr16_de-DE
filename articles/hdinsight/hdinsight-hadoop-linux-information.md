<properties
   pageTitle="Tipps zur Verwendung von Hadoop auf Linux-basierten HDInsight | Microsoft Azure"
   description="Hier erhalten Sie Implementierung Tipps zur Verwendung von Linux-basierten HDInsight (Hadoop) Cluster auf einer vertrauten Linux-Umgebung, in der Cloud Azure ausgeführt."
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
   ms.date="09/13/2016"
   ms.author="larryfr"/>

# <a name="information-about-using-hdinsight-on-linux"></a>Informationen zur Verwendung von HDInsight unter Linux

Linux-basierten Azure HDInsight Cluster bieten Hadoop auf vertrauten Linux-Umgebung, in der Cloud Azure ausgeführt. Für die meisten Elemente sollten sie genau wie alle anderen Hadoop auf Linux Installation verwendet werden. In diesem Dokument wird, bestimmte Unterschiede, denen Sie kennen sollten.

##<a name="prerequisites"></a>Erforderliche Komponenten

Viele der Schritte in diesem Dokument verwenden Sie die folgenden Dienstprogramme, die auf Ihrem System installiert werden müssen.

* [Aufrollen](https://curl.haxx.se/) - Kommunikation mit webbasierten Diensten verwendet
* [Jq](https://stedolan.github.io/jq/) - verwendet, um das JSON-Dokumente zu analysieren
* [Azure CLI](../xplat-cli-install.md) - verwendet Remote Azure Services verwalten

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell-and-cli.md)] 

## <a name="domain-names"></a>Domänennamen

Der vollqualifizierten Domänennamen (FULLY) verwenden Sie beim Herstellen einer Verbindung mit dem Cluster aus dem Internet ist ** &lt;Clustername >. azurehdinsight.net** oder (für nur SSH) ** &lt;Clustername-ssh >. azurehdinsight.net**.

Jeder Knoten im Cluster weist intern, einen Namen, der während der Clusterkonfiguration zugeordnet ist. Um die Clusternamen gefunden haben, können Sie finden Sie auf der Seite __' Hosts '__ auf der Web-Benutzeroberfläche Ambari oder anhand der folgenden eine Liste von Hosts die Ambari REST-API zurückgeben:

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/hosts" | jq '.items[].Hosts.host_name'

Ersetzen Sie mit dem Kennwort des Admin-Konto und __CLUSTERNAME__ mit dem Namen der Cluster __Kennwort__ ein. Dadurch wird ein JSON-Dokument, eine Liste der Hosts im Cluster enthält, zurückgegeben, und klicken Sie dann Jq hinein verschoben wird die `host_name` Wert für jeden Host im Cluster des Elements.

Wenn Sie den Namen des Knotens für einen bestimmten Dienst ermitteln müssen, können Sie für diese Komponente Ambari Abfragen. Verwenden Sie Folgendes ein, um die Hosts für den HDFS Namensknoten finden Sie.

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/NAMENODE" | jq '.host_components[].HostRoles.host_name'

Dies gibt ein, die den Dienst beschreiben JSON-Dokument, und klicken Sie dann Jq hinein verschoben wird nur die `host_name` Wert für die Hosts.

## <a name="remote-access-to-services"></a>Remotezugriff auf Dienste

* **Ambari (Web)** - https://&lt;Clustername >. azurehdinsight.net

    Authentifizierung über die Cluster Administrator-Benutzer und das Kennwort ein, und melden Sie sich Ambari. Dies wird auch der Cluster Administratorbenutzer und das Kennwort verwendet.

    Ist nur-Text-Authentifizierung – immer HTTPS verwenden, um sicherzustellen, dass die Verbindung sicher ist.

    > [AZURE.IMPORTANT] Während der Ambari für Ihren Cluster direkt über das Internet zugänglich ist, beruht einige Funktionen zum Zugriff auf Knoten durch den internen Domänennamen vom Cluster verwendet. Da dies eine internen Domänennamen ist und nicht öffentlich, Sie Fehler "Server nicht gefunden erhalten" bei dem Versuch, einige Features über das Internet zugreifen.
    >
    > Wenn die vollständige Funktionalität von der Ambari Web-Benutzeroberfläche verwenden möchten, verwenden Sie einen Tunnel SSH Proxy Web Datenverkehr an am Cluster-Knoten. Finden Sie unter [Verwenden SSH Tunnel Zugriff auf Ambari Web UI, Ressourcen-Manager, JobHistory, NameNode, Oozie, und andere Elemente Benutzeroberfläches von web](hdinsight-linux-ambari-ssh-tunnel.md)

* **Ambari (REST)** – https://&lt;Clustername >.azurehdinsight.net/ambari

    > [AZURE.NOTE] Authentifizierung über die Cluster Administratorbenutzer und Ihr Kennwort ein.
    >
    > Ist nur-Text-Authentifizierung – immer HTTPS verwenden, um sicherzustellen, dass die Verbindung sicher ist.

* **WebHCat (Templeton)** – https://&lt;Clustername >.azurehdinsight.net/templeton

    > [AZURE.NOTE] Authentifizierung über die Cluster Administratorbenutzer und Ihr Kennwort ein.
    >
    > Ist nur-Text-Authentifizierung – immer HTTPS verwenden, um sicherzustellen, dass die Verbindung sicher ist.

* **SSH** - &lt;Clustername >-ssh.azurehdinsight.net auf Port 22 oder 23. Anschluss 22 wird verwendet, um die Verbindung mit der primären Headnode während 23 Verbindung zu den sekundären verwendet wird. Weitere Informationen zu den am Knoten finden Sie unter [Verfügbarkeit und Zuverlässigkeit der Hadoop Cluster in HDInsight](hdinsight-high-availability-linux.md).

    > [AZURE.NOTE] Sie können nur die am Cluster-Knoten über SSH von einem Clientcomputer zugreifen. Nachdem die Verbindung hergestellt wurde, können Sie dann die Worker-Knoten mithilfe von SSH aus einer Headnode zugreifen.

## <a name="file-locations"></a>Dateispeicherorte

Hadoop-bezogene Dateien finden Sie auf den Clusterknoten bei `/usr/hdp`. Dieses Verzeichnis enthält die folgenden Unterverzeichnisse:

* __2.2.4.9-1__: Dieses Verzeichnis ist den Namen für die Version der Hortonworks Datenplattform HDInsight, indem Sie verwendet werden, sodass der Seitenzahl auf Ihren Cluster andere als die hier aufgeführten sein kann.
* __aktuelle__: Dieses Verzeichnis enthält Links zu Verzeichnisse unter dem Verzeichnis __2.2.4.9-1__ und vorhanden, sodass Sie besitzen, geben Sie eine Versionsnummer (die ändern möglicherweise) jedes Mal, wenn Sie eine Datei zugreifen möchten.

Beispieldaten und JAR-Dateien finden Sie auf Hadoop Distributed Datei System (HDFS) oder Azure BLOB-Speicher mit ' / Beispiel ' oder ' Wasbs: / / / Beispiel '.

## <a name="hdfs-azure-blob-storage-and-storage-best-practices"></a>HDFS, bewährte Methoden zum Speichern und Azure Blob-Speicher

In den meisten Hadoop wird HDFS durch lokale Speicher auf den Computern im Cluster unterstützt. Während sich effizient handelt, kann es für eine Cloud-basierte Lösung teure, wo Sie stündlich oder Minute für Ressourcen berechnen unterliegen.

HDInsight verwendet Azure Blob-Speicher als Standardinformationsspeicher, die bietet folgende Vorteile:

* Effizient langfristiges Speicher

* Eingabehilfen von externen Diensten wie Websites, Datei Upload/Download Dienstprogramme, SDKs für verschiedene Sprachen und Webbrowser

Da es sich um den Standardspeicher für HDInsight ist, müssen Sie normalerweise nichts Unternehmen, um es zu verwenden. Beispielsweise werden mit dem folgende Befehl Dateien im Ordner **/example/data** aufgelistet, bei denen auf Azure Blob Storage gespeichert ist:

    hdfs dfs -ls /example/data

Einige Befehle möglicherweise müssen Sie angeben, dass Sie Blob-Speicher verwenden. Für diese, können Sie den Befehl mit Präfix **Wasb: / /**, oder **Wasbs: / /**.

HDInsight können Sie mehrere Blob-Speicher-Konten mit einem Cluster zu verbinden. Wenn Sie Daten auf einer nicht standardmäßigen Blob-Speicher-Konto zugreifen zu können, verwenden Sie das Format **Wasbs: / /&lt;container-name>@&lt;Kontonamen >.blob.core.windows.net/**. Beispielsweise wird vor den Inhalt der **/example/data** Verzeichnis für die angegebenen Container und BLOB-Speicherkonto auflisten:

    hdfs dfs -ls wasbs://mycontainer@mystorage.blob.core.windows.net/example/data

### <a name="what-blob-storage-is-the-cluster-using"></a>Welche Blob-Speicher ist der Cluster verwenden?

Während der Clustererstellung, die Sie entweder verwenden Sie eine vorhandene Speicher Azure-Konto und Container, oder erstellen Sie einen neuen markiert haben. Klicken Sie dann vergessen Sie wahrscheinlich zu erhalten. Sie können den Speicher Standardkonto und den Container mithilfe der REST-API Ambari suchen.

1. Verwenden Sie den folgenden Befehl zum Abrufen von HDFS Konfigurationsinformationen Curl verwenden und mithilfe von [Jq](https://stedolan.github.io/jq/)filtern:

        curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["fs.defaultFS"] | select(. != null)'
    
    > [AZURE.NOTE] Dadurch wird die erste Konfiguration angewendet werden, auf dem Server zurückgegeben (`service_config_version=1`,) die werden diese Informationen enthalten. Wenn Sie einen Wert abrufen, der nach der Clustererstellung geändert wurde, müssen Sie die Liste der Konfigurationsversionen und die neuesten eine abzurufen.

    Dadurch wird einen Wert ähnlich wie der folgende, zurückgegeben, wobei __CONTAINER__ der standardmäßige Container und __Kontoname__ ist der Azure-Speicher Kontoname:

        wasbs://CONTAINER@ACCOUNTNAME.blob.core.windows.net

1. Erhalten Sie die Ressourcengruppe für das Konto Speicher möchten, verwenden Sie die [Azure CLI](../xplat-cli-install.md). Ersetzen Sie in den folgenden Befehl aus __Kontoname__ mit dem Speicher Kontonamen aus Ambari abgerufen werden:

        azure storage account list --json | jq '.[] | select(.name=="ACCOUNTNAME").resourceGroup'
    
    Dadurch wird die Gruppe Ressourcenname für das Konto zurückgegeben.
    
    > [AZURE.NOTE] Wenn nichts aus dieser Befehl zurückgegeben wird, müssen Sie die Azure CLI in Azure Ressourcenmanager Modus ändern, und führen den Befehl erneut. Um zur Azure Ressourcenmanager Modus wechseln, verwenden Sie den folgenden Befehl ein.
    >
    > `azure config mode arm`
    
2. Erhalten Sie die Taste für das Speicher-Konto an. Ersetzen Sie die Ressourcengruppe aus dem vorherigen Schritt __GRUPPENNAME__ ein. Ersetzen Sie mit dem Speicher Kontonamen __Kontoname__ :

        azure storage account keys list -g GROUPNAME ACCOUNTNAME --json | jq '.[0].value'

    Dadurch wird den Primärschlüssel für das Konto zurückgegeben.

Sie können auch die Speicherinformationen über das Azure-Portal finden:

1. Wählen Sie im [Portal Azure](https://portal.azure.com/)-HDInsight Cluster ein.

2. Wählen Sie im Abschnitt __Essentials__ __Alle Einstellungen__aus.

3. Wählen Sie __Einstellungen__aus __Azure-Speicher-Taste__.

4. Aus __Azure Speicher Tasten__, wählen Sie eine der aufgeführten Speicherkonten. Informationen über das Speicherkonto werden angezeigt.

5. Wählen Sie das Symbol Key aus. Tasten für dieses Speicherkonto werden angezeigt.

### <a name="how-do-i-access-blob-storage"></a>Wie greife ich Blob-Speicher?

Mithilfe des Befehls Hadoop aus dem Cluster werden als auf vielfältige Weise Blobs Zugriff auf:

* [Azure CLI für Mac, Linux und Windows](../xplat-cli-install.md): Line Benutzeroberfläche für die Arbeit mit Azure-Befehle. Verwenden Sie nach der Installation der `azure storage` Hilfe zur Verwendung von Speicher, Befehl oder `azure blob` für Blob-spezifischen Befehlen.

* [blobxfer.py](https://github.com/Azure/azure-batch-samples/tree/master/Python/Storage): eine Skripts für die Arbeit mit Blobs in Azure-Speicher.

* Eine Vielzahl von SDKs:

    * [Java](https://github.com/Azure/azure-sdk-for-java)

    * [Node.js](https://github.com/Azure/azure-sdk-for-node)

    * [PHP](https://github.com/Azure/azure-sdk-for-php)

    * [Python](https://github.com/Azure/azure-sdk-for-python)

    * [Ruby](https://github.com/Azure/azure-sdk-for-ruby)

    * [.NET](https://github.com/Azure/azure-sdk-for-net)

* [Speicher REST-API](https://msdn.microsoft.com/library/azure/dd135733.aspx)

##<a name="a-namescalingascaling-your-cluster"></a><a name="scaling"></a>Skalierung des Clusters

Cluster Skalierung Feature ermöglicht Ihnen, ändern die Anzahl der Datenknoten von einem Cluster, der in Azure HDInsight ausgeführt wird, ohne zu löschen und neu erstellen Cluster verwendet.

Können Sie Anpassungsbereich für Vorgänge, während andere Aufträge durchführen oder auf einem Cluster Prozesse ausgeführt werden.

Die anderen Cluster sind betroffen durch Skalierung wie folgt:

* __Hadoop__: Wenn die Anzahl von Knoten in einem Cluster nach unten zu skalieren, werden einige der Dienste im Cluster neu gestartet. Dies kann Aufträge ausgeführt wird oder aussteht verursachen, nach Abschluss des Vorgangs Skalierung fehlschlägt. Sie können die Aufträge erneut, nachdem der Vorgang abgeschlossen ist.

* __HBase__: innerhalb weniger Minuten nach Abschluss des Vorgangs Skalierung sind automatisch Landes-/ Server ausgelastet. Um Landes-/ Servern zu verteilen, gehen Sie folgendermaßen vor:

    1. Verbinden Sie mit dem HDInsight Cluster SSH verwenden. Weitere Informationen zum Verwenden von SSH mit HDInsight finden Sie die folgenden Dokumente:

        * [Verwenden von SSH mit HDInsight Linux, Unix und Mac OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

        * [Verwenden von SSH mit HDInsight von Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

    1. Starten Sie die Verwaltungsshell HBase anhand der folgenden:

            hbase shell

    2. Nachdem die Verwaltungsshell HBase geladen, die regionalen Server manuell Saldo anhand der folgenden:

            balancer

* __Storm__: Sie sollten neu zu verteilen alle laufenden Storm Topologien nach ein Skalierung Vorgang durchgeführt wurde. Dadurch wird die Suchtopologie zu passen Sie diese Parallelism Einstellungen basierend auf der neuen Anzahl von Knoten im Cluster. Verwenden Sie zum laufende Topologien neu zu verteilen, eine der folgenden Optionen aus:

    * __SSH__: eine Verbindung mit dem Server, und verwenden Sie den folgenden Befehl aus, um ein Suchtopologie neu zu verteilen:

            storm rebalance TOPOLOGYNAME

        Sie können auch Parameter, um die ursprünglich von der Suchtopologie bereitgestellten Parallelism Hinweise Überschreiben angeben. Beispielsweise `storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10` wird der Suchtopologie 5 Arbeitsprozesse, 3 Executors für die Komponente Blau-Schnauze und 10 Executors für die Gelb Bolzenkomponente neu zu konfigurieren.

    * __Storm-Benutzeroberfläche__: mit den folgenden Schritten können Sie um ein Suchtopologie mithilfe der Benutzeroberfläche Storm neu zu verteilen.

        1. Öffnen Sie in Ihrem Webbrowser, wo finde ich CLUSTERNAME den Namen Ihrer Storm Cluster __https://CLUSTERNAME.azurehdinsight.net/stormui__ ein. Wenn Sie dazu aufgefordert werden, geben Sie den HDInsight Cluster Administratornamen (Admin) und das Kennwort ein, die Sie beim Erstellen des Clusters angegeben.

        3. Wählen Sie aus der Suchtopologie, die Sie neu zu verteilen möchten, und wählen Sie dann die Schaltfläche __neu zu verteilen__ . Geben Sie die Verzögerung an, bevor der Rebalance Vorgang ausgeführt wird.

Bestimmte Informationen auf Ihren Cluster HDInsight Skalierung finden Sie unter:

* [Verwalten von Hadoop Cluster in HDInsight mithilfe der Azure-Portal](hdinsight-administer-use-portal-linux.md#scaling)

* [Verwalten von Hadoop Cluster in HDinsight mithilfe von Azure PowerShell](hdinsight-administer-use-command-line.md#scaling)

## <a name="how-do-i-install-hue-or-other-hadoop-component"></a>Wie installiere ich Farbton (oder einer anderen Komponente Hadoop)?

HDInsight ist ein verwalteten-Dienst, was bedeutet, dass Knoten in einem Cluster möglicherweise gelöscht und reprovisioned automatisch von Azure, wenn ein Problem erkannt wird. Aus diesem Grund empfiehlt es sich nicht Dinge direkt auf den Cluster-Knoten manuell zu installieren. Verwenden Sie stattdessen [HDInsight Skriptaktionen](hdinsight-hadoop-customize-cluster.md) , wenn Sie Folgendes installieren müssen:

* Einem Dienst oder einer Website wie Spark oder Farbton.
* Eine Komponente, die Konfiguration der Änderungen auf mehreren Knoten im Cluster erforderlich sind. Beispielsweise eine Umgebungsvariable erforderlich-, die Protokollierung Directory oder Erstellung einer Konfigurationsdatei erstellen.

Skript-Aktionen handelt es sich um Bash Skripts, die während der Bereitstellung Cluster ausgeführt haben, und zum Installieren und konfigurieren weitere Komponenten auf dem Cluster verwendet werden können. Beispiel für Skripts werden bei der Installation von die folgenden Komponenten bereitgestellt:

* [Farbton](hdinsight-hadoop-hue-linux.md)
* [Giraph](hdinsight-hadoop-giraph-install-linux.md)
* [Solr](hdinsight-hadoop-solr-install-linux.md)

Informationen zur Entwicklung von Skript-Aktionen finden Sie unter [Aktion Skript Development mit HDInsight](hdinsight-hadoop-script-actions-linux.md).

###<a name="jar-files"></a>JAR-Dateien

Eigenständige JAR-Dateien, die als Teil eines Auftrags MapReduce oder von Funktionen enthalten sind einige Hadoop Technologien bereitgestellt werden in Schwein oder Struktur. Während diese installiert werden können mithilfe von Skript-Aktionen, können diese häufig jedem Setup nicht erforderlich und nur hochgeladen zum Cluster nach der Bereitstellung und direkt verwendet. Wenn Sie sicher, dass die Komponente aufgenommen wurde der Cluster erneutes abbilden Makle möchten, können Sie die JAR-Datei in WASB speichern.

Wenn Sie die neueste Version von [DataFu](http://datafu.incubator.apache.org/)verwenden möchten, können Sie beispielsweise einem Glas, enthält das Projekt herunterladen und zum Cluster HDInsight hochladen. Folgen Sie dann der DataFu-Dokumentation auf Einsatzbreite Schwein oder Struktur.

> [AZURE.IMPORTANT] Einige Komponenten, die eigenständigen JAR-Dateien sind mit HDInsight bereitgestellt werden, aber nicht in den Pfad. Wenn Sie für eine bestimmte Komponente gefunden werden, können folgen Sie um ihn auf Ihrem Cluster zu suchen:
>
> ```find / -name *componentname*.jar 2>/dev/null```
>
> Dadurch wird den Pfad des alle entsprechenden JAR-Dateien zurückgegeben.

Wenn der Cluster bereits eine Version einer Komponente als eigenständigen JAR-Datei enthält, aber eine andere Version verwenden möchten, können Sie eine neue Version der Komponente zum Cluster hochladen und versuchen Sie es in Ihre Projekte verwendet wird.

> [AZURE.WARNING] Komponenten, die mit dem HDInsight Cluster bereitgestellt werden vollständig unterstützt, und hilft Microsoft Support zum Isolieren und Beheben von Problemen im Zusammenhang mit dieser Komponenten.
>
> Benutzerdefinierte Komponenten empfangen professionell angemessenen Support, um Sie dabei unterstützen, das Problem zu beheben. Dadurch kann das Problem zu beheben, oder werden Sie aufgefordert, die verfügbaren Kanäle für das open-Source-Technologien populärer, wo eingehender Erfahrung für diese Technologie gefunden wird. Angenommen, es gibt viele Communitywebsites, die, wie verwendet werden können: [MSDN-Forum für HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Apache Projekte außerdem Projektwebsites auf [http://apache.org](http://apache.org), zum Beispiel: [Hadoop](http://hadoop.apache.org/), [Spark](http://spark.apache.org/).

## <a name="next-steps"></a>Nächste Schritte

* [Migrieren von Windows-basiertem HDInsight zu Linux-basierten](hdinsight-migrate-from-windows-to-linux.md)
* [Verwenden Sie die Struktur mit HDInsight](hdinsight-use-hive.md)
* [Schwein mit HDInsight verwenden](hdinsight-use-pig.md)
* [Verwenden von MapReduce Aufträge mit HDInsight](hdinsight-use-mapreduce.md)

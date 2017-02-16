<properties
pageTitle="Migrieren von Windows-basiertem HDInsight zu Linux-basierten HDInsight | Azure"
description="Informationen Sie zum Migrieren von einem Windows-basierten HDInsight Cluster zu einem Cluster Linux-basierten HDInsight."
services="hdinsight"
documentationCenter=""
authors="Blackmist"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="10/28/2016"
ms.author="larryfr"/>

#<a name="migrate-from-a-windows-based-hdinsight-cluster-to-a-linux-based-cluster"></a>Migrieren von einem Windows-basierten HDInsight Cluster zu einem Linux-basierten cluster

Während HDInsight Windows-basiertem Hadoop in der Cloud verwendet eine einfache Möglichkeit bietet, können Sie feststellen, dass Sie einen Linux-basierten Cluster zu nutzen von Tools und Technologien, die für Ihre Lösung erforderlich sind. Sind viele Merkmale in der Hadoop-Netz Linux-basierten Betriebssystemen entwickelt und einige möglicherweise nicht zur Verfügung für die Verwendung mit Windows-basierten HDInsight. Darüber hinaus wird davon ausgegangen viele Bücher, Videos und andere Schulungsmaterial, dass Sie einem Linux System bei der Arbeit mit Hadoop arbeiten.

Dieses Dokument enthält Details über die Unterschiede zwischen HDInsight unter Windows und Linux und Anleitungen zum Migrieren von vorhandenen Auslastung zu einem Linux-basierten Cluster an.

> [AZURE.NOTE] HDInsight Cluster verwenden Ubuntu langfristig Support (LTS) als das Betriebssystem, für die Knoten im Cluster. Klicken Sie auf die Version von Ubuntu verfügbar mit HDInsight, zusammen mit anderen Komponente Versionsinformationen Informationen finden Sie unter [HDInsight Komponentenversionen](hdinsight-component-versioning.md).

## <a name="migration-tasks"></a>Schritte für die Migration

Allgemeine Workflows für die Migration lautet wie folgt aus.

![Workflowdiagramm Migration](./media/hdinsight-migrate-from-windows-to-linux/workflow.png)

1.  Lesen Sie jeden Abschnitt dieses Dokuments zu verstehen, Änderungen, die bei der Migration von vorhandenen Workflows, Aufträge usw. zu einem Linux-basierten Cluster erforderlich sein können.

2.  Erstellen eines Linux-basierten Clusters als Test/Quality Assurance Umgebung an. Weitere Informationen zum Erstellen eines Linux-basierten Clusters finden Sie unter [Erstellen von Linux-basierten Cluster in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

3.  Kopieren Sie die vorhandene Aufträge, Datenquellen und Empfängern in der neuen Umgebung. Daten kopieren-Umgebung, die im Abschnitt Weitere Informationen hierzu finden Sie unter.

4.  Führen Sie die Überprüfung testen, um sicherzustellen, dass Ihre Aufträge erwartungsgemäß auf den neuen Cluster.

Nachdem Sie überprüft haben, dass alles wie erwartet funktioniert, Ausfall der Migration zu planen. Während dieser Ausfallzeiten führen Sie die folgenden Aktionen aus.

1.  Sichern Sie alle vorübergehenden Daten lokal auf dem Clusterknoten gespeichert. Beispielsweise, wenn Sie Daten direkt auf eine am Knoten gespeichert sind.

2.  Löschen Sie den Windows-basierten Cluster.

3.  Erstellen eines Linux-basierten Clusters mit der gleichen standardmäßigen Datenspeicher, den der Windows-basierten Cluster verwendet. Dadurch wird den neuen Cluster zum Fortsetzen der Arbeit mit Ihrer vorhandenen Daten.

4.  Importieren Sie eine vorübergehenden Daten, die Sie haben gesichert.

5.  Start Aufträge/Verarbeitung fort, verwenden den neuen Cluster.

### <a name="copy-data-to-the-test-environment"></a>Kopieren Sie Daten in der testumgebung

Es gibt viele Methoden, um die Daten und Aufträge kopieren, Verschieben von Dateien zu einem Testcluster jedoch die beiden in diesem Abschnitt erläutert die einfachsten Methoden zur direkt sind.

#### <a name="hdfs-dfs-copy"></a>HDFS DFS kopieren

Sie können den Befehl Hadoop HDFS direkt Kopieren von Daten aus der Speicher für Ihren vorhandenen Herstellung Cluster auf den Speicher für einen neuen Testcluster mithilfe der folgenden Schritte verwenden.

1. Suchen der Speicher Konto und Standard Container für Ihre vorhandene Cluster. Hierzu können Sie mit dem folgenden Azure PowerShell-Skript.

        $clusterName="Your existing HDInsight cluster name"
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        write-host "Storage account name: $clusterInfo.DefaultStorageAccount.split('.')[0]"
        write-host "Default container: $clusterInfo.DefaultStorageContainer"

2. Folgen Sie den Schritten in HDInsight Dokument zum Erstellen einer neuen testumgebung Cluster-basierten Linux erstellen. Beenden Sie vor dem Erstellen des Clusters, und wählen Sie stattdessen **Optionale Konfiguration**.

3. Wählen Sie aus dem Blade optionale Konfiguration **Verknüpfte Speicher-Konten**aus.

4. Wählen Sie **Hinzufügen eines Speicher-Taste**, und wenn Sie dazu aufgefordert werden, wählen Sie das Speicherkonto, das von der PowerShell-Skript in Schritt 1 zurückgegeben wurde. Klicken Sie auf **auswählen** , klicken Sie auf jedes Blade sie zu schließen. Erstellen Sie schließlich den Cluster ein.

5. Nachdem der Cluster erstellt wurde, Herstellen einer Verbindung mit daran **SSH.** Wenn Sie nicht bei der Verwendung von SSH mit HDInsight vertraut sind, finden Sie in den folgenden Artikeln.

    * [Verwenden von SSH mit Linux-basierten HDInsight von Windows-clients](hdinsight-hadoop-linux-use-ssh-windows.md)

    * [Verwenden von SSH mit Linux-basierten HDInsight von Linux, Unix und Mac-clients](hdinsight-hadoop-linux-use-ssh-unix.md)

6. Verwenden Sie den folgenden Befehl aus der Sitzung SSH Dateien aus dem Speicherkonto verknüpfte in das neue Speicher Standardkonto zu kopieren. Ersetzen von CONTAINER und Konto mit dem Container und Kontoinformationen vom Skript PowerShell in Schritt 1 zurückgegeben. Ersetzen Sie den Pfad zu Daten durch den Pfad zu einer Datendatei aus.

        hdfs dfs -cp wasbs://CONTAINER@ACCOUNT.blob.core.windows.net/path/to/old/data /path/to/new/location

    [AZURE.NOTE] Wenn das Directory-Struktur, die die Daten enthält, auf die testumgebung nicht vorhanden ist, können Sie es mit dem folgenden Befehl erstellen.

        hdfs dfs -mkdir -p /new/path/to/create

    Die `-p` wechseln ermöglicht die Erstellung einer alle Verzeichnisse im Pfad.

#### <a name="direct-copy-between-azure-storage-blobs"></a>Direkte Kopie zwischen Azure-Speicher blobs

Sie möchten Alternativ verwenden die `Start-AzureStorageBlobCopy` Azure-PowerShell-Cmdlet zum Kopieren von Blobs zwischen Speicherkonten außerhalb HDInsight. Weitere Informationen finden Sie im Abschnitt Azure Blobs mithilfe von Azure PowerShell mit Azure-Speicher verwalten.

##<a name="client-side-technologies"></a>Clientseitige Technologien

Im Allgemeinen weiterhin clientseitige Technologien wie [Azure PowerShell-Cmdlets](../powershell-install-configure.md), [Azure CLI](../xplat-cli-install.md) oder das [.NET SDK für Hadoop](https://hadoopsdk.codeplex.com/) entwickelt identisch mit Linux-basierten Cluster, wie sie von REST-APIs abhängig sind, die über die beiden Clustertypen OS übereinstimmen.

##<a name="server-side-technologies"></a>Serverseitige Technologien

Die folgende Tabelle enthält Anleitungen auf Migrieren von serverseitigen Komponenten, die bestimmte Windows sind.

| Wenn Sie diese Technologie verwenden... | Auszuführende Aktion |
| ----- | ----- |
| **PowerShell** (serverseitigen Skripts, einschließlich Skript-Aktionen, die während der Clustererstellung verwendet) | Schreiben Sie als Bash Skripts. Skript-Aktionen finden Sie unter [Anpassen Linux-basierten HDInsight mit Skript-Aktionen](hdinsight-hadoop-customize-cluster-linux.md) und [Skripts Aktion Entwicklung für Linux-basierten HDInsight](hdinsight-hadoop-script-actions-linux.md). |
| **Azure CLI** (serverseitigen Skripts) | Während die CLI Azure Linux verfügbar ist, kommt es nicht vorinstalliert HDInsight Cluster am Knoten. Wenn Sie es zum Erstellen von serverseitigen Skripts benötigen, finden Sie weitere Informationen zum Installieren auf Linux-basierten Plattformen [Azure CLI installieren](../xplat-cli-install.md) . |
| **.NET Komponenten** | .NET wird auf Linux-basierten HDInsight Cluster nicht vollständig unterstützt. Linux-basierten Storm auf HDInsight Cluster nach 10/28/2017 Support c# Storm Topologien mit SCP.NET Framework erstellt. Weitere Unterstützung für .NET wird in zukünftigen Updates hinzugefügt werden. |
| **Win32-Komponenten oder andere nur Windows-Technologie** | Anleitungen hängt davon ab, Komponente oder Technologie; Sie möglicherweise eine Version zu finden, die mit Linux kompatibel ist, oder Sie müssen eine alternative Lösung zu finden, oder Schreiben Sie diese Komponente. |

##<a name="cluster-creation"></a>Clustererstellung

Dieser Abschnitt enthält Informationen über die Unterschiede bei der Clustererstellung von.

### <a name="ssh-user"></a>SSH Benutzer

Linux-basierten HDInsight Cluster mit das Protokoll **Secure Shell (SSH)** können remote-Zugriff auf den Cluster-Knoten bereitstellen. Im Gegensatz zu Remote Desktop für Windows-basierten Cluster die meisten SSH-Clients bieten keiner Benutzerfunktionalität, aber stattdessen bietet eine Befehlszeile, die Sie Befehle im Cluster ausführen können. Einige Clients (z. B. [MobaXterm](http://mobaxterm.mobatek.net/),) bieten einen grafisch Datei Systembrowser sowie eine Befehlszeile Remote an.

Während der Clustererstellung müssen Sie einen Benutzer SSH und ein **Kennwort** oder **Zertifikat für öffentliche Schlüssel** für die Authentifizierung bereitstellen.

Es empfiehlt sich mit Zertifikat für öffentliche Schlüssel, wie es sicherer als mit einem Kennwort ist. Zertifikatauthentifizierung funktioniert, wenn Sie eine signierten öffentlichen und privaten Schlüssel generieren und dann öffentlichen Key angeben, wenn Cluster erstellen. Bei der Verbindung mit dem Server über SSH stellt der private Schlüssel auf dem Client Authentifizierung für die Verbindung aus.

Weitere Informationen zum Verwenden von SSH mit HDInsight finden Sie unter:

- [Verwenden von SSH mit HDInsight von Windows-clients](hdinsight-hadoop-linux-use-ssh-windows.md)

- [Verwenden von SSH mit HDInsight von Linux, Unix und OS X-clients](hdinsight-hadoop-linux-use-ssh-unix.md)

### <a name="cluster-customization"></a>Cluster Anpassung

**Skript-Aktionen** zusammen mit Linux-basierten Cluster muss in Bash Skript geschrieben werden. Obwohl Skriptaktionen während der Clustererstellung verwendet werden kann, können für Linux-basierten Cluster diese auch sein verwendeten Anpassung ausführen, nachdem ein Cluster von ist und ausgeführt. Weitere Informationen finden Sie unter [Anpassen Linux-basierten HDInsight mit Skript-Aktionen](hdinsight-hadoop-customize-cluster-linux.md) und [Skripts Aktion Entwicklung für Linux-basierten HDInsight](hdinsight-hadoop-script-actions-linux.md).

Eine weitere Anpassung-Funktion ist **bootstrap**. Für Windows-Cluster können Sie den Speicherort der zusätzliche Bibliotheken zur Verwendung mit Struktur angeben. Nach der Clustererstellung, stehen diese Bibliotheken automatisch zur Verfügung, für die Verwendung mit Struktur Abfragen ohne verwenden müssen `ADD JAR`.

Bootstrap für Linux-basierten Cluster bietet dieses Feature nicht. Verwenden Sie stattdessen in [Bibliotheken Struktur hinzufügen, während der Clustererstellung](hdinsight-hadoop-add-hive-libraries.md)dokumentierten Skriptaktion aus.

### <a name="virtual-networks"></a>Virtuelle Netzwerke

Windows-basiertem HDInsight Cluster nur arbeiten mit klassischen virtuelle Netzwerke, während HDInsight Linux-basierten Cluster Ressourcenmanager virtuelle Netzwerke erfordern. Wenn Sie Ressourcen in einem klassischen virtuelle Netzwerk, denen der Cluster Linux-HDInsight verfügen um eine Verbindung herstellen muss, finden Sie unter [Verbinden mit einem klassischen virtuelle Netzwerk mit einem Ressourcenmanager virtuellen Netzwerk](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

Weitere Informationen Konfiguration Anforderungen für die Verwendung von Azure-virtuellen Netzwerken mit HDInsight finden Sie unter [-Funktionen erweitern HDInsight mithilfe eines virtuellen Netzwerks](hdinsight-extend-hadoop-virtual-network.md).

##<a name="management-and-monitoring"></a>Management und Überwachung

Viele im Web Benutzeroberflächen, die Sie möglicherweise mit Windows-basierten HDInsight, wie etwa Historie oder aus UI, verwendet haben sind über Ambari verfügbar. Die Struktur Ambari-Ansicht enthält darüber hinaus eine Möglichkeit, Struktur Abfragen mit dem Webbrowser ausführen. Der Web-Benutzeroberfläche Ambari ist auf Linux-basierten Cluster am https://CLUSTERNAME.azurehdinsight.net verfügbar.

Weitere Informationen zum Arbeiten mit Ambari finden Sie unter den folgenden Dokumenten:

- [Ambari Web](hdinsight-hadoop-manage-ambari.md)

- [Ambari REST-API](hdinsight-hadoop-manage-ambari-rest-api.md)

### <a name="ambari-alerts"></a>Ambari Benachrichtigungen

Ambari verfügt über eine Benachrichtigung System, die Sie mit dem Cluster potenzieller Probleme erkennen kann. Benachrichtigungen als rote oder gelbe Einträge in der Ambari Web-Benutzeroberfläche angezeigt, jedoch Sie auch über die REST-API abrufen können.

> [AZURE.IMPORTANT] Ambari Benachrichtigungen angeben, die es *möglicherweise* ein Problem aufgetreten, nicht vorhanden, *ist* ein Problem aufgetreten. Angenommen, erhalten Sie eine Benachrichtigung, dass HiveServer2 zugegriffen werden kann, obwohl Sie normalerweise zugreifen können.
>
> Viele Benachrichtigungen werden als Intervall-Abfragen an einem Dienst implementiert, und eine Antwort innerhalb eines bestimmten Zeitrahmens erwartet. Damit die Benachrichtigung zwangsläufig nicht, dass der Dienst ausgefallen ist, zurück nicht nur die It innerhalb des Zeitrahmens der erwarteten Ergebnisse.

Im Allgemeinen sollten Sie auswerten, ob eine Benachrichtigung für eine längere eintritt wurde weist oder Benutzerprobleme, die zuvor mit dem Cluster gemeldet wurden spiegelt bevor eine Aktion daran.

##<a name="file-system-locations"></a>Speicherorten im Dateisystem

Linux Cluster-Dateisystem ist anders als die Windows-basiertem HDInsight Cluster angeordnet. Verwenden Sie in der folgenden Tabelle, um Dateien zu finden, die häufig verwendet werden.

| Ich möchte suchen... | Es ist am... |
| ----- | ----- |
| Konfiguration | `/etc`. Beispielsweise`/etc/hadoop/conf/core-site.xml` |
| Protokolldateien | `/var/logs` |
| Hortonworks Daten Plattform (HDP) | `/usr/hdp`. Es gibt zwei Verzeichnisse ansässig hier, die die aktuelle Version von HDP ist (z. B. `2.2.9.1-1`,) und `current`. Die `current` Directory symbolische Verknüpfungen mit Dateien und Verzeichnisse im Versionsverzeichnis Zahl enthält, und ist, sofern die Zahl für den Zugriff auf Dateien HDP seit der Version auf einfache Weise geändert wird, wie die HDP Version aktualisiert wird. |
| Hadoop-streaming.jar | `/usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar` |

Wenn Sie den Namen der Datei kennen, können Sie im Allgemeinen den folgenden Befehl aus einer SSH-Sitzung verwenden, um den Dateipfad zu ermitteln:

    find / -name FILENAME 2>/dev/null

Sie können auch mit den Dateinamen Platzhalter verwenden. Beispielsweise `find / -name *streaming*.jar 2>/dev/null` zurückgegeben werden kann den Pfad auf JAR-Dateien, die das Wort'streaming' als Teil der Dateiname enthalten.

##<a name="hive-pig-and-mapreduce"></a>Struktur, Schwein und MapReduce

Schwein und MapReduce diese sind sehr ähnlich ist, klicken Sie auf Linux-basierten Cluster: ist des wichtigsten Unterschieds, wenn Sie Remotedesktop verwenden, die Verbindung zu einem Windows-basierten Cluster und Einzelvorgänge ausführen möchten, Sie SSH mit Linux-basierten Cluster verwenden.

- [Schwein mit SSH verwenden](hdinsight-hadoop-use-pig-ssh.md)

- [Verwenden von MapReduce mit SSH](hdinsight-hadoop-use-mapreduce-ssh.md)

### <a name="hive"></a>Struktur

Das folgende Diagramm enthält Richtlinien zum Migrieren der Struktur Auslastung.

| Klicken Sie auf verwenden Windows-basiertem ich... | Klicken Sie auf Linux-basierten... |
| ----- | ----- |
| **Struktur-Editor** | [Anzeigen der Struktur in Ambari](hdinsight-hadoop-use-hive-ambari-view.md) |
| `set hive.execution.engine=tez;`So aktivieren Sie Tez | Tez ist die standardmäßige Execution-Engine für Linux-basierten Cluster, damit die Set-Anweisung nicht mehr benötigt wird. |
| CMD-Dateien oder Skripts auf dem Server als Teil eines Auftrags Struktur aufgerufen | Verwenden Sie Bash Skripts |
| `hive`Befehl aus Remotedesktop | Verwenden Sie [Beeline](hdinsight-hadoop-use-hive-beeline.md) oder [von einer Sitzung SSH Struktur](hdinsight-hadoop-use-hive-ssh.md) |

##<a name="storm"></a>Storm

| Klicken Sie auf verwenden Windows-basiertem ich... | Klicken Sie auf Linux-basierten... |
| ----- | ----- |
| Storm-Dashboard | Der Storm Dashboard ist nicht verfügbar. Finden Sie unter [Bereitstellen und Verwalten von Storm Topologien auf Linux-basierten HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md) für Methoden zum Übermitteln von Topologien |
| Storm-Benutzeroberfläche | Die Benutzeroberfläche Storm ist https://CLUSTERNAME.azurehdinsight.net/stormui verfügbar |
| Visual Studio erstellen, bereitstellen und Verwalten von c# oder Hybrid Topologien | Visual Studio kann verwendet werden, zu erstellen, bereitstellen und Verwalten von c# (SCP.NET) oder Hybridtopologien auf Linux-basierten Storm auf HDInsight Cluster nach 10/28/2017 erstellt. |

##<a name="hbase"></a>HBase

Klicken Sie auf Linux-basierten Cluster Znode für HBase ist `/hbase-unsecure`. Sie müssen diese in der Konfiguration für Java-Clients Applikationen festgelegt, die systemeigene HBase Java-API verwenden.

Ein Beispiel für einen Client, der diesen Wert festlegt finden Sie unter [erstellen eine HBase Java-basierte Anwendung](hdinsight-hbase-build-java-maven.md) .

##<a name="spark"></a>Spark

Spark Cluster wurden während der Vorschau auf einem Windows-Cluster verfügbar; für Version ist Spark jedoch nur mit Linux-basierten Cluster verfügbar. Es ist keine Migrationspfad aus einem Windows-basierten Spark Vorschau Cluster zu einem Release Spark Linux-basierten Cluster.

##<a name="known-issues"></a>Bekannte Probleme

### <a name="azure-data-factory-custom-net-activities"></a>Azure Data Factory benutzerdefinierte .NET Aktivitäten

Azure Data Factory benutzerdefinierte .NET Aktivitäten werden auf HDInsight Linux-basierten Cluster derzeit nicht unterstützt. In diesem Fall sollten Sie eine der folgenden Methoden verwenden, um benutzerdefinierte Aktivitäten als Teil der Verkaufspipeline ADF implementieren.

-   Führen Sie auf Ressourcenpool Azure Stapel .NET Aktivitäten ein. Finden Sie im Abschnitt Stapel verknüpft Azure Service [Verwenden benutzerdefinierter Aktivitäten in einer Azure Data Factory Verkaufspipeline](../data-factory/data-factory-use-custom-activities.md#AzureBatch)

-   Sie können implementieren Sie die Aktivität als MapReduce Aktivitäten. Weitere Informationen finden Sie unter [MapReduce-Programmen aus Daten Factory aufzurufen](../data-factory/data-factory-map-reduce.md) .

### <a name="line-endings"></a>Ende der Zeile

Im Allgemeinen verwenden Zeilenenden auf Windows-basierten Betriebssysteme CRLF, während Linux-basierte Betriebssysteme LF verwenden. Wenn Sie erzeugen, oder, Daten mit CRLF Zeilenenden erwarten, müssen Sie möglicherweise für die Arbeit mit der LF Linie endet Hersteller oder Nutzer ändern.

Beispielsweise gibt Azure PowerShell Abfrage HDInsight auf einem Windows-basierten Cluster mit Daten mit CRLF zurück. Die gleiche Abfrage mit einem Linux-basierten Cluster gibt LF zurück. In vielen Fällen ist dies an den Datenconsumer unerheblich, jedoch vor der Migration zu einem Cluster Linux-basierten untersucht werden sollte.

Wenn Sie Skripts, die direkt auf die Linux-Cluster-Knoten (beispielsweise ein Python-Skript mit Struktur oder eines Auftrags MapReduce verwendet) ausgeführt wird verfügen, sollten Sie immer LF als das Ende der Zeile verwenden. Wenn Sie CRLF verwenden, möglicherweise Fehler angezeigt, wenn die Skripts auf einem Linux-basierten Cluster ausgeführt.

Wenn Sie wissen, dass die Skripts keine Zeichenfolgen mit eingebetteten CR Zeichen enthalten, können per Massenimport ändern Sie die Zeilenenden mithilfe einer der folgenden Methoden:

-   **Wenn Sie über Skripts, die Sie verfügen auf dem Cluster hochladen möchten**, verwenden Sie die folgenden Aussagen PowerShell, um die Zeilenenden von CRLF in LF ändern, bevor Sie das Skript zum Cluster hochladen.

        $original_file ='c:\path\to\script.py'
        $text = [IO.File]::ReadAllText($original_file) -replace "`r`n", "`n"
        [IO.File]::WriteAllText($original_file, $text)

-   **Wenn Sie über Skripts verfügen, die bereits im Speicher vom Cluster verwendet werden**, können Sie den folgenden Befehl aus einer SSH-Sitzung mit dem Linux-basierten Cluster so ändern Sie das Skript verwenden.

        hdfs dfs -get wasbs:///path/to/script.py oldscript.py
        tr -d '\r' < oldscript.py > script.py
        hdfs dfs -put -f script.py wasbs:///path/to/script.py

##<a name="next-steps"></a>Nächste Schritte

-   [Erfahren Sie, wie Linux-basierten HDInsight Cluster erstellen](hdinsight-hadoop-provision-linux-clusters.md)

-   [Verbinden Sie mit einem Linux-basierten Cluster über SSH aus einem Windows-client](hdinsight-hadoop-linux-use-ssh-windows.md)

-   [Verbinden Sie mit einem Linux-basierten Cluster über SSH von einem Linux, Mac oder Unix-client](hdinsight-hadoop-linux-use-ssh-unix.md)

-   [Verwalten von einem Linux-basierten Cluster mit Ambari](hdinsight-hadoop-manage-ambari.md)

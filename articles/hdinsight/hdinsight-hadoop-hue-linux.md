<properties
    pageTitle="Verwenden von Farbton mit Hadoop auf HDInsight Linux Cluster | Microsoft Azure"
    description="Informationen Sie zum Installieren und Verwenden von Farbton mit Hadoop Cluster auf HDInsight Linux."
    services="hdinsight"
    documentationCenter=""
    authors="nitinme"
    manager="jhubbard"
    editor="cgronlun"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/13/2016" 
    ms.author="nitinme"/>

# <a name="install-and-use-hue-on-hdinsight-hadoop-clusters"></a>Installieren und Verwenden von Farbton auf HDInsight Hadoop Cluster

Informationen Sie zum Farbton auf HDInsight Linux Cluster installieren und verwenden Tunnel, um die Anforderungen an Farbton weiterzuleiten.

## <a name="what-is-hue"></a>Was ist Farbton?

Farbton ist eine Reihe von Webanwendungen Interaktion mit einem Hadoop Cluster verwendet. Farbton können zum Durchsuchen von der Speichers einer Hadoop Cluster (wenn HDInsight Cluster WASB) zugeordnet, Struktur Aufträge und Schwein Skripts usw. ausführen. Die folgenden Komponenten sind mit Farbton-Installationen auf einem Cluster HDInsight Hadoop verfügbar.

* Bienenwachs Struktur-Editor
* Schwein
* Metastore-manager
* Oozie
* FileBrowser (der Standard-Container WASB spricht)
* Position-Browser

> [AZURE.WARNING] Komponenten, die mit dem HDInsight Cluster bereitgestellt werden vollständig unterstützt, und hilft Microsoft Support zum Isolieren und Beheben von Problemen im Zusammenhang mit dieser Komponenten.
>
> Benutzerdefinierte Komponenten empfangen professionell angemessenen Support, um Sie dabei unterstützen, das Problem zu beheben. Dadurch kann das Problem zu beheben, oder werden Sie aufgefordert, die verfügbaren Kanäle für das open-Source-Technologien populärer, wo eingehender Erfahrung für diese Technologie gefunden wird. Angenommen, es gibt viele Communitywebsites, die, wie verwendet werden können: [MSDN-Forum für HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Apache Projekte außerdem Projektwebsites auf [http://apache.org](http://apache.org), zum Beispiel: [Hadoop](http://hadoop.apache.org/).

## <a name="install-hue-using-script-actions"></a>Installieren von Farbton mit Skript-Aktionen

Die folgende Skriptaktion kann verwendet werden, Farbton in einem HDInsight Linux-basierten Cluster installieren.
https://hdiconfigactions.BLOB.Core.Windows.NET/linuxhueconfigactionv02/Install-Hue-uber-v02.sh
    
Dieser Abschnitt enthält Anweisungen zur Verwendung von das Skript beim Bereitstellen von des Clusters mithilfe der Azure-Portal an. 

> [AZURE.NOTE] Azure PowerShell, die CLI Azure, HDInsight .NET SDK oder Azure Ressourcenmanager Vorlagen können auch verwendet werden, Skriptaktionen anwenden. Sie können auch Skriptaktionen anwenden, um Cluster bereits ausgeführt. Weitere Informationen finden Sie unter [Anpassen HDInsight Cluster mit Skript-Aktionen](hdinsight-hadoop-customize-cluster-linux.md).

1. Starten Sie einen Cluster anhand der Schritte in [Bereitstellen HDInsight Cluster unter Linux](hdinsight-hadoop-provision-linux-clusters.md#portal)bereitgestellt, aber nicht abschließen Sie provisioning.

    > [AZURE.NOTE] Um Farbton auf HDInsight Cluster zu installieren, wird die empfohlene Headnode Größe mindestens A4 (8 Kernen, 14 GB Arbeitsspeicher).

2. Klicken Sie auf das Blade **Optionale Konfiguration** wählen Sie **Skript-Aktionen**aus, und geben Sie die Informationen ein, wie unten dargestellt:

    ![Bereitstellen von Skripts Aktionsparameter für Farbton] (./media/hdinsight-hadoop-hue-linux/hue_script_action.png "Bereitstellen von Skripts Aktionsparameter für Farbton")

    * __NAME__: Geben Sie einen Anzeigenamen für die Skriptaktion.
    * __Skript-URI__: https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh
    * __Kopf__: Aktivieren Sie diese Option
    * __WORKER__: Dieses Feld leer lassen.
    * __ZOOKEEPER__: Dieses Feld leer lassen.
    * __Parameter__: Dieses Feld leer lassen.

3. Am unteren Rand der **Skript-Aktionen**verwenden Sie die Schaltfläche **auswählen** zum Speichern der Konfiguration aus. Verwenden Sie schließlich die Schaltfläche **auswählen** am unteren Rand der Blade **Optionale Konfiguration** , um die optionale Konfigurationsinformationen zu speichern.

4. Fortsetzen Sie den Cluster bereitgestellt, wie unter [Bereitstellen von HDInsight Cluster unter Linux](hdinsight-hadoop-provision-linux-clusters.md#portal).

## <a name="use-hue-with-hdinsight-clusters"></a>Verwenden von Farbton mit HDInsight Cluster

SSH Tunneling ist die einzige Möglichkeit zum Farbton auf den Cluster zugreifen, nachdem er ausgeführt wird. Über SSH Tunnel ermöglicht den Datenverkehr direkt an den Headnode der Cluster wechseln, wenn Farbton ausgeführt wird. Nachdem der Cluster bereitgestellt hat, gehen Sie folgendermaßen vor, Farbton in einem HDInsight Linux Cluster verwenden.

1. Verwenden Sie die Informationen in [Verwenden SSH Tunnel Ambari Web-Benutzeroberfläche, Ressourcen-Manager, JobHistory, NameNode, Oozie, und andere Elemente Benutzeroberfläches von Web Zugriff auf](hdinsight-linux-ambari-ssh-tunnel.md) einen SSH Tunnel von Ihrem Clientsystem zum Cluster HDInsight zu erstellen, und konfigurieren Sie dann auf den Webbrowser, um den Tunnel SSH als Proxy zu verwenden.

2. Sobald Sie erstellt einen Tunnel SSH und Browser, damit Proxy-Verkehr über diese konfiguriert haben, müssen Sie den Hostnamen des primären am Knotens suchen. Sie können dies durch Herstellen einer Verbindung mit dem Cluster über SSH auf Anschluss 22 ausführen. Beispielsweise `ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net` wobei __Benutzername__ Ihr Benutzername SSH und __CLUSTERNAME__ ist der Name Ihrer Cluster.

    Weitere Informationen zur Verwendung von SSH finden Sie unter den folgenden Dokumenten:

    * [Verwenden von SSH mit Linux-basierten HDInsight von einem Linux, Unix oder Mac OS X-client](hdinsight-hadoop-linux-use-ssh-unix.md)
    * [Verwenden von SSH mit Linux-basierten HDInsight aus einem Windows-client](hdinsight-hadoop-linux-use-ssh-windows.md)

3. Nachdem die Verbindung hergestellt wurde, verwenden Sie den folgenden Befehl auf um den vollqualifizierten Domänennamen des der primären Headnode zu erhalten:

        hostname -f

    Dadurch wird einen Namen ähnlich wie der folgende zurückgegeben:

        hn0-myhdi-nfebtpfdv1nubcidphpap2eq2b.ex.internal.cloudapp.net
    
    Dies ist der Hostname des der primären Headnode, in dem die Farbton Website befindet.

2. Verwenden Sie den Browser, um das Farbton-Portal unter Http://HOSTNAME:8888 zu öffnen. Ersetzen Sie HOSTNAME durch den Namen, die, den Sie im vorherigen Schritt für Ihren Kunden.

    > [AZURE.NOTE] Bei der erstmaligen Anmeldung, werden Sie aufgefordert, ein Konto zur Anmeldung bei des Farbton Portals erstellen. Die Anmeldeinformationen, die Sie hier angeben, werden auf dem Portal eingeschränkt und beziehen sich nicht auf die Administrator- oder SSH Benutzeranmeldeinformationen, die Sie beim Bereitstellen von Cluster angegeben haben.

    ![Melden Sie sich im Portal Farbton] (./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.Login.png "Angeben von Anmeldeinformationen für Farbton-portal")

### <a name="run-a-hive-query"></a>Ausführen einer Abfrage Struktur

1. Klicken Sie auf **Abfrage-Editors**im Portal Farbton und klicken Sie dann auf die **Struktur** , um die Struktur-Editor geöffnet.

    ![Verwenden Sie die Struktur] (./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.Hive.png "Verwenden Sie die Struktur")

2. Auf der Registerkarte **unterstützen** , klicken Sie unter **Datenbank**sollte **Hivesampletable**angezeigt werden. Dies ist ein Beispiel für die Tabelle, die im Lieferumfang von alle Hadoop-Cluster auf HDInsight enthalten. Geben Sie im rechten Bereich einer Abfrage für die Stichprobe und die Ausgabe Siehe das Bildschirmfoto auf der Registerkarte **Ergebnisse** im Bereich unter, finden Sie unter.

    ![Abfrage ausführen Struktur] (./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.Hive.Query.png "Abfrage ausführen Struktur")

    Die Registerkarte **Diagramm** können Sie auch eine visuelle Darstellung des Ergebnisses angezeigt.

### <a name="browse-the-cluster-storage"></a>Navigieren Sie den Cluster-Speicher

1. Klicken Sie im Portal Farbton in der oberen rechten Ecke der Menüleiste auf **Datei-Browser** .

2. Im Dateibrowser Öffnen standardmäßig im Verzeichnis **/user/myuser** . Klicken Sie auf Schrägstrich rechts vor dem Verzeichnis des Benutzers in den Pfad zur werden im Stammordner des Containers Azure-Speicher Cluster zugeordnet zu wechseln.

    ![Verwenden von Datei-browser] (./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.File.Browser.png "Verwenden von Datei-browser")

3. Mit der rechten Maustaste auf eine Datei oder einen Ordner, um die verfügbaren Vorgänge anzuzeigen. Verwenden Sie die Schaltfläche **Hochladen** rechts zum Hochladen von Dateien in das aktuelle Verzeichnis. Verwenden Sie die Schaltfläche ' **neu** ' zum Erstellen neuer Dateien oder Verzeichnisse aus.

> [AZURE.NOTE] Farbton Dateibrowser kann nur der Inhalt des Standardcontainers zugeordnet HDInsight Cluster anzeigen. Alle zusätzlichen Speicher Konten/Container, die Sie mit dem Cluster verknüpft haben möglicherweise können mit dem Dateibrowser nicht zugreifen. Den zusätzlichen Container Cluster zugeordnet sind jedoch immer für die Struktur Einzelvorgänge zugegriffen werden. Beispielsweise, wenn Sie den Befehl eingeben `dfs -ls wasbs://newcontainer@mystore.blob.core.windows.net` im Struktur-Editor können Sie die Inhalte der sowie zusätzliche Container prüfen. In diesem Befehl ist **Newcontainer** nicht der standardmäßige Container mit einem Cluster verbunden sind.

## <a name="important-considerations"></a>Wichtige Aspekte

1. Das Skript verwendet, um Farbton installieren, wird es nur auf die primäre Headnode im Cluster installiert.

2. Während der Installation werden mehrere Hadoop-Dienste (HDFS, aus, MR2, Oozie) neu gestartet, zum Aktualisieren der Konfigurations. Nach Abschluss der Installation von Farbton das Skript kann es für andere Dienste Hadoop starten, einige dauern. Dies möglicherweise Farbton des Anfangs der Leistung. Nachdem alle Dienste starten, werden Farbton voll funktionsfähig.

3.  Farbton versteht nicht Tez Aufträge, welche aktuellen Standard für Struktur ist. Wenn Sie MapReduce als die Struktur Execution-Engine verwenden möchten, aktualisieren Sie das Skript, um den folgenden Befehl in Ihrem Skript verwenden:

        set hive.execution.engine=mr;

4.  Mit Linux Cluster können Sie ein Szenario haben, wo Ihre Dienste auf die primäre Headnode ausgeführt werden, während der Ressourcenmanager des sekundären ausgeführt werden konnte. Der Fall möglicherweise Fehler (siehe unten) Wenn Farbton mit Details Aufträge Ausführung im Cluster an. Allerdings können Sie die Details anzeigen, wenn der Job abgeschlossen ist.

    ![Farbton-Portal zurück] (./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.Error.png "Farbton-Portal zurück")

    Dies wird durch ein bekanntes Problem. Ändern Sie Ambari Problem zu umgehen, damit der aktiven Ressourcenmanager auch für die primäre Headnode ausgeführt wird.

5.  Farbton versteht WebHDFS zwar HDInsight Cluster Azure-Speicher mit `wasbs://`. Ja, wird das benutzerdefinierte Skript zusammen mit der Skriptaktion WebWasb, also ein WebHDFS-kompatibler Dienst für ein Gespräch mit WASB installiert. Ja, obwohl das Portal Farbton besagt HDFS an Orten (wie beim Verschieben der Maus über den **Datei-Browser**), sollten sie als WASB interpretiert werden.


## <a name="next-steps"></a>Nächste Schritte

- [Installieren von Giraph auf HDInsight Cluster](hdinsight-hadoop-giraph-install-linux.md). Verwenden Sie Cluster Anpassung, um Giraph auf HDInsight Hadoop Cluster zu installieren. Giraph ermöglicht es Ihnen, mit Hadoop Graph-Verarbeitung ausführen, und kann mit Azure HDInsight verwendet werden.

- [Installieren von Solr auf HDInsight Cluster](hdinsight-hadoop-solr-install-linux.md). Verwenden Sie Cluster Anpassung, um Solr auf HDInsight Hadoop Cluster zu installieren. Solr können Sie leistungsfähige Suchvorgänge auf gespeicherten Daten durchführen.

- [Installieren von R auf HDInsight Cluster](hdinsight-hadoop-r-scripts-linux.md). Verwenden Sie Cluster Anpassung, um R auf HDInsight Hadoop Cluster zu installieren. R ist ein offener Quelle Sprache und Umgebung für statistische computing. Darüber hundert integrierten statistische Funktionen und einem eigenen Programmiersprache, die kombiniert Aspekte der funktionsübergreifendes und objektorientierten Programmierung. Darüber hinaus umfangreiche grafisch bereit.

[powershell-install-configure]: install-configure-powershell-linux.md
[hdinsight-provision]: hdinsight-provision-clusters-linux.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md

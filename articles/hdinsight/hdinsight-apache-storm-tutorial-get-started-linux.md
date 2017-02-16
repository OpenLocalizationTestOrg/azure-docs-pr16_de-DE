<properties
    pageTitle="Apache Storm Lernprogramm: Erste Schritte mit Linux-basierten Storm auf HDInsight | Microsoft Azure"
    description="Erste Schritte mit big Data Analytics Apache Storm und in den Beispielen Storm Starter auf Linux-basierten HDInsight verwenden. Informationen Sie zum Verwenden von Storm zum Verarbeiten von Daten in Echtzeit."
    keywords="Apache Storm, Apache Storm Lernprogramm, big Data Analytics Storm starter"
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="java"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/12/2016"
   ms.author="larryfr"/>


# <a name="apache-storm-tutorial-get-started-with-the-storm-starter-samples-for-big-data-analytics-on-hdinsight"></a>Apache Storm Lernprogramm: Erste Schritte mit der Storm Starter Beispiele für big Data Analytics auf HDInsight

Apache Storm ist eine skalierbare, Fehlertoleranz, verteilt, in Echtzeit Berechnung System für die Verarbeitung von Daten Streams. Mit Storm auf Azure HDInsight können Sie einen cloudbasierten Storm Cluster erstellen, der big Data Analytics in Echtzeit ausführt.

> [AZURE.NOTE] Die Schritte in diesem Artikel erstellen Sie einen Linux-basierten HDInsight Cluster. Schritte zum Erstellen eines Windows-basiertem Sturms auf HDInsight Cluster, finden Sie unter [Apache Storm Lernprogramm: Erste Schritte mit der Storm Starter Stichprobe mit Daten Analytics auf HDInsight](hdinsight-apache-storm-tutorial-get-started.md)

## <a name="prerequisites"></a>Erforderliche Komponenten

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Sie müssen dieses Lernprogramm Apache Storm erfolgreich abgeschlossen mit die folgenden:

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion Azure abrufen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **Vertrautheit mit SSH und SCP**. Weitere Informationen zum Verwenden von SSH und SCP mit HDInsight finden Sie unter den folgenden:

    - **Linux, Unix oder OS X-Clients**: finden Sie unter [Verwenden SSH mit Linux-basierten Hadoop auf HDInsight von Linux, OS X oder Unix](hdinsight-hadoop-linux-use-ssh-unix.md)

    - **Windows-Clients**: finden Sie unter [Verwenden SSH mit Linux-basierten Hadoop auf HDInsight von Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

### <a name="access-control-requirements"></a>Anforderungen für Access-Steuerelement

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-a-storm-cluster"></a>Erstellen Sie einen Cluster Storm

In diesem Abschnitt erstellen Sie einen HDInsight Version 3,2 Cluster (Storm Version 0.9.3) mithilfe einer Vorlage Azure Ressourcenmanager. Informationen zu HDInsight Versionen und ihre SLAs finden Sie unter [HDInsight Komponente Versioning](hdinsight-component-versioning.md). Weitere Methoden zur Erstellung Cluster finden Sie unter [Erstellen von HDInsight Cluster](hdinsight-hadoop-provision-linux-clusters.md).

1. Klicken Sie auf die folgende Abbildung, um die Vorlage im Azure-Portal zu öffnen.         

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-storm-cluster-in-hdinsight.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>
    
    Die Vorlage befindet sich in einem öffentlichen Blob-Container *https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-storm-cluster-in-hdinsight.json*. 
   
2. Geben Sie aus dem Parameter Blade Folgendes ein:

    - **ClusterName**: Geben Sie einen Namen für den Hadoop Cluster, die Sie erstellen.
    - **Cluster-Benutzernamen und Ihr Kennwort**: der Standard-Anmeldename ist Administrator.
    - **SSH-Benutzernamen und Ihr Kennwort ein**.
    
    Bitte notieren Sie sich diese Werte an.  Sie benötigen diese später im Lernprogramm.

    > [AZURE.NOTE] SSH wird verwendet, um die Remotezugriff auf HDInsight Cluster mithilfe einer Befehlszeile. Benutzername und Kennwort ein, das Sie hier verwenden, wird bei der Verbindung mit Cluster erfolgt über SSH verwendet. Darüber hinaus muss der Benutzernamen SSH eindeutig ist, beim Erstellen eines Benutzerkontos auf allen HDInsight Clusterknoten. Die folgenden sind einige der den Kontonamen für die Verwendung von Diensten auf dem Cluster reserviert und kann nicht als Benutzername SSH verwendet werden:
    >
    > Quadratwurzel, Hdiuser, Sturm, Hbase, Ubuntu, Zookeeper, Hdfs, aus, Mapred, Hbase, Struktur, Oozie, Falcon, Sqoop, Administrator, Tez, Hcat, Hdinsight-Zookeeper.

    > Weitere Informationen zum Verwenden von SSH mit HDInsight finden Sie unter den folgenden Artikeln:

    > * [Verwenden von SSH mit Linux-basierten Hadoop auf HDInsight von Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md)
    > * [Verwenden von SSH mit Linux-basierten Hadoop auf HDInsight von Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

    
3.Klicken klicken Sie auf **OK** , um die Parameter zu speichern.

4.konfigurieren aus dem **benutzerdefinierte Bereitstellung** -Blade klicken Sie auf die Dropdown-Feld **Ressourcengruppe** , und klicken Sie dann auf **neu** , um eine neue Ressourcengruppe erstellen. Die Ressourcengruppe ist ein Container, der Cluster, das Konto abhängige Speicherplatz und andere verknüpfte Ressource gruppiert.

5 ° Klicken Sie auf die **Vertragsbedingungen**, und klicken Sie dann auf **Erstellen**.

6. Klicken Sie auf **Erstellen**. Sehen Sie eine neue Kachel mit dem Titel Submitting Bereitstellung für die Bereitstellung der Vorlage ein. Es dauert zu ungefähr 20 Minuten der Cluster und SQL-Datenbank zu erstellen.


##<a name="run-a-storm-starter-sample-on-hdinsight"></a>Ausführen einer Stichprobe Storm Starter auf HDInsight

Die Beispiele [Storm-Starter](https://github.com/apache/storm/tree/master/examples/storm-starter) sind auf dem HDInsight Cluster enthalten. In den folgenden Schritten führen Sie das Beispiel WordCount.

1. Verbinden Sie mit dem HDInsight Cluster SSH verwenden:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
        
    Wenn Sie ein Kennwort zum Sichern Ihrer SSH Benutzerkontos verwendet haben, werden Sie aufgefordert, es einzugeben. Wenn Sie einen öffentlichen Schlüssel verwendet haben, müssen Sie möglicherweise verwenden Sie die `-i` Parameter, um den passenden privaten Schlüssel anzugeben. Beispielsweise `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`.
        
    Weitere Informationen zum Verwenden von SSH mit Linux-basierten HDInsight finden Sie unter den folgenden Artikeln:
    
    * [Verwenden von SSH mit Linux-basierten Hadoop auf HDInsight von Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [Verwenden von SSH mit Linux-basierten Hadoop auf HDInsight von Windows](hdinsight-hadoop-linux-use-ssh-windows)

2. Verwenden Sie den folgenden Befehl aus, um ein Beispiel Suchtopologie zu starten:

        storm jar /usr/hdp/current/storm-client/contrib/storm-starter/storm-starter-topologies-*.jar storm.starter.WordCountTopology wordcount
        
    > [AZURE.NOTE] Die `*` Teil des Dateinamens wird verwendet, um die Versionsnummer entsprechen geändert wird, während die HDInsight aktualisiert wird.

    Dadurch wird das Beispiel WordCount Suchtopologie auf dem Cluster mit einen Anzeigenamen von 'Wordcount' gestartet. Es zufällig generiert Sätze und das Eintreten aller Wörter in der Sätze zählen.

    > [AZURE.NOTE] Wenn Suchtopologie mit dem Cluster gesendet werden, müssen Sie die JAR-Datei mit der Cluster vor der Verwendung von Kopieren der `storm` Befehl. Dies kann erreicht werden, mit der `scp` Befehl vom Client, in dem die Datei vorhanden ist. Beispielsweise`scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar`
    >
    > Im Beispiel WordCount und andere Storm Starter-Beispiele bereits auf Ihrem Cluster am enthalten sind `/usr/hdp/current/storm-client/contrib/storm-starter/`.

##<a name="monitor-the-topology"></a>Überwachen der Suchtopologie

Die Benutzeroberfläche Storm bietet eine Web-Oberfläche für die Arbeit mit Topologien ausgeführt und auf Ihren Cluster HDInsight enthalten ist.

Gehen Sie zum Überwachen der Suchtopologie mithilfe der Benutzeroberfläche Storm folgendermaßen vor:

1. Öffnen Sie einen Webbrowser zu https://CLUSTERNAME.azurehdinsight.net/stormui, darin __CLUSTERNAME__ auf den Namen der Cluster. Dadurch wird die Benutzeroberfläche Storm geöffnet.

    > [AZURE.NOTE] Wenn Sie aufgefordert werden, geben Sie einen Benutzernamen und Ihr Kennwort ein, geben Sie den Cluster-Administrator (Admin) und Ihr Kennwort ein, dass Sie, wenn verwendet erstellen Cluster.

2. Wählen Sie unter **Zusammenfassung Suchtopologie** **Wordcount** Eintrags in der Spalte **Name** aus. Weitere Informationen zu den Suchtopologie werden angezeigt.

    ![Storm-Dashboard mit Storm Starter WordCount Suchtopologie Informationen.](./media/hdinsight-apache-storm-tutorial-get-started-linux/topology-summary.png)

    Diese Seite enthält die folgende Informationen:

    * **Suchtopologie Stats** - allgemeine Informationen auf die Leistung Suchtopologie organisiert in Zeitfenster.

        > [AZURE.NOTE] Auswählen einer bestimmten Zeitfensters Änderungen des Zeitfensters Informationen in anderen Abschnitten der Seite angezeigt.

    * **Spouts** – grundlegende Informationen zu Spouts, einschließlich des letzten Fehlers zurückgegebene jedes Schnauze.

    * **Bolts** - grundlegende Informationen zu Schrauben.

    * **Suchtopologie Konfigurations** - detaillierte Informationen zur Konfiguration Suchtopologie.

    Diese Seite enthält auch Aktionen, die auf der Suchtopologie ausgeführt werden können:

    * **Aktivieren** - Lebensläufe Verarbeitung von einer Suchtopologie deaktiviert.

    * **Deaktivieren** – hält einer laufenden Suchtopologie.

    * **Neu zu verteilen** – passt die Parallelität von der Suchtopologie an. Sie sollten laufende Topologien neu zu verteilen, nachdem Sie die Anzahl der Knoten im Cluster geändert haben. Dadurch wird die Suchtopologie Parallelität für die erhöhte/geringere Anzahl der Knoten im Cluster zukommen lassen anpassen. Weitere Informationen finden Sie unter [Grundlegendes zu den Parallelismus von einem Storm Suchtopologie](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html).

    * **Beenden** - beendet ein Suchtopologie Storm nach dem angegebenen Timeout.

3. Wählen Sie auf dieser Seite einen Eintrag aus dem Abschnitt **Spouts** oder **Bolts** aus. Dadurch werden die Informationen für die ausgewählte Komponente angezeigt.

    ![Storm-Dashboard mit Informationen zum ausgewählten Komponenten.](./media/hdinsight-apache-storm-tutorial-get-started-linux/component-summary.png)

    Diese Seite zeigt die folgenden Informationen:

    * **Schnauze/herstellt Stats** - allgemeine Informationen auf die Leistung Komponente organisiert in Zeitfenster.

        > [AZURE.NOTE] Auswählen einer bestimmten Zeitfensters Änderungen des Zeitfensters Informationen in anderen Abschnitten der Seite angezeigt.

    * **Eingabe stats** (nur Bolzen) – Informationen zu Komponenten, die Daten von der herstellt verbraucht zu erzeugen.

    * **Die Ausgabe Stats** - Informationen auf Daten ausgegeben wird, indem Sie diese an.

    * **Executors** - Informationen zu Instanzen der Komponente.

    * **Fehler** : Fehler, die von dieser Komponente gefertigt.

4. Beim Anzeigen von Details zu einer Schnauze oder herstellt, wählen Sie einen Eintrag in der Spalte **Port** im Abschnitt **Executors** Details zu einer bestimmten Instanz der Komponente angezeigt.

        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: split default ["with"]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: split default ["nature"]
        2015-01-27 14:18:02 b.s.d.executor [INFO] Processing received message source: split:21, stream: default, id: {}, [snow]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: count default [snow, 747293]
        2015-01-27 14:18:02 b.s.d.executor [INFO] Processing received message source: split:21, stream: default, id: {}, [white]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: count default [white, 747293]
        2015-01-27 14:18:02 b.s.d.executor [INFO] Processing received message source: split:21, stream: default, id: {}, [seven]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: count default [seven, 1493957]

    Anhand dieser Daten können Sie sehen, dass die Word **sieben** 1493957 Zeiten aufgetreten ist. Dies ist wie oft aufgetreten sind, da diese Suchtopologie eingeleitet wurde.

##<a name="stop-the-topology"></a>Beenden der Suchtopologie

Kehren Sie zur Seite **Zusammenfassung Suchtopologie** für die Wortanzahl Suchtopologie, und wählen Sie dann die Schaltfläche " **Abbrechen** " aus dem Abschnitt **Suchtopologie Aktionen** . Wenn Sie dazu aufgefordert werden, geben Sie 10 für die Sekunden warten, bevor Sie die Suchtopologie beenden aus. Nach dem Punkt Zeitlimit wird der Suchtopologie nicht mehr angezeigt, wenn Sie im Abschnitt **Storm Benutzeroberfläche** des Dashboards besuchen.

##<a name="delete-the-cluster"></a>Löschen des Clusters

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

##<a name="a-idnextanext-steps"></a><a id="next"></a>Nächste Schritte

In diesem Lernprogramm Apache Storm haben Sie die Storm Starter verwendet, um Informationen zum Erstellen eines Sturms auf HDInsight Cluster und dem Dashboard Storm bereitstellen, überwachen und Verwalten von Storm Topologien verwenden. Als Nächstes erfahren Sie, wie [entwickeln Java-basierte Topologien Maven verwenden](hdinsight-storm-develop-java-topology.md).

Wenn Sie bereits mit entwickeln Java-basierte Topologien und zum Bereitstellen einer vorhandenen Suchtopologie mit HDInsight möchten vertraut sind, finden Sie unter [Bereitstellen und Verwalten von Apache Storm Topologien auf HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md).

Wenn Sie ein Entwickler .NET sind, können Sie erstellen, c# oder Hybrid C#- / Java Topologien mit Visual Studio. Weitere Informationen finden Sie unter [entwickeln C#-Topologien für Apache Storm auf HDInsight Hadoop-Tools für Visual Studio verwenden](hdinsight-storm-develop-csharp-visual-studio-topology.md).

Finden Sie beispielsweise Topologien mit Storm auf HDInsight, verwendet werden können, die in den folgenden Beispielen:

    * [Beispiel für Topologien für Storm auf HDInsight](hdinsight-storm-example-topology.md)

[apachestorm]: https://storm.incubator.apache.org
[stormdocs]: http://storm.incubator.apache.org/documentation/Documentation.html
[stormstarter]: https://github.com/apache/storm/tree/master/examples/storm-starter
[stormjavadocs]: https://storm.incubator.apache.org/apidocs/
[azureportal]: https://manage.windowsazure.com/
[hdinsight-provision]: hdinsight-provision-clusters.md
[preview-portal]: https://portal.azure.com/

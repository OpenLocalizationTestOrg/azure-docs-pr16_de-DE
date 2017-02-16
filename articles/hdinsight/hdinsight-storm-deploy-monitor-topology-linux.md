<properties
   pageTitle="Bereitstellen und Verwalten von Apache Storm Topologien auf Linux-basierten HDInsight | Microsoft Azure"
   description="Informationen Sie zum Bereitstellen, überwachen und Verwalten von Apache Storm Topologien mit dem Dashboard Storm auf Linux-basierten HDInsight. Verwenden Sie Hadoop-Tools für Visual Studio."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/07/2016"
   ms.author="larryfr"/>

# <a name="deploy-and-manage-apache-storm-topologies-on-linux-based-hdinsight"></a>Bereitstellen und Verwalten von Apache Storm Topologien auf Linux-basierten HDInsight

Finden Sie in diesem Dokument die Grundlagen zum Verwalten und Überwachen von Storm Topologien Linux-basierten Storm auf HDInsight Cluster ausgeführt.

> [AZURE.IMPORTANT] Die Schritte in diesem Artikel erfordern eine Linux-basierten Storm auf HDInsight Cluster. Informationen zum Bereitstellen und die Überwachung Topologien auf Windows basierende HDInsight finden Sie unter [Bereitstellen und Verwalten von Apache Storm Topologien auf Windows basierende HDInsight](hdinsight-storm-deploy-monitor-topology.md)

## <a name="prerequisites"></a>Erforderliche Komponenten

* **A Linux-basierten Storm auf HDInsight Cluster**: finden Sie unter [Erste Schritte mit Apache Storm auf HDInsight](hdinsight-apache-storm-tutorial-get-started-linux.md) schrittweise Anleitungen zum Erstellen eines Clusters

* **Vertrautheit mit SSH und SCP**: Weitere Informationen zum Verwenden von SSH und SCP mit HDInsight finden Sie hier:

    * **Linux, Unix oder OS X-Clients**: finden Sie unter [Verwenden SSH mit Linux-basierten Hadoop auf HDInsight von Linux, OS X oder Unix](hdinsight-hadoop-linux-use-ssh-unix.md)

    * **Windows-Clients**: finden Sie unter [Verwenden SSH mit Linux-basierten Hadoop auf HDInsight von Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

* **Ein SCP Client**: dies mit allen Linux, Unix und OS X Systemen vorgesehen ist. Für Windows-Clients empfehlen wir PSCP, aus dem [kitten Downloadseite](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)verfügbar ist.

## <a name="start-a-storm-topology"></a>Starten einer Suchtopologie Storm

### <a name="using-ssh-and-the-storm-command"></a>Verwenden von SSH und den Befehl Storm

1. Verwenden Sie für die Verbindung mit dem Cluster HDInsight SSH ein. Ersetzen Sie **Benutzername** der Ihr Benutzername SSH. Ersetzen Sie durch den Namen Ihrer HDInsight Cluster **CLUSTERNAME** :

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    Weitere Informationen zur Verwendung von SSH Verbindung zu Ihren Cluster HDInsight finden Sie unter den folgenden Dokumenten:
    
    * **Linux, Unix oder OS X-Clients**: finden Sie unter [Verwenden SSH mit Linux-basierten Hadoop auf HDInsight von Linux, OS X oder Unix](hdinsight-hadoop-linux-use-ssh-unix.md)
    
    * **Windows-Clients**: finden Sie unter [Verwenden SSH mit Linux-basierten Hadoop auf HDInsight von Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

2. Verwenden Sie den folgenden Befehl aus, um ein Beispiel Suchtopologie zu starten:

        storm jar /usr/hdp/current/storm-client/contrib/storm-starter/storm-starter-topologies-0.9.3.2.2.4.9-1.jar storm.starter.WordCountTopology WordCount

    Dadurch wird das Beispiel WordCount Suchtopologie im Cluster gestartet. Es zufällig generiert Sätze und das Eintreten aller Wörter in der Sätze zählen.

    > [AZURE.NOTE] Wenn Suchtopologie mit dem Cluster gesendet werden, müssen Sie die JAR-Datei mit der Cluster vor der Verwendung von Kopieren der `storm` Befehl. Dies kann erreicht werden, mit der `scp` Befehl vom Client, in dem die Datei vorhanden ist. Beispielsweise`scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar`
    >
    > Im Beispiel WordCount und andere Storm Starter-Beispiele bereits auf Ihrem Cluster am enthalten sind `/usr/hdp/current/storm-client/contrib/storm-starter/`.

### <a name="programmatically"></a>Programmgesteuert

Sie können programmgesteuert einer Suchtopologie auf Storm auf HDInsight bereitstellen, durch die Kommunikation mit dem Nimbus-Dienst in Ihrem Cluster gehostet wird. [https://github.com/Azure-Samples/hdinsight-Java-Deploy-Storm-Topology](https://github.com/Azure-Samples/hdinsight-java-deploy-storm-topology) enthält ein Beispiel für Java-Anwendung, die veranschaulicht, wie bereitstellen und einer Suchtopologie über den Nimbus-Dienst starten.

## <a name="monitor-and-manage-using-the-storm-command"></a>Überwachen und Verwalten von mit dem Befehl storm

Die `storm` Programm ermöglicht es Ihnen, arbeiten mit der Ausführung von Topologien über die Befehlszeile. Im folgenden finden eine Liste der häufig verwendete Befehle. Verwenden Sie `storm -h` für eine vollständige Liste der Befehle.

### <a name="list-topologies"></a>Liste Topologien

Verwenden Sie den folgenden Befehl aus, um alle aufzulisten ausgeführt Topologien:

    storm list
    
Dadurch wird Informationen ähnlich wie der folgende zurückgegeben:

    Topology_name        Status     Num_tasks  Num_workers  Uptime_secs
    -------------------------------------------------------------------
    WordCount            ACTIVE     29         2            263

### <a name="deactivate-and-reactivate"></a>Deaktivieren und reaktivieren

Deaktivieren einer Suchtopologie hält sie bis abgebrochen oder erneut aktiviert hat. Anhand der folgenden deaktivieren und reaktivieren möchten:

    storm Deactivate TOPOLOGYNAME
    
    storm Activate TOPOLOGYNAME

### <a name="kill-a-running-topology"></a>Beenden einer laufenden Suchtopologie

Storm Topologien, einmal gestartet wird, erhalten Sie weiterhin ausgeführt weiterspielen. Wenn ein Suchtopologie beenden möchten, verwenden Sie den folgenden Befehl aus:

    storm stop TOPOLOGYNAME

### <a name="rebalance"></a>Neu zu verteilen

Ein Suchtopologie Qualifikationsprofilen kann das System für die Parallelität von der Suchtopologie ändern. Beispielsweise wenn Sie die Größe den Cluster, um weitere Notizen hinzufügen geändert haben, Qualifikationsprofilen lässt einer laufenden Suchtopologie vornehmen der neuen Knoten verwenden.

> [AZURE.WARNING] Ein Suchtopologie zuerst Qualifikationsprofilen deaktiviert der Suchtopologie Worker gleichmäßig über den Cluster verteilt, und dann schließlich gibt der Suchtopologie auf den Status, die vor Qualifikationsprofilen aufgetreten ist. Daher müssen, wenn der Suchtopologie aktiv war, wird es wieder aktiviert. Wenn sie deaktiviert wurde, bleibt es deaktiviert.

    storm rebalance TOPOLOGYNAME

## <a name="monitor-and-manage-using-the-storm-ui"></a>Überwachen und Verwalten von mithilfe der Storm-Benutzeroberfläche

Die Benutzeroberfläche Storm bietet eine Web-Oberfläche für die Arbeit mit Topologien ausgeführt und auf Ihren Cluster HDInsight enthalten ist. Klicken Sie zum Anzeigen der Benutzeroberfläche Storm verwenden Sie einen Webbrowser __https://CLUSTERNAME.azurehdinsight.net/stormui__, öffnen, wobei __CLUSTERNAME__ den Namen Ihrer Cluster.

> [AZURE.NOTE] Wenn Sie aufgefordert werden, geben Sie einen Benutzernamen und Ihr Kennwort ein, geben Sie den Cluster-Administrator (Admin) und Ihr Kennwort ein, dass Sie, wenn verwendet erstellen Cluster.


### <a name="main-page"></a>Hauptseite

Die Hauptseite der Storm-Benutzeroberfläche bietet die folgende Informationen:

* **Zusammenfassung Cluster**: grundlegende Informationen zu den Storm Cluster.

* **Zusammenfassung Suchtopologie**: eine Liste der ausgeführten Topologien. Verwenden Sie die Links in diesem Abschnitt, um weitere Informationen zu bestimmten Topologien anzuzeigen.

* **Vorgesetzten Zusammenfassung**: Informationen zu den Storm Vorgesetzten.

* **Nimbus Konfiguration**: Nimbus Konfiguration für den Cluster.

### <a name="topology-summary"></a>Zusammenfassung Suchtopologie

Auswählen eines Links aus dem Abschnitt **Suchtopologie Zusammenfassung** zeigt die folgende Informationen zu den Suchtopologie:

* **Suchtopologie Zusammenfassung**: grundlegende Informationen zu den Suchtopologie.

* **Suchtopologie Aktionen**: Management-Aktionen, die Sie für die Suchtopologie ausführen können.

  * **Aktivieren**: Lebensläufe Verarbeitung von einer Suchtopologie deaktiviert.

  * **Deaktivieren**: einer laufenden Suchtopologie hält.

  * **Neu zu verteilen**: die Parallelität von der Suchtopologie passt. Sie sollten laufende Topologien neu zu verteilen, nachdem Sie die Anzahl der Knoten im Cluster geändert haben. Dadurch wird die Suchtopologie Parallelität für die erhöht oder verringert Anzahl der Knoten im Cluster zukommen lassen anpassen.

    Weitere Informationen finden Sie unter <a href="http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html" target="_blank">Grundlegendes zu den Parallelismus von einem Storm Suchtopologie</a>.

  * **Beenden**: beendet wird ein Suchtopologie Storm nach dem angegebenen Timeout.

* **Suchtopologie Stats**: Statistiken über die Suchtopologie. Verwenden Sie die Links in der Spalte **Fenster** , um den Zeitrahmen für die verbleibenden Einträge auf der Seite festzulegen.

* **Spouts**: die von der Suchtopologie verwendeten Spouts. Verwenden Sie die Links in diesem Abschnitt, um weitere Informationen zu bestimmten Spouts anzuzeigen.

* **Bolts**: Schrauben von der Suchtopologie verwendet. Verwenden Sie die Links in diesem Abschnitt, um weitere Informationen zu bestimmten Schrauben anzuzeigen.

* **Suchtopologie Konfiguration**: die Konfiguration von der ausgewählten Suchtopologie.

### <a name="spout-and-bolt-summary"></a>Schnauze und herstellt Zusammenfassung

In den Abschnitten **Spouts** oder **Bolts** eine Schnauze auswählen, werden die folgende Informationen über das ausgewählte Element angezeigt:

* **Komponente Zusammenfassung**: Allgemeine Informationen zur Schnauze oder herstellt.

* **Schnauze/herstellt Stats**: Statistiken zur Schnauze oder herstellt. Verwenden Sie die Links in der Spalte **Fenster** , um den Zeitrahmen für die verbleibenden Einträge auf der Seite festzulegen.

* **Eingabe stats** (nur Bolzen): Informationen über die Eingabewerte Streams verbraucht durch die herstellt.

* **Die Ausgabe Stats**: Informationen zu den Streams ausgegeben, die von diesem spout oder Bolzen.

* **Executors**: Informationen über die Instanzen der Schnauze oder herstellt. Wählen Sie den **Port** -Eintrag für einen bestimmten Executor ein Protokoll von Diagnoseinformationen gefertigt für diese Instanz anzuzeigen.

* **Fehler**: Fehlerinformationen für dieses spout oder Bolzen.

## <a name="rest-api"></a>REST-API

Die Benutzeroberfläche Storm ist auf die REST-API integriert, sodass Sie ähnliche Management und Überwachung Funktionalität mithilfe der REST-API ausführen können. Die REST-API können Sie um benutzerdefinierte Tools zur Verwaltung und Überwachung Storm Topologien zu erstellen.

Weitere Informationen finden Sie unter [Storm Benutzeroberfläche REST-API](http://storm.apache.org/releases/0.9.6/STORM-UI-REST-API.html). Die folgenden Informationen sind speziell für die REST-API mit Apache Storm auf HDInsight verwenden.

> [AZURE.IMPORTANT] Die Storm REST-API ist nicht über das Internet öffentlich verfügbar und muss mit einer SSH Tunnel zum HDInsight Cluster am Knoten zugegriffen werden. Informationen zum Erstellen und verwenden einen Tunnel SSH finden Sie unter [Verwenden SSH Tunnel Ambari Web UI, Ressourcen-Manager, JobHistory, NameNode, Oozie, und andere Elemente Benutzeroberfläches von Web Zugriff auf](hdinsight-linux-ambari-ssh-tunnel.md).

### <a name="base-uri"></a>Basis-URI

Der base-URI für die REST-API auf Linux-basierten HDInsight Cluster steht auf dem am Knoten am **Https://HEADNODEFQDN:8744/api/Version 1/**; jedoch der Domänennamen, der den am Knoten wird während der Clustererstellung generiert und ist nicht statisch.

Sie können den vollqualifizierten Domänennamen (FQDN) für den Cluster am Knoten auf verschiedene Arten finden:

* __Aus ein SSH Sitzung__: Verwenden Sie den Befehl `headnode -f` aus einer SSH-Sitzung zum Cluster.

* __Aus dem Ambari Web__: Wählen Sie oben auf der Seite __Dienste__ aus, und wählen Sie dann __Storm__. Wählen Sie auf der Registerkarte __Zusammenfassung__ __Storm Benutzeroberfläche Server__aus. Der vollqualifizierten Domänennamen des Knotens, der die Storm Benutzeroberfläche und den REST-API wird ausgeführt werden am oberen Rand der Seite.

* __Aus Ambari REST-API__: Verwenden Sie den Befehl `curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/STORM/components/STORM_UI_SERVER"` zum Abrufen von Informationen über den Knoten, die die Storm Benutzeroberfläche und den REST-API ausgeführt werden. Ersetzen Sie mit der Administratorkennworts für den Cluster __Kennwort__ ein. Ersetzen Sie __CLUSTERNAME__ durch den Clusternamen ein. In der Antwort enthält der Eintrag "Hostname" den vollqualifizierten Domänennamen des Knotens.

### <a name="authentication"></a>Authentifizierung

Anfragen an die REST-API müssen **Standardauthentifizierung**verwenden, sodass Sie die HDInsight Cluster Administratornamen und das Kennwort verwenden.

> [AZURE.NOTE] Da Standardauthentifizierung mithilfe von Klartext gesendet wird, sollten Sie **immer** verwenden HTTPS zum Sichern der Kommunikation mit dem Cluster.

### <a name="return-values"></a>Rückgabewerte

Informationen, die aus der REST-API zurückgegeben wird, kann nur innerhalb der Cluster oder virtuellen Computern auf demselben Azure virtuelle Netzwerk als Cluster verwendbar sein. Der vollqualifizierten Domänennamen (FQDN) für Zookeeper-Servern zurückgegeben wird beispielsweise nicht aus dem Internet zugänglich sein.

## <a name="next-steps"></a>Nächste Schritte

Jetzt, da Sie beherrschen zum Bereitstellen und überwachen Topologien mit dem Dashboard Storm erfahren Sie, wie [entwickeln Java-basierte Topologien Maven verwenden](hdinsight-storm-develop-java-topology.md).

Eine Liste der weitere Beispiel Topologien finden Sie unter [Beispiel Topologien für Storm auf HDInsight](hdinsight-storm-example-topology.md).

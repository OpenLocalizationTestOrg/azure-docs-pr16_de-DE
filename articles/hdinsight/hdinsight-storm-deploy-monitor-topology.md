<properties
   pageTitle="Bereitstellen und Verwalten von Apache Storm Topologien auf HDInsight | Microsoft Azure"
   description="Informationen Sie zum Bereitstellen, überwachen und Verwalten von Apache Storm Topologien mit dem Dashboard Storm auf HDInsight. Verwenden Sie Hadoop-Tools für Visual Studio."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/11/2016"
   ms.author="larryfr"/>

#<a name="deploy-and-manage-apache-storm-topologies-on-windows-based-hdinsight"></a>Bereitstellen und Verwalten von Apache Storm Topologien auf Windows basierende HDInsight

Der Storm Dashboard können Sie auf einfache Weise bereitstellen und Ausführen von Apache Storm Topologien zum Cluster HDInsight mithilfe Ihres Webbrowsers. Sie können auch das Dashboard zum Überwachen und Verwalten von laufenden Topologien verwenden. Wenn Sie Visual Studio verwenden, bieten die HDInsight Tools für Visual Studio vergleichbare Features in Visual Studio.

Das Dashboard Storm und die Storm-Features im Menü Extras HDInsight basieren auf der Storm REST-API, die verwendet werden kann, Erstellen einer eigenen Überwachung und Management-Lösungen.

> [AZURE.IMPORTANT] Die Schritte in diesem Dokument erfordern eine Windows-basiertem Storm auf HDInsight Cluster. Informationen zur Verwendung von eines Linux-basierten Clusters finden Sie unter [Bereitstellen und Verwalten von Apache Storm Topologien auf Linux-basierten HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)

##<a name="prerequisites"></a>Erforderliche Komponenten

* **Apache Storm auf HDInsight** - finden Sie unter <a href="../hdinsight-storm-getting-started/" target="_blank">Erste Schritte mit Apache Storm auf HDInsight</a> schrittweise Anleitungen zum Erstellen eines Clusters

* Für das **Dashboard Storm**: einer modernen Webbrowser, HTML5 unterstützt,

* Für **Visual Studio** - Azure SDK 2.5.1 oder höher und die HDInsight Tools für Visual Studio. Finden Sie unter <a href="../hdinsight-hadoop-visual-studio-tools-get-started/" target="_blank">Erste Schritte mit HDInsight Tools für Visual Studio</a> zu installieren und konfigurieren die HDInsight-Tools für Visual Studio.

    Eine der folgenden Versionen von Visual Studio:

    * Visual Studio 2012 mit <a href="http://www.microsoft.com/download/details.aspx?id=39305" target="_blank">4 aktualisieren</a>

    * Visual Studio 2013 mit <a href="http://www.microsoft.com/download/details.aspx?id=44921" target="_blank">4 aktualisieren</a> oder <a href="http://go.microsoft.com/fwlink/?LinkId=517284" target="_blank">Visual Studio-2013-Community</a>

    * <a href="http://visualstudio.com/downloads/visual-studio-2015-ctp-vs" target="_blank">Visual Studio 2015 CTP6</a>

    > [AZURE.NOTE] Aktuell unterstützen die HDInsight Tools für Visual Studio Storm nur auf HDInsight Clusterversion 3,2.

##<a name="storm-dashboard"></a>Storm-Dashboard

Das Storm-Dashboard ist eine Webseite anzuzeigen, die auf Ihre Storm Cluster zur Verfügung. Die URL ist **https://&lt;Clustername >.azurehdinsight.net/**, wobei **Clustername** den Namen Ihrer Storm auf HDInsight Cluster ist.

Wählen Sie oben auf dem Dashboard Storm **Suchtopologie senden**aus. Folgen Sie den Anweisungen auf der Seite zum Ausführen einer Stichprobe Suchtopologie oder zum Hochladen und Ausführen einer Suchtopologie, die Sie erstellt haben.

![der Suchtopologie-Seite senden][storm-dashboard-submit]

###<a name="storm-ui"></a>Storm-Benutzeroberfläche

Wählen Sie aus dem Dashboard Storm den **Storm Benutzeroberfläche** Link ein. Informationen zum Cluster, sowie alle laufenden Topologien werden angezeigt.

![der Storm-Benutzeroberfläche][storm-dashboard-ui]

> [AZURE.NOTE] Mit einigen Versionen von Internet Explorer können möglicherweise feststellen, dass die Benutzeroberfläche Storm nicht aktualisiert wird, nachdem Sie es zuerst besucht haben. Es kann den neuen Topologien beispielsweise nicht angezeigt, Sie auch ursprünglich eingereicht, oder es möglicherweise ein Suchtopologie als aktiv angezeigt, wenn Sie zuvor deaktiviert. Microsoft kennt dieses Problem und arbeitet an einer Lösung.

####<a name="main-page"></a>Hauptseite

Die Hauptseite der Storm-Benutzeroberfläche bietet die folgende Informationen:

* **Zusammenfassung Cluster**: grundlegende Informationen zu den Storm Cluster.

* **Zusammenfassung Suchtopologie**: eine Liste der ausgeführten Topologien. Verwenden Sie die Links in diesem Abschnitt, um weitere Informationen zu bestimmten Topologien anzuzeigen.

* **Vorgesetzten Zusammenfassung**: Informationen zu den Storm Vorgesetzten.

* **Nimbus Konfiguration**: Nimbus Konfiguration für den Cluster.

####<a name="topology-summary"></a>Zusammenfassung Suchtopologie

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

####<a name="spout-and-bolt-summary"></a>Schnauze und herstellt Zusammenfassung

In den Abschnitten **Spouts** oder **Bolts** eine Schnauze auswählen, werden die folgende Informationen über das ausgewählte Element angezeigt:

* **Komponente Zusammenfassung**: Allgemeine Informationen zur Schnauze oder herstellt.

* **Schnauze/herstellt Stats**: Statistiken zur Schnauze oder herstellt. Verwenden Sie die Links in der Spalte **Fenster** , um den Zeitrahmen für die verbleibenden Einträge auf der Seite festzulegen.

* **Eingabe stats** (nur Bolzen): Informationen über die Eingabewerte Streams verbraucht durch die herstellt.

* **Die Ausgabe Stats**: Informationen zu den Streams ausgegeben, die von diesem spout oder Bolzen.

* **Executors**: Informationen über die Instanzen der Schnauze oder herstellt. Wählen Sie den **Port** -Eintrag für einen bestimmten Executor ein Protokoll von Diagnoseinformationen gefertigt für diese Instanz anzuzeigen.

* **Fehler**: Fehlerinformationen für dieses spout oder Bolzen.

##<a name="hdinsight-tools-for-visual-studio"></a>HDInsight Tools für Visual Studio

Die HDInsight-Tools können verwendet werden, c# oder Hybrid Topologien zum Cluster Storm übermitteln. Die folgenden Schritte durch verwenden eine Stichprobe Anwendung. Informationen zum Erstellen Ihrer eigenen Topologies mithilfe der Tools HDInsight finden Sie unter [entwickeln C#-Topologien mithilfe der HDInsight Tools für Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).

Verwenden Sie die folgenden Schritte zum Bereitstellen von einer Stichprobe für Ihre Storm auf HDInsight Cluster, und klicken Sie dann anzeigen und Verwalten der Suchtopologie aus.

1. Wenn Sie die neueste Version der HDInsight Tools für Visual Studio noch nicht installiert haben, finden Sie unter <a href="../hdinsight-hadoop-visual-studio-tools-get-started/" target="_blank">Erste Schritte mit HDInsight Tools für Visual Studio</a>.

2. Öffnen Sie Visual Studio, und wählen Sie die **Datei** > **neu** > **Projekt**.

3. Erweitern Sie im Dialogfeld **Neues Projekt** **installierte** > **Vorlagen**, und wählen Sie dann **HDInsight**. Wählen Sie aus der Liste der Vorlagen **Storm Stichprobe**. Geben Sie am unteren Rand des Dialogfelds einen Namen für die Anwendung ein.

    ![Bild](./media/hdinsight-storm-deploy-monitor-topology/sample.png)

1. Klicken Sie im **Explorer Lösung**mit der rechten Maustaste in des Projekts, und wählen Sie **senden, um Storm auf HDInsight**.

    > [AZURE.NOTE] Wenn Sie dazu aufgefordert werden, geben Sie die Anmeldeinformationen für Ihr Abonnement Azure ein. Wenn Sie mehr als ein Abonnement besitzen, melden Sie sich nach dem vorkommen, die Ihre Storm auf HDInsight Cluster enthält.

2. Wählen Sie aus der Dropdownliste **Storm Cluster** Ihrer Storm auf HDInsight Cluster aus, und wählen Sie dann auf **Senden**. Sie können überwachen, ob die Übermittlung erfolgreich ist, mithilfe der im **Ausgabefenster** angezeigt.

3. Wenn der Suchtopologie übermittelt wurde, sollte die **Storm Topologien** für den Cluster angezeigt werden. Wählen Sie aus der Liste, um Informationen zu den laufenden Suchtopologie Anzeigen der Suchtopologie aus.

    ![Visual Studio monitor](./media/hdinsight-storm-deploy-monitor-topology/vsmonitor.png)

    > [AZURE.NOTE] Sie können auch **Storm Topologien** vom **Server-Explorer** anzeigen, indem Sie **Azure** > **HDInsight**, und klicken Sie dann mit der rechten Maustaste eines Sturms auf HDInsight Cluster und **Ansicht Storm Topologien**auswählen.

    Wählen Sie das Shape für den Spouts oder Schrauben, um Informationen zu diesen Komponenten anzuzeigen. Für jede ausgewählte Element wird ein neues Fenster geöffnet.
    
    > [AZURE.NOTE] Der Name des der Suchtopologie ist den Klassennamen der Suchtopologie (in diesem Fall `HelloWord`,) mit einem Zeitstempel angefügt.

4. Wählen Sie in der Ansicht **Suchtopologie Zusammenfassung** **zu beenden** , zum Beenden der Suchtopologie aus.

    > [AZURE.NOTE] Storm Topologien weiterhin ausgeführt, bis er beendet werden oder Cluster wird gelöscht.

##<a name="rest-api"></a>REST-API

Die Benutzeroberfläche Storm ist auf die REST-API integriert, sodass Sie ähnliche Management und Überwachung Funktionalität mithilfe der REST-API ausführen können. Die REST-API können Sie um benutzerdefinierte Tools zur Verwaltung und Überwachung Storm Topologien zu erstellen.

Weitere Informationen finden Sie unter [Storm Benutzeroberfläche REST-API](https://github.com/apache/storm/blob/0.9.3-branch/STORM-UI-REST-API.md). Die folgenden Informationen sind speziell für die REST-API mit Apache Storm auf HDInsight verwenden.

###<a name="base-uri"></a>Basis-URI

Ist der base-URI für die REST-API auf HDInsight Cluster **https://&lt;Clustername >.azurehdinsight.net/stormui/api/v1/**, wobei **Clustername** den Namen Ihrer Storm auf HDInsight Cluster ist.

###<a name="authentication"></a>Authentifizierung

Anfragen an die REST-API müssen **Standardauthentifizierung**verwenden, sodass Sie die HDInsight Cluster Administratornamen und das Kennwort verwenden.

> [AZURE.NOTE] Da Standardauthentifizierung mithilfe von Klartext gesendet wird, sollten Sie **immer** verwenden HTTPS zum Sichern der Kommunikation mit dem Cluster.

###<a name="return-values"></a>Rückgabewerte

Informationen, die aus der REST-API zurückgegeben wird, kann nur innerhalb der Cluster oder virtuellen Computern auf demselben Azure virtuelle Netzwerk als Cluster verwendbar sein. Der vollqualifizierten Domänennamen (FQDN) für Zookeeper-Servern zurückgegeben wird beispielsweise nicht aus dem Internet zugänglich sein.

##<a name="next-steps"></a>Nächste Schritte

Jetzt, da Sie beherrschen zum Bereitstellen und überwachen Topologien mit dem Dashboard Storm erfahren Sie, wie Sie:

* [Entwickeln Sie C#-Topologien mit den HDInsight Tools für Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md)

* [Entwickeln Sie Java-basierte Topologien mit Maven](hdinsight-storm-develop-java-topology.md)

Eine Liste der weitere Beispiel Topologien finden Sie unter [Beispiel Topologien für Storm auf HDInsight](hdinsight-storm-example-topology.md).

[hdinsight-dashboard]: ./media/hdinsight-storm-deploy-monitor-topology/dashboard-link.png
[storm-dashboard-submit]: ./media/hdinsight-storm-deploy-monitor-topology/submit.png
[storm-dashboard-ui]: ./media/hdinsight-storm-deploy-monitor-topology/storm-ui-summary.png

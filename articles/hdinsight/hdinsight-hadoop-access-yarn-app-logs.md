<properties
    pageTitle="Hadoop aus Access-Anwendung programmgesteuert protokolliert | Microsoft Azure"
    description="Access-Anwendung anmeldet programmgesteuert auf einem Cluster Hadoop in HDInsight."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian" 
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="jgao"/>

# <a name="access-yarn-application-logs-on-windows-based-hdinsight"></a>Access-aus-Anwendung protokolliert auf Windows basierende HDInsight

In diesem Thema wird erläutert, wie die Protokolle für Applikationen aus (noch einer anderen Ressource Vermittlung) zugreifen, die in einem Cluster Hadoop in Azure HDInsight abgeschlossen haben

> [AZURE.NOTE] Die Informationen in diesem Dokument gilt nur für Windows-basiertem HDInsight Cluster. Informationen zum Zugriff auf aus HDInsight Linux-basierten Cluster anmeldet, finden Sie unter [Anwendung von Access aus anmeldet Linux-basierten Hadoop auf HDInsight](hdinsight-hadoop-access-yarn-app-logs-linux.md)

### <a name="prerequisites"></a>Erforderliche Komponenten

- Ein Windows-basiertes HDInsight Cluster.  Finden Sie unter [Erstellen von Windows-basiertem Hadoop Cluster in HDInsight](hdinsight-provision-clusters.md).


## <a name="yarn-timeline-server"></a>AUS Zeitachse Server

<a href="http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html" target="_blank">Aus Zeitachse Server</a> enthält generische Informationen zu fertigen Applikationen als auch Framework-spezifische Anwendungsinformationen über zwei verschiedenen Schnittstellen an. Insbesondere:

* Speichern und Abrufen von Informationen zu Allgemeine Anwendung HDInsight Cluster wurde aktiviert mit Version 3.1.1.374 oder höher.
* Die Anwendung Framework-spezifische Informationskomponente des Servers Zeitachse ist nicht aktuell auf HDInsight Cluster zur Verfügung.


Allgemeine Informationen über die Applikationen umfasst die folgenden Arten von Daten:

* Die Anwendung-ID, einen eindeutigen Bezeichner der Anwendung
* Der Benutzer, der die Anwendung gestartet.
* Informationen zum Ausführen der Anwendungs vorgenommenen Versuche
* Die angegebenen Anwendung bei dem Versuch untersuchten Container

Diese Informationen werden auf Ihrer Cluster HDInsight von Azure Ressourcenmanager zu einem Verlauf-Speicher im Standardcontainer der Azure-Speicher Standardkonto gespeichert werden. Diese generischen Daten auf der fertigen Applikationen können über eine REST-API abgerufen werden:

    GET on https://<cluster-dns-name>.azurehdinsight.net/ws/v1/applicationhistory/apps


## <a name="yarn-applications-and-logs"></a>Applications aus und Protokolle

AUS unterstützt mehrere programming Modelle (MapReduce war einer der Teilnehmer) durch ressourcenverwaltung von der Anwendung Planung/Überwachung Entkopplung. Dies erfolgt über eine globale *Ressourcen-Manager* (RM), pro-Worker-Knoten *NodeManagers* (NMs) und pro Anwendung *ApplicationMasters* (AMs). Die Uhr pro Anwendung handelt Ressourcen (CPU, Arbeitsspeicher, Datenträger, Netzwerk) aus, für die Anwendung ausführen, mit der RM. Die RM funktioniert mit NMs gewähren Sie diese Ressourcen, die als *Container*erteilt werden. Die Uhr ist verantwortlich Fortschritts der Container der RM zugewiesen Eine Anwendung möglicherweise viele Container je nach Art der Anwendung benötigen.

Darüber hinaus jede Anwendung möglicherweise bestehen aus mehreren *Anwendung versucht* akzeptieren, um in Anwesenheit stürzt ab oder aufgrund der Verlust der Kommunikation zwischen einer Uhr Fertig stellen und eine RM. Container sind daher auf eine bestimmte Versuch der Anwendung erteilt. In einer sinnvoll ein Containers stellt den Kontext für grundlegende Arbeitseinheit ausgeführte Arbeit von einer Anwendung aus, und alle Arbeitszeit, die innerhalb des Kontexts eines Containers ist auf der Grundlage der Container zugewiesen wurde einzelnen Worker Knoten ausgeführt wird. Finden Sie unter [Konzepte aus] [ YARN-concepts] zu Referenzzwecken.

Anwendungsprotokolle (und die zugehörigen Container Protokolle) sind entscheidend, für das Debuggen problematische Hadoop Applications. AUS bietet einen übersichtliche Rahmen für erfassen, aggregieren, und Speichern von Anwendungsprotokolle mit der [Log Aggregation] [ log-aggregation] Features. Das Feature Log Aggregation macht Anwendungsprotokolle für den Zugriff auf Weise, wie sie Protokolle über alle Container auf einem Knoten Worker aggregiert und nach Beendigung der Anwendung als einen aggregierten Protokolldatei pro Worker Knoten auf Standard-Dateisystem gespeichert. Die Anwendung möglicherweise Hunderte oder Tausende von Containern verwenden, aber Protokolle für alle Container auf einem einzelnen Worker-Knoten ausgeführt werden in einer Datei, in eine Protokolldatei pro Worker Knoten von der Anwendung verwendeten resultierender immer aggregiert werden. Log Aggregation auf HDInsight Cluster standardmäßig aktiviert ist (Version 3.0 und höher), und zusammengefasster Protokolle finden Sie in der standardmäßige Container von Ihrem Cluster an folgendem Speicherort:

    wasbs:///app-logs/<user>/logs/<applicationId>

Dieses Speicherort *Benutzer* ist der Name des Benutzers, der die Anwendung gestartet, und *p* der eindeutigen Bezeichner für eine Anwendung ist wie zugewiesen durch die RM aus.

Die aggregierten Protokolle sind nicht direkt gelesen werden kann, wie sie in einer [TFile]geschrieben werden[T-file], [Binärformat] [ binary-format] vom Container indiziert. AUS bietet CLI-Tools, um diese Protokolle als nur-Text für Applikationen oder Container relevante sichern. Nur-Text, indem Sie eine der folgenden GARNS ausführen (nach dem Herstellen einer Verbindung über RDP) direkt auf den Cluster-Knoten-Befehle können Sie diese Protokolle anzeigen:

    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application>
    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application> -containerId <containerId> -nodeAddress <worker-node-address>


## <a name="yarn-resourcemanager-ui"></a>Ressourcen-Manager-Benutzeroberfläche aus

Die Ressourcen-Manager-Benutzeroberfläche aus ausgeführt wird, klicken Sie auf die Cluster Headnode und über die Azure Portals Dashboard zugegriffen werden kann: 

1. Melden Sie sich beim [Portal Azure](https://portal.azure.com/)an. 
2. Klicken Sie auf das Menü links klicken Sie auf **Durchsuchen**, klicken Sie auf **HDInsight Cluster**, klicken Sie auf einem Windows-basierten Cluster, den Sie die Anwendung-Protokolle aus zugreifen möchten.
3. Klicken Sie im oberen Menü auf **Dashboard**. Eine Seite auf einem neuen Browserfenster geöffnet, die Registerkarte bezeichnet **HDInsight Abfrage Console**wird angezeigt.
4. Klicken Sie auf **HDInsight Abfrage Console**- **Benutzeroberfläche aus**.




[YARN-timeline-server]:http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html
[log-aggregation]:http://hortonworks.com/blog/simplifying-user-logs-management-and-access-in-yarn/
[T-file]:https://issues.apache.org/jira/secure/attachment/12396286/TFile%20Specification%2020081217.pdf
[binary-format]:https://issues.apache.org/jira/browse/HADOOP-3315
[YARN-concepts]:http://hortonworks.com/blog/apache-hadoop-yarn-concepts-and-applications/

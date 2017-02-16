<properties
    pageTitle="Hadoop Cluster in HDInsight mithilfe der Ambari-API überwachen | Microsoft Azure"
    description="Verwenden Sie die Apache Ambari-APIs für das Erstellen, verwalten und Überwachung Hadoop Cluster ein. Blenden die Komplexität der Hadoop intuitive Operator Tools und APIs."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    editor="cgronlun"
    manager="jhubbard"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="jgao"/>

# <a name="monitor-hadoop-clusters-in-hdinsight-using-the-ambari-api"></a>Überwachen von Hadoop Cluster in HDInsight mithilfe der Ambari-API

Erfahren Sie, wie HDInsight Cluster mit Ambari APIs überwachen.

> [AZURE.NOTE] Die Informationen in diesem Artikel sind primär für Windows-basiertem HDInsight Cluster, die eine schreibgeschützte Version der Ambari REST-API zur Verfügung stellen. Linux-basierten Cluster finden Sie unter [Verwalten von Hadoop Cluster Ambari verwenden](hdinsight-hadoop-manage-ambari.md).

## <a name="what-is-ambari"></a>Was ist Ambari?

[Apache Ambari] [ ambari-home] für die Bereitstellung, Verwaltung und Überwachung Apache Hadoop Cluster verwendet wird. Sie enthält eine intuitive Sammlung von Tools Operator und eine umfangreiche Sammlung von APIs, die die Komplexität der Hadoop, vereinfachen den Vorgang der Cluster ausblenden. Weitere Informationen zu den APIs, finden Sie unter [Ambari-API-Referenz][ambari-api-reference]. 

HDInsight unterstützt derzeit nur das Überwachungsfeature Ambari. Ambari-API 1.0 wird von HDInsight Version 3.0 und 2.1 Cluster unterstützt. Dieser Artikel behandelt den Zugriff auf Ambari APIs auf HDInsight Version 3.1 und 2.1 Cluster. Der wesentliche Unterschied zwischen den beiden ist, dass einige Komponenten im Lieferumfang des neuen Funktionen (wie etwa den Auftrag Verlauf Server) geändert haben. 

**Erforderliche Komponenten**

Bevor Sie dieses Lernprogramm beginnen, benötigen Sie Folgendes:

- **Eine Arbeitsstationen mit Azure PowerShell**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

- (Optional) [Aufrollen] [curl]. Um ihn zu installieren, finden Sie unter [Downloads und frei Aufrollen][curl-download].

    >[AZURE.NOTE] Beim Verwenden des cURL-Befehls in Windows verwenden doppelter Anführungszeichen statt einfache Anführungszeichen für die Optionswerte ein.

- **Ein Azure HDInsight Cluster**. Anweisungen zum Cluster bereitgestellt, finden Sie unter [Erste Schritte mit HDInsight] [ hdinsight-get-started] oder [Bereitstellen von HDInsight Cluster][hdinsight-provision]. Sie benötigen die folgenden Daten ein, um anhand des Lernprogramms zu wechseln:

    Clustereigenschaft|Azure PowerShell Variablennamen|Wert|Beschreibung
    ---|---|---|---
    HDInsight Clusternamen|$clusterName||Der Name des Ihren Cluster HDInsight.
    Cluster Benutzername|$clusterUsername||Cluster-Benutzername angegeben, wenn der Cluster erstellt wurde.
    Cluster Kennwort|$clusterPassword||Cluster Benutzerkennwort.

    >[AZURE.NOTE] Ausfüllen der Werte in der Tabelle. Dies ist hilfreich beim Durcharbeiten dieses Lernprogramms wird.

## <a name="jump-start"></a>Schnellstart

Es gibt mehrere Methoden mit Ambari HDInsight Cluster überwachen.

**Verwenden von Azure PowerShell**

Im folgenden finden Sie eine Azure PowerShell-Skript zum Abrufen der MapReduce Auftrag Tracker Informationen *in einem Cluster HDInsight 3.1.*  Der wesentliche Unterschied ist, dass wir ziehen Sie diese Details auf den Dienst aus (anstelle von MapReduce).

    $clusterName = "<HDInsightClusterName>"
    $clusterUsername = "<HDInsightClusterUsername>"
    $clusterPassword = "<HDInsightClusterPassword>"

    $ambariUri = "https://$clusterName.azurehdinsight.net:443/ambari"
    $uriJobTracker = "$ambariUri/api/v1/clusters/$clusterName.azurehdinsight.net/services/yarn/components/resourcemanager"

    $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
    $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    $response = Invoke-RestMethod -Method Get -Uri $uriJobTracker -Credential $creds -OutVariable $OozieServerStatus

    $response.metrics.'yarn.queueMetrics'

Im folgenden finden eine Azure PowerShell-Skript für den Abruf des MapReduce Auftrag Tracker Informationen *in einem Cluster HDInsight 2.1*:

    $clusterName = "<HDInsightClusterName>"
    $clusterUsername = "<HDInsightClusterUsername>"
    $clusterPassword = "<HDInsightClusterPassword>"

    $ambariUri = "https://$clusterName.azurehdinsight.net:443/ambari"
    $uriJobTracker = "$ambariUri/api/v1/clusters/$clusterName.azurehdinsight.net/services/mapreduce/components/jobtracker"

    $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
    $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    $response = Invoke-RestMethod -Method Get -Uri $uriJobTracker -Credential $creds -OutVariable $OozieServerStatus

    $response.metrics.'mapred.JobTracker'

Die Ausgabe lautet:

![Jobtracker Ausgabe][img-jobtracker-output]

**Verwenden von cURL**

Im folgenden finden ein Beispiel für die erste Clusterinformationen mithilfe von cURL:

    curl -u <username>:<password> -k https://<ClusterName>.azurehdinsight.net:443/ambari/api/v1/clusters/<ClusterName>.azurehdinsight.net

Die Ausgabe lautet:

    {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/",
     "Clusters":{"cluster_name":"hdi0211v2.azurehdinsight.net","version":"2.1.3.0.432823"},
     "services"[
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/services/hdfs",
        "ServiceInfo":{"cluster_name":"hdi0211v2.azurehdinsight.net","service_name":"hdfs"}},
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/services/mapreduce",
        "ServiceInfo":{"cluster_name":"hdi0211v2.azurehdinsight.net","service_name":"mapreduce"}}],
     "hosts":[
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/hosts/headnode0",
        "Hosts":{"cluster_name":"hdi0211v2.azurehdinsight.net",
                 "host_name":"headnode0"}},
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/hosts/workernode0",
        "Hosts":{"cluster_name":"hdi0211v2.azurehdinsight.net",
                 "host_name":"headnode0.{ClusterDNS}.azurehdinsight.net"}}]}

**Für die 10/8/2014 lassen**:

Wenn Sie den Endpunkt Ambari verwenden "https://{clusterDns}.azurehdinsight.net/ambari/api/v1/clusters/{clusterDns}.azurehdinsight.net/services/{servicename}/components/{componentname}", das Feld " *Hostname* " gibt den vollqualifizierten Domänennamen (FULLY) des Knotens anstelle der Hostname. Vor der Freigabe 10/8/2014 zurückgegeben in diesem Beispiel einfach "**headnode0**". Nach der Veröffentlichung 10/8/2014, erhalten Sie den vollqualifizierten Domänennamen "**headnode0. {} ClusterDNS} .azurehdinsight .net**", wie im vorherigen Beispiel dargestellt. Diese Änderung war erforderlich, um die Szenarien zu erleichtern, in dem mehrere Clustertypen (z. B. HBase und Hadoop) in eine virtuelle Netzwerk (VNET) bereitgestellt werden kann. In diesem Fall beispielsweise, wenn HBase als Back-End-Plattform für Hadoop zu verwenden.

## <a name="ambari-monitoring-apis"></a>Ambari Überwachung APIs

Der folgenden Tabelle sind einige der am häufigsten verwendeten Ambari Überwachung API-Anrufe. Weitere Informationen über die API finden Sie unter [Ambari-API-Referenz][ambari-api-reference].

Monitor API Anruf|URI|Beschreibung
---|---|---
Erste Cluster|`/api/v1/clusters`|
Erste Clusterinformationen an.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net`|Cluster, Dienste hosts
Abrufen von Diensten|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services`|Dienste gehören: Hdfs, Mapreduce
Abrufen von Services-Informationen.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>`|
Abrufen von Servicekomponenten|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>/components`|HDFS: Namenode, datanode<br/>MapReduce: Jobtracker; tasktracker
Erste Komponente Informationen an.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>/components/<ComponentName>`|ServiceComponentInfo, Host-Komponenten, Kennzahlen
Abrufen von hosts|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts`|headnode0, workernode0
Erhalten von Host-Informationen.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>`|
Erhalten von Host-Komponenten|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>/host_components`|Namenode, Ressourcen-Manager
Erste Host Komponente Informationen an.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>/host_components/<ComponentName>`|HostRoles, Komponente, Host, Kennzahlen
Abrufen von Konfigurationen|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/configurations`|Typen von Config: Core-Website, Hdfs-Website, Mapred-Website, Struktur-Website
Erste Konfigurationsinformationen an.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/configurations?type=<ConfigType>&tag=<VersionName>`|Typen von Config: Core-Website, Hdfs-Website, Mapred-Website, Struktur-Website


##<a name="next-steps"></a>Nächste Schritte

Jetzt haben Sie gelernt Ambari Überwachung API Anrufe zu verwenden. Weitere Informationen finden Sie unter:

- [Verwenden des Portals Azure HDInsight-Cluster verwalten][hdinsight-admin-portal]
- [Mithilfe der PowerShell Azure HDInsight-Cluster verwalten][hdinsight-admin-powershell]
- [Verwalten Sie HDInsight Cluster Line Schnittstelle][hdinsight-admin-cli]
- [HDInsight Dokumentation][hdinsight-documentation]
- [Erste Schritte mit HDInsight][hdinsight-get-started]



[ambari-home]: http://ambari.apache.org/
[ambari-api-reference]: https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md

[curl]: http://curl.haxx.se
[curl-download]: http://curl.haxx.se/download.html

[microsoft-hadoop-SDK]: http://hadoopsdk.codeplex.com/wikipage?title=Ambari%20Monitoring%20Client

[powershell-install]: powershell-install-configure.md
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-admin-cli]: hdinsight-administer-use-command-line.md
[hdinsight-documentation]: /documentation/services/hdinsight/
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-provision]: hdinsight-provision-clusters.md

[img-jobtracker-output]: ./media/hdinsight-monitor-use-ambari-api/hdi.ambari.monitor.jobtracker.output.png

<properties
pageTitle="Ports, die von HDInsight | Azure"
description="Eine Liste von HDInsight ausgeführt Hadoop-Diensten verwendeten Ports."
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
ms.date="10/03/2016"
ms.author="larryfr"/>

# <a name="ports-and-uris-used-by-hdinsight"></a>Ports und URIs verwendeten HDInsight

Dieses Dokument enthält eine Liste der von Linux-basierten HDInsight Cluster ausgeführt Hadoop-Diensten verwendeten Ports. Darüber hinaus Informationen zum Verbinden mit dem Cluster über SSH Ports.

## <a name="public-ports-vs-non-public-ports"></a>Öffentliche Ports im Vergleich zu nicht öffentliche ports

Linux-basierten HDInsight Cluster und macht nur drei Ports öffentlich im Internet verfügbar. 22, 23 und 443. Diese werden verwendet, sicherer Zugriff auf den Cluster von SSH und Services, die über das sichere HTTPS-Protokoll bereitgestellt.

HDInsight wird intern, indem Sie mehrere Azure virtuellen Computern (die Knoten in Cluster) implementiert in einer Azure-virtuellen Netzwerk ausgeführt. Innerhalb des virtuellen Netzwerks, können Sie nicht über das Internet verfügbar gemacht Ports zugreifen. Beispielsweise wenn Sie mit einem der am Knoten über SSH verbinden, können aus dem am Knoten Sie dann direkt Services ausgeführt wird, klicken Sie auf die Cluster-Knoten zugreifen.

> [AZURE.IMPORTANT] Wenn Sie einen Cluster HDInsight erstellen, wenn Sie ein Azure-virtuellen Netzwerk nicht als Konfigurationsoption angeben, wird eine erstellt. Sie können anderen Computern (wie andere Azure-virtuellen Computern oder Ihrem Clientcomputer Entwicklung) jedoch nicht auf diese automatisch erstellte virtuelles Netzwerk teilnehmen. 

Zum Teilnehmen an zusätzliche Computer an das virtuelle Netzwerk müssen Sie erstellen zuerst das virtuelle Netzwerk, und geben Sie es Ihren Cluster HDInsight zu erstellen. Weitere Informationen finden Sie unter [-Funktionen erweitern HDInsight mithilfe einer Azure-virtuellen Netzwerk](hdinsight-extend-hadoop-virtual-network.md)

## <a name="public-ports"></a>Öffentliche ports

Alle Knoten in einem Cluster HDInsight in ein Azure-virtuellen Netzwerk befinden, und können nicht direkt aus dem Internet zugegriffen werden. Ein öffentlicher Gateway bietet Zugriff auf das Internet an die folgenden Ports, die für alle Arten von HDInsight Cluster häufig verwendet werden.

| Dienst | Port | Protokoll | Beschreibung |
| ---- | ---------- | -------- | ----------- | ----------- |
| sshd | 22 | SSH | Clients verbunden mit Sshd auf dem primären Headnode. Finden Sie unter [Verwenden von SSH mit Linux-basierten HDInsight](hdinsight-hadoop-linux-use-ssh-windows.md) |
| sshd | 22 | SSH | Clients verbunden mit Sshd auf den Rand Knoten (nur HDInsight Premium). Finden Sie unter [Erste Schritte mit R Server auf HDInsight](hdinsight-hadoop-r-server-get-started.md) |
| sshd | 23 | SSH | Clients verbunden mit Sshd auf der Sekundärachse Headnode. Finden Sie unter [Verwenden von SSH mit Linux-basierten HDInsight](hdinsight-hadoop-linux-use-ssh-windows.md) |
| Ambari | 443 | HTTPS | Ambari Web-Benutzeroberfläche. Finden Sie unter [Verwalten von HDInsight mithilfe der Ambari Web-Benutzeroberfläche](hdinsight-hadoop-manage-ambari.md) |
| Ambari | 443 | HTTPS | Ambari REST-API. Finden Sie unter [Verwalten von HDInsight mithilfe der Ambari REST-API](hdinsight-hadoop-manage-ambari-rest-api.md) |
| WebHCat | 443 | HTTPS | HCatalog REST-API. Finden Sie unter [Verwenden der Struktur mit Kringel](hdinsight-hadoop-use-pig-curl.md) [Schwein mit Kringel verwenden](hdinsight-hadoop-use-pig-curl.md), [MapReduce mit Kringel verwenden](hdinsight-hadoop-use-mapreduce-curl.md) |
| HiveServer2 | 443 | ODBC | Verbindet mit Struktur ODBC verwenden. Finden Sie unter [Verbinden von Excel mit HDInsight mit dem Microsoft-ODBC-Treiber](hdinsight-connect-excel-hive-odbc-driver.md). |
| HiveServer2 | 443 | JDBC | Eine Verbindung mit der Verwendung von JDBC Struktur. Finden Sie unter [Verbinden Struktur auf HDInsight Struktur JDBC-Treiber verwenden](hdinsight-connect-hive-jdbc-driver.md) |

Die folgenden sind für bestimmte Clustertypen verfügbar:

| Dienst | Port | Protokoll |Clustertyp | Beschreibung |
| ------------ | ---- |  ----------- | --- | ----------- |
| Stargate | 443 | HTTPS | HBase | HBase REST-API. Finden Sie unter [Erste Schritte mit HBase](hdinsight-hbase-tutorial-get-started-linux.md) |
| Livius | 443 | HTTPS |  Spark | Spark REST-API. Finden Sie unter [Senden Spark Aufträge mit Remote Livius](hdinsight-apache-spark-livy-rest-interface.md) |
| Storm | 443 | HTTPS | Storm | Storm-Web-Benutzeroberfläche. Finden Sie unter [Bereitstellen und Verwalten von Storm Topologien auf HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)

### <a name="authentication"></a>Authentifizierung

Alle Dienste öffentlich im Internet verfügbar gemacht müssen authentifiziert werden:

| Port | Anmeldeinformationen |
| ---- | ----------- |
| 22 oder 23 | Die Anmeldeinformationen des Benutzers SSH während der Clustererstellung angegeben |
| 443 | Der Anmeldename (Standard: Administrator) und Ihr Kennwort ein, die während der Clustererstellung festgelegt wurden |

## <a name="non-public-ports"></a>Nicht öffentliche ports

> [AZURE.NOTE] Einige Dienste sind nur für bestimmte Clustertypen verfügbar. HBase beträgt beispielsweise nur auf HBase Cluster Arten zur Verfügung.

### <a name="hdfs-ports"></a>HDFS ports

| Dienst | Knoten | Port | Protokoll | Beschreibung |
| ------- | ------- | ---- | -------- | ----------- | 
| NameNode Web-Benutzeroberfläche | Kopf Knoten | 30070 | HTTPS | Web-Benutzeroberfläche zum Anzeigen des aktuellen status |
| NameNode Metadatendienste | am Knoten | 8020 | IPC | Dateisystem-Metadaten 
| DataNode | Alle Worker Knoten | 30075 | HTTPS | Web-Benutzeroberfläche zum Anzeigen des Status, Protokolle usw.. |
| DataNode | Alle Worker Knoten | 30010 | &nbsp; | Datenübertragung |
| DataNode | Alle Worker Knoten | 30020 | IPC | Metadaten Vorgänge |
| Sekundäre NameNode | Kopf Knoten | 50090 | HTTP | Wissensstand für NameNode Metadaten |

### <a name="yarn-ports"></a>Ports aus

| Dienst | Knoten | Port | Protokoll | Beschreibung |
| ------- | ------- | ---- | -------- | ----------- |
| Ressourcenmanager Web-Benutzeroberfläche | Kopf Knoten | 8088 | HTTP | Web-Benutzeroberfläche für Ressourcenmanager |
| Ressourcenmanager Web-Benutzeroberfläche | Kopf Knoten | 8090 | HTTPS | Web-Benutzeroberfläche für Ressourcenmanager |
| Ressourcenmanager Admin-Benutzeroberfläche | am Knoten | 8141 | IPC | Für die Anwendung Übermittlungen (Struktur, Struktur Server, Schwein, usw..) |
| Ressourcenmanager scheduler | am Knoten | 8030 | HTTP | Administrative Schnittstelle |
| Ressourcenmanager Anwendungsbenutzeroberfläche | am Knoten | 8050 | HTTP |Adresse der Schnittstelle Applications-manager |
| NodeManager | Alle Worker Knoten | 30050 | &nbsp; | Die Adresse des Container-Managers |
| NodeManager Web-Benutzeroberfläche | Alle Worker Knoten | 30060 | HTTP | Ressourcen-Manager-Benutzeroberfläche |
| Zeitachse Adresse | Kopf Knoten | 10200 | RPC | Der Zeitachse RPC-Dienst. |
| Zeitachse Web-Benutzeroberfläche | Kopf Knoten | 8181 | HTTP | Der Zeitachse Service Web-Benutzeroberfläche |

### <a name="hive-ports"></a>Struktur ports

| Dienst | Knoten | Port | Protokoll | Beschreibung |
| ------- | ------- | ---- | -------- | ----------- |
| HiveServer2 | Kopf Knoten | 10001 | Thrift | Dienst zum Herstellen einer programmgesteuert Struktur (Thrift/JDBC) |
| HiveServer | Kopf Knoten | 10000 | Thrift | Dienst zum Herstellen einer programmgesteuert Struktur (Thrift/JDBC) |
| Struktur Metastore | Kopf Knoten | 9083 | Thrift | Dienst zum Herstellen einer programmgesteuert Struktur Metadaten (Thrift/JDBC) |

### <a name="webhcat-ports"></a>WebHCat ports

| Dienst | Knoten | Port | Protokoll | Beschreibung |
| ------- | ------- | ---- | -------- | ----------- |
| WebHCat server | Kopf Knoten | 30111 | HTTP | Web-API auf HCatalog und andere Dienste Hadoop |

### <a name="mapreduce-ports"></a>MapReduce ports

| Dienst | Knoten | Port | Protokoll | Beschreibung |
| ------- | ------- | ---- | -------- | ----------- |
| JobHistory | Kopf Knoten | 19888 | HTTP | MapReduce JobHistory Web-Benutzeroberfläche |
| JobHistory | Kopf Knoten | 10020 | &nbsp; | MapReduce JobHistory server |
| ShuffleHandler | &nbsp; | 13562 | &nbsp; | Zwischen-XT für Karte Übertragung-Ausgänge an Reduzierstücken anfordern |

### <a name="oozie"></a>Oozie

| Dienst | Knoten | Port | Protokoll | Beschreibung |
| ------- | ------- | ---- | -------- | ----------- |
| Oozie server | Kopf Knoten | 11000 | HTTP | URL für den Oozie-Dienst |
| Oozie server | Kopf Knoten | 11001 | HTTP | Port für Oozie-Administrator |

### <a name="ambari-metrics"></a>Ambari Kennzahlen

| Dienst | Knoten | Port | Protokoll | Beschreibung |
| ------- | ------- | ---- | -------- | ----------- |
| Zeitachse (Anwendung Verlauf) | Kopf Knoten | 6188 | HTTP | Der Zeitachse Service Web-Benutzeroberfläche |
| Zeitachse (Anwendung Verlauf) | Kopf Knoten | 30200 | RPC | Der Zeitachse Service Web-Benutzeroberfläche |

### <a name="hbase-ports"></a>HBase ports

| Dienst | Knoten | Port | Protokoll | Beschreibung |
| ------- | ------- | ---- | -------- | ----------- |
| HMaster | Kopf Knoten | 16000 | &nbsp; | &nbsp; |
| HMaster Informationen Web-Benutzeroberfläche | Kopf Knoten | 16010 | HTTP | Den Port für das HBase Master-Web-Benutzeroberfläche |
| Region server | Alle Worker Knoten | 16020 | &nbsp; | &nbsp; |
| &nbsp; | &nbsp; | 2181 | &nbsp; | Den Port, die mit Clients ZooKeeper herstellen |

### <a name="kafka-ports"></a>Kafka ports

| Dienst | Knoten | Port | Protokoll | Beschreibung |
| ------- | ------- | ---- | -------- | ----------- |
| Bank  | Worker Knoten | 9092 | [Kafka-Protokoll](http://kafka.apache.org/protocol.html) | Für die Kommunikation mit Kunden verwendet |
| &nbsp; | Zookeeper Knoten | 2181 | &nbsp; | Den Port, die mit Clients Zookeeper herstellen |

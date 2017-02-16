<properties 
   pageTitle="Verwenden von Apache Karlsruhe und Eichhörnchen in HDInsight | Microsoft Azure" 
   description="Erfahren Sie, wie Apache Karlsruhe in HDInsight verwenden und wie Sie installieren und Konfigurieren von Eichhörnchen auf Ihre Arbeitsstationen für die Verbindung zu einem Cluster HBase in HDInsight." 
   services="hdinsight" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/02/2016"
   ms.author="jgao"/>

# <a name="use-apache-phoenix-with-linux-based-hbase-clusters-in-hdinsight"></a>Verwenden von Apache Karlsruhe mit HBase Linux-basierten Cluster in HDInsight  

Erfahren Sie, wie [Apache Karlsruhe](http://phoenix.apache.org/) in HDInsight verwenden und wie Sie SQLLine verwenden. Weitere Informationen zu Karlsruhe finden Sie unter [Karlsruhe in 15 Minuten oder weniger](http://phoenix.apache.org/Phoenix-in-15-minutes-or-less.html). Die Grammatik Karlsruhe finden Sie unter [Karlsruhe Grammatik](http://phoenix.apache.org/language/index.html).

>[AZURE.NOTE] Die Version Karlsruhe Informationen in HDInsight finden Sie unter [Neuigkeiten in die Hadoop Cluster Versionen von HDInsight bereitgestellten?] [hdinsight-versions].

##<a name="use-sqlline"></a>Verwenden von SQLLine
[SQLLine](http://sqlline.sourceforge.net/) ist eine Befehlszeile-Programm, mit der SQL-Anweisung auszuführen. 

###<a name="prerequisites"></a>Erforderliche Komponenten
Bevor Sie SQLLine verwenden können, müssen Sie Folgendes:

- **A HBase Cluster in HDInsight**. Informationen zum Bereitstellen von HBase cluster, finden Sie unter [Erste Schritte mit Apache HBase in HDInsight][hdinsight-hbase-get-started].
- **Verbinden mit dem HBase Cluster über remote desktop-Protokoll**. Anweisungen finden Sie unter [Verwalten von Hadoop Cluster in HDInsight mithilfe des klassischen Azure-Portal][hdinsight-manage-portal].


Beim Herstellen einer zu einem Cluster HBase Verbindung müssen Sie die Verbindung mit einer der der lassen. Jeder Cluster HDInsight verfügt über 3 lassen. 

**Um herauszufinden, der Zookeeper Hostname**

1. Öffnen Sie Ambari, indem Sie durchsuchen, um **https://<ClusterName>. azurehdinsight.net**.
2. Geben Sie die HTTP (Cluster) Benutzernamen und Ihr Kennwort für die Anmeldung an.
3. Klicken Sie im Menü links auf **ZooKeeper** . So finden Sie unter 3 **ZooKeeper Server** aufgeführt.
4. Klicken Sie auf eine der aufgeführten **ZooKeeper Server** . Suchen Sie auf der Zusammenfassung der **Hostname**aus. Es ist ähnlich wie *zk1-jdolehb.3lnng4rcvp5uzokyktxs4a5dhd.bx.internal.cloudapp.net*.

**SQLLine verwenden**

1. Verbinden Sie mit dem Cluster SSH verwenden. Anweisungen finden Sie unter [Verwenden SSH mit Linux-basierten Hadoop auf HDInsight von Linux, Unix, oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md) oder [Verwenden SSH mit Linux-basierten Hadoop auf Windows HDInsight](hdinsight-hadoop-linux-use-ssh-windows.md) je nach dem Clientcomputer OS.

2. Führen Sie von SSH folgende Befehle SQLLine ausführen:

        cd /usr/hdp/2.2.9.1-7/phoenix/bin
        ./sqlline.py <ClusterName>:2181:/hbase-unsecure

2. Führen Sie die folgenden Befehle zum Erstellen einer Tabelle HBase, und fügen Sie Daten:

        CREATE TABLE Company (COMPANY_ID INTEGER PRIMARY KEY, NAME VARCHAR(225));
    
        !tables
        
        UPSERT INTO Company VALUES(1, 'Microsoft');
        
        SELECT * FROM Company;
        
        !quit

Weitere Informationen finden Sie unter [SQLLine manuellen](http://sqlline.sourceforge.net/#manual) und [Karlsruhe Grammatik](http://phoenix.apache.org/language/index.html).


 
##<a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie die Verwendung von Apache Karlsruhe HDInsight erhalten.  Weitere Informationen finden Sie unter

- [Übersicht über die HDInsight HBase][hdinsight-hbase-overview]: HBase ist einer Apache, Open-Source NoSQL-Datenbank basierend auf Hadoop, die Direktzugriff und sicherer Konsistenz für große Mengen von unstrukturierten und semistructured Daten enthält.
- [Bereitstellung von HBase Cluster virtuellen Netzwerk Azure][hdinsight-hbase-provision-vnet]: virtuelles Netzwerk Integration, HBase Cluster mit demselben virtuellen Netzwerk als Ihre Applikationen bereitgestellt werden, damit Applikationen direkt mit HBase kommunizieren können.
- [Konfigurieren von HBase Replikation in HDInsight](hdinsight-hbase-geo-replication.md): erfahren Sie, wie HBase Replikation über zwei Azure Datencenter konfigurieren. 
- [Analysieren Sie Twitter-Grüße mit HBase in HDInsight][hbase-twitter-sentiment]: erfahren Sie, wie in Echtzeit [Grüße Analyse](http://en.wikipedia.org/wiki/Sentiment_analysis) große Datenmengen in einem Cluster Hadoop in HDInsight mithilfe von HBase ausführen.

[azure-portal]: https://portal.azure.com
[vnet-point-to-site-connectivity]: https://msdn.microsoft.com/library/azure/09926218-92ab-4f43-aa99-83ab4d355555#BKMK_VNETPT

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-hbase-get-started]: hdinsight-hbase-tutorial-get-started.md
[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md#connect-to-hdinsight-clusters-by-using-rdp
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md

[hdinsight-hbase-phoenix-sqlline]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-phoenix-sqlline.png
[img-certificate]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-vpn-certificate.png
[img-vnet-diagram]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-vnet-point-to-site.png
[img-squirrel-driver]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-driver.png
[img-squirrel-alias]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-alias.png
[img-squirrel]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel.png
[img-squirrel-sql]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-sql.png


 

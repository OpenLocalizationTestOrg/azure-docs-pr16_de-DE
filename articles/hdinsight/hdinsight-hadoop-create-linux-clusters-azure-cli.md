<properties
    pageTitle="Hadoop, HBase oder Storm Cluster unter Linux in mithilfe der CLI Plattformen Azure HDInsight erstellen | Microsoft Azure"
    description="Erfahren Sie, wie Linux-basierten HDInsight Cluster mithilfe der Plattformen Azure CLI, Azure Ressourcenmanager Vorlagen und die Azure REST-API erstellen. Sie können den Clustertyp (Hadoop, HBase oder Storm) anzugeben, oder mithilfe von Skripts angepassten Komponenten installieren.."
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
    ms.date="09/20/2016"
    ms.author="larryfr"/>

#<a name="create-linux-based-clusters-in-hdinsight-using-the-azure-cli"></a>Erstellen von Linux-basierten Cluster in die Verwendung der CLI Azure HDInsight

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Die Azure ist ein Befehlszeilendienstprogramm Plattformen, die Sie zum Verwalten von Diensten Azure ermöglicht. Es kann, zusammen mit Azure Ressource Management Vorlagen verwendet werden ein HDInsight Cluster, zusammen mit den zugehörigen Speicherkonten und andere Dienste zu erstellen.

Azure Ressourcenverwaltung Vorlagen werden JSON-Standarddokumenten, die zur Beschreibung einer __Ressourcengruppe__ und alle Ressourcen darin (z. B. HDInsight.) Dieser Ansatz Vorlage basierende können Sie alle Ressourcen zu definieren, die Sie in eine Vorlage für HDInsight benötigen. Außerdem können Sie Änderungen an der Gruppe als Ganzes bis __Bereitstellungen__, die Änderungen für die gesamte Gruppe gelten verwalten.

Die Schritte in diesem Dokument durchgehen der Vorgang des Erstellens eines neuen HDInsight Clusters mithilfe der Azure CLI und eine Vorlage aus.

> [AZURE.IMPORTANT] Die Schritte in diesem Dokument verwenden die Standardanzahl von Worker Knoten (4) für eine HDInsight Cluster an. Wenn Sie auf mehr als 32 Worker Knoten (während der Clustererstellung oder durch Skalierung Cluster) planen, müssen Sie eine Knotengröße am mit mindestens 8 Kernen und 14 GB Ram auswählen.
>
> Weitere Informationen zu Knoten Größen und anfallenden Kosten finden Sie unter [HDInsight Preise](https://azure.microsoft.com/pricing/details/hdinsight/).

##<a name="prerequisites"></a>Erforderliche Komponenten

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion Azure abrufen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- __Azure CLI__. Die Schritte in diesem Dokument wurden zuletzt mit Azure CLI Version 0.10.1 getestet.

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)] 


### <a name="access-control-requirements"></a>Anforderungen für Access-Steuerelement

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

##<a name="log-in-to-your-azure-subscription"></a>Melden Sie sich bei Ihrem Azure-Abonnement

Führen Sie die Schritte beschrieben, die in [Verbindung mit einer Azure-Abonnement aus der Azure Line Interface (CLI Azure) herstellen](../xplat-cli-connect.md) , und Verbinden mit Ihr Abonnement mithilfe der Methode __Login__ .

##<a name="create-a-cluster"></a>Erstellen Sie einen cluster

Die folgenden Schritte sollten in einem Eingabeaufforderungsfenster, Shell oder terminal Sitzung nach dem Installieren und Konfigurieren der Azure CLI ausgeführt werden.

1. Verwenden Sie den folgenden Befehl mit Ihrem Abonnement Azure authentifiziert:

        azure login

    Sie werden aufgefordert, Ihr Name und Ihr Kennwort eingeben. Wenn Sie mehrere Azure-Abonnements verfügen, verwenden Sie `azure account set <subscriptionname>` das Abonnement festlegen, die die CLI Azure-Befehle verwenden.

3. Wechseln Sie zur Azure Ressourcenmanager Modus mit den folgenden Befehl aus:

        azure config mode arm

4. Erstellen Sie eine Ressourcengruppe aus. Diese Ressourcengruppe enthält den Cluster HDInsight und Speicher-Konto verknüpft.

        azure group create groupname location
        
    * Ersetzen Sie __Gruppenname__ durch einen eindeutigen Namen für die Gruppe ein. 
    * Ersetzen Sie __Speicherort__ durch die geografische Region, das Sie in die Gruppe erstellen möchten. 
    
        Verwenden Sie für eine Liste der gültigen Positionen, die `azure location list` Befehl aus, und verwenden Sie dann einen der Speicherorte aus der Spalte __Name__ .

5. Erstellen eines Speicher-Kontos an. Dieses Speicherkonto wird als Standardspeicher für den HDInsight Cluster verwendet werden.

        azure storage account create -g groupname --sku-name RAGRS -l location --kind Storage storagename
        
     * Ersetzen Sie __Gruppenname__ durch den Namen der Gruppe im vorherigen Schritt erstellt.
     * Ersetzen Sie __Speicherort__ mit am gleichen Speicherort, die im vorherigen Schritt. 
     * Ersetzen Sie __Storagename__ durch einen eindeutigen Namen für den Speicher-Konto an.
     
     > [AZURE.NOTE] Weitere Informationen über die Parameter in dieser Befehl verwendet werden, verwenden Sie `azure storage account create -h` um Hilfe für diesen Befehl anzuzeigen.

5. Rufen Sie den Schlüssel verwendet, um das Speicherkonto zuzugreifen.

        azure storage account keys list -g groupname storagename
        
    * Ersetzen Sie __Gruppenname__ durch den Namen der Ressource Gruppe ein.
    * Ersetzen Sie __Storagename__ durch den Namen des Speicherkontos.
    
    In den Daten, die zurückgegeben werden, speichern Sie __den Schlüsselwert für __Schlüssel1____ .

6. Erstellen eines HDInsight Clusters an.

        azure hdinsight cluster create -g groupname -l location -y Linux --clusterType Hadoop --defaultStorageAccountName storagename.blob.core.windows.net --defaultStorageAccountKey storagekey --defaultStorageContainer clustername --workerNodeCount 2 --userName admin --password httppassword --sshUserName sshuser --sshPassword sshuserpassword clustername

    * Ersetzen Sie __Gruppenname__ durch den Namen der Ressource Gruppe ein.

    * Ersetzen Sie __Hadoop__ durch den Clustertyp, den Sie erstellen möchten. Beispielsweise `Hadoop`, `HBase`, `Storm` oder `Spark`.

        > [AZURE.IMPORTANT] HDInsight Cluster einer Vielzahl von Typen, die entsprechen den Arbeitsbelastung oder Technologie, die für der Cluster optimiert ist nützlich sein. Es gibt keine unterstützte Methode zum einen Cluster zu erstellen, der mehrere Typen, z. B. Storm und HBase auf einem Cluster kombiniert. 

    * Ersetzen Sie __Speicherort__ mit am gleichen Speicherort, in den vorherigen Schritten an.

    * Ersetzen Sie __Storagename__ mit dem Speicher Kontonamen ein.

    * Ersetzen Sie __Storagekey__ mit dem Schlüssel im vorherigen Schritt abgerufen. 

    * Für die `--defaultStorageContainer` Parameter, verwenden Sie denselben Namen wie für den Cluster verwenden.

    * Ersetzen Sie __Administrator__ und __Httppassword__ mit dem Namen und das Kennwort ein, das Sie beim Zugriff auf den Cluster über HTTPS verwenden möchten.

    * Ersetzen Sie __Sshuser__ und __Sshuserpassword__ durch den Benutzernamen und das Kennwort ein, das Sie verwenden, wenn den Cluster über SSH zugreifen möchten

    Es kann mehrere Minuten für den Erstellungsprozess Cluster auf Fertig stellen. In der Regel ungefähr 15.

##<a name="next-steps"></a>Nächste Schritte

Jetzt, da Sie einen mithilfe der CLI Azure HDInsight-Cluster erfolgreich erstellt haben, verwenden Sie die folgenden Informationen zum Arbeiten mit Ihren Cluster:

###<a name="hadoop-clusters"></a>Hadoop Cluster

* [Verwenden Sie die Struktur mit HDInsight](hdinsight-use-hive.md)
* [Schwein mit HDInsight verwenden](hdinsight-use-pig.md)
* [Verwenden von MapReduce mit HDInsight](hdinsight-use-mapreduce.md)

###<a name="hbase-clusters"></a>HBase Cluster

* [Erste Schritte mit HBase auf HDInsight](hdinsight-hbase-tutorial-get-started-linux.md)
* [Java-Anwendungen für HBase auf HDInsight entwickeln](hdinsight-hbase-build-java-maven-linux.md)

###<a name="storm-clusters"></a>Storm Cluster

* [Entwickeln Sie Java Topologien für Storm auf HDInsight](hdinsight-storm-develop-java-topology.md)
* [Verwenden Sie Python Komponenten in Storm auf HDInsight](hdinsight-storm-develop-python-topology.md)
* [Bereitstellen Sie und überwachen Sie Topologien mit Storm auf HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)

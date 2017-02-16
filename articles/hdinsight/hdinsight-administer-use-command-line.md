<properties
    pageTitle="Hadoop-Cluster mithilfe von Azure CLI verwalten | Microsoft Azure"
    description="Wie Sie mithilfe der CLI Azure Hadoop Cluster in HDIsight verwalten"
    services="hdinsight"
    editor="cgronlun"
    manager="jhubbard"
    authors="mumian"
    tags="azure-portal"
    documentationCenter=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="jgao"/>

# <a name="manage-hadoop-clusters-in-hdinsight-using-the-azure-cli"></a>Verwalten von Hadoop Cluster unter Verwendung der CLI Azure HDInsight

[AZURE.INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Informationen Sie zum Verwalten von Hadoop Cluster in Azure HDInsight mithilfe der [Azure Line Benutzeroberflächen](../xplat-cli-install.md) . Die CLI Azure ist in Node.js implementiert. Sie können auf einer beliebigen Plattform verwendet werden, Node.js, einschließlich Windows, Mac und Linux unterstützt.

Dieser Artikel behandelt nur die CLI Azure mit HDInsight verwenden. Nach einem allgemeinen Leitfaden zum Azure CLI verwenden, finden Sie unter [Installieren und Konfigurieren von Azure CLI][azure-command-line-tools].

[AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

##<a name="prerequisites"></a>Erforderliche Komponenten

Vorbemerkung in diesem Artikel müssen Sie Folgendes:

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion Azure abrufen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- **Azure CLI** - finden Sie unter [Installieren und Konfigurieren der CLI Azure](../xplat-cli-install.md) Installation und Konfiguration von Informationen.
- **Verbinden mit Azure**, verwenden den folgenden Befehl aus:

        azure login

    Weitere Informationen zum Authentifizieren über ein Konto geschäftlichen oder schulnotizbücher finden Sie unter [Verbinden zu einem Azure Abonnement über die Befehlszeile Azure](xplat-cli-connect.md).
    
- **Die Ressourcenmanager Azure-Modus wechseln**, verwenden den folgenden Befehl aus:

        azure config mode arm

Um Hilfe zu erhalten, verwenden Sie den Schalter **-h** .  Beispiel:

    azure hdinsight cluster create -h
    
##<a name="create-clusters"></a>Cluster erstellen

Finden Sie unter [Erstellen von Linux-basierten Cluster in die Verwendung der CLI Azure HDInsight](hdinsight-hadoop-create-linux-clusters-azure-cli.md).

##<a name="list-and-show-cluster-details"></a>Hinzufügen und Anzeigen von Details cluster
Verwenden Sie die folgenden Befehle auflisten und Anzeigen von Details Cluster aus:

    azure hdinsight cluster list
    azure hdinsight cluster show <Cluster Name>

![HDI. CLIListCluster][image-cli-clusterlisting]


##<a name="delete-clusters"></a>Cluster löschen

Verwenden Sie den folgenden Befehl zum Löschen eines Clusters aus:

    azure hdinsight cluster delete <Cluster Name>

Sie können auch einen Cluster löschen, durch Löschen der Ressourcengruppe, die den Cluster enthält. Beachten Sie, dass dadurch alle Ressourcen in der Gruppe, einschließlich des Standardkontos-Speicher gelöscht werden.

    azure group delete <Resource Group Name>

##<a name="scale-clusters"></a>Maßstab Cluster

So ändern Sie die Größe des Hadoop cluster

    azure hdinsight cluster resize [options] <clusterName> <Target Instance Count>


## <a name="enabledisable-http-access-for-a-cluster"></a>Aktivieren/Deaktivieren der HTTP-Zugriff für einen cluster

    azure hdinsight cluster enable-http-access [options] <Cluster Name> <userName> <password>
    azure hdinsight cluster disable-http-access [options] <Cluster Name>

## <a name="enabledisable-rdp-access-for-a-cluster"></a>Deaktivieren Sie aktivieren/RDP Zugriff für einen cluster

    azure hdinsight cluster enable-rdp-access [options] <Cluster Name> <rdpUserName> <rdpPassword> <rdpExpiryDate>
    azure hdinsight cluster disable-rdp-access [options] <Cluster Name>


##<a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie gelernt verschiedene HDInsight Cluster administrative Aufgaben ausführen. Weitere Informationen finden Sie unter den folgenden Artikeln:

* [Verwalten von HDInsight mithilfe der Azure-Portal] [hdinsight-admin-portal]
* [Verwalten Sie mithilfe der PowerShell Azure HDInsight] [hdinsight-admin-powershell]
* [Erste Schritte mit Azure HDInsight] [hdinsight-get-started]
* [So verwenden Sie die Azure CLI] [azure-command-line-tools]


[azure-command-line-tools]: ../xplat-cli-install.md
[azure-create-storageaccount]: ../storage-create-storage-account.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/


[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[image-cli-account-download-import]: ./media/hdinsight-administer-use-command-line/HDI.CLIAccountDownloadImport.png
[image-cli-clustercreation]: ./media/hdinsight-administer-use-command-line/HDI.CLIClusterCreation.png
[image-cli-clustercreation-config]: ./media/hdinsight-administer-use-command-line/HDI.CLIClusterCreationConfig.png
[image-cli-clusterlisting]: ./media/hdinsight-administer-use-command-line/HDI.CLIListClusters.png "Hinzufügen und Anzeigen von Cluster"

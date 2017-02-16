<properties
    pageTitle="Verwenden von leeren Kante Knoten in HDInsight | Microsoft Azure"
    description="Wie ein Ampty Kantenknoten HDInsight Cluster hinzugefügt, die als Client verwendet werden können und Test-Host Ihre Applications HDInsight."
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
    ms.date="09/14/2016"
    ms.author="jgao"/>

# <a name="use-empty-edge-nodes-in-hdinsight"></a>Verwenden von leeren Kante Knoten in HDInsight

Erfahren Sie, wie einen leeres Kantenknoten zu einem Cluster Linux-basierten HDInsight hinzugefügt. Ein leere Kantenknoten ist ein Linux virtuellen Computern mit der gleichen Clienttools installiert und konfiguriert ist, wie in den Headnodes, aber keine Hadoop-Dienste ausgeführt. Den Kantenknoten können für den Zugriff auf den Cluster, Testen Ihre Clientanwendungen und Ihre Clientanwendungen hosten. 

Sie können einen leere Kantenknoten zu einem vorhandenen HDInsight Cluster, um einen neuen Cluster beim Erstellen des Clusters hinzufügen. Hinzufügen eines leeren Kante Knotens erfolgt mit Ressourcenmanager Azure-Vorlage.  Im folgende Beispiel wird veranschaulicht, wie dies funktioniert mithilfe einer Vorlage:

    "resources": [
        {
            "name": "[concat(parameters('clusterName'),'/', variables('applicationName'))]",
            "type": "Microsoft.HDInsight/clusters/applications",
            "apiVersion": "[variables('clusterApiVersion')]",
            "properties": {
                "marketPlaceIdentifier": "EmptyNode",
                "computeProfile": {
                    "roles": [{
                        "name": "edgenode",
                        "targetInstanceCount": 1,
                        "hardwareProfile": {
                            "vmSize": "[parameters('edgeNodeSize')]"
                        }
                    }]
                },
                "installScriptActions": [{
                    "name": "[concat('emptynode','-' ,uniquestring(variables('applicationName')))]",
                    "uri": "https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/EmptyNode/scripts/EmptyNodeSetup.sh",
                    "roles": ["edgenode"]
                }],
                "uninstallScriptActions": [],
                "httpsEndpoints": [],
                "applicationType": "CustomApplication"
            }
        }
    ],

Wie im Beispiel dargestellt, können Sie optional eine [Skriptaktion](hdinsight-hadoop-customize-cluster-linux.md) um zusätzliche Konfigurationsschritte, wie etwa der Installation von [Apache Farbton](hdinsight-hadoop-hue-linux.md) im Rand Knoten durchführen aufrufen.

Nachdem Sie einen Kantenknoten erstellt haben, können Sie auf den Rand Knoten über SSH verbinden, und führen Clienttools Hadoop Cluster in HDInsight zugreifen.

## <a name="add-an-edge-node-to-an-existing-cluster"></a>Hinzufügen eines Kante Knotens zu einem vorhandenen cluster

In diesem Abschnitt verwenden Sie eine Vorlage Ressourcenmanager einen Kantenknoten zu einem vorhandenen HDInsight Cluster hinzugefügt.  Die Vorlage Ressourcenmanager finden Sie im [GitHub](https://github.com/hdinsight/Iaas-Applications/tree/master/EmptyNode). Die Vorlage Ressourcenmanager Ruft eine Skript Aktion Skript unter https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/EmptyNode/scripts/EmptyNodeSetup.sh. Das Skript verarbeiten nicht.  Dies ist einen Skriptaktion aus einer Vorlage Ressourcenmanager zu veranschaulichen.

**Hinzufügen einen leeren Kantenknoten zu einem vorhandenen cluster**

1. Erstellen Sie einen HDInsight Cluster, wenn Sie eine noch nicht.  Finden Sie unter [Hadoop Lernprogramm: Erste Schritte mit Linux-basierten Hadoop in HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).
2. Klicken Sie auf die folgende Abbildung zum Anmelden bei Azure, und öffnen Sie die Vorlage Azure Ressourcenmanager Azure-Portal. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhdinsight%2FIaas-Applications%2Fmaster%2FEmptyNode%2Fazuredeploy.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

3. Konfigurieren Sie die folgenden Eigenschaften:

    - CLUSTERNAME: Geben Sie den Namen einer vorhandenen Cluster-HDInsight.
    - EDGENODESIZE: Wählen Sie eine der Größen virtueller Computer.
    - EDGENODEPREFIX: Der Standardwert ist **neu**.  Verwenden den Standardwert, ist der Namen des Rands Knoten **neue Edgenode**aus.  Sie können das Präfix aus dem Portal anpassen. Sie können auch den vollständigen Namen aus der Vorlage anpassen.


4. Klicken Sie auf **OK** , um die Änderungen zu speichern.
5. Wählen Sie eine Ressourcengruppe in **Ressourcengruppe**.
6. Klicken Sie auf **Überprüfen Vertragsbedingungen**, und klicken Sie dann auf **kaufen**.
7. Wählen Sie **die Pin zum Dashboard**aus, und klicken Sie dann auf **Erstellen**.

## <a name="add-an-edge-node-when-creating-a-cluster"></a>Fügen Sie einen Kantenknoten aus, wenn einen Cluster erstellen

In diesem Abschnitt mithilfe erstellen Sie eine Vorlage Ressourcenmanager HDInsight Cluster mit einem Kantenknoten.  Die Vorlage Ressourcenmanager kann in einem öffentlichen [Azure Blob-Speichercontainer](http://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-hadoop-cluster-in-hdinsight-with-edge-node.json)gefunden werden. Die Vorlage Ressourcenmanager Ruft eine Skript Aktion Skript unter https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/EmptyNode/scripts/EmptyNodeSetup.sh. Das Skript verarbeiten nicht.  Dies ist einen Skriptaktion aus einer Vorlage Ressourcenmanager zu veranschaulichen.

**Hinzufügen einen leeren Kantenknoten zu einem vorhandenen cluster**

1. Erstellen Sie einen HDInsight Cluster, wenn Sie eine noch nicht.  Finden Sie unter [Hadoop Lernprogramm: Erste Schritte mit Linux-basierten Hadoop in HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).
2. Klicken Sie auf die folgende Abbildung zum Anmelden bei Azure, und öffnen Sie die Vorlage Azure Ressourcenmanager Azure-Portal. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hadoop-cluster-in-hdinsight-with-edge-node.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

3. Konfigurieren Sie die folgenden Eigenschaften:
        
    - CLUSTERNAME: Geben Sie einen Namen für den neuen Cluster zu erstellen.
    - CLUSTERLOGINUSERNAME: Geben Sie den Benutzernamen Hadoop HTTP.  Der Standardname lautet **Admin**.
    - CLUSTERLOGINPASSWORD: Geben Sie das Benutzerkennwort Hadoop HTTP.
    - SSHUSERNAME: Geben Sie den Benutzernamen SSH. Der Standardname lautet **Sshuser**.
    - SSHPASSWORD: Geben Sie das Benutzerkennwort SSH.
    - Standort: Geben Sie den Speicherort der Ressourcengruppe Name, Cluster und des Standardkontos-Speicher.
    - CLUSTERTYPE: Der Standardwert ist **Hadoop**.
    - CLUSTERWORKERNODECOUNT: Der Standardwert ist 2.
    - EDGENODESIZE: Wählen Sie eine der Größen virtueller Computer.
    - EDGENODEPREFIX: Der Standardwert ist **neu**.  Verwenden den Standardwert, ist der Namen des Rands Knoten **neue Edgenode**aus.  Sie können das Präfix aus dem Portal anpassen. Sie können auch den vollständigen Namen aus der Vorlage anpassen.

4. Klicken Sie auf **OK** , um die Änderungen zu speichern.
5. Geben Sie einen neuen Namen für die Ressourcengruppe **Ressourcengruppe**.
6. Klicken Sie auf **Überprüfen Vertragsbedingungen**, und klicken Sie dann auf **kaufen**.
7. Wählen Sie **die Pin zum Dashboard**aus, und klicken Sie dann auf **Erstellen**. 


## <a name="access-an-edge-node"></a>Greifen Sie auf eine Kantenknoten

Der Kantenknoten ssh Endpunkt ist <EdgeNodeName>. <ClusterName>-ssh.azurehdinsight.net:22.  Beispielsweise neue-edgenode.myedgenode0914-ssh.azurehdinsight.net:22.

Der Kantenknoten wird als eine Anwendung auf dem Portal Azure angezeigt.  Im Portal bietet Ihnen die Informationen auf den Rand Knoten über SSH zuzugreifen.

**Um zu überprüfen, den Rand Knoten SSH Endpunkt**

1. Melden Sie sich auf der [Azure-Portal](https://portal.azure.com)an.
2. Öffnen Sie HDInsight Cluster mit einem Kantenknoten aus.
3. Klicken Sie auf **Applikationen** aus dem Cluster Blade. So finden Sie unter den Rand Knoten.  Der Standardname ist **neue Edgenode**.
4. Klicken Sie auf den Rand Knoten. So finden Sie unter den Endpunkt SSH.

**Klicken Sie auf den Rand Knoten mit Struktur**

5. SSH Verbindung zu den Rand Knoten verwenden.  Finden Sie unter [Verwenden von SSH mit Linux-basierten Hadoop auf HDInsight von Linux, Unix, oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md) oder [SSH mit Linux-basierten Hadoop auf HDInsight von Windows verwenden](hdinsight-hadoop-linux-use-ssh-windows.md).
6. Nachdem Sie auf den Rand Knoten über SSH verbunden haben, verwenden Sie den folgenden Befehl in die Struktur-Verwaltungskonsole zu öffnen:

        hive
7. Führen Sie den folgenden Befehl Cluster Struktur Tabellen angezeigt:

        show tables;

## <a name="delete-an-edge-node"></a>Löschen eines Kante Knotens

Sie können einen Kantenknoten vom Azure-Portal löschen.

**Zugriff auf eine Kantenknoten**

1. Melden Sie sich auf der [Azure-Portal](https://portal.azure.com)an.
2. Öffnen Sie HDInsight Cluster mit einem Kantenknoten aus.
3. Klicken Sie auf **Applikationen** aus dem Cluster Blade. Sie sind eine Liste der Kante Knoten angezeigt.  
4. Mit der rechten Maustaste in des Rand-Knotens, die, den Sie löschen möchten, und klicken Sie dann auf **Löschen**.
5. Klicken Sie auf **Ja,** um zu bestätigen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie zum Hinzufügen eines Kante Knotens und zum Zugreifen auf des Rand Knotens erhalten. Weitere Informationen finden Sie unter den folgenden Artikeln:

- [HDInsight Installieren von Applications](hdinsight-apps-install-applications.md): erfahren, wie Sie eine Anwendung HDInsight zu Ihrer Cluster zu installieren.
- [Installieren von benutzerdefinierten HDInsight Applications](hdinsight-apps-install-custom-applications.md): erfahren Sie, wie eine nicht veröffentlichte HDInsight Anwendung mit HDInsight bereitstellen.
- [Veröffentlichen von HDInsight Applications](hdinsight-apps-publish-applications.md): erfahren Sie, wie Ihre benutzerdefinierten HDInsight Applikationen zu Azure Marketplace veröffentlichen.
- [MSDN: Installieren Sie die Anwendung HDInsight](https://msdn.microsoft.com/library/mt706515.aspx): Informationen zum Definieren von Applications HDInsight.
- [Anpassen von Linux-basierten HDInsight Cluster mithilfe der Aktion Skript](hdinsight-hadoop-customize-cluster-linux.md): erfahren Sie, wie Skript-Aktion verwenden, um zusätzliche Applikationen zu installieren.
- [Hadoop erstellen Linux-basierten Cluster in HDInsight mithilfe von Vorlagen Ressourcenmanager](hdinsight-hadoop-create-linux-clusters-arm-templates.md): erfahren Sie, wie Ressourcenmanager Vorlagen zum Erstellen von HDInsight Cluster aufrufen.


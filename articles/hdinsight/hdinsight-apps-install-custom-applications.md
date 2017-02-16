<properties
    pageTitle="Installieren von Applications Hadoop auf HDInsight | Microsoft Azure"
    description="Informationen Sie zum HDInsight auf HDInsight Applikationen installieren."
    services="hdinsight"
    documentationCenter=""
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="09/14/2016"
    ms.author="jgao"/>

# <a name="install-custom-hdinsight-applications"></a>Installieren von benutzerdefinierten HDInsight Anwendungen

Eine HDInsight-Anwendung ist eine Anwendung, die Benutzer in einem HDInsight Linux-basierten Cluster installieren können.  Diese Anwendung können von Microsoft, unabhängigen Software-Anbietern (ISV) oder sich selbst entwickelt werden. In diesem Artikel erfahren Sie, wie Sie eine Anwendung HDInsight installieren, die nicht Azure-Portal auf HDInsight veröffentlicht wurde. Die Anwendung, die Sie installieren möchten ist [Farbton](http://gethue.com/). 

Andere verwandte Artikel:

- [HDInsight Installieren von Applications](hdinsight-apps-install-applications.md): erfahren, wie Sie eine Anwendung HDInsight zu Ihrer Cluster zu installieren.
- [Veröffentlichen von HDInsight Applications](hdinsight-apps-publish-applications.md): erfahren Sie, wie Ihre benutzerdefinierten HDInsight Applikationen zu Azure Marketplace veröffentlichen.
- [MSDN: Installieren Sie die Anwendung HDInsight](https://msdn.microsoft.com/library/mt706515.aspx): Informationen zum Definieren von Applications HDInsight.

 
## <a name="prerequisites"></a>Erforderliche Komponenten

Wenn Sie HDInsight Anwendungen auf einem vorhandenen HDInsight Cluster installieren möchten, müssen Sie einen HDInsight Cluster verfügen. Um eine zu erstellen, finden Sie unter [Cluster erstellen](hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster). Sie können auch HDInsight Applikationen installieren, wenn Sie einen Cluster HDInsight erstellen.


## <a name="install-hdinsight-applications"></a>Installieren von Applications HDInsight

HDInsight Applikationen können installiert werden, wenn Sie einen Cluster erstellen oder einem vorhandenen HDInsight Cluster. Definieren von Azure Ressourcenmanager Vorlagen, finden Sie unter [MSDN: Installieren Sie die Anwendung HDInsight](https://msdn.microsoft.com/library/mt706515.aspx).

Die Dateien für die Bereitstellung von dieser Anwendung (Farbton) erforderlich:

- [azuredeploy.JSON](https://github.com/hdinsight/Iaas-Applications/blob/master/Hue/azuredeploy.json): der Ressourcenmanager Vorlage für die Installation von HDInsight Anwendung. Finden Sie unter [MSDN: Installieren Sie die Anwendung HDInsight](https://msdn.microsoft.com/library/mt706515.aspx) für die Entwicklung einer eigenen Vorlage Ressourcenmanager.
- [Farbton-install_v0.sh](https://github.com/hdinsight/Iaas-Applications/blob/master/Hue/scripts/Hue-install_v0.sh): das Skript Aktion, die durch die Vorlage Ressourcenmanager zum Konfigurieren des Kante Knotens aufgerufen wird. 
- [Farbton-binaries.tgz](https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv01/hue-binaries-14-04.tgz): der Farbton Binärdatei aus den Hinweis auf ein-install_v0.sh aufgerufen wird. 
- [Farbton-Binärdateien-14-04.tgz](https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv01/hue-binaries-14-04.tgz): der Farbton Binärdatei aus den Hinweis auf ein-install_v0.sh aufgerufen wird. 
- [Webwasb-tomcat.tar.gz](https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv01/webwasb-tomcat.tar.gz): eine Stichprobe Web-Anwendung (Tomcat) aus den Hinweis auf ein-install_v0.sh aufgerufen wird.

**So installieren Sie Farbton zu einem vorhandenen HDInsight cluster**

1. Klicken Sie auf die folgende Abbildung zum Anmelden bei Azure, und öffnen Sie die Vorlage Ressourcenmanager Azure-Portal. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhdinsight%2FIaas-Applications%2Fmaster%2FHue%2Fazuredeploy.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

    Diese Schaltfläche öffnet eine Vorlage Ressourcenmanager Azure-Portal an.  Die Vorlage Ressourcenmanager befindet sich unter [https://github.com/hdinsight/Iaas-Applications/tree/master/Hue](https://github.com/hdinsight/Iaas-Applications/tree/master/Hue).  So schreiben Sie diese Vorlage Ressourcenmanager finden Sie unter [MSDN: Installieren Sie die Anwendung HDInsight](https://msdn.microsoft.com/library/mt706515.aspx).
    
2. Geben Sie aus dem **Parameter** Blade Folgendes ein:

    - **ClusterName**: Geben Sie den Namen des Cluster auf die Stelle, an der die Anwendung installiert werden soll. Dieser Cluster muss sich auf einem vorhandenen Cluster.
    
3. Klicken Sie auf **OK** , um die Parameter zu speichern.
4. Geben Sie aus dem Blade **benutzerdefinierte Bereitstellung** **Ressourcengruppe**ein.  Die Ressourcengruppe ist ein Container, der die Cluster, das Konto abhängige Speicherplatz und andere Ressourcen gruppiert. Es ist erforderlich, mit der gleichen Ressourcengruppe als Cluster.
5. Klicken Sie auf **rechtliche Ausdrücke**, und klicken Sie dann auf **Erstellen**.
6. Überprüfen Sie, ob das Kontrollkästchen **Pin zum Dashboard** ausgewählt ist, und klicken Sie dann auf **Erstellen**. Sie können den Installationsstatus von der Kachel der Portalseite Benachrichtigung zu dem Portal Dashboard angehefteten anzeigen (klicken Sie auf das Glockensymbol am oberen Rand des Portals).  Es dauert ungefähr 10 Minuten zum Installieren der Anwendung.

**So installieren Sie Farbton beim Erstellen eines Clusters**

1. Klicken Sie auf die folgende Abbildung zum Anmelden bei Azure, und öffnen Sie die Vorlage Ressourcenmanager Azure-Portal. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fhdinsightapps%2Fcreate-linux-based-hadoop-cluster-in-hdinsight.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

    Diese Schaltfläche öffnet eine Vorlage Ressourcenmanager Azure-Portal an.  Die Vorlage Ressourcenmanager befindet sich unter [https://hditutorialdata.blob.core.windows.net/hdinsightapps/create-linux-based-hadoop-cluster-in-hdinsight.json](https://hditutorialdata.blob.core.windows.net/hdinsightapps/create-linux-based-hadoop-cluster-in-hdinsight.json).  So schreiben Sie diese Vorlage Ressourcenmanager finden Sie unter [MSDN: Installieren Sie die Anwendung HDInsight](https://msdn.microsoft.com/library/mt706515.aspx).

2. Führen Sie die Anweisung Cluster zu erstellen, und installieren Farbton. Weitere Informationen zum Erstellen von HDInsight Cluster finden Sie unter [Erstellen von Linux-basierten Hadoop Cluster in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

Zusätzlich zu den Azure-Portal können Sie [Azure PowerShell](hdinsight-hadoop-create-linux-clusters-arm-templates.md#deploy-with-powershell) und [Azure CLI](hdinsight-hadoop-create-linux-clusters-arm-templates.md#deploy-with-azure-cli) Sie auch aufrufen, Ressourcenmanager Vorlagen.

## <a name="validate-the-installation"></a>Überprüfen der installation

Sie können den Anwendungsstatus im Azure-Portal zu überprüfen Sie die Installation der Anwendung überprüfen. Darüber hinaus können Sie auch überprüfen alle HTTP-Endpunkte kam von wie erwartet und der Webseite, sofern vorhanden:

**So öffnen Sie das Portal Farbton**

1. Melden Sie sich mit dem [Azure-Portal](https://portal.azure.com)aus.
2. Klicken Sie im linken Menü auf **HDInsight Cluster** .  Wenn Sie es nicht sehen, klicken Sie auf **Durchsuchen**, und klicken Sie dann auf **HDInsight Cluster**.
3. Klicken Sie auf die Stelle, an der Sie die Anwendung installiert Cluster.
4. Klicken Sie aus dem Blade **Einstellungen** unter der Kategorie **Allgemein** auf **Applications** . So finden Sie unter **Farbton** in der **Installierten Apps** Blade aufgeführt.
5. Klicken Sie auf **Farbton** aus der Liste, um die Liste der Eigenschaften.  
6. Klicken Sie auf der Webseite-Link, um die Website zu überprüfen. Öffnen Sie den HTTP-Endpunkt in einem Browser zu das Farbton Web-Benutzeroberfläche zu überprüfen, öffnen Sie den SSH-Endpunkt mit [kitten](hdinsight-hadoop-linux-use-ssh-windows.md) oder andere [SSH-Clients](hdinsight-hadoop-linux-use-ssh-unix.md)aus.
 
## <a name="troubleshoot-the-installation"></a>Problembehandlung bei der installation

Sie können überprüfen, dass den Status für die Installation von Anwendung von der Portalseite Benachrichtigung (klicken Sie auf das Glockensymbol am oberen Rand des Portals). 


Wenn eine Anwendungsinstallation fehlgeschlagen ist, können Sie die Fehlermeldungen angezeigt und Debuggen von 3 stellen Informationen:

- HDInsight Applications: Allgemeine Fehlerinformationen.

    Öffnen Sie den Cluster aus dem Portal, und klicken Sie auf Applikationen aus dem Blade Einstellungen:

    ![Hdinsight Applications Installation Anwendungsfehler](./media/hdinsight-apps-install-applications/hdinsight-apps-error.png)

- HDInsight Skript für Aktion: Wenn der eine Aktion Skriptfehler in die HDInsight-Anwendungen Fehlermeldung weist darauf hin, weitere Details zu den Skriptfehler behandelt werden im Bereich Aktionen Skript.

    Klicken Sie auf Skriptaktion aus dem Blade Einstellungen. Skript Aktionsverlauf zeigt die Fehlermeldungen

    ![Hdinsight Applikationen Skriptfehler Aktion](./media/hdinsight-apps-install-applications/hdinsight-apps-script-action-error.png)
    
- Ambari Web-Benutzeroberfläche: Wenn Skript für die Installation die Ursache des Fehlers wurde, verwenden Sie Ambari Web-Benutzeroberfläche zum Überprüfen der vollständige Protokolle über die Installationsskripts.

    Weitere Informationen finden Sie unter [Problembehandlung](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting).

## <a name="remove-hdinsight-applications"></a>Entfernen von Applications HDInsight

Es gibt mehrere Methoden zum Löschen von Applications HDInsight.

### <a name="use-portal"></a>Verwenden von portal

**So entfernen Sie eine Anwendung verwenden des Portals**

1. Melden Sie sich mit dem [Azure-Portal](https://portal.azure.com)aus.
2. Klicken Sie im linken Menü auf **HDInsight Cluster** .  Wenn Sie es nicht sehen, klicken Sie auf **Durchsuchen**, und klicken Sie dann auf **HDInsight Cluster**.
3. Klicken Sie auf die Stelle, an der Sie die Anwendung installiert Cluster.
4. Klicken Sie aus dem Blade **Einstellungen** unter der Kategorie **Allgemein** auf **Applications** . Sie sind eine Liste der installierten Anwendung angezeigt. In diesem Lernprogramm werden **Farbton** in der **Installierten Apps** Blade aufgelistet.
5. Mit der rechten Maustaste in der Anwendungs, die Sie entfernen möchten, und klicken Sie dann auf **Löschen**.
6. Klicken Sie auf **Ja,** um zu bestätigen.

Sie können auch Cluster löschen oder löschen die Ressourcengruppe, die die Anwendung enthält, im Portal.

### <a name="use-azure-powershell"></a>Verwenden von Azure PowerShell

Azure PowerShell können Sie den Cluster löschen oder löschen die Ressourcengruppe. Finden Sie unter [Cluster mithilfe der PowerShell Azure löschen](hdinsight-administer-use-powershell.md#delete-clusters).

### <a name="use-azure-cli"></a>Verwenden von Azure CLI

Azure CLI können Sie den Cluster löschen oder löschen die Ressourcengruppe. Finden Sie unter [Cluster mithilfe von Azure CLI löschen](hdinsight-administer-use-command-line.md#delete-clusters).


## <a name="next-steps"></a>Nächste Schritte

- [MSDN: Installieren Sie die Anwendung HDInsight](https://msdn.microsoft.com/library/mt706515.aspx): erfahren Sie, wie Ressourcenmanager Vorlagen für die Bereitstellung von Applications HDInsight entwickeln möchte.
- [HDInsight Installieren von Applications](hdinsight-apps-install-applications.md): erfahren, wie Sie eine Anwendung HDInsight zu Ihrer Cluster zu installieren.
- [Veröffentlichen von HDInsight Applications](hdinsight-apps-publish-applications.md): erfahren Sie, wie Ihre benutzerdefinierten HDInsight Applikationen zu Azure Marketplace veröffentlichen.
- [Anpassen von Linux-basierten HDInsight Cluster mithilfe der Aktion Skript](hdinsight-hadoop-customize-cluster-linux.md): erfahren Sie, wie Skript-Aktion verwenden, um zusätzliche Applikationen zu installieren.
- [Hadoop erstellen Linux-basierten Cluster in HDInsight mithilfe von Vorlagen Ressourcenmanager](hdinsight-hadoop-create-linux-clusters-arm-templates.md): erfahren Sie, wie Ressourcenmanager Vorlagen zum Erstellen von HDInsight Cluster aufrufen.
- [Leere Kante Knoten in HDInsight verwenden](hdinsight-apps-use-edge-node.md): erfahren, wie Sie einen leeren Kantenknoten für den Zugriff auf HDInsight Cluster, HDInsight Applikationen testen und Hosten von Applications HDInsight zu verwenden.

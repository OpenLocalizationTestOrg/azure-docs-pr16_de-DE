<properties
   pageTitle="Erstellen von Hadoop Cluster in HDInsight | Microsoft Azure"
    description="Erfahren Sie, wie Sie Cluster für Azure HDInsight mithilfe von Azure-Portal zu erstellen."
   services="hdinsight"
   documentationCenter=""
   tags="azure-portal"
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

# <a name="create-windows-based-hadoop-clusters-in-hdinsight-using-the-azure-portal"></a>Erstellen von Windows-basiertem Hadoop Cluster in Verwenden des Portals Azure HDInsight

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Informationen Sie zum Erstellen eines Hadoop Clusters in HDInsight mithilfe von Azure-Portal. Das Microsoft [Azure-Portal](../azure-portal-overview.md) ist eine zentrale Stelle, wo Sie bereitstellen und Verwalten von Azure Ressourcen können. Azure-Portal ist eines der Tools, die Sie verwenden können, um entweder Linux-basierten oder Windows-basiertem Hadoop Cluster in HDInsight zu erstellen. Für die Clustererstellung von anderen Tools und Features klicken Sie auf der Registerkarte wählen Sie am oberen Rand dieser Seite oder finden Sie unter [Methoden zur Erstellung](hdinsight-provision-clusters.md#cluster-creation-methods).

##<a name="prerequisites"></a>Voraussetzungen für:

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Bevor Sie die Anweisungen in diesem Artikel beginnen, müssen Sie die folgenden verfügen:

- Ein Azure-Abonnement. Finden Sie [kostenlose Testversion Azure abrufen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

### <a name="access-control-requirements"></a>Anforderungen für Access-Steuerelement

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-clusters"></a>Cluster erstellen


**So erstellen Sie einen HDInsight cluster**

1. Melden Sie sich bei der [Azure-Portal](https://portal.azure.com).
2. Klicken Sie auf **neu**, klicken Sie auf **Daten Analytics**, und klicken Sie dann auf **HDInsight**.

    ![Erstellen Sie einen neuen Cluster im Azure-Portal] (./media/hdinsight-provision-clusters/HDI.CreateCluster.1.png "Erstellen Sie einen neuen Cluster im Azure-Portal")

3. Geben Sie ein, oder wählen Sie die folgenden Werte aus:

    * **Cluster Name**: Geben Sie einen Namen für den Cluster. Wenn der Name verfügbar ist, wird ein grünes Häkchen neben dem Clusternamen angezeigt.

    * **Cluster Type**: Wählen Sie **Hadoop**aus. Andere Optionen Inclue **HBase**, **Storm**und **Spark**.

        > [AZURE.IMPORTANT] HDInsight Cluster einer Vielzahl von Typen, die entsprechen den Arbeitsbelastung oder Technologie, die für der Cluster optimiert ist nützlich sein. Es gibt keine unterstützte Methode zum einen Cluster zu erstellen, der mehrere Typen, z. B. Storm und HBase auf einem Cluster kombiniert.

    * **Betriebssystem Cluster**: Wählen Sie **Windows**aus. Wählen Sie zum Erstellen eines Linux-Basis Clusters **Linux**ein.
    * **Version**: [HDInsight Versionen](hdinsight-component-versioning.md)angezeigt.
    * **Abonnements**: Wählen Sie aus dem Azure-Abonnement, die zum Erstellen von diesem Cluster verwendet werden.
    * **Ressourcengruppe**: Wählen Sie aus einer vorhandenen oder Erstellen einer neuen Ressourcengruppe. Dieser Eintrag wird standardmäßig auf eine Ihrer bestehenden Gruppen für die Ressource festgelegt, sofern diese verfügbar sind.
    * **Anmeldeinformationen**: Konfigurieren der Benutzername und das Kennwort für die Hadoop (HTTP-Benutzer). Wenn Sie für den Cluster Remotedesktop aktivieren, müssen Sie den remote desktop Benutzer Benutzernamen und Kennwort und ein Kontoablaufdatum konfigurieren. Klicken Sie auf **auswählen** , unten, um die Änderungen zu speichern.

        ![Bereitstellen Cluster Anmeldeinformationen] (./media/hdinsight-provision-clusters/HDI.CreateCluster.3.png "Bereitstellen Cluster Anmeldeinformationen")

    * **Datenquelle**: Erstellen Sie ein neues Konto anzeigen oder auswählen ein vorhandenes Azure-Speicher als Standard-Dateisystem für den Cluster verwendet werden soll.

        ![Datenquellen-blade] (./media/hdinsight-provision-clusters/HDI.CreateCluster.4.png "Bereitstellen Datenquellen-Konfiguration")

        * **Auswahlmethode**: Legen Sie den Wert **alle Abonnements** aktivieren Speicherkonten aus allen Ihrer Abonnements zu durchsuchen. Legen Sie den **Zugriffstaste** , wenn Sie die **Storage Name** und **Zugriffstaste** eines vorhandenen Speicher-Kontos eingeben möchten.
        * **Wählen Sie Speicherkonto / neu erstellen**: Klicken Sie auf Durchsuchen, und wählen Sie ein vorhandenes Speicherkonto Cluster zuordnen möchten **Speicherkonto auswählen** . Oder klicken Sie auf **Neu erstellen** , um ein neues Speicherkonto erstellen. Verwenden Sie das Feld, das zur Eingabe des Namens des Speicherkontos angezeigt wird. Wenn der Name verfügbar ist, wird ein grünes Häkchen angezeigt.
        * **Wählen Sie Standardcontainer**: Verwenden Sie diese Option zur Eingabe des Namens des Standardcontainers für Cluster verwendet werden soll. Obwohl Sie hier einen beliebigen Namen eingeben können, empfehlen wir, mit demselben Namen wie des Clusters, damit Sie leicht erkennen können, dass der Container für diesen bestimmten Cluster verwendet wird.
        * **Standort**: der geografische Region, die Speicher-Konto ist, oder erstellt werden. Dieses Speicherorts wird den Cluster-Speicherort bestimmen.  Cluster und deren Speicher Standardkonto müssen in derselben Azure Data Center gemeinsame suchen.
    
    * **Knoten Preise Ebenen**: Legen Sie die Anzahl der Worker-Knoten, die Sie für den Cluster benötigen. Die geschätzte Kosten Cluster wird innerhalb der Blade angezeigt.
  

        ![Knoten Preisgestaltung Ebenen blade] (./media/hdinsight-provision-clusters/HDI.CreateCluster.5.png "Angeben von Anzahl der Clusterknoten")


    * **Optionale Konfiguration** , wählen Sie die Clusterversion sowie andere optionalen Einstellungen wie z. B. Anmelden an einem **Virtuellen Netzwerk**, halten Sie die Daten für die Struktur und Oozie, einem **Externen Metastore** einrichten konfigurieren Skript-Aktionen zu einen Cluster zum Installieren von benutzerdefinierter Komponenten anpassen verwenden, oder verwenden zusätzlichen Speicher-Konten mit dem Cluster.

    * **HDInsight Version**: Wählen Sie die Version, die Sie für den Cluster verwenden möchten. Weitere Informationen finden Sie unter [HDInsight Cluster Versionen](hdinsight-component-versioning.md).
    * **Virtuelle Netzwerk**: Wählen Sie ein Azure-virtuellen Netzwerk und das Subnetz ein, wenn den Cluster in einem virtuellen Netzwerk eingefügt werden sollen.  

        ![Virtuellen Netzwerk blade] (./media/hdinsight-provision-clusters/HDI.CreateCluster.6.png "Angeben virtuelles Netzwerk-details")

        Informationen zum Verwenden von HDInsight mit einem virtuellen Netzwerk, einschließlich Workflowkonfiguration Anforderungen für das virtuelle Netzwerk finden Sie unter [Erweitern HDInsight Capbilities mithilfe einer virtuelle Azure-Netzwerks](hdinsight-extend-hadoop-virtual-network.md).
  

        
    * **Externe Metastores**: Geben Sie eine SQL Azure-Datenbank zum Speichern der Struktur und Oozie Metadaten, die dem Cluster zugeordnet.
 
        > [AZURE.NOTE] Konfiguration von Metastore ist nicht verfügbar für HBase Clustertypen.

    ![Benutzerdefinierte Metastores blade] (./media/hdinsight-provision-clusters/HDI.CreateCluster.7.png "Externe Metastores angeben")

    **Verwenden Sie eine vorhandene SQL-Datenbank für Struktur** Metadaten klicken Sie auf **Ja**, wählen Sie eine SQL­Datenbank, und geben Sie dann das Benutzernamen und das Kennwort für die Datenbank. Wiederholen Sie diese Schritte, wenn Sie, verwenden Sie **Eine vorhandene SQL-Datenbank für Oozie Metadaten möchten**. Klicken Sie auf **auswählen** , verschieben, bis Sie wieder auf das **Optionale Konfiguration** Blade sind.

    >[AZURE.NOTE] Die Azure SQL-Datenbank für die Metastore verwendete muss Verbindung zu anderen Azure-Diensten, einschließlich Azure HDInsight zulassen. Klicken Sie auf den Namen des Servers, auf dem SQL Azure-Datenbank Dashboard, klicken Sie auf der rechten Seite. Dies ist der Server, auf dem die SQL-Datenbank-Instanz ausgeführt wird. Nachdem Sie sind, klicken Sie in der Serveransicht auf **Konfigurieren**, und klicken Sie dann für **Azure Services**, klicken Sie auf **Ja**, und klicken Sie dann auf **Speichern**.

            &nbsp;

            > [AZURE.IMPORTANT] Beim Erstellen eines Metastore verwenden Sie einen Datenbanknamen, der Striche oder Bindestriche, enthält wie dadurch den Erstellungsprozess Cluster fehlschlägt.
        
        * **Script Actions** if you want to use a custom script to customize a cluster, as the cluster is being created. For more information about script actions, see [Customize HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster.md). On the Script Actions blade provide the details as shown in the screen capture.
    

            ![Script action blade](./media/hdinsight-provision-clusters/HDI.CreateCluster.8.png "Specify script action")


        * **Azure Storage Keys**: Specify additional storage accounts to associate with the cluster. In the **Azure Storage Keys** blade, click **Add a storage key**, and then select an existing storage account or create a new account.
    

            ![Additional storage blade](./media/hdinsight-provision-clusters/HDI.CreateCluster.9.png "Specify additional storage accounts")


4. Klicken Sie auf **Erstellen**. Auswahl **an Startboard anheften** wird eine Kachel Cluster im Startboard Ihr Portal hinzufügen. Das Symbol wird laut Cluster erstellt wird, und werden so geändert, dass das HDInsight-Symbol angezeigt werden, nachdem Sie erstellt wurde.
    
    Es dauert einige Zeit für den Cluster, normalerweise ungefähr 15 Minuten erstellt werden. Verwenden Sie die Kachel auf die Startboard oder den Eintrag **Benachrichtigungen** auf der linken Seite der Seite, um auf die Bereitstellung Prozess überprüfen.
    

5. Sobald die Erstellung abgeschlossen ist, klicken Sie auf die Kachel für den Cluster aus der Startboard, um das Blade Cluster zu starten. Das Blade Cluster enthält wichtige Informationen zu den Cluster beispielsweise den Namen, die zugehörige Ressourcengruppe, die Position des Betriebssystems, die URL für den Cluster Dashboard usw. an.


    ![Cluster blade] (./media/hdinsight-provision-clusters/HDI.Cluster.Blade.png "Clustereigenschaften")


    Verwenden Sie die folgenden, um die Symbole oben diese Blade, und klicken Sie im Abschnitt **Essentials** zu verstehen:


    * **Einstellungen** und **Alle**: Zeigt das Blade **Einstellungen** für den Cluster, dem Sie detaillierte Konfigurationsinformationen für den Cluster zugreifen kann.
    * **Dashboard**, **Dashboard Cluster**und **URL**: Dies sind alle Vorgänge auf das Dashboard Cluster zugreifen, die ein Web-Portal Aufträge für den Cluster ausgeführt wird.
    * **Remotedesktop**: ermöglicht es Ihnen, die Cluster-Knoten remote Desktop aktivieren/deaktivieren.
    * **Maßstab Cluster**: ermöglicht es Ihnen, die Anzahl der Worker Knoten für diesen Cluster ändern.
    * **Löschen**: Löscht den Cluster HDInsight.
    * **Schnellstart** (![Cloud und Thunderbolt Symbol Schnellstart =](./media/hdinsight-provision-clusters/quickstart.png)): Zeigt Informationen an, mit denen Sie erste Schritte mit HDInsight.
    * **Benutzer** (![Benutzer Symbol](./media/hdinsight-provision-clusters/users.png)): Festlegen von Berechtigungen für die _Verwaltung der Portalseite_ für diesen Cluster für andere Benutzer Ihres Abonnements Azure ermöglicht.
    

        > [AZURE.IMPORTANT] Diese _nur_ wirkt sich auf Access und Berechtigungen mit diesem Cluster im Portal, und hat keine Auswirkung auf, wer Herstellen einer Verbindung mit oder Aufträge zum Cluster HDInsight senden können.
        
    * **Kategorien** (![Symbol Kategorie](./media/hdinsight-provision-clusters/tags.png)): Kategorien können Sie die Schlüssel/Wert-Paare zum Definieren einer benutzerdefinierten Taxonomie Ihrer Cloud-Dienste zu bestimmen. Sie können beispielsweise erstellen Sie einen Schlüssel mit dem Namen __Project__und verwenden Sie dann einen gemeinsamen Wert für alle Dienste, die einem bestimmten Projekt zugeordnet.

##<a name="customize-clusters"></a>Anpassen der Cluster

- Finden Sie unter [Anpassen HDInsight Cluster Bootstrap verwenden](hdinsight-hadoop-customize-cluster-bootstrap.md).
- Finden Sie unter [Anpassen von Windows-basierten HDInsight Cluster mithilfe der Aktion Skript](hdinsight-hadoop-customize-cluster.md).

##<a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie mehrere Methoden zum Erstellen eines HDInsight Clusters erhalten. Weitere Informationen finden Sie unter den folgenden Artikeln:

* [Erste Schritte mit Azure HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md) – Informationen zum Arbeiten mit Ihren Cluster HDInsight starten
* [Hadoop übermitteln Aufträge programmgesteuert](hdinsight-submit-hadoop-jobs-programmatically.md) - erfahren Sie, wie programmgesteuert Aufträge mit HDInsight senden
* [Verwalten von Hadoop Cluster in HDInsight mithilfe der Azure-Portal](hdinsight-administer-use-management-portal.md)



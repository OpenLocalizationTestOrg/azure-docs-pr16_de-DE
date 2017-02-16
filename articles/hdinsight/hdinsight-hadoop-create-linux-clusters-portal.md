<properties
    pageTitle="Hadoop, HBase, Sturm oder Spark Cluster unter Linux in HDInsight mit dem Portal erstellen | Microsoft Azure"
    description="Erfahren Sie, wie Hadoop, HBase, Sturm oder Spark Cluster unter Linux für HDInsight mithilfe eines Webbrowsers und Azure Preview-Portal zu erstellen."
    services="hdinsight"
    documentationCenter=""
    authors="nitinme"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="10/05/2016"
    ms.author="nitinme"/>


#<a name="create-linux-based-clusters-in-hdinsight-using-the-azure-portal"></a>Erstellen von Linux-basierten Cluster in Verwenden des Portals Azure HDInsight

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Das Azure Portal ist ein Web-basiertes Management-Tool für Dienste und Ressourcen, die in der Cloud Microsoft Azure gehostet wird. In diesem Artikel erfahren Sie, wie Linux-basierten HDInsight Cluster mit dem Portal erstellen.

## <a name="prerequisites"></a>Erforderliche Komponenten

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion Azure abrufen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- __Eine moderne Webbrowser__. Azure-Portal HTML5 und Javascript verwendet, und in älteren Webbrowsern möglicherweise nicht ordnungsgemäß funktioniert.

### <a name="access-control-requirements"></a>Anforderungen für Access-Steuerelement

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

##<a name="create-clusters"></a>Cluster erstellen

Azure-Portal stellt die meisten Clustereigenschaften zur Verfügung. Ressourcenmanager Azure-Vorlage können Sie viele Details ausblenden. Weitere Informationen finden Sie unter [Erstellen Linux-basierten Hadoop Cluster in mithilfe von Vorlagen Ressourcenmanager Azure HDInsight](hdinsight-hadoop-create-linux-clusters-arm-templates.md).

1. Melden Sie sich mit dem [Azure-Portal](https://portal.azure.com)aus.

2. Klicken Sie auf **neu**, klicken Sie auf **Daten Analytics**, und klicken Sie dann auf **HDInsight**.

    ![Erstellen Sie einen neuen Cluster im Azure-Portal] (./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.1.png "Erstellen Sie einen neuen Cluster im Azure-Portal")
3. Geben Sie **Ein Cluster**: dieser Name muss global eindeutig sein.
4. Klicken Sie auf **Cluster Typ auswählen**, und wählen Sie dann aus:

    - **Clustertyp**: Wenn Sie nicht, dass Was wissen auswählen, wählen Sie **Hadoop**aus. Es ist den Clustertyp der am häufigsten verwendeten.

        > [AZURE.IMPORTANT] HDInsight Cluster einer Vielzahl von Typen, die entsprechen den Arbeitsbelastung oder Technologie, die für der Cluster optimiert ist nützlich sein. Es gibt keine unterstützte Methode zum einen Cluster zu erstellen, der mehrere Typen, z. B. Storm und HBase auf einem Cluster kombiniert. 

    - **Betriebssystem**: Wählen Sie **Linux**.
    - **Version**: die Standardversion verwenden, wenn Sie wissen nicht, was auswählen. Weitere Informationen finden Sie unter [HDInsight Cluster Versionen](hdinsight-component-versioning.md).
    - **Cluster Ebene**: Azure HDInsight stellt die Cloud-Angebote big Data in zwei Kategorien: Standard Ebene und Premium Ebene. Weitere Informationen finden Sie unter [Cluster Ebenen](hdinsight-hadoop-provision-linux-clusters.md#cluster-tiers).
    
    ![HDInsight Premium Ebene Konfiguration](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-cluster-type-configuration.png)

4. Klicken Sie auf **Abonnement** zum Auswählen der Azure-Abonnements, die für den Cluster verwendet werden.

5. Klicken Sie auf die **Ressourcengruppe** , um eine vorhandene Ressourcengruppe auszuwählen, oder klicken Sie auf **neu** , um eine neue Ressourcengruppe erstellen

    > [AZURE.NOTE] Dieser Eintrag wird standardmäßig auf eine Ihrer bestehenden Gruppen für die Ressource festgelegt, sofern diese verfügbar sind.

6. Klicken Sie auf **Anmeldeinformationen** , und geben Sie ein Kennwort für den Administratorbenutzer aus. Sie müssen auch eingeben einer **SSH Benutzernamen** und ein **Kennwort** oder **Öffentlicher Schlüssel**, die zum Authentifizieren des Benutzers SSH verwendet wird. Es wird empfohlen, die mit einem öffentlichen Schlüssel. Klicken Sie zum Speichern der Anmeldeinformationen Konfiguration klicken Sie unten auf **auswählen** .

    ![Bereitstellen Cluster Anmeldeinformationen] (./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.3.png "Bereitstellen Cluster Anmeldeinformationen")

    Weitere Informationen zum Verwenden von SSH mit HDInsight finden Sie unter den folgenden Artikeln:

    * [Verwenden von SSH mit Linux-basierten Hadoop auf HDInsight von Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md)
    * [Verwenden von SSH mit Linux-basierten Hadoop auf HDInsight von Windows](hdinsight-hadoop-linux-use-ssh-windows.md)


7. Klicken Sie auf die **Datenquelle** zum Auswählen einer vorhandenen Datenquelle für den Cluster oder erstellen Sie einen neuen.

    ![Datenquellen-blade] (./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.4.png "Bereitstellen Datenquellen-Konfiguration")

    Zurzeit können Sie ein Azure-Speicher-Konto als Datenquelle für einen Cluster HDInsight auswählen. Verwenden Sie die folgenden um zu verstehen, die Einträge in der **Datenquelle** Blade aus.

    - **Auswahlmethode**: Legen Sie den Wert **alle Abonnements** aktivieren Speicherkonten aus allen Ihrer Abonnements zu durchsuchen. Legen Sie den **Zugriffstaste** , wenn Sie die **Storage Name** und **Zugriffstaste** eines vorhandenen Speicher-Kontos eingeben möchten.

    - **Speicher-Konto auswählen / neue**: Klicken Sie auf Durchsuchen, und wählen Sie ein vorhandenes Speicherkonto Cluster zuordnen möchten **Speicherkonto auswählen** . Oder klicken Sie auf **neu** , um ein neues Speicherkonto zu erstellen. Verwenden Sie das Feld, das zur Eingabe des Namens des Speicherkontos angezeigt wird. Wenn der Name verfügbar ist, wird ein grünes Häkchen angezeigt.

    - **Wählen Sie Standardcontainer**: Verwenden Sie diese Option zur Eingabe des Namens des Standardcontainers für Cluster verwendet werden soll. Obwohl Sie hier einen beliebigen Namen eingeben können, empfehlen wir, mit demselben Namen wie des Clusters, damit Sie leicht erkennen können, dass der Container für diesen bestimmten Cluster verwendet wird.

    - **Standort**: der geografische Region, die Speicher-Konto ist, oder erstellt werden.

        > [AZURE.IMPORTANT] Markieren den Speicherort für die Standarddatenquelle wird den Speicherort der Cluster HDInsight einstellen. Die Datenquelle Cluster und Standardwert muss im selben Bereich befinden.
        
    - **Cluster AAD Identität**: konfigurieren ihn aus, indem Sie Sie Cluster zugänglich machen der basierend auf der Konfiguration AAD Azure Daten Lake Stores.

    Klicken Sie auf **auswählen** , um die Datenquellenkonfiguration zu speichern.

8. Klicken Sie auf **Knoten Preise Ebenen** , um Informationen zu den Knoten anzuzeigen, die für diesen Cluster erstellt wird. Legen Sie die Anzahl der Worker-Knoten, die Sie für den Cluster benötigen. Die geschätzte Kosten Cluster wird innerhalb der Blade angezeigt.

    ![Knoten Preisgestaltung Ebenen blade] (./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.5.png "Angeben von Anzahl der Clusterknoten")
    
    > [AZURE.IMPORTANT] Wenn Sie, klicken Sie auf mehr als 32 Worker am Cluster erstellen oder Knoten beabsichtigen durch Skalierung Cluster nach der Erstellung, müssen Sie eine Knotengröße am mit mindestens 8 Kernen und 14 GB Ram auswählen.
    >
    > Weitere Informationen zu Knoten Größen und anfallenden Kosten finden Sie unter [HDInsight Preise](https://azure.microsoft.com/pricing/details/hdinsight/).

    Klicken Sie auf **auswählen** , um die Knoten Preisgestaltung Konfiguration speichern.

9. Klicken Sie auf **Optionale Konfiguration** , damit wählen Sie die Clusterversion sowie andere optionalen Einstellungen wie beitreten zu einem **Virtuellen Netzwerk**konfigurieren, Einrichten eines **Externen Metastore** Daten enthalten für Struktur und Oozie, Skript-Aktionen zu einen Cluster zum Installieren von benutzerdefinierter Komponenten anpassen verwenden oder verwenden Sie zusätzlichen Speicher-Konten mit dem Cluster.

    * **Virtuelle Netzwerk**: Wählen Sie ein Azure-virtuellen Netzwerk und das Subnetz ein, wenn den Cluster in einem virtuellen Netzwerk eingefügt werden sollen.  

        ![Virtuellen Netzwerk blade] (./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.6.png "Angeben virtuelles Netzwerk-details")

        Informationen zum Verwenden von HDInsight mit einem virtuellen Netzwerk, einschließlich Workflowkonfiguration Anforderungen für das virtuelle Netzwerk finden Sie unter [-Funktionen erweitern HDInsight mithilfe einer Azure-virtuellen Netzwerk](hdinsight-extend-hadoop-virtual-network.md).

    * Klicken Sie auf **Externe Metastores** zum SQL-Datenbank angeben, die Sie verwenden, um die Struktur und Oozie Metadaten, die dem Cluster zugeordnet speichern möchten.
    
        > [AZURE.NOTE] Konfiguration von Metastore ist nicht verfügbar für HBase Clustertypen.

        ![Benutzerdefinierte Metastores blade] (./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.7.png "Externe Metastores angeben")

        **Verwenden Sie eine vorhandene SQL-Datenbank für Struktur** Metadaten klicken Sie auf **Ja**, wählen Sie eine SQL­Datenbank, und geben Sie dann das Benutzernamen und das Kennwort für die Datenbank. Wiederholen Sie diese Schritte, wenn Sie, verwenden Sie **Eine vorhandene SQL-Datenbank für Oozie Metadaten möchten**. Klicken Sie auf **auswählen** , verschieben, bis Sie wieder auf das **Optionale Konfiguration** Blade sind.

        >[AZURE.NOTE] Die Azure SQL-Datenbank für die Metastore verwendete muss Verbindung zu anderen Azure-Diensten, einschließlich Azure HDInsight zulassen. Klicken Sie auf den Namen des Servers, auf dem SQL Azure-Datenbank Dashboard, klicken Sie auf der rechten Seite. Dies ist der Server, auf dem die SQL-Datenbank-Instanz ausgeführt wird. Nachdem Sie sind, klicken Sie in der Serveransicht auf **Konfigurieren**, und klicken Sie dann für **Azure Services**, klicken Sie auf **Ja**, und klicken Sie dann auf **Speichern**.

        &nbsp;

        > [AZURE.IMPORTANT] Beim Erstellen eines Metastore verwenden Sie einen Datenbanknamen, der Striche oder Bindestriche, enthält wie dadurch den Erstellungsprozess Cluster fehlschlägt.

    * **Skript-Aktionen** , wenn Sie ein benutzerdefiniertes Skript zu verwenden, um einem Cluster anpassen möchten wird als Cluster erstellt wird. Weitere Informationen zu Skriptaktionen finden Sie unter [Anpassen HDInsight Cluster mithilfe der Aktion Skript](hdinsight-hadoop-customize-cluster-linux.md). Geben Sie die Details auf das Skriptaktionen Blade wie auf dem Bildschirmfoto dargestellt.

        ![Die Aktion Blade Skript] (./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.8.png "Skript für Aktion angeben")

    * Klicken Sie auf **Verknüpfte Speicherkonten** zum Angeben zusätzlicher Speicher-Konten, mit dem Cluster zugeordnet werden soll. Klicken Sie in das Blade **Azure Speicher Tasten** auf **Hinzufügen einer Speicher-Taste**, und wählen Sie ein vorhandenes Speicherkonto oder erstellen Sie ein neues Konto.

        ![Zusätzlicher Speicher blade] (./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.9.png "Zusätzlicher Speicher-Konten angeben")

        Sie können auch die zusätzlichen Speicher-Konten hinzufügen, nachdem ein Cluster erstellt wurde.  Finden Sie unter [Anpassen Linux-basierten HDInsight Cluster mithilfe der Aktion Skript](hdinsight-hadoop-customize-cluster-linux.md).

        Klicken Sie auf **auswählen** , verschieben, bis Sie wieder auf das **neue HDInsight Cluster** Blade sind.
        
        Zusätzlich zu BLOB-Speicher-Konto können Sie zudem Azure Daten Lake Stores verknüpfen. Die Konfiguration kann sein durch Konfigurieren AAD aus der Datenquelle, in dem Sie die Standardkonto-Speicher und die standardmäßige Container konfiguriert.

10. In der **Neuen HDInsight Cluster** Blade stellen Sie sicher, dass **an Startboard anheften** ausgewählt ist, und klicken Sie dann auf **Erstellen**. Diese Cluster erstellen und hinzufügen eine Kachel dafür zu den Startboard von Ihrer Azure-Portal. Das Symbol wird mitgeteilt wird, dass der Cluster ist die Bereitstellung von ändert sich das Symbol HDInsight anzeigen nach Abschluss der Bereitstellung.

  	| Während der Bereitstellung | Bereitstellung abgeschlossen |
  	| ------------------ | --------------------- |
  	| ![Indikator für Startboard bereitgestellt](./media/hdinsight-hadoop-create-linux-cluster-portal/provisioning.png) | ![Bereitgestellte-Kachel](./media/hdinsight-hadoop-create-linux-cluster-portal/provisioned.png) |

    > [AZURE.NOTE] Es dauert einige Zeit für den Cluster, normalerweise ungefähr 15 Minuten erstellt werden. Verwenden Sie die Kachel auf die Startboard oder den Eintrag **Benachrichtigungen** auf der linken Seite der Seite, um auf die Bereitstellung Prozess überprüfen.

11. Nachdem Sie der Erstellungsprozess abgeschlossen ist, klicken Sie auf die Kachel für den Cluster aus der Startboard, um das Blade Cluster zu starten. Das Blade Cluster enthält wichtige Informationen zu den Cluster beispielsweise den Namen, die zugehörige Ressourcengruppe, die Position des Betriebssystems, die URL für den Cluster Dashboard usw. an.

    ![Cluster blade] (./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.Cluster.Blade.png "Clustereigenschaften")

    Verwenden Sie die folgenden, um die Symbole oben diese Blade, und klicken Sie im Abschnitt **Essentials** zu verstehen:

    * **Einstellungen** und **Alle**: Zeigt das Blade **Einstellungen** für den Cluster, dem Sie detaillierte Konfigurationsinformationen für den Cluster zugreifen kann.

    * **Dashboard**, **Dashboard Cluster**und **URL**: Dies sind alle Vorgänge auf das Dashboard Cluster zugreifen, die ein Web-Portal Aufträge für den Cluster ausgeführt wird.

    * **Secure Shell**: Informationen für den Zugriff auf den Cluster mithilfe von SSH erforderlich.

    * **Löschen**: Löscht den Cluster HDInsight.

    * **Schnellstart** (![Cloud und Thunderbolt Symbol Schnellstart =](./media/hdinsight-hadoop-create-linux-cluster-portal/quickstart.png)): Zeigt Informationen an, mit denen Sie erste Schritte mit HDInsight.

    * **Benutzer** (![Benutzer Symbol](./media/hdinsight-hadoop-create-linux-cluster-portal/users.png)): Festlegen von Berechtigungen für die _Verwaltung der Portalseite_ für diesen Cluster für andere Benutzer Ihres Abonnements Azure ermöglicht.

        > [AZURE.IMPORTANT] Diese _nur_ wirkt sich auf Access und Berechtigungen mit diesem Cluster im Azure-Portal, und hat keine Auswirkung auf, wer Herstellen einer Verbindung mit oder Aufträge zum Cluster HDInsight senden können.

    * **Kategorien** (![Symbol Kategorie](./media/hdinsight-hadoop-create-linux-cluster-portal/tags.png)): Kategorien können Sie die Schlüssel/Wert-Paare zum Definieren einer benutzerdefinierten Taxonomie Ihrer Cloud-Dienste zu bestimmen. Sie können beispielsweise erstellen Sie einen Schlüssel mit dem Namen __Project__und verwenden Sie dann einen gemeinsamen Wert für alle Dienste, die einem bestimmten Projekt zugeordnet.

##<a name="customize-clusters"></a>Anpassen der Cluster

- Finden Sie unter [Anpassen HDInsight Cluster Bootstrap verwenden](hdinsight-hadoop-customize-cluster-bootstrap.md).
- Finden Sie unter [Anpassen Linux-basierten HDInsight Cluster mithilfe der Aktion Skript](hdinsight-hadoop-customize-cluster-linux.md).

##<a name="delete-the-cluster"></a>Löschen des Clusters

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

##<a name="next-steps"></a>Nächste Schritte

Jetzt, da Sie einen HDInsight Cluster erfolgreich erstellt haben, verwenden Sie die folgenden Informationen zum Arbeiten mit Ihren Cluster:

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

###<a name="spark-clusters"></a>Spark Cluster

* [Erstellen Sie eine eigenständige Anwendung Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Führen Sie Aufträge Remote auf einem Spark Cluster Livius verwenden](hdinsight-apache-spark-livy-rest-interface.md)
* [Spark mit BI: Ausführen interaktiven Datenanalyse mithilfe von Spark in HDInsight mit BI-Tools](hdinsight-apache-spark-use-bi-tools.md)
* [Spark mit maschinellen Schulung: verwenden Spark in HDInsight Lebensmittel Prüfungsergebnissen Vorhersagen](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Spark Streaming: Verwenden Sie Spark in HDInsight zum Erstellen von in Echtzeit streaming Clientanwendungen](hdinsight-apache-spark-eventhub-streaming.md)

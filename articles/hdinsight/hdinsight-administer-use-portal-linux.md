<properties
    pageTitle="Verwalten von Linux-basierten Hadoop Cluster in HDInsight mithilfe von Azure-Portal | Microsoft Azure"
    description="Informationen Sie zum Verwenden des Portals Azure HDInsight Linux-basierten-Cluster erstellen und verwalten."
    services="hdinsight"
    documentationCenter=""
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="jgao"/>

#<a name="manage-hadoop-clusters-in-hdinsight-by-using-the-azure-portal"></a>Verwalten von Hadoop Cluster in HDInsight mithilfe des Azure-Portals

[AZURE.INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Verwenden des [Azure-Portal][azure-portal], können Sie in Azure HDInsight Cluster Linux-basierten verwalten. Verwenden Sie das tabstoppauswahlsymbol Informationen zum Erstellen von Hadoop Cluster in HDInsight mit anderen Tools aus. 

**Erforderliche Komponenten**

Vorbemerkung in diesem Artikel müssen Sie Folgendes:

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion Azure abrufen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

##<a name="open-the-portal"></a>Öffnen Sie das Portal

1. Melden Sie sich bei [https://portal.azure.com](https://portal.azure.com).
2. Nachdem Sie das Portal geöffnet haben, können Sie:

    - Klicken Sie auf **neu** aus dem linken Menü, um einen neuen Cluster zu erstellen:
    
        ![Schaltfläche ' neu HDInsight Cluster '](./media/hdinsight-administer-use-portal-linux/azure-portal-new-button.png)
    - Klicken Sie auf **HDInsight Cluster** aus dem linken Menü aus, um die vorhandenen Cluster Liste
    
        ![Azure Portals HDInsight Cluster-Schaltfläche](./media/hdinsight-administer-use-portal-linux/azure-portal-hdinsight-button.png)

        Wenn **HDInsight** im linken Menü angezeigt wird, klicken Sie auf **Durchsuchen**, und klicken Sie dann auf **HDInsight Cluster**.

        ![Azure Portals durchsuchen Cluster buttomn](./media/hdinsight-administer-use-portal-linux/azure-portal-browse-button.png)

##<a name="create-clusters"></a>Cluster erstellen

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

HDInsight funktioniert mit einer Breite von Hadoop-Komponenten. Für die Liste der Komponenten, die überprüft und unterstützt wurden, finden Sie unter [welche Version Hadoop in Azure HDInsight](hdinsight-component-versioning.md). Die Informationen über allgemeine Cluster Erstellungsdatum finden Sie unter [Erstellen Hadoop Cluster in HDInsight](hdinsight-hadoop-provision-linux-clusters.md). 

##<a name="list-and-show-clusters"></a>Hinzufügen und Anzeigen von Cluster

1. Melden Sie sich bei [https://portal.azure.com](https://portal.azure.com).
2. Klicken Sie auf **HDInsight Cluster** aus dem linken Menü vorhandener Cluster aufgelistet.
3. Klicken Sie auf den Clusternamen. Wenn die Liste Cluster lang ist, können Sie Filter am oberen Rand der Seite.
4. Doppelklicken Sie auf einem Cluster aus der Liste aus, um die Details anzuzeigen.

    **Menü und Essentials**:

    ![Azure Portals Hdinsight Cluster essentials](./media/hdinsight-administer-use-portal-linux/hdinsight-essentials.png)
    
    - **Einstellungen** und **Alle**: Zeigt das Blade **Einstellungen** für den Cluster, dem Sie detaillierte Konfigurationsinformationen für den Cluster zugreifen kann.
    - **Dashboard**, **Dashboard Cluster** und ** URL: Dies sind alle Vorgänge auf das Dashboard Cluster zugreifen, die Ambari Web für Linux-basierten Cluster ist.
    - **Secure Shell**: Zeigt die Anweisungen, um zum Cluster mithilfe von Secure Shell (SSH) Verbindung herstellen können.
    - **Maßstab Cluster**: ermöglicht es Ihnen, die Anzahl der Worker Knoten für diesen Cluster ändern.
    - **Löschen**: Löscht den Cluster.
    - **Schnellstart (![Cloud und Thunderbolt Symbol Schnellstart =](./media/hdinsight-administer-use-portal-linux/quickstart.png))**: Zeigt Informationen an, mit denen Sie erste Schritte mit HDInsight.
    - **Benutzer (![Benutzer Symbol](./media/hdinsight-administer-use-portal-linux/users.png))**: Festlegen von Berechtigungen für die _Verwaltung der Portalseite_ für diesen Cluster für andere Benutzer Ihres Abonnements Azure ermöglicht.
    
        > [AZURE.IMPORTANT] Diese _nur_ wirkt sich auf Access und Berechtigungen mit diesem Cluster im Azure-Portal, und hat keine Auswirkung auf, wer Herstellen einer Verbindung mit oder Aufträge zum Cluster HDInsight senden können.
    - **Kategorien (![Symbol Kategorie](./media/hdinsight-administer-use-portal-linux/tags.png))**: Kategorien können Sie die Schlüssel/Wert-Paare zum Definieren einer benutzerdefinierten Taxonomie Ihrer Cloud-Dienste zu bestimmen. Sie können beispielsweise erstellen Sie einen Schlüssel mit dem Namen __Project__und verwenden Sie dann einen gemeinsamen Wert für alle Dienste, die einem bestimmten Projekt zugeordnet.
    - **Ambari Ansichten**: Links zu Ambari Web.
    
    > [AZURE.IMPORTANT] Sie müssen zum Verwalten der Dienste von HDInsight Cluster Ambari Web oder die Ambari REST-API verwenden. Weitere Informationen zur Verwendung von Ambari finden Sie unter [Verwalten von HDInsight Cluster Ambari verwenden](hdinsight-hadoop-manage-ambari.md).

    **Verwendung**:
    
    ![Verwendung von Azure Portals Hdinsight cluster](./media/hdinsight-administer-use-portal-linux/hdinsight-portal-cluster-usage.png)
    
5. Klicken Sie auf **Einstellungen**.

    ![Verwendung von Azure Portals Hdinsight cluster](./media/hdinsight-administer-use-portal-linux/hdinsight.portal.cluster.settings.png)

    - **Überwachungsprotokolle**:
    - **Schnellstart**: Zeigt Informationen an, mit denen Sie erste Schritte mit HDInsight.
    - **Maßstab Cluster**: erhöhen und Verringern der Anzahl der Cluster Worker Knoten.
    - **Secure Shell**: Zeigt die Anweisungen, um zum Cluster mithilfe von Secure Shell (SSH) Verbindung herstellen können.
    - **HDInsight Partner**: der aktuellen HDInsight Partner hinzufügen/entfernen.
    - **Externe Metastores**: Anzeigen der Struktur und Oozie Metastores. Die Metastores kann nur beim Erstellen der Cluster konfiguriert werden.
    - **Skript-Aktionen**: Ausführen Bash Skripts auf dem Cluster.
    - **Eigenschaften**: Anzeigen der Clustereigenschaften.
    - **Tasten der Azure-Speicher**: Anzeigen des Standardkontos für den Speicher und den zugehörigen Schlüssel. Das Speicherkonto ist Konfiguration beim Erstellen der Cluster.
    - **Cluster AAD Identität**: 
    - **Benutzer**: Festlegen von Berechtigungen für die _Verwaltung der Portalseite_ für diesen Cluster für andere Benutzer Ihres Abonnements Azure ermöglicht.
    - **Tags**: Kategorien können Sie die Schlüssel/Wert-Paare zum Definieren einer benutzerdefinierten Taxonomie Ihrer Cloud-Dienste zu bestimmen. Sie können beispielsweise erstellen Sie einen Schlüssel mit dem Namen __Project__und verwenden Sie dann einen gemeinsamen Wert für alle Dienste, die einem bestimmten Projekt zugeordnet.
    
    > [AZURE.NOTE] Dies ist eine generische Liste von Einstellungen zur Verfügung. nicht alle wird für alle Clustertypen vorhanden sein.

6. Klicken Sie auf **Eigenschaften**:

    Die Eigenschaften sind:
    
    - **Hostname**: Cluster-Name.
    - **Cluster-URL**.
    - **Status**: einschließen abgebrochen, akzeptiert, ClusterStorageProvisioned, AzureVMConfiguration, HDInsightConfiguration, Betrieb, ausgeführt, Fehler, löschen, gelöscht haben, Timedout DeleteQueued DeleteTimedout DeleteError PatchQueued CertRolloverQueued ResizeQueued, ClusterCustomization
    - **Region**: Azure-Speicherort. Eine Liste der unterstützten Azure Orte finden Sie unter **Region** Dropdown-Listenfeld auf [HDInsight Preise](https://azure.microsoft.com/pricing/details/hdinsight/).
    - **Daten erstellt**.
    - **Betriebssystem**: entweder **Windows** oder **Linux**.
    - **Type**: Hadoop, HBase, Sturm, aufzeigen. 
    - **Version**. Finden Sie unter [HDInsight Versionen](hdinsight-component-versioning.md)
    - **Abonnements**: Abonnementname.
    - **Abonnement-ID**.
    - **Standard-Datenquelle**: die Standardeinstellung Cluster-Dateisystem.
    - **Worker Knoten Preise Ebene**.
    - **Kopf Knoten Preisgestaltung Ebene**.

##<a name="delete-clusters"></a>Cluster löschen

Löschen einer Cluster Löschen des Standardkontos für den Speicher oder alle Speicherkonten verknüpfte nicht. Sie können den Cluster neu erstellen, mit den gleichen Speicherkonten und die gleichen Metastores. Es wird empfohlen, einen neuen Standard Blob Container verwenden, wenn Sie den Cluster neu erstellen.

1. Melden Sie sich mit dem [Portal][azure-portal].
2. Klicken Sie im Menü links auf **Alle durchsuchen** , klicken Sie auf **HDInsight Cluster**, klicken Sie auf Ihren Clusternamen.
3. Klicken Sie im oberen Menü auf **Löschen** , und folgen Sie dann die Anweisungen.

Siehe auch [Anhalten/Cluster beenden](#pauseshut-down-clusters).

##<a name="scale-clusters"></a>Maßstab Cluster
Cluster Skalierung Feature ermöglicht Ihnen, ändern die Anzahl der Worker-Knoten verwendet, die für einen Cluster, der in Azure HDInsight ausgeführt wird, ohne den Cluster erneut erstellen.

>[AZURE.NOTE] Nur mit HDInsight Version 3.1.3 Cluster oder höher werden unterstützt. Wenn Sie die Version von Ihren Cluster kennen, können Sie auf die Seite Eigenschaften überprüfen.  Finden Sie unter [Liste und anzeigen Cluster](#list-and-show-clusters).

Die Auswirkung zum Ändern der Anzahl der Datenknoten für jede Art von Cluster von HDInsight unterstützt:

- Hadoop

    Sie können nahtlos die Anzahl der Worker-Knoten in einem Cluster Hadoop erhöhen, die ohne zu beeinträchtigen Ausstehend oder laufenden Aufträge ausgeführt wird. Neue Aufträge können auch gesendet werden, während der Vorgang ausgeführt wird. Fehler bei einer Skalierung sind ordnungsgemäß behandelt, sodass der Cluster funktionsfähig immer offen steht.

    Wenn ein Cluster Hadoop nach unten durch Verringern der Anzahl der Datenknoten skaliert ist, werden einige der Dienste im Cluster neu gestartet. Dadurch alle laufenden und anstehende Aufträge nach Abschluss des Vorgangs Skalierung fehlschlägt. Sie können, jedoch die Aufträge erneut, nachdem der Vorgang abgeschlossen ist.

- HBase

    Sie können nahtlos hinzufügen oder Entfernen von Knoten zum Cluster HBase während der Ausführung. Innerhalb weniger Minuten der Abschluss der Skalierung Operation sind automatisch Landes-/ Server ausgelastet. Sie können jedoch auch manuell die regionalen Server Saldo, nach in der Headnode der Cluster Protokollierung, und führen die folgenden Befehle aus, ein Eingabeaufforderungsfenster:

        >pushd %HBASE_HOME%\bin
        >hbase shell
        >balancer

    Weitere Informationen zur Verwendung der HBase-Verwaltungsshell finden Sie unter]
- Storm

    Nahtloses können Sie hinzufügen oder Entfernen von Datenknoten zum Cluster Storm während der Ausführung. Müssen Sie jedoch nach einer erfolgreichen Abschluss des Vorgangs Skalierung, wird der Suchtopologie neu zu verteilen.

    Qualifikationsprofilen kann auf zwei Arten erreicht werden:

    * Storm Web-Benutzeroberfläche
    * Tool line Interface (CLI)

    Lizenzinformationen finden Sie in der [Dokumentation Apache Storm](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) Weitere Details.

    Das Storm Web-Benutzeroberfläche steht im Cluster HDInsight zur Verfügung:

    ![Hdinsight Storm skalieren rebalance](./media/hdinsight-administer-use-portal-linux/hdinsight.portal.scale.cluster.storm.rebalance.png)

    Hier ist ein Beispiel, wie Sie mithilfe des Befehls CLI um der Suchtopologie Storm neu zu verteilen:

        ## Reconfigure the topology "mytopology" to use 5 worker processes,
        ## the spout "blue-spout" to use 3 executors, and
        ## the bolt "yellow-bolt" to use 10 executors

        $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10


**Zum Skalieren Cluster**

1. Melden Sie sich mit dem [Portal][azure-portal].
2. Klicken Sie im Menü links auf **Alle durchsuchen** , klicken Sie auf **HDInsight Cluster**, klicken Sie auf Ihren Clusternamen.
3. Klicken Sie im oberen Menü auf **Einstellungen** , und klicken Sie dann auf **Farben-Skala Cluster**.
4. Geben Sie die **Anzahl der Worker-Knoten**. Die maximale Anzahl der Cluster-Knoten variiert zwischen Azure-Abonnements. Sie können den Support zum Erhöhen des Grenzwert kontaktieren.  Die Kosteninformationen werden die Änderungen wirken sich aus, die Sie in die Anzahl der Knoten vorgenommen haben.

    ![Hdinsight Hadoop Hbase Storm Spark skalieren](./media/hdinsight-administer-use-portal-linux/hdinsight.portal.scale.cluster.png)

##<a name="pauseshut-down-clusters"></a>Beenden Sie anhalten/Cluster

Die meisten Hadoop Aufträge sind, die Stapelverarbeitung abgeschlossen, die nur gelegentlich ausgeführt haben. Für die meisten Hadoop Cluster gibt es großen Zeiträume, die der Cluster nicht für die Verarbeitung verwendet wird. Mit HDInsight Ihre Daten in Azure-Speicher gespeichert, sodass Sie problemlos einen Cluster löschen können, wenn es nicht verwendet wird.
Sie unterliegen auch nach einem HDInsight Cluster, auch wenn es nicht verwendet wird. Da die Gebühren für den Cluster oft mehr als die Gebühren für Speicher sind, ist es economic sinnvoll Cluster löschen, wenn er nicht verwendet werden.

Es gibt viele Möglichkeiten, die Sie den Prozess Programmierung verwenden können:

- Benutzer Azure Daten Factory. Finden Sie unter [Erstellen auf Anforderung Hadoop Linux-basierten Cluster in HDInsight mithilfe von Azure Data Factory](hdinsight-hadoop-create-linux-clusters-adf.md) zum Erstellen von Diensten für bei Bedarf HDInsight verknüpft.
- Verwenden von Azure PowerShell.  Finden Sie unter [Analysieren Verzögerung Flugdaten](hdinsight-analyze-flight-delay-data.md).
- Verwenden von Azure CLI. Finden Sie unter [Verwalten HDInsight Cluster Azure CLI verwenden](hdinsight-administer-use-command-line.md).
- Verwenden Sie HDInsight .NET SDK. Finden Sie unter [Senden Hadoop Aufträge](hdinsight-submit-hadoop-jobs-programmatically.md).

Die Preisinformationen finden Sie unter [HDInsight Preise](https://azure.microsoft.com/pricing/details/hdinsight/). Um einen Cluster aus dem Portal löschen möchten, finden Sie unter [Cluster löschen](#delete-clusters)

##<a name="change-passwords"></a>Ändern von Kennwörtern

Ein HDInsight Cluster kann zwei Benutzerkonten verfügen. Die HDInsight cluster-Benutzerkonto (auch bekannt als HTTP-Benutzerkonto) und das Benutzerkonto SSH beim Erstellen der erstellt werden. Sie können im Web Ambari Benutzeroberfläche zum Ändern der Cluster Benutzer Benutzername und Kennwort ein, und das Skriptaktionen so ändern Sie das Benutzerkonto SSH

###<a name="change-the-cluster-user-password"></a>Ändern Sie das Kennwort für Benutzer

Der Web-Benutzeroberfläche Ambari können Sie das Kennwort für Benutzer ändern. Wenn Sie sich bei Ambari, müssen Sie die vorhandenen Cluster-Benutzernamen und Ihr Kennwort verwenden.

> [AZURE.NOTE] Wenn Sie das Kennwort für Benutzer (Administrator) ändern, kann dies Skript führen, die Aktionen für diesen Cluster zum Fehlschlagen ausgeführt haben. Wenn Sie die Ziel-Knoten Worker Skriptaktionen dauerhaften haben, können diese fehlschlagen, wenn Sie, dass Knoten zum Cluster durch Ändern der Größe Vorgänge hinzufügen. Weitere Informationen zum Skript-Aktionen finden Sie unter [Anpassen HDInsight Cluster mit Skript-Aktionen](hdinsight-hadoop-customize-cluster-linux.md).

1. Melden Sie sich an die Ambari Web-Benutzeroberfläche mit den Anmeldeinformationen HDInsight Cluster aus. Der Standard-Benutzername ist **Admin**. Die URL ist **https://&lt;HDInsight Cluster Name > azurehdinsight.net**.
2. Klicken Sie im oberen Menü auf **Administrator** , und klicken Sie dann auf "Ambari verwalten". 
3. Klicken Sie auf **Benutzer**, wählen Sie im Menü links.
4. Klicken Sie auf **Administrator**.
5. Klicken Sie auf **Kennwort ändern**.

Ambari ändert dann das Kennwort auf allen Knoten im Cluster.

###<a name="change-the-ssh-user-password"></a>Ändern des Kennworts des SSH Benutzer

1. Mit einem Text-Editor, speichern Sie Folgendes in einer Datei namens __changepassword.sh__.

    > [AZURE.IMPORTANT] Sie müssen einen Editor verwenden, der als das Ende der Zeile LF verwendet wird. Wenn der Editor CRLF verwendet, funktioniert das Skript nicht.
    
        #! /bin/bash
        USER=$1
        PASS=$2

        usermod --password $(echo $PASS | openssl passwd -1 -stdin) $USER

2. Laden Sie die Datei an einem Speicherort, der aus HDInsight mithilfe der Adresse HTTP oder HTTPS zugegriffen werden kann. Beispielsweise eine öffentliche Datei speichern, wie etwa OneDrive oder Azure Blob-Speicher. Speichern des URIS (HTTP oder HTTPS-Adresse,) zu der Datei, wie dies, in dem nächsten Schritt fort erforderlich ist.

3. Vom Azure-Portal wählen Sie Ihren Cluster HDInsight aus, und wählen Sie __Alle Einstellungen__. Wählen Sie aus dem Blade __Einstellungen__ __Skript-Aktionen__aus.

4. Wählen Sie das __Skriptaktionen__ Blade __Neue senden__aus. Wenn das Blade __Absenden Skriptaktion__ angezeigt wird, geben Sie die folgenden Informationen ein.

  	| Feld | Wert |
  	| ----- | ----- |
  	| Namen | Ssh Kennwort ändern |
  	| Bash Skript URI | Der URI zu der Datei changepassword.sh |
  	| Knoten (Kopf, Worker, Nimbus, Vorgesetzten, Zookeeper usw.) | ✓ für alle Typen von Knoten aufgeführt |
  	| Parameter | Geben Sie den Benutzernamen SSH, und klicken Sie dann das neue Kennwort ein. Es sollte ein Leerzeichen zwischen den Benutzernamen und das Kennwort ein.
  	| Beibehalten werden Sie diese Skriptaktion... | Lassen Sie dieses Feld deaktiviert.

5. Wählen Sie __Erstellen__ , um das Skript anzuwenden. Nachdem Sie das Skript abgeschlossen ist, werden Sie für die Verbindung mit dem Cluster über SSH mit dem neuen Kennwort sein.

##<a name="grantrevoke-access"></a>Zugriff gewähren/widerrufen

HDInsight Cluster stehen die folgenden HTTP-Webdienste (alle der folgenden Dienste verwenden REST-Endpunkten):

- ODBC
- JDBC
- Ambari
- Oozie
- Templeton

Standardmäßig werden diese Dienste für den Zugriff erteilt. Sie können widerrufen/mithilfe von [Azure CLI](hdinsight-administer-use-command-line.md#enabledisable-http-access-for-a-cluster) und [Azure PowerShell](hdinsight-administer-use-powershell.md#grantrevoke-access)Zugriff gewähren.

##<a name="find-the-subscription-id"></a>Suchen Sie die Abonnement-ID

**Zum Suchen der Azure-Abonnement-IDs**

1. Melden Sie sich mit dem [Portal][azure-portal].
2. Klicken Sie im Menü links auf **Alle durchsuchen** , und klicken Sie dann auf **Abonnements**. Jedes Abonnement hat einen Namen und eine ID ein.

Jeder Cluster ist zu einem Azure-Abonnement verknüpft. Die Abonnement-ID wird auf dem Cluster **grundlegende** Kachel angezeigt. Finden Sie unter [Liste und anzeigen Cluster](#list-and-show-clusters).

##<a name="find-the-resource-group"></a>Suchen nach der Ressourcengruppe 

In der Cloud-Modus wird jede HDInsight Cluster mit einer Azure Ressourcengruppe erstellt. Die Azure Ressourcengruppe, der zu ein Cluster gehört, wird in:

- Die Liste Cluster verfügt über eine Spalte **Ressourcengruppe** .
- **Grundlegende** Cluster-Kachel.  

Finden Sie unter [Liste und anzeigen Cluster](#list-and-show-clusters).


##<a name="find-the-default-storage-account"></a>Suchen des Standardkontos-Speicher

Jeder Cluster HDInsight verfügt über kein Speicher Standardkonto. Das Standardkonto für den Speicher und deren Schlüssel für einen Cluster angezeigt wird, klicken Sie unter **Einstellungen**/**Eigenschaften**/**Azure-Speicher-Taste**. Finden Sie unter [Liste und anzeigen Cluster](#list-and-show-clusters).


##<a name="run-hive-queries"></a>Ausführen von Abfragen Struktur

Sie können keine Stapelverarbeitung Struktur direkt vom Azure-Portal, aber Sie können die Struktur Ansicht auf Ambari Web-Benutzeroberfläche verwenden.

**Zum Ausführen der Struktur Abfragen mit Ambari Struktur anzeigen**

1. Melden Sie sich an die Ambari Web-Benutzeroberfläche mit den Anmeldeinformationen HDInsight Cluster aus. Die Installationsgruppe Benutzername ist **Admin**. Die URL ist **https://&lt;HDInsight Cluster Name > azurehdinsight.net**.
2. Öffnen Sie Struktur anzeigen, wie im folgenden Screenshot gezeigt:  

    ![Hdinsight Struktur anzeigen](./media/hdinsight-administer-use-portal-linux/hdinsight-hive-view.png)
3. Klicken Sie im oberen Menü auf **Abfrage** .
4. Geben Sie eine Struktur Abfrage im **Abfrage-Editor**, und klicken Sie dann auf **Ausführen**.

##<a name="monitor-jobs"></a>Monitor Aufträge

Finden Sie unter [Verwalten von HDInsight Cluster mithilfe der Web-Benutzeroberfläche Ambari](hdinsight-hadoop-manage-ambari.md#monitoring).

##<a name="browse-files"></a>Durchsuchen von Dateien

Verwenden des Azure-Portals an, können Sie den Inhalt des Standardcontainers navigieren.

1. Melden Sie sich bei [https://portal.azure.com](https://portal.azure.com).
2. Klicken Sie auf **HDInsight Cluster** aus dem linken Menü vorhandener Cluster aufgelistet.
3. Klicken Sie auf den Clusternamen. Wenn die Liste Cluster lang ist, können Sie Filter am oberen Rand der Seite.
4. Klicken Sie auf **Einstellungen**.
5. Klicken Sie auf **Einstellungen** Blade **Azure-Speicher-Taste**.
6. Klicken Sie auf den Namen des Kontos Standard-Speicher.
7. Klicken Sie auf die Kachel **Blobs** .
8. Klicken Sie auf den Container Standardnamen.


##<a name="monitor-cluster-usage"></a>Monitor Cluster Verwendung

Im Abschnitt __Verwendung__ des Blades Cluster HDInsight zeigt Informationen über die Anzahl der verfügbaren Kerne für Ihr Abonnement für die Verwendung mit HDInsight als auch die Anzahl der Kerne reserviert diesen Cluster und wie sie für die Knoten in diesem Cluster zugewiesen sind. Finden Sie unter [Liste und anzeigen Cluster](#list-and-show-clusters).

> [AZURE.IMPORTANT] Zum Überwachen der Dienste von HDInsight Cluster, müssen Sie Ambari Web oder die Ambari REST-API verwenden. Weitere Informationen zur Verwendung von Ambari finden Sie unter [Verwalten von HDInsight Cluster mit Ambari](hdinsight-hadoop-manage-ambari.md)

##<a name="connect-to-a-cluster"></a>Verbinden mit einem cluster

Finden Sie unter [Struktur mit Hadoop in HDInsight mit SSH verwenden](hdinsight-hadoop-use-hive-ssh.md#ssh).
    
##<a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie so erstellen Sie einen Cluster HDInsight mithilfe des Portals, und wie Sie das Befehlszeile Hadoop-Verwaltungstool geöffnet haben gelernt. Weitere Informationen finden Sie unter den folgenden Artikeln:

* [Verwalten Sie mithilfe der PowerShell Azure HDInsight](hdinsight-administer-use-powershell.md)
* [Verwalten von mit CLI Azure HDInsight](hdinsight-administer-use-command-line.md)
* [HDInsight Cluster erstellen](hdinsight-provision-clusters.md)
* [Verwenden Sie die Struktur in HDInsight](hdinsight-use-hive.md)
* [Schwein in HDInsight verwenden](hdinsight-use-pig.md)
* [Verwenden von Sqoop in HDInsight](hdinsight-use-sqoop.md)
* [Erste Schritte mit Azure HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md)
* [Welche Version von Hadoop Azure HDInsight wird?](hdinsight-component-versioning.md)

[azure-portal]: https://portal.azure.com
[image-hadoopcommandline]: ./media/hdinsight-administer-use-portal-linux/hdinsight-hadoop-command-line.png "Hadoop Befehlszeile"

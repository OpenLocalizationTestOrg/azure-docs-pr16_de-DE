<properties
   pageTitle="Hadoop Lernprogramm: Erste Schritte mit Hadoop auf Windows | Microsoft Azure"
   description="Erste Schritte mit Hadoop in HDInsight. Informationen Sie zum Erstellen Hadoop Cluster unter Windows, führen Sie eine Struktur Abfrage auf Daten und Ausgabe in Excel zu analysieren."
   keywords="Hadoop auf Windows, Hadoop-Cluster Hadoop Lernprogramm erfahren Sie, Hadoop, Struktur Abfrage"
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
   ms.date="03/07/2016"
   ms.author="nitinme"/>


# <a name="hadoop-tutorial-get-started-using-hadoop-in-hdinsight-on-windows"></a>Hadoop Lernprogramm: Erste Schritte mit Hadoop in HDInsight unter Windows

> [AZURE.SELECTOR]
- [Linux-basierten](../hdinsight-hadoop-linux-tutorial-get-started.md)
- [Windows-basiertem](../hdinsight-hadoop-tutorial-get-started-windows.md)

Damit Sie Hadoop auf Windows finden und Verwenden von HDInsight, zeigt in diesem Lernprogramm zum Ausführen einer Abfrage Struktur auf unstrukturierte Daten in einem Cluster Hadoop und analysieren Sie die Ergebnisse in Microsoft Excel.

>[AZURE.NOTE] Die Informationen in diesem Dokument ist für Windows-basiertem HDInsight Cluster spezifisch. Informationen zu Linux-basierten Cluster, finden Sie unter [Hadoop Lernprogramm: Erste Schritte mit Linux-basierten Hadoop in HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).

Nehmen Sie an eine große unstrukturierte Datenmenge stehen Ihnen und in einer Abfrage Struktur darauf, um einige signifikante Informationen extrahieren ausgeführt werden sollen. Ist genau das, was Sie tun in diesem Lernprogramm abgelegt werden. Hier ist, wie Sie dies erreichen:

   !["Hadoop Lernprogramm: Erstellen Sie ein Konto; Erstellen Sie einen Hadoop Cluster; Senden Sie eine Struktur Abfrage; Analysieren von Daten in Excel.][image-hdi-getstarted-flow]

Schauen Sie sich das Video Demonstrationsvideo dieses Lernprogramms Hadoop auf HDInsight Informationen:

![Teil eines ersten Hadoop Lernprogramms Video: Senden Sie eine Struktur Abfrage in einem Cluster Hadoop und Ergebnisse in Excel zu analysieren.][img-hdi-getstarted-video]

**[Sehen Sie sich das Hadoop-Lernprogramm HDInsight auf YouTube](https://www.youtube.com/watch?v=Y4aNjnoeaHA&list=PLDrz-Fkcb9WWdY-Yp6D4fTC1ll_3lU-QS)**

Zusammen mit der allgemeinen Verfügbarkeit von Azure HDInsight stellt Microsoft auch HDInsight Emulator für Azure, früher bekannt als *Microsoft HDInsight Developer Preview*. Der Emulator Entwicklertools Szenarien ausgerichtet und unterstützt nur mit einem einzelnen Knoten Bereitstellungen. Informationen zur Verwendung von HDInsight Emulator finden Sie unter [Erste Schritte mit dem HDInsight Emulator][hdinsight-emulator].

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie dieses Lernprogramm für Hadoop auf Windows beginnen, müssen Sie die folgenden verfügen:

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion Azure abrufen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- **A Arbeitsstationscomputer** mit Office 2013 Professional Plus, Office 365 Pro Plus, Excel 2013 eigenständigen oder Office 2010 Professional Plus.

### <a name="access-control-requirements"></a>Anforderungen für Access-Steuerelement

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

##<a name="create-hadoop-clusters"></a>Hadoop Cluster erstellen

Wenn Sie einen Cluster erstellen, erstellen Sie Azure berechnen Ressourcen, die Hadoop und zugehörige Applikationen enthalten. In diesem Abschnitt erstellen Sie einen HDInsight Version 3,2 Cluster. Sie können auch Hadoop Cluster für die anderen Versionen erstellen. Anweisungen finden Sie unter [Erstellen von HDInsight Cluster mit benutzerdefinierten Optionen][hdinsight-provision]. Informationen zu HDInsight Versionen und ihre SLAs finden Sie unter [HDInsight Komponente Versioning](hdinsight-component-versioning.md).


**So erstellen Sie einen Hadoop cluster**

1. Melden Sie sich bei der [Azure-Portal](https://portal.azure.com/).
2. Klicken Sie auf **neu**, klicken Sie auf **Daten Analytics**, und klicken Sie dann auf **HDInsight**. Im Portal öffnet eine **Neues HDInsight Cluster** Blade.

    ![Erstellen einer neuen Cluster im Azure-Portal] (./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.CreateCluster.1.png "Erstellen einer neuen Cluster im Azure-Portal")

3. Geben Sie ein, oder wählen Sie Folgendes aus:

    ![EINGABETASTE Clustername und Typ] (./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.CreateCluster.2.png "EINGABETASTE Clustername und Typ")
    
  	|Feldname| Wert|
  	|----------|------|
  	|Clustername| Einen eindeutigen Namen für den Cluster identifizieren|
  	|Clustertyp| Wählen Sie in diesem Lernprogramm **Hadoop** aus. |
  	|Cluster-Betriebssystem| Wählen Sie **Windows Server 2012 R2 Datacenter** für dieses Lernprogramm aus.|
  	|HDInsight-Version| Wählen Sie die neueste Version für dieses Lernprogramm aus.|
  	|Abonnement| Wählen Sie aus dem Azure-Abonnement, das für den Cluster verwendet werden.|
  	|Ressourcengruppe | Wählen Sie eine vorhandene Azure Ressourcengruppe aus, oder erstellen Sie eine neue Ressourcengruppe. Ein grundlegende HDInsight Cluster enthält einen Cluster und deren Speicher Standardkonto.  Sie können die beiden in eine Ressourcengruppe für einfache Verwaltung gruppieren.|
  	|Anmeldeinformationen| Geben Sie die Cluster Login Benutzernamen und Ihr Kennwort ein. Windows-basierten Cluster kann 2 Benutzerkonten verfügen.  Die Cluster Benutzer (oder HTTP-Benutzer) wird zum Verwalten des Clusters und übermitteln Aufträge verwendet.  Optional können Sie einen remote Desktop (RDP) erstellen Benutzerkonto auf Remote zum Cluster verbinden. Wenn Sie Remotedesktop aktivieren, erstellen Sie das Benutzerkonto RDP.|
  	|Datenquelle| Klicken Sie auf neu erstellen, um eine neue Azure-Speicher Standardkonto zu erstellen. Verwenden Sie den Clusternamen als Standardname Container aus. Jeder HDinsight Cluster weist einen standardmäßige Blob-Container eine Accont Azure-Speicher.  Die Position des Standardkontos Azure-Speicher bestimmt den Speicherort der Cluster HDInsight.|
  	|Preise Ebenen Knoten| Verwenden von 1 oder 2 Worker Knoten mit standardmäßiger Worker Knoten und Kopf Notiz Ebene in diesem Lernprogramm Preise.|
  	|Optionale Konfiguration| Dieses Teils zu überspringen.|

9. In der **Neuen HDInsight Cluster** Blade stellen Sie sicher, dass **an Startboard anheften** ausgewählt ist, und klicken Sie dann auf **Erstellen**. Diese Cluster erstellen und hinzufügen eine Kachel dafür zu den Startboard von Ihrer Azure-Portal. Das Symbol wird mitgeteilt wird, dass der Cluster besteht im erstellen wird so geändert, dass das HDInsight-Symbol angezeigt werden, nachdem Sie erstellt wurde.

  	| Während der Erstellung | Erstellung abgeschlossen |
  	| ------------------ | --------------------- |
  	| ![Indikator für erstellen startboard](./media/hdinsight-hadoop-tutorial-get-started-windows/provisioning.png) | ![Cluster Kachel erstellt](./media/hdinsight-hadoop-tutorial-get-started-windows/provisioned.png) |

    > [AZURE.NOTE] Es dauert einige Zeit für den Cluster, normalerweise ungefähr 15 Minuten erstellt werden. Verwenden Sie die Kachel auf der Startboard oder den Eintrag **Benachrichtigungen** auf der linken Seite der Seite auf den Erstellungsprozess zu prüfen.

10. Sobald die Erstellung abgeschlossen ist, klicken Sie auf die Kachel für den Cluster aus der Startboard, um das Blade Cluster zu starten.


## <a name="run-a-hive-query-from-the-portal"></a>Führen Sie eine Struktur Abfrage aus dem portal
Jetzt, da Sie einen HDInsight Cluster erstellt haben, besteht der nächste Schritt um eine Struktur um eine Struktur Beispieltabelle Abfragen auszuführen. Wir verwenden *Hivesampletable*, die mit HDInsight Cluster stammt. Die Tabelle enthält Daten zu mobilen Gerätehersteller, Plattformen und Modelle. Eine Abfrage Struktur in dieser Tabelle ruft Daten für mobile Geräte eines bestimmten Herstellers ab.

> [AZURE.NOTE] HDInsight Tools für Visual Studio Lieferumfang der Azure SDK für .NET Version 2,5 oder höher. Mithilfe der Tools in Visual Studio können Sie Herstellen einer Verbindung HDInsight Cluster mit, Struktur Tabellen erstellen und Ausführen von Abfragen Struktur. Weitere Informationen finden Sie unter [Erste Schritte mit HDInsight Hadoop-Tools für Visual Studio][1].

**Um eine Struktur aus dem Dashboard Cluster auszuführen**

1. Melden Sie sich bei der [Azure-Portal](https://portal.azure.com/).
2. Klicken Sie auf **Alle durchsuchen** , und klicken Sie dann auf **HDInsight Cluster** zum Anzeigen einer Liste der Cluster, einschließlich des Clusters, die, den Sie gerade im vorherigen Abschnitt erstellt haben.
3. Klicken Sie auf den Namen der Cluster, den Sie zum Ausführen des Auftrags Struktur verwenden möchten, und klicken Sie dann auf **Dashboard** am oberen Rand der Blade.
4. Auf eine Webseite, die auf einer Browserregisterkarte andere wird geöffnet. Geben Sie den Hadoop Benutzerkonto und das Kennwort ein. Der Standard-Benutzername ist **Admin**. das Kennwort ist beim Erstellen des Clusters eingegeben.
5. Klicken Sie aus dem Dashboard auf der Registerkarte **Struktur-Editor** . Die folgende Webseite wird geöffnet.

    ![Registerkarte Struktur Editor im Dashboard Cluster HDInsight.][img-hdi-dashboard]

    Es gibt mehrere Registerkarten am oberen Rand der Seite aus. Standardregisterkarte ist **Struktur-Editor**, und die anderen Registerkarten sind **Historie** und **Datei-Browser**. Mithilfe von Dashboard können Sie Struktur Abfragen übermitteln, Hadoop Auftrag Protokolle aktivieren, und navigieren Sie Dateien im Speicher.

    > [AZURE.NOTE] Beachten Sie, dass die URL der Webseite * &lt;ClusterName&gt;. azurehdinsight.net*. Daher können Öffnen des Dashboards vom-Portal an, Sie das Dashboard mit einem Webbrowser öffnen mithilfe der URL.

6. Geben Sie auf der Registerkarte **Struktur Editor** **Abfragenamen** **HTC20**.  Name der Abfrage ist die Position des Kontakts. Geben Sie im Abfragebereich die Struktur Abfrage ein, wie in der Abbildung dargestellt:

    ![Struktur Abfrage eingegeben haben, klicken Sie im Abfrage-Editor Struktur.][img-hdi-dashboard-query-select]

4. Klicken Sie auf **Absenden**. Es dauert einen Moment, um die Ergebnisse zurückzukehren. Der Bildschirm wird alle 30 Sekunden aktualisiert. Sie können auch **Aktualisieren** , um den Bildschirm zu aktualisieren klicken.

    ![Ergebnisse einer Abfrage Struktur in aufgeführt am unteren Rand der Cluster Dashboard.][img-hdi-dashboard-query-select-result]

5. Nachdem der Status angezeigt wird, dass das Projekt abgeschlossen ist, klicken Sie auf den Abfragenamen auf dem Bildschirm, um die Ausgabe anzuzeigen. Notieren Sie **Auftrag starten (Coordinated)**. Sie werden diese später benötigen.

    ![Position-Anfangszeit auf der Registerkarte Historie des Dashboards Cluster HDInsight aufgeführt.][img-hdi-dashboard-query-select-result-output]

    Die Seite zeigt auch die **Ausgabe der Position** und der **Job-Protokoll**. Sie haben auch die Möglichkeit zum Herunterladen der Ausgabedatei (\_Stdout) und die Protokolldatei \(_stderr).


**Um die Datei zu suchen**

1. Klicken Sie auf dem Dashboard Cluster auf **Datei-Browser**.
2. Klicken Sie auf Ihren Kontonamen Speicher, klicken Sie auf den Namen Ihres Containers ein (das entspricht Ihren Clusternamen ist), und klicken Sie dann auf **Benutzer**.
3. Klicken Sie auf **Administrator** , und klicken Sie dann auf die GUID, die Uhrzeit der letzten Änderung (etwas nach der Auftrag Startzeit an, die Sie zuvor notiert haben) enthält. Kopieren Sie diese GUID an. Sie benötigen sie im nächsten Abschnitt.


    ![Die Struktur Abfrage ausgeben-Datei, die GUIDs auf der Registerkarte Datei-Browser angezeigt.][img-hdi-dashboard-query-browse-output]


##<a name="connect-to-microsoft-business-intelligence-tools-for-excel"></a>Verbinden Sie mit Microsoft Business Intelligence-Tools für Excel

Sie können das Power Query-add-in für Microsoft Excel die Ausgabe der Position aus HDInsight in Excel importieren, in dem Microsoft Business Intelligence-Tools verwendet werden können, um die Ergebnisse zu analysieren.

Sie müssen Excel 2013 oder 2010 installiert haben, um diesen Teil des Lernprogramms abschließen können.

**Herunterladen von Microsoft Power Query für Excel**

- Herunterladen Sie Microsoft Power Query für Microsoft Excel vom [Microsoft Download Center](http://www.microsoft.com/download/details.aspx?id=39379) und installieren Sie es.

**So importieren Sie Daten HDInsight**

1. Öffnen Sie Excel, und erstellen Sie eine neue Arbeitsmappe.
3. Klicken Sie auf das Menü **Power Query** auf **Aus anderen Quellen**, und klicken Sie dann auf **Aus Azure HDInsight**.

    ![Importieren von Excel PowerQuery Menü Azure HDInsight öffnen.][image-hdi-gettingstarted-powerquery-importdata]

3. Geben Sie den **Kontonamen** des Kontos Azure BLOB-Speicher, die Ihren Cluster zugeordnet ist, und klicken Sie dann auf **OK**. (Dies ist das Speicherkonto, die, das Sie zuvor in diesem Lernprogramm erstellt haben).
4. Geben Sie den **Kontoschlüssel** für das Konto Azure BLOB-Speicher, und klicken Sie dann auf **Speichern**.
5. Doppelklicken Sie im rechten Bereich auf den Namen des Blob. Standardmäßig ist der Blob-Name den Clusternamen entspricht.

6. Suchen nach **Stdout** in der Spalte **Name** . Stellen Sie sicher, dass die GUID in die entsprechende Spalte **Ordnerpfad** die GUID entspricht Sie zuvor kopiert haben. Eine Übereinstimmung vorschlägt, dass die Ausgabedaten den Auftrag entspricht, die Sie auch ursprünglich eingereicht. Klicken Sie auf **binäre** in der linken Spalte der **Stdout**.

    ![Suchen nach GUID die Datenausgabe in der Liste der Inhalte an.][image-hdi-gettingstarted-powerquery-importdata2]

9. Klicken Sie auf **Schließen und Laden** in die obere linke Ecke den Struktur Auftrag Ausgabe in Excel zu importieren.

##<a name="run-samples"></a>Ausführen von Beispielen

HDInsight Cluster bietet eine Abfrage-Konsole, die einen Katalog erste Schritte zum Ausführen der Beispiele direkt aus dem Portal enthält. Die Beispiele können Sie erfahren, wie mit HDInsight arbeiten, indem Sie einige grundlegende Szenarios durchgehen. Diese Beispiele im Zusammenhang mit den erforderlichen Komponenten, wie die Daten zu analysieren und die Abfragen für die Daten ausgeführt. Weitere Informationen zu den Beispielen in die erste Schritte-Katalog finden Sie unter [Informationen Hadoop in den Katalog HDInsight erste Schritte mit HDInsight](hdinsight-learn-hadoop-use-sample-gallery.md).

**Zum Ausführen des Beispiels**

1. Klicken Sie auf die Kachel für den soeben erstellten Cluster, aus der Startboard Azure-Portal.
 
2. Klicken Sie auf das neue Cluster Blade auf **Dashboard**. Wenn Sie dazu aufgefordert werden, geben Sie den Administrator-Benutzernamen und das Kennwort für den Cluster ein.

    ![Starten Sie Cluster dashboard] (./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.Cluster.Dashboard.png "Starten Sie Cluster dashboard")
 
3. Klicken Sie von der Webseite, die angezeigt wird auf der Registerkarte **Erste Schritte-Katalog** , und klicken Sie dann unter der Kategorie **Lösungen mit Beispieldaten** auf der Stichprobe, die Sie ausführen möchten. Folgen Sie den Anweisungen auf der Webseite, um die Stichprobe fertig zu stellen. In der folgenden Tabelle sind einige Beispiele aufgeführt und enthält weitere Informationen zu welche jedes Beispiel enthält.

Beispiel | Was es?
------ | ---------------
[Sensor Datenanalyse][hdinsight-sensor-data-sample] | Erfahren Sie, wie HDInsight verwenden, um zurückliegende Daten zu verarbeiten, die erstellt wird, indem Heizung, Belüftung und (HKL-System) Klimaanlagen um Systeme zu identifizieren, die nicht zuverlässig eine Temperatur festlegen verwalten können.
[Website Log-Analyse][hdinsight-weblogs-sample] | Informationen Sie zum Verwenden von HDInsight zum Analysieren von Website-Protokolldateien, um Einsichten in die Häufigkeit von Besuchen Sie die Website in einem Tag von externen Websites und eine Zusammenfassung von Fehlern bei der Website, die die Benutzer auftreten.
[Twitter-Trend-Analyse](hdinsight-analyze-twitter-data.md) | Erfahren Sie, wie Sie HDInsight zum Analysieren von Trends in Twitter.

##<a name="delete-the-cluster"></a>Löschen des Clusters

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

##<a name="next-steps"></a>Nächste Schritte
In diesem Lernprogramm Hadoop gelernt Sie zum Erstellen eines Clusters Hadoop auf Windows in HDInsight, führen Sie eine Struktur Abfrage auf Daten, und importieren Sie die Ergebnisse in Excel, in dem sie weitere werden können verarbeitet und grafisch mit Business Intelligence-Tools angezeigt. Weitere Informationen finden Sie unter den folgenden Lernprogramme:

- [Erste Schritte mit HDInsight Hadoop-Tools für Visual Studio][1]
- [Erste Schritte mit dem HDInsight Emulator][hdinsight-emulator]
- [Verwenden von Azure Blob-Speicher mit HDInsight][hdinsight-storage]
- [Verwalten von HDInsight mithilfe der PowerShell][hdinsight-admin-powershell]
- [Hochladen von Daten mit HDInsight][hdinsight-upload-data]
- [Verwenden von MapReduce mit HDInsight][hdinsight-use-mapreduce]
- [Verwenden Sie die Struktur mit HDInsight][hdinsight-use-hive]
- [Schwein mit HDInsight verwenden][hdinsight-use-pig]
- [Verwenden von Oozie mit HDInsight][hdinsight-use-oozie]
- [Entwickeln Sie MapReduce Java-Programme für HDInsight][hdinsight-develop-mapreduce]


[1]: ../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md

[hdinsight-versions]: hdinsight-component-versioning.md


[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-emulator]: hdinsight-hadoop-emulator-get-started.md
[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md
[hadoop-hdinsight-intro]: hdinsight-hadoop-introduction.md
[hdinsight-weblogs-sample]: hdinsight-hive-analyze-website-log.md
[hdinsight-sensor-data-sample]: hdinsight-hive-analyze-sensor-data.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]: ../storage-create-storage-account.md

[apache-hadoop]: http://go.microsoft.com/fwlink/?LinkId=510084
[apache-hive]: http://go.microsoft.com/fwlink/?LinkId=510085
[apache-mapreduce]: http://go.microsoft.com/fwlink/?LinkId=510086
[apache-hdfs]: http://go.microsoft.com/fwlink/?LinkId=510087
[hdinsight-hbase-custom-provision]: hdinsight-hbase-tutorial-get-started.md


[powershell-download]: http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409
[powershell-install-configure]: powershell-install-configure.md
[powershell-open]: powershell-install-configure.md#step-1-install


[img-hdi-dashboard]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.dashboard.png
[img-hdi-dashboard-query-select]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.dashboard.query.select.png
[img-hdi-dashboard-query-select-result]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.dashboard.query.select.result.png
[img-hdi-dashboard-query-select-result-output]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.dashboard.query.select.result.output.png
[img-hdi-dashboard-query-browse-output]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.dashboard.query.browse.output.png

[img-hdi-getstarted-video]: ./media/hdinsight-hadoop-tutorial-get-started-windows/hdi-get-started-video.png


[image-hdi-storageaccount-quickcreate]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.StorageAccount.QuickCreate.png
[image-hdi-clusterstatus]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.ClusterStatus.png
[image-hdi-quickcreatecluster]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.QuickCreateCluster.png
[image-hdi-getstarted-flow]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.GetStartedFlow.png

[image-hdi-gettingstarted-powerquery-importdata]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.GettingStarted.PowerQuery.ImportData.png
[image-hdi-gettingstarted-powerquery-importdata2]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.GettingStarted.PowerQuery.ImportData2.png
 

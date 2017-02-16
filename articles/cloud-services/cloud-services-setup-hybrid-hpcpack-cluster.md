<properties
    pageTitle="Einrichten von einem Hybriden HPC Cluster mit Microsoft HPC Pack | Microsoft Azure"
    description="Informationen Sie zum Verwenden von Microsoft HPC Pack und Azure ein kleines, Hybrid hohe Performance computing (HPC) Cluster einrichten"
    services="cloud-services"
    documentationCenter=""
    authors="dlepow"
    manager="timlt"
    editor=""
    tags="azure-service-management,hpc-pack"/>

<tags
    ms.service="cloud-services"
    ms.workload="big-compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/14/2016"
    ms.author="danlep"/>


# <a name="set-up-a-hybrid-high-performance-computing-hpc-cluster-with-microsoft-hpc-pack-and-on-demand-azure-compute-nodes"></a>Einrichten einer Hybrid hohe Performance computing (HPC) Cluster mit Microsoft HPC Pack und bei Bedarf Azure berechnen Knoten

Verwenden Sie Microsoft HPC Pack 2012 R2 und Azure zum Einrichten eines klein, Hybrid hohe Performance computing (HPC) Cluster an. Der Cluster besteht aus einer lokalen am Knoten (ein Computer, der das Betriebssystem Windows Server und HPC Pack ausgeführt wird), und einige Knoten, die Sie bei Bedarf als Worker Rolleninstanzen in einer Azure-Cloud-Dienst bereitstellen zu berechnen. Sie können Aufträge berechnen klicken Sie dann auf den Hybrid Cluster ausführen.

![Hybrid HPC cluster][Overview] 

In diesem Lernprogramm erfahren ein Verfahren namens manchmal Cluster "in der Cloud, Burst" um skalierbare verwenden, bei Bedarf Ressourcen Azure zur Ausführung rechenintensiven Anwendung zu berechnen.

In diesem Lernprogramm wird davon ausgegangen ohne vorherige Erfahrung mit Computecluster oder HPC Pack aus. Es soll nur beim Bereitstellen von einem berechnungscluster Hybrid schnell Zwecken Demo helfen. Aspekte und Schritte zum Bereitstellen eines Hybrid HPC Pack Clusters bei größer in einer Umgebung für die Herstellung finden Sie unter die [detaillierte Anleitung](http://go.microsoft.com/fwlink/p/?LinkID=200493). Andere Szenarien mit HPC Pack, einschließlich der automatisierten Clusterbereitstellung in Azure-virtuellen Computern finden Sie unter [HPC Cluster Optionen mit Microsoft HPC Pack in Azure](../virtual-machines/virtual-machines-windows-hpcpack-cluster-options.md).


## <a name="prerequisites"></a>Erforderliche Komponenten

* **Azure-Abonnement** – Wenn Sie ein Azure-Abonnement besitzen, können Sie eine [freie Konto](https://azure.microsoft.com/free/) nur wenigen Minuten erstellen.

* **Eine dem lokalen Computer unter Windows Server 2012 R2 oder Windows Server 2012** - diesem Computer werden die am Knoten des HPC-Cluster. Wenn Sie bereits Windows Server ausgeführt werden, können Sie herunterladen und Installieren einer [Testversion](https://www.microsoft.com/evalcenter/evaluate-windows-server-2012-r2).

    * Der Computer muss eine Active Directory-Domäne angehören. Für ein Testszenario mit einer Neuinstallation von Windows Server Sie die Rolle des Active Directory-Domänendiensten Server hinzufügen und Stufen Sie den Knoten am Computer als Domänencontroller in einer neuen Domäne (siehe Dokumentation für Windows Server).

    * Zur Unterstützung von HPC Pack muss das Betriebssystem, das auf eine der folgenden Sprachen installiert sein: Englisch, Japanisch oder Chinesisch (vereinfacht).

    * Stellen Sie sicher, dass wichtige und wichtige Updates installiert sind.

* **HPC Pack 2012 R2** - Verpacken für die neueste Version kostenlos[herunterladen](http://go.microsoft.com/fwlink/p/?linkid=328024) der Installation, und kopieren Sie die Dateien aus, zu dem Knoten am Computer oder an einem Netzwerkspeicherort. Wählen Sie Installationsdateien in einer anderen Sprache als der Installation von Windows Server aus.

* **Domänenkonto** - dieses Konto muss konfiguriert sein, mit lokalen Administratorberechtigungen für den Knoten am HPC Pack installieren.

* **TCP-Konnektivität Port 443** aus dem am Knoten in Azure.

## <a name="install-hpc-pack-on-the-head-node"></a>Installieren Sie auf dem am Knoten HPC Pack

Zuerst installieren Sie Microsoft HPC Pack auf Ihrem lokalen Computer unter Windows Server, die die am Knoten im Cluster werden sollen.

1. Melden Sie sich an den am Knoten mithilfe einer Domänenkonto mit lokalen Administratorberechtigungen an.

2. Zunächst den Assistenten zum Installieren von HPC Pack an der Setup.exe aus den HPC Pack Installationsdateien ausführen.

3. Klicken Sie auf dem Bildschirm **HPC Pack 2012 R2-Setup** auf **Neuinstallation oder Hinzufügen von neuen Features zu einer vorhandenen Installation**.

    ![HPC Pack 2012-Setup][install_hpc1]

4. Klicken Sie auf der **Seite Microsoft Software allgemeinen Geschäftsbedingungen**auf **Weiter**.

5. Klicken Sie auf der Seite **Select Installation Type** auf **einen neuen HPC Cluster durch Erstellen eines am Knotens zu erstellen**, und klicken Sie dann auf **Weiter**.

    ![Wählen Sie Installation aus][install_hpc2]

6. Der Assistent ausgeführt wird, mehrere vor der Installation überprüft werden. Klicken Sie auf **Weiter** klicken Sie auf der Seite **Installationsregeln** , wenn alle Tests bestehen. Andernfalls, überprüfen Sie die Informationen zur Verfügung gestellt, und nehmen Sie alle notwendigen Änderungen vor in Ihrer Umgebung. Führen Sie die Tests erneut aus, oder starten Sie den Assistenten zum Installieren bei Bedarf erneut.

    ![Von Installationsregeln][install_hpc3]

7. Klicken Sie auf der Seite **HPC DB Konfiguration** sicherzustellen Sie, dass für alle HPC Datenbanken **Kopf Knoten** ausgewählt ist, und klicken Sie dann auf **Weiter**.

    ![DB-Konfiguration][install_hpc4]

8. Akzeptieren Sie die Standardauswahl auf den verbleibenden Seiten des Assistenten. Klicken Sie auf der Seite **Erforderliche Komponenten installieren** auf **Installieren**.

    ![Installieren][install_hpc6]

9. Nach Abschluss die Installation **Starten HPC Cluster Manager** deaktivieren Sie, und klicken Sie dann auf **Fertig stellen**. (Sie werden in einem späteren Schritt HPC Cluster Manager starten.)

    ![Fertig stellen][install_hpc7]

## <a name="prepare-the-azure-subscription"></a>Vorbereiten des Azure-Abonnements
Mithilfe der [Azure klassischen Portal](https://manage.windowsazure.com) mit Ihrem Azure-Abonnement die folgenden Schritte ausführen. Diese sind erforderlich, damit Sie später Azure Knoten vom am lokalen Knoten bereitstellen können. Ausführliche Verfahren werden in den nächsten Abschnitten.

- Hochladen eines Management Zertifikats (für sichere Verbindungen zwischen dem am Knoten und der Azure-Dienste erforderlich)

- Erstellen eines Azure Cloud-Diensts, in welche, den Azure Knoten (Worker Rolleninstanzen) ausgeführt wird

- Erstellen Sie ein Konto Azure-Speicher

    >[AZURE.NOTE]Notieren Sie auch Ihre Azure-Abonnement-ID, die Sie noch benötigen. Finden Sie in der klassischen Portal durch Klicken auf **Einstellungen** > **Abonnements**.

### <a name="upload-the-default-management-certificate"></a>Das standardmäßige Management Zertifikat hochladen
HPC Pack installiert ein selbst signiertes Zertifikat auf den am Knoten, das standardmäßig Microsoft HPC Azure Management Zertifikat, das Sie als ein Zertifikat Azure Management hochladen können bezeichnet. Dieses Zertifikat wird zum Testen der Zwecke und Prüfung des Konzepts Bereitstellungen bereitgestellt.

1. Melden Sie sich [Azure klassischen Portal](https://manage.windowsazure.com), aus dem Knoten am Computer.

2. Klicken Sie auf **Einstellungen** > **Management Zertifikate**.

3. Klicken Sie auf der Befehlsleiste auf **Hochladen**.

    ![Zertifikat-Einstellungen][upload_cert1]

4. Navigieren Sie auf dem am Knoten für die Datei c:\Programme\Microsoft c:\Programme\Microsoft HPC Pack 2012\Bin\hpccert.cer. Klicken Sie dann auf die Schaltfläche **Aktivieren** .

    ![Hochladen des Zertifikats][install_hpc10]

**Standard HPC Azure Management** finden Sie in der Liste der Verwaltung von Zertifikaten.

### <a name="create-an-azure-cloud-service"></a>Erstellen eines Azure-Cloud-Diensts

>[AZURE.NOTE]Erstellen Sie zur Optimierung der Systemleistung Cloud-Dienst und das Speicherkonto (in einem späteren Schritt) in der gleichen geografische Region ein.

1. Klicken Sie im Portal klassischen auf der Befehlsleiste auf **neu**.

2. Klicken Sie auf **berechnen** > **Cloud-Dienst** > **schnell zu erstellen**.

3. Geben Sie eine URL für den Clouddienst, und klicken Sie dann auf **Cloud-Dienst erstellen**.

    ![Dienst erstellen][createservice1]

### <a name="create-an-azure-storage-account"></a>Erstellen Sie ein Konto Azure-Speicher

1. Klicken Sie im Portal klassischen auf der Befehlsleiste auf **neu**.

2. Klicken Sie auf **Data Services** > **Speicher** > **schnell zu erstellen**.

3. Geben Sie eine URL für das Konto ein, und klicken Sie dann auf **Speicher-Konto erstellen**.

    ![Erstellen von Speicher][createstorage1]

## <a name="configure-the-head-node"></a>Konfigurieren des am Knotens

Führen Sie zum Verwenden von HPC Cluster Manager Azure Knoten bereitstellen und Aufträge senden zuerst einige Konfigurationsschritte erforderlich Cluster an.

1. Starten Sie auf die am Knoten HPC Cluster Manager ein. Wenn das Dialogfeld **Kopf-Knoten auswählen** angezeigt wird, klicken Sie auf **Lokalen Computer**. Die **Bereitstellung Aufgabenliste** angezeigt wird.

2. Klicken Sie unter **Bereitstellungsaufgaben erforderlich**klicken Sie auf **Konfigurieren Ihres Netzwerks**.

    ![Konfigurieren von Netzwerk][config_hpc2]

3. Wählen Sie im Konfigurations-Assistenten aus **allen Knoten nur in einem Unternehmensnetzwerk** (Suchtopologie 5).

    ![Suchtopologie 5][config_hpc3]

    >[AZURE.NOTE]Dies ist die einfachste Konfiguration für Demonstrationszwecke, da der am Knoten nur einen einzigen Netzwerkadapter Verbindung mit Active Directory und im Internet benötigt. In diesem Lernprogramm befasst sich nicht Cluster Szenarien, die zusätzliche Netzwerke erfordern.

4. Klicken Sie auf **Weiter** , um zu Standardwerte auf den übrigen Seiten des Assistenten zu übernehmen. Klicken Sie dann auf der Registerkarte **Überprüfen** auf **Konfigurieren** , um die Netzwerkkonfiguration abzuschließen.

5. Klicken Sie in der **Vorgangsliste Bereitstellung**auf **die Installation Anmeldeinformationen bereitstellen**.

6. Geben Sie im Dialogfeld **Installation Anmeldeinformationen** die Anmeldeinformationen für das Domänenkonto ein, die Sie mit dem HPC Pack installiert. Klicken Sie dann auf **OK**.

    ![Installation von Anmeldeinformationen][config_hpc6]

    >[AZURE.NOTE]HPC Pack Services verwenden nur Installation Anmeldeinformationen Domänenverbund Datenverarbeitungsknoten bereitstellen. Die Azure-Knoten, die Sie in diesem Lernprogramm hinzufügen werden nicht Domäne.

7. Klicken Sie in der **Vorgangsliste Bereitstellung**auf **die Benennung von neuen Knoten konfigurieren**.

8. Klicken Sie im Dialogfeld **Angeben Knoten benennen Reihe** übernehmen Sie den Standardwert Reihe benennen, und klicken Sie auf **OK**.

    ![Benennen von Knoten][config_hpc8]

    >[AZURE.NOTE]Die Reihe naming generiert nur Namen für die Domäne Datenverarbeitungsknoten. Azure Worker Knoten werden automatisch mit dem Namen.

9. Klicken Sie in der **Vorgangsliste Bereitstellung** **Erstellen einer Vorlage**aus. Die Vorlage Knoten benötigen Cluster Azure Knoten hinzu.

10. Im Erstellen Knoten Vorlage-Assistenten folgendermaßen Sie vor:

    ein. Klicken Sie auf der Seite **Knoten Vorlagentyp auswählen** auf **Azure-Vorlage**, und klicken Sie dann auf **Weiter**.

    ![Knoten-Vorlage][config_hpc10]

    b. Klicken Sie auf **Weiter** , um zu den Namen der Vorlage zu übernehmen.

    c. Geben Sie auf der Seite **Abonnementinformationen bieten** Ihre Azure-Abonnement-ID (verfügbar in Ihre Kontoinformationen Azure) ein. **Management Zertifikat**, klicken Sie auf **Durchsuchen** , und wählen Sie **Standard HPC Azure Management.** Klicken Sie dann auf **Weiter**.

    ![-Vorlage][config_hpc12]

    d. Wählen Sie auf der Seite **Informationen zum Dienst bereitstellen** Cloud-Dienst und das Speicherkonto, das Sie in einem vorherigen Schritt erstellt haben. Klicken Sie dann auf **Weiter**.

    ![Knoten-Vorlage][config_hpc13]

    e. Klicken Sie auf **Weiter** , um zu Standardwerte auf den übrigen Seiten des Assistenten zu übernehmen. Klicken Sie dann auf der Registerkarte **Überprüfen** auf **Erstellen** , um die Vorlage zu erstellen.

    >[AZURE.NOTE]Standardmäßig umfasst die Vorlage Azure Knoten Einstellungen zum (bereitstellen) starten und beenden die Knoten manuell HPC Cluster Manager verwenden. Sie können optional einen Zeitplan zum Starten und beenden die Azure Knoten automatisch konfigurieren.

## <a name="add-azure-nodes-to-the-cluster"></a>Hinzufügen von Azure Knoten zum cluster

Jetzt verwenden Sie die Vorlage Knoten Cluster Azure Knoten hinzu. Den Knoten in die Cluster-Speicher deren Konfigurationsinformationen hinzufügen, damit Sie (bereitstellen) beginnen können sie zu einem beliebigen Zeitpunkt als Rolle Instanzen in der Cloud-Dienst. Ihr Abonnement wird nur Azure-Knoten in Rechnung gestellt, nachdem die Rolleninstanzen in der Cloud-Dienst ausgeführt werden.

In diesem Lernprogramm fügen Sie zwei kleine Knoten.

1. Klicken Sie in HPC Cluster Manager bei **Knoten Management** (sogenannte **Ressourcenverwaltung** in vorherigen Versionen von HPC Pack) im Bereich **Aktionen** auf **Knoten hinzufügen**.

    ![Hinzufügen von Knoten][add_node1]

2. Klicken Sie im Assistenten zum Hinzufügen von Knoten auf der Seite **Select-Bereitstellungsmethode** auf **Azure hinzufügen-Knoten**und klicken Sie dann auf **Weiter**.

    ![Hinzufügen von Azure Knoten][add_node1_1]

3. Wählen Sie auf der Seite **Neue Knoten angeben** die Azure-zuvor erstellte Vorlage (standardmäßig **AzureNode Standardvorlage**genannt). Geben Sie dann **2** Knoten **kleine**Größe aus, und klicken Sie dann auf **Weiter**.

    ![Geben Sie Knoten][add_node2]

    Weitere Informationen zu den verfügbaren Größen finden Sie unter [Größen für Cloud-Dienste](cloud-services-sizes-specs.md).

4. Klicken Sie auf der Seite **Fertigstellen des Assistenten zum Hinzufügen von Knoten** klicken Sie auf **Fertig stellen**.

     Zwei Azure Knoten, mit dem Namen **AzureCN-0001** und **AzureCN-0002**, werden jetzt in HPC Cluster Manager angezeigt. Beide sind im Zustand **Nicht bereitgestellt** .

    ![Hinzugefügten Knoten][add_node3]

## <a name="start-the-azure-nodes"></a>Starten Sie die Azure Knoten
Wenn Sie die Clusterressourcen in Azure verwenden möchten, verwenden Sie HPC Cluster Manager starten (bereitstellen) der Azure-Knoten und sie online schalten.

1.  In HPC Cluster Manager bei **Knoten Management** ( **Ressourcenverwaltung** in der neuesten Versionen von HPC Pack genannt), klicken Sie auf einen oder beide Knoten, und klicken Sie dann im Bereich **Aktionen** auf **Starten**.

    ![Starten Sie Knoten][add_node4]

2. Klicken Sie auf **Start**, klicken Sie im Dialogfeld **Azure-Knoten beginnen** .

    ![Starten Sie Knoten][add_node5]

    Der Knoten Übergang in den Zustand **Provisioning** . Zeigen Sie das provisioning an zum Überwachen der Bereitstellung.

    ![Bereitstellen von Knoten][add_node6]

3. Nach ein paar Minuten Azure Knoten fertig stellen, bereitgestellt und in den Zustand **Offline** sind. In diesem Zustand die Rolleninstanzen ausgeführt werden, aber Clusteraufträge noch nicht akzeptiert werden.

4. Klicken Sie auf **Cloud Services**, um zu bestätigen, dass die Rolleninstanzen, in der [klassischen Portal ausgeführt werden](https://manage.windowsazure.com) > *Your_cloud_service_name* > **Instanzen**.

    ![Instanzen ausführen][view_instances1]

    Sie sehen, dass zwei Instanzen von Worker-Rolle im Dienst ausgeführt werden. HPC Pack wird ebenfalls automatisch zwei **HpcProxy** Instanzen (Größe Mittel) zum Behandeln der Kommunikation zwischen Knoten am und Azure bereitgestellt.

5. Um die Azure-Knoten online schalten Cluster Auftrag ausführen, wählen Sie den Knoten, mit der rechten Maustaste, und klicken Sie dann auf **Online schalten**.

    ![Offline-Knoten][add_node7]

    HPC Cluster Manager zeigt an, dass die Knoten im Zustand **Online** sind.

## <a name="run-a-command-across-the-cluster"></a>Führen Sie einen Befehl, über den cluster

Um die Installation zu überprüfen, verwenden Sie den Befehl HPC Pack **Clusrun** zum Ausführen eines Befehls oder einer Anwendung auf einen oder mehrere Clusterknoten aus. Verwenden Sie ein einfaches Beispiel **Clusrun** können Sie um die IP-Konfiguration der Azure Knoten zu gelangen.

1. Öffnen Sie auf dem am Knoten ein Eingabeaufforderungsfenster.

2. Geben Sie den folgenden Befehl ein:

    `clusrun /nodes:azurecn* ipconfig`

3. Ähnlich wie das folgende Ergebnis wird angezeigt.

    ![ClusRun][clusrun1]

## <a name="run-a-test-job"></a>Führen Sie einen Auftrag testen

Jetzt übermitteln einer Test-Position, die auf dem Cluster Hybrid ausgeführt wird. In diesem Beispiel wird eine sehr einfache parametrische Zug Position (einen Typ von systembedingt parallelen Berechnung). In diesem Beispiel wird die Teilvorgänge, die eine ganze Zahl an sich selbst hinzufügen, mithilfe des Befehls **set/a** ausgeführt. Alle Knoten im Cluster eigene Notizen hinzufügen können, werden die Teilvorgänge für ganze Zahlen zwischen 1 und 100.

1. Klicken Sie in HPC Cluster Manager im **Projektmanagement**im Bereich **Aktionen** auf **Neue parametrische Zug Position**.

    ![Neue Position][test_job1]

2. Geben Sie im Dialogfeld **Neues Projekt parametrische Zug** in **einigen** `set /a *+*` (Überschreiben der Standard-Befehlszeile, die angezeigt wird). Lassen Sie Standardwerte für die verbleibenden Einstellungen, und klicken Sie auf **Absenden** , um den Auftrag zu übermitteln.

    ![Parametrische Zug][param_sweep1]

3. Wenn das Projekt abgeschlossen ist, doppelklicken Sie auf den Auftrag **Meine Zug Vorgang** .

4. Klicken Sie auf **Ansicht Vorgänge**, und klicken Sie dann auf eines Teilvorgangs zum Anzeigen der berechneten Ausgabe dieses Teilvorgangs.

    ![Task-Ergebnisse][view_job361]

5. Um anzuzeigen, welche Knoten die Berechnung für dieses Teilvorgangs ausgeführt, klicken Sie auf **Knoten zugewiesen**. (Der Cluster möglicherweise einen anderen Knotennamen angezeigt.)

    ![Ergebnisse der Aufgabe][view_job362]

## <a name="stop-the-azure-nodes"></a>Beenden der Azure-Knoten

Nachdem Sie die Cluster ausprobieren möchten, beenden Sie die Azure-Knoten zur Vermeidung von unnötige Gebühren für Ihr Konto aus. Beendet den Clouddienst, und die Instanzen Azure-Rolle entfernt.

1. Wählen Sie in **Knoten Management** ( **Verwaltung von Ressourcen** in neuere Versionen von HPC Pack genannt), der in HPC Cluster Manager beide Azure Knoten aus. Klicken Sie dann im Bereich **Aktionen** auf **Beenden**.

    ![Beenden von Knoten][stop_node1]

2. Klicken Sie auf **Beenden**, klicken Sie im Dialogfeld **Azure-Knoten beenden** .

    ![Beenden von Knoten][stop_node2]

3. Der Knoten Übergang in den Zustand **Beenden** . Nach ein paar Minuten zeigt HPC Cluster Manager an, dass die Knoten **Nicht bereitgestellt**werden.

    ![Nicht bereitgestellte Knoten][stop_node4]

4. Um zu bestätigen, dass die Rolleninstanzen sind nicht mehr in Azure ausgeführt wird, in der [klassischen Portal](https://manage.windowsazure.com)klicken Sie auf **Cloud Services** > *Your_cloud_service_name* > **Instanzen**. Keine Instanzen werden in dieser Umgebung bereitgestellt werden.

    ![Keine Instanzen][view_instances2]

    Damit ist das Lernprogramm abgeschlossen.

## <a name="next-steps"></a>Nächste Schritte

* Untersuchen der Dokumentation [HPC Pack 2012 R2 und HPC Pack 2012](http://go.microsoft.com/fwlink/p/?LinkID=263697).

* Zum Einrichten einer Hybrid HPC Pack Clusterbereitstellung bei größer finden Sie unter [Azure Worker Rolle Instanzen mit Microsoft HPC Pack getrennt](http://go.microsoft.com/fwlink/p/?LinkID=200493).

* Weitere Möglichkeiten zum Erstellen eines HPC Pack Clusters in Azure, einschließlich der Verwendung von Azure Ressourcenmanager Vorlagen finden Sie unter [HPC Cluster Optionen mit Microsoft HPC Pack in Azure](../virtual-machines/virtual-machines-windows-hpcpack-cluster-options.md).
* Finden Sie unter [in Azure groß zu berechnen: technische Ressourcen für Stapel und High Performance Computing (HPC)](../batch/big-compute-resources.md) Weitere Informationen zu dem Bereich der große berechnen und HPC cloud in Azure Lösungen.


[Overview]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/hybrid_cluster_overview.png
[install_hpc1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc1.png
[install_hpc2]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc2.png
[install_hpc3]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc3.png
[install_hpc4]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc4.png
[install_hpc6]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc6.png
[install_hpc7]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc7.png
[install_hpc10]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc10.png
[upload_cert1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/upload_cert1.png
[createstorage1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/createstorage1.png
[createservice1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/createservice1.png
[config_hpc2]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc2.png
[config_hpc3]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc3.png
[config_hpc6]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc6.png
[config_hpc8]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc8.png
[config_hpc10]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc10.png
[config_hpc12]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc12.png
[config_hpc13]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc13.png
[add_node1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node1.png
[add_node1_1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node1_1.png
[add_node2]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node2.png
[add_node3]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node3.png
[add_node4]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node4.png
[add_node5]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node5.png
[add_node6]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node6.png
[add_node7]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node7.png
[view_instances1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/view_instances1.png
[clusrun1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/clusrun1.png
[test_job1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/test_job1.png
[param_sweep1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/param_sweep1.png
[view_job361]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/view_job361.png
[view_job362]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/view_job362.png
[stop_node1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/stop_node1.png
[stop_node2]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/stop_node2.png
[stop_node4]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/stop_node4.png
[view_instances2]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/view_instances2.png

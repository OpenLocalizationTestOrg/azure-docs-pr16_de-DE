<properties
 pageTitle="Einrichten eines Windows RDMA Cluster ausgeführt Applications MPI | Microsoft Azure"
 description="Informationen Sie zum Erstellen eines Windows HPC Pack Clusters mit Größe H16r, H16mr, A8 oder A9 virtuellen Computern im Netzwerk Azure RDMA zu verwenden, um apps MPI auszuführen."
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="big-compute"
 ms.date="09/20/2016"
 ms.author="danlep"/>

# <a name="set-up-a-windows-rdma-cluster-with-hpc-pack-to-run-mpi-applications"></a>Einrichten eines Windows RDMA Clusters mit HPC Pack MPI Applikationen ausgeführt

Einrichten eines Windows RDMA Clusters in Azure mit [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) und [H-Serie oder berechnen ankommt A-Serie Instanzen](virtual-machines-windows-a8-a9-a10-a11-specs.md) parallele Nachricht übergeben Interface (MPI) Applications ausgeführt. Wenn Sie RDMA-fähigen, Windows Server-basierte Knoten in einem Cluster HPC Pack eingerichtet haben, kommunizieren Sie MPI Applikationen effizient über eine niedrige Wartezeit, hohen Durchsatz Netzwerk in Azure, die auf remote direkte Memory Access (RDMA) Technologie basiert.

Wenn Sie MPI Auslastung auf Linux virtuellen Computern ausführen, die Zugriff auf das Netzwerk Azure RDMA möchten, finden Sie unter [Einrichten einer Linux RDMA Cluster zur Ausführung MPI-Anwendung](virtual-machines-linux-classic-rdma-cluster.md).


## <a name="hpc-pack-cluster-deployment-options"></a>HPC Pack Cluster Bereitstellungsoptionen
Microsoft HPC Pack ist ein Tool, vorausgesetzt, ohne zusätzliche Kosten erstellen HPC Cluster für lokale oder Azure, Windows oder Linux HPC Applikationen ausführen. HPC Pack enthält eine Runtime-Umgebung für die Microsoft-Implementierung von der Nachricht übergeben Benutzeroberfläche für Windows (MS MPI). Bei Verwendung mit RDMA-fähigen Instanzen mit einem unterstützten Windows Server-Betriebssystem Möglichkeit HPC Pack effizient, ein Windows-MPI Programme ausführen, die Zugriff auf das Netzwerk Azure RDMA. 

In diesem Artikel werden zwei Szenarien und Links zu detaillierten Leitfaden zum Einrichten eines Winodws RDMA Clusters mit Microsoft HPC Pack vorgestellt. 

* Szenario 1. Bereitstellen von rechenintensiven Worker Rolleninstanzen (PaaS)

* Szenario 2. Bereitstellen von Computeknoten in rechenintensiven virtuellen Computern (IaaS)

Allgemeine Vorkenntnisse rechenintensiven Instanzen mit Windows verwenden finden Sie unter [About H-Serie und berechnen ankommt A-Serie virtuellen Computern](virtual-machines-windows-a8-a9-a10-a11-specs.md) .



## <a name="scenario-1-deploy-compute-intensive-worker-role-instances-paas"></a>Szenario 1. Bereitstellen von rechenintensiven Worker Rolleninstanzen (PaaS)

Hinzufügen von aus einer vorhandenen HPC Pack Cluster zusätzliche berechnen Ressourcen Azure Worker Rolle Instanzen (Azure Knoten) in einen Cloud-Service (PaaS) ausführen. Diese Funktion wird auch als "Spitzen in Azure" von HPC Pack, unterstützt einen Zellbereich Größen für die Worker Rolleninstanzen an. Beim Hinzufügen von der Azure Knoten, geben Sie eine der RDMA-fähigen Größen einfach.

Im folgenden werden Aspekte und Schritte zu RDMA-fähigen Spitzen Azure Instanzen aus einer vorhandenen (normalerweise lokal) Cluster. Verwenden Sie ähnliche Verfahren Worker Rolleninstanzen eine HPC Pack am Knoten hinzufügen, die in einer Azure-virtuellen Computer bereitgestellt wird.

>[AZURE.NOTE] Ein Lernprogramm zu Spitzen in Azure mit HPC Pack finden Sie unter [Einrichten von einem Hybriden Cluster mit HPC Pack](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md). Beachten Sie die Punkte in die folgenden Schritte aus, die speziell für RDMA-fähigen gelten Azure-Knoten.

![In Azure Spitzen][burst]

### <a name="steps"></a>Schritte

4. **Bereitstellen und Konfigurieren eines HPC Pack 2012 R2 am Knotens**

    Neueste HPC Pack Installationspaket aus dem [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=49922)herunterladen. Anforderungen und Anweisungen zur Vorbereitung einer bereitstellungs Azure Burst finden Sie unter [HPC Pack Leitfaden für erste Schritte](https://technet.microsoft.com/library/jj884144.aspx) und [Burst Azure Worker Instanzen mit Microsoft HPC Pack](https://technet.microsoft.com/library/gg481749.aspx).

5. **Konfigurieren eines Zertifikats Management im Azure-Abonnement**

    Konfigurieren Sie ein Zertifikat, um die Verbindung zwischen dem am Knoten und Azure zu sichern. Optionen und Verfahren finden Sie unter [Szenarien das Azure Management Zertifikat für HPC Pack konfigurieren](http://technet.microsoft.com/library/gg481759.aspx). Für Test-Bereitstellungen Installationen HPC Pack ein Standard Microsoft HPC Azure Management Zertifikat Sie schnell zu Ihrem Abonnement Azure hochladen können.

6. **Erstellen einer neuen Cloud-Dienst und einem Speicherkonto**

    Mithilfe des Azure klassischen Portals um einen Clouddienst und ein Speicherkonto für die Bereitstellung in einem Bereich zu erstellen, in dem die RDMA-fähigen Instanzen verfügbar sind.

7. **Erstellen einer Vorlage für Azure Knoten**

    Verwenden der Assistent zur Erstellung eines Vorlage in HPC Cluster Manager zu erstellen. Anweisungen finden Sie unter [Erstellen einer Vorlage Azure-Knoten](http://technet.microsoft.com/library/gg481758.aspx#BKMK_Templ) in "Schritte zum Bereitstellen von Azure Knoten mit Microsoft HPC Pack".

    Es wird empfohlen, für die ersten Tests Konfigurieren einer Richtlinie Verfügbarkeit der manuellen in der Vorlage.

8. **Hinzufügen von Knoten zum cluster**

    Verwenden der Assistent zur Erstellung eines in HPC Cluster Manager hinzufügen. Weitere Informationen finden Sie unter [Hinzufügen von Azure-Knoten mit dem Windows HPC Cluster](http://technet.microsoft.com/library/gg481758.aspx#BKMK_Add).

    Wenn Sie die Größe der Knoten angeben möchten, wählen Sie ein die RDMA-fähigen Instanzengrößen.
    
    >[AZURE.NOTE]In jeder Burst mit Azure-Bereitstellung mit den Instanzen berechnen ankommt bereitstellt HPC Pack automatisch mindestens 2 RDMA-fähigen Instanzen (z. B. A8) als Proxy Knoten zusätzlich zu den Azure Worker Rolleninstanzen, die Sie angeben. Verwenden Sie die Proxy-Knoten Kerne, die mit dem Abonnement zugeordnet sind und fallen Gebühren zusammen mit den Azure Worker Rolleninstanzen aus.

9. **Starten Sie (bereitstellen) Knoten und bringen Sie diese online Auftrag ausführen**

    Wählen Sie die Knoten aus, und verwenden Sie die Aktion **beginnen** in HPC Cluster Manager. Wenn provisioning abgeschlossen ist, wählen Sie die Knoten, und verwenden Sie die Aktion **Online schalten** in HPC Cluster Manager. Die Knoten sind bereit, um Aufgaben auszuführen.

10. **Übermitteln von Aufträgen zum cluster**

    Verwenden Sie HPC Pack Auftrag Einreichung Tools Cluster Auftrag ausführen. Finden Sie unter [Microsoft HPC Pack: Verwaltung der beruflichen Position](http://technet.microsoft.com/library/jj899585.aspx).

11. **Beenden (entziehen) Knoten**

    Wenn Sie laufenden Aufträge fertig sind, wie die Knoten offline, und verwenden Sie die Aktion **Beenden** in HPC Cluster Manager.





## <a name="scenario-2-deploy-compute-nodes-in-compute-intensive-vms-iaas"></a>Szenario 2. Bereitstellen von Computeknoten in rechenintensiven virtuellen Computern (IaaS)

In diesem Szenario Sie den HPC Pack am Knoten bereitstellen und Cluster berechnen Knoten auf virtuellen Computern eine Active Directory-Domäne in einem Azure-virtuellen Netzwerk hinzugefügt. HPC Pack bietet eine Reihe von [Optionen für die Bereitstellung in Azure-virtuellen Computern](virtual-machines-linux-hpcpack-cluster-options.md), einschließlich automatisierte Bereitstellungsskripts und Azure Schnellstart Vorlagen. Beispiel: Leitfaden Besonderheiten sowie die folgenden Schritte aus, das [HPC Pack IaaS Bereitstellungsskript](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) verwenden, um die meisten dieses Verfahren zu automatisieren.

![Cluster in Azure-virtuellen Computern][iaas]



### <a name="steps"></a>Schritte

1. **Erstellen Sie einen Cluster am Knoten und berechnen Sie Knoten virtuellen Computern, indem Sie das Bereitstellungsskript HPC Pack IaaS auf einem Clientcomputer ausgeführt**

    HPC Pack IaaS Bereitstellungsskript Paket aus dem [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=49922)herunterladen.

    Vorbereitung auf den Clientcomputer erstellen Sie die Skriptdatei Konfiguration, und führen Sie das Skript, finden Sie unter [Erstellen einer HPC Cluster mit der HPC Pack IaaS Bereitstellungsskript vor](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md). 
    
    Um RDMA-fähigen berechnen Knoten bereitstellen, beachten Sie folgende zusätzlichen Informationen ein:
    
    * **Virtuelle Netzwerk** - Geben Sie ein neues virtuelles Netzwerk in einem Bereich, in dem die RDMA-fähigen Instanzgröße zu verwendende steht.

    * **Betriebssystem Windows Server** - RDMA Konnektivität unterstützen, einem Betriebssystem Windows Server 2012 R2 oder Windows Server 2012 für den Berechnungsknoten virtuellen Computern angeben.

    * **Cloud Services** - wir empfehlen die Bereitstellung Ihrer am in einen Cloud-Dienst und Ihre berechnen-Knoten in einem anderen Clouddienst.

    * **Kopf Knotengröße** – in diesem Szenario sollten Sie eine Größe von mindestens A4 (zusätzliche groß) für den am Knoten.

    * **HpcVmDrivers Erweiterung** - Bereitstellungsskript installiert der Azure-virtuellen Computer-Agent und die Erweiterung HpcVmDrivers automatisch, beim Bereitstellen von Größe A8 oder A9 Knoten mit einem Windows Server-Betriebssystem zu berechnen. HpcVmDrivers installiert Treiber auf dem Berechnungsknoten virtuellen Computern, damit diese mit dem Netzwerk RDMA verbinden können.

    * **Cluster-Netzwerkkonfiguration** - Bereitstellungsskript richtet automatisch HPC Pack Cluster in Suchtopologie 5 (alle Knoten auf das Unternehmensnetzwerk). Diese Suchtopologie ist für alle HPC Pack Cluster Bereitstellungen in virtuellen Computern erforderlich. Ändern Sie der Suchtopologie Cluster Netzwerk nicht später.

2. **Setzen Sie die berechnen Knoten online Auftrag ausführen**

    Wählen Sie die Knoten aus, und verwenden Sie die Aktion **Online schalten** in HPC Cluster Manager. Die Knoten sind bereit, um Aufgaben auszuführen.

3. **Übermitteln von Aufträgen zum cluster**

    Verbinden mit dem am Knoten Aufträge senden oder einer lokalen Computer Zweck einrichten. Informationen hierzu finden Sie unter [Senden Aufträge zu einem HPC Cluster in Azure](virtual-machines-windows-hpcpack-cluster-submit-jobs.md).

4. **Offlineschalten die Knoten und beenden (Freigeben),**

    Wenn Sie die laufenden Aufträge fertig sind, machen Sie die Knoten offline in HPC Cluster Manager. Verwenden Sie dann, Azure Verwaltungstools diese beenden.



## <a name="run-mpi-applications-on-the-cluster"></a>Führen Sie auf dem Cluster MPI applications

### <a name="example-run-mpipingpong-on-an-hpc-pack-cluster"></a>Beispiel: Mpipingpong ein Cluster HPC Pack ausgeführt

Führen Sie für die Überprüfung eine HPC Pack Bereitstellung der RDMA-fähigen Instanzen auf Cluster HPC Pack **Mpipingpong** Befehl ein. **Mpipingpong** sendet Pakete von Daten zwischen gepaarten Knoten mehrmals, bis der Wartezeit und Durchsatz Maße berechnen und Statistik für das Netzwerk RDMA aktivierten Anwendung. Dieses Beispiel zeigt ein typisches Muster für die Ausführung eines Auftrags MPI (in diesem Fall **Mpipingpong**) mithilfe des Befehls **Mpiexec** Cluster.

In diesem Beispiel wird davon ausgegangen, dass Sie in einer "Azure" Aufteilungskonfiguration ([Szenario 1](#scenario-1.-deploy-compute-intensive-worker-role-instances-(PaaS) in this article). Azure Knoten hinzugefügt Wenn Sie in einem Cluster aus Azure-virtuellen Computern HPC Pack bereitgestellt haben, müssen Sie die Befehlssyntax zum Angeben einer anderen Knoten Gruppe, und legen Sie weitere Umgebungsvariablen um Netzwerkdatenverkehr mit dem Netzwerk RDMA leiten ändern.


So führen Sie Mpipingpong im Cluster aus:


1. Öffnen Sie in den am Knoten oder auf einem ordnungsgemäß konfigurierten Clientcomputer ein Eingabeaufforderungsfenster.

2. Zum Schätzen der Wartezeit zwischen Paare von Knoten in einer Azure Burst Bereitstellung von 4 Knoten, geben Sie den folgenden Befehl zum Senden eines Auftrags zum Mpipingpong über eine kleine Paketgröße und eine große Anzahl der Iterationen ausführen:

    ```
    job submit /nodegroup:azurenodes /numnodes:4 mpiexec -c 1 -affinity mpipingpong -p 1:100000 -op -s nul
    ```

    Der Befehl gibt die ID des Projekts, die gesendet wird.

    Wenn Sie bereitgestellten auf Azure-virtuellen Computern, geben Sie eine Gruppe von Knoten, die enthält HPC Pack Cluster bereitgestellt berechnen Sie Knoten, die virtuellen Computern in einer einzelnen Cloud-Dienst bereitgestellt, und ändern Sie den Befehl **Mpiexec** wie folgt:

    ```
    job submit /nodegroup:vmcomputenodes /numnodes:4 mpiexec -c 1 -affinity -env MSMPI_DISABLE_SOCK 1 -env MSMPI_PRECONNECT all -env MPICH_NETMASK 172.16.0.0/255.255.0.0 mpipingpong -p 1:100000 -op -s nul
    ```

3. Bei Auftragsabschluss, zum Anzeigen der Ausgabe (in diesem Fall die Ausgabe der Aufgabe 1 des Auftrags), geben Sie Folgendes ein

    ```
    task view <JobID>.1
    ```

    wo &lt; *Auftrags* &gt; ist die ID des Projekts, dem übermittelt wurde.

    Die Ausgabe wird Wartezeit Ergebnisse ähnlich wie der folgende enthalten.

    ![Ping Pong Wartezeit][pingpong1]

4. Geben Sie den folgenden Befehl zum Senden eines Auftrags zum Ausführen von **Mpipingpong** mit einer großen Paketgröße und eine kleine Anzahl von Iterationen zum Schätzen der Durchsatz zwischen Paare von Azure Burst Knoten:

    ```
    job submit /nodegroup:azurenodes /numnodes:4 mpiexec -c 1 -affinity mpipingpong -p 4000000:1000 -op -s nul
    ```

    Der Befehl gibt die ID des Projekts, die gesendet wird.

    Auf einem HPC Pack-Cluster auf Azure-virtuellen Computern bereitgestellt ändern Sie den Befehl wie in Schritt 2 notiert haben.

5. Bei Auftragsabschluss, zum Anzeigen der Ausgabe (in diesem Fall die Ausgabe der Aufgabe 1 des Auftrags), geben Sie Folgendes ein:

    ```
    task view <JobID>.1
    ```

  Die Ausgabe wird ähnlich wie der folgende Durchsatzergebnissen enthalten.

  ![Ping Pong Durchsatz][pingpong2]


### <a name="mpi-application-considerations"></a>MPI Anwendung Aspekte


Im folgenden sind für die Ausführung von Applications MPI mit HPC Pack in Azure Aspekte. Einige gelten nur für die Bereitstellung von Azure Knoten (Worker Rolleninstanzen in einer "Azure" Aufteilungskonfiguration hinzugefügt).

* Worker Rolleninstanzen in einen Cloud-Dienst werden regelmäßig ohne vorherige Ankündigung von Azure reprovisioned (zum Beispiel für Systemwartung oder in Groß-/Kleinschreibung, die eine Instanz des schlägt fehl). Wenn eine Instanz während der Ausführung eines Auftrags MPI reprovisioned ist, wird die Instanz Daten verloren und in den Zustand zurück, wenn sie zuerst bereitgestellt wurde, die den Auftrag MPI ein Fehler führen können. Die für einen einzigen MPI Auftrag, die mehr den Auftrag Verwendung und weitere Knoten ausgeführt wird, desto höher die Wahrscheinlichkeit, dass eine der Instanzen während eines Auftrags reprovisioned werden wird ausgeführt wird. Dies ist auch sollten, wenn Sie einen einzelnen Knoten in der Bereitstellung als Dateiserver festzulegen.


* Um MPI Aufträge in Azure ausführen zu können, verfügen Sie möglicherweise nicht die RDMA-fähigen Instanzen verwenden. Sie können jede beliebige Instanzgröße, die von HPC Pack unterstützt wird. Die RDMA-fähigen Instanzen werden jedoch für die Ausführung von relativ umfangreiche MPI Aufträge, die hängen von der Wartezeit und die Bandbreite des Netzwerks, das die Knoten verbindet empfohlen. Wenn Sie andere Größen verwenden, um vertrauliche Wartezeit und Bandbreite MPI Aufträge ausgeführt werden, wird empfohlen, kleine Aufträge, in denen eine einzelne Aufgabe nur wenige Knoten ausgeführt wird ausgeführt.

* Anwendung auf Azure Instanzen bereitgestellt unterliegen den der Anwendung zugeordneten zur Lizenzierung Ausdrücke aus. Wenden Sie sich an den Hersteller der jeden kommerzielle Antrag auf zur Lizenzierung oder eine andere Einschränkungen für die Ausführung von in der Cloud. Nicht alle Hersteller bieten Lizenzierung je nach Bedarf berechnet.


* Azure Instanzen benötigen weitere Setup zum Zugriff auf lokale Knoten Freigaben und Lizenzservern. Um die Azure Knoten Zugriff auf einen lokalen Lizenz-Server zu aktivieren, können Sie beispielsweise ein Standort-zu-Standort Azure-virtuelles Netzwerk konfigurieren.


* Um auf Azure Instanzen MPI-Programme ausführen zu können, registrieren Sie sich jede MPI-Anwendung mit Windows-Firewall auf die Instanzen durch Ausführen des Befehls **Hpcfwutil** . Dadurch wird die MPI Kommunikation auf einem Port stattfinden, die von der Firewall dynamisch zugewiesen wird.

    >[AZURE.NOTE] Für Paketburst mit Azure Bereitstellungen können Sie auch den Befehl eines Firewall-Ausnahme automatisch für alle neuen Azure-Knoten ausgeführt werden, die Ihren Cluster hinzugefügt werden, konfigurieren. Nachdem Sie den **Hpcfwutil** -Befehl ausführen, und stellen Sie sicher, dass die Anwendung, fügen Sie den Befehl zum eines Skripts zum Starten für Ihre Azure-Knoten. Weitere Informationen finden Sie unter [Verwenden eines Skripts zum Starten für Azure-Knoten](https://technet.microsoft.com/library/jj899632.aspx).



* HPC Pack verwendet die Variable CCP_MPI_NETMASK Cluster-Umgebung, um einen Zellbereich zulässigen Adressen für die Kommunikation MPI anzugeben. HPC Pack 2012 R2 ab, die CCP_MPI_NETMASK Cluster-Umgebungsvariable wirkt sich nur auf MPI-Kommunikation zwischen Cluster Domänenverbund berechnen Knoten (entweder lokal oder in Azure-virtuellen Computern). Die Variable wird von Knoten hinzugefügt in einen Paketburst Azure Konfiguration ignoriert.


* MPI Aufträge können nicht über Azure Instanzen ausgeführt werden, die in anderen Clouddienste (beispielsweise im Burst mit Azure Bereitstellungen mit anderen Knoten Vorlagen oder Azure-virtuellen Computer Datenverarbeitungsknoten in mehreren Cloud Services bereitgestellt) bereitgestellt werden. Wenn Sie mehrere Azure Knoten Bereitstellungen, die mit anderen Knoten Vorlagen gestartet werden verfügen, muss der Auftrag MPI auf nur einen Satz von Azure Knoten ausgeführt werden.


* Wenn Sie Ihren Cluster Azure Knoten hinzu, und sie online schalten, versucht der HPC Auftragszeitplandienst sofort zu Aufträge beginnen, die auf den Knoten. Wenn nur ein Teil Ihrer Arbeitsbelastung Azure ausgeführt werden kann, stellen Sie sicher, dass Sie aktualisieren oder Ihre Position Vorlagen zum definieren, welche Typen von Position auf Azure ausgeführt werden können. Angenommen, um sicherzustellen, dass nur Aufträge mit einer Auftragsvorlage übermittelt auf Azure Knoten ausgeführt werden, die Position Vorlage die Eigenschaft Knotengruppen hinzu, und wählen Sie AzureNodes als den gewünschten Wert aus. Verwenden Sie zum Erstellen benutzerdefinierter Gruppen für Ihre Azure Knoten des hinzufügen-HpcGroup HPC PowerShell-Cmdlets ein.


## <a name="next-steps"></a>Nächste Schritte

* Entwickeln Sie als Alternative bei der Verwendung von HPC Pack mit dem Dienst Azure Stapel auf verwalteten Speicherpools Datenverarbeitungsknoten in Azure MPI Programme ausführen. Finden Sie unter [Verwendung mit mehreren Instanzen Aufgaben zur Ausführung Nachricht übergeben Interface (MPI) Anwendung in Azure Blatt](../batch/batch-mpi.md).

* Wenn Sie Linux MPI Programme ausführen, die Zugriff auf das Netzwerk Azure RDMA möchten, finden Sie unter [Einrichten von einem Linux RDMA Cluster zur Ausführung MPI-Anwendung](virtual-machines-linux-classic-rdma-cluster.md).

<!--Image references-->
[burst]: ./media/virtual-machines-windows-classic-hpcpack-rdma-cluster/burst.png
[iaas]: ./media/virtual-machines-windows-classic-hpcpack-rdma-cluster/iaas.png
[pingpong1]: ./media/virtual-machines-windows-classic-hpcpack-rdma-cluster/pingpong1.png
[pingpong2]: ./media/virtual-machines-windows-classic-hpcpack-rdma-cluster/pingpong2.png
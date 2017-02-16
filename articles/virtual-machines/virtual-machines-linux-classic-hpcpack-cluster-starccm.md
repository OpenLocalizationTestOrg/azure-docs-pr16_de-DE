<properties
 pageTitle="Ausführen von Stern-CCM + mit HPC Pack auf Linux virtuellen Computern | Microsoft Azure"
 description="Bereitstellen einen Microsoft HPC Pack Cluster auf Azure, und führen Sie einen Stern mit-CCM + Position auf mehrere Linux Knoten über ein Netzwerk RDMA zu berechnen."
 services="virtual-machines-linux"
 documentationCenter=""
 authors="xpillons"
 manager="timlt"
 editor=""
 tags="azure-service-management,azure-resource-manager,hpc-pack"/>
<tags
 ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="big-compute"
 ms.date="09/13/2016"
 ms.author="xpillons"/>

# <a name="run-star-ccm-with-microsoft-hpc-pack-on-a-linux-rdma-cluster-in-azure"></a>Ausführen von Stern-CCM + mit Microsoft HPC Pack auf einem Linux RDMA cluster in Azure
In diesem Artikel wird gezeigt, wie einen Microsoft HPC Pack Cluster Azure und Ausführen Bereitstellen einer [CD-Adapco Stern-CCM +](http://www.cd-adapco.com/products/star-ccm%C2%AE) Position auf mehreren Linux berechnen Knoten, die mit InfiniBand miteinander verbunden sind.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Microsoft HPC Pack enthält Funktionen, mit eine Vielzahl von umfangreichen HPC und parallele-Anwendungen wie MPI-Anwendungen auf Cluster aus Microsoft Azure-virtuellen Computern ausführen. HPC Pack unterstützt auch laufende Linux HPC Applikationen auf Linux berechnen-Knoten virtuellen Computern, die in einem Cluster HPC Pack bereitgestellt werden. Eine Einführung in die Verwendung von Linux berechnen Sie Knoten mit HPC Pack zu, finden Sie unter [Erste Schritte mit Linux Datenverarbeitungsknoten in einer HPC Pack Cluster in Azure](virtual-machines-linux-classic-hpcpack-cluster.md).

## <a name="set-up-an-hpc-pack-cluster"></a>Richten Sie eine HPC Pack cluster
Die HPC Pack IaaS Bereitstellungsskripts aus dem [Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=44949) herunterladen und lokal zu extrahieren.

Azure PowerShell ist Voraussetzung. Wenn PowerShell nicht auf dem lokalen Computer konfiguriert ist, lesen Sie den Artikel [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).

Zum Zeitpunkt der Erstellung dieses Dokuments werden Linux Bilder aus dem Azure Marketplace (die die InfiniBand Treiber für Azure enthält) für SLES 12, CentOS 6.5 und CentOS 7.1 aus. Dieser Artikel basiert auf der Nutzung der SLES 12. Wenn Sie den Namen aller Linux Bilder abzurufen, die auf der Marketplace HPC zu unterstützen, können Sie den folgenden PowerShell-Befehl ausführen:

```
    get-azurevmimage | ?{$_.ImageName.Contains("hpc") -and $_.OS -eq "Linux" }
```

Die Ausgabe führt den Speicherort aus, in dem diese Bilder verfügbar sind, und die Bildnamen (**ImageName**), die später in der Bereitstellungsvorlage verwendet werden soll.

Bevor Sie den Cluster bereitstellen, müssen Sie eine HPC Pack Bereitstellung Vorlagendatei erstellen. Da wir einen kleinen Cluster verwendet haben, wird der am Knoten werden die Domänencontroller und hosten eine lokale SQL-Datenbank.

Die folgende Vorlage wird solcher einen am Knoten bereitstellen, erstellen eine XML-Datei mit dem Namen **MyCluster.xml**und Ersetzen Sie die Werte von **SubscriptionId**, **StorageAccount**, **Ort**, **VMName**und **ServiceName** mit Ihrem.

    <?xml version="1.0" encoding="utf-8" ?>
    <IaaSClusterConfig>
      <Subscription>
        <SubscriptionId>99999999-9999-9999-9999-999999999999</SubscriptionId>
        <StorageAccount>mystorageaccount</StorageAccount>
      </Subscription>
      <Location>North Europe</Location>
      <VNet>
        <VNetName>hpcvnetne</VNetName>
        <SubnetName>subnet-hpc</SubnetName>
      </VNet>
      <Domain>
        <DCOption>HeadNodeAsDC</DCOption>
        <DomainFQDN>hpc.local</DomainFQDN>
      </Domain>
      <Database>
        <DBOption>LocalDB</DBOption>
      </Database>
      <HeadNode>
        <VMName>myhpchn</VMName>
        <ServiceName>myhpchn</ServiceName>
        <VMSize>Standard_D4</VMSize>
      </HeadNode>
      <LinuxComputeNodes>
        <VMNamePattern>lnxcn-%0001%</VMNamePattern>
        <ServiceNamePattern>mylnxcn%01%</ServiceNamePattern>
        <MaxNodeCountPerService>20</MaxNodeCountPerService>
        <StorageAccountNamePattern>mylnxstorage%01%</StorageAccountNamePattern>
        <VMSize>A9</VMSize>
        <NodeCount>0</NodeCount>
        <ImageName>b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-hpc-v20150708</ImageName>
      </LinuxComputeNodes>
    </IaaSClusterConfig>

Starten Sie die Erstellung der Kopf-Knoten durch Ausführen des PowerShell-Befehls in ein erweitertes Eingabeaufforderungsfenster:

```
    .\New-HPCIaaSCluster.ps1 -ConfigFile MyCluster.xml
```

Nach 20 bis 30 Minuten sollten der am Knoten bereit. Sie können vom Azure-Portal zu verbinden, indem Sie auf das Symbol **Verbinden** des virtuellen Computers.

Sie müssen möglicherweise später beheben die DNS-Weiterleitung. Starten Sie hierzu DNS-Manager.

1.  Mit der rechten Maustaste in des Servernamens im DNS-Manager, wählen Sie **Eigenschaften**aus, und klicken Sie dann auf die Registerkarte **Weiterleitungen** .

2.  Klicken Sie auf die Schaltfläche **Bearbeiten** , um eine Weiterleitung zu entfernen, und klicken Sie dann auf **OK**.

3.  Stellen Sie sicher, dass das Kontrollkästchen **Hinweise auf das Stammverzeichnis verwenden, wenn keine Weiterleitung verfügbar sind** , ausgewählt ist, und klicken Sie dann auf **OK**.

## <a name="set-up-linux-compute-nodes"></a>Einrichten von Linux berechnen Knoten
Sie bereitstellen Computeknoten Linux mithilfe derselben Bereitstellungsvorlage, die Sie zum Erstellen des am Knotens verwendet.

Kopieren Sie die Datei **MyCluster.xml** von Ihrem lokalen Computer, auf den Knoten am, und aktualisieren Sie die Kategorie **NodeCount** mit der Anzahl der Knoten, die Sie bereitstellen möchten (< = 20). Achten Sie darauf, haben Sie genügend verfügbaren Kerne in Ihr Azure-Kontingent, da jede Instanz A9 16 Kerne in Ihrem Abonnement genutzt wird. Sie können A8 Instanzen (8 Kerne) statt A9 verwenden, wenn Sie weitere virtuellen Computern in demselben Budget verwenden möchten.

Kopieren Sie die Skripts HPC Pack IaaS Bereitstellung auf dem Knoten am.

Führen Sie die folgenden Azure PowerShell-Befehle in ein erweitertes Eingabeaufforderungsfenster:

1.  Führen Sie die **Hinzufügen-AzureAccount** für die Verbindung zu Ihrem Azure-Abonnement.

2.  Wenn Sie mehrere Abonnements haben, führen Sie **Get-AzureSubscription** , um sie aufzulisten.

3.  Festlegen ein Standard-Abonnements durch Ausführen der **auswählen-AzureSubscription - SubscriptionName Xxxx-Standard** Befehl.

4.  Führen Sie **.\New-HPCIaaSCluster.ps1 - ConfigFile MyCluster.xml** zum Starten der Bereitstellung von Linux Hierarchieknoten zu berechnen.

    ![Bereitstellung von Kopf-Knoten in Aktion][hndeploy]

Öffnen Sie das Tool HPC Pack Cluster-Manager. Nach dem einige Minuten werden Linux Datenverarbeitungsknoten regelmäßig in der Liste der Cluster berechnen Knoten angezeigt. IaaS virtuellen Computern sind mit der Bereitstellung klassischen Modus fortlaufend erstellt. Damit ist die Anzahl der Knoten wichtig, kann alle bereitgestellt erste sehr viel Zeit dauern.

![Linux Knoten HPC Pack Cluster Manager][clustermanager]

Nachdem Sie nun alle Knoten im Cluster einsatzbereit sind, gibt es sind zusätzliche Einstellungen vornehmen.

## <a name="set-up-an-azure-file-share-for-windows-and-linux-nodes"></a>Einrichten einer Azure Dateifreigabe für Windows und Linux Knoten
Den Dienst Azure-Datei können Sie die um Skripts, Anwendungspakete und Datendateien zu speichern. Azure-Datei bietet CIFS-Funktionen auf Azure Blob-Speicher als einen permanenten Speicher. Beachten Sie, dass dies nicht die am häufigsten skalierbare Lösung ist, aber es ist die einfachste und dedizierte virtuellen Computern setzt voraus.

Erstellen einer Azure Dateifreigabe anhand der Anweisungen im Artikel [Erste Schritte mit auf Windows Azure-Datei-Speicher](..\storage\storage-dotnet-how-to-use-files.md).

Behalten Sie den Namen Ihres Kontos Speicher als **Saname**, den Dateinamen für die Freigabe als **Freigabename**und im Speicher kontoschlüssel als **sakey**.

### <a name="mount-the-azure-file-share-on-the-head-node"></a>Bereitstellen der Azure Dateifreigabe auf dem am Knoten
Öffnen Sie ein erweitertes Eingabeaufforderungsfenster, und führen Sie den folgenden Befehl zum Speichern der Anmeldeinformationen im Tresor lokaler Computer:

```
    cmdkey /add:<saname>.file.core.windows.net /user:<saname> /pass:<sakey>
```

Klicken Sie dann, um die Azure Dateifreigabe bereitzustellen, ausführen:

```
    net use Z: \\<saname>.file.core.windows.net\<sharename> /persistent:yes
```

### <a name="mount-the-azure-file-share-on-linux-compute-nodes"></a>Laden Sie die Azure Dateifreigabe auf Linux berechnen Knoten
Eine ist hilfreich, die im Lieferumfang HPC Pack die Clusrun Tool. Dieses Tool Befehlszeile können denselben Befehl gleichzeitig auf eine Gruppe von Knoten berechnen ausführen. In diesem Fall wird es verwendet die Azure Dateifreigabe bereitstellen und es nach einem Neustart überstehen beibehalten.
Führen Sie die folgenden Befehle in ein erweitertes Eingabeaufforderungsfenster auf dem am Knoten.

So erstellen Sie ein Bereitstellungsverzeichnis

```
    clusrun /nodegroup:LinuxNodes mkdir -p /hpcdata
```

Die Azure Dateifreigabe bereitstellen zu können:

```
    clusrun /nodegroup:LinuxNodes mount -t cifs //<saname>.file.core.windows.net/<sharename> /hpcdata -o vers=2.1,username=<saname>,password='<sakey>',dir_mode=0777,file_mode=0777
```

Die Freigabe bereitstellen beibehalten werden:

```
    clusrun /nodegroup:LinuxNodes "echo //<saname>.file.core.windows.net/<sharename> /hpcdata cifs vers=2.1,username=<saname>,password='<sakey>',dir_mode=0777,file_mode=0777 >> /etc/fstab"
```

## <a name="install-star-ccm"></a>Installieren von Stern-CCM +
Azure virtueller Computer Instanzen A8 und A9 bieten InfiniBand Support und RDMA-Funktionen. Die Kerneltreibern, mit denen diese Funktionen stehen für Windows Server 2012 R2, SUSE 12, CentOS 6.5 und CentOS 7.1 Bilder in der Azure Marketplace. Microsoft MPI und Intel MPI (Version 5.x) werden die zwei MPI-Bibliotheken, die diese Treiber in Azure unterstützen.

CD-Adapco Stern-CCM + lassen Sie wieder los 11.x und höher ist zusammen mit Intel MPI Version 5.x, damit InfiniBand Unterstützung für Azure enthalten ist.

Abrufen der Linux64 Stern-Paket CCM + über das [CD-Adapco Portal](https://steve.cd-adapco.com). In diesem Fall wird die Version 11.02.010 in gemischten Genauigkeit verwendet.

Erstellen Sie auf dem Knoten am in Azure Dateifreigabe **/hpcdata** ein Shell-Skript namens **setupstarccm.sh** mit dem folgenden Inhalt aus. Dieses Skript wird ausgeführt werden, auf den einzelnen Knoten berechnen Stern einrichten-CCM + lokal.

#### <a name="sample-setupstarcmsh-script"></a>Beispiel setupstarcm.sh-Skript
```
    #!/bin/bash
    # setupstarcm.sh to set up STAR-CCM+ locally

    # Create the CD-adapco main directory
    mkdir -p /opt/CD-adapco

    # Copy the STAR-CCM package from the file share to the local directory
    cp /hpcdata/StarCCM/STAR-CCM+11.02.010_01_linux-x86_64.tar.gz /opt/CD-adapco/

    # Extract the package
    tar -xzf /opt/CD-adapco/STAR-CCM+11.02.010_01_linux-x86_64.tar.gz -C /opt/CD-adapco/

    # Start a silent installation of STAR-CCM without the FLEXlm component
    /opt/CD-adapco/starccm+_11.02.010/STAR-CCM+11.02.010_01_linux-x86_64-2.5_gnu4.8.bin -i silent -DCOMPUTE_NODE=true -DNODOC=true -DINSTALLFLEX=false

    # Update memory limits
    echo "*               hard    memlock         unlimited" >> /etc/security/limits.conf
    echo "*               soft    memlock         unlimited" >> /etc/security/limits.conf
```
Jetzt einrichten Stern-CCM + auf alle Ihre Linux Knoten zu berechnen, öffnen Sie ein erweitertes Eingabeaufforderungsfenster und den folgenden Befehl ausführen:

```
    clusrun /nodegroup:LinuxNodes bash /hpcdata/setupstarccm.sh
```

Während der Befehl ausgeführt wird, können Sie die CPU-Auslastung mithilfe der wärmebilds von Cluster Manager überwachen. Nach dem einige Minuten sollten alle Knoten ordnungsgemäß eingerichtet werden.

## <a name="run-star-ccm-jobs"></a>Ausführen von Stern-CCM + Aufträge
HPC Pack ist für seine Position Scheduler Funktionen verwendet, um Stern ausführen-CCM + Aufträge. Dazu benötigen wir die Unterstützung von wenigen Skripts, die verwendet werden, um den Auftrag starten und Ausführen von Stern-CCM +. Die Eingabedaten werden auf der Dateifreigabe Azure zur Vereinfachung ersten gespeichert.

Das folgende PowerShell-Skript wird verwendet, um einen Stern Warteschlange-CCM + Position. Es hat drei Argumente:

*  Der Name des Modells

*  Die Anzahl der Knoten verwendet werden

*  Die Anzahl der Kerne auf den einzelnen Knoten verwendet werden

Da Stern-CCM + ausfüllen die Arbeitsspeicherbandbreite kann es empfiehlt sich normalerweise weniger Kerne pro Datenverarbeitungsknoten und Hinzufügen von neuen Knoten. Die genaue Anzahl der Kerne pro Knoten wird der Prozessoren und der Geschwindigkeit der Verbindung abhängig sind.

Die Knoten sind ausschließlich für den Auftrag vorgesehen sind und nicht gemeinsam mit anderen Einzelvorgänge. Der Auftrag ist nicht direkt als eine MPI Auftrag gestartet. Die Shellskript **runstarccm.sh** wird das Startprogramm für das MPI gestartet.

Das Modell Eingabe- und das Skript **runstarccm.sh** werden in der Freigabe **/hpcdata** gespeichert, die zuvor bereitgestellt wurde.

Protokolldateien werden mit der Position ID benannt und befinden sich an den **/hpcdata freigeben**, zusammen mit den Stern-CCM + Dateien ausgeben.


#### <a name="sample-submitstarccmjobps1-script"></a>Beispiel SubmitStarccmJob.ps1-Skript
```
    Add-PSSnapin Microsoft.HPC -ErrorAction silentlycontinue
    $scheduler="headnodename"
    $modelName=$args[0]
    $nbCoresPerNode=$args[2]
    $nbNodes=$args[1]

    #---------------------------------------------------------------------------------------------------------
    # Create a new job; this will give us the job ID that's used to identify the name of the uploaded package in Azure
    #
    $job = New-HpcJob -Name "$modelName $nbNodes $nbCoresPerNode" -Scheduler $scheduler -NumNodes $nbNodes -NodeGroups "LinuxNodes" -FailOnTaskFailure $true -Exclusive $true
    $jobId = [String]$job.Id

    #---------------------------------------------------------------------------------------------------------
    # Submit the job    
    $workdir =  "/hpcdata"
    $execName = "$nbCoresPerNode runner.java $modelName.sim"

    $job | Add-HpcTask -Scheduler $scheduler -Name "Compute" -stdout "$jobId.log" -stderr "$jobId.err" -Rerunnable $false -NumNodes $nbNodes -Command "runstarccm.sh $execName" -WorkDir "$workdir"


    Submit-HpcJob -Job $job -Scheduler $scheduler
```
Ersetzen Sie **runner.java** mit Ihrem bevorzugten Stern-Startprogramm für das CCM + Java-Modell und Protokollierungscode.

#### <a name="sample-runstarccmsh-script"></a>Beispiel runstarccm.sh-Skript
```
    #!/bin/bash
    echo "start"
    # The path of this script
    SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
    echo ${SCRIPT_PATH}
    # Set the mpirun runtime environment
    export CDLMD_LICENSE_FILE=1999@flex.cd-adapco.com

    # mpirun command
    STARCCM=/opt/CD-adapco/STAR-CCM+11.02.010/star/bin/starccm+

    # Get node information from ENVs
    NODESCORES=(${CCP_NODES_CORES})
    COUNT=${#NODESCORES[@]}
    NBCORESPERNODE=$1

    # Create the hostfile file
    NODELIST_PATH=${SCRIPT_PATH}/hostfile_$$
    echo ${NODELIST_PATH}

    # Get every node name and write into the hostfile file
    I=1
    NBNODES=0
    while [ ${I} -lt ${COUNT} ]
    do
        echo "${NODESCORES[${I}]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
        let "NBNODES=${NBNODES}+1"
    done
    let "NBCORES=${NBNODES}*${NBCORESPERNODE}"

    # Run STAR-CCM with the hostfile argument
    #  
    ${STARCCM} -np ${NBCORES} -machinefile ${NODELIST_PATH} \
        -power -podkey "<yourkey>" -rsh ssh \
        -mpi intel -fabric UDAPL -cpubind bandwidth,v \
        -mppflags "-ppn $NBCORESPERNODE -genv I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -genv I_MPI_DAPL_UD=0 -genv I_MPI_DYNAMIC_CONNECTION=0" \
        -batch $2 $3
    RTNSTS=$?
    rm -f ${NODELIST_PATH}

    exit ${RTNSTS}
```

In unserem Test verwendet haben wir ein Power On Demand Lizenztoken. Für diese Token, müssen Sie die Variable **$CDLMD_LICENSE_FILE** Umgebung festgelegt **1999@flex.cd-adapco.com** und die Taste in der Option **- Podkey** der Befehlszeile.

Nach der Initialisierung einige extrahiert das Skript – **$CCP_NODES_CORES** Umgebungsvariablen, die HPC Pack festlegen – die Liste der Knoten eine Host-Datei erstellen, die Startprogramm für das MPI verwendet. Diese Host-Datei wird die Liste der Namen der berechnen Knoten enthalten, die für das Projekt, einen Namen pro Zeile verwendet werden.

Das Format der **$CCP_NODES_CORES** folgt diesem Muster:

```
<Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>...`
```

Wobei Folgendes gilt:

* `<Number of nodes>`ist die Anzahl der Knoten, dieses Projekt zugewiesen wurden.

* `<Name of node_n_...>`ist der Name der einzelnen Knoten, dieses Projekt zugewiesen wurden.

* `<Cores of node_n_...>`ist die Anzahl der Kerne auf dem Knoten, dieses Projekt zugewiesen wurden.

Die Anzahl der Kerne (**$NBCORES**) wird auch basierend auf der Anzahl der Knoten (**$NBNODES**) und die Anzahl der Kerne pro Knoten (als Parameter **$NBCORESPERNODE**bereitgestellt) berechnet.

Informationen über die Optionen MPI sind diejenigen, die mit Intel MPI auf Azure verwendet werden:

*   `-mpi intel`Intel MPI angeben.

*   `-fabric UDAPL`Azure InfiniBand Verben verwenden zu können.

*   `-cpubind bandwidth,v`zur Optimierung der Bandbreite für MPI mit Stern-CCM +.

*   `-mppflags "-ppn $NBCORESPERNODE -genv I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -genv I_MPI_DAPL_UD=0 -genv I_MPI_DYNAMIC_CONNECTION=0"`Arbeiten mit Azure InfiniBand Intel-MPI vornehmen, und legen Sie die Anzahl der Kerne pro Knoten.

*   `-batch`So starten Sie Stern-CCM + im Stapelverarbeitungsmodus ohne Benutzeroberfläche.


Um einen Auftrag zu starten, stellen Sie schließlich sicher, dass Ihre Knoten einsatzbereit und sind in Cluster Manager online ein. Führen Sie dann aus einer PowerShell-Eingabeaufforderung folgt:

```
    .\ SubmitStarccmJob.ps1 <model> <nbNodes> <nbCoresPerNode>
```

## <a name="stop-nodes"></a>Beenden von Knoten
Später aktivieren, nachdem Sie mit der Tests fertig sind, können Sie die folgenden Befehle HPC Pack PowerShell beenden, und starten Sie Knoten:

```
    Stop-HPCIaaSNode.ps1 -Name <prefix>-00*
    Start-HPCIaaSNode.ps1 -Name <prefix>-00*
```

## <a name="next-steps"></a>Nächste Schritte
Führen Sie Linux aufgrund der Ergebnisse aus. Beispielsweise finden Sie unter:

* [Linux Datenverarbeitungsknoten in Azure NAMD mit Microsoft HPC Pack ausgeführt](virtual-machines-linux-classic-hpcpack-cluster-namd.md)

* [Eine Linux RDMA Cluster in Azure OpenFOAM mit Microsoft HPC Pack ausgeführt](virtual-machines-linux-classic-hpcpack-cluster-openfoam.md)


<!--Image references-->
[hndeploy]: ./media/virtual-machines-linux-classic-hpcpack-cluster-starccm/hndeploy.png
[clustermanager]: ./media/virtual-machines-linux-classic-hpcpack-cluster-starccm/ClusterManager.png

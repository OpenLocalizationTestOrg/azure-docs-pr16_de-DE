<properties
 pageTitle="Linux berechnen virtuellen Computern in einem Cluster HPC Pack | Microsoft Azure"
 description="Informationen Sie zum Erstellen und Verwenden eines HPC Pack Clusters in Azure für Linux high Performance computing Auslastung (HPC)"
 services="virtual-machines-linux"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management,azure-resource-manager,hpc-pack"/>
<tags
 ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="big-compute"
 ms.date="10/12/2016"
 ms.author="danlep"/>

# <a name="get-started-with-linux-compute-nodes-in-an-hpc-pack-cluster-in-azure"></a>Erste Schritte mit Linux Datenverarbeitungsknoten in einer HPC Pack Cluster in Azure

Einrichten eines [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029.aspx) Clusters in Azure, die eine am unter Windows Server und mehrere berechnen Knoten Ausführen einer unterstützten Linux Verteilung enthält. Probieren Sie die Optionen zum Verschieben von Daten zwischen den Linux und der am Windows-Knoten im Cluster. Erfahren Sie, wie Linux HPC Aufträge zum Cluster senden.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


Auf hoher Ebene zeigt das folgende Diagramm HPC Pack Cluster, Sie erstellen und bearbeiten.

![HPC Pack Cluster mit Linux-Knoten][scenario]


Weitere Optionen auf Linux HPC Auslastung Azure ausgeführt werden finden Sie unter [Technische Ressourcen für Stapel und High Performance computing](../batch/big-compute-resources.md).

## <a name="deploy-an-hpc-pack-cluster-with-linux-compute-nodes"></a>Bereitstellen eines HPC Pack Clusters mit Linux berechnen Knoten

Dieser Artikel zeigt, dass Sie zwei Optionen zum Bereitstellen eines HPC Pack Clusters in Azure mit Linux Hierarchieknoten zu berechnen. Beide Methoden verwenden ein Bilds Marketplace von Windows Server mit HPC Pack zum Erstellen des am Knotens. 

* **Vorlage Ressourcenmanager Azure** - Verwenden einer Vorlage aus dem Azure Marketplace oder einer Vorlage Schnellstart aus der Community, um die Erstellung der Cluster im Bereitstellungsmodell Ressourcenmanager automatisieren. Zum Beispiel erstellt die [HPC Pack Cluster für Linux Auslastung](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) Vorlage in der Azure Marketplace eine vollständige HPC Pack Cluster Infrastruktur für Linux HPC Auslastung.

* **PowerShell-Skript** - verwenden Sie das [Microsoft HPC Pack IaaS Bereitstellungsskript](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) (**Neu-HpcIaaSCluster.ps1**) zu eine Clusterbereitstellung abgeschlossen im Bereitstellungsmodell klassischen automatisieren. Diese Azure PowerShell-Skript ein Bild HPC Pack virtueller Computer in der Azure Marketplace verwendet, für die schnelle Bereitstellung und bietet eine umfassende Reihe von Konfigurationsparameter Linux Datenverarbeitungsknoten bereitstellen.

Weitere Informationen zu HPC Pack Cluster Bereitstellungsoptionen in Azure finden Sie unter [Optionen zum Erstellen und Verwalten eines leistungsfähige computing (HPC) Cluster in Azure mit Microsoft HPC Pack](virtual-machines-linux-hpcpack-cluster-options.md).

### <a name="prerequisites"></a>Erforderliche Komponenten

* **Azure-Abonnement** – Sie können ein Abonnement in der globalen Azure oder China Azure-Dienst verwenden. Wenn Sie kein Konto haben, können Sie ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) nur wenigen Minuten erstellen.

* **Kerne Kontingent** - möglicherweise müssen Sie das Kontingent der Kerne, erhöhen insbesondere dann, wenn Sie festlegen, dass mehrere Clusterknoten mit Multikernprozessoren virtueller Computer Maßen bereitstellen. Um eine Kontingent zu erhöhen, öffnen Sie eine Supportanfrage online Kunden kostenlos.

* **Linux-Versionen** - aktuell HPC Pack unterstützt die folgenden Linux-Versionen für Knoten berechnen. Sie können diese Verteilung Marketplace-Versionen verwenden, sofern verfügbar, oder eigene angeben.

    * **CentOS-basierten**: 6.5, 6.6, 6,7, 7.0, 7.1, 7.2, 6.5 HPC, 7.1 HPC
    * **Red Hat Enterprise Linux**: 6,7, 6.8, 7.2
    * **SUSE Linux Enterprise Server**: SLES 12 SLES 12 (Premium), SLES 12 SP1, SLES 12 SP1 (Premium), SLES 12 für HPC, SLES 12 für HPC (Premium)
    * **Ubuntu-Server**: 14.04 LTS, 16.04 LTS

    >[AZURE.TIP]Um das Netzwerk Azure RDMA mit einer der Größen RDMA-fähigen virtuellen Computer verwenden zu können, geben Sie ein SUSE Linux Enterprise Server 12 HPC oder HPC CentOS gespeichertes Bild aus dem Azure Marketplace. Weitere Informationen finden Sie unter [H-Serie und berechnen ankommt A-Serie virtuellen Computern](virtual-machines-linux-a8-a9-a10-a11-specs.md).

Zusätzliche erforderliche Komponenten Cluster mithilfe des HPC Pack IaaS Bereitstellungsskripts bereitgestellt:

* **Clientcomputer** - benötigten auf einem Windows-basiertem Clientcomputer das Bereitstellungsskript Cluster ausführen.

* **Azure PowerShell** - [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) (Version 0.8.10 oder höher) auf dem Clientcomputer.

* **Bereitstellungsskript HPC Pack IaaS** -, und Entpacken Sie die neueste Version des Skripts vom [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949)herunterladen. Sie können die Version des Skripts überprüfen, indem Sie ausführen `.\New-HPCIaaSCluster.ps1 –Version`. Dieser Artikel basiert auf Version 4.4.1 oder höher des Skripts.

### <a name="deployment-option-1-use-a-resource-manager-template"></a>Bereitstellungsoption 1. Verwenden einer Vorlage Ressourcenmanager

1. Wechseln Sie zu der Vorlage [HPC Pack Cluster für Linux Auslastung](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) in dem Azure Marketplace, und klicken Sie auf **Bereitstellen**.

2. Der Azure-Portal überprüfen Sie die Informationen, und klicken Sie dann auf **Erstellen**.

    ![Portal erstellen][portal]

3. Geben Sie in die **Grundlagen** Blade einen Namen für den Cluster, der den am Knoten virtueller Computer auch Namen ein. Sie können eine vorhandene Ressourcengruppe auswählen oder erstellen Sie eine Gruppe für die Bereitstellung an einem Speicherort, der Ihnen zur Verfügung. Die Position wirkt sich auf die Verfügbarkeit von bestimmter virtueller Computer Größen und andere Azure-Dienste (siehe [Produkte nach Region verfügbar](https://azure.microsoft.com/regions/services/)) aus.

4. Klicken Sie auf der Blade **Kopf Knoten Einstellungen** für eine erste Bereitstellung können Sie die Standardeinstellungen im Allgemeinen akzeptieren. 

    >[AZURE.NOTE]**Nach der Konfiguration Skript-URL** ist eine optionale Einstellung zu einer öffentlich zugänglichen Windows PowerShell-Skript Geben Sie die gewünschten auf dem am Knoten virtueller Computer ausgeführt werden, nachdem er ausgeführt wird. 
    
5. Wählen Sie in der Blade **Knoten Einstellungen zu berechnen** ein naming Muster für Knoten, die Anzahl und die Größe des Knoten und der Linux Verteilung bereitstellen.

6. In den **Einstellungen Infrastruktur** Blade, geben Sie einen Namen für die virtuelle Netzwerk- und Active Directory Domäne, Domäne und virtueller Computer Administratorberechtigungen und ein naming Muster für den Speicherkonten.

    >[AZURE.NOTE]HPC Pack mithilfe Cluster Benutzer authentifiziert Active Directory-Domäne. 

7. Nachdem die Überprüfung Tests auszuführen, und überprüfen Sie die Nutzungsbedingungen, klicken Sie auf **kaufen**.


### <a name="deployment-option-2-use-the-iaas-deployment-script"></a>Bereitstellungsoption 2. Verwenden Sie das Bereitstellungsskript IaaS

Im folgenden werden weitere Voraussetzungen für Cluster mithilfe des HPC Pack IaaS Bereitstellungsskripts bereitstellen:

* **Clientcomputer** - benötigten auf einem Windows-basierten Clientcomputer das Bereitstellungsskript Cluster ausführen.

* **Azure PowerShell** - [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) (Version 0.8.10 oder höher) auf dem Clientcomputer.

* **Bereitstellungsskript HPC Pack IaaS** - herunterladen und Entpacken die neueste Version des Skripts vom [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949). Sie können die Version des Skripts überprüfen, indem Sie ausführen `.\New-HPCIaaSCluster.ps1 –Version`. Dieser Artikel basiert auf Version 4.4.1 oder höher des Skripts.

**XML-Konfigurationsdatei**

Das Bereitstellungsskript HPC Pack IaaS verwendet eine XML-Konfigurationsdatei als Eingabe beschreiben HPC-Cluster. Die folgende Beispielkonfigurationsdatei gibt einen kleinen Cluster, die mit einer HPC Pack am und zwei Größe A7 CentOS 7.0 Linux berechnen Knoten an. 

Ändern Sie die Datei für Ihre Umgebung und Clusterkonfiguration gewünschte nach Bedarf, und speichern Sie es mit einem Namen wie HPCDemoConfig.xml. Beispielsweise müssen Sie den Namen Ihres Abonnements und einen Kontonamen eindeutige Speicher angeben und cloud-Dienstnamen. Darüber hinaus sollten Sie ein anderes unterstütztes Linux Bild für die berechnen-Knoten auswählen. Weitere Informationen zu den Elementen in der Konfigurationsdatei finden Sie unter der Manual.rtf-Datei im Skriptordner und [Erstellen einer HPC Cluster mit HPC Pack IaaS Bereitstellungsskript](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md).

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>allvhdsje</StorageAccount>
  </Subscription>
  <Location>Japan East</Location>  
  <VNet>
    <VNetName>centos7rdmavnetje</VNetName>
    <SubnetName>CentOS7RDMACluster</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>CentOS7RDMA-HN</VMName>
    <ServiceName>centos7rdma-je</ServiceName>
  <VMSize>ExtraLarge</VMSize>
  <EnableRESTAPI />
  <EnableWebPortal />
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>CentOS7RDMA-LN%1%</VMNamePattern>
    <ServiceName>centos7rdma-je</ServiceName>
    <VMSize>A7</VMSize>
    <NodeCount>2</NodeCount>
    <ImageName>5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20150325</ImageName>
  </LinuxComputeNodes>
</IaaSClusterConfig>
```

**Das HPC Pack IaaS Bereitstellungsskript ausführen**

1. Windows PowerShell als Administrator auf dem Clientcomputer zu öffnen.

2. Änderung Verzeichnis zu dem Ordner, in dem das Skript ist, installiert (in diesem Beispiel E:\IaaSClusterScript).

    ```powershell
    cd E:\IaaSClusterScript
    ```

3. Führen Sie den folgenden Befehl HPC Pack Cluster bereitstellen. In diesem Beispiel wird davon ausgegangen, dass die Konfigurationsdatei in E:\HPCDemoConfig.xml befindet

    ```powershell
    .\New-HpcIaaSCluster.ps1 –ConfigFile E:\HPCDemoConfig.xml –AdminUserName MyAdminName
    ```

    ein. Da der **AdminPassword** im vorstehenden Befehl nicht angegeben ist, werden Sie aufgefordert, das Kennwort für Benutzer *MyAdminName*einzugeben.

    b. Das Skript beginnt dann die Konfigurationsdatei überprüfen. Es kann bis zu je nach der Netzwerkverbindung einige Minuten dauern.

    ![Überprüfung][validate]

    c. Nach Validierungen übergeben, enthält das Skript die Clusterressourcen zu erstellen. Geben Sie *Y* , um den Vorgang fortzusetzen.

    ![Ressourcen][resources]

    d. Das Skript beginnt den Cluster HPC Pack bereitstellen und schließt die Konfiguration ohne weitere manuelle Schritte. Das Skript ausgeführt werden kann für einige Minuten.

    ![Bereitstellen][deploy]
    
    >[AZURE.NOTE]In diesem Beispiel generiert das Skript automatisch eine Protokolldatei, da der **Protokolldatei -** Parameter nicht angegeben wird. Die Protokolle in Echtzeit geschrieben nicht zur Verfügung, aber am Ende der Validierung und die Bereitstellung zusammengestellt werden. Wenn der PowerShell-Prozess beendet wird, während das Skript ausgeführt wird, sind einige Protokolle gehen verloren.

## <a name="connect-to-the-head-node"></a>Verbinden Sie mit dem am Knoten

Nach der Bereitstellung von HPC Pack Cluster in Azure zum am Knoten virtueller Computer mit der Domäne [Verbinden durch Remotedesktop](virtual-machines-windows-connect-logon.md) Anmeldeinformationen Sie vorausgesetzt, wenn Sie den Cluster bereitgestellt (z. B. *Hpc\\Clusteradmin*). Sie verwalten den Cluster aus dem am Knoten.

Starten Sie auf die am Knoten HPC Cluster Manager zum Überprüfen des Status des HPC Pack Cluster aus. Sie können verwalten, und Monitor Linux berechnen Knoten genauso arbeiten mit Windows berechnen Knoten. Beispielsweise wird die Linux-Knoten in **Ressourcenverwaltung** (diese Knoten werden mit der Vorlage **LinuxNode** bereitgestellt) aufgelistet.

![Verwaltung von Knoten][management]

Sie finden Sie auch auf die Linux-Knoten in der Ansicht **Wärmebilds** .

![Wärmebilds][heatmap]

## <a name="how-to-move-data-in-a-cluster-with-linux-nodes"></a>Zum Verschieben von Daten in einem Cluster mit Linux Knoten

Sie haben eine Reihe von Optionen zum Verschieben von Daten zwischen Linux und den Windows am Knoten im Cluster. Hier werden drei allgemeine Methoden, in den folgenden Abschnitten ausführlicher beschrieben:

* **Datei Azure** - stellt eine verwaltete SMB Dateifreigabe zum Speichern von Daten in Azure-Speicher zur Verfügung. Windows und Linux Knoten können eine Azure Dateifreigabe als Laufwerk oder Ordner zur gleichen Zeit, bereitstellen, selbst wenn sie in verschiedenen virtuellen Netzwerken bereitgestellt haben.

* **Kopf Knoten SMB freigeben** – hängt einen standard Windows freigegebenen Ordner, der den am Knoten auf Linux-Knoten.

* **Kopf Knoten NFS-Server** - stellt eine Dateifreigabe Lösung für gemischten Windows- und Linux-Umgebung.

### <a name="azure-file-storage"></a>Azure Dateispeicher

Die [Datei Azure](https://azure.microsoft.com/services/storage/files/) Service macht Dateifreigaben mit dem standardmäßigen SMB 2.1-Protokoll verfügbar. Azure-virtuellen Computern und Cloud Services können Dateidaten über Anwendungskomponenten über bereitgestellten Freigaben freizugeben und zu lokalen Applikationen können Dateidaten in einer Freigabe durch den Dateispeicher API zugreifen. 

Die detaillierten Schritte zum Erstellen einer Azure Dateifreigabe und Laden es auf der am Knoten finden Sie unter [Erste Schritte mit auf Windows Azure-Datei-Speicher](../storage/storage-dotnet-how-to-use-files.md). Um die Azure Dateifreigabe auf die Linux-Knoten bereitstellen zu können, finden Sie unter [Azure Dateispeicher mit Linux verwenden](../storage/storage-how-to-use-files-linux.md). Zum Beibehalten von Verbindungen eingerichtet haben, finden Sie unter [Persisting Verbindungen mit Microsoft Azure-Dateien](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx).

Erstellen Sie im folgenden Beispiel eine Azure Dateifreigabe auf einem Speicherkonto ein. Um die Freigabe auf dem am Knoten bereitstellen möchten, öffnen Sie ein Eingabeaufforderungsfenster, und geben Sie die folgenden Befehle:

```command
cmdkey /add:allvhdsje.file.core.windows.net /user:allvhdsje /pass:<storageaccountkey>

net use Z: \\allvhdje.file.core.windows.net\rdma /persistent:yes
```

In diesem Beispiel Allvhdsje ist Ihren Kontonamen Speicher Storageaccountkey ist, Ihren kontoschlüssel Speicher und Rdma ist der Name der Azure freigeben. Die Azure Dateifreigabe ist als z auf dem am Knoten bereitgestellt.

Um die Azure Dateifreigabe auf Linux Knoten bereitzustellen, führen Sie einen Befehl **Clusrun** auf dem am Knoten aus. **[ClusRun](https://technet.microsoft.com/library/cc947685.aspx)** sehr hilfreich HPC Pack hierfür zur Durchführung von Verwaltungsaufgaben auf mehreren Knoten. (Siehe auch [Clusrun für Linux-Knoten](#Clusrun-for-Linux-nodes) in diesem Artikel.)

Öffnen Sie ein Windows PowerShell-Fenster, und geben Sie die folgenden Befehle:

```powershell
clusrun /nodegroup:LinuxNodes mkdir -p /rdma

clusrun /nodegroup:LinuxNodes mount -t cifs //allvhdsje.file.core.windows.net/rdma /rdma -o vers=2.1`,username=allvhdsje`,password=<storageaccountkey>'`,dir_mode=0777`,file_mode=0777
```

Der erste Befehl erstellt einen Ordner namens /rdma auf allen Knoten in der Gruppe LinuxNodes. Der zweite Befehl hängt von der Azure-Datei freigeben allvhdsjw.file.core.windows.net/rdma auf den Ordner /rdma mit Verzeichnis und Datei Modus Bits auf 777 gesetzt. Im zweiten Befehl Allvhdsje ist Ihren Kontonamen Speicher und Storageaccountkey Ihren kontoschlüssel Speicher.

>[AZURE.NOTE]Die "\`" Symbol im zweiten Befehl ist eine Escapesymbol für PowerShell. "\`," bedeutet, dass die "," (Komma) ein Teil des Befehls ist.

### <a name="head-node-share"></a>Am Knoten freigeben

Stellen Sie alternativ einen freigegebenen Ordner, der den am Knoten auf Linux Knoten bereit. Eine Freigabe bietet die einfachste Methode zum Freigeben von Dateien, aber die am und alle Linux-Knoten in der gleichen virtuellen Netzwerk bereitgestellt werden müssen. Hier sind die Schritte aus.

1. Erstellen eines Ordners auf dem am Knoten, und teilen Sie sie für jede Person mit Lese-und Schreibberechtigungen. Beispielsweise D:\OpenFOAM freigeben, auf dem am Knoten als \\CentOS7RDMA-HN\OpenFOAM. Hier sind CentOS7RDMA-HN der Hostname des am Knotens.

    ![Berechtigungen für die Dateifreigabe][fileshareperms]

    ![Freigeben von Dateien][filesharing]

2. Öffnen Sie ein Windows PowerShell-Fenster, und führen Sie die folgenden Befehle:

    ```powershell
    clusrun /nodegroup:LinuxNodes mkdir -p /openfoam

    clusrun /nodegroup:LinuxNodes mount -t cifs //CentOS7RDMA-HN/OpenFOAM /openfoam -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
    ```

Der erste Befehl erstellt einen Ordner namens /openfoam auf allen Knoten in der Gruppe LinuxNodes. Der zweite Befehl hängt von der freigegebenen Ordner //CentOS7RDMA-HN/OpenFOAM auf den Ordner mit dem Verzeichnis und Datei Modus Bits auf 777 gesetzt. Der Benutzername und Kennwort in den Befehl sollte der Benutzername und Kennwort eines Benutzers Cluster auf dem am Knoten sein. (Siehe [Hinzufügen oder entfernen, die Benutzer cluster](https://technet.microsoft.com/library/ff919330.aspx).)

>[AZURE.NOTE]Die "\`" Symbol im zweiten Befehl ist eine Escapesymbol für PowerShell. "\`," bedeutet, dass die "," (Komma) ein Teil des Befehls ist.


### <a name="nfs-server"></a>NFS-server

NFS-Dienst können Sie zum Freigeben und zum Migrieren von Dateien zwischen Computern unter Windows Server 2012 Betriebssystem über das Protokoll SMB und Linux-Computern mithilfe des NFS-Protokolls. Der NFS-Server und allen anderen Knoten in der gleichen virtuellen Netzwerk bereitgestellt werden müssen. Es sorgt für eine bessere Kompatibilität mit Linux Knoten im Vergleich zu einer SMB-Freigabe. Beispielsweise unterstützt diese Datei Links.

1. Installieren und Einrichten von einem NFS-Server, folgen Sie den Schritten [für System ersten Dateifreigabe im Netzwerk-End-to-End-Server](http://blogs.technet.com/b/filecab/archive/2012/10/08/server-for-network-file-system-first-share-end-to-end.aspx).

    Erstellen Sie beispielsweise einer NFS-Freigabe mit dem Namen Nfs mit den folgenden Eigenschaften:

    ![NFS-Autorisierung][nfsauth]

    ![NFS-Freigabeberechtigungen][nfsshare]

    ![NFS NTFS-Berechtigungen][nfsperm]

    ![Eigenschaften für NFS-Verwaltung][nfsmanage]

2. Öffnen Sie ein Windows PowerShell-Fenster, und führen Sie die folgenden Befehle:

    ```powershell
    clusrun /nodegroup:LinuxNodes mkdir -p /nfsshare

    clusrun /nodegroup:LinuxNodes mount CentOS7RDMA-HN:/nfs /nfsshared
    ```

  Der erste Befehl erstellt einen Ordner namens /nfsshared auf allen Knoten in der Gruppe LinuxNodes. Der zweite Befehl hängt die NFS-Freigabe CentOS7RDMA-HN: / Nfs auf den Ordner. Hier CentOS7RDMA-HN: / Nfs ist der remote-Pfad Ihrer NFS-Freigabe.

## <a name="how-to-submit-jobs"></a>Wie Aufträge senden
Es gibt mehrere Methoden zum Cluster HPC Pack Aufträge senden:

* HPC Cluster Manager oder über die Benutzeroberfläche von HPC Auftrags-Manager

* HPC Web-portal

* REST-API

Senden des Auftrags zum Cluster in Azure über HPC Pack Benutzeroberfläche Tools und das Web-Portal HPC sind die gleichen wie bei Windows Hierarchieknoten zu berechnen. Finden Sie unter [HPC Pack Auftrags-Manager](https://technet.microsoft.com/library/ff919691.aspx) und [So Aufträge aus einer lokalen Clientcomputer zu übermitteln](virtual-machines-windows-hpcpack-cluster-submit-jobs.md).

Um Aufträge über die REST-API übermitteln, finden Sie unter [Erstellen und Senden von Aufträgen mithilfe der REST-API in Microsoft HPC Pack](http://social.technet.microsoft.com/wiki/contents/articles/7737.creating-and-submitting-jobs-by-using-the-rest-api-in-microsoft-hpc-pack-windows-hpc-server.aspx). Um einen Linux-Client-Aufträge übermitteln, finden Sie auch in der Stichprobe Python im [HPC Pack SDK](https://www.microsoft.com/download/details.aspx?id=47756).

## <a name="clusrun-for-linux-nodes"></a>ClusRun für Linux Knoten

Befehle Linux Knoten entweder über ein Eingabeaufforderungsfenster oder HPC Cluster Manager ausführen, kann das HPC Pack [Clusrun](https://technet.microsoft.com/library/cc947685.aspx) -Tool verwendet werden. Es folgen einige grundlegende Beispiele.

* Anzeigen von aktuellen Benutzernamen auf allen Knoten im Cluster.

    ```command
    clusrun whoami
    ```

* Installieren Sie das Debuggertool **Gdb** mit **Yum** auf allen Knoten in der Gruppe Linuxnodes und starten Sie die Knoten nach 10 Minuten.

    ```command
    clusrun /nodegroup:linuxnodes yum install gdb –y; shutdown –r 10
    ```

* Anzeigen von jeder Zahl zwischen 1 und 10 für eine Sekunde auf jedem Linux Knoten im Cluster Shellskript erstellen, auszuführen und Ausgabe von den Knoten sofort anzeigen.

    ```command
    clusrun /interleaved /nodegroup:linuxnodes echo \"for i in {1..10}; do echo \\\"\$i\\\"; sleep 1; done\" ^> script.sh; chmod +x script.sh; ./script.sh
    ```

>[AZURE.NOTE] Sie müssen bestimmte Escapezeichen **Clusrun** Befehle verwenden. Verwenden Sie wie im folgenden Beispiel dargestellt, ^ in einem Eingabeaufforderungsfenster Escapezeichen für die ">" Symbol.

## <a name="next-steps"></a>Nächste Schritte

* Skalierung von Cluster auf eine größere Anzahl von Knoten, oder versuchen Sie eine Linux Arbeitsbelastung auf dem Cluster ausgeführt. Ein Beispiel finden Sie unter [Ausführen von NAMD mit Microsoft HPC Pack unter Linux Knoten in Azure zu berechnen](virtual-machines-linux-classic-hpcpack-cluster-namd.md).

* Versuchen Sie, einen Cluster mit [RDMA-fähigen, berechnen ankommt virtuellen Computern](virtual-machines-windows-a8-a9-a10-a11-specs.md) MPI Auslastung ausführen. Ein Beispiel finden Sie unter [OpenFOAM mit Microsoft HPC Pack auf einem Linux RDMA Cluster in Azure ausführen](virtual-machines-linux-classic-hpcpack-cluster-openfoam.md).

* Wenn Sie bei der Arbeit mit Linux-Knoten in einem lokalen HPC Pack Cluster interessiert sind, finden Sie in der [TechNet-Anleitung](https://technet.microsoft.com/library/mt595803.aspx).

<!--Image references-->
[scenario]: ./media/virtual-machines-linux-classic-hpcpack-cluster/scenario.png
[portal]: ./media/virtual-machines-linux-classic-hpcpack-cluster/portal.png
[validate]: ./media/virtual-machines-linux-classic-hpcpack-cluster/validate.png
[resources]: ./media/virtual-machines-linux-classic-hpcpack-cluster/resources.png
[deploy]: ./media/virtual-machines-linux-classic-hpcpack-cluster/deploy.png
[management]: ./media/virtual-machines-linux-classic-hpcpack-cluster/management.png
[heatmap]: ./media/virtual-machines-linux-classic-hpcpack-cluster/heatmap.png
[fileshareperms]: ./media/virtual-machines-linux-classic-hpcpack-cluster/fileshare1.png
[filesharing]: ./media/virtual-machines-linux-classic-hpcpack-cluster/fileshare2.png
[nfsauth]: ./media/virtual-machines-linux-classic-hpcpack-cluster/nfsauth.png
[nfsshare]: ./media/virtual-machines-linux-classic-hpcpack-cluster/nfsshare.png
[nfsperm]: ./media/virtual-machines-linux-classic-hpcpack-cluster/nfsperm.png
[nfsmanage]: ./media/virtual-machines-linux-classic-hpcpack-cluster/nfsmanage.png

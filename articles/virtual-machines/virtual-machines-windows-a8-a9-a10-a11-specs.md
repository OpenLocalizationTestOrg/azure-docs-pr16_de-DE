<properties
 pageTitle="Informationen zum Berechnen ankommt virtueller Computer mit Windows | Microsoft Azure"
 description="Abrufen von Hintergrundinformationen und Aspekte für die Verwendung der Azure H-Serie und A8, A9, A10 und A11 rechenintensiven Größen für Windows-virtuellen Computern und Cloud-Dienste"
 services="virtual-machines-windows, cloud-services"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="infrastructure-services"
 ms.date="09/21/2016"
 ms.author="danlep"/>

# <a name="about-h-series-and-compute-intensive-a-series-vms"></a>Informationen zum H-Serie und berechnen ankommt A-Serie virtuellen Computern

Hier ist Hintergrundinformationen sowie einige Punkte, für die Verwendung der neueren Azure H Serie die früheren A8, A9, A10 und A11 Instanzen, auch bekannt als *berechnen ankommt* Instanzen aus. Dieser Artikel befasst sich diese Instanzen für virtuelle Windows-Computer verwenden. In diesem Artikel wird auch für [Linux virtuellen Computern](virtual-machines-linux-a8-a9-a10-a11-specs.md)verfügbar.


[AZURE.INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## <a name="access-to-the-rdma-network"></a>Zugriff auf das Netzwerk RDMA

Sie können Cluster RDMA-fähige Windows Server-Instanzen erstellen und Bereitstellen eine unterstützte Implementierung von MPI im Netzwerk Azure RDMA nutzen. Dieses Netzwerk Tiefst-Wartezeit und ist für MPI-Verkehr nur reserviert.

* **Betriebssystem**
    * **Virtuellen Computern** – Windows Server 2012 R2, Windows Server 2012
    * **Cloud Services** - Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 Gast OS Familie

* **MPI** - Microsoft MPI (MS-MPI) 2012 R2 oder höher, Intel MPI-Bibliothek 5.x

Unterstützte MPI Implementierungen verwenden Sie die Microsoft Netzwerk direkte Schnittstelle für die Kommunikation zwischen Instanzen. Finden Sie unter [Einrichten von einem Windows-RDMA Cluster mit HPC Pack zur Ausführung MPI-Anwendung](virtual-machines-windows-classic-hpcpack-rdma-cluster.md) und [Verwendung mit mehreren Instanzen Aufgaben zur Ausführung Nachricht übergeben Interface (MPI) Anwendung in Azure Stapel](../batch/batch-mpi.md) Bereitstellungsoptionen und Konfigurationsschritte für die Stichprobe.


>[AZURE.NOTE]Auf virtuellen Computern RDMA-fähigen berechnen ankommt muss die Erweiterung HpcVmDrivers hinzugefügt werden, mit den virtuellen Computern Windows Netzwerk-Gerätetreiber zu installieren, die für RDMA Connectivity erforderlich sind. In den meisten Installationen wird die Erweiterung HpcVmDrivers automatisch hinzugefügt. Wenn Sie die Erweiterung selbst hinzufügen müssen, finden Sie unter [Verwalten von virtuellen Computer Erweiterungen](virtual-machines-windows-classic-manage-extensions.md).

## <a name="considerations-for-hpc-pack-and-windows"></a>Aspekte des HPC Pack und Windows

[Microsoft HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), Microsoft verfügbarer HPC Cluster und Position Management-Lösung, ist nicht für die Verwendung der rechenintensiven Instanzen mit Windows Server erforderlich. Es ist jedoch eine Möglichkeit für Sie zum Erstellen von einem berechnungscluster in Azure MPI Windows-basierten Anwendungen und andere HPC Auslastung ausführen. HPC Pack 2012 R2 und höher enthalten Runtime-Umgebung für MS MPI, die im Netzwerk Azure RDMA beim Bereitstellen auf RDMA-fähigen virtuellen Computern verwenden können.

Weitere Informationen und Checklisten rechenintensiven Instanzen mit HPC Pack unter Windows Server verwenden finden Sie unter [Einrichten von einem Windows-RDMA Cluster mit HPC Pack zur Ausführung MPI-Anwendung](virtual-machines-windows-classic-hpcpack-rdma-cluster.md).




## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen über die Verfügbarkeit und Preise der Größen berechnen ankommt finden Sie unter [virtuellen Computern Preise](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows) und [Preise Cloud Services](https://azure.microsoft.com/pricing/details/cloud-services/).

* Bereich der Speicherkapazität und Laufwerk – Details finden Sie unter [Größe für virtuelle Computer](virtual-machines-linux-sizes.md).

* Um anzufangen bereitstellen und berechnen ankommt Instanzen mit HPC Pack unter Windows verwenden, finden Sie unter [Einrichten von einem Windows-RDMA Cluster mit HPC Pack zur Ausführung MPI-Anwendung](virtual-machines-windows-classic-hpcpack-rdma-cluster.md).

* Informationen zur Verwendung von A8 und A9 Instanzen MPI Applikationen mit Azure Stapel ausführen finden Sie unter [Verwendung mit mehreren Instanzen Aufgaben zur Ausführung Nachricht übergeben Interface (MPI) Anwendung in Azure Stapel](../batch/batch-mpi.md).

<properties
 pageTitle="Linux HPC Pack Cluster Optionen in der Cloud | Microsoft Azure"
 description="Erfahren Sie mehr über die Optionen, die mit Microsoft HPC Pack erstellen und verwalten einen Linux hohen Performance computing (HPC) Cluster in der Cloud Azure"
 services="virtual-machines-linux,cloud-services"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="big-compute"
 ms.date="09/26/2016"
 ms.author="danlep"/>

# <a name="options-with-hpc-pack-to-create-and-manage-an-hpc-cluster-in-azure-for-linux-workloads"></a>Optionen mit HPC Pack erstellen und Verwalten eines HPC Cluster in Azure für Linux Auslastung

[AZURE.INCLUDE [virtual-machines-common-hpcpack-cluster-options](../../includes/virtual-machines-common-hpcpack-cluster-options.md)]

Dieser Artikel befasst sich Optionen zum Linux Auslastung ausführen HPC Pack verwenden. Es gibt auch Optionen zum Ausführen von [Windows HPC Auslastung mit HPC Pack](virtual-machines-windows-hpcpack-cluster-options.md).

## <a name="run-an-hpc-pack-cluster-in-azure-vms"></a>Führen Sie einen HPC Pack Cluster im Azure-virtuellen Computern

### <a name="azure-templates"></a>Azure-Vorlagen


* (Marketplace) [HPC Pack Cluster für Linux Auslastung](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/)

* (Schnellstart) [Erstellen einer HPC Cluster mit Linux berechnen Knoten](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster-linux-cn)


### <a name="powershell-deployment-script"></a>PowerShell Bereitstellungsskript

* [Erstellen Sie einen Linux HPC Cluster mit HPC Pack IaaS Bereitstellungsskript](virtual-machines-linux-classic-hpcpack-cluster-powershell-script.md)

### <a name="tutorials"></a>Lernprogramme

* [Lernprogramm: Erste Schritte mit Linux Datenverarbeitungsknoten in einer HPC Pack Cluster in Azure](virtual-machines-linux-classic-hpcpack-cluster.md)

* [Lernprogramm: Ausführen NAMD mit Microsoft HPC Pack unter Linux berechnen Knoten in Azure](virtual-machines-linux-classic-hpcpack-cluster-namd.md)

* [Lernprogramm: Ausführen OpenFOAM mit Microsoft HPC Pack auf einem Linux RDMA Cluster in Azure](virtual-machines-linux-classic-hpcpack-cluster-openfoam.md)

* [Lernprogramm: Ausführen Stern-CCM + mit Microsoft HPC Pack auf einem Linux RDMA cluster in Azure](virtual-machines-linux-classic-hpcpack-cluster-starccm.md)

### <a name="cluster-management"></a>Cluster management

* [Senden Sie Aufträge zu einem Cluster HPC Pack in Azure](virtual-machines-windows-hpcpack-cluster-submit-jobs.md)

* [Projektmanagement in HPC Pack](https://technet.microsoft.com/library/jj899585.aspx)


## <a name="create-rdma-clusters-for-mpi-workloads"></a>RDMA Cluster für MPI Auslastung erstellen

* [Lernprogramm: Ausführen OpenFOAM mit Microsoft HPC Pack auf einem Linux RDMA Cluster in Azure](virtual-machines-linux-classic-hpcpack-cluster-openfoam.md)

* [Einrichten eines Linux RDMA Clusters MPI Applikationen ausgeführt](virtual-machines-linux-classic-rdma-cluster.md)


<properties
 pageTitle="Windows HPC Pack Cluster Optionen in der Cloud | Microsoft Azure"
 description="Erfahren Sie mehr über die Optionen, die mit Microsoft HPC Pack erstellen und verwalten einen Windows hohen Performance computing (HPC) Cluster in der Cloud Azure"
 services="virtual-machines-windows,cloud-services,batch"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="big-compute"
 ms.date="09/26/2016"
 ms.author="danlep"/>

# <a name="options-with-hpc-pack-to-create-and-manage-a-windows-hpc-cluster-in-azure"></a>Optionen mit HPC Pack erstellen und Verwalten eines Windows HPC Clusters in Azure

[AZURE.INCLUDE [virtual-machines-common-hpcpack-cluster-options](../../includes/virtual-machines-common-hpcpack-cluster-options.md)]

Dieser Artikel befasst sich HPC Pack Cluster zum Ausführen des Windows-Auslastung zu erstellen. Es gibt auch Optionen zum Erstellen von Cluster [Linux HPC Auslastung mit HPC Pack](virtual-machines-linux-hpcpack-cluster-options.md)ausgeführt.


## <a name="run-an-hpc-pack-cluster-in-azure-vms"></a>Führen Sie einen HPC Pack Cluster im Azure-virtuellen Computern

### <a name="azure-templates"></a>Azure-Vorlagen

* (Marketplace) [HPC Pack Cluster für Windows-Auslastung](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterwindowscn/)

* (Marketplace) [HPC Pack Cluster für Excel Auslastung](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterexcelcn/)

* (Schnellstart) [Erstellen einer HPC cluster](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster)

* (Schnellstart) [Erstellen einer HPC Cluster mit benutzerdefinierten berechnen Knotenbild](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster-custom-image)

### <a name="azure-vm-images"></a>Azure virtueller Computer Bilder

* [HPC Pack unter Windows Server 2012 R2](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/)

* [HPC Pack berechnen Knoten unter Windows Server 2012 R2](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2computenodeonwindowsserver2012r2/)

* [HPC Pack berechnen Knoten mit Excel unter Windows Server 2012 R2](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2computenodewithexcelonwindowsserver2012r2/)



### <a name="powershell-deployment-script"></a>PowerShell Bereitstellungsskript

* [Erstellen eines HPC Clusters mit HPC Pack IaaS Bereitstellungsskript](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md)

### <a name="tutorials"></a>Lernprogramme

* [Lernprogramm: Erste Schritte mit einem HPC Pack Cluster in Excel und SOA Auslastung ausführen Azure](virtual-machines-windows-excel-cluster-hpcpack.md)



### <a name="manual-deployment-with-the-azure-portal"></a>Manuelle Bereitstellung mit Azure-portal

* [Einrichten des am Knotens eine HPC Pack Cluster eine Azure-virtuellen Computer](virtual-machines-windows-hpcpack-cluster-headnode.md)

### <a name="cluster-management"></a>Cluster management

* [Verwalten von berechnen-Knoten in einem Cluster HPC Pack in Azure](virtual-machines-windows-classic-hpcpack-cluster-node-manage.md)

* [Vergrößern Sie und verkleinern Sie Azure berechnen von Ressourcen in einem Cluster HPC Pack](virtual-machines-windows-classic-hpcpack-cluster-node-autogrowshrink.md)

* [Senden Sie Aufträge zu einem Cluster HPC Pack in Azure](virtual-machines-windows-hpcpack-cluster-submit-jobs.md)

* [Projektmanagement in HPC Pack](https://technet.microsoft.com/library/jj899585.aspx)


## <a name="add-worker-role-nodes-to-an-hpc-pack-cluster"></a>Hinzufügen von Arbeitskollegen Rolle Knoten zu einem Cluster HPC Pack


* [Spitzen Sie-Azure Worker Instanzen mit HPC Pack](https://technet.microsoft.com/library/gg481749.aspx)

* [Lernprogramm: Einrichten eines Hybrid Clusters mit HPC Pack in Azure](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md)

* [Hinzufügen von Azure "Burst" Knoten zu einem am HPC Pack-Knoten in Azure](virtual-machines-windows-classic-hpcpack-cluster-node-burst.md)


## <a name="integrate-with-azure-batch"></a>Integrieren mit Azure Blattnamen 

* [Spitzen Sie-Azure Stapel mit HPC Pack](https://technet.microsoft.com/library/mt612877.aspx)

## <a name="create-rdma-clusters-for-mpi-workloads"></a>RDMA Cluster für MPI Auslastung erstellen

* [Einrichten eines Windows RDMA Clusters mit HPC Pack MPI Applikationen ausgeführt](virtual-machines-windows-classic-hpcpack-rdma-cluster.md)

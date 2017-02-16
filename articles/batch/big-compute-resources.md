<properties
   pageTitle="Ressourcen für Stapel und HPC Auslastung in der Cloud | Microsoft Azure"
   description="Technische Ressourcen aufgeführt, die Sie Ihre umfangreichen Parallel, Stapel und high Performance computing (HPC) Auslastung in Azure ausführen können."
   services="batch, cloud-services, virtual-machines"
   documentationCenter=""
   authors="dlepow"
   manager="timlt"
   editor=""/>

<tags
   ms.service="multiple"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="big-compute"
   ms.date="09/22/2016"
   ms.author="danlep"/>

# <a name="big-compute-in-azure-technical-resources-for-batch-and-high-performance-computing"></a>Groß in Azure zu berechnen: technische Ressourcen für Stapel und High Performance computing 
Dies ist einer Führungslinie auf technische Ressourcen, damit Sie Ihre umfangreichen Parallel, Stapel und High Performance computing (HPC) Auslastung in Azure ausführen können. Erweitern Sie Ihrer vorhandenen Stapel oder HPC Auslastung in der Cloud Azure, oder erstellen Sie neue große berechnen Lösungen mit einem Bereich von Azure Services.

## <a name="solutions-options"></a>Optionen von Lösungen

Erfahren Sie mehr über große berechnen Optionen in Azure, und wählen Sie den richtigen Ansatz für Ihre Arbeitsbelastung und Business müssen.

* [Stapel und HPC Lösungen](batch-hpc-solutions.md)

* [Video: Groß zu berechnen, in der Cloud mit Azure und HPC](https://azure.microsoft.com/documentation/videos/teched-europe-2014-big-compute-in-the-cloud-with-high-performance-computing-on-azure/)


## <a name="azure-batch"></a>Azure Stapel

[Stapel](https://azure.microsoft.com/services/batch/) ist ein Plattformdienst, der Cloud-Aktivieren einer Linux und Windows-Anwendung, und führen Aufträge ohne einrichten und Verwalten von einer Cluster und Job Scheduler erleichtert. Verwenden Sie das SDK über verschiedene Sprachen, Auslagern von Daten auf Azure-Clientanwendungen mit Azure Blattnamen integriert werden soll, und erstellen Sie Position Ausführung Pipelines.

* [Dokumentation](https://azure.microsoft.com/documentation/services/batch/)

* [.NET](https://msdn.microsoft.com/library/azure/mt348682.aspx), [Python](http://azure-sdk-for-python.readthedocs.io/latest/), [Node.js](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/), [Java](http://azure.github.io/azure-sdk-for-java/)und [REST](https://msdn.microsoft.com/library/azure/dn820158.aspx) -API-Referenz

* [Stapel Management .NET Bibliothek](https://msdn.microsoft.com/library/mt463120.aspx) Bezug

* Lernprogramme: Erste Schritte mit [Azure Stapel Bibliothek für .NET](batch-dotnet-get-started.md) und [Stapel Python-client](batch-python-tutorial.md)

* [Stapel-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurebatch)

* [Stapel videos](https://azure.microsoft.com/documentation/videos/index/?services=batch)

## <a name="hpc-cluster-solutions"></a>HPC Cluster-Lösungen

Erweitern Sie Ihre vorhandenen Windows oder Linux HPC Cluster in Azure Ihrer berechnen stark Auslastung ausführen oder bereitstellen.  

### <a name="microsoft-hpc-pack"></a>Microsoft HPC Pack

HPC Pack ist Microsoft kostenlosen HPC Lösung auf Basis einer Microsoft Azure und Windows Server-Technologien, Windows und Linux HPC Auslastung ausgeführt werden kann.  

* [Herunterladen von HPC Pack 2012 R2 Update 3](https://www.microsoft.com/download/details.aspx?id=49922)

* [Dokumentation](https://technet.microsoft.com/library/jj899572.aspx)


* Optionen für [Linux](../virtual-machines/virtual-machines-linux-hpcpack-cluster-options.md) und [Windows](../virtual-machines/virtual-machines-windows-hpcpack-cluster-options.md) HPC Pack Cluster in Azure

* [Spitzen Sie-Azure Worker Instanzen mit HPC Pack](https://technet.microsoft.com/library/gg481749.aspx)

* [Spitzen Sie-Azure Stapel mit HPC Pack](https://technet.microsoft.com/library/mt612877.aspx)


* [Windows HPC-Foren](https://social.microsoft.com/Forums/home?category=windowshpc)

### <a name="linux-and-oss-cluster-solutions"></a>Linux und OSS Cluster-Lösungen.

Verwenden Sie diese Vorlagen Azure Linux HPC Cluster bereitstellen.

* [Beginnen einer SLURM Cluster](https://azure.microsoft.com/documentation/templates/slurm/) , und die [Blogbeitrag](http://blogs.technet.com/b/windowshpc/archive/2015/06/06/deploy-a-slurm-cluster-on-azure.aspx)

* [Drehen von einem Drehmoment cluster](https://azure.microsoft.com/documentation/templates/torque-cluster/)

* [Intel-Cloud-Edition für Luestrierung Software - auswerten](https://azure.microsoft.com/marketplace/partners/intel/lustre-cloud-edition-evaleval-lustre-2-7/)

## <a name="microsoft-mpi"></a>Microsoft MPI

[Microsoft MPI](https://msdn.microsoft.com/library/bb524831.aspx) (MS MPI) ist ein Microsoft-Implementierung der Nachricht übergeben Schnittstelle standard für die Entwicklung und parallele Applikationen für die Windows-Plattform ausgeführt. Die neueste Version ist MS MPI Version 7.


* [MS-MPI herunterladen](http://go.microsoft.com/FWLink/p/?LinkID=389556)

* [MS MPI Referenz](https://msdn.microsoft.com/library/dn473458.aspx)

* [MPI-forum](https://social.microsoft.com/Forums/en-us/home?forum=windowshpcmpi)

## <a name="compute-intensive-instances"></a>Berechnen ankommt Instanzen

Azure bietet ein [Bereich des virtuellen Computer Größen](../virtual-machines/virtual-machines-windows-sizes.md), einschließlich [rechenintensiven](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md) Instanzen Herstellen einer Verbindung mit einem Back-End-RDMA Netzwerk, Ihre Linux und Windows HPC Auslastung ausgeführt werden kann.


* [Einrichten eines Linux RDMA Clusters MPI Applikationen ausgeführt](../virtual-machines/virtual-machines-linux-classic-rdma-cluster.md)

* [Einrichten eines Windows RDMA Clusters mit Microsoft HPC Pack MPI Applikationen ausgeführt](../virtual-machines/virtual-machines-windows-classic-hpcpack-rdma-cluster.md)



## <a name="samples-and-demos"></a>Beispiele und demos

* [Beispiele für Azure Stapel c# und Python-Codes](https://github.com/Azure/azure-batch-samples)

* [Stapel Werft](https://azure.github.io/batch-shipyard/) Toolkit zur einfachen Bereitstellung von Stapel-Schreibweise Dockerized Auslastung

* [HPC Laufwerk SUSE Linux Enterprise Server prüfen](https://azure.microsoft.com/marketplace/partners/suse/suselinuxenterpriseserver12optimizedforhighperformancecompute/)

## <a name="related-azure-services"></a>Verwandte Azure-Dienste

* [Daten Factory](https://azure.microsoft.com/documentation/services/data-factory/)

* [Computer-Schulung](https://azure.microsoft.com/documentation/services/machine-learning/)

* [HDInsight](https://azure.microsoft.com/documentation/services/hdinsight/)

* [Virtuellen Computern](https://azure.microsoft.com/documentation/services/virtual-machines/)

* [Virtuellen Computern skalieren Datensätze](https://azure.microsoft.com/documentation/services/virtual-machine-scale-sets/)

* [Cloud-Dienste](https://azure.microsoft.com/documentation/services/cloud-services/)

* [App-Dienst](https://azure.microsoft.com/documentation/services/app-service/)

* [Media-Dienste](https://azure.microsoft.com/documentation/services/media-services/)

* [Funktionen](https://azure.microsoft.com/documentation/services/functions/)

## <a name="architecture-blueprints"></a>Architektur Entwürfen

* [HPC und Daten Orchestrierung mit Azure Stapel und Azure Data Factory](http://go.microsoft.com/fwlink/?linkid=717686) (PDF) und [Artikel](../data-factory/data-factory-data-processing-using-batch.md)

## <a name="industry-solutions"></a>Industry Lösungen

* [Banking- und großes Märkten](https://finance.azure.com/)

* [Technisch Simulationen](https://simulation.azure.com/) 

## <a name="customer-stories"></a>Kunden Erfolgsgeschichten

* [ANEO](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=4168) 

* [d3View](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=22088)

* [Ludwig Institute of Krebs Recherchieren](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=5830)

* [Microsoft Research](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=15634)

* [Milliman](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=14967)

* [Mitsubishi UFJ Sicherheit International](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=26266)

* [Schlumberger](http://azure.microsoft.com/blog/big-compute-for-large-engineering-simulations)

* [Towers Watson](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18222)

* [UberCloud](https://simulation.azure.com/casestudies/Team-182-ABB-UC-Final.pdf)



## <a name="next-steps"></a>Nächste Schritte

* Die neuesten Ankündigungen finden Sie unter [Microsoft HPC und Stapel Teamblog](http://blogs.technet.com/b/windowshpc/) und den [Azure-Blog](https://azure.microsoft.com/blog/tag/hpc/).
* Auch finden Sie unter [Was ist neu in Stapel](https://azure.microsoft.com/updates/?service=batch) oder abonnieren Sie den [RSS-feed](https://azure.microsoft.com/updates/feed/?service=batch).

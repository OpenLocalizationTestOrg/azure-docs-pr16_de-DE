<properties
    pageTitle="Linux und Open Source-Computing auf Azure | Microsoft Azure"
    description="Listen Linux und Open Source-Computing Artikel auf Azure, einschließlich grundlegende Linux Verwendung einige grundlegende Konzepte zum Ausführen oder Linux von Bildern in Azure und andere Inhalte zu bestimmten Technologien und Optimierungen hochladen."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="squillace"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="06/27/2016"
    ms.author="rasquill"/>



# <a name="linux-and-open-source-computing-on-azure"></a>Linux und Open-Source-computing auf Azure

Suchen Sie die Dokumentation, die Sie erstellen und Verwalten von Linux-basierten virtuellen Computern im Bereitstellungsmodell klassischen müssen.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

## <a name="get-started"></a>Erste Schritte
- [Einführung für Linux auf Azure](virtual-machines-linux-intro-on-azure.md)
- [Häufig gestellte Fragen zur Azure erstellte Maschinen mit dem Bereitstellungsmodell klassischen](virtual-machines-linux-classic-faq.md)
- [Informationen zu Bildern für virtuellen Computern](virtual-machines-linux-classic-about-images.md)
- [Distro ein eigenes Bild hochladen](virtual-machines-linux-classic-create-upload-vhd.md) (und auch Anweisungen mithilfe einer [Azure-Endorsed Verteilung](virtual-machines-linux-endorsed-distros.md))
- [Melden Sie sich bei einem Linux virtueller Computer mithilfe des klassischen Azure-Portals](virtual-machines-linux-mac-create-ssh-keys.md)

## <a name="set-up"></a>Einrichten von

- [Installieren von Azure Line Interface (Azure CLI)](../xplat-cli-install.md)


## <a name="tutorials"></a>Lernprogramme

- [Installieren Sie den Stapel LEUCHTE auf einer Linux virtuellen Computern in Azure](virtual-machines-linux-create-lamp-stack.md)
- [Ruby auf Schienen Webanwendung eine Azure-virtuellen Computers](linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md)
- [How to: Installieren Apache Qpid Proton-C für AMQP und Dienstbus](../service-bus-messaging/service-bus-amqp-apache.md)

### <a name="databases"></a>Datenbanken
- [Optimieren der Leistung von MySQL auf Azure](virtual-machines-linux-classic-optimize-mysql.md)
- [MySQL-Cluster](virtual-machines-linux-classic-mysql-cluster.md)
- [Cassandra mit Linux auf Windows Azure ausgeführte und Node.js zugreifen](virtual-machines-linux-classic-cassandra-nodejs.md)
- [Erstellen Sie einen Cluster mit mehreren Mastern von MariaDbs](virtual-machines-linux-classic-mariadb-mysql-cluster.md)

### <a name="hpc"></a>HPC
- [Erste Schritte mit Linux Datenverarbeitungsknoten in einer HPC Pack Cluster in Azure](virtual-machines-linux-classic-hpcpack-cluster.md)
- [Linux Datenverarbeitungsknoten in Azure NAMD mit Microsoft HPC Pack ausgeführt](virtual-machines-linux-classic-hpcpack-cluster-namd.md)
- [Einrichten eines Linux RDMA Clusters MPI Applikationen ausgeführt](virtual-machines-linux-classic-rdma-cluster.md)

### <a name="docker"></a>Docker
- [Mit der Erweiterung Docker virtueller Computer, über die Befehlszeile Azure Schnittstelle (Azure CLI)](virtual-machines-linux-classic-cli-use-docker.md)
- [Verwenden der Docker virtueller Computer Erweiterung vom Azure-portal](virtual-machines-linux-classic-portal-use-docker.md)
- [So Docker-Computer auf Azure verwenden](virtual-machines-linux-docker-machine.md)

### <a name="ubuntu"></a>Ubuntu
- [So: MySQL-Cluster](virtual-machines-linux-classic-mysql-cluster.md)
- [So: Node.js und Cassandra](virtual-machines-linux-classic-cassandra-nodejs.md)

### <a name="opensuse"></a>OpenSUSE
- [How to: Installieren und Ausführen von MySQL](virtual-machines-linux-classic-mysql-on-opensuse.md)

### <a name="coreos"></a>CoreOS
- [How to: Verwenden von CoreOS auf Azure](https://coreos.com/os/docs/latest/booting-on-azure.html)


## <a name="planning"></a>Planung
- [Richtlinien für die Implementierung Azure-Infrastrukturdienste](virtual-machines-linux-infrastructure-subscription-accounts-guidelines.md)
- [Auswählen eines Linux Benutzernamen](virtual-machines-linux-usernames.md)
- [So konfigurieren Sie eine Verfügbarkeit für virtuellen Computern im Bereitstellungsmodell klassischen festlegen](virtual-machines-linux-classic-configure-availability.md)
- [Vorgehensweise beim Planen der geplanten Wartung auf Azure-virtuellen Computern](virtual-machines-linux-planned-maintenance-schedule.md)
- [Verwalten der Verfügbarkeit von virtuellen Computern](virtual-machines-linux-manage-availability.md)
- [Geplante Wartung für in Azure-virtuellen Computern Linux](virtual-machines-linux-planned-maintenance.md)


## <a name="deployment"></a>Bereitstellung
- [Erstellen eines benutzerdefinierten virtuellen Computers mit Linux](virtual-machines-linux-classic-createportal.md)
- [Die Grundlagen: Erfassen eines Linux VM um eine Vorlage zu machen](virtual-machines-linux-classic-capture-image.md)
- [Informationen für die Verteilung nicht unterstützt](virtual-machines-linux-create-upload-generic.md)


## <a name="management"></a>Projektmanagement

- [SSH](virtual-machines-linux-mac-create-ssh-keys.md)
- [Zurücksetzen eines Kennworts oder SSH Eigenschaften für Linux](virtual-machines-linux-classic-reset-access.md)
- [Verwenden die Quadratwurzel](virtual-machines-linux-use-root-privileges.md)


## <a name="azure-resources"></a>Azure-Ressourcen

- [Der Azure Linux-Agent](virtual-machines-linux-agent-user-guide.md)
- [Azure-virtuellen Computer Extensions und Features](virtual-machines-windows-extensions-features.md)
- [Er benutzerdefinierte Daten in einen virtuellen Computer zur Verwendung mit der Initialisierung Cloud einfügt](virtual-machines-windows-classic-inject-custom-data.md)


## <a name="storage"></a>Speicher

- [Einen Datenträger anfügen, um einen Linux virtuellen Computer](virtual-machines-linux-classic-attach-disk.md)
- [Trennen eines Linux virtuellen Computers eines Datenträgers](virtual-machines-linux-classic-detach-disk.md)
- [RAID](virtual-machines-linux-configure-raid.md)


## <a name="networking"></a>Netzwerke
- [Zum Einrichten der Endpunkte auf einem klassischen virtuellen Computer in Azure](virtual-machines-linux-classic-setup-endpoints.md)


## <a name="troubleshooting"></a>Behandlung von Problemen
- [Behandeln von Problemen mit Secure Shell (SSH) Verbindungen mit einer Linux-basierten Azure-virtuellen Computern](virtual-machines-linux-troubleshoot-ssh-connection.md)
- [Behandeln von Problemen mit klassischen Bereitstellungsprobleme bei der Erstellung eines neuen Linux virtuellen Computers in Azure](virtual-machines-linux-classic-troubleshoot-deployment-new-vm.md)  
- [Behandeln von Problemen mit klassischen Bereitstellungsprobleme mit dem Neustarten oder Ändern der Größe einer vorhandenen Linux virtuellen Computern in Azure](virtual-machines-linux-classic-restart-resize-error-troubleshooting.md) 


## <a name="reference"></a>Bezug

- [Azure CLI-Befehle im Modus Azure Service Management (Asm)](../virtual-machines-command-line-tools.md)
- [Azure Servicemanagement REST-API](https://msdn.microsoft.com/library/azure/ee460799.aspx)




## <a name="general-links"></a>Allgemeine Links
Die folgenden Links dienen zur Microsoft-Blogs, Technet-Seiten und externe Websites statt Azure.com Dokumentation wie oben. Wie Azure sowohl in der Welt der öffnen-Quelle Datenverarbeitung schnell Ziele, es ist fast sicher, dass die folgenden Links veraltet, *trotz* der Fakultät sind, dass wir unsere besten ständig neuere Themen hinzufügen und entfernen veraltete diejenigen durchführen soll. Wenn wir eine verpasst haben, wenden Sie sich bitte lassen Sie uns wissen in den Kommentaren oder Anfordern eines Abruf zu unseren [GitHub Repo](https://github.com/Azure/azure-content/).

- [ASP.NET 5 Docker Container mit Linux ausgeführt](http://blogs.msdn.com/b/webdev/archive/2015/01/14/running-asp-net-5-applications-in-linux-containers-with-docker.aspx)
- [Gewusst wie: Bereitstellen einer CentOS virtueller Computer-Bild aus OpenLogic](https://azure.microsoft.com/blog/2013/01/11/deploying-openlogic-centos-images-on-windows-azure-virtual-machines/)
- [SUSE Update Infrastruktur](https://forums.suse.com/showthread.php?5622-New-Update-Infrastructure)
- [SUSE Linux Enterprise Server für SAP-Cloud-Einheit-Bibliothek](https://azure.microsoft.com/marketplace/partners/suse/suselinuxenterpriseserver11sp3forsapcloudappliance/)
- [Erstellen von hoch verfügbaren Linux auf Azure in 12 Schritte](http://blogs.technet.com/b/keithmayer/archive/2014/10/03/quick-start-guide-building-highly-available-linux-servers-in-the-cloud-on-microsoft-azure.aspx)
- [Automatisieren von Provisioning Linux auf Azure mit Azure CLI, node.js, (jhawk)](http://blogs.technet.com/b/keithmayer/archive/2014/11/24/step-by-step-automated-provisioning-for-linux-in-the-cloud-with-microsoft-azure-xplat-cli-json-and-node-js-part-1.aspx)
- [Klicken Sie auf Azure GlusterFS](http://dastouri.azurewebsites.net/gluster-on-azure-part-1/)

### <a name="freebsd"></a>FreeBSD
- [FreeBSD in Azure ausgeführt](https://azure.microsoft.com/blog/2014/05/22/running-freebsd-in-azure/)
- [Einfache FreeBSD bereitstellen](http://msopentech.com/blog/2014/10/24/easy-deploy-freebsd-microsoft-azure-vm-depot/)
- [Bereitstellen einer angepassten FreeBSD Images](http://msopentech.com/blog/2014/05/14/deploy-customize-freebsd-virtual-machine-image-microsoft-azure/)
- [Kaspersky AV für Linux Dateiserver](https://azure.microsoft.com/marketplace/partners/kaspersky-lab/kav-for-lfs-kav-for-lfs/)

### <a name="nosql"></a>NoSQL

- [8 NoSql Open Source-Datenbanken für Azure](http://openness.microsoft.com/blog/2014/11/03/open-source-nosql-databases-microsoft-azure/)
- [Slideshare (MSOpenTech): Erfahrung mit CouchDb auf Azure](http://www.slideshare.net/brianbenz/experiences-using-couchdb-inside-microsofts-azure-team)
- [CouchDB-als-Service mit node.js, CORS und mühselige ausgeführt](http://msopentech.com/blog/2013/12/19/tutorial-building-multi-tier-windows-azure-web-application-use-cloudants-couchdb-service-node-js-cors-grunt-2/)

- [Klicken Sie auf Windows Azure Redis Cache Dienst redis](http://msopentech.com/blog/2014/05/12/redis-on-windows/)
- [Ankündigung von ASP.NET Session State-Anbieter für Redis Preview-Version](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx)

- [Blog: RavenHQ jetzt in der Azure Marketplace verfügbar](https://azure.microsoft.com/blog/2014/08/12/ravenhq-now-available-in-the-azure-store/)

### <a name="big-data"></a>Big Data
- [Installieren von Hadoop auf Linux Azure-virtuellen Computern](http://blogs.msdn.com/b/benjguin/archive/2013/04/05/how-to-install-hadoop-on-windows-azure-linux-virtual-machines.aspx)
- [Azure HDInsight](https://azure.microsoft.com/documentation/learning-paths/hdinsight-self-guided-hadoop-training/)

### <a name="relational-database"></a>Relationale Datenbank
- [MySQL-hoher Verfügbarkeit Architektur in Microsoft Azure](http://download.microsoft.com/download/6/1/C/61C0E37C-F252-4B33-9557-42B90BA3E472/MySQL_HADR_solution_in_Azure.pdf)
- [Installieren von Postgres mit Corosync, Pg_bouncer ILB verwenden](https://github.com/chgeuer/postgres-azure)

### <a name="linux-high-performance-computing-hpc"></a>Linux high Performance computing (HPC)

- [Vorlage Schnellstart: Drehen von einem Cluster SLURM](https://github.com/Azure/azure-quickstart-templates/tree/master/slurm) (und [Blogbeitrag](http://blogs.technet.com/b/windowshpc/archive/2015/06/06/deploy-a-slurm-cluster-on-azure.aspx))
- [Vorlage Schnellstart: erstellen einen HPC Cluster mit Linux berechnen Knoten](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-linux-cn/)

### <a name="devops-management-and-optimization"></a>DevOps, Verwaltung und Optimierung

Als der Welt der Devops Verwaltung und Optimierung wird sehr umfangreichen und sehr schnell ändern, sollten Sie die Liste unter Ausgangspunkt.

- [Video: Azure-virtuellen Computern: Bäckermeister verwenden, Marionette und Docker für die Verwaltung von Linux virtuellen Computern](https://azure.microsoft.com/blog/2014/12/15/azure-virtual-machines-using-chef-puppet-and-docker-for-managing-linux-vms/)

- [Eine vollständige Anleitung zum automatischen Kubernetes Clusterbereitstellung mit CoreOS und Gewebe](https://github.com/GoogleCloudPlatform/kubernetes/blob/master/docs/getting-started-guides/coreos/azure/README.md#kubernetes-on-azure-with-coreos-and-weave)
- [Schnellansicht Kubernetes](https://azure.microsoft.com/blog/2014/08/28/hackathon-with-kubernetes-on-azure/)

- [Jenkins untergeordnete-Plug-In für Azure](http://msopentech.com/blog/2014/09/23/announcing-jenkins-slave-plugin-azure/)
- [GitHub Repo: Jenkins Speicher-Plug-In für Azure](https://github.com/jenkinsci/windows-azure-storage-plugin)

- [Drittanbieter: Hudson untergeordnete-Plug-In für Azure](http://wiki.hudson-ci.org/display/HUDSON/Azure+Slave+Plugin)
- [Drittanbieter: Hudson Speicher-Plug-In für Azure](https://github.com/hudson3-plugins/windows-azure-storage-plugin)

- [Video: Was ist Verwaltungsangestellte und wie funktioniert dies?](https://msopentech.com/blog/2014/03/31/using-chef-to-manage-azure-resources/)

- [Video: Verwendung von Azure Automatisierung mit Linux virtuellen Computern](http://channel9.msdn.com/Shows/Azure-Friday/Azure-Automation-104-managing-Linux-and-creating-Modules-with-Joe-Levy)

- [Blog: Vorgehensweise Powershell DSC für Linux](http://blogs.technet.com/b/privatecloud/archive/2014/05/19/powershell-dsc-for-linux-step-by-step.aspx)
- [GitHub: Docker Client DSC](https://github.com/anweiss/DockerClientDSC)

- [Packer-Plug-In für Azure](https://github.com/msopentech/packer-azure)

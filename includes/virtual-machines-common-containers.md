


Azure bietet hervorragende Cloudlösungen, basierend auf virtuellen Computern&mdash;basierend auf der Emulation physische Computer Hardware&mdash;aktivieren agiles Bewegung der Bereitstellung von Software und erheblich besser Ressource Konsolidierung als physische Hardware. In den letzten Jahren weitgehend Dank der [Docker](https://www.docker.com) Ansatz zum Container und die Docker-Umgebung hat Linux Container Technologie erheblich erweitert die Verfahren zum Entwickeln und Verwalten von verteilten Software. Code der Anwendung in einem Container wird aus den Host Azure-virtuellen Computer sowie andere Container des gleichen virtuellen Computers, die Sie weitere Entwicklung und Bereitstellung Flexibilität Ebene der Anwendung verleiht isoliert&mdash;sowie die Flexibilität, mit denen Sie bereits Azure-virtuellen Computern erhalten.

**Aber das alte Neuigkeiten ist.** Die *neue* Nachricht ist, dass Azure noch mehr Docker Schliff bietet:

- [Viele](../articles/virtual-machines/virtual-machines-linux-docker-machine.md) [andere](../articles/virtual-machines/virtual-machines-linux-dockerextension.md) Methoden zum Erstellen von Docker Hosts für Container entsprechend die jeweiligen situation
- [Azure Container Dienst](https://azure.microsoft.com/documentation/services/container-service/) erstellt Cluster Container Hosts Orchestrators wie **Marathon** und **swarm**verwenden.
- [Ressourcenmanager Azure](../articles/resource-group-overview.md) und [Ressourcen Gruppenvorlagen](../articles/resource-group-authoring-templates.md) , die Bereitstellung und Aktualisierung von komplexen verteilten Applications zu vereinfachen
- Integration in eine Vielzahl von Tools zum Projektmanagement beide unterstützten geschützten und Open-Source-Konfiguration

Und da virtuellen Computern und Linux programmgesteuert erstellen können Container auf Azure, können Sie auch virtueller Computer und Container *Orchestrierung* Tools zum Erstellen von Gruppen von virtuellen Computern (virtuelle Computer) und Bereitstellen von Applications innerhalb Linux Container, und jetzt [Windows Container](https://msdn.microsoft.com/virtualization/windowscontainers/about/about_overview).

Dieser Artikel beschreibt nicht nur diese Konzepte auf hoher Ebene, enthält aber auch t der Links, um weitere Informationen, Lernprogramme und Produkte, die im Zusammenhang mit der Verwendung von Container und Cluster auf Azure. Wenn Sie alle diese Informationen und möchten, dass nur die Hyperlinks, können sie direkt hier bei [Tools für die Arbeit mit Container](#tools-for-working-with-containers)aus.

## <a name="the-difference-between-virtual-machines-and-containers"></a>Der Unterschied zwischen virtuellen Computern und Container

Ausführen von virtuellen Computern innerhalb einer isoliert Hardware Virtualisierung-Umgebung, die von einem [Hypervisor](http://en.wikipedia.org/wiki/Hypervisor)bereitgestellt. In Azure [virtuellen Computern](https://azure.microsoft.com/services/virtual-machines/) Dienst behandelt alle, die für Sie: virtuelle Computer nur erstellen, indem Sie auf das Betriebssystem und konfigurieren, um die Methode ausgeführt werden sollen&mdash;oder durch Ihre eigenen benutzerdefinierten virtuellen Computer Bild hochladen. Virtuellen Computern sind eine bewährte, "statt abgesichert" Technologie und stehen viele Tools zur Verfügung, Betriebssysteme zu verwalten und so konfigurieren Sie die Anwendung, die zu installieren und auszuführen. Nichts in einem virtuellen Computer ausgeführt wird ausgeblendet des Hostbetriebssystems und, aus der Sicht einer Anwendung oder eines eigenständigen physische Computer zu sein scheint Benutzer, der innerhalb eines virtuellen Computers, des virtuellen Computers ausführt.

[Linux Container](http://en.wikipedia.org/wiki/LXC)&mdash;einschließlich der vor erstellt und gehostet Docker Tools verwenden und andere zugrundeliegt&mdash;nicht erforderlich oder verwenden Sie ein Hypervisor Isolation bereitstellen. Stattdessen verwendet der Container Host das System Isolation umfasst Prozess und den Dateinamen des Linux Kernels, um den Container (und deren Anwendung) nur bestimmte Kernelfeatures und ihr eigenes isoliert Dateisystem (mindestens) verfügbar zu machen. Aus der Sicht der Anwendung, die innerhalb eines Containers scheint eine eindeutige Betriebssysteminstanz zu sein der Container. Eine enthaltene Anwendung nicht Prozesse oder eine beliebige andere Ressourcen außerhalb des Containers angezeigt.

Da in diesem Modell Isolation und Ausführung der Kernel des Computers Host Docker freigegeben werden, und die benötigter des Containers jetzt ein vollständiges Betriebssystem nicht enthalten, sind sowohl die Startzeit des des Containers als auch die erforderlichen Datenträger Speicher Verwaltungsaufwand viel kleiner.

Es ist ziemlich beeindruckend.

Windows-Container stellen dieselben Vorteile wie Linux Container für Applikationen, die unter Windows ausgeführt werden. Windows-Container Bildformat Docker und Docker-API unterstützt, aber sie können auch verwaltet werden mithilfe der PowerShell. Zwei Container Laufzeiten sind mit Windows-Container, Windows Server Containern sowie Hyper-V verfügbar. Hyper-V-Container stellen eine zusätzliche Sicherheitsebene Isolation durch jeden Container in einem Super optimiert virtuellen Computer hosten. Informationen finden Sie unter Weitere Informationen zu Windows-Container [Zu Windows Container]( https://msdn.microsoft.com/virtualization/windowscontainers/about/about_overview). Um die Windows-Container in Azure versuchen, finden Sie unter [Schnellstart von Windows Container Azure]( https://msdn.microsoft.com/virtualization/windowscontainers/quick_start/azure_setup).

Das ist auch schon toll.

### <a name="is-this-too-good-to-be-true"></a>Handelt es sich zu gut wahr sein?

Nun, Ja&mdash;und keine. Container, wie jede andere Technologie nicht wie von Zauberhand abwesend alle harte Arbeit, die von verteilten Clientanwendungen erforderlich zurücksetzen. Gleichzeitig noch, führen Sie Container wirklich ändern:

- wie schnell Anwendungscode entwickelt und stark freigegeben werden kann
- Welche KONFIDENZ sie getestet werden kann und wie schnell
- Welche KONFIDENZ und wie schnell es bereitgestellt werden kann

Die unter dem Gesichtspunkt sind, denken Sie daran, Container auf einem Container Host ausführen&mdash;ein Betriebssystem und in Azure, die eine Azure-virtuellen Computern bedeutet. Auch wenn Sie bereits der Vorstellung davon Container schätzen, Sie nun noch eine virtueller Computer Infrastruktur Hostinganbieter den Container benötigen, aber die Vorteile sind, dass Container nicht wichtig ist, welche virtuellen Computers diese vorhanden sind (zwar gibt an, ob der Container ein Linux möchte oder Windows-Umgebung, die Ausführung wichtig, beispielsweise werden).

## <a name="what-are-containers-good-for"></a>Was sind Container für gute?

Sie befinden sich hervorragend für viele Merkmale, aber sie können&mdash;ebenso wie [Azure Cloud Services](https://azure.microsoft.com/services/cloud-services/) und [Azure Service Fabric](../articles/service-fabric/service-fabric-overview.md)&mdash;die Erstellung von Single-Dienst Microservice Orientierung distributed Applications, in welcher Anwendung Entwurf auf Weitere Webparts für kleine, zusammensetzbare statt auf größere, stärker verbundenen Komponenten basiert.

Dies ist insbesondere in öffentlichen Cloud-Umgebungen wie Azure, in dem Sie virtuellen Computern vermieten, wann und an der gewünschten. Nicht nur können Sie nun Isolation und schnelle Bereitstellung und Orchestrierung Tools, sondern umso effizientere Anwendung Infrastruktur Entscheidungen.

Beispielsweise müssen Sie aktuell eine Bereitstellung von 9 Azure virtuellen Computern einer großen Größe für eine Anwendung hoch verfügbare und verteilten aus. Wenn die Komponenten dieser Anwendung in Containern bereitgestellt werden können, können Sie möglicherweise nur 4 virtuellen Computern und Bereitstellen Ihrer Anwendungskomponenten in 20 Containern für Redundanz und Lastenausgleich.

Dies ist nur ein Beispiel, natürlich, wenn Sie dies in Ihrem Szenario tun können, Sie können jedoch auf Verwendung Spitzen mit Weitere Container statt weitere Azure-virtuellen Computern anpassen, und verwenden Sie die verbleibende generelle CPU-Auslastung wesentlich effizienter als zuvor.

Darüber hinaus stehen viele Szenarien, die nicht direkt an einen Microservices Ansatz verleihen; am besten sehen Sie, ob Microservices und Container Ihnen helfen.

### <a name="container-benefits-for-developers"></a>Container Vorteile für Entwickler

Im Allgemeinen ganz einfach Container anzeigen Technologie ist ein Schritt nach vorn, es gibt jedoch einige weitere spezifische Vorteile als auch. Werfen Sie das Beispiel Docker Container aus. In diesem Thema wird nicht die kurz-tief Docker sofort (Lesen [Neuigkeiten Docker?](https://www.docker.com/whatisdocker/) für die Geschichte oder [Wikipedia](http://wikipedia.org/wiki/Docker_%28software%29)), aber Docker und deren Netz anbieten von Vorteile für Entwickler und IT-Experten.

Entwickler nutzen Docker Container schnell, da vor allem die Nutzung von Linux und Windows-Container einfach:

- Sie können einfache, inkrementell Befehle verwenden, um eine feste Bild zu erstellen, die einfach zu ist bereitstellen und können diesen Bildern mithilfe eines Dockerfile erstellen automatisieren
- Sie können diese Bilder problemlos mit einfachen, [Git](https://git-scm.com/)freigeben-Formatvorlage Pushbenachrichtigungen fest, und Ziehen von Befehlen zur [öffentlichen](https://registry.hub.docker.com/) oder [privaten Docker Register](../articles/virtual-machines/virtual-machines-linux-docker-registry-in-blob-storage.md)
- Sie können der isoliert Anwendungskomponenten anstelle von Computern vorstellen.
- Sie können eine große Anzahl von Tools verwenden, die Docker Container und anderen Basis Bilder zu verstehen

### <a name="container-benefits-for-operations-and-it-professionals"></a>Container Vorteile für Vorgänge und IT-Experten

IT und Vorgänge Freiberufler nutzbringend auch die Kombination von Containern und virtuellen Computern.

- enthaltenen Dienste sind von virtuellen Computer Host Ausführung Umgebung isoliert
- enthaltenen Code ist überprüfbar identisch.
- enthaltenen Dienste können gestartet, beendet und schnell zwischen Entwicklung, testen und Herstellung Umgebungen verschoben werden

Features wie diese&mdash;und es werden weitere&mdash;auszeichnen definierte Unternehmen, berufliche Informationen Technologie Organisationen, in dem die Position haben der Ressourcen auszuwählen&mdash;einschließlich reines Verarbeitung Power&mdash;auf die Aufgaben, die nicht nur nicht immer in Business erforderlich, aber Akzeptanz seitens der Kunden und erreicht haben. Starts, ISVs und kleine Unternehmen verfügen über genau die gleichen Anforderung, jedoch können sie es anders beschreiben.

## <a name="what-are-virtual-machines-good-for"></a>Was sind die virtuellen Computern für gute?

Virtuelle Computer bereitzustellen, den Backbone der Cloud computing, und dies nicht geändert. Wenn virtuellen Computern langsamer starten, haben eine größere Datenträger Platzbedarf und nicht direkt an eine Microservices Architektur zugeordnet, verfügen sie sehr wichtige Vorteile:

1. Standardmäßig haben sie viel mehr robuste standardmäßigen Sicherheitsmaßnahmen für Host-computer
2. Alle Bereiche OS und Anwendungskonfigurationen unterstützten
3. Sie haben langjähriger Tool Ökosysteme Befehl und Steuerelement prüfen
4. Sie bieten die Umgebung Host Containern

Das letzte Element ist wichtig, da eine enthaltene Anwendung noch ein bestimmtes Betriebssystem und CPU-Typ erfordert, je nach der Anrufe der Anwendungs machen. Es ist wichtig, denken Sie daran, dass Sie Container auf virtuellen Computern installieren, weil sie die Applikationen enthalten, die Sie bereitstellen möchten. Container sind nicht Ersatz für virtuellen Computern oder Betriebssysteme.

## <a name="high-level-feature-comparison-of-vms-and-containers"></a>Vergleich der virtuellen Computern und Container auf hoher Ebene Features

Die folgende Tabelle beschreibt auf sehr hoher Ebene die Art der Funktion Unterschiede,&mdash;ohne viel zusätzliche Arbeit&mdash;zwischen virtuellen Computern und Linux Container vorhanden sind. Beachten Sie, dass einige Features möglicherweise mehr oder weniger wünschenswert abhängig von Ihrer eigenen Anwendung muss, wie bei allen Software, zusätzliche Arbeit bietet erhöht die Unterstützung von Features, insbesondere nicht im Bereich der Sicherheit.

|   Feature      | Virtuellen Computern | Container  |
| :------------- |-------------| ----------- |
| Unterstützung der Sicherheit "Standard" | Um einen höheren Grad | in einem etwas weniger Grad |
| Speicher auf dem Datenträger erforderlich | Vollständige OS plus -apps | Nur die App-Anforderungen |
| Zeit zum Starten | Wesentlich länger: Boot des OS sowie app laden | Im Wesentlichen verkürzen: nur apps müssen gestartet werden, weil Kernel bereits ausgeführt wird  |
| Portabilität | Portable mit der richtigen Vorbereitung | Portable in Bildformat; in der Regel kleiner |
| Abbildung Automatisierung | Variiert je nach Betriebssystem und apps | [Docker Registrierung](https://registry.hub.docker.com/); andere Personen

## <a name="creating-and-managing-groups-of-vms-and-containers"></a>Erstellen und Verwalten von Gruppen von virtuellen Computern und Container

Zu diesem Zeitpunkt entwerfen eine, Entwickler oder IT-Vorgänge, die möglicherweise Specialist denken, die "Ich kann diesen; automatisieren Diese wirklich ist Daten-Center-As-A-Service! ".

Sie haben Recht, kann sein, und es gibt eine Reihe von Systeme, von denen viele Sie bereits verwenden können, die Gruppen von Azure-virtuellen Computern verwalten und Einfügen von benutzerdefiniertem Code mithilfe von Skripts, häufig mit den [CustomScriptingExtension für Windows](https://msdn.microsoft.com/library/azure/dn781373.aspx) oder die [CustomScriptingExtension für Linux](https://azure.microsoft.com/blog/2014/08/20/automate-linux-vm-customization-tasks-using-customscript-extension/)können. Sie können&mdash;und vielleicht bereits&mdash;automatisierte Ihrer Azure-Bereitstellungen mit PowerShell oder Azure CLI Skripts.

Diese Funktionen werden häufig dann nach Tools, wie [Marionette](https://puppetlabs.com/) und [Verwaltungsangestellte](https://www.chef.io/) , um die Erstellung der automatisieren und Konfiguration für virtuelle Computer bei migriert werden. (Hier sind einige Links zur [Verwendung dieser Tools mit Azure](#tools-for-working-with-containers).)

### <a name="azure-resource-group-templates"></a>Azure Ressource Gruppenvorlagen

Weitere vor kurzem Azure freigegeben die [Azure ressourcenverwaltung](../articles/virtual-machines/virtual-machines-windows-compare-deployment-models.md) REST-API und PowerShell und Azure CLI Tools, um einfach zu verwendenden aktualisiert. Bereitstellen können, ändern oder erneut das gesamte Anwendungstopologien [Azure Ressourcenmanager Vorlagen](../articles/resource-group-authoring-templates.md) verwenden, mit der Ressource Azure Management-API mit bereitstellen:

- das [Verwenden von Vorlagen Azure-Portal](https://github.com/Azure/azure-quickstart-templates)&mdash;Hinweis, verwenden Sie die Schaltfläche "DeployToAzure"
- die [Azure CLI](../articles/virtual-machines/virtual-machines-linux-cli-deploy-templates.md)
- die [Azure PowerShell-Module](../articles/virtual-machines/virtual-machines-linux-cli-deploy-templates.md)

### <a name="deployment-and-management-of-entire-groups-of-azure-vms-and-containers"></a>Bereitstellung und Verwaltung der gesamten Gruppen von Azure-virtuellen Computern und Container

Es gibt mehrere beliebte Systeme, die gesamte Gruppen von virtuellen Computern bereitstellen und Installieren von Docker (oder andere Linux Container Hostsysteme) können diesen als automatisierbare Gruppe ein. Direkte Links finden Sie unter im Abschnitt [Container und Tools](#containers-and-vm-technologies) . Es gibt mehrere Systeme, die was eine höhere oder geringere Handlungen dazu, und diese Liste ist nicht vollständig. Je nach Ihren Kompetenzen und Szenarien wird der Fall ist oder nicht hilfreich sein.

Docker verfügt über eine eigene Gruppe von-Erstellung virtueller Computer-Tools ([Docker-Computer](../articles/virtual-machines/virtual-machines-linux-docker-machine.md)) und eine Lastenausgleich, Docker-Container Cluster-Verwaltungstool ([swarm](../articles/virtual-machines/virtual-machines-linux-docker-swarm.md)). Darüber hinaus bietet die [Azure Docker virtueller Computer Erweiterung](https://github.com/Azure/azure-docker-extension/blob/master/README.md) Standard-Unterstützung für [`docker-compose`](https://docs.docker.com/compose/), welche bereitstellen können Anwendungscontainer über mehrere Container konfiguriert.

Darüber hinaus können Sie [Den Mesosphere Data Center Betriebssystem (DCOS)](http://docs.mesosphere.com/install/azurecluster/)ausprobieren. DCOS basiert auf dem Open Source- [Mesos](http://mesos.apache.org/) "verteilte Systeme Kernel", der mit dem Datencenters als eine adressiert Service behandelt ermöglicht. DCOS weist integrierte Pakete für mehrere wichtige Systeme, wie z. B. [Spark](http://spark.apache.org/) und [Kafka](http://kafka.apache.org/) (und andere) sowie integrierten Diensten wie [Marathon](https://mesosphere.github.io/marathon/) (eines Containers Steuerelement System) und [Chronos](https://mesos.github.io/chronos/) (verteilte Scheduler). Mesos wurde Erfahrungsbericht bei Twitter, AirBnb und anderen Unternehmen Web-Skala abgeleitet wird. Sie können auch als Orchestrierungsmodul **swarm** verwenden.

Darüber hinaus ist [Kubernetes](https://azure.microsoft.com/blog/2014/08/28/hackathon-with-kubernetes-on-azure/) eine Open Source-System für virtueller Computer und Container die Verwaltung von Erfahrungsbericht bei Google abgeleitet. Sie können sogar [Kubernetes mit Ihrer Hilfe, um Unterstützung für Netzwerke bereitzustellen](https://github.com/GoogleCloudPlatform/kubernetes/blob/master/docs/getting-started-guides/coreos/azure/README.md#kubernetes-on-azure-with-coreos-and-weave)verwenden.

[Deis](http://deis.io/overview/) ist eine offene Quelle "Platform-as-a-Service" (PaaS), die zum Bereitstellen und Verwalten von Applications auf Ihren eigenen Servern erleichtert. Deis baut auf Docker und CoreOS, um eine einfache PaaS mit einem Workflow Heroku-Design zu ermöglichen. Sie können ganz einfach [einen 3-Knoten Azure-virtuellen Computer Gruppe erstellen, und installieren Deis](../articles/virtual-machines/virtual-machines-linux-deis-cluster.md) Azure und dann [Installieren einer Anwendung Hallo Welt-wechseln](../articles/virtual-machines/virtual-machines-linux-deis-cluster.md#deploy-and-scale-a-hello-world-application).

[CoreOS](https://coreos.com/os/docs/latest/booting-on-azure.html), eine Linux-Verteilung mit einer optimierten Platzbedarf, Docker Support und eigene Container mit dem Namen [Rkt](https://github.com/coreos/rkt), weist ebenfalls ein Container Gruppe Verwaltungstool [Flotte](https://coreos.com/using-coreos/clustering/)bezeichnet.

Ubuntu, ein anderes sehr beliebte Linux Verteilung Docker sehr gut unterstützt, aber auch unterstützt [Cluster mit Linux (LXC-Format)](https://help.ubuntu.com/lts/serverguide/lxc.html).

## <a name="tools-for-working-with-azure-vms-and-containers"></a>Tools für die Arbeit mit Azure-virtuellen Computern und Container

Arbeiten mit Container und Azure-virtuellen Computern verwendet Tools. Dieser Abschnitt enthält eine Liste der nur einige der am häufigsten nützlich oder wichtige Konzepte und Tools zum Container, Gruppen und die größere Konfiguration und mit ihnen verwendeten Orchestrierung-Tools.

> [AZURE.NOTE] In diesem Bereich ist erstaunlich schnell ändern möchten, und zwar wir unsere besten wird in diesem Thema und ihre Links auf dem neuesten Stand zu bleiben, könnte es unmöglich nicht. Stellen Sie sicher, dass Sie interessanten Themen zu auf dem neuesten Stand Suchen!

### <a name="containers-and-vm-technologies"></a>Container und virtueller Computer Technologien

Einige Linux Container Technologies:

- [Docker](https://www.docker.com)
- [LXC](https://linuxcontainers.org/)
- [CoreOS und rkt](https://github.com/coreos/rkt)
- [Container geöffneten Projekt](http://opencontainers.org/)
- [RancherOS](http://rancher.com/rancher-os/)

Windows-Container Links:

- [Windows-Container](https://msdn.microsoft.com/virtualization/windowscontainers/about/about_overview)

Visual Studio Docker Links:

- [Visual Studio 2015 RC Tools für Docker - Vorschau](https://visualstudiogallery.msdn.microsoft.com/6f638067-027d-4817-bcc7-aa94163338f0)

Docker Tools:

- [Docker daemon](https://docs.docker.com/installation/#installation)
- Docker-clients
    - [Windows-Docker-Client auf Chocolatey](https://chocolatey.org/packages/docker)
    - [Informationen zur Installation von docker](https://docs.docker.com/installation/#installation)


Docker auf Microsoft Azure:

- [Docker virtueller Computer-Erweiterung für Linux auf Azure](../articles/virtual-machines/virtual-machines-linux-dockerextension.md)
- [Azure Docker virtueller Computer Erweiterung Benutzerhandbuch](https://github.com/Azure/azure-docker-extension/blob/master/README.md)
- [Mit der Erweiterung Docker virtueller Computer, über die Befehlszeile Azure Schnittstelle (Azure CLI)](../articles/virtual-machines/virtual-machines-linux-classic-cli-use-docker.md)
- [Verwenden der Docker virtueller Computer Erweiterung vom Azure-portal](../articles/virtual-machines/virtual-machines-linux-classic-portal-use-docker.md)
- [So Docker-Computer auf Azure verwenden](../articles/virtual-machines/virtual-machines-linux-docker-machine.md)
- [Verwenden von Docker mit Punktschwarms auf Azure](../articles/virtual-machines/virtual-machines-linux-docker-swarm.md)
- [Erste Schritte mit Docker und verfassen auf Azure](../articles/virtual-machines/virtual-machines-linux-docker-compose-quickstart.md)
- [Verwenden einer Ressource Azure Gruppenvorlage So erstellen Sie schnell einen Docker-Host auf Azure](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu)
- [Des integrierten Support für `compose` ](https://github.com/Azure/azure-docker-extension#11-public-configuration-keys) für Applikationen enthaltenen
- [Implementieren eines privaten Docker-Registrierung auf Azure](../articles/virtual-machines/virtual-machines-linux-docker-registry-in-blob-storage.md)

Linux-Versionen und Azure Beispielen:

- [CoreOS](https://coreos.com/os/docs/latest/booting-on-azure.html)

Konfiguration, Verwaltung und Orchestrierung Container:

- [Klicken Sie auf CoreOS Flotte](https://coreos.com/using-coreos/clustering/)

-   Deis
    - [Erstellen Sie einen 3-Knoten Gruppe Azure-virtuellen Computer, installieren Deis, und starten Sie eine Anwendung Hallo Welt-wechseln](../articles/virtual-machines/virtual-machines-linux-deis-cluster.md)

-   Kubernetes
    - [Eine vollständige Anleitung zum automatischen Kubernetes Clusterbereitstellung mit CoreOS und Gewebe](https://github.com/GoogleCloudPlatform/kubernetes/blob/master/docs/getting-started-guides/coreos/azure/README.md#kubernetes-on-azure-with-coreos-and-weave)
    - [Schnellansicht Kubernetes](https://azure.microsoft.com/blog/2014/08/28/hackathon-with-kubernetes-on-azure/)

-   [Mesos](http://mesos.apache.org/)
    -   [Die Mesosphere Daten zu zentrieren Betriebssystem (DCOS)](http://beta-docs.mesosphere.com/install/azurecluster/)

-   [Jenkins](https://jenkins-ci.org/) und [Hudson](http://hudson-ci.org/)
    - [Blog: Jenkins untergeordnete-Plug-In für Azure](http://msopentech.com/blog/2014/09/23/announcing-jenkins-slave-plugin-azure/)
    - [GitHub Repo: Jenkins Speicher-Plug-In für Azure](https://github.com/jenkinsci/windows-azure-storage-plugin)
    - [Drittanbieter: Hudson untergeordnete-Plug-In für Azure](http://wiki.hudson-ci.org/display/HUDSON/Azure+Slave+Plugin)
    - [Drittanbieter: Hudson Speicher-Plug-In für Azure](https://github.com/hudson3-plugins/windows-azure-storage-plugin)

-   [Azure Automatisierung](https://azure.microsoft.com/services/automation/)
    - [Video: Verwendung von Azure Automatisierung mit Linux virtuellen Computern](http://channel9.msdn.com/Shows/Azure-Friday/Azure-Automation-104-managing-Linux-and-creating-Modules-with-Joe-Levy)

-   PowerShell DSC für Linux
    - [Blog: Vorgehensweise Powershell DSC für Linux](http://blogs.technet.com/b/privatecloud/archive/2014/05/19/powershell-dsc-for-linux-step-by-step.aspx)
    - [GitHub: Docker Client DSC](https://github.com/anweiss/DockerClientDSC)

## <a name="next-steps"></a>Nächste Schritte

Schauen Sie sich [Docker](https://www.docker.com) und [Windows-Container](https://msdn.microsoft.com/virtualization/windowscontainers/about/about_overview).

<!--Anchors-->
[microservices]: http://martinfowler.com/articles/microservices.html
[microservice]: http://martinfowler.com/articles/microservices.html
<!--Image references-->

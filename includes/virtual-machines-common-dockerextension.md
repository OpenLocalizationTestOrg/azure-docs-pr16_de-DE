

[Docker](https://www.docker.com/) ist eine der am häufigsten verwendeten Virtualisierung Vorgehensweisen, die als eine Möglichkeit zum Isolieren Anwendungsdaten, und klicken Sie auf freigegebene Ressourcen computing [Linux Container](http://en.wikipedia.org/wiki/LXC) anstelle von virtuellen Computern verwendet. Die [Erweiterung Azure Docker virtueller Computer](https://github.com/Azure/azure-docker-extension/blob/master/README.md) an den [Azure Linux Agent](../articles/virtual-machines/virtual-machines-linux-agent-user-guide.md) können eines Docker virtuellen Computers zu erstellen, die eine beliebige Anzahl von Containern für die Anwendung auf Azure hostet.

In diesem Thema werden:

+ [Docker und Linux Container]
+ [So verwenden Sie die Erweiterung Docker virtueller Computer mit Azure]
+ [Virtuellen Computern Erweiterungen für Linux und Windows]

Docker-fähige virtuelle Computer sofort erstellen zu können, finden Sie unter:

+ [So verwenden Sie die Docker virtueller Computer Erweiterung aus dem Azure Line Interface (CLI Azure)]
+ [So verwenden Sie die Erweiterung Docker virtueller Computer mit dem klassischen Azure-portal]
+ [Wie Sie schnell mit Docker in Azure Marketplace]

Weitere Informationen über die Erweiterung und deren Funktionsweise finden Sie unter des [Docker Erweiterung Benutzerhandbuch](https://github.com/Azure/azure-docker-extension/blob/master/README.md).

## <a name="docker-and-linux-containers"></a>Docker und Linux Container
[Docker](https://www.docker.com/) ist eine der am häufigsten verwendeten Virtualisierung Vorgehensweisen, die [Linux Container](http://en.wikipedia.org/wiki/LXC) anstelle von virtuellen Computern als eine Möglichkeit zum Isolieren von Daten, und klicken Sie auf freigegebene Ressourcen computing verwendet und andere Dienste, mit denen Sie das Erstellen oder Zusammenstellen von Applications schnell und Verteilen von zwischen andere Docker Container enthält.

Docker und Linux Container sind nicht [Hypervisoren](http://en.wikipedia.org/wiki/Hypervisor) wie Windows Hyper-V und [KVM](http://www.linux-kvm.org/page/Main_Page) auf Linux (es gibt viele andere Beispiele). Hypervisoren Virtualisierung das zugrunde liegenden Betriebssystem um abgeschlossen Betriebssysteme ( *virtuellen Computern*genannt), um innerhalb der Hypervisor auszuführen, als wären sie eine Anwendung zu aktivieren.

Docker und andere *Container* Ansätze geringere grundlegend haben, die verbraucht die Startzeit und Verarbeitung und Speicher mithilfe des Prozesses Verwaltungsaufwand und Isolation Leistungsmerkmale des Dateisystems des Linux Kernels verfügbar machen nur Kernelfeatures, die Sie einer ansonsten Container isoliert.

Die folgende Tabelle beschreibt die Arten von jeweiligen featureunterschiede zwischen Hypervisoren und Linux Container auf sehr hoher Ebene aus. Beachten Sie, dass einige features möglicherweise mehr oder weniger wünschenswert je nach Bedarf der eigenen Anwendung.

|   Feature      | Hypervisoren | Container  |
| :------------- |-------------| ----------- |
| Prozessisolation | Mehr oder weniger erledigt | Wenn Stamm ermittelt wird, kann Container Host gefährdet werden |
| Speicher auf dem Datenträger erforderlich | Vollständige OS plus -apps | Nur die App-Anforderungen |
| Zeit zum Starten | Wesentlich länger: Boot des OS sowie app laden | Erheblich kürzere: nur apps müssen gestartet werden, weil Kernel bereits ausgeführt wird  |
| Container Automatisierung | Variiert je nach Betriebssystem und apps | [Bildergalerie Docker](https://registry.hub.docker.com/); andere Personen

Eine allgemeine Erläuterung von Containern und deren Vorteile finden Sie unter dem [Docker hoher Ebene Whiteboard](http://channel9.msdn.com/Blogs/Regular-IT-Guy/Docker-High-Level-Whiteboard).

Weitere Informationen zu was Docker ist und wie sie wirklich funktioniert, finden Sie unter [Neuigkeiten Docker?](https://www.docker.com/whatisdocker/)

#### <a name="docker-and-linux-container-security-best-practices"></a>Bewährte Methoden für Docker und Linux Container Sicherheit

Da Container Zugriff auf dem Host-Computer Kernel, freigeben, ist bösartiger Code Stamm erlangen können sie auch nicht nur auf dem Host-Computer, sondern auch die anderen Container zugreifen aussehen. Sichern von Ihrem System Container stärker als Standardkonfiguration, [Docker empfiehlt](https://docs.docker.com/articles/security/) mit Erweiterung Gruppenrichtlinien oder [rollenbasierte Sicherheit](http://en.wikipedia.org/wiki/Role-based_access_control) sowie, z. B. [SELinux](http://selinuxproject.org/page/Main_Page) oder [AppArmor](http://wiki.apparmor.net/index.php/Main_Page), z. B. sowie verringern so weit wie möglich die Kernel-Funktionen, die den Container gewährt werden. Darüber hinaus stehen viele andere Dokumente im Internet, in denen Vorgehensweisen zur Sicherheit in Containern wie Docker beschrieben.

## <a name="how-to-use-the-docker-vm-extension-with-azure"></a>So verwenden Sie die Erweiterung Docker virtueller Computer mit Azure

Die Erweiterung Docker virtueller Computer ist eine Komponente, die in die Instanz virtueller Computer, die Sie erstellen installiert ist, die wiederum die Docker-Engine installiert und verwaltet werden remote-Kommunikation mit dem virtuellen Computer an. Es gibt zwei Möglichkeiten, um die Erweiterung virtueller Computer installieren: Sie können Ihre virtuellen Computer mit dem Verwaltungsportal erstellen oder Sie können sie aus der Azure Line Interface (CLI Azure) erstellen.

Sie können im Portal verwenden, um die Erweiterung Docker virtuellen Computer alle kompatiblen Linux VM hinzufügen (derzeit das nur Bild, das sie unterstützt das Ubuntu 14.04 LTS Bild aktueller als Juli ist). Verwenden die Befehlszeile Azure CLI, jedoch können die Erweiterung Docker virtuellen Computer installieren und erstellen und Ihre Docker Kommunikation Zertifikate alle zur gleichen Zeit bei der Erstellung die Instanz virtueller Computer hochladen.

Docker-fähige virtuelle Computer sofort erstellen zu können, finden Sie unter:

+ [So verwenden Sie die Docker virtueller Computer Erweiterung aus dem Azure Line Interface (CLI Azure)]
+ [So verwenden Sie die Erweiterung Docker virtueller Computer mit dem klassischen Azure-portal]

## <a name="virtual-machine-extensions-for-linux-and-windows"></a>Virtuellen Computern Erweiterungen für Linux und Windows
Die [Docker virtueller Computer Erweiterung für Azure](https://github.com/Azure/azure-docker-extension/blob/master/README.md) ist nur eine von mehreren virtuellen Computer-Erweiterungen, die spezielle Verhalten bereitstellen, und weitere werden in der Entwicklung. Angenommen, können mehrere [Linux virtueller Computer Agent-Erweiterung](../articles/virtual-machines/virtual-machines-linux-agent-user-guide.md) Features Sie ändern und Verwalten der virtuellen Computern, einschließlich Sicherheitsfeatures, Kernel und Netzwerke Features und So weiter. Die Erweiterung VMAccess kann beispielsweise die Administratorkennwort oder SSH Schlüssel zurücksetzen.

Eine vollständige Liste finden Sie unter [Azure-virtuellen Computer Extensions](../articles/virtual-machines/virtual-machines-windows-extensions-features.md).

<!--Anchors-->
[So verwenden Sie die Docker virtueller Computer Erweiterung aus dem Azure Line Interface (CLI Azure)]: http://azure.microsoft.com/documentation/articles/virtual-machines-docker-with-xplat-cli/
[So verwenden Sie die Erweiterung Docker virtueller Computer mit dem klassischen Azure-portal]: http://azure.microsoft.com/documentation/articles/virtual-machines-docker-with-portal/
[Wie Sie schnell mit Docker in Azure Marketplace]: http://azure.microsoft.com/documentation/articles/virtual-machines-docker-ubuntu-quickstart/
[Docker und Linux Container]: #Docker-and-Linux-Containers
[So verwenden Sie die Erweiterung Docker virtueller Computer mit Azure]: #How-to-use-the-Docker-VM-Extension-with-Azure
[Virtuellen Computern Erweiterungen für Linux und Windows]: #Virtual-Machine-Extensions-For-Linux-and-Windows

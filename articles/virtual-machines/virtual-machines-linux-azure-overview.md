 <properties
   pageTitle="Azure und Linux | Microsoft Azure"
   description="Beschreibt Azure zu berechnen, Speicher und Netzwerke Services mit Linux virtuellen Computern an."
   services="virtual-machines-linux"
   documentationCenter="virtual-machines-linux"
   authors="vlivech"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="09/14/2016"
   ms.author="v-livech"/>

# <a name="azure-and-linux"></a>Azure und Linux

Microsoft Azure ist eine wachsende Sammlung von integrierten öffentlichen cloud-Dienste einschließlich Analytics, virtuellen Computern, Datenbanken, mobile, Netzwerk, Speicher, und im web – ideal für Ihre Lösungen hosten.  Microsoft Azure stellt eine skalierbare computing Plattform, mit dem Sie Zahlen nur für was Sie verwenden, wenn er - soll ohne zu investieren in lokalen Hardware.  Wenn Sie Ihre Lösungen skalieren sind und sich mit dem Maßstab versetzt Sie benötigen, um den Bedürfnissen der Kunden-service, ist Azure bereit.

Wenn Sie mit den verschiedenen Features von der Amazon-vertraut sind AWS, können Sie das Azure im Vergleich mit einer AWS [Definition Zuordnungsdokument](https://azure.microsoft.com/campaigns/azure-vs-aws/mapping/)prüfen.


## <a name="regions"></a>Regionen
Microsoft Azure-Ressourcen werden auf mehreren Ländern / Regionen auf der ganzen Welt verteilt.  Eine "Region" steht für mehrere Centers von Daten in einer einzelnen geographischen.  Zum Zeitpunkt 1 Januar 2016: Dies umfasst: 8 in Nordamerika, 2 in Europa, 6 im asiatisch-pazifischen Raum, in Festland China 2 und 3 in Indien.  Wenn Sie eine vollständige Liste aller Azure Bereiche möchten, verwalten wir, dass eine Liste der vorhandenen und neu Regionen angekündigt.

- [Azure Regionen](https://azure.microsoft.com/regions/)

## <a name="availability"></a>Verfügbarkeit
Sie müssen zwei bereitstellen Reihenfolge für die Bereitstellung für unsere 99.95 virtueller Computer Ebene Servicevertrag qualifizieren oder legen Sie weitere virtuellen Computern Ihrer Arbeitsbelastung innerhalb einer Verfügbarkeit ausgeführt. Dadurch wird sichergestellt, Ihre virtuellen Computern sind über mehrere Fehlerstrukturanalyse Domänen in unsere Data Center verteilt als auch auf Hosts mit anderen Wartung Windows bereitgestellt. Die vollständige [Azure Vereinbarung zum SERVICELEVEL](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_0/) wird erläutert, die garantierte Verfügbarkeit von Azure als Ganzes.

## <a name="azure-virtual-machines--instances"></a>Azure-virtuellen Computern und Instanzen
Microsoft Azure unterstützt Ausführen zahlreicher beliebte Linux-Versionen zur Verfügung gestellt und verwaltet anhand einer Anzahl von Partner zur Verfügung.  Finden Sie Versionen, wie Red Hat Enterprise, CentOS, Debian, Ubuntu, CoreOS, RancherOS, FreeBSD und weiteren Funktionen in der Azure Marketplace. Wir arbeiten aktiv mit verschiedenen Linux Communitys noch mehr Arten zur [Azure Linux Distros unterstützt](virtual-machines-linux-endorsed-distros.md) Liste hinzufügen.

Wenn Ihre bevorzugten Linux Distro Wahl nicht aktuell im Katalog vorhanden ist, können Sie "Übertragen Ihrer eigenen Linux" virtuellen Computer durch das [Erstellen und Hochladen einer virtuellen Linux in Azure](virtual-machines-linux-create-upload-generic.md).

Azure-virtuellen Computern können Sie eine Vielzahl von Lösungen auf eine Weise agiles computing bereitstellen. Sie können nahezu jeder Arbeitsbelastung und eine beliebige Sprache auf nahezu jedem Betriebssystem - Windows, Linux, oder Bereitstellen ein benutzerdefinierten erstellt eine aus einem der unsere wachsende Anzahl von Partner. Immer noch nicht finden Sie unter Ihnen gesuchte?  Keine Sorge: Sie können auch eigene Bilder aus lokalen bringen.

## <a name="vm-sizes"></a>Virtueller Computer Größen
Wenn Sie einen virtuellen Computer Azure bereitstellen, überschreiten Sie wählen Sie eine Größe virtueller Computer in eine Reihe von Größen, die für Ihre Arbeitsbelastung geeignet ist. Die Größe gilt auch für die Verarbeitung von Power, Speicher, und Speicherkapazität des virtuellen Computers. Sie sind Abrechnung, basierend auf den Zeitraum der virtuellen Computer ausgeführt wird und Verarbeitung zugewiesenen Ressourcen. Eine vollständige Liste der [Größe von virtuellen Computern](virtual-machines-linux-sizes.md).

Hier sind einige grundlegenden Richtlinien für die Auswahl eines virtueller Speicher aus einem unserer Reihe (A, D, DS, G und GS).

* A-Serie virtuellen Computern sind unsere Einstiegsklasse virtuellen Computern für light Auslastung und Test-/Szenarien Preis Wert an. Sind können sie weit verfügbar in allen Regionen und eine Verbindung herstellen und alle standard-Ressourcen virtuellen Computern zur Verfügung.
* A-Serie Größen (A8 - A11) sind spezielle berechnen stark Konfigurationen für High Performance computing Cluster Applikationen geeignet.
* D-Serie virtuellen Computern sollen Programme ausführen, die höhere berechnen Power und Leistung temporärer Speicherplatz anfordern. D-Serie virtuellen Computern bieten schnellere Prozessoren, ein höheres Arbeitsspeicher-to-Core Verhältnis sowie eine State Drive (SSD) für den temporären Datenträger.
* Dv2-Serie, ist die neueste Version von unserem D-Serie, eine leistungsfähigere CPU features. Die CPU Dv2-Serie ist etwa 35 % schneller als die CPU D-Serie. Er basiert auf der neuesten Generation 2,4 GHz Intel Xeon® E5-2673 v3 (Haskell)-Prozessor und Intel Turbo steigern Technologie 2.0, bis zu 3,2 GHz wechseln können. Der Dv2-Serie weist dieselben Arbeitsspeicher und Konfigurationen als der D-Serie.
* G-Serie virtuellen Computern bieten den meisten Arbeitsspeicher und Hosts, auf denen Intel Xeon E5 V3 familiäre Prozessoren ausgeführt.

Hinweis: DS-Serie und GS-Serie virtuellen Computern haben Zugriff auf Speicher Premium – unsere SSD gesicherten leistungsfähige und niedrig Wartezeiten Speicher für e/a-stark Auslastung. Premium-Speicher steht in bestimmte Regionen zur Verfügung. Weitere Informationen finden Sie unter:

- [Premium Speicher: Leistungsstarke Storage für Auslastung Azure-virtuellen Computern](../storage/storage-premium-storage.md)

## <a name="automation"></a>Automatisierung
Um eine ordnungsgemäße DevOps Kultur zu erreichen, muss die gesamte Infrastruktur Code aus.  Wenn sämtliche die Infrastruktur in Code befindet, die sie einfach werden kann (Karlsruhe Servers) neu erstellt.  Azure funktioniert mit alle Bereiche Automatisierung wie Ansible, Verwaltungsangestellte, SaltStack und Marionette Tools.  Azure weist auch eine eigene Tools für die Automatisierung:

- [Azure-Vorlagen](virtual-machines-linux-create-ssh-secured-vm-from-template.md)

- [Azure VMAccess](virtual-machines-linux-using-vmaccess-extension.md)

Azure ist einführen Unterstützung für [Cloud der Initialisierung](http://cloud-init.io/) über die meisten Linux Distros, die es unterstützen.  Aktuell werden die kanonisch Ubuntu virtuellen Computern mit Cloud der Initialisierung standardmäßig aktiviert bereitgestellt.  RedHats RHEL, CentOS und Fedora unterstützen Cloud der Initialisierung, jedoch die Azure Bilder von Red hat geführt haben keine Cloud der Initialisierung installiert.  Wenn der Initialisierung Cloud auf Red hat Familien OS verwenden möchten, müssen Sie ein benutzerdefiniertes Bild mit der Cloud der Initialisierung installiert erstellen.

- [Verwenden der Initialisierung Cloud auf Azure Linux virtuellen Computern](virtual-machines-linux-using-cloud-init.md)

## <a name="quotas"></a>Kontingente
Jede Azure-Abonnement hat Standard Kontingent Grenzwerte in Ort, an dem die Bereitstellung von einer großen Anzahl von virtuellen Computern für Ihr Projekt beeinträchtigen kann. Die aktuelle Begrenzung auf Basis pro Abonnement ist 20 virtuellen Computern pro Region.  Speicherkontingent Grenzwerte können durch eine Support-Ticket anfordern eine höhere Grenzen Ablage ausgelöst werden.  Detaillierte Informationen zur Kontingent Grenzwerte:

- [Grenzwerte für Azure-Abonnement-Dienst](../azure-subscription-service-limits.md)


## <a name="partners"></a>Partner

Microsoft arbeitet eng mit unseren Partnern, um sicherzustellen, dass die Bilder verfügbar sind aktualisiert und optimiert für eine Azure-Laufzeit.  Weitere Informationen zu unseren Partnern aktivieren Sie deren nachfolgenden Marketplace-Seiten.

- [Linux auf Verteilung Azure unterstützt](virtual-machines-linux-endorsed-distros.md)

Red hat - [Azure Marketplace - Red Hat Enterprise Linux 7.2](https://azure.microsoft.com/marketplace/partners/redhat/redhatenterpriselinux72/)

Kanonische - [Azure Marketplace - Ubuntu Server 16.04 LTS](https://azure.microsoft.com/marketplace/partners/canonical/ubuntuserver1604lts/)

Debian - [Azure Marketplace - Debian 8 "Jessie"](https://azure.microsoft.com/marketplace/partners/credativ/debian8/)

FreeBSD - [Azure Marketplace - FreeBSD 10.3](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd103/)

CoreOS - [Azure Marketplace - CoreOS (Stabilität)](https://azure.microsoft.com/marketplace/partners/coreos/coreosstable/)

RancherOS - [Azure Marketplace - RancherOS](https://azure.microsoft.com/marketplace/partners/rancher/rancheros/)

Bitnami - [Bitnami Bibliothek für Azure](https://azure.bitnami.com/)

Mesosphere - [Azure Marketplace - Mesosphere DC/OS auf Azure](https://azure.microsoft.com/marketplace/partners/mesosphere/dcosdcos/)

Docker - [Azure Marketplace - Container Azure Service mit Docker Punktschwarms](https://azure.microsoft.com/marketplace/partners/microsoft/acsswarms/)

Jenkins - [Azure Marketplace - CloudBees Jenkins Plattform](https://azure.microsoft.com/marketplace/partners/cloudbees/jenkins-platformjenkins-platform/)


## <a name="getting-setup-on-azure"></a>Erste Setup auf Azure
Beginnen Sie mit Azure benötigten ein Azure-Konto, die Azure CLI installiert und ein Paar von SSH öffentlichen und privaten Schlüsseln.

## <a name="sign-up-for-an-account"></a>Melden Sie für ein Konto an
Dieser erste Schritt in der Cloud Azure verwenden, ist für ein Azure-Konto anmelden.  Wechseln Sie zu der Seite [Azure-Konto einrichten](https://azure.microsoft.com/pricing/free-trial/) , um anzufangen.

## <a name="install-the-cli"></a>Installieren der CLI
Mit Ihrem neuen Azure-Konto können Sie loslegen sofort mit der Azure-Portal, also ein Administrator webbasierten Systemsteuerung.  Zum Verwalten der Azure Cloud über die Befehlszeile installieren Sie die `azure-cli`.  Installieren Sie die [Azure CLI ](../xplat-cli-install.md)auf Ihrem Computer Mac oder Linux.

## <a name="create-an-ssh-key-pair"></a>Ein paar SSH erstellen
Jetzt haben Sie ein Azure-Konto, das Azure Web-Portal und die CLI Azure.  Im nächsten Schritt wird ein Paar aus SSH Schlüssel zu erstellen, die in Linux SSH wird, verwendet ohne dass ein Kennwort.  [Erstellen von SSH Tastenreihe Linux und Mac](virtual-machines-linux-mac-create-ssh-keys.md) Kennwort zugängliche Benutzernamen und Erhöhung der Sicherheit aktivieren.

## <a name="getting-started-with-linux-on-microsoft-azure"></a>Erste Schritte mit Linux auf Microsoft Azure
Mit der Azure-Konto einrichten, Azure CLI installiert und SSH Tasten erstellt sind Sie bereit zum Starten einer Infrastruktur in der Cloud Azure zu erstellen.  Die erste Aufgabe wird ein paar virtuellen Computern erstellt.

## <a name="create-a-vm-using-the-cli"></a>Erstellen eines virtuellen Computers verwenden die CLI
Erstellen einer Linux VM mithilfe der CLI ist eine schnelle Möglichkeit zum Bereitstellen eines virtuellen Computers ohne das Terminal verlassen zu müssen, in dem Sie arbeiten.  Alles im Web-Portal angegebenen steht über eine Befehlszeile Kennzeichnung oder wechseln.  

- [Erstellen einer Linux VM mithilfe der CLI](virtual-machines-linux-quick-create-cli.md)

## <a name="create-a-vm-in-the-portal"></a>Erstellen eines virtuellen Computers im portal
Erstellen einer Linux VM im Web Azure-Portal ist eine Möglichkeit auf einfache Weise zeigen und klicken Sie durch die verschiedenen Optionen, um eine Bereitstellung zu gelangen.  Anstatt Befehlszeile Kennzeichen oder Schalter zu verwenden, können Sie eine übersichtliche Weblayout der verschiedenen Optionen und Einstellungen anzeigen.  Alles über die Befehlszeile zur Verfügung steht auch im Portal.

- [Erstellen einer Linux VM Verwenden des Portals](virtual-machines-linux-quick-create-portal.md)

## <a name="login-using-ssh-without-a-password"></a>Melden Sie sich mit SSH ohne Kennwort
Der virtuellen Computer jetzt auf Azure ausgeführt wird und Sie bereit sind, melden Sie sich befinden.  Melden Sie sich über SSH mit einem Kennwort ist unsicher und Zeit in Anspruch nehmen.  Die Verwendung von SSH Schlüssel ist die sicherste Methode und auch die schnellste Möglichkeit zur Anmeldung.  Wenn Sie Sie Linux VM über das Portal oder die CLI erstellt haben, können Sie zwei Authentifizierungsoptionen.  Wenn Sie ein Kennwort für SSH auswählen, konfiguriert Azure den virtuellen Computer zum Anmelden über Kennwörter zuzulassen.  Wenn Sie einen öffentlichen SSH-Schlüssel verwenden möchten, Azure konfiguriert den virtuellen Computer, damit nur über SSH Tasten Benutzernamen und Kennwort Benutzernamen deaktiviert. Wenn Sie Ihre Linux VM sichern, indem Sie nur SSH Key Benutzernamen zu gestatten, verwenden Sie öffentliche Key SSH Option während der Erstellung virtueller Computer im Portal oder CLI.

- [Deaktivieren Sie SSH Kennwörter für Ihre Linux VM, indem SSHD konfigurieren](virtual-machines-linux-mac-disable-ssh-password-usage.md)

## <a name="related-azure-components"></a>Verwandte Azure-Komponenten

## <a name="storage"></a>Speicher

- [Einführung in Microsoft Azure-Speicher](../storage/storage-introduction.md)

- [Fügen Sie einen Datenträger einer Linux VM mithilfe der Azure-cli](virtual-machines-linux-add-disk.md)

- [So fügen Sie einen Datenträger einer Linux VM Azure-Portal](virtual-machines-linux-attach-disk-portal.md)

## <a name="networking"></a>Netzwerke

- [Virtuelle Network (Übersicht)](../virtual-network/virtual-networks-overview.md)

- [IP-Adressen in Azure](../virtual-network/virtual-network-ip-addresses-overview-arm.md)

- [Öffnen von Ports einer Linux VM in Azure](virtual-machines-linux-nsg-quickstart.md)

- [Erstellen Sie einen vollqualifizierten Domänennamen Azure-Portal](virtual-machines-linux-portal-create-fqdn.md)


## <a name="containers"></a>Container

- [Virtuellen Computern und Container in Azure](virtual-machines-linux-containers.md)

- [Azure Service Container Einführung](../container-service/container-service-intro.md)

- [Bereitstellen eines Azure Container Dienst Clusters](../container-service/container-service-deployment.md)

## <a name="next-steps"></a>Nächste Schritte

Sie haben nun einen Überblick über Linux Azure.  Im nächsten Schritt wird befassen, und erstellen ein paar virtuellen Computern!

- [Erstellen einer Linux VM auf Azure Verwenden des Portals](virtual-machines-linux-quick-create-portal.md)

- [Erstellen einer Linux VM auf Azure mithilfe der CLI](virtual-machines-linux-quick-create-cli.md)

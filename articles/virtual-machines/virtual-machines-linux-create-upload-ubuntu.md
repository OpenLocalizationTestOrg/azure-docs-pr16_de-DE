<properties
    pageTitle="Erstellen und Hochladen einer Ubuntu Linux virtuelle Festplatte in Azure"
    description="Informationen Sie zum Erstellen und Hochladen einer Azure virtuellen Festplatte (virtuelle Festplatte), die ein Betriebssystem Ubuntu Linux enthält."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="szarkos"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="szark"/>

# <a name="prepare-an-ubuntu-virtual-machine-for-azure"></a>Vorbereiten einer Ubuntu virtuellen Computern für Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="official-ubuntu-cloud-images"></a>Offizielle Ubuntu Cloud Bilder
Ubuntu veröffentlicht jetzt offizielle Azure virtuellen Festplatten unter [http://cloud-images.ubuntu.com/](http://cloud-images.ubuntu.com/)heruntergeladen werden. Wenn Sie ein eigenes Bild für spezielle Ubuntu für Azure erstellen müssen, lieber als das manuelle nachstehenden Verfahren verwenden, empfiehlt es mit folgenden bekannte virtuellen Festplatten arbeiten beginnen und entsprechend anpassen. Die neuesten Versionen Bild können immer finden Sie unter den folgenden Speicherorten:

 - Ubuntu 12.04/präzise: [ubuntu-12.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/precise/release/ubuntu-12.04-server-cloudimg-amd64-disk1.vhd.zip)
 - Ubuntu 14.04/treuen: [ubuntu-14.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/trusty/release/ubuntu-14.04-server-cloudimg-amd64-disk1.vhd.zip)
 - Ubuntu 16.04/Xenial: [ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/xenial/release/ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip)


## <a name="prerequisites"></a>Erforderliche Komponenten

In diesem Artikel wird vorausgesetzt, dass Sie bereits ein Betriebssystem Ubuntu Linux in eine virtuelle Festplatte installiert haben. Mehrere Tools vorhanden sein, um VHD-Dateien, beispielsweise eine Virtualisierungslösung wie Hyper-V zu erstellen. Anweisungen finden Sie unter [Installieren der Hyper-V-Rolle und Konfigurieren eines virtuellen Computers](http://technet.microsoft.com/library/hh846766.aspx).

**Ubuntu Installation Notizen**

- Weitere Tipps zur Vorbereitung Linux Azure Siehe auch [Allgemeine Linux Installation Notizen](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) bitte.
- Das VHDX-Format wird in Azure, nur **feste virtuelle Festplatte**nicht unterstützt.  Sie können den Datenträger mithilfe von Hyper-V-Manager oder das Cmdlet konvertieren-virtuelle Festplatte virtuelle Festplatte-Format konvertieren.
- Wenn das System Linux installieren empfiehlt es sich, dass Sie Standardpartitionen statt LVM (oft die Standardeinstellung für viele Installationen) verwenden. Dies wird LVM Namenskonflikten mit duplizierten virtuellen Computern, vermeiden, besonders, wenn Sie ein Datenträger OS jemals virtueller von einem anderen Computer zur Behandlung dieses Problems angefügt werden muss. [LVM](virtual-machines-linux-configure-lvm.md) oder [RAID](virtual-machines-linux-configure-raid.md) möglicherweise auf Daten-CDs verwendet werden, wenn die bevorzugte.
- Konfigurieren Sie eine austauschen Partition nicht auf dem Datenträger OS. Der Linux-Agent kann zum Erstellen einer austauschen Datei auf dem Datenträger temporäre Ressource konfiguriert sein.  Weitere Informationen finden Sie in den folgenden Schritten.
- Alle der virtuellen Festplatten müssen Größen, die ein Vielfaches von 1 MB sind.


## <a name="manual-steps"></a>Manuelle Schritte

> [AZURE.NOTE] Vor dem Erstellen eigener benutzerdefinierter Ubuntu Bilder für Azure, erwägen Sie das Bild aus der [http://cloud-images.ubuntu.com/](http://cloud-images.ubuntu.com/) stattdessen.


1. Wählen Sie im mittleren Bereich des Hyper-V-Managers des virtuellen Computers.

2. Klicken Sie auf **Verbinden** , um das Fenster des virtuellen Computers zu öffnen.

3.  Ersetzen Sie den aktuellen Repositorys im Bild, um Ubuntus Azure Repogeschäfte verwenden. Die Schritte sind je nach der Version Ubuntu ein wenig anders.

    Vor dem Bearbeiten /etc/apt/sources.list, empfiehlt es sich um eine Sicherungskopie zu erstellen:

        # sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak

    Ubuntu 12.04:

        # sudo sed -i "s/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g" /etc/apt/sources.list
        # sudo apt-get update

    Ubuntu 14.04:

        # sudo sed -i "s/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g" /etc/apt/sources.list
        # sudo apt-get update

4. Die Bilder Ubuntu Azure folgen jetzt Kernel *Hardware-Aktivierung* (HWE). Aktualisieren Sie das Betriebssystem, das dem letzten Kernel durch Ausführen der folgenden Befehle:

    Ubuntu 12.04:

        # sudo apt-get update
        # sudo apt-get install linux-image-generic-lts-trusty linux-cloud-tools-generic-lts-trusty
        # sudo apt-get install hv-kvp-daemon-init
        (recommended) sudo apt-get dist-upgrade

        # sudo reboot

    Ubuntu 14.04:

        # sudo apt-get update
        # sudo apt-get install linux-image-virtual-lts-vivid linux-lts-vivid-tools-common
        # sudo apt-get install hv-kvp-daemon-init
        (recommended) sudo apt-get dist-upgrade

        # sudo reboot


5. Ändern der Kernel-Startzeile für Grub weitere Parameter für Azure aufnehmen möchten. Zweck öffnen "/ usw./Standard/Grub" in einem Text-Editor, suchen Sie nach der Variable mit der Bezeichnung `GRUB_CMDLINE_LINUX_DEFAULT` (oder fügen Sie es hinzu, falls erforderlich) anzeigen und bearbeiten, um die folgenden Parameter enthalten:

        GRUB_CMDLINE_LINUX_DEFAULT="console=tty1 console=ttyS0,115200n8 earlyprintk=ttyS0,115200 rootdelay=300"

    Speichern und schließen Sie diese Datei, und führen Sie dann '`sudo update-grub`'. Dadurch wird sichergestellt, dass alle Console-Nachrichten an den ersten seriellen Anschluss gesendet werden, was die Azure Service mit Debuggen von Problemen hilfreich sein können.

6.  Stellen Sie sicher, dass der Server SSH installiert und so konfiguriert, dass beim Systemstart startet.  Dies ist normalerweise der Standardwert.

7.  Installieren Sie den Azure Linux-Agent:

        # sudo apt-get update
        # sudo apt-get install walinuxagent

    Beachten Sie die Installation von der `walinuxagent` Paket entfernen, wird die `NetworkManager` und `NetworkManager-gnome` verpackt werden, wenn sie installiert sind.

8.  Führen Sie die folgenden Befehle zum Entziehen von des virtuellen Computers und Vorbereiten der Arbeitsmappe für die Bereitstellung auf Azure ein:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

9. Klicken Sie auf in Hyper-V-Manager- **Aktion -> Beenden nach unten** . Da Ihre VHD Linux kann nun auf Azure hochgeladen werden.


## <a name="next-steps"></a>Nächste Schritte
Sie nun können die Ubuntu Linux virtuelle Festplatte zum Erstellen neuen virtueller Maschinen in Azure verwenden. Ist dies das erste Mal, dass Sie die VHD-Datei auf Azure hochgeladen haben, finden Sie die Schritte 2 und 3 unter [Erstellen und Hochladen einer virtuellen Festplatte, die das Betriebssystem Linux enthält](virtual-machines-linux-classic-create-upload-vhd.md).

## <a name="references"></a>Verweise ##

Ubuntu Hardware-Aktivierung (HWE) Kernel:

- [http://Blog.utlemming.org/2015/01/Ubuntu-1404-Azure-Images-Now-Tracking.HTML](http://blog.utlemming.org/2015/01/ubuntu-1404-azure-images-now-tracking.html)
- [http://Blog.utlemming.org/2015/02/1204-Azure-Cloud-Images-Now-Using-Hwe.HTML](http://blog.utlemming.org/2015/02/1204-azure-cloud-images-now-using-hwe.html)

<properties
    pageTitle="Vorbereiten der Debian Linux virtuelle Festplatte | Microsoft Azure"
    description="Informationen Sie zum Erstellen von Debian 7 und 8 virtuelle Festplatte Dateien für die Bereitstellung in Azure."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="szarkos"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="szark"/>



# <a name="prepare-a-debian-vhd-for-azure"></a>Vorbereiten einer Debian virtuellen für Azure

## <a name="prerequisites"></a>Erforderliche Komponenten
In diesem Abschnitt wird davon ausgegangen, dass Sie bereits eine Debian Linux Betriebssystem aus einer ISO-Datei, die von der [Debian Website](https://www.debian.org/distrib/) heruntergeladen werden, um eine virtuelle Festplatte installiert haben. Mehrere Tools vorhanden sein, um VHD-Dateien erstellen; Hyper-V ist nur ein Beispiel. Anweisungen Hyper-V verwenden finden Sie unter [Installieren der Hyper-V-Rolle und Konfigurieren eines virtuellen Computers](https://technet.microsoft.com/library/hh846766.aspx).


## <a name="installation-notes"></a>Installation von Notizen

- Weitere Tipps zur Vorbereitung Linux Azure Siehe auch [Allgemeine Linux Installation Notizen](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) bitte.
- Das neuere VHDX-Format wird in Azure nicht unterstützt. Sie können den Datenträger mithilfe von Hyper-V-Manager oder das Cmdlet **Konvertieren-virtuelle Festplatte** virtuelle Festplatte-Format konvertieren.
- Wenn das System Linux installieren empfiehlt es sich, dass Sie Standardpartitionen statt LVM (oft die Standardeinstellung für viele Installationen) verwenden. Dies wird LVM Namenskonflikten mit duplizierten virtuellen Computern, vermeiden, besonders, wenn Sie ein Datenträger OS jemals virtueller von einem anderen Computer zur Behandlung dieses Problems angefügt werden muss. [LVM](virtual-machines-linux-configure-lvm.md) oder [RAID](virtual-machines-linux-configure-raid.md) möglicherweise auf Daten-CDs verwendet werden, wenn die bevorzugte.
- Konfigurieren Sie eine austauschen Partition nicht auf dem Datenträger OS an. Der Azure Linux-Agent kann zum Erstellen einer austauschen Datei auf dem Datenträger temporäre Ressource konfiguriert sein. Weitere Informationen finden Sie in den folgenden Schritten.
- Alle der virtuellen Festplatten müssen Größen, die ein Vielfaches von 1 MB sind.


## <a name="use-azure-manage-to-create-debian-vhds"></a>Zum Erstellen von virtuellen Debian Festplatten verwenden Sie Azure verwalten

Es stehen Tools zum Generieren von Debian virtuellen Festplatten für Azure, wie die [Azure-verwalten](https://github.com/credativ/azure-manage) Skripts aus [Credativ](http://www.credativ.com/). Dies ist empfiehlt es sich im Vergleich zu Abbildung ganz neu erstellen. Beispielsweise zum Erstellen einer Debian 8 virtuellen führen Sie die folgenden Befehle zum Herunterladen Azure-verwalten (und Abhängigkeiten), und führen Sie das Skript Azure_build_image:

    # sudo apt-get update
    # sudo apt-get install git qemu-utils mbr kpartx debootstrap

    # sudo apt-get install python3-pip python3-dateutil python3-cryptography
    # sudo pip3 install azure-storage azure-servicemanagement-legacy azure-common pytest pyyaml
    # git clone https://github.com/credativ/azure-manage.git
    # cd azure-manage
    # sudo pip3 install .

    # sudo azure_build_image --option release=jessie --option image_size_gb=30 --option image_prefix=debian-jessie-azure section


## <a name="manually-prepare-a-debian-vhd"></a>Manuelles Vorbereiten einer Debian virtuellen

1. Wählen Sie im Hyper-V-Manager des virtuellen Computers.

2. Klicken Sie auf **Verbinden** , um eine Console-Fenster des virtuellen Computers zu öffnen.

3. Kommentieren Sie die Zeile für **die CD-ROM Deb** in `/etc/apt/source.list` Wenn Sie den virtuellen Computer anhand einer ISO-Datei festlegen.

4. Bearbeiten der `/etc/default/grub` ' Datei ', und ändern Sie den Parameter **GRUB_CMDLINE_LINUX** wie folgt, um weitere Parameter für Azure umfassen.

        GRUB_CMDLINE_LINUX="console=tty0 console=ttyS0,115200 earlyprintk=ttyS0,115200 rootdelay=30"

5. Die Grub neu zu erstellen und ausführen:

        # sudo update-grub

6. Hinzufügen des Debian Azure Repositorys zu /etc/apt/sources.list für Debian 7 oder 8:

    **Debian 7.x "Wheezy"**

        deb http://debian-archive.trafficmanager.net/debian wheezy-backports main
        deb-src http://debian-archive.trafficmanager.net/debian wheezy-backports main
        deb http://debian-archive.trafficmanager.net/debian-azure wheezy main
        deb-src http://debian-archive.trafficmanager.net/debian-azure wheezy main


    **Debian 8.x "Jessie"**

        deb http://debian-archive.trafficmanager.net/debian jessie-backports main
        deb-src http://debian-archive.trafficmanager.net/debian jessie-backports main
        deb http://debian-archive.trafficmanager.net/debian-azure jessie main
        deb-src http://debian-archive.trafficmanager.net/debian-azure jessie main


7. Installieren Sie den Azure Linux-Agent:

        # sudo apt-get update
        # sudo apt-get install waagent

8. Für Debian 7 ist es erforderlich, um den 3.16-basierten Kernel aus dem Repository wheezy Backports auszuführen. Erstellen Sie zuerst eine Datei namens /etc/apt/preferences.d/linux.pref mit dem folgenden Inhalt:

        Package: linux-image-amd64 initramfs-tools
        Pin: release n=wheezy-backports
        Pin-Priority: 500

    Führen Sie dann "Sudo apt-Get-Installation Linux-Bild-amd64", den neuen Kernel zu installieren.

8. Entziehen von des virtuellen Computers und Vorbereiten der Arbeitsmappe für die Bereitstellung auf Azure ausführen:

        # sudo waagent –force -deprovision
        # export HISTSIZE=0
        # logout

9. Klicken Sie auf die **Aktion** -> Beenden nach unten im Hyper-V-Manager. Da Ihre VHD Linux kann nun auf Azure hochgeladen werden.


## <a name="next-steps"></a>Nächste Schritte

Sie nun können die Debian virtuelle Festplatte zum Erstellen neuen virtueller Maschinen in Azure verwenden. Ist dies das erste Mal, dass Sie die VHD-Datei auf Azure hochgeladen haben, finden Sie die Schritte 2 und 3 unter [Erstellen und Hochladen einer virtuellen Festplatte, die das Betriebssystem Linux enthält](virtual-machines-linux-classic-create-upload-vhd.md).

<properties
    pageTitle="Erstellen und Hochladen einer Linux VHD in Azure"
    description="Informationen Sie zum Erstellen und Hochladen einer Azure virtuellen Festplatte (virtuelle Festplatte), die dem Betriebssystem Linux enthält."
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
    ms.date="09/23/2016"
    ms.author="szark"/>

# <a name="information-for-non-endorsed-distributions"></a>Informationen für die Verteilung nicht unterstützt #

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


**Wichtig**: der Azure-Plattform Vereinbarung zum SERVICELEVEL gilt für das Betriebssystem Linux virtuellen Computern nur, wenn eine der [Verteilung unterstützt](virtual-machines-linux-endorsed-distros.md) verwendet wird. Alle Linux-Versionen, die in der Bildergalerie Azure bereitgestellt werden, sind Vermerk zur Verteilung mit der Konfiguration erforderlich.

- [Linux auf Azure - unterstützt Verteilung](virtual-machines-linux-endorsed-distros.md)
- [Unterstützung für Linux Bilder in Microsoft Azure](https://support.microsoft.com/kb/2941892)

Alle Verteilung auf Windows Azure ausgeführte müssen eine Anzahl von erforderlichen Komponenten über eine Chance auf die Plattform ordnungsgemäß ausgeführt haben, entsprechen.  In diesem Artikel ist jedoch keine umfassende wie jede Verteilung unterscheidet; und es ist jedoch möglich, dass auch wenn Sie die unten aufgeführten Kriterien erfüllen müssen Sie weiterhin erheblich gibt Ihrem Linux-System, um sicherzustellen, dass es auf die Plattform ordnungsgemäß ausgeführt wird.

Es ist aus diesem Grund, die es empfiehlt sich, dass Sie mit einer der unsere [Linux auf Azure unterstützt Verteilung](virtual-machines-linux-endorsed-distros.md) nach Möglichkeit beginnen. Die folgenden Artikeln leitet Sie durch die verschiedenen Vermerk zur Linux-Versionen vorbereiten, die auf Azure unterstützt werden:

- **[CentOS-basierten Verteilung](virtual-machines-linux-create-upload-centos.md)**
- **[Debian Linux](virtual-machines-linux-debian-create-upload-vhd.md)**
- **[Oracle-Linux](virtual-machines-linux-oracle-create-upload-vhd.md)**
- **[Red Hat Enterprise Linux](virtual-machines-linux-redhat-create-upload-vhd.md)**
- **[SLES & openSUSE](virtual-machines-linux-suse-create-upload-vhd.md)**
- **[Ubuntu](virtual-machines-linux-create-upload-ubuntu.md)**

Im weiteren Verlauf dieses Artikels liegt der Schwerpunkt auf allgemeinen Leitfaden für Ihre Linux Verteilung auf Windows Azure ausgeführte.


## <a name="general-linux-installation-notes"></a>Allgemeine Linux Installation Notizen ##

- Das VHDX-Format wird in Azure, nur **feste virtuelle Festplatte**nicht unterstützt.  Sie können den Datenträger mithilfe von Hyper-V-Manager oder das Cmdlet konvertieren-virtuelle Festplatte virtuelle Festplatte-Format konvertieren. Wenn Sie VirtualBox verwenden bedeutet dies **feste Größe** im Gegensatz zu den standardmäßigen dynamisch zugewiesen werden, wenn den Datenträger erstellen auswählen.

- Wenn das System Linux installieren ist es *empfohlen* , dass Sie Standardpartitionen statt LVM (oft die Standardeinstellung für viele Installationen) verwenden. Dies wird LVM Namenskonflikten mit duplizierten virtuellen Computern, vermeiden, besonders, wenn Sie ein Datenträger OS jemals zur Behandlung dieses Problems zu einem anderen identischen virtuellen Computer angefügt werden muss. [LVM](virtual-machines-linux-configure-lvm.md) oder [RAID](virtual-machines-linux-configure-raid.md) möglicherweise auf Daten-CDs verwendet werden.

- Kernel-Unterstützung für UDFs Dateisysteme bereitstellen ist erforderlich. Beim ersten Start auf Azure wird die Bereitstellung Konfiguration für die Linux VM über Medien UDFs formatierten übergeben, der dem Gast zugeordnet ist. Der Linux Azure-Agent muss möglicherweise Laden des Dateisystems UDFs um seine Konfiguration lesen und den virtuellen Computer bereitstellen.

- Linux Kernelversionen unter 2.6.37 unterstützen keine NUMA auf Hyper-V mit größere virtueller Computer. Zu diesem Problem hauptsächlich Nachteile ältere Verteilung mithilfe der übergeordneten Red Hat 2.6.32 Kernel, und in RHEL 6.6 (Kernel 2.6.32 504) behoben wurde. Systeme mit benutzerdefinierten Kernels, die älter als 2.6.37 oder RHEL-basierten Kernels ältere als 2.6.32-504 den Boot-Parameter festlegen müssen `numa=off` auf dem Kernel in grub.conf Befehlszeile. Weitere Informationen finden Sie unter Red Hat [436883 KB](https://access.redhat.com/solutions/436883).

- Konfigurieren Sie eine austauschen Partition nicht auf dem Datenträger OS an. Der Linux-Agent kann zum Erstellen einer austauschen Datei auf dem Datenträger temporäre Ressource konfiguriert sein.  Weitere Informationen finden Sie in den folgenden Schritten.

- Alle der virtuellen Festplatten müssen Größen, die ein Vielfaches von 1 MB sind.


### <a name="installing-linux-without-hyper-v"></a>Installieren von Linux ohne Hyper-V ###

In einigen Fällen Linux Installer möglicherweise nicht enthalten die Treiber für Hyper-V in das ursprüngliche RAM-Laufwerk (Initrd oder Initramfs), wenn das Programm erkennt, dass es sich bei der Ausführung einer einer Hyper-V-Umgebung.  Bei Verwendung ein anderen Virtualisierung Systems (d. h. Virtualbox, KVM usw.) auf das Bild Linux Vorbereiten möglicherweise müssen Sie die Quelltabelle der Initrd, um sicherzustellen, dass mindestens die `hv_vmbus` und `hv_storvsc` Kernelmodule auf das ursprüngliche RAM-Laufwerk verfügbar sind.  Dies ist ein bekanntes Problem mindestens Betriebssystemen basierend auf der übergeordneten Red Hat zurück.

Das Verfahren für das Bild Initrd oder Initramfs Neuerstellen variieren je nach der Verteilung. Wenden Sie sich an Ihre Verteilung der Dokumentation, oder für das ordnungsgemäße Verfahren unterstützen.  Hier ein Beispiel für so Quelltabelle der initrd-mit den `mkinitrd` Programm:

Sichern Sie zunächst das vorhandene Initrd Bild ein:

    # cd /boot
    # sudo cp initrd-`uname -r`.img  initrd-`uname -r`.img.bak

Als Nächstes Quelltabelle der Initrd mit den `hv_vmbus` und `hv_storvsc` Kernelmodule:

    # sudo mkinitrd --preload=hv_storvsc --preload=hv_vmbus -v -f initrd-`uname -r`.img `uname -r`


### <a name="resizing-vhds"></a>Ändern der Größe von virtuellen Festplatten ###

Virtuelle Festplatte Bilder auf Azure müssen so virtuelle ausgerichtet auf 1 MB groß sein.  In der Regel sollte virtuelle Festplatten erstellt Hyper-V bereits richtig ausgerichtet werden.  Wenn die virtuelle Festplatte nicht richtig ausgerichtet ist möglicherweise dann Sie erhalten eine Fehlermeldung wie die folgende beim Versuch, ein *Bild* von Ihrem virtuellen Festplatte zu erstellen:

    "The VHD http://<mystorageaccount>.blob.core.windows.net/vhds/MyLinuxVM.vhd has an unsupported virtual size of 21475270656 bytes. The size must be a whole number (in MBs).”

Um dieses Problem zu beheben, können Sie den virtuellen Computer mithilfe der Verwaltungskonsole für Hyper-V oder das [Ändern der Größe-virtuelle Festplatte](http://technet.microsoft.com/library/hh848535.aspx) Powershell-Cmdlet Größe.  Wenn Sie nicht in einer Windows-Umgebung ausführen empfiehlt es konvertieren (falls erforderlich) mithilfe von Qemu-Img und Ändern der Größe der virtuellen Festplatte.

> [AZURE.NOTE] Es ist ein bekanntes Problem in Qemu-Img Versionen > = 2.2.1, das eine falsch formatierte virtuelle Festplatte ergibt. Das Problem wurde in QEMU 2.6 behoben. Es wird empfohlen, verwenden Sie entweder Qemu-Img 2.2.0 oder unteren oder Aktualisieren auf 2.6 oder höher. Referenz: https://bugs.launchpad.net/qemu/+bug/1490611.


 1. Ändern der Größe der virtuellen Festplatte direkt mithilfe von Tools wie `qemu-img` oder `vbox-manage` dazu führen, dass eine möglicherweise nicht mehr gestartet virtuelle Festplatte.  Daher empfiehlt es sich zum ersten konvertieren die virtuellen Festplatte zu einem Bild UNFORMATIERTE Festplatte.  Wenn das Bild virtueller Computer bereits als Image, REINE Daten (die Standardeinstellung für einige Hypervisoren wie KVM) erstellt wurde, können Sie diesen Schritt überspringen:

        # qemu-img convert -f vpc -O raw MyLinuxVM.vhd MyLinuxVM.raw

 2. Berechnen Sie die erforderliche Größe des Bilds Datenträger, stellen Sie sicher, dass die virtuelle Größe 1 MB ausgerichtet wird.  Mit diesem kann die folgenden Bash Shellskript unterstützen.  Verwendet das Skript "`qemu-img info`" die virtuelle Größe des Datenträgerabbilds bestimmt und die Größe der nächsten 1 MB berechnet:

        rawdisk="MyLinuxVM.raw"
        vhddisk="MyLinuxVM.vhd"

        MB=$((1024*1024))
        size=$(qemu-img info -f raw --output json "$rawdisk" | \
               gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

        rounded_size=$((($size/$MB + 1)*$MB))
        echo "Rounded Size = $rounded_size"

 3. Ändern der Größe der unformatierten Festplatte $Rounded_size verwenden, wie in der obigen Skripts eingerichtet sind:

        # qemu-img resize MyLinuxVM.raw $rounded_size

 4. Konvertieren Sie das UNFORMATIERTE Laufwerk jetzt wieder in eine feste Größe virtuelle Festplatte:

        # qemu-img convert -f raw -o subformat=fixed -O vpc MyLinuxVM.raw MyLinuxVM.vhd



## <a name="linux-kernel-requirements"></a>Linux Kernel Anforderungen ##

Die Linux Integration Services (LIS) Treiber für Hyper-V und Azure werden direkt an den übergeordneten Linux Kernel bereitgestellt. Viele Versionen, die eine neuere Linux Kernel-Version (d. h. 3.x) enthalten verfügen bereits über diese Treiber zur Verfügung stehen, oder andernfalls mehr Versionen dieser Treiber mit den deren Kerneln bereitstellen.  Diese Treiber werden beständig aktualisiert, in der übergeordneten Kernel mit neuen Updates und Features, damit nach Möglichkeit empfohlen wird eine [Verteilung unterstützt](virtual-machines-linux-endorsed-distros.md) ausführen, die diese Problembehebungen und Updates enthalten sein sollen.

Wenn Sie eine Variante von Red Hat Enterprise Linux Versionen **6.0-6.3**ausgeführt werden, müssen Sie die neuesten LIS Treiber für Hyper-V zu installieren. Die Treiber finden Sie [unter](http://go.microsoft.com/fwlink/p/?LinkID=254263&clcid=0x409). Bis zum RHEL **6.4 +** (und Varianten) der LIS-Treiber sind bereits mit dem Kernel enthalten und daher sind keine zusätzliche Installationspakete erforderlich, um diese Systeme auf Azure ausgeführt werden.

Wenn Sie ein benutzerdefinierter Kernel erforderlich ist, empfiehlt es sich eine neuere Kernelversion (d. h. **3,8 +**) verwenden. Für diese Verteilung oder Lieferanten und Pflege ihrer eigenen Kernel, werden einige leistungsgesteuert zur regelmäßig Backport LIS-Treiber aus dem übergeordneten Kernel an Ihre benutzerdefinierten Kernel erforderlich.  Auch wenn Sie bereits eine relativ zuletzt verwendete Kernelversion ausgeführt werden, wird es dringend empfohlen, zum Nachverfolgen eines übergeordneten Korrekturen LIS Treiber und Backport Personen je nach Bedarf. Die Position der Quelldateien LIS-Treiber ist in der Datei [MAINTAINERS](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/tree/MAINTAINERS) in der Linux Kernel Quellstruktur verfügbar:

    F:  arch/x86/include/asm/mshyperv.h
    F:  arch/x86/include/uapi/asm/hyperv.h
    F:  arch/x86/kernel/cpu/mshyperv.c
    F:  drivers/hid/hid-hyperv.c
    F:  drivers/hv/
    F:  drivers/input/serio/hyperv-keyboard.c
    F:  drivers/net/hyperv/
    F:  drivers/scsi/storvsc_drv.c
    F:  drivers/video/fbdev/hyperv_fb.c
    F:  include/linux/hyperv.h
    F:  tools/hv/

Wenigstens die Abwesenheit der folgenden Updates bekanntermaßen auf Azure Probleme verursachen und daher müssen diese in den Kernel enthalten sein. Diese Liste ist nicht vollständig oder für alle Verteilung durchführen:

- [Ata_piix: Laufwerke für die Hyper-V-Treiber standardmäßig zurückstellen](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/drivers/ata/ata_piix.c?id=cd006086fa5d91414d8ff9ff2b78fbb593878e3c)
- [Storvsc: Konto für Fremd-log Pakete in den Pfad zurücksetzen](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/drivers/scsi/storvsc_drv.c?id=5c1b10ab7f93d24f29b5630286e323d1c5802d5c)
- [Storvsc: Vermeiden der Verwendung von WRITE_SAME](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=3e8f4f4065901c8dfc51407e1984495e1748c090)
- [Storvsc: RAID und virtuelle Host Grafikkartentreiber Schreiben gleichen deaktivieren](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=54b2b50c20a61b51199bedb6e5d2f8ec2568fb43)
- [Storvsc: NULL-Zeiger Verweis beheben aufgehoben.](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=b12bb60d6c350b348a4e1460cd68f97ccae9822e)
- [Storvsc: anrufen Puffer möglicherweise fehlschlagen wird e/a-fixieren](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=e86fb5e8ab95f10ec5f2e9430119d5d35020c951)
- [Scsi_sysfs: Schützen Sie sich vor doppelte Ausführung des __scsi_remove_device](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/scsi_sysfs.c?id=be821fd8e62765de43cc4f0e2db363d0e30a7e9b)


## <a name="the-azure-linux-agent"></a>Der Azure Linux-Agent ##

Der [Azure Linux-Agent](virtual-machines-linux-agent-user-guide.md) (Waagent) ist erforderlich, eine Linux virtuellen Computern in Azure nicht ordnungsgemäß bereitstellen. Können Sie erhalten die neueste Version, die Datei Probleme oder Abruf Anfragen bei der [Linux Agent GitHub Repo](https://github.com/Azure/WALinuxAgent)übermitteln.

- Der Linux-Agent ist unter der Apache 2.0-Lizenz zur Verfügung. Viele Versionen bereits bieten u/min oder Deb Pakete für den Agent, und daher in einigen Fällen Dies kann installiert und mit wenig Aufwand aktualisiert.

- Der Azure-Linux-Agent benötigt Python 2.6 +.

- Der Agent ist auch das Modul Python-pyasn1 erforderlich. Die meisten Verteilung bietet dies als ein separates Paket, das installiert werden kann.

- In einigen Fällen möglicherweise der Azure-Linux Agent mit NetworkManager nicht kompatibel. Viele der u/Min/Deb Pakete von Verteilung bereitgestellten NetworkManager als ein Konflikt für das Paket Waagent konfigurieren und somit werden NetworkManager deinstallieren, bei der Installation von Linux-Agent-Paket.


## <a name="general-linux-system-requirements"></a>Allgemeine Linux-Systemanforderungen ##

- Ändern der Kernel-Startzeile in GRUB oder GRUB2 gehören die folgenden Parameter. Dadurch wird auch sichergestellt, alle Console-Nachrichten werden gesendet, mit dem ersten seriellen Anschluss, welche Azure helfen kann Support mit Debuggen Probleme:

        console=ttyS0,115200n8 earlyprintk=ttyS0,115200 rootdelay=300

    Dadurch wird auch sichergestellt, alle Console-Nachrichten werden gesendet, mit dem ersten seriellen Anschluss, welche Azure helfen kann Support mit Debuggen Probleme.

    Zusätzlich zu den oben genannten empfiehlt es sich so *Entfernen Sie* die folgenden Parameter, wenn sie vorhanden sind:

        rhgb quiet crashkernel=auto

    Grafische und ruhig Boot eignen sich nicht in einer Cloud-Umgebung, in dem die Protokolle der serielle Anschluss gesendet werden sollen.

    Die `crashkernel` Option möglicherweise links auf Wunsch konfigurieren, aber beachten Sie, dass für diesen Parameter reduzieren wird die Menge der verfügbaren Arbeitsspeicher auf dem virtuellen Computer durch 128MB oder mehr, was auf das kleinere virtueller Computer problematische kann.

- Installieren des Azure Linux-Agents

    Der Azure-Linux-Agent ist für die Bereitstellung eines Bilds Linux auf Azure erforderlich.  Viele Versionen Bereitstellen des Agents als u/min oder Deb-Paket (das Paket wird in der Regel 'WALinuxAgent' oder 'Walinuxagent' bezeichnet).  Der Agent kann auch mithilfe der Schritte im [Linux Agent Leitfaden](virtual-machines-linux-agent-user-guide.md)manuell installiert werden.

- Stellen Sie sicher, dass der Server SSH installiert und so konfiguriert, dass beim Systemstart startet.  Dies ist normalerweise der Standardwert.

- Erstellen Sie austauschen Speicherplatz kann nicht auf dem Datenträger OS

    Der Azure-Linux Agent können automatisch austauschen Bereiche mit einem lokalen Festplatte der, die an den virtuellen Computer angeschlossen ist, nach der Bereitstellung auf Azure konfigurieren. Beachten Sie, dass der Datenträger lokale Ressource eine *temporäre* Festplatte ist und möglicherweise gelöscht werden, wenn der virtuellen Computer hat, wird ein. Nach der Installation von der Azure Linux-Agent (siehe vorherigen Schritt), ändern Sie entsprechend die folgenden Parametern im /etc/waagent.conf:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

- Im letzten Schritt, führen Sie die folgenden Befehle zum Entziehen von des virtuellen Computers:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

    >[AZURE.NOTE] Klicken Sie auf Virtualbox möglicherweise den folgenden Fehler nach Ausführung angezeigt ' Waagent-force - entziehen ': `[Errno 5] Input/output error`. Diese Fehlermeldung ist nicht kritische und kann ignoriert werden.

- Sie müssen zum Beenden des virtuellen Computers und die virtuelle Festplatte auf Azure hochladen.

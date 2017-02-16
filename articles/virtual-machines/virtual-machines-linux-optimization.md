<properties
    pageTitle="Optimieren Ihrer Linux virtueller Computer auf Azure | Microsoft Azure"
    description="Erfahren Sie einige Tipps zur leistungsoptimierung, um sicherzustellen, dass Sie Ihre Linux VM für optimale Leistung von Azure eingerichtet haben"
    keywords="Linux virtuellen Computern, virtuellen Computern Linux, Ubuntu virtuellen Computern" 
    services="virtual-machines-linux"
    documentationCenter=""
    authors="rickstercdn"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="rclaus"/>

# <a name="optimize-your-linux-vm-on-azure"></a>Optimieren Sie Ihrer Linux virtueller Computer auf Azure

Erstellen eines Linux virtuellen Computers (virtueller Computer) kann leicht über die Befehlszeile oder aus dem Portal führen. In diesem Lernprogramm erfahren Sie, wie Sie sicherstellen können Sie ihn eingerichtet haben, um die Leistung von Microsoft Azure-Plattform optimieren. In diesem Thema wird eine Ubuntu Server virtueller Computer verwendet, aber Sie können auch Linux virtuellen Computern mit [eigener Bilder als Vorlagen](virtual-machines-linux-create-upload-generic.md)erstellen.  

## <a name="prerequisites"></a>Erforderliche Komponenten

In diesem Thema wird davon ausgegangen, Sie bereits eine arbeiten Azure-Abonnement ([kostenlose Testversion Anmeldung](https://azure.microsoft.com/pricing/free-trial/)), [die Azure CLI installiert](../xplat-cli-install.md) haben, und haben bereits nach der Bereitstellung eines virtuellen Computers in Ihrem Abonnement Azure. Bevor Sie einen anderen Wert als mit Azure - Aufwand müssen Sie Ihrem Abonnement authentifizieren. Geben Sie hierzu mit Azure CLI einfach `azure login` den interaktiven Prozess gestartet. 

## <a name="azure-os-disk"></a>Azure OS Datenträger

Nachdem Sie eine Linux Vm in Azure erstellen, muss es sich um zwei Datenträger zugeordnet. / dev/sda ist der Datenträger OS, /dev/sdb als temporären Datenträger.  Verwenden Sie nicht den Hauptfenster OS Datenträger (/ Entwicklung/Sda) für alles mit Ausnahme des Betriebssystems, wie er für schnelle virtueller Computer Startzeit optimiert ist und keine gute Leistung für Ihre Last bietet. Ein oder mehrere Laufwerke zu Ihrer virtuellen Computer abzurufenden beständigen angefügt werden soll, und Speicherplatz für Ihre Daten optimiert. 

## <a name="adding-disks-for-size-and-performance-targets"></a>Hinzufügen von Festplatten für Ziele Größe und Leistung 

Je nach Größe virtueller Computer, können Sie bis zu 16 zusätzliche Datenträger auf einer A-Serie, 32 Datenträger auf eine D-Serie Anfügen und 64 Datenträger auf eine G-Serie Computer - jedes bis zu 1 TB groß. Sie hinzufügen zusätzliche Datenträger, pro Ihrer Leerzeichen und IOps Anforderungen nach Bedarf. Jeder Datenträger verfügt über ein Performance-Ziel von 500 IOps für Standard-Speicher und bis zu 5000 IOps pro Datenträger Premium Storage.  Weitere Informationen zu Premium-Datenträger, finden Sie in [Premium Speicher: leistungsstarke Storage für Azure-virtuellen Computern](../storage/storage-premium-storage.md)

Um die höchsten IOps auf Premium-Datenträger zu erreichen, deren Einstellungen des Caches auf "Schreibgeschützt" oder "Keine" festgelegt wurden, müssen Sie "Hindernisse" deaktivieren, während Sie das Dateisystem in Linux laden. Sie brauchen nicht Hindernisse, da die schreibt in Premium Speicher gesicherten Datenträger dauerhaften für diese Einstellungen des Caches sind.

- Wenn Sie **ReiserFS**verwenden, deaktivieren Sie mit der Option bereitstellen Hindernisse "Hindernis = keine" (verwenden Sie zum Aktivieren der Hindernisse, "Hindernis Löschvorgang =")
- Wenn Sie **ext3/ext4**verwenden, deaktivieren Sie mit der Option bereitstellen Hindernisse "Hindernis = 0" (verwenden Sie zum Aktivieren der Hindernisse, "Hindernis = 1")
- Wenn Sie **XFS**verwenden, deaktivieren Sie mit der Option bereitstellen "Nobarrier" Hindernisse (zum Aktivieren der Hindernisse, verwenden Sie die Option "Hindernis")

## <a name="storage-account-considerations"></a>Aspekte für Speicher-Konto

Wenn Sie Ihre Linux VM in Azure erstellen, sollten Sie sicherstellen, dass Sie die Datenträger aus, die in der gleichen Region als Ihre virtuellen Computer Nähe sicherzustellen und Netzwerkwartezeit minimieren Speicherkonten anfügen.  Jedes Konto standardmäßigen Speicher kann maximal 20 k IOps und einer Kapazität von 500 TB Größe.  Dies funktioniert in etwa 40 häufig verwendeten Datenträger, einschließlich der Datenträger OS und alle Daten Datenträger, die Sie erstellen. Für den Speicher Premium-Konten es gibt keine Beschränkung der maximalen IOps, aber es gibt maximal 32 TB Größe aus. 

Beim Umgang mit hoher Auslastung IOps und Sie haben Standard-Speicher für Datenträger, müssen Sie Aufteilen der Datenträger über mehrere Speicherkonten, um sicherzustellen, dass Sie nicht den 20.000 IOps Grenzwert für Speicherplatz Standard-Konten getroffen haben. Ihre virtuellen Computer kann eine Kombination aus Datenträger über verschiedene Speicherkonten Kontotypen Speicher Ihre optimale Konfiguration erzielen enthalten. 

## <a name="your-vm-temporary-drive"></a>Dem virtuellen Computer temporären Laufwerk

Standardmäßig beim Erstellen eines virtuellen Computers bietet Azure ein OS (/ Entwicklung/Sda) und einen temporären Datenträger (/ Entwicklung/Sdb).  Alle zusätzlichen Datenträger, die Sie hinzufügen werden als /dev/sdc, /dev/sdd, /dev/sde und so weiter. Alle Daten auf dem temporären Datenträger (/ Entwicklung/Sdb) ist nicht dauerhaften und können verloren gehen, wenn bestimmte Ereignisse wie virtueller Computer ändern der Größe, für die erneute Bereitstellung, oder Wartung einen Neustart des Ihrer virtuellen Computer erzwingt.  Die Größe und den Typ des als temporären Datenträger bezieht sich auf die virtuellen Computer Größe, die Sie zum Zeitpunkt der Bereitstellung ausgewählt haben. Wenn keines der Premium Größe virtuellen Computern (DS, G und DS_V2 Reihen) wird das temporäre Laufwerk durch eine lokale SSD für zusätzliche Leistung von bis zu 48 k IOps unterstützt. 

## <a name="linux-swap-file"></a>Austauschen von Linux-Datei

Ist Ihre Azure-virtuellen Computer von einem Bild Ubuntu oder CoreOS, können Sie CustomData verwenden, um einen Cloud-Config an Cloud der Initialisierung zu senden. Wenn Sie [ein benutzerdefiniertes Linux Bild hochgeladen](virtual-machines-linux-upload-vhd.md) , die der Initialisierung Cloud verwendet, Sie auch austauschen Partitionen mithilfe der Initialisierung Cloud konfigurieren.

Klicken Sie auf Ubuntu Cloud Bilder müssen Sie Cloud der Initialisierung so konfigurieren Sie die austauschen Partition verwenden. Auf dem Ubuntu-Wiki Weitere Informationen hierzu finden Sie in der folgenden Seite: [AzureSwapPartitions](https://wiki.ubuntu.com/AzureSwapPartitions).

Für Bilder ohne Unterstützung der Initialisierung Cloud verfügen virtueller Computer Bilder aus dem Azure Marketplace bereitgestellt einen virtuellen Computer Linux-Agent in das Betriebssystem integriert. Dieser Agent ermöglicht den virtuellen Computer mit verschiedenen Azure Diensten interagieren. Vorausgesetzt, dass Sie ein Standardbild aus dem Azure Marketplace bereitgestellt haben, müssen Sie gehen Sie wie folgt vor, um Ihre Linux austauschen Datei Einstellungen korrekt zu konfigurieren:

Suchen und Ändern von zwei Einträge in der Datei **/etc/waagent.conf** . Steuern Sie das Vorhandensein einer Datei dedizierten austauschen und die Größe der Datei austauschen. Sind die Parameter zum Ändern gefunden werden `ResourceDisk.EnableSwap=N` und`ResourceDisk.SwapSizeMB=0` 

Sie müssen diese in folgender Weise ändern:

* ResourceDisk.EnableSwap=Y
* ResourceDisk.SwapSizeMB={size in MB an Ihre Bedürfnisse} 

Nachdem Sie die Änderung vorgenommen haben, müssen Sie einen Neustart der Waagent oder Ihrer Linux VM entsprechend aktualisiert.  Sie kennen die Änderungen durchgeführt worden sind und bei der Verwendung eine Datei austauschen erstellt wurde die `free` Befehl zum Freigeben von Speicherplatz anzeigen. Im folgenden Beispiel hat eine 512MB austauschen-Datei, die als Ergebnis ändern die Datei waagent.conf erstellt wurden.

    admin@mylinuxvm:~$ free
                total       used       free     shared    buffers     cached
    Mem:       3525156     804168    2720988        408       8428     633192
    -/+ buffers/cache:     162548    3362608
    Swap:       524284          0     524284
    admin@mylinuxvm:~$
 
## <a name="io-scheduling-algorithm-for-premium-storage"></a>E/a-Planung Algorithmus für Premium-Speicher

Mit der 2.6.18 Linux Kernel, der Standard-e/a Algorithmus Planung von Stichtag in CFQ (vollständig ' wissenschaftsmesse ' Warteschlange Algorithmus) geändert wurde. Direktzugriff e/a-Mustern ist es unerheblich Unterschied Leistungsunterschiede zwischen der CFQ und Stichtag ein.  Für SSD-basierten Datenträger, in dem das Datenträger e/a-Muster überwiegend fortlaufend ist, kann die Wechsel zurück zu den Algorithmus NOOP oder Stichtag e/a-Leistung verbessert.

### <a name="view-the-current-io-scheduler"></a>Anzeigen der aktuellen e/a-scheduler

Verwenden Sie den folgenden Befehl ein:  

    admin@mylinuxvm:~# cat /sys/block/sda/queue/scheduler

Sie sehen nach Ausgabe, die festlegt, mit den aktuellen Planer.  

    noop [deadline] cfq

###<a name="change-the-current-device-devsda-of-io-scheduling-algorithm"></a>Ändern Sie das aktuelle Gerät (/ Entwicklung/Sda) e/a-Algorithmus Planung

Verwenden Sie die folgenden Befehle:  

    azureuser@mylinuxvm:~$ sudo su -
    root@mylinuxvm:~# echo "noop" >/sys/block/sda/queue/scheduler
    root@mylinuxvm:~# sed -i 's/GRUB_CMDLINE_LINUX=""/GRUB_CMDLINE_LINUX_DEFAULT="quiet splash elevator=noop"/g' /etc/default/grub
    root@mylinuxvm:~# update-grub

>[AZURE.NOTE] Festlegen von dies für/dev/sda allein ist nicht hilfreich. Es muss auf alle Daten Datenträger festgelegt werden, in dem das Muster e/a-dominiert sequenzielle i/o.  

Die folgende Ausgabe, anzeigt, dass die grub.cfg erfolgreich neu erstellt wurde und dass die standardmäßigen Scheduler zu NOOP aktualisiert wurde, sollte angezeigt werden.  

    Generating grub configuration file ...
    Found linux image: /boot/vmlinuz-3.13.0-34-generic
    Found initrd image: /boot/initrd.img-3.13.0-34-generic
    Found linux image: /boot/vmlinuz-3.13.0-32-generic
    Found initrd image: /boot/initrd.img-3.13.0-32-generic
    Found memtest86+ image: /memtest86+.elf
    Found memtest86+ image: /memtest86+.bin
    done

Für die Verteilung Familie Red hat benötigen Sie nur den folgenden Befehl aus:   

    echo 'echo noop >/sys/block/sda/queue/scheduler' >> /etc/rc.local

## <a name="using-software-raid-to-achieve-higher-iops"></a>Mit der Software RAID erzielen höher ich / Ops

Wenn Ihre Auslastung mehr IOps benötigen als ein einzelner Datenträger bereitstellen kann, müssen Sie eine Software RAID-Konfiguration von mehreren Laufwerken zu verwenden. Da Azure bereits Datenträger Stabilität auf der Ebene der lokalen Fabric ausführt, erzielen Sie die höchste Leistung aus einer RAID0 Striping Konfiguration aus.  Sie müssen bereitstellen, erstellen neue Datenträger in der Azure-Umgebung und verbinden Sie sie mit Ihrem Linux VM vor Partitionierung, Formatierung und Bereitstellen von den Laufwerken.  Weitere Informationen zum Konfigurieren von einer Software RAID-Setup auf Ihrer Linux VM in Azure finden Sie im Dokument **[Konfigurieren-Software-RAID auf Linux](virtual-machines-linux-configure-raid.md)** .


## <a name="next-steps"></a>Nächste Schritte

Beachten Sie, wie mit allen Optimierung Diskussionen, müssen Sie Tests ausführen, vor und nach jeder Änderung an den Einfluss messen, die, den die Änderung haben wird.  Optimierung ist ein Schritt-für-Schritt-Prozess, die unterschiedliche Ergebnisse auf verschiedenen Computern in Ihrer Umgebung.  Was für eine Konfiguration funktioniert möglicherweise nicht für andere arbeiten.

Einige nützliche Links zu zusätzlichen Ressourcen: 

- [Premium Speicher: Leistungsstarke Storage für Auslastung Azure-virtuellen Computern](../storage/storage-premium-storage.md)
- [Azure Linux Agent-Benutzerhandbuch](virtual-machines-linux-agent-user-guide.md)
- [Optimieren der Leistung MySQL Linux Azure-virtuellen Computern](virtual-machines-linux-classic-optimize-mysql.md)
- [Konfigurieren von Software-RAID auf Linux](virtual-machines-linux-configure-raid.md)

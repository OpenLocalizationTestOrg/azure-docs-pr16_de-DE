<properties 
    pageTitle="Konfigurieren von Software-RAID auf einem Computer mit Linux | Microsoft Azure" 
    description="Erfahren Sie, wie mit Mdadm RAID-On Linux in Azure konfigurieren." 
    services="virtual-machines-linux" 
    documentationCenter="na" 
    authors="rickstercdn"  
    manager="timlt" 
    editor="tysonn"
    tag="azure-service-management,azure-resource-manager" />

<tags 
    ms.service="virtual-machines-linux" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/06/2016" 
    ms.author="rclaus"/>



# <a name="configure-software-raid-on-linux"></a>Konfigurieren von Software-RAID auf Linux
Es ist ein gängiges Szenario Software-RAID auf Linux virtuellen Computern in Azure verwenden, können Sie mehrere angefügten Daten Datenträger als ein einzelnes RAID-Gerät übersichtlicher. Dies kann in der Regel verwendet werden, zum Verbessern der Leistung und verbesserte Durchsatz im Vergleich zur Verwendung von nur einer einzigen Festplatte zu ermöglichen.


## <a name="attaching-data-disks"></a>Anfügen von Daten Datenträger
Zwei oder mehr Daten leeren Datenträger sind so konfigurieren Sie ein RAID-Gerät erforderlich.  Der wichtigste Grund für die Erstellung eines RAID-Geräts ist für optimale Leistung der Festplatte EA.  Je nach Ihren Anforderungen EA, können Sie Datenträger anfügen, die in unseren Standard-Speicher mit bis zu 500 EA/Ps pro Datenträger oder unsere Speicher Premium mit bis zu 5000 EA/Ps pro Datenträger gespeichert werden. In diesem Artikel geht nicht in den Details zum Bereitstellen und Daten Datenträger an einem Linux virtuellen Computern anzufügen.  Finden Sie in der Microsoft Azure Artikel [einen Datenträger anfügen](virtual-machines-linux-add-disk.md) ausführliche Anweisungen zum Anfügen eines leeren Datenträgers mit einem Linux virtuellen Computer auf Azure.


## <a name="install-the-mdadm-utility"></a>Installieren Sie die mdadm

- **Ubuntu**

        # sudo apt-get update
        # sudo apt-get install mdadm

- **CentOS & Oracle-Linux**

        # sudo yum install mdadm

- **SLES und openSUSE**

        # zypper install mdadm


## <a name="create-the-disk-partitions"></a>Erstellen der Festplattenpartitionen
In diesem Beispiel erstellen wir eine einzelne Festplattenpartition auf /dev/sdc aus. Die neue Laufwerkpartition wird /dev/sdc1 aufgerufen.

1. Starten Sie um das Erstellen von Partitionen beginnen fdisk

        # sudo fdisk /dev/sdc
        Device contains neither a valid DOS partition table, nor Sun, SGI or OSF disklabel
        Building a new DOS disklabel with disk identifier 0xa34cb70c.
        Changes will remain in memory only, until you decide to write them.
        After that, of course, the previous content won't be recoverable.

        WARNING: DOS-compatible mode is deprecated. It's strongly recommended to
                 switch off the mode (command 'c') and change display units to
                 sectors (command 'u').

2. Drücken Sie "n" aufgefordert werden, eine **n**ew Partition zu erstellen:

        Command (m for help): n

3. Drücken Sie dann 'p', um eine **p**wechseln Partition zu erstellen:

        Command action
            e   extended
            p   primary partition (1-4)

4. Drücken Sie "1" Partitionsnummer 1 auswählen:

        Partition number (1-4): 1

5. Wählen Sie den Ausgangspunkt für die neue Partition ein, oder drücken Sie `<enter>` akzeptieren Sie die Standardeinstellung, um die Partition am Anfang des freien Speicherplatzes auf dem Laufwerk zu platzieren:

        First cylinder (1-1305, default 1):
        Using default value 1

6. Wählen Sie die Größe der Partition aus, geben Sie beispielsweise '+10G', um eine 10 Gigabyte Partition zu erstellen. Oder drücken Sie `<enter>` erstellen Sie eine einzelne Partition, die das gesamte Laufwerk umfasst:

        Last cylinder, +cylinders or +size{K,M,G} (1-1305, default 1305): 
        Using default value 1305

7. Ändern Sie als Nächstes die-ID und **t**eben der Partition aus der Standard-ID '83' (Linux) ID 'fd' (Linux Raid Auto):

        Command (m for help): t
        Selected partition 1
        Hex code (type L to list codes): fd

8. Schließlich Schreiben der Partitionstabelle auf das Laufwerk, und beenden Sie Fdisk:

        Command (m for help): w
        The partition table has been altered!


## <a name="create-the-raid-array"></a>Erstellen Sie das RAID-array

1. Im folgenden Beispiel wird "Streifen" (RAID-Level 0) drei Partitionen auf drei separate Datenträger mit Daten (sdc1, sdd1, sde1) gespeichert.  Nach dem Ausführen dieser Befehl eines neuen RAID wird aufgerufen **/dev/md127** Gerät erstellt. Beachten Sie auch, wenn diese Daten Festplatten wir zuvor Teil ein anderes außer Kraft gesetzte RAID Array hinzufügen werden möglicherweise die `--force` -Parameter der `mdadm` Befehl:

        # sudo mdadm --create /dev/md127 --level 0 --raid-devices 3 \
          /dev/sdc1 /dev/sdd1 /dev/sde1

2. Erstellen Sie das Dateisystem auf dem neuen RAID-Gerät

    **CentOS, Oracle Linux, SLES 12, OpenSUSE und Ubuntu**

        # sudo mkfs -t ext4 /dev/md127

    **SLES 11**

        # sudo mkfs -t ext3 /dev/md127

    **SLES 11 und OpenSUSE** - boot.md aktivieren, und Erstellen von mdadm.conf

        # sudo -i chkconfig --add boot.md
        # sudo echo 'DEVICE /dev/sd*[0-9]' >> /etc/mdadm.conf

    >[AZURE.NOTE] Ein Neustart kann erforderlich sein, nachdem Sie diese Änderungen vorgenommen haben SUSE Betriebssystemen. Dieser Schritt ist *nicht* erforderlich, auf SLES 12.


## <a name="add-the-new-file-system-to-etcfstab"></a>Das neue Dateisystem in/etc/fstab hinzufügen

**Vorsicht:** Bearbeiten die Datei/etc/fstab nicht ordnungsgemäß ergeben in einem möglicherweise nicht mehr gestartet System. Wenn Sie unsicher sind, lesen Sie die Verteilung der Dokumentation Informationen, wie Sie diese Datei zu bearbeiten. Es wird auch empfohlen, dass eine Sicherungskopie der Datei/etc/fstab vor dem Bearbeiten erstellt wird.

1. Erstellen Sie die gewünschten Bereitstellungspunkts für Ihre neue Dateisystem beispielsweise:

        # sudo mkdir /data

2. Wenn/etc/fstab bearbeiten zu können, sollte der **UUID** in Bezug auf das Dateisystem statt auf den Namen des Geräts verwendet werden.  Verwenden der `blkid` Programm, mit die UUID für das neue Dateisystem zu ermitteln:

        # sudo /sbin/blkid
        ...........
        /dev/md127: UUID="aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee" TYPE="ext4"

3. Öffnen Sie/etc/fstab in einem Text-Editor, und fügen Sie einen Eintrag für das neue Dateisystem, beispielsweise:

        UUID=aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext4  defaults  0  2

    Oder klicken Sie auf **SLES 11 und OpenSUSE**:

        /dev/disk/by-uuid/aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext3  defaults  0  2

    Klicken Sie dann speichern Sie und schließen Sie/etc/fstab.

4. Testen der/usw./Fstab-Eintrag korrekt sind:

        # sudo mount -a

    Wenn dieser Befehl eine Fehlermeldung angezeigt führt, überprüfen Sie die Syntax der in der Datei/etc/fstab aus.

    Führen Sie als Nächstes die `mount` aus, um sicherzustellen, ist das Dateisystem bereitgestellt:

        # mount
        .................
        /dev/md127 on /data type ext4 (rw)

5. (Optional) Failsafe Boot-Parameter

    **Fstab Konfiguration**

    Viele enthält auch die entweder die `nobootwait` oder `nofail` bereitstellen Parameter, die die Datei/usw./Fstab hinzugefügt werden können. Diese Parameter zu Fehlern beim Bereitstellen von einem bestimmten Dateisystem und lassen Sie das Linux-System, um den Vorgang fortzusetzen, selbst wenn es nicht ordnungsgemäß Laden des RAID Dateisystems kann zu starten. Finden Sie weitere Informationen zu diesen Parametern in Ihrem einer Zufallsvariablen zurück.

    Beispiel (Ubuntu):

        UUID=aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext4  defaults,nobootwait  0  2

    **Linux Boot-Parameter**

    Zusätzlich zu den oben angegebenen Parameter, die Kernel-Parameter "`bootdegraded=true`" das System starten, selbst wenn das RAID als beschädigt oder beeinträchtigt, für Beispiel angesehen wird, wenn ein Datenlaufwerk aus der virtuellen Computern versehentlich entfernt wird. Standardmäßig könnte dies in einem nicht startfähigen System führen.

    Finden Sie in der Verteilung der Dokumentation zum Kernel Parameter ordnungsgemäß zu bearbeiten. Beispielsweise in viele Versionen (CentOS, Oracle Linux SLES 11) diese Parameter möglicherweise hinzugefügt werden manuell zu den "`/boot/grub/menu.lst`" Datei.  Klicken Sie auf Ubuntu für diesen Parameter hinzugefügt werden kann die `GRUB_CMDLINE_LINUX_DEFAULT` Variable auf "/ usw./Standard/Grub".

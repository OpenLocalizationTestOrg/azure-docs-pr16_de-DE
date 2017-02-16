<properties
    pageTitle="Hinzufügen von einem Datenträger zu Linux VM | Microsoft Azure"
    description="Erfahren Sie, einen beständigen Datenträger für Ihre Linux VM hinzufügen"
    keywords="Linux virtuellen Computern, fügen Sie Ressourcen Datenträger hinzu"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="rickstercdn"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager" />

<tags
    ms.service="virtual-machines-linux"
    ms.topic="article"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.date="09/06/2016"
    ms.author="rclaus"/>

# <a name="add-a-disk-to-a-linux-vm"></a>Fügen Sie einen Datenträger einer Linux VM

In diesem Artikel wird gezeigt, wie einen beständigen Datenträger Ihrer virtuellen Computer installieren, damit Sie Ihre Daten - beibehalten, können auch wenn aufgrund von Wartung oder Ändern der Größe Ihrer virtuellen Computer reprovisioned ist. Um einen Datenträger hinzufügen, [Die CLI Azure](../xplat-cli-install.md) im Modus Ressourcenmanager konfiguriert benötigten (`azure config mode arm`).  

## <a name="quick-commands"></a>Symbolleiste Befehle

Ersetzen Sie in den folgenden Befehl Beispielen, die Werte zwischen &lt; und &gt; mit den Werten aus Ihrer eigenen Umgebung.

```bash
azure vm disk attach-new <myuniquegroupname> <myuniquevmname> <size-in-GB>
```

## <a name="attach-a-disk"></a>Fügen Sie einen Datenträger

Anfügen eines neuen Datenträgers geht schnell. Typ `azure vm disk attach-new <myuniquegroupname> <myuniquevmname> <size-in-GB>` zu erstellen, und fügen Sie einen neuen GB Datenträger für Ihre virtuellen Computer. Wenn Sie ein Speicherkonto nicht explizit identifizieren, wird alle Datenträger, die Sie erstellen in demselben Speicherkonto platziert, wo sich Ihre OS Datenträger befinden.  Es sollte nun wie folgt aussehen:

```bash
azure vm disk attach-new myuniquegroupname myuniquevmname 5
```

Die Ausgabe

```bash
info:    Executing command vm disk attach-new
+ Looking up the VM "myuniquevmname"
info:    New data disk location: https://cliexxx.blob.core.windows.net/vhds/myuniquevmname-20150526-0xxxxxxx43.vhd
+ Updating VM "myuniquevmname"
info:    vm disk attach-new command OK
```

## <a name="connect-to-the-linux-vm-to-mount-the-new-disk"></a>Herstellen einer Verbindung zu den neuen Datenträger Bereitstellen der Linux VM mit

> [AZURE.NOTE] In diesem Thema verbindet mit eines virtuellen Computers Benutzernamen und Kennwörter verwenden. Um Paare aus öffentlichen und privaten Schlüsseln zur Kommunikation mit Ihrem virtuellen Computer zu verwenden, finden Sie unter [Verwenden von SSH mit Linux auf Azure](virtual-machines-linux-mac-create-ssh-keys.md). Sie können ändern, die **SSH** -Konnektivität des virtuellen Computern erstellt, mit der `azure vm quick-create` Befehl mithilfe der `azure vm reset-access` Befehl **SSH** -Zugriff vollständig zurücksetzen, hinzufügen oder Entfernen von Benutzern oder öffentliche Key Dateien zum Sichern des Zugriffs hinzufügen.

Sie müssen SSH in Ihrer Azure-virtuellen Computer partitionieren, formatieren und Bereitstellen von Ihrem neuen Datenträger, sodass Ihre Linux VM, die sie verwenden können. Wenn Sie nicht mit der Herstellen einer Verbindung mit **ssh**vertraut sind, wird der Befehl nimmt die Form `ssh <username>@<FQDNofAzureVM> -p <the ssh port>`, und sieht wie folgt aus:

```bash
ssh ops@myuni-westu-1432328437727-pip.westus.cloudapp.azure.com -p 22
```

Die Ausgabe

```bash
The authenticity of host 'myuni-westu-1432328437727-pip.westus.cloudapp.azure.com (191.239.51.1)' can't be established.
ECDSA key fingerprint is bx:xx:xx:xx:xx:xx:xx:xx:xx:x:x:x:x:x:x:xx.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'myuni-westu-1432328437727-pip.westus.cloudapp.azure.com,191.239.51.1' (ECDSA) to the list of known hosts.
ops@myuni-westu-1432328437727-pip.westus.cloudapp.azure.com's password:
Welcome to Ubuntu 14.04.2 LTS (GNU/Linux 3.16.0-37-generic x86_64)

* Documentation:  https://help.ubuntu.com/

System information as of Fri May 22 21:02:32 UTC 2015

System load: 0.37              Memory usage: 2%   Processes:       207
Usage of /:  41.4% of 1.94GB   Swap usage:   0%   Users logged in: 0

Graph this data and manage this system at:
  https://landscape.canonical.com/

Get cloud support with Ubuntu Advantage Cloud Guest:
  http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

ops@myuniquevmname:~$
```

Jetzt, da Sie mit Ihrem virtuellen Computer verbunden sind, sind Sie bereit sind, fügen Sie einen Datenträger.  Den Datenträger, zuerst suchen mit `dmesg | grep SCSI` (die Methode, die Sie verwenden, um Ihre neue Datenträger ermitteln kann variieren). In diesem Fall sieht ungefähr wie folgt aus:

```bash
dmesg | grep SCSI
```

Die Ausgabe

```bash
[    0.294784] SCSI subsystem initialized
[    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
[    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
[    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
[ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
```

und im Falle von Inhalt der `sdc` Datenträger ist diejenige, die wir möchten. Das Laufwerk mit jetzt partitionieren `sudo fdisk /dev/sdc` – unter der Voraussetzung, dass in Ihrem Fall der Datenträger war `sdc`, und stellen Sie sie einen primären Datenträger auf Partition 1, und die anderen Standardeinstellungen akzeptieren.

```bash
sudo fdisk /dev/sdc
```

Die Ausgabe

```bash
Device contains neither a valid DOS partition table, nor Sun, SGI or OSF disklabel
Building a new DOS disklabel with disk identifier 0x2a59b123.
Changes will remain in memory only, until you decide to write them.
After that, of course, the previous content won't be recoverable.

Warning: invalid flag 0x0000 of partition table 4 will be corrected by w(rite)

Command (m for help): n
Partition type:
   p   primary (0 primary, 0 extended, 4 free)
   e   extended
Select (default p): p
Partition number (1-4, default 1): 1
First sector (2048-10485759, default 2048):
Using default value 2048
Last sector, +sectors or +size{K,M,G} (2048-10485759, default 10485759):
Using default value 10485759
```

Die Partition erstellen, indem Sie mit der Eingabe `p` aufgefordert werden:

```bash
Command (m for help): p

Disk /dev/sdc: 5368 MB, 5368709120 bytes
255 heads, 63 sectors/track, 652 cylinders, total 10485760 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x2a59b123

   Device Boot      Start         End      Blocks   Id  System
/dev/sdc1            2048    10485759     5241856   83  Linux

Command (m for help): w
The partition table has been altered!

Calling ioctl() to re-read partition table.
Syncing disks.
```

Und Schreiben Sie ein Dateisystem auf die Partition mithilfe des Befehls **Mkfs** zurück, der dem Dateisystemtyp Ihrer und den Namen des Geräts angibt. In diesem Thema verwenden wir `ext4` und `/dev/sdc1` von oben:

```bash
sudo mkfs -t ext4 /dev/sdc1
```

Die Ausgabe

```bash
mke2fs 1.42.9 (4-Feb-2014)
Discarding device blocks: done
Filesystem label=
OS type: Linux
Block size=4096 (log=2)
Fragment size=4096 (log=2)
Stride=0 blocks, Stripe width=0 blocks
327680 inodes, 1310464 blocks
65523 blocks (5.00%) reserved for the super user
First data block=0
Maximum filesystem blocks=1342177280
40 block groups
32768 blocks per group, 32768 fragments per group
8192 inodes per group
Superblock backups stored on blocks:
    32768, 98304, 163840, 229376, 294912, 819200, 884736
Allocating group tables: done
Writing inode tables: done
Creating journal (32768 blocks): done
Writing superblocks and filesystem accounting information: done
```

Jetzt erstellen wir ein Verzeichnis für das Dateisystem unter Verwendung bereitstellen `mkdir`:

```bash
sudo mkdir /datadrive
```

Und Sie bereitstellen Verzeichnis mit `mount`:

```bash
sudo mount /dev/sdc1 /datadrive
```

Der Datenträger kann nun als verwenden `/datadrive`.

```bash
ls
```

Die Ausgabe

```bash
bin   datadrive  etc   initrd.img  lib64       media  opt   root  sbin  sys  usr  vmlinuz
boot  dev        home  lib         lost+found  mnt    proc  run   srv   tmp  var
```

Um sicherzustellen, dass das Laufwerk automatisch nach einem Neustart erneut bereitgestellt wird müssen sie die Datei/usw./Fstab hinzugefügt werden. Darüber hinaus es wird dringend empfohlen, dass die UUID (universell Unique IDentifier) auf das Laufwerk, sondern nur den Namen des Geräts verweisen in/usw./Fstab verwendet wird (z. B. `/dev/sdc1`). Wenn das Betriebssystem einen Fehler beim Start erkennt, vermieden mithilfe der UUID den falschen Datenträger an einem bestimmten Speicherort bereitgestellt werden. Verbleibende Daten Datenträger würde dann diese derselben Geräte-IDs zugewiesen werden. Verwenden Sie die UUID des neuen Laufwerks, um das Programm **Blkid** :

```bash
sudo -i blkid
```

Die Ausgabe sieht ähnlich wie der folgende aus:

```bash
/dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
/dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
/dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
```

>[AZURE.NOTE] Bearbeiten die **Datei/etc/fstab** nicht ordnungsgemäß ergeben in einem möglicherweise nicht mehr gestartet System. Wenn Sie unsicher sind, lesen Sie die Verteilung der Dokumentation Informationen, wie Sie diese Datei zu bearbeiten. Es wird auch empfohlen, dass vor dem Bearbeiten eine Sicherungskopie der Datei/etc/fstab erstellt wird.

Öffnen Sie als Nächstes **die/etc/fstab** -Datei in einem Text-Editor:

```bash
sudo vi /etc/fstab
```

In diesem Beispiel verwenden wir den UUID-Wert für das neue **/dev/sdc1** Gerät, das in den vorherigen Schritten und die Mountpoint **/datadrive**erstellt wurde. Fügen Sie die folgende Zeile am Ende der **Datei/etc/fstab** aus:

```bash
UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,nofail   1   2
```

>[AZURE.NOTE] Entfernen einen Datenträger später ohne Bearbeitung Fstab könnte den virtuellen Computer neu gestartet. Geben Sie entweder die meisten Verteilung der `nofail` und/oder `nobootwait` Fstab Optionen. Mit diesen Optionen können ein System zum Starten, selbst wenn der Datenträger beim Start Bereitstellung fehlschlägt. Weitere Informationen zu diesen Parametern finden Sie in der Verteilung der Dokumentation.

>[AZURE.NOTE] Die Option **Nofail** wird sichergestellt, dass der virtuellen Computer gestartet wird, selbst wenn das Dateisystem beschädigt ist oder der Datenträger ist beim Start nicht vorhanden. Ohne diese Option können Sie Verhalten auftreten, wie in [Kann nicht SSH zu Linux virtueller Computer aufgrund FSTAB Fehler](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/) beschrieben.


### <a name="trimunmap-support-for-linux-in-azure"></a>TRIM/aufheben Unterstützung für Linux in Azure
Einige Linux Kernels unterstützen TRIM/aufheben Vorgänge aus, um die nicht verwendeten Blöcke auf dem Datenträger zu verwerfen. Dies ist hauptsächlich nützlich in standard-Speicher zu informieren, Azure, die gelöschte Seiten sind nicht mehr gültig und verworfen werden können. Dies kann Kosten sparen, wenn Sie große Dateien erstellen und löschen Sie sie.

Es gibt zwei Methoden zum Kürzen aktivieren in Ihrem Linux VM unterstützen. Wie gewohnt, finden Sie in der der Verteilung empfohlen:

- Verwenden der `discard` bereitstellen Option in `/etc/fstab`, beispielsweise:

        UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,discard   1   2

- Alternativ können Sie Ausführen der `fstrim` Befehl manuell über die Befehlszeile, oder fügen sie Ihrer Crontab regelmäßig ausführen:

    **Ubuntu**

        # sudo apt-get install util-linux
        # sudo fstrim /datadrive

    **RHEL/CentOS**

        # sudo yum install util-linux
        # sudo fstrim /datadrive

## <a name="troubleshooting"></a>Behandlung von Problemen
[AZURE.INCLUDE [virtual-machines-linux-lunzero](../../includes/virtual-machines-linux-lunzero.md)]

## <a name="next-steps"></a>Nächste Schritte

- Denken Sie daran, dass der neue Datenträger nicht für den virtuellen Computer verfügbar ist, wenn es neu gestartet wird, es sei denn, Sie diese Informationen zu Ihrer Datei [Fstab](http://en.wikipedia.org/wiki/Fstab) schreiben.
- Um sicherzustellen, dass Ihre Linux VM richtig konfiguriert ist, überprüfen Sie die Empfehlungen [Optimieren der Leistung Ihrer Linux-Computer](virtual-machines-linux-optimization.md) .
- Erweitern Sie Ihre Speicherkapazität durch Hinzufügen weiterer Festplatten und für zusätzliche Leistung [RAID konfigurieren](virtual-machines-linux-configure-raid.md) .

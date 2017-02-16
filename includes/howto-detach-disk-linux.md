Wenn Sie einen Datenträger, der mit einem virtuellen Computer (virtueller Computer) angeschlossen ist nicht mehr benötigen, können Sie ganz einfach trennen. Dies wird den Datenträger aus dem virtuellen Computer entfernt, jedoch nicht aus Speicher entfernen. Wenn Sie die vorhandenen Daten auf dem Datenträger erneut verwenden möchten, können Sie den gleichen virtuellen Computer oder einem anderen Platzhalter erneut an.  

> [AZURE.NOTE] Ein virtuellen Computers in Azure verwendet die verschiedene Arten von Datenträger - Betriebssystem-Laufwerk, einem lokalen temporären Datenträger und optional Daten Datenträger. Details finden Sie unter [Informationen zum Datenträger und virtueller Festplatten für virtuelle Computer](../articles/virtual-machines/virtual-machines-linux-about-disks-vhds.md). Sie können einen Datenträger Betriebssystem nicht trennen, es sei denn, Sie auch den virtuellen Computer löschen.


## <a name="find-the-disk"></a>Suchen nach der Datenträger

Bevor Sie einen Datenträger eines virtuellen Computers trennen können müssen Sie die Anzahl LUN herauszufinden, also ein Bezeichner für den Datenträger getrennt werden. Gehen Sie dazu folgendermaßen vor:

1.  Öffnen Sie Azure CLI, und [Verbinden Sie Ihre Azure-Abonnement](../articles/xplat-cli-connect.md). Vergewissern Sie sich im Modus Azure Servicemanagement werden (`azure config mode asm`).

2.  Finden Sie heraus, welche Datenträger an Ihre virtuellen Computer, mithilfe von angeschlossen sind `azure vm disk list <virtual-machine-name>`:

        $azure vm disk list UbuntuVM
        info:    Executing command vm disk list
        + Fetching disk images
        + Getting virtual machines
        + Getting VM disks
        data:    Lun  Size(GB)  Blob-Name                         OS
        data:    ---  --------  --------------------------------  -----
        data:         30        ubuntuVM-2645b8030676c8f8.vhd  Linux
        data:    1    10        test.VHD
        data:    0    30        ubuntuVM-76f7ee1ef0f6dddc.vhd
        info:    vm disk list command OK

3.  Beachten Sie die LUN oder die **Zahl logische Einheit** für das Laufwerk, das Sie trennen möchten.

## <a name="remove-operating-system-references-to-the-disk"></a>Entfernen von Betriebssystem Verweise auf dem Datenträger

Vor dem Trennen des Datenträgers aus dem Linux Gast, sollten Sie sicherstellen, dass alle Partitionen auf dem Datenträger nicht verwendet werden. Stellen Sie sicher, dass das Betriebssystem nicht versucht, die sie nach einem Neustart stellen erneut bereit. Schritte rückgängig machen die Konfiguration, die Sie wahrscheinlich Situationen erstellt [Anfügen](../articles/virtual-machines/virtual-machines-linux-classic-attach-disk.md) der Datenträger.

1. Verwenden der `lsscsi` Befehl aus, um die Festplatte Bezeichner zu ermitteln. `lsscsi`kann installiert werden, indem Sie entweder `yum install lsscsi` (Red-hat basiert Verteilung) oder `apt-get install lsscsi` (auf Debian basiert Verteilung). Sie können Datenträger-ID suchen, die Ihnen gesuchte mithilfe der LUN-Nummer. Die Nummer des letzte im Tupel in jeder Zeile wird die LUN. Im folgenden Beispiel ordnet LUN 0 _/dev/sdc_

            ops@TestVM:~$ lsscsi
            [1:0:0:0]    cd/dvd  Msft     Virtual CD/ROM   1.0   /dev/sr0
            [2:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sda
            [3:0:1:0]    disk    Msft     Virtual Disk     1.0   /dev/sdb
            [5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc
            [5:0:0:1]    disk    Msft     Virtual Disk     1.0   /dev/sdd

2. Verwenden Sie `fdisk -l <disk>` Discovery zugeordnet Partitionen der Datenträger getrennt werden.
3. 
            $ Sudo Fdisk -l/Entwicklung/Sdc Datenträger/Entwicklung/Sdc: 1098.4 GB, 1098437885952 Byte, 2145386496 Branchen (Sectors) Einheiten = 1 * 512 = 512 Byte Bereichen Sektor Größe (logische/physische): 512 Byte/512 Byte e/a-Größe (Minimum/optimale): 512 Byte/512 Byte Datenträger beschriften Typ: Dos Datenträger Bezeichner: 0x5a1d2a1a

               Device Boot      Start         End      Blocks   Id  System
            /dev/sdc1            2048  2145386495  1072692224   83  Linux

3. Hängen Sie jede Partition aufgeführt, die für den Datenträger aus. In diesem Beispiel:`$ sudo umount /dev/sdc1`
4. Verwenden der `blkid` Befehl, um die Suche die UUIDs für alle Partitionen

            $ sudo blkid
            /dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
            /dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
            /dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
            /dev/sdd1: UUID="44444444-4b4b-4c4c-4d4d-4e4e4e4e4e4e" TYPE="ext4
            
5. Entfernen Sie Einträge in **der/etc/fstab** -Datei, die mit dem Gerätepfade oder UUIDs verknüpft ist, für alle Partitionen für den Datenträger getrennt werden.  In diesem Beispiel Einträge möglicherweise auf:

        UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults   1   2
oder

        /dev/sdc1   /datadrive   ext4   defaults   1   2


## <a name="detach-the-disk"></a>Trennen Sie den Datenträger

Nachdem Sie der Anzahl der LUN des Datenträgers ermitteln und die Bezüge Betriebssystem entfernt, können Sie diese trennen:

1.  Trennen Sie den ausgewählten Datenträger aus des virtuellen Computers durch Ausführen des Befehls `azure vm disk detach
    <virtual-machine-name> <LUN>`:

        $azure vm disk detach UbuntuVM 0
        info:    Executing command vm disk detach
        + Getting virtual machines
        + Removing Data-Disk
        info:    vm disk detach command OK

2.  Sie können prüfen, ob die Festplatte getrennt haben, indem Sie diesen Befehl ausführen:

        $azure vm disk list UbuntuVM
        info:    Executing command vm disk list
        + Fetching disk images
        + Getting virtual machines
        + Getting VM disks
        data:    Lun  Size(GB)  Blob-Name                         OS
        data:    ---  --------  --------------------------------  -----
        data:         30        ubuntuVM-2645b8030676c8f8.vhd  Linux
        data:    1    10        test.VHD
        info:    vm disk list command OK

Der getrennte Datenträger verbleibt im Speicher, jedoch nicht mehr an einem virtuellen Computer angefügt wird.
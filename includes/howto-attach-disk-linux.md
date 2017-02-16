
Finden Sie unter Weitere Details Datenträger [zu Datenträger und virtueller Festplatten für virtuelle Computer](../articles/virtual-machines/virtual-machines-linux-about-disks-vhds.md).

<a id="attachempty"></a>
## <a name="attach-an-empty-disk"></a>Fügen Sie einen leeren Datenträger

1.  Öffnen Sie Azure CLI, und [Verbinden Sie Ihre Azure-Abonnement](../articles/xplat-cli-connect.md). Vergewissern Sie sich im Modus Azure Servicemanagement werden (`azure config mode asm`).

2.  Geben Sie `azure vm disk attach-new` zu erstellen, und fügen Sie einen neuen Datenträger aus, wie im folgenden Beispiel. Ersetzen Sie _TestVM_ durch den Namen Ihres Linux virtuellen Computers aus, und geben Sie die Größe des Datenträgers in GB, also 100 GB in diesem Beispiel:

        azure vm disk attach-new TestVM 100

3.  Nachdem der Daten Datenträger erstellt und verbunden ist, in der Ausgabe aufgeführt `azure vm disk list <virtual-machine-name>` wie im folgenden Beispiel:

        $ azure vm disk list TestVM
        info:    Executing command vm disk list
        + Fetching disk images
        + Getting virtual machines
        + Getting VM disks
        data:    Lun  Size(GB)  Blob-Name                         OS
        data:    ---  --------  --------------------------------  -----
        data:         30        TestVM-2645b8030676c8f8.vhd  Linux
        data:    0    100       TestVM-76f7ee1ef0f6dddc.vhd
        info:    vm disk list command OK

<a id="attachexisting"></a>
## <a name="attach-an-existing-disk"></a>Fügen Sie einen vorhandenen Datenträger

Anfügen eines vorhandenen Datenträgers erfordert, dass Sie eine VHD verfügbar in einem Speicherkonto verfügen.

1.  Öffnen Sie Azure CLI, und [Verbinden Sie Ihre Azure-Abonnement](../articles/xplat-cli-connect.md). Vergewissern Sie sich im Modus Azure Servicemanagement werden (`azure config mode asm`).

2.  Überprüfen Sie, wenn die virtuelle Festplatte angefügt werden soll, bereits auf Ihr Abonnement Azure hochgeladen wird:

        $azure vm disk list
        info:    Executing command vm disk list
        + Fetching disk images
        data:    Name                                          OS
        data:    --------------------------------------------  -----
        data:    myTestVhd                                     Linux
        data:    TestVM-ubuntuVMasm-0-201508060029150744  Linux
        data:    TestVM-ubuntuVMasm-0-201508060040530369
        info:    vm disk list command OK

3.  Wenn Sie den Datenträger nicht, die Sie verwenden möchten finden, können Sie eine lokale virtuellen für Ihr Abonnement mithilfe hochladen `azure vm disk create` oder `azure vm disk upload`. Beispiel für `disk create` wäre wie im folgenden dargestellt:

        $azure vm disk create myTestVhd2 .\TempDisk\test.VHD -l "East US" -o Linux
        info:    Executing command vm disk create
        + Retrieving storage accounts
        info:    VHD size : 10 GB
        info:    Uploading 10485760.5 KB
        Requested:100.0% Completed:100.0% Running:   0 Time:   25s Speed:    82 KB/s
        info:    Finishing computing MD5 hash, 16% is complete.
        info:    https://mystorageaccount.blob.core.windows.net/disks/test.VHD was
        uploaded successfully
        info:    vm disk create command OK

    Sie können auch `azure vm disk upload` eine virtuelle Festplatte mit einem bestimmten Speicherkonto hochladen. Lesen Sie mehr über die Befehle zum Verwalten Ihrer Azure-virtuellen Computern Daten Datenträger [hier](virtual-machines-command-line-tools.md#commands-to-manage-your-azure-virtual-machine-data-disks).

4.  Jetzt fügen Sie die gewünschte virtuelle Festplatte an Ihre virtuellen Computern an:

        $azure vm disk attach TestVM myTestVhd
        info:    Executing command vm disk attach
        + Getting virtual machines
        + Adding Data-Disk
        info:    vm disk attach command OK

    Vergewissern Sie sich mit dem Namen von virtuellen Computern und _MyTestVhd_ mit der gewünschten virtuelle Festplatte _TestVM_ zu ersetzen.

5.  Sie können überprüfen, ob der Datenträger an den virtuellen Computer mit angefügt wird `azure vm disk list <virtual-machine-name>`:

        $azure vm disk list TestVM
        info:    Executing command vm disk list
        + Fetching disk images
        + Getting virtual machines
        + Getting VM disks
        data:    Lun  Size(GB)  Blob-Name                         OS
        data:    ---  --------  --------------------------------  -----
        data:         30        TestVM-2645b8030676c8f8.vhd  Linux
        data:    1    10        test.VHD
        data:    0    100        TestVM-76f7ee1ef0f6dddc.vhd
        info:    vm disk list command OK


> [AZURE.NOTE]
> Nachdem Sie einen Datenträger hinzugefügt haben, müssen Sie melden Sie sich bei der virtuellen Computern und Initialisierung des Datenträgers, damit des virtuellen Computers den Datenträger für Speicher verwenden können (siehe die nachfolgenden Schritte für Weitere Informationen zur Vorgehensweise).

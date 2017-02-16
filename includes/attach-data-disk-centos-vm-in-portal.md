1. Azure- [Verwaltungsportal](http://manage.windowsazure.com)klicken Sie auf **virtuellen Computern** , und wählen Sie die soeben erstellte (**Testlinuxvm**) virtuellen Computern.

2. Klicken Sie auf der Befehlsleiste klicken Sie auf **Einfügen** , und klicken Sie dann auf **Leeren Datenträger anfügen**.

    Klicken Sie im Dialogfeld **Anfügen leeren Datenträger** wird angezeigt.


3. Der **Name des virtuellen Computers**, **Speicherort**und **Dateinamen** sind bereits für Sie definiert. Sie müssen lediglich die Größe eingeben, die für den Datenträger werden soll. Geben Sie im Feld **Größe** **5** .

    ![Anfügen von leeren Datenträger][Image2]

    **Hinweis:** Alle Datenträger werden aus einer VHD-Datei in Azure-Speicher erstellt. Sie können Geben Sie einen Namen für die VHD-Datei, die Speicher hinzugefügt wird, aber Azure den Namen des Datenträgers automatisch generiert.

4. Klicken Sie auf das Häkchen, um die Daten Datenträger des virtuellen Computers installieren.

5. Klicken Sie auf den Namen des virtuellen Computers auf das Dashboard anzeigen, damit Sie überprüfen können, ob der Daten Datenträger des virtuellen Computers erfolgreich zugeordnet wurde. Der Datenträger, den Sie angefügt wird in der Tabelle **Datenträger** aufgeführt.

    Wenn Sie einen Datenträger angefügt wird, ist es nicht zur Verfügung, bis Sie anmelden, um die Einrichtung abzuschließen.

##<a name="connect-to-the-virtual-machine-using-ssh-or-putty-and-complete-setup"></a>Herstellen einer Verbindung mithilfe von SSH virtuellen Computers mit oder kitten und Setup abgeschlossen
Melden Sie sich bei Setup Abschließen des Datenträgers des virtuellen Computers, damit Sie es verwenden können, um Daten zu speichern.

1. Nachdem der virtuellen Computern bereitgestellt ist, verbinden Sie mit SSH oder kitten und melden Sie sich als **Neuer Benutzer** (wie in den vorstehenden Schritten beschrieben). 


2. Klicken Sie im Fenster SSH oder kitten Geben Sie den folgenden Befehl ein, und geben Sie dann das Kontokennwort:

    `$ sudo grep SCSI /var/log/messages`

    Sie können suchen, dass die Kennung für den letzten Daten Datenträger, der in die Nachrichten hinzugefügt wurde, die sind (**Sdc**, in diesem Beispiel) angezeigt.

    ![GREP][Image4]


3. Geben Sie in der SSH oder PuTTY Fenster den Datenträger **/dev/sdc**unterteilen den folgenden Befehl aus:

    `$ sudo fdisk /dev/sdc`


4. Geben Sie **n** , um eine neue Partition zu erstellen.

    ![FDISK][Image5]


5. Typ **p** , um der Partition die primäre Partition, geben Sie **1** nehmen Sie die erste Partition, und geben Sie dann EINGABETASTE, um den Standardwert (1) für die Zylinder-übernehmen.

    ![FDISK][Image6]


6. Geben Sie **p** , um die Details zu den Datenträger anzeigen, die aufgeteilt werden ist.

    ![FDISK][Image7]


7. Geben Sie **w** , um die Einstellungen für den Datenträger schreiben.

    ![FDISK][Image8]


8. Formatieren Sie den neuen Datenträger mit dem Befehl **Mkfs** an:

    `$ sudo mkfs -t ext4 /dev/sdc1`

9. Als Nächstes müssen Sie ein Verzeichnis für das neue Dateisystem geladen haben. Geben Sie den folgenden Befehl aus, um ein neues Verzeichnis für das Laufwerk bereitstellen zu gestalten, und geben Sie das Kontokennwort, beispielsweise:

    `sudo mkdir /datadrive`


10. Geben Sie den folgenden Befehl aus, um das Laufwerk bereitstellen:

    `sudo mount /dev/sdc1 /datadrive`

    Der Datenträger kann nun als **/datadrive**verwenden.


11. Fügen Sie das neue Laufwerk/etc/fstab hinzu:

    Um sicherzustellen, dass das Laufwerk erneut bereitgestellt automatisch nach einem Neustart wird müssen sie die Datei/etc/fstab hinzugefügt werden. Darüber hinaus wird dringend empfohlen, dass die UUID (universell Unique IDentifier) in/etc/fstab verwendet wird, um auf das Laufwerk, sondern nur den Namen des Geräts (d. h. /dev/sdc1) verweisen. Um die UUID des neuen Laufwerks finden, können Sie das Programm **Blkid** verwenden:
    
        `sudo -i blkid`

    Die Ausgabe wird ähnlich wie der folgende aus:

        `/dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"`
        `/dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"`
        `/dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"`

    >[AZURE.NOTE] Blkid möglicherweise keine Sudo Access in allen Fällen erforderlich, allerdings ist es möglicherweise leichter zu mit ausführen `sudo -i` auf einige Verteilung liegen sbin oder/Usr/Sbin nicht in Ihrer `$PATH`.

    **Vorsicht:** Bearbeiten die Datei/etc/fstab nicht ordnungsgemäß ergeben in einem möglicherweise nicht mehr gestartet System. Wenn Sie unsicher sind, finden Sie in der Verteilung der Dokumentation Informationen, wie Sie diese Datei zu bearbeiten. Es wird auch empfohlen, dass vor dem Bearbeiten eine Sicherungskopie der Datei/etc/fstab erstellt wird.

    Verwenden einen Text-Editor, geben Sie die Informationen über das neue Dateisystem am Ende der Datei/etc/fstab.  In diesem Beispiel werden wir UUID-Wert für das neue **/dev/sdc1** Gerät verwenden, die in den vorherigen Schritten und die Mountpoint **/datadrive**erstellt wurde:

        `UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults   1   2`

    Wenn zusätzliche Laufwerke oder Partitionen erstellt haben, werden Sie diese in/usw./Fstab ebenfalls separat eingeben müssen.

    Sie können nun testen, die das Dateisystem ordnungsgemäß bereitgestellt ist, indem Sie einfach Aufheben der Bereitstellung, und klicken Sie dann erneut das Dateisystem laden, d. h. verwenden das Beispiel Punkt bereitstellen `/datadrive` in früheren Schritten erstellt: 

        `sudo umount /datadrive`
        `sudo mount /datadrive`

    Wenn der zweite Befehl einen Fehler erzeugt, aktivieren Sie die Datei/etc/fstab auf die richtige Syntax.


    >[AZURE.NOTE] Entfernen anschließend einen Datenträger ohne Fstab bearbeiten könnte den virtuellen Computer neu gestartet. Dies ist häufig auftreten, und geben Sie entweder die meisten Verteilung der `nofail` und/oder `nobootwait` Fstab Optionen, mit denen werden ein System zum Starten, selbst wenn der Datenträger nicht vorhanden ist. Weitere Informationen zu diesen Parametern finden Sie in der Verteilung der Dokumentation.


[Image2]: ./media/attach-data-disk-centos-vm-in-portal/AttachDataDiskLinuxVM2.png
[Image4]: ./media/attach-data-disk-centos-vm-in-portal/GrepScsiMessages.png
[Image5]: ./media/attach-data-disk-centos-vm-in-portal/fdisk1.png
[Image6]: ./media/attach-data-disk-centos-vm-in-portal/fdisk2.png
[Image7]: ./media/attach-data-disk-centos-vm-in-portal/fdisk3.png
[Image8]: ./media/attach-data-disk-centos-vm-in-portal/fdisk4.png
[Image9]: ./media/attach-data-disk-centos-vm-in-portal/mkfs.png


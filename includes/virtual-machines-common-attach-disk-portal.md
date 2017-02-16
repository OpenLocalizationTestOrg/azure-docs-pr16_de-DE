


## <a name="find-the-virtual-machine"></a>Suchen des virtuellen Computers

1. Melden Sie sich mit dem [Azure-Portal](https://portal.azure.com/)aus.

2. Klicken Sie im Menü Hub auf **virtuellen Computern**.

3.  Wählen Sie den virtuellen Computer aus der Liste aus.

4. Klicken Sie auf virtuellen Computern vorher, in **Essentials** **Festplatten**.

    ![Öffnen Sie die Einstellungen der Datenträger](./media/virtual-machines-common-attach-disk-portal/find-disk-settings.png)

Durch folgenden Anweisungen für Anfügen entweder eine neue oder eine vorhandene Datenträger fortsetzen.

## <a name="option-1-attach-a-new-disk"></a>Option 1: Fügen Sie einen neuen Datenträger

1.  Klicken Sie auf dem **Datenträger** Blade auf **neue anfügen**.

2.  Überprüfen Sie die Standardeinstellungen, nach Bedarf aktualisieren Sie und dann auf **OK**.

    ![Überprüfen der Datenträger-Einstellungen](./media/virtual-machines-common-attach-disk-portal/attach-new.png)

3.  Nachdem Azure den Datenträger erstellt und an den virtuellen Computer angefügt, wird der neue Datenträger in des virtuellen Computers Datenträger Einstellungen unter **Daten Datenträger**aufgeführt.

## <a name="option-2-attach-an-existing-disk"></a>Option 2: Fügen Sie einen vorhandenen Datenträger

1.  Klicken Sie auf dem **Datenträger** Blade auf **vorhandene anfügen**.

2.  Klicken Sie unter **vorhandene Datenträger anfügen**klicken Sie auf **Datei virtuelle Festplatte**.

    ![Anfügen von vorhandenen Datenträger](./media/virtual-machines-common-attach-disk-portal/attach-existing.png)

3.  Wählen Sie unter **Speicher-Konten**das Konto und Container, auf die VHD-Datei ein.

    ![Suchen nach Position virtuelle Festplatte](./media/virtual-machines-common-attach-disk-portal/find-storage-container.png)

4.  Wählen Sie die VHD-Datei ein.

5.  Klicken Sie unter **Anfügen vorhandenen Datenträger**wird die soeben ausgewählte Datei unter **Virtuelle Festplatte Datei**aufgeführt. Klicken Sie auf **OK**.

6.  Nachdem Azure den Datenträger virtuellen Computer hängt, wird es in des virtuellen Computers Datenträger Einstellungen unter **Daten Datenträger**aufgeführt.





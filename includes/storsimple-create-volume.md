<!--author=SharS last changed: 02/04/2016-->

#### <a name="to-create-a-volume"></a>So erstellen Sie einen Datenträger

1. Klicken Sie auf der Seite Geräte für **Schnellstart** klicken Sie auf **Hinzufügen eines Datenträgers**. Hierdurch wird den Lautstärke Assistenten zum Hinzufügen von.

2. Hinzufügen eines Assistenten Lautstärke, klicken Sie unter **Grundlegende Einstellungen**führen Sie folgende Schritte aus:
   1. Geben Sie einen **Namen** für die Lautstärke ein.
   2. Geben Sie die **Kapazität nach der Bereitstellung** für die Lautstärke in GB oder TB. Die Lautstärke Kapazität muss zwischen 1 GB und 64 TB für ein physischen Gerät.
   3. Wählen Sie in der Dropdown-Liste den **Typ der Verwendung** für die Lautstärke ein. 
   4. Wenn Sie diesem Handbuch für archivierte Daten verwenden, wählen Sie das Kontrollkästchen **mit diesem Handbuch für weniger häufig verwendeter Archivierung Daten** . Wählen Sie für alle anderen verwenden Fälle einfach **Gestuft Lautstärke**an. (Gestufte Datenmengen wurden zuvor primäre Datenmengen genannt).

        ![Hinzufügen der Lautstärke](./media/storsimple-create-volume/ScreenshotUpdate1VolumeFlow.png)

    4. Klicken Sie auf das Pfeilsymbol ![Pfeil-Symbol](./media/storsimple-create-volume/HCS_ArrowIcon-include.png) um zur nächsten Seite zu wechseln.

3. Klicken Sie im Dialogfeld **Weitere Einstellungen** Hinzufügen eines neuen Datensatzes für Access-Steuerelement (ACR):
   1. Geben Sie einen **Namen** für Ihre ACR ein.
   2. Geben Sie unter **iSCSI Initiatornamen**, die iSCSI qualifizierte Namen (IQN) von Ihrem Windows-Host. Wenn Sie mit der IQN besitzen, wechseln Sie zum [Abrufen der IQN eines Windows Server-Hosts](#get-the-iqn-of-a-windows-server-host).
   3. Es empfiehlt sich, dass Sie eine Sicherungskopie der standardmäßigen aktivieren, indem Sie **eine Sicherungskopie der Standardwert für dieses Volume aktivieren** das Kontrollkästchen. Die Standardeinstellungen für die Sicherung wird eine Richtlinie erstellen, die ausgeführt wird, um jeden Tag (Gerätezeit) 22:30 und erstellt eine Momentaufnahme Cloud von diesem Handbuch.

        > [AZURE.NOTE] Nachdem die Sicherung hier aktiviert ist, kann es nicht wiederhergestellt werden. Sie benötigen, um die Lautstärke zur Änderung dieser Einstellung zu bearbeiten.

        ![Hinzufügen der Lautstärke](./media/storsimple-create-volume/AddVolume2-include.png)

4. Klicken Sie auf das Kontrollkästchen-Symbol ![Aktivieren Sie Symbol](./media/storsimple-create-volume/HCS_CheckIcon-include.png). Ein Datenträger wird mit der angegebenen Einstellungen erstellt werden.

![Video verfügbar](./media/storsimple-create-volume/Video_icon.png) **Video verfügbar**

Um ein Video zur Verfügung, die veranschaulicht, wie ein StorSimple Volume zu erstellen, klicken Sie auf [hier](https://azure.microsoft.com/documentation/videos/create-a-storsimple-volume/).


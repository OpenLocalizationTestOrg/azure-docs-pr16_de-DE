<!--author=SharS last changed: 9/17/15-->

#### <a name="to-install-regular-hotfixes-via-windows-powershell-for-storsimple"></a>Zum Installieren von regulären Updates über Windows PowerShell für StorSimple

1. Verbinden Sie mit dem Gerät serielle Konsole. Weitere Informationen finden Sie unter [Schritt 1: Herstellen einer Verbindung die serielle Konsole mit](storsimple-update-device.md#step1).

2. Wählen Sie im Menü seriellen Konsole Option 1, **Melden Sie sich mit Vollzugriff**aus. Geben Sie das Kennwort ein. Das standardmäßige Kennwort lautet **Kennwort1**.

3. Geben Sie an der Befehlszeile ein:

    `Start-HcsHotfix`

       >[AZURE.IMPORTANT]
       >
       >- Dieser Befehl gilt nur für normale Updates. Führen Sie diesen Befehl auf nur ein Controller, aber beide Controller aktualisiert.
       >- Sie können eine Controller während des Aktualisierungsvorgangs bemerkbar; Das Failover wirkt jedoch nicht System Verfügbarkeit oder Vorgang.

4. Wenn Sie dazu aufgefordert werden, geben Sie den Pfad zu dem freigegebenen Netzwerkordner, der die Updatedateien enthält.

5. Sie werden zur Bestätigung aufgefordert werden. Geben Sie **Y** , um die Update-Installation fortzusetzen.

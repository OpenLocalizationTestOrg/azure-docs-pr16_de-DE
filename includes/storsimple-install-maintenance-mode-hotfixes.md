<!--author=SharS last changed: 9/17/15-->

#### <a name="to-install-maintenance-mode-hotfixes-via-windows-powershell-for-storsimple"></a>Zum Installieren von Wartung Modus Updates über Windows PowerShell für StorSimple

> [AZURE.IMPORTANT] Im Modus Wartung müssen Sie das Update zuerst auf einen Controller und klicken Sie dann auf dem anderen Controller anwenden.

1. Platzieren Sie das Gerät in Wartungsmodus. Finden Sie unter [Schritt2: Geben Sie Wartung Modus](storsimple-update-device.md#step2) Anweisungen zum Wartung des Lesemodus.

2. Wenn Sie das Update anwenden möchten, geben Sie Folgendes ein:

     `Start-HcsHotfix` 

3. Wenn Sie dazu aufgefordert werden, geben Sie den Pfad zu dem freigegebenen Netzwerkordner, der die Updatedateien enthält.

4. Sie werden zur Bestätigung aufgefordert werden. Geben Sie **Y** , um die Update-Installation fortzusetzen.

5. Nachdem Sie das Update auf einen Controller angewendet haben, melden Sie sich an den anderen Controller. Deshalb sollten Sie als für den vorherigen Controller Meinten.

6. Nachdem die Updates angewendet wurden, beenden Sie Wartung-Modus. Finden Sie unter [Schritt 4: Wartung Beenden des Modus](storsimple-update-device.md#step4) Anweisungen.

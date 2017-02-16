<!--author=SharS last changed: 9/17/15-->

#### <a name="to-install-maintenance-mode-updates-via-windows-powershell-for-storsimple"></a>So installieren Sie die Wartung Modus Updates über Windows PowerShell für StorSimple

1. Wenn Sie dies noch nicht erfolgt Zugriff auf das Gerät serielle Konsole, und wählen Sie Option 1, **Melden Sie sich mit Vollzugriff**aus. 

2. Geben Sie das Kennwort ein. Das standardmäßige Kennwort lautet **Kennwort1**.

3. Geben Sie an der Befehlszeile ein:

     `Get-HcsUpdateAvailability` 
    
4. Sie werden benachrichtigt, wenn Updates verfügbar sind, und ob die Updates Unterbrechung oder ohne Unterbrechung sind. Um die Unterbrechung Updates anwenden möchten, müssen Sie das Gerät in Wartung versetzt werden soll. Finden Sie unter [Schritt2: Geben Sie Wartung Modus](storsimple-update-device.md#step2) Anweisungen.

5. Bei Ihrem Gerät im Modus Wartung, an der Eingabeaufforderung Typ:`Start-HcsUpdate`

6. Sie werden zur Bestätigung aufgefordert werden. Nachdem Sie die Updates bestätigen, werden sie auf dem Controller installiert werden, die Sie zurzeit zugreifen. Nachdem die Updates installiert wurden, wird der Controller neu gestartet. 

7. Überwachen Sie den Status der Updates. Type:

    `Get-HcsUpdateStatus`
    
    Wenn die `RunInProgress` ist `True`, wird das Update noch ausgeführt. Wenn `RunInProgress` ist `False`, es gibt an, dass die Aktualisierung abgeschlossen ist.  

7. Wenn das Update auf den aktuellen Controller installiert ist, und es wurde neu gestartet, mit der andere Controller verbinden Sie, und führen Sie die Schritte 1 bis 6.

8. Nachdem beide Controller aktualisiert wurden, beenden Sie die Wartungsmodus. Finden Sie unter [Schritt 4: Wartung Beenden des Modus](storsimple-update-device.md#step4) Anweisungen.

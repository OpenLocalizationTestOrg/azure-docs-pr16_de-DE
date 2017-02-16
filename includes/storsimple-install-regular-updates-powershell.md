<!--author=SharS last changed: 9/17/15-->

#### <a name="to-install-regular-updates-via-windows-powershell-for-storsimple"></a>So installieren Sie regelmäßige Updates über Windows PowerShell für StorSimple

1. Öffnen Sie die serielle Gerät-Verwaltungskonsole, und wählen Sie die Option 1, **Melden Sie sich mit Vollzugriff**. Geben Sie das Kennwort ein. Das standardmäßige Kennwort lautet *Kennwort1*. 

2. Geben Sie an der Befehlszeile ein:

     `Get-HcsUpdateAvailability`
    
    Sie werden benachrichtigt, wenn Updates verfügbar sind, und ob die Updates Unterbrechung oder ohne Unterbrechung sind.

3. Geben Sie an der Befehlszeile ein:

     `Start-HcsUpdate`

    Aktualisierungsvorgang wird gestartet.

> [AZURE.IMPORTANT]
>
> - Dieser Befehl gilt nur für regelmäßige Updates. Führen Sie diesen Befehl auf nur ein Controller, aber beide Controller aktualisiert. 
> - Sie können eine Controller während des Aktualisierungsvorgangs bemerkbar; Das Failover wirkt jedoch nicht System Verfügbarkeit oder Vorgang.

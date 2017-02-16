<!--author=SharS last changed: 12/01/15-->

#### <a name="to-enter-maintenance-mode"></a>Wartung-Modus eingeben

1. Wählen Sie im Menü seriellen Konsole Option 1, **Melden Sie sich mit Vollzugriff**aus.

2. Geben Sie das Kennwort ein. Das standardmäßige Kennwort lautet **Kennwort1**.

3. Geben Sie an der Befehlszeile

     `Enter-HcsMaintenanceMode`

4. Eine Warnung angezeigt, die besagt, dass die Wartungsmodus unterbrochen werden alle e/a-Anfragen und trennt die Verbindung zum klassischen Azure-Portal wird und Sie werden zur Bestätigung aufgefordert wird angezeigt. Geben Sie **Y** Wartungsmodus eingeben.

    Beide Controller werden neu gestartet. Wenn der Neustart abgeschlossen ist, wird eine Meldung angezeigt, die angibt, dass das Gerät Wartung-Modus ist.

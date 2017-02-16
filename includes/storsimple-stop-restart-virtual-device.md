#### <a name="to-stop-and-start-a-virtual-device"></a>Zum Beenden und starten ein virtuelles Gerät
Wenn ein virtuelles Gerät beenden möchten, klicken Sie auf seinen Namen, und klicken Sie dann auf **Beenden**. Während das virtuelle Gerät beendet wird, wird der Status **Beenden**. Nachdem das virtuelle Gerät beendet ist, ist der Status **weiterspielen**.

Verwenden Sie die folgenden Cmdlets zum Beenden und starten ein virtuelles Gerät aus.

`Stop-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`


`Start-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`
    
#### <a name="to-restart-a-virtual-device"></a>Ein virtuelles Gerät neu starten.

Wenn ein virtuelles Gerät ausgeführt wird und Sie ihn erneut starten möchten, klicken Sie auf seinen Namen, und klicken Sie dann auf **neu starten**. Während das virtuelle Gerät neu gestartet wurde, wird der Status **neu gestartet**. Virtuelle Gerät wird für Ihre Verwendung bereit sind, der Status **ausgeführt**.

Verwenden Sie das folgende Cmdlet ein virtuelles Gerät neu starten.

`Restart-AzureVM -ServiceName "MyStorSimpleservice1" -Name "MyStorSimpleDevice"`





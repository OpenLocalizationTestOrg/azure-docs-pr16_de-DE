## <a name="create-an-iot-hub"></a>Erstellen Sie einen IoT-hub

Erstellen Sie einen IoT-Hub für die Verbindung zu Ihrem simulierten Gerät an. Die folgenden Schritte zeigen Sie zum Ausführen dieser Aufgabe mithilfe des Azure-Portals.

1. Melden Sie sich bei der [Azure-Portal][lnk-portal].

2. Klicken Sie auf **neu**, in der Jumpbar > **Internet der Dinge** > **Azure IoT Hub**.

    ![Azure-Portal Jumpbar][1]

3. Wählen Sie die Konfiguration für Ihre IoT Hub das Blade **IoT Hub** aus.

    ![IoT Hub blade][2]

    * Geben Sie im Feld **Name** einen Namen für Ihre IoT Hub aus. Wenn der **Name** gültig und verfügbar ist, wird ein grünes Häkchen im Feld **Name** angezeigt.
    * Wählen Sie eine [Preise und Maßstab Ebene][lnk-pricing]. In diesem Lernprogramm erfordert eine bestimmte Ebene keine. In diesem Lernprogramm verwenden Sie die kostenlose F1 Ebene aus.
    * Erstellen einer neuen Ressourcengruppe in **Ressourcengruppe**oder ein vorhandenes Layout auszuwählen. Weitere Informationen finden Sie unter [Verwenden von Ressourcengruppen zum Verwalten Ihrer Azure Ressourcen][lnk-resource-groups].
    * Wählen Sie in den **Speicherort**den Speicherort für Ihre IoT Hub hosten aus. Wählen Sie in diesem Lernprogramm Ihre nächste Speicherort aus.

4. Wenn Sie Ihre IoT Hub Konfigurationsoptionen ausgewählt haben, klicken Sie auf **Erstellen**.  Es dauert ein paar Augenblicke Azure Ihrer IoT-Hub zu erstellen. Um den Status zu überprüfen, können Sie den Fortschritt der Startboard oder in der Systemsteuerung Benachrichtigungen überwachen.

    ![Neue IoT Hub status][3]

5. Wenn der Hub IoT erfolgreich erstellt wurde, klicken Sie auf die neue Kachel für Ihre IoT Hub im Portal Azure, um das Blade für den neuen IoT Hub zu öffnen. Notieren Sie die **Hostname**, und klicken Sie dann auf **freigegebenen-Richtlinien**.

    ![Neue IoT Hub blade][4]

6. Klicken Sie auf die Richtlinie **Iothubowner** in das Blade **freigegeben-Richtlinien** kopieren Sie und notieren Sie die Verbindungszeichenfolge in das Blade **Iothubowner** . Weitere Informationen finden Sie unter [Access Control] [ lnk-access-control] in "Azure IoT Hub Developer Guide."

    ![Freigegebene Access Richtlinien blade][5]


<!-- Images. -->
[1]: ./media/iot-hub-get-started-create-hub/create-iot-hub1.png
[2]: ./media/iot-hub-get-started-create-hub/create-iot-hub2.png
[3]: ./media/iot-hub-get-started-create-hub/create-iot-hub3.png
[4]: ./media/iot-hub-get-started-create-hub/create-iot-hub4.png
[5]: ./media/iot-hub-get-started-create-hub/create-iot-hub5.png

<!-- Links -->
[lnk-resource-groups]: ../articles/azure-portal/resource-group-portal.md
[lnk-portal]: https://portal.azure.com/
[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub/
[lnk-access-control]: ../articles/iot-hub/iot-hub-devguide-security.md

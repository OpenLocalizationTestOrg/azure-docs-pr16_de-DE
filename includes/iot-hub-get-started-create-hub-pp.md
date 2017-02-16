## <a name="create-a-device-management-enabled-iot-hub"></a>Erstellen Sie ein Gerät Management aktiviert IoT-Hub

Da IoT Hub Gerätemanagement in der Vorschau ist, müssen Sie einen Gerät Management aktiviert IoT Hub zu erstellen. Wenn IoT Hub Gerätemanagement allgemeine Verfügbarkeit erreicht, werden in diesem Lernprogramm aktualisiert. Die folgenden Schritte zeigen Sie zum Ausführen dieser Aufgabe mithilfe des Azure-Portals.

1.  Melden Sie sich mit dem [Azure-Portal]aus.
2.  Klicken Sie in der Jumpbar auf **neu**, und klicken Sie dann klicken Sie auf **Internet der Dinge**, und klicken Sie dann auf **Azure IoT Hub**.

    ![][img-new-hub]

3.  Wählen Sie die Konfiguration für Ihre IoT Hub das Blade **IoT Hub** aus.

    ![][img-configure-hub]

  -   Geben Sie im Feld **Name** einen Namen für Ihre IoT Hub aus. Wenn der **Name** gültig und verfügbar ist, wird ein grünes Häkchen im Feld **Name** angezeigt.
  -   Wählen Sie eine **Preise und Maßstab Ebene**aus. In diesem Lernprogramm erfordert eine bestimmte Ebene keine.
  -   Erstellen einer neuen Ressourcengruppe in **Ressourcengruppe**oder ein vorhandenes Layout auszuwählen. Weitere Informationen finden Sie unter [Verwenden von Ressourcengruppen zum Verwalten Ihrer Azure Ressourcen].
  -   Aktivieren Sie das Kontrollkästchen zu **Aktivieren Device Management - Vorschau**.
  -   Wählen Sie in den **Speicherort**den Speicherort für Ihre IoT Hub hosten aus. IoT Hub Gerätemanagement ist nur verfügbar in ostasiatischen US, North Europe und Ostasien während public Preview-Version.

    > [AZURE.NOTE]Wenn Sie das Kontrollkästchen **Aktivieren Device Management**nicht aktivieren, funktionieren nicht in den Beispielen.<br/>Überprüfen Sie **Device Management aktivieren**, erstellen Sie eine Vorschau auf die IOT Hub unterstützt nur in ostasiatischen US, North Europa und Ostasien und nicht für die Herstellung Szenarien vorgesehen. Sie können keine Geräte in die und aus Gerät Management aktiviert Hubs migrieren.

4.  Wenn Sie Ihre IoT Hub Konfigurationsoptionen ausgewählt haben, klicken Sie auf **Erstellen**. Es dauert ein paar Augenblicke Azure Ihrer IoT-Hub zu erstellen. Um den Status zu überprüfen, können Sie den Fortschritt der **Startboard** oder in der Systemsteuerung **Benachrichtigungen** überwachen.

    ![][img-monitor]

5.  Wenn der IoT Hub erfolgreich erstellt wurde, wird das Blade für Ihre Hub automatisch geöffnet. Notieren Sie die **Hostname**, und klicken Sie dann auf **freigegebenen-Richtlinien**.

    ![][img-keys]

6.  Klicken Sie auf die Richtlinie **Iothubowner** , und klicken Sie dann kopieren Sie, und notieren Sie die Verbindungszeichenfolge in das Blade **Iothubowner** . Kopieren Sie sie an einem Speicherort, den Sie später zugreifen können, da Sie es zum Bearbeiten dieses Lernprogramms benötigen.

    > [AZURE.NOTE] Herstellung Szenarien Vergewissern Sie sich, mit den **Iothubowner** Anmeldeinformationen nicht.

    ![][img-connection]

Sie haben nun ein Gerät erstellt Management aktiviert IoT Hub. Sie benötigen die Verbindungszeichenfolge zum Bearbeiten dieses Lernprogramms.

## <a name="create-a-device-identity"></a>Erstellen Sie eine Gerät Identität

In diesem Abschnitt verwenden Sie das Tool Knoten [IoT Hub Explorer] [ iot-hub-explorer] eine Gerät Identität in diesem Lernprogramm erstellen.

1. Führen Sie in Ihrer Umgebung Befehlszeile Folgendes ein:

    Npm installieren -giothub-explorer@latest

2. Führen Sie dann den folgenden Befehl zum Anmelden bei Ihrem Hub zu ersetzenden erinnern `{service connection string}` mit der IoT Hub Verbindungszeichenfolge, die Sie zuvor kopiert haben:

    Explorer-Iothub Login "{Dienst Verbindungszeichenfolge}"

3. Erstellen Sie eine neue Gerät Identität aufgerufen schließlich `myDeviceId` mit dem Befehl:

    Explorer-Iothub erstellen MyDeviceId – Verbindungszeichenfolge

Notieren Sie die Verbindungszeichenfolge Gerät aus dem Ergebnis. Diese Verbindungszeichenfolge wird von der app Gerät für die Verbindung zu Ihrem IoT Hub als ein Gerät verwendet.

![][img-identity]

Finden Sie unter [Erste Schritte mit IoT Hub] [ lnk-getstarted] für eine Möglichkeit zum Gerät Identitäten programmgesteuert zu erstellen.

<!-- images and links -->
[img-new-hub]: media/iot-hub-get-started-create-hub-pp/image1.png
[img-configure-hub]: media/iot-hub-get-started-create-hub-pp/image2.png
[img-monitor]: media/iot-hub-get-started-create-hub-pp/image3.png
[img-keys]: media/iot-hub-get-started-create-hub-pp/image4.png
[img-connection]: media/iot-hub-get-started-create-hub-pp/image5.png
[img-identity]: media/iot-hub-get-started-create-hub-pp/devidentity.png

[Azure-portal]: https://portal.azure.com/
[iot-hub-explorer]: https://github.com/Azure/azure-iot-sdks/tree/master/tools/iothub-explorer

[lnk-getstarted]: ../articles/iot-hub/iot-hub-csharp-csharp-getstarted.md
[Ressource Entwurfsphase Azure Ressourcen verwalten]: ../articles/azure-portal/resource-group-portal.md

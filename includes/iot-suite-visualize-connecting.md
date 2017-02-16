## <a name="view-device-telemetry-in-the-dashboard"></a>Ansicht Gerät werden im dashboard

Das Dashboard in die Überwachung remote-Lösung können Sie die telemetrieprotokoll anzeigen, die Ihren Geräten IoT Hub senden.

1. In Ihrem Browser zum remote Überwachung Lösung Dashboard zurückzukehren Sie, klicken Sie im linken Bereich der **Geräteliste**Navigieren auf **Geräte** .

2. Klicken Sie in der **Geräteliste**sollte angezeigt werden, dass der Status des Geräts jetzt **aktiv**ist.

    ![][18]

3. Klicken Sie auf **Dashboard** , um zum Dashboard zurückzukehren, wählen Ihr Gerät in das **Gerät zur Ansicht** Dropdown-seine werden anzeigen. Die werden aus der Stichprobe Anwendung ist 50 Einheiten für interne Temperatur, 55 Einheiten für externe Temperaturen und 50 Einheiten für Luftfeuchtigkeit. Beachten Sie, dass standardmäßig das Dashboard zeigt nur Temperatur und Feuchtigkeit Werte an.

    ![][img-telemetry]

## <a name="send-a-command-to-your-device"></a>Senden eines Befehls an Ihr Gerät

Das Dashboard in die Überwachung remote-Lösung ermöglicht es Ihnen, Befehle an Ihren Geräten über IoT Hub zu senden. Beispielsweise können Sie einen Befehl zum Festlegen der internen Temperatur von einem Gerät in der remote Überwachung Lösung senden.

1. Klicken Sie im remote Überwachung Lösung Dashboard im linken Bereich der **Geräteliste**Navigieren auf **Geräte** .

2. Klicken Sie auf **Geräte-ID** für Ihr Gerät in der **Liste Geräte**.

3. Klicken Sie im Bereich **Device-Details** auf **Befehle**.

    ![][13]

4. In der Dropdownliste **Befehl** **SetTemperature**wählen Sie aus, und geben Sie dann einen neuen Temperaturwert in **Temperatur** . Klicken Sie auf **Befehl senden** , um den Befehl an das Gerät zu senden.

    ![][14]

    > [AZURE.NOTE] Der Befehlsverlauf zeigt den Status des Befehls zuerst als **Ausstehend**. Wenn das Gerät den Befehl bestätigt, ändert sich der Status in **Success**.

5. Auf dem Dashboard stellen Sie sicher, dass das Gerät nun als neue Temperaturwert 75 sendet.

## <a name="next-steps"></a>Nächste Schritte

[Anpassen von vorkonfiguriert Lösungen] Artikel[ lnk-customize] werden einige Möglichkeiten, Sie können dieses Beispiel erweitern. Mögliche Erweiterungen einschließen real-Sensoren und weitere Befehle implementiert.

Erfahren Sie mehr über die [Berechtigungen auf der Website azureiotsuite.com][lnk-permissions].

[13]: ./media/iot-suite-visualize-connecting/suite4.png
[14]: ./media/iot-suite-visualize-connecting/suite7-1.png
[18]: ./media/iot-suite-visualize-connecting/suite10.png
[img-telemetry]: ./media/iot-suite-visualize-connecting/telemetry.png
[lnk-customize]: ../articles/iot-suite/iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-permissions]: ../articles/iot-suite/iot-suite-permissions.md

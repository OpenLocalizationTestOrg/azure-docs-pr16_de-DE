<properties
 pageTitle="Beispiel-Anwendung zum Senden von Nachrichten in der Cloud Gerät ausführen | Microsoft Azure"
 description="Bereitstellen und Ausführen einer Stichprobe-Anwendung zu Ihrem Brombeere Pi 3, die Nachrichten an IoT Hub sendet und die LED blinkt."
 services="iot-hub"
 documentationCenter=""
 authors="shizn"
 manager="timlt"
 tags=""
 keywords=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/21/2016"
 ms.author="xshi"/>

# <a name="32-run-sample-application-to-send-device-to-cloud-messages"></a>3,2 führen Sie Stichprobe Anwendung Gerät-Cloud-Nachrichten senden aus

## <a name="321-what-you-will-do"></a>3.2.1 mögliche Aktionen werden

Bereitstellen und Ausführen einer Stichprobe-Anwendung auf Ihre Brombeere Pi 3, die Nachrichten an Ihre IoT Hub sendet. Wenn Sie Probleme mit dem entsprechen, Zielwertsuche Lösungen in die [Seite zu behandeln](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="322-what-you-will-learn"></a>3.2.2 Gelernte wird

- Wie Sie mithilfe des Tools schlucken bereitstellen und Ausführen der Stichprobe Node.js Anwendungs auf Ihre Pi zurück.

## <a name="323-what-you-need"></a>3.2.3, benötigen Sie

- Sie müssen erfolgreich abgeschlossen haben im vorherigen Abschnitt in dieser Lektion: [Erstellen einer app Azure-Funktion und ein Konto Azure-Speicher zum Verarbeiten und IoT Hub Nachrichten speichern](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md).

## <a name="324-get-your-iot-hub-and-device-connection-strings"></a>3.2.4 Abrufen von Zeichenfolgen Verbindung IoT Hub und Gerät

Die Verbindungszeichenfolge Gerät wird verwendet, die Pi mit Ihrer IoT Hub verbinden. IoT-Hub für die Verbindungszeichenfolge wird verwendet, um die Identität des Geräts Ihrer IoT Hub Verbindung, die Ihre Pi im Hub IoT darstellt.

- Abrufen der Verbindungszeichenfolge für IoT Hub durch Ausführen des folgenden Azure CLI-Befehls:

```bash
az iot hub show-connection-string --name {my hub name} --resource-group iot-sample
```

`{my hub name}`ist der Name, den Sie in Lektion 2 angegeben haben. Verwenden Sie `iot-sample` als Wert des `{resource group name}` , wenn Sie den Wert in Lektion 2 geändert haben.

- Rufen Sie die Verbindungszeichenfolge Gerät, indem Sie den folgenden Befehl ausführen:

```bash
az iot device show-connection-string --hub {my hub name} --device-id myraspberrypi --resource-group iot-sample
```

`{my hub name}`hat den gleichen Wert wie mit dem vorherigen Befehl verwendet. Verwenden Sie `iot-sample` als Wert des `{resource group name}` und Verwenden von `myraspberrypi` als Wert des `{device id}` , wenn Sie den Wert in Lektion 2 geändert haben.

## <a name="325-configure-the-device-connection"></a>3.2.5 Konfigurieren der Verbindung mit dem Gerät

1. Initialisierung der Konfigurationsdatei durch Ausführen der folgenden Befehle:

    ```bash
    npm install
    gulp init
    ```

2. Öffnen Sie die Gerätekonfigurationsdatei `config-raspberrypi.json` in Visual Studio-Code, indem Sie den folgenden Befehl ausführen:

    ```bash
    # For Windows command prompt
    code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json
  
    # For macOS or Ubuntu
    code ~/.iot-hub-getting-started/config-raspberrypi.json
    ```

    ![config.JSON](media/iot-hub-raspberry-pi-lessons/lesson3/config.png)

3. Nehmen Sie die folgenden Ersatz in der `config-raspberrypi.json` Datei:

  - Ersetzen Sie mit dem Gerät IP-Adresse oder Hostname, die Sie von Ihrem System **[Gerät Hostname oder IP-Adresse]** `device-discovery-cli` oder mit dem Wert aus, den Sie in Lektion 1 konfiguriert übernommen.
  - Ersetzen Sie **[IoT Gerät Verbindungszeichenfolge]** mit den `device connection string` Sie für Ihren Kunden.
  - Ersetzen Sie **[IoT Hub Verbindungszeichenfolge]** mit den `iot hub connection string` Sie für Ihren Kunden.

Aktualisieren Sie die `config-raspberrypi.json` ablegen, damit Sie die Anwendung Stichprobe von Ihrem Computer bereitstellen können.

## <a name="326-deploy-and-run-the-sample-application"></a>3.2.6 Bereitstellen Sie, und führen Sie die Anwendung Stichprobe

Bereitstellen Sie, und führen Sie die Stichprobe Anwendung auf Ihre Pi, indem Sie den folgenden Befehl ausführen:

```bash
gulp
```

> [AZURE.NOTE] Der Standard-Task schlucken ausgeführt wird `install-tools`, `deploy`, und `run` Aufgaben nacheinander. In [Lektion 1](iot-hub-raspberry-pi-kit-node-lesson1-deploy-blink-app.md)wird die folgenden Aufgaben nacheinander getrennt ist.

## <a name="327-verify-the-sample-application-works"></a>3.2.7 Überprüfen der Stichprobe Anwendung funktioniert

Die LED sollte angezeigt werden, die mit Ihrem Pi blinken alle zwei Sekunden verbunden ist. Jedes Mal, wenn die LED blinkt, wird die Stichprobe Anwendung sendet eine Nachricht an Ihre IoT Hub und stellt sicher, dass die Nachricht an Ihre IoT Verteiler erfolgreich gesendet wurde. Darüber hinaus wird für jedes der Nachricht durch den IoT Hub empfangen, die Nachricht im Fenster Konsole gedruckt. Die Stichprobe Anwendung wird nach dem Senden 20 Nachrichten automatisch beendet.

![](media/iot-hub-raspberry-pi-lessons/lesson3/gulp_run.png)

## <a name="328-summary"></a>3.2.8 Zusammenfassung

Sie haben bereitgestellt, und führen Sie die neue blinken Beispiel-Anwendung auf Ihre Pi Gerät-Cloud-Nachrichten an Ihre IoT Hub senden. Sie können mit dem nächsten Abschnitt verschieben, um Ihre Nachrichten zu überwachen, wie sie mit dem Konto Azure-Speicher geschrieben werden.

## <a name="next-steps"></a>Nächste Schritte

[3.3 gelesenen Nachrichten beibehalten im Azure-Speicher](iot-hub-raspberry-pi-kit-node-lesson3-read-table-storage.md)
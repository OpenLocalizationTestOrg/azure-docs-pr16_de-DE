<properties
 pageTitle="Lesen von Nachrichten im Speicher Azure beibehalten | Microsoft Azure"
 description="Überwachen der Gerät-Cloud-Nachrichten an, wie sie in Ihrem Azure Table Storage geschrieben werden."
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

# <a name="33-read-messages-persisted-in-azure-storage"></a>3.3 gelesenen Nachrichten beibehalten im Azure-Speicher

## <a name="331-what-will-you-do"></a>3.3.1 Wozu kann Sie

Überwachen der Gerät-Cloud-Nachrichten, die an Ihre IoT Verteiler aus Ihrer Brombeere Pi 3 gesendet werden, wie die Nachrichten in Ihrem Azure Table Storage geschrieben werden. Wenn Sie Probleme mit dem entsprechen, Zielwertsuche Lösungen in die [Seite zu behandeln](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="332-what-will-you-learn"></a>3.3.2 was lernen Sie

- So verwenden Sie den schlucken Nachricht lesen Vorgang zum Lesen von Nachrichten, die in Ihrem Azure Table Storage beibehalten werden.

## <a name="333-what-do-you-need"></a>3.3.3 Was benötigen Sie

- Sie müssen im vorhergehenden Abschnitt [Ausführen der Azure blinken Stichprobe Anwendung auf Ihre Brombeere Pi 3](iot-hub-raspberry-pi-kit-node-lesson3-run-azure-blink.md)ist abgeschlossen.

## <a name="334-read-new-messages-from-your-storage-account"></a>3.3.4 lesen Sie neue Nachrichten von Ihrem Speicher

Im vorherigen Abschnitt haben Sie eine Beispiel-Anwendung auf Ihre Pi ausgeführt. Die Stichprobe Anwendung gesendete Nachrichten Ihrer Azure IoT Hub. Die an Ihre IoT Verteiler gesendete Nachrichten werden in Ihrem Azure Table Storage mithilfe der Funktion Azure-app gespeichert. Sie benötigen die Verbindungszeichenfolge Azure-Speicher zum Lesen von Nachrichten von Ihrem Azure Table Storage.

Gehen Sie folgendermaßen vor, um das Lesen von Nachrichten in Ihrem Azure Table Storage gespeichert:

1. Abrufen der Verbindungszeichenfolge durch Ausführen der folgenden Befehle:

    ```bash
    az storage account list -g iot-sample --query [].name
    az storage account show-connection-string -g iot-sample -n {storage name}
    ```

    Ruft der erste Befehl im `storage name` , die im zweiten Befehl beim Abrufen der Verbindungszeichenfolge verwendet wird. `iot-sample`ist der Standardwert von `{resource group name}` , wenn Sie den Wert in Lektion 2 geändert haben.

2. Öffnen Sie die Konfigurationsdatei `config-raspberrypi.json` Datei in Visual Studio-Code, indem Sie den folgenden Befehl ausführen:

    ```bash
    # For Windows command prompt
    code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json

    # For macOS or Ubuntu
    code ~/.iot-hub-getting-started/config-raspberrypi.json
    ```

3. Ersetzen Sie `[Azure Storage connection string]` mit der Verbindungszeichenfolge, die Sie in Schritt 1 erhalten haben.
4. Speichern der `config-raspberrypi.json` Datei.
5. Erneut senden von Nachrichten und Lesen Sie sie aus Ihrem Azure Table Storage durch den folgenden Befehl ausführen:

    ```bash
    gulp run --read-storage
    ```

    Die Logik zum Lesen von Azure Table Storage befindet sich in der `azure-table.js` Datei.

    ![schlucken ausführen – lesen-Speicher](media/iot-hub-raspberry-pi-lessons/lesson3/gulp_read_message.png)

## <a name="335-summary"></a>3.3.5 Zusammenfassung

Sie haben erfolgreich Ihre Pi an Ihre IoT Hub in der Cloud verbunden und verwendet die Anwendung blinken Stichprobe Gerät-Cloud-Nachrichten senden. Sie haben auch die app Azure-Funktion eingehende IoT Hub Nachrichten in Ihrem Azure Table Storage gespeichert verwendet. Sie können mit der nächsten Lektion zum Cloud-zu-Gerät Nachrichten von Ihrem IoT-Hub an Ihre Pi senden auf verschieben.

## <a name="next-steps"></a>Nächste Schritte

[Lektion 4: Senden von Nachrichten Cloud-zu-Gerät](iot-hub-raspberry-pi-kit-node-lesson4-send-cloud-to-device-messages.md)

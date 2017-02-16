<properties
    pageTitle="Ein Gerät mit dem IoT Gateway SDK simulieren | Microsoft Azure"
    description="Azure IoT Gateway SDK Anleitung für die Verwendung von Windows um zu sendende werden von einem simulierten Gerät mit dem Azure IoT Gateway SDK zu veranschaulichen."
    services="iot-hub"
    documentationCenter=""
    authors="chipalost"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="cpp"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/29/2016"
     ms.author="andbuc"/>


# <a name="azure-iot-gateway-sdk-beta--send-device-to-cloud-messages-with-a-simulated-device-using-windows"></a>Azure IoT Gateway SDK (Beta) – senden Gerät-Cloud-Nachrichten mit einem simulierten Gerät mithilfe von Windows

[AZURE.INCLUDE [iot-hub-gateway-sdk-simulated-selector](../../includes/iot-hub-gateway-sdk-simulated-selector.md)]

## <a name="build-and-run-the-sample"></a>Erstellen Sie, und führen Sie das Beispiel

Bevor Sie beginnen, müssen Sie folgende Schritte ausführen:

- [Einrichten Ihrer Entwicklungsumgebung] [ lnk-setupdevbox] für die Arbeit mit dem SDK unter Windows.
- [Erstellen einen IoT Hub] [ lnk-create-hub] in Ihrem Abonnement Azure benötigen Sie den Namen der Ihrer Hub diese exemplarische Vorgehensweise. Wenn Sie kein Konto haben, können Sie ein [kostenloses Konto] erstellen[ lnk-free-trial] in nur ein paar Minuten.
- Ihre IoT Hub zwei Geräten hinzu, und notieren Sie deren Ids und Gerät Tasten. Sie können das [Gerät Explorer oder Iothub-Explorer] [ lnk-explorer-tools] Tool zum Hinzufügen Ihrer Geräte IoT-Hub, die Sie im vorherigen Schritt erstellt haben und ihre Schlüssel abzurufen.

So erstellen Sie das Beispiel:

1. Öffnen Sie ein Eingabeaufforderungsfenster **Entwicklertools Eingabeaufforderungsfenster für VS2015** .
2. Navigieren Sie zu dem Stammordner in Ihrer lokalen Kopie des Repositorys **Azure-Iot-Gateway-Sdk** .
3. Führen Sie die **Tools\\build.cmd** Skript. Dieses Skript erstellt eine Visual Studio-Lösung-Datei, die Lösung erstellt und führt die Tests. Sie können die Visual Studio-Lösung in den Ordner **Erstellen** in Ihrer lokalen Kopie der **Azure-Iot-Gateway-Sdk** Repository suchen.

Um das Beispiel auszuführen:

Öffnen Sie die Datei in einem Texteditor **Beispiele\\Simulated_device_cloud_upload\\Src\\simulated_device_cloud_upload_win.json** in Ihrer lokalen Kopie des Repositorys **Azure-Iot-Gateway-Sdk** . Diese Datei konfiguriert die Module in der Stichprobe Gateway:

- Das Modul **IoTHub** eine Verbindung mit Ihrem IoT Hub. Sie müssen, um Daten zu Ihrer IoT Hub senden konfigurieren. Speziell, legen Sie den Wert **IoTHubName** auf den Namen der Ihrer IoT Hub, und legen Sie den Wert **IoTHubSuffix** **Azure-devices.net**. Legen Sie den Wert für **Transport** auf einen der: "HTTP", "AMQP" oder "MQTT". Beachten Sie, dass derzeit nur "HTTP" eine TCP-Verbindung für alle Nachrichten von Gerät freigeben kann. Wenn Sie den Wert "AMQP" oder "MQTT" festlegen, wird das Gateway eine separate TCP-Verbindung IoT Hub für jedes Gerät verwalten.
- Das Modul für die **Zuordnung** ordnet die MAC-Adressen Ihrer simulierten Geräte Ihrer IoT Hub Geräte-Ids. Stellen Sie sicher, dass **Geräte-ID** -Werte der Ids der zwei Geräte entsprechen, die Sie an Ihre IoT Verteiler hinzugefügt und die Werte **DeviceKey** die Taste zwei Geräte enthalten.
- Die Module **BLE1** und **BLE2** sind die simulierten Geräte. Beachten Sie, wie die MAC-Adressen im Modul **Zuordnung** übereinstimmen.
- Das Modul **Protokollierung** protokolliert Ihrer Aktivität Gateway in einer Datei.
- Der **Modulpfad** Werte abgebildet wird davon ausgegangen, dass Sie das Repository IoT Gateway SDK werden im Stammordner Ihrer Laufwerk **C:** geklont. Wenn Sie es an eine andere Position heruntergeladen haben, müssen Sie den **Pfad zum Modul** entsprechend zu ändern.
- Die Matrix **Links** am unteren Rand der JSON-Datei verbindet die Module **BLE1** und **BLE2** das Modul für die **Zuordnung** , und das Modul **Zuordnung** in das Modul **IoTHub** . Außerdem wird sichergestellt, dass alle Nachrichten, die vom Modul **Protokollierung** angemeldet sind.

```
{
    "modules" :
    [ 
        {
            "module name" : "IoTHub",
            "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\iothub\\Debug\\iothub_hl.dll",
            "args" : 
            {
                "IoTHubName" : "{Your IoT hub name}",
                "IoTHubSuffix" : "azure-devices.net",
                "Transport": "HTTP"
            }
        },
        {
            "module name" : "mapping",
            "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\identitymap\\Debug\\identity_map_hl.dll",
            "args" : 
            [
                {
                    "macAddress" : "01-01-01-01-01-01",
                    "deviceId"   : "{Device ID 1}",
                    "deviceKey"  : "{Device key 1}"
                },
                {
                    "macAddress" : "02-02-02-02-02-02",
                    "deviceId"   : "{Device ID 2}",
                    "deviceKey"  : "{Device key 2}"
                }
            ]
        },
        {
            "module name":"BLE1",
            "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\simulated_device\\Debug\\simulated_device_hl.dll",
            "args":
            {
                "macAddress" : "01-01-01-01-01-01"
            }
        },
        {
            "module name":"BLE2",
            "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\simulated_device\\Debug\\simulated_device_hl.dll",
            "args":
            {
                "macAddress" : "02-02-02-02-02-02"
            }
        },
        {
            "module name":"Logger",
            "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\logger\\Debug\\logger_hl.dll",
            "args":
            {
                "filename":"C:\\azure-iot-gateway-sdk\\deviceCloudUploadGatewaylog.log"
            }
        }
    ],
    "links" : [
        { "source" : "*", "sink" : "Logger" },
        { "source" : "BLE1", "sink" : "mapping" },
        { "source" : "BLE2", "sink" : "mapping" },
        { "source" : "mapping", "sink" : "IoTHub" }
    ]
}
```

Speichern Sie die Konfigurationsdatei vorgenommenen Änderungen.

Um das Beispiel auszuführen:

1. Navigieren Sie über die Befehlszeile zu dem Stammordner Ihrer lokalen Kopie der **Azure-Iot-Gateway-Sdk** Repository.
2. Führen Sie den folgenden Befehl ein:
  
    ```
    build\samples\simulated_device_cloud_upload\Debug\simulated_device_cloud_upload_sample.exe samples\simulated_device_cloud_upload\src\simulated_device_cloud_upload_win.json
    ```

3. Sie können das [Gerät Explorer oder Iothub-Explorer] [ lnk-explorer-tools] Tool zum Überwachen der Nachrichten, die IoT Hub vom Gateway empfängt in.


## <a name="next-steps"></a>Nächste Schritte

Wenn Sie eine erweiterte Verständnis des Gateways IoT SDK und mit einigen Codebeispielen experimentieren möchten, besuchen Sie die folgenden Entwicklertools Lernprogramme und Ressourcen:

- [Senden Sie Gerät-Cloud-Nachrichten auf einem tatsächlichen Gerät mit dem IoT Gateway SDK][lnk-physical-device]
- [Azure IoT Gateway SDK][lnk-gateway-sdk]

Die Funktionen von IoT Hub weiteren kann, finden Sie unter:

- [Leitfaden für Entwickler][lnk-devguide]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-explorer-tools]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/manage_iot_hub.md
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/

[lnk-physical-device]: iot-hub-gateway-sdk-physical-device.md

[lnk-devguide]: ./iot-hub-devguide.md
[lnk-create-hub]: iot-hub-create-through-portal.md 
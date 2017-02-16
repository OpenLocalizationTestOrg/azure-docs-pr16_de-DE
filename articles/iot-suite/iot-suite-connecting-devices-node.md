<properties
   pageTitle="Schließen Sie ein Gerät verwenden Node.js | Microsoft Azure"
   description="Beschreibt, wie ein Gerät mit der Azure IoT Suite vorkonfiguriert remote Überwachung-Lösung mit einer Anwendung geschrieben in Node.js verbinden."
   services=""
   suite="iot-suite"
   documentationCenter="na"
   authors="dominicbetts"
   manager="timlt"
   editor=""/>

<tags
   ms.service="iot-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/05/2016"
   ms.author="dobett"/>


# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-nodejs"></a>Schließen Sie Ihr Gerät in der remote Überwachung vorkonfigurierten Lösung (Node.js)

[AZURE.INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="build-and-run-the-nodejs-sample-solution"></a>Erstellen und Ausführen der Lösung node.js Stichprobe

1. Führen Sie zum *Microsoft Azure IoT SDKs* GitHub Repository klonen, und installieren Sie das *Microsoft Azure IoT Gerät SDK für Node.js* in Ihre desktop-Umgebung, [Vorbereiten Ihrer Entwicklungsumgebung] [ lnk-github-prepare] Anweisungen.

2. Aus der lokalen Kopie der [Azure-Iot-Sdks] [ lnk-github-repo] Repository, kopieren Sie die folgenden beiden Dateien aus dem Ordner Knoten/Gerät/Beispiele in einen Ordner auf Ihrem Gerät:

  - Packages.JSON
  - remote_monitoring.js

3. Öffnen Sie die Datei remote_monitoring.js, und suchen Sie nach der folgenden Variablen:

    ```
    var connectionString = "[IoT Hub device connection string]";
    ```

4. Ersetzen Sie **[IoT Hub Gerät Verbindungszeichenfolge]** mit Ihrem Gerät Verbindungszeichenfolge aus. Sie können die Werte für Ihre IoT Hub Hostname, Geräte-Id und Gerät Key im remote Überwachung Lösung Dashboard suchen. Eine Gerät Verbindungszeichenfolge weist das folgende Format:

    ```
    HostName={your IoT Hub hostname};DeviceId={your device id};SharedAccessKey={your device key}
    ```

    Wenn Ihre IoT Hub Hostname **Contoso** und Ihrem Gerät-Id **Mydevice**, sieht wie die Verbindungszeichenfolge aus:

    ```
    var connectionString = "HostName=contoso.azure-devices.net;DeviceId=mydevice;SharedAccessKey=2s ... =="
    ```

5. Speichern Sie die Datei ein. Führen Sie die folgenden Befehle über die Befehlszeile in den Ordner, der diese Dateien zum Installieren der notwendigen Pakete, und führen Sie die Anwendung Beispiel enthält:

    ```
    npm install --save
    node remote_monitoring.js
    ```

[AZURE.INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

[lnk-github-repo]: https://github.com/azure/azure-iot-sdks
[lnk-github-prepare]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
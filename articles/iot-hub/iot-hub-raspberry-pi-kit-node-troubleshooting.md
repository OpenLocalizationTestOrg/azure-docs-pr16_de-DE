<properties
 pageTitle="Problembehandlung bei | Microsoft Azure"
 description="Seite "Problembehandlung" für Brombeere Pi Node.js-Benutzeroberfläche"
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

# <a name="troubleshooting"></a>Behandlung von Problemen

## <a name="hardware-issues"></a>Hardwareproblemen

### <a name="the-application-runs-well-but-the-led-is-not-blinking"></a>Die Anwendung auch ausgeführt wird, aber nicht die LED blinkt

Dieses Problem bezieht sich immer auf die Hardware Verbindung Konnektivität. Gehen Sie folgendermaßen vor, um Probleme zu identifizieren.

1. Überprüfen Sie, ob Sie die richtige **GPIO** wählen Sie auf der Karte. Die zwei Ports in dieser Lektion sollten **GPIO GND (Pin-6)** und **GPIO 04 (Pin 7)**.
2. Überprüfen Sie, ob die Polarität der der LED korrekt ist. **Positive**, Anode Pin, sollte des Abschnitts mehr angeben.
3. Verwenden Sie **3.3V anheften** und **GND Pin** auf Ihre Brombeere Pi 3 ein. Behandeln Sie Ihre Pi als die Gleichstrom. Prüfen Sie, ob die LED einwandfrei funktioniert.

![LED-Spezifikation](media/iot-hub-raspberry-pi-lessons/troubleshooting/led_spec.png)

### <a name="other-hardware-issues"></a>Andere Hardwareproblemen

Informationen zum Beheben von häufig auftretenden Problemen auf die Himbeeren Pi 3 finden Sie in der [offiziellen Seite "Problembehandlung"](http://elinux.org/R-Pi_Troubleshooting).

## <a name="nodejs-package-issues"></a>Node.js Paket Probleme

### <a name="no-response-during-gulp-tasks"></a>Keine Antwort während schlucken Aufgaben

Wenn Sie Probleme bei der Ausführung von Aufgaben schlucken auftreten, können Sie Hinzufügen der `--verbose` Option für das Debuggen. Versuchen Sie zum Beenden der aktuellen schlucken Vorgänge mit `Ctrl + C` , und führen Sie dann den folgenden Befehl aus Ihrem Console-Fenster zu finden Sie unter Debuggen von Nachrichten. Ausführliche Fehlermeldungen, die bei der Ausgabe Console gedruckt werden möglicherweise angezeigt. 

```bash
gulp --verbose
```

### <a name="device-discovery-issues"></a>Probleme mit der Erkennung von Geräten

Um Hilfe zur Problembehandlung häufig auftretende Probleme mit der `devdisco` Befehl, und die [Infos zu](https://github.com/Azure/device-discovery-cli/blob/develop/readme.md)aktivieren.

### <a name="other-npm-issues"></a>Andere NPM Probleme

Versuchen Sie, aktualisieren Ihr NPM Paket mit den folgenden Befehl aus:

```bash
npm install -g npm
```

Wenn das Problem weiterhin besteht, lassen Sie Ihre Kommentare am Ende dieses Artikels, oder erstellen Sie ein Problem Github in unseren [Beispiel Repository](https://github.com/Azure-Samples/iot-hub-node-raspberrypi-getting-started)

## <a name="remote-debugging"></a>Remote-Debuggen

### <a name="run-the-sample-application-in-debug-mode"></a>Führen Sie die Stichprobe Anwendung im Debuggen-Modus

```bash
gulp run --debug
```

Wenn die Debuggen-Engine fertig ist, sollten Sie sehen ```Debugger listening on port 5858``` in der Ausgabe Console.

### <a name="configure-vs-code-to-connect-to-the-remote-device"></a>Konfigurieren der Code der im Vergleich mit einer Verbindung mit dem remote-Gerät

Öffnen Sie im Bereich **Debuggen** auf der linken Bildschirmseite.

Klicken Sie auf die grüne Schaltfläche **Debuggen starten** (F5). Im Vergleich mit einer Code öffnet eine **launch.json** -Datei, die Sie aktualisieren müssen.

Aktualisieren Sie die Datei **launch.json** mit dem folgenden Inhalt. Ersetzen Sie `[device hostname or IP address]` mit den tatsächlichen Gerät IP-Adresse oder Hostname.   

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Attach",
            "type": "node",
            "request": "attach",
            "port": 5858,
            "address": "[device hostname or IP address]",
            "restart": false,
            "sourceMaps": false,
            "outDir": null,
            "localRoot": "${workspaceRoot}",
            "remoteRoot": null
        }
    ]
}
```

![Remote-Debuggen-Konfiguration](media/iot-hub-raspberry-pi-lessons/troubleshooting/remote_debugging_configuration.png)

### <a name="attach-to-the-remote-application"></a>Fügen Sie an der remote-Anwendung an

Klicken Sie auf die grüne **Debuggen starten** (F5) Schaltfläche mit dem Debuggen beginnen. 

Lesen Sie [JavaScript im Vergleich mit einer Code](https://code.visualstudio.com/docs/languages/javascript#_debugging) Weitere Informationen zu den Debugger.

![Remote Debuggen interaktiv](media/iot-hub-raspberry-pi-lessons/troubleshooting/remote_debugging_interactive.png)

## <a name="azure-cli-issues"></a>Azure CLI Probleme

Azure ist eine Vorschau erstellen. Sie können auf [Vorschau installieren Leitfaden](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md) für Lösungen Zielwertsuche verweisen.

Wenn Sie mit dem Tool Fehler auftreten, Dateien Sie ein [Problem](https://github.com/Azure/azure-cli/issues) im Abschnitt **Probleme** mit der GitHub Repo.

Hilfe zur Problembehandlung für häufig auftretende Probleme die [Infos zu](https://github.com/Azure/azure-cli/blob/master/README.rst)überprüfen.

## <a name="python-installation-issues"></a>Probleme bei der Installation von Python

### <a name="legacy-installation-issues-macos"></a>Probleme bei der Installation älterer Versionen (Mac OS)

Bei der Installation von **Pip**ausgelöst ein Berechtigungsfehler Vorliegens legacy-Paketen, die mit Berechtigungen **Su** installiert werden. Diese Situation tritt auf, da die vorherige Installation von Python mit Kaffee (Mac OS) nicht vollständig deinstalliert wurde. Einige **Pip** -Paketen aus eine vorherige Installation wurden Root, erstellt, die wodurch die Berechtigungsfehler. Die Lösung besteht darin, diese Pakete installiert, indem Sie Stamm zu entfernen. Gehen Sie folgendermaßen vor, um diese Aufgabe auszuführen:

1. Wechseln Sie zu /usr/local/lib/python2.7/site-packages
2. Liste Pakete erstellen, indem Sie aus:`ls -l | grep root`
3. Deinstallieren Sie Pakete in Schritt 2:`sudo rm -rf {package name}`
4. Installieren Sie Python.

## <a name="azure-iot-hub-issues"></a>Azure IoT Hub Probleme

Wenn Sie mit Azure IoT-Hub erfolgreich bereitgestellt haben `azure-cli`, und Sie benötigen ein Tool zum Verwalten der Herstellen einer Verbindung mit Ihrem Hub IoT Geräten, versuchen Sie die folgenden Tools:

### <a name="device-explorer"></a>Gerät-Explorer

[Gerät Explorer](https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md) auf Ihrem lokalen Computer mit Windows ausgeführt wird und eine Verbindung mit Ihrem IoT Hub in Azure. Kommunikation mit den folgenden [IoT Hub Endpunkte](iot-hub-devguide.md):

- *Gerät Identitätsmanagement* bereitstellen und Verwalten von Geräten registriert mit Ihrer IoT Hub.
- *Empfangen Gerät-Cloud* , sodass Sie von Ihrem Gerät an Ihre IoT Hub gesendete Nachrichten überwachen können.
- Aktivieren Sie das Senden von Nachrichten an Ihren Geräten aus Ihrer IoT Hub *Cloud-zu-Gerät zu senden* .

Konfigurieren der `IoT hub connection string` innerhalb dieses Tool alle Funktionen verwenden.

### <a name="iot-hub-explorer"></a>IoT Hub Explorer

[IoT Hub Explorer](https://github.com/Azure/azure-iot-sdks/blob/master/tools/iothub-explorer/readme.md) ist ein Beispiel für mehrere Plattformen CLI Tool zum Verwalten von Clients Gerät. Das Tool können Sie die Geräte in der Registrierung Identität verwalten, überwachen Gerät-Cloud-Nachrichten und Cloud-zu-Gerät Befehle senden.

Um die neueste Version (Vorabversion) des Tools Iothub-Explorer zu installieren, führen Sie den folgenden Befehl in Ihrer Umgebung Befehlszeile aus:

```
npm install -g iothub-explorer@latest
```

Mit dem folgenden Befehl können weitere Hilfe zum alle Iothub Explorer-Befehle und deren Parameter anzuzeigen:

```bash
iothub-explorer help
```

### <a name="use-azure-portal-to-manage-your-resources"></a>Verwalten von Ressourcen mithilfe von Azure-portal

In diesen Lektionen wird eine vollständige CLI-Benutzeroberfläche zum Erstellen und verwalten Sie alle Ihre Azure Ressourcen bereitgestellt. Sie möchten möglicherweise auch der [Azure-Portal](../azure-portal-overview.md) verwenden, um Hilfe bereitstellen, verwalten und Debuggen Azure Ressourcen.

## <a name="azure-storage-issues"></a>Probleme Azure-Speicher

[Microsoft Azure-Speicher-Explorer (Preview)](http://storageexplorer.com) ist eine eigenständige Anwendung von Microsoft, die Sie auf Windows, Mac OS und Linux mit Azure-Speicher Daten arbeiten kann. Dieses Tool können Sie eine Verbindung mit der Tabelle und die Daten darin finden Sie unter. Dieses Tool können um Ihre Azure-Speicher Probleme zu behandeln.

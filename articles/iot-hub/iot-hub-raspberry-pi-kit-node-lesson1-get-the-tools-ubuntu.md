<properties
 pageTitle="Besorgen Sie sich die Tools (Ubuntu 16.04) | Microsoft Azure"
 description="Herunterladen Sie und installieren Sie die erforderlichen Tools und Software für das erste Beispiel Anwendung für Ihre Pi auf Ubuntu."
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

# <a name="12-get-the-tools-ubuntu-1604"></a>1.2 erhalten Sie die Tools (Ubuntu 16.04)

> [AZURE.SELECTOR]
- [Windows 7 +](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)
- [Ubuntu 16.04](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-ubuntu.md)
- [Mac OS 10.10.](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-mac.md)

## <a name="121-what-you-will-do"></a>1.2.1 mögliche Aktionen werden

Laden Sie die Entwicklungstools und der Software für das erste Stichprobe Anwendung für Ihre Brombeere Pi 3. Wenn Sie Probleme mit dem entsprechen, Zielwertsuche Lösungen in die [Seite zu behandeln](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="122-what-you-will-learn"></a>1.2.2 Gelernte wird

In diesem Abschnitt lernen Sie:

- Installieren von Git und Node.js
  - [Git](https://git-scm.com) ist ein open-Source verteilt Version Steuerelement. Die Anwendung Beispiel für diese Lektion wird auf Git gespeichert.
  - [Node.js](https://nodejs.org/en/) ist eine JavaScript-Laufzeit mit einem Rich-Paket Netz.
- So NPM verwenden, um zusätzliche Node.js Development Tools zu installieren.
  - Die erforderliche Mindestversion von Node.js ist 4,5 LTS.
  - [NPM](https://www.npmjs.com) ist eine der Paket-Manager für Node.js

## <a name="123-what-do-you-need"></a>1.2.3 Was benötigen Sie

- Eine Verbindung zu den Entwicklungstools und die Software herunterladen
- Ein Computer, der Ubuntu 16.04 oder höher ausgeführt wird 

## <a name="124-install-git-nodejs-and-npm"></a>1.2.4 Installieren von Git, Node.js und NPM

Verwenden Sie die Tastenkombination ALT + `Ctrl + Alt + T` ein Terminal-Fenster zu öffnen, und führen Sie die folgenden Befehle:

```bash
sudo apt-get update
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
sudo apt-get install -y nodejs
sudo apt-get install git
```

## <a name="125-install-additional-nodejs-development-tools"></a>1.2.5 installieren Sie zusätzliche Node.js Development tools

Sie verwenden [gulp.js](http://gulpjs.com) , um die Bereitstellung der Stichprobe zu Ihrer Pi Anwendung zu automatisieren. Sie können auch das [Gerät-Discovery-Cli](https://github.com/Azure/device-discovery-cli) zum Abrufen von Netzwerkinformationen zu Ihren IoT Geräten verwenden.

Installieren von `gulp` und `device-discovery-cli` durch den folgenden Befehl im Terminal-Fenster ausführen:

```bash
sudo npm install -g device-discovery-cli gulp
```

Wenn Sie Probleme beim Installieren von Node.js und diese zusätzlichen Development Tools auf Ubuntu auftreten, finden Sie unter der [Leitfadens zur Problembehandlung](iot-hub-raspberry-pi-kit-node-troubleshooting.md) für Lösungen für häufige Probleme.

## <a name="126-install-visual-studio-code"></a>1.2.6 installieren von Visual Studio-Code

[Herunterladen](https://code.visualstudio.com/docs/setup/linux) und Installieren von Visual Studio-Code. Visual Studio-Code ist eine einfache und dennoch leistungsfähige Quellcode-Editor für Windows, Mac OS und Linux. Später in diesem Lernprogramm verwenden Sie diesen Editor, zum Bearbeiten des Codes Stichprobe.

## <a name="127-summary"></a>1.2.7 Zusammenfassung

Sie haben die erforderlichen Development Tools und Software für das erste Beispiel-Anwendung installiert haben. Im nächsten Abschnitt erstellen, bereitstellen und Ausführen die Stichprobe Anwendung auf Ihre Pi.

## <a name="next-steps"></a>Nächste Schritte

[1.3 zu erstellen und Bereitstellen der Anwendungs blinken Stichprobe](iot-hub-raspberry-pi-kit-node-lesson1-deploy-blink-app.md)
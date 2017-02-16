<properties
 pageTitle="Besorgen Sie sich die Tools (Windows 7 +) | Microsoft Azure"
 description="Herunterladen Sie und installieren Sie die erforderlichen Tools und Software für das erste Beispiel Anwendung für Ihre Pi auf Windows 7 oder höher."
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

# <a name="12-get-the-tools-windows-7-"></a>1.2 erhalten die Tools (Windows 7 +) 

> [AZURE.SELECTOR]
- [Windows 7 +](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)
- [Ubuntu 16.04](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-ubuntu.md)
- [Mac OS 10.10.](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-mac.md)

## <a name="121-what-you-will-do"></a>1.2.1 mögliche Aktionen werden

Laden Sie die Entwicklungstools und der Software für das erste Stichprobe Anwendung für Ihre Brombeere Pi 3. Wenn Sie Probleme mit dem entsprechen, Zielwertsuche Lösungen in die [Seite zu behandeln](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="122-what-you-will-learn"></a>1.2.2 Gelernte wird
- Installieren von Git und Node.js
  - [Git](https://git-scm.com) ist ein open-Source verteilt Version Steuerelement. Die Anwendung Beispiel für diese Lektion wird auf Git gespeichert.
  - [Node.js](https://nodejs.org/en/) ist eine JavaScript-Laufzeit mit einem Rich-Paket Netz.
- So NPM verwenden, um zusätzliche Node.js Development Tools zu installieren.
  - Die Mindestversion eines Node.js ist 4,5 LTS.
  - [NPM](https://www.npmjs.com) ist eine der Paket-Manager für Node.js.

## <a name="123-what-you-need"></a>1.2.3, benötigen Sie

- Eine Verbindung zu den Entwicklungstools und die Software herunterladen
- Ein Computer, auf dem Windows ausgeführt wird

## <a name="124-install-git-and-nodejs"></a>1.2.4 Installieren von Git und Node.js

Klicken Sie auf den nachstehenden Links, um Git und Node.js LTS für Windows herunterladen und installieren.

- [Abrufen von Git für Windows](https://git-scm.com/download/win/)
- [Abrufen von Node.js LTS für Windows](https://nodejs.org/en/)

## <a name="125-install-additional-nodejs-development-tools"></a>1.2.5 installieren Sie zusätzliche Node.js Development tools

Sie verwenden [gulp.js](http://gulpjs.com) , um die Bereitstellung der Stichprobe zu Ihrer Pi Anwendung zu automatisieren. Sie können auch das [Gerät-Discovery-Cli](https://github.com/Azure/device-discovery-cli) zum Abrufen von Netzwerkinformationen zu Ihren IoT Geräten verwenden.

Starten Sie ein Eingabeaufforderungsfenster als Administrator an. Installieren von `gulp` und `device-discovery-cli` durch den folgenden Befehl ausführen:

```bash
npm install -g device-discovery-cli gulp
```
    
Wenn Sie Probleme beim Installieren von Node.js und diese zusätzlichen Node.js Development Tools auf Ihrem Computer zur Verfügung steht, finden Sie unter [Leitfadens zur Problembehandlung](iot-hub-raspberry-pi-kit-node-troubleshooting.md) für Lösungen für häufige Probleme.

## <a name="126-install-visual-studio-code"></a>1.2.6 installieren von Visual Studio-Code

[Herunterladen](https://code.visualstudio.com/docs/setup/windows) und Installieren von Visual Studio-Code. Visual Studio-Code ist eine einfache und dennoch leistungsfähige Quellcode-Editor für Windows, Mac OS und Linux. Später in diesem Lernprogramm verwenden Sie diesen Editor, zum Bearbeiten des Codes Stichprobe.

## <a name="127-summary"></a>1.2.7 Zusammenfassung

Sie haben die erforderlichen Development Tools und Software für das erste Beispiel-Anwendung installiert haben. Im nächsten Abschnitt erstellen, bereitstellen und Ausführen die Stichprobe Anwendung auf Ihre Pi.

## <a name="next-steps"></a>Nächste Schritte

[1.3 zu erstellen und Bereitstellen der Anwendungs blinken Stichprobe](iot-hub-raspberry-pi-kit-node-lesson1-deploy-blink-app.md)

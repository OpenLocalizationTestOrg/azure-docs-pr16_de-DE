<properties
 pageTitle="Optionaler Abschnitt – Ändern der ein- und auszuschalten Verhalten der LED | Microsoft Azure"
 description="Passen Sie die Nachrichten so ändern Sie der ein- und auszuschalten Verhalten der LEDS an."
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

# <a name="42-optional-section-change-the-on-and-off-behavior-of-the-led"></a>4.2 Optionaler Abschnitt: Ändern der ein- und auszuschalten Verhalten der LED

## <a name="421-what-you-will-do"></a>4.2.1 mögliche Aktionen werden

Passen Sie die Nachrichten so ändern Sie der ein- und auszuschalten Verhalten der LEDS an. Wenn Sie Probleme mit dem entsprechen, Zielwertsuche Lösungen in die [Seite zu behandeln](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="422-what-you-will-learn"></a>4.2.2 Gelernte wird

Verwenden Sie zusätzliche Node.js-Funktionen, um der ein- und auszuschalten Verhalten der LEDS ändern.

## <a name="423-what-you-need"></a>4.2.3, benötigen Sie

Sie müssen [4.1 Ausführen einer Stichprobe-Anwendung auf Ihre Brombeere Pi Cloud Gerät Nachrichten erhalten](iot-hub-raspberry-pi-kit-node-lesson4-send-cloud-to-device-messages.md)ist abgeschlossen.

## <a name="424-add-nodejs-functions"></a>4.2.4 Hinzufügen von Node.js-Funktionen

1. Öffnen Sie die Anwendung Beispiel in Visual Studio-Code, durch Ausführen der folgenden Befehle:

    ```bash
    cd Lesson4
    code .
    ```

2. Öffnen der `app.js` Datei, und fügen Sie dann am Ende der folgenden Funktionen hinzu:

    ```javascript
    function turnOnLED() {
      wpi.digitalWrite(CONFIG_PIN, 1);
    }

    function turnOffLED() {
      wpi.digitalWrite(CONFIG_PIN, 0);
    }
    ```

    ![Aktualisieren von app.js](media/iot-hub-raspberry-pi-lessons/lesson4/updated_app_js.png)

3. Fügen Sie die folgenden Bedingungen vor der vorgeschlagene im Switch-Case-Block von der `receiveMessageCallback` (Funktion):

    ```javascript
    case 'on':
      turnOnLED();
      break;
    case 'off':
      turnOffLED();
      break;
    ```

    Jetzt haben Sie die Anwendung Stichprobe reagieren auf Weitere Anweisungen durch Nachrichten konfiguriert. Die Anweisung "auf" in der LED-Anzeige und die Anweisung "deaktiviert" wird ausgeschaltet die LED.

4. Öffnen Sie die Datei gulpfile.js, und fügen Sie eine neue Funktion vor der Funktion `sendMessage`:

    ```javascript
    var buildCustomMessage = function (messageId) {
      if ((messageId & 1) && (messageId < MAX_MESSAGE_COUNT)) {
        return new Message(JSON.stringify({ command: 'on', messageId: messageId }));
      } else if (messageId < MAX_MESSAGE_COUNT) {
        return new Message(JSON.stringify({ command: 'off', messageId: messageId }));
      } else {
        return new Message(JSON.stringify({ command: 'stop', messageId: messageId }));
      }
    }
    ```

    ![Aktualisieren von gulpfile](media/iot-hub-raspberry-pi-lessons/lesson4/updated_gulpfile.png)

5. In der `sendMessage` -Funktion, ersetzen Sie die Zeile `var message = buildMessage(sentMessageCount);` mit der neuen Zeile in den folgenden Codeausschnitt dargestellt:

    ```javascript
    var message = buildCustomMessage(sentMessageCount);
    ```

6. Alle Änderungen zu speichern.

### <a name="425-deploy-and-run-the-sample-application"></a>4.2.5 bereitstellen Sie, und führen Sie die Anwendung Stichprobe

Bereitstellen Sie, und führen Sie die Stichprobe Anwendung auf Ihre Pi, indem Sie den folgenden Befehl ausführen:

```bash
gulp
```

Die LED für zwei Sekunden aktiviert, und klicken Sie dann deaktiviert für einen anderen zwei Sekunden sollte angezeigt werden. Die letzte Nachricht "Beenden" beendet die Ausführung der Anwendung Stichprobe.

![Aktivieren und deaktivieren](media/iot-hub-raspberry-pi-lessons/lesson4/gulp_on_and_off.png)

Herzlichen Glückwunsch! Sie haben die Nachrichten, die von Ihrer IoT-Hub an die Pi gesendet werden, erfolgreich angepasst.

### <a name="427-summary"></a>4.2.7 Zusammenfassung

In diesem Abschnitt optionale demos wie die Nachrichten so anpassen, dass die Anwendung Stichprobe ein- und Ausschalten der LED-Verhalten auf eine andere Weise steuern kann.


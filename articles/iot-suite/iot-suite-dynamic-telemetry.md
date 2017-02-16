<properties
    pageTitle="Verwenden von dynamischen telemetrieprotokoll | Microsoft Azure"
    description="Führen Sie dieses Lernprogramm erfahren Sie, wie dynamische werden mit der remote Überwachung vorkonfigurierten Solution verwendet."
    services=""
    suite="iot-suite"
    documentationCenter=""
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-suite"
     ms.devlang="na"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/25/2016"
     ms.author="dobett"/>

# <a name="use-dynamic-telemetry-with-the-remote-monitoring-preconfigured-solution"></a>Verwenden von dynamischen werden mit der remote-Überwachung vorkonfiguriert Lösung

## <a name="introduction"></a>Einführung

Dynamische telemetrieprotokoll ermöglicht es Ihnen visualisiert werden alle an der remote Überwachung vorkonfigurierten Solution gesendet werden sollen. Die simulierten Geräten, die mit dem vorkonfigurierten Lösung bereitstellen senden Temperatur und Feuchtigkeit werden Sie auf dem Dashboard visualisieren können. Wenn Sie die vorhandenen simulierten Geräte anpassen, erstellen neue simulierte Geräten oder Herstellen einer Verbindung die vorkonfigurierten Lösung mit physische Geräte können Sie andere Werte werden wie externe Temperaturen, u/min oder Windspeed senden. Dann können Sie diese zusätzlichen werden auf dem Dashboard darstellen.

In diesem Lernprogramm verwendet ein einfaches Node.js simuliertes Gerät, das Sie einfach ändern können, um experimentieren Sie mit den dynamischen werden.

Um dieses Lernprogramms abgeschlossen haben, müssen Sie:

- Ein aktives Azure-Abonnement. Wenn Sie kein Konto haben, können Sie ein kostenloses Testversion Konto nur wenigen Minuten erstellen. Ausführliche Informationen finden Sie [Kostenlose Testversion Azure][lnk_free_trial].
- [Node.js] [ lnk-node] Version 0.12.x oder höher.

Sie können Ausführen dieses Lernprogramms auf einem beliebigen Betriebssystem, wie etwa Windows oder Linux, wo Sie Node.js installieren können.

[AZURE.INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

## <a name="configure-the-nodejs-simulated-device"></a>Konfigurieren des simulierten Node.js-Geräts

1. Klicken Sie auf dem Dashboard remote Überwachung klicken Sie auf **+ Gerät hinzufügen** , und fügen Sie ein benutzerdefiniertes Gerät. Notieren Sie sich den IoT-Hub Hostname, Geräte-Id und Gerät-Taste. Sie benötigen diese später in diesem Lernprogramm, wenn Sie die remote_monitoring.js Gerät Clientanwendung vorbereiten.

2. Stellen Sie sicher, Node.js Version 0.12.x oder höher installiert ist, auf Ihrem Entwicklungscomputer. Führen Sie `node --version` über die Befehlszeile oder in einer Shell die Version zu überprüfen. Informationen zur Verwendung eines Paket-Managers, um Node.js unter Linux zu installieren, finden Sie unter [Installieren von Node.js über Paket-Manager][node-linux].

3. Wenn Sie Node.js installiert haben, Klonen die neueste Version der [Azure-Iot-Sdks] [ lnk-github-repo] Repository auf Ihren Entwicklungscomputer. Verwenden Sie immer die **Gestaltungsvorlage** Verzweigung für die neueste Version von Bibliotheken und Beispiele.

4. Aus der lokalen Kopie der [Azure-Iot-Sdks] [ lnk-github-repo] Repository, kopieren Sie die folgenden beiden Dateien aus dem Ordner Knoten/Gerät/Beispiele in einen leeren Ordner auf Ihrem Entwicklungscomputer:

  - Packages.JSON
  - remote_monitoring.js

5. Öffnen Sie die Datei remote_monitoring.js, und suchen Sie die Definition der folgenden:

    ```
    var connectionString = "[IoT Hub device connection string]";
    ```

6. Ersetzen Sie **[IoT Hub Gerät Verbindungszeichenfolge]** mit Ihrem Gerät Verbindungszeichenfolge aus. Verwenden Sie die Werte für Ihre IoT Hub Hostname, Geräte-Id und Gerät-Taste, die Sie in Schritt 1 Notieren Sie sich vorgenommen. Eine Gerät Verbindungszeichenfolge weist das folgende Format:

    ```
    HostName={your IoT Hub hostname};DeviceId={your device id};SharedAccessKey={your device key}
    ```

    Wenn Ihre IoT Hub Hostname **Contoso** und Ihrem Gerät-Id **Mydevice**, sieht die Verbindungszeichenfolge wie folgt aus:

    ```
    var connectionString = "HostName=contoso.azure-devices.net;DeviceId=mydevice;SharedAccessKey=2s ... =="
    ```

7. Speichern Sie die Datei ein. Führen Sie in einer Shell oder im Eingabeaufforderungsfenster in den Ordner, der diese Dateien zum Installieren der notwendigen Pakete, und führen Sie die Anwendung Beispiel enthält die folgenden Befehle:

    ```
    npm install
    node remote_monitoring.js
    ```

## <a name="observe-dynamic-telemetry-in-action"></a>Beobachten Sie dynamische werden in Aktion

Das Dashboard zeigt die Temperatur und Feuchtigkeit werden von den vorhandenen simulierten Geräten:

![Der Standard-dashboard][image1]

Wenn Sie das Node.js simulierte Gerät, die, das Sie im vorherigen Abschnitt ausgeführt haben auswählen, sehen Sie Temperatur, Luftfeuchtigkeit und externe Temperaturen werden:

![Hinzufügen von externen Temperatur zum dashboard][image2]

Die Überwachung remote-Lösung Typ werden zusätzliche externe Temperaturen erkennt und das Diagramm auf dem Dashboard hinzugefügt.

## <a name="add-a-telemetry-type"></a>Fügen Sie einen Typ werden

Im nächsten Schritt wird so ersetzen Sie die vom Node.js simulierten Gerät mit einem neuen Satz von Werten generiert werden:

1. Beenden des Node.js simulierten Geräts durch Eingabe von **STRG + C** in Ihrem Eingabeaufforderungsfenster oder Shell.

2. Klicken Sie in der Datei remote_monitoring.js sehen Sie die Basis Datenwerte für vorhandene Temperatur, Luftfeuchtigkeit und externe Temperaturen werden. Fügen Sie einen Basis Datenwert **u/Min** wie folgt ein:

    ```
    // Sensors data
    var temperature = 50;
    var humidity = 50;
    var externalTemperature = 55;
    var rpm = 200;
    ```

3. Node.js simulierte Gerät verwendet die Funktion **GenerateRandomIncrement** in der Datei remote_monitoring.js eine zufällige Inkrement auf der Basis Datenwerte hinzufügen. Zufällige Sie den Wert für **u/Min** durch Hinzufügen einer Zeile des Codes nach der vorhandenen Randomizations wie folgt aus:

    ```
    temperature += generateRandomIncrement();
    externalTemperature += generateRandomIncrement();
    humidity += generateRandomIncrement();
    rpm += generateRandomIncrement();
    ```

4. Fügen Sie den neuen Wert für u/Min an die JSON-Nutzlast, die das Gerät an IoT Hub sendet hinzu:

    ```
    var data = JSON.stringify({
      'DeviceID': deviceId,
      'Temperature': temperature,
      'Humidity': humidity,
      'ExternalTemperature': externalTemperature,
      'RPM': rpm
    });
    ```

5. Führen Sie die simulierten Node.js-Gerät mit dem folgenden Befehl aus:

    ```
    node remote_monitoring.js
    ```

6. Sehen Sie sich den neuen u/Min werden ein, der im Diagramm im Dashboard angezeigt werden:

![Hinzufügen von u/Min zum dashboard][image3]

> [AZURE.NOTE] Sie müssen möglicherweise deaktivieren und aktivieren dann das Gerät Node.js auf der Seite **Geräte** im Dashboard, damit die Änderung sofort angezeigt.

## <a name="customize-the-dashboard-display"></a>Anpassen der Dashboardanzeige

Die Nachricht **Gerät-Informationen** kann Metadaten über die werden Kontaktelemente das Gerät an IoT Verteiler senden kann. Diese Metadaten kann die Typen werden angeben, die das Gerät sendet. Ändern Sie den Wert **DeviceMetaData** in der Datei remote_monitoring.js folgen die **Befehle** Definition **werden** Definition aufnehmen möchten. Der folgende Codeausschnitt zeigt die Definition **Befehle** (Achten Sie darauf, dass Sie zum Hinzufügen eines `,` nach der Definition der **Befehle** ):

```
'Commands': [{
  'Name': 'SetTemperature',
  'Parameters': [{
    'Name': 'Temperature',
    'Type': 'double'
  }]
},
{
  'Name': 'SetHumidity',
  'Parameters': [{
    'Name': 'Humidity',
    'Type': 'double'
  }]
}],
'Telemetry': [{
  'Name': 'Temperature',
  'Type': 'double'
},
{
  'Name': 'Humidity',
  'Type': 'double'
},
{
  'Name': 'ExternalTemperature',
  'Type': 'double'
}]
```

> [AZURE.NOTE] Die Überwachung remote-Lösung verwendet eine Übereinstimmung Groß-und Kleinschreibung, um die Metadatendefinition mit Daten im Stream werden vergleichen.

Hinzufügen einer Definition **werden** , wie im vorherigen Codeausschnitt dargestellt wird das Verhalten des Dashboards nicht geändert werden. Allerdings kann die Metadaten auch ein Attribut **DisplayName** zum Anpassen der Anzeige im Dashboard enthalten. Aktualisieren Sie die Definition der **werden** Metadaten aus, wie im folgenden Codeausschnitt dargestellt:

```
'Telemetry': [
{
  'Name': 'Temperature',
  'Type': 'double',
  'DisplayName': 'Temperature (C*)'
},
{
  'Name': 'Humidity',
  'Type': 'double',
  'DisplayName': 'Humidity (relative)'
},
{
  'Name': 'ExternalTemperature',
  'Type': 'double',
  'DisplayName': 'Outdoor Temperature (C*)'
}
]
```

Das folgende Bildschirmabbild zeigt, wie diese Änderung die Diagrammlegende auf dem Dashboard ändert:

![Anpassen der Diagrammlegende][image4]

> [AZURE.NOTE] Sie müssen möglicherweise deaktivieren und aktivieren dann das Gerät Node.js auf der Seite **Geräte** im Dashboard, damit die Änderung sofort angezeigt.

## <a name="filter-the-telemetry-types"></a>Filtern der Datentypen werden

Standardmäßig wird das Diagramm auf dem Dashboard jede Datenreihe im Stream werden. Die Metadaten **Gerät-Informationen** können Sie die Anzeige von bestimmter werden im Diagramm unterdrücken. 

Um das Diagramm anzeigen nur Temperatur und Feuchtigkeit werden gestalten, lassen Sie **ExternalTemperature** wie folgt aus den Metadaten der **Gerät-Informationen** **werden aus** :

```
'Telemetry': [
{
  'Name': 'Temperature',
  'Type': 'double',
  'DisplayName': 'Temperature (C*)'
},
{
  'Name': 'Humidity',
  'Type': 'double',
  'DisplayName': 'Humidity (relative)'
},
//{
//  'Name': 'ExternalTemperature',
//  'Type': 'double',
//  'DisplayName': 'Outdoor Temperature (C*)'
//}
]
```

Die **Im freien Temperatur** wird im Diagramm nicht mehr angezeigt:

![Filtern der werden auf dem dashboard][image5]

Diese Änderung wirkt sich nur auf die Darstellung des Diagramms. Der Datenwerte **ExternalTemperature** werden weiterhin gespeichert und für die Back-End-Verarbeitung zur Verfügung gestellt.

> [AZURE.NOTE] Sie müssen möglicherweise deaktivieren und aktivieren dann das Gerät Node.js auf der Seite **Geräte** im Dashboard, damit die Änderung sofort angezeigt.

## <a name="handle-errors"></a>Behandeln von Fehlern

Für einen Datenstream im Diagramm angezeigt werden muss seinen **Typ** in den Metadaten **Gerät-Info** den Datentyp der Werte werden übereinstimmen. Angenommen, wenn die Metadaten gibt an, dass der **Typ** Feuchtigkeit **Int** und **doppelte** im Stream werden gefunden wird werden dann die Luftfeuchtigkeit werden keine im Diagramm angezeigt. Die **Luftfeuchtigkeit** Werte werden jedoch weiterhin gespeichert und für die Back-End-Verarbeitung zur Verfügung gestellt.

## <a name="next-steps"></a>Nächste Schritte

Jetzt, da Sie zum Verwenden von dynamischen werden gesehen haben, können Sie erfahren Sie mehr darüber, wie die vorkonfigurierten Lösungen Geräteinformationen verwenden: [Gerät Informations-Metadaten in der Fernbedienung überwachen vorkonfigurierten Lösung][lnk-devinfo].

[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[image1]: media/iot-suite-dynamic-telemetry/image1.png
[image2]: media/iot-suite-dynamic-telemetry/image2.png
[image3]: media/iot-suite-dynamic-telemetry/image3.png
[image4]: media/iot-suite-dynamic-telemetry/image4.png
[image5]: media/iot-suite-dynamic-telemetry/image5.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-node]: http://nodejs.org
[node-linux]: https://github.com/nodejs/node-v0.x-archive/wiki/Installing-Node.js-via-package-manager
[lnk-github-repo]: https://github.com/Azure/azure-iot-sdks

> [AZURE.SELECTOR]
- [Node.js](../articles/iot-hub/iot-hub-node-node-twin-getstarted.md)
- [C#](../articles/iot-hub/iot-hub-csharp-node-twin-getstarted.md)

## <a name="introduction"></a>Einführung

Gerät im Vergleich sind JSON-Dokumente, die Gerätestatusinformationen (Metadaten, Konfigurationen und Bedingungen) zu speichern. IoT Hub behält ein Gerät Ziel für jedes Gerät, das Herstellen einer Verbindung IoT-Hub an.

Verwenden Sie Gerät im Vergleich zu:

* Gerät Metadaten aus der Back-End zu speichern.
* Bericht mit aktuellen Zustandsinformationen wie verfügbaren Funktionen und Bedingungen (beispielsweise der Connectivity-Methode verwendet) aus der app Gerät.
* Den Zustand des langer Workflows (z. B. Firmware und Konfiguration Updates) zwischen Gerät app und Back-End zu synchronisieren.
* Abfragen von Ihrem Gerät Metadaten, Konfiguration oder Zustand.

> [AZURE.NOTE] Gerät im Vergleich dienen für die Synchronisierung und zum Abfragen von Gerätekonfigurationen und Bedingungen. [Gerät-Cloud - Nachrichten] verwenden[ lnk-d2c] Zeichenfolgen mit einem Zeitstempel versehen Ereignisse (z. B. werden Streams von einem zeitbasierten Sensordaten) und [Cloud-zu-Gerät Methoden] [ lnk-methods] für die interaktive Steuerung von Geräten, wie etwa ein Lüfter aus einer app benutzergesteuerte einschalten.

Gerät im Vergleich werden in einem IoT-Hub gespeichert und enthalten:

* *Tags*, Gerät Metadaten kann nur über die Back-End zugegriffen;
* *gewünschten Eigenschaften*, JSON-Objekte, die in die Back-End und Observable von der app Gerät geändert werden. und
* Beenden *gemeldeten Eigenschaften*, JSON-Objekte, die von der app Gerät veränderbare und von der Rückseite gelesen werden. Tags und Eigenschaften Arrays enthalten, aber Objekte geschachtelt werden können.

![][img-twin]

Darüber hinaus kann das app-Back-End Gerät im Vergleich basierend auf alle die oben angegebenen Daten Abfragen.
Verweisen auf [arbeiten Gerät im Vergleich] [ lnk-twins] für Weitere Informationen zu Gerät im Vergleich und zur [Abfragesprache IoT Hub] [ lnk-query] Bezug für Abfragen.

> [AZURE.NOTE] Zu diesem Zeitpunkt für Gerät im Vergleich nur von Geräten, die Verbindung IoT Hub mit dem MQTT-Protokoll zugegriffen werden. Lizenzinformationen finden Sie die [MQTT unterstützen] [ lnk-devguide-mqtt] Artikel Anweisungen zum Konvertieren von vorhandenen Gerät app MQTT verwenden.

In diesem Lernprogramm erfahren Sie, wie in:

- Erstellen einer Back-End-app, die ein Gerät Ziel und ein simuliertes Gerät, das seinen Connectivity-Kanal als eine *Eigenschaft gemeldeten* auf dem Gerät und Berichte *Kategorien* hinzugefügt.
- Abfrage Geräte aus der Back-End-app Verwenden von Filtern auf Kategorien und Eigenschaften, die zuvor erstellt.


<!-- images -->
[img-twin]: media/iot-hub-selector-twin-get-started/twin.png

<!-- links -->
[lnk-query]: ../articles/iot-hub/iot-hub-devguide-query-language.md
[lnk-twins]: ../articles/iot-hub/iot-hub-devguide-device-twins.md
[lnk-d2c]: ../articles/iot-hub/iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: ../articles/iot-hub/iot-hub-mqtt-support.md
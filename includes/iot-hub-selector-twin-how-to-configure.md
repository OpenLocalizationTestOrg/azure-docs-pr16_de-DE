> [AZURE.SELECTOR]
- [Node.js](../articles/iot-hub/iot-hub-node-node-twin-how-to-configure.md)
- [C#](../articles/iot-hub/iot-hub-csharp-node-twin-how-to-configure.md)

## <a name="introduction"></a>Einführung

[Erste]Schritte mit IoT Hub im Vergleich[lnk-twin-tutorial], Sie haben gelernt, wie Gerät Metadaten Ihrer Lösung Back-End mithilfe von *Kategorien*, Bericht Gerät Bedingungen aus einer Gerät app mit *gemeldeten Eigenschaften*festlegen und diese Informationen mithilfe einer SQL-ähnliche Sprache Abfragen.

In diesem Lernprogramm erfahren Sie, wie mithilfe der das Ziel des *gewünschten Eigenschaften* zusammen mit den *Eigenschaften gemeldet*, Gerät apps Remote konfigurieren. Genauer, in diesem Lernprogramm erfahren, wie das Ziel gemeldet und gewünschte Eigenschaften eine Konfiguration mit mehreren Schritte der anwendungseinstellung für ein Gerät aktivieren, und sorgen Sie die Transparenz an der Lösung Back-End über den Status dieser Vorgang alle Geräte.

Auf hoher Ebene resultiert dieses Lernprogramms die *gewünschten Statusmuster* für die Verwaltung von Gerät aus. Die Grundidee dieses Musters besteht darin, die Lösung Back-End Geben Sie den gewünschten Status für die verwaltete Geräte, statt Sendens von bestimmter Befehle – haben. Dadurch wird das Gerät Computersysteme einrichten die beste Methode zum Erreichen des gewünschten Status (sehr wichtig in IoT Szenarien, in denen bestimmte Gerät Bedingungen die Möglichkeit beeinträchtigen, bestimmte Befehle sofort ausführen), während der aktuellen Status und mögliche Fehler ständig an die Back-End reporting. Das gewünschten Statusmuster ist entscheidend für die Verwaltung von großen Mengen von Geräten, da es Back-End volle Transparenz des Status des Konfigurationsprozesses auf allen Geräten haben ermöglicht.
Finden Sie weitere Informationen über die Rolle des gewünschten Status Musters, bei Gerät-Verwaltung in [Übersicht Azure IoT Hub Gerätemanagement][lnk-dm-overview].

> [AZURE.NOTE] In Szenarien, in dem Geräte auf Weitere interaktive Weise (Aktivieren eines Lüfters in einer app benutzergesteuerte) gesteuert werden, erwägen Sie [Cloud-zu-Gerät Methoden][lnk-methods].

In diesem Lernprogramm folgt die Anwendung Back-End-Änderungen die Konfiguration werden von einem Zielgerät und als Ergebnis, das Gerät app mehreren Schritten zum Anwenden einer Aktualisierung der Konfiguration (z. B. Software Modul neu gestartet werden muss), die in diesem Lernprogramm mit einer einfachen Verzögerung simuliert).

Die Back-End speichert die Konfiguration in das Gerät und die gewünschten Eigenschaften wie folgt aus:

        {
            ...
            "properties": {
                ...
                "desired": {
                    "telemetryConfig": {
                        "configId": "{id of the configuration}",
                        "sendFrequency": "{config}"
                    }
                }
                ...
            }
            ...
        }

> [AZURE.NOTE] Da Konfigurationen komplexe Objekte werden können, werden diese normalerweise eindeutige Ids zugewiesen (Hashes oder [GUIDs][lnk-guid]) in den Überblick behalten ihre Vergleiche sind möglich.

Die app Gerät meldet seine aktuelle Konfiguration spiegeln die gewünschte Eigenschaft **TelemetryConfig** in den gemeldeten Eigenschaften:

        {
            "properties": {
                ...
                "reported": {
                    "telemetryConfig": {
                        "changeId": "{id of the current configuration}",
                        "sendFrequency": "{current configuration}",
                        "status": "Success",
                    }
                }
                ...
            }
        }

Beachten Sie, wie die gemeldeten **TelemetryConfig** eine weitere Eigenschaft **Status**verwendet, um den Status der Konfiguration Aktualisierungsvorgang melden hat.

Wenn eine neue gewünschte Konfiguration eingeht, meldet die Gerät app eine ausstehende Konfiguration durch Ändern der Informationen:

        {
            "properties": {
                ...
                "reported": {
                    "telemetryConfig": {
                        "changeId": "{id of the current configuration}",
                        "sendFrequency": "{current configuration}",
                        "status": "Pending",
                        "pendingConfig": {
                            "changeId": "{id of the pending configuration}",
                            "sendFrequency": "{pending configuration}"
                        }
                    }
                }
                ...
            }
        }

Klicken Sie dann zu einem späteren Zeitpunkt meldet die app Gerät den Erfolg der Fehler bei diesem Vorgang durch die oben angegebenen Eigenschaft zu aktualisieren.
Beachten Sie, wie die Back-End zu einem beliebigen Zeitpunkt, um den Status des Konfigurationsprozesses über alle Geräte Abfragen können, ist.

In diesem Lernprogramm erfahren Sie, wie in:

- Erstellen Sie ein simuliertes Gerät, empfängt Konfiguration Updates vom Back-End und den Aktualisierungsvorgang Konfiguration mehrere Updates als *gemeldete Eigenschaften* Berichte.
- Erstellen einer Back-End-app, die so aktualisiert, dass die gewünschte Konfiguration von einem Gerät aus, und klicken Sie dann in der Konfiguration Aktualisierungsvorgang Abfragen an.

<!-- links -->

[lnk-methods]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
[lnk-dm-overview]: ../articles/iot-hub/iot-hub-device-management-overview.md
[lnk-twin-tutorial]: ../articles/iot-hub/iot-hub-node-node-twin-getstarted.md
[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier
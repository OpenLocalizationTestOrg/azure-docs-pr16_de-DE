> [AZURE.SELECTOR]
- [Linux](../articles/iot-hub/iot-hub-linux-gateway-sdk-simulated-device.md)
- [Windows](../articles/iot-hub/iot-hub-windows-gateway-sdk-simulated-device.md)

Diese exemplarische Vorgehensweise der [simulierten Gerät Cloud hochladen Beispiel] veranschaulicht, wie das [Azure IoT Gateway SDK] [ lnk-sdk] Gerät Cloud werden an IoT Verteiler von simulierten Geräten senden.

Diese exemplarische Vorgehensweise umfasst:

1. **Architektur**: wichtige Architektur Informationen im Beispiel eines simulierten Gerät Cloud hochladen.

2. **Erstellen und Ausführen**: die erforderlichen Schritte zum Erstellen und Ausführen der Stichprobe.

## <a name="architecture"></a>Architektur

Im Beispiel simulierten Gerät Cloud hochladen veranschaulicht, wie das SDK verwenden, um ein Gateway beträchtlich erhöhen, werden von simulierten Geräte an einen Hub IoT sendet. Die simulierten Geräten können nicht direkt an IoT Hub verbinden, da:

- Ein Kommunikationsprotokoll IoT Hub verständlich verwenden Sie die Geräte.
- Die Geräte sind nicht intelligentes genug, die ihnen von IoT Hub zugewiesenen Identität merken.

Das Gateway löst diese Probleme für den simulierten Geräten wie folgt:

- Das Gateway versteht Protokoll wird von den simulierten Geräten, empfängt Gerät Cloud werden von den Geräten und leitet die Nachrichten an IoT Verteiler über ein Protokoll vom Hub verstanden.
- Das Gateway speichert Identitäten IoT Hub für die simulierten Geräte und fungiert als Proxy, wenn die simulierten Geräte IoT Hub Nachrichten senden.

Die folgende Abbildung zeigt die Hauptkomponenten der Stichprobe, einschließlich der Gateway-Module:

![][1]


> [AZURE.NOTE] Die Module übergeben nicht Nachrichten direkt an. Die Module veröffentlichen Nachrichten auf eine interne Bank, die die Nachrichten, an die anderen Module mit einem Abonnement Verfahren übermittelt, wie in der folgenden Abbildung gezeigt. Weitere Informationen finden Sie unter [Erste Schritte mit dem IoT Gateway SDK][lnk-gw-getstarted].

### <a name="protocol-ingestion-module"></a>Protokoll Aufnahme-Modul

Dieses Modul ist der Ausgangspunkt für den Abruf von Daten von Geräten, über das Gateway, und klicken Sie in der Cloud. In der Stichprobe führt das Modul vier Aufgaben:

1.  Simuliertes Temperaturdaten erstellt.
    
    Hinweis: Wenn Sie reale Geräte verwendet haben, würde das Modul Daten aus diesen physische Geräte lesen.

2.  Es platziert die simulierten Temperaturdaten in den Inhalt von einer Nachricht an.

3.  Die Nachricht, die simulierten Temperaturdaten enthält, hinzugefügt eine Eigenschaft mit einer gefälschten MAC-Adresse.

4.  Dies ist die Nachricht in der Kette des nächsten Moduls zur Verfügung.

> [AZURE.NOTE] Das **Protokoll X Aufnahme** -Modul im obigen Diagramm heißt **eines simulierten Gerät** im Quellcode.

### <a name="mac-lt-gt-iot-hub-id-module"></a>MAC &lt; - &gt; IoT Hub-ID-Modul

Dieses Modul scannt für Nachrichten, die eine Eigenschaft enthalten, die die MAC-Adresse, die hinzugefügt, indem Sie im Modul Protokoll Erfassung des Geräts simulierten enthält. Findet das Modul eine entsprechende Eigenschaft, die Nachricht eine weitere Eigenschaft mit einem IoT Hub Geräteschlüssel hinzugefügt, und klicken Sie dann bereitgestellt, die Nachricht in das nächste Modul in der Kette. Dies ist, wie im Beispiel IoT Hub Gerät Identitäten mit simulierten Geräten verknüpft. Der Entwickler richtet die Zuordnung zwischen MAC-Adressen und IoT Hub Identitäten manuell als Teil der Modulkonfiguration. 

> [AZURE.NOTE]  In diesem Beispiel verwendet eine MAC-Adresse als eine eindeutige Geräte-ID. und es mit einer IoT Hub Gerät Identität abgeglichen werden. Sie können jedoch eigene Modul schreiben, das einen anderen eindeutigen Bezeichner verwendet. Angenommen, Sie möglicherweise Geräte mit eindeutigen fortlaufenden Zahlen oder werden Daten, die hat einen eindeutige Gerätenamen darin, mit deren Hilfe zum Bestimmen der Identität IoT Hub Gerät eingebettet.

### <a name="iot-hub-communication-module"></a>IoT Hub Communication-Modul

Dieses Modul dauert Nachrichten mit einem IoT-Hub Gerät Identität zugewiesen, indem Sie die vorherigen Modul und den Nachrichteninhalt IoT Hub über HTTP sendet. HTTP ist eine der drei Protokolle IoT Hub verständlich.

Statt eine Verbindung mit IoT Hub für jedes simulierten Gerät öffnen, dieses Modul öffnet eine einzelne HTTP-Verbindung des Gateways auf dem Hub IoT und multiplexes Verbindungen aus allen simulierten Geräten über diese Verbindung. Dadurch wird ein einzelnes Gateway Verbindung viele weitere Geräten simulierten oder auf andere Weise, als wäre möglich, wenn sie eine eindeutige Verbindung für jedes Gerät geöffnet haben.

![][2]


<!-- Images -->
[1]: media/iot-hub-gateway-sdk-simulated-selector/image1.png
[2]: media/iot-hub-gateway-sdk-simulated-selector/image2.png

<!-- Links -->
[Beispiel für den simulierten Gerät Cloud hochladen]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/sample_simulated_device_cloud_upload.md
[lnk-sdk]: https://github.com/Azure/azure-iot-gateway-sdk
[lnk-gw-getstarted]: ../articles/iot-hub/iot-hub-linux-gateway-sdk-get-started.md
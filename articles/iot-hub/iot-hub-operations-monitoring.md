<properties
 pageTitle="Überwachung IoT Hub von Vorgängen"
 description="Eine Übersicht über Azure IoT Hub Vorgänge Überwachung, dass Sie den Status von Vorgängen auf Ihre IoT Hub in Echtzeit überwachen."
 services="iot-hub"
 documentationCenter=""
 authors="nberdy"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/11/2016"
 ms.author="nberdy"/>

# <a name="introduction-to-operations-monitoring"></a>Einführung in die Überwachung von Vorgängen

Überwachung IoT Hub von Vorgängen können Sie den Status von Vorgängen auf Ihre IoT Hub in Echtzeit zu überwachen. IoT Hub verfolgt Ereignisse über mehrere Kategorien des Felds Vorgänge und können Sie in das Senden von Ereignissen aus einer oder mehreren Kategorien an einen Endpunkt Ihrer IoT-Hub für die Verarbeitung ablehnen. Überwachen Sie die Daten auf Fehler oder komplexere Verarbeitung basierend auf Datenmustern einrichten können.

IoT Hub überwacht fünf Kategorien von Ereignissen:

- Gerät Identität Vorgänge
- Gerät werden
- Cloud-zu-Gerät-Befehle
- Verbindungen
- Datei hochgeladen wird

## <a name="how-to-enable-operations-monitoring"></a>So aktivieren Sie die Überwachung von Vorgängen

1. Erstellen Sie einen IoT-Hub an. Anweisungen finden Sie unter So erstellen Sie einen IoT Hub in das [Erste Schritte] [ lnk-get-started] Guide.

2. Öffnen Sie Ihre IoT Hub Falz. Klicken Sie auf **Vorgänge für die Überwachung**, von dort aus.

    ![][1]

3. Wählen Sie die überwachen Kategorien aus, das, die Sie möchten Sie überwachen, und klicken Sie dann auf **Speichern**. Die Ereignisse sind für das Lesen aus den Ereignis-Hub kompatiblen Endpunkt aufgeführt, die in den **Einstellungen Überwachung**verfügbar. Der Endpunkt IoT Hub heißt `messages/operationsmonitoringevents`.

    ![][2]

## <a name="event-categories-and-how-to-use-them"></a>Ereigniskategorien und deren Verwendung

Jede Kategorie Spuren Überwachung von Vorgängen eine andere Art von Interaktion mit IoT Hub und jede Kategorie Überwachung über ein Schema verfügt, die Strukturierung von Ereignisse in dieser Kategorie definiert.

### <a name="device-identity-operations"></a>Gerät Identität Vorgänge

Die Kategorie Gerät Identität Operationen werden Fehler, die auftreten, wenn Sie versuchen, erstellen, aktualisieren oder Löschen einen Eintrag in der IoT-Hub Identität Registrierung. Nachverfolgen von dieser Kategorie eignet sich für die Bereitstellung von Szenarien.

    {
        "time": "UTC timestamp",
         "operationName": "create",
         "category": "DeviceIdentityOperations",
         "level": "Error",
         "statusCode": 4XX,
         "statusDescription": "MessageDescription",
         "deviceId": "device-ID",
         "durationMs": 1234,
         "userAgent": "userAgent",
         "sharedAccessPolicy": "accessPolicy"
    }

### <a name="device-telemetry"></a>Gerät werden

Die Gerät werden Kategorie verfolgt Fehler, die bei dem IoT Hub und stehen im Zusammenhang mit der Verkaufspipeline werden. Dieser Kategorie gehören Fehler, die auftreten, wenn Ereignisse zu werden (wie etwa begrenzungsebene) senden und empfangen werden Ereignisse (z. B. nicht autorisierte Leser). Beachten Sie, dass diese Kategorie Fehlern, die auf dem Gerät selbst ausgeführten Code entstehen erfassen kann nicht.

    {
         "messageSizeInBytes": 1234,
         "batching": 0,
         "protocol": "Amqp",
         "authType": "{\"scope\":\"device\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
         "time": "UTC timestamp",
         "operationName": "ingress",
         "category": "DeviceTelemetry",
         "level": "Error",
         "statusCode": 4XX,
         "statusType": 4XX001,
         "statusDescription": "MessageDescription",
         "deviceId": "device-ID",
         "EventProcessedUtcTime": "UTC timestamp",
         "PartitionId": 1,
         "EventEnqueuedUtcTime": "UTC timestamp"
    }

### <a name="cloud-to-device-commands"></a>Cloud-zu-Gerät-Befehle

Die Cloud-zu-Gerät Befehle Kategorie verfolgt Fehler, die bei dem IoT Hub und stehen im Zusammenhang mit dem Gerät Befehl Verkaufspipeline. Diese Kategorie umfasst Fehler beim Senden von Befehlen (z. B. nicht autorisierte Absender), Befehle (beispielsweise Übermittlung zählen überschritten) empfangen und Empfangen von Feedback Befehl (z. B. Feedback abgelaufen). Diese Kategorie keine Fehler auf einem Gerät ab, die einen Befehl nicht ordnungsgemäß verarbeitet, wenn der Befehl erfolgreich übermittelt wurde.

    {
         "messageSizeInBytes": 1234,
         "authType": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
         "deliveryAcknowledgement": 0,
         "protocol": "Amqp",
         "time": " UTC timestamp",
         "operationName": "ingress",
         "category": "C2DCommands",
         "level": "Error",
         "statusCode": 4XX,
         "statusType": 4XX001,
         "statusDescription": "MessageDescription",
         "deviceId": "device-ID",
         "EventProcessedUtcTime": "UTC timestamp",
         "PartitionId": 1,
         "EventEnqueuedUtcTime": “UTC timestamp"
    }

### <a name="connections"></a>Verbindungen

Die Kategorie Verbindungen verfolgt Fehler beim Geräte verbinden oder von einem IoT-Hub Trennen an. Nachverfolgen von dieser Kategorie eignet versucht, eine nicht autorisierte Verbindung identifizieren und zum Nachverfolgen, wenn eine Verbindung für Geräte in Bereiche beeinträchtigt Konnektivität verloren.

    {
         "durationMs": 1234,
         "authType": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
         "protocol": "Amqp",
         "time": " UTC timestamp",
         "operationName": "deviceConnect",
         "category": "Connections",
         "level": "Error",
         "statusCode": 4XX,
         "statusType": 4XX001,
         "statusDescription": "MessageDescription",
         "deviceId": "device-ID"
    }

### <a name="file-uploads"></a>Datei hochgeladen wird

Die Datei hochladen Kategorie verfolgt Fehler, die bei dem IoT Hub und stehen im Zusammenhang mit der Funktion zum Hochladen. Diese Kategorie enthält Fehler, die mit dem SAS URI (z. B., wenn es abläuft, bevor ein Gerät den Hub an einem Upload abgeschlossen benachrichtigt) auftreten, fehlgeschlagene Uploads gemeldet, indem Sie das Gerät, und wenn Sie eine Datei während der Erstellung von IoT Hub Benachrichtigung Nachricht nicht im Speicher gefunden wird. Beachten Sie, dass diese Kategorie Fehler abzufangen nicht möglich, die direkt ausgeführt werden, während das Gerät eine Datei auf Speicher hochgeladen wird.

    {
         "authType": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
         "protocol": "HTTP",
         "time": " UTC timestamp",
         "operationName": "ingress",
         "category": "fileUpload",
         "level": "Error",
         "statusCode": 4XX,
         "statusType": 4XX001,
         "statusDescription": "MessageDescription",
         "deviceId": "device-ID",
         "blobUri": "http//bloburi.com",
         "durationMs": 1234
    }

## <a name="next-steps"></a>Nächste Schritte

Die Funktionen von IoT Hub weiteren kann, finden Sie unter:

- [Leitfaden für Entwickler][lnk-devguide]
- [Ein Gerät mit dem IoT Gateway SDK simulieren][lnk-gateway]

<!-- Links and images -->
[1]: media/iot-hub-operations-monitoring/enable-OM-1.png
[2]: media/iot-hub-operations-monitoring/enable-OM-2.png

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[lnk-diagnostic-metrics]: iot-hub-metrics.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-dr]: iot-hub-ha-dr.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
<properties
 pageTitle="Leitfaden für Entwickler - Datei hochladen | Microsoft Azure"
 description="Azure IoT Hub Developer Guide - Hochladen von Dateien auf einem Gerät mit IoT Hub"
 services="iot-hub"
 documentationCenter=".net"
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="dobett"/>

# <a name="upload-files-from-a-device"></a>Hochladen von Dateien auf einem Gerät

## <a name="overview"></a>(Übersicht)

Entsprechend den Details in die [Endpunkte IoT Hub] [ lnk-endpoints] Artikel Geräte können initiieren Dateiuploads durch Senden einer Benachrichtigung über einen Endpunkt Gerät zugänglichen (**/devices/ {Geräte-ID} / Dateien**).  Wenn ein Gerät IoT-Hub von einem Upload abgeschlossen informiert, generiert IoT Hub Benachrichtigungen über Uploads, die Sie über einen Endpunkt Dienst zugänglichen (**/messages/servicebound/filenotifications**) als Nachrichten empfangen können.

Statt aushandeln Nachrichten über IoT Hub selbst fungiert IoT Hub stattdessen als Verteiler zu einem zugeordneten Speicher Azure-Konto. Ein Gerät fordert ein Speicher Token aus, die speziell für die Datei ist, die das Gerät hochladen möchte IoT-Hub an. Das Gerät verwendet den SAS-URI zum Hochladen der Datei auf Speichern, und nach Abschluss des Uploads sendet das Gerät eine Benachrichtigung der Fertigstellung an IoT Hub. IoT Hub stellt sicher, dass die Datei hochgeladen wurde, und klicken Sie dann den neuen Dienst zugänglichen Datei Benachrichtigung per Endpunkt eine Datei hochladen Benachrichtigung hinzugefügt.

Bevor Sie eine Datei in IoT-Hub auf einem Gerät hochladen, müssen Sie Ihre Hub durch [Zuordnen einer Azure-Speicher] konfigurieren[ lnk-associate-storage] zu berücksichtigen.

Ihr Gerät kann dann [einen Upload Initialisierung] [ lnk-initialize] , und klicken Sie dann [Benachrichtigen IoT Hub] [ lnk-notify] Abschluss des Uploads. Optional, wenn ein Gerät IoT Hub informiert, dass der Upload abgeschlossen ist, der Dienst kann Generieren einer [Benachrichtigung][lnk-service-notification].

### <a name="when-to-use"></a>Verwenden von

Verwenden Sie dieses Feature IoT Hub aus, wenn Sie zum Hochladen einer Datei auf einem Gerät mit Ihrem Back-End-Dienst Senden einer Nachricht durch IoT Hub müssen.

## <a name="associate-an-azure-storage-account-with-iot-hub"></a>Ordnen Sie ein Konto Azure-Speicher IoT-Hub

Wenn die Datei hochladen Funktionalität verwenden möchten, müssen Sie zuerst ein Konto Azure-Speicher an den IoT Hub verknüpfen. Sie können dafür entweder über das [Azure-Portal][lnk-management-portal], oder programmgesteuert über den [Anbieter für Ressourcen IoT Hub REST-APIs][lnk-resource-provider-apis]. Nachdem Sie Ihre IoT Hub ein Konto Azure-Speicher zugeordnet haben, gibt der Dienst einen SAS URI an ein Gerät, wenn das Gerät die Anforderung einer Datei hochladen initiiert.

> [AZURE.NOTE] Die [Azure IoT Hub SDKs] [ lnk-sdks] automatisch verarbeiten der SAS URI abrufen, die Datei hochladen und benachrichtigen IoT-Hub von einem Upload abgeschlossen.

## <a name="initialize-a-file-upload"></a>Initialisierung einer Datei hochladen

IoT Hub weist einen Endpunkt speziell für Geräte so fordern Sie ein SAS-URI für Speicher zum Hochladen einer Datei an. Initiiert das Gerät während des Datei-Uploads per einen Beitrag an den Hub IoT am `{iot hub}.azure-devices.net/devices/{deviceId}/files` mit den folgenden JSON-Text:

```
{
    "blobName": "{name of the file for which a SAS URI will be generated}"
}
```

IoT Hub gibt die folgenden Werte, die das Gerät verwendet, um die Datei hochzuladen:

```
{
    "correlationId": "somecorrelationid",
    "hostname": "contoso.azure-devices.net",
    "containerName": "testcontainer",
    "blobName": "test-device1/image.jpg",
    "sasToken": "1234asdfSAStoken"
}
```

### <a name="deprecated-initialize-a-file-upload-with-a-get"></a>Veraltete: Initialisierung Dateiuploads mit einer abrufen

> [AZURE.NOTE] In diesem Abschnitt werden nicht mehr unterstützter Funktionalität für so einen SAS URI von IoT Hub zu erhalten. Verwenden Sie die oben beschriebene POST-Methode.

IoT Hub verfügt über zwei REST-Endpunkte zur Unterstützung von Dateiuploads, eine der SAS-URI für den Speicher und die andere den IoT-Hub an einem Upload abgeschlossen benachrichtigen abgerufen. Initiiert das Gerät Upload-Vorgang durch Senden einen GET an den Hub IoT bei `{iot hub}.azure-devices.net/devices/{deviceId}/files/{filename}`. Hub gibt einen SAS URI bestimmte die Datei hochgeladen werden und ein Korrelations-ID ein, nach dem Abschluss des Uploads verwendet werden soll.

## <a name="notify-iot-hub-of-a-completed-file-upload"></a>Benachrichtigen der fertigen Dateiuploads IoT-Hub

Das Gerät ist für das Hochladen der Datei auf den Azure-Speicher SDKs mit Speicher verantwortlich. Nach Abschluss des Uploads, sendet das Gerät einen Beitrag an den Hub IoT am `{iot hub}.azure-devices.net/devices/{deviceId}/files/notifications` mit den folgenden JSON-Text:

```
{
    "correlationId": "{correlation ID received from the initial request}",
    "isSuccess": bool,
    "statusCode": XXX,
    "statusDescription": "Description of status"
}
```

Der Wert der `isSuccess` ist ein boolescher darstellt, und zwar unabhängig davon, ob die Datei erfolgreich hochgeladen wurde. Der Statuscode für `statusCode` wird der Status für den Upload der Datei an den Speicher und die `statusDescription` entspricht der `statusCode`.

## <a name="reference-topics"></a>Verweis Themen:

Die folgenden Verweis Themen bieten weitere Informationen zum Hochladen von Dateien auf einem Gerät.

## <a name="file-upload-notifications"></a>Datei hochladen Benachrichtigungen

Wenn ein Gerät wird eine Datei hochgeladen und IoT Hub der Upload Fertigstellung benachrichtigt, erstellt der Dienst optional eine Benachrichtigung, die den Namen und den Speicherort der Datei enthält.

Wie in [Endpunkte]erläutert[lnk-endpoints], IoT Hub stellt Datei hochladen Benachrichtigungen über einen Endpunkt Dienst zugänglichen (**/messages/servicebound/fileuploadnotifications**) als Nachrichten. Die empfangen Semantik für Datei hochladen Benachrichtigungen sind die gleichen wie bei Cloud-zu-Gerät Nachrichten und die gleichen [Nachricht Lebenszyklus]haben[lnk-lifecycle]. Jede Nachricht, die aus der Datei hochladen Benachrichtigung Endpunkt abgerufen wird eine JSON-Eintrag mit den folgenden Eigenschaften:

| Eigenschaft | Beschreibung |
| -------- | ----------- |
| EnqueuedTimeUtc | Zeitstempel, der angibt, wann die Benachrichtigung erstellt wurde. |
| Geräte-ID | **Geräte-ID** des Geräts der die Datei hochgeladen. |
| BlobUri | URI der hochzuladenden Datei. |
| BlobName | Name der hochzuladenden Datei. |
| LastUpdatedTime | Zeitstempel, der angibt, wann die Datei zuletzt aktualisiert wurde. |
| BlobSizeInBytes | Die Größe der hochzuladenden Datei. |

**Beispiel**. Dies ist ein Beispiel für Textkörper einer Benachrichtigung für Datei hochladen.

```
{
    "deviceId":"mydevice",
    "blobUri":"https://{storage account}.blob.core.windows.net/{container name}/mydevice/myfile.jpg",
    "blobName":"mydevice/myfile.jpg",
    "lastUpdatedTime":"2016-06-01T21:22:41+00:00",
    "blobSizeInBytes":1234,
    "enqueuedTimeUtc":"2016-06-01T21:22:43.7996883Z"
}
```

## <a name="file-upload-notification-configuration-options"></a>Datei hochladen Konfiguration Benachrichtigungsoptionen

Jede IoT Hub stellt die folgenden Konfigurationsoptionen für Datei hochladen Benachrichtigungen zur Verfügung:

| Eigenschaft | Beschreibung | Bereich und Standardwert |
| -------- | ----------- | ----------------- |
| **enableFileUploadNotifications** | Steuert, ob die Datei hochladen Benachrichtigungen in der Datei Benachrichtigungen Endpunkt geschrieben werden. | Bool. Standard: True. |
| **fileNotifications.ttlAsIso8601** | Standard-TTL für Benachrichtigungen für Datei hochladen. | ISO_8601 Intervall nach Zeitphasen bis zum 48 H (minimale 1 Minute). Standard: 1 Stunde. |
| **fileNotifications.lockDuration** | Sperren Sie die Dauer für die Datei hochladen Benachrichtigungen Warteschlange. | 5 bis 300 Sekunden (mindestens 5 Sekunden). Standard: 60 Sekunden. |
| **fileNotifications.maxDeliveryCount** | Übermittlung maximale Anzahl für die Datei hochladen Benachrichtigungswarteschlange. | 1 und 100. Standard: 100. |

## <a name="additional-reference-material"></a>Weiteres Referenzmaterial

Andere Verweis in der Developer Guide Themen:

- [IoT Hub Endpunkte] [ lnk-endpoints] beschrieben, die verschiedene Endpunkte, die jeder IoT Hub für Laufzeit und Management Vorgänge macht.
- [Beschränkung und Kontingente] [ lnk-quotas] beschreibt die Kontingente, die beziehen sich auf den Dienst IoT Hub und das Drosselung Verhalten zu erwarten, wenn Sie den Dienst verwenden.
- [IoT Hub Geräte und Dienste SDKs] [ lnk-sdks] Listet die verschiedenen Sprache SDKs Sie eine zu verwenden, wenn Sie das Gerät und der Dienst Clientanwendungen, die Interaktion mit einem IoT Hub entwickeln.
- [Abfragesprache für im Vergleich, Methoden und Aufträge] [ lnk-query] die Abfragesprache Sie Informationen über IoT Hub Ihr Gerät im Vergleich, Methoden und Aufträge abgerufen können werden.
- [Support IoT Hub MQTT] [ lnk-devguide-mqtt] enthält weitere Informationen über die Unterstützung von IoT Hub für das Protokoll MQTT.

## <a name="next-steps"></a>Nächste Schritte

Jetzt Sie zum Hochladen von Dateien von Geräten mit IoT Hub vertraut gemacht haben, können Sie in den folgenden Themen Developer Guide interessiert wenden:

- [Verwalten von Gerät Identitäten IoT Hub][lnk-devguide-identities]
- [Steuern des Zugriffs auf IoT-Hub][lnk-devguide-security]
- [Verwenden Sie Gerät im Vergleich zum Synchronisieren von Zustand und Konfigurationen][lnk-devguide-device-twins]
- [Um eine direkte Methode auf einem Gerät aufzurufen][lnk-devguide-directmethods]
- [Planen von Projekten auf mehreren Geräten][lnk-devguide-jobs]

Wenn Sie einige der in diesem Artikel beschriebenen Konzepte ausprobieren möchten, möglicherweise in den folgenden IoT Hub Lernprogramm interessiert werden:

- [Zum Hochladen von Dateien von Geräten in der Cloud mit IoT-Hub][lnk-fileupload-tutorial]

[lnk-resource-provider-apis]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-management-portal]: https://portal.azure.com
[lnk-fileupload-tutorial]: iot-hub-csharp-csharp-file-upload.md
[lnk-associate-storage]: iot-hub-devguide-file-upload.md#associate-an-azure-storage-account-with-iot-hub
[lnk-initialize]: iot-hub-devguide-file-upload.md#initialize-a-file-upload
[lnk-notify]: iot-hub-devguide-file-upload.md#notify-iot-hub-of-a-completed-file-upload
[lnk-service-notification]: iot-hub-devguide-file-upload.md#file-upload-notifications
[lnk-lifecycle]: iot-hub-devguide-messaging.md#message-lifecycle

[lnk-devguide-identities]: iot-hub-devguide-identity-registry.md
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md

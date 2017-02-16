<properties
 pageTitle="Leitfaden für Entwickler – Aufträge | Microsoft Azure"
 description="Azure IoT Hub Entwicklertools Leitfaden – an Ihre Hub angeschlossen Planen von Aufträgen auf mehreren Geräten auszuführen"
 services="iot-hub"
 documentationCenter=".net"
 authors="juanjperez"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="juanpere"/>

# <a name="schedule-jobs-on-multiple-devices-preview"></a>Planen von Aufträgen auf mehreren Geräten (Preview)

## <a name="overview"></a>(Übersicht)

Wie vorherigen Artikeln beschrieben, aktiviert Azure IoT Hub eine Reihe von Bausteinen ([Gerät zwei Eigenschaften und Kategorien] [ lnk-twin-devguide] und [Cloud-zu-Gerät Methoden][lnk-dev-methods]).  Aktivieren in der Regel IoT Back-End-Applikationen Gerät Administratoren und Operatoren, um zu aktualisieren und interagieren mit IoT Geräte in Massen und zu einem geplanten Zeitpunkt.  Aufträge schließen die Ausführung der Gerät und Updates und C2D Methoden anhand einer Reihe von Geräten jeweils Terminplan.  Beispielsweise verwenden ein Operators eine Back-End-Anwendung, die initiieren möchten, und verfolgen ein Projekt, um eine Reihe von Geräten bei der Erstellung von 43 und Boden 3, die nicht in die Vorgänge Gebäude Unterbrechung wären jeweils neu zu starten.

### <a name="when-to-use"></a>Verwenden von

Erwägen Sie mithilfe von Aufträgen und wann: Lösung Back-end planen und Fortschritte nachzuverfolgen muss eine der folgenden Aktivitäten auf eine Reihe von Gerät:

- Eigenschaften des Geräts gewünscht und aktualisieren
- Aktualisieren von Gerät zwei Kategorien
- C2D Methoden aufrufen

## <a name="job-lifecycle"></a>Position Lebenszyklus

Aufträge werden initiiert von Lösung Back-End und von IoT Hub verwaltet.  Sie können ein Projekt durch einen Dienst zugänglichen URI initiieren (`{iot hub}/jobs/v2/{device id}/methods/<jobID>?api-version=2016-09-30-preview`) und die Abfrage für den Fortschritt auf einem Ausführen des Auftrags durch einen Dienst zugänglichen URI (`{iot hub}/jobs/v2/<jobId>?api-version=2016-09-30-preview`).  Nachdem Sie ein Auftrag initiiert wird, werden automatisch für Projekte Abfragen die Back-End-Anwendung, um den Status der ausgeführten Aufträge zu aktualisieren aktiviert.

> [AZURE.NOTE] Wenn Sie ein Projekt initiieren, Eigenschaftennamen und Werte können nur enthalten US-ASCII-druckbare alphanumerische, außer in den folgenden Satz: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.

## <a name="reference-topics"></a>Verweis Themen:

Die folgenden Bezug Themen bieten weitere Informationen zur Verwendung von Aufträgen.

## <a name="jobs-to-execute-direct-methods"></a>Aufträge auszuführende direkte Methoden

Im folgenden finden Sie die Details der Anforderung HTTP 1.1 zum Ausführen einer [direkten Methode] [ lnk-dev-methods] auf eine Reihe von Geräten mithilfe eines Projekt:

    ```
    PUT /jobs/v2/<jobId>?api-version=2016-09-30-preview
    
    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>

    {
        jobId: '<jobId>',
        type: 'scheduleDirectRequest', 
        cloudToDeviceMethod: {
            methodName: '<methodName>',
            payload: <payload>,                 
            timeoutInSeconds: methodTimeoutInSeconds 
        },
        queryCondition: '<queryOrDevices>', // if the queryOrDevices parameter is a string
        deviceIds: '<queryOrDevices>',      // if the queryOrDevices parameter is an array
        startTime: <jobStartTime>,          // as an ISO-8601 date string
        maxExecutionTimeInSeconds: <maxExecutionTimeInSeconds>        
    }
    ```
    
## <a name="jobs-to-update-device-twin-properties"></a>Aufträge Gerät und Eigenschaften aktualisieren

Im folgenden finden die Details der Anforderung HTTP 1.1 für die Aktualisierung Gerät zwei Eigenschaften mithilfe eines Projekt:

    ```
    PUT /jobs/v2/<jobId>?api-version=2016-09-30-preview
    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>

    {
        jobId: '<jobId>',
        type: 'scheduleTwinUpdate', 
        updateTwin: <patch>                 // Valid JSON object
        queryCondition: '<queryOrDevices>', // if the queryOrDevices parameter is a string
        deviceIds: '<queryOrDevices>',      // if the queryOrDevices parameter is an array
        startTime: <jobStartTime>,          // as an ISO-8601 date string
        maxExecutionTimeInSeconds: <maxExecutionTimeInSeconds>        // format TBD
    }
    ```

## <a name="querying-for-progress-on-jobs"></a>Für den Fortschritt für Projekte Abfragen

Im folgenden finden Sie die Details der Anforderung HTTP 1.1 für die [Abfrage zum Aufträge][lnk-query]:

    ```
    GET /jobs/v2/query?api-version=2016-09-30-preview[&jobType=<jobType>][&jobStatus=<jobStatus>][&pageSize=<pageSize>][&continuationToken=<continuationToken>]
    
    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>
    ```
    
Die ContinuationToken werden aus der Antwort bereitgestellt.  

## <a name="jobs-properties"></a>Eigenschaften der Aufträge

Im folgenden finden eine Liste der Eigenschaften und entsprechenden Beschreibungen aufgeführt, die für stellen oder Auftragsergebnisse bei der Abfrage verwendet werden können.

| Eigenschaft | Beschreibung |
| -------------- | -----------------|
| **Auftrags-IDs** | Anwendung bereitgestellt ID für das Projekt. |
| **Startzeit** | Anwendung bereitgestellt Startzeit (ISO 8601) für das Projekt. |
| **Endzeit** | IoT Hub bereitgestellten Datum (ISO 8601) für die Position der Fertigstellung. Gültig nur nach der Auftrag den 'fertigen' Status erreicht. | 
| **Typ** | Typen von Aufträgen: |
| | **ScheduledUpdateTwin**: ein Projekt verwendet, um eine Reihe von Eigenschaften und gewünscht oder Kategorien zu aktualisieren. |
| | **ScheduledDeviceMethod**: ein Projekt verwendet, um eine Gerät-Methode für eine Reihe von zwei aufzurufen. |
| **Status** | Aktuellen Status des Projekts. Mögliche Werte für Status: |
| | **Ausstehend** : geplant und Empfang vom Auftragsdienst bereit. |
| | **Geplante** : nach einer Uhrzeit in der Zukunft geplant. |
| | **ausgeführt** : aktuell aktive Position. |
| | **abgebrochen** : Auftrag wurde abgebrochen. |
| | **Fehler** : Fehler beim Auftrag. |
| | **abgeschlossen** : Job abgeschlossen hat. |
| **deviceJobStatistics** | Statistik zur Ausführung des Projekts. |

Bei der Vorschau steht das Objekt DeviceJobStatistics erst nach der Auftrag abgeschlossen ist.

| Eigenschaft | Beschreibung |
| -------------- | -----------------|
| **deviceJobStatistics.deviceCount** | Anzahl der Geräte im Auftrag. |
| **deviceJobStatistics.failedCount** | Anzahl der Geräte, in dem der Auftrag fehlgeschlagen ist. |
| **deviceJobStatistics.succeededCount** | Anzahl der Geräte, die der Auftrag erfolgreich wurden. |
| **deviceJobStatistics.runningCount** | Anzahl der Geräte, die aktuell den Auftrag ausgeführt werden. |
| **deviceJobStatistics.pendingCount** | Anzahl der Geräte, die zum Ausführen des Auftrags ausstehen. |


### <a name="additional-reference-material"></a>Weiteres Referenzmaterial

Andere Verweis in der Developer Guide Themen:

- [IoT Hub Endpunkte] [ lnk-endpoints] beschrieben, die verschiedene Endpunkte, die jeder IoT Hub für Laufzeit und Management Vorgänge macht.
- [Beschränkung und Kontingente] [ lnk-quotas] beschreibt die Kontingente, die beziehen sich auf den Dienst IoT Hub und das Drosselung Verhalten zu erwarten, wenn Sie den Dienst verwenden.
- [IoT Hub Geräte und Dienste SDKs] [ lnk-sdks] Listet die verschiedenen Sprache SDKs Sie eine zu verwenden, wenn Sie das Gerät und der Dienst Clientanwendungen, die Interaktion mit einem IoT Hub entwickeln.
- [Abfragesprache für im Vergleich, Methoden und Aufträge] [ lnk-query] die Abfragesprache Sie Informationen über IoT Hub Ihr Gerät im Vergleich, Methoden und Aufträge abgerufen können werden.
- [Support IoT Hub MQTT] [ lnk-devguide-mqtt] enthält weitere Informationen über die Unterstützung von IoT Hub für das Protokoll MQTT.

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie einige der in diesem Artikel beschriebenen Konzepte ausprobieren möchten, möglicherweise in den folgenden IoT Hub Lernprogramm interessiert werden:

- [Planen und übertragen Aufträge][lnk-jobs-tutorial]

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-jobs-tutorial]: iot-hub-schedule-jobs.md
[lnk-c2d-methods]: iot-hub-c2d-methods.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-get-started-twin]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-devguide]: iot-hub-devguide-device-twins.md

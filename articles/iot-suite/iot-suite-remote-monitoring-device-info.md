<properties
 pageTitle="Gerät Informations-Metadaten in die Überwachung remote-Lösung | Microsoft Azure"
 description="Eine Beschreibung der Azure IoT vorkonfiguriert Lösung remote-Überwachung und seine Architektur."
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
 ms.date="09/12/2016"
 ms.author="dobett"/>

# <a name="device-information-metadata-in-the-remote-monitoring-preconfigured-solution"></a>Gerät Informations-Metadaten in die Überwachung vorkonfiguriert Lösung

Die Überwachung vorkonfigurierten Lösung remote Azure IoT Suite veranschaulicht einen Ansatz für die Verwaltung von Gerätemetadaten. In diesem Artikel werden den Ansatz, den diese Lösung benötigt, damit Sie verstehen können:

- Welche Gerätemetadaten speichert die Lösung.
- Wie die Lösung die Gerätemetadaten verwaltet.

## <a name="context"></a>Kontextmenü

Die remote Überwachung vorkonfigurierte Lösung verwendet [Azure IoT Hub] [ lnk-iot-hub] Ihre Geräte zum Senden von Daten in der Cloud zu aktivieren. IoT Hub enthält ein [Gerät Identität Registrierung] [ lnk-identity-registry] zum Steuern des Zugriffs auf IoT Hub. Die Registrierung IoT Hub Geräts Identität unterscheidet sich von der Fernbedienung Überwachung Lösung-spezifische *Registrierung Geräts* , auf dem Gerät Informationen Metadaten gespeichert. Die Überwachung remote-Lösung verwendet eine [DocumentDB] [ lnk-docdb] Datenbank seiner Registrierung Gerät zum Speichern von Informations-Gerätemetadaten implementiert wird. Die [Microsoft Azure IoT Bezug Architektur] [ lnk-ref-arch] beschreibt die Rolle der Registrierung Gerät in eine normale IoT-Lösung.

> [AZURE.NOTE] Die remote Überwachung vorkonfigurierte Lösung behält die Registrierung des Geräts Identität synchron mit der Registrierung Gerät. Beide Formular mit der gleichen Geräte-Id eindeutig identifizieren von jedem Gerät an Ihre IoT-Hub angeschlossen.

Die [IoT Hub Gerät Management Vorschau] [ lnk-dm-preview] IoT-Hub an, der das Gerät Informationen in diesem Artikel beschriebenen Verwaltungsfunktionen ähneln Features hinzugefügt. Aktuell, werden die remote Überwachung Lösung nur allgemein verfügbar (GA) Features IoT Hub verwendet.

## <a name="device-information-metadata"></a>Gerät Informations-Metadaten

Ein Gerät Informationen Metadaten JSON gespeichertes Dokument in der Datenbank für Geräte Registrierung DocumentDB weist die folgende Struktur:

```
{
  "DeviceProperties": {
    "DeviceID": "deviceid1",
    "HubEnabledState": null,
    "CreatedTime": "2016-04-25T23:54:01.313802Z",
    "DeviceState": "normal",
    "UpdatedTime": null
    },
  "SystemProperties": {
    "ICCID": null
  },
  "Commands": [],
  "CommandHistory": [],
  "IsSimulatedDevice": false,
  "id": "fe81a81c-bcbc-4970-81f4-7f12f2d8bda8"
}
```

- **DeviceProperties**: das Gerät selbst schreibt diese Eigenschaften und das Gerät ist die Zertifizierungsstelle für diese Daten. Andere Eigenschaften zum Beispiel von Gerät sind Hersteller, Modell und Seriennummer. 
- **Geräte-ID**: der eindeutige Geräte-Id. Dieser Wert ist in der Registrierung IoT Hub Geräts Identität gleich.
- **HubEnabledState**: der Status des Geräts IoT Hub. Dieser Wert wird anfangs auf **null** festgelegt, bis das Gerät zuerst eine Verbindung herstellt. Im Portal Lösung ist **ein Nullwert** als Gerät, das gerade "registrierten jedoch nicht vorhanden." dargestellt.
- **CreatedTime**: die Zeit, die das Gerät erstellt wurde.
- **DeviceState**: der Zustand gemeldet, indem Sie das Gerät.
- **UpdatedTime**: die Zeit, die das Gerät über das Portal Lösung zuletzt aktualisiert wurde.
- **SystemProperties**: das Lösung Portal schreibt Systemeigenschaften und das Gerät hat keine Kenntnisse über diese Eigenschaften. Eine Beispiel-System-Eigenschaft wird die **ICCID** aus, wenn die Lösung mit berechtigt ist und bei einer Verbindung zu einem Dienst SIM aktivierten Geräte verwalten.
- **Befehle**: eine Liste der Befehle das Gerät unterstützt. Das Gerät stellt diese Informationen für die Lösung.
- **CommandHistory**: eine Liste der Befehle, die von der remote Überwachung Solution gesendet, um das Gerät und den Status dieser Befehle.
- **IsSimulatedDevice**: eine Kennzeichnung, die ein Gerät als simulierten Gerät an.
- **ID**: der eindeutige DocumentDB Bezeichner für dieses Dokument Gerät.

> [AZURE.NOTE] Geräteinformationen kann ebenfalls sind die Metadaten, um das Gerät zu IoT Hub sendet die telemetrieprotokoll beschreiben. Die Überwachung remote-Lösung verwendet diese Metadaten werden zum Anpassen der Anzeige von [dynamischen werden]in des Dashboards[lnk-dynamic-telemetry].

## <a name="lifecycle"></a>Lebenszyklus

Wenn Sie ein Gerät zuerst im Portal Lösung erstellen, erstellt die Lösung einen Eintrag in der Registrierung Gerät wie zuvor gezeigt. Viele der Informationen ist anfangs Stub und die **HubEnabledState** auf **null**festgelegt ist. Die Lösung erstellt einen Eintrag für das Gerät an dieser Stelle auch in das Gerät Identität Registrierung ein, die damit die Tasten generiert wird, wenn, die das Gerät wird verwendet, um mit IoT Hub authentifizieren.

Wenn Sie ein Gerät zuerst mit der Lösung verbunden ist, wird eine Gerät Informationen-Nachricht gesendet. Diese Gerät Informationsnachricht enthält Geräteeigenschaften wie die Gerätehersteller, Modell und Seriennummer. Eine Gerät Informationsnachricht enthält darüber hinaus eine Liste der Befehle, die das Gerät, einschließlich Informationen zu einem beliebigen Befehlsparametern unterstützt. Wenn die Lösung dieser Nachricht empfängt, wird die Gerät Informations-Metadaten in der Registrierung Gerät aktualisiert.

### <a name="view-and-edit-device-information-in-the-solution-portal"></a>Anzeigen und Bearbeiten von Geräteinformationen im Portal Lösung

Die Geräteliste im Portal Lösung zeigt die folgenden Geräteeigenschaften als Spalten: **Status**, **Geräte-ID**, **Hersteller**, **Modell Zahl**, **fortlaufende Zahl**, **Firmware**, **Plattform**, **Prozessor**und **RAM installiert**. Die Geräteeigenschaften ** **Breite und** ** versorgen den Speicherort in der Bing-Karte auf dem Dashboard. 

![Geräteliste][img-device-list]

Wenn Sie **Bearbeiten** im Bereich **Device-Details** im Portal Lösung klicken, können Sie alle diese Eigenschaften bearbeiten. Bearbeiten dieser Eigenschaften aktualisiert den Eintrag für das Gerät in der Datenbank DocumentDB. Wenn ein Gerät eine aktualisierte Gerät Informationen Nachricht sendet, überschreibt es im Portal Lösung vorgenommenen Änderungen. Sie können die **Geräte-ID**, **Hostname**, **HubEnabledState**, **CreatedTime**, **DeviceState**und **UpdatedTime** Eigenschaften im Portal Lösung nicht bearbeiten, da nur das Gerät Zertifizierungsstelle über diese Eigenschaften enthält.

![Gerät bearbeiten][img-device-edit]

Das Lösung Portal können Sie um ein Gerät aus der Lösung zu entfernen. Wenn Sie ein Gerät entfernen, die Lösung entfernt die Metadaten Gerät Informationen aus der Lösung Gerät Registrierung und den Eintrag Gerät in der Registrierung IoT Hub Geräts Identität. Bevor Sie ein Gerät entfernen können, müssen Sie ihn deaktivieren.

![Gerät entfernen][img-device-remove]

## <a name="device-information-message-processing"></a>Verarbeitung von Gerät Informationen Nachrichten

Gerät Informationsnachrichten von einem Gerät unterscheiden sich von Nachrichten werden. Gerät Informationsnachrichten umfassen Geräteeigenschaften, die Befehle, die, denen auf einem Gerät antworten kann, und alle Befehlsverlauf. IoT-Hub selbst weist keine Kenntnisse über die Metadaten, die in die Nachricht ein Gerät Informationen enthalten und verarbeitet die Nachricht auf die gleiche Weise, die er Gerät-Cloud-Nachrichten verarbeitet. In der remote Überwachung Lösung eine [Azure Stream Analytics] [ lnk-stream-analytics] (ASA) Position liest die Nachrichten aus IoT Hub. Filter für Nachrichten, die enthalten der beruflichen Position der **DeviceInfo** Stream Analytics **"Objekttyp": "DeviceInfo"** und leitet sie an die **EventProcessorHost** Host-Instanz, die in einem Webauftrag ausgeführt wird. Logik in der **EventProcessorHost** -Instanz verwendet die Geräte-Id, um den Eintrag DocumentDB für das gewünschte Gerät suchen und aktualisieren Sie den Datensatz. Der Gerät Registry-Eintrag enthält jetzt Informationen wie Eigenschaften des Geräts, Befehle und Befehle.

> [AZURE.NOTE] Eine Gerät Informationsnachricht ist eine standard-Gerät Cloud-Nachricht. Die Lösung unterscheidet zwischen Gerät Informationen und werden Nachrichten mithilfe von ASA Abfragen.

## <a name="example-device-information-records"></a>Beispiel für Gerät Informationsdatensätze

Die remote Überwachung vorkonfigurierte Lösung verwendet zwei Arten von Gerät Informationen Einträge: Einträge für die simulierten Geräte mit der Lösung und Einträge für die benutzerdefinierte Geräte, das Sie eine mit der Lösung Verbindung bereitgestellt.

### <a name="simulated-device"></a>Simuliertes Gerät

Das folgende Beispiel zeigt den JSON-Gerät Informationen-Eintrag für ein simuliertes Gerät. Dieser Eintrag hat einen Standardwert für **UpdatedTime**, das angibt, dass das Gerät eine **DeviceInfo** -Nachricht an IoT Verteiler gesendet hat. Der Datensatz enthält einige allgemeine Geräteeigenschaften, definiert sechs Befehle unterstützen die simulierten Geräte sowie wurde die Kennzeichnung **IsSimulatedDevice** auf **1**festgelegt.

```
{
  "DeviceProperties": {
    "DeviceID": "SampleDevice001_455",
    "HubEnabledState": true,
    "CreatedTime": "2016-01-26T19:02:01.4550695Z",
    "DeviceState": "normal",
    "UpdatedTime": "2016-06-01T15:28:41.8105157Z",
    "Manufacturer": "Contoso Inc.",
    "ModelNumber": "MD-369",
    "SerialNumber": "SER9009",
    "FirmwareVersion": "1.39",
    "Platform": "Plat-34",
    "Processor": "i3-2191",
    "InstalledRAM": "3 MB",
    "Latitude": 47.583582,
    "Longitude": -122.130622
  },
  "Commands": [
    {
      "Name": "PingDevice",
      "Parameters": null
    },
    {
      "Name": "StartTelemetry",
      "Parameters": null
    },
    {
      "Name": "StopTelemetry",
      "Parameters": null
    },
    {
      "Name": "ChangeSetPointTemp",
      "Parameters": [
        {
          "Name": "SetPointTemp",
          "Type": "double"
        }
      ]
    },
    {
      "Name": "DiagnosticTelemetry",
      "Parameters": [
        {
          "Name": "Active",
          "Type": "boolean"
        }
      ]
    },
    {
      "Name": "ChangeDeviceState",
      "Parameters": [
        {
          "Name": "DeviceState",
          "Type": "string"
        }
      ]
    }
  ],
  "CommandHistory": [],
  "IsSimulatedDevice": 1,
  "Version": "1.0",
  "ObjectType": "DeviceInfo",
  "IoTHub": {
    "MessageId": null,
    "CorrelationId": null,
    "ConnectionDeviceId": "SampleDevice001_455",
    "ConnectionDeviceGenerationId": "635894317219942540",
    "EnqueuedTime": "0001-01-01T00:00:00",
    "StreamId": null
  },
  "SystemProperties": {
    "ICCID": null
  },
  "id": "7101c002-085f-4954-b9aa-7466980a2aaf"
}
```

### <a name="custom-device"></a>Benutzerdefiniertes Gerät

Im folgende Beispiel zeigt den JSON-Gerät Informationsdatensatz für ein benutzerdefiniertes Gerät und **IsSimulatedDevice** Kennzeichnung festlegen **0**hat. Sie können sehen, dass diese benutzerdefinierte Gerät zwei Befehle unterstützt und das Lösung Portal **SetTemperature** Befehl an das Gerät gesendet hat:

```
{
  "DeviceProperties": {
    "DeviceID": "mydevice01",
    "HubEnabledState": true,
    "CreatedTime": "2016-03-28T21:05:06.6061104Z",
    "DeviceState": "normal",
    "UpdatedTime": "2016-06-07T22:05:34.2802549Z"
  },
  "SystemProperties": {
    "ICCID": null
  },
  "Commands": [
    {
      "Name": "SetHumidity",
      "Parameters": [
        {
          "Name": "humidity",
          "Type": "int"
        }
      ]
    },
    {
      "Name": "SetTemperature",
      "Parameters": [
        {
          "Name": "temperature",
          "Type": "int"
        }
      ]
    }
  ],
  "CommandHistory": [
    {
      "Name": "SetTemperature",
      "MessageId": "2a0cec61-5eca-4de7-92dc-9c0bc4211c46",
      "CreatedTime": "2016-06-07T21:05:18.140796Z",
      "Parameters": {
        "temperature": 20
      },
      "UpdatedTime": "2016-06-07T21:05:18.716076Z",
      "Result": "Expired"
    }
  ],
  "IsSimulatedDevice": 0,
  "id": "6184ae0f-2d94-4fbd-91cd-4b193aecc9d1",
  "ObjectType": "DeviceInfo",
  "Version": "1.0",
  "IoTHub": {
    "MessageId": null,
    "CorrelationId": null,
    "ConnectionDeviceId": "SampleCustom",
    "ConnectionDeviceGenerationId": "635947959068246845",
    "EnqueuedTime": "0001-01-01T00:00:00",
    "StreamId": null
  }
}
```

Die nachstehende Abbildung zeigt der Nachricht JSON **DeviceInfo** das Gerät senden, um das Gerät Informations-Metadaten zu aktualisieren:

```
{ "ObjectType":"DeviceInfo",
  "Version":"1.0",
  "IsSimulatedDevice":false,
  "DeviceProperties": { "DeviceID":"mydevice01", "HubEnabledState":true },
  "Commands": [
    {"Name":"SetHumidity", "Parameters":[{"Name":"humidity","Type":"double"}]},
    {"Name":"SetTemperature", "Parameters":[{"Name":"temperature","Type":"double"}]}
  ]
}
```

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie lernen, wie Sie anpassen können, die vorkonfigurierten Lösungen abgeschlossen haben, können Sie einige der anderen Features und Funktionen von den IoT Suite vorkonfiguriert Lösungen durchsuchen:

- [Vorhersagen Wartung vorkonfiguriert Lösung (Übersicht)][lnk-predictive-overview]
- [Häufig gestellte Fragen zu IoT Suite][lnk-faq]
- [IoT Sicherheit von Grund auf][lnk-security-groundup]



<!-- Images and links -->
[img-device-list]: media/iot-suite-remote-monitoring-device-info/image1.png
[img-device-edit]: media/iot-suite-remote-monitoring-device-info/image2.png
[img-device-remove]: media/iot-suite-remote-monitoring-device-info/image3.png

[lnk-iot-hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-identity-registry]: ../iot-hub/iot-hub-devguide-identity-registry.md
[lnk-docdb]: https://azure.microsoft.com/documentation/services/documentdb/
[lnk-ref-arch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
[lnk-stream-analytics]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-dm-preview]: ../iot-hub/iot-hub-device-management-overview.md
[lnk-dynamic-telemetry]: iot-suite-dynamic-telemetry.md

[lnk-predictive-overview]: iot-suite-predictive-overview.md
[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md

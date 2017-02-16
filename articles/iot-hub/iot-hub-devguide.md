<properties
 pageTitle="Leitfaden Themen IoT Hub Entwicklertools | Microsoft Azure"
 description="Azure IoT Hub Entwicklertools Leitfadens, der umfasst IoT Hub Endpunkte, Sicherheit, Gerät Identität Registrierung, Gerätemanagement und messaging"
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

# <a name="azure-iot-hub-developer-guide"></a>Azure IoT Hub Developer guide

Azure IoT-Hub ist ein vollständig verwalteter Dienst, der hilft aktivieren zuverlässige und sichere bidirektionale Kommunikation zwischen mehreren Millionen IoT Geräte und eine Anwendung wieder zu beenden.

Azure IoT Hub bietet Ihnen:

* Sichere Kommunikation mithilfe der Sicherheitsanmeldeinformationen pro Gerät und Steuerelement zugreifen.
* Zuverlässigen Gerät Cloud und Cloud-zu-Gerät hyper-Skala messaging.
* Einfache Device-Konnektivität mit Gerätebibliotheken für die am häufigsten verwendeten Sprachen und Plattformen.

Mit diesem IoT Hub Entwicklertools Leitfaden enthält die folgenden Artikeln:

- [Senden und Empfangen von Nachrichten mit IoT Hub] [ devguide-messaging] beschreibt die messaging Features (Gerät Cloud und Cloud-zu-Gerät), die IoT Hub macht. Der Artikel enthält auch Informationen zu Themen wie Nachrichtenformate, und die Kommunikationsprotokolle unterstützten und die Portnummern, die sie verwenden.
- [Hochladen von Dateien auf einem Gerät] [ devguide-upload] beschreibt, wie Sie Dateien auf einem Gerät hochladen können. Der Artikel enthält auch Informationen zu Themen wie die Benachrichtigungen, dass der Prozess Uplaod senden kann.
- [Gerät Identitäten IoT Hub verwalten] [ devguide-identities] beschrieben, welche Informationen jede IoT-Hub Gerät Identität Registrierung gespeichert und wie Sie zugreifen und diese bearbeiten können.
- [Steuern des Zugriffs auf IoT Hub] [ devguide-security] Sicherheitsmodell zum Gewähren des Zugriffs auf IoT Hub Funktionalität für beide Geräte und cloud-Komponenten verwendet werden. Der Artikel enthält Informationen zur Verwendung von Token und x. 509-Zertifikate und Details über die Berechtigungen, die Sie gewähren können.
- [Verwenden Sie Gerät im Vergleich zum Synchronisieren von Zustand und Konfigurationen] [ devguide-device-twins] beschreibt das *Gerät zwei* Konzept und die Funktionalität, beispielsweise das Synchronisieren von einem Gerät mit dessen Ziel macht. Der Artikel enthält Informationen zu den Daten in einem Gerät und gespeichert.
- [Um eine direkte Methode auf einem Gerät aufzurufen] [ devguide-directmethods] des Lebenszyklus von eine direkte Methode, Informationen zum Aufrufen von Methoden auf einem Gerät aus der Back-End-app und die direkte Methode auf Ihrem Gerät verarbeitet werden.
- [Planen von Aufträgen auf mehreren Geräten] [ devguide-jobs] beschreibt, wie Sie Projekte auf mehreren Geräten planen können. Der Artikel beschreibt, wie Aufträge senden, die als Ausführen einer direkten Methode, Aktualisieren eines Devcie mit einer Devcie zwei Aufgaben ausführen. Es wird beschrieben, wie den Status eines Auftrags Abfrage wird.
- [Verweis - IoT Hub Endpunkte] [ devguide-endpoints] beschrieben, die verschiedene Endpunkte, die jeder IoT Hub für Laufzeit und Management Vorgänge macht. Dieser Artikel beschreibt auch, wie Sie einen Feld Gateway verwenden können, einige Geräte Verbindung zu Ihrer Endpunkte IoT Hub zu aktivieren.
- [Verweis - Abfragesprache für im Vergleich, Methoden und Aufträge] [ devguide-query] die Abfragesprache, die zum Abrufen von Informationen über Ihre Hub zu Ihrem Gerät im Vergleich und Aufträge aktiviert werden.
- [Verweis - Kontingente und begrenzungsebene] [ devguide-quotas] enthält eine Übersicht über die Kontingente festlegen in den Dienst IoT Hub und das Verhalten des Drosselung beständig zu sehen, wenn Sie ein Kontingent überschritten.
- [Verweis - Geräte und Dienste SDKs] [ devguide-sdks] Listen die SDKs können entwickeln Gerät und der Dienst Anwendungen, die mit Ihrem Hub IoT interagieren. Der Artikel enthält Links zu API-Onlinedokumentation.
- [Verweis - Support IoT Hub MQTT] [ devguide-mqtt] enthält ausführliche Informationen zur wie IoT Hub für das MQTT-Protokoll unterstützt. Im Artikel beschreibt die Unterstützung für das MQTT Protokoll zu den SDKs integrierten und enthält Informationen zur Verwendung von des Protokolls MQTT direkt.
- [Glossar] [ devguide-glossary] eine Liste der allgemeinen IoT Hub-bezogene Begriffe.



[devguide-messaging]: iot-hub-devguide-messaging.md
[devguide-upload]: iot-hub-devguide-file-upload.md
[devguide-identities]: iot-hub-devguide-identity-registry.md
[devguide-security]: iot-hub-devguide-security.md
[devguide-device-twins]: iot-hub-devguide-device-twins.md
[devguide-directmethods]: iot-hub-devguide-direct-methods.md
[devguide-jobs]: iot-hub-devguide-jobs.md
[devguide-endpoints]: iot-hub-devguide-endpoints.md
[devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[devguide-query]: iot-hub-devguide-query-language.md
[devguide-sdks]: iot-hub-devguide-sdks.md
[devguide-mqtt]: iot-hub-mqtt-support.md
[devguide-glossary]: iot-hub-devguide-glossary.md


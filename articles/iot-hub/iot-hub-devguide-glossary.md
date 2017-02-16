<properties
 pageTitle="Leitfaden für Entwickler - Glossar | Microsoft Azure"
 description="Ein Glossar allgemeine IoT Hub für Ausdrücke"
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

# <a name="glossary-of-iot-hub-terms"></a>Glossar IoT-Hub

In diesem Artikel werden einige der gängigen Begriffe IoT Hub zugeordnet aufgelistet.

## <a name="advanced-message-queueing-protocol-amqp"></a>Erweiterte Nachricht einfügen in die Warteschlange-Protokoll (AMQP)

[AMQP](https://www.amqp.org/) ist der messaging Protokolle, die IOT Hub für die Kommunikation mit Geräten unterstützt. Weitere Informationen zu IoT Hub unterstützt messaging-Protokolle, finden Sie unter [Senden und Empfangen von Nachrichten mit IoT Hub](iot-hub-devguide-messaging.md).

## <a name="cloud-to-device-c2d"></a>Cloud-zu-Gerät (C2D)

In der Regel verwendet auf Nachrichten von IoT Hub für ein verbundenes Gerät verweisen. Häufig, diese Informationen sind Befehle, die anweisen, das Gerät eine Aktion auszuführen. Weitere Informationen finden Sie unter [Senden und Empfangen von Nachrichten mit IoT Hub](iot-hub-devguide-messaging.md).

## <a name="condition"></a>Bedingung

Verweist auf Gerätestatusinformationen, beispielsweise die Connectivity-Methode derzeit in verwenden, indem Sie eine app Gerät gemeldet. Geräte können auch die Funktionen melden. Sie können die Bedingung und Videofunktionen mit dem Gerät und Abfragen.

## <a name="data-point-message"></a>Datenpunkt Nachricht

Eine Datenpunkt Nachricht ist eine Nachricht Cloud-zu-Gerät, die werden die Daten wie windgeschwindigkeit oder Temperatur enthält.

## <a name="desired-properties"></a>Gewünschten Eigenschaften

Im Kontext Gerät im Vergleich werden die gewünschte Eigenschaften zum Synchronisieren von Gerätekonfiguration oder Bedingung in Verbindung mit den *Eigenschaften gemeldet* verwendet. Gewünschte Eigenschaften können nur festgelegt werden durch die Anwendung wieder beenden und von der app Gerät beobachtet werden. 

## <a name="device-to-cloud-d2c"></a>Gerät Cloud (D2C)

Verweisen auf einem verbundenen Gerät an IoT Hub gesendete Nachrichten verwendet in der Regel. Diese Nachrichten möglicherweise [Datenpunkt](#data-point-message) oder [interaktiv](#interactive-message) Nachrichten. Weitere Informationen finden Sie unter [Senden und Empfangen von Nachrichten mit IoT Hub](iot-hub-devguide-messaging.md).

## <a name="device-identity-registry"></a>Registrierung des Geräts Identität

Der [Registrierung des Geräts Identität](iot-hub-devguide-identity-registry.md) ist der integrierten Bestandteil einer IoT Hub, in dem Informationen zu den einzelnen Geräten zulässig Verbindung mit einem Hub gespeichert.

## <a name="device"></a>Gerät

Im Kontext IoT ist ein Gerät in der Regel eine kleine, eigenständigen Computer, die möglicherweise Daten sammeln oder andere Geräte steuern. Beispielsweise ein Gerät möglicherweise, eine Umgebung Überwachung Gerät oder ein Controller für die Belüftung und düngen Systeme in Ihre.

## <a name="device-twin"></a>Zwei Gerät

Ein [Gerät Ziel](iot-hub-devguide-device-twins.md) ist eine Kopie in der Bedingung und Konfiguration Einstellungen eines physischen Geräts IoT-Hub an. Sie können ein Gerät und zum Verwalten der Konfigurations des physischen Geräts verwenden.

## <a name="direct-method"></a>Direkte Methode

Eine [direkte Methode](iot-hub-devguide-direct-methods.md) ist eine Möglichkeit, eine Methode auslösen auf einem Gerät nicht ausführen, indem Sie eine API auf Ihre IoT-Hub an.

## <a name="event-hub-compatible-endpoint"></a>Ereignis Hub kompatiblen Endpunkt

Wenn Gerät-Cloud-Nachrichten an Ihre IoT Verteiler lesen möchten, können auf Ihre Hub Herstellen einer Verbindung einen Endpunkt mit und Verwenden einer beliebigen Ereignis Hub kompatiblen Methode, um die Nachrichten lesen. Ereignis Hub kompatiblen Methoden enthalten, mit dem Ereignis Hubs SDKs und Azure Stream Analytics.

## <a name="field-gateway"></a>Feld gateway

Ein Feld Gateway ermöglicht Konnektivität für Geräte, die nicht direkt zu IoT Hub herstellen und in der Regel bereitgestellt wird lokal mit Ihren Geräten. Weitere Informationen finden Sie unter [Neuigkeiten Azure IoT Hub?](iot-hub-what-is-iot-hub.md).

## <a name="job"></a>Position

Ihre Lösung Back-End können Aufträge zu planen und Nachverfolgen von Aktivitäten in eine Reihe von Geräten, die mit Ihrem Hub IoT registriert. Aktivitäten gehören Gerät gewünscht und Eigenschaften aktualisieren, aktualisieren Gerät zwei Kategorien und direkte Methoden aufrufen.

## <a name="protocol-gateway"></a>Protokollgateway

Ein Protokollgateway ist in der Regel in der Cloud bereitgestellt und bietet Protokoll Übersetzungsdienste für Geräte Herstellen einer Verbindung mit IoT Hub. Weitere Informationen finden Sie unter [Neuigkeiten Azure IoT Hub?](iot-hub-what-is-iot-hub.md).

## <a name="interactive-message"></a>Interaktive Meldung

Eine interaktive Nachricht ist eine Cloud-zu-Gerät-Nachricht, die in der Anwendung Back-End sofortige eine Aktion auslöst. Ein Gerät möglicherweise beispielsweise eine Erinnerung zu einem Fehler senden, die in einem CRM-System automatisch protokolliert werden sollen.

## <a name="iot-hub"></a>IoT-Hub

IoT-Hub ist ein vollständig verwaltete Azure-Dienst, der zuverlässigen und sicheren bidirektionale Kommunikation zwischen mehreren Millionen IoT Geräte und eine Lösung Back-End ermöglicht. Weitere Informationen finden Sie unter [Neuigkeiten Azure IoT Hub?](iot-hub-what-is-iot-hub.md).

## <a name="iot-suite"></a>IoT Suite

Azure IoT Suite verpackt zusammen mit vorkonfigurierten Lösungen mehrere Azure-Dienste. Diese vorkonfigurierten Lösungen können Sie schnell mit End-to-End-Implementierungen allgemeine IoT Szenarien beginnen. Weitere Informationen finden Sie unter [Neuigkeiten Azure IoT Suite?](../iot-suite/iot-suite-overview.md).

## <a name="job"></a>Position

Eine [Position](iot-hub-devguide-jobs.md) in IoT Hub ermöglicht es Ihnen, Operationen wie eine Firmware Upgrade über mehrere Geräte an Ihrem Hub angeschlossen.

## <a name="mqtt"></a>MQTT

[MQTT](http://mqtt.org/) ist der messaging Protokolle, die IOT Hub für die Kommunikation mit Geräten unterstützt. Weitere Informationen zu IoT Hub unterstützt messaging-Protokolle, finden Sie unter [Senden und Empfangen von Nachrichten mit IoT Hub](iot-hub-devguide-messaging.md).

## <a name="reported-properties"></a>Gemeldete Eigenschaften

In dem Kontext des Geräts im Vergleich werden gemeldete Eigenschaften zusammen mit den *gewünschten Eigenschaften* zum Synchronisieren von Gerätekonfiguration oder Bedingung verwendet. Gemeldete Eigenschaften kann nur von der app Gerät festgelegt werden und lesen und abgefragt, indem Sie die Anwendung Back-End.

## <a name="tags"></a>Kategorien

Im Kontext Devcie im Vergleich sind Kategorien Gerät Metadaten gespeichert und von der Anwendung Back-End in Form eines JSON-Dokuments abgerufen. Tags sind nicht für apps auf einem Gerät sichtbar.

## <a name="system-properties"></a>Systemeigenschaften

In dem Kontext des Geräts im Vergleich Systemeigenschaften sind schreibgeschützt und Informationen zur Verwendung von das Gerät wie letzten Aktivität Zeit und Verbindungsstatus.
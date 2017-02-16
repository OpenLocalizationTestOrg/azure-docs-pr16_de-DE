<properties
 pageTitle="Developer Guide - IoT Hub Endpunkte | Microsoft Azure"
 description="Azure IoT Hub Developer Guide - Referenzinformationen zu IoT Hub Endpunkte"
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

# <a name="reference---iot-hub-endpoints"></a>Verweis - IoT Hub Endpunkte

## <a name="list-of-iot-hub-endpoints"></a>Liste von Endpunkten IoT-Hub

Azure IoT-Hub ist ein mit mehreren Mandanten-Dienst, der seine Funktionalität für verschiedene Akteuren macht. Das folgende Diagramm veranschaulicht die verschiedenen Endpunkte IoT Hub macht verfügbar.

![IoT Hub Endpunkte][img-endpoints]

Im folgenden finden eine Beschreibung für die Endpunkte:

* **Anbieter für Ressourcen**. Der Anbieter für die Ressource IoT Hub weist eine [Azure Ressourcenmanager] [ lnk-arm] Schnittstelle, Azure-Abonnement Besitzer erstellen ermöglicht und Löschen von IoT Hubs und IoT Hub Eigenschaften aktualisieren. IoT Hub Eigenschaften Aufsicht [Sicherheit auf Benutzerebene-Hub Richtlinien][lnk-accesscontrol], im Gegensatz zum Gerät Benutzerebene und funktionsübergreifendes Optionen für messaging Cloud-zu-Gerät und Gerät Cloud. Der Anbieter IoT Hub für Ressourcen können Sie auch so [Exportieren Sie Gerät Identitäten][lnk-importexport].
* **Gerät Identitätsmanagement**. Jede IoT Hub stellt eine Reihe von HTTP (REST) von Endpunkten in Identitäten Gerät zu verwalten (erstellen, abrufen, aktualisieren und löschen). [Gerät Identitäten] [ lnk-device-identities] für Gerät Anmelde- und Access-Steuerelement verwendet werden.
* **Gerät und Management**. Jede IoT Hub stellt eine Reihe von Dienst zugänglichen HTTP (REST) Endpunkt Abfragen und Aktualisieren von [Gerät im Vergleich] [ lnk-twins] (Tags und Eigenschaften aktualisieren).
* **Aufträge Management**. Jede IoT Hub stellt eine Reihe von Dienst zugänglichen HTTP (REST) Endpunkt Abfragen und Verwalten von [Aufträgen][lnk-jobs].
* **Gerät Endpunkte**. Für jedes Gerät in der Registrierung des Geräts Identität bereitgestellt stellt IoT Hub eine Reihe von Endpunkten, die ein Gerät zum Senden und Empfangen von Nachrichten verwenden können:
    - *Gerät-Cloud - Nachrichten senden*. Verwenden Sie diesen Endpunkt [Gerät-Cloud - Nachrichten]senden[lnk-d2c].
    - *Die Cloud-zu-Gerät Nachrichten empfangen*. Ein Gerät verwendet diesen Endpunkt gezielte [Cloud-zu-Gerät Nachrichten]erhalten[lnk-c2d].
    - *Dateiuploads einleiten*. Ein Gerät verwendet diesen Endpunkt einer Azure-Speicher SAS-URI aus IoT Hub zum [Hochladen einer Datei]erhalten[lnk-upload].
    - *Abrufen und aktualisieren und den Eigenschaften*. Ein Gerät verwendet diese Endpunkte dessen [Ziel Gerät]Zugriff auf[lnk-twins]der Eigenschaften.
    - *Empfangen direkten Methoden Anfragen*. Ein Gerät verwendet diese Endpunkte mit [direkten Methoden]anhören[lnk-methods]des Serviceanfragen.

    Diese Grenzwerte werden zur Verfügung gestellt mit [MQTT v3.1.1][lnk-mqtt], HTTP 1.1 und [1.0 AMQP] [ lnk-amqp] Protokolle. Beachten Sie, dass AMQP über [WebSockets] auch steht[ lnk-websockets] Port 443.
    
    Die im Vergleich und Methoden Endpoins stehen nur mit [MQTT v3.1.1][lnk-mqtt].

* **Endpunkte**. Jede IoT Hub verfügt über einen Satz von Endpunkten, die Ihrer Anwendung Back-End zur Kommunikation mit Ihren Geräten verwenden kann. Diese Grenzwerte sind derzeit nur zugänglicher mithilfe der [AMQP] [ lnk-amqp] Protokoll, eine Ausnahme bilden jedoch die Methode aufrufen Endpunkt, die über HTTP 1.1 verfügbar gemacht werden.
    - *Gerät-zu-Cloud-Nachrichten empfangen*. Dieser Endpunkt mit [Azure Ereignis Hubs]kompatibel ist[lnk-event-hubs]. Ein Back-End-Dienst können in die [Cloud-Gerät Nachrichten] lesen[ lnk-d2c] von Ihren Geräten gesendet.
    - *Cloud-zu-Gerät-Nachrichten senden und Empfangen von Übermittlung Danksagungen*. Diese Grenzwerte aktivieren Ihrer Anwendung Back-End zuverlässigen [Cloud-zu-Gerät Nachrichten]senden[lnk-c2d], und die entsprechenden Übermittlung oder Ablauf Danksagungen zu erhalten.
    - *Die Datei Benachrichtigungen empfangen*. Diese messaging Endpunkt können Sie Benachrichtigungen über zu erhalten, wenn Ihre Geräte erfolgreich eine Datei hochladen. 
    - *Direkte Methode aufrufen*. Diesen Endpunkt ermöglicht einen Back-End-Dienst, um eine [direkte Methode] aufzurufen[ lnk-methods] auf einem Gerät.

[IoT Hub APIs und SDKs] [ lnk-sdks] diesem Artikel werden die verschiedenen Optionen, um diese Endpunkte zuzugreifen.

Schließlich ist zu beachten, dass alle IoT Hub Endpunkte für die Verwendung der [TLS] [ lnk-tls] Protokoll sowie kein Endpunkt jemals auf entschlüsselt/unsichere Kanäle bereitgestellt wird.

## <a name="field-gateways"></a>Feld gateways

Befindet sich in einer Lösung IoT ein *Feld Gateway* zwischen Ihren Geräten und Ihrer Endpunkte IoT Hub ein. Diese befindet sich normalerweise in der Nähe der Geräte. Ihre Geräte kommunizieren direkt mit dem Feld Gateway über ein Protokoll unterstützt, indem Sie die Geräte. Das Feld Gateway verbindet an den Endpunkt IoT Hub verwenden ein Protokoll, das von IoT Hub unterstützt wird. Ein Gateway Feld kann sehr spezielle Hardware oder einem low-Power-Software, die das End-to-End-Szenario längere für das Gateway vorgesehenen Computer sein.

Können die [Azure IoT Gateway SDK] [ lnk-gateway-sdk] einen Feld Gateway implementiert wird. Dieses SDK bietet bestimmten Funktionen wie die Möglichkeit, die Kommunikation von mehreren Geräten auf derselben Verbindung IoT Hub bündeln.

## <a name="next-steps"></a>Nächste Schritte

Weitere Themen Bezug Leitfadens Entwicklertools IoT Hub umfassen:

- [Abfragesprache für im Vergleich, Methoden und Aufträge][lnk-devguide-query]
- [Kontingente und Beschränkung][lnk-devguide-quotas]
- [IoT Hub MQTT support][lnk-devguide-mqtt]

[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk

[img-endpoints]: ./media/iot-hub-devguide-endpoints/endpoints.png
[lnk-amqp]: https://www.amqp.org/
[lnk-mqtt]: http://mqtt.org/
[lnk-websockets]: https://tools.ietf.org/html/rfc6455
[lnk-arm]: ../azure-resource-manager/resource-group-overview.md
[lnk-event-hubs]: http://azure.microsoft.com/documentation/services/event-hubs/

[lnk-tls]: https://tools.ietf.org/html/rfc5246


[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-accesscontrol]: iot-hub-devguide-security.md#access-control-and-permissions
[lnk-importexport]: iot-hub-devguide-identity-registry.md#import-and-export-device-identities
[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-device-identities]: iot-hub-devguide-identity-registry.md
[lnk-upload]: iot-hub-devguide-file-upload.md
[lnk-c2d]: iot-hub-devguide-messaging.md#cloud-to-device-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-jobs]: iot-hub-devguide-jobs.md

[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
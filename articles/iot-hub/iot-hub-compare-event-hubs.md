<properties
 pageTitle="Vergleich von Azure IoT-Hub an Azure Ereignis Hubs | Microsoft Azure"
 description="Einen Vergleich der Azure IoT Hub und Azure Ereignis Hubs Dienste hervorheben Funktionsunterschiede und Fällen verwenden."
 services="iot-hub"
 documentationCenter=""
 authors="fsautomata"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="06/06/2016"
 ms.author="elioda"/>

# <a name="comparison-of-azure-iot-hub-and-azure-event-hubs"></a>Vergleich von Azure IoT Hub und Azure Ereignis Hubs

Eine der wichtigsten Anwendungsmöglichkeiten für IoT Hub ist werden von Geräten sammeln. Aus diesem Grund wird IoT Hub häufig mit [Azure Ereignis Hubs][]verglichen. Wie IoT-Hub ist Ereignis Hubs eines Ereignisses processing-Dienst, Ereignis und werden eingehende in der Cloud bei umfangreichen Skalieren mit niedriger Wartezeit und hohe Zuverlässigkeit ermöglicht.

Die Dienste müssen jedoch viele der Unterschiede, die in der nachstehenden Tabelle ausführlich dargestellt werden.

| Bereich | IoT-Hub | Ereignis Hubs |
| ---- | ------- | ---------- |
| Kommunikation Mustern | Ermöglicht das Gerät Cloud und Cloud-zu-Gerät messaging. | Nur ermöglicht Ereignis eingehende (normalerweise für Gerät-Cloud-Szenarien betrachtet). |
| Unterstützung für Geräte-Protokoll | Unterstützt die MQTT, AMQP, AMQP über WebSockets und HTTP. Darüber hinaus IoT Hub funktioniert mit dem [Azure IoT Protokollgateway][lnk-azure-protocol-gateway], eine anpassbare Protokoll Gateway-Implementierung zur Unterstützung von benutzerdefinierten Protokolle. | Unterstützt AMQP, AMQP über WebSockets und HTTP. |
| Sicherheit | Stellt pro Gerät Identität und widerrufen Access-Steuerelement. Finden Sie im [Abschnitt Sicherheit des Handbuchs Entwicklertools IoT Hub]aus. | Ereignis Hubs organisationsweite [Richtlinien freigegebenen Access]bietet[Event Hubs - security], mit eingeschränkter Sperrung unterstützt, über die [Richtlinien des Herausgebers][Event Hubs publisher policies]. IoT Lösungen werden häufig zum Implementieren einer benutzerdefinierten Lösung zur Unterstützung von Anmeldeinformationen pro Gerät und Measures Anti-spoofing erforderlich. |
| Überwachung von Vorgängen | Ermöglicht IoT-Lösungen für eine umfangreiche Menge von Gerät Identität Management und Verbindungsproblemen Ereignisse wie einzelne Gerät Authentifizierungsfehler, begrenzungsebene und ungültiges Format Ausnahmen abonnieren. Diese Ereignisse können Sie Probleme bei der Verbindung auf der Geräteebene einzelner schnell zu erkennen. | Macht nur eine Zusammenfassung der Kennzahlen verfügbar. |
| Skalieren | Zur Unterstützung mehrerer Millionen gleichzeitig verbundenen Geräte optimiert ist. | Eine weitere eingeschränkte Anzahl von gleichzeitigem Verbindungen – bis zu 5.000 AMQP Verbindungen, gemäß [Azure-Dienstbus Kontingente][]können unterstützt werden. Ereignis Hubs ermöglicht andererseits, die Partition für jede gesendete Nachricht angeben. |
| Geräte-SDKs | [Gerät SDKs] bietet[ Azure IoT Hub SDKs] für eine Vielzahl von Plattformen und Sprachen. | Wird für .NET und c unterstützt. Auch bietet AMQP und HTTP-Schnittstellen senden. |
| Hochladen von Dateien | Ermöglicht IoT Lösungen zum Hochladen von Dateien von Geräten in der Cloud. Enthält einen Datei Benachrichtigung Endpunkt für Workflowintegration und einer Kategorie unterstützt das Debuggen Überwachung von Vorgängen. | Anhand eines anfordern Kontrollkästchen Musters manuell anfordern von Dateien aus Geräte und Geräte mit einem Speicherschlüssel für die Transaktion bereitstellen. |

Kurz gesagt selbst ist der einzige Anwendungsfall-Gerät Cloud werden eingehende, bietet IoT Hub einen Dienst, der speziell für die IoT Gerät Konnektivität. Weiterhin wird die für diese Szenarios mit IoT-spezifische Features nutzen zu erweitern. Hubs Ereignis ist für das Ereignis eingehende bei einer umfangreichen, beide im Kontext zwischen Datacenter und innerhalb-Datacenter Szenarien.

Es ist nicht selten verwenden Sie IoT Hub und Ereignis Hubs in der gleichen Lösung – wobei IoT Hub behandelt die Gerät-Cloud-Kommunikation und Ereignis Hubs verarbeitet eingehende späteren Zeitpunkt Ereignis in Echtzeit Verarbeitung TTS.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Planen der Bereitstellung IoT Hub finden Sie unter [Skalierung, HA und DR][lnk-scaling].

Die Funktionen von IoT Hub weiteren kann, finden Sie unter:

- [Leitfaden für Entwickler][lnk-devguide]
- [Ein Gerät mit dem IoT Gateway SDK simulieren][lnk-gateway]

[Azure Ereignis Hubs]: ../event-hubs/event-hubs-what-is-event-hubs.md
[Sicherheitsabschnitt des Handbuchs Entwicklertools IoT-Hub]: iot-hub-devguide-security.md
[Event Hubs - security]: ../event-hubs/event-hubs-authentication-and-security-model-overview.md
[Event Hubs publisher policies]: ../event-hubs/event-hubs-overview.md#common-publisher-tasks
[Azure Dienstbus von Kontingenten]: ../service-bus-messaging/service-bus-quotas.md
[Azure IoT Hub SDKs]: https://github.com/Azure/azure-iot-sdks/blob/master/readme.md
[lnk-azure-protocol-gateway]: iot-hub-protocol-gateway.md

[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md

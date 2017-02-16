<properties
 pageTitle="Azure IoT Hub Skalierung | Microsoft Azure"
 description="Beschreibt, wie Azure IoT Hub skalieren."
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
 ms.date="09/19/2016"
 ms.author="elioda"/>

# <a name="scaling-iot-hub"></a>Anpassungsbereich für IoT-Hub

Azure IoT Hub kann bis zu einem Millionen gleichzeitig verbundenen Geräte unterstützen. Weitere Informationen finden Sie unter [IoT Hub Preise][lnk-pricing]. Jede Einheit IoT Hub ermöglicht eine bestimmte Anzahl von täglich e-Mail-Nachrichten an.

Wenn Ihre Lösung ordnungsgemäß skalieren möchten, sollten Sie Ihre bestimmte Verwendung von IoT Hub aus. Beachten Sie vor allem die erforderlichen Spitzendurchsatz für die folgenden Kategorien von JOIN-Operationen aus:

* Gerät-Cloud-Nachrichten
* Cloud-zu-Gerät-Nachrichten
* Identität Registrierung Vorgänge

Zusätzlich zu diesen Informationen Durchsatz finden Sie unter [IoT Hub Kontingente und verringert][] und Entwerfen Sie Ihrer Lösung entsprechend.

## <a name="device-to-cloud-and-cloud-to-device-message-throughput"></a>Nachrichtendurchsatz Gerät Cloud und Cloud-zu-Gerät

Die beste Möglichkeit, eine Lösung IoT Hub Größe ist für den Datenverkehr auf Basis pro Einheit ausgewertet werden soll.

Gerät-Cloud-Nachrichten folgen Sie diesen Richtlinien konstanter Datendurchsatz.

| Ebene | Konstanter Datendurchsatz | Kontinuierliche senden Zins |
| ---- | -------------------- | ------------------- |
| S1 | Nach Zeitphasen bis zum 1111 KB pro Minute pro Einheit<br/>(1,5 GB/Tag/Unit) | Mittelwert der 278 Nachrichten pro Minute pro Einheit<br/>(400.000 Nachrichten/Tag pro Einheit) |
| S2 | Bis zu 16 MB pro Minute pro Einheit<br/>(22,8 GB/Tag/Unit) | Mittelwert der 4167 Nachrichten pro Minute pro Einheit<br/>(6 Millionen Nachrichten/Tag pro Einheit) |
| S3 | Nach Zeitphasen bis zum 814 MB/Minute pro Einheit<br/>(1144.4 GB/Tag/Unit) | Mittelwert der 208,333 Nachrichten pro Minute pro Einheit<br/>(300 Millionen Nachrichten/Tag pro Einheit) |

## <a name="identity-registry-operation-throughput"></a>Identität Registrierung Vorgang Durchsatz

IoT Hub Identität Registrierung Vorgänge werden nicht kenne Runtime-Vorgänge werden als hauptsächlich zur Bereitstellung von Gerät zueinander stehen.

Bestimmte Burst Leistung Zahlen finden Sie unter [IoT Hub Kontingente und verringert][].

## <a name="sharding"></a>Sharding

Während ein einzelner IoT-Hub an Millionen von Geräten skaliert werden kann, ist manchmal Ihre Lösung spezifische Leistungsmerkmale, die ein einzelner IoT Hub sichergestellt ist, kann nicht erforderlich. In diesem Fall empfiehlt es sich, dass Sie Ihren Geräten in mehreren IoT Hubs aufteilen. Mehrere IoT Hubs kontinuierliche Datenverkehr Bursts und erhalten die erforderlichen Durchsatz oder Vorgang Sätzen, die erforderlich sind.

## <a name="next-steps"></a>Nächste Schritte

Die Funktionen von IoT Hub weiteren kann, finden Sie unter:

- [Leitfaden für Entwickler][lnk-devguide]
- [Ein Gerät mit dem IoT Gateway SDK simulieren][lnk-gateway]

[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub
[IoT Hub Kontingente und verringert]: iot-hub-devguide-quotas-throttling.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md

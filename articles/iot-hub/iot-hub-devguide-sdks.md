<properties
 pageTitle="Developer Guide - IoT Hub SDKs | Microsoft Azure"
 description="Azure IoT Hub Developer Guide - Informationen und Links zu den verschiedenen SDKs von Azure IoT Hub Geräte und Dienste."
 services="iot-hub"
 documentationCenter=""
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

# <a name="iot-hub-sdks"></a>IoT Hub SDKs

## <a name="iot-hub-device-sdks"></a>IoT Hub-Gerät SDKs

Das Gerät Microsoft Azure IoT SDKs Code enthalten, der erleichtert Entwickeln von Geräten und Programmen, die Herstellen einer Verbindung mit und von Azure IoT Hub Services verwaltet werden.

Das folgende IoT Gerät stehen SDKs vom GitHub heruntergeladen:

- [Azure IoT Gerät SDK für C] [ lnk-c-device-sdk] in ANSI C (C99) für Portabilität und Breite Plattformkompatibilität geschrieben.
- [Azure IoT Gerät SDK für .NET][lnk-dotnet-device-sdk]
- [Azure IoT Gerät SDK für Java][lnk-java-device-sdk]
- [Azure IoT Gerät SDK für Node.js][lnk-node-device-sdk]
- [Microsoft Azure IoT Gerät SDK für Python 2.7][lnk-python-device-sdk]

> [AZURE.NOTE] Finden Sie unter der Infodateien in den Repositorys GitHub Informationen zur Verwendung von Sprache und Plattform-spezifische Paket-Managern, um Binärdateien und Abhängigkeiten auf Ihrem Entwicklungscomputer zu installieren.

## <a name="os-platforms-and-hardware-compatibility"></a>OS Plattformen und Hardware-Kompatibilität

Weitere Informationen zu SDK Kompatibilität mit bestimmten Hardwaregeräte, finden Sie unter der [Azure zertifiziert für IoT Gerät Katalog][lnk-certified].

## <a name="iot-hub-service-sdks"></a>IoT Hub Dienst SDKs

Die Microsoft Azure IoT Dienst SDKs enthalten Code, der Baustein Clientanwendungen, die interagieren direkt mit einem IoT Hub Verwaltung von Geräten und Sicherheit erleichtert.

Der folgende IoT Dienst SDKs sind zum Herunterladen von GitHub verfügbar:

- [Azure IoT Service SDK für .NET][lnk-dotnet-service-sdk]
- [Azure IoT Dienst SDK Node.js][lnk-node-service-sdk]
- [Azure IoT Service SDK für Java][lnk-java-service-sdk]

> [AZURE.NOTE] Finden Sie unter der Infodateien in den Repositorys GitHub Informationen zur Verwendung von Sprache und Plattform-spezifische Paket-Managern, um Binärdateien und Abhängigkeiten auf Ihrem Entwicklungscomputer zu installieren.

## <a name="azure-iot-gateway-sdk"></a>Azure IoT Gateway SDK

Dieses Azure IoT Gateway SDK enthält die Infrastruktur und Module IoT Gateway-Lösungen zu erstellen. Sie können das SDK zum Erstellen des Gateways auf einem beliebigen End-to-End-Szenario zugeschnitten erweitern.

Sie können das [Azure IoT Gateway SDK] herunterladen[ lnk-gateway-sdk] aus GitHub.

## <a name="online-api-reference-documentation"></a>Onlinedokumentation API Bezug

Im folgenden finden eine Liste mit Links zu API Bezug Onlinedokumentation für Azure IoT Gerät, Service und Gateway-Bibliotheken:

- [Das Internet der Dinge (IoT) .NET][lnk-dotnet-ref]
- [IoT Hub REST][lnk-rest-ref]
- [Azure IoT Gerät SDK für C][lnk-c-ref]
- [Azure IoT Gerät SDK für Java][lnk-java-ref]
- [Azure IoT Service SDK für Java][lnk-java-service-ref]
- [Azure IoT Gerät SDK für Node.js][lnk-node-ref]
- [Azure IoT Dienst SDK Node.js][lnk-node-service-ref]
- [Azure IoT Gateway SDK][lnk-gateway-ref]

## <a name="next-steps"></a>Nächste Schritte

Weitere Themen Bezug Leitfadens Entwicklertools IoT Hub umfassen:

- [IoT Hub Endpunkte][lnk-devguide-endpoints]
- [Abfragesprache für im Vergleich, Methoden und Aufträge][lnk-devguide-query]
- [Kontingente und Beschränkung][lnk-devguide-quotas]
- [IoT Hub MQTT support][lnk-devguide-mqtt]

<!-- Links and images -->

[lnk-c-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/c/readme.md
[lnk-dotnet-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/csharp/device/readme.md
[lnk-java-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/java/device/readme.md
[lnk-dotnet-service-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/csharp/service/README.md
[lnk-java-service-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/java/service/readme.md
[lnk-node-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/node/device/readme.md
[lnk-node-service-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/node/service/README.md
[lnk-python-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/python/device/readme.md
[lnk-certified]: https://catalog.azureiotsuite.com/
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/README.md

[lnk-dotnet-ref]: https://msdn.microsoft.com/library/mt488521.aspx
[lnk-c-ref]: http://azure.github.io/azure-iot-sdks/c/api_reference/index.html
[lnk-java-ref]: http://azure.github.io/azure-iot-sdks/java/device/api_reference/index.html
[lnk-node-ref]: http://azure.github.io/azure-iot-sdks/node/api_reference/azure-iot-device/1.0.15/index.html
[lnk-rest-ref]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-java-service-ref]: http://azure.github.io/azure-iot-sdks/java/service/api_reference/index.html
[lnk-node-service-ref]: http://azure.github.io/azure-iot-sdks/node/api_reference/azure-iothub/1.0.17/index.html
[lnk-gateway-ref]: http://azure.github.io/azure-iot-gateway-sdk/api_reference/c/html/

[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
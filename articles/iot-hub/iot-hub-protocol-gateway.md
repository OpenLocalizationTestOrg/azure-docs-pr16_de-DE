<properties
   pageTitle="Azure IoT Protokollgateway | Microsoft Azure"
   description="Beschreibt, wie Azure IoT Protokollgateway zu verwenden, um die Funktionen und die Unterstützung von Azure IoT Hub Protokolle zu erweitern."
   services="iot-hub"
   documentationCenter=""
   authors="kdotchkoff"
   manager="timlt"
   editor=""/>

<tags
   ms.service="iot-hub"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/23/2016"
   ms.author="kdotchko"/>

# <a name="supporting-additional-protocols-for-iot-hub"></a>Unterstützung für weitere Protokolle für IoT-Hub

Azure IoT Hub unterstützt Kommunikation über die Protokolle MQTT, AMQP und HTTP. In einigen Fällen Geräten oder Feld Gateways möglicherweise nicht in der Lage, verwenden Sie eine der folgenden standardmäßigen Protokolle und Protokoll Anpassung benötigen. In diesen Fällen können Sie einen benutzerdefinierten Gateway verwenden. Ein benutzerdefinierter Gateway kann Protokoll Anpassung für IoT Hub Endpunkte aktivieren, indem Sie den Datenverkehr an und von IoT Hub bridging. [Azure IoT Protokollgateway](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md) können als benutzerdefinierte Gateway Protokoll Anpassung zur IoT Hub zu aktivieren.

## <a name="azure-iot-protocol-gateway"></a>Azure IoT Protokollgateway

Azure IoT Protokoll Gateways ist ein Framework für Protokoll Anpassung, die hochgradig skalierbar, bidirektionale Gerätekommunikation mit IoT Hub ausgelegt ist. Das Protokollgateway ist eine Pass-Through-Komponente, die Gerät Verbindungen über ein bestimmtes Protokoll akzeptiert. Es schließt den Datenverkehr an IoT Verteiler über AMQP 1.0. Das Azure IoT Protocol-Gateway ist als Projekt öffnen Source-Software auf Flexibilität beim Hinzufügen von Unterstützung für eine Vielzahl von Protokollen und Protocol-Versionen zur Verfügung.

Sie können das Protokollgateway in Azure hochgradig skalierbare so mithilfe von Azure Cloud Services Worker-Rollen bereitstellen. Darüber hinaus kann das Protokollgateway in einer lokalen Umgebung z. B. Feld Gateways bereitgestellt werden.

Das Azure IoT Protokollgateway enthält ein MQTT Protokoll Netzwerkadapter, die Sie gegebenenfalls das MQTT Protokollverhalten anpassen kann. Da IoT Hub Integrierte das MQTT v3.1.1-Protokoll unterstützt, sollten Sie Sie nur die MQTT Protokoll Netzwerkadapter verwenden, wenn Sie eine Notwendigkeit Protokoll Anpassungen oder bestimmte Anforderungen für zusätzliche Funktionen verfügen.

Die MQTT Netzwerkadapter wird auch veranschaulicht Modell das Programmierung für das Protokoll Netzwerkadapter für andere Protokolle erstellen. Darüber hinaus können mit das Programmierung Azure IoT Protokoll Gateway-Modell Sie benutzerdefinierter Komponenten für spezielle Verarbeitung wie benutzerdefinierte Authentifizierung, Nachricht Transformationen, Komprimierung/ursprünglich oder Verschlüsselung/Entschlüsseln des Datenverkehrs zwischen Geräten und IoT-Hub angeschlossen.

Für Flexibilität werden das Protokollgateway und MQTT Implementierung in einem Projekt öffnen Source-Software bereitgestellt. So können Sie die Implementierung entsprechend anpassen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum verwenden und es als Teil der Lösung IoT bereitstellen Azure IoT Protokoll Gateways und Informationen finden Sie unter:

* [Azure IoT Protokoll Gateway Repository auf GitHub](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md)
* [Azure IoT Protokoll Gateway Developer guide](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/docs/DeveloperGuide.md)

Weitere Informationen zum Planen der Bereitstellung IoT Hub finden Sie unter:

- [Mit dem Ereignis Hubs vergleichen][lnk-compare]
- [Skalieren, HA und DR][lnk-scaling]

Die Funktionen von IoT Hub weiteren kann, finden Sie unter:

- [Leitfaden für Entwickler][lnk-devguide]
- [Ein Gerät mit dem IoT Gateway SDK simulieren][lnk-gateway]

[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md

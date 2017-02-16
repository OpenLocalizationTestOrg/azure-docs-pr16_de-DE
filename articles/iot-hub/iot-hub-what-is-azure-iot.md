<properties
 pageTitle="Azure-Lösungen für Internet der Dinge | Microsoft Azure"
 description="Einen Überblick über IoT auf Azure, einschließlich einer Stichprobe Lösungsarchitektur und deren Bedeutung Azure IoT Hub, Geräte-SDKs und vorkonfigurierten Lösungen"
 services="iot-hub"
 documentationCenter=""
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/05/2016"
 ms.author="dobett"/>

[AZURE.INCLUDE [iot-azure-and-iot](../../includes/iot-azure-and-iot.md)]

## <a name="next-steps"></a>Nächste Schritte

Azure IoT-Hub ist ein Azure-Dienst, der sichere ermöglicht und zuverlässige bidirektionale Kommunikation zwischen Ihrer Anwendung sichern, Ende und Millionen von Geräten. Sie können die Anwendung Back-End:

- Erhalten Sie werden bei von Ihren Geräten.
- Daten aus Ihren Geräten auf einen Stream Ereignisprozessor geleitet.
- Erhalten von Dateiuploads von Geräten aus.
- Senden Sie Befehle Cloud-zu-Gerät an bestimmte Geräte.

IoT Hub können Sie eigene Lösung Back-End implementieren. Darüber hinaus enthält IoT Hub ein Gerät Identität Registry zur Bereitstellung von Geräten, deren Sicherheitsanmeldeinformationen und ihre Rechte in Verbindung mit dem Hub verwendet. Weitere Informationen zum IoT Hub finden Sie unter [Neuigkeiten IoT Hub?] [lnk-iot-hub].

Wie ermöglicht Azure IoT Hub standardisierten IoT Device Management für Sie remote verwalten, konfigurieren und aktualisieren Ihren Geräten finden Sie unter [Übersicht der Azure IoT Hub Gerätemanagement][lnk-device-management].

Implementieren der Clientanwendungen auf eine Vielzahl von Hardware-Plattformen und Betriebssystemen beschrieben, können Sie das Gerät IoT SDKs verwenden. Das Gerät IoT SDKs gehören Bibliotheken, die zu sendende werden einen IoT Hub und Cloud-zu-Gerät Befehle empfangen. Wenn Sie die SDKs verwenden, können Sie zur Kommunikation mit IoT Hub aus mehreren Netzwerkprotokolle auswählen. Weitere Informationen finden Sie unter [Informationen zum Gerät SDKs][lnk-device-sdks].

Um anzufangen Schreiben von Code und einige Beispiele ausführen, finden Sie in die [Erste Schritte mit IoT Hub] [ lnk-getstarted] Lernprogramm.

Sie können möglicherweise auch [Azure IoT Suite]interessiert[lnk-iot-suite], d. h. einer Sammlung von vorkonfigurierten Lösungen. IoT Suite können Sie schnell beginnen und skalieren IoT Projekte häufige IoT Szenarien – wie etwa remote-Überwachung, Anlage Verwaltung und Vorhersage Wartung Adresse.

[lnk-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks/blob/master/readme.md
[lnk-iot-hub]: iot-hub-what-is-iot-hub.md
[lnk-iot-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-iotdev]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: iot-hub-device-management-overview.md
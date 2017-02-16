<properties
 pageTitle="Azure IoT Hub Übersicht | Microsoft Azure"
 description="Übersicht über die IoT Hub Azure Service: Was ist Iot Hub, Gerät Connectivity, Internet Dinge Kommunikationsmuster und Kommunikationsmuster Service-Assistenten"
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
 ms.date="08/25/2016"
 ms.author="dobett"/>

# <a name="what-is-azure-iot-hub"></a>Was ist Azure IoT Hub?

Willkommen bei Azure IoT Hub. Dieser Artikel bietet einen Überblick über die Azure IoT Hub und beschreibt, warum Sie diesen Dienst verwenden sollten, um eine Lösung Internet der Dinge (IoT) zu implementieren. Azure IoT-Hub ist ein vollständig verwalteter Dienst, der zuverlässigen und sicheren bidirektionale Kommunikation zwischen mehreren Millionen IoT Geräte und eine Lösung Back-End ermöglicht. Azure IoT Hub:

- Stellt zuverlässigen-Gerät Cloud und Cloud-zu-Gerät bei messaging.
- Ermöglicht sichere Kommunikation mit Sicherheitsanmeldeinformationen pro Gerät und Steuerelement zugreifen.
- Bietet umfassende Überwachungsfunktionen für Gerätekonnektivität und Gerät Identität überwachen.
- Schließt ein Gerätebibliotheken für die am häufigsten verwendeten Sprachen und Plattformen.

[Vergleich der IoT Hub und Ereignis Hubs] Artikel[ lnk-compare] beschreibt die wichtigsten Unterschiede zwischen diese beiden Dienste und der Vorteile der Verwendung von IoT Hub in Ihrem IoT Lösungen hervorgehoben.

![Azure IoT Hub als Cloud Gateways im Internet der Dinge Lösung][img-architecture]

> [AZURE.NOTE] Eine ausführliche Erläuterung von IoT Architektur finden Sie unter der [Microsoft Azure IoT Bezug Architektur][lnk-refarch].

## <a name="iot-device-connectivity-challenges"></a>IoT Connectivity-Gerät Probleme

IoT Hub und das Gerätebibliotheken helfen Ihnen, zuverlässig und sicher Geräte mit der Lösung Back-End Herstellen einer Verbindung zu bewältigen. IoT Geräte:

- Werden häufig eingebettete Systeme mit keine personenbezogenen Operator.
- Können remote an, wobei der physische Zugriff teure.
- Kann nur über die Back-End Lösung erreichbar sein.
- Möglicherweise Power und Verarbeitung Ressourcen zugänglich gemacht.
- Möglicherweise müssen Sie bei unterbrochener, langsam oder teure Netzwerkkonnektivität.
- Möglicherweise müssen Sie vertrauliche, benutzerdefinierte oder branchenspezifische Anwendungsprotokolle verwenden.
- Können mit einem großen Satz von beliebte Hard- und Software Plattformen erstellt werden.

Zusätzlich zu den oben genannten Vorschriften muss alle IoT Lösung auch skalieren, Sicherheit und Zuverlässigkeit vorführen. Die resultierende Datensatzgruppe Connectivity Anforderungen ist harte und zeitaufwändiger implementiert wird, wenn Sie herkömmliche Technologien, wie z. B. Web Container und messaging Makler verwenden.

## <a name="why-use-azure-iot-hub"></a>Warum verwenden Azure IoT-Hub

Azure IoT Hub Adressen Herausforderung Gerät-Konnektivität wie folgt:

-   **Pro Gerät Anmelde- und sichere Konnektivität**. Bereitstellung von jedem Gerät mit einem eigenen [Sicherheit-Taste] können Sie[ lnk-devguide-security] Sie darauf, um die Verbindung IoT Hub aktivieren. Der [IoT Hub Identität Registrierung] [ lnk-devguide-identityregistry] speichert Gerät Identitäten und Tasten in einer Lösung. Eine Lösung Back-End kann einzelne Geräte gestatten oder verbieten Listen so aktivieren Sie die vollständige Kontrolle über Access Gerät hinzufügen.

-   **Überwachung der Gerät Connectivity Vorgänge**. Sie können die Protokolle für detaillierte Informationen zum Gerät Identität Management Vorgänge und Gerät Connectivity Ereignisse empfangen. Diese Überwachungsfunktionen ermöglicht Ihre Lösung IoT Verbindungsprobleme, z. B. Geräten, die zum Herstellen von Verbindungen mit falschen Anmeldeinformationen, versuchen Sie so oft Senden von Nachrichten oder Ablehnen aller Nachrichten von Cloud-zu-Gerät zu identifizieren.

-   **Eine umfassende Reihe von Gerätebibliotheken**. [Azure IoT Gerät SDKs] [ lnk-device-sdks] stehen zur Verfügung und für verschiedene Sprachen und Plattformen – C für viele Linux-Versionen, Windows und in Echtzeit Betriebssystemen unterstützt. Azure IoT Geräte-SDKs unterstützt auch verwaltete Sprachen, z. B. c#, Java und JavaScript.

-   **IoT Protokolle und Erweiterbarkeit**. Wenn Ihre Lösung die Gerätebibliotheken verwenden kann, macht IoT Hub ein öffentlichen Protokoll, Geräten systembedingt verwenden Sie die MQTT v3.1.1, HTTP 1.1 oder 1.0 AMQP Protokolle ermöglicht. Sie können auch IoT-Hub, um Unterstützung für benutzerdefinierte Protokolle durch bieten erweitern:

    - Erstellen einen Feld Gateway mit dem [Azure IoT Gateway SDK] [ lnk-gateway-sdk] , die Ihre benutzerdefinierte Protokoll in eines der drei Protokolle IoT Hub verständlich konvertiert. 
    - Anpassen des [Azure IoT Protokollgateway][protocol-gateway], einer geöffneten Quellkomponente, die in der Cloud ausgeführt wird.

-   **Skalieren**. Azure IoT Hub skaliert Millionen von gleichzeitig verbundenen Geräten und Millionen Ereignisse pro Sekunde.

Diese Vorteile sind für viele Kommunikation Mustern generisch. IoT Hub können Sie derzeit die folgenden bestimmte Kommunikationsmuster implementieren:

-   **Ereignis-basierten Gerät Cloud-Gliederung.** IoT Hub können Millionen Ereignisse pro Sekunde zuverlässig von Ihren Geräten erhalten. Sie können sie dann auf Ihrem Pfad wichtiges mithilfe einer Event-Engine Prozessor verarbeiten. Sie können auch auf Ihrem kalt Pfad für die Analyse speichern. IoT Hub behält die Daten für bis zu sieben Tage zuverlässigen Verarbeitung zu gewährleisten und Spitzen in Last aufnehmen.

-   * *Zuverlässigen messaging-Cloud-zu-Gerät (oder *Befehle*). ** Lösung Back-End kann IoT Hub zum Senden von Nachrichten mit einer Garantie einmaligem am wenigsten Übermittlung einzelner Geräte verwenden. Jede Nachricht verfügt über eine einzelne Time-to-live-Einstellung und Back-End kann sowohl Übermittlungs- und Ablauf Belege anfordern. Diese Belege Vergewissern Sie sich vollständigen Einblick in den Lebenszyklus einer Nachricht Cloud-zu-Gerät. Sie können dann Geschäftslogik implementieren, die Vorgänge enthält, die auf Geräten ausgeführt werden.

-   **Hochladen von Dateien und Cache Sensordaten in der Cloud.** Ihren Geräten Hochladen von Dateien zum Azure-Speicher von IoT Hub für Sie verwaltet SAS-URIs verwenden können. IoT Hub können Benachrichtigungen beim Eintreffen von Dateien in der Cloud So aktivieren Sie die Back-End für die Verarbeitung zu generieren.

## <a name="gateways"></a>Gateways

Ein Gateway in eine Lösung IoT ist in der Regel ein [Protokollgateway] [ lnk-gateway] bereitgestellt wird in der Cloud oder ein [Feld Gateway] [ lnk-field-gateway] lokal mit Ihren Geräten bereitgestellt wird. Ein Gateway Protocol führt Protokoll Übersetzung, beispielsweise MQTT zu AMQP. Ein Gateway Feld kann Analytics ausführen, klicken Sie auf den Rand, treffen von Entscheidungen auf dringende Wartezeit reduzieren, Rechteverwaltungsdienste Gerät bereitstellen, erzwingen, Sicherheit und Datenschutz Einschränkungen und sondern auch Protokoll Übersetzung. Beide Typen Gateway dienen als Mittler zwischen Ihren Geräten und Ihre IoT Hub.

Ein Gateway Feld unterscheidet sich von der eines einfachen Datenverkehr routing-Geräts (beispielsweise ein Gerät Network Address Translation oder die Firewall), da sie eine aktive Rolle in der Regel ausführt, bei der Verwaltung von Access und den Informationsfluss in Ihre Lösung.

Eine Lösung kann sowohl Protokoll und Feld Gateways enthalten.

## <a name="how-does-iot-hub-work"></a>Wie funktioniert die IoT Hub?

Azure IoT Hub implementiert die [Kommunikation Service-Assistenten] [ lnk-service-assisted-pattern] Muster, um die Interaktionen zwischen Ihren Geräten und Ihre Lösung Back-End zu vermitteln. Das Ziel der Kommunikation Service-Assistenten zu einem vertrauenswürdigen herstellen, bidirektionale Kommunikationspfade zwischen einem Steuerelement System, z. B. IoT Hub und spezielle Geräten, die in bereitgestellt werden nicht vertrauenswürdig Platzbedarf. Das Muster enthält die folgenden Grundsätze:

- Sicherheit Vorrang vor alle anderen Funktionen.
- Geräte annehmen unerwünschte Netzwerkinformationen nicht. Ein Gerät stellt alle Verbindungen und leitet in einer reinen ausgehenden Weise her. Für ein Gerät erhalten Sie einen Befehl vom Back-End muss das Gerät regelmäßig eine Verbindung zu prüfen, ob alle ausstehenden Befehle Verarbeitungszeit einleiten.
- Geräte sollten nur Herstellen einer Verbindung mit oder leitet an bekannte Dienste, die sie mit, wie z. B. IoT Hub Dies sind herzustellen.
- Kommunikationspfad zwischen Gerät und Dienst oder Gerät und Gateway wird auf der Ebene der Anwendung Protokoll gesichert.
- System Ebene Autorisierung und Authentifizierung basieren auf Identitäten pro Gerät. Vorgenommenen Anmeldeinformationen für den Zugriff und Berechtigungen beinahe sofort widerrufen.
- Bidirektionale Kommunikation für Geräte nur manchmal korrekt aufgrund von Power oder Connectivity Bedenken wird erleichtert durch Befehle und Gerät Benachrichtigungen gedrückt, bis ein Gerät eine Verbindung herstellt, um diese zu erhalten. IoT Hub unterhält Geräte-spezifischen Warteschlangen für die Befehle, die sie sendet.
- Anwendung Nutzlastdaten ist für geschützte Übertragung über Gateways, um einen bestimmten Dienst separat gesichert.

Die mobile Branche hat das Dienst unterstützt Kommunikationsmuster bei großes verwendet, um Pushbenachrichtigungen Benachrichtigungsdiensten wie [Windows Pushbenachrichtigungen Benachrichtigung Services]implementieren[lnk-wns], [Google Cloud Messaging][lnk-google-messaging], und [Apple-Pushbenachrichtigungsdienst][lnk-apple-push].

IoT Hub wird über ExpressRoutes öffentlichen Peeringliste Pfad unterstützt.

## <a name="next-steps"></a>Nächste Schritte

Wie ermöglicht Azure IoT Hub standardisierten IoT Device Management für Sie remote verwalten, konfigurieren und aktualisieren Ihren Geräten finden Sie unter [Übersicht der Azure IoT Hub Gerätemanagement][lnk-device-management].

Implementieren der Clientanwendungen auf eine Vielzahl von Hardware-Plattformen und Betriebssystemen beschrieben, können Sie das Gerät IoT SDKs verwenden. Das Gerät IoT SDKs gehören Bibliotheken, die zu sendende werden einen IoT Hub und Cloud-zu-Gerät Befehle empfangen. Wenn Sie die SDKs verwenden, können Sie zur Kommunikation mit IoT Hub aus verschiedenen Netzwerkprotokolle auswählen. Weitere Informationen finden Sie unter [Informationen zum Gerät SDKs][lnk-device-sdks].

Um anzufangen Schreiben von Code und einige Beispiele ausführen, finden Sie in die [Erste Schritte mit IoT Hub] [ lnk-get-started] Lernprogramm.

[img-architecture]: media/iot-hub-what-is-iot-hub/hubarchitecture.png


[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[protocol-gateway]: https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md
[lnk-service-assisted-pattern]: http://blogs.msdn.com/b/clemensv/archive/2014/02/10/service-assisted-communication-for-connected-devices.aspx "Dienst unterstützt Kommunikation, Blogbeitrag von Clemens Vasters"
[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-gateway]: iot-hub-protocol-gateway.md
[lnk-field-gateway]: iot-hub-devguide-endpoints.md#field-gateways
[lnk-devguide-identityregistry]: iot-hub-devguide-identity-registry.md
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-wns]: https://msdn.microsoft.com/library/windows/apps/mt187203.aspx
[lnk-google-messaging]: https://developers.google.com/cloud-messaging/
[lnk-apple-push]: https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW9
[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk
[lnk-device-management]: iot-hub-device-management-overview.md

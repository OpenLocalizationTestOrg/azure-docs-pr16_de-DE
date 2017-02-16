<properties
 pageTitle="Developer Guide - Kontingente und begrenzungsebene | Microsoft Azure"
 description="Azure IoT Hub Entwicklertools Leitfaden – Beschreibung der Kontingente, die auf IoT Hub und erwartet Drosselung anwenden"
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

# <a name="reference---quotas-and-throttling"></a>Verweis - Kontingente und Beschränkung

## <a name="quotas-and-throttling"></a>Kontingente und Beschränkung

Jede Azure-Abonnement kann maximal 10 IoT Hubs haben.

Jede IoT Hub wird nach der Bereitstellung mit einer bestimmten Anzahl von Einheiten in einer bestimmten SKU (Weitere Informationen finden Sie unter [Azure IoT Hub Preise][lnk-pricing]). Ermitteln die SKU und die Anzahl der Einheiten das maximale tägliche Kontingent für Nachrichten, die Sie senden können.

Die SKU bestimmt auch Drosselung Grenzwerte angezeigt, die erzwingt IoT-Hub auf alle Vorgänge.

## <a name="operation-throttles"></a>Vorgang Drosselungen

Vorgang Drosselungen sind Zins Einschränkungen, die in die Minute Bereiche angewendet werden, und sollen Missbrauch zu verhindern. IoT Hub versucht, um zu vermeiden, Fehlern, wann immer möglich zurückgeben, aber es Ausnahmen zurückgeben, wenn die Steuerung zu lange verletzt wird gestartet.

Im folgenden finden die Liste der erzwungenen Drosselungen. Werte beziehen sich auf eine einzelne Hub.

| Schränken Sie | Wert pro hub |
| -------- | ------------- |
| Identität Registrierung Vorgänge (erstellen, abrufen, Liste, aktualisieren und löschen) | 5000/min/Einheit (für S3) <br/> 100/min/Einheit (für S1 und S2). |
| Gerät Verbindungen | 6000/sec/Einheit (für die Kürzel a3), 120/sec/Einheit (für S2), 12/sec/Einheit (für S1). <br/>Mindestens 100/sec. <br/> Beispielsweise sind zwei S1 Einheiten 2\*12 = 24/sec, aber Sie müssen mindestens 100/sec über Ihre Einheiten. Mit neun S1 Einheiten, haben Sie 108/sec (9\*12) über Ihre Einheiten. |
| Sendet Cloud-Gerät | 6000/sec/Einheit (für die Kürzel a3), 120/sec/Einheit (für S2), 12/sec/Einheit (für S1). <br/>Mindestens 100/sec. <br/> Beispielsweise sind zwei S1 Einheiten 2\*12 = 24/sec, aber Sie müssen mindestens 100/sec über Ihre Einheiten. Mit neun S1 Einheiten, haben Sie 108/sec (9\*12) über Ihre Einheiten. |
| Sendet Cloud-zu-Gerät | 5000/min/Einheit (für die Kürzel a3), 100/min/Einheit (für S1 und S2). |
| Cloud-zu-Gerät empfängt. | 50000/min/Einheit (für die Kürzel a3), 1000/min/Einheit (für S1 und S2). |
| Datei hochladen Vorgängen | 5000-Datei hochladen Benachrichtigungen/min/Einheit (für S3), 100 Datei hochladen Benachrichtigungen/min/Einheit (für S1 und S2). <br/> 10000 SAS-URIs können gleichzeitig out für ein Konto Azure-Speicher sein.<br/> 10 SAS-URIs/Gerät kann sich gleichzeitig sein. | 

Es ist wichtig, um zu verdeutlichen, dass das *Gerät Verbindungen* Einschränkung der Zins steuert, neue Gerät Verbindungen mit einem IoT-Hub und nicht die maximale Anzahl der gleichzeitig verbundenen Geräte eingerichtet werden können. Die Steuerung hängt von der Anzahl der Einheiten, die für den Hub bereitgestellt werden.

Wenn Sie eine einzelne Einheit S1 erwerben, erhalten Sie eine Einschränkung der 100 Verbindungen pro Sekunde. Dies bedeutet, dass das Herstellen der Verbindung zwischen 100.000 Geräten dauert mindestens 1000 Sekunden (etwa 16 Minuten). Müssen Sie jedoch so viele gleichzeitig verbundenen Geräte, wie Sie Geräte in Ihrem Gerät Identität Registrierung registriert haben.

Eine ausführliche Erläuterung von IoT Hub begrenzungsebene Verhalten, finden Sie im Blogbeitrag [IoT Hub begrenzungsebene und][lnk-throttle-blog].

>[AZURE.NOTE] Bei einem beliebigen Zeitpunkt ist es möglich, Kontingente oder Einschränkung Grenzwerte erhöhen, indem Sie die Anzahl der in einem IoT-Hub bereitgestellte Einheiten.

>[AZURE.IMPORTANT] Identität Registrierung Vorgänge sind für die Verwendung in Gerätemanagement und Bereitstellung von Szenarien vorgesehen. Lesen oder Aktualisieren einer großen Anzahl Gerät Identitäten wird durch [Importieren und Exportieren von Aufträgen]unterstützt[lnk-importexport].

## <a name="next-steps"></a>Nächste Schritte

Weitere Themen Bezug Leitfadens Entwicklertools IoT Hub umfassen:

- [IoT Hub Endpunkte][lnk-devguide-endpoints]
- [Abfragesprache für im Vergleich, Methoden und Aufträge][lnk-devguide-query]
- [IoT Hub MQTT support][lnk-devguide-mqtt]

[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub
[lnk-throttle-blog]: https://azure.microsoft.com/blog/iot-hub-throttling-and-you/
[lnk-importexport]: iot-hub-devguide-identity-registry.md#import-and-export-device-identities

[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
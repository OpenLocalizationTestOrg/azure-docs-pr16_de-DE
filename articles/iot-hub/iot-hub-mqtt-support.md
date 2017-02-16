<properties
 pageTitle="IoT Hub MQTT Support | Microsoft Azure"
 description="Beschreibung der MQTT in IoT Hub Ebene unterstützen"
 services="iot-hub"
 documentationCenter=".net"
 authors="kdotchkoff"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/24/2016"
 ms.author="kdotchko"/>

# <a name="iot-hub-mqtt-support"></a>IoT Hub MQTT support

IoT Hub ermöglicht Geräte zur Kommunikation mit den IoT Hub Gerät Endpunkten mithilfe der [MQTT v3.1.1] [ lnk-mqtt-org] Protokoll auf Port 8883 oder MQTT v3.1.1 über WebSocket-Protokoll Port 443. IoT Hub erfordert alle Gerätekommunikation über TLS/SSL-gesichert (daher IoT Hub nicht unterstützen nicht sichere Verbindungen über den Port 1883).

## <a name="connecting-to-iot-hub"></a>Herstellen einer Verbindung mit IoT-Hub

Ein Gerät kann das Protokoll MQTT Verbindung zu einem entweder mit der Bibliotheken in der [Microsoft Azure IoT SDKs] IoT-Hub verwenden[ lnk-device-sdks] oder direkt mithilfe der MQTT Protokoll.

## <a name="using-the-device-client-sdks"></a>Mithilfe des Geräte-Clients SDKs

[Gerät Client SDKs] [ lnk-device-sdks] , unterstützen das Protokoll MQTT für Java, Node.js, C, c# und Python verfügbar sind. Das Gerät SDKs Kunde der standardmäßigen IoT Hub Verbindungszeichenfolge zum Herstellen einer Verbindung mit einem Hub IoT. Um das Protokoll MQTT verwenden, muss der Client-Protokoll-Parameter auf **MQTT**festgelegt werden. Standardmäßig Gerät-Client, die Verbindung mit einem IoT-Hub mit der **CleanSession** SDKs festlegen **0** kennzeichnen und **QoS 1** für den Austausch von Nachrichten mit dem IoT-Hub verwenden.

Wenn Sie ein Gerät an einem IoT-Hub angeschlossen ist, der Gerät Client SDKs Methoden, mit denen das Gerät, um Nachrichten zu senden und Empfangen von Nachrichten aus einem IoT-Hub bereitstellen.

In der folgenden Tabelle enthält Links zu Codebeispielen für die einzelnen unterstützten Sprachen und gibt den Parameter zum Herstellen einer Verbindungs mit über das Protokoll MQTT IoT-Hub an.

| Sprache                   | Protokoll-parameter        |
| -------------------------- | ------------------------- |
| [Node.js][lnk-sample-node] | Azure-Iot-Gerät-mqtt     |
| [Java][lnk-sample-java]    | IotHubClientProtocol.MQTT |
| [C][lnk-sample-c]          | MQTT_Protocol             |
| [C#][lnk-sample-csharp]    | TransportType.Mqtt        |
| [Python][lnk-sample-python] | IoTHubTransportProvider.MQTT |

### <a name="migrating-a-device-app-from-amqp-to-mqtt"></a>Migrieren einer app Gerät aus AMQP zu MQTT
Wenn Sie das [Gerät Client SDKs]arbeiten[lnk-device-sdks], umsteigen von AMQP zu MQTT mithilfe den Protokoll-Parameter bei der Clientinitialisierung ändern, wie bereits erwähnt erfordert.

Wenn Sie dies ausführen, stellen Sie sicher, überprüfen Sie die folgenden Elemente:

* AMQP gibt Fehler für viele Bedingungen, während MQTT die Verbindung beendet wird. Die Code für die Fehlerbehandlung Ausnahme möglicherweise daher bei einigen Änderungen erforderlich.
* MQTT unterstützt nicht die Vorgänge *Ablehnen* beim Empfangen von [Nachrichten C2D][lnk-messaging]. Wenn Ihre Back-End eine Antwort von der app Gerät erhalten muss, erwägen Sie [direkten Methoden][lnk-methods].

## <a name="using-the-mqtt-protocol-directly"></a>Verwenden das Protokoll MQTT direkt

Wenn ein Gerät auf den Gerät Client SDKs verwenden kann, können es weiterhin an der öffentlichen Gerät Endpunkte über das Protokoll MQTT verbinden. Das Gerät sollte die folgenden Werte verwenden, in das Paket **Verbinden** :

- Verwenden Sie die **Geräte-ID**für das Feld **ClientId** . 
- Verwenden Sie für das Feld **Username** `{iothubhostname}/{device_id}`, wobei {Iothubhostname} der vollständigen CName eines IoT-Hub ist.

    Beispielsweise ist der Name des Ihrer IoT Hub **contoso.azure-devices.net** und der Name Ihres Geräts **MyDevice01**ist, das Feld für den vollständige **Benutzernamen** sollte enthalten `contoso.azure-devices.net/MyDevice01`.

- Verwenden Sie für das Feld **Kennwort** ein SAS Token. Das Format der SAS Token entspricht dem für das HTTP- und AMQP Protokolle:<br/>`SharedAccessSignature sig={signature-string}&se={expiry}&sr={URL-encoded-resourceURI}`.

    Weitere Informationen dazu, wie Sie SAS Token generieren, finden Sie im Abschnitt Gerät, des [Sicherheitstokens IoT-Hub verwenden][lnk-sas-tokens].
    
    Beim Testen, können Sie auch das [Gerät Explorer] [ lnk-device-explorer] Tool schnell ein Token SAS generieren, die Sie kopieren und in Ihren eigenen Code einfügen können:
    
    1. Wechseln Sie zu der Registerkarte **Verwaltung** in Gerät Explorer.
    2. Klicken Sie auf **SAS Token** (oben rechts).
    3. Klicken Sie auf **SASTokenForm**, wählen Sie Ihr Gerät in der **Geräte-ID** Dropdown-Liste. Festlegen der **TTL**.
    4. Klicken Sie auf **generieren** , um Ihr Token zu erstellen.
    
    Das SAS Token, das generiert wird, ist für diese Struktur:   `HostName={your hub name}.azure-devices.net;DeviceId=javadevice;SharedAccessSignature=SharedAccessSignature sr={your hub name}.azure-devices.net%2fdevices%2fMyDevice01&sig=vSgHBMUG.....Ntg%3d&se=1456481802`.

    Der Teil dieses Token zu verwenden, wenn das Feld **Kennwort** ein, um eine Verbindung mit MQTT ist:   `SharedAccessSignature sr={your hub name}.azure-devices.net%2fdevices%2fyDevice01&sig=vSgHBMUG.....Ntg%3d&se=1456481802g%3d&se=1456481802`.

Für MQTT verbinden und trennen Pakete, IoT Hub Probleme eines Ereignisses auf dem Kanal **Für die Überwachung von Vorgängen** mit zusätzlichen Informationen, die Sie zur Behandlung von Verbindungsproblemen helfen können.

### <a name="sending-messages-to-iot-hub"></a>Senden von Nachrichten an IoT-Hub

Stellen Sie die Verbindung erfolgreich hergestellt wurde, ein Gerät kann Nachrichten senden an IoT Verteiler mit `devices/{device_id}/messages/events/` oder `devices/{device_id}/messages/events/{property_bag}` als **Name des Themas**. Die `{property_bag}` Element ermöglicht das Gerät zum Senden von Nachrichten mit zusätzlichen Eigenschaften in einem Url-codierte Format. Beispiel:

```
RFC 2396-encoded(<PropertyName1>)=RFC 2396-encoded(<PropertyValue1>)&RFC 2396-encoded(<PropertyName2>)=RFC 2396-encoded(<PropertyValue2>)…
```

> [AZURE.NOTE] Dies `{property_bag}` Element verwendet dieselbe Codierung wie für die Abfragezeichenfolgen in das HTTP-Protokoll.

Die Clientanwendung Gerät können Sie auch `devices/{device_id}/messages/events/{property_bag}` als der **Name des Themas wird** zum Definieren von *Nachrichten werden* als Nachricht werden weitergeleitet werden sollen.

IoT Hub unterstützt QoS 2 Nachrichten nicht. Wenn Sie ein Gerät-Client eine Nachricht mit **QoS 2**veröffentlicht, schließt IoT Hub die Verbindung aus.
IoT Hub nicht beibehalten Nachrichten erhalten bleibt. Wenn ein Gerät eine Nachricht mit der **beibehalten** Kennzeichnung auf 1 festgelegt sendet, IoT Hub addiert die **X-Suchbegriffen-beibehalten** Application-Eigenschaft für die Nachricht. In diesem Fall übergibt statt beibehalten die Nachricht beibehalten, IoT Hub sie die Back-End-Anwendung.

### <a name="receiving-messages"></a>Empfangen von Nachrichten

Um Nachrichten von IoT Hub zu erhalten, sollte ein Gerät abonnieren, mit `devices/{device_id}/messages/devicebound/#` als **Thema filtern**. Die mit mehreren Ebenen Platzhalterzeichen **#** im Thema Filter dient nur zum Zulassen des Geräts erhalten Sie zusätzliche Eigenschaften in den Namen des Themas. IoT Hub lässt sich nicht auf die Verwendung von der **#** oder **?** Platzhalter untergeordnete Themen zu filtern. Da IoT Hub keine allgemeine Pub-Sub messaging Bank ist, unterstützt nur die dokumentierten Thema Namen und Thema Filter.

Bitte beachten Sie, dass das Gerät erhalten keine werden Nachrichten über IoT-Hub, bevor sie Endpunkts bestimmte Gerät erfolgreich abonniert hat dargestellt durch die `devices/{device_id}/messages/devicebound/#` Thema filtern. Nach dem erfolgreichen Abonnement eingerichtet wurde, beginnt des Geräts empfangen nur Cloud-zu-Gerät-Nachrichten, die nach dem Zeitpunkt des Abonnements an sie gesendet wurden. Wenn das Gerät mit **CleanSession** Kennzeichnung festlegen **0**verbunden ist, wird das Abonnement bei anderen Sitzungen beibehalten werden. In diesem Fall wird das nächste Mal mit **CleanSession 0** das Gerät verbindet ausstehende Nachrichten erhalten, die darauf gesendet wurden, während sie getrennt wurde. Wenn das Gerät **CleanSession** Kennzeichnung auf **1** festgelegt verwendet, obwohl sie keine Nachrichten über IoT Hub erhalten werden bis zu ihrem Gerät-Endpunkt abonniert.

IoT Hub übermittelt Nachrichten mit dem **Namen des Themas** `devices/{device_id}/messages/devicebound/`, oder `devices/{device_id}/messages/devicebound/{property_bag}` Wenn eine Nachrichteneigenschaften vorhanden sind. `{property_bag}`enthält Url-codierte Schlüssel-Wert-Paare von Eigenschaften der Nachricht an. Nur die Anwendungseigenschaften und Benutzer festgelegt werden Systemeigenschaften (wie etwa **Nachrichten-ID** oder **CorrelationId**) sind in die Eigenschaftensammlung enthalten. System Eigenschaftennamen haben das Präfix **$**, Anwendungseigenschaften verwenden Sie den Namen der ursprünglichen Eigenschaft ohne Präfix.

Wenn ein Thema mit **QoS 2**ein Gerät Client abonniert, gewährt IoT Hub maximale QoS Ebene 1 in das Paket **SUBACK** . Anschließend wird IoT Hub Nachrichten mit dem Gerät her QoS 1 zu übermitteln.

### <a name="additional-considerations"></a>Weitere Aspekte

Als abgeschlossen berücksichtigt, wenn Sie zum Anpassen des Verhaltens MQTT Protokoll, klicken Sie auf der Seite Cloud müssen Sie sollten überprüfen [Azure IoT Protokollgateway] [ lnk-azure-protocol-gateway] werden, können Sie einen benutzerdefiniertes Protokoll leistungsfähige Gateway bereitstellen, die direkt mit IoT Hub-Schnittstellen. Azure IoT Protokoll Gateways können Sie das Gerät Protokoll Brownfield MQTT Bereitstellungen gerecht werden kann oder anderen benutzerdefinierten Protokolle anpassen. Dieser Ansatz ist, allerdings erforderlich, die Sie ausführen, und verwenden einen benutzerdefiniertes Protokollgateway.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter [Notizen MQTT unterstützt] [ lnk-mqtt-devguide] im Azure IoT Hub Developer Guide.

Erfahren Sie mehr über das Protokoll MQTT, finden Sie in der [Dokumentation MQTT][lnk-mqtt-docs].

Weitere Informationen zum Planen der Bereitstellung IoT Hub finden Sie unter:

- [Azure zertifiziert für IoT Gerät Katalog][lnk-devices]
- [Unterstützung für weitere Protokolle][lnk-protocols]
- [Mit dem Ereignis Hubs vergleichen][lnk-compare]
- [Skalieren, HA und DR][lnk-scaling]

Die Funktionen von IoT Hub weiteren kann, finden Sie unter:

- [Leitfaden für Entwickler][lnk-devguide]
- [Ein Gerät mit dem IoT Gateway SDK simulieren][lnk-gateway]

[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks/blob/master/readme.md
[lnk-mqtt-org]: http://mqtt.org/
[lnk-mqtt-docs]: http://mqtt.org/documentation
[lnk-sample-node]: https://github.com/Azure/azure-iot-sdks/blob/develop/node/device/samples/simple_sample_device.js
[lnk-sample-java]: https://github.com/Azure/azure-iot-sdks/blob/develop/java/device/samples/send-receive-sample/src/main/java/samples/com/microsoft/azure/iothub/SendReceive.java
[lnk-sample-c]: https://github.com/Azure/azure-iot-sdks/tree/master/c/iothub_client/samples/iothub_client_sample_mqtt
[lnk-sample-csharp]: https://github.com/Azure/azure-iot-sdks/tree/master/csharp/device/samples
[lnk-sample-python]: https://github.com/Azure/azure-iot-sdks/tree/master/python/device/samples
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/readme.md
[lnk-sas-tokens]: iot-hub-devguide-security.md#using-sas-tokens-as-a-device
[lnk-mqtt-devguide]: iot-hub-devguide-messaging.md#notes-on-mqtt-support
[lnk-azure-protocol-gateway]: iot-hub-protocol-gateway.md

[lnk-devices]: https://catalog.azureiotsuite.com/
[lnk-protocols]: iot-hub-protocol-gateway.md
[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md

[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-messaging]: iot-hub-devguide-messaging.md

<properties
 pageTitle="Leitfaden für Entwickler – direkten Methoden | Microsoft Azure"
 description="Azure IoT Hub Entwicklertools Leitfaden – verwenden direkte Methoden zum Aufrufen von Code auf Ihren Geräten"
 services="iot-hub"
 documentationCenter=".net"
 authors="nberdy"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="nberdy"/>

# <a name="invoke-a-direct-method-on-a-device-preview"></a>Rufen Sie eine direkte Methode auf einem Gerät (Preview)

## <a name="overview"></a>(Übersicht)

IoT Hub gibt Ihnen die Möglichkeit zum Aufrufen von Methoden auf Geräten aus der Cloud. Methoden darstellen eine Besprechungsanfrage Antworten Interaktion mit einem Gerät zu einem Anruf HTTP ähnliche darin, dass er erfolgreich ist oder nicht sofort (nach einem Benutzer angegebenen Timeout) informieren den Benutzer den Status des Anrufs kennen. Dies ist nützlich Szenarien, in denen die Vorgehensweise sofortige Installationsschritte abhängig davon, ob das Gerät reagieren, wie das Senden von SMS-Wakeup an ein Gerät ist ein Gerät offline (SMS teurer als eine Methode Anruf wird), war.

Sie können eine Methode als remote Procedure Call direkt an das Gerät vorstellen. Nur Methoden, die auf einem Gerät implementiert wurden möglicherweise aus der Cloud aufgerufen werden. Wenn die Cloud versucht, eine Methode auf einem Gerät aufzurufen, die diese Methode definiert keinen enthält, schlägt die Methode fehl.

Jede Methode Gerät richtet sich an ein einziges Gerät. [Aufträge] [ lnk-devguide-jobs] bieten eine Möglichkeit zum Aufrufen von Methoden auf mehreren Geräten und Warteschlange Aufrufen von Methoden für getrennte Geräte.

Jede Person mit **Service verbinden** von Berechtigungen für IoT Hub möglicherweise eine Methode auf einem Gerät aufzurufen.

### <a name="when-to-use"></a>Verwenden von

Gerät Methoden ähneln [Cloud-zu-Gerät Nachrichten] [ lnk-devguide-messages] , da beide die Cloud Back-End übergeben von Informationen an ein Gerät aktivieren sie grundlegende Methoden unterscheiden. Im Prinzip sind Methoden synchron und nicht dauerhaften Cloud-zu-Gerät Nachrichten mit bis zu 48 Stunden Zuverlässigkeit asynchrone.

Methoden folgen einem Muster Anforderung-Antwort und sind nicht beständig. Das fehlen Zuverlässigkeit bietet zwei unmittelbare Vorteile, wenn Sie die Geräte Befehle sind:

- **Sofortiges Feedback auf Ausführung Methode** bedeutet, dass es ist nicht erforderlich, für die Sie zum Verwalten der Korrelationskoeffizienten zwischen Anfrage und Antworten.
- **Höhere Durchsatzraten** bedeutet, dass die Vorgänge schneller ausgeführt werden können, da IoT Hub keine Zuverlässigkeit bereitstellt. IoT Hub können weitere Methode Anrufe pro Einheit als Cloud-zu-Gerät-Nachrichten an.

Cloud-zu-Gerät Nachrichten sind nicht unbedingt Befehle auf das Gerät, aber darstellen lieber auf einen Clouddienst einige bisschen Informationen übergeben, das Gerät für sie bei deren Gelegenheit Abholen und dem Gerät möglicherweise oder reagiert eventuell nicht mehr. Cloud-zu-Gerät Nachrichten müssen mehr Timeout Zeit (bis zu 48 Stunden) während Methoden viel schneller abläuft.

Verwenden Sie Gerät Methoden für das sofortige Befehl aufrufen auf einem Gerät und Aufträge für geplanten Aufrufen eines Befehls auf einem Gerät aus.

## <a name="method-lifecycle"></a>Methode Lebenszyklus

Methoden werden auf dem Gerät implementiert und erfordern möglicherweise NULL oder mehr Eingaben in den Nutzdaten Methode, um ordnungsgemäß zu instanziieren. Sie eine direkte Methode durch einen Dienst zugänglichen URI aufrufen (`{iot hub}/twins/{device id}/methods/`). Ein Gerät empfängt direkte Methoden durch ein Gerät-spezifische MQTT Thema (`$iothub/methods/POST/{method name}/`). Wir möglicherweise in der Zukunft Methoden auf Weitere Gerät angeordneten Netzwerke Protokolle unterstützen.

> [AZURE.NOTE] Wenn Sie eine direkte Methode auf einem Gerät aufzurufen, Eigenschaftennamen und Werte können nur enthalten US-ASCII-druckbare alphanumerische, außer in den folgenden Satz: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.

Methoden sind synchron und entweder erfolgreich ist oder ein Fehler auftreten, nach dem Punkt Zeitlimit (Standard: 30 Sekunden, nach oben zum 3.600 Sekunden festgelegt werden kann). Methoden eignen sich in interaktive Szenarien, die ein Gerät, wenn das Gerät online und empfangen Befehle, wie etwa ein Licht von einem Telefon aus einschalten ist fungieren soll. In diesen Fällen möchten finden Sie unter eine sofortige erfolgreichen oder nicht erfolgreichen, damit der Cloud-Dienst das Ergebnis so früh wie möglich bearbeiten kann. Das Gerät kann einige Nachrichtentext als Ergebnis der Methode zurückgeben, aber nicht für die Methode dazu erforderlich ist. Es gibt keine Garantie auf Sortierung oder eine beliebige Semantik Parallelität auf Methode Anrufe aus.

Gerät Methode Anrufe sind nur HTTP aus der Cloud-Seite und MQTT nur von der Geräteseite.

## <a name="reference-topics"></a>Verweis Themen:

Die folgenden Themen bieten Ihnen weitere Informationen zur direkte Methoden verwenden.

## <a name="invoke-a-direct-method-from-a-back-end-app"></a>Um eine direkte Methode aus einer Back-End-app aufzurufen

### <a name="method-invocation"></a>Aufrufen von Methoden

Direkte Methodenaufrufe auf einem Gerät handelt es sich um HTTP-Aufrufe umfassen:

- Speziell für das Gerät *URI* (`{iot hub}/twins/{device id}/methods/`)
- Der POST- *Methode*
- Anfordern von *Überschriften* , die die Autorisierung enthalten-ID, Inhaltstyp und Codierung des Inhalts
- Eines transparenten JSON *Textkörper* in folgendem Format:

```
{
    "methodName": "reboot",
    "timeoutInSeconds": 200,
    "payload": {
        "input1": "someInput",
        "input2": "anotherInput"
    }
}
```

  Timeout ist in Sekunden. Wenn das Timeout nicht festgelegt ist, wird standardmäßig 30 Sekunden.
  
### <a name="response"></a>Antwort

Die Back-End empfängt eine Antwort dem einbezogen:

- *HTTP-Statuscode*, der vom IoT Hub, einschließlich der Fehler 404 für zurzeit nicht verbundenen Geräte in Kürze Fehler verwendet wird
- Anfordern von *Überschriften* , die das Etag enthalten-ID, Inhaltstyp und Codierung des Inhalts
- Eine JSON- *Textkörper* in folgendem Format:

```
{
    "status" : 201,
    "payload" : {...}
}
```
  
   Beide `status` und `body` vom Gerät bereitgestellt und verwendet, um mit der des Geräts Statuscode und/oder Beschreibung zu reagieren.

## <a name="handle-a-direct-method-on-a-devcie"></a>Eine direkte Methode auf einer Devcie behandeln

### <a name="method-invocation"></a>Aufrufen von Methoden

Geräte erhalten direkten Methode Anforderungen über das Thema MQTT:`$iothub/methods/POST/{method name}/?$rid={request id}`

Die Stelle, die das Gerät empfängt befindet sich in folgendem Format:

```
{
    "input1": "someInput",
    "input2": "anotherInput"
}
```

Methode Anfragen sind QoS 0 an.

### <a name="response"></a>Antwort

Das Gerät sendet Antworten auf `$iothub/methods/res/{status}/?$rid={request id}`, wobei:

 - Die `status` Eigenschaft wird der Status Gerät gelieferten der Ausführung der Methode.
 - Die `$rid` Eigenschaft ist die Anfrage-ID aus der Methode aufrufen von IoT Hub empfangen haben.

Textkörper vom Gerät festgelegt ist und alle Status sind möglich.

## <a name="additional-reference-material"></a>Weiteres Referenzmaterial

Andere Verweis in der Developer Guide Themen:

- [IoT Hub Endpunkte] [ lnk-endpoints] beschrieben, die verschiedene Endpunkte, die jeder IoT Hub für Laufzeit und Management Vorgänge macht.
- [Beschränkung und Kontingente] [ lnk-quotas] beschreibt die Kontingente, die beziehen sich auf den Dienst IoT Hub und das Drosselung Verhalten zu erwarten, wenn Sie den Dienst verwenden.
- [IoT Hub Geräte und Dienste SDKs] [ lnk-sdks] Listet die verschiedenen Sprache SDKs Sie eine zu verwenden, wenn Sie das Gerät und der Dienst Clientanwendungen, die Interaktion mit einem IoT Hub entwickeln.
- [Abfragesprache für im Vergleich, Methoden und Aufträge] [ lnk-query] die Abfragesprache Sie Informationen über IoT Hub Ihr Gerät im Vergleich, Methoden und Aufträge abgerufen können werden.
- [Support IoT Hub MQTT] [ lnk-devguide-mqtt] enthält weitere Informationen über die Unterstützung von IoT Hub für das Protokoll MQTT.

## <a name="next-steps"></a>Nächste Schritte

Jetzt so direkte Methoden verwenden gelernten, können Sie im Artikel mit folgenden Developer Guide interessiert wenden:

- [Planen von Projekten auf mehreren Geräten][lnk-devguide-jobs]

Wenn Sie einige der in diesem Artikel beschriebenen Konzepte ausprobieren möchten, möglicherweise in den folgenden IoT Hub Lernprogramm interessiert werden:

- [Cloud-zu-Gerät Methoden verwenden][lnk-methods-tutorial]

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-methods-tutorial]: iot-hub-c2d-methods.md
[lnk-devguide-messages]: iot-hub-devguide-messaging.md

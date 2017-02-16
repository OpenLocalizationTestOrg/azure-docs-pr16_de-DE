<properties
 pageTitle="Leitfaden für Entwickler - messaging | Microsoft Azure"
 description="Azure IoT Hub Developer Guide - Gerät Cloud und messaging Cloud-zu-Gerät"
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

# <a name="send-and-receive-messages-with-iot-hub"></a>Senden und Empfangen von Nachrichten mit IoT-Hub

## <a name="overview"></a>(Übersicht)

IoT Hub bietet die folgenden messaging primitive zur Kommunikation mit einem Gerät:

- [Gerät Cloud] [ lnk-d2c] back-End auf einem Gerät mit einer Anwendung.
- [Cloud-zu-Gerät] [ lnk-c2d] aus einer Anwendung-back-End ("*Service* " oder " *Cloud*").

Haupteigenschaften des IoT Hub messaging-Funktionen sind die Zuverlässigkeit und Zuverlässigkeit von Nachrichten an. Diese Eigenschaften ermöglichen die Verfügbarkeit bei unterbrochener Verbindung auf der Geräteseite, und Laden Sie Spitzen in der Cloud auf Verarbeitung von Ereignissen. IoT Hub implementiert *mindestens einmal* Übermittlung Garantien für messaging sowohl Gerät Cloud-Cloud-zu-Gerät an.

IoT Hub unterstützt mehrere [Protokolle Gerät zugänglichen] [ lnk-protocols] (z. B. MQTT, AMQP und HTTP). Nahtlose Zusammenarbeit über Protokolle unterstützt, definiert IoT Hub eine [Allgemeine Nachrichtenformat] [ lnk-message-format] , die alle Gerät zugänglichen Protokolle unterstützen.

IoT Hub macht ein [Ereignis Hub kompatiblen Endpunkt] [ lnk-compatible-endpoint] Back-End-Anwendungen, die durch den Hub empfangen Gerät-Cloud-Nachrichten lesen aktivieren.

### <a name="when-to-use"></a>Verwenden von

Messaging ist eine Core-Funktion von IoT Hub. Verwenden Sie diese zum Senden von Nachrichten von Ihrem Gerät an Ihre Back-End oder Senden von Nachrichten an ein Gerät vom Back-End Bedarf.

Einen Vergleich der Dienste IoT Hub und Ereignis Hubs, finden Sie unter [Vergleich der IoT Hub und Ereignis Hubs][lnk-compare].

## <a name="device-to-cloud-messages"></a>Gerät-Cloud-Nachrichten

Sie senden Gerät-Cloud-Nachrichten über einen Endpunkt Gerät zugänglichen (**/devices/ {Geräte-ID} / Nachrichten/Ereignisse**). Ihre Back-End-Dienst empfängt Gerät-Cloud-Nachrichten über einen Dienst zugänglichen Endpunkt (**/messages/events**), die mit dem [Ereignis Hubs]kompatibel ist[lnk-event-hubs]. Daher können standard [Ereignis Hubs Integration und SDKs] [ lnk-compatible-endpoint] Gerät-Cloud-Nachrichten empfangen werden sollen.

IoT Hub implementiert Gerät Cloud messaging auf eine Weise, die [Ereignis Hubs]ähnelt[lnk-event-hubs]. IoT-Hub Gerät-Cloud-Nachrichten werden eher Ereignis Hubs *Ereignisse* als [Dienstbus] [ lnk-servicebus] *Nachrichten*.

Diese Implementierung weist die folgenden folgen:

* Auf ähnliche Weise auf Ereignis Hubs Ereignisse, Gerät-Cloud-Nachrichten sind dauerhaften und beibehalten, in einem IoT-Hub für bis zu sieben Tage ( [Gerät-Cloud - Konfigurationsoptionen]finden Sie unter[lnk-d2c-configuration]).
* Gerät-Cloud-Nachrichten über einen festen Satz von Partitionen, die zum Zeitpunkt der Erstellung festgelegt werden aufgeteilt werden ( [Gerät-Cloud - Konfigurationsoptionen]finden Sie unter[lnk-d2c-configuration]).
* An Ereignis Hubs, müssen Clients Gerät-Cloud-Nachrichten lesen Analog Partitionen und für alle behandeln. [Ereignis Hubs – Verwenden von Ereignissen]finden Sie unter[lnk-event-hubs-consuming-events].
* Wie Ereignis Hubs Ereignisse Gerät-Cloud-Nachrichten können höchstens 256 KB sein und sendet optimieren stapelweise gruppiert werden können. Blattnamen können höchstens 256 KB und höchstens 500 Nachrichten sein.

Es gibt jedoch einige wichtige Unterschiede zwischen der IoT Hub Gerät Cloud messaging und Ereignis Hubs:

* Wie [Steuern des Zugriffs auf IoT Hub] erläutert[ lnk-devguide-security] Abschnitt IoT Hub ermöglicht pro Gerät Anmelde- und Access-Steuerelement.
* IoT Hub ermöglicht Millionen von gleichzeitig verbundenen Geräten (finden Sie unter [Kontingente und begrenzungsebene][lnk-quotas]), während das Ereignis Hubs 5000 AMQP Verbindungen pro Namespace beschränkt ist.
* IoT Hub lässt nicht zufällige Partitionierung mithilfe einer **PartitionKey**. Gerät-Cloud-Nachrichten werden basierend auf ihrer ursprünglichen **Geräte-ID**unterteilt.
* Skalierung IoT Hub ist geringfügig Skalierung Ereignis Hubs. Weitere Informationen finden Sie unter [Skalieren IoT Hub][lnk-guidance-scale].

> [AZURE.NOTE] In allen Szenarien können Sie keinen IoT Hub für Ereignis Hubs verwenden. In einigen Ereignis Verarbeitung Berechnungen, möglicherweise sie beispielsweise Ereignisse in Bezug auf eine andere Eigenschaft oder ein Feld Neupartitionierung vor dem Analysieren der Datenstreams erforderlich sein. In diesem Szenario können Sie ein Ereignis Hub um zwei Teile des Streams Verarbeitung Verkaufspipeline entkoppeln. Weitere Informationen finden Sie unter *Partitionen* in [Azure Ereignis Hubs Übersicht][lnk-eventhub-partitions].

Details zur Verwendung von Gerät-Cloud-messaging, [IoT Hub APIs und SDKs]finden Sie unter[lnk-sdks].

> [AZURE.NOTE] Bei Verwendung von HTTP-Gerät-Cloud-Nachrichten senden, Eigenschaftennamen und Werte können nur ASCII-alphanumerische Zeichen enthalten, sowie ``{'!', '#', '$', '%, '&', "'", '*', '*', '+', '-', '.', '^', '_', '`', '|', '~'}``.

### <a name="non-telemetry-traffic"></a>Nicht-telemetrieprotokoll Datenverkehr

Geräte senden häufig zusätzlich werden Datenpunkte, auch Nachrichten und Besprechungsanfragen, die Ausführung und Behandlung von der Anwendung Business Logikebene erfordern. Beispielsweise kritischen Benachrichtigungen, die eine bestimmte Aktion in die Back-End oder Gerät Antworten auf Befehle, die vom Back-End gesendet auslösen müssen.

Weitere Informationen über die beste Möglichkeit, diese Art von Nachricht verarbeiten, finden Sie unter der [Lernprogramm: wie IoT Hub Gerät-Cloud-Nachrichten verarbeitet] [ lnk-d2c-tutorial] Lernprogramm.

### <a name="device-to-cloud-configuration-options"></a>Optionen für die Cloud-Gerät

Ein Hub IoT macht die folgenden Eigenschaften, denen Sie Gerät Cloud messaging steuern können.

* **Partitionsanzahl**. Legen Sie diese Eigenschaft bei der Erstellung die Anzahl der Partitionen für Gerät-Cloud-Ereignis Aufnahme definieren.
* **Aufbewahrungsrichtlinien Zeit**. Diese Eigenschaft gibt die Aufbewahrungszeit Gerät-Cloud-Nachrichten an. Die Standardeinstellung ist ein Tag, aber es auf sieben Tage erhöht werden kann.

Darüber hinaus analog zu Ereignis Hubs, IoT Hub ermöglicht es Ihnen, verwalten Consumer Gruppen in der Cloud Gerät erhalten Endpunkt.

Sie können alle diese Eigenschaften, entweder über den [Anbieter für Ressourcen IoT Hub REST-APIs]programmgesteuert ändern[lnk-resource-provider-apis], oder mithilfe der [Azure-Portal][lnk-management-portal].

### <a name="anti-spoofing-properties"></a>Anti-spoofing Eigenschaften

Gerät Gerät-Cloud-Nachrichten, IoT Hub spoofing vermieden werden sollten Stempel alle Nachrichten mit den folgenden Eigenschaften:

* **ConnectionDeviceId**
* **ConnectionDeviceGenerationId**
* **ConnectionAuthMethod**

Die ersten beiden enthalten, die **Geräte-ID** und **GenerationId** des ursprünglichen Geräts, gemäß [Gerät Identitätseigenschaften][lnk-device-properties].

Die Eigenschaft **ConnectionAuthMethod** enthält ein serialisiert JSON-Objekt, mit den folgenden Eigenschaften:

```
{
  "scope": "{ hub | device}",
  "type": "{ symkey | sas}",
  "issuer": "iothub"
}
```

## <a name="cloud-to-device-messages"></a>Cloud-zu-Gerät-Nachrichten

Sie senden Cloud-zu-Gerät Nachrichten über einen Endpunkt Dienst zugänglichen (**/messages/devicebound**). Ein Gerät empfängt diese über einen Endpunkt Geräte-spezifischen (**/devices/ {Geräte-ID} / Nachrichten/Devicebound**).

Jede Nachricht Cloud-zu-Gerät ist ein einzelnes Gerät ausgelegt, indem Sie die Eigenschaft **zu** auf **/devices/ {Geräte-ID} / Nachrichten/Devicebound**festlegen.

>[AZURE.IMPORTANT] Jedes Gerätewarteschlange enthält höchstens 50 Cloud-zu-Gerät-Nachrichten. Beim Versuch, mehr Nachrichten auf dem gleichen Gerät ergibt einen Fehler zu senden.

> [AZURE.NOTE] Wenn Sie Cloud-zu-Gerät-Nachrichten senden, Eigenschaftennamen und Werte können nur ASCII-alphanumerische Zeichen enthalten, sowie ``{'!', '#', '$', '%, '&', "'", '*', '*', '+', '-', '.', '^', '_', '`', '|', '~'}``.

### <a name="message-lifecycle"></a>Nachricht Lebenszyklus

Damit der Nachrichtenübermittlung mindestens einmal sichergestellt ist, behält IoT Hub Cloud-zu-Gerät Nachrichten in Warteschlange pro Gerät. Geräte müssen explizit bestätigen Sie *Fertigstellung* für IoT Hub, um sie aus der Warteschlange zu entfernen. Dadurch wird sichergestellt, Stabilität gegen Konnektivität und Fehlern Gerät.

Das folgende Diagramm veranschaulicht im Lebenszyklus Zustand Diagramm für eine Nachricht Cloud-zu-Gerät an.

![Cloud-zu-Gerät Nachricht Lebenszyklus][img-lifecycle]

Wenn der Dienst eine Nachricht sendet, gilt die *Warteschlange*. Wenn ein Gerät möchte *erhalten* eine Nachricht IoT Hub *Sperren* der Nachricht (: Legt den Status auf **nicht sichtbaren**) anderer Threads zuzulassen, klicken Sie auf dem gleichen Gerät zu anderen Nachrichten empfangen zu starten. Wenn ein Gerät Thread die Verarbeitung einer Nachricht abgeschlossen ist, benachrichtigt er IoT Hub *durchführen* der Nachricht ein.

Ein Gerät kann auch aus:

- *Ablehnen* die Nachricht, wodurch IoT Hub es in den Zustand **Deadlettered** festlegen. Hinweis: Herstellen einer Verbindung mit MQTT Geräten können nicht C2D Nachrichten ablehnen.
- Legen Sie die Nachricht, wodurch IoT Hub zu setzen die Nachricht in der Warteschlange, mit dem Status wieder *aufgeben* auf **Warteschlange**aus.

Ein Thread konnte nicht ohne Benachrichtigung IoT Hub eine Meldung zu verarbeiten. In diesem Fall Übergabe Nachrichten automatisch aus dem **unsichtbare** Status wieder in der **Warteschlange** Zustand nach einem *Timeout für Sichtbarkeit (oder Sperren)*. Des Standardwerts für diese Timeout ist eine Minute.

Eine Nachricht kann Übergang zwischen der **Warteschlange** und **unsichtbare** Staaten für höchstens, die Anzahl der in der Eigenschaft **max Übermittlung zählen** auf IoT Hub angegebenen Zeiten ausführen. Nach dieser Anzahl von Übergängen legt IoT Hub den Status der Nachricht auf **Deadlettered**. Auf ähnliche Weise IoT Hub wird der Status einer Nachricht auf **Deadlettered** nach dessen Ablaufzeit ( [Time to live]finden Sie unter[lnk-ttl]).

Ein Lernprogramm Cloud-zu-Gerät Nachrichten, finden Sie unter [Lernprogramm: Informationen zum Senden von Nachrichten von Cloud-zu-Gerät mit IoT Hub][lnk-c2d-tutorial]. Für die Verweis-Themen, wie verschiedene APIs und SDKs die Cloud-zu-Gerät-Funktionalität bereitzustellen, finden Sie unter [IoT Hub APIs und SDKs][lnk-sdks].

> [AZURE.NOTE] Führen Sie in der Regel Cloud-zu-Gerät Nachrichten bei jedem der Verlust der Nachricht keinen Einfluss auf die Logik der würden. Beispielsweise der Nachrichteninhalt wurde erfolgreich im lokalen Speicher beibehalten oder ein Vorgang wurde erfolgreich durchgeführt. Die Nachricht konnte auch vorübergehende Informationen Durchführung werden, deren Verlust die Funktionalität der Anwendung nicht beeinträchtigt. In manchen Fällen können Sie die Nachricht Cloud-zu-Gerät langer Vorgänge Fertig stellen nach der Beschreibung der Aufgabe in einem lokalen Speicher beibehalten. Dann können Sie die Anwendung Back-End mit mindestens ein Gerät-Cloud-Nachrichten in den verschiedenen Phasen des Fortschritts des Vorgangs benachrichtigen.

### <a name="message-expiration-time-to-live"></a>Nachrichtenablauf (Zeit, live)

Jede Nachricht Cloud-zu-Gerät verfügt über eine Uhrzeit Ablauf. Diesmal wird mit einer Eigenschaft IoT Hub festgelegte standardmäßig *Gültigkeitsdauer* vom Dienst (in der Eigenschaft **ExpiryTimeUtc** ) oder des IoT Hub festgelegt. [Cloud-zu-Gerät Konfigurationsoptionen]finden Sie unter[lnk-c2d-configuration].

> [AZURE.NOTE] Relativem Nutzen der Nachrichtenablauf und zu vermeiden ist Senden von Nachrichten an getrennte Geräte zu wenig Zeit live Werte festlegen. Dieser Ansatz erzielt dasselbe Ergebnis wie des Verbindungsstatus Gerät, während Sie effizienter verwalten. Wenn Sie die Nachricht Empfangsbestätigungen anfordern, benachrichtigt IoT Hub welche Geräte Nachrichten empfangen werden, und welche Geräte nicht online sind oder nicht bestanden haben.

### <a name="message-feedback"></a>Nachricht feedback

Wenn Sie eine Cloud-zu-Gerät-Nachricht senden, können die Übermittlung pro Nachricht Feedback über den endgültigen Zustand dieser Nachricht Serviceanfrage.

- Wenn Sie die Eigenschaft **Bestätigung** in einen **positiven Wert**festlegen, generiert IoT Hub eine Feedback-Nachricht an, wenn und nur dann, wenn die Nachricht Cloud-zu-Gerät Status " **abgeschlossen** " erreicht.
- Wenn Sie die Eigenschaft **Bestätigung** auf **negative**festlegen, generiert IoT Hub eine Nachricht Feedback nur, wenn, die Cloud-zu-Gerät-Meldung im **Deadlettered** Zustand erreicht.
- Wenn Sie die Eigenschaft **Bestätigung** auf **vollständig**festlegen, generiert IoT Hub eine Feedback-Nachricht in beiden Fällen an.

> [AZURE.NOTE] **Bestätigung** **voll**ist, und Sie keine Feedback-Nachricht erhalten, bedeutet dies, dass die Nachricht Feedback abgelaufen sind. Der Dienst kann nicht wissen, wo befindet sich die ursprüngliche Nachricht. In der Praxis sollten ein Dienst sicherstellen, dass das Feedback verarbeitet werden können, bevor diese abgelaufen ist. Maximale niedrigen wird zwei Tage, wodurch viel Zeit zum Abrufen des Diensts erneut ausgeführt, wenn ein Fehler auftritt.

Wie in [Endpunkte]erläutert[lnk-endpoints], IoT Hub stellt Feedback kann über einen Endpunkt Dienst zugänglichen (**/messages/servicebound/feedback**) als Nachrichten. Die Semantik für den Empfang von Feedback sind die gleichen wie bei Nachrichten Cloud-zu-Gerät, und die gleichen [Nachricht Lebenszyklus]haben[lnk-lifecycle]. Wann immer möglich, ist die Nachricht Feedback in einer einzelnen Nachricht mit folgendem Format zusammengefasst:

| Eigenschaft | Beschreibung |
| -------- | ----------- |
| EnqueuedTime | Zeitstempel, der angibt, wann die Nachricht erstellt wurde. |
| Benutzer-ID | `{iot hub name}` |
| ContentType | `application/vnd.microsoft.iothub.feedback.json` |

Der Text ist ein JSON-serialisierten Array von Datensätzen, jede mit den folgenden Eigenschaften:

| Eigenschaft | Beschreibung |
| -------- | ----------- |
| EnqueuedTimeUtc | Zeitstempel, der angibt, wenn das Ergebnis der Nachricht ist. Beispielsweise abgelaufen ist das Gerät abgeschlossen oder in der Nachricht ein. |
| OriginalMessageId | **Nachrichten-ID** der Nachricht Cloud-zu-Gerät zu der diese Feedback Informationen gelten. |
| StatusCode | Erforderlicher Integer-Wert. Feedback Nachrichten von IoT Hub generiert verwendet. <br/> 0 = Erfolg <br/> 1 = Nachricht abgelaufen ist. <br/> 2 = Übermittlung maximale Anzahl überschritten <br/> 3 = Nachricht abgelehnt |
| Beschreibung | Zeichenfolgenwerte für **StatusCode**. |
| Geräte-ID | **Geräte-ID** des Zielgeräts der Nachricht Cloud-zu-Gerät diese Information Feedback betreffen. |
| DeviceGenerationId | **DeviceGenerationId** des Zielgeräts der Nachricht Cloud-zu-Gerät diese Information Feedback betreffen. |


>[AZURE.IMPORTANT] Der Dienst muss eine **Nachrichten-ID** für die Cloud-zu-Gerät-Nachricht mit der ursprünglichen Nachricht deren Feedback zu koordinieren können angeben.

Das folgende Beispiel zeigt den Textkörper einer Nachricht Feedback.

```
[
  {
    "OriginalMessageId": "0987654321",
    "EnqueuedTimeUtc": "2015-07-28T16:24:48.789Z",
    "StatusCode": 0
    "Description": "Success",
    "DeviceId": "123",
    "DeviceGenerationId": "abcdefghijklmnopqrstuvwxyz"
  },
  {
    ...
  },
  ...
]
```

### <a name="cloud-to-device-configuration-options"></a>Optionen für die Cloud-zu-Gerät

Jede IoT Hub stellt die folgenden Konfigurationsoptionen für messaging Cloud-zu-Gerät zur Verfügung.

| Eigenschaft | Beschreibung | Bereich und Standardwert |
| -------- | ----------- | ----------------- |
| defaultTtlAsIso8601 | Standard-TTL für Nachrichten Cloud-zu-Gerät. | ISO_8601 Intervall nach Zeitphasen bis zum 2D (minimale 1 Minute). Standard: 1 Stunde. |
| maxDeliveryCount | Übermittlung maximale Anzahl für Cloud-zu-Gerät pro Gerät Warteschlangen. | 1 und 100. Standard: 10. |
| feedback.ttlAsIso8601 | Aufbewahrungsrichtlinien für Nachrichten für gebundene Dienst Feedback. | ISO_8601 Intervall nach Zeitphasen bis zum 2D (minimale 1 Minute). Standard: 1 Stunde. |
| feedback.maxDeliveryCount | Übermittlung maximale Anzahl für Feedback Warteschlange. | 1 und 100. Standard: 100. |

Weitere Informationen finden Sie unter [Erstellen von IoT Hubs][lnk-portal].

## <a name="read-device-to-cloud-messages"></a>Gerät-Cloud-Nachrichten lesen

IoT Hub stellt einen Endpunkt für Ihre Back-End-Dienste zum Lesen von der Gerät-Cloud-Nachrichten erhalten von Ihrem Hub zur Verfügung. Der Endpunkt ist Ereignis Hub-kompatiblen, womit Sie keines der Verfahren verwenden Sie den Dienst Ereignis Hubs kann zum Lesen von Nachrichten unterstützt.

Bei Verwendung des [Azure Service Bus SDK für .NET] [ lnk-servicebus-sdk] oder das [Ereignis Hubs - Ereignis Prozessor Host][lnk-eventprocessorhost], Sie können jede beliebige Verbindungszeichenfolge IoT Hub mit den richtigen Berechtigungen verwenden. Verwenden Sie dann **Nachrichten/Ereignisse** als Hub Ereignis Namen ein.

Wenn Sie die SDKs (oder Produktintegration) verwenden, die IoT Hub nicht bekannt sind, müssen Sie ein Ereignis Hub kompatiblen Endpunkt und Ereignis Hub kompatiblen Namen aus der IoT Hub Einstellungen im [Portal Azure]abrufen[lnk-management-portal]:

1. Klicken Sie in das IoT Hub Blade **Messaging**auf.
2. Im Abschnitt **Gerät-Cloud - Einstellungen** finden Sie die folgenden Werte: **Ereignis Hub kompatiblen Endpunkt**, **Ereignis Hub kompatiblen Namen**und **Partitionen**.

    ![Gerät-Cloud-Einstellungen][img-eventhubcompatible]

> [AZURE.NOTE] Wenn das SDK einen Wert für **Hostname** oder **Namespace** erforderlich ist, entfernen Sie das Schema aus dem **Ereignis Hub kompatiblen Endpunkt**aus. Wenn beispielsweise Ihre Ereignis Hub kompatiblen Endpunkt ist **sb://iothub-ns-myiothub-1234.servicebus.windows.net/**der **Hostname** wäre **Iothub-ns-Myiothub-1234.servicebus.windows.net**und den **Namespace** wäre **Iothub-ns-Myiothub-1234**.

Anschließend können Sie alle freigegebenen Access Sicherheitsrichtlinie, die die **ServiceConnect** Berechtigungen für die Verbindung mit dem angegebenen Ereignis Hub verfügt.

Wenn Sie zum Erstellen einer Ereignisses Hub-Verbindungszeichenfolge mithilfe den vorherigen Informationen benötigen, verwenden Sie das folgende Muster:

```
Endpoint={Event Hub-compatible endpoint};SharedAccessKeyName={iot hub policy name};SharedAccessKey={iot hub policy key}
```

Im folgenden finden eine Liste der SDKs und Integration, die mit dem Ereignis Hub kompatiblen Endpunkte verwendet werden können, die IoT Hub macht:

* [Java Ereignis Hubs client](https://github.com/hdinsight/eventhubs-client)
* [Apache Storm spout](../hdinsight/hdinsight-storm-develop-csharp-event-hub-topology.md). Sie können die [Quelle spout](https://github.com/apache/storm/tree/master/external/storm-eventhubs) auf GitHub anzeigen.
* [Apache Spark-integration](../hdinsight/hdinsight-apache-spark-eventhub-streaming.md)

## <a name="reference-topics"></a>Verweis Themen:

Die folgenden Themen bieten Ihnen mit weiteren Informationen über den Austausch von Nachrichten mit IoT-Hub an.

## <a name="message-format"></a>Nachrichtenformat

IoT Hub Nachrichten umfassen:

* Eine Reihe von *Systemeigenschaften*. Eigenschaften, die IoT Hub oder legt interpretiert. Diese sind vordefinierten.
* Eine Reihe von *Anwendungseigenschaften*. Ein Wörterbuch der gemeinsame Zeichenfolgeneigenschaften, die die Anwendung definiert werden kann, und greifen Sie dann ohne Nachrichtentext deserialisieren. IoT Hub ändert nie diese Eigenschaften.
* Eine undurchsichtig binäre Textkörper.

Weitere Informationen darüber, wie die Nachricht in verschiedenen Protokolle codiert werden, finden Sie unter [IoT Hub APIs und SDKs][lnk-sdks].

Die folgende Tabelle enthält den Satz von Systemeigenschaften in IoT Hub Nachrichten.

| Eigenschaft | Beschreibung |
| -------- | ----------- |
| Nachrichten-ID | Ein Benutzer festgelegt werden Bezeichner für die Nachricht, die Anforderung-Antwort Mustern verwendet werden soll. Format: Eine Groß-/Kleinschreibung beachtet Zeichenfolge (bis zu 128 Zeichen lang sein) ASCII-7-Bit-alphanumerische Zeichen + `{'-', ':',’.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}`. |
| Laufende Nummer | Eine Zahl (pro Gerät-Warteschlange eindeutig) von IoT-Hub auf jede Nachricht an Cloud-zu-Gerät zugewiesen. |
| An | In der [Cloud-zu-Gerät] angegebenen Ziel[ lnk-c2d] Nachrichten. |
| ExpiryTimeUtc | Datum und Uhrzeit der Nachricht abläuft. |
| EnqueuedTime | Datum und Uhrzeit, die die Nachricht vom IoT Hub empfangen wurde. |
| CorrelationId | Eine Zeichenfolgeneigenschaft in eine Antwortnachricht, die in der Regel die Nachrichten-ID der Anforderung, in der Besprechungsanfrage Antworten Muster enthält. |
| Benutzer-ID | Personalnummer verwendet, um den Ursprung der Nachrichten angeben. Wenn Sie Nachrichten von IoT Hub generiert werden, zum Festlegen `{iot hub name}`. |
| Bestätigung | Eine Nachricht Feedback-Generator. Diese Eigenschaft wird in der Cloud-zu-Gerät Nachrichten verwendet IoT Hub Feedback Nachrichten als Ergebnis der Verbrauch der Nachricht generieren anfordern vom Gerät. Mögliche Werte: **keine** (Standard): kein Feedback Nachricht wird ausgelöst, **positive**: eine Feedback-Nachricht empfangen, wenn die Nachricht abgeschlossen wurde, **negative**: eine Feedback-Nachricht empfangen, wenn die Nachricht abgelaufen (oder Übermittlung maximale Anzahl erreicht wurde), ohne vom Gerät oder **vollständigen**abgeschlossen wird: positive und negative. Weitere Informationen finden Sie unter [Nachricht Feedback][lnk-feedback]. |
| ConnectionDeviceId | Festlegen von IoT Hub Gerät-Cloud-Nachrichten Personalnummer. Sie enthält die **Geräte-ID** des Geräts, die die Nachricht gesendet. |
| ConnectionDeviceGenerationId | Festlegen von IoT Hub Gerät-Cloud-Nachrichten Personalnummer. Sie enthält die **GenerationId** (gemäß [Gerät Identitätseigenschaften][lnk-device-properties]) des Geräts, die die Nachricht gesendet. |
| ConnectionAuthMethod | Festlegen von IoT Hub Gerät-Cloud-Nachrichten Authentifizierungsmethode. Diese Eigenschaft enthält Informationen zu der Authentifizierungsmethode verwendet, um das Gerät senden der Nachricht authentifiziert. Weitere Informationen finden Sie unter [Gerät in die cloud Anti-spoofing][lnk-antispoofing].|

## <a name="communication-protocols"></a>Kommunikationsprotokolle

IoT Hub ermöglicht Geräte mit [MQTT][lnk-mqtt], MQTT über WebSockets, [AMQP][lnk-amqp], AMQP über WebSockets und HTTP-Protokolle für Gerät angeordneten Kommunikation. Die folgende Tabelle enthält die auf hoher Ebene Empfehlungen für Ihrer Wahl der Protokoll:

| Protokoll | Wenn Sie dieses Protokoll auswählen sollten |
| -------- | ------------------------------------ |
| MQTT <br> MQTT über WebSocket     | Verwenden Sie auf allen Geräten, die nicht über dieselbe TLS-Verbindung Verbindung mehrerer Geräte (jeder mit einem eigenen Anmeldeinformationen pro Gerät) erforderlich ist. |
| AMQP <br> AMQP über WebSocket    | Formular mit auf Feld und Cloud Gateways Verbindung multiplexing über Geräte nutzen. |
| HTTP    | Verwenden Sie für Geräte, die andere Protokolle nicht unterstützt. |

Beachten Sie die folgenden Punkte, wenn Sie Ihr Protokoll für Gerät angeordneten Kommunikation auswählen:

* **Cloud-zu-Gerät Muster**. HTTP verfügt keine effiziente Möglichkeit zum Server Pushbenachrichtigungen implementieren. Wenn Sie HTTP verwenden, Umfrage Geräte als solche IoT Hub für Nachrichten Cloud-zu-Gerät. Dieser Ansatz ist effizient, wenn das Gerät und die IoT Hub. Klicken Sie unter aktuelle HTTP-Richtlinien sollte jedem Gerät für Nachrichten alle 25 Minuten oder mehr Umfrage. MQTT und AMQP unterstützen Server Pushbenachrichtigungen andererseits, wenn Cloud-zu-Gerät Nachrichten empfangen. Sie ermöglichen sofortige schiebt Nachrichten über IoT-Hub an das Gerät aus. Wenn Wartezeit bei der Nachrichtenübermittlung ein Problem darstellt, sind MQTT oder AMQP die beste Protokolle verwenden. Selten verbundenen Geräten sowie passt HTTP.
* **Feld Gateways**. Bei Verwendung von MQTT und HTTP nicht mehrere Geräte (jeder mit einem eigenen Anmeldeinformationen pro Gerät) herstellen mithilfe der gleichen TLS-Verbindungs. Auf diese Weise für [Feld Gateway Szenarien][lnk-azure-gateway-guidance], diese Protokolle nicht optimalen sind, weil sie eine erfordern TLS-Verbindung zwischen dem Feld Gateway und IoT Hub für jedes Gerät mit dem Feld Gateway verbunden.
* **Niedrig Ressource Geräte**. Die MQTT und HTTP-Bibliotheken müssen kleineren als die Bibliotheken AMQP Grundfläche. Als solche, wenn das Gerät mit begrenzten Ressourcen (z. B. weniger als 1 MB RAM) kann, diese Protokolle der einzige Protocol-Implementierung möglicherweise.
* **Netzwerk durchlaufen**. Das standardmäßige AMQP Protokoll verwendet den Port 5671, während MQTT Anschluss 8883, überwacht die Probleme in Netzwerken verursachen können, die in nicht-HTTP-Protokolle geschlossen sind. MQTT über WebSockets, AMQP über WebSockets und HTTP stehen in diesem Szenario verwendet werden.
* **Nutzlastgröße**. MQTT und AMQP sind binäre Protokolle, die als HTTP kompaktere Fracht führen.

> [AZURE.NOTE] Bei Verwendung von HTTP sollte jedem Gerät Cloud-zu-Gerät Nachrichten alle 25 Minuten oder mehr abrufen. Während der Entwicklung ist es jedoch zulässig, häufiger als alle 25 Minuten Umfrage.

## <a name="port-numbers"></a>Portnummern

Geräte können mit IoT Hub in Azure über verschiedene Protokolle kommunizieren. Die Wahl der Protokoll wird in der Regel durch die spezifischen Anforderungen der Lösung gesteuert. Die folgende Tabelle listet die ausgehende Ports, die für ein Gerät werden sollen, verwenden Sie ein bestimmtes Protokoll geöffnet sein müssen:

| Protokoll | Ports |
| -------- | ------- |
| MQTT     | 8883    |
| MQTT über WebSockets | 443    |
| AMQP     | 5671    |
| AMQP über WebSockets | 443    |
| HTTP     | 443     |
| LWM2M (Device Management) | 5684 |

Nachdem Sie einen IoT Hub in einer Azure Region erstellt haben, bleibt im Hub die IP-Adresse für die Gültigkeitsdauer von diesen Hub an. Hingegen wird zum für Quality of Service, verwaltet werden, wenn Microsoft IoT-Hub an einer anderen Maßstab Einheit verschoben wird dann sie eine neue IP-Adresse zugewiesen.

## <a name="notes-on-mqtt-support"></a>Notizen MQTT-Unterstützung

IoT Hub implementiert das Protokoll MQTT v3.1.1, mit dem folgenden Einschränkungen und bestimmte Verhalten:

  * **QoS 2 wird nicht unterstützt**. Wenn ein Gerät-Client eine Nachricht mit **QoS 2**veröffentlicht, schließt IoT Hub die Verbindung aus. Wenn ein Thema mit **QoS 2**ein Gerät Client abonniert, gewährt IoT Hub maximale QoS Ebene 1 in das Paket **SUBACK** .
  * **Beibehalten Nachrichten nicht beibehalten werden**. Wenn ein Gerät-Client eine Nachricht mit der beibehalten Kennzeichnung auf 1 festgelegt veröffentlicht, IoT Hub addiert die **X-Suchbegriffen-beibehalten** Application-Eigenschaft für die Nachricht. In diesem Fall IoT Hub beibehalten Nachricht nicht erhalten bleibt, aber übergibt sie stattdessen die Back-End-Anwendung.

Weitere Informationen finden Sie unter [IoT Hub MQTT unterstützen][lnk-devguide-mqtt].

Als abgeschlossen berücksichtigt, sollten Sie überprüfen, [Azure IoT Protokollgateway] [ lnk-azure-protocol-gateway] werden, können Sie einen benutzerdefiniertes Protokoll leistungsfähige Gateway bereitstellen, die direkt mit IoT Hub-Schnittstellen. Azure IoT Protokoll Gateways können Sie das Gerät Protokoll Brownfield MQTT Bereitstellungen gerecht werden kann oder anderen benutzerdefinierten Protokolle anpassen. Dieser Ansatz ist, allerdings erforderlich, die Sie ausführen, und verwenden einen benutzerdefiniertes Protokollgateway.

## <a name="additional-reference-material"></a>Weiteres Referenzmaterial

Andere Verweis in der Developer Guide Themen:

- [IoT Hub Endpunkte] [ lnk-endpoints] beschrieben, die verschiedene Endpunkte, die jeder IoT Hub für Laufzeit und Management Vorgänge macht.
- [Beschränkung und Kontingente] [ lnk-quotas] beschreibt die Kontingente, die beziehen sich auf den Dienst IoT Hub und das Drosselung Verhalten zu erwarten, wenn Sie den Dienst verwenden.
- [IoT Hub Geräte und Dienste SDKs] [ lnk-sdks] Listet die verschiedenen Sprache SDKs Sie eine zu verwenden, wenn Sie das Gerät und der Dienst Clientanwendungen, die Interaktion mit einem IoT Hub entwickeln.
- [Abfragesprache für im Vergleich, Methoden und Aufträge] [ lnk-query] die Abfragesprache Sie Informationen über IoT Hub Ihr Gerät im Vergleich, Methoden und Aufträge abgerufen können werden.
- [Support IoT Hub MQTT] [ lnk-devguide-mqtt] enthält weitere Informationen über die Unterstützung von IoT Hub für das Protokoll MQTT.

## <a name="next-steps"></a>Nächste Schritte

Jetzt Sie zum Senden und Empfangen von Nachrichten mit IoT Hub gelernt haben, können Sie in den folgenden Themen Developer Guide interessiert wenden:

- [Hochladen von Dateien auf einem Gerät][lnk-devguide-upload]
- [Verwalten von Gerät Identitäten IoT Hub][lnk-devguide-identities]
- [Steuern des Zugriffs auf IoT-Hub][lnk-devguide-security]
- [Verwenden Sie Gerät im Vergleich zum Synchronisieren von Zustand und Konfigurationen][lnk-devguide-device-twins]
- [Um eine direkte Methode auf einem Gerät aufzurufen][lnk-devguide-directmethods]
- [Planen von Projekten auf mehreren Geräten][lnk-devguide-jobs]

Wenn Sie einige der in diesem Artikel beschriebenen Konzepte ausprobieren möchten, können Sie die folgenden IoT Hub Lernprogramme interessant sein:

- [Erste Schritte mit Azure IoT Hub][lnk-getstarted-tutorial]
- [Informationen zum Senden von Nachrichten von Cloud-zu-Gerät mit IoT-Hub][lnk-c2d-tutorial]
- [Wie IoT Hub Gerät-Cloud-Nachrichten verarbeitet][lnk-d2c-tutorial]


[img-lifecycle]: ./media/iot-hub-devguide-messaging/lifecycle.png
[img-eventhubcompatible]: ./media/iot-hub-devguide-messaging/eventhubcompatible.png

[lnk-resource-provider-apis]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-azure-gateway-guidance]: iot-hub-devguide-endpoints.md#field-gateways
[lnk-guidance-scale]: iot-hub-scaling.md
[lnk-azure-protocol-gateway]: iot-hub-protocol-gateway.md
[lnk-amqp]: http://docs.oasis-open.org/amqp/core/v1.0/os/amqp-core-complete-v1.0-os.pdf
[lnk-mqtt]: http://docs.oasis-open.org/mqtt/mqtt/v3.1.1/mqtt-v3.1.1.pdf
[lnk-event-hubs]: http://azure.microsoft.com/documentation/services/event-hubs/
[lnk-event-hubs-consuming-events]: ../event-hubs/event-hubs-programming-guide.md#event-consumers
[lnk-management-portal]: https://portal.azure.com
[lnk-servicebus]: http://azure.microsoft.com/documentation/services/service-bus/
[lnk-eventhub-partitions]: ../event-hubs/event-hubs-overview.md#partitions
[lnk-portal]: iot-hub-create-through-portal.md

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-c2d]: iot-hub-devguide-messaging.md#cloud-to-device-messages
[lnk-compatible-endpoint]: iot-hub-devguide-messaging.md#read-device-to-cloud-messages
[lnk-protocols]: iot-hub-devguide-messaging.md#communication-protocols
[lnk-message-format]: iot-hub-devguide-messaging.md#message-format
[lnk-d2c-configuration]: iot-hub-devguide-messaging.md#device-to-cloud-configuration-options
[lnk-device-properties]: iot-hub-devguide-identity-registry.md#device-identity-properties
[lnk-ttl]: iot-hub-devguide-messaging.md#message-expiration-time-to-live
[lnk-c2d-configuration]: iot-hub-devguide-messaging.md#cloud-to-device-configuration-options
[lnk-lifecycle]: iot-hub-devguide-messaging.md#message-lifecycle
[lnk-feedback]: iot-hub-devguide-messaging.md#message-feedback
[lnk-antispoofing]: iot-hub-devguide-messaging.md#anti-spoofing-properties
[lnk-compare]: iot-hub-compare-event-hubs.md

[lnk-devguide-upload]: iot-hub-devguide-file-upload.md
[lnk-devguide-identities]: iot-hub-devguide-identity-registry.md
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-servicebus-sdk]: https://www.nuget.org/packages/WindowsAzure.ServiceBus
[lnk-eventprocessorhost]: http://blogs.msdn.com/b/servicebus/archive/2015/01/16/event-processor-host-best-practices-part-1.aspx


[lnk-getstarted-tutorial]: iot-hub-csharp-csharp-getstarted.md
[lnk-c2d-tutorial]: iot-hub-csharp-csharp-c2d.md
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

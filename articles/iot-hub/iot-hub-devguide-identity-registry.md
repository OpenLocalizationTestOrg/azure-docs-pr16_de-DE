<properties
 pageTitle="Developer Guide - Gerät Identität Registrierung | Microsoft Azure"
 description="Azure IoT Hub Entwicklertools Leitfaden – Beschreibung der Registrierung Identität Gerät und zur gemeinsamen Nutzung Ihrer Geräte verwalten"
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

# <a name="manage-device-identities-in-iot-hub"></a>Verwalten von Gerät Identitäten IoT Hub

## <a name="overview"></a>(Übersicht)

Jeder IoT Hub verfügt über ein Gerät Identität Registry, in dem Informationen zu den Geräten gespeichert, die Verbindung zu den Hub zulässig sind. Bevor Sie ein Gerät an einen Verteiler herstellen kann, muss ein Eintrag für das Gerät in der Registrierung für den Hub des Geräts Identität vorhanden sein. Ein Gerät muss auch mit dem Hub basierend auf Anmeldeinformationen in das Gerät Identität Registrierung gespeicherten authentifizieren.

Auf hoher Ebene besteht aus der Registrierung des Geräts Identität REST-fähige Gerät Identität Ressourcen. Wenn Sie einen Eintrag in der Registrierung hinzufügen, erstellt IoT Hub eine Reihe von Ressourcen pro Gerät des Diensts wie der Warteschlange, die Flight Cloud-zu-Gerät-Nachrichten enthält.

### <a name="when-to-use"></a>Verwenden von

Verwenden Sie beim Bereitstellen von Geräten, das Herstellen einer Verbindung IoT Hub mit und wann Sie pro Gerät Zugriff auf die Endpunkte Gerät zugänglichen in Ihrem Hub steuern müssen, Sie müssen die Registrierung des Geräts Identität.

> [AZURE.NOTE] Die Registrierung der Identität des Geräts enthalten keine anwendungsspezifische Metadaten.

## <a name="device-identity-registry-operations"></a>Gerät Identität Registrierung Vorgänge

Die IoT Hub Gerät Identität Registrierung stellt die folgenden Vorgänge zur Verfügung:

* Erstellen der Identität des Geräts
* Aktualisieren der Identität des Geräts
* Gerät Identität nach ID abrufen
* Löschen der Identität des Geräts
* Liste von bis zu 1000 Identitäten
* Exportieren Sie alle Identitäten in Azure Blob-Speicher
* Importieren von Identitäten aus Azure Blob-Speicher

All diese Vorgänge können optimistische gleichzeitiger Zugriff, als in angegebenen [RFC7232][lnk-rfc7232].

> [AZURE.IMPORTANT] Die einzige Möglichkeit zum Abrufen von alle Identitäten in einem Hub Identität Registrierung besteht darin, das [Exportieren] verwenden[ lnk-export] Funktionalität.

Eine IoT Hub Gerät Identität Registrierungsschlüssel:

- Enthält keine Metadaten für die Anwendung.
- Wie ein Wörterbuch, erfolgt mithilfe der **Geräte-ID** als Schlüssel.
- Ausdrucksbasierte Abfragen unterstützt nicht.

Eine Lösung IoT weist normalerweise eine separate Lösung-spezifische Store, die anwendungsspezifische Metadaten enthält. Beispielsweise würde der Lösung-spezifische Store in einer SmartArt-Baustein-Lösung den Chatroom aufzeichnen, in dem ein Temperatursensor bereitgestellt wird.

> [AZURE.IMPORTANT] Verwenden Sie nur die Registrierung der Identität des Geräts für Gerätemanagement und Bereitstellung von Vorgängen ein. Hoher Durchsatz Vorgänge zur Laufzeit sollten nicht ausführen von Vorgängen in der Registrierung des Geräts Identität abhängig. Überprüfen den Verbindungsstatus des ein Gerät, vor dem Senden eines Befehls beträgt beispielsweise kein Muster unterstützt. Stellen Sie sicher überprüfen [begrenzungsebene Sätzen] [ lnk-quotas] für die Registrierung der Identität des Geräts und das [Gerät Heartbeat] [ lnk-guidance-heartbeat] Muster.

## <a name="disable-devices"></a>Deaktivieren von Geräten

Sie können Geräte deaktivieren, indem Sie die Eigenschaft **Status** einer Identität in der Registrierung zu aktualisieren. Normalerweise verwenden Sie diese Eigenschaft in den folgenden beiden Szenarien:

- Bei einem provisioning Orchestrierung. Weitere Informationen finden Sie unter [Gerät Provisioning][lnk-guidance-provisioning].
- Wenn aus irgendeinem Grund erwägen ein Gerät beeinträchtigt wird oder nicht autorisierte geworden.

## <a name="import-and-export-device-identities"></a>Importieren und Exportieren von Gerät Identitäten

Sie können Gerät Identitäten in Massen mithilfe von asynchrone Vorgänge auf den [IoT Hub Ressource Anbieterendpunkt]aus einem IoT-Hub Identität Registrierung, exportieren[lnk-endpoints]. Exporte werden Aufträge mit langer Ausführung, mit denen ein Containers Blob Kunden bereitgestellten um Identität Gerätedaten Lesen aus der Registrierung Identität zu speichern.

Sie können Gerät Identitäten in Massen mithilfe von asynchrone Vorgänge auf den [IoT Hub Ressource Anbieterendpunkt]IoT-Hub Identität Registrierung, importieren[lnk-endpoints]. Importe sind Aufträge mit langer Ausführung, mit denen Daten in einem Kunden bereitgestellten Blob Container um Identität Gerätedaten in der Registrierung des Geräts Identität zu schreiben.

- Ausführliche Informationen zu den Import und Export APIs, finden Sie unter [IoT Hub Ressourcenanbieter REST-APIs][lnk-resource-provider-apis].
- Wenn Sie weitere Informationen zum Ausführen von Import- und Aufträge exportieren, finden Sie unter [Massen Verwaltung von IoT Hub Gerät Identitäten][lnk-bulk-identity].

## <a name="device-provisioning"></a>Gerät bereitgestellt.

Die Gerätedaten in eine bestimmte IoT Lösung gespeichert hängt die spezifischen Erfordernisse die Lösung. Jedoch mindestens eine Lösung Gerät Identitäten und Authentifizierungsschlüssel speichern muss. Azure IoT Hub enthält eine Identität Registrierung, die Werte für die einzelnen Geräte wie IDs, Authentifizierungsschlüssel und Statuscodes speichern kann. Eine Lösung können anderen Azure Diensten wie Azure Tabellenspeicher, Azure Blob-Speicher oder Azure DocumentDB zusätzliche Gerätedaten gespeichert werden.

*Gerät bereitgestellt* wird die Vorgehensweise zum Hinzufügen der anfänglichen Gerätedaten in die Speicher in Ihre Lösung. Um ein neues Gerät für die Verbindung zu Ihrem Hub zu aktivieren, müssen Sie eine neue Geräte-ID und Schlüssel zur Registrierung IoT Hub Identität hinzufügen. Als Teil der Bereitstellung Prozess müssen Sie die Geräte-spezifischen Daten in anderen Lösung Stores Initialisierung.

## <a name="device-heartbeat"></a>Gerät heartbeat

Die IoT Hub Identität Registrierung enthält ein Feld namens **ConnectionState**. Verwenden Sie das Feld **ConnectionState** nur während der Entwicklung und für das Debuggen IoT Lösungen sollte nicht Abfragen, das Feld zur Laufzeit (z. B. prüfen, ob ein Gerät angeschlossen ist, müssen, um zu entscheiden, ob Sie eine Nachricht Cloud-zu-Gerät oder ein SMS senden).

Wenn Ihre Lösung IoT muss wissen, ob ein Gerät verbunden ist, (entweder zur Laufzeit oder mit mehr Genauigkeit als die **ConnectionState** -Eigenschaft), Ihre Lösung sollte das *Heartbeat Muster*zu implementieren.

Im Muster Heartbeat sendet das Gerät Gerät-Cloud-Nachrichten mindestens einmal jeder festgelegten Zeitraum (z. B. mindestens einmal pro Stunde). Dies bedeutet, dass auch wenn ein Gerät keinen Daten zu senden, sendet er immer noch eine leere Gerät-Cloud-Nachricht (in der Regel mit einer Eigenschaft, die sie als einen Takt identifiziert). Klicken Sie auf der Dienstseite die Lösung unterhält eine Karte mit den letzten Takt für jedes Gerät erhalten und wird davon ausgegangen, dass ein Problem mit einem Gerät vorhanden ist, wenn sie nicht über eine Taktnachricht innerhalb der erwarteten Zeit erhält.

Eine komplexere Implementierung konnte nehmen Sie die Informationen aus der [Vorgänge Überwachung] [ lnk-devguide-opmon] Geräte ermitteln, die versuchen, eine Verbindung herstellen oder kommunizieren, aber weiß nicht. Wenn Sie das Heartbeat Muster implementieren, stellen Sie sicher, [IoT Hub Kontingente und verringert]überprüfen[lnk-quotas].

> [AZURE.NOTE] Wenn eine Lösung IoT benötigt des Verbindungsstatus Gerät ausschließlich, um festzustellen, ob Cloud-zu-Gerät-Nachrichten senden und Nachrichten werden nicht auf große Mengen von Geräten übertragen, sollten ein viel einfacheres Muster zu berücksichtigen ist eine kurze Ablauf Uhrzeit verwenden. Damit wird das gleiche Ergebnis wie eine Gerät Verbindung Zustand Registrierung mithilfe des Musters Heartbeat, wesentlich effizienter verwalten erzielt. Es ist auch möglich, per Nachricht bereitgestellten von IoT Hub welche Geräte Nachrichten empfangen werden, und die nicht online sind oder werden konnte nicht benachrichtigt werden.

## <a name="reference-topics"></a>Verweis Themen:

Die folgenden Bezug Themen bieten weitere Informationen zur Registrierung Identität Devcie.

## <a name="device-identity-properties"></a>Eigenschaften des Geräts Identität

Gerät Identitäten werden als JSON-Dokumente mit den folgenden Eigenschaften dargestellt.

| Eigenschaft | Optionen | Beschreibung |
| -------- | ------- | ----------- |
| Geräte-ID | erforderlich, klicken Sie auf Updates schreibgeschützt | Eine Textzeichenfolge Groß-/Kleinschreibung (maximal 128 Zeichen lang sein) ASCII-7-Bit-alphanumerische Zeichen + `{'-', ':', '.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}`. |
| generationId | erforderlich, schreibgeschützt | Eine Hub generiert, Groß-/Kleinschreibung beachtet Zeichenfolge mit maximal 128 Zeichen lang sein. Hiermit wird die Geräte mit der gleichen **Geräte-ID**, unterscheiden, wenn sie gelöscht und neu erstellt wurden. |
| ETag | erforderlich, schreibgeschützt | Eine Zeichenfolge mit einer schwachen Etag für die Identität des Geräts gemäß [RFC7232][lnk-rfc7232].|
| Authentifizierung | Optional | Ein composite-Objekt, Authentifizierung Informationen und Sicherheit Material enthält. |
| auth.symkey | Optional | Ein zusammengesetztes Objekt mit einem primären und sekundären Schlüssel, die im base64-Format gespeichert werden. |
| Status | Erforderlich | Ein Access-Indikator. **Aktiviert** oder **deaktiviert**können werden. Wenn **aktiviert**, das Gerät Verbindung zulässig ist. Wenn Sie **deaktiviert ist**, dieses Gerät einem beliebigen Gerät zugänglichen Endpunkt Zugriff nicht möglich ist. |
| statusReason | Optional | Eine 128 Zeichen lange Zeichenfolge, die den Grund für den Status des Geräts Identität speichert. Alle UTF-8-Zeichen zulässig sind. |
| statusUpdateTime | schreibgeschützt | Ein zeitlicher Indikator mit Datum und Uhrzeit der letzten statusaktualisierung. |
| connectionState | schreibgeschützt | Ein Feld, der angibt, Verbindungsstatus: entweder **verbunden** oder **Verbindung getrennt**. Dieses Feld legt die IoT Hub Ansicht des Verbindungsstatus Gerät angezeigt. **Wichtig**: sollte dieses Feld nur für die Entwicklung/Debuggen Zwecke verwendet werden. Des Verbindungsstatus wird nur für Geräte über MQTT oder AMQP aktualisiert. Darüber hinaus basieren auf auf Protokollstufe Pings (MQTT Pings oder AMQP Pings), und es kann besitzen eine maximale Verzögerung von nur 5 Minuten. Aus diesen Gründen möglich falsche positive wie Geräte gemeldet verbunden, aber die tatsächlich getrennt werden. |
| connectionStateUpdatedTime | schreibgeschützt | Eine zeitliche Anzeige, Datum und Uhrzeit der letzten des Verbindungsstatus aktualisiert wurde. |
| lastActivityTime  | schreibgeschützt | Ein zeitlicher Indikator, Datum und Uhrzeit der letzten mit dem Gerät verbunden, erhalten oder eine Nachricht gesendet. |

> [AZURE.NOTE] Verbindungsstatus kann nur die Ansicht den Status der Verbindung IoT Hub darstellen. Updates an diesen Status können, je nach netzwerkbedingungen und Konfigurationen verzögert werden.

## <a name="additional-reference-material"></a>Weiteres Referenzmaterial

Andere Verweis in der Developer Guide Themen:

- [IoT Hub Endpunkte] [ lnk-endpoints] beschrieben, die verschiedene Endpunkte, die jeder IoT Hub für Laufzeit und Management Vorgänge macht.
- [Beschränkung und Kontingente] [ lnk-quotas] beschreibt die Kontingente, die beziehen sich auf den Dienst IoT Hub und das Drosselung Verhalten zu erwarten, wenn Sie den Dienst verwenden.
- [IoT Hub Geräte und Dienste SDKs] [ lnk-sdks] Listet die verschiedenen Sprache SDKs Sie eine zu verwenden, wenn Sie das Gerät und der Dienst Clientanwendungen, die Interaktion mit einem IoT Hub entwickeln.
- [Abfragesprache für im Vergleich, Methoden und Aufträge] [ lnk-query] die Abfragesprache Sie Informationen über IoT Hub Ihr Gerät im Vergleich, Methoden und Aufträge abgerufen können werden.
- [Support IoT Hub MQTT] [ lnk-devguide-mqtt] enthält weitere Informationen über die Unterstützung von IoT Hub für das Protokoll MQTT.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie mit der Registrierung IoT Hub Geräts Identität vertraut gemacht haben, möglicherweise in den folgenden Themen Developer Guide interessiert werden:

- [Steuern des Zugriffs auf IoT-Hub][lnk-devguide-security]
- [Verwenden Sie Gerät im Vergleich zum Synchronisieren von Zustand und Konfigurationen][lnk-devguide-device-twins]
- [Um eine direkte Methode auf einem Gerät aufzurufen][lnk-devguide-directmethods]
- [Planen von Projekten auf mehreren Geräten][lnk-devguide-jobs]

Wenn Sie einige der in diesem Artikel beschriebenen Konzepte ausprobieren möchten, möglicherweise in den folgenden IoT Hub Lernprogramm interessiert werden:

- [Erste Schritte mit Azure IoT Hub][lnk-getstarted-tutorial]


<!-- Links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-resource-provider-apis]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-guidance-provisioning]: iot-hub-devguide-identity-registry.md#device-provisioning
[lnk-guidance-heartbeat]: iot-hub-devguide-identity-registry.md#device-heartbeat
[lnk-rfc7232]: https://tools.ietf.org/html/rfc7232
[lnk-bulk-identity]: iot-hub-bulk-identity-mgmt.md
[lnk-export]: iot-hub-devguide-identity-registry.md#import-and-export-device-identities
[lnk-devguide-opmon]: iot-hub-operations-monitoring.md

[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md

[lnk-getstarted-tutorial]: iot-hub-csharp-csharp-getstarted.md
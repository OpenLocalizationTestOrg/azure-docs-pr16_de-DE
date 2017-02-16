> [AZURE.SELECTOR]
- [Linux](../articles/iot-hub/iot-hub-linux-gateway-sdk-get-started.md)
- [Windows](../articles/iot-hub/iot-hub-windows-gateway-sdk-get-started.md)

Dieser Artikel enthält eine ausführliche exemplarische [Hallo Welt Stichprobe Code] [ lnk-helloworld-sample] Erläutern Sie die grundlegenden Komponenten des [Azure IoT Gateway SDK] [ lnk-gateway-sdk] Architektur. Das Beispiel verwendet das IoT Hub Gateway SDK um eine einfache Gateway zu erstellen, der in eine Datei alle fünf Sekunden eine Nachricht "Hallo Welt" protokolliert.

Diese exemplarische Vorgehensweise umfasst:

- **Konzepte**: eine grundlegende Übersicht über die Komponenten, die alle Gateway Verfassen Sie mit dem IoT Gateway SDK erstellen.  
- **Hallo Welt Stichprobe Architektur**: Beschreibt, wie die Konzepte zum Beispiel Hallo Welt gelten, und wie die Komponenten zusammenwirken.
- **So erstellen Sie das Beispiel**: die erforderlichen Schritte zum Erstellen des Beispiels.
- **So führen Sie das Beispiel**: die erforderlichen Schritte zum Ausführen des Beispiels. 
- **Typische Ergebnis**: ein Beispiel für die Ausgabe zu erwarten, wenn Sie das Beispiel ausführen.
- **Codeausschnitte**: eine Auflistung von Codeausschnitte, um anzuzeigen, wie im Beispiel Hallo Welt Key Gateway-Komponenten implementiert.

## <a name="azure-iot-gateway-sdk-concepts"></a>Azure IoT Gateway SDK Konzepte

Bevor Sie den Stichprobe Code untersuchen oder Erstellen eigener Feld Gateway mit dem IoT Gateway SDK, sollten Sie wichtige Konzepte verstehen, die die Architektur des SDK zugrundeliegt.

### <a name="modules"></a>Module

Erstellen Sie ein Gateway mit dem Azure IoT Gateway SDK durch das Erstellen und *Module*zusammenstellen. Module werden *Nachrichten* Datenaustausch miteinander verwenden. Ein Modul erhält eine Nachricht, eine Aktion ausgeführt wird, klicken Sie auf diese, wandelt sie optional in einer neuen Nachricht und dann veröffentlicht ihn für andere Module Verarbeitungszeit. Einige Module möglicherweise Naturprodukte nur neue Nachrichten und nie eingehende Nachrichten verarbeiten. Eine Kette von Module erstellt eine Verkaufspipeline Datenverarbeitung mit jedem Modul die Daten an eine Stelle in diesem Verkaufspipeline eine Transformation ausführen.

![Eine Kette von Module in das Feld Gateway mit dem Azure IoT Gateway SDK erstellt][1]
 
Das SDK enthält Folgendes:

- Vordefinierter Module, mit die häufige Gatewayfunktionen ausführen.
- Die Schnittstellen kann ein Entwickler zum Schreiben von benutzerdefinierten Module verwenden.
- Die Infrastruktur bereitstellen, und führen Sie eine Reihe von Modulen erforderlich.

Das SDK bietet eine Abstraktionsschicht, die Sie zum Erstellen des Gateways auf einer Vielzahl von Betriebssystemen und Plattformen ausführen kann.

![Azure IoT Gateway SDK Abstraktionsschicht][2]

### <a name="messages"></a>Nachrichten

Zwar denken übergeben Nachrichten aneinander Module bequem wie ein Gatewayfunktionen zu erfassen, spiegelt es am genauesten keine was passiert. Mithilfe einer Bank Module kommunizieren, die sie Veröffentlichen von Nachrichten in der Makler (Bus, Pubsub oder ein anderes messaging Muster), und lassen Sie dann auf der die Nachricht an die verbundenen Module weiterleiten Makler.

Eine Module wird die **Broker_Publish** -Funktion verwendet, um eine Nachricht in der Makler zu veröffentlichen. Der Makler übermittelt Nachrichten an ein Modul, indem Sie eine Rückruffunktion. Eine Nachricht besteht aus einer Reihe von Schlüssel/Wert-Eigenschaften und Inhalte als einen Textblock Arbeitsspeicher übergeben.

![Die Rolle des der Makler im Azure IoT Gateway SDK][3]

### <a name="message-routing-and-filtering"></a>Weiterleiten von Nachrichten und Filtern

Es gibt zwei Arten von Nachrichten an die richtige Module zu leiten. Eine Reihe von Links kann an der Makler weitergegeben werden, damit der Makler die Quelle und der Empfänger für jedes Modul kennt, oder des Moduls in den Eigenschaften der Nachricht filtern. Ein Modul sollte eine Nachricht nur reagieren, wenn die Nachricht dafür vorgesehen ist. Unter den Links und Filtern von Nachrichten ist was effektiv eine Nachricht Verkaufspipeline erstellt.

## <a name="hello-world-sample-architecture"></a>Hallo Welt Stichprobe Architektur

Das Hallo Welt veranschaulicht die Konzepte im vorherigen Abschnitt beschrieben. Im Beispiel Hallo Welt implementiert ein Gateways, das eine Verkaufspipeline besteht aus zwei Module hat:

-   Das Modul *Hallo Welt* erstellt eine Nachricht alle fünf Sekunden und übergibt diese an das Modul für die Protokollierung.
-   Das *Protokollierung* Modul schreibt die empfangenen Nachrichten in eine Datei an.

![Hallo Welt Stichprobe erstellt, mit dem Azure IoT Gateway SDK Architektur][4]

Wie im vorherigen Abschnitt beschrieben, wird das Modul Hallo Welt nicht Nachrichten direkt in das Modul für die Protokollierung alle fünf Sekunden übergeben. In diesem Fall veröffentlicht sie eine Nachricht der Makler alle fünf Sekunden.

Das Modul für die Protokollierung erhält eine Nachricht von der Makler und fungiert, schreiben den Inhalt der Nachricht in eine Datei.

Das Protokollierung Modul verbraucht nur Nachrichten aus der Makler, nie neue Nachrichten in der Makler veröffentlicht.

![Der Makler das Weiterleiten von Nachrichten zwischen Module im Azure IoT Gateway SDK][5]

In der obigen Abbildung zeigt die Architektur des Beispiels Hallo Welt und die relative Pfade zu den Quelldateien, die verschiedene Teile des Beispiels im [Repository]implementieren[lnk-gateway-sdk]. Erforschen des Codes ein, oder verwenden Sie die Codeausschnitte unter als Leitfaden.

<!-- Images -->
[1]: media/iot-hub-gateway-sdk-getstarted-selector/modules.png
[2]: media/iot-hub-gateway-sdk-getstarted-selector/modules_2.png
[3]: media/iot-hub-gateway-sdk-getstarted-selector/messages_1.png
[4]: media/iot-hub-gateway-sdk-getstarted-selector/high_level_architecture.png
[5]: media/iot-hub-gateway-sdk-getstarted-selector/detailed_architecture.png

<!-- Links -->
[lnk-helloworld-sample]: https://github.com/Azure/azure-iot-gateway-sdk/tree/master/samples/hello_world
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk
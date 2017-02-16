<properties
 pageTitle="IoT Hub diagnostic Kennzahlen"
 description="Übersicht über Azure IoT Hub Kennzahlen, zulassen, dass Benutzer den allgemeinen Zustand des deren Ressource bewerten"
 services="iot-hub"
 documentationCenter=""
 authors="nberdy"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/11/2016"
 ms.author="nberdy"/>

# <a name="introduction-to-diagnostic-metrics"></a>Einführung in diagnostic Kennzahlen

Diagnose Kennzahlen bieten Ihnen eine bessere Daten über den Status der Azure Ressourcen in Ihrem Azure-Abonnement. Kennzahlen können Sie den allgemeinen Zustand des Dienstes und damit verbundenen Geräte bewerten. Benutzer zugänglichen Statistiken sind wichtig, weil sie dabei, Sie sehen helfen, was mit Ihrer IoT-Hub und Hilfe Ursache Probleme vor sich geht ohne Azure-Support wenden.

Sie können diagnostic Metrik vom Azure-Portal aktivieren.

## <a name="how-to-enable-diagnostic-metrics"></a>Aktivieren von Diagnoseprotokollen Kennzahlen

1. Erstellen Sie einen IoT-Hub an. Anweisungen finden Sie unter So erstellen Sie einen IoT Hub in das [Erste Schritte] [ lnk-get-started] Guide.

2. Öffnen Sie Ihre IoT Hub Falz. Klicken Sie hier auf **Diagnose**.

    ![][1]

3. Konfigurieren Sie Ihre Diagnose, indem Sie den Status bei **auf** und Auswählen eines Kontos Azure-Speicher zum Speichern der Diagnosedaten. Aktivieren Sie **Kennzahlen**aus, und drücken Sie dann auf **Speichern**. Beachten Sie, dass das Konto Azure-Speicher im Voraus erstellt werden muss und Ihnen separat für Speicher berechnet werden. Sie können auch Ihrer Diagnosedaten an einen Endpunkt Ereignis Hubs senden auswählen.

    ![][2]

4. Nachdem Sie die Diagnose eingerichtet haben, kehren Sie zu der **Übersicht** IoT Hub Blade zurück. Kennzahlen Informationen werden im Abschnitt **Überwachung** des Blades eingetragen. Das Diagramm klicken, wird im Bereich Kennzahlen, wo Sie eine Zusammenfassung der Kennzahlen Informationen für Ihre IoT Hub anzeigen und bearbeiten die Markierung der im Diagramm angezeigten Kennzahlen können, geöffnet. Sie können auch Benachrichtigungen auf der Grundlage metrischen Werte konfigurieren.

    ![][3]

## <a name="metrics-and-how-to-use-them"></a>Kennzahlen und deren Verwendung

IoT Hub bietet mehrere Kennzahlen, damit Sie sich einen Überblick über die Integrität des Ihrer Hub und die Gesamtzahl der verbundenen Geräte können. Sie können Daten aus mehreren Kriterien zum Zeichnen eines Bilds Vergrößern des Status des IoT-Hub kombinieren. Die folgende Tabelle beschreibt die Metrik, die jeder IoT Hub verfolgt und wie jede Metrik auf den allgemeinen Status IoT-Hub bezieht.

| Metrisch | Metrische Beschreibung | Wofür die Metrik verwendet wird |
| ---- | ---- | ---- |
| d2c.telemetry.Ingress.allProtocol | Die Anzahl der auf allen Geräten gesendeten Nachrichten | Übersicht über die Daten auf Nachricht sendet |
| d2c.telemetry.Ingress.Success | Die Anzahl aller Nachrichten, die erfolgreich an den hub | Übersicht über die Meldung zur erfolgreichen eingehende an den hub |
| c2d.Commands.Egress.Complete.Success | Die Anzahl aller Befehl Nachrichten nach dem empfangen Gerät auf allen Geräten abgeschlossen | Zusammen mit der Kennzahlen Abandon und Ablehnen erhalten einen Überblick über die gesamten C2D Befehl Erfolg Zins |
| c2d.Commands.Egress.Abandon.Success | Die Anzahl aller Nachrichten, die erfolgreich abgebrochen, indem das Gerät empfangen auf allen Geräten | Mögliche Probleme hervorgehoben, wenn Nachrichten öfter als erwartet abgebrochen erste sind |
| c2d.Commands.Egress.Reject.Success | Die Anzahl aller Nachrichten, die erfolgreich vom Gerät empfangen auf allen Geräten abgelehnt | Mögliche Probleme hervorgehoben, wenn Nachrichten öfter als erwartet abgelehnt erste werden |
| devices.totalDevices | Der Mittelwert, min und Max die Anzahl der Geräte an den Hub IoT registriert | Die Anzahl der Geräte an den Hub registriert |
| devices.connectedDevices.allProtocol | Der Mittelwert, min und Max die Anzahl der gleichzeitig verbundenen Geräte | Übersicht über die Anzahl der an den Hub angeschlossen Geräte |

## <a name="next-steps"></a>Nächste Schritte

Jetzt, da Sie einen Überblick über die Diagnose Kennzahlen gesehen haben, folgen Sie diesen Link, um weitere Informationen zur Verwaltung von Azure IoT Hub:

- [Überwachung von Vorgängen][lnk-monitor]

Die Funktionen von IoT Hub weiteren kann, finden Sie unter:

- [Leitfaden für Entwickler][lnk-devguide]
- [Ein Gerät mit dem IoT Gateway SDK simulieren][lnk-gateway]

<!-- Links and images -->
[1]: media/iot-hub-metrics/enable-metrics-1.png
[2]: media/iot-hub-metrics/enable-metrics-2.png
[3]: media/iot-hub-metrics/enable-metrics-3.png

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[lnk-operations-monitoring]: iot-hub-operations-monitoring.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-dr]: iot-hub-ha-dr.md

[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md

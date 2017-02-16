<properties
 pageTitle="Azure IoT vorkonfiguriert Lösungen | Microsoft Azure"
 description="Eine Beschreibung der Azure IoT vorkonfiguriert Lösungen und deren Architektur mit Links zu zusätzlichen Ressourcen."
 services=""
 suite="iot-suite"
 documentationCenter=""
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-suite"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/09/2016"
 ms.author="dobett"/>

# <a name="what-are-the-azure-iot-suite-preconfigured-solutions"></a>Was sind die Azure IoT Suite vorkonfiguriert Lösungen?

Die Lösungen Azure IoT Suite vorkonfiguriert werden Implementierungen mit häufigen IoT Lösungsmustern, die Sie mit Ihrem Abonnement Azure bereitstellen können. Sie können die vorkonfigurierten Lösungen verwenden:

- Als Ausgangspunkt für eigene IoT Lösungen.
- Lernen häufige Muster in IoT Lösung entwerfen und entwickeln.

Jede vorkonfigurierten Lösung ist eine vollständige, End-to-End-Implementierung, simulierte Geräten generiert werden.

Zusätzlich zum Bereitstellen und Ausführen der Lösungen in Azure, können Sie den vollständigen Quellcode herunterladen und dann anpassen und erweitern Sie die Lösung, um Ihre spezifischen IoT Bedürfnisse.

> [AZURE.NOTE] Um eine der vorkonfigurierten Solutions bereitstellen zu können, finden Sie auf [Microsoft Azure IoT Suite][lnk-azureiotsuite]. [Erste Schritte mit den Lösungen IoT vorkonfiguriert] Artikel[ lnk-getstarted-preconfigured] enthält weitere Informationen zum Bereitstellen, und führen Sie die Lösungen.

Die folgende Tabelle zeigt, wie die Lösungen Besonderheiten IoT zuordnen:

| Lösung | Erfassung von Daten | Gerät Identität | Befehl und Steuerung | Regeln und Aktionen | Vorhersageanalytik |
|------------------------|-----|-----|-----|-----|-----|
| [Remote-Überwachung][lnk-getstarted-preconfigured] | Ja | Ja | Ja | Ja | -   |
| [Vorhersagen Wartung][lnk-predictive-maintenance] | Ja | Ja | Ja | Ja | Ja |

- *Erfassung von Daten*: eingehende Datenseite bei in der Cloud.
- *Gerät Identität*: eindeutige Identitäten jeder verbundenen Gerät zu verwalten.
- *Befehl und Steuerung*: Senden von Nachrichten an ein Gerät aus der Cloud zu dazu führen, dass das Gerät eine Aktion auszuführen.
- *Regeln und Aktionen*: Lösung Back-End mithilfe von Regeln, die auf bestimmte Geräte-Cloud-Daten angewendet werden.
- *Vorhersageanalytik*: gilt die Lösung Back-End analysiert Gerät Cloud Daten zum Vorhersagen, wenn bestimmte Aktionen ausgeführt werden soll. Analysieren beispielsweise Flugzeug-Engine werden, um zu ermitteln, wann die Wartung Engine fällig ist.

## <a name="remote-monitoring-preconfigured-solution-overview"></a>Remote-Überwachung vorkonfiguriert Lösung (Übersicht)

Wir haben die remote Überwachung vorkonfigurierte Lösung in diesem Artikel diskutieren, da es viele allgemeine Designelemente veranschaulicht, die die anderen Lösungen freigeben.

Das folgende Diagramm veranschaulicht die wichtigsten Elemente der Überwachung remote-Lösung. Die folgenden Abschnitte enthalten weitere Informationen zu diesen Elementen.

![Remote-Überwachung vorkonfiguriert Architektur der Lösung][img-remote-monitoring-arch]

## <a name="devices"></a>Geräte

Wenn Sie die remote Überwachung vorkonfigurierte Lösung bereitstellen, sind vier simulierten Geräte vorab bereitgestellte in die Lösung, die ein Kühlgerät simulieren. Diese simulierten Geräte weisen eine integrierte Temperatur und Feuchtigkeit-Modell, das werden gibt aus. Diese simulierten Geräten sind enthalten, den End-to-End-Datenfluss durch die Lösung veranschaulichen und eine geeignete Informationsquelle werden und ein Ziel für Befehle angeben, wenn Sie die Lösung als Ausgangspunkt für eine benutzerdefinierte Implementierung mit Back-End-Entwickler sind.

Wenn ein Gerät in der remote Überwachung vorkonfigurierten Lösung zuerst mit IoT Hub verbunden, listet die Gerät Informationsnachricht an den Hub IoT die Liste der Befehle, denen auf das Gerät antworten kann. Sind in der remote Überwachung vorkonfigurierten Lösung die Befehle aus: 

- *Ping Gerät*: das Gerät diesen Befehl mit einer Bestätigung beantwortet. Dies ist sinnvoll zur Überprüfung, dass das Gerät noch aktiv und überwachenden ist.
- *Starten Sie werden*: weist das Gerät zu starten, werden senden.
- *Beenden der werden*: weist das Gerät so beenden Sie gesendet werden.
- *Änderung Grenzwert Temperatur*: steuert die simulierten Temperatur werden Werte, die das Gerät sendet. Dies ist nützlich für die Back-End-Logik testen.
- *Diagnose werden*: steuert, wenn das Gerät die externe Temperatur als werden senden soll.
- *Ändern des Status Gerät*.: Legt die Gerät Zustand Metadateneigenschaft, die das Gerät Berichte. Dies ist nützlich für die Back-End-Logik testen.

Sie können weitere simulierte Geräten die Lösung, die die gleichen werden Ausgeben von und Antworten auf die gleichen Befehle hinzufügen. 

## <a name="iot-hub"></a>IoT-Hub

In dieser vorkonfigurierten Lösung, die Instanz IoT Hub *Cloud Gateway* in eine normale [IoT Lösungsarchitektur]entspricht[lnk-what-is-azure-iot].

Ein IoT-Hub empfängt werden von den Geräten an einen einzelnen Endpunkt. Ein Hub IoT verwaltet auch Gerät bestimmte Endpunkte, aus dem jeder Geräte die Befehle abrufen können, die an sie gesendet werden.

IoT-Hub bereitgestellt, die empfangenen werden durch den Dienst angeordneten werden Endpunkt lesen.

## <a name="azure-stream-analytics"></a>Azure Stream Analytics

Die vorkonfigurierte Lösung verwendet drei [Azure Stream Analytics] [ lnk-asa] (ASA) Aufträge zum Filtern von Streams werden von den Geräten:


- *DeviceInfo Auftrag* - gibt Daten an einen Hub an, mit dem Gerät Registrierung bestimmte Nachrichten gesendet, wenn ein Gerät zuerst eine Verbindung herstellt, oder als Antwort auf einen Befehl **Ändern Gerät Zustand** zur Lösung Gerät Registrierung (eine Datenbank DocumentDB) weitergeleitet Ereignis aus. 
- *Position werden* – sendet alle unformatierten werden in Azure Blob-Speicher für Haut Speicher und berechnet werden Aggregationen, die in der Lösung Dashboard angezeigt.
- *Regeln Auftrag* - Filter werden Streams für Werte, die eine Regelschwellenwerte überschreiten, und gibt die Daten an einen Hub Ereignis. Wenn eine Regel wird ausgelöst, wird die Lösung Portal Dashboard-Ansicht zeigt das Ereignis als neue Zeile in der Verlaufstabelle Erinnerung und löst eine Aktion basierend auf der Einstellungen für die Regeln und Aktionen Sichten im Portal Lösung definiert.

In dieser Lösung vorkonfigurierten bilden die Einzelvorgänge ASA Teil der **Lösung IoT back-End** in einer normalen [IoT Lösungsarchitektur][lnk-what-is-azure-iot].

## <a name="event-processor"></a>Ereignisprozessor

In dieser Lösung vorkonfigurierten Formularen der Ereignisprozessor Teil der **Lösung IoT back-End** in einer normalen [IoT Lösungsarchitektur][lnk-what-is-azure-iot].

Die Einzelvorgänge **DeviceInfo** und **Regeln** ASA deren Ausgabe an Ereignis Hubs für die Übermittlung an andere Back-End-Dienste zu senden. Die Lösung verwendet eine [EventPocessorHost] [ lnk-event-processor] Instanz, die in einer [WebJob]ausführen[lnk-web-job], lesen Sie die Nachrichten aus diesen Hubs Ereignis. Die **EventProcessorHost** verwendet die **DeviceInfo** Daten zum Aktualisieren von Gerätedaten in der Datenbank DocumentDB, und verwendet die Daten von **Regeln** zum Aufrufen der Logik app und die Aktualisierung die Benachrichtigungen im Portal Lösung anzuzeigen.

## <a name="device-identity-registry-and-documentdb"></a>Gerät Identität Registrierung und DocumentDB

Jeder IoT-Hub enthält ein [Gerät Identität Registrierung] [ lnk-identity-registry] , in dem Gerät Schlüssel gespeichert. IoT Hub verwendet diese Informationen authentifizieren Geräte – ein Gerät muss registriert sein und einen gültigen Key haben, bevor sie an den Hub eine Verbindung herstellen kann.

Diese Lösung speichert Weitere Informationen zu Geräten wie ihren Status, die von Ihnen unterstützten Befehle und sonstige Metadaten. Die Lösung verwendet eine Datenbank DocumentDB Speichern der Lösung Geräte-Daten und des Portals Lösung Ruft Daten aus dieser Datenbank DocumentDB für anzeigen und bearbeiten.

Die Lösung muss auch die Informationen in der Registrierung Geräts Identität synchronisiert mit dem Inhalt der Datenbank DocumentDB beibehalten. Die **EventProcessorHost** verwendet die Daten aus **DeviceInfo** Stream Analytics Auftrag die Synchronisierung verwalten.

## <a name="solution-portal"></a>Lösung-portal

![Lösung dashboard][img-dashboard]

Das Lösung-Portal ist eine webbasierte Benutzeroberfläche, die in der Cloud als Teil der vorkonfigurierten Lösung bereitgestellt wird. Sie können:

- Hier werden und Erinnerung Historie in einem Dashboard angezeigt.
- Bereitstellen von neuen Geräten.
- Verwalten und Überwachen von Geräten.
- Senden Sie Befehle an bestimmte Geräte.
- Verwalten von Regeln und Aktionen.

In dieser Lösung vorkonfigurierten Formularen im Portal Lösung Teil der **Lösung IoT back-End** und Teil der **Verarbeitung und Business Connectivity** in der Regel [IoT Lösungsarchitektur][lnk-what-is-azure-iot].

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu IoT Lösungsarchitekturen, finden Sie unter [Microsoft Azure IoT Services: Verweis Architektur][lnk-refarch].

Nun wissen Sie, welche eine vorkonfigurierte Lösung ist, Sie können beginnen durch die Bereitstellung der Lösung für die *remote-Überwachung* vorkonfiguriert: [Erste Schritte mit den vorkonfigurierten Lösungen][lnk-getstarted-preconfigured].

[img-remote-monitoring-arch]: ./media/iot-suite-what-are-preconfigured-solutions/remote-monitoring-arch1.png
[img-dashboard]: ./media/iot-suite-what-are-preconfigured-solutions/dashboard.png
[lnk-what-is-azure-iot]: iot-suite-what-is-azure-iot.md
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-event-processor]: ../event-hubs/event-hubs-programming-guide.md#event-processor-host
[lnk-web-job]: ../app-service-web/web-sites-create-web-jobs.md
[lnk-identity-registry]: ../iot-hub/iot-hub-devguide-identity-registry.md
[lnk-predictive-maintenance]: iot-suite-predictive-overview.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
[lnk-getstarted-preconfigured]: iot-suite-getstarted-preconfigured-solutions.md
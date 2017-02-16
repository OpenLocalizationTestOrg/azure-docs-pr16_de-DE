<properties
 pageTitle="IoT Hub HA und DR | Microsoft Azure"
 description="Beschreibt die Features, mit deren Hilfe hoch verfügbaren IoT Lösungen mit Disaster Wiederherstellungsfunktionen erstellen."
 services="iot-hub"
 documentationCenter=""
 authors="fsautomata"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="02/03/2016"
 ms.author="elioda"/>

# <a name="iot-hub-high-availability-and-disaster-recovery"></a>Hohe Verfügbarkeit und Disaster Wiederherstellung IoT-Hub

Als Azure-Dienst bietet IoT Hub hohen Verfügbarkeit Redundanzen Azure Region auf oberster Ebene ohne zusätzlichen Aufwand erforderlich, indem Sie die Lösung verwenden. Azure bietet darüber hinaus eine Reihe von Funktionen, mit deren Hilfe um Lösungen mit Disaster Wiederherstellung (DR) Funktionen oder Cross-Region Verfügbarkeit zu erstellen, falls erforderlich. Sie müssen entwerfen und Vorbereiten die Lösungen für diese DR-Funktionen, wenn Sie globale angeben möchten, Cross-Region hohen Verfügbarkeit für Geräte oder Benutzer nutzen. Im Artikel [Azure Business Continuity technischen Leitfaden](../resiliency/resiliency-technical-guidance.md) beschreibt die integrierten Funktionen in Azure Geschäftskontinuität und DR an. Das Papier [Wiederherstellung und hohe Verfügbarkeit für Azure Applications][] bietet Architektur Anleitungen auf Strategien für Azure Applications HA und DR erzielen.

## <a name="azure-iot-hub-dr"></a>Azure IoT Hub DR
Zusätzlich zu innerhalb-Region HA implementiert IoT Hub Failovermechanismen für die Wiederherstellung, die keinen Eingriff des Benutzers erforderlich ist. IoT Hub DR Self initiiert, und verfügt über eine Wiederherstellung Zeit Ziel (RTO) von 2-26 Stunden und folgende Wiederherstellung-Ziele (RPOs).

| Funktionalität | RPO |
| ------------- | --- |
| Verfügbarkeit von Diensten für Registrierung und Kommunikation Vorgänge | Mögliche CName Verlust |
| Identitätsdaten in der Registrierung des Geräts Identität | 0-5 Minuten Datenverlust |
| Gerät-Cloud-Nachrichten | Aller ungelesene Nachrichten verloren. |
| Überwachung von Nachrichten von Vorgängen | Aller ungelesene Nachrichten verloren. |
| Cloud-zu-Gerät-Nachrichten | 0-5 Minuten Datenverlust |
| Cloud-zu-Gerät Feedback Warteschlange | Aller ungelesene Nachrichten verloren. |

## <a name="regional-failover-with-iot-hub"></a>Landes-/ Failover mit IoT-Hub

Eine ausführliche Erläuterung von Bereitstellungstopologien in IoT Lösungen befindet sich außerhalb dieses Artikels, aber zum Zweck der hohen Verfügbarkeit und Wiederherstellung wir das Modell zur Bereitstellung von *regionalen Failover* berücksichtigt.

In einem Modell Landes-/ Failover Lösung Back-End hauptsächlich in einem Datacenter Ort ausgeführt wird, jedoch eine zusätzliche IoT Hub und Back-End in einen anderen Datacenter-Zielort für Failoverzwecke, bereitgestellt werden, falls der IoT Hub im primären Rechenzentrum einen Ausfall weist oder die Netzwerkkonnektivität vom Gerät zu primären Datencenter aus irgendeinem Grund unterbrochen. Geräte verwenden einen sekundären-Endpunkt aus, wenn der primären Gateways erreicht werden kann. Mit einer Failoverfähigkeit Cross-Region kann die Lösung Verfügbarkeit jenseits der hohen Verfügbarkeit von Bereich mit einem einzelnen verbessert werden.

Auf hoher Ebene werden zum Implementieren eines Modells Landes-/ Failover mit IoT-Hub benötigen Sie Folgendes.

* **Eine sekundäre IoT Hub und routing Logik Gerät**: im Fall einer dienststörung in Ihrem primären Region, müssen Geräte starten, Herstellen einer Verbindung mit Ihrem sekundären Region. Ausgehend von der Art-Unterstützung von den meisten Dienste beteiligt sind, ist es für Administratoren der Lösung zum Auslösen des zwischen Region Failovervorgangs üblich. Lernen Sie den neuen Endpunkt an Geräten kommunizieren, bei weitgehender Kontrolle über den Prozess ist bitten einen *Concierge* -Dienst für den aktuellen aktiven Endpunkt regelmäßig zu überprüfen. Der Concierge-Dienst kann eine einfache Anwendung, die repliziert und gehalten erreichbar ist nicht mit DNS-Umleitung Techniken (z. B. mit [Azure Datenverkehr Manager][]).
* **Identität Registrierungsreplikation** - um verwendet, werden die sekundäre IoT Hub alle Gerät Identitäten enthalten muss, die auf die Lösung zugreifen können. Die Lösung sollte Geo repliziert Sicherungen Gerät Identitäten beibehalten, und klicken Sie auf im sekundären IoT Hub vor dem Umschalten von des aktiven Endpunkts für die Geräte hochladen. Die Funktion des Geräts Identität Exportieren von IoT Hub ist sehr hilfreich, in diesem Zusammenhang. Weitere Informationen finden Sie unter [IoT Hub Developer Guide - Identität Registrierung][].
* **Zusammenführen von Logik** – Wenn Sie die primäre Region wieder zur Verfügung steht, zurücksetzen, die den Status und die Daten, die in den sekundären Standort erstellt wurden migriert werden müssen in der primären Region. Dies bezieht sich hauptsächlich auf Gerät Identitäten und Anwendung Metadaten, die mit der primäre IoT Hub und anderen anwendungsspezifische speichern in der primären Region zusammengeführt werden müssen. Um diesen Schritt zu vereinfachen, empfiehlt es in der Regel, dass Sie Idempotent Operationen verwenden. Dadurch wird die Seite Effekte nicht nur vom tatsächlichen konsistent Verteilung von Ereignissen, sondern auch Duplikate oder außerhalb der Reihenfolge Senden von Ereignissen minimiert. Darüber hinaus sollten die Logik der entworfen werden, um potenzielle Inkonsistenzen oder von Datum Status "geringfügig" tolerieren. Dies basiert auf Wiederherstellung Punkt Ziele (RPO) aufgrund der zusätzlichen Zeitaufwand für das System, um "lösen".

## <a name="next-steps"></a>Nächste Schritte

Führen Sie die folgenden Links, um weitere Informationen zur Azure IoT Hub:

- [Erste Schritte mit IoT Hubs (Lernprogramm)][lnk-get-started]
- [Was ist Azure IoT Hub?][]

[Wiederherstellung und hohe Verfügbarkeit für Azure applications]: ../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md
[Azure Business Continuity Technical Guidance]: https://azure.microsoft.com/documentation/articles/resiliency-technical-guidance/
[Azure Datenverkehr-Manager]: https://azure.microsoft.com/documentation/services/traffic-manager/
[IoT Hub Developer Guide - Identität Registrierung]: iot-hub-devguide-identity-registry.md

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[Was ist Azure IoT Hub?]: iot-hub-what-is-iot-hub.md

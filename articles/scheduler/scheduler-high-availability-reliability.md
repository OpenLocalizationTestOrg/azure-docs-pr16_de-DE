<properties
 pageTitle="Scheduler hohe Verfügbarkeit und Zuverlässigkeit"
 description="Scheduler hohe Verfügbarkeit und Zuverlässigkeit"
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.date="08/16/2016"
 ms.author="deli"/>


# <a name="scheduler-high-availability-and-reliability"></a>Scheduler hohe Verfügbarkeit und Zuverlässigkeit

## <a name="azure-scheduler-high-availability"></a>Azure Scheduler hoher Verfügbarkeit

Als eine Core Azure Platform-Dienst Azure Scheduler hochgradig steht und verfügt über redundante Geo Service-Bereitstellung und Replikation Geo-Regions-Position.

### <a name="geo-redundant-service-deployment"></a>Geo redundante Service-Bereitstellung

Azure Scheduler steht über die Benutzeroberfläche in fast allen Geo Regionen, die in Azure heute ist. Die Liste der Gebiete, denen in Azure Scheduler zur Verfügung ist [hier aufgeführt](https://azure.microsoft.com/regions/#services). Wenn einem Data Center in einer gehosteten Region nicht verfügbar wiedergegeben wird, sind die Failoverfunktionen des Azure Scheduler an, dass der Dienst von einem anderen Datacenter verfügbar ist.

### <a name="geo-regional-job-replication"></a>Geo-Regions-Auftrag Replikation

Es ist nicht nur der Scheduler Azure Front-End-Management Anfragen, aber Ihre eigenen Arbeit auch Geo repliziert zur Verfügung. Bei ein Ausfall in einer Region, Azure Scheduler über schlägt fehl, und stellt sicher, dass der Auftrag aus einem anderen Data Center in der für geografische Region ausgeführt wird.

Wenn Sie ein Projekt in Süd zentralen uns erstellt haben, repliziert Azure Scheduler beispielsweise automatisch die Position in US North Central. Wenn ein Fehler in Süd zentralen uns vorliegt, Azure Scheduler ist sichergestellt, dass der Auftrag von US North Central ausgeführt wird. 

![][1]

Daher ist Azure Scheduler sichergestellt, dass die Daten innerhalb der gleichen breitere geografische Region im Fall eines Fehlers Azure bleibt. Daher müssen Sie Ihre Arbeit einfach, um hohe Verfügbarkeit hinzufügen nicht duplizieren – Azure Scheduler bietet automatisch hoher Verfügbarkeit-Funktionen für Ihre Projekte.

## <a name="azure-scheduler-reliability"></a>Azure Scheduler Zuverlässigkeit

Azure Scheduler garantiert eine eigene hoher Verfügbarkeit und bietet einen anderen Ansatz für Aufträge Benutzer erstellt. Ihre Aufgabe kann beispielsweise einen HTTP-Endpunkt aufrufen, der nicht verfügbar ist. Azure Scheduler versucht trotzdem Ihre Arbeit erfolgreich durch benennen Sie alternative Optionen für den Umgang mit Fehler ausgeführt. Azure Scheduler macht's auf zwei Arten:

### <a name="configurable-retry-policy-via-retrypolicy"></a>Konfigurierbare wiederholen Richtlinie über "RetryPolicy"

Azure Scheduler können Sie eine Richtlinie "Wiederholen" konfigurieren. Standardmäßig Wenn ein Auftrag fehlschlägt, versucht Scheduler den Auftrag erneut viermal, in Intervallen von 30 Sekunden. Konfigurieren Sie möglicherweise dieser Richtlinie "Wiederholen" benutzerspezifisch kürzere (z. B. zehn Mal in Intervallen von 30 Sekunden) oder lockereres (z. B. zweimal, in täglichen Intervallen.)

Als Beispiel wenn dies möglicherweise helfen möglicherweise Sie ein Projekt erstellen, wird wöchentlich ausgeführt und ruft einen HTTP-Endpunkt. Ist der HTTP-Endpunkt nach unten für ein paar Stunden Wenn Ihre Arbeit ausgeführt wird, möchten Sie möglicherweise nicht warten, eine weitere Woche für das Projekt erneut ausführen, da auch die Standardrichtlinie "Wiederholen" fehl. In diesen Fällen können Sie die Richtlinie standard "Wiederholen" alle drei Stunden (zum Beispiel) zu wiederholen, statt alle 30 Sekunden erneut konfigurieren.

Informationen zum Konfigurieren einer Richtlinie "Wiederholen" [RetryPolicy](scheduler-concepts-terms.md#retrypolicy)finden.

### <a name="alternate-endpoint-configurability-via-erroraction"></a>Alternative Endpunkt anspruchsvolle über "ErrorAction"

Wenn der Zielendpunkt für den Job Azure Scheduler nicht erreichbar bleibt, greift Azure Scheduler zurück an die alternative Fehlerbehandlung Endpunkt nach deren Richtlinie "Wiederholen" folgen. Wenn ein alternativer Fehlerbehandlung Endpunkt konfiguriert ist, wird Sie Azure Scheduler. Mit einem alternativen Endpunkt sind Eigene Aufträge unter Fehler hoch verfügbar.

Als Beispiel, in der nachstehenden Abbildung folgt Azure Scheduler seine "Wiederholen" Richtlinie einen Webdienst New York Treffer werden soll. Nachdem die Versuche fehlschlagen, überprüft, ob eine alternative vorhanden ist. Klicken Sie dann im Voraus bewegt, und Ausführen einer Anforderung auf die alternative mit der gleichen "Wiederholen" Richtlinie beginnt.

![][2]

Beachten Sie, dass die gleiche "Wiederholen" Richtlinie gilt für sowohl die ursprüngliche Aktion und den alternativen zurück. Es ist auch möglich, dass die alternative Fehleraktion Aktionstyp von der Hauptseite Aktion Aktionstyp abweichen. Beispielsweise während die Hauptfenster Aktion einen HTTP-Endpunkt aufrufen kann, die Fehleraktion stattdessen möglicherweise ein Speicherwarteschlange, Service Bus Warteschlange, oder Service Bus Thema Aktion, die zurück-Protokollierung unterstützt.

Informationen zum Konfigurieren eines alternativen Endpunkts [ErrorAction](scheduler-concepts-terms.md#action-and-erroraction)finden.

## <a name="see-also"></a>Siehe auch

 [Was ist der Scheduler?](scheduler-intro.md)

 [Azure Scheduler Konzepte und Terminologie Entitätshierarchie](scheduler-concepts-terms.md)

 [Erste Schritte mit Scheduler Azure-Portal](scheduler-get-started-portal.md)

 [Pläne und Abrechnung in Azure Scheduler](scheduler-plans-billing.md)

 [So erstellen Sie komplexe Zeitpläne und erweiterte Serie mit Azure Scheduler](scheduler-advanced-complexity.md)

 [Azure Scheduler REST-API-Referenz](https://msdn.microsoft.com/library/mt629143)

 [Bezug auf Azure Scheduler PowerShell-cmdlets](scheduler-powershell-reference.md)

 [Grenzwerte für Azure Scheduler, Standardeinstellungen und Fehlercodes](scheduler-limits-defaults-errors.md)

 [Azure Scheduler ausgehende Authentifizierung](scheduler-outbound-authentication.md)


[1]: ./media/scheduler-high-availability-reliability/scheduler-high-availability-reliability-image1.png

[2]: ./media/scheduler-high-availability-reliability/scheduler-high-availability-reliability-image2.png

<properties
    pageTitle="Datenverkehr-Manager – Datenverkehr für die Weiterleitung Methoden | Microsoft Azure"
    description="Dieser Artikel hilft Ihnen die unterschiedlichen Verkehr Weiterleitung verwendeten Methoden vom Datenverkehr-Manager zu verstehen."
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="traffic-manager-traffic-routing-methods"></a>Datenverkehr Manager Datenverkehr-routing Methoden

Azure Datenverkehr Manager unterstützt drei Datenverkehr-routing Methoden zum Bestimmen der Netzwerkdatenverkehr an die verschiedenen Service Endpunkte weiterzuleiten. Datenverkehr Manager gilt für den Datenverkehr-routing Methode jede DNS-Abfrage, die sie empfängt. Die Datenverkehr-routing Methode bestimmt, welche Endpunkt in der DNS-Antwort zurückgegeben.

Die Ressourcenmanager Azure-Unterstützung für den Datenverkehr Manager verwendet andere Terminologie als das Bereitstellungsmodell klassischen. Die folgende Tabelle zeigt die Unterschiede zwischen der Ressourcenmanager und klassischen Ausdrücke:

| Ressourcenmanager Ausdruck | Klassische Ausdruck |
|-----------------------|--------------|
| Datenverkehr-routing Methode | Lastenausgleich Methode |
| Priorität-Methode | Failover-Methode |
| Gewichteten Methode | Round-Robert-Methode |
| Leistung-Methode | Leistung-Methode |

Basierend auf Feedback von Kunden, geändert wir die Terminologie zum Verbessern der übersichtlich und häufige Missverständnisse zu verringern. Es gibt keinen Unterschied zwischen Funktionalität.

Es gibt drei routing Datenverkehr-Methoden in den Datenverkehr Manager verfügbar:

- **Priorität:** Wählen Sie 'Priorität', wenn Sie möchten, verwenden einen primären-Endpunkt für den gesamten Verkehr und Sicherungen bereitstellen, für den Fall, dass die primäre oder die Sicherung Endpunkte nicht verfügbar sind.
- **Gewichteten:** Option 'gewichteten' Wenn Sie den Datenverkehr auf eine Reihe von Endpunkten verteilen möchten, definieren entweder gleichmäßig oder entsprechend Breiten, die Sie.
- **Leistung:** Wählen Sie "Leistung", wenn Sie die Endpunkte an unterschiedlichen geografischen Standorten haben und Endbenutzer den "nächstliegende" Endpunkt im Hinblick auf den niedrigsten Netzwerkwartezeit verwendet werden soll.

Alle Datenverkehr Manager Profile gehören Endpunkt Gesundheit und automatische Endpunkt Failover zu überwachen. Weitere Informationen finden Sie unter [Den Datenverkehr Manager Endpunkt überwachen](traffic-manager-monitoring.md). Ein einzelnes Datenverkehr-Manager-Profil kann nur eine Datenverkehr routing Methode verwenden. Sie können eine andere Datenverkehr routing Methode zu einem beliebigen Zeitpunkt für Ihr Profil auswählen. Änderungen, die innerhalb einer Minute angewendet werden, und Störungsfreiheit entsteht. Datenverkehr-routing Methoden können mithilfe von verschachtelten Datenverkehr Manager Profile kombiniert werden. Schachtelungsebenen ermöglicht anspruchsvolle und flexible Datenverkehr-routing Konfigurationen, die den Anforderungen der größeren, komplexe Applications. Weitere Informationen finden Sie unter [geschachtelte Datenverkehr-Manager-Profilen](traffic-manager-nested-profiles.md).

## <a name="priority-traffic-routing-method"></a>Priorität Datenverkehr-routing Methode

Häufig möchte eine Organisation bereitstellen Zuverlässigkeit für seine Services durch die Bereitstellung von einen oder mehrere zusätzliche Dienste für den Fall, dass ihre primäre Service-fällt aus. Die 'Priorität' Datenverkehr-routing Methode kann Azure Kunden dieses Failover Muster leicht zu implementieren.

![Azure Datenverkehr-Manager 'Priorität' Datenverkehr-routing Methode][1]

Das Profil Datenverkehr Manager enthält eine nach Priorität sortierte Liste der Endpunkte. Standardmäßig sendet den Datenverkehr Manager gesamten Verkehr an den Endpunkt der primären (höchste Priorität). Wenn der primäre Endpunkt nicht verfügbar ist, leitet Datenverkehr Manager den Datenverkehr an den zweiten Endpunkt an. Wenn die Endpunkte primären und sekundären nicht verfügbar sind, geht der Datenverkehr an Dritte usw. aus. Verfügbarkeit von den Endpunkt basiert auf den konfigurierten Status (aktiviert oder deaktiviert) und die laufenden Endpunkt Überwachung.

### <a name="configuring-endpoints"></a>Konfigurieren von Endpunkten

Mit Azure Ressourcenmanager, konfigurieren Sie die Endpunkt Priorität verwenden die Prioritätseigenschaft '' explizit für jeden Endpunkt. Diese Eigenschaft ist ein Wert zwischen 1 und 1000. Niedrigere Werte stellen eine höhere Priorität dar. Prioritätswerte gemeinsam nicht verwenden. Festlegen der Eigenschaft ist optional. Wenn nicht angegeben, wird standardmäßig die Priorität basierend auf der Reihenfolge der Endpunkt verwendet.

Die Endpunkt Priorität ist mit der Oberfläche Classic implizit konfiguriert. Die Priorität basiert auf der Reihenfolge, in der die Endpunkte in der Profildefinition aufgeführt sind.

## <a name="weighted-traffic-routing-method"></a>Gewichteten Datenverkehr-routing Methode

Die 'Gewichtete' Datenverkehr-routing Methode können Sie die Verkehr gleichmäßig verteilen oder verwenden Sie eine vordefinierte Gewichtung.

![Azure Datenverkehr-Manager 'Gewichteten' Datenverkehr-routing Methode][2]

In der gewichteten Datenverkehr-routing Methode weisen Sie eine Stärke jedem Endpunkt im Datenverkehr Manager Profil Konfiguration aus. Die Stärke ist eine Ganzzahl zwischen 1 und 1000. Für diesen Parameter ist optional. Wenn nicht angegeben, wird Datenverkehr Projektmanager einer standardmäßigen Stärke von "1" verwendet.

Für jede DNS-Abfrage erhalten wählt den Datenverkehr Manager zufällig einen Endpunkt aus. Die Wahrscheinlichkeit, einen Endpunkt basiert auf der Gewichtung, die alle verfügbaren Endpunkte zugeordnet. Verwenden dasselbe Gewicht über alle Endpunkte Ergebnisse in einer Verteilung gerade Datenverkehr an. Verwenden höhere oder niedrigere Gewichtung auf bestimmte Endpunkte bewirkt, dass diese Endpunkte mehr oder weniger häufig in die DNS-Antworten zurückgegeben werden soll.

Die gewichtete Methode können einige hilfreiche Szenarien:

- Schrittweise Anwendungsupgrade: einen Prozentsatz des Datenverkehrs zum Weiterleiten an einen neuen Endpunkt zuweisen und den Datenverkehr über einen Zeitraum auf 100 % graduell zu erhöhen.
- Anwendungsmigration in Azure: Erstellen eines Profils mit Azure und externen Endpunkte. Passen Sie die Stärke der Endpunkte, die neuen Endpunkte den Vorzug.
- Für zusätzliche Kapazität Cloud trennen: schnell eine lokalen Bereitstellung in der Cloud zu erweitern, indem Sie es hinter den Datenverkehr Manager Profil platzieren. Wenn Sie zusätzliche Kapazität in der Cloud benötigen, können hinzufügen oder weitere Endpunkte aktivieren und angeben, welcher Teil des Datenverkehrs jedem Endpunkt wechselt.

Das neue Azure-Portal unterstützt die Konfiguration des gewichteten Datenverkehrs-routing. -Stärken können nicht in der klassischen Portal konfiguriert werden. Sie können auch mithilfe der Ressourcenmanager und klassischen Versionen des Azure PowerShell, CLI und die restlichen APIs-Stärken konfigurieren.

Es ist wichtig zu verstehen, dass die DNS-Antworten zwischengespeichert werden, indem Clients und die rekursive DNS-Server, mit denen die Clients DNS-Namen auflösen. Diese Zwischenspeichern kann auf gewichteten Datenverkehr Verteilung auswirken. Wenn die Anzahl der Clients und rekursive DNSServers umfangreich ist, funktioniert den Datenverkehr Verteilung wie erwartet. Jedoch, wenn die Anzahl der Kunden oder rekursive DNSServers klein ist, kann Zwischenspeichern erheblich Datenverkehr Verteilung für die Neigung.

Gemeinsame Verwendung Fällen umfassen:

- Entwicklung und Testen Umgebungen
- Anwendung auf Kommunikation
- Applikationen richtet sich an eine schmale Benutzer-Basis, die eine allgemeine rekursive DNS-Infrastruktur (z. B. Mitarbeiter eines Unternehmens Herstellen einer Verbindung über einen Proxy) freigeben

Diese DNS-Einträge zwischenspeichern Effekte gelten für alle DNS-basierte Datenverkehr Weiterleitung Systeme, nicht nur Azure Datenverkehr Manager zur Verfügung. In einigen Fällen kann explizit Löschen des DNS-Caches zur Umgehung dieses Problems vorsehen. In anderen Fällen ist eine alternative Datenverkehr-routing Methode besser geeignet.

## <a name="performance-traffic-routing-method"></a>Leistung Datenverkehr-routing Methode

Bereitstellen von Endpunkten in zwei oder mehr Orte auf der ganzen Welt kann Verbesserung der Reaktionszeiten viele Clientanwendungen durch routing des Datenverkehrs an die Position, die nächsten Ihnen ist. Diese Funktion wird die "Leistung" Datenverkehr-routing Methode bereitgestellt.

![Azure Datenverkehr Manager "Leistung" Datenverkehr-routing Methode][3]

Der Endpunkt 'am nächsten' ist nicht unbedingt nächstgelegenen gemessen nach geografischen Abstand. Die "Leistung" Datenverkehr-routing Methode bestimmt stattdessen den nächsten Endpunkt durch Messen Netzwerkwartezeit an. Datenverkehr Manager unterhält eine Internet Wartezeit Tabelle zum Nachverfolgen der Roundtripzeit zwischen IP-Adressbereiche und jede Azure Datacenter.

Datenverkehr Manager sucht die Quelle IP-Adresse der eingehenden Anforderung DNS-Einträge in der Tabelle der Internet Wartezeit aus. Datenverkehr Manager wählt einen Endpunkt im Azure Datencenter, das weist die niedrigste Wartezeit für die IP-Adressbereich aus und gibt dann diesen Endpunkt in der DNS-Antwort an.

Wie unter [Funktionsweise der Datenverkehr Manager](traffic-manager-how-traffic-manager-works.md)erläutert wird, erhält Datenverkehr Manager DNS-Abfragen nicht direkt von Clients. DNS-Abfragen stammen lieber, der rekursive DNS-Dienst, dass die Clients für die Verwendung konfiguriert sind. Daher die IP-Adresse verwendet, um der Endpunkt 'am nächsten' ist nicht die IP-Adresse des Clients, aber es ist die IP-Adresse des DNS-Diensts rekursive ermitteln. In der Praxis ist diese IP-Adresse ein guter Proxy für den Client.

Datenverkehr Manager aktualisiert regelmäßig die Tabelle Internet Wartezeit, um die Änderungen in der globalen Internet und neue Azure Regionen zu berücksichtigen. Jedoch Leistung der Anwendung variiert je nach Variationen in Echtzeit beim Laden über das Internet. Leistung Datenverkehr-routing überwacht nicht auf einen bestimmten Dienstendpunkt laden. Wenn Sie ein Endpunkt nicht mehr verfügbar ist, jedoch Datenverkehr Manager es im Antworten auf DNS-Abfragen.

Punkte beachten:

- Wenn Ihr Profil mehrere Endpunkte in der gleichen Azure Region enthält, verteilt dann Datenverkehr Manager Verkehr gleichmäßig auf die verfügbaren Endpunkte in diesem Bereich. Falls eine anderen Datenverkehr Verteilung innerhalb eines Bereichs gewünscht, können Sie [geschachtelte Datenverkehr-Manager-Profilen](traffic-manager-nested-profiles.md)verwenden.

- Wenn alle aktiviert Endpunkte in einer bestimmten Azure Region heruntergestuft werden, verteilt Datenverkehr Manager Verkehr auf anderen verfügbaren Endpunkte anstelle der weiter nächstgelegenen Endpunkt an. Diese Logik wird verhindert, dass einen cascading Fehler auftreten, indem Sie nicht den Endpunkt weiter nächstgelegenen überladen. Wenn Sie eine bevorzugte Failover Folge festlegen möchten, verwenden Sie [verschachtelte Datenverkehr-Manager-Profilen](traffic-manager-nested-profiles.md).

- Wenn die Leistung Datenverkehr routing Methode mit externen Endpunkte oder geschachtelte Endpunkte verwenden zu können, müssen Sie den Speicherort der diese Endpunkte anzugeben. Wählen Sie Ihre Azure Region der Bereitstellung. Diese Speicherorte sind die Werte außerhalb der Internet Wartezeit Tabelle.

- Der Algorithmus, der den Endpunkt wählt ist deterministisch. Wiederholte DNS-Abfragen vom selben Client werden an den gleichen Endpunkt geleitet. Normalerweise werden Clients mit verschiedenen rekursive DNS-Server wenn unterwegs. Der Client kann zu einem anderen Endpunkt weitergeleitet werden. Routing kann auch von Updates zur Tabelle Wartezeit Internet beeinflusst werden. Die Leistung Datenverkehr-routing Methode garantiert daher nicht, dass ein Client immer an dieselbe IP-Adresse weitergeleitet wird.

- Sobald der Internet Wartezeit Tabelle geändert hat, werden Sie eventuell feststellen, dass einige Clients zu einem anderen Endpunkt weitergeleitet werden. Diese Änderung Weiterleitung ist genauer basierend auf dem aktuellen Wartezeit Daten. Diese Updates sind für die Genauigkeit der Leistung Datenverkehr-routing verwalten im Internet ständig Weiterentwicklung unverzichtbar.

## <a name="next-steps"></a>Nächste Schritte

Informationen Sie zum Entwickeln hoher Verfügbarkeit von Applications [Datenverkehr Manager Endpunkt Überwachung](traffic-manager-monitoring.md) verwenden

Informationen zum [Erstellen eines Profils Datenverkehr-Managers](traffic-manager-manage-profiles.md)

<!--Image references-->
[1]: ./media/traffic-manager-routing-methods/priority.png
[2]: ./media/traffic-manager-routing-methods/weighted.png
[3]: ./media/traffic-manager-routing-methods/performance.png

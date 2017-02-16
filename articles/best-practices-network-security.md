<properties
   pageTitle="Microsoft-Cloud-Diensten und Netzwerk-Sicherheit | Microsoft Azure"
   description="Erfahren Sie mehr über die wichtigsten Features in Azure verwaltenden sicheren Netzwerk-Umgebungen"
   services="virtual-network"
   documentationCenter="na"
   authors="tracsman"
   manager="rossort"
   editor=""/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/14/2016"
   ms.author="jonor;sivae"/>

# <a name="microsoft-cloud-services-and-network-security"></a>Microsoft-Cloud-Diensten und Netzwerk-Sicherheit

Microsoft-Cloud-Diensten vorführen Hyperscale Services und Infrastruktur, unternehmensweite Funktionen und viele Auswahlmöglichkeiten für Hybrid Connectivity. Kunden können diese Dienste über das Internet oder mit Azure ExpressRoute, die als "Privat" Netzwerkkonnektivität bereitstellt Zugriff auf auswählen. Die Microsoft Azure-Plattform kann Kunden nahtlos erweitern ihre Infrastruktur in der Cloud zu erstellen und mehrstufige Architekturen. Darüber hinaus können dritte erweiterte Funktionen aktivieren, durch das Angebot Security Services und virtuelle Einheiten. In diesem Whitepaper bietet einen Überblick über die Sicherheit und Architektur Probleme, die Kunden bei Verwendung von Microsoft-Cloud-Diensten über ExpressRoute zugegriffen berücksichtigen sollten. Es wird beschrieben, sicherer Dienste in Azure virtuelle Netzwerke erstellen.

## <a name="fast-start"></a>Schnellstart
Das folgende Diagramm der Logik können Sie ein bestimmtes Beispiel der vielen Sicherheit Techniken zur Verfügung, mit der Azure-Plattform direkte. Kurzübersicht finden Sie im Beispiel, dass am besten Ihr Fall entspricht. Genauere erläuterungen weiterhin über das Papier lesen.
![Flussdiagramm für die Sicherheit Optionen][0]

[Beispiel 1: Erstellen einer Shape-Netzwerk (auch als DMZ, demilitarisierte Zone und überwachtes Subnetz) zum Schutz von Applications mit Netzwerk-Sicherheitsgruppen (NSGs).](#example-1-build-a-simple-dmz-with-nsgs)</br>
[Beispiel 2: Erstellen einer Shape-Netzwerk zum Schutz von Applications mit einer Firewall und NSGs.](#example-2-build-a-dmz-to-protect-applications-with-a-firewall-and-nsgs)</br>
[Beispiel 3: Erstellen einer Shape-Netzwerk zum Schutz von Netzwerken mit einer Firewall, benutzerdefinierte Routing (UDR) und NSG.](#example-3-build-a-dmz-to-protect-networks-with-a-firewall-udr-and-nsg)</br>
[Beispiel 4: Hinzufügen einer Hybrid Verbindungs mit einer Website-zu-Standort, virtuelle Einheit virtuelles privates Netzwerk (VPN).](#example-4-adding-a-hybrid-connection-with-a-site-to-site-virtual-appliance-vpn)</br>
[Beispiel 5: Hinzufügen einer Hybrid-Verbindungs mit einem Gateway-Standorten, Azure VPN.](#example-5-adding-a-hybrid-connection-with-a-site-to-site-azure-gateway-vpn)</br>
[Beispiel 6: Hinzufügen einer Verbindungs Hybrid mit ExpressRoute.](#example-6-adding-a-hybrid-connection-with-expressroute)</br>
Beispiele für das Hinzufügen von Verbindungen zwischen virtuelle Netzwerke, hohen Verfügbarkeit und Verketten von Diensten werden in den nächsten Monaten dieses Dokument hinzugefügt werden.

## <a name="microsoft-compliance-and-infrastructure-protection"></a>Microsoft-Kompatibilität und Infrastruktur Schutz
Microsoft hat eine führende Position Einhaltungsinitiativen erforderliche Enterprise-Kunden unterstützende übernommen. Im folgenden werden einige der Compliance-Zertifizierung für Azure: ![Azure Compliance-Badges][1]

Weitere Informationen hierzu finden Sie im [Microsoft-Trust Center](https://azure.microsoft.com/support/trust-center/compliance/)Compliance-Informationen.

Microsoft hat einen umfassenden Ansatz zum Schützen Cloud-Infrastruktur zum Ausführen von Hyperscale globale Services erforderlich sind. Microsoft-Cloud-Infrastruktur umfasst Hardware, Software, Netzwerken und administrative und Mitarbeiter, sowie die physische Datencenter.

![Azure-Sicherheitsfeatures][2]

Dieser Ansatz bietet eine weitere sichere Grundlage für Kunden ihre Dienste in der Microsoft-Cloud bereitstellen. Im nächsten Schritt wird für Kunden entwerfen und erstellen eine Sicherheitsarchitektur zum Schützen dieser Dienste.

## <a name="traditional-security-architectures-and-perimeter-networks"></a>Traditionelle Sicherheitsmaßnahmen Architekturen und Umfang Netzwerken
Obwohl Microsoft stark investiert in die Cloud-Infrastruktur schützen, müssen Kunden auch ihre Cloud-Diensten und Ressourcengruppen schützen. Ein mehrschichtigen Ansatz zur Sicherheit stellt den besten Schutz vor. Zonen Umfang Netzwerk Loss internen Netzwerkressourcen aus einem nicht vertrauenswürdigen Netzwerk. Ein Shape-Netzwerk bezieht sich auf die Kanten oder Teile des Netzwerks, der zwischen dem Internet und der geschützten Enterprise IT-Infrastruktur befinden.

In normalen Enterprise Netzwerke wird die zentrale Infrastruktur stark am Perimeter, mit mehreren Ebenen Sicherheit Geräte untersuchende. Die Begrenzungslinie der einzelnen Ebenen besteht aus Geräte und Richtlinie Durchsetzung Punkte. Geräte konnte zählen folgende: Firewalls, verteilt Dienstausfall (DDoS) Prevention, einen unbefugten oder Schutz Systeme (IDS/IP-Adressen) und VPN-Geräte. Durchsetzung von Richtlinien kann die Form eines Firewall-Richtlinien, Access Steuerelement Zugriffssteuerungslisten oder bestimmte routing dauern. Die erste Zeile des Verteidigungslinien im Netzwerk, direkt akzeptiert eingehenden Datenverkehr aus dem Internet, ist eine Kombination der folgenden Verfahren zum Blockieren Angriffen und gefährlichen Datenverkehr während der zulässigen Anforderungen weiteren in das Netzwerk zulassen. Dieser Datenverkehr, die direkt an Ressourcen im Netzwerk Umfang weitergeleitet werden. Diese Ressource möglicherweise dann "auf Ressourcen im Netzwerk, die nächste Begrenzungslinie für Überprüfung Emissionswerte tieferen ersten sprechen". Die äußerste Ebene heißt im Netzwerk Umfang, da dieser Teil des Netzwerks mit dem Internet, in der Regel mit einigen Form des Schutzes beidseitiges verfügbar gemacht wird. Die folgende Abbildung zeigt ein Beispiel für ein einzelnes Subnetz Umfang Netzwerk in einem Unternehmensnetzwerk, mit zwei Sicherheit Begrenzung an.

![Ein Shape-Netzwerk in einem Unternehmensnetzwerk][3]

Es gibt viele Architekturen zum Implementieren eines Netzwerks Umfang aus einer einfachen Lastenausgleich vor einer Webfarm mit einem mehrere Subnetz Umfang Netzwerk mit unterschiedlichen Verfahren an jeder Grenze zu blockieren und schützen die tieferen Ebenen des Unternehmensnetzwerks verwendet. Erstellungsweise das Umfang Netzwerk hängt die spezifischen Bedürfnisse der Organisation und deren Fehlertoleranz für insgesamt Risiko ab.

Wie Kunden ihre Auslastung öffentliche Wolken verschieben möchten, ist es entscheidend, die ähnliche Funktionen für Umfang Netzwerkarchitektur in Azure zu Sicherheit und Einhaltung von Vorschriften erfüllen zu unterstützen. Dieses Dokument enthält Richtlinien auf wie Kunden eine sichere Umgebung in Azure erstellen können. Es liegt der Schwerpunkt auf den Rand herum Netzwerk, aber auch enthält eine umfassende Diskussion viele Aspekte der Netzwerk Sicherheit. Die folgenden Fragen zu dieser Diskussion informieren:

- Wie kann ein Netzwerk Umfang in Azure werden erstellt?
- Was sind einige der Azure Features zur Verfügung, um den Rand herum Netzwerk erstellen?
- Wie können die Back-End-Auslastung werden geschützt?
- Wie werden Internet-Kommunikation auf die Auslastung in Azure werden gesteuert?
- Wie können die lokale Netzwerke aus Bereitstellungen in Azure werden geschützt?
- Wann sollte systemeigenen Azure Sicherheitsfeatures im Vergleich zu Drittanbietern Einheiten oder Diensten werden verwendet?

Das folgende Diagramm veranschaulicht die verschiedenen Ebenen eines Wertpapiers zurück, die für Kunden Azure ermöglicht. Diese Ebenen werden sowohl in der Azure-Plattform selbst einheitlichen und Kunden definierten Features:

![Sicherheit von Azure-Architektur][4]

Eingehende aus dem Internet, Azure DDoS umfangreichen Angriffen auf Azure hilft gescannt. Die nächste Ebene ist benutzerdefiniert öffentlichen Endpunkte, die verwendet werden, um zu bestimmen, welche Datenverkehr an das virtuelle Netzwerk Cloud-Dienst passieren kann. Systemeigene Azure-virtuellen Netzwerkisolation vollständig isoliert von allen anderen Netzwerken wird sichergestellt, und den Datenverkehr nur über so konfiguriert, dass Benutzerpfade und Methoden fließt. Diese Netzwerkpfade und Methoden sind die nächste Ebene, die Stelle, an der NSGs, UDR und virtuelle Netzwerkgeräte zum Erstellen von Sicherheitsgrenzen zum Schutz der Anwendung Bereitstellungen in der geschützten Netzwerk verwendet werden können.

Im nächste Abschnitt bietet einen Überblick über Azure virtuelle Netzwerke. Diese virtuelle Netzwerke von Kunden erstellt werden, und es werden, was deren bereitgestellten Auslastung mit verbunden sind. Virtuelle Netzwerke sind die Grundlage für alle Netzwerksicherheitsfeatures zum Herstellen von einem Shape-Netzwerk zum Schützen von Kunden Bereitstellungen in Azure erforderlich.

## <a name="overview-of-azure-virtual-networks"></a>Übersicht über Azure virtuelle Netzwerke
Vor der Azure virtuelle Netzwerke Datenverkehr im Internet aufrufen kann, gibt es zwei Sicherheitsebenen gehörende zur Azure-Plattform:

1.  **DDoS Schutz**: DDoS Schutz ist eine Ebene des Azure physischen Netzwerks, die die Azure-Plattform selbst von umfangreichen internetbasierten Angriffen schützen. Solchen Angriffen verwenden mehrerer "Bot" Knoten bei einem Versuch, einen Internetdienst überlastet. Azure verfügt über eine robuste DDoS Schutz auf alle eingehenden Internet Connectivity Netz. Diese DDoS Schutz Ebene weist keine Benutzer konfigurierbare Attribute und kein Kunden zur Verfügung stehen. Dies Loss Azure als eine Plattform von umfangreichen Angriffen, sondern einzelne Kunden Anwendung direkt nicht geschützt werden. Es werden zusätzliche Stabilität können vom Kunden gegen einen lokalisierten konfiguriert werden. Wenn Sie mit einer umfangreichen DDoS-Angriffen an einem öffentlichen Endpunkt ausgesetzt Kunde A blockiert Azure beispielsweise Verbindungen mit diesem Dienst. Kunde A konnte nicht über in ein anderes virtuelle Netzwerk, oder service-Endpunkts der Angriffen Dienst Wiederherstellen nicht die. Bedenken Sie, dass zwar Kunde A auf diesen Endpunkt betroffen sein könnten, keine anderen Dienste außerhalb dieser Endpunkt betroffen wären. Darüber hinaus würde andere Kunden und Dienste gar nicht vor diesem Angriffen finden Sie unter.
2.  **Endpunkte**: Endpunkte ermöglichen Cloud Services oder Ressource Gruppen öffentlichen Internet IP-Adressen und Ports verfügbar gemacht haben. Der Endpunkt wird (Netzwerkadressübersetzung) für Datenverkehr an die interne Adresse und den Port Azure virtuelle Netzwerk verwenden. Dies ist der primäre Pfad für externe Datenverkehr in das virtuelle Netzwerk weitergeleitet werden. Die Endpunkte sind Benutzer konfiguriert, um festzustellen, welche übergeben wird, und wie und wo er wird konvertiert, um auf das virtuelle Netzwerk.

Nachdem erreicht Verkehr auf das virtuelle Netzwerk, gibt es viele Features, die in der Wiedergabe kommen. Azure virtuelle Netzwerke sind die Grundlage für Kunden anfügen, deren Auslastung und grundlegende Netzwerk Sicherheitsstufe, angewendet wird. Es ist ein privates Netzwerk (eine virtuelle Netzwerk überlagert) in Azure für Kunden mit den folgenden Funktionen und Merkmale:
 
- **Datenverkehr Isolation**: ein virtuelles Netzwerk ist, die Datenverkehr Isolation Begrenzungslinie auf der Azure-Plattform. Virtuellen Computern (virtuelle Computer) in einem Netzwerk auf virtuellen kann nicht direkt auf virtuellen Computern in einem anderen virtuellen Netzwerk kommunizieren, auch wenn beide virtuelle Netzwerke mit demselben Kunden erstellt werden. Dies ist eine wichtige Eigenschaft, die Kunden virtuellen Computern sichergestellt und Kommunikation innerhalb eines virtuellen Netzwerks privat bleibt.
- **Mehrstufigen Suchtopologie**: virtuelle Netzwerke ermöglichen es Kunden, mehrstufigen Suchtopologie zu definieren, indem Sie Subnetze zuordnen und separate Adresse Leerzeichen für andere Elemente oder deren Auslastung "Ebenen" festlegen. Diese logischen Gruppen und Topologien Kunden zum Definieren von verschiedenen Zugriffsrichtlinie auf Grundlage der Arbeitsbelastung Typen aktivieren, und auch den Datenverkehr Zahlungen zwischen Ebenen steuern.
- **Cross lokale Konnektivität**: Kunden können übergreifend lokale Verbindung zwischen einem virtuellen Netzwerk und mehrere lokale Websites oder in anderen Netzwerken virtuellen in Azure herstellen. Hierzu können Kunden Azure VPN-Gateways oder Drittanbieter-Netzwerkgeräte virtuelle verwenden. Azure unterstützt Standort-zu-Standort (S2S) VPN standard IPSec-/IKE Protokolle und private Connectivity ExpressRoute.
- **NSG** ermöglicht Kunden zum Erstellen von Regeln (ACLs) auf die gewünschte Ebene der Genauigkeit: Netzwerk-Schnittstellen, einzelne virtuellen Computern oder virtuelle Subnetze. Kunden können Zugriff zulassen oder Verweigern der Kommunikation zwischen der Auslastung innerhalb eines virtuellen Netzwerks, von Systemen auf Kunden Netzwerken über Cross lokale Konnektivität, steuern oder direkte Internet-Kommunikation.
- **UDR** und **IP-Weiterleitung** können Kunden die Kommunikationspfade zwischen verschiedenen Ebenen in einem virtuellen Netzwerk definieren. Kunden können Sie eine Firewall, IDS/IP-Adressen und andere virtuellen Einheiten bereitstellen und Netzwerkverkehr durch diese Wertpapiers Einheiten für Sicherheit Begrenzungslinie Durchsetzung, Überwachung und Prüfung weiterleiten.
- **Virtuelle Netzwerkgeräte** in der Azure Marketplace: Wertpapiers Einheiten wie Firewalls, Lastenausgleich und IDS/IP-Adressen stehen in den Azure Marketplace und der Bildergalerie virtueller Computer. Kunden können diese Einheiten in ihre virtuelle Netzwerke und insbesondere bei deren Sicherheit Begrenzung (einschließlich der Umfang Netzwerksubnets) bereitstellen, um eine sichere Umgebung mehrstufige abzuschließen.

Mit diesen Features und Funktionen ist ein Beispiel dafür, wie eine Shape Netzwerkarchitektur in Azure erstellt werden konnte Folgendes:

![Ein Shape-Netzwerk in einem Azure-virtuellen Netzwerk][5]

## <a name="perimeter-network-characteristics-and-requirements"></a>Umfang Netzwerk Merkmale und Anforderungen
Das Shape-Netzwerk soll der front-End des Netzwerks, direkt Kommunikation aus dem Internet verbunden sein. Eingehende Pakete sollten über die Sicherheit Einheiten, wie die Firewall, IDS und IP-Adressen, Datenfluss, vor dem Zugriff auf die Back-End-Server. Internet gerichtete Pakete von der Auslastung können auch über die Sicherheit Einheiten im Netzwerk Umfang für Durchsetzung, Prüfung und ü Zwecke, Datenfluss, bevor Sie das Netzwerk verlassen. Darüber hinaus kann im Netzwerk Umfang Cross lokale VPN-Gateways zwischen Kunden virtuelle Netzwerke und lokale Netzwerke hosten.

### <a name="perimeter-network-characteristics"></a>Umfang Netzwerk Merkmale
Zum Verweisen auf der vorherigen Abbildung sind einige der Merkmale von einem guten Umfang Netzwerk wie folgt:

- Internet zugänglichen:
    - Das Shape-Netzwerk-Subnetz selbst ist internetfähigen, direkt mit dem Internet zu kommunizieren.
    - Öffentliche IP-Adressen, VIPs und/oder Endpunkte übertragen Internetdatenverkehr können mit dem Front-End-Netzwerk und Geräte.
    - Aus dem Internet eingehender Datenverkehr durchläuft Sicherheitsgeräte, bevor Sie andere Ressourcen in der Front-End-Netzwerk.
    - Wenn ausgehender Sicherheit aktiviert ist, durchläuft Datenverkehr Sicherheitsgeräten als der letzte Schritt, bevor Sie mit dem Internet übergeben.
- Geschützten Netzwerk:
    - Es ist kein direkter Pfad aus dem Internet, die zentrale Infrastruktur.
    - Die zentrale Infrastruktur-Kanäle durchlaufen müssen über Sicherheitsgeräte wie NSGs, Firewalls oder VPN-Geräten.
    - Andere Geräte müssen nicht Internet- und die zentrale Infrastruktur zu schließen.
    - Sicherheitsgeräte auf das Internet zugänglichen und geschützten Netzwerk gegenüberliegende Begrenzung des Netzwerks Umfang (beispielsweise die beiden Firewall Symbole in der vorherigen Abbildung gezeigt) möglicherweise eine einzelne virtuelle Einheit mit gestaffelter Regeln oder Schnittstellen für jede Begrenzungslinie tatsächlich. (D. h., ein Gerät logisch getrennt, Behandeln von Last für beide Grenzen des Netzwerks Umfang.)
- Andere allgemeine Methoden und Einschränkungen:
    - Auslastung darf keine wichtige Geschäftsinformationen speichern.
    - Zugreifen auf und Updates zu Umkreisnetzwerkkonfigurationen und Bereitstellungen sind auf nur autorisierte Administratoren beschränkt.

### <a name="perimeter-network-requirements"></a>Shape-Netzwerk-Anforderungen
Führen Sie zum Aktivieren dieser Merkmale diesen Richtlinien virtuelles Netzwerk Anforderungen zum Implementieren eines erfolgreichen Umfang Netzwerks:

- **Subnetz Architektur:** Geben Sie das virtuelle Netzwerk so, dass ein ganzes Subnetz wie das Shape-Netzwerk, getrennt von anderen Subnetzen im gleichen virtuellen Netzwerk vorgesehen ist. Dadurch wird sichergestellt, den Datenverkehr zwischen dem Shape-Netzwerk und anderen internen oder privaten Subnetz Ebenen Zahlungen über einen Firewall oder virtuelle Einheit mit IDS/IP-Adressen auf die Begrenzung Subnetz mit User defined weitergeleitet.
- **NSG:** Das Shape-Netzwerk-Subnetz selbst sollten für die Kommunikation mit dem Internet offen, aber nicht bedeutet, dass Kunden NSGs umgangen werden soll. Führen Sie die allgemeine Sicherheitsmethoden, um die Netzwerk Flächen im Internet offen gelegt zu minimieren. Sperren Sie den Zugriff auf die Bereitstellungen oder die bestimmte Anwendungsprotokolle und geöffneten Ports zulässig remote Adressbereiche. Umstände können bestehen, jedoch in denen dies nicht immer möglich ist. Beispielsweise wenn Kunden Azure eine externe Website haben, im Netzwerk Umfang sollte die eingehenden Webanfragen aus öffentlichen IP-Adressen zulassen, aber öffnen nur die Web-Anwendungsports: TCP: 80 und TCP:443.
- **Routingtabelle:** Das Shape-Netzwerk-Subnetz selbst sollten direkt mit dem Internet kommunizieren, aber direkten Kommunikation an und von den wieder Ende oder lokalen Netzwerken nicht über eine Firewall oder Sicherheit Einheit sollten nicht zulassen.
- **Sicherheit Anwendungskonfiguration:** Um weiterleiten, und prüfen Pakete zwischen dem Shape-Netzwerk und den Rest der geschützten Netzwerke, die Sicherheit Einheiten wie Firewall, IDS und IP-Adressen Geräte mehrfach vernetzten möglicherweise. Sie müssen möglicherweise separaten NICs für das Shape-Netzwerk und die Back-End-Subnetze. Die NICs im Netzwerk Umfang kommunizieren direkt an und aus dem Internet, mit den entsprechenden NSGs und den Rand herum Netzwerk-routing-Tabelle. Der Herstellen einer Verbindung mit den Back-End-Subnetzen NICs haben mehrere NSGs und die entsprechenden Back-End-Subnetze routing-Tabellen eingeschränkt.
- **Sicherheit Einheit Funktionalität:** Die Sicherheit Einheiten, die in der Regel in den Rand herum Netzwerk bereitgestellt führen Sie die folgende Funktionen:
    - Firewall: Erzwingen Firewall-Regeln oder Steuerelement-Richtlinien für eingehenden Anfragen an.
    - Verfahren zum Erstellen von Erkennung und Prevention: erkennen und beheben Angriffen aus dem Internet.
    - Überwachen und protokollieren: Verwalten von detaillierte Protokolle für die Überwachung und Analyse.
    - Reverse Proxy: Umleiten eingehenden Anfragen zu den entsprechenden Back-End-Servern. Dies umfasst die Zuordnung und übersetzen auf Front-End-Geräten, die Zieladressen in der Regel firewalls, um die Back-End-Server-Adressen.
    - Weiterleiten von Proxy: Bereitstellen von NAT und Ausführen der Überwachung für die Kommunikation mit dem Internet von innerhalb des virtuellen Netzwerks initiiert.
    - Router: Weiterleitung eingehende und Cross-Subnetz innerhalb des virtuellen Netzwerks.
    - VPN-Gerät: als die Cross lokale VPN-Gateways für Cross lokale VPN-Konnektivität zwischen Kunden lokalen Netzwerken und Azure-virtuellen Netzwerken fungiert.
    - VPN-Server: Herstellen einer Verbindung mit Azure virtuelle Netzwerke VPN-Clients akzeptiert.

>[AZURE.TIP] Trennen Sie die folgenden zwei Gruppen: die Personen berechtigt, um den Rand herum Netzwerk Sicherheit Zahnrad sowie die Personen, die als Administratoren für die Entwicklung, Bereitstellung oder Vorgänge autorisiert zuzugreifen. Diesen Gruppen separaten planmäßigen für eine Trennung der Aufgaben ermöglicht und verhindert, dass eine einzige Person sowohl Applikationen Sicherheit und Netzwerk Sicherheit Steuerelemente nicht umgehen können.

### <a name="questions-to-be-asked-when-building-network-boundaries"></a>Fragen, die beim Erstellen von Netzwerkgrenzen aufgefordert
Sofern nicht ausdrücklich angegeben ist, verweist der Begriff "Netzwerke" in diesem Abschnitt auf private Azure virtuelle Netzwerke, die von einem Abonnementadministrator erstellt. Der Begriff bezieht sich nicht auf der zugrunde liegenden physischen Netzwerke in Azure.

Darüber hinaus werden Azure virtuelle Netzwerke häufig verwendet, um herkömmliche lokalen Netzwerken zu erweitern. Es ist möglich, entweder zwischen Standorten oder ExpressRoute Hybrid Netzwerke Lösungen mit Umfang Netzwerkarchitekturen einbinden. Dies ist ein wichtiger Aspekt beim Netzwerk Sicherheitsgrenzen zu erstellen.

Die folgenden drei Fragen sind entscheidend, die beantwortet werden, wenn Sie ein Netzwerk mit einem Shape-Netzwerk und mehrere Sicherheit Begrenzung erstellen.

#### <a name="1-how-many-boundaries-are-needed"></a>1) wie viele Begrenzung erforderlich sind?
Der erste Entscheidungspunkt befindet sich entscheiden, wie viele Sicherheit Begrenzung in einem bestimmten Szenario erforderlich sind:

- Eine einzelne Begrenzungslinie: eine Netzwerk Front-End-Shape zwischen dem virtuellen Netzwerk und im Internet.
- Zwei Grenzen: eine auf der Seite Internet im Netzwerk Rand herum und ein anderes zwischen dem Shape-Netzwerk-Subnetz und die Back-End-Subnetze in der Azure-virtuellen Netzwerken.
- Drei Grenzen: auf der Seite Internet des Netzwerks Umfang, eine zwischen den Rand herum Netzwerk und Back-End-Subnetze und eine zwischen die Back-End-Subnetze und dem lokalen Netzwerk.
- N Begrenzung: Variable einer Zahl zurück. Je nach Bedarf Sicherheit ist es wirklich eine beliebige Anzahl von Sicherheit, die in einem Netzwerk angewendet werden können.

Die Anzahl und Typ der Begrenzung erforderlich eines Unternehmens Risiko Fehlertoleranz und dem jeweiligen Szenario implementiert wird je wird. Dies ist oft eine gemeinsame Entscheidung, indem Sie mehrere Gruppen in einer Organisation, die häufig einschließlich ein Risiko und Compliance-Team, einem Netzwerk und Platform-Team und eine Anwendung Entwicklungsteam vorgenommen. Personen mit der Sicherheit, die zur Verwaltung der Daten und der verwendeten Technologien wissen sollten ein Wort in dieser Entscheidung, um sicherzustellen, dass die entsprechenden Security Abwehr für jede Implementierung haben.

>[AZURE.TIP] Verwenden Sie den kleinsten Wert der Begrenzung, die die Sicherheit Anforderungen für eine bestimmte Situation erfüllen. Mit der schwieriger Weitere Begrenzung Vorgänge und Problembehandlung können werden, sowie den Verwaltungsaufwand verbindet, bei der Verwaltung von mehreren Begrenzungslinie Richtlinien über einen Zeitraum. Nicht genügend Begrenzung erhöhen jedoch Risiko. Suchen den Saldo ist entscheidend.

![Hybrid Netzwerk mit drei Begrenzung der Sicherheit][6]

Die vorstehende Abbildung zeigt eine Ansicht auf hoher Ebene eines Netzwerks Begrenzungslinie drei Sicherheit. Die Begrenzung sind zwischen den Rand herum Netzwerk und im Internet, die Azure Front-End- und Back-End private Subnetze, und im Azure Back-End-Subnetz und dem lokalen Firmennetzwerk.

#### <a name="2-where-are-the-boundaries-located"></a>2), wo befinden sich die Begrenzung?
Sobald die Anzahl der Begrenzung entschieden werden, ist, wo sie Implementieren der nächste Entscheidungspunkt an. Im Allgemeinen stehen drei Optionen zur Verfügung:
- Mithilfe einer internetbasierten temporären Service (beispielsweise eine cloudbasierte Web Anwendung Firewall, der nicht in diesem Dokument erläutert wird)
- Verwenden von systemeigenen Features und/oder virtuelle Netzwerkgeräte in Azure
- Physische Geräte verwenden auf dem lokalen Netzwerk

In rein Azure Netzwerken stehen die Optionen systemeigene Azure-Funktionen (z. B. Azure Lastenausgleich) oder virtuelle Netzwerkgeräte aus der Rich-Partner-Netz von Azure (z. B. Check Point Firewalls).

Wenn eine Begrenzungslinie zwischen Azure und einem lokalen Netzwerk benötigt wird, können die Sicherheitsgeräte auf beiden Seiten des die Verbindung (oder beide Seiten) befinden. Daher muss eine Entscheidung auf die Stelle platziert Sicherheit Zahnrad vorgenommen werden.

In der Abbildung oben im Internet-zu-Shape-Netzwerk und die Vorderseite Back-End-Begrenzung vollständig in Azure enthalten sind, und müssen einen systemeigenen Azure Features oder Netzwerk virtuelle Einheiten. Sicherheitsgeräte auf die Begrenzungslinie zwischen Azure (Back-End-Subnetz) und dem Firmennetzwerk könnte entweder auf der Seite Azure oder der lokalen Seite oder auch eine Kombination von Geräten auf beiden Seiten. Es kann sein signifikante vor- und Nachteile auf eine der Optionen, die sehr ernst berücksichtigt werden müssen.

Angenommen, hat das vorhandene physische Sicherheit Zahnrad auf der lokalen Network-Seite mit den Vorteil, dass keine neuen Zahnrad erforderlich ist. Völlig neu konfiguriert. Die Nachteile, ist jedoch die gesamten Verkehr wieder aus Azure im muss Zusammenhang mit dem lokalen Netzwerk von der Sicherheit Zahnrad angezeigt werden. Daher konnte Azure-Azure-Datenverkehr signifikante Wartezeit anfallen, und die Leistung der Anwendung auswirken und Benutzer kommen, wenn sie wieder zu dem lokalen Netzwerk für die Durchsetzung von Richtlinien erzwungen wurde.

#### <a name="3-how-are-the-boundaries-implemented"></a>3) wie werden die Begrenzung implementiert?
Jede Sicherheit Begrenzungslinie müssen wahrscheinlich anderen Videofunktionen Anforderungen (z. B. IDS und Firewall-Regeln auf der Seite Internet den Rand herum Netzwerk, aber nur ACLs zwischen den Rand herum Netzwerk und Back-End-Subnetz). Die zu verwendenden Geräte Entscheidung, hängt von den Anforderungen Szenario und Sicherheit. Im folgenden Abschnitt diskutieren Beispiele 1, 2 und 3 einige Optionen, die verwendet werden können. Überprüfen der Azure systemeigenen Netzwerkfeatures und die Geräte in Azure das Partnerökosystem stellt zeigt die zahllosen Optionen zur Lösung von praktisch jedem Szenario verfügbar.

Ein weiterer ist Key Implementierung Entscheidung so mit Azure lokalen Netzwerk herzustellen. Sollten Sie das Azure-virtuellen Gateway oder eine virtuelle Netzwerk-Anwendung verwenden? Diese Optionen sind die folgenden Abschnitte enthalten (Beispiele 4, 5 und 6) ausführlicher beschrieben.

Darüber hinaus kann Datenverkehr zwischen virtuellen Netzwerken in Azure erforderlich sein. Diese Szenarios werden zu einem späteren Zeitpunkt hinzugefügt werden.

Nachdem Sie die Antworten auf die vorherige Fragen kennen, hilft Ihnen im Abschnitt [Fast Start](#fast-start) identifizieren, welche Beispiele für ein bestimmtes Szenario am besten geeignet sind.

## <a name="examples-building-security-boundaries-with-azure-virtual-networks"></a>Beispiele: Erstellen von Sicherheit Begrenzung mit Azure virtuelle Netzwerke
### <a name="example-1-build-a-perimeter-network-to-help-protect-applications-with-nsgs"></a>Beispiel 1: Erstellen einer Shape-Netzwerk zum Schutz von Applications mit NSGs
[Zurück zum Schnellstart](#fast-start) | [detailliert erstellen Anweisungen in diesem Beispiel][Example1]

![Shape-Netzwerk mit NSG eingehende][7]

#### <a name="environment-description"></a>Beschreibung der Umgebung
In diesem Beispiel ist ein Abonnement, die folgenden enthält:

- Zwei cloud Services: "FrontEnd001" und "BackEnd001"
- Ein virtuelles Netzwerk "CorpNetwork", mit unterschiedlichen zwei: "Front-End" und "Back-End"
- Eine Netzwerk-Sicherheitsgruppe, die auf beide Subnetze angewendet wird
- Einen Windows-Server, der einen Anwendung Webserver iis01 ("Neu")
- Zwei Windows-Servern, die Anwendung Back-End-Server ("AppVM01", "AppVM02") darstellen.
- Einen Windows-Server, der einen DNS-Server ("DNS01")

Skripts und einer Ressourcenmanager Azure-Vorlage finden Sie unter den [detaillierte Anweisungen][Example1].

#### <a name="nsg-description"></a>NSG Beschreibung
In diesem Beispiel wird eine Gruppe NSG integriert, und klicken Sie dann mit sechs Regeln geladen.

>[AZURE.TIP] Im Allgemeinen sollten Sie bestimmten "Erlauben" Regeln zuerst, gefolgt von der eher generischen "Verweigern" Regeln erstellen. Die angegebene Priorität schreibt vor, welche Regeln zuerst ausgewertet werden. Nachdem Sie den Datenverkehr auf einer bestimmten Regel anzuwendende gefunden wird, werden keine weiteren Regeln ausgewertet. NSG Regeln können in der eingehenden oder ausgehenden Richtung (hinsichtlich der im Subnetz) anwenden.

Deklarativ, sind die folgenden Regeln für eingehenden Datenverkehr erstellt wird:

1.  Internen DNS-Verkehr (Port 53) ist zulässig.
2.  RDP-Datenverkehr (Port 3389) aus dem Internet zu einem beliebigen virtuellen Computern zulässig ist.
3.  HTTP-Verkehr (Port 80) aus dem Internet auf Webserver (iis01 neu) ist zulässig.
4.  Alle Verkehr (alle Ports) von iis01 neu zu AppVM1 zulässig ist.
5.  Alle Datenverkehr (alle Ports) aus dem Internet auf das gesamte virtuelle Netzwerk (beide Subnetze) wird verweigert.
6.  Alle Datenverkehr (alle Ports) aus dem Front-End-Subnetz mit dem Back-End-Subnetz wird verweigert.

Mit dieser Regeln gebunden für jedes Subnetz, wenn eine HTTP-Anforderung aus dem Internet an den Webserver, beide Regeln 3 eingehenden wurde (zulassen) und 5 (ablehnen) anwenden möchten. Aber da Regel 3 eine höhere Priorität hat, sie möchten nur anzuwenden, und Regel 5 nicht ins Spiel kommen würde. Daher würde die HTTP-Anforderung an den Webserver gewährt werden. Wenn die gleiche Datenverkehr an den Server DNS01 zu erreichen versucht hat, Regel 5 (ablehnen) wäre das erste anwenden und der Datenverkehr zulässig wäre nicht auf dem Server zu übergeben. Regel 6 (ablehnen) blockiert das Front-End-Subnetz aus ein Gespräch mit dem Back-End-Subnetz (mit Ausnahme der zulässige Datenverkehr in Regeln 1 und 4). Dadurch werden die Back-End-Netzwerk geschützt, für den Fall, dass ein Angreifer am front-End der Anwendung gefährdet. Der Angreifer würde Zugriff auf das "geschützten" Back-End-Netzwerk (nur für die Ressourcen bereitgestellt, die auf dem Server AppVM01) zugänglich gemacht.

Es gibt eine standardmäßige ausgehende Regel, die Datenverkehr, mit dem Internet zulässt. In diesem Beispiel haben wir zuzulassen ausgehenden Datenverkehr und alle ausgehenden Regeln nicht ändern. Um den Datenverkehr in beiden Richtungen zu sperren, benutzerdefinierte routing ist erforderlich (siehe Beispiel 3).

#### <a name="conclusion"></a>Abschluss
Dies ist eine relativ einfache und direkte Möglichkeit, isolieren das Back-End-Subnetz aus eingehenden Datenverkehr. Weitere Informationen finden Sie unter den [detaillierte Anweisungen][Example1]. Diese Anweisungen umfassen:

- So erstellen Sie dieses Shape-Netzwerk mit PowerShell-Skripts.
- So erstellen Sie dieses Shape-Netzwerk mit einer Vorlage Azure Ressourcenmanager.
- Detaillierte Beschreibung der einzelnen NSG Befehle.
- Ausführliche Datenverkehr Fluss Szenarien, wie Datenverkehr zulässig oder verweigert in jeder Ebene anzeigt.


 ### <a name="example-2-build-a-perimeter-network-to-help-protect-applications-with-a-firewall-and-nsgs"></a>Beispiel 2: Erstellen einer Shape-Netzwerk zum Schutz von Applications mit einer Firewall und NSGs
[Zurück zum Schnellstart](#fast-start) | [detailliert erstellen Anweisungen in diesem Beispiel][Example2]

![Shape-Netzwerk mit NVA und NSG eingehende][8]

#### <a name="environment-description"></a>Beschreibung der Umgebung
In diesem Beispiel ist ein Abonnement, die folgenden enthält:

- Zwei cloud Services: "FrontEnd001" und "BackEnd001"
- Ein virtuelles Netzwerk "CorpNetwork", mit unterschiedlichen zwei: "Front-End" und "Back-End"
- Eine Netzwerk-Sicherheitsgruppe, die auf beide Subnetze angewendet wird
- Eine virtuelle Netzwerkeinheit in diesem Fall eine Firewall, mit dem Front-End-Subnetz verbunden
- Einen Windows-Server, der einen Anwendung Webserver iis01 ("Neu")
- Zwei Windows-Servern, die Anwendung Back-End-Server ("AppVM01", "AppVM02") darstellen.
- Einen Windows-Server, der einen DNS-Server ("DNS01")

Skripts und einer Ressourcenmanager Azure-Vorlage finden Sie unter den [detaillierte Anweisungen][Example2].

#### <a name="nsg-description"></a>NSG Beschreibung
In diesem Beispiel wird eine Gruppe NSG integriert, und klicken Sie dann mit sechs Regeln geladen.

>[AZURE.TIP] Im Allgemeinen sollten Sie bestimmten "Erlauben" Regeln zuerst, gefolgt von der eher generischen "Verweigern" Regeln erstellen. Die angegebene Priorität schreibt vor, welche Regeln zuerst ausgewertet werden. Nachdem Sie den Datenverkehr auf einer bestimmten Regel anzuwendende gefunden wird, werden keine weiteren Regeln ausgewertet. NSG Regeln können in der eingehenden oder ausgehenden Richtung (hinsichtlich der im Subnetz) anwenden.

Deklarativ, sind die folgenden Regeln für eingehenden Datenverkehr erstellt wird:

1.  Internen DNS-Verkehr (Port 53) ist zulässig.
2.  RDP-Datenverkehr (Port 3389) aus dem Internet zu einem beliebigen virtuellen Computern zulässig ist.
3.  Alle den Internetdatenverkehr (alle Ports) in die virtuelle Netzwerk-Anwendung (Firewall) ist zulässig.
4.  Alle Verkehr (alle Ports) von iis01 neu zu AppVM1 zulässig ist.
5.  Alle Datenverkehr (alle Ports) aus dem Internet auf das gesamte virtuelle Netzwerk (beide Subnetze) wird verweigert.
6.  Alle Datenverkehr (alle Ports) aus dem Front-End-Subnetz mit dem Back-End-Subnetz wird verweigert.

Mit dieser Regeln gebunden für jedes Subnetz, wenn eine HTTP-Anforderung aus dem Internet die Firewall, beide Regeln 3 gehandelt wurde (zulassen) und 5 (ablehnen) anwenden möchten. Aber da Regel 3 eine höhere Priorität hat, sie möchten nur anzuwenden, und Regel 5 nicht ins Spiel kommen würde. Daher würde die HTTP-Anforderung an die Firewall gewährt werden. Wenn die gleiche Datenverkehr versucht hat, den Server iis01 neu zu erreichen, obwohl sie die Front-End-Subnetz ist, Regel 5 (ablehnen) anwenden möchten, und der Datenverkehr zulässig wäre nicht auf dem Server zu übergeben. Regel 6 (ablehnen) blockiert das Front-End-Subnetz aus ein Gespräch mit dem Back-End-Subnetz (mit Ausnahme der zulässige Datenverkehr in Regeln 1 und 4). Dadurch werden die Back-End-Netzwerk geschützt, für den Fall, dass ein Angreifer am front-End der Anwendung gefährdet. Der Angreifer würde Zugriff auf das "geschützten" Back-End-Netzwerk (nur für die Ressourcen bereitgestellt, die auf dem Server AppVM01) zugänglich gemacht.

Es gibt eine standardmäßige ausgehende Regel, die Datenverkehr, mit dem Internet zulässt. In diesem Beispiel haben wir zuzulassen ausgehenden Datenverkehr und alle ausgehenden Regeln nicht ändern. Um den Datenverkehr in beiden Richtungen zu sperren, benutzerdefinierte routing ist erforderlich (siehe Beispiel 3).

#### <a name="firewall-rule-description"></a>Beschreibung der Firewall-Regel
Klicken Sie auf die Firewall sollte die Option weiterleiten Regeln erstellt werden. Regel ist erforderlich, da in diesem Beispiel nur Datenverkehr im Internet Grenze an die Firewall leitet, und klicken Sie dann auf den Webserver nur eine Weiterleitung Netzwerk-Adresse (Netzwerkadressübersetzung).

Die Weiterleitungsregel akzeptiert alle eingehenden Quellbilds, die die Firewall versucht, HTTP (Port 80 oder 443 für HTTPS) erreichen Treffer. Es hat aus lokalen Schnittstelle die Firewall gesendet und auf dem Webserver mit der IP-Adresse des 10.0.1.5 geleitet.

#### <a name="conclusion"></a>Abschluss
Dies ist eine relativ einfach Verfahren zum Schutz Ihrer Anwendungs durch eine Firewall und das Back-End-Subnetz aus eingehenden Datenverkehr isolieren. Weitere Informationen finden Sie unter den [detaillierte Anweisungen][Example2]. Diese Anweisungen umfassen:

- So erstellen Sie dieses Shape-Netzwerk mit PowerShell-Skripts.
- So erstellen Sie in diesem Beispiel mit einem Ressourcenmanager Azure-Vorlage.
- Detaillierte Beschreibung der einzelnen NSG Befehl und Firewall-Regeln.
- Ausführliche Datenverkehr Fluss Szenarien, wie Datenverkehr zulässig oder verweigert in jeder Ebene anzeigt.

### <a name="example-3-build-a-perimeter-network-to-help-protect-networks-with-a-firewall-udr-and-nsg"></a>Beispiel 3: Erstellen einer Shape-Netzwerk zum Schutz von Netzwerken durch eine Firewall, UDR und NSG
[Zurück zum Schnellstart](#fast-start) | [detailliert erstellen Anweisungen in diesem Beispiel][Example3]

![Bidirektionale Umfang Netzwerk mit NVA, NSG und UDR][9]

#### <a name="environment-description"></a>Beschreibung der Umgebung
In diesem Beispiel ist ein Abonnement, die folgenden enthält:

- Drei cloud Services: "SecSvc001", "FrontEnd001" und "BackEnd001"
- Ein virtuelles Netzwerk "CorpNetwork", mit unterschiedlichen drei: "SecNet", "Front-End" und "Back-End"
- Eine virtuelle Netzwerkeinheit in diesem Fall eine Firewall, mit dem SecNet Subnetz verbunden
- Einen Windows-Server, der einen Anwendung Webserver iis01 ("Neu")
- Zwei Windows-Servern, die Anwendung Back-End-Server ("AppVM01", "AppVM02") darstellen.
- Einen Windows-Server, der einen DNS-Server ("DNS01")

Skripts und einer Ressourcenmanager Azure-Vorlage finden Sie unter den [detaillierte Anweisungen][Example3].

#### <a name="udr-description"></a>UDR Beschreibung
Standardmäßig werden die folgenden System leitet wie folgt definiert:

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.0.0/16}     VNETLocal                            Active   Default    
         {0.0.0.0/0}       Internet                             Active   Default    
         {10.0.0.0/8}      Null                                 Active   Default    
         {100.64.0.0/10}   Null                                 Active   Default    
         {172.16.0.0/12}   Null                                 Active   Default    
         {192.168.0.0/16}  Null                                 Active   Default

Die VNETLocal ist immer die prefix(es) definierten Adresse des virtuellen Netzwerks für die bestimmte Netzwerk (d. h., es ändert sich von virtuellen Netzwerk virtuelle Netzwerk, je nachdem wie jedes bestimmtes virtuelles Netzwerk definiert ist). Die verbleibende System leitet sind statisch und Standard wie in der Tabelle angegeben.

In diesem Beispiel werden zwei routing-Tabellen erstellt, jeweils eine für die Front-End- und Back-End-Subnetze. Jede Tabelle wird mit statischen leitet für das angegebene Subnetz geeignet geladen. In diesem Beispiel wird jede Tabelle verfügt über drei weitergeleitet, die gesamten Verkehr (0.0.0.0/0) direkte durch die Firewall (des nächsten Abschnitts = virtuelle Einheit IP-Adresse):

1. Datenverkehr im lokalen Subnetz mit keine der nächsten Abschnitte definiert, um den Datenverkehr im lokalen Subnetz umgehen die Firewall zulassen.
2. Virtuelle Netzwerkdatenverkehr mit einer nächsten Abschnitte als Firewall definiert; Dadurch wird die Standard-Regel, die lokale virtuelle Netzwerkverkehr zum Weiterleiten von direkt ermöglicht überschrieben.
3. Alle übrigen Datenverkehr (0/0) mit einer nächsten Abschnitte als Firewall definiert.

>[AZURE.TIP] In der UDR den Eintrag lokale Subnetz ohne wird die lokale Subnetz Kommunikation unterbrochen. 
> - In diesem Beispiel 10.0.1.0/24 auf VNETLocal ist entscheidend, als Paket andernfalls, zu verlassen, die dem Webserver (10.0.1.4) zu einem anderen lokalen Server (zum Beispiel) 10.0.1.25 bestimmt, wie sie über an die NVA gesendet werden, die mit dem Subnetz schicken, fehl, und im Subnetz wird erneut senden Sie diese an den NVA usw..
> - Chancen Weiterleitung endlos wiedergegeben wird, sind in der Regel höhere auf mehrere verschiedene Netzwerkkarten Einheiten, die direkt für jedes Subnetz, mit der Sie kommunizieren, also häufig traditionell, lokal, Einheiten verbunden sind. 

Nachdem Sie die Weiterleitung Tabellen erstellt werden, sind sie zu ihrer Subnetze gebunden. Die Front-End-Subnetz routing Tabelle einmal erstellt und mit dem Subnetz, gebunden würde wie folgt aussehen:

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.1.0/24}     VNETLocal                            Active 
         {10.0.0.0/16}     VirtualAppliance 10.0.0.4            Active    
         {0.0.0.0/0}       VirtualAppliance 10.0.0.4            Active

>[AZURE.NOTE] UDR kann jetzt mit dem Gateway Subnetz angewendet werden, auf dem die Verbindung ExpressRoute verbunden ist.
>
> Beispiele zum Aktivieren des Netzwerks Umfang mit ExpressRoute oder Website-zu-Standort-Netzwerke werden in den Beispielen 3 und 4 angezeigt.


#### <a name="ip-forwarding-description"></a>Beschreibung IP-Weiterleitung
IP-Weiterleitung ist eine Companion-Funktion zu UDR. Dies ist eine Einstellung für eine virtuelle Anwendung, die nicht speziell an die Einheit adressierte Datenverkehr empfangen, und leiten dann den ultimate-Zielort der Datenverkehr ermöglicht.

Angenommen, wenn Datenverkehr von AppVM01 auf dem Server DNS01 anfordert, würde UDR dies an die Firewall weiterleiten. Mit IP-Weiterleitung aktiviert der Datenverkehr für das Ziel DNS01 (10.0.2.4) akzeptiert wird durch das Gerät (10.0.0.4) und dann den ultimate (10.0.2.4)-Zielort weitergeleitet. Ohne IP-Weiterleitung für die Firewall aktiviert würde Datenverkehr nicht von der Anwendung akzeptiert werden, obwohl die Routingtabelle die Firewall als den nächsten Abschnitt hat. Um eine virtuelle Anwendung verwenden zu können, ist es entscheidend, denken Sie daran, die IP-Weiterleitung in Verbindung mit UDR aktivieren.

#### <a name="nsg-description"></a>NSG Beschreibung
In diesem Beispiel wird eine Gruppe NSG erstellt, und klicken Sie dann mit einer einzelnen Regel geladen. Diese Gruppe gebunden ist dann nur für die Front-End- und Back-End-Subnetze (nicht die SecNet). Die folgende Regel wird deklarativ erstellt:

- Alle Datenverkehr (alle Ports) aus dem Internet auf das gesamte virtuelle Netzwerk (alle Subnetze) wird verweigert.

Obwohl NSGs in diesem Beispiel verwendet werden, ist ihr Hauptfenster Zweck als sekundäre Schutz vor falsche manuelle Konfiguration. Das Ziel ist mit der Front-End- oder Back-End-Teilnetzen alle eingehenden Datenverkehr aus dem Internet blockieren. Datenverkehr sollte nur über das Subnetz SecNet an die Firewall Datenfluss (und dann, sofern zutreffend, bei der Front-End- oder Back-End-Subnetze). Plus würde mit den UDR Regeln an Ort, Datenverkehr, die in der Front-End- oder Back-End-Subnetze aufgenommen werden konnten, an der Firewall (Dank UDR) weitergeleitet werden. Die Firewall sieht dies als eine asymmetrische Fluss und legen Sie den ausgehenden Datenverkehr würden. Es gibt also drei Sicherheitsebenen Schutz der Subnetze:
- Klicken Sie auf die FrontEnd001 und BackEnd001 keine geöffneten Endpunkte cloud Services.
- NSGs Datenverkehr aus dem Internet verweigern.
- Der Firewall asymmetrische Datenverkehr ablegen.

Eine interessanten Punkts hinsichtlich der NSG in diesem Beispiel ist, dass sie nur eine Regel die besteht enthält darin, auf das gesamte virtuelle Netzwerk, einschließlich des Sicherheit Subnetzes Datenverkehr im Internet verweigern. Jedoch, da die NSG nur an die Front-End- und Back-End-Subnetze gebunden ist, die Regel ist nicht verarbeitet auf den Datenverkehr an das Subnetz Sicherheit eingehenden. Daher fließt Datenverkehr an das Subnetz Sicherheit.

#### <a name="firewall-rules"></a>Firewall-Regeln
Klicken Sie auf die Firewall sollte die Option weiterleiten Regeln erstellt werden. Da die Firewall blockieren oder Weiterleitung alle eingehenden, ausgehenden und übermitteln virtuelle Netzwerkverkehr ist, sind viele Firewall-Regeln erforderlich. Darüber hinaus werden alle eingehender Datenverkehr Security Service öffentliche IP-Adresse (auf andere Ports), drücken Sie durch die Firewall verarbeitet werden soll. Es wird empfohlen, die logischen Zahlungen vor dem Einrichten der Subnetze in einem Diagramm und später überarbeiten Sie Firewall-Regeln, um zu vermeiden. Die folgende Abbildung zeigt eine logische Ansicht der Firewall-Regeln in diesem Beispiel:
 
![Logische Ansicht der Firewall-Regeln][10]

>[AZURE.NOTE] Basierend auf das Netzwerk virtuelle Einheit verwendet, werden die Management-Ports variieren. In diesem Beispiel wird eine Barracuda NextGen Firewall verwiesen die Ports 22, 801 und 807 verwendet. Finden Sie in der Dokumentation des Herstellers Einheit genügt die genauen Ports für die Verwaltung von dem verwendeten Gerät verwendet.

#### <a name="firewall-rules-description"></a>Beschreibung der Firewall-Regeln
In der vorherigen logisches Diagramm wird das Sicherheit Subnetz nicht angezeigt. Dies ist, da die Firewall der einzige Ressource in diesem Subnetz ist und Firewall-Regeln, und wie diese logisch zulassen oder verweigern Datenverkehr Zahlungen, die nicht den tatsächlichen weitergeleitete Pfad dieses Diagramm angezeigt wird. Darüber hinaus die externen Ports für der Datenverkehr RDP sind höher aktiviert einem Bereich Ports (8014 – 8026) und etwas mit den letzten beiden Oktetts der lokale IP-Adresse Lesbarkeit ausrichten ausgewählt wurden (beispielsweise lokale Serveradresse 10.0.1.4 ist externen Anschluss 8014 zugeordnet). Eine höhere-Konflikte verursachende Ports, könnten jedoch verwendet werden.

In diesem Beispiel benötigen wir sieben verschiedene Arten von Regeln:

- Externe Regeln (für eingehenden Datenverkehr):
  1.    Regel für Firewall-Verwaltung: Diese App umleiten Regel lässt Datenverkehr an die Management-Ports der Einheit virtuelle Netzwerk übergeben.
  2.    RDP Regeln (bei jeder Windows-Server): Diese vier Regeln (eine für jede Server) Verwaltung der einzelnen Server über RDP zulassen. Dies kann auch in einer Regel, je nach den Funktionen der verwendeten Netzwerk virtuelle Einheit zusammengefasst.
  3.    Anwendung Datenverkehr Regeln: Es gibt zwei dieser Regeln, die erste für die Front-End-Web-Verkehr und die Sekunde für den Back-End-Datenverkehr (z. B. Webserver mit Datenebene). Die Konfiguration dieser Regeln hängt von der Netzwerkarchitektur (der Ihre Server platziert sind) und Datenverkehr Zahlungen (welcher Richtung die Datenverkehr Zahlungen und welche ports verwendet werden).
      - Die erste Regel ermöglicht den eigentlichen Anwendungsdatenverkehr Anwendungsserver erreicht haben. Während die anderen Regeln für die Verwaltung von Sicherheits- und zulassen, sind Anwendung Datenverkehr Regeln an, was externe Benutzer oder Dienste die Anwendung(en) zugreifen können. In diesem Beispiel gibt es ein einzelnen Webserver auf Port 80. Daher leitet die Regel für eine einzelne Firewall-Anwendung eingehenden Datenverkehr an die externe IP-Adresse der Web-Servern internen IP-Adresse. Die Sitzung umgeleitet Datenverkehr würde an den internen Server über NAT übersetzt werden.
      - Die zweite Regel ist die Back-End-Regel an, damit den Webserver an den Server AppVM01 (aber nicht AppVM02) sprechen über einen beliebigen Anschluss.
- Interner Regeln (zum Übermitteln virtuelle Netzwerkverkehr)
  4.    Ausgehende Internet Regel: mit dieser Regel lässt Datenverkehr von jedes Netzwerk an den ausgewählten Netzwerken übergeben. Diese Regel ist in der Regel eine Standardregel bereits auf die Firewall, aber deaktiviert. In diesem Beispiel sollte diese Regel aktiviert sein.
  5.    DNS-Regel: mit dieser Regel kann nur DNS-(Port 53) Datenverkehr an den DNS-Server zu übergeben. Für diese Umgebung ist die meisten Datenverkehr vom front-End an die Back-End blockiert. Diese Regel ermöglicht insbesondere DNS aus einem beliebigen lokalen Subnetz an.
  6.    Subnetz Subnetz Regel: mit dieser Regel wird an, damit alle Server im Subnetz Back-End-Verbindung zu einem beliebigen Server auf dem Front-End-Subnetz (aber nicht umgekehrt).
- Fail Regel (für den Datenverkehr, die eine des vorhergehenden entsprechen nicht):
  7.    Regel für alle Datenverkehr verweigern: Dies sollten immer die endgültige Regel (in Bezug auf die Priorität) und als solche Wenn ein Datenfluss nicht überein der vorangehenden Regeln werden Sie von dieser Regel gelöscht werden. Dies ist eine Standardregel und in der Regel aktiviert. Keine Änderungen werden im Allgemeinen erforderlich.

>[AZURE.TIP] Klicken Sie auf die zweite Anwendung Datenverkehr Regel ist zur Vereinfachung dieses Beispiels, einen beliebigen Anschluss zulässig. In einer realen Szenario sollte den genauesten Port und Adressbereiche verwendet werden, um die Angriffen mit dieser Regel verringern.

Nachdem alle vorherigen Regeln erstellt werden, ist es wichtig, um zu überprüfen, die Priorität der einzelnen Regeln, um sicherzustellen, dass Datenverkehr zulässig oder verweigert wie gewünscht wird. In diesem Beispiel werden die Regeln Priorität aus.

#### <a name="conclusion"></a>Abschluss
Dies ist eine Art komplexere aber genauere Schutz und isolieren im Netzwerk als den vorherigen Beispielen. (Beispiel 2 werden nur die Anwendung geschützt und Beispiel 1 isoliert nur Subnetze). Dieser Entwurf ermöglicht Datenverkehr in beiden Richtungen, für die Überwachung und nicht nur den eingehende Anwendungsserver Loss jedoch setzt Netzwerksicherheitsrichtlinie für alle Server in diesem Netzwerk. Darüber hinaus können abhängig von der Einheit verwendet, vollständige Datenverkehr Überwachung und Präsenz erzielt werden. Weitere Informationen finden Sie unter den [detaillierte Anweisungen][Example3]. Diese Anweisungen umfassen:

- So erstellen Sie dieses Beispiel Umfang Netzwerk mit PowerShell-Skripts.
- So erstellen Sie in diesem Beispiel mit einem Ressourcenmanager Azure-Vorlage.
- Detaillierte Beschreibung der einzelnen UDR, NSG Befehl und Firewall-Regel.
- Ausführliche Datenverkehr Fluss Szenarien, wie Datenverkehr zulässig oder verweigert in jeder Ebene anzeigt.

### <a name="example-4-add-a-hybrid-connection-with-a-site-to-site-virtual-appliance-virtual-private-network-vpn"></a>Beispiel 4: Hinzufügen einer Hybrid-Verbindung mit einer Website-zu-Standort, virtuelle Einheit virtuelles privates Netzwerk (VPN)
[Zurück zum Schnellstart](#fast-start) | Generator-Anweisungen zur Verfügung bald detaillierte

![Shape-Netzwerk mit dem Netzwerk verbundenen Hybrid NVA][11]

#### <a name="environment-description"></a>Beschreibung der Umgebung
Der Umfang Netzwerktypen beschrieben, die in den Beispielen 1, 2 oder 3 kann Hybrid Netzwerke mithilfe einer virtuelle Netzwerk-Anwendung (NVA) hinzugefügt werden.

Wie in der vorherigen Abbildung zu sehen ist, wird eine VPN-Verbindung über das Internet (Standort-zu-Standort) in Verbindung mit einer Azure virtuelle Netzwerk über eine NVA einer lokalen Netzwerk verwendet.

>[AZURE.NOTE] Wenn Sie bei aktivierter Option Azure öffentlichen Peering ExpressRoute verwenden, sollte eine statische Routing erstellt werden. Dies sollte an die NVA VPN-IP-Adresse, Ihre corporate Internet und nicht über die ExpressRoute WAN-weiterleiten. Erforderlich, klicken Sie auf die Option ExpressRoute Azure öffentlichen Peering NAT kann die VPN-Sitzung getrennt werden.

Wenn das Option VPN eingerichtet ist, wird die NVA zentralen Hub für alle Netzwerke und Subnetze an. Die Firewall Weiterleitungsregeln fest, welche Datenverkehr fließt dürfen, über NAT übersetzt werden, umgeleitet werden oder sind (auch für den Datenverkehr Zahlungen zwischen dem lokalen Netzwerk und Azure) gelöscht wird.

Datenverkehr fließt sollte sorgfältig, gilt als optimiert werden kann, oder nach diesem Entwurfsmuster heruntergestuft abhängig von dem betreffenden Fall verwenden.

Mithilfe der integrierten Beispiel 3-Umgebung und dann eine Website-zu-Standort VPN-Hybrid Netzwerkverbindung erzeugt das folgende Design auf:

![Shape-Netzwerk mit NVA verbunden sind, über ein VPN zwischen Standorten][12]

Die lokale Router oder alle anderen Netzwerkgeräte, die mit Ihrem NVA für VPN, kompatibel ist wäre der VPN-Client. Diese physische Gerät wäre verantwortlich initiieren und verwalten die Option VPN Verbindung mit Ihrem NVA.

Logisch sieht in der NVA im Netzwerk vier Separate "Sicherheitszonen" mit der Regeln auf das NVA der primären Leiter der Verkehr zwischen diesen Zonen wird:

![Logisches Netzwerk aus NVA Sicht][13]

#### <a name="conclusion"></a>Abschluss
Das Hinzufügen einer Website-zu-Standort VPN-Hybrid Netzwerk Verbindung mit einer Azure virtuellen Netzwerk kann das lokalen Netzwerk in Azure auf sichere Weise erweitern. Verwendung einer VPN-Verbindungs, Ihren Datenverkehr ist verschlüsselt und leitet über das Internet. Die NVA in diesem Beispiel bietet einen zentralen Speicherort erzwingen und Verwalten der Sicherheitsrichtlinie. Weitere Informationen finden Sie unter den detaillierte Anweisungen (in Kürze). Diese Anweisungen umfassen:

- So erstellen Sie dieses Beispiel Umfang Netzwerk mit PowerShell-Skripts.
- So erstellen Sie in diesem Beispiel mit einem Ressourcenmanager Azure-Vorlage.
- Ausführliche Datenverkehr Fluss Szenarien mit dieser Entwurf wie Datenverkehr durchläuft.

### <a name="example-5-add-a-hybrid-connection-with-a-site-to-site-azure-gateway-vpn"></a>Beispiel 5: Hinzufügen einer Hybrid-Verbindung mit einem Gateway-Standorten, Azure VPN
[Zurück zum Schnellstart](#fast-start) | Generator-Anweisungen zur Verfügung bald detaillierte

![Shape-Netzwerk mit dem Netzwerk verbundenen Hybrid gateway][14]

#### <a name="environment-description"></a>Beschreibung der Umgebung
Hybrid Netzwerke über einen Azure VPN Gateway kann entweder Umfang Netzwerktyp beschrieben, die in den Beispielen 1 oder 2 hinzugefügt werden.

(Siehe vorstehende Abbildung) wird eine VPN-Verbindung über das Internet (Standort-zu-Standort) zu einer lokalen Netzwerk Verbindung mit einer Azure virtuelle Netzwerk über ein VPN Azure Gateway verwendet.

>[AZURE.NOTE] Wenn Sie bei aktivierter Option Azure öffentlichen Peering ExpressRoute verwenden, sollte eine statische Routing erstellt werden. Dies sollte an die NVA VPN-IP-Adresse, Ihre corporate Internet und nicht über die ExpressRoute WAN-weiterleiten. Erforderlich, klicken Sie auf die Option ExpressRoute Azure öffentlichen Peering NAT kann die VPN-Sitzung getrennt werden.

Die folgende Abbildung zeigt die beiden Netzwerk Kanten in diese Option. Klicken Sie auf der ersten Kante Steuern der NVA und NSGs Datenverkehr fließt übermitteln Azure Netzwerke und zwischen Azure und im Internet. Die zweite Kante ist das Gateway Azure VPN, also eine Kante vollständig separat und isoliert Netzwerk zwischen lokalen und Azure.

Datenverkehr fließt sollte sorgfältig, gilt als optimiert werden kann, oder nach diesem Entwurfsmuster heruntergestuft abhängig von dem betreffenden Fall verwenden.

Verwendung der Umgebung in Beispiel 1 erstellt und dann eine Standort-zu-Standort VPN-Hybrid Netzwerkverbindung erzeugt das folgende Design auf:

![Shape-Netzwerk mit Gateway verbunden, eine Verbindung mit ExpressRoute verwenden][15]

#### <a name="conclusion"></a>Abschluss
Das Hinzufügen einer Website-zu-Standort VPN-Hybrid Netzwerk Verbindung mit einer Azure virtuellen Netzwerk kann das lokalen Netzwerk in Azure auf sichere Weise erweitern. Mithilfe des systemeigenen Azure VPN Gateways, Ihren Verkehr ist IPSec verschlüsselt und über das Internet weiterleitet. Außerdem kann mithilfe des Gateways Azure VPN eine niedrigere Kosten Option (keine zusätzliche Lizenzierung wie bei Drittanbieter-NVAs Kosten) bereitstellen. Dies ist besonders preisgünstige in Beispiel 1, wenn keine NVA verwendet wird. Weitere Informationen finden Sie unter den detaillierte Anweisungen (in Kürze). Diese Anweisungen umfassen:

- So erstellen Sie dieses Beispiel Umfang Netzwerk mit PowerShell-Skripts.
- So erstellen Sie in diesem Beispiel mit einem Ressourcenmanager Azure-Vorlage.
- Ausführliche Datenverkehr Fluss Szenarien mit dieser Entwurf wie Datenverkehr durchläuft.

### <a name="example-6-add-a-hybrid-connection-with-expressroute"></a>Beispiel 6: Hinzufügen einer Hybrid-Verbindung mit ExpressRoute
[Zurück zum Schnellstart](#fast-start) | Generator-Anweisungen zur Verfügung bald detaillierte

![Shape-Netzwerk mit dem Netzwerk verbundenen Hybrid gateway][16]

#### <a name="environment-description"></a>Beschreibung der Umgebung
Hybrid Netzwerke eine Verbindung mit ExpressRoute private Peeringliste verwenden kann entweder Umfang Netzwerktyp beschrieben, die in den Beispielen 1 oder 2 hinzugefügt werden.

(Siehe vorstehende Abbildung) bietet ExpressRoute private peering eine direkte Verbindung zwischen Ihrem lokalen und Azure virtuelle Netzwerk. Datenverkehr ihrer Durchquerung nur die Service Provider und Microsoft Azure Netzwerk, nie berühren im Internet.

>[AZURE.NOTE] Es gibt bestimmte Einschränkungen bei Verwendung von UDR mit ExpressRoute, die Komplexität des dynamischen routing in das Azure-virtuellen Gateway verwendet werden. Dies sind:
>
>- UDR sollten nicht mit dem Gateway Subnetz angewendet werden, auf dem ExpressRoute verknüpfte Azure virtuelle Gateways verbunden ist.
>- ExpressRoute verknüpfte Azure virtuelle Gateways nicht möglich, dass das Gerät NextHop für andere UDR Subnetze gebunden.
>
>
<br />

>[AZURE.TIP] Mithilfe von ExpressRoute behält corporate Netzwerkverkehr aus dem Internet zur Erhöhung der Sicherheit und eine erheblich verbesserte Leistung. Darüber hinaus kann für den Dienst Servicelevel von Ihrem Anbieter ExpressRoute. Das Gateway Azure können bis zu 2 Gb/s mit ExpressRoute, übergeben, wohingegen mit Standort-zu-Standort VPN, der maximale Durchsatz Azure Gateway 200 Mb/s ist.

Wie in der folgenden Abbildung zu sehen, mit dieser Option die Umgebung verfügt jetzt über zwei Netzwerk Kanten. Die NVA und NSG steuern Datenverkehr fließt übermitteln Azure Netzwerke und zwischen Azure und im Internet, während das Gateway eine Kante vollständig separat und isoliert Netzwerk zwischen lokalen und Azure ist.

Datenverkehr fließt sollte sorgfältig, gilt als optimiert werden kann, oder nach diesem Entwurfsmuster heruntergestuft abhängig von dem betreffenden Fall verwenden.

Verwendung der Umgebung in Beispiel 1 erstellt und dann eine ExpressRoute Hybrid Verbindung, erzeugt das folgende Design auf:

![Shape-Netzwerk mit Gateway verbunden, eine Verbindung mit ExpressRoute verwenden][17]

#### <a name="conclusion"></a>Abschluss
Das Hinzufügen von eine Verbindung ExpressRoute privat Peering kann das lokalen Netzwerk in Azure in eine sichere, unteren Wartezeit, höher Durchführung Weise erweitern. Mithilfe der systemeigenen Azure Gateways, wie in diesem Beispiel stellt außerdem eine niedrigere Kosten Option (keine zusätzlichen wie bei NVAs Drittanbieter-Lizenzierung). Weitere Informationen finden Sie unter den detaillierte Anweisungen (in Kürze). Diese Anweisungen umfassen:

- So erstellen Sie dieses Beispiel Umfang Netzwerk mit PowerShell-Skripts.
- So erstellen Sie in diesem Beispiel mit einem Ressourcenmanager Azure-Vorlage.
- Ausführliche Datenverkehr Fluss Szenarien mit dieser Entwurf wie Datenverkehr durchläuft.

## <a name="references"></a>Verweise
### <a name="helpful-websites-and-documentation"></a>Nützliche Websites und Dokumentation
- Access Azure mit Azure Ressourcenmanager:
- Zugreifen auf Azure mit PowerShell: [https://azure.microsoft.com/documentation/articles/powershell-install-configure/](./powershell-install-configure.md)
- Virtuelle Netzwerke Dokumentation: [https://azure.microsoft.com/documentation/services/virtual-network/](https://azure.microsoft.com/documentation/services/virtual-network/)
- Netzwerk-Dokumentation zu Sicherheit: [https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/](./virtual-network/virtual-networks-nsg.md)
- Benutzerdefinierte routing Dokumentation: [https://azure.microsoft.com/documentation/articles/virtual-networks-udr-overview/](./virtual-network/virtual-networks-udr-overview.md)
- Azure-virtuellen Gateways: [https://azure.microsoft.com/documentation/services/vpn-gateway/](https://azure.microsoft.com/documentation/services/vpn-gateway/)
- Website-zu-Standort VPN: [https://azure.microsoft.com/documentation/articles/vpn-gateway-create-site-to-site-rm-powershell](./vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)
- ExpressRoute Dokumentation (Achten Sie darauf, die in den Abschnitten "Erste Schritte" und "How To" auszuchecken): [https://azure.microsoft.com/documentation/services/expressroute/](https://azure.microsoft.com/documentation/services/expressroute/)

<!--Image References-->
[0]: ./media/best-practices-network-security/flowchart.png "Flussdiagramm für die Sicherheit Optionen"
[1]: ./media/best-practices-network-security/compliancebadges.png "Azure Compliance-Badges"
[2]: ./media/best-practices-network-security/azuresecurityfeatures.png "Azure-Sicherheitsfeatures"
[3]: ./media/best-practices-network-security/dmzcorporate.png "Eine DMZ in einem Unternehmensnetzwerk"
[4]: ./media/best-practices-network-security/azuresecurityarchitecture.png "Sicherheit von Azure-Architektur"
[5]: ./media/best-practices-network-security/dmzazure.png "Eine DMZ in ein Azure-virtuellen Netzwerk"
[6]: ./media/best-practices-network-security/dmzhybrid.png "Hybrid Netzwerk mit drei Begrenzung der Sicherheit"
[7]: ./media/best-practices-network-security/example1design.png "Eingehende DMZ mit NSG"
[8]: ./media/best-practices-network-security/example2design.png "Eingehende DMZ mit NVA und NSG"
[9]: ./media/best-practices-network-security/example3design.png "Bidirektionale DMZ mit NVA, NSG und UDR"
[10]: ./media/best-practices-network-security/example3firewalllogical.png "Logische Ansicht der Firewall-Regeln"
[11]: ./media/best-practices-network-security/example4designoptions.png "DMZ mit NVA Hybrid-Netzwerk verbunden."
[12]: ./media/best-practices-network-security/example4designs2s.png "DMZ mit NVA über ein VPN zwischen Standorten verbunden"
[13]: ./media/best-practices-network-security/example4networklogical.png "Logisches Netzwerk aus NVA Sicht"
[14]: ./media/best-practices-network-security/example5designoptions.png "DMZ mit Azure Gateway verbunden Standorten Hybrid Netzwerk."
[15]: ./media/best-practices-network-security/example5designs2s.png "DMZ mit Azure mithilfe von Website-zu-Standort VPN Gateway"
[16]: ./media/best-practices-network-security/example6designoptions.png "DMZ mit Azure Gateway verbunden ExpressRoute Hybrid-Netzwerk."
[17]: ./media/best-practices-network-security/example6designexpressroute.png "DMZ mit Azure Gateway eine Verbindung mit ExpressRoute verwenden"

<!--Link References-->
[Example1]: ./virtual-network/virtual-networks-dmz-nsg-asm.md
[Example2]: ./virtual-network/virtual-networks-dmz-nsg-fw-asm.md
[Example3]: ./virtual-network/virtual-networks-dmz-nsg-fw-udr-asm.md
[Example4]: ./virtual-network/virtual-networks-hybrid-s2s-nva-asm.md
[Example5]: ./virtual-network/virtual-networks-hybrid-s2s-agw-asm.md
[Example6]: ./virtual-network/virtual-networks-hybrid-expressroute-asm.md
[Example7]: ./virtual-network/virtual-networks-vnet2vnet-direct-asm.md
[Example8]: ./virtual-network/virtual-networks-vnet2vnet-transit-asm.md

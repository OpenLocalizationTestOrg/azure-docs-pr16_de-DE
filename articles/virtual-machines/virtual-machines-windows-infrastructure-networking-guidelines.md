<properties
    pageTitle="Networking Infrastructure Richtlinien | Microsoft Azure"
    description="Lernen Sie die wichtigsten Planung und Implementierung Richtlinien für die Bereitstellung von virtuellen Netzwerke in Azure-Infrastrukturdiensten aus."
    documentationCenter=""
    services="virtual-machines-windows"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="networking-infrastructure-guidelines"></a>Netzwerke Infrastruktur Richtlinien

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)] 

In diesem Artikel liegt der Schwerpunkt auf die Planung erforderlichen Schritte für virtuelle Netzwerke in Azure und Konnektivität zwischen vorhandenen auf Prem Umgebungen Grundlegendes.


## <a name="implementation-guidelines-for-virtual-networks"></a>Von Implementierungsrichtlinien für virtuelle Netzwerke

Entscheidungen:

- Welche Art von virtuellen Netzwerk müssen Sie Ihre IT-Arbeitsbelastung oder Infrastruktur hosten (nur Cloud- oder übergreifend lokale)?
- Für Cross lokale virtuelle Netzwerke wie viel Speicherplatz Adresse müssen Sie die Subnetze und virtuellen Computern jetzt und in der Zukunft angemessenen erläuterten hosten möchte?
- Möchten Sie die zentrale virtuelle Netzwerke erstellen oder einzelne virtuelle Netzwerke für jede Ressourcengruppe erstellen?

Aufgaben:

- Definieren Sie den Abstand Adresse für das virtuelle Netzwerke erstellt werden.
- Definieren von Adresssubnetzen und den Abstand Adresse für jeden.
- Für Cross lokale virtuelle Netzwerke definieren von lokalen Netzwerk Adresse Leerzeichen für die lokale Orte, die die virtuellen Computern in das virtuelle Netzwerk erreicht haben müssen.
- Arbeiten mit lokalen networking-Team zu der entsprechenden Weiterleitung zu gewährleisten beim Erstellen Cross virtuelle Netzwerke lokale konfiguriert ist.
- Erstellen Sie das virtuelle Netzwerk Ihrer Benennungskonvention verwenden.


## <a name="virtual-networks"></a>Virtuelle Netzwerke

Virtuelle Netzwerke sind zur Unterstützung der Kommunikation zwischen virtuellen Computern (virtuellen Computern) erforderlich sind. Sie können definieren Subnetze, benutzerdefinierte IP-Adresse, DNS-Einstellungen, Sicherheit, Filtern und Lastenausgleich, wie bei physischen Netzwerken. Über ein [VPN-Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md) oder [Express-Routing Verbindung](../expressroute/expressroute-introduction.md), können Sie mit Ihrem lokalen Netzwerken Azure-virtuelle Netzwerken verbinden. Weitere Informationen über [virtuelle Netzwerke und deren Komponenten](../virtual-network/virtual-networks-overview.md).

Mithilfe von Ressourcengruppen, müssen Sie die Flexibilität in, wie Sie Ihre virtuellen Netzwerken Komponenten entwerfen. Virtuelle Netzwerke außerhalb ihrer eigenen Ressourcengruppe können virtuellen Computern verbinden. Ein allgemeine Ansatz wäre zentrale Ressourcengruppen erstellen, die Ihre Infrastruktur des Netzwerks Core enthalten, die mit einem gemeinsamen Team, und dann auf virtuellen Computern und deren-Anwendung bereitgestellt separaten Ressource Gruppen verwaltet werden können. Dieser Ansatz ermöglicht die Anwendung Besitzer Zugriff auf die Ressourcengruppe, die ihre virtuellen Computer ohne Zugriff auf die Konfiguration der breiter virtuellen Netzwerken Ressourcen Öffnung enthält.

## <a name="site-connectivity"></a>Website-Konnektivität

### <a name="cloud-only-virtual-networks"></a>Cloud nur virtuelle Netzwerke
Lokale Benutzer und Computern laufenden Verbindung zu virtuellen Computern in einem Azure-virtuellen Netzwerk nicht benötigen, ist Ihre virtuelle Netzwerkentwurf direkt weiterleiten:

![Einfaches Cloud nur virtuelle Netzwerkdiagramm](./media/virtual-machines-common-infrastructure-service-guidelines/vnet01.png)

Dieser Ansatz empfiehlt sich in der Regel für Internet zugänglichen Auslastung, wie etwa einer internetbasierten Webserver. Sie können diese virtuelle Computer mit RDP oder Punkt-zu-Standort VPN-Verbindungen verwalten.

Da nicht mit Ihrem lokalen Netzwerk verbunden sind, können nur Azure-virtuellen Netzwerke verwenden ein Teils des privaten IP-Adressbereichs, verwenden lokale, auch bei der gleiche private Speicherplatz in.


### <a name="cross-premises-virtual-networks"></a>Cross lokale virtuelle Netzwerke
Wenn lokale Benutzer und Computern laufenden Verbindung zu virtuellen Computern in einem Azure-virtuellen Netzwerk benötigen, erstellen Sie ein Kreuz lokale virtuelle Netzwerk aus.  Verbinden Sie es mit Ihrem lokalen Netzwerk mit einer ExpressRoute oder Website-zu-Standort VPN-Verbindung aus.

![Cross lokale virtuelle Netzplandiagramm](./media/virtual-machines-common-infrastructure-service-guidelines/vnet02.png)

In dieser Konfiguration ist das Azure virtuelle Netzwerk im Wesentlichen eine cloudbasierte Erweiterung von Ihrem lokalen Netzwerk.

Da sie eine Verbindung mit Ihrem lokalen Netzwerk herstellen, Cross lokale virtuelle Netzwerke verwenden müssen, einen Teil des Adressbereichs verwendet, die von Ihrer Organisation, der eindeutig ist. Auf die gleiche Weise, die unterschiedliche corporate Standorten spezielles IP-Subnetz zugeordnet sind, wird Azure einen anderen Speicherort aus, wie Sie Sie Ihr Netzwerk erweitern.

Pakete mit Ihrem lokalen Netzwerk aus Ihrem Cross lokale virtuelle Netzwerk Reisen lässt, müssen Sie den Satz von relevanten lokalen Adresspräfixe als Teil der lokalen Netzwerk Definition für das virtuelle Netzwerk konfigurieren. Je nach den Abstand der Adresse des virtuellen Netzwerks und Festlegen der entsprechenden lokalen Speicherorte können viele Adresspräfixe in dem lokalen Netzwerk vorhanden sein.

Sie können ein virtuelles Netzwerk Cloud nur mit einem Cross lokale virtuelle Netzwerk konvertieren, höchstwahrscheinlich erfordert jedoch um Abstände IP-des Adressbereichs virtuelles Netzwerk und Azure Ressourcen. Überlegen Sie daher, wenn ein virtuelles Netzwerk muss mit Ihrem lokalen Netzwerk verbunden werden, wenn Sie über ein IP-Subnetz zuweisen.

## <a name="subnets"></a>Subnetze
Subnetze ermöglichen es Ihnen, Ressourcen zu organisieren, die verwandt sind, entweder logisch (z. B. ein Subnetz für virtuellen Computern, die mit der gleichen Anwendung verknüpft ist) oder physisch (beispielsweise ein Subnetz pro Ressourcengruppe). Sie können auch Subnetz Isolation Techniken zur Erhöhung der Sicherheit einsetzen.

Für Cross lokale virtuelle Netzwerke sollten Sie Subnetze mit den gleichen Konventionen entwerfen, die Sie für lokale Ressourcen verwenden. **Immer Azure verwendet die ersten drei IP-Adressen des Adressbereichs für jedes Subnetz**. Ermitteln Sie die Anzahl der Adressen für das Subnetz erforderlich, starten Sie durch zählen der Anzahl von virtuellen Computern, die Sie jetzt benötigen. Für zukünftiges Wachstum schätzen Sie, und verwenden Sie dann in der folgenden Tabelle, um die Größe der im Subnetz zu bestimmen.

Anzahl von virtuellen Computern erforderlich | Anzahl der Hostbits erforderlich | Größe des im Subnetz
--- | --- | ---
1 bis 3 | 3 | / 29
4 – 11     | 4 | / 28
12 – 27 | 5 | / 27
28 – 59 | 6 | / 26
60 – 123 | 7 | / 25

> [AZURE.NOTE] Für lokale normalen Subnetze ist die maximale Anzahl von Hostadressen für ein Subnetz mit n Hostbits 2<sup>n</sup> – 2. Für eine Azure Subnetz ist die maximale Anzahl von Hostadressen für ein Subnetz mit n Hostbits 2<sup>n</sup> – 5 (2 und 3 für die Adressen, die in jedem Subnetz Azure verwendet).

Wenn Sie eine Größe Subnetz, die zu klein ist auswählen, Sie müssen IP-Abstände und erneut bereitstellen, die virtuellen Computern im Subnetz.


## <a name="network-security-groups"></a>Netzwerk-Sicherheitsgruppen
Sie können zum Filtern von Regeln auf den Datenverkehr, der fließt über Ihre virtuelle Netzwerke anwenden, mithilfe von Netzwerk-Sicherheitsgruppen. Steuern der eingehenden und ausgehenden Datenverkehr, Quell- und Zielfelder IP-Bereiche, zulässige Ports usw. können Sie präzise Filterung Regeln zum Sichern des virtuellen Umgebung Netzwerken, erstellen. Netzwerk-Sicherheitsgruppen können Subnetze in einem Netzwerk virtuelle oder direkt zu einer bestimmten virtuellen Netzwerkschnittstelle angewendet werden. Es wird empfohlen, zu einem gewissen Grad Netzwerk-Sicherheitsgruppe Filtern Verkehr auf Ihre virtuelle Netzwerke haben. Weitere Informationen zum [Netzwerk-Sicherheitsgruppen](../virtual-network/virtual-networks-nsg.md).


## <a name="additional-network-components"></a>Zusätzliche Netzwerkkomponenten
Wie bei lokalen physische Netzwerke Infrastruktur kann Azure virtuelle Netzwerke mehr als Subnetze und IP-Adressen enthalten. Wie Sie Ihrer Anwendungsinfrastruktur entwerfen, sollten Sie einige dieser zusätzlichen Komponenten einbinden:

- [VPN-Gateways](../vpn-gateway/vpn-gateway-about-vpngateways.md) - Azure virtuelle Netzwerke mit anderen Azure virtuellen Netzwerken verbinden, oder Verbinden mit lokalen Netzwerke über eine Website-zu-Standort VPN-Verbindung. Express-Routing-Verbindungen für dedizierte, sichere Verbindungen zu implementieren. Sie können auch mit Punkt-zu-Standort VPN-Verbindungen Direktzugriff Benutzer bereitstellen.
- [Lastenausgleich](../load-balancer/load-balancer-overview.md) - bietet Lastenausgleich des Datenverkehrs für externe und interne Datenverkehr wie gewünscht.
- [Application Gateway](../application-gateway/application-gateway-introduction.md) - HTTP Lastenausgleich auf der Anwendungsebene, einige weitere Vorteile für Webanwendungen bereitstellen statt der Bereitstellung von der Azure Lastenausgleich.
- [Datenverkehr Manager](../traffic-manager/traffic-manager-overview.md) - DNS-basierte Datenverkehr Häufigkeiten Endbenutzer an den nächsten verfügbaren Anwendungsendpunkt direkte überwacht zum Hosten der Anwendung von Azure Rechenzentren in unterschiedlichen Regionen.


## <a name="next-steps"></a>Nächste Schritte

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)] 
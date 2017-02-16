<properties
   pageTitle="Übersicht über die Sicherheit von Azure Netzwerk | Microsoft Azure"
   description=" In diesem Artikel erleichtert zu verstehen, was Microsoft Azure zu im Bereich der Netzwerk Sicherheit zu bieten hat. Wir Lehrplan in grundlegende Core Netzwerk Sicherheitskonzepte und Anforderungen und Informationen auf was Azure zu in jedem der folgenden Bereiche zu bieten hat. "
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="MBaldwin"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/09/2016"
   ms.author="terrylan"/>

# <a name="azure-network-security-overview"></a>Übersicht über die Sicherheit von Azure Netzwerk

Microsoft Azure umfasst eine robuste Netzwerke Infrastruktur Ihrer Anwendung und Dienst Connectivity Anforderungen unterstützt. Netzwerkkonnektivität zwischen Ressourcen in Azure zwischen lokalen möglich ist und Azure gehostet Ressourcen, und aus den Internet- und Azure.

Das Ziel dieses Artikels ist, damit es einfacher für Sie zu verstehen, was Microsoft Azure zu im Bereich der Netzwerk Sicherheit zu bieten hat. Hier stellen wir grundlegende erläuterungen für Core Netzwerk Sicherheitskonzepte und Anforderungen an. Außerdem bieten wir Sie Informationen auf was Azure zu in jedem der folgenden Bereiche zu bieten hat. Es gibt zahlreiche Links zu anderen Inhalten, mit denen Sie ein besseres Verständnis für den Bereichen abrufen, in dem Sie interessiert sind.

Dieser Artikel Übersicht über die Sicherheit von Azure-Netzwerk liegt der Schwerpunkt auf Folgendes:

- Azure Netzwerke
- Access-Netzwerk-Steuerelements
- Secure remote Access und Cross lokale Konnektivität
- Verfügbarkeit
- Protokollierung
- Mit einer namensauflösung von
- DMZ Architektur
- Azure-Sicherheitscenter

## <a name="azure-networking"></a>Azure Netzwerke

Virtuellen Computern benötigen Netzwerkkonnektivität. Um die Anforderung zu unterstützen, erfordert Azure virtuellen Computern mit einer Azure-virtuellen Netzwerk verbunden sein. Ein Azure-virtuellen Netzwerk ist eine logische erstellen, die auf der Struktur physischen Azure Netzwerk aufgebaut. Jedes logische virtuelle Azure-Netzwerk wird von allen anderen virtuellen Azure-Netzwerken isoliert. Dadurch wird sichergestellt, dass in Ihrem Bereitstellungen Netzwerkverkehr nicht mit anderen Kunden Microsoft Azure zugegriffen werden kann.

Weitere Informationen:

- [Virtuelle Network (Übersicht)](../virtual-network/virtual-networks-overview.md)

## <a name="network-access-control"></a>Network Access Control
Access-Netzwerk-Steuerelements ist die Act Connectivity an und von bestimmter Geräte oder Subnetzen innerhalb einer Azure-virtuellen Netzwerk zu beschränken. Das Ziel von Network Access Control ist, um sicherzustellen, dass von virtuellen Computern und Dienstleistungen nur für Benutzer und Geräte stehen, die diese zugegriffen werden soll. Access-Steuerelemente basieren auf zulassen oder verweigern Entscheidungen für Verbindungen an und von Ihrem virtuellen Computern oder einen bestimmten Dienst.

Azure unterstützt verschiedene Typen von Network Access Control. Hierzu gehören:

- Netzwerk Layer-Steuerelement
- Steuerelement weiterleiten und Erzwungene Tunnel
- Virtuelle Sicherheit Netzwerkgeräte

### <a name="network-layer-control"></a>Network Layer Control
Alle sicheren Bereitstellung erfordert einige Measure von Network Access Control. Das Ziel von Network Access Control ist, um sicherzustellen, dass Sie Ihren virtuellen Computern und der Netzwerkdienste, die auf diesen virtuellen Computern ausgeführt werden, die zur Kommunikation mit benötigten und alle anderen Verbindungsversuche blockiert werden nur in Verbindung mit anderen Netzwerkgeräte kommunizieren können.

Wenn Sie Standard-Netzwerk Zugriff auf Ebene Steuerelement (basierend auf IP-Adresse und die Protokolle TCP oder UDP) benötigen, können Sie Sicherheitsgruppen Netzwerk verwenden. Ein Netzwerk Sicherheit Gruppe (NSG) ist eine einfache dynamische Paket Firewall Filterung, und es ermöglicht Ihnen das Steuern des Zugriffs basierend auf ein [5-Tupel](https://www.techopedia.com/definition/28190/5-tuple). NSGs bieten keine Überprüfung auf Ebenen oder authentifiziert Access-Steuerelemente.

Weitere Informationen:

- [Netzwerk-Sicherheitsgruppen](../virtual-network/virtual-networks-nsg.md)

### <a name="route-control-and-forced-tunneling"></a>Routing Steuerelement und Erzwungene Tunnel
Die Möglichkeit, die Weiterleitung Verhalten auf Ihre Azure-virtuellen Netzwerke steuern ist eine Funktion kritische Netzwerk Sicherheit und Access-Steuerelement. Wenn routing nicht richtig konfiguriert ist, können Anwendungen und Dienste auf Ihre virtuellen Computern herstellen einer Verbindung mit Geräten, die Sie eine Verbindung hergestellt werden sollen nicht einschließlich Geräte gehört und wird betrieben von potenziellen Angreifern.

Azure Netzwerke unterstützt die Möglichkeit, das Weiterleitung Verhalten für Netzwerkdatenverkehr auf Ihre Azure-virtuellen Netzwerke anzupassen. Dies können Sie die standardmäßigen routing Tabelleneinträge in Ihrem Azure-virtuellen Netzwerk ändern. Steuern des routing Verhalten können, die Sie sicherstellen, dass alle Datenverkehr von einem bestimmten Gerät oder eine Gruppe von Geräten wechselt oder bewirkt, Ihr Azure-virtuellen Netzwerk über einen bestimmten Standort dass.

Beispielsweise müssen Sie eine virtuelle Netzwerk Sicherheit Einheit in Ihrem Azure-virtuellen Netzwerk ein. Sie möchten sicherstellen, dass diese virtuelle Sicherheit Einheit gesamten Verkehr an und von Ihrem Azure-virtuellen Netzwerk durchläuft. Dies ist möglich, indem Sie [Benutzer definiert leitet](../virtual-network/virtual-networks-udr-overview.md) in Azure konfigurieren.

[Erzwungene Tunnel](https://www.petri.com/azure-forced-tunneling) ist eine Methode, die Sie verwenden können, um sicherzustellen, dass Ihre Dienste einleiten eine Verbindung mit Geräte im Internet nicht zulässig sind. Beachten Sie, dass dies von akzeptieren von eingehenden Verbindungen, und klicken Sie dann auf diese reagieren unterscheidet. Front-End-Webservern Anforderung aus dem Internethosts beantworten müssen, und daher Internet-Quelle zugelassen auf diese Webserver eingehende und die Webserver reagiert zulässig sind.

Was Sie nicht zulassen möchten, ist ein Front-End-Webserver eine ausgehende Anfrage einleiten. Solche Anfragen möglicherweise ein Sicherheitsrisiko darstellen, da diese Verbindungen zum Herunterladen von Schadsoftware verwendet werden können. Auch wenn Sie diese Front-End-Server mit dem Internet ausgehende Anfragen einleiten möchten, sollten Sie erzwingen, dass sie über Ihre lokalen Webproxys wechseln, damit Sie URL-Filterung und Protokollierung nutzen können.

Möchten Sie stattdessen erzwungenen Tunnel verwenden, um dies zu verhindern. Wenn Sie die erzwungene Tunnel aktivieren, werden alle Verbindungen mit dem Internet über Ihrem lokalen Gateway erzwungen. Sie können konfigurieren, Erzwungene Tunnel durch die Nutzung der Benutzer definiert ist.

Weitere Informationen:

- [Was sind Benutzer definiert ist und die IP-Weiterleitung](../virtual-network/virtual-networks-udr-overview.md)

### <a name="virtual-network-security-appliances"></a>Virtuelle Sicherheit Netzwerkgeräte
Während Netzwerk Sicherheitsgruppen, Benutzer definiert leitet und Erzwungene Tunnel Sie eine Sicherheitsstufe bei der Netzwerk- und Transport Ebenen des [OSI-Modells](https://en.wikipedia.org/wiki/OSI_model)zur Verfügung stellen, werden möglicherweise vorkommen, dass Sie die Sicherheit auf Ebenen höher als im Netzwerk aktivieren möchten.

Angenommen, enthalten möglicherweise Ihren Anforderungen Sicherheit:

- Authentifizierung und Autorisierung vor dem Gewähren des Zugriffs auf eine Anwendung
- Einen unbefugten und Intrusion Antwort
- Anwendung Layer sind für übergeordneten Protokolle
- URL-Filterung
- Netzwerk Ebene Viren und Malware
- Anti-Bot Schutz
- Access-Steuerelement Anwendung
- Zusätzliche DDoS Schutz (oberhalb der DDoS Schutz der Azure-Struktur selbst bereitgestellt)

Sie können diese verbesserte Sicherheit Netzwerkfeatures mithilfe einer Lösung Azure Partner zugreifen. Sie können die aktuellsten Azure Partner Network Sicherheit Lösungen zu finden, besuchen die [Azure Marketplace](https://azure.microsoft.com/marketplace/) und Suchen nach "Sicherheit" und "Network Security".

## <a name="secure-remote-access-and-cross-premises-connectivity"></a>Sicheren Remotezugriff und Cross lokale Konnektivität
Installation, Konfiguration und Verwaltung von Ihrer Azure Ressourcen Remote durchgeführt werden muss. Sie möchten darüber hinaus [Hybrid IT](http://social.technet.microsoft.com/wiki/contents/articles/18120.hybrid-cloud-infrastructure-design-considerations.aspx) -Lösungen bereitstellen, die Komponenten lokal aufweisen und in der Azure öffentlichen Cloud. Diese Szenarios erfordern sicheren remote-Zugriff.

Azure Netzwerke unterstützt die folgenden sicheren Remotezugriffsszenarien:

- Herstellen einer Verbindung eine Azure-virtuellen Netzwerk mit einzelnen Arbeitsstationen
- Herstellen einer Verbindung ein Azure-virtuellen Netzwerk mit einem VPN mit Ihrem lokalen Netzwerk
- Herstellen einer Verbindung eine virtuelle Azure-Netzwerk mit eine dedizierte WAN-Verbindung mit Ihrem lokalen Netzwerk
- Herstellen einer Verbindung untereinander mit Azure virtuelle Netzwerke

### <a name="connect-individual-workstations-to-an-azure-virtual-network"></a>Herstellen einer Verbindung ein Azure-virtuellen Netzwerk mit einzelnen Arbeitsstationen
Es möglicherweise vorkommen, dass Sie einzelne Entwickler oder Vorgänge Personal zum Verwalten von virtuellen Computern und Diensten in Azure aktivieren möchten. Angenommen, benötigen Sie Zugriff auf einen virtuellen Computern in einer Azure-virtuellen Netzwerk und Ihre Sicherheitsrichtlinie lässt sich nicht auf RDP oder SSH Remotezugriff auf einzelne virtuelle Maschinen. In diesem Fall können Sie eine Punkt-zu-Standort VPN-Verbindung.

Die Punkt-zu-Standort VPN-Verbindung verwendet das [SSTP VPN-](https://technet.microsoft.com/library/cc731352.aspx) Protokoll können Sie eine private und sichere Verbindung zwischen dem Benutzer und das virtuelle Azure-Netzwerk einrichten. Nachdem die VPN-Verbindung hergestellt wurde, werden der Benutzer RDP oder SSH über die VPN-Verbindung in einem beliebigen virtuellen Computern im Azure-virtuellen Netzwerk können (vorausgesetzt, dass der Benutzer authentifiziert kann und berechtigt ist).

Weitere Informationen:

- [Konfigurieren einer Punkt-zu-Standort-Verbindung zu einem virtuellen Netzwerk mithilfe der PowerShell](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

### <a name="connect-your-on-premises-network-to-an-azure-virtual-network-with-a-vpn"></a>Herstellen einer Verbindung ein Azure-virtuellen Netzwerk mit einem VPN mit Ihrem lokalen Netzwerk
Sie möchten Ihre gesamte Unternehmensnetzwerk oder Teile davon mit einem Azure-virtuellen-Netzwerk verbunden. Dies ist in Hybrid IT allgemeine Szenarien, in dem Unternehmen [Erweitern ihrer lokalen Datacenter in Azure](https://gallery.technet.microsoft.com/Datacenter-extension-687b1d84). In vielen Fällen hosten Unternehmen Teile eines Diensts in Azure und Webparts auf lokale, z. B., wenn eine Lösung Front-End-Webservern in Azure und Back-End-Datenbanken lokalen enthält. Nehmen Sie diese Art von "Cross lokale" Verbindungen auch Verwaltung von Azure ansässig Ressourcen mehr sichern und ermöglichen Szenarien wie das Active Directory-Domänencontroller in Azure erweitern.

Eine Möglichkeit hierfür besteht darin, eine [Standort-zu-Standort VPN](https://www.techopedia.com/definition/30747/site-to-site-vpn)verwenden. Der Unterschied zwischen einem Standort-zu-Standort VPN und ein Punkt-zu-Standort VPN ist, dass ein Punkt-zu-Standort VPN ein einzelnes Gerät zu einem Azure-virtuellen-Netzwerk verbindet, während Sie ein Standort-zu-Standort VPN ein gesamtes Netzwerk (beispielsweise lokale Netzwerk) zu einem Azure-virtuellen Netzwerk verbunden. Website-zu-Standort verwendeten zu einem Azure-virtuellen Netzwerk sehr sicheren IPsec Tunnelmodus VPN-Protokoll.

Weitere Informationen:

- [Erstellen einer VNet Ressourcenmanager mit einer Website-zu-Standort VPN-Verbindung über das Azure-Portal](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Planung und Design für VPN-gateway](../vpn-gateway/vpn-gateway-plan-design.md)

### <a name="connect-your-on-premises-network-to-an-azure-virtual-network-with-a-dedicated-wan-link"></a>Herstellen einer Verbindung ein Azure-virtuellen Netzwerk mit eine dedizierte WAN-Verbindung mit Ihrem lokalen Netzwerk
Punkt-zu-Standort und Standort-zu-Standort VPN-Verbindungen gelten für das Cross lokale Konnektivität aktivieren. Sollten Sie jedoch einige Organisationen können die folgenden Nachteile haben:

- VPN-Verbindungen Verschieben von Daten über das Internet – Dies stellt diese Verbindungen mit potenzieller Sicherheitsprobleme bei der Verlagerung von Daten über ein öffentliches Netzwerk zur Verfügung. Darüber hinaus können Zuverlässigkeit und Verfügbarkeit für internetverbindungen garantiert werden.
- VPN-Verbindungen mit Azure-virtuellen Netzwerken möglicherweise als eingeschränkter Bandbreite für einige Applikationen und Zwecke, sobald diese max, am um 200Mbps angesehen werden.

Organisationen, die in der Regel, Sicherheit und Verfügbarkeit die höchste Ebene für ihre Cross lokale Verbindungen benötigen verwenden dedizierte WAN Links für die Verbindung zu remote-Websites. Azure bietet Ihnen die Möglichkeit, eine dedizierte WAN-Verbindung zu verwenden, die mit Ihrem lokalen Netzwerk mit einem Azure-virtuellen-Netzwerk verbunden werden können. Dies wird durch Azure ExpressRoute aktiviert.

Weitere Informationen:

- [ExpressRoute – technische Übersicht](../expressroute/expressroute-introduction.md)

### <a name="connect-azure-virtual-networks-to-each-other"></a>Herstellen einer Verbindung untereinander mit Azure virtuelle Netzwerke
Es ist möglich, dass Sie viele Azure-virtuellen Netzwerke für die Bereitstellung verwendet werden soll. Es gibt viele Gründe, warum Sie dies tun können. Einer der Gründe möglicherweise Verwaltung zu vereinfachen; ein anderes möglicherweise aus Gründen der Sicherheit. Unabhängig von der Motivation oder zum Einfügen von Ressourcen in unterschiedlichen Azure-virtuellen Netzwerken Gründe möglicherweise manchmal Ressourcen auf den einzelnen Netzwerken miteinander zu verbinden möchten.

Eine Möglichkeit wäre für Services eine Verbindung zu Diensten auf einem anderen Azure-virtuellen Netzwerk "Schleife wieder" Azure Netzwerk virtuelle über das Internet. Die Verbindung zunächst in eine Azure-virtuellen Netzwerk, über das Internet wechseln, und dann wieder zum Ziel Azure-virtuellen Netzwerk. Diese Option stellt die Verbindung mit der Sicherheitsprobleme zu einem beliebigen internetbasierten Kommunikation gehörende zur Verfügung.

Eine bessere Option möglicherweise eine Azure virtuelle Netzwerk-Azure-virtuellen Netzwerk Standort-zu-Standort VPN erstellen. Azure virtuelle Netzwerk-Azure-virtuellen Netzwerk Standort-zu-Standort VPN dasselbe [IPsec Tunnelmodus](https://technet.microsoft.com/library/cc786385.aspx) Protokoll als weiter oben erwähnten Cross lokalen Standort-zu-Standort VPN-Verbindung verwendet wird.

Der Vorteil der Verwendung einer Azure virtuelle Netzwerk-Azure-virtuellen Netzwerk Standort-zu-Standort VPN ist, dass die VPN-Verbindung über der Netzwerkstruktur Azure eingerichtet wurde. Es ist nicht über das Internet verbinden. Dies stellt eine zusätzliche Sicherheitsebene im Vergleich zu Standort-zu-Standort VPN, die über das Internet verbinden.

Weitere Informationen:

- [Konfigurieren einer VNet-VNet-Verbindungs mithilfe von Azure Ressourcenmanager und PowerShell](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

## <a name="availability"></a>Verfügbarkeit
Verfügbarkeit ist eine wichtige Komponente von einem beliebigen Sicherheitsprogramm. Wenn Ihre Anwender und Systeme den benötigten Zugriff über das Netzwerk zugreifen können, der Dienst kann betrachtet gefährdet. Azure weist Netzwerke Technologien, die die folgenden Verfahren hoher Verfügbarkeit unterstützen:

- HTTP-basierten Lastenausgleich
- Netzwerklastenausgleich Ebene
- Globale Lastenausgleich

Lastenausgleich ist ein Verfahren Verbindungen zwischen mehreren Geräten gleichmäßig verteilt werden soll. Die Ziele für den Lastenausgleich sind:

- Verbessern der Verfügbarkeit – wenn Sie über mehrere Geräte Saldo Verbindungen zu laden, eine oder mehrere Geräte nicht verfügbar mehr können und können weiterhin auf die verbleibende online Geräte ausgeführten Dienste dienen den Inhalt aus dem Dienst
- Erhöhen der Leistung – beim Laden Doppelraten-Verbindungen über mehrere Geräte verfügt nicht über ein einzelnes Gerät ausführen den Prozessor Treffer. In diesem Fall ist der Verarbeitung und Speicherkapazität Auslastung für die Inhalte auf mehrere Geräte verteilen.

### <a name="http-based-load-balancing"></a>HTTP-basierten Lastenausgleich
Organisationen, die webbasierte Services häufig ausgeführt werden wünschen, dass ein HTTP-basierten Lastenausgleich vor helfen ausreichende Ebenen Performance und hohe Verfügbarkeit sicherzustellen, dass diese Webdienste. Im Gegensatz zu herkömmlichen Netzwerk-basierten Lastenausgleich den Lastenausgleich von HTTP-basierten vorgenommene Entscheidungen Lastenausgleich Merkmale im HTTP-Protokoll, die sich nicht auf das Netzwerk und Transport Layer Protokolle basieren.

Wenn Sie HTTP-basierten Lastenausgleich für Ihre Dienstleistungen webbasierten bereit, bietet Azure Azure Anwendung Gateways. Das Gateway der Azure-Anwendung unterstützt:

- HTTP-basierten Lastenausgleich – laden Lastenausgleich Entscheidungen erfolgen basierend auf Merkmale Inhalte an das HTTP-Protokoll
- Cookie-basierten Sitzung Zugehörigkeit – diese Funktion wird sichergestellt, dass es sich bei Verbindungen zu einem der Server hinter dieser Lastenausgleich zwischen Client und Server intakt bleibt. Stabilität der Transaktionen wird sichergestellt.
- SSL-Verschiebung – Wenn eine Clientverbindung hergestellt mit einem Lastenausgleich, dass die Sitzung zwischen dem Client und Lastenausgleich verschlüsselt ist mit der HTTPS (SSL /) Protokoll. Um die Leistung zu verbessern, müssen Sie jedoch die Option, um die Verbindung zwischen dem Lastenausgleich und dem Webserver hinter dem Lastenausgleich laden, verwenden Sie das Protokoll HTTP (Klartext). Dies ist als bezeichnet "SSL-Verschiebung", da die Webserver hinter den Lastenausgleich daher Serviceanfragen können schneller sollten und nicht den Prozessor Verwaltungsaufwand durch Verschlüsselung Funktionalität.
- URL-basierte des inhaltsroutings – dieses Feature ermöglicht es Ihnen, für den Lastenausgleich, wo Sie Verbindungen basierend auf die Ziel-URL weiterleiten Entscheidungen vornehmen. Dadurch wird ein viel mehr Flexibilität als Techniken in Lastenausgleich Entscheidungen basierend auf IP-Adressen zu laden.

Weitere Informationen:

- [Übersicht über die Anwendung Gateway](../application-gateway/application-gateway-introduction.md)

### <a name="network-level-load-balancing"></a>Netzwerklastenausgleich Ebene
Im Gegensatz zu HTTP-basierten Lastenausgleich macht Netzwerk Ebene Lastenausgleich laden Lastenausgleich Entscheidungen basierend auf IP-Adresse und den Port (TCP oder UDP) Zahlen.
Sie können die Vorteile mit dem Netzwerk Ebene Lastenausgleich in Azure mithilfe der Azure-Lastenausgleich nutzen. Einige Hauptmerkmale des Lastenausgleich Azure umfassen:

- Netzwerk Ebene Lastenausgleich basierend auf IP-Adresse und den Port Zahlen
- Unterstützung für eine Anwendung Layer-Protokoll
- Laden von Salden zu Azure-virtuellen Computern und Cloud services-Rolleninstanzen
- Für Internet zugänglichen (externe Lastenausgleich) und nicht-Internet verwendet werden können gegenüberliegende Applikationen (internen Lastenausgleich) und virtuellen Computern
- Endpunkt für die Überwachung, die verwendet wird, um festzustellen, ob beliebige dieser Dienste hinter den Lastenausgleich nicht mehr verfügbar sein müssen

Weitere Informationen:

- [Internet gegenüberliegende Lastenausgleich zwischen mehreren virtuellen Computern oder Diensten](../load-balancer/load-balancer-internet-overview.md)
- [Interner laden Lastenausgleich (Übersicht)](../load-balancer/load-balancer-internal-overview.md)

### <a name="global-load-balancing"></a>Globale Lastenausgleich
Einige Organisationen werden die höchste Ebene über die Verfügbarkeit von möglichen möchten. Eine Möglichkeit, dieses Ziel zu erreichen, besteht darin Host Applications in Global verteilten Rechenzentren. Wenn eine Anwendung in Data Center weltweit gehostet wird, ist es möglich, dass eine ganze geopolitische Region nicht verfügbar sind, und haben Sie noch die Anwendung von und ausgeführt.

Zusätzlich zur Verfügbarkeit Vorteile, denen, die Sie durch das Hosten von Applications in Global verteilten Rechenzentren kommunizieren, können Sie auch Leistungsvorteile erhalten. Mithilfe von ein Verfahren ein, der für den Dienst in das Rechenzentrum Anfragen weiterleitet, die am nächsten ist, das Gerät ist, die die Anforderung gesendet hat, können diese Leistungsvorteile abgerufen werden.

Globale Lastenausgleich können Sie diese Vorteile bieten. In Azure können Sie die Vorteile der globale Lastenausgleich mithilfe von Azure Datenverkehr Manager erhalten.

Weitere Informationen:

- [Was ist Datenverkehr Manager?](../traffic-manager/traffic-manager-overview.md)

## <a name="logging"></a>Protokollierung
Eine zentrale Funktion für jedes Netzwerk Sicherheitsszenario ist die Protokollierung in einem Netzwerk auf. In Azure können Sie Informationen, die für Netzwerk Sicherheitsgruppen abzurufenden Netzwerkebene Protokollierung Informationen erhalten protokollieren. Bei der Protokollierung NSG erhalten Sie Informationen aus:

- Überwachungsprotokolle – diese Protokolle werden alle Vorgänge, die Ihren Azure Abonnements übermittelten Anzeigen verwendet. Diese Protokolle sind standardmäßig aktiviert und innerhalb des Portals Azure verwendet werden können.
- Ereignisprotokollen – diese Protokolle Angaben zur welche NSG Regeln angewendet wurden.
- Zähler Protokolle – diese Protokolle informiert Sie darüber, wie oft jede NSG Regel zum verweigern, oder lassen Sie den Datenverkehr angewendet wurde.

Sie können auch [Microsoft Power BI](https://powerbi.microsoft.com/what-is-power-bi/), eines leistungsfähige Visualisierung-Tools, zum Anzeigen und analysieren diese Protokolle verwenden.

Weitere Informationen:

- [Log Analytics für Netzwerk-Sicherheitsgruppen (NSGs)](../virtual-network/virtual-network-nsg-manage-log.md)

## <a name="name-resolution"></a>Mit einer namensauflösung von
Mit einer namensauflösung von ist eine kritische Funktion für alle Dienste, die Sie in Azure hosten. Aus Gründen der Sicherheit kann Kompromisse der Funktion mit einer Auflösung von Namen zu Angreifer umleiten Anfragen aus Ihren Websites zu einer seine Website führen. Sichere namensauflösung ist eine Vorbedingung für alle gehostet Clouddienste.

Es gibt zwei Arten von mit einer namensauflösung von Adresse benötigte aus:

- Interne Name – mit einer Auflösung von internen namensauflösung wird von Dienste auf Ihre Azure-virtuellen Netzwerke, Ihren lokalen Netzwerken oder beides verwendet. Namen für das internen namensauflösung sind nicht über das Internet zugänglich. Für optimale Sicherheit zu gewährleisten ist es wichtig, dass Ihre internen Schema nicht für externe Benutzer zugegriffen werden kann.
- Auflösung externer Namen – mit einer namensauflösung von externen wird von Personen und Geräten außerhalb Ihrer lokalen und Azure-virtuellen Netzwerken verwendet. Hierbei handelt es sich um den Namen, die im Internet sichtbar sind, und werden verwendet, um das direkte Verbindung mit der Cloud-basierte Services.

Für interne namensauflösung haben Sie zwei Optionen:

- Ein Azure-virtuellen Netzwerk-DNS-Server – beim Erstellen eines neuen virtuellen Azure-Netzwerks, wird ein DNS-Server für Sie erstellt. Die Namen der Computer in einem Netzwerk virtuelle Azure befinden kann diesen DNS-Server aufgelöst werden. Dieser DNS-Server kann nicht konfiguriert und von der Azure-Struktur-Manager, wodurch die Nutzung der Lösung eines sicheren Namen mit einer Auflösung von verwaltet wird.
- Zeigen Sie Ihre eigenen DNS-Server – und Sie haben die Möglichkeit, mit einem DNS-Server Ihrer Wahl in Ihrem Azure-virtuellen Netzwerk. Dieser DNS-Server könnte, dass eine Active Directory-integrierte DNS-Server oder eine dedizierte DNS-Server-Lösung bereitgestellt, die von einem Azure Partner, die Sie aus dem Azure Marketplace abrufen können.

Weitere Informationen:

- [Virtuelle Network (Übersicht)](../virtual-network/virtual-networks-overview.md)
- [Verwalten von DNS-Server, eine virtuelle Netzwerk (VNet) verwendet wird](../virtual-network/virtual-networks-manage-dns-in-vnet.md)

Für externe DNS-Lösung haben Sie zwei Optionen:

- Ihre eigenen externen DNS-Server lokal gehostet
- Hosten Sie Ihrer eigenen externen DNS-Server mit einem Internetdienstanbieter

Viele große Organisationen, die ihre eigenen DNS-Server lokal gehostet werden. Sie möglich dies, da sie die Netzwerke Fachwissen und globale Präsenz vergeblich sind.

In den meisten Fällen empfiehlt sich die DNS-Namen mit einer Auflösung von Dienste mit einem Internetdienstanbieter hosten. Diese Dienstanbieter haben die und eine globale Präsenz zu sehr hohe Verfügbarkeit für Ihre Namen mit einer Auflösung von Dienstleistungen sicherzustellen. Verfügbarkeit ist unverzichtbar für DNS-Dienste, da, wenn Ihre Namen mit einer Auflösung von Dienstleistungen ein Fehler auftreten, niemand mit Ihrer Dienste gegenüberliegende Internet herstellen kann.

Azure bietet Ihnen eine hochgradig verfügbar und leistungsfähigen externen DNS-Lösung in Form von Azure DNS. Lösung mit einer Auflösung von externen Namen nutzt der weltweiten Azure DNS-Infrastruktur. Sie können Sie Ihre Domäne in Azure verwenden dieselben Anmeldeinformationen, APIs, Tools und Abrechnung als anderer Azure Dienste hosten. Als Bestandteil der Azure erbt es auch die hohe Sicherheit Steuerelemente in die Plattform integriert.

Weitere Informationen:

- [Azure DNS (Übersicht)](../dns/dns-overview.md)

## <a name="dmz-architecture"></a>DMZ Architektur
DMZ Formular vieler Unternehmen nur mit ihren Netzwerken zum Erstellen einer Puffer-Zone zwischen dem Internet und ihre Dienste segmentieren. Der DMZ Teil des Netzwerks wird eine niedrige Sicherheitsstufe Zone betrachtet und keine wichtigen Anlagen in diesem Netzwerksegment platziert werden. Sicherheit Netzwerkgeräte, die eine Schnittstelle im DMZ-Segment und einen anderen Netzwerk-Benutzeroberfläche mit einem Netzwerk, bei dem virtuellen Computern und Diensten, die aus dem Internet eingehende Verbindungen akzeptieren verbunden haben, wird in der Regel angezeigt werden.

Es gibt eine Reihe von Variationen der DMZ entwerfen und die Entscheidung zum Bereitstellen einer DMZ, und welche Art von DMZ verwenden, wenn Sie sich entscheiden, die eine verwenden dann basiert auf den Erfordernissen Ihres Netzwerks Sicherheit.

Weitere Informationen:

- [Microsoft-Cloud-Diensten und Netzwerk-Sicherheit](../best-practices-network-security.md)

## <a name="azure-security-center"></a>Azure-Sicherheitscenter
Sicherheitscenter hilft Ihnen zu verhindern, erkennen und Beantworten von Risiken und stellt, dass Sie die Transparenz, und steuern, die Sicherheit Ihrer Azure Ressourcen erhöht. Es bietet integrierte Sicherheit Überwachung und Policy Management über Ihre Azure-Abonnements, hilft Angriffen, die andernfalls aufgefallen und funktioniert mit einem ausgedehnten System von Lösungen Sicherheit erkennen.

Azure-Sicherheitscenter unterstützt Sie beim Optimieren und überwachen Netzwerk Sicherheit durch:

- Bereitstellen von Netzwerk Sicherheit Empfehlungen
- Überwachen des Status der Netzwerkkonfiguration Sicherheit
- Warnung bei Network-basierten Risiken sowohl auf den Endpunkt und Netzwerk Ebenen

Weitere Informationen:

- [Einführung in Azure-Sicherheitscenter](../security-center/security-center-intro.md)

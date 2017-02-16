<properties
   pageTitle="Bewährte Methoden für die Sicherheit von Azure Netzwerk | Microsoft Azure"
   description="Dieser Artikel bietet eine Reihe von bewährte Methoden für die Verwendung von Netzwerk-Sicherheit in Azure-Funktionen erstellt werden."
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="swadhwa"
   editor="TomShinder"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/25/2016"
   ms.author="TomSh"/>

# <a name="azure-network-security-best-practices"></a>Bewährte Methoden für die Sicherheit von Azure Netzwerk

Microsoft Azure ermöglicht es Ihnen virtuellen Computern und Einheiten an andere Netzwerkgeräte verbinden, indem sie auf Azure-virtuellen Netzwerken platziert werden. Ein Azure-virtuellen Netzwerk ist ein virtuelles Netzwerk erstellen, die Sie in Verbindung mit einem virtuellen Netzwerk zulassen der Kommunikation zwischen aktiviert Netzwerkgeräte TCP/IP basierenden virtuelles Netzwerk Benutzeroberflächen-Karten ermöglicht. Azure-virtuellen Computern mit einer Azure-virtuellen-Netzwerk verbunden sind, Geräte auf derselben Azure virtuellen Netzwerk und anderen Azure virtuelle Netzwerke, im Internet oder sogar auf Ihrer eigenen lokalen Netzwerken eine Verbindung herstellen können.

In diesem Artikel wird eine Zusammenstellung von bewährte Methoden für die Sicherheit von Azure Netzwerk besprochen. Bewährten unserer Erfahrung mit Azure Netzwerke abgeleitet sind und die Verwendung des Kunden wie selbst.

Für jede bewährte Methode wird erläutert:

- Was ist die beste Methode
- Warum Sie diese bewährte Methode aktivieren möchten
- Was kann das Ergebnis sein, wenn Sie nicht die beste Methode aktivieren
- Mögliche Alternativen bewährte Methode
- Wie können Sie lernen, wie die bewährte Methode aktivieren

In diesem Artikel bewährte Methoden für Azure Netzwerk Sicherheit basiert auf eine Übereinstimmung Meinung und Azure-Plattformfunktionen und Features, sie zu dem Zeitpunkt vorhanden sind, die in diesem Artikel geschrieben wurde. Meinungen und Technologien Zeitverlauf ändern und in diesem Artikel wird in regelmäßigen Abständen diese Änderungen entsprechend aktualisiert werden.

Azure Netzwerk bewährte Methoden für Sicherheit in diesem Artikel beschriebenen umfassen:

- Logisch Segment Subnetze
- Steuern der Weiterleitung Verhalten
- Aktivieren eines erzwungenen Tunnel
- Verwenden von virtuellen Netzwerkgeräte
- Bereitstellen von DMZ für Sicherheit zoning
- Vermeiden Sie mit dem Internet mit dedizierten WAN Links anzeigen
- Optimieren der Verfügbarkeit und Leistung
- Globale Lastenausgleich verwenden
- Deaktivieren Sie RDP Zugriff auf Azure-virtuellen Computern
- Azure-Sicherheitscenter aktivieren
- Erweitern Sie in Azure Datencenters


## <a name="logically-segment-subnets"></a>Logisch Segment Subnetze

[Virtuelle Netzwerke Azure](https://azure.microsoft.com/documentation/services/virtual-network/) sind ähnlich wie mit einem LAN in Ihrem lokalen Netzwerk. Die Idee hinter ein Azure-virtuellen Netzwerk ist, dass Sie ein einzelnes privates IP-Adresse Leerzeichen-Netzwerk erstellen, in dem alle Ihre [Azure-virtuellen Computern](https://azure.microsoft.com/services/virtual-machines/)platziert werden können. Die privaten IP-Adresse Leerzeichen verfügbar sind in der Klasse A (10.0.0.0/8), Klasse B (172.16.0.0/12) und Klasse C Bereiche (192.168.0.0/16).

Was ähnlich wie lokale Aktionen, die wahrscheinlich möchten Sie den größeren Adressbereich in Subnetze segmentieren. [CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) -basierte Subnetz Prinzipien können Sie Ihre Subnetze erstellen.

Routing zwischen Subnetzen tritt automatisch, und Sie müssen nicht zum manuellen Konfigurieren von routing-Tabellen. Die Standardeinstellung ist jedoch, dass keine Netzwerk-Access-Steuerelemente zwischen den Subnets, die Sie, klicken Sie auf das virtuelle Azure-Netzwerk erstellen enthalten sind. Access-Steuerelemente zwischen Subnetzen Netzwerk zu erstellen, müssen Sie etwas zwischen den Subnets setzen.

Eine der Maßnahmen, die Sie verwenden können, um diese Aufgabe auszuführen ist eine [Sicherheitsgruppe Netzwerk](../virtual-network/virtual-networks-nsg.md) (NSG). NSGs sind einfache dynamische Paket Prüfung Geräte 5-Tupels (die Quelle IP-, Quellport, IP-Zieladresse, Zielport und Layer 4-Protokoll) verwenden Ansatz zum Zulassen/Verweigern Erstellen von Regeln für den Netzwerkverkehr. Sie können zulassen oder Datenverkehr an und von einzelnen IP-Adresse zu von mehreren IP-Adressen oder sogar und aus ganze Subnets verweigern.

NSGs für Access-Steuerelements zwischen Subnetzen Netzwerk verwenden, können Sie Ressourcen setzen, die die gleichen Sicherheitszone oder Rolle in ihren jeweiligen Subnetzen angehören. Betrachten Sie beispielsweise eine einfache 3-Ebenen-Anwendung, die eine Stufe Web kann es eine Anwendung Logikebene und eine Datenbank weist. Sie setzen virtuellen Computern, die zu den einzelnen diese Ebenen in eigene Subnetze gehören. Verwenden Sie dann NSGs um zu steuern, den Datenverkehr zwischen den Subnets aus:

- Web in virtuellen Computern können nur Verbindungen auf den Computern der Anwendung Logik initiieren und annehmen können nur Verbindungen aus dem Internet
- Anwendung Logik virtuellen Computern können nur Verbindungen mit Datenbankebene initiieren und annehmen können nur Verbindungen aus der Webebene
- Datenbank in virtuellen Computern kann keine Verbindung mit etwas außerhalb der eigenen Subnetz initiieren und kann nur von der Anwendung Logikebene Verbindungen zulassen

Lesen Sie den Artikel, um weitere Informationen zu Sicherheitsgruppen Netzwerk und wie Sie diese Ihre Azure-virtuellen Netzwerke logisch segmentieren verwenden können, [Was ist eine Sicherheitsgruppe Netzwerk](../virtual-network/virtual-networks-nsg.md) (NSG).

## <a name="control-routing-behavior"></a>Steuern der Weiterleitung Verhalten

Wenn Sie einen virtuellen Computer in ein Azure-virtuellen Netzwerk vorsehen, sehen Sie, dass des virtuellen Computers mit anderen virtuellen Computern im selben Azure virtuelle Netzwerk herstellen kann, auch wenn die anderen virtuellen Computern in verschiedenen Subnetzen sind. Der Grund, warum dies möglich ist, ist, dass es folgt eine Zusammenstellung von System weitergeleitet, die standardmäßig aktiviert sind, mit denen diese Art der Kommunikation. Diese Standard-leitet zulassen virtuellen Computern im selben Azure virtuelle Netzwerk Verbindungen mit miteinander und mit dem Internet (für ausgehende Kommunikation mit dem Internet nur) einleiten.

Während der Standard-System leitet für viele Szenarien für die Bereitstellung verwendet werden, gibt es jedoch vorkommen, dass Sie die routing-Konfiguration für die Bereitstellung Ihrer anpassen möchten. Diese Anpassungen wieder ermöglicht Ihnen so konfigurieren Sie die Adresse des nächste Abschnitts um bestimmte Ziele erreicht haben.

Es empfiehlt sich, dass Sie beim Bereitstellen einer Einheit virtuelles Netzwerk Sicherheit, wir in einer späteren bewährte Methode sprechen werden Benutzer definiert leitet konfigurieren.

> [AZURE.NOTE] Benutzer definiert ist, sind nicht erforderlich, und der Standard-System leitet funktionieren in den meisten Fällen.

Sie erhalten weitere Informationen zu Benutzer definiert ist und wie Sie diese konfigurieren, indem Sie im Artikel [Was sind Benutzer definiert ist und die IP-Weiterleitung](../virtual-network/virtual-networks-udr-overview.md)lesen.

## <a name="enable-forced-tunneling"></a>Aktivieren eines erzwungenen Tunnel

Zum besseren Verständnis erzwungenen Tunnel ist es sinnvoll, zu verstehen, welche "geteilte Tunnel" ist.
Die am häufigsten verwendeten Beispiel für geteilte Tunnel wird mit VPN-Verbindungen angezeigt. Stellen Sie sich, dass Sie eine VPN-Verbindung mit dem Unternehmensnetzwerk aus Ihrer Hotelzimmer herstellen. Dieser Verbindung können Sie die Verbindung zu Ressourcen in Ihrem Netzwerk Ihres Unternehmens und alle Kommunikation mit Ressourcen im Unternehmensnetzwerk durch den VPN-Tunnel wechseln.

Was geschieht, wenn Sie mit den Ressourcen im Internet verbinden möchten? Wenn geteilten Tunnel aktiviert ist, wechseln Sie direkt mit dem Internet und nicht durch den VPN-Tunnel diese Verbindungen. Einige Experten für Sicherheit sollten Sie diese Option, um ein potenzieller Risiko werden und empfehlen daher geteilten Tunnel deaktiviert werden und alle Verbindungen, den für das Internet bestimmt ist, und den für Ressourcen des Unternehmens, bestimmt den VPN-Tunnel aufzurufen. Der Vorteil hierfür ist, dass Verbindungen mit dem Internet dann über die Sicherheit Unternehmensnetzwerk Geräte, das der Fall wäre erzwungenen, wenn der VPN-Client mit dem Internet außerhalb der VPN-Tunnel verbunden.

Jetzt wir fügen Sie dieser wieder zu virtuellen Computern in einer Azure-virtuellen Netzwerk. Die standardmäßige leitet für ein Azure-virtuellen Netzwerk zulassen virtuellen Computern Datenverkehr mit dem Internet einleiten. Dies kann auch ein Sicherheitsrisiko darzustellen, wie diese ausgehenden Verbindungen die Angriffsfläche eines virtuellen Computers vergrößern konnte und von Angreifern genutzt werden.
Aus diesem Grund wird empfohlen, dass Sie erzwungenen Tunnel auf Ihre virtuellen Computern aktivieren Wenn Sie übergreifend lokale Verbindung zwischen Ihrem Azure-virtuellen Netzwerk und Ihrem lokalen Netzwerk haben. Wir werden cross lokale Konnektivität weiter unten in diesem Azure Netzwerke bewährte Methoden Dokument zu sprechen.

Wenn Sie nicht über eine Cross lokale Verbindung verfügen, stellen Sie sicher, Sie nutzen Netzwerk Sicherheitsgruppen (erörtert) oder Azure-virtuellen Netzwerk Wertpapiers Einheiten (erläutert weiter) um ausgehende Verbindungen mit dem Internet aus Ihren Azure-virtuellen Computern zu verhindern.

Weitere Informationen zu den, Tunnel und wie Sie aktivieren, wird Sie im Artikel [Konfigurieren Tunnel wird mithilfe von PowerShell und Azure Ressourcenmanager](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md)lesen.

## <a name="use-virtual-network-appliances"></a>Verwenden von virtuellen Netzwerkgeräte

Während Sicherheitsgruppen Netzwerk und Benutzer definiert Routing eine bestimmte Measure Netzwerk Wertpapiers am im Netzwerk befinden und Transport Ebenen des [OSI-Modells](https://en.wikipedia.org/wiki/OSI_model)zur Verfügung stellen können, werden es jetzt Situationen sein, wenn Sie erhalten möchten oder müssen Sie Sicherheit auf hoher Ebene des Stapels aktivieren. In diesen Fällen wird empfohlen, dass Sie virtuelle Sicherheit Netzwerkgeräte von Azure Partner bereitgestellten bereitstellen.

Azure-Sicherheit Netzwerkgeräte können wesentlich erweiterte Sicherheitsebenen vorführen, über welche von Ebene Netzwerk-Steuerelementen zur Verfügung steht. Einige der Funktionen Netzwerk Sicherheit von virtuellen Sicherheit Netzwerkgeräte einbeziehen möchten:

- Durch eine Firewall
- Intrusion Erkennung/Intrusion Prevention
- Sicherheitsrisiko management
- Steuerelement Anwendung
- Netzwerk-basierte Normalbetriebswerte
- Web-Filterung
- Antivirenprogramm
- Botnet Schutz

Wenn Sie von Network Security eine höhere Ebene anfordern, als Sie mit dem Netzwerk Zugriff auf Ebene Steuerelemente erhalten können, empfehlen wir, dass Sie ermitteln und Azure virtuelle Sicherheit Netzwerkgeräte bereitstellen.

Informationen darüber, welche Azure virtuelle Sicherheit Netzwerkgeräte verfügbar sind und deren Funktionen finden Sie auf der [Azure Marketplace](https://azure.microsoft.com/marketplace/) und suchen Sie nach "Sicherheit" und "Network Security".

##<a name="deploy-dmzs-for-security-zoning"></a>Bereitstellen von DMZ für Sicherheit zoning
Eine DMZ oder "Umfang Netzwerk" ist eine physische oder logische Netzwerksegment, die eine zusätzliche Sicherheitsebene zwischen Ihrer Ressourcen und im Internet bereitgestellt werden sollen. Der Zweck des der DMZ besteht darin, das spezielle Access Steuerelement Netzwerkgeräte auf den Rand des Netzwerks DMZ zu platzieren, damit nur die gewünschten Datenverkehr hinter dem Netzwerk Sicherheitsgerät und in Ihr Azure-virtuellen Netzwerk zulässig ist.

DMZ sind nützlich, da Sie Ihre Steuerelement Verwaltung, Überwachung, Protokollierung und reporting auf Geräten am Rand des Netzwerks Azure virtuelle konzentrieren können. Hier würden Sie normalerweise DDoS Prevention, Erkennung/Intrusion Intrusionsprävention Systeme (IDS/IP-Adressen), Firewall-Regeln und Richtlinien, Web-Filterung, Netzwerk-Modul und mehr aktivieren. Die Sicherheit Netzwerkgeräte zwischen dem Internet und Ihrem Azure-virtuellen Netzwerk befinden und eine Benutzeroberfläche in beiden Netzwerken haben.

Während sich die grundlegende Gestaltung von einer DMZ handelt, es gibt viele verschiedene DMZ Entwürfe, wie kaskadierende dreifach gehostet, mehrfach vernetzten und andere.

Wir empfehlen, dass Sie darüber nachdenken, einer DMZ zum Verbessern Sie der Sicherheitsstufe Netzwerk für Ihre Azure-Ressourcen für alle hohe Sicherheit Bereitstellungen.

Weitere Informationen zum DMZ und wie Sie sie in Azure bereitstellen, lesen Sie im Artikel [Microsoft-Cloud-Diensten und Netzwerk Sicherheit](../best-practices-network-security.md).

## <a name="avoid-exposure-to-the-internet-with-dedicated-wan-links"></a>Vermeiden Sie mit dem Internet mit dedizierten WAN Links anzeigen
Viele Organisationen haben die Hybrid IT-Routing. In Hybrid IT sind einige der Datenbestände des Unternehmens in Azure, während andere lokale bleiben. In vielen Fällen werden einige Komponenten eines Diensts in Azure ausgeführt werden, während andere unsichere Komponenten lokal bleiben.

In der Hybrid IT-Szenario ist in der Regel einige Art Cross lokale Verbindung. Diese Cross lokale, dass Konnektivität des Unternehmens zu ihren lokalen Netzwerken mit Azure-virtuellen Netzwerken eine Verbindung herstellen können. Es gibt zwei Cross lokale Konnektivitätslösungen zur Verfügung:

- Website-zu-Standort VPN
- ExpressRoute

[Standort-zu-Standort VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md) stellt eine virtuelle private Verbindung zwischen Ihrem lokalen Netzwerk und ein Azure-virtuellen Netzwerk. Diese Verbindung erfolgt über das Internet und ermöglicht es Ihnen, "Tunnel" Informationen in eine verschlüsselte Verbindung zwischen Ihrem Netzwerk und Azure. Website-zu-Standort VPN ist eine sichere, Reifen Technologie, die von Unternehmen aller Größen Jahrzehnten bereitgestellt wurde. Tunnel-Verschlüsselung erfolgt im [IPSec-Tunnel](https://technet.microsoft.com/library/cc786385.aspx).

Während der Standort-zu-Standort VPN eine vertrauenswürdige, zuverlässig und definierte Technologie ist, Datenverkehr innerhalb der Tunnel im Internet durchlaufen. Darüber hinaus wird eingeschränkter Bandbreite relativ zu einem Maximum von Informationen zu 200Mbps.

Wenn Sie einem außergewöhnlichen Pegel Sicherheit oder Leistung für Ihre Cross lokale Verbindungen benötigen, empfehlen wir, dass Sie für Ihre Cross lokale Konnektivität Azure ExpressRoute verwenden. ExpressRoute ist eine dedizierte WAN Verknüpfung zwischen Ihrem lokalen Speicherort oder ein Exchange-Hostinganbieter. Da es sich um eine Verbindung Telekommunikation handelt, werden Ihre Daten nicht über das Internet übertragen und daher für potenziellen Risiken im Internetkommunikation nicht sichtbar.

Finden Sie weitere Informationen zur Funktionsweise von Azure ExpressRoute und zum Bereitstellen, lesen Sie im Artikel [ExpressRoute – technische Übersicht](../expressroute/expressroute-introduction.md).

## <a name="optimize-uptime-and-performance"></a>Optimieren der Verfügbarkeit und Leistung
Vertraulichkeit, Integrität und Verfügbarkeit (CIA) bestehen aus der Triade des heutigen am häufigsten Einfluss Sicherheitsmodell. Vertraulichkeit wird über die Verschlüsselung und Datenschutz, Integrität geht es stellt sicher, dass die Daten von unbefugten Personal nicht geändert werden, und Verfügbarkeit geht es stellt sicher, dass befugte Personen können auf die Informationen zugreifen, die sie für den Zugriff auf autorisiert sind. Fehler in jeder der folgenden Bereiche stellt eine mögliche Verletzung Sicherheit.

Verfügbarkeit sozusagen als zur Verfügbarkeit und Leistung wird. Wenn ein Dienst ausgefallen ist, kann Informationen zugegriffen werden. Ist Leistung daher beeinträchtigt unter ', um die Daten unbrauchbar zu machen, können wir die Daten nicht zugegriffen werden berücksichtigen. Aus Gründen der Sicherheit müssen wir daher, welchen wir können, um sicherzustellen, dass unserer Dienste optimale Verfügbarkeit und Leistung haben kann.
Eine beliebte und effektive Methode zur Verfügbarkeit und Leistung zu verbessern sollten Lastenausgleich verwenden. Lastenausgleich ist eine Methode zum Verteilen von Netzwerkdatenverkehr auf Servern, die Teil eines Diensts sind. Angenommen, wenn Sie als Teil des Diensts Front-End-Webservern verfügen, können Sie den Lastenausgleich den Datenverkehr auf mehrere Front-End-Webservern verteilen.

Diese Verteilung des Datenverkehrs erhöht die Verfügbarkeit, da der, wenn einer der Webserver nicht verfügbar ist, der Lastenausgleich beenden, senden den Datenverkehr auf diesem Server und leiten Sie den Datenverkehr auf dem Server, die noch online sind. Lastenausgleich hilft auch Leistung, da der Prozessor, Netzwerk und Arbeitsspeicher, dass alle beim Laden Verwaltungsaufwand zum Beantworten von Besprechungsanfragen verteilt werden Server ausgelastet.

Es empfiehlt sich, dass Sie den Lastenausgleich möglich, und je nach Bedarf für Ihre Dienstleistungen einsetzen. Wir werden in den folgenden Abschnitten Angemessenheit Adresse.
Ebene der Azure-virtuellen Netzwerk bietet Azure an, dass Sie mit drei primären Lastenausgleich Optionen laden:

- HTTP-basierten Lastenausgleich
- Externe Lastenausgleich
- Interner Lastenausgleich

## <a name="http-based-load-balancing"></a>HTTP-basierten Lastenausgleich
HTTP-basierten Lastenausgleich basiert entscheiden, welche Server Verbindungen mit Merkmale im HTTP-Protokoll zu senden. Azure verfügt über eine HTTP-Lastenausgleich, die nach dem Namen des Gateways Anwendung wechselt.

Wir empfehlen, die Sie uns Azure Application Gateway beim:

- Programme, die erfordern Anfragen der gleichen Benutzer/Client-Sitzung auf der gleichen Back-End-virtuellen Computern erreicht haben. Beispiele für diese würde Einkaufswagen apps und e-Mail-Webservern Einkaufs-werden.
- Programme, die Web-Server-Farmen von SSL Beendigung Verwaltungsaufwand durch die Nutzung der Anwendung des Gateways [SSL Auslagern](https://f5.com/glossary/ssl-offloading) Feature freigegeben werden soll.
- Anwendungen, wie ein Netzwerk Bereitstellung von Inhalten erforderlich, die mit, dass mehrere HTTP-Anforderungen auf derselben langer TCP-Verbindung weitergeleitet werden oder laden zu anderen Back-End-Servern verteilt.

Finden Sie weitere Informationen zur Funktionsweise von Azure Application Gateway und wie Sie es in Ihre Bereitstellungen verwenden können, lesen Sie im Artikel [Application Gateway Overview](../application-gateway/application-gateway-introduction.md).

## <a name="external-load-balancing"></a>Externe Lastenausgleich
Externe Lastenausgleich erfolgt bei eingehende Verbindungen aus dem Internet Lastenausgleich zwischen Ihren Servern in einem Azure-virtuellen Netzwerk ansässig sind. Externe Azure Lastenausgleich können Sie diese Funktion bereitstellen, und es empfiehlt sich, dass verwenden, wenn Sie nicht die Kurznotizen Sitzungen oder SSL auslagern.

Im Gegensatz zu HTTP-basierten Lastenausgleich verwendet der externen Lastenausgleich Informationen in das Netzwerk und Transport Schichten des OSI-Modells Netzwerke aus, um die Entscheidungen treffen, auf welche Server zu Doppelraten-Verbindung zu laden.

Es empfiehlt sich, dass Sie externe Lastenausgleich verwenden, sobald Sie [statusfreie Applikationen](http://whatis.techtarget.com/definition/stateless-app) eingehende Anfragen aus dem Internet akzeptiert haben.

Finden Sie im Artikel [Erstellen eines Internet gegenüberliegende Lastenausgleich in Ressourcenmanager mithilfe der PowerShell Get-Schritte](../load-balancer/load-balancer-get-started-internet-arm-ps.md), erfahren mehr über die Funktionsweise der externen Azure Lastenausgleich, und melden, wie Sie es, bereitstellen können.

## <a name="internal-load-balancing"></a>Interner Lastenausgleich
Interner Lastenausgleich ähnelt dem externen Lastenausgleich und verwendet das gleiche Verfahren um Doppelraten-Verbindungen auf die Server hinter diese zu laden. Der einzige Unterschied ist, dass Lastenausgleich Verbindungen aus virtuellen Computern, die nicht im Internet sind, in diesem Fall akzeptiert wird. In den meisten Fällen sind die Verbindungen, die für den Lastenausgleich akzeptiert werden von Geräten in einem Azure-virtuellen Netzwerk initiiert.

Es empfiehlt sich, internen Lastenausgleich für Szenarien, die von dieser Funktion, wie z. B., wenn Sie laden Doppelraten-Verbindungen mit SQL Server oder internen Webservern müssen profitieren zu verwenden.

Finden Sie weitere Informationen zur Funktionsweise von Azure internen Lastenausgleich und wie Sie es bereitstellen können, lesen Sie im Artikel [Erstellen eines internen Lastenausgleich mithilfe der PowerShell Get-Schritte](../load-balancer/load-balancer-get-started-internet-arm-ps.md#update-an-existing-load-balancer).

## <a name="use-global-load-balancing"></a>Globale Lastenausgleich verwenden
Öffentliche Cloud computing ist es möglich, Global bereitzustellen die Applikationen, die Komponenten in Rechenzentren weltweit ansässig sind verteilt. Dies ist auf Microsoft Azure aufgrund Azure des globalen Datacenter Anwesenheitsinformationen möglich. Im Gegensatz zu den Lastenausgleich Technologien weiter oben erwähnten ermöglicht der globale Lastenausgleich Services verfügbar machen, auch wenn der gesamte Rechenzentren nicht mehr verfügbar sind möglicherweise.

Sie können diese Art von globaler Lastenausgleich in Azure durch die Nutzung der [Azure Datenverkehr Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)erhalten. Datenverkehr-Managers lassen kann Doppelraten-Verbindungen mit Ihrer Dienste anhand der Position des Benutzers zu laden.

Wenn der Benutzer eine Anforderung an den Dienst aus der EU gesendet hat, wird die Verbindung zu Ihrer Dienste befindet sich in einem Rechenzentrum EU geleitet. Dieses Teils des Datenverkehrs-Managers globale laden Lastenausgleich Leistung Leben, da Herstellen einer Verbindung mit der nächste Datacenter schneller ist als Herstellen einer Verbindung mit Datencenter, die weit entfernt werden können.

Klicken Sie auf der Seite Verfügbarkeit sichergestellt globaler Lastenausgleich, dass Ihr Dienst verfügbar ist, auch wenn Sie eine gesamte Datacenter zur Verfügung stehen soll.

Angenommen, wenn ein Azure Datencenter Umgebung Gründen oder aufgrund Ausfall (z. B. Landes-/ Netzwerkfehler) nicht mehr verfügbar sollte, Verbindungen mit Ihrem Dienst würde werden umgeleitet zu der nächste online Datacenter. Diese globaler Lastenausgleich erfolgt durch die Nutzung von DNS-Richtlinien, die Sie in den Datenverkehr Manager erstellen können.

Wir empfehlen, Sie Datenverkehr Manager für alle Cloudlösung, die Sie entwickeln mit, die hat einen stark verteilten Bereich über mehrere Regionen und die höchste Ebene der Verfügbarkeit von möglichen erfordert.

Erfahren Sie mehr über Azure Datenverkehr Manager und wie diese bereitstellen, lesen Sie im Artikel [Was ist Datenverkehr-Manager](../traffic-manager/traffic-manager-overview.md).

## <a name="disable-rdpssh-access-to-azure-virtual-machines"></a>Deaktivieren Sie RDP/SSH-Zugriff auf Azure-virtuellen Computern
Es ist möglich, Azure-Computer mithilfe des [Remote Desktop Protocol](https://en.wikipedia.org/wiki/Remote_Desktop_Protocol) (RDP) und den [Secure Shell](https://en.wikipedia.org/wiki/Secure_Shell) (SSH) Protokollen erreicht haben. Diese Protokolle ermöglichen das Verwalten von virtuellen Computern von remote-Standorten und standard in Datacenter computing sind.

Das potenzielle Sicherheitsproblem mit über diese Protokolle über das Internet ist, dass Angreifern verschiedene [Bruteforce](https://en.wikipedia.org/wiki/Brute-force_attack) Techniken für den Zugriff auf Azure-Computer verwenden können. Sobald die Angreifer zugreifen, können sie verwenden des virtuellen Computers als ein Ausgangspunkt für andere Computer im Netzwerk virtuelle Azure beeinträchtigen oder sogar Angriffen Netzwerkgeräte außerhalb Azure.

Aus diesem Grund wird empfohlen, dass Sie RDP und SSH Direktzugriff zu Ihren Azure-virtuellen Computern aus dem Internet deaktivieren. Nachdem RDP und SSH Direktzugriff aus dem Internet deaktiviert ist, müssen Sie andere Optionen, die Sie verwenden können, um diesen virtuellen Computern für remote-Verwaltung zuzugreifen:

- Punkt-zu-Standort VPN
- Website-zu-Standort VPN
- ExpressRoute

[Punkt-zu-Standort VPN](../vpn-gateway/vpn-gateway-point-to-site-create.md) ist ein anderer Begriff für eine RAS VPN Client-/Server-Verbindung. Ein Punkt-zu-Standort VPN ermöglicht einen einzelnen Benutzer über das Internet eine Verbindung mit einem Azure-virtuellen Netzwerk. Nachdem die Punkt-zu-Standort-Verbindung hergestellt wurde, wird der Benutzer sein zum RDP oder SSH Herstellen einer Verbindung mit einem beliebigen virtuellen Computern im Azure-virtuellen-Netzwerk, die der Benutzer über Punkt-zu-Standort VPN verbunden. Dies wird davon ausgegangen, dass der Benutzer berechtigt ist, diesen virtuellen Computern erreicht haben.

Punkt-zu-Standort VPN ist sicherer als direkte RDP oder SSH Verbindungen, da der Benutzer zum Authentifizieren zweimal vor dem Herstellen einer Verbindung mit einem virtuellen Computer hat. Zunächst muss der Benutzer authentifiziert (und autorisiert werden) zum Herstellen der Verbindung des Punkt-zu-Standort VPN; zweites, muss der Benutzer authentifiziert (und autorisiert werden), die RDP oder SSH-Sitzung herzustellen.

Ein [Standort-zu-Standort VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md) verbindet ein gesamtes Netzwerk zu einem anderen Netzwerk über das Internet an. Ein Standort-zu-Standort VPN können Sie Ihrem lokalen Netzwerk mit einem Azure-virtuellen-Netzwerk verbunden. Wenn Sie in einem Website-zu-Standort VPN Benutzer bereitstellen Ihres Netzwerks für lokale Verbindung mit virtuellen Computern in Ihrem Azure-virtuellen Netzwerk über das Protokoll RDP oder SSH über die Standort-zu-Standort VPN-Verbindung können, und fordert Sie RDP oder SSH Direktzugriff über das Internet dürfen nicht.

Sie können auch eine dedizierte WAN-Verbindung verwenden, mit dem Standort-zu-Standort VPN ähnliche Funktionalität bereitstellen. Die wichtigsten Unterschiede sind 1. dedizierte WAN-Verbindung durchlaufen nicht im Internet und 2. dedizierte WAN Links sind in der Regel Weitere Stabilität und leistungsfähige. Azure bietet eine dedizierte WAN-Verbindung-Lösung in Form eines [ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/).

## <a name="enable-azure-security-center"></a>Azure-Sicherheitscenter aktivieren
Azure-Sicherheitscenter hilft Ihnen zu verhindern, erkennen und Beantworten von Risiken und stellt, dass Sie die Transparenz, und steuern, die Sicherheit Ihrer Azure Ressourcen erhöht. Es bietet integrierte Sicherheit Überwachung und Policy Management über Ihre Azure-Abonnements, hilft Angriffen, die andernfalls aufgefallen und funktioniert mit einem ausgedehnten System von Lösungen Sicherheit erkennen.

Azure-Sicherheitscenter unterstützt Sie beim Optimieren und überwachen Netzwerk Sicherheit durch:

- Bereitstellen von Netzwerk Sicherheit Empfehlungen
- Überwachen des Status der Netzwerkkonfiguration Sicherheit
- Warnung bei Network-basierten Risiken sowohl auf den Endpunkt und Netzwerk Ebenen

Es wird dringend empfohlen, dass Sie für alle Ihre Azure-Bereitstellungen Azure-Sicherheitscenter aktivieren.

Wenn Sie weitere Informationen zur Azure-Sicherheitscenter und wie Sie es für die Bereitstellung Ihrer aktivieren, lesen Sie im Artikel [Einführung Azure Sicherheitscenter](../security-center/security-center-intro.md).

## <a name="securely-extend-your-datacenter-into-azure"></a>Sichere Datencenters in Azure erweitern
Viele Enterprise IT Organisationen zum Erweitern Sie in der Cloud ihren lokalen Rechenzentren wachsende gefunden werden. Diese Erweiterung stellt eine Erweiterung der vorhandenen IT-Infrastruktur in der öffentlichen Cloud. Durch die Nutzung von Cross lokale Verbindungsoptionen ist es möglich, Ihre Azure-virtuellen Netzwerke als nur ein anderes Subnetz auf Ihrem lokalen Netzwerk-Infrastruktur zu behandeln.

Es gibt jedoch zahlreiche Planung und Design Probleme, die zuerst berücksichtigt werden müssen. Dies ist besonders wichtig im Bereich der Netzwerk Sicherheit. Eine der bewährte Methoden zu verstehen, wie Sie ein Design Ansatz ist ein Beispiel zu sehen.

Microsoft hat erstellt, die [Erweiterung Bezug Architektur Datencenter](https://gallery.technet.microsoft.com/Datacenter-extension-687b1d84#content) und unterstützende Materialien, damit Sie verstehen, wie diese eine Datacenter Verlängerung aussehen würde. Dadurch wird eine Beispiel Bezug Implementierung, die Sie zum Planen und Entwerfen einer secure Enterprise Datacenter-Erweiterung in der Cloud verwenden können. Es empfiehlt sich, dass Sie dieses Dokument um ein Überblick über die wichtigsten Komponenten von einer sicheren Lösung verschaffen überprüfen.

Weitere Informationen zum sicheren Datencenters in Azure erweitern finden Sie in der video [Erweitern Ihrer Datacenter in Microsoft Azure](https://www.youtube.com/watch?v=Th1oQQCb2KA).

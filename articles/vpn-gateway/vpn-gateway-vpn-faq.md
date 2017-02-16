<properties 
   pageTitle="Virtuelle Netzwerk VPN Gateway häufig gestellte Fragen zu | Microsoft Azure"
   description="Das VPN-Gateway häufig gestellte Fragen. Häufig gestellte Fragen zur Microsoft Azure-virtuellen Netzwerk Cross lokale Verbindungen, Hybrid Konfiguration Verbindungen und VPN-Gateways"
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor="" />
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/10/2016"
   ms.author="yushwang" />

# <a name="vpn-gateway-faq"></a>VPN-Gateway häufig gestellte Fragen

## <a name="connecting-to-virtual-networks"></a>Herstellen einer Verbindung mit virtuelle Netzwerke

### <a name="can-i-connect-virtual-networks-in-different-azure-regions"></a>Kann ich eine virtuelle Netzwerke in unterschiedlichen Azure Regionen herstellen?
Ja. Es gibt sogar keine Region-Einschränkung. Ein virtuelles Netzwerk kann mit einem anderen virtuellen Netzwerk in der gleichen Region oder in ein anderes Azure Region verbinden.

### <a name="can-i-connect-virtual-networks-in-different-subscriptions"></a>Kann virtuelle Netzwerke in anderen Abonnements Verbindung herstellen?
Ja.

### <a name="can-i-connect-to-multiple-sites-from-a-single-virtual-network"></a>Kann ich eine Verbindung zu mehreren Websites aus einem einzelnen virtuellen Netzwerk herstellen?

Sie können mit mehreren Websites herstellen, mithilfe von Windows PowerShell und der REST Azure-APIs. Finden Sie im Abschnitt [mit mehreren Standorten und VNet-VNet - Konnektivität](#multi-site-and-vnet-to-vnet-connectivity) häufig gestellte Fragen.
## <a name="what-are-my-cross-premises-connection-options"></a>Was sind meine Cross lokale Verbindungsoptionen?

Die folgenden Cross lokale Verbindungen werden unterstützt:

- [Website-zu-Standort](vpn-gateway-site-to-site-create.md) – VPN-Verbindung über IPsec (IKE Version 1 und 2 IKE). Diese Art von Verbindung erfordert ein VPN-Gerät oder RRAS.

- [Punkt-zu-Standort](vpn-gateway-point-to-site-create.md) – VPN-Verbindung über SSTP (Secure Sockets Tunnel Protocol). Diese Verbindung ist ein VPN-Gerät nicht erforderlich.

- [VNet-zu-VNet](virtual-networks-configure-vnet-to-vnet-connection.md) – dieser Art von Verbindung ist eine Website-zu-Standort-Konfiguration identisch. VNet zu VNet ist eine VPN-Verbindung über IPsec (IKE Version 1 und 2 IKE) an. Es sind kein Gerät VPN erforderlich.

- [Website mit mehreren](vpn-gateway-multi-site.md) – Dies ist eine Variation einer Standort-zu-Standort-Konfiguration, die Sie mehrere lokale Websites zu einem virtuellen Netzwerk herstellen kann.

- [ExpressRoute](../expressroute/expressroute-introduction.md) – ExpressRoute ist eine direkte Verbindung mit Azure aus Ihrem WAN, nicht über das öffentliche Internet. Finden Sie unter der [ExpressRoute – technische Übersicht](../expressroute/expressroute-introduction.md) und die [ExpressRoute häufig gestellte Fragen](../expressroute/expressroute-faqs.md) für Weitere Informationen.

Weitere Informationen zu Verbindungen finden Sie unter [Informationen zum VPN-Gateway](vpn-gateway-about-vpngateways.md).

### <a name="what-is-the-difference-between-a-site-to-site-connection-and-point-to-site"></a>Was ist der Unterschied zwischen einer Verbindung zwischen Standorten und Punkt-zu-Standort?

Verbindungen **Zwischen Standorten** ermöglichen Ihnen die verbinden keines dieser Computer auf Ihre lokale jeder virtuellen Computern und die Instanz der Rolle innerhalb des virtuellen Netzwerks, je nachdem, wie Sie wählen routing konfigurieren. Es ist eine gute Option für eine Verbindung immer verfügbar Cross lokale und eignet sich gut für möchten. Diese Art von Verbindung basiert auf einer IPsec VPN-Anwendung (Hardware oder weiche Einheit), die am Rand des Netzwerks bereitgestellt werden muss. Wenn Sie diese Art von Verbindung erstellen zu können, müssen Sie die erforderlichen VPN Hardware und eine extern ausgerichteten IPv4-Adresse verfügen.

**Punkt-zu-Standort** ermöglichen die aus einem einzelnen Computer, von überall eine Verbindung Verbindungen auf einen anderen Wert befindet sich in Ihrem virtuelle Netzwerk. Es wird den Windows-Box-VPN-Client verwendet. Als Teil der Punkt-zu-Standort-Konfiguration installieren Sie ein Zertifikat und ein VPN-Client-Konfigurationspaket, die Einstellungen enthält, mit denen Ihren Computer für die Verbindung zu einem beliebigen virtuellen Computern oder Rolleninstanz innerhalb des virtuellen Netzwerks ein. Es ist hervorragend, wenn Sie ein virtuelles Netzwerk eine Verbindung herstellen möchten, aber am lokalen nicht zur Verfügung. Es ist auch eine gute Wahl, wenn Sie keine Zugriff auf VPN Hardware oder eine extern zugänglichen IPv4-Adresse, die für eine Verbindung zwischen Standorten erforderlich sind. 

Sie können Ihre virtuelle Netzwerk sowohl zwischen Standorten und Punkt-zu-Standort gleichzeitig, verwenden konfigurieren, vorausgesetzt, dass Sie die Website-zu-Standort-Verbindung mit einer Routing-basierten VPN-Typ für Ihr Gateway erstellen. Routing-basierte VPN-Typen werden dynamische Gateways im Bereitstellungsmodell klassischen bezeichnet.

### <a name="what-is-expressroute"></a>Was ist ExpressRoute?

ExpressRoute ermöglicht die Erstellung private Verbindungen zwischen Microsoft-Rechenzentren und Infrastruktur, die Ihre lokal oder in einer Umgebung für die gemeinsame Speicherort ist. Mit ExpressRoute können Sie herstellen von Verbindungen mit Microsoft-Cloud-Diensten wie Microsoft Azure und Office 365 in einer ExpressRoute Partner gemeinsame Speicherort Einrichtung oder direkt aus dem vorhandenen WAN-Netzwerk (beispielsweise eine MPLS VPN von einem Netzwerk-Dienstanbieter bereitgestellte) herstellen.

ExpressRoute Verbindungen bieten eine höhere Sicherheit, mehr Zuverlässigkeit, höhere Bandbreite und unteren Wartezeiten als normalen Verbindungen über das Internet an. In einigen Fällen kann mithilfe von ExpressRoute-Verbindungen zum Übertragen von Daten zwischen Ihrem lokalen Netzwerk und Azure auch signifikante Kostenvorteile erzielt werden. Wenn Sie eine Verbindung Cross lokale aus Ihrem lokalen Netzwerk in Azure bereits erstellt haben, können Sie auf eine Verbindung ExpressRoute Beibehaltung Netzwerks virtuelle intakt migrieren.

Finden Sie weitere Details der [ExpressRoute häufig gestellte Fragen](../expressroute/expressroute-faqs.md) .

## <a name="site-to-site-connections-and-vpn-devices"></a>Website-zu-Standort-Verbindungen und VPN-Geräte

### <a name="what-should-i-consider-when-selecting-a-vpn-device"></a>Was sollte ich berücksichtigen Sie beim Auswählen eines VPN-Gerät?

Wir haben eine Reihe von standard-Standorten VPN-Geräten in Zusammenarbeit mit Gerätehersteller überprüft. Eine Liste der bekannten kompatiblen VPN-Geräten, deren entsprechenden Konfiguration Anweisungen oder Beispiele und Gerät Spezifikationen finden Sie [hier](vpn-gateway-about-vpn-devices.md). Alle Geräte in aufgeführten als bekannte kompatible Gerät Familien sollte virtuelles Netzwerk konzipiert. Wenn Sie Ihr Gerät VPN konfigurieren, finden Sie in das Gerät Konfiguration Stichprobe oder die Verknüpfung, die entsprechenden Gerätefamilie entspricht.

### <a name="what-do-i-do-if-i-have-a-vpn-device-that-isnt-in-the-known-compatible-device-list"></a>Was mache ich, wenn ich ein VPN-Gerät verfügen, die nicht in der Liste der bekannten kompatibel ist?

Wenn Ihr Gerät als bekannte kompatiblen VPN-Gerät aufgeführt wird nicht angezeigt, und es für die VPN-Verbindung verwendet werden soll, müssen Sie überprüfen, ob sie die unterstützten IPSec-/IKE Konfiguration Optionen und Parameter aufgelisteten [hier](vpn-gateway-about-vpn-devices.md#devices-not-on-the-compatible-list)erfüllt. Gut mit VPN-Gateways sollte die Mindestanforderungen Besprechung Geräte funktionieren. Wenden Sie sich an Ihrem Gerätehersteller Weitere Anweisungen für Support und Konfiguration.

### <a name="why-does-my-policy-based-vpn-tunnel-go-down-when-traffic-is-idle"></a>Warum wechseln mein Policy-basierte VPN-Tunnel nach unten, wenn Datenverkehr im Leerlauf ist?

Dies ist die erwartete Verhalten für Policy-basierte (auch bekannt als statisches routing) VPN-Gateways. Wenn der Datenverkehr über den Tunnel für mehr als 5 Minuten im Leerlauf befindet, wird der Tunnel abgebrochen werden. Beim Starten von Datenverkehr in Richtung parallelen wird der Tunnel sofort wiederhergestellt.

### <a name="can-i-use-software-vpns-to-connect-to-azure"></a>Kann ich die Software VPN Verbindung zum Azure verwenden?

Wir unterstützen Windows Server 2012-Routing und RAS (RRAS) Servern für Standort-zu-Standort Cross lokale Konfiguration.

Andere Software VPN Lösungen sollten mit unserer Gateway arbeiten, solange sie Industry standard IPSec-Implementierungen entsprechen. Wenden Sie sich an den Hersteller der Software, Konfiguration und Support-Anweisungen.

## <a name="point-to-site-connections"></a>Punkt-zu-Standort-Verbindungen

### <a name="what-operating-systems-can-i-use-with-point-to-site"></a>Welche Betriebssysteme kann ich mit Punkt-zu-Standort verwenden?

Die folgenden Betriebssysteme werden unterstützt:

- Windows 7 (32-Bit- und 64-Bit)

- Windows Server 2008 R2 (nur 64-Bit)

- Windows 8 (32-Bit- und 64-Bit)

- Windows 8.1 (32-Bit- und 64-Bit)

- WindowsServer 2012 (nur 64-Bit)

- Windows Server 2012 R2 (nur 64-Bit)

- Windows-10

### <a name="can-i-use-any-software-vpn-client-for-point-to-site-that-supports-sstp"></a>Kann ich alle Software VPN-Client für Punkt-zu-Standort verwenden, die SSTP unterstützt?

Nein. Support ist nur für die oben aufgeführten Betriebssystem-Versionen von Windows beschränkt.

### <a name="how-many-vpn-client-endpoints-can-i-have-in-my-point-to-site-configuration"></a>Wie viele VPN-Clientendpunkte kann ich in meinem Punkt-zu-Standort-Konfiguration verwenden?

Wir unterstützen bis zu 128 VPN-Clients, um die Verbindung zu einem virtuellen Netzwerk zur gleichen Zeit hergestellt werden.

### <a name="can-i-use-my-own-internal-pki-root-ca-for-point-to-site-connectivity"></a>Kann ich meinen eigenen internen PKI Stammzertifizierungsstelle für Punkt-zu-Standort-Konnektivität verwenden?

Ja. In früheren Versionen konnte nur selbstsignierten Stammzertifikate verwendet werden. Sie können weiterhin 20 Stammzertifikate hochladen.

### <a name="can-i-traverse-proxies-and-firewalls-using-point-to-site-capability"></a>Können Proxys und Firewalls mithilfe der Funktion Punkt-zu-Standort durchlaufen?

Ja. Wir verwenden SSTP (Secure Sockets Tunnel Protocol), um Tunnel über Firewalls. Diesen Tunnel wird als eine HTTPs-Verbindung angezeigt.

### <a name="if-i-restart-a-client-computer-configured-for-point-to-site-will-the-vpn-automatically-reconnect"></a>Wenn ich einen Clientcomputer konfiguriert für Punkt-zu-Standort neu zu starten, wird das Option VPN automatisch erneut verbunden?

Standardmäßig wird der Clientcomputer die VPN-Verbindung nicht automatisch wiederherzustellen.

### <a name="does-point-to-site-support-auto-reconnect-and-ddns-on-the-vpn-clients"></a>Ist Support Punkt-zu-Standort automatisch verbinden, und klicken Sie auf den VPN-Clients DDNS?

Automatisch verbinden und DDNS werden in Punkt-zu-Standort VPN derzeit nicht unterstützt.

### <a name="can-i-have-site-to-site-and-point-to-site-configurations-coexist-for-the-same-virtual-network"></a>Kann zwischen Standorten haben und werden Punkt-zu-Standort Konfigurationen gleichzeitig verwendet, für das gleiche virtuelle Netzwerk?

Ja. Beide folgenden Lösungsvorschlägen funktionieren, wenn Sie ein anderes RouteBased VPN für Ihr Gateway haben. Für das Bereitstellungsmodell klassischen benötigen Sie einen dynamischen Gateway ein. Wir kann nicht Punkt-zu-Supportwebsite für statische Weiterleitung VPN-Gateways oder Gateways - VpnType Richtlinien verwenden.

### <a name="can-i-configure-a-point-to-site-client-to-connect-to-multiple-virtual-networks-at-the-same-time"></a>Kann ich gleichzeitig eine Verbindung mit mehreren virtuellen Netzwerken einen Punkt-zu-Standort-Client konfigurieren?

Ja, ist es möglich. Aber die virtuellen Netzwerke keine überlappende IP-Präfixe und die Punkt-zu-Standort Adresse Leerzeichen dürfen nicht zwischen den virtuellen Netzwerken überlappen.

### <a name="how-much-throughput-can-i-expect-through-site-to-site-or-point-to-site-connections"></a>Wie viel Durchsatz können bis zu Standorten oder Punkt-zu-Standort-Verbindungen zu erwarten?

Es ist schwer zu den genauen Durchsatz, der die VPN-Tunnel verwalten. IPsec und SSTP sind kryptomobilität überladene VPN-Protokolle. Durchsatz ist auch die Wartezeit und die Bandbreite zwischen Ihrem lokale und im Internet zugänglich.

## <a name="gateways"></a>Gateways

### <a name="what-is-a-policy-based-static-routing-gateway"></a>Was ist ein Gateway Policy-basierte (statische routing)?

Policy-basierte Gateways implementieren Richtlinie-basierten VPN. Richtlinie-basierten VPN verschlüsseln und Pakete durch IPsec Tunnel auf Grundlage der Kombinationen aus Adresspräfixe zwischen Ihrem lokalen Netzwerk und die Azure VNet direkte. Die Richtlinie (oder den Datenverkehr Ansichtsauswahl) wird in der Regel als Access-Liste in die VPN-Konfiguration definiert.

### <a name="what-is-a-route-based-dynamic-routing-gateway"></a>Was ist ein Gateway Routing-basierte (Dynamic routing)?

Routing-basierten Gateways implementieren die Routing-basierten VPN. Routing-basierten VPN verwenden "leitet" in die IP-Adresse weiterleiten oder routing-Tabelle in die entsprechenden Tunnelschnittstellen für die direkte Paketen. Die Tunnelschnittstellen dann verschlüsseln oder die Pakete ein-und die Tunnel entschlüsseln. Die Richtlinie oder Datenverkehr Auswahl für Routing basierend VPN als any-to-any konfiguriert sind (oder Platzhaltern).

### <a name="can-i-get-my-vpn-gateway-ip-address-before-i-create-it"></a>Erhalte ich meine VPN Gateway IP Address, bevor ich es erstellen?

Nein. Sie müssen Ihr Gateway zuerst, um die IP-Adresse erhalten erstellen. Ändert sich die IP-Adresse, wenn Sie löschen und neu Ihrer VPN-Gateway erstellen.

### <a name="how-does-my-vpn-tunnel-get-authenticated"></a>Wie authentifiziert Meine VPN-Tunnel erhalten?

Azure VPN verwendet Authentifizierung PSK (vorinstallierter Schlüssel). Wir generieren einen vorinstallierten Schlüssel (PSK), wenn wir den VPN-Tunnel erstellen. Sie können die automatisch generierte PSK eigene mit dem Festlegen vorinstallierter Schlüssel PowerShell-Cmdlet oder REST-API ändern.

### <a name="can-i-use-the-set-pre-shared-key-api-to-configure-my-policy-based-static-routing-gateway-vpn"></a>Kann ich mithilfe der festlegen vorinstallierter Schlüssel-API Meine Policy-basierte konfigurieren (statisches routing) Gateway VPN?

Ja, kann das Festlegen vorinstallierter Schlüssel API und PowerShell-Cmdlet Azure Policy-basierte (statisch) VPN und Routing-basierten (dynamic) routing VPN konfigurieren verwendet werden.

### <a name="can-i-use-other-authentication-options"></a>Können andere Authentifizierungsoptionen?

Wir sind bei der Verwendung von vorinstallierter Schlüssel (PSK) für die Authentifizierung beschränkt.

### <a name="what-is-the-gateway-subnet-and-why-is-it-needed"></a>Was ist die "Gateway Subnetz", und warum ist es erforderlich?

Wir haben einen Gatewaydienst, den wir ausgeführt werden, um Cross lokale Konnektivität zu aktivieren. 

Sie müssen ein Gateway-Subnetz für Ihre VNet so konfigurieren Sie einen VPN-Gateway zu erstellen. Alle Gateway Subnetze müssen den Namen GatewaySubnet ordnungsgemäß funktioniert. Benennen Sie nicht Ihrer Gateway Subnetz etwas anderes. Und nicht mit dem Gateway Subnetz virtuellen Computern oder Sonstiges bereitstellen.

Die Gateway Subnetz Mindestgröße hängt vollständig die Konfiguration, die Sie erstellen möchten. Obwohl es möglich, erstellen Sie ein Gateway-Subnetz /29 für einige Konfigurationen möglichst klein ist, es empfiehlt sich, dass Sie ein Gateway Subnetz /28 oder größere erstellen (/ 28, /27, /26 usw..). 

### <a name="can-i-deploy-virtual-machines-or-role-instances-to-my-gateway-subnet"></a>Kann ich virtuellen Computern oder Rolleninstanzen mit meinem Gateway Subnetz bereitstellen?

Nein.

### <a name="how-do-i-specify-which-traffic-goes-through-the-vpn-gateway"></a>Wie gebe ich an der Datenverkehr durchläuft VPN-Gateway?

Wenn Sie das klassische Azure-Portal verwenden, fügen Sie jeder Bereich über das Gateway für das virtuelle Netzwerk auf der Seite Netzwerke, klicken Sie unter lokale Netzwerke gesendet.

### <a name="can-i-configure-forced-tunneling"></a>Kann ich Erzwungene Tunnel konfigurieren?

Ja. Finden Sie unter [Konfigurieren Erzwungene Tunnel](vpn-gateway-about-forced-tunneling.md).

### <a name="can-i-set-up-my-own-vpn-server-in-azure-and-use-it-to-connect-to-my-on-premises-network"></a>Kann ich eigene VPN-Server in Azure einrichten und verwenden, um eine Verbindung mit meinem lokalen Netzwerk?

Ja, können Sie Ihre eigenen VPN-Gateways oder auf Servern in Azure aus dem Azure Marketplace oder erstellen Ihre eigenen VPN Router bereitstellen. Sie müssen so konfigurieren Sie benutzerdefinierte leitet in Ihrem Netzwerk virtuelle, um sicherzustellen, dass Datenverkehr zwischen Ihrem lokalen Netzwerken und Ihre virtuelle Netzwerksubnets ordnungsgemäß weitergeleitet wird.

### <a name="why-are-certain-ports-opened-on-my-vpn-gateway"></a>Warum sind bestimmte Ports auf Meine VPN Gateway geöffnet?

Sie sind für die Kommunikation Azure Infrastruktur erforderlich. Sie sind geschützt (gesperrt) durch Azure Zertifikate Ohne korrekten Zertifikate werden externe Benutzer, einschließlich der diese Gateways, Kunden nicht Einfluss auf die Endpunkte verursachen können.

Ein VPN-Gateway ist im Grunde ein mehrfach vernetzten Gerät mit einer NIC tippen in der Kunden privates Netzwerk und eine NIC gegenüberliegende im öffentliche Netzwerk. Azure Infrastruktur Personen können nicht in Kunden private Netzwerke aus Gründen der Compliance Tippen Sie auf, damit die benötigten öffentlichen Endpunkte für die Kommunikation Infrastruktur nutzen. Die öffentliche Endpunkte werden regelmäßig von Azure Sicherheit Audit gescannt werden.


### <a name="more-information-about-gateway-types-requirements-and-throughput"></a>Weitere Informationen zu den Typen der Gateways, Anforderungen und Durchsatz

Weitere Informationen finden Sie unter [Informationen zum VPN Gateway Settings](vpn-gateway-about-vpn-gateway-settings.md).

## <a name="multi-site-and-vnet-to-vnet-connectivity"></a>Website mit mehreren und VNet-VNet-Konnektivität

### <a name="which-type-of-gateways-can-support-multi-site-and-vnet-to-vnet-connectivity"></a>Die Art des Gateways kann mit mehreren Standorten und VNet-VNet-Konnektivität unterstützen?

Nur Routing-basierten VPN (dynamisches routing).

### <a name="can-i-connect-a-vnet-with-a-routebased-vpn-type-to-another-vnet-with-a-policybased-vpn-type"></a>Kann ich eine VNet mit einem RouteBased VPN-Typ zu einem anderen VNet mit einem Typ PolicyBased VPN verbinden?

Nein, beide virtuelle Netzwerke müssen Routing-basierte (dynamisches routing) verwenden VPN.

### <a name="is-the-vnet-to-vnet-traffic-secure"></a>Ist der VNet-VNet-Datenverkehr sicher?

Ja, ist es durch Verschlüsselung IPSec-/IKE geschützt.

### <a name="does-vnet-to-vnet-traffic-travel-over-the-azure-backbone"></a>Durchläuft VNet-VNet-Verkehr über dem Azure Backbone zwar?

Ja.

### <a name="how-many-on-premises-sites-and-virtual-networks-can-one-virtual-network-connect-to"></a>Wie viele lokale Websites und virtuelle Netzwerke kann eine virtuelles Netzwerk eine Verbindung herstellen?

Max. 10 kombiniert für die Gateways Basic- und Standard dynamisches Routing; 30 für die hohe Leistung VPN-Gateways.

### <a name="can-i-use-point-to-site-vpns-with-my-virtual-network-with-multiple-vpn-tunnels"></a>Kann ich verwenden Punkt-zu-Standort VPN mit meinem virtuelles Netzwerk mit mehreren VPN-Tunnel?

Ja, können die Punkt-zu-Standort (P2S) VPN mit der Herstellen einer Verbindung mit mehreren lokalen Websites und andere virtuelle Netzwerke VPN-Gateways verwendet werden.

### <a name="can-i-configure-multiple-tunnels-between-my-virtual-network-and-my-on-premises-site-using-multi-site-vpn"></a>Kann ich mehrere Tunnel zwischen meinem virtuelle Netzwerk und meinem lokalen Website mit mehreren Standorten VPN konfigurieren?

Nein, redundante Tunnel zwischen einem Azure virtuelle Netzwerk und einer lokalen Website werden nicht unterstützt.

### <a name="can-there-be-overlapping-address-spaces-among-the-connected-virtual-networks-and-on-premises-local-sites"></a>Werden Adresse Leerzeichen zwischen den verbundenen virtuelle Netzwerke und lokale lokale Websites kann es überlappende?

Nein. Überlappende Adressbereiche wird das Netzwerk Konfiguration Datei hochladen oder "Erstellen von virtuellen Netzwerk" zum Fehlschlagen verursachen.

### <a name="do-i-get-more-bandwidth-with-more-site-to-site-vpns-than-for-a-single-virtual-network"></a>Erhalte ich weitere Bandbreite mit Weitere Standort-zu-Standort VPN als für ein einzelnes virtuelles Netzwerk?

Nein, alle VPN-Tunnel, einschließlich Punkt-zu-Standort VPN, freigeben, das gleiche Azure VPN Gateway und der verfügbaren Bandbreite.

### <a name="can-i-use-azure-vpn-gateway-to-transit-traffic-between-my-on-premises-sites-or-to-another-virtual-network"></a>Können Azure VPN-Gateway werden verwendet, den Datenverkehr zwischen verschiedenen Websites meinem lokalen oder in ein anderes virtuelle Netzwerk anzuwenden ist?

**Klassische Bereitstellungsmodell**<br>
Während der Übertragung Verkehr über Azure VPN-Gateway mithilfe des Modells klassischen Bereitstellung möglich ist, aber statisch definierte Adresse Leerzeichen in der Konfigurationsdatei Netzwerk abhängig. BGP wird mit Azure virtuelle Netzwerke und VPN-Gateways mithilfe des Modells klassischen Bereitstellung noch nicht unterstützt. Ohne BGP wird sehr Fehler häufig und nicht empfohlen Manuelles Definieren Übertragung Adresse Leerzeichen.<br>
**Modell zur Bereitstellung von Ressourcenmanager**<br>
Wenn Sie das Modell zur Bereitstellung von Ressourcenmanager verwenden, finden Sie im Abschnitt [BGP](#bgp) für Weitere Informationen.

### <a name="does-azure-generate-the-same-ipsecike-pre-shared-key-for-all-my-vpn-connections-for-the-same-virtual-network"></a>Erzeugt Azure demselben IPSec-/IKE vorinstallierten Schlüssel für alle VPN-Verbindungen für das gleiche virtuelle Netzwerk?

Nein, generiert Azure standardmäßig verschiedene vorinstallierten Schlüssel für verschiedene VPN-Verbindungen. Das VPN-Gateway-Taste REST-API festlegen oder PowerShell-Cmdlet können Sie jedoch den Schlüsselwert festlegen, in den Sie arbeiten. Der Schlüssel muss alphanumerische Zeichenfolge der Länge zwischen 1 und 128 Zeichen lang sein.

### <a name="does-azure-charge-for-traffic-between-virtual-networks"></a>Gestellt Azure wird für den Datenverkehr zwischen virtuelle Netzwerke Rechnung?

Für den Datenverkehr zwischen verschiedenen Azure virtuelle Netzwerke Gebühren Azure nur für den Datenverkehr also von einem Azure Region in ein anderes aus. Die Gebühr Rate wird auf der Seite Azure [VPN Gateway Preise](https://azure.microsoft.com/pricing/details/vpn-gateway/) aufgeführt.


### <a name="can-i-connect-a-virtual-network-with-ipsec-vpns-to-my-expressroute-circuit"></a>Kann ich mit meinem ExpressRoute Verbindung ein virtuelles Netzwerk mit IPsec VPN verbinden?

Ja, wird dies unterstützt. Weitere Informationen finden Sie unter [Konfigurieren von ExpressRoute und Standort-zu-Standort VPN-Verbindungen, die gleichzeitig verwendet werden](../expressroute/expressroute-howto-coexist-classic.md).

## <a name="a-namebgpabgp"></a><a name="bgp"></a>BGP

[AZURE.INCLUDE [vpn-gateway-bgp-faq-include](../../includes/vpn-gateway-bpg-faq-include.md)] 



## <a name="cross-premises-connectivity-and-vms"></a>Cross lokale Konnektivität und virtuellen Computern

### <a name="if-my-virtual-machine-is-in-a-virtual-network-and-i-have-a-cross-premises-connection-how-should-i-connect-to-the-vm"></a>Wenn meine virtuellen Computern ein virtuelles Netzwerk ist, und ich eine Verbindung Cross lokale habe, wie sollte ich mit dem virtuellen Computer verbinden?

Es gibt einige Optionen. Wenn Sie RDP aktiviert haben, und Sie einen Endpunkt erstellt haben, können Sie Ihre virtuellen Computern mithilfe der VIP verbinden. In diesem Fall würden Sie angeben der VIP und den Port, dem Sie in eine Verbindung herstellen möchten. Sie müssen den Port auf Ihre virtuellen Computern für den Datenverkehr zu konfigurieren. Normalerweise würden Sie klassische Azure-Portal wechseln und speichern die Einstellungen für die Verbindung RDP auf Ihrem Computer. Die Einstellungen enthalten die erforderlichen Verbindungsinformationen.

Wenn Sie ein virtuelles Netzwerk mit Cross lokale Verbindung konfiguriert haben, können Sie Ihre virtuellen Computern mithilfe des internen DIP oder private IP-Adresse verbinden. Sie können auch mit Ihrer virtuellen Computern durch internen DIP aus einem anderen virtuellen Computern herstellen, die auf der gleichen virtuellen Netzwerk befindet. Sie können nicht RDP zum virtuellen Computer mithilfe von DIP aus, wenn Sie über eine Position außerhalb des virtuellen Netzwerks verbinden. Beispielsweise können nicht, wenn Sie ein virtuelles Punkt-zu-Standort-Netzwerk konfiguriert haben, und Sie keine Verbindung, von Ihrem Computer herstellen, Sie virtuellen Computer nach DIP verbinden.

### <a name="if-my-virtual-machine-is-in-a-virtual-network-with-cross-premises-connectivity-does-all-the-traffic-from-my-vm-go-through-that-connection"></a>Ist meine virtuellen Computern in einem virtuellen Netzwerk mit Cross lokale Konnektivität, geht die gesamten Verkehr aus meiner virtuellen Computer über diese Verbindung?

Nein. Nur den Datenverkehr, der ein Ziel werden IP, die in die virtuelle Netzwerk lokale Netzwerk IP-Adressbereiche enthalten ist, die Sie angegeben über das Gateway virtuelles Netzwerk geleitet. Datenverkehr hat das Ziel, die innerhalb des virtuellen Netzwerks ansässig IP innerhalb des virtuellen Netzwerks bleibt. Anderer Verkehr über den Lastenausgleich für öffentliche Netzwerke gesendet, oder wenn erzwungenen Tunnel verwendet wird, über das Gateway Azure VPN gesendet. Wenn Sie Probleme haben, ist es wichtig, um sicherzustellen, dass Sie alle in Ihrem lokalen Netzwerk, die Sie über das Gateway senden möchten aufgeführten Bereiche haben. Stellen Sie sicher, dass die lokale Netzwerk-Adressbereiche mithilfe einer der Adressbereiche in das virtuelle Netzwerk nicht überschneiden. Sie möchten auch, stellen Sie sicher, dass der DNS-Server, die, den Sie verwenden, den Namen in die korrekte IP-Adresse aufgelöst wird.


## <a name="virtual-network-faq"></a>Virtuelle Netzwerk häufig gestellte Fragen

Sie anzeigen zusätzliche virtuelle Netzwerkinformationen, in die [Virtuelle Netzwerk häufig gestellte Fragen](../virtual-network/virtual-networks-faq.md).
 

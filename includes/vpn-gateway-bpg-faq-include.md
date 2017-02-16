### <a name="is-bgp-supported-on-all-azure-vpn-gateway-skus"></a>Werden BGP wird auf alle Azure VPN Gateway SKUs unterstützt?

Nein, BGP auf Azure **Standard-** und **HighPerformance** VPN-Gateways unterstützt. **Grundlegende** SKU wird nicht unterstützt.

### <a name="can-i-use-bgp-with-azure-policy-based-vpn-gateways"></a>Kann ich mit Azure Policy-Based VPN-Gateways BGP verwenden?

Nein, BGP auf nur Routing-basierten VPN-Gateways unterstützt.

### <a name="can-i-use-private-asns-autonomous-system-numbers"></a>Kann ich private ASNs (eigenständigen System Zahlen) werden verwendet?

Ja, können Sie eigene öffentliche ASNs oder private ASNs für Ihre lokale Netzwerke und Azure virtuelle Netzwerke verwenden.

#### <a name="are-there-asns-reserved-by-azure"></a>Gibt es ASNs von Azure reserviert?

Ja, sind die folgenden ASNs Azure für internen und externen Peerings reserviert:

- Öffentliche ASNs: 8075, 8076, 12076
- Privat ASNs: 65515, 65517, 65518, 65519, 65520

Sie können keine diese ASNs für Ihre auf lokale Geräte VPN angeben, bei der Verbindung mit Azure VPN-Gateways.

### <a name="can-i-use-the-same-asn-for-both-on-premises-vpn-networks-and-azure-vnets"></a>Kann ich das gleiche ASN für lokale VPN-Netzwerke und Azure VNets verwenden?

Nein, müssen Sie verschiedene ASNs zwischen Ihrem lokalen Netzwerken und Ihre Azure VNets zuweisen, wenn Sie diese zusammen mit BGP verbinden. Azure VPN-Gateways verfügen über den Standardwert ASN der 65515 zugewiesen, ob BGP für für Ihre Cross lokale Verbindung nicht aktiviert ist. Sie können diese Standardeinstellung außer Kraft setzen, durch eine andere ASN zuweisen, beim Erstellen des Gateways VPN oder das ASN ändern sich nach das Gateway erstellt wird. Sie benötigen die entsprechende Azure lokales Netzwerkgateways Ihrem lokalen ASNs zuweisen.

### <a name="what-address-prefixes-will-azure-vpn-gateways-advertise-to-me"></a>Welche Adresspräfixe werden Azure VPN-Gateways an mich ankündigen?

Azure VPN-Gateway wird die folgenden leitet auf lokale Geräte BGP ankündigen:

- Ihre VNet Adresspräfixe
- Adresspräfixe für jede lokale Netzwerkgateways verbunden Azure VPN-Gateway
- Leitet Kenntnisse aus anderen Peeringliste BGP-Sitzungen Azure VPN-Gateway, **mit Ausnahme der standardmäßigen oder Strecken überlappt mit einem beliebigen VNet Präfix**verbunden.

#### <a name="can-i-advertise-default-route-00000-to-azure-vpn-gateways"></a>Kann ich die standardmäßigen Routing (0.0.0.0/0) zu Azure VPN-Gateways ankündigen?

Nicht zu diesem Zeitpunkt.

#### <a name="can-i-advertise-the-exact-prefixes-as-my-virtual-network-prefixes"></a>Können die genauen Präfixe als meine Präfixe virtuelles Netzwerk werden ankündigen?

Nein, die gleichen Präfixe als eine des virtuellen Netzwerks Werbung Adresspräfixe blockiert oder werden der Azure-Plattform gefiltert. Sie können jedoch ein Präfix ankündigen, die was Sie innerhalb des virtuellen Netzwerks haben übergeordnet ist. Beispielsweise könnten virtuelle Netzwerk die Adresse Leerzeichen 10.10.0.0/16 und könnten Sie 10.0.0.0/8 ankündigen.

### <a name="can-i-use-bgp-with-my-vnet-to-vnet-connections"></a>Kann ich mit meinem VNet-VNet-Verbindungen BGP verwenden?

Ja, können Sie BGP für Cross lokale Verbindungen und VNet-VNet-Verbindungen verwenden.

### <a name="can-i-mix-bgp-with-non-bgp-connections-for-my-azure-vpn-gateways"></a>Kann ich für meine Azure VPN Gateways BGP mit nicht BGP Verbindungen mischen?

Ja, können Sie sowohl die BGP nicht BGP Verbindungen für das gleiche Azure VPN Gateway kombinieren.

### <a name="does-azure-vpn-gateway-support-bgp-transit-routing"></a>Bedeutet Azure VPN Gateway Support BGP Übertragung routing?

Ja, BGP Übertragung routing, mit der Ausnahme unterstützt, dass Azure VPN Gateways wird **nicht** standardmäßige leitet an andere Kollegen BGP ankündigen. Klicken Sie zum Aktivieren der Übertragung routing über mehrere Azure VPN-Gateways müssen Sie auf alle zwischen-XT für VNet-VNet-Verbindungen BGP aktivieren.

### <a name="can-i-have-more-than-one-tunnels-between-azure-vpn-gateway-and-my-on-premises-network"></a>Kann ich mehr als ein Tunnel zwischen Azure VPN-Gateway und meinem lokalen Netzwerk verwenden?

Ja, können Sie mehrere S2S VPN-Tunnel zwischen einem Azure VPN Gateway und Ihrem lokalen Netzwerk herstellen. Bitte beachten Sie, dass alle diese Tunnel gegen die Gesamtzahl der Tunnel für Ihren Azure VPN Gateways gezählt werden sollen. Beispielsweise, wenn Sie zwei redundante Tunnel zwischen Schlüsselaufgaben Azure VPN und eine mit Ihrem lokalen Netzwerk verfügen, nutzen 2 Tunnel total Kontingent für Ihre Azure VPN Gateway (10 für Standard) und 30 für HighPerformance.

### <a name="can-i-have-multiple-tunnels-between-two-azure-vnets-with-bgp"></a>Kann ich mehrere Tunnel zwischen zwei Azure VNets mit BGP verwenden?

Nein, redundante Tunnel zwischen zwei virtuelle Netzwerke werden nicht unterstützt.

### <a name="can-i-use-bgp-for-s2s-vpn-in-an-expressroutes2s-vpn-co-existence-configuration"></a>Kann ich in einer ExpressRoute/S2S VPN gemeinsame Vorhandensein Konfiguration BGP für S2S VPN verwenden?

Nicht zu diesem Zeitpunkt.

### <a name="what-address-does-azure-vpn-gateway-use-for-bgp-peer-ip"></a>Welche Adresse werden Azure VPN-Gateway wird für BGP Peer-IP verwendet?

Azure VPN-Gateway wird eine einzelne IP-Adresse aus dem GatewaySubnet Bereich für das virtuelle Netzwerk definiert zugewiesen werden. Standardmäßig ist es die zweite letzte Adresse des Bereichs. Beispielsweise ist Ihre GatewaySubnet 10.12.255.0/27, werden von 10.12.255.0 bis hin zu 10.12.255.31, klicken Sie dann die BGP Peer-IP-Adresse auf dem Gateway Azure VPN 10.12.255.30. Sie können diese Informationen suchen, wenn Sie die Azure VPN Gateway Informationen auflisten.

### <a name="what-are-the-requirements-for-the-bgp-peer-ip-addresses-on-my-vpn-device"></a>Was sind die Anforderungen für die BGP Peer-IP-Adressen auf meinem Gerät VPN?

Ihre lokalen BGP peer Adresse **Darf nicht** gleich der öffentlichen IP-Adresse Ihres Geräts VPN sein. Verwenden Sie eine andere IP-Adresse auf dem Gerät VPN für Ihre IP-BGP-Peer. Es kann eine Loopbackschnittstelle auf dem Gerät zugewiesene Adresse sein. Geben Sie diese Adresse in das entsprechende lokale Netzwerkgateway, die die Position darstellt.

### <a name="what-should-i-specify-as-my-address-prefixes-for-the-local-network-gateway-when-i-use-bgp"></a>Was sollte ich angeben als meine Adresspräfixe für das Gateway für lokales Netzwerk bei der Verwendung von BGP?

Azure lokales Netzwerk-Gateway gibt die ursprüngliche Adresspräfixe für das lokale Netzwerk an. Mit BGP, müssen Sie das Präfix Host reservieren (/ 32 Präfix) Ihrer BGP Peer-IP-Adresse als die Adresse Speicherplatz für die lokale Netzwerk. Wenn Ihre BGP Peer-IP-Adresse 10.52.255.254 ist, sollten Sie "10.52.255.254/32" als die LocalNetworkAddressSpace des lokalen Netzwerkgateways, die diesem lokalen Netzwerk darstellt angeben. Dies ist, um sicherzustellen, dass das Gateway Azure VPN stellt die Sitzung BGP durch den Tunnel S2S VPN her.

### <a name="what-should-i-add-to-my-on-premises-vpn-device-for-the-bgp-peering-session"></a>Was sollte ich in meinem lokalen VPN-Gerät für die BGP Peeringliste Sitzung hinzufügen?

Sie sollten eine Host-Routing der Azure BGP Peer-IP-Adresse auf Ihrem VPN-Gerät der Tunnel IPsec S2S VPN auf Hinzufügen. Wenn die Azure VPN-Peer-IP-Adresse "10.12.255.30" ist, sollten Sie eine Host-Routing für "10.12.255.30" mit einer Nexthop Schnittstelle der übereinstimmende IPsec Tunnelschnittstelle auf Ihrem Gerät VPN hinzufügen.

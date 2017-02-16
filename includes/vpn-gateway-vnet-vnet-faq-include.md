- Die virtuelle Netzwerke können in der gleichen oder in verschiedenen Azure Regionen (Speicherorte) befinden.

- Cloud-Dienst oder einen Endpunkt für den Lastenausgleich kann nicht über virtuelle Netzwerke umfassen, auch wenn diese miteinander verbunden sind.

- Mehrere Azure virtuelle Netzwerke miteinander zu verbinden erforderlich keine lokalen VPN-Gateways, es sei denn, Cross lokale Konnektivität erforderlich ist.

- VNet-zu-VNet unterstützt verbindende von virtuelle Netzwerken. Nicht unterstützen Verbindungslinien virtuellen Computern oder cloud Services nicht in einem virtuellen Netzwerk.

- VNet-zu-VNet erfordert Azure VPN-Gateways mit RouteBased (vormals dynamisches Routing bezeichnet) VPN-Typen. 

- Virtuelle Netzwerkkonnektivität gleichzeitig mit mehreren Standorten VPN, mit bis zu 10 (Standard/Standard-Gateways) verwendet werden kann oder 30 (hohe Leistung Gateways) VPN für ein virtuelles Netzwerk VPN-Gateway Herstellen einer Verbindung mit entweder andere virtuelle Netzwerke Tunnel oder lokalen Websites.

- Die Adresse Leerzeichen der Sites virtuelle Netzwerke und lokale lokales Netzwerk dürfen nicht überlappen. Überlappende Adressbereiche bewirkt, dass die Erstellung von VNet-VNet-Verbindungen fehlschlägt.

- Redundante Tunnel zwischen zwei virtuelle Netzwerke werden nicht unterstützt.

- Alle VPN-Tunnel des virtuellen Netzwerks freigeben die verfügbare Bandbreite Azure VPN-Gateway und die gleichen VPN Gateway Verfügbarkeit Vereinbarung zum SERVICELEVEL in Azure.

- VNet-VNet-Datenverkehr bewegt sich über die Microsoft Network, nicht im Internet.

- VNet-VNet-Datenverkehr innerhalb der gleichen Region ist für beide Richtungen kostenlos; Cross Region VNet-VNet-Ausgang wird Datenverkehr mit der ausgehenden zwischen-VNet Daten durchstellen Sätzen basierend auf der Quelle Regionen belastet. Lizenzinformationen finden Sie die [Preise Seite](https://azure.microsoft.com/pricing/details/vpn-gateway/) Details.
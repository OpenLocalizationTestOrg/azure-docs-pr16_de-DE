|    | **VPN-Gateway Durchsatz (1)** | **Max IPsec VPN-Gateway Tunnel (2)** | **ExpressRoute Gateway Durchsatz** | **VPN-Gateway und ExpressRoute kombiniert werden.**|
|--- |----------------------------|-----------------------------------|-------------------------------------|-----------------------------------------|
| **Grundlegende SKU (3)(5)**              |  100 MB/s | 10                         |  500 MB/s                           | Nein   |
| **Standard-SKU (4)(5)**           |  100 MB/s | 10                         | 1000/s                           | Ja  |
| **Hohe Leistung SKU (4)**   | 200 MB  | 30                         | 2000/s                           | Ja  |

- (1) der Option VPN Durchsatz ist eine Schätzung basierend auf die Maße zwischen VNets in der gleichen Azure Region. Es ist keinen garantiert Durchsatz für Cross lokale Verbindungen über das Internet. Es ist die maximale mögliche Durchsatz Maße aus.
- (2) die Anzahl der Tunnel verweisen zu RouteBased VPN. Ein PolicyBased VPN unterstützt nur eine Standort-zu-Standort VPN-Tunnel.
- (3) BGP wird für das grundlegende SKU nicht unterstützt.
- (4) VPN Richtlinien werden für diese SKU nicht unterstützt. Sie sind für das grundlegende SKU nur unterstützt.
- (5) aktive S2S VPN-Gateway-Verbindungen werden für diese SKU nicht unterstützt. Aktive wird auf dem HighPerformance SKU nur unterstützt.
Es ist zu beachten, dass es zwei Methoden zum Konfigurieren einer Gruppe Verfügbarkeit Zuhörer Azure gibt. Diese Methoden unterscheiden sich in den Typ des Azure Lastenausgleich, die Sie beim Erstellen der Zuhörer verwenden. Die folgende Tabelle beschreibt die Unterschiede:

| Lastenausgleich | Implementierung | Verwenden Sie wann aus: |
| ------------- | -------------- | ----------- |
| **Externe** | Verwendet die **öffentliche virtuelle IP-Adresse** des Cloud-Dienst, der den virtuellen Computern hostet. | Sie müssen die Zuhörer von außerhalb des virtuellen Netzwerks, einschließlich aus dem Internet zugreifen. |
| **Interner** | Verwendet **Internen laden Lastenausgleich (ILB)** mit einer privaten Adresse für die Zuhörer. | Sie nur auf die Zuhörer aus innerhalb der gleichen virtuellen Netzwerk zugreifen. Dies umfasst Standort-zu-Standort VPN in Hybrid Szenarien aus. |

>[AZURE.IMPORTANT] Für eine Zuhörer wird mit der Cloud-Dienst öffentliche VIP (externer Lastenausgleich), solange die Client, Zuhörer und Datenbanken in der gleichen Azure Region werden nicht Ausgang anfallen. Andernfalls wird durch die Zuhörer zurückgegebenen Daten Ausgang betrachtet und belastet bei normaler Daten durchstellen Sätzen. 

ILB kann nur auf virtuelle Netzwerke mit regionalen Bereich konfiguriert werden. ILB können keine vorhandenen virtuelle Netzwerke, die für eine Gruppe für die Zugehörigkeit konfiguriert wurden. Weitere Informationen finden Sie unter [Internen Lastenausgleich](../articles/load-balancer/load-balancer-internal-overview.md).
